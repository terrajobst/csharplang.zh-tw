---
ms.openlocfilehash: 9ed1aa75d581cecbf754a84d1f523c6334b8c0ac
ms.sourcegitcommit: 5278336b61519956240a6f7d83bcb4322019e032
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/10/2020
ms.locfileid: "79485256"
---
# <a name="async-streams"></a>非同步資料流程

* [x] 提議
* [x] 原型
* [] 執行
* [] 規格

## <a name="summary"></a>摘要
[summary]: #summary

C#具有 iterator 方法和非同步方法的支援，但不支援同時為 iterator 和 async 方法的方法。  我們應該藉由允許將 `await` 用於新的 `async` 反覆運算器，其中一個會傳回 `IAsyncEnumerable<T>` 或 `IAsyncEnumerator<T>` 而不是 `IEnumerable<T>` 或 `IEnumerator<T>`，`IAsyncEnumerable<T>` 可在新的 `await foreach`中使用。  `IAsyncDisposable` 介面也用來啟用非同步清除。

## <a name="related-discussion"></a>相關討論
- https://github.com/dotnet/roslyn/issues/261
- https://github.com/dotnet/roslyn/issues/114

## <a name="detailed-design"></a>詳細設計
[design]: #detailed-design

## <a name="interfaces"></a>介面

### <a name="iasyncdisposable"></a>IAsyncDisposable

`IAsyncDisposable` 的討論（例如 https://github.com/dotnet/roslyn/issues/114)，以及是否是不錯的想法。  不過，這是加入非同步反覆運算器支援的必要概念。  由於 `finally` 區塊可能包含 `await`s，而且因為必須在處置反覆運算器的過程中執行 `finally` 區塊，所以我們需要非同步處置。  它也只會在清除資源的任何時間都很有用，例如關閉檔案（需要排清）、取消註冊回呼，並提供一種方法來得知何時完成解除註冊等等。

下列介面會新增至核心 .NET 程式庫（例如 CoreLib/System.webserver）：
```csharp
namespace System
{
    public interface IAsyncDisposable
    {
        ValueTask DisposeAsync();
    }
}
```
如同 `Dispose`，可以接受多次叫用 `DisposeAsync`，而第一個呼叫之後的後續調用則應該視為 nops，並傳回同步完成的成功工作（但`DisposeAsync` 不需要具備執行緒安全，但不需要支援並行調用）。  此外，型別可能會同時執行 `IDisposable` 和 `IAsyncDisposable`，如果有的話，就可以叫用 `Dispose` 然後 `DisposeAsync` 或相反的方式，但只有第一個是有意義的，而且後續的叫用都應該是 nop。  因此，如果型別確實執行這兩個，則取用者只會呼叫一次，而只有一次以內容為基礎的相關方法，`Dispose` 在同步內容中，並在非同步環境中 `DisposeAsync`。

（我會留下討論 `IAsyncDisposable` 如何與 `using` 互動，以進行個別討論。  以及如何與 `foreach` 互動的涵蓋範圍，會在本提案稍後進行處理）。

考慮的替代方案：
- _`DisposeAsync` 接受 `CancellationToken`_ ：在理論上，可以取消任何非同步作業，處置是關於清除、關閉 free'ing 資源等，這通常不是應該取消的專案;針對已取消的工作，清除仍然很重要。  導致實際取消工作的相同 `CancellationToken` 通常是傳遞給 `DisposeAsync`的相同權杖，因此 `DisposeAsync` 不萬能，因為取消工作會導致 `DisposeAsync` 成為 nop。  如果有人想要避免封鎖等候處置，他們可以避免等待產生的 `ValueTask`，或只等待一段時間。
- _`DisposeAsync` 傳回 `Task`_ ：現在，非泛型 `ValueTask` 存在，而且可以從 `IValueTaskSource`加以建立，從 `ValueTask` 傳回 `DisposeAsync` 可讓現有的物件重複使用，以表示 `DisposeAsync`的最終非同步完成，在 `Task` 非同步完成的情況下，儲存 `DisposeAsync` 配置。
- _使用 `bool continueOnCapturedContext` （`ConfigureAwait`）設定 `DisposeAsync`_ ：雖然可能會有一些問題與使用此概念的 `using`、`foreach`和其他語言結構公開，但從介面的觀點來看，它實際上不會執行任何 `await`的作業，而且沒有任何可設定的東西 .。。不過，`ValueTask` 的取用者可以視需要取用它。
- _`IAsyncDisposable` 繼承 `IDisposable`_ ：因為只應使用其中一個或另一個，所以強制型別同時執行這兩種方式並不合理。
- _`IDisposableAsync`，而不是 `IAsyncDisposable`_ ：我們已遵循將專案/類型命名為「非同步事物」，而作業是「完成非同步」的動作，因此，類型會以 "async" 做為前置詞，而方法會以 "async" 作為尾碼。

