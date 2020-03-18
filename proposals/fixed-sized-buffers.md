---
ms.openlocfilehash: b51f27b2f58fd19851c80beb9cedcbd32b80b165
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484584"
---
# <a name="fixed-sized-buffers"></a><span data-ttu-id="d7e5d-101">固定大小的緩衝區</span><span class="sxs-lookup"><span data-stu-id="d7e5d-101">Fixed Sized Buffers</span></span>

* <span data-ttu-id="d7e5d-102">[x] 提議</span><span class="sxs-lookup"><span data-stu-id="d7e5d-102">[x] Proposed</span></span>
* <span data-ttu-id="d7e5d-103">[] 原型：未啟動</span><span class="sxs-lookup"><span data-stu-id="d7e5d-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="d7e5d-104">[] 執行：未啟動</span><span class="sxs-lookup"><span data-stu-id="d7e5d-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="d7e5d-105">[] 規格：未啟動</span><span class="sxs-lookup"><span data-stu-id="d7e5d-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="d7e5d-106">摘要</span><span class="sxs-lookup"><span data-stu-id="d7e5d-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="d7e5d-107">提供一般用途和安全的機制，將固定大小的緩衝區宣告為C#語言。</span><span class="sxs-lookup"><span data-stu-id="d7e5d-107">Provide a general-purpose and safe mechanism for declaring fixed sized buffers to the C# language.</span></span>

## <a name="motivation"></a><span data-ttu-id="d7e5d-108">動機</span><span class="sxs-lookup"><span data-stu-id="d7e5d-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="d7e5d-109">目前，使用者能夠在不安全的內容中建立固定大小的緩衝區。</span><span class="sxs-lookup"><span data-stu-id="d7e5d-109">Today, users have the ability to create fixed-sized buffers in an unsafe-context.</span></span> <span data-ttu-id="d7e5d-110">不過，這需要使用者處理指標、手動執行界限檢查，而且只支援一組有限的類型（`bool`、`byte`、`char`、`short`、`int`、`long`、`sbyte`、`ushort`、`uint`、`ulong`、`float`和 `double`）。</span><span class="sxs-lookup"><span data-stu-id="d7e5d-110">However, this requires the user to deal with pointers, manually perform bounds checks, and only supports a limited set of types (`bool`, `byte`, `char`, `short`, `int`, `long`, `sbyte`, `ushort`, `uint`, `ulong`, `float`, and `double`).</span></span>

<span data-ttu-id="d7e5d-111">最常見的抱怨是，固定大小的緩衝區無法在安全的程式碼中編制索引。</span><span class="sxs-lookup"><span data-stu-id="d7e5d-111">The most common complaint is that fixed-size buffers cannot be indexed in safe code.</span></span> <span data-ttu-id="d7e5d-112">第二個無法使用更多類型。</span><span class="sxs-lookup"><span data-stu-id="d7e5d-112">Inability to use more types is the second.</span></span>

<span data-ttu-id="d7e5d-113">有幾個次要調整，我們可以提供一般用途的固定大小緩衝區，以支援任何類型、可用於安全內容中，以及執行自動界限檢查。</span><span class="sxs-lookup"><span data-stu-id="d7e5d-113">With a few minor tweaks, we could provide general-purpose fixed-sized buffers which support any type, can be used in a safe context, and have automatic bounds checking performed.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="d7e5d-114">詳細設計</span><span class="sxs-lookup"><span data-stu-id="d7e5d-114">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="d7e5d-115">其中一個會透過下列方式宣告安全的固定大小緩衝區：</span><span class="sxs-lookup"><span data-stu-id="d7e5d-115">One would declare a safe fixed-sized buffer via the following:</span></span>

```csharp
public fixed DXGI_RGB GammaCurve[1025];
```

<span data-ttu-id="d7e5d-116">宣告會轉譯成編譯器的內部標記法，如下所示</span><span class="sxs-lookup"><span data-stu-id="d7e5d-116">The declaration would get translated into an internal representation by the compiler that is similar to the following</span></span>

```csharp
[FixedBuffer(typeof(DXGI_RGB), 1024)]
public ConsoleApp1.<Buffer>e__FixedBuffer_1024<DXGI_RGB> GammaCurve;

// Pack = 0 is the default packing and should result in indexable layout.
[CompilerGenerated, UnsafeValueType, StructLayout(LayoutKind.Sequential, Pack = 0)]
struct <Buffer>e__FixedBuffer_1024<T>
{
    private T _e0;
    private T _e1;
    // _e2 ... _e1023
    private T _e1024;

    public ref T this[int index] => ref (uint)index <= 1024u ?
                                         ref RefAdd<T>(ref _e0, index):
                                         throw new IndexOutOfRange();
}
```

<span data-ttu-id="d7e5d-117">因為這類固定大小的緩衝區不再需要使用 `fixed`，所以允許任何元素類型是合理的。</span><span class="sxs-lookup"><span data-stu-id="d7e5d-117">Since such fixed-sized buffers no longer require use of `fixed`, it makes sense to allow any element type.</span></span>  

> <span data-ttu-id="d7e5d-118">注意：仍會支援 `fixed`，但只有在元素類型為 `blittable`</span><span class="sxs-lookup"><span data-stu-id="d7e5d-118">NOTE: `fixed` will still be supported, but only if the element type is `blittable`</span></span>

