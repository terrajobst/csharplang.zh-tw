---
ms.openlocfilehash: 9ed1aa75d581cecbf754a84d1f523c6334b8c0ac
ms.sourcegitcommit: 5278336b61519956240a6f7d83bcb4322019e032
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/10/2020
ms.locfileid: "79485256"
---
# <a name="async-streams"></a><span data-ttu-id="ccc6e-101">非同步資料流程</span><span class="sxs-lookup"><span data-stu-id="ccc6e-101">Async Streams</span></span>

* <span data-ttu-id="ccc6e-102">[x] 提議</span><span class="sxs-lookup"><span data-stu-id="ccc6e-102">[x] Proposed</span></span>
* <span data-ttu-id="ccc6e-103">[x] 原型</span><span class="sxs-lookup"><span data-stu-id="ccc6e-103">[x] Prototype</span></span>
* <span data-ttu-id="ccc6e-104">[] 執行</span><span class="sxs-lookup"><span data-stu-id="ccc6e-104">[ ] Implementation</span></span>
* <span data-ttu-id="ccc6e-105">[] 規格</span><span class="sxs-lookup"><span data-stu-id="ccc6e-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="ccc6e-106">摘要</span><span class="sxs-lookup"><span data-stu-id="ccc6e-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="ccc6e-107">C#具有 iterator 方法和非同步方法的支援，但不支援同時為 iterator 和 async 方法的方法。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-107">C# has support for iterator methods and async methods, but no support for a method that is both an iterator and an async method.</span></span>  <span data-ttu-id="ccc6e-108">我們應該藉由允許將 `await` 用於新的 `async` 反覆運算器，其中一個會傳回 `IAsyncEnumerable<T>` 或 `IAsyncEnumerator<T>` 而不是 `IEnumerable<T>` 或 `IEnumerator<T>`，`IAsyncEnumerable<T>` 可在新的 `await foreach`中使用。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-108">We should rectify this by allowing for `await` to be used in a new form of `async` iterator, one that returns an `IAsyncEnumerable<T>` or `IAsyncEnumerator<T>` rather than an `IEnumerable<T>` or `IEnumerator<T>`, with `IAsyncEnumerable<T>` consumable in a new `await foreach`.</span></span>  <span data-ttu-id="ccc6e-109">`IAsyncDisposable` 介面也用來啟用非同步清除。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-109">An `IAsyncDisposable` interface is also used to enable asynchronous cleanup.</span></span>

## <a name="related-discussion"></a><span data-ttu-id="ccc6e-110">相關討論</span><span class="sxs-lookup"><span data-stu-id="ccc6e-110">Related discussion</span></span>
- https://github.com/dotnet/roslyn/issues/261
- https://github.com/dotnet/roslyn/issues/114

## <a name="detailed-design"></a><span data-ttu-id="ccc6e-111">詳細設計</span><span class="sxs-lookup"><span data-stu-id="ccc6e-111">Detailed design</span></span>
[design]: #detailed-design

## <a name="interfaces"></a><span data-ttu-id="ccc6e-112">介面</span><span class="sxs-lookup"><span data-stu-id="ccc6e-112">Interfaces</span></span>

### <a name="iasyncdisposable"></a><span data-ttu-id="ccc6e-113">IAsyncDisposable</span><span class="sxs-lookup"><span data-stu-id="ccc6e-113">IAsyncDisposable</span></span>

<span data-ttu-id="ccc6e-114">`IAsyncDisposable` 的討論（例如 https://github.com/dotnet/roslyn/issues/114)，以及是否是不錯的想法。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-114">There has been much discussion of `IAsyncDisposable` (e.g. https://github.com/dotnet/roslyn/issues/114) and whether it's a good idea.</span></span>  <span data-ttu-id="ccc6e-115">不過，這是加入非同步反覆運算器支援的必要概念。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-115">However, it's a required concept to add in support of async iterators.</span></span>  <span data-ttu-id="ccc6e-116">由於 `finally` 區塊可能包含 `await`s，而且因為必須在處置反覆運算器的過程中執行 `finally` 區塊，所以我們需要非同步處置。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-116">Since `finally` blocks may contain `await`s, and since `finally` blocks need to be run as part of disposing of iterators, we need async disposal.</span></span>  <span data-ttu-id="ccc6e-117">它也只會在清除資源的任何時間都很有用，例如關閉檔案（需要排清）、取消註冊回呼，並提供一種方法來得知何時完成解除註冊等等。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-117">It's also just generally useful any time cleaning up of resources might take any period of time, e.g. closing files (requiring flushes), deregistering callbacks and providing a way to know when deregistration has completed, etc.</span></span>

<span data-ttu-id="ccc6e-118">下列介面會新增至核心 .NET 程式庫（例如 CoreLib/System.webserver）：</span><span class="sxs-lookup"><span data-stu-id="ccc6e-118">The following interface is added to the core .NET libraries (e.g. System.Private.CoreLib / System.Runtime):</span></span>
```csharp
namespace System
{
    public interface IAsyncDisposable
    {
        ValueTask DisposeAsync();
    }
}
```
<span data-ttu-id="ccc6e-119">如同 `Dispose`，可以接受多次叫用 `DisposeAsync`，而第一個呼叫之後的後續調用則應該視為 nops，並傳回同步完成的成功工作（但`DisposeAsync` 不需要具備執行緒安全，但不需要支援並行調用）。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-119">As with `Dispose`, invoking `DisposeAsync` multiple times is acceptable, and subsequent invocations after the first should be treated as nops, returning a synchronously completed successful task (`DisposeAsync` need not be thread-safe, though, and need not support concurrent invocation).</span></span>  <span data-ttu-id="ccc6e-120">此外，型別可能會同時執行 `IDisposable` 和 `IAsyncDisposable`，如果有的話，就可以叫用 `Dispose` 然後 `DisposeAsync` 或相反的方式，但只有第一個是有意義的，而且後續的叫用都應該是 nop。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-120">Further, types may implement both `IDisposable` and `IAsyncDisposable`, and if they do, it's similarly acceptable to invoke `Dispose` and then `DisposeAsync` or vice versa, but only the first should be meaningful and subsequent invocations of either should be a nop.</span></span>  <span data-ttu-id="ccc6e-121">因此，如果型別確實執行這兩個，則取用者只會呼叫一次，而只有一次以內容為基礎的相關方法，`Dispose` 在同步內容中，並在非同步環境中 `DisposeAsync`。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-121">As such, if a type does implement both, consumers are encouraged to call once and only once the more relevant method based on the context, `Dispose` in synchronous contexts and `DisposeAsync` in asynchronous ones.</span></span>

<span data-ttu-id="ccc6e-122">（我會留下討論 `IAsyncDisposable` 如何與 `using` 互動，以進行個別討論。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-122">(I'm leaving discussion of how `IAsyncDisposable` interacts with `using` to a separate discussion.</span></span>  <span data-ttu-id="ccc6e-123">以及如何與 `foreach` 互動的涵蓋範圍，會在本提案稍後進行處理）。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-123">And coverage of how it interacts with `foreach` is handled later in this proposal.)</span></span>

