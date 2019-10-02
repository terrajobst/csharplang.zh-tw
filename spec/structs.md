---
ms.openlocfilehash: 6dd1dde67597b2125de9a1aa2fab9144128d533f
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704034"
---
# <a name="structs"></a><span data-ttu-id="69ad3-101">結構</span><span class="sxs-lookup"><span data-stu-id="69ad3-101">Structs</span></span>

<span data-ttu-id="69ad3-102">結構類似于類別，它們代表可包含資料成員和函式成員的資料結構。</span><span class="sxs-lookup"><span data-stu-id="69ad3-102">Structs are similar to classes in that they represent data structures that can contain data members and function members.</span></span> <span data-ttu-id="69ad3-103">不過，不同于類別，結構是實數值型別，不需要堆積配置。</span><span class="sxs-lookup"><span data-stu-id="69ad3-103">However, unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="69ad3-104">結構型別的變數直接包含結構的資料，而類別型別的變數則包含資料的參考，後者稱為物件。</span><span class="sxs-lookup"><span data-stu-id="69ad3-104">A variable of a struct type directly contains the data of the struct, whereas a variable of a class type contains a reference to the data, the latter known as an object.</span></span>

<span data-ttu-id="69ad3-105">結構特別適用於含有實值語意的小型資料結構。</span><span class="sxs-lookup"><span data-stu-id="69ad3-105">Structs are particularly useful for small data structures that have value semantics.</span></span> <span data-ttu-id="69ad3-106">複數、座標系統中的點或字典中的索引鍵/值組都是結構的良好範例。</span><span class="sxs-lookup"><span data-stu-id="69ad3-106">Complex numbers, points in a coordinate system, or key-value pairs in a dictionary are all good examples of structs.</span></span> <span data-ttu-id="69ad3-107">這些資料結構的關鍵在於它們有幾個資料成員，不需要使用繼承或參考身分識別，而且可以使用值語義輕鬆地實作為，其中指派會複製值，而不是參考。</span><span class="sxs-lookup"><span data-stu-id="69ad3-107">Key to these data structures is that they have few data members, that they do not require use of inheritance or referential identity, and that they can be conveniently implemented using value semantics where assignment copies the value instead of the reference.</span></span>

