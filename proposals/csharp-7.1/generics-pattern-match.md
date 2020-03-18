---
ms.openlocfilehash: 4f66c0f60d05ed6509a1d0843318a71d1b36c351
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484703"
---
# <a name="pattern-matching-with-generics"></a><span data-ttu-id="f71cb-101">搭配泛型的模式比對</span><span class="sxs-lookup"><span data-stu-id="f71cb-101">pattern-matching with generics</span></span>

* <span data-ttu-id="f71cb-102">[x] 提議</span><span class="sxs-lookup"><span data-stu-id="f71cb-102">[x] Proposed</span></span>
* <span data-ttu-id="f71cb-103">[] 原型：</span><span class="sxs-lookup"><span data-stu-id="f71cb-103">[ ] Prototype:</span></span>
* <span data-ttu-id="f71cb-104">[] 執行：</span><span class="sxs-lookup"><span data-stu-id="f71cb-104">[ ] Implementation:</span></span>
* <span data-ttu-id="f71cb-105">[] 規格：</span><span class="sxs-lookup"><span data-stu-id="f71cb-105">[ ] Specification:</span></span>

## <a name="summary"></a><span data-ttu-id="f71cb-106">摘要</span><span class="sxs-lookup"><span data-stu-id="f71cb-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="f71cb-107">[ C#現有 as 運算子](../../spec/expressions.md#the-as-operator)的規格可讓運算元的類型與指定的類型之間不會有任何轉換（當兩者都是開放式類型時）。</span><span class="sxs-lookup"><span data-stu-id="f71cb-107">The specification for the [existing C# as operator](../../spec/expressions.md#the-as-operator) permits there to be no conversion between the type of the operand and the specified type when either is an open type.</span></span> <span data-ttu-id="f71cb-108">不過，在C# 7 中，`Type identifier` 模式需要在輸入類型與指定類型之間進行轉換。</span><span class="sxs-lookup"><span data-stu-id="f71cb-108">However, in C# 7 the `Type identifier` pattern requires there be a conversion between the type of the input and the given type.</span></span>

<span data-ttu-id="f71cb-109">我們建議您放寬這項功能，並變更 `expression is Type identifier`（除了在C# 7 中允許的情況下允許），也可以在允許 `expression as Type` 時使用。</span><span class="sxs-lookup"><span data-stu-id="f71cb-109">We propose to relax this and change `expression is Type identifier`, in addition to being permitted in the conditions when it is permitted in C# 7, to also be permitted when `expression as Type` would be allowed.</span></span> <span data-ttu-id="f71cb-110">具體來說，新的案例是運算式的類型或指定的類型是開放式類型的情況。</span><span class="sxs-lookup"><span data-stu-id="f71cb-110">Specifically, the new cases are cases where the type of the expression or the specified type is an open type.</span></span> 

## <a name="motivation"></a><span data-ttu-id="f71cb-111">動機</span><span class="sxs-lookup"><span data-stu-id="f71cb-111">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="f71cb-112">模式比對應該「明顯」的情況下，目前會無法編譯。</span><span class="sxs-lookup"><span data-stu-id="f71cb-112">Cases where pattern-matching should "obviously" be permitted currently fail to compile.</span></span> <span data-ttu-id="f71cb-113">例如，請參閱 https://github.com/dotnet/roslyn/issues/16195。</span><span class="sxs-lookup"><span data-stu-id="f71cb-113">See, for example, https://github.com/dotnet/roslyn/issues/16195.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="f71cb-114">詳細設計</span><span class="sxs-lookup"><span data-stu-id="f71cb-114">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="f71cb-115">我們會變更模式比對規格中的段落（建議的加法以粗體顯示）：</span><span class="sxs-lookup"><span data-stu-id="f71cb-115">We change the paragraph in the pattern-matching specification (the proposed addition is shown in bold):</span></span>

> <span data-ttu-id="f71cb-116">左側和指定類型之靜態類型的某些組合會被視為不相容，並導致編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="f71cb-116">Certain combinations of static type of the left-hand-side and the given type are considered incompatible and result in compile-time error.</span></span> <span data-ttu-id="f71cb-117">如果存在身分識別轉換、隱含參考轉換、裝箱轉換、明確參考轉換或從 `E` 到 `T`的取消裝箱轉換，**或 `E` 或 `T` 是開啟的類型，** 則靜態類型 `E` 的值會被視為與類型 `T` 的*模式相容*。</span><span class="sxs-lookup"><span data-stu-id="f71cb-117">A value of static type `E` is said to be *pattern compatible* with the type `T` if there exists an identity conversion, an implicit reference conversion, a boxing conversion, an explicit reference conversion, or an unboxing conversion from `E` to `T`**, or if either `E` or `T` is an open type**.</span></span> <span data-ttu-id="f71cb-118">如果 `E` 類型的運算式在與其相符的類型模式中的類型模式不相容，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="f71cb-118">It is a compile-time error if an expression of type `E` is not pattern compatible with the type in a type pattern that it is matched with.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="f71cb-119">缺點</span><span class="sxs-lookup"><span data-stu-id="f71cb-119">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="f71cb-120">無。</span><span class="sxs-lookup"><span data-stu-id="f71cb-120">None.</span></span>

## <a name="alternatives"></a><span data-ttu-id="f71cb-121">替代方案</span><span class="sxs-lookup"><span data-stu-id="f71cb-121">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="f71cb-122">無。</span><span class="sxs-lookup"><span data-stu-id="f71cb-122">None.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="f71cb-123">未解決的問題</span><span class="sxs-lookup"><span data-stu-id="f71cb-123">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="f71cb-124">無。</span><span class="sxs-lookup"><span data-stu-id="f71cb-124">None.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="f71cb-125">設計會議</span><span class="sxs-lookup"><span data-stu-id="f71cb-125">Design meetings</span></span>

<span data-ttu-id="f71cb-126">LDM 認為這個問題，並覺得它是一個 bug 修正層級變更。</span><span class="sxs-lookup"><span data-stu-id="f71cb-126">LDM considered this question and felt it was a bug-fix level change.</span></span> <span data-ttu-id="f71cb-127">我們會將它視為不同的語言功能，因為只要在發行語言後進行變更，就會導致向前不相容。</span><span class="sxs-lookup"><span data-stu-id="f71cb-127">We are treating it as a separate language feature because just making the change after the language has been released would introduce a forward incompatibility.</span></span> <span data-ttu-id="f71cb-128">使用建議的變更時，程式設計人員必須指定語言版本7.1。</span><span class="sxs-lookup"><span data-stu-id="f71cb-128">Using the proposed change requires that the programmer specify language version 7.1.</span></span>