<span data-ttu-id="ccc6e-124">考慮的替代方案：</span><span class="sxs-lookup"><span data-stu-id="ccc6e-124">Alternatives considered:</span></span>
- <span data-ttu-id="ccc6e-125">_`DisposeAsync` 接受 `CancellationToken`_ ：在理論上，可以取消任何非同步作業，處置是關於清除、關閉 free'ing 資源等，這通常不是應該取消的專案;針對已取消的工作，清除仍然很重要。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-125">_`DisposeAsync` accepting a `CancellationToken`_: while in theory it makes sense that anything async can be canceled, disposal is about cleanup, closing things out, free'ing resources, etc., which is generally not something that should be canceled; cleanup is still important for work that's canceled.</span></span>  <span data-ttu-id="ccc6e-126">導致實際取消工作的相同 `CancellationToken` 通常是傳遞給 `DisposeAsync`的相同權杖，因此 `DisposeAsync` 不萬能，因為取消工作會導致 `DisposeAsync` 成為 nop。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-126">The same `CancellationToken` that caused the actual work to be canceled would typically be the same token passed to `DisposeAsync`, making `DisposeAsync` worthless because cancellation of the work would cause `DisposeAsync` to be a nop.</span></span>  <span data-ttu-id="ccc6e-127">如果有人想要避免封鎖等候處置，他們可以避免等待產生的 `ValueTask`，或只等待一段時間。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-127">If someone wants to avoid being blocked waiting for disposal, they can avoid waiting on the resulting `ValueTask`, or wait on it only for some period of time.</span></span>
- <span data-ttu-id="ccc6e-128">_`DisposeAsync` 傳回 `Task`_ ：現在，非泛型 `ValueTask` 存在，而且可以從 `IValueTaskSource`加以建立，從 `ValueTask` 傳回 `DisposeAsync` 可讓現有的物件重複使用，以表示 `DisposeAsync`的最終非同步完成，在 `Task` 非同步完成的情況下，儲存 `DisposeAsync` 配置。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-128">_`DisposeAsync` returning a `Task`_: Now that a non-generic `ValueTask` exists and can be constructed from an `IValueTaskSource`, returning `ValueTask` from `DisposeAsync` allows an existing object to be reused as the promise representing the eventual async completion of `DisposeAsync`, saving a `Task` allocation in the case where `DisposeAsync` completes asynchronously.</span></span>
- <span data-ttu-id="ccc6e-129">_使用 `bool continueOnCapturedContext` （`ConfigureAwait`）設定 `DisposeAsync`_ ：雖然可能會有一些問題與使用此概念的 `using`、`foreach`和其他語言結構公開，但從介面的觀點來看，它實際上不會執行任何 `await`的作業，而且沒有任何可設定的東西 .。。不過，`ValueTask` 的取用者可以視需要取用它。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-129">_Configuring `DisposeAsync` with a `bool continueOnCapturedContext` (`ConfigureAwait`)_: While there may be issues related to how such a concept is exposed to `using`, `foreach`, and other language constructs that consume this, from an interface perspective it's not actually doing any `await`'ing and there's nothing to configure... consumers of the `ValueTask` can consume it however they wish.</span></span>
- <span data-ttu-id="ccc6e-130">_`IAsyncDisposable` 繼承 `IDisposable`_ ：因為只應使用其中一個或另一個，所以強制型別同時執行這兩種方式並不合理。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-130">_`IAsyncDisposable` inheriting `IDisposable`_:  Since only one or the other should be used, it doesn't make sense to force types to implement both.</span></span>
- <span data-ttu-id="ccc6e-131">_`IDisposableAsync`，而不是 `IAsyncDisposable`_ ：我們已遵循將專案/類型命名為「非同步事物」，而作業是「完成非同步」的動作，因此，類型會以 "async" 做為前置詞，而方法會以 "async" 作為尾碼。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-131">_`IDisposableAsync` instead of `IAsyncDisposable`_: We've been following the naming that things/types are an "async something" whereas operations are "done async", so types have "Async" as a prefix and methods have "Async" as a suffix.</span></span>

### <a name="iasyncenumerable--iasyncenumerator"></a><span data-ttu-id="ccc6e-132">IAsyncEnumerable/IAsyncEnumerator</span><span class="sxs-lookup"><span data-stu-id="ccc6e-132">IAsyncEnumerable / IAsyncEnumerator</span></span>

<span data-ttu-id="ccc6e-133">核心 .NET 程式庫中新增了兩個介面：</span><span class="sxs-lookup"><span data-stu-id="ccc6e-133">Two interfaces are added to the core .NET libraries:</span></span>

```csharp
namespace System.Collections.Generic
{
    public interface IAsyncEnumerable<out T>
    {
        IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken cancellationToken = default);
    }

    public interface IAsyncEnumerator<out T> : IAsyncDisposable
    {
        ValueTask<bool> MoveNextAsync();
        T Current { get; }
    }
}
```

<span data-ttu-id="ccc6e-134">一般耗用量（不含其他語言功能）看起來會像這樣：</span><span class="sxs-lookup"><span data-stu-id="ccc6e-134">Typical consumption (without additional language features) would look like:</span></span>

```csharp
IAsyncEnumerator<T> enumerator = enumerable.GetAsyncEnumerator();
try
{
    while (await enumerator.MoveNextAsync())
    {
        Use(enumerator.Current);
    }
}
finally { await enumerator.DisposeAsync(); }
```

<span data-ttu-id="ccc6e-135">被視為的捨棄選項：</span><span class="sxs-lookup"><span data-stu-id="ccc6e-135">Discarded options considered:</span></span>
- <span data-ttu-id="ccc6e-136">_`Task<bool> MoveNextAsync(); T current { get; }`_ ：使用 `Task<bool>` 可支援使用快取的工作物件來代表同步、成功的 `MoveNextAsync` 呼叫，但非同步完成時仍然需要配置。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-136">_`Task<bool> MoveNextAsync(); T current { get; }`_: Using `Task<bool>` would support using a cached task object to represent synchronous, successful `MoveNextAsync` calls, but an allocation would still be required for asynchronous completion.</span></span>  <span data-ttu-id="ccc6e-137">藉由傳回 `ValueTask<bool>`，我們可以讓列舉值物件本身實作為 `IValueTaskSource<bool>`，並做為 `MoveNextAsync`所傳回 `ValueTask<bool>` 的支援，進而大幅降低負荷。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-137">By returning `ValueTask<bool>`, we enable the enumerator object to itself implement `IValueTaskSource<bool>` and be used as the backing for the `ValueTask<bool>` returned from `MoveNextAsync`, which in turn allows for significantly reduced overheads.</span></span>
- <span data-ttu-id="ccc6e-138">_`ValueTask<(bool, T)> MoveNextAsync();`_ ：使用並不難，但這表示 `T` 不會再是「協變數」。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-138">_`ValueTask<(bool, T)> MoveNextAsync();`_: It's not only harder to consume, but it means that `T` can no longer be covariant.</span></span>
- <span data-ttu-id="ccc6e-139">_`ValueTask<T?> TryMoveNextAsync();`_ ：不是協變數。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-139">_`ValueTask<T?> TryMoveNextAsync();`_: Not covariant.</span></span>
- <span data-ttu-id="ccc6e-140">_`Task<T?> TryMoveNextAsync();`_ ：不是協變數、每個呼叫上的配置等等。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-140">_`Task<T?> TryMoveNextAsync();`_: Not covariant, allocations on every call, etc.</span></span>
- <span data-ttu-id="ccc6e-141">_`ITask<T?> TryMoveNextAsync();`_ ：不是協變數、每個呼叫上的配置等等。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-141">_`ITask<T?> TryMoveNextAsync();`_: Not covariant, allocations on every call, etc.</span></span>
- <span data-ttu-id="ccc6e-142">_`ITask<(bool,T)> TryMoveNextAsync();`_ ：不是協變數、每個呼叫上的配置等等。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-142">_`ITask<(bool,T)> TryMoveNextAsync();`_: Not covariant, allocations on every call, etc.</span></span>
- <span data-ttu-id="ccc6e-143">_`Task<bool> TryMoveNextAsync(out T result);`_ ：當作業以同步方式傳回時，必須設定 `out` 的結果，而不是以非同步方式完成工作，未來可能會有一段時間很久，此時就無法傳達結果。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-143">_`Task<bool> TryMoveNextAsync(out T result);`_: The `out` result would need to be set when the operation returns synchronously, not when it asynchronously completes the task potentially sometime long in the future, at which point there'd be no way to communicate the result.</span></span>
- <span data-ttu-id="ccc6e-144">_`IAsyncEnumerator<T>` 不會執行 `IAsyncDisposable`_ ：我們可以選擇分隔這些。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-144">_`IAsyncEnumerator<T>` not implementing `IAsyncDisposable`_: We could choose to separate these.</span></span>  <span data-ttu-id="ccc6e-145">不過，這麼做會使提議中的某些其他區域變得複雜，因為程式碼必須能夠處理列舉值無法提供處置的可能性，因此很難以撰寫以模式為基礎的協助程式。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-145">However, doing so complicates certain other areas of the proposal, as code must then be able to deal with the possibility that an enumerator doesn't provide disposal, which makes it difficult to write pattern-based helpers.</span></span>  <span data-ttu-id="ccc6e-146">此外，列舉值通常會有需要處置的需求（例如，具有 finally 區塊的C#任何非同步反覆運算器、從網路連接列舉資料的大部分專案等等），如果沒有的話，就可以簡單地將方法實作為 `public ValueTask DisposeAsync() => default(ValueTask);`，只需最少的額外負荷。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-146">Further, it will be common for enumerators to have a need for disposal (e.g. any C# async iterator that has a finally block, most things enumerating data from a network connection, etc.), and if one doesn't, it is simple to implement the method purely as `public ValueTask DisposeAsync() => default(ValueTask);` with minimal additional overhead.</span></span>
- <span data-ttu-id="ccc6e-147">_ `IAsyncEnumerator<T> GetAsyncEnumerator()`：沒有解除標記參數。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-147">_ `IAsyncEnumerator<T> GetAsyncEnumerator()`: No cancellation token parameter.</span></span>

