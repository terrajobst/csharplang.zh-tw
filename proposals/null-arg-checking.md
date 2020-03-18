---
ms.openlocfilehash: 76065293f652979ab395e131d657e44899c5a65b
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484549"
---
# <a name="simplified-null-argument-checking"></a><span data-ttu-id="e062f-101">簡化的 Null 引數檢查</span><span class="sxs-lookup"><span data-stu-id="e062f-101">Simplified Null Argument Checking</span></span>

## <a name="summary"></a><span data-ttu-id="e062f-102">摘要</span><span class="sxs-lookup"><span data-stu-id="e062f-102">Summary</span></span>
<span data-ttu-id="e062f-103">此提議提供簡化的語法，用於驗證方法引數不 `null`，且會適當地擲回 `ArgumentNullException`。</span><span class="sxs-lookup"><span data-stu-id="e062f-103">This proposal provides a simplified syntax for validating method arguments are not `null` and throwing `ArgumentNullException` appropriately.</span></span>

## <a name="motivation"></a><span data-ttu-id="e062f-104">動機</span><span class="sxs-lookup"><span data-stu-id="e062f-104">Motivation</span></span>
<span data-ttu-id="e062f-105">設計可為 null 的參考型別的工作導致我們檢查 `null` 引數驗證所需的程式碼。</span><span class="sxs-lookup"><span data-stu-id="e062f-105">The work on designing nullable reference types has caused us to examine the code necessary for `null` argument validation.</span></span> <span data-ttu-id="e062f-106">假設 NRT 不會影響程式碼執行，開發人員仍然必須加入 `if (arg is null) throw` 定案盤子程式碼，即使在完全 `null` 乾淨的專案中也一樣。</span><span class="sxs-lookup"><span data-stu-id="e062f-106">Given that NRT doesn't affect code execution developers still must add `if (arg is null) throw` boiler plate code even in projects which are fully `null` clean.</span></span> <span data-ttu-id="e062f-107">這讓我們希望能夠探索引數的最小語法，`null` 以語言進行驗證。</span><span class="sxs-lookup"><span data-stu-id="e062f-107">This gave us the desire to explore a minimal syntax for argument `null` validation in the language.</span></span> 

<span data-ttu-id="e062f-108">雖然此 `null` 參數驗證語法預期會與 NRT 一併使用，但提議與它完全無關。</span><span class="sxs-lookup"><span data-stu-id="e062f-108">While this `null` parameter validation syntax is expected to pair frequently with NRT the proposal is fully independent of it.</span></span> <span data-ttu-id="e062f-109">語法可以獨立于 `#nullable` 指示詞之外使用。</span><span class="sxs-lookup"><span data-stu-id="e062f-109">The syntax can be used independent of `#nullable` directives.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="e062f-110">詳細設計</span><span class="sxs-lookup"><span data-stu-id="e062f-110">Detailed Design</span></span> 

### <a name="null-validation-parameter-syntax"></a><span data-ttu-id="e062f-111">Null 驗證參數語法</span><span class="sxs-lookup"><span data-stu-id="e062f-111">Null validation parameter syntax</span></span>
<span data-ttu-id="e062f-112">驚嘆號運算子 `!`可以放在參數清單中的參數名稱之後，這會導致C#編譯器發出標準 `null` 檢查該參數的程式碼。</span><span class="sxs-lookup"><span data-stu-id="e062f-112">The bang operator, `!`, can be positioned after a parameter name in a parameter list and this will cause the C# compiler to emit standard `null` checking code for that parameter.</span></span> <span data-ttu-id="e062f-113">這稱為 `null` 驗證參數語法。</span><span class="sxs-lookup"><span data-stu-id="e062f-113">This is referred to as `null` validation parameter syntax.</span></span> <span data-ttu-id="e062f-114">例如：</span><span class="sxs-lookup"><span data-stu-id="e062f-114">For example:</span></span>

``` csharp
void M(string name!) {
    ...
}
```

<span data-ttu-id="e062f-115">將轉譯為：</span><span class="sxs-lookup"><span data-stu-id="e062f-115">Will be translated into:</span></span>

``` csharp
void M(string name) {
    if (name is null) {
        throw new ArgumentNullException(nameof(name));
    }
    ...
}
```

<span data-ttu-id="e062f-116">產生的 `null` 檢查會在任何開發人員撰寫方法中的程式碼之前進行。</span><span class="sxs-lookup"><span data-stu-id="e062f-116">The generated `null` check will occur before any developer authored code in the method.</span></span> <span data-ttu-id="e062f-117">當多個參數包含 `!` 運算子時，會依照宣告參數的順序來進行檢查。</span><span class="sxs-lookup"><span data-stu-id="e062f-117">When multiple parameters contain the `!` operator then the checks will occur in the same order as the parameters are declared.</span></span>

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

