---
ms.openlocfilehash: f238a711e710bbac7f5b7400fa938bd85dec00c6
ms.sourcegitcommit: 5278336b61519956240a6f7d83bcb4322019e032
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/10/2020
ms.locfileid: "79485249"
---
# <a name="support-for--and--on-tuple-types"></a><span data-ttu-id="6b8e9-101">支援元組類型上的 = = 和！ =</span><span class="sxs-lookup"><span data-stu-id="6b8e9-101">Support for == and != on tuple types</span></span>

<span data-ttu-id="6b8e9-102">允許運算式 `t1 == t2` 其中 `t1` 和 `t2` 是相同基數的元組或可為 null 的元組類型，並大致評估為 `temp1.Item1 == temp2.Item1 && temp1.Item2 == temp2.Item2` （假設 `var temp1 = t1; var temp2 = t2;`）。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-102">Allow expressions `t1 == t2` where `t1` and `t2` are tuple or nullable tuple types of same cardinality, and evaluate them roughly as `temp1.Item1 == temp2.Item1 && temp1.Item2 == temp2.Item2` (assuming `var temp1 = t1; var temp2 = t2;`).</span></span>

<span data-ttu-id="6b8e9-103">相反地，它會允許 `t1 != t2`，並將其評估為 `temp1.Item1 != temp2.Item1 || temp1.Item2 != temp2.Item2`。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-103">Conversely it would allow `t1 != t2` and evaluate it as `temp1.Item1 != temp2.Item1 || temp1.Item2 != temp2.Item2`.</span></span>

<span data-ttu-id="6b8e9-104">在可為 null 的情況下，會使用 `temp1.HasValue` 和 `temp2.HasValue` 的額外檢查。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-104">In the nullable case, additional checks for `temp1.HasValue` and `temp2.HasValue` are used.</span></span> <span data-ttu-id="6b8e9-105">例如，`nullableT1 == nullableT2` 會評估為 `temp1.HasValue == temp2.HasValue ? (temp1.HasValue ? ... : true) : false`。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-105">For instance, `nullableT1 == nullableT2` evaluates as `temp1.HasValue == temp2.HasValue ? (temp1.HasValue ? ... : true) : false`.</span></span>

<span data-ttu-id="6b8e9-106">當元素的比較傳回非 bool 的結果時（例如，當使用非 bool 使用者定義的 `operator ==` 或 `operator !=`，或在動態比較中）時，該結果將會轉換成 `bool`，或透過 `operator true` 或 `operator false` 執行以取得 `bool`。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-106">When an element-wise comparison returns a non-bool result (for instance, when a non-bool user-defined `operator ==` or `operator !=` is used, or in a dynamic comparison), then that result will be either converted to `bool` or run through `operator true` or `operator false` to get a `bool`.</span></span> <span data-ttu-id="6b8e9-107">元組比較一律會最後傳回 `bool`。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-107">The tuple comparison always ends up returning a `bool`.</span></span>

<span data-ttu-id="6b8e9-108">C# 7.2 之後，這類程式碼就會產生錯誤（`error CS0019: Operator '==' cannot be applied to operands of type '(...)' and '(...)'`），除非有使用者定義的 `operator==`。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-108">As of C# 7.2, such code produces an error (`error CS0019: Operator '==' cannot be applied to operands of type '(...)' and '(...)'`), unless there is a user-defined `operator==`.</span></span>

## <a name="details"></a><span data-ttu-id="6b8e9-109">詳細資料</span><span class="sxs-lookup"><span data-stu-id="6b8e9-109">Details</span></span>