### <a name="iasyncenumerable--iasyncenumerator"></a>IAsyncEnumerable/IAsyncEnumerator

核心 .NET 程式庫中新增了兩個介面：

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

一般耗用量（不含其他語言功能）看起來會像這樣：

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

被視為的捨棄選項：
- _`Task<bool> MoveNextAsync(); T current { get; }`_ ：使用 `Task<bool>` 可支援使用快取的工作物件來代表同步、成功的 `MoveNextAsync` 呼叫，但非同步完成時仍然需要配置。  藉由傳回 `ValueTask<bool>`，我們可以讓列舉值物件本身實作為 `IValueTaskSource<bool>`，並做為 `MoveNextAsync`所傳回 `ValueTask<bool>` 的支援，進而大幅降低負荷。
- _`ValueTask<(bool, T)> MoveNextAsync();`_ ：使用並不難，但這表示 `T` 不會再是「協變數」。
- _`ValueTask<T?> TryMoveNextAsync();`_ ：不是協變數。
- _`Task<T?> TryMoveNextAsync();`_ ：不是協變數、每個呼叫上的配置等等。
- _`ITask<T?> TryMoveNextAsync();`_ ：不是協變數、每個呼叫上的配置等等。
- _`ITask<(bool,T)> TryMoveNextAsync();`_ ：不是協變數、每個呼叫上的配置等等。
- _`Task<bool> TryMoveNextAsync(out T result);`_ ：當作業以同步方式傳回時，必須設定 `out` 的結果，而不是以非同步方式完成工作，未來可能會有一段時間很久，此時就無法傳達結果。
- _`IAsyncEnumerator<T>` 不會執行 `IAsyncDisposable`_ ：我們可以選擇分隔這些。  不過，這麼做會使提議中的某些其他區域變得複雜，因為程式碼必須能夠處理列舉值無法提供處置的可能性，因此很難以撰寫以模式為基礎的協助程式。  此外，列舉值通常會有需要處置的需求（例如，具有 finally 區塊的C#任何非同步反覆運算器、從網路連接列舉資料的大部分專案等等），如果沒有的話，就可以簡單地將方法實作為 `public ValueTask DisposeAsync() => default(ValueTask);`，只需最少的額外負荷。
- _ `IAsyncEnumerator<T> GetAsyncEnumerator()`：沒有解除標記參數。

#### <a name="viable-alternative"></a>可行的替代方案：

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

在內部迴圈中使用 `TryGetNext`，只要它們可以同步使用，就能取用具有單一介面呼叫的專案。  當下一個專案無法以同步方式抓取時，它會傳回 false，而且每當傳回 false 時，呼叫端都必須叫用 `WaitForNextAsync`，等候下一個專案可以使用，或判斷永遠不會有另一個專案。 一般耗用量（不含其他語言功能）看起來會像這樣：

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

這是兩折的優點，一個次要和一個主要：
- _次要：允許枚舉器支援多個取用者_。 在某些情況下，列舉值可以支援多個並行取用者。  當 `MoveNextAsync` 和 `Current` 分開時，就無法達成此目的，因為實現無法使其使用不可部分完成。  相反地，這個方法會提供單一方法 `TryGetNext`，支援將列舉值向前推送並取得下一個專案，因此枚舉器可以在需要時啟用不可部分完成性。  不過，這種情況可能也會藉由提供每個取用者自己的來自共用可列舉枚舉器來啟用。  此外，我們不想要強制每個列舉值都支援並行使用，因為這會對不需要它的多數案例新增非一般的負荷，這表示介面的取用者通常無法以這種方式依賴這種情況。
- _主要：效能_。 `MoveNextAsync`/`Current` 的方法，每個作業都需要兩個介面呼叫，而 `WaitForNextAsync`/`TryGetNext` 的最佳案例是，大部分的反復專案都是以同步方式完成，以 `TryGetNext`啟用緊密的內部迴圈，因此每個作業只有一個介面呼叫。  當介面呼叫佔據運算的情況時，這可能會有顯著的影響。

