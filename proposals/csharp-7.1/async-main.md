---
ms.openlocfilehash: 405153448d0e3685d6f22725e00d75d9250b3e20
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484794"
---
# <a name="async-main"></a>非同步 Main

* [x] 提議
* [] 原型
* [] 執行
* [] 規格

## <a name="summary"></a>摘要
[summary]: #summary

允許在應用程式的主要/entrypoint 方法中使用 `await`，方法是允許 entrypoint 傳回 `Task` / `Task<int>`，並將其標示為 `async`。

## <a name="motivation"></a>動機
[motivation]: #motivation

當學習C#、撰寫以主控台為基礎的公用程式時，以及撰寫小型測試應用程式時，想要從 Main 呼叫和 `await` `async` 方法時，這是很常見的情況。  我們今天在這裡增加了複雜性層級，方法是強制在不同的非同步方法中執行這類 `await`，讓開發人員只需要撰寫如下所示的建議，就可以開始：

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

我們可以移除這項建議的需求，並讓主要本身得以 `async`，以便在其中使用 `await`。

## <a name="detailed-design"></a>詳細設計
[design]: #detailed-design

下列簽章目前允許 e：

```csharp
static void Main()
static void Main(string[])
static int Main()
static int Main(string[])
```

我們會擴充允許的 e 清單，使其包含：

```csharp
static Task Main()
static Task<int> Main()
static Task Main(string[])
static Task<int> Main(string[])
```

為了避免相容性風險，如果沒有先前集合的多載存在，這些新的簽章只會被視為有效的 e。
語言/編譯器不會要求將進入點標記為 `async`，但我們預期大部分的使用都會標示為。

當其中一個識別為進入點時，編譯器會合成實際的 entrypoint 方法，以呼叫下列其中一個程式碼方法：
- ```static Task Main()``` 會導致編譯器發出對等的 ```private static void $GeneratedMain() => Main().GetAwaiter().GetResult();```
- ```static Task Main(string[])``` 會導致編譯器發出對等的 ```private static void $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```
- ```static Task<int> Main()``` 會導致編譯器發出對等的 ```private static int $GeneratedMain() => Main().GetAwaiter().GetResult();```
- ```static Task<int> Main(string[])``` 會導致編譯器發出對等的 ```private static int $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```

使用方式範例：

```csharp
using System;
using System.Net.Http;

class Test
{
    static async Task Main(string[] args) =>
        Console.WriteLine(await new HttpClient().GetStringAsync(args[0]));
}
```

## <a name="drawbacks"></a>缺點
[drawbacks]: #drawbacks

主要缺點只是支援額外的進入點簽章的額外複雜性。

## <a name="alternatives"></a>替代方案
[alternatives]: #alternatives

其他考慮的變數：

允許 `async void`。  我們需要針對直接呼叫它的程式碼保持相同的語法，這樣會使產生的進入點難以呼叫它（不會傳回工作）。  我們可以藉由產生兩個其他方法來解決此問題，例如

```csharp
public static async void Main()
{
   ... // await code
}
```

變成

```csharp
public static async void Main() => await $MainTask();

private static void $EntrypointMain() => Main().GetAwaiter().GetResult();

private static async Task $MainTask()
{
    ... // await code
}
```

同時也有關于鼓勵使用 `async void`的顧慮。

使用 "MainAsync" 而非 "Main" 作為名稱。  雖然建議使用非同步尾碼來進行工作傳回的方法，但這主要是關於程式庫功能（主要不是），而且支援超出 "Main" 的其他 entrypoint 名稱，並不值得這麼做。

## <a name="unresolved-questions"></a>未解決的問題
[unresolved]: #unresolved-questions

n/a

## <a name="design-meetings"></a>設計會議

n/a