#### <a name="viable-alternative"></a><span data-ttu-id="ccc6e-148">可行的替代方案：</span><span class="sxs-lookup"><span data-stu-id="ccc6e-148">Viable alternative:</span></span>

```csharp
namespace System.Collections.Generic
{
    public interface IAsyncEnumerable<out T>
    {
        IAsyncEnumerator<T> GetAsyncEnumerator();
    }

    public interface IAsyncEnumerator<out T> : IAsyncDisposable
    {
        ValueTask<bool> WaitForNextAsync();
        T TryGetNext(out bool success);
    }
}
```

<span data-ttu-id="ccc6e-149">在內部迴圈中使用 `TryGetNext`，只要它們可以同步使用，就能取用具有單一介面呼叫的專案。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-149">`TryGetNext` is used in an inner loop to consume items with a single interface call as long as they're available synchronously.</span></span>  <span data-ttu-id="ccc6e-150">當下一個專案無法以同步方式抓取時，它會傳回 false，而且每當傳回 false 時，呼叫端都必須叫用 `WaitForNextAsync`，等候下一個專案可以使用，或判斷永遠不會有另一個專案。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-150">When the next item can't be retrieved synchronously, it returns false, and any time it returns false, a caller must subsequently invoke `WaitForNextAsync` to either wait for the next item to be available or to determine that there will never be another item.</span></span> <span data-ttu-id="ccc6e-151">一般耗用量（不含其他語言功能）看起來會像這樣：</span><span class="sxs-lookup"><span data-stu-id="ccc6e-151">Typical consumption (without additional language features) would look like:</span></span>

```csharp
IAsyncEnumerator<T> enumerator = enumerable.GetAsyncEnumerator();
try
{
    while (await enumerator.WaitForNextAsync())
    {
        while (true)
        {
            int item = enumerator.TryGetNext(out bool success);
            if (!success) break;
            Use(item);
        }
    }
}
finally { await enumerator.DisposeAsync(); }
```

<span data-ttu-id="ccc6e-152">這是兩折的優點，一個次要和一個主要：</span><span class="sxs-lookup"><span data-stu-id="ccc6e-152">The advantage of this is two-fold, one minor and one major:</span></span>
- <span data-ttu-id="ccc6e-153">_次要：允許枚舉器支援多個取用者_。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-153">_Minor: Allows for an enumerator to support multiple consumers_.</span></span> <span data-ttu-id="ccc6e-154">在某些情況下，列舉值可以支援多個並行取用者。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-154">There may be scenarios where it's valuable for an enumerator to support multiple concurrent consumers.</span></span>  <span data-ttu-id="ccc6e-155">當 `MoveNextAsync` 和 `Current` 分開時，就無法達成此目的，因為實現無法使其使用不可部分完成。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-155">That can't be achieved when `MoveNextAsync` and `Current` are separate such that an implementation can't make their usage atomic.</span></span>  <span data-ttu-id="ccc6e-156">相反地，這個方法會提供單一方法 `TryGetNext`，支援將列舉值向前推送並取得下一個專案，因此枚舉器可以在需要時啟用不可部分完成性。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-156">In contrast, this approach provides a single method `TryGetNext` that supports pushing the enumerator forward and getting the next item, so the enumerator can enable atomicity if desired.</span></span>  <span data-ttu-id="ccc6e-157">不過，這種情況可能也會藉由提供每個取用者自己的來自共用可列舉枚舉器來啟用。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-157">However, it's likely that such scenarios could also be enabled by giving each consumer its own enumerator from a shared enumerable.</span></span>  <span data-ttu-id="ccc6e-158">此外，我們不想要強制每個列舉值都支援並行使用，因為這會對不需要它的多數案例新增非一般的負荷，這表示介面的取用者通常無法以這種方式依賴這種情況。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-158">Further, we don't want to enforce that every enumerator support concurrent usage, as that would add non-trivial overheads to the majority case that doesn't require it, which means a consumer of the interface generally couldn't rely on this any way.</span></span>
- <span data-ttu-id="ccc6e-159">_主要：效能_。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-159">_Major: Performance_.</span></span> <span data-ttu-id="ccc6e-160">`MoveNextAsync`/`Current` 的方法，每個作業都需要兩個介面呼叫，而 `WaitForNextAsync`/`TryGetNext` 的最佳案例是，大部分的反復專案都是以同步方式完成，以 `TryGetNext`啟用緊密的內部迴圈，因此每個作業只有一個介面呼叫。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-160">The `MoveNextAsync`/`Current` approach requires two interface calls per operation, whereas the best case for `WaitForNextAsync`/`TryGetNext` is that most iterations complete synchronously, enabling a tight inner loop with `TryGetNext`, such that we only have one interface call per operation.</span></span>  <span data-ttu-id="ccc6e-161">當介面呼叫佔據運算的情況時，這可能會有顯著的影響。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-161">This can have a measurable impact in situations where the interface calls dominate the computation.</span></span>

<span data-ttu-id="ccc6e-162">不過，有一些非一般的缺點，包括以手動方式使用這些專案時的複雜性大幅增加，而且在使用 bug 時，可能會增加錯誤的機率。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-162">However, there are non-trivial downsides, including significantly increased complexity when consuming these manually, and an increased chance of introducing bugs when using them.</span></span>  <span data-ttu-id="ccc6e-163">雖然效能優勢會出現在 microbenchmarks 中，但我們並不相信它們會在大部分的實際使用方式下影響力。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-163">And while the performance benefits show up in microbenchmarks, we don't believe they'll be impactful in the vast majority of real usage.</span></span>  <span data-ttu-id="ccc6e-164">如果結果是，我們可以用淺的方式引進第二組介面。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-164">If it turns out they are, we can introduce a second set of interfaces in a light-up fashion.</span></span>

<span data-ttu-id="ccc6e-165">被視為的捨棄選項：</span><span class="sxs-lookup"><span data-stu-id="ccc6e-165">Discarded options considered:</span></span>
- <span data-ttu-id="ccc6e-166">`ValueTask<bool> WaitForNextAsync(); bool TryGetNext(out T result);`： `out` 參數不可以是不可變的。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-166">`ValueTask<bool> WaitForNextAsync(); bool TryGetNext(out T result);`: `out` parameters can't be covariant.</span></span>  <span data-ttu-id="ccc6e-167">這裡也有些許影響（一般 try 模式的問題），這可能會產生參考型別結果的執行時間寫入屏障。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-167">There's also a small impact here (an issue with the try pattern in general) that this likely incurs a runtime write barrier for reference type results.</span></span>

#### <a name="cancellation"></a><span data-ttu-id="ccc6e-168">取消</span><span class="sxs-lookup"><span data-stu-id="ccc6e-168">Cancellation</span></span>