<span data-ttu-id="6b8e9-110">系結 `==` （或 `!=`）運算子時，現有的規則包括：（1）動態案例、（2）多載解析，以及（3）失敗。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-110">When binding the `==` (or `!=`) operator, the existing rules are: (1) dynamic case, (2) overload resolution, and (3) fail.</span></span>
<span data-ttu-id="6b8e9-111">此提議會在（1）和（2）之間加入一個元組案例：如果比較運算子的兩個運算元都是元組（具有元組類型或為元組常值）且具有相符的基數，則比較會執行元素取向。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-111">This proposal adds a tuple case between (1) and (2): if both operands of a comparison operator are tuples (have tuple types or are tuple literals) and have matching cardinality, then the comparison is performed element-wise.</span></span> <span data-ttu-id="6b8e9-112">這個元組相等也會提升至可為 null 的元組。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-112">This tuple equality is also lifted onto nullable tuples.</span></span>

<span data-ttu-id="6b8e9-113">兩個運算元（如果是元組常值，則為其元素）會依序從左至右進行評估。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-113">Both operands (and, in the case of tuple literals, their elements) are evaluated in order from left to right.</span></span> <span data-ttu-id="6b8e9-114">接著會使用每一對元素做為運算元，以遞迴方式系結運算子 `==` （或 `!=`）。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-114">Each pair of elements is then used as operands to bind the operator `==` (or `!=`), recursively.</span></span> <span data-ttu-id="6b8e9-115">任何具有編譯時間類型的元素 `dynamic` 都會造成錯誤。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-115">Any elements with compile-time type `dynamic` cause an error.</span></span> <span data-ttu-id="6b8e9-116">這些元素的比較結果會當做條件式和（或）運算子鏈中的運算元使用。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-116">The results of those element-wise comparisons are used as operands in a chain of conditional AND (or OR) operators.</span></span>

<span data-ttu-id="6b8e9-117">例如，在 `(int, (int, int)) t1, t2;`的內容中，`t1 == (1, (2, 3))` 會評估為 `temp1.Item1 == temp2.Item1 && temp1.Item2.Item1 == temp2.Item2.Item1 && temp2.Item2.Item2 == temp2.Item2.Item2`。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-117">For instance, in the context of `(int, (int, int)) t1, t2;`, `t1 == (1, (2, 3))` would evaluate as `temp1.Item1 == temp2.Item1 && temp1.Item2.Item1 == temp2.Item2.Item1 && temp2.Item2.Item2 == temp2.Item2.Item2`.</span></span>

<span data-ttu-id="6b8e9-118">當使用元組常值做為運算元（在任一端）時，它會接收以元素取向轉換所形成的已轉換的元組類型，這會在系結運算子 `==` （或 `!=`）元素時引進。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-118">When a tuple literal is used as operand (on either side), it receives a converted tuple type formed by the element-wise conversions which are introduced when binding the operator `==` (or `!=`) element-wise.</span></span> 

<span data-ttu-id="6b8e9-119">例如，在 `(1L, 2, "hello") == (1, 2L, null)`中，兩個元組常值的轉換類型為 `(long, long, string)`，而第二個常值沒有自然類型。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-119">For instance, in `(1L, 2, "hello") == (1, 2L, null)`, the converted type for both tuple literals is `(long, long, string)` and the second literal has no natural type.</span></span>


### <a name="deconstruction-and-conversions-to-tuple"></a><span data-ttu-id="6b8e9-120">元組的解構和轉換</span><span class="sxs-lookup"><span data-stu-id="6b8e9-120">Deconstruction and conversions to tuple</span></span>
<span data-ttu-id="6b8e9-121">在 `(a, b) == x`中，`x` 可以解構至兩個元素的事實並不扮演角色。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-121">In `(a, b) == x`, the fact that `x` can deconstruct into two elements does not play a role.</span></span> <span data-ttu-id="6b8e9-122">這可能會在未來的提案中，但它會提出有關 `x == y` 的問題（這是簡單的比較或元素的比較，如果是使用什麼基數？）。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-122">That could conceivably be in a future proposal, although it would raise questions about `x == y` (is this a simple comparison or an element-wise comparison, and if so using what cardinality?).</span></span>
<span data-ttu-id="6b8e9-123">同樣地，對元組的轉換也不扮演任何角色。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-123">Similarly, conversions to tuple play no role.</span></span>

