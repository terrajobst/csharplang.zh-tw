---
ms.openlocfilehash: dbea611280a644adc25247b9887986e129c59b68
ms.sourcegitcommit: a5e393b018b04dfa55aae0000290ca087b508495
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/14/2019
ms.locfileid: "72310367"
---
# <a name="unsafe-code"></a><span data-ttu-id="21ce4-101">Unsafe 程式碼</span><span class="sxs-lookup"><span data-stu-id="21ce4-101">Unsafe code</span></span>

<span data-ttu-id="21ce4-102">如先前C#章節中所定義的核心語言，與 C 和C++將指標省略為資料類型的方式不同。</span><span class="sxs-lookup"><span data-stu-id="21ce4-102">The core C# language, as defined in the preceding chapters, differs notably from C and C++ in its omission of pointers as a data type.</span></span> <span data-ttu-id="21ce4-103">相反地C# ，會提供參考以及建立由垃圾收集行程管理之物件的能力。</span><span class="sxs-lookup"><span data-stu-id="21ce4-103">Instead, C# provides references and the ability to create objects that are managed by a garbage collector.</span></span> <span data-ttu-id="21ce4-104">這種設計與其他功能結合，可C#提供比 C 或C++更安全的語言。</span><span class="sxs-lookup"><span data-stu-id="21ce4-104">This design, coupled with other features, makes C# a much safer language than C or C++.</span></span> <span data-ttu-id="21ce4-105">在核心C#語言中，只是不可能有未初始化的變數、「無關聯的」指標，或是索引陣列超出其界限的運算式。</span><span class="sxs-lookup"><span data-stu-id="21ce4-105">In the core C# language it is simply not possible to have an uninitialized variable, a "dangling" pointer, or an expression that indexes an array beyond its bounds.</span></span> <span data-ttu-id="21ce4-106">因此，災禍 C 和C++程式的整個錯誤類別會被排除。</span><span class="sxs-lookup"><span data-stu-id="21ce4-106">Whole categories of bugs that routinely plague C and C++ programs are thus eliminated.</span></span>

<span data-ttu-id="21ce4-107">實際上，每個指標類型在 C 中C++的結構，或的引用C#類型都是，不過在某些情況下，指標類型的存取會是必要的。</span><span class="sxs-lookup"><span data-stu-id="21ce4-107">While practically every pointer type construct in C or C++ has a reference type counterpart in C#, nonetheless, there are situations where access to pointer types becomes a necessity.</span></span> <span data-ttu-id="21ce4-108">例如，與基礎作業系統互動、存取記憶體對應的裝置，或執行時間緊迫的演算法，在不需要存取指標的情況下，可能不可行或不切實際。</span><span class="sxs-lookup"><span data-stu-id="21ce4-108">For example, interfacing with the underlying operating system, accessing a memory-mapped device, or implementing a time-critical algorithm may not be possible or practical without access to pointers.</span></span> <span data-ttu-id="21ce4-109">為了滿足此需求， C#提供了撰寫 unsafe 程式***代碼***的功能。</span><span class="sxs-lookup"><span data-stu-id="21ce4-109">To address this need, C# provides the ability to write ***unsafe code***.</span></span>

<span data-ttu-id="21ce4-110">在 unsafe 程式碼中，您可以宣告和操作指標、執行指標和整數類型之間的轉換、接受變數的位址等等。</span><span class="sxs-lookup"><span data-stu-id="21ce4-110">In unsafe code it is possible to declare and operate on pointers, to perform conversions between pointers and integral types, to take the address of variables, and so forth.</span></span> <span data-ttu-id="21ce4-111">就意義而言，撰寫 unsafe C#程式碼很像是在程式中撰寫 C 程式碼。</span><span class="sxs-lookup"><span data-stu-id="21ce4-111">In a sense, writing unsafe code is much like writing C code within a C# program.</span></span>

<span data-ttu-id="21ce4-112">不安全的程式碼實際上是從開發人員和使用者的角度來看的「安全」功能。</span><span class="sxs-lookup"><span data-stu-id="21ce4-112">Unsafe code is in fact a "safe" feature from the perspective of both developers and users.</span></span> <span data-ttu-id="21ce4-113">不安全的程式碼必須以修飾詞清楚標示 `unsafe`，讓開發人員不會意外地使用不安全的功能，而且執行引擎可以確保不安全的程式碼無法在未受信任的環境中執行。</span><span class="sxs-lookup"><span data-stu-id="21ce4-113">Unsafe code must be clearly marked with the modifier `unsafe`, so developers can't possibly use unsafe features accidentally, and the execution engine works to ensure that unsafe code cannot be executed in an untrusted environment.</span></span>

## <a name="unsafe-contexts"></a><span data-ttu-id="21ce4-114">Unsafe 內容</span><span class="sxs-lookup"><span data-stu-id="21ce4-114">Unsafe contexts</span></span>

<span data-ttu-id="21ce4-115">不安全的C#功能僅適用于 unsafe 內容。</span><span class="sxs-lookup"><span data-stu-id="21ce4-115">The unsafe features of C# are available only in unsafe contexts.</span></span> <span data-ttu-id="21ce4-116">不安全的內容是藉由在類型或成員的宣告中包含 `unsafe` 修飾詞，或藉由採用*unsafe_statement*而引進：</span><span class="sxs-lookup"><span data-stu-id="21ce4-116">An unsafe context is introduced by including an `unsafe` modifier in the declaration of a type or member, or by employing an *unsafe_statement*:</span></span>

*  <span data-ttu-id="21ce4-117">類別、結構、介面或委派的宣告可能包含 `unsafe` 修飾詞，在這種情況下，該類型宣告的整個文字範圍（包括類別、結構或介面的主體）會被視為不安全的內容。</span><span class="sxs-lookup"><span data-stu-id="21ce4-117">A declaration of a class, struct, interface, or delegate may include an `unsafe` modifier, in which case the entire textual extent of that type declaration (including the body of the class, struct, or interface) is considered an unsafe context.</span></span>
*  <span data-ttu-id="21ce4-118">欄位、方法、屬性、事件、索引子、運算子、實例的程式化、析構函數或靜態的函式的宣告可能包含 @no__t 0 修飾詞，在這種情況下，該成員宣告的整個文字範圍會被視為不安全的內容。</span><span class="sxs-lookup"><span data-stu-id="21ce4-118">A declaration of a field, method, property, event, indexer, operator, instance constructor, destructor, or static constructor may include an `unsafe` modifier, in which case the entire textual extent of that member declaration is considered an unsafe context.</span></span>
*  <span data-ttu-id="21ce4-119">*Unsafe_statement*可讓您在*區塊*內使用不安全的內容。</span><span class="sxs-lookup"><span data-stu-id="21ce4-119">An *unsafe_statement* enables the use of an unsafe context within a *block*.</span></span> <span data-ttu-id="21ce4-120">相關聯*區塊*的整個文字範圍會被視為不安全的內容。</span><span class="sxs-lookup"><span data-stu-id="21ce4-120">The entire textual extent of the associated *block* is considered an unsafe context.</span></span>

<span data-ttu-id="21ce4-121">相關聯的文法生產如下所示。</span><span class="sxs-lookup"><span data-stu-id="21ce4-121">The associated grammar productions are shown below.</span></span>

```antlr
class_modifier_unsafe
    : 'unsafe'
    ;

struct_modifier_unsafe
    : 'unsafe'
    ;

interface_modifier_unsafe
    : 'unsafe'
    ;

delegate_modifier_unsafe
    : 'unsafe'
    ;

field_modifier_unsafe
    : 'unsafe'
    ;

method_modifier_unsafe
    : 'unsafe'
    ;

property_modifier_unsafe
    : 'unsafe'
    ;

event_modifier_unsafe
    : 'unsafe'
    ;

indexer_modifier_unsafe
    : 'unsafe'
    ;

operator_modifier_unsafe
    : 'unsafe'
    ;

constructor_modifier_unsafe
    : 'unsafe'
    ;

destructor_declaration_unsafe
    : attributes? 'extern'? 'unsafe'? '~' identifier '(' ')' destructor_body
    | attributes? 'unsafe'? 'extern'? '~' identifier '(' ')' destructor_body
    ;

static_constructor_modifiers_unsafe
    : 'extern'? 'unsafe'? 'static'
    | 'unsafe'? 'extern'? 'static'
    | 'extern'? 'static' 'unsafe'?
    | 'unsafe'? 'static' 'extern'?
    | 'static' 'extern'? 'unsafe'?
    | 'static' 'unsafe'? 'extern'?
    ;

embedded_statement_unsafe
    : unsafe_statement
    | fixed_statement
    ;

unsafe_statement
    : 'unsafe' block
    ;
```

<span data-ttu-id="21ce4-122">在範例中</span><span class="sxs-lookup"><span data-stu-id="21ce4-122">In the example</span></span>

```csharp
public unsafe struct Node
{
    public int Value;
    public Node* Left;
    public Node* Right;
}
```

<span data-ttu-id="21ce4-123">結構宣告中指定的 `unsafe` 修飾詞會導致結構宣告的整個文字範圍成為不安全的內容。</span><span class="sxs-lookup"><span data-stu-id="21ce4-123">the `unsafe` modifier specified in the struct declaration causes the entire textual extent of the struct declaration to become an unsafe context.</span></span> <span data-ttu-id="21ce4-124">因此，您可以將 `Left` 和 @no__t 1 欄位宣告為指標類型。</span><span class="sxs-lookup"><span data-stu-id="21ce4-124">Thus, it is possible to declare the `Left` and `Right` fields to be of a pointer type.</span></span> <span data-ttu-id="21ce4-125">也可以撰寫上述範例</span><span class="sxs-lookup"><span data-stu-id="21ce4-125">The example above could also be written</span></span>

```csharp
public struct Node
{
    public int Value;
    public unsafe Node* Left;
    public unsafe Node* Right;
}
```

<span data-ttu-id="21ce4-126">在這裡，欄位宣告中的 `unsafe` 修飾詞會導致這些宣告被視為不安全的內容。</span><span class="sxs-lookup"><span data-stu-id="21ce4-126">Here, the `unsafe` modifiers in the field declarations cause those declarations to be considered unsafe contexts.</span></span>

<span data-ttu-id="21ce4-127">除了建立不安全的內容，因此允許使用指標類型，`unsafe` 修飾詞對類型或成員不會有任何影響。</span><span class="sxs-lookup"><span data-stu-id="21ce4-127">Other than establishing an unsafe context, thus permitting the use of pointer types, the `unsafe` modifier has no effect on a type or a member.</span></span> <span data-ttu-id="21ce4-128">在範例中</span><span class="sxs-lookup"><span data-stu-id="21ce4-128">In the example</span></span>

```csharp
public class A
{
    public unsafe virtual void F() {
        char* p;
        ...
    }
}

public class B: A
{
    public override void F() {
        base.F();
        ...
    }
}
```

<span data-ttu-id="21ce4-129">`A` 中 `F` 方法上的 `unsafe` 修飾詞，只會使 `F` 的文字範圍成為不安全的內容，而不會使用語言的 unsafe 功能。</span><span class="sxs-lookup"><span data-stu-id="21ce4-129">the `unsafe` modifier on the `F` method in `A` simply causes the textual extent of `F` to become an unsafe context in which the unsafe features of the language can be used.</span></span> <span data-ttu-id="21ce4-130">在 `B` 的 `F` 覆寫中，不需要重新指定 `unsafe` 修飾詞，除非在 `B` 中的 `F` 方法本身需要存取不安全的功能。</span><span class="sxs-lookup"><span data-stu-id="21ce4-130">In the override of `F` in `B`, there is no need to re-specify the `unsafe` modifier -- unless, of course, the `F` method in `B` itself needs access to unsafe features.</span></span>

<span data-ttu-id="21ce4-131">當指標類型是方法簽章的一部分時，這種情況會略有不同</span><span class="sxs-lookup"><span data-stu-id="21ce4-131">The situation is slightly different when a pointer type is part of the method's signature</span></span>

```csharp
public unsafe class A
{
    public virtual void F(char* p) {...}
}

public class B: A
{
    public unsafe override void F(char* p) {...}
}
```

<span data-ttu-id="21ce4-132">在這裡，因為 `F` 的簽章包含指標類型，所以只能在不安全的內容中寫入。</span><span class="sxs-lookup"><span data-stu-id="21ce4-132">Here, because `F`'s signature includes a pointer type, it can only be written in an unsafe context.</span></span> <span data-ttu-id="21ce4-133">不過，不安全的內容可以藉由讓整個類別不安全（如 `A` 中的情況），或在方法宣告中包含 `unsafe` 修飾詞來引進，如同 `B` 中的情況。</span><span class="sxs-lookup"><span data-stu-id="21ce4-133">However, the unsafe context can be introduced by either making the entire class unsafe, as is the case in `A`, or by including an `unsafe` modifier in the method declaration, as is the case in `B`.</span></span>

## <a name="pointer-types"></a><span data-ttu-id="21ce4-134">指標類型</span><span class="sxs-lookup"><span data-stu-id="21ce4-134">Pointer types</span></span>

