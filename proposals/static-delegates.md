---
ms.openlocfilehash: a8822137c85f449444ed675c6f2912315c041691
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484472"
---
# <a name="static-delegates"></a><span data-ttu-id="78b64-101">靜態委派</span><span class="sxs-lookup"><span data-stu-id="78b64-101">Static Delegates</span></span>

* <span data-ttu-id="78b64-102">[x] 提議</span><span class="sxs-lookup"><span data-stu-id="78b64-102">[x] Proposed</span></span>
* <span data-ttu-id="78b64-103">[] 原型：未啟動</span><span class="sxs-lookup"><span data-stu-id="78b64-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="78b64-104">[] 執行：未啟動</span><span class="sxs-lookup"><span data-stu-id="78b64-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="78b64-105">[] 規格：未啟動</span><span class="sxs-lookup"><span data-stu-id="78b64-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="78b64-106">摘要</span><span class="sxs-lookup"><span data-stu-id="78b64-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="78b64-107">提供一般用途、輕量的C#回呼功能給語言。</span><span class="sxs-lookup"><span data-stu-id="78b64-107">Provide a general-purpose, lightweight callback capability to the C# language.</span></span>

## <a name="motivation"></a><span data-ttu-id="78b64-108">動機</span><span class="sxs-lookup"><span data-stu-id="78b64-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="78b64-109">目前，使用者能夠使用 `System.Delegate` 類型來建立回呼。</span><span class="sxs-lookup"><span data-stu-id="78b64-109">Today, users have the ability to create callbacks using the `System.Delegate` type.</span></span> <span data-ttu-id="78b64-110">不過，這些都是相當龐大的（例如，需要堆積配置，並一律處理將回呼連結在一起）。</span><span class="sxs-lookup"><span data-stu-id="78b64-110">However, these are fairly heavyweight (such as requiring a heap allocation and always having handling for chaining callbacks together).</span></span>

<span data-ttu-id="78b64-111">此外，`System.Delegate` 不會提供使用非受控函式指標的最佳互通性，也就是因為不是全像的，而且會在它跨越受控/非受控界限時要求封送處理。</span><span class="sxs-lookup"><span data-stu-id="78b64-111">Additionally, `System.Delegate` does not provide the best interop with unmanaged function pointers, namely due being non-blittable and requiring marshalling anytime it crosses the managed/unmanaged boundary.</span></span>

<span data-ttu-id="78b64-112">只要稍加調整，我們就可以使用原生程式碼，提供輕量、一般用途及 interops 的新委派類型。</span><span class="sxs-lookup"><span data-stu-id="78b64-112">With a few minor tweaks, we could provide a new type of delegate that is lightweight, general-purpose, and interops well with native code.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="78b64-113">詳細設計</span><span class="sxs-lookup"><span data-stu-id="78b64-113">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="78b64-114">其中一個會透過下列方式宣告靜態委派：</span><span class="sxs-lookup"><span data-stu-id="78b64-114">One would declare a static delegate via the following:</span></span>

```C#
static delegate int Func()
```

<span data-ttu-id="78b64-115">其中一個可以額外使用類似于 `System.Runtime.InteropServices.UnmanagedFunctionPointer` 的宣告來設定宣告，以便控制呼叫慣例、字串封送處理和設定最後一次錯誤行為。</span><span class="sxs-lookup"><span data-stu-id="78b64-115">One could additionally attribute the declaration with something similar to `System.Runtime.InteropServices.UnmanagedFunctionPointer` so that the calling convention, string marshalling, and set last error behavior can be controlled.</span></span> <span data-ttu-id="78b64-116">注意：使用 `System.Runtime.InteropServices.UnmanagedFunctionPointer` 本身將無法執行，因為它僅適用于委派。</span><span class="sxs-lookup"><span data-stu-id="78b64-116">NOTE: Using `System.Runtime.InteropServices.UnmanagedFunctionPointer` itself will not work, as it is only usable on Delegates.</span></span>

<span data-ttu-id="78b64-117">宣告會轉譯成編譯器的內部標記法，如下所示</span><span class="sxs-lookup"><span data-stu-id="78b64-117">The declaration would get translated into an internal representation by the compiler that is similar to the following</span></span>

```C#
struct <Func>e__StaticDelegate
{
    IntPtr pFunction;

    static int WellKnownCompilerName();
}
```

