---
ms.openlocfilehash: 72d17175dfb8ef284dce6cf7e5837420fa06f16a
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488873"
---
# <a name="structs"></a>結構

結構是類似於類別，在於它們代表可以包含資料成員和函式成員的資料結構。 不過，不同於類別，結構是實值類型，而且不需要堆積配置。 結構類型的變數直接包含資料的結構，而類別類型的變數包含資料，後者稱為物件的參考。

結構特別適用於含有實值語意的小型資料結構。 複數、座標系統中的點或字典中的索引鍵/值組都是結構的良好範例。 這些資料結構的索引鍵是他們擁有較少的資料成員，它們不需要使用繼承或參考的身分識別，以及他們可以透過設定複製而非參考值的實值語意來輕鬆實作。

中所述[簡單型別](types.md#simple-types)，例如 C# 中，所提供的簡單類型`int`， `double`，和`bool`，事實上是所有結構類型。 就如同這些預先定義的型別為結構，它也可使用結構和 C# 語言中實作新的 「 基本 」 型別多載的運算子。 此章節的結尾會提供這些類型的兩個範例 ([結構範例](structs.md#struct-examples))。

## <a name="struct-declarations"></a>結構宣告

A *struct_declaration*是*type_declaration* ([型別宣告](namespaces.md#type-declarations)) 宣告新的結構：

```antlr
struct_declaration
    : attributes? struct_modifier* 'partial'? 'struct' identifier type_parameter_list?
      struct_interfaces? type_parameter_constraints_clause* struct_body ';'?
    ;
```

A *struct_declaration*組成的一組選擇性*屬性*([屬性](attributes.md))，後面接著一組選擇性的*struct_modifier*s ([結構修飾詞](structs.md#struct-modifiers))，後面接著選擇性`partial`修飾詞，後面接著關鍵字`struct`並*識別碼*可命名結構，後面接著選擇性*type_parameter_list*規格 ([型別參數](classes.md#type-parameters))，後面接著選擇性*struct_interfaces*規格 ([Partial 修飾詞](structs.md#partial-modifier)))，後面接著選擇性*type_parameter_constraints_clause*s 規格 ([類型參數條件約束](classes.md#type-parameter-constraints))，後面接著*struct_body* ([結構主體](structs.md#struct-body))，或者後面接著一個分號。

### <a name="struct-modifiers"></a>結構修飾詞

A *struct_declaration*可以選擇性地包含一連串的結構修飾詞：

```antlr
struct_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | struct_modifier_unsafe
    ;
```

它是在結構宣告中出現多次相同的修飾詞的編譯時期錯誤。

結構宣告的修飾詞具有相同的意義的類別宣告 ([類別宣告](classes.md#class-declarations))。

### <a name="partial-modifier"></a>Partial 修飾詞

`partial`修飾詞表示這*struct_declaration*是部分的型別宣告。 多個具有相同的名稱，在封入的命名空間或類型宣告中的部分結構宣告結合以形成一個結構宣告，下列規則中指定[部分型別](classes.md#partial-types)。

### <a name="struct-interfaces"></a>結構的介面

結構宣告可能包含*struct_interfaces*規格，稱為直接實作特定的介面型別案例結構。

```antlr
struct_interfaces
    : ':' interface_type_list
    ;
```

介面實作討論中進一步[介面實作](interfaces.md#interface-implementations)。

### <a name="struct-body"></a>結構主體

*Struct_body*結構會定義結構的成員。

```antlr
struct_body
    : '{' struct_member_declaration* '}'
    ;
```

## <a name="struct-members"></a>結構成員

結構的成員包含所引入的成員及其*struct_member_declaration*s 和成員繼承自型別`System.ValueType`。

```antlr
struct_member_declaration
    : constant_declaration
    | field_declaration
    | method_declaration
    | property_declaration
    | event_declaration
    | indexer_declaration
    | operator_declaration
    | constructor_declaration
    | static_constructor_declaration
    | type_declaration
    | struct_member_declaration_unsafe
    ;
```

除了中所述的差異[類別和結構的差異](structs.md#class-and-struct-differences)中, 提供的類別成員的說明[類別成員](classes.md#class-members)透過[迭代器](classes.md#iterators)套用至結構以及成員。

## <a name="class-and-struct-differences"></a>類別和結構的差異

與類別的結構是在數個重要方面不同：

*  結構是實值型別 ([值語意](structs.md#value-semantics))。
*  所有結構類型都隱含地都繼承自類別`System.ValueType`([繼承](structs.md#inheritance))。
*  指派給變數的結構類型會建立一份所指派的值 ([指派](structs.md#assignment))。
*  結構的預設值是藉由將所有實值型別欄位設定為其預設值和所有參考型別欄位為產生的值`null`([預設值](structs.md#default-values))。
*  Boxing 和 unboxing 作業可用於結構類型之間轉換以及`object`([Boxing 和 unboxing](structs.md#boxing-and-unboxing))。
*  意義`this`是不同的結構 ([這項存取](expressions.md#this-access))。
*  結構的執行個體欄位宣告不得包含變數的初始設定式 ([欄位初始設定式](structs.md#field-initializers))。
*  結構不允許宣告無參數的執行個體建構函式 ([建構函式](structs.md#constructors))。
*  結構不允許宣告解構函式 ([解構函式](structs.md#destructors))。

### <a name="value-semantics"></a>實值語意

結構是實值型別 ([實值型別](types.md#value-types)) 並被視為具有實值語意。 類別，相反地，是參考型別 ([參考的型別](types.md#reference-types)) 並被視為具有參考語意。

結構類型的變數直接包含資料的結構，而類別類型的變數包含資料，後者稱為物件的參考。 當結構`B`包含的型別執行個體欄位`A`和`A`是結構類型時，會產生編譯時期錯誤`A`取決於`B`或從類型建構`B`。 結構`X`***直接相依於***結構`Y`如果`X`包含類型的執行個體欄位`Y`。 如果已指定這個定義，一組完整的結構所依存的結構是可轉移關閉***直接相依於***關聯性。  例如
```csharp
struct Node
{
    int data;
    Node next; // error, Node directly depends on itself
}
```
會發生錯誤，因為`Node`包含其本身類型的執行個體欄位。  另一個範例
```csharp
struct A { B b; }

struct B { C c; }

struct C { A a; }
```
會發生錯誤，因為每個型別`A`， `B`，和`C`彼此相依。

類別中，很可能兩個變數來參考相同的物件，且因此可能會影響其他變數所參考的物件的一個變數上進行的作業。 使用結構時，每個變數都有自己的資料複本 (的情況除外`ref`和`out`參數變數)，而且不可能會影響其他的其中一個上的作業。 此外，因為結構不是參考型別，它不是結構類型的值可能`null`。

指定的宣告
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
程式碼片段
```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 100;
System.Console.WriteLine(b.x);
```
輸出值`10`。 指派`a`來`b`會建立一份值，並`b`不會因此受到指派給`a.x`。 有`Point`改為已宣告為類別，則輸出會是`100`因為`a`和`b`會參考相同的物件。

### <a name="inheritance"></a>繼承

所有結構類型都隱含地都繼承自類別`System.ValueType`，它會接著繼承自類別`object`。 結構宣告可能會指定一份實作的介面，但它並不適用結構宣告，以指定的基底類別。

結構類型不能為抽象，並一律隱含地密封。 `abstract`和`sealed`結構宣告中也因此不允許修飾詞。

因為結構不支援繼承，結構成員的宣告存取範圍不能`protected`或`protected internal`。

在結構中的函式成員不能`abstract`或`virtual`，而`override`修飾詞只允許對覆寫方法繼承自`System.ValueType`。

### <a name="assignment"></a>指派

指派給變數的結構類型會建立一份所指派的值。 這不同於指派給變數的類別類型時，它將會複製參考，而不是參考所識別的物件。

類似於指派，結構是做為值參數傳遞或傳回為函式成員的結果時，將會建立結構的複本。 可能使用函式成員的參考所傳遞的結構`ref`或`out`參數。

當屬性或索引子的結構指派的目標，執行個體相關聯的運算式屬性或索引子存取必須歸類為變數。 如果執行個體運算式分類為值，就會發生編譯時期錯誤。 這會進一步詳細說明[簡單指派](expressions.md#simple-assignment)。

### <a name="default-values"></a>預設值

中所述[預設值](variables.md#default-values)，有數種變數會自動初始化為其預設值建立時。 類別型別和其他參考型別變數，此預設值是`null`。 不過，因為結構是實值型別，不能`null`，預設值的結構會將所有實值型別欄位設定為其預設值和所有參考型別欄位為所產生的值`null`。

參考`Point`上述的範例中，宣告結構
```csharp
Point[] a = new Point[100];
```
初始化每個`Point`陣列中要設定所產生的值`x`和`y`欄位設為零。

結構的預設值對應至結構中的預設建構函式所傳回的值 ([預設建構函式](types.md#default-constructors))。 不同於類別中，結構不是允許宣告無參數的執行個體建構函式。 相反地，每個結構會隱含地具有無參數的執行個體建構函式一律會傳回值所產生將所有實值型別欄位設定為其預設值和所有參考型別欄位為`null`。

結構應該設計成有效的狀態，請考慮預設的初始化狀態。 在範例
```csharp
using System;

struct KeyValuePair
{
    string key;
    string value;

    public KeyValuePair(string key, string value) {
        if (key == null || value == null) throw new ArgumentException();
        this.key = key;
        this.value = value;
    }
}
```
使用者定義的執行個體建構函式可防止只呼叫它的位置明確的 null 值。 萬一其中`KeyValuePair`變數預設值初始化，可能受到`key`和`value`欄位將會是 null，和結構必須準備好處理這個狀態。

### <a name="boxing-and-unboxing"></a>Boxing 和 Unboxing

類別類型的值可以轉換成輸入`object`或由類別實作，只要參考視為另一種類型在編譯時期為介面類型。 同樣地，類型的值`object`或介面類型的值可以轉換回類別型別，而不需要變更參考 （但當然的執行階段類型檢查需要在此情況下）。

因為結構不是參考型別，這些作業的結構類型的實作方式不同。 結構類型的值會轉換成類型`object`或結構實作介面型別，boxing 作業就會發生。 同樣地，當類型的值`object`或介面類型的值會轉換成結構的型別，unboxing 作業會發生。 從相同的作業，在類別類型上的主要差異是，boxing 和 unboxing 結構會將值複製到或已封裝的執行個體。 因此，下列 boxing 或 unboxing 作業時，unboxed 結構所做的變更不會反映在 boxed 結構中。

當結構類型會覆寫虛擬方法繼承自`System.Object`(例如`Equals`， `GetHashCode`，或`ToString`)，透過結構類型的執行個體的虛擬方法的引動過程並不會發生 boxing。 即使當類別可用來做為型別參數，並透過型別參數類型的執行個體的引動過程，也是如此。 例如：
```csharp
using System;

struct Counter
{
    int value;

    public override string ToString() {
        value++;
        return value.ToString();
    }
}

class Program
{
    static void Test<T>() where T: new() {
        T x = new T();
        Console.WriteLine(x.ToString());
        Console.WriteLine(x.ToString());
        Console.WriteLine(x.ToString());
    }

    static void Main() {
        Test<Counter>();
    }
}
```

程式輸出為：
```
1
2
3
```

雖然它是不正確樣式`ToString`有副作用，此範例會示範的三個引動過程，發生任何 boxing `x.ToString()`。

同樣地，永遠不會隱含 boxing 發生於存取受條件約束的型別參數上的成員。 例如，假設介面`ICounter`包含的方法`Increment`可用來修改值。 如果`ICounter`做為條件約束 」，這是實作`Increment`方法呼叫變數的參考，`Increment`呼叫永遠不會經過 boxing 處理的複本上。

```csharp
using System;

interface ICounter
{
    void Increment();
}

struct Counter: ICounter
{
    int value;

    public override string ToString() {
        return value.ToString();
    }

    void ICounter.Increment() {
        value++;
    }
}

class Program
{
    static void Test<T>() where T: ICounter, new() {
        T x = new T();
        Console.WriteLine(x);
        x.Increment();                    // Modify x
        Console.WriteLine(x);
        ((ICounter)x).Increment();        // Modify boxed copy of x
        Console.WriteLine(x);
    }

    static void Main() {
        Test<Counter>();
    }
}
```

第一次呼叫`Increment`變數中的值會修改`x`。 這並不等於第二次呼叫`Increment`，這會修改 boxed 複本中的值`x`。 因此，程式的輸出為：
```
0
1
1
```

如需 boxing 和 unboxing 的進一步詳細資訊，請參閱 < [Boxing 和 unboxing](types.md#boxing-and-unboxing)。

### <a name="meaning-of-this"></a>這個意義

執行個體建構函式或類別的執行個體函式成員內`this`會分類為值。 因此，雖然`this`可用來參考執行個體的函式成員已叫用，就無法將指派給`this`函式成員的類別。

在結構中，執行個體建構函式內`this`對應至`out`結構型別的和結構的執行個體函式成員內的參數`this`對應至`ref`結構類型的參數。 在這兩種情況下，`this`歸類為變數，您也可以修改整個結構指派給已叫用函式成員`this`或藉由傳遞為`ref`或`out`參數。

### <a name="field-initializers"></a>欄位初始設定式

中所述[預設值](structs.md#default-values)，將所有實值型別欄位設定為其預設值和所有參考型別欄位為產生的值包含結構的預設值`null`。 基於這個理由，結構不允許包含變數的初始設定式的執行個體欄位宣告。 這項限制只適用於執行個體欄位。 結構的靜態欄位可以包含變數的初始設定式。

此範例
```csharp
struct Point
{
    public int x = 1;  // Error, initializer not permitted
    public int y = 1;  // Error, initializer not permitted
}
```
處於錯誤，因為執行個體欄位宣告包含變數的初始設定式。

### <a name="constructors"></a>建構函式

不同於類別中，結構不是允許宣告無參數的執行個體建構函式。 相反地，每個結構會隱含地具有無參數的執行個體建構函式一律會傳回所產生將所有實值型別欄位設定為其預設值和所有參考為 null 的型別欄位的值 ([預設建構函式](types.md#default-constructors)). 結構可以宣告具有參數的執行個體建構函式。 例如
```csharp
struct Point
{
    int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```

指定上述宣告中，陳述式
```csharp
Point p1 = new Point();
Point p2 = new Point(0, 0);
```
兩者皆可建立`Point`具有`x`和`y`初始化為零。

結構執行個體建構函式不允許包含表單的建構函式初始設定式`base(...)`。

如果結構執行個體建構函式未指定的建構函式初始設定式中，`this`變數對應至`out`結構型別的和類似的參數`out`參數，`this`必須明確地指派 （[明確指派](variables.md#definite-assignment)) 在其中建構函式會傳回每個位置。 如果結構執行個體建構函式指定的建構函式初始設定式中，`this`變數對應至`ref`結構型別的和類似的參數`ref`參數，`this`會被視為已明確指派上要建構函式主體的項目。 請考慮以下的執行個體建構函式實作：
```csharp
struct Point
{
    int x, y;

    public int X {
        set { x = value; }
    }

    public int Y {
        set { y = value; }
    }

    public Point(int x, int y) {
        X = x;        // error, this is not yet definitely assigned
        Y = y;        // error, this is not yet definitely assigned
    }
}
```

任何執行個體成員函式 (包括屬性 set 存取子`X`和`Y`) 可以呼叫，直到明確指派所建構之結構的所有欄位。 唯一的例外狀況包含自動實作的屬性 ([自動實作屬性](classes.md#automatically-implemented-properties))。 明確指派規則 ([簡單的指派運算式](variables.md#simple-assignment-expressions)) 特別排除指派 auto 屬性的結構類型的執行個體建構函式內，該結構類型： 此類指派會被視為明確auto 屬性的隱藏的支援欄位的指派。 因此，以下被允許的：

```csharp
struct Point
{
    public int X { get; set; }
    public int Y { get; set; }

    public Point(int x, int y) {
        X = x;      // allowed, definitely assigns backing field
        Y = y;      // allowed, definitely assigns backing field
    }
```

### <a name="destructors"></a>解構函式

結構不是允許宣告解構函式。

### <a name="static-constructors"></a>靜態建構函式

結構的靜態建構函式會依照大多與類別相同的規則。 結構類型的靜態建構函式的執行是由應用程式定義域中發生下列事件的第一個觸發：

*  參考的結構類型的靜態成員。
*  結構類型的明確宣告建構函式會呼叫。

建立的預設值 ([預設值](structs.md#default-values)) 的結構類型不會觸發靜態建構函式。 （這個範例是陣列中元素的初始值）。

## <a name="struct-examples"></a>結構範例

下圖顯示兩個重要的使用範例`struct`型別，以建立可以用來預先定義的型別語言，但具有已修改的語意類似的類型。

### <a name="database-integer-type"></a>資料庫的整數型別

`DBInt`結構的實作，可代表一組完整的值的整數類型`int`型別，再加上額外的狀態，指出未知的值。 具有這些特性的型別常用的資料庫。

```csharp
using System;

public struct DBInt
{
    // The Null member represents an unknown DBInt value.

    public static readonly DBInt Null = new DBInt();

    // When the defined field is true, this DBInt represents a known value
    // which is stored in the value field. When the defined field is false,
    // this DBInt represents an unknown value, and the value field is 0.

    int value;
    bool defined;

    // Private instance constructor. Creates a DBInt with a known value.

    DBInt(int value) {
        this.value = value;
        this.defined = true;
    }

    // The IsNull property is true if this DBInt represents an unknown value.

    public bool IsNull { get { return !defined; } }

    // The Value property is the known value of this DBInt, or 0 if this
    // DBInt represents an unknown value.

    public int Value { get { return value; } }

    // Implicit conversion from int to DBInt.

    public static implicit operator DBInt(int x) {
        return new DBInt(x);
    }

    // Explicit conversion from DBInt to int. Throws an exception if the
    // given DBInt represents an unknown value.

    public static explicit operator int(DBInt x) {
        if (!x.defined) throw new InvalidOperationException();
        return x.value;
    }

    public static DBInt operator +(DBInt x) {
        return x;
    }

    public static DBInt operator -(DBInt x) {
        return x.defined ? -x.value : Null;
    }

    public static DBInt operator +(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value + y.value: Null;
    }

    public static DBInt operator -(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value - y.value: Null;
    }

    public static DBInt operator *(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value * y.value: Null;
    }

    public static DBInt operator /(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value / y.value: Null;
    }

    public static DBInt operator %(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value % y.value: Null;
    }

    public static DBBool operator ==(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value == y.value: DBBool.Null;
    }

    public static DBBool operator !=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value != y.value: DBBool.Null;
    }

    public static DBBool operator >(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value > y.value: DBBool.Null;
    }

    public static DBBool operator <(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value < y.value: DBBool.Null;
    }

    public static DBBool operator >=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value >= y.value: DBBool.Null;
    }

    public static DBBool operator <=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value <= y.value: DBBool.Null;
    }

    public override bool Equals(object obj) {
        if (!(obj is DBInt)) return false;
        DBInt x = (DBInt)obj;
        return value == x.value && defined == x.defined;
    }

    public override int GetHashCode() {
        return value;
    }

    public override string ToString() {
        return defined? value.ToString(): "DBInt.Null";
    }
}
```

### <a name="database-boolean-type"></a>資料庫布林型別

`DBBool`結構實作的三種值的邏輯類型。 可能的值，這個型別的`DBBool.True`， `DBBool.False`，並`DBBool.Null`，其中`Null`成員表示未知的值。 這種三值邏輯的類型通常會用於資料庫。

```csharp
using System;

public struct DBBool
{
    // The three possible DBBool values.

    public static readonly DBBool Null = new DBBool(0);
    public static readonly DBBool False = new DBBool(-1);
    public static readonly DBBool True = new DBBool(1);

    // Private field that stores -1, 0, 1 for False, Null, True.

    sbyte value;

    // Private instance constructor. The value parameter must be -1, 0, or 1.

    DBBool(int value) {
        this.value = (sbyte)value;
    }

    // Properties to examine the value of a DBBool. Return true if this
    // DBBool has the given value, false otherwise.

    public bool IsNull { get { return value == 0; } }

    public bool IsFalse { get { return value < 0; } }

    public bool IsTrue { get { return value > 0; } }

    // Implicit conversion from bool to DBBool. Maps true to DBBool.True and
    // false to DBBool.False.

    public static implicit operator DBBool(bool x) {
        return x? True: False;
    }

    // Explicit conversion from DBBool to bool. Throws an exception if the
    // given DBBool is Null, otherwise returns true or false.

    public static explicit operator bool(DBBool x) {
        if (x.value == 0) throw new InvalidOperationException();
        return x.value > 0;
    }

    // Equality operator. Returns Null if either operand is Null, otherwise
    // returns True or False.

    public static DBBool operator ==(DBBool x, DBBool y) {
        if (x.value == 0 || y.value == 0) return Null;
        return x.value == y.value? True: False;
    }

    // Inequality operator. Returns Null if either operand is Null, otherwise
    // returns True or False.

    public static DBBool operator !=(DBBool x, DBBool y) {
        if (x.value == 0 || y.value == 0) return Null;
        return x.value != y.value? True: False;
    }

    // Logical negation operator. Returns True if the operand is False, Null
    // if the operand is Null, or False if the operand is True.

    public static DBBool operator !(DBBool x) {
        return new DBBool(-x.value);
    }

    // Logical AND operator. Returns False if either operand is False,
    // otherwise Null if either operand is Null, otherwise True.

    public static DBBool operator &(DBBool x, DBBool y) {
        return new DBBool(x.value < y.value? x.value: y.value);
    }

    // Logical OR operator. Returns True if either operand is True, otherwise
    // Null if either operand is Null, otherwise False.

    public static DBBool operator |(DBBool x, DBBool y) {
        return new DBBool(x.value > y.value? x.value: y.value);
    }

    // Definitely true operator. Returns true if the operand is True, false
    // otherwise.

    public static bool operator true(DBBool x) {
        return x.value > 0;
    }

    // Definitely false operator. Returns true if the operand is False, false
    // otherwise.

    public static bool operator false(DBBool x) {
        return x.value < 0;
    }

    public override bool Equals(object obj) {
        if (!(obj is DBBool)) return false;
        return value == ((DBBool)obj).value;
    }

    public override int GetHashCode() {
        return value;
    }

    public override string ToString() {
        if (value > 0) return "DBBool.True";
        if (value < 0) return "DBBool.False";
        return "DBBool.Null";
    }
}
```