### <a name="tuple-element-names"></a><span data-ttu-id="6b8e9-124">元組元素名稱</span><span class="sxs-lookup"><span data-stu-id="6b8e9-124">Tuple element names</span></span>

<span data-ttu-id="6b8e9-125">轉換元組常值時，我們會在常值中提供明確的元組元素名稱時發出警告，但不符合目標元組元素名稱。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-125">When converting a tuple literal, we warn when an explicit tuple element name was provided in the literal, but it doesn't match the target tuple element name.</span></span>
<span data-ttu-id="6b8e9-126">我們在元組比較中使用相同的規則，因此假設 `(int a, int b) t` 我們會在 `t == (c, d: 0)`中 `d` 警告。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-126">We use the same rule in tuple comparison, so that assuming `(int a, int b) t` we warn on `d` in `t == (c, d: 0)`.</span></span>

### <a name="non-bool-element-wise-comparison-results"></a><span data-ttu-id="6b8e9-127">非 bool 元素的比較結果</span><span class="sxs-lookup"><span data-stu-id="6b8e9-127">Non-bool element-wise comparison results</span></span>

<span data-ttu-id="6b8e9-128">如果元素的比較在元組相等中是動態的，我們會使用運算子 `false` 的動態調用，並否定以取得 `bool`，並繼續進行進一步的元素比較。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-128">If an element-wise comparison is dynamic in a tuple equality, we use a dynamic invocation of the operator `false` and negate that to get a `bool` and continue with further element-wise comparisons.</span></span> 

<span data-ttu-id="6b8e9-129">如果元素的比較會傳回元組相等中的其他非 bool 類型，則有兩種情況：</span><span class="sxs-lookup"><span data-stu-id="6b8e9-129">If an element-wise comparison returns some other non-bool type in a tuple equality, there are two cases:</span></span>
- <span data-ttu-id="6b8e9-130">如果非 bool 類型轉換成 `bool`，我們會套用該轉換。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-130">if the non-bool type converts to `bool`, we apply that conversion,</span></span>
- <span data-ttu-id="6b8e9-131">如果沒有這種轉換，但型別具有運算子 `false`，我們將使用它，並將結果否定。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-131">if there is no such conversion, but the type has an operator `false`, we'll use that and negate the result.</span></span>

<span data-ttu-id="6b8e9-132">在元組不等比較中，會套用相同的規則，但我們會使用運算子 `true` （不含否定），而不是運算子 `false`。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-132">In a tuple inequality, the same rules apply except that we'll use the operator `true` (without negation) instead of the operator `false`.</span></span>

<span data-ttu-id="6b8e9-133">這些規則類似于在 `if` 語句中使用非 bool 類型的規則，以及一些其他現有的內容。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-133">Those rules are similar to the rules involved for using a non-bool type in an `if` statement and some other existing contexts.</span></span>

## <a name="evaluation-order-and-special-cases"></a><span data-ttu-id="6b8e9-134">評估順序和特殊案例</span><span class="sxs-lookup"><span data-stu-id="6b8e9-134">Evaluation order and special cases</span></span>
<span data-ttu-id="6b8e9-135">左側值會先評估，然後再評估右邊的值，然後從左至右（包括轉換，以及根據條件式和/或運算子的現有規則，儘早結束）進行元素的比較。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-135">The left-hand-side value is evaluated first, then the right-hand-side value, then the element-wise comparisons from left to right (including conversions, and with early exit based on existing rules for conditional AND/OR operators).</span></span>

<span data-ttu-id="6b8e9-136">例如，如果從類型 `A` 轉換成類型 `B` 和方法 `(A, A) GetTuple()`，則評估 `(new A(1), (new B(2), new B(3))) == (new B(4), GetTuple())` 表示：</span><span class="sxs-lookup"><span data-stu-id="6b8e9-136">For instance, if there is a conversion from type `A` to type `B` and a method `(A, A) GetTuple()`, evaluating `(new A(1), (new B(2), new B(3))) == (new B(4), GetTuple())` means:</span></span>
- `new A(1)`
- `new B(2)`
- `new B(3)`
- `new B(4)`
- `GetTuple()`
- <span data-ttu-id="6b8e9-137">接著會評估元素取向的轉換和比較和條件式邏輯（將 `new A(1)` 轉換為類型 `B`，然後與 `new B(4)`進行比較，依此類推）。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-137">then the element-wise conversions and comparisons and conditional logic is evaluated (convert `new A(1)` to type `B`, then compare it with `new B(4)`, and so on).</span></span>