<span data-ttu-id="ccc6e-169">有數種可能的方法可支援取消：</span><span class="sxs-lookup"><span data-stu-id="ccc6e-169">There are several possible approaches to supporting cancellation:</span></span>
1. <span data-ttu-id="ccc6e-170">`IAsyncEnumerable<T>`/`IAsyncEnumerator<T>` 不相容： `CancellationToken` 不會出現在任何位置。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-170">`IAsyncEnumerable<T>`/`IAsyncEnumerator<T>` are cancellation-agnostic: `CancellationToken` doesn't appear anywhere.</span></span>  <span data-ttu-id="ccc6e-171">取消的達成方式是以邏輯方式，將 `CancellationToken` 烘焙到可列舉的和（或）枚舉器中，例如呼叫反覆運算器、將 `CancellationToken` 當做引數傳遞至 iterator 方法，並在反覆運算器主體中使用它，就像使用任何其他參數一樣。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-171">Cancellation is achieved by logically baking the `CancellationToken` into the enumerable and/or enumerator in whatever manner is appropriate, e.g. when calling an iterator, passing the `CancellationToken` as an argument to the iterator method and using it in the body of the iterator, as is done with any other parameter.</span></span>
2. <span data-ttu-id="ccc6e-172">`IAsyncEnumerator<T>.GetAsyncEnumerator(CancellationToken)`：您會將 `CancellationToken` 傳遞給 `GetAsyncEnumerator`，而後續的 `MoveNextAsync` 作業則會遵循此動作。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-172">`IAsyncEnumerator<T>.GetAsyncEnumerator(CancellationToken)`: You pass a `CancellationToken` to `GetAsyncEnumerator`, and subsequent `MoveNextAsync` operations respect it however it can.</span></span>
3. <span data-ttu-id="ccc6e-173">`IAsyncEnumerator<T>.MoveNextAsync(CancellationToken)`：您會將 `CancellationToken` 傳遞給每個個別的 `MoveNextAsync` 呼叫。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-173">`IAsyncEnumerator<T>.MoveNextAsync(CancellationToken)`: You pass a `CancellationToken` to each individual `MoveNextAsync` call.</span></span>
4. <span data-ttu-id="ccc6e-174">1 & & 2：您會在可列舉/枚舉器中內嵌 `CancellationToken`，並將 `CancellationToken`傳遞至 `GetAsyncEnumerator`。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-174">1 && 2: You both embed `CancellationToken`s into your enumerable/enumerator and pass `CancellationToken`s into `GetAsyncEnumerator`.</span></span>
5. <span data-ttu-id="ccc6e-175">1 & & 3：您可以在可列舉/枚舉器中內嵌 `CancellationToken`，並將 `CancellationToken`傳遞至 `MoveNextAsync`。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-175">1 && 3: You both embed `CancellationToken`s into your enumerable/enumerator and pass `CancellationToken`s into `MoveNextAsync`.</span></span>

<span data-ttu-id="ccc6e-176">從純粹理論的觀點來看，（5）是最穩固的，在此（a）中，`MoveNextAsync` 接受 `CancellationToken` 可讓您對取消的內容進行最精細的控制，而（b） `CancellationToken` 只是可當做引數傳遞至反覆運算器、內嵌在任意類型等中的任何其他類型。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-176">From a purely theoretical perspective, (5) is the most robust, in that (a) `MoveNextAsync` accepting a `CancellationToken` enables the most fine-grained control over what's canceled, and (b) `CancellationToken` is just any other type that can passed as an argument into iterators, embedded in arbitrary types, etc.</span></span>

<span data-ttu-id="ccc6e-177">不過，這種方法有多個問題：</span><span class="sxs-lookup"><span data-stu-id="ccc6e-177">However, there are multiple problems with that approach:</span></span>
- <span data-ttu-id="ccc6e-178">如何將 `CancellationToken` 傳遞至 `GetAsyncEnumerator` 讓它進入 iterator 的主體？</span><span class="sxs-lookup"><span data-stu-id="ccc6e-178">How does a `CancellationToken` passed to `GetAsyncEnumerator` make it into the body of the iterator?</span></span>  <span data-ttu-id="ccc6e-179">我們可以公開新的 `iterator` 關鍵字，讓您可以從點開始，以存取傳遞給 `GetEnumerator`的 `CancellationToken`。但是，這是很多額外的機械，b）我們會使其成為非常頂級的公民，c）99% 的案例似乎是相同的程式碼，兩者都呼叫 iterator 並在其上呼叫 `GetAsyncEnumerator`，在此情況下，它只會將 `CancellationToken` 做為引數傳遞至方法。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-179">We could expose a new `iterator` keyword that you could dot off of to get access to the `CancellationToken` passed to `GetEnumerator`, but a) that's a lot of additional machinery, b) we're making it a very first-class citizen, and c) the 99% case would seem to be the same code both calling an iterator and calling `GetAsyncEnumerator` on it, in which case it can just pass the `CancellationToken` as an argument into the method.</span></span>
- <span data-ttu-id="ccc6e-180">如何將 `CancellationToken` 傳遞至 `MoveNextAsync` 進入方法的主體？</span><span class="sxs-lookup"><span data-stu-id="ccc6e-180">How does a `CancellationToken` passed to `MoveNextAsync` get into the body of the method?</span></span>  <span data-ttu-id="ccc6e-181">這種情況更糟，因為它是公開在 `iterator` 的本機物件之外，所以它的值可能會在等候期間變更，這表示任何已註冊權杖的程式碼都必須在等候之前取消註冊，然後再于之後重新登錄;無論是在每個 `MoveNextAsync` 呼叫中進行註冊和取消註冊，都可能需要耗費大量資源，不論編譯器是由反覆運算器或開發人員手動執行。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-181">This is even worse, as if it's exposed off of an `iterator` local object, its value could change across awaits, which means any code that registered with the token would need to unregister from it prior to awaits and then re-register after; it's also potentially quite expensive to need to do such registering and unregistering in every `MoveNextAsync` call, regardless of whether implemented by the compiler in an iterator or by a developer manually.</span></span>
- <span data-ttu-id="ccc6e-182">開發人員如何取消 `foreach` 迴圈？</span><span class="sxs-lookup"><span data-stu-id="ccc6e-182">How does a developer cancel a `foreach` loop?</span></span>  <span data-ttu-id="ccc6e-183">如果它是藉由提供 `CancellationToken` 給可列舉/枚舉器，則是），我們需要支援對列舉值執行 `foreach`'。這會將它們提升為第一方公民，而現在您需要開始思考在列舉值（例如 LINQ 方法）或 b）上所建立的生態系統，我們需要在可儲存所提供權杖的 `IAsyncEnumerable<T>` 以外的部分 `WithCancellation` 擴充方法，然後將它傳遞到包裝的可列舉的 `GetAsyncEnumerator` 中，然後在傳回的結構上 `GetAsyncEnumerator` 時，將 `CancellationToken` 內嵌會叫用（忽略該 token）。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-183">If it's done by giving a `CancellationToken` to an enumerable/enumerator, then either a) we need to support `foreach`'ing over enumerators, which raises them to being first-class citizens, and now you need to start thinking about an ecosystem built up around enumerators (e.g. LINQ methods) or b) we need to embed the `CancellationToken` in the enumerable anyway by having some `WithCancellation` extension method off of `IAsyncEnumerable<T>` that would store the provided token and then pass it into  the wrapped enumerable's `GetAsyncEnumerator` when the `GetAsyncEnumerator` on the returned struct is invoked (ignoring that token).</span></span>  <span data-ttu-id="ccc6e-184">或者，您可以只使用 foreach 主體中的 `CancellationToken`。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-184">Or, you can just use the `CancellationToken` you have in the body of the foreach.</span></span>
- <span data-ttu-id="ccc6e-185">如果支援查詢智力，則如何將 `CancellationToken` 提供給 `GetEnumerator` 或 `MoveNextAsync` 傳遞至每個子句？</span><span class="sxs-lookup"><span data-stu-id="ccc6e-185">If/when query comprehensions are supported, how would the `CancellationToken` supplied to `GetEnumerator` or `MoveNextAsync` be passed into each clause?</span></span>  <span data-ttu-id="ccc6e-186">最簡單的方式就是讓子句來加以捕捉，而將任何權杖傳遞至 `GetAsyncEnumerator`/`MoveNextAsync` 會被忽略。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-186">The easiest way would simply be for the clause to capture it, at which point whatever token is passed to `GetAsyncEnumerator`/`MoveNextAsync` is ignored.</span></span>

<span data-ttu-id="ccc6e-187">建議使用此檔的較早版本（1），但我們已切換至（4）。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-187">An earlier version of this document recommended (1), but we since switched to (4).</span></span>

<span data-ttu-id="ccc6e-188">（1）的兩個主要問題：</span><span class="sxs-lookup"><span data-stu-id="ccc6e-188">The two main problems with (1):</span></span>
- <span data-ttu-id="ccc6e-189">可取消可列舉值的產生者必須實行一些樣板化，而且只能利用編譯器的非同步反覆運算器支援來執行 `IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken)` 方法。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-189">producers of cancellable enumerables have to implement some boilerplate, and can only leverage the compiler's support for async-iterators to implement a `IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken)` method.</span></span>
- <span data-ttu-id="ccc6e-190">許多產生者可能會想要改為將 `CancellationToken` 參數新增至其非同步可列舉簽章，這會防止取用者在提供 `IAsyncEnumerable` 類型時，傳遞所需的取消權杖。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-190">it is likely that many producers would be tempted to just add a `CancellationToken` parameter to their async-enumerable signature instead, which will prevent consumers from passing the cancellation token they want when they are given an `IAsyncEnumerable` type.</span></span>

