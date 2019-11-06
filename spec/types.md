---
ms.openlocfilehash: 088c4a77cecde490c556c44c239a3496f896582e
ms.sourcegitcommit: 4ddf18d000734c1b6d0a48127bf338086fc3f2c3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2019
ms.locfileid: "73616132"
---
# <a name="types"></a><span data-ttu-id="f034e-101">型別</span><span class="sxs-lookup"><span data-stu-id="f034e-101">Types</span></span>

<span data-ttu-id="f034e-102">C#語言的類型分成兩個主要類別：實***數值型別***和***參考型別***。</span><span class="sxs-lookup"><span data-stu-id="f034e-102">The types of the C# language are divided into two main categories: ***value types*** and ***reference types***.</span></span> <span data-ttu-id="f034e-103">實數值型別和參考型別都可以是***泛型型別***，這會採用一或多個***類型參數***。</span><span class="sxs-lookup"><span data-stu-id="f034e-103">Both value types and reference types may be ***generic types***, which take one or more ***type parameters***.</span></span> <span data-ttu-id="f034e-104">類型參數可以同時指定實數值型別和參考型別。</span><span class="sxs-lookup"><span data-stu-id="f034e-104">Type parameters can designate both value types and reference types.</span></span>

```antlr
type
    : value_type
    | reference_type
    | type_parameter
    | type_unsafe
    ;
```

