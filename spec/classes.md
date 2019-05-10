---
ms.openlocfilehash: af7af574814dc04ee3ece0396b7ae5f86b3ec8eb
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488907"
---
# <a name="classes"></a>類別

類別是資料結構，其中可能包含資料成員 （常數和欄位），函式成員 （方法、 屬性、 事件、 索引子、 運算子、 執行個體建構函式、 解構函式和靜態建構函式） 和巢狀型別。 類別類型支援繼承，衍生的類別可以延伸及特製化的基底類別的其中一種機制。

## <a name="class-declarations"></a>類別宣告

A *class_declaration*是*type_declaration* ([型別宣告](namespaces.md#type-declarations)) 宣告的新類別。

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

A *class_declaration*組成的一組選擇性*屬性*([屬性](attributes.md))，後面接著一組選擇性的*class_modifier*s ([類別修飾詞](classes.md#class-modifiers))，後面接著選擇性`partial`修飾詞，後面接著關鍵字`class`並*識別碼*可命名的類別，後面接著選擇性*type_parameter_list* ([型別參數](classes.md#type-parameters))，後面接著選擇性*class_base*規格 ([類別的基底規格](classes.md#class-base-specification))，後面接著一組選擇性*type_parameter_constraints_clause*s ([類型參數條件約束](classes.md#type-parameter-constraints))，後面接著*class_body* ([類別主體](classes.md#class-body))，或者後面接著一個分號。

在類別宣告無法提供*type_parameter_constraints_clause*s 除非還會提供*type_parameter_list*。

提供的類別宣告*type_parameter_list*是***泛型類別宣告***。 此外，巢狀在泛型類別宣告或泛型結構宣告內的任何類別本身是泛型類別宣告，因為建立建構的型別都必須提供包含類型的型別參數。

### <a name="class-modifiers"></a>類別修飾詞

A *class_declaration*可以選擇性地包含一連串的類別修飾詞：

```antlr
class_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'abstract'
    | 'sealed'
    | 'static'
    | class_modifier_unsafe
    ;
```

它是在類別宣告中出現多次相同的修飾詞的編譯時期錯誤。

`new`巢狀類別使用修飾詞。 它會指定類別隱藏繼承的成員，以相同的名稱，如中所述[的新修飾詞](classes.md#the-new-modifier)。 它是編譯時期錯誤`new`出現在類別宣告內，不是巢狀的類別宣告修飾詞。

`public`， `protected`， `internal`，和`private`修飾詞可以控制類別的存取範圍。 根據在類別宣告發生所在的內容，這些修飾詞的一些可能不允許 ([宣告存取範圍](basic-concepts.md#declared-accessibility))。

`abstract`，`sealed`和`static`修飾詞會在下列各節中討論。

#### <a name="abstract-classes"></a>抽象類別

`abstract`修飾詞用來指出類別是不完整，且它用來只當做基底類別。 自非抽象類別的抽象類別是在利用下列方式有所不同：

*  抽象類別無法直接執行個體化，而且使用編譯時間錯誤`new`運算子在抽象類別。 雖然您可以將變數和其編譯時期型別是抽象的值，這類變數和值一定是會`null`或包含衍生自抽象類型的非抽象類別的執行個體的參考。
*  抽象類別是允許 （但非必要） 包含抽象成員。
*  無法密封抽象類別。

當非抽象類別衍生自抽象類別時，非抽象類別必須包含所有繼承抽象成員，藉此覆寫這些抽象成員的實際實作。 在範例
```csharp
abstract class A
{
    public abstract void F();
}

abstract class B: A
{
    public void G() {}
}

class C: B
{
    public override void F() {
        // actual implementation of F
    }
}
```
抽象類別`A`導入了抽象方法`F`。 類別`B`導入了另一種方法`G`，因為它並未提供的實作，但`F`，`B`也必須宣告為抽象。 類別`C`會覆寫`F`，並提供實際的實作。 因為沒有在抽象成員`C`， `C` ，允許 （但非必要） 為非抽象。

#### <a name="sealed-classes"></a>密封的類別

`sealed`修飾詞用來防止衍生的類別。 如果密封的類別指定為另一個類別的基底類別，就會發生編譯時期錯誤。

密封的類別不能也是抽象類別。

`sealed`修飾詞主要用來防止意外的衍生，但它也可讓執行階段的特定最佳化。 特別是，由於密封的類別永遠不會有任何衍生的類別，就可以將密封的類別執行個體上的虛擬函式成員引動過程轉換成非虛擬的引動過程。

#### <a name="static-classes"></a>靜態類別

`static`修飾詞用來將被宣告為類別標記***靜態類別***。 靜態類別無法具現化不能用做為類型，可以只包含靜態成員。 只有靜態類別可以包含宣告的擴充方法 ([擴充方法](classes.md#extension-methods))。

靜態類別宣告受限於下列限制：

*  靜態類別不能包含`sealed`或`abstract`修飾詞。 不過請注意，因為無法具現化或衍生自靜態類別，其行為就如同它已密封且抽象。
*  靜態類別不能包含*class_base*規格 ([類別的基底規格](classes.md#class-base-specification)) 並無法明確指定的基底類別或實作介面的清單。 靜態類別隱含繼承自型別`object`。
*  靜態類別只能包含靜態成員 ([靜態和執行個體成員](classes.md#static-and-instance-members))。 請注意，會將常數和巢狀型別歸類為靜態成員。
*  靜態類別不能有具有成員`protected`或`protected internal`宣告存取範圍。

它是編譯時期錯誤違反任何限制。

靜態類別有任何執行個體建構函式。 不可能在靜態類別中，執行個體建構函式和任何預設執行個體建構函式宣告 ([預設建構函式](classes.md#default-constructors)) 提供靜態類別。

靜態類別的成員自動靜態的而且非成員宣告必須明確包含`static`修飾詞 （除了常數和巢狀型別）。 類別是巢狀結構，靜態的外部類別內，巢狀的類別不是靜態類別，除非它明確包含`static`修飾詞。

__參考靜態類別類型__

A *namespace_or_type_name* ([命名空間和型別名稱](basic-concepts.md#namespace-and-type-names)) 可以參考靜態類別

*  *Namespace_or_type_name*是`T`中*namespace_or_type_name*格式的`T.I`，或
*  *Namespace_or_type_name*是`T`中*typeof_expression* ([引數清單](expressions.md#argument-lists)1) 的形式`typeof(T)`。

A *primary_expression* ([函式成員](expressions.md#function-members)) 可以參考靜態類別

*  *Primary_expression*是`E`中*member_access* ([編譯時期檢查動態的多載解析](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) 的形式`E.I`。

在任何其他內容中它是參考靜態類別的編譯時期錯誤。 比方說，它是靜態類別，以用做為基底類別，構成類型的錯誤 ([巢狀型別](classes.md#nested-types)) 成員 」、 「 泛用型別引數，或 「 型別參數條件約束。 同樣地，靜態類別不能在陣列類型、 指標類型`new`運算式，轉型運算式，`is`運算式`as`運算式，`sizeof`運算式或預設值運算式。

### <a name="partial-modifier"></a>Partial 修飾詞

`partial`修飾詞用來指示這個*class_declaration*是部分的型別宣告。 構成一個型別宣告，結合多個具有相同的名稱，在封入的命名空間或類型宣告中的部分型別宣告，下列規則中指定[部分型別](classes.md#partial-types)。

讓分散在不同區段的程式文字類別的宣告可以是很有用，如果這些區段會產生或保存在不同的內容。 比方說，在類別宣告的其中一部分可能是電腦產生，，而其他手動撰寫。 兩個文字分隔會防止一個之間發生衝突的更新，其他更新。

### <a name="type-parameters"></a>型別參數

型別參數是簡單的識別項，表示型別引數提供給建立建構的類型的預留位置。 型別參數是一種類型，稍後會提供正式預留位置。 相較之下，型別引數 ([型別引數](types.md#type-arguments)) 用來替代類型參數建構的型別建立時的實際類型。

```antlr
type_parameter_list
    : '<' type_parameters '>'
    ;

type_parameters
    : attributes? type_parameter
    | type_parameters ',' attributes? type_parameter
    ;

type_parameter
    : identifier
    ;
```

在類別宣告中的每個類型參數的宣告空間中定義的名稱 ([宣告](basic-concepts.md#declarations)) 該類別。 因此，它不能有另一個類型參數名稱相同，或在該類別中宣告的成員。 型別參數不能有型別本身相同的名稱。

### <a name="class-base-specification"></a>類別的基底規格

類別宣告可能包含*class_base*規格，其中定義的類別和介面的直接基底類別 ([介面](interfaces.md)) 直接由類別實作。

```antlr
class_base
    : ':' class_type
    | ':' interface_type_list
    | ':' class_type ',' interface_type_list
    ;

interface_type_list
    : interface_type (',' interface_type)*
    ;
```

類別宣告中指定的基底類別可以是建構的類別類型 ([建構類型](types.md#constructed-types))。 基底類別不能，型別參數，但它可以包含在範圍中的型別參數。

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a>基底類別

當*class_type*納入*class_base*，它會指定所要宣告類別的直接基底類別。 如果類別宣告不含任何*class_base*，或如果*class_base*清單只有介面類型、 直接的基底類別會假設為`object`。 類別會繼承自其直接基底類別成員中所述[繼承](classes.md#inheritance)。

在範例
```csharp
class A {}

class B: A {}
```
類別`A`要直接基底類別`B`，並`B`即為衍生自`A`。 由於`A`沒有未明確指定 直接基底類別，其直接基底類別會以隱含方式`object`。

建構的類別類型，如果在泛型類別宣告中，指定基底類別建構類型的基底類別是由取代來取得，每個*type_parameter*在基底類別宣告中，對應*type_argument*建構的類型。 指定泛型類別宣告
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
建構類型的基底類別`G<int>`會是`B<string,int[]>`。

類別類型的直接基底類別必須至少像一樣地存取自己的類別型別 ([存取範圍定義域](basic-concepts.md#accessibility-domains))。 比方說，它是編譯時期錯誤`public`類別來衍生自`private`或`internal`類別。

類別類型的直接基底類別不能任何下列類型： `System.Array`， `System.Delegate`， `System.MulticastDelegate`， `System.Enum`，或`System.ValueType`。 此外，無法使用泛型類別宣告`System.Attribute`為直接或間接基底類別。

在決定直接基底類別規格的意義`A`類別的`B`，直接基底類別`B`暫時會假設為`object`。 這確保基底類別規格的意義不能以遞迴方式的直覺的方式取決於本身。 範例：
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
處於錯誤，因為在基底類別規格`A<C.B>`直接基底類別`C`會被視為`object`，，因此 (的規則所[命名空間和型別名稱](basic-concepts.md#namespace-and-type-names))`C`不會被視為有一個成員`B`。

類別類型的基底類別是直接基底類別和其基底類別。 換句話說，一組基底類別是直接基底類別的關聯性可轉移關閉。 指的上述基底類別的範例`B`都`A`和`object`。 在範例
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
基底類別`D<int>`都`C<int[]>`， `B<IComparable<int[]>>`， `A`，和`object`。

除了類別`object`，每個類別類型有一個直接的基底類別。 `object`類別沒有直接的基底類別，而且所有其他類別的 ultimate 基底類別。

當類別`B`衍生自類別`A`，它是編譯時期錯誤`A`取決於`B`。 類別***直接相依於***其直接基底類別 （如果有的話） 和***直接相依於***所在它巢狀的類別 （如果有的話）。 指定此定義，一組完整的類別所相依的類別是的自反和轉移結束***直接相依於***關聯性。

此範例
```csharp
class A: A {}
```
因為類別取決於本身，則為錯誤。 同樣地，範例
```csharp
class A: B {}
class B: C {}
class C: A {}
```
因為類別循環相依在本身是錯誤。 最後，範例
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
導致編譯時期錯誤，因為`A`而定`B.C`（其直接基底類別），這取決於`B`（其立即封入類別），循環相依於`A`。

請注意，類別不會依賴巢狀類別。 在範例
```csharp
class A
{
    class B: A {}
}
```
`B` 取決於`A`(因為`A`同時為其直接基底類別和其立即封入類別)，但`A`不需依賴`B`(因為`B`不是基底類別或封入類別的`A`). 因此，此範例是有效的。

不可能衍生自`sealed`類別。 在範例
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
類別`B`處於錯誤，因為它會嘗試將衍生自`sealed`類別`A`。

#### <a name="interface-implementations"></a>介面實作

A *class_base*規格可能包含一份介面類型，在其中情況下，類別即直接實作特定的介面型別。 介面實作討論中進一步[介面實作](interfaces.md#interface-implementations)。

### <a name="type-parameter-constraints"></a>類型參數條件約束

泛型型別和方法的宣告可以選擇性地指定類型參數條件約束，藉以*type_parameter_constraints_clause*s。

```antlr
type_parameter_constraints_clause
    : 'where' type_parameter ':' type_parameter_constraints
    ;

type_parameter_constraints
    : primary_constraint
    | secondary_constraints
    | constructor_constraint
    | primary_constraint ',' secondary_constraints
    | primary_constraint ',' constructor_constraint
    | secondary_constraints ',' constructor_constraint
    | primary_constraint ',' secondary_constraints ',' constructor_constraint
    ;

primary_constraint
    : class_type
    | 'class'
    | 'struct'
    ;

secondary_constraints
    : interface_type
    | type_parameter
    | secondary_constraints ',' interface_type
    | secondary_constraints ',' type_parameter
    ;

constructor_constraint
    : 'new' '(' ')'
    ;
```

每個*type_parameter_constraints_clause*組成權杖`where`，後面接著型別參數的名稱，後面接著冒號和該型別參數條件約束的清單。 有最多可達一`where`子句，每個類型參數，而`where`子句可以依照任意順序列出。 像是`get`並`set`屬性存取子中的語彙基元`where`語彙基元不是關鍵字。

指定的條件約束清單`where`子句可以包含任何下列的元件，依此順序： 單一主要的條件約束、 一或多個次要條件約束，以及建構函式條件約束`new()`。

主要的條件約束可以是類別類型或***參考類型條件約束***`class`或***值類型條件約束*** `struct`。 第二個條件約束*type_parameter*或是*interface_type*。

參考類型條件約束指定型別參數使用的類型引數必須是參考型別。 所有類別類型、 介面類型、 委派類型、 陣列型別，以及已知為參考型別 （如有定義如下） 的型別參數都符合此條件約束。

實值類型條件約束指定型別參數使用的類型引數必須是不可為 null 的實值型別。 所有不可為 null 的結構類型、 列舉型別和實值類型條件約束的型別參數符合此條件約束。 請注意，雖然歸類為實值型別，可為 null 的型別 ([可為 Null 的型別](types.md#nullable-types)) 不符合的值類型條件約束。 也不能有具有實值類型條件約束的型別參數*constructor_constraint*。

指標類型永遠不允許為型別引數，而且不會被視為符合其中一個的參考類型或值類型條件約束。

如果類別型別、 介面型別或型別參數條件約束，該型別指定最小 「 基底類型 」 必須支援該類型參數所使用的每個類型引數。 每次使用建構的類型或泛型方法時，針對在編譯時期型別參數的條件約束檢查的類型引數。 提供的型別引數必須滿足的條件中所述[滿足條件約束](types.md#satisfying-constraints)。

A *class_type*條件約束必須滿足下列規則：

*  類型必須是類別類型。
*  型別不能`sealed`。
*  類型必須不是下列類型之一： `System.Array`， `System.Delegate`， `System.Enum`，或`System.ValueType`。
*  型別不能`object`。 因為所有的型別衍生自`object`，如果允許這類條件約束會有任何作用。
*  最多一個條件約束指定型別參數可以是類別類型。

為指定的型別*interface_type*條件約束必須滿足下列規則：

*  類型必須是介面類型。
*  型別必須不超過一次指定在給定`where`子句。

在任一情況下，可以包含任何相關聯的類型或方法宣告做為建構的類型，一部分的型別參數條件約束，而且可能是正在宣告的型別。

指定為型別參數條件約束必須是至少一樣地存取任何類別或介面類型 ([協助工具的條件約束](basic-concepts.md#accessibility-constraints)) 做為泛型類型或方法已宣告。

為指定的型別*type_parameter*條件約束必須滿足下列規則：

*  類型必須是型別參數。
*  型別必須不超過一次指定在給定`where`子句。

此外中必須有任何循環相依性圖形的型別參數，其中的相依性是以可轉移的關聯所定義：

*  如果型別參數`T`用作型別參數條件約束`S`再`S`***取決於*** `T`。
*  如果型別參數`S`型別參數而定`T`並`T`取決於型別參數`U`然後`S`***取決於*** `U`。

指定這個關聯性，就 （直接或間接） 相依於本身的類型參數的編譯時期錯誤。

任何條件約束必須是相依類型參數一致。 如果型別參數`S`取決於型別參數`T`然後：

*  `T` 不能有實值類型條件約束。 否則，請`T`實際上會密封，因此`S`會強制讓相同的型別`T`，不必將兩個類型參數。
*  如果`S`有值類型條件約束，然後`T`不能*class_type*條件約束。
*  如果`S`已經*class_type*條件約束`A`並`T`具有*class_type*條件約束`B`則必須是身分識別轉換或隱含參考轉換`A`要`B`或從隱含參考轉換`B`至`A`。
*  如果`S`也取決於型別參數`U`並`U`已*class_type*條件約束`A`並`T`具有*class_type*條件約束`B`則必須是識別轉換或從隱含參考轉換`A`要`B`或從隱含參考轉換`B`到`A`。

它是適用於`S`有實值類型條件約束和`T`有參考類型條件約束。 這會限制實際上`T`的型別`System.Object`， `System.ValueType`， `System.Enum`，以及任何介面類型。

如果`where`型別參數的子句會包含建構函式條件約束 (其中包含表單`new()`)，就可以使用`new`運算子來建立型別的執行個體 ([物件建立運算式](expressions.md#object-creation-expressions)). 任何類型使用的引數的建構函式條件約束的型別參數必須有公用無參數建構函式 （這個建構函式會隱含地存在任何實值類型） 或類型參數各有實值類型條件約束或建構函式條件約束 （請參閱[類型參數條件約束](classes.md#type-parameter-constraints)如需詳細資訊)。

條件約束的範例如下：
```csharp
interface IPrintable
{
    void Print();
}

interface IComparable<T>
{
    int CompareTo(T value);
}

interface IKeyProvider<T>
{
    T GetKey();
}

class Printer<T> where T: IPrintable {...}

class SortedList<T> where T: IComparable<T> {...}

class Dictionary<K,V>
    where K: IComparable<K>
    where V: IPrintable, IKeyProvider<K>, new()
{
    ...
}
```

下列範例會將發生錯誤，因為它造成循環相依性圖形的型別參數：
```csharp
class Circular<S,T>
    where S: T
    where T: S                // Error, circularity in dependency graph
{
    ...
}
```

下列範例會說明其他無效的情況：
```csharp
class Sealed<S,T>
    where S: T
    where T: struct        // Error, T is sealed
{
    ...
}

class A {...}

class B {...}

class Incompat<S,T>
    where S: A, T
    where T: B                // Error, incompatible class-type constraints
{
    ...
}

class StructWithClass<S,T,U>
    where S: struct, T
    where T: U
    where U: A                // Error, A incompatible with struct
{
    ...
}
```

***有效基底類別***型別參數的`T`定義如下：

*  如果`T`沒有主要的條件約束或類型參數條件約束，其有效的基底類別是`object`。
*  如果`T`具有值的類型條件約束，其有效的基底類別是`System.ValueType`。
*  如果`T`已經*class_type*條件約束`C`但沒有*type_parameter*條件約束，其有效的基底類別是`C`。
*  如果`T`沒有任何*class_type*條件約束，但有一或多個*type_parameter*條件約束，其有效的基底類別是最置於包含的型別 ([提昇的轉換運算子](conversions.md#lifted-conversion-operators)) 中的一組的有效基底類別其*type_parameter*條件約束。 一致性規則確保這類最置於包含的類型存在。
*  如果`T`同時具有*class_type*條件約束和一或多個*type_parameter*條件約束，其有效的基底類別是最置於包含的型別 ([提昇的轉換運算子](conversions.md#lifted-conversion-operators)) 所組成的集中*class_type*條件約束`T`和有效的基底類別的其*type_parameter*條件約束。 一致性規則確保這類最置於包含的類型存在。
*  如果`T`具有參考類型條件約束，但沒有*class_type*條件約束，其有效的基底類別是`object`。

為了這些規則，如果 T 具有條件約束`V`也就是說*value_type*，改為使用最特定的基底類型的`V`也就是說*class_type*。 這可以永遠不會發生在明確指定的條件約束，但可能會發生時的條件約束的泛型方法會隱含地繼承覆寫的方法宣告或明確實作介面方法。

這些規則確保有效的基底類別永遠*class_type*。

***有效的介面組***型別參數的`T`定義如下：

*  如果`T`沒有任何*secondary_constraints*，其有效的介面集是空的。
*  如果`T`已經*interface_type*條件約束，但沒有*type_parameter*條件約束，其有效的介面集是它的一組*interface_type*條件約束。
*  如果`T`沒有任何*interface_type*條件約束但具有*type_parameter*條件約束，其有效的介面組是有效的介面集合的聯集其*type_參數*條件約束。
*  如果`T`同時具有*interface_type*條件約束並*type_parameter*條件約束，其有效的介面集是它的一組的聯集*interface_type*條件約束和有效的介面組及其*type_parameter*條件約束。

型別參數是***已知為參考型別***如果它具有參考類型條件約束，或不是有效的基底類別`object`或`System.ValueType`。

受條件約束的型別參數類型的值可用來存取隱含的條件約束的執行個體成員。 在範例
```csharp
interface IPrintable
{
    void Print();
}

class Printer<T> where T: IPrintable
{
    void PrintOne(T x) {
        x.Print();
    }
}
```
方法`IPrintable`可叫用直接`x`因為`T`一律實作受限於`IPrintable`。

### <a name="class-body"></a>在類別主體

*Class_body*類別會定義該類別的成員。

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a>部分型別

型別宣告可跨多個分割***部分的型別宣告***。 型別宣告是從建構其組件所遵循的規則，在此區段中，但它會被視為程式的編譯時期和執行階段處理期間的其餘部分的單一宣告。

A *class_declaration*， *struct_declaration*或是*interface_declaration*表示部分類型宣告，如果它包含`partial`修飾詞。 `partial` 不能是關鍵字，以及如果它出現在其中一個關鍵字之前，只會做為修飾詞`class`，`struct`或是`interface`在類型宣告中，或在型別前面`void`方法宣告中。 在其他內容中可以當做它正常的識別項。

部分型別宣告的每個部分必須包含`partial`修飾詞。 它必須具有相同的名稱，並在相同的命名空間或其他組件的型別宣告中宣告。 `partial`修飾詞表示的型別宣告的其他部分可能存在其他地方，但這類額外的組件是否存在，就不需要，它是適用於具有單一宣告的型別，以包含`partial`修飾詞。

使組件可以在編譯階段合併至單一類型宣告部分類型的所有組件必須一起編譯。 部分型別特別不允許已編譯的型別擴充。

巢狀型別可能會在多個部分宣告使用`partial`修飾詞。 一般而言，使用宣告包含型別`partial`自如，以及每個部分的巢狀型別是包含類型的不同組件中宣告。

`partial`委派或列舉宣告上不允許修飾詞。

### <a name="attributes"></a>屬性

在部分類型的屬性取決於未指定的順序，結合的各部分的屬性。 如果屬性位於多個部分，它就相當於型別上多次指定屬性。 例如，兩個部分：

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
相當於宣告這類：
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

型別參數的屬性結合以類似的方式。

### <a name="modifiers"></a>修飾詞

當部分的型別宣告包含協助工具規格 ( `public`， `protected`， `internal`，和`private`修飾詞) 它必須與所有其他部分，包括網頁可及性規格一致。 如果在部分類型的任何部分不包含協助工具規格，為型別指定適當的預設存取範圍 ([宣告存取範圍](basic-concepts.md#declared-accessibility))。

如果包含的巢狀類型的一或多個部分宣告`new`修飾詞，如果巢狀型別會隱藏繼承的成員，則會報告任何警告 ([經由繼承隱藏](basic-concepts.md#hiding-through-inheritance))。

如果一或多個部分宣告的類別包含`abstract`修飾詞，此類別會被視為抽象 ([抽象類別](classes.md#abstract-classes))。 否則，該類別被視為非抽象。

如果一或多個部分宣告的類別包含`sealed`修飾詞，此類別會被視為密封 ([密封類別](classes.md#sealed-classes))。 否則，該類別就會視為未密封。

請注意，類別不能同時為 abstract 和 sealed。

當`unsafe`修飾詞用在部分類型宣告中，只有該特定的組件會被視為不安全的內容 ([Unsafe 內容](unsafe-code.md#unsafe-contexts))。

### <a name="type-parameters-and-constraints"></a>型別參數和條件約束

如果多個組件中宣告為泛型型別，則每個組件必須狀態的型別參數。 每個組件必須有相同數目的型別參數，以及每個類型參數，相同的名稱順序。

當部分的泛型型別宣告包含條件約束 (`where`子句)，條件約束必須與包含條件約束的所有其他部分一致。 具體來說，每個部分都包含條件約束必須相同集合的型別參數的條件約束，而且每個類型參數的主要、 次要資料庫，且建構函式條件約束必須相等。 條件約束的兩個集合相等如果它們包含相同的成員。 如果部分的泛型型別不屬於指定類型參數條件約束，參數會被視為未受限制的型別。

此範例
```csharp
partial class Dictionary<K,V>
    where K: IComparable<K>
    where V: IKeyProvider<K>, IPersistable
{
    ...
}

partial class Dictionary<K,V>
    where V: IPersistable, IKeyProvider<K>
    where K: IComparable<K>
{
    ...
}

partial class Dictionary<K,V>
{
    ...
}
```
完全正確，因為這些組件，有效地包含條件約束 （前兩個） 指定的相同設定的主要、 次要和型別參數，一組相同的建構函式條件約束分別。

### <a name="base-class"></a>基底類別

部分類別宣告包含基底類別規格時，它必須與包含指定的基底類別的所有其他部分保持一致。 如果部分類別的任何部分不包含基底類別規格，就會變成基底類別`System.Object`([基底類別](classes.md#base-classes))。

### <a name="base-interfaces"></a>基底介面

在多個部分宣告類型的基底介面的集合是在每個組件上指定的基底介面的聯集。 在特定的基底介面只可以命名為一次在每個部分，但它允許多個組件，以便名稱相同的基底介面。 只能有一個實作，任何指定的基底介面的成員。

在範例
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
類別的基底介面的集合`C`已`IA`， `IB`，和`IC`。

通常，每個組件會提供一個在該部分; 宣告的介面實作不過，這不是需求。 組件可能會提供在不同的部分宣告介面的實作：
```csharp
partial class X
{
    int IComparable.CompareTo(object o) {...}
}

partial class X: IComparable
{
    ...
}
```

### <a name="members"></a>成員

除了部分方法 ([部分方法](classes.md#partial-methods))，在多個部分宣告類型的成員就會直接在每個組件中宣告的成員集合的聯集。 型別宣告的所有部分的主體會共用相同的宣告空間 ([宣告](basic-concepts.md#declarations))，以及每個成員的範圍 ([範圍](basic-concepts.md#scopes)) 延伸至所有部分的內文。 任一成員的存取範圍定義域一定會包含封入類型; 的所有組件`private`一個組件中宣告的成員可以自由存取另一個組件。 它是宣告相同的成員類型的多個組件中的編譯時期錯誤，除非該成員是具有的型別`partial`修飾詞。

```csharp
partial class A
{
    int x;                     // Error, cannot declare x more than once

    partial class Inner        // Ok, Inner is a partial type
    {
        int y;
    }
}

partial class A
{
    int x;                     // Error, cannot declare x more than once

    partial class Inner        // Ok, Inner is a partial type
    {
        int z;
    }
}
```

型別中的成員的排序是對 C# 程式碼，很少有重大影響，但配合其他語言及環境時可能會很大。 在這些情況下，在多個部分宣告的型別中的成員順序是未定義。

### <a name="partial-methods"></a>部分方法

部分方法可以定義的型別宣告某個部分，並在另一個實作。 實作是選擇性的。如果沒有任何部分實作部分方法，與部分方法宣告和所有的呼叫，它會從組件的組合所產生的型別宣告移除。

部分方法不能定義存取修飾詞，但是會以隱含方式`private`。 其傳回類型必須是`void`，而且它們的參數不能有`out`修飾詞。 識別項`partial`似乎之前時，才能夠辨識為特殊關鍵字方法宣告中`void`類型; 否則為一般識別項使用。 部分方法無法明確實作介面方法。

有兩種類型的部分方法宣告：如果方法宣告的主體是分號，宣告即為***定義部分方法宣告***。 如果主體指定為*區塊*，宣告要***實作部分方法宣告***。 各個型別宣告的組件可以有只有一個定義部分方法宣告，使用指定的簽章，而且可以有只有一個實作部分方法宣告，使用指定的簽章。 如果有實作部分方法宣告，對應的定義部分方法宣告必須存在，而且宣告必須符合為指定下列：

* 宣告必須有相同修飾詞 （雖然不一定是相同的順序），方法名稱、 型別參數數目和參數的數目。
* 對應的參數，在宣告中必須有相同修飾詞 （雖然不一定是在相同的順序） 和相同的型別 （模數型別參數名稱中的差異）。
* 在宣告中的對應型別參數必須有相同的條件約束 （模數型別參數名稱中的差異）。

實作部分方法宣告可以出現在對應的定義部分方法宣告為相同的組件。

只有一個定義的部分方法會參與多載解析。 因此，不論是否指定實作的宣告，引動過程運算式可能會解析成部分方法的引動過程。 因為部分方法一律會傳回`void`，這類引動過程運算式一律會運算式陳述式。 此外，因為部分方法會以隱含方式`private`，這類陳述式一定會發生在其中宣告部分方法的型別宣告的組件的其中一個內。

如果部分的型別宣告的任何部分不包含指定的部分方法的實作宣告，叫用它的任何運算式陳述式只會移除從組合的型別宣告。 因此引動過程運算式，包括任何構成的運算式，在執行階段沒有任何作用。 部分方法本身也會一併移除，並不會合併的類型宣告的成員。

如果指定的部分方法實作的宣告存在，則會保留部分方法的引動過程。 部分方法會造成在方法宣告類似於實作的部分方法宣告，但下列除外：

* `partial`修飾詞不包含
* 產生的方法宣告中的屬性會結合的屬性定義和實作部分方法宣告中未指定的順序。 不會移除重複項目。
* 產生的方法宣告的參數上的屬性會定義和實作部分方法宣告的對應參數結合的屬性中未指定的順序。 不會移除重複項目。

如果定義宣告，但不是實作宣告為部分方法 M 指定，就會適用下列限制：

* 它是編譯時期錯誤建立方法的委派 ([委派建立運算式](expressions.md#delegate-creation-expressions))。
* 它是編譯時期錯誤是指`M`內的匿名函式會轉換成運算式樹狀架構型別 ([匿名函式轉換成運算式樹狀架構類型的評估](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types))。
* 運算式發生過程的引動過程`M`不會影響明確指派狀態 ([明確指派](variables.md#definite-assignment))，這可能會導致編譯時期錯誤。
* `M` 不能為應用程式的進入點 ([應用程式啟動](basic-concepts.md#application-startup))。

部分方法可用於讓自訂行為的另一個組件，例如，一個由工具產生的型別宣告的一部分。 請考慮下列的部分類別宣告：
```csharp
partial class Customer
{
    string name;

    public string Name {
        get { return name; }
        set {
            OnNameChanging(value);
            name = value;
            OnNameChanged();
        }

    }

    partial void OnNameChanging(string newName);

    partial void OnNameChanged();
}
```

如果此類別編譯而不需要任何其他組件時，將會移除定義的部分方法宣告和其引動過程，和所產生之組合的類別宣告就相當於下列：
```csharp
class Customer
{
    string name;

    public string Name {
        get { return name; }
        set { name = value; }
    }
}
```

假設，另一個組件提供，不過，它提供的部分方法實作的宣告：
```csharp
partial class Customer
{
    partial void OnNameChanging(string newName)
    {
        Console.WriteLine("Changing " + name + " to " + newName);
    }

    partial void OnNameChanged()
    {
        Console.WriteLine("Changed to " + name);
    }
}
```

然後產生結合的類別宣告會相當於下列：
```csharp
class Customer
{
    string name;

    public string Name {
        get { return name; }
        set {
            OnNameChanging(value);
            name = value;
            OnNameChanged();
        }

    }

    void OnNameChanging(string newName)
    {
        Console.WriteLine("Changing " + name + " to " + newName);
    }

    void OnNameChanged()
    {
        Console.WriteLine("Changed to " + name);
    }
}
```

### <a name="name-binding"></a>名稱繫結

雖然每個部分的可延伸的型別必須相同的命名空間內宣告，組件通常是撰寫在不同的命名空間宣告中。 因此，不同`using`指示詞 ([Using 指示詞](namespaces.md#using-directives)) 可能會存在於每個部分。 當解譯的簡單名稱 ([型別推斷](expressions.md#type-inference)) 一組件內，只有`using`封入該部分之命名空間宣告的指示詞會被視為。 這可能會導致相同的識別項不同的組件中具有不同的意義：
```csharp
namespace N
{
    using List = System.Collections.ArrayList;

    partial class A
    {
        List x;                // x has type System.Collections.ArrayList
    }
}

namespace N
{
    using List = Widgets.LinkedList;

    partial class A
    {
        List y;                // y has type Widgets.LinkedList
    }
}
```

## <a name="class-members"></a>類別成員

類別的成員包含所引入的成員及其*class_member_declaration*從直接基底類別繼承的 s 和成員。

```antlr
class_member_declaration
    : constant_declaration
    | field_declaration
    | method_declaration
    | property_declaration
    | event_declaration
    | indexer_declaration
    | operator_declaration
    | constructor_declaration
    | destructor_declaration
    | static_constructor_declaration
    | type_declaration
    ;
```

類別類型的成員分成下列類別：

*  常數，表示與類別相關聯的常數值 ([常數](classes.md#constants))。
*  欄位，也就是類別的變數 ([欄位](classes.md#fields))。
*  實作的計算和類別可以執行動作的方法 ([方法](classes.md#methods))。
*  屬性，定義名為特性以及讀取和寫入這些特性相關聯的動作 ([屬性](classes.md#properties))。
*  定義可以類別所產生的通知的事件 ([事件](classes.md#events))。
*  索引子，允許相同的方式編製索引類別的執行個體 （語法） 做為陣列 ([索引子](classes.md#indexers))。
*  運算子定義可套用至類別的執行個體的運算式運算子 ([運算子](classes.md#operators))。
*  執行個體建構函式，實作初始化類別的執行個體所需的動作 ([執行個體建構函式](classes.md#instance-constructors))
*  解構函式，實作會永久捨棄類別執行個體之前要執行的動作 ([解構函式](classes.md#destructors))。
*  靜態建構函式，實作初始化類別本身所需的動作 ([靜態建構函式](classes.md#static-constructors))。
*  類型，代表本機類別的型別 ([巢狀型別](classes.md#nested-types))。

可以包含可執行程式碼的成員統稱為*函式成員*類別類型。 函式成員的類別類型是方法、 屬性、 事件、 索引子、 運算子、 執行個體建構函式、 解構函式和該類別類型的靜態建構函式。

A *class_declaration*建立新的宣告空間 ([宣告](basic-concepts.md#declarations))，而*class_member_declaration*立即所包含的 s*類別_declaration*這個宣告空間中引入新的成員。 下列規則適用於*class_member_declaration*s:

*  執行個體建構函式、 解構函式和靜態建構函式必須立即封入類別名稱相同。 所有其他成員必須具有與立即封入類別名稱不同的名稱。
*  常數、 欄位、 屬性、 事件或類型的名稱必須與其他相同類別中宣告的所有成員的名稱不同。
*  方法的名稱必須與所有其他非方法相同的類別中宣告的名稱不同。 此外，簽章 ([簽章和多載](basic-concepts.md#signatures-and-overloading)) 的方法必須不同於在相同類別中，宣告的所有其他方法的簽章，在相同類別中宣告的兩種方法可能沒有完全不同的簽章藉由`ref`和`out`。
*  執行個體建構函式的簽章必須不同於在相同類別中，宣告的所有其他執行個體建構函式的簽章，並在相同類別中宣告的兩個建構函式不能單獨由不同的簽章`ref`和`out`。
*  索引子的簽章必須與在相同類別中宣告的其他所有索引子的簽章不同。
*  運算子的簽章必須與其他相同類別中宣告的所有運算子的簽章不同。

類別類型的繼承的成員 ([繼承](classes.md#inheritance)) 不是類別的宣告空間的一部分。 因此，在衍生的類別可以宣告具有相同的名稱或簽章的繼承成員 （這實際上會隱藏繼承的成員）。

### <a name="the-instance-type"></a>執行個體類型

每個類別宣告都有相關聯的繫結型別 ([繫結和解除繫結型別](types.md#bound-and-unbound-types))，則***執行個體類型***。 對於泛型類別宣告，藉由建立建構的型別構成的執行個體類型 ([建構類型](types.md#constructed-types)) 型別宣告中，從與每個提供的型別所對應的引數型別參數。 由於執行個體類型使用的型別參數，它僅適用於在範圍內; 所在的型別參數也就是類別宣告中。 執行個體型別是種`this`類別宣告內撰寫的程式碼。 非泛型類別的執行個體類型會是只是宣告的類別。 下列範例顯示數個類別宣告，以及其執行個體類型： 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a>建構類型的成員

透過替代，每個所取得的是非繼承的成員建構的型別*type_parameter*在成員宣告中，對應*type_argument*建構的類型。 替代程序為基礎的型別宣告語意意義，而且不只是文字的替代。

例如，指定泛型類別宣告
```csharp
class Gen<T,U>
{
    public T[,] a;
    public void G(int i, T t, Gen<U,T> gt) {...}
    public U Prop { get {...} set {...} }
    public int H(double d) {...}
}
```
建構的型別`Gen<int[],IComparable<string>>`具有下列成員：
```csharp
public int[,][] a;
public void G(int i, int[] t, Gen<IComparable<string>,int[]> gt) {...}
public IComparable<string> Prop { get {...} set {...} }
public int H(double d) {...}
```

成員的型別`a`泛型類別宣告中`Gen`是 「 二維陣列`T`"，因此成員的型別`a`上述建構的類型是 「 二維陣列的一維陣列`int`"，或`int[,][]`。

在執行個體函式成員的型別`this`的執行個體類型 ([執行個體類型](classes.md#the-instance-type)) 包含宣告。

泛型類別的所有成員可以都使用從任何的封入類別的型別參數，直接或做為建構類型的一部分。 使用特定的關閉建構的型別 ([開放和封閉類型](types.md#open-and-closed-types)) 用在執行階段，每次使用的型別參數會取代實際的類型引數提供給建構類型。 例如: 
```csharp
class C<V>
{
    public V f1;
    public C<V> f2 = null;

    public C(V x) {
        this.f1 = x;
        this.f2 = this;
    }
}

class Application
{
    static void Main() {
        C<int> x1 = new C<int>(1);
        Console.WriteLine(x1.f1);        // Prints 1

        C<double> x2 = new C<double>(3.1415);
        Console.WriteLine(x2.f1);        // Prints 3.1415
    }
}
```

### <a name="inheritance"></a>繼承

類別***繼承***其直接基底類別型別的成員。 繼承意謂類別以隱含方式包含其直接基底類別型別，但執行個體建構函式、 解構函式和基底類別的靜態建構函式除外的所有成員。 繼承的一些重要層面是：

*  繼承可以轉移。 如果`C`衍生自`B`，並`B`衍生自`A`，然後`C`繼承中宣告的成員`B`中所宣告的成員以及`A`。
*  在衍生的類別會擴充其直接基底類別。 衍生類別可以在其繼承的成員中新增新的成員，但無法移除所繼承成員的定義。
*  不會繼承執行個體建構函式、 解構函式和靜態建構函式，但所有其他成員，不論其已宣告存取範圍 ([成員存取](basic-concepts.md#member-access))。 不過，根據其宣告的存取範圍，繼承的成員可能無法在衍生類別中存取。
*  在衍生的類別可以***隱藏***([經由繼承隱藏](basic-concepts.md#hiding-through-inheritance)) 藉由宣告具有相同的名稱或簽章的新成員的繼承成員。 請注意，不過，隱藏繼承的成員不會移除該成員 — 它只會讓該成員無法存取直接透過衍生的類別。
*  類別的執行個體包含一組宣告的類別和其基底類別，以及隱含轉換中的所有執行個體欄位 ([隱含參考轉換](conversions.md#implicit-reference-conversions)) 至其任何基底類別型別存在從衍生的類別的型別。 因此，某些衍生類別的執行個體的參考可以視為任何其基底類別的執行個體的參考。
*  類別可宣告虛擬方法、 屬性和索引子和衍生的類別可以覆寫這些函式成員的實作。 這可讓類別來呈現多型行為，其中函式成員引動過程所執行的動作，將根據叫用該函式成員的執行個體的執行階段類型而有所差異。

建構的類別類型的繼承的成員是直接基底類別型別的成員 ([基底類別](classes.md#base-classes))，其位所得到的建構類型的類型引數對應型別的每個項目中的參數*class_base*規格。 接著，這些成員，會轉換以替代，每個*type_parameter*在成員宣告中，對應*type_argument*的*class_base*規格。

```csharp
class B<U>
{
    public U F(long index) {...}
}

class D<T>: B<T[]>
{
    public T G(string s) {...}
}
```

在上述範例中，建構的型別`D<int>`有一個非繼承的成員`public int G(string s)`取得所得到的類型引數`int`做為 type 參數`T`。 `D<int>` 也有繼承的成員，從類別宣告`B`。 此繼承的成員由前，先判斷在基底類別型別`B<int[]>`的`D<int>`替代`int`如`T`基底類別規格中`B<T[]>`。 然後，作為類型引數`B`，`int[]`用來替代`U`中`public U F(long index)`，產生繼承的成員`public int[] F(long index)`。

### <a name="the-new-modifier"></a>New 修飾詞

A *class_member_declaration*允許宣告具有相同的名稱或簽章的成員，做為繼承的成員。 當發生這種情況時，即稱為衍生的類別成員***隱藏***基底類別成員。 隱藏繼承的成員不被視為錯誤，但會造成編譯器將發出警告。 若要隱藏警告，請在衍生的類別成員的宣告可以包含`new`修飾詞來表示衍生的成員要隱藏基底的成員。 本主題會討論中進一步[經由繼承隱藏](basic-concepts.md#hiding-through-inheritance)。

如果`new`修飾詞加入的宣告，也不會隱藏繼承的成員，該效果會發出警告。 藉由移除隱藏這個警告`new`修飾詞。

### <a name="access-modifiers"></a>存取修飾詞

A *class_member_declaration*可以有五種可能的宣告存取任何的範圍一個 ([宣告存取範圍](basic-concepts.md#declared-accessibility)): `public`， `protected internal`， `protected`， `internal`或`private`。 除了`protected internal`組合，它會指定一個以上的存取修飾詞的編譯時期錯誤。 當*class_member_declaration*不包含任何存取修飾詞，`private`假設。

### <a name="constituent-types"></a>構成類型

成員的宣告中所使用的類型稱為組成的型別，該成員。 可能構成類型是常數、 欄位、 屬性、 事件或索引子的型別、 方法或運算子的傳回型別和方法、 索引子、 運算子或執行個體建構函式的參數類型。 必須至少像一樣地存取該成員本身的成員組成的型別 ([協助工具的條件約束](basic-concepts.md#accessibility-constraints))。

### <a name="static-and-instance-members"></a>靜態和執行個體的成員

類別的成員都是***靜態成員***或是***執行個體成員***。 一般而言，最好將靜態成員屬於類別型別和物件 （類別類型的執行個體） 的執行個體成員。

當欄位、 方法、 屬性、 事件、 運算子或建構函式的宣告包含`static`修飾詞，它會宣告為靜態成員。 此外，常數或型別宣告隱含宣告的靜態成員。 靜態成員具有下列特性：

*  靜態成員時`M`簡稱*member_access* ([成員存取](expressions.md#member-access)) 形式的`E.M`，`E`必須表示型別，其中`M`。 它是編譯時期錯誤`E`代表執行個體。
*  靜態欄位只能識別一個儲存體位置的特定已關閉的類別類型的所有執行個體共用。 不論建立多少個特定的已關閉的類別類型執行個體，會只有一份靜態欄位。
*  靜態函式成員 （方法、 屬性、 事件、 運算子或建構函式） 不能進行特定的執行個體，並參考編譯時期錯誤`this`這類函式成員。

欄位、 方法、 屬性、 事件、 索引子、 建構函式或解構函式宣告不包括的當`static`修飾詞，它會宣告執行個體成員。 （執行個體成員有時也稱為非靜態成員。）執行個體成員具有下列特性：

*  當執行個體成員`M`中參考*member_access* ([成員存取](expressions.md#member-access)) 形式的`E.M`，`E`必須代表包含型別的執行個體`M`. 它會繫結時間錯誤`E`代表型別。
*  每個類別執行個體包含一組個別的類別的所有執行個體欄位。
*  指定類別的執行個體作業 （方法、 屬性、 索引子、 執行個體建構函式或解構函式） 執行個體函式成員，而且可以當做這個執行個體`this`([這項存取](expressions.md#this-access))。

下列範例說明的規則，來存取靜態和執行個體成員：
```csharp
class Test
{
    int x;
    static int y;

    void F() {
        x = 1;            // Ok, same as this.x = 1
        y = 1;            // Ok, same as Test.y = 1
    }

    static void G() {
        x = 1;            // Error, cannot access this.x
        y = 1;            // Ok, same as Test.y = 1
    }

    static void Main() {
        Test t = new Test();
        t.x = 1;          // Ok
        t.y = 1;          // Error, cannot access static member through instance
        Test.x = 1;       // Error, cannot access instance member through type
        Test.y = 1;       // Ok
    }
}
```

`F`方法所示範的是，在執行個體函式成員*simple_name* ([簡單名稱](expressions.md#simple-names)) 可用來存取執行個體成員和靜態成員。 `G`方法顯示在靜態函式成員中，它是編譯時期錯誤，以存取透過執行個體成員*simple_name*。 `Main`方法會顯示在*member_access* ([成員存取](expressions.md#member-access))、 執行個體成員必須透過執行個體和靜態成員必須透過型別。

### <a name="nested-types"></a>巢狀型別

在類別或結構宣告內宣告的型別稱為***巢狀類型***。 在編譯單位或命名空間中宣告的型別稱為***非巢狀型別***。

在範例
```csharp
using System;

class A
{
    class B
    {
        static void F() {
            Console.WriteLine("A.B.F");
        }
    }
}
```
類別`B`是巢狀的類型，因為它在類別中宣告`A`，和類別`A`是非巢狀類型，因為它宣告為編譯單位中。

#### <a name="fully-qualified-name"></a>完整格式的名稱

完整格式的名稱 ([完整名稱](basic-concepts.md#fully-qualified-names)) 為巢狀的類型`S.N`其中`S`是哪種類型中的型別完整格式的名稱`N`宣告。

#### <a name="declared-accessibility"></a>已宣告存取範圍

非巢狀型別可以有`public`或是`internal`宣告存取範圍，而且有`internal`預設宣告存取範圍。 巢狀型別可以有這些形式的宣告存取範圍，再加上一個或多個額外的宣告存取範圍，根據包含的類型是類別或結構形式：

*  類別中宣告的巢狀的類型可以具有任何已宣告存取範圍的五種 (`public`， `protected internal`， `protected`， `internal`，或`private`) 和其他類別和成員一樣，預設值為`private`宣告存取範圍。
*  巢狀的類型宣告的結構可以具有任何三種形式的宣告存取範圍 (`public`， `internal`，或`private`) 且如同其他的結構成員，預設值為`private`宣告存取範圍。

此範例
```csharp
public class List
{
    // Private data structure
    private class Node
    { 
        public object Data;
        public Node Next;

        public Node(object data, Node next) {
            this.Data = data;
            this.Next = next;
        }
    }

    private Node first = null;
    private Node last = null;

    // Public interface
    public void AddToFront(object o) {...}
    public void AddToBack(object o) {...}
    public object RemoveFromFront() {...}
    public object RemoveFromBack() {...}
    public int Count { get {...} }
}
```
宣告私用巢狀的類別`Node`。

#### <a name="hiding"></a>隱藏

可能會隱藏巢狀的類型 ([名稱隱藏](basic-concepts.md#name-hiding)) 基底的成員。 `new` ，以便可以明確表示隱藏巢狀的類型宣告使用修飾詞。 此範例
```csharp
using System;

class Base
{
    public static void M() {
        Console.WriteLine("Base.M");
    }
}

class Derived: Base 
{
    new public class M 
    {
        public static void F() {
            Console.WriteLine("Derived.M.F");
        }
    }
}

class Test 
{
    static void Main() {
        Derived.M.F();
    }
}
```
顯示巢狀的類別`M`，以隱藏方法`M`中所定義`Base`。

#### <a name="this-access"></a>此存取權

巢狀的類型和其包含類型沒有特殊的關聯性方面*this_access* ([此存取權](expressions.md#this-access))。 具體來說，`this`內巢狀類型不能用來參考包含類型的執行個體成員。 在巢狀的類型需要存取其包含類型的執行個體成員的位置的情況下，才能提供存取藉由提供`this`作為巢狀類型的建構函式引數包含型別的執行個體。 下列範例
```csharp
using System;

class C
{
    int i = 123;

    public void F() {
        Nested n = new Nested(this);
        n.G();
    }

    public class Nested
    {
        C this_c;

        public Nested(C c) {
            this_c = c;
        }

        public void G() {
            Console.WriteLine(this_c.i);
        }
    }
}

class Test
{
    static void Main() {
        C c = new C();
        c.F();
    }
}
```
顯示這項技術。 執行個體`C`建立的執行個體`Nested`，並傳遞它自己`this`要`Nested`的建構函式，以便後續存取`C`的執行個體成員。

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a>包含型別的 private 和 protected 成員的存取權

巢狀型別具有存取權的所有成員都可以存取其包含的類型，包括已包含的型別成員`private`和`protected`宣告存取範圍。 此範例
```csharp
using System;

class C 
{
    private static void F() {
        Console.WriteLine("C.F");
    }

    public class Nested 
    {
        public static void G() {
            F();
        }
    }
}

class Test 
{
    static void Main() {
        C.Nested.G();
    }
}
```
顯示的類別`C`，其中包含巢狀的類別`Nested`。 內`Nested`，方法`G`呼叫靜態方法`F`中定義`C`，和`F`具有私用宣告存取範圍。

巢狀型別也可以存取其包含類型的基底類型中定義的受保護的成員。 在範例
```csharp
using System;

class Base 
{
    protected void F() {
        Console.WriteLine("Base.F");
    }
}

class Derived: Base 
{
    public class Nested 
    {
        public void G() {
            Derived d = new Derived();
            d.F();        // ok
        }
    }
}

class Test 
{
    static void Main() {
        Derived.Nested n = new Derived.Nested();
        n.G();
    }
}
```
巢狀的類別`Derived.Nested`存取受保護的方法`F`中定義`Derived`的基底類別`Base`、 藉由呼叫的執行個體`Derived`。

#### <a name="nested-types-in-generic-classes"></a>泛型類別中的巢狀的類型

泛型類別宣告可以包含巢狀的類型宣告。 在封入類別的型別參數可用於巢狀型別。 巢狀的類型宣告可以包含其他的類型參數僅適用於巢狀型別。

包含在泛型類別宣告內的每個型別宣告為隱含泛型型別宣告。 當撰寫型別的參考巢狀泛型類型內時，包含建構的類型，包括其型別引數，必須具有名稱。 不過，從在外部類別中，巢狀型別可不搭配限定性條件;建構巢狀型別時，就可以隱含地使用外部類別的執行個體類型。 下列範例示範三種不同的正確方式來參考建構的型別，從建立`Inner`; 前兩個相等：
```csharp
class Outer<T>
{
    class Inner<U>
    {
        public static void F(T t, U u) {...}
    }

    static void F(T t) {
        Outer<T>.Inner<string>.F(t, "abc");      // These two statements have
        Inner<string>.F(t, "abc");               // the same effect

        Outer<int>.Inner<string>.F(3, "abc");    // This type is different

        Outer.Inner<string>.F(t, "abc");         // Error, Outer needs type arg
    }
}
```

雖然不正確，但程式設計風格，巢狀型別中的類型參數，即可隱藏成員或型別中的外部類型宣告的參數：
```csharp
class Outer<T>
{
    class Inner<T>        // Valid, hides Outer's T
    {
        public T t;       // Refers to Inner's T
    }
}
```

### <a name="reserved-member-names"></a>保留的成員名稱

基礎 C# 執行階段實作中，每個來源成員宣告的屬性、 事件或索引子，以便實作都必須保留兩個方法簽章的類型成員宣告，它的名稱和其類型為基礎。 它是編譯時期錯誤宣告的成員，其簽章符合其中一種保留簽章，即使基礎實作中，執行階段不會讓程式使用的保留區。

保留的名稱不會產生宣告，因此它們不會參與成員查詢。 不過，在宣告的相關聯保留簽章會參與繼承的方法 ([繼承](classes.md#inheritance))，而且可以使用隱藏`new`修飾詞 ([new 修飾詞](classes.md#the-new-modifier))。

保留這些名稱有三種用途：

*  若要允許使用一般的識別項為 get 方法名稱，或將存取設定為 C# 語言功能的基礎實作。
*  若要讓其他語言交互操作，使用一般的識別項為 get 方法名稱，或將存取設定為 C# 語言功能。
*  為了協助確保來源接受一個合格的編譯器接受由另一個，藉由保留成員的詳細資訊名稱一致跨所有 C# 實作。

解構函式宣告 ([解構函式](classes.md#destructors)) 也會導致保留的簽章 ([保留為解構函式的成員名稱](classes.md#member-names-reserved-for-destructors))。

#### <a name="member-names-reserved-for-properties"></a>保留給屬性的成員名稱

屬性`P`([屬性](classes.md#properties)) 的型別`T`，會保留下列簽章：

```csharp
T get_P();
void set_P(T value);
```

這兩個簽章都會保留，即使屬性是唯讀或唯寫。

在範例
```csharp
using System;

class A
{
    public int P {
        get { return 123; }
    }
}

class B: A
{
    new public int get_P() {
        return 456;
    }

    new public void set_P(int value) {
    }
}

class Test
{
    static void Main() {
        B b = new B();
        A a = b;
        Console.WriteLine(a.P);
        Console.WriteLine(b.P);
        Console.WriteLine(b.get_P());
    }
}
```
類別`A`唯讀屬性會定義`P`，因此保留簽章`get_P`和`set_P`方法。 類別`B`衍生自`A`並隱藏這兩個這些保留的簽章。 此範例會產生輸出：
```
123
123
456
```

#### <a name="member-names-reserved-for-events"></a>保留事件的成員名稱

事件`E`([事件](classes.md#events)) 的委派型別`T`，會保留下列簽章：
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a>保留給索引子的成員名稱

索引子 ([索引子](classes.md#indexers)) 的型別`T`使用的參數清單`L`，會保留下列簽章：
```csharp
T get_Item(L);
void set_Item(L, T value);
```

這兩個簽章都會保留，即使這個索引子會是唯讀或唯寫。

此外成員名稱`Item`已保留。

#### <a name="member-names-reserved-for-destructors"></a>保留的解構函式的成員名稱

類別包含解構函式 ([解構函式](classes.md#destructors))，會保留下列簽章：
```csharp
void Finalize();
```

## <a name="constants"></a>常數

A***常數***類別成員，表示為常數值： 可以在編譯時期計算的值。 A *constant_declaration*導入了一個或多個指定類型的常數。

```antlr
constant_declaration
    : attributes? constant_modifier* 'const' type constant_declarators ';'
    ;

constant_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;

constant_declarators
    : constant_declarator (',' constant_declarator)*
    ;

constant_declarator
    : identifier '=' constant_expression
    ;
```

A *constant_declaration*可能包含一組*屬性*([屬性](attributes.md))，則`new`修飾詞 ([new 修飾詞](classes.md#the-new-modifier))，和有效的四種存取修飾詞組合 ([存取修飾詞](classes.md#access-modifiers))。 屬性和修飾詞套用至所宣告的成員全部*constant_declaration*。 即使常數會被視為靜態成員而言*constant_declaration*都不需要也不允許`static`修飾詞。 它是相同的修飾詞在常數宣告中出現多次錯誤。

*型別*的*constant_declaration*指定宣告引入的成員型別。 類型後面接著一份*constant_declarator*s，其中每一個導入了新的成員。 A *constant_declarator*組成*識別項*可命名成員，後面接著"`=`"語彙基元，後面接著*constant_expression* ([常數運算式](expressions.md#constant-expressions)) 提供成員的值。

*型別*常數中指定的宣告必須能`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char`，`float`， `double`， `decimal`， `bool`， `string`，則*enum_type*，或有*reference_type*。 每個*constant_expression*必須使用 yield 產生值的目標型別或可以隱含的轉換方式轉換成目標類型的類型 ([隱含轉換](conversions.md#implicit-conversions))。

*型別*必須至少像常數本身那樣的常數 ([存取範圍條件約束](basic-concepts.md#accessibility-constraints))。

取得在運算式中使用的常數值*simple_name* ([簡單名稱](expressions.md#simple-names)) 或*member_access* ([成員存取](expressions.md#member-access))。

常數可以本身參與*constant_expression*。 因此，可能在任何需要的建構中使用常數*constant_expression*。 這類建構的範例包括`case`標籤`goto case`陳述式，`enum`成員宣告、 屬性和其他常數宣告。

中所述[常數運算式](expressions.md#constant-expressions)，則*constant_expression*是可以在編譯時期完整評估的運算式。 之後建立的非 null 值的唯一辦法*reference_type*以外`string`是套用`new`運算子，而由於`new`中不允許運算子*constant_運算式*，唯一可能的值的常數*reference_type*以外的 s`string`是`null`。

當想要的常數值的符號名稱，但該值類型時不允許在常數宣告中，或當無法在編譯時期所計算的值*constant_expression*、`readonly`欄位 （[唯讀欄位](classes.md#readonly-fields)) 可能會改為使用。

常數宣告會宣告多個常數，相當於具有相同的屬性、 修飾詞與類型的單一常數的多個宣告。 例如
```csharp
class A
{
    public const double X = 1.0, Y = 2.0, Z = 3.0;
}
```
相當於
```csharp
class A
{
    public const double X = 1.0;
    public const double Y = 2.0;
    public const double Z = 3.0;
}
```

常數可以相依於同一個程式內的其他常數，只要相依性不是循環的性質。 編譯器會自動排列要評估的常數宣告中適當的順序。 在範例
```csharp
class A
{
    public const int X = B.Z + 1;
    public const int Y = 10;
}

class B
{
    public const int Z = A.Y + 1;
}
```
編譯器會先評估`A.Y`，則會評估`B.Z`，然後最後評估`A.X`，產生的值`10`， `11`，和`12`。 常數宣告可能相依於其他程式中的常數，但這類相依性時，才可以在一個方向。 參考上述範例中，如果`A`並`B`已宣告於不同的程式，它是可行的`A.X`取決於`B.Z`，但`B.Z`不同時可能接著取決於`A.Y`。

## <a name="fields"></a>欄位

A***欄位***成員表示的物件或類別相關聯的變數。 A *field_declaration*導入了一個或多個指定類型的欄位。

```antlr
field_declaration
    : attributes? field_modifier* type variable_declarators ';'
    ;

field_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'readonly'
    | 'volatile'
    | field_modifier_unsafe
    ;

variable_declarators
    : variable_declarator (',' variable_declarator)*
    ;

variable_declarator
    : identifier ('=' variable_initializer)?
    ;

variable_initializer
    : expression
    | array_initializer
    ;
```

A *field_declaration*可能包含一組*屬性*([屬性](attributes.md))，則`new`修飾詞 ([new 修飾詞](classes.md#the-new-modifier))、有效的四種存取修飾詞組合 ([存取修飾詞](classes.md#access-modifiers))，以及`static`修飾詞 ([靜態和執行個體欄位](classes.md#static-and-instance-fields))。 颾魤 ㄛ *field_declaration*可能會包含`readonly`修飾詞 ([唯讀欄位](classes.md#readonly-fields)) 或`volatile`修飾詞 ([Volatile 欄位](classes.md#volatile-fields))，但非兩者。 屬性和修飾詞套用至所宣告的成員全部*field_declaration*。 它是相同的修飾詞，若要在欄位宣告中出現多次錯誤。

*型別*的*field_declaration*指定宣告引入的成員型別。 類型後面接著一份*variable_declarator*s，其中每一個導入了新的成員。 A *variable_declarator*組成*識別項*可命名該成員，並且選擇性地加 」`=`"語彙基元和*variable_initializer* ([變數初始設定式](classes.md#variable-initializers))，可讓該成員的初始值。

*型別*必須至少像欄位本身那樣的欄位 ([存取範圍條件約束](basic-concepts.md#accessibility-constraints))。

取得欄位的值中使用運算式*simple_name* ([簡單名稱](expressions.md#simple-names)) 或*member_access* ([成員存取](expressions.md#member-access))。 使用修改的非唯讀欄位的值*指派*([指派運算子](expressions.md#assignment-operators))。 非唯讀欄位的值可以是取得和使用修改後置遞增和遞減運算子 ([後置遞增和遞減運算子](expressions.md#postfix-increment-and-decrement-operators)) 和前置遞增和遞減運算子 ([前置詞遞增和遞減運算子](expressions.md#prefix-increment-and-decrement-operators))。

欄位宣告會宣告多個欄位相當於具有相同的屬性、 修飾詞與類型的單一欄位的多個宣告。 例如
```csharp
class A
{
    public static int X = 1, Y, Z = 100;
}
```
相當於
```csharp
class A
{
    public static int X = 1;
    public static int Y;
    public static int Z = 100;
}
```

### <a name="static-and-instance-fields"></a>靜態和執行個體欄位

當欄位宣告包含`static`修飾詞，宣告所引進的欄位都***靜態欄位***。 若未`static`修飾詞存在，則宣告所引進的欄位***執行個體欄位***。 靜態欄位和執行個體欄位是一種變數的兩個 ([變數](variables.md)) 支援 C# 中，而且有時候它們會稱為***靜態變數***和***執行個體變數***分別。

靜態欄位不是特定的執行個體; 的一部分相反地，它會在封閉類型的所有執行個體間共用 ([開放和封閉類型](types.md#open-and-closed-types))。 不論建立多少個已關閉的類別類型執行個體，沒有相關聯的應用程式定義域的靜態欄位的只有一個複本。

例如：
```csharp
class C<V>
{
    static int count = 0;

    public C() {
        count++;
    }

    public static int Count {
        get { return count; }
    }
}

class Application
{
    static void Main() {
        C<int> x1 = new C<int>();
        Console.WriteLine(C<int>.Count);        // Prints 1

        C<double> x2 = new C<double>();
        Console.WriteLine(C<int>.Count);        // Prints 1

        C<int> x3 = new C<int>();
        Console.WriteLine(C<int>.Count);        // Prints 2
    }
}
```

執行個體欄位所屬的執行個體。 具體來說，每個類別執行個體包含一組個別的該類別的所有執行個體欄位。

欄位中的參考時*member_access* ([成員存取](expressions.md#member-access)) 的表單`E.M`時，如果`M`是靜態欄位，`E`必須表示型別，其中`M`如果`M`是執行個體欄位，E 必須代表執行個體的型別，其中`M`。

靜態之間的差異和執行個體成員討論中進一步[靜態和執行個體成員](classes.md#static-and-instance-members)。

### <a name="readonly-fields"></a>唯讀欄位

當*field_declaration*包含`readonly`修飾詞，宣告所引進的欄位會***唯讀欄位***。 將唯讀欄位的直接指派只能當做執行個體建構函式或靜態的建構函式中相同的類別中宣告的一部分。 （將唯讀欄位可以指派給多個時間在這些內容中。）具體來說，直接指派至`readonly`欄位允許只在下列情況：

*  在  *variable_declarator*導入的欄位 (加*variable_initializer*宣告中)。
*  執行個體欄位，在包含欄位宣告; 類別的執行個體建構函式靜態欄位，則是在包含欄位宣告類別的靜態建構函式中進行。 這些也是它會將有效的唯一內容`readonly`欄位做為`out`或`ref`參數。

嘗試將指派給`readonly`欄位，或將它傳遞為`out`或`ref`在任何其他內容中的參數是編譯時期錯誤。

#### <a name="using-static-readonly-fields-for-constants"></a>使用靜態唯讀欄位常數

A`static readonly`欄位非常有用，當想要的常數值的符號名稱，但當值的型別中不允許`const`宣告，或當無法在編譯時期計算的值。 在範例
```csharp
public class Color
{
    public static readonly Color Black = new Color(0, 0, 0);
    public static readonly Color White = new Color(255, 255, 255);
    public static readonly Color Red = new Color(255, 0, 0);
    public static readonly Color Green = new Color(0, 255, 0);
    public static readonly Color Blue = new Color(0, 0, 255);

    private byte red, green, blue;

    public Color(byte r, byte g, byte b) {
        red = r;
        green = g;
        blue = b;
    }
}
```
`Black`， `White`， `Red`， `Green`，並`Blue`成員不可以宣告為`const`成員因為其值無法在編譯時期計算。 不過，宣告就`static readonly`改為具有許多相同的效果。

#### <a name="versioning-of-constants-and-static-readonly-fields"></a>常數和靜態唯讀欄位的版本控制

常數和唯讀欄位有不同的二進位版本設定語意。 當運算式參考常數時，在編譯時期取得常數的值，但是當運算式參考到唯讀欄位，欄位的值不會取得執行階段。 請考慮這兩個不同的程式所組成的應用程式：
```csharp
using System;

namespace Program1
{
    public class Utils
    {
        public static readonly int X = 1;
    }
}

namespace Program2
{
    class Test
    {
        static void Main() {
            Console.WriteLine(Program1.Utils.X);
        }
    }
}
```

`Program1`和`Program2`命名空間表示這兩個分開編譯的程式。 因為`Program1.Utils.X`宣告為靜態唯讀欄位的值輸出`Console.WriteLine`陳述式會在編譯時期不知道，但已取得在執行階段。 因此，如果值`X`變更並`Program1`重新編譯時，`Console.WriteLine`陳述式會將新的值，即使`Program2`未重新編譯。 不過，有`X`已常數的值`X`會取得當時`Program2`編譯，並不會受到影響的變更`Program1`直到`Program2`重新編譯。

### <a name="volatile-fields"></a>Volatile 欄位

當*field_declaration*包含`volatile`修飾詞，該宣告所引進的欄位會***volatile 欄位***。

對於靜態欄位，重新排列指示的最佳化技術可能會導致非預期且無法預期的結果，可存取欄位不需要同步處理，例如提供的多執行緒程式中*lock_statement* ([Lock 陳述式](statements.md#the-lock-statement))。 編譯器、 執行階段系統中，或硬體，可以執行這些最佳化。 Volatile 欄位，這種重新最佳化會限制：

*  Volatile 欄位讀取稱為***暫時性讀取***。 暫時性讀取具有 「 取得語意 」;也就是說，它保證記憶體發生之後，在指令序列中的任何參考之前發生。
*  Volatile 欄位的寫入稱為***暫時性寫入***。 暫時性寫入有 「 發行語意 」;也就是說，它保證任何的記憶體參考之前在指令序列中的寫入指令之後發生。

這些限制可確保所有的執行緒將會觀察任何其他執行緒執行的 volatile 寫入會按照其執行的順序進行。 合格的實作不是需要提供的單一總排序的變動性寫入所有執行緒的執行中所示。 Volatile 欄位的類型必須是下列其中一項：

*  A *reference_type*。
*  型別`byte`， `sbyte`， `short`， `ushort`， `int`， `uint`， `char`， `float`， `bool`， `System.IntPtr`，或` System.UIntPtr`。
*  *Enum_type*的列舉基底型別`byte`， `sbyte`， `short`， `ushort`， `int`，或`uint`。

此範例
```csharp
using System;
using System.Threading;

class Test
{
    public static int result;   
    public static volatile bool finished;

    static void Thread2() {
        result = 143;    
        finished = true; 
    }

    static void Main() {
        finished = false;

        // Run Thread2() in a new thread
        new Thread(new ThreadStart(Thread2)).Start();

        // Wait for Thread2 to signal that it has a result by setting
        // finished to true.
        for (;;) {
            if (finished) {
                Console.WriteLine("result = {0}", result);
                return;
            }
        }
    }
}
```
產生下列輸出：
```
result = 143
```

在此範例中，此方法`Main`啟動新的執行緒執行方法`Thread2`。 這個方法會將值儲存到名為的靜態欄位`result`，然後將儲存`true`volatile 欄位在`finished`。 在主執行緒等候欄位`finished`設為`true`，然後讀取欄位`result`。 由於`finished`已宣告`volatile`，在主執行緒必須讀取值`143`從欄位`result`。 如果欄位`finished`未宣告`volatile`，則會是允許的儲存庫至`result`之後的存放區會被主執行緒看見`finished`，，因此讀取值之主執行緒`0`從欄位`result`。 宣告`finished`做為`volatile`欄位會防止任何這類不一致。

### <a name="field-initialization"></a>欄位初始設定

初始欄位的值，它是靜態欄位或執行個體欄位，是否為預設值 ([預設值](variables.md#default-values)) 的欄位的類型。 您不可能再發生此預設值初始化，而且欄位因此永遠不會 「 未初始化 」 看到欄位的值。 此範例
```csharp
using System;

class Test
{
    static bool b;
    int i;

    static void Main() {
        Test t = new Test();
        Console.WriteLine("b = {0}, i = {1}", b, t.i);
    }
}
```
產生下列輸出
```
b = False, i = 0
```
因為`b`和`i`都會自動初始化為預設值。

### <a name="variable-initializers"></a>變數初始設定式

欄位宣告可能包含*variable_initializer*s。 變數初始設定式靜態欄位，對應類別初始設定期間所執行的指派陳述式。 執行個體欄位，變數的初始設定式會對應至類別的執行個體建立時執行的指派陳述式。

此範例
```csharp
using System;

class Test
{
    static double x = Math.Sqrt(2.0);
    int i = 100;
    string s = "Hello";

    static void Main() {
        Test a = new Test();
        Console.WriteLine("x = {0}, i = {1}, s = {2}", x, a.i, a.s);
    }
}
```
產生下列輸出
```
x = 1.4142135623731, i = 100, s = Hello
```
因為指派給`x`就會發生靜態欄位初始設定式的執行時，並指派給`i`和`s`執行個體欄位初始設定式執行時，會發生。

中所述的預設值，初始化[欄位初始化](classes.md#field-initialization)針對所有欄位，包括有變數的初始設定式的欄位，就會發生。 因此，初始化類別時，該類別中的所有靜態欄位會先初始化為其預設值，並接著靜態欄位初始設定式來執行文字順序。 同樣地，建立類別的執行個體時，該執行個體中的所有執行個體欄位會先初始化為其預設值，並接著執行個體欄位初始設定式來執行文字順序。

您可使用變數的初始設定式的預設值狀態的靜態欄位。 不過，這是樣式的強烈建議您不要。 此範例
```csharp
using System;

class Test
{
    static int a = b + 1;
    static int b = a + 1;

    static void Main() {
        Console.WriteLine("a = {0}, b = {1}", a, b);
    }
}
```
示範此行為。 儘管的循環定義，而 b，程式是有效。 它會導致輸出
```
a = 1, b = 2
```
因為靜態欄位`a`並`b`初始化為`0`(的預設值為`int`) 會執行其初始設定式之前。 時的初始設定式`a`執行時，值`b`為零，因此`a`初始化為`1`。 時的初始設定式`b`執行時，值`a`已經`1`，，因此`b`會初始化為`2`。

#### <a name="static-field-initialization"></a>靜態欄位初始設定

類別的靜態欄位變數初始設定式會對應至一系列以其出現在類別宣告中的文字順序所執行的設定。 如果靜態建構函式 ([靜態建構函式](classes.md#static-constructors)) 存在於類別中的靜態欄位初始設定式會執行之前執行該靜態建構函式。 否則靜態欄位初始設定式會執行該類別的靜態欄位的第一次使用之前視實作而定時間。 此範例
```csharp
using System;

class Test 
{ 
    static void Main() {
        Console.WriteLine("{0} {1}", B.Y, A.X);
    }

    public static int F(string s) {
        Console.WriteLine(s);
        return 1;
    }
}

class A
{
    public static int X = Test.F("Init A");
}

class B
{
    public static int Y = Test.F("Init B");
}
```
可能會產生下列輸出：
```
Init A
Init B
1 1
```
或下列輸出：
```
Init B
Init A
1 1
```
因為執行`X`的初始設定式和`Y`的初始設定式可以按照任何順序發生; 它們只受到這些欄位的參考之前發生。 不過，在此範例中：
```csharp
using System;

class Test
{
    static void Main() {
        Console.WriteLine("{0} {1}", B.Y, A.X);
    }

    public static int F(string s) {
        Console.WriteLine(s);
        return 1;
    }
}

class A
{
    static A() {}

    public static int X = Test.F("Init A");
}

class B
{
    static B() {}

    public static int Y = Test.F("Init B");
}
```
必須是輸出：
```
Init B
Init A
1 1
```
因為靜態建構函式時執行的規則 (中所定義[靜態建構函式](classes.md#static-constructors)) 提供`B`的靜態建構函式 (因此`B`的靜態欄位初始設定式) 必須先執行才能`A`的靜態建構函式和欄位初始設定式。

#### <a name="instance-field-initialization"></a>執行個體欄位初始設定

執行個體欄位變數初始設定式的類別對應至一系列的項目至其中的執行個體建構函式時立即執行的設定 ([建構函式初始設定式](classes.md#constructor-initializers)) 該類別。 變數的初始設定式會以其出現在類別宣告中的文字順序執行。 類別執行個體建立和初始化程序所述進一步[執行個體建構函式](classes.md#instance-constructors)。

執行個體欄位的變數初始設定式無法參考所建立的執行個體。 因此，它是編譯時期錯誤參考`this`中的變數初始設定式，因為它是參考透過任何執行個體成員變數初始設定式的編譯時期錯誤*simple_name*。 在範例
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
變數初始設定式`y`導致編譯時期錯誤，因為它參考所建立的執行個體的成員。

## <a name="methods"></a>方法

「方法」是實作物件或類別所能執行之計算或動作的成員。 方法使用宣告*method_declaration*s:

```antlr
method_declaration
    : method_header method_body
    ;

method_header
    : attributes? method_modifier* 'partial'? return_type member_name type_parameter_list?
      '(' formal_parameter_list? ')' type_parameter_constraints_clause*
    ;

method_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | 'async'
    | method_modifier_unsafe
    ;

return_type
    : type
    | 'void'
    ;

member_name
    : identifier
    | interface_type '.' identifier
    ;

method_body
    : block
    | '=>' expression ';'
    | ';'
    ;
```

A *method_declaration*可能包含一組*屬性*([屬性](attributes.md)) 和有效的四種存取修飾詞組合 ([存取修飾詞](classes.md#access-modifiers))，則`new`([的新修飾詞](classes.md#the-new-modifier))， `static` ([靜態和執行個體方法](classes.md#static-and-instance-methods))， `virtual` ([虛擬方法](classes.md#virtual-methods))， `override` ([覆寫方法](classes.md#override-methods))， `sealed` ([密封方法](classes.md#sealed-methods))， `abstract` ([抽象方法](classes.md#abstract-methods))，以及`extern` ([外部方法](classes.md#external-methods)) 修飾詞。

下列所有動作，則為 true 時，宣告就會擁有有效的修飾詞組合：

*  宣告包含有效的存取修飾詞組合 ([存取修飾詞](classes.md#access-modifiers))。
*  宣告不包含相同的修飾詞多次。
*  宣告包含最多有一個下列修飾詞： `static`， `virtual`，和`override`。
*  宣告包含最多有一個下列修飾詞：`new`和`override`。
*  如果宣告包含`abstract`修飾詞，則宣告不包含任何下列修飾詞： `static`， `virtual`，`sealed`或`extern`。
*  如果宣告包含`private`修飾詞，則宣告不包含任何下列修飾詞： `virtual`， `override`，或`abstract`。
*  如果宣告包含`sealed`修飾詞，則宣告也包含`override`修飾詞。
*  如果宣告包含`partial`修飾詞，則它不包含任何下列修飾詞： `new`， `public`， `protected`， `internal`， `private`， `virtual`， `sealed`， `override``abstract`，或`extern`。

方法具有`async`修飾詞是非同步函式，並且遵循規則中所述[非同步函式](classes.md#async-functions)。

*Return_type*方法的宣告會指定方法所計算並傳回值的型別。 *Return_type*是`void`如果方法沒有傳回值。 如果宣告包含`partial`修飾詞，則傳回型別必須可`void`。

*Member_name*指定方法的名稱。 除非該方法是明確的介面成員實作 ([明確介面成員實作](interfaces.md#explicit-interface-member-implementations))，則*member_name*是只要*識別碼*。 明確介面成員實作，如*member_name*組成*interface_type*後面加上"`.`"和*識別碼*。

選擇性*type_parameter_list*指定之方法的型別參數 ([型別參數](classes.md#type-parameters))。 如果*type_parameter_list*的方法指定***泛型方法***。 如果方法沒有`extern`修飾詞*type_parameter_list*不能指定。

選擇性*formal_parameter_list*指定之方法的參數 ([方法參數](classes.md#method-parameters))。

選擇性*type_parameter_constraints_clause*指定個別的型別參數的條件約束 ([類型參數條件約束](classes.md#type-parameter-constraints))，才可指定如果和*type_parameter_清單*也提供，而且此方法沒有`override`修飾詞。

*Return_type*和每個參考中的型別*formal_parameter_list*必須至少像方法本身那樣的方法 ([存取範圍條件約束](basic-concepts.md#accessibility-constraints)).

*Method_body*是其中一個分號***陳述式主體***或是***運算式主體***。 陳述式主體所組成*區塊*，指定要叫用方法時執行的陳述式。 運算式主體所組成`=>`後面接著*運算式*和分號則代表叫用方法時要執行的單一運算式。 

針對`abstract`並`extern`方法*method_body*只包含一個分號。 針對`partial`方法*method_body*可能包含分號、 區塊主體或運算式的主體。 如需所有其他的方法*method_body*運算式主體或區塊的主體。

如果*method_body*包含一個分號，然後宣告不能包含`async`修飾詞。

名稱、 型別參數清單和方法的型式參數清單定義的簽章 ([簽章和多載](basic-concepts.md#signatures-and-overloading)) 的方法。 具體而言，方法的簽章包含其名稱、 型別參數的數目、 修飾詞和其型式參數類型的數目。 對於這些用途，不是由其名稱，但類型引數清單中之方法的序數位置識別之方法的型式參數的型別，就會發生的任何型別參數。傳回的型別不是方法簽章的一部分，也是類型參數或型式參數的名稱。

方法的名稱必須與所有其他非方法相同的類別中宣告的名稱不同。 此外，方法的簽章必須不同於在相同類別中，宣告的所有其他方法的簽章，而且在相同類別中宣告的兩種方法不能單獨由不同的簽章`ref`和`out`。

方法的*type_parameter*會在整個範圍中的 s *method_declaration*，而且可用來在該範圍中的表單類型*return_type*， *method_body*，並*type_parameter_constraints_clause*s 中使用，但是*屬性*。

所有的型式參數和型別參數必須有不同的名稱。

### <a name="method-parameters"></a>方法參數

方法的參數如果有的話，所宣告的方法*formal_parameter_list*。

```antlr
formal_parameter_list
    : fixed_parameters
    | fixed_parameters ',' parameter_array
    | parameter_array
    ;

fixed_parameters
    : fixed_parameter (',' fixed_parameter)*
    ;

fixed_parameter
    : attributes? parameter_modifier? type identifier default_argument?
    ;

default_argument
    : '=' expression
    ;

parameter_modifier
    : 'ref'
    | 'out'
    | 'this'
    ;

parameter_array
    : attributes? 'params' array_type identifier
    ;
```

型式參數清單包含一或多個以逗號分隔的參數的可能只有最後一個*parameter_array*。

A *fixed_parameter*組成的一組選擇性*屬性*([屬性](attributes.md))，選擇性`ref`，`out`或`this`修飾詞，*型別*，則*識別項*，以及一個選擇性*default_argument*。 每個*fixed_parameter*宣告指定的型別，具有指定名稱的參數。 `this`修飾詞將方法指定為擴充方法，而且只允許一種靜態方法的第一個參數上。 擴充方法會更詳盡的說明[擴充方法](classes.md#extension-methods)。

A *fixed_parameter*具有*default_argument*稱為***選擇性參數***，而*fixed_parameter*沒有*default_argument*是***必要的參數***。 必要的參數中的選擇性參數之後可能不會出現*formal_parameter_list*。

A`ref`或是`out`參數不能有*default_argument*。 *運算式*中*default_argument*必須是下列其中之一：

*  a *constant_expression*
*  格式的運算式`new S()`其中`S`是實值類型
*  格式的運算式`default(S)`其中`S`是實值類型

*運算式*必須由身分識別或可為 null 轉換成的類型參數的隱含轉換。

如果選擇性的參數會出現在實作部分方法宣告 ([部分方法](classes.md#partial-methods))，是明確的介面成員實作 ([明確介面成員實作](interfaces.md#explicit-interface-member-implementations)) 或單一參數的索引子宣告 ([索引子](classes.md#indexers)) 編譯器應該提供一則警告，因為可以永遠不會允許可省略的引數的方式叫用這些成員。

A *parameter_array*組成的一組選擇性*屬性*([屬性](attributes.md))，則`params`修飾詞， *array_type*，並*識別碼*。 參數陣列宣告具有指定名稱的指定的陣列類型的單一參數。 *Array_type*參數的陣列必須是單一維度陣列型別 ([陣列型別](arrays.md#array-types))。 在方法引動過程中，參數陣列可允許其中一個單一引數指定之陣列型別的指定，或允許零或多個陣列項目型別指定的引數。 參數陣列中所述進一步[參數陣列](classes.md#parameter-arrays)。

A *parameter_array*可能會發生後的選擇性參數，但不能有預設值--省略的引數*parameter_array*而是會導致建立空的陣列。

下列範例說明不同類型的參數：
```csharp
public void M(
    ref int      i,
    decimal      d,
    bool         b = false,
    bool?        n = false,
    string       s = "Hello",
    object       o = null,
    T            t = default(T),
    params int[] a
) { }
```

在  *formal_parameter_list*如`M`，`i`是必要的 ref 參數，`d`是必要的值參數， `b`， `s`，`o`和`t`是選擇性的值的參數和`a`是參數陣列。

在方法宣告會建立個別的宣告空間參數，型別參數和區域變數。 型別參數清單和方法的型式參數清單和區域變數宣告中，名稱會導入此宣告空間*區塊*的方法。 它是方法的宣告空間的兩個成員都有相同名稱的錯誤。 它是方法的宣告空間和巢狀的宣告空間，以包含項目具有相同名稱的本機變數的宣告空間錯誤。

方法引動過程 ([方法引動過程](expressions.md#method-invocations)) 建立複本，該引動過程的特定、 型式參數和區域變數、 方法和引動過程的引數清單中，指派值或變數參考新建立的型式參數。 內*區塊*可以在其識別項所參考的方法，型式參數*simple_name*運算式 ([簡單名稱](expressions.md#simple-names))。

有四種型式參數：

*  值參數，而不需要任何修飾詞宣告。
*  參考參數，以宣告`ref`修飾詞。
*  輸出參數，以宣告`out`修飾詞。
*  參數陣列，以宣告`params`修飾詞。

中所述[簽章和多載](basic-concepts.md#signatures-and-overloading)，則`ref`並`out`修飾詞是方法簽章的一部分，但`params`修飾詞不是。

#### <a name="value-parameters"></a>值參數

以任何修飾詞宣告的參數是實值參數。 實值參數相當於從對應的方法引動過程中提供的引數取得其初始值的區域變數。

實值參數的型式參數時，為方法引動過程中對應的引數必須是可以隱含轉換的運算式 ([隱含轉換](conversions.md#implicit-conversions)) 成正式參數類型。

方法允許將新的值指派給實值參數。 這類指派只會影響值參數所代表的本機儲存體位置，它們不影響實際方法引動過程中所提供的引數。

#### <a name="reference-parameters"></a>傳址參數

參數宣告`ref`修飾詞為參考參數。 不同於實值參數，參考參數不會建立新的存放位置。 相反地，參考參數會表示為給定的方法引動過程中的引數變數相同的儲存位置。

參考參數的型式參數時，對應的引數，方法引動過程中必須包含關鍵字`ref`後面接著*variable_reference* ([來判斷精確的規則明確指派](variables.md#precise-rules-for-determining-definite-assignment)) 的型式參數相同類型。 之前將它傳遞做為參考參數，就必須明確指派變數。

在方法中，參考參數一律會視為已明確指派。

方法宣告為迭代器 ([迭代器](classes.md#iterators)) 不能有參考參數。

此範例
```csharp
using System;

class Test
{
    static void Swap(ref int x, ref int y) {
        int temp = x;
        x = y;
        y = temp;
    }

    static void Main() {
        int i = 1, j = 2;
        Swap(ref i, ref j);
        Console.WriteLine("i = {0}, j = {1}", i, j);
    }
}
```
產生下列輸出
```
i = 2, j = 1
```

引動過程`Swap`中`Main`，`x`代表`i`並`y`代表`j`。 因此，引動過程的效果的值的交換`i`和`j`。

在方法中會參考參數，您有多個代表相同的儲存位置的名稱。 在範例
```csharp
class A
{
    string s;

    void F(ref string a, ref string b) {
        s = "One";
        a = "Two";
        b = "Three";
    }

    void G() {
        F(ref s, ref s);
    }
}
```
引動過程`F`中`G`的參照傳遞`s`兩者`a`和`b`。 因此，針對該的引動過程中，而名稱`s`， `a`，並`b`全都參考相同的儲存體位置，而所有的三個指派修改執行個體欄位`s`。

#### <a name="output-parameters"></a>輸出參數

參數宣告`out`修飾詞是一個 output 參數。 類似於參考參數，輸出參數不會建立新的存放位置。 相反地，輸出參數會表示為給定的方法引動過程中的引數變數相同的儲存位置。

輸出參數的型式參數時，對應的引數，方法引動過程中必須包含關鍵字`out`後面接著*variable_reference* ([來判斷精確的規則明確指派](variables.md#precise-rules-for-determining-definite-assignment)) 的型式參數相同類型。 變數必須未明確指派之前將它傳遞為 output 參數，但下列其中一個變數傳遞做為輸出參數的引動過程，變數會被視為已明確指派。

在方法內，就像在本機變數，output 參數一開始被視為未設定，並且必須確實指派，才能使用其值。

在方法傳回之前，就必須明確指派方法的每個輸出參數。

方法宣告為部分方法 ([部分方法](classes.md#partial-methods)) 或迭代器 ([迭代器](classes.md#iterators)) 不能有輸出參數。

輸出參數通常用於會產生多個傳回值的方法。 例如: 
```csharp
using System;

class Test
{
    static void SplitPath(string path, out string dir, out string name) {
        int i = path.Length;
        while (i > 0) {
            char ch = path[i - 1];
            if (ch == '\\' || ch == '/' || ch == ':') break;
            i--;
        }
        dir = path.Substring(0, i);
        name = path.Substring(i);
    }

    static void Main() {
        string dir, name;
        SplitPath("c:\\Windows\\System\\hello.txt", out dir, out name);
        Console.WriteLine(dir);
        Console.WriteLine(name);
    }
}
```

此範例會產生輸出：
```
c:\Windows\System\
hello.txt
```

請注意，`dir`並`name`變數可以是未指派，才能傳遞至`SplitPath`，而，它們會被視為已明確指派接在呼叫。

#### <a name="parameter-arrays"></a>參數陣列

參數宣告`params`修飾詞是參數陣列。 如果型式參數清單包括參數陣列，它必須是在清單中的最後一個參數，它必須是單一維度陣列型別。 例如，類型`string[]`並`string[][]`可用來當做陣列的型別參數，但類型`string[,]`不可以。 不可以結合`params`修飾詞與修飾詞`ref`和`out`。

參數陣列會允許在下列其中一種方法的引動過程中指定的引數：

*  指定參數陣列可以是隱含轉換的單一運算式的引數 ([隱含轉換](conversions.md#implicit-conversions)) 為參數陣列型別。 在此情況下，參數陣列的行為很類似值的參數。
*  或者，引動過程可以指定為參數陣列，其中每個引數是可以隱含地轉換運算式的零或多個引數 ([隱含轉換](conversions.md#implicit-conversions)) 參數陣列的項目型別。 在此情況下，叫用建立具有相對應的引數的長度參數陣列型別的執行個體初始化為指定的引數的值，將陣列執行個體的項目，做為新建立的陣列執行個體的實際引數。

除了允許引數數目可變的引動過程中，參數陣列就相當於實值參數 ([實值參數](classes.md#value-parameters)) 型別相同。

此範例
```csharp
using System;

class Test
{
    static void F(params int[] args) {
        Console.Write("Array contains {0} elements:", args.Length);
        foreach (int i in args) 
            Console.Write(" {0}", i);
        Console.WriteLine();
    }

    static void Main() {
        int[] arr = {1, 2, 3};
        F(arr);
        F(10, 20, 30, 40);
        F();
    }
}
```
產生下列輸出
```
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

第一個引動過程`F`只會將陣列傳遞`a`做為值參數。 第二個引動過程`F`會自動建立四個元素`int[]`與的指定項目值陣列做為值參數的執行個體的傳遞。 同樣地，第三個引動過程`F`會建立零個元素`int[]`，並將該執行個體傳遞做為值參數。 第二個和第三個引動過程是相當於寫入項目：
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

含有參數陣列的方法執行的多載解析時，可能適用的一般形式或在展開的形式 ([適用的函式成員](expressions.md#applicable-function-member))。 只有當方法的一般形式不適用，而且只有在具有相同的簽章和擴充形式的適用方法未在相同的型別中已經宣告使用展開的形式的方法。

此範例
```csharp
using System;

class Test
{
    static void F(params object[] a) {
        Console.WriteLine("F(object[])");
    }

    static void F() {
        Console.WriteLine("F()");
    }

    static void F(object a0, object a1) {
        Console.WriteLine("F(object,object)");
    }

    static void Main() {
        F();
        F(1);
        F(1, 2);
        F(1, 2, 3);
        F(1, 2, 3, 4);
    }
}
```
產生下列輸出
```
F();
F(object[]);
F(object,object);
F(object[]);
F(object[]);
```

在範例中，兩個含有參數陣列的方法可能展開的形式中已包含此類別做為一般方法。 這些擴充的形式因此時不會考慮執行多載解析，因此選取第一個和第三個方法引動過程的一般方法。 當類別宣告含有參數陣列的方法時，它不常見也包含一些一般方法為展開的形式。 這樣可避免將陣列的配置會叫用擴充方法以參數陣列的形式時，就會發生的執行個體。

參數陣列的型別時`object[]`，潛在的模稜兩可，就會發生之方法的一般形式與單一的擴充的形式之間`object`參數。 模稜兩可的原因在於`object[]`本身是隱含轉換成類型`object`。 模稜兩可不會有問題，不過，因為它可以解決插入所需的型別轉換。

此範例
```csharp
using System;

class Test
{
    static void F(params object[] args) {
        foreach (object o in args) {
            Console.Write(o.GetType().FullName);
            Console.Write(" ");
        }
        Console.WriteLine();
    }

    static void Main() {
        object[] a = {1, "Hello", 123.456};
        object o = a;
        F(a);
        F((object)a);
        F(o);
        F((object[])o);
    }
}
```
產生下列輸出
```
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

中的第一個和最後一個引動過程`F`的一般形式`F`適合，因為隱含轉換存在引數類型的參數類型 (兩者都是型別的`object[]`)。 因此，多載解析會選取的一般形式`F`，而且引數會當做一般的實值參數。 在第二個和第三個引動過程的一般形式`F`不適用，因為沒有隱含轉換存在引數類型的參數型別 (型別`object`無法以隱含方式轉換為類型`object[]`)。 不過的展開的表單`F`是適用的因此多載解析會選取它。 如此一來，一個項目`object[]`由引動過程，和指定的引數的值進行初始化陣列的單一項目 (而其本身是參考`object[]`)。

### <a name="static-and-instance-methods"></a>靜態和執行個體方法

當方法宣告包含`static`修飾詞，方法即稱為是靜態方法。 若未`static`修飾詞存在，方法要執行個體方法。

靜態方法無法運作以特定的執行個體和它是編譯時期錯誤是指`this`中的靜態方法。

指定類別的執行個體上運作的執行個體方法，且該執行個體可存取做為`this`([這項存取](expressions.md#this-access))。

方法中的參考時*member_access* ([成員存取](expressions.md#member-access)) 的表單`E.M`時，如果`M`是靜態方法，`E`必須代表已包含類型`M`，而且如果`M`是執行個體方法，`E`必須代表執行個體的型別，其中`M`。

靜態之間的差異和執行個體成員討論中進一步[靜態和執行個體成員](classes.md#static-and-instance-members)。

### <a name="virtual-methods"></a>虛擬方法

當執行個體方法宣告包含`virtual`修飾詞，方法即稱為是虛擬的方法。 若未`virtual`修飾詞存在，則此方法即為非虛擬方法。

非虛擬方法的實作是不變：是否在類別的執行個體上叫用方法的實作是相同的宣告或衍生類別的執行個體。 相反地，虛擬方法的實作可以取代由衍生類別中。 取代繼承虛擬方法的實作程序稱為***覆寫***該方法 ([覆寫方法](classes.md#override-methods))。

中的虛擬方法引動過程中，***執行階段類型***引動過程的執行個體的位置會決定要叫用的實際方法實作。 在非虛擬方法引動過程中，***編譯時期型別***執行個體是決定因素。 準確地說，當方法的名稱為`N`的引數清單叫用`A`編譯時期型別執行個體上`C`和執行階段型別`R`(其中`R`是`C`或衍生的類別從`C`)，叫用處理，如下所示：

*  首先，多載解析會套`C`， `N`，並`A`，以選取特定的方法`M`從一組方法中宣告，而且繼承`C`。 這中所述[方法引動過程](expressions.md#method-invocations)。
*  然後，如果`M`為非虛擬方法，`M`叫用。
*  否則，請`M`虛擬方法，且最具衍生性的實作`M`相對於`R`叫用。

對於每種虛擬方法中宣告或繼承的類別存在***最常衍生的實作***相對於該類別的方法。 虛擬方法的最具衍生性的實作`M`相對於類別`R`判斷方式如下：

*  如果`R`包含簡介`virtual`deklarace `M`，則這是最具衍生性的實作`M`。
*  否則，如果`R`包含`override`的`M`，則這是最具衍生性的實作`M`。
*  最多衍生實作的否則為`M`相對於`R`等同於最具衍生性的實作`M`方面的直接基底類別`R`。

下列範例說明虛擬和非虛擬方法之間的差異：
```csharp
using System;

class A
{
    public void F() { Console.WriteLine("A.F"); }

    public virtual void G() { Console.WriteLine("A.G"); }
}

class B: A
{
    new public void F() { Console.WriteLine("B.F"); }

    public override void G() { Console.WriteLine("B.G"); }
}

class Test
{
    static void Main() {
        B b = new B();
        A a = b;
        a.F();
        b.F();
        a.G();
        b.G();
    }
}
```

在範例中，`A`引進了非虛擬方法`F`和虛擬方法`G`。 此類別`B`引進一個新的非虛擬方法`F`，因此隱藏已繼承`F`，也會覆寫繼承的方法和`G`。 此範例會產生輸出：
```
A.F
B.F
B.G
B.G
```

請注意，此陳述式`a.G()`叫用`B.G`，而非`A.G`。 這是因為執行階段類型的執行個體 (即`B`)，不執行個體的編譯時間類型 (也就是`A`)，判斷要叫用的實際方法實作。

方法可以隱藏繼承的方法，因為它可能會包含數個具有相同的簽章的虛擬方法的類別。 這不會出現發生模稜兩可的問題，因為所有但最具衍生性的方法會隱藏。 在範例
```csharp
using System;

class A
{
    public virtual void F() { Console.WriteLine("A.F"); }
}

class B: A
{
    public override void F() { Console.WriteLine("B.F"); }
}

class C: B
{
    new public virtual void F() { Console.WriteLine("C.F"); }
}

class D: C
{
    public override void F() { Console.WriteLine("D.F"); }
}

class Test
{
    static void Main() {
        D d = new D();
        A a = d;
        B b = d;
        C c = d;
        a.F();
        b.F();
        c.F();
        d.F();
    }
}
```
`C`和`D`類別包含具有相同的簽章的兩個虛擬方法：其中一個所引入`A`所導入一個`C`。 所導入的方法`C`隱藏繼承自方法`A`。 因此，在覆寫宣告`D`所引入的方法就會覆寫`C`，而且不可能`D`覆寫所引入的方法`A`。 此範例會產生輸出：
```
B.F
B.F
D.F
D.F
```

請注意，它可以存取的執行個體來叫用虛擬方法隱藏的`D`透過更少衍生型別所在的方法不會隱藏。

### <a name="override-methods"></a>覆寫方法

當執行個體方法宣告包含`override`修飾詞，方法即稱為***覆寫方法***。 覆寫方法會覆寫具有相同的簽章的繼承虛擬方法。 虛擬方法宣告會導入新的方法，而覆寫方法宣告則是會提供現有已繼承之虛擬方法的新實作，來將該方法特製化。

藉由覆寫的方法`override`宣告就所謂***覆寫基底方法***。 覆寫方法`M`類別中宣告`C`，覆寫的基底方法會檢查每個基底類別型別來判斷`C`開始的直接基底類別型別，`C`並繼續每個後續直接基底類別型別，直到至少一個可存取的方法是位於其指定的基底類別型別中具有相同的簽章`M`之後替代型別引數。 尋找覆寫的基底方法的目的，方法會被視為它是否可存取`public`，則為`protected`，則為`protected internal`，或者它是否`internal`並且在相同的程式，做為宣告`C`。

除非下列所有條件皆成立的覆寫宣告，就會發生編譯時期錯誤：

*  覆寫的基底方法可以位於上面所述。
*  沒有一個這類覆寫基底方法。 只有在基底類別型別是建構的型別其中的型別引數替代讓兩個方法的簽章相同，這項限制會沒有作用。
*  覆寫基底方法，是虛擬、 抽象或覆寫方法。 換句話說，靜態或非虛擬，不能覆寫的基底方法。
*  覆寫的基底方法不是密封的方法。
*  覆寫方法，並覆寫的基底方法有相同的傳回類型。
*  覆寫宣告和覆寫的基底方法有相同的宣告存取範圍。 換句話說，覆寫宣告無法變更虛擬方法的存取範圍。 不過，如果受到內部覆寫的基底方法宣告，且它不同的組件中非組件包含的覆寫方法，則覆寫方法的宣告存取範圍必須受到保護。
*  覆寫宣告未指定類型參數的條件約束子句。 改為條件約束被繼承自基底方法，覆寫。 請注意，覆寫方法中的類型參數的條件約束可能會由繼承的條件約束中的型別引數取代。 這可能會導致不合法時明確指定，例如實值類型或密封的類型的條件約束。

下列範例會示範適用於泛型類別的覆寫規則的運作方式：
```csharp
abstract class C<T>
{
    public virtual T F() {...}
    public virtual C<T> G() {...}
    public virtual void H(C<T> x) {...}
}

class D: C<string>
{
    public override string F() {...}            // Ok
    public override C<string> G() {...}         // Ok
    public override void H(C<T> x) {...}        // Error, should be C<string>
}

class E<T,U>: C<U>
{
    public override U F() {...}                 // Ok
    public override C<U> G() {...}              // Ok
    public override void H(C<T> x) {...}        // Error, should be C<U>
}
```

覆寫宣告可以存取覆寫基底方法使用*base_access* ([基底存取](expressions.md#base-access))。 在範例
```csharp
class A
{
    int x;

    public virtual void PrintFields() {
        Console.WriteLine("x = {0}", x);
    }
}

class B: A
{
    int y;

    public override void PrintFields() {
        base.PrintFields();
        Console.WriteLine("y = {0}", y);
    }
}
```
`base.PrintFields()`中的引動過程`B`叫用`PrintFields`方法宣告中`A`。 A *base_access*停用虛擬引動過程的機制，並只將基底方法視為非虛擬方法。 在只有引動過程`B`已寫入`((A)this).PrintFields()`，它會以遞迴方式叫用`PrintFields`方法宣告中`B`，不是宣告中`A`，因為`PrintFields`是虛擬和執行階段類型的`((A)this)`是`B`。

只要包括`override`修飾詞可以方法覆寫另一種方法。 在其他情況下，使用相同的簽章，做為繼承的方法，只會隱藏繼承的方法。 在範例
```csharp
class A
{
    public virtual void F() {}
}

class B: A
{
    public virtual void F() {}        // Warning, hiding inherited F()
}
```
`F`方法中的`B`不包含`override`修飾詞，因此不會覆寫`F`中的方法`A`。 相反地，`F`方法中的`B`中的方法會隱藏`A`，並且回報警告，因為宣告不包括`new`修飾詞。

在範例
```csharp
class A
{
    public virtual void F() {}
}

class B: A
{
    new private void F() {}        // Hides A.F within body of B
}

class C: B
{
    public override void F() {}    // Ok, overrides A.F
}
```
`F`方法中的`B`隱藏虛擬`F`方法繼承自`A`。 因為新`F`中`B`具有私人存取權，其範圍只包含的類別主體`B`並不會延伸到`C`。 因此，宣告`F`中`C`允許覆寫`F`繼承自`A`。

### <a name="sealed-methods"></a>密封的方法

當執行個體方法宣告包含`sealed`修飾詞，方法即為***密封方法***。 如果執行個體方法宣告包含`sealed`修飾詞，也必須包含`override`修飾詞。 使用`sealed`修飾詞可防止在衍生的類別進一步覆寫方法。

在範例
```csharp
using System;

class A
{
    public virtual void F() {
        Console.WriteLine("A.F");
    }

    public virtual void G() {
        Console.WriteLine("A.G");
    }
}

class B: A
{
    sealed override public void F() {
        Console.WriteLine("B.F");
    } 

    override public void G() {
        Console.WriteLine("B.G");
    } 
}

class C: B
{
    override public void G() {
        Console.WriteLine("C.G");
    } 
}
```
類別`B`提供兩個覆寫的方法：`F`具有方法`sealed`修飾詞和`G`不的方法。 `B`使用。 密封`modifier`可防止`C`進一步地覆寫`F`。

### <a name="abstract-methods"></a>抽象方法

當執行個體方法宣告包含`abstract`修飾詞，方法即為***抽象方法***。 雖然抽象方法也會隱含地為虛擬方法，它不能有修飾詞`virtual`。

抽象方法宣告會引進一個新的虛擬方法，但不是提供該方法的實作。 相反地，非抽象衍生的類別必須提供自己的實作，藉由覆寫該方法。 抽象方法不提供任何實際的實作，因為*method_body*的抽象方法僅包含一個分號。

抽象方法宣告只允許出現在抽象類別 ([抽象類別](classes.md#abstract-classes))。

在範例
```csharp
public abstract class Shape
{
    public abstract void Paint(Graphics g, Rectangle r);
}

public class Ellipse: Shape
{
    public override void Paint(Graphics g, Rectangle r) {
        g.DrawEllipse(r);
    }
}

public class Box: Shape
{
    public override void Paint(Graphics g, Rectangle r) {
        g.DrawRect(r);
    }
}
```
`Shape`類別會定義可以自我繪製幾何形狀物件的抽象概念。 `Paint`方法是抽象的因為沒有任何有意義的預設實作。 `Ellipse`並`Box`類別是具體`Shape`實作。 因為這些類別是抽象的它們必須覆寫`Paint`方法，並提供實際的實作。

它是編譯時期錯誤*base_access* ([基底存取](expressions.md#base-access)) 來參考抽象方法。 在範例
```csharp
abstract class A
{
    public abstract void F();
}

class B: A
{
    public override void F() {
        base.F();                        // Error, base.F is abstract
    }
}
```
編譯時期錯誤報告之`base.F()`引動過程因為它參考了抽象方法。

抽象方法宣告，並允許覆寫虛擬方法。 這允許強制重新實作的方法在衍生類別中，抽象類別，並會使原始方法的實作無法使用。 在範例
```csharp
using System;

class A
{
    public virtual void F() {
        Console.WriteLine("A.F");
    }
}

abstract class B: A
{
    public abstract override void F();
}

class C: B
{
    public override void F() {
        Console.WriteLine("C.F");
    }
}
```
類別`A`宣告虛擬方法，而類別`B`覆寫此方法使用的抽象方法與類別`C`覆寫抽象的方法，以提供它自己的實作。

### <a name="external-methods"></a>外部方法

當方法宣告包含`extern`修飾詞，方法即為***外部方法***。 外部方法會實作在外部，通常會使用 C# 以外的語言。 外部方法宣告未提供實際實作，因為*method_body*的外部方法僅包含一個分號。 外部方法不可以是泛型。

`extern`修飾詞通常用於搭配`DllImport`屬性 ([COM 與 Win32 元件的互通性](attributes.md#interoperation-with-com-and-win32-components))，可讓 Dll （動態連結程式庫） 所實作的外部方法。 執行環境可能支援讓外部方法的實作可以提供其他機制。

當外部方法包含`DllImport`屬性，方法宣告必須也會包含`static`修飾詞。 此範例示範如何使用`extern`修飾詞和`DllImport`屬性：
```csharp
using System.Text;
using System.Security.Permissions;
using System.Runtime.InteropServices;

class Path
{
    [DllImport("kernel32", SetLastError=true)]
    static extern bool CreateDirectory(string name, SecurityAttribute sa);

    [DllImport("kernel32", SetLastError=true)]
    static extern bool RemoveDirectory(string name);

    [DllImport("kernel32", SetLastError=true)]
    static extern int GetCurrentDirectory(int bufSize, StringBuilder buf);

    [DllImport("kernel32", SetLastError=true)]
    static extern bool SetCurrentDirectory(string name);
}
```

### <a name="partial-methods-recap"></a>部分方法 （回顧）

當方法宣告包含`partial`修飾詞，方法即為***部分方法***。 部分方法只可以宣告為部分類型的成員 ([部分型別](classes.md#partial-types))，而且可能會有所限制的數目。 部分方法進一步詳述於[部分方法](classes.md#partial-methods)。

### <a name="extension-methods"></a>擴充方法

當方法的第一個參數包含`this`修飾詞，方法即為***擴充方法***。 只能在非泛型、 非巢狀的靜態類別中宣告擴充方法。 擴充方法的第一個參數可以有任何修飾詞以外`this`，以及參數類型不能是指標類型。

靜態類別會宣告兩個擴充方法的範例如下：
```csharp
public static class Extensions
{
    public static int ToInt32(this string s) {
        return Int32.Parse(s);
    }

    public static T[] Slice<T>(this T[] source, int index, int count) {
        if (index < 0 || count < 0 || source.Length - index < count)
            throw new ArgumentException();
        T[] result = new T[count];
        Array.Copy(source, index, result, 0, count);
        return result;
    }
}
```

擴充方法是一般的靜態方法。 此外，當其封入的靜態類別是在範圍內時，擴充方法可以叫用執行個體方法引動過程語法 ([擴充方法引動過程](expressions.md#extension-method-invocations))，做為第一個引數使用的接收者運算式。

下列程式會使用上面所宣告的擴充方法：
```csharp
static class Program
{
    static void Main() {
        string[] strings = { "1", "22", "333", "4444" };
        foreach (string s in strings.Slice(1, 2)) {
            Console.WriteLine(s.ToInt32());
        }
    }
}
```

`Slice`方法可用於`string[]`，而`ToInt32`方法位於`string`，因為它們已宣告為擴充方法。 程式的意義等同於下列，並使用一般的靜態方法呼叫：
```csharp
static class Program
{
    static void Main() {
        string[] strings = { "1", "22", "333", "4444" };
        foreach (string s in Extensions.Slice(strings, 1, 2)) {
            Console.WriteLine(Extensions.ToInt32(s));
        }
    }
}
```

### <a name="method-body"></a>方法主體

*Method_body*方法的宣告包含區塊的主體、 運算式主體或分號。

***結果型別***是一種方法`void`的傳回型別是否`void`，或如果是非同步方法，而且傳回型別是`System.Threading.Tasks.Task`。 否則，非同步方法的結果型別是其傳回類型和非同步方法的結果型別，以傳回型別`System.Threading.Tasks.Task<T>`是`T`。

當方法具有`void`型別和區塊的主體中，會造成`return`陳述式 ([return 陳述式](statements.md#the-return-statement)) 區塊中不允許以指定的運算式。 如果正常完成之區塊的 void 方法的執行 （也就是控制流程離開方法主體的結尾），方法只會傳回其目前的呼叫端。
    
當方法具有`void`結果與運算式主體中，運算式`E`必須*statement_expression*，且主體為完全相當於表單的區塊主體`{ E; }`。
    
當方法具有非 void 結果型別和區塊的主體，每個`return`區塊中的陳述式必須指定為隱含地轉換成的結果類型的運算式。 值，傳回方法的區塊主體的端點不能連線。 換句話說，在區塊內文傳回值的方法，控制項不允許方法主體的結尾。
    
當方法具有非 void 結果型別和運算式主體時，運算式必須隱含地轉換成的結果型別，而主體就完全相當於表單的區塊主體`{ return E; }`。
    
在範例
```csharp
class A
{
    public int F() {}            // Error, return value required

    public int G() {
        return 1;
    }

    public int H(bool b) {
        if (b) {
            return 1;
        }
        else {
            return 0;
        }
    }

    public int I(bool b) => b ? 1 : 0;
}
```
傳回值的`F`方法會產生編譯時期錯誤，因為控制項可以離開方法主體的結尾。 `G`和`H`方法是否正確，因為所有可能的執行路徑結束於指定的傳回值的 return 陳述式。 `I`方法是否正確，因為其主體，相當於只是單一 return 陳述式中的陳述式區塊。

### <a name="method-overloading"></a>方法多載

方法多載解析規則所述[型別推斷](expressions.md#type-inference)。

## <a name="properties"></a>屬性

A***屬性***成員可存取物件或類別的特性。 屬性的範例包括字型、 標題的視窗中，一位客戶的名稱的大小字串的長度等等。 屬性是欄位的自然延伸，兩者都具有具名成員相關聯的型別，且存取欄位和屬性的語法相同。 不過，與欄位不同的是，屬性並不會指示儲存位置。 取而代之的是，屬性會有「存取子」，這些存取子會指定讀取或寫入其值時要執行的陳述式。 屬性會因此提供的機制，來將動作關聯的讀取和寫入物件的屬性;此外，它們允許這類屬性，來計算。

屬性使用宣告*property_declaration*s:

```antlr
property_declaration
    : attributes? property_modifier* type member_name property_body
    ;

property_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | property_modifier_unsafe
    ;

property_body
    : '{' accessor_declarations '}' property_initializer?
    | '=>' expression ';'
    ;

property_initializer
    : '=' variable_initializer ';'
    ;
```

A *property_declaration*可能包含一組*屬性*([屬性](attributes.md)) 和有效的四種存取修飾詞組合 ([存取修飾詞](classes.md#access-modifiers))，則`new`([的新修飾詞](classes.md#the-new-modifier))， `static` ([靜態和執行個體方法](classes.md#static-and-instance-methods))， `virtual` ([虛擬方法](classes.md#virtual-methods))， `override` ([覆寫方法](classes.md#override-methods))， `sealed` ([密封方法](classes.md#sealed-methods))， `abstract` ([抽象方法](classes.md#abstract-methods))，以及`extern` ([外部方法](classes.md#external-methods)) 修飾詞。

屬性宣告受限於相同的方法宣告規則 ([方法](classes.md#methods)) 方面的修飾詞的有效組合。

*型別*屬性的宣告會指定類型的宣告所引進的屬性和*member_name*指定屬性的名稱。 屬性是明確的介面成員實作，除非*member_name*是只要*識別項*。 明確介面成員實作 ([明確介面成員實作](interfaces.md#explicit-interface-member-implementations))，則*member_name*組成*interface_type*後面加上"`.`」 和 「*識別碼*。

*型別*必須至少像一樣地存取屬性本身的屬性 ([存取範圍條件約束](basic-concepts.md#accessibility-constraints))。

A *property_body*可能是組成***存取子主體***或是***運算式主體***。 在存取子主體中， *accessor_declarations*，這必須括在 「`{`"和"`}`"權杖宣告存取子 ([存取子](classes.md#accessors)) 的屬性。 存取子會指定可執行讀取和寫入的屬性相關聯的陳述式。

組成的運算式主體`=>`後面接著*運算式*`E`分號就完全相當於陳述式主體和`{ get { return E; } }`，因此只可用來指定僅限 getter 和其中的 getter 結果為單一運算式所指定的屬性。

A *property_initializer*只可能會提供給自動實作的屬性 ([自動實作屬性](classes.md#automatically-implemented-properties))，並造成這類的基礎欄位的初始化具有所指定值的屬性*運算式*。

即使存取屬性的語法是相同的欄位，屬性不會分類為變數。 因此，不可能將屬性當做`ref`或`out`引數。

當屬性宣告包含`extern`修飾詞，此屬性要***external 屬性***。 因為外部屬性宣告未不提供任何實際的實作，每個及其*accessor_declarations*只包含一個分號。

### <a name="static-and-instance-properties"></a>靜態和執行個體的屬性

當屬性宣告包含`static`修飾詞，此屬性要***靜態屬性***。 若未`static`修飾詞存在，則此屬性要***執行個體屬性***。

靜態屬性相關聯的特定執行個體，並不是編譯時期錯誤，以指向`this`在靜態屬性的存取子。

執行個體屬性與指定類別的執行個體相關聯，且該執行個體可存取為`this`([這項存取](expressions.md#this-access)) 中的該屬性存取子。

屬性中的參考時*member_access* ([成員存取](expressions.md#member-access)) 的形式`E.M`時，如果`M`是靜態屬性，`E`必須代表已包含類型`M`，而且如果`M`是一個執行個體的屬性，E 必須代表執行個體的型別，其中`M`。

靜態之間的差異和執行個體成員討論中進一步[靜態和執行個體成員](classes.md#static-and-instance-members)。

### <a name="accessors"></a>存取子

*Accessor_declarations*屬性指定的讀取和寫入該屬性相關聯的可執行陳述式。

```antlr
accessor_declarations
    : get_accessor_declaration set_accessor_declaration?
    | set_accessor_declaration get_accessor_declaration?
    ;

get_accessor_declaration
    : attributes? accessor_modifier? 'get' accessor_body
    ;

set_accessor_declaration
    : attributes? accessor_modifier? 'set' accessor_body
    ;

accessor_modifier
    : 'protected'
    | 'internal'
    | 'private'
    | 'protected' 'internal'
    | 'internal' 'protected'
    ;

accessor_body
    : block
    | ';'
    ;
```

存取子宣告組成*get_accessor_declaration*，則*set_accessor_declaration*，或兩者。 每個存取子宣告所組成的語彙基元`get`或`set`後面接著選擇性*accessor_modifier*並*accessor_body*。

善用*accessor_modifier*s 受到下列限制：

*  *Accessor_modifier*可能不適用於在介面或明確介面成員實作。
*  屬性或沒有任何索引子`override`修飾詞， *accessor_modifier*屬性或索引子同時具有時，才允許`get`和`set`存取子，然後才允許的其中之一，存取子。
*  屬性或包含的索引子`override`修飾詞，存取子必須符合*accessor_modifier*、 如果有的話，要覆寫的存取子。
*  *Accessor_modifier*必須宣告更嚴格限制比屬性或索引子本身的宣告存取範圍的存取範圍。 若要明確地說：
   * 如果屬性或索引子已宣告存取範圍`public`，則*accessor_modifier*可以是`protected internal`， `internal`， `protected`，或`private`。
   * 如果屬性或索引子已宣告存取範圍`protected internal`，則*accessor_modifier*可以是`internal`， `protected`，或`private`。
   * 如果屬性或索引子已宣告存取範圍`internal`或是`protected`，則*accessor_modifier*必須是`private`。
   * 如果屬性或索引子已宣告存取範圍`private`，沒有*accessor_modifier*可用。

針對`abstract`並`extern`屬性， *accessor_body*指定每個存取子是只是分號。 非抽象，非 extern 屬性可能會有每個*accessor_body*是分號，在此情況下很***自動實作屬性***([自動實作的屬性](classes.md#automatically-implemented-properties)). 自動實作的屬性必須有至少一個 get 存取子。 任何其他非抽象，非外部屬性，存取子*accessor_body*是*區塊*指定對應的存取子會叫用時要執行的陳述式。

A`get`存取子對應到無參數的方法具有傳回值的屬性型別。 在運算式中，參考屬性時，做為目標的指派，除非`get`屬性存取子會叫用來計算屬性的值 ([運算式的值](expressions.md#values-of-expressions))。 主體`get`存取子必須符合的規則，傳回值的方法中所述[方法主體](classes.md#method-body)。 特別是，所有`return`陳述式的主體中`get`存取子必須指定為隱含轉換為屬性類型的運算式。 此外，端點`get`存取子不能連線。

A`set`存取子會對應至具有單一值參數的屬性類型的方法和`void`傳回型別。 隱含參數`set`存取子的名稱永遠`value`。 當指派的目標為參考屬性 ([指派運算子](expressions.md#assignment-operators))，或作為運算元`++`或`--`([後置遞增和遞減運算子](expressions.md#postfix-increment-and-decrement-operators)， [前置遞增和遞減運算子](expressions.md#prefix-increment-and-decrement-operators))，則`set`的引數叫用存取子 (其值是指派的右邊的運算元`++`或`--`運算子)，提供新值 ([簡單指派](expressions.md#simple-assignment))。 主體`set`存取子必須符合的規則`void`方法中所述[方法主體](classes.md#method-body)。 特別是，`return`中的陳述式`set`存取子主體不允許指定的運算式。 由於`set`存取子會隱含地擁有名為的參數`value`，它是本機變數或常數宣告中的編譯時期錯誤`set`具有該名稱的存取子。

根據存在與否`get`和`set`存取子，屬性分類如下：

*  此屬性，同時包含`get`存取子和`set`存取子要***讀寫***屬性。
*  此屬性，只有`get`存取子要***唯讀***屬性。 它是唯讀的屬性，若要指派的目標的編譯時期錯誤。
*  此屬性，只有`set`存取子要***唯寫***屬性。 除了作為指派目標，它是參考運算式中的唯寫屬性的編譯時期錯誤。

在範例
```csharp
public class Button: Control
{
    private string caption;

    public string Caption {
        get {
            return caption;
        }
        set {
            if (caption != value) {
                caption = value;
                Repaint();
            }
        }
    }

    public override void Paint(Graphics g, Rectangle r) {
        // Painting code goes here
    }
}
```
`Button`控制宣告公用`Caption`屬性。 `get`存取子`Caption`屬性會傳回儲存在私用的字串`caption`欄位。 `set`存取子會檢查新的值是否不同於目前的值，如果是的話，它會儲存新的值，並重新繪製控制項。 屬性通常會遵循的模式，如上所示：`get`存取子只會傳回值，這個值儲存在私用欄位，而`set`存取子會修改該私用欄位，然後執行 完整更新物件的狀態所需的任何其他動作。

給定`Button`上述的類別，以下是範例使用`Caption`屬性：
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

在這裡，`set`存取子會將值指派給屬性，叫用和`get`存取子會叫用參考運算式中的屬性。

`get`和`set`屬性存取子不是不同的成員，而且不可以宣告子的屬性分開。 因此，不可能的讀 / 寫屬性的兩個存取子具有不同的存取範圍。 此範例
```csharp
class A
{
    private string name;

    public string Name {                // Error, duplicate member name
        get { return name; }
    }

    public string Name {                // Error, duplicate member name
        set { name = value; }
    }
}
```
未宣告的單一讀寫屬性。 相反地，它會宣告兩個具有相同名稱，其中一個唯讀的屬性，另一個唯寫。 因為在相同類別中宣告的兩個成員不能有相同的名稱，此範例會導致發生編譯時期錯誤。

當在衍生的類別繼承的屬性名稱相同的宣告屬性時，衍生的屬性會隱藏繼承的屬性，相對於讀取和寫入。 在範例
```csharp
class A
{
    public int P {
        set {...}
    }
}

class B: A
{
    new public int P {
        get {...}
    }
}
```
`P`中的屬性`B`隱藏`P`屬性中的`A`相對於讀取和寫入。 因此，在陳述式
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
指派給`b.P`會導致編譯時期錯誤回報，因為唯讀`P`中的屬性`B`隱藏唯`P`中的屬性`A`。 不過請注意，型別轉換，可用來存取的隱藏`P`屬性。

不同於公用欄位，屬性會提供物件的內部狀態和其公用介面之間的分隔。 此範例，請考慮：
```csharp
class Label
{
    private int x, y;
    private string caption;

    public Label(int x, int y, string caption) {
        this.x = x;
        this.y = y;
        this.caption = caption;
    }

    public int X {
        get { return x; }
    }

    public int Y {
        get { return y; }
    }

    public Point Location {
        get { return new Point(x, y); }
    }

    public string Caption {
        get { return caption; }
    }
}
```

在這裡，`Label`類別會使用兩個`int`欄位`x`和`y`來儲存它的位置。 位置是公開公開兩種形式`X`並`Y`屬性並做為`Location`型別的屬性`Point`。 如果在未來的版本`Label`，會變得更方便地儲存位置設定為`Point`就內部而言，進行變更，而不會影響該類別的公用介面：
```csharp
class Label
{
    private Point location;
    private string caption;

    public Label(int x, int y, string caption) {
        this.location = new Point(x, y);
        this.caption = caption;
    }

    public int X {
        get { return location.x; }
    }

    public int Y {
        get { return location.y; }
    }

    public Point Location {
        get { return location; }
    }

    public string Caption {
        get { return caption; }
    }
}
```

有`x`並`y`改為已`public readonly`欄位，就會進行這類變更不可能`Label`類別。

公開透過屬性的狀態不一定比直接公開欄位會比較有效率。 特別的是，當屬性為非虛擬的且包含只有少量的程式碼，執行環境可能會取代呼叫存取子的存取子的實際程式碼。 此程序稱為***內嵌***，以及它可以讓屬性存取的效率不如欄位存取權，但會保留屬性的較大彈性。

因為叫用`get`存取子是概念上相當於讀取欄位的值，則會視為不良程式設計樣式`get`存取子具有可預見的副作用。 在範例
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
值`Next`屬性相依於先前存取該屬性的次數。 因此，存取的屬性會產生副作用，而屬性應該改為實作做為方法。

「 沒有任何副作用 」 慣例`get`存取子不表示`get`一律應該寫入存取子只會傳回儲存在欄位的值。 事實上，`get`通常存取多個欄位，或叫用方法計算的屬性值的存取子。 不過，設計得當`get`存取子會執行任何動作都可觀察的變更會導致物件的狀態。

屬性可用來延遲初始設定的資源之前先參考它的時刻。 例如：
```csharp
using System.IO;

public class Console
{
    private static TextReader reader;
    private static TextWriter writer;
    private static TextWriter error;

    public static TextReader In {
        get {
            if (reader == null) {
                reader = new StreamReader(Console.OpenStandardInput());
            }
            return reader;
        }
    }

    public static TextWriter Out {
        get {
            if (writer == null) {
                writer = new StreamWriter(Console.OpenStandardOutput());
            }
            return writer;
        }
    }

    public static TextWriter Error {
        get {
            if (error == null) {
                error = new StreamWriter(Console.OpenStandardError());
            }
            return error;
        }
    }
}
```

`Console`類別包含三個屬性， `In`， `Out`，和`Error`，分別代表標準輸入、 輸出和錯誤的裝置。 藉由公開為屬性，這些成員`Console`類別可延遲其初始化，直到實際使用。 例如，在第一次參考時`Out`屬性，如下所示
```csharp
Console.Out.WriteLine("hello, world");
```
基礎`TextWriter`會建立輸出裝置。 但是，如果應用程式會參考在沒有`In`和`Error`屬性，則沒有任何物件會針對這些裝置。

### <a name="automatically-implemented-properties"></a>自動實作的屬性

自動實作的屬性 (或***auto 屬性***簡稱)，是以分號專用存取子主體的非抽象非外部屬性。 Auto 屬性必須有 get 存取子，而且可以選擇性地包含 set 存取子。

為自動實作屬性指定屬性時，隱藏的支援欄位會自動提供給屬性，並實作存取子來讀取和寫入至該支援欄位。 如果自動屬性沒有 set 存取子，支援欄位會被視為`readonly`([唯讀欄位](classes.md#readonly-fields))。 就像`readonly` 欄位中，僅限 getter 的 auto 屬性也可以指派至封入類別的建構函式主體中。 這類指派則會直接對唯讀支援欄位的屬性。

Auto 屬性可能會有*property_initializer*，這會直接套用至做為支援欄位*variable_initializer* ([區域變數初始設定式](classes.md#variable-initializers)).

下列範例：
```csharp
public class Point {
    public int X { get; set; } = 0;
    public int Y { get; set; } = 0;
}
```
相當於下列宣告：
```csharp
public class Point {
    private int __x = 0;
    private int __y = 0;
    public int X { get { return __x; } set { __x = value; } }
    public int Y { get { return __y; } set { __y = value; } }
}
```

下列範例：
```csharp
public class ReadOnlyPoint
{
    public int X { get; }
    public int Y { get; }
    public ReadOnlyPoint(int x, int y) { X = x; Y = y; }
}
```
相當於下列宣告：
```csharp
public class ReadOnlyPoint
{
    private readonly int __x;
    private readonly int __y;
    public int X { get { return __x; } }
    public int Y { get { return __y; } }
    public ReadOnlyPoint(int x, int y) { __x = x; __y = y; }
}
```

請注意，指派給唯讀欄位合法的因為它們發生在建構函式。


### <a name="accessibility"></a>協助工具選項

如果存取子已*accessor_modifier*，存取範圍定義域 ([存取範圍定義域](basic-concepts.md#accessibility-domains)) 的存取子用來決定的宣告存取範圍*accessor_modifier*. 如果存取子沒有*accessor_modifier*，存取子的存取範圍定義域由屬性或索引子的宣告存取範圍。

出現與否*accessor_modifier*永遠不會影響成員查閱 ([運算子](expressions.md#operators)) 或多載解析 ([多載解析](expressions.md#overload-resolution))。 在屬性或索引子的修飾詞，請務必判斷哪一個屬性或索引子繫結，不論存取的內容。

一旦選取特定的屬性或索引子，相關的特定存取子的存取範圍定義域用來判斷是否為有效的使用方式：

*  如果使用量做為值 ([運算式的值](expressions.md#values-of-expressions))，則`get`存取子必須存在且可存取。
*  如果使用量為目標的簡單指派 ([簡單指派](expressions.md#simple-assignment))，則`set`存取子必須存在且可存取。
*  其使用方式是否為目標的複合指派 ([複合指派](expressions.md#compound-assignment))，或做為目標的`++`或是`--`運算子 ([函式成員](expressions.md#function-members).9， [引動過程運算式](expressions.md#invocation-expressions))，這兩個`get`存取子和`set`存取子必須存在且可存取。

在下列範例中，屬性`A.Text`隱藏屬性所`B.Text`即使是在內容中在只有`set`呼叫存取子。 相較之下，屬性`B.Count`不是類別存取`M`，因此可存取的屬性`A.Count`改為使用。

```csharp
class A
{
    public string Text {
        get { return "hello"; }
        set { }
    }

    public int Count {
        get { return 5; }
        set { }
    }
}

class B: A
{
    private string text = "goodbye"; 
    private int count = 0;

    new public string Text {
        get { return text; }
        protected set { text = value; }
    }

    new protected int Count { 
        get { return count; }
        set { count = value; }
    }
}

class M
{
    static void Main() {
        B b = new B();
        b.Count = 12;             // Calls A.Count set accessor
        int i = b.Count;          // Calls A.Count get accessor
        b.Text = "howdy";         // Error, B.Text set accessor not accessible
        string s = b.Text;        // Calls B.Text get accessor
    }
}
```

用來實作介面的存取子不能*accessor_modifier*。 如果只有一個存取子用來實作介面，可能會以宣告其他存取子*accessor_modifier*:
```csharp
public interface I
{
    string Prop { get; }
}

public class C: I
{
    public Prop {
        get { return "April"; }       // Must not have a modifier here
        internal set {...}            // Ok, because I.Prop has no set accessor
    }
}
```

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a>虛擬、 密封、 覆寫及抽象屬性存取子

A`virtual`屬性宣告可讓您指定的屬性存取子是虛擬。 `virtual`修飾詞套用至兩個讀寫屬性存取子，不可能的讀 / 寫屬性只能有一個存取子為虛擬。

`abstract`屬性宣告指定的屬性存取子是虛擬的但不是提供存取子的實際實作。 相反地，非抽象衍生的類別才能覆寫屬性提供自己的存取子的實作。 因為抽象屬性宣告的存取子不提供任何實際的實作，其*accessor_body*僅包含一個分號。

同時包含屬性宣告`abstract`和`override`修飾詞指定的屬性是抽象的並會覆寫基底屬性。 這類屬性的存取子也是抽象的。

抽象屬性宣告只允許在抽象類別 ([抽象類別](classes.md#abstract-classes))。繼承的虛擬屬性存取子可以覆寫衍生類別中包含指定的屬性宣告`override`指示詞。 這就所謂***覆寫屬性宣告***。 覆寫的屬性宣告並不會宣告新的屬性。 相反地，它只會指定現有的虛擬屬性存取子的實作。

覆寫的屬性宣告必須指定和繼承的屬性完全相同的存取範圍修飾詞、 類型和名稱。 如果繼承的屬性只有單一存取子 （亦即，如果繼承的屬性是唯讀或唯寫），覆寫的屬性必須包含該存取子。 如果繼承的屬性會包含兩個存取子 （亦即，如果繼承的屬性是讀寫），覆寫屬性可以包含單一存取子，或是這兩個存取子。

覆寫的屬性宣告可能包含`sealed`修飾詞。 使用此修飾詞可防止在衍生的類別進一步覆寫屬性。 密封屬性存取子也被密封的。

除了宣告和引動過程之間的差異語法、 虛擬、 sealed、 override、 和抽象方法和行為完全相同虛擬、 密封、 覆寫和抽象方法。 規則中所述的具體來說，[虛擬方法](classes.md#virtual-methods)，[覆寫方法](classes.md#override-methods)，[密封方法](classes.md#sealed-methods)，以及[抽象方法](classes.md#abstract-methods)套用如同存取子所對應之表單的方法：

*  A`get`存取子對應的無參數方法具有傳回值的屬性類型和相同的修飾詞和包含的屬性。
*  A`set`存取子對應至具有單一值參數的屬性型別方法`void`傳回型別和相同的修飾詞和包含的屬性。

在範例
```csharp
abstract class A
{
    int y;

    public virtual int X {
        get { return 0; }
    }

    public virtual int Y {
        get { return y; }
        set { y = value; }
    }

    public abstract int Z { get; set; }
}
```
`X` 是虛擬的唯讀屬性，`Y`是虛擬的讀寫屬性，以及`Z`是抽象的讀 / 寫屬性。 因為`Z`是抽象的包含類別`A`也必須宣告為抽象。

類別衍生自`A`會如下所示：
```csharp
class B: A
{
    int z;

    public override int X {
        get { return base.X + 1; }
    }

    public override int Y {
        set { base.Y = value < 0? 0: value; }
    }

    public override int Z {
        get { return z; }
        set { z = value; }
    }
}
```

這裡的宣告`X`， `Y`，和`Z`會覆寫屬性宣告。 如果每個屬性宣告的存取範圍修飾詞、 類型和對應的繼承屬性的名稱完全符合。 `get`存取子`X`並`set`存取子`Y`使用`base`存取繼承的存取子關鍵字。 宣告`Z`覆寫兩個抽象存取子，因此，會在沒有未處理的抽象函式成員`B`，和`B`允許非抽象類別。

當屬性宣告為`override`，覆寫的任何存取子必須覆寫的程式碼存取。 此外，宣告的屬性或索引子本身的子範圍，必須符合覆寫的成員和存取子。 例如: 
```csharp
public class B
{
    public virtual int P {
        protected set {...}
        get {...}
    }
}

public class D: B
{
    public override int P {
        protected set {...}            // Must specify protected here
        get {...}                      // Must not have a modifier here
    }
}
```

## <a name="events"></a>事件

***事件***是可讓物件或類別，以提供通知的成員。 用戶端可附加事件的可執行程式碼，藉由提供***事件處理常式***。

事件使用宣告*event_declaration*s:

```antlr
event_declaration
    : attributes? event_modifier* 'event' type variable_declarators ';'
    | attributes? event_modifier* 'event' type member_name '{' event_accessor_declarations '}'
    ;

event_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | event_modifier_unsafe
    ;

event_accessor_declarations
    : add_accessor_declaration remove_accessor_declaration
    | remove_accessor_declaration add_accessor_declaration
    ;

add_accessor_declaration
    : attributes? 'add' block
    ;

remove_accessor_declaration
    : attributes? 'remove' block
    ;
```

*Event_declaration*可能包含一組*屬性*([屬性](attributes.md)) 和有效的四種存取修飾詞組合 ([存取修飾詞](classes.md#access-modifiers))，則`new`([的新修飾詞](classes.md#the-new-modifier))， `static` ([靜態和執行個體方法](classes.md#static-and-instance-methods))， `virtual` ([虛擬方法](classes.md#virtual-methods))， `override` ([覆寫方法](classes.md#override-methods))， `sealed` ([密封方法](classes.md#sealed-methods))， `abstract` ([抽象方法](classes.md#abstract-methods))，以及`extern` ([外部方法](classes.md#external-methods)) 修飾詞。

事件宣告受限於相同的方法宣告規則 ([方法](classes.md#methods)) 方面的修飾詞的有效組合。

*型別*事件的宣告必須是*delegate_type* ([參考型別](types.md#reference-types))，且*delegate_type*必須至少為為事件本身的存取 ([協助工具的條件約束](basic-concepts.md#accessibility-constraints))。

事件宣告可能包含*event_accessor_declarations*。 不過，如果沒有，非外部連結的非抽象事件，則編譯器會提供這些自動 ([欄位型事件](classes.md#field-like-events)); 外部事件存取子會從外部提供。

事件宣告省略*event_accessor_declarations*定義一或多個事件，各供一個*variable_declarator*s。 屬性和修飾詞套用至所有這類宣告的成員*event_declaration*。

它是編譯時期錯誤*event_declaration*同時包含`abstract`修飾詞和大括號分隔*event_accessor_declarations*。

當事件宣告包含`extern`修飾詞，此事件要***外部事件***。 因為外部事件宣告未不提供任何實際的實作，是它同時包含錯誤`extern`修飾詞和*event_accessor_declarations*。

它是編譯時期錯誤*variable_declarator*的事件宣告`abstract`或`external`修飾詞，以包含*variable_initializer*。

事件可用來當做左運算元`+=`並`-=`運算子 ([事件指派](expressions.md#event-assignment))。 這些運算子使用時，分別附加事件處理常式，或移除事件處理常式事件，以及事件的存取修飾詞控制允許使用這類作業的內容。

由於`+=`和`-=`是唯一允許外部宣告的事件、 外部程式碼的型別事件的作業可以新增和移除事件處理常式，但不能以任何其他方式取得或修改事件的基礎清單處理常式。

在作業中的表單`x += y`或`x -= y`，當`x`是事件，並且包含宣告的型別之外的參考發生`x`，作業的結果的類型`void`（而不需要型別`x`，其值為`x`指派之後)。 此規則可禁止外部程式碼間接檢查事件的基礎委派。

下列範例示範如何將事件處理常式附加到的執行個體`Button`類別：
```csharp
public delegate void EventHandler(object sender, EventArgs e);

public class Button: Control
{
    public event EventHandler Click;
}

public class LoginDialog: Form
{
    Button OkButton;
    Button CancelButton;

    public LoginDialog() {
        OkButton = new Button(...);
        OkButton.Click += new EventHandler(OkButtonClick);
        CancelButton = new Button(...);
        CancelButton.Click += new EventHandler(CancelButtonClick);
    }

    void OkButtonClick(object sender, EventArgs e) {
        // Handle OkButton.Click event
    }

    void CancelButtonClick(object sender, EventArgs e) {
        // Handle CancelButton.Click event
    }
}
```

在這裡，`LoginDialog`執行個體建構函式會建立兩個`Button`執行個體，並將附加事件處理常式`Click`事件。

### <a name="field-like-events"></a>欄位型事件

在類別或結構，其中包含事件的宣告的程式文字，某些事件可用欄位與欄位類似。 若要使用這種方式，事件不能`abstract`或`extern`，而且必須明確包含*event_accessor_declarations*。 這種情況下可以用於任何允許欄位的內容。 欄位包含的委派 ([委派](delegates.md)) 就是指已加入至事件的事件處理常式的清單。 如果尚未新增任何事件處理常式，欄位會包含`null`。

在範例
```csharp
public delegate void EventHandler(object sender, EventArgs e);

public class Button: Control
{
    public event EventHandler Click;

    protected void OnClick(EventArgs e) {
        if (Click != null) Click(this, e);
    }

    public void Reset() {
        Click = null;
    }
}
```
`Click` 使用中的欄位為`Button`類別。 如範例所示，該欄位可以檢查、 修改，並委派引動過程運算式中使用。 `OnClick`方法中的`Button`類別 「 引發 」`Click`事件。 引發事件的概念完全等同於叫用該事件所代表的委派項目，因此，就引發事件而言，並沒有任何特殊的語言建構。 請注意，委派引動過程會加上核取，以確保委派為非 null。

宣告之外`Button`類別，`Click`成員僅適用於在左手邊`+=`和`-=`運算子，如下所示
```csharp
b.Click += new EventHandler(...);
```
將附加委派的引動過程清單`Click`事件，以及
```csharp
b.Click -= new EventHandler(...);
```
以從引動過程清單移除委派`Click`事件。

在編譯時的欄位型事件，編譯器會自動建立儲存空間來保存此委派，，並建立事件，新增或移除事件處理常式委派欄位，存取子。 新增和移除作業執行緒安全，並可能 （但並不需要） 時完成持有鎖定 ([lock 陳述式](statements.md#the-lock-statement)) 執行個體事件，包含的物件或型別物件 ([匿名物件建立運算式](expressions.md#anonymous-object-creation-expressions)) 靜態事件。

因此，執行個體事件宣告的形式：
```csharp
class X
{
    public event D Ev;
}
```
將會編譯為相等的項目：
```csharp
class X
{
    private D __Ev;  // field to hold the delegate

    public event D Ev {
        add {
            /* add the delegate in a thread safe way */
        }

        remove {
            /* remove the delegate in a thread safe way */
        }
    }
}
```
在類別內`X`，參考`Ev`的左手邊`+=`和`-=`運算子造成新增和移除要叫用的存取子。 所有其他參考`Ev`編譯為參考的隱藏的欄位`__Ev`改為 ([成員存取](expressions.md#member-access))。 名稱"`__Ev`"是任意; 隱藏的欄位可能會有任何名稱不完全。

### <a name="event-accessors"></a>事件存取子

事件宣告通常會忽略*event_accessor_declarations*，如下所示`Button`上述範例。 一種情況下，執行此步驟涉及在其中一個欄位，每個事件的儲存體成本不是可接受的情況。 在此情況下，類別可以包含*event_accessor_declarations* ，並使用私用機制來儲存事件處理常式的清單。

*Event_accessor_declarations*事件的指定可執行的陳述式加入和移除事件處理常式相關聯。

存取子宣告組成*add_accessor_declaration*並*remove_accessor_declaration*。 每個存取子宣告所組成的語彙基元`add`或是`remove`後面*區塊*。 *區塊*聯*add_accessor_declaration*指定的陳述式執行時加入事件處理常式，而*區塊*相關聯*remove_accessor_declaration*指定移除事件處理常式時要執行的陳述式。

每個*add_accessor_declaration*並*remove_accessor_declaration*對應至具有事件類型的單一值參數的方法和`void`傳回型別。 事件存取子的隱含參數之所以名為`value`。 當事件指派使用事件時，會使用適當的事件存取子。 具體來說，如果指派運算子`+=`則會使用的 add 存取子，，和指派運算子是`-=`則會使用 remove 存取子。 在任一情況下，指派運算子的右運算元做為事件存取子的引數中。 區塊*add_accessor_declaration*或是*remove_accessor_declaration*必須符合的規則`void`中所述的方法[方法主體](classes.md#method-body)。 特別是，`return`這類區塊中的陳述式不允許指定的運算式。

因為事件存取子會隱含地擁有名為的參數`value`，本機變數或常數宣告中的事件存取子，具有這個名稱是編譯時期錯誤。

在範例
```csharp
class Control: Component
{
    // Unique keys for events
    static readonly object mouseDownEventKey = new object();
    static readonly object mouseUpEventKey = new object();

    // Return event handler associated with key
    protected Delegate GetEventHandler(object key) {...}

    // Add event handler associated with key
    protected void AddEventHandler(object key, Delegate handler) {...}

    // Remove event handler associated with key
    protected void RemoveEventHandler(object key, Delegate handler) {...}

    // MouseDown event
    public event MouseEventHandler MouseDown {
        add { AddEventHandler(mouseDownEventKey, value); }
        remove { RemoveEventHandler(mouseDownEventKey, value); }
    }

    // MouseUp event
    public event MouseEventHandler MouseUp {
        add { AddEventHandler(mouseUpEventKey, value); }
        remove { RemoveEventHandler(mouseUpEventKey, value); }
    }

    // Invoke the MouseUp event
    protected void OnMouseUp(MouseEventArgs args) {
        MouseEventHandler handler; 
        handler = (MouseEventHandler)GetEventHandler(mouseUpEventKey);
        if (handler != null)
            handler(this, args);
    }
}
```
`Control`類別會實作事件的內部儲存機制。 `AddEventHandler`方法會將委派值關聯的索引鍵`GetEventHandler`方法會傳回目前的索引鍵相關聯的委派和`RemoveEventHandler`方法會移除與指定的事件的事件處理常式的委派。 我們可以假設，基礎儲存機制的設計可讓沒有任何相關聯的成本`null`委派以索引鍵的值，並因此未處理的事件取用任何儲存體。

### <a name="static-and-instance-events"></a>靜態和執行個體的事件

當事件宣告包含`static`修飾詞，此事件要***靜態事件***。 若未`static`修飾詞存在，則此事件要***執行個體事件***。

靜態事件相關聯的特定執行個體，並不是編譯時期錯誤，以指向`this`中的靜態事件存取子。

執行個體事件與指定類別的執行個體相關聯，而且可以當做這個執行個體`this`([這項存取](expressions.md#this-access)) 中的該事件存取子。

事件中的參考時*member_access* ([成員存取](expressions.md#member-access)) 的形式`E.M`時，如果`M`是靜態事件，`E`必須代表已包含類型`M`，而且如果`M`是執行個體的事件，E 必須代表執行個體的型別，其中`M`。

靜態之間的差異和執行個體成員討論中進一步[靜態和執行個體成員](classes.md#static-and-instance-members)。

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a>虛擬、 密封、 覆寫及抽象事件存取子

A`virtual`事件宣告可讓您指定的事件存取子是虛擬。 `virtual`修飾詞套用至兩個事件存取子。

`abstract`事件宣告指定的事件存取子是虛擬的但不是提供存取子的實際實作。 相反地，非抽象衍生的類別必須提供自己的實作存取子的覆寫事件。 抽象事件宣告未提供實際實作，因為它無法提供大括號分隔*event_accessor_declarations*。

同時包含事件宣告`abstract`和`override`修飾詞指定的事件是抽象的而且會覆寫基底的事件。 這類事件存取子也是抽象的。

抽象事件宣告中只允許出現在抽象類別 ([抽象類別](classes.md#abstract-classes))。

繼承的虛擬事件的存取子可以覆寫衍生類別中包含指定的事件宣告`override`修飾詞。 這就所謂***覆寫事件宣告***。 覆寫的事件宣告並不會宣告新的事件。 相反地，它只會指定現有的虛擬活動的存取子的實作。

覆寫的事件宣告必須指定完全相同的存取範圍修飾詞、 類型和名稱為覆寫的事件。

覆寫的事件宣告可能包含`sealed`修飾詞。 使用此修飾詞可防止在衍生的類別進一步覆寫事件。 密封的事件存取子也被密封的。

它會覆寫事件宣告中包含的編譯時期錯誤`new`修飾詞。

除了宣告和引動過程之間的差異語法、 虛擬、 sealed、 override、 和抽象方法和行為完全相同虛擬、 密封、 覆寫和抽象方法。 規則中所述的具體來說，[虛擬方法](classes.md#virtual-methods)，[覆寫方法](classes.md#override-methods)，[密封方法](classes.md#sealed-methods)，以及[抽象方法](classes.md#abstract-methods)套用如同存取子所對應之表單的方法。 每個存取子對應至具有單一值參數的事件類型，方法`void`傳回型別，並與包含事件的相同修飾詞。

## <a name="indexers"></a>索引子

***Indexer***是讓物件可以做為陣列相同的方式編製索引的成員。 使用宣告索引子*indexer_declaration*s:

```antlr
indexer_declaration
    : attributes? indexer_modifier* indexer_declarator indexer_body
    ;

indexer_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | indexer_modifier_unsafe
    ;

indexer_declarator
    : type 'this' '[' formal_parameter_list ']'
    | type interface_type '.' 'this' '[' formal_parameter_list ']'
    ;

indexer_body
    : '{' accessor_declarations '}' 
    | '=>' expression ';'
    ;
```

*Indexer_declaration*可能包含一組*屬性*([屬性](attributes.md)) 和有效的四種存取修飾詞組合 ([存取修飾詞](classes.md#access-modifiers))，則`new`([的新修飾詞](classes.md#the-new-modifier))， `virtual` ([虛擬方法](classes.md#virtual-methods))， `override` ([覆寫方法](classes.md#override-methods))， `sealed` ([密封方法](classes.md#sealed-methods))， `abstract` ([抽象方法](classes.md#abstract-methods))，以及`extern`([外部方法](classes.md#external-methods)) 修飾詞。

索引子宣告受限於相同的方法宣告規則 ([方法](classes.md#methods)) 方面的修飾詞的有效組合，有一個例外狀況，在於 static 修飾詞不允許在索引子宣告。

修飾詞`virtual`， `override`，和`abstract`互斥除了在其中一個案例。 `abstract`和`override`以便抽象索引子可以覆寫虛擬介面修飾詞也可能會一起使用。

*型別*索引子的宣告會指定索引子，宣告所引進的項目類型。 索引子是明確介面成員實作，除非*型別*後面跟著關鍵字`this`。 明確介面成員實作，*型別*後面*interface_type*、 「`.`"，和關鍵字`this`。 不同於其他成員中，索引子不會有使用者定義的名稱。

*Formal_parameter_list*指定的索引子的參數。 索引子的型式參數清單對應至方法的 ([方法的參數](classes.md#method-parameters))，但必須指定至少一個參數，且`ref`和`out`不允許參數修飾詞.

*型別*的索引子和每個參考中的型別*formal_parameter_list*必須是至少一樣地存取索引子本身 ([存取範圍條件約束](basic-concepts.md#accessibility-constraints)).

*Indexer_body*可能是組成***存取子主體***或是***運算式主體***。 在存取子主體中， *accessor_declarations*，這必須括在 「`{`"和"`}`"權杖宣告存取子 ([存取子](classes.md#accessors)) 的屬性。 存取子會指定可執行讀取和寫入的屬性相關聯的陳述式。

組成的運算式主體 」`=>`"接著一運算式`E`分號就完全相當於陳述式主體和`{ get { return E; } }`，並因此只可用來指定僅限 getter 的索引子的 getter 結果的所在根據單一運算式。

即使存取索引子元素的語法是相同的陣列項目，索引子元素不會分類為變數。 因此，不可能傳遞的索引子元素當做`ref`或`out`引數。

索引子的型式參數清單會定義簽章 ([簽章和多載](basic-concepts.md#signatures-and-overloading)) 的索引子。 具體而言，索引子的簽章所組成的數目和其型式參數的類型。 項目型別和型式參數名稱不是索引子的簽章的一部分。

索引子的簽章必須與在相同類別中宣告的其他所有索引子的簽章不同。

索引子和屬性在概念上，非常類似，但在下列方面也不同：

*  屬性會識別的名稱，而索引子由其簽章。
*  屬性透過存取*simple_name* ([簡單名稱](expressions.md#simple-names)) 或*member_access* ([成員存取](expressions.md#member-access))，而索引子項目經由*element_access* ([索引子存取](expressions.md#indexer-access))。
*  屬性可以是`static`成員，而索引子是一律執行個體成員。
*  A`get`存取子的屬性會對應至使用任何參數，方法，而`get`索引子的存取子會對應至具有相同的型式參數清單與索引子的方法。
*  A`set`屬性存取子對應至方法，與具有單一參數具名`value`，而`set`索引子的存取子會對應至具有相同索引子，再加上額外的參數的型式參數清單的方法名為`value`。
*  它是索引子存取子宣告本機變數具有相同名稱做為索引子參數的編譯時期錯誤。
*  在 覆寫的屬性宣告，繼承的屬性可使用語法`base.P`，其中`P`是屬性名稱。 在覆寫索引子宣告中，繼承的索引子來存取使用語法`base[E]`，其中`E`是運算式的逗號分隔清單。
*  沒有 「 自動實作索引子 」 的概念。 它是有非抽象，非外部的索引子，以分號存取子的錯誤。

除了這些差異，定義中的所有規則[存取子](classes.md#accessors)並[自動實作屬性](classes.md#automatically-implemented-properties)套用索引子存取子以及屬性存取子。

當包含索引子宣告`extern`修飾詞，即為索引子***外部的索引子***。 因為外部索引子宣告未不提供任何實際的實作，每個及其*accessor_declarations*只包含一個分號。

下列範例會宣告`BitArray`實作索引子來存取個別位元的位元陣列中的類別。
```csharp
using System;

class BitArray
{
    int[] bits;
    int length;

    public BitArray(int length) {
        if (length < 0) throw new ArgumentException();
        bits = new int[((length - 1) >> 5) + 1];
        this.length = length;
    }

    public int Length {
        get { return length; }
    }

    public bool this[int index] {
        get {
            if (index < 0 || index >= length) {
                throw new IndexOutOfRangeException();
            }
            return (bits[index >> 5] & 1 << index) != 0;
        }
        set {
            if (index < 0 || index >= length) {
                throw new IndexOutOfRangeException();
            }
            if (value) {
                bits[index >> 5] |= 1 << index;
            }
            else {
                bits[index >> 5] &= ~(1 << index);
            }
        }
    }
}
```

執行個體`BitArray`類別會取用頻寬極小的記憶體比對應`bool[]`（因為前者的每個值佔而不是只包含一個位元，後者的一個位元組），但它允許在相同的作業為`bool[]`。

下列`CountPrimes`類別會使用`BitArray`和傳統 「 sieve 」 演算法來計算介於 1 到指定的最大的質數的數目：
```csharp
class CountPrimes
{
    static int Count(int max) {
        BitArray flags = new BitArray(max + 1);
        int count = 1;
        for (int i = 2; i <= max; i++) {
            if (!flags[i]) {
                for (int j = i * 2; j <= max; j += i) flags[j] = true;
                count++;
            }
        }
        return count;
    }

    static void Main(string[] args) {
        int max = int.Parse(args[0]);
        int count = Count(max);
        Console.WriteLine("Found {0} primes between 1 and {1}", count, max);
    }
}
```

請注意，來存取的項目語法`BitArray`正是相同`bool[]`。

下列範例顯示具有兩個參數的索引子的 26 * 10 方格類別。 第一個參數必須是大寫或小寫字母 A 到 Z、 範圍內，第二個必須是範圍 0-9 中的整數。

```csharp
using System;

class Grid
{
    const int NumRows = 26;
    const int NumCols = 10;

    int[,] cells = new int[NumRows, NumCols];

    public int this[char c, int col] {
        get {
            c = Char.ToUpper(c);
            if (c < 'A' || c > 'Z') {
                throw new ArgumentException();
            }
            if (col < 0 || col >= NumCols) {
                throw new IndexOutOfRangeException();
            }
            return cells[c - 'A', col];
        }

        set {
            c = Char.ToUpper(c);
            if (c < 'A' || c > 'Z') {
                throw new ArgumentException();
            }
            if (col < 0 || col >= NumCols) {
                throw new IndexOutOfRangeException();
            }
            cells[c - 'A', col] = value;
        }
    }
}
```

### <a name="indexer-overloading"></a>索引子多載

索引子多載解析規則所述[型別推斷](expressions.md#type-inference)。

## <a name="operators"></a>運算子

***運算子***定義可套用至類別的執行個體的運算式運算子的意義的成員。 使用宣告運算子*operator_declaration*s:

```antlr
operator_declaration
    : attributes? operator_modifier+ operator_declarator operator_body
    ;

operator_modifier
    : 'public'
    | 'static'
    | 'extern'
    | operator_modifier_unsafe
    ;

operator_declarator
    : unary_operator_declarator
    | binary_operator_declarator
    | conversion_operator_declarator
    ;

unary_operator_declarator
    : type 'operator' overloadable_unary_operator '(' type identifier ')'
    ;

overloadable_unary_operator
    : '+' | '-' | '!' | '~' | '++' | '--' | 'true' | 'false'
    ;

binary_operator_declarator
    : type 'operator' overloadable_binary_operator '(' type identifier ',' type identifier ')'
    ;

overloadable_binary_operator
    : '+'   | '-'   | '*'   | '/'   | '%'   | '&'   | '|'   | '^'   | '<<'
    | right_shift | '=='  | '!='  | '>'   | '<'   | '>='  | '<='
    ;

conversion_operator_declarator
    : 'implicit' 'operator' type '(' type identifier ')'
    | 'explicit' 'operator' type '(' type identifier ')'
    ;

operator_body
    : block
    | '=>' expression ';'
    | ';'
    ;
```

有三種類別的多載運算子：一元運算子 ([一元運算子](classes.md#unary-operators))，二元運算子 ([二元運算子](classes.md#binary-operators))，並轉換運算子 ([轉換運算子](classes.md#conversion-operators))。

*Operator_body*是其中一個分號***陳述式主體***或是***運算式主體***。 陳述式主體所組成*區塊*，指定要執行時叫用運算子的陳述式。 *區塊*必須符合的傳回值的規則中所述的方法[方法主體](classes.md#method-body)。 運算式主體包含`=>`後面的運算式和分號，並代表單一運算式時，運算子會叫用執行。

針對`extern`運算子*operator_body*只包含一個分號。 所有其他運算子，如*operator_body*運算式主體或區塊的主體。

下列規則適用於所有運算子的宣告：

*  運算子宣告必須同時包含`public`和`static`修飾詞。
*  運算子的參數必須是實值參數 ([實值參數](variables.md#value-parameters))。 它是運算子宣告，以指定的編譯時期錯誤`ref`或`out`參數。
*  運算子的簽章 ([一元運算子](classes.md#unary-operators)，[二元運算子](classes.md#binary-operators)，[轉換運算子](classes.md#conversion-operators)) 必須不同於其他宣告中的所有運算子的簽章相同的類別。
*  運算子宣告中所參考的所有類型都必須至少像運算子本身那樣 ([協助工具的條件約束](basic-concepts.md#accessibility-constraints))。
*  它是相同的修飾詞在運算子宣告中出現多次錯誤。

每個操作員類別目錄都會加入額外的限制，如同下列各節所述。

如同其他成員，在衍生類別會繼承基底類別中宣告運算子。 運算子宣告一律需要類別或結構宣告運算子的簽章中加入該運算子，因為它不可能在衍生類別中宣告的運算子，隱藏在基底類別中宣告的運算子。 因此，`new`永遠不會有需要，並因此也不允許出現在運算子宣告修飾詞。

一元和二元運算子的其他資訊可在[運算子](expressions.md#operators)。

轉換運算子的其他資訊可在[使用者定義轉換](conversions.md#user-defined-conversions)。

### <a name="unary-operators"></a>一元運算子

下列規則適用於一元運算子的宣告，其中`T`表示類別或結構，其中包含運算子宣告的執行個體類型：

*  一元`+`， `-`， `!`，或`~`運算子必須採用單一參數的型別`T`或`T?`並可傳回任何類型。
*  一元`++`或是`--`運算子必須採用單一參數型別的`T`或`T?`而且必須傳回相同的型別或型別衍生自它。
*  一元`true`或是`false`運算子必須採用單一參數型別的`T`或`T?`而且必須傳回型別`bool`。

一元運算子的簽章所組成的運算子語彙基元 (`+`， `-`， `!`， `~`， `++`， `--`， `true`，或`false`) 和單一的型式參數的型別。 傳回的型別不是一元運算子的簽章的一部分，也是型式參數的名稱。

`true`和`false`一元運算子需要成對的宣告。 如果類別是下列運算子之一不需要也宣告其他宣告，就會發生編譯時期錯誤。 `true`並`false`運算子會中將進一步說明[使用者定義的條件式邏輯運算子](expressions.md#user-defined-conditional-logical-operators)並[布林運算式](expressions.md#boolean-expressions)。

下列範例顯示實作和後續使用量的`operator ++`整數向量類別：
```csharp
public class IntVector
{
    public IntVector(int length) {...}

    public int Length {...}                 // read-only property

    public int this[int index] {...}        // read-write indexer

    public static IntVector operator ++(IntVector iv) {
        IntVector temp = new IntVector(iv.Length);
        for (int i = 0; i < iv.Length; i++)
            temp[i] = iv[i] + 1;
        return temp;
    }
}

class Test
{
    static void Main() {
        IntVector iv1 = new IntVector(4);    // vector of 4 x 0
        IntVector iv2;

        iv2 = iv1++;    // iv2 contains 4 x 0, iv1 contains 4 x 1
        iv2 = ++iv1;    // iv2 contains 4 x 2, iv1 contains 4 x 2
    }
}
```

請注意如何運算子方法會傳回值所產生的加 1 的運算元，如同後置遞增和遞減運算子 ([後置遞增和遞減運算子](expressions.md#postfix-increment-and-decrement-operators))，以及前置遞增和遞減運算子 ([前置遞增和遞減運算子](expressions.md#prefix-increment-and-decrement-operators))。 不同於在C++，這個方法需要直接修改其運算元的值。 事實上，修改的運算元值違反標準後置遞增運算子的語意。

### <a name="binary-operators"></a>二元運算子

下列規則適用於二元運算子的宣告，其中`T`表示類別或結構，其中包含運算子宣告的執行個體類型：

*  二進位非移位運算子必須採用兩個參數，至少其中之一必須具有型別`T`或`T?`，並可傳回任何類型。
*  二進位`<<`或`>>`運算子必須採用兩個參數，其中第一個型別必須`T`或`T?`，其中第二個型別必須`int`或`int?`，並可傳回任何類型。

二元運算子的簽章所組成的運算子語彙基元 (`+`， `-`， `*`， `/`， `%`， `&`， `|`， `^`， `<<`， `>>`，`==`， `!=`， `>`， `<`， `>=`，或`<=`) 和兩個型式參數的類型。 傳回的型別和型式參數的名稱不是二元運算子的簽章的一部分。

某些二元運算子需要成對的宣告。 運算子對其中之一的每個宣告，必須有相符配對的其他運算子的宣告。 它們有相同的傳回類型與每個參數類型時，就會符合兩個運算子的宣告。 下列運算子需要成對的宣告：

*  `operator ==` 和 `operator !=`
*  `operator >` 和 `operator <`
*  `operator >=` 和 `operator <=`

### <a name="conversion-operators"></a>轉換運算子

轉換運算子宣告會引入***使用者定義的轉換***([使用者定義轉換](conversions.md#user-defined-conversions)) 的增強了預先定義的隱含和明確轉換。

包含的轉換運算子宣告`implicit`關鍵字導入了使用者定義的隱含轉換。 在許多情況下，包括函式成員引動過程、 轉型運算式，以及指派可能是隱含轉換。 這是更進一步的說明[隱含轉換](conversions.md#implicit-conversions)。

包含的轉換運算子宣告`explicit`關鍵字導入了使用者定義的明確轉換。 明確轉換可能會發生在轉型運算式，並會進一步說明，在[明確轉換](conversions.md#explicit-conversions)。

轉換運算子，將轉換成目標型別，以轉換運算子的傳回型別轉換運算子的參數類型所指定來源型別。

指定的來源型別的`S`和目標型別`T`，如果`S`或`T`是可為 null 的型別，讓`S0`並`T0`指其基礎類型，否則為`S0`和`T0`是相等`S`和`T`分別。 類別或結構，並允許宣告從來源類型轉換`S`成目標型別`T`只有當下列所有項目為真：

*  `S0` 和`T0`是不同的型別。
*  請`S0`或`T0`是運算子宣告進行的類別或結構類型。
*  既不`S0`也`T0`是*interface_type*。
*  排除使用者定義的轉換，轉換沒有從`S`來`T`或來自`T`至`S`。

基於這些規則的目的，任何型別參數相關聯`S`或`T`會被視為具有其他類型，任何繼承關聯性和參數會忽略這些型別上的任何條件約束的唯一型別。

在範例
```csharp
class C<T> {...}

class D<T>: C<T>
{
    public static implicit operator C<int>(D<T> value) {...}      // Ok
    public static implicit operator C<string>(D<T> value) {...}   // Ok
    public static implicit operator C<T>(D<T> value) {...}        // Error
}
```
第一次的兩個運算子宣告都受到允許，因為目的[索引子](classes.md#indexers).3`T`並`int`和`string`分別會被視為唯一的型別沒有關聯性。 不過，第三個運算子是錯誤，因為`C<T>`的基底類別的`D<T>`。

從第二個規則，轉換運算子必須轉換至或自宣告該運算子的類別或結構類型。 比方說，就可以對類別或結構類型`C`來定義轉換`C`來`int`進出`int`來`C`，而不是從`int`至`bool`。

您不可能直接重新定義預先定義的轉換。 因此，轉換運算子不會允許將轉換至或從中`object`存在於之間的隱含和明確轉換因為`object`和所有其他型別。 同樣地，來源和轉換的目標型別都不可以是基底類型的另一個，因為轉換之前就已經存在。

不過，就可以宣告泛型型別上，對於特定類型引數，指定已存在的轉換為預先定義的轉換的運算子。 在範例
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
類型時`object`指定為型別引數`T`，第二個運算子會將宣告轉換已經存在 (隱含的因此也可以是明確轉換存在從任何型別型別`object`)。

在兩個類型之間的預先定義的轉換存在的情況下，會忽略任何使用者定義類型之間轉換這些。 尤其是：

*  如果預先定義的隱含轉換 ([隱含轉換](conversions.md#implicit-conversions)) 從類型`S`鍵入`T`，所有使用者定義轉換 （隱含或明確） 從`S`至`T`都會被忽略。
*  如果預先定義的明確轉換 ([明確轉換](conversions.md#explicit-conversions)) 從類型`S`鍵入`T`，從，任何使用者定義的明確轉換`S`至`T`都會被忽略。 此外：

如果`T`介面類型時，使用者定義的隱含地轉換`S`到`T`都會被忽略。

否則，使用者定義的隱含轉換，從`S`至`T`仍會視為。

所有類型，但`object`，由宣告運算子`Convertible<T>`上述類型預先定義的轉換不會衝突。 例如: 
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

不過，對於型別`object`，預先定義的轉換會隱藏在所有情況下，但是其中一種使用者定義的轉換：

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

使用者定義的轉換不允許將轉換至或從中*interface_type*s。 特別的是，這項限制可確保沒有使用者定義轉換發生時將轉換成*interface_type*，且以轉換成*interface_type*才會成功的物件正在轉換實際實作指定*interface_type*。

轉換運算子的簽章是由來源類型和目標型別所組成。 （請注意這是為其傳回類型會參與簽章的成員的唯一形式）。`implicit`或`explicit`轉換運算子的分類不是運算子的簽章的一部分。 因此，類別或結構不能宣告兩者`implicit`和`explicit`具有相同的來源和目標類型的轉換運算子。

一般情況下，應該永遠不會擲回例外狀況，並不會遺失資訊設計使用者定義的隱含轉換。 如果使用者定義的轉換可能會導致大量例外狀況 （例如，因為來源引數超出範圍） 或遺失的資訊 （例如捨棄高序位位元），則該轉換應該定義為明確的轉換。

在範例
```csharp
using System;

public struct Digit
{
    byte value;

    public Digit(byte value) {
        if (value < 0 || value > 9) throw new ArgumentException();
        this.value = value;
    }

    public static implicit operator byte(Digit d) {
        return d.value;
    }

    public static explicit operator Digit(byte b) {
        return new Digit(b);
    }
}
```
從轉換`Digit`要`byte`是隱含的因為它永遠不會擲回例外狀況或遺失的詳細資訊，但從轉換`byte`要`Digit`是因為明確`Digit`只能表示可能的子集值`byte`。

## <a name="instance-constructors"></a>執行個體建構函式

「執行個體建構函式」是實作將類別執行個體初始化所需之動作的成員。 執行個體建構函式使用宣告*constructor_declaration*s:

```antlr
constructor_declaration
    : attributes? constructor_modifier* constructor_declarator constructor_body
    ;

constructor_modifier
    : 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'extern'
    | constructor_modifier_unsafe
    ;

constructor_declarator
    : identifier '(' formal_parameter_list? ')' constructor_initializer?
    ;

constructor_initializer
    : ':' 'base' '(' argument_list? ')'
    | ':' 'this' '(' argument_list? ')'
    ;

constructor_body
    : block
    | ';'
    ;
```

A *constructor_declaration*可能包含一組*屬性*([屬性](attributes.md))，是有效的四種存取修飾詞組合 ([存取修飾詞](classes.md#access-modifiers))，以及`extern`([外部方法](classes.md#external-methods)) 修飾詞。 建構函式宣告不允許多次包含相同的修飾詞。

*識別碼*的*constructor_declarator*必須命名的執行個體建構函式宣告所在的類別。 如果指定的任何其他名稱，則會發生編譯時期錯誤。

選擇性*formal_parameter_list*執行個體的建構函式受限於相同的規則*formal_parameter_list*方法的 ([方法](classes.md#methods))。 型式參數清單會定義簽章 ([簽章和多載](basic-concepts.md#signatures-and-overloading)) 的執行個體建構函式，並且讓管理程序多載解析 ([型別推斷](expressions.md#type-inference)) 選取特定用引動過程中的執行個體建構函式。

每個參考中的型別*formal_parameter_list*執行個體的建構函式必須是至少一樣地存取自己的建構函式 ([存取範圍條件約束](basic-concepts.md#accessibility-constraints))。

選擇性*constructor_initializer*指定執行中的陳述式之前叫用的另一個執行個體建構函式*constructor_body*這個執行個體建構函式。 這是更進一步的說明[建構函式初始設定式](classes.md#constructor-initializers)。

當建構函式宣告包含`extern`修飾詞，建構函式即為***外部的建構函式***。 因為外部的建構函式宣告未不提供任何實際的實作，其*constructor_body*只包含一個分號。 所有其他的建構函式，如*constructor_body*組成*區塊*指定的陳述式來初始化類別的新執行個體。 這就相當於*區塊*的執行個體方法`void`傳回型別 ([方法主體](classes.md#method-body))。

執行個體建構函式不會繼承。 因此，類別具有以外實際上在類別中宣告的任何執行個體建構函式。 如果類別未不包含任何執行個體建構函式宣告，會自動提供的預設執行個體建構函式 ([預設建構函式](classes.md#default-constructors))。

執行個體建構函式會叫用*object_creation_expression*s ([物件建立運算式](expressions.md#object-creation-expressions)) 及透過*constructor_initializer*s。

### <a name="constructor-initializers"></a>建構函式初始設定式

所有執行個體建構函式 (但不包括類別`object`)，隱含地包含另一個執行個體建構函式的引動過程正前方*constructor_body*。 隱含地叫用建構函式由*constructor_initializer*:

*  表單的執行個體建構函式初始設定式`base(argument_list)`或`base()`會導致叫用直接基底類別的執行個體建構函式。 使用選取該建構函式*argument_list*如果存在且多載解析規則[多載解析](expressions.md#overload-resolution)。 候選項目執行個體建構函式的集合，包含所有可存取的執行個體建構函式的直接基底類別中所包含的預設建構函式 ([預設建構函式](classes.md#default-constructors))，如果沒有執行個體建構函式宣告中直接基底類別。 如果這個集合是空的或如果找不到單一最佳的執行個體建構函式，就會發生編譯時期錯誤。
*  表單的執行個體建構函式初始設定式`this(argument-list)`或`this()`會導致從本身要叫用類別的執行個體建構函式。 使用選取的建構函式*argument_list*如果存在且多載解析規則[多載解析](expressions.md#overload-resolution)。 候選項目執行個體建構函式的集合是由所有可存取的執行個體建構函式宣告中類別本身所組成。 如果這個集合是空的或如果找不到單一最佳的執行個體建構函式，就會發生編譯時期錯誤。 如果執行個體建構函式宣告包含本身的建構函式會叫用的建構函式初始設定式，就會發生編譯時期錯誤。

如果執行個體建構函式有沒有建構函式初始設定式，在表單的建構函式初始設定式`base()`以隱含方式提供。 因此，表單執行個體建構函式宣告
```csharp
C(...) {...}
```
就完全相當於
```csharp
C(...): base() {...}
```

所指定之參數的範圍*formal_parameter_list*的執行個體建構函式的宣告包含宣告的建構函式初始設定式。 因此，建構函式初始設定式可以存取的建構函式的參數。 例如: 
```csharp
class A
{
    public A(int x, int y) {}
}

class B: A
{
    public B(int x, int y): base(x + y, x - y) {}
}
```

執行個體建構函式初始設定式無法存取所建立的執行個體。 因此它是編譯時期錯誤參考`this`建構函式初始設定式的引數運算式，做為是它的任何執行個體透過參考成員的引數運算式的編譯時期錯誤*simple_name*.

### <a name="instance-variable-initializers"></a>執行個體變數初始設定式

當執行個體建構函式有沒有建構函式初始設定式，或它有表單的建構函式初始設定式`base(...)`，該建構函式會隱含地執行所指定的初始化*variable_initializer*的 s在其類別中，宣告的執行個體欄位。 這會對應到一系列的項目建構函式，以及直接基底類別建構函式的隱含引動過程之前立即執行的設定。 變數的初始設定式會以其出現在類別宣告中的文字順序執行。

### <a name="constructor-execution"></a>建構函式執行

變數初始設定式會轉換成指派陳述式，以及這些指派陳述式會在基底類別的執行個體建構函式的引動過程之前執行。 這種排序可確保能夠存取該執行個體的任何陳述式會在執行之前，會由其區域變數初始設定式初始化所有執行個體欄位。

提供的範例
```csharp
using System;

class A
{
    public A() {
        PrintFields();
    }

    public virtual void PrintFields() {}
}

class B: A
{
    int x = 1;
    int y;

    public B() {
        y = -1;
    }

    public override void PrintFields() {
        Console.WriteLine("x = {0}, y = {1}", x, y);
    }
}
```
當`new B()`用來建立的執行個體`B`，會產生下列輸出：
```
x = 1, y = 0
```

值`x`為 1，因為變數的初始設定式會在基底類別的執行個體建構函式會叫用之前執行。 不過，值`y`為 0 (預設值`int`) 因為指派給`y`之前不會執行基底類別建構函式傳回之後。

您最好將視為自動插入之前的陳述式執行個體變數初始設定式和建構函式初始設定式*constructor_body*。 此範例
```csharp
using System;
using System.Collections;

class A
{
    int x = 1, y = -1, count;

    public A() {
        count = 0;
    }

    public A(int n) {
        count = n;
    }
}

class B: A
{
    double sqrt2 = Math.Sqrt(2.0);
    ArrayList items = new ArrayList(100);
    int max;

    public B(): this(100) {
        items.Add("default");
    }

    public B(int n): base(n - 1) {
        max = n;
    }
}
```
包含數個變數初始設定式;它也包含這兩種形式的建構函式初始設定式 (`base`和`this`)。 此範例相當於程式碼如下所示，其中每個註解指出會自動插入的陳述式 （用於自動插入建構函式引動過程的語法無效，但只提供可說明此機制）。

```csharp
using System.Collections;

class A
{
    int x, y, count;

    public A() {
        x = 1;                       // Variable initializer
        y = -1;                      // Variable initializer
        object();                    // Invoke object() constructor
        count = 0;
    }

    public A(int n) {
        x = 1;                       // Variable initializer
        y = -1;                      // Variable initializer
        object();                    // Invoke object() constructor
        count = n;
    }
}

class B: A
{
    double sqrt2;
    ArrayList items;
    int max;

    public B(): this(100) {
        B(100);                      // Invoke B(int) constructor
        items.Add("default");
    }

    public B(int n): base(n - 1) {
        sqrt2 = Math.Sqrt(2.0);      // Variable initializer
        items = new ArrayList(100);  // Variable initializer
        A(n - 1);                    // Invoke A(int) constructor
        max = n;
    }
}
```

### <a name="default-constructors"></a>預設建構函式

如果類別未不包含任何執行個體建構函式宣告，會自動提供的預設執行個體建構函式。 該預設建構函式只會叫用直接基底類別的無參數建構函式。 如果類別是抽象的是受保護的預設建構函式的宣告存取範圍。 否則，預設建構函式的宣告存取範圍是公用的。 因此，預設建構函式一律會是表單

```csharp
protected C(): base() {}
```
或
```csharp
public C(): base() {}
```
其中`C`是類別的名稱。 如果多載解析無法判斷基底類別建構函式初始設定式的唯一最佳候選項目，則會發生編譯時期錯誤。

在範例
```csharp
class Message
{
    object sender;
    string text;
}
```
提供的預設建構函式，因為這個類別不包含任何執行個體建構函式宣告。 因此，範例就相當於
```csharp
class Message
{
    object sender;
    string text;

    public Message(): base() {}
}
```

### <a name="private-constructors"></a>私用建構函式

當類別`T`宣告只有私用執行個體建構函式，它無法進行類別外的程式文字`T`衍生自`T`，或直接建立的執行個體`T`。 因此，如果類別只包含靜態成員，而且不想要具現化，加入空白的私用執行個體建構函式會造成具現化。 例如：
```csharp
public class Trig
{
    private Trig() {}        // Prevent instantiation

    public const double PI = 3.14159265358979323846;

    public static double Sin(double x) {...}
    public static double Cos(double x) {...}
    public static double Tan(double x) {...}
}
```

`Trig`類別群組相關的方法與常數，但不是要具現化。 因此它會宣告單一空白的私用執行個體建構函式。 必須宣告至少一個執行個體建構函式，來隱藏自動產生預設建構函式。

### <a name="optional-instance-constructor-parameters"></a>選擇性的執行個體建構函式參數

`this(...)`形式的建構函式初始設定式通常用於搭配多載實作選擇性的執行個體建構函式參數。 在範例
```csharp
class Text
{
    public Text(): this(0, 0, null) {}

    public Text(int x, int y): this(x, y, null) {}

    public Text(int x, int y, string s) {
        // Actual constructor implementation
    }
}
```
第一次的兩個執行個體建構函式只是提供遺漏的引數的預設值。 兩者都使用`this(...)`建構函式初始設定式來叫用第三個執行個體建構函式，它會實際初始化新執行個體的工作。 效果是選擇性的建構函式參數：
```csharp
Text t1 = new Text();                    // Same as Text(0, 0, null)
Text t2 = new Text(5, 10);               // Same as Text(5, 10, null)
Text t3 = new Text(5, 20, "Hello");
```

## <a name="static-constructors"></a>靜態建構函式

A***靜態建構函式***是實作初始化封閉的類別型別所需之動作的成員。 靜態建構函式使用宣告*static_constructor_declaration*s:

```antlr
static_constructor_declaration
    : attributes? static_constructor_modifiers identifier '(' ')' static_constructor_body
    ;

static_constructor_modifiers
    : 'extern'? 'static'
    | 'static' 'extern'?
    | static_constructor_modifiers_unsafe
    ;

static_constructor_body
    : block
    | ';'
    ;
```

A *static_constructor_declaration*可能包含一組*屬性*([屬性](attributes.md)) 以及`extern`修飾詞 ([外部方法](classes.md#external-methods)).

*識別碼*的*static_constructor_declaration*必須命名為在靜態建構函式宣告的類別。 如果指定的任何其他名稱，則會發生編譯時期錯誤。

當靜態建構函式宣告包含`extern`修飾詞，靜態建構函式即為***外部的靜態建構函式***。 因為外部的靜態建構函式宣告未不提供任何實際的實作，其*static_constructor_body*只包含一個分號。 所有其他的靜態建構函式宣告中， *static_constructor_body*組成*區塊*指定要執行以便初始化類別的陳述式。 這就相當於*method_body*使用的靜態方法`void`傳回型別 ([方法主體](classes.md#method-body))。

靜態建構函式不會繼承，並不能直接呼叫。

已關閉的類別類型的靜態建構函式最多一次執行指定的應用程式定義域中。 靜態建構函式的執行是由應用程式定義域中發生下列事件的第一個觸發：

*  建立類別類型的執行個體。
*  參考的任何類別類型的靜態成員。

如果類別包含`Main`方法 ([應用程式啟動](basic-concepts.md#application-startup)) 中執行開始時，靜態建構函式之前執行該類別的`Main`呼叫方法。

若要初始化新的已關閉的類別類型，第一次一組新的靜態欄位 ([靜態和執行個體欄位](classes.md#static-and-instance-fields)) 會建立該特定的封閉型別。 每個靜態欄位初始化為其預設值 ([預設值](variables.md#default-values))。 下一步，靜態欄位初始設定式 ([靜態欄位初始化](classes.md#static-field-initialization)) 會執行這些靜態欄位。 最後，靜態建構函式會執行。

此範例
```csharp
using System;

class Test
{
    static void Main() {
        A.F();
        B.F();
    }
}

class A
{
    static A() {
        Console.WriteLine("Init A");
    }
    public static void F() {
        Console.WriteLine("A.F");
    }
}

class B
{
    static B() {
        Console.WriteLine("Init B");
    }
    public static void F() {
        Console.WriteLine("B.F");
    }
}
```
必須產生的輸出：
```
Init A
A.F
Init B
B.F
```
因為執行`A`的靜態建構函式的呼叫所觸發`A.F`，及執行`B`的靜態建構函式的呼叫所觸發`B.F`。

就可以建構允許使用變數的初始設定式的預設值狀態的靜態欄位的循環相依性。

此範例
```csharp
using System;

class A
{
    public static int X;

    static A() {
        X = B.Y + 1;
    }
}

class B
{
    public static int Y = A.X + 1;

    static B() {}

    static void Main() {
        Console.WriteLine("X = {0}, Y = {1}", A.X, B.Y);
    }
}
```
產生下列輸出
```
X = 1, Y = 2
```

若要執行`Main`方法，系統第一次執行的初始設定式`B.Y`類別之前，`B`的靜態建構函式。 `Y`初始設定式會導致`A`的執行，因為靜態建構函式的值`A.X`參考。 靜態建構函式 `A`接著會繼續計算的值 `X`，並在此情況下的預設值是提取 `Y`，這是零。 `A.X` 因此會初始化為 1。 執行的程序`A`的靜態欄位初始設定式和靜態建構函式，然後完成時，傳回的初始值計算 `Y`，其結果就會變成 2。

針對每個封閉式建構的類別型別，正好一次執行靜態建構函式，因為它是方便的地方，以強制執行無法在編譯時期透過條件約束檢查的型別參數的執行階段檢查 ([型別參數條件約束](classes.md#type-parameter-constraints))。 例如，下列類型會使用靜態建構函式來強制執行型別引數是列舉：
```csharp
class Gen<T> where T: struct
{
    static Gen() {
        if (!typeof(T).IsEnum) {
            throw new ArgumentException("T must be an enum");
        }
    }
}
```

## <a name="destructors"></a>解構函式

A***解構函式***成員實作來解構類別的執行個體所需的動作。 使用宣告解構函式*destructor_declaration*:

```antlr
destructor_declaration
    : attributes? 'extern'? '~' identifier '(' ')' destructor_body
    | destructor_declaration_unsafe
    ;

destructor_body
    : block
    | ';'
    ;
```

A *destructor_declaration*可能包含一組*屬性*([屬性](attributes.md))。

*識別碼*的*destructor_declaration*宣告解構函式的類別必須命名。 如果指定的任何其他名稱，則會發生編譯時期錯誤。

當解構函式宣告包含`extern`修飾詞，解構函式即為***外部的解構函式***。 因為外部的解構函式宣告未不提供任何實際的實作，其*destructor_body*只包含一個分號。 對於所有其他的解構函式， *destructor_body*組成*區塊*指定要執行以便解構類別的執行個體的陳述式。 A *destructor_body*就相當於*method_body*的執行個體方法`void`傳回型別 ([方法主體](classes.md#method-body))。

解構函式不會繼承。 因此，類別也有可能會在該類別宣告以外的任何解構函式。

解構函式需要不有任何參數，因為它不能多載的情況，因此類別可以有，最多一個解構函式。

解構函式會自動叫用，並無法明確地叫用。 已不再使用該執行個體的任何程式碼時，適合進行解構執行個體。 執行執行個體的解構函式可能會發生在任何時間之後的執行個體會變成適合進行解構。 當解構執行個體時，該執行個體的繼承鏈結中的解構函式呼叫時，依序以最多衍生至最少衍生。 解構函式可能會在任何執行緒上執行。 如進一步討論管理何時及如何執行解構函式的規則，請參閱[自動記憶體管理](basic-concepts.md#automatic-memory-management)。

此範例的輸出
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("A's destructor");
    }
}

class B: A
{
    ~B() {
        Console.WriteLine("B's destructor");
    }
}

class Test
{
   static void Main() {
        B b = new B();
        b = null;
        GC.Collect();
        GC.WaitForPendingFinalizers();
   }
}
```
是
```
B's destructor
A's destructor
```
因為順序，會呼叫解構函式中的繼承鏈結以最多衍生至最少衍生。

解構函式會藉由覆寫虛擬方法實作`Finalize`上`System.Object`。 C# 程式不允許覆寫這個方法，或呼叫 （或它的覆寫） 直接。 比方說，程式
```csharp
class A 
{
    override protected void Finalize() {}    // error

    public void F() {
        this.Finalize();                     // error
    }
}
```
包含兩個錯誤。

編譯器行為就如同此方法，並覆寫它，進行根本不存在。 因此，此程式：
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
有效，並顯示隱藏的方法`System.Object`的`Finalize`方法。

如需行為的討論的解構函式擲回例外狀況時，請參閱[如何處理例外狀況](exceptions.md#how-exceptions-are-handled)。

## <a name="iterators"></a>迭代器

函式成員 ([函式成員](expressions.md#function-members)) 實作使用迭代器區塊 ([區塊](statements.md#blocks)) 稱為***迭代器***。

迭代器區塊可能會用做函式成員的主體，只要對應的函式成員的傳回型別是其中一個列舉程式介面 ([列舉程式介面](classes.md#enumerator-interfaces)) 或其中一個可列舉的介面 ([可列舉的介面](classes.md#enumerable-interfaces))。 它可能是以*method_body*， *operator_body*或是*accessor_body*，而事件、 執行個體建構函式、 靜態建構函式和解構函式不可為實作為迭代器。

使用迭代器區塊來實作函式成員時，它是型式參數清單中指定任何函式成員的編譯時期錯誤`ref`或`out`參數。

### <a name="enumerator-interfaces"></a>列舉程式介面

***列舉程式介面***為非泛型介面`System.Collections.IEnumerator`及 泛型介面的所有具現化`System.Collections.Generic.IEnumerator<T>`。 為了保持簡潔，在這一章中這些介面會當做`IEnumerator`和`IEnumerator<T>`分別。

### <a name="enumerable-interfaces"></a>可列舉的介面

***可列舉的介面***為非泛型介面`System.Collections.IEnumerable`及 泛型介面的所有具現化`System.Collections.Generic.IEnumerable<T>`。 為了保持簡潔，在這一章中這些介面會當做`IEnumerable`和`IEnumerable<T>`分別。

### <a name="yield-type"></a>產生類型

迭代器會產生一連串的值，所有型別相同。 這種類型稱為***產生類型***迭代器。

*  傳回迭代器的 yield 類型`IEnumerator`或是`IEnumerable`是`object`。
*  傳回迭代器的 yield 類型`IEnumerator<T>`或是`IEnumerable<T>`是`T`。

### <a name="enumerator-objects"></a>列舉值物件

使用迭代器區塊來實作介面型別傳回列舉值的函式成員時，叫用函式成員不會立即執行程式碼中的迭代器區塊。 相反地，***列舉值物件***會建立並傳回。 此物件會封裝在迭代器區塊中，指定的程式碼和的迭代器區塊中的程式碼會執行當 enumerator 物件的`MoveNext`叫用方法。 列舉值物件具有下列特性：

*  它會實作`IEnumerator`並`IEnumerator<T>`，其中`T`iterator 的 yield 類型。
*  它會實作 `System.IDisposable`。
*  （如果有的話），它會初始化引數值的複本和執行個體的值傳遞至函式成員。
*  它有四個可能的狀態：***之前***，***執行***，***暫止***，以及***之後***，和一開始處於***之前***狀態。

列舉值物件通常是封裝在迭代器區塊中的程式碼，並實作列舉程式介面中，編譯器產生的列舉值類別的執行個體，但可能會有其他的實作方法。 如果列舉值類別由編譯器產生的該類別巢狀，直接或間接包含函式成員的類別中會有私用存取範圍，而它必須保留供編譯器使用的名稱 ([識別碼](lexical-structure.md#identifiers)).

列舉值物件可以實作比上述指定的多個介面。

下列各節描述的確切行為`MoveNext`， `Current`，並`Dispose`的成員`IEnumerable`和`IEnumerable<T>`介面列舉值物件所提供的實作。

請注意，不支援列舉值物件`IEnumerator.Reset`方法。 叫用這個方法會導致`System.NotSupportedException`擲回。

#### <a name="the-movenext-method"></a>MoveNext 方法

`MoveNext`方法的列舉值物件封裝的迭代器區塊的程式碼。 叫用`MoveNext`方法會執行程式碼中的迭代器區塊並將設定`Current`適當的列舉值物件的屬性。 所執行的精確動作`MoveNext`取決於列舉值物件的狀態時`MoveNext`叫用：

*  如果列舉值物件的狀態***之前***叫用、 `MoveNext`:
   * 狀態變更為***執行***。
   * 初始化參數 (包括`this`) 的引數值和列舉值物件初始化時，所儲存的執行個體值的迭代器區塊。
   * 執行從開頭的迭代器區塊，直到執行將會中斷 （如下所述）。
*  如果列舉值物件的狀態***執行***，叫用的結果`MoveNext`未指定。
*  如果列舉值物件的狀態***暫止***叫用、 `MoveNext`:
   * 狀態變更為***執行***。
   * 迭代器區塊執行的最後暫停時，儲存的值來還原所有的本機變數和參數 （包括這） 的值。 請注意，這些變數所參考的任何物件的內容可能已變更，因為前一個呼叫 MoveNext。
   * 繼續執行的正後方的迭代器區塊`yield return`陳述式，造成執行暫停並繼續進行，直到執行將會中斷 （如下所述）。
*  如果列舉值物件的狀態***之後***、 叫用`MoveNext`傳回`false`。


當`MoveNext`執行迭代器區塊中，可以中斷執行，以四種方式：藉由`yield return`陳述式、 藉由`yield break`陳述式遇到區塊的結尾迭代器，以及例外狀況被擲回並傳播出迭代器區塊。

*  當`yield return`遇到陳述式 ([yield 陳述式](statements.md#the-yield-statement)):
   * 陳述式中指定的運算式會評估，以隱含方式轉換產生的型別，並指派給`Current`列舉值物件的屬性。
   * 暫停執行的迭代器主體。 所有本機變數和參數的值 (包括`this`) 會儲存，因為這個位置`yield return`陳述式。 如果`yield return`陳述式位於一或多個`try`區塊中使用，相關聯`finally`區塊不會執行這一次。
   * 列舉值物件的狀態會變為***暫止***。
   * `MoveNext`方法會傳回`true`給其呼叫端，表示在反覆項目成功地前移至下一個值。
*  當`yield break`遇到陳述式 ([yield 陳述式](statements.md#the-yield-statement)):
   * 如果`yield break`陳述式位於一或多個`try`區塊中使用，相關聯`finally`區塊會執行。
   * 列舉值物件的狀態會變為***之後***。
   * `MoveNext`方法會傳回`false`給其呼叫端，表示在反覆項目已完成。
*  當遇到的迭代器主體結尾：
   * 列舉值物件的狀態會變為***之後***。
   * `MoveNext`方法會傳回`false`給其呼叫端，表示在反覆項目已完成。
*  當例外狀況會擲回，並傳播出迭代器區塊：
   * 適當`finally`迭代器主體中的區塊將已執行的例外狀況傳播。
   * 列舉值物件的狀態會變為***之後***。
   * 例外狀況傳播的呼叫端會繼續`MoveNext`方法。

#### <a name="the-current-property"></a>目前的屬性

列舉值物件的`Current`屬性會受到`yield return`迭代器區塊中的陳述式。

當列舉值物件處於***暫止***狀態時，值`Current`是先前呼叫所設定的值`MoveNext`。 當列舉值物件處於***之前***，***執行***，或***之後***所述，存取的結果`Current`未指定。

迭代器與 yield 類型並非`object`，存取的結果`Current`透過列舉值物件的`IEnumerable`實作對應至存取`Current`透過列舉值物件的`IEnumerator<T>`實作，並將轉換結果`object`。

#### <a name="the-dispose-method"></a>Dispose 方法

`Dispose`方法用來清除反覆項目列舉值物件帶入***之後***狀態。

*  如果列舉值物件的狀態***之前***、 叫用`Dispose`狀態變更為***之後***。
*  如果列舉值物件的狀態***執行***，叫用的結果`Dispose`未指定。
*  如果列舉值物件的狀態***暫止***叫用、 `Dispose`:
   * 狀態變更為***執行***。
   * 執行任何 finally 區塊，如果上次執行的`yield return`陳述式已`yield break`陳述式。 如果這導致例外狀況擲回並傳播到迭代器主體之外，列舉值物件的狀態設為***之後***例外狀況會傳播給呼叫端`Dispose`方法。
   * 狀態變更為***之後***。
*  如果列舉值物件的狀態***之後***叫用、`Dispose`沒有任何作用。

### <a name="enumerable-objects"></a>可列舉的物件

使用迭代器區塊來實作傳回可列舉的介面類型的函式成員時，叫用函式成員不會立即執行程式碼中的迭代器區塊。 相反地，***可列舉物件***會建立並傳回。 可列舉的物件`GetEnumerator`方法會傳回列舉值物件，封裝的程式碼區塊中指定迭代器，而且執行中的迭代器區塊的程式碼，就會發生時 enumerator 物件的`MoveNext`叫用方法。 可列舉的物件具有下列特性：

*  它會實作`IEnumerable`並`IEnumerable<T>`，其中`T`iterator 的 yield 類型。
*  （如果有的話），它會初始化引數值的複本和執行個體的值傳遞至函式成員。

可列舉的物件通常是類別的執行個體編譯器產生可列舉的封裝中的迭代器區塊的程式碼，並實作可列舉的介面，但可能會有其他的實作方法。 如果由編譯器產生的可列舉的類別，該類別巢狀，直接或間接包含函式成員的類別中會有私用存取範圍，而且會有保留供編譯器使用的名稱 ([識別碼](lexical-structure.md#identifiers)).

可列舉物件可以實作比上述指定的多個介面。 特別是，也會實作可列舉物件`IEnumerator`和`IEnumerator<T>`，讓它做為可列舉和列舉程式。 在這類的實作中，第一次可列舉物件的`GetEnumerator`叫用方法時，會傳回可列舉物件本身。 後續的引動過程之可列舉物件`GetEnumerator`，如果任何項目，傳回可列舉物件的複本。 因此，每個傳回的列舉程式有自己的狀態，和一個列舉值的變更將不會影響另一個。

#### <a name="the-getenumerator-method"></a>GetEnumerator 方法

可列舉物件會提供實作`GetEnumerator`方法`IEnumerable`和`IEnumerable<T>`介面。 這兩個`GetEnumerator`方法共用通用的實作，取得並傳回可用的列舉值物件。 列舉值物件會以引數的值初始化和執行個體的可列舉物件時初始化，否則儲存值的列舉值物件函式中所述[列舉值物件](classes.md#enumerator-objects)。

### <a name="implementation-example"></a>實作範例

本節說明可能的實作，根據標準的 C# 建構迭代器。 此處所述的實作根據 Microsoft C# 編譯器，使用相同的原則，但它不是託管的實作或只有一個可能。

下列`Stack<T>`類別會實作其`GetEnumerator`使用迭代器的方法。 迭代器會列舉由上而下順序中的堆疊項的目。

```csharp
using System;
using System.Collections;
using System.Collections.Generic;

class Stack<T>: IEnumerable<T>
{
    T[] items;
    int count;

    public void Push(T item) {
        if (items == null) {
            items = new T[4];
        }
        else if (items.Length == count) {
            T[] newItems = new T[count * 2];
            Array.Copy(items, 0, newItems, 0, count);
            items = newItems;
        }
        items[count++] = item;
    }

    public T Pop() {
        T result = items[--count];
        items[count] = default(T);
        return result;
    }

    public IEnumerator<T> GetEnumerator() {
        for (int i = count - 1; i >= 0; --i) yield return items[i];
    }
}
```

`GetEnumerator`方法可以轉譯為封裝的程式碼在迭代器區塊中，編譯器產生的列舉值類別的具現化，如下列所示。

```csharp
class Stack<T>: IEnumerable<T>
{
    ...

    public IEnumerator<T> GetEnumerator() {
        return new __Enumerator1(this);
    }

    class __Enumerator1: IEnumerator<T>, IEnumerator
    {
        int __state;
        T __current;
        Stack<T> __this;
        int i;

        public __Enumerator1(Stack<T> __this) {
            this.__this = __this;
        }

        public T Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            switch (__state) {
                case 1: goto __state1;
                case 2: goto __state2;
            }
            i = __this.count - 1;
        __loop:
            if (i < 0) goto __state2;
            __current = __this.items[i];
            __state = 1;
            return true;
        __state1:
            --i;
            goto __loop;
        __state2:
            __state = 2;
            return false;
        }

        public void Dispose() {
            __state = 2;
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

在上述的轉譯，迭代器區塊中的程式碼會轉換成狀態機器，並放置於`MoveNext`列舉值類別的方法。 此外，區域變數`i`會轉換成列舉值物件中的欄位中，因此它可以繼續存在的引動過程之間`MoveNext`。

下列範例會列印整數 1 到 10 的簡單乘法表。 `FromTo`在範例中的方法會傳回可列舉的物件，並使用迭代器實作。

```csharp
using System;
using System.Collections.Generic;

class Test
{
    static IEnumerable<int> FromTo(int from, int to) {
        while (from <= to) yield return from++;
    }

    static void Main() {
        IEnumerable<int> e = FromTo(1, 10);
        foreach (int x in e) {
            foreach (int y in e) {
                Console.Write("{0,3} ", x * y);
            }
            Console.WriteLine();
        }
    }
}
```

`FromTo`方法可以轉譯為編譯器產生 enumerable 類別會封裝在迭代器區塊中，程式碼的具現化，如下列所示。

```csharp
using System;
using System.Threading;
using System.Collections;
using System.Collections.Generic;

class Test
{
    ...

    static IEnumerable<int> FromTo(int from, int to) {
        return new __Enumerable1(from, to);
    }

    class __Enumerable1:
        IEnumerable<int>, IEnumerable,
        IEnumerator<int>, IEnumerator
    {
        int __state;
        int __current;
        int __from;
        int from;
        int to;
        int i;

        public __Enumerable1(int __from, int to) {
            this.__from = __from;
            this.to = to;
        }

        public IEnumerator<int> GetEnumerator() {
            __Enumerable1 result = this;
            if (Interlocked.CompareExchange(ref __state, 1, 0) != 0) {
                result = new __Enumerable1(__from, to);
                result.__state = 1;
            }
            result.from = result.__from;
            return result;
        }

        IEnumerator IEnumerable.GetEnumerator() {
            return (IEnumerator)GetEnumerator();
        }

        public int Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            switch (__state) {
            case 1:
                if (from > to) goto case 2;
                __current = from++;
                __state = 1;
                return true;
            case 2:
                __state = 2;
                return false;
            default:
                throw new InvalidOperationException();
            }
        }

        public void Dispose() {
            __state = 2;
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

Enumerable 類別會實作可列舉的介面和列舉程式介面，讓它做為可列舉和列舉程式。 第一次`GetEnumerator`叫用方法時，會傳回可列舉物件本身。 後續的引動過程之可列舉物件`GetEnumerator`，如果任何項目，傳回可列舉物件的複本。 因此，每個傳回的列舉程式有自己的狀態，和一個列舉值的變更將不會影響另一個。 `Interlocked.CompareExchange`方法用來確保安全執行緒作業。

`from`和`to`參數會轉換成 enumerable 類別中的欄位。 因為`from`修改在迭代器區塊中，額外`__from`引進欄位來保存初始值給`from`中每個列舉值。

`MoveNext`方法會擲回`InvalidOperationException`如果它時，會呼叫`__state`是`0`。 這樣可以防止作為列舉值物件，但是未先呼叫之可列舉物件`GetEnumerator`。

下列範例顯示簡單的樹狀目錄類別。 `Tree<T>`類別會實作其`GetEnumerator`使用迭代器的方法。 迭代器列舉順序中置中的樹狀結構的項目。

```csharp
using System;
using System.Collections.Generic;

class Tree<T>: IEnumerable<T>
{
    T value;
    Tree<T> left;
    Tree<T> right;

    public Tree(T value, Tree<T> left, Tree<T> right) {
        this.value = value;
        this.left = left;
        this.right = right;
    }

    public IEnumerator<T> GetEnumerator() {
        if (left != null) foreach (T x in left) yield x;
        yield value;
        if (right != null) foreach (T x in right) yield x;
    }
}

class Program
{
    static Tree<T> MakeTree<T>(T[] items, int left, int right) {
        if (left > right) return null;
        int i = (left + right) / 2;
        return new Tree<T>(items[i], 
            MakeTree(items, left, i - 1),
            MakeTree(items, i + 1, right));
    }

    static Tree<T> MakeTree<T>(params T[] items) {
        return MakeTree(items, 0, items.Length - 1);
    }

    // The output of the program is:
    // 1 2 3 4 5 6 7 8 9
    // Mon Tue Wed Thu Fri Sat Sun

    static void Main() {
        Tree<int> ints = MakeTree(1, 2, 3, 4, 5, 6, 7, 8, 9);
        foreach (int i in ints) Console.Write("{0} ", i);
        Console.WriteLine();

        Tree<string> strings = MakeTree(
            "Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun");
        foreach (string s in strings) Console.Write("{0} ", s);
        Console.WriteLine();
    }
}
```

`GetEnumerator`方法可以轉譯為封裝的程式碼在迭代器區塊中，編譯器產生的列舉值類別的具現化，如下列所示。

```csharp
class Tree<T>: IEnumerable<T>
{
    ...

    public IEnumerator<T> GetEnumerator() {
        return new __Enumerator1(this);
    }

    class __Enumerator1 : IEnumerator<T>, IEnumerator
    {
        Node<T> __this;
        IEnumerator<T> __left, __right;
        int __state;
        T __current;

        public __Enumerator1(Node<T> __this) {
            this.__this = __this;
        }

        public T Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            try {
                switch (__state) {

                case 0:
                    __state = -1;
                    if (__this.left == null) goto __yield_value;
                    __left = __this.left.GetEnumerator();
                    goto case 1;

                case 1:
                    __state = -2;
                    if (!__left.MoveNext()) goto __left_dispose;
                    __current = __left.Current;
                    __state = 1;
                    return true;

                __left_dispose:
                    __state = -1;
                    __left.Dispose();

                __yield_value:
                    __current = __this.value;
                    __state = 2;
                    return true;

                case 2:
                    __state = -1;
                    if (__this.right == null) goto __end;
                    __right = __this.right.GetEnumerator();
                    goto case 3;

                case 3:
                    __state = -3;
                    if (!__right.MoveNext()) goto __right_dispose;
                    __current = __right.Current;
                    __state = 3;
                    return true;

                __right_dispose:
                    __state = -1;
                    __right.Dispose();

                __end:
                    __state = 4;
                    break;

                }
            }
            finally {
                if (__state < 0) Dispose();
            }
            return false;
        }

        public void Dispose() {
            try {
                switch (__state) {

                case 1:
                case -2:
                    __left.Dispose();
                    break;

                case 3:
                case -3:
                    __right.Dispose();
                    break;

                }
            }
            finally {
                __state = 4;
            }
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

中所使用的編譯器產生暫存`foreach`陳述式會將提取`__left`和`__right`列舉值物件的欄位。 `__state`仔細更新列舉值物件的欄位，讓正確`Dispose()`如果擲回例外狀況，則方法也會正確地呼叫。 請注意，不可以撰寫具有簡單轉譯的程式碼`foreach`陳述式。

## <a name="async-functions"></a>非同步函式

方法 ([方法](classes.md#methods)) 或匿名函式 ([匿名函式運算式](expressions.md#anonymous-function-expressions)) 與`async`修飾詞稱為***非同步函式***。 一般情況下，詞彙***非同步***用來描述任何一種函式具有`async`修飾詞。

它是編譯時期錯誤，如需非同步函式指定任何的型式參數清單`ref`或`out`參數。

*Return_type*非同步方法必須是其中一個`void`或是***工作類型***。 工作類型是`System.Threading.Tasks.Task`及來自建構型別的`System.Threading.Tasks.Task<T>`。 為了保持簡潔，在這一章中這些類型會當做`Task`和`Task<T>`分別。 非同步方法傳回的工作類型是要傳回工作。

工作類型的確切定義實作所定義，但是從語言的觀點來看工作型別為其中一個狀態不完整，成功或發生錯誤。 發生錯誤的工作記錄相關的例外狀況。 成功`Task<T>`記錄類型之結果的`T`。 工作類型是即時資訊，並因此可能的運算元 await 運算式 ([Await 運算式](expressions.md#await-expressions))。

非同步函式引動過程都能夠暫止評估透過 await 運算式 ([Await 運算式](expressions.md#await-expressions)) 在其主體中。 評估可能會在稍後繼續在暫止 await 運算式，藉由***繼續委派***。 繼續委派是型別`System.Action`，並叫用它時，非同步函式引動過程的評估會繼續從中斷處 await 運算式。 ***目前的呼叫端***非同步函式引動過程因原始呼叫端，如果永遠不會暫止的函式引動過程或繼續委派的最新呼叫端。

### <a name="evaluation-of-a-task-returning-async-function"></a>工作傳回非同步函式的評估

工作傳回非同步函式的引動過程會導致傳回的工作類型，產生的執行個體。 這就叫做***傳回的工作***的非同步函式。 工作一開始處於不完整的狀態。

非同步函式主體再評估直到它被擱置 （藉由到達 await 運算式） 或終止時，在這點控制回到呼叫端，以及傳回的工作。

當非同步函式主體結束時，傳回的工作移出時未完成的狀態：

*  如果因達到的 return 陳述式或主體的結尾，終止函式主體，任何結果值會記錄在傳回的工作中，放入成功的狀態。
*  如果函式主體會終止無法攔截的例外狀況的結果 ([throw 陳述式](statements.md#the-throw-statement)) 例外狀況會記錄在傳回工作進入錯誤狀態。

### <a name="evaluation-of-a-void-returning-async-function"></a>傳回 void 的非同步函式的評估

如果非同步函式的傳回型別是`void`，評估不同於上述方式如下：完成及目前的執行緒例外狀況會不傳回任何工作，因為函式改用通訊***同步處理內容***。 視實作而定，同步處理內容的確切定義，但代表目前的執行緒執行的"where"。 傳回 void 的非同步函式的評估開始、 成功完成，或導致擲回未攔截到例外狀況時，會收到通知的同步處理內容。

這可讓追蹤多少傳回 void 的非同步函式會在其下方執行的並決定如何傳播來自它們的例外狀況的內容。