不過，有一些非一般的缺點，包括以手動方式使用這些專案時的複雜性大幅增加，而且在使用 bug 時，可能會增加錯誤的機率。  雖然效能優勢會出現在 microbenchmarks 中，但我們並不相信它們會在大部分的實際使用方式下影響力。  如果結果是，我們可以用淺的方式引進第二組介面。

被視為的捨棄選項：
- `ValueTask<bool> WaitForNextAsync(); bool TryGetNext(out T result);`： `out` 參數不可以是不可變的。  這裡也有些許影響（一般 try 模式的問題），這可能會產生參考型別結果的執行時間寫入屏障。

#### <a name="cancellation"></a>取消

有數種可能的方法可支援取消：
1. `IAsyncEnumerable<T>`/`IAsyncEnumerator<T>` 不相容： `CancellationToken` 不會出現在任何位置。  取消的達成方式是以邏輯方式，將 `CancellationToken` 烘焙到可列舉的和（或）枚舉器中，例如呼叫反覆運算器、將 `CancellationToken` 當做引數傳遞至 iterator 方法，並在反覆運算器主體中使用它，就像使用任何其他參數一樣。
2. `IAsyncEnumerator<T>.GetAsyncEnumerator(CancellationToken)`：您會將 `CancellationToken` 傳遞給 `GetAsyncEnumerator`，而後續的 `MoveNextAsync` 作業則會遵循此動作。
3. `IAsyncEnumerator<T>.MoveNextAsync(CancellationToken)`：您會將 `CancellationToken` 傳遞給每個個別的 `MoveNextAsync` 呼叫。
4. 1 & & 2：您會在可列舉/枚舉器中內嵌 `CancellationToken`，並將 `CancellationToken`傳遞至 `GetAsyncEnumerator`。
5. 1 & & 3：您可以在可列舉/枚舉器中內嵌 `CancellationToken`，並將 `CancellationToken`傳遞至 `MoveNextAsync`。

從純粹理論的觀點來看，（5）是最穩固的，在此（a）中，`MoveNextAsync` 接受 `CancellationToken` 可讓您對取消的內容進行最精細的控制，而（b） `CancellationToken` 只是可當做引數傳遞至反覆運算器、內嵌在任意類型等中的任何其他類型。

不過，這種方法有多個問題：
- 如何將 `CancellationToken` 傳遞至 `GetAsyncEnumerator` 讓它進入 iterator 的主體？  我們可以公開新的 `iterator` 關鍵字，讓您可以從點開始，以存取傳遞給 `GetEnumerator`的 `CancellationToken`。但是，這是很多額外的機械，b）我們會使其成為非常頂級的公民，c）99% 的案例似乎是相同的程式碼，兩者都呼叫 iterator 並在其上呼叫 `GetAsyncEnumerator`，在此情況下，它只會將 `CancellationToken` 做為引數傳遞至方法。
- 如何將 `CancellationToken` 傳遞至 `MoveNextAsync` 進入方法的主體？  這種情況更糟，因為它是公開在 `iterator` 的本機物件之外，所以它的值可能會在等候期間變更，這表示任何已註冊權杖的程式碼都必須在等候之前取消註冊，然後再于之後重新登錄;無論是在每個 `MoveNextAsync` 呼叫中進行註冊和取消註冊，都可能需要耗費大量資源，不論編譯器是由反覆運算器或開發人員手動執行。
- 開發人員如何取消 `foreach` 迴圈？  如果它是藉由提供 `CancellationToken` 給可列舉/枚舉器，則是），我們需要支援對列舉值執行 `foreach`'。這會將它們提升為第一方公民，而現在您需要開始思考在列舉值（例如 LINQ 方法）或 b）上所建立的生態系統，我們需要在可儲存所提供權杖的 `IAsyncEnumerable<T>` 以外的部分 `WithCancellation` 擴充方法，然後將它傳遞到包裝的可列舉的 `GetAsyncEnumerator` 中，然後在傳回的結構上 `GetAsyncEnumerator` 時，將 `CancellationToken` 內嵌會叫用（忽略該 token）。  或者，您可以只使用 foreach 主體中的 `CancellationToken`。
- 如果支援查詢智力，則如何將 `CancellationToken` 提供給 `GetEnumerator` 或 `MoveNextAsync` 傳遞至每個子句？  最簡單的方式就是讓子句來加以捕捉，而將任何權杖傳遞至 `GetAsyncEnumerator`/`MoveNextAsync` 會被忽略。

