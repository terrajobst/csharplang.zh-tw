---
ms.openlocfilehash: 54ae4ffabde6dca49b7e6bfb626d65837eabc8f5
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281940"
---
# <a name="simple-programs"></a><span data-ttu-id="d7a0e-101">簡單的程式</span><span class="sxs-lookup"><span data-stu-id="d7a0e-101">Simple programs</span></span>

* <span data-ttu-id="d7a0e-102">[x] 提議</span><span class="sxs-lookup"><span data-stu-id="d7a0e-102">[x] Proposed</span></span>
* <span data-ttu-id="d7a0e-103">[x] 原型：已啟動</span><span class="sxs-lookup"><span data-stu-id="d7a0e-103">[x] Prototype: Started</span></span>
* <span data-ttu-id="d7a0e-104">[] 執行：未啟動</span><span class="sxs-lookup"><span data-stu-id="d7a0e-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="d7a0e-105">[] 規格：未啟動</span><span class="sxs-lookup"><span data-stu-id="d7a0e-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="d7a0e-106">摘要</span><span class="sxs-lookup"><span data-stu-id="d7a0e-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="d7a0e-107">允許在*compilation_unit* （也就是來源檔案）的*namespace_member_declaration*s 之前，執行一系列的*語句*。</span><span class="sxs-lookup"><span data-stu-id="d7a0e-107">Allow a sequence of *statements* to occur right before the *namespace_member_declaration*s of a *compilation_unit* (i.e. source file).</span></span>

<span data-ttu-id="d7a0e-108">其語義是，如果有這類的*語句*，就會發出下列類型宣告，並將實際的類型名稱和方法名稱模數。</span><span class="sxs-lookup"><span data-stu-id="d7a0e-108">The semantics are that if such a sequence of *statements* is present, the following type declaration, modulo the actual type name and the method name, would be emitted:</span></span>

``` c#
static class Program
{
    static async Task Main()
    {
        // statements
    }
}
```

<span data-ttu-id="d7a0e-109">另請參閱 https://github.com/dotnet/csharplang/issues/3117。</span><span class="sxs-lookup"><span data-stu-id="d7a0e-109">See also https://github.com/dotnet/csharplang/issues/3117.</span></span>

## <a name="motivation"></a><span data-ttu-id="d7a0e-110">動機</span><span class="sxs-lookup"><span data-stu-id="d7a0e-110">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="d7a0e-111">即使是最簡單的程式，也有一定數量的重複使用方式，因為需要明確的 `Main` 方法。</span><span class="sxs-lookup"><span data-stu-id="d7a0e-111">There's a certain amount of boilerplate surrounding even the simplest of programs, because of the need for an explicit `Main` method.</span></span> <span data-ttu-id="d7a0e-112">這似乎可以讓語言學習和程式清楚明瞭。</span><span class="sxs-lookup"><span data-stu-id="d7a0e-112">This seems to get in the way of language learning and program clarity.</span></span> <span data-ttu-id="d7a0e-113">因此，此功能的主要目標是要讓C#程式不需要不必要的反復專案，以方便學習和撰寫程式碼。</span><span class="sxs-lookup"><span data-stu-id="d7a0e-113">The primary goal of the feature therefore is to allow C# programs without unnecessary boilerplate around them, for the sake of learners and the clarity of code.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="d7a0e-114">詳細設計</span><span class="sxs-lookup"><span data-stu-id="d7a0e-114">Detailed design</span></span>
[design]: #detailed-design

### <a name="syntax"></a><span data-ttu-id="d7a0e-115">語法</span><span class="sxs-lookup"><span data-stu-id="d7a0e-115">Syntax</span></span>

<span data-ttu-id="d7a0e-116">唯一的另一個語法是在編譯單位中允許一連串的*語句*，就在*namespace_member_declaration*s 之前：</span><span class="sxs-lookup"><span data-stu-id="d7a0e-116">The only additional syntax is allowing a sequence of *statement*s in a compilation unit, just before the *namespace_member_declaration*s:</span></span>

``` antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? statement* namespace_member_declaration*
    ;
```

<span data-ttu-id="d7a0e-117">只允許一個*compilation_unit*有*語句*s。</span><span class="sxs-lookup"><span data-stu-id="d7a0e-117">Only one *compilation_unit* is allowed to have *statement*s.</span></span> 

<span data-ttu-id="d7a0e-118">範例：</span><span class="sxs-lookup"><span data-stu-id="d7a0e-118">Example:</span></span>

``` c#
if (System.Environment.CommandLine.Length == 0
    || !int.TryParse(System.Environment.CommandLine, out int n)
    || n < 0) return;
Console.WriteLine(Fib(n).curr);

(int curr, int prev) Fib(int i)
{
    if (i == 0) return (1, 0);
    var (curr, prev) = Fib(i - 1);
    return (curr + prev, curr);
}
```

### <a name="semantics"></a><span data-ttu-id="d7a0e-119">語意</span><span class="sxs-lookup"><span data-stu-id="d7a0e-119">Semantics</span></span>

<span data-ttu-id="d7a0e-120">如果程式的任何編譯單位中有任何最上層語句，其意義就如同它們已結合在全域命名空間中 `Program` 類別的 `Main` 方法的區塊主體中，如下所示：</span><span class="sxs-lookup"><span data-stu-id="d7a0e-120">If any top-level statements are present in any compilation unit of the program, the meaning is as if they were combined in the block body of a `Main` method of a `Program` class in the global namespace, as follows:</span></span>

``` c#
static class Program
{
    static async Task Main()
    {
        // statements
    }
}
```

<span data-ttu-id="d7a0e-121">請注意，名稱「程式」和「主要」僅供說明之用，編譯器所使用的實際名稱是相依的，而且型別和方法都不能由原始程式碼參考。</span><span class="sxs-lookup"><span data-stu-id="d7a0e-121">Note that the names "Program" and "Main" are used only for illustrations purposes, actual names used by compiler are implementation dependent and neither the type, nor the method can be referenced by name from source code.</span></span>

<span data-ttu-id="d7a0e-122">方法會指定為程式的進入點。</span><span class="sxs-lookup"><span data-stu-id="d7a0e-122">The method is designated as the entry point of the program.</span></span> <span data-ttu-id="d7a0e-123">根據慣例，明確宣告的方法可視為會忽略進入點候選項目。</span><span class="sxs-lookup"><span data-stu-id="d7a0e-123">Explicitly declared methods that by convention could be considered as an entry point candidates are ignored.</span></span> <span data-ttu-id="d7a0e-124">當發生這種情況時，就會回報警告。</span><span class="sxs-lookup"><span data-stu-id="d7a0e-124">A warning is reported when that happens.</span></span> <span data-ttu-id="d7a0e-125">當有最上層的語句時，指定 `-main:<type>` 編譯器參數是錯誤的。</span><span class="sxs-lookup"><span data-stu-id="d7a0e-125">It is an error to specify `-main:<type>` compiler switch when there are top-level statements.</span></span>

<span data-ttu-id="d7a0e-126">在一般非同步進入點方法內的語句中，最上層語句允許非同步作業。</span><span class="sxs-lookup"><span data-stu-id="d7a0e-126">Async operations are allowed in top-level statements to the degree they are allowed in statements within a regular async entry point method.</span></span> <span data-ttu-id="d7a0e-127">不過，它們不是必要的，如果省略 `await` 運算式和其他非同步作業，就不會產生任何警告。</span><span class="sxs-lookup"><span data-stu-id="d7a0e-127">However, they are not required, if `await` expressions and other async operations are omitted, no warning is produced.</span></span> <span data-ttu-id="d7a0e-128">相反地，產生的進入點方法的簽章相當於</span><span class="sxs-lookup"><span data-stu-id="d7a0e-128">Instead the signature of the generated entry point method is equivalent to</span></span> 
``` c#
    static void Main()
```

<span data-ttu-id="d7a0e-129">上述範例會產生下列 `$Main` 方法宣告：</span><span class="sxs-lookup"><span data-stu-id="d7a0e-129">The example above would yield the following `$Main` method declaration:</span></span>

