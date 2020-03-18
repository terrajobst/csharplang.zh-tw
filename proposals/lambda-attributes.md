---
ms.openlocfilehash: 9647fff40a1e45bef917f140612ae4e91abea958
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484570"
---
# <a name="lambda-attributes"></a><span data-ttu-id="d3e78-101">Lambda 屬性</span><span class="sxs-lookup"><span data-stu-id="d3e78-101">Lambda Attributes</span></span>

* <span data-ttu-id="d3e78-102">[x] 提議</span><span class="sxs-lookup"><span data-stu-id="d3e78-102">[x] Proposed</span></span>
* <span data-ttu-id="d3e78-103">[] 原型</span><span class="sxs-lookup"><span data-stu-id="d3e78-103">[ ] Prototype</span></span>
* <span data-ttu-id="d3e78-104">[] 執行</span><span class="sxs-lookup"><span data-stu-id="d3e78-104">[ ] Implementation</span></span>
* <span data-ttu-id="d3e78-105">[] 規格</span><span class="sxs-lookup"><span data-stu-id="d3e78-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="d3e78-106">摘要</span><span class="sxs-lookup"><span data-stu-id="d3e78-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="d3e78-107">允許將屬性套用至 lambda （和匿名方法）和 lambda/匿名方法參數，因為它們可以在一般方法上。</span><span class="sxs-lookup"><span data-stu-id="d3e78-107">Allow attributes to be applied to lambdas (and anonymous methods) and to lambda / anonymous method parameters, as they can be on regular methods.</span></span>

## <a name="motivation"></a><span data-ttu-id="d3e78-108">動機</span><span class="sxs-lookup"><span data-stu-id="d3e78-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="d3e78-109">兩個主要動機：</span><span class="sxs-lookup"><span data-stu-id="d3e78-109">Two primary motivations:</span></span>

1. <span data-ttu-id="d3e78-110">提供分析器在編譯時期看到的中繼資料。</span><span class="sxs-lookup"><span data-stu-id="d3e78-110">To provide metadata visible to analyzers at compile-time.</span></span>
2. <span data-ttu-id="d3e78-111">提供反映和工具在執行時間可見的中繼資料。</span><span class="sxs-lookup"><span data-stu-id="d3e78-111">To provide metadata visible to reflection and tooling at run-time.</span></span>

<span data-ttu-id="d3e78-112">做為（1）的範例：對於效能敏感的程式碼，能夠讓分析器在為關閉狀態的 lambda 配置結束和委派時旗標。</span><span class="sxs-lookup"><span data-stu-id="d3e78-112">As an example of (1): For performance-sensitive code, it is helpful to be able to have an analyzer that flags when closures and delegates are being allocated for lambdas that close over state.</span></span>  <span data-ttu-id="d3e78-113">通常這類程式碼的開發人員會離開其以避免捕捉任何狀態，因此編譯器可以產生靜態方法和方法的可快取委派，而開發人員會確保唯一關閉的狀態是 `this`，讓編譯器至少可以避免配置結束物件。</span><span class="sxs-lookup"><span data-stu-id="d3e78-113">Often a developer of such code will go out of his or her way to avoid capturing any state, so that the compiler can generate a static method and a cacheable delegate for the method, or the developer will ensure that the only state being closed over is `this`, allowing the compiler at least to avoid allocating a closure object.</span></span>  <span data-ttu-id="d3e78-114">但是，如果沒有語言支援來限制可能會捕捉到的內容，則很容易不小心關閉狀態。</span><span class="sxs-lookup"><span data-stu-id="d3e78-114">But, without language support for limiting what may be captured, it is all too easy to accidentally close over state.</span></span>  <span data-ttu-id="d3e78-115">如果開發人員可以使用屬性來標注 lambda，以指出可以關閉的狀態，就很有價值，例如：</span><span class="sxs-lookup"><span data-stu-id="d3e78-115">It would be valuable if a developer could annotate lambdas with attributes to indicate what state they're allowed to close over, for example:</span></span>

```csharp
[CaptureNone] // can't close over any instance state
[CaptureThis] // can only capture `this` and no other instance state
[CaptureAny] // can close over any instance state
```

<span data-ttu-id="d3e78-116">然後，可以在不正確地捕捉狀態時，將分析器寫入旗標，例如：</span><span class="sxs-lookup"><span data-stu-id="d3e78-116">Then an analyzer can be written to flag when state is captured incorrectly, for example:</span></span>

```csharp
var results = collection.Select([CaptureNone](i) => Process(item)); // Analyzer error: [CaptureNone] lambdas captures `this`
...
private U Process(T item) { ... }
```

## <a name="detailed-design"></a><span data-ttu-id="d3e78-117">詳細設計</span><span class="sxs-lookup"><span data-stu-id="d3e78-117">Detailed design</span></span>
[design]: #detailed-design