<span data-ttu-id="e062f-118">檢查會特別提供 `null`的參考相等，而不會叫用 `==` 或任何使用者定義的運算子。</span><span class="sxs-lookup"><span data-stu-id="e062f-118">The check will be specifically for reference equality to `null`, it does not invoke `==` or any user defined operators.</span></span> <span data-ttu-id="e062f-119">這也表示，`!` 運算子只能加入至可針對 `null`測試其類型是否相等的參數。</span><span class="sxs-lookup"><span data-stu-id="e062f-119">This also means the `!` operator can only be added to parameters whose type can be tested for equality against `null`.</span></span> <span data-ttu-id="e062f-120">這表示它無法用於其類型已知為實數值型別的參數。</span><span class="sxs-lookup"><span data-stu-id="e062f-120">This means it can't be used on a parameter whose type is known to be a value type.</span></span>

``` csharp
// Error: Cannot use ! on parameters who types derive from System.ValueType
void G<T>(T arg!) where T : struct {

}
```

<span data-ttu-id="e062f-121">在使用函式的情況下，`null` 驗證會在任何其他程式碼于此函式中執行。</span><span class="sxs-lookup"><span data-stu-id="e062f-121">In the case of a constructor the `null` validation will occur before any other code in the constructor.</span></span> <span data-ttu-id="e062f-122">其中包括：</span><span class="sxs-lookup"><span data-stu-id="e062f-122">That includes:</span></span> 

- <span data-ttu-id="e062f-123">連結至具有 `this` 或 `base` 的其他處理函式</span><span class="sxs-lookup"><span data-stu-id="e062f-123">Chaining to other constructors with `this` or `base`</span></span> 
- <span data-ttu-id="e062f-124">在此函式中隱含出現的欄位初始化運算式</span><span class="sxs-lookup"><span data-stu-id="e062f-124">Field initializers which implicitly occur in the constructor</span></span>

<span data-ttu-id="e062f-125">例如：</span><span class="sxs-lookup"><span data-stu-id="e062f-125">For example:</span></span>

``` csharp
class C {
    string field = GetString();
    C(string name!): this(name) {
        ...
    }
}
```

<span data-ttu-id="e062f-126">大致會轉譯成下列內容：</span><span class="sxs-lookup"><span data-stu-id="e062f-126">Will be roughly translated into the following:</span></span>

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

<span data-ttu-id="e062f-127">注意：這不是合法C#的程式碼，而只是實作為執行功能的近似值。</span><span class="sxs-lookup"><span data-stu-id="e062f-127">Note: this is not legal C# code but instead just an approximation of what the implementation does.</span></span> 

<span data-ttu-id="e062f-128">`null` 驗證參數語法也會在 lambda 參數清單上有效。</span><span class="sxs-lookup"><span data-stu-id="e062f-128">The `null` validation parameter syntax will also be valid on lambda parameter lists.</span></span> <span data-ttu-id="e062f-129">即使在缺少括弧的單一參數語法中，這也是有效的。</span><span class="sxs-lookup"><span data-stu-id="e062f-129">This is valid even in the single parameter syntax that lacks parens.</span></span>

``` csharp
void G() {
    // An identity lambda which throws on a null input
    Func<string, string> s = x! => x;
}
```

<span data-ttu-id="e062f-130">語法在反覆運算器方法的參數上也是有效的。</span><span class="sxs-lookup"><span data-stu-id="e062f-130">The syntax is also valid on parameters to iterator methods.</span></span> <span data-ttu-id="e062f-131">不同于 iterator 中的其他程式碼，當叫用 iterator 方法時，而不是在進行基礎枚舉器時，就會發生 `null` 驗證。</span><span class="sxs-lookup"><span data-stu-id="e062f-131">Unlike other code in the iterator the `null` validation will occur when the iterator method is invoked, not when the underlying enumerator is walked.</span></span> <span data-ttu-id="e062f-132">這適用于傳統或 `async` 的反覆運算器。</span><span class="sxs-lookup"><span data-stu-id="e062f-132">This is true for traditional or `async` iterators.</span></span>

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

<span data-ttu-id="e062f-133">`!` 運算子只能用於具有相關聯方法主體的參數清單。</span><span class="sxs-lookup"><span data-stu-id="e062f-133">The `!` operator can only be used for parameter lists which have an associated method body.</span></span> <span data-ttu-id="e062f-134">這表示它不能用在 `abstract` 方法、`interface`、`delegate` 或 `partial` 方法定義中。</span><span class="sxs-lookup"><span data-stu-id="e062f-134">This means it cannot be used in an `abstract` method, `interface`, `delegate` or `partial` method definition.</span></span>