``` c#
static class $Program
{
    static void $Main()
    {
        if (System.Environment.CommandLine.Length == 0
            || !int.TryParse(System.Environment.CommandLine, out int n)
            || n < 0) return;
        Console.WriteLine(Fib(n).curr);
        
        (int curr, int prev) Fib(int i)
        {
            if (i == 0) return (1, 0);
            var (curr, prev) = Fib(i - 1);
            return (curr + prev, curr);
        }
    }
}
```

<span data-ttu-id="d7a0e-130">在此同時，範例如下所示：</span><span class="sxs-lookup"><span data-stu-id="d7a0e-130">At the same time an example like this:</span></span>
``` c#
await System.Threading.Tasks.Task.Delay(1000);
System.Console.WriteLine("Hi!");
```

<span data-ttu-id="d7a0e-131">會產生：</span><span class="sxs-lookup"><span data-stu-id="d7a0e-131">would  yield:</span></span>
``` c#
static class $Program
{
    static async Task $Main()
    {
        await System.Threading.Tasks.Task.Delay(1000);
        System.Console.WriteLine("Hi!");
    }
}
```

### <a name="scope-of-top-level-local-variables-and-local-functions"></a><span data-ttu-id="d7a0e-132">最上層區域變數和區域函式的範圍</span><span class="sxs-lookup"><span data-stu-id="d7a0e-132">Scope of top-level local variables and local functions</span></span>

<span data-ttu-id="d7a0e-133">即使最上層區域變數和函式已「包裝」到產生的進入點方法中，在每個編譯單位中，這些變數還是應該在整個程式的範圍內。</span><span class="sxs-lookup"><span data-stu-id="d7a0e-133">Even though top-level local variables and functions are "wrapped" into the generated entry point method, they should still be in scope throughout the program in every compilation unit.</span></span>
<span data-ttu-id="d7a0e-134">基於簡單名稱評估的目的，一旦達到全域命名空間：</span><span class="sxs-lookup"><span data-stu-id="d7a0e-134">For the purpose of simple-name evaluation, once the global namespace is reached:</span></span>
- <span data-ttu-id="d7a0e-135">首先，嘗試評估產生的進入點方法中的名稱，而且只有在此嘗試失敗時才會進行</span><span class="sxs-lookup"><span data-stu-id="d7a0e-135">First, an attempt is made to evaluate the name within the generated entry point method and only if this attempt fails</span></span> 
- <span data-ttu-id="d7a0e-136">全域命名空間宣告內的 "regular" 評估會執行。</span><span class="sxs-lookup"><span data-stu-id="d7a0e-136">The "regular" evaluation within the global namespace declaration is performed.</span></span> 

<span data-ttu-id="d7a0e-137">這可能會導致命名空間的名稱和在全域命名空間內宣告的類型，以及匯入的名稱的遮蔽。</span><span class="sxs-lookup"><span data-stu-id="d7a0e-137">This could lead to name shadowing of namespaces and types declared within the global namespace as well as to shadowing of imported names.</span></span>

<span data-ttu-id="d7a0e-138">如果簡單名稱評估是在最上層語句之外進行，而評估產生最上層的區域變數或函式，則會導致錯誤。</span><span class="sxs-lookup"><span data-stu-id="d7a0e-138">If the simple name evaluation occurs outside of the top-level statements and the evaluation yields a top-level local variable or function, that should lead to an error.</span></span>

<span data-ttu-id="d7a0e-139">如此一來，我們就可以保護未來更能解決「最上層功能」（ https://github.com/dotnet/csharplang/issues/3117)案例2的能力，而且能夠為不小心認為受到支援的使用者提供有用的診斷。</span><span class="sxs-lookup"><span data-stu-id="d7a0e-139">In this way we protect our future ability to better address "Top-level functions" (scenario 2 in https://github.com/dotnet/csharplang/issues/3117), and are able to give useful diagnostics to users who mistakenly believe them to be supported.</span></span>

