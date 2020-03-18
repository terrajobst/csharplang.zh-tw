---
ms.openlocfilehash: 1457c1eb018e12af30ce5b38be704bf8851d4b25
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "79485011"
---
# <a name="efficient-params-and-string-formatting"></a><span data-ttu-id="0323c-101">有效率的參數和字串格式</span><span class="sxs-lookup"><span data-stu-id="0323c-101">Efficient Params and String Formatting</span></span>

## <a name="summary"></a><span data-ttu-id="0323c-102">摘要</span><span class="sxs-lookup"><span data-stu-id="0323c-102">Summary</span></span>
<span data-ttu-id="0323c-103">這項功能的組合將會提高格式化 `string` 值和傳遞 `params` 樣式引數的效率。</span><span class="sxs-lookup"><span data-stu-id="0323c-103">This combination of features will increase the efficiency of formatting `string` values and passing of `params` style arguments.</span></span>

## <a name="motivation"></a><span data-ttu-id="0323c-104">動機</span><span class="sxs-lookup"><span data-stu-id="0323c-104">Motivation</span></span>
<span data-ttu-id="0323c-105">`string` 值進行格式化的配置負擔可能會使許多以文字為基礎的應用程式的效能降低：從 `struct` 類型的裝箱損失、`params` 的 `object[]` 配置，以及在 `string` 呼叫期間的中繼 `string.Format` 分配。</span><span class="sxs-lookup"><span data-stu-id="0323c-105">The allocation overhead of formatting `string` values can dominate the performance of many text based applications: from the boxing penalty of `struct` types, the `object[]` allocation for `params` and the intermediate `string` allocations during `string.Format` calls.</span></span> <span data-ttu-id="0323c-106">為了維持效率，這類應用程式通常需要放棄生產力的功能，例如 `params` 和 `string` 插補，並移至非標準、手動編碼的解決方案。</span><span class="sxs-lookup"><span data-stu-id="0323c-106">In order to maintain efficiency such applications often need to abandon productivity features such as `params` and `string` interpolation and move to non-standard, hand coded solutions.</span></span> 

<span data-ttu-id="0323c-107">請考慮使用 MSBuild 作為範例。</span><span class="sxs-lookup"><span data-stu-id="0323c-107">Consider MSBuild as an example.</span></span> <span data-ttu-id="0323c-108">這是使用非常重視效能的開發C#人員所撰寫的許多現代化功能。</span><span class="sxs-lookup"><span data-stu-id="0323c-108">This is written using a lot of modern C# features by developers who are conscious of performance.</span></span> <span data-ttu-id="0323c-109">但在一個代表性的組建範例中，MSBuild 會使用最少的詳細資訊來產生 `string` 配置的262MB。</span><span class="sxs-lookup"><span data-stu-id="0323c-109">Yet in one representative build sample MSBuild will generate 262MB of `string` allocation using minimal verbosity.</span></span> <span data-ttu-id="0323c-110">這1/2 的配置是 `string.Format`內短期的配置。</span><span class="sxs-lookup"><span data-stu-id="0323c-110">Of that 1/2 of the allocations are short lived allocations inside `string.Format`.</span></span> <span data-ttu-id="0323c-111">這些功能會在 .NET Desktop 上移除大部分的內容，並在 .NET Core 上使其變得近乎零，因為 `Span<T>` 的可用性</span><span class="sxs-lookup"><span data-stu-id="0323c-111">These features would remove much of that on .NET Desktop and get it down to nearly zero on .NET Core due to the availability of `Span<T>`</span></span>

<span data-ttu-id="0323c-112">這裡所述的一組語言功能，可讓應用程式繼續使用這些功能，幾乎不需要對其應用程式的程式碼基底進行任何變換，同時移除大部分案例中非預期的配置額外負荷。</span><span class="sxs-lookup"><span data-stu-id="0323c-112">The set of language features described here will enable applications to continue using these features, with very little or no churn to their application code base, while removing the unintended allocation overhead in the majority of cases.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="0323c-113">詳細設計</span><span class="sxs-lookup"><span data-stu-id="0323c-113">Detailed Design</span></span> 
<span data-ttu-id="0323c-114">這裡會使用一組功能來達成這些結果：</span><span class="sxs-lookup"><span data-stu-id="0323c-114">There are a set of features that will be used here to achieve these results:</span></span>

- <span data-ttu-id="0323c-115">擴充 `params` 以支援一組更廣泛的集合類型。</span><span class="sxs-lookup"><span data-stu-id="0323c-115">Expanding `params` to support a broader set of collection types.</span></span>
- <span data-ttu-id="0323c-116">可讓開發人員自訂如何達到 `string` 插補。</span><span class="sxs-lookup"><span data-stu-id="0323c-116">Allowing for developers to customize how `string` interpolation is achieved.</span></span> 
- <span data-ttu-id="0323c-117">允許插入 `string` 系結至更有效率的 `string.Format` 多載。</span><span class="sxs-lookup"><span data-stu-id="0323c-117">Allowing for interpolated `string` to bind to more efficient `string.Format` overloads.</span></span>

