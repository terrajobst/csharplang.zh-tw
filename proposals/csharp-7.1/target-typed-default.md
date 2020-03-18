---
ms.openlocfilehash: 1852356b830e29c3537a4de559fef32fd2c2f8b6
ms.sourcegitcommit: f7952cdddf85316a4beec493a0ecc14fcb3241c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/30/2019
ms.locfileid: "79484983"
---
# <a name="target-typed-default-literal"></a><span data-ttu-id="d53ac-101">目標型別 "default" 常值</span><span class="sxs-lookup"><span data-stu-id="d53ac-101">Target-typed "default" literal</span></span>

* <span data-ttu-id="d53ac-102">[x] 提議</span><span class="sxs-lookup"><span data-stu-id="d53ac-102">[x] Proposed</span></span>
* <span data-ttu-id="d53ac-103">[x] 原型</span><span class="sxs-lookup"><span data-stu-id="d53ac-103">[x] Prototype</span></span>
* <span data-ttu-id="d53ac-104">[x] 執行</span><span class="sxs-lookup"><span data-stu-id="d53ac-104">[x] Implementation</span></span>
* <span data-ttu-id="d53ac-105">[] 規格</span><span class="sxs-lookup"><span data-stu-id="d53ac-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="d53ac-106">摘要</span><span class="sxs-lookup"><span data-stu-id="d53ac-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="d53ac-107">目標具類型的 `default` 功能是 `default(T)` 運算子的較短形式變體，可省略類型。</span><span class="sxs-lookup"><span data-stu-id="d53ac-107">The target-typed `default` feature is a shorter form variation of the `default(T)` operator, which allows the type to be omitted.</span></span> <span data-ttu-id="d53ac-108">它的類型是由目標型別推斷而來。</span><span class="sxs-lookup"><span data-stu-id="d53ac-108">Its type is inferred by target-typing instead.</span></span> <span data-ttu-id="d53ac-109">除此之外，它的行為就像 `default(T)`。</span><span class="sxs-lookup"><span data-stu-id="d53ac-109">Aside from that, it behaves like `default(T)`.</span></span>

## <a name="motivation"></a><span data-ttu-id="d53ac-110">動機</span><span class="sxs-lookup"><span data-stu-id="d53ac-110">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="d53ac-111">主要動機是避免輸入多餘的資訊。</span><span class="sxs-lookup"><span data-stu-id="d53ac-111">The main motivation is to avoid typing redundant information.</span></span>

<span data-ttu-id="d53ac-112">例如，叫用 `void Method(ImmutableArray<SomeType> array)`時，*預設常值*允許 `M(default)` 取代 `M(default(ImmutableArray<SomeType>))`。</span><span class="sxs-lookup"><span data-stu-id="d53ac-112">For instance, when invoking `void Method(ImmutableArray<SomeType> array)`, the *default* literal allows `M(default)` in place of `M(default(ImmutableArray<SomeType>))`.</span></span>

<span data-ttu-id="d53ac-113">這適用于許多案例，例如：</span><span class="sxs-lookup"><span data-stu-id="d53ac-113">This is applicable in a number of scenarios, such as:</span></span>

- <span data-ttu-id="d53ac-114">宣告區域變數（`ImmutableArray<SomeType> x = default;`）</span><span class="sxs-lookup"><span data-stu-id="d53ac-114">declaring locals (`ImmutableArray<SomeType> x = default;`)</span></span>
- <span data-ttu-id="d53ac-115">三元運算（`var x = flag ? default : ImmutableArray<SomeType>.Empty;`）</span><span class="sxs-lookup"><span data-stu-id="d53ac-115">ternary operations (`var x = flag ? default : ImmutableArray<SomeType>.Empty;`)</span></span>
- <span data-ttu-id="d53ac-116">在方法和 lambda 中傳回（`return default;`）</span><span class="sxs-lookup"><span data-stu-id="d53ac-116">returning in methods and lambdas (`return default;`)</span></span>
- <span data-ttu-id="d53ac-117">宣告選擇性參數的預設值（`void Method(ImmutableArray<SomeType> arrayOpt = default)`）</span><span class="sxs-lookup"><span data-stu-id="d53ac-117">declaring default values for optional parameters (`void Method(ImmutableArray<SomeType> arrayOpt = default)`)</span></span>
- <span data-ttu-id="d53ac-118">在陣列建立運算式中包含預設值（`var x = new[] { default, ImmutableArray.Create(y) };`）</span><span class="sxs-lookup"><span data-stu-id="d53ac-118">including default values in array creation expressions (`var x = new[] { default, ImmutableArray.Create(y) };`)</span></span>


## <a name="detailed-design"></a><span data-ttu-id="d53ac-119">詳細設計</span><span class="sxs-lookup"><span data-stu-id="d53ac-119">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="d53ac-120">引進了新的運算式，也就是預設的常*值*。</span><span class="sxs-lookup"><span data-stu-id="d53ac-120">A new expression is introduced, the *default* literal.</span></span> <span data-ttu-id="d53ac-121">具有此分類的運算式可以透過*預設的常值轉換*，隱含地轉換成任何類型。</span><span class="sxs-lookup"><span data-stu-id="d53ac-121">An expression with this classification can be implicitly converted to any type, by a *default literal conversion*.</span></span> 

