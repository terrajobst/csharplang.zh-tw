---
ms.openlocfilehash: fa3326bf69c83b6042b1db7b5567fd5c28d6f81a
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484521"
---
# <a name="callerargumentexpression"></a><span data-ttu-id="1f669-101">CallerArgumentExpression</span><span class="sxs-lookup"><span data-stu-id="1f669-101">CallerArgumentExpression</span></span>

* <span data-ttu-id="1f669-102">[x] 提議</span><span class="sxs-lookup"><span data-stu-id="1f669-102">[x] Proposed</span></span>
* <span data-ttu-id="1f669-103">[] 原型：未啟動</span><span class="sxs-lookup"><span data-stu-id="1f669-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="1f669-104">[] 執行：未啟動</span><span class="sxs-lookup"><span data-stu-id="1f669-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="1f669-105">[] 規格：未啟動</span><span class="sxs-lookup"><span data-stu-id="1f669-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="1f669-106">摘要</span><span class="sxs-lookup"><span data-stu-id="1f669-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="1f669-107">允許開發人員捕獲傳遞至方法的運算式，以在診斷/測試 Api 中啟用更好的錯誤訊息，並減少擊鍵。</span><span class="sxs-lookup"><span data-stu-id="1f669-107">Allow developers to capture the expressions passed to a method, to enable better error messages in diagnostic/testing APIs and reduce keystrokes.</span></span>

## <a name="motivation"></a><span data-ttu-id="1f669-108">動機</span><span class="sxs-lookup"><span data-stu-id="1f669-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="1f669-109">當判斷提示或引數驗證失敗時，開發人員想要盡可能知道失敗的位置和原因。</span><span class="sxs-lookup"><span data-stu-id="1f669-109">When an assertion or argument validation fails, the developer wants to know as much as possible about where and why it failed.</span></span> <span data-ttu-id="1f669-110">不過，現今的診斷 Api 並不能完全加速。</span><span class="sxs-lookup"><span data-stu-id="1f669-110">However, today's diagnostic APIs do not fully facilitate this.</span></span> <span data-ttu-id="1f669-111">請考慮下列方法：</span><span class="sxs-lookup"><span data-stu-id="1f669-111">Consider the following method:</span></span>

```csharp
T Single<T>(this T[] array)
{
    Debug.Assert(array != null);
    Debug.Assert(array.Length == 1);

    return array[0];
}
```

<span data-ttu-id="1f669-112">當其中一個判斷提示失敗時，堆疊追蹤中只會提供檔案名、行號和方法名稱。</span><span class="sxs-lookup"><span data-stu-id="1f669-112">When one of the asserts fail, only the filename, line number, and method name will be provided in the stack trace.</span></span> <span data-ttu-id="1f669-113">開發人員將無法分辨此資訊中有哪些判斷提示失敗，他必須開啟檔案並流覽至提供的行號，以查看發生錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="1f669-113">The developer will not be able to tell which assert failed from this information-- (s)he will have to open the file and navigate to the provided line number to see what went wrong.</span></span>

<span data-ttu-id="1f669-114">這也是測試架構必須提供各種 assert 方法的原因。</span><span class="sxs-lookup"><span data-stu-id="1f669-114">This is also the reason testing frameworks have to provide a variety of assert methods.</span></span> <span data-ttu-id="1f669-115">使用 xUnit 時，`Assert.True` 和 `Assert.False` 不常使用，因為它們無法提供有關失敗原因的足夠內容。</span><span class="sxs-lookup"><span data-stu-id="1f669-115">With xUnit, `Assert.True` and `Assert.False` are not frequently used because they do not provide enough context about what failed.</span></span>

<span data-ttu-id="1f669-116">雖然此情況比較適合引數驗證，因為對開發人員顯示無效引數的名稱，所以開發人員必須手動將這些名稱傳遞給例外狀況。</span><span class="sxs-lookup"><span data-stu-id="1f669-116">While the situation is a bit better for argument validation because the names of invalid arguments are shown to the developer, the developer must pass these names to exceptions manually.</span></span> <span data-ttu-id="1f669-117">如果上述範例已重寫為使用傳統引數驗證，而不是 `Debug.Assert`，則看起來會像這樣</span><span class="sxs-lookup"><span data-stu-id="1f669-117">If the above example were rewritten to use traditional argument validation instead of `Debug.Assert`, it would look like</span></span>

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

