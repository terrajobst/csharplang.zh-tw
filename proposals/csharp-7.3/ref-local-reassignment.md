---
ms.openlocfilehash: c1a77d9337ecd16f5ea1c30d18f6422552b0dfb2
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484598"
---
# <a name="ref-local-reassignment"></a>Ref 本機重新指派

在C# 7.3 中，我們新增了重新系結 ref 區域變數或 ref 參數之參考的支援。

我們會將下列新增至 `assignment_operator`的集合中。

```antlr
assignment_operator
    : '=' 'ref'
    ;
```

`=ref` 運算子稱為***ref 指派運算子***。 這不是*複合指派運算子*。 左運算元必須是系結至 ref 區域變數、ref 參數（不是 `this`）或 out 參數的運算式。 右運算元必須是產生左值的運算式，並指定與左運算元相同類型的值。

右運算元必須在 ref 指派的位置明確指派。

當左運算元系結至 `out` 參數時，如果未在 ref 指派運算子的開頭明確指派 `out` 參數，則會產生錯誤。

如果左運算元是可寫入的 ref （也就是指定了 `ref readonly` 本機或 `in` 參數以外的任何值），則右運算元必須是可寫入的左值。

Ref 指派運算子會產生指派類型的左值。 如果左運算元是可寫入的（亦即，不是 `ref readonly` 或 `in`），則可寫入。

此運算子的安全性規則包括：

- 針對 ref 重新指派 `e1 = ref e2`，`e2` 的*ref 安全對 escape*必須至少與 `e1`的*ref 安全到 escape*的範圍相同。

在 ref 型別的[安全性](../csharp-7.2/span-safety.md)中，會定義*ref-安全對 escape*
