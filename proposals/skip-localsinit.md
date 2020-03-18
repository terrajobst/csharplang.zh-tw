---
ms.openlocfilehash: 52b43abd2d8fb56088a68c7169289a63c43ce96f
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484486"
---
# <a name="suppress-emitting-of-localsinit-flag"></a><span data-ttu-id="d4d78-101">隱藏發出 `localsinit` 旗標。</span><span class="sxs-lookup"><span data-stu-id="d4d78-101">Suppress emitting of `localsinit` flag.</span></span>

* <span data-ttu-id="d4d78-102">[x] 提議</span><span class="sxs-lookup"><span data-stu-id="d4d78-102">[x] Proposed</span></span>
* <span data-ttu-id="d4d78-103">[] 原型：未啟動</span><span class="sxs-lookup"><span data-stu-id="d4d78-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="d4d78-104">[] 執行：未啟動</span><span class="sxs-lookup"><span data-stu-id="d4d78-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="d4d78-105">[] 規格：未啟動</span><span class="sxs-lookup"><span data-stu-id="d4d78-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="d4d78-106">摘要</span><span class="sxs-lookup"><span data-stu-id="d4d78-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="d4d78-107">允許透過 `SkipLocalsInitAttribute` 屬性隱藏發出 `localsinit` 旗標。</span><span class="sxs-lookup"><span data-stu-id="d4d78-107">Allow suppressing emit of `localsinit` flag via `SkipLocalsInitAttribute` attribute.</span></span> 

## <a name="motivation"></a><span data-ttu-id="d4d78-108">動機</span><span class="sxs-lookup"><span data-stu-id="d4d78-108">Motivation</span></span>
[motivation]: #motivation


### <a name="background"></a><span data-ttu-id="d4d78-109">背景</span><span class="sxs-lookup"><span data-stu-id="d4d78-109">Background</span></span>
<span data-ttu-id="d4d78-110">針對不包含參考的每個 CLR 規格區域變數，VM/JIT 不會將其初始化為特定值。</span><span class="sxs-lookup"><span data-stu-id="d4d78-110">Per CLR spec local variables that do not contain references are not initialized to a particular value by the VM/JIT.</span></span> <span data-ttu-id="d4d78-111">從這類沒有初始化的變數讀取是型別安全的，否則行為是未定義的，而且是特定的執行。</span><span class="sxs-lookup"><span data-stu-id="d4d78-111">Reading from such variables without initialization is type-safe, but otherwise the behavior is undefined and implementation specific.</span></span> <span data-ttu-id="d4d78-112">通常未初始化的區域變數會包含目前堆疊框架所佔用的記憶體中所剩的任何值。</span><span class="sxs-lookup"><span data-stu-id="d4d78-112">Typically uninitialized locals contain whatever values were left in the memory that is now occupied by the stack frame.</span></span> <span data-ttu-id="d4d78-113">這可能會導致不具決定性的行為，並難以重現 bug。</span><span class="sxs-lookup"><span data-stu-id="d4d78-113">That could lead to nondeterministic behavior and hard to reproduce bugs.</span></span> 

<span data-ttu-id="d4d78-114">有兩種方式可以「指派」本機變數：</span><span class="sxs-lookup"><span data-stu-id="d4d78-114">There are two ways to "assign" a local variable:</span></span> 
- <span data-ttu-id="d4d78-115">藉由儲存值或</span><span class="sxs-lookup"><span data-stu-id="d4d78-115">by storing a value or</span></span> 
- <span data-ttu-id="d4d78-116">藉由指定 `localsinit` 旗標，將所有配置的本機記憶體集區所配置的專案，都設為零初始化注意：這包括本機變數和 `stackalloc` 資料。</span><span class="sxs-lookup"><span data-stu-id="d4d78-116">by specifying `localsinit` flag which forces everything that is allocated form the local memory pool to be zero-initialized NOTE: this includes both local variables and `stackalloc` data.</span></span>    

<span data-ttu-id="d4d78-117">不建議使用未初始化的資料，而且不允許在可驗證的程式碼中使用。</span><span class="sxs-lookup"><span data-stu-id="d4d78-117">Use of uninitialized data is discouraged and is not allowed in verifiable code.</span></span> <span data-ttu-id="d4d78-118">雖然您可以透過流程分析的方式來證明它，但允許驗證演算法非常保守，而且只需要設定 `localsinit`。</span><span class="sxs-lookup"><span data-stu-id="d4d78-118">While it might be possible to prove that by the means of flow analysis, it is permitted for the verification algorithm to be conservative and simply require that `localsinit` is set.</span></span>

