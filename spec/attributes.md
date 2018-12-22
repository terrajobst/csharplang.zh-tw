# <a name="attributes"></a><span data-ttu-id="e5d45-101">屬性</span><span class="sxs-lookup"><span data-stu-id="e5d45-101">Attributes</span></span>

<span data-ttu-id="e5d45-102">許多 C# 語言可讓程式設計人員指定的程式中定義的實體的宣告式資訊。</span><span class="sxs-lookup"><span data-stu-id="e5d45-102">Much of the C# language enables the programmer to specify declarative information about the entities defined in the program.</span></span> <span data-ttu-id="e5d45-103">比方說，裝飾它與指定類別中方法的存取範圍*method_modifier*s `public`， `protected`， `internal`，和`private`。</span><span class="sxs-lookup"><span data-stu-id="e5d45-103">For example, the accessibility of a method in a class is specified by decorating it with the *method_modifier*s `public`, `protected`, `internal`, and `private`.</span></span>

<span data-ttu-id="e5d45-104">C# 可讓程式設計人員創造新的宣告式的資訊，稱為***屬性***。</span><span class="sxs-lookup"><span data-stu-id="e5d45-104">C# enables programmers to invent new kinds of declarative information, called ***attributes***.</span></span> <span data-ttu-id="e5d45-105">程式設計人員可以將屬性附加至不同的程式實體，並擷取在執行階段環境中的屬性資訊。</span><span class="sxs-lookup"><span data-stu-id="e5d45-105">Programmers can then attach attributes to various program entities, and retrieve attribute information in a run-time environment.</span></span> <span data-ttu-id="e5d45-106">比方說，架構可能會定義`HelpAttribute`可以放在特定的程式項目 （例如類別和方法），提供將這些程式項目對應至其文件的屬性。</span><span class="sxs-lookup"><span data-stu-id="e5d45-106">For instance, a framework might define a `HelpAttribute` attribute that can be placed on certain program elements (such as classes and methods) to provide a mapping from those program elements to their documentation.</span></span>

