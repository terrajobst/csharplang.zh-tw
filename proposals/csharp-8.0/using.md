---
ms.openlocfilehash: 91afbc3e3412049cd183c36c8035f1862c520413
ms.sourcegitcommit: da1180f7eacdd5067b32d291a76e6764159e00fe
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2019
ms.locfileid: "79485053"
---
# <a name="pattern-based-using-and-using-declarations"></a><span data-ttu-id="fd8a2-101">「以模式為基礎的 using」和「using 宣告」</span><span class="sxs-lookup"><span data-stu-id="fd8a2-101">"pattern-based using" and "using declarations"</span></span>

## <a name="summary"></a><span data-ttu-id="fd8a2-102">摘要</span><span class="sxs-lookup"><span data-stu-id="fd8a2-102">Summary</span></span>

<span data-ttu-id="fd8a2-103">此語言會在 `using` 語句周圍加入兩項新功能，以簡化資源管理：除了 `IDisposable`，`using` 應該辨識可處置模式，並將 `using` 宣告新增至語言。</span><span class="sxs-lookup"><span data-stu-id="fd8a2-103">The language will add two new capabilities around the `using` statement in order to make resource management simpler: `using` should recognize a disposable pattern in addition to `IDisposable` and add a `using` declaration to the language.</span></span>

## <a name="motivation"></a><span data-ttu-id="fd8a2-104">動機</span><span class="sxs-lookup"><span data-stu-id="fd8a2-104">Motivation</span></span>

<span data-ttu-id="fd8a2-105">`using` 語句是現今資源管理的有效工具，但它需要相當多的儀式。</span><span class="sxs-lookup"><span data-stu-id="fd8a2-105">The `using` statement is an effective tool for resource management today but it requires quite a bit of ceremony.</span></span> <span data-ttu-id="fd8a2-106">具有多個要管理之資源的方法，可以使用一系列的 `using` 語句，在語法上很快。</span><span class="sxs-lookup"><span data-stu-id="fd8a2-106">Methods that have a number of resources to manage can get syntactically bogged down with a series of `using` statements.</span></span> <span data-ttu-id="fd8a2-107">這種語法的負擔足以讓大部分的程式碼樣式指導方針在此案例中明確地有括弧括住的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="fd8a2-107">This syntax burden is enough that most coding style guidelines explicitly have an exception around braces for this scenario.</span></span> 

<span data-ttu-id="fd8a2-108">`using` 聲明在此移除大部分的儀式，並C#與其他包含資源管理區塊的語言同等。</span><span class="sxs-lookup"><span data-stu-id="fd8a2-108">The `using` declaration removes much of the ceremony here and gets C# on par with other languages that include resource management blocks.</span></span> <span data-ttu-id="fd8a2-109">此外，以模式為基礎的 `using` 可讓開發人員擴充可參與此處的類型集合。</span><span class="sxs-lookup"><span data-stu-id="fd8a2-109">Additionally the pattern-based `using` lets developers expand the set of types that can participate here.</span></span> <span data-ttu-id="fd8a2-110">在許多情況下，不需要建立僅存在於 `using` 語句中使用值的包裝函式類型。</span><span class="sxs-lookup"><span data-stu-id="fd8a2-110">In many cases removing the need to create wrapper types that only exist to allow for a values use in a `using` statement.</span></span> 

<span data-ttu-id="fd8a2-111">這些功能一起可讓開發人員簡化和擴充可套用 `using` 的案例。</span><span class="sxs-lookup"><span data-stu-id="fd8a2-111">Together these features allow developers to simplify and expand the scenarios where `using` can be applied.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="fd8a2-112">詳細設計</span><span class="sxs-lookup"><span data-stu-id="fd8a2-112">Detailed Design</span></span> 

### <a name="using-declaration"></a><span data-ttu-id="fd8a2-113">using 宣告</span><span class="sxs-lookup"><span data-stu-id="fd8a2-113">using declaration</span></span>

<span data-ttu-id="fd8a2-114">此語言可將 `using` 新增至本機變數宣告中。</span><span class="sxs-lookup"><span data-stu-id="fd8a2-114">The language will allow for `using` to be added to a local variable declaration.</span></span> <span data-ttu-id="fd8a2-115">這類宣告會與在相同位置的 `using` 語句中宣告變數的效果相同。</span><span class="sxs-lookup"><span data-stu-id="fd8a2-115">Such a declaration will have the same effect as declaring the variable in a `using` statement at the same location.</span></span>

```csharp
if (...) 
{ 
   using FileStream f = new FileStream(@"C:\users\jaredpar\using.md");
   // statements
}

// Equivalent to 
if (...) 
{ 
   using (FileStream f = new FileStream(@"C:\users\jaredpar\using.md")) 
   {
    // statements
   }
}
```

<span data-ttu-id="fd8a2-116">`using` 本機的存留期會延伸到其宣告範圍的結尾。</span><span class="sxs-lookup"><span data-stu-id="fd8a2-116">The lifetime of a `using` local will extend to the end of the scope in which it is declared.</span></span> <span data-ttu-id="fd8a2-117">然後，`using` 的區域變數會依照其宣告的反向順序來處置。</span><span class="sxs-lookup"><span data-stu-id="fd8a2-117">The `using` locals will then be disposed in the reverse order in which they are declared.</span></span> 

