---
ms.openlocfilehash: 0a09585f4f885647230354c66a2449abb7ef1f44
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488768"
---
# <a name="interfaces"></a>介面

介面定義合約。 類別或結構實作介面必須遵守其合約。 介面可以繼承自多個基底介面，以及類別或結構可以實作多個介面。

介面可以包含方法、 屬性、 事件和索引子。 介面本身不提供它所定義之成員的實作。 介面只會指定必須提供的類別或結構實作介面成員。

## <a name="interface-declarations"></a>介面宣告

*Interface_declaration*是*type_declaration* ([型別宣告](namespaces.md#type-declarations))，宣告了新的介面型別。

```antlr
interface_declaration
    : attributes? interface_modifier* 'partial'? 'interface'
      identifier variant_type_parameter_list? interface_base?
      type_parameter_constraints_clause* interface_body ';'?
    ;
```

*Interface_declaration*組成的一組選擇性*屬性*([屬性](attributes.md))，後面接著一組選擇性的*interface_modifier*s ([介面修飾詞](interfaces.md#interface-modifiers))，後面接著選擇性`partial`修飾詞，後面接著關鍵字`interface`並*識別碼*可命名介面後面接著選擇性*variant_type_parameter_list*規格 ([Variant 類型參數清單](interfaces.md#variant-type-parameter-lists))，後面接著選擇性*interface_base*規格 ([的基底介面](interfaces.md#base-interfaces))，後面接著選擇性*type_parameter_constraints_clause*s 規格 ([類型參數條件約束](classes.md#type-parameter-constraints))後面接著*interface_body* ([介面主體](interfaces.md#interface-body))，或者後面接著一個分號。

### <a name="interface-modifiers"></a>介面修飾詞

*Interface_declaration*可以選擇性地包含一連串的介面修飾詞：

```antlr
interface_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | interface_modifier_unsafe
    ;
```

它是在 interface 宣告中出現多次相同的修飾詞的編譯時期錯誤。

`new`修飾詞只能在類別內定義的介面上。 它會指定介面會隱藏繼承的成員，以相同的名稱，如中所述[的新修飾詞](classes.md#the-new-modifier)。

`public`， `protected`， `internal`，和`private`修飾詞控制介面的存取範圍。 根據介面宣告發生所在的內容，只有部分這些修飾詞可能會允許 ([宣告存取範圍](basic-concepts.md#declared-accessibility))。

### <a name="partial-modifier"></a>Partial 修飾詞

`partial`修飾詞表示這*interface_declaration*是部分的型別宣告。 在封入命名空間或類型宣告同名的多個部分的介面宣告結合構成一個介面宣告，下列規則中指定[部分型別](classes.md#partial-types)。

### <a name="variant-type-parameter-lists"></a>Variant 類型參數清單

Variant 類型參數清單只能出現在介面和委派類型。 從一般的差異*type_parameter_list*s 是選擇性*variance_annotation*上每個類型參數。

```antlr
variant_type_parameter_list
    : '<' variant_type_parameters '>'
    ;

variant_type_parameters
    : attributes? variance_annotation? type_parameter
    | variant_type_parameters ',' attributes? variance_annotation? type_parameter
    ;

variance_annotation
    : 'in'
    | 'out'
    ;
```

如果變異數附註`out`，該型別參數要***covariant***。 如果變異數附註`in`，該型別參數要***contravariant***。 如果沒有任何變異數附註，該型別參數要***非變異***。

在範例
```csharp
interface C<out X, in Y, Z> 
{
  X M(Y y);
  Z P { get; set; }
}
```
`X` 是 covariant，`Y`是 contravariant 和`Z`為非變異。

#### <a name="variance-safety"></a>變異數的安全

類型參數清單中的類型的變異數附註的相符項目會限制型別可能會發生型別宣告內的位置。

型別`T`已***輸出 unsafe***如果下列其中一種保留：

*  `T` 為 contravariant 類型參數。
*  `T` 是輸出不安全的項目類型的陣列類型
*  `T` 是介面或委派的型別`S<A1,...,Ak>`建構自泛型型別的`S<X1,...,Xk>`至少一個 where`Ai`下列其中一種保留：
   * `Xi` 共變性或非變異值和`Ai`輸出不安全。
   * `Xi` 逆變性或非變異值和`Ai`是輸入安全。
   
型別`T`已***輸入 unsafe***如果下列其中一種保留：

*  `T` 為 covariant 類型參數。
*  `T` 是不安全的輸入項目類型的陣列類型
*  `T` 是介面或委派的型別`S<A1,...,Ak>`建構自泛型型別的`S<X1,...,Xk>`至少一個 where`Ai`下列其中一種保留：
   * `Xi` 共變性或非變異值和`Ai`輸入不安全。
   * `Xi` 逆變性或非變異值和`Ai`輸出不安全。

直接易懂的方式，輸出不安全的型別中的輸出位置，禁止使用，並禁止到輸入位置的輸入不安全的類型。

型別是***輸出安全***如果不是輸出不安全，並***輸入安全***如果不是輸入不安全。

#### <a name="variance-conversion"></a>變異數轉換

變異數附註的目的是提供較寬鬆 （但仍型別安全） 轉換成介面和委派類型。 這個最後的隱含定義 ([隱含轉換](conversions.md#implicit-conversions)) 和明確轉換 ([明確轉換](conversions.md#explicit-conversions)) 讓使用變異數可轉換性，其定義，如下所示的概念：

型別`T<A1,...,An>`可轉換變異數的型別`T<B1,...,Bn>`如果`T`介面或委派型別宣告具有 variant 類型參數`T<X1,...,Xn>`，和每個 variant 類型參數`Xi`下列其中之一保留：

*  `Xi` 是 covariant，且是隱含的參考或識別轉換從`Ai`至 `Bi`
*  `Xi` 而且遠端系統 contravariant 的隱含參考或識別轉換存在從`Bi`至 `Ai`
*  `Xi` 是不區分及身分識別轉換存在從`Ai`至 `Bi`

### <a name="base-interfaces"></a>基底介面

介面可以繼承自零或多個介面型別，稱為***明確的基底介面***的介面。 當介面有一或多個明確的基底介面時，該介面，宣告中的介面識別碼接著冒號和逗號分隔的基底介面型別清單。

```antlr
interface_base
    : ':' interface_type_list
    ;
```

建構的介面類型，明確的基底介面明確的基底介面宣告的泛型型別宣告，並取代，每個形成*type_parameter*基底介面中宣告對應*type_argument*建構的類型。

明確的基底介面的介面必須至少像一樣地存取介面本身 ([協助工具的條件約束](basic-concepts.md#accessibility-constraints))。 比方說，它是指定的編譯時期錯誤`private`或是`internal`介面中*interface_base*的`public`介面。

它是介面，以直接或間接繼承自它自己的編譯時期錯誤。

***的基底介面***介面是明確的基底介面和其基底介面。 換句話說，基底介面的集合會是完整的可轉移關閉的明確的基底介面，其明確的基底介面，以及等等。 介面會繼承其基底介面的所有成員。 在範例
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

interface IComboBox: ITextBox, IListBox {}
```
基底介面`IComboBox`都`IControl`， `ITextBox`，和`IListBox`。

亦即`IComboBox`上述介面繼承成員`SetText`並`SetItems`以及`Paint`。

介面的每個基底介面都必須是輸出安全 ([變異數安全](interfaces.md#variance-safety))。 類別或結構也會隱含地實作介面會實作所有介面的基底介面。

### <a name="interface-body"></a>介面主體內

*Interface_body*介面會定義介面的成員。

```antlr
interface_body
    : '{' interface_member_declaration* '}'
    ;
```

## <a name="interface-members"></a>介面成員

介面的成員會繼承自基底介面的成員，介面本身所宣告的成員。

```antlr
interface_member_declaration
    : interface_method_declaration
    | interface_property_declaration
    | interface_event_declaration
    | interface_indexer_declaration
    ;
```

介面宣告可以宣告為零或多個成員。 介面成員必須是方法、 屬性、 事件或索引子。 介面不能包含常數、 欄位、 運算子、 執行個體建構函式、 解構函式或類型，也不介面可以包含任何類型的靜態成員。

所有介面成員以隱含方式都具有公用存取。 它是介面成員宣告，包括任何修飾詞的編譯時期錯誤。 特別是，介面成員不可以宣告和修飾詞`abstract`， `public`， `protected`， `internal`， `private`， `virtual`， `override`，或`static`。

此範例
```csharp
public delegate void StringListEvent(IStringList sender);

public interface IStringList
{
    void Add(string s);
    int Count { get; }
    event StringListEvent Changed;
    string this[int index] { get; set; }
}
```
宣告介面，包含其中每個可能的成員種類：方法、 屬性、 事件和索引子。

*Interface_declaration*建立新的宣告空間 ([宣告](basic-concepts.md#declarations))，而*interface_member_declaration*立即包含s*interface_declaration*這個宣告空間中引入新的成員。 下列規則適用於*interface_member_declaration*s:

*  方法的名稱必須與不同的所有屬性和事件在相同的介面中宣告的名稱。 此外，簽章 ([簽章和多載](basic-concepts.md#signatures-and-overloading)) 的方法必須與相同的介面中宣告的所有其他方法的簽章不同，在相同的介面中宣告的兩種方法可能沒有簽章，差異在於完全`ref`和`out`。
*  屬性或事件的名稱必須與相同介面中宣告的所有其他成員的名稱不同。
*  索引子的簽章必須與相同介面中所宣告之其他所有索引子的簽章不同。

介面的繼承的成員是特別不屬於介面的宣告空間的一部分。 因此，介面可以宣告具有相同的名稱或簽章的成員，做為繼承的成員。 當發生這種情況時，衍生的介面成員即隱藏基底介面成員。 隱藏繼承的成員不被視為錯誤，但會造成編譯器將發出警告。 若要隱藏警告，衍生的介面成員的宣告必須包含`new`修飾詞來表示衍生的成員要隱藏基底的成員。 本主題會討論中進一步[經由繼承隱藏](basic-concepts.md#hiding-through-inheritance)。

如果`new`修飾詞加入的宣告，也不會隱藏繼承的成員，來指出發出警告。 藉由移除隱藏這個警告`new`修飾詞。

請注意，在類別成員`object`則不會，嚴格地說，任何介面的成員 ([介面成員](interfaces.md#interface-members))。 不過，在類別成員`object`可透過任何介面類型中進行成員查詢 ([成員查閱](expressions.md#member-lookup))。

### <a name="interface-methods"></a>介面方法

介面方法使用宣告*interface_method_declaration*s:

```antlr
interface_method_declaration
    : attributes? 'new'? return_type identifier type_parameter_list
      '(' formal_parameter_list? ')' type_parameter_constraints_clause* ';'
    ;
```

*屬性*， *return_type*，*識別碼*，以及*formal_parameter_list*的介面方法宣告都具有相同這表示的類別中的方法宣告 ([方法](classes.md#methods))。 若要指定方法的主體中，不允許介面方法宣告，並宣告因此一律會以分號結束。

介面方法的每個型式參數類型必須是輸入安全 ([變異數安全](interfaces.md#variance-safety))，而且傳回型別必須是`void`或輸出安全。 此外，每個類別類型條件約束、 介面類型條件約束和方法的任何型別參數的類型參數條件約束必須是輸入安全。

這些規則確保任何 covariant 或 contravariant 的介面的使用方式會保留型別安全。 例如，套用至物件的
```csharp
interface I<out T> { void M<U>() where U : T; }
```
不合法因為的使用方式`T`的型別參數條件約束為`U`不是輸入安全。

這項限制已不存在有可能違反類型安全，以下列方式：
```csharp
class B {}
class D : B{}
class E : B {}
class C : I<D> { public void M<U>() {...} }
...
I<B> b = new C();
b.M<E>();
```
這是實際的呼叫`C.M<E>`。 但是這種呼叫需要`E`衍生自`D`，因此這裡違反類型安全。

### <a name="interface-properties"></a>介面內容

介面屬性使用宣告*interface_property_declaration*s:

```antlr
interface_property_declaration
    : attributes? 'new'? type identifier '{' interface_accessors '}'
    ;

interface_accessors
    : attributes? 'get' ';'
    | attributes? 'set' ';'
    | attributes? 'get' ';' attributes? 'set' ';'
    | attributes? 'set' ';' attributes? 'get' ';'
    ;
```

*屬性*，*型別*，並*識別碼*介面屬性宣告的類別中有意義的屬性宣告相同 ([屬性](classes.md#properties))。

介面屬性宣告的存取子對應的類別屬性宣告存取子 ([存取子](classes.md#accessors))，不同之處在於存取子主體必須是分號。 因此，存取子只是指出屬性是讀寫、 唯讀或唯寫。

介面屬性的類型必須是輸出-安全，如果沒有 get 存取子，以及必須輸入為安全，如果沒有 set 存取子。

### <a name="interface-events"></a>介面事件

使用宣告介面事件*interface_event_declaration*s:

```antlr
interface_event_declaration
    : attributes? 'new'? 'event' type identifier ';'
    ;
```

*屬性*，*型別*，並*識別碼*介面事件宣告的類別中有相同的意義相同的事件宣告 ([事件](classes.md#events)).

介面事件的型別必須輸入為安全。

### <a name="interface-indexers"></a>介面索引子

使用宣告介面索引子*interface_indexer_declaration*s:

```antlr
interface_indexer_declaration
    : attributes? 'new'? type 'this' '[' formal_parameter_list ']' '{' interface_accessors '}'
    ;
```

*屬性*，*型別*，並*formal_parameter_list*介面索引子宣告的有意義相同的索引子宣告中的類別 ([索引子](classes.md#indexers))。

介面索引子宣告的存取子對應的類別索引子宣告存取子 ([索引子](classes.md#indexers))，不同之處在於存取子主體必須是分號。 因此，存取子只是表示索引子是讀寫、 唯讀或唯寫。

介面索引子的型式參數類型必須是輸入安全。 此外，任何`out`或`ref`型式參數類型必須也是輸出安全。 請注意，即使`out`參數必須是輸入安全，因為基礎執行平台的限制。

介面索引子的型別必須是輸出-安全，如果沒有 get 存取子，以及必須輸入為安全，如果沒有 set 存取子。

### <a name="interface-member-access"></a>介面成員存取

成員存取透過存取介面成員 ([成員存取](expressions.md#member-access)) 和索引子存取 ([索引子存取](expressions.md#indexer-access)) 形式的運算式`I.M`並`I[A]`，其中`I`介面類型時，`M`是方法、 屬性或事件，該介面型別，以及`A`是索引子引數清單。

絕對的介面的單一繼承 （每個介面繼承鏈結中的有剛好零個或一個直接的基底介面）、 成員查詢的影響 ([成員查閱](expressions.md#member-lookup))，方法引動過程 ([方法引動過程](expressions.md#method-invocations))，和索引子存取 ([索引子存取](expressions.md#indexer-access)) 規則並完全與類別和結構相同：更多衍生成員隱藏較少衍生的成員具有相同名稱或簽章。 不過，如多重繼承的介面，可能會發生模稜兩可，若有兩個或多個不相關的基底介面宣告具有相同的名稱或簽章的成員。 本節說明幾個範例，這種情況。 在所有情況下，明確轉換 （cast） 可用來解決模稜兩可。

在範例
```csharp
interface IList
{
    int Count { get; set; }
}

interface ICounter
{
    void Count(int i);
}

interface IListCounter: IList, ICounter {}

class C
{
    void Test(IListCounter x) {
        x.Count(1);                  // Error
        x.Count = 1;                 // Error
        ((IList)x).Count = 1;        // Ok, invokes IList.Count.set
        ((ICounter)x).Count(1);      // Ok, invokes ICounter.Count
    }
}
```
前兩個陳述式會造成編譯時期錯誤，因為成員查閱 ([成員查閱](expressions.md#member-lookup)) 的`Count`在`IListCounter`模稜兩可。 如範例所示，透過轉型來解決模稜兩可`x`成適當的基底介面型別。 這類轉換不需要執行階段成本 — 它們只包含做為衍生程度更小的類型在編譯時期檢視的執行個體。

在範例
```csharp
interface IInteger
{
    void Add(int i);
}

interface IDouble
{
    void Add(double d);
}

interface INumber: IInteger, IDouble {}

class C
{
    void Test(INumber n) {
        n.Add(1);                // Invokes IInteger.Add
        n.Add(1.0);              // Only IDouble.Add is applicable
        ((IInteger)n).Add(1);    // Only IInteger.Add is a candidate
        ((IDouble)n).Add(1);     // Only IDouble.Add is a candidate
    }
}
```
引動過程`n.Add(1)`選取`IInteger.Add`藉由套用的多載解析規則[多載解析](expressions.md#overload-resolution)。 同樣的引動過程`n.Add(1.0)`選取`IDouble.Add`。 當插入明確的轉型時，是只有一個候選項目方法，並因此沒有模稜兩可。

在範例
```csharp
interface IBase
{
    void F(int i);
}

interface ILeft: IBase
{
    new void F(int i);
}

interface IRight: IBase
{
    void G();
}

interface IDerived: ILeft, IRight {}

class A
{
    void Test(IDerived d) {
        d.F(1);                 // Invokes ILeft.F
        ((IBase)d).F(1);        // Invokes IBase.F
        ((ILeft)d).F(1);        // Invokes ILeft.F
        ((IRight)d).F(1);       // Invokes IBase.F
    }
}
```
`IBase.F`藉由隱藏成員`ILeft.F`成員。 引動過程`d.F(1)`因此會選取`ILeft.F`，即使`IBase.F`似乎不會逐步引導的存取路徑中隱藏`IRight`。

多重繼承的介面中隱藏的直覺式規則只是這個：如果成員隱藏的任何存取路徑中，它會隱藏所有的存取路徑中。 因為存取路徑，從`IDerived`來`ILeft`要`IBase`隱藏`IBase.F`，從的存取路徑中的成員也在隱藏`IDerived`來`IRight`來`IBase`。

## <a name="fully-qualified-interface-member-names"></a>完整的介面成員名稱

介面成員有時是由其***完整限定的名稱***。 介面成員的完整的名稱是由在其中成員是宣告，後面接著一個點，再加上該成員名稱的介面名稱所組成。 成員的完整限定的名稱參考該成員所宣告的介面。 例如，假設宣告
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}
```
完整限定的名稱`Paint`是`IControl.Paint`和 完整格式的名稱`SetText`是`ITextBox.SetText`。

在上述範例中，它不可以參考`Paint`做為`ITextBox.Paint`。

命名空間的一部分介面時，介面成員的完整的名稱包括命名空間名稱。 例如
```csharp
namespace System
{
    public interface ICloneable
    {
        object Clone();
    }
}
```

這裡，完整限定的名稱`Clone`方法是`System.ICloneable.Clone`。

## <a name="interface-implementations"></a>介面實作

可由類別和結構實作介面。 若要表示類別或結構直接實作的介面，基底類別清單的類別或結構中包含的介面識別項。 例如: 
```csharp
interface ICloneable
{
    object Clone();
}

interface IComparable
{
    int CompareTo(object other);
}

class ListEntry: ICloneable, IComparable
{
    public object Clone() {...}
    public int CompareTo(object other) {...}
}
```

類別或結構，直接也可以直接實作的介面會實作所有介面的基底介面的隱含。 這是 true，即使類別或結構並未明確列出基底類別清單中的所有基底介面。 例如：
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

class TextBox: ITextBox
{
    public void Paint() {...}
    public void SetText(string text) {...}
}
```

在這裡，類別`TextBox`會實作`IControl`和`ITextBox`。

當類別`C`直接實作一個介面，衍生自 C 的所有類別也隱含實作介面。 類別宣告中指定的基底介面可以是建構的介面型別 ([建構類型](types.md#constructed-types))。 基底介面不能，型別參數，但它可以包含在範圍中的型別參數。 下列程式碼說明如何實作類別和擴充建構的類型：
```csharp
class C<U,V> {}

interface I1<V> {}

class D: C<string,int>, I1<string> {}

class E<T>: C<int,T>, I1<T> {}
```

泛型類別宣告基底介面都必須滿足唯一性規則中所述[實作的介面的唯一性](interfaces.md#uniqueness-of-implemented-interfaces)。

### <a name="explicit-interface-member-implementations"></a>明確介面成員實作

為了實作介面，類別或結構可以宣告***明確介面成員實作***。 明確介面成員實作是方法、 屬性、 事件或索引子的宣告參考完整的介面成員名稱。 例如
```csharp
interface IList<T>
{
    T[] GetElements();
}

interface IDictionary<K,V>
{
    V this[K key];
    void Add(K key, V value);
}

class List<T>: IList<T>, IDictionary<int,T>
{
    T[] IList<T>.GetElements() {...}
    T IDictionary<int,T>.this[int index] {...}
    void IDictionary<int,T>.Add(int index, T value) {...}
}
```

以下`IDictionary<int,T>.this`和`IDictionary<int,T>.Add`是明確介面成員實作。

在某些情況下，可能無法適用於實作的類別，在此案例介面成員可能會使用實作明確介面成員實作介面成員的名稱。 類別實作檔案抽象概念，例如，可能會實作`Close`釋出的檔案資源的影響，而且實作的成員函式`Dispose`方法`IDisposable`介面使用明確的介面成員的實作：
```csharp
interface IDisposable
{
    void Dispose();
}

class MyFile: IDisposable
{
    void IDisposable.Dispose() {
        Close();
    }

    public void Close() {
        // Do what's necessary to close the file
        System.GC.SuppressFinalize(this);
    }
}
```

您不可能透過方法引動過程、 屬性存取或索引子存取中的完整限定名稱存取的明確介面成員實作。 明確介面成員實作只能透過介面執行個體，來存取，並在此情況下直接參考其成員名稱。

它是編譯時期錯誤，包括存取修飾詞，明確介面成員實作，它是編譯時期錯誤包含修飾詞`abstract`， `virtual`， `override`，或`static`。

明確介面成員實作各有不同的協助工具比其他成員的特性。 因為永遠不會存取其完整名稱中的方法引動過程或屬性存取透過明確介面成員實作，它們是在某種意義上私用。 不過，它們可以透過介面執行個體，因為它們是在某種意義上也是公用。

明確介面成員實作有兩個主要的用途：

*  因為無法存取類別或結構的執行個體透過明確介面成員實作，它們就會允許要排除之公用介面的類別或結構的介面實作。 此方法特別有用的類別或結構實作不感興趣的取用者，該類別或結構的內部介面。
*  明確介面成員實作可讓有相同的簽章的介面成員的去除混淆。 不含明確介面成員實作它是類別或結構不可能有不同的實作的介面具有相同的簽章的成員，並傳回型別，做為它是不可能的類別或結構有任何實作在所有具有相同的簽章，但具有不同的介面成員的傳回型別。

有效明確介面成員實作，類別或結構必須命名為其基底類別清單中，包含的成員，其完整的名稱、 類型和參數類型完全相符的明確介面成員的介面實作。 因此，在下列類別
```csharp
class Shape: ICloneable
{
    object ICloneable.Clone() {...}
    int IComparable.CompareTo(object other) {...}    // invalid
}
```
deklarace`IComparable.CompareTo`導致編譯時期錯誤，因為`IComparable`基底類別清單中未列出`Shape`並不是基底介面的`ICloneable`。 同樣地，在宣告中
```csharp
class Shape: ICloneable
{
    object ICloneable.Clone() {...}
}

class Ellipse: Shape
{
    object ICloneable.Clone() {...}    // invalid
}
```
deklarace`ICloneable.Clone`中`Ellipse`導致編譯時期錯誤，因為`ICloneable`基底類別清單中未明確列出`Ellipse`。

介面成員的完整的名稱必須參考該成員已宣告的介面。 因此，在宣告中
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

class TextBox: ITextBox
{
    void IControl.Paint() {...}
    void ITextBox.SetText(string text) {...}
}
```
明確介面成員實作`Paint`必須寫成`IControl.Paint`。

### <a name="uniqueness-of-implemented-interfaces"></a>實作介面的唯一性

泛型型別宣告所實作的介面必須維持唯一的所有可能的建構型別。 而不需要這項規則，就無法判斷要呼叫的特定建構類型的正確方法。 例如，假設允許泛型類別宣告的可寫入，如下所示：
```csharp
interface I<T>
{
    void F();
}

class X<U,V>: I<U>, I<V>                    // Error: I<U> and I<V> conflict
{
    void I<U>.F() {...}
    void I<V>.F() {...}
}
```

在允許這種情況，它會無法判斷哪一個程式碼執行在下列情況：
```csharp
I<int> x = new X<int,int>();
x.F();
```

若要判斷泛型類型宣告的介面清單是否有效，執行下列步驟：

*  可讓`L`是直接在泛型類別、 結構或介面宣告中指定的介面清單`C`。
*  若要新增`L`任何基底介面的介面已經在`L`。
*  移除任何重複項目從`L`。
*  如果任何可能會建構從建立的型別`C`型別引數會替代至之後會`L`，會導致在兩個介面`L`完全一樣的宣告`C`無效。 決定所有可能的建構型別時，不會考慮條件約束宣告。

在類別宣告`X`介面清單上方`L`組成`I<U>`和`I<V>`。 宣告無效，因為任何建構的型別`U`和`V`正在相同的型別會導致這兩個介面是相同的型別。

您可在整合不同的繼承層級指定的介面：
```csharp
interface I<T>
{
    void F();
}

class Base<U>: I<U>
{
    void I<U>.F() {...}
}

class Derived<U,V>: Base<U>, I<V>    // Ok
{
    void I<V>.F() {...}
}
```

此程式碼無效，即使`Derived<U,V>`會實作`I<U>`和`I<V>`。 程式碼
```csharp
I<int> x = new Derived<int,int>();
x.F();
```
叫用的方法`Derived`，因為`Derived<int,int>`有效地重新實作`I<int>`([重新實作的介面](interfaces.md#interface-re-implementation))。

### <a name="implementation-of-generic-methods"></a>泛型方法的實作

當泛型方法會隱含地實作介面方法，指定每個方法類型參數必須是兩個宣告中的對等 （之後任何介面類型會以適當的型別引數取代參數），其中的條件約束方法型別參數被識別序數位置，由左到右。

當泛型方法明確實作介面方法時，不過，不允許條件約束實作的方法。 相反地，條件約束被繼承自介面方法

```csharp
interface I<A,B,C>
{
    void F<T>(T t) where T: A;
    void G<T>(T t) where T: B;
    void H<T>(T t) where T: C;
}

class C: I<object,C,string>
{
    public void F<T>(T t) {...}                    // Ok
    public void G<T>(T t) where T: C {...}         // Ok
    public void H<T>(T t) where T: string {...}    // Error
}
```

此方法`C.F<T>`隱含地實作`I<object,C,string>.F<T>`。 在此情況下，`C.F<T>`不需要 （也不會允許） 指定的條件約束`T:object`因為`object`是隱含的所有類型參數條件約束。 此方法`C.G<T>`隱含地實作`I<object,C,string>.G<T>`因為條件約束符合那些在介面中之後介面型別參數會取代對應的型別引數。 方法的條件約束`C.H<T>`是錯誤，因為密封類型 (`string`在此情況下) 無法作為條件約束。 省略此條件約束會也會是個錯誤，因為隱含的介面方法實作的條件約束，才能符合。 因此，就無法以隱含方式實作`I<object,C,string>.H<T>`。 這個介面方法只可以使用明確介面成員實作來實作：
```csharp
class C: I<object,C,string>
{
    ...

    public void H<U>(U u) where U: class {...}

    void I<object,C,string>.H<T>(T t) {
        string s = t;    // Ok
        H<T>(t);
    }
}
```

在此範例中，明確介面成員實作會叫用具有嚴格較弱的條件約束的公用方法。 請注意，從指派`t`要`s`無效，因為`T`繼承的條件約束`T:string`，即使此條件約束不是在原始程式碼中表示。

### <a name="interface-mapping"></a>介面對應

類別或結構必須提供的介面的類別或結構的基底類別清單中列出的所有成員的實作。 尋找實作介面成員實作的類別或結構中的程序稱為***介面對應***。

類別或結構的介面對應`C`找出每個成員的基底類別清單中指定每個介面的實作`C`。 在特定介面成員的實作`I.M`，其中`I`是在其中的介面成員`M`宣告，會檢查每個類別或結構來判斷`S`開始，`C`和針對每個後續的基底類別的重複`C`，直到找到符合的：

*  如果`S`包含相符的明確介面成員實作宣告`I`並`M`，則這個成員是實作`I.M`。
*  否則，如果`S`包含的非靜態的公用成員符合宣告`M`，則這個成員是實作`I.M`。 如果超過一個成員的相符項目並未指定哪一個成員是實作`I.M`。 如果只可以發生這種情況`S`為建構的類型，其中泛型型別中宣告的兩個成員具有不同的簽章，但型別引數，使其簽章相同。

如果實作找不到指定的基底類別清單中的所有介面的所有成員，就會發生編譯時期錯誤`C`。 請注意，介面的成員會包含這些成員繼承自基底介面。

介面對應，類別成員的目的而言`A`符合介面成員`B`時：

*  `A` 並`B`方法，且名稱、 類型、 和型式參數清單`A`和`B`完全相同。
*  `A` 和`B`是屬性的名稱和型別`A`並`B`互異，和`A`具有相同的存取子實作為`B`(`A`不允許有其他存取子，如果它不是明確的介面成員實作）。
*  `A` 並`B`是事件，及名稱和型別`A`和`B`完全相同。
*  `A` 和`B`是索引子的型別和型式參數清單`A`並`B`互異，和`A`具有相同的存取子實作為`B`(`A`不允許有其他存取子，如果不是明確介面成員實作）。

值得注意的介面對應演算法的含意如下：

*  明確介面成員實作優先於其他相同類別或結構中的成員時決定實作介面成員的類別或結構成員。
*  或都不非公用靜態成員參與介面對應。

在範例
```csharp
interface ICloneable
{
    object Clone();
}

class C: ICloneable
{
    object ICloneable.Clone() {...}
    public object Clone() {...}
}
```
`ICloneable.Clone`隸屬`C`變成實作`Clone`在`ICloneable`因為明確介面成員實作的優先順序高於其他成員。

如果類別或結構實作了兩個或多個介面，包含具有相同名稱、 類型和參數類型的成員，就可以將每個介面成員拖曳到單一類別或結構成員。 例如
```csharp
interface IControl
{
    void Paint();
}

interface IForm
{
    void Paint();
}

class Page: IControl, IForm
{
    public void Paint() {...}
}
```

在這裡，`Paint`這兩種`IControl`並`IForm`會對應到`Paint`中的方法`Page`。 當然也很可能有兩種方法的另一個明確介面成員實作。

如果類別或結構實作介面，包含隱藏的成員，則某些成員必須一定實作透過明確介面成員實作。 例如
```csharp
interface IBase
{
    int P { get; }
}

interface IDerived: IBase
{
    new int P();
}
```

此介面的實作需要至少一個明確介面成員實作，並會採用下列格式的其中一個
```csharp
class C: IDerived
{
    int IBase.P { get {...} }
    int IDerived.P() {...}
}

class C: IDerived
{
    public int P { get {...} }
    int IDerived.P() {...}
}

class C: IDerived
{
    int IBase.P { get {...} }
    public int P() {...}
}
```

當類別實作多個具有相同的基底介面的介面時，可以有只有一個實作的基底介面。 在範例
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

class ComboBox: IControl, ITextBox, IListBox
{
    void IControl.Paint() {...}
    void ITextBox.SetText(string text) {...}
    void IListBox.SetItems(string[] items) {...}
}
```
不可以有不同的實作，如`IControl`在基底類別清單中，名為`IControl`繼承`ITextBox`，而`IControl`繼承`IListBox`。 事實上，沒有這些介面的個別身分識別的概念。 相反地，實作`ITextBox`和`IListBox`共用相同的實作`IControl`，和`ComboBox`只會被視為實作三個介面， `IControl`， `ITextBox`，和`IListBox`。

基底類別的成員會參與介面對應。 在範例
```csharp
interface Interface1
{
    void F();
}

class Class1
{
    public void F() {}
    public void G() {}
}

class Class2: Class1, Interface1
{
    new public void G() {}
}
```
此方法`F`中`Class1`用於`Class2`的實作`Interface1`。

### <a name="interface-implementation-inheritance"></a>介面實作繼承

類別會繼承其基底類別所提供的所有介面實作。

而不需要明確地***重新實作***介面衍生的類別無法以任何方式改變它繼承自其基底類別的介面對應。 例如，在宣告中
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    public void Paint() {...}
}

class TextBox: Control
{
    new public void Paint() {...}
}
```
`Paint`方法中的`TextBox`隱藏`Paint`中的方法`Control`，但不會更改對應`Control.Paint`拖曳至`IControl.Paint`，和呼叫`Paint`透過類別執行個體和介面執行個體將有下列效果
```csharp
Control c = new Control();
TextBox t = new TextBox();
IControl ic = c;
IControl it = t;
c.Paint();            // invokes Control.Paint();
t.Paint();            // invokes TextBox.Paint();
ic.Paint();           // invokes Control.Paint();
it.Paint();           // invokes Control.Paint();
```

不過，當介面方法對應到類別中的虛擬方法時，就可以為衍生的類別，以覆寫虛擬方法，然後修改介面的實作。 例如，重寫上述宣告
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    public virtual void Paint() {...}
}

class TextBox: Control
{
    public override void Paint() {...}
}
```
現在可觀察到下列的效果
```csharp
Control c = new Control();
TextBox t = new TextBox();
IControl ic = c;
IControl it = t;
c.Paint();            // invokes Control.Paint();
t.Paint();            // invokes TextBox.Paint();
ic.Paint();           // invokes Control.Paint();
it.Paint();           // invokes TextBox.Paint();
```

明確介面成員實作不能宣告為虛擬，因為它不可以覆寫明確介面成員實作。 不過，它是完全適用於呼叫另一個方法，明確介面成員實作，而且，其他方法可以宣告為虛擬化來允許衍生類別覆寫它。 例如
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    void IControl.Paint() { PaintControl(); }
    protected virtual void PaintControl() {...}
}

class TextBox: Control
{
    protected override void PaintControl() {...}
}
```

在這裡，衍生自`Control`可以是專為實作`IControl.Paint`藉由覆寫`PaintControl`方法。

### <a name="interface-re-implementation"></a>介面重新實作

繼承的介面實作的類別允許***重新實作***加在基底類別清單中的介面。

介面重新實作能遵循完全相同介面對應的規則初始介面的實作。 因此，繼承的介面對應沒有雅致的介面對應建立重新實作介面的任何作用。 例如，在宣告中
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    void IControl.Paint() {...}
}

class MyControl: Control, IControl
{
    public void Paint() {}
}
```
事實上，`Control`對應`IControl.Paint`拖曳至`Control.IControl.Paint`不會影響在重新實作`MyControl`，哪一個 map`IControl.Paint`拖曳至`MyControl.Paint`。

繼承的公用成員宣告和繼承的明確介面成員宣告加入重新實作介面的介面對應程序。 例如
```csharp
interface IMethods
{
    void F();
    void G();
    void H();
    void I();
}

class Base: IMethods
{
    void IMethods.F() {}
    void IMethods.G() {}
    public void H() {}
    public void I() {}
}

class Derived: Base, IMethods
{
    public void F() {}
    void IMethods.H() {}
}
```

這裡，實作`IMethods`中`Derived`對應至介面方法`Derived.F`， `Base.IMethods.G`， `Derived.IMethods.H`，和`Base.I`。

當類別實作介面，它會隱含也會實作所有介面的基底介面。 同樣地，重新實作也是介面的隱含的重新實作所有介面的基底介面。 例如
```csharp
interface IBase
{
    void F();
}

interface IDerived: IBase
{
    void G();
}

class C: IDerived
{
    void IBase.F() {...}
    void IDerived.G() {...}
}

class D: C, IDerived
{
    public void F() {...}
    public void G() {...}
}
```

這裡，重新實作`IDerived`也重新實作`IBase`對應`IBase.F`拖曳至`D.F`。

### <a name="abstract-classes-and-interfaces"></a>抽象類別和介面

如同非抽象類別，抽象類別必須提供的介面的類別的基底類別清單中列出的所有成員的實作。 不過，抽象類別可以對應至抽象方法的介面方法。 例如
```csharp
interface IMethods
{
    void F();
    void G();
}

abstract class C: IMethods
{
    public abstract void F();
    public abstract void G();
}
```

這裡，實作`IMethods`對應`F`並`G`至抽象方法，這必須覆寫在非抽象類別衍生自`C`。

請注意，明確介面成員實作都不能是抽象的但明確介面成員實作當然可以呼叫抽象方法。 例如
```csharp
interface IMethods
{
    void F();
    void G();
}

abstract class C: IMethods
{
    void IMethods.F() { FF(); }
    void IMethods.G() { GG(); }
    protected abstract void FF();
    protected abstract void GG();
}
```

這裡，衍生自非抽象類別`C`才能覆寫`FF`並`GG`，因此能提供的實際實作`IMethods`。
