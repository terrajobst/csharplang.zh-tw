---
ms.openlocfilehash: fdd858c895d56a7a6b410e6ea7be3032e4851fd6
ms.sourcegitcommit: 5a88d5432d32c690c6b870fc4be32cf26cadd76f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/11/2019
ms.locfileid: "79484955"
---
# <a name="null-coalescing-assignment"></a>Null 聯合指派

* [x] 提議
* [] 原型：未啟動
* [] 執行：未啟動
* [] 規格：以下

## <a name="summary"></a>摘要
[summary]: #summary

簡化一般程式碼模式，其中如果變數是 null，則會將值指派給它。

在本提案中，我們也會放寬 `??` 的型別需求，以允許其型別為不受限制的型別參數在左側使用的運算式。

## <a name="motivation"></a>動機
[motivation]: #motivation

通常會看到表單的程式碼

```csharp
if (variable == null)
{
    variable = expression;
}
```

此提議會將非可多載的二元運算子新增至執行此函式的語言。

這項功能至少有八個個別的社區要求。

## <a name="detailed-design"></a>詳細設計
[design]: #detailed-design

我們新增指派運算子的形式

``` antlr
assignment_operator
    : '??='
    ;
```

這會遵循[複合指派運算子的現有語義規則](../../spec/expressions.md#compound-assignment)，但如果左側不是 null，我們會省略指派。 這項功能的規則如下所示。

指定 `a ??= b`，其中 `A` 是 `a`的類型，`B` 是 `b`的類型，而 `A0` 是可為 null 的實數值型別時，`A` 的基礎類型：`A`

1. 如果 `A` 不存在，或不是不可為 null 的實值型別，就會發生編譯時期錯誤。
2. 如果 `B` 無法隱含地轉換成 `A` 或 `A0` （如果 `A0` 存在），就會發生編譯時期錯誤。
3. 如果 `A0` 存在，而且 `B` 可以隱含地轉換成 `A0`，而且 `B` 不是動態的，則 `a ??= b` 的類型會是 `A0`。 `a ??= b` 在執行時間評估為：
   ```C#
   var tmp = a.GetValueOrDefault();
   if (!a.HasValue) { tmp = b; a = tmp; }
   tmp
   ```
   除了 `a` 只會評估一次。
4. 否則，會 `A``a ??= b` 的類型。 `a ??= b` 在執行時間會當做 `a ?? (a = b)`進行評估，但 `a` 只會評估一次。


如需 `??`類型需求的放寬，我們會更新其目前陳述的規格，並指定 `a ?? b`，其中 `A` 是 `a`的類型：

> 1. 如果存在且不是可為 null 的型別或參考型別，就會發生編譯時期錯誤。

我們放寬了這項需求：

1. 如果存在，而且是不可為 null 的實數值型別，則會發生編譯時期錯誤。

這可讓 null 聯合運算子在不受限制的型別參數上工作，因為不受限的型別參數 T 不存在、不是可為 null 的型別，而且不是參考型別。

## <a name="drawbacks"></a>缺點
[drawbacks]: #drawbacks

就像任何語言功能一樣，我們還必須問您，是否重金換回其他語言的複雜性，以提供給可從功能獲益C#的程式主體。

## <a name="alternatives"></a>替代方案
[alternatives]: #alternatives

程式設計人員可以手動撰寫 `(x = x ?? y)`、`if (x == null) x = y;`或 `x ?? (x = y)`。

## <a name="unresolved-questions"></a>未解決的問題
[unresolved]: #unresolved-questions

- [] 需要 LDM 審查
- [] 我們也應該支援 `&&=` 和 `||=` 運算子嗎？

## <a name="design-meetings"></a>設計會議

無。
