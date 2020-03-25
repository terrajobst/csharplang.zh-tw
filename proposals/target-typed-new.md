---
ms.openlocfilehash: 07b4afe4a3fcbf10c978f05e642dfd8a47d53ea5
ms.sourcegitcommit: 194a043db72b9244f8db45db326cc82de6cec965
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/24/2020
ms.locfileid: "80217199"
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

- **任何結構類型**（包括元組類型）
- **任何參考型別**（包括委派類型）
- 具有函數或 `struct` 條件約束的**任何類型參數**

但有下列例外狀況：

- **列舉類型：** 並非所有列舉類型都包含常數零，因此最好使用明確列舉成員。
- **介面類別型：** 這是一項小規模功能，最好是明確提及該類型。
- **陣列類型：** 陣列需要特殊的語法來提供長度。
- **動態：** 我們不允許 `new dynamic()`，因此不允許以 `dynamic` 作為目標型別的 `new()`。

也會排除*object_creation_expression*中不允許的所有其他類型，例如指標類型。

當目標型別是可為 null 的實值型別時，目標型別 `new` 將會轉換成基礎型別，而不是可為 null 的型別。

> **開啟問題：** 我們是否允許委派和元組作為目標型別？

上述規則包括委派（參考型別）和元組（結構型別）。 雖然這兩種類型都是可建構的，但如果類型是 inferable，則可以使用匿名函式或元組常值。
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found
```

### <a name="miscellaneous"></a>其他

不允許 `throw new()`。

二元運算子不允許使用目標具類型的 `new`。

當沒有要設為目標的類型時，不允許使用：一元運算子、`foreach`的集合在 `using`中，在 `await` 運算式的解構中，以匿名型別屬性（`new { Prop = new() }`），在 `lock` 語句中，在 `sizeof`的成員存取（`fixed`）的`new().field`語句中，以動態分派的作業（`someDynamic.Method(new())`）表示 `is` 運算子的運算元，做為 `??` 運算子的左運算元。,  ...

也不允許做為 `ref`。

## <a name="drawbacks"></a>缺點
[drawbacks]: #drawbacks

有一些關於目標型別的問題 `new` 建立新的重大變更分類，但我們已經有 `null` 和 `default`的問題，而且這一點也不是重大的問題。

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
- [LDM-2018-05-21](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [LDM-2018-06-25](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [LDM-2018-08-22](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [LDM-2018-10-17](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