```csharp
{ 
    using var f1 = new FileStream("...");
    using var f2 = new FileStream("..."), f3 = new FileStream("...");
    ...
    // Dispose f3
    // Dispose f2 
    // Dispose f1
}
```

<span data-ttu-id="fd8a2-118">在 `using` 宣告的臉部中，`goto`或任何其他控制流程結構都沒有任何限制。</span><span class="sxs-lookup"><span data-stu-id="fd8a2-118">There are no restrictions around `goto`, or any other control flow construct in the face of a `using` declaration.</span></span> <span data-ttu-id="fd8a2-119">相反地，程式碼的作用就如同對等的 `using` 語句一樣：</span><span class="sxs-lookup"><span data-stu-id="fd8a2-119">Instead the code acts just as it would for the equivalent `using` statement:</span></span>

```csharp
{
    using var f1 = new FileStream("...");
  target:
    using var f2 = new FileStream("...");
    if (someCondition) 
    {
        // Causes f2 to be disposed but has no effect on f1
        goto target;
    }
}
```

<span data-ttu-id="fd8a2-120">在 `using` 區域宣告中宣告的區域將隱含為唯讀。</span><span class="sxs-lookup"><span data-stu-id="fd8a2-120">A local declared in a `using` local declaration will be implicitly read-only.</span></span> <span data-ttu-id="fd8a2-121">這符合在 `using` 語句中宣告的區域變數行為。</span><span class="sxs-lookup"><span data-stu-id="fd8a2-121">This matches the behavior of locals declared in a `using` statement.</span></span> 

<span data-ttu-id="fd8a2-122">`using` 宣告的語言文法將如下所示：</span><span class="sxs-lookup"><span data-stu-id="fd8a2-122">The language grammar for `using` declarations will be the following:</span></span>

```antlr
local-using-declaration:
  using type using-declarators

using-declarators:
  using-declarator
  using-declarators , using-declarator
  
using-declarator:
  identifier = expression
```

<span data-ttu-id="fd8a2-123">`using` 聲明方面的限制：</span><span class="sxs-lookup"><span data-stu-id="fd8a2-123">Restrictions around `using` declaration:</span></span>

- <span data-ttu-id="fd8a2-124">可能不會直接出現在 `case` 標籤內，而是必須在 `case` 標籤內的區塊內。</span><span class="sxs-lookup"><span data-stu-id="fd8a2-124">May not appear directly inside a `case` label but instead must be within a block inside the `case` label.</span></span>
- <span data-ttu-id="fd8a2-125">可能不會顯示為 `out` 變數宣告的一部分。</span><span class="sxs-lookup"><span data-stu-id="fd8a2-125">May not appear as part of an `out` variable declaration.</span></span> 
- <span data-ttu-id="fd8a2-126">每個宣告子都必須有初始化運算式。</span><span class="sxs-lookup"><span data-stu-id="fd8a2-126">Must have an initializer for each declarator.</span></span>
- <span data-ttu-id="fd8a2-127">區欄位型別必須可以隱含地轉換成 `IDisposable` 或滿足 `using` 模式。</span><span class="sxs-lookup"><span data-stu-id="fd8a2-127">The local type must be implicitly convertible to `IDisposable` or fulfill the `using` pattern.</span></span>

### <a name="pattern-based-using"></a><span data-ttu-id="fd8a2-128">以模式為基礎的 using</span><span class="sxs-lookup"><span data-stu-id="fd8a2-128">pattern-based using</span></span>

<span data-ttu-id="fd8a2-129">此語言將會加入可處置模式的概念：這是具有可存取的 `Dispose` 實例方法的類型。</span><span class="sxs-lookup"><span data-stu-id="fd8a2-129">The language will add the notion of a disposable pattern: that is a type which has an accessible `Dispose` instance method.</span></span> <span data-ttu-id="fd8a2-130">符合可處置模式的類型可以參與 `using` 語句或宣告，而不需要執行 `IDisposable`。</span><span class="sxs-lookup"><span data-stu-id="fd8a2-130">Types which fit the disposable pattern can participate in a `using` statement or declaration without being required to implement `IDisposable`.</span></span> 

```csharp
class Resource
{ 
    public void Dispose() { ... }
}

using (var r = new Resource())
{
    // statements
}
```

<span data-ttu-id="fd8a2-131">這可讓開發人員在一些新案例中運用 `using`：</span><span class="sxs-lookup"><span data-stu-id="fd8a2-131">This will allow developers to leverage `using` in a number of new scenarios:</span></span>

- <span data-ttu-id="fd8a2-132">`ref struct`：這些類型無法立即執行介面，因此無法參與 `using` 語句。</span><span class="sxs-lookup"><span data-stu-id="fd8a2-132">`ref struct`: These types can't implement interfaces today and hence can't participate in `using` statements.</span></span>
- <span data-ttu-id="fd8a2-133">擴充方法可讓開發人員增強其他元件中的類型，以參與 `using` 語句。</span><span class="sxs-lookup"><span data-stu-id="fd8a2-133">Extension methods will allow developers to augment types in other assemblies to participate in `using` statements.</span></span>