<span data-ttu-id="d4d78-119">在C#過去，編譯器會在宣告區域變數的所有方法上發出 `localsinit` 旗標。</span><span class="sxs-lookup"><span data-stu-id="d4d78-119">Historically C# compiler emits `localsinit` flag on all methods that declare locals.</span></span>

<span data-ttu-id="d4d78-120">雖然C#會採用明確的指派分析，而這比 CLR 規格所需的更C#嚴格（也需要考慮區域變數的範圍），但並不保證產生的程式碼會正式驗證：</span><span class="sxs-lookup"><span data-stu-id="d4d78-120">While C# employs definite-assignment analysis which is more strict than what CLR spec would require (C# also needs to consider scoping of locals), it is not strictly guaranteed that the resulting code would be formally verifiable:</span></span>
- <span data-ttu-id="d4d78-121">CLR 和C#規則可能不會同意是否將本機當做 `out` 引數傳遞 `use`。</span><span class="sxs-lookup"><span data-stu-id="d4d78-121">CLR and C# rules may not agree on whether passing a local as `out` argument is a `use`.</span></span>
- <span data-ttu-id="d4d78-122">CLR 和C#規則在已知條件（常數傳播）時，可能不會同意條件式分支的處理。</span><span class="sxs-lookup"><span data-stu-id="d4d78-122">CLR and C# rules may not agree on treatment of conditional branches when conditions are known (constant propagation).</span></span>
- <span data-ttu-id="d4d78-123">CLR 也可能只需要 `localinits`，因為這是允許的。</span><span class="sxs-lookup"><span data-stu-id="d4d78-123">CLR could as well simply require `localinits`, since that is permitted.</span></span>  

### <a name="problem"></a><span data-ttu-id="d4d78-124">問題</span><span class="sxs-lookup"><span data-stu-id="d4d78-124">Problem</span></span>
<span data-ttu-id="d4d78-125">在高效能應用程式中，強制零初始化的成本可能會很明顯。</span><span class="sxs-lookup"><span data-stu-id="d4d78-125">In high-performance application the cost of forced zero-initialization could be noticeable.</span></span> <span data-ttu-id="d4d78-126">使用 `stackalloc` 時，特別明顯。</span><span class="sxs-lookup"><span data-stu-id="d4d78-126">It is particularly noticeable when `stackalloc` is used.</span></span>

<span data-ttu-id="d4d78-127">在某些情況下，JIT 可以在後續的指派中「終止」這類初始化時，省略個別區域變數的初始零初始化。</span><span class="sxs-lookup"><span data-stu-id="d4d78-127">In some cases JIT can elide initial zero-initialization of individual locals when such initialization is "killed" by subsequent assignments.</span></span> <span data-ttu-id="d4d78-128">並非所有 JITs 都這麼做，因此這類優化有其限制。</span><span class="sxs-lookup"><span data-stu-id="d4d78-128">Not all JITs do this and such optimization has limits.</span></span> <span data-ttu-id="d4d78-129">它不會協助 `stackalloc`。</span><span class="sxs-lookup"><span data-stu-id="d4d78-129">It does not help with `stackalloc`.</span></span>

<span data-ttu-id="d4d78-130">為了說明問題是 real，有一個已知的 bug，其中不包含任何 `IL` 區域變數的方法不會有 `localsinit` 旗標。</span><span class="sxs-lookup"><span data-stu-id="d4d78-130">To illustrate that the problem is real - there is a known bug where a method not containing any `IL` locals would not have `localsinit` flag.</span></span> <span data-ttu-id="d4d78-131">使用者已藉由將 `stackalloc` 放入這類方法中，故意避免初始化成本，而使 bug 遭到惡意探索。</span><span class="sxs-lookup"><span data-stu-id="d4d78-131">The bug is already being exploited by users by putting `stackalloc` into such methods - intentionally to avoid initialization costs.</span></span> <span data-ttu-id="d4d78-132">這是儘管沒有 `IL` 的區域變數是不穩定的計量，而且可能會根據 codegen 策略中的變更而有所不同。</span><span class="sxs-lookup"><span data-stu-id="d4d78-132">That is despite the fact that absence of `IL` locals is an unstable metric and may vary depending on changes in codegen strategy.</span></span> <span data-ttu-id="d4d78-133">Bug 應該是固定的，而且使用者應該會得到更有記載且更可靠的方式來隱藏旗標。</span><span class="sxs-lookup"><span data-stu-id="d4d78-133">The bug should be fixed and users should get a more documented and reliable way of suppressing the flag.</span></span> 

