# <a name="enums"></a>列舉

***列舉型別***不同的實值型別 ([實值型別](types.md#value-types)) 宣告一組具名常數。

此範例

```csharp
enum Color
{
    Red,
    Green,
    Blue
}
```

宣告名為列舉型別`Color`與成員`Red`， `Green`，和`Blue`。

## <a name="enum-declarations"></a>列舉宣告

列舉宣告會宣告新的列舉型別。 列舉宣告的開頭關鍵字`enum`，並定義名稱、 協助工具、 基礎型別和列舉的成員。

```antlr
enum_declaration
    : attributes? enum_modifier* 'enum' identifier enum_base? enum_body ';'?
    ;

enum_base
    : ':' integral_type
    ;

enum_body
    : '{' enum_member_declarations? '}'
    | '{' enum_member_declarations ',' '}'
    ;
```

每個列舉類型都有對應的整數類資料類型，稱為***基礎型別***的列舉型別。 這個基礎類型必須能夠代表列舉中定義的所有列舉值。 列舉宣告可以明確宣告基礎型別`byte`， `sbyte`， `short`， `ushort`， `int`， `uint`，`long`或`ulong`。 請注意，`char`不能做為基礎的類型。 沒有明確宣告的基礎類型的列舉宣告都具有基礎類型的`int`。

此範例

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

宣告列舉的基礎型別`long`。 開發人員可能會選擇使用的基礎類型`long`，如範例所示，若要允許的範圍中的值使用`long`但不是在範圍內的`int`，或保留供日後使用此選項。

## <a name="enum-modifiers"></a>列舉修飾詞

*Enum_declaration*可以選擇性地包含一連串的列舉修飾詞：

```antlr
enum_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;
```

它是相同的修飾詞在 enum 宣告中出現多次的編譯時期錯誤。

列舉宣告的修飾詞具有相同的意義的類別宣告 ([類別修飾詞](classes.md#class-modifiers))。 不過請注意，`abstract`和`sealed`enum 宣告中不允許修飾詞。 列舉不可為抽象，且不允許衍生。

## <a name="enum-members"></a>列舉成員

列舉型別宣告的主體會定義零或多個列舉成員，也就是列舉類型的具名的常數。 沒有兩個列舉的成員可以有相同的名稱。

```antlr
enum_member_declarations
    : enum_member_declaration (',' enum_member_declaration)*
    ;

enum_member_declaration
    : attributes? identifier ('=' constant_expression)?
    ;
```

每個列舉的成員具有相關聯的常數值。 此值的型別是包含列舉的基礎類型。 每個列舉成員的常值必須是列舉的基礎類型的範圍中。 此範例

```csharp
enum Color: uint
{
    Red = -1,
    Green = -2,
    Blue = -3
}
```

因為會導致編譯時期錯誤的常數值`-1`， `-2`，並`-3`不在基礎整數類資料類型的範圍中`uint`。

多個列舉的成員可能會共用同一個相關聯的值。 此範例

```csharp
enum Color 
{
    Red,
    Green,
    Blue,

    Max = Blue
}
```

顯示兩個列舉成員，列舉`Blue`和`Max`-具有相同的關聯值。

隱含或明確指派的列舉成員相關聯的值。 列舉成員的宣告是否*constant_expression*初始設定式、 常數運算式，會隱含地轉換為列舉的基礎類型的值是列舉成員的相關聯的值。 如果列舉成員的宣告有沒有初始設定式，其相關聯的值是隱含設定，如下所示：

*  如果列舉成員的列舉類型中宣告的第一個列舉成員，其相關聯的值為零。
*  否則，列舉成員的相關聯的值取得來增加一個賦予一個列舉成員的相關聯的值。 這個增加的值必須是可表示的基礎類型的值的範圍內，否則會發生編譯時期錯誤。

此範例

```csharp
using System;

enum Color
{
    Red,
    Green = 10,
    Blue
}

class Test
{
    static void Main() {
        Console.WriteLine(StringFromColor(Color.Red));
        Console.WriteLine(StringFromColor(Color.Green));
        Console.WriteLine(StringFromColor(Color.Blue));
    }

    static string StringFromColor(Color c) {
        switch (c) {
            case Color.Red: 
                return String.Format("Red = {0}", (int) c);

            case Color.Green:
                return String.Format("Green = {0}", (int) c);

            case Color.Blue:
                return String.Format("Blue = {0}", (int) c);

            default:
                return "Invalid color";
        }
    }
}
```

印出的列舉成員名稱和其相關聯的值。 輸出為：

```
Red = 0
Green = 10
Blue = 11
```

原因如下：

*  列舉成員`Red`會自動指派值為零 （因為它沒有初始設定式，而且是第一個列舉成員）;
*  列舉成員`Green`明確的值會指定`10`;
*  和列舉成員`Blue`會自動指派值的值大於賦予在它之前的成員。

列舉成員的相關聯的值可能不是，直接或間接使用它自己相關聯的列舉成員的值。 非此循環限制，列舉成員初始設定式可以自由參考其他列舉成員初始設定式，無論其文字的位置。 列舉成員初始設定式內其他列舉成員的值會一律視為擁有其基礎類型，以便參考其他列舉成員時，不需要轉換 （cast）。

此範例

```csharp
enum Circular
{
    A = B,
    B
}
```

因為會導致編譯時期錯誤的宣告`A`和`B`會循環。 `A` 取決於`B`明確地與`B`取決於`A`隱含。

名為列舉成員，並範圍內之類別完全一樣的方式。 列舉成員的範圍是其包含的列舉類型的主體。 在該範圍中，列舉成員可以被指其簡單的名稱。 從所有其他的程式碼，列舉成員的名稱必須限定其列舉型別的名稱。 列舉成員並沒有任何已宣告存取範圍 (列舉成員是可存取其包含的列舉型別是否可存取）。

## <a name="the-systemenum-type"></a>System.Enum 類型

型別`System.Enum`（這是相異且不同於列舉型別的基礎型別） 的所有列舉類型的抽象基底都類別和成員繼承自`System.Enum`位於任何列舉型別。 Boxing 轉換 ([Boxing 轉換](types.md#boxing-conversions)) 存在從任何列舉型別`System.Enum`，和 unboxing 轉換 ([Unboxing 轉換](types.md#unboxing-conversions)) 從`System.Enum`任何列舉類型。

請注意，`System.Enum`本身並不會*enum_type*。 而是*class_type*所有*enum_type*s 衍生。 型別`System.Enum`繼承自型別`System.ValueType`([System.ValueType 型別](types.md#the-systemvaluetype-type))，它會接著繼承自型別`object`。 在執行階段，類型的值`System.Enum`可以是`null`或任何列舉類型的 boxed 值的參考。

## <a name="enum-values-and-operations"></a>列舉值和作業

每個列舉類型定義不同的類型;明確的列舉型別轉換 ([明確的列舉型別轉換](conversions.md#explicit-enumeration-conversions))，才可將列舉型別之間的整數類資料類型，或兩個列舉類型之間轉換。 列舉成員不會受限於一組可對列舉類型的值。 特別的是，列舉的基礎類型的任何值可以轉換為列舉型別，是該列舉型別的相異有效值。

列舉成員具有其包含的列舉型別的型別 (除了其他列舉成員初始設定式內： 請參閱[列舉成員](enums.md#enum-members))。 列舉類型中宣告的列舉成員值`E`相關聯的值`v`是`(E)v`。

下列運算子可用於列舉類型的值： `==`， `!=`， `<`， `>`， `<=`， `>=` ([列舉型別比較運算子](expressions.md#enumeration-comparison-operators))，的二進位檔`+`([加法運算子](expressions.md#addition-operator))、 二進位`-`([減法運算子](expressions.md#subtraction-operator))， `^`， `&`， `|` ([列舉邏輯運算子](expressions.md#enumeration-logical-operators))， `~` ([位元補充運算子](expressions.md#bitwise-complement-operator))，`++`並`--`([後置遞增和遞減運算子](expressions.md#postfix-increment-and-decrement-operators)並[前置遞增和遞減運算子](expressions.md#prefix-increment-and-decrement-operators))。

每個列舉型別會自動將衍生自類別`System.Enum`(其中，又，衍生自`System.ValueType`和`object`)。 因此，這個類別的繼承的方法和屬性可用的列舉類型的值。
