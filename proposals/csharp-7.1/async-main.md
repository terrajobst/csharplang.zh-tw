---
ms.openlocfilehash: 405153448d0e3685d6f22725e00d75d9250b3e20
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484794"
---
# <a name="async-main"></a><span data-ttu-id="a6ced-101">非同步 Main</span><span class="sxs-lookup"><span data-stu-id="a6ced-101">Async Main</span></span>

* <span data-ttu-id="a6ced-102">[x] 提議</span><span class="sxs-lookup"><span data-stu-id="a6ced-102">[x] Proposed</span></span>
* <span data-ttu-id="a6ced-103">[] 原型</span><span class="sxs-lookup"><span data-stu-id="a6ced-103">[ ] Prototype</span></span>
* <span data-ttu-id="a6ced-104">[] 執行</span><span class="sxs-lookup"><span data-stu-id="a6ced-104">[ ] Implementation</span></span>
* <span data-ttu-id="a6ced-105">[] 規格</span><span class="sxs-lookup"><span data-stu-id="a6ced-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="a6ced-106">摘要</span><span class="sxs-lookup"><span data-stu-id="a6ced-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="a6ced-107">允許在應用程式的主要/entrypoint 方法中使用 `await`，方法是允許 entrypoint 傳回 `Task` / `Task<int>`，並將其標示為 `async`。</span><span class="sxs-lookup"><span data-stu-id="a6ced-107">Allow `await` to be used in an application's Main / entrypoint method by allowing the entrypoint to return `Task` / `Task<int>` and be marked `async`.</span></span>

## <a name="motivation"></a><span data-ttu-id="a6ced-108">動機</span><span class="sxs-lookup"><span data-stu-id="a6ced-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="a6ced-109">當學習C#、撰寫以主控台為基礎的公用程式時，以及撰寫小型測試應用程式時，想要從 Main 呼叫和 `await` `async` 方法時，這是很常見的情況。</span><span class="sxs-lookup"><span data-stu-id="a6ced-109">It is very common when learning C#, when writing console-based utilities, and when writing small test apps to want to call and `await` `async` methods from Main.</span></span>  <span data-ttu-id="a6ced-110">我們今天在這裡增加了複雜性層級，方法是強制在不同的非同步方法中執行這類 `await`，讓開發人員只需要撰寫如下所示的建議，就可以開始：</span><span class="sxs-lookup"><span data-stu-id="a6ced-110">Today we add a level of complexity here by forcing such `await`'ing to be done in a separate async method, which causes developers to need to write boilerplate like the following just to get started:</span></span>

```csharp
public static void Main()
{
    MainAsync().GetAwaiter().GetResult();
}

private static async Task MainAsync()
{
    ... // Main body here
}
```

<span data-ttu-id="a6ced-111">我們可以移除這項建議的需求，並讓主要本身得以 `async`，以便在其中使用 `await`。</span><span class="sxs-lookup"><span data-stu-id="a6ced-111">We can remove the need for this boilerplate and make it easier to get started simply by allowing Main itself to be `async` such that `await`s can be used in it.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="a6ced-112">詳細設計</span><span class="sxs-lookup"><span data-stu-id="a6ced-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="a6ced-113">下列簽章目前允許 e：</span><span class="sxs-lookup"><span data-stu-id="a6ced-113">The following signatures are currently allowed entrypoints:</span></span>

```csharp
static void Main()
static void Main(string[])
static int Main()
static int Main(string[])
```

<span data-ttu-id="a6ced-114">我們會擴充允許的 e 清單，使其包含：</span><span class="sxs-lookup"><span data-stu-id="a6ced-114">We extend the list of allowed entrypoints to include:</span></span>

```csharp
static Task Main()
static Task<int> Main()
static Task Main(string[])
static Task<int> Main(string[])
```

<span data-ttu-id="a6ced-115">為了避免相容性風險，如果沒有先前集合的多載存在，這些新的簽章只會被視為有效的 e。</span><span class="sxs-lookup"><span data-stu-id="a6ced-115">To avoid compatibility risks, these new signatures will only be considered as valid entrypoints if no overloads of the previous set are present.</span></span>
<span data-ttu-id="a6ced-116">語言/編譯器不會要求將進入點標記為 `async`，但我們預期大部分的使用都會標示為。</span><span class="sxs-lookup"><span data-stu-id="a6ced-116">The language / compiler will not require that the entrypoint be marked as `async`, though we expect the vast majority of uses will be marked as such.</span></span>

