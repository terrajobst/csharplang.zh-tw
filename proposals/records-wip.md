---
ms.openlocfilehash: 258ae6865c5b2c3103a0cdf7e1e5a2cdee11e740
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281953"
---
# <a name="records-work-in-progress"></a>記錄進行中的工作

不同于其他記錄提案，這不是本身的提案，而是針對記錄功能記錄共識設計決策的工作進行中。 系統會視需要新增規格詳細資料以解決問題。

建議新增記錄的語法，如下所示：

```antlr
class_declaration
    : attributes? class_modifiers? 'partial'? 'class' identifier type_parameter_list?
      parameter_list? type_parameter_constraints_clauses? class_body
    ;

struct_declaration
    : attributes? struct_modifiers? 'partial'? 'struct' identifier type_parameter_list?
      parameter_list? struct_interfaces? type_parameter_constraints_clauses? struct_body
    ;

class_body
    : '{' class_member_declarations? '}'
    | ';'
    ;

struct_body
    : '{' struct_members_declarations? '}'
    | ';'
    ;
```

`attributes` 的非終端機也會允許新的內容屬性，`data`。

使用參數清單或 `data` 修飾詞宣告的類別（結構）稱為記錄類別（記錄結構），其中之一是記錄類型。

宣告不含參數清單和 `data` 修飾詞的記錄類型是錯誤的。

## <a name="members-of-a-record-type"></a>記錄類型的成員

除了在類別主體中宣告的成員之外，記錄類型還有下列其他成員：

### <a name="primary-constructor"></a>主要的構造函式

記錄類型具有公用的函式，其簽章對應于類型宣告的值參數。 這稱為類型的主要「函式」，並會隱藏隱含宣告的預設函數。 具有主要的函式，以及具有相同簽章的相同簽章已存在於類別中，是錯誤的。
在執行時間，主要的函式 

1. 執行出現在類別主體中的實例欄位初始化運算式;然後叫用沒有引數的基類處理函式。

1. 針對對應至值參數的屬性，初始化編譯器產生的支援欄位（如果這些屬性是由編譯器提供，請參閱[合成的屬性](#Synthesized Properties)）


[] TODO：新增基底呼叫語法和規格，以透過多載解析選擇基底函數

### <a name="properties"></a>屬性

對於記錄類型宣告的每個記錄參數，有一個對應的公用屬性成員，其名稱和類型取自值參數宣告。 如果沒有具有 get 存取子的具象（非抽象）屬性，而且已明確宣告或繼承此名稱和類型，則編譯器會產生，如下所示：

若為記錄結構或記錄類別：

* 建立公用的僅限自動屬性。 它的值會在結構化期間使用對應的主要函式參數值進行初始化。 會覆寫每個「相符」的繼承抽象屬性 get 存取子。

### <a name="equality-members"></a>等號比較成員

記錄類型會產生下列方法的合成實作為：

* `object.GetHashCode()` 覆寫，除非已密封或使用者提供
* `object.Equals(object)` 覆寫，除非已密封或使用者提供
* `T Equals(T)` 方法，其中 `T` 為目前的類型

指定 `T Equals(T)` 以執行實值相等性，並比較每個主要的函式參數與另一個類型之對應屬性的名稱相同的屬性。
`object.Equals` 執行的對等

```C#
override Equals(object o) => Equals(o as T);
```

## <a name="with-expression"></a>`with` 運算式

`with` 運算式是使用下列語法的新運算式。

```antlr
with_expression
    : switch_expression
    | switch_expression 'with' anonymous_object_initializer
```

`with` 運算式允許「非破壞性變化」，其設計目的是要產生接收者運算式的複本，並修改 `anonymous_object_initializer`中列出的屬性。

有效的 `with` 運算式具有非 void 類型的接收者。 接收器類型必須包含一個可存取的實例方法，稱為 `With` 並具有適當的參數和傳回型別。 如果有多個非覆寫 `With` 方法，這就是錯誤。 如果有多個 `With` 覆寫，就必須有非覆寫 `With` 方法，這是目標方法。 否則，必須只有一個 `With` 方法。

在 `with` 運算式右側是一個 `anonymous_object_initializer`，其中具有一系列具有指派之欄位或屬性的指派，而右邊的任意運算式則可以隱含地轉換成左邊的型別，這是一個具有序列的指定。

假設目標 `With` 方法，則傳回類型必須是接收者運算式類型的類型，或其基底類型。 針對 `With` 方法的每個參數，在具有相同名稱和相同類型的接收者類型上，必須有可存取的對應實例欄位或可讀取的屬性。 With 運算式右側的每個屬性或欄位也必須對應至 `With` 方法中相同名稱的參數。

假設有有效的 `With` 方法，`with` 運算式的評估就相當於使用 `anonymous_object_initializer` 中的運算式來呼叫 `With` 方法，以取代與左側屬性相同名稱的參數。 如果 `anonymous_object_initializer`中的指定參數沒有相符的屬性，則引數會評估接收者上相同名稱的欄位或屬性。

副作用的評估順序如下所示，每個運算式只會評估一次：

1. 接收者運算式

2. `anonymous_object_initializer`中的運算式（依詞法順序）

3. 以 `With` 方法參數定義的順序，評估符合 `With` 方法參數的任何屬性。