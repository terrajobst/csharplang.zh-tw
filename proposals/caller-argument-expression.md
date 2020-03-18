---
ms.openlocfilehash: fa3326bf69c83b6042b1db7b5567fd5c28d6f81a
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484521"
---
# <a name="callerargumentexpression"></a>CallerArgumentExpression

* [x] 提議
* [] 原型：未啟動
* [] 執行：未啟動
* [] 規格：未啟動

## <a name="summary"></a>摘要
[summary]: #summary

允許開發人員捕獲傳遞至方法的運算式，以在診斷/測試 Api 中啟用更好的錯誤訊息，並減少擊鍵。

## <a name="motivation"></a>動機
[motivation]: #motivation

當判斷提示或引數驗證失敗時，開發人員想要盡可能知道失敗的位置和原因。 不過，現今的診斷 Api 並不能完全加速。 請考慮下列方法：

```csharp
T Single<T>(this T[] array)
{
    Debug.Assert(array != null);
    Debug.Assert(array.Length == 1);

    return array[0];
}
```

當其中一個判斷提示失敗時，堆疊追蹤中只會提供檔案名、行號和方法名稱。 開發人員將無法分辨此資訊中有哪些判斷提示失敗，他必須開啟檔案並流覽至提供的行號，以查看發生錯誤的原因。

這也是測試架構必須提供各種 assert 方法的原因。 使用 xUnit 時，`Assert.True` 和 `Assert.False` 不常使用，因為它們無法提供有關失敗原因的足夠內容。

雖然此情況比較適合引數驗證，因為對開發人員顯示無效引數的名稱，所以開發人員必須手動將這些名稱傳遞給例外狀況。 如果上述範例已重寫為使用傳統引數驗證，而不是 `Debug.Assert`，則看起來會像這樣

```csharp
T Single<T>(this T[] array)
{
    if (array == null)
    {
        throw new ArgumentNullException(nameof(array));
    }

    if (array.Length != 1)
    {
        throw new ArgumentException("Array must contain a single element.", nameof(array));
    }

    return array[0];
}
```

請注意，`nameof(array)` 必須傳遞至每個例外狀況，雖然它已經從內容中清楚指出引數無效。

## <a name="detailed-design"></a>詳細設計
[design]: #detailed-design

在上述範例中，包含判斷提示訊息中的字串 `"array != null"` 或 `"array.Length == 1"` 可協助開發人員判定失敗的內容。 輸入 `CallerArgumentExpression`：此為可供架構用來取得與特定方法引數相關聯之字串的屬性。 我們會將它新增至 `Debug.Assert`，如下所示

```csharp
public static class Debug
{
    public static void Assert(bool condition, [CallerArgumentExpression("condition")] string message = null);
}
```

上述範例中的原始程式碼會保持不變。 不過，編譯器實際發出的程式碼會對應至

```csharp
T Single<T>(this T[] array)
{
    Debug.Assert(array != null, "array != null");
    Debug.Assert(array.Length == 1, "array.Length == 1");

    return array[0];
}
```

編譯器會特別識別 `Debug.Assert`上的屬性。 它會在呼叫位置傳遞與屬性的函式（在此案例中為 `condition`）中所參考之引數相關聯的字串。 當任一項判斷提示失敗時，開發人員將會顯示為 false 的條件，並知道哪一個失敗。

針對引數驗證，屬性不能直接使用，但可以透過 helper 類別來使用：

```csharp
public static class Verify
{
    public static void Argument(bool condition, string message, [CallerArgumentExpression("condition")] string conditionExpression = null)
    {
        if (!condition) throw new ArgumentException(message: message, paramName: conditionExpression);
    }

    public static void InRange(int argument, int low, int high,
        [CallerArgumentExpression("argument")] string argumentExpression = null,
        [CallerArgumentExpression("low")] string lowExpression = null,
        [CallerArgumentExpression("high")] string highExpression = null)
    {
        if (argument < low)
        {
            throw new ArgumentOutOfRangeException(paramName: argumentExpression,
                message: $"{argumentExpression} ({argument}) cannot be less than {lowExpression} ({low}).");
        }

        if (argument > high)
        {
            throw new ArgumentOutOfRangeException(paramName: argumentExpression,
                message: $"{argumentExpression} ({argument}) cannot be greater than {highExpression} ({high}).");
        }
    }

    public static void NotNull<T>(T argument, [CallerArgumentExpression("argument")] string argumentExpression = null)
        where T : class
    {
        if (argument == null) throw new ArgumentNullException(paramName: argumentExpression);
    }
}

T Single<T>(this T[] array)
{
    Verify.NotNull(array); // paramName: "array"
    Verify.Argument(array.Length == 1, "Array must contain a single element."); // paramName: "array.Length == 1"

    return array[0];
}

T ElementAt(this T[] array, int index)
{
    Verify.NotNull(array); // paramName: "array"
    // paramName: "index"
    // message: "index (-1) cannot be less than 0 (0).", or
    //          "index (6) cannot be greater than array.Length - 1 (5)."
    Verify.InRange(index, 0, array.Length - 1);

    return array[index];
}
```

在 https://github.com/dotnet/corefx/issues/17068進行將這類 helper 類別加入至架構的提案。 若此語言功能已實行，則可以更新提案以利用這項功能。

### <a name="extension-methods"></a>擴充方法

`CallerArgumentExpression`可以參考擴充方法中的 `this` 參數。 例如：

```csharp
public static void ShouldBe<T>(this T @this, T expected, [CallerArgumentExpression("this")] string thisExpression = null) {}

contestant.Points.ShouldBe(1337); // thisExpression: "contestant.Points"
```