<span data-ttu-id="f034e-105">類型、指標的最終分類僅適用于 unsafe 程式碼。</span><span class="sxs-lookup"><span data-stu-id="f034e-105">The final category of types, pointers, is available only in unsafe code.</span></span> <span data-ttu-id="f034e-106">這會在[指標類型](unsafe-code.md#pointer-types)中進一步討論。</span><span class="sxs-lookup"><span data-stu-id="f034e-106">This is discussed further in [Pointer types](unsafe-code.md#pointer-types).</span></span>

<span data-ttu-id="f034e-107">實值型別與參考型別不同的是，實值型別的變數會直接包含其資料，而參考型別的變數則會儲存其資料的***參考***，後者又稱為***物件***。</span><span class="sxs-lookup"><span data-stu-id="f034e-107">Value types differ from reference types in that variables of the value types directly contain their data, whereas variables of the reference types store ***references*** to their data, the latter being known as ***objects***.</span></span> <span data-ttu-id="f034e-108">有了參考型別，兩個變數都可以參考相同的物件，因此在某個變數上的作業可能會影響另一個變數所參考的物件。</span><span class="sxs-lookup"><span data-stu-id="f034e-108">With reference types, it is possible for two variables to reference the same object, and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="f034e-109">使用實值型別時，每個變數都有自己的資料複本，而且其中一個的作業不可能影響另一個。</span><span class="sxs-lookup"><span data-stu-id="f034e-109">With value types, the variables each have their own copy of the data, and it is not possible for operations on one to affect the other.</span></span>

<span data-ttu-id="f034e-110">C#的類型系統是統一的，因此可以將任何類型的值視為物件。</span><span class="sxs-lookup"><span data-stu-id="f034e-110">C#'s type system is unified such that a value of any type can be treated as an object.</span></span> <span data-ttu-id="f034e-111">C# 中的每個型別都直接或間接衍生自 `object` 類別型別，而 `object` 是所有型別的基底類別。</span><span class="sxs-lookup"><span data-stu-id="f034e-111">Every type in C# directly or indirectly derives from the `object` class type, and `object` is the ultimate base class of all types.</span></span> <span data-ttu-id="f034e-112">參考型別的值之所以會視為物件，只是將這些值當作 `object` 型別來檢視。</span><span class="sxs-lookup"><span data-stu-id="f034e-112">Values of reference types are treated as objects simply by viewing the values as type `object`.</span></span> <span data-ttu-id="f034e-113">實數值型別的值會藉由執行裝箱和取消裝箱作業（[裝箱和取消](types.md#boxing-and-unboxing)裝箱）來視為物件。</span><span class="sxs-lookup"><span data-stu-id="f034e-113">Values of value types are treated as objects by performing boxing and unboxing operations ([Boxing and unboxing](types.md#boxing-and-unboxing)).</span></span>

## <a name="value-types"></a><span data-ttu-id="f034e-114">值類型</span><span class="sxs-lookup"><span data-stu-id="f034e-114">Value types</span></span>

<span data-ttu-id="f034e-115">實值型別可以是結構型別或列舉型別。</span><span class="sxs-lookup"><span data-stu-id="f034e-115">A value type is either a struct type or an enumeration type.</span></span> <span data-ttu-id="f034e-116">C#提供一組稱為***簡單類型***的預先定義結構類型。</span><span class="sxs-lookup"><span data-stu-id="f034e-116">C# provides a set of predefined struct types called the ***simple types***.</span></span> <span data-ttu-id="f034e-117">簡單類型是透過保留字來識別。</span><span class="sxs-lookup"><span data-stu-id="f034e-117">The simple types are identified through reserved words.</span></span>

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

<span data-ttu-id="f034e-118">不同于參考型別的變數，實數值型別的變數只有在實數值型別是可為 null 的類型時，才可以包含值 `null`。</span><span class="sxs-lookup"><span data-stu-id="f034e-118">Unlike a variable of a reference type, a variable of a value type can contain the value `null` only if the value type is a nullable type.</span></span>  <span data-ttu-id="f034e-119">針對每一個不可為 null 的實數值型別，有一個對應的可為 null 實數值型別，表示相同的值集加上 `null`的值。</span><span class="sxs-lookup"><span data-stu-id="f034e-119">For every non-nullable value type there is a corresponding nullable value type denoting the same set of values plus the value `null`.</span></span>

<span data-ttu-id="f034e-120">指派至實數值型別的變數會建立所指派值的複本。</span><span class="sxs-lookup"><span data-stu-id="f034e-120">Assignment to a variable of a value type creates a copy of the value being assigned.</span></span> <span data-ttu-id="f034e-121">這與指派給參考型別的變數不同，後者會複製參考，而不是參考所識別的物件。</span><span class="sxs-lookup"><span data-stu-id="f034e-121">This differs from assignment to a variable of a reference type, which copies the reference but not the object identified by the reference.</span></span>

### <a name="the-systemvaluetype-type"></a><span data-ttu-id="f034e-122">System.object 類型</span><span class="sxs-lookup"><span data-stu-id="f034e-122">The System.ValueType type</span></span>

<span data-ttu-id="f034e-123">所有實值型別都會隱含繼承自類別 `System.ValueType`，而這又繼承自類別 `object`。</span><span class="sxs-lookup"><span data-stu-id="f034e-123">All value types implicitly inherit from the class `System.ValueType`, which, in turn, inherits from class `object`.</span></span> <span data-ttu-id="f034e-124">任何類型都無法衍生自實數值型別，而實數值型別因此隱含密封（[密封類別](classes.md#sealed-classes)）。</span><span class="sxs-lookup"><span data-stu-id="f034e-124">It is not possible for any type to derive from a value type, and value types are thus implicitly sealed ([Sealed classes](classes.md#sealed-classes)).</span></span>

<span data-ttu-id="f034e-125">請注意，`System.ValueType` 本身並不是*value_type*。</span><span class="sxs-lookup"><span data-stu-id="f034e-125">Note that `System.ValueType` is not itself a *value_type*.</span></span> <span data-ttu-id="f034e-126">相反地，它是自動衍生所有*value_type*的*class_type* 。</span><span class="sxs-lookup"><span data-stu-id="f034e-126">Rather, it is a *class_type* from which all *value_type*s are automatically derived.</span></span>

### <a name="default-constructors"></a><span data-ttu-id="f034e-127">預設建構函式</span><span class="sxs-lookup"><span data-stu-id="f034e-127">Default constructors</span></span>

<span data-ttu-id="f034e-128">所有實值型別會隱含地宣告一個稱為***預設***的函式的公用無參數實例的函數。</span><span class="sxs-lookup"><span data-stu-id="f034e-128">All value types implicitly declare a public parameterless instance constructor called the ***default constructor***.</span></span> <span data-ttu-id="f034e-129">預設的函式會傳回零初始化的實例，稱為實數值型別的***預設值***：</span><span class="sxs-lookup"><span data-stu-id="f034e-129">The default constructor returns a zero-initialized instance known as the ***default value*** for the value type:</span></span>

*  <span data-ttu-id="f034e-130">針對所有*simple_type*s，預設值為所有零的位模式所產生的值：</span><span class="sxs-lookup"><span data-stu-id="f034e-130">For all *simple_type*s, the default value is the value produced by a bit pattern of all zeros:</span></span>
    * <span data-ttu-id="f034e-131">針對 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`和 `ulong`，預設值為 [`0`]。</span><span class="sxs-lookup"><span data-stu-id="f034e-131">For `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, and `ulong`, the default value is `0`.</span></span>
    * <span data-ttu-id="f034e-132">針對 `char`，預設值為 `'\x0000'`。</span><span class="sxs-lookup"><span data-stu-id="f034e-132">For `char`, the default value is `'\x0000'`.</span></span>
    * <span data-ttu-id="f034e-133">針對 `float`，預設值為 `0.0f`。</span><span class="sxs-lookup"><span data-stu-id="f034e-133">For `float`, the default value is `0.0f`.</span></span>
    * <span data-ttu-id="f034e-134">針對 `double`，預設值為 `0.0d`。</span><span class="sxs-lookup"><span data-stu-id="f034e-134">For `double`, the default value is `0.0d`.</span></span>
    * <span data-ttu-id="f034e-135">針對 `decimal`，預設值為 `0.0m`。</span><span class="sxs-lookup"><span data-stu-id="f034e-135">For `decimal`, the default value is `0.0m`.</span></span>
    * <span data-ttu-id="f034e-136">針對 `bool`，預設值為 `false`。</span><span class="sxs-lookup"><span data-stu-id="f034e-136">For `bool`, the default value is `false`.</span></span>
*  <span data-ttu-id="f034e-137">針對*enum_type* `E`，預設值為 `0`，轉換為類型 `E`。</span><span class="sxs-lookup"><span data-stu-id="f034e-137">For an *enum_type* `E`, the default value is `0`, converted to the type `E`.</span></span>
*  <span data-ttu-id="f034e-138">對於*struct_type*，預設值是將所有實值型別字段設定為其預設值，以及要 `null`的所有參考型別字段所產生的值。</span><span class="sxs-lookup"><span data-stu-id="f034e-138">For a *struct_type*, the default value is the value produced by setting all value type fields to their default value and all reference type fields to `null`.</span></span>
*  <span data-ttu-id="f034e-139">對於*nullable_type* ，預設值是 `HasValue` 屬性為 false 且未定義 `Value` 屬性的實例。</span><span class="sxs-lookup"><span data-stu-id="f034e-139">For a *nullable_type* the default value is an instance for which the `HasValue` property is false and the `Value` property is undefined.</span></span> <span data-ttu-id="f034e-140">預設值也稱為可為 null 類型的***null 值***。</span><span class="sxs-lookup"><span data-stu-id="f034e-140">The default value is also known as the ***null value*** of the nullable type.</span></span>

<span data-ttu-id="f034e-141">就像任何其他實例的函式一樣，實值型別的預設的函數會使用 `new` 運算子來叫用。</span><span class="sxs-lookup"><span data-stu-id="f034e-141">Like any other instance constructor, the default constructor of a value type is invoked using the `new` operator.</span></span> <span data-ttu-id="f034e-142">基於效率的考慮，這項需求並不是要實際讓執行產生一個函式呼叫。</span><span class="sxs-lookup"><span data-stu-id="f034e-142">For efficiency reasons, this requirement is not intended to actually have the implementation generate a constructor call.</span></span> <span data-ttu-id="f034e-143">在下列範例中，`i` 和 `j` 的變數都會初始化為零。</span><span class="sxs-lookup"><span data-stu-id="f034e-143">In the example below, variables `i` and `j` are both initialized to zero.</span></span>

```csharp
class A
{
    void F() {
        int i = 0;
        int j = new int();
    }
}
```

<span data-ttu-id="f034e-144">由於每個實數值型別都隱含具有公用無參數實例的函式，因此結構類型不可能包含無參數之函數的明確宣告。</span><span class="sxs-lookup"><span data-stu-id="f034e-144">Because every value type implicitly has a public parameterless instance constructor, it is not possible for a struct type to contain an explicit declaration of a parameterless constructor.</span></span> <span data-ttu-id="f034e-145">不過，結構類型允許宣告參數化實例的函式[（函](structs.md#constructors)式）。</span><span class="sxs-lookup"><span data-stu-id="f034e-145">A struct type is however permitted to declare parameterized instance constructors ([Constructors](structs.md#constructors)).</span></span>

### <a name="struct-types"></a><span data-ttu-id="f034e-146">結構型別</span><span class="sxs-lookup"><span data-stu-id="f034e-146">Struct types</span></span>

<span data-ttu-id="f034e-147">結構類型是實數值型別，可以宣告常數、欄位、方法、屬性、索引子、運算子、實例的函式、靜態的函式和巢狀型別。</span><span class="sxs-lookup"><span data-stu-id="f034e-147">A struct type is a value type that can declare constants, fields, methods, properties, indexers, operators, instance constructors, static constructors, and nested types.</span></span> <span data-ttu-id="f034e-148">結構類型的宣告會在[結構](structs.md#struct-declarations)宣告中說明。</span><span class="sxs-lookup"><span data-stu-id="f034e-148">The declaration of struct types is described in [Struct declarations](structs.md#struct-declarations).</span></span>

### <a name="simple-types"></a><span data-ttu-id="f034e-149">簡單型別</span><span class="sxs-lookup"><span data-stu-id="f034e-149">Simple types</span></span>

<span data-ttu-id="f034e-150">C#提供一組稱為***簡單類型***的預先定義結構類型。</span><span class="sxs-lookup"><span data-stu-id="f034e-150">C# provides a set of predefined struct types called the ***simple types***.</span></span> <span data-ttu-id="f034e-151">簡單類型是透過保留字來識別，但這些保留字只是 `System` 命名空間中預先定義結構類型的別名，如下表所述。</span><span class="sxs-lookup"><span data-stu-id="f034e-151">The simple types are identified through reserved words, but these reserved words are simply aliases for predefined struct types in the `System` namespace, as described in the table below.</span></span>


| <span data-ttu-id="f034e-152">__保留字__</span><span class="sxs-lookup"><span data-stu-id="f034e-152">__Reserved word__</span></span> | <span data-ttu-id="f034e-153">__別名類型__</span><span class="sxs-lookup"><span data-stu-id="f034e-153">__Aliased type__</span></span> |
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

<span data-ttu-id="f034e-154">因為簡單型別會將結構型別做為別名，所以每個簡單型別都有成員。</span><span class="sxs-lookup"><span data-stu-id="f034e-154">Because a simple type aliases a struct type, every simple type has members.</span></span> <span data-ttu-id="f034e-155">例如，`int` 具有在 `System.Int32` 中宣告的成員，以及繼承自 `System.Object`的成員，且允許下列語句：</span><span class="sxs-lookup"><span data-stu-id="f034e-155">For example, `int` has the members declared in `System.Int32` and the members inherited from `System.Object`, and the following statements are permitted:</span></span>

```csharp
int i = int.MaxValue;           // System.Int32.MaxValue constant
string s = i.ToString();        // System.Int32.ToString() instance method
string t = 123.ToString();      // System.Int32.ToString() instance method
```

<span data-ttu-id="f034e-156">簡單型別與其他結構型別的差異在於它們可允許某些額外的作業：</span><span class="sxs-lookup"><span data-stu-id="f034e-156">The simple types differ from other struct types in that they permit certain additional operations:</span></span>

*  <span data-ttu-id="f034e-157">大部分的簡單類型都允許藉由撰寫*常*值（[常](lexical-structure.md#literals)值）來建立值。</span><span class="sxs-lookup"><span data-stu-id="f034e-157">Most simple types permit values to be created by writing *literals* ([Literals](lexical-structure.md#literals)).</span></span> <span data-ttu-id="f034e-158">例如，`123` 是 `int` 類型的常值，而 `'a'` 是 `char`類型的常值。</span><span class="sxs-lookup"><span data-stu-id="f034e-158">For example, `123` is a literal of type `int` and `'a'` is a literal of type `char`.</span></span> <span data-ttu-id="f034e-159">C#一般不會布建結構類型的常值，而且其他結構類型的非預設值最後一律會透過這些結構類型的實例構造函式來建立。</span><span class="sxs-lookup"><span data-stu-id="f034e-159">C# makes no provision for literals of struct types in general, and non-default values of other struct types are ultimately always created through instance constructors of those struct types.</span></span>
*  <span data-ttu-id="f034e-160">當運算式的運算元都是簡單類型常數時，編譯器可以在編譯時期評估運算式。</span><span class="sxs-lookup"><span data-stu-id="f034e-160">When the operands of an expression are all simple type constants, it is possible for the compiler to evaluate the expression at compile-time.</span></span> <span data-ttu-id="f034e-161">這類運算式稱為*constant_expression* （[常數運算式](expressions.md#constant-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="f034e-161">Such an expression is known as a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)).</span></span> <span data-ttu-id="f034e-162">涉及其他結構類型所定義之運算子的運算式，不會被視為常數運算式。</span><span class="sxs-lookup"><span data-stu-id="f034e-162">Expressions involving operators defined by other struct types are not considered to be constant expressions.</span></span>
*  <span data-ttu-id="f034e-163">透過 `const` 宣告，可以宣告簡單類型（[常數](classes.md#constants)）的常數。</span><span class="sxs-lookup"><span data-stu-id="f034e-163">Through `const` declarations it is possible to declare constants of the simple types ([Constants](classes.md#constants)).</span></span> <span data-ttu-id="f034e-164">不可能有其他結構類型的常數，但 `static readonly` 欄位會提供類似的效果。</span><span class="sxs-lookup"><span data-stu-id="f034e-164">It is not possible to have constants of other struct types, but a similar effect is provided by `static readonly` fields.</span></span>
*  <span data-ttu-id="f034e-165">包含簡單類型的轉換可以參與評估其他結構類型所定義的轉換運算子，但使用者定義的轉換運算子不能參與其他使用者定義運算子的評估（[評估使用者定義的轉換](conversions.md#evaluation-of-user-defined-conversions)）。</span><span class="sxs-lookup"><span data-stu-id="f034e-165">Conversions involving simple types can participate in evaluation of conversion operators defined by other struct types, but a user-defined conversion operator can never participate in evaluation of another user-defined operator ([Evaluation of user-defined conversions](conversions.md#evaluation-of-user-defined-conversions)).</span></span>

### <a name="integral-types"></a><span data-ttu-id="f034e-166">整數類資料型別</span><span class="sxs-lookup"><span data-stu-id="f034e-166">Integral types</span></span>

<span data-ttu-id="f034e-167">C#支援九種整數類型： `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`和 `char`。</span><span class="sxs-lookup"><span data-stu-id="f034e-167">C# supports nine integral types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, and `char`.</span></span> <span data-ttu-id="f034e-168">整數類資料類型具有下列大小和範圍的值：</span><span class="sxs-lookup"><span data-stu-id="f034e-168">The integral types have the following sizes and ranges of values:</span></span>

*  <span data-ttu-id="f034e-169">`sbyte` 類型代表帶正負號的8位整數，其值介於-128 和127之間。</span><span class="sxs-lookup"><span data-stu-id="f034e-169">The `sbyte` type represents signed 8-bit integers with values between -128 and 127.</span></span>
*  <span data-ttu-id="f034e-170">`byte` 類型代表不帶正負號的8位整數，其值介於0到255之間。</span><span class="sxs-lookup"><span data-stu-id="f034e-170">The `byte` type represents unsigned 8-bit integers with values between 0 and 255.</span></span>
*  <span data-ttu-id="f034e-171">`short` 類型代表帶正負號的16位整數，其值介於-32768 和32767之間。</span><span class="sxs-lookup"><span data-stu-id="f034e-171">The `short` type represents signed 16-bit integers with values between -32768 and 32767.</span></span>
*  <span data-ttu-id="f034e-172">`ushort` 類型代表不帶正負號的16位整數，其值介於0到65535之間。</span><span class="sxs-lookup"><span data-stu-id="f034e-172">The `ushort` type represents unsigned 16-bit integers with values between 0 and 65535.</span></span>
*  <span data-ttu-id="f034e-173">`int` 類型代表帶正負號的32位整數，其值介於-2147483648 與2147483647之間。</span><span class="sxs-lookup"><span data-stu-id="f034e-173">The `int` type represents signed 32-bit integers with values between -2147483648 and 2147483647.</span></span>
*  <span data-ttu-id="f034e-174">`uint` 類型代表不帶正負號的32位整數，其值介於0到4294967295之間。</span><span class="sxs-lookup"><span data-stu-id="f034e-174">The `uint` type represents unsigned 32-bit integers with values between 0 and 4294967295.</span></span>
*  <span data-ttu-id="f034e-175">`long` 類型代表帶正負號的64位整數，其值介於-9223372036854775808 到9223372036854775807之間。</span><span class="sxs-lookup"><span data-stu-id="f034e-175">The `long` type represents signed 64-bit integers with values between -9223372036854775808 and 9223372036854775807.</span></span>
*  <span data-ttu-id="f034e-176">`ulong` 類型代表不帶正負號的64位整數，其值介於0到18446744073709551615之間。</span><span class="sxs-lookup"><span data-stu-id="f034e-176">The `ulong` type represents unsigned 64-bit integers with values between 0 and 18446744073709551615.</span></span>
*  <span data-ttu-id="f034e-177">`char` 類型代表不帶正負號的16位整數，其值介於0到65535之間。</span><span class="sxs-lookup"><span data-stu-id="f034e-177">The `char` type represents unsigned 16-bit integers with values between 0 and 65535.</span></span> <span data-ttu-id="f034e-178">`char` 類型的可能值集合會對應到 Unicode 字元集。</span><span class="sxs-lookup"><span data-stu-id="f034e-178">The set of possible values for the `char` type corresponds to the Unicode character set.</span></span> <span data-ttu-id="f034e-179">雖然 `char` 與 `ushort`有相同的標記法，但不允許在另一個類型上允許所有作業。</span><span class="sxs-lookup"><span data-stu-id="f034e-179">Although `char` has the same representation as `ushort`, not all operations permitted on one type are permitted on the other.</span></span>

<span data-ttu-id="f034e-180">整數類型的一元和二元運算子一律會使用帶正負號的32位有效位數、不帶正負號的32位精確度、帶正負號的64位有效位數或不帶正負號的64位有效位數來運作：</span><span class="sxs-lookup"><span data-stu-id="f034e-180">The integral-type unary and binary operators always operate with signed 32-bit precision, unsigned 32-bit precision, signed 64-bit precision, or unsigned 64-bit precision:</span></span>

*  <span data-ttu-id="f034e-181">對於一元 `+` 和 `~` 運算子而言，運算元會轉換成類型 `T`，其中 `T` 是 `int`、`uint`、`long`和 `ulong` 的第一個，可以完全表示運算元的所有可能值。</span><span class="sxs-lookup"><span data-stu-id="f034e-181">For the unary `+` and `~` operators, the operand is converted to type `T`, where `T` is the first of `int`, `uint`, `long`, and `ulong` that can fully represent all possible values of the operand.</span></span> <span data-ttu-id="f034e-182">接著會使用 `T`類型的有效位數來執行作業，並 `T`結果的類型。</span><span class="sxs-lookup"><span data-stu-id="f034e-182">The operation is then performed using the precision of type `T`, and the type of the result is `T`.</span></span>
*  <span data-ttu-id="f034e-183">針對一元 `-` 運算子，運算元會轉換成類型 `T`，其中 `T` 是可以完全代表運算元所有可能值的 `int` 和 `long` 的第一個。</span><span class="sxs-lookup"><span data-stu-id="f034e-183">For the unary `-` operator, the operand is converted to type `T`, where `T` is the first of `int` and `long` that can fully represent all possible values of the operand.</span></span> <span data-ttu-id="f034e-184">接著會使用 `T`類型的有效位數來執行作業，並 `T`結果的類型。</span><span class="sxs-lookup"><span data-stu-id="f034e-184">The operation is then performed using the precision of type `T`, and the type of the result is `T`.</span></span> <span data-ttu-id="f034e-185">一元 `-` 運算子不能套用至 `ulong`類型的運算元。</span><span class="sxs-lookup"><span data-stu-id="f034e-185">The unary `-` operator cannot be applied to operands of type `ulong`.</span></span>
*  <span data-ttu-id="f034e-186">針對二進位 `+`，`-`、`*`、`/`、`%`、`&`、`^`、`|`、`==`、`!=`、`>`、`<`、`>=`和 `<=` 運算子會將運算元轉換成類型 `T`，其中 `T` 是 `int`、`uint`、`long`和 `ulong` 的第一個，可以完全表示這兩個運算元的所有可能值。</span><span class="sxs-lookup"><span data-stu-id="f034e-186">For the binary `+`, `-`, `*`, `/`, `%`, `&`, `^`, `|`, `==`, `!=`, `>`, `<`, `>=`, and `<=` operators, the operands are converted to type `T`, where `T` is the first of `int`, `uint`, `long`, and `ulong` that can fully represent all possible values of both operands.</span></span> <span data-ttu-id="f034e-187">然後，會使用 `T`類型的有效位數來執行作業，而結果的類型會是 `T` （或關聯式運算子的 `bool`）。</span><span class="sxs-lookup"><span data-stu-id="f034e-187">The operation is then performed using the precision of type `T`, and the type of the result is `T` (or `bool` for the relational operators).</span></span> <span data-ttu-id="f034e-188">一個運算元不允許 `long` 類型，另一個則屬於 `ulong` 與二元運算子的類型。</span><span class="sxs-lookup"><span data-stu-id="f034e-188">It is not permitted for one operand to be of type `long` and the other to be of type `ulong` with the binary operators.</span></span>
*  <span data-ttu-id="f034e-189">針對二元 `<<` 和 `>>` 運算子，左運算元會轉換成類型 `T`，其中 `T` 是 `int`、`uint`、`long`和 `ulong` 的第一個，可以完全表示運算元的所有可能值。</span><span class="sxs-lookup"><span data-stu-id="f034e-189">For the binary `<<` and `>>` operators, the left operand is converted to type `T`, where `T` is the first of `int`, `uint`, `long`, and `ulong` that can fully represent all possible values of the operand.</span></span> <span data-ttu-id="f034e-190">接著會使用 `T`類型的有效位數來執行作業，並 `T`結果的類型。</span><span class="sxs-lookup"><span data-stu-id="f034e-190">The operation is then performed using the precision of type `T`, and the type of the result is `T`.</span></span>

<span data-ttu-id="f034e-191">`char` 類型會分類為整數類資料類型，但它與其他整數類型不同，有兩種方式：</span><span class="sxs-lookup"><span data-stu-id="f034e-191">The `char` type is classified as an integral type, but it differs from the other integral types in two ways:</span></span>

*  <span data-ttu-id="f034e-192">沒有從其他類型到 `char` 類型的隱含轉換。</span><span class="sxs-lookup"><span data-stu-id="f034e-192">There are no implicit conversions from other types to the `char` type.</span></span> <span data-ttu-id="f034e-193">特別的是，即使 `sbyte`、`byte`和 `ushort` 類型的值範圍，都可以使用 `char` 類型來完整顯示，但是從 `sbyte`、`byte`或 `ushort` 到 `char` 的隱含轉換並不存在。</span><span class="sxs-lookup"><span data-stu-id="f034e-193">In particular, even though the `sbyte`, `byte`, and `ushort` types have ranges of values that are fully representable using the `char` type, implicit conversions from `sbyte`, `byte`, or `ushort` to `char` do not exist.</span></span>
*  <span data-ttu-id="f034e-194">`char` 類型的常數必須寫成*character_literal*s 或*integer_literal*s，並結合轉換成類型 `char`。</span><span class="sxs-lookup"><span data-stu-id="f034e-194">Constants of the `char` type must be written as *character_literal*s or as *integer_literal*s in combination with a cast to type `char`.</span></span> <span data-ttu-id="f034e-195">例如，`(char)10` 與 `'\x000A'` 相同。</span><span class="sxs-lookup"><span data-stu-id="f034e-195">For example, `(char)10` is the same as `'\x000A'`.</span></span>

<span data-ttu-id="f034e-196">`checked` 和 `unchecked` 運算子和語句是用來控制整數型別算數運算和轉換（[checked 和 unchecked 運算子](expressions.md#the-checked-and-unchecked-operators)）的溢位檢查。</span><span class="sxs-lookup"><span data-stu-id="f034e-196">The `checked` and `unchecked` operators and statements are used to control overflow checking for integral-type arithmetic operations and conversions ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)).</span></span> <span data-ttu-id="f034e-197">在 `checked` 內容中，溢位會產生編譯時期錯誤，或導致 `System.OverflowException` 擲回。</span><span class="sxs-lookup"><span data-stu-id="f034e-197">In a `checked` context, an overflow produces a compile-time error or causes a `System.OverflowException` to be thrown.</span></span> <span data-ttu-id="f034e-198">在 `unchecked` 內容中，會忽略溢位，而且不會捨棄任何不符合目的地類型的高序位位。</span><span class="sxs-lookup"><span data-stu-id="f034e-198">In an `unchecked` context, overflows are ignored and any high-order bits that do not fit in the destination type are discarded.</span></span>

### <a name="floating-point-types"></a><span data-ttu-id="f034e-199">浮點類型</span><span class="sxs-lookup"><span data-stu-id="f034e-199">Floating point types</span></span>

<span data-ttu-id="f034e-200">C#支援兩種浮點類型： `float` 和 `double`。</span><span class="sxs-lookup"><span data-stu-id="f034e-200">C# supports two floating point types: `float` and `double`.</span></span> <span data-ttu-id="f034e-201">`float` 和 `double` 類型會使用32位單精確度和64位雙精確度 IEEE 754 格式來表示，這會提供下列集合的值：</span><span class="sxs-lookup"><span data-stu-id="f034e-201">The `float` and `double` types are represented using the 32-bit single-precision and 64-bit double-precision IEEE 754 formats, which provide the following sets of values:</span></span>

*  <span data-ttu-id="f034e-202">正零和負零。</span><span class="sxs-lookup"><span data-stu-id="f034e-202">Positive zero and negative zero.</span></span> <span data-ttu-id="f034e-203">在大部分情況下，正零和負零的行為與簡單值零相同，但是某些作業會區分兩個（[除法運算子](expressions.md#division-operator)）。</span><span class="sxs-lookup"><span data-stu-id="f034e-203">In most situations, positive zero and negative zero behave identically as the simple value zero, but certain operations distinguish between the two ([Division operator](expressions.md#division-operator)).</span></span>
*  <span data-ttu-id="f034e-204">正無限大和負無限大。</span><span class="sxs-lookup"><span data-stu-id="f034e-204">Positive infinity and negative infinity.</span></span> <span data-ttu-id="f034e-205">無限大是由這類作業所產生，會將非零的數位零除。</span><span class="sxs-lookup"><span data-stu-id="f034e-205">Infinities are produced by such operations as dividing a non-zero number by zero.</span></span> <span data-ttu-id="f034e-206">例如，`1.0 / 0.0` 產生正無限大，`-1.0 / 0.0` 產生負無限大。</span><span class="sxs-lookup"><span data-stu-id="f034e-206">For example, `1.0 / 0.0` yields positive infinity, and `-1.0 / 0.0` yields negative infinity.</span></span>
*  <span data-ttu-id="f034e-207">***非數位***值，通常是 NaN 縮寫。</span><span class="sxs-lookup"><span data-stu-id="f034e-207">The ***Not-a-Number*** value, often abbreviated NaN.</span></span> <span data-ttu-id="f034e-208">Nan 是由不正確浮點運算所產生，例如零除零。</span><span class="sxs-lookup"><span data-stu-id="f034e-208">NaNs are produced by invalid floating-point operations, such as dividing zero by zero.</span></span>
*  <span data-ttu-id="f034e-209">表單 `s * m * 2^e`的有限非零值集合，其中 `s` 為1或-1，而 `m` 和 `e` 是由特定的浮點類型所決定： `float`、`0 < m < 2^24` 和 `-149 <= e <= 104``double`、`0 < m < 2^53` 和 `-1075 <= e <= 970`的、和。</span><span class="sxs-lookup"><span data-stu-id="f034e-209">The finite set of non-zero values of the form `s * m * 2^e`, where `s` is 1 or -1, and `m` and `e` are determined by the particular floating-point type: For `float`, `0 < m < 2^24` and `-149 <= e <= 104`, and for `double`, `0 < m < 2^53` and `-1075 <= e <= 970`.</span></span> <span data-ttu-id="f034e-210">不正規化的浮點數會被視為有效的非零值。</span><span class="sxs-lookup"><span data-stu-id="f034e-210">Denormalized floating-point numbers are considered valid non-zero values.</span></span>

<span data-ttu-id="f034e-211">`float` 類型可以代表範圍從大約 `1.5 * 10^-45` 到 `3.4 * 10^38`，精確度為7位數的值。</span><span class="sxs-lookup"><span data-stu-id="f034e-211">The `float` type can represent values ranging from approximately `1.5 * 10^-45` to `3.4 * 10^38` with a precision of 7 digits.</span></span>

<span data-ttu-id="f034e-212">`double` 類型可以代表範圍從大約 `5.0 * 10^-324` 到 `1.7 × 10^308`，精確度為15-16 位數的值。</span><span class="sxs-lookup"><span data-stu-id="f034e-212">The `double` type can represent values ranging from approximately `5.0 * 10^-324` to `1.7 × 10^308` with a precision of 15-16 digits.</span></span>

<span data-ttu-id="f034e-213">如果二元運算子的其中一個運算元屬於浮點類型，則另一個運算元必須是整數類資料類型或浮點類型，而運算的評估方式如下：</span><span class="sxs-lookup"><span data-stu-id="f034e-213">If one of the operands of a binary operator is of a floating-point type, then the other operand must be of an integral type or a floating-point type, and the operation is evaluated as follows:</span></span>

*  <span data-ttu-id="f034e-214">如果其中一個運算元是整數類資料類型，則該運算元會轉換成另一個運算元的浮點類型。</span><span class="sxs-lookup"><span data-stu-id="f034e-214">If one of the operands is of an integral type, then that operand is converted to the floating-point type of the other operand.</span></span>
*  <span data-ttu-id="f034e-215">然後，如果任一個運算元的類型是 `double`，另一個運算元會轉換成 `double`，則會使用至少 `double` 範圍和有效位數來執行作業，而結果的類型會是 `double` （或 `bool` 關係運算子）。</span><span class="sxs-lookup"><span data-stu-id="f034e-215">Then, if either of the operands is of type `double`, the other operand is converted to `double`, the operation is performed using at least `double` range and precision, and the type of the result is `double` (or `bool` for the relational operators).</span></span>
*  <span data-ttu-id="f034e-216">否則，會使用至少 `float` 的範圍和精確度來執行作業，而結果的類型會是 `float` （或關聯式運算子的 `bool`）。</span><span class="sxs-lookup"><span data-stu-id="f034e-216">Otherwise, the operation is performed using at least `float` range and precision, and the type of the result is `float` (or `bool` for the relational operators).</span></span>

<span data-ttu-id="f034e-217">浮點運算子（包括指派運算子）永遠不會產生例外狀況。</span><span class="sxs-lookup"><span data-stu-id="f034e-217">The floating-point operators, including the assignment operators, never produce exceptions.</span></span> <span data-ttu-id="f034e-218">相反地，在例外狀況下，浮點運算會產生零、無限大或 NaN，如下所述：</span><span class="sxs-lookup"><span data-stu-id="f034e-218">Instead, in exceptional situations, floating-point operations produce zero, infinity, or NaN, as described below:</span></span>

*  <span data-ttu-id="f034e-219">如果浮點運算的結果太小而無法用於目的地格式，作業的結果就會變成正零或負零。</span><span class="sxs-lookup"><span data-stu-id="f034e-219">If the result of a floating-point operation is too small for the destination format, the result of the operation becomes positive zero or negative zero.</span></span>
*  <span data-ttu-id="f034e-220">如果浮點運算的結果對目的地格式而言太大，則作業的結果會變成正無限大或負無限大。</span><span class="sxs-lookup"><span data-stu-id="f034e-220">If the result of a floating-point operation is too large for the destination format, the result of the operation becomes positive infinity or negative infinity.</span></span>
*  <span data-ttu-id="f034e-221">如果浮點運算無效，作業的結果就會變成 NaN。</span><span class="sxs-lookup"><span data-stu-id="f034e-221">If a floating-point operation is invalid, the result of the operation becomes NaN.</span></span>
*  <span data-ttu-id="f034e-222">如果浮點運算的一個或兩個運算元都是 NaN，則作業的結果會變成 NaN。</span><span class="sxs-lookup"><span data-stu-id="f034e-222">If one or both operands of a floating-point operation is NaN, the result of the operation becomes NaN.</span></span>

<span data-ttu-id="f034e-223">執行浮點運算時，精確度可能會高於作業的結果類型。</span><span class="sxs-lookup"><span data-stu-id="f034e-223">Floating-point operations may be performed with higher precision than the result type of the operation.</span></span> <span data-ttu-id="f034e-224">例如，某些硬體架構支援「延伸」或「長雙精度」浮點類型，其範圍和有效位數大於 `double` 類型，並使用這個較高的精確度類型來隱含執行所有浮點運算。</span><span class="sxs-lookup"><span data-stu-id="f034e-224">For example, some hardware architectures support an "extended" or "long double" floating-point type with greater range and precision than the `double` type, and implicitly perform all floating-point operations using this higher precision type.</span></span> <span data-ttu-id="f034e-225">只有在效能過高的情況下，才能夠以較低的精確度執行浮點運算，而不需要實作為要略過效能和精確度的實體系， C#而是允許更高的精確度類型用於所有浮點運算。</span><span class="sxs-lookup"><span data-stu-id="f034e-225">Only at excessive cost in performance can such hardware architectures be made to perform floating-point operations with less precision, and rather than require an implementation to forfeit both performance and precision, C# allows a higher precision type to be used for all floating-point operations.</span></span> <span data-ttu-id="f034e-226">除了提供更精確的結果，這種情況很少會有任何明顯的影響。</span><span class="sxs-lookup"><span data-stu-id="f034e-226">Other than delivering more precise results, this rarely has any measurable effects.</span></span> <span data-ttu-id="f034e-227">不過，在表單 `x * y / z`的運算式中，乘法會產生超出 `double` 範圍的結果，但後續的除法會將暫存結果重新帶入 `double` 範圍，這是運算式在中評估的事實。較高的範圍格式可能會導致產生有限的結果，而不是無限大。</span><span class="sxs-lookup"><span data-stu-id="f034e-227">However, in expressions of the form `x * y / z`, where the multiplication produces a result that is outside the `double` range, but the subsequent division brings the temporary result back into the `double` range, the fact that the expression is evaluated in a higher range format may cause a finite result to be produced instead of an infinity.</span></span>

### <a name="the-decimal-type"></a><span data-ttu-id="f034e-228">Decimal 類型</span><span class="sxs-lookup"><span data-stu-id="f034e-228">The decimal type</span></span>

<span data-ttu-id="f034e-229">`decimal` 型別是 128 位元的資料型別，適合財務和貨幣計算。</span><span class="sxs-lookup"><span data-stu-id="f034e-229">The `decimal` type is a 128-bit data type suitable for financial and monetary calculations.</span></span> <span data-ttu-id="f034e-230">`decimal` 類型可以代表範圍從 `1.0 * 10^-28` 到大約 `7.9 * 10^28` 28-29 個有效位數的值。</span><span class="sxs-lookup"><span data-stu-id="f034e-230">The `decimal` type can represent values ranging from `1.0 * 10^-28` to approximately `7.9 * 10^28` with 28-29 significant digits.</span></span>

<span data-ttu-id="f034e-231">`decimal` 類型的一組有限值的格式為 `(-1)^s * c * 10^-e`，其中的正負號 `s` 為0或1，`c` 是由 `0 <= *c* < 2^96`指定，而尺規 `e` 則是 `0 <= e <= 28`。`decimal` 類型不支援帶正負號的零、無限大或 NaN。</span><span class="sxs-lookup"><span data-stu-id="f034e-231">The finite set of values of type `decimal` are of the form `(-1)^s * c * 10^-e`, where the sign `s` is 0 or 1, the coefficient `c` is given by `0 <= *c* < 2^96`, and the scale `e` is such that `0 <= e <= 28`.The `decimal` type does not support signed zeros, infinities, or NaN's.</span></span> <span data-ttu-id="f034e-232">`decimal` 是以10的乘冪來表示的96位整數。</span><span class="sxs-lookup"><span data-stu-id="f034e-232">A `decimal` is represented as a 96-bit integer scaled by a power of ten.</span></span> <span data-ttu-id="f034e-233">針對絕對值小於 `1.0m`的 `decimal`s，此值會精確到28個小數位數，但不會再進一步。</span><span class="sxs-lookup"><span data-stu-id="f034e-233">For `decimal`s with an absolute value less than `1.0m`, the value is exact to the 28th decimal place, but no further.</span></span> <span data-ttu-id="f034e-234">針對絕對值大於或等於 `1.0m`的 `decimal`s，此值會精確為28或29個數字。</span><span class="sxs-lookup"><span data-stu-id="f034e-234">For `decimal`s with an absolute value greater than or equal to `1.0m`, the value is exact to 28 or 29 digits.</span></span> <span data-ttu-id="f034e-235">相反于 `float` 和 `double` 的資料類型，十進位的小數數位，例如0.1，可以完全在 `decimal` 標記法中表示。</span><span class="sxs-lookup"><span data-stu-id="f034e-235">Contrary to the `float` and `double` data types, decimal fractional numbers such as 0.1 can be represented exactly in the `decimal` representation.</span></span> <span data-ttu-id="f034e-236">在 `float` 和 `double` 標記法中，這類數位通常是無限的分數，因此這些標記法更容易發生舍入錯誤。</span><span class="sxs-lookup"><span data-stu-id="f034e-236">In the `float` and `double` representations, such numbers are often infinite fractions, making those representations more prone to round-off errors.</span></span>

<span data-ttu-id="f034e-237">如果二元運算子的其中一個運算元屬於 `decimal`類型，則另一個運算元必須是整數類型或類型 `decimal`。</span><span class="sxs-lookup"><span data-stu-id="f034e-237">If one of the operands of a binary operator is of type `decimal`, then the other operand must be of an integral type or of type `decimal`.</span></span> <span data-ttu-id="f034e-238">如果整數類型運算元存在，則會先將它轉換成 `decimal`，然後才執行作業。</span><span class="sxs-lookup"><span data-stu-id="f034e-238">If an integral type operand is present, it is converted to `decimal` before the operation is performed.</span></span>

<span data-ttu-id="f034e-239">`decimal` 類型值上的運算結果是，這會因計算確切的結果（根據每個運算子定義的保留尺規）而產生，然後四捨五入以符合標記法。</span><span class="sxs-lookup"><span data-stu-id="f034e-239">The result of an operation on values of type `decimal` is that which would result from calculating an exact result (preserving scale, as defined for each operator) and then rounding to fit the representation.</span></span> <span data-ttu-id="f034e-240">結果會舍入到最接近的可顯示值，而且當結果同樣接近兩個可顯示的值時，就是在最小有效位數位置（也就是「四進位」）中具有偶數數值的值。</span><span class="sxs-lookup"><span data-stu-id="f034e-240">Results are rounded to the nearest representable value, and, when a result is equally close to two representable values, to the value that has an even number in the least significant digit position (this is known as "banker's rounding").</span></span> <span data-ttu-id="f034e-241">零的結果一律具有0的正負號，而小數值為0。</span><span class="sxs-lookup"><span data-stu-id="f034e-241">A zero result always has a sign of 0 and a scale of 0.</span></span>

<span data-ttu-id="f034e-242">如果十進位算數運算產生的值小於或等於 `5 * 10^-29` 以絕對值表示，則作業的結果會變成零。</span><span class="sxs-lookup"><span data-stu-id="f034e-242">If a decimal arithmetic operation produces a value less than or equal to `5 * 10^-29` in absolute value, the result of the operation becomes zero.</span></span> <span data-ttu-id="f034e-243">如果 `decimal` 算數運算產生的結果對 `decimal` 格式而言太大，則會擲回 `System.OverflowException`。</span><span class="sxs-lookup"><span data-stu-id="f034e-243">If a `decimal` arithmetic operation produces a result that is too large for the `decimal` format, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="f034e-244">`decimal` 類型的精確度較高，但範圍比浮點類型更小。</span><span class="sxs-lookup"><span data-stu-id="f034e-244">The `decimal` type has greater precision but smaller range than the floating-point types.</span></span> <span data-ttu-id="f034e-245">因此，從浮點類型到 `decimal` 的轉換可能會產生溢位例外狀況，而從 `decimal` 到浮點類型的轉換可能會導致失去精確度。</span><span class="sxs-lookup"><span data-stu-id="f034e-245">Thus, conversions from the floating-point types to `decimal` might produce overflow exceptions, and conversions from `decimal` to the floating-point types might cause loss of precision.</span></span> <span data-ttu-id="f034e-246">基於這些理由，浮點類型與 `decimal`之間不存在隱含轉換，而且沒有明確轉換，也無法在同一個運算式中混合使用浮點和 `decimal` 運算元。</span><span class="sxs-lookup"><span data-stu-id="f034e-246">For these reasons, no implicit conversions exist between the floating-point types and `decimal`, and without explicit casts, it is not possible to mix floating-point and `decimal` operands in the same expression.</span></span>

### <a name="the-bool-type"></a><span data-ttu-id="f034e-247">Bool 類型</span><span class="sxs-lookup"><span data-stu-id="f034e-247">The bool type</span></span>

<span data-ttu-id="f034e-248">`bool` 類型代表布林邏輯數量。</span><span class="sxs-lookup"><span data-stu-id="f034e-248">The `bool` type represents boolean logical quantities.</span></span> <span data-ttu-id="f034e-249">`bool` 類型的可能值為 `true` 和 `false`。</span><span class="sxs-lookup"><span data-stu-id="f034e-249">The possible values of type `bool` are `true` and `false`.</span></span>

<span data-ttu-id="f034e-250">`bool` 和其他類型之間不存在標準轉換。</span><span class="sxs-lookup"><span data-stu-id="f034e-250">No standard conversions exist between `bool` and other types.</span></span> <span data-ttu-id="f034e-251">特別是，`bool` 類型是不同的，而且與整數類資料類型不同，而且不能使用 `bool` 值來取代整數值，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="f034e-251">In particular, the `bool` type is distinct and separate from the integral types, and a `bool` value cannot be used in place of an integral value, and vice versa.</span></span>

<span data-ttu-id="f034e-252">在 C 和C++語言中，零整數或浮點數，或 null 指標可以轉換成布林值 `false`和非零整數或浮點數，或非 null 指標可以轉換成布林值 `true`）。</span><span class="sxs-lookup"><span data-stu-id="f034e-252">In the C and C++ languages, a zero integral or floating-point value, or a null pointer can be converted to the boolean value `false`, and a non-zero integral or floating-point value, or a non-null pointer can be converted to the boolean value `true`.</span></span> <span data-ttu-id="f034e-253">在C#中，這類轉換會藉由明確地將整數或浮點數與零進行比較，或明確地比較物件參考與 `null`來完成。</span><span class="sxs-lookup"><span data-stu-id="f034e-253">In C#, such conversions are accomplished by explicitly comparing an integral or floating-point value to zero, or by explicitly comparing an object reference to `null`.</span></span>

### <a name="enumeration-types"></a><span data-ttu-id="f034e-254">列舉類型</span><span class="sxs-lookup"><span data-stu-id="f034e-254">Enumeration types</span></span>

<span data-ttu-id="f034e-255">列舉類型是具有已命名常數的相異類型。</span><span class="sxs-lookup"><span data-stu-id="f034e-255">An enumeration type is a distinct type with named constants.</span></span> <span data-ttu-id="f034e-256">每個列舉類型都具有基礎類型，必須是 `byte`、`sbyte`、`short`、`ushort`、`int`、`uint`、`long` 或 `ulong`。</span><span class="sxs-lookup"><span data-stu-id="f034e-256">Every enumeration type has an underlying type, which must be `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` or `ulong`.</span></span> <span data-ttu-id="f034e-257">列舉類型的值集合與基礎類型的一組值相同。</span><span class="sxs-lookup"><span data-stu-id="f034e-257">The set of values of the enumeration type is the same as the set of values of the underlying type.</span></span> <span data-ttu-id="f034e-258">列舉類型的值不限於已命名常數的值。</span><span class="sxs-lookup"><span data-stu-id="f034e-258">Values of the enumeration type are not restricted to the values of the named constants.</span></span> <span data-ttu-id="f034e-259">列舉類型是透過列舉宣告（[列舉](enums.md#enum-declarations)宣告）來定義。</span><span class="sxs-lookup"><span data-stu-id="f034e-259">Enumeration types are defined through enumeration declarations ([Enum declarations](enums.md#enum-declarations)).</span></span>

### <a name="nullable-types"></a><span data-ttu-id="f034e-260">可為 Null 的型別</span><span class="sxs-lookup"><span data-stu-id="f034e-260">Nullable types</span></span>

<span data-ttu-id="f034e-261">可為 null 的型別可以代表其***基礎類型***的所有值加上額外的 null 值。</span><span class="sxs-lookup"><span data-stu-id="f034e-261">A nullable type can represent all values of its ***underlying type*** plus an additional null value.</span></span> <span data-ttu-id="f034e-262">可為 null 的型別會 `T?`寫入，其中 `T` 是基礎型別。</span><span class="sxs-lookup"><span data-stu-id="f034e-262">A nullable type is written `T?`, where `T` is the underlying type.</span></span> <span data-ttu-id="f034e-263">此語法是 `System.Nullable<T>`的縮寫，而這兩種形式可以交換使用。</span><span class="sxs-lookup"><span data-stu-id="f034e-263">This syntax is shorthand for `System.Nullable<T>`, and the two forms can be used interchangeably.</span></span>

<span data-ttu-id="f034e-264">相反地，***不可為 null 的實值型***別就是除了 `System.Nullable<T>` 和其速記 `T?` （適用于任何 `T`）以外的任何實值型別，加上任何限制為不可為 null 的實值型別的型別參數（也就是具有 `struct` 的任何型別參數條件約束）。</span><span class="sxs-lookup"><span data-stu-id="f034e-264">A ***non-nullable value type*** conversely is any value type other than `System.Nullable<T>` and its shorthand `T?` (for any `T`), plus any type parameter that is constrained to be a non-nullable value type (that is, any type parameter with a `struct` constraint).</span></span> <span data-ttu-id="f034e-265">`System.Nullable<T>` 型別會指定 `T` （[型別參數條件約束](classes.md#type-parameter-constraints)）的實值型別條件約束，這表示可為 null 型別的基礎型別可以是任何不可為 null 的實值型別。</span><span class="sxs-lookup"><span data-stu-id="f034e-265">The `System.Nullable<T>` type specifies the value type constraint for `T` ([Type parameter constraints](classes.md#type-parameter-constraints)), which means that the underlying type of a nullable type can be any non-nullable value type.</span></span> <span data-ttu-id="f034e-266">可為 null 類型的基礎類型不能是可為 null 的類型或參考型別。</span><span class="sxs-lookup"><span data-stu-id="f034e-266">The underlying type of a nullable type cannot be a nullable type or a reference type.</span></span> <span data-ttu-id="f034e-267">例如，`int??` 和 `string?` 是不正確類型。</span><span class="sxs-lookup"><span data-stu-id="f034e-267">For example, `int??` and `string?` are invalid types.</span></span>

<span data-ttu-id="f034e-268">可為 null 的型別 `T?` 的實例有兩個公用唯讀屬性：</span><span class="sxs-lookup"><span data-stu-id="f034e-268">An instance of a nullable type `T?` has two public read-only properties:</span></span>

*  <span data-ttu-id="f034e-269">類型的 `HasValue` 屬性 `bool`</span><span class="sxs-lookup"><span data-stu-id="f034e-269">A `HasValue` property of type `bool`</span></span>
*  <span data-ttu-id="f034e-270">類型的 `Value` 屬性 `T`</span><span class="sxs-lookup"><span data-stu-id="f034e-270">A `Value` property of type `T`</span></span>

<span data-ttu-id="f034e-271">`HasValue` 為 true 的實例稱為非 null。</span><span class="sxs-lookup"><span data-stu-id="f034e-271">An instance for which `HasValue` is true is said to be non-null.</span></span> <span data-ttu-id="f034e-272">非 null 實例包含已知的值，`Value` 會傳回該值。</span><span class="sxs-lookup"><span data-stu-id="f034e-272">A non-null instance contains a known value and `Value` returns that value.</span></span>

<span data-ttu-id="f034e-273">`HasValue` 為 false 的實例稱為 null。</span><span class="sxs-lookup"><span data-stu-id="f034e-273">An instance for which `HasValue` is false is said to be null.</span></span> <span data-ttu-id="f034e-274">Null 實例有未定義的值。</span><span class="sxs-lookup"><span data-stu-id="f034e-274">A null instance has an undefined value.</span></span> <span data-ttu-id="f034e-275">嘗試讀取 null 實例的 `Value` 會導致擲回 `System.InvalidOperationException`。</span><span class="sxs-lookup"><span data-stu-id="f034e-275">Attempting to read the `Value` of a null instance causes a `System.InvalidOperationException` to be thrown.</span></span> <span data-ttu-id="f034e-276">存取可為 null 之實例之 `Value` 屬性的程式稱為解除***包裝***。</span><span class="sxs-lookup"><span data-stu-id="f034e-276">The process of accessing the `Value` property of a nullable instance is referred to as ***unwrapping***.</span></span>

<span data-ttu-id="f034e-277">除了預設的函式，每個可為 null 的型別 `T?` 都具有公用的函式，該函式會接受 `T`類型的單一引數。</span><span class="sxs-lookup"><span data-stu-id="f034e-277">In addition to the default constructor, every nullable type `T?` has a public constructor that takes a single argument of type `T`.</span></span> <span data-ttu-id="f034e-278">假設 `T`類型的值 `x`，則為格式的函式呼叫</span><span class="sxs-lookup"><span data-stu-id="f034e-278">Given a value `x` of type `T`, a constructor invocation of the form</span></span>

```csharp
new T?(x)
```
<span data-ttu-id="f034e-279">建立 `T?` 的非 null 實例，其 `Value` 屬性會 `x`。</span><span class="sxs-lookup"><span data-stu-id="f034e-279">creates a non-null instance of `T?` for which the `Value` property is `x`.</span></span> <span data-ttu-id="f034e-280">針對指定的值建立可為 null 之型別的非 null 實例的程式稱為「***包裝***」。</span><span class="sxs-lookup"><span data-stu-id="f034e-280">The process of creating a non-null instance of a nullable type for a given value is referred to as ***wrapping***.</span></span>

<span data-ttu-id="f034e-281">隱含轉換可從 `null` 常值取得，以 `T?` （[Null 常值轉換](conversions.md#null-literal-conversions)），以及從 `T` 到 `T?` （[隱含可為 null 的轉換](conversions.md#implicit-nullable-conversions)）。</span><span class="sxs-lookup"><span data-stu-id="f034e-281">Implicit conversions are available from the `null` literal to `T?` ([Null literal conversions](conversions.md#null-literal-conversions)) and from `T` to `T?` ([Implicit nullable conversions](conversions.md#implicit-nullable-conversions)).</span></span>

## <a name="reference-types"></a><span data-ttu-id="f034e-282">參考型別</span><span class="sxs-lookup"><span data-stu-id="f034e-282">Reference types</span></span>

<span data-ttu-id="f034e-283">參考型別是類別類型、介面類別型、陣列類型或委派類型。</span><span class="sxs-lookup"><span data-stu-id="f034e-283">A reference type is a class type, an interface type, an array type, or a delegate type.</span></span>

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

<span data-ttu-id="f034e-284">參考型別值是類型***實例***的參考，後者稱為***物件***。</span><span class="sxs-lookup"><span data-stu-id="f034e-284">A reference type value is a reference to an ***instance*** of the type, the latter known as an ***object***.</span></span> <span data-ttu-id="f034e-285">`null` 的特殊值與所有參考型別相容，並指出不存在實例。</span><span class="sxs-lookup"><span data-stu-id="f034e-285">The special value `null` is compatible with all reference types and indicates the absence of an instance.</span></span>

### <a name="class-types"></a><span data-ttu-id="f034e-286">類別型別</span><span class="sxs-lookup"><span data-stu-id="f034e-286">Class types</span></span>

<span data-ttu-id="f034e-287">類別類型會定義資料結構，其中包含資料成員（常數和欄位）、函式成員（方法、屬性、事件、索引子、運算子、實例的函式、析構函式和靜態的函式）和巢狀型別。</span><span class="sxs-lookup"><span data-stu-id="f034e-287">A class type defines a data structure that contains data members (constants and fields), function members (methods, properties, events, indexers, operators, instance constructors, destructors and static constructors), and nested types.</span></span> <span data-ttu-id="f034e-288">類別類型支援繼承，這是衍生類別可以擴充和特殊化基類的機制。</span><span class="sxs-lookup"><span data-stu-id="f034e-288">Class types support inheritance, a mechanism whereby derived classes can extend and specialize base classes.</span></span> <span data-ttu-id="f034e-289">類別類型的實例是使用*object_creation_expression*s （[物件建立運算式](expressions.md#object-creation-expressions)）所建立。</span><span class="sxs-lookup"><span data-stu-id="f034e-289">Instances of class types are created using *object_creation_expression*s ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span>

<span data-ttu-id="f034e-290">類別中會描述類別[類型。](classes.md)</span><span class="sxs-lookup"><span data-stu-id="f034e-290">Class types are described in [Classes](classes.md).</span></span>

<span data-ttu-id="f034e-291">某些預先定義的C#類別類型在語言中具有特殊意義，如下表所述。</span><span class="sxs-lookup"><span data-stu-id="f034e-291">Certain predefined class types have special meaning in the C# language, as described in the table below.</span></span>


| <span data-ttu-id="f034e-292">__類別類型__</span><span class="sxs-lookup"><span data-stu-id="f034e-292">__Class type__</span></span>     | <span data-ttu-id="f034e-293">__說明__</span><span class="sxs-lookup"><span data-stu-id="f034e-293">__Description__</span></span>                                         |
|--------------------|---------------------------------------------------------|
| `System.Object`    | <span data-ttu-id="f034e-294">所有其他類型的最終基底類別。</span><span class="sxs-lookup"><span data-stu-id="f034e-294">The ultimate base class of all other types.</span></span> <span data-ttu-id="f034e-295">請參閱[物件類型](types.md#the-object-type)。</span><span class="sxs-lookup"><span data-stu-id="f034e-295">See [The object type](types.md#the-object-type).</span></span> | 
| `System.String`    | <span data-ttu-id="f034e-296">C#語言的字串類型。</span><span class="sxs-lookup"><span data-stu-id="f034e-296">The string type of the C# language.</span></span> <span data-ttu-id="f034e-297">請參閱[字串類型](types.md#the-string-type)。</span><span class="sxs-lookup"><span data-stu-id="f034e-297">See [The string type](types.md#the-string-type).</span></span>         |
| `System.ValueType` | <span data-ttu-id="f034e-298">所有實數值型別的基類。</span><span class="sxs-lookup"><span data-stu-id="f034e-298">The base class of all value types.</span></span> <span data-ttu-id="f034e-299">請參閱[system.object 類型](types.md#the-systemvaluetype-type)。</span><span class="sxs-lookup"><span data-stu-id="f034e-299">See [The System.ValueType type](types.md#the-systemvaluetype-type).</span></span>          |
| `System.Enum`      | <span data-ttu-id="f034e-300">所有列舉類型的基類。</span><span class="sxs-lookup"><span data-stu-id="f034e-300">The base class of all enum types.</span></span> <span data-ttu-id="f034e-301">請[參閱](enums.md)列舉。</span><span class="sxs-lookup"><span data-stu-id="f034e-301">See [Enums](enums.md).</span></span>              |
| `System.Array`     | <span data-ttu-id="f034e-302">所有陣列類型的基類。</span><span class="sxs-lookup"><span data-stu-id="f034e-302">The base class of all array types.</span></span> <span data-ttu-id="f034e-303">請參閱[陣列](arrays.md)。</span><span class="sxs-lookup"><span data-stu-id="f034e-303">See [Arrays](arrays.md).</span></span>             |
| `System.Delegate`  | <span data-ttu-id="f034e-304">所有委派類型的基類。</span><span class="sxs-lookup"><span data-stu-id="f034e-304">The base class of all delegate types.</span></span> <span data-ttu-id="f034e-305">請參閱[委派](delegates.md)。</span><span class="sxs-lookup"><span data-stu-id="f034e-305">See [Delegates](delegates.md).</span></span>          |
| `System.Exception` | <span data-ttu-id="f034e-306">所有例外狀況類型的基類。</span><span class="sxs-lookup"><span data-stu-id="f034e-306">The base class of all exception types.</span></span> <span data-ttu-id="f034e-307">請參閱[例外](exceptions.md)狀況。</span><span class="sxs-lookup"><span data-stu-id="f034e-307">See [Exceptions](exceptions.md).</span></span>         |

### <a name="the-object-type"></a><span data-ttu-id="f034e-308">物件型別</span><span class="sxs-lookup"><span data-stu-id="f034e-308">The object type</span></span>

<span data-ttu-id="f034e-309">`object` 類別類型是所有其他類型的最終基底類別。</span><span class="sxs-lookup"><span data-stu-id="f034e-309">The `object` class type is the ultimate base class of all other types.</span></span> <span data-ttu-id="f034e-310">中C#的每個型別都是直接或間接衍生自 `object` 類別型別。</span><span class="sxs-lookup"><span data-stu-id="f034e-310">Every type in C# directly or indirectly derives from the `object` class type.</span></span>

<span data-ttu-id="f034e-311">關鍵字 `object` 只是預先定義之類別 `System.Object`的別名。</span><span class="sxs-lookup"><span data-stu-id="f034e-311">The keyword `object` is simply an alias for the predefined class `System.Object`.</span></span>

### <a name="the-dynamic-type"></a><span data-ttu-id="f034e-312">動態型別</span><span class="sxs-lookup"><span data-stu-id="f034e-312">The dynamic type</span></span>

<span data-ttu-id="f034e-313">`dynamic` 型別（例如 `object`）可以參考任何物件。</span><span class="sxs-lookup"><span data-stu-id="f034e-313">The `dynamic` type, like `object`, can reference any object.</span></span> <span data-ttu-id="f034e-314">當運算子套用至 `dynamic`類型的運算式時，其解析會延遲到程式執行為止。</span><span class="sxs-lookup"><span data-stu-id="f034e-314">When operators are applied to expressions of type `dynamic`, their resolution is deferred until the program is run.</span></span> <span data-ttu-id="f034e-315">因此，如果運算子無法合法套用至參考的物件，則在編譯期間不會提供任何錯誤。</span><span class="sxs-lookup"><span data-stu-id="f034e-315">Thus, if the operator cannot legally be applied to the referenced object, no error is given during compilation.</span></span> <span data-ttu-id="f034e-316">相反地，當運算子的解析在執行時間失敗時，將會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="f034e-316">Instead an exception will be thrown when resolution of the operator fails at run-time.</span></span>

<span data-ttu-id="f034e-317">其目的是要允許動態系結，這在[動態](expressions.md#dynamic-binding)系結中有詳細的說明。</span><span class="sxs-lookup"><span data-stu-id="f034e-317">Its purpose is to allow dynamic binding, which is described in detail in [Dynamic binding](expressions.md#dynamic-binding).</span></span>

<span data-ttu-id="f034e-318">`dynamic` 會視為與 `object` 相同，但下列方面除外：</span><span class="sxs-lookup"><span data-stu-id="f034e-318">`dynamic` is considered identical to `object` except in the following respects:</span></span>

*  <span data-ttu-id="f034e-319">`dynamic` 類型之運算式上的作業可以動態繫結（[動態](expressions.md#dynamic-binding)系結）。</span><span class="sxs-lookup"><span data-stu-id="f034e-319">Operations on expressions of type `dynamic` can be dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span>
*  <span data-ttu-id="f034e-320">型別推斷（[型別推斷](expressions.md#type-inference)）會偏好 `object` 的 `dynamic`，如果兩者都是候選項目。</span><span class="sxs-lookup"><span data-stu-id="f034e-320">Type inference ([Type inference](expressions.md#type-inference)) will prefer `dynamic` over `object` if both are candidates.</span></span>

<span data-ttu-id="f034e-321">由於這項等價，下列內容包含：</span><span class="sxs-lookup"><span data-stu-id="f034e-321">Because of this equivalence, the following holds:</span></span>

*  <span data-ttu-id="f034e-322">`object` 和 `dynamic`之間，以及以 `object` 取代 `dynamic` 時，兩者之間有隱含的身分識別轉換。</span><span class="sxs-lookup"><span data-stu-id="f034e-322">There is an implicit identity conversion between `object` and `dynamic`, and between constructed types that are the same when replacing `dynamic` with `object`</span></span>
*  <span data-ttu-id="f034e-323">`object` 的隱含和明確轉換也適用于 `dynamic`的和。</span><span class="sxs-lookup"><span data-stu-id="f034e-323">Implicit and explicit conversions to and from `object` also apply to and from `dynamic`.</span></span>
*  <span data-ttu-id="f034e-324">以 `object` 取代 `dynamic` 時，方法簽章會被視為相同的簽章</span><span class="sxs-lookup"><span data-stu-id="f034e-324">Method signatures that are the same when replacing `dynamic` with `object` are considered the same signature</span></span>
*  <span data-ttu-id="f034e-325">類型 `dynamic` 在執行時間與 `object` 不區分。</span><span class="sxs-lookup"><span data-stu-id="f034e-325">The type `dynamic` is indistinguishable from `object` at run-time.</span></span>
*  <span data-ttu-id="f034e-326">`dynamic` 類型的運算式稱為***動態運算式***。</span><span class="sxs-lookup"><span data-stu-id="f034e-326">An expression of the type `dynamic` is referred to as a ***dynamic expression***.</span></span>

### <a name="the-string-type"></a><span data-ttu-id="f034e-327">字串型別</span><span class="sxs-lookup"><span data-stu-id="f034e-327">The string type</span></span>

<span data-ttu-id="f034e-328">`string` 類型是直接繼承自 `object`的密封類別類型。</span><span class="sxs-lookup"><span data-stu-id="f034e-328">The `string` type is a sealed class type that inherits directly from `object`.</span></span> <span data-ttu-id="f034e-329">`string` 類別的實例代表 Unicode 字元字串。</span><span class="sxs-lookup"><span data-stu-id="f034e-329">Instances of the `string` class represent Unicode character strings.</span></span>

<span data-ttu-id="f034e-330">`string` 類型的值可以撰寫為字串常值（[字串常](lexical-structure.md#string-literals)值）。</span><span class="sxs-lookup"><span data-stu-id="f034e-330">Values of the `string` type can be written as string literals ([String literals](lexical-structure.md#string-literals)).</span></span>

<span data-ttu-id="f034e-331">關鍵字 `string` 只是預先定義之類別 `System.String`的別名。</span><span class="sxs-lookup"><span data-stu-id="f034e-331">The keyword `string` is simply an alias for the predefined class `System.String`.</span></span>

### <a name="interface-types"></a><span data-ttu-id="f034e-332">介面型別</span><span class="sxs-lookup"><span data-stu-id="f034e-332">Interface types</span></span>

<span data-ttu-id="f034e-333">介面會定義合約。</span><span class="sxs-lookup"><span data-stu-id="f034e-333">An interface defines a contract.</span></span> <span data-ttu-id="f034e-334">實作為介面的類別或結構必須遵守其合約。</span><span class="sxs-lookup"><span data-stu-id="f034e-334">A class or struct that implements an interface must adhere to its contract.</span></span> <span data-ttu-id="f034e-335">介面可能繼承自多個基底介面，而類別或結構可能會執行多個介面。</span><span class="sxs-lookup"><span data-stu-id="f034e-335">An interface may inherit from multiple base interfaces, and a class or struct may implement multiple interfaces.</span></span>

<span data-ttu-id="f034e-336">介面類別型會[在介面中說明。](interfaces.md)</span><span class="sxs-lookup"><span data-stu-id="f034e-336">Interface types are described in [Interfaces](interfaces.md).</span></span>

### <a name="array-types"></a><span data-ttu-id="f034e-337">陣列型別</span><span class="sxs-lookup"><span data-stu-id="f034e-337">Array types</span></span>

<span data-ttu-id="f034e-338">陣列是一種資料結構，其中包含零個或多個透過計算索引存取的變數。</span><span class="sxs-lookup"><span data-stu-id="f034e-338">An array is a data structure that contains zero or more variables which are accessed through computed indices.</span></span> <span data-ttu-id="f034e-339">陣列中包含的變數（也稱為陣列的元素）都是相同的類型，而此類型稱為陣列的元素類型。</span><span class="sxs-lookup"><span data-stu-id="f034e-339">The variables contained in an array, also called the elements of the array, are all of the same type, and this type is called the element type of the array.</span></span>

<span data-ttu-id="f034e-340">陣列中會描述陣列[類型。](arrays.md)</span><span class="sxs-lookup"><span data-stu-id="f034e-340">Array types are described in [Arrays](arrays.md).</span></span>

### <a name="delegate-types"></a><span data-ttu-id="f034e-341">委派型別</span><span class="sxs-lookup"><span data-stu-id="f034e-341">Delegate types</span></span>

<span data-ttu-id="f034e-342">「委派」（delegate）是指一或多個方法的資料結構。</span><span class="sxs-lookup"><span data-stu-id="f034e-342">A delegate is a data structure that refers to one or more methods.</span></span> <span data-ttu-id="f034e-343">若為實例方法，它也會參考其對應的物件實例。</span><span class="sxs-lookup"><span data-stu-id="f034e-343">For instance methods, it also refers to their corresponding object instances.</span></span>

<span data-ttu-id="f034e-344">C 或C++中委派最接近的對等是函式指標，但函式指標只能參考靜態函式，而委派可以同時參考靜態和實例方法。</span><span class="sxs-lookup"><span data-stu-id="f034e-344">The closest equivalent of a delegate in C or C++ is a function pointer, but whereas a function pointer can only reference static functions, a delegate can reference both static and instance methods.</span></span> <span data-ttu-id="f034e-345">在後者的情況下，委派不僅會儲存對方法進入點的參考，也會存放要叫用方法之物件實例的參考。</span><span class="sxs-lookup"><span data-stu-id="f034e-345">In the latter case, the delegate stores not only a reference to the method's entry point, but also a reference to the object instance on which to invoke the method.</span></span>

<span data-ttu-id="f034e-346">委派中會描述委派[類型。](delegates.md)</span><span class="sxs-lookup"><span data-stu-id="f034e-346">Delegate types are described in [Delegates](delegates.md).</span></span>

## <a name="boxing-and-unboxing"></a><span data-ttu-id="f034e-347">Boxing 和 Unboxing</span><span class="sxs-lookup"><span data-stu-id="f034e-347">Boxing and unboxing</span></span>

<span data-ttu-id="f034e-348">裝箱和取消裝箱的概念是類型系統C#的核心。</span><span class="sxs-lookup"><span data-stu-id="f034e-348">The concept of boxing and unboxing is central to C#'s type system.</span></span> <span data-ttu-id="f034e-349">它會允許*value_type*的任何值在類型 `object`之間來回轉換，藉此提供*value_type*s 與*reference_type*之間的橋樑。</span><span class="sxs-lookup"><span data-stu-id="f034e-349">It provides a bridge between *value_type*s and *reference_type*s by permitting any value of a *value_type* to be converted to and from type `object`.</span></span> <span data-ttu-id="f034e-350">「裝箱」和「取消裝箱」可讓類型系統的統一視圖，其中任何類型的值最終可以視為物件。</span><span class="sxs-lookup"><span data-stu-id="f034e-350">Boxing and unboxing enables a unified view of the type system wherein a value of any type can ultimately be treated as an object.</span></span>

### <a name="boxing-conversions"></a><span data-ttu-id="f034e-351">裝箱轉換</span><span class="sxs-lookup"><span data-stu-id="f034e-351">Boxing conversions</span></span>

<span data-ttu-id="f034e-352">「裝箱」轉換允許將*value_type*隱含地轉換成*reference_type*。</span><span class="sxs-lookup"><span data-stu-id="f034e-352">A boxing conversion permits a *value_type* to be implicitly converted to a *reference_type*.</span></span> <span data-ttu-id="f034e-353">有下列的裝箱轉換：</span><span class="sxs-lookup"><span data-stu-id="f034e-353">The following boxing conversions exist:</span></span>

*  <span data-ttu-id="f034e-354">從任何*value_type*到類型 `object`。</span><span class="sxs-lookup"><span data-stu-id="f034e-354">From any *value_type* to the type `object`.</span></span>
*  <span data-ttu-id="f034e-355">從任何*value_type*到類型 `System.ValueType`。</span><span class="sxs-lookup"><span data-stu-id="f034e-355">From any *value_type* to the type `System.ValueType`.</span></span>
*  <span data-ttu-id="f034e-356">從任何*non_nullable_value_type*到*value_type*所執行的任何*interface_type* 。</span><span class="sxs-lookup"><span data-stu-id="f034e-356">From any *non_nullable_value_type* to any *interface_type* implemented by the *value_type*.</span></span>
*  <span data-ttu-id="f034e-357">從任何*nullable_type*到*nullable_type*基礎類型所實*interface_type*的任何。</span><span class="sxs-lookup"><span data-stu-id="f034e-357">From any *nullable_type* to any *interface_type* implemented by the underlying type of the *nullable_type*.</span></span>
*  <span data-ttu-id="f034e-358">從任何*enum_type*到類型 `System.Enum`。</span><span class="sxs-lookup"><span data-stu-id="f034e-358">From any *enum_type* to the type `System.Enum`.</span></span>
*  <span data-ttu-id="f034e-359">從任何具有基礎*enum_type*的*nullable_type*到類型 `System.Enum`。</span><span class="sxs-lookup"><span data-stu-id="f034e-359">From any *nullable_type* with an underlying *enum_type* to the type `System.Enum`.</span></span>
*  <span data-ttu-id="f034e-360">請注意，從型別參數隱含的轉換會當做「裝箱」轉換來執行（如果在執行時間，它最後會從實值型別轉換成引用型別（[包括型別參數的隱含轉換](conversions.md#implicit-conversions-involving-type-parameters)）。</span><span class="sxs-lookup"><span data-stu-id="f034e-360">Note that an implicit conversion from a type parameter will be executed as a boxing conversion if at run-time it ends up converting from a value type to a reference type ([Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters)).</span></span>

<span data-ttu-id="f034e-361">為*non_nullable_value_type*的值進行裝箱，包括設定物件實例，以及將*non_nullable_value_type*值複製到該實例。</span><span class="sxs-lookup"><span data-stu-id="f034e-361">Boxing a value of a *non_nullable_value_type* consists of allocating an object instance and copying the *non_nullable_value_type* value into that instance.</span></span>

<span data-ttu-id="f034e-362">如果*nullable_type*的值為 `null` 值（`HasValue` `false`），或解除包裝並將基礎值裝箱的結果，則會產生 null 參考。</span><span class="sxs-lookup"><span data-stu-id="f034e-362">Boxing a value of a *nullable_type* produces a null reference if it is the `null` value (`HasValue` is `false`), or the result of unwrapping and boxing the underlying value otherwise.</span></span>

<span data-ttu-id="f034e-363">若要將*non_nullable_value_type*的值進行裝箱，最佳的處理方式是想像泛型的***裝箱類別***是否存在，其行為就如同宣告如下：</span><span class="sxs-lookup"><span data-stu-id="f034e-363">The actual process of boxing a value of a *non_nullable_value_type* is best explained by imagining the existence of a generic ***boxing class***, which behaves as if it were declared as follows:</span></span>

```csharp
sealed class Box<T>: System.ValueType
{
    T value;

    public Box(T t) {
        value = t;
    }
}
```

<span data-ttu-id="f034e-364">`T` 類型的值 `v` 的裝箱現在包含執行運算式 `new Box<T>(v)`，並以 `object`類型的值傳回產生的實例。</span><span class="sxs-lookup"><span data-stu-id="f034e-364">Boxing of a value `v` of type `T` now consists of executing the expression `new Box<T>(v)`, and returning the resulting instance as a value of type `object`.</span></span> <span data-ttu-id="f034e-365">因此，語句</span><span class="sxs-lookup"><span data-stu-id="f034e-365">Thus, the statements</span></span>
```csharp
int i = 123;
object box = i;
```
<span data-ttu-id="f034e-366">概念上對應至</span><span class="sxs-lookup"><span data-stu-id="f034e-366">conceptually correspond to</span></span>
```csharp
int i = 123;
object box = new Box<int>(i);
```

<span data-ttu-id="f034e-367">如上 `Box<T>` 的裝箱類別並不存在，而且已裝箱值的動態類型實際上不是類別類型。</span><span class="sxs-lookup"><span data-stu-id="f034e-367">A boxing class like `Box<T>` above doesn't actually exist and the dynamic type of a boxed value isn't actually a class type.</span></span> <span data-ttu-id="f034e-368">相反地，`T` 類型的已裝箱值具有動態類型 `T`，而使用 `is` 運算子的動態類型檢查可以直接參考類型 `T`。</span><span class="sxs-lookup"><span data-stu-id="f034e-368">Instead, a boxed value of type `T` has the dynamic type `T`, and a dynamic type check using the `is` operator can simply reference type `T`.</span></span> <span data-ttu-id="f034e-369">例如，套用至物件的</span><span class="sxs-lookup"><span data-stu-id="f034e-369">For example,</span></span>
```csharp
int i = 123;
object box = i;
if (box is int) {
    Console.Write("Box contains an int");
}
```
<span data-ttu-id="f034e-370">會在主控台上輸出字串 "`Box contains an int`"。</span><span class="sxs-lookup"><span data-stu-id="f034e-370">will output the string "`Box contains an int`" on the console.</span></span>

<span data-ttu-id="f034e-371">「裝箱」轉換意指建立所要封裝之值的複本。</span><span class="sxs-lookup"><span data-stu-id="f034e-371">A boxing conversion implies making a copy of the value being boxed.</span></span> <span data-ttu-id="f034e-372">這不同于*reference_type*轉換成類型 `object`，其中值會繼續參考相同的實例，而且只會被視為較少的衍生類型 `object`。</span><span class="sxs-lookup"><span data-stu-id="f034e-372">This is different from a conversion of a *reference_type* to type `object`, in which the value continues to reference the same instance and simply is regarded as the less derived type `object`.</span></span> <span data-ttu-id="f034e-373">例如，假設宣告</span><span class="sxs-lookup"><span data-stu-id="f034e-373">For example, given the declaration</span></span>
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
<span data-ttu-id="f034e-374">下列語句</span><span class="sxs-lookup"><span data-stu-id="f034e-374">the following statements</span></span>
```csharp
Point p = new Point(10, 10);
object box = p;
p.x = 20;
Console.Write(((Point)box).x);
```
<span data-ttu-id="f034e-375">會在主控台上輸出值10，因為 `box` 指派 `p` 中的隱含裝箱作業會導致複製 `p` 的值。</span><span class="sxs-lookup"><span data-stu-id="f034e-375">will output the value 10 on the console because the implicit boxing operation that occurs in the assignment of `p` to `box` causes the value of `p` to be copied.</span></span> <span data-ttu-id="f034e-376">已將 `Point` 宣告為 `class`，則值20會是輸出，因為 `p` 和 `box` 會參考相同的實例。</span><span class="sxs-lookup"><span data-stu-id="f034e-376">Had `Point` been declared a `class` instead, the value 20 would be output because `p` and `box` would reference the same instance.</span></span>

### <a name="unboxing-conversions"></a><span data-ttu-id="f034e-377">取消裝箱轉換</span><span class="sxs-lookup"><span data-stu-id="f034e-377">Unboxing conversions</span></span>

<span data-ttu-id="f034e-378">「取消裝箱」轉換允許將*reference_type*明確轉換成*value_type*。</span><span class="sxs-lookup"><span data-stu-id="f034e-378">An unboxing conversion permits a *reference_type* to be explicitly converted to a *value_type*.</span></span> <span data-ttu-id="f034e-379">下列的取消裝箱轉換已存在：</span><span class="sxs-lookup"><span data-stu-id="f034e-379">The following unboxing conversions exist:</span></span>

*  <span data-ttu-id="f034e-380">從 [類型] `object` 到任何*value_type*。</span><span class="sxs-lookup"><span data-stu-id="f034e-380">From the type `object` to any *value_type*.</span></span>
*  <span data-ttu-id="f034e-381">從 [類型] `System.ValueType` 到任何*value_type*。</span><span class="sxs-lookup"><span data-stu-id="f034e-381">From the type `System.ValueType` to any *value_type*.</span></span>
*  <span data-ttu-id="f034e-382">從任何*interface_type*到任何會執行*interface_type*的*non_nullable_value_type* 。</span><span class="sxs-lookup"><span data-stu-id="f034e-382">From any *interface_type* to any *non_nullable_value_type* that implements the *interface_type*.</span></span>
*  <span data-ttu-id="f034e-383">從任何*interface_type*到其基礎類型會實作為*interface_type*的任何*nullable_type* 。</span><span class="sxs-lookup"><span data-stu-id="f034e-383">From any *interface_type* to any *nullable_type* whose underlying type implements the *interface_type*.</span></span>
*  <span data-ttu-id="f034e-384">從 [類型] `System.Enum` 到任何*enum_type*。</span><span class="sxs-lookup"><span data-stu-id="f034e-384">From the type `System.Enum` to any *enum_type*.</span></span>
*  <span data-ttu-id="f034e-385">從類型 `System.Enum` 至具有基礎*enum_type*的任何*nullable_type* 。</span><span class="sxs-lookup"><span data-stu-id="f034e-385">From the type `System.Enum` to any *nullable_type* with an underlying *enum_type*.</span></span>
*  <span data-ttu-id="f034e-386">請注意，明確轉換為類型參數時，如果是在執行時間將其從參考型別轉換成實值型別（[明確動態轉換](conversions.md#explicit-dynamic-conversions)），就會以「取消鎖定」轉換的方式執行。</span><span class="sxs-lookup"><span data-stu-id="f034e-386">Note that an explicit conversion to a type parameter will be executed as an unboxing conversion if at run-time it ends up converting from a reference type to a value type ([Explicit dynamic conversions](conversions.md#explicit-dynamic-conversions)).</span></span>

<span data-ttu-id="f034e-387">*Non_nullable_value_type*的取消裝箱作業包含第一次檢查物件實例是否為給定*non_nullable_value_type*的已裝箱值，然後將值從實例中複製出來。</span><span class="sxs-lookup"><span data-stu-id="f034e-387">An unboxing operation to a *non_nullable_value_type* consists of first checking that the object instance is a boxed value of the given *non_nullable_value_type*, and then copying the value out of the instance.</span></span>

<span data-ttu-id="f034e-388">如果 `null`來源運算元，則取消裝箱至*nullable_type*會產生*nullable_type*的 null 值，否則會將物件實例取消包裝到*nullable_type*的基礎類型。</span><span class="sxs-lookup"><span data-stu-id="f034e-388">Unboxing to a *nullable_type* produces the null value of the *nullable_type* if the source operand is `null`, or the wrapped result of unboxing the object instance to the underlying type of the *nullable_type* otherwise.</span></span>

<span data-ttu-id="f034e-389">參考上一節所述的虛數裝箱類別，將物件 `box` 的取消裝箱轉換為*value_type* `T` 由執行運算式 `((Box<T>)box).value`所組成。</span><span class="sxs-lookup"><span data-stu-id="f034e-389">Referring to the imaginary boxing class described in the previous section, an unboxing conversion of an object `box` to a *value_type* `T` consists of executing the expression `((Box<T>)box).value`.</span></span> <span data-ttu-id="f034e-390">因此，語句</span><span class="sxs-lookup"><span data-stu-id="f034e-390">Thus, the statements</span></span>
```csharp
object box = 123;
int i = (int)box;
```
<span data-ttu-id="f034e-391">概念上對應至</span><span class="sxs-lookup"><span data-stu-id="f034e-391">conceptually correspond to</span></span>
```csharp
object box = new Box<int>(123);
int i = ((Box<int>)box).value;
```

<span data-ttu-id="f034e-392">對於給定*non_nullable_value_type*在執行時間成功的取消裝箱轉換，來源運算元的值必須是該*non_nullable_value_type*之已裝箱值的參考。</span><span class="sxs-lookup"><span data-stu-id="f034e-392">For an unboxing conversion to a given *non_nullable_value_type* to succeed at run-time, the value of the source operand must be a reference to a boxed value of that *non_nullable_value_type*.</span></span> <span data-ttu-id="f034e-393">如果 `null`來源運算元，則會擲回 `System.NullReferenceException`。</span><span class="sxs-lookup"><span data-stu-id="f034e-393">If the source operand is `null`, a `System.NullReferenceException` is thrown.</span></span> <span data-ttu-id="f034e-394">如果來源運算元是不相容物件的參考，則會擲回 `System.InvalidCastException`。</span><span class="sxs-lookup"><span data-stu-id="f034e-394">If the source operand is a reference to an incompatible object, a `System.InvalidCastException` is thrown.</span></span>

<span data-ttu-id="f034e-395">對於給定*nullable_type*在執行時間成功的取消裝箱轉換，來源運算元的值必須是 `null` 或*nullable_type*之基礎*non_nullable_value_type*的已裝箱值的參考。</span><span class="sxs-lookup"><span data-stu-id="f034e-395">For an unboxing conversion to a given *nullable_type* to succeed at run-time, the value of the source operand must be either `null` or a reference to a boxed value of the underlying *non_nullable_value_type* of the *nullable_type*.</span></span> <span data-ttu-id="f034e-396">如果來源運算元是不相容物件的參考，則會擲回 `System.InvalidCastException`。</span><span class="sxs-lookup"><span data-stu-id="f034e-396">If the source operand is a reference to an incompatible object, a `System.InvalidCastException` is thrown.</span></span>

## <a name="constructed-types"></a><span data-ttu-id="f034e-397">結構化類型</span><span class="sxs-lookup"><span data-stu-id="f034e-397">Constructed types</span></span>

<span data-ttu-id="f034e-398">泛型型別宣告本身則代表一個未系結的***泛型型***別，它是用來做為「藍圖」，藉由套用型別***引數***來形成許多不同的類型。</span><span class="sxs-lookup"><span data-stu-id="f034e-398">A generic type declaration, by itself, denotes an ***unbound generic type*** that is used as a "blueprint" to form many different types, by way of applying ***type arguments***.</span></span> <span data-ttu-id="f034e-399">型別引數是以在泛型型別名稱之後的角括弧（`<` 和 `>`）撰寫。</span><span class="sxs-lookup"><span data-stu-id="f034e-399">The type arguments are written within angle brackets (`<` and `>`) immediately following the name of the generic type.</span></span> <span data-ttu-id="f034e-400">包含至少一個型別引數的型別稱為「***結構化型***別」。</span><span class="sxs-lookup"><span data-stu-id="f034e-400">A type that includes at least one type argument is called a ***constructed type***.</span></span> <span data-ttu-id="f034e-401">在可顯示類型名稱的語言中，大部分的位置都可以使用結構化類型。</span><span class="sxs-lookup"><span data-stu-id="f034e-401">A constructed type can be used in most places in the language in which a type name can appear.</span></span> <span data-ttu-id="f034e-402">未系結的泛型型別只能用在*typeof_expression* （[typeof 運算子](expressions.md#the-typeof-operator)）內。</span><span class="sxs-lookup"><span data-stu-id="f034e-402">An unbound generic type can only be used within a *typeof_expression* ([The typeof operator](expressions.md#the-typeof-operator)).</span></span>

<span data-ttu-id="f034e-403">結構化類型也可以在運算式中當做簡單名稱（[簡單名稱](expressions.md#simple-names)），或在存取成員（[成員存取](expressions.md#member-access)）時使用。</span><span class="sxs-lookup"><span data-stu-id="f034e-403">Constructed types can also be used in expressions as simple names ([Simple names](expressions.md#simple-names)) or when accessing a member ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="f034e-404">評估*namespace_or_type_name*時，只會考慮具有正確數目之類型參數的泛型型別。</span><span class="sxs-lookup"><span data-stu-id="f034e-404">When a *namespace_or_type_name* is evaluated, only generic types with the correct number of type parameters are considered.</span></span> <span data-ttu-id="f034e-405">因此，只要類型的類型參數數目不同，就可以使用相同的識別碼來識別不同的類型。</span><span class="sxs-lookup"><span data-stu-id="f034e-405">Thus, it is possible to use the same identifier to identify different types, as long as the types have different numbers of type parameters.</span></span> <span data-ttu-id="f034e-406">在相同的程式中混合泛型和非泛型類別時，這會很有用：</span><span class="sxs-lookup"><span data-stu-id="f034e-406">This is useful when mixing generic and non-generic classes in the same program:</span></span>

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

<span data-ttu-id="f034e-407">*Type_name*可能會識別已建立的類型，即使它並未直接指定類型參數也一樣。</span><span class="sxs-lookup"><span data-stu-id="f034e-407">A *type_name* might identify a constructed type even though it doesn't specify type parameters directly.</span></span> <span data-ttu-id="f034e-408">這種情況可能發生在泛型類別宣告內的類型，而且包含宣告的實例類型會隱含用於名稱查閱（[泛型類別中的巢狀型別](classes.md#nested-types-in-generic-classes)）：</span><span class="sxs-lookup"><span data-stu-id="f034e-408">This can occur where a type is nested within a generic class declaration, and the instance type of the containing declaration is implicitly used for name lookup ([Nested types in generic classes](classes.md#nested-types-in-generic-classes)):</span></span>

```csharp
class Outer<T>
{
    public class Inner {...}

    public Inner i;                // Type of i is Outer<T>.Inner
}
```

<span data-ttu-id="f034e-409">在 unsafe 程式碼中，不能使用結構化類型做為*unmanaged_type* （[指標類型](unsafe-code.md#pointer-types)）。</span><span class="sxs-lookup"><span data-stu-id="f034e-409">In unsafe code, a constructed type cannot be used as an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

### <a name="type-arguments"></a><span data-ttu-id="f034e-410">型別引數</span><span class="sxs-lookup"><span data-stu-id="f034e-410">Type arguments</span></span>

<span data-ttu-id="f034e-411">型別引數清單中的每個引數都只是一個*型*別。</span><span class="sxs-lookup"><span data-stu-id="f034e-411">Each argument in a type argument list is simply a *type*.</span></span>

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

<span data-ttu-id="f034e-412">在 unsafe 程式碼（[unsafe 程式碼](unsafe-code.md)）中， *type_argument*可能不是指標類型。</span><span class="sxs-lookup"><span data-stu-id="f034e-412">In unsafe code ([Unsafe code](unsafe-code.md)), a *type_argument* may not be a pointer type.</span></span> <span data-ttu-id="f034e-413">每個型別引數都必須滿足對應型別參數的任何條件約束（[型別參數條件約束](classes.md#type-parameter-constraints)）。</span><span class="sxs-lookup"><span data-stu-id="f034e-413">Each type argument must satisfy any constraints on the corresponding type parameter ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>

### <a name="open-and-closed-types"></a><span data-ttu-id="f034e-414">開啟和關閉類型</span><span class="sxs-lookup"><span data-stu-id="f034e-414">Open and closed types</span></span>

<span data-ttu-id="f034e-415">所有類型都可以分類為***開放式類型***或***封閉式類型***。</span><span class="sxs-lookup"><span data-stu-id="f034e-415">All types can be classified as either ***open types*** or ***closed types***.</span></span> <span data-ttu-id="f034e-416">「開啟類型」是包含型別參數的型別。</span><span class="sxs-lookup"><span data-stu-id="f034e-416">An open type is a type that involves type parameters.</span></span> <span data-ttu-id="f034e-417">更具體而言：</span><span class="sxs-lookup"><span data-stu-id="f034e-417">More specifically:</span></span>

*  <span data-ttu-id="f034e-418">型別參數會定義開放式型別。</span><span class="sxs-lookup"><span data-stu-id="f034e-418">A type parameter defines an open type.</span></span>
*  <span data-ttu-id="f034e-419">只有在其專案類型為開放式類型時，陣列類型才是開啟類型。</span><span class="sxs-lookup"><span data-stu-id="f034e-419">An array type is an open type if and only if its element type is an open type.</span></span>
*  <span data-ttu-id="f034e-420">如果只有一或多個型別引數是開放式型別，則結構化型別就是開放式型別。</span><span class="sxs-lookup"><span data-stu-id="f034e-420">A constructed type is an open type if and only if one or more of its type arguments is an open type.</span></span> <span data-ttu-id="f034e-421">只有當一個或多個類型引數或其包含類型的類型引數是開放式類型時，結構化的巢狀型別才是開放式類型。</span><span class="sxs-lookup"><span data-stu-id="f034e-421">A constructed nested type is an open type if and only if one or more of its type arguments or the type arguments of its containing type(s) is an open type.</span></span>

<span data-ttu-id="f034e-422">封閉式類型是不是開放式類型的類型。</span><span class="sxs-lookup"><span data-stu-id="f034e-422">A closed type is a type that is not an open type.</span></span>

<span data-ttu-id="f034e-423">在執行時間，泛型型別宣告內的所有程式碼都是在封閉式結構型別的內容中執行，而這是透過將型別引數套用至泛型宣告所建立。</span><span class="sxs-lookup"><span data-stu-id="f034e-423">At run-time, all of the code within a generic type declaration is executed in the context of a closed constructed type that was created by applying type arguments to the generic declaration.</span></span> <span data-ttu-id="f034e-424">泛型型別中的每個型別參數都會系結至特定的執行時間型別。</span><span class="sxs-lookup"><span data-stu-id="f034e-424">Each type parameter within the generic type is bound to a particular run-time type.</span></span> <span data-ttu-id="f034e-425">所有語句和運算式的執行時間處理一律會發生于封閉式類型，而開放式類型只會在編譯時間處理期間發生。</span><span class="sxs-lookup"><span data-stu-id="f034e-425">The run-time processing of all statements and expressions always occurs with closed types, and open types occur only during compile-time processing.</span></span>

<span data-ttu-id="f034e-426">每個封閉的結構類型都有自己的靜態變數集，不會與任何其他已關閉的結構類型共用。</span><span class="sxs-lookup"><span data-stu-id="f034e-426">Each closed constructed type has its own set of static variables, which are not shared with any other closed constructed types.</span></span> <span data-ttu-id="f034e-427">由於開啟的類型不存在於執行時間，因此沒有與開放式類型相關聯的靜態變數。</span><span class="sxs-lookup"><span data-stu-id="f034e-427">Since an open type does not exist at run-time, there are no static variables associated with an open type.</span></span> <span data-ttu-id="f034e-428">如果兩個封閉的結構型別是從相同的未系結泛型型別進行構造，而且其對應的型別引數都是相同的型別，</span><span class="sxs-lookup"><span data-stu-id="f034e-428">Two closed constructed types are the same type if they are constructed from the same unbound generic type, and their corresponding type arguments are the same type.</span></span>

### <a name="bound-and-unbound-types"></a><span data-ttu-id="f034e-429">系結和未系結類型</span><span class="sxs-lookup"><span data-stu-id="f034e-429">Bound and unbound types</span></span>

<span data-ttu-id="f034e-430">「未系結***類型***」一詞指的是非泛型型別或未系結的泛型型別。</span><span class="sxs-lookup"><span data-stu-id="f034e-430">The term ***unbound type*** refers to a non-generic type or an unbound generic type.</span></span> <span data-ttu-id="f034e-431">「系結***型***別」一詞指的是非泛型型別或結構化型別。</span><span class="sxs-lookup"><span data-stu-id="f034e-431">The term ***bound type*** refers to a non-generic type or a constructed type.</span></span>

<span data-ttu-id="f034e-432">未系結的型別是指由型別宣告所宣告的實體。</span><span class="sxs-lookup"><span data-stu-id="f034e-432">An unbound type refers to the entity declared by a type declaration.</span></span> <span data-ttu-id="f034e-433">未系結的泛型型別本身不是類型，而且不能當做變數、引數或傳回值的類型或基底類型使用。</span><span class="sxs-lookup"><span data-stu-id="f034e-433">An unbound generic type is not itself a type, and cannot be used as the type of a variable, argument or return value, or as a base type.</span></span> <span data-ttu-id="f034e-434">唯一可以參考未系結泛型型別的結構是 `typeof` 運算式（[typeof 運算子](expressions.md#the-typeof-operator)）。</span><span class="sxs-lookup"><span data-stu-id="f034e-434">The only construct in which an unbound generic type can be referenced is the `typeof` expression ([The typeof operator](expressions.md#the-typeof-operator)).</span></span>

### <a name="satisfying-constraints"></a><span data-ttu-id="f034e-435">滿足條件約束</span><span class="sxs-lookup"><span data-stu-id="f034e-435">Satisfying constraints</span></span>

<span data-ttu-id="f034e-436">每當參考了結構化型別或泛型方法時，就會針對泛型型別或方法（[型別參數條件約束](classes.md#type-parameter-constraints)）上宣告的型別參數條件約束，檢查提供的型別引數。</span><span class="sxs-lookup"><span data-stu-id="f034e-436">Whenever a constructed type or generic method is referenced, the supplied type arguments are checked against the type parameter constraints declared on the generic type or method ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="f034e-437">針對每個 `where` 子句，會針對每個條件約束檢查對應至命名類型參數的類型引數 `A`，如下所示：</span><span class="sxs-lookup"><span data-stu-id="f034e-437">For each `where` clause, the type argument `A` that corresponds to the named type parameter is checked against each constraint as follows:</span></span>

*  <span data-ttu-id="f034e-438">如果條件約束是類別類型、介面類別型或類型參數，則 let `C` 會以提供的類型引數取代條件約束中出現的任何類型參數，來表示該條件約束。</span><span class="sxs-lookup"><span data-stu-id="f034e-438">If the constraint is a class type, an interface type, or a type parameter, let `C` represent that constraint with the supplied type arguments substituted for any type parameters that appear in the constraint.</span></span> <span data-ttu-id="f034e-439">若要滿足條件約束，必須將類型 `A` 轉換成下列其中一項所 `C` 的類型：</span><span class="sxs-lookup"><span data-stu-id="f034e-439">To satisfy the constraint, it must be the case that type `A` is convertible to type `C` by one of the following:</span></span>
    * <span data-ttu-id="f034e-440">身分識別轉換（身分[識別轉換](conversions.md#identity-conversion)）</span><span class="sxs-lookup"><span data-stu-id="f034e-440">An identity conversion ([Identity conversion](conversions.md#identity-conversion))</span></span>
    * <span data-ttu-id="f034e-441">隱含參考轉換（[隱含參考轉換](conversions.md#implicit-reference-conversions)）</span><span class="sxs-lookup"><span data-stu-id="f034e-441">An implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions))</span></span>
    * <span data-ttu-id="f034e-442">如果類型 A 是不可為 null 的實數值型別，則為裝箱轉換（[裝箱轉換](conversions.md#boxing-conversions)）。</span><span class="sxs-lookup"><span data-stu-id="f034e-442">A boxing conversion ([Boxing conversions](conversions.md#boxing-conversions)), provided that type A is a non-nullable value type.</span></span>
    * <span data-ttu-id="f034e-443">從型別參數轉換成 `C`的隱含參考、裝箱或型別參數 `A`。</span><span class="sxs-lookup"><span data-stu-id="f034e-443">An implicit reference, boxing or type parameter conversion from a type parameter `A` to `C`.</span></span>
*  <span data-ttu-id="f034e-444">如果條件約束是參考型別條件約束（`class`），則類型 `A` 必須符合下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="f034e-444">If the constraint is the reference type constraint (`class`), the type `A` must satisfy one of the following:</span></span>
    * <span data-ttu-id="f034e-445">`A` 是介面型別、類別型別、委派型別或陣列型別。</span><span class="sxs-lookup"><span data-stu-id="f034e-445">`A` is an interface type, class type, delegate type or array type.</span></span> <span data-ttu-id="f034e-446">請注意，`System.ValueType` 和 `System.Enum` 是符合此條件約束的參考型別。</span><span class="sxs-lookup"><span data-stu-id="f034e-446">Note that `System.ValueType` and `System.Enum` are reference types that satisfy this constraint.</span></span>
    * <span data-ttu-id="f034e-447">`A` 是已知為參考型別的型別參數（[型別參數條件約束](classes.md#type-parameter-constraints)）。</span><span class="sxs-lookup"><span data-stu-id="f034e-447">`A` is a type parameter that is known to be a reference type ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
*  <span data-ttu-id="f034e-448">如果條件約束是實值型別條件約束（`struct`），則型別 `A` 必須滿足下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="f034e-448">If the constraint is the value type constraint (`struct`), the type `A` must satisfy one of the following:</span></span>
    * <span data-ttu-id="f034e-449">`A` 是結構型別或列舉型別，但不能是可為 null 的型別。</span><span class="sxs-lookup"><span data-stu-id="f034e-449">`A` is a struct type or enum type, but not a nullable type.</span></span> <span data-ttu-id="f034e-450">請注意，`System.ValueType` 和 `System.Enum` 是不符合此條件約束的參考型別。</span><span class="sxs-lookup"><span data-stu-id="f034e-450">Note that `System.ValueType` and `System.Enum` are reference types that do not satisfy this constraint.</span></span>
    * <span data-ttu-id="f034e-451">`A` 是具有實數值型別條件約束的類型參數（[類型參數條件約束](classes.md#type-parameter-constraints)）。</span><span class="sxs-lookup"><span data-stu-id="f034e-451">`A` is a type parameter having the value type constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
*  <span data-ttu-id="f034e-452">如果條件約束是 `new()`的函式條件約束，則類型 `A` 不得 `abstract`，而且必須具有公用無參數的函式。</span><span class="sxs-lookup"><span data-stu-id="f034e-452">If the constraint is the constructor constraint `new()`, the type `A` must not be `abstract` and must have a public parameterless constructor.</span></span> <span data-ttu-id="f034e-453">如果下列其中一項為真，就會滿足此情況：</span><span class="sxs-lookup"><span data-stu-id="f034e-453">This is satisfied if one of the following is true:</span></span>
    * <span data-ttu-id="f034e-454">`A` 是實數值型別，因為所有實數值型別都具有公用預設的函式（[預設](types.md#default-constructors)的函式）。</span><span class="sxs-lookup"><span data-stu-id="f034e-454">`A` is a value type, since all value types have a public default constructor ([Default constructors](types.md#default-constructors)).</span></span>
    * <span data-ttu-id="f034e-455">`A` 是具有「函式條件約束」（[型別參數條件約束](classes.md#type-parameter-constraints)）的型別參數。</span><span class="sxs-lookup"><span data-stu-id="f034e-455">`A` is a type parameter having the constructor constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
    * <span data-ttu-id="f034e-456">`A` 是具有實數值型別條件約束的類型參數（[類型參數條件約束](classes.md#type-parameter-constraints)）。</span><span class="sxs-lookup"><span data-stu-id="f034e-456">`A` is a type parameter having the value type constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
    * <span data-ttu-id="f034e-457">`A` 是不 `abstract` 的類別，而且包含明確宣告的 `public` 不含任何參數的函式。</span><span class="sxs-lookup"><span data-stu-id="f034e-457">`A` is a class that is not `abstract` and contains an explicitly declared `public` constructor with no parameters.</span></span>
    * <span data-ttu-id="f034e-458">`A` 不 `abstract`，而且具有預設的函式（[預設](classes.md#default-constructors)的函式）。</span><span class="sxs-lookup"><span data-stu-id="f034e-458">`A` is not `abstract` and has a default constructor ([Default constructors](classes.md#default-constructors)).</span></span>

<span data-ttu-id="f034e-459">如果指定的型別引數不符合一個或多個型別參數的條件約束，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="f034e-459">A compile-time error occurs if one or more of a type parameter's constraints are not satisfied by the given type arguments.</span></span>

<span data-ttu-id="f034e-460">因為類型參數不會繼承，所以永遠不會繼承條件約束。</span><span class="sxs-lookup"><span data-stu-id="f034e-460">Since type parameters are not inherited, constraints are never inherited either.</span></span> <span data-ttu-id="f034e-461">在下列範例中，`D` 需要在其型別參數上指定條件約束 `T`，讓 `T` 滿足基類 `B<T>`所強加的條件約束。</span><span class="sxs-lookup"><span data-stu-id="f034e-461">In the example below, `D` needs to specify the constraint on its type parameter `T` so that `T` satisfies the constraint imposed by the base class `B<T>`.</span></span> <span data-ttu-id="f034e-462">相反地，類別 `E` 不需要指定條件約束，因為 `List<T>` 會針對任何 `T`來執行 `IEnumerable`。</span><span class="sxs-lookup"><span data-stu-id="f034e-462">In contrast, class `E` need not specify a constraint, because `List<T>` implements `IEnumerable` for any `T`.</span></span>

```csharp
class B<T> where T: IEnumerable {...}

class D<T>: B<T> where T: IEnumerable {...}

class E<T>: B<List<T>> {...}
```

## <a name="type-parameters"></a><span data-ttu-id="f034e-463">型別參數</span><span class="sxs-lookup"><span data-stu-id="f034e-463">Type parameters</span></span>

<span data-ttu-id="f034e-464">型別參數是一個識別碼，用來指定在執行時間系結參數的實值型別或參考型別。</span><span class="sxs-lookup"><span data-stu-id="f034e-464">A type parameter is an identifier designating a value type or reference type that the parameter is bound to at run-time.</span></span>

```antlr
type_parameter
    : identifier
    ;
```

<span data-ttu-id="f034e-465">因為類型參數可以使用許多不同的實際類型引數具現化，所以類型參數與其他類型的作業和限制會稍有不同。</span><span class="sxs-lookup"><span data-stu-id="f034e-465">Since a type parameter can be instantiated with many different actual type arguments, type parameters have slightly different operations and restrictions than other types.</span></span> <span data-ttu-id="f034e-466">它們包括：</span><span class="sxs-lookup"><span data-stu-id="f034e-466">These include:</span></span>

*  <span data-ttu-id="f034e-467">型別參數不能直接用來宣告基類（[基類](classes.md#base-class)）或介面（[Variant 型別參數清單](interfaces.md#variant-type-parameter-lists)）。</span><span class="sxs-lookup"><span data-stu-id="f034e-467">A type parameter cannot be used directly to declare a base class ([Base class](classes.md#base-class)) or interface ([Variant type parameter lists](interfaces.md#variant-type-parameter-lists)).</span></span>
*  <span data-ttu-id="f034e-468">類型參數上成員查閱的規則取決於套用至類型參數的條件約束（如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="f034e-468">The rules for member lookup on type parameters depend on the constraints, if any, applied to the type parameter.</span></span> <span data-ttu-id="f034e-469">它們會在[成員查閱](expressions.md#member-lookup)中詳述。</span><span class="sxs-lookup"><span data-stu-id="f034e-469">They are detailed in [Member lookup](expressions.md#member-lookup).</span></span>
*  <span data-ttu-id="f034e-470">型別參數的可用轉換取決於套用至型別參數的條件約束（如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="f034e-470">The available conversions for a type parameter depend on the constraints, if any, applied to the type parameter.</span></span> <span data-ttu-id="f034e-471">它們會在[涉及型別參數](conversions.md#implicit-conversions-involving-type-parameters)和[明確動態轉換](conversions.md#explicit-dynamic-conversions)的隱含轉換中詳述。</span><span class="sxs-lookup"><span data-stu-id="f034e-471">They are detailed in [Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters) and [Explicit dynamic conversions](conversions.md#explicit-dynamic-conversions).</span></span>
*  <span data-ttu-id="f034e-472">常值 `null` 無法轉換成型別參數所指定的型別，除非已知型別參數是引用型別（[涉及型別參數的隱含轉換](conversions.md#implicit-conversions-involving-type-parameters)）。</span><span class="sxs-lookup"><span data-stu-id="f034e-472">The literal `null` cannot be converted to a type given by a type parameter, except if the type parameter is known to be a reference type ([Implicit conversions involving type parameters](conversions.md#implicit-conversions-involving-type-parameters)).</span></span> <span data-ttu-id="f034e-473">不過，您可以改為使用 `default` 運算式（[預設值運算式](expressions.md#default-value-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="f034e-473">However, a `default` expression ([Default value expressions](expressions.md#default-value-expressions)) can be used instead.</span></span> <span data-ttu-id="f034e-474">此外，除非類型參數具有實數值型別條件約束，否則，類型參數所指定之類型的值可以與使用 `==` 和 `!=` （[參考類型等號比較運算子](expressions.md#reference-type-equality-operators)）的 `null` 進行比較。</span><span class="sxs-lookup"><span data-stu-id="f034e-474">In addition, a value with a type given by a type parameter can be compared with `null` using `==` and `!=` ([Reference type equality operators](expressions.md#reference-type-equality-operators)) unless the type parameter has the value type constraint.</span></span>
*  <span data-ttu-id="f034e-475">如果型別參數受到*constructor_constraint*或實值型別條件約束（[型別參數條件約束](classes.md#type-parameter-constraints)）的限制，則 `new` 運算式（[物件建立運算式](expressions.md#object-creation-expressions)）只能與型別參數一起使用。</span><span class="sxs-lookup"><span data-stu-id="f034e-475">A `new` expression ([Object creation expressions](expressions.md#object-creation-expressions)) can only be used with a type parameter if the type parameter is constrained by a *constructor_constraint* or the value type constraint ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span>
*  <span data-ttu-id="f034e-476">類型參數不能用在屬性內的任何位置。</span><span class="sxs-lookup"><span data-stu-id="f034e-476">A type parameter cannot be used anywhere within an attribute.</span></span>
*  <span data-ttu-id="f034e-477">型別參數不能用在成員存取（[成員存取](expressions.md#member-access)）或型別名稱（[命名空間和型別名稱](basic-concepts.md#namespace-and-type-names)）中，以識別靜態成員或嵌套型別。</span><span class="sxs-lookup"><span data-stu-id="f034e-477">A type parameter cannot be used in a member access ([Member access](expressions.md#member-access)) or type name ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) to identify a static member or a nested type.</span></span>
*  <span data-ttu-id="f034e-478">在 unsafe 程式碼中，類型參數不能當做*unmanaged_type* （[指標類型](unsafe-code.md#pointer-types)）使用。</span><span class="sxs-lookup"><span data-stu-id="f034e-478">In unsafe code, a type parameter cannot be used as an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

<span data-ttu-id="f034e-479">就型別而言，型別參數純粹是編譯時間結構。</span><span class="sxs-lookup"><span data-stu-id="f034e-479">As a type, type parameters are purely a compile-time construct.</span></span> <span data-ttu-id="f034e-480">在執行時間，每個型別參數都會系結至執行時間型別，其指定方式是提供泛型型別宣告的型別引數。</span><span class="sxs-lookup"><span data-stu-id="f034e-480">At run-time, each type parameter is bound to a run-time type that was specified by supplying a type argument to the generic type declaration.</span></span> <span data-ttu-id="f034e-481">因此，使用型別參數宣告之變數的型別將會在執行時間為封閉的結構化型別（[開放式和封閉式類型](types.md#open-and-closed-types)）。</span><span class="sxs-lookup"><span data-stu-id="f034e-481">Thus, the type of a variable declared with a type parameter will, at run-time, be a closed constructed type ([Open and closed types](types.md#open-and-closed-types)).</span></span> <span data-ttu-id="f034e-482">所有涉及型別參數之語句和運算式的執行時間執行，都會使用當做該參數的型別引數提供的實際型別。</span><span class="sxs-lookup"><span data-stu-id="f034e-482">The run-time execution of all statements and expressions involving type parameters uses the actual type that was supplied as the type argument for that parameter.</span></span>

## <a name="expression-tree-types"></a><span data-ttu-id="f034e-483">運算式樹狀架構類型</span><span class="sxs-lookup"><span data-stu-id="f034e-483">Expression tree types</span></span>

<span data-ttu-id="f034e-484">***運算式樹狀***架構允許將 lambda 運算式表示為資料結構，而不是可執行檔程式碼。</span><span class="sxs-lookup"><span data-stu-id="f034e-484">***Expression trees*** permit lambda expressions to be represented as data structures instead of executable code.</span></span> <span data-ttu-id="f034e-485">運算式樹狀架構是 `System.Linq.Expressions.Expression<D>`形式之***運算式樹狀架構類型***的值，其中 `D` 是任何委派類型。</span><span class="sxs-lookup"><span data-stu-id="f034e-485">Expression trees are values of ***expression tree types*** of the form `System.Linq.Expressions.Expression<D>`, where `D` is any delegate type.</span></span> <span data-ttu-id="f034e-486">針對此規格的其餘部分，我們將使用速記 `Expression<D>`來參考這些類型。</span><span class="sxs-lookup"><span data-stu-id="f034e-486">For the remainder of this specification we will refer to these types using the shorthand `Expression<D>`.</span></span>

<span data-ttu-id="f034e-487">如果從 lambda 運算式到委派類型 `D`的轉換存在，`Expression<D>`的運算式樹狀架構類型也會有轉換。</span><span class="sxs-lookup"><span data-stu-id="f034e-487">If a conversion exists from a lambda expression to a delegate type `D`, a conversion also exists to the expression tree type `Expression<D>`.</span></span> <span data-ttu-id="f034e-488">當 lambda 運算式轉換成委派類型時，會產生一個委派來參考 lambda 運算式的可執行程式碼，而轉換成運算式樹狀架構類型會建立 lambda 運算式的運算式樹狀結構標記法。</span><span class="sxs-lookup"><span data-stu-id="f034e-488">Whereas the conversion of a lambda expression to a delegate type generates a delegate that references executable code for the lambda expression, conversion to an expression tree type creates an expression tree representation of the lambda expression.</span></span>

<span data-ttu-id="f034e-489">運算式樹狀架構是 lambda 運算式的有效率記憶體內部資料標記法，並可讓 lambda 運算式的結構變成透明和明確。</span><span class="sxs-lookup"><span data-stu-id="f034e-489">Expression trees are efficient in-memory data representations of lambda expressions and make the structure of the lambda expression transparent and explicit.</span></span>

<span data-ttu-id="f034e-490">就像委派型別 `D`，`Expression<D>` 會有參數和傳回型別，這與 `D`的類型相同。</span><span class="sxs-lookup"><span data-stu-id="f034e-490">Just like a delegate type `D`, `Expression<D>` is said to have parameter and return types, which are the same as those of `D`.</span></span>

<span data-ttu-id="f034e-491">下列範例會將 lambda 運算式表示為可執行程式碼和運算式樹狀架構。</span><span class="sxs-lookup"><span data-stu-id="f034e-491">The following example represents a lambda expression both as executable code and as an expression tree.</span></span> <span data-ttu-id="f034e-492">因為 `Func<int,int>`的轉換存在，`Expression<Func<int,int>>`的轉換也存在：</span><span class="sxs-lookup"><span data-stu-id="f034e-492">Because a conversion exists to `Func<int,int>`, a conversion also exists to `Expression<Func<int,int>>`:</span></span>

```csharp
Func<int,int> del = x => x + 1;                    // Code

Expression<Func<int,int>> exp = x => x + 1;        // Data
```

<span data-ttu-id="f034e-493">在這些指派之後，委派 `del` 會參考傳回 `x + 1`的方法，而且運算式樹狀架構 `exp` 會參考描述運算式 `x => x + 1`的資料結構。</span><span class="sxs-lookup"><span data-stu-id="f034e-493">Following these assignments, the delegate `del` references a method that returns `x + 1`, and the expression tree `exp` references a data structure that describes the expression `x => x + 1`.</span></span>

<span data-ttu-id="f034e-494">泛型型別的確切定義 `Expression<D>` 以及將 lambda 運算式轉換成運算式樹狀架構類型時，用來建立運算式樹狀架構的精確規則，兩者都在此規格的範圍之外。</span><span class="sxs-lookup"><span data-stu-id="f034e-494">The exact definition of the generic type `Expression<D>` as well as the precise rules for constructing an expression tree when a lambda expression is converted to an expression tree type, are both outside the scope of this specification.</span></span>

<span data-ttu-id="f034e-495">明確來說，有兩件事要特別注意：</span><span class="sxs-lookup"><span data-stu-id="f034e-495">Two things are important to make explicit:</span></span>

*  <span data-ttu-id="f034e-496">並非所有 lambda 運算式都可以轉換成運算式樹狀架構。</span><span class="sxs-lookup"><span data-stu-id="f034e-496">Not all lambda expressions can be converted to expression trees.</span></span> <span data-ttu-id="f034e-497">例如，具有語句主體的 lambda 運算式，以及包含指派運算式的 lambda 運算式無法表示。</span><span class="sxs-lookup"><span data-stu-id="f034e-497">For instance, lambda expressions with statement bodies, and lambda expressions containing assignment expressions cannot be represented.</span></span> <span data-ttu-id="f034e-498">在這些情況下，轉換仍然存在，但會在編譯時間失敗。</span><span class="sxs-lookup"><span data-stu-id="f034e-498">In these cases, a conversion still exists, but will fail at compile-time.</span></span> <span data-ttu-id="f034e-499">這些例外狀況詳述于[匿名函數轉換](conversions.md#anonymous-function-conversions)中。</span><span class="sxs-lookup"><span data-stu-id="f034e-499">These exceptions are detailed in [Anonymous function conversions](conversions.md#anonymous-function-conversions).</span></span>
*   <span data-ttu-id="f034e-500">`Expression<D>` 提供實例方法 `Compile`，其會產生類型 `D`的委派：</span><span class="sxs-lookup"><span data-stu-id="f034e-500">`Expression<D>` offers an instance method `Compile` which produces a delegate of type `D`:</span></span>

    ```csharp
    Func<int,int> del2 = exp.Compile();
    ```

    <span data-ttu-id="f034e-501">叫用此委派會使運算式樹狀架構所代表的程式碼執行。</span><span class="sxs-lookup"><span data-stu-id="f034e-501">Invoking this delegate causes the code represented by the expression tree to be executed.</span></span> <span data-ttu-id="f034e-502">因此，在上述定義中，del 和 del2 是相同的，而下列兩個語句會有相同的效果：</span><span class="sxs-lookup"><span data-stu-id="f034e-502">Thus, given the definitions above, del and del2 are equivalent, and the following two statements will have the same effect:</span></span>

    ```csharp
    int i1 = del(1);
    
    int i2 = del2(1);
    ```

    <span data-ttu-id="f034e-503">執行此程式碼之後，`i1` 和 `i2` 都會有 `2`的值。</span><span class="sxs-lookup"><span data-stu-id="f034e-503">After executing this code,  `i1` and `i2` will both have the value `2`.</span></span>

