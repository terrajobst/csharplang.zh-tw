---
ms.openlocfilehash: e0def754174ab8646f9b849abe86d2c375c958b6
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703975"
---
# <a name="classes"></a>類別

「類別」（class）是一種資料結構，其中可能包含資料成員（常數和欄位）、函式成員（方法、屬性、事件、索引子、運算子、實例的函式、析構函式和靜態的函式）和巢狀型別。 類別類型支援繼承，這是衍生類別可以擴充和特殊化基類的機制。

## <a name="class-declarations"></a>類別宣告

*Class_declaration*是宣告新類別的*type_declaration* （[型](namespaces.md#type-declarations)別宣告）。

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

*Class_declaration*由一組選擇性的*屬性*（[屬性](attributes.md)）所組成，後面接著一組選擇性的*class_modifier*s （[類別](classes.md#class-modifiers)修飾詞），後面接著一個選擇性的 `partial` 修飾詞，後面接著關鍵字 `class` 和名為類別的*識別碼*，接著是選擇性*的 type_parameter_list 規格*[（](classes.md#type-parameters)[類別基底規格](classes.md#class-base-specification)），接著以一組選擇性的*type_parameter_constraints_clause*s （[類型參數條件約束](classes.md#type-parameter-constraints)），後面接著*class_body* （[類別主體](classes.md#class-body)），並選擇性地後面加上分號。

類別宣告無法提供*type_parameter_constraints_clause*s，除非它也提供*type_parameter_list*。

提供*type_parameter_list*的類別宣告是***泛型類別***宣告。 此外，任何嵌套在泛型類別宣告或泛型結構宣告內的類別本身都是泛型類別宣告，因為必須提供包含類型的類型參數來建立結構化類型。

### <a name="class-modifiers"></a>類別修飾詞

*Class_declaration*可以選擇性地包含一連串的類別修飾詞：

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

在類別宣告中多次出現相同的修飾詞時，會發生編譯時期錯誤。

在嵌套類別上允許使用 `new` 修飾詞。 它會指定類別以相同名稱隱藏繼承的成員，如[新修飾](classes.md#the-new-modifier)詞中所述。 在不是嵌套類別宣告的類別宣告上出現 `new` 修飾詞時，就會發生編譯時期錯誤。

`public`、`protected`、`internal`和 `private` 修飾詞會控制類別的存取範圍。 根據類別宣告發生的內容，可能不允許其中一些修飾詞（宣告的[存取](basic-concepts.md#declared-accessibility)範圍）。

`abstract`、`sealed` 和 `static` 修飾詞會在下列各節中討論。

#### <a name="abstract-classes"></a>抽象類別

`abstract` 修飾詞是用來指出類別不完整，而且僅供做為基類使用。 抽象類別與非抽象類別有下列不同之處：

*  抽象類別無法直接具現化，而且在抽象類別上使用 `new` 運算子時，會發生編譯時期錯誤。 雖然可能有編譯時間類型為抽象的變數和值，但這類變數和值一定會 `null`，或包含衍生自抽象類別型之非抽象類別實例的參考。
*  允許抽象類別（但非必要）包含抽象成員。
*  抽象類別不能是密封的。

當非抽象類別衍生自抽象類別時，非抽象類別必須包括所有繼承之抽象成員的實際執行，因而覆寫這些抽象成員。 在範例中
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
抽象類別 `A` 引進抽象方法 `F`。 類別 `B` 引進了額外的方法 `G`，但因為它並未提供 `F`的執行，所以 `B` 也必須宣告為抽象。 類別 `C` 會覆寫 `F` 並提供實際的實作為。 因為 `C`中沒有抽象成員，所以允許（但非必要） `C` 為非抽象。

#### <a name="sealed-classes"></a>密封類別

`sealed` 修飾詞是用來防止衍生自類別。 如果將密封類別指定為另一個類別的基類，則會發生編譯時期錯誤。

密封的類別也不能是抽象類別。

`sealed` 修飾詞主要是用來防止非預期的衍生，但它也會啟用特定執行時間優化。 特別是，因為已知密封的類別永遠不會有任何衍生類別，所以可以將密封類別實例上的虛擬函式成員調用轉換成非虛擬調用。

#### <a name="static-classes"></a>靜態類別

`static` 修飾詞是用來標示要宣告為***靜態類別***的類別。 靜態類別無法具現化，不能當做類型使用，而且只能包含靜態成員。 只有靜態類別可以包含擴充方法（[擴充方法](classes.md#extension-methods)）的宣告。

靜態類別宣告受到下列限制：

*  靜態類別不能包含 `sealed` 或 `abstract` 修飾詞。 不過，請注意，因為靜態類別無法具現化或衍生自，所以其行為就像是密封和抽象。
*  靜態類別不能包含*class_base*規格（[類別基底規格](classes.md#class-base-specification)），而且無法明確指定基類或實作為實介面的清單。 靜態類別隱含繼承自類型 `object`。
*  靜態類別只能包含靜態成員（[靜態和實例成員](classes.md#static-and-instance-members)）。 請注意，常數和巢狀型別會分類為靜態成員。
*  靜態類別不能有具有 `protected` 或 `protected internal` 宣告存取範圍的成員。

這是編譯時期錯誤，違反上述任何一項限制。

靜態類別沒有實例的函式。 您不能在靜態類別中宣告實例的函式，也不會提供靜態類別的預設實例（[default](classes.md#default-constructors)）。

靜態類別的成員不會自動成為靜態，而且成員宣告必須明確包含 `static` 修飾詞（常數和巢狀型別除外）。 當類別是在靜態外部類別中嵌套時，除非明確包含 `static` 修飾詞，否則，此嵌套類別不是靜態類別。

__參考靜態類別類型__

若為，則允許*namespace_or_type_name* （[命名空間和類型名稱](basic-concepts.md#namespace-and-type-names)）參考靜態類別

*  *Namespace_or_type_name*是 `T.I`格式的*namespace_or_type_name*中的 `T`，或是
*  *Namespace_or_type_name*是表單`T` *typeof_expression* 的（[引數清單](expressions.md#argument-lists)1）中`typeof(T)`的。

若為，則允許*primary_expression* （函式[成員](expressions.md#function-members)）參考靜態類別

*  *Primary_expression* `E`是形式 的 *member_access* （[針對動態多載解析的編譯時間檢查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)）`E.I`中的。

在任何其他內容中，都是參考靜態類別的編譯時期錯誤。 例如，使用靜態類別做為基類、成員的構成類型（[嵌套](classes.md#nested-types)類型）、泛型型別引數或類型參數條件約束，就會發生錯誤。 同樣地，靜態類別不能用於陣列類型、指標類型、`new` 運算式、cast 運算式、`is` 運算式、`as` 運算式、`sizeof` 運算式或預設值運算式。

### <a name="partial-modifier"></a>Partial 修飾詞

`partial` 修飾詞是用來指出此*class_declaration*是部分類型宣告。 在封入命名空間或類型宣告中，多個具有相同名稱的部分類型宣告會結合成一個類型宣告，遵循[部分類型](classes.md#partial-types)中指定的規則。

如果在不同的內容中產生或維護這些區段，則在程式文字的個別區段上散發的類別宣告會很有用。 例如，類別宣告的其中一個部分可能是電腦產生的，而另一個則是手動撰寫。 這兩者的文字分隔會防止更新與另一個更新衝突。

### <a name="type-parameters"></a>型別參數

型別參數是簡單的識別碼，代表提供用來建立結構化型別之型別引數的預留位置。 類型參數是稍後將提供之類型的正式預留位置。 相較之下，型別引數（[型別引數](types.md#type-arguments)）是建立結構化型別時，用來取代型別參數的實際型別。

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

類別宣告中的每個型別參數都會定義該類別的宣告[空間（宣告](basic-concepts.md#declarations)）中的名稱。 因此，它不能與另一個類型參數或該類別中宣告的成員具有相同的名稱。 類型參數不能與類型本身具有相同的名稱。

### <a name="class-base-specification"></a>類別基底規格

類別宣告可以包含*class_base*規格，這會定義類別的直接基類，以及類別所直接實作為介面（[介面](interfaces.md)）。

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

在類別宣告中指定的基類可以是結構化類別類型（[結構化類型](types.md#constructed-types)）。 基類本身不能是型別參數，不過它可以包含範圍內的型別參數。

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a>基底類別

當*class_type*包含在*class_base*中時，它會指定所宣告之類別的直接基類。 如果類別宣告沒有*class_base*，或*class_base*只列出介面類別型，則會假設直接基底類別為 `object`。 類別會繼承其直接基類的成員，如[繼承](classes.md#inheritance)中所述。

在範例中
```csharp
class A {}

class B: A {}
```
類別 `A` 稱為 `B`的直接基類，而 `B` 則稱為衍生自 `A`。 由於 `A` 並未明確指定直接基類，因此會隱含地 `object`其直接基類。

針對已建立的類別類型，如果在泛型類別宣告中指定基類，則會針對基類宣告中的每個*type_parameter* ，以替代結構類型的對應*type_argument*來取得結構化類型的基底類別。 給定泛型類別宣告
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
`B<string,int[]>`的結構化型別 `G<int>` 的基類。

類別類型的直接基類至少必須與類別類型本身（[存取範圍網域](basic-concepts.md#accessibility-domains)）一樣可以存取。 例如，`public` 類別衍生自 `private` 或 `internal` 類別時，就會發生編譯時期錯誤。

類別類型的直接基類不得為下列任何類型： `System.Array`、`System.Delegate`、`System.MulticastDelegate`、`System.Enum`或 `System.ValueType`。 此外，泛型類別宣告不能使用 `System.Attribute` 做為直接或間接基類。

在決定 `B`的直接基類規格 `A` 的意義時，`B` 的直接基底類別會暫時假設為 `object`。 這可確保基類規格的意義無法以遞迴方式相依于本身。 範例：
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
發生錯誤，因為在基類規格中 `A<C.B>` 會將 `C` 的直接基類視為 `object`，因此（藉由[命名空間和型別名稱](basic-concepts.md#namespace-and-type-names)的規則） `C` 不會被視為具有成員 `B`。

類別類型的基類是直接基類和其基類。 換句話說，一組基類是直接基底類別關聯性的可轉移關閉。 參考上述範例，`B` 的基類 `A` 和 `object`。 在範例中
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
`D<int>` 的基類 `C<int[]>`、`B<IComparable<int[]>>`、`A`和 `object`。

除了類別 `object`以外，每個類別類型都只有一個直接基類。 `object` 類別沒有直接基類，而且是所有其他類別的最終基類。

當類別 `B` 衍生自類別 `A`時，`A` 相依于 `B`時，就會發生編譯時期錯誤。 類別會***直接相依于***其直接基類（如果有的話），而***直接取決於***立即嵌套的類別（如果有的話）。 根據這個定義，類別所相依的一組完整類別是***直接相依于***關聯性的自反和可轉移關閉。

範例
```csharp
class A: A {}
```
是錯誤的，因為類別相依于本身。 同樣地，此範例
```csharp
class A: B {}
class B: C {}
class C: A {}
```
發生錯誤，因為類別會迴圈相依于本身。 最後，範例
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
會導致編譯時期錯誤，因為 `A` 相依于 `B.C` （其直接的基類），這取決於 `B` （其立即封入類別），這會迴圈取決於 `A`。

請注意，類別不會相依于其內的類別。 在範例中
```csharp
class A
{
    class B: A {}
}
```
`B` 相依于 `A` （因為 `A` 同時是它的直接基類和其立即封入類別），但 `A` 不依賴 `B` （因為 `B` 不是基類，也不是 `A`的封入類別）。 因此，此範例是有效的。

不可能從 `sealed` 類別衍生。 在範例中
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
類別 `B` 發生錯誤，因為它會嘗試從 `sealed` 類別 `A`衍生。

#### <a name="interface-implementations"></a>介面實作

*Class_base*規格可能包括介面類別型的清單，在此情況下，類別會直接執行指定的介面類別型。 介面的執行會在[介面實現](interfaces.md#interface-implementations)中進一步討論。

### <a name="type-parameter-constraints"></a>類型參數條件約束

泛型型別和方法宣告可以選擇性地指定型別參數條件約束，方法是包含*type_parameter_constraints_clause*s。

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

每個*type_parameter_constraints_clause*都包含權杖 `where`，後面接著型別參數的名稱，後面接著冒號和該型別參數的條件約束清單。 每個類型參數最多隻能有一個 `where` 子句，而 `where` 子句可以依任何順序列出。 就像屬性存取子中的 `get` 和 `set` 權杖一樣，`where` token 不是關鍵字。

在 `where` 子句中提供的條件約束清單，可以依下列順序包含下列任何元件：單一主要條件約束、一個或多個次要條件約束，以及一個 `new()`的函式條件約束。

主要條件約束可以是類別類型或***參考類型條件約束***`class` 或 `struct`的實***數值型別條件***約束。 次要條件約束可以是*type_parameter*或*interface_type*。

參考型別條件約束會指定用於型別參數的型別引數必須是參考型別。 所有類別類型、介面類別型、委派類型、陣列類型，以及已知為參考型別的類型參數（如下面所定義）都符合這個條件約束。

實值型別條件約束會指定用於型別參數的型別引數，必須是不可為 null 的實值型別。 所有不可為 null 的結構類型、列舉類型，以及具有實數值型別條件約束的類型參數都滿足此條件約束。 請注意，雖然分類為實值型別，但可為 null 的型別（[可為 null](types.md#nullable-types)的類型）並不符合實數值型別條件約束。 具有實數值型別條件約束的類型參數，也不能有*constructor_constraint*。

指標類型永遠不允許是類型引數，而且不會被視為滿足參考型別或實數值型別條件約束。

如果條件約束是類別型別、介面型別或型別參數，則該型別會指定每個用於該型別參數的型別引數都必須支援的最小「基底型別」。 每當使用了結構化型別或泛型方法時，就會在編譯時期針對型別參數的條件約束檢查型別引數。 提供的型別引數必須滿足滿足條件[約束](types.md#satisfying-constraints)中所述的條件。

*Class_type*條件約束必須符合下列規則：

*  型別必須是類別型別。
*  類型不得 `sealed`。
*  類型不得為下列其中一種類型： `System.Array`、`System.Delegate`、`System.Enum`或 `System.ValueType`。
*  類型不得 `object`。 由於所有類型都是衍生自 `object`，因此如果允許的話，這類條件約束就不會有任何作用。
*  給定類型參數最多隻能有一個條件約束為類別類型。

指定為*interface_type*條件約束的類型必須符合下列規則：

*  型別必須是介面型別。
*  在指定的 `where` 子句中，類型不得指定一次以上。

不論是哪一種情況，條件約束都可以包含相關聯型別或方法宣告的任何型別參數，做為結構化型別的一部分，而且可能牽涉到宣告的型別。

任何指定為型別參數條件約束的類別或介面型別，都必須至少與所宣告的泛型型別或方法為可存取（[存取範圍條件約束](basic-concepts.md#accessibility-constraints)）。

指定為*type_parameter*條件約束的類型必須符合下列規則：

*  型別必須是型別參數。
*  在指定的 `where` 子句中，類型不得指定一次以上。

此外，型別參數的相依性圖形中也不能有迴圈，其中 dependency 是所定義的可轉移關聯性：

*  如果使用型別參數 `T` 做為型別參數的條件約束 `S` 則 `S`***取決於***`T`。
*  如果型別參數 `S` 取決於型別參數 `T` 而且 `T` 取決於型別參數 `U` 則 `S` 會***依賴***`U`。

基於此關聯性，類型參數要相依于本身（直接或間接）的編譯階段錯誤。

所有條件約束在相依型別參數之間必須一致。 如果型別參數 `S` 取決於型別參數 `T` 則：

*  `T` 不能有實數值型別條件約束。 否則，`T` 實際上是密封的，因此 `S` 會強制為與 `T`相同的類型，而不需要兩個型別參數。
*  如果 `S` 具有實數值型別條件約束，則 `T` 不能有*class_type*條件約束。
*  如果 `S` 具有*class_type*條件約束 `A` 而且 `T` 有*class_type*條件約束，則必須有從 `B` 到 `A` 或從 `B` 到 `B` 的隱含參考轉換的識別轉換或隱含參考轉換。`A`
*  如果 `S` 也相依于型別參數 `U` 而且 `U` 具有*class_type*條件約束 `A` 而 `T` 有*class_type*條件約束 `B` 則必須從 `A` 到 `B` 或從 `B` 到 `A`的隱含參考轉換，進行身分識別轉換或隱含參考轉換。

`S` 具有實數值型別條件約束，且 `T` 具有參考型別條件約束，則有效。 實際上，這會限制 `T` `System.Object`、`System.ValueType`、`System.Enum`和任何介面類別型的類型。

如果型別參數的 `where` 子句包含「函式」條件約束（其具有 `new()`的形式），則可以使用 `new` 運算子來建立型別的實例（[物件建立運算式](expressions.md#object-creation-expressions)）。 任何用於具有「函式」條件約束之型別參數的型別引數，都必須具有公用無參數的處理常式（這個此函式會針對任何實值型別隱含存在），或是具有實值型別條件約束或「函式條件約束[」的型](classes.md#type-parameter-constraints)別參數

以下是條件約束的範例：
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

下列範例是錯誤的，因為它會在型別參數的相依性圖形中造成迴圈：
```csharp
class Circular<S,T>
    where S: T
    where T: S                // Error, circularity in dependency graph
{
    ...
}
```

下列範例說明其他不正確情況：
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

類型參數 `T` 的***有效基類***定義如下：

*  如果 `T` 沒有主要條件約束或類型參數條件約束，則會 `object`其有效基類。
*  如果 `T` 具有實數值型別條件約束，則會 `System.ValueType`其有效基類。
*  如果 `T` 有*class_type*條件約束 `C` 但沒有*type_parameter*條件約束，則會 `C`其有效基類。
*  如果 `T` 沒有*class_type*條件約束，但有一或多個*type_parameter*條件約束，其有效基類就是其*type_parameter*條件約束之有效基類集中最包含的類型（[提升轉換運算子](conversions.md#lifted-conversion-operators)）。 一致性規則可確保這類最包含的類型存在。
*  如果 `T` 同時具備*class_type*條件約束和一或多個*type_parameter*條件約束，其有效基類是包含 `T` 之*class_type*條件約束和其*type_parameter*條件約束之有效基類的集合中最包含的類型（[提升轉換運算子](conversions.md#lifted-conversion-operators)）。 一致性規則可確保這類最包含的類型存在。
*  如果 `T` 有參考型別條件約束，但沒有*class_type*條件約束，則會 `object`有效的基類。

基於這些規則的目的，如果 T 具有*value_type*的條件約束 `V`，請改用*class_type*的最特定 `V` 基底類型。 這可能永遠不會發生在明確指定的條件約束中，但當覆寫方法宣告或介面方法的明確執行隱含繼承泛型方法的條件約束時，可能會發生這種情況。

這些規則會確保有效的基類一律為*class_type*。

`T` 類型參數的***有效介面集***定義如下：

*  如果 `T` 沒有*secondary_constraints*，其有效的介面集就是空的。
*  如果 `T` 有*interface_type*條件約束，但沒有*type_parameter*條件約束，其有效的介面集就是它的一組*interface_type*條件約束。
*  如果 `T` 沒有*interface_type*條件約束，但有*type_parameter*條件約束，其有效介面集就是其*type_parameter*條件約束之有效介面集的聯集。
*  如果 `T` 同時具備*interface_type*條件約束和*type_parameter*條件約束，其有效的介面集就是其*type_parameter*條件約束集合的聯集*interface_type*條件約束和有效介面集。

如果類型參數具有參考型別條件約束，或其有效基類不是 `object` 或 `System.ValueType`，則其為***參考型別***。

條件約束類型參數類型的值，可以用來存取限制式所隱含的實例成員。 在範例中
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
`IPrintable` 的方法可以直接在 `x` 上叫用，因為 `T` 受到限制，一律會執行 `IPrintable`。

### <a name="class-body"></a>類別主體

類別的*class_body*會定義該類別的成員。

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a>部分型別

類型宣告可以分割成多個***部分類型***宣告。 類型宣告會依照本節中的規則從其元件加以結構化，此時在程式的編譯時間和執行時間處理的其餘部分期間，會將它視為單一宣告。

*Class_declaration*， *struct_declaration*或*interface_declaration*如果包含 `partial` 修飾詞，則代表部分類型宣告。 `partial` 不是關鍵字，而且只會作為修飾詞（如果它緊接在 `class`的其中一個關鍵字之前）、`struct` 或 `interface` 在類型宣告中，或在方法宣告中的類型 `void` 之前。 在其他內容中，它可以用來做為一般識別碼。

部分類型宣告的每個部分都必須包含 `partial` 修飾詞。 它必須具有相同的名稱，而且必須在與其他元件相同的命名空間或類型宣告中宣告。 `partial` 修飾詞表示類型宣告的其他部分可能存在於其他地方，但這類額外的部分不是必要條件。它適用于具有單一宣告的類型，以包含 `partial` 修飾詞。

部分類型的所有部分都必須一起編譯，使元件可以在編譯時期合併成單一類型宣告。 部分類型特別不允許延伸已編譯的類型。

您可以使用 `partial` 修飾詞，在多個元件中宣告嵌套的類型。 一般來說，包含類型也會使用 `partial` 來宣告，而巢狀型別的每個部分都會在包含類型的不同部分中宣告。

委派或列舉宣告上不允許 `partial` 修飾詞。

### <a name="attributes"></a>屬性

部分類型的屬性是藉由以未指定的順序結合每個元件的屬性來決定。 如果屬性放在多個部分，它就相當於在類型上多次指定屬性。 例如，這兩個部分：

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
相當於如下的宣告：
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

類型參數上的屬性會以類似的方式結合。

### <a name="modifiers"></a>修飾詞

當部分類型宣告包含協助工具規格（`public`、`protected`、`internal`和 `private` 修飾詞）時，必須同意包含協助工具規格的所有其他部分。 如果部分類型中沒有任何部分包含協助工具規格，則會提供適當的預設存取範圍（已宣告的[存取](basic-concepts.md#declared-accessibility)範圍）給該類型。

如果巢狀型別的一或多個部分宣告包含 `new` 修飾詞，則如果嵌套的類型隱藏繼承的成員（[透過繼承隱藏](basic-concepts.md#hiding-through-inheritance)），則不會回報警告。

如果類別的一個或多個部分宣告包含 `abstract` 修飾詞，則會將類別視為抽象（[抽象類別](classes.md#abstract-classes)）。 否則，類別會被視為非抽象。

如果類別的一個或多個部分宣告包含 `sealed` 修飾詞，則會將類別視為 sealed （[密封類別](classes.md#sealed-classes)）。 否則，會將類別視為未密封。

請注意，類別不能同時為 abstract 和 sealed。

`unsafe`在部分類型宣告上使用修飾詞時，只有該特定部分會被視為不安全的內容（[不安全的內容](unsafe-code.md#unsafe-contexts)）。

### <a name="type-parameters-and-constraints"></a>類型參數和條件約束

如果泛型型別宣告于多個部分，則每個部分都必須陳述型別參數。 每個元件都必須具有相同數目的類型參數，而且每個類型參數的名稱相同。

當部分泛型型別宣告包含條件約束（`where` 子句）時，條件約束必須同意包含條件約束的所有其他部分。 具體而言，每個包含條件約束的元件都必須具有相同類型參數集的條件約束，而針對每個類型參數，主要、次要和函式條件約束的集合必須相等。 如果兩組條件約束包含相同的成員，則兩者都是相等的。 如果部分泛型型別沒有任何部分指定類型參數條件約束，則會將類型參數視為不受限制。

範例
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
是正確的，因為包含條件約束（前兩個）的元件會分別針對同一組類型參數，有效地指定相同的主要、次要和函式條件約束集合。

### <a name="base-class"></a>基底類別

當部分類別宣告包含基類規格時，必須同意包含基類規格的所有其他部分。 如果部分類別中沒有部分包含基類規格，基底類別會變成 `System.Object` （[基類](classes.md#base-classes)）。

### <a name="base-interfaces"></a>基底介面

在多個元件中宣告之類型的基底介面集合，是在每個元件上所指定之基底介面的聯集。 特定基底介面只能在每個元件上命名一次，但允許多個部分命名為相同的基底介面。 任何給定基底介面的成員都只能有一個執行。

在範例中
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
類別 `C` 的基底介面集是 `IA`、`IB`和 `IC`。

一般來說，每個部分都會提供在該元件上宣告之介面的執行;不過，這不是必要條件。 元件可以針對在不同元件上宣告的介面提供執行：
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

除了部分方法（[部分方法](classes.md#partial-methods)）之外，在多個部分宣告之類型的成員集合，只是在每個部分宣告的成員集合的聯集。 類型宣告的所有部分的主體會共用相同的宣告空間（宣告[），](basic-concepts.md#declarations)而每個成員的範圍（[範圍](basic-concepts.md#scopes)）都會延伸至所有元件的主體。 任何成員的存取範圍定義域一律包含封入類型的所有部分;在一個元件中宣告的 `private` 成員可從另一個部分自由存取。 在類型的多個部分中宣告相同的成員是編譯時期錯誤，除非該成員是具有 `partial` 修飾詞的類型。

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

類型中的成員順序很少對C#程式碼很重要，但在與其他語言和環境互動時可能會很重要。 在這些情況下，在多個元件中宣告的類型內的成員順序未定義。

### <a name="partial-methods"></a>部分方法

部分方法可以在類型宣告的某個部分中定義，並在另一個元件中執行。 執行是選擇性的;如果沒有部分執行部分方法，部分方法宣告和其所有呼叫都會從元件組合所產生的類型宣告中移除。

部分方法不能定義存取修飾詞，而是隱含 `private`。 其傳回型別必須 `void`，而且其參數不能有 `out` 修飾詞。 只有當識別碼 `partial` 在 `void` 類型之前出現時，才會被辨識為方法宣告中的特殊關鍵字。否則，它可以用來做為一般識別碼。 部分方法無法明確地執行介面方法。

部分方法宣告的類型有兩種：如果方法宣告的主體是分號，則宣告會被視為***定義部分方法***宣告。 如果主體是以*區塊*的形式提供，則宣告會被視為實作為***部分方法***宣告。 在類型宣告的各個部分中，只能有一個定義具有指定簽章的部分方法宣告，而且只能有一個具有指定簽章的執行部分方法宣告。 如果指定了執行部分方法宣告，則必須有對應的定義部分方法宣告，而且宣告必須符合下列內容中所指定的：

* 宣告必須具有相同的修飾詞（雖然不一定是相同的順序）、方法名稱、型別參數數目和參數數目。
* 宣告中的對應參數必須具有相同的修飾詞（雖然不一定是相同的順序）和相同的類型（類型參數名稱的模數差異）。
* 宣告中的對應類型參數必須具有相同的條件約束（類型參數名稱的模數差異）。

執行部分方法宣告可以與對應的定義部分方法宣告出現在相同的部分。

只有定義部分方法會參與多載解析。 因此，不論是否指定了實做宣告，調用運算式都可以解析為部分方法的調用。 因為部分方法一律會傳回 `void`，所以這類調用運算式一律會是運算式語句。 此外，由於部分方法會隱含 `private`，因此這類語句一律會出現在宣告部分方法之類型宣告的其中一個部分內。

如果部分類型宣告中沒有任何部分包含給定部分方法的實作為宣告，則叫用它的任何運算式語句都會直接從結合的類型宣告中移除。 因此，叫用運算式（包括任何組成運算式）在執行時間不會有任何作用。 部分方法本身也會移除，且不會是合併類型宣告的成員。

如果指定的部分方法有執行宣告，則會保留部分方法的調用。 部分方法會增加與執行部分方法宣告類似的方法宣告，但不包括下列各項：

* 不包含 `partial` 修飾詞
* 產生的方法宣告中的屬性是定義的結合屬性和以未指定循序執行的部分方法宣告。 不會移除重複專案。
* 產生的方法宣告之參數上的屬性，是以未指定順序定義和執行部分方法宣告之對應參數的結合屬性。 不會移除重複專案。

如果指定了部分方法 M 的定義宣告，但不是執行宣告，則適用下列限制：

* 建立方法（[委派建立運算式](expressions.md#delegate-creation-expressions)）的委派時，會發生編譯時期錯誤。
* 在轉換成運算式樹狀架構類型的匿名函式（[對運算式樹狀結構類型的匿名函數轉換評估](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)）內，會發生編譯時期 `M` 錯誤。
* 出現在 `M` 調用中的運算式不會影響明確指派狀態（[明確指派](variables.md#definite-assignment)），這可能會導致編譯時期錯誤。
* `M` 不可以是應用程式的進入點（[應用程式啟動](basic-concepts.md#application-startup)）。

部分方法可讓類型宣告的其中一個部分自訂另一個部分（例如，由工具產生的）的行為。 請考慮下列部分類別宣告：
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

如果編譯此類別時沒有任何其他元件，則會移除定義的部分方法宣告及其調用，而產生的合併類別宣告將相當於下列內容：
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

假設有另一個部分是提供給部分方法的實作為宣告的：
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

然後產生的合併類別宣告將相當於下列內容：
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

### <a name="name-binding"></a>名稱系結

雖然可延伸類型的每個部分都必須在相同的命名空間內宣告，但這些元件通常會在不同的命名空間宣告中寫入。 因此，每個元件可能會有不同的 `using` 指示詞（[Using](namespaces.md#using-directives)指示詞）。 在某個部分中解讀簡單名稱（[類型推斷](expressions.md#type-inference)）時，只會考慮包含該部分之命名空間宣告的 `using` 指示詞。 這可能會導致相同的識別碼在不同的部分具有不同的意義：
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

類別的成員是由其*class_member_declaration*所引進的成員，以及繼承自直接基類的成員所組成。

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

類別類型的成員會分成下列類別：

*  常數，表示與類別（[常數](classes.md#constants)）相關聯的常數值。
*  欄位，也就是類別（[欄位](classes.md#fields)）的變數。
*  方法，其會執行類別（[方法](classes.md#methods)）可執行檔計算和動作。
*  屬性，定義已命名的特性，以及與讀取和寫入這些特性（[屬性](classes.md#properties)）相關聯的動作。
*  定義可由類別（[事件](classes.md#events)）產生之通知的事件。
*  索引子，允許以相同的方式（語法）將類別的實例編制索引為數組（[索引子](classes.md#indexers)）。
*  運算子，定義可以套用至類別（[運算子](classes.md#operators)）實例的運算式運算子。
*  實例函式，其會執行初始化類別的實例所需的動作（[實例構造](classes.md#instance-constructors)函式）
*  析構函數，其會執行要在類別的實例永久捨棄（[析構函數](classes.md#destructors)）之前執行的動作。
*  靜態的函式，其會執行初始化類別本身所需的動作（[靜態](classes.md#static-constructors)的函式）。
*  類型，表示類別的本機類型（[巢狀型別](classes.md#nested-types)）。

可以包含可執行程式碼的成員統稱為類別類型的函式*成員*。 類別類型的函式成員是該類別類型的方法、屬性、事件、索引子、運算子、實例的函式、析構函式和靜態函式。

*Class_declaration*會建立新的宣告[空間（宣告](basic-concepts.md#declarations)），而*class_declaration*立即包含的*class_member_declaration*會將新的成員引進此宣告空間。 下列規則適用于*class_member_declaration*s：

*  實例的函式、析構函式和靜態的函式必須與直接封入類別的名稱相同。 所有其他成員的名稱都必須與立即封入類別的名稱不同。
*  常數、欄位、屬性、事件或類型的名稱，必須與在相同類別中宣告的所有其他成員名稱不同。
*  方法的名稱必須與在相同類別中宣告之所有其他非方法的名稱不同。 此外，方法的簽章（簽章[和](basic-concepts.md#signatures-and-overloading)多載）必須與相同類別中宣告之所有其他方法的簽章不同，而且在相同類別中宣告的兩個方法，不能有不同于 `ref` 和 `out`的簽章。
*  實例函式的簽章必須不同于相同類別中宣告之所有其他實例的簽章，而且在相同類別中宣告的兩個函式不能有不同于 `ref` 和 `out`的簽章。
*  索引子的簽章必須與相同類別中宣告之所有其他索引子的簽章不同。
*  運算子的簽章必須與在相同類別中宣告之所有其他運算子的簽章不同。

類別類型的繼承成員（[繼承](classes.md#inheritance)）不是類別之宣告空間的一部分。 因此，衍生類別可以宣告與繼承成員具有相同名稱或簽章的成員（這實際上會隱藏繼承的成員）。

### <a name="the-instance-type"></a>實例類型

每個類別宣告都有相關聯的系結類型（系結[和未](types.md#bound-and-unbound-types)系結類型），也就是***實例類型***。 針對泛型類別宣告，實例類型的形成方式是從類型宣告建立結構化類型（[結構化類型](types.md#constructed-types)），其中每個提供的類型引數都是對應的類型參數。 由於實例型別會使用型別參數，因此只能在型別參數在範圍內時使用。也就是在類別宣告內。 實例類型是在類別宣告內撰寫之程式碼的 `this` 類型。 針對非泛型類別，實例類型只是宣告的類別。 以下顯示數個類別宣告以及其實例類型： 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a>結構化類型的成員

結構化類型的非繼承成員是藉由以成員宣告中的每個*type_parameter*的對應*type_argument*來取代。 替代程式是以類型宣告的語義意義為基礎，而不是單純的文字替代。

例如，假設泛型類別宣告
```csharp
class Gen<T,U>
{
    public T[,] a;
    public void G(int i, T t, Gen<U,T> gt) {...}
    public U Prop { get {...} set {...} }
    public int H(double d) {...}
}
```
`Gen<int[],IComparable<string>>` 的結構化類型具有下列成員：
```csharp
public int[,][] a;
public void G(int i, int[] t, Gen<IComparable<string>,int[]> gt) {...}
public IComparable<string> Prop { get {...} set {...} }
public int H(double d) {...}
```

泛型類別宣告中的成員 `a` 類型 `Gen` 為 "二維陣列 of `T`"，因此上述結構類型中的成員 `a` 類型為 "二維陣列（一維陣列 of `int`"）或 `int[,][]`。

在實例函式成員內，`this` 的類型是包含宣告的實例類型（[實例類型](classes.md#the-instance-type)）。

泛型類別的所有成員都可以直接或做為結構化型別的一部分，使用任何封入類別中的型別參數。 在執行時間使用特定封閉結構類型（[開放式和封閉式類型](types.md#open-and-closed-types)）時，會將類型參數的每個使用，取代為提供給結構化類型的實際類型引數。 例如：
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

類別會***繼承***其直接基類類型的成員。 繼承表示，類別會隱含包含其直接基類類型的所有成員，但不包括實例的函式、函式和基類的靜態方法。 繼承的一些重要層面如下：

*  繼承是可轉移的。 如果 `C` 衍生自 `B`，而 `B` 衍生自 `A`，則 `C` 會繼承 `B` 中宣告的成員，以及 `A`中宣告的成員。
*  衍生類別會擴充其直接基類。 衍生類別可以在其繼承的成員中新增新的成員，但無法移除所繼承成員的定義。
*  實例的函式、析構函式和靜態的函式不會繼承，但是所有其他成員都是，而不論其宣告的存取範圍為何（[成員存取權](basic-concepts.md#member-access)）。 不過，視其宣告的存取範圍而定，繼承的成員可能無法在衍生類別中存取。
*  衍生類別可以藉由宣告具有相同名稱或簽章的新成員，***隱藏***（[透過繼承](basic-concepts.md#hiding-through-inheritance)）繼承的成員。 不過，請注意，隱藏繼承的成員並不會移除該成員，而只是讓該成員直接透過衍生類別來存取。
*  類別的實例包含一組在類別及其基類中宣告的所有實例欄位，以及從衍生類別類型到其任何基類類型的隱含轉換（[隱含參考轉換](conversions.md#implicit-reference-conversions)）。 因此，某些衍生類別之實例的參考可視為其任何基類之實例的參考。
*  類別可以宣告虛擬方法、屬性和索引子，而衍生類別可以覆寫這些函式成員的執行。 這可讓類別展示多型行為，其中由函式成員調用所執行的動作會根據叫用該函式成員之實例的執行時間類型而有所不同。

已結構化類別類型的繼承成員是直接基底類別類型（[基類](classes.md#base-classes)）的成員，它是藉由以*class_base*規格中對應類型參數的每個出現專案的類型引數取代所找到的。 接著，這些成員會藉由替代成員宣告中的每個*type_parameter* （對應*type_argument* *class_base*規格）進行轉換。

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

在上述範例中，`D<int>` 的結構化型別有一個非繼承的成員 `public int G(string s)` 藉由替代型別參數 `T`的型別引數 `int` 取得。 `D<int>` 也有類別宣告 `B`繼承的成員。 這個繼承的成員是藉由在基類規格 `B<T[]>`中替換 `T` 的 `int`，來判斷 `D<int>` 的 `B<int[]>` 的基類型別。 然後，做為 `B`的型別引數，`int[]` 會在 `public U F(long index)`中用來取代 `U`，產生繼承的成員 `public int[] F(long index)`。

### <a name="the-new-modifier"></a>New 修飾詞

允許*class_member_declaration*宣告與繼承成員具有相同名稱或簽章的成員。 發生這種情況時，衍生的類別成員會被視為***隱藏***基類成員。 隱藏繼承的成員不會被視為錯誤，但會導致編譯器發出警告。 若要隱藏警告，衍生類別成員的宣告可以包含 `new` 修飾詞，以指出衍生的成員要用來隱藏基底成員。 本主題會在[透過繼承隱藏](basic-concepts.md#hiding-through-inheritance)中進一步討論。

如果 `new` 修飾詞包含在不會隱藏繼承成員的宣告中，就會發出該效果的警告。 移除 `new` 修飾詞會隱藏這個警告。

### <a name="access-modifiers"></a>存取修飾詞

*Class_member_declaration*可以有五種可能的宣告存取範圍（宣告的[存取](basic-concepts.md#declared-accessibility)範圍）其中之一： `public`、`protected internal`、`protected`、`internal`或 `private`。 除了 `protected internal` 組合之外，指定一個以上的存取修飾詞是編譯時期錯誤。 當*class_member_declaration*不包含任何存取修飾詞時，就會假設 `private`。

### <a name="constituent-types"></a>組成類型

成員宣告中使用的類型稱為該成員的構成類型。 可能的構成類型為常數、欄位、屬性、事件或索引子的類型、方法或運算子的傳回類型，以及方法、索引子、運算子或實例的參數類型。 成員的構成類型必須至少與該成員本身一樣可以存取（[存取範圍條件約束](basic-concepts.md#accessibility-constraints)）。

### <a name="static-and-instance-members"></a>靜態和實例成員

類別的成員可能是***靜態成員***或***實例成員***。 一般來說，將靜態成員視為屬於物件的類別型別和實例成員（類別型別的實例）會很有説明。

當欄位、方法、屬性、事件、運算子或函式宣告包含 `static` 修飾詞時，它會宣告靜態成員。 此外，常數或型別宣告會隱含宣告靜態成員。 靜態成員具有下列特性：

*  當靜態成員 `M` 在表單 `E.M`的*member_access* （[成員存取](expressions.md#member-access)）中被參考時，`E` 必須代表包含 `M`的類型。 `E` 表示實例時，就會發生編譯時期錯誤。
*  靜態欄位可識別指定之封閉式類別類型的所有實例所要共用的一個儲存位置。 不論指定之封閉式類別類型的多少個實例已建立，只有一個靜態欄位的複本。
*  靜態函式成員（方法、屬性、事件、運算子或函式）不會在特定的實例上運作，而且在這類函式成員中參考 `this` 時，會發生編譯時期錯誤。

當欄位、方法、屬性、事件、索引子、函數或析構函式宣告不包含 `static` 修飾詞時，它會宣告實例成員。 （實例成員有時稱為非靜態成員）。實例成員具有下列特性：

*  當實例成員 `M` 在表單 `E.M`的*member_access* （[成員存取](expressions.md#member-access)）中被參考時，`E` 必須代表包含 `M`之類型的實例。 這是 `E` 用來表示類型的系結時錯誤。
*  類別的每個實例都包含一組個別的類別的實例欄位。
*  實例函式成員（方法、屬性、索引子、實例處理常式或析構函數）會在指定的類別實例上運作，而且這個實例可以當做 `this` （[這項存取](expressions.md#this-access)）來存取。

下列範例說明用來存取靜態和實例成員的規則：
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

`F` 的方法顯示，在實例函式成員中，可以使用*simple_name* （[簡單名稱](expressions.md#simple-names)）來存取實例成員和靜態成員。 `G` 方法顯示，在靜態函式成員中，這是編譯時期錯誤，透過*simple_name*存取實例成員。 `Main` 方法會顯示在*member_access* （[成員存取](expressions.md#member-access)）中，實例成員必須透過實例存取，而且靜態成員必須透過類型存取。

### <a name="nested-types"></a>巢狀型別

在類別或結構宣告內宣告的型別稱為「***嵌套型***別」。 在編譯單位或命名空間中宣告的類型稱為***非巢狀型別***。

在範例中
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
類別 `B` 是一個嵌套型別，因為它是在類別 `A`內宣告，而類別 `A` 是非嵌套型別，因為它是在編譯單位內宣告的。

#### <a name="fully-qualified-name"></a>完整名稱

巢狀型別的完整限定名稱（[完整](basic-concepts.md#fully-qualified-names)名稱）為 `S.N`，其中 `S` 是宣告類型 `N` 之類型的完整名稱。

#### <a name="declared-accessibility"></a>已宣告存取範圍

非巢狀型別可以有 `public` 或 `internal` 宣告的存取範圍，而且預設會 `internal` 宣告存取範圍。 巢狀型別也可以有這些形式的宣告存取範圍，再加上一或多個其他形式的宣告存取範圍，視包含的類型是否為類別或結構而定：

*  在類別中宣告的巢狀型別可以有五種形式的宣告存取範圍（`public`、`protected internal`、`protected`、`internal`或 `private`），而且就像其他類別成員一樣，預設為 `private` 宣告的存取範圍。
*  在結構中宣告的巢狀型別可以有三種形式的宣告存取範圍（`public`、`internal`或 `private`），而且就像其他結構成員一樣，預設為 `private` 宣告的存取範圍。

範例
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
宣告私用的嵌套類別 `Node`。

#### <a name="hiding"></a>起來

巢狀型別可能會隱藏（[名稱隱藏](basic-concepts.md#name-hiding)）基底成員。 在 nested 類型宣告上允許使用 `new` 修飾詞，因此可以明確表示隱藏。 範例
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
顯示隱藏 `Base`中所定義之方法 `M` 的嵌套類別 `M`。

#### <a name="this-access"></a>此存取權

嵌套型別及其包含型別對於*this_access* （[此存取權](expressions.md#this-access)）沒有特殊關聯性。 具體而言，巢狀型別中的 `this` 不能用來參考包含類型的實例成員。 當巢狀型別需要存取其包含類型的實例成員時，可以提供包含類型實例的 `this` 做為巢狀型別的函式引數，藉以提供存取權。 下列範例
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
顯示這項技術。 實例 `C` 會建立 `Nested` 的實例，並將它自己的 `this` 傳遞至 `Nested`的函式，以便提供對 `C`實例成員的後續存取。

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a>存取包含類型的私用和受保護成員

巢狀型別可以存取其包含類型可存取的所有成員，包括具有 `private` 和 `protected` 宣告存取範圍之包含類型的成員。 範例
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
顯示包含 `Nested`之嵌套類別 `C` 的類別。 在 `Nested`中，方法 `G` 會呼叫 `C`中定義的靜態方法 `F`，而 `F` 具有私用的可存取性。

嵌套型別也可以存取其包含型別之基底型別中所定義的受保護成員。 在範例中
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
此嵌套類別 `Derived.Nested` 透過 `Derived`的實例呼叫，來存取受保護的方法 `F` `Derived`的基類中定義的 `Base`。

#### <a name="nested-types-in-generic-classes"></a>泛型類別中的巢狀型別

泛型類別宣告可以包含嵌套的類型宣告。 封入類別的型別參數可以在巢狀型別中使用。 巢狀型別宣告可以包含僅適用于巢狀型別的其他類型參數。

泛型類別宣告中包含的每個類型宣告，都是隱含的泛型型別宣告。 在泛型型別中撰寫類型的參考時，包含結構化型別（包括它的型別引數）必須命名為。 不過，在外部類別中，可以使用嵌套型別，而不需要限定。在建立嵌套的類型時，可以隱含地使用外部類別的實例類型。 下列範例顯示三個不同的正確方式，以參考從 `Inner`建立的結構化型別。前兩個是相等的：
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

雖然這是不正確的程式設計樣式，但嵌套型別中的型別參數可以隱藏在外部型別中宣告的成員或型別參數：
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

為了加速基礎C#執行時間的執行，針對屬於屬性、事件或索引子的每個來源成員宣告，此執行必須根據成員宣告的類型、其名稱和類型來保留兩個方法簽章。 這是一個編譯時期錯誤，讓程式宣告其簽章符合其中一個保留簽章的成員，即使基礎執行時間執行不會使用這些保留區。

保留的名稱不會引進宣告，因此不會參與成員查閱。 不過，宣告相關聯的保留方法簽章會參與繼承（[繼承](classes.md#inheritance)），而且可以使用 `new` 修飾詞（[新的修飾](classes.md#the-new-modifier)詞）加以隱藏。

這些名稱的保留有三個用途：

*  允許基礎執行程式使用一般識別碼作為方法名稱，以取得或設定C#語言功能的存取權。
*  若要允許其他語言互通，請使用一般識別碼做為 get 或 set 存取C#語言功能的方法名稱。
*  為了協助確保另一個符合的編譯器所接受的來源可被其他人接受，方法是讓保留成員名稱的細節在C#所有的執行中保持一致。

「析構函數」[（析構函式）的](classes.md#destructors)宣告也會保留簽章（[針對析構函數保留的成員名稱](classes.md#member-names-reserved-for-destructors)）。

#### <a name="member-names-reserved-for-properties"></a>保留給屬性的成員名稱

若是類型`P` 的屬性（[屬性](classes.md#properties)），則會保留下列簽`T`章：

```csharp
T get_P();
void set_P(T value);
```

這兩個簽章都會保留，即使屬性是唯讀或僅限寫入。

在範例中
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
類別 `A` 定義 `P`的唯讀屬性，因此會保留 `get_P` 和 `set_P` 方法的簽章。 類別 `B` 衍生自 `A`，並隱藏這兩個保留的簽章。 此範例會產生輸出：
```console
123
123
456
```

#### <a name="member-names-reserved-for-events"></a>保留給事件的成員名稱

若為委派`E`類型 [的事件（](classes.md#events)事件`T`），則會保留下列簽章：
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a>保留給索引子的成員名稱

針對類型 `T` 的索引子（[索引子](classes.md#indexers)）與參數清單 `L`，會保留下列簽章：
```csharp
T get_Item(L);
void set_Item(L, T value);
```

這兩個簽章都會保留，即使索引子是唯讀或僅限寫入。

此外，`Item` 的成員名稱是保留的。

#### <a name="member-names-reserved-for-destructors"></a>保留給析構函數的成員名稱

對於包含「析構函數」（[析構](classes.md#destructors)函式）的類別，會保留下列簽章：
```csharp
void Finalize();
```

## <a name="constants"></a>常數

***常數***是代表常數值的類別成員：可以在編譯時期計算的值。 *Constant_declaration*導入了一或多個指定類型的常數。

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

*Constant_declaration*可能包括一組*屬性*（attribute）、`new` 修飾[詞（](attributes.md)新的[修飾](classes.md#the-new-modifier)詞），以及四個存取修飾詞（[存取](classes.md#access-modifiers)修飾詞）的有效組合。 屬性和修飾詞適用于*constant_declaration*所宣告的所有成員。 即使常數視為靜態成員， *constant_declaration*都不需要也不允許 `static` 修飾詞。 在常數宣告中多次出現相同的修飾詞時，就會發生錯誤。

*Constant_declaration*的*類型*會指定宣告引進的成員類型。 類型後面會接著*constant_declarator*的清單，其中每個都會引進新的成員。 *Constant_declarator*包含命名成員的*識別碼*，後面接著 "`=`" token，接著是提供成員值的*constant_expression* （[常數運算式](expressions.md#constant-expressions)）。

在常數宣告中指定的*類型*必須是 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`、`decimal`、`bool`、`string`、 *enum_type*或*reference_type*。 每個*constant_expression*都必須產生目標型別的值，或可以透過隱含轉換（[隱含](conversions.md#implicit-conversions)轉換）轉換成目標型別的類型。

常數的*類型*至少必須與常數本身一樣可以存取（[存取範圍條件約束](basic-concepts.md#accessibility-constraints)）。

您可使用*simple_name* （[簡單名稱](expressions.md#simple-names)）或*member_access* （[成員存取](expressions.md#member-access)），在運算式中取得常數的值。

常數本身可以參與*constant_expression*。 因此，常數可以用於任何需要*constant_expression*的結構中。 這類結構的範例包括 `case` 標籤、`goto case` 語句、`enum` 成員宣告、屬性和其他常數宣告。

如[常數運算式](expressions.md#constant-expressions)中所述， *constant_expression*是可以在編譯時期完整評估的運算式。 因為建立非 null 值的唯一方法是要套用 `new` 運算子，而且在*constant_expression*中不允許 `new` 運算子，所以 reference_type `string`*以外的常數的唯一*可能值為 `null`，而不是 `string` 的*reference_type* 。

如果需要常數值的符號名稱，但常數宣告中不允許該值的型別，或是在編譯時期無法以*constant_expression*計算值，則可以改用 `readonly` 欄位（[Readonly 欄位](classes.md#readonly-fields)）。

宣告多個常數的常數宣告相當於多個具有相同屬性、修飾詞和類型之單一常數的宣告。 例如
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

只要相依性不是迴圈本質，常數就可以相依于同一個程式內的其他常數。 編譯器會自動排文，以適當的順序來評估常數宣告。 在範例中
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
編譯器會先評估 `A.Y`，然後評估 `B.Z`，最後再評估 `A.X`，產生 `10`、`11`和 `12`的值。 常數宣告可能取決於其他程式的常數，但這類相依性只能以單向方式進行。 參考上述範例，如果 `A` 和 `B` 是在不同的程式中宣告，`A.X` 可能會相依于 `B.Z`，但 `B.Z` 之後可能不會同時相依于 `A.Y`。

## <a name="fields"></a>欄位

「***欄位***」（field）是代表與物件或類別相關聯之變數的成員。 *Field_declaration*會引進一或多個指定類型的欄位。

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

*Field_declaration*可能包括一組*屬性*（attribute）、`new` 修飾[詞（](attributes.md)新的[修飾](classes.md#the-new-modifier)詞）、四個存取修飾詞（[存取](classes.md#access-modifiers)修飾詞）的有效組合，以及 `static` 修飾詞（[靜態和實例欄位](classes.md#static-and-instance-fields)）。 此外， *field_declaration*可能`readonly`包括修飾詞（[Readonly 欄位](classes.md#readonly-fields)`volatile`）或修飾詞（[Volatile 欄位](classes.md#volatile-fields)），但不能同時包含兩者。 屬性和修飾詞適用于*field_declaration*所宣告的所有成員。 在欄位宣告中多次出現相同的修飾詞是錯誤的。

*Field_declaration*的*類型*會指定宣告引進的成員類型。 類型後面會接著*variable_declarator*的清單，其中每個都會引進新的成員。 *Variable_declarator*由*識別*該成員的識別碼（選擇性地後面接著 "`=`" token）和*variable_initializer* （[變數初始化運算式](classes.md#variable-initializers)）所組成，以提供該成員的初始值。

欄位的*類型*至少必須與欄位本身（[存取範圍條件約束](basic-concepts.md#accessibility-constraints)）一樣可以存取。

欄位的值是在使用*simple_name* （[簡單名稱](expressions.md#simple-names)）或*member_access* （[成員存取](expressions.md#member-access)）的運算式中取得。 非唯讀欄位的值是使用*指派*（[指派運算子](expressions.md#assignment-operators)）進行修改。 您可以使用後置遞增和遞減運算子（後置[遞增和遞減運算子](expressions.md#postfix-increment-and-decrement-operators)）和前置遞增和遞減運算子（[前置遞增](expressions.md#prefix-increment-and-decrement-operators)和遞減運算子）來取得和修改非唯讀欄位的值。

宣告多個欄位的欄位宣告相當於具有相同屬性、修飾詞和類型之單一欄位的多個宣告。 例如
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

### <a name="static-and-instance-fields"></a>靜態和實例欄位

當欄位宣告包含 `static` 修飾詞時，宣告所引進的欄位就是***靜態欄位***。 當沒有 `static` 的修飾詞時，宣告所引進的欄位就是***實例欄位***。 靜態欄位和實例欄位是支援的數種變數（[變數](variables.md)） C#中的兩種，有時分別稱為***靜態變數***和***執行個體變數***。

靜態欄位不是特定實例的一部分;相反地，它會在封閉式類型（[開放式和封閉式類型](types.md#open-and-closed-types)）的所有實例之間共用。 無論已關閉的類別類型有多少個實例，相關聯的應用程式域只會有一個靜態欄位複本。

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

實例欄位屬於實例。 具體而言，類別的每個實例都包含該類別的一組個別的實例欄位。

在表單 `E.M`的*member_access* （[成員存取](expressions.md#member-access)）中參考欄位時，如果 `M` 是靜態欄位，`E` 必須代表包含 `M`的類型，而且如果 `M` 是實例欄位，則 E 必須代表包含 `M`之類型的實例。

靜態和實例成員之間的差異會在[靜態和實例成員](classes.md#static-and-instance-members)中進一步討論。

### <a name="readonly-fields"></a>唯讀欄位

當*field_declaration*包含 `readonly` 修飾詞時，宣告所引進的欄位就是***唯讀欄位***。 唯讀欄位的直接指派只能做為該宣告的一部分，或在相同類別中的實例的函式或靜態的函數中。 （您可以在這些內容中多次指派 readonly 欄位）。具體而言，只有在下列內容中才允許直接指派 `readonly` 欄位：

*  在引進欄位的*variable_declarator*中（藉由在宣告中包含*variable_initializer* ）。
*  針對實例欄位，在包含欄位宣告的類別的實例函式中。針對靜態欄位，在包含欄位宣告之類別的靜態函式中。 這些也是唯一有效的內容，可將 `readonly` 欄位當做 `out` 或 `ref` 參數傳遞。

嘗試指派給 `readonly` 欄位，或在任何其他內容中以 `out` 或 `ref` 參數的形式傳遞，會產生編譯時期錯誤。

#### <a name="using-static-readonly-fields-for-constants"></a>使用常數的靜態唯讀欄位

當需要常數值的符號名稱，但在 `const` 宣告中不允許值的類型，或在編譯時期無法計算值時，`static readonly` 欄位會很有用。 在範例中
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
`Black`、`White`、`Red`、`Green`和 `Blue` 成員無法宣告為 `const` 成員，因為它們的值無法在編譯時期計算。 不過，將它們宣告 `static readonly` 反而會有相同的效果。

#### <a name="versioning-of-constants-and-static-readonly-fields"></a>常數和靜態唯讀欄位的版本控制

常數和 readonly 欄位具有不同的二進位版本控制語義。 當運算式參考常數時，常數的值會在編譯時期取得，但當運算式參考 readonly 欄位時，在執行時間之前不會取得欄位的值。 假設有一個應用程式是由兩個不同的程式所組成：
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

`Program1` 和 `Program2` 命名空間代表分別編譯的兩個程式。 因為 `Program1.Utils.X` 宣告為靜態唯讀欄位，所以 `Console.WriteLine` 語句的輸出值在編譯時期未知，而是在執行時間取得。 因此，如果 `X` 的值已變更，且 `Program1` 重新編譯，則 `Console.WriteLine` 語句會輸出新的值，即使 `Program2` 並未重新編譯也一樣。 不過，`X` 是常數，`X` 的值會在編譯 `Program2` 時取得，而且在重新編譯 `Program2` 之前，`Program1` 中的變更不會受到影響。

### <a name="volatile-fields"></a>變動欄位

當*field_declaration*包含 `volatile` 修飾詞時，該宣告引進的欄位就是***變動性欄位***。

對於非動態欄位而言，重新排列指令的優化技術可能會導致非預期且無法預期的結果，而不需要同步處理（如*lock_statement* （[lock 語句](statements.md#the-lock-statement)）所提供）的多執行緒程式存取欄位。 這些優化可以由編譯器、執行時間系統或硬體執行。 對於變動性欄位，這類重新排列優化會受到限制：

*  讀取 volatile 欄位稱為「***變動性讀取***」。 「變動性讀取」具有「取得語義」;也就是說，在指令序列之後發生的記憶體參照之前，一定會發生這種情況。
*  Volatile 欄位的寫入稱為「***變動性寫入***」。 Volatile 寫入具有「發行語義」;也就是說，在指令序列中寫入指令之前的任何記憶體參考之後，一定會發生這種情況。

這些限制可確保所有的執行緒將會觀察任何其他執行緒執行的 volatile 寫入會按照其執行的順序進行。 不需要有符合的執行方式，就能提供單一的變動性寫入順序，如同所有執行執行緒所看到的一樣。 Volatile 欄位的類型必須是下列其中一項：

*  *Reference_type*。
*  類型 `byte`、`sbyte`、`short`、`ushort`、`int`、`uint`、`char`、`float`、`bool`、`System.IntPtr`或 `System.UIntPtr`。
*  具有 `byte`、`sbyte`、`short`、`ushort`、`int`或 `uint`列舉基底類型的*enum_type* 。

範例
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
```console
result = 143
```

在此範例中，方法 `Main` 會啟動新的執行緒，以執行 `Thread2`的方法。 這個方法會將值儲存在稱為 `result`的非 volatile 欄位中，然後將 `true` 儲存在 volatile 欄位 `finished`中。 主要執行緒會等候欄位 `finished` 設定為 `true`，然後讀取欄位 `result`。 因為 `finished` 已 `volatile`宣告，所以主執行緒必須從欄位 `result`讀取值 `143`。 如果未將欄位 `finished` 宣告 `volatile`，則會允許存放區在 `finished`存放區之後，讓主執行緒看到其 `result`，因此主執行緒會從欄位 `0` 讀取值 `result`。 將 `finished` 宣告為 `volatile` 欄位，可防止任何這類不一致的情況。

### <a name="field-initialization"></a>欄位初始化

欄位的初始值（不論是靜態欄位或實例欄位）是欄位類型的預設值（[預設](variables.md#default-values)值）。 在此預設初始化發生之前，不可能觀察到欄位的值，而且欄位永遠不會「未初始化」。 範例
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
```console
b = False, i = 0
```
因為 `b` 和 `i` 都會自動初始化為預設值。

### <a name="variable-initializers"></a>變數初始化運算式

欄位宣告可能包括*variable_initializer*s。 對於靜態欄位，變數初始化運算式會對應到在類別初始化期間執行的指派語句。 就實例欄位而言，變數初始化運算式會對應至建立類別的實例時所執行的指派語句。

範例
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
```console
x = 1.4142135623731, i = 100, s = Hello
```
因為在執行實例欄位初始化運算式時，會將指派給 `x`，因此會發生靜態欄位初始化運算式，並將其指派給 `i` 並 `s`。

[欄位初始化](classes.md#field-initialization)中所述的預設值初始化會針對所有欄位進行，包括具有變數初始化運算式的欄位。 因此，在初始化類別時，該類別中的所有靜態欄位都會先初始化為其預設值，然後靜態欄位初始化運算式會以文字循序執行。 同樣地，當建立類別的實例時，該實例中的所有實例欄位都會先初始化為其預設值，然後再以文字循序執行實例欄位初始化運算式。

具有變數初始化運算式的靜態欄位有可能會以其預設值狀態觀察到。 不過，強烈建議您不要使用這種樣式。 範例
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
展示此行為。 雖然 a 和 b 的迴圈定義，但程式是有效的。 這會導致輸出
```console
a = 1, b = 2
```
因為靜態欄位 `a` 和 `b` 會先初始化為 `0` （`int`的預設值），才會執行其初始化運算式。 當 `a` 的初始化運算式執行時，`b` 的值為零，因此 `a` 會初始化為 `1`。 當 `b` 的初始化運算式執行時，`a` 的值已經 `1`，因此 `b` 會初始化為 `2`。

#### <a name="static-field-initialization"></a>靜態欄位初始化

類別的靜態欄位變數初始化運算式會對應到一系列指派，這些是以它們出現在類別宣告中的文字順序來執行。 如果靜態的函式（[靜態](classes.md#static-constructors)的函式）存在於類別中，則在執行該靜態的函式之前，會立即執行靜態欄位初始化運算式。 否則，靜態欄位初始化運算式會在第一次使用該類別的靜態欄位之前，在與執行相關的時間執行。 範例
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
可能會產生以下輸出：
```console
Init A
Init B
1 1
```
或輸出：
```console
Init B
Init A
1 1
```
因為執行 `X`的初始化運算式和 `Y`的初始化運算式可能會以任一順序發生，它們只會在這些欄位的參考之前受到限制。 不過，在此範例中：
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
輸出必須是：
```console
Init B
Init A
1 1
```
因為靜態執行檔（如[靜態](classes.md#static-constructors)的函式中所定義）的規則，會提供 `B`的靜態函式（因此 `B`的靜態欄位初始化運算式），必須在 `A`的靜態的程式和欄位初始化運算式之前執行。

#### <a name="instance-field-initialization"></a>實例欄位初始化

類別的實例欄位變數初始化運算式，會對應至該類別的任何一個實例函式（「函式[初始化運算式](classes.md#constructor-initializers)」）進入時立即執行的一系列指派。 變數初始化運算式會以它們出現在類別宣告中的文字順序來執行。 類別實例的建立和初始化程式會在[實例](classes.md#instance-constructors)的函式中進一步說明。

實例欄位的變數初始化運算式無法參考所建立的實例。 因此，在變數初始化運算式中參考 `this` 時，就會發生編譯時期錯誤，因為變數初始化運算式會透過*simple_name*來參考任何實例成員。 在範例中
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
`y` 的變數初始化運算式會導致編譯時期錯誤，因為它會參考所建立之實例的成員。

## <a name="methods"></a>方法

「方法」是實作物件或類別所能執行之計算或動作的成員。 方法是使用*method_declaration*s 來宣告：

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

*Method_declaration*可能包括一組*屬性*（attribute）和四個存取修飾[詞（](attributes.md)[存取](classes.md#access-modifiers)修飾詞）的有效組合、`new` （[新的修飾](classes.md#the-new-modifier)詞）、`static` （[靜態和實例方法](classes.md#static-and-instance-methods)）、`virtual` （[虛擬方法](classes.md#virtual-methods)）、`override` （覆[寫方法](classes.md#override-methods)）、`sealed` （[密封方法](classes.md#sealed-methods)）、`abstract` （[抽象方法](classes.md#abstract-methods)），以及 `extern` （[外部方法](classes.md#external-methods)）修飾詞。

如果下列所有條件都成立，宣告就會有有效的修飾片語合：

*  宣告包含有效的存取修飾詞（[存取](classes.md#access-modifiers)修飾詞）組合。
*  宣告不會多次包含相同的修飾詞。
*  宣告最多包含下列其中一個修飾詞： `static`、`virtual`和 `override`。
*  宣告最多包含下列其中一個修飾詞： `new` 和 `override`。
*  如果宣告包含 `abstract` 修飾詞，則宣告不會包含下列任何修飾符： `static`、`virtual`、`sealed` 或 `extern`。
*  如果宣告包含 `private` 修飾詞，則宣告不會包含下列任何修飾符： `virtual`、`override`或 `abstract`。
*  如果宣告包含 `sealed` 修飾詞，則宣告也會包含 `override` 修飾詞。
*  如果宣告包含 `partial` 修飾詞，則不會包含下列任何修飾詞： `new`、`public`、`protected`、`internal`、`private`、`virtual`、`sealed`、`override`、`abstract`或 `extern`。

具有 `async` 修飾詞的方法是非同步函式，並遵循[非同步函數](classes.md#async-functions)中所述的規則。

方法宣告的*return_type*會指定方法所計算和傳回的數值型別。 如果方法沒有傳回值，則會 `void` *return_type* 。 如果宣告包含 `partial` 修飾詞，則傳回類型必須是 `void`。

*Member_name*指定方法的名稱。 除非方法是明確介面成員執行（[明確介面成員](interfaces.md#explicit-interface-member-implementations)的實作為），否則*member_name*只是一個*識別碼*。 若為明確介面成員的執行，則*member_name*包含*interface_type*後面接著 "`.`" 和*識別碼*。

選擇性的*type_parameter_list*會指定方法的類型參數（[類型參數](classes.md#type-parameters)）。 如果指定了*type_parameter_list* ，則方法是***泛型方法***。 如果方法具有 `extern` 修飾詞，則無法指定*type_parameter_list* 。

選擇性的*formal_parameter_list*會指定方法的參數（[方法參數](classes.md#method-parameters)）。

選擇性的*type_parameter_constraints_clause*會指定個別型別參數（[型別參數條件約束](classes.md#type-parameter-constraints)）的條件約束，而且只有在同時提供*type_parameter_list* ，而且方法沒有 `override` 修飾詞時，才可指定。

*Return_type*和方法的*formal_parameter_list*中所參考的每個類型，必須至少與方法本身（[存取範圍條件約束](basic-concepts.md#accessibility-constraints)）相同。

*Method_body*可以是分號、***語句主體***或***運算式主體***。 語句主體是由*區塊*所組成，它會指定叫用方法時要執行的語句。 運算式主體包含 `=>` 後面接著*運算式*和分號，並代表叫用方法時要執行的單一運算式。 

對於 `abstract` 和 `extern` 方法， *method_body*只會包含一個分號。 針對 `partial` 方法， *method_body*可能包含分號、區塊主體或運算式主體。 對於所有其他方法， *method_body*是區塊主體或運算式主體。

如果*method_body*是由分號組成，則宣告可能不會包含 `async` 修飾詞。

方法的名稱、型別參數清單和正式參數清單會定義方法的簽章（簽章[和](basic-concepts.md#signatures-and-overloading)多載）。 具體而言，方法的簽章包含其名稱、型別參數的數目，以及其正式參數的數目、修飾詞和類型。 基於這些目的，在型式參數類型中發生之方法的任何型別參數，都不是以其名稱來識別，而是依其在方法的型別引數清單中的序數位置。傳回型別不是方法簽章的一部分，也不是型別參數或正式參數的名稱。

方法的名稱必須與在相同類別中宣告之所有其他非方法的名稱不同。 此外，方法的簽章必須與相同類別中宣告之所有其他方法的簽章不同，而且在相同類別中宣告的兩個方法，不能有不同于 `ref` 和 `out`的簽章。

方法的*type_parameter*在整個*method_declaration*的範圍內，而且可用來在*return_type*、 *method_body*和*type_parameter_constraints_clause*中的整個範圍內形成類型，但不能用在*屬性*中。

所有正式參數和類型參數都必須有不同的名稱。

### <a name="method-parameters"></a>方法參數

方法的參數（如果有的話）會由方法的*formal_parameter_list*宣告。

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

型式參數清單是由一或多個以逗號分隔的參數所組成，其中只有最後一個可以是*parameter_array*。

*Fixed_parameter*由一組選擇性的*屬性*（[屬性](attributes.md)）、選擇性的 `ref`、`out` 或 `this` 修飾詞、*類型*、*識別碼*和選擇性*default_argument*所組成。 每個*fixed_parameter*都會宣告具有指定名稱之給定類型的參數。 `this` 修飾詞將方法指定為擴充方法，而且只允許用於靜態方法的第一個參數。 擴充方法會進一步說明擴充[方法。](classes.md#extension-methods)

具有*default_argument*的*fixed_parameter*稱為「***選擇性參數***」，而不含*default_argument*的*fixed_parameter*則是***必要參數***。 必要參數可能不會出現在*formal_parameter_list*中的選擇性參數之後。

`ref` 或 `out` 參數不能有*default_argument*。 *Default_argument*中的*運算式*必須是下列其中一項：

*  *constant_expression*
*  表單 `new S()` 的運算式，其中 `S` 是實數值型別
*  表單 `default(S)` 的運算式，其中 `S` 是實數值型別

*運算式*必須以身分識別或可為 null 的轉換為參數的類型，以隱含方式轉換。

如果在執行部分方法宣告（[部分方法](classes.md#partial-methods)）中發生選擇性參數，則明確的介面成員執行（[明確介面成員](interfaces.md#explicit-interface-member-implementations)實作為），或在單一參數索引子宣告（[索引子](classes.md#indexers)）中，編譯器應該會發出警告，因為這些成員絕對無法以允許省略引數的方式叫用。

*Parameter_array*由一組選擇性的*屬性*（[屬性](attributes.md)）、`params` 修飾詞、 *array_type*和*識別碼*所組成。 參數陣列會宣告具有指定名稱之指定陣列類型的單一參數。 參數陣列的*array_type*必須是一維陣列類型（[陣列類型](arrays.md#array-types)）。 在方法調用中，參數陣列允許指定給定陣列類型的單一引數，或允許指定陣列元素類型的零個或多個引數。 參數陣列中會進一步描述參數[陣列。](classes.md#parameter-arrays)

*Parameter_array*可能會在選擇性參數之後發生，但不能有預設值--省略*parameter_array*的引數會改為建立空陣列。

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

在 `M`的*formal_parameter_list*中，`i` 是必要的 ref 參數，`d` 是必要的值參數，`b`，`s`，`o` 和 `t` 是選擇性的值參數，而 `a` 是參數陣列。

方法宣告會為參數、型別參數和區域變數建立個別的宣告空間。 名稱會由方法的類型參數清單和方法的型式參數清單和方法的*區塊*中的區域變數宣告，引進此宣告空間中。 方法宣告空間的兩個成員都有相同的名稱，這是一項錯誤。 方法宣告空間和嵌套宣告空間的區域變數宣告空間，包含具有相同名稱的專案，這是一項錯誤。

方法調用（[方法](expressions.md#method-invocations)叫用）會建立該調用的特定複本，這是方法的型式參數和區域變數的本機變數，而叫用的引數清單會將值或變數參考指派給新建立的正式參數。 在方法的*區塊*內，型式參數可由其在*Simple_name*運算式（[簡單名稱](expressions.md#simple-names)）中的識別碼來參考。

型式參數共有四種：

*  未宣告任何修飾詞的值參數。
*  參考參數，使用 `ref` 修飾詞進行宣告。
*  輸出參數，使用 `out` 修飾詞進行宣告。
*  參數陣列，使用 `params` 修飾詞進行宣告。

如簽章[和](basic-concepts.md#signatures-and-overloading)多載中所述，`ref` 和 `out` 修飾詞都是方法簽章的一部分，但 `params` 修飾詞則不是。

#### <a name="value-parameters"></a>值參數

以 no 修飾詞宣告的參數是值參數。 值參數會對應至本機變數，以從方法調用中提供的對應引數取得其初始值。

當型式參數是值參數時，方法調用中的對應引數必須是可隱含轉換（[隱含轉換](conversions.md#implicit-conversions)）為正式參數類型的運算式。

允許方法將新值指派給值參數。 這類指派只會影響值參數所代表的本機儲存位置，而不會影響在方法調用中指定的實際引數。

#### <a name="reference-parameters"></a>傳址參數

使用 `ref` 修飾詞宣告的參數是參考參數。 與值參數不同的是，參考參數不會建立新的儲存位置。 相反地，參考參數代表的儲存位置與指定為方法調用中引數的變數相同。

當型式參數是參考參數時，方法調用中的對應引數必須包含關鍵字 `ref` 後面接著*variable_reference* （[判斷明確指派的精確規則](variables.md#precise-rules-for-determining-definite-assignment)），其類型與正式參數相同。 必須先明確指派變數，才可以將它當做參考參數傳遞。

在方法內，一律會將參考參數視為明確指派。

宣告為 iterator （[反覆運算](classes.md#iterators)器）的方法不能有參考參數。

範例
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
```console
i = 2, j = 1
```

針對 `Main`中的 `Swap` 調用，`x` 代表 `i`，而 `y` 代表 `j`。 因此，叫用會影響 `i` 和 `j`的值交換。

在採用參考參數的方法中，有可能會有多個名稱代表相同的儲存位置。 在範例中
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
`G` 中的 `F` 的調用會將 `a` 和 `b`的參考傳遞給 `s`。 因此，針對該叫用，`s`、`a`和 `b` 的名稱全都參考相同的儲存位置，而三個指派全都會修改實例欄位 `s`。

#### <a name="output-parameters"></a>輸出參數

使用 `out` 修飾詞宣告的參數是輸出參數。 類似于參考參數，輸出參數不會建立新的儲存位置。 相反地，output 參數代表的儲存位置與指定為方法調用中引數的變數相同。

當型式參數是輸出參數時，方法調用中的對應引數必須包含關鍵字 `out` 後面接著*variable_reference* （[判斷明確指派的精確規則](variables.md#precise-rules-for-determining-definite-assignment)），其類型與正式參數相同。 變數不一定要先被指派，才可以做為輸出參數來傳遞，但在將變數當做輸出參數傳遞的叫用之後，會將變數視為明確指派。

在方法內，如同本機變數，輸出參數一開始會被視為未指派，而且必須在使用其值之前明確指派。

方法的每個輸出參數都必須在方法傳回之前明確指派。

宣告為部分方法（[部分方法](classes.md#partial-methods)）或 Iterator （[反覆運算](classes.md#iterators)器）的方法不能有輸出參數。

輸出參數通常用於產生多個傳回值的方法。 例如：
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
```console
c:\Windows\System\
hello.txt
```

請注意，`dir` 和 `name` 變數可以在傳遞至 `SplitPath`之前解除指派，並在呼叫之後將其視為明確指派。

#### <a name="parameter-arrays"></a>參數陣列

使用 `params` 修飾詞宣告的參數是參數陣列。 如果正式參數清單包含參數陣列，它必須是清單中的最後一個參數，而且必須是一維陣列類型。 例如，`string[]` 和 `string[][]` 的類型可用來做為參數陣列的類型，但類型 `string[,]` 則不能使用。 您無法將 `params` 修飾詞結合 `ref` 和 `out`的修飾詞。

參數陣列允許在方法調用中以兩種方式之一來指定引數：

*  為參數陣列提供的引數可以是可隱含轉換（[隱含轉換](conversions.md#implicit-conversions)）為參數陣列類型的單一運算式。 在此情況下，參數陣列的作用會與值參數完全相同。
*  或者，叫用可以為參數陣列指定零個或多個引數，其中每個引數都是隱含轉換（[隱含轉換](conversions.md#implicit-conversions)）為參數陣列之元素類型的運算式。 在此情況下，調用會使用對應引數數目的長度來建立參數陣列類型的實例、使用指定的引數值初始化陣列實例的元素，並使用新建立的陣列實例作為實際的引數.

除了允許在調用中使用不定數目的引數，參數陣列也會精確地等同于相同類型的值參數（[值](classes.md#value-parameters)參數）。

範例
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
```console
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

`F` 的第一個調用只會將陣列 `a` 當做值參數傳遞。 `F` 的第二個調用會自動以指定的元素值建立四個元素的 `int[]`，並將該陣列實例當做值參數傳遞。 同樣地，`F` 的第三個調用會建立零元素 `int[]`，並將該實例當做值參數傳遞。 第二個和第三個調用相當於撰寫：
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

執行多載解析時，具有參數陣列的方法可能會以其一般格式或擴充形式（適用函式[成員](expressions.md#applicable-function-member)）的形式適用。 只有當方法的一般形式不適用時，才可以使用方法的展開形式，而且只有在相同類型中尚未宣告與展開表單具有相同簽章的適用方法時，才會提供。

範例
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
```console
F();
F(object[]);
F(object,object);
F(object[]);
F(object[]);
```

在此範例中，具有參數陣列之方法的兩個可能擴充形式已包含在類別中，做為一般方法。 因此，在執行多載解析時，不會考慮這些展開的表單，而第一個和第三個方法調用會因此選取一般方法。 當類別宣告具有參數陣列的方法時，通常也會將部分擴充形式納入為一般方法。 如此一來，就可以在叫用具有參數陣列的方法擴充形式時，避免配置所發生的陣列實例。

當參數陣列的類型是 `object[]`時，方法的一般形式和單一 `object` 參數所花費的格式可能會有不明確的情況。 不明確的原因是 `object[]` 本身會隱含地轉換成類型 `object`。 但不明確呈現問題，因為它可以在需要時插入轉換來加以解決。

範例
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
```console
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

在 `F`的第一個和最後一個調用中，`F` 的一般形式是適用的，因為從引數類型到參數類型的隱含轉換（兩者都是 `object[]`的類型）。 因此，多載解析會選取 `F`的一般形式，而引數會當做一般值參數傳遞。 在第二個和第三個調用中，一般的 `F` 形式並不適用，因為引數類型到參數類型的隱含轉換不是（類型 `object` 無法隱含地轉換成類型 `object[]`）。 不過，`F` 的擴充形式是適用的，因此會由多載解析選取。 因此，呼叫會建立一個專案 `object[]`，而陣列的單一元素會以指定的引數值（其本身是 `object[]`的參考）進行初始化。

### <a name="static-and-instance-methods"></a>靜態和執行個體方法

當方法宣告包含 `static` 修飾詞時，該方法會被視為靜態方法。 當沒有 `static` 的修飾詞時，此方法會被視為實例方法。

靜態方法不會在特定的實例上運作，而是編譯時期錯誤，以在靜態方法中參考 `this`。

實例方法會在類別的指定實例上運作，而且該實例可以當做 `this` （[此存取](expressions.md#this-access)）來存取。

當方法在表單 `E.M`的*member_access* （[成員存取](expressions.md#member-access)）中被參考時，如果 `M` 是靜態方法，`E` 必須代表包含 `M`的類型，而且如果 `M` 是實例方法，則 `E` 必須表示包含 `M`之類型的實例。

靜態和實例成員之間的差異會在[靜態和實例成員](classes.md#static-and-instance-members)中進一步討論。

### <a name="virtual-methods"></a>虛擬方法

當實例方法宣告包含 `virtual` 修飾詞時，該方法會被視為虛擬方法。 當沒有 `virtual` 的修飾詞時，方法就稱為非虛擬方法。

非虛擬方法的實作為不變：不論方法是在宣告它的類別實例或衍生類別的實例上叫用，都是一樣的。 相反地，衍生類別可以取代虛擬方法的執行。 取代繼承虛擬方法的執行程式，稱為覆***寫***該方法（覆[寫方法](classes.md#override-methods)）。

在虛擬方法調用中，叫用的實例的***執行時間類型***會決定要叫用的實際方法執行。 在非虛擬方法調用中，實例的***編譯階段類型***是決定因素。 具體而言，當使用引數清單叫用名為 `N` 的方法時，`A` 在具有編譯時間類型 `C` 和執行時間類型 `R` （其中 `R` 是 `C` 或衍生自 `C`的類別）的實例上，則會以下列方式處理調用：

*  首先，多載解析會套用至 `C`、`N`和 `A`，以從 `C`所宣告並繼承的方法集合中選取特定的方法 `M`。 這會在[方法調用](expressions.md#method-invocations)中說明。
*  然後，如果 `M` 是非虛擬方法，就會叫用 `M`。
*  否則，`M` 是虛擬方法，而且會叫用與 `R` 有關的 `M` 最衍生的實。

針對類別所宣告或繼承的每個虛擬方法，會有與該類別相關之方法的***最大衍生執行***。 與類別 `R` 相關之虛擬方法的最大衍生執行 `M`，如下所示：

*  如果 `R` 包含 `M`的 `virtual` 宣告簡介，則這是 `M`的最衍生執行。
*  否則，如果 `R` 包含 `M`的 `override`，則這是 `M`的最衍生執行。
*  否則，與 `R`的直接基類有關的最多衍生 `M`，`M` 就是與 `R` 的最佳衍生執行。

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

在此範例中，`A` 引進了非虛擬方法 `F` 和 `G`的虛擬方法。 類別 `B` 引進新的非虛擬方法 `F`，因此會隱藏繼承的 `F`，也會覆寫繼承的方法 `G`。 此範例會產生輸出：
```console
A.F
B.F
B.G
B.G
```

請注意，語句 `a.G()` 會叫用 `B.G`，而不是 `A.G`。 這是因為實例的執行時間類型（`B`），而不是實例的編譯時間類型（`A`），會決定要叫用的實際方法執行。

因為允許方法隱藏繼承的方法，所以類別可以包含數個具有相同簽章的虛擬方法。 這不會造成不明確的問題，因為所有的衍生方法都是隱藏的。 在範例中
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
[`C`] 和 [`D`] 類別包含兩個具有相同簽章的虛擬方法： `A` 所引進的和 `C`所導入的一種。 `C` 引進的方法會隱藏繼承自 `A`的方法。 因此，`D` 中的覆寫宣告會覆寫由 `C`所導入的方法，而且不可能 `D` 覆寫 `A`所引進的方法。 此範例會產生輸出：
```console
B.F
B.F
D.F
D.F
```

請注意，透過不會隱藏方法的較小衍生類型來存取 `D` 的實例，就可以叫用隱藏的虛擬方法。

### <a name="override-methods"></a>覆寫方法

當實例方法宣告包含 `override` 修飾詞時，此方法會被視為覆***寫方法***。 Override 方法會以相同的簽章覆寫繼承的虛擬方法。 虛擬方法宣告會導入新的方法，而覆寫方法宣告則是會提供現有已繼承之虛擬方法的新實作，來將該方法特製化。

`override` 宣告所覆寫的方法稱為覆寫的***基底方法***。 如果覆寫方法 `M` 在類別 `C`中宣告，覆寫基底方法的判斷方式是檢查每個基類類型的 `C`，從 `C` 的直接基類類型開始，然後繼續每個連續的直接基類類型，直到指定的基類類型中，至少有一個可存取的方法，其簽章與替代類型引數之後 `M` 的簽章相同。 為了找出覆寫的基底方法，如果方法已 `public``protected`，則會被視為可存取，如果已 `protected internal`，或 `internal` 並在與 `C`相同的程式中宣告。

如果覆寫宣告的下列所有條件都成立，就會發生編譯時期錯誤：

*  如上面所述，已覆寫的基底方法可以找到。
*  只有一個這種覆寫的基底方法。 只有當基類型別是結構化型別，而其中類型引數的替代方式讓兩個方法的簽章相同時，這項限制才會生效。
*  覆寫的基底方法是虛擬、抽象或覆寫方法。 換句話說，覆寫的基底方法不可以是靜態或非虛擬的。
*  覆寫的基底方法不是密封的方法。
*  覆寫方法和覆寫的基底方法具有相同的傳回類型。
*  覆寫宣告和覆寫的基底方法具有相同的宣告存取範圍。 換句話說，覆寫宣告無法變更虛擬方法的存取範圍。 不過，如果覆寫的基底方法是內部保護的，而且在與包含覆寫方法的元件不同的元件中宣告，則覆寫方法的宣告存取範圍必須受到保護。
*  覆寫宣告不會指定類型參數條件約束子句。 相反地，條件約束會繼承自覆寫的基底方法。 請注意，在覆寫方法中，類型參數的條件約束可能會由繼承之條件約束中的類型引數取代。 這可能會導致在明確指定（例如實數值型別或密封類型）時不合法的條件約束。

下列範例示範泛型類別的覆寫規則的使用方式：
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

覆寫宣告可以使用*base_access* （[基底存取](expressions.md#base-access)）來存取覆寫的基底方法。 在範例中
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
`B` 中的 `base.PrintFields()` 調用會叫用 `A`中所宣告的 `PrintFields` 方法。 *Base_access*會停用虛擬調用機制，而且只會將基底方法視為非虛擬方法。 已 `((A)this).PrintFields()`撰寫 `B` 中的調用，它會以遞迴方式叫用 `B`中所宣告的 `PrintFields` 方法，而不是在 `A`中宣告的方法，因為 `PrintFields` 是虛擬的，而 `((A)this)` 的執行時間類型是 `B`。

只有包含 `override` 修飾詞，方法才能覆寫另一個方法。 在所有其他情況下，與繼承方法具有相同簽章的方法，只會隱藏繼承的方法。 在範例中
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
`B` 中的 `F` 方法不包含 `override` 修飾詞，因此不會覆寫 `A`中的 `F` 方法。 相反地，`B` 中的 `F` 方法會隱藏 `A`中的方法，而且會回報警告，因為宣告不包含 `new` 修飾詞。

在範例中
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
`B` 中的 `F` 方法會隱藏繼承自 `A`的虛擬 `F` 方法。 因為 `B` 中的新 `F` 具有私用存取權，所以其範圍僅包含 `B` 的類別主體，並不會延伸到 `C`。 因此，`C` 中的 `F` 宣告允許覆寫繼承自 `A`的 `F`。

### <a name="sealed-methods"></a>密封方法

當實例方法宣告包含 `sealed` 修飾詞時，該方法會被視為密封的***方法***。 如果實例方法宣告包含 `sealed` 修飾詞，它也必須包含 `override` 修飾詞。 使用 `sealed` 修飾詞可防止衍生類別進一步覆寫方法。

在範例中
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
類別 `B` 提供兩個覆寫方法：具有 `sealed` 修飾詞的 `F` 方法，以及不具有的 `G` 方法。 `B`使用密封的 `modifier` 可防止 `C` 進一步覆寫 `F`。

### <a name="abstract-methods"></a>抽象方法

當實例方法宣告包含 `abstract` 修飾詞時，該方法會被視為***抽象方法***。 雖然抽象方法也是隱含的虛擬方法，但它不能有修飾詞 `virtual`。

抽象方法宣告引進新的虛擬方法，但不提供該方法的執行。 相反地，非抽象衍生類別必須覆寫該方法，才能提供自己的執行。 因為抽象方法不提供實際的實值，所以抽象方法的*method_body*只會包含一個分號。

抽象方法宣告只允許用於抽象類別（[抽象類別](classes.md#abstract-classes)）。

在範例中
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
`Shape` 類別會定義可繪製本身的幾何形狀物件的抽象概念。 `Paint` 方法是抽象的，因為沒有有意義的預設實值。 `Ellipse` 和 `Box` 類別是具體的 `Shape` 實作為。 因為這些類別是非抽象的，所以必須覆寫 `Paint` 方法並提供實際的實作為。

*Base_access* （[基底存取](expressions.md#base-access)）參考抽象方法時，會發生編譯時期錯誤。 在範例中
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
`base.F()` 調用會回報編譯時期錯誤，因為它會參考抽象方法。

允許抽象方法宣告覆寫虛擬方法。 這可讓抽象類別在衍生類別中強制重新執行方法，並使方法的原始實作為無法使用。 在範例中
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
類別 `A` 宣告虛擬方法，類別 `B` 會以抽象方法覆寫這個方法，而類別 `C` 會覆寫抽象方法以提供自己的實作為。

### <a name="external-methods"></a>外部方法

當方法宣告包含 `extern` 修飾詞時，該方法會被視為***外部方法***。 外部方法會在外部執行，通常使用以外的語言C#。 因為外部方法宣告不提供實際的實值，所以外部方法的*method_body*只會包含一個分號。 外部方法可能不是泛型。

`extern` 修飾詞通常會與 `DllImport` 屬性（[與 COM 和 Win32 元件](attributes.md#interoperation-with-com-and-win32-components)互通）搭配使用，讓外部方法可以由 Dll （動態連結程式庫）來執行。 執行環境可支援其他機制，讓您可以提供外部方法的實現。

當外部方法包含 `DllImport` 屬性時，方法宣告也必須包含 `static` 修飾詞。 這個範例示範如何使用 `extern` 修飾詞和 `DllImport` 屬性：
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

### <a name="partial-methods-recap"></a>部分方法（回顧）

當方法宣告包含 `partial` 修飾詞時，該方法會被視為***部分方法***。 部分方法只能宣告為部分類型（[部分類型](classes.md#partial-types)）的成員，而且受到一些限制。 部分方法會進一步說明部分[方法。](classes.md#partial-methods)

### <a name="extension-methods"></a>擴充方法

當方法的第一個參數包含 `this` 修飾詞時，該方法會被視為***擴充方法***。 擴充方法只能在非泛型、非嵌套的靜態類別中宣告。 擴充方法的第一個參數不能有 `this`以外的任何修飾詞，且參數類型不能是指標類型。

以下是宣告兩個擴充方法的靜態類別範例：
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

擴充方法是一般的靜態方法。 此外，其封閉式靜態類別在範圍內，可以使用實例方法調用語法（[擴充方法調用](expressions.md#extension-method-invocations)）叫用擴充方法，使用接收者運算式做為第一個引數。

下列程式會使用上述所宣告的擴充方法：
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

`Slice` 方法可在 `string[]`上取得，而 `ToInt32` 方法則可在 `string`上使用，因為它們已宣告為擴充方法。 使用一般靜態方法呼叫時，程式的意義與下列內容相同：
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

方法宣告的*method_body*是由區塊主體、運算式主體或分號所組成。

如果傳回型別 `void`，或如果方法是非同步，而且傳回型別為 `System.Threading.Tasks.Task`，則方法的***結果型***別會 `void`。 否則，非非同步方法的結果型別就是它的傳回型別，而且具有傳回型別 `System.Threading.Tasks.Task<T>` 之非同步方法的結果型別會 `T`。

當方法具有 `void` 結果型別和區塊主體時，不允許在區塊中使用 `return` 的語句（[return 語句](statements.md#the-return-statement)）來指定運算式。 如果 void 方法的區塊執行正常（也就是，控制在方法主體結尾的流程），該方法只會回到其目前的呼叫端。
    
當方法具有 `void` 結果和運算式主體時，運算式 `E` 必須是*statement_expression*，而主體則完全等同于表單 `{ E; }`的區塊主體。
    
當方法具有非 void 的結果型別和區塊主體時，區塊中的每個 `return` 語句都必須指定可隱含轉換成結果型別的運算式。 值傳回方法之區塊主體的端點不得為可連線。 換句話說，在具有區塊主體的值傳回方法中，不允許控制項在方法主體的結尾流動。
    
當方法具有非 void 的結果型別和運算式主體時，運算式必須可以隱含地轉換成結果型別，而主體則完全等同于表單 `{ return E; }`的區塊主體。
    
在範例中
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
傳回的值 `F` 方法會導致編譯時期錯誤，因為控制項可以在方法主體的結尾處流動。 `G` 和 `H` 方法是正確的，因為所有可能的執行路徑都在指定傳回值的 return 語句中結束。 `I` 方法是正確的，因為它的主體等同于其中只有一個 return 語句的語句區塊。

### <a name="method-overloading"></a>方法多載

方法多載解析規則會在[型別推斷](expressions.md#type-inference)中說明。

## <a name="properties"></a>屬性

***屬性***是可存取物件或類別特性的成員。 屬性的範例包括字串的長度、字型的大小、視窗的標題、客戶的名稱等等等等。 屬性是欄位的自然延伸，兩者都是具有相關聯類型的命名成員，而存取欄位和屬性的語法則相同。 不過，與欄位不同的是，屬性並不會指示儲存位置。 取而代之的是，屬性會有「存取子」，這些存取子會指定讀取或寫入其值時要執行的陳述式。 因此，屬性會提供一個機制，讓動作與物件屬性的讀取和寫入產生關聯;此外，它們允許計算這類屬性。

屬性會使用*property_declaration*s 來宣告：

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

*Property_declaration*可能包括一組*屬性*（attribute）和四個存取修飾[詞（](attributes.md)[存取](classes.md#access-modifiers)修飾詞）的有效組合、`new` （[新的修飾](classes.md#the-new-modifier)詞）、`static` （[靜態和實例方法](classes.md#static-and-instance-methods)）、`virtual` （[虛擬方法](classes.md#virtual-methods)）、`override` （覆[寫方法](classes.md#override-methods)）、`sealed` （[密封方法](classes.md#sealed-methods)）、`abstract` （[抽象方法](classes.md#abstract-methods)），以及 `extern` （[外部方法](classes.md#external-methods)）修飾詞。

屬性宣告受限於與方法宣告（[方法](classes.md#methods)）相同的規則，但與修飾詞的有效組合有關。

屬性宣告的*型*別會指定宣告所引進的屬性型別，而*member_name*會指定屬性的名稱。 除非屬性是明確介面成員的執行，否則*member_name*只是一個*識別碼*。 若為明確介面成員（[明確介面成員](interfaces.md#explicit-interface-member-implementations)的執行），則*member_name*包含*interface_type*後面接著 "`.`" 和*識別碼*。

屬性的*類型*至少必須與屬性本身（[存取範圍條件約束](basic-concepts.md#accessibility-constraints)）一樣可以存取。

*Property_body*可能是由***存取子主體***或***運算式主體***所組成。 在存取子主體中， *accessor_declarations*（必須包含在 "`{`" 和 "`}`" 標記中），請宣告屬性的存取子（[存取](classes.md#accessors)子）。 存取子會指定與讀取和寫入屬性相關聯的可執行語句。

包含 `=>` 後面接著*運算式*`E` 和分號的運算式主體，與語句主體 `{ get { return E; } }`完全相等，因此只能用來指定僅限 getter 的屬性，其中 getter 的結果是由單一運算式所指定。

只能為自動執行的屬性（[自動實作為屬性](classes.md#automatically-implemented-properties)）提供*property_initializer* ，並將這類屬性的基礎欄位初始化，使其具有*運算式*所指定的值。

雖然存取屬性的語法與欄位相同，但屬性不會分類為變數。 因此，不可能將屬性當做 `ref` 或 `out` 引數傳遞。

當屬性宣告包含 `extern` 修飾詞時，屬性稱為***外部屬性***。 因為外部屬性宣告不提供實際的實值，所以它的每個*accessor_declarations*都包含一個分號。

### <a name="static-and-instance-properties"></a>靜態和實例屬性

當屬性宣告包含 `static` 修飾詞時，屬性稱為***靜態屬性***。 當沒有 `static` 的修飾詞時，此屬性稱為***實例屬性***。

靜態屬性未與特定實例相關聯，而且是編譯時期錯誤，可在靜態屬性的存取子中參考 `this`。

實例屬性與類別的指定實例相關聯，而且該實例可以在該屬性的存取子中以 `this` （[此存取權](expressions.md#this-access)）的方式來存取。

當屬性在表單 `E.M`的*member_access* （[成員存取](expressions.md#member-access)）中被參考時，如果 `M` 是靜態屬性，`E` 必須代表包含 `M`的類型，而且如果 `M` 是實例屬性，則 E 必須代表包含 `M`之類型的實例。

靜態和實例成員之間的差異會在[靜態和實例成員](classes.md#static-and-instance-members)中進一步討論。

### <a name="accessors"></a>取值

屬性的*accessor_declarations*會指定與讀取和寫入該屬性相關聯的可執行語句。

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

存取子宣告包含*get_accessor_declaration*、 *set_accessor_declaration*或兩者。 每個存取子宣告都包含 `get` 或 `set` 的 token，後面接著選擇性的*accessor_modifier*和*accessor_body*。

使用*accessor_modifier*會受到下列限制：

*  *Accessor_modifier*不能用在介面或明確介面成員的執行中。
*  對於沒有 `override` 修飾詞的屬性或索引子，只有在屬性或索引子同時具有 `get` 和 `set` 存取子，而且只允許在其中一個存取子上時，才允許*accessor_modifier* 。
*  對於包含 `override` 修飾詞的屬性或索引子，存取子必須符合所要覆寫之存取子的*accessor_modifier*（如果有的話）。
*  *Accessor_modifier*必須宣告比屬性或索引子本身的宣告存取範圍嚴格更嚴格的存取範圍。 精確：
   * 如果屬性或索引子具有 `public`的已宣告存取範圍，則*accessor_modifier*可能是 `protected internal`、`internal`、`protected`或 `private`。
   * 如果屬性或索引子具有 `protected internal`的宣告存取範圍， *accessor_modifier*可能是 `internal`、`protected`或 `private`。
   * 如果屬性或索引子具有 `internal` 或 `protected`的已宣告存取範圍，則必須 `private`*accessor_modifier* 。
   * 如果屬性或索引子具有 `private`的宣告存取範圍，則不會使用任何*accessor_modifier* 。

對於 `abstract` 和 `extern` 屬性而言，指定的每個存取子的*accessor_body*只是一個分號。 非抽象的非外部屬性可能會有每個*accessor_body*都是分號，在此情況下，它是***自動***實作為屬性（[自動實作為](classes.md#automatically-implemented-properties)屬性）。 自動執行的屬性至少必須有 get 存取子。 對於任何其他非抽象、非外部屬性的存取子， *accessor_body*是一個*區塊*，它會指定叫用對應存取子時要執行的語句。

`get` 存取子會對應至具有屬性類型傳回值的無參數方法。 除了指派的目標以外，在運算式中參考屬性時，會叫用屬性的 `get` 存取子來計算屬性的值（[運算式的值](expressions.md#values-of-expressions)）。 `get` 存取子的主體必須符合[方法主體](classes.md#method-body)中所述之傳回值之方法的規則。 特別是 `get` 存取子主體中的所有 `return` 語句，都必須指定可隱含轉換成屬性類型的運算式。 此外，也不得連線到 `get` 存取子的端點。

`set` 存取子會對應至方法，其中包含屬性類型的單一值參數和 `void` 傳回類型。 `set` 存取子的隱含參數一律會命名為 `value`。 當屬性當做指派（[指派運算子](expressions.md#assignment-operators)）的目標來參考時，或作為 `++` 或 `--` 的運算元（後置[遞增和遞減運算子](expressions.md#postfix-increment-and-decrement-operators)、[前置遞增和遞減運算子](expressions.md#prefix-increment-and-decrement-operators)），會使用引數（其值是指派的右手邊或 `++` 或 `--` 運算子右邊的運算元）來叫用 `set` 存取子，以提供新的值（[簡單指派](expressions.md#simple-assignment)）。 `set` 存取子的主體必須符合[方法主體](classes.md#method-body)中所述 `void` 方法的規則。 特別是，不允許 `set` 存取子主體中的 `return` 語句指定運算式。 由於 `set` 存取子會隱含地擁有名為 `value`的參數，因此，在 `set` 存取子中的區域變數或常數宣告必須有該名稱，才會發生編譯時期錯誤。

根據 `get` 和 `set` 存取子的存在與否，屬性的分類方式如下：

*  包含 `get` 存取子和 `set` 存取子的屬性稱為***讀寫***屬性。
*  只有 `get` 存取子的屬性稱為***唯讀***屬性。 唯讀屬性為指派的目標時，會發生編譯時期錯誤。
*  只有 `set` 存取子的屬性稱為「僅***限寫入***」屬性。 除了指派的目標以外，在運算式中參考僅限寫入屬性的編譯時期錯誤。

在範例中
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
`Button` 控制項會宣告公用的 `Caption` 屬性。 `Caption` 屬性的 `get` 存取子會傳回儲存在私用 `caption` 欄位中的字串。 `set` 存取子會檢查新值是否與目前的值不同，如果是，則會儲存新的值，並重新繪製控制項。 屬性通常會遵循上面顯示的模式： `get` 存取子只會傳回儲存在私用欄位中的值，而 `set` 存取子則會修改該私用欄位，然後執行完全更新物件狀態所需的任何其他動作。

假設上述的 `Button` 類別，以下是使用 `Caption` 屬性的範例：
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

在這裡，`set` 存取子是藉由指派值給屬性來叫用，而 `get` 存取子則是藉由參考運算式中的屬性來叫用。

屬性（property）的 `get` 和 `set` 存取子不是不同的成員，而且不可能分別宣告屬性的存取子。 因此，讀寫屬性的兩個存取子不可能有不同的存取範圍。 範例
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
不會宣告單一讀寫屬性。 相反地，它會宣告兩個具有相同名稱的屬性，一個是唯讀，一個只寫入。 由於在同一個類別中宣告的兩個成員不能有相同的名稱，因此此範例會導致編譯時期錯誤發生。

當衍生類別宣告的屬性與繼承屬性的名稱相同時，衍生的屬性會隱藏與讀取和寫入有關的繼承屬性。 在範例中
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
`B` 中的 `P` 屬性會在讀取和寫入方面隱藏 `A` 中的 `P` 屬性。 因此，在語句中
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
`b.P` 的指派會導致回報編譯時期錯誤，因為 `B` 中的唯讀 `P` 屬性會隱藏 `A`中的寫入 `P` 屬性。 不過，請注意，轉換可以用來存取隱藏的 `P` 屬性。

不同于公用欄位，屬性會提供物件的內部狀態與其公用介面之間的分隔。 請考慮下列範例：
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

在這裡，`Label` 類別會使用兩個 `int` 欄位（`x` 和 `y`）來儲存其位置。 位置會公開為 `X` 和 `Y` 屬性，以及 `Point`類型的 `Location` 屬性。 如果在未來的 `Label`版本中，將位置儲存為內部 `Point` 會變得更方便，但不會影響類別的公用介面，因此可以進行變更：
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

`x`，而 `y` 改為 `public readonly` 欄位，就無法對 `Label` 類別進行這類變更。

透過屬性公開狀態不一定要比直接公開欄位更有效率。 特別是當屬性不是虛擬，而且只包含少量的程式碼時，執行環境可能會以存取子的實際程式碼來取代存取子的呼叫。 此程式稱為「***內嵌***」，它可讓屬性存取的效率與欄位存取一樣有效率，但仍會保留更多的屬性彈性。

因為叫用 `get` 存取子在概念上相當於讀取欄位的值，所以將其視為不正確的程式設計樣式，以讓 `get` 存取子有明顯的副作用。 在範例中
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
`Next` 屬性的值取決於屬性先前被存取的次數。 因此，存取屬性會產生可觀察的副作用，而屬性應實作為方法。

`get` 存取子的「沒有副作用」慣例並不表示 `get` 存取子應該一律撰寫成隻傳回儲存在欄位中的值。 事實上，`get` 存取子通常會藉由存取多個欄位或叫用方法來計算屬性的值。 不過，適當設計的 `get` 存取子不會執行任何動作，而導致物件狀態中的可觀察變更。

屬性可以用來延遲資源的初始化，直到第一次參考它為止。 例如：
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

`Console` 類別包含三個屬性： `In`、`Out`和 `Error`，分別代表標準的輸入、輸出和錯誤裝置。 藉由將這些成員公開為屬性，`Console` 類別可以延遲其初始化，直到實際使用為止。 例如，在第一次參考 `Out` 屬性時，如下所示：
```csharp
Console.Out.WriteLine("hello, world");
```
會建立輸出裝置的基礎 `TextWriter`。 但是，如果應用程式不會參考 `In` 並 `Error` 屬性，則不會為這些裝置建立任何物件。

### <a name="automatically-implemented-properties"></a>自動執行的屬性

自動執行的屬性（簡稱為「***自動」屬性***）是具有僅限分號存取子主體的非抽象非外部屬性。 Auto 屬性必須有 get 存取子，而且可以選擇性地擁有 set 存取子。

當屬性指定為自動執行的屬性時，會自動提供屬性的隱藏支援欄位，而且會實作為讀取和寫入該支援欄位的存取子。 如果 auto 屬性沒有 set 存取子，則會將支援欄位視為 `readonly` （[Readonly 欄位](classes.md#readonly-fields)）。 就像 `readonly` 欄位一樣，僅限 getter 的 auto 屬性也可以指派給封閉式類別的函式主體。 這類指派會直接指派給屬性的 readonly 支援欄位。

Auto 屬性可以選擇性地具有*property_initializer*，這會直接套用至支援欄位做為*variable_initializer* （[變數初始化運算式](classes.md#variable-initializers)）。

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

請注意，[唯讀] 欄位的指派是合法的，因為它們會出現在此函式中。


### <a name="accessibility"></a>協助工具選項

如果存取子具有*accessor_modifier*，存取子的存取範圍定義域（[存取](basic-concepts.md#accessibility-domains)範圍網域）就會使用*accessor_modifier*的宣告存取範圍來決定。 如果存取子沒有*accessor_modifier*，存取子的存取範圍定義域就會從屬性或索引子的宣告存取範圍來決定。

*Accessor_modifier*的存在不會影響成員查閱（[運算子](expressions.md#operators)）或多載解析（多載[解析](expressions.md#overload-resolution)）。 屬性或索引子上的修飾詞一律會決定系結至哪一個屬性或索引子，而不論存取的內容為何。

一旦選取了特定的屬性或索引子，就會使用所涉及之特定存取子的存取範圍定義域來判斷該用法是否有效：

*  如果使用方式為值（[運算式的值](expressions.md#values-of-expressions)），則 `get` 存取子必須存在且可供存取。
*  如果使用方式是做為簡單指派（[簡單指派](expressions.md#simple-assignment)）的目標，則 `set` 存取子必須存在且可供存取。
*  如果使用方式是複合指派（[複合指派](expressions.md#compound-assignment)）的目標，或做為 `++` 或 `--` 運算子（函式[成員](expressions.md#function-members).9，[調用運算式](expressions.md#invocation-expressions)）的目標，則 `get` 存取子和 `set` 存取子必須存在且可供存取。

在下列範例中，屬性 `B.Text`會隱藏屬性 `A.Text`，即使在只呼叫 `set` 存取子的內容中也是如此。 相反地，類別 `M`無法存取屬性 `B.Count`，因此會改用可存取的屬性 `A.Count`。

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

用來執行介面的存取子可能沒有*accessor_modifier*。 如果只有一個存取子是用來執行介面，則可以使用*accessor_modifier*來宣告另一個存取子：
```csharp
public interface I
{
    string Prop { get; }
}

public class C: I
{
    public string Prop {
        get { return "April"; }       // Must not have a modifier here
        internal set {...}            // Ok, because I.Prop has no set accessor
    }
}
```

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a>虛擬、密封、覆寫和抽象屬性存取子

`virtual` 屬性宣告會指定屬性的存取子是虛擬的。 `virtual` 修飾詞會套用到讀寫屬性的兩個存取子，而唯讀屬性的一個存取子則不可能是虛擬的。

`abstract` 屬性宣告會指定屬性的存取子是虛擬的，但不提供存取子的實際執行。 相反地，非抽象衍生類別必須藉由覆寫屬性，為存取子提供自己的執行。 因為抽象屬性宣告的存取子不提供實際的實值，所以它的*accessor_body*只會包含一個分號。

同時包含 `abstract` 和 `override` 修飾詞的屬性宣告，會指定屬性為抽象，並覆寫基底屬性。 這類屬性的存取子也是抽象的。

抽象屬性宣告只允許用於抽象類別（[抽象類別](classes.md#abstract-classes)）。藉由包含指定 `override` 指示詞的屬性宣告，可以在衍生類別中覆寫繼承的虛擬屬性的存取子。 這就是所謂的覆***寫屬性***宣告。 覆寫屬性宣告不會宣告新的屬性。 相反地，它只會特製化現有虛擬屬性的存取子。

覆寫屬性宣告必須指定與繼承的屬性完全相同的存取範圍修飾詞、類型和名稱。 如果繼承的屬性只有一個存取子（也就是，如果繼承的屬性是唯讀或僅限寫入），則覆寫屬性必須只包含該存取子。 如果繼承的屬性同時包含這兩個存取子（也就是，如果繼承的屬性是讀寫），則覆寫屬性可以包含單一存取子或兩個存取子。

覆寫屬性宣告可能包含 `sealed` 修飾詞。 使用此修飾詞可防止衍生類別進一步覆寫屬性。 密封屬性的存取子也是密封的。

除了宣告和調用語法的差異之外，virtual、sealed、override 和 abstract 存取子的行為與虛擬、sealed、override 和 abstract 方法完全相同。 具體而言，[虛擬方法](classes.md#virtual-methods)、覆[寫方法](classes.md#override-methods)、[密封方法](classes.md#sealed-methods)和[抽象方法](classes.md#abstract-methods)中所述的規則，會如同存取子是對應格式的方法一樣適用：

*  `get` 存取子會對應到無參數的方法，其中包含屬性類型的傳回值，以及與包含屬性相同的修飾詞。
*  `set` 存取子會對應至方法，其中包含屬性類型的單一值參數、`void` 傳回類型，以及與包含屬性相同的修飾詞。

在範例中
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
`X` 是虛擬唯讀屬性，`Y` 是虛擬讀寫屬性，而 `Z` 則是抽象的讀寫屬性。 因為 `Z` 是抽象的，所以包含類別 `A` 也必須宣告為抽象。

衍生自 `A` 的類別如下所示：
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

在這裡，`X`、`Y`和 `Z` 的宣告會覆寫屬性宣告。 每個屬性宣告完全符合協助工具修飾詞、類型和對應繼承屬性的名稱。 `X` 的 `get` 存取子和 `Y` 的 `set` 存取子會使用 `base` 關鍵字來存取繼承的存取子。 `Z` 的宣告會覆寫抽象存取子，因此 `B`中沒有任何未處理的抽象函數成員，而 `B` 則允許成為非抽象類別。

當屬性宣告為 `override`時，所有覆寫的存取子都必須可供覆寫程式碼存取。 此外，屬性或索引子本身和存取子的宣告存取範圍，都必須符合覆寫成員和存取子的。 例如：
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

「***事件***」（event）是可讓物件或類別提供通知的成員。 用戶端可以藉由提供***事件處理常式***，附加事件的可執行程式碼。

事件是使用*event_declaration*s 來宣告：

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

*Event_declaration*可能包括一組*屬性*（attribute）和四個存取修飾[詞（](attributes.md)[存取](classes.md#access-modifiers)修飾詞）的有效組合、`new` （[新的修飾](classes.md#the-new-modifier)詞）、`static` （[靜態和實例方法](classes.md#static-and-instance-methods)）、`virtual` （[虛擬方法](classes.md#virtual-methods)）、`override` （覆[寫方法](classes.md#override-methods)）、`sealed` （[密封方法](classes.md#sealed-methods)）、`abstract` （[抽象方法](classes.md#abstract-methods)），以及 `extern` （[外部方法](classes.md#external-methods)）修飾詞。

事件宣告受限於與方法宣告（[方法](classes.md#methods)）相同的規則，如同有效的修飾片語合。

事件宣告的*類型*必須是*delegate_type* （[參考型別](types.md#reference-types)），而且*Delegate_type*必須至少與事件本身（[存取範圍條件約束](basic-concepts.md#accessibility-constraints)）相同。

事件宣告可能包含*event_accessor_declarations*。 不過，如果不是在非外部的非抽象事件中，則編譯器會自動提供它們（[類似欄位的事件](classes.md#field-like-events)）;若是外來事件，存取子會在外部提供。

省略*event_accessor_declarations*的事件宣告會定義一或多個事件，每個*variable_declarator*各一個。 屬性和修飾詞適用于這類*event_declaration*所宣告的所有成員。

*Event_declaration*要同時包含 `abstract` 修飾詞和以大括弧分隔的*event_accessor_declarations*時，就會發生編譯時期錯誤。

當事件宣告包含 `extern` 修飾詞時，事件稱為「***外來事件***」。 因為外來事件宣告不提供實際的實值，所以它會發生錯誤，同時包含 `extern` 修飾詞和*event_accessor_declarations*。

如果事件宣告的*variable_declarator*具有 `abstract` 或 `external` 修飾詞以包含*variable_initializer*，就會發生編譯時期錯誤。

事件可用來做為 `+=` 和 `-=` 運算子（[事件指派](expressions.md#event-assignment)）的左運算元。 這些運算子分別用來將事件處理常式附加至或，以從事件中移除事件處理常式，而事件的存取修飾詞會控制允許這類作業的內容。

因為 `+=` 和 `-=` 是在宣告事件的類型外的事件上所允許的唯一作業，所以外部程式碼可以加入和移除事件的處理常式，但無法以任何其他方式取得或修改事件處理常式的基礎清單。

在表單 `x += y` 或 `x -= y`的作業中，當 `x` 是事件，而參考發生在包含 `x`宣告的類型之外時，作業的結果會有類型 `void` （而不是 `x`的類型，而是在指派後的值為 `x`）。 此規則會禁止外部程式碼間接檢查事件的基礎委派。

下列範例顯示如何將事件處理常式附加至 `Button` 類別的實例：
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

在這裡，`LoginDialog` 實例的程式會建立兩個 `Button` 實例，並將事件處理常式附加至 `Click` 事件。

### <a name="field-like-events"></a>類似欄位的事件

在包含事件宣告的類別或結構的程式文字內，某些事件可以像欄位一樣使用。 若要以這種方式使用，事件不得 `abstract` 或 `extern`，而且不得明確包含*event_accessor_declarations*。 這類事件可以在允許欄位的任何內容中使用。 欄位包含委派（[委派](delegates.md)），其參考已加入至事件的事件處理常式清單。 如果尚未加入任何事件處理常式，則欄位會包含 `null`。

在範例中
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
`Click` 會當做 `Button` 類別中的欄位使用。 如範例所示，您可以在委派調用運算式中檢查、修改和使用欄位。 `Button` 類別中的 `OnClick` 方法會「引發」 `Click` 事件。 引發事件的概念完全等同於叫用該事件所代表的委派項目，因此，就引發事件而言，並沒有任何特殊的語言建構。 請注意，委派調用的前面會有一個檢查可確保委派為非 null。

在 `Button` 類別的宣告外，`Click` 成員只能在 `+=` 和 `-=` 運算子的左邊使用，如下所示：
```csharp
b.Click += new EventHandler(...);
```
這會將委派附加至 `Click` 事件的調用清單，而
```csharp
b.Click -= new EventHandler(...);
```
這會從 `Click` 事件的調用清單中移除委派。

當編譯類似欄位的事件時，編譯器會自動建立儲存區來保存委派，並建立事件的存取子，以便將事件處理常式加入至委派欄位。 新增和移除作業是安全線程，而且可能（但並非必要）在針對實例事件的包含物件保留鎖定（[lock 語句](statements.md#the-lock-statement)），或靜態事件的類型物件（[匿名物件建立運算式](expressions.md#anonymous-object-creation-expressions)）時，執行（但不需要）。

因此，實例事件宣告的形式如下：
```csharp
class X
{
    public event D Ev;
}
```
將會編譯成相當於的內容：
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
在類別 `X`中，`+=` 和 `-=` 運算子左側 `Ev` 的參考會導致叫用 add 和 remove 存取子。 `Ev` 的所有其他參考都會編譯為參考隱藏欄位 `__Ev` 而不是（[成員存取](expressions.md#member-access)）。 "`__Ev`" 名稱是任意的;隱藏欄位可能會有任何名稱或完全沒有名稱。

### <a name="event-accessors"></a>事件存取子

事件宣告通常會省略*event_accessor_declarations*，如上述的 `Button` 範例所示。 執行此動作的其中一種情況，包括無法接受每個事件的一個欄位的儲存成本。 在這種情況下，類別可以包含*event_accessor_declarations* ，並使用私用機制來儲存事件處理常式的清單。

事件的*event_accessor_declarations*會指定與加入和移除事件處理常式相關聯的可執行語句。

存取子宣告包含*add_accessor_declaration*和*remove_accessor_declaration*。 每個存取子宣告都包含 `add` 或 `remove` 後面接著*區塊*的 token。 與*add_accessor_declaration*相關聯的*區塊*會指定在加入事件處理常式時要執行的語句，而與*remove_accessor_declaration*相關聯的*區塊*則會指定移除事件處理常式時要執行的語句。

每個*add_accessor_declaration*和*remove_accessor_declaration*都會對應至具有事件種類之單一值參數和 `void` 傳回類型的方法。 事件存取子的隱含參數名為 `value`。 在事件指派中使用事件時，會使用適當的事件存取子。 具體而言，如果指派運算子是 `+=` 則會使用 add 存取子，而且如果指派運算子為 `-=`，則會使用 remove 存取子。 在任一情況下，指派運算子的右手運算元會當做事件存取子的引數使用。 *Add_accessor_declaration*或*remove_accessor_declaration*的區塊必須符合[方法主體](classes.md#method-body)中所述 `void` 方法的規則。 特別是，這類區塊中的 `return` 語句不允許指定運算式。

由於事件存取子會隱含地具有名為 `value`的參數，因此，在事件存取子中宣告的區域變數或常數必須有該名稱，才會發生編譯時期錯誤。

在範例中
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
`Control` 類別會實作為事件的內部儲存機制。 `AddEventHandler` 方法會將委派值與索引鍵產生關聯，`GetEventHandler` 方法會傳回目前與索引鍵相關聯的委派，而 `RemoveEventHandler` 方法則會移除委派，做為指定事件的事件處理常式。 大概而言，基礎儲存機制的設計，是將 `null` 委派值與金鑰建立關聯不會產生任何費用，因此未處理的事件就不會耗用任何儲存體。

### <a name="static-and-instance-events"></a>靜態和實例事件

當事件宣告包含 `static` 修飾詞時，事件稱為***靜態事件***。 當沒有 `static` 的修飾詞時，事件就稱為「實例」***事件***。

靜態事件不會與特定實例相關聯，而且會發生編譯時期錯誤，以在靜態事件的存取子中參考 `this`。

實例事件與類別的指定實例相關聯，而且可以在該事件的存取子中，以 `this` （[此存取權](expressions.md#this-access)）的方式存取這個實例。

在表單 `E.M`的*member_access* （[成員存取](expressions.md#member-access)）中參考事件時，如果 `M` 是靜態事件，`E` 必須代表包含 `M`的類型，而且如果 `M` 是實例事件，則 E 必須表示包含 `M`之類型的實例。

靜態和實例成員之間的差異會在[靜態和實例成員](classes.md#static-and-instance-members)中進一步討論。

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a>虛擬、密封、覆寫和抽象事件存取子

`virtual` 事件宣告會指定該事件的存取子是虛擬的。 `virtual` 修飾詞適用于事件的兩個存取子。

`abstract` 事件宣告會指定事件的存取子是虛擬的，但不會提供存取子的實際執行。 相反地，非抽象衍生類別必須藉由覆寫事件，為存取子提供自己的執行。 因為抽象事件宣告不提供實際的實值，所以無法提供以大括弧分隔的*event_accessor_declarations*。

同時包含 `abstract` 和 `override` 修飾詞的事件宣告，會指定事件為抽象，並覆寫基底事件。 這類事件的存取子也是抽象的。

抽象事件宣告只允許用於抽象類別（[抽象類別](classes.md#abstract-classes)）。

繼承之虛擬事件的存取子可以在衍生類別中覆寫，方法是包含指定 `override` 修飾詞的事件宣告。 這就是所謂的覆***寫事件***宣告。 覆寫事件宣告不會宣告新的事件。 相反地，它只是特製化現有虛擬事件的存取子。

覆寫事件宣告必須指定與覆寫事件完全相同的存取範圍修飾詞、類型和名稱。

覆寫事件宣告可能包含 `sealed` 的修飾詞。 使用此修飾詞可防止衍生類別進一步覆寫事件。 密封事件的存取子也是密封的。

覆寫事件宣告要包含 `new` 修飾詞，這是編譯時期錯誤。

除了宣告和調用語法的差異之外，virtual、sealed、override 和 abstract 存取子的行為與虛擬、sealed、override 和 abstract 方法完全相同。 具體而言，[虛擬方法](classes.md#virtual-methods)、覆[寫方法](classes.md#override-methods)、[密封方法](classes.md#sealed-methods)和[抽象方法](classes.md#abstract-methods)中所述的規則，會如同存取子是對應表單的方法一樣適用。 每個存取子都會對應至一個方法，其中具有事件種類的單一值參數、`void` 傳回類型，以及與包含事件相同的修飾詞。

## <a name="indexers"></a>索引子

***索引子***是一種成員，可讓物件以與陣列相同的方式進行索引。 使用*indexer_declaration*s 宣告索引子：

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

*Indexer_declaration*可能包括一組*屬性*（attribute）和四個存取修飾[詞（](attributes.md)[存取](classes.md#access-modifiers)修飾詞）的有效組合、`new` （[新的修飾](classes.md#the-new-modifier)詞）、`virtual` （[虛擬方法](classes.md#virtual-methods)）、`override` （覆[寫方法](classes.md#override-methods)）、`sealed` （[密封方法](classes.md#sealed-methods)）、`abstract` （[抽象方法](classes.md#abstract-methods)），以及 `extern` （[外部方法](classes.md#external-methods)）修飾詞。

索引子宣告受限於與方法宣告（[方法](classes.md#methods)）相同的規則，與修飾詞的有效組合有關，唯一的例外是在索引子宣告上不允許使用 static 修飾詞。

除了在一種情況下，`virtual`、`override`和 `abstract` 的修飾詞互斥。 `abstract` 和 `override` 修飾詞可以一起使用，讓抽象的索引子可以覆寫虛擬一個。

索引子宣告的*類型*會指定宣告所引進之索引子的元素類型。 除非索引子是明確的介面成員執行，否則*類型*後面會接著關鍵字 `this`。 若為明確介面成員的執行，*類型*後面會接著*interface_type*、"`.`" 和關鍵字 `this`。 不同于其他成員，索引子沒有使用者定義的名稱。

*Formal_parameter_list*指定索引子的參數。 索引子的型式參數清單會對應至方法（[方法參數](classes.md#method-parameters)）的類型，但至少必須指定一個參數，而且不允許 `ref` 和 `out` 參數修飾詞。

索引子的*類型*和*formal_parameter_list*中所參考的每個類型，必須至少與索引子本身（[存取範圍條件約束](basic-concepts.md#accessibility-constraints)）相同。

*Indexer_body*可能是由***存取子主體***或***運算式主體***所組成。 在存取子主體中， *accessor_declarations*（必須包含在 "`{`" 和 "`}`" 標記中），請宣告屬性的存取子（[存取](classes.md#accessors)子）。 存取子會指定與讀取和寫入屬性相關聯的可執行語句。

運算式主體，其中包含 "`=>`" 後面接著運算式 `E` 而且分號與語句主體 `{ get { return E; } }`完全相等，因此只能用來指定僅限 getter 的索引子，其中 getter 的結果是由單一運算式所指定。

雖然存取索引子專案的語法與陣列元素的語法相同，但索引子元素不會分類為變數。 因此，您無法將索引子專案當做 `ref` 或 `out` 引數傳遞。

索引子的正式參數清單會定義索引子的簽章（簽章[和](basic-concepts.md#signatures-and-overloading)多載）。 具體而言，索引子的簽章是由其型式參數的數目和類型所組成。 元素類型和型式參數的名稱不是索引子簽章的一部分。

索引子的簽章必須與相同類別中宣告之所有其他索引子的簽章不同。

索引子和屬性在概念上非常類似，但下列方式不同：

*  屬性是由其名稱所識別，而索引子則是由其簽章來識別。
*  屬性是透過*simple_name* （[簡單名稱](expressions.md#simple-names)）或*member_access* （[成員存取](expressions.md#member-access)）來存取，而索引子元素則是透過*element_access* （[索引子存取](expressions.md#indexer-access)）來存取。
*  屬性可以是 `static` 成員，而索引子一律是實例成員。
*  屬性的 `get` 存取子會對應到沒有參數的方法，而索引子的 `get` 存取子則會對應至具有與索引子相同的型式參數清單的方法。
*  屬性的 `set` 存取子會對應至具有名為 `value`之單一參數的方法，而索引子的 `set` 存取子會對應至具有與索引子相同的型式參數清單的方法，再加上名為 `value`的其他參數。
*  這是一個編譯時期錯誤，可讓索引子存取子宣告與索引子參數名稱相同的本機變數。
*  在覆寫屬性宣告中，繼承的屬性會使用 `base.P`語法來存取，其中 `P` 是屬性名稱。 在覆寫索引子宣告中，會使用語法 `base[E]`來存取繼承的索引子，其中 `E` 是運算式的逗號分隔清單。
*  「自動執行的索引子」沒有概念。 具有不具有分號存取子的非抽象、非外部索引子是錯誤的。

除了這些差異之外，在[存取](classes.md#accessors)子和自動實作為[屬性](classes.md#automatically-implemented-properties)中定義的所有規則都適用于索引子存取子以及屬性存取子。

當索引子宣告包含 `extern` 修飾詞時，索引子稱為***外部索引子***。 因為外部索引子宣告不提供實際的實作為，所以它的每個*accessor_declarations*都包含一個分號。

下列範例會宣告一個 `BitArray` 類別，它會執行索引子來存取位陣列中的個別位。
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

`BitArray` 類別的實例所耗用的記憶體遠低於對應的 `bool[]` （因為前者的每一個值只會佔用一個位，而不是後者的一個位元組），但它允許與 `bool[]`相同的作業。

下列 `CountPrimes` 類別使用 `BitArray` 和傳統 "eratosthenes" 演算法來計算介於1和指定最大值之間的質數數目：
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

請注意，用來存取 `BitArray` 元素的語法，與 `bool[]`的語法完全相同。

下列範例顯示 26 * 10 格線類別，其中有一個具有兩個參數的索引子。 第一個參數必須是範圍 a-z 中的大寫或小寫字母，而第二個則必須是範圍0-9 中的整數。

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

索引子多載解析規則會在[型別推斷](expressions.md#type-inference)中說明。

## <a name="operators"></a>運算子

「***運算子***」（operator）是一個成員，其定義可以套用至類別實例之運算式運算子的意義。 運算子是使用*operator_declaration*s 來宣告：

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

有三種類別的多載運算子：一元運算子（[一元運算子](classes.md#unary-operators)）、二元運算子（[二元運算子](classes.md#binary-operators)）和轉換運算子（[轉換運算子](classes.md#conversion-operators)）。

*Operator_body*可以是分號、***語句主體***或***運算式主體***。 語句主體是由*區塊*所組成，它會指定叫用運算子時要執行的語句。 *區塊*必須符合[方法主體](classes.md#method-body)中所述之傳回值方法的規則。 運算式主體包含 `=>` 後面接著運算式和分號，並代表叫用運算子時要執行的單一運算式。

針對 `extern` 運算子， *operator_body*只包含一個分號。 對於所有其他運算子， *operator_body*是區塊主體或運算式主體。

下列規則適用于所有的運算子宣告：

*  運算子宣告必須同時包含 `public` 和 `static` 修飾詞。
*  運算子的參數必須是值參數（[值參數](variables.md#value-parameters)）。 運算子宣告指定 `ref` 或 `out` 參數時，會發生編譯時期錯誤。
*  運算子（[一元運算子](classes.md#unary-operators)、[二元運算子](classes.md#binary-operators)、[轉換運算子](classes.md#conversion-operators)）的簽章必須與相同類別中宣告之所有其他運算子的簽章不同。
*  在運算子宣告中參考的所有類型，都必須至少與運算子本身（存取範圍[條件約束](basic-concepts.md#accessibility-constraints)）一樣可以存取。
*  在運算子宣告中多次出現相同的修飾詞時，會發生錯誤。

每個運算子類別都會有額外的限制，如下列各節所述。

就像其他成員一樣，在基類中宣告的運算子會由衍生類別繼承。 因為運算子宣告一律需要宣告運算子以參與運算子簽章的類別或結構，所以在衍生類別中宣告的運算子不可能隱藏在基類中宣告的運算子。 因此，在運算子宣告中，絕對不需要 `new` 修飾詞，因此不允許使用。

如需一元和二元運算子的其他資訊，請參閱[運算子](expressions.md#operators)。

您可以在[使用者定義的轉換](conversions.md#user-defined-conversions)中找到轉換運算子的其他資訊。

### <a name="unary-operators"></a>一元運算子

下列規則適用于一元運算子宣告，其中 `T` 代表包含運算子宣告之類別或結構的實例類型：

*  一元 `+`、`-`、`!`或 `~` 運算子必須接受類型 `T` 或 `T?` 的單一參數，而且可以傳回任何類型。
*  一元 `++` 或 `--` 運算子必須接受類型 `T` 或 `T?` 的單一參數，而且必須傳回該相同類型或從中衍生的類型。
*  一元 `true` 或 `false` 運算子必須接受類型 `T` 或 `T?` 的單一參數，而且必須傳回類型 `bool`。

一元運算子的簽章是由運算子 token （`+`、`-`、`!`、`~`、`++`、`--`、`true`或 `false`）和單一型式參數的類型所組成。 傳回型別不是一元運算子的簽章的一部分，也不是型式參數的名稱。

`true` 和 `false` 一元運算子需要成對的宣告。 如果類別宣告其中一個運算子，但未同時宣告另一個，就會發生編譯時期錯誤。 `true` 和 `false` 運算子會在[使用者定義的條件式邏輯運算子](expressions.md#user-defined-conditional-logical-operators)和[布林運算式](expressions.md#boolean-expressions)中進一步說明。

下列範例顯示整數向量類別的 `operator ++` 的執行和後續使用方式：
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

請注意，運算子方法如何傳回將1新增至運算元所產生的值，就像後置遞增和遞減運算子（後置[遞增和遞減運算子](expressions.md#postfix-increment-and-decrement-operators)），以及前置遞增和遞減運算子（[前置遞增](expressions.md#prefix-increment-and-decrement-operators)和遞減運算子）。 不同于C++中的，此方法不需要直接修改其運算元的值。 事實上，修改運算元值會違反後置遞增運算子的標準語義。

### <a name="binary-operators"></a>二元運算子

下列規則適用于二元運算子宣告，其中 `T` 代表包含運算子宣告之類別或結構的實例類型：

*  二元非移位運算子必須接受兩個參數，其中至少一個必須具有類型 `T` 或 `T?`，而且可以傳回任何類型。
*  二元 `<<` 或 `>>` 運算子必須接受兩個參數，其中第一個必須有類型 `T` 或 `T?`，而第二個必須具有類型 `int` 或 `int?`，而且可以傳回任何類型。

二元運算子的簽章包含運算子 token （`+`、`-`、`*`、`/`、`%`、`&`、`|`、`^`、`<<`、`>>`、`==`、`!=`、`>`、`<`、`>=`或 `<=`）和兩個正式參數的類型。 傳回類型和型式參數的名稱不是二元運算子簽章的一部分。

某些二元運算子需要成對的宣告。 對於成對之任一個運算子的每個宣告，必須有配對的另一個運算子的相符宣告。 當兩個運算子宣告具有相同的傳回類型，且每個參數都有相同的類型時，就會進行比對。 下列運算子需要成對的宣告：

*  `operator ==` 和 `operator !=`
*  `operator >` 和 `operator <`
*  `operator >=` 和 `operator <=`

### <a name="conversion-operators"></a>轉換運算子

轉換運算子宣告導入了***使用者***定義的轉換（[使用者定義](conversions.md#user-defined-conversions)的轉換），這會擴大預先定義的隱含和明確轉換。

包含 `implicit` 關鍵字的轉換運算子宣告會引進使用者定義的隱含轉換。 隱含轉換可能會在各種情況下發生，包括函數成員調用、轉換運算式和指派。 這會在[隱含轉換](conversions.md#implicit-conversions)中進一步說明。

包含 `explicit` 關鍵字的轉換運算子宣告會引進使用者定義的明確轉換。 明確轉換可以在 cast 運算式中進行，並在[明確轉換](conversions.md#explicit-conversions)中進一步說明。

轉換運算子會從轉換運算子的參數類型所表示的來源類型，轉換成目標型別（由轉換運算子的傳回類型所表示）。

對於給定的來源類型 `S` 和目標型別 `T`，如果 `S` 或 `T` 是可為 null 的類型，請讓 `S0` 和 `T0` 參考其基礎類型，否則 `S0` 和 `T0` 會分別等於 `S` 和 `T`。 只有在下列所有條件都成立時，才能使用類別或結構來宣告從來源類型 `S` 至目標型別的轉換 `T`：

*  `S0` 和 `T0` 是不同的類型。
*  `S0` 或 `T0` 是發生運算子宣告的類別或結構類型。
*  `S0` 或 `T0` 都不是*interface_type*。
*  除了使用者定義的轉換之外，從 `S` 到 `T` 或從 `T` 到 `S`的轉換不存在。

基於這些規則的目的，與 `S` 或 `T` 相關聯的任何類型參數都會被視為與其他類型沒有繼承關係的唯一類型，而且會忽略這些類型參數上的任何條件約束。

在範例中
```csharp
class C<T> {...}

class D<T>: C<T>
{
    public static implicit operator C<int>(D<T> value) {...}      // Ok
    public static implicit operator C<string>(D<T> value) {...}   // Ok
    public static implicit operator C<T>(D<T> value) {...}        // Error
}
```
前兩個運算子宣告是允許的，因為[索引子](classes.md#indexers).3 的目的，分別 `T` 和 `int` 和 `string` 視為唯一沒有關聯性的類型。 不過，第三個運算子是錯誤，因為 `C<T>` 是 `D<T>`的基類。

從第二個規則來看，轉換運算子必須在宣告運算子的類別或結構類型中轉換成或。 例如，類別或結構類型 `C` 定義從 `C` 到 `int` 的轉換，以及從 `int` 到 `C`，但不能從 `int` 到 `bool`。

不可能直接重新定義預先定義的轉換。 因此，轉換運算子不允許從或轉換成 `object`，因為 `object` 與所有其他類型之間已經存在隱含和明確轉換。 同樣地，來源或轉換的目標型別都不能是另一個的基底類型，因為轉換之後就已經存在。

不過，您可以在泛型型別上宣告運算子，針對特定類型引數，指定已經存在做為預先定義轉換的轉換。 在範例中
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
當型別 `object` 指定為 `T`的型別引數時，第二個運算子會宣告已經存在的轉換（隱含的，因此也是明確的，從任何類型到 `object`類型的轉換都有）。

如果兩個類型之間存在預先定義的轉換，則會忽略這些類型之間的任何使用者定義轉換。 尤其是：

*  如果從類型 `S` 的預先定義隱含轉換（[隱含轉換](conversions.md#implicit-conversions)）存在於類型 `T`，則會忽略從 `S` 到 `T` 的所有使用者定義轉換（隱含或明確）。
*  如果從類型 `S` 的預先定義明確轉換（[明確轉換](conversions.md#explicit-conversions)）存在於類型 `T`，則會忽略從 `S` 到 `T` 的任何使用者定義明確轉換。 此外，

如果 `T` 是介面類別型，則會忽略從 `S` 到 `T` 的使用者定義隱含轉換。

否則，使用者定義的從 `S` 到 `T` 的隱含轉換仍會被視為。

針對所有類型但 `object`，上述 `Convertible<T>` 類型所宣告的運算子不會與預先定義的轉換衝突。 例如：
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

不過，針對型別 `object`，預先定義的轉換會在所有情況下隱藏使用者定義的轉換，但有一個：

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

使用者定義的轉換不允許從或轉換成*interface_type*s。 特別是，這項限制可確保轉換成*interface_type*時，不會發生任何使用者定義的轉換，而且只有在轉換的物件實際實作為指定的*interface_type*時，才會成功轉換成*interface_type* 。

轉換運算子的簽章是由來源類型和目標型別所組成。 （請注意，這是傳回型別參與簽章的唯一成員形式）。轉換運算子的 `implicit` 或 `explicit` 分類不是操作員簽章的一部分。 因此，類別或結構無法同時宣告具有相同來源和目標型別的 `implicit` 和 `explicit` 轉換運算子。

一般而言，使用者定義的隱含轉換應該設計成永遠不會擲回例外狀況，而且永遠不會遺失資訊。 如果使用者定義的轉換可能會導致例外狀況（例如，因為來源引數超出範圍）或遺失資訊（例如捨棄高序位位），則應該將該轉換定義為明確轉換。

在範例中
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
從 `Digit` 到 `byte` 的轉換是隱含的，因為它永遠不會擲回例外狀況或遺失資訊，但從 `byte` 到 `Digit` 的轉換是明確的，因為 `Digit` 只能代表 `byte`的可能值子集。

## <a name="instance-constructors"></a>實例構造函式

「執行個體建構函式」是實作將類別執行個體初始化所需之動作的成員。 實例的函式是使用*constructor_declaration*s 來宣告：

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

*Constructor_declaration*可能包括一組*屬性*（attribute）、四個存取修飾[詞（](attributes.md)[存取](classes.md#access-modifiers)修飾詞）的有效組合，以及 `extern` （[外部方法](classes.md#external-methods)）修飾詞。 不允許將相同的修飾詞多次包含在函式宣告中。

*Constructor_declarator*的*識別碼*必須為宣告實例的函式所在的類別命名。 如果指定了任何其他名稱，就會發生編譯時期錯誤。

實例參數的選擇性*formal_parameter_list*受限於方法（[方法](classes.md#methods)） *formal_parameter_list*的相同規則。 正式參數清單會定義實例的簽章（簽章[和](basic-concepts.md#signatures-and-overloading)多載），並控制多載解析（[類型推斷](expressions.md#type-inference)）在調用中選取特定實例的程式。

實例函式的*formal_parameter_list*中所參考的每個型別，都必須至少與此「函數」（[存取範圍條件約束](basic-concepts.md#accessibility-constraints)）一樣可以存取。

選擇性的*constructor_initializer*會指定另一個要叫用的實例函式，然後再執行這個實例的*constructor_body*中提供的語句。 這會在[函數初始化運算式](classes.md#constructor-initializers)中進一步說明。

當函式宣告包含 `extern` 修飾詞時，會將此函式稱為***外部***的「函數」（external）。 因為外部的函式宣告不提供實際的實作為，所以其*constructor_body*由分號組成。 對於所有其他的函式， *constructor_body*包含指定語句的*區塊*，以初始化類別的新實例。 這會完全對應至具有 `void` 傳回型別（[方法主體](classes.md#method-body)）之實例方法的*區塊*。

不會繼承實例的構造函式。 因此，除了在類別中實際宣告的類別以外，沒有任何實例的函式。 如果類別未包含任何實例的函式宣告，則會自動提供預設的實例（[預設](classes.md#default-constructors)為函式）。

實例構造函式是由*object_creation_expression*s （[物件建立運算式](expressions.md#object-creation-expressions)）和*constructor_initializer*s 叫用。

### <a name="constructor-initializers"></a>建構函式初始設定式

所有實例的「函式」（class `object`的對應項除外）會隱含地在*constructor_body*之前，直接包含另一個實例的程式調用。 隱含叫用的構造函式取決於*constructor_initializer*：

*  `base(argument_list)` 或 `base()` 形式的實例的「版本設定」初始化運算式，將會叫用直接基類的實例函式。 如果存在，則會使用*argument_list*選取該函式，以及多載[解析](expressions.md#overload-resolution)的多載解析規則。 候選實例的集合是由直接基類中包含的所有可存取的實例函式所組成，如果在直接基類中未宣告任何實例的函式，則是預設的函式（[預設](classes.md#default-constructors)的函數）。 如果這個集合是空的，或者無法識別單一最佳實例的函式，就會發生編譯時期錯誤。
*  `this(argument-list)` 或 `this()` 形式的實例的「版本設定」初始化運算式，將會叫用類別本身的實例函式。 如果存在，則會使用*argument_list*來選取此函式，以及多載[解析](expressions.md#overload-resolution)的多載解析規則。 候選實例的集合是由類別本身所宣告的所有可存取的實例函式所組成。 如果這個集合是空的，或者無法識別單一最佳實例的函式，就會發生編譯時期錯誤。 如果實例的「函式宣告」包含會叫用此函式本身的「函數初始化運算式」，就會發生編譯時期錯誤。

如果實例的「建立器」沒有任何「函式」初始化運算式，則會以隱含方式提供 `base()` 形式的「程式」（ 因此，表單的具現化函數宣告
```csharp
C(...) {...}
```
完全等同于
```csharp
C(...): base() {...}
```

實例的「參數」（instance）宣告之*formal_parameter_list*所指定的參數範圍包含該宣告的「函式初始化運算式」。 因此，允許使用函式初始化運算式來存取此函式的參數。 例如：
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

實例的「函式初始化運算式」無法存取所建立的實例。 因此，在函式初始化運算式的引數運算式中參考 `this` 時，會發生編譯時期錯誤，這是引數運算式透過*simple_name*參考任何實例成員的編譯時期錯誤。

### <a name="instance-variable-initializers"></a>執行個體變數初始化運算式

當實例的函式沒有任何函式初始化運算式，或它的格式為 `base(...)`的函式初始化運算式時，該函數會以隱含方式執行其類別中所宣告之實例欄位的*variable_initializer*所指定的初始化。 這會對應到在進入函式時，以及在直接基類的隱含調用之前，立即執行的一系列指派。 變數初始化運算式會以它們出現在類別宣告中的文字順序來執行。

### <a name="constructor-execution"></a>執行的函式

變數初始化運算式會轉換成指派語句，而這些指派語句會在叫用基類實例的函式之前執行。 這種排序可確保所有實例欄位都是由其變數初始化運算式來初始化，然後才會執行可存取該實例的任何語句。

假設範例
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
當 `new B()` 用來建立 `B`的實例時，會產生下列輸出：
```console
x = 1, y = 0
```

`x` 的值為1，因為在叫用基類實例的函式之前，會先執行變數初始化運算式。 不過，`y` 的值為0（`int`的預設值），因為在基底類別的函式傳回之前，不會執行指派給 `y`。

將執行個體變數初始化運算式和函式初始化運算式視為在*constructor_body*之前自動插入的語句，會很有説明。 範例
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
包含數個變數初始化運算式;它也包含兩種形式的函式初始化運算式（`base` 和 `this`）。 此範例會對應至如下所示的程式碼，其中每個批註都會指出自動插入的語句（用於自動插入的函式調用的語法無效，但只是用來說明機制）。

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

如果類別未包含任何實例的「函式」宣告，則會自動提供預設實例的「函式」。 該預設的構造函式只會叫用直接基類的無參數的函式。 如果類別是抽象的，則預設的函式的宣告存取範圍會受到保護。 否則，預設的函式的宣告存取範圍是公用的。 因此，預設的函式一律為格式

```csharp
protected C(): base() {}
```
或
```csharp
public C(): base() {}
```
其中 `C` 是類別的名稱。 如果多載解析無法判斷基類檢查程式初始化運算式的唯一最佳候選，則會發生編譯時期錯誤。

在範例中
```csharp
class Message
{
    object sender;
    string text;
}
```
會提供預設的函式，因為此類別不包含任何實例的函式宣告。 因此，此範例完全等同于
```csharp
class Message
{
    object sender;
    string text;

    public Message(): base() {}
}
```

### <a name="private-constructors"></a>私用函式

當類別 `T` 只宣告私用實例的函式時，`T` 的程式文字以外的類別不可能衍生自 `T` 或直接建立 `T`的實例。 因此，如果類別只包含靜態成員，而不想要具現化，則加入空的私用實例的函式將會導致無法具現化。 例如：
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

`Trig` 類別會將相關的方法和常數分組，但不打算具現化。 因此，它會宣告單一空白私用實例的函式。 至少必須宣告一個實例的函式，以隱藏自動產生預設的函式。

### <a name="optional-instance-constructor-parameters"></a>選擇性的實例構造函式參數

`this(...)` 形式的函式初始化運算式通常會與多載搭配使用，以執行選擇性的實例處理函式參數。 在範例中
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
前兩個實例的中繼資料只會提供遺漏引數的預設值。 兩者都使用 `this(...)` 的「函式初始化運算式」來叫用第三個實例的「函式」，這會實際執行初始化新實例的工作。 這是選擇性的函式參數的效果：
```csharp
Text t1 = new Text();                    // Same as Text(0, 0, null)
Text t2 = new Text(5, 10);               // Same as Text(5, 10, null)
Text t3 = new Text(5, 20, "Hello");
```

## <a name="static-constructors"></a>靜態建構函式

***靜態***的「函式」是一個成員，它會執行初始化封閉式類別型別的必要動作。 靜態的函式是使用*static_constructor_declaration*s 來宣告：

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

*Static_constructor_declaration*可以包含一組*屬性*（[屬性](attributes.md)）和 `extern` 修飾詞（[外部方法](classes.md#external-methods)）。

*Static_constructor_declaration*的*識別碼*必須命名用來宣告靜態函式的類別。 如果指定了任何其他名稱，就會發生編譯時期錯誤。

當靜態的函式宣告包含 `extern` 修飾詞時，靜態的函式會被視為***外部靜態***的函式。 因為外部靜態的「函式宣告」不提供實際的實值，所以其*static_constructor_body*由分號組成。 針對所有其他的靜態函式宣告， *static_constructor_body*由*區塊*所組成，指定要執行的語句以初始化類別。 這會完全對應至具有 `void` 傳回類型（[方法主體](classes.md#method-body)）之靜態方法的*method_body* 。

靜態的函式不會繼承，也不能直接呼叫。

封閉式類別類型的靜態函式在指定的應用程式域中最多隻會執行一次。 靜態函式的執行會由下列第一個事件所觸發，以在應用程式域內發生：

*  建立類別類型的實例。
*  會參考類別類型的任何靜態成員。

如果類別包含執行開始的 `Main` 方法（[應用程式啟動](basic-concepts.md#application-startup)），則該類別的靜態函式會在呼叫 `Main` 方法之前執行。

若要初始化新的封閉式類別類型，請先建立該特定封閉式類型的一組新靜態欄位（[靜態和實例欄位](classes.md#static-and-instance-fields)）。 每個靜態欄位都會初始化為其預設值（[預設值](variables.md#default-values)）。 接下來，靜態欄位初始化運算式（[靜態欄位初始化](classes.md#static-field-initialization)）會針對那些靜態欄位執行。 最後，會執行靜態的函式。

範例
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
必須產生輸出：
```console
Init A
A.F
Init B
B.F
```
因為 `A.F`的呼叫會觸發 `A`的靜態函式的執行，而且 `B`的靜態函式的執行是由 `B.F`的呼叫所觸發。

您可以建立迴圈相依性，讓具有變數初始化運算式的靜態欄位得以觀察其預設值狀態。

範例
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
```console
X = 1, Y = 2
```

若要執行 `Main` 方法，系統會先執行 `B.Y`的初始化運算式，再于類別 `B`的靜態方法。 `Y`的初始化運算式會導致 `A`的靜態函式執行，因為參考的是 `A.X` 的值。  `A` 的靜態函式接著會繼續計算 `X`的值，而這麼做會提取 `Y`的預設值，也就是零。 `A.X` 因此會初始化為1。 執行 `A`的靜態欄位初始化運算式和靜態處理常式的程式，然後傳回 `Y`初始值的計算，其結果會變成2。

由於靜態的函式只會針對每個封閉的結構化類別類型執行一次，因此，在無法透過條件約束（[類型參數條件約束](classes.md#type-parameter-constraints)）于編譯時期檢查的類型參數上強制執行執行時間檢查是一個方便的位置。 例如，下列型別會使用靜態的函式來強制型別引數為列舉：
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

「***析構函***式」是一個成員，它會執行銷毀類別的實例所需的動作。 使用*destructor_declaration*宣告的析構函式：

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

*Destructor_declaration*可能包含一組*屬性*（[屬性](attributes.md)）。

*Destructor_declaration*的*識別碼*必須命名為宣告函式的類別。 如果指定了任何其他名稱，就會發生編譯時期錯誤。

當析構函式宣告包含 `extern` 修飾詞時，會將此析構函數稱為***外部的析構函***式。 因為外部的析構函式宣告不提供實際的實值，所以其*destructor_body*由分號組成。 對於所有其他的析構函數， *destructor_body*由*區塊*所組成，其指定要執行的語句，以便銷毀類別的實例。 *Destructor_body*與 `void` 傳回型別（[方法主體](classes.md#method-body)）完全對應至實例方法的*method_body* 。

不會繼承析構函數。 因此，類別沒有任何可在該類別中宣告的析構函數。

由於析構函式必須沒有任何參數，因此無法多載，因此一個類別最多隻能有一個析構函式。

系統會自動叫用析構函，而無法明確叫用。 當任何程式碼無法再使用該實例時，實例就會成為可供銷毀的條件。 實例的析構函式可能會在實例符合終結條件之後的任何時間執行。 解構實例時，會依序呼叫該實例繼承鏈中的析構函數，從最常衍生到最低衍生。 可以在任何執行緒上執行一個析構函式。 如需有關控制何時以及如何執行「析構函式」之規則的進一步討論，請參閱[自動記憶體管理](basic-concepts.md#automatic-memory-management)。

範例的輸出
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
is
```
B's destructor
A's destructor
```
因為繼承鏈中的析構函數是依順序呼叫，從最常衍生到最不衍生的。

藉由覆寫 `System.Object`上 `Finalize` 的虛擬方法，來實作為析構函數。 C#不允許程式覆寫此方法，或直接呼叫它（或覆寫）。 例如，程式
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

編譯器的行為就像這個方法和覆寫一樣，完全不存在。 因此，此程式：
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
有效，且顯示的方法會隱藏 `System.Object`的 `Finalize` 方法。

如需從析構函式擲回例外狀況時的行為討論，請參閱[如何處理例外](exceptions.md#how-exceptions-are-handled)狀況。

## <a name="iterators"></a>迭代器

使用 iterator 區塊（[區塊](statements.md#blocks)）所實作用的函式成員（[函數成員](expressions.md#function-members)）稱為***iterator***。

只要對應的函式成員的傳回型別是其中一個列舉值介面（[枚舉器介面](classes.md#enumerator-interfaces)）或其中一個可列舉介面（可列舉[介面），](classes.md#enumerable-interfaces)iterator 區塊就可以當做函式成員的主體使用。 它可以是*method_body*、 *operator_body*或*accessor_body*，而事件、實例的函式、靜態的函式和析構函數無法實作為反覆運算器。

使用 iterator 區塊來實作用函式成員時，函式成員的型式參數清單會發生編譯時期錯誤，以指定任何 `ref` 或 `out` 參數。

### <a name="enumerator-interfaces"></a>列舉值介面

***列舉值介面***是 `System.Collections.IEnumerator` 的非泛型介面，而且泛型介面 `System.Collections.Generic.IEnumerator<T>`的所有具現化。 為了簡潔起見，在本章中，這些介面會分別參考為 `IEnumerator` 和 `IEnumerator<T>`。

### <a name="enumerable-interfaces"></a>可列舉介面

可列舉***介面***是 `System.Collections.IEnumerable` 的非泛型介面，而且泛型介面 `System.Collections.Generic.IEnumerable<T>`的所有具現化。 為了簡潔起見，在本章中，這些介面會分別參考為 `IEnumerable` 和 `IEnumerable<T>`。

### <a name="yield-type"></a>Yield 類型

Iterator 會產生一系列的值，這些都是相同的型別。 這個型別稱為 iterator 的***yield 型***別。

*  傳回 `IEnumerator` 或 `IEnumerable` 之反覆運算器的產生類型 `object`。
*  傳回 `IEnumerator<T>` 或 `IEnumerable<T>` 之反覆運算器的產生類型 `T`。

### <a name="enumerator-objects"></a>列舉值物件

當使用 iterator 區塊來執行傳回枚舉器介面類別型的函式成員時，叫用函式成員並不會立即執行 iterator 區塊中的程式碼。 相反地，會建立並傳回***列舉值物件***。 這個物件會封裝 iterator 區塊中指定的程式碼，而當叫用列舉值物件的 `MoveNext` 方法時，就會執行 iterator 區塊中的程式碼。 列舉值物件具有下列特性：

*  它會執行 `IEnumerator` 和 `IEnumerator<T>`，其中 `T` 是反覆運算器的產生類型。
*  它會實作 `System.IDisposable`。
*  它會使用傳遞至函式成員的引數值（如果有）和實例值的複本進行初始化。
*  它有四個可能的狀態： [***之前*** ***]、[執行中]***、[已***暫停***] 和 [***之後***]，***而 [開始于]*** 狀態。

枚舉器物件通常是編譯器所產生列舉值類別的實例，它會將程式碼封裝在 iterator 區塊中並實作為列舉值介面，但可能會有其他的執行方法。 如果列舉值類別是由編譯器產生，則會直接或間接在包含函式成員的類別中嵌套該類別，它將會具有私用存取範圍，而且會有保留給編譯器使用的名稱（[識別碼](lexical-structure.md#identifiers)）。

列舉值物件所執行的介面，可能會比上述指定的數目多。

下列各節描述列舉值物件所提供之 `IEnumerable` 和 `IEnumerable<T>` 介面的 `MoveNext`、`Current`和 `Dispose` 成員的確切行為。

請注意，列舉值物件不支援 `IEnumerator.Reset` 方法。 叫用這個方法會導致擲回 `System.NotSupportedException`。

#### <a name="the-movenext-method"></a>MoveNext 方法

列舉值物件的 `MoveNext` 方法會封裝 iterator 區塊的程式碼。 叫用 `MoveNext` 方法會在 iterator 區塊中執行程式碼，並適當地設定列舉值物件的 `Current` 屬性。 `MoveNext` 所執行的精確動作，取決於叫用 `MoveNext` 時，列舉值物件的狀態：

*  如果列舉值物件的狀態在***之前***，請叫用 `MoveNext`：
   * 將狀態變更為 [***正在***執行]。
   * 將反覆運算器區塊的參數（包括 `this`）初始化為初始化枚舉器物件時所儲存的引數值和實例值。
   * 從開始執行反覆運算器區塊，直到執行中斷為止（如下所述）。
*  如果列舉值物件的狀態為 [***正在***執行]，則會未指定叫用 `MoveNext` 的結果。
*  如果列舉值物件的狀態為已***暫***止，則叫用 `MoveNext`：
   * 將狀態變更為 [***正在***執行]。
   * 將所有區域變數和參數的值（包括 this）還原為上次暫停執行反覆運算器區塊時所儲存的值。 請注意，這些變數所參考的任何物件內容可能會在前一次呼叫 MoveNext 之後變更。
   * 在導致執行暫止的 `yield return` 語句之後，繼續執行反覆運算器區塊，直到中斷執行為止（如下所述）。
*  如果列舉值物件的狀態是***after***，叫用 `MoveNext` 會傳回 `false`。


當 `MoveNext` 執行 iterator 區塊時，可以透過下列四種方式中斷執行：藉由 `yield return` 語句、由 `yield break` 語句、遇到反覆運算器區塊的結尾，以及擲回的例外狀況，並從反覆運算器區塊傳播。

*  遇到 `yield return` 語句時（[yield 語句](statements.md#the-yield-statement)）：
   * 語句中所指定的運算式會進行評估、隱含地轉換成 yield 型別，以及指派給列舉值物件的 `Current` 屬性。
   * 反覆運算器主體的執行已暫停。 所有區域變數和參數的值（包括 `this`）都會儲存起來，就像這個 `yield return` 語句的位置一樣。 如果 `yield return` 語句位於一個或多個 `try` 區塊內，此時不會執行相關聯的 `finally` 區塊。
   * 列舉值物件的狀態會變更為 [已***暫停***]。
   * `MoveNext` 方法會將 `true` 傳回給其呼叫端，表示反復專案已成功前進到下一個值。
*  遇到 `yield break` 語句時（[yield 語句](statements.md#the-yield-statement)）：
   * 如果 `yield break` 語句位於一個或多個 `try` 區塊內，則會執行相關聯的 `finally` 區塊。
   * 列舉值物件的狀態會變更為 [***之後***]。
   * `MoveNext` 方法會將 `false` 傳回給其呼叫端，表示反復專案已完成。
*  遇到反覆運算器主體的結尾時：
   * 列舉值物件的狀態會變更為 [***之後***]。
   * `MoveNext` 方法會將 `false` 傳回給其呼叫端，表示反復專案已完成。
*  當擲回例外狀況並從反覆運算器區塊傳播時：
   * 反覆運算器主體中適當的 `finally` 區塊，會由例外狀況傳播執行。
   * 列舉值物件的狀態會變更為 [***之後***]。
   * 例外狀況傳播會繼續到 `MoveNext` 方法的呼叫端。

#### <a name="the-current-property"></a>目前的屬性

列舉值物件的 `Current` 屬性會受到 iterator 區塊中 `yield return` 語句的影響。

當枚舉器物件處於***暫停***狀態時，`Current` 的值就是先前呼叫 `MoveNext`所設定的值。 當列舉值物件處於 [***之前*** ***]、[執行中]*** 或 [***之後***] 狀態時，就不會指定存取 `Current` 的結果。

針對具有 `object`以外之 yield 類型的反覆運算器，透過列舉值物件的 `IEnumerable` 執行來存取 `Current` 的結果，會對應到透過列舉值物件的 `IEnumerator<T>` 執行來存取 `Current`，並將結果轉換成 `object`。

#### <a name="the-dispose-method"></a>Dispose 方法

`Dispose` 方法是用來藉由將列舉值物件帶到***after***狀態來清除反復專案。

*  如果列舉值物件的狀態在***之前***，叫用 `Dispose` 會將狀態變更為 [***之後***]。
*  如果列舉值物件的狀態為 [***正在***執行]，則會未指定叫用 `Dispose` 的結果。
*  如果列舉值物件的狀態為已***暫***止，則叫用 `Dispose`：
   * 將狀態變更為 [***正在***執行]。
   * 執行任何 finally 區塊，就如同最後一次執行的 `yield return` 語句是 `yield break` 的語句一樣。 如果這會導致擲回例外狀況，並將其傳播到反覆運算器主體外，則列舉值物件的狀態會設定為***after*** ，而例外狀況會傳播至 `Dispose` 方法的呼叫端。
   * 將狀態變更為 [***之後***]。
*  如果列舉值物件的狀態是***after***，叫用 `Dispose` 不會有任何影響。

### <a name="enumerable-objects"></a>可列舉物件

當使用 iterator 區塊來執行傳回可列舉介面類別型的函式成員時，叫用函式成員並不會立即執行 iterator 區塊中的程式碼。 相反地，會建立並傳回可列舉***物件***。 可列舉物件的 `GetEnumerator` 方法會傳回列舉反覆運算器區塊中指定之程式碼的列舉值物件，而當叫用枚舉器物件的 `MoveNext` 方法時，就會執行 iterator 區塊中的程式碼。 可列舉物件具有下列特性：

*  它會執行 `IEnumerable` 和 `IEnumerable<T>`，其中 `T` 是反覆運算器的產生類型。
*  它會使用傳遞至函式成員的引數值（如果有）和實例值的複本進行初始化。

可列舉物件通常是編譯器產生之可列舉類別的實例，它會將程式碼封裝在 iterator 區塊中並實作為可列舉介面，但可能的其他方法也是可行的。 如果可列舉的類別是由編譯器產生，則會直接或間接在包含函式成員的類別中嵌套該類別，它會有私用存取範圍，而且它會有保留供編譯器使用的名稱（[識別碼](lexical-structure.md#identifiers)）。

可列舉物件所執行的介面，可能會比上述指定的數目多。 特別是，可列舉物件也可以實作為 `IEnumerator` 和 `IEnumerator<T>`，讓它同時做為可列舉和枚舉器。 在這種類型的執行中，第一次叫用可列舉物件的 `GetEnumerator` 方法時，會傳回可列舉物件本身。 可列舉物件 `GetEnumerator`的後續調用（如果有的話）會傳回可列舉物件的複本。 因此，每個傳回的列舉值都有自己的狀態，而其中一個列舉值的變更將不會影響另一個。

#### <a name="the-getenumerator-method"></a>GetEnumerator 方法

可列舉物件提供 `IEnumerable` 和 `IEnumerable<T>` 介面之 `GetEnumerator` 方法的執行。 這兩個 `GetEnumerator` 方法共用一個通用的實值，可取得並傳回可用的列舉值物件。 枚舉器物件會使用初始化可列舉物件時所儲存的引數值和實例值進行初始化，否則列舉值物件會如[列舉值物件](classes.md#enumerator-objects)中所述的方式運作。

### <a name="implementation-example"></a>執行範例

本節描述標準C#結構的可能執行反覆運算器。 此處所述的執行是以 Microsoft C#編譯器所使用的相同原則為基礎，但它不是強制執行或唯一可行的方法。

下列 `Stack<T>` 類別使用 iterator 來實作為其 `GetEnumerator` 方法。 反覆運算器會以上到下的順序列舉堆疊的元素。

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

`GetEnumerator` 方法可以轉譯為編譯器產生的列舉值類別的具現化，該類別會將程式碼封裝在 iterator 區塊中，如下所示。

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

在前述的轉譯中，iterator 區塊中的程式碼會轉換成狀態機器，並放在列舉值類別的 `MoveNext` 方法中。 此外，本機變數 `i` 會轉換為列舉值物件中的欄位，因此它可以在 `MoveNext`的調用之間繼續存在。

下列範例會列印整數1到10的簡單乘法表。 範例中的 `FromTo` 方法會傳回可列舉的物件，並使用 iterator 來執行。

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

`FromTo` 方法可以轉譯成編譯器所產生之可列舉類別的具現化，以將程式碼封裝在 iterator 區塊中，如下所示。

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

可列舉的類別會同時執行可列舉介面和枚舉器介面，讓它同時作為可列舉和枚舉器。 第一次叫用 `GetEnumerator` 方法時，會傳回可列舉物件本身。 可列舉物件 `GetEnumerator`的後續調用（如果有的話）會傳回可列舉物件的複本。 因此，每個傳回的列舉值都有自己的狀態，而其中一個列舉值的變更將不會影響另一個。 `Interlocked.CompareExchange` 方法是用來確保安全線程作業。

`from` 和 `to` 參數會轉換成可列舉類別中的欄位。 由於 `from` 會在 iterator 區塊中修改，因此會引進額外的 `__from` 欄位，以保存提供給每個列舉值 `from` 的初始值。

`MoveNext` 方法會在 `0``__state` 時，擲回 `InvalidOperationException`。 這可防止在沒有第一次呼叫 `GetEnumerator`的情況下，將可列舉物件當做枚舉器物件使用。

下列範例顯示簡單的樹狀目錄類別。 `Tree<T>` 類別會使用 iterator 來執行其 `GetEnumerator` 方法。 反覆運算器會以中綴順序列舉樹狀結構的元素。

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

`GetEnumerator` 方法可以轉譯為編譯器產生的列舉值類別的具現化，該類別會將程式碼封裝在 iterator 區塊中，如下所示。

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

在 `foreach` 語句中使用的編譯器產生的而暫存物件會提升至列舉值物件的 `__left` 和 `__right` 欄位。 列舉值物件的 `__state` 欄位會經過仔細更新，如此一來，如果擲回例外狀況，就會正確地呼叫正確的 `Dispose()` 方法。 請注意，您無法使用簡單的 `foreach` 語句來撰寫已轉譯的程式碼。

## <a name="async-functions"></a>Async 函數

具有 `async` 修飾詞的方法（[方法](classes.md#methods)）或匿名函式（匿名函式[運算式](expressions.md#anonymous-function-expressions)）稱為***非同步函數***。 一般來說，「***非同步***」一詞是用來描述具有 `async` 修飾詞的任何一種函數。

非同步函式的型式參數清單若要指定任何 `ref` 或 `out` 參數，就會發生編譯時期錯誤。

非同步方法的*return_type*必須是 `void` 或工作***類型***。 工作類型是 `System.Threading.Tasks.Task`，而類型是由 `System.Threading.Tasks.Task<T>`所構成。 為了簡潔起見，在本章中，這些類型分別會當做 `Task` 和 `Task<T>`參考。 傳回工作類型的非同步方法稱為「工作傳回」（task）。

工作類型的確切定義是已定義的執行，但從語言的觀點來看，工作類型的狀態為 [不完整]、[成功] 或 [錯誤]。 錯誤的工作會記錄相關的例外狀況。 成功的 `Task<T>` 會記錄類型 `T`的結果。 工作類型是可等候，因此可以是 await 運算式的運算元（[await 運算式](expressions.md#await-expressions)）。

非同步函式呼叫能夠透過其主體中的 await 運算式（[await 運算式](expressions.md#await-expressions)）來暫停評估。 稍後可以透過繼續***委派***，在暫停 await 運算式的時間點繼續評估。 繼續委派的類型為 `System.Action`，而當叫用時，非同步函式呼叫的評估會從停止的 await 運算式繼續進行。 如果函式呼叫從未暫止，或是繼續委派的最新呼叫端，則非同步函式調用的***目前呼叫***端是原始呼叫端。

### <a name="evaluation-of-a-task-returning-async-function"></a>評估工作傳回的 async 函數

叫用工作傳回的非同步函式，會產生所傳回工作類型的實例。 這稱為 async 函數***的傳回工作。*** 工作最初處於不完整的狀態。

接著會評估 async 函式主體，直到它暫停（藉由到達 await 運算式）或終止為止，此時會將控制項傳回給呼叫端，以及傳回的工作。

當 async 函式的主體終止時，傳回的工作會移出不完整的狀態：

*  如果函式主體因到達 return 語句或主體結尾而終止，則會在傳回工作中記錄任何結果值，而該工作會進入成功狀態。
*  如果函式主體因未攔截的例外狀況（[throw 語句](statements.md#the-throw-statement)）而終止，則會在傳回錯誤狀態的傳回工作中記錄例外狀況。

### <a name="evaluation-of-a-void-returning-async-function"></a>評估傳回 void 的 async 函數

如果非同步函式的傳回型別是 `void`，則評估會以下列方式與上述不同：因為不會傳回任何工作，所以函式會改為將完成和例外狀況傳達給目前線程的***同步處理內容***。 同步處理內容的確切定義與實作為相依，但表示目前線程正在執行的「位置」。 當評估傳回的非同步函式開始、成功完成，或導致擲回未攔截的例外狀況時，會通知同步處理內容。

這可讓內容追蹤在其底下執行的空傳回非同步函式數目，以及決定如何傳播來自它們的例外狀況。