### <a name="extending-is-null"></a><span data-ttu-id="e062f-135">擴充為 null</span><span class="sxs-lookup"><span data-stu-id="e062f-135">Extending is null</span></span>
<span data-ttu-id="e062f-136">運算式 `is null` 有效的類型將會擴充以包含不受限制的類型參數。</span><span class="sxs-lookup"><span data-stu-id="e062f-136">The types for which the expression `is null` is valid will be extended to include unconstrained type parameters.</span></span> <span data-ttu-id="e062f-137">這可讓它在 `null` 檢查有效的所有類型上，填滿檢查 `null` 的意圖。</span><span class="sxs-lookup"><span data-stu-id="e062f-137">This will allow it to fill the intent of checking for `null` on all types which a `null` check is valid.</span></span> <span data-ttu-id="e062f-138">具體而言，這是不一定是實數值型別的類型。</span><span class="sxs-lookup"><span data-stu-id="e062f-138">Specifically that is types which are not definitely known to be value types.</span></span> <span data-ttu-id="e062f-139">例如，限制為 `struct` 的型別參數不能與這個語法搭配使用。</span><span class="sxs-lookup"><span data-stu-id="e062f-139">For example Type parameters which are constrained to `struct` cannot be used with this syntax.</span></span>

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

<span data-ttu-id="e062f-140">在類型參數上 `is null` 的行為，會與今天 `== null` 相同。</span><span class="sxs-lookup"><span data-stu-id="e062f-140">The behavior of `is null` on a type parameter will be the same as `== null` today.</span></span> <span data-ttu-id="e062f-141">在類型參數具現化為實數值型別的情況下，程式碼將會評估為 `false`。</span><span class="sxs-lookup"><span data-stu-id="e062f-141">In the cases where the type parameter is instantiated as a value type the code will be evaluated as `false`.</span></span> <span data-ttu-id="e062f-142">如果是參考型別，則程式碼會執行適當的 `is null` 檢查。</span><span class="sxs-lookup"><span data-stu-id="e062f-142">For cases where it is a reference type the code will do a proper `is null` check.</span></span>

### <a name="intersection-with-nullable-reference-types"></a><span data-ttu-id="e062f-143">具有可為 Null 之參考型別的交集</span><span class="sxs-lookup"><span data-stu-id="e062f-143">Intersection with Nullable Reference Types</span></span>
<span data-ttu-id="e062f-144">具有套用至其名稱之 `!` 運算子的任何參數，會以無法 `null`的可為 null 狀態開始。</span><span class="sxs-lookup"><span data-stu-id="e062f-144">Any parameter which has a `!` operator applied to it's name will start with the nullable state being not `null`.</span></span> <span data-ttu-id="e062f-145">即使參數本身的類型可能 `null`，也是如此。</span><span class="sxs-lookup"><span data-stu-id="e062f-145">This is true even if the type of the parameter itself is potentially `null`.</span></span> <span data-ttu-id="e062f-146">這可能會發生于明確可為 null 的型別，例如 `string?`或具有不受限制的型別參數。</span><span class="sxs-lookup"><span data-stu-id="e062f-146">That can occur with an explicitly nullable type, such as say `string?`, or with an unconstrained type parameter.</span></span> 

<span data-ttu-id="e062f-147">當參數上的 `!` 語法與參數上明確的可為 null 類型結合時，編譯器會發出警告：</span><span class="sxs-lookup"><span data-stu-id="e062f-147">When a `!` syntax on parameters is combined with an explicitly nullable type on the parameter then a warning will be issued by the compiler:</span></span>

``` csharp
void WarnCase<T>(
    string? name!, // Warning: combining explicit null checking with a nullable type
    T value1 // Okay
)
```

## <a name="open-issues"></a><span data-ttu-id="e062f-148">開啟問題</span><span class="sxs-lookup"><span data-stu-id="e062f-148">Open Issues</span></span>
<span data-ttu-id="e062f-149">None</span><span class="sxs-lookup"><span data-stu-id="e062f-149">None</span></span>

## <a name="considerations"></a><span data-ttu-id="e062f-150">考量</span><span class="sxs-lookup"><span data-stu-id="e062f-150">Considerations</span></span>

