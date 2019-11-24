---
ms.openlocfilehash: d162d4b6a489032dcdfca9094a39d88fd03d4013
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704091"
---
# <a name="delegates"></a>委派

委派可讓其他語言（例如C++，Pascal 和 Modula）的案例能夠使用函式指標來解決。 不過C++ ，不同于函式指標，委派是完全面向物件， C++而且與成員函式的指標不同，委派會封裝物件實例和方法。

委派宣告會定義衍生自類別的類別 `System.Delegate`。 委派實例會封裝一個調用清單，這是一或多個方法的清單，每個方法都稱為可呼叫的實體。 若是實例方法，可呼叫的實體是由實例和該實例上的方法所組成。 若為靜態方法，可呼叫的實體只會包含方法。 使用一組適當的引數來叫用委派實例，會使每一個委派的可呼叫實體以一組指定的引數叫用。

委派實例有一個有趣且實用的屬性，就是它不知道或在意它所封裝之方法的類別;重點在於這些方法與委派的型別相容（[委派](delegates.md#delegate-declarations)宣告）。 這使得委派非常適合「匿名」調用。

## <a name="delegate-declarations"></a>委派宣告

*Delegate_declaration*是宣告新委派類型的*type_declaration* （[類型](namespaces.md#type-declarations)宣告）。

```antlr
delegate_declaration
    : attributes? delegate_modifier* 'delegate' return_type
      identifier variant_type_parameter_list?
      '(' formal_parameter_list? ')' type_parameter_constraints_clause* ';'
    ;

delegate_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | delegate_modifier_unsafe
    ;
```

在委派宣告中多次出現相同的修飾詞時，就會發生編譯時期錯誤。

`new` 修飾詞只允許用於在另一個類型中宣告的委派，在此情況下，它會指定這類委派會隱藏具有相同名稱的繼承成員，如[新修飾](classes.md#the-new-modifier)詞中所述。

`public`、`protected`、`internal`和 `private` 修飾詞會控制委派類型的存取範圍。 根據發生委派宣告的內容，可能不允許其中一些修飾詞（宣告的[存取](basic-concepts.md#declared-accessibility)範圍）。

委派的類型名稱為*identifier*。

選擇性的*formal_parameter_list*會指定委派的參數，而*return_type*則表示委派的傳回型別。

選擇性的*variant_type_parameter_list* （[variant 型別參數清單](interfaces.md#variant-type-parameter-lists)）指定委派本身的型別參數。

委派類型的傳回類型必須是 `void`或輸出安全（變異數[安全性](interfaces.md#variance-safety)）。

委派類型的所有正式參數類型都必須是輸入安全的。 此外，任何 `out` 或 `ref` 參數類型也必須為輸出安全。 請注意，即使 `out` 參數是輸入安全的，因為基礎執行平臺的限制。

中C#的委派類型為對等的名稱，不是結構上的對等用法。 具體而言，有兩個不同的委派類型具有相同的參數清單和傳回類型，會被視為不同的委派類型。 不過，兩個不同但結構相等之委派類型的實例，可能會比較為相等（[委派相等運算子](expressions.md#delegate-equality-operators)）。

例如：

```csharp
delegate int D1(int i, double d);

class A
{
    public static int M1(int a, double b) {...}
}

class B
{
    delegate int D2(int c, double d);
    public static int M1(int f, double g) {...}
    public static void M2(int k, double l) {...}
    public static int M3(int g) {...}
    public static void M4(int g) {...}
}
```

`A.M1` 和 `B.M1` 的方法與委派類型 `D1` 和 `D2` 相容，因為它們具有相同的傳回類型和參數清單;不過，這些委派類型是兩個不同的類型，因此無法互換。 `B.M2`、`B.M3`和 `B.M4` 的方法與 `D1` 和 `D2`的委派類型不相容，因為它們有不同的傳回類型或參數清單。

如同其他泛型型別宣告，必須提供類型引數，才能建立結構化委派類型。 建立結構化委派類型的參數類型和傳回型別，是藉由以委派宣告中的每個型別參數來取代，這是結構化委派型別的對應型別引數。 產生的傳回型別和參數類型是用來判斷哪些方法與結構化的委派型別相容。 例如：

```csharp
delegate bool Predicate<T>(T value);

class X
{
    static bool F(int i) {...}
    static bool G(string s) {...}
}
```

`X.F` 的方法與委派類型 `Predicate<int>` 相容，而且方法 `X.G` 與 `Predicate<string>` 的委派類型相容。

宣告委派類型的唯一方式是透過*delegate_declaration*。 委派類型是衍生自 `System.Delegate`的類別類型。 委派類型會隱含 `sealed`，因此不允許從委派類型衍生任何類型。 也不允許從 `System.Delegate`衍生非委派類別類型。 請注意，`System.Delegate` 本身並不是委派類型;它是衍生所有委派類型的類別類型。

C#提供委派具現化和調用的特殊語法。 除了具現化以外，可以套用至類別或類別實例的任何作業，也可以分別套用至委派類別或實例。 特別的是，您可以透過一般的成員存取語法來存取 `System.Delegate` 類型的成員。

委派實例所封裝的方法集合稱為「調用清單」。 從單一方法建立委派實例（[委派相容性](delegates.md#delegate-compatibility)）時，它會封裝該方法，而其調用清單只會包含一個專案。 不過，當結合兩個非 null 委派實例時，會串連其調用清單--在 order left 運算元 then 右運算元--中，以形成新的調用清單，其中包含兩個或多個專案。

委派會使用 binary `+` （[加號](expressions.md#addition-operator)）和 `+=` 運算子（[複合指派](expressions.md#compound-assignment)）結合。 您可以使用 binary `-` （[減法運算子](expressions.md#subtraction-operator)）和 `-=` 運算子（[複合指派](expressions.md#compound-assignment)），從委派的組合中移除委派。 委派可以比較是否相等（[委派相等運算子](expressions.md#delegate-equality-operators)）。

下列範例顯示一些委派的具現化，以及其對應的調用清單：

```csharp
delegate void D(int x);

class C
{
    public static void M1(int i) {...}
    public static void M2(int i) {...}

}

class Test
{
    static void Main() {
        D cd1 = new D(C.M1);      // M1
        D cd2 = new D(C.M2);      // M2
        D cd3 = cd1 + cd2;        // M1 + M2
        D cd4 = cd3 + cd1;        // M1 + M2 + M1
        D cd5 = cd4 + cd3;        // M1 + M2 + M1 + M1 + M2
    }

}
```

當 `cd1` 和 `cd2` 具現化時，它們會分別封裝一種方法。 當 `cd3` 具現化時，它會有兩個方法的調用清單，`M1` 和 `M2`（依該順序）。 `cd4`的調用清單包含以該順序 `M1`、`M2`和 `M1`。 最後，`cd5`的調用清單包含以該順序 `M1`、`M2`、`M1`、`M1`和 `M2`。 如需結合（以及移除）委派的更多範例，請參閱[委派調用](delegates.md#delegate-invocation)。

## <a name="delegate-compatibility"></a>委派相容性

如果下列所有條件都成立，則方法或委派 `M` 與委派類型 `D`***相容***：

*  `D` 和 `M` 的參數數目相同，而且 `D` 中的每個參數都與 `out` 中的對應參數具有相同的 `ref` 或 `M`修飾詞。
*  針對每個值參數（不含 `ref` 或 `out` 修飾詞的參數），會從 `D` 中的參數類型，將識別轉換（[識別轉換](conversions.md#identity-conversion)）或隱含參考轉換（[隱含參考](conversions.md#implicit-reference-conversions)轉換）存在於 `M`中的對應參數類型。
*  針對每個 `ref` 或 `out` 參數，`D` 中的參數類型與 `M`中的參數類型相同。
*  識別或隱含參考轉換存在於 `M` 的傳回型別中，`D`的傳回型別。

## <a name="delegate-instantiation"></a>委派具現化

委派的實例是由*delegate_creation_expression* （[委派建立運算式](expressions.md#delegate-creation-expressions)）或委派類型的轉換所建立。 新建立的委派實例則是指：

*  *Delegate_creation_expression*中所參考的靜態方法，或
*  在*delegate_creation_expression*中參考的目標物件（不能是 `null`）和實例方法，或
*  另一個委派。

例如：

```csharp
delegate void D(int x);

class C
{
    public static void M1(int i) {...}
    public void M2(int i) {...}
}

class Test
{
    static void Main() { 
        D cd1 = new D(C.M1);        // static method
        C t = new C();
        D cd2 = new D(t.M2);        // instance method
        D cd3 = new D(cd2);        // another delegate
    }
}
```

一旦具現化之後，委派實例一律會參考相同的目標物件和方法。 請記住，當兩個委派合併，或其中一個被移除時，新的委派結果會有自己的調用清單;結合或移除委派的調用清單會保持不變。

## <a name="delegate-invocation"></a>委派調用

C#提供用來叫用委派的特殊語法。 當叫用清單中包含一個專案的非 null 委派實例被叫用時，它會使用指定的相同引數叫用一個方法，並傳回與所參考方法相同的值。 （如需委派調用的詳細資訊，請參閱[委派調用](expressions.md#delegate-invocations)）。如果在這類委派的調用期間發生例外狀況，而且該例外狀況不會在叫用的方法內攔截，則搜尋例外狀況 catch 子句會繼續在呼叫委派的方法中，如同該方法直接呼叫該委派所參考的方法一樣。

叫用其調用清單包含多個專案的委派實例，會依照順序以同步方式叫用調用清單中的每個方法繼續進行。 呼叫的每個方法都會傳遞與指定給委派實例相同的引數集。 如果這類委派調用包含參考參數（[參考參數](classes.md#reference-parameters)），則會發生具有相同變數參考的每個方法調用;此變數在調用清單中的方法變更，將會在調用清單中進一步向下顯示。 如果委派調用包含輸出參數或傳回值，其最終值會來自清單中最後一個委派的調用。

如果在處理這類委派的調用期間發生例外狀況，而且該例外狀況不會在叫用的方法內攔截，則搜尋例外狀況 catch 子句會繼續在呼叫委派的方法中，並于後續的任何方法中執行不會叫用調用清單。

嘗試叫用值為 null 的委派實例，會導致 `System.NullReferenceException`類型的例外狀況。

下列範例示範如何具現化、結合、移除和叫用委派：

```csharp
using System;

delegate void D(int x);

class C
{
    public static void M1(int i) {
        Console.WriteLine("C.M1: " + i);
    }

    public static void M2(int i) {
        Console.WriteLine("C.M2: " + i);
    }

    public void M3(int i) {
        Console.WriteLine("C.M3: " + i);
    }
}

class Test
{
    static void Main() { 
        D cd1 = new D(C.M1);
        cd1(-1);                // call M1

        D cd2 = new D(C.M2);
        cd2(-2);                // call M2

        D cd3 = cd1 + cd2;
        cd3(10);                // call M1 then M2

        cd3 += cd1;
        cd3(20);                // call M1, M2, then M1

        C c = new C();
        D cd4 = new D(c.M3);
        cd3 += cd4;
        cd3(30);                // call M1, M2, M1, then M3

        cd3 -= cd1;             // remove last M1
        cd3(40);                // call M1, M2, then M3

        cd3 -= cd4;
        cd3(50);                // call M1 then M2

        cd3 -= cd2;
        cd3(60);                // call M1

        cd3 -= cd2;             // impossible removal is benign
        cd3(60);                // call M1

        cd3 -= cd1;             // invocation list is empty so cd3 is null

        cd3(70);                // System.NullReferenceException thrown

        cd3 -= cd1;             // impossible removal is benign
    }
}
```

如 `cd3 += cd1;`的語句所示，一個委派可以多次出現在調用清單中。 在此情況下，它只會在每次發生時叫用一次。 在這類調用清單中，移除該委派時，在調用清單中最後一次出現的專案就是實際移除的。

在執行最後一個語句之前，`cd3 -= cd1;`，委派 `cd3` 會參考空的調用清單。 嘗試從空白清單中移除委派（或從非空白清單中移除不存在的委派）並不是錯誤。

產生的輸出為：

```console
C.M1: -1
C.M2: -2
C.M1: 10
C.M2: 10
C.M1: 20
C.M2: 20
C.M1: 20
C.M1: 30
C.M2: 30
C.M1: 30
C.M3: 30
C.M1: 40
C.M2: 40
C.M3: 40
C.M1: 50
C.M2: 50
C.M1: 60
C.M1: 60
```
