# <a name="types"></a><span data-ttu-id="25483-101">型別</span><span class="sxs-lookup"><span data-stu-id="25483-101">Types</span></span>

<span data-ttu-id="25483-102">C# 語言的類型可分為兩種主要類別：***實值型別***並***參考型別***。</span><span class="sxs-lookup"><span data-stu-id="25483-102">The types of the C# language are divided into two main categories: ***value types*** and ***reference types***.</span></span> <span data-ttu-id="25483-103">可能是實值型別和參考型別***泛型型別***，這需要一或多個***型別參數***。</span><span class="sxs-lookup"><span data-stu-id="25483-103">Both value types and reference types may be ***generic types***, which take one or more ***type parameters***.</span></span> <span data-ttu-id="25483-104">類型參數可以指定這兩個實值型別和參考型別。</span><span class="sxs-lookup"><span data-stu-id="25483-104">Type parameters can designate both value types and reference types.</span></span>

```antlr
type
    : value_type
    | reference_type
    | type_parameter
    | type_unsafe
    ;
```

<span data-ttu-id="25483-105">最後一類的類型，指標是只能用於 unsafe 程式碼。</span><span class="sxs-lookup"><span data-stu-id="25483-105">The final category of types, pointers, is available only in unsafe code.</span></span> <span data-ttu-id="25483-106">這個討論中進一步[指標類型](unsafe-code.md#pointer-types)。</span><span class="sxs-lookup"><span data-stu-id="25483-106">This is discussed further in [Pointer types](unsafe-code.md#pointer-types).</span></span>

<span data-ttu-id="25483-107">實值型別不同於參考類型，因為實值型別的變數直接包含其資料，而參考的變數類型的存放區***參考***其資料，後者即***物件***.</span><span class="sxs-lookup"><span data-stu-id="25483-107">Value types differ from reference types in that variables of the value types directly contain their data, whereas variables of the reference types store ***references*** to their data, the latter being known as ***objects***.</span></span> <span data-ttu-id="25483-108">參考型別，它是兩種變數可以參考相同的物件，並因此可能會影響其他變數所參考的物件的一個變數上進行的作業。</span><span class="sxs-lookup"><span data-stu-id="25483-108">With reference types, it is possible for two variables to reference the same object, and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="25483-109">具有實值類型，每個變數都有自己一份資料，而且不可能會影響其他的其中一個上的作業。</span><span class="sxs-lookup"><span data-stu-id="25483-109">With value types, the variables each have their own copy of the data, and it is not possible for operations on one to affect the other.</span></span>

<span data-ttu-id="25483-110">C# 類型系統統一，任何類型的值可視為物件。</span><span class="sxs-lookup"><span data-stu-id="25483-110">C#'s type system is unified such that a value of any type can be treated as an object.</span></span> <span data-ttu-id="25483-111">C# 中的每個型別都直接或間接衍生自 `object` 類別型別，而 `object` 是所有型別的基底類別。</span><span class="sxs-lookup"><span data-stu-id="25483-111">Every type in C# directly or indirectly derives from the `object` class type, and `object` is the ultimate base class of all types.</span></span> <span data-ttu-id="25483-112">參考型別的值之所以會視為物件，只是將這些值當作 `object` 型別來檢視。</span><span class="sxs-lookup"><span data-stu-id="25483-112">Values of reference types are treated as objects simply by viewing the values as type `object`.</span></span> <span data-ttu-id="25483-113">實值類型的值時，會被當作物件上，執行 boxing 和 unboxing 作業 ([Boxing 和 unboxing](types.md#boxing-and-unboxing))。</span><span class="sxs-lookup"><span data-stu-id="25483-113">Values of value types are treated as objects by performing boxing and unboxing operations ([Boxing and unboxing](types.md#boxing-and-unboxing)).</span></span>

## <a name="value-types"></a><span data-ttu-id="25483-114">值類型</span><span class="sxs-lookup"><span data-stu-id="25483-114">Value types</span></span>

<span data-ttu-id="25483-115">實值型別是結構類型或列舉類型。</span><span class="sxs-lookup"><span data-stu-id="25483-115">A value type is either a struct type or an enumeration type.</span></span> <span data-ttu-id="25483-116">C# 提供一組預先定義的結構類型稱為***簡單型別***。</span><span class="sxs-lookup"><span data-stu-id="25483-116">C# provides a set of predefined struct types called the ***simple types***.</span></span> <span data-ttu-id="25483-117">簡單型別是透過保留字識別。</span><span class="sxs-lookup"><span data-stu-id="25483-117">The simple types are identified through reserved words.</span></span>

```antlr
value_type
    : struct_type
    | enum_type
    ;

struct_type
    : type_name
    | simple_type
    | nullable_type
    ;

simple_type
    : numeric_type
    | 'bool'
    ;

numeric_type
    : integral_type
    | floating_point_type
    | 'decimal'
    ;

integral_type
    : 'sbyte'
    | 'byte'
    | 'short'
    | 'ushort'
    | 'int'
    | 'uint'
    | 'long'
    | 'ulong'
    | 'char'
    ;

floating_point_type
    : 'float'
    | 'double'
    ;

nullable_type
    : non_nullable_value_type '?'
    ;

non_nullable_value_type
    : type
    ;

enum_type
    : type_name
    ;
```

<span data-ttu-id="25483-118">不同於參考類型的變數，實值型別的變數可以包含值`null`只有實值型別是可為 null 的型別。</span><span class="sxs-lookup"><span data-stu-id="25483-118">Unlike a variable of a reference type, a variable of a value type can contain the value `null` only if the value type is a nullable type.</span></span>  <span data-ttu-id="25483-119">每個不可為 null 的實值型別沒有對應的可為 null 的實值型別，用來表示相同的值加上值集`null`。</span><span class="sxs-lookup"><span data-stu-id="25483-119">For every non-nullable value type there is a corresponding nullable value type denoting the same set of values plus the value `null`.</span></span>

<span data-ttu-id="25483-120">指派給變數的實值型別會建立一份所指派的值。</span><span class="sxs-lookup"><span data-stu-id="25483-120">Assignment to a variable of a value type creates a copy of the value being assigned.</span></span> <span data-ttu-id="25483-121">這不同於指派給變數的參考型別，它將會複製參考，而不是參考所識別的物件。</span><span class="sxs-lookup"><span data-stu-id="25483-121">This differs from assignment to a variable of a reference type, which copies the reference but not the object identified by the reference.</span></span>

### <a name="the-systemvaluetype-type"></a><span data-ttu-id="25483-122">System.ValueType 類型</span><span class="sxs-lookup"><span data-stu-id="25483-122">The System.ValueType type</span></span>

<span data-ttu-id="25483-123">所有實值型別都隱含地繼承自類別`System.ValueType`，它會接著繼承自類別`object`。</span><span class="sxs-lookup"><span data-stu-id="25483-123">All value types implicitly inherit from the class `System.ValueType`, which, in turn, inherits from class `object`.</span></span> <span data-ttu-id="25483-124">不可能從實值型別衍生任何型別和實值型別都隱含密封 ([密封類別](classes.md#sealed-classes))。</span><span class="sxs-lookup"><span data-stu-id="25483-124">It is not possible for any type to derive from a value type, and value types are thus implicitly sealed ([Sealed classes](classes.md#sealed-classes)).</span></span>

<span data-ttu-id="25483-125">請注意，`System.ValueType`本身並不會*value_type*。</span><span class="sxs-lookup"><span data-stu-id="25483-125">Note that `System.ValueType` is not itself a *value_type*.</span></span> <span data-ttu-id="25483-126">而是*class_type*所有*value_type*s 自動衍生。</span><span class="sxs-lookup"><span data-stu-id="25483-126">Rather, it is a *class_type* from which all *value_type*s are automatically derived.</span></span>

### <a name="default-constructors"></a><span data-ttu-id="25483-127">預設建構函式</span><span class="sxs-lookup"><span data-stu-id="25483-127">Default constructors</span></span>

<span data-ttu-id="25483-128">所有實值型別會隱含地宣告公用的無參數執行個體建構函式呼叫***預設建構函式***。</span><span class="sxs-lookup"><span data-stu-id="25483-128">All value types implicitly declare a public parameterless instance constructor called the ***default constructor***.</span></span> <span data-ttu-id="25483-129">預設建構函式會傳回零初始化執行個體稱為***預設值***實值型別：</span><span class="sxs-lookup"><span data-stu-id="25483-129">The default constructor returns a zero-initialized instance known as the ***default value*** for the value type:</span></span>

*  <span data-ttu-id="25483-130">針對所有*simple_type*s，預設值是全部為零的位元模式所產生的值：</span><span class="sxs-lookup"><span data-stu-id="25483-130">For all *simple_type*s, the default value is the value produced by a bit pattern of all zeros:</span></span>
    * <span data-ttu-id="25483-131">針對`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`，並`ulong`，預設值是`0`。</span><span class="sxs-lookup"><span data-stu-id="25483-131">For `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, and `ulong`, the default value is `0`.</span></span>
    * <span data-ttu-id="25483-132">針對`char`，預設值是`'\x0000'`。</span><span class="sxs-lookup"><span data-stu-id="25483-132">For `char`, the default value is `'\x0000'`.</span></span>
    * <span data-ttu-id="25483-133">針對`float`，預設值是`0.0f`。</span><span class="sxs-lookup"><span data-stu-id="25483-133">For `float`, the default value is `0.0f`.</span></span>
    * <span data-ttu-id="25483-134">針對`double`，預設值是`0.0d`。</span><span class="sxs-lookup"><span data-stu-id="25483-134">For `double`, the default value is `0.0d`.</span></span>
    * <span data-ttu-id="25483-135">針對`decimal`，預設值是`0.0m`。</span><span class="sxs-lookup"><span data-stu-id="25483-135">For `decimal`, the default value is `0.0m`.</span></span>
    * <span data-ttu-id="25483-136">針對`bool`，預設值是`false`。</span><span class="sxs-lookup"><span data-stu-id="25483-136">For `bool`, the default value is `false`.</span></span>
*  <span data-ttu-id="25483-137">針對*enum_type* `E`，預設值是`0`轉換成的型別、 `E`。</span><span class="sxs-lookup"><span data-stu-id="25483-137">For an *enum_type* `E`, the default value is `0`, converted to the type `E`.</span></span>
*  <span data-ttu-id="25483-138">針對*struct_type*，預設值是藉由將所有實值型別欄位設定為其預設值和所有參考型別欄位為產生的值`null`。</span><span class="sxs-lookup"><span data-stu-id="25483-138">For a *struct_type*, the default value is the value produced by setting all value type fields to their default value and all reference type fields to `null`.</span></span>
*  <span data-ttu-id="25483-139">針對*nullable_type*的預設值是為其執行個體`HasValue`屬性為 false，`Value`未定義屬性。</span><span class="sxs-lookup"><span data-stu-id="25483-139">For a *nullable_type* the default value is an instance for which the `HasValue` property is false and the `Value` property is undefined.</span></span> <span data-ttu-id="25483-140">預設值就是所謂***hodnota***可為 null 的型別。</span><span class="sxs-lookup"><span data-stu-id="25483-140">The default value is also known as the ***null value*** of the nullable type.</span></span>

<span data-ttu-id="25483-141">任何其他執行個體建構函式，例如實值類型的預設建構函式會叫用使用`new`運算子。</span><span class="sxs-lookup"><span data-stu-id="25483-141">Like any other instance constructor, the default constructor of a value type is invoked using the `new` operator.</span></span> <span data-ttu-id="25483-142">基於效率考量，這項需求不是實際擁有 產生建構函式呼叫的實作。</span><span class="sxs-lookup"><span data-stu-id="25483-142">For efficiency reasons, this requirement is not intended to actually have the implementation generate a constructor call.</span></span> <span data-ttu-id="25483-143">在下列範例中，變數`i`和`j`都初始化為零。</span><span class="sxs-lookup"><span data-stu-id="25483-143">In the example below, variables `i` and `j` are both initialized to zero.</span></span>

```csharp
class A
{
    void F() {
        int i = 0;
        int j = new int();
    }
}
```

<span data-ttu-id="25483-144">每個實值型別以隱含方式具有公用無參數的執行個體建構函式，因為它不可能包含明確無參數建構函式宣告為結構類型。</span><span class="sxs-lookup"><span data-stu-id="25483-144">Because every value type implicitly has a public parameterless instance constructor, it is not possible for a struct type to contain an explicit declaration of a parameterless constructor.</span></span> <span data-ttu-id="25483-145">不過允許宣告參數化的執行個體建構函式的結構類型 ([建構函式](structs.md#constructors))。</span><span class="sxs-lookup"><span data-stu-id="25483-145">A struct type is however permitted to declare parameterized instance constructors ([Constructors](structs.md#constructors)).</span></span>

### <a name="struct-types"></a><span data-ttu-id="25483-146">結構型別</span><span class="sxs-lookup"><span data-stu-id="25483-146">Struct types</span></span>

<span data-ttu-id="25483-147">結構類型是常數、 欄位、 方法、 屬性、 索引子、 運算子、 執行個體建構函式、 靜態的建構函式和巢狀型別可以宣告實值型別。</span><span class="sxs-lookup"><span data-stu-id="25483-147">A struct type is a value type that can declare constants, fields, methods, properties, indexers, operators, instance constructors, static constructors, and nested types.</span></span> <span data-ttu-id="25483-148">建構型別的宣告述[結構宣告](structs.md#struct-declarations)。</span><span class="sxs-lookup"><span data-stu-id="25483-148">The declaration of struct types is described in [Struct declarations](structs.md#struct-declarations).</span></span>

### <a name="simple-types"></a><span data-ttu-id="25483-149">簡單型別</span><span class="sxs-lookup"><span data-stu-id="25483-149">Simple types</span></span>

<span data-ttu-id="25483-150">C# 提供一組預先定義的結構類型稱為***簡單型別***。</span><span class="sxs-lookup"><span data-stu-id="25483-150">C# provides a set of predefined struct types called the ***simple types***.</span></span> <span data-ttu-id="25483-151">簡單型別透過保留字，但這些保留的字是只在預先定義的結構類型的別名`System`命名空間，在下表中所述。</span><span class="sxs-lookup"><span data-stu-id="25483-151">The simple types are identified through reserved words, but these reserved words are simply aliases for predefined struct types in the `System` namespace, as described in the table below.</span></span>


| <span data-ttu-id="25483-152">__保留的字__</span><span class="sxs-lookup"><span data-stu-id="25483-152">__Reserved word__</span></span> | <span data-ttu-id="25483-153">__別名類型__</span><span class="sxs-lookup"><span data-stu-id="25483-153">__Aliased type__</span></span> |
|-------------------|------------------|
| `sbyte`           | `System.SByte`   | 
| `byte`            | `System.Byte`    | 
| `short`           | `System.Int16`   | 
| `ushort`          | `System.UInt16`  | 
| `int`             | `System.Int32`   | 
| `uint`            | `System.UInt32`  | 
| `long`            | `System.Int64`   | 
| `ulong`           | `System.UInt64`  | 
| `char`            | `System.Char`    | 
| `float`           | `System.Single`  | 
| `double`          | `System.Double`  | 
| `bool`            | `System.Boolean` | 
| `decimal`         | `System.Decimal` | 

<span data-ttu-id="25483-154">簡單類型別名的結構類型，因為每個簡單的型別具有成員。</span><span class="sxs-lookup"><span data-stu-id="25483-154">Because a simple type aliases a struct type, every simple type has members.</span></span> <span data-ttu-id="25483-155">比方說，`int`已宣告中的成員`System.Int32`及成員繼承自`System.Object`，並允許下列陳述式：</span><span class="sxs-lookup"><span data-stu-id="25483-155">For example, `int` has the members declared in `System.Int32` and the members inherited from `System.Object`, and the following statements are permitted:</span></span>

```csharp
int i = int.MaxValue;           // System.Int32.MaxValue constant
string s = i.ToString();        // System.Int32.ToString() instance method
string t = 123.ToString();      // System.Int32.ToString() instance method
```

<span data-ttu-id="25483-156">簡單型別與其他結構型別的差異在於它們可允許某些額外的作業：</span><span class="sxs-lookup"><span data-stu-id="25483-156">The simple types differ from other struct types in that they permit certain additional operations:</span></span>

*  <span data-ttu-id="25483-157">大部分的簡單型別允許值，以建立撰寫*常值*([常值](lexical-structure.md#literals))。</span><span class="sxs-lookup"><span data-stu-id="25483-157">Most simple types permit values to be created by writing *literals* ([Literals](lexical-structure.md#literals)).</span></span> <span data-ttu-id="25483-158">比方說，`123`是常值型別的`int`並`'a'`是類型的常值`char`。</span><span class="sxs-lookup"><span data-stu-id="25483-158">For example, `123` is a literal of type `int` and `'a'` is a literal of type `char`.</span></span> <span data-ttu-id="25483-159">C# 一般情況下，對結構類型的常值沒有佈建，而透過這些結構類型的執行個體建構函式最終一律建立其他的結構類型的非預設值。</span><span class="sxs-lookup"><span data-stu-id="25483-159">C# makes no provision for literals of struct types in general, and non-default values of other struct types are ultimately always created through instance constructors of those struct types.</span></span>
*  <span data-ttu-id="25483-160">簡單類型常數運算式的運算元時，就可以評估在編譯時間運算式編譯器。</span><span class="sxs-lookup"><span data-stu-id="25483-160">When the operands of an expression are all simple type constants, it is possible for the compiler to evaluate the expression at compile-time.</span></span> <span data-ttu-id="25483-161">這類運算式就所謂*constant_expression* ([常數運算式](expressions.md#constant-expressions))。</span><span class="sxs-lookup"><span data-stu-id="25483-161">Such an expression is known as a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)).</span></span> <span data-ttu-id="25483-162">定義的其他型別運算子的運算式不會被視為常數運算式。</span><span class="sxs-lookup"><span data-stu-id="25483-162">Expressions involving operators defined by other struct types are not considered to be constant expressions.</span></span>
*  <span data-ttu-id="25483-163">透過`const`宣告就可以宣告簡單類型的常數 ([常數](classes.md#constants))。</span><span class="sxs-lookup"><span data-stu-id="25483-163">Through `const` declarations it is possible to declare constants of the simple types ([Constants](classes.md#constants)).</span></span> <span data-ttu-id="25483-164">不可以有常數的其他型別，但會提供類似的效果`static readonly`欄位。</span><span class="sxs-lookup"><span data-stu-id="25483-164">It is not possible to have constants of other struct types, but a similar effect is provided by `static readonly` fields.</span></span>
*  <span data-ttu-id="25483-165">包含簡單的型別轉換可以參與其他型別，所定義的轉換運算子的評估，但使用者定義轉換運算子不能參與另一個使用者定義運算子的評估結果 ([評估使用者定義轉換](conversions.md#evaluation-of-user-defined-conversions))。</span><span class="sxs-lookup"><span data-stu-id="25483-165">Conversions involving simple types can participate in evaluation of conversion operators defined by other struct types, but a user-defined conversion operator can never participate in evaluation of another user-defined operator ([Evaluation of user-defined conversions](conversions.md#evaluation-of-user-defined-conversions)).</span></span>

### <a name="integral-types"></a><span data-ttu-id="25483-166">整數類資料型別</span><span class="sxs-lookup"><span data-stu-id="25483-166">Integral types</span></span>

<span data-ttu-id="25483-167">C# 支援九個整數類資料類型： `sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`，和`char`。</span><span class="sxs-lookup"><span data-stu-id="25483-167">C# supports nine integral types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, and `char`.</span></span> <span data-ttu-id="25483-168">整數類資料類型會有下列的大小和範圍的值：</span><span class="sxs-lookup"><span data-stu-id="25483-168">The integral types have the following sizes and ranges of values:</span></span>

*  <span data-ttu-id="25483-169">`sbyte`型別代表帶正負號的 8 位元整數，其值介於-128 到 127 之間。</span><span class="sxs-lookup"><span data-stu-id="25483-169">The `sbyte` type represents signed 8-bit integers with values between -128 and 127.</span></span>
*  <span data-ttu-id="25483-170">`byte`型別代表不帶正負號的 8 位元整數，其值介於 0 和 255 之間。</span><span class="sxs-lookup"><span data-stu-id="25483-170">The `byte` type represents unsigned 8-bit integers with values between 0 and 255.</span></span>
*  <span data-ttu-id="25483-171">`short`型別代表帶正負號的 16 位元整數，其值介於-32768 和 32767 之間。</span><span class="sxs-lookup"><span data-stu-id="25483-171">The `short` type represents signed 16-bit integers with values between -32768 and 32767.</span></span>
*  <span data-ttu-id="25483-172">`ushort`型別代表不帶正負號的 16 位元整數，其值介於 0 到 65535 之間。</span><span class="sxs-lookup"><span data-stu-id="25483-172">The `ushort` type represents unsigned 16-bit integers with values between 0 and 65535.</span></span>
*  <span data-ttu-id="25483-173">`int`型別代表帶正負號的 32 位元整數，其值介於-2147483648 到 2147483647 之間。</span><span class="sxs-lookup"><span data-stu-id="25483-173">The `int` type represents signed 32-bit integers with values between -2147483648 and 2147483647.</span></span>
*  <span data-ttu-id="25483-174">`uint`型別代表不帶正負號的 32 位元整數，其值介於 0 和 4294967295 之間。</span><span class="sxs-lookup"><span data-stu-id="25483-174">The `uint` type represents unsigned 32-bit integers with values between 0 and 4294967295.</span></span>
*  <span data-ttu-id="25483-175">`long`型別代表帶正負號的 64 位元整數，其值介於-9223372036854775808 和 9223372036854775807。</span><span class="sxs-lookup"><span data-stu-id="25483-175">The `long` type represents signed 64-bit integers with values between -9223372036854775808 and 9223372036854775807.</span></span>
*  <span data-ttu-id="25483-176">`ulong`型別代表不帶正負號的 64 位元整數，其值介於 0 和 18446744073709551615 之間。</span><span class="sxs-lookup"><span data-stu-id="25483-176">The `ulong` type represents unsigned 64-bit integers with values between 0 and 18446744073709551615.</span></span>
*  <span data-ttu-id="25483-177">`char`型別代表不帶正負號的 16 位元整數，其值介於 0 到 65535 之間。</span><span class="sxs-lookup"><span data-stu-id="25483-177">The `char` type represents unsigned 16-bit integers with values between 0 and 65535.</span></span> <span data-ttu-id="25483-178">可能的值為一組`char`類型會對應至 Unicode 字元集。</span><span class="sxs-lookup"><span data-stu-id="25483-178">The set of possible values for the `char` type corresponds to the Unicode character set.</span></span> <span data-ttu-id="25483-179">雖然`char`具有相同的表示法`ushort`，並非所有允許一種類型的作業可以在其他。</span><span class="sxs-lookup"><span data-stu-id="25483-179">Although `char` has the same representation as `ushort`, not all operations permitted on one type are permitted on the other.</span></span>

<span data-ttu-id="25483-180">整數型別一元和二元運算子一律操作與帶正負號的 32 位元精確度、 不帶正負號的 32 位元精確度、 帶正負號的 64 位元精確度或不帶正負號的 64 位元精確度：</span><span class="sxs-lookup"><span data-stu-id="25483-180">The integral-type unary and binary operators always operate with signed 32-bit precision, unsigned 32-bit precision, signed 64-bit precision, or unsigned 64-bit precision:</span></span>

*  <span data-ttu-id="25483-181">一元`+`並`~`運算子，運算元會轉換成類型`T`，其中`T`的正是第一`int`， `uint`， `long`，和`ulong`可完全代表所有可能的運算元的值。</span><span class="sxs-lookup"><span data-stu-id="25483-181">For the unary `+` and `~` operators, the operand is converted to type `T`, where `T` is the first of `int`, `uint`, `long`, and `ulong` that can fully represent all possible values of the operand.</span></span> <span data-ttu-id="25483-182">使用型別的精確度再執行作業`T`，且結果類型是`T`。</span><span class="sxs-lookup"><span data-stu-id="25483-182">The operation is then performed using the precision of type `T`, and the type of the result is `T`.</span></span>
*  <span data-ttu-id="25483-183">一元`-`運算子，運算元會轉換成類型`T`，其中`T`是第一封`int`和`long`可完全代表運算元的所有可能的值。</span><span class="sxs-lookup"><span data-stu-id="25483-183">For the unary `-` operator, the operand is converted to type `T`, where `T` is the first of `int` and `long` that can fully represent all possible values of the operand.</span></span> <span data-ttu-id="25483-184">使用型別的精確度再執行作業`T`，且結果類型是`T`。</span><span class="sxs-lookup"><span data-stu-id="25483-184">The operation is then performed using the precision of type `T`, and the type of the result is `T`.</span></span> <span data-ttu-id="25483-185">一元`-`無法將運算子套用至類型的運算元`ulong`。</span><span class="sxs-lookup"><span data-stu-id="25483-185">The unary `-` operator cannot be applied to operands of type `ulong`.</span></span>
*  <span data-ttu-id="25483-186">二進位檔`+`， `-`， `*`， `/`， `%`， `&`， `^`， `|`， `==`， `!=`， `>`， `<`， `>=`，並`<=`運算子，運算元會轉換為類型`T`，其中`T`的正是第一`int`， `uint`， `long`，和`ulong`可完全代表所有可能這兩個運算元的值。</span><span class="sxs-lookup"><span data-stu-id="25483-186">For the binary `+`, `-`, `*`, `/`, `%`, `&`, `^`, `|`, `==`, `!=`, `>`, `<`, `>=`, and `<=` operators, the operands are converted to type `T`, where `T` is the first of `int`, `uint`, `long`, and `ulong` that can fully represent all possible values of both operands.</span></span> <span data-ttu-id="25483-187">使用型別的精確度再執行作業`T`，且結果類型是`T`(或`bool`關聯式運算子)。</span><span class="sxs-lookup"><span data-stu-id="25483-187">The operation is then performed using the precision of type `T`, and the type of the result is `T` (or `bool` for the relational operators).</span></span> <span data-ttu-id="25483-188">它不允許有一個運算元必須是類型`long`，另一個則為型別`ulong`具有二元運算子。</span><span class="sxs-lookup"><span data-stu-id="25483-188">It is not permitted for one operand to be of type `long` and the other to be of type `ulong` with the binary operators.</span></span>
*  <span data-ttu-id="25483-189">二進位檔`<<`並`>>`運算子的左運算元轉換為類型`T`，其中`T`的正是第一`int`， `uint`， `long`，和`ulong`可完全代表所有可能的運算元的值。</span><span class="sxs-lookup"><span data-stu-id="25483-189">For the binary `<<` and `>>` operators, the left operand is converted to type `T`, where `T` is the first of `int`, `uint`, `long`, and `ulong` that can fully represent all possible values of the operand.</span></span> <span data-ttu-id="25483-190">使用型別的精確度再執行作業`T`，且結果類型是`T`。</span><span class="sxs-lookup"><span data-stu-id="25483-190">The operation is then performed using the precision of type `T`, and the type of the result is `T`.</span></span>

<span data-ttu-id="25483-191">`char`型別被歸類為整數型別，但不同於其他整數類資料類型有兩種：</span><span class="sxs-lookup"><span data-stu-id="25483-191">The `char` type is classified as an integral type, but it differs from the other integral types in two ways:</span></span>

*  <span data-ttu-id="25483-192">從其他類型沒有隱含轉換`char`型別。</span><span class="sxs-lookup"><span data-stu-id="25483-192">There are no implicit conversions from other types to the `char` type.</span></span> <span data-ttu-id="25483-193">特別的是，即使`sbyte`， `byte`，和`ushort`型別有的是完全可使用的值範圍`char`輸入時，隱含的轉換，從`sbyte`， `byte`，或`ushort`至`char`不存在。</span><span class="sxs-lookup"><span data-stu-id="25483-193">In particular, even though the `sbyte`, `byte`, and `ushort` types have ranges of values that are fully representable using the `char` type, implicit conversions from `sbyte`, `byte`, or `ushort` to `char` do not exist.</span></span>
*  <span data-ttu-id="25483-194">常數`char`型別必須寫成*character_literal*s 或*integer_literal*搭配轉型為類型 s `char`。</span><span class="sxs-lookup"><span data-stu-id="25483-194">Constants of the `char` type must be written as *character_literal*s or as *integer_literal*s in combination with a cast to type `char`.</span></span> <span data-ttu-id="25483-195">例如，`(char)10` 與 `'\x000A'` 相同。</span><span class="sxs-lookup"><span data-stu-id="25483-195">For example, `(char)10` is the same as `'\x000A'`.</span></span>

<span data-ttu-id="25483-196">`checked`並`unchecked`運算子和陳述式用來控制溢位檢查整數型別算術運算和轉換 ([checked 與 unchecked 運算子](expressions.md#the-checked-and-unchecked-operators))。</span><span class="sxs-lookup"><span data-stu-id="25483-196">The `checked` and `unchecked` operators and statements are used to control overflow checking for integral-type arithmetic operations and conversions ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)).</span></span> <span data-ttu-id="25483-197">在 `checked`溢位的內容會產生編譯時期錯誤，或導致`System.OverflowException`擲回。</span><span class="sxs-lookup"><span data-stu-id="25483-197">In a `checked` context, an overflow produces a compile-time error or causes a `System.OverflowException` to be thrown.</span></span> <span data-ttu-id="25483-198">在 `unchecked`內容溢位則會忽略和目的型別不符合任何高序位位元會被捨棄。</span><span class="sxs-lookup"><span data-stu-id="25483-198">In an `unchecked` context, overflows are ignored and any high-order bits that do not fit in the destination type are discarded.</span></span>

### <a name="floating-point-types"></a><span data-ttu-id="25483-199">浮點類型</span><span class="sxs-lookup"><span data-stu-id="25483-199">Floating point types</span></span>

<span data-ttu-id="25483-200">C# 支援兩個浮點數型別：`float`和`double`。</span><span class="sxs-lookup"><span data-stu-id="25483-200">C# supports two floating point types: `float` and `double`.</span></span> <span data-ttu-id="25483-201">`float`和`double`類型會表示使用 32 位元單精確度和 64 位元雙精確度 IEEE 754 格式，可提供下列值的集合：</span><span class="sxs-lookup"><span data-stu-id="25483-201">The `float` and `double` types are represented using the 32-bit single-precision and 64-bit double-precision IEEE 754 formats, which provide the following sets of values:</span></span>

*  <span data-ttu-id="25483-202">正零和負零。</span><span class="sxs-lookup"><span data-stu-id="25483-202">Positive zero and negative zero.</span></span> <span data-ttu-id="25483-203">在大部分情況下，正零和負零運作方式完全相同的簡單值零，但某些作業區別這兩個 ([除法運算子](expressions.md#division-operator))。</span><span class="sxs-lookup"><span data-stu-id="25483-203">In most situations, positive zero and negative zero behave identically as the simple value zero, but certain operations distinguish between the two ([Division operator](expressions.md#division-operator)).</span></span>
*  <span data-ttu-id="25483-204">正無限大和負無限大方向。</span><span class="sxs-lookup"><span data-stu-id="25483-204">Positive infinity and negative infinity.</span></span> <span data-ttu-id="25483-205">將會產生這類作業，做為非零的數字除以零。</span><span class="sxs-lookup"><span data-stu-id="25483-205">Infinities are produced by such operations as dividing a non-zero number by zero.</span></span> <span data-ttu-id="25483-206">例如，`1.0 / 0.0`會產生無限大的正數，和`-1.0 / 0.0`產生負無限大。</span><span class="sxs-lookup"><span data-stu-id="25483-206">For example, `1.0 / 0.0` yields positive infinity, and `-1.0 / 0.0` yields negative infinity.</span></span>
*  <span data-ttu-id="25483-207">***不是數字***值，通常縮寫的 NaN。</span><span class="sxs-lookup"><span data-stu-id="25483-207">The ***Not-a-Number*** value, often abbreviated NaN.</span></span> <span data-ttu-id="25483-208">無效的浮點運算，例如零除以零會產生 Nan。</span><span class="sxs-lookup"><span data-stu-id="25483-208">NaNs are produced by invalid floating-point operations, such as dividing zero by zero.</span></span>
*  <span data-ttu-id="25483-209">有限的非零值的表單`s * m * 2^e`，其中`s`為 1 或-1，並`m`和`e`取決於特定的浮點類型：針對`float`，`0 < m < 2^24`並`-149 <= e <= 104`，以及`double`，`0 < m < 2^53`和`1075 <= e <= 970`。</span><span class="sxs-lookup"><span data-stu-id="25483-209">The finite set of non-zero values of the form `s * m * 2^e`, where `s` is 1 or -1, and `m` and `e` are determined by the particular floating-point type: For `float`, `0 < m < 2^24` and `-149 <= e <= 104`, and for `double`, `0 < m < 2^53` and `1075 <= e <= 970`.</span></span> <span data-ttu-id="25483-210">反正規化浮點的數字會被視為有效的非零值。</span><span class="sxs-lookup"><span data-stu-id="25483-210">Denormalized floating-point numbers are considered valid non-zero values.</span></span>

<span data-ttu-id="25483-211">`float`型別可以代表值的範圍從大約`1.5 * 10^-45`到`3.4 * 10^38`精確度為 7 位數。</span><span class="sxs-lookup"><span data-stu-id="25483-211">The `float` type can represent values ranging from approximately `1.5 * 10^-45` to `3.4 * 10^38` with a precision of 7 digits.</span></span>

<span data-ttu-id="25483-212">`double`型別可以代表值的範圍從大約`5.0 * 10^-324`到`1.7 × 10^308`具有 15-16 位數精確度。</span><span class="sxs-lookup"><span data-stu-id="25483-212">The `double` type can represent values ranging from approximately `5.0 * 10^-324` to `1.7 × 10^308` with a precision of 15-16 digits.</span></span>

<span data-ttu-id="25483-213">如果其中一個二元運算子的運算元是浮點類型，然後將另一個運算元必須是整數類資料類型或浮點類型，和運算的評估，如下所示：</span><span class="sxs-lookup"><span data-stu-id="25483-213">If one of the operands of a binary operator is of a floating-point type, then the other operand must be of an integral type or a floating-point type, and the operation is evaluated as follows:</span></span>

*  <span data-ttu-id="25483-214">如果其中一個運算元屬於整數類資料類型時，運算元會轉換到另一個運算元的浮點類型。</span><span class="sxs-lookup"><span data-stu-id="25483-214">If one of the operands is of an integral type, then that operand is converted to the floating-point type of the other operand.</span></span>
*  <span data-ttu-id="25483-215">然後，如果任一運算元的類型`double`，另一個運算元會轉換成`double`，為作業使用最少`double`範圍和有效位數和結果的型別是`double`(或`bool`的關係運算子）。</span><span class="sxs-lookup"><span data-stu-id="25483-215">Then, if either of the operands is of type `double`, the other operand is converted to `double`, the operation is performed using at least `double` range and precision, and the type of the result is `double` (or `bool` for the relational operators).</span></span>
*  <span data-ttu-id="25483-216">否則，為作業使用至少`float`範圍和有效位數和結果的類型是`float`(或`bool`關聯式運算子)。</span><span class="sxs-lookup"><span data-stu-id="25483-216">Otherwise, the operation is performed using at least `float` range and precision, and the type of the result is `float` (or `bool` for the relational operators).</span></span>

<span data-ttu-id="25483-217">浮點數的運算子，包括指派運算子，永遠不會產生例外狀況。</span><span class="sxs-lookup"><span data-stu-id="25483-217">The floating-point operators, including the assignment operators, never produce exceptions.</span></span> <span data-ttu-id="25483-218">相反地，在例外狀況的情況下，浮點運算產生零個、 無限大或 NaN，如下所述：</span><span class="sxs-lookup"><span data-stu-id="25483-218">Instead, in exceptional situations, floating-point operations produce zero, infinity, or NaN, as described below:</span></span>

*  <span data-ttu-id="25483-219">如果浮點運算的結果太小，目的格式，作業的結果會成為正零或負零。</span><span class="sxs-lookup"><span data-stu-id="25483-219">If the result of a floating-point operation is too small for the destination format, the result of the operation becomes positive zero or negative zero.</span></span>
*  <span data-ttu-id="25483-220">如果浮點運算的結果太大，目的格式，作業的結果會成為無限大的正數或負的無限值。</span><span class="sxs-lookup"><span data-stu-id="25483-220">If the result of a floating-point operation is too large for the destination format, the result of the operation becomes positive infinity or negative infinity.</span></span>
*  <span data-ttu-id="25483-221">如果浮點運算無效，作業的結果就會變成 NaN。</span><span class="sxs-lookup"><span data-stu-id="25483-221">If a floating-point operation is invalid, the result of the operation becomes NaN.</span></span>
*  <span data-ttu-id="25483-222">如果浮點運算的一個或兩個運算元是 NaN，作業的結果就會變成 NaN。</span><span class="sxs-lookup"><span data-stu-id="25483-222">If one or both operands of a floating-point operation is NaN, the result of the operation becomes NaN.</span></span>

<span data-ttu-id="25483-223">浮點運算可能會執行，而較高的精確度卻高於此作業的結果類型。</span><span class="sxs-lookup"><span data-stu-id="25483-223">Floating-point operations may be performed with higher precision than the result type of the operation.</span></span> <span data-ttu-id="25483-224">例如，某些硬體架構支援 「 extended 」 或 「 long double"浮點類型，具有較大範圍和精確度卻高於`double`類型，而且以隱含方式執行所有使用這個較高的有效位數類型的浮點運算。</span><span class="sxs-lookup"><span data-stu-id="25483-224">For example, some hardware architectures support an "extended" or "long double" floating-point type with greater range and precision than the `double` type, and implicitly perform all floating-point operations using this higher precision type.</span></span> <span data-ttu-id="25483-225">只有在過度耗用效能可以這類硬體架構是執行較少的精確度，浮點運算，而不需要實作，以同時損失效能和精確度，C# 允許較高的有效位數類型為用於所有的浮點運算。</span><span class="sxs-lookup"><span data-stu-id="25483-225">Only at excessive cost in performance can such hardware architectures be made to perform floating-point operations with less precision, and rather than require an implementation to forfeit both performance and precision, C# allows a higher precision type to be used for all floating-point operations.</span></span> <span data-ttu-id="25483-226">以外提供更精確的結果，這很少會有任何明顯的影響。</span><span class="sxs-lookup"><span data-stu-id="25483-226">Other than delivering more precise results, this rarely has any measurable effects.</span></span> <span data-ttu-id="25483-227">不過，在表單的運算式`x * y / z`、 乘法運算產生的結果，超出`double`範圍，但後續的除法帶來暫時的結果，回`double`範圍，此運算式的事實評估在更高版本範圍格式可能會導致有限的結果，而不是無限大的值產生。</span><span class="sxs-lookup"><span data-stu-id="25483-227">However, in expressions of the form `x * y / z`, where the multiplication produces a result that is outside the `double` range, but the subsequent division brings the temporary result back into the `double` range, the fact that the expression is evaluated in a higher range format may cause a finite result to be produced instead of an infinity.</span></span>

### <a name="the-decimal-type"></a><span data-ttu-id="25483-228">Decimal 型別</span><span class="sxs-lookup"><span data-stu-id="25483-228">The decimal type</span></span>

<span data-ttu-id="25483-229">`decimal` 型別是 128 位元的資料型別，適合財務和貨幣計算。</span><span class="sxs-lookup"><span data-stu-id="25483-229">The `decimal` type is a 128-bit data type suitable for financial and monetary calculations.</span></span> <span data-ttu-id="25483-230">`decimal`型別可以代表值的範圍從`1.0 * 10^-28`到大約`7.9 * 10^28`具有 28-29 個有效位數。</span><span class="sxs-lookup"><span data-stu-id="25483-230">The `decimal` type can represent values ranging from `1.0 * 10^-28` to approximately `7.9 * 10^28` with 28-29 significant digits.</span></span>

<span data-ttu-id="25483-231">有限的一組值的型別`decimal`的格式`(-1)^s * c * 10^-e`，其中登`s`是 0 或 1，決定係數`c`由提供`0 <= *c* < 2^96`，和小數位數`e`會使得`0 <= e <= 28`。`decimal`類型不帶正負號的零、 無限或 NaN 的支援。</span><span class="sxs-lookup"><span data-stu-id="25483-231">The finite set of values of type `decimal` are of the form `(-1)^s * c * 10^-e`, where the sign `s` is 0 or 1, the coefficient `c` is given by `0 <= *c* < 2^96`, and the scale `e` is such that `0 <= e <= 28`.The `decimal` type does not support signed zeros, infinities, or NaN's.</span></span> <span data-ttu-id="25483-232">A`decimal`以調整的 10 乘冪的 96 位元整數。</span><span class="sxs-lookup"><span data-stu-id="25483-232">A `decimal` is represented as a 96-bit integer scaled by a power of ten.</span></span> <span data-ttu-id="25483-233">針對`decimal`與絕對值小於`1.0m`，值是確切為 28 小數位數，但就對了。</span><span class="sxs-lookup"><span data-stu-id="25483-233">For `decimal`s with an absolute value less than `1.0m`, the value is exact to the 28th decimal place, but no further.</span></span> <span data-ttu-id="25483-234">針對`decimal`與絕對值大於或等於`1.0m`，值就會是 28 或 29 位數。</span><span class="sxs-lookup"><span data-stu-id="25483-234">For `decimal`s with an absolute value greater than or equal to `1.0m`, the value is exact to 28 or 29 digits.</span></span> <span data-ttu-id="25483-235">Contrary 來`float`並`double`資料類型，例如 0.1 十進位小數的數字可以完全代表`decimal`表示法。</span><span class="sxs-lookup"><span data-stu-id="25483-235">Contrary to the `float` and `double` data types, decimal fractional numbers such as 0.1 can be represented exactly in the `decimal` representation.</span></span> <span data-ttu-id="25483-236">在 `float`和`double`表示法，這樣的數字通常是無限的分數，讓那些表示法更容易捨入錯誤。</span><span class="sxs-lookup"><span data-stu-id="25483-236">In the `float` and `double` representations, such numbers are often infinite fractions, making those representations more prone to round-off errors.</span></span>

<span data-ttu-id="25483-237">如果其中一個二元運算子的運算元為類型`decimal`，然後將另一個運算元必須是整數類資料類型或類型`decimal`。</span><span class="sxs-lookup"><span data-stu-id="25483-237">If one of the operands of a binary operator is of type `decimal`, then the other operand must be of an integral type or of type `decimal`.</span></span> <span data-ttu-id="25483-238">如果整數類資料類型的運算元，則它會轉換成`decimal`執行作業之前。</span><span class="sxs-lookup"><span data-stu-id="25483-238">If an integral type operand is present, it is converted to `decimal` before the operation is performed.</span></span>

<span data-ttu-id="25483-239">類型的值上作業的結果`decimal`，這會造成的計算的確切的結果 （保留擴展、 為每個運算子所定義），然後捨入成表示法。</span><span class="sxs-lookup"><span data-stu-id="25483-239">The result of an operation on values of type `decimal` is that which would result from calculating an exact result (preserving scale, as defined for each operator) and then rounding to fit the representation.</span></span> <span data-ttu-id="25483-240">結果會捨入為最接近的可表示值，當結果同樣很接近兩個可顯示的值 （這稱為 「 銀行進位 」） 的最小顯著性數字位置有偶數數目的值。</span><span class="sxs-lookup"><span data-stu-id="25483-240">Results are rounded to the nearest representable value, and, when a result is equally close to two representable values, to the value that has an even number in the least significant digit position (this is known as "banker's rounding").</span></span> <span data-ttu-id="25483-241">零結果一律會有 0 的符號，小數位數 0。</span><span class="sxs-lookup"><span data-stu-id="25483-241">A zero result always has a sign of 0 and a scale of 0.</span></span>

<span data-ttu-id="25483-242">如果小數點的算術運算會產生值小於或等於`5 * 10^-29`絕對值，在作業的結果會變成零。</span><span class="sxs-lookup"><span data-stu-id="25483-242">If a decimal arithmetic operation produces a value less than or equal to `5 * 10^-29` in absolute value, the result of the operation becomes zero.</span></span> <span data-ttu-id="25483-243">如果`decimal`算術運算會產生太大的結果`decimal`格式，`System.OverflowException`就會擲回。</span><span class="sxs-lookup"><span data-stu-id="25483-243">If a `decimal` arithmetic operation produces a result that is too large for the `decimal` format, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="25483-244">`decimal`型別具有較高有效位數比浮點數類型但較小的範圍。</span><span class="sxs-lookup"><span data-stu-id="25483-244">The `decimal` type has greater precision but smaller range than the floating-point types.</span></span> <span data-ttu-id="25483-245">因此，從浮點類型的轉換`decimal`可能會產生溢位例外狀況，並從轉換`decimal`於浮點類型可能會導致遺失有效位數。</span><span class="sxs-lookup"><span data-stu-id="25483-245">Thus, conversions from the floating-point types to `decimal` might produce overflow exceptions, and conversions from `decimal` to the floating-point types might cause loss of precision.</span></span> <span data-ttu-id="25483-246">基於這些理由，沒有隱含轉換則是浮點類型之間存在並`decimal`，，而不需要明確的轉型，則不可以混合使用浮點和`decimal`中同一個運算式的運算元。</span><span class="sxs-lookup"><span data-stu-id="25483-246">For these reasons, no implicit conversions exist between the floating-point types and `decimal`, and without explicit casts, it is not possible to mix floating-point and `decimal` operands in the same expression.</span></span>

### <a name="the-bool-type"></a><span data-ttu-id="25483-247">布林值類型</span><span class="sxs-lookup"><span data-stu-id="25483-247">The bool type</span></span>

<span data-ttu-id="25483-248">`bool`類型代表布林值邏輯的數量。</span><span class="sxs-lookup"><span data-stu-id="25483-248">The `bool` type represents boolean logical quantities.</span></span> <span data-ttu-id="25483-249">可能的值型別的`bool`都`true`和`false`。</span><span class="sxs-lookup"><span data-stu-id="25483-249">The possible values of type `bool` are `true` and `false`.</span></span>

<span data-ttu-id="25483-250">標準轉換存在之間`bool`和其他類型。</span><span class="sxs-lookup"><span data-stu-id="25483-250">No standard conversions exist between `bool` and other types.</span></span> <span data-ttu-id="25483-251">特別是，`bool`型別是有別，各自獨立的整數類資料類型，和`bool`值不能用來取代一個整數值，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="25483-251">In particular, the `bool` type is distinct and separate from the integral types, and a `bool` value cannot be used in place of an integral value, and vice versa.</span></span>

<span data-ttu-id="25483-252">在 C 和 c + + 語言中，零整數或浮點值或 null 指標可以轉換成布林值`false`，而非零整數或浮點值或為非 null 指標可以轉換成布林值`true`。</span><span class="sxs-lookup"><span data-stu-id="25483-252">In the C and C++ languages, a zero integral or floating-point value, or a null pointer can be converted to the boolean value `false`, and a non-zero integral or floating-point value, or a non-null pointer can be converted to the boolean value `true`.</span></span> <span data-ttu-id="25483-253">在 C# 中，藉由明確地比較的整數或浮點數的值為零，或藉由明確地比較的物件參考，會完成這類轉換`null`。</span><span class="sxs-lookup"><span data-stu-id="25483-253">In C#, such conversions are accomplished by explicitly comparing an integral or floating-point value to zero, or by explicitly comparing an object reference to `null`.</span></span>

### <a name="enumeration-types"></a><span data-ttu-id="25483-254">列舉類型</span><span class="sxs-lookup"><span data-stu-id="25483-254">Enumeration types</span></span>

<span data-ttu-id="25483-255">列舉型別是具名常數的不同類型。</span><span class="sxs-lookup"><span data-stu-id="25483-255">An enumeration type is a distinct type with named constants.</span></span> <span data-ttu-id="25483-256">每個列舉類型都具有基礎類型，必須是`byte`， `sbyte`， `short`， `ushort`， `int`， `uint`，`long`或`ulong`。</span><span class="sxs-lookup"><span data-stu-id="25483-256">Every enumeration type has an underlying type, which must be `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` or `ulong`.</span></span> <span data-ttu-id="25483-257">一組值的列舉型別是一組與基礎型別的值相同。</span><span class="sxs-lookup"><span data-stu-id="25483-257">The set of values of the enumeration type is the same as the set of values of the underlying type.</span></span> <span data-ttu-id="25483-258">列舉類型的值不限於具名常數的值。</span><span class="sxs-lookup"><span data-stu-id="25483-258">Values of the enumeration type are not restricted to the values of the named constants.</span></span> <span data-ttu-id="25483-259">列舉型別會定義透過列舉型別宣告 ([列舉宣告](enums.md#enum-declarations))。</span><span class="sxs-lookup"><span data-stu-id="25483-259">Enumeration types are defined through enumeration declarations ([Enum declarations](enums.md#enum-declarations)).</span></span>

### <a name="nullable-types"></a><span data-ttu-id="25483-260">可為 Null 的型別</span><span class="sxs-lookup"><span data-stu-id="25483-260">Nullable types</span></span>

<span data-ttu-id="25483-261">可為 null 的型別可以代表所有值及其***基礎型別***再加上額外的 null 值。</span><span class="sxs-lookup"><span data-stu-id="25483-261">A nullable type can represent all values of its ***underlying type*** plus an additional null value.</span></span> <span data-ttu-id="25483-262">可為 null 的型別會撰寫`T?`，其中`T`為基礎的類型。</span><span class="sxs-lookup"><span data-stu-id="25483-262">A nullable type is written `T?`, where `T` is the underlying type.</span></span> <span data-ttu-id="25483-263">此語法是速記`System.Nullable<T>`，兩種形式可以交替使用。</span><span class="sxs-lookup"><span data-stu-id="25483-263">This syntax is shorthand for `System.Nullable<T>`, and the two forms can be used interchangeably.</span></span>

<span data-ttu-id="25483-264">A***不可為 null 的實值型別***相反的是任何實值型別`System.Nullable<T>`和其簡短`T?`(針對任何`T`)，再加上任何已限制為不可為 null 的實值型別 （也就是任何的型別參數型別參數使用`struct`條件約束)。</span><span class="sxs-lookup"><span data-stu-id="25483-264">A ***non-nullable value type*** conversely is any value type other than `System.Nullable<T>` and its shorthand `T?` (for any `T`), plus any type parameter that is constrained to be a non-nullable value type (that is, any type parameter with a `struct` constraint).</span></span> <span data-ttu-id="25483-265">`System.Nullable<T>`型別指定的值類型條件約束`T`([類型參數條件約束](classes.md#type-parameter-constraints))，這表示可為 null 的型別的基礎型別可以是任何非可為 null 的實值類型。</span><span class="sxs-lookup"><span data-stu-id="25483-265">The `System.Nullable<T>` type specifies the value type constraint for `T` ([Type parameter constraints](classes.md#type-parameter-constraints)), which means that the underlying type of a nullable type can be any non-nullable value type.</span></span> <span data-ttu-id="25483-266">可為 null 的型別的基礎型別不能為 null 的類型或參考型別。</span><span class="sxs-lookup"><span data-stu-id="25483-266">The underlying type of a nullable type cannot be a nullable type or a reference type.</span></span> <span data-ttu-id="25483-267">例如，`int??`和`string?`都是無效的型別。</span><span class="sxs-lookup"><span data-stu-id="25483-267">For example, `int??` and `string?` are invalid types.</span></span>

<span data-ttu-id="25483-268">可為 null 型別的執行個體`T?`有兩個公用唯讀屬性：</span><span class="sxs-lookup"><span data-stu-id="25483-268">An instance of a nullable type `T?` has two public read-only properties:</span></span>

*  <span data-ttu-id="25483-269">A`HasValue`類型屬性 `bool`</span><span class="sxs-lookup"><span data-stu-id="25483-269">A `HasValue` property of type `bool`</span></span>
*  <span data-ttu-id="25483-270">A`Value`類型屬性 `T`</span><span class="sxs-lookup"><span data-stu-id="25483-270">A `Value` property of type `T`</span></span>

<span data-ttu-id="25483-271">執行個體`HasValue`為 true 即為非 null。</span><span class="sxs-lookup"><span data-stu-id="25483-271">An instance for which `HasValue` is true is said to be non-null.</span></span> <span data-ttu-id="25483-272">非 null 執行個體包含已知的值和`Value`傳回該值。</span><span class="sxs-lookup"><span data-stu-id="25483-272">A non-null instance contains a known value and `Value` returns that value.</span></span>

<span data-ttu-id="25483-273">執行個體`HasValue`是 false 」 即為 null。</span><span class="sxs-lookup"><span data-stu-id="25483-273">An instance for which `HasValue` is false is said to be null.</span></span> <span data-ttu-id="25483-274">Null 執行個體有未定義的值。</span><span class="sxs-lookup"><span data-stu-id="25483-274">A null instance has an undefined value.</span></span> <span data-ttu-id="25483-275">嘗試讀取`Value`的 null 執行個體，會導致`System.InvalidOperationException`擲回。</span><span class="sxs-lookup"><span data-stu-id="25483-275">Attempting to read the `Value` of a null instance causes a `System.InvalidOperationException` to be thrown.</span></span> <span data-ttu-id="25483-276">存取的程序`Value`可為 null 的執行個體的屬性指***解除包裝***。</span><span class="sxs-lookup"><span data-stu-id="25483-276">The process of accessing the `Value` property of a nullable instance is referred to as ***unwrapping***.</span></span>

<span data-ttu-id="25483-277">除了預設建構函式，每個可為 null 的型別`T?`的公用建構函式接受單一引數的型別`T`。</span><span class="sxs-lookup"><span data-stu-id="25483-277">In addition to the default constructor, every nullable type `T?` has a public constructor that takes a single argument of type `T`.</span></span> <span data-ttu-id="25483-278">指定值`x`型別的`T`，表單的建構函式引動過程</span><span class="sxs-lookup"><span data-stu-id="25483-278">Given a value `x` of type `T`, a constructor invocation of the form</span></span>

```csharp
new T?(x)
```
<span data-ttu-id="25483-279">建立的非 null 執行個體`T?`為其`Value`屬性是`x`。</span><span class="sxs-lookup"><span data-stu-id="25483-279">creates a non-null instance of `T?` for which the `Value` property is `x`.</span></span> <span data-ttu-id="25483-280">針對指定的值就是建立可為 null 類型的非 null 執行個體的程序***包裝***。</span><span class="sxs-lookup"><span data-stu-id="25483-280">The process of creating a non-null instance of a nullable type for a given value is referred to as ***wrapping***.</span></span>

<span data-ttu-id="25483-281">隱含轉換都是從`null`常值，以便`T?`([Null 常值轉換](conversions.md#null-literal-conversions)) 以及從`T`來`T?`([隱含的 null 轉換](conversions.md#implicit-nullable-conversions)).</span><span class="sxs-lookup"><span data-stu-id="25483-281">Implicit conversions are available from the `null` literal to `T?` ([Null literal conversions](conversions.md#null-literal-conversions)) and from `T` to `T?` ([Implicit nullable conversions](conversions.md#implicit-nullable-conversions)).</span></span>

## <a name="reference-types"></a><span data-ttu-id="25483-282">參考型別</span><span class="sxs-lookup"><span data-stu-id="25483-282">Reference types</span></span>

<span data-ttu-id="25483-283">參考型別是類別類型、 介面類型、 陣列類型或委派型別。</span><span class="sxs-lookup"><span data-stu-id="25483-283">A reference type is a class type, an interface type, an array type, or a delegate type.</span></span>

```antlr
reference_type
    : class_type
    | interface_type
    | array_type
    | delegate_type
    ;

class_type
    : type_name
    | 'object'
    | 'dynamic'
    | 'string'
    ;

interface_type
    : type_name
    ;

array_type
    : non_array_type rank_specifier+
    ;

non_array_type
    : type
    ;

rank_specifier
    : '[' dim_separator* ']'
    ;

dim_separator
    : ','
    ;

delegate_type
    : type_name
    ;
```

<span data-ttu-id="25483-284">參考型別值是參考***執行個體***型別，而後者就稱為***物件***。</span><span class="sxs-lookup"><span data-stu-id="25483-284">A reference type value is a reference to an ***instance*** of the type, the latter known as an ***object***.</span></span> <span data-ttu-id="25483-285">特殊值`null`適用於所有參考型別，並指出執行個體不存在。</span><span class="sxs-lookup"><span data-stu-id="25483-285">The special value `null` is compatible with all reference types and indicates the absence of an instance.</span></span>

### <a name="class-types"></a><span data-ttu-id="25483-286">類別型別</span><span class="sxs-lookup"><span data-stu-id="25483-286">Class types</span></span>

<span data-ttu-id="25483-287">類別型別定義資料結構，其中包含資料成員 （常數和欄位），函式成員 （方法、 屬性、 事件、 索引子、 運算子、 執行個體建構函式、 解構函式和靜態建構函式） 和巢狀型別。</span><span class="sxs-lookup"><span data-stu-id="25483-287">A class type defines a data structure that contains data members (constants and fields), function members (methods, properties, events, indexers, operators, instance constructors, destructors and static constructors), and nested types.</span></span> <span data-ttu-id="25483-288">類別類型支援繼承，藉此延伸及特製化的基底類別衍生的類別的機制。</span><span class="sxs-lookup"><span data-stu-id="25483-288">Class types support inheritance, a mechanism whereby derived classes can extend and specialize base classes.</span></span> <span data-ttu-id="25483-289">使用建立類別類型的執行個體*object_creation_expression*s ([物件建立運算式](expressions.md#object-creation-expressions))。</span><span class="sxs-lookup"><span data-stu-id="25483-289">Instances of class types are created using *object_creation_expression*s ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span>

<span data-ttu-id="25483-290">類別型別所述[類別](classes.md)。</span><span class="sxs-lookup"><span data-stu-id="25483-290">Class types are described in [Classes](classes.md).</span></span>

<span data-ttu-id="25483-291">下表中所述，某些預先定義的類別類型會有在 C# 語言中，特殊的意義。</span><span class="sxs-lookup"><span data-stu-id="25483-291">Certain predefined class types have special meaning in the C# language, as described in the table below.</span></span>


| <span data-ttu-id="25483-292">__類別類型__</span><span class="sxs-lookup"><span data-stu-id="25483-292">__Class type__</span></span>     | <span data-ttu-id="25483-293">__描述__</span><span class="sxs-lookup"><span data-stu-id="25483-293">__Description__</span></span>                                         |
|--------------------|---------------------------------------------------------|
| `System.Object`    | <span data-ttu-id="25483-294">所有其他類型的 ultimate 基底類別。</span><span class="sxs-lookup"><span data-stu-id="25483-294">The ultimate base class of all other types.</span></span> <span data-ttu-id="25483-295">請參閱[的物件型別](types.md#the-object-type)。</span><span class="sxs-lookup"><span data-stu-id="25483-295">See [The object type](types.md#the-object-type).</span></span> | 
| `System.String`    | <span data-ttu-id="25483-296">C# 語言的字串型別。</span><span class="sxs-lookup"><span data-stu-id="25483-296">The string type of the C# language.</span></span> <span data-ttu-id="25483-297">請參閱[string 型別](types.md#the-string-type)。</span><span class="sxs-lookup"><span data-stu-id="25483-297">See [The string type](types.md#the-string-type).</span></span>         |
| `System.ValueType` | <span data-ttu-id="25483-298">所有實值型別的基底類別。</span><span class="sxs-lookup"><span data-stu-id="25483-298">The base class of all value types.</span></span> <span data-ttu-id="25483-299">請參閱[System.ValueType 類型](types.md#the-systemvaluetype-type)。</span><span class="sxs-lookup"><span data-stu-id="25483-299">See [The System.ValueType type](types.md#the-systemvaluetype-type).</span></span>          |
| `System.Enum`      | <span data-ttu-id="25483-300">所有列舉類型的基底類別。</span><span class="sxs-lookup"><span data-stu-id="25483-300">The base class of all enum types.</span></span> <span data-ttu-id="25483-301">請參閱[列舉](enums.md)。</span><span class="sxs-lookup"><span data-stu-id="25483-301">See [Enums](enums.md).</span></span>              |
| `System.Array`     | <span data-ttu-id="25483-302">所有陣列類型的基底類別。</span><span class="sxs-lookup"><span data-stu-id="25483-302">The base class of all array types.</span></span> <span data-ttu-id="25483-303">請參閱[陣列](arrays.md)。</span><span class="sxs-lookup"><span data-stu-id="25483-303">See [Arrays](arrays.md).</span></span>             |
| `System.Delegate`  | <span data-ttu-id="25483-304">所有的委派類型的基底類別。</span><span class="sxs-lookup"><span data-stu-id="25483-304">The base class of all delegate types.</span></span> <span data-ttu-id="25483-305">請參閱[委派](delegates.md)。</span><span class="sxs-lookup"><span data-stu-id="25483-305">See [Delegates](delegates.md).</span></span>          |
| `System.Exception` | <span data-ttu-id="25483-306">所有的例外狀況類型的基底類別。</span><span class="sxs-lookup"><span data-stu-id="25483-306">The base class of all exception types.</span></span> <span data-ttu-id="25483-307">請參閱[例外狀況](exceptions.md)。</span><span class="sxs-lookup"><span data-stu-id="25483-307">See [Exceptions](exceptions.md).</span></span>         |

### <a name="the-object-type"></a><span data-ttu-id="25483-308">物件類型</span><span class="sxs-lookup"><span data-stu-id="25483-308">The object type</span></span>

<span data-ttu-id="25483-309">`object`類別類型是所有其他類型的 ultimate 基底類別。</span><span class="sxs-lookup"><span data-stu-id="25483-309">The `object` class type is the ultimate base class of all other types.</span></span> <span data-ttu-id="25483-310">在 C# 中的每個類型直接或間接衍生自`object`類別型別。</span><span class="sxs-lookup"><span data-stu-id="25483-310">Every type in C# directly or indirectly derives from the `object` class type.</span></span>

<span data-ttu-id="25483-311">關鍵字`object`是只要預先定義的類別別名`System.Object`。</span><span class="sxs-lookup"><span data-stu-id="25483-311">The keyword `object` is simply an alias for the predefined class `System.Object`.</span></span>

### <a name="the-dynamic-type"></a><span data-ttu-id="25483-312">動態類型</span><span class="sxs-lookup"><span data-stu-id="25483-312">The dynamic type</span></span>

<span data-ttu-id="25483-313">`dynamic`類型，如`object`，可以參考任何物件。</span><span class="sxs-lookup"><span data-stu-id="25483-313">The `dynamic` type, like `object`, can reference any object.</span></span> <span data-ttu-id="25483-314">當運算子會套用至類型的運算式`dynamic`，及其解決方式延後，直到執行程式。</span><span class="sxs-lookup"><span data-stu-id="25483-314">When operators are applied to expressions of type `dynamic`, their resolution is deferred until the program is run.</span></span> <span data-ttu-id="25483-315">因此，如果合法無法將運算子套用至參考的物件，沒有錯誤可以在編譯期間。</span><span class="sxs-lookup"><span data-stu-id="25483-315">Thus, if the operator cannot legally be applied to the referenced object, no error is given during compilation.</span></span> <span data-ttu-id="25483-316">改為在執行階段的運算子的解析失敗時，例外狀況就會擲回。</span><span class="sxs-lookup"><span data-stu-id="25483-316">Instead an exception will be thrown when resolution of the operator fails at run-time.</span></span>

<span data-ttu-id="25483-317">其目的是要允許動態繫結，在詳細資料中所述[動態繫結](expressions.md#dynamic-binding)。</span><span class="sxs-lookup"><span data-stu-id="25483-317">Its purpose is to allow dynamic binding, which is described in detail in [Dynamic binding](expressions.md#dynamic-binding).</span></span>

<span data-ttu-id="25483-318">`dynamic` 會視為相同`object`除了在下列方面：</span><span class="sxs-lookup"><span data-stu-id="25483-318">`dynamic` is considered identical to `object` except in the following respects:</span></span>

*  <span data-ttu-id="25483-319">類型的運算式上的作業`dynamic`可以動態地繫結 ([動態繫結](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="25483-319">Operations on expressions of type `dynamic` can be dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span>
*  <span data-ttu-id="25483-320">型別推斷 ([型別推斷](expressions.md#type-inference)) 會偏好`dynamic`透過`object`兩者都是候選項目。</span><span class="sxs-lookup"><span data-stu-id="25483-320">Type inference ([Type inference](expressions.md#type-inference)) will prefer `dynamic` over `object` if both are candidates.</span></span>

<span data-ttu-id="25483-321">因為此是否相等，下列的保留：</span><span class="sxs-lookup"><span data-stu-id="25483-321">Because of this equivalence, the following holds:</span></span>

*  <span data-ttu-id="25483-322">之間隱含的身分識別轉換`object`並`dynamic`，並取代時，都是相同的建構型別之間`dynamic`與 `object`</span><span class="sxs-lookup"><span data-stu-id="25483-322">There is an implicit identity conversion between `object` and `dynamic`, and between constructed types that are the same when replacing `dynamic` with `object`</span></span>
*  <span data-ttu-id="25483-323">隱含和明確轉換，來回`object`也適用於與`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="25483-323">Implicit and explicit conversions to and from `object` also apply to and from `dynamic`.</span></span>
*  <span data-ttu-id="25483-324">取代時，都是相同的方法簽章`dynamic`與`object`會被視為相同的簽章</span><span class="sxs-lookup"><span data-stu-id="25483-324">Method signatures that are the same when replacing `dynamic` with `object` are considered the same signature</span></span>
*  <span data-ttu-id="25483-325">型別`dynamic`區別`object`在執行階段。</span><span class="sxs-lookup"><span data-stu-id="25483-325">The type `dynamic` is indistinguishable from `object` at run-time.</span></span>
*  <span data-ttu-id="25483-326">類型的運算式`dynamic`指***動態運算式***。</span><span class="sxs-lookup"><span data-stu-id="25483-326">An expression of the type `dynamic` is referred to as a ***dynamic expression***.</span></span>

### <a name="the-string-type"></a><span data-ttu-id="25483-327">String 型別</span><span class="sxs-lookup"><span data-stu-id="25483-327">The string type</span></span>

<span data-ttu-id="25483-328">`string`型別是密封的類別型別直接繼承自`object`。</span><span class="sxs-lookup"><span data-stu-id="25483-328">The `string` type is a sealed class type that inherits directly from `object`.</span></span> <span data-ttu-id="25483-329">執行個體`string`類別代表 Unicode 字元字串。</span><span class="sxs-lookup"><span data-stu-id="25483-329">Instances of the `string` class represent Unicode character strings.</span></span>

<span data-ttu-id="25483-330">值`string`型別可以寫成字串常值 ([字串常值](lexical-structure.md#string-literals))。</span><span class="sxs-lookup"><span data-stu-id="25483-330">Values of the `string` type can be written as string literals ([String literals](lexical-structure.md#string-literals)).</span></span>

<span data-ttu-id="25483-331">關鍵字`string`是只要預先定義的類別別名`System.String`。</span><span class="sxs-lookup"><span data-stu-id="25483-331">The keyword `string` is simply an alias for the predefined class `System.String`.</span></span>

### <a name="interface-types"></a><span data-ttu-id="25483-332">介面型別</span><span class="sxs-lookup"><span data-stu-id="25483-332">Interface types</span></span>

<span data-ttu-id="25483-333">介面定義合約。</span><span class="sxs-lookup"><span data-stu-id="25483-333">An interface defines a contract.</span></span> <span data-ttu-id="25483-334">類別或結構實作介面必須遵守其合約。</span><span class="sxs-lookup"><span data-stu-id="25483-334">A class or struct that implements an interface must adhere to its contract.</span></span> <span data-ttu-id="25483-335">介面可以繼承自多個基底介面，以及類別或結構可以實作多個介面。</span><span class="sxs-lookup"><span data-stu-id="25483-335">An interface may inherit from multiple base interfaces, and a class or struct may implement multiple interfaces.</span></span>

<span data-ttu-id="25483-336">介面型別所述[介面](interfaces.md)。</span><span class="sxs-lookup"><span data-stu-id="25483-336">Interface types are described in [Interfaces](interfaces.md).</span></span>

### <a name="array-types"></a><span data-ttu-id="25483-337">陣列型別</span><span class="sxs-lookup"><span data-stu-id="25483-337">Array types</span></span>

<span data-ttu-id="25483-338">陣列是資料結構，其中包含零個或多個變數是透過計算索引存取。</span><span class="sxs-lookup"><span data-stu-id="25483-338">An array is a data structure that contains zero or more variables which are accessed through computed indices.</span></span> <span data-ttu-id="25483-339">陣列，也稱為陣列的項目中包含的變數都相同的型別，以及這個型別稱為陣列的項目類型。</span><span class="sxs-lookup"><span data-stu-id="25483-339">The variables contained in an array, also called the elements of the array, are all of the same type, and this type is called the element type of the array.</span></span>

<span data-ttu-id="25483-340">陣列型別所述[陣列](arrays.md)。</span><span class="sxs-lookup"><span data-stu-id="25483-340">Array types are described in [Arrays](arrays.md).</span></span>

### <a name="delegate-types"></a><span data-ttu-id="25483-341">委派型別</span><span class="sxs-lookup"><span data-stu-id="25483-341">Delegate types</span></span>

<span data-ttu-id="25483-342">委派是參考一或多個方法的資料結構。</span><span class="sxs-lookup"><span data-stu-id="25483-342">A delegate is a data structure that refers to one or more methods.</span></span> <span data-ttu-id="25483-343">執行個體方法，它也是指其對應的物件執行個體。</span><span class="sxs-lookup"><span data-stu-id="25483-343">For instance methods, it also refers to their corresponding object instances.</span></span>

<span data-ttu-id="25483-344">最接近對等項目，在 C 或 c + + 中委派的函式指標，但函式指標只能參考靜態函式，而委派可以參考這兩個靜態和執行個體方法。</span><span class="sxs-lookup"><span data-stu-id="25483-344">The closest equivalent of a delegate in C or C++ is a function pointer, but whereas a function pointer can only reference static functions, a delegate can reference both static and instance methods.</span></span> <span data-ttu-id="25483-345">在後者的情況下，委派會儲存參考至方法的進入點，不僅要叫用方法的物件執行個體的參考。</span><span class="sxs-lookup"><span data-stu-id="25483-345">In the latter case, the delegate stores not only a reference to the method's entry point, but also a reference to the object instance on which to invoke the method.</span></span>

<span data-ttu-id="25483-346">委派型別所述[委派](delegates.md)。</span><span class="sxs-lookup"><span data-stu-id="25483-346">Delegate types are described in [Delegates](delegates.md).</span></span>

## <a name="boxing-and-unboxing"></a><span data-ttu-id="25483-347">Boxing 和 Unboxing</span><span class="sxs-lookup"><span data-stu-id="25483-347">Boxing and unboxing</span></span>

<span data-ttu-id="25483-348">Boxing 和 unboxing 的概念是 C# 類型系統的核心。</span><span class="sxs-lookup"><span data-stu-id="25483-348">The concept of boxing and unboxing is central to C#'s type system.</span></span> <span data-ttu-id="25483-349">它提供之間的橋樑*value_type*s 並*reference_type*s 所允許的任何值*value_type*型別來回轉換`object`。</span><span class="sxs-lookup"><span data-stu-id="25483-349">It provides a bridge between *value_type*s and *reference_type*s by permitting any value of a *value_type* to be converted to and from type `object`.</span></span> <span data-ttu-id="25483-350">Boxing 和 unboxing 可讓其中任何類型的值可以最終會視為物件的型別系統的統一的檢視。</span><span class="sxs-lookup"><span data-stu-id="25483-350">Boxing and unboxing enables a unified view of the type system wherein a value of any type can ultimately be treated as an object.</span></span>

### <a name="boxing-conversions"></a><span data-ttu-id="25483-351">Boxing 轉換</span><span class="sxs-lookup"><span data-stu-id="25483-351">Boxing conversions</span></span>

<span data-ttu-id="25483-352">Boxing 轉換允許*value_type*隱含地轉換成*reference_type*。</span><span class="sxs-lookup"><span data-stu-id="25483-352">A boxing conversion permits a *value_type* to be implicitly converted to a *reference_type*.</span></span> <span data-ttu-id="25483-353">下列的 boxing 轉換存在：</span><span class="sxs-lookup"><span data-stu-id="25483-353">The following boxing conversions exist:</span></span>

*  <span data-ttu-id="25483-354">從任何*value_type*型別`object`。</span><span class="sxs-lookup"><span data-stu-id="25483-354">From any *value_type* to the type `object`.</span></span>
*  <span data-ttu-id="25483-355">從任何*value_type*型別`System.ValueType`。</span><span class="sxs-lookup"><span data-stu-id="25483-355">From any *value_type* to the type `System.ValueType`.</span></span>
*  <span data-ttu-id="25483-356">從任何*non_nullable_value_type*任何*interface_type*實作*value_type*。</span><span class="sxs-lookup"><span data-stu-id="25483-356">From any *non_nullable_value_type* to any *interface_type* implemented by the *value_type*.</span></span>
*  <span data-ttu-id="25483-357">從任何*nullable_type*任何*interface_type*的基礎類型所實作*nullable_type*。</span><span class="sxs-lookup"><span data-stu-id="25483-357">From any *nullable_type* to any *interface_type* implemented by the underlying type of the *nullable_type*.</span></span>
*  <span data-ttu-id="25483-358">從任何*enum_type*型別`System.Enum`。</span><span class="sxs-lookup"><span data-stu-id="25483-358">From any *enum_type* to the type `System.Enum`.</span></span>
*  <span data-ttu-id="25483-359">從任何*nullable_type*與基礎*enum_type*類型`System.Enum`。</span><span class="sxs-lookup"><span data-stu-id="25483-359">From any *nullable_type* with an underlying *enum_type* to the type `System.Enum`.</span></span>
*  <span data-ttu-id="25483-360">請注意，型別參數的隱含轉換將會執行 boxing 轉換為是否到最後在執行階段從實值型別轉換成參考型別 ([涉及型別參數的隱含轉換](conversions.md#implicit-conversions-involving-type-parameters))。</span><span class="sxs-lookup"><span data-stu-id="25483-360">Note that an implicit conversion from a type parameter will be executed as a boxing conversion if at run-time it ends up converting from a value type to a reference type ([Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters)).</span></span>

<span data-ttu-id="25483-361">Box 處理的值*non_nullable_value_type*配置的物件執行個體，並複製所組成*non_nullable_value_type*到該執行個體的值。</span><span class="sxs-lookup"><span data-stu-id="25483-361">Boxing a value of a *non_nullable_value_type* consists of allocating an object instance and copying the *non_nullable_value_type* value into that instance.</span></span>

<span data-ttu-id="25483-362">Box 處理的值*nullable_type*會產生 null 參考，它是否`null`值 (`HasValue`是`false`)，或解除包裝成為和否則 boxing 基礎值的結果。</span><span class="sxs-lookup"><span data-stu-id="25483-362">Boxing a value of a *nullable_type* produces a null reference if it is the `null` value (`HasValue` is `false`), or the result of unwrapping and boxing the underlying value otherwise.</span></span>

<span data-ttu-id="25483-363">Box 處理的值的實際程序*non_nullable_value_type*最能說明想像的泛型存在***boxing 類別***，其行為就如同它宣告，如下所示：</span><span class="sxs-lookup"><span data-stu-id="25483-363">The actual process of boxing a value of a *non_nullable_value_type* is best explained by imagining the existence of a generic ***boxing class***, which behaves as if it were declared as follows:</span></span>

```csharp
sealed class Box<T>: System.ValueType
{
    T value;

    public Box(T t) {
        value = t;
    }
}
```

<span data-ttu-id="25483-364">值的 boxing`v`型別的`T`現在包含執行運算式`new Box<T>(v)`，並傳回產生的執行個體類型的值為`object`。</span><span class="sxs-lookup"><span data-stu-id="25483-364">Boxing of a value `v` of type `T` now consists of executing the expression `new Box<T>(v)`, and returning the resulting instance as a value of type `object`.</span></span> <span data-ttu-id="25483-365">因此，這些陳述式</span><span class="sxs-lookup"><span data-stu-id="25483-365">Thus, the statements</span></span>
```csharp
int i = 123;
object box = i;
```
<span data-ttu-id="25483-366">在概念上對應至</span><span class="sxs-lookup"><span data-stu-id="25483-366">conceptually correspond to</span></span>
```csharp
int i = 123;
object box = new Box<int>(i);
```

<span data-ttu-id="25483-367">Boxing 類別，例如`Box<T>`以上版本並不實際存在和動態的 boxed 實值類型不是實際的類別類型。</span><span class="sxs-lookup"><span data-stu-id="25483-367">A boxing class like `Box<T>` above doesn't actually exist and the dynamic type of a boxed value isn't actually a class type.</span></span> <span data-ttu-id="25483-368">相反地，類型的 boxed 的值`T`具有動態類型`T`，並使用動態型別檢查`is`運算子只會參考型別`T`。</span><span class="sxs-lookup"><span data-stu-id="25483-368">Instead, a boxed value of type `T` has the dynamic type `T`, and a dynamic type check using the `is` operator can simply reference type `T`.</span></span> <span data-ttu-id="25483-369">例如，套用至物件的</span><span class="sxs-lookup"><span data-stu-id="25483-369">For example,</span></span>
```csharp
int i = 123;
object box = i;
if (box is int) {
    Console.Write("Box contains an int");
}
```
<span data-ttu-id="25483-370">會輸出字串"`Box contains an int`」 在主控台上。</span><span class="sxs-lookup"><span data-stu-id="25483-370">will output the string "`Box contains an int`" on the console.</span></span>

<span data-ttu-id="25483-371">Boxing 轉換表示的值進行 box 處理的複本。</span><span class="sxs-lookup"><span data-stu-id="25483-371">A boxing conversion implies making a copy of the value being boxed.</span></span> <span data-ttu-id="25483-372">這與不同的轉換成*reference_type*鍵入`object`中的值仍然會參考相同的執行個體只會被視為較少衍生的型別`object`。</span><span class="sxs-lookup"><span data-stu-id="25483-372">This is different from a conversion of a *reference_type* to type `object`, in which the value continues to reference the same instance and simply is regarded as the less derived type `object`.</span></span> <span data-ttu-id="25483-373">例如，假設宣告</span><span class="sxs-lookup"><span data-stu-id="25483-373">For example, given the declaration</span></span>
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
<span data-ttu-id="25483-374">下列陳述式</span><span class="sxs-lookup"><span data-stu-id="25483-374">the following statements</span></span>
```csharp
Point p = new Point(10, 10);
object box = p;
p.x = 20;
Console.Write(((Point)box).x);
```
<span data-ttu-id="25483-375">將會輸出主控台上的值 10，因為中的指派，就會發生隱含 boxing 作業`p`要`box`box 的值`p`複製。</span><span class="sxs-lookup"><span data-stu-id="25483-375">will output the value 10 on the console because the implicit boxing operation that occurs in the assignment of `p` to `box` causes the value of `p` to be copied.</span></span> <span data-ttu-id="25483-376">有`Point`已宣告`class`相反地，值 20 就是輸出，因為`p`和`box`會參考相同的執行個體。</span><span class="sxs-lookup"><span data-stu-id="25483-376">Had `Point` been declared a `class` instead, the value 20 would be output because `p` and `box` would reference the same instance.</span></span>

### <a name="unboxing-conversions"></a><span data-ttu-id="25483-377">Unboxing 轉換</span><span class="sxs-lookup"><span data-stu-id="25483-377">Unboxing conversions</span></span>

<span data-ttu-id="25483-378">Unboxing 轉換允許*reference_type*明確轉換成*value_type*。</span><span class="sxs-lookup"><span data-stu-id="25483-378">An unboxing conversion permits a *reference_type* to be explicitly converted to a *value_type*.</span></span> <span data-ttu-id="25483-379">下列 unboxing 轉換存在：</span><span class="sxs-lookup"><span data-stu-id="25483-379">The following unboxing conversions exist:</span></span>

*  <span data-ttu-id="25483-380">從型別`object`任何*value_type*。</span><span class="sxs-lookup"><span data-stu-id="25483-380">From the type `object` to any *value_type*.</span></span>
*  <span data-ttu-id="25483-381">從型別`System.ValueType`任何*value_type*。</span><span class="sxs-lookup"><span data-stu-id="25483-381">From the type `System.ValueType` to any *value_type*.</span></span>
*  <span data-ttu-id="25483-382">從任何*interface_type*任何*non_nullable_value_type*實作*interface_type*。</span><span class="sxs-lookup"><span data-stu-id="25483-382">From any *interface_type* to any *non_nullable_value_type* that implements the *interface_type*.</span></span>
*  <span data-ttu-id="25483-383">從任何*interface_type*任何*nullable_type*其基礎類型會實作*interface_type*。</span><span class="sxs-lookup"><span data-stu-id="25483-383">From any *interface_type* to any *nullable_type* whose underlying type implements the *interface_type*.</span></span>
*  <span data-ttu-id="25483-384">從型別`System.Enum`任何*enum_type*。</span><span class="sxs-lookup"><span data-stu-id="25483-384">From the type `System.Enum` to any *enum_type*.</span></span>
*  <span data-ttu-id="25483-385">從型別`System.Enum`任何*nullable_type*與基礎*enum_type*。</span><span class="sxs-lookup"><span data-stu-id="25483-385">From the type `System.Enum` to any *nullable_type* with an underlying *enum_type*.</span></span>
*  <span data-ttu-id="25483-386">請注意 unboxing 轉換為會執行，類型參數的明確轉換，是否在執行階段，它最終會從參考型別轉換成實值型別 ([明確的動態轉換](conversions.md#explicit-dynamic-conversions))。</span><span class="sxs-lookup"><span data-stu-id="25483-386">Note that an explicit conversion to a type parameter will be executed as an unboxing conversion if at run-time it ends up converting from a reference type to a value type ([Explicit dynamic conversions](conversions.md#explicit-dynamic-conversions)).</span></span>

<span data-ttu-id="25483-387">Unboxing 作業，以*non_nullable_value_type*所組成的物件執行個體 boxed 的值的第一個檢查給定*non_nullable_value_type*，然後將複製的值執行個體。</span><span class="sxs-lookup"><span data-stu-id="25483-387">An unboxing operation to a *non_nullable_value_type* consists of first checking that the object instance is a boxed value of the given *non_nullable_value_type*, and then copying the value out of the instance.</span></span>

<span data-ttu-id="25483-388">以 unboxing *nullable_type*產生 null 值的*nullable_type*來源運算元是否`null`，或包裝的結果 unboxing 的基礎類型的物件執行個體*nullable_type*否則。</span><span class="sxs-lookup"><span data-stu-id="25483-388">Unboxing to a *nullable_type* produces the null value of the *nullable_type* if the source operand is `null`, or the wrapped result of unboxing the object instance to the underlying type of the *nullable_type* otherwise.</span></span>

<span data-ttu-id="25483-389">參考前一個區段中，unboxing 轉換的物件中所述的虛數的 boxing 類別`box`要*value_type* `T`組成執行運算式`((Box<T>)box).value`。</span><span class="sxs-lookup"><span data-stu-id="25483-389">Referring to the imaginary boxing class described in the previous section, an unboxing conversion of an object `box` to a *value_type* `T` consists of executing the expression `((Box<T>)box).value`.</span></span> <span data-ttu-id="25483-390">因此，這些陳述式</span><span class="sxs-lookup"><span data-stu-id="25483-390">Thus, the statements</span></span>
```csharp
object box = 123;
int i = (int)box;
```
<span data-ttu-id="25483-391">在概念上對應至</span><span class="sxs-lookup"><span data-stu-id="25483-391">conceptually correspond to</span></span>
```csharp
object box = new Box<int>(123);
int i = ((Box<int>)box).value;
```

<span data-ttu-id="25483-392">Unboxing 轉換至給定*non_nullable_value_type*若要在執行階段成功，來源運算元的值必須是參考的 boxed 值*non_nullable_value_type*。</span><span class="sxs-lookup"><span data-stu-id="25483-392">For an unboxing conversion to a given *non_nullable_value_type* to succeed at run-time, the value of the source operand must be a reference to a boxed value of that *non_nullable_value_type*.</span></span> <span data-ttu-id="25483-393">如果來源運算元`null`、`System.NullReferenceException`就會擲回。</span><span class="sxs-lookup"><span data-stu-id="25483-393">If the source operand is `null`, a `System.NullReferenceException` is thrown.</span></span> <span data-ttu-id="25483-394">如果來源運算元是不相容的物件，參考`System.InvalidCastException`就會擲回。</span><span class="sxs-lookup"><span data-stu-id="25483-394">If the source operand is a reference to an incompatible object, a `System.InvalidCastException` is thrown.</span></span>

<span data-ttu-id="25483-395">Unboxing 轉換至給定*nullable_type*若要在執行階段成功，來源運算元的值必須是`null`或 boxed 值的基礎參考*non_nullable_value_type*的*nullable_type*。</span><span class="sxs-lookup"><span data-stu-id="25483-395">For an unboxing conversion to a given *nullable_type* to succeed at run-time, the value of the source operand must be either `null` or a reference to a boxed value of the underlying *non_nullable_value_type* of the *nullable_type*.</span></span> <span data-ttu-id="25483-396">如果來源運算元是不相容的物件，參考`System.InvalidCastException`就會擲回。</span><span class="sxs-lookup"><span data-stu-id="25483-396">If the source operand is a reference to an incompatible object, a `System.InvalidCastException` is thrown.</span></span>

## <a name="constructed-types"></a><span data-ttu-id="25483-397">建構的類型</span><span class="sxs-lookup"><span data-stu-id="25483-397">Constructed types</span></span>

<span data-ttu-id="25483-398">泛型型別宣告，單獨使用時，表示***未繫結的泛型型別***做為 「 藍圖 以形成許多不同類型的詳細資訊，藉由套用***型別引數***。</span><span class="sxs-lookup"><span data-stu-id="25483-398">A generic type declaration, by itself, denotes an ***unbound generic type*** that is used as a "blueprint" to form many different types, by way of applying ***type arguments***.</span></span> <span data-ttu-id="25483-399">型別引數撰寫在角括弧 (`<`和`>`) 緊接的泛型型別名稱。</span><span class="sxs-lookup"><span data-stu-id="25483-399">The type arguments are written within angle brackets (`<` and `>`) immediately following the name of the generic type.</span></span> <span data-ttu-id="25483-400">包含至少一個類型引數的型別稱為***建構的型別***。</span><span class="sxs-lookup"><span data-stu-id="25483-400">A type that includes at least one type argument is called a ***constructed type***.</span></span> <span data-ttu-id="25483-401">建構的類型可用以型別名稱可以出現的語言中的大部分位置中。</span><span class="sxs-lookup"><span data-stu-id="25483-401">A constructed type can be used in most places in the language in which a type name can appear.</span></span> <span data-ttu-id="25483-402">未繫結的泛型類型只可用於*typeof_expression* ([typeof 運算子](expressions.md#the-typeof-operator))。</span><span class="sxs-lookup"><span data-stu-id="25483-402">An unbound generic type can only be used within a *typeof_expression* ([The typeof operator](expressions.md#the-typeof-operator)).</span></span>

<span data-ttu-id="25483-403">建構的類型也可用在運算式中做為簡單名稱 ([簡單名稱](expressions.md#simple-names)) 或存取成員時 ([成員存取](expressions.md#member-access))。</span><span class="sxs-lookup"><span data-stu-id="25483-403">Constructed types can also be used in expressions as simple names ([Simple names](expressions.md#simple-names)) or when accessing a member ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="25483-404">當*namespace_or_type_name*是型別參數會被視為正確數目的已評估、 僅泛用型別。</span><span class="sxs-lookup"><span data-stu-id="25483-404">When a *namespace_or_type_name* is evaluated, only generic types with the correct number of type parameters are considered.</span></span> <span data-ttu-id="25483-405">因此，很可能只要類型有不同數量的型別參數，來識別不同的類型，使用相同的識別項。</span><span class="sxs-lookup"><span data-stu-id="25483-405">Thus, it is possible to use the same identifier to identify different types, as long as the types have different numbers of type parameters.</span></span> <span data-ttu-id="25483-406">混合相同程式中的泛型和非泛型類別時，這很有用：</span><span class="sxs-lookup"><span data-stu-id="25483-406">This is useful when mixing generic and non-generic classes in the same program:</span></span>

```csharp
namespace Widgets
{
    class Queue {...}
    class Queue<TElement> {...}
}

namespace MyApplication
{
    using Widgets;

    class X
    {
        Queue q1;            // Non-generic Widgets.Queue
        Queue<int> q2;       // Generic Widgets.Queue
    }
}
```

<span data-ttu-id="25483-407">A *type_name*可能會識別建構的類型，即使它不會直接指定型別參數。</span><span class="sxs-lookup"><span data-stu-id="25483-407">A *type_name* might identify a constructed type even though it doesn't specify type parameters directly.</span></span> <span data-ttu-id="25483-408">這可能會發生在泛型類別宣告中，巢狀類型，包含宣告的執行個體類型隱含地用於名稱查閱 ([巢狀在泛型類別的型別](classes.md#nested-types-in-generic-classes)):</span><span class="sxs-lookup"><span data-stu-id="25483-408">This can occur where a type is nested within a generic class declaration, and the instance type of the containing declaration is implicitly used for name lookup ([Nested types in generic classes](classes.md#nested-types-in-generic-classes)):</span></span>

```csharp
class Outer<T>
{
    public class Inner {...}

    public Inner i;                // Type of i is Outer<T>.Inner
}
```

<span data-ttu-id="25483-409">在不安全的程式碼中建構的類型不能做*unmanaged_type* ([指標類型](unsafe-code.md#pointer-types))。</span><span class="sxs-lookup"><span data-stu-id="25483-409">In unsafe code, a constructed type cannot be used as an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

### <a name="type-arguments"></a><span data-ttu-id="25483-410">型別引數</span><span class="sxs-lookup"><span data-stu-id="25483-410">Type arguments</span></span>

<span data-ttu-id="25483-411">每個引數型別引數清單中的只是單純*型別*。</span><span class="sxs-lookup"><span data-stu-id="25483-411">Each argument in a type argument list is simply a *type*.</span></span>

```antlr
type_argument_list
    : '<' type_arguments '>'
    ;

type_arguments
    : type_argument (',' type_argument)*
    ;

type_argument
    : type
    ;
```

<span data-ttu-id="25483-412">Unsafe 程式碼中 ([Unsafe 程式碼](unsafe-code.md))，則*type_argument*可能不是指標類型。</span><span class="sxs-lookup"><span data-stu-id="25483-412">In unsafe code ([Unsafe code](unsafe-code.md)), a *type_argument* may not be a pointer type.</span></span> <span data-ttu-id="25483-413">每個類型引數必須滿足任何條件約束對應型別參數 ([類型參數條件約束](classes.md#type-parameter-constraints))。</span><span class="sxs-lookup"><span data-stu-id="25483-413">Each type argument must satisfy any constraints on the corresponding type parameter ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>

### <a name="open-and-closed-types"></a><span data-ttu-id="25483-414">開放和封閉類型</span><span class="sxs-lookup"><span data-stu-id="25483-414">Open and closed types</span></span>

<span data-ttu-id="25483-415">所有型別可以分類為 「***開啟類型***或是***封閉類型***。</span><span class="sxs-lookup"><span data-stu-id="25483-415">All types can be classified as either ***open types*** or ***closed types***.</span></span> <span data-ttu-id="25483-416">開啟型別是包含型別參數的類型。</span><span class="sxs-lookup"><span data-stu-id="25483-416">An open type is a type that involves type parameters.</span></span> <span data-ttu-id="25483-417">具體而言：</span><span class="sxs-lookup"><span data-stu-id="25483-417">More specifically:</span></span>

*  <span data-ttu-id="25483-418">型別參數會定義開啟型別。</span><span class="sxs-lookup"><span data-stu-id="25483-418">A type parameter defines an open type.</span></span>
*  <span data-ttu-id="25483-419">陣列型別是開放式類型，才其項目型別是開放式類型。</span><span class="sxs-lookup"><span data-stu-id="25483-419">An array type is an open type if and only if its element type is an open type.</span></span>
*  <span data-ttu-id="25483-420">建構的型別是開放式類型，才一或多個其型別引數是開啟型別。</span><span class="sxs-lookup"><span data-stu-id="25483-420">A constructed type is an open type if and only if one or more of its type arguments is an open type.</span></span> <span data-ttu-id="25483-421">建構的巢狀型別是開放式類型，才一或多個其型別引數或其包含類型的類型引數是開啟型別。</span><span class="sxs-lookup"><span data-stu-id="25483-421">A constructed nested type is an open type if and only if one or more of its type arguments or the type arguments of its containing type(s) is an open type.</span></span>

<span data-ttu-id="25483-422">封閉式的型別是不是開啟類型的類型。</span><span class="sxs-lookup"><span data-stu-id="25483-422">A closed type is a type that is not an open type.</span></span>

<span data-ttu-id="25483-423">在執行階段，所有的泛型類型宣告中的程式碼是藉由套用至泛型宣告的型別引數建立封閉式建構類型的內容中執行。</span><span class="sxs-lookup"><span data-stu-id="25483-423">At run-time, all of the code within a generic type declaration is executed in the context of a closed constructed type that was created by applying type arguments to the generic declaration.</span></span> <span data-ttu-id="25483-424">在泛型型別中的每個類型參數是繫結至特定的執行階段型別。</span><span class="sxs-lookup"><span data-stu-id="25483-424">Each type parameter within the generic type is bound to a particular run-time type.</span></span> <span data-ttu-id="25483-425">執行階段處理的所有陳述式及運算式永遠發生在封閉式的型別，並開啟類型只會在發生編譯時間處理。</span><span class="sxs-lookup"><span data-stu-id="25483-425">The run-time processing of all statements and expressions always occurs with closed types, and open types occur only during compile-time processing.</span></span>

<span data-ttu-id="25483-426">每個封閉式建構的類型都有它自己組不會與任何其他封閉式建構類型共用的靜態變數。</span><span class="sxs-lookup"><span data-stu-id="25483-426">Each closed constructed type has its own set of static variables, which are not shared with any other closed constructed types.</span></span> <span data-ttu-id="25483-427">由於開啟的型別不存在於執行階段中，有任何開放式型別相關聯的靜態變數。</span><span class="sxs-lookup"><span data-stu-id="25483-427">Since an open type does not exist at run-time, there are no static variables associated with an open type.</span></span> <span data-ttu-id="25483-428">兩個封閉式的建構型別都是相同類型，如果它們從相同的繫結泛型型別，建構，而且其對應的型別引數都是相同的型別。</span><span class="sxs-lookup"><span data-stu-id="25483-428">Two closed constructed types are the same type if they are constructed from the same unbound generic type, and their corresponding type arguments are the same type.</span></span>

### <a name="bound-and-unbound-types"></a><span data-ttu-id="25483-429">繫結和解除繫結類型</span><span class="sxs-lookup"><span data-stu-id="25483-429">Bound and unbound types</span></span>

<span data-ttu-id="25483-430">詞彙***未繫結的型別***是指非泛型類型或繫結的泛型型別。</span><span class="sxs-lookup"><span data-stu-id="25483-430">The term ***unbound type*** refers to a non-generic type or an unbound generic type.</span></span> <span data-ttu-id="25483-431">詞彙***繫結型別***是指非泛型類型或建構的型別。</span><span class="sxs-lookup"><span data-stu-id="25483-431">The term ***bound type*** refers to a non-generic type or a constructed type.</span></span>

<span data-ttu-id="25483-432">未繫結的類型是指型別宣告所宣告的實體。</span><span class="sxs-lookup"><span data-stu-id="25483-432">An unbound type refers to the entity declared by a type declaration.</span></span> <span data-ttu-id="25483-433">未繫結的泛型類型本身不是類型，並不能做為類型的變數、 引數或傳回值，或做為基底類型。</span><span class="sxs-lookup"><span data-stu-id="25483-433">An unbound generic type is not itself a type, and cannot be used as the type of a variable, argument or return value, or as a base type.</span></span> <span data-ttu-id="25483-434">唯一的建構可以參考繫結的泛型型別是`typeof`運算式 ([typeof 運算子](expressions.md#the-typeof-operator))。</span><span class="sxs-lookup"><span data-stu-id="25483-434">The only construct in which an unbound generic type can be referenced is the `typeof` expression ([The typeof operator](expressions.md#the-typeof-operator)).</span></span>

### <a name="satisfying-constraints"></a><span data-ttu-id="25483-435">滿足的條件約束</span><span class="sxs-lookup"><span data-stu-id="25483-435">Satisfying constraints</span></span>

<span data-ttu-id="25483-436">針對泛型類型或方法上宣告的型別參數條件約束時，參考建構的類型或泛型方法，會檢查提供的型別引數 ([類型參數條件約束](classes.md#type-parameter-constraints))。</span><span class="sxs-lookup"><span data-stu-id="25483-436">Whenever a constructed type or generic method is referenced, the supplied type arguments are checked against the type parameter constraints declared on the generic type or method ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="25483-437">每個`where`子句中，型別引數`A`，其對應至名為型別參數根據檢查每個條件約束，如下所示：</span><span class="sxs-lookup"><span data-stu-id="25483-437">For each `where` clause, the type argument `A` that corresponds to the named type parameter is checked against each constraint as follows:</span></span>

*  <span data-ttu-id="25483-438">如果類別型別、 介面型別或型別參數條件約束，可讓`C`代表使用提供的型別引數的條件約束取代任何出現在條件約束的型別參數。</span><span class="sxs-lookup"><span data-stu-id="25483-438">If the constraint is a class type, an interface type, or a type parameter, let `C` represent that constraint with the supplied type arguments substituted for any type parameters that appear in the constraint.</span></span> <span data-ttu-id="25483-439">若要滿足的條件約束，它必須的情況下，鍵入`A`可以轉換成類型`C`由下列其中之一：</span><span class="sxs-lookup"><span data-stu-id="25483-439">To satisfy the constraint, it must be the case that type `A` is convertible to type `C` by one of the following:</span></span>
    * <span data-ttu-id="25483-440">身分識別轉換 ([身分識別轉換](conversions.md#identity-conversion))</span><span class="sxs-lookup"><span data-stu-id="25483-440">An identity conversion ([Identity conversion](conversions.md#identity-conversion))</span></span>
    * <span data-ttu-id="25483-441">隱含參考轉換 ([隱含參考轉換](conversions.md#implicit-reference-conversions))</span><span class="sxs-lookup"><span data-stu-id="25483-441">An implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions))</span></span>
    * <span data-ttu-id="25483-442">Boxing 轉換 ([Boxing 轉換](conversions.md#boxing-conversions))，前提是型別 A 是不可為 null 的實值型別。</span><span class="sxs-lookup"><span data-stu-id="25483-442">A boxing conversion ([Boxing conversions](conversions.md#boxing-conversions)), provided that type A is a non-nullable value type.</span></span>
    * <span data-ttu-id="25483-443">從型別參數隱含參考、 boxing 或類型參數轉換`A`至`C`。</span><span class="sxs-lookup"><span data-stu-id="25483-443">An implicit reference, boxing or type parameter conversion from a type parameter `A` to `C`.</span></span>
*  <span data-ttu-id="25483-444">如果條件約束參考類型條件約束 (`class`)，型別`A`必須符合下列其中之一：</span><span class="sxs-lookup"><span data-stu-id="25483-444">If the constraint is the reference type constraint (`class`), the type `A` must satisfy one of the following:</span></span>
    * <span data-ttu-id="25483-445">`A` 是介面類型、 類別類型、 委派型別或陣列型別。</span><span class="sxs-lookup"><span data-stu-id="25483-445">`A` is an interface type, class type, delegate type or array type.</span></span> <span data-ttu-id="25483-446">請注意，`System.ValueType`和`System.Enum`是參考型別符合此條件約束。</span><span class="sxs-lookup"><span data-stu-id="25483-446">Note that `System.ValueType` and `System.Enum` are reference types that satisfy this constraint.</span></span>
    * <span data-ttu-id="25483-447">`A` 已知為參考型別為類型參數 ([類型參數條件約束](classes.md#type-parameter-constraints))。</span><span class="sxs-lookup"><span data-stu-id="25483-447">`A` is a type parameter that is known to be a reference type ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
*  <span data-ttu-id="25483-448">如果條件約束的值類型條件約束 (`struct`)，型別`A`必須符合下列其中之一：</span><span class="sxs-lookup"><span data-stu-id="25483-448">If the constraint is the value type constraint (`struct`), the type `A` must satisfy one of the following:</span></span>
    * <span data-ttu-id="25483-449">`A` 為結構類型或列舉型別，但不是可為 null 的型別。</span><span class="sxs-lookup"><span data-stu-id="25483-449">`A` is a struct type or enum type, but not a nullable type.</span></span> <span data-ttu-id="25483-450">請注意，`System.ValueType`和`System.Enum`是參考型別不符合此條件約束。</span><span class="sxs-lookup"><span data-stu-id="25483-450">Note that `System.ValueType` and `System.Enum` are reference types that do not satisfy this constraint.</span></span>
    * <span data-ttu-id="25483-451">`A` 是具有實值類型條件約束的型別參數 ([類型參數條件約束](classes.md#type-parameter-constraints))。</span><span class="sxs-lookup"><span data-stu-id="25483-451">`A` is a type parameter having the value type constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
*  <span data-ttu-id="25483-452">如果條件約束是建構函式的條件約束`new()`，型別`A`不得`abstract`，而且必須具有公用的無參數建構函式。</span><span class="sxs-lookup"><span data-stu-id="25483-452">If the constraint is the constructor constraint `new()`, the type `A` must not be `abstract` and must have a public parameterless constructor.</span></span> <span data-ttu-id="25483-453">這被符合下列其中一項，則為 true 時：</span><span class="sxs-lookup"><span data-stu-id="25483-453">This is satisfied if one of the following is true:</span></span>
    * <span data-ttu-id="25483-454">`A` 是實值類型，因為所有的值類型都有一個公用預設建構函式 ([預設建構函式](types.md#default-constructors))。</span><span class="sxs-lookup"><span data-stu-id="25483-454">`A` is a value type, since all value types have a public default constructor ([Default constructors](types.md#default-constructors)).</span></span>
    * <span data-ttu-id="25483-455">`A` 是具有建構函式條件約束的型別參數 ([類型參數條件約束](classes.md#type-parameter-constraints))。</span><span class="sxs-lookup"><span data-stu-id="25483-455">`A` is a type parameter having the constructor constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
    * <span data-ttu-id="25483-456">`A` 是具有實值類型條件約束的型別參數 ([類型參數條件約束](classes.md#type-parameter-constraints))。</span><span class="sxs-lookup"><span data-stu-id="25483-456">`A` is a type parameter having the value type constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
    * <span data-ttu-id="25483-457">`A` 是不是類別`abstract`且包含明確宣告`public`不含任何參數的建構函式。</span><span class="sxs-lookup"><span data-stu-id="25483-457">`A` is a class that is not `abstract` and contains an explicitly declared `public` constructor with no parameters.</span></span>
    * <span data-ttu-id="25483-458">`A` 不是`abstract`且具有預設建構函式 ([預設建構函式](classes.md#default-constructors))。</span><span class="sxs-lookup"><span data-stu-id="25483-458">`A` is not `abstract` and has a default constructor ([Default constructors](classes.md#default-constructors)).</span></span>

<span data-ttu-id="25483-459">如果指定的型別引數不符合一或多個類型參數的條件約束，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="25483-459">A compile-time error occurs if one or more of a type parameter's constraints are not satisfied by the given type arguments.</span></span>

<span data-ttu-id="25483-460">因為不繼承型別參數條件約束也不可能繼承而來。</span><span class="sxs-lookup"><span data-stu-id="25483-460">Since type parameters are not inherited, constraints are never inherited either.</span></span> <span data-ttu-id="25483-461">在下列範例中，`D`必須指定其類型參數條件約束`T`以便`T`滿足條件約束的基底類別所加諸`B<T>`。</span><span class="sxs-lookup"><span data-stu-id="25483-461">In the example below, `D` needs to specify the constraint on its type parameter `T` so that `T` satisfies the constraint imposed by the base class `B<T>`.</span></span> <span data-ttu-id="25483-462">相反地，類別`E`因為，則不需要指定的條件約束`List<T>`實作`IEnumerable`任何`T`。</span><span class="sxs-lookup"><span data-stu-id="25483-462">In contrast, class `E` need not specify a constraint, because `List<T>` implements `IEnumerable` for any `T`.</span></span>

```csharp
class B<T> where T: IEnumerable {...}

class D<T>: B<T> where T: IEnumerable {...}

class E<T>: B<List<T>> {...}
```

## <a name="type-parameters"></a><span data-ttu-id="25483-463">型別參數</span><span class="sxs-lookup"><span data-stu-id="25483-463">Type parameters</span></span>

<span data-ttu-id="25483-464">型別參數是指定實值類型或參考型別參數所繫結在執行階段的識別碼。</span><span class="sxs-lookup"><span data-stu-id="25483-464">A type parameter is an identifier designating a value type or reference type that the parameter is bound to at run-time.</span></span>

```antlr
type_parameter
    : identifier
    ;
```

<span data-ttu-id="25483-465">類型參數可以使用許多不同的實際型別引數具現化，因為型別參數會有稍微不同的作業與比其他類型的限制。</span><span class="sxs-lookup"><span data-stu-id="25483-465">Since a type parameter can be instantiated with many different actual type arguments, type parameters have slightly different operations and restrictions than other types.</span></span> <span data-ttu-id="25483-466">它們包括：</span><span class="sxs-lookup"><span data-stu-id="25483-466">These include:</span></span>

*  <span data-ttu-id="25483-467">型別參數不能直接以宣告的基底類別 ([基底類別](classes.md#base-class)) 或介面 ([Variant 類型參數清單](interfaces.md#variant-type-parameter-lists))。</span><span class="sxs-lookup"><span data-stu-id="25483-467">A type parameter cannot be used directly to declare a base class ([Base class](classes.md#base-class)) or interface ([Variant type parameter lists](interfaces.md#variant-type-parameter-lists)).</span></span>
*  <span data-ttu-id="25483-468">參數取決於條件約束，如果有的話，成員查詢型別上的規則套用至型別參數。</span><span class="sxs-lookup"><span data-stu-id="25483-468">The rules for member lookup on type parameters depend on the constraints, if any, applied to the type parameter.</span></span> <span data-ttu-id="25483-469">它們會詳述[成員查閱](expressions.md#member-lookup)。</span><span class="sxs-lookup"><span data-stu-id="25483-469">They are detailed in [Member lookup](expressions.md#member-lookup).</span></span>
*  <span data-ttu-id="25483-470">可用的轉換型別參數取決於條件約束，如果有的話，套用至型別參數。</span><span class="sxs-lookup"><span data-stu-id="25483-470">The available conversions for a type parameter depend on the constraints, if any, applied to the type parameter.</span></span> <span data-ttu-id="25483-471">它們會詳述[涉及型別參數的隱含轉換](conversions.md#implicit-conversions-involving-type-parameters)並[明確的動態轉換](conversions.md#explicit-dynamic-conversions)。</span><span class="sxs-lookup"><span data-stu-id="25483-471">They are detailed in [Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters) and [Explicit dynamic conversions](conversions.md#explicit-dynamic-conversions).</span></span>
*  <span data-ttu-id="25483-472">常值`null`無法轉換成指定型別參數，但如果已知型別參數是參考類型的類型 ([涉及型別參數的隱含轉換](conversions.md#implicit-conversions-involving-type-parameters))。</span><span class="sxs-lookup"><span data-stu-id="25483-472">The literal `null` cannot be converted to a type given by a type parameter, except if the type parameter is known to be a reference type ([Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters)).</span></span> <span data-ttu-id="25483-473">不過，`default`運算式 ([預設值運算式](expressions.md#default-value-expressions)) 可以使用。</span><span class="sxs-lookup"><span data-stu-id="25483-473">However, a `default` expression ([Default value expressions](expressions.md#default-value-expressions)) can be used instead.</span></span> <span data-ttu-id="25483-474">此外，要有類型參數所指定類型的值與比較`null`使用`==`並`!=`([參考型別等號比較運算子](expressions.md#reference-type-equality-operators)) 除非型別參數具有實值類型條件約束。</span><span class="sxs-lookup"><span data-stu-id="25483-474">In addition, a value with a type given by a type parameter can be compared with `null` using `==` and `!=` ([Reference type equality operators](expressions.md#reference-type-equality-operators)) unless the type parameter has the value type constraint.</span></span>
*  <span data-ttu-id="25483-475">A`new`運算式 ([物件建立運算式](expressions.md#object-creation-expressions)) 僅適用於具有型別參數如果類型參數受到*constructor_constraint*或實值類型條件約束 ([類型參數條件約束](classes.md#type-parameter-constraints))。</span><span class="sxs-lookup"><span data-stu-id="25483-475">A `new` expression ([Object creation expressions](expressions.md#object-creation-expressions)) can only be used with a type parameter if the type parameter is constrained by a *constructor_constraint* or the value type constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
*  <span data-ttu-id="25483-476">型別參數無法使用屬性中的任何位置。</span><span class="sxs-lookup"><span data-stu-id="25483-476">A type parameter cannot be used anywhere within an attribute.</span></span>
*  <span data-ttu-id="25483-477">型別參數不能在成員存取 ([成員存取](expressions.md#member-access)) 或型別名稱 ([命名空間和型別名稱](basic-concepts.md#namespace-and-type-names)) 來識別的靜態成員或巢狀型別。</span><span class="sxs-lookup"><span data-stu-id="25483-477">A type parameter cannot be used in a member access ([Member access](expressions.md#member-access)) or type name ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) to identify a static member or a nested type.</span></span>
*  <span data-ttu-id="25483-478">在不安全的程式碼中的型別參數不能做*unmanaged_type* ([指標類型](unsafe-code.md#pointer-types))。</span><span class="sxs-lookup"><span data-stu-id="25483-478">In unsafe code, a type parameter cannot be used as an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

<span data-ttu-id="25483-479">做為類型，類型參數都是單純的編譯時期建構。</span><span class="sxs-lookup"><span data-stu-id="25483-479">As a type, type parameters are purely a compile-time construct.</span></span> <span data-ttu-id="25483-480">在執行階段，每個類型參數是繫結至所提供的泛型型別宣告的類型引數指定的執行階段類型。</span><span class="sxs-lookup"><span data-stu-id="25483-480">At run-time, each type parameter is bound to a run-time type that was specified by supplying a type argument to the generic type declaration.</span></span> <span data-ttu-id="25483-481">因此，在執行階段型別參數會以宣告的變數的型別是封閉式的建構的類型 ([開放和封閉類型](types.md#open-and-closed-types))。</span><span class="sxs-lookup"><span data-stu-id="25483-481">Thus, the type of a variable declared with a type parameter will, at run-time, be a closed constructed type ([Open and closed types](types.md#open-and-closed-types)).</span></span> <span data-ttu-id="25483-482">執行階段執行的所有陳述式和包含型別參數的運算式會使用提供的實際型別為該參數的型別引數。</span><span class="sxs-lookup"><span data-stu-id="25483-482">The run-time execution of all statements and expressions involving type parameters uses the actual type that was supplied as the type argument for that parameter.</span></span>

## <a name="expression-tree-types"></a><span data-ttu-id="25483-483">運算式樹狀架構類型</span><span class="sxs-lookup"><span data-stu-id="25483-483">Expression tree types</span></span>

<span data-ttu-id="25483-484">***運算式樹狀架構***允許 lambda 運算式表示為資料結構，而不是可執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="25483-484">***Expression trees*** permit lambda expressions to be represented as data structures instead of executable code.</span></span> <span data-ttu-id="25483-485">運算式樹狀架構為值***運算式樹狀架構類型***的表單`System.Linq.Expressions.Expression<D>`，其中`D`是任何委派型別。</span><span class="sxs-lookup"><span data-stu-id="25483-485">Expression trees are values of ***expression tree types*** of the form `System.Linq.Expressions.Expression<D>`, where `D` is any delegate type.</span></span> <span data-ttu-id="25483-486">此規格的其餘部分中，我們會參考使用速記時這些類型`Expression<D>`。</span><span class="sxs-lookup"><span data-stu-id="25483-486">For the remainder of this specification we will refer to these types using the shorthand `Expression<D>`.</span></span>

<span data-ttu-id="25483-487">如果有從 lambda 運算式的委派型別`D`，轉換也存在於運算式樹狀架構型別`Expression<D>`。</span><span class="sxs-lookup"><span data-stu-id="25483-487">If a conversion exists from a lambda expression to a delegate type `D`, a conversion also exists to the expression tree type `Expression<D>`.</span></span> <span data-ttu-id="25483-488">轉換成委派類型的 lambda 運算式會產生參考 lambda 運算式的可執行程式碼的委派，而轉換為運算式樹狀結構類型會建立 lambda 運算式的運算式樹狀結構表示法。</span><span class="sxs-lookup"><span data-stu-id="25483-488">Whereas the conversion of a lambda expression to a delegate type generates a delegate that references executable code for the lambda expression, conversion to an expression tree type creates an expression tree representation of the lambda expression.</span></span>

<span data-ttu-id="25483-489">運算式樹狀架構是 lambda 運算式的高效率的記憶體內部資料表示法，以及讓透明且明確的 lambda 運算式的結構。</span><span class="sxs-lookup"><span data-stu-id="25483-489">Expression trees are efficient in-memory data representations of lambda expressions and make the structure of the lambda expression transparent and explicit.</span></span>

<span data-ttu-id="25483-490">就像委派型別`D`，`Expression<D>`即具有參數和傳回型別，這是一樣的`D`。</span><span class="sxs-lookup"><span data-stu-id="25483-490">Just like a delegate type `D`, `Expression<D>` is said to have parameter and return types, which are the same as those of `D`.</span></span>

<span data-ttu-id="25483-491">下列範例代表 lambda 運算式作為可執行程式碼和運算式樹狀架構。</span><span class="sxs-lookup"><span data-stu-id="25483-491">The following example represents a lambda expression both as executable code and as an expression tree.</span></span> <span data-ttu-id="25483-492">因為轉換成型`Func<int,int>`，也別轉換`Expression<Func<int,int>>`:</span><span class="sxs-lookup"><span data-stu-id="25483-492">Because a conversion exists to `Func<int,int>`, a conversion also exists to `Expression<Func<int,int>>`:</span></span>

```csharp
Func<int,int> del = x => x + 1;                    // Code

Expression<Func<int,int>> exp = x => x + 1;        // Data
```

<span data-ttu-id="25483-493">遵循這些指派，委派`del`參考方法會傳回`x + 1`，並將運算式樹狀架構`exp`參考的資料結構，描述運算式`x => x + 1`。</span><span class="sxs-lookup"><span data-stu-id="25483-493">Following these assignments, the delegate `del` references a method that returns `x + 1`, and the expression tree `exp` references a data structure that describes the expression `x => x + 1`.</span></span>

<span data-ttu-id="25483-494">泛型類型的確切定義`Expression<D>`建構運算式樹狀架構 lambda 運算式轉換成運算式樹狀架構型別時的精確規則，以及都超出此規格的範圍。</span><span class="sxs-lookup"><span data-stu-id="25483-494">The exact definition of the generic type `Expression<D>` as well as the precise rules for constructing an expression tree when a lambda expression is converted to an expression tree type, are both outside the scope of this specification.</span></span>

<span data-ttu-id="25483-495">兩件事是，您必須明確的：</span><span class="sxs-lookup"><span data-stu-id="25483-495">Two things are important to make explicit:</span></span>

*  <span data-ttu-id="25483-496">並非所有的 lambda 運算式可以轉換成運算式樹狀架構。</span><span class="sxs-lookup"><span data-stu-id="25483-496">Not all lambda expressions can be converted to expression trees.</span></span> <span data-ttu-id="25483-497">比方說，無法表示 lambda 運算式與陳述式主體，並包含指派運算式的 lambda 運算式。</span><span class="sxs-lookup"><span data-stu-id="25483-497">For instance, lambda expressions with statement bodies, and lambda expressions containing assignment expressions cannot be represented.</span></span> <span data-ttu-id="25483-498">在這些情況下，轉換仍然存在，但在編譯時期將會失敗。</span><span class="sxs-lookup"><span data-stu-id="25483-498">In these cases, a conversion still exists, but will fail at compile-time.</span></span> <span data-ttu-id="25483-499">這些例外狀況的詳細[匿名函式轉換](conversions.md#anonymous-function-conversions)。</span><span class="sxs-lookup"><span data-stu-id="25483-499">These exceptions are detailed in [Anonymous function conversions](conversions.md#anonymous-function-conversions).</span></span>
*   <span data-ttu-id="25483-500">`Expression<D>` 提供執行個體方法`Compile`這會產生之型別的委派`D`:</span><span class="sxs-lookup"><span data-stu-id="25483-500">`Expression<D>` offers an instance method `Compile` which produces a delegate of type `D`:</span></span>

    ```csharp
    Func<int,int> del2 = exp.Compile();
    ```

    <span data-ttu-id="25483-501">叫用此委派會導致執行運算式樹狀架構所表示的程式碼。</span><span class="sxs-lookup"><span data-stu-id="25483-501">Invoking this delegate causes the code represented by the expression tree to be executed.</span></span> <span data-ttu-id="25483-502">因此，假設上述的定義，del 和 del2 相等，然後下列兩個陳述式將會有相同的效果：</span><span class="sxs-lookup"><span data-stu-id="25483-502">Thus, given the definitions above, del and del2 are equivalent, and the following two statements will have the same effect:</span></span>

    ```csharp
    int i1 = del(1);
    
    int i2 = del2(1);
    ```

    <span data-ttu-id="25483-503">在執行此程式碼之後,`i1`並`i2`都會有值`2`。</span><span class="sxs-lookup"><span data-stu-id="25483-503">After executing this code,  `i1` and `i2` will both have the value `2`.</span></span>

