---
ms.openlocfilehash: 76065293f652979ab395e131d657e44899c5a65b
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484549"
---
# <a name="simplified-null-argument-checking"></a>簡化的 Null 引數檢查

## <a name="summary"></a>摘要
此提議提供簡化的語法，用於驗證方法引數不 `null`，且會適當地擲回 `ArgumentNullException`。

## <a name="motivation"></a>動機
設計可為 null 的參考型別的工作導致我們檢查 `null` 引數驗證所需的程式碼。 假設 NRT 不會影響程式碼執行，開發人員仍然必須加入 `if (arg is null) throw` 定案盤子程式碼，即使在完全 `null` 乾淨的專案中也一樣。 這讓我們希望能夠探索引數的最小語法，`null` 以語言進行驗證。 

雖然此 `null` 參數驗證語法預期會與 NRT 一併使用，但提議與它完全無關。 語法可以獨立于 `#nullable` 指示詞之外使用。

## <a name="detailed-design"></a>詳細設計 

### <a name="null-validation-parameter-syntax"></a>Null 驗證參數語法
驚嘆號運算子 `!`可以放在參數清單中的參數名稱之後，這會導致C#編譯器發出標準 `null` 檢查該參數的程式碼。 這稱為 `null` 驗證參數語法。 例如：

``` csharp
void M(string name!) {
    ...
}
```

將轉譯為：

``` csharp
void M(string name) {
    if (name is null) {
        throw new ArgumentNullException(nameof(name));
    }
    ...
}
```

產生的 `null` 檢查會在任何開發人員撰寫方法中的程式碼之前進行。 當多個參數包含 `!` 運算子時，會依照宣告參數的順序來進行檢查。

``` csharp
void M(string p1, string p2) {
    if (p1 is null) {
        throw new ArgumentNullException(nameof(p1));
    }
    if (p2 is null) {
        throw new ArgumentNullException(nameof(p2));
    }
    ...
}
```

檢查會特別提供 `null`的參考相等，而不會叫用 `==` 或任何使用者定義的運算子。 這也表示，`!` 運算子只能加入至可針對 `null`測試其類型是否相等的參數。 這表示它無法用於其類型已知為實數值型別的參數。

``` csharp
// Error: Cannot use ! on parameters who types derive from System.ValueType
void G<T>(T arg!) where T : struct {

}
```

在使用函式的情況下，`null` 驗證會在任何其他程式碼于此函式中執行。 其中包括： 

- 連結至具有 `this` 或 `base` 的其他處理函式 
- 在此函式中隱含出現的欄位初始化運算式

例如：

``` csharp
class C {
    string field = GetString();
    C(string name!): this(name) {
        ...
    }
}
```

大致會轉譯成下列內容：

``` csharp
class C {
    C(string name)
        if (name is null) {
            throw new ArgumentNullException(nameof(name));
        }
        field = GetString();
        :this(name);
        ...
}
```

注意：這不是合法C#的程式碼，而只是實作為執行功能的近似值。 

`null` 驗證參數語法也會在 lambda 參數清單上有效。 即使在缺少括弧的單一參數語法中，這也是有效的。

``` csharp
void G() {
    // An identity lambda which throws on a null input
    Func<string, string> s = x! => x;
}
```

語法在反覆運算器方法的參數上也是有效的。 不同于 iterator 中的其他程式碼，當叫用 iterator 方法時，而不是在進行基礎枚舉器時，就會發生 `null` 驗證。 這適用于傳統或 `async` 的反覆運算器。

``` csharp
class Iterators {
    IEnumerable<char> GetCharacters(string s!) {
        foreach (var c in s) {
            yield return c;
        }
    }

    void Use() {
        // The invocation of GetCharacters will throw
        IEnumerable<char> e = GetCharacters(null);
    }
}
```

`!` 運算子只能用於具有相關聯方法主體的參數清單。 這表示它不能用在 `abstract` 方法、`interface`、`delegate` 或 `partial` 方法定義中。

### <a name="extending-is-null"></a>擴充為 null
運算式 `is null` 有效的類型將會擴充以包含不受限制的類型參數。 這可讓它在 `null` 檢查有效的所有類型上，填滿檢查 `null` 的意圖。 具體而言，這是不一定是實數值型別的類型。 例如，限制為 `struct` 的型別參數不能與這個語法搭配使用。

