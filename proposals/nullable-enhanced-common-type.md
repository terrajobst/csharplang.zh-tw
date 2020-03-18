---
ms.openlocfilehash: 032cb8590a0b6e83f8ab6201e10720f1b254c605
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484528"
---
# <a name="nullable-enhanced-common-type"></a><span data-ttu-id="d2683-101">可為 null 增強的一般類型</span><span class="sxs-lookup"><span data-stu-id="d2683-101">Nullable-Enhanced Common Type</span></span>

* <span data-ttu-id="d2683-102">[x] 提議</span><span class="sxs-lookup"><span data-stu-id="d2683-102">[x] Proposed</span></span>
* <span data-ttu-id="d2683-103">[] 原型：無</span><span class="sxs-lookup"><span data-stu-id="d2683-103">[ ] Prototype: None</span></span>
* <span data-ttu-id="d2683-104">[] 執行：無</span><span class="sxs-lookup"><span data-stu-id="d2683-104">[ ] Implementation: None</span></span>
* <span data-ttu-id="d2683-105">[] 規格：請參閱下文</span><span class="sxs-lookup"><span data-stu-id="d2683-105">[ ] Specification: See below</span></span>

## <a name="summary"></a><span data-ttu-id="d2683-106">摘要</span><span class="sxs-lookup"><span data-stu-id="d2683-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="d2683-107">在某些情況下，目前的一般類型演算法結果會以直覺方式呈現，並導致程式設計人員加入類似于程式碼的重複轉換。</span><span class="sxs-lookup"><span data-stu-id="d2683-107">There is a situation in which the current common-type algorithm results are counter-intuitive, and results in the programmer adding what feels like a redundant cast to the code.</span></span> <span data-ttu-id="d2683-108">透過這項變更，運算式（例如 `condition ? 1 : null`）會產生 `int?`類型的值，而運算式（例如 `condition ? x : 1.0`，其中 `x` 的類型 `int?` 會產生 `double?`類型的值。</span><span class="sxs-lookup"><span data-stu-id="d2683-108">With this change, an expression such as `condition ? 1 : null` would result in a value of type `int?`, and an expression such as `condition ? x : 1.0` where `x` is of type `int?` would result in a value of type `double?`.</span></span>

## <a name="motivation"></a><span data-ttu-id="d2683-109">動機</span><span class="sxs-lookup"><span data-stu-id="d2683-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="d2683-110">這是常見的程式設計人員所造成的原因，例如不必要的未定案程式碼。</span><span class="sxs-lookup"><span data-stu-id="d2683-110">This is a common cause of what feels to the programmer like needless boilerplate code.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="d2683-111">詳細設計</span><span class="sxs-lookup"><span data-stu-id="d2683-111">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="d2683-112">我們會修改用來[尋找一組運算式之最佳一般類型](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions)的規格，以影響下列情況：</span><span class="sxs-lookup"><span data-stu-id="d2683-112">We modify the specification for [finding the best common type of a set of expressions](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) to affect the following situations:</span></span>

- <span data-ttu-id="d2683-113">如果一個運算式是不可為 null 的實值型別 `T`，另一個則是 null 常值，則結果會是 `T?`類型。</span><span class="sxs-lookup"><span data-stu-id="d2683-113">If one expression is of a non-nullable value type `T` and the other is a null literal, the result is of type `T?`.</span></span>
- <span data-ttu-id="d2683-114">如果一個運算式是可為 null 的實值型別 `T?` 而且另一個是 `U`的實值型別，而且有從 `T` 到 `U`的隱含轉換，則結果會是 `U?`型別。</span><span class="sxs-lookup"><span data-stu-id="d2683-114">If one expression is of a nullable value type `T?` and the other is of a value type `U`, and there is an implicit conversion from `T` to `U`, then the result is of type `U?`.</span></span>

<span data-ttu-id="d2683-115">這應該會影響語言的下列各方面：</span><span class="sxs-lookup"><span data-stu-id="d2683-115">This is expected to affect the following aspects of the language:</span></span>