<span data-ttu-id="1f669-118">請注意，`nameof(array)` 必須傳遞至每個例外狀況，雖然它已經從內容中清楚指出引數無效。</span><span class="sxs-lookup"><span data-stu-id="1f669-118">Notice that `nameof(array)` must be passed to each exception, although it's already clear from context which argument is invalid.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="1f669-119">詳細設計</span><span class="sxs-lookup"><span data-stu-id="1f669-119">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="1f669-120">在上述範例中，包含判斷提示訊息中的字串 `"array != null"` 或 `"array.Length == 1"` 可協助開發人員判定失敗的內容。</span><span class="sxs-lookup"><span data-stu-id="1f669-120">In the above examples, including the string `"array != null"` or `"array.Length == 1"` in the assert message would help the developer determine what failed.</span></span> <span data-ttu-id="1f669-121">輸入 `CallerArgumentExpression`：此為可供架構用來取得與特定方法引數相關聯之字串的屬性。</span><span class="sxs-lookup"><span data-stu-id="1f669-121">Enter `CallerArgumentExpression`: it's an attribute the framework can use to obtain the string associated with a particular method argument.</span></span> <span data-ttu-id="1f669-122">我們會將它新增至 `Debug.Assert`，如下所示</span><span class="sxs-lookup"><span data-stu-id="1f669-122">We would add it to `Debug.Assert` like so</span></span>

```csharp
public static class Debug
{
    public static void Assert(bool condition, [CallerArgumentExpression("condition")] string message = null);
}
```

<span data-ttu-id="1f669-123">上述範例中的原始程式碼會保持不變。</span><span class="sxs-lookup"><span data-stu-id="1f669-123">The source code in the above example would stay the same.</span></span> <span data-ttu-id="1f669-124">不過，編譯器實際發出的程式碼會對應至</span><span class="sxs-lookup"><span data-stu-id="1f669-124">However, the code the compiler actually emits would correspond to</span></span>

```csharp
T Single<T>(this T[] array)
{
    Debug.Assert(array != null, "array != null");
    Debug.Assert(array.Length == 1, "array.Length == 1");

    return array[0];
}
```

<span data-ttu-id="1f669-125">編譯器會特別識別 `Debug.Assert`上的屬性。</span><span class="sxs-lookup"><span data-stu-id="1f669-125">The compiler specially recognizes the attribute on `Debug.Assert`.</span></span> <span data-ttu-id="1f669-126">它會在呼叫位置傳遞與屬性的函式（在此案例中為 `condition`）中所參考之引數相關聯的字串。</span><span class="sxs-lookup"><span data-stu-id="1f669-126">It passes the string associated with the argument referred to in the attribute's constructor (in this case, `condition`) at the call site.</span></span> <span data-ttu-id="1f669-127">當任一項判斷提示失敗時，開發人員將會顯示為 false 的條件，並知道哪一個失敗。</span><span class="sxs-lookup"><span data-stu-id="1f669-127">When either assert fails, the developer will be shown the condition that was false and will know which one failed.</span></span>

<span data-ttu-id="1f669-128">針對引數驗證，屬性不能直接使用，但可以透過 helper 類別來使用：</span><span class="sxs-lookup"><span data-stu-id="1f669-128">For argument validation, the attribute cannot be used directly, but can be made use of through a helper class:</span></span>

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

<span data-ttu-id="1f669-129">在 https://github.com/dotnet/corefx/issues/17068進行將這類 helper 類別加入至架構的提案。</span><span class="sxs-lookup"><span data-stu-id="1f669-129">A proposal to add such a helper class to the framework is underway at https://github.com/dotnet/corefx/issues/17068.</span></span> <span data-ttu-id="1f669-130">若此語言功能已實行，則可以更新提案以利用這項功能。</span><span class="sxs-lookup"><span data-stu-id="1f669-130">If this language feature was implemented, the proposal could be updated to take advantage of this feature.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="1f669-131">擴充方法</span><span class="sxs-lookup"><span data-stu-id="1f669-131">Extension methods</span></span>

<span data-ttu-id="1f669-132">`CallerArgumentExpression`可以參考擴充方法中的 `this` 參數。</span><span class="sxs-lookup"><span data-stu-id="1f669-132">The `this` parameter in an extension method may be referenced by `CallerArgumentExpression`.</span></span> <span data-ttu-id="1f669-133">例如：</span><span class="sxs-lookup"><span data-stu-id="1f669-133">For example:</span></span>

```csharp
public static void ShouldBe<T>(this T @this, T expected, [CallerArgumentExpression("this")] string thisExpression = null) {}

contestant.Points.ShouldBe(1337); // thisExpression: "contestant.Points"
```

