---
ms.openlocfilehash: 8bf3a18dc42e225e64bd3ccda2106aed89b421ed
ms.sourcegitcommit: 9aa177443b83116fe1be2ab28e2c7291947fe32d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/21/2020
ms.locfileid: "80108385"
---
# <a name="function-pointers"></a>函式指標

## <a name="summary"></a>摘要

本提案提供的語言結構會公開目前無法有效率地存取的 IL opcode，或目前的C# `ldftn` 和 `calli`。 這些 IL opcode 在高效能程式碼中很重要，而開發人員需要有效率的方式來存取它們。

## <a name="motivation"></a>動機

下列問題會說明這項功能的動機和背景（這是功能的可能執行）：

https://github.com/dotnet/csharplang/issues/191

這是[編譯器內建函式](https://github.com/dotnet/csharplang/blob/master/proposals/intrinsics.md)的替代設計提案

## <a name="detailed-design"></a>詳細設計

### <a name="function-pointers"></a>函式指標

語言可讓您使用 `delegate*` 語法來宣告函式指標。 下一節將詳細說明完整的語法，但其目的是要類似 `Func` 和 `Action` 型別宣告所使用的語法。

``` csharp
unsafe class Example {
    void Example(Action<int> a, delegate*<int, void> f) {
        a(42);
        f(42);
    }
}
```

這些類型是使用如 ECMA-335 中所述的函式指標類型來表示。 這表示 `delegate*` 的調用會使用 `calli`，其中 `delegate` 的調用會在 `Invoke` 方法上使用 `callvirt`。
在語法上，這兩個結構的調用都相同。

方法指標的 ECMA-335 定義包含呼叫慣例做為類型簽章的一部分（第7.1 節）。
預設的呼叫慣例將會 `managed`。 在 `delegate*` 語法之後加入適當的修飾詞，即可指定替代形式： `managed`、`cdecl`、`stdcall`、`thiscall`或 `unmanaged`。 範例：

``` csharp
// This method will be invoked using the cdecl calling convention
delegate* cdecl<int, int>;

// This method will be invoked using the stdcall calling convention
delegate* stdcall<int, int>;
```

`delegate*` 類型之間的轉換是根據其簽章（包括呼叫慣例）來完成。

``` csharp
unsafe class Example {
    void Conversions() {
        delegate*<int, int, int> p1 = ...;
        delegate* managed<int, int, int> p2 = ...;
        delegate* cdecl<int, int, int> p3 = ...;

        p1 = p2; // okay p1 and p2 have compatible signatures
        Console.WriteLine(p2 == p1); // True
        p2 = p3; // error: calling conventions are incompatible
    }
}
```

`delegate*` 類型是指標類型，這表示它具有標準指標類型的所有功能和限制：

- 僅在 `unsafe` 內容中有效。
- 只能從 `unsafe` 內容呼叫包含 `delegate*` 參數或傳回類型的方法。
- 無法轉換成 `object`。
- 不能當做泛型引數使用。
- 可以將 `delegate*` 隱含地轉換成 `void*`。
- 可以從 `void*` 明確轉換成 `delegate*`。

限制：

- 自訂屬性不能套用至 `delegate*` 或其任何元素。
- `delegate*` 參數不能標記為 `params`
- `delegate*` 類型具有一般指標類型的所有限制。

### <a name="function-pointer-syntax"></a>函式指標語法

完整的函式指標語法是以下列文法表示：

```antlr
pointer_type
    : ...
    | funcptr_type
    ;

funcptr_type
    : 'delegate' '*' calling_convention? '<' (funcptr_parameter_modifier? type ',')* funcptr_return_modifier? return_type '>'
    ;

calling_convention
    : 'cdecl'
    | 'managed'
    | 'stdcall'
    | 'thiscall'
    | 'unmanaged'
    ;

funcptr_parameter_modifier
    : 'ref'
    | 'out'
    | 'in'
    ;

funcptr_return_modifier
    : 'ref'
    | 'ref readonly'
    ;
```

`unmanaged` 呼叫慣例代表目前平臺上機器碼的預設呼叫慣例，而且會編碼為 winapi。
所有 `calling_convention`在前面加上 `delegate*`時都是內容關鍵字。

``` csharp
delegate int Func1(string s);
delegate Func1 Func2(Func1 f);

// Function pointer equivalent without calling convention
delegate*<string, int>;
delegate*<delegate*<string, int>, delegate*<string, int>>;

// Function pointer equivalent with calling convention
delegate* managed<string, int>;
delegate*<delegate* managed<string, int>, delegate*<string, int>>;
```

### <a name="function-pointer-conversions"></a>函式指標轉換

在不安全的內容中，可用的隱含轉換集合（隱含轉換）會擴充以包含下列隱含指標轉換：
- [_現有的轉換_](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#pointer-conversions)
- 從_funcptr\_輸入_`F0` 至另一個_funcptr\_類型_`F1`，但前提是下列所有條件皆成立：
    - `F0` 和 `F1` 的參數數目相同，而且 `F0` 中 `D0n` 的每個參數都有與 `ref`中對應參數 `out`相同的 `in`、`D1n` 或 `F1`修飾詞。
    - 針對每個值參數（不含 `ref`、`out`或 `in` 修飾詞的參數），會從 `F0` 中的參數類型，將識別轉換、隱含參考轉換或隱含指標轉換存在 `F1`中的對應參數類型。
    - 針對每個 `ref`、`out`或 `in` 參數，`F0` 中的參數類型與 `F1`中對應的參數類型相同。
    - 如果傳回類型是以傳值方式傳回（不 `ref` 或 `ref readonly`），則會從 `F1` 的傳回類型，將識別、隱含參考或隱含指標轉換存在於 `F0`的傳回類型。
    - 如果傳回型別是以傳址方式（`ref` 或 `ref readonly`），則 `F1` 的傳回型別和 `ref` 修飾詞會與 `ref` 的傳回型別和 `F0`修飾詞相同。
    - `F0` 的呼叫慣例與 `F1`的呼叫慣例相同。

### <a name="allow-address-of-to-target-methods"></a>允許目標方法的位址

方法群組現在將允許當做位址運算式的引數。 這類運算式的類型會是具有目標方法的對等簽章和 managed 呼叫慣例的 `delegate*`：

``` csharp
unsafe class Util {
    public static void Log() { }

    void Use() {
        delegate*<void> ptr1 = &Util.Log;

        // Error: type "delegate*<void>" not compatible with "delegate*<int>";
        delegate*<int> ptr2 = &Util.Log;

        // Okay. Conversion to void* is always allowed.
        void* v = &Util.Log;
   }
}
```

在不安全的內容中，如果下列所有條件都成立，則方法 `M` 與函式指標型別相容 `F`：
- `M` 和 `F` 的參數數目相同，而且 `D` 中的每個參數都有相同的 `ref`、`out`或 `in` 修飾詞，做為 `F`中的對應參數。
- 針對每個值參數（不含 `ref`、`out`或 `in` 修飾詞的參數），會從 `M` 中的參數類型，將識別轉換、隱含參考轉換或隱含指標轉換存在 `F`中的對應參數類型。
- 針對每個 `ref`、`out`或 `in` 參數，`M` 中的參數類型與 `F`中對應的參數類型相同。
- 如果傳回類型是以傳值方式傳回（不 `ref` 或 `ref readonly`），則會從 `F` 的傳回類型，將識別、隱含參考或隱含指標轉換存在於 `M`的傳回類型。
- 如果傳回型別是以傳址方式（`ref` 或 `ref readonly`），則 `F` 的傳回型別和 `ref` 修飾詞會與 `ref` 的傳回型別和 `M`修飾詞相同。
- `M` 的呼叫慣例與 `F`的呼叫慣例相同。
- `M` 是靜態方法。

在不安全的內容中，從目標是方法群組 `E` 到相容函式指標型別的隱含轉換存在 `F` 如果 `E` 包含至少一個適用于其一般格式的方法，並使用 `F`的參數類型和修飾詞所建立的引數清單，如下所述。
- 選取的單一方法 `M` 對應至表單 `E(A)` 的方法調用，並具有下列修改：
   - 引數清單 `A` 是運算式的清單，每一個都會分類為一個變數，並使用對應的_正式\_參數_的類型和修飾詞（`ref`、`out`或 `in`）\_`D`清單。
   - 候選方法只是適用于其一般格式的方法，而不是適用于其擴充形式的方法。
   - 候選方法只是靜態的方法。
- 如果方法調用的演算法產生錯誤，則會發生編譯時期錯誤。 否則，此演算法會產生一個最佳方法，`M` 具有與 `F` 相同數目的參數，並將轉換視為存在。
- 選取的方法 `M` 必須與函式指標類型 `F`相容（如上面所定義）。 否則，會發生編譯時期錯誤。
- 轉換的結果是 `F`類型的函式指標。

隱含的轉換存在於其目標為方法群組的運算式位址，如果 `E`中只有一個靜態方法 `M`，則會 `E` `void*`。
如果有一個靜態方法，則會 `M``E` 的單一最佳方法。
否則，會發生編譯時期錯誤。

這表示開發人員可以相依于多載解析規則，以搭配使用位址運算子：

``` csharp
unsafe class Util {
    public static void Log() { }
    public static void Log(string p1) { }
    public static void Log(int i) { };

    void Use() {
        delegate*<void> a1 = &Log; // Log()
        delegate*<int, void> a2 = &Log; // Log(int i)

        // Error: ambiguous conversion from method group Log to "void*"
        void* v = &Log;
    }
```

將使用 `ldftn` 指令來實作為位址的運算子。

這項功能的限制：

- 僅適用于標示為 `static`的方法。
- 非`static` 的區域函式不能用在 `&`中。 語言不會指定這些方法的執行詳細資料。 這包括它們是靜態的還是實例，或是它們所發出的簽章。

### <a name="better-function-member"></a>更好的函式成員

較佳的函式成員規格將會變更，以包含下列程式程式碼：

> `delegate*` 比 `void*` 更具體

這表示可以在 `void*` 和 `delegate*` 上多載，但仍 sensibly 使用 address 運算子。

## <a name="open-issues"></a>開啟問題

### <a name="nativecallableattribute"></a>NativeCallableAttribute

這是 CLR 在叫用時，用來避免 managed 至原生序言的屬性。 這個屬性所標記的方法只能從機器碼呼叫，而不是 managed （無法呼叫方法、建立委派等 ...）。 屬性對 mscorlib 而言並非特別的;執行時間會將具有相同名稱的任何屬性視為具有相同的語法。

執行時間和語言可以搭配使用，以完全支援這種方式。 語言可以選擇將具有 `NativeCallable` 屬性的 `static` 成員視為具有指定呼叫慣例的 `delegate*`。

``` csharp
unsafe class NativeCallableExample {
    [NativeCallable(CallingConvention.CDecl)]
    static void CloseHandle(IntPtr p) => Marshal.FreeHGlobal(p);

    void Use() {
        delegate*<IntPtr, void> p1 = &CloseHandle; // Error: Invalid calling convention

        delegate* cdecl<IntPtr, void> p2 = &CloseHandle; // Okay
    }
}

```

此外，語言可能也會想要：

- 將以 `NativeCallable` 標記之方法的任何 managed 呼叫，標示為錯誤。 假設無法從 managed 程式碼叫用函式，編譯器應該會防止開發人員嘗試這樣的調用。
- 當方法標記為 `NativeCallable`時，防止方法群組轉換為 `delegate`。

不過，這不是支援 `NativeCallable` 的必要動作。 編譯器可以使用現有的語法來支援 `NativeCallable` 屬性。 程式只需要在轉換成正確的 `delegate*` 簽章之前，轉型為 `void*`。 這不會比今天的支援更糟。

``` csharp
void* v = &CloseHandle;
delegate* cdecl<IntPtr, bool> f1 = (delegate* cdecl<IntPtr, bool>)v;
```

### <a name="extensible-set-of-unmanaged-calling-conventions"></a>一組可擴充的非受控呼叫慣例

目前 ECMA-335 編碼所支援的一組非受控呼叫慣例已過期。 我們已看到新增支援更多非受控呼叫慣例的要求，例如：

- [vectorcall](https://docs.microsoft.com/cpp/cpp/vectorcall) https://github.com/dotnet/coreclr/issues/12120
- 明確 StdCall 此 https://github.com/dotnet/coreclr/pull/23974#issuecomment-482991750

這項功能的設計應允許在未來視需要擴充非受控呼叫慣例的集合。 這些問題包含用於編碼呼叫慣例的有限空間（`IMAGE_CEE_CS_CALLCONV_MASK`中的12個值中所採用），以及需要觸及以加入新呼叫慣例的位置數目。 可能的解決方法是使用[`System.Runtime.InteropServices.CallingConvention`](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.callingconvention)列舉來引進代表呼叫慣例的新編碼方式。

如需參考， https://github.com/llvm/llvm-project/blob/master/llvm/include/llvm/IR/CallingConv.h 具有 LLVM 支援的呼叫慣例清單。 雖然 .NET 不太可能需要支援所有的專案，但它會示範呼叫慣例的空間非常豐富。

## <a name="considerations"></a>考量

### <a name="allow-instance-methods"></a>允許實例方法

您可以利用 `EXPLICITTHIS` CLI 呼叫慣例（在程式碼中C#名為 `instance`），擴充提案以支援實例方法。 這種形式的 CLI 函式指標會將 `this` 參數放成函數指標語法的明確第一個參數。

``` csharp
unsafe class Instance {
    void Use() {
        delegate* instance<Instance, string> f = &ToString;
        f(this);
    }
}
```

這聽起來很好，但會對提案加上一些複雜的部分。 特別是因為呼叫慣例不同的函式指標 `instance` 和 `managed` 會是不相容的，即使這兩個案例都是用來叫C#用具有相同簽章的 managed 方法也一樣。 此外，在每個案例中，如果有一個簡單的解決方法，這會是很有説明的：使用 `static` 區域函數。

``` csharp
unsafe class Instance {
    void Use() {
        static string toString(Instance i) = i.ToString();
        delgate*<Instance, string> f = &toString;
        f(this);
    }
}
```

### <a name="dont-require-unsafe-at-declaration"></a>宣告時不需要 unsafe

不需要在每次使用 `delegate*`時都要求 `unsafe`，而是只在將方法群組轉換成 `delegate*`時才需要它。 這就是核心安全問題的運作位置（知道包含的元件無法在值為「作用中」時卸載）。 在其他位置上需要 `unsafe` 可能會被視為過度。

這就是設計原本的預期方式。 但是產生的語言規則覺得非常難以用。 不可能隱藏這是指標值，即使沒有 `unsafe` 關鍵字，它還是會繼續查看。 例如，不允許轉換為 `object`，它不能是 `class`的成員等等 .。。其C#設計是要求所有指標使用 `unsafe`，因此這種設計會遵循這種方式。

開發人員仍然能夠在 `delegate*` 的值之上呈現_安全_的包裝函式，就像今天一般的指標類型一樣。 請考慮：

``` csharp
unsafe struct Action {
    delegate*<void> _ptr;

    Action(delegate*<void> ptr) => _ptr = ptr;
    public void Invoke() => _ptr();
}
```

### <a name="using-delegates"></a>使用委派

不使用新的語法元素 `delegate*`，只要使用現有的 `delegate` 類型，並在類型後面加上 `*` 即可：

``` csharp
Func<object, object, bool>* ptr = &object.ReferenceEquals;
```

藉由使用指定 `CallingConvention` 值的屬性來標注 `delegate` 類型，即可完成處理呼叫慣例。 缺少屬性會表示 managed 呼叫慣例。

在 IL 中編碼此項會造成問題。 基礎值必須以指標表示，但它也需要：

1. 具有唯一的類型，可允許具有不同函式指標類型的多載。
1. 適用于跨元件界限的 OHI 用途。

最後一點特別有問題。 這表示使用 `Func<int>*` 的每個元件都必須在中繼資料中編碼對等的類型，即使 `Func<int>*` 是在元件中定義的，仍不會受到控制。
此外，在非 mscorlib 的元件中，以名稱 `System.Func<T>` 定義的其他類型，必須與在 mscorlib.dll 中定義的版本不同。

探索到的一個選項是將這類指標發出為 `mod_req(Func<int>) void*`。 不過，如果 `mod_req` 無法系結至 `TypeSpec`，因而無法以一般具現化為目標，就無法使用這種方式。

### <a name="named-function-pointers"></a>命名函式指標

函式指標語法可能很麻煩，特別是在複雜的情況下，例如嵌套函式指標。 而不是讓開發人員在每次語言允許函式指標的命名宣告時，都輸入簽章，如同使用 `delegate`所做的一樣。

``` csharp
func* void Action();

unsafe class NamedExample {
    void M(Action a) {
        a();
    }
}
```

這裡的問題之一是基礎 CLI 基本類型沒有名稱，因此這純粹是C#家發明，而且需要一些中繼資料工作才能啟用。 這是雖可行的，但對工作來說相當重要。 基本上，它C#只需要為這些名稱提供類型 def 資料表的隨附。

此外，當已檢查命名函式指標的引數時，我們發現它們可以同樣適用于許多其他案例。 比方說，宣告命名的元組是很方便的，因為在所有情況下都不需要輸入完整的簽章。

``` csharp
(int x, int y) Point;

class NamedTupleExample {
    void M(Point p) {
        Console.WriteLine(p.x);
    }
}
```

討論之後，我們決定不允許 `delegate*` 類型的命名宣告。 如果我們發現，根據客戶的使用意見反應，有很大的需求，我們將調查適用于函式指標、元組、泛型等的命名解決方案。這可能類似于其他建議的格式，例如語言中的完整 `typedef` 支援。

## <a name="future-considerations"></a>未來的考量

### <a name="static-local-functions"></a>靜態區域函式

這是指允許在區域函式上進行 `static` 修飾詞[的提案](https://github.com/dotnet/csharplang/issues/1565)。 這類函式會保證會當做 `static` 發出，並以原始程式碼中指定的完整簽章。 這類函式應該是 `&` 的有效引數，因為它包含目前沒有任何問題的本機函式

### <a name="static-delegates"></a>靜態委派

這是指允許宣告 `delegate` 型別的[提案](https://github.com/dotnet/csharplang/issues/302)，而這些型別只能參考 `static` 的成員。 其優點是這類 `delegate` 實例可以在效能敏感性案例中，免費和更好的配置。

如果函式指標功能已實作為，則可能會關閉 `static delegate` 提案。該功能的提議優點是配置的免費特性。 不過，最近的調查發現，因為元件卸載而無法達到此目的。 `static delegate` 中必須要有一個強式控制碼，才能從它所參考的方法中將元件卸載。

若要維護每個 `static delegate` 實例，您必須配置新的控制碼，以執行此提議的目標計數器。 有一些設計可將配置分攤到每個呼叫網站的單一配置，但這有點複雜，而且似乎不值得取捨。

這表示開發人員基本上必須在下列取捨之間做決定：

1. 元件卸載的安全性：這需要配置，因此 `delegate` 已經是足夠的選項。
1. 元件卸載沒有任何安全性：使用 `delegate*`。 這可以包裝在 `struct` 中，以允許在其餘程式碼中的 `unsafe` 內容之外使用。
