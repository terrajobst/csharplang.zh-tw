---
ms.openlocfilehash: 922353d043653ddb651534a01f3fb98f85cd756e
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484479"
---
# <a name="readonly-locals-and-parameters"></a><span data-ttu-id="818b7-101">readonly 區域變數和參數</span><span class="sxs-lookup"><span data-stu-id="818b7-101">readonly locals and parameters</span></span>

* <span data-ttu-id="818b7-102">[x] 提議</span><span class="sxs-lookup"><span data-stu-id="818b7-102">[x] Proposed</span></span>
* <span data-ttu-id="818b7-103">[] 原型</span><span class="sxs-lookup"><span data-stu-id="818b7-103">[ ] Prototype</span></span>
* <span data-ttu-id="818b7-104">[] 執行</span><span class="sxs-lookup"><span data-stu-id="818b7-104">[ ] Implementation</span></span>
* <span data-ttu-id="818b7-105">[] 規格</span><span class="sxs-lookup"><span data-stu-id="818b7-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="818b7-106">摘要</span><span class="sxs-lookup"><span data-stu-id="818b7-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="818b7-107">允許將區域變數和參數標注為 readonly，以防止這些區域變數和參數的淺層變化。</span><span class="sxs-lookup"><span data-stu-id="818b7-107">Allow locals and parameters to be annotated as readonly in order to prevent shallow mutation of those locals and parameters.</span></span>

## <a name="motivation"></a><span data-ttu-id="818b7-108">動機</span><span class="sxs-lookup"><span data-stu-id="818b7-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="818b7-109">現在，`readonly` 關鍵字可以套用至欄位;這項功能的作用是確保欄位只能在結構中寫入（在靜態欄位的情況下為靜態結構，或在實例欄位的情況下為實例結構），這可協助開發人員不小心覆寫不應修改的狀態，以避免錯誤。</span><span class="sxs-lookup"><span data-stu-id="818b7-109">Today, the `readonly` keyword can be applied to fields; this has the effect of ensuring that a field can only be written to during construction (static construction in the case of a static field, or instance construction in the case of an instance field), which helps developers avoid mistakes by accidentally overwriting state which should not be modified.</span></span> <span data-ttu-id="818b7-110">但欄位並不是開發人員想要確保值不會變化的唯一位置。</span><span class="sxs-lookup"><span data-stu-id="818b7-110">But fields aren't the only places developers want to ensure that values aren't mutated.</span></span> <span data-ttu-id="818b7-111">特別是，通常會建立本機變數來儲存暫時性狀態，而且不小心更新暫時狀態可能會導致錯誤的計算和其他這類錯誤，尤其是在 lambda 中捕捉這類「區域變數」時，就會將這些提升欄位標記為 `readonly`。</span><span class="sxs-lookup"><span data-stu-id="818b7-111">In particular, it's common to create a local variable to store temporary state, and accidentally updating that temporary state can result in erroneous calculations and other such bugs, especially when such "locals" are captured in lambdas, at which point they are lifted to fields, but there's no way today to mark such lifted fields as `readonly`.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="818b7-112">詳細設計</span><span class="sxs-lookup"><span data-stu-id="818b7-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="818b7-113">區域變數也會可注釋為 `readonly`，讓編譯器確保它們只在宣告時設定（中C#的特定區域變數已隱含唯讀，例如 ' foreach ' 迴圈中的反覆運算變數或 ' using ' 區塊中使用的變數，但目前開發人員無法將其他區域變數標記為 `readonly`）。</span><span class="sxs-lookup"><span data-stu-id="818b7-113">Locals will be annotatable as `readonly` as well, with the compiler ensuring that they're only set at the time of declaration (certain locals in C# are already implicitly readonly, such as the iteration variable in a 'foreach' loop or the used variable in a 'using' block, but currently a developer has no ability to mark other locals as `readonly`).</span></span> <span data-ttu-id="818b7-114">這類 `readonly` 區域變數必須有初始化運算式：</span><span class="sxs-lookup"><span data-stu-id="818b7-114">Such `readonly` locals must have an initializer:</span></span>