<span data-ttu-id="d53ac-122">*預設常值*的類型推斷運作方式與*null*常值相同，不同之處在于允許任何類型（而不只是參考類型）。</span><span class="sxs-lookup"><span data-stu-id="d53ac-122">The inference of the type for the *default* literal works the same as that for the *null* literal, except that any type is allowed (not just reference types).</span></span>

<span data-ttu-id="d53ac-123">這項轉換會產生推斷類型的預設值。</span><span class="sxs-lookup"><span data-stu-id="d53ac-123">This conversion produces the default value of the inferred type.</span></span>

<span data-ttu-id="d53ac-124">*預設*常值可能會有常數值，視推斷的類型而定。</span><span class="sxs-lookup"><span data-stu-id="d53ac-124">The *default* literal may have a constant value, depending on the inferred type.</span></span> <span data-ttu-id="d53ac-125">`const int x = default;` 是合法的，但 `const int? y = default;` 不是。</span><span class="sxs-lookup"><span data-stu-id="d53ac-125">So `const int x = default;` is legal, but `const int? y = default;` is not.</span></span>

<span data-ttu-id="d53ac-126">只要另一個運算元具有類型，*預設常值*可以是等號比較運算子的運算元。</span><span class="sxs-lookup"><span data-stu-id="d53ac-126">The *default* literal can be the operand of equality operators, as long as the other operand has a type.</span></span> <span data-ttu-id="d53ac-127">因此 `default == x` 和 `x == default` 都是有效的運算式，但 `default == default` 是不合法的。</span><span class="sxs-lookup"><span data-stu-id="d53ac-127">So `default == x` and `x == default` are valid expressions, but `default == default` is illegal.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="d53ac-128">缺點</span><span class="sxs-lookup"><span data-stu-id="d53ac-128">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="d53ac-129">次要缺點是，*預設常值*可以用來取代大部分內容中的*null*常值。</span><span class="sxs-lookup"><span data-stu-id="d53ac-129">A minor drawback is that *default* literal can be used in place of *null* literal in most contexts.</span></span> <span data-ttu-id="d53ac-130">有兩個例外狀況 `throw null;` 和 `null == null`，這是允許用於*null*常值，但不是*預設常值*的。</span><span class="sxs-lookup"><span data-stu-id="d53ac-130">Two of the exceptions are `throw null;` and `null == null`, which are allowed for the *null* literal, but not the *default* literal.</span></span>

## <a name="alternatives"></a><span data-ttu-id="d53ac-131">替代方案</span><span class="sxs-lookup"><span data-stu-id="d53ac-131">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="d53ac-132">有幾個需要考慮的替代方案：</span><span class="sxs-lookup"><span data-stu-id="d53ac-132">There are a couple of alternatives to consider:</span></span>

- <span data-ttu-id="d53ac-133">現狀：此功能不會在其本身的優勢下證明，而開發人員會繼續使用具有明確類型的預設運算子。</span><span class="sxs-lookup"><span data-stu-id="d53ac-133">The status quo:  The feature is not justified on its own merits and developers continue to use the default operator with an explicit type.</span></span>
- <span data-ttu-id="d53ac-134">擴充 null 常值：這是使用 `Nothing`的 VB 方法。</span><span class="sxs-lookup"><span data-stu-id="d53ac-134">Extending the null literal: This is the VB approach with `Nothing`.</span></span> <span data-ttu-id="d53ac-135">我們可能會允許 `int x = null;`。</span><span class="sxs-lookup"><span data-stu-id="d53ac-135">We could allow `int x = null;`.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="d53ac-136">未解決的問題</span><span class="sxs-lookup"><span data-stu-id="d53ac-136">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="d53ac-137">[x] 應該*預設*為使用 as 或*as* *運算子的運算元*？</span><span class="sxs-lookup"><span data-stu-id="d53ac-137">[x] Should *default* be allowed as the operand of the *is* or *as* operators?</span></span> <span data-ttu-id="d53ac-138">答：不允許 `default is T`、允許 `x is default`、允許 `default as RefType` （具有 always-null 警告）</span><span class="sxs-lookup"><span data-stu-id="d53ac-138">Answer:  disallow `default is T`, allow `x is default`, allow `default as RefType` (with always-null warning)</span></span>

## <a name="design-meetings"></a><span data-ttu-id="d53ac-139">設計會議</span><span class="sxs-lookup"><span data-stu-id="d53ac-139">Design meetings</span></span>

- [<span data-ttu-id="d53ac-140">LDM 3/7/2017</span><span class="sxs-lookup"><span data-stu-id="d53ac-140">LDM 3/7/2017</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-07.md)
- [<span data-ttu-id="d53ac-141">LDM 3/28/2017</span><span class="sxs-lookup"><span data-stu-id="d53ac-141">LDM 3/28/2017</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-28.md)
- [<span data-ttu-id="d53ac-142">LDM 5/31/2017</span><span class="sxs-lookup"><span data-stu-id="d53ac-142">LDM 5/31/2017</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md#default-in-operators)