## <a name="detailed-design"></a><span data-ttu-id="d4d78-134">詳細設計</span><span class="sxs-lookup"><span data-stu-id="d4d78-134">Detailed design</span></span>

<span data-ttu-id="d4d78-135">允許將 `System.Runtime.CompilerServices.SkipLocalsInitAttribute` 指定為指示編譯器不要發出 `localsinit` 旗標的方式。</span><span class="sxs-lookup"><span data-stu-id="d4d78-135">Allow specifying `System.Runtime.CompilerServices.SkipLocalsInitAttribute` as a way to tell the compiler to not emit `localsinit` flag.</span></span>
 
<span data-ttu-id="d4d78-136">最後的結果就是，不能由 JIT 以零初始化區域變數，這在大部分情況下會 unobservable C#。</span><span class="sxs-lookup"><span data-stu-id="d4d78-136">The end result of this will be that the locals may not be zero-initialized by the JIT, which is in most cases unobservable in C#.</span></span>  
<span data-ttu-id="d4d78-137">除了該 `stackalloc` 資料也不會以零初始化。</span><span class="sxs-lookup"><span data-stu-id="d4d78-137">In addition to that `stackalloc` data will not be zero-initialized.</span></span> <span data-ttu-id="d4d78-138">這絕對是可觀察的，但也是最能激發的案例。</span><span class="sxs-lookup"><span data-stu-id="d4d78-138">That is definitely observable, but also is the most motivating scenario.</span></span>

<span data-ttu-id="d4d78-139">允許和可辨識的屬性目標為： `Method`、`Property`、`Module`、`Class`、`Struct`、`Interface`、`Constructor`。</span><span class="sxs-lookup"><span data-stu-id="d4d78-139">Permitted and recognized attribute targets are: `Method`, `Property`, `Module`, `Class`, `Struct`, `Interface`, `Constructor`.</span></span> <span data-ttu-id="d4d78-140">不過，編譯器不需要以列出的目標來定義屬性，也不會在意定義屬性的元件。</span><span class="sxs-lookup"><span data-stu-id="d4d78-140">However compiler will not require that attribute is defined with the listed targets nor it will care in which assembly the attribute is defined.</span></span> 

<span data-ttu-id="d4d78-141">在容器上指定屬性時（`class`，`module`，包含嵌套方法的方法，...）時，旗標會影響容器中包含的所有方法。</span><span class="sxs-lookup"><span data-stu-id="d4d78-141">When attribute is specified on a container (`class`, `module`, containing method for a nested method, ...), the flag affects all methods contained within the container.</span></span>

<span data-ttu-id="d4d78-142">合成方法會從邏輯容器/擁有者「繼承」旗標。</span><span class="sxs-lookup"><span data-stu-id="d4d78-142">Synthesized methods "inherit" the flag from the logical container/owner.</span></span> 

<span data-ttu-id="d4d78-143">旗標只會影響實際方法主體的 codegen 策略。</span><span class="sxs-lookup"><span data-stu-id="d4d78-143">The flag affects only codegen strategy for actual method bodies.</span></span> <span data-ttu-id="d4d78-144">亦即.</span><span class="sxs-lookup"><span data-stu-id="d4d78-144">I.E.</span></span> <span data-ttu-id="d4d78-145">旗標不會影響抽象方法，而且不會傳播至覆寫/執行方法。</span><span class="sxs-lookup"><span data-stu-id="d4d78-145">the flag has no effect on abstract methods and is not propagated to overriding/implementing methods.</span></span>

<span data-ttu-id="d4d78-146">這是明確的 **_編譯器功能_** ，而 **_不是語言功能_** 。</span><span class="sxs-lookup"><span data-stu-id="d4d78-146">This is explicitly a **_compiler feature_** and **_not a language feature_**.</span></span>  
<span data-ttu-id="d4d78-147">類似于編譯器命令列參數：此功能可控制特定 codegen 策略的執行詳細資料，而且不需要由C#規格所要求。</span><span class="sxs-lookup"><span data-stu-id="d4d78-147">Similarly to compiler command line switches the feature controls implementation details of a particular codegen strategy and does not need to be required by the C# spec.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="d4d78-148">缺點</span><span class="sxs-lookup"><span data-stu-id="d4d78-148">Drawbacks</span></span>
[drawbacks]: #drawbacks

