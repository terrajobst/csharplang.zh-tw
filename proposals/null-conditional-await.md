---
ms.openlocfilehash: 3d10cacef98e802333c8cbe65edb93c19c74cabf
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484493"
---
# <a name="null-conditional-await"></a><span data-ttu-id="f8166-101">null-條件式 await</span><span class="sxs-lookup"><span data-stu-id="f8166-101">null-conditional await</span></span>

* <span data-ttu-id="f8166-102">[x] 提議</span><span class="sxs-lookup"><span data-stu-id="f8166-102">[x] Proposed</span></span>
* <span data-ttu-id="f8166-103">[] 原型：無</span><span class="sxs-lookup"><span data-stu-id="f8166-103">[ ] Prototype: None</span></span>
* <span data-ttu-id="f8166-104">[] 執行：無</span><span class="sxs-lookup"><span data-stu-id="f8166-104">[ ] Implementation: None</span></span>
* <span data-ttu-id="f8166-105">[] 規格：已啟動，下方</span><span class="sxs-lookup"><span data-stu-id="f8166-105">[ ] Specification: Started, below</span></span>

## <a name="summary"></a><span data-ttu-id="f8166-106">摘要</span><span class="sxs-lookup"><span data-stu-id="f8166-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="f8166-107">支援表單 `await? e`的運算式，如果它不是 null，則會等候 `e`，否則會導致 `null`。</span><span class="sxs-lookup"><span data-stu-id="f8166-107">Support an expression of the form `await? e`, which awaits `e` if it is non-null, otherwise it results in `null`.</span></span>

## <a name="motivation"></a><span data-ttu-id="f8166-108">動機</span><span class="sxs-lookup"><span data-stu-id="f8166-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="f8166-109">這是常見的編碼模式，而且這項功能會與現有的 null 傳播和 null 聯合運算子具有良好的協力。</span><span class="sxs-lookup"><span data-stu-id="f8166-109">This is a common coding pattern, and this feature would have nice synergy with the existing null-propagating and null-coalescing operators.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="f8166-110">詳細設計</span><span class="sxs-lookup"><span data-stu-id="f8166-110">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="f8166-111">我們新增了一種新形式的*await_expression*：</span><span class="sxs-lookup"><span data-stu-id="f8166-111">We add a new form of the *await_expression*:</span></span>

```antlr
await_expression
    : 'await' '?' unary_expression
    ;
```

<span data-ttu-id="f8166-112">Null 條件 `await` 運算子只有在該運算元不是 null 時，才會等候其運算元。</span><span class="sxs-lookup"><span data-stu-id="f8166-112">The null-conditional `await` operator awaits its operand only if that operand is non-null.</span></span> <span data-ttu-id="f8166-113">否則，套用運算子的結果為 null。</span><span class="sxs-lookup"><span data-stu-id="f8166-113">Otherwise the result of applying the operator is null.</span></span>

<span data-ttu-id="f8166-114">結果的類型是使用[null 條件運算子的規則來](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#null-conditional-operator)計算。</span><span class="sxs-lookup"><span data-stu-id="f8166-114">The type of the result is computed using the [rules for the null-conditional operator](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#null-conditional-operator).</span></span>

> <span data-ttu-id="f8166-115">**注意：** 如果 `e` 的類型為 `Task`，則 `await? e;` 會在 `e` `null`時不會執行任何動作，而如果未 `e`，則會進行 await `null`。</span><span class="sxs-lookup"><span data-stu-id="f8166-115">**NOTE:** If `e` is of type `Task`, then `await? e;` would do nothing if `e` is `null`, and await `e` if it is not `null`.</span></span>
>
> <span data-ttu-id="f8166-116">如果 `e` 的類型 `Task<K>`，其中 `K` 是實數值型別，則 `await? e` 會產生類型 `K?`的值。</span><span class="sxs-lookup"><span data-stu-id="f8166-116">If `e` is of type `Task<K>` where `K` is a value type, then `await? e` would yield a value of type `K?`.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="f8166-117">缺點</span><span class="sxs-lookup"><span data-stu-id="f8166-117">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="f8166-118">就像任何語言功能一樣，我們還必須問您，是否重金換回其他語言的複雜性，以提供給可從功能獲益C#的程式主體。</span><span class="sxs-lookup"><span data-stu-id="f8166-118">As with any language feature, we must question whether the additional complexity to the language is repaid in the additional clarity offered to the body of C# programs that would benefit from the feature.</span></span>

## <a name="alternatives"></a><span data-ttu-id="f8166-119">替代方案</span><span class="sxs-lookup"><span data-stu-id="f8166-119">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="f8166-120">雖然它需要一些重複使用的程式碼，但此運算子的用法通常會取代為類似 `(e == null) ? null : await e` 的運算式或類似 `if (e != null) await e`的語句。</span><span class="sxs-lookup"><span data-stu-id="f8166-120">Although it requires some boilerplate code, uses of this operator can often be replaced by an expression something like `(e == null) ? null : await e` or a statement like `if (e != null) await e`.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="f8166-121">未解決的問題</span><span class="sxs-lookup"><span data-stu-id="f8166-121">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="f8166-122">[] 需要 LDM 審查</span><span class="sxs-lookup"><span data-stu-id="f8166-122">[ ] Requires LDM review</span></span>

## <a name="design-meetings"></a><span data-ttu-id="f8166-123">設計會議</span><span class="sxs-lookup"><span data-stu-id="f8166-123">Design meetings</span></span>

<span data-ttu-id="f8166-124">無。</span><span class="sxs-lookup"><span data-stu-id="f8166-124">None.</span></span>
