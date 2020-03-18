---
ms.openlocfilehash: 2070cf3b3269585055791adc3427cbd134df444d
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484654"
---
# <a name="pattern-based-fixed-statement"></a><span data-ttu-id="a48ab-101">模式型 `fixed` 陳述式</span><span class="sxs-lookup"><span data-stu-id="a48ab-101">Pattern-based `fixed` statement</span></span>

## <a name="summary"></a><span data-ttu-id="a48ab-102">摘要</span><span class="sxs-lookup"><span data-stu-id="a48ab-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="a48ab-103">引進允許類型參與 `fixed` 語句的模式。</span><span class="sxs-lookup"><span data-stu-id="a48ab-103">Introduce a pattern that would allow types to participate in `fixed` statements.</span></span> 

## <a name="motivation"></a><span data-ttu-id="a48ab-104">動機</span><span class="sxs-lookup"><span data-stu-id="a48ab-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="a48ab-105">語言提供一個機制來釘選 managed 資料，並取得基礎緩衝區的原生指標。</span><span class="sxs-lookup"><span data-stu-id="a48ab-105">The language provides a mechanism for pinning managed data and obtain a native pointer to the underlying buffer.</span></span>

```csharp
fixed(byte* ptr = byteArray)
{
   // ptr is a native pointer to the first element of the array
   // byteArray is protected from being moved/collected by the GC for the duration of this block 
}

```

<span data-ttu-id="a48ab-106">可以參與 `fixed` 的類型集合會進行硬式編碼，並限制為數組和 `System.String`。</span><span class="sxs-lookup"><span data-stu-id="a48ab-106">The set of types that can participate in `fixed` is hardcoded and limited to arrays and `System.String`.</span></span> <span data-ttu-id="a48ab-107">引進新的基本專案（例如 `ImmutableArray<T>`、`Span<T>`、`Utf8String`）時，硬式編碼「特殊」類型不會進行調整。</span><span class="sxs-lookup"><span data-stu-id="a48ab-107">Hardcoding "special" types does not scale when new primitives such as `ImmutableArray<T>`, `Span<T>`, `Utf8String` are introduced.</span></span> 

<span data-ttu-id="a48ab-108">此外，`System.String` 的目前解決方案會依賴相當嚴格的 API。</span><span class="sxs-lookup"><span data-stu-id="a48ab-108">In addition, the current solution for `System.String` relies on a fairly rigid API.</span></span> <span data-ttu-id="a48ab-109">API 的形狀意指 `System.String` 是連續的物件，它會在物件標頭的固定位移上內嵌 UTF16 編碼的資料。</span><span class="sxs-lookup"><span data-stu-id="a48ab-109">The shape of the API implies that `System.String` is a contiguous object that embeds UTF16 encoded data at a fixed offset from the object header.</span></span> <span data-ttu-id="a48ab-110">這種方法在數個可能需要變更基礎配置的提案中發現問題。</span><span class="sxs-lookup"><span data-stu-id="a48ab-110">Such approach has been found problematic in several proposals that could require changes to the underlying layout.</span></span> <span data-ttu-id="a48ab-111">最好能夠切換到更有彈性的東西，將 `System.String` 物件與內部標記法分開，以因應非受控 interop 的目的。</span><span class="sxs-lookup"><span data-stu-id="a48ab-111">It would be desirable to be able to switch to something more flexible that decouples `System.String` object from its internal representation for the purpose of unmanaged interop.</span></span> 

## <a name="detailed-design"></a><span data-ttu-id="a48ab-112">詳細設計</span><span class="sxs-lookup"><span data-stu-id="a48ab-112">Detailed design</span></span>
[design]: #detailed-design