### <a name="extending-params"></a><span data-ttu-id="0323c-118">擴充 params</span><span class="sxs-lookup"><span data-stu-id="0323c-118">Extending params</span></span>
<span data-ttu-id="0323c-119">此語言可讓方法簽章中的 `params` 具有 `Span<T>`、`ReadOnlySpan<T>` 和 `IEnumerable<T>`的類型。</span><span class="sxs-lookup"><span data-stu-id="0323c-119">The language will allow for `params` in a method signature to have the types `Span<T>`, `ReadOnlySpan<T>` and `IEnumerable<T>`.</span></span> <span data-ttu-id="0323c-120">同樣適用于調用的規則會套用至 `params T[]`的這些新類型：</span><span class="sxs-lookup"><span data-stu-id="0323c-120">The same rules for invocation will apply to these new types that apply to `params T[]`:</span></span>

- <span data-ttu-id="0323c-121">不能多載，其中唯一的差異是 `params` 關鍵字。</span><span class="sxs-lookup"><span data-stu-id="0323c-121">Can't overload where the only difference is a `params` keyword.</span></span>
- <span data-ttu-id="0323c-122">可以藉由傳遞一系列可隱含轉換成 `T` 或單一 `Span<T>` / 
`ReadOnlySpan<T>` / `IEnumerable<T>` 引數的引數來叫用。</span><span class="sxs-lookup"><span data-stu-id="0323c-122">Can invoke by passing a series of arguments that are implicitly convertible to `T` or a single `Span<T>` / 
`ReadOnlySpan<T>` / `IEnumerable<T>` argument.</span></span>
- <span data-ttu-id="0323c-123">必須是方法簽章中的最後一個參數。</span><span class="sxs-lookup"><span data-stu-id="0323c-123">Must be the last parameter in a method signature.</span></span>
- <span data-ttu-id="0323c-124">依此類推 。</span><span class="sxs-lookup"><span data-stu-id="0323c-124">Etc ...</span></span> 

<span data-ttu-id="0323c-125">為了簡單起見，`Span<T>` 和 `ReadOnlySpan<T>` 變體在下方會稱為 `Span<T>`。</span><span class="sxs-lookup"><span data-stu-id="0323c-125">The `Span<T>` and `ReadOnlySpan<T>` variants will be referred to as `Span<T>` below for simplicity.</span></span> <span data-ttu-id="0323c-126">在 `ReadOnlySpan<T>` 的行為不同的情況下，會明確地將其稱為。</span><span class="sxs-lookup"><span data-stu-id="0323c-126">In cases where the behavior of `ReadOnlySpan<T>` differs it will be explicitly called out.</span></span> 

<span data-ttu-id="0323c-127">`params` 提供的 `Span<T>` 變體的優點是，它可讓編譯器在配置 `Span<T>` 值的備份儲存體時有很大的彈性。</span><span class="sxs-lookup"><span data-stu-id="0323c-127">The advantage the `Span<T>` variants of `params` provides is it gives the compiler great flexibility in how it allocates the backing storage for the `Span<T>` value.</span></span> <span data-ttu-id="0323c-128">使用 `params T[]` 編譯器必須為每個 `params` 方法的調用配置新的 `T[]`。</span><span class="sxs-lookup"><span data-stu-id="0323c-128">With a `params T[]` the compiler must allocate a new `T[]` for every invocation of a `params` method.</span></span> <span data-ttu-id="0323c-129">不可能重複使用，因為它必須假設被呼叫者已儲存並重複使用參數。</span><span class="sxs-lookup"><span data-stu-id="0323c-129">Re-use is not possible because it must assume the callee stored and reused the parameter.</span></span> <span data-ttu-id="0323c-130">這可能會導致具有大量 `params` 調用的方法有很大的效率。</span><span class="sxs-lookup"><span data-stu-id="0323c-130">This can lead to a large inefficiency in methods with lots of `params` invocations.</span></span>

<span data-ttu-id="0323c-131">指定的 `Span<T>` 變異 `ref struct` 被呼叫端無法儲存引數。</span><span class="sxs-lookup"><span data-stu-id="0323c-131">Given `Span<T>` variants are `ref struct` the callee cannot store the argument.</span></span> <span data-ttu-id="0323c-132">因此，編譯器可以採取像是重新使用引數的動作，將呼叫網站優化。</span><span class="sxs-lookup"><span data-stu-id="0323c-132">Hence the compiler can optimize the call sites by taking actions like re-using the argument.</span></span> <span data-ttu-id="0323c-133">相較于 `T[]`，這可讓重複調用的效率非常有效率。</span><span class="sxs-lookup"><span data-stu-id="0323c-133">This can make repeated invocations very efficient as compared to `T[]`.</span></span> <span data-ttu-id="0323c-134">不過，此語言不會對這類 callsite 的優化提供任何特定的保證。</span><span class="sxs-lookup"><span data-stu-id="0323c-134">The language though will make no specific guarantees about how such callsites are optimized.</span></span> <span data-ttu-id="0323c-135">請注意，在叫用 `params Span<T>` 方法時，編譯器可以免費使用 `T[]` 以外的值。</span><span class="sxs-lookup"><span data-stu-id="0323c-135">Only note that the compiler is free to use values other than `T[]` when invoking a `params Span<T>` method.</span></span> 