- <span data-ttu-id="d4d78-149">舊的/其他編譯器可能不會接受屬性。</span><span class="sxs-lookup"><span data-stu-id="d4d78-149">Old/other compilers may not honor the attribute.</span></span>
<span data-ttu-id="d4d78-150">忽略屬性為相容的行為。</span><span class="sxs-lookup"><span data-stu-id="d4d78-150">Ignoring the attribute is compatible behavior.</span></span> <span data-ttu-id="d4d78-151">只有可能會造成些許效能的衝擊。</span><span class="sxs-lookup"><span data-stu-id="d4d78-151">Only may result in a slight perf hit.</span></span>

- <span data-ttu-id="d4d78-152">沒有 `localinits` 旗標的程式碼可能會觸發驗證失敗。</span><span class="sxs-lookup"><span data-stu-id="d4d78-152">The code without `localinits` flag may trigger verification failures.</span></span>
<span data-ttu-id="d4d78-153">要求這項功能的使用者通常會以可驗證性不在乎。</span><span class="sxs-lookup"><span data-stu-id="d4d78-153">Users that ask for this feature are generally unconcerned with verifiability.</span></span> 
 
- <span data-ttu-id="d4d78-154">在比個別方法更高的層級套用屬性會有非使用中的效果，這在使用 `stackalloc` 時是可觀察的。</span><span class="sxs-lookup"><span data-stu-id="d4d78-154">Applying the attribute at higher levels than an individual method has nonlocal effect, which is observable when `stackalloc` is used.</span></span> <span data-ttu-id="d4d78-155">不過，這是最常要求的案例。</span><span class="sxs-lookup"><span data-stu-id="d4d78-155">Yet, this is the most requested scenario.</span></span>

## <a name="alternatives"></a><span data-ttu-id="d4d78-156">替代方案</span><span class="sxs-lookup"><span data-stu-id="d4d78-156">Alternatives</span></span>
[alternatives]: #alternatives

- <span data-ttu-id="d4d78-157">`unsafe` 內容中宣告方法時，請省略 `localinits` 旗標。</span><span class="sxs-lookup"><span data-stu-id="d4d78-157">omit `localinits` flag when method is declared in `unsafe` context.</span></span> <span data-ttu-id="d4d78-158">這可能會導致在 `stackalloc`的情況下，不具決定性且危險的行為變更為不確定性。</span><span class="sxs-lookup"><span data-stu-id="d4d78-158">That could cause silent and dangerous behavior change from deterministic to nondeterministic in a case of `stackalloc`.</span></span>

- <span data-ttu-id="d4d78-159">一律省略 `localinits` 旗標。</span><span class="sxs-lookup"><span data-stu-id="d4d78-159">omit `localinits` flag always.</span></span>
<span data-ttu-id="d4d78-160">比上述更糟。</span><span class="sxs-lookup"><span data-stu-id="d4d78-160">Even worse than above.</span></span>

- <span data-ttu-id="d4d78-161">除非在方法主體中使用 `stackalloc`，否則請省略 `localinits` 旗標。</span><span class="sxs-lookup"><span data-stu-id="d4d78-161">omit `localinits` flag unless `stackalloc` is used in the method body.</span></span>
<span data-ttu-id="d4d78-162">不會處理最常要求的案例，也可能會使程式碼無法驗證，而不會有任何選項可還原回。</span><span class="sxs-lookup"><span data-stu-id="d4d78-162">Does not address the most requested scenario and may turn code unverifiable with no option to revert that back.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="d4d78-163">未解決的問題</span><span class="sxs-lookup"><span data-stu-id="d4d78-163">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="d4d78-164">屬性是否實際發出至中繼資料？</span><span class="sxs-lookup"><span data-stu-id="d4d78-164">Should the attribute be actually emitted to metadata?</span></span> 

## <a name="design-meetings"></a><span data-ttu-id="d4d78-165">設計會議</span><span class="sxs-lookup"><span data-stu-id="d4d78-165">Design meetings</span></span>

<span data-ttu-id="d4d78-166">還沒有。</span><span class="sxs-lookup"><span data-stu-id="d4d78-166">None yet.</span></span> 