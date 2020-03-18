---
ms.openlocfilehash: 25e95b3ab8c384a7e66e59a7f9422cc9699074d7
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484717"
---
# <a name="infer-tuple-names-aka-tuple-projection-initializers"></a><span data-ttu-id="fc1db-101">推斷元組名稱（也稱為）。</span><span class="sxs-lookup"><span data-stu-id="fc1db-101">Infer tuple names (aka.</span></span> <span data-ttu-id="fc1db-102">元組投影初始化運算式）</span><span class="sxs-lookup"><span data-stu-id="fc1db-102">tuple projection initializers)</span></span>

## <a name="summary"></a><span data-ttu-id="fc1db-103">摘要</span><span class="sxs-lookup"><span data-stu-id="fc1db-103">Summary</span></span>
[summary]: #summary

<span data-ttu-id="fc1db-104">在許多常見的情況下，這項功能可省略和推斷元組專案名稱。</span><span class="sxs-lookup"><span data-stu-id="fc1db-104">In a number of common cases, this feature allows the tuple element names to be omitted and instead be inferred.</span></span> <span data-ttu-id="fc1db-105">例如，您可以從 `(x.f1, x?.f2)`推斷元素名稱 "f1" 和 "f2"，而不是輸入 `(f1: x.f1, f2: x?.f2)`。</span><span class="sxs-lookup"><span data-stu-id="fc1db-105">For instance, instead of typing `(f1: x.f1, f2: x?.f2)`, the element names "f1" and "f2" can be inferred from `(x.f1, x?.f2)`.</span></span>

<span data-ttu-id="fc1db-106">這會使匿名型別的行為更加相似，這會允許在建立期間推斷成員名稱。</span><span class="sxs-lookup"><span data-stu-id="fc1db-106">This parallels the behavior of  anonymous types, which allow inferring member names during creation.</span></span> <span data-ttu-id="fc1db-107">例如，`new { x.f1, y?.f2 }` 宣告成員 "f1" 和 "f2"。</span><span class="sxs-lookup"><span data-stu-id="fc1db-107">For instance, `new { x.f1, y?.f2 }` declares members "f1" and "f2".</span></span>

<span data-ttu-id="fc1db-108">在 LINQ 中使用元組時，這特別有用：</span><span class="sxs-lookup"><span data-stu-id="fc1db-108">This is particularly handy when using tuples in LINQ:</span></span>

```csharp
// "c" and "result" have element names "f1" and "f2"
var result = list.Select(c => (c.f1, c.f2)).Where(t => t.f2 == 1); 
```

## <a name="detailed-design"></a><span data-ttu-id="fc1db-109">詳細設計</span><span class="sxs-lookup"><span data-stu-id="fc1db-109">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="fc1db-110">變更有兩個部分：</span><span class="sxs-lookup"><span data-stu-id="fc1db-110">There are two parts to the change:</span></span>

1.  <span data-ttu-id="fc1db-111">嘗試為沒有明確名稱的每個元組元素推斷候選名稱：</span><span class="sxs-lookup"><span data-stu-id="fc1db-111">Try to infer a candidate name for each tuple element which does not have an explicit name:</span></span>
    -   <span data-ttu-id="fc1db-112">使用與匿名型別的名稱推斷相同的規則。</span><span class="sxs-lookup"><span data-stu-id="fc1db-112">Using same rules as name inference for anonymous types.</span></span>
        - <span data-ttu-id="fc1db-113">在C#中，這可允許三種情況： `y` （識別碼）、`x.y` （簡單成員存取）和 `x?.y` （條件式存取）。</span><span class="sxs-lookup"><span data-stu-id="fc1db-113">In C#, this allows three cases: `y` (identifier), `x.y` (simple member access) and `x?.y` (conditional access).</span></span>
        - <span data-ttu-id="fc1db-114">在 VB 中，這可允許其他情況，例如 `x.y()`。</span><span class="sxs-lookup"><span data-stu-id="fc1db-114">In VB, this allows for additional cases, such as `x.y()`.</span></span>
    -   <span data-ttu-id="fc1db-115">拒絕保留的元組名稱（在中C#區分大小寫，在 VB 中則不區分大小寫），因為它們被禁止或已隱含。</span><span class="sxs-lookup"><span data-stu-id="fc1db-115">Rejecting reserved tuple names (case-sensitive in C#, case-insensitive in VB), as they are either forbidden or already implicit.</span></span> <span data-ttu-id="fc1db-116">例如 `ItemN`、`Rest`和 `ToString`。</span><span class="sxs-lookup"><span data-stu-id="fc1db-116">For instance, such as `ItemN`, `Rest`, and `ToString`.</span></span>
    -   <span data-ttu-id="fc1db-117">如果任何候選名稱重複（區分大小寫C#，在 VB 中則不區分大小寫），則會捨棄這些候選項目，</span><span class="sxs-lookup"><span data-stu-id="fc1db-117">If any candidate names are duplicates (case-sensitive in C#, case-insensitive in VB) within the entire tuple, we drop those candidates,</span></span>
