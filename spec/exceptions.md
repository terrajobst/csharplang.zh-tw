---
ms.openlocfilehash: 75fcd5b00ea5cac218a9f7809c53b179df97825c
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488943"
---
# <a name="exceptions"></a>例外狀況

在 C# 中的例外狀況會提供結構化、 統一和型別安全的方式來處理系統層級和應用程式層級錯誤狀況。 中的例外狀況機制C#是相當類似於C++，但有一些重要差異：

*  在 C# 中，所有例外狀況必須衍生自的類別類型的執行個體所表示`System.Exception`。 在C++，任何型別的任何值可用來表示例外狀況。
*  在 C# 中，finally 區塊 ([try 陳述式](statements.md#the-try-statement)) 可用來撰寫一般執行和例外狀況中執行的終止程式碼。 這類程式碼很難撰寫C++不含重複的程式碼。
*  在 C# 中，系統層級例外狀況例如溢位、 除數為零和 null 取值也定義了例外狀況類別和應用程式層級錯誤狀況與同等。

## <a name="causes-of-exceptions"></a>例外狀況的原因

兩個不同的方式，可以擲回例外狀況。

*  A`throw`陳述式 ([throw 陳述式](statements.md#the-throw-statement)) 立即且無條件地擲回例外狀況。 控制永遠不會到達緊接`throw`。
*  某些 C# 陳述式和運算式的處理期間所發生的例外狀況會導致在某些情況下發生例外狀況時無法正常完成的作業。 例如，整數除法運算 ([除法運算子](expressions.md#division-operator)) 會擲回`System.DivideByZeroException`如果分母為零。 請參閱[常見的例外狀況類別](exceptions.md#common-exception-classes)取得一份可能會發生這種方式的各種例外狀況。

## <a name="the-systemexception-class"></a>System.Exception 類別

`System.Exception`類別是所有例外狀況的基底類型。 這個類別會有共用的所有例外狀況的幾個重要屬性：

*  `Message` 是唯讀的屬性型別的`string`，包含人類看得懂的例外狀況的原因描述。
*  `InnerException` 是唯讀的屬性型別的`Exception`。 如果其值為非 null，它是指造成目前例外狀況的例外狀況，也就是在 catch 區塊中引發目前的例外狀況處理`InnerException`。 否則，其值為 null，表示這個例外狀況不是由另一個例外狀況所造成的。 以這種方式鏈結在一起的例外狀況物件的數目可以是任意的。

這些屬性的值可以指定的執行個體建構函式的呼叫中`System.Exception`。

## <a name="how-exceptions-are-handled"></a>如何處理例外狀況

例外狀況會由`try`陳述式 ([try 陳述式](statements.md#the-try-statement))。

發生例外狀況時，系統會搜尋最接近`catch`可以處理例外狀況，例外狀況的執行階段型別所決定的子句。 首先，目前的方法搜尋語彙封閉式`try`陳述式和相關聯的 catch 子句的 try 陳述式會被視為順序。 如果失敗，請呼叫目前方法的方法會搜尋語彙封閉式`try`目前方法的呼叫點封入陳述式。 此搜尋會繼續直到`catch`子句找到可以處理目前的例外狀況，藉由命名為相同的類別或基底類別之型別的執行階段擲回例外狀況的例外狀況類別。 A`catch`子句未命名的例外狀況類別可以處理任何例外狀況。

一旦找到相符的 catch 子句，系統會將控制權轉移到 catch 子句的第一個陳述式準備。 Catch 子句執行開始之前，系統會先執行，依序任何`finally`子句所需的 try 陳述式相關聯的巢狀，比攔截到例外狀況。

如果不找到任何相符的 catch 子句，就會發生下列其中一種：

*  如果搜尋相符的 catch 子句已達到靜態建構函式 ([靜態建構函式](classes.md#static-constructors)) 或靜態欄位初始設定式，則會顯示`System.TypeInitializationException`觸發靜態建構函式的引動過程之處擲回。 內部例外狀況的`System.TypeInitializationException`包含原先擲回的例外狀況。
*  如果比對 catch 子句的搜尋已達到執行緒的初始啟動的程式碼，然後會終止執行緒的執行。 這類終止的影響是由實作定義。

解構函式執行期間所發生的例外狀況是值得特別一提。 如果解構函式執行期間發生的例外狀況，而且未攔截到例外狀況，然後該解構函式的執行已終止，並 （如果有的話） 的基底類別的解構函式呼叫。 如果沒有基底類別 (如果是做為`object`型別) 或如果沒有基底類別解構函式，則會捨棄例外狀況。

## <a name="common-exception-classes"></a>常見的例外狀況類別

某些 C# 作業會擲回下列例外狀況。

|                                      |                |
|--------------------------------------|----------------|
| `System.ArithmeticException`         | 在算術運算期間所發生的例外狀況 (例如 `System.DivideByZeroException` 和 `System.OverflowException`) 的基底類別。 | 
| `System.ArrayTypeMismatchException`  | 陣列的存放區失敗，因為實際的預存的項目類型是與陣列的實際型別不相容時，便會擲回。 | 
| `System.DivideByZeroException`       | 當發生除以零的整數值的嘗試時，便會擲回。 | 
| `System.IndexOutOfRangeException`    | 當嘗試透過索引小於零或超出陣列界限的陣列編製索引時，便會擲回。 | 
| `System.InvalidCastException`        | 基底類型或介面從衍生類型的明確轉換在執行階段失敗時擲回。 | 
| `System.NullReferenceException`      | 時擲回`null`參考會在讓需要參考的物件。 | 
| `System.OutOfMemoryException`        | 當嘗試配置記憶體時擲回 (透過`new`) 就會失敗。 | 
| `System.OverflowException`           | `checked` 內容中的算術運算溢位時擲回。 | 
| `System.StackOverflowException`      | 當有太多暫止方法呼叫; 耗盡執行堆疊時擲回通常是用來指示非常深或無限遞迴。 | 
| `System.TypeInitializationException` | 靜態建構函式會擲回例外狀況，但不含任何時擲回`catch`子句存在攔截它。 | 