<span data-ttu-id="ccc6e-191">主要的耗用量案例有兩種：</span><span class="sxs-lookup"><span data-stu-id="ccc6e-191">There are two main consumption scenarios:</span></span>
1. <span data-ttu-id="ccc6e-192">`await foreach (var i in GetData(token)) ...` 取用者呼叫非同步反覆運算器方法的位置，</span><span class="sxs-lookup"><span data-stu-id="ccc6e-192">`await foreach (var i in GetData(token)) ...` where the consumer calls the async-iterator method,</span></span>
2. <span data-ttu-id="ccc6e-193">`await foreach (var i in givenIAsyncEnumerable.WithCancellation(token)) ...` 取用者處理給定 `IAsyncEnumerable` 實例的位置。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-193">`await foreach (var i in givenIAsyncEnumerable.WithCancellation(token)) ...` where the consumer deals with a given `IAsyncEnumerable` instance.</span></span>

<span data-ttu-id="ccc6e-194">我們發現有一項合理的危害可支援這兩種案例，這種方式對非同步資料流程的生產者和取用者而言都很方便，就是在非同步反覆運算器方法中使用特別標注的參數。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-194">We find that a reasonable compromise to support both scenarios in a way that is convenient for both producers and consumers of async-streams is to use a specially annotated parameter in the async-iterator method.</span></span> <span data-ttu-id="ccc6e-195">`[EnumeratorCancellation]` 屬性是用於此用途。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-195">The `[EnumeratorCancellation]` attribute is used for this purpose.</span></span> <span data-ttu-id="ccc6e-196">將這個屬性放在參數上會告訴編譯器，如果將權杖傳遞給 `GetAsyncEnumerator` 方法，就應該使用該 token，而不是原本針對參數傳遞的值。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-196">Placing this attribute on a parameter tells the compiler that if a token is passed to the `GetAsyncEnumerator` method, that token should be used instead of the value originally passed for the parameter.</span></span>

<span data-ttu-id="ccc6e-197">請考慮 `IAsyncEnumerable<int> GetData([EnumeratorCancellation] CancellationToken token = default)`。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-197">Consider `IAsyncEnumerable<int> GetData([EnumeratorCancellation] CancellationToken token = default)`.</span></span> <span data-ttu-id="ccc6e-198">這個方法的實施者可以直接在方法主體中使用參數。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-198">The implementer of this method can simply use the parameter in the method body.</span></span> <span data-ttu-id="ccc6e-199">取用者可以使用上述任一耗用量模式：</span><span class="sxs-lookup"><span data-stu-id="ccc6e-199">The consumer can use either consumption patterns above:</span></span>
1. <span data-ttu-id="ccc6e-200">如果您使用 `GetData(token)`，則權杖會儲存至非同步可列舉，並會在反復專案中使用。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-200">if you use `GetData(token)`, then the token is saved into the async-enumerable and will be used in iteration,</span></span>
2. <span data-ttu-id="ccc6e-201">如果您使用 `givenIAsyncEnumerable.WithCancellation(token)`，傳遞至 `GetAsyncEnumerator` 的權杖將會取代任何儲存在非同步可列舉中的 token。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-201">if you use `givenIAsyncEnumerable.WithCancellation(token)`, then the token passed to `GetAsyncEnumerator` will supersede any token saved in the async-enumerable.</span></span>

## <a name="foreach"></a><span data-ttu-id="ccc6e-202">foreach</span><span class="sxs-lookup"><span data-stu-id="ccc6e-202">foreach</span></span>

<span data-ttu-id="ccc6e-203">除了其現有的 `IEnumerable<T>`支援之外，也會增強 `foreach` 以支援 `IAsyncEnumerable<T>`。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-203">`foreach` will be augmented to support `IAsyncEnumerable<T>` in addition to its existing support for `IEnumerable<T>`.</span></span>  <span data-ttu-id="ccc6e-204">而且，如果相關的成員公開公開，又回到直接使用介面（如果沒有的話），則會支援對等的 `IAsyncEnumerable<T>` 做為模式，以啟用以結構為基礎的延伸模組，以避免配置及使用替代的 awaitables 做為 `MoveNextAsync` 和 `DisposeAsync`的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-204">And it will support the equivalent of `IAsyncEnumerable<T>` as a pattern if the relevant members are exposed publicly, falling back to using the interface directly if not, in order to enable struct-based extensions that avoid allocating as well as using alternative awaitables as the return type of `MoveNextAsync` and `DisposeAsync`.</span></span>

### <a name="syntax"></a><span data-ttu-id="ccc6e-205">語法</span><span class="sxs-lookup"><span data-stu-id="ccc6e-205">Syntax</span></span>

<span data-ttu-id="ccc6e-206">使用語法：</span><span class="sxs-lookup"><span data-stu-id="ccc6e-206">Using the syntax:</span></span>

```csharp
foreach (var i in enumerable)
```

<span data-ttu-id="ccc6e-207">C#會繼續將 `enumerable` 視為同步可列舉，即使它公開非同步可列舉值的相關 Api （公開模式或執行介面），它還是只會考慮同步 Api。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-207">C# will continue to treat `enumerable` as a synchronous enumerable, such that even if it exposes the relevant APIs for async enumerables (exposing the pattern or implementing the interface), it will only consider the synchronous APIs.</span></span>

<span data-ttu-id="ccc6e-208">若要強制 `foreach` 改為只考慮非同步 Api，`await` 插入如下：</span><span class="sxs-lookup"><span data-stu-id="ccc6e-208">To force `foreach` to instead only consider the asynchronous APIs, `await` is inserted as follows:</span></span>

```csharp
await foreach (var i in enumerable)
```

<span data-ttu-id="ccc6e-209">不會提供支援使用 async 或 sync Api 的語法;開發人員必須根據所使用的語法進行選擇。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-209">No syntax would be provided that would support using either the async or the sync APIs; the developer must choose based on the syntax used.</span></span>

<span data-ttu-id="ccc6e-210">被視為的捨棄選項：</span><span class="sxs-lookup"><span data-stu-id="ccc6e-210">Discarded options considered:</span></span>
- <span data-ttu-id="ccc6e-211">_`foreach (var i in await enumerable)`_ ：這已經是有效的語法，而變更其意義會是中斷性變更。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-211">_`foreach (var i in await enumerable)`_: This is already valid syntax, and changing its meaning would be a breaking change.</span></span>  <span data-ttu-id="ccc6e-212">這表示 `await` `enumerable`，請從其同步可反復執行，然後以同步方式逐一查看該問題。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-212">This means to `await` the `enumerable`, get back something synchronously iterable from it, and then synchronously iterate through that.</span></span>
- <span data-ttu-id="ccc6e-213">_`foreach (var i await in enumerable)`、`foreach (var await i in enumerable)``foreach (await var i in enumerable)`_ ：這些全都建議我們等待下一個專案，但有其他與 foreach 相關的等候，特別是當可列舉為 `IAsyncDisposable`時，我們將會 `await`' 進行非同步處置。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-213">_`foreach (var i await in enumerable)`, `foreach (var await i in enumerable)`, `foreach (await var i in enumerable)`_: These all suggest that we're awaiting the next item, but there are other awaits involved in foreach, in particular if the enumerable is an `IAsyncDisposable`, we will be `await`'ing its async disposal.</span></span>  <span data-ttu-id="ccc6e-214">該 await 會當做 foreach 的範圍，而不是每個個別的元素，因此 `await` 關鍵字應該是 `foreach` 層級。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-214">That await is as the scope of the foreach rather than for each individual element, and thus the `await` keyword deserves to be at the `foreach` level.</span></span>  <span data-ttu-id="ccc6e-215">此外，讓它與 `foreach` 相關聯，讓我們能夠以不同的詞彙來描述 `foreach`，例如「await foreach」。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-215">Further, having it associated with the `foreach` gives us a way to describe the `foreach` with a different term, e.g. a "await foreach".</span></span>  <span data-ttu-id="ccc6e-216">但更重要的是，考慮 `foreach` 語法與 `using` 語法同時保持一致，因此 `using (await ...)` 已經是有效的語法。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-216">But more importantly, there's value in considering `foreach` syntax at the same time as `using` syntax, so that they remain consistent with each other, and `using (await ...)` is already valid syntax.</span></span>
- _`foreach await (var i in enumerable)`_