<span data-ttu-id="0323c-136">以下是其中一種可能的執行方式。</span><span class="sxs-lookup"><span data-stu-id="0323c-136">One such potential implementation is the following.</span></span> <span data-ttu-id="0323c-137">請考慮方法主體中的所有 `params` 調用。</span><span class="sxs-lookup"><span data-stu-id="0323c-137">Consider all `params` invocation in a method body.</span></span> <span data-ttu-id="0323c-138">編譯器可以配置大小等於最大 `params` 調用的陣列，並藉由在陣列上建立適當大小的 `Span<T>` 實例，將其用於所有調用。</span><span class="sxs-lookup"><span data-stu-id="0323c-138">The compiler could allocate an array which has a size equal to the largest `params` invocation and use that for all of the invocations by creating appropriately sized `Span<T>` instances over the array.</span></span> <span data-ttu-id="0323c-139">例如：</span><span class="sxs-lookup"><span data-stu-id="0323c-139">For example:</span></span>

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

<span data-ttu-id="0323c-140">編譯器可以選擇發出 `Go` 的主體，如下所示：</span><span class="sxs-lookup"><span data-stu-id="0323c-140">The compiler could choose to emit the body of `Go` as follows:</span></span>

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

<span data-ttu-id="0323c-141">這可以大幅減少在應用程式中配置的陣列數目。</span><span class="sxs-lookup"><span data-stu-id="0323c-141">This can significantly reduce the number of arrays allocated in an application.</span></span> <span data-ttu-id="0323c-142">如果執行時間為數組的更聰明堆疊配置提供公用程式，則配置可能更進一步降低。</span><span class="sxs-lookup"><span data-stu-id="0323c-142">Allocations can be even further reduced if the runtime provides utilities for smarter stack allocation of arrays.</span></span>

<span data-ttu-id="0323c-143">不過，這種優化不一定會套用。</span><span class="sxs-lookup"><span data-stu-id="0323c-143">This optimization cannot always be applied though.</span></span> <span data-ttu-id="0323c-144">雖然被呼叫端無法捕捉 `params` 引數，但當有 `ref` 或 `out / ref` 參數本身是 `ref struct` 類型時，仍然可以在呼叫端中加以捕捉。</span><span class="sxs-lookup"><span data-stu-id="0323c-144">Even though the callee cannot capture the `params` argument it can still be captured in the caller when there is a `ref` or a `out / ref` parameter that is itself a `ref struct` type.</span></span> 

``` csharp
static class SneakyCapture {
    static ref int M(params Span<T> span) => ref span[0];

    static void Oops() {
        // This now holds onto the memory backing the Span<T> 
        ref int r = ref M(42);
    }
}
```

<span data-ttu-id="0323c-145">不過，這些情況可透過靜態方式偵測。</span><span class="sxs-lookup"><span data-stu-id="0323c-145">These cases are statically detectable though.</span></span> <span data-ttu-id="0323c-146">當有 `ref` 傳回或 `out` 或 `ref`傳遞 `ref struct` 參數時，就可能發生這種情況。</span><span class="sxs-lookup"><span data-stu-id="0323c-146">It potentially occurs whenever there is a `ref` return or a `ref struct` parameter passed by `out` or `ref`.</span></span> <span data-ttu-id="0323c-147">在這種情況下，編譯器必須為每個調用配置一個全新的 `T[]`。</span><span class="sxs-lookup"><span data-stu-id="0323c-147">In such a case the compiler must allocate a fresh `T[]` for every invocation.</span></span> 

<span data-ttu-id="0323c-148">本檔結尾會討論數個其他可能的優化策略。</span><span class="sxs-lookup"><span data-stu-id="0323c-148">Several other potential optimization strategies are discussed at the end of this document.</span></span>

<span data-ttu-id="0323c-149">`IEnumerable<T>` 變體只是便利的多載。</span><span class="sxs-lookup"><span data-stu-id="0323c-149">The `IEnumerable<T>` variant is a merely a convenience overload.</span></span> <span data-ttu-id="0323c-150">這在經常使用 `IEnumerable<T>`，但也有許多 `params` 使用量的案例中很有用。</span><span class="sxs-lookup"><span data-stu-id="0323c-150">It's useful in scenarios which have frequent uses of `IEnumerable<T>` but also have lots of `params` usage.</span></span> <span data-ttu-id="0323c-151">在 `T` 引數中叫用時，將會將支援儲存體配置為 `T[]`，就像現在 `params T[]` 完成一樣。</span><span class="sxs-lookup"><span data-stu-id="0323c-151">When invoked in `T` argument form the backing storage will be allocated as a `T[]` just as `params T[]` is done today.</span></span>

### <a name="params-overload-resolution-changes"></a><span data-ttu-id="0323c-152">params 多載解析變更</span><span class="sxs-lookup"><span data-stu-id="0323c-152">params overload resolution changes</span></span>
<span data-ttu-id="0323c-153">此提案表示語言現在有四種 `params` 變體，在其中有一個。</span><span class="sxs-lookup"><span data-stu-id="0323c-153">This proposal means the language now has four variants of `params` where before it had one.</span></span> <span data-ttu-id="0323c-154">方法可以定義只有 `params` 宣告的類型不同之方法的多載。</span><span class="sxs-lookup"><span data-stu-id="0323c-154">It is sensible for methods to define overloads of methods that differ only on the type of a `params` declarations.</span></span> 