建議使用此檔的較早版本（1），但我們已切換至（4）。

（1）的兩個主要問題：
- 可取消可列舉值的產生者必須實行一些樣板化，而且只能利用編譯器的非同步反覆運算器支援來執行 `IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken)` 方法。
- 許多產生者可能會想要改為將 `CancellationToken` 參數新增至其非同步可列舉簽章，這會防止取用者在提供 `IAsyncEnumerable` 類型時，傳遞所需的取消權杖。

主要的耗用量案例有兩種：
1. `await foreach (var i in GetData(token)) ...` 取用者呼叫非同步反覆運算器方法的位置，
2. `await foreach (var i in givenIAsyncEnumerable.WithCancellation(token)) ...` 取用者處理給定 `IAsyncEnumerable` 實例的位置。

我們發現有一項合理的危害可支援這兩種案例，這種方式對非同步資料流程的生產者和取用者而言都很方便，就是在非同步反覆運算器方法中使用特別標注的參數。 `[EnumeratorCancellation]` 屬性是用於此用途。 將這個屬性放在參數上會告訴編譯器，如果將權杖傳遞給 `GetAsyncEnumerator` 方法，就應該使用該 token，而不是原本針對參數傳遞的值。

請考慮 `IAsyncEnumerable<int> GetData([EnumeratorCancellation] CancellationToken token = default)`。 這個方法的實施者可以直接在方法主體中使用參數。 取用者可以使用上述任一耗用量模式：
1. 如果您使用 `GetData(token)`，則權杖會儲存至非同步可列舉，並會在反復專案中使用。
2. 如果您使用 `givenIAsyncEnumerable.WithCancellation(token)`，傳遞至 `GetAsyncEnumerator` 的權杖將會取代任何儲存在非同步可列舉中的 token。

## <a name="foreach"></a>foreach

除了其現有的 `IEnumerable<T>`支援之外，也會增強 `foreach` 以支援 `IAsyncEnumerable<T>`。  而且，如果相關的成員公開公開，又回到直接使用介面（如果沒有的話），則會支援對等的 `IAsyncEnumerable<T>` 做為模式，以啟用以結構為基礎的延伸模組，以避免配置及使用替代的 awaitables 做為 `MoveNextAsync` 和 `DisposeAsync`的傳回型別。

### <a name="syntax"></a>語法

使用語法：

```csharp
foreach (var i in enumerable)
```

C#會繼續將 `enumerable` 視為同步可列舉，即使它公開非同步可列舉值的相關 Api （公開模式或執行介面），它還是只會考慮同步 Api。

若要強制 `foreach` 改為只考慮非同步 Api，`await` 插入如下：

```csharp
await foreach (var i in enumerable)
```

不會提供支援使用 async 或 sync Api 的語法;開發人員必須根據所使用的語法進行選擇。

被視為的捨棄選項：
- _`foreach (var i in await enumerable)`_ ：這已經是有效的語法，而變更其意義會是中斷性變更。  這表示 `await` `enumerable`，請從其同步可反復執行，然後以同步方式逐一查看該問題。
- _`foreach (var i await in enumerable)`、`foreach (var await i in enumerable)``foreach (await var i in enumerable)`_ ：這些全都建議我們等待下一個專案，但有其他與 foreach 相關的等候，特別是當可列舉為 `IAsyncDisposable`時，我們將會 `await`' 進行非同步處置。  該 await 會當做 foreach 的範圍，而不是每個個別的元素，因此 `await` 關鍵字應該是 `foreach` 層級。  此外，讓它與 `foreach` 相關聯，讓我們能夠以不同的詞彙來描述 `foreach`，例如「await foreach」。  但更重要的是，考慮 `foreach` 語法與 `using` 語法同時保持一致，因此 `using (await ...)` 已經是有效的語法。
- _`foreach await (var i in enumerable)`_

還是要考慮：
- `foreach` 今天不支援逐一查看列舉值。  我們預期會有更常見的 `IAsyncEnumerator<T>`，因此同時支援 `IAsyncEnumerable<T>` 和 `IAsyncEnumerator<T>`的 `await foreach`。  但是在我們新增這類支援之後，它會介紹 `IAsyncEnumerator<T>` 是否為第一方公民的問題，以及是否除了可列舉值以外，是否還需要在列舉值上運作的組合器多載？    我們想要鼓勵方法傳回枚舉器，而不是可列舉值嗎？ 我們應該繼續討論這一點。  如果我們決定不想要支援它，我們可能會想要引進擴充方法 `public static IAsyncEnumerable<T> AsEnumerable<T>(this IAsyncEnumerator<T> enumerator);`，讓列舉值仍然 `foreach`。  如果我們決定要支援它，我們也必須決定 `await foreach` 是否會負責呼叫列舉值的 `DisposeAsync`，而答案可能是「否，控制處置的人應該要由呼叫 `GetEnumerator`的人員處理。」