## <a name="pattern"></a><span data-ttu-id="a48ab-113">*模式*</span><span class="sxs-lookup"><span data-stu-id="a48ab-113">*Pattern*</span></span> ##
<span data-ttu-id="a48ab-114">以模式為基礎的可行「固定」需要：</span><span class="sxs-lookup"><span data-stu-id="a48ab-114">A viable pattern-based “fixed” need to:</span></span>
-   <span data-ttu-id="a48ab-115">提供受控參考來釘選實例並初始化指標（最好是相同的參考）</span><span class="sxs-lookup"><span data-stu-id="a48ab-115">Provide the managed references to pin the instance and to initialize the pointer (preferably this is the same reference)</span></span>
-   <span data-ttu-id="a48ab-116">明確傳達非受控元素的類型（例如 "char" 代表 "string"）</span><span class="sxs-lookup"><span data-stu-id="a48ab-116">Convey unambiguously the type of the unmanaged element   (i.e. “char” for “string”)</span></span>
-   <span data-ttu-id="a48ab-117">在沒有可參考的情況下，規定「空白」案例中的行為。</span><span class="sxs-lookup"><span data-stu-id="a48ab-117">Prescribe the behavior in "empty" case when there is nothing to refer to.</span></span> 
-   <span data-ttu-id="a48ab-118">不應將 API 作者推送至不會在 `fixed`外損及類型的設計決策。</span><span class="sxs-lookup"><span data-stu-id="a48ab-118">Should not push API authors toward design decisions that hurt the use of the type outside of `fixed`.</span></span>

<span data-ttu-id="a48ab-119">我認為可以藉由辨識特別命名的 ref 傳回成員來滿足上述條件： `ref [readonly] T GetPinnableReference()`。</span><span class="sxs-lookup"><span data-stu-id="a48ab-119">I think the above could be satisfied by recognizing a specially named ref-returning member: `ref [readonly] T GetPinnableReference()`.</span></span>

<span data-ttu-id="a48ab-120">為了供 `fixed` 語句使用，必須符合下列條件：</span><span class="sxs-lookup"><span data-stu-id="a48ab-120">In order to be used by the `fixed` statement the following conditions must be met:</span></span>

1. <span data-ttu-id="a48ab-121">其中只有一個為類型提供的成員。</span><span class="sxs-lookup"><span data-stu-id="a48ab-121">There is only one such member provided for a type.</span></span>
1. <span data-ttu-id="a48ab-122">由 `ref` 或 `ref readonly`傳回。</span><span class="sxs-lookup"><span data-stu-id="a48ab-122">Returns by `ref` or `ref readonly`.</span></span> <span data-ttu-id="a48ab-123">（允許`readonly`，讓不可變/唯讀類型的作者可以在不新增可在安全程式碼中使用的可寫入 API 的情況下，執行模式）</span><span class="sxs-lookup"><span data-stu-id="a48ab-123">(`readonly` is permitted so that authors of immutable/readonly types could implement the pattern without adding writeable API that could be used in safe code)</span></span>
1. <span data-ttu-id="a48ab-124">T 是非受控型別。</span><span class="sxs-lookup"><span data-stu-id="a48ab-124">T is an unmanaged type.</span></span>
<span data-ttu-id="a48ab-125">（因為 `T*` 會變成指標類型。</span><span class="sxs-lookup"><span data-stu-id="a48ab-125">(since `T*` becomes the pointer type.</span></span> <span data-ttu-id="a48ab-126">當「非受控」的概念展開時，限制會自然地展開</span><span class="sxs-lookup"><span data-stu-id="a48ab-126">The restriction will naturally expand if/when the notion of "unmanaged" is expanded)</span></span>
1. <span data-ttu-id="a48ab-127">當沒有要釘選的資料時，會傳回 managed `nullptr` –可能是傳達是否空序列的最便宜方式。</span><span class="sxs-lookup"><span data-stu-id="a48ab-127">Returns managed `nullptr` when there is no data to pin – probably the cheapest way to convey emptiness.</span></span>
<span data-ttu-id="a48ab-128">（請注意，"" 字串會傳回 ' \ 0 ' 的參考，因為字串是以 null 終止）</span><span class="sxs-lookup"><span data-stu-id="a48ab-128">(note that “” string returns a ref to '\0' since strings are null-terminated)</span></span>

<span data-ttu-id="a48ab-129">或者，`#3` 我們可以允許空白案例中的結果為未定義或特定執行。</span><span class="sxs-lookup"><span data-stu-id="a48ab-129">Alternatively for the `#3` we can allow the result in empty cases be undefined or implementation-specific.</span></span> <span data-ttu-id="a48ab-130">不過，這可能會使 API 變得更危險，而且容易發生濫用和非預期的相容性負擔。</span><span class="sxs-lookup"><span data-stu-id="a48ab-130">That, however, may make the API more dangerous and prone to abuse and unintended compatibility burdens.</span></span> 