<span data-ttu-id="0323c-155">請考慮 `StringBuilder.AppendFormat` 除了 `params object[]`以外，一定會加入 `params ReadOnlySpan<object>` 多載。</span><span class="sxs-lookup"><span data-stu-id="0323c-155">Consider that `StringBuilder.AppendFormat` would certainly add a `params ReadOnlySpan<object>` overload in addition to the `params object[]`.</span></span> <span data-ttu-id="0323c-156">這讓 it 能夠藉由減少收集配置，而不需要對呼叫程式碼進行任何變更，大幅提升效能。</span><span class="sxs-lookup"><span data-stu-id="0323c-156">This would allow it to substantially improve performance by reducing collection allocations without requiring any changes to the calling code.</span></span> 

<span data-ttu-id="0323c-157">為了協助這種方式，此語言會引進下列多載解析系結規則。</span><span class="sxs-lookup"><span data-stu-id="0323c-157">To facilitate this the language will introduce the following overload resolution tie breaking rule.</span></span> <span data-ttu-id="0323c-158">當候選方法只有 `params` 參數的差異時，就會依照下列順序來偏好候選人：</span><span class="sxs-lookup"><span data-stu-id="0323c-158">When the candidate methods differ only by the `params` parameter then the candidates will be preferred in the following order:</span></span>

1. `ReadOnlySpan<T>`
1. `Span<T>`
1. `T[]`
1. `IEnumerable<T>`

<span data-ttu-id="0323c-159">這是一般案例最不有效率的順序。</span><span class="sxs-lookup"><span data-stu-id="0323c-159">This order is the most to the least efficient for the general case.</span></span>

