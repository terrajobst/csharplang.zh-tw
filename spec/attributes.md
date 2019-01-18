---
ms.openlocfilehash: a8ad8a8b3eda1d00fa745bd92e4371eacc36b79f
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229540"
---
# <a name="attributes"></a>屬性

許多 C# 語言可讓程式設計人員指定的程式中定義的實體的宣告式資訊。 比方說，裝飾它與指定類別中方法的存取範圍*method_modifier*s `public`， `protected`， `internal`，和`private`。

C# 可讓程式設計人員創造新的宣告式的資訊，稱為***屬性***。 程式設計人員可以將屬性附加至不同的程式實體，並擷取在執行階段環境中的屬性資訊。 比方說，架構可能會定義`HelpAttribute`可以放在特定的程式項目 （例如類別和方法），提供將這些程式項目對應至其文件的屬性。

屬性定義透過屬性類別的宣告 ([屬性類別](attributes.md#attribute-classes))，這可能會有位置和具名參數 ([位置和具名的參數](attributes.md#positional-and-named-parameters))。 屬性會附加至 C# 程式使用屬性規格中的實體 ([屬性規格](attributes.md#attribute-specification))，而且可以在執行階段擷取為屬性執行個體 ([屬性執行個體](attributes.md#attribute-instances))。

## <a name="attribute-classes"></a>屬性類別

衍生自抽象類別的類別`System.Attribute`，無論是直接或間接地會***屬性類別***。 宣告屬性類別定義的新***屬性***可放置在宣告。 依照慣例，屬性類別會命名為尾碼`Attribute`。 屬性的用法可能會包含或省略此尾碼。

### <a name="attribute-usage"></a>屬性使用方式

屬性`AttributeUsage`([AttributeUsage 屬性](attributes.md#the-attributeusage-attribute)) 用來描述如何使用屬性類別。

`AttributeUsage` 已為位置參數 ([位置和具名的參數](attributes.md#positional-and-named-parameters))，可讓屬性類別，以指定在其使用的宣告類型。 此範例

```csharp
using System;

[AttributeUsage(AttributeTargets.Class | AttributeTargets.Interface)]
public class SimpleAttribute: Attribute 
{
    ...
}
```

定義屬性類別名為`SimpleAttribute`上可放置*class_declaration*s 及*interface_declaration*僅。 此範例

```csharp
[Simple] class Class1 {...}

[Simple] interface Interface1 {...}
```

顯示了多個`Simple`屬性。 雖然這個屬性定義同名`SimpleAttribute`，使用這個屬性時，`Attribute`後置詞可能會省略，導致 簡短名稱`Simple`。 因此，上述範例在語意上等於下列：

```csharp
[SimpleAttribute] class Class1 {...}

[SimpleAttribute] interface Interface1 {...}
```

`AttributeUsage` 有一個具名的參數 ([位置和具名的參數](attributes.md#positional-and-named-parameters)) 稱為`AllowMultiple`，指出是否將屬性可以指定一次以上對指定的實體。 如果`AllowMultiple`類別是屬性，則為 true，則該屬性類別***多重用途屬性類別***，並指定在實體上一次以上。 如果`AllowMultiple`類別是屬性，則為 false 或並未指定，則該屬性類別***單次使用屬性類別***，並可指定在實體上一次。

此範例

```csharp
using System;

[AttributeUsage(AttributeTargets.Class, AllowMultiple = true)]
public class AuthorAttribute: Attribute
{
    private string name;

    public AuthorAttribute(string name) {
        this.name = name;
    }

    public string Name {
        get { return name; }
    }
}
```

定義名為多重用途屬性類別`AuthorAttribute`。 此範例

```csharp
[Author("Brian Kernighan"), Author("Dennis Ritchie")] 
class Class1
{
    ...
}
```

顯示兩種用法的類別宣告`Author`屬性。

`AttributeUsage` 有另一個具名的參數呼叫`Inherited`，表示屬性，指定基底類別，是否也會由衍生自該基底類別的類別繼承。 如果`Inherited`屬性類別為 true，則該屬性繼承。 如果`Inherited`類別是屬性，則為 false，則不會繼承該屬性。 如果未指定，其預設值為 true。

屬性類別`X`沒有`AttributeUsage`，附加做為中的屬性

```csharp
using System;

class X: Attribute {...}
```

相當於下列：

```csharp
using System;

[AttributeUsage(
    AttributeTargets.All,
    AllowMultiple = false,
    Inherited = true)
]
class X: Attribute {...}
```

### <a name="positional-and-named-parameters"></a>位置和具名參數

屬性的類別可以有***位置參數***並***具名參數***。 屬性類別的每個公用執行個體建構函式會定義該屬性類別的位置參數是有效的序列。 每個非靜態公用讀寫欄位和屬性類別的屬性會定義屬性類別的具名的參數。

此範例

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class HelpAttribute: Attribute
{
    public HelpAttribute(string url) {        // Positional parameter
        ...
    }

    public string Topic {                     // Named parameter
        get {...}
        set {...}
    }

    public string Url {
        get {...}
    }
}
```

定義屬性類別名為`HelpAttribute`有一個位置的參數， `url`，和一個具名的參數， `Topic`。 雖然非靜態、 公用屬性`Url`不會定義一個具名的參數，因為它不是讀寫。

可能會使用這個屬性類別如下所示：

```csharp
[Help("http://www.mycompany.com/.../Class1.htm")]
class Class1
{
    ...
}

[Help("http://www.mycompany.com/.../Misc.htm", Topic = "Class2")]
class Class2
{
    ...
}
```

### <a name="attribute-parameter-types"></a>屬性參數類型

屬性類別的位置和具名參數的類型僅限於***屬性的參數型別***，這是：

*  下列類型之一： `bool`， `byte`， `char`， `double`， `float`， `int`， `long`， `sbyte`， `short`， `string`， `uint`， `ulong`，`ushort`.
*  `object` 類型。
*  `System.Type` 類型。
*  列舉型別，提供它具有公用存取範圍，且在它巢狀 （如果有的話） 的型別也有公用存取範圍 ([屬性規格](attributes.md#attribute-specification))。
*  上述類型的一維陣列。
*  建構函式引數或公用欄位沒有其中一個類型，不能作為屬性規格中的位置或具名參數。

## <a name="attribute-specification"></a>屬性規格

***屬性規格***是先前定義的屬性宣告的應用程式。 屬性是一種指定宣告的其他宣告資訊。 屬性可以指定在全域範圍 （以指定包含組件或模組上的屬性） 以及*type_declaration*s ([型別宣告](namespaces.md#type-declarations))， *class_member_declaration*s ([類型參數條件約束](classes.md#type-parameter-constraints))， *interface_member_declaration*s ([介面成員](interfaces.md#interface-members))， *struct_member_declaration*s ([結構成員](structs.md#struct-members))， *enum_member_declaration*s ([列舉成員](enums.md#enum-members))， *accessor_declarations* ([存取子](classes.md#accessors))， *event_accessor_declarations* ([欄位型事件](classes.md#field-like-events))，和*formal_parameter_list*s ([方法參數](classes.md#method-parameters))。

屬性中指定***屬性區段***。 屬性區段包含一組的方括號括住的一或多個屬性的逗號分隔清單。 在這類清單中，指定屬性和在其中區段附加至相同的程式實體的順序排列的順序並不重要。 比方說，屬性規格`[A][B]`， `[B][A]`， `[A,B]`，和`[B,A]`相等。

```antlr
global_attributes
    : global_attribute_section+
    ;

global_attribute_section
    : '[' global_attribute_target_specifier attribute_list ']'
    | '[' global_attribute_target_specifier attribute_list ',' ']'
    ;

global_attribute_target_specifier
    : global_attribute_target ':'
    ;

global_attribute_target
    : 'assembly'
    | 'module'
    ;

attributes
    : attribute_section+
    ;

attribute_section
    : '[' attribute_target_specifier? attribute_list ']'
    | '[' attribute_target_specifier? attribute_list ',' ']'
    ;

attribute_target_specifier
    : attribute_target ':'
    ;

attribute_target
    : 'field'
    | 'event'
    | 'method'
    | 'param'
    | 'property'
    | 'return'
    | 'type'
    ;

attribute_list
    : attribute (',' attribute)*
    ;

attribute
    : attribute_name attribute_arguments?
    ;

attribute_name
    : type_name
    ;

attribute_arguments
    : '(' positional_argument_list? ')'
    | '(' positional_argument_list ',' named_argument_list ')'
    | '(' named_argument_list ')'
    ;

positional_argument_list
    : positional_argument (',' positional_argument)*
    ;

positional_argument
    : attribute_argument_expression
    ;

named_argument_list
    : named_argument (','  named_argument)*
    ;

named_argument
    : identifier '=' attribute_argument_expression
    ;

attribute_argument_expression
    : expression
    ;
```

屬性包含*attribute_name*和選擇性的位置和具名引數清單。 位置的引數 （如果有的話） 之前的具名引數。 位置引數所組成*attribute_argument_expression*; 在具名引數所組成的名稱，後面接著等號，後面接著*attribute_argument_expression*，它會在一起會受限於與簡單指派相同的規則。 具名引數的順序並不重要。

*Attribute_name*識別屬性類別。 如果格式*attribute_name*是*type_name*則此名稱必須參考屬性類別。 否則，會發生編譯時期錯誤。 此範例

```csharp
class Class1 {}

[Class1] class Class2 {}    // Error
```

導致編譯時期錯誤，因為它會嘗試使用`Class1`做為屬性類別時`Class1`不是屬性類別。

某些內容會允許在多個目標上的屬性規格。 程式可以明確指定目標包括*attribute_target_specifier*。 屬性會放在全域層級中，當*global_attribute_target_specifier*需要。 在所有其他位置，會套用合理的預設值，但是*attribute_target_specifier*可用來確認，或覆寫預設值，在某些模稜兩可的情況下 （或只是確認中非模稜兩可的情況下的預設值）。 因此，通常*attribute_target_specifier*s，則可以省略以外的全域層級。 解析模稜兩可的內容，如下所示：

*  指定在全域範圍的屬性可以套用至目標組件或目標模組。 沒有預設值存在此內容中，因此*attribute_target_specifier*一律需要在此內容中。 出現與否`assembly` *attribute_target_specifier*表示此屬性套用至目標組件，是否存在`module` *attribute_target_specifier*表示屬性會套用至目標模組。
*  在 delegate 宣告上指定的屬性可以套用至所宣告的委派或它的傳回值。 如果沒有*attribute_target_specifier*，屬性會套用至委派。 出現與否`type` *attribute_target_specifier*屬性會套用至委派，會指出是否存在`return` *attribute_target_specifier*表示屬性會套用至傳回值。
*  在方法宣告上指定的屬性可以套用至所宣告的方法或其傳回值。 如果沒有*attribute_target_specifier*，屬性會套用至方法。 出現與否`method` *attribute_target_specifier*指出屬性會套用至方法，是否存在`return` *attribute_target_specifier*表示該屬性會套用至傳回值。
*  在運算子宣告中指定的屬性可以套用至宣告中的運算子或它的傳回值。 如果沒有*attribute_target_specifier*，屬性會套用到運算子。 出現與否`method` *attribute_target_specifier*指出屬性適用於運算子; 是否存在`return` *attribute_target_specifier*表示屬性會套用至傳回值。
*  省略事件存取子的事件宣告中所指定的屬性可以套用至所宣告的事件、 相關聯的欄位 （如果事件不是抽象的），或相關聯的 add 和 remove 方法。 如果沒有*attribute_target_specifier*，屬性會套用至事件。 出現與否`event` *attribute_target_specifier*指出屬性會套用至事件，是否存在`field` *attribute_target_specifier*表示屬性會套用至欄位;功能和表現做`method` *attribute_target_specifier*指出屬性會套用至方法。
*  Get 存取子宣告屬性或索引子的宣告上指定的屬性可以套用至相關聯的方法，或為其傳回的值。 如果沒有*attribute_target_specifier*，屬性會套用至方法。 出現與否`method` *attribute_target_specifier*指出屬性會套用至方法，是否存在`return` *attribute_target_specifier*表示該屬性會套用至傳回值。
*  Set 存取子屬性或索引子宣告中所指定的屬性可以套用至相關聯的方法，或為其獨立的隱含參數。 如果沒有*attribute_target_specifier*，屬性會套用至方法。 出現與否`method` *attribute_target_specifier*指出屬性會套用至方法，是否存在`param` *attribute_target_specifier*表示屬性會套用至參數;出現與否`return` *attribute_target_specifier*指出屬性會套用至傳回值。
*  事件宣告可以套用至相關聯的方法，或為其單一參數，add 或 remove 存取子宣告上指定的屬性。 如果沒有*attribute_target_specifier*，屬性會套用至方法。 出現與否`method` *attribute_target_specifier*指出屬性會套用至方法，是否存在`param` *attribute_target_specifier*表示屬性會套用至參數;出現與否`return` *attribute_target_specifier*指出屬性會套用至傳回值。

在其他內容中，包含*attribute_target_specifier*是允許但非必要。 比方說，在類別宣告可能包含或省略規範`type`:

```csharp
[type: Author("Brian Kernighan")]
class Class1 {}

[Author("Dennis Ritchie")]
class Class2 {}
```

它是指定了無效的錯誤*attribute_target_specifier*。 比方說，規範`param`不能在類別宣告：

```csharp
[param: Author("Brian Kernighan")]        // Error
class Class1 {}
```

依照慣例，屬性類別會命名為尾碼`Attribute`。 *Attribute_name*的表單*type_name*可能包含或省略此尾碼。 如果找到屬性類別使用和不含此尾碼，模稜兩可存在，而且會產生編譯時期錯誤。 如果*attribute_name*拼寫，其最右邊*識別項*是逐字識別項 ([識別碼](lexical-structure.md#identifiers))，則沒有尾碼的屬性會比對，如此一來解決這種模稜兩可。 此範例

```csharp
using System;

[AttributeUsage(AttributeTargets.All)]
public class X: Attribute
{}

[AttributeUsage(AttributeTargets.All)]
public class XAttribute: Attribute
{}

[X]                     // Error: ambiguity
class Class1 {}

[XAttribute]            // Refers to XAttribute
class Class2 {}

[@X]                    // Refers to X
class Class3 {}

[@XAttribute]           // Refers to XAttribute
class Class4 {}
```

顯示兩個屬性類別叫做`X`和`XAttribute`。 屬性`[X]`模稜兩可，因為它無法參考其中一個`X`或`XAttribute`。 使用逐字識別項，可讓在這類罕見的情況下指定確切的意圖。 屬性`[XAttribute]`不是模稜兩可 (不過如果是屬性類別名為`XAttributeAttribute`！)。 如果類別的宣告`X`移除，則這兩個屬性會參考名為的屬性類別`XAttribute`、，如下所示：

```csharp
using System;

[AttributeUsage(AttributeTargets.All)]
public class XAttribute: Attribute
{}

[X]                     // Refers to XAttribute
class Class1 {}

[XAttribute]            // Refers to XAttribute
class Class2 {}

[@X]                    // Error: no attribute named "X"
class Class3 {}
```

它是編譯時期錯誤一次以上相同的實體使用單次使用的屬性類別。 此範例

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class HelpStringAttribute: Attribute
{
    string value;

    public HelpStringAttribute(string value) {
        this.value = value;
    }

    public string Value {
        get {...}
    }
}

[HelpString("Description of Class1")]
[HelpString("Another description of Class1")]
public class Class1 {}
```

導致編譯時期錯誤，因為它會嘗試使用`HelpString`，這是單次使用屬性的類別，一次以上的宣告上`Class1`。

運算式`E`已*attribute_argument_expression*所有下列陳述式，則為 true 時：

*  型別`E`是屬性參數類型 ([屬性參數類型](attributes.md#attribute-parameter-types))。
*  在編譯時期，值`E`可解析成下列其中之一：
   * 常數值。
   * `System.Type` 物件。
   * 一維陣列*attribute_argument_expression*s。

例如: 

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class TestAttribute: Attribute
{
    public int P1 {
        get {...}
        set {...}
    }

    public Type P2 {
        get {...}
        set {...}
    }

    public object P3 {
        get {...}
        set {...}
    }
}

[Test(P1 = 1234, P3 = new int[] {1, 3, 5}, P2 = typeof(float))]
class MyClass {}
```

A *typeof_expression* ([typeof 運算子](expressions.md#the-typeof-operator)) 做為屬性引數運算式可以參考的非泛型型別、 封閉式建構的類型或未繫結的泛型型別，但它不能參考開啟類型。 這是為了確保可以在編譯時期已解析運算式。

```csharp
class A: Attribute
{
    public A(Type t) {...}
}

class G<T>
{
    [A(typeof(T))] T t;                  // Error, open type in attribute
}

class X
{
    [A(typeof(List<int>))] int x;        // Ok, closed constructed type
    [A(typeof(List<>))] int y;           // Ok, unbound generic type
}
```

## <a name="attribute-instances"></a>屬性執行個體

***屬性執行個體***是執行個體，表示在執行階段屬性。 屬性是定義為具有屬性類別，位置引數，而具名引數。 屬性執行個體是使用位置和具名引數初始化屬性類別的執行個體。

擷取的屬性執行個體牽涉到編譯時期和執行階段處理，如下列各節中所述。

### <a name="compilation-of-an-attribute"></a>屬性的編譯

編譯*屬性*屬性類別與`T`， *positional_argument_list* `P`並*named_argument_list* `N`，包含下列步驟：

*  請依照下列編譯時間處理步驟，來編譯*object_creation_expression*表單的`new T(P)`。 這些步驟會導致編譯時期錯誤，或判斷執行個體建構函式`C`上`T`，可以在執行階段叫用。
*  如果`C`並沒有公用存取範圍，則會發生編譯時期錯誤。
*  每個*named_argument* `Arg`在`N`:
   * 可讓`Name`能*識別項*的*named_argument* `Arg`。
   * `Name` 必須識別的非靜態唯讀公用欄位或屬性上`T`。 如果`T`有沒有這類欄位或屬性，則會發生編譯時期錯誤。
*  請為執行階段具現化屬性的下列資訊： 屬性類別`T`，執行個體建構函式`C`上`T`，則*positional_argument_list* `P`而*named_argument_list* `N`。

### <a name="run-time-retrieval-of-an-attribute-instance"></a>執行階段擷取的屬性執行個體

編譯*屬性*產生屬性類別`T`，執行個體建構函式`C`上`T`，則*positional_argument_list* `P`，和*named_argument_list* `N`。 經由這項資訊，可以在執行階段使用下列步驟擷取的屬性執行個體：

*  遵循執行的執行階段處理步驟*object_creation_expression*的表單`new T(P)`，使用的執行個體建構函式`C`在編譯時期決定。 這些步驟會導致例外狀況，或產生執行個體`O`的`T`。
*  每個*named_argument* `Arg`在`N`，順序：
   * 可讓`Name`能*識別項*的*named_argument* `Arg`。 如果`Name`不會識別的非靜態公用讀寫欄位或屬性上`O`，則會擲回例外狀況。
   * 可讓`Value`要評估的結果*attribute_argument_expression*的`Arg`。
   * 如果`Name`識別要在欄位`O`，然後將此欄位設定為`Value`。
   * 否則，請`Name`識別的屬性上`O`。 將此屬性設定為`Value`。
   * 結果是`O`，屬性類別的執行個體`T`，已使用初始化*positional_argument_list* `P`並*named_argument_list*`N`.

## <a name="reserved-attributes"></a>保留的屬性

少數的屬性會影響以某種方式的語言。 這些屬性包括：

*  `System.AttributeUsageAttribute` ([AttributeUsage 屬性](attributes.md#the-attributeusage-attribute))，這用來描述屬性類別可以使用的方式。
*  `System.Diagnostics.ConditionalAttribute` ([Conditional 屬性](attributes.md#the-conditional-attribute))，這用來定義條件式方法。
*  `System.ObsoleteAttribute` ([Obsolete 屬性](attributes.md#the-obsolete-attribute))，以用來將標記為過時的成員。
*  `System.Runtime.CompilerServices.CallerLineNumberAttribute``System.Runtime.CompilerServices.CallerFilePathAttribute`並`System.Runtime.CompilerServices.CallerMemberNameAttribute`([呼叫端資訊屬性](attributes.md#caller-info-attributes))，這用來提供選擇性的參數呼叫內容的相關資訊。

### <a name="the-attributeusage-attribute"></a>AttributeUsage 屬性

屬性`AttributeUsage`用來描述可以在其中使用屬性類別的方式。

裝飾的類別`AttributeUsage`屬性必須衍生自`System.Attribute`，直接或間接。 否則，會發生編譯時期錯誤。

```csharp
namespace System
{
    [AttributeUsage(AttributeTargets.Class)]
    public class AttributeUsageAttribute: Attribute
    {
        public AttributeUsageAttribute(AttributeTargets validOn) {...}
        public virtual bool AllowMultiple { get {...} set {...} }
        public virtual bool Inherited { get {...} set {...} }
        public virtual AttributeTargets ValidOn { get {...} }
    }

    public enum AttributeTargets
    {
        Assembly     = 0x0001,
        Module       = 0x0002,
        Class        = 0x0004,
        Struct       = 0x0008,
        Enum         = 0x0010,
        Constructor  = 0x0020,
        Method       = 0x0040,
        Property     = 0x0080,
        Field        = 0x0100,
        Event        = 0x0200,
        Interface    = 0x0400,
        Parameter    = 0x0800,
        Delegate     = 0x1000,
        ReturnValue  = 0x2000,

        All = Assembly | Module | Class | Struct | Enum | Constructor | 
            Method | Property | Field | Event | Interface | Parameter | 
            Delegate | ReturnValue
    }
}
```

### <a name="the-conditional-attribute"></a>Conditional 屬性

屬性`Conditional`讓您定義的***條件式方法***並***conditional 屬性類別***。

```csharp
namespace System.Diagnostics
{
    [AttributeUsage(AttributeTargets.Method | AttributeTargets.Class, AllowMultiple = true)]
    public class ConditionalAttribute: Attribute
    {
        public ConditionalAttribute(string conditionString) {...}
        public string ConditionString { get {...} }
    }
}
```

#### <a name="conditional-methods"></a>條件式方法

使用裝飾方法`Conditional`屬性是條件式方法。 `Conditional`屬性表示一種情況，藉由測試條件式編譯符號。 條件式方法的呼叫包含或省略取決於是否定義在呼叫的這個符號。 如果定義符號，則會包括呼叫;否則，會省略 （包括接收者的評估和呼叫的參數） 的呼叫。

條件式方法受限於下列限制：

*  條件式方法必須是中的方法*class_declaration*或是*struct_declaration*。 如果，就會發生編譯時期錯誤`Conditional`介面宣告中的方法上指定屬性。
*  條件式方法必須傳回型別`void`。
*  條件式方法不能標示與`override`修飾詞。 條件式方法可能會標記為使用`virtual`修飾詞，不過。 這種方法的覆寫隱含條件式，而且不能明確標示與`Conditional`屬性。
*  條件式方法不能實作介面方法。 否則，會發生編譯時期錯誤。

如果條件式方法會在編譯時期錯誤將會發生颾魤 ㄛ *delegate_creation_expression*。 此範例

```csharp
#define DEBUG

using System;
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public static void M() {
        Console.WriteLine("Executed Class1.M");
    }
}

class Class2
{
    public static void Test() {
        Class1.M();
    }
}
```

宣告`Class1.M`做為條件式方法。 `Class2``Test`方法會呼叫這個方法。 因為條件式編譯符號`DEBUG`定義，如果`Class2.Test`是呼叫，它會呼叫`M`。 如果符號`DEBUG`有未定義，然後`Class2.Test`不會呼叫`Class1.M`。

請務必請注意，包含或排除的條件式方法的呼叫會受到在呼叫的條件式編譯符號。 在範例

檔案`class1.cs`:

```csharp
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public static void F() {
        Console.WriteLine("Executed Class1.F");
    }
}
```

檔案`class2.cs`:

```csharp
#define DEBUG

class Class2
{
    public static void G() {
        Class1.F();                // F is called
    }
}
```

檔案`class3.cs`:

```csharp
#undef DEBUG

class Class3
{
    public static void H() {
        Class1.F();                // F is not called
    }
}
```

類別`Class2`並`Class3`每個包含條件式方法的呼叫`Class1.F`，也就是條件式根據是否`DEBUG`定義。 因為這個符號已定義的內容中`Class2`而非`Class3`，來呼叫`F`中`Class2`都會包括在內，而呼叫`F`中`Class3`省略。

使用條件式方法的繼承鏈結中可能會造成混淆。 透過條件式方法的呼叫`base`，在表單的`base.M`，受制於一般的條件式方法呼叫的規則。 在範例

檔案`class1.cs`:

```csharp
using System;
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public virtual void M() {
        Console.WriteLine("Class1.M executed");
    }
}
```

檔案`class2.cs`:

```csharp
using System;

class Class2: Class1
{
    public override void M() {
        Console.WriteLine("Class2.M executed");
        base.M();                        // base.M is not called!
    }
}
```

檔案`class3.cs`:

```csharp
#define DEBUG

using System;

class Class3
{
    public static void Test() {
        Class2 c = new Class2();
        c.M();                            // M is called
    }
}
```

`Class2` 包含呼叫`M`其基底類別中定義。 這個呼叫會省略，因為基底方法，是條件式根據符號的存在`DEBUG`，這是未定義。 因此，此方法將寫入主控台"`Class2.M executed`」 只。 明智地使用*pp_declaration*s 可以排除這類問題。

#### <a name="conditional-attribute-classes"></a>Conditional 屬性類別

屬性類別 ([屬性類別](attributes.md#attribute-classes)) 以一個或多個裝飾`Conditional`屬性是***conditional 屬性類別***。 Conditional 屬性類別有因此關聯中所宣告的條件式編譯符號其`Conditional`屬性。 此範例中：

```csharp
using System;
using System.Diagnostics;
[Conditional("ALPHA")]
[Conditional("BETA")]
public class TestAttribute : Attribute {}
```

宣告`TestAttribute`做為條件式編譯符號相關聯的 conditional 屬性類別`ALPHA`和`BETA`。

屬性規格 ([屬性規格](attributes.md#attribute-specification)) 的條件式屬性如果將包含一或多個與其相關聯的條件式編譯符號定義在規格中，否則屬性規格會省略。

請務必請注意，包含或排除的條件式屬性類別的屬性規格會受到規格的條件式編譯符號。 在範例

檔案`test.cs`:

```csharp
using System;
using System.Diagnostics;

[Conditional("DEBUG")]

public class TestAttribute : Attribute {}
```

檔案`class1.cs`:

```csharp
#define DEBUG

[Test]                // TestAttribute is specified

class Class1 {}
```

檔案`class2.cs`:

```csharp
#undef DEBUG

[Test]                 // TestAttribute is not specified

class Class2 {}
```

類別`Class1`並`Class2`是每個屬性裝飾`Test`，也就是條件式根據是否`DEBUG`定義。 因為這個符號已定義的內容中`Class1`而非`Class2`的規格`Test`屬性`Class1`就會包含在指定時`Test`屬性`Class2`省略。

### <a name="the-obsolete-attribute"></a>Obsolete 屬性

屬性`Obsolete`用來標記不再使用的類型的類型和成員。

```csharp
namespace System
{
    [AttributeUsage(
        AttributeTargets.Class | 
        AttributeTargets.Struct |
        AttributeTargets.Enum | 
        AttributeTargets.Interface | 
        AttributeTargets.Delegate |
        AttributeTargets.Method | 
        AttributeTargets.Constructor |
        AttributeTargets.Property | 
        AttributeTargets.Field |
        AttributeTargets.Event,
        Inherited = false)
    ]
    public class ObsoleteAttribute: Attribute
    {
        public ObsoleteAttribute() {...}
        public ObsoleteAttribute(string message) {...}
        public ObsoleteAttribute(string message, bool error) {...}
        public string Message { get {...} }
        public bool IsError { get {...} }
    }
}
```

如果程式使用的類型或成員裝飾`Obsolete`屬性，則編譯器會發出警告或錯誤。 具體來說，編譯器會發出警告不提供任何錯誤的參數，則會提供錯誤的參數，且具有值`false`。 如果錯誤參數指定，且具有值時，編譯器會發出錯誤`true`。

在範例

```csharp
[Obsolete("This class is obsolete; use class B instead")]
class A
{
    public void F() {}
}

class B
{
    public void F() {}
}

class Test
{
    static void Main() {
        A a = new A();         // Warning
        a.F();
    }
}
```

此類別`A`裝飾`Obsolete`屬性。 每次使用`A`在`Main`時會產生警告，其中包含指定的訊息，「 這個類別已經過時;請改用類別 B。 」

### <a name="caller-info-attributes"></a>呼叫端資訊屬性

以供記錄和報告，它有時候是很有用的函式成員，才能取得特定呼叫程式碼的編譯時間資訊。 呼叫端資訊屬性可用來以透明的方式傳遞這類資訊。

當呼叫端資訊屬性的其中一個註解的選擇性參數時，則省略對應的引數的呼叫中，不會不一定會造成用來取代預設參數值。 相反地，如果指定的資訊呼叫內容的相關功能，該資訊會傳遞做為引數的值。

例如: 

```csharp
using System.Runtime.CompilerServices

...

public void Log(
    [CallerLineNumber] int line = -1,
    [CallerFilePath]   string path = null,
    [CallerMemberName] string name = null
)
{
    Console.WriteLine((line < 0) ? "No line" : "Line "+ line);
    Console.WriteLine((path == null) ? "No file path" : path);
    Console.WriteLine((name == null) ? "No member name" : name);
}
```

呼叫`Log()`不使用引數可能會列印呼叫，行號和檔案路徑，以及呼叫發生在其中成員的名稱。

呼叫端資訊屬性可能會發生在任何一處，選擇性的參數包括委派宣告中。 不過，特定的呼叫端資訊屬性有限制他們可以屬性，參數型別上，一律會從替代值的隱含轉換成參數類型。

它是定義和實作部分方法宣告的一部分的參數上有相同的呼叫端資訊屬性的錯誤。 套用只呼叫端資訊屬性中定義的一部分，而只有在實作的組件中發生的呼叫端資訊屬性會被忽略。

呼叫端資訊不會影響多載解析。 從原始程式檔的呼叫端仍省略屬性化的選擇性參數，因為多載解析會忽略那些參數相同的方式，它會忽略其他省略的選擇性參數 ([多載解析](expressions.md#overload-resolution))。

在原始程式碼中明確地叫用函式時，呼叫端資訊是只會用來替代。 隱含的引動過程，例如隱含父建構函式呼叫沒有來源位置，而且不會取代呼叫端資訊。 此外，動態地繫結的呼叫將不會取代呼叫端資訊。 如果呼叫端資訊，在此情況下省略屬性化的參數，參數指定的預設值使用。

查詢運算式是一個例外。 這些皆視為語法擴充，並呼叫它們會展開以略過與呼叫端資訊屬性的選擇性參數，如果呼叫端資訊會被取代。 使用的位置是查詢子句，這個子句會呼叫所產生的位置。

如果指定的參數上指定一個以上的呼叫端資訊屬性，則不會依下列順序選擇較： `CallerLineNumber`， `CallerFilePath`， `CallerMemberName`。

#### <a name="the-callerlinenumber-attribute"></a>CallerLineNumber 屬性

`System.Runtime.CompilerServices.CallerLineNumberAttribute`標準的隱含轉換時，會將選擇性參數上允許 ([標準的隱含轉換](conversions.md#standard-implicit-conversions)) 的常數值從`int.MaxValue`參數的型別。 這可確保不會發生錯誤，可以傳遞至該值的任何非負號。

如果函式引動過程，從原始程式碼中的位置會省略選擇性參數`CallerLineNumberAttribute`，則表示該位置的行號的數值常值做為取代預設參數值的引動過程的引數。

如果引動過程會跨越多行，選擇線條是視實作而定。

請注意，行號可能會受到`#line`指示詞 ([行指示詞](lexical-structure.md#line-directives))。

#### <a name="the-callerfilepath-attribute"></a>CallerFilePath 屬性

`System.Runtime.CompilerServices.CallerFilePathAttribute`標準的隱含轉換時，會將選擇性參數上允許 ([標準的隱含轉換](conversions.md#standard-implicit-conversions)) 從`string`參數的型別。

如果函式引動過程，從原始程式碼中的位置會省略選擇性參數`CallerFilePathAttribute`，則表示該位置的檔案路徑的字串常值做為取代預設參數值的引動過程的引數。

檔案路徑的格式是視實作而定。

請注意，檔案路徑可能會受到`#line`指示詞 ([行指示詞](lexical-structure.md#line-directives))。

#### <a name="the-callermembername-attribute"></a>CallerMemberName 屬性

`System.Runtime.CompilerServices.CallerMemberNameAttribute`標準的隱含轉換時，會將選擇性參數上允許 ([標準的隱含轉換](conversions.md#standard-implicit-conversions)) 從`string`參數的型別。

如果函式成員的主體內，或在屬性內的位置從函式引動過程會套用至函式的成員，本身或其傳回型別、 參數或型別參數中的原始程式碼會省略選擇性參數`CallerMemberNameAttribute`，則會顯示字串常值代表該成員的名稱做為取代預設參數值的引動過程的引數。

引動過程發生泛型方法內，會使用僅方法名稱本身，沒有類型參數清單。

如果引動過程發生在明確介面成員實作，會使用僅方法名稱本身，而不需要上述介面限定性條件。

引動過程屬性或事件存取子中發生，所使用的成員名稱是屬性或事件本身。

如果引動過程發生在索引子存取子，使用的成員名稱是所提供的`IndexerNameAttribute`([IndexerName 屬性](attributes.md#the-indexername-attribute)) 上的索引子的成員，如果有的話或將預設名稱`Item`否則。

執行個體建構函式、 靜態建構函式、 解構函式和運算子宣告中發生之成員的引動過程使用的名稱會是視實作而定。

## <a name="attributes-for-interoperation"></a>交互操作的屬性

注意:本節是僅適用於 Microsoft.NET 實作的C#。

### <a name="interoperation-with-com-and-win32-components"></a>與 COM 和 Win32 元件的互通性

.NET 執行階段會提供大量的屬性，可讓 C# 程式使用 COM 與 Win32 Dll 所撰寫的元件相互操作。 例如，`DllImport`屬性可以用於`static extern`方法，以表示方法的實作是在 Win32 DLL 中找到。 這些屬性位於`System.Runtime.InteropServices`命名空間，而這些屬性的詳細文件位於.NET 執行階段文件。

### <a name="interoperation-with-other-net-languages"></a>與其他.NET 語言的互通性

#### <a name="the-indexername-attribute"></a>IndexerName 屬性。

索引子會實作在.NET 中使用索引的屬性，而且.NET 中繼資料中有一個名稱。 如果沒有`IndexerName`屬性，則索引子，則名稱`Item`預設會使用。 `IndexerName`屬性可讓開發人員覆寫此預設值，並指定不同的名稱。

```csharp
namespace System.Runtime.CompilerServices.CSharp
{
    [AttributeUsage(AttributeTargets.Property)]
    public class IndexerNameAttribute: Attribute
    {
        public IndexerNameAttribute(string indexerName) {...}
        public string Value { get {...} } 
    }
}
```