<span data-ttu-id="a6ced-117">當其中一個識別為進入點時，編譯器會合成實際的 entrypoint 方法，以呼叫下列其中一個程式碼方法：</span><span class="sxs-lookup"><span data-stu-id="a6ced-117">When one of these is identified as the entrypoint, the compiler will synthesize an actual entrypoint method that calls one of these coded methods:</span></span>
- <span data-ttu-id="a6ced-118">```static Task Main()``` 會導致編譯器發出對等的 ```private static void $GeneratedMain() => Main().GetAwaiter().GetResult();```</span><span class="sxs-lookup"><span data-stu-id="a6ced-118">```static Task Main()``` will result in the compiler emitting the equivalent of ```private static void $GeneratedMain() => Main().GetAwaiter().GetResult();```</span></span>
- <span data-ttu-id="a6ced-119">```static Task Main(string[])``` 會導致編譯器發出對等的 ```private static void $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```</span><span class="sxs-lookup"><span data-stu-id="a6ced-119">```static Task Main(string[])``` will result in the compiler emitting the equivalent of ```private static void $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```</span></span>
- <span data-ttu-id="a6ced-120">```static Task<int> Main()``` 會導致編譯器發出對等的 ```private static int $GeneratedMain() => Main().GetAwaiter().GetResult();```</span><span class="sxs-lookup"><span data-stu-id="a6ced-120">```static Task<int> Main()``` will result in the compiler emitting the equivalent of ```private static int $GeneratedMain() => Main().GetAwaiter().GetResult();```</span></span>
- <span data-ttu-id="a6ced-121">```static Task<int> Main(string[])``` 會導致編譯器發出對等的 ```private static int $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```</span><span class="sxs-lookup"><span data-stu-id="a6ced-121">```static Task<int> Main(string[])``` will result in the compiler emitting the equivalent of ```private static int $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```</span></span>

<span data-ttu-id="a6ced-122">使用方式範例：</span><span class="sxs-lookup"><span data-stu-id="a6ced-122">Example usage:</span></span>

```csharp
using System;
using System.Net.Http;

class Test
{
    static async Task Main(string[] args) =>
        Console.WriteLine(await new HttpClient().GetStringAsync(args[0]));
}
```

## <a name="drawbacks"></a><span data-ttu-id="a6ced-123">缺點</span><span class="sxs-lookup"><span data-stu-id="a6ced-123">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="a6ced-124">主要缺點只是支援額外的進入點簽章的額外複雜性。</span><span class="sxs-lookup"><span data-stu-id="a6ced-124">The main drawback is simply the additional complexity of supporting additional entrypoint signatures.</span></span>

## <a name="alternatives"></a><span data-ttu-id="a6ced-125">替代方案</span><span class="sxs-lookup"><span data-stu-id="a6ced-125">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="a6ced-126">其他考慮的變數：</span><span class="sxs-lookup"><span data-stu-id="a6ced-126">Other variants considered:</span></span>

<span data-ttu-id="a6ced-127">允許 `async void`。</span><span class="sxs-lookup"><span data-stu-id="a6ced-127">Allowing `async void`.</span></span>  <span data-ttu-id="a6ced-128">我們需要針對直接呼叫它的程式碼保持相同的語法，這樣會使產生的進入點難以呼叫它（不會傳回工作）。</span><span class="sxs-lookup"><span data-stu-id="a6ced-128">We need to keep the semantics the same for code calling it directly, which would then make it difficult for a generated entrypoint to call it (no Task returned).</span></span>  <span data-ttu-id="a6ced-129">我們可以藉由產生兩個其他方法來解決此問題，例如</span><span class="sxs-lookup"><span data-stu-id="a6ced-129">We could solve this by generating two other methods, e.g.</span></span>

```csharp
public static async void Main()
{
   ... // await code
}
```

<span data-ttu-id="a6ced-130">變成</span><span class="sxs-lookup"><span data-stu-id="a6ced-130">becomes</span></span>

```csharp
public static async void Main() => await $MainTask();

private static void $EntrypointMain() => Main().GetAwaiter().GetResult();

private static async Task $MainTask()
{
    ... // await code
}
```

<span data-ttu-id="a6ced-131">同時也有關于鼓勵使用 `async void`的顧慮。</span><span class="sxs-lookup"><span data-stu-id="a6ced-131">There are also concerns around encouraging usage of `async void`.</span></span>

<span data-ttu-id="a6ced-132">使用 "MainAsync" 而非 "Main" 作為名稱。</span><span class="sxs-lookup"><span data-stu-id="a6ced-132">Using "MainAsync" instead of "Main" as the name.</span></span>  <span data-ttu-id="a6ced-133">雖然建議使用非同步尾碼來進行工作傳回的方法，但這主要是關於程式庫功能（主要不是），而且支援超出 "Main" 的其他 entrypoint 名稱，並不值得這麼做。</span><span class="sxs-lookup"><span data-stu-id="a6ced-133">While the async suffix is recommended for Task-returning methods, that's primarily about library functionality, which Main is not, and supporting additional entrypoint names beyond "Main" is not worth it.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="a6ced-134">未解決的問題</span><span class="sxs-lookup"><span data-stu-id="a6ced-134">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="a6ced-135">n/a</span><span class="sxs-lookup"><span data-stu-id="a6ced-135">n/a</span></span>

## <a name="design-meetings"></a><span data-ttu-id="a6ced-136">設計會議</span><span class="sxs-lookup"><span data-stu-id="a6ced-136">Design meetings</span></span>

<span data-ttu-id="a6ced-137">n/a</span><span class="sxs-lookup"><span data-stu-id="a6ced-137">n/a</span></span>
