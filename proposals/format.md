---
ms.openlocfilehash: 1457c1eb018e12af30ce5b38be704bf8851d4b25
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "79485011"
---
# <a name="efficient-params-and-string-formatting"></a>有效率的參數和字串格式

## <a name="summary"></a>摘要
這項功能的組合將會提高格式化 `string` 值和傳遞 `params` 樣式引數的效率。

## <a name="motivation"></a>動機
`string` 值進行格式化的配置負擔可能會使許多以文字為基礎的應用程式的效能降低：從 `struct` 類型的裝箱損失、`params` 的 `object[]` 配置，以及在 `string` 呼叫期間的中繼 `string.Format` 分配。 為了維持效率，這類應用程式通常需要放棄生產力的功能，例如 `params` 和 `string` 插補，並移至非標準、手動編碼的解決方案。 

請考慮使用 MSBuild 作為範例。 這是使用非常重視效能的開發C#人員所撰寫的許多現代化功能。 但在一個代表性的組建範例中，MSBuild 會使用最少的詳細資訊來產生 `string` 配置的262MB。 這1/2 的配置是 `string.Format`內短期的配置。 這些功能會在 .NET Desktop 上移除大部分的內容，並在 .NET Core 上使其變得近乎零，因為 `Span<T>` 的可用性

這裡所述的一組語言功能，可讓應用程式繼續使用這些功能，幾乎不需要對其應用程式的程式碼基底進行任何變換，同時移除大部分案例中非預期的配置額外負荷。

## <a name="detailed-design"></a>詳細設計 
這裡會使用一組功能來達成這些結果：

- 擴充 `params` 以支援一組更廣泛的集合類型。
- 可讓開發人員自訂如何達到 `string` 插補。 
- 允許插入 `string` 系結至更有效率的 `string.Format` 多載。

### <a name="extending-params"></a>擴充 params
此語言可讓方法簽章中的 `params` 具有 `Span<T>`、`ReadOnlySpan<T>` 和 `IEnumerable<T>`的類型。 同樣適用于調用的規則會套用至 `params T[]`的這些新類型：

- 不能多載，其中唯一的差異是 `params` 關鍵字。
- 可以藉由傳遞一系列可隱含轉換成 `T` 或單一 `Span<T>` / 
`ReadOnlySpan<T>` / `IEnumerable<T>` 引數的引數來叫用。
- 必須是方法簽章中的最後一個參數。
- 依此類推 。 

為了簡單起見，`Span<T>` 和 `ReadOnlySpan<T>` 變體在下方會稱為 `Span<T>`。 在 `ReadOnlySpan<T>` 的行為不同的情況下，會明確地將其稱為。 

`params` 提供的 `Span<T>` 變體的優點是，它可讓編譯器在配置 `Span<T>` 值的備份儲存體時有很大的彈性。 使用 `params T[]` 編譯器必須為每個 `params` 方法的調用配置新的 `T[]`。 不可能重複使用，因為它必須假設被呼叫者已儲存並重複使用參數。 這可能會導致具有大量 `params` 調用的方法有很大的效率。

指定的 `Span<T>` 變異 `ref struct` 被呼叫端無法儲存引數。 因此，編譯器可以採取像是重新使用引數的動作，將呼叫網站優化。 相較于 `T[]`，這可讓重複調用的效率非常有效率。 不過，此語言不會對這類 callsite 的優化提供任何特定的保證。 請注意，在叫用 `params Span<T>` 方法時，編譯器可以免費使用 `T[]` 以外的值。 

以下是其中一種可能的執行方式。 請考慮方法主體中的所有 `params` 調用。 編譯器可以配置大小等於最大 `params` 調用的陣列，並藉由在陣列上建立適當大小的 `Span<T>` 實例，將其用於所有調用。 例如：

``` csharp
static class OneAllocation {
    static void Use(params Span<string> spans) {
        ...
    }

    static void Go() {
        Use("jaredpar");
        Use("hello", "world");
        Use("a", "longer", "set");
    }
}
```

編譯器可以選擇發出 `Go` 的主體，如下所示：

