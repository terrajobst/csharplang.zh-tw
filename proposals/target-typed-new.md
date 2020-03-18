---
ms.openlocfilehash: 4e2a536bab00859b003e8d967cb1927a99a9fa21
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484535"
---

# <a name="target-typed-new-expressions"></a>目標型別 `new` 運算式

* [x] 提議
* [x][原型](https://github.com/alrz/roslyn/tree/features/target-typed-new)
* [] 執行
* [] 規格

## <a name="summary"></a>摘要
[summary]: #summary

當已知型別時，不需要型別規格。 

## <a name="motivation"></a>動機
[motivation]: #motivation

允許欄位初始化，而不復制類型。
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```
當可以從使用方式推斷時，允許省略類型。
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```
將物件具現化，而不將型別拼出。
```cs
private readonly static object s_syncObj = new();
```
## <a name="detailed-design"></a>詳細設計
[design]: #detailed-design

當有括弧時，會修改*object_creation_expression*語法，讓*類型*成為選擇性。 這是解決*anonymous_object_creation_expression*不明確的必要工作。
```antlr
object_creation_expression
    : 'new' type? '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;
```
目標具類型的 `new` 可以轉換成任何類型。 因此，它不會影響多載解析。 這主要是為了避免無法預期的重大變更。

在決定型別之後，將會系結引數清單和初始化運算式運算式。

運算式的型別是從目標型別推斷而來，必須是下列其中一項：

- **任何結構類型**
- **任何參考型別**
- 具有函數或 `struct` 條件約束的**任何類型參數**

但有下列例外狀況：

- **列舉類型：** 並非所有列舉類型都包含常數零，因此最好使用明確列舉成員。
- **介面類別型：** 這是一項小規模功能，最好是明確提及該類型。
- **陣列類型：** 陣列需要特殊的語法來提供長度。
- **結構預設**的函式：此規則會排除所有基本型別和大部分的實值型別。 如果您想要使用這類類型的預設值，您可以改為撰寫 `default`。

也會排除*object_creation_expression*中不允許的所有其他類型，例如指標類型。

> **開啟問題：** 我們是否允許委派和元組作為目標型別？

上述規則包括委派（參考型別）和元組（結構型別）。 雖然這兩種類型都是可建構的，但如果類型是 inferable，則可以使用匿名函式或元組常值。
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found

var x = new() == (1, 2); // ruled out by "use of struct default constructor"
var x = new(1, 2) == (1, 2) // "new" is redundant
```


> **開啟問題：** 我們是否允許以 `Exception` 的 `throw new()` 做為目標型別？

我們目前已 `throw null`，但不 `throw default` （雖然會有相同的效果）。 另一方面，`throw new()` 可以做為 `throw new Exception(...)`的縮寫。 請注意，目前的規格已允許此檔案。 `Exception` 是參考型別，而 throw 語句的規格指出運算式會轉換成 `Exception`。

> **開啟問題：** 我們是否允許使用目標型別的 `new` 搭配使用者定義的比較和算術運算子？

為了進行比較，`default` 只支援相等（使用者定義和內建）運算子。 同時支援 `new()` 的其他運算子也是有意義的嗎？

## <a name="drawbacks"></a>缺點
[drawbacks]: #drawbacks

無。

## <a name="alternatives"></a>替代方案
[alternatives]: #alternatives

大部分關於類型在欄位初始化中無法重複的抱怨，都是關於類型*引數*，而不是類型本身，我們可以只推斷類型引數，例如 `new Dictionary(...)` （或類似），並從引數或集合初始化運算式在本機推斷類型引數。

## <a name="questions"></a>問題
[questions]: #questions

- 是否應該禁止運算式樹狀架構中的使用方式？ 不要
- 功能如何與 `dynamic` 引數互動？ （無特殊處理）
- IntelliSense 應該如何與 `new()`搭配使用？ （只有在只有單一目標型別時）
## <a name="design-meetings"></a>設計會議

- [LDM-2017-10-18](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
