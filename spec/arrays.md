---
ms.openlocfilehash: 155c1beecddfdfcce2e7948bcb8d6b80428fbd7a
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229552"
---
# <a name="arrays"></a>陣列

陣列是資料結構，其中包含數個透過計算索引存取的變數。 陣列，也稱為陣列的項目中包含的變數都相同的型別，以及這個型別稱為陣列的項目類型。

陣列的陣序規範用來決定每個陣列項目相關聯的索引鍵的數目。 陣列的陣序規範也稱為陣列的維度。 陣列陣序規範的其中一個稱為***維陣列***。 陣列陣序規範大於其中一個稱為***多維陣列***。 特定大小的多維度陣列通常稱為二維陣列，三維陣列，以及等等。

陣列的每個維度具有相關聯的長度，也就是大於或等於零的整數。 維度長度不是陣列型別的一部分，但而不是建立陣列型別的執行個體建立在執行階段時。 維度的長度將決定該維度的索引的有效範圍：維度的長度`N`，索引的範圍可以從`0`到`N - 1`（含)。 陣列中的元素總數是產品之陣列中每個維度的長度。 如果一或多個維度的陣列擁有長度為零，陣列即為空白。

陣列的元素型別可以是任一型別，包括陣列型別。

## <a name="array-types"></a>陣列型別

陣列類型以寫入*non_array_type*後面接著一或多個*rank_specifier*s:

```antlr
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
```

A *non_array_type*可以是任何*型別*也就是本身並非*array_type*。

陣列類型的陣序規範根據最左邊*rank_specifier*中*array_type*:A *rank_specifier*表示陣列的陣列，陣序規範的數目加一，「`,`"的語彙基元*rank_specifier*。

陣列類型的項目類型是類型所產生的刪除最左邊*rank_specifier*:

*  陣列類型的表單`T[R]`是陣列陣序規範`R`和非陣列元素型別`T`。
*  陣列類型的表單`T[R][R1]...[Rn]`是陣列陣序規範`R`和 項目類型`T[R1]...[Rn]`。

實際上*rank_specifier*s 會從左向右讀取前的最後一個非陣列項目的類型。 型別`int[][,,][,]`是一維陣列的二維陣列的三維陣列`int`。

在執行階段陣列型別的值可以是`null`或該陣列型別的執行個體的參考。

### <a name="the-systemarray-type"></a>System.Array 型別

