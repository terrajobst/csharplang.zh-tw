# <a name="structs"></a><span data-ttu-id="61e06-101">結構</span><span class="sxs-lookup"><span data-stu-id="61e06-101">Structs</span></span>

<span data-ttu-id="61e06-102">結構是類似於類別，在於它們代表可以包含資料成員和函式成員的資料結構。</span><span class="sxs-lookup"><span data-stu-id="61e06-102">Structs are similar to classes in that they represent data structures that can contain data members and function members.</span></span> <span data-ttu-id="61e06-103">不過，不同於類別，結構是實值類型，而且不需要堆積配置。</span><span class="sxs-lookup"><span data-stu-id="61e06-103">However, unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="61e06-104">結構類型的變數直接包含資料的結構，而類別類型的變數包含資料，後者稱為物件的參考。</span><span class="sxs-lookup"><span data-stu-id="61e06-104">A variable of a struct type directly contains the data of the struct, whereas a variable of a class type contains a reference to the data, the latter known as an object.</span></span>

<span data-ttu-id="61e06-105">結構特別適用於含有實值語意的小型資料結構。</span><span class="sxs-lookup"><span data-stu-id="61e06-105">Structs are particularly useful for small data structures that have value semantics.</span></span> <span data-ttu-id="61e06-106">複數、座標系統中的點或字典中的索引鍵/值組都是結構的良好範例。</span><span class="sxs-lookup"><span data-stu-id="61e06-106">Complex numbers, points in a coordinate system, or key-value pairs in a dictionary are all good examples of structs.</span></span> <span data-ttu-id="61e06-107">這些資料結構的索引鍵是他們擁有較少的資料成員，它們不需要使用繼承或參考的身分識別，以及他們可以透過設定複製而非參考值的實值語意來輕鬆實作。</span><span class="sxs-lookup"><span data-stu-id="61e06-107">Key to these data structures is that they have few data members, that they do not require use of inheritance or referential identity, and that they can be conveniently implemented using value semantics where assignment copies the value instead of the reference.</span></span>