<span data-ttu-id="69ad3-108">如[簡單](types.md#simple-types)型別中所述，所提供的C#簡單類型（例如 `int`、`double` 和 `bool`）實際上是所有結構類型。</span><span class="sxs-lookup"><span data-stu-id="69ad3-108">As described in [Simple types](types.md#simple-types), the simple types provided by C#, such as `int`, `double`, and `bool`, are in fact all struct types.</span></span> <span data-ttu-id="69ad3-109">就像這些預先定義的類型是結構一樣，也可以使用結構和運算子多載，以語言來執行新的C# 「基本」類型。</span><span class="sxs-lookup"><span data-stu-id="69ad3-109">Just as these predefined types are structs, it is also possible to use structs and operator overloading to implement new "primitive" types in the C# language.</span></span> <span data-ttu-id="69ad3-110">這類類型的兩個範例會在本章結尾處提供（[結構範例](structs.md#struct-examples)）。</span><span class="sxs-lookup"><span data-stu-id="69ad3-110">Two examples of such types are given at the end of this chapter ([Struct examples](structs.md#struct-examples)).</span></span>

## <a name="struct-declarations"></a><span data-ttu-id="69ad3-111">結構宣告</span><span class="sxs-lookup"><span data-stu-id="69ad3-111">Struct declarations</span></span>

<span data-ttu-id="69ad3-112">*Struct_declaration*是宣告新結構的*type_declaration* （[型](namespaces.md#type-declarations)別宣告）：</span><span class="sxs-lookup"><span data-stu-id="69ad3-112">A *struct_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new struct:</span></span>

```antlr
struct_declaration
    : attributes? struct_modifier* 'partial'? 'struct' identifier type_parameter_list?
      struct_interfaces? type_parameter_constraints_clause* struct_body ';'?
    ;
```

<span data-ttu-id="69ad3-113">*Struct_declaration*是由一組選擇性的*屬性*（[屬性](attributes.md)）所組成，後面接著一組選擇性的*struct_modifier*s （[結構](structs.md#struct-modifiers)修飾詞），後面接著選擇性的 `partial` 修飾詞，後面接著關鍵字 `struct` 和命名結構的*識別碼*，後面接著選擇性的*Type_parameter_list*規格（[類型參數](classes.md#type-parameters)），後面接著選擇性的*struct_interfaces*規格（[部分修飾](structs.md#partial-modifier)詞），後面接著選擇性的*type_parameter_constraints_clause*s 規格（[類型參數條件約束](classes.md#type-parameter-constraints)），後面接著*struct_body* （[結構主體](structs.md#struct-body)），並選擇性地加上分號。</span><span class="sxs-lookup"><span data-stu-id="69ad3-113">A *struct_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *struct_modifier*s ([Struct modifiers](structs.md#struct-modifiers)), followed by an optional `partial` modifier, followed by the keyword `struct` and an *identifier* that names the struct, followed by an optional *type_parameter_list* specification ([Type parameters](classes.md#type-parameters)), followed by an optional *struct_interfaces* specification ([Partial modifier](structs.md#partial-modifier)) ), followed by an optional *type_parameter_constraints_clause*s specification ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by a *struct_body* ([Struct body](structs.md#struct-body)), optionally followed by a semicolon.</span></span>

### <a name="struct-modifiers"></a><span data-ttu-id="69ad3-114">結構修飾詞</span><span class="sxs-lookup"><span data-stu-id="69ad3-114">Struct modifiers</span></span>

<span data-ttu-id="69ad3-115">*Struct_declaration*可以選擇性地包含一連串的結構修飾詞：</span><span class="sxs-lookup"><span data-stu-id="69ad3-115">A *struct_declaration* may optionally include a sequence of struct modifiers:</span></span>

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

<span data-ttu-id="69ad3-116">在結構宣告中多次出現相同的修飾詞時，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="69ad3-116">It is a compile-time error for the same modifier to appear multiple times in a struct declaration.</span></span>

<span data-ttu-id="69ad3-117">結構宣告的修飾詞與類別宣告的修飾詞（[類別](classes.md#class-declarations)宣告）具有相同的意義。</span><span class="sxs-lookup"><span data-stu-id="69ad3-117">The modifiers of a struct declaration have the same meaning as those of a class declaration ([Class declarations](classes.md#class-declarations)).</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="69ad3-118">Partial 修飾詞</span><span class="sxs-lookup"><span data-stu-id="69ad3-118">Partial modifier</span></span>

<span data-ttu-id="69ad3-119">@No__t-0 修飾詞表示這個*struct_declaration*是部分類型宣告。</span><span class="sxs-lookup"><span data-stu-id="69ad3-119">The `partial` modifier indicates that this *struct_declaration* is a partial type declaration.</span></span> <span data-ttu-id="69ad3-120">在封入命名空間或類型宣告中，具有相同名稱的多個部分結構宣告會結合成一個結構宣告，遵循[部分類型](classes.md#partial-types)中指定的規則。</span><span class="sxs-lookup"><span data-stu-id="69ad3-120">Multiple partial struct declarations with the same name within an enclosing namespace or type declaration combine to form one struct declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

### <a name="struct-interfaces"></a><span data-ttu-id="69ad3-121">結構介面</span><span class="sxs-lookup"><span data-stu-id="69ad3-121">Struct interfaces</span></span>

<span data-ttu-id="69ad3-122">結構宣告可能包括*struct_interfaces*規格，在此情況下，會將結構視為直接實作為指定的介面類別型。</span><span class="sxs-lookup"><span data-stu-id="69ad3-122">A struct declaration may include a *struct_interfaces* specification, in which case the struct is said to directly implement the given interface types.</span></span>

```antlr
struct_interfaces
    : ':' interface_type_list
    ;
```

<span data-ttu-id="69ad3-123">介面的執行會在[介面實現](interfaces.md#interface-implementations)中進一步討論。</span><span class="sxs-lookup"><span data-stu-id="69ad3-123">Interface implementations are discussed further in [Interface implementations](interfaces.md#interface-implementations).</span></span>

### <a name="struct-body"></a><span data-ttu-id="69ad3-124">結構主體</span><span class="sxs-lookup"><span data-stu-id="69ad3-124">Struct body</span></span>

<span data-ttu-id="69ad3-125">結構的*struct_body*會定義結構的成員。</span><span class="sxs-lookup"><span data-stu-id="69ad3-125">The *struct_body* of a struct defines the members of the struct.</span></span>

```antlr
struct_body
    : '{' struct_member_declaration* '}'
    ;
```

## <a name="struct-members"></a><span data-ttu-id="69ad3-126">結構成員</span><span class="sxs-lookup"><span data-stu-id="69ad3-126">Struct members</span></span>

<span data-ttu-id="69ad3-127">結構的成員是由其*struct_member_declaration*所引進的成員，以及繼承自類型 `System.ValueType` 的成員所組成。</span><span class="sxs-lookup"><span data-stu-id="69ad3-127">The members of a struct consist of the members introduced by its *struct_member_declaration*s and the members inherited from the type `System.ValueType`.</span></span>

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

<span data-ttu-id="69ad3-128">除了[類別和結構差異](structs.md#class-and-struct-differences)中所述的差異之外，[類別成員](classes.md#class-members)透過[反覆運算](classes.md#iterators)器所提供的類別成員描述也適用于結構成員。</span><span class="sxs-lookup"><span data-stu-id="69ad3-128">Except for the differences noted in [Class and struct differences](structs.md#class-and-struct-differences), the descriptions of class members provided in [Class members](classes.md#class-members) through [Iterators](classes.md#iterators) apply to struct members as well.</span></span>

## <a name="class-and-struct-differences"></a><span data-ttu-id="69ad3-129">類別和結構的差異</span><span class="sxs-lookup"><span data-stu-id="69ad3-129">Class and struct differences</span></span>

<span data-ttu-id="69ad3-130">結構與類別有幾個重要的差異：</span><span class="sxs-lookup"><span data-stu-id="69ad3-130">Structs differ from classes in several important ways:</span></span>

*  <span data-ttu-id="69ad3-131">結構是實數值型別（[值語義](structs.md#value-semantics)）。</span><span class="sxs-lookup"><span data-stu-id="69ad3-131">Structs are value types ([Value semantics](structs.md#value-semantics)).</span></span>
*  <span data-ttu-id="69ad3-132">所有結構類型都會隱含繼承自類別 `System.ValueType` （[繼承](structs.md#inheritance)）。</span><span class="sxs-lookup"><span data-stu-id="69ad3-132">All struct types implicitly inherit from the class `System.ValueType` ([Inheritance](structs.md#inheritance)).</span></span>
*  <span data-ttu-id="69ad3-133">指派至結構類型的變數會建立所指派值的複本（[指派](structs.md#assignment)）。</span><span class="sxs-lookup"><span data-stu-id="69ad3-133">Assignment to a variable of a struct type creates a copy of the value being assigned ([Assignment](structs.md#assignment)).</span></span>
*  <span data-ttu-id="69ad3-134">結構的預設值是將所有實值型別字段設定為預設值，並將所有參考型別字段設為 `null` （[預設值](structs.md#default-values)）所產生的值。</span><span class="sxs-lookup"><span data-stu-id="69ad3-134">The default value of a struct is the value produced by setting all value type fields to their default value and all reference type fields to `null` ([Default values](structs.md#default-values)).</span></span>
*  <span data-ttu-id="69ad3-135">使用裝箱和取消裝箱作業，在結構類型和 `object` （[裝箱和取消裝箱](structs.md#boxing-and-unboxing)）之間進行轉換。</span><span class="sxs-lookup"><span data-stu-id="69ad3-135">Boxing and unboxing operations are used to convert between a struct type and `object` ([Boxing and unboxing](structs.md#boxing-and-unboxing)).</span></span>
*  <span data-ttu-id="69ad3-136">結構（[此存取](expressions.md#this-access)）的 `this` 的意義不同。</span><span class="sxs-lookup"><span data-stu-id="69ad3-136">The meaning of `this` is different for structs ([This access](expressions.md#this-access)).</span></span>
*  <span data-ttu-id="69ad3-137">不允許結構的實例欄位宣告包含變數初始化運算式（[欄位初始化運算式](structs.md#field-initializers)）。</span><span class="sxs-lookup"><span data-stu-id="69ad3-137">Instance field declarations for a struct are not permitted to include variable initializers ([Field initializers](structs.md#field-initializers)).</span></span>
*  <span data-ttu-id="69ad3-138">不允許結構宣告無參數實例的函式（[構造](structs.md#constructors)函式）。</span><span class="sxs-lookup"><span data-stu-id="69ad3-138">A struct is not permitted to declare a parameterless instance constructor ([Constructors](structs.md#constructors)).</span></span>
*  <span data-ttu-id="69ad3-139">不允許結構宣告析構函數（[析構](structs.md#destructors)函式）。</span><span class="sxs-lookup"><span data-stu-id="69ad3-139">A struct is not permitted to declare a destructor ([Destructors](structs.md#destructors)).</span></span>

### <a name="value-semantics"></a><span data-ttu-id="69ad3-140">值的語義</span><span class="sxs-lookup"><span data-stu-id="69ad3-140">Value semantics</span></span>

<span data-ttu-id="69ad3-141">結構是實值型別（實[值](types.md#value-types)型別），也稱為具有值的語義。</span><span class="sxs-lookup"><span data-stu-id="69ad3-141">Structs are value types ([Value types](types.md#value-types)) and are said to have value semantics.</span></span> <span data-ttu-id="69ad3-142">另一方面，類別是參考型別（[參考](types.md#reference-types)型別），也稱為具有參考語義。</span><span class="sxs-lookup"><span data-stu-id="69ad3-142">Classes, on the other hand, are reference types ([Reference types](types.md#reference-types)) and are said to have reference semantics.</span></span>

<span data-ttu-id="69ad3-143">結構型別的變數直接包含結構的資料，而類別型別的變數則包含資料的參考，後者稱為物件。</span><span class="sxs-lookup"><span data-stu-id="69ad3-143">A variable of a struct type directly contains the data of the struct, whereas a variable of a class type contains a reference to the data, the latter known as an object.</span></span> <span data-ttu-id="69ad3-144">當結構 `B` 包含型別 `A` 的實例欄位，而 `A` 是結構型別時，就會發生編譯時期錯誤，`A` 會相依于 `B` 或從 `B` 所結構化的型別。</span><span class="sxs-lookup"><span data-stu-id="69ad3-144">When a struct `B` contains an instance field of type `A` and `A` is a struct type, it is a compile-time error for `A` to depend on `B` or a type constructed from `B`.</span></span> <span data-ttu-id="69ad3-145">如果 `X` 包含 `Y` 類型的實例欄位，則結構 `X` 會***直接相依于***結構 `Y`。</span><span class="sxs-lookup"><span data-stu-id="69ad3-145">A struct `X` ***directly depends on*** a struct `Y` if `X` contains an instance field of type `Y`.</span></span> <span data-ttu-id="69ad3-146">根據這個定義，結構所相依的完整結構集是***直接相依于***關聯性的可轉移關閉。</span><span class="sxs-lookup"><span data-stu-id="69ad3-146">Given this definition, the complete set of structs upon which a struct depends is the transitive closure of the ***directly depends on*** relationship.</span></span>  <span data-ttu-id="69ad3-147">例如：</span><span class="sxs-lookup"><span data-stu-id="69ad3-147">For example</span></span>
```csharp
struct Node
{
    int data;
    Node next; // error, Node directly depends on itself
}
```
<span data-ttu-id="69ad3-148">為錯誤，因為 `Node` 包含其本身類型的實例欄位。</span><span class="sxs-lookup"><span data-stu-id="69ad3-148">is an error because `Node` contains an instance field of its own type.</span></span>  <span data-ttu-id="69ad3-149">另一個範例</span><span class="sxs-lookup"><span data-stu-id="69ad3-149">Another example</span></span>
```csharp
struct A { B b; }

struct B { C c; }

struct C { A a; }
```
<span data-ttu-id="69ad3-150">是錯誤，因為每個類型 `A`、`B` 和 `C` 相依于彼此。</span><span class="sxs-lookup"><span data-stu-id="69ad3-150">is an error because each of the types `A`, `B`, and `C` depend on each other.</span></span>

<span data-ttu-id="69ad3-151">使用類別時，兩個變數可能會參考相同的物件，因此，對某個變數的作業可能會影響另一個變數所參考的物件。</span><span class="sxs-lookup"><span data-stu-id="69ad3-151">With classes, it is possible for two variables to reference the same object, and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="69ad3-152">使用結構時，每個變數都有自己的資料複本（除了 `ref` 和 @no__t 1 參數變數以外），而且不可能對其中一個作業影響另一個。</span><span class="sxs-lookup"><span data-stu-id="69ad3-152">With structs, the variables each have their own copy of the data (except in the case of `ref` and `out` parameter variables), and it is not possible for operations on one to affect the other.</span></span> <span data-ttu-id="69ad3-153">此外，因為結構不是參考型別，所以結構類型的值不可能 `null`。</span><span class="sxs-lookup"><span data-stu-id="69ad3-153">Furthermore, because structs are not reference types, it is not possible for values of a struct type to be `null`.</span></span>

<span data-ttu-id="69ad3-154">指定宣告</span><span class="sxs-lookup"><span data-stu-id="69ad3-154">Given the declaration</span></span>
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
<span data-ttu-id="69ad3-155">程式碼片段</span><span class="sxs-lookup"><span data-stu-id="69ad3-155">the code fragment</span></span>
```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 100;
System.Console.WriteLine(b.x);
```
<span data-ttu-id="69ad3-156">輸出 `10` 的值。</span><span class="sxs-lookup"><span data-stu-id="69ad3-156">outputs the value `10`.</span></span> <span data-ttu-id="69ad3-157">指派 `a` 給 `b` 會建立值的複本，而 `b` 則不會受到指派至 `a.x` 的影響。</span><span class="sxs-lookup"><span data-stu-id="69ad3-157">The assignment of `a` to `b` creates a copy of the value, and `b` is thus unaffected by the assignment to `a.x`.</span></span> <span data-ttu-id="69ad3-158">已將 `Point` 改宣告為類別，則輸出會 `100`，因為 `a` 和 `b` 會參考相同的物件。</span><span class="sxs-lookup"><span data-stu-id="69ad3-158">Had `Point` instead been declared as a class, the output would be `100` because `a` and `b` would reference the same object.</span></span>

### <a name="inheritance"></a><span data-ttu-id="69ad3-159">繼承</span><span class="sxs-lookup"><span data-stu-id="69ad3-159">Inheritance</span></span>

<span data-ttu-id="69ad3-160">所有結構類型都會隱含繼承自類別 `System.ValueType`，而這又會繼承自類別 `object`。</span><span class="sxs-lookup"><span data-stu-id="69ad3-160">All struct types implicitly inherit from the class `System.ValueType`, which, in turn, inherits from class `object`.</span></span> <span data-ttu-id="69ad3-161">結構宣告可以指定實作為實介面的清單，但結構宣告不可能指定基類。</span><span class="sxs-lookup"><span data-stu-id="69ad3-161">A struct declaration may specify a list of implemented interfaces, but it is not possible for a struct declaration to specify a base class.</span></span>

<span data-ttu-id="69ad3-162">結構類型絕對不會是抽象的，而且一律會隱含地密封。</span><span class="sxs-lookup"><span data-stu-id="69ad3-162">Struct types are never abstract and are always implicitly sealed.</span></span> <span data-ttu-id="69ad3-163">因此，在結構宣告中不允許 `abstract` 和 @no__t 1 修飾詞。</span><span class="sxs-lookup"><span data-stu-id="69ad3-163">The `abstract` and `sealed` modifiers are therefore not permitted in a struct declaration.</span></span>

<span data-ttu-id="69ad3-164">因為結構不支援繼承，所以結構成員的宣告存取範圍不能 `protected` 或 `protected internal`。</span><span class="sxs-lookup"><span data-stu-id="69ad3-164">Since inheritance isn't supported for structs, the declared accessibility of a struct member cannot be `protected` or `protected internal`.</span></span>

<span data-ttu-id="69ad3-165">結構中的函式成員不能 `abstract` 或 `virtual`，而且只允許 `override` 修飾詞覆寫繼承自 `System.ValueType` 的方法。</span><span class="sxs-lookup"><span data-stu-id="69ad3-165">Function members in a struct cannot be `abstract` or `virtual`, and the `override` modifier is allowed only to override methods inherited from `System.ValueType`.</span></span>

### <a name="assignment"></a><span data-ttu-id="69ad3-166">指派</span><span class="sxs-lookup"><span data-stu-id="69ad3-166">Assignment</span></span>

<span data-ttu-id="69ad3-167">指派至結構類型的變數會建立所指派值的複本。</span><span class="sxs-lookup"><span data-stu-id="69ad3-167">Assignment to a variable of a struct type creates a copy of the value being assigned.</span></span> <span data-ttu-id="69ad3-168">這與指派給類別類型的變數不同，後者會複製參考，而不是參考所識別的物件。</span><span class="sxs-lookup"><span data-stu-id="69ad3-168">This differs from assignment to a variable of a class type, which copies the reference but not the object identified by the reference.</span></span>

<span data-ttu-id="69ad3-169">類似于指派，當結構當做值參數傳遞或當做函數成員的結果傳回時，會建立結構的複本。</span><span class="sxs-lookup"><span data-stu-id="69ad3-169">Similar to an assignment, when a struct is passed as a value parameter or returned as the result of a function member, a copy of the struct is created.</span></span> <span data-ttu-id="69ad3-170">您可以使用 `ref` 或 `out` 參數，以傳址方式將結構傳遞給函式成員。</span><span class="sxs-lookup"><span data-stu-id="69ad3-170">A struct may be passed by reference to a function member using a `ref` or `out` parameter.</span></span>

<span data-ttu-id="69ad3-171">當結構的屬性或索引子是指派的目標時，與屬性或索引子存取相關聯的實例運算式必須分類為變數。</span><span class="sxs-lookup"><span data-stu-id="69ad3-171">When a property or indexer of a struct is the target of an assignment, the instance expression associated with the property or indexer access must be classified as a variable.</span></span> <span data-ttu-id="69ad3-172">如果實例運算式分類為值，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="69ad3-172">If the instance expression is classified as a value, a compile-time error occurs.</span></span> <span data-ttu-id="69ad3-173">這在[簡單指派](expressions.md#simple-assignment)中會進一步詳細說明。</span><span class="sxs-lookup"><span data-stu-id="69ad3-173">This is described in further detail in [Simple assignment](expressions.md#simple-assignment).</span></span>

### <a name="default-values"></a><span data-ttu-id="69ad3-174">預設值</span><span class="sxs-lookup"><span data-stu-id="69ad3-174">Default values</span></span>

<span data-ttu-id="69ad3-175">如[預設值](variables.md#default-values)所述，在建立時，會將數種變數自動初始化為其預設值。</span><span class="sxs-lookup"><span data-stu-id="69ad3-175">As described in [Default values](variables.md#default-values), several kinds of variables are automatically initialized to their default value when they are created.</span></span> <span data-ttu-id="69ad3-176">對於類別類型和其他參考型別的變數，此預設值為 `null`。</span><span class="sxs-lookup"><span data-stu-id="69ad3-176">For variables of class types and other reference types, this default value is `null`.</span></span> <span data-ttu-id="69ad3-177">不過，因為結構是無法 `null` 的實值型別，所以結構的預設值就是將所有實值型別字段設定為預設值，並將所有參考型別字段設為 `null` 所產生的值。</span><span class="sxs-lookup"><span data-stu-id="69ad3-177">However, since structs are value types that cannot be `null`, the default value of a struct is the value produced by setting all value type fields to their default value and all reference type fields to `null`.</span></span>

<span data-ttu-id="69ad3-178">參考上面所宣告的 `Point` 結構，範例</span><span class="sxs-lookup"><span data-stu-id="69ad3-178">Referring to the `Point` struct declared above, the example</span></span>
```csharp
Point[] a = new Point[100];
```
<span data-ttu-id="69ad3-179">將陣列中的每個 `Point` 初始化為將 `x` 和 @no__t 2 欄位設定為零所產生的值。</span><span class="sxs-lookup"><span data-stu-id="69ad3-179">initializes each `Point` in the array to the value produced by setting the `x` and `y` fields to zero.</span></span>

<span data-ttu-id="69ad3-180">結構的預設值會對應至結構的預設函式所傳回的值（[預設](types.md#default-constructors)的函式）。</span><span class="sxs-lookup"><span data-stu-id="69ad3-180">The default value of a struct corresponds to the value returned by the default constructor of the struct ([Default constructors](types.md#default-constructors)).</span></span> <span data-ttu-id="69ad3-181">不同于類別，結構不允許宣告無參數的實例的函式。</span><span class="sxs-lookup"><span data-stu-id="69ad3-181">Unlike a class, a struct is not permitted to declare a parameterless instance constructor.</span></span> <span data-ttu-id="69ad3-182">相反地，每個結構隱含具有無參數的實例函式，其一律會傳回將所有實值型別字段設定為預設值，以及所有參考型別字段 `null` 所產生的值。</span><span class="sxs-lookup"><span data-stu-id="69ad3-182">Instead, every struct implicitly has a parameterless instance constructor which always returns the value that results from setting all value type fields to their default value and all reference type fields to `null`.</span></span>

<span data-ttu-id="69ad3-183">結構應該設計成將預設的初始化狀態視為有效的狀態。</span><span class="sxs-lookup"><span data-stu-id="69ad3-183">Structs should be designed to consider the default initialization state a valid state.</span></span> <span data-ttu-id="69ad3-184">在範例中</span><span class="sxs-lookup"><span data-stu-id="69ad3-184">In the example</span></span>
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
<span data-ttu-id="69ad3-185">使用者定義的實例的函式只會在明確呼叫它時，保護其是否為 null 值。</span><span class="sxs-lookup"><span data-stu-id="69ad3-185">the user-defined instance constructor protects against null values only where it is explicitly called.</span></span> <span data-ttu-id="69ad3-186">如果 @no__t 0 變數受限於預設值初始化，則 `key` 和 @no__t 2 欄位將會是 null，而且必須準備結構來處理此狀態。</span><span class="sxs-lookup"><span data-stu-id="69ad3-186">In cases where a `KeyValuePair` variable is subject to default value initialization, the `key` and `value` fields will be null, and the struct must be prepared to handle this state.</span></span>

### <a name="boxing-and-unboxing"></a><span data-ttu-id="69ad3-187">Boxing 和 Unboxing</span><span class="sxs-lookup"><span data-stu-id="69ad3-187">Boxing and unboxing</span></span>

<span data-ttu-id="69ad3-188">類別類型的值可以轉換成類型 `object`，或是在編譯時期將參考視為另一個類型，藉此將其實作為介面類別型。</span><span class="sxs-lookup"><span data-stu-id="69ad3-188">A value of a class type can be converted to type `object` or to an interface type that is implemented by the class simply by treating the reference as another type at compile-time.</span></span> <span data-ttu-id="69ad3-189">同樣地，`object` 類型的值或介面類別型的值可以轉換回類別類型，而不需要變更參考（但在此情況下，需要執行時間類型檢查）。</span><span class="sxs-lookup"><span data-stu-id="69ad3-189">Likewise, a value of type `object` or a value of an interface type can be converted back to a class type without changing the reference (but of course a run-time type check is required in this case).</span></span>

<span data-ttu-id="69ad3-190">因為結構不是參考型別，所以這些作業會針對結構型別以不同的方式執行。</span><span class="sxs-lookup"><span data-stu-id="69ad3-190">Since structs are not reference types, these operations are implemented differently for struct types.</span></span> <span data-ttu-id="69ad3-191">當結構類型的值轉換成類型 `object` 或結構實作為介面類別型時，就會進行一個裝箱作業。</span><span class="sxs-lookup"><span data-stu-id="69ad3-191">When a value of a struct type is converted to type `object` or to an interface type that is implemented by the struct, a boxing operation takes place.</span></span> <span data-ttu-id="69ad3-192">同樣地，當類型的值 `object` 或介面類別型的值轉換回結構類型時，就會進行取消程式作業。</span><span class="sxs-lookup"><span data-stu-id="69ad3-192">Likewise, when a value of type `object` or a value of an interface type is converted back to a struct type, an unboxing operation takes place.</span></span> <span data-ttu-id="69ad3-193">與類別類型上相同作業的主要差異在於，裝箱和取消裝箱會將結構值複製到或傳出已裝箱的實例。</span><span class="sxs-lookup"><span data-stu-id="69ad3-193">A key difference from the same operations on class types is that boxing and unboxing copies the struct value either into or out of the boxed instance.</span></span> <span data-ttu-id="69ad3-194">因此，在裝箱或取消封裝作業之後，對取消裝箱結構所做的變更不會反映在已裝箱的結構中。</span><span class="sxs-lookup"><span data-stu-id="69ad3-194">Thus, following a boxing or unboxing operation, changes made to the unboxed struct are not reflected in the boxed struct.</span></span>

<span data-ttu-id="69ad3-195">當結構類型覆寫繼承自 `System.Object` （例如 `Equals`、`GetHashCode` 或 `ToString`）的虛擬方法時，透過結構類型的實例叫用虛擬方法，並不會導致發生裝箱。</span><span class="sxs-lookup"><span data-stu-id="69ad3-195">When a struct type overrides a virtual method inherited from `System.Object` (such as `Equals`, `GetHashCode`, or `ToString`), invocation of the virtual method through an instance of the struct type does not cause boxing to occur.</span></span> <span data-ttu-id="69ad3-196">即使結構是當做型別參數使用，而且會透過型別參數型別的實例進行調用，也是如此。</span><span class="sxs-lookup"><span data-stu-id="69ad3-196">This is true even when the struct is used as a type parameter and the invocation occurs through an instance of the type parameter type.</span></span> <span data-ttu-id="69ad3-197">例如:</span><span class="sxs-lookup"><span data-stu-id="69ad3-197">For example:</span></span>
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

<span data-ttu-id="69ad3-198">程式的輸出為：</span><span class="sxs-lookup"><span data-stu-id="69ad3-198">The output of the program is:</span></span>
```console
1
2
3
```

<span data-ttu-id="69ad3-199">雖然 `ToString` 的樣式不正確，但有副作用，但此範例會示範 `x.ToString()` 的三個調用中未發生任何裝箱。</span><span class="sxs-lookup"><span data-stu-id="69ad3-199">Although it is bad style for `ToString` to have side effects, the example demonstrates that no boxing occurred for the three invocations of `x.ToString()`.</span></span>

<span data-ttu-id="69ad3-200">同樣地，存取限制型別參數上的成員時，也不會隱含地進行裝箱。</span><span class="sxs-lookup"><span data-stu-id="69ad3-200">Similarly, boxing never implicitly occurs when accessing a member on a constrained type parameter.</span></span> <span data-ttu-id="69ad3-201">例如，假設介面 `ICounter` 包含可用於修改值的方法 `Increment`。</span><span class="sxs-lookup"><span data-stu-id="69ad3-201">For example, suppose an interface `ICounter` contains a method `Increment` which can be used to modify a value.</span></span> <span data-ttu-id="69ad3-202">如果 `ICounter` 當做條件約束使用，則會使用在上呼叫 `Increment` 之變數的參考來呼叫 `Increment` 方法，而不是已封裝的複本。</span><span class="sxs-lookup"><span data-stu-id="69ad3-202">If `ICounter` is used as a constraint, the implementation of the `Increment` method is called with a reference to the variable that `Increment` was called on, never a boxed copy.</span></span>

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

<span data-ttu-id="69ad3-203">第一次呼叫 `Increment` 時，會將變數中的值修改 `x`。</span><span class="sxs-lookup"><span data-stu-id="69ad3-203">The first call to `Increment` modifies the value in the variable `x`.</span></span> <span data-ttu-id="69ad3-204">這不等於第二次呼叫 `Increment`，這會修改 `x` 之已封裝複本中的值。</span><span class="sxs-lookup"><span data-stu-id="69ad3-204">This is not equivalent to the second call to `Increment`, which modifies the value in a boxed copy of `x`.</span></span> <span data-ttu-id="69ad3-205">因此，程式的輸出為：</span><span class="sxs-lookup"><span data-stu-id="69ad3-205">Thus, the output of the program is:</span></span>
```console
0
1
1
```

<span data-ttu-id="69ad3-206">如需有關裝箱和取消裝箱的進一步詳細資料，請參閱[裝箱和取消](types.md#boxing-and-unboxing)裝箱。</span><span class="sxs-lookup"><span data-stu-id="69ad3-206">For further details on boxing and unboxing, see [Boxing and unboxing](types.md#boxing-and-unboxing).</span></span>

### <a name="meaning-of-this"></a><span data-ttu-id="69ad3-207">意義</span><span class="sxs-lookup"><span data-stu-id="69ad3-207">Meaning of this</span></span>

<span data-ttu-id="69ad3-208">在類別的實例或實例函式成員內，`this` 會分類為值。</span><span class="sxs-lookup"><span data-stu-id="69ad3-208">Within an instance constructor or instance function member of a class, `this` is classified as a value.</span></span> <span data-ttu-id="69ad3-209">因此，雖然 `this` 可以用來參考叫用函式成員的實例，但無法在類別的函式成員中指派給 `this`。</span><span class="sxs-lookup"><span data-stu-id="69ad3-209">Thus, while `this` can be used to refer to the instance for which the function member was invoked, it is not possible to assign to `this` in a function member of a class.</span></span>

<span data-ttu-id="69ad3-210">在結構的實例函式中，`this` 對應到結構類型的 @no__t 1 參數，而且在結構的實例函式成員內，`this` 對應到結構類型的 `ref` 參數。</span><span class="sxs-lookup"><span data-stu-id="69ad3-210">Within an instance constructor of a struct, `this` corresponds to an `out` parameter of the struct type, and within an instance function member of a struct, `this` corresponds to a `ref` parameter of the struct type.</span></span> <span data-ttu-id="69ad3-211">在這兩種情況下，`this` 會分類為變數，而且可以藉由指派給 `this` 或將它傳遞為 `ref` 或 `out` 參數，來修改叫用函式成員的整個結構。</span><span class="sxs-lookup"><span data-stu-id="69ad3-211">In both cases, `this` is classified as a variable, and it is possible to modify the entire struct for which the function member was invoked by assigning to `this` or by passing this as a `ref` or `out` parameter.</span></span>

### <a name="field-initializers"></a><span data-ttu-id="69ad3-212">欄位初始化運算式</span><span class="sxs-lookup"><span data-stu-id="69ad3-212">Field initializers</span></span>

<span data-ttu-id="69ad3-213">如[預設值](structs.md#default-values)所述，結構的預設值包含將所有數值型別欄位設定為其預設值，以及所有參考類型欄位為 `null` 所產生的值。</span><span class="sxs-lookup"><span data-stu-id="69ad3-213">As described in [Default values](structs.md#default-values), the default value of a struct consists of the value that results from setting all value type fields to their default value and all reference type fields to `null`.</span></span> <span data-ttu-id="69ad3-214">基於這個理由，結構不允許實例欄位宣告包含變數初始化運算式。</span><span class="sxs-lookup"><span data-stu-id="69ad3-214">For this reason, a struct does not permit instance field declarations to include variable initializers.</span></span> <span data-ttu-id="69ad3-215">這種限制僅適用于實例欄位。</span><span class="sxs-lookup"><span data-stu-id="69ad3-215">This restriction applies only to instance fields.</span></span> <span data-ttu-id="69ad3-216">結構的靜態欄位允許包含變數初始化運算式。</span><span class="sxs-lookup"><span data-stu-id="69ad3-216">Static fields of a struct are permitted to include variable initializers.</span></span>

<span data-ttu-id="69ad3-217">範例</span><span class="sxs-lookup"><span data-stu-id="69ad3-217">The example</span></span>
```csharp
struct Point
{
    public int x = 1;  // Error, initializer not permitted
    public int y = 1;  // Error, initializer not permitted
}
```
<span data-ttu-id="69ad3-218">發生錯誤，因為實例欄位宣告包含變數初始化運算式。</span><span class="sxs-lookup"><span data-stu-id="69ad3-218">is in error because the instance field declarations include variable initializers.</span></span>

### <a name="constructors"></a><span data-ttu-id="69ad3-219">建構函式</span><span class="sxs-lookup"><span data-stu-id="69ad3-219">Constructors</span></span>

<span data-ttu-id="69ad3-220">不同于類別，結構不允許宣告無參數的實例的函式。</span><span class="sxs-lookup"><span data-stu-id="69ad3-220">Unlike a class, a struct is not permitted to declare a parameterless instance constructor.</span></span> <span data-ttu-id="69ad3-221">相反地，每個結構隱含具有無參數實例的函式，其一律會傳回將所有數值型別欄位設定為預設值，以及所有參考類型欄位為 null （[預設](types.md#default-constructors)的函式）所產生的值。</span><span class="sxs-lookup"><span data-stu-id="69ad3-221">Instead, every struct implicitly has a parameterless instance constructor which always returns the value that results from setting all value type fields to their default value and all reference type fields to null ([Default constructors](types.md#default-constructors)).</span></span> <span data-ttu-id="69ad3-222">結構可以宣告具有參數的實例構造函式。</span><span class="sxs-lookup"><span data-stu-id="69ad3-222">A struct can declare instance constructors having parameters.</span></span> <span data-ttu-id="69ad3-223">例如：</span><span class="sxs-lookup"><span data-stu-id="69ad3-223">For example</span></span>
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

<span data-ttu-id="69ad3-224">假設上述宣告，語句</span><span class="sxs-lookup"><span data-stu-id="69ad3-224">Given the above declaration, the statements</span></span>
```csharp
Point p1 = new Point();
Point p2 = new Point(0, 0);
```
<span data-ttu-id="69ad3-225">兩者都會建立 `Point`，`x`，而 `y` 初始化為零。</span><span class="sxs-lookup"><span data-stu-id="69ad3-225">both create a `Point` with `x` and `y` initialized to zero.</span></span>

<span data-ttu-id="69ad3-226">結構實例的函式不能包含格式為 `base(...)` 的函數初始化運算式。</span><span class="sxs-lookup"><span data-stu-id="69ad3-226">A struct instance constructor is not permitted to include a constructor initializer of the form `base(...)`.</span></span>

<span data-ttu-id="69ad3-227">如果結構實例的建立式不會指定函式初始化運算式，則 @no__t 0 變數會對應至結構類型的 @no__t 1 參數，而且類似于 @no__t 2 參數，`this` 必須明確指派（[明確指派](variables.md#definite-assignment)），位於此函式所傳回的每個位置。</span><span class="sxs-lookup"><span data-stu-id="69ad3-227">If the struct instance constructor doesn't specify a constructor initializer, the `this` variable corresponds to an `out` parameter of the struct type, and similar to an `out` parameter, `this` must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) at every location where the constructor returns.</span></span> <span data-ttu-id="69ad3-228">如果結構實例的函式指定了一個函式初始化運算式，則 `this` 變數會對應至結構類型的 @no__t 1 參數，而且類似于 @no__t 2 參數，`this` 會視為在對該函數主體的專案上明確指派.</span><span class="sxs-lookup"><span data-stu-id="69ad3-228">If the struct instance constructor specifies a constructor initializer, the `this` variable corresponds to a `ref` parameter of the struct type, and similar to a `ref` parameter, `this` is considered definitely assigned on entry to the constructor body.</span></span> <span data-ttu-id="69ad3-229">請考慮下列的實例函式執行：</span><span class="sxs-lookup"><span data-stu-id="69ad3-229">Consider the instance constructor implementation below:</span></span>
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

<span data-ttu-id="69ad3-230">您必須先明確指派結構的所有欄位，才能呼叫實例成員函式（包括屬性 `X` 和 `Y` 的 set 存取子）。</span><span class="sxs-lookup"><span data-stu-id="69ad3-230">No instance member function (including the set accessors for the properties `X` and `Y`) can be called until all fields of the struct being constructed have been definitely assigned.</span></span> <span data-ttu-id="69ad3-231">唯一的例外狀況包括自動實作為屬性（[自動實作為屬性](classes.md#automatically-implemented-properties)）。</span><span class="sxs-lookup"><span data-stu-id="69ad3-231">The only exception involves automatically implemented properties ([Automatically implemented properties](classes.md#automatically-implemented-properties)).</span></span> <span data-ttu-id="69ad3-232">明確指派規則（[簡單指派運算式](variables.md#simple-assignment-expressions)）會特別豁免指派給該結構型別之實例函式中結構型別的自動屬性（property），這種指派會被視為隱藏的明確指派。auto 屬性的支援欄位。</span><span class="sxs-lookup"><span data-stu-id="69ad3-232">The definite assignment rules ([Simple assignment expressions](variables.md#simple-assignment-expressions)) specifically exempt assignment to an auto-property of a struct type within an instance constructor of that struct type: such an assignment is considered a definite assignment of the hidden backing field of the auto-property.</span></span> <span data-ttu-id="69ad3-233">因此，允許下列各項：</span><span class="sxs-lookup"><span data-stu-id="69ad3-233">Thus, the following is allowed:</span></span>

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

### <a name="destructors"></a><span data-ttu-id="69ad3-234">解構函式</span><span class="sxs-lookup"><span data-stu-id="69ad3-234">Destructors</span></span>

<span data-ttu-id="69ad3-235">不允許結構宣告析構函式。</span><span class="sxs-lookup"><span data-stu-id="69ad3-235">A struct is not permitted to declare a destructor.</span></span>

### <a name="static-constructors"></a><span data-ttu-id="69ad3-236">靜態建構函式</span><span class="sxs-lookup"><span data-stu-id="69ad3-236">Static constructors</span></span>

<span data-ttu-id="69ad3-237">結構的靜態構造函式會遵循與類別相同的大部分規則。</span><span class="sxs-lookup"><span data-stu-id="69ad3-237">Static constructors for structs follow most of the same rules as for classes.</span></span> <span data-ttu-id="69ad3-238">結構類型的靜態函式執行是由下列事件中的第一個所觸發，以在應用程式域內發生：</span><span class="sxs-lookup"><span data-stu-id="69ad3-238">The execution of a static constructor for a struct type is triggered by the first of the following events to occur within an application domain:</span></span>

*  <span data-ttu-id="69ad3-239">會參考結構類型的靜態成員。</span><span class="sxs-lookup"><span data-stu-id="69ad3-239">A static member of the struct type is referenced.</span></span>
*  <span data-ttu-id="69ad3-240">會呼叫結構類型的明確宣告的函式。</span><span class="sxs-lookup"><span data-stu-id="69ad3-240">An explicitly declared constructor of the struct type is called.</span></span>

<span data-ttu-id="69ad3-241">建立結構類型的預設值（[預設值](structs.md#default-values)）並不會觸發靜態的函式。</span><span class="sxs-lookup"><span data-stu-id="69ad3-241">The creation of default values ([Default values](structs.md#default-values)) of struct types does not trigger the static constructor.</span></span> <span data-ttu-id="69ad3-242">（其中一個範例是陣列中元素的初始值）。</span><span class="sxs-lookup"><span data-stu-id="69ad3-242">(An example of this is the initial value of elements in an array.)</span></span>

## <a name="struct-examples"></a><span data-ttu-id="69ad3-243">結構範例</span><span class="sxs-lookup"><span data-stu-id="69ad3-243">Struct examples</span></span>

<span data-ttu-id="69ad3-244">以下顯示兩個使用 @no__t 0 型別來建立類型的重要範例，其使用方式類似于預先定義的語言類型，但具有修改過的語義。</span><span class="sxs-lookup"><span data-stu-id="69ad3-244">The following shows two significant examples of using `struct` types to create types that can be used similarly to the predefined types of the language, but with modified semantics.</span></span>

### <a name="database-integer-type"></a><span data-ttu-id="69ad3-245">資料庫整數類型</span><span class="sxs-lookup"><span data-stu-id="69ad3-245">Database integer type</span></span>

<span data-ttu-id="69ad3-246">下列的 `DBInt` 結構會執行整數類型，可以代表一組完整的 `int` 類型值，再加上表示未知值的其他狀態。</span><span class="sxs-lookup"><span data-stu-id="69ad3-246">The `DBInt` struct below implements an integer type that can represent the complete set of values of the `int` type, plus an additional state that indicates an unknown value.</span></span> <span data-ttu-id="69ad3-247">具有這些特性的類型通常用於資料庫中。</span><span class="sxs-lookup"><span data-stu-id="69ad3-247">A type with these characteristics is commonly used in databases.</span></span>

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

### <a name="database-boolean-type"></a><span data-ttu-id="69ad3-248">資料庫布林值類型</span><span class="sxs-lookup"><span data-stu-id="69ad3-248">Database boolean type</span></span>

<span data-ttu-id="69ad3-249">下列的 `DBBool` 結構會執行三值的邏輯型別。</span><span class="sxs-lookup"><span data-stu-id="69ad3-249">The `DBBool` struct below implements a three-valued logical type.</span></span> <span data-ttu-id="69ad3-250">此類型的可能值為 `DBBool.True`、`DBBool.False` 和 `DBBool.Null`，其中 `Null` 成員表示未知的值。</span><span class="sxs-lookup"><span data-stu-id="69ad3-250">The possible values of this type are `DBBool.True`, `DBBool.False`, and `DBBool.Null`, where the `Null` member indicates an unknown value.</span></span> <span data-ttu-id="69ad3-251">這類三值邏輯類型通常用於資料庫中。</span><span class="sxs-lookup"><span data-stu-id="69ad3-251">Such three-valued logical types are commonly used in databases.</span></span>

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
