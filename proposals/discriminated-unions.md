---
ms.openlocfilehash: 2e4a3a32def6900797c151264c984378b09b4988
ms.sourcegitcommit: 5983461e05be62f39c77383cb7857539518cb04f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2019
ms.locfileid: "79485123"
---

# <a name="discriminated-unions--enum-class"></a>區分聯集/`enum class`

`enum class`es 是一種新類型的宣告，有時稱為「區分等位」，其中每個可能的實例都會列出該類型，而且每個實例都是非重迭。

`enum class` 是使用下列語法定義的：

```antlr
enum_class
    : 'partial'? 'enum class' identifier type_parameter_list? type_parameter_constraints_clause* 
      '{' enum_class_body '}'
    ;

enum_class_body
    : enum_class_cases?
    | enum_class_cases ','
    ;

enum_class_cases
    : enum_class_case
    | enum_class_case ',' enum_class_cases
    ;

enum_class_case
    : enum_class
    | class_declaration
    | identifier type_parameter_list? '(' formal_parameter_list? ')'
    | identifier
    ;

```

範例語法：

```C#
enum class Shape
{
    Rectangle(float Width, float Length),
    Circle(float Radius),
}
```

## <a name="semantics"></a>語意

`enum class` 定義會定義根類型，這是與 `enum class` 宣告同名的抽象類別，以及一組成員，其中每一個都有類型，也就是根類型的子類型。 如果有多個 `partial enum class` 定義，則所有成員都會被視為列舉類別定義的成員。 不同于使用者定義的抽象類別定義，`enum class` 根類型預設為部分，並定義為具有預設的*私*用無參數的函式。

請注意，由於根類型已定義為部分抽象類別，因此也可以加入*根類型*的部分定義，其中會允許類別主體的標準語法形式。
不過，除了指定為 `enum class` 成員的任何宣告之外，沒有任何類型可以直接繼承自根類型。 此外，根類型也不允許使用者定義的函數。

有四種類型的 `enum class` 成員宣告：

* 簡單類別成員

* 複雜類別成員

* `enum class` 成員

* 值成員。

### <a name="simple-class-members"></a>簡單類別成員

簡單的類別成員宣告會定義具有相同名稱的新的嵌套「記錄」類別（在本檔中刻意保持未定義狀態）。 Nested 類別繼承自根型別。

假設上述範例程式碼，

```C#
enum class Shape
{
    Rectangle(float Width, float Length),
    Circle(float Radius)
}
```

`enum class` 宣告的語義等同于下列宣告

```C#
partial abstract class Shape
{
    public data class Rectangle(float Width, float Length) : Shape,
    public data class Circle(float Radius) : Shape
}
```

### <a name="complex-class-members"></a>複雜類別成員

您也可以在 `enum class` 宣告底下，嵌套整個類別宣告。 它會被視為根類型的嵌套類別。 語法允許任何類別宣告，但複雜類別成員必須從直接包含 `enum class` 宣告繼承。 

### <a name="enum-class-members"></a>`enum class` 成員

`enum classes` 可以彼此嵌套，例如

```C#
enum class Expr
{
    enum class Binary
    {
        Addition(Expr left, Expr right),
        Multiplication(Expr left, Expr right)
    }
}
```

這幾乎與最上層 `enum class`的語義相同，不同之處在于嵌套的 enum 類別會定義嵌套根類型，而嵌套的 enum 類別底下的所有專案，都是嵌套根類型的子類型，而不是最上層的根類型。

```C#
partial abstract class Expr
{
    partial abstract class Binary : Expr
    {
        public data class Addition(Expr left, Expr right) : Binary,
        public data class Multiplication(Expr left, Expr right) : Binary
    }
}
```

### <a name="value-members"></a>值成員

`enum classes` 也可以包含值成員。 值成員會在根型別上定義公用的僅取得靜態屬性，同時也會傳回根型別，例如

```C#
enum class Color
{
    Red,
    Green
}
```

的屬性相當於

```C#
partial abstract class Color
{
    public static Color Red => ...;
    public static Color Green => ...;
}
```

完整的語義會被視為一個執行詳細資料，但保證每個屬性都會傳回一個唯一的實例，而且重複叫用時也會傳回相同的實例。


### <a name="switch-expression-and-patterns"></a>Switch 運算式和模式

有一些建議的模式比對和切換運算式的調整，以處理 `enum classes`。 Switch 運算式已經透過變數模式來比對型別，但目前針對引用型別，switch 運算式中沒有任何一組切換臂被視為完整，除了比對引數的靜態型別或子型別。

Switch 運算式會進行變更，如此一來，如果 `enum class` 的根類型是 switch 運算式之引數的靜態類型，而且有一組模式符合列舉的所有成員，則參數會被視為完整的。

由於值成員不是常數，而且不會定義新的靜態類型，因此目前無法依模式來比對。 若要這麼做，將會加入使用常數模式語法的新模式，以允許與 `enum class` 值成員進行比對。 只有當模式比對的引數與 `enum class` 值成員所傳回的值相等時，才會將比對定義為成功，雖然不需要執行這項檢查。


## <a name="open-questions"></a>開啟問題

- [] 一般類型演算法對 `enum class` 成員的看法為何？ 這是有效的程式碼嗎？
    * `var x = b ? new Shape.Rectangle(...) : new Shape.Circle(...)`

- [] 只為值成員加入新的模式似乎很繁重。 是否有更通用的版本結構，這有意義？
    - [] 值成員也無法正確對應至平行的嵌套類別結構，因為

- [] 正在針對具有 `enum class` 靜態類型的引數進行切換，保證是固定時間？

- [] 是否有方法可讓 `enum class`在 switch 運算式中不被視為已完成？ 有 `virtual`的前置詞嗎？

- [] `enum class`上應允許哪些修飾詞？