```csharp
readonly long maxBytesToDelete = (stream.LimitBytes - stream.MaxBytes) / 10;
...
maxBytesToDelete = 0; // Error: can't assign to readonly locals outside of declaration
```

<span data-ttu-id="818b7-115">而做為 `readonly var`的縮寫，可以使用現有的內容關鍵字 `let`，例如</span><span class="sxs-lookup"><span data-stu-id="818b7-115">And as shorthand for `readonly var`, the existing contextual keyword `let` may be used, e.g.</span></span>

```csharp
let maxBytesToDelete = (stream.LimitBytes - stream.MaxBytes) / 10;
...
maxBytesToDelete = 0; // Error: can't assign to readonly locals outside of declaration
```

<span data-ttu-id="818b7-116">初始化運算式可能沒有任何特殊的條件約束，而且可以是任何目前有效的專案做為區域變數的初始化運算式，例如</span><span class="sxs-lookup"><span data-stu-id="818b7-116">There are no special constraints for what the initializer can be, and can be anything currently valid as an initializer for locals, e.g.</span></span>

```csharp
readonly T data = arg1 ?? arg2;
```

<span data-ttu-id="818b7-117">使用 lambda 和閉時，在區域變數上 `readonly` 特別有用。</span><span class="sxs-lookup"><span data-stu-id="818b7-117">`readonly` on locals is particularly valuable when working with lambdas and closures.</span></span> <span data-ttu-id="818b7-118">當匿名方法或 lambda 從封入範圍存取本機狀態時，編譯器會將該狀態捕捉到結束項，這是由「顯示類別」所表示。</span><span class="sxs-lookup"><span data-stu-id="818b7-118">When an anonymous method or lambda accesses local state from the enclosing scope, that state is captured into a closure by the compiler, which is represented by a "display class."</span></span>  <span data-ttu-id="818b7-119">所捕捉到的每個「區域」都是此類別中的欄位，但因為編譯器會代表您產生此欄位，所以您不會有機會將它標注為 `readonly`，以防止 lambda 錯誤地寫入「區域」（在引號中，因為它其實不是本機，至少不在產生的 MSIL 中）。</span><span class="sxs-lookup"><span data-stu-id="818b7-119">Each "local" that's captured is a field in this class, yet because the compiler is generating this field on your behalf, you have no opportunity to annotate it as `readonly` in order to prevent the lambda from erroneously writing to the "local" (in quotes because it's really not a local, at least not in the resulting MSIL).</span></span> <span data-ttu-id="818b7-120">使用 `readonly` 的區域變數，編譯器可以防止 lambda 寫入至本機，這在涉及錯誤寫入的多執行緒案例中特別有用，因為這可能會導致危險但很罕見且難以發現的並行處理錯誤。</span><span class="sxs-lookup"><span data-stu-id="818b7-120">With `readonly` locals, the compiler can prevent the lambda from writing to local, which is particularly valuable in scenarios involving multithreading where an erroneous write could result in a dangerous but rare and hard-to-find concurrency bug.</span></span>

```csharp
readonly long index = ...;
Parallel.ForEach(data, item => {
    T element = item[index];
    index = 0; // Error: can't assign to readonly locals outside of declaration
});
```

