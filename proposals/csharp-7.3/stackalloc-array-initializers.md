---
ms.openlocfilehash: 7ea62713416ef82034963aef06f3cb11703342ed
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484605"
---
# <a name="stackalloc-array-initializers"></a><span data-ttu-id="3b118-101">Stackalloc 陣列初始設定式</span><span class="sxs-lookup"><span data-stu-id="3b118-101">Stackalloc array initializers</span></span>

## <a name="summary"></a><span data-ttu-id="3b118-102">摘要</span><span class="sxs-lookup"><span data-stu-id="3b118-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="3b118-103">允許陣列初始化運算式語法與 `stackalloc`搭配使用。</span><span class="sxs-lookup"><span data-stu-id="3b118-103">Allow array initializer syntax to be used with `stackalloc`.</span></span>

## <a name="motivation"></a><span data-ttu-id="3b118-104">動機</span><span class="sxs-lookup"><span data-stu-id="3b118-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="3b118-105">一般陣列可以在建立時初始化其元素。</span><span class="sxs-lookup"><span data-stu-id="3b118-105">Ordinary arrays can have their elements initialized at creation time.</span></span> <span data-ttu-id="3b118-106">在 `stackalloc` 情況下，這似乎是合理的。</span><span class="sxs-lookup"><span data-stu-id="3b118-106">It seems reasonable to allow that in `stackalloc` case.</span></span>

<span data-ttu-id="3b118-107">為什麼不允許使用這種語法的問題，`stackalloc` 會經常發生。</span><span class="sxs-lookup"><span data-stu-id="3b118-107">The question of why such syntax is not allowed with `stackalloc` arises fairly frequently.</span></span>  
<span data-ttu-id="3b118-108">例如，請參閱[#1112](https://github.com/dotnet/csharplang/issues/1112)</span><span class="sxs-lookup"><span data-stu-id="3b118-108">See, for example, [#1112](https://github.com/dotnet/csharplang/issues/1112)</span></span>

## <a name="detailed-design"></a><span data-ttu-id="3b118-109">詳細設計</span><span class="sxs-lookup"><span data-stu-id="3b118-109">Detailed design</span></span>

<span data-ttu-id="3b118-110">一般陣列可以透過下列語法建立：</span><span class="sxs-lookup"><span data-stu-id="3b118-110">Ordinary arrays can be created through the following syntax:</span></span>

```csharp
new int[3]
new int[3] { 1, 2, 3 }
new int[] { 1, 2, 3 }
new[] { 1, 2, 3 }
```

<span data-ttu-id="3b118-111">我們應該允許透過下列各建立堆疊配置的陣列：</span><span class="sxs-lookup"><span data-stu-id="3b118-111">We should allow stack allocated arrays be created through:</span></span>  

```csharp
stackalloc int[3]               // currently allowed
stackalloc int[3] { 1, 2, 3 }
stackalloc int[] { 1, 2, 3 }
stackalloc[] { 1, 2, 3 }
```

<span data-ttu-id="3b118-112">所有案例的語義大致上與陣列相同。</span><span class="sxs-lookup"><span data-stu-id="3b118-112">The semantics of all cases is roughly the same as with arrays.</span></span>  
<span data-ttu-id="3b118-113">例如：在最後一個案例中，會從初始化運算式推斷元素型別，而且必須是「非受控」型別。</span><span class="sxs-lookup"><span data-stu-id="3b118-113">For example: in the last case the element type is inferred from the initializer and must be an "unmanaged" type.</span></span>

<span data-ttu-id="3b118-114">注意：此功能不會相依于做為 `Span<T>`的目標。</span><span class="sxs-lookup"><span data-stu-id="3b118-114">NOTE: the feature is not dependent on the target being a `Span<T>`.</span></span> <span data-ttu-id="3b118-115">它就像是 `T*` 案例中一樣，因此在 `Span<T>` 的情況下，述詞似乎不合理。</span><span class="sxs-lookup"><span data-stu-id="3b118-115">It is just as applicable in `T*` case, so it does not seem reasonable to predicate it on `Span<T>` case.</span></span>  

## <a name="translation"></a><span data-ttu-id="3b118-116">翻譯</span><span class="sxs-lookup"><span data-stu-id="3b118-116">Translation</span></span>

<span data-ttu-id="3b118-117">單純的實作為在透過一系列的元素指派來建立之後，就可以直接初始化陣列。</span><span class="sxs-lookup"><span data-stu-id="3b118-117">The naive implementation could just initialize the array right after creation through a series of element-wise assignments.</span></span>  

<span data-ttu-id="3b118-118">類似于使用陣列的情況，偵測所有或大部分專案都是全像類型的情況，並透過複製所有常數專案的預先建立狀態，來使用更有效率的技術，可能也會是理想的做法。</span><span class="sxs-lookup"><span data-stu-id="3b118-118">Similarly to the case with arrays, it might be possible and desirable to detect cases where all or most of the elements are blittable types and use more efficient techniques by copying over the pre-created state of all the constant elements.</span></span> 

## <a name="drawbacks"></a><span data-ttu-id="3b118-119">缺點</span><span class="sxs-lookup"><span data-stu-id="3b118-119">Drawbacks</span></span>
[drawbacks]: #drawbacks

## <a name="alternatives"></a><span data-ttu-id="3b118-120">替代方案</span><span class="sxs-lookup"><span data-stu-id="3b118-120">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="3b118-121">這是方便的功能。</span><span class="sxs-lookup"><span data-stu-id="3b118-121">This is a convenience feature.</span></span> <span data-ttu-id="3b118-122">您可以不執行任何動作。</span><span class="sxs-lookup"><span data-stu-id="3b118-122">It is possible to just do nothing.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="3b118-123">未解決的問題</span><span class="sxs-lookup"><span data-stu-id="3b118-123">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a><span data-ttu-id="3b118-124">設計會議</span><span class="sxs-lookup"><span data-stu-id="3b118-124">Design meetings</span></span>

<span data-ttu-id="3b118-125">還沒有。</span><span class="sxs-lookup"><span data-stu-id="3b118-125">None yet.</span></span> 