### <a name="comparing-null-to-null"></a><span data-ttu-id="6b8e9-138">比較 `null` 與 `null`</span><span class="sxs-lookup"><span data-stu-id="6b8e9-138">Comparing `null` to `null`</span></span>

<span data-ttu-id="6b8e9-139">這是一般比較的特殊案例，它會執行元組比較。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-139">This is a special case from regular comparisons, that carries over to tuple comparisons.</span></span> <span data-ttu-id="6b8e9-140">允許 `null == null` 比較，而且 `null` 常值不會取得任何類型。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-140">The `null == null` comparison is allowed, and the `null` literals do not get any type.</span></span>
<span data-ttu-id="6b8e9-141">在元組相等中，這表示也允許 `(0, null) == (0, null)`，且 `null` 和元組常值不會取得類型。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-141">In tuple equality, this means, `(0, null) == (0, null)` is also allowed and the `null` and tuple literals don't get a type either.</span></span>

### <a name="comparing-a-nullable-struct-to-null-without-operator"></a><span data-ttu-id="6b8e9-142">比較可為 null 的結構與 `null`，而不 `operator==`</span><span class="sxs-lookup"><span data-stu-id="6b8e9-142">Comparing a nullable struct to `null` without `operator==`</span></span>

<span data-ttu-id="6b8e9-143">這是一般比較的另一個特殊案例，它會執行元組比較。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-143">This is another special case from regular comparisons, that carries over to tuple comparisons.</span></span>
<span data-ttu-id="6b8e9-144">如果您有沒有 `operator==`的 `struct S`，則會允許 `(S?)x == null` 比較，並將其轉譯為 `((S?).x).HasValue`。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-144">If you have a `struct S` without `operator==`, the `(S?)x == null` comparison is allowed, and it is interpreted as `((S?).x).HasValue`.</span></span>
<span data-ttu-id="6b8e9-145">在元組相等中，會套用相同的規則，因此允許 `(0, (S?)x) == (0, null)`。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-145">In tuple equality, the same rule is applied, so `(0, (S?)x) == (0, null)` is allowed.</span></span>

## <a name="compatibility"></a><span data-ttu-id="6b8e9-146">相容性</span><span class="sxs-lookup"><span data-stu-id="6b8e9-146">Compatibility</span></span>

<span data-ttu-id="6b8e9-147">如果有人使用比較運算子的執行來撰寫自己的 `ValueTuple` 類型，則先前已藉由多載解析來挑選它。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-147">If someone wrote their own `ValueTuple` types with  an implementation of the comparison operator, it would have previously been picked up by overload resolution.</span></span> <span data-ttu-id="6b8e9-148">但由於新的元組大小寫在多載解析之前，我們會使用元組比較來處理此案例，而不是依賴使用者定義的比較。</span><span class="sxs-lookup"><span data-stu-id="6b8e9-148">But since the new tuple case comes before overload resolution, we would handle this case with tuple comparison instead of relying on the user-defined comparison.</span></span>

----

<span data-ttu-id="6b8e9-149">與[關聯式和類型測試運算子](../../spec/expressions.md#relational-and-type-testing-operators)相關，與[#190](https://github.com/dotnet/csharplang/issues/190)相關聯</span><span class="sxs-lookup"><span data-stu-id="6b8e9-149">Relates to [relational and type testing operators](../../spec/expressions.md#relational-and-type-testing-operators) Relates to [#190](https://github.com/dotnet/csharplang/issues/190)</span></span>