<span data-ttu-id="21ce4-135">在不安全的內容中，*類型*（[類型](types.md)）可能是*pointer_type* ，也可以是*value_type*或*reference_type*。</span><span class="sxs-lookup"><span data-stu-id="21ce4-135">In an unsafe context, a *type* ([Types](types.md)) may be a *pointer_type* as well as a *value_type* or a *reference_type*.</span></span> <span data-ttu-id="21ce4-136">不過， *pointer_type*也可用於不安全內容外的 @no__t 1 運算式（[匿名物件建立運算式](expressions.md#anonymous-object-creation-expressions)），因為這種用法並不安全。</span><span class="sxs-lookup"><span data-stu-id="21ce4-136">However, a *pointer_type* may also be used in a `typeof` expression ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) outside of an unsafe context as such usage is not unsafe.</span></span>

```antlr
type_unsafe
    : pointer_type
    ;
```

<span data-ttu-id="21ce4-137">*Pointer_type*會寫成*unmanaged_type*或關鍵字 `void`，後面接著 `*` token：</span><span class="sxs-lookup"><span data-stu-id="21ce4-137">A *pointer_type* is written as an *unmanaged_type* or the keyword `void`, followed by a `*` token:</span></span>

```antlr
pointer_type
    : unmanaged_type '*'
    | 'void' '*'
    ;

unmanaged_type
    : type
    ;
```

<span data-ttu-id="21ce4-138">指標類型中 `*` 之前指定的類型，稱為指標類型的***參考型別***。</span><span class="sxs-lookup"><span data-stu-id="21ce4-138">The type specified before the `*` in a pointer type is called the ***referent type*** of the pointer type.</span></span> <span data-ttu-id="21ce4-139">它代表指標類型的值所指向的變數類型。</span><span class="sxs-lookup"><span data-stu-id="21ce4-139">It represents the type of the variable to which a value of the pointer type points.</span></span>

<span data-ttu-id="21ce4-140">不同于參考（參考型別的值），垃圾收集行程不會追蹤指標，垃圾收集行程並不知道指標和其指向的資料。</span><span class="sxs-lookup"><span data-stu-id="21ce4-140">Unlike references (values of reference types), pointers are not tracked by the garbage collector -- the garbage collector has no knowledge of pointers and the data to which they point.</span></span> <span data-ttu-id="21ce4-141">因此，指標不允許指向參考或包含參考的結構，而且指標的參考型別必須是*unmanaged_type*。</span><span class="sxs-lookup"><span data-stu-id="21ce4-141">For this reason a pointer is not permitted to point to a reference or to a struct that contains references, and the referent type of a pointer must be an *unmanaged_type*.</span></span>

<span data-ttu-id="21ce4-142">*Unmanaged_type*是不是*reference_type*或結構化類型的任何類型，而且不包含任何嵌套層級的*reference_type*或結構化類型欄位。</span><span class="sxs-lookup"><span data-stu-id="21ce4-142">An *unmanaged_type* is any type that isn't a *reference_type* or constructed type, and doesn't contain *reference_type* or constructed type fields at any level of nesting.</span></span> <span data-ttu-id="21ce4-143">換句話說， *unmanaged_type*是下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="21ce4-143">In other words, an *unmanaged_type* is one of the following:</span></span>

*  <span data-ttu-id="21ce4-144">`sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、0、1 或 2。</span><span class="sxs-lookup"><span data-stu-id="21ce4-144">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, or `bool`.</span></span>
*  <span data-ttu-id="21ce4-145">任何*enum_type*。</span><span class="sxs-lookup"><span data-stu-id="21ce4-145">Any *enum_type*.</span></span>
*  <span data-ttu-id="21ce4-146">任何*pointer_type*。</span><span class="sxs-lookup"><span data-stu-id="21ce4-146">Any *pointer_type*.</span></span>
*  <span data-ttu-id="21ce4-147">任何不是結構化型別且僅包含*unmanaged_type*之欄位的使用者定義*struct_type* 。</span><span class="sxs-lookup"><span data-stu-id="21ce4-147">Any user-defined *struct_type* that is not a constructed type and contains fields of *unmanaged_type*s only.</span></span>

<span data-ttu-id="21ce4-148">混合使用指標和參考的直覺規則是，參考（物件）的物件可以包含指標，但不允許指標的物件包含參考。</span><span class="sxs-lookup"><span data-stu-id="21ce4-148">The intuitive rule for mixing of pointers and references is that referents of references (objects) are permitted to contain pointers, but referents of pointers are not permitted to contain references.</span></span>

<span data-ttu-id="21ce4-149">下表提供指標類型的一些範例：</span><span class="sxs-lookup"><span data-stu-id="21ce4-149">Some examples of pointer types are given in the table below:</span></span>

| <span data-ttu-id="21ce4-150">__範例__</span><span class="sxs-lookup"><span data-stu-id="21ce4-150">__Example__</span></span> | <span data-ttu-id="21ce4-151">__描述__</span><span class="sxs-lookup"><span data-stu-id="21ce4-151">__Description__</span></span>                               |
|-------------|-----------------------------------------------|
| `byte*`     | <span data-ttu-id="21ce4-152">@No__t 的指標-0</span><span class="sxs-lookup"><span data-stu-id="21ce4-152">Pointer to `byte`</span></span>                             |
| `char*`     | <span data-ttu-id="21ce4-153">@No__t 的指標-0</span><span class="sxs-lookup"><span data-stu-id="21ce4-153">Pointer to `char`</span></span>                             |
| `int**`     | <span data-ttu-id="21ce4-154">指標指向 `int`</span><span class="sxs-lookup"><span data-stu-id="21ce4-154">Pointer to pointer to `int`</span></span>                   |
| `int*[]`    | <span data-ttu-id="21ce4-155">@No__t-0 的一維指標陣列</span><span class="sxs-lookup"><span data-stu-id="21ce4-155">Single-dimensional array of pointers to `int`</span></span> |
| `void*`     | <span data-ttu-id="21ce4-156">未知類型的指標</span><span class="sxs-lookup"><span data-stu-id="21ce4-156">Pointer to unknown type</span></span>                       |

<span data-ttu-id="21ce4-157">若為指定的執行，所有指標類型都必須具有相同的大小和表示。</span><span class="sxs-lookup"><span data-stu-id="21ce4-157">For a given implementation, all pointer types must have the same size and representation.</span></span>

<span data-ttu-id="21ce4-158">不同于 C C++和，在相同宣告中宣告多個指標時， C#在中，`*` 會與基礎類型一併寫入，而不是在每個指標名稱上當做前置詞標點符號。</span><span class="sxs-lookup"><span data-stu-id="21ce4-158">Unlike C and C++, when multiple pointers are declared in the same declaration, in C# the `*` is written along with the underlying type only, not as a prefix punctuator on each pointer name.</span></span> <span data-ttu-id="21ce4-159">例如：</span><span class="sxs-lookup"><span data-stu-id="21ce4-159">For example</span></span>

```csharp
int* pi, pj;    // NOT as int *pi, *pj;
```

<span data-ttu-id="21ce4-160">類型為 `T*` 的指標值表示 `T` 類型的變數位址。</span><span class="sxs-lookup"><span data-stu-id="21ce4-160">The value of a pointer having type `T*` represents the address of a variable of type `T`.</span></span> <span data-ttu-id="21ce4-161">指標間接運算子 `*` （[指標間接](unsafe-code.md#pointer-indirection)取值）可用來存取這個變數。</span><span class="sxs-lookup"><span data-stu-id="21ce4-161">The pointer indirection operator `*` ([Pointer indirection](unsafe-code.md#pointer-indirection)) may be used to access this variable.</span></span> <span data-ttu-id="21ce4-162">例如，假設 `int*` 類型的變數 `P`，則運算式 `*P` 表示在 `P` 中所包含的位址上找到的 `int` 變數。</span><span class="sxs-lookup"><span data-stu-id="21ce4-162">For example, given a variable `P` of type `int*`, the expression `*P` denotes the `int` variable found at the address contained in `P`.</span></span>

<span data-ttu-id="21ce4-163">就像物件參考一樣，指標可能會 `null`。</span><span class="sxs-lookup"><span data-stu-id="21ce4-163">Like an object reference, a pointer may be `null`.</span></span> <span data-ttu-id="21ce4-164">將間接運算子套用至 @no__t 0 指標會導致實作為定義的行為。</span><span class="sxs-lookup"><span data-stu-id="21ce4-164">Applying the indirection operator to a `null` pointer results in implementation-defined behavior.</span></span> <span data-ttu-id="21ce4-165">值為 `null` 的指標是以全位-零表示。</span><span class="sxs-lookup"><span data-stu-id="21ce4-165">A pointer with value `null` is represented by all-bits-zero.</span></span>

<span data-ttu-id="21ce4-166">@No__t-0 類型代表未知類型的指標。</span><span class="sxs-lookup"><span data-stu-id="21ce4-166">The `void*` type represents a pointer to an unknown type.</span></span> <span data-ttu-id="21ce4-167">因為參考型別未知，所以間接運算子不能套用至類型 `void*` 的指標，也不能對這類指標執行任何算數運算。</span><span class="sxs-lookup"><span data-stu-id="21ce4-167">Because the referent type is unknown, the indirection operator cannot be applied to a pointer of type `void*`, nor can any arithmetic be performed on such a pointer.</span></span> <span data-ttu-id="21ce4-168">不過，類型 `void*` 的指標可以轉換成任何其他指標類型（反之亦然）。</span><span class="sxs-lookup"><span data-stu-id="21ce4-168">However, a pointer of type `void*` can be cast to any other pointer type (and vice versa).</span></span>

<span data-ttu-id="21ce4-169">指標類型是不同類別的類型。</span><span class="sxs-lookup"><span data-stu-id="21ce4-169">Pointer types are a separate category of types.</span></span> <span data-ttu-id="21ce4-170">不同于參考型別和實數值型別，指標類型不會繼承自 `object`，而且指標類型和 `object` 之間不會有任何轉換。</span><span class="sxs-lookup"><span data-stu-id="21ce4-170">Unlike reference types and value types, pointer types do not inherit from `object` and no conversions exist between pointer types and `object`.</span></span> <span data-ttu-id="21ce4-171">特別的是，指標不支援裝箱和取消裝箱（[裝箱和取消](types.md#boxing-and-unboxing)裝箱）。</span><span class="sxs-lookup"><span data-stu-id="21ce4-171">In particular, boxing and unboxing ([Boxing and unboxing](types.md#boxing-and-unboxing)) are not supported for pointers.</span></span> <span data-ttu-id="21ce4-172">不過，在不同的指標類型之間，以及指標類型與整數類型之間允許轉換。</span><span class="sxs-lookup"><span data-stu-id="21ce4-172">However, conversions are permitted between different pointer types and between pointer types and the integral types.</span></span> <span data-ttu-id="21ce4-173">這會在[指標轉換](unsafe-code.md#pointer-conversions)中說明。</span><span class="sxs-lookup"><span data-stu-id="21ce4-173">This is described in [Pointer conversions](unsafe-code.md#pointer-conversions).</span></span>

<span data-ttu-id="21ce4-174">*Pointer_type*不能當做類型引數（結構化[類型](types.md#constructed-types)）使用，而且類型推斷（[類型推斷](expressions.md#type-inference)）會在泛型方法呼叫上失敗，而此呼叫會將類型引數推斷為指標類型。</span><span class="sxs-lookup"><span data-stu-id="21ce4-174">A *pointer_type* cannot be used as a type argument ([Constructed types](types.md#constructed-types)), and type inference ([Type inference](expressions.md#type-inference)) fails on generic method calls that would have inferred a type argument to be a pointer type.</span></span>

<span data-ttu-id="21ce4-175">*Pointer_type*可以當做 volatile 欄位（[volatile 欄位](classes.md#volatile-fields)）的類型使用。</span><span class="sxs-lookup"><span data-stu-id="21ce4-175">A *pointer_type* may be used as the type of a volatile field ([Volatile fields](classes.md#volatile-fields)).</span></span>

<span data-ttu-id="21ce4-176">雖然指標可以做為 `ref` 或 @no__t 1 參數傳遞，但這麼做可能會造成未定義的行為，因為指標可能會設定為指向在呼叫的方法傳回時不再存在的區域變數，或其用來指向的固定物件。不再是固定的。</span><span class="sxs-lookup"><span data-stu-id="21ce4-176">Although pointers can be passed as `ref` or `out` parameters, doing so can cause undefined behavior, since the pointer may well be set to point to a local variable which no longer exists when the called method returns, or the fixed object to which it used to point, is no longer fixed.</span></span> <span data-ttu-id="21ce4-177">例如:</span><span class="sxs-lookup"><span data-stu-id="21ce4-177">For example:</span></span>

```csharp
using System;

class Test
{
    static int value = 20;

    unsafe static void F(out int* pi1, ref int* pi2) {
        int i = 10;
        pi1 = &i;

        fixed (int* pj = &value) {
            // ...
            pi2 = pj;
        }
    }

    static void Main() {
        int i = 10;
        unsafe {
            int* px1;
            int* px2 = &i;

            F(out px1, ref px2);

            Console.WriteLine("*px1 = {0}, *px2 = {1}",
                *px1, *px2);    // undefined behavior
        }
    }
}
```

<span data-ttu-id="21ce4-178">方法可以傳回某種類型的值，而該類型可以是指標。</span><span class="sxs-lookup"><span data-stu-id="21ce4-178">A method can return a value of some type, and that type can be a pointer.</span></span> <span data-ttu-id="21ce4-179">例如，當給定的連續 `int` 的序列、該序列的元素計數，以及其他 `int` 值的指標時，如果發生相符，下列方法會傳回該序列中該值的位址。否則，它會傳回 `null`：</span><span class="sxs-lookup"><span data-stu-id="21ce4-179">For example, when given a pointer to a contiguous sequence of `int`s, that sequence's element count, and some other `int` value, the following method returns the address of that value in that sequence, if a match occurs; otherwise it returns `null`:</span></span>

```csharp
unsafe static int* Find(int* pi, int size, int value) {
    for (int i = 0; i < size; ++i) {
        if (*pi == value) 
            return pi;
        ++pi;
    }
    return null;
}
```

<span data-ttu-id="21ce4-180">在不安全的內容中，有數個結構可用於操作指標：</span><span class="sxs-lookup"><span data-stu-id="21ce4-180">In an unsafe context, several constructs are available for operating on pointers:</span></span>

*  <span data-ttu-id="21ce4-181">@No__t-0 運算子可用來執行指標間接取值（[指標間接](unsafe-code.md#pointer-indirection)取值）。</span><span class="sxs-lookup"><span data-stu-id="21ce4-181">The `*` operator may be used to perform pointer indirection ([Pointer indirection](unsafe-code.md#pointer-indirection)).</span></span>
*  <span data-ttu-id="21ce4-182">@No__t-0 運算子可用來透過指標（[指標成員存取](unsafe-code.md#pointer-member-access)）來存取結構的成員。</span><span class="sxs-lookup"><span data-stu-id="21ce4-182">The `->` operator may be used to access a member of a struct through a pointer ([Pointer member access](unsafe-code.md#pointer-member-access)).</span></span>
*  <span data-ttu-id="21ce4-183">@No__t-0 運算子可用來為指標（[指標元素存取](unsafe-code.md#pointer-element-access)）編制索引。</span><span class="sxs-lookup"><span data-stu-id="21ce4-183">The `[]` operator may be used to index a pointer ([Pointer element access](unsafe-code.md#pointer-element-access)).</span></span>
*  <span data-ttu-id="21ce4-184">@No__t-0 運算子可用來取得變數的位址（[即位址運算子](unsafe-code.md#the-address-of-operator)）。</span><span class="sxs-lookup"><span data-stu-id="21ce4-184">The `&` operator may be used to obtain the address of a variable ([The address-of operator](unsafe-code.md#the-address-of-operator)).</span></span>
*  <span data-ttu-id="21ce4-185">@No__t-0 和 @no__t 1 運算子可用來遞增和遞減指標（[指標遞增和遞減](unsafe-code.md#pointer-increment-and-decrement)）。</span><span class="sxs-lookup"><span data-stu-id="21ce4-185">The `++` and `--` operators may be used to increment and decrement pointers ([Pointer increment and decrement](unsafe-code.md#pointer-increment-and-decrement)).</span></span>
*  <span data-ttu-id="21ce4-186">@No__t-0 和 @no__t 1 運算子可用來執行指標算術（[指標算術](unsafe-code.md#pointer-arithmetic)）。</span><span class="sxs-lookup"><span data-stu-id="21ce4-186">The `+` and `-` operators may be used to perform pointer arithmetic ([Pointer arithmetic](unsafe-code.md#pointer-arithmetic)).</span></span>
*  <span data-ttu-id="21ce4-187">@No__t-0、`!=`、@no__t 2、`>`、`<=` 和 @no__t 5 運算子可用來比較指標（[指標比較](unsafe-code.md#pointer-comparison)）。</span><span class="sxs-lookup"><span data-stu-id="21ce4-187">The `==`, `!=`, `<`, `>`, `<=`, and `=>` operators may be used to compare pointers ([Pointer comparison](unsafe-code.md#pointer-comparison)).</span></span>
*  <span data-ttu-id="21ce4-188">@No__t-0 運算子可用來從呼叫堆疊配置記憶體（[固定大小緩衝區](unsafe-code.md#fixed-size-buffers)）。</span><span class="sxs-lookup"><span data-stu-id="21ce4-188">The `stackalloc` operator may be used to allocate memory from the call stack ([Fixed size buffers](unsafe-code.md#fixed-size-buffers)).</span></span>
*  <span data-ttu-id="21ce4-189">您可以使用 `fixed` 語句來暫時修正變數，以便取得其位址（[fixed 語句](unsafe-code.md#the-fixed-statement)）。</span><span class="sxs-lookup"><span data-stu-id="21ce4-189">The `fixed` statement may be used to temporarily fix a variable so its address can be obtained ([The fixed statement](unsafe-code.md#the-fixed-statement)).</span></span>

## <a name="fixed-and-moveable-variables"></a><span data-ttu-id="21ce4-190">固定和可移動變數</span><span class="sxs-lookup"><span data-stu-id="21ce4-190">Fixed and moveable variables</span></span>

<span data-ttu-id="21ce4-191">Address 運算子（「位址」[運算子](unsafe-code.md#the-address-of-operator)）和 @no__t 1 語句（[fixed 語句](unsafe-code.md#the-fixed-statement)）會將變數分成兩個類別：已***修正變數***和***可移動的變數***。</span><span class="sxs-lookup"><span data-stu-id="21ce4-191">The address-of operator ([The address-of operator](unsafe-code.md#the-address-of-operator)) and the `fixed` statement ([The fixed statement](unsafe-code.md#the-fixed-statement)) divide variables into two categories: ***Fixed variables*** and ***moveable variables***.</span></span>

<span data-ttu-id="21ce4-192">固定的變數位於不受垃圾收集行程操作影響的儲存位置。</span><span class="sxs-lookup"><span data-stu-id="21ce4-192">Fixed variables reside in storage locations that are unaffected by operation of the garbage collector.</span></span> <span data-ttu-id="21ce4-193">（固定變數的範例包括區域變數、值參數，以及透過引用指標所建立的變數）。另一方面，可移動變數位於可能會由垃圾收集行程重新配置或處置的儲存位置中。</span><span class="sxs-lookup"><span data-stu-id="21ce4-193">(Examples of fixed variables include local variables, value parameters, and variables created by dereferencing pointers.) On the other hand, moveable variables reside in storage locations that are subject to relocation or disposal by the garbage collector.</span></span> <span data-ttu-id="21ce4-194">（可移動變數的範例包括物件中的欄位和陣列的元素）。</span><span class="sxs-lookup"><span data-stu-id="21ce4-194">(Examples of moveable variables include fields in objects and elements of arrays.)</span></span>

<span data-ttu-id="21ce4-195">@No__t-0 運算子（[通訊運算子](unsafe-code.md#the-address-of-operator)）允許取得固定變數的位址，而不受限制。</span><span class="sxs-lookup"><span data-stu-id="21ce4-195">The `&` operator ([The address-of operator](unsafe-code.md#the-address-of-operator)) permits the address of a fixed variable to be obtained without restrictions.</span></span> <span data-ttu-id="21ce4-196">不過，因為可移動變數可能會由垃圾收集行程重新配置或處置，所以可移動變數的位址只能使用 `fixed` 語句（[fixed 語句](unsafe-code.md#the-fixed-statement)）取得，而且該位址只會針對@no__t 2 語句的持續時間。</span><span class="sxs-lookup"><span data-stu-id="21ce4-196">However, because a moveable variable is subject to relocation or disposal by the garbage collector, the address of a moveable variable can only be obtained using a `fixed` statement ([The fixed statement](unsafe-code.md#the-fixed-statement)), and that address remains valid only for the duration of that `fixed` statement.</span></span>

<span data-ttu-id="21ce4-197">具體而言，固定的變數是下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="21ce4-197">In precise terms, a fixed variable is one of the following:</span></span>

*  <span data-ttu-id="21ce4-198">由參考區域變數或值參數的*simple_name* （[簡單名稱](expressions.md#simple-names)）所產生的變數，除非該變數是由匿名函數所捕捉。</span><span class="sxs-lookup"><span data-stu-id="21ce4-198">A variable resulting from a *simple_name* ([Simple names](expressions.md#simple-names)) that refers to a local variable or a value parameter, unless the variable is captured by an anonymous function.</span></span>
*  <span data-ttu-id="21ce4-199">由 `V.I` 格式的*member_access* （[成員存取](expressions.md#member-access)）所產生的變數，其中 `V` 是*struct_type*的固定變數。</span><span class="sxs-lookup"><span data-stu-id="21ce4-199">A variable resulting from a *member_access* ([Member access](expressions.md#member-access)) of the form `V.I`, where `V` is a fixed variable of a *struct_type*.</span></span>
*  <span data-ttu-id="21ce4-200">從*pointer_indirection_expression* （[指標間接](unsafe-code.md#pointer-indirection)取值）形成的變數，其格式為 `*P`、格式為 `P->I` 的*pointer_member_access* （[指標成員存取](unsafe-code.md#pointer-member-access)），或*pointer_element_access* （[指標元素存取](unsafe-code.md#pointer-element-access)）的格式為 `P[E]`。</span><span class="sxs-lookup"><span data-stu-id="21ce4-200">A variable resulting from a *pointer_indirection_expression* ([Pointer indirection](unsafe-code.md#pointer-indirection)) of the form `*P`, a *pointer_member_access* ([Pointer member access](unsafe-code.md#pointer-member-access)) of the form `P->I`, or a *pointer_element_access* ([Pointer element access](unsafe-code.md#pointer-element-access)) of the form `P[E]`.</span></span>

<span data-ttu-id="21ce4-201">所有其他變數都會分類為可移動變數。</span><span class="sxs-lookup"><span data-stu-id="21ce4-201">All other variables are classified as moveable variables.</span></span>

<span data-ttu-id="21ce4-202">請注意，靜態欄位會分類為可移動變數。</span><span class="sxs-lookup"><span data-stu-id="21ce4-202">Note that a static field is classified as a moveable variable.</span></span> <span data-ttu-id="21ce4-203">另請注意，即使為參數提供的引數是固定的變數，@no__t 0 或 @no__t 1 參數也會分類為可移動變數。</span><span class="sxs-lookup"><span data-stu-id="21ce4-203">Also note that a `ref` or `out` parameter is classified as a moveable variable, even if the argument given for the parameter is a fixed variable.</span></span> <span data-ttu-id="21ce4-204">最後，請注意，透過取值指標所產生的變數一律會分類為固定的變數。</span><span class="sxs-lookup"><span data-stu-id="21ce4-204">Finally, note that a variable produced by dereferencing a pointer is always classified as a fixed variable.</span></span>

## <a name="pointer-conversions"></a><span data-ttu-id="21ce4-205">指標轉換</span><span class="sxs-lookup"><span data-stu-id="21ce4-205">Pointer conversions</span></span>

<span data-ttu-id="21ce4-206">在不安全的內容中，可用的隱含轉換集合（[隱含轉換](conversions.md#implicit-conversions)）會擴充以包含下列隱含指標轉換：</span><span class="sxs-lookup"><span data-stu-id="21ce4-206">In an unsafe context, the set of available implicit conversions ([Implicit conversions](conversions.md#implicit-conversions)) is extended to include the following implicit pointer conversions:</span></span>

*  <span data-ttu-id="21ce4-207">從任何*pointer_type*到類型 `void*`。</span><span class="sxs-lookup"><span data-stu-id="21ce4-207">From any *pointer_type* to the type `void*`.</span></span>
*  <span data-ttu-id="21ce4-208">從 `null` 常值到任何*pointer_type*。</span><span class="sxs-lookup"><span data-stu-id="21ce4-208">From the `null` literal to any *pointer_type*.</span></span>

<span data-ttu-id="21ce4-209">此外，在不安全的內容中，可用的明確轉換（[明確轉換](conversions.md#explicit-conversions)）集合會擴充以包含下列明確指標轉換：</span><span class="sxs-lookup"><span data-stu-id="21ce4-209">Additionally, in an unsafe context, the set of available explicit conversions ([Explicit conversions](conversions.md#explicit-conversions)) is extended to include the following explicit pointer conversions:</span></span>

*  <span data-ttu-id="21ce4-210">從任何*pointer_type*到任何其他*pointer_type*。</span><span class="sxs-lookup"><span data-stu-id="21ce4-210">From any *pointer_type* to any other *pointer_type*.</span></span>
*  <span data-ttu-id="21ce4-211">從 `sbyte`，`byte`，`short`，`ushort`，`int`，`uint`，`long`，或 `ulong` 到任何*pointer_type*。</span><span class="sxs-lookup"><span data-stu-id="21ce4-211">From `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `ulong` to any *pointer_type*.</span></span>
*  <span data-ttu-id="21ce4-212">從任何*pointer_type*到 `sbyte`，`byte`，`short`，`ushort`，`int`，`uint`，`long` 或 `ulong`。</span><span class="sxs-lookup"><span data-stu-id="21ce4-212">From any *pointer_type* to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `ulong`.</span></span>

<span data-ttu-id="21ce4-213">最後，在不安全的內容中，標準隱含轉換（[標準隱含轉換](conversions.md#standard-implicit-conversions)）的集合包含下列指標轉換：</span><span class="sxs-lookup"><span data-stu-id="21ce4-213">Finally, in an unsafe context, the set of standard implicit conversions ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) includes the following pointer conversion:</span></span>

*  <span data-ttu-id="21ce4-214">從任何*pointer_type*到類型 `void*`。</span><span class="sxs-lookup"><span data-stu-id="21ce4-214">From any *pointer_type* to the type `void*`.</span></span>

<span data-ttu-id="21ce4-215">兩個指標類型之間的轉換永遠不會變更實際的指標值。</span><span class="sxs-lookup"><span data-stu-id="21ce4-215">Conversions between two pointer types never change the actual pointer value.</span></span> <span data-ttu-id="21ce4-216">換句話說，從一個指標類型轉換成另一個，並不會影響指標所提供的基礎位址。</span><span class="sxs-lookup"><span data-stu-id="21ce4-216">In other words, a conversion from one pointer type to another has no effect on the underlying address given by the pointer.</span></span>

<span data-ttu-id="21ce4-217">當其中一個指標類型轉換成另一個時，如果產生的指標未正確對齊所指的類型，則如果已取值，則行為會是未定義的。</span><span class="sxs-lookup"><span data-stu-id="21ce4-217">When one pointer type is converted to another, if the resulting pointer is not correctly aligned for the pointed-to type, the behavior is undefined if the result is dereferenced.</span></span> <span data-ttu-id="21ce4-218">一般來說，「正確對齊」這個概念是可轉移的：如果類型的指標 `A` 已正確對齊型別的指標 `B`，而後者則是正確對齊型別 `C` 的指標，則 `A` 類型的指標會正確對齊類型的指標 `C`。</span><span class="sxs-lookup"><span data-stu-id="21ce4-218">In general, the concept "correctly aligned" is transitive: if a pointer to type `A` is correctly aligned for a pointer to type `B`, which, in turn, is correctly aligned for a pointer to type `C`, then a pointer to type `A` is correctly aligned for a pointer to type `C`.</span></span>

<span data-ttu-id="21ce4-219">請考慮下列情況，其中具有一個類型的變數是透過不同類型的指標來存取：</span><span class="sxs-lookup"><span data-stu-id="21ce4-219">Consider the following case in which a variable having one type is accessed via a pointer to a different type:</span></span>

```csharp
char c = 'A';
char* pc = &c;
void* pv = pc;
int* pi = (int*)pv;
int i = *pi;         // undefined
*pi = 123456;        // undefined
```

<span data-ttu-id="21ce4-220">當指標類型轉換成 byte 的指標時，結果會指向變數的最低定址位元組。</span><span class="sxs-lookup"><span data-stu-id="21ce4-220">When a pointer type is converted to a pointer to byte, the result points to the lowest addressed byte of the variable.</span></span> <span data-ttu-id="21ce4-221">連續遞增的結果（最大為變數的大小）會產生該變數剩餘位元組的指標。</span><span class="sxs-lookup"><span data-stu-id="21ce4-221">Successive increments of the result, up to the size of the variable, yield pointers to the remaining bytes of that variable.</span></span> <span data-ttu-id="21ce4-222">例如，下列方法會將 double 中的每個位元組顯示為十六進位值：</span><span class="sxs-lookup"><span data-stu-id="21ce4-222">For example, the following method displays each of the eight bytes in a double as a hexadecimal value:</span></span>

```csharp
using System;

class Test
{
    unsafe static void Main() {
      double d = 123.456e23;
        unsafe {
           byte* pb = (byte*)&d;
            for (int i = 0; i < sizeof(double); ++i)
               Console.Write("{0:X2} ", *pb++);
            Console.WriteLine();
        }
    }
}
```

<span data-ttu-id="21ce4-223">當然，產生的輸出取決於 endian。</span><span class="sxs-lookup"><span data-stu-id="21ce4-223">Of course, the output produced depends on endianness.</span></span>

<span data-ttu-id="21ce4-224">指標和整數之間的對應是執行定義的。</span><span class="sxs-lookup"><span data-stu-id="21ce4-224">Mappings between pointers and integers are implementation-defined.</span></span> <span data-ttu-id="21ce4-225">不過，在具有線性位址空間的 32 \* 和64位 CPU 架構上，從整數類資料類型來回轉換的行為，通常會與這些整數類型之間的 `uint` 或 @no__t 1 值的轉換完全相同。</span><span class="sxs-lookup"><span data-stu-id="21ce4-225">However, on 32\* and 64-bit CPU architectures with a linear address space, conversions of pointers to or from integral types typically behave exactly like conversions of `uint` or `ulong` values, respectively, to or from those integral types.</span></span>

### <a name="pointer-arrays"></a><span data-ttu-id="21ce4-226">指標陣列</span><span class="sxs-lookup"><span data-stu-id="21ce4-226">Pointer arrays</span></span>

<span data-ttu-id="21ce4-227">在不安全的內容中，可以構造指標陣列。</span><span class="sxs-lookup"><span data-stu-id="21ce4-227">In an unsafe context, arrays of pointers can be constructed.</span></span> <span data-ttu-id="21ce4-228">只有一些適用于其他陣列類型的轉換可以在指標陣列上使用：</span><span class="sxs-lookup"><span data-stu-id="21ce4-228">Only some of the conversions that apply to other array types are allowed on pointer arrays:</span></span>

*  <span data-ttu-id="21ce4-229">隱含參考轉換（[隱含參考](conversions.md#implicit-reference-conversions)轉換）從任何*array_type*到 `System.Array`，而它所執行的介面也適用于指標陣列。</span><span class="sxs-lookup"><span data-stu-id="21ce4-229">The implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) from any *array_type* to `System.Array` and the interfaces it implements also applies to pointer arrays.</span></span> <span data-ttu-id="21ce4-230">不過，嘗試透過 `System.Array` 或其所執行的介面來存取陣列專案，將會在執行時間導致例外狀況，因為指標類型無法轉換成 `object`。</span><span class="sxs-lookup"><span data-stu-id="21ce4-230">However, any attempt to access the array elements through `System.Array` or the interfaces it implements will result in an exception at run-time, as pointer types are not convertible to `object`.</span></span>
*  <span data-ttu-id="21ce4-231">從一維陣列類型的隱含和明確參考轉換（[隱含參考轉換](conversions.md#implicit-reference-conversions)、[明確參考](conversions.md#explicit-reference-conversions)轉換） `S[]` 到 `System.Collections.Generic.IList<T>`，而且其泛型基底介面永遠不會套用至指標陣列，由於指標類型不能當做類型引數使用，而且不會從指標類型轉換成非指標類型。</span><span class="sxs-lookup"><span data-stu-id="21ce4-231">The implicit and explicit reference conversions ([Implicit reference conversions](conversions.md#implicit-reference-conversions), [Explicit reference conversions](conversions.md#explicit-reference-conversions)) from a single-dimensional array type `S[]` to `System.Collections.Generic.IList<T>` and its generic base interfaces never apply to pointer arrays, since pointer types cannot be used as type arguments, and there are no conversions from pointer types to non-pointer types.</span></span>
*  <span data-ttu-id="21ce4-232">從 `System.Array` 的明確參考轉換（[明確參考轉換](conversions.md#explicit-reference-conversions)）以及它對任何*array_type*所實作用的介面，都會套用至指標陣列。</span><span class="sxs-lookup"><span data-stu-id="21ce4-232">The explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) from `System.Array` and the interfaces it implements to any *array_type* applies to pointer arrays.</span></span>
*  <span data-ttu-id="21ce4-233">從 `System.Collections.Generic.IList<S>` 及其基底介面到一維陣列類型的明確參考轉換（[明確參考轉換](conversions.md#explicit-reference-conversions)） `T[]` 永遠不會套用至指標陣列，因為指標類型不能當做類型引數使用，而且有不會從指標類型轉換成非指標類型。</span><span class="sxs-lookup"><span data-stu-id="21ce4-233">The explicit reference conversions ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) from `System.Collections.Generic.IList<S>` and its base interfaces to a single-dimensional array type `T[]` never applies to pointer arrays, since pointer types cannot be used as type arguments, and there are no conversions from pointer types to non-pointer types.</span></span>

<span data-ttu-id="21ce4-234">這些限制表示在[foreach 語句](statements.md#the-foreach-statement)中所描述的陣列上，@no__t 0 語句的展開無法套用至指標陣列。</span><span class="sxs-lookup"><span data-stu-id="21ce4-234">These restrictions mean that the expansion for the `foreach` statement over arrays described in [The foreach statement](statements.md#the-foreach-statement) cannot be applied to pointer arrays.</span></span> <span data-ttu-id="21ce4-235">相反地，表單的 foreach 語句</span><span class="sxs-lookup"><span data-stu-id="21ce4-235">Instead, a foreach statement of the form</span></span>

```csharp
foreach (V v in x) embedded_statement
```

<span data-ttu-id="21ce4-236">其中 `x` 的類型是格式為 `T[,,...,]` 的陣列類型，而 `N` 是維度的數目減1，而 `T` 或 `V` 是指標類型，則會使用 nested for-迴圈展開，如下所示：</span><span class="sxs-lookup"><span data-stu-id="21ce4-236">where the type of `x` is an array type of the form `T[,,...,]`, `N` is the number of dimensions minus 1 and `T` or `V` is a pointer type, is expanded using nested for-loops as follows:</span></span>

```csharp
{
    T[,,...,] a = x;
    for (int i0 = a.GetLowerBound(0); i0 <= a.GetUpperBound(0); i0++)
    for (int i1 = a.GetLowerBound(1); i1 <= a.GetUpperBound(1); i1++)
    ...
    for (int iN = a.GetLowerBound(N); iN <= a.GetUpperBound(N); iN++) {
        V v = (V)a.GetValue(i0,i1,...,iN);
        embedded_statement
    }
}
```

<span data-ttu-id="21ce4-237">@No__t-0、`i0`、`i1`、...、`iN` 的變數無法在 `x` 或*embedded_statement*或程式的任何其他原始程式碼中看見或存取。</span><span class="sxs-lookup"><span data-stu-id="21ce4-237">The variables `a`, `i0`, `i1`, ..., `iN` are not visible to or accessible to `x` or the *embedded_statement* or any other source code of the program.</span></span> <span data-ttu-id="21ce4-238">變數 `v` 在內嵌語句中是唯讀的。</span><span class="sxs-lookup"><span data-stu-id="21ce4-238">The variable `v` is read-only in the embedded statement.</span></span> <span data-ttu-id="21ce4-239">如果沒有從 `T` （元素類型）到 `V` 的明確轉換（[指標轉換](unsafe-code.md#pointer-conversions)），就會產生錯誤，而且不會採取任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="21ce4-239">If there is not an explicit conversion ([Pointer conversions](unsafe-code.md#pointer-conversions)) from `T` (the element type) to `V`, an error is produced and no further steps are taken.</span></span> <span data-ttu-id="21ce4-240">如果 `x` 的值 `null`，則會在執行時間擲回 `System.NullReferenceException`。</span><span class="sxs-lookup"><span data-stu-id="21ce4-240">If `x` has the value `null`, a `System.NullReferenceException` is thrown at run-time.</span></span>

## <a name="pointers-in-expressions"></a><span data-ttu-id="21ce4-241">運算式中的指標</span><span class="sxs-lookup"><span data-stu-id="21ce4-241">Pointers in expressions</span></span>

<span data-ttu-id="21ce4-242">在不安全的內容中，運算式可能會產生指標類型的結果，但在不安全的內容之外，運算式必須是指標類型才會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="21ce4-242">In an unsafe context, an expression may yield a result of a pointer type, but outside an unsafe context it is a compile-time error for an expression to be of a pointer type.</span></span> <span data-ttu-id="21ce4-243">具體而言，在不安全的內容外部，如果有任何*simple_name* （[簡單名稱](expressions.md#simple-names)）、 *member_access* （[成員存取](expressions.md#member-access)）、 *invocation_expression* （[調用運算式](expressions.md#invocation-expressions)）或 *，就會發生編譯時期錯誤。element_access* （[元素存取](expressions.md#element-access)）是指標類型。</span><span class="sxs-lookup"><span data-stu-id="21ce4-243">In precise terms, outside an unsafe context a compile-time error occurs if any *simple_name* ([Simple names](expressions.md#simple-names)), *member_access* ([Member access](expressions.md#member-access)), *invocation_expression* ([Invocation expressions](expressions.md#invocation-expressions)), or *element_access* ([Element access](expressions.md#element-access)) is of a pointer type.</span></span>

<span data-ttu-id="21ce4-244">在不安全的內容中， *primary_no_array_creation_expression* （[主要運算式](expressions.md#primary-expressions)）和*unary_expression* （[一元運算子](expressions.md#unary-operators)）的生產會允許下列其他的結構：</span><span class="sxs-lookup"><span data-stu-id="21ce4-244">In an unsafe context, the *primary_no_array_creation_expression* ([Primary expressions](expressions.md#primary-expressions)) and *unary_expression* ([Unary operators](expressions.md#unary-operators)) productions permit the following additional constructs:</span></span>

```antlr
primary_no_array_creation_expression_unsafe
    : pointer_member_access
    | pointer_element_access
    | sizeof_expression
    ;

unary_expression_unsafe
    : pointer_indirection_expression
    | addressof_expression
    ;
```

<span data-ttu-id="21ce4-245">下列各節將說明這些結構。</span><span class="sxs-lookup"><span data-stu-id="21ce4-245">These constructs are described in the following sections.</span></span> <span data-ttu-id="21ce4-246">文法會隱含 unsafe 運算子的優先順序和關聯性。</span><span class="sxs-lookup"><span data-stu-id="21ce4-246">The precedence and associativity of the unsafe operators is implied by the grammar.</span></span>

### <a name="pointer-indirection"></a><span data-ttu-id="21ce4-247">指標間接取值</span><span class="sxs-lookup"><span data-stu-id="21ce4-247">Pointer indirection</span></span>

<span data-ttu-id="21ce4-248">*Pointer_indirection_expression*包含星號（`*`），後面接著*unary_expression*。</span><span class="sxs-lookup"><span data-stu-id="21ce4-248">A *pointer_indirection_expression* consists of an asterisk (`*`) followed by a *unary_expression*.</span></span>

```antlr
pointer_indirection_expression
    : '*' unary_expression
    ;
```

<span data-ttu-id="21ce4-249">一元 `*` 運算子代表指標間接取值，用來取得指標指向的變數。</span><span class="sxs-lookup"><span data-stu-id="21ce4-249">The unary `*` operator denotes pointer indirection and is used to obtain the variable to which a pointer points.</span></span> <span data-ttu-id="21ce4-250">評估 `*P` 的結果，其中 `P` 是指標類型的運算式 `T*`，是 `T` 類型的變數。</span><span class="sxs-lookup"><span data-stu-id="21ce4-250">The result of evaluating `*P`, where `P` is an expression of a pointer type `T*`, is a variable of type `T`.</span></span> <span data-ttu-id="21ce4-251">將一元 `*` 運算子套用至類型 `void*` 的運算式或不是指標類型的運算式時，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="21ce4-251">It is a compile-time error to apply the unary `*` operator to an expression of type `void*` or to an expression that isn't of a pointer type.</span></span>

<span data-ttu-id="21ce4-252">將一元 `*` 運算子套用至 @no__t 1 指標的效果是由實作為定義的。</span><span class="sxs-lookup"><span data-stu-id="21ce4-252">The effect of applying the unary `*` operator to a `null` pointer is implementation-defined.</span></span> <span data-ttu-id="21ce4-253">特別是，不保證此作業會擲回 `System.NullReferenceException`。</span><span class="sxs-lookup"><span data-stu-id="21ce4-253">In particular, there is no guarantee that this operation throws a `System.NullReferenceException`.</span></span>

<span data-ttu-id="21ce4-254">如果將不正確值指派給指標，則不會定義一元 `*` 運算子的行為。</span><span class="sxs-lookup"><span data-stu-id="21ce4-254">If an invalid value has been assigned to the pointer, the behavior of the unary `*` operator is undefined.</span></span> <span data-ttu-id="21ce4-255">在一元 `*` 運算子取值指標的無效值中，會有一個位址不適當地對應到所指向的類型（請參閱[指標轉換](unsafe-code.md#pointer-conversions)中的範例），以及其存留期結束後的變數位址。</span><span class="sxs-lookup"><span data-stu-id="21ce4-255">Among the invalid values for dereferencing a pointer by the unary `*` operator are an address inappropriately aligned for the type pointed to (see example in [Pointer conversions](unsafe-code.md#pointer-conversions)), and the address of a variable after the end of its lifetime.</span></span>

<span data-ttu-id="21ce4-256">基於明確指派分析的目的，評估格式為 `*P` 的運算式所產生的變數會被視為一開始指派（[最初指派的變數](variables.md#initially-assigned-variables)）。</span><span class="sxs-lookup"><span data-stu-id="21ce4-256">For purposes of definite assignment analysis, a variable produced by evaluating an expression of the form `*P` is considered initially assigned ([Initially assigned variables](variables.md#initially-assigned-variables)).</span></span>

### <a name="pointer-member-access"></a><span data-ttu-id="21ce4-257">指標成員存取</span><span class="sxs-lookup"><span data-stu-id="21ce4-257">Pointer member access</span></span>

<span data-ttu-id="21ce4-258">*Pointer_member_access*是由*primary_expression*所組成，後面接著 "`->`" 權杖，後面接著一個*識別碼*和一個選擇性的*type_argument_list*。</span><span class="sxs-lookup"><span data-stu-id="21ce4-258">A *pointer_member_access* consists of a *primary_expression*, followed by a "`->`" token, followed by an *identifier* and an optional *type_argument_list*.</span></span>

```antlr
pointer_member_access
    : primary_expression '->' identifier
    ;
```

<span data-ttu-id="21ce4-259">在表單 `P->I` 的指標成員存取中，`P` 必須是 `void*` 以外指標類型的運算式，而且 `I` 必須代表 `P` 點之類型的可存取成員。</span><span class="sxs-lookup"><span data-stu-id="21ce4-259">In a pointer member access of the form `P->I`, `P` must be an expression of a pointer type other than `void*`, and `I` must denote an accessible member of the type to which `P` points.</span></span>

<span data-ttu-id="21ce4-260">@No__t-0 格式的指標成員存取，會完全依照 `(*P).I` 進行評估。</span><span class="sxs-lookup"><span data-stu-id="21ce4-260">A pointer member access of the form `P->I` is evaluated exactly as `(*P).I`.</span></span> <span data-ttu-id="21ce4-261">如需指標間接運算子（`*`）的說明，請參閱[指標間接](unsafe-code.md#pointer-indirection)取值。</span><span class="sxs-lookup"><span data-stu-id="21ce4-261">For a description of the pointer indirection operator (`*`), see [Pointer indirection](unsafe-code.md#pointer-indirection).</span></span> <span data-ttu-id="21ce4-262">如需成員存取運算子（`.`）的說明，請參閱[成員存取](expressions.md#member-access)。</span><span class="sxs-lookup"><span data-stu-id="21ce4-262">For a description of the member access operator (`.`), see [Member access](expressions.md#member-access).</span></span>

<span data-ttu-id="21ce4-263">在範例中</span><span class="sxs-lookup"><span data-stu-id="21ce4-263">In the example</span></span>

```csharp
using System;

struct Point
{
    public int x;
    public int y;

    public override string ToString() {
        return "(" + x + "," + y + ")";
    }
}

class Test
{
    static void Main() {
        Point point;
        unsafe {
            Point* p = &point;
            p->x = 10;
            p->y = 20;
            Console.WriteLine(p->ToString());
        }
    }
}
```

<span data-ttu-id="21ce4-264">`->` 運算子是用來存取欄位，並透過指標叫用結構的方法。</span><span class="sxs-lookup"><span data-stu-id="21ce4-264">the `->` operator is used to access fields and invoke a method of a struct through a pointer.</span></span> <span data-ttu-id="21ce4-265">因為 `P->I` 的作業會精確地等同于 `(*P).I`，所以已撰寫 @no__t 2 方法：</span><span class="sxs-lookup"><span data-stu-id="21ce4-265">Because the operation `P->I` is precisely equivalent to `(*P).I`, the `Main` method could equally well have been written:</span></span>

```csharp
class Test
{
    static void Main() {
        Point point;
        unsafe {
            Point* p = &point;
            (*p).x = 10;
            (*p).y = 20;
            Console.WriteLine((*p).ToString());
        }
    }
}
```

### <a name="pointer-element-access"></a><span data-ttu-id="21ce4-266">指標元素存取</span><span class="sxs-lookup"><span data-stu-id="21ce4-266">Pointer element access</span></span>

<span data-ttu-id="21ce4-267">*Pointer_element_access*包含*primary_no_array_creation_expression* ，後面接著以 "`[`" 和 "`]`" 括住的運算式。</span><span class="sxs-lookup"><span data-stu-id="21ce4-267">A *pointer_element_access* consists of a *primary_no_array_creation_expression* followed by an expression enclosed in "`[`" and "`]`".</span></span>

```antlr
pointer_element_access
    : primary_no_array_creation_expression '[' expression ']'
    ;
```

<span data-ttu-id="21ce4-268">在 `P[E]` 格式的指標專案存取中，`P` 必須是 `void*` 以外指標類型的運算式，而且 `E` 必須是可以隱含轉換成 `int`、`uint`、`long` 或 `ulong` 的運算式。</span><span class="sxs-lookup"><span data-stu-id="21ce4-268">In a pointer element access of the form `P[E]`, `P` must be an expression of a pointer type other than `void*`, and `E` must be an expression that can be implicitly converted to `int`, `uint`, `long`, or `ulong`.</span></span>

<span data-ttu-id="21ce4-269">@No__t-0 格式的指標專案存取，會完全依照 `*(P + E)` 進行評估。</span><span class="sxs-lookup"><span data-stu-id="21ce4-269">A pointer element access of the form `P[E]` is evaluated exactly as `*(P + E)`.</span></span> <span data-ttu-id="21ce4-270">如需指標間接運算子（`*`）的說明，請參閱[指標間接](unsafe-code.md#pointer-indirection)取值。</span><span class="sxs-lookup"><span data-stu-id="21ce4-270">For a description of the pointer indirection operator (`*`), see [Pointer indirection](unsafe-code.md#pointer-indirection).</span></span> <span data-ttu-id="21ce4-271">如需指標加號運算子（`+`）的說明，請參閱[指標算術](unsafe-code.md#pointer-arithmetic)。</span><span class="sxs-lookup"><span data-stu-id="21ce4-271">For a description of the pointer addition operator (`+`), see [Pointer arithmetic](unsafe-code.md#pointer-arithmetic).</span></span>

<span data-ttu-id="21ce4-272">在範例中</span><span class="sxs-lookup"><span data-stu-id="21ce4-272">In the example</span></span>

```csharp
class Test
{
    static void Main() {
        unsafe {
            char* p = stackalloc char[256];
            for (int i = 0; i < 256; i++) p[i] = (char)i;
        }
    }
}
```

<span data-ttu-id="21ce4-273">指標專案存取是用來初始化 `for` 迴圈中的字元緩衝區。</span><span class="sxs-lookup"><span data-stu-id="21ce4-273">a pointer element access is used to initialize the character buffer in a `for` loop.</span></span> <span data-ttu-id="21ce4-274">由於作業 `P[E]` 完全等同于 `*(P + E)`，因此，此範例也可能已撰寫：</span><span class="sxs-lookup"><span data-stu-id="21ce4-274">Because the operation `P[E]` is precisely equivalent to `*(P + E)`, the example could equally well have been written:</span></span>

```csharp
class Test
{
    static void Main() {
        unsafe {
            char* p = stackalloc char[256];
            for (int i = 0; i < 256; i++) *(p + i) = (char)i;
        }
    }
}
```

<span data-ttu-id="21ce4-275">指標元素存取運算子不會檢查超出範圍的錯誤，而且不會定義存取超出範圍的元素時的行為。</span><span class="sxs-lookup"><span data-stu-id="21ce4-275">The pointer element access operator does not check for out-of-bounds errors and the behavior when accessing an out-of-bounds element is undefined.</span></span> <span data-ttu-id="21ce4-276">這與 C 和C++相同。</span><span class="sxs-lookup"><span data-stu-id="21ce4-276">This is the same as C and C++.</span></span>

### <a name="the-address-of-operator"></a><span data-ttu-id="21ce4-277">Address 運算子</span><span class="sxs-lookup"><span data-stu-id="21ce4-277">The address-of operator</span></span>

<span data-ttu-id="21ce4-278">*Addressof_expression*是由後面接著*unary_expression*的連字號（`&`）所組成。</span><span class="sxs-lookup"><span data-stu-id="21ce4-278">An *addressof_expression* consists of an ampersand (`&`) followed by a *unary_expression*.</span></span>

```antlr
addressof_expression
    : '&' unary_expression
    ;
```

<span data-ttu-id="21ce4-279">假設有一個運算式 `E`，這種類型 `T`，並分類為固定變數（[固定和可移動的變數](unsafe-code.md#fixed-and-moveable-variables)），則結構 `&E` 會計算 `E` 所提供的變數位址。</span><span class="sxs-lookup"><span data-stu-id="21ce4-279">Given an expression `E` which is of a type `T` and is classified as a fixed variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)), the construct `&E` computes the address of the variable given by `E`.</span></span> <span data-ttu-id="21ce4-280">結果的類型為 `T*`，並分類為值。</span><span class="sxs-lookup"><span data-stu-id="21ce4-280">The type of the result is `T*` and is classified as a value.</span></span> <span data-ttu-id="21ce4-281">如果 `E` 分類為唯讀區域變數，或如果 `E` 表示可移動的變數，則會發生編譯時期錯誤，如果 `E` 未分類為變數。</span><span class="sxs-lookup"><span data-stu-id="21ce4-281">A compile-time error occurs if `E` is not classified as a variable, if `E` is classified as a read-only local variable, or if `E` denotes a moveable variable.</span></span> <span data-ttu-id="21ce4-282">在最後一個案例中，固定的語句（[fixed 語句](unsafe-code.md#the-fixed-statement)）可以用來暫時「修正」變數，再取得其位址。</span><span class="sxs-lookup"><span data-stu-id="21ce4-282">In the last case, a fixed statement ([The fixed statement](unsafe-code.md#the-fixed-statement)) can be used to temporarily "fix" the variable before obtaining its address.</span></span> <span data-ttu-id="21ce4-283">如[成員存取](expressions.md#member-access)中所述，在定義 @no__t 1 欄位的結構或類別之外，在實例的參數或靜態的函式以外，該欄位會被視為值，而不是變數。</span><span class="sxs-lookup"><span data-stu-id="21ce4-283">As stated in [Member access](expressions.md#member-access), outside an instance constructor or static constructor for a struct or class that defines a `readonly` field, that field is considered a value, not a variable.</span></span> <span data-ttu-id="21ce4-284">因此，無法取得其位址。</span><span class="sxs-lookup"><span data-stu-id="21ce4-284">As such, its address cannot be taken.</span></span> <span data-ttu-id="21ce4-285">同樣地，也無法取得常數的位址。</span><span class="sxs-lookup"><span data-stu-id="21ce4-285">Similarly, the address of a constant cannot be taken.</span></span>

<span data-ttu-id="21ce4-286">@No__t-0 運算子不需要明確指派其引數，但在 `&` 作業之後，套用運算子的變數會被視為在作業發生所在的執行路徑中明確指派。</span><span class="sxs-lookup"><span data-stu-id="21ce4-286">The `&` operator does not require its argument to be definitely assigned, but following an `&` operation, the variable to which the operator is applied is considered definitely assigned in the execution path in which the operation occurs.</span></span> <span data-ttu-id="21ce4-287">程式設計人員必須負責確保在此情況下，確實會進行正確的變數初始化。</span><span class="sxs-lookup"><span data-stu-id="21ce4-287">It is the responsibility of the programmer to ensure that correct initialization of the variable actually does take place in this situation.</span></span>

<span data-ttu-id="21ce4-288">在範例中</span><span class="sxs-lookup"><span data-stu-id="21ce4-288">In the example</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        int i;
        unsafe {
            int* p = &i;
            *p = 123;
        }
        Console.WriteLine(i);
    }
}
```

<span data-ttu-id="21ce4-289">`i` 會被視為依照用來初始化 `p` 的 `&i` 作業進行明確指派。</span><span class="sxs-lookup"><span data-stu-id="21ce4-289">`i` is considered definitely assigned following the `&i` operation used to initialize `p`.</span></span> <span data-ttu-id="21ce4-290">作用中 `*p` 的指派會初始化 `i`，但是加入此初始化是程式設計人員的責任，如果移除指派，則不會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="21ce4-290">The assignment to `*p` in effect initializes `i`, but the inclusion of this initialization is the responsibility of the programmer, and no compile-time error would occur if the assignment was removed.</span></span>

<span data-ttu-id="21ce4-291">@No__t-0 運算子的明確指派規則存在，因此可以避免區域變數的重複初始化。</span><span class="sxs-lookup"><span data-stu-id="21ce4-291">The rules of definite assignment for the `&` operator exist such that redundant initialization of local variables can be avoided.</span></span> <span data-ttu-id="21ce4-292">例如，許多外部 Api 都會採用 API 所填入之結構的指標。</span><span class="sxs-lookup"><span data-stu-id="21ce4-292">For example, many external APIs take a pointer to a structure which is filled in by the API.</span></span> <span data-ttu-id="21ce4-293">對這類 Api 的呼叫通常會傳遞本機結構變數的位址，而且若沒有此規則，就需要重複的結構變數初始化。</span><span class="sxs-lookup"><span data-stu-id="21ce4-293">Calls to such APIs typically pass the address of a local struct variable, and without the rule, redundant initialization of the struct variable would be required.</span></span>

### <a name="pointer-increment-and-decrement"></a><span data-ttu-id="21ce4-294">指標遞增和遞減</span><span class="sxs-lookup"><span data-stu-id="21ce4-294">Pointer increment and decrement</span></span>

<span data-ttu-id="21ce4-295">在不安全的內容中，`++` 和 @no__t 1 運算子（後置[遞增和遞減運算子](expressions.md#postfix-increment-and-decrement-operators)和[前置遞增和遞減運算子](expressions.md#prefix-increment-and-decrement-operators)）可以套用至所有類型的指標變數，但不包括 `void*`。</span><span class="sxs-lookup"><span data-stu-id="21ce4-295">In an unsafe context, the `++` and `--` operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)) can be applied to pointer variables of all types except `void*`.</span></span> <span data-ttu-id="21ce4-296">因此，對於每個指標類型 `T*`，會隱含地定義下列運算子：</span><span class="sxs-lookup"><span data-stu-id="21ce4-296">Thus, for every pointer type `T*`, the following operators are implicitly defined:</span></span>

```csharp
T* operator ++(T* x);
T* operator --(T* x);
```

<span data-ttu-id="21ce4-297">運算子會分別產生與 `x + 1` 和 `x - 1` （[指標算術](unsafe-code.md#pointer-arithmetic)）相同的結果。</span><span class="sxs-lookup"><span data-stu-id="21ce4-297">The operators produce the same results as `x + 1` and `x - 1`, respectively ([Pointer arithmetic](unsafe-code.md#pointer-arithmetic)).</span></span> <span data-ttu-id="21ce4-298">換句話說，針對類型 `T*` 的指標變數，`++` 運算子會將 `sizeof(T)` 新增至變數中包含的位址，而 `--` 運算子會從變數中包含的位址減去 `sizeof(T)`。</span><span class="sxs-lookup"><span data-stu-id="21ce4-298">In other words, for a pointer variable of type `T*`, the `++` operator adds `sizeof(T)` to the address contained in the variable, and the `--` operator subtracts `sizeof(T)` from the address contained in the variable.</span></span>

<span data-ttu-id="21ce4-299">如果指標遞增或遞減運算溢出指標類型的定義域，則結果會是實作為定義，但是不會產生任何例外狀況。</span><span class="sxs-lookup"><span data-stu-id="21ce4-299">If a pointer increment or decrement operation overflows the domain of the pointer type, the result is implementation-defined, but no exceptions are produced.</span></span>

### <a name="pointer-arithmetic"></a><span data-ttu-id="21ce4-300">指標算術</span><span class="sxs-lookup"><span data-stu-id="21ce4-300">Pointer arithmetic</span></span>

<span data-ttu-id="21ce4-301">在不安全的內容中，`+` 和 @no__t 1 運算子（[加法運算子](expressions.md#addition-operator)和[減法運算子](expressions.md#subtraction-operator)）可以套用至所有指標類型的值，但 `void*` 除外。</span><span class="sxs-lookup"><span data-stu-id="21ce4-301">In an unsafe context, the `+` and `-` operators ([Addition operator](expressions.md#addition-operator) and [Subtraction operator](expressions.md#subtraction-operator)) can be applied to values of all pointer types except `void*`.</span></span> <span data-ttu-id="21ce4-302">因此，對於每個指標類型 `T*`，會隱含地定義下列運算子：</span><span class="sxs-lookup"><span data-stu-id="21ce4-302">Thus, for every pointer type `T*`, the following operators are implicitly defined:</span></span>

```csharp
T* operator +(T* x, int y);
T* operator +(T* x, uint y);
T* operator +(T* x, long y);
T* operator +(T* x, ulong y);

T* operator +(int x, T* y);
T* operator +(uint x, T* y);
T* operator +(long x, T* y);
T* operator +(ulong x, T* y);

T* operator -(T* x, int y);
T* operator -(T* x, uint y);
T* operator -(T* x, long y);
T* operator -(T* x, ulong y);

long operator -(T* x, T* y);
```

<span data-ttu-id="21ce4-303">假設有一個運算式 `P` 的指標型別 `T*` 和一個運算式 @no__t `int`，`uint`，`long` 或 `ulong`，則運算式 `P + N` 和 `N + P` 會計算 `T*` 類型的指標值，而結果是將 @no__t 新增至位址提供者為 1。</span><span class="sxs-lookup"><span data-stu-id="21ce4-303">Given an expression `P` of a pointer type `T*` and an expression `N` of type `int`, `uint`, `long`, or `ulong`, the expressions `P + N` and `N + P` compute the pointer value of type `T*` that results from adding `N * sizeof(T)` to the address given by `P`.</span></span> <span data-ttu-id="21ce4-304">同樣地，運算式 `P - N` 會計算 `T*` 類型的指標值，這是從 `P` 所指定的位址減去 `N * sizeof(T)` 所產生的。</span><span class="sxs-lookup"><span data-stu-id="21ce4-304">Likewise, the expression `P - N` computes the pointer value of type `T*` that results from subtracting `N * sizeof(T)` from the address given by `P`.</span></span>

<span data-ttu-id="21ce4-305">假設有兩個運算式（`P` 和 `Q`）的指標類型 `T*`，運算式 `P - Q` 會計算 `P` 和 `Q` 所指定的位址之間的差異，然後將該差異除以 `sizeof(T)`。</span><span class="sxs-lookup"><span data-stu-id="21ce4-305">Given two expressions, `P` and `Q`, of a pointer type `T*`, the expression `P - Q` computes the difference between the addresses given by `P` and `Q` and then divides that difference by `sizeof(T)`.</span></span> <span data-ttu-id="21ce4-306">結果的類型一律為 `long`。</span><span class="sxs-lookup"><span data-stu-id="21ce4-306">The type of the result is always `long`.</span></span> <span data-ttu-id="21ce4-307">實際上，`P - Q` 會計算為 `((long)(P) - (long)(Q)) / sizeof(T)`。</span><span class="sxs-lookup"><span data-stu-id="21ce4-307">In effect, `P - Q` is computed as `((long)(P) - (long)(Q)) / sizeof(T)`.</span></span>

<span data-ttu-id="21ce4-308">例如:</span><span class="sxs-lookup"><span data-stu-id="21ce4-308">For example:</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        unsafe {
            int* values = stackalloc int[20];
            int* p = &values[1];
            int* q = &values[15];
            Console.WriteLine("p - q = {0}", p - q);
            Console.WriteLine("q - p = {0}", q - p);
        }
    }
}
```

<span data-ttu-id="21ce4-309">這會產生輸出：</span><span class="sxs-lookup"><span data-stu-id="21ce4-309">which produces the output:</span></span>

```console
p - q = -14
q - p = 14
```

<span data-ttu-id="21ce4-310">如果指標算數運算溢出指標類型的定義域，則會以實作為定義的方式截斷結果，但不會產生任何例外狀況。</span><span class="sxs-lookup"><span data-stu-id="21ce4-310">If a pointer arithmetic operation overflows the domain of the pointer type, the result is truncated in an implementation-defined fashion, but no exceptions are produced.</span></span>

### <a name="pointer-comparison"></a><span data-ttu-id="21ce4-311">指標比較</span><span class="sxs-lookup"><span data-stu-id="21ce4-311">Pointer comparison</span></span>

<span data-ttu-id="21ce4-312">在不安全的內容中，`==`、`!=`、`<`、`>`、`<=` 和 @no__t 5 運算子（[關聯式和類型測試運算子](expressions.md#relational-and-type-testing-operators)）可以套用至所有指標類型的值。</span><span class="sxs-lookup"><span data-stu-id="21ce4-312">In an unsafe context, the `==`, `!=`, `<`, `>`, `<=`, and `=>` operators ([Relational and type-testing operators](expressions.md#relational-and-type-testing-operators)) can be applied to values of all pointer types.</span></span> <span data-ttu-id="21ce4-313">指標比較運算子如下：</span><span class="sxs-lookup"><span data-stu-id="21ce4-313">The pointer comparison operators are:</span></span>

```csharp
bool operator ==(void* x, void* y);
bool operator !=(void* x, void* y);
bool operator <(void* x, void* y);
bool operator >(void* x, void* y);
bool operator <=(void* x, void* y);
bool operator >=(void* x, void* y);
```

<span data-ttu-id="21ce4-314">因為從任何指標類型到 `void*` 類型都有隱含的轉換，所以可以使用這些運算子來比較任何指標類型的運算元。</span><span class="sxs-lookup"><span data-stu-id="21ce4-314">Because an implicit conversion exists from any pointer type to the `void*` type, operands of any pointer type can be compared using these operators.</span></span> <span data-ttu-id="21ce4-315">比較運算子會比較兩個運算元所指定的位址，如同它們是不帶正負號的整數。</span><span class="sxs-lookup"><span data-stu-id="21ce4-315">The comparison operators compare the addresses given by the two operands as if they were unsigned integers.</span></span>

### <a name="the-sizeof-operator"></a><span data-ttu-id="21ce4-316">Sizeof 運算子</span><span class="sxs-lookup"><span data-stu-id="21ce4-316">The sizeof operator</span></span>

<span data-ttu-id="21ce4-317">`sizeof` 運算子會返回指定型別變數所佔用的位元組總數。</span><span class="sxs-lookup"><span data-stu-id="21ce4-317">The `sizeof` operator returns the number of bytes occupied by a variable of a given type.</span></span> <span data-ttu-id="21ce4-318">指定為 `sizeof` 之運算元的類型必須是*unmanaged_type* （[指標類型](unsafe-code.md#pointer-types)）。</span><span class="sxs-lookup"><span data-stu-id="21ce4-318">The type specified as an operand to `sizeof` must be an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

```antlr
sizeof_expression
    : 'sizeof' '(' unmanaged_type ')'
    ;
```

<span data-ttu-id="21ce4-319">@No__t-0 運算子的結果是 `int` 類型的值。</span><span class="sxs-lookup"><span data-stu-id="21ce4-319">The result of the `sizeof` operator is a value of type `int`.</span></span> <span data-ttu-id="21ce4-320">針對某些預先定義的類型，`sizeof` 運算子會產生常數值，如下表所示。</span><span class="sxs-lookup"><span data-stu-id="21ce4-320">For certain predefined types, the `sizeof` operator yields a constant value as shown in the table below.</span></span>


| <span data-ttu-id="21ce4-321">__運算式__</span><span class="sxs-lookup"><span data-stu-id="21ce4-321">__Expression__</span></span>   | <span data-ttu-id="21ce4-322">__結果__</span><span class="sxs-lookup"><span data-stu-id="21ce4-322">__Result__</span></span> |
|------------------|------------|
| `sizeof(sbyte)`  | `1`        |
| `sizeof(byte)`   | `1`        |
| `sizeof(short)`  | `2`        |
| `sizeof(ushort)` | `2`        |
| `sizeof(int)`    | `4`        |
| `sizeof(uint)`   | `4`        |
| `sizeof(long)`   | `8`        |
| `sizeof(ulong)`  | `8`        |
| `sizeof(char)`   | `2`        |
| `sizeof(float)`  | `4`        |
| `sizeof(double)` | `8`        |
| `sizeof(bool)`   | `1`        |

<span data-ttu-id="21ce4-323">對於所有其他類型，`sizeof` 運算子的結果會定義為實作為值，而不是常數。</span><span class="sxs-lookup"><span data-stu-id="21ce4-323">For all other types, the result of the `sizeof` operator is implementation-defined and is classified as a value, not a constant.</span></span>

<span data-ttu-id="21ce4-324">成員封裝到結構中的順序未指定。</span><span class="sxs-lookup"><span data-stu-id="21ce4-324">The order in which members are packed into a struct is unspecified.</span></span>

<span data-ttu-id="21ce4-325">基於對齊目的，結構的開頭可能會有未命名的填補、在結構中，以及在結構的結尾處。</span><span class="sxs-lookup"><span data-stu-id="21ce4-325">For alignment purposes, there may be unnamed padding at the beginning of a struct, within a struct, and at the end of the struct.</span></span> <span data-ttu-id="21ce4-326">用來做為填補的位內容是不確定的。</span><span class="sxs-lookup"><span data-stu-id="21ce4-326">The contents of the bits used as padding are indeterminate.</span></span>

<span data-ttu-id="21ce4-327">套用至具有結構類型的運算元時，結果會是該類型變數中的總位元組數，包括任何填補。</span><span class="sxs-lookup"><span data-stu-id="21ce4-327">When applied to an operand that has struct type, the result is the total number of bytes in a variable of that type, including any padding.</span></span>

## <a name="the-fixed-statement"></a><span data-ttu-id="21ce4-328">Fixed 語句</span><span class="sxs-lookup"><span data-stu-id="21ce4-328">The fixed statement</span></span>

<span data-ttu-id="21ce4-329">在不安全的內容中， *embedded_statement* （[語句](statements.md)）生產會允許額外的結構，也就是 `fixed` 語句，這是用來「修正」可移動變數，使其位址在語句的持續時間內保持不變.</span><span class="sxs-lookup"><span data-stu-id="21ce4-329">In an unsafe context, the *embedded_statement* ([Statements](statements.md)) production permits an additional construct, the `fixed` statement, which is used to "fix" a moveable variable such that its address remains constant for the duration of the statement.</span></span>

```antlr
fixed_statement
    : 'fixed' '(' pointer_type fixed_pointer_declarators ')' embedded_statement
    ;

fixed_pointer_declarators
    : fixed_pointer_declarator (','  fixed_pointer_declarator)*
    ;

fixed_pointer_declarator
    : identifier '=' fixed_pointer_initializer
    ;

fixed_pointer_initializer
    : '&' variable_reference
    | expression
    ;
```

<span data-ttu-id="21ce4-330">每個*fixed_pointer_declarator*都會宣告給定*pointer_type*的區域變數，並使用對應的*fixed_pointer_initializer*所計算的位址來初始化該本機變數。</span><span class="sxs-lookup"><span data-stu-id="21ce4-330">Each *fixed_pointer_declarator* declares a local variable of the given *pointer_type* and initializes that local variable with the address computed by the corresponding *fixed_pointer_initializer*.</span></span> <span data-ttu-id="21ce4-331">在 `fixed` 語句中宣告的區域變數，可以在該變數宣告的右邊發生的任何*fixed_pointer_initializer*中，以及在 @no__t 3 語句的*embedded_statement*中存取。</span><span class="sxs-lookup"><span data-stu-id="21ce4-331">A local variable declared in a `fixed` statement is accessible in any *fixed_pointer_initializer*s occurring to the right of that variable's declaration, and in the *embedded_statement* of the `fixed` statement.</span></span> <span data-ttu-id="21ce4-332">由 @no__t 0 的語句所宣告的區域變數會被視為唯讀。</span><span class="sxs-lookup"><span data-stu-id="21ce4-332">A local variable declared by a `fixed` statement is considered read-only.</span></span> <span data-ttu-id="21ce4-333">如果內嵌語句嘗試修改這個本機變數（透過指派或 `++` 和 @no__t 1 運算子），或將它當做 `ref` 或 `out` 參數傳遞，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="21ce4-333">A compile-time error occurs if the embedded statement attempts to modify this local variable (via assignment or the `++` and `--` operators) or pass it as a `ref` or `out` parameter.</span></span>

<span data-ttu-id="21ce4-334">*Fixed_pointer_initializer*可以是下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="21ce4-334">A *fixed_pointer_initializer* can be one of the following:</span></span>

*  <span data-ttu-id="21ce4-335">Token "`&`" 後面接著*variable_reference* （[判斷明確指派的精確規則](variables.md#precise-rules-for-determining-definite-assignment)） `T` 的非受控類型的可移動變數（[固定和可移動變數](unsafe-code.md#fixed-and-moveable-variables)），前提是 `T*` 的類型為可隱含轉換為 `fixed` 語句中提供的指標類型。</span><span class="sxs-lookup"><span data-stu-id="21ce4-335">The token "`&`" followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) to a moveable variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)) of an unmanaged type `T`, provided the type `T*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="21ce4-336">在此情況下，初始化運算式會計算給定變數的位址，並保證在 `fixed` 語句期間，變數會保留在固定位址。</span><span class="sxs-lookup"><span data-stu-id="21ce4-336">In this case, the initializer computes the address of the given variable, and the variable is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span>
*  <span data-ttu-id="21ce4-337">*Array_type*的運算式，其中包含非受控類型的專案 `T`，前提是類型 `T*` 可以隱含地轉換成 `fixed` 語句中提供的指標類型。</span><span class="sxs-lookup"><span data-stu-id="21ce4-337">An expression of an *array_type* with elements of an unmanaged type `T`, provided the type `T*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="21ce4-338">在此情況下，初始化運算式會計算陣列中第一個元素的位址，而整個陣列保證會在 `fixed` 語句期間保持固定位址。</span><span class="sxs-lookup"><span data-stu-id="21ce4-338">In this case, the initializer computes the address of the first element in the array, and the entire array is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span> <span data-ttu-id="21ce4-339">如果陣列運算式為 null，或者陣列有零個元素，則初始化運算式會將位址計算為等於零。</span><span class="sxs-lookup"><span data-stu-id="21ce4-339">If the array expression is null or if the array has zero elements, the initializer computes an address equal to zero.</span></span>
*  <span data-ttu-id="21ce4-340">類型為 `string` 的運算式，但前提是類型 `char*` 可以隱含地轉換成 `fixed` 語句中提供的指標類型。</span><span class="sxs-lookup"><span data-stu-id="21ce4-340">An expression of type `string`, provided the type `char*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="21ce4-341">在此情況下，初始化運算式會計算字串中第一個字元的位址，而在 `fixed` 語句期間，整個字串保證會保留在固定位址。</span><span class="sxs-lookup"><span data-stu-id="21ce4-341">In this case, the initializer computes the address of the first character in the string, and the entire string is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span> <span data-ttu-id="21ce4-342">如果字串運算式為 null，則 `fixed` 語句的行為會定義為「執行中」。</span><span class="sxs-lookup"><span data-stu-id="21ce4-342">The behavior of the `fixed` statement is implementation-defined if the string expression is null.</span></span>
*  <span data-ttu-id="21ce4-343">*Simple_name*或*member_access* ，參考可移動變數的固定大小緩衝區成員，前提是固定大小緩衝區成員的類型可以隱含地轉換成在 `fixed` 語句中指定的指標類型。</span><span class="sxs-lookup"><span data-stu-id="21ce4-343">A *simple_name* or *member_access* that references a fixed size buffer member of a moveable variable, provided the type of the fixed size buffer member is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="21ce4-344">在此情況下，初始化運算式會計算固定大小緩衝區之第一個元素的指標（[運算式中的固定大小緩衝區](unsafe-code.md#fixed-size-buffers-in-expressions)），而固定大小的緩衝區保證會在 `fixed` 語句期間保留在固定位址。</span><span class="sxs-lookup"><span data-stu-id="21ce4-344">In this case, the initializer computes a pointer to the first element of the fixed size buffer ([Fixed size buffers in expressions](unsafe-code.md#fixed-size-buffers-in-expressions)), and the fixed size buffer is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span>

<span data-ttu-id="21ce4-345">對於*fixed_pointer_initializer*所計算的每個位址，`fixed` 語句可確保在 @no__t 2 語句期間，垃圾收集行程不會重新配置或處置位址所參考的變數。</span><span class="sxs-lookup"><span data-stu-id="21ce4-345">For each address computed by a *fixed_pointer_initializer* the `fixed` statement ensures that the variable referenced by the address is not subject to relocation or disposal by the garbage collector for the duration of the `fixed` statement.</span></span> <span data-ttu-id="21ce4-346">例如，如果*fixed_pointer_initializer*所計算的位址參考了物件的欄位或陣列實例的元素，則 @no__t 1 語句可保證包含的物件實例在執行期間不會重新置放或處置語句的存留期。</span><span class="sxs-lookup"><span data-stu-id="21ce4-346">For example, if the address computed by a *fixed_pointer_initializer* references a field of an object or an element of an array instance, the `fixed` statement guarantees that the containing object instance is not relocated or disposed of during the lifetime of the statement.</span></span>

<span data-ttu-id="21ce4-347">程式設計人員必須負責確保 @no__t 0 語句所建立的指標不會在執行這些語句之後存留下來。</span><span class="sxs-lookup"><span data-stu-id="21ce4-347">It is the programmer's responsibility to ensure that pointers created by `fixed` statements do not survive beyond execution of those statements.</span></span> <span data-ttu-id="21ce4-348">例如，當由 @no__t 0 的語句所建立的指標傳遞給外部 Api 時，程式設計人員必須負責確保 Api 不會保留這些指標的任何記憶體。</span><span class="sxs-lookup"><span data-stu-id="21ce4-348">For example, when pointers created by `fixed` statements are passed to external APIs, it is the programmer's responsibility to ensure that the APIs retain no memory of these pointers.</span></span>

<span data-ttu-id="21ce4-349">固定物件可能會造成堆積的片段（因為無法移動）。</span><span class="sxs-lookup"><span data-stu-id="21ce4-349">Fixed objects may cause fragmentation of the heap (because they can't be moved).</span></span> <span data-ttu-id="21ce4-350">基於這個理由，只有在絕對必要時才應該修正物件，而且只會在最短的時間內完成。</span><span class="sxs-lookup"><span data-stu-id="21ce4-350">For that reason, objects should be fixed only when absolutely necessary and then only for the shortest amount of time possible.</span></span>

<span data-ttu-id="21ce4-351">範例</span><span class="sxs-lookup"><span data-stu-id="21ce4-351">The example</span></span>

```csharp
class Test
{
    static int x;
    int y;

    unsafe static void F(int* p) {
        *p = 1;
    }

    static void Main() {
        Test t = new Test();
        int[] a = new int[10];
        unsafe {
            fixed (int* p = &x) F(p);
            fixed (int* p = &t.y) F(p);
            fixed (int* p = &a[0]) F(p);
            fixed (int* p = a) F(p);
        }
    }
}
```

<span data-ttu-id="21ce4-352">示範 `fixed` 語句的數種用法。</span><span class="sxs-lookup"><span data-stu-id="21ce4-352">demonstrates several uses of the `fixed` statement.</span></span> <span data-ttu-id="21ce4-353">第一個語句會修正並取得靜態欄位的位址，第二個語句會修正並取得實例欄位的位址，而第三個語句會修正並取得陣列元素的位址。</span><span class="sxs-lookup"><span data-stu-id="21ce4-353">The first statement fixes and obtains the address of a static field, the second statement fixes and obtains the address of an instance field, and the third statement fixes and obtains the address of an array element.</span></span> <span data-ttu-id="21ce4-354">在每個案例中，使用一般的 `&` 運算子就會發生錯誤，因為變數全都分類為可移動變數。</span><span class="sxs-lookup"><span data-stu-id="21ce4-354">In each case it would have been an error to use the regular `&` operator since the variables are all classified as moveable variables.</span></span>

<span data-ttu-id="21ce4-355">上述範例中的第四個 `fixed` 語句，會產生與第三個類似的結果。</span><span class="sxs-lookup"><span data-stu-id="21ce4-355">The fourth `fixed` statement in the example above produces a similar result to the third.</span></span>

<span data-ttu-id="21ce4-356">這個 `fixed` 語句的範例會使用 `string`：</span><span class="sxs-lookup"><span data-stu-id="21ce4-356">This example of the `fixed` statement uses `string`:</span></span>

```csharp
class Test
{
    static string name = "xx";

    unsafe static void F(char* p) {
        for (int i = 0; p[i] != '\0'; ++i)
            Console.WriteLine(p[i]);
    }

    static void Main() {
        unsafe {
            fixed (char* p = name) F(p);
            fixed (char* p = "xx") F(p);
        }
    }
}
```

<span data-ttu-id="21ce4-357">在一維陣列的 unsafe 內容陣列元素中，會以遞增的索引順序儲存，從索引 `0` 開始，並以索引 `Length - 1` 結束。</span><span class="sxs-lookup"><span data-stu-id="21ce4-357">In an unsafe context array elements of single-dimensional arrays are stored in increasing index order, starting with index `0` and ending with index `Length - 1`.</span></span> <span data-ttu-id="21ce4-358">若為多維陣列，會儲存陣列元素，以便先增加最右邊維度的索引，然後再將下一個左維度放在左側。</span><span class="sxs-lookup"><span data-stu-id="21ce4-358">For multi-dimensional arrays, array elements are stored such that the indices of the rightmost dimension are increased first, then the next left dimension, and so on to the left.</span></span> <span data-ttu-id="21ce4-359">在 `fixed` 語句中，取得 `p` 的指標至陣列實例 `a`，指標值範圍從 `p` 到 `p + a.Length - 1` 表示陣列中元素的位址。</span><span class="sxs-lookup"><span data-stu-id="21ce4-359">Within a `fixed` statement that obtains a pointer `p` to an array instance `a`, the pointer values ranging from `p` to `p + a.Length - 1` represent addresses of the elements in the array.</span></span> <span data-ttu-id="21ce4-360">同樣地，範圍從 `p[0]` 到 `p[a.Length - 1]` 的變數則代表實際的陣列元素。</span><span class="sxs-lookup"><span data-stu-id="21ce4-360">Likewise, the variables ranging from `p[0]` to `p[a.Length - 1]` represent the actual array elements.</span></span> <span data-ttu-id="21ce4-361">假設陣列的儲存方式，我們可以將任何維度的陣列視為線性。</span><span class="sxs-lookup"><span data-stu-id="21ce4-361">Given the way in which arrays are stored, we can treat an array of any dimension as though it were linear.</span></span>

<span data-ttu-id="21ce4-362">例如:</span><span class="sxs-lookup"><span data-stu-id="21ce4-362">For example:</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        int[,,] a = new int[2,3,4];
        unsafe {
            fixed (int* p = a) {
                for (int i = 0; i < a.Length; ++i)    // treat as linear
                    p[i] = i;
            }
        }

        for (int i = 0; i < 2; ++i)
            for (int j = 0; j < 3; ++j) {
                for (int k = 0; k < 4; ++k)
                    Console.Write("[{0},{1},{2}] = {3,2} ", i, j, k, a[i,j,k]);
                Console.WriteLine();
            }
    }
}
```

<span data-ttu-id="21ce4-363">這會產生輸出：</span><span class="sxs-lookup"><span data-stu-id="21ce4-363">which produces the output:</span></span>

```console
[0,0,0] =  0 [0,0,1] =  1 [0,0,2] =  2 [0,0,3] =  3
[0,1,0] =  4 [0,1,1] =  5 [0,1,2] =  6 [0,1,3] =  7
[0,2,0] =  8 [0,2,1] =  9 [0,2,2] = 10 [0,2,3] = 11
[1,0,0] = 12 [1,0,1] = 13 [1,0,2] = 14 [1,0,3] = 15
[1,1,0] = 16 [1,1,1] = 17 [1,1,2] = 18 [1,1,3] = 19
[1,2,0] = 20 [1,2,1] = 21 [1,2,2] = 22 [1,2,3] = 23
```

<span data-ttu-id="21ce4-364">在範例中</span><span class="sxs-lookup"><span data-stu-id="21ce4-364">In the example</span></span>

```csharp
class Test
{
    unsafe static void Fill(int* p, int count, int value) {
        for (; count != 0; count--) *p++ = value;
    }

    static void Main() {
        int[] a = new int[100];
        unsafe {
            fixed (int* p = a) Fill(p, 100, -1);
        }
    }
}
```

<span data-ttu-id="21ce4-365">`fixed` 語句是用來修正陣列，因此其位址可以傳遞至接受指標的方法。</span><span class="sxs-lookup"><span data-stu-id="21ce4-365">a `fixed` statement is used to fix an array so its address can be passed to a method that takes a pointer.</span></span>

<span data-ttu-id="21ce4-366">在下列範例中：</span><span class="sxs-lookup"><span data-stu-id="21ce4-366">In the example:</span></span>

```csharp
unsafe struct Font
{
    public int size;
    public fixed char name[32];
}

class Test
{
    unsafe static void PutString(string s, char* buffer, int bufSize) {
        int len = s.Length;
        if (len > bufSize) len = bufSize;
        for (int i = 0; i < len; i++) buffer[i] = s[i];
        for (int i = len; i < bufSize; i++) buffer[i] = (char)0;
    }

    Font f;

    unsafe static void Main()
    {
        Test test = new Test();
        test.f.size = 10;
        fixed (char* p = test.f.name) {
            PutString("Times New Roman", p, 32);
        }
    }
}
```

<span data-ttu-id="21ce4-367">fixed 語句用來修正結構的固定大小緩衝區，使其位址可以當做指標使用。</span><span class="sxs-lookup"><span data-stu-id="21ce4-367">a fixed statement is used to fix a fixed size buffer of a struct so its address can be used as a pointer.</span></span>

<span data-ttu-id="21ce4-368">藉由修正字串實例所產生的 @no__t 0 值，一律會指向以 null 結束的字串。</span><span class="sxs-lookup"><span data-stu-id="21ce4-368">A `char*` value produced by fixing a string instance always points to a null-terminated string.</span></span> <span data-ttu-id="21ce4-369">在取得指標 `p` 到字串實例 `s` 的 fixed 語句中，指標值的範圍從 `p` 到 `p + s.Length - 1` 代表字串中的字元位址，而指標值 `p + s.Length` 一律指向 null 字元（值為 `'\0'`）的字元。</span><span class="sxs-lookup"><span data-stu-id="21ce4-369">Within a fixed statement that obtains a pointer `p` to a string instance `s`, the pointer values ranging from `p` to `p + s.Length - 1` represent addresses of the characters in the string, and the pointer value `p + s.Length` always points to a null character (the character with value `'\0'`).</span></span>

<span data-ttu-id="21ce4-370">透過固定指標修改 managed 類型的物件，可能會導致未定義的行為。</span><span class="sxs-lookup"><span data-stu-id="21ce4-370">Modifying objects of managed type through fixed pointers can results in undefined behavior.</span></span> <span data-ttu-id="21ce4-371">例如，因為字串是不可變的，所以程式設計人員必須負責確保固定字串的指標所參考的字元不會修改。</span><span class="sxs-lookup"><span data-stu-id="21ce4-371">For example, because strings are immutable, it is the programmer's responsibility to ensure that the characters referenced by a pointer to a fixed string are not modified.</span></span>

<span data-ttu-id="21ce4-372">呼叫需要 "C 樣式" 字串的外部 Api 時，自動 null 終止的字串會特別方便。</span><span class="sxs-lookup"><span data-stu-id="21ce4-372">The automatic null-termination of strings is particularly convenient when calling external APIs that expect "C-style" strings.</span></span> <span data-ttu-id="21ce4-373">不過，請注意，字串實例允許包含 null 字元。</span><span class="sxs-lookup"><span data-stu-id="21ce4-373">Note, however, that a string instance is permitted to contain null characters.</span></span> <span data-ttu-id="21ce4-374">如果有這類 null 字元，當視為以 null 終止的 `char*` 時，字串會顯示為截斷。</span><span class="sxs-lookup"><span data-stu-id="21ce4-374">If such null characters are present, the string will appear truncated when treated as a null-terminated `char*`.</span></span>

## <a name="fixed-size-buffers"></a><span data-ttu-id="21ce4-375">固定大小的緩衝區</span><span class="sxs-lookup"><span data-stu-id="21ce4-375">Fixed size buffers</span></span>

<span data-ttu-id="21ce4-376">固定大小緩衝區是用來將「C 樣式」的內嵌陣列宣告為結構的成員，而且主要適用于與非受控 Api 互動。</span><span class="sxs-lookup"><span data-stu-id="21ce4-376">Fixed size buffers are used to declare "C style" in-line arrays as members of structs, and are primarily useful for interfacing with unmanaged APIs.</span></span>

### <a name="fixed-size-buffer-declarations"></a><span data-ttu-id="21ce4-377">固定大小的緩衝區宣告</span><span class="sxs-lookup"><span data-stu-id="21ce4-377">Fixed size buffer declarations</span></span>

<span data-ttu-id="21ce4-378">***固定大小緩衝區***是一個成員，代表指定類型變數之固定長度緩衝區的儲存區。</span><span class="sxs-lookup"><span data-stu-id="21ce4-378">A ***fixed size buffer*** is a member that represents storage for a fixed length buffer of variables of a given type.</span></span> <span data-ttu-id="21ce4-379">固定大小的緩衝區宣告會引進一或多個指定元素類型的固定大小緩衝區。</span><span class="sxs-lookup"><span data-stu-id="21ce4-379">A fixed size buffer declaration introduces one or more fixed size buffers of a given element type.</span></span> <span data-ttu-id="21ce4-380">只有在結構宣告中才允許固定大小緩衝區，而且只能在不安全的內容中發生（[不安全](unsafe-code.md#unsafe-contexts)的內容）。</span><span class="sxs-lookup"><span data-stu-id="21ce4-380">Fixed size buffers are only permitted in struct declarations and can only occur in unsafe contexts ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span>

```antlr
struct_member_declaration_unsafe
    : fixed_size_buffer_declaration
    ;

fixed_size_buffer_declaration
    : attributes? fixed_size_buffer_modifier* 'fixed' buffer_element_type fixed_size_buffer_declarator+ ';'
    ;

fixed_size_buffer_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'unsafe'
    ;

buffer_element_type
    : type
    ;

fixed_size_buffer_declarator
    : identifier '[' constant_expression ']'
    ;
```

<span data-ttu-id="21ce4-381">固定大小的緩衝區宣告可能包括一組屬性（[屬性](attributes.md)）、一個 @no__t 的修飾[詞（修飾](classes.md#modifiers)詞）、四個存取修飾詞（[型別參數和條件約束](classes.md#type-parameters-and-constraints)）的有效組合，以及一個 `unsafe` 個修飾詞（[Unsafe內容）。](unsafe-code.md#unsafe-contexts)</span><span class="sxs-lookup"><span data-stu-id="21ce4-381">A fixed size buffer declaration may include a set of attributes ([Attributes](attributes.md)), a `new` modifier ([Modifiers](classes.md#modifiers)), a valid combination of the four access modifiers ([Type parameters and constraints](classes.md#type-parameters-and-constraints)) and an `unsafe` modifier ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span> <span data-ttu-id="21ce4-382">屬性和修飾詞適用于固定大小緩衝區宣告所宣告的所有成員。</span><span class="sxs-lookup"><span data-stu-id="21ce4-382">The attributes and modifiers apply to all of the members declared by the fixed size buffer declaration.</span></span> <span data-ttu-id="21ce4-383">在固定大小的緩衝區宣告中多次出現相同的修飾詞時，就會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="21ce4-383">It is an error for the same modifier to appear multiple times in a fixed size buffer declaration.</span></span>

<span data-ttu-id="21ce4-384">固定大小的緩衝區宣告不允許包含 `static` 修飾詞。</span><span class="sxs-lookup"><span data-stu-id="21ce4-384">A fixed size buffer declaration is not permitted to include the `static` modifier.</span></span>

<span data-ttu-id="21ce4-385">固定大小緩衝區宣告的緩衝區元素類型會指定宣告所引進之緩衝區的元素類型。</span><span class="sxs-lookup"><span data-stu-id="21ce4-385">The buffer element type of a fixed size buffer declaration specifies the element type of the buffer(s) introduced by the declaration.</span></span> <span data-ttu-id="21ce4-386">Buffer 元素類型必須是其中一個預先定義的類型 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、0 或 1。</span><span class="sxs-lookup"><span data-stu-id="21ce4-386">The buffer element type must be one of the predefined types `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, or `bool`.</span></span>

<span data-ttu-id="21ce4-387">Buffer 元素類型後面接著固定大小的緩衝區宣告子清單，其中每個宣告子都會引進新的成員。</span><span class="sxs-lookup"><span data-stu-id="21ce4-387">The buffer element type is followed by a list of fixed size buffer declarators, each of which introduces a new member.</span></span> <span data-ttu-id="21ce4-388">固定大小的緩衝區宣告子包含命名成員的識別碼，後面接著以 `[` 和 @no__t 1 標記括住的常數運算式。</span><span class="sxs-lookup"><span data-stu-id="21ce4-388">A fixed size buffer declarator consists of an identifier that names the member, followed by a constant expression enclosed in `[` and `]` tokens.</span></span> <span data-ttu-id="21ce4-389">常數運算式代表該固定大小緩衝區宣告子所引進之成員中的元素數目。</span><span class="sxs-lookup"><span data-stu-id="21ce4-389">The constant expression denotes the number of elements in the member introduced by that fixed size buffer declarator.</span></span> <span data-ttu-id="21ce4-390">常數運算式的類型必須可以隱含地轉換成類型 `int`，而且值必須是非零的正整數。</span><span class="sxs-lookup"><span data-stu-id="21ce4-390">The type of the constant expression must be implicitly convertible to type `int`, and the value must be a non-zero positive integer.</span></span>

<span data-ttu-id="21ce4-391">固定大小緩衝區的元素一定會在記憶體中依序排列。</span><span class="sxs-lookup"><span data-stu-id="21ce4-391">The elements of a fixed size buffer are guaranteed to be laid out sequentially in memory.</span></span>

<span data-ttu-id="21ce4-392">宣告多個固定大小緩衝區的固定大小緩衝區宣告相當於具有相同屬性和元素類型的單一固定大小緩衝區宣告的多個宣告。</span><span class="sxs-lookup"><span data-stu-id="21ce4-392">A fixed size buffer declaration that declares multiple fixed size buffers is equivalent to multiple declarations of a single fixed size buffer declaration with the same attributes, and element types.</span></span> <span data-ttu-id="21ce4-393">例如：</span><span class="sxs-lookup"><span data-stu-id="21ce4-393">For example</span></span>

```csharp
unsafe struct A
{
   public fixed int x[5], y[10], z[100];
}
```

<span data-ttu-id="21ce4-394">相當於</span><span class="sxs-lookup"><span data-stu-id="21ce4-394">is equivalent to</span></span>

```csharp
unsafe struct A
{
   public fixed int x[5];
   public fixed int y[10];
   public fixed int z[100];
}
```

### <a name="fixed-size-buffers-in-expressions"></a><span data-ttu-id="21ce4-395">運算式中的固定大小緩衝區</span><span class="sxs-lookup"><span data-stu-id="21ce4-395">Fixed size buffers in expressions</span></span>

<span data-ttu-id="21ce4-396">固定大小緩衝區成員的成員查閱（[運算子](expressions.md#operators)）會繼續與欄位的成員查閱相同。</span><span class="sxs-lookup"><span data-stu-id="21ce4-396">Member lookup ([Operators](expressions.md#operators)) of a fixed size buffer member proceeds exactly like member lookup of a field.</span></span>

<span data-ttu-id="21ce4-397">您可以使用*simple_name* （[型別推斷](expressions.md#type-inference)）或*member_access* （動態多載[解析的編譯時間檢查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)），在運算式中參考固定大小的緩衝區。</span><span class="sxs-lookup"><span data-stu-id="21ce4-397">A fixed size buffer can be referenced in an expression using a *simple_name* ([Type inference](expressions.md#type-inference)) or a *member_access* ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span>

<span data-ttu-id="21ce4-398">當固定大小的緩衝區成員當做簡單名稱來參考時，其效果會與表單 `this.I` 的成員存取相同，其中 `I` 是固定大小的緩衝區成員。</span><span class="sxs-lookup"><span data-stu-id="21ce4-398">When a fixed size buffer member is referenced as a simple name, the effect is the same as a member access of the form `this.I`, where `I` is the fixed size buffer member.</span></span>

<span data-ttu-id="21ce4-399">在表單 `E.I` 的成員存取中，如果 `E` 是結構類型，而且該結構類型中的成員查閱 `I` 識別固定大小成員，則會評估 `E.I`，其分類方式如下：</span><span class="sxs-lookup"><span data-stu-id="21ce4-399">In a member access of the form `E.I`, if `E` is of a struct type and a member lookup of `I` in that struct type identifies a fixed size member, then `E.I` is evaluated an classified as follows:</span></span>

*  <span data-ttu-id="21ce4-400">如果不安全的內容中出現運算式 `E.I`，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="21ce4-400">If the expression `E.I` does not occur in an unsafe context, a compile-time error occurs.</span></span>
*  <span data-ttu-id="21ce4-401">如果 `E` 分類為值，則會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="21ce4-401">If `E` is classified as a value, a compile-time error occurs.</span></span>
*  <span data-ttu-id="21ce4-402">否則，如果 `E` 是可移動變數（[固定和可移動的](unsafe-code.md#fixed-and-moveable-variables)變數），而且運算式 `E.I` 不是*fixed_pointer_initializer* （[fixed 語句](unsafe-code.md#the-fixed-statement)），就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="21ce4-402">Otherwise, if `E` is a moveable variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)) and the expression `E.I` is not a *fixed_pointer_initializer* ([The fixed statement](unsafe-code.md#the-fixed-statement)), a compile-time error occurs.</span></span>
*  <span data-ttu-id="21ce4-403">否則，`E` 會參考固定的變數，而運算式的結果會是在 `E` 中，`I` 之固定大小緩衝區成員的第一個元素的指標。</span><span class="sxs-lookup"><span data-stu-id="21ce4-403">Otherwise, `E` references a fixed variable and the result of the expression is a pointer to the first element of the fixed size buffer member `I` in `E`.</span></span> <span data-ttu-id="21ce4-404">結果的類型為 `S*`，其中 `S` 是 `I` 的元素類型，且分類為值。</span><span class="sxs-lookup"><span data-stu-id="21ce4-404">The result is of type `S*`, where `S` is the element type of `I`, and is classified as a value.</span></span>

<span data-ttu-id="21ce4-405">您可以使用第一個元素的指標作業來存取固定大小緩衝區的後續元素。</span><span class="sxs-lookup"><span data-stu-id="21ce4-405">The subsequent elements of the fixed size buffer can be accessed using pointer operations from the first element.</span></span> <span data-ttu-id="21ce4-406">不同于陣列的存取權，對固定大小緩衝區的元素存取是不安全的作業，而且不會進行範圍檢查。</span><span class="sxs-lookup"><span data-stu-id="21ce4-406">Unlike access to arrays, access to the elements of a fixed size buffer is an unsafe operation and is not range checked.</span></span>

<span data-ttu-id="21ce4-407">下列範例會宣告並使用具有固定大小緩衝區成員的結構。</span><span class="sxs-lookup"><span data-stu-id="21ce4-407">The following example declares and uses a struct with a fixed size buffer member.</span></span>

```csharp
unsafe struct Font
{
    public int size;
    public fixed char name[32];
}

class Test
{
    unsafe static void PutString(string s, char* buffer, int bufSize) {
        int len = s.Length;
        if (len > bufSize) len = bufSize;
        for (int i = 0; i < len; i++) buffer[i] = s[i];
        for (int i = len; i < bufSize; i++) buffer[i] = (char)0;
    }

    unsafe static void Main()
    {
        Font f;
        f.size = 10;
        PutString("Times New Roman", f.name, 32);
    }
}
```

### <a name="definite-assignment-checking"></a><span data-ttu-id="21ce4-408">明確的指派檢查</span><span class="sxs-lookup"><span data-stu-id="21ce4-408">Definite assignment checking</span></span>

<span data-ttu-id="21ce4-409">固定大小緩衝區不受限於明確的指派檢查（[明確指派](variables.md#definite-assignment)），而且會忽略固定大小的緩衝區成員，以用於結構類型變數的明確指派檢查。</span><span class="sxs-lookup"><span data-stu-id="21ce4-409">Fixed size buffers are not subject to definite assignment checking ([Definite assignment](variables.md#definite-assignment)), and fixed size buffer members are ignored for purposes of definite assignment checking of struct type variables.</span></span>

<span data-ttu-id="21ce4-410">當固定大小緩衝區成員的最外層包含結構變數為靜態變數、類別實例的執行個體變數或陣列元素時，固定大小緩衝區的元素會自動初始化為其預設值（預設值）[值](variables.md#default-values)）。</span><span class="sxs-lookup"><span data-stu-id="21ce4-410">When the outermost containing struct variable of a fixed size buffer member is a static variable, an instance variable of a class instance, or an array element, the elements of the fixed size buffer are automatically initialized to their default values ([Default values](variables.md#default-values)).</span></span> <span data-ttu-id="21ce4-411">在所有其他情況下，固定大小緩衝區的初始內容是未定義的。</span><span class="sxs-lookup"><span data-stu-id="21ce4-411">In all other cases, the initial content of a fixed size buffer is undefined.</span></span>

## <a name="stack-allocation"></a><span data-ttu-id="21ce4-412">堆疊配置</span><span class="sxs-lookup"><span data-stu-id="21ce4-412">Stack allocation</span></span>

<span data-ttu-id="21ce4-413">在不安全的內容中，本機變數宣告（[區域變數](statements.md#local-variable-declarations)宣告）可能包含從呼叫堆疊配置記憶體的堆疊配置初始化運算式。</span><span class="sxs-lookup"><span data-stu-id="21ce4-413">In an unsafe context, a local variable declaration ([Local variable declarations](statements.md#local-variable-declarations)) may include a stack allocation initializer which allocates memory from the call stack.</span></span>

```antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

<span data-ttu-id="21ce4-414">*Unmanaged_type*會指出將儲存在新配置位置的專案類型，而*運算式*會指出這些專案的數目。</span><span class="sxs-lookup"><span data-stu-id="21ce4-414">The *unmanaged_type* indicates the type of the items that will be stored in the newly allocated location, and the *expression* indicates the number of these items.</span></span> <span data-ttu-id="21ce4-415">結合在一起，即可指定所需的配置大小。</span><span class="sxs-lookup"><span data-stu-id="21ce4-415">Taken together, these specify the required allocation size.</span></span> <span data-ttu-id="21ce4-416">因為堆疊配置的大小不能為負數，所以會發生編譯時期錯誤，將專案數目指定為評估為負值的*constant_expression* 。</span><span class="sxs-lookup"><span data-stu-id="21ce4-416">Since the size of a stack allocation cannot be negative, it is a compile-time error to specify the number of items as a *constant_expression* that evaluates to a negative value.</span></span>

<span data-ttu-id="21ce4-417">格式為 `stackalloc T[E]` 的堆疊配置初始化運算式需要 `T` 為非受控類型（[指標類型](unsafe-code.md#pointer-types)），而 `E` 則為類型 `int` 的運算式。</span><span class="sxs-lookup"><span data-stu-id="21ce4-417">A stack allocation initializer of the form `stackalloc T[E]` requires `T` to be an unmanaged type ([Pointer types](unsafe-code.md#pointer-types)) and `E` to be an expression of type `int`.</span></span> <span data-ttu-id="21ce4-418">結構會從呼叫堆疊配置 `E * sizeof(T)` 個位元組，並將類型 `T*` 的指標傳回至新配置的區塊。</span><span class="sxs-lookup"><span data-stu-id="21ce4-418">The construct allocates `E * sizeof(T)` bytes from the call stack and returns a pointer, of type `T*`, to the newly allocated block.</span></span> <span data-ttu-id="21ce4-419">如果 `E` 是負值，則行為是未定義的。</span><span class="sxs-lookup"><span data-stu-id="21ce4-419">If `E` is a negative value, then the behavior is undefined.</span></span> <span data-ttu-id="21ce4-420">如果 `E` 為零，則不會進行任何配置，而傳回的指標會定義為執行。</span><span class="sxs-lookup"><span data-stu-id="21ce4-420">If `E` is zero, then no allocation is made, and the pointer returned is implementation-defined.</span></span> <span data-ttu-id="21ce4-421">如果沒有足夠的記憶體可配置指定大小的區塊，則會擲回 @no__t 0。</span><span class="sxs-lookup"><span data-stu-id="21ce4-421">If there is not enough memory available to allocate a block of the given size, a `System.StackOverflowException` is thrown.</span></span>

<span data-ttu-id="21ce4-422">新配置記憶體的內容尚未被定義。</span><span class="sxs-lookup"><span data-stu-id="21ce4-422">The content of the newly allocated memory is undefined.</span></span>

<span data-ttu-id="21ce4-423">@No__t-0 或 @no__t 1 區塊（[try 語句](statements.md#the-try-statement)）中不允許堆疊配置初始化運算式。</span><span class="sxs-lookup"><span data-stu-id="21ce4-423">Stack allocation initializers are not permitted in `catch` or `finally` blocks ([The try statement](statements.md#the-try-statement)).</span></span>

<span data-ttu-id="21ce4-424">沒有任何方法可以明確釋放使用 `stackalloc` 所配置的記憶體。</span><span class="sxs-lookup"><span data-stu-id="21ce4-424">There is no way to explicitly free memory allocated using `stackalloc`.</span></span> <span data-ttu-id="21ce4-425">在函式成員執行期間建立的所有堆疊配置記憶體區塊，會在該函式成員傳回時自動捨棄。</span><span class="sxs-lookup"><span data-stu-id="21ce4-425">All stack allocated memory blocks created during the execution of a function member are automatically discarded when that function member returns.</span></span> <span data-ttu-id="21ce4-426">這會對應到 `alloca` 函式，這是通常在 C 和C++實作為中找到的延伸模組。</span><span class="sxs-lookup"><span data-stu-id="21ce4-426">This corresponds to the `alloca` function, an extension commonly found in C and C++ implementations.</span></span>

<span data-ttu-id="21ce4-427">在範例中</span><span class="sxs-lookup"><span data-stu-id="21ce4-427">In the example</span></span>

```csharp
using System;

class Test
{
    static string IntToString(int value) {
        int n = value >= 0? value: -value;
        unsafe {
            char* buffer = stackalloc char[16];
            char* p = buffer + 16;
            do {
                *--p = (char)(n % 10 + '0');
                n /= 10;
            } while (n != 0);
            if (value < 0) *--p = '-';
            return new string(p, 0, (int)(buffer + 16 - p));
        }
    }

    static void Main() {
        Console.WriteLine(IntToString(12345));
        Console.WriteLine(IntToString(-999));
    }
}
```

<span data-ttu-id="21ce4-428">`IntToString` 方法中會使用 @no__t 0 初始化運算式，在堆疊上配置16個字元的緩衝區。</span><span class="sxs-lookup"><span data-stu-id="21ce4-428">a `stackalloc` initializer is used in the `IntToString` method to allocate a buffer of 16 characters on the stack.</span></span> <span data-ttu-id="21ce4-429">當方法傳回時，會自動捨棄緩衝區。</span><span class="sxs-lookup"><span data-stu-id="21ce4-429">The buffer is automatically discarded when the method returns.</span></span>

## <a name="dynamic-memory-allocation"></a><span data-ttu-id="21ce4-430">動態記憶體配置</span><span class="sxs-lookup"><span data-stu-id="21ce4-430">Dynamic memory allocation</span></span>

<span data-ttu-id="21ce4-431">除了 `stackalloc` 運算子以外， C#不會提供任何預先定義的結構來管理非垃圾收集的記憶體。</span><span class="sxs-lookup"><span data-stu-id="21ce4-431">Except for the `stackalloc` operator, C# provides no predefined constructs for managing non-garbage collected memory.</span></span> <span data-ttu-id="21ce4-432">這類服務通常是透過支援類別庫來提供，或直接從基礎作業系統匯入。</span><span class="sxs-lookup"><span data-stu-id="21ce4-432">Such services are typically provided by supporting class libraries or imported directly from the underlying operating system.</span></span> <span data-ttu-id="21ce4-433">例如，下列的 `Memory` 類別說明如何從下列來源C#存取基礎作業系統的堆積功能：</span><span class="sxs-lookup"><span data-stu-id="21ce4-433">For example, the `Memory` class below illustrates how the heap functions of an underlying operating system might be accessed from C#:</span></span>

```csharp
using System;
using System.Runtime.InteropServices;

public static unsafe class Memory
{
    // Handle for the process heap. This handle is used in all calls to the
    // HeapXXX APIs in the methods below.
    private static readonly IntPtr s_heap = GetProcessHeap();

    // Allocates a memory block of the given size. The allocated memory is
    // automatically initialized to zero.
    public static void* Alloc(int size)
    {
        void* result = HeapAlloc(s_heap, HEAP_ZERO_MEMORY, (UIntPtr)size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Copies count bytes from src to dst. The source and destination
    // blocks are permitted to overlap.
    public static void Copy(void* src, void* dst, int count)
    {
        byte* ps = (byte*)src;
        byte* pd = (byte*)dst;
        if (ps > pd)
        {
            for (; count != 0; count--) *pd++ = *ps++;
        }
        else if (ps < pd)
        {
            for (ps += count, pd += count; count != 0; count--) *--pd = *--ps;
        }
    }

    // Frees a memory block.
    public static void Free(void* block)
    {
        if (!HeapFree(s_heap, 0, block)) throw new InvalidOperationException();
    }

    // Re-allocates a memory block. If the reallocation request is for a
    // larger size, the additional region of memory is automatically
    // initialized to zero.
    public static void* ReAlloc(void* block, int size)
    {
        void* result = HeapReAlloc(s_heap, HEAP_ZERO_MEMORY, block, (UIntPtr)size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Returns the size of a memory block.
    public static int SizeOf(void* block)
    {
        int result = (int)HeapSize(s_heap, 0, block);
        if (result == -1) throw new InvalidOperationException();
        return result;
    }

    // Heap API flags
    private const int HEAP_ZERO_MEMORY = 0x00000008;

    // Heap API functions
    [DllImport("kernel32")]
    private static extern IntPtr GetProcessHeap();

    [DllImport("kernel32")]
    private static extern void* HeapAlloc(IntPtr hHeap, int flags, UIntPtr size);

    [DllImport("kernel32")]
    private static extern bool HeapFree(IntPtr hHeap, int flags, void* block);

    [DllImport("kernel32")]
    private static extern void* HeapReAlloc(IntPtr hHeap, int flags, void* block, UIntPtr size);

    [DllImport("kernel32")]
    private static extern UIntPtr HeapSize(IntPtr hHeap, int flags, void* block);
}
```

<span data-ttu-id="21ce4-434">以下提供使用 `Memory` 類別的範例：</span><span class="sxs-lookup"><span data-stu-id="21ce4-434">An example that uses the `Memory` class is given below:</span></span>

```csharp
class Test
{
    static unsafe void Main()
    {
        byte* buffer = null;
        try
        {
            const int Size = 256;
            buffer = (byte*)Memory.Alloc(Size);
            for (int i = 0; i < Size; i++) buffer[i] = (byte)i;
            byte[] array = new byte[Size];
            fixed (byte* p = array) Memory.Copy(buffer, p, Size);
            for (int i = 0; i < Size; i++) Console.WriteLine(array[i]);
        }
        finally
        {
            if (buffer != null) Memory.Free(buffer);
        }
    }
}
```

<span data-ttu-id="21ce4-435">此範例會透過 `Memory.Alloc` 配置256位元組的記憶體，並將值從0增加到255的記憶體區塊初始化。</span><span class="sxs-lookup"><span data-stu-id="21ce4-435">The example allocates 256 bytes of memory through `Memory.Alloc` and initializes the memory block with values increasing from 0 to 255.</span></span> <span data-ttu-id="21ce4-436">然後，它會配置256元素位元組陣列，並使用 `Memory.Copy` 將記憶體區塊的內容複寫到位元組陣列。</span><span class="sxs-lookup"><span data-stu-id="21ce4-436">It then allocates a 256 element byte array and uses `Memory.Copy` to copy the contents of the memory block into the byte array.</span></span> <span data-ttu-id="21ce4-437">最後，記憶體區塊會使用 `Memory.Free` 來釋放，而位元組陣列的內容則會在主控台上輸出。</span><span class="sxs-lookup"><span data-stu-id="21ce4-437">Finally, the memory block is freed using `Memory.Free` and the contents of the byte array are output on the console.</span></span>