<span data-ttu-id="61e06-108">中所述[簡單型別](types.md#simple-types)，例如 C# 中，所提供的簡單類型`int`， `double`，和`bool`，事實上是所有結構類型。</span><span class="sxs-lookup"><span data-stu-id="61e06-108">As described in [Simple types](types.md#simple-types), the simple types provided by C#, such as `int`, `double`, and `bool`, are in fact all struct types.</span></span> <span data-ttu-id="61e06-109">就如同這些預先定義的型別為結構，它也可使用結構和 C# 語言中實作新的 「 基本 」 型別多載的運算子。</span><span class="sxs-lookup"><span data-stu-id="61e06-109">Just as these predefined types are structs, it is also possible to use structs and operator overloading to implement new "primitive" types in the C# language.</span></span> <span data-ttu-id="61e06-110">此章節的結尾會提供這些類型的兩個範例 ([結構範例](structs.md#struct-examples))。</span><span class="sxs-lookup"><span data-stu-id="61e06-110">Two examples of such types are given at the end of this chapter ([Struct examples](structs.md#struct-examples)).</span></span>

## <a name="struct-declarations"></a><span data-ttu-id="61e06-111">結構宣告</span><span class="sxs-lookup"><span data-stu-id="61e06-111">Struct declarations</span></span>

<span data-ttu-id="61e06-112">A *struct_declaration*是*type_declaration* ([型別宣告](namespaces.md#type-declarations)) 宣告新的結構：</span><span class="sxs-lookup"><span data-stu-id="61e06-112">A *struct_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new struct:</span></span>

```antlr
struct_declaration
    : attributes? struct_modifier* 'partial'? 'struct' identifier type_parameter_list?
      struct_interfaces? type_parameter_constraints_clause* struct_body ';'?
    ;
```

<span data-ttu-id="61e06-113">A *struct_declaration*組成的一組選擇性*屬性*([屬性](attributes.md))，後面接著一組選擇性的*struct_modifier*s ([結構修飾詞](structs.md#struct-modifiers))，後面接著選擇性`partial`修飾詞，後面接著關鍵字`struct`並*識別碼*可命名結構，後面接著選擇性*type_parameter_list*規格 ([型別參數](classes.md#type-parameters))，後面接著選擇性*struct_interfaces*規格 ([Partial 修飾詞](structs.md#partial-modifier)))，後面接著選擇性*type_parameter_constraints_clause*s 規格 ([類型參數條件約束](classes.md#type-parameter-constraints))，後面接著*struct_body* ([結構主體](structs.md#struct-body))，或者後面接著一個分號。</span><span class="sxs-lookup"><span data-stu-id="61e06-113">A *struct_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *struct_modifier*s ([Struct modifiers](structs.md#struct-modifiers)), followed by an optional `partial` modifier, followed by the keyword `struct` and an *identifier* that names the struct, followed by an optional *type_parameter_list* specification ([Type parameters](classes.md#type-parameters)), followed by an optional *struct_interfaces* specification ([Partial modifier](structs.md#partial-modifier)) ), followed by an optional *type_parameter_constraints_clause*s specification ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by a *struct_body* ([Struct body](structs.md#struct-body)), optionally followed by a semicolon.</span></span>

### <a name="struct-modifiers"></a><span data-ttu-id="61e06-114">結構修飾詞</span><span class="sxs-lookup"><span data-stu-id="61e06-114">Struct modifiers</span></span>

<span data-ttu-id="61e06-115">A *struct_declaration*可以選擇性地包含一連串的結構修飾詞：</span><span class="sxs-lookup"><span data-stu-id="61e06-115">A *struct_declaration* may optionally include a sequence of struct modifiers:</span></span>

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

<span data-ttu-id="61e06-116">它是在結構宣告中出現多次相同的修飾詞的編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="61e06-116">It is a compile-time error for the same modifier to appear multiple times in a struct declaration.</span></span>

<span data-ttu-id="61e06-117">結構宣告的修飾詞具有相同的意義的類別宣告 ([類別宣告](classes.md#class-declarations))。</span><span class="sxs-lookup"><span data-stu-id="61e06-117">The modifiers of a struct declaration have the same meaning as those of a class declaration ([Class declarations](classes.md#class-declarations)).</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="61e06-118">Partial 修飾詞</span><span class="sxs-lookup"><span data-stu-id="61e06-118">Partial modifier</span></span>

<span data-ttu-id="61e06-119">`partial`修飾詞表示這*struct_declaration*是部分的型別宣告。</span><span class="sxs-lookup"><span data-stu-id="61e06-119">The `partial` modifier indicates that this *struct_declaration* is a partial type declaration.</span></span> <span data-ttu-id="61e06-120">多個具有相同的名稱，在封入的命名空間或類型宣告中的部分結構宣告結合以形成一個結構宣告，下列規則中指定[部分型別](classes.md#partial-types)。</span><span class="sxs-lookup"><span data-stu-id="61e06-120">Multiple partial struct declarations with the same name within an enclosing namespace or type declaration combine to form one struct declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

### <a name="struct-interfaces"></a><span data-ttu-id="61e06-121">結構的介面</span><span class="sxs-lookup"><span data-stu-id="61e06-121">Struct interfaces</span></span>

<span data-ttu-id="61e06-122">結構宣告可能包含*struct_interfaces*規格，稱為直接實作特定的介面型別案例結構。</span><span class="sxs-lookup"><span data-stu-id="61e06-122">A struct declaration may include a *struct_interfaces* specification, in which case the struct is said to directly implement the given interface types.</span></span>

```antlr
struct_interfaces
    : ':' interface_type_list
    ;
```

<span data-ttu-id="61e06-123">介面實作討論中進一步[介面實作](interfaces.md#interface-implementations)。</span><span class="sxs-lookup"><span data-stu-id="61e06-123">Interface implementations are discussed further in [Interface implementations](interfaces.md#interface-implementations).</span></span>

### <a name="struct-body"></a><span data-ttu-id="61e06-124">結構主體</span><span class="sxs-lookup"><span data-stu-id="61e06-124">Struct body</span></span>

<span data-ttu-id="61e06-125">*Struct_body*結構會定義結構的成員。</span><span class="sxs-lookup"><span data-stu-id="61e06-125">The *struct_body* of a struct defines the members of the struct.</span></span>

```antlr
struct_body
    : '{' struct_member_declaration* '}'
    ;
```

## <a name="struct-members"></a><span data-ttu-id="61e06-126">結構成員</span><span class="sxs-lookup"><span data-stu-id="61e06-126">Struct members</span></span>

<span data-ttu-id="61e06-127">結構的成員包含所引入的成員及其*struct_member_declaration*s 和成員繼承自型別`System.ValueType`。</span><span class="sxs-lookup"><span data-stu-id="61e06-127">The members of a struct consist of the members introduced by its *struct_member_declaration*s and the members inherited from the type `System.ValueType`.</span></span>

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

<span data-ttu-id="61e06-128">除了中所述的差異[類別和結構的差異](structs.md#class-and-struct-differences)中, 提供的類別成員的說明[類別成員](classes.md#class-members)透過[迭代器](classes.md#iterators)套用至結構以及成員。</span><span class="sxs-lookup"><span data-stu-id="61e06-128">Except for the differences noted in [Class and struct differences](structs.md#class-and-struct-differences), the descriptions of class members provided in [Class members](classes.md#class-members) through [Iterators](classes.md#iterators) apply to struct members as well.</span></span>

## <a name="class-and-struct-differences"></a><span data-ttu-id="61e06-129">類別和結構的差異</span><span class="sxs-lookup"><span data-stu-id="61e06-129">Class and struct differences</span></span>

<span data-ttu-id="61e06-130">與類別的結構是在數個重要方面不同：</span><span class="sxs-lookup"><span data-stu-id="61e06-130">Structs differ from classes in several important ways:</span></span>

*  <span data-ttu-id="61e06-131">結構是實值型別 ([值語意](structs.md#value-semantics))。</span><span class="sxs-lookup"><span data-stu-id="61e06-131">Structs are value types ([Value semantics](structs.md#value-semantics)).</span></span>
*  <span data-ttu-id="61e06-132">所有結構類型都隱含地都繼承自類別`System.ValueType`([繼承](structs.md#inheritance))。</span><span class="sxs-lookup"><span data-stu-id="61e06-132">All struct types implicitly inherit from the class `System.ValueType` ([Inheritance](structs.md#inheritance)).</span></span>
*  <span data-ttu-id="61e06-133">指派給變數的結構類型會建立一份所指派的值 ([指派](structs.md#assignment))。</span><span class="sxs-lookup"><span data-stu-id="61e06-133">Assignment to a variable of a struct type creates a copy of the value being assigned ([Assignment](structs.md#assignment)).</span></span>
*  <span data-ttu-id="61e06-134">結構的預設值是藉由將所有實值型別欄位設定為其預設值和所有參考型別欄位為產生的值`null`([預設值](structs.md#default-values))。</span><span class="sxs-lookup"><span data-stu-id="61e06-134">The default value of a struct is the value produced by setting all value type fields to their default value and all reference type fields to `null` ([Default values](structs.md#default-values)).</span></span>
*  <span data-ttu-id="61e06-135">Boxing 和 unboxing 作業可用於結構類型之間轉換以及`object`([Boxing 和 unboxing](structs.md#boxing-and-unboxing))。</span><span class="sxs-lookup"><span data-stu-id="61e06-135">Boxing and unboxing operations are used to convert between a struct type and `object` ([Boxing and unboxing](structs.md#boxing-and-unboxing)).</span></span>
*  <span data-ttu-id="61e06-136">意義`this`是不同的結構 ([這項存取](expressions.md#this-access))。</span><span class="sxs-lookup"><span data-stu-id="61e06-136">The meaning of `this` is different for structs ([This access](expressions.md#this-access)).</span></span>
*  <span data-ttu-id="61e06-137">結構的執行個體欄位宣告不得包含變數的初始設定式 ([欄位初始設定式](structs.md#field-initializers))。</span><span class="sxs-lookup"><span data-stu-id="61e06-137">Instance field declarations for a struct are not permitted to include variable initializers ([Field initializers](structs.md#field-initializers)).</span></span>
*  <span data-ttu-id="61e06-138">結構不允許宣告無參數的執行個體建構函式 ([建構函式](structs.md#constructors))。</span><span class="sxs-lookup"><span data-stu-id="61e06-138">A struct is not permitted to declare a parameterless instance constructor ([Constructors](structs.md#constructors)).</span></span>
*  <span data-ttu-id="61e06-139">結構不允許宣告解構函式 ([解構函式](structs.md#destructors))。</span><span class="sxs-lookup"><span data-stu-id="61e06-139">A struct is not permitted to declare a destructor ([Destructors](structs.md#destructors)).</span></span>

### <a name="value-semantics"></a><span data-ttu-id="61e06-140">實值語意</span><span class="sxs-lookup"><span data-stu-id="61e06-140">Value semantics</span></span>

<span data-ttu-id="61e06-141">結構是實值型別 ([實值型別](types.md#value-types)) 並被視為具有實值語意。</span><span class="sxs-lookup"><span data-stu-id="61e06-141">Structs are value types ([Value types](types.md#value-types)) and are said to have value semantics.</span></span> <span data-ttu-id="61e06-142">類別，相反地，是參考型別 ([參考的型別](types.md#reference-types)) 並被視為具有參考語意。</span><span class="sxs-lookup"><span data-stu-id="61e06-142">Classes, on the other hand, are reference types ([Reference types](types.md#reference-types)) and are said to have reference semantics.</span></span>

<span data-ttu-id="61e06-143">結構類型的變數直接包含資料的結構，而類別類型的變數包含資料，後者稱為物件的參考。</span><span class="sxs-lookup"><span data-stu-id="61e06-143">A variable of a struct type directly contains the data of the struct, whereas a variable of a class type contains a reference to the data, the latter known as an object.</span></span> <span data-ttu-id="61e06-144">當結構`B`包含的型別執行個體欄位`A`和`A`是結構類型時，會產生編譯時期錯誤`A`取決於`B`或從類型建構`B`。</span><span class="sxs-lookup"><span data-stu-id="61e06-144">When a struct `B` contains an instance field of type `A` and `A` is a struct type, it is a compile-time error for `A` to depend on `B` or a type constructed from `B`.</span></span> <span data-ttu-id="61e06-145">結構`X`***直接相依於***結構`Y`如果`X`包含類型的執行個體欄位`Y`。</span><span class="sxs-lookup"><span data-stu-id="61e06-145">A struct `X` ***directly depends on*** a struct `Y` if `X` contains an instance field of type `Y`.</span></span> <span data-ttu-id="61e06-146">如果已指定這個定義，一組完整的結構所依存的結構是可轉移關閉***直接相依於***關聯性。</span><span class="sxs-lookup"><span data-stu-id="61e06-146">Given this definition, the complete set of structs upon which a struct depends is the transitive closure of the ***directly depends on*** relationship.</span></span>  <span data-ttu-id="61e06-147">例如</span><span class="sxs-lookup"><span data-stu-id="61e06-147">For example</span></span>
```csharp
struct Node
{
    int data;
    Node next; // error, Node directly depends on itself
}
```
<span data-ttu-id="61e06-148">會發生錯誤，因為`Node`包含其本身類型的執行個體欄位。</span><span class="sxs-lookup"><span data-stu-id="61e06-148">is an error because `Node` contains an instance field of its own type.</span></span>  <span data-ttu-id="61e06-149">另一個範例</span><span class="sxs-lookup"><span data-stu-id="61e06-149">Another example</span></span>
```csharp
struct A { B b; }

struct B { C c; }

struct C { A a; }
```
<span data-ttu-id="61e06-150">會發生錯誤，因為每個型別`A`， `B`，和`C`彼此相依。</span><span class="sxs-lookup"><span data-stu-id="61e06-150">is an error because each of the types `A`, `B`, and `C` depend on each other.</span></span>

<span data-ttu-id="61e06-151">類別中，很可能兩個變數來參考相同的物件，且因此可能會影響其他變數所參考的物件的一個變數上進行的作業。</span><span class="sxs-lookup"><span data-stu-id="61e06-151">With classes, it is possible for two variables to reference the same object, and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="61e06-152">使用結構時，每個變數都有自己的資料複本 (的情況除外`ref`和`out`參數變數)，而且不可能會影響其他的其中一個上的作業。</span><span class="sxs-lookup"><span data-stu-id="61e06-152">With structs, the variables each have their own copy of the data (except in the case of `ref` and `out` parameter variables), and it is not possible for operations on one to affect the other.</span></span> <span data-ttu-id="61e06-153">此外，因為結構不是參考型別，它不是結構類型的值可能`null`。</span><span class="sxs-lookup"><span data-stu-id="61e06-153">Furthermore, because structs are not reference types, it is not possible for values of a struct type to be `null`.</span></span>

<span data-ttu-id="61e06-154">指定的宣告</span><span class="sxs-lookup"><span data-stu-id="61e06-154">Given the declaration</span></span>
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
<span data-ttu-id="61e06-155">程式碼片段</span><span class="sxs-lookup"><span data-stu-id="61e06-155">the code fragment</span></span>
```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 100;
System.Console.WriteLine(b.x);
```
<span data-ttu-id="61e06-156">輸出值`10`。</span><span class="sxs-lookup"><span data-stu-id="61e06-156">outputs the value `10`.</span></span> <span data-ttu-id="61e06-157">指派`a`來`b`會建立一份值，並`b`不會因此受到指派給`a.x`。</span><span class="sxs-lookup"><span data-stu-id="61e06-157">The assignment of `a` to `b` creates a copy of the value, and `b` is thus unaffected by the assignment to `a.x`.</span></span> <span data-ttu-id="61e06-158">有`Point`改為已宣告為類別，則輸出會是`100`因為`a`和`b`會參考相同的物件。</span><span class="sxs-lookup"><span data-stu-id="61e06-158">Had `Point` instead been declared as a class, the output would be `100` because `a` and `b` would reference the same object.</span></span>

### <a name="inheritance"></a><span data-ttu-id="61e06-159">繼承</span><span class="sxs-lookup"><span data-stu-id="61e06-159">Inheritance</span></span>

<span data-ttu-id="61e06-160">所有結構類型都隱含地都繼承自類別`System.ValueType`，它會接著繼承自類別`object`。</span><span class="sxs-lookup"><span data-stu-id="61e06-160">All struct types implicitly inherit from the class `System.ValueType`, which, in turn, inherits from class `object`.</span></span> <span data-ttu-id="61e06-161">結構宣告可能會指定一份實作的介面，但它並不適用結構宣告，以指定的基底類別。</span><span class="sxs-lookup"><span data-stu-id="61e06-161">A struct declaration may specify a list of implemented interfaces, but it is not possible for a struct declaration to specify a base class.</span></span>

<span data-ttu-id="61e06-162">結構類型不能為抽象，並一律隱含地密封。</span><span class="sxs-lookup"><span data-stu-id="61e06-162">Struct types are never abstract and are always implicitly sealed.</span></span> <span data-ttu-id="61e06-163">`abstract`和`sealed`結構宣告中也因此不允許修飾詞。</span><span class="sxs-lookup"><span data-stu-id="61e06-163">The `abstract` and `sealed` modifiers are therefore not permitted in a struct declaration.</span></span>

<span data-ttu-id="61e06-164">因為結構不支援繼承，結構成員的宣告存取範圍不能`protected`或`protected internal`。</span><span class="sxs-lookup"><span data-stu-id="61e06-164">Since inheritance isn't supported for structs, the declared accessibility of a struct member cannot be `protected` or `protected internal`.</span></span>

<span data-ttu-id="61e06-165">在結構中的函式成員不能`abstract`或`virtual`，而`override`修飾詞只允許對覆寫方法繼承自`System.ValueType`。</span><span class="sxs-lookup"><span data-stu-id="61e06-165">Function members in a struct cannot be `abstract` or `virtual`, and the `override` modifier is allowed only to override methods inherited from `System.ValueType`.</span></span>

### <a name="assignment"></a><span data-ttu-id="61e06-166">指派</span><span class="sxs-lookup"><span data-stu-id="61e06-166">Assignment</span></span>

<span data-ttu-id="61e06-167">指派給變數的結構類型會建立一份所指派的值。</span><span class="sxs-lookup"><span data-stu-id="61e06-167">Assignment to a variable of a struct type creates a copy of the value being assigned.</span></span> <span data-ttu-id="61e06-168">這不同於指派給變數的類別類型時，它將會複製參考，而不是參考所識別的物件。</span><span class="sxs-lookup"><span data-stu-id="61e06-168">This differs from assignment to a variable of a class type, which copies the reference but not the object identified by the reference.</span></span>

<span data-ttu-id="61e06-169">類似於指派，結構是做為值參數傳遞或傳回為函式成員的結果時，將會建立結構的複本。</span><span class="sxs-lookup"><span data-stu-id="61e06-169">Similar to an assignment, when a struct is passed as a value parameter or returned as the result of a function member, a copy of the struct is created.</span></span> <span data-ttu-id="61e06-170">可能使用函式成員的參考所傳遞的結構`ref`或`out`參數。</span><span class="sxs-lookup"><span data-stu-id="61e06-170">A struct may be passed by reference to a function member using a `ref` or `out` parameter.</span></span>

<span data-ttu-id="61e06-171">當屬性或索引子的結構指派的目標，執行個體相關聯的運算式屬性或索引子存取必須歸類為變數。</span><span class="sxs-lookup"><span data-stu-id="61e06-171">When a property or indexer of a struct is the target of an assignment, the instance expression associated with the property or indexer access must be classified as a variable.</span></span> <span data-ttu-id="61e06-172">如果執行個體運算式分類為值，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="61e06-172">If the instance expression is classified as a value, a compile-time error occurs.</span></span> <span data-ttu-id="61e06-173">這會進一步詳細說明[簡單指派](expressions.md#simple-assignment)。</span><span class="sxs-lookup"><span data-stu-id="61e06-173">This is described in further detail in [Simple assignment](expressions.md#simple-assignment).</span></span>

### <a name="default-values"></a><span data-ttu-id="61e06-174">預設值</span><span class="sxs-lookup"><span data-stu-id="61e06-174">Default values</span></span>

<span data-ttu-id="61e06-175">中所述[預設值](variables.md#default-values)，有數種變數會自動初始化為其預設值建立時。</span><span class="sxs-lookup"><span data-stu-id="61e06-175">As described in [Default values](variables.md#default-values), several kinds of variables are automatically initialized to their default value when they are created.</span></span> <span data-ttu-id="61e06-176">類別型別和其他參考型別變數，此預設值是`null`。</span><span class="sxs-lookup"><span data-stu-id="61e06-176">For variables of class types and other reference types, this default value is `null`.</span></span> <span data-ttu-id="61e06-177">不過，因為結構是實值型別，不能`null`，預設值的結構會將所有實值型別欄位設定為其預設值和所有參考型別欄位為所產生的值`null`。</span><span class="sxs-lookup"><span data-stu-id="61e06-177">However, since structs are value types that cannot be `null`, the default value of a struct is the value produced by setting all value type fields to their default value and all reference type fields to `null`.</span></span>

<span data-ttu-id="61e06-178">參考`Point`上述的範例中，宣告結構</span><span class="sxs-lookup"><span data-stu-id="61e06-178">Referring to the `Point` struct declared above, the example</span></span>
```csharp
Point[] a = new Point[100];
```
<span data-ttu-id="61e06-179">初始化每個`Point`陣列中要設定所產生的值`x`和`y`欄位設為零。</span><span class="sxs-lookup"><span data-stu-id="61e06-179">initializes each `Point` in the array to the value produced by setting the `x` and `y` fields to zero.</span></span>

<span data-ttu-id="61e06-180">結構的預設值對應至結構中的預設建構函式所傳回的值 ([預設建構函式](types.md#default-constructors))。</span><span class="sxs-lookup"><span data-stu-id="61e06-180">The default value of a struct corresponds to the value returned by the default constructor of the struct ([Default constructors](types.md#default-constructors)).</span></span> <span data-ttu-id="61e06-181">不同於類別中，結構不是允許宣告無參數的執行個體建構函式。</span><span class="sxs-lookup"><span data-stu-id="61e06-181">Unlike a class, a struct is not permitted to declare a parameterless instance constructor.</span></span> <span data-ttu-id="61e06-182">相反地，每個結構會隱含地具有無參數的執行個體建構函式一律會傳回值所產生將所有實值型別欄位設定為其預設值和所有參考型別欄位為`null`。</span><span class="sxs-lookup"><span data-stu-id="61e06-182">Instead, every struct implicitly has a parameterless instance constructor which always returns the value that results from setting all value type fields to their default value and all reference type fields to `null`.</span></span>

<span data-ttu-id="61e06-183">結構應該設計成有效的狀態，請考慮預設的初始化狀態。</span><span class="sxs-lookup"><span data-stu-id="61e06-183">Structs should be designed to consider the default initialization state a valid state.</span></span> <span data-ttu-id="61e06-184">在範例</span><span class="sxs-lookup"><span data-stu-id="61e06-184">In the example</span></span>
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
<span data-ttu-id="61e06-185">使用者定義的執行個體建構函式可防止只呼叫它的位置明確的 null 值。</span><span class="sxs-lookup"><span data-stu-id="61e06-185">the user-defined instance constructor protects against null values only where it is explicitly called.</span></span> <span data-ttu-id="61e06-186">萬一其中`KeyValuePair`變數預設值初始化，可能受到`key`和`value`欄位將會是 null，和結構必須準備好處理這個狀態。</span><span class="sxs-lookup"><span data-stu-id="61e06-186">In cases where a `KeyValuePair` variable is subject to default value initialization, the `key` and `value` fields will be null, and the struct must be prepared to handle this state.</span></span>

### <a name="boxing-and-unboxing"></a><span data-ttu-id="61e06-187">Boxing 和 Unboxing</span><span class="sxs-lookup"><span data-stu-id="61e06-187">Boxing and unboxing</span></span>

<span data-ttu-id="61e06-188">類別類型的值可以轉換成輸入`object`或由類別實作，只要參考視為另一種類型在編譯時期為介面類型。</span><span class="sxs-lookup"><span data-stu-id="61e06-188">A value of a class type can be converted to type `object` or to an interface type that is implemented by the class simply by treating the reference as another type at compile-time.</span></span> <span data-ttu-id="61e06-189">同樣地，類型的值`object`或介面類型的值可以轉換回類別型別，而不需要變更參考 （但當然的執行階段類型檢查需要在此情況下）。</span><span class="sxs-lookup"><span data-stu-id="61e06-189">Likewise, a value of type `object` or a value of an interface type can be converted back to a class type without changing the reference (but of course a run-time type check is required in this case).</span></span>

<span data-ttu-id="61e06-190">因為結構不是參考型別，這些作業的結構類型的實作方式不同。</span><span class="sxs-lookup"><span data-stu-id="61e06-190">Since structs are not reference types, these operations are implemented differently for struct types.</span></span> <span data-ttu-id="61e06-191">結構類型的值會轉換成類型`object`或結構實作介面型別，boxing 作業就會發生。</span><span class="sxs-lookup"><span data-stu-id="61e06-191">When a value of a struct type is converted to type `object` or to an interface type that is implemented by the struct, a boxing operation takes place.</span></span> <span data-ttu-id="61e06-192">同樣地，當類型的值`object`或介面類型的值會轉換成結構的型別，unboxing 作業會發生。</span><span class="sxs-lookup"><span data-stu-id="61e06-192">Likewise, when a value of type `object` or a value of an interface type is converted back to a struct type, an unboxing operation takes place.</span></span> <span data-ttu-id="61e06-193">從相同的作業，在類別類型上的主要差異是，boxing 和 unboxing 結構會將值複製到或已封裝的執行個體。</span><span class="sxs-lookup"><span data-stu-id="61e06-193">A key difference from the same operations on class types is that boxing and unboxing copies the struct value either into or out of the boxed instance.</span></span> <span data-ttu-id="61e06-194">因此，下列 boxing 或 unboxing 作業時，unboxed 結構所做的變更不會反映在 boxed 結構中。</span><span class="sxs-lookup"><span data-stu-id="61e06-194">Thus, following a boxing or unboxing operation, changes made to the unboxed struct are not reflected in the boxed struct.</span></span>

<span data-ttu-id="61e06-195">當結構類型會覆寫虛擬方法繼承自`System.Object`(例如`Equals`， `GetHashCode`，或`ToString`)，透過結構類型的執行個體的虛擬方法的引動過程並不會發生 boxing。</span><span class="sxs-lookup"><span data-stu-id="61e06-195">When a struct type overrides a virtual method inherited from `System.Object` (such as `Equals`, `GetHashCode`, or `ToString`), invocation of the virtual method through an instance of the struct type does not cause boxing to occur.</span></span> <span data-ttu-id="61e06-196">即使當類別可用來做為型別參數，並透過型別參數類型的執行個體的引動過程，也是如此。</span><span class="sxs-lookup"><span data-stu-id="61e06-196">This is true even when the struct is used as a type parameter and the invocation occurs through an instance of the type parameter type.</span></span> <span data-ttu-id="61e06-197">例如: </span><span class="sxs-lookup"><span data-stu-id="61e06-197">For example:</span></span>
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

<span data-ttu-id="61e06-198">程式輸出為：</span><span class="sxs-lookup"><span data-stu-id="61e06-198">The output of the program is:</span></span>
```
1
2
3
```

<span data-ttu-id="61e06-199">雖然它是不正確樣式`ToString`有副作用，此範例會示範的三個引動過程，發生任何 boxing `x.ToString()`。</span><span class="sxs-lookup"><span data-stu-id="61e06-199">Although it is bad style for `ToString` to have side effects, the example demonstrates that no boxing occurred for the three invocations of `x.ToString()`.</span></span>

<span data-ttu-id="61e06-200">同樣地，永遠不會隱含 boxing 發生於存取受條件約束的型別參數上的成員。</span><span class="sxs-lookup"><span data-stu-id="61e06-200">Similarly, boxing never implicitly occurs when accessing a member on a constrained type parameter.</span></span> <span data-ttu-id="61e06-201">例如，假設介面`ICounter`包含的方法`Increment`可用來修改值。</span><span class="sxs-lookup"><span data-stu-id="61e06-201">For example, suppose an interface `ICounter` contains a method `Increment` which can be used to modify a value.</span></span> <span data-ttu-id="61e06-202">如果`ICounter`做為條件約束 」，這是實作`Increment`方法呼叫變數的參考，`Increment`呼叫永遠不會經過 boxing 處理的複本上。</span><span class="sxs-lookup"><span data-stu-id="61e06-202">If `ICounter` is used as a constraint, the implementation of the `Increment` method is called with a reference to the variable that `Increment` was called on, never a boxed copy.</span></span>

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

<span data-ttu-id="61e06-203">第一次呼叫`Increment`變數中的值會修改`x`。</span><span class="sxs-lookup"><span data-stu-id="61e06-203">The first call to `Increment` modifies the value in the variable `x`.</span></span> <span data-ttu-id="61e06-204">這並不等於第二次呼叫`Increment`，這會修改 boxed 複本中的值`x`。</span><span class="sxs-lookup"><span data-stu-id="61e06-204">This is not equivalent to the second call to `Increment`, which modifies the value in a boxed copy of `x`.</span></span> <span data-ttu-id="61e06-205">因此，程式的輸出為：</span><span class="sxs-lookup"><span data-stu-id="61e06-205">Thus, the output of the program is:</span></span>
```
0
1
1
```

<span data-ttu-id="61e06-206">如需 boxing 和 unboxing 的進一步詳細資訊，請參閱 < [Boxing 和 unboxing](types.md#boxing-and-unboxing)。</span><span class="sxs-lookup"><span data-stu-id="61e06-206">For further details on boxing and unboxing, see [Boxing and unboxing](types.md#boxing-and-unboxing).</span></span>

### <a name="meaning-of-this"></a><span data-ttu-id="61e06-207">這個意義</span><span class="sxs-lookup"><span data-stu-id="61e06-207">Meaning of this</span></span>

<span data-ttu-id="61e06-208">執行個體建構函式或類別的執行個體函式成員內`this`會分類為值。</span><span class="sxs-lookup"><span data-stu-id="61e06-208">Within an instance constructor or instance function member of a class, `this` is classified as a value.</span></span> <span data-ttu-id="61e06-209">因此，雖然`this`可用來參考執行個體的函式成員已叫用，就無法將指派給`this`函式成員的類別。</span><span class="sxs-lookup"><span data-stu-id="61e06-209">Thus, while `this` can be used to refer to the instance for which the function member was invoked, it is not possible to assign to `this` in a function member of a class.</span></span>

<span data-ttu-id="61e06-210">在結構中，執行個體建構函式內`this`對應至`out`結構型別的和結構的執行個體函式成員內的參數`this`對應至`ref`結構類型的參數。</span><span class="sxs-lookup"><span data-stu-id="61e06-210">Within an instance constructor of a struct, `this` corresponds to an `out` parameter of the struct type, and within an instance function member of a struct, `this` corresponds to a `ref` parameter of the struct type.</span></span> <span data-ttu-id="61e06-211">在這兩種情況下，`this`歸類為變數，您也可以修改整個結構指派給已叫用函式成員`this`或藉由傳遞為`ref`或`out`參數。</span><span class="sxs-lookup"><span data-stu-id="61e06-211">In both cases, `this` is classified as a variable, and it is possible to modify the entire struct for which the function member was invoked by assigning to `this` or by passing this as a `ref` or `out` parameter.</span></span>

### <a name="field-initializers"></a><span data-ttu-id="61e06-212">欄位初始設定式</span><span class="sxs-lookup"><span data-stu-id="61e06-212">Field initializers</span></span>

<span data-ttu-id="61e06-213">中所述[預設值](structs.md#default-values)，將所有實值型別欄位設定為其預設值和所有參考型別欄位為產生的值包含結構的預設值`null`。</span><span class="sxs-lookup"><span data-stu-id="61e06-213">As described in [Default values](structs.md#default-values), the default value of a struct consists of the value that results from setting all value type fields to their default value and all reference type fields to `null`.</span></span> <span data-ttu-id="61e06-214">基於這個理由，結構不允許包含變數的初始設定式的執行個體欄位宣告。</span><span class="sxs-lookup"><span data-stu-id="61e06-214">For this reason, a struct does not permit instance field declarations to include variable initializers.</span></span> <span data-ttu-id="61e06-215">這項限制只適用於執行個體欄位。</span><span class="sxs-lookup"><span data-stu-id="61e06-215">This restriction applies only to instance fields.</span></span> <span data-ttu-id="61e06-216">結構的靜態欄位可以包含變數的初始設定式。</span><span class="sxs-lookup"><span data-stu-id="61e06-216">Static fields of a struct are permitted to include variable initializers.</span></span>

<span data-ttu-id="61e06-217">此範例</span><span class="sxs-lookup"><span data-stu-id="61e06-217">The example</span></span>
```csharp
struct Point
{
    public int x = 1;  // Error, initializer not permitted
    public int y = 1;  // Error, initializer not permitted
}
```
<span data-ttu-id="61e06-218">處於錯誤，因為執行個體欄位宣告包含變數的初始設定式。</span><span class="sxs-lookup"><span data-stu-id="61e06-218">is in error because the instance field declarations include variable initializers.</span></span>

### <a name="constructors"></a><span data-ttu-id="61e06-219">建構函式</span><span class="sxs-lookup"><span data-stu-id="61e06-219">Constructors</span></span>

<span data-ttu-id="61e06-220">不同於類別中，結構不是允許宣告無參數的執行個體建構函式。</span><span class="sxs-lookup"><span data-stu-id="61e06-220">Unlike a class, a struct is not permitted to declare a parameterless instance constructor.</span></span> <span data-ttu-id="61e06-221">相反地，每個結構會隱含地具有無參數的執行個體建構函式一律會傳回所產生將所有實值型別欄位設定為其預設值和所有參考為 null 的型別欄位的值 ([預設建構函式](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="61e06-221">Instead, every struct implicitly has a parameterless instance constructor which always returns the value that results from setting all value type fields to their default value and all reference type fields to null ([Default constructors](types.md#default-constructors)).</span></span> <span data-ttu-id="61e06-222">結構可以宣告具有參數的執行個體建構函式。</span><span class="sxs-lookup"><span data-stu-id="61e06-222">A struct can declare instance constructors having parameters.</span></span> <span data-ttu-id="61e06-223">例如</span><span class="sxs-lookup"><span data-stu-id="61e06-223">For example</span></span>
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

<span data-ttu-id="61e06-224">指定上述宣告中，陳述式</span><span class="sxs-lookup"><span data-stu-id="61e06-224">Given the above declaration, the statements</span></span>
```csharp
Point p1 = new Point();
Point p2 = new Point(0, 0);
```
<span data-ttu-id="61e06-225">兩者皆可建立`Point`具有`x`和`y`初始化為零。</span><span class="sxs-lookup"><span data-stu-id="61e06-225">both create a `Point` with `x` and `y` initialized to zero.</span></span>

<span data-ttu-id="61e06-226">結構執行個體建構函式不允許包含表單的建構函式初始設定式`base(...)`。</span><span class="sxs-lookup"><span data-stu-id="61e06-226">A struct instance constructor is not permitted to include a constructor initializer of the form `base(...)`.</span></span>

<span data-ttu-id="61e06-227">如果結構執行個體建構函式未指定的建構函式初始設定式中，`this`變數對應至`out`結構型別的和類似的參數`out`參數，`this`必須明確地指派 （[明確指派](variables.md#definite-assignment)) 在其中建構函式會傳回每個位置。</span><span class="sxs-lookup"><span data-stu-id="61e06-227">If the struct instance constructor doesn't specify a constructor initializer, the `this` variable corresponds to an `out` parameter of the struct type, and similar to an `out` parameter, `this` must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) at every location where the constructor returns.</span></span> <span data-ttu-id="61e06-228">如果結構執行個體建構函式指定的建構函式初始設定式中，`this`變數對應至`ref`結構型別的和類似的參數`ref`參數，`this`會被視為已明確指派上要建構函式主體的項目。</span><span class="sxs-lookup"><span data-stu-id="61e06-228">If the struct instance constructor specifies a constructor initializer, the `this` variable corresponds to a `ref` parameter of the struct type, and similar to a `ref` parameter, `this` is considered definitely assigned on entry to the constructor body.</span></span> <span data-ttu-id="61e06-229">請考慮以下的執行個體建構函式實作：</span><span class="sxs-lookup"><span data-stu-id="61e06-229">Consider the instance constructor implementation below:</span></span>
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

<span data-ttu-id="61e06-230">任何執行個體成員函式 (包括屬性 set 存取子`X`和`Y`) 可以呼叫，直到明確指派所建構之結構的所有欄位。</span><span class="sxs-lookup"><span data-stu-id="61e06-230">No instance member function (including the set accessors for the properties `X` and `Y`) can be called until all fields of the struct being constructed have been definitely assigned.</span></span> <span data-ttu-id="61e06-231">唯一的例外狀況包含自動實作的屬性 ([自動實作屬性](classes.md#automatically-implemented-properties))。</span><span class="sxs-lookup"><span data-stu-id="61e06-231">The only exception involves automatically implemented properties ([Automatically implemented properties](classes.md#automatically-implemented-properties)).</span></span> <span data-ttu-id="61e06-232">明確指派規則 ([簡單的指派運算式](variables.md#simple-assignment-expressions)) 特別排除指派 auto 屬性的結構類型的執行個體建構函式內，該結構類型： 此類指派會被視為明確auto 屬性的隱藏的支援欄位的指派。</span><span class="sxs-lookup"><span data-stu-id="61e06-232">The definite assignment rules ([Simple assignment expressions](variables.md#simple-assignment-expressions)) specifically exempt assignment to an auto-property of a struct type within an instance constructor of that struct type: such an assignment is considered a definite assignment of the hidden backing field of the auto-property.</span></span> <span data-ttu-id="61e06-233">因此，以下被允許的：</span><span class="sxs-lookup"><span data-stu-id="61e06-233">Thus, the following is allowed:</span></span>

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

### <a name="destructors"></a><span data-ttu-id="61e06-234">解構函式</span><span class="sxs-lookup"><span data-stu-id="61e06-234">Destructors</span></span>

<span data-ttu-id="61e06-235">結構不是允許宣告解構函式。</span><span class="sxs-lookup"><span data-stu-id="61e06-235">A struct is not permitted to declare a destructor.</span></span>

### <a name="static-constructors"></a><span data-ttu-id="61e06-236">靜態建構函式</span><span class="sxs-lookup"><span data-stu-id="61e06-236">Static constructors</span></span>

<span data-ttu-id="61e06-237">結構的靜態建構函式會依照大多與類別相同的規則。</span><span class="sxs-lookup"><span data-stu-id="61e06-237">Static constructors for structs follow most of the same rules as for classes.</span></span> <span data-ttu-id="61e06-238">結構類型的靜態建構函式的執行是由應用程式定義域中發生下列事件的第一個觸發：</span><span class="sxs-lookup"><span data-stu-id="61e06-238">The execution of a static constructor for a struct type is triggered by the first of the following events to occur within an application domain:</span></span>

*  <span data-ttu-id="61e06-239">參考的結構類型的靜態成員。</span><span class="sxs-lookup"><span data-stu-id="61e06-239">A static member of the struct type is referenced.</span></span>
*  <span data-ttu-id="61e06-240">結構類型的明確宣告建構函式會呼叫。</span><span class="sxs-lookup"><span data-stu-id="61e06-240">An explicitly declared constructor of the struct type is called.</span></span>

<span data-ttu-id="61e06-241">建立的預設值 ([預設值](structs.md#default-values)) 的結構類型不會觸發靜態建構函式。</span><span class="sxs-lookup"><span data-stu-id="61e06-241">The creation of default values ([Default values](structs.md#default-values)) of struct types does not trigger the static constructor.</span></span> <span data-ttu-id="61e06-242">（這個範例是陣列中元素的初始值）。</span><span class="sxs-lookup"><span data-stu-id="61e06-242">(An example of this is the initial value of elements in an array.)</span></span>

## <a name="struct-examples"></a><span data-ttu-id="61e06-243">結構範例</span><span class="sxs-lookup"><span data-stu-id="61e06-243">Struct examples</span></span>

<span data-ttu-id="61e06-244">下圖顯示兩個重要的使用範例`struct`型別，以建立可以用來預先定義的型別語言，但具有已修改的語意類似的類型。</span><span class="sxs-lookup"><span data-stu-id="61e06-244">The following shows two significant examples of using `struct` types to create types that can be used similarly to the predefined types of the language, but with modified semantics.</span></span>

### <a name="database-integer-type"></a><span data-ttu-id="61e06-245">資料庫的整數型別</span><span class="sxs-lookup"><span data-stu-id="61e06-245">Database integer type</span></span>

<span data-ttu-id="61e06-246">`DBInt`結構的實作，可代表一組完整的值的整數類型`int`型別，再加上額外的狀態，指出未知的值。</span><span class="sxs-lookup"><span data-stu-id="61e06-246">The `DBInt` struct below implements an integer type that can represent the complete set of values of the `int` type, plus an additional state that indicates an unknown value.</span></span> <span data-ttu-id="61e06-247">具有這些特性的型別常用的資料庫。</span><span class="sxs-lookup"><span data-stu-id="61e06-247">A type with these characteristics is commonly used in databases.</span></span>

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

### <a name="database-boolean-type"></a><span data-ttu-id="61e06-248">資料庫布林型別</span><span class="sxs-lookup"><span data-stu-id="61e06-248">Database boolean type</span></span>

<span data-ttu-id="61e06-249">`DBBool`結構實作的三種值的邏輯類型。</span><span class="sxs-lookup"><span data-stu-id="61e06-249">The `DBBool` struct below implements a three-valued logical type.</span></span> <span data-ttu-id="61e06-250">可能的值，這個型別的`DBBool.True`， `DBBool.False`，並`DBBool.Null`，其中`Null`成員表示未知的值。</span><span class="sxs-lookup"><span data-stu-id="61e06-250">The possible values of this type are `DBBool.True`, `DBBool.False`, and `DBBool.Null`, where the `Null` member indicates an unknown value.</span></span> <span data-ttu-id="61e06-251">這種三值邏輯的類型通常會用於資料庫。</span><span class="sxs-lookup"><span data-stu-id="61e06-251">Such three-valued logical types are commonly used in databases.</span></span>

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
