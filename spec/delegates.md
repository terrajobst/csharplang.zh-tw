---
ms.openlocfilehash: 994b22f5375d57cfc4c7537c64345a27ddf3e416
ms.sourcegitcommit: 09e0ddec3bb6aa99b7340158bbac86a5a8243b43
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66193876"
---
# <a name="delegates"></a>委派

委派可讓案例的其他語言，例如C++，依照 pascal 命名法和 Modula-解決與函式指標。 不同於C++函式指標，不過，委派完全是物件導向，以及不同的是C++成員函式委派指標封裝物件執行個體和方法。

在 delegate 宣告中定義的類別，衍生自類別`System.Delegate`。 委派執行個體封裝引動過程清單中，這是一或多個方法，每一個都指可呼叫的實體的清單。 執行個體方法，可呼叫的實體包含的執行個體與該執行個體上的方法。 如需靜態方法，可呼叫的實體所組成的一個方法。 叫用委派執行個體使用一組適合的引數會導致每個委派的可呼叫實體，若要使用一組指定的引數叫用。

有趣且實用的屬性，委派執行個體的是，它不知道或在意它會封裝; 方法的類別最重要的是，這些方法是相容 ([委派宣告](delegates.md#delegate-declarations)) 與委派的型別。 這可讓委派非常適用於 「 匿名 」 的引動過程。

## <a name="delegate-declarations"></a>委派宣告

A *delegate_declaration*是*type_declaration* ([型別宣告](namespaces.md#type-declarations)) 宣告新的委派類型。

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

它是相同的修飾詞在 delegate 宣告中出現多次的編譯時期錯誤。

`new`修飾詞僅允許委派宣告另一個類型內，在此情況下它會指定這類委派隱藏繼承的成員相同的名稱，如中所述[的新修飾詞](classes.md#the-new-modifier)。

`public`， `protected`， `internal`，和`private`修飾詞控制的委派型別存取範圍。 根據和委派宣告發生所在的內容，這些修飾詞的一些可能不允許 ([宣告存取範圍](basic-concepts.md#declared-accessibility))。

委派的型別名稱是*識別碼*。

選擇性*formal_parameter_list*指定之參數的委派，以及*return_type*表示委派的傳回型別。

選擇性*variant_type_parameter_list* ([Variant 類型參數清單](interfaces.md#variant-type-parameter-lists)) 指定的委派本身的類型參數。

委派類型的傳回型別必須是`void`，或輸出安全 ([變異數安全](interfaces.md#variance-safety))。

委派型別的所有型式參數類型必須是輸入安全。 此外，任何`out`或`ref`參數類型必須也是輸出安全。 請注意，即使`out`參數必須是輸入安全，因為基礎執行平台的限制。

C# 中的委派類型是名稱相等結構上相等。 具體而言，具有相同的參數的兩種不同的委派類型列出，並傳回型別會被視為不同的委派類型。 不過，兩個不同但結構上相等的委派型別的執行個體可能會比較為相等 ([委派等號比較運算子](expressions.md#delegate-equality-operators))。

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

方法`A.M1`並`B.M1`這兩個委派類型是否相容`D1`和`D2`，因為它們有相同的傳回型別和參數的清單; 不過，這些委派型別是兩個不同的類型，因此它們不是可互換的。 方法`B.M2`， `B.M3`，並`B.M4`與委派類型不相容`D1`和`D2`，因為它們有不同的傳回型別或參數清單。

其他泛型類型宣告，例如型別引數必須有建立建構的委派型別。 參數型別和傳回型別建構的委派型別會建立在委派宣告中，每個類型參數的替代建構的委派類型的對應的型別引數。 產生的傳回型別和參數類型會用於決定哪些方法與建構的委派型別相容。 例如: 

```csharp
delegate bool Predicate<T>(T value);

class X
{
    static bool F(int i) {...}
    static bool G(string s) {...}
}
```

此方法`X.F`相容的委派型別`Predicate<int>`和方法`X.G`與委派型別相容`Predicate<string>`。

宣告委派類型的唯一方式是透過*delegate_declaration*。 委派型別是衍生自的類別類型`System.Delegate`。 委派類型都是隱含`sealed`，因此不允許從委派型別衍生任何型別。 它也不是允許衍生類別的非委派類型，從`System.Delegate`。 請注意，`System.Delegate`是委派類型本身不; 它是從中衍生所有的委派類型的類別類型。

C# 提供特殊語法委派具現化和引動過程。 除了在具現化，可以套用至類別或類別執行個體的任何作業也可以套用至委派的類別或執行個體，分別。 特別是，就可以存取的成員`System.Delegate`經由一般成員存取語法類型。

一組的委派執行個體所封裝的方法會呼叫的引動過程清單。 建立委派執行個體時 ([委派相容性](delegates.md#delegate-compatibility)) 從單一方法，它會封裝該方法中，且其引動過程清單包含只有一個項目。 不過，當合併兩個非 null 委派執行個體，其引動過程清單串連-左運算元，則右運算元-的順序，以形成新的引動過程清單，其中包含兩個或多個項目。

委派結合使用二進位`+`([加法運算子](expressions.md#addition-operator)) 及`+=`運算子 ([複合指派](expressions.md#compound-assignment))。 委派可以移除委派，使用二進位檔的組合`-`([減法運算子](expressions.md#subtraction-operator)) 及`-=`運算子 ([複合指派](expressions.md#compound-assignment))。 委派可以進行相等比較 ([委派等號比較運算子](expressions.md#delegate-equality-operators))。

下列範例示範數個委派具現化，其對應的引動過程清單：

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

當`cd1`和`cd2`會具現化，它們每個封裝一種方法。 當`cd3`是具現化，它有兩個方法的引動過程清單`M1`和`M2`，依此順序。 `cd4`引動過程清單包含`M1`， `M2`，和`M1`，依此順序。 最後，`cd5`的引動過程清單包含`M1`， `M2`， `M1`， `M1`，和`M2`，依此順序。 結合 （也會有一樣的移除） 委派的詳細範例，請參閱[委派引動過程](delegates.md#delegate-invocation)。

## <a name="delegate-compatibility"></a>委派相容性

方法或委派`M`已***相容***委派`D`符合下列所有項目時：

*  `D` 並`M`有相同數目的參數，並在每個參數`D`具有相同`ref`或`out`中的對應參數的修飾詞`M`。
*  針對每個值參數 (含參數`ref`或`out`修飾詞)，身分識別轉換 ([識別轉換](conversions.md#identity-conversion)) 或隱含參考轉換 ([隱含參考轉換](conversions.md#implicit-reference-conversions))在參數型別`D`中與對應參數類型`M`。
*  每個`ref`或是`out`中的參數，參數類型`D`等同於在參數型別`M`。
*  身分識別或隱含參考轉換存在的傳回型別`M`的傳回型別`D`。

## <a name="delegate-instantiation"></a>委派具現化

委派的執行個體由*delegate_creation_expression* ([委派建立運算式](expressions.md#delegate-creation-expressions)) 或轉換成委派類型。 新建立的委派執行個體則會參考為：

*  中所參考的靜態方法*delegate_creation_expression*，或
*  目標物件 (它不能`null`) 和執行個體中所參考方法*delegate_creation_expression*，或
*  另一個委派。

例如: 

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

一旦具現化，委派執行個體一律會參考相同的目標物件和方法。 請記住，當合併兩個委派，或從另一個，新的委派結果，與它自己的引動過程清單，移除的其中一個結合，或移除的委派引動過程清單維持不變。

## <a name="delegate-invocation"></a>委派引動過程

C# 提供特殊的語法叫用委派。 叫用的非 null 委派執行個體，其引動過程清單包含一個項目時，它會叫用一個具有相同的引數，它提供，並傳回相同的值參考的方法至方法。 (請參閱[委派引動過程](expressions.md#delegate-invocations)的委派引動過程的詳細資訊。)如果這類委派引動過程中發生例外狀況，該例外狀況下，叫用之方法內未攔截到例外狀況 catch 子句的搜尋會繼續在方法呼叫的委派，視為已直接呼叫該方法委派的方法參考。

引動過程委派執行個體，其引動過程清單包含多個項目進行的每個方法引動過程清單中，依順序中以同步方式叫用。 所謂的每個方法會傳遞引數，如未提供以委派執行個體的同一組。 如果這類委派引動過程包含參考參數 ([參考參數](classes.md#reference-parameters))，每個方法引動過程會發生相同的變數的參考，但會變更該變數，方法引動過程清單中的一個方法才會顯示進一步的方法引動過程清單。 如果委派引動過程包含輸出參數或傳回值，其最終的值會來自清單中的最後一個委派的引動過程。

如果在處理這類委派的引動過程期間發生的例外狀況，並叫用之方法內未攔截到例外狀況，例外狀況 catch 子句的搜尋會繼續在方法呼叫的委派，並進一步向下的任何方法不會叫用引動過程清單。

嘗試叫用委派執行個體，其值是 null 類型的例外狀況會導致`System.NullReferenceException`。

下列範例示範如何具現化、 合併、 移除及叫用委派：

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

陳述式中所示`cd3 += cd1;`，委派可以多次出現在引動過程清單中。 在此情況下，它會直接叫用一次每次出現。 在這類引動過程清單，移除該委派時，引動過程清單中的最後一個相符項目是會實際移除。

最後一個陳述式中，執行前立即`cd3 -= cd1;`，委派`cd3`指的是空白的引動過程清單。 嘗試從空的清單移除委派 （或非空白清單中移除不存在的委派） 不是錯誤。

產生的輸出為：

```
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
