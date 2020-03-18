---
ms.openlocfilehash: 392d932459ff0a4cb0d6d32c1606f73f9b913c68
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484451"
---
# <a name="covariant-return-types"></a><span data-ttu-id="87b6b-101">協變數傳回類型</span><span class="sxs-lookup"><span data-stu-id="87b6b-101">covariant return types</span></span>

* <span data-ttu-id="87b6b-102">[x] 提議</span><span class="sxs-lookup"><span data-stu-id="87b6b-102">[x] Proposed</span></span>
* <span data-ttu-id="87b6b-103">[] 原型：未啟動</span><span class="sxs-lookup"><span data-stu-id="87b6b-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="87b6b-104">[] 執行：未啟動</span><span class="sxs-lookup"><span data-stu-id="87b6b-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="87b6b-105">[] 規格：未啟動</span><span class="sxs-lookup"><span data-stu-id="87b6b-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="87b6b-106">摘要</span><span class="sxs-lookup"><span data-stu-id="87b6b-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="87b6b-107">支援_協_變數傳回類型。</span><span class="sxs-lookup"><span data-stu-id="87b6b-107">Support _covariant return types_.</span></span> <span data-ttu-id="87b6b-108">具體而言，允許覆寫方法擁有比它所覆寫的方法更多的衍生參考型別。</span><span class="sxs-lookup"><span data-stu-id="87b6b-108">Specifically, allow an overriding method to have a more derived reference type than the method it overrides.</span></span>

## <a name="motivation"></a><span data-ttu-id="87b6b-109">動機</span><span class="sxs-lookup"><span data-stu-id="87b6b-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="87b6b-110">這是程式碼中常見的模式，不同的方法名稱必須產生才能解決語言條件約束，覆寫必須傳回與覆寫方法相同的類型。</span><span class="sxs-lookup"><span data-stu-id="87b6b-110">It is a common pattern in code that different method names have to be invented to work around the language constraint that overrides must return the same type as the overridden method.</span></span> <span data-ttu-id="87b6b-111">如需 Roslyn 程式碼基底的範例，請參閱下面的。</span><span class="sxs-lookup"><span data-stu-id="87b6b-111">See below for an example from the Roslyn code base.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="87b6b-112">詳細設計</span><span class="sxs-lookup"><span data-stu-id="87b6b-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="87b6b-113">支援_協_變數傳回類型。</span><span class="sxs-lookup"><span data-stu-id="87b6b-113">Support _covariant return types_.</span></span> <span data-ttu-id="87b6b-114">具體而言，允許覆寫方法擁有比它所覆寫的方法更多的衍生參考型別。</span><span class="sxs-lookup"><span data-stu-id="87b6b-114">Specifically, allow an overriding method to have a more derived reference type than the method it overrides.</span></span> <span data-ttu-id="87b6b-115">這會套用至方法和屬性，並在類別和介面中受到支援。</span><span class="sxs-lookup"><span data-stu-id="87b6b-115">This would apply to methods and properties, and be supported in classes and interfaces.</span></span>

<span data-ttu-id="87b6b-116">這在 factory 模式中很有用。</span><span class="sxs-lookup"><span data-stu-id="87b6b-116">This would be useful in the factory pattern.</span></span> <span data-ttu-id="87b6b-117">例如，在 Roslyn 程式碼基底中，我們會</span><span class="sxs-lookup"><span data-stu-id="87b6b-117">For example, in the Roslyn code base we would have</span></span>

``` cs
class Compilation ...
{
    virtual Compilation WithOptions(Options options)...
}
```

``` cs
class CSharpCompilation : Compilation
{
    override CSharpCompilation WithOptions(Options options)...
}
```

<span data-ttu-id="87b6b-118">這是為了讓編譯器將覆寫方法發出為「新的」虛擬方法，以隱藏基類方法，以及使用衍生類別方法的呼叫來實作為基底類別方法的_橋接器方法_。</span><span class="sxs-lookup"><span data-stu-id="87b6b-118">The implementation of this would be for the compiler to emit the overriding method as a "new" virtual method that hides the base class method, along with a _bridge method_ that implements the base class method with a call to the derived class method.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="87b6b-119">缺點</span><span class="sxs-lookup"><span data-stu-id="87b6b-119">Drawbacks</span></span>
[drawbacks]: #drawbacks

- <span data-ttu-id="87b6b-120">[] 每一種語言變更都必須支付自己的費用。</span><span class="sxs-lookup"><span data-stu-id="87b6b-120">[ ] Every language change must pay for itself.</span></span>
- <span data-ttu-id="87b6b-121">[] 我們應確保效能合理，即使在深層繼承階層架構中也一樣</span><span class="sxs-lookup"><span data-stu-id="87b6b-121">[ ] We should ensure that the performance is reasonable, even in the case of deep inheritance hierarchies</span></span>
- <span data-ttu-id="87b6b-122">[] 我們應該確保轉譯策略的成品不會影響語言的語義，即使從舊的編譯器取用新的 IL 也一樣。</span><span class="sxs-lookup"><span data-stu-id="87b6b-122">[ ] We should ensure that artifacts of the translation strategy do not affect language semantics, even when consuming new IL from old compilers.</span></span>

## <a name="alternatives"></a><span data-ttu-id="87b6b-123">替代方案</span><span class="sxs-lookup"><span data-stu-id="87b6b-123">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="87b6b-124">我們可以稍微放寬語言規則，讓來源中的</span><span class="sxs-lookup"><span data-stu-id="87b6b-124">We could relax the language rules slightly to allow, in source,</span></span>

```csharp
abstract class Cloneable
{
    public abstract Cloneable Clone();
}

class Digit : Cloneable
{
    public override Cloneable Clone()
    {
        return this.Clone();
    }

    public new Digit Clone() // Error: 'Digit' already defines a member called 'Clone' with the same parameter types
    {
        return this;
    }
}
```

## <a name="unresolved-questions"></a><span data-ttu-id="87b6b-125">未解決的問題</span><span class="sxs-lookup"><span data-stu-id="87b6b-125">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="87b6b-126">[] 如何使用此功能編譯的 Api 適用于舊版的語言？</span><span class="sxs-lookup"><span data-stu-id="87b6b-126">[ ] How will APIs that have been compiled to use this feature work in older versions of the language?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="87b6b-127">設計會議</span><span class="sxs-lookup"><span data-stu-id="87b6b-127">Design meetings</span></span>

<span data-ttu-id="87b6b-128">還沒有。</span><span class="sxs-lookup"><span data-stu-id="87b6b-128">None yet.</span></span> <span data-ttu-id="87b6b-129"><https://github.com/dotnet/roslyn/issues/357>上有一些討論。</span><span class="sxs-lookup"><span data-stu-id="87b6b-129">There has been some discussion at <https://github.com/dotnet/roslyn/issues/357>.</span></span>