<span data-ttu-id="e5d45-107">屬性定義透過屬性類別的宣告 ([屬性類別](attributes.md#attribute-classes))，這可能會有位置和具名參數 ([位置和具名的參數](attributes.md#positional-and-named-parameters))。</span><span class="sxs-lookup"><span data-stu-id="e5d45-107">Attributes are defined through the declaration of attribute classes ([Attribute classes](attributes.md#attribute-classes)), which may have positional and named parameters ([Positional and named parameters](attributes.md#positional-and-named-parameters)).</span></span> <span data-ttu-id="e5d45-108">屬性會附加至 C# 程式使用屬性規格中的實體 ([屬性規格](attributes.md#attribute-specification))，而且可以在執行階段擷取為屬性執行個體 ([屬性執行個體](attributes.md#attribute-instances))。</span><span class="sxs-lookup"><span data-stu-id="e5d45-108">Attributes are attached to entities in a C# program using attribute specifications ([Attribute specification](attributes.md#attribute-specification)), and can be retrieved at run-time as attribute instances ([Attribute instances](attributes.md#attribute-instances)).</span></span>

## <a name="attribute-classes"></a><span data-ttu-id="e5d45-109">屬性類別</span><span class="sxs-lookup"><span data-stu-id="e5d45-109">Attribute classes</span></span>

<span data-ttu-id="e5d45-110">衍生自抽象類別的類別`System.Attribute`，無論是直接或間接地會***屬性類別***。</span><span class="sxs-lookup"><span data-stu-id="e5d45-110">A class that derives from the abstract class `System.Attribute`, whether directly or indirectly, is an ***attribute class***.</span></span> <span data-ttu-id="e5d45-111">宣告屬性類別定義的新***屬性***可放置在宣告。</span><span class="sxs-lookup"><span data-stu-id="e5d45-111">The declaration of an attribute class defines a new kind of ***attribute*** that can be placed on a declaration.</span></span> <span data-ttu-id="e5d45-112">依照慣例，屬性類別會命名為尾碼`Attribute`。</span><span class="sxs-lookup"><span data-stu-id="e5d45-112">By convention, attribute classes are named with a suffix of `Attribute`.</span></span> <span data-ttu-id="e5d45-113">屬性的用法可能會包含或省略此尾碼。</span><span class="sxs-lookup"><span data-stu-id="e5d45-113">Uses of an attribute may either include or omit this suffix.</span></span>

### <a name="attribute-usage"></a><span data-ttu-id="e5d45-114">屬性使用方式</span><span class="sxs-lookup"><span data-stu-id="e5d45-114">Attribute usage</span></span>

<span data-ttu-id="e5d45-115">屬性`AttributeUsage`([AttributeUsage 屬性](attributes.md#the-attributeusage-attribute)) 用來描述如何使用屬性類別。</span><span class="sxs-lookup"><span data-stu-id="e5d45-115">The attribute `AttributeUsage` ([The AttributeUsage attribute](attributes.md#the-attributeusage-attribute)) is used to describe how an attribute class can be used.</span></span>

<span data-ttu-id="e5d45-116">`AttributeUsage` 已為位置參數 ([位置和具名的參數](attributes.md#positional-and-named-parameters))，可讓屬性類別，以指定在其使用的宣告類型。</span><span class="sxs-lookup"><span data-stu-id="e5d45-116">`AttributeUsage` has a positional parameter ([Positional and named parameters](attributes.md#positional-and-named-parameters)) that enables an attribute class to specify the kinds of declarations on which it can be used.</span></span> <span data-ttu-id="e5d45-117">此範例</span><span class="sxs-lookup"><span data-stu-id="e5d45-117">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class | AttributeTargets.Interface)]
public class SimpleAttribute: Attribute 
{
    ...
}
```

<span data-ttu-id="e5d45-118">定義屬性類別名為`SimpleAttribute`上可放置*class_declaration*s 及*interface_declaration*僅。</span><span class="sxs-lookup"><span data-stu-id="e5d45-118">defines an attribute class named `SimpleAttribute` that can be placed on *class_declaration*s and *interface_declaration*s only.</span></span> <span data-ttu-id="e5d45-119">此範例</span><span class="sxs-lookup"><span data-stu-id="e5d45-119">The example</span></span>

```csharp
[Simple] class Class1 {...}

[Simple] interface Interface1 {...}
```

<span data-ttu-id="e5d45-120">顯示了多個`Simple`屬性。</span><span class="sxs-lookup"><span data-stu-id="e5d45-120">shows several uses of the `Simple` attribute.</span></span> <span data-ttu-id="e5d45-121">雖然這個屬性定義同名`SimpleAttribute`，使用這個屬性時，`Attribute`後置詞可能會省略，導致 簡短名稱`Simple`。</span><span class="sxs-lookup"><span data-stu-id="e5d45-121">Although this attribute is defined with the name `SimpleAttribute`, when this attribute is used, the `Attribute` suffix may be omitted, resulting in the short name `Simple`.</span></span> <span data-ttu-id="e5d45-122">因此，上述範例在語意上等於下列：</span><span class="sxs-lookup"><span data-stu-id="e5d45-122">Thus, the example above is semantically equivalent to the following:</span></span>

```csharp
[SimpleAttribute] class Class1 {...}

[SimpleAttribute] interface Interface1 {...}
```

<span data-ttu-id="e5d45-123">`AttributeUsage` 有一個具名的參數 ([位置和具名的參數](attributes.md#positional-and-named-parameters)) 稱為`AllowMultiple`，指出是否將屬性可以指定一次以上對指定的實體。</span><span class="sxs-lookup"><span data-stu-id="e5d45-123">`AttributeUsage` has a named parameter ([Positional and named parameters](attributes.md#positional-and-named-parameters)) called `AllowMultiple`, which indicates whether the attribute can be specified more than once for a given entity.</span></span> <span data-ttu-id="e5d45-124">如果`AllowMultiple`類別是屬性，則為 true，則該屬性類別***多重用途屬性類別***，並指定在實體上一次以上。</span><span class="sxs-lookup"><span data-stu-id="e5d45-124">If `AllowMultiple` for an attribute class is true, then that attribute class is a ***multi-use attribute class***, and can be specified more than once on an entity.</span></span> <span data-ttu-id="e5d45-125">如果`AllowMultiple`類別是屬性，則為 false 或並未指定，則該屬性類別***單次使用屬性類別***，並可指定在實體上一次。</span><span class="sxs-lookup"><span data-stu-id="e5d45-125">If `AllowMultiple` for an attribute class is false or it is unspecified, then that attribute class is a ***single-use attribute class***, and can be specified at most once on an entity.</span></span>

<span data-ttu-id="e5d45-126">此範例</span><span class="sxs-lookup"><span data-stu-id="e5d45-126">The example</span></span>

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

<span data-ttu-id="e5d45-127">定義名為多重用途屬性類別`AuthorAttribute`。</span><span class="sxs-lookup"><span data-stu-id="e5d45-127">defines a multi-use attribute class named `AuthorAttribute`.</span></span> <span data-ttu-id="e5d45-128">此範例</span><span class="sxs-lookup"><span data-stu-id="e5d45-128">The example</span></span>

```csharp
[Author("Brian Kernighan"), Author("Dennis Ritchie")] 
class Class1
{
    ...
}
```

<span data-ttu-id="e5d45-129">顯示兩種用法的類別宣告`Author`屬性。</span><span class="sxs-lookup"><span data-stu-id="e5d45-129">shows a class declaration with two uses of the `Author` attribute.</span></span>

<span data-ttu-id="e5d45-130">`AttributeUsage` 有另一個具名的參數呼叫`Inherited`，表示屬性，指定基底類別，是否也會由衍生自該基底類別的類別繼承。</span><span class="sxs-lookup"><span data-stu-id="e5d45-130">`AttributeUsage` has another named parameter called `Inherited`, which indicates whether the attribute, when specified on a base class, is also inherited by classes that derive from that base class.</span></span> <span data-ttu-id="e5d45-131">如果`Inherited`屬性類別為 true，則該屬性繼承。</span><span class="sxs-lookup"><span data-stu-id="e5d45-131">If `Inherited` for an attribute class is true, then that attribute is inherited.</span></span> <span data-ttu-id="e5d45-132">如果`Inherited`類別是屬性，則為 false，則不會繼承該屬性。</span><span class="sxs-lookup"><span data-stu-id="e5d45-132">If `Inherited` for an attribute class is false then that attribute is not inherited.</span></span> <span data-ttu-id="e5d45-133">如果未指定，其預設值為 true。</span><span class="sxs-lookup"><span data-stu-id="e5d45-133">If it is unspecified, its default value is true.</span></span>

<span data-ttu-id="e5d45-134">屬性類別`X`沒有`AttributeUsage`，附加做為中的屬性</span><span class="sxs-lookup"><span data-stu-id="e5d45-134">An attribute class `X` not having an `AttributeUsage` attribute attached to it, as in</span></span>

```csharp
using System;

class X: Attribute {...}
```

<span data-ttu-id="e5d45-135">相當於下列：</span><span class="sxs-lookup"><span data-stu-id="e5d45-135">is equivalent to the following:</span></span>

```csharp
using System;

[AttributeUsage(
    AttributeTargets.All,
    AllowMultiple = false,
    Inherited = true)
]
class X: Attribute {...}
```

### <a name="positional-and-named-parameters"></a><span data-ttu-id="e5d45-136">位置和具名參數</span><span class="sxs-lookup"><span data-stu-id="e5d45-136">Positional and named parameters</span></span>

<span data-ttu-id="e5d45-137">屬性的類別可以有***位置參數***並***具名參數***。</span><span class="sxs-lookup"><span data-stu-id="e5d45-137">Attribute classes can have ***positional parameters*** and ***named parameters***.</span></span> <span data-ttu-id="e5d45-138">屬性類別的每個公用執行個體建構函式會定義該屬性類別的位置參數是有效的序列。</span><span class="sxs-lookup"><span data-stu-id="e5d45-138">Each public instance constructor for an attribute class defines a valid sequence of positional parameters for that attribute class.</span></span> <span data-ttu-id="e5d45-139">每個非靜態公用讀寫欄位和屬性類別的屬性會定義屬性類別的具名的參數。</span><span class="sxs-lookup"><span data-stu-id="e5d45-139">Each non-static public read-write field and property for an attribute class defines a named parameter for the attribute class.</span></span>

<span data-ttu-id="e5d45-140">此範例</span><span class="sxs-lookup"><span data-stu-id="e5d45-140">The example</span></span>

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

<span data-ttu-id="e5d45-141">定義屬性類別名為`HelpAttribute`有一個位置的參數， `url`，和一個具名的參數， `Topic`。</span><span class="sxs-lookup"><span data-stu-id="e5d45-141">defines an attribute class named `HelpAttribute` that has one positional parameter, `url`, and one named parameter, `Topic`.</span></span> <span data-ttu-id="e5d45-142">雖然非靜態、 公用屬性`Url`不會定義一個具名的參數，因為它不是讀寫。</span><span class="sxs-lookup"><span data-stu-id="e5d45-142">Although it is non-static and public, the property `Url` does not define a named parameter, since it is not read-write.</span></span>

<span data-ttu-id="e5d45-143">可能會使用這個屬性類別如下所示：</span><span class="sxs-lookup"><span data-stu-id="e5d45-143">This attribute class might be used as follows:</span></span>

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

### <a name="attribute-parameter-types"></a><span data-ttu-id="e5d45-144">屬性參數類型</span><span class="sxs-lookup"><span data-stu-id="e5d45-144">Attribute parameter types</span></span>

<span data-ttu-id="e5d45-145">屬性類別的位置和具名參數的類型僅限於***屬性的參數型別***，這是：</span><span class="sxs-lookup"><span data-stu-id="e5d45-145">The types of positional and named parameters for an attribute class are limited to the ***attribute parameter types***, which are:</span></span>

*  <span data-ttu-id="e5d45-146">下列類型之一： `bool`， `byte`， `char`， `double`， `float`， `int`， `long`， `sbyte`， `short`， `string`， `uint`， `ulong`，`ushort`.</span><span class="sxs-lookup"><span data-stu-id="e5d45-146">One of the following types: `bool`, `byte`, `char`, `double`, `float`, `int`, `long`, `sbyte`, `short`, `string`, `uint`, `ulong`, `ushort`.</span></span>
*  <span data-ttu-id="e5d45-147">`object` 類型。</span><span class="sxs-lookup"><span data-stu-id="e5d45-147">The type `object`.</span></span>
*  <span data-ttu-id="e5d45-148">`System.Type` 類型。</span><span class="sxs-lookup"><span data-stu-id="e5d45-148">The type `System.Type`.</span></span>
*  <span data-ttu-id="e5d45-149">列舉型別，提供它具有公用存取範圍，且在它巢狀 （如果有的話） 的型別也有公用存取範圍 ([屬性規格](attributes.md#attribute-specification))。</span><span class="sxs-lookup"><span data-stu-id="e5d45-149">An enum type, provided it has public accessibility and the types in which it is nested (if any) also have public accessibility ([Attribute specification](attributes.md#attribute-specification)).</span></span>
*  <span data-ttu-id="e5d45-150">上述類型的一維陣列。</span><span class="sxs-lookup"><span data-stu-id="e5d45-150">Single-dimensional arrays of the above types.</span></span>
*  <span data-ttu-id="e5d45-151">建構函式引數或公用欄位沒有其中一個類型，不能作為屬性規格中的位置或具名參數。</span><span class="sxs-lookup"><span data-stu-id="e5d45-151">A constructor argument or public field which does not have one of these types, cannot be used as a positional or named parameter in an attribute specification.</span></span>

## <a name="attribute-specification"></a><span data-ttu-id="e5d45-152">屬性規格</span><span class="sxs-lookup"><span data-stu-id="e5d45-152">Attribute specification</span></span>

<span data-ttu-id="e5d45-153">***屬性規格***是先前定義的屬性宣告的應用程式。</span><span class="sxs-lookup"><span data-stu-id="e5d45-153">***Attribute specification*** is the application of a previously defined attribute to a declaration.</span></span> <span data-ttu-id="e5d45-154">屬性是一種指定宣告的其他宣告資訊。</span><span class="sxs-lookup"><span data-stu-id="e5d45-154">An attribute is a piece of additional declarative information that is specified for a declaration.</span></span> <span data-ttu-id="e5d45-155">屬性可以指定在全域範圍 （以指定包含組件或模組上的屬性） 以及*type_declaration*s ([型別宣告](namespaces.md#type-declarations))， *class_member_declaration*s ([類型參數條件約束](classes.md#type-parameter-constraints))， *interface_member_declaration*s ([介面成員](interfaces.md#interface-members))， *struct_member_declaration*s ([結構成員](structs.md#struct-members))， *enum_member_declaration*s ([列舉成員](enums.md#enum-members))， *accessor_declarations* ([存取子](classes.md#accessors))， *event_accessor_declarations* ([欄位型事件](classes.md#field-like-events))，和*formal_parameter_list*s ([方法參數](classes.md#method-parameters))。</span><span class="sxs-lookup"><span data-stu-id="e5d45-155">Attributes can be specified at global scope (to specify attributes on the containing assembly or module) and for *type_declaration*s ([Type declarations](namespaces.md#type-declarations)), *class_member_declaration*s ([Type parameter constraints](classes.md#type-parameter-constraints)), *interface_member_declaration*s ([Interface members](interfaces.md#interface-members)), *struct_member_declaration*s ([Struct members](structs.md#struct-members)), *enum_member_declaration*s ([Enum members](enums.md#enum-members)), *accessor_declarations* ([Accessors](classes.md#accessors)), *event_accessor_declarations* ([Field-like events](classes.md#field-like-events)), and *formal_parameter_list*s ([Method parameters](classes.md#method-parameters)).</span></span>

<span data-ttu-id="e5d45-156">屬性中指定***屬性區段***。</span><span class="sxs-lookup"><span data-stu-id="e5d45-156">Attributes are specified in ***attribute sections***.</span></span> <span data-ttu-id="e5d45-157">屬性區段包含一組的方括號括住的一或多個屬性的逗號分隔清單。</span><span class="sxs-lookup"><span data-stu-id="e5d45-157">An attribute section consists of a pair of square brackets, which surround a comma-separated list of one or more attributes.</span></span> <span data-ttu-id="e5d45-158">在這類清單中，指定屬性和在其中區段附加至相同的程式實體的順序排列的順序並不重要。</span><span class="sxs-lookup"><span data-stu-id="e5d45-158">The order in which attributes are specified in such a list, and the order in which sections attached to the same program entity are arranged, is not significant.</span></span> <span data-ttu-id="e5d45-159">比方說，屬性規格`[A][B]`， `[B][A]`， `[A,B]`，和`[B,A]`相等。</span><span class="sxs-lookup"><span data-stu-id="e5d45-159">For instance, the attribute specifications `[A][B]`, `[B][A]`, `[A,B]`, and `[B,A]` are equivalent.</span></span>

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

<span data-ttu-id="e5d45-160">屬性包含*attribute_name*和選擇性的位置和具名引數清單。</span><span class="sxs-lookup"><span data-stu-id="e5d45-160">An attribute consists of an *attribute_name* and an optional list of positional and named arguments.</span></span> <span data-ttu-id="e5d45-161">位置的引數 （如果有的話） 之前的具名引數。</span><span class="sxs-lookup"><span data-stu-id="e5d45-161">The positional arguments (if any) precede the named arguments.</span></span> <span data-ttu-id="e5d45-162">位置引數所組成*attribute_argument_expression*; 在具名引數所組成的名稱，後面接著等號，後面接著*attribute_argument_expression*，它會在一起會受限於與簡單指派相同的規則。</span><span class="sxs-lookup"><span data-stu-id="e5d45-162">A positional argument consists of an *attribute_argument_expression*; a named argument consists of a name, followed by an equal sign, followed by an *attribute_argument_expression*, which, together, are constrained by the same rules as simple assignment.</span></span> <span data-ttu-id="e5d45-163">具名引數的順序並不重要。</span><span class="sxs-lookup"><span data-stu-id="e5d45-163">The order of named arguments is not significant.</span></span>

<span data-ttu-id="e5d45-164">*Attribute_name*識別屬性類別。</span><span class="sxs-lookup"><span data-stu-id="e5d45-164">The *attribute_name* identifies an attribute class.</span></span> <span data-ttu-id="e5d45-165">如果格式*attribute_name*是*type_name*則此名稱必須參考屬性類別。</span><span class="sxs-lookup"><span data-stu-id="e5d45-165">If the form of *attribute_name* is *type_name* then this name must refer to an attribute class.</span></span> <span data-ttu-id="e5d45-166">否則，會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="e5d45-166">Otherwise, a compile-time error occurs.</span></span> <span data-ttu-id="e5d45-167">此範例</span><span class="sxs-lookup"><span data-stu-id="e5d45-167">The example</span></span>

```csharp
class Class1 {}

[Class1] class Class2 {}    // Error
```

<span data-ttu-id="e5d45-168">導致編譯時期錯誤，因為它會嘗試使用`Class1`做為屬性類別時`Class1`不是屬性類別。</span><span class="sxs-lookup"><span data-stu-id="e5d45-168">results in a compile-time error because it attempts to use `Class1` as an attribute class when `Class1` is not an attribute class.</span></span>

<span data-ttu-id="e5d45-169">某些內容會允許在多個目標上的屬性規格。</span><span class="sxs-lookup"><span data-stu-id="e5d45-169">Certain contexts permit the specification of an attribute on more than one target.</span></span> <span data-ttu-id="e5d45-170">程式可以明確指定目標包括*attribute_target_specifier*。</span><span class="sxs-lookup"><span data-stu-id="e5d45-170">A program can explicitly specify the target by including an *attribute_target_specifier*.</span></span> <span data-ttu-id="e5d45-171">屬性會放在全域層級中，當*global_attribute_target_specifier*需要。</span><span class="sxs-lookup"><span data-stu-id="e5d45-171">When an attribute is placed at the global level, a *global_attribute_target_specifier* is required.</span></span> <span data-ttu-id="e5d45-172">在所有其他位置，會套用合理的預設值，但是*attribute_target_specifier*可用來確認，或覆寫預設值，在某些模稜兩可的情況下 （或只是確認中非模稜兩可的情況下的預設值）。</span><span class="sxs-lookup"><span data-stu-id="e5d45-172">In all other locations, a reasonable default is applied, but an *attribute_target_specifier* can be used to affirm or override the default in certain ambiguous cases (or to just affirm the default in non-ambiguous cases).</span></span> <span data-ttu-id="e5d45-173">因此，通常*attribute_target_specifier*s，則可以省略以外的全域層級。</span><span class="sxs-lookup"><span data-stu-id="e5d45-173">Thus, typically, *attribute_target_specifier*s can be omitted except at the global level.</span></span> <span data-ttu-id="e5d45-174">解析模稜兩可的內容，如下所示：</span><span class="sxs-lookup"><span data-stu-id="e5d45-174">The potentially ambiguous contexts are resolved as follows:</span></span>

*  <span data-ttu-id="e5d45-175">指定在全域範圍的屬性可以套用至目標組件或目標模組。</span><span class="sxs-lookup"><span data-stu-id="e5d45-175">An attribute specified at global scope can apply either to the target assembly or the target module.</span></span> <span data-ttu-id="e5d45-176">沒有預設值存在此內容中，因此*attribute_target_specifier*一律需要在此內容中。</span><span class="sxs-lookup"><span data-stu-id="e5d45-176">No default exists for this context, so an *attribute_target_specifier* is always required in this context.</span></span> <span data-ttu-id="e5d45-177">出現與否`assembly` *attribute_target_specifier*表示此屬性套用至目標組件，是否存在`module` *attribute_target_specifier*表示屬性會套用至目標模組。</span><span class="sxs-lookup"><span data-stu-id="e5d45-177">The presence of the `assembly` *attribute_target_specifier* indicates that the attribute applies to the target assembly; the presence of the `module` *attribute_target_specifier* indicates that the attribute applies to the target module.</span></span>
*  <span data-ttu-id="e5d45-178">在 delegate 宣告上指定的屬性可以套用至所宣告的委派或它的傳回值。</span><span class="sxs-lookup"><span data-stu-id="e5d45-178">An attribute specified on a delegate declaration can apply either to the delegate being declared or to its return value.</span></span> <span data-ttu-id="e5d45-179">如果沒有*attribute_target_specifier*，屬性會套用至委派。</span><span class="sxs-lookup"><span data-stu-id="e5d45-179">In the absence of an *attribute_target_specifier*, the attribute applies to the delegate.</span></span> <span data-ttu-id="e5d45-180">出現與否`type` *attribute_target_specifier*屬性會套用至委派，會指出是否存在`return` *attribute_target_specifier*表示屬性會套用至傳回值。</span><span class="sxs-lookup"><span data-stu-id="e5d45-180">The presence of the `type` *attribute_target_specifier* indicates that the attribute applies to the delegate; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="e5d45-181">在方法宣告上指定的屬性可以套用至所宣告的方法或其傳回值。</span><span class="sxs-lookup"><span data-stu-id="e5d45-181">An attribute specified on a method declaration can apply either to the method being declared or to its return value.</span></span> <span data-ttu-id="e5d45-182">如果沒有*attribute_target_specifier*，屬性會套用至方法。</span><span class="sxs-lookup"><span data-stu-id="e5d45-182">In the absence of an *attribute_target_specifier*, the attribute applies to the method.</span></span> <span data-ttu-id="e5d45-183">出現與否`method` *attribute_target_specifier*指出屬性會套用至方法，是否存在`return` *attribute_target_specifier*表示該屬性會套用至傳回值。</span><span class="sxs-lookup"><span data-stu-id="e5d45-183">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the method; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="e5d45-184">在運算子宣告中指定的屬性可以套用至宣告中的運算子或它的傳回值。</span><span class="sxs-lookup"><span data-stu-id="e5d45-184">An attribute specified on an operator declaration can apply either to the operator being declared or to its return value.</span></span> <span data-ttu-id="e5d45-185">如果沒有*attribute_target_specifier*，屬性會套用到運算子。</span><span class="sxs-lookup"><span data-stu-id="e5d45-185">In the absence of an *attribute_target_specifier*, the attribute applies to the operator.</span></span> <span data-ttu-id="e5d45-186">出現與否`method` *attribute_target_specifier*指出屬性適用於運算子; 是否存在`return` *attribute_target_specifier*表示屬性會套用至傳回值。</span><span class="sxs-lookup"><span data-stu-id="e5d45-186">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the operator; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="e5d45-187">省略事件存取子的事件宣告中所指定的屬性可以套用至所宣告的事件、 相關聯的欄位 （如果事件不是抽象的），或相關聯的 add 和 remove 方法。</span><span class="sxs-lookup"><span data-stu-id="e5d45-187">An attribute specified on an event declaration that omits event accessors can apply to the event being declared, to the associated field (if the event is not abstract), or to the associated add and remove methods.</span></span> <span data-ttu-id="e5d45-188">如果沒有*attribute_target_specifier*，屬性會套用至事件。</span><span class="sxs-lookup"><span data-stu-id="e5d45-188">In the absence of an *attribute_target_specifier*, the attribute applies to the event.</span></span> <span data-ttu-id="e5d45-189">出現與否`event` *attribute_target_specifier*指出屬性會套用至事件，是否存在`field` *attribute_target_specifier*表示屬性會套用至欄位;功能和表現做`method` *attribute_target_specifier*指出屬性會套用至方法。</span><span class="sxs-lookup"><span data-stu-id="e5d45-189">The presence of the `event` *attribute_target_specifier* indicates that the attribute applies to the event; the presence of the `field` *attribute_target_specifier* indicates that the attribute applies to the field; and the presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the methods.</span></span>
*  <span data-ttu-id="e5d45-190">Get 存取子宣告屬性或索引子的宣告上指定的屬性可以套用至相關聯的方法，或為其傳回的值。</span><span class="sxs-lookup"><span data-stu-id="e5d45-190">An attribute specified on a get accessor declaration for a property or indexer declaration can apply either to the associated method or to its return value.</span></span> <span data-ttu-id="e5d45-191">如果沒有*attribute_target_specifier*，屬性會套用至方法。</span><span class="sxs-lookup"><span data-stu-id="e5d45-191">In the absence of an *attribute_target_specifier*, the attribute applies to the method.</span></span> <span data-ttu-id="e5d45-192">出現與否`method` *attribute_target_specifier*指出屬性會套用至方法，是否存在`return` *attribute_target_specifier*表示該屬性會套用至傳回值。</span><span class="sxs-lookup"><span data-stu-id="e5d45-192">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the method; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="e5d45-193">Set 存取子屬性或索引子宣告中所指定的屬性可以套用至相關聯的方法，或為其獨立的隱含參數。</span><span class="sxs-lookup"><span data-stu-id="e5d45-193">An attribute specified on a set accessor for a property or indexer declaration can apply either to the associated method or to its lone implicit parameter.</span></span> <span data-ttu-id="e5d45-194">如果沒有*attribute_target_specifier*，屬性會套用至方法。</span><span class="sxs-lookup"><span data-stu-id="e5d45-194">In the absence of an *attribute_target_specifier*, the attribute applies to the method.</span></span> <span data-ttu-id="e5d45-195">出現與否`method` *attribute_target_specifier*指出屬性會套用至方法，是否存在`param` *attribute_target_specifier*表示屬性會套用至參數;出現與否`return` *attribute_target_specifier*指出屬性會套用至傳回值。</span><span class="sxs-lookup"><span data-stu-id="e5d45-195">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the method; the presence of the `param` *attribute_target_specifier* indicates that the attribute applies to the parameter; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="e5d45-196">事件宣告可以套用至相關聯的方法，或為其單一參數，add 或 remove 存取子宣告上指定的屬性。</span><span class="sxs-lookup"><span data-stu-id="e5d45-196">An attribute specified on an add or remove accessor declaration for an event declaration can apply either to the associated method or to its lone parameter.</span></span> <span data-ttu-id="e5d45-197">如果沒有*attribute_target_specifier*，屬性會套用至方法。</span><span class="sxs-lookup"><span data-stu-id="e5d45-197">In the absence of an *attribute_target_specifier*, the attribute applies to the method.</span></span> <span data-ttu-id="e5d45-198">出現與否`method` *attribute_target_specifier*指出屬性會套用至方法，是否存在`param` *attribute_target_specifier*表示屬性會套用至參數;出現與否`return` *attribute_target_specifier*指出屬性會套用至傳回值。</span><span class="sxs-lookup"><span data-stu-id="e5d45-198">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the method; the presence of the `param` *attribute_target_specifier* indicates that the attribute applies to the parameter; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>

<span data-ttu-id="e5d45-199">在其他內容中，包含*attribute_target_specifier*是允許但非必要。</span><span class="sxs-lookup"><span data-stu-id="e5d45-199">In other contexts, inclusion of an *attribute_target_specifier* is permitted but unnecessary.</span></span> <span data-ttu-id="e5d45-200">比方說，在類別宣告可能包含或省略規範`type`:</span><span class="sxs-lookup"><span data-stu-id="e5d45-200">For instance, a class declaration may either include or omit the specifier `type`:</span></span>

```csharp
[type: Author("Brian Kernighan")]
class Class1 {}

[Author("Dennis Ritchie")]
class Class2 {}
```

<span data-ttu-id="e5d45-201">它是指定了無效的錯誤*attribute_target_specifier*。</span><span class="sxs-lookup"><span data-stu-id="e5d45-201">It is an error to specify an invalid *attribute_target_specifier*.</span></span> <span data-ttu-id="e5d45-202">比方說，規範`param`不能在類別宣告：</span><span class="sxs-lookup"><span data-stu-id="e5d45-202">For instance, the specifier `param` cannot be used on a class declaration:</span></span>

```csharp
[param: Author("Brian Kernighan")]        // Error
class Class1 {}
```

<span data-ttu-id="e5d45-203">依照慣例，屬性類別會命名為尾碼`Attribute`。</span><span class="sxs-lookup"><span data-stu-id="e5d45-203">By convention, attribute classes are named with a suffix of `Attribute`.</span></span> <span data-ttu-id="e5d45-204">*Attribute_name*的表單*type_name*可能包含或省略此尾碼。</span><span class="sxs-lookup"><span data-stu-id="e5d45-204">An *attribute_name* of the form *type_name* may either include or omit this suffix.</span></span> <span data-ttu-id="e5d45-205">如果找到屬性類別使用和不含此尾碼，模稜兩可存在，而且會產生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="e5d45-205">If an attribute class is found both with and without this suffix, an ambiguity is present, and a compile-time error results.</span></span> <span data-ttu-id="e5d45-206">如果*attribute_name*拼寫，其最右邊*識別項*是逐字識別項 ([識別碼](lexical-structure.md#identifiers))，則沒有尾碼的屬性會比對，如此一來解決這種模稜兩可。</span><span class="sxs-lookup"><span data-stu-id="e5d45-206">If the *attribute_name* is spelled such that its right-most *identifier* is a verbatim identifier ([Identifiers](lexical-structure.md#identifiers)), then only an attribute without a suffix is matched, thus enabling such an ambiguity to be resolved.</span></span> <span data-ttu-id="e5d45-207">此範例</span><span class="sxs-lookup"><span data-stu-id="e5d45-207">The example</span></span>

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

<span data-ttu-id="e5d45-208">顯示兩個屬性類別叫做`X`和`XAttribute`。</span><span class="sxs-lookup"><span data-stu-id="e5d45-208">shows two attribute classes named `X` and `XAttribute`.</span></span> <span data-ttu-id="e5d45-209">屬性`[X]`模稜兩可，因為它無法參考其中一個`X`或`XAttribute`。</span><span class="sxs-lookup"><span data-stu-id="e5d45-209">The attribute `[X]` is ambiguous, since it could refer to either `X` or `XAttribute`.</span></span> <span data-ttu-id="e5d45-210">使用逐字識別項，可讓在這類罕見的情況下指定確切的意圖。</span><span class="sxs-lookup"><span data-stu-id="e5d45-210">Using a verbatim identifier allows the exact intent to be specified in such rare cases.</span></span> <span data-ttu-id="e5d45-211">屬性`[XAttribute]`不是模稜兩可 (不過如果是屬性類別名為`XAttributeAttribute`！)。</span><span class="sxs-lookup"><span data-stu-id="e5d45-211">The attribute `[XAttribute]` is not ambiguous (although it would be if there was an attribute class named `XAttributeAttribute`!).</span></span> <span data-ttu-id="e5d45-212">如果類別的宣告`X`移除，則這兩個屬性會參考名為的屬性類別`XAttribute`、，如下所示：</span><span class="sxs-lookup"><span data-stu-id="e5d45-212">If the declaration for class `X` is removed, then both attributes refer to the attribute class named `XAttribute`, as follows:</span></span>

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

<span data-ttu-id="e5d45-213">它是編譯時期錯誤一次以上相同的實體使用單次使用的屬性類別。</span><span class="sxs-lookup"><span data-stu-id="e5d45-213">It is a compile-time error to use a single-use attribute class more than once on the same entity.</span></span> <span data-ttu-id="e5d45-214">此範例</span><span class="sxs-lookup"><span data-stu-id="e5d45-214">The example</span></span>

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

<span data-ttu-id="e5d45-215">導致編譯時期錯誤，因為它會嘗試使用`HelpString`，這是單次使用屬性的類別，一次以上的宣告上`Class1`。</span><span class="sxs-lookup"><span data-stu-id="e5d45-215">results in a compile-time error because it attempts to use `HelpString`, which is a single-use attribute class, more than once on the declaration of `Class1`.</span></span>

<span data-ttu-id="e5d45-216">運算式`E`已*attribute_argument_expression*所有下列陳述式，則為 true 時：</span><span class="sxs-lookup"><span data-stu-id="e5d45-216">An expression `E` is an *attribute_argument_expression* if all of the following statements are true:</span></span>

*  <span data-ttu-id="e5d45-217">型別`E`是屬性參數類型 ([屬性參數類型](attributes.md#attribute-parameter-types))。</span><span class="sxs-lookup"><span data-stu-id="e5d45-217">The type of `E` is an attribute parameter type ([Attribute parameter types](attributes.md#attribute-parameter-types)).</span></span>
*  <span data-ttu-id="e5d45-218">在編譯時期，值`E`可解析成下列其中之一：</span><span class="sxs-lookup"><span data-stu-id="e5d45-218">At compile-time, the value of `E` can be resolved to one of the following:</span></span>
   * <span data-ttu-id="e5d45-219">常數值。</span><span class="sxs-lookup"><span data-stu-id="e5d45-219">A constant value.</span></span>
   * <span data-ttu-id="e5d45-220">`System.Type` 物件。</span><span class="sxs-lookup"><span data-stu-id="e5d45-220">A `System.Type` object.</span></span>
   * <span data-ttu-id="e5d45-221">一維陣列*attribute_argument_expression*s。</span><span class="sxs-lookup"><span data-stu-id="e5d45-221">A one-dimensional array of *attribute_argument_expression*s.</span></span>

<span data-ttu-id="e5d45-222">例如: </span><span class="sxs-lookup"><span data-stu-id="e5d45-222">For example:</span></span>

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

<span data-ttu-id="e5d45-223">A *typeof_expression* ([typeof 運算子](expressions.md#the-typeof-operator)) 做為屬性引數運算式可以參考的非泛型型別、 封閉式建構的類型或未繫結的泛型型別，但它不能參考開啟類型。</span><span class="sxs-lookup"><span data-stu-id="e5d45-223">A *typeof_expression* ([The typeof operator](expressions.md#the-typeof-operator)) used as an attribute argument expression can reference a non-generic type, a closed constructed type, or an unbound generic type, but it cannot reference an open type.</span></span> <span data-ttu-id="e5d45-224">這是為了確保可以在編譯時期已解析運算式。</span><span class="sxs-lookup"><span data-stu-id="e5d45-224">This is to ensure that the expression can be resolved at compile-time.</span></span>

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

## <a name="attribute-instances"></a><span data-ttu-id="e5d45-225">屬性執行個體</span><span class="sxs-lookup"><span data-stu-id="e5d45-225">Attribute instances</span></span>

<span data-ttu-id="e5d45-226">***屬性執行個體***是執行個體，表示在執行階段屬性。</span><span class="sxs-lookup"><span data-stu-id="e5d45-226">An ***attribute instance*** is an instance that represents an attribute at run-time.</span></span> <span data-ttu-id="e5d45-227">屬性是定義為具有屬性類別，位置引數，而具名引數。</span><span class="sxs-lookup"><span data-stu-id="e5d45-227">An attribute is defined with an attribute class, positional arguments, and named arguments.</span></span> <span data-ttu-id="e5d45-228">屬性執行個體是使用位置和具名引數初始化屬性類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="e5d45-228">An attribute instance is an instance of the attribute class that is initialized with the positional and named arguments.</span></span>

<span data-ttu-id="e5d45-229">擷取的屬性執行個體牽涉到編譯時期和執行階段處理，如下列各節中所述。</span><span class="sxs-lookup"><span data-stu-id="e5d45-229">Retrieval of an attribute instance involves both compile-time and run-time processing, as described in the following sections.</span></span>

### <a name="compilation-of-an-attribute"></a><span data-ttu-id="e5d45-230">屬性的編譯</span><span class="sxs-lookup"><span data-stu-id="e5d45-230">Compilation of an attribute</span></span>

<span data-ttu-id="e5d45-231">編譯*屬性*屬性類別與`T`， *positional_argument_list* `P`並*named_argument_list* `N`，包含下列步驟：</span><span class="sxs-lookup"><span data-stu-id="e5d45-231">The compilation of an *attribute* with attribute class `T`, *positional_argument_list* `P` and *named_argument_list* `N`, consists of the following steps:</span></span>

*  <span data-ttu-id="e5d45-232">請依照下列編譯時間處理步驟，來編譯*object_creation_expression*表單的`new T(P)`。</span><span class="sxs-lookup"><span data-stu-id="e5d45-232">Follow the compile-time processing steps for compiling an *object_creation_expression* of the form `new T(P)`.</span></span> <span data-ttu-id="e5d45-233">這些步驟會導致編譯時期錯誤，或判斷執行個體建構函式`C`上`T`，可以在執行階段叫用。</span><span class="sxs-lookup"><span data-stu-id="e5d45-233">These steps either result in a compile-time error, or determine an instance constructor `C` on `T` that can be invoked at run-time.</span></span>
*  <span data-ttu-id="e5d45-234">如果`C`並沒有公用存取範圍，則會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="e5d45-234">If `C` does not have public accessibility, then a compile-time error occurs.</span></span>
*  <span data-ttu-id="e5d45-235">每個*named_argument* `Arg`在`N`:</span><span class="sxs-lookup"><span data-stu-id="e5d45-235">For each *named_argument* `Arg` in `N`:</span></span>
   * <span data-ttu-id="e5d45-236">可讓`Name`能*識別項*的*named_argument* `Arg`。</span><span class="sxs-lookup"><span data-stu-id="e5d45-236">Let `Name` be the *identifier* of the *named_argument* `Arg`.</span></span>
   * <span data-ttu-id="e5d45-237">`Name` 必須識別的非靜態唯讀公用欄位或屬性上`T`。</span><span class="sxs-lookup"><span data-stu-id="e5d45-237">`Name` must identify a non-static read-write public field or property on `T`.</span></span> <span data-ttu-id="e5d45-238">如果`T`有沒有這類欄位或屬性，則會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="e5d45-238">If `T` has no such field or property, then a compile-time error occurs.</span></span>
*  <span data-ttu-id="e5d45-239">請為執行階段具現化屬性的下列資訊： 屬性類別`T`，執行個體建構函式`C`上`T`，則*positional_argument_list* `P`而*named_argument_list* `N`。</span><span class="sxs-lookup"><span data-stu-id="e5d45-239">Keep the following information for run-time instantiation of the attribute: the attribute class `T`, the instance constructor `C` on `T`, the *positional_argument_list* `P` and the *named_argument_list* `N`.</span></span>

### <a name="run-time-retrieval-of-an-attribute-instance"></a><span data-ttu-id="e5d45-240">執行階段擷取的屬性執行個體</span><span class="sxs-lookup"><span data-stu-id="e5d45-240">Run-time retrieval of an attribute instance</span></span>

<span data-ttu-id="e5d45-241">編譯*屬性*產生屬性類別`T`，執行個體建構函式`C`上`T`，則*positional_argument_list* `P`，和*named_argument_list* `N`。</span><span class="sxs-lookup"><span data-stu-id="e5d45-241">Compilation of an *attribute* yields an attribute class `T`, an instance constructor `C` on `T`, a *positional_argument_list* `P`, and a *named_argument_list* `N`.</span></span> <span data-ttu-id="e5d45-242">經由這項資訊，可以在執行階段使用下列步驟擷取的屬性執行個體：</span><span class="sxs-lookup"><span data-stu-id="e5d45-242">Given this information, an attribute instance can be retrieved at run-time using the following steps:</span></span>

*  <span data-ttu-id="e5d45-243">遵循執行的執行階段處理步驟*object_creation_expression*的表單`new T(P)`，使用的執行個體建構函式`C`在編譯時期決定。</span><span class="sxs-lookup"><span data-stu-id="e5d45-243">Follow the run-time processing steps for executing an *object_creation_expression* of the form `new T(P)`, using the instance constructor `C` as determined at compile-time.</span></span> <span data-ttu-id="e5d45-244">這些步驟會導致例外狀況，或產生執行個體`O`的`T`。</span><span class="sxs-lookup"><span data-stu-id="e5d45-244">These steps either result in an exception, or produce an instance `O` of `T`.</span></span>
*  <span data-ttu-id="e5d45-245">每個*named_argument* `Arg`在`N`，順序：</span><span class="sxs-lookup"><span data-stu-id="e5d45-245">For each *named_argument* `Arg` in `N`, in order:</span></span>
   * <span data-ttu-id="e5d45-246">可讓`Name`能*識別項*的*named_argument* `Arg`。</span><span class="sxs-lookup"><span data-stu-id="e5d45-246">Let `Name` be the *identifier* of the *named_argument* `Arg`.</span></span> <span data-ttu-id="e5d45-247">如果`Name`不會識別的非靜態公用讀寫欄位或屬性上`O`，則會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="e5d45-247">If `Name` does not identify a non-static public read-write field or property on `O`, then an exception is thrown.</span></span>
   * <span data-ttu-id="e5d45-248">可讓`Value`要評估的結果*attribute_argument_expression*的`Arg`。</span><span class="sxs-lookup"><span data-stu-id="e5d45-248">Let `Value` be the result of evaluating the *attribute_argument_expression* of `Arg`.</span></span>
   * <span data-ttu-id="e5d45-249">如果`Name`識別要在欄位`O`，然後將此欄位設定為`Value`。</span><span class="sxs-lookup"><span data-stu-id="e5d45-249">If `Name` identifies a field on `O`, then set this field to `Value`.</span></span>
   * <span data-ttu-id="e5d45-250">否則，請`Name`識別的屬性上`O`。</span><span class="sxs-lookup"><span data-stu-id="e5d45-250">Otherwise, `Name` identifies a property on `O`.</span></span> <span data-ttu-id="e5d45-251">將此屬性設定為`Value`。</span><span class="sxs-lookup"><span data-stu-id="e5d45-251">Set this property to `Value`.</span></span>
   * <span data-ttu-id="e5d45-252">結果是`O`，屬性類別的執行個體`T`，已使用初始化*positional_argument_list* `P`並*named_argument_list*`N`.</span><span class="sxs-lookup"><span data-stu-id="e5d45-252">The result is `O`, an instance of the attribute class `T` that has been initialized with the *positional_argument_list* `P` and the *named_argument_list* `N`.</span></span>

## <a name="reserved-attributes"></a><span data-ttu-id="e5d45-253">保留的屬性</span><span class="sxs-lookup"><span data-stu-id="e5d45-253">Reserved attributes</span></span>

<span data-ttu-id="e5d45-254">少數的屬性會影響以某種方式的語言。</span><span class="sxs-lookup"><span data-stu-id="e5d45-254">A small number of attributes affect the language in some way.</span></span> <span data-ttu-id="e5d45-255">這些屬性包括：</span><span class="sxs-lookup"><span data-stu-id="e5d45-255">These attributes include:</span></span>

*  <span data-ttu-id="e5d45-256">`System.AttributeUsageAttribute` ([AttributeUsage 屬性](attributes.md#the-attributeusage-attribute))，這用來描述屬性類別可以使用的方式。</span><span class="sxs-lookup"><span data-stu-id="e5d45-256">`System.AttributeUsageAttribute` ([The AttributeUsage attribute](attributes.md#the-attributeusage-attribute)), which is used to describe the ways in which an attribute class can be used.</span></span>
*  <span data-ttu-id="e5d45-257">`System.Diagnostics.ConditionalAttribute` ([Conditional 屬性](attributes.md#the-conditional-attribute))，這用來定義條件式方法。</span><span class="sxs-lookup"><span data-stu-id="e5d45-257">`System.Diagnostics.ConditionalAttribute` ([The Conditional attribute](attributes.md#the-conditional-attribute)), which is used to define conditional methods.</span></span>
*  <span data-ttu-id="e5d45-258">`System.ObsoleteAttribute` ([Obsolete 屬性](attributes.md#the-obsolete-attribute))，以用來將標記為過時的成員。</span><span class="sxs-lookup"><span data-stu-id="e5d45-258">`System.ObsoleteAttribute` ([The Obsolete attribute](attributes.md#the-obsolete-attribute)), which is used to mark a member as obsolete.</span></span>
*  <span data-ttu-id="e5d45-259">`System.Runtime.CompilerServices.CallerLineNumberAttribute``System.Runtime.CompilerServices.CallerFilePathAttribute`並`System.Runtime.CompilerServices.CallerMemberNameAttribute`([呼叫端資訊屬性](attributes.md#caller-info-attributes))，這用來提供選擇性的參數呼叫內容的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="e5d45-259">`System.Runtime.CompilerServices.CallerLineNumberAttribute`, `System.Runtime.CompilerServices.CallerFilePathAttribute` and `System.Runtime.CompilerServices.CallerMemberNameAttribute` ([Caller info attributes](attributes.md#caller-info-attributes)), which are used to supply information about the calling context to optional parameters.</span></span>

### <a name="the-attributeusage-attribute"></a><span data-ttu-id="e5d45-260">AttributeUsage 屬性</span><span class="sxs-lookup"><span data-stu-id="e5d45-260">The AttributeUsage attribute</span></span>

<span data-ttu-id="e5d45-261">屬性`AttributeUsage`用來描述可以在其中使用屬性類別的方式。</span><span class="sxs-lookup"><span data-stu-id="e5d45-261">The attribute `AttributeUsage` is used to describe the manner in which the attribute class can be used.</span></span>

<span data-ttu-id="e5d45-262">裝飾的類別`AttributeUsage`屬性必須衍生自`System.Attribute`，直接或間接。</span><span class="sxs-lookup"><span data-stu-id="e5d45-262">A class that is decorated with the `AttributeUsage` attribute must derive from `System.Attribute`, either directly or indirectly.</span></span> <span data-ttu-id="e5d45-263">否則，會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="e5d45-263">Otherwise, a compile-time error occurs.</span></span>

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

### <a name="the-conditional-attribute"></a><span data-ttu-id="e5d45-264">Conditional 屬性</span><span class="sxs-lookup"><span data-stu-id="e5d45-264">The Conditional attribute</span></span>

<span data-ttu-id="e5d45-265">屬性`Conditional`讓您定義的***條件式方法***並***conditional 屬性類別***。</span><span class="sxs-lookup"><span data-stu-id="e5d45-265">The attribute `Conditional` enables the definition of ***conditional methods*** and ***conditional attribute classes***.</span></span>

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

#### <a name="conditional-methods"></a><span data-ttu-id="e5d45-266">條件式方法</span><span class="sxs-lookup"><span data-stu-id="e5d45-266">Conditional methods</span></span>

<span data-ttu-id="e5d45-267">使用裝飾方法`Conditional`屬性是條件式方法。</span><span class="sxs-lookup"><span data-stu-id="e5d45-267">A method decorated with the `Conditional` attribute is a conditional method.</span></span> <span data-ttu-id="e5d45-268">`Conditional`屬性表示一種情況，藉由測試條件式編譯符號。</span><span class="sxs-lookup"><span data-stu-id="e5d45-268">The `Conditional` attribute indicates a condition by testing a conditional compilation symbol.</span></span> <span data-ttu-id="e5d45-269">條件式方法的呼叫包含或省略取決於是否定義在呼叫的這個符號。</span><span class="sxs-lookup"><span data-stu-id="e5d45-269">Calls to a conditional method are either included or omitted depending on whether this symbol is defined at the point of the call.</span></span> <span data-ttu-id="e5d45-270">如果定義符號，則會包括呼叫;否則，會省略 （包括接收者的評估和呼叫的參數） 的呼叫。</span><span class="sxs-lookup"><span data-stu-id="e5d45-270">If the symbol is defined, the call is included; otherwise, the call (including evaluation of the receiver and parameters of the call) is omitted.</span></span>

<span data-ttu-id="e5d45-271">條件式方法受限於下列限制：</span><span class="sxs-lookup"><span data-stu-id="e5d45-271">A conditional method is subject to the following restrictions:</span></span>

*  <span data-ttu-id="e5d45-272">條件式方法必須是中的方法*class_declaration*或是*struct_declaration*。</span><span class="sxs-lookup"><span data-stu-id="e5d45-272">The conditional method must be a method in a *class_declaration* or *struct_declaration*.</span></span> <span data-ttu-id="e5d45-273">如果，就會發生編譯時期錯誤`Conditional`介面宣告中的方法上指定屬性。</span><span class="sxs-lookup"><span data-stu-id="e5d45-273">A compile-time error occurs if the `Conditional` attribute is specified on a method in an interface declaration.</span></span>
*  <span data-ttu-id="e5d45-274">條件式方法必須傳回型別`void`。</span><span class="sxs-lookup"><span data-stu-id="e5d45-274">The conditional method must have a return type of `void`.</span></span>
*  <span data-ttu-id="e5d45-275">條件式方法不能標示與`override`修飾詞。</span><span class="sxs-lookup"><span data-stu-id="e5d45-275">The conditional method must not be marked with the `override` modifier.</span></span> <span data-ttu-id="e5d45-276">條件式方法可能會標記為使用`virtual`修飾詞，不過。</span><span class="sxs-lookup"><span data-stu-id="e5d45-276">A conditional method may be marked with the `virtual` modifier, however.</span></span> <span data-ttu-id="e5d45-277">這種方法的覆寫隱含條件式，而且不能明確標示與`Conditional`屬性。</span><span class="sxs-lookup"><span data-stu-id="e5d45-277">Overrides of such a method are implicitly conditional, and must not be explicitly marked with a `Conditional` attribute.</span></span>
*  <span data-ttu-id="e5d45-278">條件式方法不能實作介面方法。</span><span class="sxs-lookup"><span data-stu-id="e5d45-278">The conditional method must not be an implementation of an interface method.</span></span> <span data-ttu-id="e5d45-279">否則，會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="e5d45-279">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="e5d45-280">如果條件式方法會在編譯時期錯誤將會發生颾魤 ㄛ *delegate_creation_expression*。</span><span class="sxs-lookup"><span data-stu-id="e5d45-280">In addition, a compile-time error occurs if a conditional method is used in a *delegate_creation_expression*.</span></span> <span data-ttu-id="e5d45-281">此範例</span><span class="sxs-lookup"><span data-stu-id="e5d45-281">The example</span></span>

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

<span data-ttu-id="e5d45-282">宣告`Class1.M`做為條件式方法。</span><span class="sxs-lookup"><span data-stu-id="e5d45-282">declares `Class1.M` as a conditional method.</span></span> <span data-ttu-id="e5d45-283">`Class2``Test`方法會呼叫這個方法。</span><span class="sxs-lookup"><span data-stu-id="e5d45-283">`Class2`'s `Test` method calls this method.</span></span> <span data-ttu-id="e5d45-284">因為條件式編譯符號`DEBUG`定義，如果`Class2.Test`是呼叫，它會呼叫`M`。</span><span class="sxs-lookup"><span data-stu-id="e5d45-284">Since the conditional compilation symbol `DEBUG` is defined, if `Class2.Test` is called, it will call `M`.</span></span> <span data-ttu-id="e5d45-285">如果符號`DEBUG`有未定義，然後`Class2.Test`不會呼叫`Class1.M`。</span><span class="sxs-lookup"><span data-stu-id="e5d45-285">If the symbol `DEBUG` had not been defined, then `Class2.Test` would not call `Class1.M`.</span></span>

<span data-ttu-id="e5d45-286">請務必請注意，包含或排除的條件式方法的呼叫會受到在呼叫的條件式編譯符號。</span><span class="sxs-lookup"><span data-stu-id="e5d45-286">It is important to note that the inclusion or exclusion of a call to a conditional method is controlled by the conditional compilation symbols at the point of the call.</span></span> <span data-ttu-id="e5d45-287">在範例</span><span class="sxs-lookup"><span data-stu-id="e5d45-287">In the example</span></span>

<span data-ttu-id="e5d45-288">檔案`class1.cs`:</span><span class="sxs-lookup"><span data-stu-id="e5d45-288">File `class1.cs`:</span></span>

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

<span data-ttu-id="e5d45-289">檔案`class2.cs`:</span><span class="sxs-lookup"><span data-stu-id="e5d45-289">File `class2.cs`:</span></span>

```csharp
#define DEBUG

class Class2
{
    public static void G() {
        Class1.F();                // F is called
    }
}
```

<span data-ttu-id="e5d45-290">檔案`class3.cs`:</span><span class="sxs-lookup"><span data-stu-id="e5d45-290">File `class3.cs`:</span></span>

```csharp
#undef DEBUG

class Class3
{
    public static void H() {
        Class1.F();                // F is not called
    }
}
```

<span data-ttu-id="e5d45-291">類別`Class2`並`Class3`每個包含條件式方法的呼叫`Class1.F`，也就是條件式根據是否`DEBUG`定義。</span><span class="sxs-lookup"><span data-stu-id="e5d45-291">the classes `Class2` and `Class3` each contain calls to the conditional method `Class1.F`, which is conditional based on whether or not `DEBUG` is defined.</span></span> <span data-ttu-id="e5d45-292">因為這個符號已定義的內容中`Class2`而非`Class3`，來呼叫`F`中`Class2`都會包括在內，而呼叫`F`中`Class3`省略。</span><span class="sxs-lookup"><span data-stu-id="e5d45-292">Since this symbol is defined in the context of `Class2` but not `Class3`, the call to `F` in `Class2` is included, while the call to `F` in `Class3` is omitted.</span></span>

<span data-ttu-id="e5d45-293">使用條件式方法的繼承鏈結中可能會造成混淆。</span><span class="sxs-lookup"><span data-stu-id="e5d45-293">The use of conditional methods in an inheritance chain can be confusing.</span></span> <span data-ttu-id="e5d45-294">透過條件式方法的呼叫`base`，在表單的`base.M`，受制於一般的條件式方法呼叫的規則。</span><span class="sxs-lookup"><span data-stu-id="e5d45-294">Calls made to a conditional method through `base`, of the form `base.M`, are subject to the normal conditional method call rules.</span></span> <span data-ttu-id="e5d45-295">在範例</span><span class="sxs-lookup"><span data-stu-id="e5d45-295">In the example</span></span>

<span data-ttu-id="e5d45-296">檔案`class1.cs`:</span><span class="sxs-lookup"><span data-stu-id="e5d45-296">File `class1.cs`:</span></span>

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

<span data-ttu-id="e5d45-297">檔案`class2.cs`:</span><span class="sxs-lookup"><span data-stu-id="e5d45-297">File `class2.cs`:</span></span>

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

<span data-ttu-id="e5d45-298">檔案`class3.cs`:</span><span class="sxs-lookup"><span data-stu-id="e5d45-298">File `class3.cs`:</span></span>

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

<span data-ttu-id="e5d45-299">`Class2` 包含呼叫`M`其基底類別中定義。</span><span class="sxs-lookup"><span data-stu-id="e5d45-299">`Class2` includes a call to the `M` defined in its base class.</span></span> <span data-ttu-id="e5d45-300">這個呼叫會省略，因為基底方法，是條件式根據符號的存在`DEBUG`，這是未定義。</span><span class="sxs-lookup"><span data-stu-id="e5d45-300">This call is omitted because the base method is conditional based on the presence of the symbol `DEBUG`, which is undefined.</span></span> <span data-ttu-id="e5d45-301">因此，此方法將寫入主控台"`Class2.M executed`」 只。</span><span class="sxs-lookup"><span data-stu-id="e5d45-301">Thus, the method writes to the console "`Class2.M executed`" only.</span></span> <span data-ttu-id="e5d45-302">明智地使用*pp_declaration*s 可以排除這類問題。</span><span class="sxs-lookup"><span data-stu-id="e5d45-302">Judicious use of *pp_declaration*s can eliminate such problems.</span></span>

#### <a name="conditional-attribute-classes"></a><span data-ttu-id="e5d45-303">Conditional 屬性類別</span><span class="sxs-lookup"><span data-stu-id="e5d45-303">Conditional attribute classes</span></span>

<span data-ttu-id="e5d45-304">屬性類別 ([屬性類別](attributes.md#attribute-classes)) 以一個或多個裝飾`Conditional`屬性是***conditional 屬性類別***。</span><span class="sxs-lookup"><span data-stu-id="e5d45-304">An attribute class ([Attribute classes](attributes.md#attribute-classes)) decorated with one or more `Conditional` attributes is a ***conditional attribute class***.</span></span> <span data-ttu-id="e5d45-305">Conditional 屬性類別有因此關聯中所宣告的條件式編譯符號其`Conditional`屬性。</span><span class="sxs-lookup"><span data-stu-id="e5d45-305">A conditional attribute class is thus associated with the conditional compilation symbols declared in its `Conditional` attributes.</span></span> <span data-ttu-id="e5d45-306">此範例中：</span><span class="sxs-lookup"><span data-stu-id="e5d45-306">This example:</span></span>

```csharp
using System;
using System.Diagnostics;
[Conditional("ALPHA")]
[Conditional("BETA")]
public class TestAttribute : Attribute {}
```

<span data-ttu-id="e5d45-307">宣告`TestAttribute`做為條件式編譯符號相關聯的 conditional 屬性類別`ALPHA`和`BETA`。</span><span class="sxs-lookup"><span data-stu-id="e5d45-307">declares `TestAttribute` as a conditional attribute class associated with the conditional compilations symbols `ALPHA` and `BETA`.</span></span>

<span data-ttu-id="e5d45-308">屬性規格 ([屬性規格](attributes.md#attribute-specification)) 的條件式屬性如果將包含一或多個與其相關聯的條件式編譯符號定義在規格中，否則屬性規格會省略。</span><span class="sxs-lookup"><span data-stu-id="e5d45-308">Attribute specifications ([Attribute specification](attributes.md#attribute-specification)) of a conditional attribute are included if one or more of its associated conditional compilation symbols is defined at the point of specification, otherwise the attribute specification is omitted.</span></span>

<span data-ttu-id="e5d45-309">請務必請注意，包含或排除的條件式屬性類別的屬性規格會受到規格的條件式編譯符號。</span><span class="sxs-lookup"><span data-stu-id="e5d45-309">It is important to note that the inclusion or exclusion of an attribute specification of a conditional attribute class is controlled by the conditional compilation symbols at the point of the specification.</span></span> <span data-ttu-id="e5d45-310">在範例</span><span class="sxs-lookup"><span data-stu-id="e5d45-310">In the example</span></span>

<span data-ttu-id="e5d45-311">檔案`test.cs`:</span><span class="sxs-lookup"><span data-stu-id="e5d45-311">File `test.cs`:</span></span>

```csharp
using System;
using System.Diagnostics;

[Conditional("DEBUG")]

public class TestAttribute : Attribute {}
```

<span data-ttu-id="e5d45-312">檔案`class1.cs`:</span><span class="sxs-lookup"><span data-stu-id="e5d45-312">File `class1.cs`:</span></span>

```csharp
#define DEBUG

[Test]                // TestAttribute is specified

class Class1 {}
```

<span data-ttu-id="e5d45-313">檔案`class2.cs`:</span><span class="sxs-lookup"><span data-stu-id="e5d45-313">File `class2.cs`:</span></span>

```csharp
#undef DEBUG

[Test]                 // TestAttribute is not specified

class Class2 {}
```

<span data-ttu-id="e5d45-314">類別`Class1`並`Class2`是每個屬性裝飾`Test`，也就是條件式根據是否`DEBUG`定義。</span><span class="sxs-lookup"><span data-stu-id="e5d45-314">the classes `Class1` and `Class2` are each decorated with attribute `Test`, which is conditional based on whether or not `DEBUG` is defined.</span></span> <span data-ttu-id="e5d45-315">因為這個符號已定義的內容中`Class1`而非`Class2`的規格`Test`屬性`Class1`就會包含在指定時`Test`屬性`Class2`省略。</span><span class="sxs-lookup"><span data-stu-id="e5d45-315">Since this symbol is defined in the context of `Class1` but not `Class2`, the specification of the `Test` attribute on `Class1` is included, while the specification of the `Test` attribute on `Class2` is omitted.</span></span>

### <a name="the-obsolete-attribute"></a><span data-ttu-id="e5d45-316">Obsolete 屬性</span><span class="sxs-lookup"><span data-stu-id="e5d45-316">The Obsolete attribute</span></span>

<span data-ttu-id="e5d45-317">屬性`Obsolete`用來標記不再使用的類型的類型和成員。</span><span class="sxs-lookup"><span data-stu-id="e5d45-317">The attribute `Obsolete` is used to mark types and members of types that should no longer be used.</span></span>

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

<span data-ttu-id="e5d45-318">如果程式使用的類型或成員裝飾`Obsolete`屬性，則編譯器會發出警告或錯誤。</span><span class="sxs-lookup"><span data-stu-id="e5d45-318">If a program uses a type or member that is decorated with the `Obsolete` attribute, the compiler issues a warning or an error.</span></span> <span data-ttu-id="e5d45-319">具體來說，編譯器會發出警告不提供任何錯誤的參數，則會提供錯誤的參數，且具有值`false`。</span><span class="sxs-lookup"><span data-stu-id="e5d45-319">Specifically, the compiler issues a warning if no error parameter is provided, or if the error parameter is provided and has the value `false`.</span></span> <span data-ttu-id="e5d45-320">如果錯誤參數指定，且具有值時，編譯器會發出錯誤`true`。</span><span class="sxs-lookup"><span data-stu-id="e5d45-320">The compiler issues an error if the error parameter is specified and has the value `true`.</span></span>

<span data-ttu-id="e5d45-321">在範例</span><span class="sxs-lookup"><span data-stu-id="e5d45-321">In the example</span></span>

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

<span data-ttu-id="e5d45-322">此類別`A`裝飾`Obsolete`屬性。</span><span class="sxs-lookup"><span data-stu-id="e5d45-322">the class `A` is decorated with the `Obsolete` attribute.</span></span> <span data-ttu-id="e5d45-323">每次使用`A`在`Main`時會產生警告，其中包含指定的訊息，「 這個類別已經過時;請改用類別 B。 」</span><span class="sxs-lookup"><span data-stu-id="e5d45-323">Each use of `A` in `Main` results in a warning that includes the specified message, "This class is obsolete; use class B instead."</span></span>

### <a name="caller-info-attributes"></a><span data-ttu-id="e5d45-324">呼叫端資訊屬性</span><span class="sxs-lookup"><span data-stu-id="e5d45-324">Caller info attributes</span></span>

<span data-ttu-id="e5d45-325">以供記錄和報告，它有時候是很有用的函式成員，才能取得特定呼叫程式碼的編譯時間資訊。</span><span class="sxs-lookup"><span data-stu-id="e5d45-325">For purposes such as logging and reporting, it is sometimes useful for a function member to obtain certain compile-time information about the calling code.</span></span> <span data-ttu-id="e5d45-326">呼叫端資訊屬性可用來以透明的方式傳遞這類資訊。</span><span class="sxs-lookup"><span data-stu-id="e5d45-326">The caller info attributes provide a way to pass such information transparently.</span></span>

<span data-ttu-id="e5d45-327">當呼叫端資訊屬性的其中一個註解的選擇性參數時，則省略對應的引數的呼叫中，不會不一定會造成用來取代預設參數值。</span><span class="sxs-lookup"><span data-stu-id="e5d45-327">When an optional parameter is annotated with one of the caller info attributes, omitting the corresponding argument in a call does not necessarily cause the default parameter value to be substituted.</span></span> <span data-ttu-id="e5d45-328">相反地，如果指定的資訊呼叫內容的相關功能，該資訊會傳遞做為引數的值。</span><span class="sxs-lookup"><span data-stu-id="e5d45-328">Instead, if the specified information about the calling context is available, that information will be passed as the argument value.</span></span>

<span data-ttu-id="e5d45-329">例如: </span><span class="sxs-lookup"><span data-stu-id="e5d45-329">For example:</span></span>

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

<span data-ttu-id="e5d45-330">呼叫`Log()`不使用引數可能會列印呼叫，行號和檔案路徑，以及呼叫發生在其中成員的名稱。</span><span class="sxs-lookup"><span data-stu-id="e5d45-330">A call to `Log()` with no arguments would print the line number and file path of the call, as well as the name of the member within which the call occurred.</span></span>

<span data-ttu-id="e5d45-331">呼叫端資訊屬性可能會發生在任何一處，選擇性的參數包括委派宣告中。</span><span class="sxs-lookup"><span data-stu-id="e5d45-331">Caller info attributes can occur on optional parameters anywhere, including in delegate declarations.</span></span> <span data-ttu-id="e5d45-332">不過，特定的呼叫端資訊屬性有限制他們可以屬性，參數型別上，一律會從替代值的隱含轉換成參數類型。</span><span class="sxs-lookup"><span data-stu-id="e5d45-332">However, the specific caller info attributes have restrictions on the types of the parameters they can attribute, so that there will always be an implicit conversion from a substituted value to the parameter type.</span></span>

<span data-ttu-id="e5d45-333">它是定義和實作部分方法宣告的一部分的參數上有相同的呼叫端資訊屬性的錯誤。</span><span class="sxs-lookup"><span data-stu-id="e5d45-333">It is an error to have the same caller info attribute on a parameter of both the defining and implementing part of a partial method declaration.</span></span> <span data-ttu-id="e5d45-334">套用只呼叫端資訊屬性中定義的一部分，而只有在實作的組件中發生的呼叫端資訊屬性會被忽略。</span><span class="sxs-lookup"><span data-stu-id="e5d45-334">Only caller info attributes in the defining part are applied, whereas caller info attributes occurring only in the implementing part are ignored.</span></span>

<span data-ttu-id="e5d45-335">呼叫端資訊不會影響多載解析。</span><span class="sxs-lookup"><span data-stu-id="e5d45-335">Caller information does not affect overload resolution.</span></span> <span data-ttu-id="e5d45-336">從原始程式檔的呼叫端仍省略屬性化的選擇性參數，因為多載解析會忽略那些參數相同的方式，它會忽略其他省略的選擇性參數 ([多載解析](expressions.md#overload-resolution))。</span><span class="sxs-lookup"><span data-stu-id="e5d45-336">As the attributed optional parameters are still omitted from the source code of the caller, overload resolution ignores those parameters in the same way it ignores other omitted optional parameters ([Overload resolution](expressions.md#overload-resolution)).</span></span>

<span data-ttu-id="e5d45-337">在原始程式碼中明確地叫用函式時，呼叫端資訊是只會用來替代。</span><span class="sxs-lookup"><span data-stu-id="e5d45-337">Caller information is only substituted when a function is explicitly invoked in source code.</span></span> <span data-ttu-id="e5d45-338">隱含的引動過程，例如隱含父建構函式呼叫沒有來源位置，而且不會取代呼叫端資訊。</span><span class="sxs-lookup"><span data-stu-id="e5d45-338">Implicit invocations such as implicit parent constructor calls do not have a source location and will not substitute caller information.</span></span> <span data-ttu-id="e5d45-339">此外，動態地繫結的呼叫將不會取代呼叫端資訊。</span><span class="sxs-lookup"><span data-stu-id="e5d45-339">Also, calls that are dynamically bound will not substitute caller information.</span></span> <span data-ttu-id="e5d45-340">如果呼叫端資訊，在此情況下省略屬性化的參數，參數指定的預設值使用。</span><span class="sxs-lookup"><span data-stu-id="e5d45-340">When a caller info attributed parameter is omitted in such cases, the specified default value of the parameter is used instead.</span></span>

<span data-ttu-id="e5d45-341">查詢運算式是一個例外。</span><span class="sxs-lookup"><span data-stu-id="e5d45-341">One exception is query-expressions.</span></span> <span data-ttu-id="e5d45-342">這些皆視為語法擴充，並呼叫它們會展開以略過與呼叫端資訊屬性的選擇性參數，如果呼叫端資訊會被取代。</span><span class="sxs-lookup"><span data-stu-id="e5d45-342">These are considered syntactic expansions, and if the calls they expand to omit optional parameters with caller info attributes, caller information will be substituted.</span></span> <span data-ttu-id="e5d45-343">使用的位置是查詢子句，這個子句會呼叫所產生的位置。</span><span class="sxs-lookup"><span data-stu-id="e5d45-343">The location used is the location of the query clause which the call was generated from.</span></span>

<span data-ttu-id="e5d45-344">如果指定的參數上指定一個以上的呼叫端資訊屬性，則不會依下列順序選擇較： `CallerLineNumber`， `CallerFilePath`， `CallerMemberName`。</span><span class="sxs-lookup"><span data-stu-id="e5d45-344">If more than one caller info attribute is specified on a given parameter, they are preferred in the following order: `CallerLineNumber`, `CallerFilePath`, `CallerMemberName`.</span></span>

#### <a name="the-callerlinenumber-attribute"></a><span data-ttu-id="e5d45-345">CallerLineNumber 屬性</span><span class="sxs-lookup"><span data-stu-id="e5d45-345">The CallerLineNumber attribute</span></span>

<span data-ttu-id="e5d45-346">`System.Runtime.CompilerServices.CallerLineNumberAttribute`標準的隱含轉換時，會將選擇性參數上允許 ([標準的隱含轉換](conversions.md#standard-implicit-conversions)) 的常數值從`int.MaxValue`參數的型別。</span><span class="sxs-lookup"><span data-stu-id="e5d45-346">The `System.Runtime.CompilerServices.CallerLineNumberAttribute` is allowed on optional parameters when there is a standard implicit conversion ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) from the constant value `int.MaxValue` to the parameter's type.</span></span> <span data-ttu-id="e5d45-347">這可確保不會發生錯誤，可以傳遞至該值的任何非負號。</span><span class="sxs-lookup"><span data-stu-id="e5d45-347">This ensures that any non-negative line number up to that value can be passed without error.</span></span>

<span data-ttu-id="e5d45-348">如果函式引動過程，從原始程式碼中的位置會省略選擇性參數`CallerLineNumberAttribute`，則表示該位置的行號的數值常值做為取代預設參數值的引動過程的引數。</span><span class="sxs-lookup"><span data-stu-id="e5d45-348">If a function invocation from a location in source code omits an optional parameter with the `CallerLineNumberAttribute`, then a numeric literal representing that location's line number is used as an argument to the invocation instead of the default parameter value.</span></span>

<span data-ttu-id="e5d45-349">如果引動過程會跨越多行，選擇線條是視實作而定。</span><span class="sxs-lookup"><span data-stu-id="e5d45-349">If the invocation spans multiple lines, the line chosen is implementation-dependent.</span></span>

<span data-ttu-id="e5d45-350">請注意，行號可能會受到`#line`指示詞 ([行指示詞](lexical-structure.md#line-directives))。</span><span class="sxs-lookup"><span data-stu-id="e5d45-350">Note that the line number may be affected by `#line` directives ([Line directives](lexical-structure.md#line-directives)).</span></span>

#### <a name="the-callerfilepath-attribute"></a><span data-ttu-id="e5d45-351">CallerFilePath 屬性</span><span class="sxs-lookup"><span data-stu-id="e5d45-351">The CallerFilePath attribute</span></span>

<span data-ttu-id="e5d45-352">`System.Runtime.CompilerServices.CallerFilePathAttribute`標準的隱含轉換時，會將選擇性參數上允許 ([標準的隱含轉換](conversions.md#standard-implicit-conversions)) 從`string`參數的型別。</span><span class="sxs-lookup"><span data-stu-id="e5d45-352">The `System.Runtime.CompilerServices.CallerFilePathAttribute` is allowed on optional parameters when there is a standard implicit conversion ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) from `string` to the parameter's type.</span></span>

<span data-ttu-id="e5d45-353">如果函式引動過程，從原始程式碼中的位置會省略選擇性參數`CallerFilePathAttribute`，則表示該位置的檔案路徑的字串常值做為取代預設參數值的引動過程的引數。</span><span class="sxs-lookup"><span data-stu-id="e5d45-353">If a function invocation from a location in source code omits an optional parameter with the `CallerFilePathAttribute`, then a string literal representing that location's file path is used as an argument to the invocation instead of the default parameter value.</span></span>

<span data-ttu-id="e5d45-354">檔案路徑的格式是視實作而定。</span><span class="sxs-lookup"><span data-stu-id="e5d45-354">The format of the file path is implementation-dependent.</span></span>

<span data-ttu-id="e5d45-355">請注意，檔案路徑可能會受到`#line`指示詞 ([行指示詞](lexical-structure.md#line-directives))。</span><span class="sxs-lookup"><span data-stu-id="e5d45-355">Note that the file path may be affected by `#line` directives ([Line directives](lexical-structure.md#line-directives)).</span></span>

#### <a name="the-callermembername-attribute"></a><span data-ttu-id="e5d45-356">CallerMemberName 屬性</span><span class="sxs-lookup"><span data-stu-id="e5d45-356">The CallerMemberName attribute</span></span>

<span data-ttu-id="e5d45-357">`System.Runtime.CompilerServices.CallerMemberNameAttribute`標準的隱含轉換時，會將選擇性參數上允許 ([標準的隱含轉換](conversions.md#standard-implicit-conversions)) 從`string`參數的型別。</span><span class="sxs-lookup"><span data-stu-id="e5d45-357">The `System.Runtime.CompilerServices.CallerMemberNameAttribute` is allowed on optional parameters when there is a standard implicit conversion ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) from `string` to the parameter's type.</span></span>

<span data-ttu-id="e5d45-358">如果函式成員的主體內，或在屬性內的位置從函式引動過程會套用至函式的成員，本身或其傳回型別、 參數或型別參數中的原始程式碼會省略選擇性參數`CallerMemberNameAttribute`，則會顯示字串常值代表該成員的名稱做為取代預設參數值的引動過程的引數。</span><span class="sxs-lookup"><span data-stu-id="e5d45-358">If a function invocation from a location within the body of a function member or within an attribute applied to the function member itself or its return type, parameters or type parameters in source code omits an optional parameter with the `CallerMemberNameAttribute`, then a string literal representing the name of that member is used as an argument to the invocation instead of the default parameter value.</span></span>

<span data-ttu-id="e5d45-359">引動過程發生泛型方法內，會使用僅方法名稱本身，沒有類型參數清單。</span><span class="sxs-lookup"><span data-stu-id="e5d45-359">For invocations that occur within generic methods, only the method name itself is used, without the type parameter list.</span></span>

<span data-ttu-id="e5d45-360">如果引動過程發生在明確介面成員實作，會使用僅方法名稱本身，而不需要上述介面限定性條件。</span><span class="sxs-lookup"><span data-stu-id="e5d45-360">For invocations that occur within explicit interface member implementations, only the method name itself is used, without the preceding interface qualification.</span></span>

<span data-ttu-id="e5d45-361">引動過程屬性或事件存取子中發生，所使用的成員名稱是屬性或事件本身。</span><span class="sxs-lookup"><span data-stu-id="e5d45-361">For invocations that occur within property or event accessors, the member name used is that of the property or event itself.</span></span>

<span data-ttu-id="e5d45-362">如果引動過程發生在索引子存取子，使用的成員名稱是所提供的`IndexerNameAttribute`([IndexerName 屬性](attributes.md#the-indexername-attribute)) 上的索引子的成員，如果有的話或將預設名稱`Item`否則。</span><span class="sxs-lookup"><span data-stu-id="e5d45-362">For invocations that occur within indexer accessors, the member name used is that supplied by an `IndexerNameAttribute` ([The IndexerName attribute](attributes.md#the-indexername-attribute)) on the indexer member, if present, or the default name `Item` otherwise.</span></span>

<span data-ttu-id="e5d45-363">執行個體建構函式、 靜態建構函式、 解構函式和運算子宣告中發生之成員的引動過程使用的名稱會是視實作而定。</span><span class="sxs-lookup"><span data-stu-id="e5d45-363">For invocations that occur within declarations of instance constructors, static constructors, destructors and operators the member name used is implementation-dependent.</span></span>

## <a name="attributes-for-interoperation"></a><span data-ttu-id="e5d45-364">交互操作的屬性</span><span class="sxs-lookup"><span data-stu-id="e5d45-364">Attributes for Interoperation</span></span>

<span data-ttu-id="e5d45-365">注意:本節是僅適用於 Microsoft.NET 實作的C#。</span><span class="sxs-lookup"><span data-stu-id="e5d45-365">Note: This section is applicable only to the Microsoft .NET implementation of C#.</span></span>

### <a name="interoperation-with-com-and-win32-components"></a><span data-ttu-id="e5d45-366">與 COM 和 Win32 元件的互通性</span><span class="sxs-lookup"><span data-stu-id="e5d45-366">Interoperation with COM and Win32 components</span></span>

<span data-ttu-id="e5d45-367">.NET 執行階段會提供大量的屬性，可讓 C# 程式使用 COM 與 Win32 Dll 所撰寫的元件相互操作。</span><span class="sxs-lookup"><span data-stu-id="e5d45-367">The .NET run-time provides a large number of attributes that enable C# programs to interoperate with components written using COM and Win32 DLLs.</span></span> <span data-ttu-id="e5d45-368">例如，`DllImport`屬性可以用於`static extern`方法，以表示方法的實作是在 Win32 DLL 中找到。</span><span class="sxs-lookup"><span data-stu-id="e5d45-368">For example, the `DllImport` attribute can be used on a `static extern` method to indicate that the implementation of the method is to be found in a Win32 DLL.</span></span> <span data-ttu-id="e5d45-369">這些屬性位於`System.Runtime.InteropServices`命名空間，而這些屬性的詳細文件位於.NET 執行階段文件。</span><span class="sxs-lookup"><span data-stu-id="e5d45-369">These attributes are found in the `System.Runtime.InteropServices` namespace, and detailed documentation for these attributes is found in the .NET runtime documentation.</span></span>

### <a name="interoperation-with-other-net-languages"></a><span data-ttu-id="e5d45-370">與其他.NET 語言的互通性</span><span class="sxs-lookup"><span data-stu-id="e5d45-370">Interoperation with other .NET languages</span></span>

#### <a name="the-indexername-attribute"></a><span data-ttu-id="e5d45-371">IndexerName 屬性。</span><span class="sxs-lookup"><span data-stu-id="e5d45-371">The IndexerName attribute</span></span>

<span data-ttu-id="e5d45-372">索引子會實作在.NET 中使用索引的屬性，而且.NET 中繼資料中有一個名稱。</span><span class="sxs-lookup"><span data-stu-id="e5d45-372">Indexers are implemented in .NET using indexed properties, and have a name in the .NET metadata.</span></span> <span data-ttu-id="e5d45-373">如果沒有`IndexerName`屬性，則索引子，則名稱`Item`預設會使用。</span><span class="sxs-lookup"><span data-stu-id="e5d45-373">If no `IndexerName` attribute is present for an indexer, then the name `Item` is used by default.</span></span> <span data-ttu-id="e5d45-374">`IndexerName`屬性可讓開發人員覆寫此預設值，並指定不同的名稱。</span><span class="sxs-lookup"><span data-stu-id="e5d45-374">The `IndexerName` attribute enables a developer to override this default and specify a different name.</span></span>

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