## <a name="drawbacks"></a><span data-ttu-id="d7e5d-119">缺點</span><span class="sxs-lookup"><span data-stu-id="d7e5d-119">Drawbacks</span></span>
[drawbacks]: #drawbacks

* <span data-ttu-id="d7e5d-120">回溯相容性可能會有一些挑戰，但假設現有的固定大小緩衝區僅適用于基本類型的選取專案，則編譯器可能會在使用者將固定緩衝區視為時，繼續「單純」滑鼠.</span><span class="sxs-lookup"><span data-stu-id="d7e5d-120">There could be some challenges with backwards compatibility, but given that the existing fixed-sized buffers only work with a selection of primitive types, it should be possible for the compiler to continue "just-working" if the user treats the fixed-buffer as a pointer.</span></span>
* <span data-ttu-id="d7e5d-121">不相容的結構可能需要使用稍微不同的 `v2` 編碼來隱藏舊編譯器中的欄位。</span><span class="sxs-lookup"><span data-stu-id="d7e5d-121">Incompatible constructs may need to use slightly different `v2` encoding to hide the fields from old compiler.</span></span>
* <span data-ttu-id="d7e5d-122">在泛型型別的 IL 規格中，未妥善定義封裝。</span><span class="sxs-lookup"><span data-stu-id="d7e5d-122">Packing is not well defined in IL spec for generic types.</span></span> <span data-ttu-id="d7e5d-123">雖然此方法應該可行，但我們仍會相鄰未記載的行為。</span><span class="sxs-lookup"><span data-stu-id="d7e5d-123">While the approach should work, we will be bordering on undocumented behavior.</span></span> <span data-ttu-id="d7e5d-124">我們應該將其記載，並確定其他 JITs （例如 Mono）具有相同的行為。</span><span class="sxs-lookup"><span data-stu-id="d7e5d-124">We should make that documented and make sure other JITs like Mono have the same behavior.</span></span>
* <span data-ttu-id="d7e5d-125">為每個長度指定不同的類型（可能是 `readonly` 欄位的另一個）會對中繼資料造成影響。</span><span class="sxs-lookup"><span data-stu-id="d7e5d-125">Specifying a separate type for every length (an possibly another for `readonly` fields, if supported) will have impact on metadata.</span></span> <span data-ttu-id="d7e5d-126">它會受限於給定應用程式中不同大小的陣列數目。</span><span class="sxs-lookup"><span data-stu-id="d7e5d-126">It will be bound by the number of arrays of different sizes in the given app.</span></span>
* <span data-ttu-id="d7e5d-127">`ref` math 無法正式驗證（因為它不安全）。</span><span class="sxs-lookup"><span data-stu-id="d7e5d-127">`ref` math is not formally verifiable (since it is unsafe).</span></span> <span data-ttu-id="d7e5d-128">我們需要尋找更新驗證規則的方法，以瞭解我們的使用方式良好。</span><span class="sxs-lookup"><span data-stu-id="d7e5d-128">We will need to find a way to update verification rules to know that our use is ok.</span></span>

## <a name="alternatives"></a><span data-ttu-id="d7e5d-129">替代方案</span><span class="sxs-lookup"><span data-stu-id="d7e5d-129">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="d7e5d-130">手動宣告您的結構，並使用不安全的程式碼來建立索引子。</span><span class="sxs-lookup"><span data-stu-id="d7e5d-130">Manually declare your structures and use unsafe code to construct indexers.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="d7e5d-131">未解決的問題</span><span class="sxs-lookup"><span data-stu-id="d7e5d-131">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="d7e5d-132">我們是否應該允許 `readonly`？</span><span class="sxs-lookup"><span data-stu-id="d7e5d-132">should we allow `readonly`?</span></span>  <span data-ttu-id="d7e5d-133">（使用 readonly 索引子）</span><span class="sxs-lookup"><span data-stu-id="d7e5d-133">(with readonly indexer)</span></span>
- <span data-ttu-id="d7e5d-134">我們應該允許陣列初始化運算式嗎？</span><span class="sxs-lookup"><span data-stu-id="d7e5d-134">should we allow array initializers?</span></span>
- <span data-ttu-id="d7e5d-135">是否需要 `fixed` 關鍵字？</span><span class="sxs-lookup"><span data-stu-id="d7e5d-135">is `fixed` keyword necessary?</span></span>
- <span data-ttu-id="d7e5d-136">`foreach`?</span><span class="sxs-lookup"><span data-stu-id="d7e5d-136">`foreach`?</span></span>
- <span data-ttu-id="d7e5d-137">只有結構中的實例欄位？</span><span class="sxs-lookup"><span data-stu-id="d7e5d-137">only instance fields in structs?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="d7e5d-138">設計會議</span><span class="sxs-lookup"><span data-stu-id="d7e5d-138">Design meetings</span></span>

<span data-ttu-id="d7e5d-139">連結至會影響此提案的設計附注，並針對它們所導致的變更，逐一說明一個句子。</span><span class="sxs-lookup"><span data-stu-id="d7e5d-139">Link to design notes that affect this proposal, and describe in one sentence for each what changes they led to.</span></span>