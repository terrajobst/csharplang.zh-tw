---
ms.openlocfilehash: 49720d62c496ff904eacacb31ae29ef97a5aefaa
ms.sourcegitcommit: 8152182f0a477cb3082e625b607262cc459a17f3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/23/2019
ms.locfileid: "79484941"
---
# <a name="records"></a>記錄

* [x] 提議
* [] 原型：[完成](https://github.com/PROTOTYPE_OWNER/roslyn/BRANCH_NAME)
* [] 執行[中：進行中](https://github.com/dotnet/roslyn/BRANCH_NAME)
* [] 規格：包含的草稿規格

## <a name="summary"></a>摘要
[summary]: #summary

記錄是一種新的簡化宣告形式C# ，適用于類別和結構類型，可結合一些較簡單功能的優點。 我們會描述新的功能（呼叫端-接收者參數和*with-expression*s）、提供記錄宣告的語法和語義，然後提供一些範例。


## <a name="motivation"></a>動機
[motivation]: #motivation

中C#的大量型別宣告，只是類型資料的匯總集合而已。 可惜的是，宣告這類類型需要大量的重複使用程式碼。 *記錄*提供一種機制來宣告資料類型，方法是描述匯總的成員以及其他程式碼，或與一般的重複使用方式偏差（如果有的話）。

請參閱下面的[範例](#examples)。

## <a name="detailed-design"></a>詳細設計
[design]: #detailed-design

### <a name="caller-receiver-parameters"></a>呼叫端-接收者參數

目前，方法參數的*預設引數*必須是 
- *常數運算式*;或
- 表單 `new S()` 的運算式，其中 `S` 是實值型別;或
- 表單 `default(S)` 的運算式，其中 `S` 是實數值型別

我們會擴充此項來新增下列
- 格式為的運算式 `this.Identifier`

這個新的表單稱為*呼叫者-接收者的預設引數*，只有在滿足下列所有條件時，才允許使用
- 其出現的方法是實例方法;和
- 運算式 `this.Identifier` 系結至封閉類型的實例成員，必須是欄位或屬性;和
- 它所系結的成員（以及 `get` 存取子（如果它是屬性），至少會如同方法一樣存取。和
- `this.Identifier` 的類型可透過身分識別或可為 null 的轉換為參數類型（這是*預設引數*上的現有條件約束）隱含轉換。

當具有*呼叫端預設引數*之對應選擇性參數的函式成員調用中省略引數時，會隱含地傳遞接收者成員的值。 

> **設計附注**：呼叫端-接收者參數的主要原因是要支援*with 運算式*。 其概念是，您可以宣告如下的方法
> ```cs
> class Point
> {
>     public readonly int X;
>     public readonly int Y;
>     public Point With(int x = this.X, int y = this.Y) => new Point(x, y);
>     // etc
> }
> ```
> 然後使用它，如下所示
> ```cs
>     Point p = new Point(3, 4);
>     p = p.With(x: 1);
> ```
> 若要建立新的 `Point`，如同現有 `Point`，但已變更 `X` 的值。
> 
> 這是一個開放問題，不論是否有使用呼叫端接收者參數的支援，都需要加上*運算式*的語法形式，因此我們可以執行此動作，*而*不是*除了* *with-運算式*以外。

- []**開啟問題**：針對其他引數評估*呼叫者-接收者的預設引數*的順序為何？ 我們是否應該指明它是未指定的？

### <a name="with-expressions"></a>with-運算式

建議使用新的運算式表單：

```antlr
primary_expression
    : with_expression
    ;

with_expression
    : primary_expression 'with' '{' with_initializer_list '}'
    ;

with_initializer_list
    : with_initializer
    | with_initializer ',' with_initializer_list
    ;

with_initializer
    : identifier '=' expression
    ;
```

Token `with` 是一個新的內容相關關鍵字。

*With_initializer*左邊的每個*識別碼*都必須系結至*with_expression*之*primary_expression*類型的可存取實例欄位或屬性。 指定*with_expression*的這些識別碼之間可能沒有重複的名稱。

表單的*with_expression*

> *e1* `with` `{`*識別碼* = *e2*，... `}`

會被視為形式的調用

> *e1*`.With(`*identifier2*`:` *e2*，...`)`

其中，針對 `With` 名為*identifier2 之可存取實例成員的*每個方法，我們會選取 [ *identifier2* ] 做為該方法中第一個參數的名稱，該方法的呼叫端-接收者參數與實例欄位或系結至*識別碼*的屬性相同。 如果找不到此類參數，就不會考慮該方法。 要叫用的方法是透過多載解析從剩餘的候選項目中選取。

> **設計附注**：指定呼叫端-接收者參數，有許多*with 運算式*的優點都可以使用，而不需要這個特殊的語法形式。 因此，我們會考慮是否需要它。 其主要優點是可讓您以欄位和屬性的名稱來進行程式設計，而不是以參數的名稱為依據。 如此一來，我們就可以同時改善可讀性和工具品質（例如， *with_expression*的識別碼上的進入定義會導覽至屬性，而不是方法參數）。

- []**開啟問題**：應該修改此描述以支援擴充方法。

### <a name="pattern-matching"></a>模式比對

請參閱[模式](csharp-8.0/patterns.md#positional-pattern)比對規格，以瞭解 `Deconstruct` 的規格及其與模式比對的關聯性。

> **設計附注**：根據此處所指定的編譯器產生的 `Deconstruct`，以及模式比對的規格，記錄宣告
> ```cs
> public class Point(int X, int Y);
> ```
> 將支援位置模式比對，如下所示
> ```cs
> Point p = new Point(3, 4);
> if (p is Point(3, var y)) { // if X is 3
>     Console.WriteLine(y);   // print Y
> }
> ```

### <a name="record-type-declarations"></a>記錄類型宣告

已擴充 `class` 或 `struct` 宣告的語法以支援值參數;參數會變成類型的屬性：

```antlr
class_declaration
    : attributes? class_modifiers? 'partial'? 'class' identifier type_parameter_list?
      record_parameters? record_class_base? type_parameter_constraints_clauses? class_body
    ;

struct_declaration
    : attributes? struct_modifiers? 'partial'? 'struct' identifier type_parameter_list?
      record_parameters? struct_interfaces? type_parameter_constraints_clauses? struct_body
    ;

record_class_base
    : class_type record_base_arguments?
    | interface_type_list
    | class_type record_base_arguments? ',' interface_type_list
    ;

record_base_arguments
    : '(' argument_list? ')'
    ;

record_parameters
    : '(' record_parameter_list? ')'
    ;

record_parameter_list
    : record_parameter
    | record_parameter ',' record_parameter_list
    ;

record_parameter
    : attributes? type identifier record_property_name? default_argument?
    ;

record_property_name
    : ':' identifier
    ;

class_body
    : '{' class_member_declarations? '}'
    | ';'
    ;

struct_body
    : '{' struct_members_declarations? '}'
    | ';'
    ;
```

> **設計附注**：因為記錄類型通常很有用，而不需要在類別主體中明確宣告任何成員，所以我們會修改宣告的語法，讓主體只是分號。

以*記錄參數*宣告的類別（結構）稱為*記錄類別*（*記錄結構*），其中之一是*記錄類型*。

- []**開啟問題**：我們必須在文法中包含*primary_constructor_body* ，讓它可以出現在記錄類型宣告中。
- []**開啟問題**：參數名稱的名稱衝突規則為何？ 假設有一個不允許與型別參數或另一個*記錄參數*衝突。
- []**開啟問題**：我們需要指定記錄參數的範圍。 可以使用它們的位置？ 大概是在實例欄位初始化運算式中，而且至少*primary_constructor_body* 。
- []**開啟問題**：記錄類型宣告是否為部分？ 若是如此，必須在每個元件上重複參數嗎？

#### <a name="members-of-a-record-type"></a>記錄類型的成員

除了在*類別主體*中宣告的成員之外，記錄類型還有下列其他成員：

##### <a name="primary-constructor"></a>主要的構造函式

記錄類型具有 `public` 的函式，其簽章對應于類型宣告的值參數。 這稱為類型的*主要*「函式」，並會隱藏隱含宣告的*預設函數*。

在執行時間，主要的函式

* 針對對應至值參數的屬性，初始化編譯器產生的支援欄位（如果這些屬性是由編譯器提供，則為[請參閱 1.1.2](#1.1.2)）;請
* 執行出現在*類別主體*中的實例欄位初始化運算式;然後
* 叫用基類的構造函式：
    * 如果*record_base_arguments*中有引數，則會叫用具有這些引數的多載解析所選取的基底函式;
    * 否則，會叫用不含引數的基底函式。
* 依來源循序執行每個*primary_constructor_body*的主體（如果有的話）。

- []**開啟問題**：我們需要指定該順序，特別是跨部分的編譯單位。
- []**開啟問題**：我們需要指定每個明確宣告的函式都必須與主要的函數鏈連結。
- []**開啟問題**：是否允許在主要的函式上變更存取修飾詞？
- []**開啟問題**：在記錄結構中，沒有任何記錄參數會發生錯誤？

##### <a name="primary-constructor-body"></a>主要的函數主體

```antlr
primary_constructor_body
    : attributes? constructor_modifiers? identifier block
    ;
```

*Primary_constructor_body*只能用在記錄類型宣告中。 *Primary_constructor_body*的*識別碼*應命名其宣告所在的記錄類型。

*Primary_constructor_body*不會自行宣告成員，但可讓程式設計人員提供的屬性，並指定的存取記錄類型的主要程式設計函數。 它也可讓程式設計師提供在結構化記錄類型的實例時，將會執行的其他程式碼。

- []**開啟問題**：我們應該注意結構預設的函式會略過此。
- []**開啟問題**：我們應該指定初始化的執行順序。
- []**開啟問題**：在非記錄類型宣告中，我們是否允許像是*primary_constructor_body* （假設沒有屬性和修飾詞），並將它視為實例欄位初始化運算式的程式碼？

##### <a name="properties"></a>屬性

對於記錄類型宣告的每個記錄參數，都有一個對應的 `public` 屬性成員，其名稱和類型取自值參數聲明。 其名稱是*record_property_name*的*識別碼*（如果有的話）或*record_parameter*的*識別碼*，否則為。 如果沒有具有 `get` 存取子的具象（非抽象）公用屬性，且已明確宣告或繼承此名稱和類型，則編譯器會產生，如下所示：

* 若為*記錄結構*或 `sealed`*記錄類別*：
 * `private` `readonly` 欄位會產生為 `readonly` 屬性的支援欄位。 它的值會在結構化期間使用對應的主要函式參數值進行初始化。
 * 屬性的 `get` 存取子會實作為傳回支援欄位的值。
 * 會覆寫每個「相符」的已繼承虛擬屬性的 `get` 存取子。

> **設計附注**：換句話說，如果您擴充基類，或實作為宣告公用抽象屬性的介面，且其名稱和類型與記錄參數相同，則會覆寫或執行該屬性。

- []**開啟問題**：明確宣告屬性時，是否可以變更其上的存取修飾詞？
- []**開啟問題**：是否可以取代屬性的欄位？

##### <a name="object-methods"></a>物件方法

若為*記錄結構*或 `sealed`*記錄類別*，除非使用者提供，否則編譯器會產生 `object.GetHashCode()` 和 `object.Equals(object)` 方法的執行。

- []**開啟問題**：我們應精確指定其執行方式。
- []**開啟問題**：我們也應該加入記錄類型的介面 `IEquatable<T>`，並指定要提供的實作為。
- []**開啟問題**：我們也應該指定我們會執行每個 `IEquatable<T>.Equals`。
- []**開啟問題**：我們應精確指定我們如何解決記錄繼承臉部中的 Equals 問題：特別是如何產生等號比較方法，使其為對稱、可轉移、反身等。
- []**開啟問題**：我們已建議為記錄類型執行 `operator ==` 和 `operator !=`。
- []**開啟問題**：我們是否應該自動產生 `object.ToString`的執行？

##### `Deconstruct`

記錄類型具有編譯器產生的 `public` 方法 `void Deconstruct`，除非使用者提供任何簽章。 每個參數都是 `out` 參數，其名稱和類型與記錄類型的對應參數相同。 編譯器提供的此方法的實值，必須將每個 `out` 參數指派為對應屬性的值。

如需 `Deconstruct`的語法，請參閱模式比對[規格](csharp-8.0/patterns.md#positional-pattern)。

##### <a name="with-method"></a>`With` 方法

除非宣告了名為 `With` 的使用者宣告成員，否則記錄類型會具有編譯器提供的方法，名為 `With` 其傳回類型為記錄類型本身，而且包含一個對應于每個*記錄參數*的值參數，其順序與這些參數出現在記錄類型宣告中的順序相同。 每個參數都必須有對應屬性的*呼叫端-接收者預設引數*。

在 `abstract` 記錄類別中，編譯器提供的 `With` 方法是抽象的。 在 record 結構或密封的記錄類別中，編譯器提供的 `With` 方法是 `sealed`。 否則，編譯器提供的 `With` 方法是「虛擬」，而且它的執行應會傳回新的實例，其方式是叫用主要的函式，並將參數當做引數，以從參數建立新的實例，並傳回新的實例。

- []**開啟問題**：我們也應該指定在哪些條件下，我們會覆寫或執行繼承的虛擬 `With` 方法，或從實作為介面的 `With` 方法。
- []**開啟問題**：我們應該會說，當我們繼承非虛擬 `With` 方法時，會發生什麼事。

> **設計附注**：因為記錄類型預設為不可變，所以 `With` 方法會提供一種方式來建立新的實例，其與現有的實例相同，但已選取的屬性會指定新的值。 例如，假設
> ```cs
> public class Point(int X, int Y);
> ```
> 有一個編譯器提供的成員
> ```cs
>     public virtual Point With(int X = this.X, int Y = this.Y) => new Point(X, Y);
> ```
> 這會啟用記錄類型的變數
> ```cs
> var p = new Point(3, 4);
> ```
> 要取代為具有一或多個屬性不同的實例
> ```cs
>     p = p.With(X: 5);
> ```
> 這也可以使用*with_expression*來表示：
> ```cs
>     p = p with { X = 5 };
> ```

### <a name="examples"></a>範例

#### <a name="compatibility-of-record-types"></a>記錄類型的相容性

因為程式設計人員可以將成員加入至記錄類型宣告，所以通常可以變更記錄元素的集合，而不會影響現有的用戶端。 例如，給定記錄類型的初始版本

```cs
// v1
public class Person(string Name, DateTime DateOfBirth);
```

記錄類型的新元素可以相容性加入至類型的下一個修訂中，而不會影響二進位或來源的相容性：

```cs
// v2
public class Person(string Name, DateTime DateOfBirth, string HomeTown)
{
    // Note: below operations added to retain binary compatibility with v1
    public Person(string Name, DateTime DateOfBirth) : this(Name, DateOfBirth, string.Empty) {}
    public static void operator is(Person self, out string Name, out DateTime DateOfBirth)
        { Name = self.Name; DateOfBirth = self.DateOfBirth; }
    public Person With(string Name, DateTime DateOfBirth) => new Person(Name, DateOfBirth);
}
```

#### <a name="record-struct-example"></a>record 結構範例

此記錄結構

```cs
public struct Pair(object First, object Second);
```

會轉譯為此程式碼

```cs
public struct Pair : IEquatable<Pair>
{
    public object First { get; }
    public object Second { get; }
    public Pair(object First, object Second)
    {
        this.First = First;
        this.Second = Second;
    }
    public bool Equals(Pair other) // for IEquatable<Pair>
    {
        return Equals(First, other.First) && Equals(Second, other.Second);
    }
    public override bool Equals(object other)
    {
        return (other as Pair)?.Equals(this) == true;
    }
    public override int GetHashCode()
    {
        return (First?.GetHashCode()*17 + Second?.GetHashCode()).GetValueOrDefault();
    }
    public Pair With(object First = this.First, object Second = this.Second) => new Pair(First, Second);
    public void Deconstruct(out object First, out object Second)
    {
        First = self.First;
        Second = self.Second;
    }
}
```

- []**開啟問題**：是否應將 Equals （配對其他）的實作為配對的公用成員？
- []**開啟問題**：此 `Equals` 的執行在面對繼承時不對稱。
- []**開啟問題**：記錄是否應宣告 `operator ==` 和 `operator !=`？

> **設計附注**：因為一種記錄類型可以繼承自另一個，而且此 `Equals` 的執行在這種情況下不會是對稱的，所以不正確。 我們建議以這種方式來執行相等：
> ```cs
>     public bool Equals(Pair other) // for IEquatable<Pair>
>     {
>         return other != null && EqualityContract == other.EqualityContract &&
>             Equals(First, other.First) && Equals(Second, other.Second);
>     }
>     protected virtual Type EqualityContract => typeof(Pair);
> ```
> 衍生的記錄會 `override EqualityContract`。 較不具吸引力的替代方法是限制繼承。

#### <a name="sealed-record-example"></a>密封記錄範例

這個密封的記錄類別

```cs
public sealed class Student(string Name, decimal Gpa);
```

會轉譯為此程式碼

```cs
public sealed class Student : IEquatable<Student>
{
    public string Name { get; }
    public decimal Gpa { get; }
    public Student(string Name, decimal Gpa)
    {
        this.Name = Name;
        this.Gpa = Gpa;
    }
    public bool Equals(Student other) // for IEquatable<Student>
    {
        return other != null && Equals(Name, other.Name) && Equals(Gpa, other.Gpa);
    }
    public override bool Equals(object other)
    {
        return this.Equals(other as Student);
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()*17 + Gpa?.GetHashCode()).GetValueOrDefault();
    }
    public Student With(string Name = this.Name, decimal Gpa = this.Gpa) => new Student(Name, Gpa);
    public void Deconstruct(out string Name, out decimal Gpa)
    {
        Name = self.Name;
        Gpa = self.Gpa;
    }
}
```

#### <a name="abstract-record-class-example"></a>抽象記錄類別範例

此抽象記錄類別

```cs
public abstract class Person(string Name);
```

會轉譯為此程式碼

```cs
public abstract class Person : IEquatable<Person>
{
    public string Name { get; }
    public Person(string Name)
    {
        this.Name = Name;
    }
    public bool Equals(Person other)
    {
        return other != null && Equals(Name, other.Name);
    }
    public override bool Equals(object other)
    {
        return Equals(other as Person);
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()).GetValueOrDefault();
    }
    public abstract Person With(string Name = this.Name);
    public void Deconstruct(Person self, out string Name)
    {
        Name = self.Name;
    }
}
```

#### <a name="combining-abstract-and-sealed-records"></a>結合抽象和密封記錄

假設 `Person` 上述的抽象記錄類別，則此密封記錄類別

```cs
public sealed class Student(string Name, decimal Gpa) : Person(Name);
```

會轉譯為此程式碼

```cs
public sealed class Student : Person, IEquatable<Student>
{
    public override string Name { get; }
    public decimal Gpa { get; }
    public Student(string Name, decimal Gpa) : base(Name)
    {
        this.Name = Name;
        this.Gpa = Gpa;
    }
    public override bool Equals(Student other) // for IEquatable<Student>
    {
        return Equals(Name, other.Name) && Equals(Gpa, other.Gpa);
    }
    public bool Equals(Person other) // for IEquatable<Person>
    {
        return (other as Student)?.Equals(this) == true;
    }
    public override bool Equals(object other)
    {
        return (other as Student)?.Equals(this) == true;
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()*17 + Gpa.GetHashCode()).GetValueOrDefault();
    }
    public Student With(string Name = this.Name, decimal Gpa = this.Gpa) => new Student(Name, Gpa);
    public override Person With(string Name = this.Name) => new Student(Name, Gpa);
    public void Deconstruct(Student self, out string Name, out decimal Gpa)
    {
        Name = self.Name;
        Gpa = self.Gpa;
    }
}
```

## <a name="drawbacks"></a>缺點
[drawbacks]: #drawbacks

就像任何語言功能一樣，我們還必須問您，是否重金換回其他語言的複雜性，以提供給可從功能獲益C#的程式主體。

## <a name="alternatives"></a>替代方案
[alternatives]: #alternatives

我們已考慮*primary constructors*在6中C#新增主要的函式。 雖然它們會佔用與此提案相同的語法介面，但我們發現它們的優點是記錄所提供的優勢。

## <a name="unresolved-questions"></a>未解決的問題
[unresolved]: #unresolved-questions

提出的問題會出現在提案的主體中。

## <a name="design-meetings"></a>設計會議

TBD