<span data-ttu-id="fd8a2-134">在類型可以隱含地轉換成 `IDisposable` 而且也符合可處置模式的情況下，就會優先使用 `IDisposable`。</span><span class="sxs-lookup"><span data-stu-id="fd8a2-134">In the situation where a type can be implicitly converted to `IDisposable` and also fits the disposable pattern, then `IDisposable` will be preferred.</span></span> <span data-ttu-id="fd8a2-135">雖然這會採用相反的 `foreach` 方式（在介面上慣用的模式），但還是必須提供回溯相容性。</span><span class="sxs-lookup"><span data-stu-id="fd8a2-135">While this takes the opposite approach of `foreach` (pattern preferred over interface) it is necessary for backwards compatibility.</span></span>

<span data-ttu-id="fd8a2-136">也適用于傳統 `using` 語句的相同限制：在 `using` 中宣告的區域變數是唯讀的，`null` 值不會造成擲回例外狀況，依此類推 .。。程式碼產生將會不同，只有在呼叫 Dispose 之前，不會將轉換成 `IDisposable`：</span><span class="sxs-lookup"><span data-stu-id="fd8a2-136">The same restrictions from a traditional `using` statement apply here as well: local variables declared in the `using` are read-only, a `null` value will not cause an exception to be thrown, etc ... The code generation will be different only in that there will not be a cast to `IDisposable` before calling Dispose:</span></span>

```csharp
{
      Resource r = new Resource();
      try {
            // statements
      }
      finally {
            if (resource != null) resource.Dispose();
      }
}
```

<span data-ttu-id="fd8a2-137">為了符合可處置的模式，`Dispose` 的方法必須是可存取的、無參數的，而且具有 `void` 的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="fd8a2-137">In order to fit the disposable pattern the `Dispose` method must be accessible, parameterless and have a `void` return type.</span></span> <span data-ttu-id="fd8a2-138">沒有其他限制。</span><span class="sxs-lookup"><span data-stu-id="fd8a2-138">There are no other restrictions.</span></span> <span data-ttu-id="fd8a2-139">這明確表示可以在這裡使用擴充方法。</span><span class="sxs-lookup"><span data-stu-id="fd8a2-139">This explicitly means that extension methods can be used here.</span></span>

## <a name="considerations"></a><span data-ttu-id="fd8a2-140">考量</span><span class="sxs-lookup"><span data-stu-id="fd8a2-140">Considerations</span></span>

### <a name="case-labels-without-blocks"></a><span data-ttu-id="fd8a2-141">沒有區塊的案例標籤</span><span class="sxs-lookup"><span data-stu-id="fd8a2-141">case labels without blocks</span></span>

<span data-ttu-id="fd8a2-142">`using declaration` 在 `case` 標籤中直接不合法，因為其實際存留期的複雜性。</span><span class="sxs-lookup"><span data-stu-id="fd8a2-142">A `using declaration` is illegal directly inside a `case` label due to complications around its actual lifetime.</span></span> <span data-ttu-id="fd8a2-143">其中一個可能的解決方法，就是在相同的位置中，為它提供與 `out var` 相同的存留期。</span><span class="sxs-lookup"><span data-stu-id="fd8a2-143">One potential solution is to simply give it the same lifetime as an `out var` in the same location.</span></span> <span data-ttu-id="fd8a2-144">它已被視為功能執行的額外複雜性，而且輕鬆地解決此問題（只要將區塊新增至 `case` 標籤）就不會採用此路由。</span><span class="sxs-lookup"><span data-stu-id="fd8a2-144">It was deemed the extra complexity to the feature implementation and the ease of the work around (just add a block to the `case` label) didn't justify taking this route.</span></span>

## <a name="future-expansions"></a><span data-ttu-id="fd8a2-145">未來的擴充</span><span class="sxs-lookup"><span data-stu-id="fd8a2-145">Future Expansions</span></span>

### <a name="fixed-locals"></a><span data-ttu-id="fd8a2-146">已修正區域變數</span><span class="sxs-lookup"><span data-stu-id="fd8a2-146">fixed locals</span></span>

<span data-ttu-id="fd8a2-147">`fixed` 語句具有 `using` 語句的所有屬性，可讓您擁有 `using` 區域變數的能力。</span><span class="sxs-lookup"><span data-stu-id="fd8a2-147">A `fixed` statement has all of the properties of `using` statements that motivated the ability to have `using` locals.</span></span> <span data-ttu-id="fd8a2-148">請考慮將此功能延伸到 `fixed` 區域變數。</span><span class="sxs-lookup"><span data-stu-id="fd8a2-148">Consideration should be given to extending this feature to `fixed` locals as well.</span></span> <span data-ttu-id="fd8a2-149">存留期和順序規則應該同樣適用于 `using` 和 `fixed`。</span><span class="sxs-lookup"><span data-stu-id="fd8a2-149">The lifetime and ordering rules should apply equally well for `using` and `fixed` here.</span></span>