<span data-ttu-id="ccc6e-217">還是要考慮：</span><span class="sxs-lookup"><span data-stu-id="ccc6e-217">Still to consider:</span></span>
- <span data-ttu-id="ccc6e-218">`foreach` 今天不支援逐一查看列舉值。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-218">`foreach` today does not support iterating through an enumerator.</span></span>  <span data-ttu-id="ccc6e-219">我們預期會有更常見的 `IAsyncEnumerator<T>`，因此同時支援 `IAsyncEnumerable<T>` 和 `IAsyncEnumerator<T>`的 `await foreach`。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-219">We expect it will be more common to have `IAsyncEnumerator<T>`s handed around, and thus it's tempting to support `await foreach` with both `IAsyncEnumerable<T>` and `IAsyncEnumerator<T>`.</span></span>  <span data-ttu-id="ccc6e-220">但是在我們新增這類支援之後，它會介紹 `IAsyncEnumerator<T>` 是否為第一方公民的問題，以及是否除了可列舉值以外，是否還需要在列舉值上運作的組合器多載？</span><span class="sxs-lookup"><span data-stu-id="ccc6e-220">But once we add such support, it introduces the question of whether `IAsyncEnumerator<T>` is a first-class citizen, and whether we need to have overloads of combinators that operate on enumerators in addition to enumerables?</span></span>    <span data-ttu-id="ccc6e-221">我們想要鼓勵方法傳回枚舉器，而不是可列舉值嗎？</span><span class="sxs-lookup"><span data-stu-id="ccc6e-221">Do we want to encourage methods to return enumerators rather than enumerables?</span></span> <span data-ttu-id="ccc6e-222">我們應該繼續討論這一點。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-222">We should continue to discuss this.</span></span>  <span data-ttu-id="ccc6e-223">如果我們決定不想要支援它，我們可能會想要引進擴充方法 `public static IAsyncEnumerable<T> AsEnumerable<T>(this IAsyncEnumerator<T> enumerator);`，讓列舉值仍然 `foreach`。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-223">If we decide we don't want to support it, we might want to introduce an extension method `public static IAsyncEnumerable<T> AsEnumerable<T>(this IAsyncEnumerator<T> enumerator);` that would allow an enumerator to still be `foreach`'d.</span></span>  <span data-ttu-id="ccc6e-224">如果我們決定要支援它，我們也必須決定 `await foreach` 是否會負責呼叫列舉值的 `DisposeAsync`，而答案可能是「否，控制處置的人應該要由呼叫 `GetEnumerator`的人員處理。」</span><span class="sxs-lookup"><span data-stu-id="ccc6e-224">If we decide we do want to support it, we'll need to also decide on whether the `await foreach` would be responsible for calling `DisposeAsync` on the enumerator, and the answer is likely "no, control over disposal should be handled by whoever called `GetEnumerator`."</span></span>

### <a name="pattern-based-compilation"></a><span data-ttu-id="ccc6e-225">以模式為基礎的編譯</span><span class="sxs-lookup"><span data-stu-id="ccc6e-225">Pattern-based Compilation</span></span>

<span data-ttu-id="ccc6e-226">編譯器會系結至以模式為基礎的 Api （如果有的話），並偏好使用介面（此模式可能符合實例方法或擴充方法）。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-226">The compiler will bind to the pattern-based APIs if they exist, preferring those over using the interface (the pattern may be satisfied with instance methods or extension methods).</span></span>  <span data-ttu-id="ccc6e-227">模式的需求如下：</span><span class="sxs-lookup"><span data-stu-id="ccc6e-227">The requirements for the pattern are:</span></span>
- <span data-ttu-id="ccc6e-228">可列舉必須公開不含引數呼叫的 `GetAsyncEnumerator` 方法，並傳回符合相關模式的列舉值。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-228">The enumerable must expose a `GetAsyncEnumerator` method that may be called with no arguments and that returns an enumerator that meets the relevant pattern.</span></span>
- <span data-ttu-id="ccc6e-229">列舉值必須公開不使用引數呼叫的 `MoveNextAsync` 方法，並傳回可能是 `await`ed 且其 `GetResult()` 傳回 `bool`的專案。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-229">The enumerator must expose a `MoveNextAsync` method that may be called with no arguments and that returns something which may be `await`ed and whose `GetResult()` returns a `bool`.</span></span>
- <span data-ttu-id="ccc6e-230">列舉值也必須公開 `Current` 屬性，其 getter 會傳回代表所列舉資料類型的 `T`。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-230">The enumerator must also expose `Current` property whose getter returns a `T` representing the kind of data being enumerated.</span></span>
- <span data-ttu-id="ccc6e-231">列舉值可能會選擇性地公開不使用引數叫用的 `DisposeAsync` 方法，並傳回可以 `await`ed 且其 `GetResult()` 會傳回 `void`的內容。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-231">The enumerator may optionally expose a `DisposeAsync` method that may be invoked with no arguments and that returns something that can be `await`ed and whose `GetResult()` returns `void`.</span></span>

<span data-ttu-id="ccc6e-232">此程式碼：</span><span class="sxs-lookup"><span data-stu-id="ccc6e-232">This code:</span></span>

```csharp
var enumerable = ...;
await foreach (T item in enumerable)
{
   ...
}
```

<span data-ttu-id="ccc6e-233">會轉譯為對等的：</span><span class="sxs-lookup"><span data-stu-id="ccc6e-233">is translated to the equivalent of:</span></span>

```csharp
var enumerable = ...;
var enumerator = enumerable.GetAsyncEnumerator();
try
{
    while (await enumerator.MoveNextAsync())
    {
       T item = enumerator.Current;
       ...
    }
}
finally
{
    await enumerator.DisposeAsync(); // omitted, along with the try/finally, if the enumerator doesn't expose DisposeAsync
}
```

<span data-ttu-id="ccc6e-234">如果反復查看的類型未公開正確的模式，則會使用介面。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-234">If the iterated type doesn't expose the right pattern, the interfaces will be used.</span></span>

### <a name="configureawait"></a><span data-ttu-id="ccc6e-235">ConfigureAwait</span><span class="sxs-lookup"><span data-stu-id="ccc6e-235">ConfigureAwait</span></span>

<span data-ttu-id="ccc6e-236">這個以模式為基礎的編譯可讓您透過 `ConfigureAwait` 擴充方法，在所有等候上使用 `ConfigureAwait`：</span><span class="sxs-lookup"><span data-stu-id="ccc6e-236">This pattern-based compilation will allow `ConfigureAwait` to be used on all of the awaits, via a `ConfigureAwait` extension method:</span></span>

```csharp
await foreach (T item in enumerable.ConfigureAwait(false))
{
   ...
}
```

<span data-ttu-id="ccc6e-237">這會以我們也會新增至 .NET 的類型為基礎，可能會加入至 [System.web]：</span><span class="sxs-lookup"><span data-stu-id="ccc6e-237">This will be based on types we'll add to .NET as well, likely to System.Threading.Tasks.Extensions.dll:</span></span>

```csharp
// Approximate implementation, omitting arg validation and the like
namespace System.Threading.Tasks
{
    public static class AsyncEnumerableExtensions
    {
        public static ConfiguredAsyncEnumerable<T> ConfigureAwait<T>(this IAsyncEnumerable<T> enumerable, bool continueOnCapturedContext) =>
            new ConfiguredAsyncEnumerable<T>(enumerable, continueOnCapturedContext);

        public struct ConfiguredAsyncEnumerable<T>
        {
            private readonly IAsyncEnumerable<T> _enumerable;
            private readonly bool _continueOnCapturedContext;

            internal ConfiguredAsyncEnumerable(IAsyncEnumerable<T> enumerable, bool continueOnCapturedContext)
            {
                _enumerable = enumerable;
                _continueOnCapturedContext = continueOnCapturedContext;
            }

            public ConfiguredAsyncEnumerator<T> GetAsyncEnumerator() =>
                new ConfiguredAsyncEnumerator<T>(_enumerable.GetAsyncEnumerator(), _continueOnCapturedContext);

            public struct Enumerator
            {
                private readonly IAsyncEnumerator<T> _enumerator;
                private readonly bool _continueOnCapturedContext;

                internal Enumerator(IAsyncEnumerator<T> enumerator, bool continueOnCapturedContext)
                {
                    _enumerator = enumerator;
                    _continueOnCapturedContext = continueOnCapturedContext;
                }

                public ConfiguredValueTaskAwaitable<bool> MoveNextAsync() =>
                    _enumerator.MoveNextAsync().ConfigureAwait(_continueOnCapturedContext);

                public T Current => _enumerator.Current;

                public ConfiguredValueTaskAwaitable DisposeAsync() =>
                    _enumerator.DisposeAsync().ConfigureAwait(_continueOnCapturedContext);
            }
        }
    }
}
```

