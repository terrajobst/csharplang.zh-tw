---
ms.openlocfilehash: ac2b233eb703b5eea3bd2dfdbeeadd7494b0c695
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484668"
---
# <a name="non-trailing-named-arguments"></a><span data-ttu-id="004ca-101">非後置具名引數</span><span class="sxs-lookup"><span data-stu-id="004ca-101">Non-trailing named arguments</span></span>

## <a name="summary"></a><span data-ttu-id="004ca-102">摘要</span><span class="sxs-lookup"><span data-stu-id="004ca-102">Summary</span></span>
[summary]: #summary
<span data-ttu-id="004ca-103">允許在非尾端位置使用具名引數，只要它們是用於正確的位置即可。</span><span class="sxs-lookup"><span data-stu-id="004ca-103">Allow named arguments to be used in non-trailing position, as long as they are used in their correct position.</span></span> <span data-ttu-id="004ca-104">例如： `DoSomething(isEmployed:true, name, age);` 。</span><span class="sxs-lookup"><span data-stu-id="004ca-104">For example: `DoSomething(isEmployed:true, name, age);`.</span></span>

## <a name="motivation"></a><span data-ttu-id="004ca-105">動機</span><span class="sxs-lookup"><span data-stu-id="004ca-105">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="004ca-106">主要動機是避免輸入多餘的資訊。</span><span class="sxs-lookup"><span data-stu-id="004ca-106">The main motivation is to avoid typing redundant information.</span></span> <span data-ttu-id="004ca-107">通常會將作為常值的引數命名（例如 `null`，`true`），以便用來闡明程式碼，而不是以不按照順序的方式傳遞引數。</span><span class="sxs-lookup"><span data-stu-id="004ca-107">It is common to name an argument that is a literal (such as `null`, `true`) for the purpose of clarifying the code, rather than of passing arguments out-of-order.</span></span>
<span data-ttu-id="004ca-108">目前不允許（`CS1738`），除非下列所有引數也都命名為。</span><span class="sxs-lookup"><span data-stu-id="004ca-108">That is currently disallowed (`CS1738`) unless all the following arguments are also named.</span></span>

```csharp
DoSomething(isEmployed:true, name, age); // currently disallowed, even though all arguments are in position
// CS1738 "Named argument specifications must appear after all fixed arguments have been specified"
```

<span data-ttu-id="004ca-109">一些其他範例：</span><span class="sxs-lookup"><span data-stu-id="004ca-109">Some additional examples:</span></span>
```csharp
public void DoSomething(bool isEmployed, string personName, int personAge) { ... }

DoSomething(isEmployed:true, name, age); // currently CS1738, but would become legal
DoSomething(true, personName:name, age); // currently CS1738, but would become legal
DoSomething(name, isEmployed:true, age); // remains illegal
DoSomething(name, age, isEmployed:true); // remains illegal
DoSomething(true, personAge:age, personName:name); // already legal
```

<span data-ttu-id="004ca-110">這也可以搭配 params 使用：</span><span class="sxs-lookup"><span data-stu-id="004ca-110">This would also work with params:</span></span>
```csharp
public class Task
{
    public static Task When(TaskStatus all, TaskStatus any, params Task[] tasks);
}
Task.When(all: TaskStatus.RanToCompletion, any: TaskStatus.Faulted, task1, task2)
```

## <a name="detailed-design"></a><span data-ttu-id="004ca-111">詳細設計</span><span class="sxs-lookup"><span data-stu-id="004ca-111">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="004ca-112">在§7.5.1 版（引數清單）中，規格目前顯示：</span><span class="sxs-lookup"><span data-stu-id="004ca-112">In §7.5.1 (Argument lists), the spec currently says:</span></span>
> <span data-ttu-id="004ca-113">具有*引數名稱*的*引數*稱為__具名引數__，而沒有*引數名稱*的*引數*則是__位置引數__。</span><span class="sxs-lookup"><span data-stu-id="004ca-113">An *argument* with an *argument-name* is referred to as a __named argument__, whereas an *argument* without an *argument-name* is a __positional argument__.</span></span> <span data-ttu-id="004ca-114">在*引數清單*中的具名引數後面出現位置引數時，會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="004ca-114">It is an error for a positional argument to appear after a named argument in an *argument-list*.</span></span>

<span data-ttu-id="004ca-115">建議您移除此錯誤，並更新規則以尋找引數的對應參數（§7.5.1.1）：</span><span class="sxs-lookup"><span data-stu-id="004ca-115">The proposal is to remove this error and update the rules for finding the corresponding parameter for an argument (§7.5.1.1):</span></span>

<span data-ttu-id="004ca-116">引數中的引數-實例的函式、方法、索引子和委派的清單：</span><span class="sxs-lookup"><span data-stu-id="004ca-116">Arguments in the argument-list of instance constructors, methods, indexers and delegates:</span></span>
- <span data-ttu-id="004ca-117">[現有規則]</span><span class="sxs-lookup"><span data-stu-id="004ca-117">[existing rules]</span></span>
- <span data-ttu-id="004ca-118">未命名的引數會在超出位置的具名引數或具名引數引數之後，對應到沒有參數。</span><span class="sxs-lookup"><span data-stu-id="004ca-118">An unnamed argument corresponds to no parameter when it is after an out-of-position named argument or a named params argument.</span></span>

