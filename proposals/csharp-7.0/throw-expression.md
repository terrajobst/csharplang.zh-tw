---
ms.openlocfilehash: 2532a24e867930d2f27614f19c77585dbce80dfa
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484542"
---
# <a name="throw-expression"></a>Throw 運算式

我們會擴充要包含的運算式表單集合

```antlr
throw_expression
    : 'throw' null_coalescing_expression
    ;

null_coalescing_expression
    : throw_expression
    ;
```

類型規則如下所示：

- *Throw_expression*沒有類型。
- *Throw_expression*可以透過隱含轉換，轉換成每一種類型。

*Throw 運算式會擲*回評估*null_coalescing_expression*所產生的值，這必須代表衍生自 `System.Exception` 的類別型別值 `System.Exception`，或是具有 `System.Exception` （或其子類別）做為其有效基類的型別參數型別。 如果運算式的評估產生 `null`，會改為擲回 `System.NullReferenceException`。

在執行時間評估*throw 運算式*的行為與[針對*throw 語句*所指定](../../spec/statements.md#the-throw-statement)的行為相同。

流程分析規則如下所示：

- 針對每個*變數 v*，在*throw_expression* iff 之前，會*null_coalescing_expression*先明確指派*v* ，而不是在*throw_expression*之前明確指派。
- 針對每個變數*v*，會在*throw_expression*之後明確指派*v* 。

只有下列語法內容允許*throw 運算式*：
- 做為三元條件運算子的第二個或第三個運算元 `?:`
- 當做 null 聯合運算子的第二個運算元 `??`
- 做為運算式主體 lambda 或方法的主體。