<span data-ttu-id="78b64-118">也就是說，它是由具有 `IntPtr` 類型之單一成員的結構內部表示（這類結構是可直接完成的，而且不會產生任何堆積配置）：</span><span class="sxs-lookup"><span data-stu-id="78b64-118">That is to say, it is internally represented by a struct that has a single member of type `IntPtr` (such a struct is blittable and does not incur any heap allocations):</span></span>
* <span data-ttu-id="78b64-119">成員包含要做為回呼的函式位址。</span><span class="sxs-lookup"><span data-stu-id="78b64-119">The member contains the address of the function that is to be the callback.</span></span>
* <span data-ttu-id="78b64-120">型別會宣告符合回呼之方法簽章的方法。</span><span class="sxs-lookup"><span data-stu-id="78b64-120">The type declares a method matching the method signature of the callback.</span></span>
* <span data-ttu-id="78b64-121">結構的名稱不應為使用者可建構（與其他內部產生的支援結構相同）。</span><span class="sxs-lookup"><span data-stu-id="78b64-121">The name of the struct should not be user-constructible (as we do with other internally generated backing structures).</span></span>
 * <span data-ttu-id="78b64-122">例如：固定大小緩衝區會產生名稱格式為 `<name>e__FixedBuffer` 的結構（`<`，而 `>` 是識別碼的一部分，並使識別碼不可建構在中C#，但在 IL 中仍然可用）。</span><span class="sxs-lookup"><span data-stu-id="78b64-122">For example: fixed size buffers generate a struct with a name in the format of `<name>e__FixedBuffer` (`<` and `>` are part of the identifier and make the identifier not constructible in C#, but still useable in IL).</span></span>
* <span data-ttu-id="78b64-123">方法宣告的名稱應該是在所有靜態委派類型上使用的知名名稱（這可讓編譯器知道在判斷簽章時所要尋找的名稱）。</span><span class="sxs-lookup"><span data-stu-id="78b64-123">The name of the method declaration should be a well known name used across all static delegate types (this allows the compiler to know the name to look for when determining the signature).</span></span>

<span data-ttu-id="78b64-124">靜態委派的值只能系結至符合回呼簽章的靜態方法。</span><span class="sxs-lookup"><span data-stu-id="78b64-124">The value of the static delegate can only be bound to a static method that matches the signature of the callback.</span></span>

<span data-ttu-id="78b64-125">不支援將回呼連結在一起。</span><span class="sxs-lookup"><span data-stu-id="78b64-125">Chaining callbacks together is not supported.</span></span>

<span data-ttu-id="78b64-126">回呼的調用會由 `calli` 指令來執行。</span><span class="sxs-lookup"><span data-stu-id="78b64-126">Invocation of the callback would be implemented by the `calli` instruction.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="78b64-127">缺點</span><span class="sxs-lookup"><span data-stu-id="78b64-127">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="78b64-128">靜態委派不適用於使用一般委派的現有 Api （其中一個必須在相同簽章的一般委派中包裝所謂的靜態委派）。</span><span class="sxs-lookup"><span data-stu-id="78b64-128">Static Delegates would not work with existing APIs that use regular delegates (one would need to wrap said static delegate in a regular delegate of the same signature).</span></span>
* <span data-ttu-id="78b64-129">假設 `System.Delegate` 在內部表示為一組 `object` 和 `IntPtr` 欄位（ http://source.dot.net/#System.Private.CoreLib/src/System/Delegate.cs)，則可以允許將靜態委派隱含轉換成具有相符方法簽章的 `System.Delegate`。</span><span class="sxs-lookup"><span data-stu-id="78b64-129">Given that `System.Delegate` is represented internally as a set of `object` and `IntPtr` fields (http://source.dot.net/#System.Private.CoreLib/src/System/Delegate.cs), it would be possible to allow implicit conversion of a static delegate to a `System.Delegate` that has a matching method signature.</span></span> <span data-ttu-id="78b64-130">您也可以在相反的方向提供明確的轉換，前提是 `System.Delegate` 符合靜態委派的所有需求。</span><span class="sxs-lookup"><span data-stu-id="78b64-130">It would also be possible to provide an explicit conversion in the opposite direction, provided the `System.Delegate` conformed to all the requirements of being a static delegate.</span></span>

<span data-ttu-id="78b64-131">需要額外的工作，才能讓靜態委派在核心架構中立即可用。</span><span class="sxs-lookup"><span data-stu-id="78b64-131">Additional work would be needed to make Static Delegate readily usable in the core framework.</span></span>

## <a name="alternatives"></a><span data-ttu-id="78b64-132">替代方案</span><span class="sxs-lookup"><span data-stu-id="78b64-132">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="78b64-133">TBD</span><span class="sxs-lookup"><span data-stu-id="78b64-133">TBD</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="78b64-134">未解決的問題</span><span class="sxs-lookup"><span data-stu-id="78b64-134">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="78b64-135">設計的哪些部分仍待 TBD？</span><span class="sxs-lookup"><span data-stu-id="78b64-135">What parts of the design are still TBD?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="78b64-136">設計會議</span><span class="sxs-lookup"><span data-stu-id="78b64-136">Design meetings</span></span>

<span data-ttu-id="78b64-137">連結至會影響此提案的設計附注，並針對它們所導致的變更，逐一說明一個句子。</span><span class="sxs-lookup"><span data-stu-id="78b64-137">Link to design notes that affect this proposal, and describe in one sentence for each what changes they led to.</span></span>


