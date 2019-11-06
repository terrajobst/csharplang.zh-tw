---
ms.openlocfilehash: 088c4a77cecde490c556c44c239a3496f896582e
ms.sourcegitcommit: 4ddf18d000734c1b6d0a48127bf338086fc3f2c3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2019
ms.locfileid: "73616132"
---
# <a name="types"></a>型別

C#語言的類型分成兩個主要類別：實***數值型別***和***參考型別***。 實數值型別和參考型別都可以是***泛型型別***，這會採用一或多個***類型參數***。 類型參數可以同時指定實數值型別和參考型別。

```antlr
type
    : value_type
    | reference_type
    | type_parameter
    | type_unsafe
    ;
```

類型、指標的最終分類僅適用于 unsafe 程式碼。 這會在[指標類型](unsafe-code.md#pointer-types)中進一步討論。

實值型別與參考型別不同的是，實值型別的變數會直接包含其資料，而參考型別的變數則會儲存其資料的***參考***，後者又稱為***物件***。 有了參考型別，兩個變數都可以參考相同的物件，因此在某個變數上的作業可能會影響另一個變數所參考的物件。 使用實值型別時，每個變數都有自己的資料複本，而且其中一個的作業不可能影響另一個。

C#的類型系統是統一的，因此可以將任何類型的值視為物件。 C# 中的每個型別都直接或間接衍生自 `object` 類別型別，而 `object` 是所有型別的基底類別。 參考型別的值之所以會視為物件，只是將這些值當作 `object` 型別來檢視。 實數值型別的值會藉由執行裝箱和取消裝箱作業（[裝箱和取消](types.md#boxing-and-unboxing)裝箱）來視為物件。

## <a name="value-types"></a>值類型

實值型別可以是結構型別或列舉型別。 C#提供一組稱為***簡單類型***的預先定義結構類型。 簡單類型是透過保留字來識別。

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

不同于參考型別的變數，實數值型別的變數只有在實數值型別是可為 null 的類型時，才可以包含值 `null`。  針對每一個不可為 null 的實數值型別，有一個對應的可為 null 實數值型別，表示相同的值集加上 `null`的值。

指派至實數值型別的變數會建立所指派值的複本。 這與指派給參考型別的變數不同，後者會複製參考，而不是參考所識別的物件。

### <a name="the-systemvaluetype-type"></a>System.object 類型

所有實值型別都會隱含繼承自類別 `System.ValueType`，而這又繼承自類別 `object`。 任何類型都無法衍生自實數值型別，而實數值型別因此隱含密封（[密封類別](classes.md#sealed-classes)）。

請注意，`System.ValueType` 本身並不是*value_type*。 相反地，它是自動衍生所有*value_type*的*class_type* 。

### <a name="default-constructors"></a>預設建構函式

所有實值型別會隱含地宣告一個稱為***預設***的函式的公用無參數實例的函數。 預設的函式會傳回零初始化的實例，稱為實數值型別的***預設值***：

*  針對所有*simple_type*s，預設值為所有零的位模式所產生的值：
    * 針對 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`和 `ulong`，預設值為 [`0`]。
    * 針對 `char`，預設值為 `'\x0000'`。
    * 針對 `float`，預設值為 `0.0f`。
    * 針對 `double`，預設值為 `0.0d`。
    * 針對 `decimal`，預設值為 `0.0m`。
    * 針對 `bool`，預設值為 `false`。
*  針對*enum_type* `E`，預設值為 `0`，轉換為類型 `E`。
*  對於*struct_type*，預設值是將所有實值型別字段設定為其預設值，以及要 `null`的所有參考型別字段所產生的值。
*  對於*nullable_type* ，預設值是 `HasValue` 屬性為 false 且未定義 `Value` 屬性的實例。 預設值也稱為可為 null 類型的***null 值***。

就像任何其他實例的函式一樣，實值型別的預設的函數會使用 `new` 運算子來叫用。 基於效率的考慮，這項需求並不是要實際讓執行產生一個函式呼叫。 在下列範例中，`i` 和 `j` 的變數都會初始化為零。

```csharp
class A
{
    void F() {
        int i = 0;
        int j = new int();
    }
}
```

由於每個實數值型別都隱含具有公用無參數實例的函式，因此結構類型不可能包含無參數之函數的明確宣告。 不過，結構類型允許宣告參數化實例的函式[（函](structs.md#constructors)式）。

### <a name="struct-types"></a>結構型別

結構類型是實數值型別，可以宣告常數、欄位、方法、屬性、索引子、運算子、實例的函式、靜態的函式和巢狀型別。 結構類型的宣告會在[結構](structs.md#struct-declarations)宣告中說明。

### <a name="simple-types"></a>簡單型別

C#提供一組稱為***簡單類型***的預先定義結構類型。 簡單類型是透過保留字來識別，但這些保留字只是 `System` 命名空間中預先定義結構類型的別名，如下表所述。


| __保留字__ | __別名類型__ |
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

因為簡單型別會將結構型別做為別名，所以每個簡單型別都有成員。 例如，`int` 具有在 `System.Int32` 中宣告的成員，以及繼承自 `System.Object`的成員，且允許下列語句：

```csharp
int i = int.MaxValue;           // System.Int32.MaxValue constant
string s = i.ToString();        // System.Int32.ToString() instance method
string t = 123.ToString();      // System.Int32.ToString() instance method
```

簡單型別與其他結構型別的差異在於它們可允許某些額外的作業：

*  大部分的簡單類型都允許藉由撰寫*常*值（[常](lexical-structure.md#literals)值）來建立值。 例如，`123` 是 `int` 類型的常值，而 `'a'` 是 `char`類型的常值。 C#一般不會布建結構類型的常值，而且其他結構類型的非預設值最後一律會透過這些結構類型的實例構造函式來建立。
*  當運算式的運算元都是簡單類型常數時，編譯器可以在編譯時期評估運算式。 這類運算式稱為*constant_expression* （[常數運算式](expressions.md#constant-expressions)）。 涉及其他結構類型所定義之運算子的運算式，不會被視為常數運算式。
*  透過 `const` 宣告，可以宣告簡單類型（[常數](classes.md#constants)）的常數。 不可能有其他結構類型的常數，但 `static readonly` 欄位會提供類似的效果。
*  包含簡單類型的轉換可以參與評估其他結構類型所定義的轉換運算子，但使用者定義的轉換運算子不能參與其他使用者定義運算子的評估（[評估使用者定義的轉換](conversions.md#evaluation-of-user-defined-conversions)）。

### <a name="integral-types"></a>整數類資料型別

C#支援九種整數類型： `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`和 `char`。 整數類資料類型具有下列大小和範圍的值：

*  `sbyte` 類型代表帶正負號的8位整數，其值介於-128 和127之間。
*  `byte` 類型代表不帶正負號的8位整數，其值介於0到255之間。
*  `short` 類型代表帶正負號的16位整數，其值介於-32768 和32767之間。
*  `ushort` 類型代表不帶正負號的16位整數，其值介於0到65535之間。
*  `int` 類型代表帶正負號的32位整數，其值介於-2147483648 與2147483647之間。
*  `uint` 類型代表不帶正負號的32位整數，其值介於0到4294967295之間。
*  `long` 類型代表帶正負號的64位整數，其值介於-9223372036854775808 到9223372036854775807之間。
*  `ulong` 類型代表不帶正負號的64位整數，其值介於0到18446744073709551615之間。
*  `char` 類型代表不帶正負號的16位整數，其值介於0到65535之間。 `char` 類型的可能值集合會對應到 Unicode 字元集。 雖然 `char` 與 `ushort`有相同的標記法，但不允許在另一個類型上允許所有作業。

整數類型的一元和二元運算子一律會使用帶正負號的32位有效位數、不帶正負號的32位精確度、帶正負號的64位有效位數或不帶正負號的64位有效位數來運作：

*  對於一元 `+` 和 `~` 運算子而言，運算元會轉換成類型 `T`，其中 `T` 是 `int`、`uint`、`long`和 `ulong` 的第一個，可以完全表示運算元的所有可能值。 接著會使用 `T`類型的有效位數來執行作業，並 `T`結果的類型。
*  針對一元 `-` 運算子，運算元會轉換成類型 `T`，其中 `T` 是可以完全代表運算元所有可能值的 `int` 和 `long` 的第一個。 接著會使用 `T`類型的有效位數來執行作業，並 `T`結果的類型。 一元 `-` 運算子不能套用至 `ulong`類型的運算元。
*  針對二進位 `+`，`-`、`*`、`/`、`%`、`&`、`^`、`|`、`==`、`!=`、`>`、`<`、`>=`和 `<=` 運算子會將運算元轉換成類型 `T`，其中 `T` 是 `int`、`uint`、`long`和 `ulong` 的第一個，可以完全表示這兩個運算元的所有可能值。 然後，會使用 `T`類型的有效位數來執行作業，而結果的類型會是 `T` （或關聯式運算子的 `bool`）。 一個運算元不允許 `long` 類型，另一個則屬於 `ulong` 與二元運算子的類型。
*  針對二元 `<<` 和 `>>` 運算子，左運算元會轉換成類型 `T`，其中 `T` 是 `int`、`uint`、`long`和 `ulong` 的第一個，可以完全表示運算元的所有可能值。 接著會使用 `T`類型的有效位數來執行作業，並 `T`結果的類型。

`char` 類型會分類為整數類資料類型，但它與其他整數類型不同，有兩種方式：

*  沒有從其他類型到 `char` 類型的隱含轉換。 特別的是，即使 `sbyte`、`byte`和 `ushort` 類型的值範圍，都可以使用 `char` 類型來完整顯示，但是從 `sbyte`、`byte`或 `ushort` 到 `char` 的隱含轉換並不存在。
*  `char` 類型的常數必須寫成*character_literal*s 或*integer_literal*s，並結合轉換成類型 `char`。 例如，`(char)10` 與 `'\x000A'` 相同。

`checked` 和 `unchecked` 運算子和語句是用來控制整數型別算數運算和轉換（[checked 和 unchecked 運算子](expressions.md#the-checked-and-unchecked-operators)）的溢位檢查。 在 `checked` 內容中，溢位會產生編譯時期錯誤，或導致 `System.OverflowException` 擲回。 在 `unchecked` 內容中，會忽略溢位，而且不會捨棄任何不符合目的地類型的高序位位。

### <a name="floating-point-types"></a>浮點類型

C#支援兩種浮點類型： `float` 和 `double`。 `float` 和 `double` 類型會使用32位單精確度和64位雙精確度 IEEE 754 格式來表示，這會提供下列集合的值：

*  正零和負零。 在大部分情況下，正零和負零的行為與簡單值零相同，但是某些作業會區分兩個（[除法運算子](expressions.md#division-operator)）。
*  正無限大和負無限大。 無限大是由這類作業所產生，會將非零的數位零除。 例如，`1.0 / 0.0` 產生正無限大，`-1.0 / 0.0` 產生負無限大。
*  ***非數位***值，通常是 NaN 縮寫。 Nan 是由不正確浮點運算所產生，例如零除零。
*  表單 `s * m * 2^e`的有限非零值集合，其中 `s` 為1或-1，而 `m` 和 `e` 是由特定的浮點類型所決定： `float`、`0 < m < 2^24` 和 `-149 <= e <= 104``double`、`0 < m < 2^53` 和 `-1075 <= e <= 970`的、和。 不正規化的浮點數會被視為有效的非零值。

`float` 類型可以代表範圍從大約 `1.5 * 10^-45` 到 `3.4 * 10^38`，精確度為7位數的值。

`double` 類型可以代表範圍從大約 `5.0 * 10^-324` 到 `1.7 × 10^308`，精確度為15-16 位數的值。

如果二元運算子的其中一個運算元屬於浮點類型，則另一個運算元必須是整數類資料類型或浮點類型，而運算的評估方式如下：

*  如果其中一個運算元是整數類資料類型，則該運算元會轉換成另一個運算元的浮點類型。
*  然後，如果任一個運算元的類型是 `double`，另一個運算元會轉換成 `double`，則會使用至少 `double` 範圍和有效位數來執行作業，而結果的類型會是 `double` （或 `bool` 關係運算子）。
*  否則，會使用至少 `float` 的範圍和精確度來執行作業，而結果的類型會是 `float` （或關聯式運算子的 `bool`）。

浮點運算子（包括指派運算子）永遠不會產生例外狀況。 相反地，在例外狀況下，浮點運算會產生零、無限大或 NaN，如下所述：

*  如果浮點運算的結果太小而無法用於目的地格式，作業的結果就會變成正零或負零。
*  如果浮點運算的結果對目的地格式而言太大，則作業的結果會變成正無限大或負無限大。
*  如果浮點運算無效，作業的結果就會變成 NaN。
*  如果浮點運算的一個或兩個運算元都是 NaN，則作業的結果會變成 NaN。

執行浮點運算時，精確度可能會高於作業的結果類型。 例如，某些硬體架構支援「延伸」或「長雙精度」浮點類型，其範圍和有效位數大於 `double` 類型，並使用這個較高的精確度類型來隱含執行所有浮點運算。 只有在效能過高的情況下，才能夠以較低的精確度執行浮點運算，而不需要實作為要略過效能和精確度的實體系， C#而是允許更高的精確度類型用於所有浮點運算。 除了提供更精確的結果，這種情況很少會有任何明顯的影響。 不過，在表單 `x * y / z`的運算式中，乘法會產生超出 `double` 範圍的結果，但後續的除法會將暫存結果重新帶入 `double` 範圍，這是運算式在中評估的事實。較高的範圍格式可能會導致產生有限的結果，而不是無限大。

### <a name="the-decimal-type"></a>Decimal 類型

`decimal` 型別是 128 位元的資料型別，適合財務和貨幣計算。 `decimal` 類型可以代表範圍從 `1.0 * 10^-28` 到大約 `7.9 * 10^28` 28-29 個有效位數的值。

`decimal` 類型的一組有限值的格式為 `(-1)^s * c * 10^-e`，其中的正負號 `s` 為0或1，`c` 是由 `0 <= *c* < 2^96`指定，而尺規 `e` 則是 `0 <= e <= 28`。`decimal` 類型不支援帶正負號的零、無限大或 NaN。 `decimal` 是以10的乘冪來表示的96位整數。 針對絕對值小於 `1.0m`的 `decimal`s，此值會精確到28個小數位數，但不會再進一步。 針對絕對值大於或等於 `1.0m`的 `decimal`s，此值會精確為28或29個數字。 相反于 `float` 和 `double` 的資料類型，十進位的小數數位，例如0.1，可以完全在 `decimal` 標記法中表示。 在 `float` 和 `double` 標記法中，這類數位通常是無限的分數，因此這些標記法更容易發生舍入錯誤。

如果二元運算子的其中一個運算元屬於 `decimal`類型，則另一個運算元必須是整數類型或類型 `decimal`。 如果整數類型運算元存在，則會先將它轉換成 `decimal`，然後才執行作業。

`decimal` 類型值上的運算結果是，這會因計算確切的結果（根據每個運算子定義的保留尺規）而產生，然後四捨五入以符合標記法。 結果會舍入到最接近的可顯示值，而且當結果同樣接近兩個可顯示的值時，就是在最小有效位數位置（也就是「四進位」）中具有偶數數值的值。 零的結果一律具有0的正負號，而小數值為0。

如果十進位算數運算產生的值小於或等於 `5 * 10^-29` 以絕對值表示，則作業的結果會變成零。 如果 `decimal` 算數運算產生的結果對 `decimal` 格式而言太大，則會擲回 `System.OverflowException`。

`decimal` 類型的精確度較高，但範圍比浮點類型更小。 因此，從浮點類型到 `decimal` 的轉換可能會產生溢位例外狀況，而從 `decimal` 到浮點類型的轉換可能會導致失去精確度。 基於這些理由，浮點類型與 `decimal`之間不存在隱含轉換，而且沒有明確轉換，也無法在同一個運算式中混合使用浮點和 `decimal` 運算元。

### <a name="the-bool-type"></a>Bool 類型

`bool` 類型代表布林邏輯數量。 `bool` 類型的可能值為 `true` 和 `false`。

`bool` 和其他類型之間不存在標準轉換。 特別是，`bool` 類型是不同的，而且與整數類資料類型不同，而且不能使用 `bool` 值來取代整數值，反之亦然。

在 C 和C++語言中，零整數或浮點數，或 null 指標可以轉換成布林值 `false`和非零整數或浮點數，或非 null 指標可以轉換成布林值 `true`）。 在C#中，這類轉換會藉由明確地將整數或浮點數與零進行比較，或明確地比較物件參考與 `null`來完成。

### <a name="enumeration-types"></a>列舉類型

列舉類型是具有已命名常數的相異類型。 每個列舉類型都具有基礎類型，必須是 `byte`、`sbyte`、`short`、`ushort`、`int`、`uint`、`long` 或 `ulong`。 列舉類型的值集合與基礎類型的一組值相同。 列舉類型的值不限於已命名常數的值。 列舉類型是透過列舉宣告（[列舉](enums.md#enum-declarations)宣告）來定義。

### <a name="nullable-types"></a>可為 Null 的型別

可為 null 的型別可以代表其***基礎類型***的所有值加上額外的 null 值。 可為 null 的型別會 `T?`寫入，其中 `T` 是基礎型別。 此語法是 `System.Nullable<T>`的縮寫，而這兩種形式可以交換使用。

相反地，***不可為 null 的實值型***別就是除了 `System.Nullable<T>` 和其速記 `T?` （適用于任何 `T`）以外的任何實值型別，加上任何限制為不可為 null 的實值型別的型別參數（也就是具有 `struct` 的任何型別參數條件約束）。 `System.Nullable<T>` 型別會指定 `T` （[型別參數條件約束](classes.md#type-parameter-constraints)）的實值型別條件約束，這表示可為 null 型別的基礎型別可以是任何不可為 null 的實值型別。 可為 null 類型的基礎類型不能是可為 null 的類型或參考型別。 例如，`int??` 和 `string?` 是不正確類型。

可為 null 的型別 `T?` 的實例有兩個公用唯讀屬性：

*  類型的 `HasValue` 屬性 `bool`
*  類型的 `Value` 屬性 `T`

`HasValue` 為 true 的實例稱為非 null。 非 null 實例包含已知的值，`Value` 會傳回該值。

`HasValue` 為 false 的實例稱為 null。 Null 實例有未定義的值。 嘗試讀取 null 實例的 `Value` 會導致擲回 `System.InvalidOperationException`。 存取可為 null 之實例之 `Value` 屬性的程式稱為解除***包裝***。

除了預設的函式，每個可為 null 的型別 `T?` 都具有公用的函式，該函式會接受 `T`類型的單一引數。 假設 `T`類型的值 `x`，則為格式的函式呼叫

```csharp
new T?(x)
```
建立 `T?` 的非 null 實例，其 `Value` 屬性會 `x`。 針對指定的值建立可為 null 之型別的非 null 實例的程式稱為「***包裝***」。

隱含轉換可從 `null` 常值取得，以 `T?` （[Null 常值轉換](conversions.md#null-literal-conversions)），以及從 `T` 到 `T?` （[隱含可為 null 的轉換](conversions.md#implicit-nullable-conversions)）。

## <a name="reference-types"></a>參考型別

參考型別是類別類型、介面類別型、陣列類型或委派類型。

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

參考型別值是類型***實例***的參考，後者稱為***物件***。 `null` 的特殊值與所有參考型別相容，並指出不存在實例。

### <a name="class-types"></a>類別型別

類別類型會定義資料結構，其中包含資料成員（常數和欄位）、函式成員（方法、屬性、事件、索引子、運算子、實例的函式、析構函式和靜態的函式）和巢狀型別。 類別類型支援繼承，這是衍生類別可以擴充和特殊化基類的機制。 類別類型的實例是使用*object_creation_expression*s （[物件建立運算式](expressions.md#object-creation-expressions)）所建立。

類別中會描述類別[類型。](classes.md)

某些預先定義的C#類別類型在語言中具有特殊意義，如下表所述。


| __類別類型__     | __說明__                                         |
|--------------------|---------------------------------------------------------|
| `System.Object`    | 所有其他類型的最終基底類別。 請參閱[物件類型](types.md#the-object-type)。 | 
| `System.String`    | C#語言的字串類型。 請參閱[字串類型](types.md#the-string-type)。         |
| `System.ValueType` | 所有實數值型別的基類。 請參閱[system.object 類型](types.md#the-systemvaluetype-type)。          |
| `System.Enum`      | 所有列舉類型的基類。 請[參閱](enums.md)列舉。              |
| `System.Array`     | 所有陣列類型的基類。 請參閱[陣列](arrays.md)。             |
| `System.Delegate`  | 所有委派類型的基類。 請參閱[委派](delegates.md)。          |
| `System.Exception` | 所有例外狀況類型的基類。 請參閱[例外](exceptions.md)狀況。         |

### <a name="the-object-type"></a>物件型別

`object` 類別類型是所有其他類型的最終基底類別。 中C#的每個型別都是直接或間接衍生自 `object` 類別型別。

關鍵字 `object` 只是預先定義之類別 `System.Object`的別名。

### <a name="the-dynamic-type"></a>動態型別

`dynamic` 型別（例如 `object`）可以參考任何物件。 當運算子套用至 `dynamic`類型的運算式時，其解析會延遲到程式執行為止。 因此，如果運算子無法合法套用至參考的物件，則在編譯期間不會提供任何錯誤。 相反地，當運算子的解析在執行時間失敗時，將會擲回例外狀況。

其目的是要允許動態系結，這在[動態](expressions.md#dynamic-binding)系結中有詳細的說明。

`dynamic` 會視為與 `object` 相同，但下列方面除外：

*  `dynamic` 類型之運算式上的作業可以動態繫結（[動態](expressions.md#dynamic-binding)系結）。
*  型別推斷（[型別推斷](expressions.md#type-inference)）會偏好 `object` 的 `dynamic`，如果兩者都是候選項目。

由於這項等價，下列內容包含：

*  `object` 和 `dynamic`之間，以及以 `object` 取代 `dynamic` 時，兩者之間有隱含的身分識別轉換。
*  `object` 的隱含和明確轉換也適用于 `dynamic`的和。
*  以 `object` 取代 `dynamic` 時，方法簽章會被視為相同的簽章
*  類型 `dynamic` 在執行時間與 `object` 不區分。
*  `dynamic` 類型的運算式稱為***動態運算式***。

### <a name="the-string-type"></a>字串型別

`string` 類型是直接繼承自 `object`的密封類別類型。 `string` 類別的實例代表 Unicode 字元字串。

`string` 類型的值可以撰寫為字串常值（[字串常](lexical-structure.md#string-literals)值）。

關鍵字 `string` 只是預先定義之類別 `System.String`的別名。

### <a name="interface-types"></a>介面型別

介面會定義合約。 實作為介面的類別或結構必須遵守其合約。 介面可能繼承自多個基底介面，而類別或結構可能會執行多個介面。

介面類別型會[在介面中說明。](interfaces.md)

### <a name="array-types"></a>陣列型別

陣列是一種資料結構，其中包含零個或多個透過計算索引存取的變數。 陣列中包含的變數（也稱為陣列的元素）都是相同的類型，而此類型稱為陣列的元素類型。

陣列中會描述陣列[類型。](arrays.md)

### <a name="delegate-types"></a>委派型別

「委派」（delegate）是指一或多個方法的資料結構。 若為實例方法，它也會參考其對應的物件實例。

C 或C++中委派最接近的對等是函式指標，但函式指標只能參考靜態函式，而委派可以同時參考靜態和實例方法。 在後者的情況下，委派不僅會儲存對方法進入點的參考，也會存放要叫用方法之物件實例的參考。

委派中會描述委派[類型。](delegates.md)

## <a name="boxing-and-unboxing"></a>Boxing 和 Unboxing

裝箱和取消裝箱的概念是類型系統C#的核心。 它會允許*value_type*的任何值在類型 `object`之間來回轉換，藉此提供*value_type*s 與*reference_type*之間的橋樑。 「裝箱」和「取消裝箱」可讓類型系統的統一視圖，其中任何類型的值最終可以視為物件。

### <a name="boxing-conversions"></a>裝箱轉換

「裝箱」轉換允許將*value_type*隱含地轉換成*reference_type*。 有下列的裝箱轉換：

*  從任何*value_type*到類型 `object`。
*  從任何*value_type*到類型 `System.ValueType`。
*  從任何*non_nullable_value_type*到*value_type*所執行的任何*interface_type* 。
*  從任何*nullable_type*到*nullable_type*基礎類型所實*interface_type*的任何。
*  從任何*enum_type*到類型 `System.Enum`。
*  從任何具有基礎*enum_type*的*nullable_type*到類型 `System.Enum`。
*  請注意，從型別參數隱含的轉換會當做「裝箱」轉換來執行（如果在執行時間，它最後會從實值型別轉換成引用型別（[包括型別參數的隱含轉換](conversions.md#implicit-conversions-involving-type-parameters)）。

為*non_nullable_value_type*的值進行裝箱，包括設定物件實例，以及將*non_nullable_value_type*值複製到該實例。

如果*nullable_type*的值為 `null` 值（`HasValue` `false`），或解除包裝並將基礎值裝箱的結果，則會產生 null 參考。

若要將*non_nullable_value_type*的值進行裝箱，最佳的處理方式是想像泛型的***裝箱類別***是否存在，其行為就如同宣告如下：

```csharp
sealed class Box<T>: System.ValueType
{
    T value;

    public Box(T t) {
        value = t;
    }
}
```

`T` 類型的值 `v` 的裝箱現在包含執行運算式 `new Box<T>(v)`，並以 `object`類型的值傳回產生的實例。 因此，語句
```csharp
int i = 123;
object box = i;
```
概念上對應至
```csharp
int i = 123;
object box = new Box<int>(i);
```

如上 `Box<T>` 的裝箱類別並不存在，而且已裝箱值的動態類型實際上不是類別類型。 相反地，`T` 類型的已裝箱值具有動態類型 `T`，而使用 `is` 運算子的動態類型檢查可以直接參考類型 `T`。 例如，套用至物件的
```csharp
int i = 123;
object box = i;
if (box is int) {
    Console.Write("Box contains an int");
}
```
會在主控台上輸出字串 "`Box contains an int`"。

「裝箱」轉換意指建立所要封裝之值的複本。 這不同于*reference_type*轉換成類型 `object`，其中值會繼續參考相同的實例，而且只會被視為較少的衍生類型 `object`。 例如，假設宣告
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
下列語句
```csharp
Point p = new Point(10, 10);
object box = p;
p.x = 20;
Console.Write(((Point)box).x);
```
會在主控台上輸出值10，因為 `box` 指派 `p` 中的隱含裝箱作業會導致複製 `p` 的值。 已將 `Point` 宣告為 `class`，則值20會是輸出，因為 `p` 和 `box` 會參考相同的實例。

### <a name="unboxing-conversions"></a>取消裝箱轉換

「取消裝箱」轉換允許將*reference_type*明確轉換成*value_type*。 下列的取消裝箱轉換已存在：

*  從 [類型] `object` 到任何*value_type*。
*  從 [類型] `System.ValueType` 到任何*value_type*。
*  從任何*interface_type*到任何會執行*interface_type*的*non_nullable_value_type* 。
*  從任何*interface_type*到其基礎類型會實作為*interface_type*的任何*nullable_type* 。
*  從 [類型] `System.Enum` 到任何*enum_type*。
*  從類型 `System.Enum` 至具有基礎*enum_type*的任何*nullable_type* 。
*  請注意，明確轉換為類型參數時，如果是在執行時間將其從參考型別轉換成實值型別（[明確動態轉換](conversions.md#explicit-dynamic-conversions)），就會以「取消鎖定」轉換的方式執行。

*Non_nullable_value_type*的取消裝箱作業包含第一次檢查物件實例是否為給定*non_nullable_value_type*的已裝箱值，然後將值從實例中複製出來。

如果 `null`來源運算元，則取消裝箱至*nullable_type*會產生*nullable_type*的 null 值，否則會將物件實例取消包裝到*nullable_type*的基礎類型。

參考上一節所述的虛數裝箱類別，將物件 `box` 的取消裝箱轉換為*value_type* `T` 由執行運算式 `((Box<T>)box).value`所組成。 因此，語句
```csharp
object box = 123;
int i = (int)box;
```
概念上對應至
```csharp
object box = new Box<int>(123);
int i = ((Box<int>)box).value;
```

對於給定*non_nullable_value_type*在執行時間成功的取消裝箱轉換，來源運算元的值必須是該*non_nullable_value_type*之已裝箱值的參考。 如果 `null`來源運算元，則會擲回 `System.NullReferenceException`。 如果來源運算元是不相容物件的參考，則會擲回 `System.InvalidCastException`。

對於給定*nullable_type*在執行時間成功的取消裝箱轉換，來源運算元的值必須是 `null` 或*nullable_type*之基礎*non_nullable_value_type*的已裝箱值的參考。 如果來源運算元是不相容物件的參考，則會擲回 `System.InvalidCastException`。

## <a name="constructed-types"></a>結構化類型

泛型型別宣告本身則代表一個未系結的***泛型型***別，它是用來做為「藍圖」，藉由套用型別***引數***來形成許多不同的類型。 型別引數是以在泛型型別名稱之後的角括弧（`<` 和 `>`）撰寫。 包含至少一個型別引數的型別稱為「***結構化型***別」。 在可顯示類型名稱的語言中，大部分的位置都可以使用結構化類型。 未系結的泛型型別只能用在*typeof_expression* （[typeof 運算子](expressions.md#the-typeof-operator)）內。

結構化類型也可以在運算式中當做簡單名稱（[簡單名稱](expressions.md#simple-names)），或在存取成員（[成員存取](expressions.md#member-access)）時使用。

評估*namespace_or_type_name*時，只會考慮具有正確數目之類型參數的泛型型別。 因此，只要類型的類型參數數目不同，就可以使用相同的識別碼來識別不同的類型。 在相同的程式中混合泛型和非泛型類別時，這會很有用：

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

*Type_name*可能會識別已建立的類型，即使它並未直接指定類型參數也一樣。 這種情況可能發生在泛型類別宣告內的類型，而且包含宣告的實例類型會隱含用於名稱查閱（[泛型類別中的巢狀型別](classes.md#nested-types-in-generic-classes)）：

```csharp
class Outer<T>
{
    public class Inner {...}

    public Inner i;                // Type of i is Outer<T>.Inner
}
```

在 unsafe 程式碼中，不能使用結構化類型做為*unmanaged_type* （[指標類型](unsafe-code.md#pointer-types)）。

### <a name="type-arguments"></a>型別引數

型別引數清單中的每個引數都只是一個*型*別。

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

在 unsafe 程式碼（[unsafe 程式碼](unsafe-code.md)）中， *type_argument*可能不是指標類型。 每個型別引數都必須滿足對應型別參數的任何條件約束（[型別參數條件約束](classes.md#type-parameter-constraints)）。

### <a name="open-and-closed-types"></a>開啟和關閉類型

所有類型都可以分類為***開放式類型***或***封閉式類型***。 「開啟類型」是包含型別參數的型別。 更具體而言：

*  型別參數會定義開放式型別。
*  只有在其專案類型為開放式類型時，陣列類型才是開啟類型。
*  如果只有一或多個型別引數是開放式型別，則結構化型別就是開放式型別。 只有當一個或多個類型引數或其包含類型的類型引數是開放式類型時，結構化的巢狀型別才是開放式類型。

封閉式類型是不是開放式類型的類型。

在執行時間，泛型型別宣告內的所有程式碼都是在封閉式結構型別的內容中執行，而這是透過將型別引數套用至泛型宣告所建立。 泛型型別中的每個型別參數都會系結至特定的執行時間型別。 所有語句和運算式的執行時間處理一律會發生于封閉式類型，而開放式類型只會在編譯時間處理期間發生。

每個封閉的結構類型都有自己的靜態變數集，不會與任何其他已關閉的結構類型共用。 由於開啟的類型不存在於執行時間，因此沒有與開放式類型相關聯的靜態變數。 如果兩個封閉的結構型別是從相同的未系結泛型型別進行構造，而且其對應的型別引數都是相同的型別，

### <a name="bound-and-unbound-types"></a>系結和未系結類型

「未系結***類型***」一詞指的是非泛型型別或未系結的泛型型別。 「系結***型***別」一詞指的是非泛型型別或結構化型別。

未系結的型別是指由型別宣告所宣告的實體。 未系結的泛型型別本身不是類型，而且不能當做變數、引數或傳回值的類型或基底類型使用。 唯一可以參考未系結泛型型別的結構是 `typeof` 運算式（[typeof 運算子](expressions.md#the-typeof-operator)）。

### <a name="satisfying-constraints"></a>滿足條件約束

每當參考了結構化型別或泛型方法時，就會針對泛型型別或方法（[型別參數條件約束](classes.md#type-parameter-constraints)）上宣告的型別參數條件約束，檢查提供的型別引數。 針對每個 `where` 子句，會針對每個條件約束檢查對應至命名類型參數的類型引數 `A`，如下所示：

*  如果條件約束是類別類型、介面類別型或類型參數，則 let `C` 會以提供的類型引數取代條件約束中出現的任何類型參數，來表示該條件約束。 若要滿足條件約束，必須將類型 `A` 轉換成下列其中一項所 `C` 的類型：
    * 身分識別轉換（身分[識別轉換](conversions.md#identity-conversion)）
    * 隱含參考轉換（[隱含參考轉換](conversions.md#implicit-reference-conversions)）
    * 如果類型 A 是不可為 null 的實數值型別，則為裝箱轉換（[裝箱轉換](conversions.md#boxing-conversions)）。
    * 從型別參數轉換成 `C`的隱含參考、裝箱或型別參數 `A`。
*  如果條件約束是參考型別條件約束（`class`），則類型 `A` 必須符合下列其中一項：
    * `A` 是介面型別、類別型別、委派型別或陣列型別。 請注意，`System.ValueType` 和 `System.Enum` 是符合此條件約束的參考型別。
    * `A` 是已知為參考型別的型別參數（[型別參數條件約束](classes.md#type-parameter-constraints)）。
*  如果條件約束是實值型別條件約束（`struct`），則型別 `A` 必須滿足下列其中一項：
    * `A` 是結構型別或列舉型別，但不能是可為 null 的型別。 請注意，`System.ValueType` 和 `System.Enum` 是不符合此條件約束的參考型別。
    * `A` 是具有實數值型別條件約束的類型參數（[類型參數條件約束](classes.md#type-parameter-constraints)）。
*  如果條件約束是 `new()`的函式條件約束，則類型 `A` 不得 `abstract`，而且必須具有公用無參數的函式。 如果下列其中一項為真，就會滿足此情況：
    * `A` 是實數值型別，因為所有實數值型別都具有公用預設的函式（[預設](types.md#default-constructors)的函式）。
    * `A` 是具有「函式條件約束」（[型別參數條件約束](classes.md#type-parameter-constraints)）的型別參數。
    * `A` 是具有實數值型別條件約束的類型參數（[類型參數條件約束](classes.md#type-parameter-constraints)）。
    * `A` 是不 `abstract` 的類別，而且包含明確宣告的 `public` 不含任何參數的函式。
    * `A` 不 `abstract`，而且具有預設的函式（[預設](classes.md#default-constructors)的函式）。

如果指定的型別引數不符合一個或多個型別參數的條件約束，就會發生編譯時期錯誤。

因為類型參數不會繼承，所以永遠不會繼承條件約束。 在下列範例中，`D` 需要在其型別參數上指定條件約束 `T`，讓 `T` 滿足基類 `B<T>`所強加的條件約束。 相反地，類別 `E` 不需要指定條件約束，因為 `List<T>` 會針對任何 `T`來執行 `IEnumerable`。

```csharp
class B<T> where T: IEnumerable {...}

class D<T>: B<T> where T: IEnumerable {...}

class E<T>: B<List<T>> {...}
```

## <a name="type-parameters"></a>型別參數

型別參數是一個識別碼，用來指定在執行時間系結參數的實值型別或參考型別。

```antlr
type_parameter
    : identifier
    ;
```

因為類型參數可以使用許多不同的實際類型引數具現化，所以類型參數與其他類型的作業和限制會稍有不同。 它們包括：

*  型別參數不能直接用來宣告基類（[基類](classes.md#base-class)）或介面（[Variant 型別參數清單](interfaces.md#variant-type-parameter-lists)）。
*  類型參數上成員查閱的規則取決於套用至類型參數的條件約束（如果有的話）。 它們會在[成員查閱](expressions.md#member-lookup)中詳述。
*  型別參數的可用轉換取決於套用至型別參數的條件約束（如果有的話）。 它們會在[涉及型別參數](conversions.md#implicit-conversions-involving-type-parameters)和[明確動態轉換](conversions.md#explicit-dynamic-conversions)的隱含轉換中詳述。
*  常值 `null` 無法轉換成型別參數所指定的型別，除非已知型別參數是引用型別（[涉及型別參數的隱含轉換](conversions.md#implicit-conversions-involving-type-parameters)）。 不過，您可以改為使用 `default` 運算式（[預設值運算式](expressions.md#default-value-expressions)）。 此外，除非類型參數具有實數值型別條件約束，否則，類型參數所指定之類型的值可以與使用 `==` 和 `!=` （[參考類型等號比較運算子](expressions.md#reference-type-equality-operators)）的 `null` 進行比較。
*  如果型別參數受到*constructor_constraint*或實值型別條件約束（[型別參數條件約束](classes.md#type-parameter-constraints)）的限制，則 `new` 運算式（[物件建立運算式](expressions.md#object-creation-expressions)）只能與型別參數一起使用。
*  類型參數不能用在屬性內的任何位置。
*  型別參數不能用在成員存取（[成員存取](expressions.md#member-access)）或型別名稱（[命名空間和型別名稱](basic-concepts.md#namespace-and-type-names)）中，以識別靜態成員或嵌套型別。
*  在 unsafe 程式碼中，類型參數不能當做*unmanaged_type* （[指標類型](unsafe-code.md#pointer-types)）使用。

就型別而言，型別參數純粹是編譯時間結構。 在執行時間，每個型別參數都會系結至執行時間型別，其指定方式是提供泛型型別宣告的型別引數。 因此，使用型別參數宣告之變數的型別將會在執行時間為封閉的結構化型別（[開放式和封閉式類型](types.md#open-and-closed-types)）。 所有涉及型別參數之語句和運算式的執行時間執行，都會使用當做該參數的型別引數提供的實際型別。

## <a name="expression-tree-types"></a>運算式樹狀架構類型

***運算式樹狀***架構允許將 lambda 運算式表示為資料結構，而不是可執行檔程式碼。 運算式樹狀架構是 `System.Linq.Expressions.Expression<D>`形式之***運算式樹狀架構類型***的值，其中 `D` 是任何委派類型。 針對此規格的其餘部分，我們將使用速記 `Expression<D>`來參考這些類型。

如果從 lambda 運算式到委派類型 `D`的轉換存在，`Expression<D>`的運算式樹狀架構類型也會有轉換。 當 lambda 運算式轉換成委派類型時，會產生一個委派來參考 lambda 運算式的可執行程式碼，而轉換成運算式樹狀架構類型會建立 lambda 運算式的運算式樹狀結構標記法。

運算式樹狀架構是 lambda 運算式的有效率記憶體內部資料標記法，並可讓 lambda 運算式的結構變成透明和明確。

就像委派型別 `D`，`Expression<D>` 會有參數和傳回型別，這與 `D`的類型相同。

下列範例會將 lambda 運算式表示為可執行程式碼和運算式樹狀架構。 因為 `Func<int,int>`的轉換存在，`Expression<Func<int,int>>`的轉換也存在：

```csharp
Func<int,int> del = x => x + 1;                    // Code

Expression<Func<int,int>> exp = x => x + 1;        // Data
```

在這些指派之後，委派 `del` 會參考傳回 `x + 1`的方法，而且運算式樹狀架構 `exp` 會參考描述運算式 `x => x + 1`的資料結構。

泛型型別的確切定義 `Expression<D>` 以及將 lambda 運算式轉換成運算式樹狀架構類型時，用來建立運算式樹狀架構的精確規則，兩者都在此規格的範圍之外。

明確來說，有兩件事要特別注意：

*  並非所有 lambda 運算式都可以轉換成運算式樹狀架構。 例如，具有語句主體的 lambda 運算式，以及包含指派運算式的 lambda 運算式無法表示。 在這些情況下，轉換仍然存在，但會在編譯時間失敗。 這些例外狀況詳述于[匿名函數轉換](conversions.md#anonymous-function-conversions)中。
*   `Expression<D>` 提供實例方法 `Compile`，其會產生類型 `D`的委派：

    ```csharp
    Func<int,int> del2 = exp.Compile();
    ```

    叫用此委派會使運算式樹狀架構所代表的程式碼執行。 因此，在上述定義中，del 和 del2 是相同的，而下列兩個語句會有相同的效果：

    ```csharp
    int i1 = del(1);
    
    int i2 = del2(1);
    ```

    執行此程式碼之後，`i1` 和 `i2` 都會有 `2`的值。

