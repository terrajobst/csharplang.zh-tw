---
ms.openlocfilehash: 3b142d7dbda8a94a4cf2c973a2e380065dcbf5ee
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703971"
---
# <a name="enums"></a>列舉

***列舉類型***是宣告一組已命名常數的相異實數值型別（實[值](types.md#value-types)類型）。

範例

```csharp
enum Color
{
    Red,
    Green,
    Blue
}
```

宣告名為 `Color` 的列舉類型，成員 `Red`、`Green` 和 `Blue`。

## <a name="enum-declarations"></a>列舉宣告

Enum 宣告會宣告新的列舉類型。 Enum 宣告的開頭為關鍵字 `enum`，並定義列舉的名稱、存取範圍、基礎類型和成員。

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

每個列舉型別都有對應的整數型別，稱為列舉型別的***基礎型***別。 這個基礎類型必須能夠代表列舉中所定義的所有列舉值。 Enum 宣告可以明確宣告 `byte`、`sbyte`、`short`、`ushort`、`int`、`uint`、`long` 或 `ulong` 的基礎類型。 請注意，`char` 不能當做基礎類型使用。 未明確宣告基礎類型的列舉宣告具有 `int` 的基礎類型。

範例

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

宣告具有 `long` 之基礎類型的列舉。 開發人員可能會選擇使用基礎類型的 `long` （如範例所示），以啟用範圍為 `long` 但不在 `int` 範圍內的值，或保留此選項供未來使用。

## <a name="enum-modifiers"></a>Enum 修飾詞

*Enum_declaration*可以選擇性地包含一連串的 enum 修飾詞：

```antlr
enum_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;
```

在列舉宣告中多次出現相同的修飾詞時，就會發生編譯時期錯誤。

Enum 宣告的修飾詞與類別宣告（[類別](classes.md#class-modifiers)修飾詞）的修飾詞具有相同的意義。 不過，請注意，enum 宣告中不允許 `abstract` 和 @no__t 1 修飾詞。 列舉不能是抽象的，而且不允許衍生。

## <a name="enum-members"></a>列舉成員

列舉類型宣告的主體會定義零或多個列舉成員，這是列舉類型的已命名常數。 沒有兩個列舉成員可以具有相同的名稱。

```antlr
enum_member_declarations
    : enum_member_declaration (',' enum_member_declaration)*
    ;

enum_member_declaration
    : attributes? identifier ('=' constant_expression)?
    ;
```

每個列舉成員都有相關聯的常數值。 此值的類型是包含列舉的基礎類型。 每個列舉成員的常數值必須在列舉之基礎類型的範圍內。 範例

```csharp
enum Color: uint
{
    Red = -1,
    Green = -2,
    Blue = -3
}
```

產生編譯時期錯誤，因為 `-1`、`-2` 和 `-3` 的常數值不在基礎整數類型 `uint` 的範圍內。

多個列舉成員可能共用相同的關聯值。 範例

```csharp
enum Color 
{
    Red,
    Green,
    Blue,

    Max = Blue
}
```

顯示列舉，其中兩個列舉成員--`Blue`，而 `Max`--具有相同的關聯值。

列舉成員的相關值會隱含或明確地指派。 如果列舉成員的宣告具有*constant_expression*初始化運算式，則該常數運算式的值（隱含地轉換為列舉的基礎類型）是列舉成員的相關值。 如果列舉成員的宣告沒有初始化運算式，則會隱含設定其相關聯的值，如下所示：

*  如果列舉成員是列舉類型中所宣告的第一個列舉成員，其相關聯的值為零。
*  否則，會藉由將每個以上而下的列舉成員相關聯的值遞增一，來取得列舉成員的相關聯值。 這個增加的值必須在可由基礎類型表示的值範圍內，否則會發生編譯時期錯誤。

範例

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

印出列舉成員名稱及其相關聯的值。 輸出為：

```console
Red = 0
Green = 10
Blue = 11
```

原因如下：

*  列舉成員 `Red` 會自動指派值零（因為它沒有初始化運算式，而且是第一個列舉成員）;
*  列舉成員 `Green` 已明確指定值 `10`;
*  而且列舉成員 `Blue` 會自動指派一個大於以全形方式出現在其前面的成員的值。

列舉成員的相關值可能不是直接或間接使用其本身相關聯列舉成員的值。 除了此迴圈限制以外，列舉成員初始化運算式可以自由地參考其他列舉成員初始化運算式，不論其文字位置為何。 在列舉成員初始化運算式中，其他列舉成員的值一律會被視為具有其基礎類型的類型，因此在參考其他列舉成員時，並不需要轉換。

範例

```csharp
enum Circular
{
    A = B,
    B
}
```

會導致編譯時期錯誤，因為 `A` 和 `B` 的宣告是迴圈的。 `A` 會明確地相依于 `B`，而 `B` 則會隱含地相依于 `A`。

列舉成員的命名方式和範圍，與類別中的欄位完全類似。 列舉成員的範圍是其包含列舉類型的主體。 在該範圍內，列舉成員可以透過其簡單名稱來參考。 在所有其他程式碼中，列舉成員的名稱必須以其列舉類型的名稱來限定。 列舉成員沒有任何宣告的存取範圍--如果可存取其包含列舉類型，列舉成員就可以存取。

## <a name="the-systemenum-type"></a>System.string 類型

類型 `System.Enum` 是所有列舉類型的抽象基類（這是相異的，與列舉類型的基礎類型不同），而繼承自 `System.Enum` 的成員可以在任何列舉類型中使用。 從任何列舉型別到 `System.Enum` 的「裝箱」轉換（[裝箱](types.md#boxing-conversions)轉換）都存在，而且從 `System.Enum` 到任何列舉型別都有「取消裝箱」轉換（「[取消裝箱](types.md#unboxing-conversions)」轉換）。

請注意，`System.Enum` 本身並不是*enum_type*。 相反地，它是所有*enum_type*的衍生來源的*class_type* 。 @No__t-0 的型別繼承自型別 `System.ValueType` （system.string[型](types.md#the-systemvaluetype-type)別），而後者又繼承自型別 `object`。 在執行時間，`System.Enum` 類型的值可以 `null` 或任何列舉類型之已裝箱值的參考。

## <a name="enum-values-and-operations"></a>列舉值和作業

每個列舉類型都會定義不同的類型。必須要有明確的列舉轉換（[明確列舉](conversions.md#explicit-enumeration-conversions)轉換），才能在列舉類型和整數類型之間轉換，或是在兩個列舉類型之間進行轉換。 列舉型別可接受的值集合不受其列舉成員的限制。 特別是，列舉之基礎類型的任何值都可以轉換成列舉類型，而且是該列舉類型的相異有效值。

Enum 成員具有其包含列舉類型的類型（但在其他列舉成員初始化運算式內除外）：請參閱[列舉成員](enums.md#enum-members)）。 列舉類型中宣告的列舉成員值 `E`，`v` 的相關聯值為 `(E)v`。

下列運算子可用於列舉類型的值： `==`、`!=`、`<`、`>`、`<=`、`>=` @ no__t-6 （[列舉比較運算子](expressions.md#enumeration-comparison-operators)）、binary `+` @ no__t-9 （[加法運算子](expressions.md#addition-operator)）、binary 1 @ no__t-12 （[減法運算子](expressions.md#subtraction-operator)）、4、5、6 @ no__t-17 （[列舉邏輯運算子](expressions.md#enumeration-logical-operators)）、9 @ No__t-20 （[位補數運算子](expressions.md#bitwise-complement-operator)）、2 和 3 @ no__t-24 （後置[增量和遞減運算子](expressions.md#postfix-increment-and-decrement-operators)和[前置遞增和遞減運算子](expressions.md#prefix-increment-and-decrement-operators)）。

每個列舉類型都會自動衍生自類別 `System.Enum` （而這又衍生自 `System.ValueType` 和 `object`）。 因此，可以在列舉類型的值上使用這個類別的繼承方法和屬性。
