---
ms.openlocfilehash: ac4c8760e3b6a0934f01ae634f666af60aa1c0fe
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484563"
---
# <a name="compiler-intrinsics"></a>編譯器內建

## <a name="summary"></a>摘要

此提案提供的語言結構會公開目前無法有效率地存取的低層級 IL opcode，或完全： `ldftn`、`ldvirtftn`、`ldtoken` 和 `calli`。 在高效能程式碼中，這些低層級的 opcode 可能很重要，而開發人員需要有效率的方式來存取它們。

## <a name="motivation"></a>動機

下列問題會說明這項功能的動機和背景（這是功能的可能執行）： 

https://github.com/dotnet/csharplang/issues/191

這項替代設計提案會在複習原始提案的原型執行後，透過 @msjabby 以及整個重要程式碼基底的使用方式來完成。 這種設計是透過 @mjsabby、@tmat 和 @jkotas的大量輸入進行。

## <a name="detailed-design"></a>詳細設計 

### <a name="allow-address-of-to-target-methods"></a>允許目標方法的位址

方法群組現在將允許當做位址運算式的引數。 這類運算式的類型將會 `void*`。 

``` csharp
class Util { 
    public static void Log() { } 
}

// ldftn Util.Log
void* ptr = &Util.Log; 
```

假設這裡沒有委派轉換，在方法群組中篩選成員的唯一機制是透過靜態/實例存取。 如果無法區分成員，則會發生編譯時期錯誤。

``` csharp
class Util { 
    public void Log() { } 
    public void Log(string p1) { } 
    public static void Log(int i) { };
}

unsafe {
    // Error: Method group Log has more than one applicable candidate.
    void* ptr1 = &Log; 

    // Okay: only one static member to consider here.
    void* ptr2 = &Util.Log;
}
```

此內容中的 addressof 運算式會以下列方式執行：

- ldftn：當此方法為非虛擬時。
- ldvirtftn：當方法為虛擬時。

這項功能的限制：

- 只有在對值使用調用運算式時，才能指定實例方法
- 區域函數不能用在 `&`中。 語言不會指定這些方法的執行詳細資料。 這包括它們是靜態的還是實例，或是它們所發出的簽章。

### <a name="handleof"></a>handleof

`handleof` 內容關鍵字會使用 `ldtoken` 指令，將欄位、成員或類型轉譯為其對等的 `RuntimeHandle` 類型。 運算式的確切類型將取決於 `handleof`中的名稱類型：

- 欄位： `RuntimeFieldHandle`
- 類型： `RuntimeTypeHandle`
- 方法： `RuntimeMethodHandle`

`handleof` 的引數與 `nameof`相同。 它必須是簡單名稱、限定名稱、成員存取、具有指定成員的基本存取權，或具有指定成員的此存取權。 引數運算式會識別程式碼定義，但永遠不會加以評估。

`handleof` 運算式會在執行時間進行評估，而且具有 `RuntimeHandle`的傳回類型。 這可以在安全的程式碼和不安全的中執行。 

``` 
RuntimeHandle stringHandle = handleof(string);
```

這項功能的限制：

- 屬性不能用在 `handleof` 運算式中。
- 當範圍內有現有的 `handleof` 名稱時，就無法使用 `handleof` 運算式。 例如，類型、命名空間等。

### <a name="calli"></a>calli

編譯器會新增新類型 `extern` 函數的支援，以有效率地轉譯成 `.calli` 指令。 Extern 屬性會以下列圖形的屬性標示：

``` csharp
[AttributeUsage(AttributeTargets.Method)]
public sealed class CallIndirectAttribute : Attribute
{
    public CallingConvention CallingConvention { get; }
    public CallIndirectAttribute(CallingConvention callingConvention)
    {
        CallingConvention = callingConvention;
    }
}
```

這可讓開發人員以下列形式定義方法：

``` csharp
[CallIndirect(CallingConvention.Cdecl)]
static extern int MapValue(string s, void *ptr);

unsafe {
    var i = MapValue("42", &int.Parse);
    Console.WriteLine(i);
}
```

已套用 `CallIndirect` 屬性之方法的限制：

- 不能有 `DllImport` 屬性。
- 不可以是泛型。

## <a name="open-issues"></a>開啟問題

### <a name="callingconvention"></a>CallingConvention

所設計的 `CallIndirectAttribute` 會使用缺少受控呼叫慣例專案的 `CallingConvention` 列舉。 列舉必須擴充以包含此呼叫慣例，否則屬性需要採用不同的方法。

## <a name="considerations"></a>考量

### <a name="disambiguating-method-groups"></a>厘清方法群組

有一些功能討論，可讓您更輕鬆地區分傳遞給傳址運算式的方法群組。 例如，可能會將簽章元素新增至語法：

``` csharp
class Util {
    public static void Log() { ... }
    public static void Log(string) { ... }
}

unsafe {
    // Error: ambiguous Log
    void *ptr1 = &Util.Log;

    // Use Util.Log();
    void *ptr2 = &Util.Log();
}
```

這會被拒絕，因為您可能無法進行很引人注目的情況，也無法在此想像出簡單的語法。 另外還有一個相當直接的解決方法：簡單地定義另一個明確的方法，並C#使用程式碼呼叫所需的函式。 

``` csharp
class Workaround {
    public static void LocalLog() => Util.Log();
}
unsafe { 
    void* ptr = &Workaround.LocalLog;
}
```

如果 `static` 區域函式進入語言，這會變得更簡單。 然後，您可以在使用不明確的作業通訊的相同函式中定義因應的工作：

``` csharp
unsafe { 
    static void LocalLog() => Util.Log();
    void* ptr = &Workaround.LocalLog;
}
```

### <a name="loadtypetokenint32"></a>LoadTypeTokenInt32

在編譯時期，允許將元資料標記載入為 `int` 值的原始提案。 基本上具有與 `handleof` 相同之引數的 `tokenof`，但會在編譯時期評估為 `int` 常數。 

這會遭到拒絕，因為它會造成 IL 重寫（其中 .NET 有許多）的嚴重問題。 這種 rewriters 通常會以可能會使這些值失效的方式來操作中繼資料資料表。 當這類 rewriters 儲存為簡單 `int` 值時，沒有合理的方式可更新這些值。

具有中繼資料專案之不透明控制碼的基本概念，將繼續由執行時間小組探索。 

## <a name="future-considerations"></a>未來的考量

### <a name="static-local-functions"></a>靜態區域函式

這是指允許在區域函式上進行 `static` 修飾詞[的提案](https://github.com/dotnet/csharplang/issues/1565)。 這類函式會保證會當做 `static` 發出，並以原始程式碼中指定的完整簽章。 這類函式應該是 `&` 的有效引數，因為它不包含本機函式目前所沒有的任何問題。

### <a name="nativecallableattribute"></a>NativeCallableAttribute

CLR 有一項功能，可讓 managed 方法發出，使其可以直接從機器碼呼叫。 這是藉由將 `NativeCallableAttribute` 新增至方法來完成。 這種方法只能從機器碼呼叫，因此只能在簽章中包含可複製的類型。 從 managed 程式碼呼叫會導致執行階段錯誤。 

這項功能適用于此提案，因為它允許：

- 將 managed 程式碼中定義的函式傳遞至機器碼，做為函式指標（透過的位址），而不會造成 managed 或機器碼的額外負荷。 
- 執行時間可能會在 managed 程式碼中引進這類函式的「使用網站錯誤」，以避免在編譯時期叫用這些函式。