<span data-ttu-id="1f669-134">`thisExpression` 將會接收對應于點之前之物件的運算式。</span><span class="sxs-lookup"><span data-stu-id="1f669-134">`thisExpression` will receive the expression corresponding to the object before the dot.</span></span> <span data-ttu-id="1f669-135">如果使用靜態方法語法（例如 `Ext.ShouldBe(contestant.Points, 1337)`）來呼叫它，其行為會如同第一個參數未標記為 `this`。</span><span class="sxs-lookup"><span data-stu-id="1f669-135">If it's called with static method syntax, e.g. `Ext.ShouldBe(contestant.Points, 1337)`, it will behave as if first parameter wasn't marked `this`.</span></span>

<span data-ttu-id="1f669-136">應該一律會有對應到 `this` 參數的運算式。</span><span class="sxs-lookup"><span data-stu-id="1f669-136">There should always be an expression corresponding to the `this` parameter.</span></span> <span data-ttu-id="1f669-137">即使類別的實例本身會呼叫擴充方法，例如從集合型別內部 `this.Single()`，`this` 仍會由編譯器強制，以便 `"this"` 傳遞。</span><span class="sxs-lookup"><span data-stu-id="1f669-137">Even if an instance of a class calls an extension method on itself, e.g. `this.Single()` from inside a collection type, the `this` is mandated by the compiler so `"this"` will get passed.</span></span> <span data-ttu-id="1f669-138">如果未來此規則有所變更，我們可以考慮傳遞 `null` 或空字串。</span><span class="sxs-lookup"><span data-stu-id="1f669-138">If this rule is changed in the future, we can consider passing `null` or the empty string.</span></span>

### <a name="extra-details"></a><span data-ttu-id="1f669-139">其他詳細資料</span><span class="sxs-lookup"><span data-stu-id="1f669-139">Extra details</span></span>

- <span data-ttu-id="1f669-140">就像 `CallerMemberName`之類的其他 `Caller*` 屬性，這個屬性只能用於具有預設值的參數。</span><span class="sxs-lookup"><span data-stu-id="1f669-140">Like the other `Caller*` attributes, such as `CallerMemberName`, this attribute may only be used on parameters with default values.</span></span>
- <span data-ttu-id="1f669-141">允許以 `CallerArgumentExpression` 標記的多個參數，如上所示。</span><span class="sxs-lookup"><span data-stu-id="1f669-141">Multiple parameters marked with `CallerArgumentExpression` are permitted, as shown above.</span></span>
- <span data-ttu-id="1f669-142">屬性的命名空間將會 `System.Runtime.CompilerServices`。</span><span class="sxs-lookup"><span data-stu-id="1f669-142">The attribute's namespace will be `System.Runtime.CompilerServices`.</span></span>
- <span data-ttu-id="1f669-143">如果 `null` 或未提供參數名稱的字串（例如 `"notAParameterName"`），則編譯器會傳入空字串。</span><span class="sxs-lookup"><span data-stu-id="1f669-143">If `null` or a string that is not a parameter name (e.g. `"notAParameterName"`) is provided, the compiler will pass in an empty string.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="1f669-144">缺點</span><span class="sxs-lookup"><span data-stu-id="1f669-144">Drawbacks</span></span>
[drawbacks]: #drawbacks

- <span data-ttu-id="1f669-145">知道如何使用 decompilers 的人員，將能夠在使用此屬性標記之方法的呼叫網站上查看一些原始程式碼。</span><span class="sxs-lookup"><span data-stu-id="1f669-145">People who know how to use decompilers will be able to see some of the source code at call sites for methods marked with this attribute.</span></span> <span data-ttu-id="1f669-146">對於已關閉的來源軟體，這可能是不需要/非預期的。</span><span class="sxs-lookup"><span data-stu-id="1f669-146">This may be undesirable/unexpected for closed-source software.</span></span>

- <span data-ttu-id="1f669-147">雖然這不是功能本身的缺陷，但有一項顧慮的原因可能是目前有一個 `Debug.Assert` 的 API，只會接受 `bool`。</span><span class="sxs-lookup"><span data-stu-id="1f669-147">Although this is not a flaw in the feature itself, a source of concern may be that there exists a `Debug.Assert` API today that only takes a `bool`.</span></span> <span data-ttu-id="1f669-148">即使採用訊息的多載具有以這個屬性標記的第二個參數，並設為選擇性，編譯器仍會在多載解析中挑選沒有訊息。</span><span class="sxs-lookup"><span data-stu-id="1f669-148">Even if the overload taking a message had its second parameter marked with this attribute and made optional, the compiler would still pick the no-message one in overload resolution.</span></span> <span data-ttu-id="1f669-149">因此，您必須移除無訊息多載，才能利用這項功能，這會是二進位（但不是來源）中斷性變更。</span><span class="sxs-lookup"><span data-stu-id="1f669-149">Therefore, the no-message overload would have to be removed to take advantage of this feature, which would be a binary (although not source) breaking change.</span></span>