### <a name="constructors"></a><span data-ttu-id="e062f-151">建構函式</span><span class="sxs-lookup"><span data-stu-id="e062f-151">Constructors</span></span>
<span data-ttu-id="e062f-152">函式的程式碼產生，表示從標準 `null` 驗證，以及 `null` 驗證參數語法（`!`）移動時，有一個小但可觀察的行為變更。</span><span class="sxs-lookup"><span data-stu-id="e062f-152">The code generation for constructors means there is a small, but observable, behavior change when moving from standard `null` validation today and the `null` validation parameter syntax (`!`).</span></span> <span data-ttu-id="e062f-153">標準驗證中的 `null` 檢查會在欄位初始化運算式和任何 `base` 或 `this` 呼叫之後發生。</span><span class="sxs-lookup"><span data-stu-id="e062f-153">The `null` check in standard validation occurs after both field initializers and any `base` or `this` calls.</span></span> <span data-ttu-id="e062f-154">這表示開發人員不一定要將其 `null` 驗證的100% 遷移至新的語法。</span><span class="sxs-lookup"><span data-stu-id="e062f-154">This means a developer can't necessarily migrate 100% of their `null` validation to the new syntax.</span></span> <span data-ttu-id="e062f-155">至少需要進行一些檢查。</span><span class="sxs-lookup"><span data-stu-id="e062f-155">Constructors at least require some inspection.</span></span>

<span data-ttu-id="e062f-156">在討論之後，我們決定這不太可能會造成任何重大的採用問題。</span><span class="sxs-lookup"><span data-stu-id="e062f-156">After discussion though it was decided that this is very unlikely to cause any significant adoption issues.</span></span> <span data-ttu-id="e062f-157">`null` 檢查在此函式中的任何邏輯執行之前，會有更多的邏輯。</span><span class="sxs-lookup"><span data-stu-id="e062f-157">It's more logical that the `null` check run before any logic in the constructor does.</span></span> <span data-ttu-id="e062f-158">如果發現重大的相容性問題，可以重新流覽。</span><span class="sxs-lookup"><span data-stu-id="e062f-158">Can revisit if significant compat issues are discovered.</span></span>

### <a name="warning-when-mixing--and-"></a><span data-ttu-id="e062f-159">混合時出現警告？</span><span class="sxs-lookup"><span data-stu-id="e062f-159">Warning when mixing ?</span></span> <span data-ttu-id="e062f-160">和!</span><span class="sxs-lookup"><span data-stu-id="e062f-160">and !</span></span>
<span data-ttu-id="e062f-161">當 `!` 語法套用至明確輸入為可為 null 的型別的參數時，是否應發出警告，是一段冗長的討論。</span><span class="sxs-lookup"><span data-stu-id="e062f-161">There was a lengthy discussion on whether or not a warning should be issued when the `!` syntax is applied to a parameter which is explicitly typed to a nullable type.</span></span> <span data-ttu-id="e062f-162">就表面而言，開發人員似乎會有無意義的宣告，但在某些情況下，型別階層可能會強制開發人員進行這類情況。</span><span class="sxs-lookup"><span data-stu-id="e062f-162">On the surface it seems like a nonsensical declaration by the developer but there are cases where type hierarchies could force developers into such a situation.</span></span> 

<span data-ttu-id="e062f-163">請考慮一系列元件的下列類別階層（假設全部都是以已啟用 `null` 檢查）來編譯：</span><span class="sxs-lookup"><span data-stu-id="e062f-163">Consider the following class hierarchy across a series of assemblies (assuming all are compiled with `null` checking enabled):</span></span>

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

<span data-ttu-id="e062f-164">在這裡，`C3` 的作者決定要將 `null` 驗證新增至 `o`參數。</span><span class="sxs-lookup"><span data-stu-id="e062f-164">Here the author of `C3` decided to add `null` validation to the parameter `o`.</span></span> <span data-ttu-id="e062f-165">這完全符合功能的使用方式。</span><span class="sxs-lookup"><span data-stu-id="e062f-165">This is completely in line with how the feature is intended to be used.</span></span>

<span data-ttu-id="e062f-166">現在，假設 Assembly2 的作者決定新增下列覆寫：</span><span class="sxs-lookup"><span data-stu-id="e062f-166">Now imagine at a later date the author of Assembly2 decides to add the following override:</span></span>

``` csharp
// Assembly2
abstract class C2 : C1 {
   protected override void M(object? o) { 
       ...
   }
}
```