`thisExpression` 將會接收對應于點之前之物件的運算式。 如果使用靜態方法語法（例如 `Ext.ShouldBe(contestant.Points, 1337)`）來呼叫它，其行為會如同第一個參數未標記為 `this`。

應該一律會有對應到 `this` 參數的運算式。 即使類別的實例本身會呼叫擴充方法，例如從集合型別內部 `this.Single()`，`this` 仍會由編譯器強制，以便 `"this"` 傳遞。 如果未來此規則有所變更，我們可以考慮傳遞 `null` 或空字串。

### <a name="extra-details"></a>其他詳細資料

- 就像 `CallerMemberName`之類的其他 `Caller*` 屬性，這個屬性只能用於具有預設值的參數。
- 允許以 `CallerArgumentExpression` 標記的多個參數，如上所示。
- 屬性的命名空間將會 `System.Runtime.CompilerServices`。
- 如果 `null` 或未提供參數名稱的字串（例如 `"notAParameterName"`），則編譯器會傳入空字串。

## <a name="drawbacks"></a>缺點
[drawbacks]: #drawbacks

- 知道如何使用 decompilers 的人員，將能夠在使用此屬性標記之方法的呼叫網站上查看一些原始程式碼。 對於已關閉的來源軟體，這可能是不需要/非預期的。

- 雖然這不是功能本身的缺陷，但有一項顧慮的原因可能是目前有一個 `Debug.Assert` 的 API，只會接受 `bool`。 即使採用訊息的多載具有以這個屬性標記的第二個參數，並設為選擇性，編譯器仍會在多載解析中挑選沒有訊息。 因此，您必須移除無訊息多載，才能利用這項功能，這會是二進位（但不是來源）中斷性變更。

## <a name="alternatives"></a>替代方案
[alternatives]: #alternatives

- 如果能夠在呼叫網站上查看使用此屬性之方法的原始程式碼證明為問題，我們可以加入宣告屬性的效果。 開發人員會透過其放入 `AssemblyInfo.cs`的整個元件 `[assembly: EnableCallerArgumentExpression]` 屬性來啟用它。
  - 在未啟用屬性效果的情況下，呼叫以屬性標記的方法不會是錯誤，而是允許現有的方法使用屬性並維護來源相容性。 不過，屬性會被忽略，且會使用提供的任何預設值來呼叫方法。

```csharp
// Assembly1

void Foo(string bar); // V1
void Foo(string bar, string barExpression = "not provided"); // V2
void Foo(string bar, [CallerArgumentExpression("bar")] string barExpression = "not provided"); // V3

// Assembly2

Foo(a); // V1: Compiles to Foo(a), V2, V3: Compiles to Foo(a, "not provided")
Foo(a, "provided"); // V2, V3: Compiles to Foo(a, "provided")

// Assembly3

[assembly: EnableCallerArgumentExpression]

Foo(a); // V1: Compiles to Foo(a), V2: Compiles to Foo(a, "not provided"), V3: Compiles to Foo(a, "a")
Foo(a, "provided"); // V2, V3: Compiles to Foo(a, "provided")
```

- 為避免每次我們想要將新的呼叫端資訊新增至 `Debug.Assert`時，發生[二進位相容性問題][ drawbacks] ，替代方案是將 `CallerInfo` 結構新增至包含呼叫端所有必要資訊的架構。

```csharp
struct CallerInfo
{
    public string MemberName { get; set; }
    public string TypeName { get; set; }
    public string Namespace { get; set; }
    public string FullTypeName { get; set; }
    public string FilePath { get; set; }
    public int LineNumber { get; set; }
    public int ColumnNumber { get; set; }
    public Type Type { get; set; }
    public MethodBase Method { get; set; }
    public string[] ArgumentExpressions { get; set; }
}

[Flags]
enum CallerInfoOptions
{
    MemberName = 1, TypeName = 2, ...
}

public static class Debug
{
    public static void Assert(bool condition,
        // If a flag is not set here, the corresponding CallerInfo member is not populated by the caller, so it's
        // pay-for-play friendly.
        [CallerInfo(CallerInfoOptions.FilePath | CallerInfoOptions.Method | CallerInfoOptions.ArgumentExpressions)] CallerInfo callerInfo = default(CallerInfo))
    {
        string filePath = callerInfo.FilePath;
        MethodBase method = callerInfo.Method;
        string conditionExpression = callerInfo.ArgumentExpressions[0];
        ...
    }
}

class Bar
{
    void Foo()
    {
        Debug.Assert(false);

        // Translates to:

        var callerInfo = new CallerInfo();
        callerInfo.FilePath = @"C:\Bar.cs";
        callerInfo.Method = MethodBase.GetCurrentMethod();
        callerInfo.ArgumentExpressions = new string[] { "false" };
        Debug.Assert(false, callerInfo);
    }
}
```

這原本是在 https://github.com/dotnet/csharplang/issues/87提議。

這種方法有幾個缺點：

- 雖然可讓您指定所需的屬性，但仍可供使用，但是即使在判斷提示成功時，還是會藉由配置運算式/呼叫 `MethodBase.GetCurrentMethod` 的陣列，來大幅降低效能。

- 此外，將新的旗標傳遞至 `CallerInfo` 屬性不是重大變更，`Debug.Assert` 不保證會從針對舊版方法所編譯的呼叫位置，實際收到該新參數。

## <a name="unresolved-questions"></a>未解決的問題
[unresolved]: #unresolved-questions

TBD

## <a name="design-meetings"></a>設計會議

N/A