<span data-ttu-id="004ca-119">特別是，這會防止使用 `M(c: false, valueB);`叫用 `void M(bool a = true, bool b = true, bool c = true, );`。</span><span class="sxs-lookup"><span data-stu-id="004ca-119">In particular, this prevents invoking `void M(bool a = true, bool b = true, bool c = true, );` with `M(c: false, valueB);`.</span></span> <span data-ttu-id="004ca-120">第一個引數會使用不在位置（引數會在第一個位置使用，但名為 "c" 的參數會在第三個位置），因此下列引數應該命名為。</span><span class="sxs-lookup"><span data-stu-id="004ca-120">The first argument is used out-of-position (the argument is used in first position, but the parameter named "c" is in third position), so the following arguments should be named.</span></span>

<span data-ttu-id="004ca-121">換句話說，只有在名稱和位置導致尋找相同的對應參數時，才允許非尾端的具名引數。</span><span class="sxs-lookup"><span data-stu-id="004ca-121">In other words, non-trailing named arguments are only allowed when the name and the position result in finding the same corresponding parameter.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="004ca-122">缺點</span><span class="sxs-lookup"><span data-stu-id="004ca-122">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="004ca-123">此提議會 exacerbates 具有多載解析中具名引數的現有微妙差異。</span><span class="sxs-lookup"><span data-stu-id="004ca-123">This proposal exacerbates existing subtleties with named arguments in overload resolution.</span></span> <span data-ttu-id="004ca-124">例如：</span><span class="sxs-lookup"><span data-stu-id="004ca-124">For instance:</span></span>

```csharp
void M(int x, int y) { }
void M<T>(T y, int x) { }

void M2()
{
    M(3, 4);
    M(y: 3, x: 4); // Invokes M(int, int)
    M(y: 3, 4); // Invokes M<T>(T, int)
}
```

<span data-ttu-id="004ca-125">您現在可以藉由交換參數來取得這種情況：</span><span class="sxs-lookup"><span data-stu-id="004ca-125">You could get this situation today by swapping the parameters:</span></span>

```csharp
void M(int y, int x) { }
void M<T>(int x, T y) { }

void M2()
{
    M(3, 4);
    M(x: 3, y: 4); // Invokes M(int, int)
    M(3, y: 4); // Invokes M<T>(int, T)
}
```

<span data-ttu-id="004ca-126">同樣地，如果您有兩個方法 `void M(int a, int b)` 和 `void M(int x, string y)`，則不被叫用的調用 `M(x: 1, 2)` 將會根據第二個多載產生診斷（「無法從 ' int ' 轉換為 ' string '」）。</span><span class="sxs-lookup"><span data-stu-id="004ca-126">Similarly, if you have two methods `void M(int a, int b)` and `void M(int x, string y)`, the mistaken invocation `M(x: 1, 2)` will produce a diagnostic based on the second overload ("cannot convert from 'int' to 'string'").</span></span> <span data-ttu-id="004ca-127">當具名引數用於尾端位置時，這個問題已經存在。</span><span class="sxs-lookup"><span data-stu-id="004ca-127">This problem already exists when the named argument is used in a trailing position.</span></span>

## <a name="alternatives"></a><span data-ttu-id="004ca-128">替代方案</span><span class="sxs-lookup"><span data-stu-id="004ca-128">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="004ca-129">有幾個需要考慮的替代方案：</span><span class="sxs-lookup"><span data-stu-id="004ca-129">There are a couple of alternatives to consider:</span></span>

- <span data-ttu-id="004ca-130">現狀</span><span class="sxs-lookup"><span data-stu-id="004ca-130">The status quo</span></span>
- <span data-ttu-id="004ca-131">提供 IDE 協助，讓您在中間輸入特定名稱時，填滿尾端引數的所有名稱。</span><span class="sxs-lookup"><span data-stu-id="004ca-131">Providing IDE assistance to fill-in all the names of trailing arguments when you type specific a name in the middle.</span></span>

<span data-ttu-id="004ca-132">這兩種情況都能從更詳細的資訊，因為它們會引進多個具名引數，即使在引數清單的開頭只需要一個常值的名稱。</span><span class="sxs-lookup"><span data-stu-id="004ca-132">Both of those suffer from more verbosity, as they introduce multiple named arguments even if you just need one name of a literal at the beginning of the argument list.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="004ca-133">未解決的問題</span><span class="sxs-lookup"><span data-stu-id="004ca-133">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a><span data-ttu-id="004ca-134">設計會議</span><span class="sxs-lookup"><span data-stu-id="004ca-134">Design meetings</span></span>
[ldm]: #ldm
<span data-ttu-id="004ca-135">此功能已在5月 2017 16 日的 LDM 中簡短討論，並以原則進行核准（「確定」移至「提案」/「原型」）。</span><span class="sxs-lookup"><span data-stu-id="004ca-135">The feature was briefly discussed in LDM on May 16th 2017, with approval in principle (ok to move to proposal/prototype).</span></span> <span data-ttu-id="004ca-136">它也會在2017年6月28日簡短討論。</span><span class="sxs-lookup"><span data-stu-id="004ca-136">It was also briefly discussed on June 28th 2017.</span></span>

<span data-ttu-id="004ca-137">與探討問題相關的初始討論 https://github.com/dotnet/csharplang/issues/518 https://github.com/dotnet/csharplang/issues/570</span><span class="sxs-lookup"><span data-stu-id="004ca-137">Relates to initial discussion https://github.com/dotnet/csharplang/issues/518 Relates to championed issue https://github.com/dotnet/csharplang/issues/570</span></span>
