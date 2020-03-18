---
ms.openlocfilehash: fdd858c895d56a7a6b410e6ea7be3032e4851fd6
ms.sourcegitcommit: 5a88d5432d32c690c6b870fc4be32cf26cadd76f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/11/2019
ms.locfileid: "79484955"
---
# <a name="null-coalescing-assignment"></a><span data-ttu-id="a7bce-101">Null 聯合指派</span><span class="sxs-lookup"><span data-stu-id="a7bce-101">null coalescing assignment</span></span>

* <span data-ttu-id="a7bce-102">[x] 提議</span><span class="sxs-lookup"><span data-stu-id="a7bce-102">[x] Proposed</span></span>
* <span data-ttu-id="a7bce-103">[] 原型：未啟動</span><span class="sxs-lookup"><span data-stu-id="a7bce-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="a7bce-104">[] 執行：未啟動</span><span class="sxs-lookup"><span data-stu-id="a7bce-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="a7bce-105">[] 規格：以下</span><span class="sxs-lookup"><span data-stu-id="a7bce-105">[ ] Specification: Below</span></span>

## <a name="summary"></a><span data-ttu-id="a7bce-106">摘要</span><span class="sxs-lookup"><span data-stu-id="a7bce-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="a7bce-107">簡化一般程式碼模式，其中如果變數是 null，則會將值指派給它。</span><span class="sxs-lookup"><span data-stu-id="a7bce-107">Simplifies a common coding pattern where a variable is assigned a value if it is null.</span></span>

<span data-ttu-id="a7bce-108">在本提案中，我們也會放寬 `??` 的型別需求，以允許其型別為不受限制的型別參數在左側使用的運算式。</span><span class="sxs-lookup"><span data-stu-id="a7bce-108">As part of this proposal, we will also loosen the type requirements on `??` to allow an expression whose type is an unconstrained type parameter to be used on the left-hand side.</span></span>

## <a name="motivation"></a><span data-ttu-id="a7bce-109">動機</span><span class="sxs-lookup"><span data-stu-id="a7bce-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="a7bce-110">通常會看到表單的程式碼</span><span class="sxs-lookup"><span data-stu-id="a7bce-110">It is common to see code of the form</span></span>

```csharp
if (variable == null)
{
    variable = expression;
}
```

<span data-ttu-id="a7bce-111">此提議會將非可多載的二元運算子新增至執行此函式的語言。</span><span class="sxs-lookup"><span data-stu-id="a7bce-111">This proposal adds a non-overloadable binary operator to the language that performs this function.</span></span>

<span data-ttu-id="a7bce-112">這項功能至少有八個個別的社區要求。</span><span class="sxs-lookup"><span data-stu-id="a7bce-112">There have been at least eight separate community requests for this feature.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="a7bce-113">詳細設計</span><span class="sxs-lookup"><span data-stu-id="a7bce-113">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="a7bce-114">我們新增指派運算子的形式</span><span class="sxs-lookup"><span data-stu-id="a7bce-114">We add a new form of assignment operator</span></span>

``` antlr
assignment_operator
    : '??='
    ;
```