<span data-ttu-id="e062f-167">這是可為 null 的參考型別所允許的，因為它是合理的，讓合約更有彈性地輸入位置。</span><span class="sxs-lookup"><span data-stu-id="e062f-167">This is allowed by nullable reference types as it's legal to make the contract more flexible for input positions.</span></span> <span data-ttu-id="e062f-168">一般的 NRT 功能允許在參數/傳回 null 屬性上有合理的共同/反變數。</span><span class="sxs-lookup"><span data-stu-id="e062f-168">The NRT feature in general allows for reasonable co/contravariance on parameter / return nullability.</span></span> <span data-ttu-id="e062f-169">不過，此語言會根據最特定的覆寫來進行共同/反變數檢查，而不是原始的宣告。</span><span class="sxs-lookup"><span data-stu-id="e062f-169">However the language does the co/contravariance checking based on the most specific override, not the original declaration.</span></span> <span data-ttu-id="e062f-170">這表示 Assembly3 的作者會收到有關 `o` 不相符之類型的警告，而且需要將簽章變更為下列內容，才能將其排除：</span><span class="sxs-lookup"><span data-stu-id="e062f-170">This means the author of Assembly3 will get a warning about the type of `o` not matching and will need to change the signature to the following to eliminate it:</span></span> 

``` csharp
// Assembly3
abstract class C3 : C2 { 
    protected override void M(object? o!) {
        ...
    }
}
```

<span data-ttu-id="e062f-171">到目前為止，Assembly3 的作者有幾個選擇：</span><span class="sxs-lookup"><span data-stu-id="e062f-171">At this point the author of Assembly3 has a few choices:</span></span>

- <span data-ttu-id="e062f-172">他們可以接受/隱藏關於 `object?` 和 `object` 不相符的警告。</span><span class="sxs-lookup"><span data-stu-id="e062f-172">They can accept / suppress the warning about `object?` and `object` mismatch.</span></span>
- <span data-ttu-id="e062f-173">他們可以接受/隱藏關於 `object?` 和 `!` 不相符的警告。</span><span class="sxs-lookup"><span data-stu-id="e062f-173">They can accept / suppress the warning about `object?` and `!` mismatch.</span></span>
- <span data-ttu-id="e062f-174">他們可以直接移除 `null` 驗證檢查（delete `!` 並執行明確檢查）</span><span class="sxs-lookup"><span data-stu-id="e062f-174">They can just remove the `null` validation check (delete `!` and do explicit checking)</span></span>

<span data-ttu-id="e062f-175">這是真正的案例，但現在的概念是要繼續進行警告。</span><span class="sxs-lookup"><span data-stu-id="e062f-175">This is a real scenario but for now the idea is to move forward with the warning.</span></span> <span data-ttu-id="e062f-176">如果出現警告的頻率比我們預期的還要頻繁，我們可以稍後再將它移除（相反的結果不是 true）。</span><span class="sxs-lookup"><span data-stu-id="e062f-176">If it turns out the warning happens more frequently than we anticipate then we can remove it later (the reverse is not true).</span></span>

### <a name="implicit-property-setter-arguments"></a><span data-ttu-id="e062f-177">隱含屬性 setter 引數</span><span class="sxs-lookup"><span data-stu-id="e062f-177">Implicit property setter arguments</span></span>
<span data-ttu-id="e062f-178">參數的 `value` 引數是隱含的，而且不會出現在任何參數清單中。</span><span class="sxs-lookup"><span data-stu-id="e062f-178">The `value` argument of a parameter is implicit and does not appear in any parameter list.</span></span> <span data-ttu-id="e062f-179">這表示它不能是這項功能的目標。</span><span class="sxs-lookup"><span data-stu-id="e062f-179">That means it cannot be a target of this feature.</span></span> <span data-ttu-id="e062f-180">屬性 setter 語法可以擴充以包含參數清單，以允許套用 `!` 運算子。</span><span class="sxs-lookup"><span data-stu-id="e062f-180">The property setter syntax could be extended to include a parameter list to allow the `!` operator to be applied.</span></span> <span data-ttu-id="e062f-181">但是，這項功能的概念會使 `null` 驗證變得更簡單。</span><span class="sxs-lookup"><span data-stu-id="e062f-181">But that cuts against the idea of this feature making `null` validation simpler.</span></span> <span data-ttu-id="e062f-182">如此一來，隱含 `value` 引數就不會使用這項功能。</span><span class="sxs-lookup"><span data-stu-id="e062f-182">As such the implicit `value` argument just won't work with this feature.</span></span>

## <a name="future-considerations"></a><span data-ttu-id="e062f-183">未來的考量</span><span class="sxs-lookup"><span data-stu-id="e062f-183">Future Considerations</span></span>
<span data-ttu-id="e062f-184">None</span><span class="sxs-lookup"><span data-stu-id="e062f-184">None</span></span>