<span data-ttu-id="ccc6e-238">請注意，這種方法並不會讓 `ConfigureAwait` 與以模式為基礎的可列舉值搭配使用。但再次強調，`ConfigureAwait` 只會公開為 `Task`/`Task<T>`/`ValueTask`/`ValueTask<T>` 的延伸模組，而且不能套用至任意可等候的專案，因為它只適用于套用至工作的情況（它會控制工作的接續支援中所實的行為），因此在使用模式時，可等候專案可能不會有任何意義。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-238">Note that this approach will not enable `ConfigureAwait` to be used with pattern-based enumerables, but then again it's already the case that the `ConfigureAwait` is only exposed as an extension on `Task`/`Task<T>`/`ValueTask`/`ValueTask<T>` and can't be applied to arbitrary awaitable things, as it only makes sense when applied to Tasks (it controls a behavior implemented in Task's continuation support), and thus doesn't make sense when using a pattern where the awaitable things may not be tasks.</span></span>  <span data-ttu-id="ccc6e-239">傳回可等候專案的任何人都可以在這類 advanced 案例中提供自己的自訂行為。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-239">Anyone returning awaitable things can provide their own custom behavior in such advanced scenarios.</span></span>

<span data-ttu-id="ccc6e-240">（如果我們可以提供一些方法來支援範圍或元件層級的 `ConfigureAwait` 解決方案，那麼就不需要這麼做）。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-240">(If we can come up with some way to support a scope- or assembly-level `ConfigureAwait` solution, then this won't be necessary.)</span></span>

## <a name="async-iterators"></a><span data-ttu-id="ccc6e-241">非同步反覆運算器</span><span class="sxs-lookup"><span data-stu-id="ccc6e-241">Async Iterators</span></span>

<span data-ttu-id="ccc6e-242">除了使用語言/編譯器以外，它還支援產生 `IAsyncEnumerable<T>`s 和 `IAsyncEnumerator<T>`。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-242">The language / compiler will support producing `IAsyncEnumerable<T>`s and `IAsyncEnumerator<T>`s in addition to consuming them.</span></span> <span data-ttu-id="ccc6e-243">現在語言支援撰寫反覆運算器，如下所示：</span><span class="sxs-lookup"><span data-stu-id="ccc6e-243">Today the language supports writing an iterator like:</span></span>

```csharp
static IEnumerable<int> MyIterator()
{
    try
    {
        for (int i = 0; i < 100; i++)
        {
            Thread.Sleep(1000);
            yield return i;
        }
    }
    finally
    {
        Thread.Sleep(200);
        Console.WriteLine("finally");
    }
}
```

<span data-ttu-id="ccc6e-244">但是 `await` 不能用在這些反覆運算器的主體中。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-244">but `await` can't be used in the body of these iterators.</span></span>  <span data-ttu-id="ccc6e-245">我們會新增該支援。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-245">We will add that support.</span></span>

### <a name="syntax"></a><span data-ttu-id="ccc6e-246">語法</span><span class="sxs-lookup"><span data-stu-id="ccc6e-246">Syntax</span></span>

<span data-ttu-id="ccc6e-247">反覆運算器的現有語言支援會根據它是否包含任何 `yield`來推斷方法的 iterator 本質。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-247">The existing language support for iterators infers the iterator nature of the method based on whether it contains any `yield`s.</span></span>  <span data-ttu-id="ccc6e-248">非同步反覆運算器的情況也是如此。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-248">The same will be true for async iterators.</span></span>  <span data-ttu-id="ccc6e-249">這類非同步反覆運算器將會 config.js 區分，並藉由將 `async` 加入簽章來與同步反覆運算器區別，而且也必須 `IAsyncEnumerable<T>` 或 `IAsyncEnumerator<T>` 做為其傳回型別。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-249">Such async iterators will be demarcated and differentiated from synchronous iterators via adding `async` to the signature, and must then also have either `IAsyncEnumerable<T>` or `IAsyncEnumerator<T>` as its return type.</span></span>  <span data-ttu-id="ccc6e-250">例如，上述範例可以撰寫為非同步反覆運算器，如下所示：</span><span class="sxs-lookup"><span data-stu-id="ccc6e-250">For example, the above example could be written as an async iterator as follows:</span></span>

```csharp
static async IAsyncEnumerable<int> MyIterator()
{
    try
    {
        for (int i = 0; i < 100; i++)
        {
            await Task.Delay(1000);
            yield return i;
        }
    }
    finally
    {
        await Task.Delay(200);
        Console.WriteLine("finally");
    }
}
```

<span data-ttu-id="ccc6e-251">考慮的替代方案：</span><span class="sxs-lookup"><span data-stu-id="ccc6e-251">Alternatives considered:</span></span>
- <span data-ttu-id="ccc6e-252">_不使用簽章中的 `async`_ ：編譯器在技術上可能需要使用 `async`，因為它會使用它來判斷 `await` 在該內容中是否有效。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-252">_Not using `async` in the signature_: Using `async` is likely technically required by the compiler, as it uses it to determine whether `await` is valid in that context.</span></span>  <span data-ttu-id="ccc6e-253">但是，即使不是必要的，我們也建立了 `await` 只能用在標示為 `async`的方法中，而且保持一致性似乎很重要。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-253">But even if it's not required, we've established that `await` may only be used in methods marked as `async`, and it seems important to keep the consistency.</span></span>
- <span data-ttu-id="ccc6e-254">_啟用 `IAsyncEnumerable<T>`的自訂_產生器：這是我們在未來可以查看的內容，但此機制很複雜，而且不支援同步對應的。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-254">_Enabling custom builders for `IAsyncEnumerable<T>`_:  That's something we could look at for the future, but the machinery is complicated and we don't support that for the synchronous counterparts.</span></span>
- <span data-ttu-id="ccc6e-255">在簽章_中具有 `iterator` 關鍵字_：非同步反覆運算器會在簽章中使用 `async iterator`，而 `yield` 只能用在包含 `iterator`的 `async` 方法中;然後會在同步反覆運算器上將 `iterator` 設為選擇性。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-255">_Having an `iterator` keyword in the signature_: Async iterators would use `async iterator` in the signature, and `yield` could only be used in `async` methods that included `iterator`; `iterator` would then be made optional on synchronous iterators.</span></span>  <span data-ttu-id="ccc6e-256">根據您的觀點，這有其優點，就是讓方法的簽章非常清楚，不論是否允許 `yield`，以及方法是否實際上是否要傳回 `IAsyncEnumerable<T>` 類型的實例，而不是根據程式碼是否使用 `yield` 來製造編譯器。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-256">Depending on your perspective, this has the benefit of making it very clear by the signature of the method whether `yield` is allowed and whether the method is actually meant to return instances of type `IAsyncEnumerable<T>` rather than the compiler manufacturing one based on whether the code uses `yield` or not.</span></span>  <span data-ttu-id="ccc6e-257">但它與同步反覆運算器不同，這不是必要的，也不能用來要求一個。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-257">But it is different from synchronous iterators, which don't and can't be made to require one.</span></span>  <span data-ttu-id="ccc6e-258">此外，有些開發人員不喜歡額外的語法。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-258">Plus some developers don't like the extra syntax.</span></span>  <span data-ttu-id="ccc6e-259">如果我們從頭開始設計，我們可能會將它設為必要，但在此情況下，保持非同步反覆運算器接近同步反覆運算器會有更多價值。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-259">If we were designing it from scratch, we'd probably make this required, but at this point there's much more value in keeping async iterators close to sync iterators.</span></span>

## <a name="linq"></a><span data-ttu-id="ccc6e-260">LINQ</span><span class="sxs-lookup"><span data-stu-id="ccc6e-260">LINQ</span></span>

