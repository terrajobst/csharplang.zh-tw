---
ms.openlocfilehash: 61eeae6173eaa19f9cf6d6e985f3dc107d4c3ac9
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488950"
---
# <a name="conversions"></a>轉換

A***轉換***可讓運算式被視為特定的型別。 轉換可能會造成指定的型別被視為擁有不同類型的運算式，或可能會導致沒有取得型別之型別的運算式。 轉換十分***隱含***或***明確***，這會決定是否需要明確轉換。 比方說，從類型轉換`int`鍵入`long`是隱含的因此運算式的型別`int`隱含地視為類型`long`。 相反的轉換，從型別`long`輸入`int`，是明確，因此需要明確轉換。

```csharp
int a = 123;
long b = a;         // implicit conversion from int to long
int c = (int) b;    // explicit conversion from long to int
```

部分轉換是由語言定義。 程式也可以定義自己的轉換 ([使用者定義轉換](conversions.md#user-defined-conversions))。

## <a name="implicit-conversions"></a>隱含轉換

下列轉換會歸類為隱含轉換：

*  身分識別轉換
*  隱含數值轉換
*  隱含的列舉型別轉換。
*  可為 null 的隱含轉換
*  Null 常值轉換
*  隱含參考轉換
*  Boxing 轉換
*  動態的隱含轉換
*  隱含的常數運算式轉換
*  使用者定義的隱含轉換
*  匿名函式轉換
*  方法群組轉換

隱含轉換可能會發生各種不同的情況下，包括函式成員引動過程 ([編譯時期檢查動態的多載解析](expressions.md#compile-time-checking-of-dynamic-overload-resolution))，轉換運算式 ([轉型運算式](expressions.md#cast-expressions))，和指派 ([指派運算子](expressions.md#assignment-operators))。

預先定義的隱含轉換永遠成功，而且永遠不會造成擲回的例外狀況。 設計得當的以使用者定義的隱含轉換，應該會表現出以及這些特性。

類型的轉換，基於`object`和`dynamic`都視為相等。

不過，動態轉換 ([動態的隱含轉換](conversions.md#implicit-dynamic-conversions)並[明確的動態轉換](conversions.md#explicit-dynamic-conversions)) 只適用於運算式的型別`dynamic`([動態型別](types.md#the-dynamic-type)).

### <a name="identity-conversion"></a>身分識別轉換

身分識別轉換將從任何類型轉換成相同的型別。 這項轉換存在，可說是實體已經有必要的型別可轉換成該類型。

*  因為物件及動態都視為相等，所以沒有身分識別轉換之間`object`及`dynamic`，並取代所有出現時，都是相同的建構型別之間`dynamic`與`object`。

### <a name="implicit-numeric-conversions"></a>隱含數值轉換

隱含數值轉換為：

*  從`sbyte`要`short`， `int`， `long`， `float`， `double`，或`decimal`。
*  從`byte`要`short`， `ushort`， `int`， `uint`， `long`， `ulong`， `float`， `double`，或`decimal`。
*  從`short`要`int`， `long`， `float`， `double`，或`decimal`。
*  從`ushort`要`int`， `uint`， `long`， `ulong`， `float`， `double`，或`decimal`。
*  從`int`要`long`， `float`， `double`，或`decimal`。
*  從`uint`要`long`， `ulong`， `float`， `double`，或`decimal`。
*  從`long`要`float`， `double`，或`decimal`。
*  從`ulong`要`float`， `double`，或`decimal`。
*  從`char`要`ushort`， `int`， `uint`， `long`， `ulong`， `float`， `double`，或`decimal`。
*  從`float`至`double`。

從不`int`， `uint`， `long`，或`ulong`來`float`進出`long`或`ulong`至`double`可能會導致失去精確度，但永遠不會造成又級距性遺失。 其他隱含數值轉換不會遺失任何資訊。

有沒有隱含轉換`char`型別，因此其他整數類資料類型的值無法自動轉換成`char`型別。

### <a name="implicit-enumeration-conversions"></a>隱含的列舉型別轉換

允許隱含的列舉型別轉換*decimal_integer_literal* `0`轉換成任何*enum_type*和任何*nullable_type*其基礎類型是*enum_type*。 在後者的情況，則轉換會評估轉換至基礎*enum_type*換行的結果 ([可為 Null 的型別](types.md#nullable-types))。

### <a name="implicit-interpolated-string-conversions"></a>隱含的字串插值的轉換

隱含的插補字串轉換允許*interpolated_string_expression* ([插補字串](expressions.md#interpolated-strings)) 轉換成`System.IFormattable`或`System.FormattableString`（它會實作`System.IFormattable`).

當套用這項轉換的字串值不被由字串插值。 改為執行個體`System.FormattableString`是建立中，將將進一步說明[插補字串](expressions.md#interpolated-strings)。

### <a name="implicit-nullable-conversions"></a>可為 null 的隱含轉換

預先定義的隱含轉換對不可為 null 的實值型別也可以搭配這些類型可為 null 的表單。 針對每個預先定義隱含的身分識別和從非可為 null 的實值型別轉換的數值轉換`S`不可為 null 的實值型別的`T`，下列隱含可為 null 的轉換：

*  從的隱含轉換`S?`至`T?`。
*  從的隱含轉換`S`至`T?`。

評估可為 null 的隱含轉換為基礎的基礎轉換`S`至`T`程序如下：

*  如果可為 null 的轉換是來自`S?`至`T?`:
    * 如果來源值為 null (`HasValue`屬性為 false)，結果就是 null 值的型別`T?`。
    * 否則，則轉換會評估為從`S?`來`S`，後面接著從基礎轉換`S`來`T`，後面接著換行 ([可為 Null 的型別](types.md#nullable-types)) 從`T`至`T?`。

*  如果可為 null 的轉換是來自`S`來`T?`，則轉換會評估為基礎的轉換，從`S`來`T`後面接著從繞`T`來`T?`。

### <a name="null-literal-conversions"></a>Null 常值轉換

隱含的轉換`null`常值至任何可為 null 的型別。 這項轉換會產生 null 值 ([可為 Null 的型別](types.md#nullable-types)) 指定可為 null 的類型。

### <a name="implicit-reference-conversions"></a>隱含參考轉換

隱含參考轉換為：

*  從任何*reference_type*要`object`和`dynamic`。
*  從任何*class_type* `S`任何*class_type* `T`提供`S`衍生自`T`。
*  從任何*class_type* `S`任何*interface_type* `T`提供`S`實作`T`。
*  從任何*interface_type* `S`任何*interface_type* `T`提供`S`衍生自`T`。
*  從*array_type* `S`項目類型`SE`來*array_type* `T`項目類型`TE`，前提是所有下列其中一項條件成立：
    * `S` 和`T`的差別只在於項目型別。 亦即`S`和`T`有相同的維度數目。
    * 兩者`SE`並`TE`會*reference_type*s。
    * 隱含參考轉換存在`SE`至`TE`。
*  從任何*array_type*到`System.Array`和它所實作的介面。
*  從一維陣列的型別`S[]`要`System.Collections.Generic.IList<T>`和其基底介面，提供從隱含身分識別或參考轉換`S`至`T`。
*  從任何*delegate_type*到`System.Delegate`和它所實作的介面。
*  從任何 null 常值*reference_type*。
*  從任何*reference_type*要*reference_type* `T`有隱含的身分識別或參考轉換為*reference_type* `T0`和`T0`具有身分識別轉換至`T`。
*  從任何*reference_type*介面或委派型別的`T`有隱含的身分識別或參考轉換為介面或委派的型別`T0`和`T0`可轉換變異數 ([變異數轉換](interfaces.md#variance-conversion)) 以`T`。
*  涉及為參考型別的已知型別參數的隱含轉換。 請參閱[涉及型別參數的隱含轉換](conversions.md#implicit-conversions-involving-type-parameters)如需詳細資訊，涉及型別參數的隱含轉換。

隱含參考轉換是指在轉換*reference_type*s 可證實為一律會成功，並因此需要在執行階段沒有任何檢查。

參考轉換，隱含或明確的永遠不會變更轉換的物件參考的識別。 換句話說，雖然參考轉換可能會變更參考的類型，但它絕不會變更型別或所參考之物件的值。

### <a name="boxing-conversions"></a>Boxing 轉換

Boxing 轉換允許*value_type*隱含地轉換成參考型別。 Boxing 轉換存在從任何*non_nullable_value_type*要`object`並`dynamic`至`System.ValueType`和任何*interface_type*藉由將*non_nullable_value_type*。 此外*enum_type*可以轉換成類型`System.Enum`。

Boxing 轉換存在*nullable_type*為參考型別，如果且只有 boxing 轉換存在從基礎*non_nullable_value_type*參考類型。

實值型別具有介面類型的 boxing 轉換`I`是否為介面類型的 boxing 轉換`I0`並`I0`具有身分識別轉換至`I`。

實值型別具有介面類型的 boxing 轉換`I`有介面或委派型別的 boxing 轉換`I0`並`I0`可轉換變異數 ([變異數轉換](interfaces.md#variance-conversion)) 到`I`.

Box 處理的值*non_nullable_value_type*配置的物件執行個體，並複製所組成*value_type*到該執行個體的值。 結構可以成為 boxed 型別`System.ValueType`，因為這是全部為結構的基底類別 ([繼承](structs.md#inheritance))。

Box 處理的值*nullable_type*程序如下：

*  如果來源值為 null (`HasValue`屬性為 false)，結果為 null 參考的目標型別。
*  否則，結果就是參考 boxed 的`T`解除包裝成為和 boxing 來源值產生。

Boxing 轉換中所述進一步[Boxing 轉換](types.md#boxing-conversions)。

### <a name="implicit-dynamic-conversions"></a>動態的隱含轉換

隱含的動態轉換存在類型的運算式`dynamic`任何型別的`T`。 轉換動態繫結 ([動態繫結](expressions.md#dynamic-binding))，這表示，會在執行階段從執行階段類型的運算式來搜尋的隱含轉換`T`。 如果不找到任何轉換，則會擲回執行階段例外狀況。

請注意這項隱含轉換看似違反開頭的建議[隱含轉換](conversions.md#implicit-conversions)隱含的轉換應該永遠不會造成例外狀況。 不過它不是轉換本身，但*尋找*造成例外狀況的轉換。 執行階段例外狀況的風險是可繼承的動態繫結使用。 如果不需要轉換的動態繫結，運算式可以先轉換成`object`，然後將所需的類型。

下列範例說明隱含的動態轉換：

```csharp
object o  = "object"
dynamic d = "dynamic";

string s1 = o; // Fails at compile-time -- no conversion exists
string s2 = d; // Compiles and succeeds at run-time
int i     = d; // Compiles but fails at run-time -- no conversion exists
```

若要指派`s2`和`i`同時採用隱含動態轉換作業的繫結中暫止執行階段。 在執行階段隱含轉換為搜尋的執行階段型別`d`  --  `string` -成目標型別。 轉換發現`string`而無法供`int`。

### <a name="implicit-constant-expression-conversions"></a>隱含的常數運算式轉換

隱含的常數運算式允許下列轉換：

*  A *constant_expression* ([常數運算式](expressions.md#constant-expressions)) 的型別`int`可以轉換為類型`sbyte`， `byte`， `short`， `ushort`， `uint`，或`ulong`，提供的值*constant_expression*目的地類型的範圍內。
*  A *constant_expression*型別的`long`可以轉換為類型`ulong`，提供的值*constant_expression*不是負數。

### <a name="implicit-conversions-involving-type-parameters"></a>包含型別參數的隱含轉換

下列的隱含轉換存在指定的型別參數`T`:

*  從`T`其有效的基底類別`C`，從`T`任何基底類別`C`，以及從`T`實作任何介面`C`。 在執行階段，如果`T`是實值類型，轉換會執行 boxing 轉換。 否則，轉換會執行隱含參考轉換或識別轉換。
*  從`T`為介面型別`I`中`T`設定有效的介面以及從`T`任何基底介面`I`。 在執行階段，如果`T`是實值類型，轉換會執行 boxing 轉換。 否則，轉換會執行隱含參考轉換或識別轉換。
*  從`T`類型參數`U`提供`T`取決於`U`([類型參數條件約束](classes.md#type-parameter-constraints))。 在執行階段，如果`U`是實值類型，則`T`和`U`一定都是相同的型別並不會執行轉換。 否則，如果`T`是實值類型，轉換會執行 boxing 轉換。 否則，轉換會執行隱含參考轉換或識別轉換。
*  Null 常值，以便從`T`提供`T`已知為參考型別。
*  從`T`參考型別`I`是否有參考類型的隱含轉換`S0`並`S0`具有身分識別轉換至`S`。 轉換會在執行階段執行內容轉換成一樣`S0`。
*  從`T`為介面型別`I`有隱含轉換成介面或委派的型別`I0`並`I0`可轉換變異數來`I`([變異數轉換](interfaces.md#variance-conversion)). 在執行階段，如果`T`是實值類型，轉換會執行 boxing 轉換。 否則，轉換會執行隱含參考轉換或識別轉換。

如果`T`已知為參考型別 ([類型參數條件約束](classes.md#type-parameter-constraints))，則上述的轉換會歸類為隱含參考轉換 ([隱含參考轉換](conversions.md#implicit-reference-conversions))。 如果`T`不是知道是參考類型，則上述轉換歸類為 boxing 轉換 ([Boxing 轉換](conversions.md#boxing-conversions))。

### <a name="user-defined-implicit-conversions"></a>使用者定義的隱含轉換

使用者定義的隱含轉換是由選擇性標準隱含的轉換，後面接著執行使用者定義的隱含轉換運算子，後面接著另一個選用標準的隱含轉換所組成。 評估使用者定義的隱含轉換的確切規則所述[處理的使用者定義的隱含轉換](conversions.md#processing-of-user-defined-implicit-conversions)。

### <a name="anonymous-function-conversions-and-method-group-conversions"></a>匿名函式轉換和方法群組轉換

匿名函式和方法群組沒有型別本身，但可能會隱含地轉換成委派類型或運算式樹狀架構類型。 更詳細地說明匿名函式轉換[匿名函式轉換](conversions.md#anonymous-function-conversions)和中的方法群組轉換[方法群組轉換](conversions.md#method-group-conversions)。

## <a name="explicit-conversions"></a>明確轉換

下列轉換會歸類為明確的轉換：

*  所有的隱含轉換。
*  明確數值轉換。
*  明確的列舉型別轉換。
*  明確可為 null 的轉換。
*  明確參考轉換。
*  明確介面轉換。
*  Unboxing 轉換。
*  明確的動態轉換
*  使用者定義的明確轉換。

明確轉換可能會發生在轉型運算式 ([轉型運算式](expressions.md#cast-expressions))。

明確轉換一組包含所有的隱含轉換。 這表示可以重複轉型運算式。

跨網域的差異足以值得一讀明確的型別不是隱含轉換的明確轉換是無法證明一律會成功的轉換，可能會遺失資訊，已知的轉換和轉換標記法。

### <a name="explicit-numeric-conversions"></a>明確數值轉換

明確數值轉換是從不*numeric_type*到另一個*numeric_type* ，隱含數值轉換 ([隱含數值轉換](conversions.md#implicit-numeric-conversions))不存在：

*  從`sbyte`要`byte`， `ushort`， `uint`， `ulong`，或`char`。
*  從`byte`要`sbyte`和`char`。
*  從`short`要`sbyte`， `byte`， `ushort`， `uint`， `ulong`，或`char`。
*  從`ushort`要`sbyte`， `byte`， `short`，或`char`。
*  從`int`要`sbyte`， `byte`， `short`， `ushort`， `uint`， `ulong`，或`char`。
*  從`uint`要`sbyte`， `byte`， `short`， `ushort`， `int`，或`char`。
*  從`long`要`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `ulong`，或`char`。
*  從`ulong`要`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`，或`char`。
*  從`char`要`sbyte`， `byte`，或`short`。
*  從`float`要`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char`，或`decimal`。
*  從`double`要`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char`， `float`，或`decimal`。
*  從`decimal`要`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char`， `float`，或`double`。

因為明確的轉換會包括所有隱含和明確數值轉換，它就一定能夠從任何轉換*numeric_type*任何其他*numeric_type*使用 cast 運算式 （[轉型運算式](expressions.md#cast-expressions))。

明確數值轉換可能會遺失資訊，或可能會導致擲回的例外狀況。 明確數值轉換的處理方式，如下所示：

*  從整數類資料類型轉換為另一種整數類資料類型，處理取決於溢位檢查內容 ([checked 與 unchecked 運算子](expressions.md#the-checked-and-unchecked-operators)) 會轉換在放置：
    * 在 `checked`內容，轉換成功，如果來源運算元的值範圍內的目的型別，但會擲回`System.OverflowException`若來源運算元的值超出目的地類型的範圍。
    * 在 `unchecked`內容，轉換一律會成功，然後繼續進行，如下所示。
        * 如果來源類型大於目的地類型，會藉由捨棄其「額外」最高有效位元，來截斷來源值。 然後會將結果視為目標類型的值。
        * 如果來源類型小於目標類型，則來源值可以是正負號擴充或零擴充，以使其與目標類型的大小相同。 如果來源類型帶正負號，則會使用正負號擴充；如果來源類型不帶正負號，則會使用零擴充。 然後會將結果視為目標類型的值。
        * 如果來源類型與目標類型的大小相同，則來源值將視為目標類型的值。
*  從轉換`decimal`為整數類型，來源值會捨入為最接近的整數值零，而此整數的值會變成轉換的結果。 如果產生的整數值超出目的地類型中，各種`System.OverflowException`就會擲回。
*  從轉換`float`或是`double`為整數類型，處理需視溢位檢查內容 ([checked 與 unchecked 運算子](expressions.md#the-checked-and-unchecked-operators)) 在會轉換：
    * 在 `checked`內容，轉換程序如下：
        * 如果運算元的值是 NaN 或 infinite，`System.OverflowException`就會擲回。
        * 否則，來源運算元會捨入為最接近的整數值零。 如果此整數的值為目的地類型的範圍內這個值會是轉換的結果。
        * 否則會擲回 `System.OverflowException`。
    * 在 `unchecked`內容，轉換一律會成功，然後繼續進行，如下所示。
        * 如果運算元的值是否為 NaN 或無限，轉換的結果是未指定的目的地類型的值。
        * 否則，來源運算元會捨入為最接近的整數值零。 如果此整數的值為目的地類型的範圍內這個值會是轉換的結果。
        * 否則，轉換的結果就是目的地類型的未指定的值。
*  從轉換`double`要`float`，則`double`值會四捨五入為最接近`float`值。 如果`double`值太小而無法表示為`float`，結果變成正零或負零。 如果`double`值太大而無法表示為`float`，結果會成為無限大的正數或負的無限值。 如果`double`值是 NaN，結果也是 NaN。
*  從轉換`float`或`double`來`decimal`，來源值轉換成`decimal`表示法和捨入到最接近的數字第 28 小數點之後如有必要 ([decimal 型別](types.md#the-decimal-type)). 如果來源值太小而無法表示為`decimal`，結果就會變成零。 如果來源值是 NaN、 無限，或太大而無法表示為`decimal`、`System.OverflowException`就會擲回。
*  從轉換`decimal`要`float`或`double`，則`decimal`值會四捨五入為最接近`double`或`float`值。 雖然這種轉換可能會遺失有效位數，絕不會造成擲回例外狀況。

### <a name="explicit-enumeration-conversions"></a>明確的列舉型別轉換

明確的列舉型別轉換為：

*  從`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char`， `float`， `double`，或`decimal`至任何*enum_type*。
*  從任何*enum_type*要`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char`， `float`， `double`，或`decimal`。
*  從任何*enum_type*任何其他*enum_type*。

兩個類型之間的明確的列舉型別轉換處理，是將任何參與*enum_type*為基礎的類型*enum_type*，然後執行隱含或明確產生的型別之間的數字轉換。 例如，假設*enum_type* `E`使用和的基礎類型`int`，從轉換`E`來`byte`處理成明確數值轉換 ([明確數值轉換](conversions.md#explicit-numeric-conversions)) 從`int`要`byte`，和從轉換`byte`來`E`處理時，隱含數值轉換 ([隱含數值轉換](conversions.md#implicit-numeric-conversions))從`byte`至`int`。

### <a name="explicit-nullable-conversions"></a>可為 null 的明確轉換

***可為 null 的明確轉換***允許預先定義作用於不可為 null 的實值型別也可搭配可為 null 的表單，這些類型的明確轉換。 針對每個預先定義的明確轉換，從非可為 null 的實值型別轉換`S`不可為 null 的實值型別的`T`([識別轉換](conversions.md#identity-conversion)，[隱含數值轉換](conversions.md#implicit-numeric-conversions)，[隱含的列舉型別轉換](conversions.md#implicit-enumeration-conversions)，[明確數值轉換](conversions.md#explicit-numeric-conversions)，和[明確的列舉型別轉換](conversions.md#explicit-enumeration-conversions))，下列可為 null 的轉換存在：

*  明確轉換`S?`至`T?`。
*  明確轉換`S`至`T?`。
*  明確轉換`S?`至`T`。

可為 null 的轉換的評估會根據基礎的轉換，從`S`至`T`程序如下：

*  如果可為 null 的轉換是來自`S?`至`T?`:
    * 如果來源值為 null (`HasValue`屬性為 false)，結果就是 null 值的型別`T?`。
    * 否則，則轉換會評估為從`S?`來`S`，後面接著從基礎轉換`S`來`T`，後面接著從繞`T`來`T?`。
*  如果可為 null 的轉換是來自`S`來`T?`，則轉換會評估為基礎的轉換，從`S`來`T`後面接著從繞`T`來`T?`。
*  如果可為 null 的轉換是來自`S?`來`T`，則轉換會評估為從`S?`來`S`後面接著從基礎轉換`S`至`T`。

請注意，嘗試解除包裝的可為 null 的值將會擲回例外狀況的值是否`null`。

### <a name="explicit-reference-conversions"></a>明確參考轉換

明確參考轉換為：

*  從`object`並`dynamic`任何其他*reference_type*。
*  從任何*class_type* `S`任何*class_type* `T`提供`S`的基底類別的`T`。
*  從任何*class_type* `S`任何*interface_type* `T`提供`S`未密封且未提供`S`不會實作`T`。
*  從任何*interface_type* `S`任何*class_type* `T`提供`T`未密封格式，或提供`T`實作`S`。
*  從任何*interface_type* `S`任何*interface_type* `T`提供`S`不衍生自`T`。
*  從*array_type* `S`項目類型`SE`來*array_type* `T`項目類型`TE`，前提是所有下列其中一項條件成立：
    * `S` 和`T`的差別只在於項目型別。 亦即`S`和`T`有相同的維度數目。
    * 兩者`SE`並`TE`會*reference_type*s。
    * 明確參考轉換存在`SE`至`TE`。
*  從`System.Array`介面的方式實作任何*array_type*。
*  從一維陣列的型別`S[]`要`System.Collections.Generic.IList<T>`和其基底介面，提供從明確參考轉換`S`至`T`。
*  從`System.Collections.Generic.IList<S>`和其基底介面為單一維度陣列型別`T[]`，前提是沒有明確的身分識別或參考轉換從`S`至`T`。
*  從`System.Delegate`介面的方式實作任何*delegate_type*。
*  從參考類型的參考型別`T`是否有參考類型的明確參考轉換`T0`並`T0`具有身分識別轉換`T`。
*  從介面或委派型別的參考型別`T`有明確參考轉換為介面或委派的型別`T0`任一個`T0`可轉換變異數來`T`或`T`是變異數可轉換為`T0`([變異數轉換](interfaces.md#variance-conversion))。
*  從`D<S1...Sn>`來`D<T1...Tn>`何處`D<X1...Xn>`是泛型委派類型時，`D<S1...Sn>`不相容於或等於`D<T1...Tn>`，和每個類型參數`Xi`的`D`下列保留：
    * 如果`Xi`為非變異，然後`Si`等同於`Ti`。
    * 如果`Xi`是 covariant，則是隱含或明確識別或參考轉換從`Si`至`Ti`。
    * 如果`Xi`是逆變性，則`Si`和`Ti`都是相同或是兩者都參考型別。
*  涉及為參考型別的已知型別參數的明確轉換。 如需涉及型別參數的明確轉換的詳細資訊，請參閱 <<c0> [ 涉及型別參數的明確轉換](conversions.md#explicit-conversions-involving-type-parameters)。

明確參考轉換，是指需要執行階段檢查，以確保其為正確的參考類型之間轉換。

若要在執行階段成功明確參考轉換，來源運算元的值必須是`null`，或來源運算元所參考的物件的實際型別必須是可以轉換成目的地類型的隱含參考的類型轉換 ([隱含參考轉換](conversions.md#implicit-reference-conversions)) 或 boxing 轉換 ([Boxing 轉換](conversions.md#boxing-conversions))。 如果明確參考轉換失敗，`System.InvalidCastException`就會擲回。

參考轉換，隱含或明確的永遠不會變更轉換的物件參考的識別。 換句話說，雖然參考轉換可能會變更參考的類型，但它絕不會變更型別或所參考之物件的值。

### <a name="unboxing-conversions"></a>Unboxing 轉換

Unboxing 轉換允許明確轉換成參考型別*value_type*。 Unboxing 轉換存在從型別`object`，`dynamic`並`System.ValueType`任何*non_nullable_value_type*，並從任何*interface_type*任何*non_nullable_value_type*可實*interface_type*。 此外鍵入`System.Enum`可以是任何 unboxed *enum_type*。

Unboxing 轉換存在從參考的型別*nullable_type*有 unboxing 轉換的參考型別從基礎*non_nullable_value_type*的*nullable_type*。

實值型別`S`具有 unboxing 轉換將介面類型`I`是否有從介面類型 unboxing 轉換`I0`並`I0`具有身分識別轉換至`I`。

實值型別`S`已從介面類型 unboxing 轉換`I`有 unboxing 轉換從介面或委派的型別`I0`任一個`I0`可轉換變異數來`I`或`I`可轉換變異數來`I0`([變異數轉換](interfaces.md#variance-conversion))。

Unboxing 作業包含第一個檢查的物件執行個體的 boxed 的值的指定*value_type*，然後複製到執行個體的值。 Unboxing 的 null 參考*nullable_type*產生 null 值的*nullable_type*。 結構可以從型別 unboxed `System.ValueType`，因為這是全部為結構的基底類別 ([繼承](structs.md#inheritance))。

Unboxing 轉換中所述進一步[Unboxing 轉換](types.md#unboxing-conversions)。

### <a name="explicit-dynamic-conversions"></a>明確的動態轉換

從類型的運算式存在明確的動態轉換`dynamic`任何型別的`T`。 轉換動態繫結 ([動態繫結](expressions.md#dynamic-binding))，這表示，將會在執行階段從執行階段類型的運算式來搜尋的明確轉換`T`。 如果不找到任何轉換，則會擲回執行階段例外狀況。

如果不需要轉換的動態繫結，運算式可以先轉換成`object`，然後將所需的類型。

假設下列類別的定義：
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

最佳轉換`o`至`C`找到在編譯時期要明確參考轉換。 這會失敗，在執行階段，因為`"1"`不是事實`C`。 轉換`d`要`C`不過，做為明確的動態轉換，則會暫停執行階段，其中使用者定義的執行階段類型的轉換`d`  --  `string` -為`C`找到，便會成功。

### <a name="explicit-conversions-involving-type-parameters"></a>包含型別參數的明確轉換

下列的明確轉換存在指定的型別參數`T`:

*  從有效的基底類別`C`的`T`要`T`並從任何基底類別`C`到`T`。 在執行階段，如果`T`是實值類型，轉換會執行為 unboxing 轉換。 否則，轉換會執行為明確參考轉換或識別轉換。
*  從任何介面類型到`T`。 在執行階段，如果`T`是實值類型，轉換會執行為 unboxing 轉換。 否則，轉換會執行為明確參考轉換或識別轉換。
*  從`T`任何*interface_type* `I`提供的還沒有的隱含轉換`T`至`I`。 在執行階段，如果`T`是實值類型，轉換會執行為 boxing 轉換，後面接著明確參考轉換。 否則，轉換會執行為明確參考轉換或識別轉換。
*  從型別參數`U`要`T`提供`T`取決於`U`([類型參數條件約束](classes.md#type-parameter-constraints))。 在執行階段，如果`U`是實值類型，則`T`和`U`一定都是相同的型別並不會執行轉換。 否則，如果`T`是實值類型，轉換會執行為 unboxing 轉換。 否則，轉換會執行為明確參考轉換或識別轉換。

如果`T`是已知為參考型別，則上述轉換全都歸類為明確參考轉換 ([明確參考轉換](conversions.md#explicit-reference-conversions))。 如果`T`不是知道是參考類型，上述的轉換會歸類為 unboxing 轉換 ([Unboxing 轉換](conversions.md#unboxing-conversions))。

上述規則不允許直接從明確轉換的未受限制的型別參數至非介面的類型，這可能會令人意外。 此規則的原因是避免混淆，這類清除的轉換語意。 例如，請參考下列宣告：
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)t;                // Error 
    }
}
```

如果直接明確轉換`t`要`int`允許的可能會輕易地以為所`X<int>.F(7)`會傳回 `7L`。 不過，它不會，因為已知型別是數值繫結時間時，只會考慮標準的數字轉換。 若要進行語意清楚、 上述的範例必須改為撰寫：
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)(object)t;        // Ok, but will only work when T is long
    }
}
```

此程式碼現在會進行編譯但執行`X<int>.F(7)`會再擲回例外狀況在執行階段自 boxed`int`無法直接轉換成`long`。

### <a name="user-defined-explicit-conversions"></a>使用者定義的明確轉換

使用者定義的明確轉換所組成的選擇性標準明確轉換，後面接著執行使用者定義的隱含或明確轉換運算子，後面接著另一個選用標準的明確轉換。 評估使用者定義的明確轉換的確切規則所述[處理的使用者定義的明確轉換](conversions.md#processing-of-user-defined-explicit-conversions)。

## <a name="standard-conversions"></a>標準轉換

標準的轉換是轉換的預先定義的使用者定義過程中可能發生的。

### <a name="standard-implicit-conversions"></a>標準的隱含轉換

下列的隱含轉換會歸類為標準的隱含轉換：

*  身分識別轉換 ([身分識別轉換](conversions.md#identity-conversion))
*  隱含數值轉換 ([隱含數值轉換](conversions.md#implicit-numeric-conversions))
*  可為 null 的隱含轉換 ([可為 null 的隱含轉換](conversions.md#implicit-nullable-conversions))
*  隱含參考轉換 ([隱含參考轉換](conversions.md#implicit-reference-conversions))
*  Boxing 轉換 ([Boxing 轉換](conversions.md#boxing-conversions))
*  隱含的常數運算式轉換 ([隱含的動態轉換](conversions.md#implicit-dynamic-conversions))
*  包含型別參數的隱含轉換 ([涉及型別參數的隱含轉換](conversions.md#implicit-conversions-involving-type-parameters))

標準的隱含轉換特別排除使用者定義的隱含轉換。

### <a name="standard-explicit-conversions"></a>標準的明確轉換

標準的明確轉換，是所有標準的隱含轉換，再加上存在的相反的標準的隱含轉換的明確轉換的子集。 換句話說，如果隱含的標準轉換存在從型別`A`型別`B`，則標準的明確轉換存在從型別`A`鍵入`B`與從型別`B`輸入`A`。

## <a name="user-defined-conversions"></a>使用者定義轉換

C# 允許預先定義的隱含和明確轉換成增強***使用者定義轉換***。 使用者定義轉換導入的宣告轉換運算子 ([轉換運算子](classes.md#conversion-operators)) 類別和結構的類型。

### <a name="permitted-user-defined-conversions"></a>允許使用者定義轉換

C# 允許只有特定使用者定義轉換為宣告。 特別是，不能重新定義現有的隱含或明確轉換。

指定的來源型別的`S`和目標型別`T`，如果`S`或`T`是可為 null 的型別，讓`S0`並`T0`指其基礎類型，否則為`S0`和`T0`是相等`S`和`T`分別。 類別或結構，並允許宣告從來源類型轉換`S`成目標型別`T`只有當下列所有項目為真：

*  `S0` 和`T0`是不同的型別。
*  請`S0`或`T0`是運算子宣告進行的類別或結構類型。
*  既不`S0`也`T0`是*interface_type*。
*  排除使用者定義的轉換，轉換沒有從`S`來`T`或來自`T`至`S`。

以使用者定義轉換適用之限制的討論中進一步[轉換運算子](classes.md#conversion-operators)。

### <a name="lifted-conversion-operators"></a>提昇的轉換運算子

指定使用者定義轉換運算子可從不可為 null 的實值型別轉換`S`不可為 null 的實值型別的`T`，則***提昇的轉換運算子***存在從轉換`S?`到`T?`. 此提昇的轉換運算子會執行從形式`S?`來`S`後面接著從使用者定義轉換`S`來`T`後面接著從繞`T`至`T?`，不同之處在於 nullvalued`S?`將轉換成 null 值直接`T?`。

提昇的轉換運算子有相同的隱含或明確分類，作為其基礎的使用者定義轉換運算子。 使用適用於 「 使用者定義轉換 」 一詞使用者定義，並提昇的轉換運算子。

### <a name="evaluation-of-user-defined-conversions"></a>使用者定義轉換的評估結果

使用者定義的轉換會將值轉換自其型別，稱為***來源類型***，另一種類型，稱為***目標類型***。 評估使用者定義轉換的中心有關尋找***最特定***尋找特定的來源和目標類型的使用者定義轉換運算子。 這項判斷會分成幾個步驟：

*  尋找一組類別和結構會被視為使用者定義轉換運算子。 這個集合是由來源類型和其基底類別和目標型別和其基底類別 （只有類別和結構可以宣告使用者定義的運算子和非類別類型具有基底類別的隱含假設） 所組成。 此步驟中，如果來源或目標類型是基於*nullable_type*、 其基礎類型改為使用。
*  從該型別組合，決定其使用者定義和提昇的轉換運算子皆適用。 適用於轉換運算子，它必須能夠執行標準轉換 ([標準轉換](conversions.md#standard-conversions)) 從來源類型運算元的運算子和它的型別必須是能夠執行標準轉換從目標型別運算子的結果類型。
*  從適用於使用者定義的運算子，判斷哪個運算子最適合明確地設定。 一般而言，最特定的運算子會是其運算元的類型是 「 最靠近 」 的來源類型，且結果類型為 「 最靠近 」 的目標類型的運算子。 使用者定義轉換運算子是慣用的透過消除的轉換運算子。 下列各節中定義建立最明確的使用者定義轉換運算子的確切規則。

一旦已識別出最明確的使用者定義轉換運算子，實際執行使用者定義的轉換就會包含最多三個步驟：

*  首先，若有必要，執行從來源類型的標準轉換成使用者定義或提昇轉換運算子的運算元型別。
*  接下來，您可以叫用使用者定義或提昇轉換運算子，以執行轉換。
*  最後，若有必要，執行從使用者定義或提昇轉換運算子的結果類型的標準轉換成目標型別。

使用者定義轉換永遠不會評估涉及一個以上的使用者定義或提昇轉換運算子。 換句話說，從類型轉換`S`輸入`T`將永遠不會先執行使用者定義的轉換，從`S`要`X`，然後執行使用者定義的轉換，從`X`來`T`。

下列各節會提供評估的使用者定義的隱含或明確轉換的確切定義。 定義將使用下列詞彙：

*  如果是標準的隱含轉換 ([標準的隱含轉換](conversions.md#standard-implicit-conversions)) 存在從型別`A`型別`B`，和如果既未`A`也`B`是*interface_type*s，然後`A`要***包含*** `B`，和`B`稱為***包含*** `A`。
*  ***最內含的型別***中一組型別是一個包含集合中的所有其他類型的類型。 如果沒有單一的型別包含所有其他型別，集合都沒有最內含的型別。 更具直覺性而言，最內含的型別是集合中的 「 大 」 類型，每個其他類型可以隱含地轉換為一種類型。
*  ***最所包含型別***中一組型別是集合中的所有型別包含一種類型。 如果沒有單一的型別被包含所有其他型別時，集合已最不會包含型別。 更具直覺性而言，最被內含的類型是 「 小 」 的型別集合中，能夠隱含轉換成其他類型的每一種類型。

### <a name="processing-of-user-defined-implicit-conversions"></a>處理的使用者定義的隱含轉換

從型別，使用者定義的隱含轉換`S`輸入`T`處理，如下所示：

*  判斷型別`S0`和`T0`。 如果`S`或`T`是可為 null 的型別，`S0`並`T0`是其基礎類型，否則為`S0`並`T0`等於`S`和`T`分別。
*  尋找一組類型， `D`，運算子會被視為從哪一個使用者定義的轉換。 這組組成`S0`(如果`S0`是類別或結構)，基底類別的`S0`(如果`S0`是一種類別)，並`T0`(如果`T0`是類別或結構)。
*  尋找一組適用於使用者定義和提昇轉換運算子， `U`。 這個集合所組成的類別或結構中的所宣告的使用者定義和提昇的隱含轉換運算子`D`，將從型別，包含轉換`S`所包含的型別`T`。 如果`U`是空的轉換為未定義，就會發生編譯時期錯誤。
*  尋找最特定的來源類型，`SX`中的運算子`U`:
    * 如果任一項中的運算子`U`轉換從`S`，然後`SX`是`S`。
    * 否則，請`SX`是來源型別之運算子的組合中最置於包含的型別`U`。 如果只有一個最包含找不到型別，則轉換模稜兩可和就會發生編譯時期錯誤。
*  尋找最特定的目標型別，`TX`中的運算子`U`:
    * 如果任一項中的運算子`U`轉換成`T`，然後`TX`是`T`。
    * 否則，請`TX`是最內含的型別，在目標型別中的運算子組合`U`。 如果找不到一個最內含的型別，則轉換是模稜兩可，就會發生編譯時期錯誤。
*  尋找最明確的轉換運算子：
    * 如果`U`包含一個會將從轉換的使用者定義的轉換運算子`SX`到`TX`，則這是最特定的轉換運算子。
    * 否則，如果`U`包含一個會將從轉換的提昇的轉換運算子`SX`到`TX`，則這是最特定的轉換運算子。
    * 否則，轉換模稜兩可，而且就會發生編譯時期錯誤。
*  最後，將套用的轉換：
    * 如果`S`不是`SX`，然後從標準隱含轉換`S`到`SX`會執行。
    * 最特定的轉換運算子會叫用來從轉換`SX`至`TX`。
    * 如果`TX`不是`T`，然後從標準隱含轉換`TX`到`T`會執行。

### <a name="processing-of-user-defined-explicit-conversions"></a>處理的使用者定義的明確轉換

從型別，使用者定義的明確轉換`S`輸入`T`處理，如下所示：

*  判斷型別`S0`和`T0`。 如果`S`或`T`是可為 null 的型別，`S0`並`T0`是其基礎類型，否則為`S0`並`T0`等於`S`和`T`分別。
*  尋找一組類型， `D`，運算子會被視為從哪一個使用者定義的轉換。 這組組成`S0`(如果`S0`是類別或結構)，基底類別`S0`(如果`S0`是一種類別)， `T0` (如果`T0`是類別或結構)，和基底類別的`T0`(如果`T0`是類別)。
*  尋找一組適用於使用者定義和提昇轉換運算子， `U`。 這一組含有使用者定義和提昇隱含或明確轉換運算子宣告的類別或結構中的所`D`，從內含型別轉換，或包含`S`至內含型別所`T`. 如果`U`是空的轉換為未定義，就會發生編譯時期錯誤。
*  尋找最特定的來源類型，`SX`中的運算子`U`:
    * 如果任一項中的運算子`U`轉換從`S`，然後`SX`是`S`。
    * 否則，如果任一項中的運算子`U`從包含的類型轉換`S`，然後`SX`是最置於包含的型別，這些運算子的來源類型的組合中。 如果沒有大部分包含可以找到型別，則轉換模稜兩可和就會發生編譯時期錯誤。
    * 否則，請`SX`是來源型別之運算子的組合中的最內含類型`U`。 如果找不到一個最內含的型別，則轉換是模稜兩可，就會發生編譯時期錯誤。
*  尋找最特定的目標型別，`TX`中的運算子`U`:
    * 如果任一項中的運算子`U`轉換成`T`，然後`TX`是`T`。
    * 否則，如果任一項中的運算子`U`轉換為所包含的類型`T`，然後`TX`是最內含的型別，這些運算子的目標類型的組合中。 如果找不到一個最內含的型別，則轉換是模稜兩可，就會發生編譯時期錯誤。
    * 否則，請`TX`是最置於包含的型別中運算子的目標類型的組合中`U`。 如果沒有大部分包含可以找到型別，則轉換模稜兩可和就會發生編譯時期錯誤。
*  尋找最明確的轉換運算子：
    * 如果`U`包含一個會將從轉換的使用者定義的轉換運算子`SX`到`TX`，則這是最特定的轉換運算子。
    * 否則，如果`U`包含一個會將從轉換的提昇的轉換運算子`SX`到`TX`，則這是最特定的轉換運算子。
    * 否則，轉換模稜兩可，而且就會發生編譯時期錯誤。
*  最後，將套用的轉換：
    * 如果`S`不是`SX`，然後從標準明確轉換`S`到`SX`會執行。
    * 最特定的使用者定義轉換運算子會叫用來從轉換`SX`至`TX`。
    * 如果`TX`不是`T`，然後從標準明確轉換`TX`到`T`會執行。

## <a name="anonymous-function-conversions"></a>匿名函式轉換

*Anonymous_method_expression*或是*lambda_expression*歸類為匿名函式 ([匿名函式運算式](expressions.md#anonymous-function-expressions))。 運算式沒有類型，但可以隱含地轉換成相容的委派型別或運算式樹狀架構型別。 具體來說，匿名函式`F`與委派型別相容`D`提供：

*  如果`F`包含*anonymous_function_signature*，然後`D`和`F`有相同數目的參數。
*  如果`F`neobsahuje *anonymous_function_signature*，然後`D`可能有零個或多個參數的任何類型的任何參數只要`D`具有`out`參數修飾詞。
*  如果`F`具有明確類型的參數清單，在每個參數`D`中的對應參數有相同的類型和修飾詞`F`。
*  如果`F`具有隱含型別的參數清單，`D`沒有`ref`或`out`參數。
*  如果主體`F`是運算式，且`D`已`void`傳回型別或`F`是非同步和`D`具有傳回型別`Task`，然後當每個參數的`F`指定的型別中對應的參數`D`，本文`F`是有效的運算式 (wrt[運算式](expressions.md))，以允許*statement_expression* ([運算式陳述式](statements.md#expression-statements))。
*  如果主體`F`是陳述式區塊，且`D`已`void`傳回型別或`F`是非同步和`D`具有傳回型別`Task`，然後當每個參數的`F`指定的型別中對應的參數`D`，本文`F`是有效的陳述式區塊 (wrt[區塊](statements.md#blocks)) 中且未`return`陳述式指定的運算式。
*  如果主體`F`是運算式，並*任一*`F`是非同步和`D`具有非 void 傳回型別`T`，*或*`F`是非同步和`D`的傳回型別`Task<T>`，然後當每個參數`F`中的對應參數的型別`D`，主體`F`是有效的運算式 (wrt [運算式](expressions.md))，會隱含地轉換成`T`。
*  如果主體`F`陳述式區塊，並*任一*`F`是非同步和`D`具有非 void 傳回型別`T`，*或*`F`是非同步和`D`的傳回型別`Task<T>`，然後當每個參數`F`中的對應參數的型別`D`，主體`F`是有效的陳述式區塊 (wrt[區塊](statements.md#blocks)) 與非可聯繫的端點，其中每一個`return`陳述式指定的運算式，會隱含地轉換成`T`。

為了簡潔起見，本節中，請使用工作類型的簡短形式`Task`並`Task<T>`([非同步函式](classes.md#async-functions))。

Lambda 運算式`F`相容的運算式樹狀架構型別`Expression<D>`如果`F`相容於委派型別`D`。 請注意這不適用於匿名方法，只是 lambda 運算式。

特定的 lambda 運算式無法轉換成運算式樹狀架構類型：即使轉換*存在*，它在編譯時期就會失敗。 這是則 lambda 運算式：

*  已*區塊*主體
*  包含簡單或複合指派運算子
*  包含動態繫結的運算式
*  為非同步

請依照下列範例會使用泛型委派型別`Func<A,R>`表示接受類型的引數的函式`A`，並傳回型別的值`R`:
```csharp
delegate R Func<A,R>(A arg);
```

在工作分派
```csharp
Func<int,int> f1 = x => x + 1;                 // Ok

Func<int,double> f2 = x => x + 1;              // Ok

Func<double,int> f3 = x => x + 1;              // Error

Func<int, Task<int>> f4 = async x => x + 1;    // Ok
```
每個匿名函式的參數和傳回型別是匿名函式會指派給變數的類型決定。

第一次指派成功轉換成委派類型的匿名函式`Func<int,int>`因為，當`x`為 object 型別`int`，`x+1`是有效的運算式，會隱含地轉換成輸入`int`。

同樣地，第二個指派成功會將匿名函式的委派型別`Func<int,double>`因為的結果`x+1`(型別的`int`) 是隱含轉換成類型`double`。

不過，第三個的指派是編譯時期錯誤，因為，當`x`為 object 型別`double`，結果`x+1`(型別`double`) 不是隱含轉換成類型`int`。

第四個指派成功將匿名的非同步函式轉換成委派類型`Func<int, Task<int>>`因為的結果`x+1`(型別的`int`) 會隱含地轉換成的結果型別`int`型別的工作`Task<int>`.

匿名函式可能會影響多載解析，並參與型別推斷。 請參閱[函式成員](expressions.md#function-members)如需詳細資訊。

### <a name="evaluation-of-anonymous-function-conversions-to-delegate-types"></a>匿名函式轉換成委派類型的評估結果

轉換成委派類型的匿名函式會產生委派執行個體參考的匿名函式和 （可能是空的） 擷取評估時處於作用中的外部變數的集合。 叫用委派時，會執行匿名函式的主體。 在本文中的程式碼會執行使用一組擷取委派所參考的外部變數。

所產生的匿名函式的委派引動過程清單包含單一項目。 確切的目標物件和目標方法的委派都未指定。 特別是，它未指定目標物件的委派是否`null`，則`this`封入函式成員或其他某些物件的值。

轉換相同語意的匿名函式以擷取外部變數執行個體相同的 （可能是空的） 設定為相同的委派類型都允許 （但非必要） 傳回相同的委派執行個體。 語意上完全相同的詞彙是此處用來表示，匿名函式的執行會在所有情況下，產生相同的效果，提供相同的引數。 此規則會允許程式碼，如下所示進行最佳化。

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

由於兩個的匿名函式委派具有相同的 （空的） 集合擷取的外部變數，以及因為匿名函式的語意相同，編譯器允許具有參考相同的目標方法的委派。 事實上，編譯器可以從這兩個匿名函式運算式中傳回相同的委派執行個體。

### <a name="evaluation-of-anonymous-function-conversions-to-expression-tree-types"></a>匿名函式轉換成運算式樹狀架構類型的評估結果

匿名函式轉換成運算式樹狀架構型別會產生運算式樹狀架構 ([運算式樹狀架構類型](types.md#expression-tree-types))。 更精確地說，評估的匿名函式轉換會導致建構的物件結構，表示匿名函式本身的結構。 運算式樹狀架構，以及用於建立它，確切的程序的精確結構會實作所定義。

### <a name="implementation-example"></a>實作範例

本節說明可能的實作，根據其他 C# 建構的匿名函式轉換。 此處所述的實作根據相同的原則，由 Microsoft C# 編譯器，但它不是託管的實作中，也不是只有一個可能的。 它僅簡單提及轉換成運算式樹狀架構，因為其確切語意不超出此規格的範圍。

本節的其餘部分會提供數個範例包含具有不同特性的匿名函式的程式碼。 對於每個範例中，提供對應的翻譯，使用只有其他 C# 建構程式碼。 在範例中，識別碼`D`會假設為代表下列委派型別：
```csharp
public delegate void D();
```

匿名函式的最簡單形式是擷取任何外部變數的其中一個：
```csharp
class Test
{
    static void F() {
        D d = () => { Console.WriteLine("test"); };
    }
}
```

這可以轉譯為委派具現化參考編譯器產生的靜態方法，匿名函式的程式碼位於：
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

在下列範例中，匿名函式參考的執行個體成員`this`:
```csharp
class Test
{
    int x;

    void F() {
        D d = () => { Console.WriteLine(x); };
    }
}
```

這可以轉譯為包含匿名函式的程式碼編譯器產生的執行個體方法：
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

在此範例中，匿名函式會擷取區域變數：
```csharp
class Test
{
    void F() {
        int y = 123;
        D d = () => { Console.WriteLine(y); };
    }
}
```

本機變數的存留期必須延伸為至少匿名函式委派的存留期。 這可藉由編譯器產生類別的欄位"hoisting 」 的本機變數。 具現化的本機變數 ([具現化的本機變數](expressions.md#instantiation-of-local-variables)) 再建立的執行個體的編譯器產生的類別，以及存取本機變數對應至存取的執行個體中的欄位對應編譯器產生的類別。 此外，匿名函式會變成編譯器產生類別的執行個體方法：
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

最後，下列匿名函式擷取`this`以及使用不同的存留期的兩個區域變數：
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

在這裡，建立每個陳述式的編譯器產生類別中的區域變數會擷取讓不同的區塊中的區域變數可以擁有不同的存留期的區塊。 執行個體`__Locals2`，編譯器產生的類別內部陳述式區塊中，包含本機變數`z`的欄位參考的執行個體，以及`__Locals1`。  執行個體`__Locals1`，外部陳述式區塊中，編譯器產生的類別會包含區域變數`y`和 [參考] 欄位`this`封入函式成員。 使用這些資料結構，就可以連到所有擷取外部變數的執行個體透過`__Local2`，而匿名函式的程式碼可以因此實作為該類別的執行個體方法。

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

將匿名函式轉換成運算式樹狀架構時，也可以用相同的技巧，這裡套用至擷取的區域變數：編譯器產生物件的參考可以儲存在運算式樹狀架構，可以代表本機變數的存取權，因為這些物件上的欄位存取。 此方法的優點是它可讓 「 提取 」 的本機變數，委派和運算式樹狀架構之間共用。

## <a name="method-group-conversions"></a>方法群組轉換

隱含的轉換 ([隱含轉換](conversions.md#implicit-conversions)) 存在從方法群組 ([運算式分類](expressions.md#expression-classifications)) 相容的委派型別。 指定的委派型別`D`和運算式`E`可分類為方法群組、 隱含的轉換`E`要`D`如果`E`包含至少一個的方法，其一般表單 （適用於[適用的函式成員](expressions.md#applicable-function-member)) 引數清單中，建構所使用的參數類型和修飾詞`D`，如下列所述。

從方法群組轉換的編譯時期應用程式`E`成委派類型`D`下列所述。 請注意，是否存在的隱含轉換`E`至`D`不保證轉換的編譯時期應用程式將會成功且沒有錯誤。

*  單一方法`M`已選取對應至方法的引動過程 ([方法引動過程](expressions.md#method-invocations)) 的形式`E(A)`，進行下列修改：
    * 引數清單`A`是運算式，每個已分類為變數，且具有類型和修飾詞的清單 (`ref`或是`out`) 中的對應參數*formal_parameter_list* 的`D`.
    * 考慮的候選項目方法為只適用於一般形式的方法 ([適用的函式成員](expressions.md#applicable-function-member))，不列入只適用於其展開的形式。
*  如果演算法[方法引動過程](expressions.md#method-invocations)產生錯誤，則會發生編譯時期錯誤。 否則，演算法會產生單一的最佳方法`M`具有相同數目的參數做為`D`並轉換不存在。
*  選取的方法`M`必須是相容 ([委派相容性](delegates.md#delegate-compatibility)) 與委派類型`D`，或發生編譯時期錯誤，否則為。
*  如果選取的方法`M`是執行個體方法，與相關聯的執行個體運算式`E`判斷目標物件的委派。
*  如果選取的方法 M 表示透過成員存取運算式的執行個體上的擴充方法，該執行個體運算式會判斷目標物件的委派。
*  轉換的結果是型別的值 `D`，也就是指的是所選的方法和目標物件的新建立的委派。
*  請注意，如果此程序會造成的擴充方法，委派建立的演算法[方法引動過程](expressions.md#method-invocations)無法找到的執行個體方法，但成功處理的引動過程`E(A)`作為擴充功能方法引動過程 ([擴充方法引動過程](expressions.md#extension-method-invocations))。 委派，因此建立擷取的擴充方法，以及其第一個引數。

下列範例會示範方法群組轉換：
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

指派給`d1`會隱含地轉換方法群組`F`類型的值來`D1`。

若要指派`d2`示範如何建立具有較少衍生 (contravariant) 型別參數的方法的委派可以和更多衍生 （共變數） 的傳回型別。

若要指派`d3`顯示沒有轉換的存在於是否方法不適用。

若要指派`d4`顯示方式的方法必須以其標準形式。

若要指派`d5`顯示允許如何與不同只適用於參考類型的委派和方法的參數和傳回類型。

如同所有其他隱含和明確轉換，轉換運算子可用來明確地執行方法群組轉換。 因此，範例
```csharp
object obj = new EventHandler(myDialog.OkClick);
```
可以改為撰寫
```csharp
object obj = (EventHandler)myDialog.OkClick;
```

方法群組可能會影響多載解析，並參與型別推斷。 請參閱[函式成員](expressions.md#function-members)如需詳細資訊。

方法群組轉換執行階段評估程序進行，如下所示：

*  如果在編譯時期選取的方法是執行個體方法，或是存取執行個體方法的擴充方法，委派的目標物件從相關聯的執行個體運算式決定`E`:
    * 執行個體運算式評估。 如果此評估會產生例外狀況，則會不執行任何進一步的步驟。
    * 如果執行個體的運算式*reference_type*，執行個體運算式所計算的值會成為目標物件。 如果選取的方法是執行個體方法和目標物件是`null`、`System.NullReferenceException`就會擲回，而且會執行任何進一步的步驟。
    * 如果執行個體的運算式*value_type*，boxing 作業 ([Boxing 轉換](types.md#boxing-conversions)) 會將值轉換成物件，而此物件會變成目標物件。
*  否則選取的方法是靜態方法呼叫的一部分，委派的目標物件`null`。
*  委派類型的新執行個體`D`配置。 如果不是記憶體不足，無法配置新的執行個體，`System.OutOfMemoryException`就會擲回，而且會執行任何進一步的步驟。
*  新的委派執行個體初始化方法在編譯時期所決定的參考，且上述計算的目標物件的參考。