<span data-ttu-id="a7bce-115">這會遵循[複合指派運算子的現有語義規則](../../spec/expressions.md#compound-assignment)，但如果左側不是 null，我們會省略指派。</span><span class="sxs-lookup"><span data-stu-id="a7bce-115">Which follows the [existing semantic rules for compound assignment operators](../../spec/expressions.md#compound-assignment), except that we elide the assignment if the left-hand side is non-null.</span></span> <span data-ttu-id="a7bce-116">這項功能的規則如下所示。</span><span class="sxs-lookup"><span data-stu-id="a7bce-116">The rules for this feature are as follows.</span></span>

<span data-ttu-id="a7bce-117">指定 `a ??= b`，其中 `A` 是 `a`的類型，`B` 是 `b`的類型，而 `A0` 是可為 null 的實數值型別時，`A` 的基礎類型：`A`</span><span class="sxs-lookup"><span data-stu-id="a7bce-117">Given `a ??= b`, where `A` is the type of `a`, `B` is the type of `b`, and `A0` is the underlying type of `A` if `A` is a nullable value type:</span></span>

1. <span data-ttu-id="a7bce-118">如果 `A` 不存在，或不是不可為 null 的實值型別，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="a7bce-118">If `A` does not exist or is a non-nullable value type, a compile-time error occurs.</span></span>
2. <span data-ttu-id="a7bce-119">如果 `B` 無法隱含地轉換成 `A` 或 `A0` （如果 `A0` 存在），就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="a7bce-119">If `B` is not implicitly convertible to `A` or `A0` (if `A0` exists), a compile-time error occurs.</span></span>
3. <span data-ttu-id="a7bce-120">如果 `A0` 存在，而且 `B` 可以隱含地轉換成 `A0`，而且 `B` 不是動態的，則 `a ??= b` 的類型會是 `A0`。</span><span class="sxs-lookup"><span data-stu-id="a7bce-120">If `A0` exists and `B` is implicitly convertible to `A0`, and `B` is not dynamic, then the type of `a ??= b` is `A0`.</span></span> <span data-ttu-id="a7bce-121">`a ??= b` 在執行時間評估為：</span><span class="sxs-lookup"><span data-stu-id="a7bce-121">`a ??= b` is evaluated at runtime as:</span></span>
   ```C#
   var tmp = a.GetValueOrDefault();
   if (!a.HasValue) { tmp = b; a = tmp; }
   tmp
   ```
   <span data-ttu-id="a7bce-122">除了 `a` 只會評估一次。</span><span class="sxs-lookup"><span data-stu-id="a7bce-122">Except that `a` is only evaluated once.</span></span>
4. <span data-ttu-id="a7bce-123">否則，會 `A``a ??= b` 的類型。</span><span class="sxs-lookup"><span data-stu-id="a7bce-123">Otherwise, the type of `a ??= b` is `A`.</span></span> <span data-ttu-id="a7bce-124">`a ??= b` 在執行時間會當做 `a ?? (a = b)`進行評估，但 `a` 只會評估一次。</span><span class="sxs-lookup"><span data-stu-id="a7bce-124">`a ??= b` is evaluated at runtime as `a ?? (a = b)`, except that `a` is only evaluated once.</span></span>


<span data-ttu-id="a7bce-125">如需 `??`類型需求的放寬，我們會更新其目前陳述的規格，並指定 `a ?? b`，其中 `A` 是 `a`的類型：</span><span class="sxs-lookup"><span data-stu-id="a7bce-125">For the relaxation of the type requirements of `??`, we update the spec where it currently states that, given `a ?? b`, where `A` is the type of `a`:</span></span>

> 1. <span data-ttu-id="a7bce-126">如果存在且不是可為 null 的型別或參考型別，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="a7bce-126">If A exists and is not a nullable type or a reference type, a compile-time error occurs.</span></span>

<span data-ttu-id="a7bce-127">我們放寬了這項需求：</span><span class="sxs-lookup"><span data-stu-id="a7bce-127">We relax this requirement to:</span></span>

1. <span data-ttu-id="a7bce-128">如果存在，而且是不可為 null 的實數值型別，則會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="a7bce-128">If A exists and is a non-nullable value type, a compile-time error occurs.</span></span>

<span data-ttu-id="a7bce-129">這可讓 null 聯合運算子在不受限制的型別參數上工作，因為不受限的型別參數 T 不存在、不是可為 null 的型別，而且不是參考型別。</span><span class="sxs-lookup"><span data-stu-id="a7bce-129">This allows the null coalescing operator to work on unconstrained type parameters, as the unconstrained type parameter T exists, is not a nullable type, and is not a reference type.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="a7bce-130">缺點</span><span class="sxs-lookup"><span data-stu-id="a7bce-130">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="a7bce-131">就像任何語言功能一樣，我們還必須問您，是否重金換回其他語言的複雜性，以提供給可從功能獲益C#的程式主體。</span><span class="sxs-lookup"><span data-stu-id="a7bce-131">As with any language feature, we must question whether the additional complexity to the language is repaid in the additional clarity offered to the body of C# programs that would benefit from the feature.</span></span>

## <a name="alternatives"></a><span data-ttu-id="a7bce-132">替代方案</span><span class="sxs-lookup"><span data-stu-id="a7bce-132">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="a7bce-133">程式設計人員可以手動撰寫 `(x = x ?? y)`、`if (x == null) x = y;`或 `x ?? (x = y)`。</span><span class="sxs-lookup"><span data-stu-id="a7bce-133">The programmer can write `(x = x ?? y)`, `if (x == null) x = y;`, or `x ?? (x = y)` by hand.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="a7bce-134">未解決的問題</span><span class="sxs-lookup"><span data-stu-id="a7bce-134">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="a7bce-135">[] 需要 LDM 審查</span><span class="sxs-lookup"><span data-stu-id="a7bce-135">[ ] Requires LDM review</span></span>
- <span data-ttu-id="a7bce-136">[] 我們也應該支援 `&&=` 和 `||=` 運算子嗎？</span><span class="sxs-lookup"><span data-stu-id="a7bce-136">[ ] Should we also support `&&=` and `||=` operators?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="a7bce-137">設計會議</span><span class="sxs-lookup"><span data-stu-id="a7bce-137">Design meetings</span></span>

<span data-ttu-id="a7bce-138">無。</span><span class="sxs-lookup"><span data-stu-id="a7bce-138">None.</span></span>
