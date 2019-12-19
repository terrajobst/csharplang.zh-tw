---
ms.openlocfilehash: 6dd1dde67597b2125de9a1aa2fab9144128d533f
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704034"
---
# <a name="structs"></a>結構

結構類似于類別，它們代表可包含資料成員和函式成員的資料結構。 不過，不同于類別，結構是實數值型別，不需要堆積配置。 結構型別的變數直接包含結構的資料，而類別型別的變數則包含資料的參考，後者稱為物件。

結構特別適用於含有實值語意的小型資料結構。 複數、座標系統中的點或字典中的索引鍵/值組都是結構的良好範例。 這些資料結構的關鍵在於它們有幾個資料成員，不需要使用繼承或參考身分識別，而且可以使用值語義輕鬆地實作為，其中指派會複製值，而不是參考。

如[簡單](types.md#simple-types)型別中所述，所提供C#的簡單型別（例如 `int`、`double`和 `bool`）實際上是所有結構類型。 就像這些預先定義的類型是結構一樣，也可以使用結構和運算子多載，以語言來執行新的C# 「基本」類型。 這類類型的兩個範例會在本章結尾處提供（[結構範例](structs.md#struct-examples)）。

## <a name="struct-declarations"></a>結構宣告

*Struct_declaration*是宣告新結構的*type_declaration* （[型](namespaces.md#type-declarations)別宣告）：

```antlr
struct_declaration
    : attributes? struct_modifier* 'partial'? 'struct' identifier type_parameter_list?
      struct_interfaces? type_parameter_constraints_clause* struct_body ';'?
    ;
```

*Struct_declaration*由一組選擇性的*屬性*（[屬性](attributes.md)）組成，後面接著一組選擇性的*struct_modifier*s （[結構修飾詞](structs.md#struct-modifiers)），後面接著選擇性的 `partial` 修飾詞，後面接著關鍵字 `struct` 和命名結構的*識別碼*，接著*是選擇性的* *type_parameter_list 規格（* [部分修飾](structs.md#partial-modifier)詞），後面接著選擇性的*type_parameter_constraints_clause*s 規格（[[類型參數](classes.md#type-parameters)條件約束](classes.md#type-parameter-constraints)），後面接著*struct_body* （[結構主體](structs.md#struct-body)），並選擇性地加上分號。

### <a name="struct-modifiers"></a>結構修飾詞

*Struct_declaration*可以選擇性地包含一連串的結構修飾詞：

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

在結構宣告中多次出現相同的修飾詞時，就會發生編譯時期錯誤。

結構宣告的修飾詞與類別宣告的修飾詞（[類別](classes.md#class-declarations)宣告）具有相同的意義。

### <a name="partial-modifier"></a>Partial 修飾詞

`partial` 修飾詞表示此*struct_declaration*是部分類型宣告。 在封入命名空間或類型宣告中，具有相同名稱的多個部分結構宣告會結合成一個結構宣告，遵循[部分類型](classes.md#partial-types)中指定的規則。

### <a name="struct-interfaces"></a>結構介面

結構宣告可以包含*struct_interfaces*規格，在此情況下，會將結構視為直接實作為指定的介面類別型。

```antlr
struct_interfaces
    : ':' interface_type_list
    ;
```

介面的執行會在[介面實現](interfaces.md#interface-implementations)中進一步討論。

### <a name="struct-body"></a>結構主體

結構的*struct_body*定義結構的成員。

```antlr
struct_body
    : '{' struct_member_declaration* '}'
    ;
```

## <a name="struct-members"></a>結構成員

結構的成員是由其*struct_member_declaration*所引進的成員，以及繼承自類型 `System.ValueType`的成員所組成。

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

除了[類別和結構差異](structs.md#class-and-struct-differences)中所述的差異之外，[類別成員](classes.md#class-members)透過[反覆運算](classes.md#iterators)器所提供的類別成員描述也適用于結構成員。

## <a name="class-and-struct-differences"></a>類別和結構的差異

結構與類別有幾個重要的差異：

*  結構是實數值型別（[值語義](structs.md#value-semantics)）。
*  所有結構類型都會隱含繼承自類別 `System.ValueType` （[繼承](structs.md#inheritance)）。
*  指派至結構類型的變數會建立所指派值的複本（[指派](structs.md#assignment)）。
*  結構的預設值是將所有實值型別字段設定為其預設值，以及要 `null` 的所有參考型別字段（[預設值](structs.md#default-values)）所產生的值。
*  使用裝箱和取消裝箱作業來轉換結構類型和 `object` （[裝箱和取消裝箱](structs.md#boxing-and-unboxing)）。
*  結構（[此存取](expressions.md#this-access)）的 `this` 意義不同。
*  不允許結構的實例欄位宣告包含變數初始化運算式（[欄位初始化運算式](structs.md#field-initializers)）。
*  不允許結構宣告無參數實例的函式（[構造](structs.md#constructors)函式）。
*  不允許結構宣告析構函數（[析構](structs.md#destructors)函式）。

### <a name="value-semantics"></a>值的語義

結構是實值型別（實[值](types.md#value-types)型別），也稱為具有值的語義。 另一方面，類別是參考型別（[參考](types.md#reference-types)型別），也稱為具有參考語義。

結構型別的變數直接包含結構的資料，而類別型別的變數則包含資料的參考，後者稱為物件。 當結構 `B` 包含型別 `A` 的實例欄位，而 `A` 是結構型別時，`A` 相依于 `B` 或從 `B`所構成的型別，就會發生編譯時期錯誤。 結構 `X`***直接相依于***結構 `Y` 如果 `X` 包含 `Y`類型的實例欄位。 根據這個定義，結構所相依的完整結構集是***直接相依于***關聯性的可轉移關閉。  例如
```csharp
struct Node
{
    int data;
    Node next; // error, Node directly depends on itself
}
```
是錯誤，因為 `Node` 包含其本身類型的實例欄位。  另一個範例
```csharp
struct A { B b; }

struct B { C c; }

struct C { A a; }
```
是錯誤，因為每個類型 `A`、`B`和 `C` 彼此相依。

使用類別時，兩個變數可能會參考相同的物件，因此，對某個變數的作業可能會影響另一個變數所參考的物件。 使用結構時，每個變數都有自己的資料複本（除了 `ref` 和 `out` 參數變數以外），而且不可能對其中一個作業影響另一個。 此外，因為結構不是參考型別，所以不可能 `null`結構型別的值。

指定宣告
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
輸出 `10`的值。 `a` 指派給 `b` 會建立值的複本，而 `b` 則不會受到指派至 `a.x`的影響。 已將 `Point` 宣告為類別，所以會 `100` 輸出，因為 `a` 和 `b` 會參考相同的物件。

### <a name="inheritance"></a>繼承

所有結構類型都隱含繼承自類別 `System.ValueType`，而後者又繼承自類別 `object`。 結構宣告可以指定實作為實介面的清單，但結構宣告不可能指定基類。

結構類型絕對不會是抽象的，而且一律會隱含地密封。 因此，在結構宣告中不允許 `abstract` 和 `sealed` 修飾詞。

因為結構不支援繼承，所以無法 `protected` 或 `protected internal`結構成員的宣告存取範圍。

結構中的函式成員不能 `abstract` 或 `virtual`，而且 `override` 修飾詞只能用來覆寫繼承自 `System.ValueType`的方法。

### <a name="assignment"></a>指派

指派至結構類型的變數會建立所指派值的複本。 這與指派給類別類型的變數不同，後者會複製參考，而不是參考所識別的物件。

類似于指派，當結構當做值參數傳遞或當做函數成員的結果傳回時，會建立結構的複本。 您可以使用 `ref` 或 `out` 參數，以傳址方式將結構傳遞給函式成員。

當結構的屬性或索引子是指派的目標時，與屬性或索引子存取相關聯的實例運算式必須分類為變數。 如果實例運算式分類為值，就會發生編譯時期錯誤。 這在[簡單指派](expressions.md#simple-assignment)中會進一步詳細說明。

### <a name="default-values"></a>預設值

如[預設值](variables.md#default-values)所述，在建立時，會將數種變數自動初始化為其預設值。 對於類別類型和其他參考型別的變數，此預設值為 `null`。 不過，因為結構是無法 `null`的實值型別，所以結構的預設值就是將所有實值型別字段設定為其預設值，以及要 `null`的所有參考型別字段所產生的值。

參考上面所宣告的 `Point` 結構，範例
```csharp
Point[] a = new Point[100];
```
將陣列中的每個 `Point` 初始化為將 `x` 和 `y` 欄位設定為零所產生的值。

結構的預設值會對應至結構的預設函式所傳回的值（[預設](types.md#default-constructors)的函式）。 不同于類別，結構不允許宣告無參數的實例的函式。 取而代之的是，每個結構都會隱含地具有無參數的實例函式，而這個值一律會傳回將所有實值型別字段設定為預設值，以及要 `null`的所有參考型別字段所產生的

結構應該設計成將預設的初始化狀態視為有效的狀態。 在範例中
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
使用者定義的實例的函式只會在明確呼叫它時，保護其是否為 null 值。 如果 `KeyValuePair` 變數受限於預設值初始化，[`key`] 和 [`value`] 欄位將會是 null，且結構必須準備好處理此狀態。

### <a name="boxing-and-unboxing"></a>Boxing 和 Unboxing

類別類型的值可以轉換成類型 `object` 或類別的介面類別型，方法是在編譯時期將參考視為另一個類型來執行。 同樣地，`object` 類型的值或介面類別型的值可以轉換回類別類型，而不需要變更參考（但在此情況下，需要執行時間類型檢查）。

因為結構不是參考型別，所以這些作業會針對結構型別以不同的方式執行。 當結構類型的值轉換成類型 `object` 或結構所實作為介面類別型時，就會進行「裝箱」作業。 同樣地，當類型的值 `object` 或介面類別型的值轉換回結構類型時，就會進行取消程式作業。 與類別類型上相同作業的主要差異在於，裝箱和取消裝箱會將結構值複製到或傳出已裝箱的實例。 因此，在裝箱或取消封裝作業之後，對取消裝箱結構所做的變更不會反映在已裝箱的結構中。

當結構類型覆寫繼承自 `System.Object` 的虛擬方法時（例如 `Equals`、`GetHashCode`或 `ToString`），透過結構類型的實例叫用虛擬方法，並不會導致進行裝箱。 即使結構是當做型別參數使用，而且會透過型別參數型別的實例進行調用，也是如此。 例如：
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

程式的輸出為：
```console
1
2
3
```

雖然 `ToString` 具有副作用的樣式不正確，但此範例會示範 `x.ToString()`的三個調用中未發生任何裝箱。

同樣地，存取限制型別參數上的成員時，也不會隱含地進行裝箱。 例如，假設介面 `ICounter` 包含可用於修改值的方法 `Increment`。 如果 `ICounter` 當做條件約束使用，則會使用對所呼叫之 `Increment` 變數的參考來呼叫 `Increment` 方法的實，而不是已封裝的複本。

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

第一次呼叫 `Increment` 會修改變數 `x`中的值。 這不等於 `Increment`的第二個呼叫，它會修改 `x`的已封裝複本中的值。 因此，程式的輸出為：
```console
0
1
1
```

如需有關裝箱和取消裝箱的進一步詳細資料，請參閱[裝箱和取消](types.md#boxing-and-unboxing)裝箱。

### <a name="meaning-of-this"></a>意義

在類別的實例函式或實例函數成員內，`this` 會分類為值。 因此，雖然 `this` 可以用來參考叫用函式成員的實例，但無法在類別的函式成員中指派給 `this`。

在結構的實例函式中，`this` 對應至結構類型的 `out` 參數，而且在結構的實例函式成員中，`this` 對應至結構類型的 `ref` 參數。 在這兩種情況下，`this` 都會分類為變數，而且可以藉由指派給 `this` 或將它傳遞為 `ref` 或 `out` 參數，來修改叫用函式成員的整個結構。

### <a name="field-initializers"></a>欄位初始化運算式

如[預設值](structs.md#default-values)所述，結構的預設值包含將所有數值型別欄位設定為其預設值，以及要 `null`的所有參考類型欄位所產生的值。 基於這個理由，結構不允許實例欄位宣告包含變數初始化運算式。 這種限制僅適用于實例欄位。 結構的靜態欄位允許包含變數初始化運算式。

範例
```csharp
struct Point
{
    public int x = 1;  // Error, initializer not permitted
    public int y = 1;  // Error, initializer not permitted
}
```
發生錯誤，因為實例欄位宣告包含變數初始化運算式。

### <a name="constructors"></a>建構函式

不同于類別，結構不允許宣告無參數的實例的函式。 相反地，每個結構隱含具有無參數實例的函式，其一律會傳回將所有數值型別欄位設定為預設值，以及所有參考類型欄位為 null （[預設](types.md#default-constructors)的函式）所產生的值。 結構可以宣告具有參數的實例構造函式。 例如
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

假設上述宣告，語句
```csharp
Point p1 = new Point();
Point p2 = new Point(0, 0);
```
兩者都使用 `x` 建立 `Point`，並 `y` 初始化為零。

結構實例的函式不允許包含 `base(...)`格式的函式初始化運算式。

如果結構實例的建立式不會指定函式初始化運算式，則 `this` 變數會對應至結構類型的 `out` 參數，而且類似于 `out` 參數，`this` 必須在此函式所傳回的每個位置明確指派（[明確](variables.md#definite-assignment)指派）。 如果結構實例的函式指定了一個函式初始化運算式，則 `this` 變數會對應至結構類型的 `ref` 參數，而且類似于 `ref` 參數，`this` 會被視為在對函式主體的專案上明確指派。 請考慮下列的實例函式執行：
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

您必須先明確指派結構的所有欄位，才能呼叫實例成員函式（包括屬性 `X` 和 `Y`的 set 存取子）。 唯一的例外狀況包括自動實作為屬性（[自動實作為屬性](classes.md#automatically-implemented-properties)）。 明確指派規則（[簡單指派運算式](variables.md#simple-assignment-expressions)）會特別豁免指派給該結構型別之實例函式中結構型別的自動屬性（property），這種指派會被視為自動屬性的隱藏支援欄位的明確指派。 因此，允許下列各項：

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

不允許結構宣告析構函式。

### <a name="static-constructors"></a>靜態建構函式

結構的靜態構造函式會遵循與類別相同的大部分規則。 結構類型的靜態函式執行是由下列事件中的第一個所觸發，以在應用程式域內發生：

*  會參考結構類型的靜態成員。
*  會呼叫結構類型的明確宣告的函式。

建立結構類型的預設值（[預設值](structs.md#default-values)）並不會觸發靜態的函式。 （其中一個範例是陣列中元素的初始值）。

## <a name="struct-examples"></a>結構範例

以下顯示兩個使用 `struct` 類型來建立類型的重要範例，其使用方式類似于預先定義的語言類型，但具有修改過的語義。

### <a name="database-integer-type"></a>資料庫整數類型

下列 `DBInt` 結構會執行整數類型，可以代表一組完整的 `int` 類型值，再加上表示未知值的其他狀態。 具有這些特性的類型通常用於資料庫中。

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

### <a name="database-boolean-type"></a>資料庫布林值類型

下列 `DBBool` 結構會執行三個值的邏輯型別。 此類型的可能值為 `DBBool.True`、`DBBool.False`和 `DBBool.Null`，其中 `Null` 成員會指出未知的值。 這類三值邏輯類型通常用於資料庫中。

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