### <a name="pattern-based-compilation"></a>以模式為基礎的編譯

編譯器會系結至以模式為基礎的 Api （如果有的話），並偏好使用介面（此模式可能符合實例方法或擴充方法）。  模式的需求如下：
- 可列舉必須公開不含引數呼叫的 `GetAsyncEnumerator` 方法，並傳回符合相關模式的列舉值。
- 列舉值必須公開不使用引數呼叫的 `MoveNextAsync` 方法，並傳回可能是 `await`ed 且其 `GetResult()` 傳回 `bool`的專案。
- 列舉值也必須公開 `Current` 屬性，其 getter 會傳回代表所列舉資料類型的 `T`。
- 列舉值可能會選擇性地公開不使用引數叫用的 `DisposeAsync` 方法，並傳回可以 `await`ed 且其 `GetResult()` 會傳回 `void`的內容。

此程式碼：

```csharp
var enumerable = ...;
await foreach (T item in enumerable)
{
   ...
}
```

會轉譯為對等的：

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

如果反復查看的類型未公開正確的模式，則會使用介面。

### <a name="configureawait"></a>ConfigureAwait

這個以模式為基礎的編譯可讓您透過 `ConfigureAwait` 擴充方法，在所有等候上使用 `ConfigureAwait`：

```csharp
await foreach (T item in enumerable.ConfigureAwait(false))
{
   ...
}
```

這會以我們也會新增至 .NET 的類型為基礎，可能會加入至 [System.web]：

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

請注意，這種方法並不會讓 `ConfigureAwait` 與以模式為基礎的可列舉值搭配使用。但再次強調，`ConfigureAwait` 只會公開為 `Task`/`Task<T>`/`ValueTask`/`ValueTask<T>` 的延伸模組，而且不能套用至任意可等候的專案，因為它只適用于套用至工作的情況（它會控制工作的接續支援中所實的行為），因此在使用模式時，可等候專案可能不會有任何意義。  傳回可等候專案的任何人都可以在這類 advanced 案例中提供自己的自訂行為。

（如果我們可以提供一些方法來支援範圍或元件層級的 `ConfigureAwait` 解決方案，那麼就不需要這麼做）。

## <a name="async-iterators"></a>非同步反覆運算器

除了使用語言/編譯器以外，它還支援產生 `IAsyncEnumerable<T>`s 和 `IAsyncEnumerator<T>`。 現在語言支援撰寫反覆運算器，如下所示：

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

但是 `await` 不能用在這些反覆運算器的主體中。  我們會新增該支援。

### <a name="syntax"></a>語法

反覆運算器的現有語言支援會根據它是否包含任何 `yield`來推斷方法的 iterator 本質。  非同步反覆運算器的情況也是如此。  這類非同步反覆運算器將會 config.js 區分，並藉由將 `async` 加入簽章來與同步反覆運算器區別，而且也必須 `IAsyncEnumerable<T>` 或 `IAsyncEnumerator<T>` 做為其傳回型別。  例如，上述範例可以撰寫為非同步反覆運算器，如下所示：

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

考慮的替代方案：
- _不使用簽章中的 `async`_ ：編譯器在技術上可能需要使用 `async`，因為它會使用它來判斷 `await` 在該內容中是否有效。  但是，即使不是必要的，我們也建立了 `await` 只能用在標示為 `async`的方法中，而且保持一致性似乎很重要。
- _啟用 `IAsyncEnumerable<T>`的自訂_產生器：這是我們在未來可以查看的內容，但此機制很複雜，而且不支援同步對應的。
- 在簽章_中具有 `iterator` 關鍵字_：非同步反覆運算器會在簽章中使用 `async iterator`，而 `yield` 只能用在包含 `iterator`的 `async` 方法中;然後會在同步反覆運算器上將 `iterator` 設為選擇性。  根據您的觀點，這有其優點，就是讓方法的簽章非常清楚，不論是否允許 `yield`，以及方法是否實際上是否要傳回 `IAsyncEnumerable<T>` 類型的實例，而不是根據程式碼是否使用 `yield` 來製造編譯器。  但它與同步反覆運算器不同，這不是必要的，也不能用來要求一個。  此外，有些開發人員不喜歡額外的語法。  如果我們從頭開始設計，我們可能會將它設為必要，但在此情況下，保持非同步反覆運算器接近同步反覆運算器會有更多價值。

