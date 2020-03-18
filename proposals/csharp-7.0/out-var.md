---
ms.openlocfilehash: a0d80afc47e9f0073237db9b8d7a4f0b045c1b0b
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484507"
---
# <a name="out-variable-declarations"></a>Out 變數宣告

*Out 變數*宣告功能可讓變數在當做 `out` 引數傳遞的位置進行宣告。

```antlr
argument_value
    : 'out' type identifier
    | ...
    ;
```

以這種方式宣告的變數稱為*out 變數*。 您可以將內容關鍵字 `var` 用於變數的類型。 範圍會與透過模式比對所引進的*模式變數*相同。

根據語言規格（區段7.6.7 元素存取），元素存取（索引運算式）的引數清單不包含 ref 或 out 引數。 不過，編譯器會針對各種案例（例如，在接受 `out`的中繼資料中宣告的索引子）允許它們。

在 argument_value 所引進的區域變數範圍內，在其宣告之前的文字位置中參考該區域變數是編譯時期錯誤。

在立即包含其宣告的同一個引數清單中，參考隱含類型（§8.5.1） out 變數也是錯誤。

多載解析的修改方式如下：

我們會加入新的轉換：

> *從運算式轉換*成每個型別都有一個從隱含型別 out 變數宣告到每種類型。

還要

> 明確類型 out 變數引數的類型是宣告的類型。

和

> 隱含類型的 out 變數引數沒有類型。

從運算式*轉換*成隱含類型的 out 變數宣告，並不會比*從運算式*進行任何其他轉換更好。

隱含類型 out 變數的類型是多載解析所選取之方法簽章中對應參數的類型。

新增了新的語法節點 `DeclarationExpressionSyntax`，以代表 out var 引數中的宣告。