2.  <span data-ttu-id="fc1db-118">在轉換期間（檢查和警告從元組常值中卸載名稱），推斷的名稱不會產生任何警告。</span><span class="sxs-lookup"><span data-stu-id="fc1db-118">During conversions (which check and warn about dropping names from tuple literals), inferred names would not produce any warnings.</span></span> <span data-ttu-id="fc1db-119">這可避免中斷現有的元組程式碼。</span><span class="sxs-lookup"><span data-stu-id="fc1db-119">This avoids breaking existing tuple code.</span></span>

<span data-ttu-id="fc1db-120">請注意，處理重複專案的規則與匿名型別的規則不同。</span><span class="sxs-lookup"><span data-stu-id="fc1db-120">Note that the rule for handling duplicates is different than that for anonymous types.</span></span> <span data-ttu-id="fc1db-121">比方說，`new { x.f1, x.f1 }` 會產生錯誤，但仍然允許 `(x.f1, x.f1)` （不含任何推斷的名稱）。</span><span class="sxs-lookup"><span data-stu-id="fc1db-121">For instance, `new { x.f1, x.f1 }` produces an error, but `(x.f1, x.f1)` would still be allowed (just without any inferred names).</span></span> <span data-ttu-id="fc1db-122">這可避免中斷現有的元組程式碼。</span><span class="sxs-lookup"><span data-stu-id="fc1db-122">This avoids breaking existing tuple code.</span></span>

<span data-ttu-id="fc1db-123">為求一致，相同的適用于解構指派所產生的元組（ C#在中）：</span><span class="sxs-lookup"><span data-stu-id="fc1db-123">For consistency, the same would apply to tuples produced by deconstruction-assignments (in C#):</span></span>

```csharp
// tuple has element names "f1" and "f2" 
var tuple = ((x.f1, x?.f2) = (1, 2));
```

<span data-ttu-id="fc1db-124">同樣也適用于 VB 元組，使用 VB 特有的規則從運算式推斷名稱，並不區分大小寫的名稱比較。</span><span class="sxs-lookup"><span data-stu-id="fc1db-124">The same would also apply to VB tuples, using the VB-specific rules for inferring name from expression and case-insensitive name comparisons.</span></span>

<span data-ttu-id="fc1db-125">使用C# 7.1 編譯器（或更新版本）搭配語言版本 "7.0" 時，將會推斷專案名稱（儘管功能無法使用），但會有嘗試存取的使用網站錯誤。</span><span class="sxs-lookup"><span data-stu-id="fc1db-125">When using the C# 7.1 compiler (or later) with language version "7.0", the element names will be inferred (despite the feature not being available), but there will be a use-site error for trying to access them.</span></span> <span data-ttu-id="fc1db-126">這會限制新增新程式碼，稍後會面臨相容性問題（如下所述）。</span><span class="sxs-lookup"><span data-stu-id="fc1db-126">This will limit additions of new code that would later face the compatibility issue (described below).</span></span>

## <a name="drawbacks"></a><span data-ttu-id="fc1db-127">缺點</span><span class="sxs-lookup"><span data-stu-id="fc1db-127">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="fc1db-128">主要缺點是這會引進C# 7.0 的相容性中斷：</span><span class="sxs-lookup"><span data-stu-id="fc1db-128">The main drawback is that this introduces a compatibility break from C# 7.0:</span></span>

```csharp
Action y = () => M();
var t = (x: x, y);
t.y(); // this might have previously picked up an extension method called “y”, but would now call the lambda.
```

<span data-ttu-id="fc1db-129">相容性委員會發現這項中斷是可接受的，因為它會受到限制，而時間範圍自C#元組（7.0）則短。</span><span class="sxs-lookup"><span data-stu-id="fc1db-129">The compatibility council found this break acceptable, given that it is limited and the time window since tuples shipped (in C# 7.0) is short.</span></span>

## <a name="references"></a><span data-ttu-id="fc1db-130">參考</span><span class="sxs-lookup"><span data-stu-id="fc1db-130">References</span></span>
- [<span data-ttu-id="fc1db-131">LDM 4 月4日2017</span><span class="sxs-lookup"><span data-stu-id="fc1db-131">LDM April 4th 2017</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md#tuple-names)
- <span data-ttu-id="fc1db-132">[Github 討論](https://github.com/dotnet/csharplang/issues/370)（感謝 @alrz 讓此問題上線）</span><span class="sxs-lookup"><span data-stu-id="fc1db-132">[Github discussion](https://github.com/dotnet/csharplang/issues/370) (thanks @alrz for bringing this issue up)</span></span>
- [<span data-ttu-id="fc1db-133">元組設計</span><span class="sxs-lookup"><span data-stu-id="fc1db-133">Tuples design</span></span>](https://github.com/dotnet/roslyn/blob/master/docs/features/tuples.md)