## <a name="alternatives"></a><span data-ttu-id="1f669-150">替代方案</span><span class="sxs-lookup"><span data-stu-id="1f669-150">Alternatives</span></span>
[alternatives]: #alternatives

- <span data-ttu-id="1f669-151">如果能夠在呼叫網站上查看使用此屬性之方法的原始程式碼證明為問題，我們可以加入宣告屬性的效果。</span><span class="sxs-lookup"><span data-stu-id="1f669-151">If being able to see source code at call sites for methods that use this attribute proves to be a problem, we can make the attribute's effects opt-in.</span></span> <span data-ttu-id="1f669-152">開發人員會透過其放入 `AssemblyInfo.cs`的整個元件 `[assembly: EnableCallerArgumentExpression]` 屬性來啟用它。</span><span class="sxs-lookup"><span data-stu-id="1f669-152">Developers will enable it through an assembly-wide `[assembly: EnableCallerArgumentExpression]` attribute they put in `AssemblyInfo.cs`.</span></span>
  - <span data-ttu-id="1f669-153">在未啟用屬性效果的情況下，呼叫以屬性標記的方法不會是錯誤，而是允許現有的方法使用屬性並維護來源相容性。</span><span class="sxs-lookup"><span data-stu-id="1f669-153">In the case the attribute's effects are not enabled, calling methods marked with the attribute would not be an error, to allow existing methods to use the attribute and maintain source compatibility.</span></span> <span data-ttu-id="1f669-154">不過，屬性會被忽略，且會使用提供的任何預設值來呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="1f669-154">However, the attribute would be ignored and the method would be called with whatever default value was provided.</span></span>

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

- <span data-ttu-id="1f669-155">為避免每次我們想要將新的呼叫端資訊新增至 `Debug.Assert`時，發生[二進位相容性問題][ drawbacks] ，替代方案是將 `CallerInfo` 結構新增至包含呼叫端所有必要資訊的架構。</span><span class="sxs-lookup"><span data-stu-id="1f669-155">To prevent the [binary compatibility problem][drawbacks] from occurring every time we want to add new caller info to `Debug.Assert`, an alternative solution would be to add a `CallerInfo` struct to the framework that contains all the necessary information about the caller.</span></span>

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

<span data-ttu-id="1f669-156">這原本是在 https://github.com/dotnet/csharplang/issues/87提議。</span><span class="sxs-lookup"><span data-stu-id="1f669-156">This was originally proposed at https://github.com/dotnet/csharplang/issues/87.</span></span>

<span data-ttu-id="1f669-157">這種方法有幾個缺點：</span><span class="sxs-lookup"><span data-stu-id="1f669-157">There are a few disadvantages of this approach:</span></span>

- <span data-ttu-id="1f669-158">雖然可讓您指定所需的屬性，但仍可供使用，但是即使在判斷提示成功時，還是會藉由配置運算式/呼叫 `MethodBase.GetCurrentMethod` 的陣列，來大幅降低效能。</span><span class="sxs-lookup"><span data-stu-id="1f669-158">Despite being pay-for-play friendly by allowing you to specify which properties you need, it could still hurt perf significantly by allocating an array for the expressions/calling `MethodBase.GetCurrentMethod` even when the assert passes.</span></span>

- <span data-ttu-id="1f669-159">此外，將新的旗標傳遞至 `CallerInfo` 屬性不是重大變更，`Debug.Assert` 不保證會從針對舊版方法所編譯的呼叫位置，實際收到該新參數。</span><span class="sxs-lookup"><span data-stu-id="1f669-159">Additionally, while passing a new flag to the `CallerInfo` attribute won't be a breaking change, `Debug.Assert` won't be guaranteed to actually receive that new parameter from call sites that compiled against an old version of the method.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="1f669-160">未解決的問題</span><span class="sxs-lookup"><span data-stu-id="1f669-160">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="1f669-161">TBD</span><span class="sxs-lookup"><span data-stu-id="1f669-161">TBD</span></span>

## <a name="design-meetings"></a><span data-ttu-id="1f669-162">設計會議</span><span class="sxs-lookup"><span data-stu-id="1f669-162">Design meetings</span></span>

<span data-ttu-id="1f669-163">N/A</span><span class="sxs-lookup"><span data-stu-id="1f669-163">N/A</span></span>