``` csharp
    static void Go() {
        var args = new string[3];
        args[0] = "jaredpar";
        Use(new Span<string>(args, start: 0, length: 1));

        args[0] = "hello";
        args[1] = "world";
        Use(new Span<string>(args, start: 0, length: 2));

        args[0] = "a";
        args[1] = "longer";
        args[2] = "set";
        Use(new Span<string>(args, start: 0, length: 3));
   }
```

這可以大幅減少在應用程式中配置的陣列數目。 如果執行時間為數組的更聰明堆疊配置提供公用程式，則配置可能更進一步降低。

不過，這種優化不一定會套用。 雖然被呼叫端無法捕捉 `params` 引數，但當有 `ref` 或 `out / ref` 參數本身是 `ref struct` 類型時，仍然可以在呼叫端中加以捕捉。 

``` csharp
static class SneakyCapture {
    static ref int M(params Span<T> span) => ref span[0];

    static void Oops() {
        // This now holds onto the memory backing the Span<T> 
        ref int r = ref M(42);
    }
}
```

不過，這些情況可透過靜態方式偵測。 當有 `ref` 傳回或 `out` 或 `ref`傳遞 `ref struct` 參數時，就可能發生這種情況。 在這種情況下，編譯器必須為每個調用配置一個全新的 `T[]`。 

本檔結尾會討論數個其他可能的優化策略。

`IEnumerable<T>` 變體只是便利的多載。 這在經常使用 `IEnumerable<T>`，但也有許多 `params` 使用量的案例中很有用。 在 `T` 引數中叫用時，將會將支援儲存體配置為 `T[]`，就像現在 `params T[]` 完成一樣。

### <a name="params-overload-resolution-changes"></a>params 多載解析變更
此提案表示語言現在有四種 `params` 變體，在其中有一個。 方法可以定義只有 `params` 宣告的類型不同之方法的多載。 

請考慮 `StringBuilder.AppendFormat` 除了 `params object[]`以外，一定會加入 `params ReadOnlySpan<object>` 多載。 這讓 it 能夠藉由減少收集配置，而不需要對呼叫程式碼進行任何變更，大幅提升效能。 

為了協助這種方式，此語言會引進下列多載解析系結規則。 當候選方法只有 `params` 參數的差異時，就會依照下列順序來偏好候選人：

1. `ReadOnlySpan<T>`
1. `Span<T>`
1. `T[]`
1. `IEnumerable<T>`

這是一般案例最不有效率的順序。

