---
ms.openlocfilehash: 25756c1811d5e6dc97512ce70f99ab7fefa91c4a
ms.sourcegitcommit: 2a6dffb60718065ece95df75e1cc7eb509e48a8d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/01/2020
ms.locfileid: "79485235"
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