``` csharp
void NullCheck<T1, T2>(T1 p1, T2 p2) where T2 : struct {
    // Okay: T1 could be a class or struct here.
    if (p1 is null) {
        ...
    }

    // Error 
    if (p2 is null) { 
        ...
    }
}
```

在類型參數上 `is null` 的行為，會與今天 `== null` 相同。 在類型參數具現化為實數值型別的情況下，程式碼將會評估為 `false`。 如果是參考型別，則程式碼會執行適當的 `is null` 檢查。

### <a name="intersection-with-nullable-reference-types"></a>具有可為 Null 之參考型別的交集
具有套用至其名稱之 `!` 運算子的任何參數，會以無法 `null`的可為 null 狀態開始。 即使參數本身的類型可能 `null`，也是如此。 這可能會發生于明確可為 null 的型別，例如 `string?`或具有不受限制的型別參數。 

當參數上的 `!` 語法與參數上明確的可為 null 類型結合時，編譯器會發出警告：

``` csharp
void WarnCase<T>(
    string? name!, // Warning: combining explicit null checking with a nullable type
    T value1 // Okay
)
```

## <a name="open-issues"></a>開啟問題
None

## <a name="considerations"></a>考量

### <a name="constructors"></a>建構函式
函式的程式碼產生，表示從標準 `null` 驗證，以及 `null` 驗證參數語法（`!`）移動時，有一個小但可觀察的行為變更。 標準驗證中的 `null` 檢查會在欄位初始化運算式和任何 `base` 或 `this` 呼叫之後發生。 這表示開發人員不一定要將其 `null` 驗證的100% 遷移至新的語法。 至少需要進行一些檢查。

在討論之後，我們決定這不太可能會造成任何重大的採用問題。 `null` 檢查在此函式中的任何邏輯執行之前，會有更多的邏輯。 如果發現重大的相容性問題，可以重新流覽。

### <a name="warning-when-mixing--and-"></a>混合時出現警告？ 和!
當 `!` 語法套用至明確輸入為可為 null 的型別的參數時，是否應發出警告，是一段冗長的討論。 就表面而言，開發人員似乎會有無意義的宣告，但在某些情況下，型別階層可能會強制開發人員進行這類情況。 

請考慮一系列元件的下列類別階層（假設全部都是以已啟用 `null` 檢查）來編譯：

``` csharp
// Assembly1
abstract class C1 {
    protected abstract void M(object o); 
}

// Assembly2
abstract class C2 : C1 {

}

// Assembly3
abstract class C3 : C2 { 
    protected override void M(object o!) {
        ...
    }
}
```

在這裡，`C3` 的作者決定要將 `null` 驗證新增至 `o`參數。 這完全符合功能的使用方式。

現在，假設 Assembly2 的作者決定新增下列覆寫：

``` csharp
// Assembly2
abstract class C2 : C1 {
   protected override void M(object? o) { 
       ...
   }
}
```

這是可為 null 的參考型別所允許的，因為它是合理的，讓合約更有彈性地輸入位置。 一般的 NRT 功能允許在參數/傳回 null 屬性上有合理的共同/反變數。 不過，此語言會根據最特定的覆寫來進行共同/反變數檢查，而不是原始的宣告。 這表示 Assembly3 的作者會收到有關 `o` 不相符之類型的警告，而且需要將簽章變更為下列內容，才能將其排除： 

``` csharp
// Assembly3
abstract class C3 : C2 { 
    protected override void M(object? o!) {
        ...
    }
}
```

到目前為止，Assembly3 的作者有幾個選擇：

- 他們可以接受/隱藏關於 `object?` 和 `object` 不相符的警告。
- 他們可以接受/隱藏關於 `object?` 和 `!` 不相符的警告。
- 他們可以直接移除 `null` 驗證檢查（delete `!` 並執行明確檢查）

這是真正的案例，但現在的概念是要繼續進行警告。 如果出現警告的頻率比我們預期的還要頻繁，我們可以稍後再將它移除（相反的結果不是 true）。

### <a name="implicit-property-setter-arguments"></a>隱含屬性 setter 引數
參數的 `value` 引數是隱含的，而且不會出現在任何參數清單中。 這表示它不能是這項功能的目標。 屬性 setter 語法可以擴充以包含參數清單，以允許套用 `!` 運算子。 但是，這項功能的概念會使 `null` 驗證變得更簡單。 如此一來，隱含 `value` 引數就不會使用這項功能。

## <a name="future-considerations"></a>未來的考量
None
