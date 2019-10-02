---
ms.openlocfilehash: 3b142d7dbda8a94a4cf2c973a2e380065dcbf5ee
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703971"
---
# <a name="enums"></a><span data-ttu-id="e3c14-101">列舉</span><span class="sxs-lookup"><span data-stu-id="e3c14-101">Enums</span></span>

<span data-ttu-id="e3c14-102">***列舉類型***是宣告一組已命名常數的相異實數值型別（實[值](types.md#value-types)類型）。</span><span class="sxs-lookup"><span data-stu-id="e3c14-102">An ***enum type*** is a distinct value type ([Value types](types.md#value-types)) that declares a set of named constants.</span></span>

<span data-ttu-id="e3c14-103">範例</span><span class="sxs-lookup"><span data-stu-id="e3c14-103">The example</span></span>

```csharp
enum Color
{
    Red,
    Green,
    Blue
}
```

<span data-ttu-id="e3c14-104">宣告名為 `Color` 的列舉類型，成員 `Red`、`Green` 和 `Blue`。</span><span class="sxs-lookup"><span data-stu-id="e3c14-104">declares an enum type named `Color` with members `Red`, `Green`, and `Blue`.</span></span>

## <a name="enum-declarations"></a><span data-ttu-id="e3c14-105">列舉宣告</span><span class="sxs-lookup"><span data-stu-id="e3c14-105">Enum declarations</span></span>

<span data-ttu-id="e3c14-106">Enum 宣告會宣告新的列舉類型。</span><span class="sxs-lookup"><span data-stu-id="e3c14-106">An enum declaration declares a new enum type.</span></span> <span data-ttu-id="e3c14-107">Enum 宣告的開頭為關鍵字 `enum`，並定義列舉的名稱、存取範圍、基礎類型和成員。</span><span class="sxs-lookup"><span data-stu-id="e3c14-107">An enum declaration begins with the keyword `enum`, and defines the name, accessibility, underlying type, and members of the enum.</span></span>

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

<span data-ttu-id="e3c14-108">每個列舉型別都有對應的整數型別，稱為列舉型別的***基礎型***別。</span><span class="sxs-lookup"><span data-stu-id="e3c14-108">Each enum type has a corresponding integral type called the ***underlying type*** of the enum type.</span></span> <span data-ttu-id="e3c14-109">這個基礎類型必須能夠代表列舉中所定義的所有列舉值。</span><span class="sxs-lookup"><span data-stu-id="e3c14-109">This underlying type must be able to represent all the enumerator values defined in the enumeration.</span></span> <span data-ttu-id="e3c14-110">Enum 宣告可以明確宣告 `byte`、`sbyte`、`short`、`ushort`、`int`、`uint`、`long` 或 `ulong` 的基礎類型。</span><span class="sxs-lookup"><span data-stu-id="e3c14-110">An enum declaration may explicitly declare an underlying type of `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` or `ulong`.</span></span> <span data-ttu-id="e3c14-111">請注意，`char` 不能當做基礎類型使用。</span><span class="sxs-lookup"><span data-stu-id="e3c14-111">Note that `char` cannot be used as an underlying type.</span></span> <span data-ttu-id="e3c14-112">未明確宣告基礎類型的列舉宣告具有 `int` 的基礎類型。</span><span class="sxs-lookup"><span data-stu-id="e3c14-112">An enum declaration that does not explicitly declare an underlying type has an underlying type of `int`.</span></span>

<span data-ttu-id="e3c14-113">範例</span><span class="sxs-lookup"><span data-stu-id="e3c14-113">The example</span></span>

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

<span data-ttu-id="e3c14-114">宣告具有 `long` 之基礎類型的列舉。</span><span class="sxs-lookup"><span data-stu-id="e3c14-114">declares an enum with an underlying type of `long`.</span></span> <span data-ttu-id="e3c14-115">開發人員可能會選擇使用基礎類型的 `long` （如範例所示），以啟用範圍為 `long` 但不在 `int` 範圍內的值，或保留此選項供未來使用。</span><span class="sxs-lookup"><span data-stu-id="e3c14-115">A developer might choose to use an underlying type of `long`, as in the example, to enable the use of values that are in the range of `long` but not in the range of `int`, or to preserve this option for the future.</span></span>

## <a name="enum-modifiers"></a><span data-ttu-id="e3c14-116">Enum 修飾詞</span><span class="sxs-lookup"><span data-stu-id="e3c14-116">Enum modifiers</span></span>

<span data-ttu-id="e3c14-117">*Enum_declaration*可以選擇性地包含一連串的 enum 修飾詞：</span><span class="sxs-lookup"><span data-stu-id="e3c14-117">An *enum_declaration* may optionally include a sequence of enum modifiers:</span></span>

```antlr
enum_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;
```

<span data-ttu-id="e3c14-118">在列舉宣告中多次出現相同的修飾詞時，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="e3c14-118">It is a compile-time error for the same modifier to appear multiple times in an enum declaration.</span></span>

<span data-ttu-id="e3c14-119">Enum 宣告的修飾詞與類別宣告（[類別](classes.md#class-modifiers)修飾詞）的修飾詞具有相同的意義。</span><span class="sxs-lookup"><span data-stu-id="e3c14-119">The modifiers of an enum declaration have the same meaning as those of a class declaration ([Class modifiers](classes.md#class-modifiers)).</span></span> <span data-ttu-id="e3c14-120">不過，請注意，enum 宣告中不允許 `abstract` 和 @no__t 1 修飾詞。</span><span class="sxs-lookup"><span data-stu-id="e3c14-120">Note, however, that the `abstract` and `sealed` modifiers are not permitted in an enum declaration.</span></span> <span data-ttu-id="e3c14-121">列舉不能是抽象的，而且不允許衍生。</span><span class="sxs-lookup"><span data-stu-id="e3c14-121">Enums cannot be abstract and do not permit derivation.</span></span>

## <a name="enum-members"></a><span data-ttu-id="e3c14-122">列舉成員</span><span class="sxs-lookup"><span data-stu-id="e3c14-122">Enum members</span></span>

<span data-ttu-id="e3c14-123">列舉類型宣告的主體會定義零或多個列舉成員，這是列舉類型的已命名常數。</span><span class="sxs-lookup"><span data-stu-id="e3c14-123">The body of an enum type declaration defines zero or more enum members, which are the named constants of the enum type.</span></span> <span data-ttu-id="e3c14-124">沒有兩個列舉成員可以具有相同的名稱。</span><span class="sxs-lookup"><span data-stu-id="e3c14-124">No two enum members can have the same name.</span></span>

```antlr
enum_member_declarations
    : enum_member_declaration (',' enum_member_declaration)*
    ;

enum_member_declaration
    : attributes? identifier ('=' constant_expression)?
    ;
```

<span data-ttu-id="e3c14-125">每個列舉成員都有相關聯的常數值。</span><span class="sxs-lookup"><span data-stu-id="e3c14-125">Each enum member has an associated constant value.</span></span> <span data-ttu-id="e3c14-126">此值的類型是包含列舉的基礎類型。</span><span class="sxs-lookup"><span data-stu-id="e3c14-126">The type of this value is the underlying type for the containing enum.</span></span> <span data-ttu-id="e3c14-127">每個列舉成員的常數值必須在列舉之基礎類型的範圍內。</span><span class="sxs-lookup"><span data-stu-id="e3c14-127">The constant value for each enum member must be in the range of the underlying type for the enum.</span></span> <span data-ttu-id="e3c14-128">範例</span><span class="sxs-lookup"><span data-stu-id="e3c14-128">The example</span></span>

```csharp
enum Color: uint
{
    Red = -1,
    Green = -2,
    Blue = -3
}
```

<span data-ttu-id="e3c14-129">產生編譯時期錯誤，因為 `-1`、`-2` 和 `-3` 的常數值不在基礎整數類型 `uint` 的範圍內。</span><span class="sxs-lookup"><span data-stu-id="e3c14-129">results in a compile-time error because the constant values `-1`, `-2`, and `-3` are not in the range of the underlying integral type `uint`.</span></span>

<span data-ttu-id="e3c14-130">多個列舉成員可能共用相同的關聯值。</span><span class="sxs-lookup"><span data-stu-id="e3c14-130">Multiple enum members may share the same associated value.</span></span> <span data-ttu-id="e3c14-131">範例</span><span class="sxs-lookup"><span data-stu-id="e3c14-131">The example</span></span>

```csharp
enum Color 
{
    Red,
    Green,
    Blue,

    Max = Blue
}
```

<span data-ttu-id="e3c14-132">顯示列舉，其中兩個列舉成員--`Blue`，而 `Max`--具有相同的關聯值。</span><span class="sxs-lookup"><span data-stu-id="e3c14-132">shows an enum in which two enum members -- `Blue` and `Max` -- have the same associated value.</span></span>

<span data-ttu-id="e3c14-133">列舉成員的相關值會隱含或明確地指派。</span><span class="sxs-lookup"><span data-stu-id="e3c14-133">The associated value of an enum member is assigned either implicitly or explicitly.</span></span> <span data-ttu-id="e3c14-134">如果列舉成員的宣告具有*constant_expression*初始化運算式，則該常數運算式的值（隱含地轉換為列舉的基礎類型）是列舉成員的相關值。</span><span class="sxs-lookup"><span data-stu-id="e3c14-134">If the declaration of the enum member has a *constant_expression* initializer, the value of that constant expression, implicitly converted to the underlying type of the enum, is the associated value of the enum member.</span></span> <span data-ttu-id="e3c14-135">如果列舉成員的宣告沒有初始化運算式，則會隱含設定其相關聯的值，如下所示：</span><span class="sxs-lookup"><span data-stu-id="e3c14-135">If the declaration of the enum member has no initializer, its associated value is set implicitly, as follows:</span></span>

*  <span data-ttu-id="e3c14-136">如果列舉成員是列舉類型中所宣告的第一個列舉成員，其相關聯的值為零。</span><span class="sxs-lookup"><span data-stu-id="e3c14-136">If the enum member is the first enum member declared in the enum type, its associated value is zero.</span></span>
*  <span data-ttu-id="e3c14-137">否則，會藉由將每個以上而下的列舉成員相關聯的值遞增一，來取得列舉成員的相關聯值。</span><span class="sxs-lookup"><span data-stu-id="e3c14-137">Otherwise, the associated value of the enum member is obtained by increasing the associated value of the textually preceding enum member by one.</span></span> <span data-ttu-id="e3c14-138">這個增加的值必須在可由基礎類型表示的值範圍內，否則會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="e3c14-138">This increased value must be within the range of values that can be represented by the underlying type, otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="e3c14-139">範例</span><span class="sxs-lookup"><span data-stu-id="e3c14-139">The example</span></span>

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

<span data-ttu-id="e3c14-140">印出列舉成員名稱及其相關聯的值。</span><span class="sxs-lookup"><span data-stu-id="e3c14-140">prints out the enum member names and their associated values.</span></span> <span data-ttu-id="e3c14-141">輸出為：</span><span class="sxs-lookup"><span data-stu-id="e3c14-141">The output is:</span></span>

```console
Red = 0
Green = 10
Blue = 11
```

<span data-ttu-id="e3c14-142">原因如下：</span><span class="sxs-lookup"><span data-stu-id="e3c14-142">for the following reasons:</span></span>

*  <span data-ttu-id="e3c14-143">列舉成員 `Red` 會自動指派值零（因為它沒有初始化運算式，而且是第一個列舉成員）;</span><span class="sxs-lookup"><span data-stu-id="e3c14-143">the enum member `Red` is automatically assigned the value zero (since it has no initializer and is the first enum member);</span></span>
*  <span data-ttu-id="e3c14-144">列舉成員 `Green` 已明確指定值 `10`;</span><span class="sxs-lookup"><span data-stu-id="e3c14-144">the enum member `Green` is explicitly given the value `10`;</span></span>
*  <span data-ttu-id="e3c14-145">而且列舉成員 `Blue` 會自動指派一個大於以全形方式出現在其前面的成員的值。</span><span class="sxs-lookup"><span data-stu-id="e3c14-145">and the enum member `Blue` is automatically assigned the value one greater than the member that textually precedes it.</span></span>

<span data-ttu-id="e3c14-146">列舉成員的相關值可能不是直接或間接使用其本身相關聯列舉成員的值。</span><span class="sxs-lookup"><span data-stu-id="e3c14-146">The associated value of an enum member may not, directly or indirectly, use the value of its own associated enum member.</span></span> <span data-ttu-id="e3c14-147">除了此迴圈限制以外，列舉成員初始化運算式可以自由地參考其他列舉成員初始化運算式，不論其文字位置為何。</span><span class="sxs-lookup"><span data-stu-id="e3c14-147">Other than this circularity restriction, enum member initializers may freely refer to other enum member initializers, regardless of their textual position.</span></span> <span data-ttu-id="e3c14-148">在列舉成員初始化運算式中，其他列舉成員的值一律會被視為具有其基礎類型的類型，因此在參考其他列舉成員時，並不需要轉換。</span><span class="sxs-lookup"><span data-stu-id="e3c14-148">Within an enum member initializer, values of other enum members are always treated as having the type of their underlying type, so that casts are not necessary when referring to other enum members.</span></span>

<span data-ttu-id="e3c14-149">範例</span><span class="sxs-lookup"><span data-stu-id="e3c14-149">The example</span></span>

```csharp
enum Circular
{
    A = B,
    B
}
```

<span data-ttu-id="e3c14-150">會導致編譯時期錯誤，因為 `A` 和 `B` 的宣告是迴圈的。</span><span class="sxs-lookup"><span data-stu-id="e3c14-150">results in a compile-time error because the declarations of `A` and `B` are circular.</span></span> <span data-ttu-id="e3c14-151">`A` 會明確地相依于 `B`，而 `B` 則會隱含地相依于 `A`。</span><span class="sxs-lookup"><span data-stu-id="e3c14-151">`A` depends on `B` explicitly, and `B` depends on `A` implicitly.</span></span>

<span data-ttu-id="e3c14-152">列舉成員的命名方式和範圍，與類別中的欄位完全類似。</span><span class="sxs-lookup"><span data-stu-id="e3c14-152">Enum members are named and scoped in a manner exactly analogous to fields within classes.</span></span> <span data-ttu-id="e3c14-153">列舉成員的範圍是其包含列舉類型的主體。</span><span class="sxs-lookup"><span data-stu-id="e3c14-153">The scope of an enum member is the body of its containing enum type.</span></span> <span data-ttu-id="e3c14-154">在該範圍內，列舉成員可以透過其簡單名稱來參考。</span><span class="sxs-lookup"><span data-stu-id="e3c14-154">Within that scope, enum members can be referred to by their simple name.</span></span> <span data-ttu-id="e3c14-155">在所有其他程式碼中，列舉成員的名稱必須以其列舉類型的名稱來限定。</span><span class="sxs-lookup"><span data-stu-id="e3c14-155">From all other code, the name of an enum member must be qualified with the name of its enum type.</span></span> <span data-ttu-id="e3c14-156">列舉成員沒有任何宣告的存取範圍--如果可存取其包含列舉類型，列舉成員就可以存取。</span><span class="sxs-lookup"><span data-stu-id="e3c14-156">Enum members do not have any declared accessibility -- an enum member is accessible if its containing enum type is accessible.</span></span>

## <a name="the-systemenum-type"></a><span data-ttu-id="e3c14-157">System.string 類型</span><span class="sxs-lookup"><span data-stu-id="e3c14-157">The System.Enum type</span></span>

<span data-ttu-id="e3c14-158">類型 `System.Enum` 是所有列舉類型的抽象基類（這是相異的，與列舉類型的基礎類型不同），而繼承自 `System.Enum` 的成員可以在任何列舉類型中使用。</span><span class="sxs-lookup"><span data-stu-id="e3c14-158">The type `System.Enum` is the abstract base class of all enum types (this is distinct and different from the underlying type of the enum type), and the members inherited from `System.Enum` are available in any enum type.</span></span> <span data-ttu-id="e3c14-159">從任何列舉型別到 `System.Enum` 的「裝箱」轉換（[裝箱](types.md#boxing-conversions)轉換）都存在，而且從 `System.Enum` 到任何列舉型別都有「取消裝箱」轉換（「[取消裝箱](types.md#unboxing-conversions)」轉換）。</span><span class="sxs-lookup"><span data-stu-id="e3c14-159">A boxing conversion ([Boxing conversions](types.md#boxing-conversions)) exists from any enum type to `System.Enum`, and an unboxing conversion ([Unboxing conversions](types.md#unboxing-conversions)) exists from `System.Enum` to any enum type.</span></span>

<span data-ttu-id="e3c14-160">請注意，`System.Enum` 本身並不是*enum_type*。</span><span class="sxs-lookup"><span data-stu-id="e3c14-160">Note that `System.Enum` is not itself an *enum_type*.</span></span> <span data-ttu-id="e3c14-161">相反地，它是所有*enum_type*的衍生來源的*class_type* 。</span><span class="sxs-lookup"><span data-stu-id="e3c14-161">Rather, it is a *class_type* from which all *enum_type*s are derived.</span></span> <span data-ttu-id="e3c14-162">@No__t-0 的型別繼承自型別 `System.ValueType` （system.string[型](types.md#the-systemvaluetype-type)別），而後者又繼承自型別 `object`。</span><span class="sxs-lookup"><span data-stu-id="e3c14-162">The type `System.Enum` inherits from the type `System.ValueType` ([The System.ValueType type](types.md#the-systemvaluetype-type)), which, in turn, inherits from type `object`.</span></span> <span data-ttu-id="e3c14-163">在執行時間，`System.Enum` 類型的值可以 `null` 或任何列舉類型之已裝箱值的參考。</span><span class="sxs-lookup"><span data-stu-id="e3c14-163">At run-time, a value of type `System.Enum` can be `null` or a reference to a boxed value of any enum type.</span></span>

## <a name="enum-values-and-operations"></a><span data-ttu-id="e3c14-164">列舉值和作業</span><span class="sxs-lookup"><span data-stu-id="e3c14-164">Enum values and operations</span></span>

<span data-ttu-id="e3c14-165">每個列舉類型都會定義不同的類型。必須要有明確的列舉轉換（[明確列舉](conversions.md#explicit-enumeration-conversions)轉換），才能在列舉類型和整數類型之間轉換，或是在兩個列舉類型之間進行轉換。</span><span class="sxs-lookup"><span data-stu-id="e3c14-165">Each enum type defines a distinct type; an explicit enumeration conversion ([Explicit enumeration conversions](conversions.md#explicit-enumeration-conversions)) is required to convert between an enum type and an integral type, or between two enum types.</span></span> <span data-ttu-id="e3c14-166">列舉型別可接受的值集合不受其列舉成員的限制。</span><span class="sxs-lookup"><span data-stu-id="e3c14-166">The set of values that an enum type can take on is not limited by its enum members.</span></span> <span data-ttu-id="e3c14-167">特別是，列舉之基礎類型的任何值都可以轉換成列舉類型，而且是該列舉類型的相異有效值。</span><span class="sxs-lookup"><span data-stu-id="e3c14-167">In particular, any value of the underlying type of an enum can be cast to the enum type, and is a distinct valid value of that enum type.</span></span>

<span data-ttu-id="e3c14-168">Enum 成員具有其包含列舉類型的類型（但在其他列舉成員初始化運算式內除外）：請參閱[列舉成員](enums.md#enum-members)）。</span><span class="sxs-lookup"><span data-stu-id="e3c14-168">Enum members have the type of their containing enum type (except within other enum member initializers: see [Enum members](enums.md#enum-members)).</span></span> <span data-ttu-id="e3c14-169">列舉類型中宣告的列舉成員值 `E`，`v` 的相關聯值為 `(E)v`。</span><span class="sxs-lookup"><span data-stu-id="e3c14-169">The value of an enum member declared in enum type `E` with associated value `v` is `(E)v`.</span></span>

<span data-ttu-id="e3c14-170">下列運算子可用於列舉類型的值： `==`、`!=`、`<`、`>`、`<=`、`>=` @ no__t-6 （[列舉比較運算子](expressions.md#enumeration-comparison-operators)）、binary `+` @ no__t-9 （[加法運算子](expressions.md#addition-operator)）、binary 1 @ no__t-12 （[減法運算子](expressions.md#subtraction-operator)）、4、5、6 @ no__t-17 （[列舉邏輯運算子](expressions.md#enumeration-logical-operators)）、9 @ No__t-20 （[位補數運算子](expressions.md#bitwise-complement-operator)）、2 和 3 @ no__t-24 （後置[增量和遞減運算子](expressions.md#postfix-increment-and-decrement-operators)和[前置遞增和遞減運算子](expressions.md#prefix-increment-and-decrement-operators)）。</span><span class="sxs-lookup"><span data-stu-id="e3c14-170">The following operators can be used on values of enum types: `==`, `!=`, `<`, `>`, `<=`, `>=` ([Enumeration comparison operators](expressions.md#enumeration-comparison-operators)), binary `+` ([Addition operator](expressions.md#addition-operator)), binary `-` ([Subtraction operator](expressions.md#subtraction-operator)), `^`, `&`, `|` ([Enumeration logical operators](expressions.md#enumeration-logical-operators)), `~` ([Bitwise complement operator](expressions.md#bitwise-complement-operator)), `++` and `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="e3c14-171">每個列舉類型都會自動衍生自類別 `System.Enum` （而這又衍生自 `System.ValueType` 和 `object`）。</span><span class="sxs-lookup"><span data-stu-id="e3c14-171">Every enum type automatically derives from the class `System.Enum` (which, in turn, derives from `System.ValueType` and `object`).</span></span> <span data-ttu-id="e3c14-172">因此，可以在列舉類型的值上使用這個類別的繼承方法和屬性。</span><span class="sxs-lookup"><span data-stu-id="e3c14-172">Thus, inherited methods and properties of this class can be used on values of an enum type.</span></span>