<span data-ttu-id="ccc6e-261">在 `System.Linq.Enumerable` 類別上，有超過 ~ 200 的方法多載，所有工作都是以 `IEnumerable<T>`為依據;其中有些會接受 `IEnumerable<T>`，其中有些會產生 `IEnumerable<T>`，而許多都是這麼做。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-261">There are over ~200 overloads of methods on the `System.Linq.Enumerable` class, all of which work in terms of `IEnumerable<T>`; some of these accept `IEnumerable<T>`, some of them produce `IEnumerable<T>`, and many do both.</span></span>  <span data-ttu-id="ccc6e-262">新增 `IAsyncEnumerable<T>` 的 LINQ 支援可能需要為它複製所有這些多載，而不是另一個 ~ 200。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-262">Adding LINQ support for `IAsyncEnumerable<T>` would likely entail duplicating all of these overloads for it, for another ~200.</span></span>  <span data-ttu-id="ccc6e-263">而且，由於 `IAsyncEnumerator<T>` 可能會比在非同步世界中的獨立實體更常見，而不是在同步世界中 `IEnumerator<T>`，所以我們可能需要另一個與 `IAsyncEnumerator<T>`搭配使用的 ~ 200 多載。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-263">And since `IAsyncEnumerator<T>` is likely to be more common as a standalone entity in the asynchronous world than `IEnumerator<T>` is in the synchronous world, we could potentially need another ~200 overloads that work with `IAsyncEnumerator<T>`.</span></span>  <span data-ttu-id="ccc6e-264">此外，大量多載處理述詞（例如採用 `Func<T, bool>`的 `Where`），而且可能會想要以 `IAsyncEnumerable<T>`為基礎的多載同時處理同步和非同步述詞（例如，除了 `Func<T, bool>`以外的 `Func<T, ValueTask<bool>>`）。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-264">Plus, a large number of the overloads deal with predicates (e.g. `Where` that takes a `Func<T, bool>`), and it may be desirable to have `IAsyncEnumerable<T>`-based overloads that deal with both synchronous and asynchronous predicates (e.g. `Func<T, ValueTask<bool>>` in addition to `Func<T, bool>`).</span></span>  <span data-ttu-id="ccc6e-265">雖然這不適用於所有現在的 ~ 400 新多載，但粗略的計算是將它套用至一半，這表示另一個 ~ 200 多載，總計 ~ 600 個新方法。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-265">While this isn't applicable to all of the now ~400 new overloads, a rough calculation is that it'd be applicable to half, which means another ~200 overloads, for a total of ~600 new methods.</span></span>

<span data-ttu-id="ccc6e-266">這是一種驚人的 Api，在考慮像是互動式延伸模組（Ix）之類的延伸程式庫時，可能會有更多的功能。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-266">That is a staggering number of APIs, with the potential for even more when extension libraries like Interactive Extensions (Ix) are considered.</span></span>  <span data-ttu-id="ccc6e-267">但是，Ix 已經有許多這類的執行，而且似乎不是複製該工作的絕佳原因。我們應該改為協助社區改善 Ix，並在開發人員想要使用 LINQ 搭配 `IAsyncEnumerable<T>`時加以建議。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-267">But Ix already has an implementation of many of these, and there doesn't seem to be a great reason to duplicate that work; we should instead help the community improve Ix and recommend it for when developers want to use LINQ with `IAsyncEnumerable<T>`.</span></span>

<span data-ttu-id="ccc6e-268">此外，也有查詢理解語法的問題。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-268">There is also the issue of query comprehension syntax.</span></span>  <span data-ttu-id="ccc6e-269">查詢智力的模式式本質可讓它們「只」使用某些運算子，例如，如果 Ix 提供下列方法：</span><span class="sxs-lookup"><span data-stu-id="ccc6e-269">The pattern-based nature of query comprehensions would allow them to "just work" with some operators, e.g. if Ix provides the following methods:</span></span>

```csharp
public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, TResult> func);
public static IAsyncEnumerable<T> Where(this IAsyncEnumerable<T> source, Func<T, bool> func);
```

<span data-ttu-id="ccc6e-270">這C#段程式碼就會「工作」：</span><span class="sxs-lookup"><span data-stu-id="ccc6e-270">then this C# code will "just work":</span></span>

```csharp
IAsyncEnumerable<int> enumerable = ...;
IAsyncEnumerable<int> result = from item in enumerable
                               where item % 2 == 0
                               select item * 2;
```

<span data-ttu-id="ccc6e-271">不過，不支援在子句中使用 `await` 的查詢理解語法，因此如果加入 Ix，則為，例如：</span><span class="sxs-lookup"><span data-stu-id="ccc6e-271">However, there is no query comprehension syntax that supports using `await` in the clauses, so if Ix added, for example:</span></span>

```csharp
public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, ValueTask<TResult>> func);
```

<span data-ttu-id="ccc6e-272">如此一來，就可以「順利」執行：</span><span class="sxs-lookup"><span data-stu-id="ccc6e-272">then this would "just work":</span></span>

```csharp
IAsyncEnumerable<string> result = from url in urls
                                  where item % 2 == 0
                                  select SomeAsyncMethod(item);

async ValueTask<int> SomeAsyncMethod(int item)
{
    await Task.Yield();
    return item * 2;
}
```

<span data-ttu-id="ccc6e-273">但是沒有辦法使用 `select` 子句中的 `await` 內嵌來撰寫它。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-273">but there'd be no way to write it with the `await` inline in the `select` clause.</span></span>  <span data-ttu-id="ccc6e-274">另外，我們也可以將 `async { ... }` 運算式加入至語言，此時我們可以讓它們在查詢智力中使用，而上述專案可以改寫成：</span><span class="sxs-lookup"><span data-stu-id="ccc6e-274">As a separate effort, we could look into adding `async { ... }` expressions to the language, at which point we could allow them to be used in query comprehensions and the above could instead be written as:</span></span>

```csharp
IAsyncEnumerable<int> result = from item in enumerable
                               where item % 2 == 0
                               select async
                               {
                                   await Task.Yield();
                                   return item * 2;
                               };
```

<span data-ttu-id="ccc6e-275">或表示啟用 `await` 直接用於運算式，例如支援 `async from`。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-275">or to enabling `await` to be used directly in expressions, such as by supporting `async from`.</span></span>  <span data-ttu-id="ccc6e-276">不過，這裡的設計不太可能會影響另一種方法的其餘部分，而這不是特別高價值的東西，因此目前的建議是在此不提供任何額外的功能。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-276">However, it's unlikely a design here would impact the rest of the feature set one way or the other, and this isn't a particularly high-value thing to invest in right now, so the proposal is to do nothing additional here right now.</span></span>

## <a name="integration-with-other-asynchronous-frameworks"></a><span data-ttu-id="ccc6e-277">與其他非同步架構整合</span><span class="sxs-lookup"><span data-stu-id="ccc6e-277">Integration with other asynchronous frameworks</span></span>

<span data-ttu-id="ccc6e-278">與 `IObservable<T>` 和其他非同步架構（例如，被動串流）的整合會在程式庫層級執行，而不是在語言層級。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-278">Integration with `IObservable<T>` and other asynchronous frameworks (e.g. reactive streams) would be done at the library level rather than at the language level.</span></span>  <span data-ttu-id="ccc6e-279">例如，`IAsyncEnumerator<T>` 中的所有資料都可以發行至 `IObserver<T>`，只要 `await foreach`' 進行列舉值，並 `OnNext`將資料移至觀察者，因此可能會有 `AsObservable<T>` 的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-279">For example, all of the data from an `IAsyncEnumerator<T>` can be published to an `IObserver<T>` simply by `await foreach`'ing over the enumerator and `OnNext`'ing the data to the observer, so an `AsObservable<T>` extension method is possible.</span></span>  <span data-ttu-id="ccc6e-280">在 `await foreach` 中使用 `IObservable<T>` 需要緩衝處理資料（如果在前一個專案仍在處理時，另一個專案會推送），但這類推播提取介面卡可以輕易地實作為，讓 `IObservable<T>` 從 `IAsyncEnumerator<T>`提取。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-280">Consuming an `IObservable<T>` in a `await foreach` requires buffering the data (in case another item is pushed while the previous item is still being processing), but such a push-pull adapter can easily be implemented to enable an `IObservable<T>` to be pulled from with an `IAsyncEnumerator<T>`.</span></span>  <span data-ttu-id="ccc6e-281">等. Rx/Ix 已經提供這類實現的原型，而 https://github.com/dotnet/corefx/tree/master/src/System.Threading.Channels 的程式庫則提供各種緩衝處理資料結構。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-281">Etc.  Rx/Ix already provide prototypes of such implementations, and libraries like https://github.com/dotnet/corefx/tree/master/src/System.Threading.Channels provide various kinds of buffering data structures.</span></span>  <span data-ttu-id="ccc6e-282">此階段不需要涉及此語言。</span><span class="sxs-lookup"><span data-stu-id="ccc6e-282">The language need not be involved at this stage.</span></span>