### <a name="variant"></a>變數
CoreFX 會建立名為[Variant](https://github.com/dotnet/corefxlab/pull/2595)的新 managed 型別原型。 這種類型的目的是要用於預期異類值，但不想使用 `object` 做為參數所帶來的額外負荷的 Api。 `Variant` 類型會提供通用儲存體，但可避免最常使用之類型的裝箱配置。 在 `string.Format` 之類的 Api 中使用此類型，可以消除大部分案例中的「裝箱」額外負荷。

這種類型本身不一定是語言特有的。 這份檔會另外引進，因為它會成為提案其他部分的執行詳細資料。 

### <a name="efficient-interpolated-strings"></a>有效率的字串插值
內插字串是中C#的熱門但沒有效率的功能。 最常見的語法（使用插入 `string` 做為 `string`）會轉譯成 `string.Format(string, params object[])` 呼叫。 這將會產生所有實值型別的「設定」（中繼 `string` 配置）配置，而當引數數目超過 `string.Format`的「快速」多載的參數數量時，此實作為執行時，主要會使用 `object.ToString` 進行格式化以及陣列配置。 

語言會變更其插補，以考慮 `string.Format`的替代多載。 它會考慮所有形式的 `string.Format(string, params)`，並挑選符合引數類型的「最佳」多載。
「最佳」 `params` 多載將由上面討論的規則決定。 這表示插補 `string` 現在可以系結至非常有效率的多載，例如 `string.Format(string format, params ReadOnlySpan<Variant> args)`。 在許多情況下，這會移除所有中繼配置。

### <a name="customizable-interpolated-strings"></a>可自訂的內插字串
開發人員可以使用 `FormattableString`自訂內插字串的行為。 這包含進入字串插值的資料：格式 `string` 和引數當做陣列。 不過，這仍然具有「裝箱」和「引數」陣列配置，以及 `FormattableString` 的配置（這是 `abstract class`）。 因此，對 `string` 格式設定繁重的應用程式很少使用。

為了有效率地插入字串格式，語言將會辨識新的類型： `System.ValueFormattableString`。 所有字串插值都會將目標型別轉換成這個型別。 這會藉由將插入字串轉譯為呼叫中的方式來執行，`ValueFormattableString.Create` 完全如同今天 `FormattableString.Create` 所做的一樣。 當您尋找最適合的 `ValueFormattableString.Create` 方法時，語言將支援本檔中所述的所有 `params` 選項。 

``` csharp
readonly struct ValueFormattableString {
    public static ValueFormattableString Create(Variant v) { ... } 
    public static ValueFormattableString Create(string s) { ... } 
    public static ValueFormattableString Create(string s, params ReadOnlySpan<Variant> collection) { ... } 
}

class ConsoleEx { 
    static void Write(ValueFormattableString f) { ... }
}

class Program { 
    static void Main() { 
        ConsoleEx.Write(42);
        ConsoleEx.Write($"hello {DateTime.UtcNow}");

        // Translates into 
        ConsoleEx.Write(ValueFormattableString.Create((Variant)42));
        ConsoleEx.Write(ValueFormattableString.Create(
            "hello {0}", 
            new Variant(DateTime.UtcNow));
    }
}
```

當引數為字串插值時，多載解析規則將會變更為偏好 `string` 的 `ValueFormattableString`。 這表示只有在 `string` 和 `ValueFormattableString`上有不同的多載才有價值。 現今具有 `FormattableString` 的這類多載並不重要，因為編譯器一律會偏好 `string` 版本（除非開發人員使用明確轉換）。 

## <a name="open-issues"></a>開啟問題

### <a name="valueformattablestring-breaking-change"></a>ValueFormattableString 重大變更
在透過 `string` 進行多載解析時，偏好 `ValueFormattableString` 的變更是一種重大變更。 開發人員可以立即定義名為 `ValueFormattableString` 的類型，並在具有 `string`的方法多載中使用它。 這項建議的變更會導致編譯器在執行這組功能之後，挑選不同的多載。 

這種情況似乎相當低。 此類型需要完整名稱 `System.ValueFormattableString`，而且必須具有名為 `Create`的 `static` 方法。 假設開發人員強烈不建議在 `System` 命名空間中定義任何類型，這種中斷就好像是合理的危害。

### <a name="expanding-to-more-types"></a>擴充至更多類型
假設我們在此區域中，我們應該考慮將 `IList<T>`、`ICollection<T>` 和 `IReadOnlyList<T>` 新增至支援 `params` 的一組集合。 就實行的角度而言，這裡的其他工作將會花費較小的成本。

不過，LDM 必須判斷這種語言的複雜情況是否值得。 新增 `IEnumerable<T>` 會移除非常特定的摩擦點。 若缺少此 `params` 解決方案，許多客戶會在呼叫 `params` 方法時，強制從 `IEnumerable<T>` 配置 `T[]`。 新增 `IEnumerable<T>` 會修正此問題。 這裡沒有其他介面修正的特定摩擦點。 

## <a name="considerations"></a>考量

### <a name="variant2-and-variant3"></a>Variant2 和 Variant3
CoreFX 小組也有一組非配置的儲存類型，最多可有三個 `Variant` 引數。 這些是單一 `Variant`，`Variant2` 和 `Variant3`。 所有的方法都有一組可用來取得配置的免費 `Span<Variant>`： `CreateSpan` 和 `KeepAlive`。 這表示對於最多三個引數的 `params Span<Variant>`，呼叫位置可以完全免費的配置。

``` csharp
static class ZeroAllocation {
    static void Use(params Span<Variant> spans) {
        ...
    }

    static void Go() {
        Use("hello", "world");
    }
}
```

`Go` 方法可以降低為下列各項：

``` csharp
static class ZeroAllocation {
    static void Go() {
        Variant2 _v;
        _v.Variant1 = new Variant("hello");
        _v.Variant2 = new Variant("word");
        Use(_v.CreateSpan());
        _v.KeepAlive();
    }
}
```

這只需要在提案上執行非常少的工作，就能在 `params Span<T>` 呼叫之間重複使用 `T[]`。 編譯器已經需要管理每個呼叫的暫存，並在之後執行清理工作（即使在一種情況下，它只會將內部 temp 標示為可用）。 

注意：只有在桌面上才需要 `KeepAlive` 函數。 在 .NET Core 上，方法將無法使用，因此編譯器不會發出呼叫。

### <a name="clr-stack-allocation-helpers"></a>CLR 堆疊配置協助程式
CLR 只會提供連續記憶體之堆疊配置的[localloc](https://docs.microsoft.com/en-us/dotnet/api/system.reflection.emit.opcodes.localloc?redirectedfrom=MSDN&view=netframework-4.7.2) 。 此指示的限制在於僅適用于 `unmanaged` 類型。 這表示它無法做為可有效率地為 `params 
 Span<T>`配置支援儲存體的通用解決方案。 

不過，這項限制並不是部分基本限制，而是更多的歷程記錄。 CLR 可以加入宣告新的 op 代碼/內建函式，以提供通用堆疊配置。 然後，這些可用來配置最 `params Span<T>` 呼叫的支援儲存體。

``` csharp
static class BetterAllocation {
    static void Use(params Span<string> spans) {
        ...
    }

    static void Go() {
        Use("hello", "world");
    }
}
```

`Go` 方法可以降低為下列各項：

``` csharp
static class ZeroAllocation {
    static void Go() {
        Span<T> span = RuntimeIntrinsic.StackAlloc<string>(length: 2);
        span[0] = "hello";
        span[1] = "world";
        Use(span);
    }
}
```

雖然這種方法是非常堆積的，但它會造成額外的堆疊使用。 在具有深度堆疊和許多 `params` 使用方式的演算法中，這可能會導致 `StackOverflowException` 產生簡單 `T[]` 配置成功的位置。 

可惜C#的是，不會針對方法間分析的類型進行設定，它可以讓您瞭解呼叫是否應該使用 `params`的堆疊或堆積配置。 它只能考慮自己的每個呼叫。

CLR 是在執行時間進行這類判斷的最佳設定。 因此，我們可能會讓執行時間為通用堆疊配置提供兩種方法：

1. `Span<T> StackAlloc<T>(int length)`：這具有 `localloc` 的行為和限制，但它可以在任何類型 `T`上使用。 
1. `Span<T> MaybeStackAlloc<T>(int length)`：此執行時間可以選擇執行堆疊或堆積配置來執行此作業。 然後，執行時間就可以使用它所呼叫的執行內容來決定如何配置 `Span<T>`。 不過，呼叫端一律會將它視為已配置堆疊。

對於非常簡單的案例，例如一到兩個自C#變數，編譯器一律會使用 `StackAlloc<T>` variant。 在大部分情況下，這不太可能會對堆疊耗盡造成明顯的影響。 針對其他情況，編譯器可以選擇改用 `MaybeStackAlloc<T>`，並讓執行時間進行呼叫。

我們的選擇可能需要更深入的調查和檢查真實世界的應用程式。 但是，如果有這些新的內建函式，則會提供這種類型的彈性。

### <a name="why-not-varargs"></a>為什麼不是 varargs？ 
在此將現有的[varargs](https://docs.microsoft.com/en-us/cpp/windows/variable-argument-lists-dot-dot-dot-cpp-cli?view=vs-2017)功能視為可行的解決方案。 不過，這項功能主要是C++針對/cli 案例，並在其他案例中具有已知的漏洞。 此外，將此項移植到 Unix 也會有相當大的成本。 因此，它不會被視為可行的解決方案。

## <a name="related-issues"></a>相關問題
此規格與下列問題相關： 

- https://github.com/dotnet/csharplang/issues/1757
- https://github.com/dotnet/csharplang/issues/179
- https://github.com/dotnet/corefxlab/pull/2595

