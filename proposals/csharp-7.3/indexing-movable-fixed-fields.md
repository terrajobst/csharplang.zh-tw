---
ms.openlocfilehash: 36f3e6204d12c2569b3a55f3a47f58337e8a08e4
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484626"
---
# <a name="indexing-fixed-fields-should-not-require-pinning-regardless-of-the-movableunmovable-context"></a><span data-ttu-id="9f5bf-101">不論可移動/不可移動的內容為何，索引 `fixed` 欄位都不應該要求釘選。</span><span class="sxs-lookup"><span data-stu-id="9f5bf-101">Indexing `fixed` fields should not require pinning regardless of the movable/unmovable context.</span></span> #

<span data-ttu-id="9f5bf-102">變更具有 bug 修正的大小。</span><span class="sxs-lookup"><span data-stu-id="9f5bf-102">The change has the size of a bug fix.</span></span> <span data-ttu-id="9f5bf-103">它可以是7.3，而且不會與我們進一步採取的任何方向衝突。</span><span class="sxs-lookup"><span data-stu-id="9f5bf-103">It can be in 7.3 and does not conflict with whatever direction we take further.</span></span>
<span data-ttu-id="9f5bf-104">這項變更只是為了讓下列案例即使 `s` 可移動，仍可正常執行。</span><span class="sxs-lookup"><span data-stu-id="9f5bf-104">This change is only about allowing the following scenario to work even though `s` is moveable.</span></span> <span data-ttu-id="9f5bf-105">當 `s` 不是可移動的時，它已經是有效的。</span><span class="sxs-lookup"><span data-stu-id="9f5bf-105">It is already valid when `s` is not moveable.</span></span> 

<span data-ttu-id="9f5bf-106">注意：不論是哪一種情況，它仍然需要 `unsafe` 內容。</span><span class="sxs-lookup"><span data-stu-id="9f5bf-106">NOTE: in either case, it still requires `unsafe` context.</span></span> <span data-ttu-id="9f5bf-107">可以讀取未初始化的資料，甚至是超出範圍。</span><span class="sxs-lookup"><span data-stu-id="9f5bf-107">It is possible to read uninitialized data or even out of range.</span></span> <span data-ttu-id="9f5bf-108">這不會變更。</span><span class="sxs-lookup"><span data-stu-id="9f5bf-108">That is not changing.</span></span>

```csharp
unsafe struct S
{
    public fixed int myFixedField[10];
}

class Program
{
    static S s;

    unsafe static void Main()
    {
        int p = s.myFixedField[5]; // indexing fixed-size array fields would be ok
    }
}
```

<span data-ttu-id="9f5bf-109">我在這裡看到的主要「挑戰」是如何說明規格中的放寬。特別是，由於下列情況仍然需要釘選。</span><span class="sxs-lookup"><span data-stu-id="9f5bf-109">The main “challenge” that I see here is how to explain the relaxation in the spec. In particular, since the following would still need pinning.</span></span> <span data-ttu-id="9f5bf-110">（因為 `s` 是可移動的，而我們明確使用欄位做為指標）</span><span class="sxs-lookup"><span data-stu-id="9f5bf-110">(because `s` is moveable and we explicitly use the field as a pointer)</span></span>

```csharp
unsafe struct S
{
    public fixed int myFixedField[10];
}

class Program
{
    static S s;

    unsafe static void Main()
    {
        int* ptr = s.myFixedField; // taking a pointer explicitly still requires pinning.
        int p = ptr[5];
    }
}
```

<span data-ttu-id="9f5bf-111">我們需要在移動時將目標釘選的其中一個原因是我們的程式碼產生策略的成品，我們一律會轉換成非受控指標，因此會強制使用者透過 `fixed` 語句來釘選。</span><span class="sxs-lookup"><span data-stu-id="9f5bf-111">One reason why we require pinning of the target when it is movable is the artifact of our code generation strategy, - we always convert to an unmanaged pointer and thus force the user to pin via `fixed` statement.</span></span> <span data-ttu-id="9f5bf-112">不過，在進行索引編制時，不需要轉換成非受控。</span><span class="sxs-lookup"><span data-stu-id="9f5bf-112">However, conversion to unmanaged is unnecessary when doing indexing.</span></span> <span data-ttu-id="9f5bf-113">當我們的接收者是 managed 指標的形式時，相同的 unsafe 指標數學也同樣適用。</span><span class="sxs-lookup"><span data-stu-id="9f5bf-113">The same unsafe pointer math is equally applicable when we have the receiver in the form of a managed pointer.</span></span> <span data-ttu-id="9f5bf-114">如果這樣做，就會管理中繼參考（GC 追蹤），而不需要釘選。</span><span class="sxs-lookup"><span data-stu-id="9f5bf-114">If we do that, then the intermediate ref is managed (GC-tracked) and the pinning is unnecessary.</span></span>

<span data-ttu-id="9f5bf-115">變更 https://github.com/dotnet/roslyn/pull/24966 是放寬此需求的原型 PR。</span><span class="sxs-lookup"><span data-stu-id="9f5bf-115">The change https://github.com/dotnet/roslyn/pull/24966 is a prototype PR that relaxes this requirement.</span></span>
