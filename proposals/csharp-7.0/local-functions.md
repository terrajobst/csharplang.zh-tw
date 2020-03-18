---
ms.openlocfilehash: a4b0fbbc600eaf1e705ad8e6bd9fcecb44100761
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484437"
---
# <a name="local-functions"></a>區域函式

我們擴充C#以支援區塊範圍中的函式宣告。 區域函式可以使用（capture）自封閉範圍中的變數。

編譯器會使用流程分析來偵測區域函式在指派值之前所使用的變數。 函式的每個呼叫都需要明確指派這類變數。 同樣地，編譯器會決定要在傳回時明確指派的變數。 叫用區域函數之後，這類變數會被視為明確指派。

在定義之前，可以從詞法點呼叫區域函式。 區域函式宣告語句不會在無法連線時造成警告。

TODO：_寫入規格_

## <a name="syntax-grammar"></a>語法文法

這個文法是以與目前規格文法的差異來表示。

```diff
declaration-statement
    : local-variable-declaration ';'
    | local-constant-declaration ';'
+   | local-function-declaration
    ;

+local-function-declaration
+   : local-function-header local-function-body
+   ;

+local-function-header
+   : local-function-modifiers? return-type identifier type-parameter-list?
+       ( formal-parameter-list? ) type-parameter-constraints-clauses
+   ;

+local-function-modifiers
+   : (async | unsafe)
+   ;

+local-function-body
+   : block
+   | arrow-expression-body
+   ;
```

區域函式可以使用在封閉範圍中定義的變數。 目前的執行要求在區域函式中讀取的每個變數都必須明確指派，如同在其定義點執列區域函數一樣。 此外，區域函式定義必須在任何使用點上「執行」。

在實驗之後（例如，無法定義兩個互相遞迴的區域函式），我們自修改了我們希望明確指派的運作方式。 修訂（尚未實作為）是在區域函式的每次叫用時，都必須明確指派所有讀取于區域函式中的本機變數。 這其實比聽起來更微妙，而且還有一堆工作可以讓它正常執行。 完成後，您就能夠將您的區域函式移至其封閉區塊的結尾。

新的明確指派規則與推斷區域函式的傳回型別不相容，因此我們可能會移除推斷傳回型別的支援。

除非您將區域函式轉換成委派，否則會在實數值型別的框架中進行捕捉。 這表示您不會因為使用區域函式與捕捉而產生任何 GC 壓力。

### <a name="reachability"></a>達

我們將新增至規格

> 語句主體 lambda 運算式或區域函式的主體會被視為可連接。