<span data-ttu-id="818b7-121">以特殊形式的本機而言，參數也會可注釋為 `readonly`。</span><span class="sxs-lookup"><span data-stu-id="818b7-121">As a special form of local, parameters will also be annotatable as `readonly`.</span></span> <span data-ttu-id="818b7-122">這不會影響方法的呼叫端傳遞至參數的方式（就像沒有限制哪些值可以儲存在 `readonly` 欄位中一樣），但就像任何 `readonly` 本機一樣，編譯器會禁止程式碼在宣告之後寫入參數，這表示方法的主體禁止寫入參數。</span><span class="sxs-lookup"><span data-stu-id="818b7-122">This would have no effect on what the caller of the method is able to pass to the parameter (just as there's no constraint on what values may be stored into a `readonly` field), but as with any `readonly` local, the compiler would prohibit code from writing to the parameter after declaration, which means the body of the method is prohibited from writing to the parameter.</span></span>

```csharp
public void Update(readonly int index = 0) // Default values are ok though not required
{
    ...
    index = 0; // Error: can't assign to readonly parameters
    ...
}
```

<span data-ttu-id="818b7-123">`readonly` 參數不會影響編譯器針對該方法發出的簽章/中繼資料，而只會影響編譯器如何處理方法主體的編譯。</span><span class="sxs-lookup"><span data-stu-id="818b7-123">`readonly` parameters do not affect the signature/metadata emitted by the compiler for that method, and simply affect how the compiler handles the compilation of the method's body.</span></span> <span data-ttu-id="818b7-124">例如，基底虛擬方法可能會有 `readonly` 參數，而且該參數在覆寫中可以是可寫入的。</span><span class="sxs-lookup"><span data-stu-id="818b7-124">Thus, for example, a base virtual method could have a `readonly` parameter, and that parameter could be writable in an override.</span></span>

<span data-ttu-id="818b7-125">如同欄位，區域變數和參數的 `readonly` 是淺層，會影響儲存位置，但不會對物件圖形進行間接影響。</span><span class="sxs-lookup"><span data-stu-id="818b7-125">As with fields, `readonly` for locals and parameters is shallow, affecting the storage location but not transitively affecting the object graph.</span></span> <span data-ttu-id="818b7-126">不過，也就像欄位一樣，在 `readonly` 的本機/參數結構上呼叫方法，實際上會建立結構的複本，並在複本上呼叫方法，以避免 `this`的內部變化。</span><span class="sxs-lookup"><span data-stu-id="818b7-126">However, also as with fields, calling a method on a `readonly` local/parameter struct will actually make a copy of the struct and call the method on the copy, in order to avoid internal mutation of `this`.</span></span>

<span data-ttu-id="818b7-127">除非也支援 `ref readonly`，否則無法將 `readonly` 區域變數和參數當做 `ref` 或 `out` 引數傳遞。</span><span class="sxs-lookup"><span data-stu-id="818b7-127">`readonly` locals and parameters can't be passed as `ref` or `out` arguments, unless/until `ref readonly` is also supported.</span></span>

## <a name="alternatives"></a><span data-ttu-id="818b7-128">替代方案</span><span class="sxs-lookup"><span data-stu-id="818b7-128">Alternatives</span></span>
[alternatives]: #alternatives

- <span data-ttu-id="818b7-129">`val` 可用來做為 `let`的替代縮寫。</span><span class="sxs-lookup"><span data-stu-id="818b7-129">`val` could be used as an alternative shorthand to `let`.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="818b7-130">未解決的問題</span><span class="sxs-lookup"><span data-stu-id="818b7-130">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="818b7-131">`readonly ref` / `ref readonly` / `readonly ref readonly`：我已將有關如何處理 `ref readonly` 的問題留給這份提案。</span><span class="sxs-lookup"><span data-stu-id="818b7-131">`readonly ref` / `ref readonly` / `readonly ref readonly`: I've left the question of how to handle `ref readonly` as separate from this proposal.</span></span>
- <span data-ttu-id="818b7-132">此提案不會處理 readonly 結構/不可變的類型。</span><span class="sxs-lookup"><span data-stu-id="818b7-132">This proposal does not tackle readonly structs / immutable types.</span></span> <span data-ttu-id="818b7-133">這會保留給個別的提案。</span><span class="sxs-lookup"><span data-stu-id="818b7-133">That is left for a separate proposal.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="818b7-134">設計會議</span><span class="sxs-lookup"><span data-stu-id="818b7-134">Design meetings</span></span>

- <span data-ttu-id="818b7-135">在2015年1月21日簡短討論（<https://github.com/dotnet/roslyn/issues/98>）</span><span class="sxs-lookup"><span data-stu-id="818b7-135">Briefly discussed on Jan 21, 2015 (<https://github.com/dotnet/roslyn/issues/98>)</span></span>
