---
ms.openlocfilehash: a28397b1ce97dbead6d5014e2b20e108a1018502
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229550"
---
# <a name="types"></a>型別

C# 語言的類型可分為兩種主要類別：***實值型別***並***參考型別***。 可能是實值型別和參考型別***泛型型別***，這需要一或多個***型別參數***。 類型參數可以指定這兩個實值型別和參考型別。

```antlr
type
    : value_type
    | reference_type
    | type_parameter
    | type_unsafe
    ;
```

最後一類的類型，指標是只能用於 unsafe 程式碼。 這個討論中進一步[指標類型](unsafe-code.md#pointer-types)。

實值型別不同於參考類型，因為實值型別的變數直接包含其資料，而參考的變數類型的存放區***參考***其資料，後者即***物件***. 參考型別，它是兩種變數可以參考相同的物件，並因此可能會影響其他變數所參考的物件的一個變數上進行的作業。 具有實值類型，每個變數都有自己一份資料，而且不可能會影響其他的其中一個上的作業。

C# 類型系統統一，任何類型的值可視為物件。 C# 中的每個型別都直接或間接衍生自 `object` 類別型別，而 `object` 是所有型別的基底類別。 參考型別的值之所以會視為物件，只是將這些值當作 `object` 型別來檢視。 實值類型的值時，會被當作物件上，執行 boxing 和 unboxing 作業 ([Boxing 和 unboxing](types.md#boxing-and-unboxing))。

## <a name="value-types"></a>值類型

實值型別是結構類型或列舉類型。 C# 提供一組預先定義的結構類型稱為***簡單型別***。 簡單型別是透過保留字識別。

```antlr
value_type
    : struct_type
    | enum_type
    ;

struct_type
    : type_name
    | simple_type
    | nullable_type
    ;

simple_type
    : numeric_type
    | 'bool'
    ;

numeric_type
    : integral_type
    | floating_point_type
    | 'decimal'
    ;

integral_type
    : 'sbyte'
    | 'byte'
    | 'short'
    | 'ushort'
    | 'int'
    | 'uint'
    | 'long'
    | 'ulong'
    | 'char'
    ;

floating_point_type
    : 'float'
    | 'double'
    ;

nullable_type
    : non_nullable_value_type '?'
    ;

non_nullable_value_type
    : type
    ;

enum_type
    : type_name
    ;
```

不同於參考類型的變數，實值型別的變數可以包含值`null`只有實值型別是可為 null 的型別。  每個不可為 null 的實值型別沒有對應的可為 null 的實值型別，用來表示相同的值加上值集`null`。

指派給變數的實值型別會建立一份所指派的值。 這不同於指派給變數的參考型別，它將會複製參考，而不是參考所識別的物件。

### <a name="the-systemvaluetype-type"></a>System.ValueType 類型

所有實值型別都隱含地繼承自類別`System.ValueType`，它會接著繼承自類別`object`。 不可能從實值型別衍生任何型別和實值型別都隱含密封 ([密封類別](classes.md#sealed-classes))。

請注意，`System.ValueType`本身並不會*value_type*。 而是*class_type*所有*value_type*s 自動衍生。

### <a name="default-constructors"></a>預設建構函式

所有實值型別會隱含地宣告公用的無參數執行個體建構函式呼叫***預設建構函式***。 預設建構函式會傳回零初始化執行個體稱為***預設值***實值型別：

*  針對所有*simple_type*s，預設值是全部為零的位元模式所產生的值：
    * 針對`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`，並`ulong`，預設值是`0`。
    * 針對`char`，預設值是`'\x0000'`。
    * 針對`float`，預設值是`0.0f`。
    * 針對`double`，預設值是`0.0d`。
    * 針對`decimal`，預設值是`0.0m`。
    * 針對`bool`，預設值是`false`。
*  針對*enum_type* `E`，預設值是`0`轉換成的型別、 `E`。
*  針對*struct_type*，預設值是藉由將所有實值型別欄位設定為其預設值和所有參考型別欄位為產生的值`null`。
*  針對*nullable_type*的預設值是為其執行個體`HasValue`屬性為 false，`Value`未定義屬性。 預設值就是所謂***hodnota***可為 null 的型別。

任何其他執行個體建構函式，例如實值類型的預設建構函式會叫用使用`new`運算子。 基於效率考量，這項需求不是實際擁有 產生建構函式呼叫的實作。 在下列範例中，變數`i`和`j`都初始化為零。

```csharp
class A
{
    void F() {
        int i = 0;
        int j = new int();
    }
}
```

每個實值型別以隱含方式具有公用無參數的執行個體建構函式，因為它不可能包含明確無參數建構函式宣告為結構類型。 不過允許宣告參數化的執行個體建構函式的結構類型 ([建構函式](structs.md#constructors))。

### <a name="struct-types"></a>結構型別

結構類型是常數、 欄位、 方法、 屬性、 索引子、 運算子、 執行個體建構函式、 靜態的建構函式和巢狀型別可以宣告實值型別。 建構型別的宣告述[結構宣告](structs.md#struct-declarations)。

### <a name="simple-types"></a>簡單型別

C# 提供一組預先定義的結構類型稱為***簡單型別***。 簡單型別透過保留字，但這些保留的字是只在預先定義的結構類型的別名`System`命名空間，在下表中所述。


| __保留的字__ | __別名類型__ |
|-------------------|------------------|
| `sbyte`           | `System.SByte`   | 
| `byte`            | `System.Byte`    | 
| `short`           | `System.Int16`   | 
| `ushort`          | `System.UInt16`  | 
| `int`             | `System.Int32`   | 
| `uint`            | `System.UInt32`  | 
| `long`            | `System.Int64`   | 
| `ulong`           | `System.UInt64`  | 
| `char`            | `System.Char`    | 
| `float`           | `System.Single`  | 
| `double`          | `System.Double`  | 
| `bool`            | `System.Boolean` | 
| `decimal`         | `System.Decimal` | 

簡單類型別名的結構類型，因為每個簡單的型別具有成員。 比方說，`int`已宣告中的成員`System.Int32`及成員繼承自`System.Object`，並允許下列陳述式：

```csharp
int i = int.MaxValue;           // System.Int32.MaxValue constant
string s = i.ToString();        // System.Int32.ToString() instance method
string t = 123.ToString();      // System.Int32.ToString() instance method
```

簡單型別與其他結構型別的差異在於它們可允許某些額外的作業：

*  大部分的簡單型別允許值，以建立撰寫*常值*([常值](lexical-structure.md#literals))。 比方說，`123`是常值型別的`int`並`'a'`是類型的常值`char`。 C# 一般情況下，對結構類型的常值沒有佈建，而透過這些結構類型的執行個體建構函式最終一律建立其他的結構類型的非預設值。
*  簡單類型常數運算式的運算元時，就可以評估在編譯時間運算式編譯器。 這類運算式就所謂*constant_expression* ([常數運算式](expressions.md#constant-expressions))。 定義的其他型別運算子的運算式不會被視為常數運算式。
*  透過`const`宣告就可以宣告簡單類型的常數 ([常數](classes.md#constants))。 不可以有常數的其他型別，但會提供類似的效果`static readonly`欄位。
*  包含簡單的型別轉換可以參與其他型別，所定義的轉換運算子的評估，但使用者定義轉換運算子不能參與另一個使用者定義運算子的評估結果 ([評估使用者定義轉換](conversions.md#evaluation-of-user-defined-conversions))。

### <a name="integral-types"></a>整數類資料型別

C# 支援九個整數類資料類型： `sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`，和`char`。 整數類資料類型會有下列的大小和範圍的值：

*  `sbyte`型別代表帶正負號的 8 位元整數，其值介於-128 到 127 之間。
*  `byte`型別代表不帶正負號的 8 位元整數，其值介於 0 和 255 之間。
*  `short`型別代表帶正負號的 16 位元整數，其值介於-32768 和 32767 之間。
*  `ushort`型別代表不帶正負號的 16 位元整數，其值介於 0 到 65535 之間。
*  `int`型別代表帶正負號的 32 位元整數，其值介於-2147483648 到 2147483647 之間。
*  `uint`型別代表不帶正負號的 32 位元整數，其值介於 0 和 4294967295 之間。
*  `long`型別代表帶正負號的 64 位元整數，其值介於-9223372036854775808 和 9223372036854775807。
*  `ulong`型別代表不帶正負號的 64 位元整數，其值介於 0 和 18446744073709551615 之間。
*  `char`型別代表不帶正負號的 16 位元整數，其值介於 0 到 65535 之間。 可能的值為一組`char`類型會對應至 Unicode 字元集。 雖然`char`具有相同的表示法`ushort`，並非所有允許一種類型的作業可以在其他。

整數型別一元和二元運算子一律操作與帶正負號的 32 位元精確度、 不帶正負號的 32 位元精確度、 帶正負號的 64 位元精確度或不帶正負號的 64 位元精確度：

*  一元`+`並`~`運算子，運算元會轉換成類型`T`，其中`T`的正是第一`int`， `uint`， `long`，和`ulong`可完全代表所有可能的運算元的值。 使用型別的精確度再執行作業`T`，且結果類型是`T`。
*  一元`-`運算子，運算元會轉換成類型`T`，其中`T`是第一封`int`和`long`可完全代表運算元的所有可能的值。 使用型別的精確度再執行作業`T`，且結果類型是`T`。 一元`-`無法將運算子套用至類型的運算元`ulong`。
*  二進位檔`+`， `-`， `*`， `/`， `%`， `&`， `^`， `|`， `==`， `!=`， `>`， `<`， `>=`，並`<=`運算子，運算元會轉換為類型`T`，其中`T`的正是第一`int`， `uint`， `long`，和`ulong`可完全代表所有可能這兩個運算元的值。 使用型別的精確度再執行作業`T`，且結果類型是`T`(或`bool`關聯式運算子)。 它不允許有一個運算元必須是類型`long`，另一個則為型別`ulong`具有二元運算子。
*  二進位檔`<<`並`>>`運算子的左運算元轉換為類型`T`，其中`T`的正是第一`int`， `uint`， `long`，和`ulong`可完全代表所有可能的運算元的值。 使用型別的精確度再執行作業`T`，且結果類型是`T`。

`char`型別被歸類為整數型別，但不同於其他整數類資料類型有兩種：

*  從其他類型沒有隱含轉換`char`型別。 特別的是，即使`sbyte`， `byte`，和`ushort`型別有的是完全可使用的值範圍`char`輸入時，隱含的轉換，從`sbyte`， `byte`，或`ushort`至`char`不存在。
*  常數`char`型別必須寫成*character_literal*s 或*integer_literal*搭配轉型為類型 s `char`。 例如，`(char)10` 與 `'\x000A'` 相同。

`checked`並`unchecked`運算子和陳述式用來控制溢位檢查整數型別算術運算和轉換 ([checked 與 unchecked 運算子](expressions.md#the-checked-and-unchecked-operators))。 在 `checked`溢位的內容會產生編譯時期錯誤，或導致`System.OverflowException`擲回。 在 `unchecked`內容溢位則會忽略和目的型別不符合任何高序位位元會被捨棄。

### <a name="floating-point-types"></a>浮點類型

C# 支援兩個浮點數型別：`float`和`double`。 `float`和`double`類型會表示使用 32 位元單精確度和 64 位元雙精確度 IEEE 754 格式，可提供下列值的集合：

*  正零和負零。 在大部分情況下，正零和負零運作方式完全相同的簡單值零，但某些作業區別這兩個 ([除法運算子](expressions.md#division-operator))。
*  正無限大和負無限大方向。 將會產生這類作業，做為非零的數字除以零。 例如，`1.0 / 0.0`會產生無限大的正數，和`-1.0 / 0.0`產生負無限大。
*  ***不是數字***值，通常縮寫的 NaN。 無效的浮點運算，例如零除以零會產生 Nan。
*  有限的非零值的表單`s * m * 2^e`，其中`s`為 1 或-1，並`m`和`e`取決於特定的浮點類型：針對`float`，`0 < m < 2^24`並`-149 <= e <= 104`，以及`double`，`0 < m < 2^53`和`1075 <= e <= 970`。 反正規化浮點的數字會被視為有效的非零值。

`float`型別可以代表值的範圍從大約`1.5 * 10^-45`到`3.4 * 10^38`精確度為 7 位數。

`double`型別可以代表值的範圍從大約`5.0 * 10^-324`到`1.7 × 10^308`具有 15-16 位數精確度。

如果其中一個二元運算子的運算元是浮點類型，然後將另一個運算元必須是整數類資料類型或浮點類型，和運算的評估，如下所示：

*  如果其中一個運算元屬於整數類資料類型時，運算元會轉換到另一個運算元的浮點類型。
*  然後，如果任一運算元的類型`double`，另一個運算元會轉換成`double`，為作業使用最少`double`範圍和有效位數和結果的型別是`double`(或`bool`的關係運算子）。
*  否則，為作業使用至少`float`範圍和有效位數和結果的類型是`float`(或`bool`關聯式運算子)。

浮點數的運算子，包括指派運算子，永遠不會產生例外狀況。 相反地，在例外狀況的情況下，浮點運算產生零個、 無限大或 NaN，如下所述：

*  如果浮點運算的結果太小，目的格式，作業的結果會成為正零或負零。
*  如果浮點運算的結果太大，目的格式，作業的結果會成為無限大的正數或負的無限值。
*  如果浮點運算無效，作業的結果就會變成 NaN。
*  如果浮點運算的一個或兩個運算元是 NaN，作業的結果就會變成 NaN。

浮點運算可能會執行，而較高的精確度卻高於此作業的結果類型。 例如，某些硬體架構支援 「 extended 」 或 「 long double"浮點類型，具有較大範圍和精確度卻高於`double`類型，而且以隱含方式執行所有使用這個較高的有效位數類型的浮點運算。 只有在過度耗用效能可以這類硬體架構是執行較少的精確度，浮點運算，而不需要實作，以同時損失效能和精確度，C# 允許較高的有效位數類型為用於所有的浮點運算。 以外提供更精確的結果，這很少會有任何明顯的影響。 不過，在表單的運算式`x * y / z`、 乘法運算產生的結果，超出`double`範圍，但後續的除法帶來暫時的結果，回`double`範圍，此運算式的事實評估在更高版本範圍格式可能會導致有限的結果，而不是無限大的值產生。

### <a name="the-decimal-type"></a>Decimal 型別

`decimal` 型別是 128 位元的資料型別，適合財務和貨幣計算。 `decimal`型別可以代表值的範圍從`1.0 * 10^-28`到大約`7.9 * 10^28`具有 28-29 個有效位數。

有限的一組值的型別`decimal`的格式`(-1)^s * c * 10^-e`，其中登`s`是 0 或 1，決定係數`c`由提供`0 <= *c* < 2^96`，和小數位數`e`會使得`0 <= e <= 28`。`decimal`類型不帶正負號的零、 無限或 NaN 的支援。 A`decimal`以調整的 10 乘冪的 96 位元整數。 針對`decimal`與絕對值小於`1.0m`，值是確切為 28 小數位數，但就對了。 針對`decimal`與絕對值大於或等於`1.0m`，值就會是 28 或 29 位數。 Contrary 來`float`並`double`資料類型，例如 0.1 十進位小數的數字可以完全代表`decimal`表示法。 在 `float`和`double`表示法，這樣的數字通常是無限的分數，讓那些表示法更容易捨入錯誤。

如果其中一個二元運算子的運算元為類型`decimal`，然後將另一個運算元必須是整數類資料類型或類型`decimal`。 如果整數類資料類型的運算元，則它會轉換成`decimal`執行作業之前。

類型的值上作業的結果`decimal`，這會造成的計算的確切的結果 （保留擴展、 為每個運算子所定義），然後捨入成表示法。 結果會捨入為最接近的可表示值，當結果同樣很接近兩個可顯示的值 （這稱為 「 銀行進位 」） 的最小顯著性數字位置有偶數數目的值。 零結果一律會有 0 的符號，小數位數 0。

如果小數點的算術運算會產生值小於或等於`5 * 10^-29`絕對值，在作業的結果會變成零。 如果`decimal`算術運算會產生太大的結果`decimal`格式，`System.OverflowException`就會擲回。

`decimal`型別具有較高有效位數比浮點數類型但較小的範圍。 因此，從浮點類型的轉換`decimal`可能會產生溢位例外狀況，並從轉換`decimal`於浮點類型可能會導致遺失有效位數。 基於這些理由，沒有隱含轉換則是浮點類型之間存在並`decimal`，，而不需要明確的轉型，則不可以混合使用浮點和`decimal`中同一個運算式的運算元。

### <a name="the-bool-type"></a>布林值類型

`bool`類型代表布林值邏輯的數量。 可能的值型別的`bool`都`true`和`false`。

標準轉換存在之間`bool`和其他類型。 特別是，`bool`型別是有別，各自獨立的整數類資料類型，和`bool`值不能用來取代一個整數值，反之亦然。

在 C 和 c + + 語言中，零整數或浮點值或 null 指標可以轉換成布林值`false`，而非零整數或浮點值或為非 null 指標可以轉換成布林值`true`。 在 C# 中，藉由明確地比較的整數或浮點數的值為零，或藉由明確地比較的物件參考，會完成這類轉換`null`。

### <a name="enumeration-types"></a>列舉類型

列舉型別是具名常數的不同類型。 每個列舉類型都具有基礎類型，必須是`byte`， `sbyte`， `short`， `ushort`， `int`， `uint`，`long`或`ulong`。 一組值的列舉型別是一組與基礎型別的值相同。 列舉類型的值不限於具名常數的值。 列舉型別會定義透過列舉型別宣告 ([列舉宣告](enums.md#enum-declarations))。

### <a name="nullable-types"></a>可為 Null 的型別

可為 null 的型別可以代表所有值及其***基礎型別***再加上額外的 null 值。 可為 null 的型別會撰寫`T?`，其中`T`為基礎的類型。 此語法是速記`System.Nullable<T>`，兩種形式可以交替使用。

A***不可為 null 的實值型別***相反的是任何實值型別`System.Nullable<T>`和其簡短`T?`(針對任何`T`)，再加上任何已限制為不可為 null 的實值型別 （也就是任何的型別參數型別參數使用`struct`條件約束)。 `System.Nullable<T>`型別指定的值類型條件約束`T`([類型參數條件約束](classes.md#type-parameter-constraints))，這表示可為 null 的型別的基礎型別可以是任何非可為 null 的實值類型。 可為 null 的型別的基礎型別不能為 null 的類型或參考型別。 例如，`int??`和`string?`都是無效的型別。

可為 null 型別的執行個體`T?`有兩個公用唯讀屬性：

*  A`HasValue`類型屬性 `bool`
*  A`Value`類型屬性 `T`

執行個體`HasValue`為 true 即為非 null。 非 null 執行個體包含已知的值和`Value`傳回該值。

執行個體`HasValue`是 false 」 即為 null。 Null 執行個體有未定義的值。 嘗試讀取`Value`的 null 執行個體，會導致`System.InvalidOperationException`擲回。 存取的程序`Value`可為 null 的執行個體的屬性指***解除包裝***。

除了預設建構函式，每個可為 null 的型別`T?`的公用建構函式接受單一引數的型別`T`。 指定值`x`型別的`T`，表單的建構函式引動過程

```csharp
new T?(x)
```
建立的非 null 執行個體`T?`為其`Value`屬性是`x`。 針對指定的值就是建立可為 null 類型的非 null 執行個體的程序***包裝***。

隱含轉換都是從`null`常值，以便`T?`([Null 常值轉換](conversions.md#null-literal-conversions)) 以及從`T`來`T?`([隱含的 null 轉換](conversions.md#implicit-nullable-conversions)).

## <a name="reference-types"></a>參考型別

參考型別是類別類型、 介面類型、 陣列類型或委派型別。

```antlr
reference_type
    : class_type
    | interface_type
    | array_type
    | delegate_type
    ;

class_type
    : type_name
    | 'object'
    | 'dynamic'
    | 'string'
    ;

interface_type
    : type_name
    ;

array_type
    : non_array_type rank_specifier+
    ;

non_array_type
    : type
    ;

rank_specifier
    : '[' dim_separator* ']'
    ;

dim_separator
    : ','
    ;

delegate_type
    : type_name
    ;
```

參考型別值是參考***執行個體***型別，而後者就稱為***物件***。 特殊值`null`適用於所有參考型別，並指出執行個體不存在。

### <a name="class-types"></a>類別型別

類別型別定義資料結構，其中包含資料成員 （常數和欄位），函式成員 （方法、 屬性、 事件、 索引子、 運算子、 執行個體建構函式、 解構函式和靜態建構函式） 和巢狀型別。 類別類型支援繼承，藉此延伸及特製化的基底類別衍生的類別的機制。 使用建立類別類型的執行個體*object_creation_expression*s ([物件建立運算式](expressions.md#object-creation-expressions))。

類別型別所述[類別](classes.md)。

下表中所述，某些預先定義的類別類型會有在 C# 語言中，特殊的意義。


| __類別類型__     | __描述__                                         |
|--------------------|---------------------------------------------------------|
| `System.Object`    | 所有其他類型的 ultimate 基底類別。 請參閱[的物件型別](types.md#the-object-type)。 | 
| `System.String`    | C# 語言的字串型別。 請參閱[string 型別](types.md#the-string-type)。         |
| `System.ValueType` | 所有實值型別的基底類別。 請參閱[System.ValueType 類型](types.md#the-systemvaluetype-type)。          |
| `System.Enum`      | 所有列舉類型的基底類別。 請參閱[列舉](enums.md)。              |
| `System.Array`     | 所有陣列類型的基底類別。 請參閱[陣列](arrays.md)。             |
| `System.Delegate`  | 所有的委派類型的基底類別。 請參閱[委派](delegates.md)。          |
| `System.Exception` | 所有的例外狀況類型的基底類別。 請參閱[例外狀況](exceptions.md)。         |

### <a name="the-object-type"></a>物件類型

`object`類別類型是所有其他類型的 ultimate 基底類別。 在 C# 中的每個類型直接或間接衍生自`object`類別型別。

關鍵字`object`是只要預先定義的類別別名`System.Object`。

### <a name="the-dynamic-type"></a>動態類型

`dynamic`類型，如`object`，可以參考任何物件。 當運算子會套用至類型的運算式`dynamic`，及其解決方式延後，直到執行程式。 因此，如果合法無法將運算子套用至參考的物件，沒有錯誤可以在編譯期間。 改為在執行階段的運算子的解析失敗時，例外狀況就會擲回。

其目的是要允許動態繫結，在詳細資料中所述[動態繫結](expressions.md#dynamic-binding)。

`dynamic` 會視為相同`object`除了在下列方面：

*  類型的運算式上的作業`dynamic`可以動態地繫結 ([動態繫結](expressions.md#dynamic-binding))。
*  型別推斷 ([型別推斷](expressions.md#type-inference)) 會偏好`dynamic`透過`object`兩者都是候選項目。

因為此是否相等，下列的保留：

*  之間隱含的身分識別轉換`object`並`dynamic`，並取代時，都是相同的建構型別之間`dynamic`與 `object`
*  隱含和明確轉換，來回`object`也適用於與`dynamic`。
*  取代時，都是相同的方法簽章`dynamic`與`object`會被視為相同的簽章
*  型別`dynamic`區別`object`在執行階段。
*  類型的運算式`dynamic`指***動態運算式***。

### <a name="the-string-type"></a>String 型別

`string`型別是密封的類別型別直接繼承自`object`。 執行個體`string`類別代表 Unicode 字元字串。

值`string`型別可以寫成字串常值 ([字串常值](lexical-structure.md#string-literals))。

關鍵字`string`是只要預先定義的類別別名`System.String`。

### <a name="interface-types"></a>介面型別

介面定義合約。 類別或結構實作介面必須遵守其合約。 介面可以繼承自多個基底介面，以及類別或結構可以實作多個介面。

介面型別所述[介面](interfaces.md)。

### <a name="array-types"></a>陣列型別

陣列是資料結構，其中包含零個或多個變數是透過計算索引存取。 陣列，也稱為陣列的項目中包含的變數都相同的型別，以及這個型別稱為陣列的項目類型。

陣列型別所述[陣列](arrays.md)。

### <a name="delegate-types"></a>委派型別

委派是參考一或多個方法的資料結構。 執行個體方法，它也是指其對應的物件執行個體。

最接近對等項目，在 C 或 c + + 中委派的函式指標，但函式指標只能參考靜態函式，而委派可以參考這兩個靜態和執行個體方法。 在後者的情況下，委派會儲存參考至方法的進入點，不僅要叫用方法的物件執行個體的參考。

委派型別所述[委派](delegates.md)。

## <a name="boxing-and-unboxing"></a>Boxing 和 Unboxing

Boxing 和 unboxing 的概念是 C# 類型系統的核心。 它提供之間的橋樑*value_type*s 並*reference_type*s 所允許的任何值*value_type*型別來回轉換`object`。 Boxing 和 unboxing 可讓其中任何類型的值可以最終會視為物件的型別系統的統一的檢視。

### <a name="boxing-conversions"></a>Boxing 轉換

Boxing 轉換允許*value_type*隱含地轉換成*reference_type*。 下列的 boxing 轉換存在：

*  從任何*value_type*型別`object`。
*  從任何*value_type*型別`System.ValueType`。
*  從任何*non_nullable_value_type*任何*interface_type*實作*value_type*。
*  從任何*nullable_type*任何*interface_type*的基礎類型所實作*nullable_type*。
*  從任何*enum_type*型別`System.Enum`。
*  從任何*nullable_type*與基礎*enum_type*類型`System.Enum`。
*  請注意，型別參數的隱含轉換將會執行 boxing 轉換為是否到最後在執行階段從實值型別轉換成參考型別 ([涉及型別參數的隱含轉換](conversions.md#implicit-conversions-involving-type-parameters))。

Box 處理的值*non_nullable_value_type*配置的物件執行個體，並複製所組成*non_nullable_value_type*到該執行個體的值。

Box 處理的值*nullable_type*會產生 null 參考，它是否`null`值 (`HasValue`是`false`)，或解除包裝成為和否則 boxing 基礎值的結果。

Box 處理的值的實際程序*non_nullable_value_type*最能說明想像的泛型存在***boxing 類別***，其行為就如同它宣告，如下所示：

```csharp
sealed class Box<T>: System.ValueType
{
    T value;

    public Box(T t) {
        value = t;
    }
}
```

值的 boxing`v`型別的`T`現在包含執行運算式`new Box<T>(v)`，並傳回產生的執行個體類型的值為`object`。 因此，這些陳述式
```csharp
int i = 123;
object box = i;
```
在概念上對應至
```csharp
int i = 123;
object box = new Box<int>(i);
```

Boxing 類別，例如`Box<T>`以上版本並不實際存在和動態的 boxed 實值類型不是實際的類別類型。 相反地，類型的 boxed 的值`T`具有動態類型`T`，並使用動態型別檢查`is`運算子只會參考型別`T`。 例如，套用至物件的
```csharp
int i = 123;
object box = i;
if (box is int) {
    Console.Write("Box contains an int");
}
```
會輸出字串"`Box contains an int`」 在主控台上。

Boxing 轉換表示的值進行 box 處理的複本。 這與不同的轉換成*reference_type*鍵入`object`中的值仍然會參考相同的執行個體只會被視為較少衍生的型別`object`。 例如，假設宣告
```csharp
struct Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
下列陳述式
```csharp
Point p = new Point(10, 10);
object box = p;
p.x = 20;
Console.Write(((Point)box).x);
```
將會輸出主控台上的值 10，因為中的指派，就會發生隱含 boxing 作業`p`要`box`box 的值`p`複製。 有`Point`已宣告`class`相反地，值 20 就是輸出，因為`p`和`box`會參考相同的執行個體。

### <a name="unboxing-conversions"></a>Unboxing 轉換

Unboxing 轉換允許*reference_type*明確轉換成*value_type*。 下列 unboxing 轉換存在：

*  從型別`object`任何*value_type*。
*  從型別`System.ValueType`任何*value_type*。
*  從任何*interface_type*任何*non_nullable_value_type*實作*interface_type*。
*  從任何*interface_type*任何*nullable_type*其基礎類型會實作*interface_type*。
*  從型別`System.Enum`任何*enum_type*。
*  從型別`System.Enum`任何*nullable_type*與基礎*enum_type*。
*  請注意 unboxing 轉換為會執行，類型參數的明確轉換，是否在執行階段，它最終會從參考型別轉換成實值型別 ([明確的動態轉換](conversions.md#explicit-dynamic-conversions))。

Unboxing 作業，以*non_nullable_value_type*所組成的物件執行個體 boxed 的值的第一個檢查給定*non_nullable_value_type*，然後將複製的值執行個體。

以 unboxing *nullable_type*產生 null 值的*nullable_type*來源運算元是否`null`，或包裝的結果 unboxing 的基礎類型的物件執行個體*nullable_type*否則。

參考前一個區段中，unboxing 轉換的物件中所述的虛數的 boxing 類別`box`要*value_type* `T`組成執行運算式`((Box<T>)box).value`。 因此，這些陳述式
```csharp
object box = 123;
int i = (int)box;
```
在概念上對應至
```csharp
object box = new Box<int>(123);
int i = ((Box<int>)box).value;
```

Unboxing 轉換至給定*non_nullable_value_type*若要在執行階段成功，來源運算元的值必須是參考的 boxed 值*non_nullable_value_type*。 如果來源運算元`null`、`System.NullReferenceException`就會擲回。 如果來源運算元是不相容的物件，參考`System.InvalidCastException`就會擲回。

Unboxing 轉換至給定*nullable_type*若要在執行階段成功，來源運算元的值必須是`null`或 boxed 值的基礎參考*non_nullable_value_type*的*nullable_type*。 如果來源運算元是不相容的物件，參考`System.InvalidCastException`就會擲回。

## <a name="constructed-types"></a>建構的類型

泛型型別宣告，單獨使用時，表示***未繫結的泛型型別***做為 「 藍圖 以形成許多不同類型的詳細資訊，藉由套用***型別引數***。 型別引數撰寫在角括弧 (`<`和`>`) 緊接的泛型型別名稱。 包含至少一個類型引數的型別稱為***建構的型別***。 建構的類型可用以型別名稱可以出現的語言中的大部分位置中。 未繫結的泛型類型只可用於*typeof_expression* ([typeof 運算子](expressions.md#the-typeof-operator))。

建構的類型也可用在運算式中做為簡單名稱 ([簡單名稱](expressions.md#simple-names)) 或存取成員時 ([成員存取](expressions.md#member-access))。

當*namespace_or_type_name*是型別參數會被視為正確數目的已評估、 僅泛用型別。 因此，很可能只要類型有不同數量的型別參數，來識別不同的類型，使用相同的識別項。 混合相同程式中的泛型和非泛型類別時，這很有用：

```csharp
namespace Widgets
{
    class Queue {...}
    class Queue<TElement> {...}
}

namespace MyApplication
{
    using Widgets;

    class X
    {
        Queue q1;            // Non-generic Widgets.Queue
        Queue<int> q2;       // Generic Widgets.Queue
    }
}
```

A *type_name*可能會識別建構的類型，即使它不會直接指定型別參數。 這可能會發生在泛型類別宣告中，巢狀類型，包含宣告的執行個體類型隱含地用於名稱查閱 ([巢狀在泛型類別的型別](classes.md#nested-types-in-generic-classes)):

```csharp
class Outer<T>
{
    public class Inner {...}

    public Inner i;                // Type of i is Outer<T>.Inner
}
```

在不安全的程式碼中建構的類型不能做*unmanaged_type* ([指標類型](unsafe-code.md#pointer-types))。

### <a name="type-arguments"></a>型別引數

每個引數型別引數清單中的只是單純*型別*。

```antlr
type_argument_list
    : '<' type_arguments '>'
    ;

type_arguments
    : type_argument (',' type_argument)*
    ;

type_argument
    : type
    ;
```

Unsafe 程式碼中 ([Unsafe 程式碼](unsafe-code.md))，則*type_argument*可能不是指標類型。 每個類型引數必須滿足任何條件約束對應型別參數 ([類型參數條件約束](classes.md#type-parameter-constraints))。

### <a name="open-and-closed-types"></a>開放和封閉類型

所有型別可以分類為 「***開啟類型***或是***封閉類型***。 開啟型別是包含型別參數的類型。 具體而言：

*  型別參數會定義開啟型別。
*  陣列型別是開放式類型，才其項目型別是開放式類型。
*  建構的型別是開放式類型，才一或多個其型別引數是開啟型別。 建構的巢狀型別是開放式類型，才一或多個其型別引數或其包含類型的類型引數是開啟型別。

封閉式的型別是不是開啟類型的類型。

在執行階段，所有的泛型類型宣告中的程式碼是藉由套用至泛型宣告的型別引數建立封閉式建構類型的內容中執行。 在泛型型別中的每個類型參數是繫結至特定的執行階段型別。 執行階段處理的所有陳述式及運算式永遠發生在封閉式的型別，並開啟類型只會在發生編譯時間處理。

每個封閉式建構的類型都有它自己組不會與任何其他封閉式建構類型共用的靜態變數。 由於開啟的型別不存在於執行階段中，有任何開放式型別相關聯的靜態變數。 兩個封閉式的建構型別都是相同類型，如果它們從相同的繫結泛型型別，建構，而且其對應的型別引數都是相同的型別。

### <a name="bound-and-unbound-types"></a>繫結和解除繫結類型

詞彙***未繫結的型別***是指非泛型類型或繫結的泛型型別。 詞彙***繫結型別***是指非泛型類型或建構的型別。

未繫結的類型是指型別宣告所宣告的實體。 未繫結的泛型類型本身不是類型，並不能做為類型的變數、 引數或傳回值，或做為基底類型。 唯一的建構可以參考繫結的泛型型別是`typeof`運算式 ([typeof 運算子](expressions.md#the-typeof-operator))。

### <a name="satisfying-constraints"></a>滿足的條件約束

針對泛型類型或方法上宣告的型別參數條件約束時，參考建構的類型或泛型方法，會檢查提供的型別引數 ([類型參數條件約束](classes.md#type-parameter-constraints))。 每個`where`子句中，型別引數`A`，其對應至名為型別參數根據檢查每個條件約束，如下所示：

*  如果類別型別、 介面型別或型別參數條件約束，可讓`C`代表使用提供的型別引數的條件約束取代任何出現在條件約束的型別參數。 若要滿足的條件約束，它必須的情況下，鍵入`A`可以轉換成類型`C`由下列其中之一：
    * 身分識別轉換 ([身分識別轉換](conversions.md#identity-conversion))
    * 隱含參考轉換 ([隱含參考轉換](conversions.md#implicit-reference-conversions))
    * Boxing 轉換 ([Boxing 轉換](conversions.md#boxing-conversions))，前提是型別 A 是不可為 null 的實值型別。
    * 從型別參數隱含參考、 boxing 或類型參數轉換`A`至`C`。
*  如果條件約束參考類型條件約束 (`class`)，型別`A`必須符合下列其中之一：
    * `A` 是介面類型、 類別類型、 委派型別或陣列型別。 請注意，`System.ValueType`和`System.Enum`是參考型別符合此條件約束。
    * `A` 已知為參考型別為類型參數 ([類型參數條件約束](classes.md#type-parameter-constraints))。
*  如果條件約束的值類型條件約束 (`struct`)，型別`A`必須符合下列其中之一：
    * `A` 為結構類型或列舉型別，但不是可為 null 的型別。 請注意，`System.ValueType`和`System.Enum`是參考型別不符合此條件約束。
    * `A` 是具有實值類型條件約束的型別參數 ([類型參數條件約束](classes.md#type-parameter-constraints))。
*  如果條件約束是建構函式的條件約束`new()`，型別`A`不得`abstract`，而且必須具有公用的無參數建構函式。 這被符合下列其中一項，則為 true 時：
    * `A` 是實值類型，因為所有的值類型都有一個公用預設建構函式 ([預設建構函式](types.md#default-constructors))。
    * `A` 是具有建構函式條件約束的型別參數 ([類型參數條件約束](classes.md#type-parameter-constraints))。
    * `A` 是具有實值類型條件約束的型別參數 ([類型參數條件約束](classes.md#type-parameter-constraints))。
    * `A` 是不是類別`abstract`且包含明確宣告`public`不含任何參數的建構函式。
    * `A` 不是`abstract`且具有預設建構函式 ([預設建構函式](classes.md#default-constructors))。

如果指定的型別引數不符合一或多個類型參數的條件約束，就會發生編譯時期錯誤。

因為不繼承型別參數條件約束也不可能繼承而來。 在下列範例中，`D`必須指定其類型參數條件約束`T`以便`T`滿足條件約束的基底類別所加諸`B<T>`。 相反地，類別`E`因為，則不需要指定的條件約束`List<T>`實作`IEnumerable`任何`T`。

```csharp
class B<T> where T: IEnumerable {...}

class D<T>: B<T> where T: IEnumerable {...}

class E<T>: B<List<T>> {...}
```

## <a name="type-parameters"></a>型別參數

型別參數是指定實值類型或參考型別參數所繫結在執行階段的識別碼。

```antlr
type_parameter
    : identifier
    ;
```

類型參數可以使用許多不同的實際型別引數具現化，因為型別參數會有稍微不同的作業與比其他類型的限制。 它們包括：

*  型別參數不能直接以宣告的基底類別 ([基底類別](classes.md#base-class)) 或介面 ([Variant 類型參數清單](interfaces.md#variant-type-parameter-lists))。
*  參數取決於條件約束，如果有的話，成員查詢型別上的規則套用至型別參數。 它們會詳述[成員查閱](expressions.md#member-lookup)。
*  可用的轉換型別參數取決於條件約束，如果有的話，套用至型別參數。 它們會詳述[涉及型別參數的隱含轉換](conversions.md#implicit-conversions-involving-type-parameters)並[明確的動態轉換](conversions.md#explicit-dynamic-conversions)。
*  常值`null`無法轉換成指定型別參數，但如果已知型別參數是參考類型的類型 ([涉及型別參數的隱含轉換](conversions.md#implicit-conversions-involving-type-parameters))。 不過，`default`運算式 ([預設值運算式](expressions.md#default-value-expressions)) 可以使用。 此外，要有類型參數所指定類型的值與比較`null`使用`==`並`!=`([參考型別等號比較運算子](expressions.md#reference-type-equality-operators)) 除非型別參數具有實值類型條件約束。
*  A`new`運算式 ([物件建立運算式](expressions.md#object-creation-expressions)) 僅適用於具有型別參數如果類型參數受到*constructor_constraint*或實值類型條件約束 ([類型參數條件約束](classes.md#type-parameter-constraints))。
*  型別參數無法使用屬性中的任何位置。
*  型別參數不能在成員存取 ([成員存取](expressions.md#member-access)) 或型別名稱 ([命名空間和型別名稱](basic-concepts.md#namespace-and-type-names)) 來識別的靜態成員或巢狀型別。
*  在不安全的程式碼中的型別參數不能做*unmanaged_type* ([指標類型](unsafe-code.md#pointer-types))。

做為類型，類型參數都是單純的編譯時期建構。 在執行階段，每個類型參數是繫結至所提供的泛型型別宣告的類型引數指定的執行階段類型。 因此，在執行階段型別參數會以宣告的變數的型別是封閉式的建構的類型 ([開放和封閉類型](types.md#open-and-closed-types))。 執行階段執行的所有陳述式和包含型別參數的運算式會使用提供的實際型別為該參數的型別引數。

## <a name="expression-tree-types"></a>運算式樹狀架構類型

***運算式樹狀架構***允許 lambda 運算式表示為資料結構，而不是可執行程式碼。 運算式樹狀架構為值***運算式樹狀架構類型***的表單`System.Linq.Expressions.Expression<D>`，其中`D`是任何委派型別。 此規格的其餘部分中，我們會參考使用速記時這些類型`Expression<D>`。

如果有從 lambda 運算式的委派型別`D`，轉換也存在於運算式樹狀架構型別`Expression<D>`。 轉換成委派類型的 lambda 運算式會產生參考 lambda 運算式的可執行程式碼的委派，而轉換為運算式樹狀結構類型會建立 lambda 運算式的運算式樹狀結構表示法。

運算式樹狀架構是 lambda 運算式的高效率的記憶體內部資料表示法，以及讓透明且明確的 lambda 運算式的結構。

就像委派型別`D`，`Expression<D>`即具有參數和傳回型別，這是一樣的`D`。

下列範例代表 lambda 運算式作為可執行程式碼和運算式樹狀架構。 因為轉換成型`Func<int,int>`，也別轉換`Expression<Func<int,int>>`:

```csharp
Func<int,int> del = x => x + 1;                    // Code

Expression<Func<int,int>> exp = x => x + 1;        // Data
```

遵循這些指派，委派`del`參考方法會傳回`x + 1`，並將運算式樹狀架構`exp`參考的資料結構，描述運算式`x => x + 1`。

泛型類型的確切定義`Expression<D>`建構運算式樹狀架構 lambda 運算式轉換成運算式樹狀架構型別時的精確規則，以及都超出此規格的範圍。

兩件事是，您必須明確的：

*  並非所有的 lambda 運算式可以轉換成運算式樹狀架構。 比方說，無法表示 lambda 運算式與陳述式主體，並包含指派運算式的 lambda 運算式。 在這些情況下，轉換仍然存在，但在編譯時期將會失敗。 這些例外狀況的詳細[匿名函式轉換](conversions.md#anonymous-function-conversions)。
*   `Expression<D>` 提供執行個體方法`Compile`這會產生之型別的委派`D`:

    ```csharp
    Func<int,int> del2 = exp.Compile();
    ```

    叫用此委派會導致執行運算式樹狀架構所表示的程式碼。 因此，假設上述的定義，del 和 del2 相等，然後下列兩個陳述式將會有相同的效果：

    ```csharp
    int i1 = del(1);
    
    int i2 = del2(1);
    ```

    在執行此程式碼之後,`i1`並`i2`都會有值`2`。

