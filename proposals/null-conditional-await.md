---
ms.openlocfilehash: 3d10cacef98e802333c8cbe65edb93c19c74cabf
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484493"
---
# <a name="null-conditional-await"></a>null-條件式 await

* [x] 提議
* [] 原型：無
* [] 執行：無
* [] 規格：已啟動，下方

## <a name="summary"></a>摘要
[summary]: #summary

支援表單 `await? e`的運算式，如果它不是 null，則會等候 `e`，否則會導致 `null`。

## <a name="motivation"></a>動機
[motivation]: #motivation

這是常見的編碼模式，而且這項功能會與現有的 null 傳播和 null 聯合運算子具有良好的協力。

## <a name="detailed-design"></a>詳細設計
[design]: #detailed-design

我們新增了一種新形式的*await_expression*：

```antlr
await_expression
    : 'await' '?' unary_expression
    ;
```

Null 條件 `await` 運算子只有在該運算元不是 null 時，才會等候其運算元。 否則，套用運算子的結果為 null。

結果的類型是使用[null 條件運算子的規則來](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#null-conditional-operator)計算。

> **注意：** 如果 `e` 的類型為 `Task`，則 `await? e;` 會在 `e` `null`時不會執行任何動作，而如果未 `e`，則會進行 await `null`。
>
> 如果 `e` 的類型 `Task<K>`，其中 `K` 是實數值型別，則 `await? e` 會產生類型 `K?`的值。

## <a name="drawbacks"></a>缺點
[drawbacks]: #drawbacks

就像任何語言功能一樣，我們還必須問您，是否重金換回其他語言的複雜性，以提供給可從功能獲益C#的程式主體。

## <a name="alternatives"></a>替代方案
[alternatives]: #alternatives

雖然它需要一些重複使用的程式碼，但此運算子的用法通常會取代為類似 `(e == null) ? null : await e` 的運算式或類似 `if (e != null) await e`的語句。

## <a name="unresolved-questions"></a>未解決的問題
[unresolved]: #unresolved-questions

- [] 需要 LDM 審查

## <a name="design-meetings"></a>設計會議

無。