- <span data-ttu-id="d3e78-118">使用與一般方法相同的屬性語法時，可以在 lambda 或匿名方法的開頭套用屬性，例如：</span><span class="sxs-lookup"><span data-stu-id="d3e78-118">Using the same attribute syntax as on normal methods, attributes may be applied at the beginning of a lambda or anonymous method, for example:</span></span>

```csharp
[SomeAttribute(...)] () => { ... }
[SomeAttribute(...)] delegate (int i) { ... }
```

- <span data-ttu-id="d3e78-119">為了避免屬性套用至 lambda 方法或其中一個引數時，只有在任何引數周圍使用括弧時，才會使用屬性，例如：</span><span class="sxs-lookup"><span data-stu-id="d3e78-119">To avoid ambiguity as to whether an attribute applies to the lambda method or to one of the arguments, attributes may only be used when parens are used around any arguments, for example:</span></span>

```csharp
[SomeAttribute] i => { ... } // ERROR
[SomeAttribute] (i) => { ... } // Ok
[SomeAttribute] (int i) => { ... } // Ok
```

- <span data-ttu-id="d3e78-120">使用匿名方法時，不需要括弧，就能在 `delegate` 關鍵字之前將屬性套用至方法，例如：</span><span class="sxs-lookup"><span data-stu-id="d3e78-120">With anonymous methods, parens are not needed in order to apply an attribute to the method before the `delegate` keyword, for example:</span></span>

```csharp
[SomeAttribute] delegate { ... } // Ok
[SomeAttribute] delegate (int i) => { ... } // Ok
```

- <span data-ttu-id="d3e78-121">您可以透過標準的逗點分隔語法或完整屬性語法來套用多個屬性，例如：</span><span class="sxs-lookup"><span data-stu-id="d3e78-121">Multiple attributes may be applied, either via standard comma-delimited syntax or via full-attribute syntax, for example:</span></span>

```csharp
[FirstAttribute, SecondAttribute] (i) => { ... } // Ok
[FirstAttribute] [SecondAttribute] (i) => { .... } // Ok
```

- <span data-ttu-id="d3e78-122">屬性可以套用至匿名方法或 lambda 的參數，但僅限於在任何引數周圍使用括弧時，例如：</span><span class="sxs-lookup"><span data-stu-id="d3e78-122">Attributes may be applied to the parameters to an anonymous method or lambda, but only when parens are used around any arguments, for example:</span></span>

```csharp
[SomeAttribute] i => { ... } // ERROR
([SomeAttribute] i) => { .... } // Ok
([SomeAttribute] int i) => { ... } // Ok
([SomeAttribute] i, [SomeOtherAttribute] j) => { ... } // Ok
```

- <span data-ttu-id="d3e78-123">您可以使用逗號分隔或完整屬性語法，將多個屬性套用至匿名方法或 lambda 的參數，例如：</span><span class="sxs-lookup"><span data-stu-id="d3e78-123">Multiple attributes may be applied to the parameters of an anonymous method or lambda, using either the comma-delimited or full-attribute syntax, for example:</span></span>

```csharp
([FirstAttribute, SecondAttribute] i) => { ... } // Ok
([FirstAttribute] [SecondAttribute] i) => { ... } // Ok
```

- <span data-ttu-id="d3e78-124">以 `return`為目標的屬性也可以在 lambda 上使用，例如：</span><span class="sxs-lookup"><span data-stu-id="d3e78-124">`return`-targeted attributes may also be used on lambdas, for example:</span></span>

```csharp
([return: SomeAttribute] (i) => { ... }) // Ok
```

- <span data-ttu-id="d3e78-125">編譯器會將屬性輸出到產生的方法和這些方法的引數，就如同任何其他方法一樣。</span><span class="sxs-lookup"><span data-stu-id="d3e78-125">The compiler outputs the attributes onto the generated method and arguments to those methods as it would for any other method.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="d3e78-126">缺點</span><span class="sxs-lookup"><span data-stu-id="d3e78-126">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="d3e78-127">n/a</span><span class="sxs-lookup"><span data-stu-id="d3e78-127">n/a</span></span>

## <a name="alternatives"></a><span data-ttu-id="d3e78-128">替代方案</span><span class="sxs-lookup"><span data-stu-id="d3e78-128">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="d3e78-129">n/a</span><span class="sxs-lookup"><span data-stu-id="d3e78-129">n/a</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="d3e78-130">未解決的問題</span><span class="sxs-lookup"><span data-stu-id="d3e78-130">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="d3e78-131">n/a</span><span class="sxs-lookup"><span data-stu-id="d3e78-131">n/a</span></span>

## <a name="design-meetings"></a><span data-ttu-id="d3e78-132">設計會議</span><span class="sxs-lookup"><span data-stu-id="d3e78-132">Design meetings</span></span>

<span data-ttu-id="d3e78-133">n/a</span><span class="sxs-lookup"><span data-stu-id="d3e78-133">n/a</span></span>