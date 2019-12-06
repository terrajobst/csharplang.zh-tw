---
ms.openlocfilehash: 4d6d28a3127bc701867afe157aa5496377a06f69
ms.sourcegitcommit: 63d276488c9770a565fd787020783ffc1d2af9d6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2019
ms.locfileid: "74868000"
---
# <a name="conversions"></a>轉換

***轉換***可讓運算式被視為屬於特定類型。 轉換可能會使給定類型的運算式被視為具有不同的類型，或者，它可能會導致沒有類型的運算式取得類型。 轉換可以是***隱含***或***明確***的，這會決定是否需要明確轉換。 例如，從類型 `int` 到類型 `long` 的轉換是隱含的，因此類型 `int` 的運算式可以隱含地視為類型 `long`。 相反的轉換（從類型 `long` 到類型 `int`）是明確的，因此需要明確轉換。

```csharp
int a = 123;
long b = a;         // implicit conversion from int to long
int c = (int) b;    // explicit conversion from long to int
```

某些轉換是由語言所定義。 程式也可以定義自己的轉換（[使用者定義的轉換](conversions.md#user-defined-conversions)）。

## <a name="implicit-conversions"></a>隱含轉換

下列轉換會分類為隱含轉換：

*  身分識別轉換
*  隱含數值轉換
*  隱含列舉轉換
*  隱含插入字串轉換
*  隱含可為 null 的轉換
*  Null 常值轉換
*  隱含參考轉換
*  裝箱轉換
*  隱含動態轉換
*  隱含常數運算式轉換
*  使用者定義的隱含轉換
*  匿名函數轉換
*  方法群組轉換

隱含轉換可能會在各種情況下發生，包括函數成員調用（[動態多載解析的編譯時間檢查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)）、轉換運算式（[cast 運算式](expressions.md#cast-expressions)）和指派（[指派運算子](expressions.md#assignment-operators)）。

預先定義的隱含轉換一律會成功，而且永遠不會擲回例外狀況。 適當設計的使用者定義隱含轉換也應該展現這些特性。

基於轉換的目的，`object` 和 `dynamic` 的類型會被視為相等。

不過，動態轉換（[隱含動態轉換](conversions.md#implicit-dynamic-conversions)和[明確動態轉換](conversions.md#explicit-dynamic-conversions)）只適用于 `dynamic` 類型的運算式（[動態類型](types.md#the-dynamic-type)）。

### <a name="identity-conversion"></a>身分識別轉換

身分識別轉換會從任何類型轉換成相同的類型。 這種轉換存在的情況下，您可以將已經具有必要類型的實體視為可轉換成該類型。

*  由於 `object` 和 `dynamic` 被視為相等，因此在 `object` 和 `dynamic`之間，以及將所有出現的 `dynamic` 取代為 `object`時，兩者之間會有識別轉換。

### <a name="implicit-numeric-conversions"></a>隱含數值轉換

隱含數值轉換包括：

*  從 `sbyte` 到 `short`、`int`、`long`、`float`、`double`或 `decimal`。
*  從 `byte` 到 `short`、`ushort`、`int`、`uint`、`long`、`ulong`、`float`、`double`或 `decimal`。
*  從 `short` 到 `int`、`long`、`float`、`double`或 `decimal`。
*  從 `ushort` 到 `int`、`uint`、`long`、`ulong`、`float`、`double`或 `decimal`。
*  從 `int` 到 `long`、`float`、`double`或 `decimal`。
*  從 `uint` 到 `long`、`ulong`、`float`、`double`或 `decimal`。
*  從 `long` 到 `float`、`double`或 `decimal`。
*  從 `ulong` 到 `float`、`double`或 `decimal`。
*  從 `char` 到 `ushort`、`int`、`uint`、`long`、`ulong`、`float`、`double`或 `decimal`。
*  從 `float` 到 `double`。

從 `int`、`uint`、`long`或 `ulong` 轉換成 `float` 和從 `long` 或 `ulong` 到 `double` 可能會導致精確度遺失，但絕不會造成損失。 其他隱含數值轉換永遠不會遺失任何資訊。

`char` 類型沒有隱含轉換，因此其他整數類型的值不會自動轉換成 `char` 類型。

### <a name="implicit-enumeration-conversions"></a>隱含列舉轉換

隱含列舉轉換可讓*decimal_integer_literal* `0` 轉換成任何*enum_type* ，以及其基礎類型為*enum_type*的任何*nullable_type* 。 在後者的情況下，轉換會藉由轉換成基礎*enum_type*並將結果包裝起來（[可為 null 的類型](types.md#nullable-types)）來進行評估。

### <a name="implicit-interpolated-string-conversions"></a>隱含插入字串轉換

隱含插入字串轉換可讓*interpolated_string_expression* （[字串插值](expressions.md#interpolated-strings)）轉換成 `System.IFormattable` 或 `System.FormattableString` （這會實 `System.IFormattable`）。

套用此轉換時，字串值不是由插入字串所組成。 相反地，會建立 `System.FormattableString` 的實例，如[字串插值](expressions.md#interpolated-strings)中所述。

### <a name="implicit-nullable-conversions"></a>隱含可為 null 的轉換

在不可為 null 的實數值型別上操作的預先定義隱含轉換，也可以搭配這些類型的可為 null 形式使用。 針對從不可為 null 的實值型別轉換成不可為 null 之實值型別的每個預先定義的隱含識別和數值轉換 `S` 為不能為 null 的實數值型別 `T`，會存在下列隱含可為 null

*  從 `S?` 到 `T?`的隱含轉換。
*  從 `S` 到 `T?`的隱含轉換。

根據從 `S` 到 `T` 的基礎轉換，評估隱含可為 null 的轉換，如下所示：

*  如果可為 null 的轉換是從 `S?` 到 `T?`：
    * 如果來源值為 null （`HasValue` 屬性為 false），則結果會是 `T?`類型的 null 值。
    * 否則，會將轉換評估為從 `S?` 解除包裝為 `S`，後面接著從 `S` 到 `T`的基礎轉換，後面接著從 `T` 到 `T?`的包裝（[可為 null 的類型](types.md#nullable-types)）。

*  如果可為 null 的轉換是從 `S` 到 `T?`，則會將轉換評估為從 `S` 到 `T` 的基礎轉換，然後再從 `T` 換成 `T?`。

### <a name="null-literal-conversions"></a>Null 常值轉換

從 `null` 常值到任何可為 null 的型別都有隱含的轉換。 這項轉換會產生給定可為 null 類型的 null 值（[可為](types.md#nullable-types)null 的類型）。

### <a name="implicit-reference-conversions"></a>隱含參考轉換

隱含參考轉換包括：

*  從任何*reference_type*到 `object` 並 `dynamic`。
*  從任何*class_type* `S` 到任何*class_type*的 `T`，提供 `S` 衍生自 `T`。
*  從任何*class_type* `S` 到任何*interface_type* `T`，提供 `S` 執行 `T`。
*  從任何*interface_type* `S` 到任何*interface_type*的 `T`，提供 `S` 衍生自 `T`。
*  從具有元素類型的*array_type* `S` `SE` 至專案類型 `T` 的*array_type* `TE`，前提是下列所有條件皆成立：
    * `S` 和 `T` 只有元素類型不同。 換句話說，`S` 和 `T` 的維度數目相同。
    * `SE` 和 `TE` 都是*reference_type*s。
    * 從 `SE` 到 `TE`都有隱含的參考轉換。
*  從任何*array_type*到 `System.Array`，以及它所執行的介面。
*  從一維陣列類型 `S[]` 到 `System.Collections.Generic.IList<T>` 及其基底介面，但前提是從 `S` 到 `T`的隱含識別或參考轉換。
*  從任何*delegate_type*到 `System.Delegate`，以及它所執行的介面。
*  從 null 常值到任何*reference_type*。
*  從任何*reference_type*到*reference_type* `T` 如果它具有*reference_type* `T0` 的隱含識別或參考轉換，而且 `T0` 有身分識別轉換為 `T`。
*  從任何*reference_type*到介面或委派類型 `T` 如果它具有介面或委派類型的隱含識別或參考轉換 `T0` 而且 `T0` 可轉換為 `T`的變異數（變異數[轉換](interfaces.md#variance-conversion)）。
*  包含已知為參考型別之類型參數的隱含轉換。 如需有關涉及型別參數之隱含轉換的詳細資訊，請參閱[涉及型別參數的隱含轉換](conversions.md#implicit-conversions-involving-type-parameters)。

隱含參考轉換是*reference_type*之間的轉換，可證明一律成功，因此不需要在執行時間進行任何檢查。

參考轉換（隱含或明確）永遠不會變更正在轉換之物件的參考身分識別。 換句話說，雖然參考轉換可能會變更參考的類型，但它不會變更所參考物件的類型或值。

### <a name="boxing-conversions"></a>裝箱轉換

「裝箱」轉換允許將*value_type*隱含地轉換成參考型別。 從任何*non_nullable_value_type*到 `object` 和 `dynamic`的「裝箱」轉換，`System.ValueType` 和*interface_type*所實的任何*non_nullable_value_type* 。 此外， *enum_type*可以轉換成類型 `System.Enum`。

只有在從基礎*non_nullable_value_type*到參考型別的裝箱轉換存在時，從*nullable_type*到參考型別的裝箱轉換才會存在。

實值型別具有介面型別的「裝箱」轉換，`I` 如果它有 `I0` 的介面型別的裝箱轉換，而且 `I0` 具有 `I`的身分識別轉換。

實值型別具有介面型別的裝箱轉換，`I` 如果它有介面或委派型別的裝箱轉換 `I0` 而且 `I0` 可以區分變異數（變異數[轉換](interfaces.md#variance-conversion)）為 `I`。

為*non_nullable_value_type*的值進行裝箱，包括設定物件實例，以及將*value_type*值複製到該實例。 結構可以封裝成型別 `System.ValueType`，因為這是所有結構（[繼承](structs.md#inheritance)）的基類（base class）。

將*nullable_type*的值進行裝箱，如下所示：

*  如果來源值為 null （`HasValue` 屬性為 false），則結果會是目標型別的 null 參考。
*  否則，結果會是透過解除包裝和裝箱來源值所產生的盒裝 `T` 的參考。

在「[裝箱」轉換](types.md#boxing-conversions)中會進一步說明「裝箱」轉換。

### <a name="implicit-dynamic-conversions"></a>隱含動態轉換

從類型的運算式 `dynamic` 到任何類型 `T`的隱含動態轉換存在。 轉換是動態系結的（[動態](expressions.md#dynamic-binding)系結），這表示在執行時間會從運算式的執行時間類型中搜尋隱含轉換，以 `T`。 如果找不到任何轉換，則會擲回執行時間例外狀況。

請注意，這項隱含轉換似乎違反隱含轉換開始不會造成例外狀況的[隱性](conversions.md#implicit-conversions)轉換開頭的建議。 不過，它不是轉換本身，而是*尋找*導致例外狀況的轉換。 使用動態系結時，會造成執行時間例外狀況的風險。 如果不想要轉換的動態系結，可以先將運算式轉換成 `object`，然後再轉換成所需的類型。

下列範例說明隱含的動態轉換：

```csharp
object o  = "object"
dynamic d = "dynamic";

string s1 = o; // Fails at compile-time -- no conversion exists
string s2 = d; // Compiles and succeeds at run-time
int i     = d; // Compiles but fails at run-time -- no conversion exists
```

`s2` 和 `i` 的指派會採用隱含的動態轉換，其中作業的系結會暫止到執行時間。 在執行時間，隱含轉換會從執行時間類型的 `d` -- `string`--到目標型別。 `string` 發現轉換，而不是 `int`。

### <a name="implicit-constant-expression-conversions"></a>隱含常數運算式轉換

隱含的常數運算式轉換允許下列轉換：

*  如果 *`short`* 的值在目的地類型的範圍內，則 `int` 類型的*constant_expression* （[常數運算式](expressions.md#constant-expressions)）可以轉換成類型 `sbyte`、`byte`、`ushort`、`uint`、`ulong`或 constant_expression。
*  如果*constant_expression*的值不是負值，則 `long` 類型的*constant_expression*可以轉換成類型 `ulong`。

### <a name="implicit-conversions-involving-type-parameters"></a>涉及型別參數的隱含轉換

指定的類型參數 `T`有下列隱含轉換：

*  從 `T` 到其有效基類 `C`，從 `T` 到 `C`的任何基類，以及從 `T` 到 `C`所實的任何介面。 在執行時間，如果 `T` 是實值型別，則會以「裝箱」轉換來執行轉換。 否則，會以隱含的參考轉換或身分識別轉換來執行轉換。
*  從 `T` 到 `T`有效介面集內 `I` 介面類別型，以及從 `T` 到 `I`的任何基底介面。 在執行時間，如果 `T` 是實值型別，則會以「裝箱」轉換來執行轉換。 否則，會以隱含的參考轉換或身分識別轉換來執行轉換。
*  從 `T` 到型別參數 `U`，提供 `T` 取決於 `U` （[型別參數條件約束](classes.md#type-parameter-constraints)）。 在執行時間，如果 `U` 是實值型別，則 `T` 和 `U` 一定是相同的型別，而且不會執行任何轉換。 否則，如果 `T` 是實值型別，則會以「裝箱」轉換來執行轉換。 否則，會以隱含的參考轉換或身分識別轉換來執行轉換。
*  從 null 常值到 `T`，提供的 `T` 已知為參考型別。
*  從 `T` 到參考型別 `I` 如果它有隱含轉換成參考型別 `S0` 而且 `S0` 有識別轉換成 `S`。 在執行時間，轉換的執行方式與轉換 `S0`相同。
*  從 `T` 到介面型別，`I` 如果它具有介面或委派型別的隱含轉換 `I0` 而且 `I0` 可轉換成 `I` （變異數[轉換](interfaces.md#variance-conversion)）。 在執行時間，如果 `T` 是實值型別，則會以「裝箱」轉換來執行轉換。 否則，會以隱含的參考轉換或身分識別轉換來執行轉換。

如果已知 `T` 是參考型別（型別[參數條件約束](classes.md#type-parameter-constraints)），上述的轉換就會分類為隱含參考轉換（[隱含參考轉換](conversions.md#implicit-reference-conversions)）。 如果 `T` 不知道是參考型別，上述的轉換會分類為「裝箱」轉換（「[裝箱](conversions.md#boxing-conversions)」轉換）。

### <a name="user-defined-implicit-conversions"></a>使用者定義的隱含轉換

使用者定義的隱含轉換是由選擇性的標準隱含轉換所組成，接著執行使用者定義的隱含轉換運算子，再接著另一個選擇性的標準隱含轉換。 在[處理使用者定義的隱含](conversions.md#processing-of-user-defined-implicit-conversions)轉換時，會說明評估使用者定義隱含轉換的確切規則。

### <a name="anonymous-function-conversions-and-method-group-conversions"></a>匿名函數轉換和方法群組轉換

匿名函式和方法群組本身並沒有類型，但可以隱含地轉換成委派類型或運算式樹狀架構類型。 匿名函數轉換在[方法群組轉換](conversions.md#method-group-conversions)中的[匿名](conversions.md#anonymous-function-conversions)函式轉換和方法群組轉換中有更詳細的說明。

## <a name="explicit-conversions"></a>明確轉換

下列轉換會分類為明確轉換：

*  所有隱含轉換。
*  明確數值轉換。
*  明確列舉轉換。
*  可為 null 的明確轉換。
*  明確參考轉換。
*  明確的介面轉換。
*  取消裝箱轉換。
*  明確動態轉換
*  使用者定義的明確轉換。

明確轉換可以在 cast 運算式中進行（[cast 運算式](expressions.md#cast-expressions)）。

明確轉換的集合包含所有隱含轉換。 這表示允許使用多餘的轉換運算式。

不是隱含轉換的明確轉換是無法證明一律成功的轉換、已知可能會遺失資訊的轉換，以及跨類型的多個網域的轉換，足以明確地進行萬用字元.

### <a name="explicit-numeric-conversions"></a>明確數值轉換

明確數值轉換是從*numeric_type*到另一個*numeric_type*的轉換，也就是隱含數值轉換（[隱含數值](conversions.md#implicit-numeric-conversions)轉換）尚不存在：

*  從 `sbyte` 到 `byte`、`ushort`、`uint`、`ulong`或 `char`。
*  從 `byte` 到 `sbyte` 並 `char`。
*  從 `short` 到 `sbyte`、`byte`、`ushort`、`uint`、`ulong`或 `char`。
*  從 `ushort` 到 `sbyte`、`byte`、`short`或 `char`。
*  從 `int` 到 `sbyte`、`byte`、`short`、`ushort`、`uint`、`ulong`或 `char`。
*  從 `uint` 到 `sbyte`、`byte`、`short`、`ushort`、`int`或 `char`。
*  從 `long` 到 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`ulong`或 `char`。
*  從 `ulong` 到 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`或 `char`。
*  從 `char` 到 `sbyte`、`byte`或 `short`。
*  從 `float` 到 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`或 `decimal`。
*  從 `double` 到 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`或 `decimal`。
*  從 `decimal` 到 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`或 `double`。

由於明確轉換包括所有隱含和明確的數值轉換，因此一律可以使用 cast 運算式（[cast 運算式](expressions.md#cast-expressions)），從任何*numeric_type*轉換成任何其他*numeric_type* 。

明確的數值轉換可能會遺失資訊，或可能導致擲回例外狀況。 明確數值轉換的處理方式如下：

*  若要從整數類資料類型轉換為另一個整數類型，處理方式取決於發生轉換的溢位檢查內容（[checked 和 unchecked 運算子](expressions.md#the-checked-and-unchecked-operators)）：
    * 在 `checked` 內容中，如果來源運算元的值在目的地類型的範圍內，轉換就會成功，但如果來源運算元的值超出目的地類型的範圍，則會擲回 `System.OverflowException`。
    * 在 `unchecked` 內容中，轉換一律會成功，並依照下列方式繼續進行。
        * 如果來源類型大於目的地類型，會藉由捨棄其「額外」最高有效位元，來截斷來源值。 然後會將結果視為目標類型的值。
        * 如果來源類型小於目標類型，則來源值可以是正負號擴充或零擴充，以使其與目標類型的大小相同。 如果來源類型帶正負號，則會使用正負號擴充；如果來源類型不帶正負號，則會使用零擴充。 然後會將結果視為目標類型的值。
        * 如果來源類型與目標類型的大小相同，則來源值將視為目標類型的值。
*  從 `decimal` 轉換成整數類資料類型時，會將來源值舍入至最接近整數值的零，而這個整數值會變成轉換的結果。 如果產生的整數值超出目的地類型的範圍，則會擲回 `System.OverflowException`。
*  若要從 `float` 或 `double` 轉換成整數類資料類型，處理會根據發生轉換的溢位檢查內容（[checked 和 unchecked 運算子](expressions.md#the-checked-and-unchecked-operators)）而定：
    * 在 `checked` 內容中，轉換會繼續進行，如下所示：
        * 如果運算元的值是 NaN 或無限大，則會擲回 `System.OverflowException`。
        * 否則，會將來源運算元向零進位到最接近的整數值。 如果這個整數值在目的地類型的範圍內，則這個值是轉換的結果。
        * 否則會擲回 `System.OverflowException`。
    * 在 `unchecked` 內容中，轉換一律會成功，並依照下列方式繼續進行。
        * 如果運算元的值是 NaN 或無限大，則轉換的結果會是目的地類型的未指定值。
        * 否則，會將來源運算元向零進位到最接近的整數值。 如果這個整數值在目的地類型的範圍內，則這個值是轉換的結果。
        * 否則，轉換的結果會是目的地類型的未指定值。
*  若要從 `double` 轉換為 `float`，`double` 值會四捨五入為最接近的 `float` 值。 如果 `double` 值太小而無法表示為 `float`，則結果會變成正零或負零。 如果 `double` 值太大，而無法表示為 `float`，則結果會變成正無限大或負無限大。 如果 `double` 值是 NaN，則結果也是 NaN。
*  若要從 `float` 或 `double` 轉換為 `decimal`，會將來源值轉換成 `decimal` 表示，並在必要時將其四捨五入為最接近的數位（[decimal 類型](types.md#the-decimal-type)）。 如果來源值太小而無法表示為 `decimal`，則結果會變成零。 如果來源值為 NaN、無限大，或太大而無法表示為 `decimal`，就會擲回 `System.OverflowException`。
*  若要從 `decimal` 到 `float` 或 `double`的轉換，`decimal` 值會舍入到最接近的 `double` 或 `float` 值。 雖然這種轉換可能會遺失精確度，但不會導致擲回例外狀況。

### <a name="explicit-enumeration-conversions"></a>明確列舉轉換

明確列舉轉換如下：

*  從 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`或 `decimal` 到任何*enum_type*。
*  從任何*enum_type*到 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`或 `decimal`。
*  從任何*enum_type*到任何其他*enum_type*。

在兩個類型之間進行明確列舉轉換的處理方式是將任何參與的*enum_type*視為該*enum_type*的基礎類型，然後在產生的類型之間執行隱含或明確的數值轉換。 例如，假設*enum_type* `E` 和 `int`的基礎類型，則 `E` 從 `byte` 到 `int` 的轉換會當做明確的數值轉換（明確的[數值](conversions.md#explicit-numeric-conversions)轉換）來處理，並從 `byte`轉換成 `byte`，然後將從 `E` 到 `byte` 的轉換當做隱含數值轉換（[隱含數值](conversions.md#implicit-numeric-conversions)轉換），從 `int`到進行處理。

### <a name="explicit-nullable-conversions"></a>明確可為 null 的轉換

***明確的可為 null 轉換***允許在不可為 null 的實數值型別上運作的預先定義明確轉換，也可以搭配這些類型的可為 null 形式來使用。 對於從不可為 null 的實值型別轉換 `S` 為不可為 null 的實值型別 `T` （[識別轉換](conversions.md#identity-conversion)、[隱含數值轉換](conversions.md#implicit-numeric-conversions)、[隱含列舉轉換](conversions.md#implicit-enumeration-conversions)、[明確的數值轉換](conversions.md#explicit-numeric-conversions)和[明確列舉轉換](conversions.md#explicit-enumeration-conversions)）的每個預先定義的明確轉換，都存在下列可為 null 的轉換：

*  從 `S?` 到 `T?`的明確轉換。
*  從 `S` 到 `T?`的明確轉換。
*  從 `S?` 到 `T`的明確轉換。

根據從 `S` 到 `T` 的基礎轉換，評估可為 null 的轉換，如下所示：

*  如果可為 null 的轉換是從 `S?` 到 `T?`：
    * 如果來源值為 null （`HasValue` 屬性為 false），則結果會是 `T?`類型的 null 值。
    * 否則，會將轉換評估為從 `S?` 解除包裝為 `S`，然後從 `S` 到 `T`進行基礎轉換，然後再從 `T` 換成 `T?`。
*  如果可為 null 的轉換是從 `S` 到 `T?`，則會將轉換評估為從 `S` 到 `T` 的基礎轉換，然後再從 `T` 換成 `T?`。
*  如果可為 null 的轉換是從 `S?` 到 `T`，則會將轉換評估為從 `S?` 解除包裝到 `S`，然後再從 `S` 到 `T`進行基礎轉換。

請注意，如果 `null`值，則嘗試解除包裝可為 null 的值將會擲回例外狀況。

### <a name="explicit-reference-conversions"></a>明確參考轉換

明確的參考轉換包括：

*  從 `object` 和 `dynamic` 到任何其他*reference_type*。
*  從任何*class_type* `S` 到任何*class_type*的 `T`，提供 `S` 是 `T`的基類。
*  從任何*class_type* `S` 到任何*interface_type*的 `T`，提供的 `S` 不是密封的，而且 `S` 不會執行 `T`。
*  從任何*interface_type* `S` 到任何*class_type*的 `T`，提供 `T` 不是密封或提供 `T` `S`。
*  從任何*interface_type* `S` 到任何*interface_type* `T`，提供的 `S` 不是衍生自 `T`。
*  從具有元素類型的*array_type* `S` `SE` 至專案類型 `T` 的*array_type* `TE`，前提是下列所有條件皆成立：
    * `S` 和 `T` 只有元素類型不同。 換句話說，`S` 和 `T` 的維度數目相同。
    * `SE` 和 `TE` 都是*reference_type*s。
    * 從 `SE` 到 `TE`都有明確的參考轉換。
*  從 `System.Array` 和它所執行的介面到任何*array_type*。
*  從一維陣列類型 `S[]` 到 `System.Collections.Generic.IList<T>` 及其基底介面，但前提是從 `S` 到 `T`的明確參考轉換。
*  從 `System.Collections.Generic.IList<S>` 及其基底介面到 `T[]`的一維陣列類型，前提是有明確識別或從 `S` 到 `T`的參考轉換。
*  從 `System.Delegate` 和它所執行的介面到任何*delegate_type*。
*  從參考型別到參考型別 `T` 如果它有參考型別的明確參考轉換 `T0` 而且 `T0` 具有 `T`的識別轉換。
*  從參考型別到介面或委派型別 `T` 如果它有介面或委派型別的明確參考轉換 `T0` 而且 `T0` 可以轉換成 `T` 或 `T` 可轉換成 `T0` （變異數[轉換](interfaces.md#variance-conversion)）。
*  從 `D<S1...Sn>` 到 `D<T1...Tn>`，其中 `D<X1...Xn>` 是泛型委派型別，`D<S1...Sn>` 與 `D<T1...Tn>`不相容，而針對每個型別參數 `Xi` `D` 下列各項：
    * 如果 `Xi` 是不變的，則 `Si` 等同于 `Ti`。
    * 如果 `Xi` 是協變數的，則會有從 `Si` 到 `Ti`的隱含或明確識別或參考轉換。
    * 如果 `Xi` 為逆變性，則 `Si` 和 `Ti` 都是相同或兩個參考型別。
*  包含已知為參考型別之類型參數的明確轉換。 如需有關涉及型別參數之明確轉換的詳細資訊，請參閱[涉及型別參數的明確轉換](conversions.md#explicit-conversions-involving-type-parameters)。

明確參考轉換是指需要執行時間檢查以確保它們正確的參考型別之間的轉換。

若要在執行時間成功進行明確的參考轉換，必須 `null`來源運算元的值，或來源運算元所參考之物件的實際類型，必須是可以透過隱含參考轉換（[隱含參考](conversions.md#implicit-reference-conversions)轉換）或「裝箱轉換」（「[裝箱](conversions.md#boxing-conversions)轉換」）轉換成目的類型的類型。 如果明確的參考轉換失敗，則會擲回 `System.InvalidCastException`。

參考轉換（隱含或明確）永遠不會變更正在轉換之物件的參考身分識別。 換句話說，雖然參考轉換可能會變更參考的類型，但它不會變更所參考物件的類型或值。

### <a name="unboxing-conversions"></a>取消裝箱轉換

「取消裝箱」轉換允許將參考型別明確轉換成*value_type*。 從 `object`的類型、`dynamic` 和 `System.ValueType` 到任何*non_nullable_value_type*，以及從任何*interface_type*到任何會執行*non_nullable_value_type*的任何*interface_type* ，都有一個取消程式轉換。 此外，您也可以將類型 `System.Enum` 取消加入任何*enum_type*。

如果從參考型別存在到*nullable_type*的基礎*non_nullable_value_type*的取消裝箱轉換，則會從參考型別到*nullable_type*的取消裝箱轉換。

實值型別 `S` 具有從介面型別進行的取消裝箱轉換 `I` 如果它有從介面型別進行的取消裝箱轉換 `I0` 而且 `I0` 會將身分識別轉換成 `I`。

實值型別 `S` 具有從介面型別進行的取消裝箱轉換 `I` 如果它有從介面或委派型別進行的取消裝箱轉換 `I0` 而且 `I0` 可轉換成 `I` 或 `I` 可以轉換成 `I0` （變異數[轉換](interfaces.md#variance-conversion)）。

取消封裝作業包含第一次檢查物件實例是否為指定*value_type*的已裝箱值，然後從實例複製值。 取消對*nullable_type*的 null 參考時，會產生*nullable_type*的 null 值。 結構可以從類型 `System.ValueType`中取消裝箱，因為這是所有結構（[繼承](structs.md#inheritance)）的基類。

取消[裝箱轉換中會](types.md#unboxing-conversions)進一步說明取消功能轉換。

### <a name="explicit-dynamic-conversions"></a>明確動態轉換

從類型的運算式 `dynamic` 到任何類型 `T`都有明確的動態轉換。 轉換是動態系結的（[動態](expressions.md#dynamic-binding)系結），這表示在執行時間會從運算式的執行時間類型中搜尋明確轉換，以 `T`。 如果找不到任何轉換，則會擲回執行時間例外狀況。

如果不想要轉換的動態系結，可以先將運算式轉換成 `object`，然後再轉換成所需的類型。

假設已定義下列類別：
```csharp
class C
{
    int i;

    public C(int i) { this.i = i; }

    public static explicit operator C(string s) 
    {
        return new C(int.Parse(s));
    }
}
```

下列範例說明明確的動態轉換：
```csharp
object o  = "1";
dynamic d = "2";

var c1 = (C)o; // Compiles, but explicit reference conversion fails
var c2 = (C)d; // Compiles and user defined conversion succeeds
```

在編譯時期，將 `o` 到 `C` 的最佳轉換是明確的參考轉換。 這會在執行時間失敗，因為 `"1"` 事實上並不是 `C`。 不過，將 `d` 轉換成 `C` 明確動態轉換的執行時間，會將使用者定義從執行時間類型的 `d` -- `string`--轉換成 `C`），並成功完成。

### <a name="explicit-conversions-involving-type-parameters"></a>涉及型別參數的明確轉換

下列明確轉換存在於指定的類型參數 `T`：

*  從有效的基類 `C` `T` 到 `T`，以及從任何 `C` 基類到 `T`。 在執行時間，如果 `T` 是實值型別，則會以「取消裝箱」轉換來執行轉換。 否則，轉換會當做明確參考轉換或身分識別轉換來執行。
*  從任何介面類別型到 `T`。 在執行時間，如果 `T` 是實值型別，則會以「取消裝箱」轉換來執行轉換。 否則，轉換會當做明確參考轉換或身分識別轉換來執行。
*  從 `T` 到任何*interface_type* `I` 提供的不是從 `T` 到 `I`的隱含轉換。 在執行時間，如果 `T` 是實值型別，則轉換會執行為裝箱轉換，後面接著明確的參考轉換。 否則，轉換會當做明確參考轉換或身分識別轉換來執行。
*  從型別參數 `U` 至 `T`，但前提 `T` 取決於 `U` （[型別參數條件約束](classes.md#type-parameter-constraints)）。 在執行時間，如果 `U` 是實值型別，則 `T` 和 `U` 一定是相同的型別，而且不會執行任何轉換。 否則，如果 `T` 是實值型別，則會將轉換當做取消裝箱轉換來執行。 否則，轉換會當做明確參考轉換或身分識別轉換來執行。

如果已知 `T` 是參考型別，上述的轉換會分類為明確參考轉換（[明確參考轉換](conversions.md#explicit-reference-conversions)）。 如果 `T` 不知道是參考型別，上述的轉換會分類為「取消裝箱」轉換（[取消裝箱轉換](conversions.md#unboxing-conversions)）。

上述規則不允許從不受限制的型別參數直接明確轉換成非介面型別，這可能會令人驚訝。 此規則的原因是為了避免混淆，並讓這類轉換的語法清楚明瞭。 例如，請參考下列宣告：
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)t;                // Error 
    }
}
```

如果允許 `t` 直接明確轉換為 `int`，則可能會輕易地預期 `X<int>.F(7)` 會傳回 `7L`。 不過，它不會這麼做，因為只有在系結時間已知為數值的類型時，才會考慮標準數值轉換。 為了讓語法清楚明瞭，必須改為撰寫上述範例：
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)(object)t;        // Ok, but will only work when T is long
    }
}
```

這段程式碼現在會進行編譯，但執行 `X<int>.F(7)` 接著會在執行時間擲回例外狀況，因為無法直接將已封裝的 `int` 轉換成 `long`。

### <a name="user-defined-explicit-conversions"></a>使用者定義的明確轉換

使用者定義的明確轉換是由選擇性的標準明確轉換所組成，接著執行使用者定義的隱含或明確轉換運算子，再接著另一個選擇性的標準明確轉換。 用於評估使用者定義明確轉換的確切規則，將在[處理使用者定義的明確轉換](conversions.md#processing-of-user-defined-explicit-conversions)中說明。

## <a name="standard-conversions"></a>標準轉換

標準轉換是那些預先定義的轉換，可能會在使用者定義的轉換中發生。

### <a name="standard-implicit-conversions"></a>標準隱含轉換

下列隱含轉換會分類為標準隱含轉換：

*  身分識別轉換（身分[識別轉換](conversions.md#identity-conversion)）
*  隱含數值轉換（[隱含數值轉換](conversions.md#implicit-numeric-conversions)）
*  隱含可為 null 的轉換（[隱含的可為 null 轉換](conversions.md#implicit-nullable-conversions)）
*  隱含參考轉換（[隱含參考轉換](conversions.md#implicit-reference-conversions)）
*  裝箱轉換（[裝箱轉換](conversions.md#boxing-conversions)）
*  隱含常數運算式轉換（[隱含動態轉換](conversions.md#implicit-dynamic-conversions)）
*  涉及型別參數的隱含轉換（[涉及型別參數的隱含轉換](conversions.md#implicit-conversions-involving-type-parameters)）

標準隱含轉換會特別排除使用者定義的隱含轉換。

### <a name="standard-explicit-conversions"></a>標準明確轉換

標準明確轉換是標準隱含轉換，加上相反標準隱含轉換存在之明確轉換的子集。 換句話說，如果從型別 `A` 到型別 `B`的標準隱含轉換，則從型別 `A` 到型別 `B` 以及從型別 `B` 到型別 `A`都有標準的明確轉換。

## <a name="user-defined-conversions"></a>使用者定義轉換

C#允許透過***使用者定義的轉換***來增強預先定義的隱含和明確轉換。 使用者定義的轉換是藉由在類別和結構類型中宣告轉換運算子（[轉換運算子](classes.md#conversion-operators)）來引入。

### <a name="permitted-user-defined-conversions"></a>允許的使用者定義轉換

C#只允許宣告特定使用者定義的轉換。 特別是，您無法重新定義已經存在的隱含或明確轉換。

對於給定的來源類型 `S` 和目標型別 `T`，如果 `S` 或 `T` 是可為 null 的類型，請讓 `S0` 和 `T0` 參考其基礎類型，否則 `S0` 和 `T0` 會分別等於 `S` 和 `T`。 只有在下列所有條件都成立時，才能使用類別或結構來宣告從來源類型 `S` 至目標型別的轉換 `T`：

*  `S0` 和 `T0` 是不同的類型。
*  `S0` 或 `T0` 是發生運算子宣告的類別或結構類型。
*  `S0` 或 `T0` 都不是*interface_type*。
*  除了使用者定義的轉換之外，從 `S` 到 `T` 或從 `T` 到 `S`的轉換不存在。

適用于使用者定義轉換的限制會在[轉換運算子](classes.md#conversion-operators)中進一步討論。

### <a name="lifted-conversion-operators"></a>提升的轉換運算子

假設使用者定義的轉換運算子從不可為 null 的實值型別轉換 `S` 到不可為 null 的實值型別 `T`，則會有一個從 `S?` 轉換成 `T?`的***提升轉換運算子***。 這個提升轉換運算子會從 `S?` 解除包裝到 `S` 後面接著使用者定義的從 `S` 轉換成 `T`，然後再從 `T` 換成 `T?`，但是 null 值 `S?` 會直接轉換成 null 值 `T?`。

提升轉換運算子與其基礎使用者定義的轉換運算子具有相同的隱含或明確分類。 「使用者定義轉換」一詞適用于使用者定義和提升轉換運算子的使用。

### <a name="evaluation-of-user-defined-conversions"></a>評估使用者定義的轉換

使用者定義的轉換會將其類型的值（稱為「***來源類型***」）轉換成另一種類型，稱為「***目標型別」（target type***）。 評估使用者定義的轉換中心，以尋找特定來源和目標型別的***最特定***使用者定義轉換運算子。 這項判斷分為數個步驟：

*  尋找將視為使用者定義轉換運算子的一組類別和結構。 這個集合是由來源類型及其基類和目標型別及其基類所組成（隱含假設只有類別和結構可以宣告使用者定義的運算子，而且該非類別的類型沒有基類）。 基於此步驟的目的，如果來源或目標型別為*nullable_type*，則會改用其基礎類型。
*  從該類型集合中，判斷適用的使用者定義和提升轉換運算子。 若要讓轉換運算子適用，必須從來源類型執行標準轉換（[標準](conversions.md#standard-conversions)轉換）為運算子的運算元類型，而且必須能夠從運算子的結果型別執行標準轉換至目標型別。
*  從一組適用的使用者定義運算子，判斷哪一個運算子明確是最明確的。 一般來說，最特定的運算子是運算子，其運算元類型會「最接近」來源類型，且其結果類型會「最接近」目標型別。 使用者定義的轉換運算子優先于提升的轉換運算子。 建立最特定使用者定義轉換運算子的確切規則定義于下列各節中。

一旦識別出最特定的使用者定義轉換運算子之後，實際執行的使用者定義轉換就會牽涉到三個步驟：

*  首先，如有必要，執行從來源類型到使用者定義或提升轉換運算子之運算元類型的標準轉換。
*  接下來，叫用使用者定義或提升的轉換運算子，以執行轉換。
*  最後，如有必要，從使用者定義或提升轉換運算子的結果型別執行標準轉換至目標型別。

評估使用者定義的轉換絕不會牽涉到一個以上的使用者定義或提升轉換運算子。 換句話說，從類型 `S` 到類型 `T` 的轉換永遠不會先從 `X` `S` 執行使用者定義的轉換，然後再從 `X` 執行使用者定義的轉換至 `T`。

下列各節提供使用者定義隱含或明確轉換的確切評估定義。 定義會使用下列詞彙：

*  如果標準隱含轉換（[標準隱含](conversions.md#standard-implicit-conversions)轉換）存在於類型 `A` `B`，且 `A` 或 `B` 都不是*interface_type*s，則 `A` 會被視為***包含***`B`，而 `B` 則稱為***包含***`A`。
*  一組類型中最包含的***類型***，是一種包含集合中所有其他類型的類型。 如果沒有單一類型包含所有其他類型，則該集合沒有最多包含的類型。 更直覺的說，最包含的型別是集合中的「最大」型別，也就是每個其他型別都可以隱含轉換成的一種類型。
*  一組類型中***最包含的類型***，就是集合中所有其他類型所包含的一種類型。 如果沒有任何其他類型包含任何單一類型，則該集合不會包含大部分包含的類型。 更直覺的說，最包含的型別是集合中的「最小」型別，也就是可以隱含地轉換成每個其他型別的型別。

### <a name="processing-of-user-defined-implicit-conversions"></a>使用者定義隱含轉換的處理

從類型 `S` 到類型 `T` 的使用者定義隱含轉換，會依照下列方式處理：

*  判斷 `S0` 和 `T0`的類型。 如果 `S` 或 `T` 是可為 null 的類型，`S0` 和 `T0` 是其基礎類型，否則 `S0` 和 `T0` 分別等於 `S` 和 `T`。
*  尋找要將使用者定義的轉換運算子視為其來源的類型集合（`D`）。 這個集合包含 `S0` （如果 `S0` 是類別或結構）、`S0` 的基類（如果 `S0` 是類別），以及 `T0` （如果 `T0` 是類別或結構）。
*  尋找一組適用的使用者定義和提升轉換運算子，`U`。 這個集合是由 `D` 中的類別或結構所宣告的使用者定義和提升隱含轉換運算子所組成，而這些運算式會從包含 `S` 的類型轉換成由 `T`所組成的類型。 如果 `U` 是空的，則轉換會是未定義的，而且會發生編譯時期錯誤。
*  尋找 `U`中運算子的最特定來源類型 `SX`：
    * 如果 `U` 中的任何運算子從 `S`轉換，則 `SX` 會 `S`。
    * 否則，`SX` 是 `U`中運算子的結合一組來源類型的最包含類型。 如果找不到其中一個最包含的類型，則轉換會是不明確的，而且會發生編譯時期錯誤。
*  找出 `U`中運算子的最特定目標型別 `TX`：
    * 如果 `U` 中的任何運算子轉換成 `T`，則 `TX` 會 `T`。
    * 否則，`TX` 是 `U`之運算子的一組目標型別中最包含的類型。 如果找不到一個最包含的類型，則轉換會是不明確的，而且會發生編譯時期錯誤。
*  尋找最特定的轉換運算子：
    * 如果 `U` 只包含一個從 `SX` 轉換為 `TX`的使用者定義轉換運算子，則這是最特定的轉換運算子。
    * 否則，如果 `U` 只包含一個從 `SX` 轉換為 `TX`的提升轉換運算子，則這是最特定的轉換運算子。
    * 否則，轉換會是不明確的，而且會發生編譯時期錯誤。
*  最後，套用轉換：
    * 如果未 `SX``S`，則會執行從 `S` 到 `SX` 的標準隱含轉換。
    * 叫用最特定的轉換運算子，以從 `SX` 轉換成 `TX`。
    * 如果未 `T``TX`，則會執行從 `TX` 到 `T` 的標準隱含轉換。

### <a name="processing-of-user-defined-explicit-conversions"></a>使用者定義的明確轉換的處理

從類型 `S` 到類型 `T` 的使用者定義明確轉換會依照下列方式處理：

*  判斷 `S0` 和 `T0`的類型。 如果 `S` 或 `T` 是可為 null 的類型，`S0` 和 `T0` 是其基礎類型，否則 `S0` 和 `T0` 分別等於 `S` 和 `T`。
*  尋找要將使用者定義的轉換運算子視為其來源的類型集合（`D`）。 這個集合包含 `S0` （如果 `S0` 是類別或結構）、`S0` 的基類（如果 `S0` 是類別）、`T0` （如果 `T0` 是類別或結構），以及 `T0` 的基類（如果 `T0` 是類別）。
*  尋找一組適用的使用者定義和提升轉換運算子，`U`。 這個集合是由 `D` 中的類別或結構所宣告的使用者定義和提升的隱含或明確轉換運算子所組成，而這些方法會從包含或包含在 `S` 的類型轉換成包含或包含 `T`的類型。 如果 `U` 是空的，則轉換會是未定義的，而且會發生編譯時期錯誤。
*  尋找 `U`中運算子的最特定來源類型 `SX`：
    * 如果 `U` 中的任何運算子從 `S`轉換，則 `SX` 會 `S`。
    * 否則，如果 `U` 中的任何運算子從包含 `S`的類型進行轉換，則 `SX` 是這些運算子的一組來源類型中最包含的類型。 如果找不到最包含的類型，則轉換會不明確，且會發生編譯時期錯誤。
    * 否則，`SX` 是 `U`中運算子的結合一組來源類型的最包含類型。 如果找不到一個最包含的類型，則轉換會是不明確的，而且會發生編譯時期錯誤。
*  找出 `U`中運算子的最特定目標型別 `TX`：
    * 如果 `U` 中的任何運算子轉換成 `T`，則 `TX` 會 `T`。
    * 否則，如果 `U` 中的任何運算子轉換成 `T`所包含的類型，則 `TX` 是這些運算子的一組目標型別中最包含的類型。 如果找不到一個最包含的類型，則轉換會是不明確的，而且會發生編譯時期錯誤。
    * 否則，`TX` 是 `U`中運算子的一組目標型別的最包含類型。 如果找不到最包含的類型，則轉換會不明確，且會發生編譯時期錯誤。
*  尋找最特定的轉換運算子：
    * 如果 `U` 只包含一個從 `SX` 轉換為 `TX`的使用者定義轉換運算子，則這是最特定的轉換運算子。
    * 否則，如果 `U` 只包含一個從 `SX` 轉換為 `TX`的提升轉換運算子，則這是最特定的轉換運算子。
    * 否則，轉換會是不明確的，而且會發生編譯時期錯誤。
*  最後，套用轉換：
    * 如果未 `SX``S`，則會執行從 `S` 到 `SX` 的標準明確轉換。
    * 叫用最特定的使用者定義轉換運算子，以從 `SX` 轉換成 `TX`。
    * 如果未 `T``TX`，則會執行從 `TX` 到 `T` 的標準明確轉換。

## <a name="anonymous-function-conversions"></a>匿名函數轉換

*Anonymous_method_expression*或*lambda_expression*會分類為匿名函式（[匿名函數運算式](expressions.md#anonymous-function-expressions)）。 運算式沒有類型，但可以隱含地轉換成相容的委派類型或運算式樹狀架構類型。 具體而言，匿名函數 `F` 與 `D` 提供的委派類型相容：

*  如果 `F` 包含*anonymous_function_signature*，則 `D` 和 `F` 的參數數目相同。
*  如果 `F` 不包含*anonymous_function_signature*，則 `D` 可能會有任何類型的零或多個參數，只要 `D` 的參數沒有 `out` 參數修飾詞。
*  如果 `F` 有明確類型的參數清單，則 `D` 中的每個參數都與 `F`中的對應參數具有相同的類型和修飾詞。
*  如果 `F` 有隱含類型的參數清單，`D` 就不會有 `ref` 或 `out` 參數。
*  如果 `F` 的主體是運算式，而且 `D` 有 `void` 傳回型別或 `F` 是非同步，而且 `D` 的傳回型別 `Task`，則當 `F` 的每個參數都指定 `D`中對應參數的型別時，`F` 的主體就是一個有效的運算式（wrt[運算式](expressions.md)），允許做為*statement_expression* （[expression 語句](statements.md#expression-statements)）。
*  如果 `F` 的主體是語句區塊，而且 `D` 有 `void` 傳回類型或 `F` 是非同步，而且 `D` 具有傳回類型 `Task`，則當 `F` 的每個參數都指定 `D`中對應參數的類型時，`F` 的主體就是有效的語句區塊[（wrt 組塊](statements.md#blocks)），其中沒有任何 `return` 語句指定運算式。
*  如果 `F` 的主體是運算式，*而且 `F` 是*非非同步，而且 `D` 具有非 void 的傳回類型 `T`，*或*`F` 是非同步，而且 `D` 的傳回類型 `Task<T>`，則 `F` 的主體會是有效的運算式（wrt[運算式](expressions.md)），可以隱含地轉換成 `D`中的對應參數。
*  如果 `F` 的主體為語句區塊，且 `F` 為非非同步 *，而且 `D`* 具有非 void 的傳回類型 `T`，*或*`F` 是非同步，`D` 有 `Task<T>`的傳回型別，則當 `F` 的每個參數都指定 `D`中對應參數的型別時，`F` 的主體就是有效的語句區塊[（wrt 組塊](statements.md#blocks)），其中每個 `return` 語句都會指定一個可隱含轉換成 `T`的運算式。

為了簡潔起見，本節使用工作類型的簡短形式 `Task` 和 `Task<T>` （[非同步](classes.md#async-functions)函式）。

如果 `F` 與 `D`的委派類型相容，則 lambda 運算式 `F` 與運算式樹狀架構類型相容 `Expression<D>`。 請注意，這不會套用到匿名方法，只適用于 lambda 運算式。

某些 lambda 運算式無法轉換成運算式樹狀架構類型：雖然轉換*存在*，但它會在編譯時期失敗。 如果是 lambda 運算式，就會發生這種情況：

*  具有*區塊*主體
*  包含簡單或複合指派運算子
*  包含動態繫結運算式
*  為 async

接下來的範例會使用泛型委派型別，`Func<A,R>` 代表接受 `A` 型別之引數的函式，並傳回 `R`型別的值：
```csharp
delegate R Func<A,R>(A arg);
```

在指派中
```csharp
Func<int,int> f1 = x => x + 1;                 // Ok

Func<int,double> f2 = x => x + 1;              // Ok

Func<double,int> f3 = x => x + 1;              // Error

Func<int, Task<int>> f4 = async x => x + 1;    // Ok
```
每個匿名函式的參數和傳回型別，都是根據指派匿名函式的變數類型來決定。

第一個指派會成功將匿名函式轉換為委派類型 `Func<int,int>` 因為，當 `x` 是指定的類型 `int`時，`x+1` 是可隱含轉換成類型 `int`的有效運算式。

同樣地，第二個指派會成功將匿名函式轉換為委派類型 `Func<int,double>` 因為 `x+1` （類型 `int`）的結果會隱含地轉換成類型 `double`。

不過，第三個指派是編譯時期錯誤，因為當 `x` 的類型 `double`時，`x+1` （類型 `double`）的結果不會隱含地轉換成類型 `int`。

第四個指派會成功將匿名非同步函式轉換為委派類型 `Func<int, Task<int>>` 因為 `x+1` （類型 `int`）的結果會隱含地轉換成工作類型 `Task<int>`的結果類型 `int`。

匿名函數可能會影響多載解析，並參與型別推斷。 如需進一步的詳細資料，請參閱[函數成員](expressions.md#function-members)。

### <a name="evaluation-of-anonymous-function-conversions-to-delegate-types"></a>評估委派類型的匿名函式轉換

將匿名函式轉換成委派類型時，會產生參考匿名函式的委派實例，以及在評估時作用中的已捕捉外部變數（可能是空的）集。 叫用委派時，會執行匿名函式的主體。 主體中的程式碼會使用委派所參考的一組捕捉外部變數來執行。

從匿名函式產生之委派的調用清單包含單一專案。 未指定委派的確切目標物件和目標方法。 特別是，它不會指定委派的目標物件是 `null`、封入函式成員的 `this` 值，或其他物件。

允許（但非必要）將具有相同（可能是空的）集的匿名函數轉換成相同的委派類型，以傳回相同的委派實例。 這裡使用「語義相同」一詞，這表示在所有情況下，匿名函式的執行會產生相同的效果，並提供相同的引數。 此規則允許將下列程式碼優化。

```csharp
delegate double Function(double x);

class Test
{
    static double[] Apply(double[] a, Function f) {
        double[] result = new double[a.Length];
        for (int i = 0; i < a.Length; i++) result[i] = f(a[i]);
        return result;
    }

    static void F(double[] a, double[] b) {
        a = Apply(a, (double x) => Math.Sin(x));
        b = Apply(b, (double y) => Math.Sin(y));
        ...
    }
}
```

由於這兩個匿名函式委派具有相同（空白）的已捕捉外部變數集，而且因為匿名函式在語義上相同，所以允許編譯器讓委派參考相同的目標方法。 事實上，編譯器允許從這兩個匿名函式運算式傳回非常相同的委派實例。

### <a name="evaluation-of-anonymous-function-conversions-to-expression-tree-types"></a>評估匿名函數轉換為運算式樹狀架構類型

將匿名函式轉換成運算式樹狀架構類型會產生運算式樹狀架構（[運算式樹狀架構類型](types.md#expression-tree-types)）。 更精確地說，匿名函式轉換的評估會導致物件結構的結構表示匿名函數本身的結構。 運算式樹狀架構的精確結構，以及建立它的確切程式，都是定義的執行。

### <a name="implementation-example"></a>實作範例

本節描述以其他C#結構為依據的匿名函式轉換的可能執行。 這裡所述的執行是以 Microsoft C#編譯器所使用的相同原則為基礎，但它並不是強制執行的，也不是唯一可行的方法。 它只會簡短提及運算式樹狀架構的轉換，因為它們的確切語義超出此規格的範圍。

本節的其餘部分會提供幾個程式碼範例，其中包含具有不同特性的匿名函式。 針對每個範例，會提供只使用其他C#結構之程式碼的對應轉譯。 在範例中，會假設識別碼 `D` 代表下列委派類型：
```csharp
public delegate void D();
```

匿名函式的最簡單形式是不會捕捉到外部變數的函數：
```csharp
class Test
{
    static void F() {
        D d = () => { Console.WriteLine("test"); };
    }
}
```

這可以轉譯成委派具現化，該實例會參考編譯器產生的靜態方法，其中會放置匿名函式的程式碼：
```csharp
class Test
{
    static void F() {
        D d = new D(__Method1);
    }

    static void __Method1() {
        Console.WriteLine("test");
    }
}
```

在下列範例中，匿名函數會參考 `this`的實例成員：
```csharp
class Test
{
    int x;

    void F() {
        D d = () => { Console.WriteLine(x); };
    }
}
```

這可以轉譯為編譯器產生的實例方法，其中包含匿名函式的程式碼：
```csharp
class Test
{
    int x;

    void F() {
        D d = new D(__Method1);
    }

    void __Method1() {
        Console.WriteLine(x);
    }
}
```

在此範例中，匿名函式會捕捉本機變數：
```csharp
class Test
{
    void F() {
        int y = 123;
        D d = () => { Console.WriteLine(y); };
    }
}
```

本機變數的存留期現在必須至少延伸至匿名函式委派的存留期。 這可以藉由將本機變數「hoisting」到編譯器所產生類別的欄位來達成。 區域變數的具現化（[區域變數的](expressions.md#instantiation-of-local-variables)具現化）接著會對應到建立編譯器所產生類別的實例，而存取區域變數會對應到在編譯器產生之類別的實例中存取欄位。 此外，匿名函式會成為編譯器所產生類別的實例方法：
```csharp
class Test
{
    void F() {
        __Locals1 __locals1 = new __Locals1();
        __locals1.y = 123;
        D d = new D(__locals1.__Method1);
    }

    class __Locals1
    {
        public int y;

        public void __Method1() {
            Console.WriteLine(y);
        }
    }
}
```

最後，下列匿名函式會捕捉 `this`，以及兩個具有不同存留期的本機變數：
```csharp
class Test
{
    int x;

    void F() {
        int y = 123;
        for (int i = 0; i < 10; i++) {
            int z = i * 2;
            D d = () => { Console.WriteLine(x + y + z); };
        }
    }
}
```

在這裡，會針對每個語句區塊建立編譯器產生的類別，其中會針對其中的區域變數進行攔截，讓不同區塊中的區域變數可以有獨立的存留期。 `__Locals2`的實例（內部語句區塊的編譯器產生的類別）包含區域變數 `z` 和參考 `__Locals1`實例的欄位。  `__Locals1`的實例，這是外部語句區塊的編譯器產生的類別，其中包含區域變數 `y` 和參考封入函式成員 `this` 的欄位。 使用這些資料結構，可以透過 `__Local2`的實例來觸及所有已捕捉的外部變數，而匿名函式的程式碼則可實作為該類別的實例方法。

```csharp
class Test
{
    void F() {
        __Locals1 __locals1 = new __Locals1();
        __locals1.__this = this;
        __locals1.y = 123;
        for (int i = 0; i < 10; i++) {
            __Locals2 __locals2 = new __Locals2();
            __locals2.__locals1 = __locals1;
            __locals2.z = i * 2;
            D d = new D(__locals2.__Method1);
        }
    }

    class __Locals1
    {
        public Test __this;
        public int y;
    }

    class __Locals2
    {
        public __Locals1 __locals1;
        public int z;

        public void __Method1() {
            Console.WriteLine(__locals1.__this.x + __locals1.y + z);
        }
    }
}
```

將匿名函式轉換為運算式樹狀架構時，也可以使用此處所套用的相同技術來捕捉區域變數：編譯器所產生之物件的參考可以儲存在運算式樹狀架構中，而本機變數的存取可以是表示為這些物件上的欄位存取。 這種方法的優點是，它允許在委派和運算式樹狀結構之間共用「提升」的區域變數。

## <a name="method-group-conversions"></a>方法群組轉換

隱含轉換（[隱含轉換](conversions.md#implicit-conversions)）會從方法群組（[運算式分類](expressions.md#expression-classifications)）存在至相容的委派類型。 假設委派類型 `D` 和分類為方法群組的運算式 `E`，如果 `E` 包含至少一個適用于其正常形式（適用于函式[成員](expressions.md#applicable-function-member)）的方法，並使用 `D`的參數類型和修飾詞所建立的引數清單（如下列所述），則隱含轉換會從 `E` 到 `D`。

從方法群組 `E` 至委派類型 `D` 之轉換的編譯時期應用程式如下所述。 請注意，從 `E` 到 `D` 的隱含轉換並不會保證轉換的編譯時期應用程式將會成功，而不會發生錯誤。

*  選取單一方法 `M`，其對應至表單 `E(A)`的方法調用（[方法調用](expressions.md#method-invocations)），但有下列修改：
    * 引數清單 `A` 是運算式的清單，每一個都會分類為一個變數，並在 `D`的*formal_parameter_list*中，使用對應參數的類型和修飾詞（`ref` 或 `out`）。
    * 所考慮的候選方法是僅適用于其一般格式（適用函式[成員](expressions.md#applicable-function-member)）的方法，而不是只適用于其擴充形式的方法。
*  如果[方法調用](expressions.md#method-invocations)的演算法產生錯誤，則會發生編譯時期錯誤。 否則，此演算法會產生一個最佳方法，`M` 具有與 `D` 相同數目的參數，並將轉換視為存在。
*  選取的方法 `M` 必須與委派類型 `D`相容（[委派相容性](delegates.md#delegate-compatibility)），否則會發生編譯時期錯誤。
*  如果選取的方法 `M` 是實例方法，則與 `E` 相關聯的實例運算式會決定委派的目標物件。
*  如果選取的方法 M 是透過實例運算式上的成員存取所表示的擴充方法，則該實例運算式會決定委派的目標物件。
*  轉換的結果是 `D`類型的值，也就是參考所選方法和目標物件的新建立委派。
*  請注意，如果[方法調用](expressions.md#method-invocations)的演算法找不到實例方法，但卻成功將 `E(A)` 的調用當做擴充方法調用（[擴充方法](expressions.md#extension-method-invocations)調用）處理，則此程式可能會導致建立擴充方法的委派。 因此，建立的委派會捕捉擴充方法以及其第一個引數。

下列範例示範方法群組轉換：
```csharp
delegate string D1(object o);

delegate object D2(string s);

delegate object D3();

delegate string D4(object o, params object[] a);

delegate string D5(int i);

class Test
{
    static string F(object o) {...}

    static void G() {
        D1 d1 = F;            // Ok
        D2 d2 = F;            // Ok
        D3 d3 = F;            // Error -- not applicable
        D4 d4 = F;            // Error -- not applicable in normal form
        D5 d5 = F;            // Error -- applicable but not compatible

    }
}
```

`d1` 的指派會將方法群組 `F` 隱含地轉換為 `D1`類型的值。

[指派給 `d2`] 會顯示如何建立委派給較少衍生（逆變）參數類型的方法，以及更多衍生的（協變數）傳回型別。

指派給 `d3` 會顯示如果方法不適用，則不會有任何轉換存在。

[指派給 `d4`] 會顯示方法在其一般格式中的運作方式。

`d5` 的指派會顯示委派和方法的參數和傳回型別，如何只針對引用型別而有所不同。

如同所有其他的隱含和明確轉換，cast 運算子可以用來明確地執行方法群組轉換。 因此，範例
```csharp
object obj = new EventHandler(myDialog.OkClick);
```
可以改為寫入
```csharp
object obj = (EventHandler)myDialog.OkClick;
```

方法群組可能會影響多載解析，並參與型別推斷。 如需進一步的詳細資料，請參閱[函數成員](expressions.md#function-members)。

方法群組轉換的執行時間評估會繼續進行，如下所示：

*  如果在編譯時期選取的方法是實例方法，或者它是當做實例方法存取的擴充方法，則會從與 `E`相關聯的實例運算式來決定委派的目標物件：
    * 實例運算式會進行評估。 如果此評估導致例外狀況，則不會執行任何進一步的步驟。
    * 如果實例運算式是*reference_type*，實例運算式所計算的值就會成為目標物件。 如果選取的方法是實例方法，而目標物件是 `null`，就會擲回 `System.NullReferenceException`，而且不會執行任何進一步的步驟。
    * 如果實例運算式是*value_type*，則會執行「裝箱」作業（「[裝箱」轉換](types.md#boxing-conversions)）來將值轉換為物件，而此物件會成為目標物件。
*  否則，選取的方法是靜態方法呼叫的一部分，而委派的目標物件則是 `null`。
*  已配置 `D` 委派類型的新實例。 如果沒有足夠的記憶體可配置新的實例，則會擲回 `System.OutOfMemoryException`，且不會執行任何進一步的步驟。
*  新的委派實例會使用在編譯時期決定的方法參考，以及上述所計算之目標物件的參考，進行初始化。