### <a name="variant"></a><span data-ttu-id="0323c-160">變數</span><span class="sxs-lookup"><span data-stu-id="0323c-160">Variant</span></span>
<span data-ttu-id="0323c-161">CoreFX 會建立名為[Variant](https://github.com/dotnet/corefxlab/pull/2595)的新 managed 型別原型。</span><span class="sxs-lookup"><span data-stu-id="0323c-161">CoreFX is prototyping a new managed type named [Variant](https://github.com/dotnet/corefxlab/pull/2595).</span></span> <span data-ttu-id="0323c-162">這種類型的目的是要用於預期異類值，但不想使用 `object` 做為參數所帶來的額外負荷的 Api。</span><span class="sxs-lookup"><span data-stu-id="0323c-162">This type is meant to be used in APIs which expect heterogeneous values but don't want the overhead brought on by using `object` as the parameter.</span></span> <span data-ttu-id="0323c-163">`Variant` 類型會提供通用儲存體，但可避免最常使用之類型的裝箱配置。</span><span class="sxs-lookup"><span data-stu-id="0323c-163">The `Variant` type provides universal storage but avoids the boxing allocation for the most commonly used types.</span></span> <span data-ttu-id="0323c-164">在 `string.Format` 之類的 Api 中使用此類型，可以消除大部分案例中的「裝箱」額外負荷。</span><span class="sxs-lookup"><span data-stu-id="0323c-164">Using this type in APIs like `string.Format` can eliminate the boxing overhead in the majority of cases.</span></span>

<span data-ttu-id="0323c-165">這種類型本身不一定是語言特有的。</span><span class="sxs-lookup"><span data-stu-id="0323c-165">This type itself is not necessarily special to the language.</span></span> <span data-ttu-id="0323c-166">這份檔會另外引進，因為它會成為提案其他部分的執行詳細資料。</span><span class="sxs-lookup"><span data-stu-id="0323c-166">It is being introduced in this document separately though as it becomes an implementation detail of other parts of the proposal.</span></span> 

### <a name="efficient-interpolated-strings"></a><span data-ttu-id="0323c-167">有效率的字串插值</span><span class="sxs-lookup"><span data-stu-id="0323c-167">Efficient interpolated strings</span></span>
<span data-ttu-id="0323c-168">內插字串是中C#的熱門但沒有效率的功能。</span><span class="sxs-lookup"><span data-stu-id="0323c-168">Interpolated strings are a popular yet inefficient feature in C#.</span></span> <span data-ttu-id="0323c-169">最常見的語法（使用插入 `string` 做為 `string`）會轉譯成 `string.Format(string, params object[])` 呼叫。</span><span class="sxs-lookup"><span data-stu-id="0323c-169">The most common syntax, using an interpolated `string` as a `string`, translates into a `string.Format(string, params object[])` call.</span></span> <span data-ttu-id="0323c-170">這將會產生所有實值型別的「設定」（中繼 `string` 配置）配置，而當引數數目超過 `string.Format`的「快速」多載的參數數量時，此實作為執行時，主要會使用 `object.ToString` 進行格式化以及陣列配置。</span><span class="sxs-lookup"><span data-stu-id="0323c-170">That will incur boxing allocations for all value types, intermediate `string` allocations as the implementation largely uses `object.ToString` for formatting as well as array allocations once the number of arguments exceeds the amount of parameters on the "fast" overloads of `string.Format`.</span></span> 

<span data-ttu-id="0323c-171">語言會變更其插補，以考慮 `string.Format`的替代多載。</span><span class="sxs-lookup"><span data-stu-id="0323c-171">The language will change its interpolation lowering to consider alternate overloads of `string.Format`.</span></span> <span data-ttu-id="0323c-172">它會考慮所有形式的 `string.Format(string, params)`，並挑選符合引數類型的「最佳」多載。</span><span class="sxs-lookup"><span data-stu-id="0323c-172">It will consider all forms of `string.Format(string, params)` and pick the "best" overload which satisfies the argument types.</span></span>
<span data-ttu-id="0323c-173">「最佳」 `params` 多載將由上面討論的規則決定。</span><span class="sxs-lookup"><span data-stu-id="0323c-173">The "best" `params` overload will be determined by the rules discussed above.</span></span> <span data-ttu-id="0323c-174">這表示插補 `string` 現在可以系結至非常有效率的多載，例如 `string.Format(string format, params ReadOnlySpan<Variant> args)`。</span><span class="sxs-lookup"><span data-stu-id="0323c-174">This means interpolated `string` can now bind to very efficient overloads like `string.Format(string format, params ReadOnlySpan<Variant> args)`.</span></span> <span data-ttu-id="0323c-175">在許多情況下，這會移除所有中繼配置。</span><span class="sxs-lookup"><span data-stu-id="0323c-175">In many cases this will remove all intermediate allocations.</span></span>

### <a name="customizable-interpolated-strings"></a><span data-ttu-id="0323c-176">可自訂的內插字串</span><span class="sxs-lookup"><span data-stu-id="0323c-176">Customizable interpolated strings</span></span>
<span data-ttu-id="0323c-177">開發人員可以使用 `FormattableString`自訂內插字串的行為。</span><span class="sxs-lookup"><span data-stu-id="0323c-177">Developers are able to customize the behavior of interpolated strings with `FormattableString`.</span></span> <span data-ttu-id="0323c-178">這包含進入字串插值的資料：格式 `string` 和引數當做陣列。</span><span class="sxs-lookup"><span data-stu-id="0323c-178">This contains the data which goes into an interpolated string: the format `string` and the arguments as an array.</span></span> <span data-ttu-id="0323c-179">不過，這仍然具有「裝箱」和「引數」陣列配置，以及 `FormattableString` 的配置（這是 `abstract class`）。</span><span class="sxs-lookup"><span data-stu-id="0323c-179">This though still has the boxing and argument array allocation as well as the allocation for `FormattableString` (it's an `abstract class`).</span></span> <span data-ttu-id="0323c-180">因此，對 `string` 格式設定繁重的應用程式很少使用。</span><span class="sxs-lookup"><span data-stu-id="0323c-180">Hence it's of little use to applications which are allocation heavy in `string` formatting.</span></span>

<span data-ttu-id="0323c-181">為了有效率地插入字串格式，語言將會辨識新的類型： `System.ValueFormattableString`。</span><span class="sxs-lookup"><span data-stu-id="0323c-181">To make interpolated string formatting efficient the language will recognize a new type: `System.ValueFormattableString`.</span></span> <span data-ttu-id="0323c-182">所有字串插值都會將目標型別轉換成這個型別。</span><span class="sxs-lookup"><span data-stu-id="0323c-182">All interpolated strings will have a target type conversion to this type.</span></span> <span data-ttu-id="0323c-183">這會藉由將插入字串轉譯為呼叫中的方式來執行，`ValueFormattableString.Create` 完全如同今天 `FormattableString.Create` 所做的一樣。</span><span class="sxs-lookup"><span data-stu-id="0323c-183">This will be implemented by translating the interpolated string into the call `ValueFormattableString.Create` exactly as is done for `FormattableString.Create` today.</span></span> <span data-ttu-id="0323c-184">當您尋找最適合的 `ValueFormattableString.Create` 方法時，語言將支援本檔中所述的所有 `params` 選項。</span><span class="sxs-lookup"><span data-stu-id="0323c-184">The language will support all `params` options described in this document when looking for the most suitable `ValueFormattableString.Create` method.</span></span> 

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

<span data-ttu-id="0323c-185">當引數為字串插值時，多載解析規則將會變更為偏好 `string` 的 `ValueFormattableString`。</span><span class="sxs-lookup"><span data-stu-id="0323c-185">Overload resolution rules will be changed to prefer `ValueFormattableString` over `string` when the argument is an interpolated string.</span></span> <span data-ttu-id="0323c-186">這表示只有在 `string` 和 `ValueFormattableString`上有不同的多載才有價值。</span><span class="sxs-lookup"><span data-stu-id="0323c-186">This means it will be valuable to have overloads which differ only on `string` and `ValueFormattableString`.</span></span> <span data-ttu-id="0323c-187">現今具有 `FormattableString` 的這類多載並不重要，因為編譯器一律會偏好 `string` 版本（除非開發人員使用明確轉換）。</span><span class="sxs-lookup"><span data-stu-id="0323c-187">Such an overload today with `FormattableString` is not valuable as the compiler will always prefer the `string` version (unless the developer uses an explicit cast).</span></span> 

## <a name="open-issues"></a><span data-ttu-id="0323c-188">開啟問題</span><span class="sxs-lookup"><span data-stu-id="0323c-188">Open Issues</span></span>

### <a name="valueformattablestring-breaking-change"></a><span data-ttu-id="0323c-189">ValueFormattableString 重大變更</span><span class="sxs-lookup"><span data-stu-id="0323c-189">ValueFormattableString breaking change</span></span>
<span data-ttu-id="0323c-190">在透過 `string` 進行多載解析時，偏好 `ValueFormattableString` 的變更是一種重大變更。</span><span class="sxs-lookup"><span data-stu-id="0323c-190">The change to prefer `ValueFormattableString` during overload resolution over `string` is a breaking change.</span></span> <span data-ttu-id="0323c-191">開發人員可以立即定義名為 `ValueFormattableString` 的類型，並在具有 `string`的方法多載中使用它。</span><span class="sxs-lookup"><span data-stu-id="0323c-191">It is possible for a developer to have defined a type called `ValueFormattableString` today and use it in method overloads with `string`.</span></span> <span data-ttu-id="0323c-192">這項建議的變更會導致編譯器在執行這組功能之後，挑選不同的多載。</span><span class="sxs-lookup"><span data-stu-id="0323c-192">This proposed change would cause the compiler to pick a different overload once this set of features was implemented.</span></span> 

<span data-ttu-id="0323c-193">這種情況似乎相當低。</span><span class="sxs-lookup"><span data-stu-id="0323c-193">The possibility of this seems reasonably low.</span></span> <span data-ttu-id="0323c-194">此類型需要完整名稱 `System.ValueFormattableString`，而且必須具有名為 `Create`的 `static` 方法。</span><span class="sxs-lookup"><span data-stu-id="0323c-194">The type would need the full name `System.ValueFormattableString` and it would need to have `static` methods named `Create`.</span></span> <span data-ttu-id="0323c-195">假設開發人員強烈不建議在 `System` 命名空間中定義任何類型，這種中斷就好像是合理的危害。</span><span class="sxs-lookup"><span data-stu-id="0323c-195">Given that developers are strongly discouraged from defining any type in the `System` namespace this break seems like a reasonable compromise.</span></span>

### <a name="expanding-to-more-types"></a><span data-ttu-id="0323c-196">擴充至更多類型</span><span class="sxs-lookup"><span data-stu-id="0323c-196">Expanding to more types</span></span>
<span data-ttu-id="0323c-197">假設我們在此區域中，我們應該考慮將 `IList<T>`、`ICollection<T>` 和 `IReadOnlyList<T>` 新增至支援 `params` 的一組集合。</span><span class="sxs-lookup"><span data-stu-id="0323c-197">Given we're in the area we should consider adding `IList<T>`, `ICollection<T>` and `IReadOnlyList<T>` to the set of collections for which `params` is supported.</span></span> <span data-ttu-id="0323c-198">就實行的角度而言，這裡的其他工作將會花費較小的成本。</span><span class="sxs-lookup"><span data-stu-id="0323c-198">In terms of implementation it will cost a small amount over the other work here.</span></span>

<span data-ttu-id="0323c-199">不過，LDM 必須判斷這種語言的複雜情況是否值得。</span><span class="sxs-lookup"><span data-stu-id="0323c-199">LDM needs to decide if the complication to the language is worth it though.</span></span> <span data-ttu-id="0323c-200">新增 `IEnumerable<T>` 會移除非常特定的摩擦點。</span><span class="sxs-lookup"><span data-stu-id="0323c-200">The addition of `IEnumerable<T>` removes a very specific friction point.</span></span> <span data-ttu-id="0323c-201">若缺少此 `params` 解決方案，許多客戶會在呼叫 `params` 方法時，強制從 `IEnumerable<T>` 配置 `T[]`。</span><span class="sxs-lookup"><span data-stu-id="0323c-201">Lacking this `params` solution many customers were forced to allocate `T[]` from an `IEnumerable<T>` when calling a `params` method.</span></span> <span data-ttu-id="0323c-202">新增 `IEnumerable<T>` 會修正此問題。</span><span class="sxs-lookup"><span data-stu-id="0323c-202">The addition of `IEnumerable<T>` fixes this though.</span></span> <span data-ttu-id="0323c-203">這裡沒有其他介面修正的特定摩擦點。</span><span class="sxs-lookup"><span data-stu-id="0323c-203">There is no specific friction point that the other interfaces fix here.</span></span> 

## <a name="considerations"></a><span data-ttu-id="0323c-204">考量</span><span class="sxs-lookup"><span data-stu-id="0323c-204">Considerations</span></span>

### <a name="variant2-and-variant3"></a><span data-ttu-id="0323c-205">Variant2 和 Variant3</span><span class="sxs-lookup"><span data-stu-id="0323c-205">Variant2 and Variant3</span></span>
<span data-ttu-id="0323c-206">CoreFX 小組也有一組非配置的儲存類型，最多可有三個 `Variant` 引數。</span><span class="sxs-lookup"><span data-stu-id="0323c-206">The CoreFX team also has a non-allocating set of storage types for up to three `Variant` arguments.</span></span> <span data-ttu-id="0323c-207">這些是單一 `Variant`，`Variant2` 和 `Variant3`。</span><span class="sxs-lookup"><span data-stu-id="0323c-207">These are a single `Variant`, `Variant2` and `Variant3`.</span></span> <span data-ttu-id="0323c-208">所有的方法都有一組可用來取得配置的免費 `Span<Variant>`： `CreateSpan` 和 `KeepAlive`。</span><span class="sxs-lookup"><span data-stu-id="0323c-208">All have a pair of methods for getting an allocation free `Span<Variant>` off of them: `CreateSpan` and `KeepAlive`.</span></span> <span data-ttu-id="0323c-209">這表示對於最多三個引數的 `params Span<Variant>`，呼叫位置可以完全免費的配置。</span><span class="sxs-lookup"><span data-stu-id="0323c-209">This means for a `params Span<Variant>` of up to three arguments the call site can be entirely allocation free.</span></span>

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

<span data-ttu-id="0323c-210">`Go` 方法可以降低為下列各項：</span><span class="sxs-lookup"><span data-stu-id="0323c-210">The `Go` method can be lowered to the following:</span></span>

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

<span data-ttu-id="0323c-211">這只需要在提案上執行非常少的工作，就能在 `params Span<T>` 呼叫之間重複使用 `T[]`。</span><span class="sxs-lookup"><span data-stu-id="0323c-211">This requires very little work on top of the proposal to re-use `T[]` between `params Span<T>` calls.</span></span> <span data-ttu-id="0323c-212">編譯器已經需要管理每個呼叫的暫存，並在之後執行清理工作（即使在一種情況下，它只會將內部 temp 標示為可用）。</span><span class="sxs-lookup"><span data-stu-id="0323c-212">The compiler already needs to manage a temporary per call and do clean up work after (even if in one case it's just marking an internal temp as free).</span></span> 

<span data-ttu-id="0323c-213">注意：只有在桌面上才需要 `KeepAlive` 函數。</span><span class="sxs-lookup"><span data-stu-id="0323c-213">Note: the `KeepAlive` function is only necessary on desktop.</span></span> <span data-ttu-id="0323c-214">在 .NET Core 上，方法將無法使用，因此編譯器不會發出呼叫。</span><span class="sxs-lookup"><span data-stu-id="0323c-214">On .NET Core the method will not be available and hence the compiler won't emit a call to it.</span></span>

### <a name="clr-stack-allocation-helpers"></a><span data-ttu-id="0323c-215">CLR 堆疊配置協助程式</span><span class="sxs-lookup"><span data-stu-id="0323c-215">CLR stack allocation helpers</span></span>
<span data-ttu-id="0323c-216">CLR 只會提供連續記憶體之堆疊配置的[localloc](https://docs.microsoft.com/en-us/dotnet/api/system.reflection.emit.opcodes.localloc?redirectedfrom=MSDN&view=netframework-4.7.2) 。</span><span class="sxs-lookup"><span data-stu-id="0323c-216">The CLR only provides only [localloc](https://docs.microsoft.com/en-us/dotnet/api/system.reflection.emit.opcodes.localloc?redirectedfrom=MSDN&view=netframework-4.7.2) for stack allocation of contiguous memory.</span></span> <span data-ttu-id="0323c-217">此指示的限制在於僅適用于 `unmanaged` 類型。</span><span class="sxs-lookup"><span data-stu-id="0323c-217">This instruction is limited in that it only works for `unmanaged` types.</span></span> <span data-ttu-id="0323c-218">這表示它無法做為可有效率地為 `params 
 Span<T>`配置支援儲存體的通用解決方案。</span><span class="sxs-lookup"><span data-stu-id="0323c-218">This means it can't be used as a universal solution for efficiently allocating the backing storage for `params 
 Span<T>`.</span></span> 

<span data-ttu-id="0323c-219">不過，這項限制並不是部分基本限制，而是更多的歷程記錄。</span><span class="sxs-lookup"><span data-stu-id="0323c-219">This limitation is not some fundamental restriction though but instead more an artifact of history.</span></span> <span data-ttu-id="0323c-220">CLR 可以加入宣告新的 op 代碼/內建函式，以提供通用堆疊配置。</span><span class="sxs-lookup"><span data-stu-id="0323c-220">The CLR could choose to add new op codes / intrinsics which provide universal stack allocation.</span></span> <span data-ttu-id="0323c-221">然後，這些可用來配置最 `params Span<T>` 呼叫的支援儲存體。</span><span class="sxs-lookup"><span data-stu-id="0323c-221">These could then be used to allocate the backing storage for most `params Span<T>` calls.</span></span>

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

<span data-ttu-id="0323c-222">`Go` 方法可以降低為下列各項：</span><span class="sxs-lookup"><span data-stu-id="0323c-222">The `Go` method can be lowered to the following:</span></span>

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

<span data-ttu-id="0323c-223">雖然這種方法是非常堆積的，但它會造成額外的堆疊使用。</span><span class="sxs-lookup"><span data-stu-id="0323c-223">While this approach is very heap efficient it does cause extra stack usage.</span></span> <span data-ttu-id="0323c-224">在具有深度堆疊和許多 `params` 使用方式的演算法中，這可能會導致 `StackOverflowException` 產生簡單 `T[]` 配置成功的位置。</span><span class="sxs-lookup"><span data-stu-id="0323c-224">In an algorithm which has a deep stack and lots of `params` usage it's possible this could cause a `StackOverflowException` to be generated where a simple `T[]` allocation would succeed.</span></span> 

<span data-ttu-id="0323c-225">可惜C#的是，不會針對方法間分析的類型進行設定，它可以讓您瞭解呼叫是否應該使用 `params`的堆疊或堆積配置。</span><span class="sxs-lookup"><span data-stu-id="0323c-225">Unfortunately C# is not set up for the type of inter-method analysis where it could make an educated determination of whether or not call should use stack or heap allocation of `params`.</span></span> <span data-ttu-id="0323c-226">它只能考慮自己的每個呼叫。</span><span class="sxs-lookup"><span data-stu-id="0323c-226">It can only really consider each call on its own.</span></span>

<span data-ttu-id="0323c-227">CLR 是在執行時間進行這類判斷的最佳設定。</span><span class="sxs-lookup"><span data-stu-id="0323c-227">The CLR is best setup for making this type of determination at runtime.</span></span> <span data-ttu-id="0323c-228">因此，我們可能會讓執行時間為通用堆疊配置提供兩種方法：</span><span class="sxs-lookup"><span data-stu-id="0323c-228">Hence we'd likely have the runtime provide two methods for universal stack allocation:</span></span>

1. <span data-ttu-id="0323c-229">`Span<T> StackAlloc<T>(int length)`：這具有 `localloc` 的行為和限制，但它可以在任何類型 `T`上使用。</span><span class="sxs-lookup"><span data-stu-id="0323c-229">`Span<T> StackAlloc<T>(int length)`: this has the same behaviors and limitations of `localloc` except it can work on any type `T`.</span></span> 
1. <span data-ttu-id="0323c-230">`Span<T> MaybeStackAlloc<T>(int length)`：此執行時間可以選擇執行堆疊或堆積配置來執行此作業。</span><span class="sxs-lookup"><span data-stu-id="0323c-230">`Span<T> MaybeStackAlloc<T>(int length)`: this runtime can choose to implement this by doing a stack or heap allocation.</span></span> <span data-ttu-id="0323c-231">然後，執行時間就可以使用它所呼叫的執行內容來決定如何配置 `Span<T>`。</span><span class="sxs-lookup"><span data-stu-id="0323c-231">The runtime can then use the execution context in which it's called to determine how the `Span<T>` is allocated.</span></span> <span data-ttu-id="0323c-232">不過，呼叫端一律會將它視為已配置堆疊。</span><span class="sxs-lookup"><span data-stu-id="0323c-232">The caller though will always treat it as if it were stack allocated.</span></span>

<span data-ttu-id="0323c-233">對於非常簡單的案例，例如一到兩個自C#變數，編譯器一律會使用 `StackAlloc<T>` variant。</span><span class="sxs-lookup"><span data-stu-id="0323c-233">For very simple cases, like one to two arguments, the C# compiler could always use `StackAlloc<T>` variant.</span></span> <span data-ttu-id="0323c-234">在大部分情況下，這不太可能會對堆疊耗盡造成明顯的影響。</span><span class="sxs-lookup"><span data-stu-id="0323c-234">This is unlikely to significantly contribute to stack exhaustion in most cases.</span></span> <span data-ttu-id="0323c-235">針對其他情況，編譯器可以選擇改用 `MaybeStackAlloc<T>`，並讓執行時間進行呼叫。</span><span class="sxs-lookup"><span data-stu-id="0323c-235">For other cases the compiler could choose to use `MaybeStackAlloc<T>` instead and let the runtime make the call.</span></span>

<span data-ttu-id="0323c-236">我們的選擇可能需要更深入的調查和檢查真實世界的應用程式。</span><span class="sxs-lookup"><span data-stu-id="0323c-236">How we choose will likely require a deeper investigation and examination of real world apps.</span></span> <span data-ttu-id="0323c-237">但是，如果有這些新的內建函式，則會提供這種類型的彈性。</span><span class="sxs-lookup"><span data-stu-id="0323c-237">But if these new intrinsics are available then it will give us this type of flexibility.</span></span>

### <a name="why-not-varargs"></a><span data-ttu-id="0323c-238">為什麼不是 varargs？</span><span class="sxs-lookup"><span data-stu-id="0323c-238">Why not varargs?</span></span> 
<span data-ttu-id="0323c-239">在此將現有的[varargs](https://docs.microsoft.com/en-us/cpp/windows/variable-argument-lists-dot-dot-dot-cpp-cli?view=vs-2017)功能視為可行的解決方案。</span><span class="sxs-lookup"><span data-stu-id="0323c-239">The existing [varargs](https://docs.microsoft.com/en-us/cpp/windows/variable-argument-lists-dot-dot-dot-cpp-cli?view=vs-2017) feature was considered here as a possible solution.</span></span> <span data-ttu-id="0323c-240">不過，這項功能主要是C++針對/cli 案例，並在其他案例中具有已知的漏洞。</span><span class="sxs-lookup"><span data-stu-id="0323c-240">This feature though is meant primarily for C++/CLI scenarios and has known holes for other scenarios.</span></span> <span data-ttu-id="0323c-241">此外，將此項移植到 Unix 也會有相當大的成本。</span><span class="sxs-lookup"><span data-stu-id="0323c-241">Additionally there is significant cost in porting this to Unix.</span></span> <span data-ttu-id="0323c-242">因此，它不會被視為可行的解決方案。</span><span class="sxs-lookup"><span data-stu-id="0323c-242">Hence it wasn't seen as a viable solution.</span></span>

## <a name="related-issues"></a><span data-ttu-id="0323c-243">相關問題</span><span class="sxs-lookup"><span data-stu-id="0323c-243">Related Issues</span></span>
<span data-ttu-id="0323c-244">此規格與下列問題相關：</span><span class="sxs-lookup"><span data-stu-id="0323c-244">This spec is related to the following issues:</span></span> 

- https://github.com/dotnet/csharplang/issues/1757
- https://github.com/dotnet/csharplang/issues/179
- https://github.com/dotnet/corefxlab/pull/2595