- <span data-ttu-id="d2683-116">[三元運算式](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#conditional-operator)</span><span class="sxs-lookup"><span data-stu-id="d2683-116">the [ternary expression](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#conditional-operator)</span></span>
- <span data-ttu-id="d2683-117">隱含類型[陣列建立運算式](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#array-creation-expressions)</span><span class="sxs-lookup"><span data-stu-id="d2683-117">implicitly typed [array creation expression](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#array-creation-expressions)</span></span>
- <span data-ttu-id="d2683-118">推斷型別推斷的[lambda 傳回型](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#inferred-return-type)別</span><span class="sxs-lookup"><span data-stu-id="d2683-118">inferring the [return type of a lambda](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#inferred-return-type) for type inference</span></span>
- <span data-ttu-id="d2683-119">涉及泛型的案例，例如叫用 `M<T>(T a, T b)` 做為 `M(1, null)`。</span><span class="sxs-lookup"><span data-stu-id="d2683-119">cases involving generics, such as invoking `M<T>(T a, T b)` as `M(1, null)`.</span></span>

<span data-ttu-id="d2683-120">更精確的說，我們會變更規格的下列區段（以粗體插入、刪除刪除）：</span><span class="sxs-lookup"><span data-stu-id="d2683-120">More precisely, we change the following sections of the specification (insertions in bold, deletions in strikethrough):</span></span>

> #### <a name="output-type-inferences"></a><span data-ttu-id="d2683-121">輸出類型推斷</span><span class="sxs-lookup"><span data-stu-id="d2683-121">Output type inferences</span></span>
> 
> <span data-ttu-id="d2683-122">*輸出類型推斷*是*從*運算式 `E`*到*類型 `T` 以下列方式進行：</span><span class="sxs-lookup"><span data-stu-id="d2683-122">An *output type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>
> 
> *  <span data-ttu-id="d2683-123">如果 `E` 是具有推斷傳回類型的匿名函式 `U`[（推斷](expressions.md#inferred-return-type)的傳回型別），而 `T` 是具有傳回型別 `Tb`的委派型別或運算式樹狀結構型別，則會*從*`U`*到*`Tb`進行*較低*的系結推斷（[較低](expressions.md#lower-bound-inferences)系結）。</span><span class="sxs-lookup"><span data-stu-id="d2683-123">If `E` is an anonymous function with inferred return type  `U` ([Inferred return type](expressions.md#inferred-return-type)) and `T` is a delegate type or expression tree type with return type `Tb`, then a *lower-bound inference* ([Lower-bound inferences](expressions.md#lower-bound-inferences)) is made *from* `U` *to* `Tb`.</span></span>
> *  <span data-ttu-id="d2683-124">否則，如果 `E` 是方法群組，而 `T` 是具有參數類型 `T1...Tk` 和傳回類型 `Tb`的委派類型或運算式樹狀結構類型，而且 `E` 具有類型 `T1...Tk` 的多載解析會產生傳回類型 `U`的單一方法，則會*從*`U`*到*`Tb`進行*下限推斷*。</span><span class="sxs-lookup"><span data-stu-id="d2683-124">Otherwise, if `E` is a method group and `T` is a delegate type or expression tree type with parameter types `T1...Tk` and return type `Tb`, and overload resolution of `E` with the types `T1...Tk` yields a single method with return type `U`, then a *lower-bound inference* is made *from* `U` *to* `Tb`.</span></span>
> *  <span data-ttu-id="d2683-125">\* \* 否則，如果 `E` 是可為 null 的實數值型別 `U?`的運算式，則會從 `U`*到*`T` 進行*下限推斷*，並將*null*系結加入 `T`*中*。</span><span class="sxs-lookup"><span data-stu-id="d2683-125">\*\*Otherwise, if `E` is an expression with nullable value type `U?`, then a *lower-bound inference* is made *from* `U` *to* `T` and a *null bound* is added to `T`.</span></span> **
> *  <span data-ttu-id="d2683-126">否則，如果 `E` 是具有類型 `U`的運算式，則會*從*`U`*到*`T`進行*較低*系結的推斷。</span><span class="sxs-lookup"><span data-stu-id="d2683-126">Otherwise, if `E` is an expression with type `U`, then a *lower-bound inference* is made *from* `U` *to* `T`.</span></span>
> *  <span data-ttu-id="d2683-127">**否則，如果 `E` 是具有值 `null`的常數運算式，則會將*null*系結新增至 `T`**</span><span class="sxs-lookup"><span data-stu-id="d2683-127">**Otherwise, if `E` is a constant expression with value `null`, then a *null bound* is added to `T`**</span></span> 
> *  <span data-ttu-id="d2683-128">否則，就不會進行推斷。</span><span class="sxs-lookup"><span data-stu-id="d2683-128">Otherwise, no inferences are made.</span></span>

> #### <a name="fixing"></a><span data-ttu-id="d2683-129">修正</span><span class="sxs-lookup"><span data-stu-id="d2683-129">Fixing</span></span>
> 
> <span data-ttu-id="d2683-130">未*固定的類型變數*`Xi` 具有一組界限，如下*所示：*</span><span class="sxs-lookup"><span data-stu-id="d2683-130">An *unfixed* type variable `Xi` with a set of bounds is *fixed* as follows:</span></span>
> 
> *  <span data-ttu-id="d2683-131">*候選類型*的集合 `Uj` 一開始就是 `Xi`界限集合中的所有類型集合。</span><span class="sxs-lookup"><span data-stu-id="d2683-131">The set of *candidate types* `Uj` starts out as the set of all types in the set of bounds for `Xi`.</span></span>
> *  <span data-ttu-id="d2683-132">接著，我們會檢查每個 `Xi` 的系結：對於 `Xi` 所有與 `U` 不完全相同的 `U` 之每個完全系結的 `Uj`，會從候選集合中移除。</span><span class="sxs-lookup"><span data-stu-id="d2683-132">We then examine each bound for `Xi` in turn: For each exact bound `U` of `Xi` all types `Uj` which are not identical to `U` are removed from the candidate set.</span></span> <span data-ttu-id="d2683-133">對於 `Xi` 的每個下限 `U`，會從候選集合中移除*不*是從 `U` 隱含轉換的所有類型 `Uj`。</span><span class="sxs-lookup"><span data-stu-id="d2683-133">For each lower bound `U` of `Xi` all types `Uj` to which there is *not* an implicit conversion from `U` are removed from the candidate set.</span></span> <span data-ttu-id="d2683-134">針對 `Xi` 的每個上限 `U`，*不*會從候選集合中移除隱含轉換成 `U` 的所有類型 `Uj`。</span><span class="sxs-lookup"><span data-stu-id="d2683-134">For each upper bound `U` of `Xi` all types `Uj` from which there is *not* an implicit conversion to `U` are removed from the candidate set.</span></span>
> *  <span data-ttu-id="d2683-135">如果剩餘的候選類型 `Uj` 有一個唯一的類型 `V`，其中隱含轉換至所有其他候選類型，則~~`Xi` 會固定到 `V`。~~</span><span class="sxs-lookup"><span data-stu-id="d2683-135">If among the remaining candidate types `Uj` there is a unique type `V` from which there is an implicit conversion to all the other candidate types, then ~~`Xi` is fixed to `V`.~~</span></span>
>     -  <span data-ttu-id="d2683-136">**如果 `V` 是實值型別，且 `Xi`有*null*系結，則 `Xi` 會固定為 `V?`**</span><span class="sxs-lookup"><span data-stu-id="d2683-136">**If `V` is a value type and there is a *null bound* for `Xi`, then `Xi` is fixed to `V?`**</span></span>
>     -  <span data-ttu-id="d2683-137">**否則 `Xi` 會固定為 `V`**</span><span class="sxs-lookup"><span data-stu-id="d2683-137">**Otherwise   `Xi` is fixed to `V`**</span></span>
> *  <span data-ttu-id="d2683-138">否則，型別推斷會失敗。</span><span class="sxs-lookup"><span data-stu-id="d2683-138">Otherwise, type inference fails.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="d2683-139">缺點</span><span class="sxs-lookup"><span data-stu-id="d2683-139">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="d2683-140">此提案可能會有一些不相容的問題。</span><span class="sxs-lookup"><span data-stu-id="d2683-140">There may be some incompatibilities introduced by this proposal.</span></span>

## <a name="alternatives"></a><span data-ttu-id="d2683-141">替代方案</span><span class="sxs-lookup"><span data-stu-id="d2683-141">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="d2683-142">無。</span><span class="sxs-lookup"><span data-stu-id="d2683-142">None.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="d2683-143">未解決的問題</span><span class="sxs-lookup"><span data-stu-id="d2683-143">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="d2683-144">[] 此提案引進的不相容性嚴重性為何（如果有的話），以及如何仲裁？</span><span class="sxs-lookup"><span data-stu-id="d2683-144">[ ] What is the severity of incompatibility introduced by this proposal, if any, and how can it be moderated?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="d2683-145">設計會議</span><span class="sxs-lookup"><span data-stu-id="d2683-145">Design meetings</span></span>

<span data-ttu-id="d2683-146">無。</span><span class="sxs-lookup"><span data-stu-id="d2683-146">None.</span></span>