型別`System.Array`是所有陣列類型的抽象基底類型。 隱含參考轉換 ([隱含參考轉換](conversions.md#implicit-reference-conversions)) 存在從任何陣列型別`System.Array`，並明確參考轉換 ([明確參考轉換](conversions.md#explicit-reference-conversions)) 是否存在從`System.Array`任何陣列類型。 請注意，`System.Array`本身並不會*array_type*。 而是*class_type*所有*array_type*s 衍生。

在執行階段，類型的值`System.Array`可以是`null`或任何陣列類型的執行個體的參考。

### <a name="arrays-and-the-generic-ilist-interface"></a>陣列和泛型 IList 介面

一維陣列`T[]`實作介面`System.Collections.Generic.IList<T>`(`IList<T>`簡稱) 和其基底介面。 因此，沒有從的隱含轉換`T[]`至`IList<T>`和其基底介面。 此外，如果沒有從隱含參考轉換`S`要`T`再`S[]`實作`IList<T>`，而且沒有從隱含參考轉換`S[]`至`IList<T>`和其基底介面 （[隱含參考轉換](conversions.md#implicit-reference-conversions))。 如果沒有明確參考轉換從`S`來`T`的明確參考轉換就`S[]`來`IList<T>`及其基底介面 ([明確參考轉換](conversions.md#explicit-reference-conversions)). 例如: 
```csharp
using System.Collections.Generic;

class Test
{
    static void Main() {
        string[] sa = new string[5];
        object[] oa1 = new object[5];
        object[] oa2 = sa;

        IList<string> lst1 = sa;                    // Ok
        IList<string> lst2 = oa1;                   // Error, cast needed
        IList<object> lst3 = sa;                    // Ok
        IList<object> lst4 = oa1;                   // Ok

        IList<string> lst5 = (IList<string>)oa1;    // Exception
        IList<string> lst6 = (IList<string>)oa2;    // Ok
    }
}
```

工作分派`lst2 = oa1`從轉換後就會產生編譯時期錯誤`object[]`到`IList<string>`是明確的轉換，不隱含。 轉型`(IList<string>)oa1`將會在之後的執行時期擲回的例外狀況`oa1`參考`object[]`而非`string[]`。 不過轉型`(IList<string>)oa2`不會因為擲回例外狀況`oa2`參考`string[]`。

每當沒有隱含或明確參考轉換`S[]`來`IList<T>`，也是從明確參考轉換`IList<T>`和其基底介面`S[]`([明確參考轉換](conversions.md#explicit-reference-conversions))。

當陣列型別`S[]`實作`IList<T>`，某些實作介面的成員可能會擲回例外狀況。 精確的行為的介面的實作已超出此規格的範圍。

## <a name="array-creation"></a>建立陣列

藉由建立陣列執行個體*array_creation_expression*s ([陣列建立運算式](expressions.md#array-creation-expressions)) 或欄位或區域變數宣告包含*array_initializer*([陣列初始設定式](arrays.md#array-initializers))。

建立陣列執行個體時，順位和每個維度的長度所建立，然後保持不變執行個體的整個存留期間。 換句話說，就無法變更現有的陣列執行個體的陣序，也不是可調整大小及其維度。

陣列執行個體一律會是陣列型別。 `System.Array`型別是抽象類型無法具現化。

所建立的陣列的元素*array_creation_expression*s 一律會初始化為其預設值 ([預設值](variables.md#default-values))。

## <a name="array-element-access"></a>陣列元素存取

陣列項目用來存取*element_access*運算式 ([陣列存取](expressions.md#array-access)) 的形式`A[I1, I2, ..., In]`，其中`A`是陣列型別和每個運算式`Ix`是類型的運算式`int`， `uint`， `long`， `ulong`，或可以隱含地轉換為一或多個這些型別。 陣列元素存取的結果是一個變數，也就是選取索引的陣列元素。

陣列的項目可以使用列舉`foreach`陳述式 ([foreach 陳述式](statements.md#the-foreach-statement))。

## <a name="array-members"></a>陣列成員

每個陣列型別會繼承由宣告成員`System.Array`型別。

## <a name="array-covariance"></a>陣列共變數

任何兩個*reference_type*s`A`並`B`，如果隱含參考轉換 ([隱含參考轉換](conversions.md#implicit-reference-conversions)) 或明確參考轉換 ([明確參考轉換](conversions.md#explicit-reference-conversions)) 從`A`來`B`，然後從陣列類型也存在相同的參考轉換`A[R]`為陣列型別`B[R]`，其中`R`的話給定*rank_specifier* （但相同陣列型別）。 此關聯性就所謂***陣列共變數***。 陣列共變數特別表示陣列型別的值`A[R]`實際上可能是陣列型別的執行個體的參考`B[R]`，如果隱含參考轉換存在`B`至`A`。

陣列共變數，因為指派給參考型別陣列的項目會包含執行階段檢查可確保指派給陣列元素的值實際上是允許的型別 ([簡單指派](expressions.md#simple-assignment))。 例如: 
```csharp
class Test
{
    static void Fill(object[] array, int index, int count, object value) {
        for (int i = index; i < index + count; i++) array[i] = value;
    }

    static void Main() {
        string[] strings = new string[100];
        Fill(strings, 0, 100, "Undefined");
        Fill(strings, 0, 10, null);
        Fill(strings, 90, 10, 0);
    }
}
```

指派給`array[i]`中`Fill`方法會隱含地包含執行階段檢查可確保所參考的物件`value`是`null`實際的項目類型相容的執行個體或`array`. 在 `Main`的前兩個引動過程`Fill`成功，但第三個引動過程會導致`System.ArrayTypeMismatchException`擲回時執行的第一個指派`array[i]`。 因為發生例外狀況 boxed`int`無法儲存在`string`陣列。

陣列共變數特別不會延伸到的陣列*value_type*s。 例如，沒有任何轉換存在該允許`int[]`視為`object[]`。

## <a name="array-initializers"></a>陣列初始設定式

陣列初始設定式可能會在欄位宣告中指定 ([欄位](classes.md#fields))，本機變數宣告 ([區域變數宣告](statements.md#local-variable-declarations))，和陣列建立運算式 ([陣列建立運算式](expressions.md#array-creation-expressions)):

```antlr
array_initializer
    : '{' variable_initializer_list? '}'
    | '{' variable_initializer_list ',' '}'
    ;

variable_initializer_list
    : variable_initializer (',' variable_initializer)*
    ;

variable_initializer
    : expression
    | array_initializer
    ;
```

陣列初始設定式所組成的變數初始設定式，用括住的一連串 「`{`"和"`}`「 語彙基元和分隔 」`,`"語彙基元。 每個變數的初始設定式是一種運算式或在多維度陣列，巢狀的陣列初始設定式的情況下。

其中的陣列初始設定式會使用內容決定要初始化之陣列的類型。 陣列建立運算式，在陣列型別，立即之前的初始設定式，或從陣列初始設定式中的運算式推斷進行相關的設定。 在欄位或變數宣告中，陣列型別是欄位或所宣告變數的類型。 陣列初始設定式中使用時的欄位或變數宣告，例如：
```csharp
int[] a = {0, 2, 4, 6, 8};
```
它是簡略的對等陣列建立運算式：
```csharp
int[] a = new int[] {0, 2, 4, 6, 8};
```

一維陣列，陣列初始設定式必須包含一系列的指派相容於陣列的項目類型的運算式。 運算式，初始化陣列元素，以遞增次序順序，從索引零處的項目。 陣列初始設定式中的運算式數目會決定所建立的陣列執行個體的長度。 例如，上述的陣列初始設定式會建立`int[]`長度為 5 的執行個體，然後初始化的執行個體，使用下列值：
```csharp
a[0] = 0; a[1] = 2; a[2] = 4; a[3] = 6; a[4] = 8;
```

多維陣列，陣列初始設定式必須有多個層級的巢狀陣列中的維度相同。 最外層巢狀層級對應至最左邊的維度，最內層的巢狀層級對應於最右側的維度。 陣列的每個維度的長度取決於對應的巢狀層級的陣列初始設定式中的項目數。 每個巢狀的陣列初始設定式中，項目數目必須與其他陣列初始設定式在相同的層級相同。 範例：
```csharp
int[,] b = {{0, 1}, {2, 3}, {4, 5}, {6, 7}, {8, 9}};
```
建立二維陣列的長度為五個最左邊的維度與長度為兩個最右邊的維度：
```csharp
int[,] b = new int[5, 2];
```
然後初始化陣列執行個體和使用下列值：
```csharp
b[0, 0] = 0; b[0, 1] = 1;
b[1, 0] = 2; b[1, 1] = 3;
b[2, 0] = 4; b[2, 1] = 5;
b[3, 0] = 6; b[3, 1] = 7;
b[4, 0] = 8; b[4, 1] = 9;
```

如果維度以外的最右邊長度為零，會假設後續的維度，也有長度為零。 範例：
```csharp
int[,] c = {};
```
建立二維陣列的長度為零，最左邊和右邊的維度：
```csharp
int[,] c = new int[0, 0];
```

當陣列建立運算式包括明確的維度長度及陣列初始設定式時，長度必須是常數運算式，並在每個巢狀層級的項目數必須符合相對應維度的長度。 以下是一些範例：
```csharp
int i = 3;
int[] x = new int[3] {0, 1, 2};        // OK
int[] y = new int[i] {0, 1, 2};        // Error, i not a constant
int[] z = new int[3] {0, 1, 2, 3};     // Error, length/initializer mismatch
```

這裡的初始設定式`y`導致編譯時期錯誤，因為維度長度運算式不是常數，與的初始設定式`z`導致編譯時期錯誤，因為長度和中的項目數不同意初始設定式。
