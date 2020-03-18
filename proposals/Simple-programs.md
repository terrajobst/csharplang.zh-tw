---
ms.openlocfilehash: b9697fc1d772ba59ed3b1de339a5a3d4eb24b1bd
ms.sourcegitcommit: 36b028f4d6e88bd7d4a843c6d384d1b63cc73334
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/19/2020
ms.locfileid: "79485221"
---
# <a name="simple-programs"></a>簡單的程式

* [x] 提議
* [x] 原型：已啟動
* [] 執行：未啟動
* [] 規格：未啟動

## <a name="summary"></a>摘要
[summary]: #summary

允許在*compilation_unit* （也就是來源檔案）的*namespace_member_declaration*s 之前，執行一系列的*語句*。

其語義是，如果有這類的*語句*，就會發出下列類型宣告，並將實際的類型名稱和方法名稱模數。

``` c#
static class Program
{
    static async Task Main()
    {
        // statements
    }
}
```

另請參閱 https://github.com/dotnet/csharplang/issues/3117。

## <a name="motivation"></a>動機
[motivation]: #motivation

即使是最簡單的程式，也有一定數量的重複使用方式，因為需要明確的 `Main` 方法。 這似乎可以讓語言學習和程式清楚明瞭。 因此，此功能的主要目標是要讓C#程式不需要不必要的反復專案，以方便學習和撰寫程式碼。

## <a name="detailed-design"></a>詳細設計
[design]: #detailed-design

### <a name="syntax"></a>語法

唯一的另一個語法是在編譯單位中允許一連串的*語句*，就在*namespace_member_declaration*s 之前：

``` antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? statement* namespace_member_declaration*
    ;
```

除了一個*compilation_unit* *語句*s 都必須是區域函式宣告。 

範例：

``` c#
// File 1 - any statements
if (args.Length == 0
    || !int.TryParse(args[0], out int n)
    || n < 0) return;
Console.WriteLine(Fib(n).curr);

// File 2 - only local functions
(int curr, int prev) Fib(int i)
{
    if (i == 0) return (1, 0);
    var (curr, prev) = Fib(i - 1);
    return (curr + prev, curr);
}
```

### <a name="semantics"></a>語意

如果程式的任何編譯單位中有任何最上層語句，其意義就如同它們已結合在全域命名空間中 `Program` 類別的 `Main` 方法的區塊主體中，如下所示：

``` c#
static class Program
{
    static async Task Main()
    {
        // File 1 statements
        // File 2 local functions
        // ...
    }
}
```

請注意，名稱「程式」和「主要」僅供說明之用，編譯器所使用的實際名稱是相依的，而且型別和方法都不能由原始程式碼參考。

方法會指定為程式的進入點。 根據慣例，明確宣告的方法可視為會忽略進入點候選項目。 當發生這種情況時，就會回報警告。 指定 `-main:<type>` 編譯器參數是錯誤的。

如果任何一個編譯單位具有區域函式宣告以外的語句，則會先發生來自該編譯單位的語句。 這會使某個檔案中的區域函式合法，以參考另一個檔案中的區域變數。 來自其他編譯單位的語句貢獻（全部都是區域函式）順序未定義。

在一般非同步進入點方法內的語句中，最上層語句允許非同步作業。 不過，它們不是必要的，如果省略 `await` 運算式和其他非同步作業，就不會產生任何警告。 相反地，產生的進入點方法的簽章相當於 
``` c#
    static void Main()
```

上述範例會產生下列 `$Main` 方法宣告：

``` c#
static class $Program
{
    static void $Main()
    {
        // Statements from File 1
        if (args.Length == 0
            || !int.TryParse(args[0], out int n)
            || n < 0) return;
        Console.WriteLine(Fib(n).curr);
        
        // Local functions from File 2
        (int curr, int prev) Fib(int i)
        {
            if (i == 0) return (1, 0);
            var (curr, prev) = Fib(i - 1);
            return (curr + prev, curr);
        }
    }
}
```

在此同時，範例如下所示：
``` c#
// File 1
await System.Threading.Tasks.Task.Delay(1000);
System.Console.WriteLine("Hi!");
```

會產生：
``` c#
static class $Program
{
    static async Task $Main()
    {
        // Statements from File 1
        await System.Threading.Tasks.Task.Delay(1000);
        System.Console.WriteLine("Hi!");
    }
}
```

### <a name="scope-of-top-level-local-variables-and-local-functions"></a>最上層區域變數和區域函式的範圍

即使最上層區域變數和函式已「包裝」到產生的進入點方法中，這些變數和函式仍應在整個程式的範圍內。
基於簡單名稱評估的目的，一旦達到全域命名空間：
- 首先，嘗試評估產生的進入點方法中的名稱，而且只有在此嘗試失敗時才會進行 
- 全域命名空間宣告內的 "regular" 評估會執行。 

這可能會導致命名空間的名稱和在全域命名空間內宣告的類型，以及匯入的名稱的遮蔽。

如果簡單名稱評估是在最上層語句之外進行，而評估產生最上層的區域變數或函式，則會導致錯誤。

如此一來，我們就可以保護未來更能解決「最上層功能」（ https://github.com/dotnet/csharplang/issues/3117)案例2的能力，而且能夠為不小心認為受到支援的使用者提供有用的診斷。