## <a name="linq"></a>LINQ

在 `System.Linq.Enumerable` 類別上，有超過 ~ 200 的方法多載，所有工作都是以 `IEnumerable<T>`為依據;其中有些會接受 `IEnumerable<T>`，其中有些會產生 `IEnumerable<T>`，而許多都是這麼做。  新增 `IAsyncEnumerable<T>` 的 LINQ 支援可能需要為它複製所有這些多載，而不是另一個 ~ 200。  而且，由於 `IAsyncEnumerator<T>` 可能會比在非同步世界中的獨立實體更常見，而不是在同步世界中 `IEnumerator<T>`，所以我們可能需要另一個與 `IAsyncEnumerator<T>`搭配使用的 ~ 200 多載。  此外，大量多載處理述詞（例如採用 `Func<T, bool>`的 `Where`），而且可能會想要以 `IAsyncEnumerable<T>`為基礎的多載同時處理同步和非同步述詞（例如，除了 `Func<T, bool>`以外的 `Func<T, ValueTask<bool>>`）。  雖然這不適用於所有現在的 ~ 400 新多載，但粗略的計算是將它套用至一半，這表示另一個 ~ 200 多載，總計 ~ 600 個新方法。

這是一種驚人的 Api，在考慮像是互動式延伸模組（Ix）之類的延伸程式庫時，可能會有更多的功能。  但是，Ix 已經有許多這類的執行，而且似乎不是複製該工作的絕佳原因。我們應該改為協助社區改善 Ix，並在開發人員想要使用 LINQ 搭配 `IAsyncEnumerable<T>`時加以建議。

此外，也有查詢理解語法的問題。  查詢智力的模式式本質可讓它們「只」使用某些運算子，例如，如果 Ix 提供下列方法：

```csharp
public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, TResult> func);
public static IAsyncEnumerable<T> Where(this IAsyncEnumerable<T> source, Func<T, bool> func);
```

這C#段程式碼就會「工作」：

```csharp
IAsyncEnumerable<int> enumerable = ...;
IAsyncEnumerable<int> result = from item in enumerable
                               where item % 2 == 0
                               select item * 2;
```

不過，不支援在子句中使用 `await` 的查詢理解語法，因此如果加入 Ix，則為，例如：

```csharp
public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, ValueTask<TResult>> func);
```

如此一來，就可以「順利」執行：

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

但是沒有辦法使用 `select` 子句中的 `await` 內嵌來撰寫它。  另外，我們也可以將 `async { ... }` 運算式加入至語言，此時我們可以讓它們在查詢智力中使用，而上述專案可以改寫成：

```csharp
IAsyncEnumerable<int> result = from item in enumerable
                               where item % 2 == 0
                               select async
                               {
                                   await Task.Yield();
                                   return item * 2;
                               };
```

或表示啟用 `await` 直接用於運算式，例如支援 `async from`。  不過，這裡的設計不太可能會影響另一種方法的其餘部分，而這不是特別高價值的東西，因此目前的建議是在此不提供任何額外的功能。

## <a name="integration-with-other-asynchronous-frameworks"></a>與其他非同步架構整合

與 `IObservable<T>` 和其他非同步架構（例如，被動串流）的整合會在程式庫層級執行，而不是在語言層級。  例如，`IAsyncEnumerator<T>` 中的所有資料都可以發行至 `IObserver<T>`，只要 `await foreach`' 進行列舉值，並 `OnNext`將資料移至觀察者，因此可能會有 `AsObservable<T>` 的擴充方法。  在 `await foreach` 中使用 `IObservable<T>` 需要緩衝處理資料（如果在前一個專案仍在處理時，另一個專案會推送），但這類推播提取介面卡可以輕易地實作為，讓 `IObservable<T>` 從 `IAsyncEnumerator<T>`提取。  等. Rx/Ix 已經提供這類實現的原型，而 https://github.com/dotnet/corefx/tree/master/src/System.Threading.Channels 的程式庫則提供各種緩衝處理資料結構。  此階段不需要涉及此語言。