## <a name="translation"></a><span data-ttu-id="a48ab-131">*翻譯*</span><span class="sxs-lookup"><span data-stu-id="a48ab-131">*Translation*</span></span> ##

```csharp
fixed(byte* ptr = thing)
{ 
    // <BODY>
}
```

<span data-ttu-id="a48ab-132">會成為下列虛擬代碼（並非所有可C#表達的）</span><span class="sxs-lookup"><span data-stu-id="a48ab-132">becomes the following pseudocode (not all expressible in C#)</span></span>

```csharp
byte* ptr;
// specially decorated "pinned" IL local slot, not visible to user code.
pinned ref byte _pinned;

try
{
    // NOTE: null check is omitted for value types 
    // NOTE: `thing` is evaluated only once (temporary is introduced if necessary) 
    if (thing != null)
    {
        // obtain and "pin" the reference
        _pinned = ref thing.GetPinnableReference();

        // unsafe cast in IL
        ptr = (byte*)_pinned;
    }
    else
    {
        ptr = default(byte*);
    }

    // <BODY> 
}
finally   // finally can be omitted when not observable
{
    // "unpin" the object
    _pinned = nullptr;
}
```

## <a name="drawbacks"></a><span data-ttu-id="a48ab-133">缺點</span><span class="sxs-lookup"><span data-stu-id="a48ab-133">Drawbacks</span></span>
[drawbacks]: #drawbacks

- <span data-ttu-id="a48ab-134">GetPinnableReference 只能在 `fixed`中使用，但不會避免在安全程式碼中使用，因此實作項必須記住這一點。</span><span class="sxs-lookup"><span data-stu-id="a48ab-134">GetPinnableReference is intended to be used only in `fixed`, but nothing prevents its use in safe code, so implementor must keep that in mind.</span></span>

## <a name="alternatives"></a><span data-ttu-id="a48ab-135">替代方案</span><span class="sxs-lookup"><span data-stu-id="a48ab-135">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="a48ab-136">使用者可以引進 GetPinnableReference 或類似的成員，並將其當做</span><span class="sxs-lookup"><span data-stu-id="a48ab-136">Users can introduce GetPinnableReference or similar member and use it as</span></span>
 
```csharp
fixed(byte* ptr = thing.GetPinnableReference())
{ 
    // <BODY>
}
```

<span data-ttu-id="a48ab-137">如果需要替代方案，則沒有解決方案可供 `System.String`。</span><span class="sxs-lookup"><span data-stu-id="a48ab-137">There is no solution for `System.String` if alternative solution is desired.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="a48ab-138">未解決的問題</span><span class="sxs-lookup"><span data-stu-id="a48ab-138">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="a48ab-139">[] 處於「空白」狀態的行為。</span><span class="sxs-lookup"><span data-stu-id="a48ab-139">[ ] Behavior in "empty" state.</span></span><span data-ttu-id="a48ab-140"> - `nullptr` 或 `undefined`？</span><span class="sxs-lookup"><span data-stu-id="a48ab-140"> - `nullptr` or `undefined` ?</span></span> 
- <span data-ttu-id="a48ab-141">[] 應該考慮擴充方法嗎？</span><span class="sxs-lookup"><span data-stu-id="a48ab-141">[ ] Should the extension methods be considered ?</span></span> 
- <span data-ttu-id="a48ab-142">[] 如果在 `System.String`上偵測到模式，它是否會獲勝？</span><span class="sxs-lookup"><span data-stu-id="a48ab-142">[ ] If a pattern is detected on `System.String`, should it win over ?</span></span> 

## <a name="design-meetings"></a><span data-ttu-id="a48ab-143">設計會議</span><span class="sxs-lookup"><span data-stu-id="a48ab-143">Design meetings</span></span>

<span data-ttu-id="a48ab-144">還沒有。</span><span class="sxs-lookup"><span data-stu-id="a48ab-144">None yet.</span></span> 
