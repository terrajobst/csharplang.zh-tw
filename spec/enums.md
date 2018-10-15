# <a name="enums"></a><span data-ttu-id="92289-101">列舉</span><span class="sxs-lookup"><span data-stu-id="92289-101">Enums</span></span>

<span data-ttu-id="92289-102">***列舉型別***不同的實值型別 ([實值型別](types.md#value-types)) 宣告一組具名常數。</span><span class="sxs-lookup"><span data-stu-id="92289-102">An ***enum type*** is a distinct value type ([Value types](types.md#value-types)) that declares a set of named constants.</span></span>

<span data-ttu-id="92289-103">此範例</span><span class="sxs-lookup"><span data-stu-id="92289-103">The example</span></span>

```csharp
enum Color
{
    Red,
    Green,
    Blue
}
```

<span data-ttu-id="92289-104">宣告名為列舉型別`Color`與成員`Red`， `Green`，和`Blue`。</span><span class="sxs-lookup"><span data-stu-id="92289-104">declares an enum type named `Color` with members `Red`, `Green`, and `Blue`.</span></span>

## <a name="enum-declarations"></a><span data-ttu-id="92289-105">列舉宣告</span><span class="sxs-lookup"><span data-stu-id="92289-105">Enum declarations</span></span>

<span data-ttu-id="92289-106">列舉宣告會宣告新的列舉型別。</span><span class="sxs-lookup"><span data-stu-id="92289-106">An enum declaration declares a new enum type.</span></span> <span data-ttu-id="92289-107">列舉宣告的開頭關鍵字`enum`，並定義名稱、 協助工具、 基礎型別和列舉的成員。</span><span class="sxs-lookup"><span data-stu-id="92289-107">An enum declaration begins with the keyword `enum`, and defines the name, accessibility, underlying type, and members of the enum.</span></span>

```antlr
enum_declaration
    : attributes? enum_modifier* 'enum' identifier enum_base? enum_body ';'?
    ;

enum_base
    : ':' integral_type
    ;

enum_body
    : '{' enum_member_declarations? '}'
    | '{' enum_member_declarations ',' '}'
    ;
```

<span data-ttu-id="92289-108">每個列舉類型都有對應的整數類資料類型，稱為***基礎型別***的列舉型別。</span><span class="sxs-lookup"><span data-stu-id="92289-108">Each enum type has a corresponding integral type called the ***underlying type*** of the enum type.</span></span> <span data-ttu-id="92289-109">這個基礎類型必須能夠代表列舉中定義的所有列舉值。</span><span class="sxs-lookup"><span data-stu-id="92289-109">This underlying type must be able to represent all the enumerator values defined in the enumeration.</span></span> <span data-ttu-id="92289-110">列舉宣告可以明確宣告基礎型別`byte`， `sbyte`， `short`， `ushort`， `int`， `uint`，`long`或`ulong`。</span><span class="sxs-lookup"><span data-stu-id="92289-110">An enum declaration may explicitly declare an underlying type of `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` or `ulong`.</span></span> <span data-ttu-id="92289-111">請注意，`char`不能做為基礎的類型。</span><span class="sxs-lookup"><span data-stu-id="92289-111">Note that `char` cannot be used as an underlying type.</span></span> <span data-ttu-id="92289-112">沒有明確宣告的基礎類型的列舉宣告都具有基礎類型的`int`。</span><span class="sxs-lookup"><span data-stu-id="92289-112">An enum declaration that does not explicitly declare an underlying type has an underlying type of `int`.</span></span>

<span data-ttu-id="92289-113">此範例</span><span class="sxs-lookup"><span data-stu-id="92289-113">The example</span></span>

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

<span data-ttu-id="92289-114">宣告列舉的基礎型別`long`。</span><span class="sxs-lookup"><span data-stu-id="92289-114">declares an enum with an underlying type of `long`.</span></span> <span data-ttu-id="92289-115">開發人員可能會選擇使用的基礎類型`long`，如範例所示，若要允許的範圍中的值使用`long`但不是在範圍內的`int`，或保留供日後使用此選項。</span><span class="sxs-lookup"><span data-stu-id="92289-115">A developer might choose to use an underlying type of `long`, as in the example, to enable the use of values that are in the range of `long` but not in the range of `int`, or to preserve this option for the future.</span></span>

## <a name="enum-modifiers"></a><span data-ttu-id="92289-116">列舉修飾詞</span><span class="sxs-lookup"><span data-stu-id="92289-116">Enum modifiers</span></span>

<span data-ttu-id="92289-117">*Enum_declaration*可以選擇性地包含一連串的列舉修飾詞：</span><span class="sxs-lookup"><span data-stu-id="92289-117">An *enum_declaration* may optionally include a sequence of enum modifiers:</span></span>

```antlr
enum_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;
```

<span data-ttu-id="92289-118">它是相同的修飾詞在 enum 宣告中出現多次的編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="92289-118">It is a compile-time error for the same modifier to appear multiple times in an enum declaration.</span></span>

<span data-ttu-id="92289-119">列舉宣告的修飾詞具有相同的意義的類別宣告 ([類別修飾詞](classes.md#class-modifiers))。</span><span class="sxs-lookup"><span data-stu-id="92289-119">The modifiers of an enum declaration have the same meaning as those of a class declaration ([Class modifiers](classes.md#class-modifiers)).</span></span> <span data-ttu-id="92289-120">不過請注意，`abstract`和`sealed`enum 宣告中不允許修飾詞。</span><span class="sxs-lookup"><span data-stu-id="92289-120">Note, however, that the `abstract` and `sealed` modifiers are not permitted in an enum declaration.</span></span> <span data-ttu-id="92289-121">列舉不可為抽象，且不允許衍生。</span><span class="sxs-lookup"><span data-stu-id="92289-121">Enums cannot be abstract and do not permit derivation.</span></span>

## <a name="enum-members"></a><span data-ttu-id="92289-122">列舉成員</span><span class="sxs-lookup"><span data-stu-id="92289-122">Enum members</span></span>

<span data-ttu-id="92289-123">列舉型別宣告的主體會定義零或多個列舉成員，也就是列舉類型的具名的常數。</span><span class="sxs-lookup"><span data-stu-id="92289-123">The body of an enum type declaration defines zero or more enum members, which are the named constants of the enum type.</span></span> <span data-ttu-id="92289-124">沒有兩個列舉的成員可以有相同的名稱。</span><span class="sxs-lookup"><span data-stu-id="92289-124">No two enum members can have the same name.</span></span>

```antlr
enum_member_declarations
    : enum_member_declaration (',' enum_member_declaration)*
    ;

enum_member_declaration
    : attributes? identifier ('=' constant_expression)?
    ;
```

<span data-ttu-id="92289-125">每個列舉的成員具有相關聯的常數值。</span><span class="sxs-lookup"><span data-stu-id="92289-125">Each enum member has an associated constant value.</span></span> <span data-ttu-id="92289-126">此值的型別是包含列舉的基礎類型。</span><span class="sxs-lookup"><span data-stu-id="92289-126">The type of this value is the underlying type for the containing enum.</span></span> <span data-ttu-id="92289-127">每個列舉成員的常值必須是列舉的基礎類型的範圍中。</span><span class="sxs-lookup"><span data-stu-id="92289-127">The constant value for each enum member must be in the range of the underlying type for the enum.</span></span> <span data-ttu-id="92289-128">此範例</span><span class="sxs-lookup"><span data-stu-id="92289-128">The example</span></span>

```csharp
enum Color: uint
{
    Red = -1,
    Green = -2,
    Blue = -3
}
```

<span data-ttu-id="92289-129">因為會導致編譯時期錯誤的常數值`-1`， `-2`，並`-3`不在基礎整數類資料類型的範圍中`uint`。</span><span class="sxs-lookup"><span data-stu-id="92289-129">results in a compile-time error because the constant values `-1`, `-2`, and `-3` are not in the range of the underlying integral type `uint`.</span></span>

<span data-ttu-id="92289-130">多個列舉的成員可能會共用同一個相關聯的值。</span><span class="sxs-lookup"><span data-stu-id="92289-130">Multiple enum members may share the same associated value.</span></span> <span data-ttu-id="92289-131">此範例</span><span class="sxs-lookup"><span data-stu-id="92289-131">The example</span></span>

```csharp
enum Color 
{
    Red,
    Green,
    Blue,

    Max = Blue
}
```

<span data-ttu-id="92289-132">顯示兩個列舉成員，列舉`Blue`和`Max`-具有相同的關聯值。</span><span class="sxs-lookup"><span data-stu-id="92289-132">shows an enum in which two enum members -- `Blue` and `Max` -- have the same associated value.</span></span>

<span data-ttu-id="92289-133">隱含或明確指派的列舉成員相關聯的值。</span><span class="sxs-lookup"><span data-stu-id="92289-133">The associated value of an enum member is assigned either implicitly or explicitly.</span></span> <span data-ttu-id="92289-134">列舉成員的宣告是否*constant_expression*初始設定式、 常數運算式，會隱含地轉換為列舉的基礎類型的值是列舉成員的相關聯的值。</span><span class="sxs-lookup"><span data-stu-id="92289-134">If the declaration of the enum member has a *constant_expression* initializer, the value of that constant expression, implicitly converted to the underlying type of the enum, is the associated value of the enum member.</span></span> <span data-ttu-id="92289-135">如果列舉成員的宣告有沒有初始設定式，其相關聯的值是隱含設定，如下所示：</span><span class="sxs-lookup"><span data-stu-id="92289-135">If the declaration of the enum member has no initializer, its associated value is set implicitly, as follows:</span></span>

*  <span data-ttu-id="92289-136">如果列舉成員的列舉類型中宣告的第一個列舉成員，其相關聯的值為零。</span><span class="sxs-lookup"><span data-stu-id="92289-136">If the enum member is the first enum member declared in the enum type, its associated value is zero.</span></span>
*  <span data-ttu-id="92289-137">否則，列舉成員的相關聯的值取得來增加一個賦予一個列舉成員的相關聯的值。</span><span class="sxs-lookup"><span data-stu-id="92289-137">Otherwise, the associated value of the enum member is obtained by increasing the associated value of the textually preceding enum member by one.</span></span> <span data-ttu-id="92289-138">這個增加的值必須是可表示的基礎類型的值的範圍內，否則會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="92289-138">This increased value must be within the range of values that can be represented by the underlying type, otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="92289-139">此範例</span><span class="sxs-lookup"><span data-stu-id="92289-139">The example</span></span>

```csharp
using System;

enum Color
{
    Red,
    Green = 10,
    Blue
}

class Test
{
    static void Main() {
        Console.WriteLine(StringFromColor(Color.Red));
        Console.WriteLine(StringFromColor(Color.Green));
        Console.WriteLine(StringFromColor(Color.Blue));
    }

    static string StringFromColor(Color c) {
        switch (c) {
            case Color.Red: 
                return String.Format("Red = {0}", (int) c);

            case Color.Green:
                return String.Format("Green = {0}", (int) c);

            case Color.Blue:
                return String.Format("Blue = {0}", (int) c);

            default:
                return "Invalid color";
        }
    }
}
```

<span data-ttu-id="92289-140">印出的列舉成員名稱和其相關聯的值。</span><span class="sxs-lookup"><span data-stu-id="92289-140">prints out the enum member names and their associated values.</span></span> <span data-ttu-id="92289-141">輸出為：</span><span class="sxs-lookup"><span data-stu-id="92289-141">The output is:</span></span>

```
Red = 0
Green = 10
Blue = 11
```

<span data-ttu-id="92289-142">原因如下：</span><span class="sxs-lookup"><span data-stu-id="92289-142">for the following reasons:</span></span>

*  <span data-ttu-id="92289-143">列舉成員`Red`會自動指派值為零 （因為它沒有初始設定式，而且是第一個列舉成員）;</span><span class="sxs-lookup"><span data-stu-id="92289-143">the enum member `Red` is automatically assigned the value zero (since it has no initializer and is the first enum member);</span></span>
*  <span data-ttu-id="92289-144">列舉成員`Green`明確的值會指定`10`;</span><span class="sxs-lookup"><span data-stu-id="92289-144">the enum member `Green` is explicitly given the value `10`;</span></span>
*  <span data-ttu-id="92289-145">和列舉成員`Blue`會自動指派值的值大於賦予在它之前的成員。</span><span class="sxs-lookup"><span data-stu-id="92289-145">and the enum member `Blue` is automatically assigned the value one greater than the member that textually precedes it.</span></span>

<span data-ttu-id="92289-146">列舉成員的相關聯的值可能不是，直接或間接使用它自己相關聯的列舉成員的值。</span><span class="sxs-lookup"><span data-stu-id="92289-146">The associated value of an enum member may not, directly or indirectly, use the value of its own associated enum member.</span></span> <span data-ttu-id="92289-147">非此循環限制，列舉成員初始設定式可以自由參考其他列舉成員初始設定式，無論其文字的位置。</span><span class="sxs-lookup"><span data-stu-id="92289-147">Other than this circularity restriction, enum member initializers may freely refer to other enum member initializers, regardless of their textual position.</span></span> <span data-ttu-id="92289-148">列舉成員初始設定式內其他列舉成員的值會一律視為擁有其基礎類型，以便參考其他列舉成員時，不需要轉換 （cast）。</span><span class="sxs-lookup"><span data-stu-id="92289-148">Within an enum member initializer, values of other enum members are always treated as having the type of their underlying type, so that casts are not necessary when referring to other enum members.</span></span>

<span data-ttu-id="92289-149">此範例</span><span class="sxs-lookup"><span data-stu-id="92289-149">The example</span></span>

```csharp
enum Circular
{
    A = B,
    B
}
```

<span data-ttu-id="92289-150">因為會導致編譯時期錯誤的宣告`A`和`B`會循環。</span><span class="sxs-lookup"><span data-stu-id="92289-150">results in a compile-time error because the declarations of `A` and `B` are circular.</span></span> <span data-ttu-id="92289-151">`A` 取決於`B`明確地與`B`取決於`A`隱含。</span><span class="sxs-lookup"><span data-stu-id="92289-151">`A` depends on `B` explicitly, and `B` depends on `A` implicitly.</span></span>

<span data-ttu-id="92289-152">名為列舉成員，並範圍內之類別完全一樣的方式。</span><span class="sxs-lookup"><span data-stu-id="92289-152">Enum members are named and scoped in a manner exactly analogous to fields within classes.</span></span> <span data-ttu-id="92289-153">列舉成員的範圍是其包含的列舉類型的主體。</span><span class="sxs-lookup"><span data-stu-id="92289-153">The scope of an enum member is the body of its containing enum type.</span></span> <span data-ttu-id="92289-154">在該範圍中，列舉成員可以被指其簡單的名稱。</span><span class="sxs-lookup"><span data-stu-id="92289-154">Within that scope, enum members can be referred to by their simple name.</span></span> <span data-ttu-id="92289-155">從所有其他的程式碼，列舉成員的名稱必須限定其列舉型別的名稱。</span><span class="sxs-lookup"><span data-stu-id="92289-155">From all other code, the name of an enum member must be qualified with the name of its enum type.</span></span> <span data-ttu-id="92289-156">列舉成員並沒有任何已宣告存取範圍 (列舉成員是可存取其包含的列舉型別是否可存取）。</span><span class="sxs-lookup"><span data-stu-id="92289-156">Enum members do not have any declared accessibility -- an enum member is accessible if its containing enum type is accessible.</span></span>

## <a name="the-systemenum-type"></a><span data-ttu-id="92289-157">System.Enum 類型</span><span class="sxs-lookup"><span data-stu-id="92289-157">The System.Enum type</span></span>

<span data-ttu-id="92289-158">型別`System.Enum`（這是相異且不同於列舉型別的基礎型別） 的所有列舉類型的抽象基底都類別和成員繼承自`System.Enum`位於任何列舉型別。</span><span class="sxs-lookup"><span data-stu-id="92289-158">The type `System.Enum` is the abstract base class of all enum types (this is distinct and different from the underlying type of the enum type), and the members inherited from `System.Enum` are available in any enum type.</span></span> <span data-ttu-id="92289-159">Boxing 轉換 ([Boxing 轉換](types.md#boxing-conversions)) 存在從任何列舉型別`System.Enum`，和 unboxing 轉換 ([Unboxing 轉換](types.md#unboxing-conversions)) 從`System.Enum`任何列舉類型。</span><span class="sxs-lookup"><span data-stu-id="92289-159">A boxing conversion ([Boxing conversions](types.md#boxing-conversions)) exists from any enum type to `System.Enum`, and an unboxing conversion ([Unboxing conversions](types.md#unboxing-conversions)) exists from `System.Enum` to any enum type.</span></span>

<span data-ttu-id="92289-160">請注意，`System.Enum`本身並不會*enum_type*。</span><span class="sxs-lookup"><span data-stu-id="92289-160">Note that `System.Enum` is not itself an *enum_type*.</span></span> <span data-ttu-id="92289-161">而是*class_type*所有*enum_type*s 衍生。</span><span class="sxs-lookup"><span data-stu-id="92289-161">Rather, it is a *class_type* from which all *enum_type*s are derived.</span></span> <span data-ttu-id="92289-162">型別`System.Enum`繼承自型別`System.ValueType`([System.ValueType 型別](types.md#the-systemvaluetype-type))，它會接著繼承自型別`object`。</span><span class="sxs-lookup"><span data-stu-id="92289-162">The type `System.Enum` inherits from the type `System.ValueType` ([The System.ValueType type](types.md#the-systemvaluetype-type)), which, in turn, inherits from type `object`.</span></span> <span data-ttu-id="92289-163">在執行階段，類型的值`System.Enum`可以是`null`或任何列舉類型的 boxed 值的參考。</span><span class="sxs-lookup"><span data-stu-id="92289-163">At run-time, a value of type `System.Enum` can be `null` or a reference to a boxed value of any enum type.</span></span>

## <a name="enum-values-and-operations"></a><span data-ttu-id="92289-164">列舉值和作業</span><span class="sxs-lookup"><span data-stu-id="92289-164">Enum values and operations</span></span>

<span data-ttu-id="92289-165">每個列舉類型定義不同的類型;明確的列舉型別轉換 ([明確的列舉型別轉換](conversions.md#explicit-enumeration-conversions))，才可將列舉型別之間的整數類資料類型，或兩個列舉類型之間轉換。</span><span class="sxs-lookup"><span data-stu-id="92289-165">Each enum type defines a distinct type; an explicit enumeration conversion ([Explicit enumeration conversions](conversions.md#explicit-enumeration-conversions)) is required to convert between an enum type and an integral type, or between two enum types.</span></span> <span data-ttu-id="92289-166">列舉成員不會受限於一組可對列舉類型的值。</span><span class="sxs-lookup"><span data-stu-id="92289-166">The set of values that an enum type can take on is not limited by its enum members.</span></span> <span data-ttu-id="92289-167">特別的是，列舉的基礎類型的任何值可以轉換為列舉型別，是該列舉型別的相異有效值。</span><span class="sxs-lookup"><span data-stu-id="92289-167">In particular, any value of the underlying type of an enum can be cast to the enum type, and is a distinct valid value of that enum type.</span></span>

<span data-ttu-id="92289-168">列舉成員具有其包含的列舉型別的型別 (除了其他列舉成員初始設定式內： 請參閱[列舉成員](enums.md#enum-members))。</span><span class="sxs-lookup"><span data-stu-id="92289-168">Enum members have the type of their containing enum type (except within other enum member initializers: see [Enum members](enums.md#enum-members)).</span></span> <span data-ttu-id="92289-169">列舉類型中宣告的列舉成員值`E`相關聯的值`v`是`(E)v`。</span><span class="sxs-lookup"><span data-stu-id="92289-169">The value of an enum member declared in enum type `E` with associated value `v` is `(E)v`.</span></span>

<span data-ttu-id="92289-170">下列運算子可用於列舉類型的值： `==`， `!=`， `<`， `>`， `<=`， `>=` ([列舉型別比較運算子](expressions.md#enumeration-comparison-operators))，的二進位檔`+`([加法運算子](expressions.md#addition-operator))、 二進位`-`([減法運算子](expressions.md#subtraction-operator))， `^`， `&`， `|` ([列舉邏輯運算子](expressions.md#enumeration-logical-operators))， `~` ([位元補充運算子](expressions.md#bitwise-complement-operator))，`++`並`--`([後置遞增和遞減運算子](expressions.md#postfix-increment-and-decrement-operators)並[前置遞增和遞減運算子](expressions.md#prefix-increment-and-decrement-operators))。</span><span class="sxs-lookup"><span data-stu-id="92289-170">The following operators can be used on values of enum types: `==`, `!=`, `<`, `>`, `<=`, `>=` ([Enumeration comparison operators](expressions.md#enumeration-comparison-operators)), binary `+` ([Addition operator](expressions.md#addition-operator)), binary `-` ([Subtraction operator](expressions.md#subtraction-operator)), `^`, `&`, `|` ([Enumeration logical operators](expressions.md#enumeration-logical-operators)), `~` ([Bitwise complement operator](expressions.md#bitwise-complement-operator)), `++` and `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="92289-171">每個列舉型別會自動將衍生自類別`System.Enum`(其中，又，衍生自`System.ValueType`和`object`)。</span><span class="sxs-lookup"><span data-stu-id="92289-171">Every enum type automatically derives from the class `System.Enum` (which, in turn, derives from `System.ValueType` and `object`).</span></span> <span data-ttu-id="92289-172">因此，這個類別的繼承的方法和屬性可用的列舉類型的值。</span><span class="sxs-lookup"><span data-stu-id="92289-172">Thus, inherited methods and properties of this class can be used on values of an enum type.</span></span>
