---
ms.openlocfilehash: f000dda7eeb1c4f17c26f94c326a12a9d0014288
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281966"
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

## <a name="specification"></a>規格
[design]: #detailed-design

*Object_creation_expression*的新語法表單*target_typed_new*接受，其中*類型*是選擇性的。

```antlr
object_creation_expression
    : 'new' type '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    | target_typed_new
    ;
target_typed_new
    : 'new' '(' argument_list? ')' object_or_collection_initializer?
    ;
```

*Target_typed_new*運算式沒有類型。 不過，有一個新的*物件建立轉換*是從運算式隱含轉換，這是從*target_typed_new*到每個型別。

假設有目標型別 `T`，如果 `T` 是 `System.Nullable`的實例，則類型 `T0` 會 `T`的基礎類型。 否則會 `T``T0`。 轉換成類型 `T` 之*target_typed_new*運算式的意義，與指定 `T0` 做為類型之對應*object_creation_expression*的意義相同。

如果*target_typed_new*當做一元或二元運算子的運算元使用，或如果使用它而不受*物件建立轉換*，則為編譯時期錯誤。

> **開啟問題：** 我們是否允許委派和元組作為目標型別？

上述規則包括委派（參考型別）和元組（結構型別）。 雖然這兩種類型都是可建構的，但如果類型是 inferable，則可以使用匿名函式或元組常值。
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // OK; same as (0, 0)
Action a = new(); // no constructor found
```

### <a name="miscellaneous"></a>其他

以下是規格的結果：

- 允許 `throw new()` （目標型別為 `System.Exception`）
- 二元運算子不允許使用目標具類型的 `new`。
- 當沒有要設為目標的類型時，不允許使用：一元運算子、`foreach`的集合在 `using`中，在 `await` 運算式的解構中，以匿名型別屬性（`new { Prop = new() }`），在 `lock` 語句中，在 `sizeof`的成員存取（`fixed`）的`new().field`語句中，以動態分派的作業（`someDynamic.Method(new())`）表示 `is` 運算子的運算元，做為 `??` 運算子的左運算元。,  ...
- 也不允許做為 `ref`。
- 下列種類的類型不允許做為轉換的目標
  - **列舉類型：** `new()` 將可運作（因為 `new Enum()` 會提供預設值），但 `new(1)` 將無法運作，因為列舉類型沒有任何方法。
  - **介面類別型：** 這適用于 COM 類型的對應建立運算式。
  - **陣列類型：** 陣列需要特殊的語法來提供長度。    
  - **動態：** 我們不允許 `new dynamic()`，因此不允許以 `dynamic` 作為目標型別的 `new()`。
  - **元組：** 這些與使用基礎類型建立物件的意義相同。
  - 也會排除*object_creation_expression*中不允許的所有其他類型，例如指標類型。   

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
- [LDM-2020-03-25](https://github.com/dotnet/csharplang/blob/master/meetings/2020/LDM-2020-03-25.md)
