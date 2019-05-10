---
ms.openlocfilehash: 90001cf3d48f216787fc65e59166ec57c5d0ca34
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488806"
---
# <a name="unsafe-code"></a><span data-ttu-id="5c92c-101">Unsafe 程式碼</span><span class="sxs-lookup"><span data-stu-id="5c92c-101">Unsafe code</span></span>

<span data-ttu-id="5c92c-102">核心C#的語言，定義在前述章節中，與不同值得注意的是 C 和C++中的指標其省略，做為資料類型。</span><span class="sxs-lookup"><span data-stu-id="5c92c-102">The core C# language, as defined in the preceding chapters, differs notably from C and C++ in its omission of pointers as a data type.</span></span> <span data-ttu-id="5c92c-103">相反地，C# 提供參考，並且能夠建立由記憶體回收行程所管理的物件。</span><span class="sxs-lookup"><span data-stu-id="5c92c-103">Instead, C# provides references and the ability to create objects that are managed by a garbage collector.</span></span> <span data-ttu-id="5c92c-104">這項設計，搭配其他功能，讓C#更安全語言比 C 或C++。</span><span class="sxs-lookup"><span data-stu-id="5c92c-104">This design, coupled with other features, makes C# a much safer language than C or C++.</span></span> <span data-ttu-id="5c92c-105">在 core C# 語言中不只可以有未初始化的變數、 「 懸空 」 的指標或索引的陣列，其界限外的運算式。</span><span class="sxs-lookup"><span data-stu-id="5c92c-105">In the core C# language it is simply not possible to have an uninitialized variable, a "dangling" pointer, or an expression that indexes an array beyond its bounds.</span></span> <span data-ttu-id="5c92c-106">錯誤種類頭痛的 C 和C++因此會刪除程式。</span><span class="sxs-lookup"><span data-stu-id="5c92c-106">Whole categories of bugs that routinely plague C and C++ programs are thus eliminated.</span></span>

<span data-ttu-id="5c92c-107">幾乎每個指標類型時建構在 C 或C++中有參考型別對應C#，然而，有很多情況下，指標類型的存取權是不可或缺。</span><span class="sxs-lookup"><span data-stu-id="5c92c-107">While practically every pointer type construct in C or C++ has a reference type counterpart in C#, nonetheless, there are situations where access to pointer types becomes a necessity.</span></span> <span data-ttu-id="5c92c-108">例如，與基礎作業系統互動、 存取記憶體對應的裝置，或實作關鍵時間的演算法不可能可行或切合實際，而不需要存取指標。</span><span class="sxs-lookup"><span data-stu-id="5c92c-108">For example, interfacing with the underlying operating system, accessing a memory-mapped device, or implementing a time-critical algorithm may not be possible or practical without access to pointers.</span></span> <span data-ttu-id="5c92c-109">若要解決這項需求，C# 讓您能夠撰寫***unsafe 程式碼***。</span><span class="sxs-lookup"><span data-stu-id="5c92c-109">To address this need, C# provides the ability to write ***unsafe code***.</span></span>

<span data-ttu-id="5c92c-110">Unsafe 程式碼中就可以宣告並對指標執行指標與整數類資料類型，來取得變數的位址之間的轉換等等。</span><span class="sxs-lookup"><span data-stu-id="5c92c-110">In unsafe code it is possible to declare and operate on pointers, to perform conversions between pointers and integral types, to take the address of variables, and so forth.</span></span> <span data-ttu-id="5c92c-111">就某方面來說，撰寫不安全的程式碼很像是撰寫 C# 程式中的 C 程式碼一樣。</span><span class="sxs-lookup"><span data-stu-id="5c92c-111">In a sense, writing unsafe code is much like writing C code within a C# program.</span></span>

<span data-ttu-id="5c92c-112">Unsafe 程式碼實際上是 「 安全 」 的功能從開發人員和使用者的觀點來看。</span><span class="sxs-lookup"><span data-stu-id="5c92c-112">Unsafe code is in fact a "safe" feature from the perspective of both developers and users.</span></span> <span data-ttu-id="5c92c-113">Unsafe 程式碼必須明確標示以修飾詞`unsafe`，因此開發人員不可能會不小心，不安全的功能使用，並確保無法在不受信任的環境中執行不安全的程式碼如何執行引擎。</span><span class="sxs-lookup"><span data-stu-id="5c92c-113">Unsafe code must be clearly marked with the modifier `unsafe`, so developers can't possibly use unsafe features accidentally, and the execution engine works to ensure that unsafe code cannot be executed in an untrusted environment.</span></span>

## <a name="unsafe-contexts"></a><span data-ttu-id="5c92c-114">不安全的內容</span><span class="sxs-lookup"><span data-stu-id="5c92c-114">Unsafe contexts</span></span>

<span data-ttu-id="5c92c-115">C# 的不安全的功能是僅適用於不安全的內容。</span><span class="sxs-lookup"><span data-stu-id="5c92c-115">The unsafe features of C# are available only in unsafe contexts.</span></span> <span data-ttu-id="5c92c-116">不安全的內容以包含引入`unsafe`修飾詞宣告的型別或成員，或採用*unsafe_statement*:</span><span class="sxs-lookup"><span data-stu-id="5c92c-116">An unsafe context is introduced by including an `unsafe` modifier in the declaration of a type or member, or by employing an *unsafe_statement*:</span></span>

*  <span data-ttu-id="5c92c-117">類別、 結構、 介面或委派的宣告可能包含`unsafe`修飾詞，在其中案例 （包括類別、 結構或介面的主體） 該型別宣告的整個文字範圍被視為不安全的內容。</span><span class="sxs-lookup"><span data-stu-id="5c92c-117">A declaration of a class, struct, interface, or delegate may include an `unsafe` modifier, in which case the entire textual extent of that type declaration (including the body of the class, struct, or interface) is considered an unsafe context.</span></span>
*  <span data-ttu-id="5c92c-118">欄位、 方法、 屬性、 事件、 索引子、 運算子、 執行個體建構函式、 解構函式或靜態建構函式的宣告可能包含`unsafe`修飾詞，在其中案例該成員宣告的整個文字範圍被視為不安全內容。</span><span class="sxs-lookup"><span data-stu-id="5c92c-118">A declaration of a field, method, property, event, indexer, operator, instance constructor, destructor, or static constructor may include an `unsafe` modifier, in which case the entire textual extent of that member declaration is considered an unsafe context.</span></span>
*  <span data-ttu-id="5c92c-119">*Unsafe_statement*可讓您使用不安全的內容內*區塊*。</span><span class="sxs-lookup"><span data-stu-id="5c92c-119">An *unsafe_statement* enables the use of an unsafe context within a *block*.</span></span> <span data-ttu-id="5c92c-120">整個文字範圍相關聯*區塊*會被視為不安全的內容。</span><span class="sxs-lookup"><span data-stu-id="5c92c-120">The entire textual extent of the associated *block* is considered an unsafe context.</span></span>

<span data-ttu-id="5c92c-121">相關聯的文法生產如下所示。</span><span class="sxs-lookup"><span data-stu-id="5c92c-121">The associated grammar productions are shown below.</span></span>

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

<span data-ttu-id="5c92c-122">在範例</span><span class="sxs-lookup"><span data-stu-id="5c92c-122">In the example</span></span>

```csharp
public unsafe struct Node
{
    public int Value;
    public Node* Left;
    public Node* Right;
}
```

<span data-ttu-id="5c92c-123">`unsafe`結構宣告中指定的修飾詞會造成在結構宣告，變得不安全的內容的整個文字範圍。</span><span class="sxs-lookup"><span data-stu-id="5c92c-123">the `unsafe` modifier specified in the struct declaration causes the entire textual extent of the struct declaration to become an unsafe context.</span></span> <span data-ttu-id="5c92c-124">因此，就可以宣告`Left`和`Right`是指標類型的欄位。</span><span class="sxs-lookup"><span data-stu-id="5c92c-124">Thus, it is possible to declare the `Left` and `Right` fields to be of a pointer type.</span></span> <span data-ttu-id="5c92c-125">也可以撰寫上面的範例</span><span class="sxs-lookup"><span data-stu-id="5c92c-125">The example above could also be written</span></span>

```csharp
public struct Node
{
    public int Value;
    public unsafe Node* Left;
    public unsafe Node* Right;
}
```

<span data-ttu-id="5c92c-126">在這裡，`unsafe`中的欄位宣告修飾詞會造成這些宣告，才會被視為不安全的內容。</span><span class="sxs-lookup"><span data-stu-id="5c92c-126">Here, the `unsafe` modifiers in the field declarations cause those declarations to be considered unsafe contexts.</span></span>

<span data-ttu-id="5c92c-127">建立不安全的內容，因此允許使用指標類型以外`unsafe`修飾詞都不會影響類型或成員。</span><span class="sxs-lookup"><span data-stu-id="5c92c-127">Other than establishing an unsafe context, thus permitting the use of pointer types, the `unsafe` modifier has no effect on a type or a member.</span></span> <span data-ttu-id="5c92c-128">在範例</span><span class="sxs-lookup"><span data-stu-id="5c92c-128">In the example</span></span>

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

<span data-ttu-id="5c92c-129">`unsafe`修飾詞`F`方法中的`A`只會導致文字範圍`F`變得不安全的內容可以在其中使用不安全的語言功能。</span><span class="sxs-lookup"><span data-stu-id="5c92c-129">the `unsafe` modifier on the `F` method in `A` simply causes the textual extent of `F` to become an unsafe context in which the unsafe features of the language can be used.</span></span> <span data-ttu-id="5c92c-130">中的覆寫`F`中`B`，則不需要重新指定`unsafe`修飾詞，除非，當然`F`中的方法`B`本身必須存取不安全的功能。</span><span class="sxs-lookup"><span data-stu-id="5c92c-130">In the override of `F` in `B`, there is no need to re-specify the `unsafe` modifier -- unless, of course, the `F` method in `B` itself needs access to unsafe features.</span></span>

<span data-ttu-id="5c92c-131">指標類型是方法簽章的一部分時，情況就會稍有不同</span><span class="sxs-lookup"><span data-stu-id="5c92c-131">The situation is slightly different when a pointer type is part of the method's signature</span></span>

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

<span data-ttu-id="5c92c-132">在這裡，因為`F`的簽章包含指標類型，它只能寫入 unsafe 內容中。</span><span class="sxs-lookup"><span data-stu-id="5c92c-132">Here, because `F`'s signature includes a pointer type, it can only be written in an unsafe context.</span></span> <span data-ttu-id="5c92c-133">不過，不安全的內容可以藉由任一整個類別不安全，在此情況下，在引入`A`，或包含`unsafe`修飾詞在方法宣告中，在此情況下，在`B`。</span><span class="sxs-lookup"><span data-stu-id="5c92c-133">However, the unsafe context can be introduced by either making the entire class unsafe, as is the case in `A`, or by including an `unsafe` modifier in the method declaration, as is the case in `B`.</span></span>

## <a name="pointer-types"></a><span data-ttu-id="5c92c-134">指標類型</span><span class="sxs-lookup"><span data-stu-id="5c92c-134">Pointer types</span></span>

<span data-ttu-id="5c92c-135">在 unsafe 內容中，*型別*([型別](types.md)) 可能*pointer_type* ，以及*value_type*或*reference_type*.</span><span class="sxs-lookup"><span data-stu-id="5c92c-135">In an unsafe context, a *type* ([Types](types.md)) may be a *pointer_type* as well as a *value_type* or a *reference_type*.</span></span> <span data-ttu-id="5c92c-136">不過， *pointer_type*也可用於`typeof`運算式 ([匿名物件建立運算式](expressions.md#anonymous-object-creation-expressions)) 不安全的內容外，這類使用量不是不安全。</span><span class="sxs-lookup"><span data-stu-id="5c92c-136">However, a *pointer_type* may also be used in a `typeof` expression ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) outside of an unsafe context as such usage is not unsafe.</span></span>

```antlr
type_unsafe
    : pointer_type
    ;
```

<span data-ttu-id="5c92c-137">A *pointer_type*寫成*unmanaged_type*或關鍵字`void`，後面接著`*`語彙基元：</span><span class="sxs-lookup"><span data-stu-id="5c92c-137">A *pointer_type* is written as an *unmanaged_type* or the keyword `void`, followed by a `*` token:</span></span>

```antlr
pointer_type
    : unmanaged_type '*'
    | 'void' '*'
    ;

unmanaged_type
    : type
    ;
```

<span data-ttu-id="5c92c-138">前面所指定的型別`*`指標呼叫的型別***參照型別***與指標類型。</span><span class="sxs-lookup"><span data-stu-id="5c92c-138">The type specified before the `*` in a pointer type is called the ***referent type*** of the pointer type.</span></span> <span data-ttu-id="5c92c-139">它代表指標類型的值所指向變數的型別。</span><span class="sxs-lookup"><span data-stu-id="5c92c-139">It represents the type of the variable to which a value of the pointer type points.</span></span>

<span data-ttu-id="5c92c-140">不同於參考 （參考類型的值），記憶體回收行程不會追蹤指標--記憶體回收行程並不知道指標和它們所指向的資料。</span><span class="sxs-lookup"><span data-stu-id="5c92c-140">Unlike references (values of reference types), pointers are not tracked by the garbage collector -- the garbage collector has no knowledge of pointers and the data to which they point.</span></span> <span data-ttu-id="5c92c-141">針對此原因的指標不允許指向參考或結構，其中包含參考，而指標的參考型別必須是*unmanaged_type*。</span><span class="sxs-lookup"><span data-stu-id="5c92c-141">For this reason a pointer is not permitted to point to a reference or to a struct that contains references, and the referent type of a pointer must be an *unmanaged_type*.</span></span>

<span data-ttu-id="5c92c-142">*Unmanaged_type*是不是任何型別*reference_type*或建構的型別，而且未包含*reference_type*或建構的任何層級的型別欄位巢狀。</span><span class="sxs-lookup"><span data-stu-id="5c92c-142">An *unmanaged_type* is any type that isn't a *reference_type* or constructed type, and doesn't contain *reference_type* or constructed type fields at any level of nesting.</span></span> <span data-ttu-id="5c92c-143">亦即*unmanaged_type*是下列其中之一：</span><span class="sxs-lookup"><span data-stu-id="5c92c-143">In other words, an *unmanaged_type* is one of the following:</span></span>

*  <span data-ttu-id="5c92c-144">`sbyte``byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char`， `float`， `double`， `decimal`，或`bool`。</span><span class="sxs-lookup"><span data-stu-id="5c92c-144">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, or `bool`.</span></span>
*  <span data-ttu-id="5c92c-145">任何*enum_type*。</span><span class="sxs-lookup"><span data-stu-id="5c92c-145">Any *enum_type*.</span></span>
*  <span data-ttu-id="5c92c-146">任何*pointer_type*。</span><span class="sxs-lookup"><span data-stu-id="5c92c-146">Any *pointer_type*.</span></span>
*  <span data-ttu-id="5c92c-147">任何使用者定義*struct_type*中不是建構的類型，並包含的欄位*unmanaged_type*僅。</span><span class="sxs-lookup"><span data-stu-id="5c92c-147">Any user-defined *struct_type* that is not a constructed type and contains fields of *unmanaged_type*s only.</span></span>

<span data-ttu-id="5c92c-148">混合的指標和參考的直覺式規則是允許包含指標、 參考 （物件），但不是允許包含參考的指標的參考。</span><span class="sxs-lookup"><span data-stu-id="5c92c-148">The intuitive rule for mixing of pointers and references is that referents of references (objects) are permitted to contain pointers, but referents of pointers are not permitted to contain references.</span></span>

<span data-ttu-id="5c92c-149">指標類型的一些範例會提供下表中：</span><span class="sxs-lookup"><span data-stu-id="5c92c-149">Some examples of pointer types are given in the table below:</span></span>

| <span data-ttu-id="5c92c-150">__範例__</span><span class="sxs-lookup"><span data-stu-id="5c92c-150">__Example__</span></span> | <span data-ttu-id="5c92c-151">__描述__</span><span class="sxs-lookup"><span data-stu-id="5c92c-151">__Description__</span></span>                               |
|-------------|-----------------------------------------------|
| `byte*`     | <span data-ttu-id="5c92c-152">指標 `byte`</span><span class="sxs-lookup"><span data-stu-id="5c92c-152">Pointer to `byte`</span></span>                             |
| `char*`     | <span data-ttu-id="5c92c-153">指標 `char`</span><span class="sxs-lookup"><span data-stu-id="5c92c-153">Pointer to `char`</span></span>                             |
| `int**`     | <span data-ttu-id="5c92c-154">指標的指標 `int`</span><span class="sxs-lookup"><span data-stu-id="5c92c-154">Pointer to pointer to `int`</span></span>                   |
| `int*[]`    | <span data-ttu-id="5c92c-155">單一維度陣列的指標 `int`</span><span class="sxs-lookup"><span data-stu-id="5c92c-155">Single-dimensional array of pointers to `int`</span></span> |
| `void*`     | <span data-ttu-id="5c92c-156">未知的類型指標</span><span class="sxs-lookup"><span data-stu-id="5c92c-156">Pointer to unknown type</span></span>                       |

<span data-ttu-id="5c92c-157">對於給定的實作，所有指標類型必須都具有相同的大小和表示法。</span><span class="sxs-lookup"><span data-stu-id="5c92c-157">For a given implementation, all pointer types must have the same size and representation.</span></span>

<span data-ttu-id="5c92c-158">不同於 C 和C++，當在相同的宣告中宣告多個指標C#`*`基礎型別，以及寫入不為每個指標名稱的前置標點符號。</span><span class="sxs-lookup"><span data-stu-id="5c92c-158">Unlike C and C++, when multiple pointers are declared in the same declaration, in C# the `*` is written along with the underlying type only, not as a prefix punctuator on each pointer name.</span></span> <span data-ttu-id="5c92c-159">例如</span><span class="sxs-lookup"><span data-stu-id="5c92c-159">For example</span></span>

```csharp
int* pi, pj;    // NOT as int *pi, *pj;
```

<span data-ttu-id="5c92c-160">具有類型指標的值`T*`表示的型別變數的位址`T`。</span><span class="sxs-lookup"><span data-stu-id="5c92c-160">The value of a pointer having type `T*` represents the address of a variable of type `T`.</span></span> <span data-ttu-id="5c92c-161">指標間接運算子`*`([指標間接取值](unsafe-code.md#pointer-indirection)) 可用來存取這個變數。</span><span class="sxs-lookup"><span data-stu-id="5c92c-161">The pointer indirection operator `*` ([Pointer indirection](unsafe-code.md#pointer-indirection)) may be used to access this variable.</span></span> <span data-ttu-id="5c92c-162">例如，假設變數`P`型別的`int*`，運算式`*P`代表`int`變數中包含的位址，請參閱`P`。</span><span class="sxs-lookup"><span data-stu-id="5c92c-162">For example, given a variable `P` of type `int*`, the expression `*P` denotes the `int` variable found at the address contained in `P`.</span></span>

<span data-ttu-id="5c92c-163">指標可能是物件參考，例如`null`。</span><span class="sxs-lookup"><span data-stu-id="5c92c-163">Like an object reference, a pointer may be `null`.</span></span> <span data-ttu-id="5c92c-164">間接取值運算子，以套用`null`指標會產生實作定義行為。</span><span class="sxs-lookup"><span data-stu-id="5c92c-164">Applying the indirection operator to a `null` pointer results in implementation-defined behavior.</span></span> <span data-ttu-id="5c92c-165">值的指標`null`由零的所有位元。</span><span class="sxs-lookup"><span data-stu-id="5c92c-165">A pointer with value `null` is represented by all-bits-zero.</span></span>

<span data-ttu-id="5c92c-166">`void*`型別代表未知類型的指標。</span><span class="sxs-lookup"><span data-stu-id="5c92c-166">The `void*` type represents a pointer to an unknown type.</span></span> <span data-ttu-id="5c92c-167">間接取值運算子參照類型是未知的因為無法套用至類型的指標`void*`，也可以執行的任何計算這類的指標。</span><span class="sxs-lookup"><span data-stu-id="5c92c-167">Because the referent type is unknown, the indirection operator cannot be applied to a pointer of type `void*`, nor can any arithmetic be performed on such a pointer.</span></span> <span data-ttu-id="5c92c-168">不過，類型的指標`void*`可轉換成任何其他指標類型 （反之亦然）。</span><span class="sxs-lookup"><span data-stu-id="5c92c-168">However, a pointer of type `void*` can be cast to any other pointer type (and vice versa).</span></span>

<span data-ttu-id="5c92c-169">指標類型是一個不同的類別的型別。</span><span class="sxs-lookup"><span data-stu-id="5c92c-169">Pointer types are a separate category of types.</span></span> <span data-ttu-id="5c92c-170">不同於參考型別和實值型別，指標類型不是繼承自`object`以及指標類型之間的任何轉換存在和`object`。</span><span class="sxs-lookup"><span data-stu-id="5c92c-170">Unlike reference types and value types, pointer types do not inherit from `object` and no conversions exist between pointer types and `object`.</span></span> <span data-ttu-id="5c92c-171">特別是，boxing 和 unboxing ([Boxing 和 unboxing](types.md#boxing-and-unboxing)) 不支援指標。</span><span class="sxs-lookup"><span data-stu-id="5c92c-171">In particular, boxing and unboxing ([Boxing and unboxing](types.md#boxing-and-unboxing)) are not supported for pointers.</span></span> <span data-ttu-id="5c92c-172">不過，在不同的指標類型之間以及指標類型和整數類資料類型之間被可以進行轉換。</span><span class="sxs-lookup"><span data-stu-id="5c92c-172">However, conversions are permitted between different pointer types and between pointer types and the integral types.</span></span> <span data-ttu-id="5c92c-173">這中所述[指標轉換](unsafe-code.md#pointer-conversions)。</span><span class="sxs-lookup"><span data-stu-id="5c92c-173">This is described in [Pointer conversions](unsafe-code.md#pointer-conversions).</span></span>

<span data-ttu-id="5c92c-174">A *pointer_type*不能作為類型引數 ([建構型別](types.md#constructed-types))，與型別推斷 ([型別推斷](expressions.md#type-inference)) 上會有推斷的一般方法呼叫失敗輸入是指標類型的引數。</span><span class="sxs-lookup"><span data-stu-id="5c92c-174">A *pointer_type* cannot be used as a type argument ([Constructed types](types.md#constructed-types)), and type inference ([Type inference](expressions.md#type-inference)) fails on generic method calls that would have inferred a type argument to be a pointer type.</span></span>

<span data-ttu-id="5c92c-175">A *pointer_type*皆可用為 volatile 欄位的型別 ([Volatile 欄位](classes.md#volatile-fields))。</span><span class="sxs-lookup"><span data-stu-id="5c92c-175">A *pointer_type* may be used as the type of a volatile field ([Volatile fields](classes.md#volatile-fields)).</span></span>

<span data-ttu-id="5c92c-176">雖然指標可以當做`ref`或`out`參數，這樣可能會導致未定義的行為，因為指標可能也會設定為指向區域變數已不存在時呼叫的方法傳回時或它的固定的物件用來點，不會再修正。</span><span class="sxs-lookup"><span data-stu-id="5c92c-176">Although pointers can be passed as `ref` or `out` parameters, doing so can cause undefined behavior, since the pointer may well be set to point to a local variable which no longer exists when the called method returns, or the fixed object to which it used to point, is no longer fixed.</span></span> <span data-ttu-id="5c92c-177">例如: </span><span class="sxs-lookup"><span data-stu-id="5c92c-177">For example:</span></span>

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

<span data-ttu-id="5c92c-178">一種方法可以傳回某種類型的值和該類型可以是指標。</span><span class="sxs-lookup"><span data-stu-id="5c92c-178">A method can return a value of some type, and that type can be a pointer.</span></span> <span data-ttu-id="5c92c-179">例如，當指定指標的連續序列`int`s，該序列的項目計數，以及一些其他`int`值，下列方法會傳回該值的地址以該序列中，如果相符，否則傳回`null`:</span><span class="sxs-lookup"><span data-stu-id="5c92c-179">For example, when given a pointer to a contiguous sequence of `int`s, that sequence's element count, and some other `int` value, the following method returns the address of that value in that sequence, if a match occurs; otherwise it returns `null`:</span></span>

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

<span data-ttu-id="5c92c-180">在 unsafe 內容中，數個建構可用來操作指標：</span><span class="sxs-lookup"><span data-stu-id="5c92c-180">In an unsafe context, several constructs are available for operating on pointers:</span></span>

*  <span data-ttu-id="5c92c-181">`*`運算子可用來執行指標間接取值 ([指標間接取值](unsafe-code.md#pointer-indirection))。</span><span class="sxs-lookup"><span data-stu-id="5c92c-181">The `*` operator may be used to perform pointer indirection ([Pointer indirection](unsafe-code.md#pointer-indirection)).</span></span>
*  <span data-ttu-id="5c92c-182">`->`運算子可用來透過指標存取結構的成員 ([指標成員存取](unsafe-code.md#pointer-member-access))。</span><span class="sxs-lookup"><span data-stu-id="5c92c-182">The `->` operator may be used to access a member of a struct through a pointer ([Pointer member access](unsafe-code.md#pointer-member-access)).</span></span>
*  <span data-ttu-id="5c92c-183">`[]`運算子可用來編製索引的指標 ([指標元素存取](unsafe-code.md#pointer-element-access))。</span><span class="sxs-lookup"><span data-stu-id="5c92c-183">The `[]` operator may be used to index a pointer ([Pointer element access](unsafe-code.md#pointer-element-access)).</span></span>
*  <span data-ttu-id="5c92c-184">`&`運算子可用來取得變數位址 ([傳址運算子](unsafe-code.md#the-address-of-operator))。</span><span class="sxs-lookup"><span data-stu-id="5c92c-184">The `&` operator may be used to obtain the address of a variable ([The address-of operator](unsafe-code.md#the-address-of-operator)).</span></span>
*  <span data-ttu-id="5c92c-185">`++`並`--`運算子可能會用來遞增和遞減指標 ([指標遞增和遞減](unsafe-code.md#pointer-increment-and-decrement))。</span><span class="sxs-lookup"><span data-stu-id="5c92c-185">The `++` and `--` operators may be used to increment and decrement pointers ([Pointer increment and decrement](unsafe-code.md#pointer-increment-and-decrement)).</span></span>
*  <span data-ttu-id="5c92c-186">`+`並`-`運算子可用來執行指標算術 ([指標算術](unsafe-code.md#pointer-arithmetic))。</span><span class="sxs-lookup"><span data-stu-id="5c92c-186">The `+` and `-` operators may be used to perform pointer arithmetic ([Pointer arithmetic](unsafe-code.md#pointer-arithmetic)).</span></span>
*  <span data-ttu-id="5c92c-187">`==`， `!=`， `<`， `>`， `<=`，和`=>`運算子可用來比較指標 ([指標比較](unsafe-code.md#pointer-comparison))。</span><span class="sxs-lookup"><span data-stu-id="5c92c-187">The `==`, `!=`, `<`, `>`, `<=`, and `=>` operators may be used to compare pointers ([Pointer comparison](unsafe-code.md#pointer-comparison)).</span></span>
*  <span data-ttu-id="5c92c-188">`stackalloc`運算子可用來配置呼叫堆疊的記憶體 ([固定大小緩衝區](unsafe-code.md#fixed-size-buffers))。</span><span class="sxs-lookup"><span data-stu-id="5c92c-188">The `stackalloc` operator may be used to allocate memory from the call stack ([Fixed size buffers](unsafe-code.md#fixed-size-buffers)).</span></span>
*  <span data-ttu-id="5c92c-189">`fixed`陳述式可用來暫時修正變數，因此您可以取得其位址 ([fixed 陳述式](unsafe-code.md#the-fixed-statement))。</span><span class="sxs-lookup"><span data-stu-id="5c92c-189">The `fixed` statement may be used to temporarily fix a variable so its address can be obtained ([The fixed statement](unsafe-code.md#the-fixed-statement)).</span></span>

## <a name="fixed-and-moveable-variables"></a><span data-ttu-id="5c92c-190">固定和可移動變數</span><span class="sxs-lookup"><span data-stu-id="5c92c-190">Fixed and moveable variables</span></span>

<span data-ttu-id="5c92c-191">傳址運算子 ([傳址運算子](unsafe-code.md#the-address-of-operator)) 和`fixed`陳述式 ([fixed 陳述式](unsafe-code.md#the-fixed-statement)) 分成兩大類別的變數：***已修正變數***並***可移動的變數***。</span><span class="sxs-lookup"><span data-stu-id="5c92c-191">The address-of operator ([The address-of operator](unsafe-code.md#the-address-of-operator)) and the `fixed` statement ([The fixed statement](unsafe-code.md#the-fixed-statement)) divide variables into two categories: ***Fixed variables*** and ***moveable variables***.</span></span>

<span data-ttu-id="5c92c-192">固定的變數位於儲存體位置，不會受到記憶體回收行程的作業。</span><span class="sxs-lookup"><span data-stu-id="5c92c-192">Fixed variables reside in storage locations that are unaffected by operation of the garbage collector.</span></span> <span data-ttu-id="5c92c-193">（固定變數的範例包括本機變數、 值參數和變數建立為指標取值）。相反地，可移動的變數存在於受限於記憶體回收行程的重新配置或配置的儲存體位置中。</span><span class="sxs-lookup"><span data-stu-id="5c92c-193">(Examples of fixed variables include local variables, value parameters, and variables created by dereferencing pointers.) On the other hand, moveable variables reside in storage locations that are subject to relocation or disposal by the garbage collector.</span></span> <span data-ttu-id="5c92c-194">（可移動的變數的範例包括欄位中物件和陣列的元素。）</span><span class="sxs-lookup"><span data-stu-id="5c92c-194">(Examples of moveable variables include fields in objects and elements of arrays.)</span></span>

<span data-ttu-id="5c92c-195">`&`運算子 ([傳址運算子](unsafe-code.md#the-address-of-operator)) 允許固定的變數，以取得無限制的位址。</span><span class="sxs-lookup"><span data-stu-id="5c92c-195">The `&` operator ([The address-of operator](unsafe-code.md#the-address-of-operator)) permits the address of a fixed variable to be obtained without restrictions.</span></span> <span data-ttu-id="5c92c-196">不過，因為可移動的變數受限於記憶體回收行程的重新配置或配置，可移動的變數的位址可以只使用來取得`fixed`陳述式 ([fixed 陳述式](unsafe-code.md#the-fixed-statement))，和該位址只會針對該持續時間會保持有效`fixed`陳述式。</span><span class="sxs-lookup"><span data-stu-id="5c92c-196">However, because a moveable variable is subject to relocation or disposal by the garbage collector, the address of a moveable variable can only be obtained using a `fixed` statement ([The fixed statement](unsafe-code.md#the-fixed-statement)), and that address remains valid only for the duration of that `fixed` statement.</span></span>

<span data-ttu-id="5c92c-197">準確地說，固定的變數可以是下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="5c92c-197">In precise terms, a fixed variable is one of the following:</span></span>

*  <span data-ttu-id="5c92c-198">變數所得*simple_name* ([簡單名稱](expressions.md#simple-names))，指的本機變數或值參數，除非該變數會擷取匿名函式。</span><span class="sxs-lookup"><span data-stu-id="5c92c-198">A variable resulting from a *simple_name* ([Simple names](expressions.md#simple-names)) that refers to a local variable or a value parameter, unless the variable is captured by an anonymous function.</span></span>
*  <span data-ttu-id="5c92c-199">變數所得*member_access* ([成員存取](expressions.md#member-access)) 的表單`V.I`，其中`V`是固定的變數*struct_type*。</span><span class="sxs-lookup"><span data-stu-id="5c92c-199">A variable resulting from a *member_access* ([Member access](expressions.md#member-access)) of the form `V.I`, where `V` is a fixed variable of a *struct_type*.</span></span>
*  <span data-ttu-id="5c92c-200">變數的所產生的*pointer_indirection_expression* ([指標間接取值](unsafe-code.md#pointer-indirection)) 的形式`*P`，則*pointer_member_access* ([指標成員存取](unsafe-code.md#pointer-member-access)) 的形式`P->I`，或*pointer_element_access* ([指標元素存取](unsafe-code.md#pointer-element-access)) 表單的`P[E]`。</span><span class="sxs-lookup"><span data-stu-id="5c92c-200">A variable resulting from a *pointer_indirection_expression* ([Pointer indirection](unsafe-code.md#pointer-indirection)) of the form `*P`, a *pointer_member_access* ([Pointer member access](unsafe-code.md#pointer-member-access)) of the form `P->I`, or a *pointer_element_access* ([Pointer element access](unsafe-code.md#pointer-element-access)) of the form `P[E]`.</span></span>

<span data-ttu-id="5c92c-201">所有其他變數會分類為可移動的變數。</span><span class="sxs-lookup"><span data-stu-id="5c92c-201">All other variables are classified as moveable variables.</span></span>

<span data-ttu-id="5c92c-202">請注意靜態欄位會分類為可移動的變數。</span><span class="sxs-lookup"><span data-stu-id="5c92c-202">Note that a static field is classified as a moveable variable.</span></span> <span data-ttu-id="5c92c-203">也請注意，`ref`或`out`參數分類為可移動的變數，即使指定給參數的引數是固定的變數。</span><span class="sxs-lookup"><span data-stu-id="5c92c-203">Also note that a `ref` or `out` parameter is classified as a moveable variable, even if the argument given for the parameter is a fixed variable.</span></span> <span data-ttu-id="5c92c-204">最後，請注意，所產生的取值指標變數一律會分類為固定變數。</span><span class="sxs-lookup"><span data-stu-id="5c92c-204">Finally, note that a variable produced by dereferencing a pointer is always classified as a fixed variable.</span></span>

## <a name="pointer-conversions"></a><span data-ttu-id="5c92c-205">指標轉換</span><span class="sxs-lookup"><span data-stu-id="5c92c-205">Pointer conversions</span></span>

<span data-ttu-id="5c92c-206">在 unsafe 內容中，可用的隱含轉換的集合 ([隱含轉換](conversions.md#implicit-conversions)) 已擴充而納入下列的隱含指標轉換：</span><span class="sxs-lookup"><span data-stu-id="5c92c-206">In an unsafe context, the set of available implicit conversions ([Implicit conversions](conversions.md#implicit-conversions)) is extended to include the following implicit pointer conversions:</span></span>

*  <span data-ttu-id="5c92c-207">從任何*pointer_type*型別`void*`。</span><span class="sxs-lookup"><span data-stu-id="5c92c-207">From any *pointer_type* to the type `void*`.</span></span>
*  <span data-ttu-id="5c92c-208">從`null`常值的任何*pointer_type*。</span><span class="sxs-lookup"><span data-stu-id="5c92c-208">From the `null` literal to any *pointer_type*.</span></span>

<span data-ttu-id="5c92c-209">此外，在不安全的內容，可用的明確轉換的集合 ([明確轉換](conversions.md#explicit-conversions)) 已擴充而納入下列明確指標轉換：</span><span class="sxs-lookup"><span data-stu-id="5c92c-209">Additionally, in an unsafe context, the set of available explicit conversions ([Explicit conversions](conversions.md#explicit-conversions)) is extended to include the following explicit pointer conversions:</span></span>

*  <span data-ttu-id="5c92c-210">從任何*pointer_type*任何其他*pointer_type*。</span><span class="sxs-lookup"><span data-stu-id="5c92c-210">From any *pointer_type* to any other *pointer_type*.</span></span>
*  <span data-ttu-id="5c92c-211">從`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`，或`ulong`任何*pointer_type*。</span><span class="sxs-lookup"><span data-stu-id="5c92c-211">From `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `ulong` to any *pointer_type*.</span></span>
*  <span data-ttu-id="5c92c-212">從任何*pointer_type*要`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`，或`ulong`。</span><span class="sxs-lookup"><span data-stu-id="5c92c-212">From any *pointer_type* to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `ulong`.</span></span>

<span data-ttu-id="5c92c-213">最後，在不安全的內容，標準的隱含轉換的集合 ([標準的隱含轉換](conversions.md#standard-implicit-conversions)) 包含下列指標的轉換：</span><span class="sxs-lookup"><span data-stu-id="5c92c-213">Finally, in an unsafe context, the set of standard implicit conversions ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) includes the following pointer conversion:</span></span>

*  <span data-ttu-id="5c92c-214">從任何*pointer_type*型別`void*`。</span><span class="sxs-lookup"><span data-stu-id="5c92c-214">From any *pointer_type* to the type `void*`.</span></span>

<span data-ttu-id="5c92c-215">兩個指標類型之間轉換永遠不會變更實際的指標值。</span><span class="sxs-lookup"><span data-stu-id="5c92c-215">Conversions between two pointer types never change the actual pointer value.</span></span> <span data-ttu-id="5c92c-216">換句話說，從一個指標類型轉換為另一個沒有指標所提供的基礎位址上的任何作用。</span><span class="sxs-lookup"><span data-stu-id="5c92c-216">In other words, a conversion from one pointer type to another has no effect on the underlying address given by the pointer.</span></span>

<span data-ttu-id="5c92c-217">當一個指標類型轉換到另一個，如果產生的指標不指向類型正確對齊時，如果已取值的結果行為是未定義。</span><span class="sxs-lookup"><span data-stu-id="5c92c-217">When one pointer type is converted to another, if the resulting pointer is not correctly aligned for the pointed-to type, the behavior is undefined if the result is dereferenced.</span></span> <span data-ttu-id="5c92c-218">一般情況下，「 正確對齊 」 的概念是可轉移的： 如果類型的指標`A`正確對齊類型的指標`B`，它會依次正確對齊的類型的指標`C`，然後指向的型別`A`正確對齊類型的指標`C`。</span><span class="sxs-lookup"><span data-stu-id="5c92c-218">In general, the concept "correctly aligned" is transitive: if a pointer to type `A` is correctly aligned for a pointer to type `B`, which, in turn, is correctly aligned for a pointer to type `C`, then a pointer to type `A` is correctly aligned for a pointer to type `C`.</span></span>

<span data-ttu-id="5c92c-219">請考慮在其中有一種類型的變數透過不同類型的指標存取下列情況：</span><span class="sxs-lookup"><span data-stu-id="5c92c-219">Consider the following case in which a variable having one type is accessed via a pointer to a different type:</span></span>

```csharp
char c = 'A';
char* pc = &c;
void* pv = pc;
int* pi = (int*)pv;
int i = *pi;         // undefined
*pi = 123456;        // undefined
```

<span data-ttu-id="5c92c-220">當指標類型轉換成位元組，指標結果會指向的最低定址位元組的變數。</span><span class="sxs-lookup"><span data-stu-id="5c92c-220">When a pointer type is converted to a pointer to byte, the result points to the lowest addressed byte of the variable.</span></span> <span data-ttu-id="5c92c-221">後續的增量，產生的結果，但不超過變數的大小會產生剩餘的位元組，該變數的指標。</span><span class="sxs-lookup"><span data-stu-id="5c92c-221">Successive increments of the result, up to the size of the variable, yield pointers to the remaining bytes of that variable.</span></span> <span data-ttu-id="5c92c-222">例如，下列方法會顯示每個八個位元組的雙精度浮點數中做為十六進位的值：</span><span class="sxs-lookup"><span data-stu-id="5c92c-222">For example, the following method displays each of the eight bytes in a double as a hexadecimal value:</span></span>

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

<span data-ttu-id="5c92c-223">當然，產生的輸出取決於位元組序。</span><span class="sxs-lookup"><span data-stu-id="5c92c-223">Of course, the output produced depends on endianness.</span></span>

<span data-ttu-id="5c92c-224">指標與整數之間的對應是由實作定義。</span><span class="sxs-lookup"><span data-stu-id="5c92c-224">Mappings between pointers and integers are implementation-defined.</span></span> <span data-ttu-id="5c92c-225">不過，在 32 \* 和 64 位元 CPU 架構與線性位址空間的指標的轉換整數類資料類型通常和行為完全相同的轉換`uint`或`ulong`分別，或從這些整數類資料類型的值。</span><span class="sxs-lookup"><span data-stu-id="5c92c-225">However, on 32\* and 64-bit CPU architectures with a linear address space, conversions of pointers to or from integral types typically behave exactly like conversions of `uint` or `ulong` values, respectively, to or from those integral types.</span></span>

### <a name="pointer-arrays"></a><span data-ttu-id="5c92c-226">指標陣列</span><span class="sxs-lookup"><span data-stu-id="5c92c-226">Pointer arrays</span></span>

<span data-ttu-id="5c92c-227">在 unsafe 內容中，可以建構陣列的指標。</span><span class="sxs-lookup"><span data-stu-id="5c92c-227">In an unsafe context, arrays of pointers can be constructed.</span></span> <span data-ttu-id="5c92c-228">只對某些適用於其他陣列類型的轉換可在指標陣列：</span><span class="sxs-lookup"><span data-stu-id="5c92c-228">Only some of the conversions that apply to other array types are allowed on pointer arrays:</span></span>

*  <span data-ttu-id="5c92c-229">隱含參考轉換 ([隱含參考轉換](conversions.md#implicit-reference-conversions)) 從任何*array_type*到`System.Array`，它也會實作的介面將套用至指標陣列。</span><span class="sxs-lookup"><span data-stu-id="5c92c-229">The implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) from any *array_type* to `System.Array` and the interfaces it implements also applies to pointer arrays.</span></span> <span data-ttu-id="5c92c-230">不過，任何嘗試存取陣列項目，透過`System.Array`或它所實作的介面將會導致在執行階段發生例外狀況為指標類型不可以轉換成`object`。</span><span class="sxs-lookup"><span data-stu-id="5c92c-230">However, any attempt to access the array elements through `System.Array` or the interfaces it implements will result in an exception at run-time, as pointer types are not convertible to `object`.</span></span>
*  <span data-ttu-id="5c92c-231">隱含和明確參考轉換 ([隱含參考轉換](conversions.md#implicit-reference-conversions)，[明確參考轉換](conversions.md#explicit-reference-conversions)) 從一維陣列的型別`S[]`至`System.Collections.Generic.IList<T>`和其一般基底介面永遠不會用於指標的陣列，因為指標類型不能當做類型引數，而且沒有指標類型的轉換成非指標類型。</span><span class="sxs-lookup"><span data-stu-id="5c92c-231">The implicit and explicit reference conversions ([Implicit reference conversions](conversions.md#implicit-reference-conversions), [Explicit reference conversions](conversions.md#explicit-reference-conversions)) from a single-dimensional array type `S[]` to `System.Collections.Generic.IList<T>` and its generic base interfaces never apply to pointer arrays, since pointer types cannot be used as type arguments, and there are no conversions from pointer types to non-pointer types.</span></span>
*  <span data-ttu-id="5c92c-232">明確參考轉換 ([明確參考轉換](conversions.md#explicit-reference-conversions)) 從`System.Array`介面的方式實作任何*array_type*適用於指標的陣列。</span><span class="sxs-lookup"><span data-stu-id="5c92c-232">The explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) from `System.Array` and the interfaces it implements to any *array_type* applies to pointer arrays.</span></span>
*  <span data-ttu-id="5c92c-233">明確參考轉換 ([明確參考轉換](conversions.md#explicit-reference-conversions)) 從`System.Collections.Generic.IList<S>`和其基底介面的一維陣列類型`T[]`永遠不會套用至指標陣列，因為不能是指標類型使用做為型別引數，並沒有指標類型的轉換成非指標類型。</span><span class="sxs-lookup"><span data-stu-id="5c92c-233">The explicit reference conversions ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) from `System.Collections.Generic.IList<S>` and its base interfaces to a single-dimensional array type `T[]` never applies to pointer arrays, since pointer types cannot be used as type arguments, and there are no conversions from pointer types to non-pointer types.</span></span>

<span data-ttu-id="5c92c-234">這些限制表示，用於擴充`foreach`陣列上的陳述式中所述[foreach 陳述式](statements.md#the-foreach-statement)不適用於指標的陣列。</span><span class="sxs-lookup"><span data-stu-id="5c92c-234">These restrictions mean that the expansion for the `foreach` statement over arrays described in [The foreach statement](statements.md#the-foreach-statement) cannot be applied to pointer arrays.</span></span> <span data-ttu-id="5c92c-235">相反地，在表單的 foreach 陳述式</span><span class="sxs-lookup"><span data-stu-id="5c92c-235">Instead, a foreach statement of the form</span></span>

```csharp
foreach (V v in x) embedded_statement
```

<span data-ttu-id="5c92c-236">其中的型別`x`是陣列類型的形式`T[,,...,]`，`N`會減 1 的維度數目並`T`或`V`是指標類型，會擴大使用巢狀的 for 迴圈，如下所示：</span><span class="sxs-lookup"><span data-stu-id="5c92c-236">where the type of `x` is an array type of the form `T[,,...,]`, `N` is the number of dimensions minus 1 and `T` or `V` is a pointer type, is expanded using nested for-loops as follows:</span></span>

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

<span data-ttu-id="5c92c-237">變數`a`， `i0`， `i1`、...、`iN`不可見的或存取`x`或*embedded_statement*或任何其他來源的程式碼的程式。</span><span class="sxs-lookup"><span data-stu-id="5c92c-237">The variables `a`, `i0`, `i1`, ..., `iN` are not visible to or accessible to `x` or the *embedded_statement* or any other source code of the program.</span></span> <span data-ttu-id="5c92c-238">變數`v`是唯讀，在內嵌的陳述式。</span><span class="sxs-lookup"><span data-stu-id="5c92c-238">The variable `v` is read-only in the embedded statement.</span></span> <span data-ttu-id="5c92c-239">如果沒有明確的轉換 ([指標轉換](unsafe-code.md#pointer-conversions)) 從`T`（項目型別） 到`V`，會產生錯誤並採取任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="5c92c-239">If there is not an explicit conversion ([Pointer conversions](unsafe-code.md#pointer-conversions)) from `T` (the element type) to `V`, an error is produced and no further steps are taken.</span></span> <span data-ttu-id="5c92c-240">如果`x`具有值`null`、`System.NullReferenceException`會在執行階段擲回。</span><span class="sxs-lookup"><span data-stu-id="5c92c-240">If `x` has the value `null`, a `System.NullReferenceException` is thrown at run-time.</span></span>

## <a name="pointers-in-expressions"></a><span data-ttu-id="5c92c-241">運算式中的指標</span><span class="sxs-lookup"><span data-stu-id="5c92c-241">Pointers in expressions</span></span>

<span data-ttu-id="5c92c-242">在 unsafe 內容中，運算式可能會產生結果的指標類型，但不安全的內容之外，它是指標類型運算式的編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="5c92c-242">In an unsafe context, an expression may yield a result of a pointer type, but outside an unsafe context it is a compile-time error for an expression to be of a pointer type.</span></span> <span data-ttu-id="5c92c-243">準確地說，不安全的內容之外發生編譯時期錯誤，就會發生的話*simple_name* ([簡單名稱](expressions.md#simple-names))， *member_access* ([成員存取](expressions.md#member-access))， *invocation_expression* ([引動過程運算式](expressions.md#invocation-expressions))，或*element_access* ([元素存取](expressions.md#element-access)) 的指標類型。</span><span class="sxs-lookup"><span data-stu-id="5c92c-243">In precise terms, outside an unsafe context a compile-time error occurs if any *simple_name* ([Simple names](expressions.md#simple-names)), *member_access* ([Member access](expressions.md#member-access)), *invocation_expression* ([Invocation expressions](expressions.md#invocation-expressions)), or *element_access* ([Element access](expressions.md#element-access)) is of a pointer type.</span></span>

<span data-ttu-id="5c92c-244">在 unsafe 內容中， *primary_no_array_creation_expression* ([主要運算式](expressions.md#primary-expressions)) 和*unary_expression* ([的一元運算子](expressions.md#unary-operators))生產允許下列的其他建構函式：</span><span class="sxs-lookup"><span data-stu-id="5c92c-244">In an unsafe context, the *primary_no_array_creation_expression* ([Primary expressions](expressions.md#primary-expressions)) and *unary_expression* ([Unary operators](expressions.md#unary-operators)) productions permit the following additional constructs:</span></span>

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

<span data-ttu-id="5c92c-245">這些建構是由下列各節所述。</span><span class="sxs-lookup"><span data-stu-id="5c92c-245">These constructs are described in the following sections.</span></span> <span data-ttu-id="5c92c-246">文法所默許的優先順序和順序關聯性的不安全的運算子。</span><span class="sxs-lookup"><span data-stu-id="5c92c-246">The precedence and associativity of the unsafe operators is implied by the grammar.</span></span>

### <a name="pointer-indirection"></a><span data-ttu-id="5c92c-247">指標間接取值</span><span class="sxs-lookup"><span data-stu-id="5c92c-247">Pointer indirection</span></span>

<span data-ttu-id="5c92c-248">A *pointer_indirection_expression*包含星號 (`*`) 後面接著*unary_expression*。</span><span class="sxs-lookup"><span data-stu-id="5c92c-248">A *pointer_indirection_expression* consists of an asterisk (`*`) followed by a *unary_expression*.</span></span>

```antlr
pointer_indirection_expression
    : '*' unary_expression
    ;
```

<span data-ttu-id="5c92c-249">一元`*`表示指標間接取值運算子，並用來取得指標所指向的變數。</span><span class="sxs-lookup"><span data-stu-id="5c92c-249">The unary `*` operator denotes pointer indirection and is used to obtain the variable to which a pointer points.</span></span> <span data-ttu-id="5c92c-250">評估的結果`*P`，其中`P`是指標類型的運算式`T*`，這類型的變數`T`。</span><span class="sxs-lookup"><span data-stu-id="5c92c-250">The result of evaluating `*P`, where `P` is an expression of a pointer type `T*`, is a variable of type `T`.</span></span> <span data-ttu-id="5c92c-251">它是編譯時期錯誤套用一元`*`類型的運算式的運算子`void*`或不是指標類型的運算式。</span><span class="sxs-lookup"><span data-stu-id="5c92c-251">It is a compile-time error to apply the unary `*` operator to an expression of type `void*` or to an expression that isn't of a pointer type.</span></span>

<span data-ttu-id="5c92c-252">套用一元的效果`*`運算子來`null`指標是由實作定義。</span><span class="sxs-lookup"><span data-stu-id="5c92c-252">The effect of applying the unary `*` operator to a `null` pointer is implementation-defined.</span></span> <span data-ttu-id="5c92c-253">特別是，則此作業會擲回無法保證`System.NullReferenceException`。</span><span class="sxs-lookup"><span data-stu-id="5c92c-253">In particular, there is no guarantee that this operation throws a `System.NullReferenceException`.</span></span>

<span data-ttu-id="5c92c-254">如果無效的值已指派給指標，也就是一元的行為`*`運算子是未定義。</span><span class="sxs-lookup"><span data-stu-id="5c92c-254">If an invalid value has been assigned to the pointer, the behavior of the unary `*` operator is undefined.</span></span> <span data-ttu-id="5c92c-255">無效值的指標取值的一元`*`運算子會不一致的類型指的位址 (在中的範例，請參閱[指標轉換](unsafe-code.md#pointer-conversions))，以及之後的變數的位址存留期結束。</span><span class="sxs-lookup"><span data-stu-id="5c92c-255">Among the invalid values for dereferencing a pointer by the unary `*` operator are an address inappropriately aligned for the type pointed to (see example in [Pointer conversions](unsafe-code.md#pointer-conversions)), and the address of a variable after the end of its lifetime.</span></span>

<span data-ttu-id="5c92c-256">為了明確設定分析的詳細資訊，藉由評估格式的運算式所產生的變數`*P`被視為一開始指派 ([最初指派變數](variables.md#initially-assigned-variables))。</span><span class="sxs-lookup"><span data-stu-id="5c92c-256">For purposes of definite assignment analysis, a variable produced by evaluating an expression of the form `*P` is considered initially assigned ([Initially assigned variables](variables.md#initially-assigned-variables)).</span></span>

### <a name="pointer-member-access"></a><span data-ttu-id="5c92c-257">指標成員存取</span><span class="sxs-lookup"><span data-stu-id="5c92c-257">Pointer member access</span></span>

<span data-ttu-id="5c92c-258">A *pointer_member_access*組成*primary_expression*，後面接著"`->`"語彙基元，後面接著*識別碼*和選擇性的*type_argument_list*。</span><span class="sxs-lookup"><span data-stu-id="5c92c-258">A *pointer_member_access* consists of a *primary_expression*, followed by a "`->`" token, followed by an *identifier* and an optional *type_argument_list*.</span></span>

```antlr
pointer_member_access
    : primary_expression '->' identifier
    ;
```

<span data-ttu-id="5c92c-259">在表單的指標成員存取`P->I`，`P`必須是指標類型的運算式而非`void*`，和`I`必須表示成的型別可存取成員`P`點。</span><span class="sxs-lookup"><span data-stu-id="5c92c-259">In a pointer member access of the form `P->I`, `P` must be an expression of a pointer type other than `void*`, and `I` must denote an accessible member of the type to which `P` points.</span></span>

<span data-ttu-id="5c92c-260">指標成員存取的表單`P->I`評估為完全`(*P).I`。</span><span class="sxs-lookup"><span data-stu-id="5c92c-260">A pointer member access of the form `P->I` is evaluated exactly as `(*P).I`.</span></span> <span data-ttu-id="5c92c-261">如需指標間接運算子的說明 (`*`)，請參閱 <<c2> [ 指標間接取值](unsafe-code.md#pointer-indirection)。</span><span class="sxs-lookup"><span data-stu-id="5c92c-261">For a description of the pointer indirection operator (`*`), see [Pointer indirection](unsafe-code.md#pointer-indirection).</span></span> <span data-ttu-id="5c92c-262">如需說明的成員存取運算子 (`.`)，請參閱 <<c2> [ 成員存取](expressions.md#member-access)。</span><span class="sxs-lookup"><span data-stu-id="5c92c-262">For a description of the member access operator (`.`), see [Member access](expressions.md#member-access).</span></span>

<span data-ttu-id="5c92c-263">在範例</span><span class="sxs-lookup"><span data-stu-id="5c92c-263">In the example</span></span>

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

<span data-ttu-id="5c92c-264">`->`運算子用來存取欄位，並叫用透過指標結構的方法。</span><span class="sxs-lookup"><span data-stu-id="5c92c-264">the `->` operator is used to access fields and invoke a method of a struct through a pointer.</span></span> <span data-ttu-id="5c92c-265">因為作業`P->I`就相當於`(*P).I`，則`Main`方法可以同樣撰寫：</span><span class="sxs-lookup"><span data-stu-id="5c92c-265">Because the operation `P->I` is precisely equivalent to `(*P).I`, the `Main` method could equally well have been written:</span></span>

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

### <a name="pointer-element-access"></a><span data-ttu-id="5c92c-266">指標元素存取</span><span class="sxs-lookup"><span data-stu-id="5c92c-266">Pointer element access</span></span>

<span data-ttu-id="5c92c-267">A *pointer_element_access*組成*primary_no_array_creation_expression*後面接著括住的運算式"`[`"和"`]`」。</span><span class="sxs-lookup"><span data-stu-id="5c92c-267">A *pointer_element_access* consists of a *primary_no_array_creation_expression* followed by an expression enclosed in "`[`" and "`]`".</span></span>

```antlr
pointer_element_access
    : primary_no_array_creation_expression '[' expression ']'
    ;
```

<span data-ttu-id="5c92c-268">在表單的指標元素存取`P[E]`，`P`必須是指標類型的運算式而非`void*`，和`E`必須是能夠隱含轉換成的運算式`int`， `uint`， `long`，或`ulong`。</span><span class="sxs-lookup"><span data-stu-id="5c92c-268">In a pointer element access of the form `P[E]`, `P` must be an expression of a pointer type other than `void*`, and `E` must be an expression that can be implicitly converted to `int`, `uint`, `long`, or `ulong`.</span></span>

<span data-ttu-id="5c92c-269">表單的指標元素存取`P[E]`評估為完全`*(P + E)`。</span><span class="sxs-lookup"><span data-stu-id="5c92c-269">A pointer element access of the form `P[E]` is evaluated exactly as `*(P + E)`.</span></span> <span data-ttu-id="5c92c-270">如需指標間接運算子的說明 (`*`)，請參閱 <<c2> [ 指標間接取值](unsafe-code.md#pointer-indirection)。</span><span class="sxs-lookup"><span data-stu-id="5c92c-270">For a description of the pointer indirection operator (`*`), see [Pointer indirection](unsafe-code.md#pointer-indirection).</span></span> <span data-ttu-id="5c92c-271">如需指標的加法運算子的說明 (`+`)，請參閱 <<c2> [ 指標算術](unsafe-code.md#pointer-arithmetic)。</span><span class="sxs-lookup"><span data-stu-id="5c92c-271">For a description of the pointer addition operator (`+`), see [Pointer arithmetic](unsafe-code.md#pointer-arithmetic).</span></span>

<span data-ttu-id="5c92c-272">在範例</span><span class="sxs-lookup"><span data-stu-id="5c92c-272">In the example</span></span>

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

<span data-ttu-id="5c92c-273">指標元素存取用來初始化中的字元緩衝區`for`迴圈。</span><span class="sxs-lookup"><span data-stu-id="5c92c-273">a pointer element access is used to initialize the character buffer in a `for` loop.</span></span> <span data-ttu-id="5c92c-274">因為作業`P[E]`就相當於`*(P + E)`，範例可以同樣撰寫：</span><span class="sxs-lookup"><span data-stu-id="5c92c-274">Because the operation `P[E]` is precisely equivalent to `*(P + E)`, the example could equally well have been written:</span></span>

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

<span data-ttu-id="5c92c-275">指標的項目存取運算子不會超出範圍檢查錯誤及存取時的行為項目未定義超出範圍。</span><span class="sxs-lookup"><span data-stu-id="5c92c-275">The pointer element access operator does not check for out-of-bounds errors and the behavior when accessing an out-of-bounds element is undefined.</span></span> <span data-ttu-id="5c92c-276">這等同於 C 和C++。</span><span class="sxs-lookup"><span data-stu-id="5c92c-276">This is the same as C and C++.</span></span>

### <a name="the-address-of-operator"></a><span data-ttu-id="5c92c-277">傳址運算子</span><span class="sxs-lookup"><span data-stu-id="5c92c-277">The address-of operator</span></span>

<span data-ttu-id="5c92c-278">*Addressof_expression*包含連字號 (`&`) 後面接著*unary_expression*。</span><span class="sxs-lookup"><span data-stu-id="5c92c-278">An *addressof_expression* consists of an ampersand (`&`) followed by a *unary_expression*.</span></span>

```antlr
addressof_expression
    : '&' unary_expression
    ;
```

<span data-ttu-id="5c92c-279">指定運算式`E`類型`T`歸類為固定變數和 ([固定和可移動變數](unsafe-code.md#fixed-and-moveable-variables))，建構`&E`計算所指定變數的位址`E`.</span><span class="sxs-lookup"><span data-stu-id="5c92c-279">Given an expression `E` which is of a type `T` and is classified as a fixed variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)), the construct `&E` computes the address of the variable given by `E`.</span></span> <span data-ttu-id="5c92c-280">結果的型別是`T*`和分類為值。</span><span class="sxs-lookup"><span data-stu-id="5c92c-280">The type of the result is `T*` and is classified as a value.</span></span> <span data-ttu-id="5c92c-281">如果，就會發生編譯時期錯誤`E`未分類為變數中，如果`E`歸類為唯讀的本機變數，或如果`E`代表可移動的變數。</span><span class="sxs-lookup"><span data-stu-id="5c92c-281">A compile-time error occurs if `E` is not classified as a variable, if `E` is classified as a read-only local variable, or if `E` denotes a moveable variable.</span></span> <span data-ttu-id="5c92c-282">在最後一個案例中，在 fixed 陳述式 ([fixed 陳述式](unsafe-code.md#the-fixed-statement)) 可用來暫時 「 修正 」 變數，然後再取得其位址。</span><span class="sxs-lookup"><span data-stu-id="5c92c-282">In the last case, a fixed statement ([The fixed statement](unsafe-code.md#the-fixed-statement)) can be used to temporarily "fix" the variable before obtaining its address.</span></span> <span data-ttu-id="5c92c-283">中所述[成員存取](expressions.md#member-access)、 執行個體建構函式或靜態的建構函式的結構或類別來定義外部`readonly` 欄位中，該欄位會被視為值，而不是變數。</span><span class="sxs-lookup"><span data-stu-id="5c92c-283">As stated in [Member access](expressions.md#member-access), outside an instance constructor or static constructor for a struct or class that defines a `readonly` field, that field is considered a value, not a variable.</span></span> <span data-ttu-id="5c92c-284">因此，無法取得其位址。</span><span class="sxs-lookup"><span data-stu-id="5c92c-284">As such, its address cannot be taken.</span></span> <span data-ttu-id="5c92c-285">同樣地，您無法取得常數的位址。</span><span class="sxs-lookup"><span data-stu-id="5c92c-285">Similarly, the address of a constant cannot be taken.</span></span>

<span data-ttu-id="5c92c-286">`&`運算子不需要它的引數，明確指派，但下列`&`作業中，套用運算子的變數會被視為已明確指派作業發生所在的執行路徑中。</span><span class="sxs-lookup"><span data-stu-id="5c92c-286">The `&` operator does not require its argument to be definitely assigned, but following an `&` operation, the variable to which the operator is applied is considered definitely assigned in the execution path in which the operation occurs.</span></span> <span data-ttu-id="5c92c-287">它是以確保該正確地初始化變數的程式設計人員的責任實際未發生在此情況下。</span><span class="sxs-lookup"><span data-stu-id="5c92c-287">It is the responsibility of the programmer to ensure that correct initialization of the variable actually does take place in this situation.</span></span>

<span data-ttu-id="5c92c-288">在範例</span><span class="sxs-lookup"><span data-stu-id="5c92c-288">In the example</span></span>

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

<span data-ttu-id="5c92c-289">`i` 會被視為明確指派後面`&i`用來初始化作業`p`。</span><span class="sxs-lookup"><span data-stu-id="5c92c-289">`i` is considered definitely assigned following the `&i` operation used to initialize `p`.</span></span> <span data-ttu-id="5c92c-290">指派給`*p`實際上是初始化`i`，這項初始化包含負責程式設計人員，但如果已移除指派，就會發生任何編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="5c92c-290">The assignment to `*p` in effect initializes `i`, but the inclusion of this initialization is the responsibility of the programmer, and no compile-time error would occur if the assignment was removed.</span></span>

<span data-ttu-id="5c92c-291">明確指派之規則`&`運算子存在，您可以避免多餘的初始化的區域變數。</span><span class="sxs-lookup"><span data-stu-id="5c92c-291">The rules of definite assignment for the `&` operator exist such that redundant initialization of local variables can be avoided.</span></span> <span data-ttu-id="5c92c-292">比方說，許多外部 Api 需要 api 會填入結構的指標。</span><span class="sxs-lookup"><span data-stu-id="5c92c-292">For example, many external APIs take a pointer to a structure which is filled in by the API.</span></span> <span data-ttu-id="5c92c-293">這類 Api 的呼叫一般為 pass 本機結構變數的位址，且沒有規則，多餘的初始化結構變數的需要。</span><span class="sxs-lookup"><span data-stu-id="5c92c-293">Calls to such APIs typically pass the address of a local struct variable, and without the rule, redundant initialization of the struct variable would be required.</span></span>

### <a name="pointer-increment-and-decrement"></a><span data-ttu-id="5c92c-294">指標遞增和遞減</span><span class="sxs-lookup"><span data-stu-id="5c92c-294">Pointer increment and decrement</span></span>

<span data-ttu-id="5c92c-295">在 unsafe 內容中，`++`並`--`運算子 ([後置遞增和遞減運算子](expressions.md#postfix-increment-and-decrement-operators)並[前置遞增和遞減運算子](expressions.md#prefix-increment-and-decrement-operators)) 可以套用至指標以外的所有類型的變數`void*`。</span><span class="sxs-lookup"><span data-stu-id="5c92c-295">In an unsafe context, the `++` and `--` operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)) can be applied to pointer variables of all types except `void*`.</span></span> <span data-ttu-id="5c92c-296">因此，針對每個指標類型`T*`，隱含定義了下列運算子：</span><span class="sxs-lookup"><span data-stu-id="5c92c-296">Thus, for every pointer type `T*`, the following operators are implicitly defined:</span></span>

```csharp
T* operator ++(T* x);
T* operator --(T* x);
```

<span data-ttu-id="5c92c-297">這些運算子會產生與相同的結果`x + 1`並`x - 1`分別 ([指標算術](unsafe-code.md#pointer-arithmetic))。</span><span class="sxs-lookup"><span data-stu-id="5c92c-297">The operators produce the same results as `x + 1` and `x - 1`, respectively ([Pointer arithmetic](unsafe-code.md#pointer-arithmetic)).</span></span> <span data-ttu-id="5c92c-298">換句話說，之指標變數的型別`T*`，則`++`運算子會加入`sizeof(T)`變數中所包含的位址和`--`運算子減去`sizeof(T)`從變數中所包含的位址。</span><span class="sxs-lookup"><span data-stu-id="5c92c-298">In other words, for a pointer variable of type `T*`, the `++` operator adds `sizeof(T)` to the address contained in the variable, and the `--` operator subtracts `sizeof(T)` from the address contained in the variable.</span></span>

<span data-ttu-id="5c92c-299">如果指標遞增或遞減運算讓指標類型的定義域溢位，結果是由實作定義，但會產生任何例外狀況。</span><span class="sxs-lookup"><span data-stu-id="5c92c-299">If a pointer increment or decrement operation overflows the domain of the pointer type, the result is implementation-defined, but no exceptions are produced.</span></span>

### <a name="pointer-arithmetic"></a><span data-ttu-id="5c92c-300">指標算術</span><span class="sxs-lookup"><span data-stu-id="5c92c-300">Pointer arithmetic</span></span>

<span data-ttu-id="5c92c-301">在 unsafe 內容中，`+`並`-`運算子 ([加法運算子](expressions.md#addition-operator)並[減法運算子](expressions.md#subtraction-operator)) 可以套用至值以外的所有指標類型`void*`。</span><span class="sxs-lookup"><span data-stu-id="5c92c-301">In an unsafe context, the `+` and `-` operators ([Addition operator](expressions.md#addition-operator) and [Subtraction operator](expressions.md#subtraction-operator)) can be applied to values of all pointer types except `void*`.</span></span> <span data-ttu-id="5c92c-302">因此，針對每個指標類型`T*`，隱含定義了下列運算子：</span><span class="sxs-lookup"><span data-stu-id="5c92c-302">Thus, for every pointer type `T*`, the following operators are implicitly defined:</span></span>

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

<span data-ttu-id="5c92c-303">指定運算式`P`指標型別的`T*`和運算式`N`型別的`int`， `uint`， `long`，或`ulong`，運算式`P + N`和`N + P`計算指標值的型別`T*`，會將`N * sizeof(T)`所指定的位址`P`。</span><span class="sxs-lookup"><span data-stu-id="5c92c-303">Given an expression `P` of a pointer type `T*` and an expression `N` of type `int`, `uint`, `long`, or `ulong`, the expressions `P + N` and `N + P` compute the pointer value of type `T*` that results from adding `N * sizeof(T)` to the address given by `P`.</span></span> <span data-ttu-id="5c92c-304">同樣地，運算式`P - N`計算類型的指標值`T*`所產生減去`N * sizeof(T)`所指定的位址`P`。</span><span class="sxs-lookup"><span data-stu-id="5c92c-304">Likewise, the expression `P - N` computes the pointer value of type `T*` that results from subtracting `N * sizeof(T)` from the address given by `P`.</span></span>

<span data-ttu-id="5c92c-305">給定兩個運算式，`P`並`Q`，指標類型的`T*`，運算式`P - Q`計算指定的位址之間的差異`P`和`Q`並再將該差異除以`sizeof(T)`.</span><span class="sxs-lookup"><span data-stu-id="5c92c-305">Given two expressions, `P` and `Q`, of a pointer type `T*`, the expression `P - Q` computes the difference between the addresses given by `P` and `Q` and then divides that difference by `sizeof(T)`.</span></span> <span data-ttu-id="5c92c-306">結果的型別一律為`long`。</span><span class="sxs-lookup"><span data-stu-id="5c92c-306">The type of the result is always `long`.</span></span> <span data-ttu-id="5c92c-307">實際上`P - Q`會計算為`((long)(P) - (long)(Q)) / sizeof(T)`。</span><span class="sxs-lookup"><span data-stu-id="5c92c-307">In effect, `P - Q` is computed as `((long)(P) - (long)(Q)) / sizeof(T)`.</span></span>

<span data-ttu-id="5c92c-308">例如: </span><span class="sxs-lookup"><span data-stu-id="5c92c-308">For example:</span></span>

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

<span data-ttu-id="5c92c-309">產生下列輸出：</span><span class="sxs-lookup"><span data-stu-id="5c92c-309">which produces the output:</span></span>

```
p - q = -14
q - p = 14
```

<span data-ttu-id="5c92c-310">如果指標的算術運算溢位的指標類型的網域，結果會截斷以實作定義的方式，但會產生任何例外狀況。</span><span class="sxs-lookup"><span data-stu-id="5c92c-310">If a pointer arithmetic operation overflows the domain of the pointer type, the result is truncated in an implementation-defined fashion, but no exceptions are produced.</span></span>

### <a name="pointer-comparison"></a><span data-ttu-id="5c92c-311">指標比較</span><span class="sxs-lookup"><span data-stu-id="5c92c-311">Pointer comparison</span></span>

<span data-ttu-id="5c92c-312">在 unsafe 內容中， `==`， `!=`， `<`， `>`， `<=`，和`=>`運算子 ([關係和類型測試運算子](expressions.md#relational-and-type-testing-operators)) 可以套用至所有的值指標類型。</span><span class="sxs-lookup"><span data-stu-id="5c92c-312">In an unsafe context, the `==`, `!=`, `<`, `>`, `<=`, and `=>` operators ([Relational and type-testing operators](expressions.md#relational-and-type-testing-operators)) can be applied to values of all pointer types.</span></span> <span data-ttu-id="5c92c-313">指標比較運算子包括︰</span><span class="sxs-lookup"><span data-stu-id="5c92c-313">The pointer comparison operators are:</span></span>

```csharp
bool operator ==(void* x, void* y);
bool operator !=(void* x, void* y);
bool operator <(void* x, void* y);
bool operator >(void* x, void* y);
bool operator <=(void* x, void* y);
bool operator >=(void* x, void* y);
```

<span data-ttu-id="5c92c-314">因為隱含轉換存在任何指標類型，若要從`void*`能夠使用這些運算子比較型別，任何指標類型的運算元。</span><span class="sxs-lookup"><span data-stu-id="5c92c-314">Because an implicit conversion exists from any pointer type to the `void*` type, operands of any pointer type can be compared using these operators.</span></span> <span data-ttu-id="5c92c-315">比較運算子比較兩個運算元所指定，如同它們是不帶正負號的整數的位址。</span><span class="sxs-lookup"><span data-stu-id="5c92c-315">The comparison operators compare the addresses given by the two operands as if they were unsigned integers.</span></span>

### <a name="the-sizeof-operator"></a><span data-ttu-id="5c92c-316">Sizeof 運算子</span><span class="sxs-lookup"><span data-stu-id="5c92c-316">The sizeof operator</span></span>

<span data-ttu-id="5c92c-317">`sizeof`運算子會傳回指定型別的變數所佔用的位元組數目。</span><span class="sxs-lookup"><span data-stu-id="5c92c-317">The `sizeof` operator returns the number of bytes occupied by a variable of a given type.</span></span> <span data-ttu-id="5c92c-318">做為運算元所指定的型別`sizeof`必須是*unmanaged_type* ([指標類型](unsafe-code.md#pointer-types))。</span><span class="sxs-lookup"><span data-stu-id="5c92c-318">The type specified as an operand to `sizeof` must be an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

```antlr
sizeof_expression
    : 'sizeof' '(' unmanaged_type ')'
    ;
```

<span data-ttu-id="5c92c-319">結果`sizeof`運算子是值型別的`int`。</span><span class="sxs-lookup"><span data-stu-id="5c92c-319">The result of the `sizeof` operator is a value of type `int`.</span></span> <span data-ttu-id="5c92c-320">針對某些預先定義的型別，`sizeof`運算子會產生常數值下, 表所示。</span><span class="sxs-lookup"><span data-stu-id="5c92c-320">For certain predefined types, the `sizeof` operator yields a constant value as shown in the table below.</span></span>


| <span data-ttu-id="5c92c-321">__運算式__</span><span class="sxs-lookup"><span data-stu-id="5c92c-321">__Expression__</span></span>   | <span data-ttu-id="5c92c-322">__結果__</span><span class="sxs-lookup"><span data-stu-id="5c92c-322">__Result__</span></span> |
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

<span data-ttu-id="5c92c-323">對於所有其他類型，結果`sizeof`運算子是由實作定義，並會分類為值，而不是常數。</span><span class="sxs-lookup"><span data-stu-id="5c92c-323">For all other types, the result of the `sizeof` operator is implementation-defined and is classified as a value, not a constant.</span></span>

<span data-ttu-id="5c92c-324">未指定封裝至結構的成員的順序。</span><span class="sxs-lookup"><span data-stu-id="5c92c-324">The order in which members are packed into a struct is unspecified.</span></span>

<span data-ttu-id="5c92c-325">用於對齊用途，可能有未命名的填補結構，結構內的開頭和結尾的結構。</span><span class="sxs-lookup"><span data-stu-id="5c92c-325">For alignment purposes, there may be unnamed padding at the beginning of a struct, within a struct, and at the end of the struct.</span></span> <span data-ttu-id="5c92c-326">用來做為填補字元位元的內容就無法確定。</span><span class="sxs-lookup"><span data-stu-id="5c92c-326">The contents of the bits used as padding are indeterminate.</span></span>

<span data-ttu-id="5c92c-327">當套用至具有結構類型的運算元，結果會是該型別，包括任何填補字元變數中的位元組總數。</span><span class="sxs-lookup"><span data-stu-id="5c92c-327">When applied to an operand that has struct type, the result is the total number of bytes in a variable of that type, including any padding.</span></span>

## <a name="the-fixed-statement"></a><span data-ttu-id="5c92c-328">Fixed 陳述式</span><span class="sxs-lookup"><span data-stu-id="5c92c-328">The fixed statement</span></span>

<span data-ttu-id="5c92c-329">在 unsafe 內容中， *embedded_statement* ([陳述式](statements.md)) 生產環境允許額外的建構，`fixed`陳述式，這用來 「 修正 」 可移動的變數，其位址會維持不變的陳述式的持續時間。</span><span class="sxs-lookup"><span data-stu-id="5c92c-329">In an unsafe context, the *embedded_statement* ([Statements](statements.md)) production permits an additional construct, the `fixed` statement, which is used to "fix" a moveable variable such that its address remains constant for the duration of the statement.</span></span>

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

<span data-ttu-id="5c92c-330">每個*fixed_pointer_declarator*宣告的區域變數指定*pointer_type* ，並使用計算由相對應的位址初始化，區域變數*fixed_pointer_initializer*。</span><span class="sxs-lookup"><span data-stu-id="5c92c-330">Each *fixed_pointer_declarator* declares a local variable of the given *pointer_type* and initializes that local variable with the address computed by the corresponding *fixed_pointer_initializer*.</span></span> <span data-ttu-id="5c92c-331">在宣告的區域變數`fixed`陳述式是在任何可存取*fixed_pointer_initializer*發生該變數的宣告，並在右邊的 s *embedded_statement*的`fixed`陳述式。</span><span class="sxs-lookup"><span data-stu-id="5c92c-331">A local variable declared in a `fixed` statement is accessible in any *fixed_pointer_initializer*s occurring to the right of that variable's declaration, and in the *embedded_statement* of the `fixed` statement.</span></span> <span data-ttu-id="5c92c-332">所宣告的區域變數`fixed`陳述式會被視為唯讀。</span><span class="sxs-lookup"><span data-stu-id="5c92c-332">A local variable declared by a `fixed` statement is considered read-only.</span></span> <span data-ttu-id="5c92c-333">如果內嵌的陳述式嘗試修改這個本機變數，就會發生編譯時期錯誤 (透過指派或`++`並`--`運算子) 或將它傳遞為`ref`或`out`參數。</span><span class="sxs-lookup"><span data-stu-id="5c92c-333">A compile-time error occurs if the embedded statement attempts to modify this local variable (via assignment or the `++` and `--` operators) or pass it as a `ref` or `out` parameter.</span></span>

<span data-ttu-id="5c92c-334">A *fixed_pointer_initializer*可以是下列其中之一：</span><span class="sxs-lookup"><span data-stu-id="5c92c-334">A *fixed_pointer_initializer* can be one of the following:</span></span>

*  <span data-ttu-id="5c92c-335">語彙基元"`&`"後面接著*variable_reference* ([精確規則決定明確](variables.md#precise-rules-for-determining-definite-assignment)) 可移動的變數 ([固定和可移動變數](unsafe-code.md#fixed-and-moveable-variables))非受控型別的`T`，提供型別`T*`隱含地轉換成指標類型所提供`fixed`陳述式。</span><span class="sxs-lookup"><span data-stu-id="5c92c-335">The token "`&`" followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) to a moveable variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)) of an unmanaged type `T`, provided the type `T*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="5c92c-336">在此情況下，初始設定式會計算指定變數的位址，並保證變數會在固定位址的持續時間`fixed`陳述式。</span><span class="sxs-lookup"><span data-stu-id="5c92c-336">In this case, the initializer computes the address of the given variable, and the variable is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span>
*  <span data-ttu-id="5c92c-337">運算式*array_type* unmanaged 類型的項目`T`，提供型別`T*`隱含地轉換成指標類型所提供`fixed`陳述式。</span><span class="sxs-lookup"><span data-stu-id="5c92c-337">An expression of an *array_type* with elements of an unmanaged type `T`, provided the type `T*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="5c92c-338">在此情況下，初始設定式計算陣列中的第一個元素的位址，並將維持固定位址的持續時間保證，整個陣列`fixed`陳述式。</span><span class="sxs-lookup"><span data-stu-id="5c92c-338">In this case, the initializer computes the address of the first element in the array, and the entire array is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span> <span data-ttu-id="5c92c-339">如果陣列運算式為 null，或如果陣列中有零個項目，則初始設定式會計算等於零的位址。</span><span class="sxs-lookup"><span data-stu-id="5c92c-339">If the array expression is null or if the array has zero elements, the initializer computes an address equal to zero.</span></span>
*  <span data-ttu-id="5c92c-340">類型的運算式`string`，提供型別`char*`隱含地轉換成指標類型所提供`fixed`陳述式。</span><span class="sxs-lookup"><span data-stu-id="5c92c-340">An expression of type `string`, provided the type `char*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="5c92c-341">在此情況下，初始設定式計算，以字串的第一個字元的位址，而且整個字串保證維持在固定位址的持續時間`fixed`陳述式。</span><span class="sxs-lookup"><span data-stu-id="5c92c-341">In this case, the initializer computes the address of the first character in the string, and the entire string is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span> <span data-ttu-id="5c92c-342">行為`fixed`陳述式會實作定義的字串運算式是否為 null。</span><span class="sxs-lookup"><span data-stu-id="5c92c-342">The behavior of the `fixed` statement is implementation-defined if the string expression is null.</span></span>
*  <span data-ttu-id="5c92c-343">A *simple_name*或是*member_access*提供固定的大小緩衝區成員的類型是隱含地轉換成指標型別，指定參考的可移動的變數，固定的大小緩衝區成員在 `fixed`陳述式。</span><span class="sxs-lookup"><span data-stu-id="5c92c-343">A *simple_name* or *member_access* that references a fixed size buffer member of a moveable variable, provided the type of the fixed size buffer member is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="5c92c-344">初始設定式在此情況下，計算的第一個元素的固定的大小緩衝區的指標 ([運算式中固定大小緩衝區](unsafe-code.md#fixed-size-buffers-in-expressions))，並保證的固定的大小緩衝區期間保持固定位址`fixed`陳述式。</span><span class="sxs-lookup"><span data-stu-id="5c92c-344">In this case, the initializer computes a pointer to the first element of the fixed size buffer ([Fixed size buffers in expressions](unsafe-code.md#fixed-size-buffers-in-expressions)), and the fixed size buffer is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span>

<span data-ttu-id="5c92c-345">每個位址來計算*fixed_pointer_initializer* `fixed`陳述式可確保位址所參考的變數不是重新配置或記憶體回收行程期間的處置`fixed`陳述式。</span><span class="sxs-lookup"><span data-stu-id="5c92c-345">For each address computed by a *fixed_pointer_initializer* the `fixed` statement ensures that the variable referenced by the address is not subject to relocation or disposal by the garbage collector for the duration of the `fixed` statement.</span></span> <span data-ttu-id="5c92c-346">例如，如果所計算的地址*fixed_pointer_initializer*參考物件的欄位或陣列執行個體的項目`fixed`陳述式可以保證包含的物件執行個體不會重新定位或處置陳述式的存留期間。</span><span class="sxs-lookup"><span data-stu-id="5c92c-346">For example, if the address computed by a *fixed_pointer_initializer* references a field of an object or an element of an array instance, the `fixed` statement guarantees that the containing object instance is not relocated or disposed of during the lifetime of the statement.</span></span>

<span data-ttu-id="5c92c-347">程式設計人員必須負責確保所建立的指標`fixed`陳述式不會存留超過執行這些陳述式。</span><span class="sxs-lookup"><span data-stu-id="5c92c-347">It is the programmer's responsibility to ensure that pointers created by `fixed` statements do not survive beyond execution of those statements.</span></span> <span data-ttu-id="5c92c-348">例如，當指標建立`fixed`陳述式傳遞至外部 Api、 程式設計人員必須負責確保 Api 保有這些指標的任何記憶體。</span><span class="sxs-lookup"><span data-stu-id="5c92c-348">For example, when pointers created by `fixed` statements are passed to external APIs, it is the programmer's responsibility to ensure that the APIs retain no memory of these pointers.</span></span>

<span data-ttu-id="5c92c-349">（因為它們不能移動），則固定的物件可能會造成堆積的片段。</span><span class="sxs-lookup"><span data-stu-id="5c92c-349">Fixed objects may cause fragmentation of the heap (because they can't be moved).</span></span> <span data-ttu-id="5c92c-350">基於這個理由，才將物件固定只在絕對必要時，然後只針對最短的時間可能量。</span><span class="sxs-lookup"><span data-stu-id="5c92c-350">For that reason, objects should be fixed only when absolutely necessary and then only for the shortest amount of time possible.</span></span>

<span data-ttu-id="5c92c-351">此範例</span><span class="sxs-lookup"><span data-stu-id="5c92c-351">The example</span></span>

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

<span data-ttu-id="5c92c-352">示範了多個`fixed`陳述式。</span><span class="sxs-lookup"><span data-stu-id="5c92c-352">demonstrates several uses of the `fixed` statement.</span></span> <span data-ttu-id="5c92c-353">第一個陳述式修正，並取得靜態欄位的位址、 第二個陳述式修正，並取得的地址的執行個體欄位，和第三個陳述式修正，並取得陣列元素的位址。</span><span class="sxs-lookup"><span data-stu-id="5c92c-353">The first statement fixes and obtains the address of a static field, the second statement fixes and obtains the address of an instance field, and the third statement fixes and obtains the address of an array element.</span></span> <span data-ttu-id="5c92c-354">每個案例中就會使用一般錯誤`&`運算子，因為變數會歸類為可移動的變數。</span><span class="sxs-lookup"><span data-stu-id="5c92c-354">In each case it would have been an error to use the regular `&` operator since the variables are all classified as moveable variables.</span></span>

<span data-ttu-id="5c92c-355">第四個`fixed`在上述範例中的陳述式會產生類似的結果，第三個。</span><span class="sxs-lookup"><span data-stu-id="5c92c-355">The fourth `fixed` statement in the example above produces a similar result to the third.</span></span>

<span data-ttu-id="5c92c-356">此範例中的`fixed`陳述式會使用`string`:</span><span class="sxs-lookup"><span data-stu-id="5c92c-356">This example of the `fixed` statement uses `string`:</span></span>

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

<span data-ttu-id="5c92c-357">在 unsafe 內容中的一維陣列的陣列項目會儲存在遞增索引順序，從索引`0`和結束索引`Length - 1`。</span><span class="sxs-lookup"><span data-stu-id="5c92c-357">In an unsafe context array elements of single-dimensional arrays are stored in increasing index order, starting with index `0` and ending with index `Length - 1`.</span></span> <span data-ttu-id="5c92c-358">針對多維陣列，陣列項目會儲存，最右邊的維度的索引會增加第一次，然後下一步 左邊的維度，依此類推左邊。</span><span class="sxs-lookup"><span data-stu-id="5c92c-358">For multi-dimensional arrays, array elements are stored such that the indices of the rightmost dimension are increased first, then the next left dimension, and so on to the left.</span></span> <span data-ttu-id="5c92c-359">內`fixed`取得的指標的陳述式`p`陣列執行個體`a`，範圍從指標值`p`到`p + a.Length - 1`代表陣列中元素的位址。</span><span class="sxs-lookup"><span data-stu-id="5c92c-359">Within a `fixed` statement that obtains a pointer `p` to an array instance `a`, the pointer values ranging from `p` to `p + a.Length - 1` represent addresses of the elements in the array.</span></span> <span data-ttu-id="5c92c-360">同樣地，範圍介於變數`p[0]`至`p[a.Length - 1]`代表實際的陣列項目。</span><span class="sxs-lookup"><span data-stu-id="5c92c-360">Likewise, the variables ranging from `p[0]` to `p[a.Length - 1]` represent the actual array elements.</span></span> <span data-ttu-id="5c92c-361">給定陣列所儲存的方式，我們可以處理任何維度的陣列，就好像線性。</span><span class="sxs-lookup"><span data-stu-id="5c92c-361">Given the way in which arrays are stored, we can treat an array of any dimension as though it were linear.</span></span>

<span data-ttu-id="5c92c-362">例如：</span><span class="sxs-lookup"><span data-stu-id="5c92c-362">For example:</span></span>

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

<span data-ttu-id="5c92c-363">產生下列輸出：</span><span class="sxs-lookup"><span data-stu-id="5c92c-363">which produces the output:</span></span>

```
[0,0,0] =  0 [0,0,1] =  1 [0,0,2] =  2 [0,0,3] =  3
[0,1,0] =  4 [0,1,1] =  5 [0,1,2] =  6 [0,1,3] =  7
[0,2,0] =  8 [0,2,1] =  9 [0,2,2] = 10 [0,2,3] = 11
[1,0,0] = 12 [1,0,1] = 13 [1,0,2] = 14 [1,0,3] = 15
[1,1,0] = 16 [1,1,1] = 17 [1,1,2] = 18 [1,1,3] = 19
[1,2,0] = 20 [1,2,1] = 21 [1,2,2] = 22 [1,2,3] = 23
```

<span data-ttu-id="5c92c-364">在範例</span><span class="sxs-lookup"><span data-stu-id="5c92c-364">In the example</span></span>

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

<span data-ttu-id="5c92c-365">`fixed`陳述式用來修正陣列，因此它的位址可以傳遞至採用指標的方法。</span><span class="sxs-lookup"><span data-stu-id="5c92c-365">a `fixed` statement is used to fix an array so its address can be passed to a method that takes a pointer.</span></span>

<span data-ttu-id="5c92c-366">在下列範例中：</span><span class="sxs-lookup"><span data-stu-id="5c92c-366">In the example:</span></span>

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

<span data-ttu-id="5c92c-367">fixed 陳述式用來修正固定的大小緩衝區的結構，因此可以使用其位址的指標。</span><span class="sxs-lookup"><span data-stu-id="5c92c-367">a fixed statement is used to fix a fixed size buffer of a struct so its address can be used as a pointer.</span></span>

<span data-ttu-id="5c92c-368">A`char*`藉由修正一律指向以 null 結束的字串的字串執行個體所產生的值。</span><span class="sxs-lookup"><span data-stu-id="5c92c-368">A `char*` value produced by fixing a string instance always points to a null-terminated string.</span></span> <span data-ttu-id="5c92c-369">取得指標 fixed 陳述式內`p`字串執行個體`s`，範圍從指標值`p`來`p + s.Length - 1`代表字串中，而指標值中字元的位址`p + s.Length`永遠指向 null 字元 (具有值的字元`'\0'`)。</span><span class="sxs-lookup"><span data-stu-id="5c92c-369">Within a fixed statement that obtains a pointer `p` to a string instance `s`, the pointer values ranging from `p` to `p + s.Length - 1` represent addresses of the characters in the string, and the pointer value `p + s.Length` always points to a null character (the character with value `'\0'`).</span></span>

<span data-ttu-id="5c92c-370">透過固定的指標的 managed 類型的修改的物件可以產生未定義的行為。</span><span class="sxs-lookup"><span data-stu-id="5c92c-370">Modifying objects of managed type through fixed pointers can results in undefined behavior.</span></span> <span data-ttu-id="5c92c-371">例如，字串是不可變的因為它是程式設計人員必須負責確保不會修改為固定字串的指標所參考的字元。</span><span class="sxs-lookup"><span data-stu-id="5c92c-371">For example, because strings are immutable, it is the programmer's responsibility to ensure that the characters referenced by a pointer to a fixed string are not modified.</span></span>

<span data-ttu-id="5c92c-372">呼叫外部"C style"字串的 Api 時，自動 null 終止的字串會格外有用。</span><span class="sxs-lookup"><span data-stu-id="5c92c-372">The automatic null-termination of strings is particularly convenient when calling external APIs that expect "C-style" strings.</span></span> <span data-ttu-id="5c92c-373">不過請注意，字串執行個體可以含有 null 字元。</span><span class="sxs-lookup"><span data-stu-id="5c92c-373">Note, however, that a string instance is permitted to contain null characters.</span></span> <span data-ttu-id="5c92c-374">如果存在這類的 null 字元，字串會被截斷視為以 null 結束時`char*`。</span><span class="sxs-lookup"><span data-stu-id="5c92c-374">If such null characters are present, the string will appear truncated when treated as a null-terminated `char*`.</span></span>

## <a name="fixed-size-buffers"></a><span data-ttu-id="5c92c-375">固定的大小緩衝區</span><span class="sxs-lookup"><span data-stu-id="5c92c-375">Fixed size buffers</span></span>

<span data-ttu-id="5c92c-376">固定的大小緩衝區用來將"C style"內嵌陣列宣告為結構的成員，並且主要適用於與 unmanaged Api 互動。</span><span class="sxs-lookup"><span data-stu-id="5c92c-376">Fixed size buffers are used to declare "C style" in-line arrays as members of structs, and are primarily useful for interfacing with unmanaged APIs.</span></span>

### <a name="fixed-size-buffer-declarations"></a><span data-ttu-id="5c92c-377">固定的大小緩衝區宣告</span><span class="sxs-lookup"><span data-stu-id="5c92c-377">Fixed size buffer declarations</span></span>

<span data-ttu-id="5c92c-378">A***固定大小緩衝區***成員，表示儲存體的固定的長度的緩衝區的指定類型的變數。</span><span class="sxs-lookup"><span data-stu-id="5c92c-378">A ***fixed size buffer*** is a member that represents storage for a fixed length buffer of variables of a given type.</span></span> <span data-ttu-id="5c92c-379">固定的大小緩衝區宣告導入了一個或多個固定的大小的緩衝區的指定項目類型。</span><span class="sxs-lookup"><span data-stu-id="5c92c-379">A fixed size buffer declaration introduces one or more fixed size buffers of a given element type.</span></span> <span data-ttu-id="5c92c-380">只允許在結構宣告固定的大小緩衝區，並且只能在不安全的內容中 ([Unsafe 內容](unsafe-code.md#unsafe-contexts))。</span><span class="sxs-lookup"><span data-stu-id="5c92c-380">Fixed size buffers are only permitted in struct declarations and can only occur in unsafe contexts ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span>

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

<span data-ttu-id="5c92c-381">固定的大小緩衝區宣告可能包含一組屬性 ([屬性](attributes.md))，則`new`修飾詞 ([修飾詞](classes.md#modifiers))，是有效的四種存取修飾詞組合 ([類型參數和條件約束](classes.md#type-parameters-and-constraints)) 和`unsafe`修飾詞 ([Unsafe 內容](unsafe-code.md#unsafe-contexts))。</span><span class="sxs-lookup"><span data-stu-id="5c92c-381">A fixed size buffer declaration may include a set of attributes ([Attributes](attributes.md)), a `new` modifier ([Modifiers](classes.md#modifiers)), a valid combination of the four access modifiers ([Type parameters and constraints](classes.md#type-parameters-and-constraints)) and an `unsafe` modifier ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span> <span data-ttu-id="5c92c-382">屬性和修飾詞套用至所有固定的大小緩衝區宣告所宣告的成員。</span><span class="sxs-lookup"><span data-stu-id="5c92c-382">The attributes and modifiers apply to all of the members declared by the fixed size buffer declaration.</span></span> <span data-ttu-id="5c92c-383">它會在固定的大小緩衝區宣告中出現多次相同的修飾詞產生錯誤。</span><span class="sxs-lookup"><span data-stu-id="5c92c-383">It is an error for the same modifier to appear multiple times in a fixed size buffer declaration.</span></span>

<span data-ttu-id="5c92c-384">固定的大小緩衝區宣告不允許包含`static`修飾詞。</span><span class="sxs-lookup"><span data-stu-id="5c92c-384">A fixed size buffer declaration is not permitted to include the `static` modifier.</span></span>

<span data-ttu-id="5c92c-385">固定的大小緩衝區宣告的緩衝區項目類型指定緩衝區的項目類型，宣告所引進。</span><span class="sxs-lookup"><span data-stu-id="5c92c-385">The buffer element type of a fixed size buffer declaration specifies the element type of the buffer(s) introduced by the declaration.</span></span> <span data-ttu-id="5c92c-386">緩衝區元素類型必須是其中一個預先定義的型別`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char`， `float`， `double`，或`bool`。</span><span class="sxs-lookup"><span data-stu-id="5c92c-386">The buffer element type must be one of the predefined types `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, or `bool`.</span></span>

<span data-ttu-id="5c92c-387">緩衝區元素型別後面接著一份固定的大小緩衝區的宣告子，其中每一個導入了新的成員。</span><span class="sxs-lookup"><span data-stu-id="5c92c-387">The buffer element type is followed by a list of fixed size buffer declarators, each of which introduces a new member.</span></span> <span data-ttu-id="5c92c-388">固定的大小緩衝區宣告子包含名稱的成員，後面接著括住常數運算式的識別項`[`和`]`語彙基元。</span><span class="sxs-lookup"><span data-stu-id="5c92c-388">A fixed size buffer declarator consists of an identifier that names the member, followed by a constant expression enclosed in `[` and `]` tokens.</span></span> <span data-ttu-id="5c92c-389">常數運算式，即表示該固定的大小緩衝區宣告子所導入的成員中的項目數。</span><span class="sxs-lookup"><span data-stu-id="5c92c-389">The constant expression denotes the number of elements in the member introduced by that fixed size buffer declarator.</span></span> <span data-ttu-id="5c92c-390">常數運算式的類型必須隱含轉換成類型`int`，而且此值必須是非零的正整數。</span><span class="sxs-lookup"><span data-stu-id="5c92c-390">The type of the constant expression must be implicitly convertible to type `int`, and the value must be a non-zero positive integer.</span></span>

<span data-ttu-id="5c92c-391">固定的大小緩衝區的項目一定會循序配置在記憶體中。</span><span class="sxs-lookup"><span data-stu-id="5c92c-391">The elements of a fixed size buffer are guaranteed to be laid out sequentially in memory.</span></span>

<span data-ttu-id="5c92c-392">固定的大小緩衝區宣告會宣告多個固定的大小緩衝區就相當於單一的固定的大小緩衝區宣告具有相同的屬性和項目類型的多個宣告的。</span><span class="sxs-lookup"><span data-stu-id="5c92c-392">A fixed size buffer declaration that declares multiple fixed size buffers is equivalent to multiple declarations of a single fixed size buffer declaration with the same attributes, and element types.</span></span> <span data-ttu-id="5c92c-393">例如</span><span class="sxs-lookup"><span data-stu-id="5c92c-393">For example</span></span>

```csharp
unsafe struct A
{
   public fixed int x[5], y[10], z[100];
}
```

<span data-ttu-id="5c92c-394">相當於</span><span class="sxs-lookup"><span data-stu-id="5c92c-394">is equivalent to</span></span>

```csharp
unsafe struct A
{
   public fixed int x[5];
   public fixed int y[10];
   public fixed int z[100];
}
```

### <a name="fixed-size-buffers-in-expressions"></a><span data-ttu-id="5c92c-395">在運算式中的固定的大小緩衝區</span><span class="sxs-lookup"><span data-stu-id="5c92c-395">Fixed size buffers in expressions</span></span>

<span data-ttu-id="5c92c-396">成員查閱 ([運算子](expressions.md#operators)) 固定大小的緩衝區成員進行成員查詢欄位的完全相同。</span><span class="sxs-lookup"><span data-stu-id="5c92c-396">Member lookup ([Operators](expressions.md#operators)) of a fixed size buffer member proceeds exactly like member lookup of a field.</span></span>

<span data-ttu-id="5c92c-397">在運算式中使用，可參考的固定的大小緩衝區*simple_name* ([型別推斷](expressions.md#type-inference)) 或*member_access* ([編譯時間檢查動態的多載解析](expressions.md#compile-time-checking-of-dynamic-overload-resolution))。</span><span class="sxs-lookup"><span data-stu-id="5c92c-397">A fixed size buffer can be referenced in an expression using a *simple_name* ([Type inference](expressions.md#type-inference)) or a *member_access* ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span>

<span data-ttu-id="5c92c-398">當以簡單名稱參考固定的大小緩衝區成員時，效果等同於表單的成員存取`this.I`，其中`I`是固定的大小緩衝區。</span><span class="sxs-lookup"><span data-stu-id="5c92c-398">When a fixed size buffer member is referenced as a simple name, the effect is the same as a member access of the form `this.I`, where `I` is the fixed size buffer member.</span></span>

<span data-ttu-id="5c92c-399">在表單的成員存取`E.I`，如果`E`的結構類型和成員查閱`I`在於結構類型會識別固定的大小的成員，然後`E.I`是評估分類，如下所示：</span><span class="sxs-lookup"><span data-stu-id="5c92c-399">In a member access of the form `E.I`, if `E` is of a struct type and a member lookup of `I` in that struct type identifies a fixed size member, then `E.I` is evaluated an classified as follows:</span></span>

*  <span data-ttu-id="5c92c-400">如果運算式`E.I`不會發生在不安全的內容中，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="5c92c-400">If the expression `E.I` does not occur in an unsafe context, a compile-time error occurs.</span></span>
*  <span data-ttu-id="5c92c-401">如果`E`會分類為值，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="5c92c-401">If `E` is classified as a value, a compile-time error occurs.</span></span>
*  <span data-ttu-id="5c92c-402">否則，如果`E`是可移動的變數 ([固定和可移動變數](unsafe-code.md#fixed-and-moveable-variables)) 和運算式`E.I`不*fixed_pointer_initializer* ([固定陳述式](unsafe-code.md#the-fixed-statement))，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="5c92c-402">Otherwise, if `E` is a moveable variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)) and the expression `E.I` is not a *fixed_pointer_initializer* ([The fixed statement](unsafe-code.md#the-fixed-statement)), a compile-time error occurs.</span></span>
*  <span data-ttu-id="5c92c-403">否則，請`E`參考為固定的變數和運算式的結果是固定的大小緩衝區成員的第一個元素的指標`I`在`E`。</span><span class="sxs-lookup"><span data-stu-id="5c92c-403">Otherwise, `E` references a fixed variable and the result of the expression is a pointer to the first element of the fixed size buffer member `I` in `E`.</span></span> <span data-ttu-id="5c92c-404">結果會是類型`S*`，其中`S`的項目類型`I`，並會分類為值。</span><span class="sxs-lookup"><span data-stu-id="5c92c-404">The result is of type `S*`, where `S` is the element type of `I`, and is classified as a value.</span></span>

<span data-ttu-id="5c92c-405">固定的大小緩衝區的後續項目，可以使用指標作業，從第一個項目來存取。</span><span class="sxs-lookup"><span data-stu-id="5c92c-405">The subsequent elements of the fixed size buffer can be accessed using pointer operations from the first element.</span></span> <span data-ttu-id="5c92c-406">不同於陣列的存取權，存取權的固定的大小緩衝區項目是不安全的作業，並不檢查的範圍。</span><span class="sxs-lookup"><span data-stu-id="5c92c-406">Unlike access to arrays, access to the elements of a fixed size buffer is an unsafe operation and is not range checked.</span></span>

<span data-ttu-id="5c92c-407">下列範例會宣告，並使用固定的大小緩衝區成員的結構。</span><span class="sxs-lookup"><span data-stu-id="5c92c-407">The following example declares and uses a struct with a fixed size buffer member.</span></span>

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

### <a name="definite-assignment-checking"></a><span data-ttu-id="5c92c-408">明確設定檢查</span><span class="sxs-lookup"><span data-stu-id="5c92c-408">Definite assignment checking</span></span>

<span data-ttu-id="5c92c-409">固定的大小緩衝區並不受限於明確設定檢查 ([明確指派](variables.md#definite-assignment))，固定的大小緩衝區的成員也會檢查結構型別變數的明確指派的目的被忽略。</span><span class="sxs-lookup"><span data-stu-id="5c92c-409">Fixed size buffers are not subject to definite assignment checking ([Definite assignment](variables.md#definite-assignment)), and fixed size buffer members are ignored for purposes of definite assignment checking of struct type variables.</span></span>

<span data-ttu-id="5c92c-410">當最外層的固定的大小緩衝區成員包含結構變數的靜態變數、 類別執行個體或陣列元素的執行個體變數，固定的大小緩衝區的項目會自動初始化為其預設值 ([預設值](variables.md#default-values))。</span><span class="sxs-lookup"><span data-stu-id="5c92c-410">When the outermost containing struct variable of a fixed size buffer member is a static variable, an instance variable of a class instance, or an array element, the elements of the fixed size buffer are automatically initialized to their default values ([Default values](variables.md#default-values)).</span></span> <span data-ttu-id="5c92c-411">在其他情況下，固定的大小緩衝區的初始內容未定義。</span><span class="sxs-lookup"><span data-stu-id="5c92c-411">In all other cases, the initial content of a fixed size buffer is undefined.</span></span>

## <a name="stack-allocation"></a><span data-ttu-id="5c92c-412">堆疊配置</span><span class="sxs-lookup"><span data-stu-id="5c92c-412">Stack allocation</span></span>

<span data-ttu-id="5c92c-413">在 unsafe 內容中，本機變數宣告 ([區域變數宣告](statements.md#local-variable-declarations)) 可能包含的堆疊配置初始設定式在呼叫堆疊中所配置的記憶體。</span><span class="sxs-lookup"><span data-stu-id="5c92c-413">In an unsafe context, a local variable declaration ([Local variable declarations](statements.md#local-variable-declarations)) may include a stack allocation initializer which allocates memory from the call stack.</span></span>

```antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

<span data-ttu-id="5c92c-414">*Unmanaged_type*表示類型的項目會儲存在新配置的位置，而*運算式*指出這些項目數目。</span><span class="sxs-lookup"><span data-stu-id="5c92c-414">The *unmanaged_type* indicates the type of the items that will be stored in the newly allocated location, and the *expression* indicates the number of these items.</span></span> <span data-ttu-id="5c92c-415">這些結合起來，指定所需的配置大小。</span><span class="sxs-lookup"><span data-stu-id="5c92c-415">Taken together, these specify the required allocation size.</span></span> <span data-ttu-id="5c92c-416">堆疊配置的大小不能是負的因為它是編譯時期錯誤，以指定的項目數目*constant_expression*評估為負數值。</span><span class="sxs-lookup"><span data-stu-id="5c92c-416">Since the size of a stack allocation cannot be negative, it is a compile-time error to specify the number of items as a *constant_expression* that evaluates to a negative value.</span></span>

<span data-ttu-id="5c92c-417">表單的堆疊配置初始設定式`stackalloc T[E]`需要`T`非受控型別 ([指標型別](unsafe-code.md#pointer-types)) 和`E`是類型的運算式`int`。</span><span class="sxs-lookup"><span data-stu-id="5c92c-417">A stack allocation initializer of the form `stackalloc T[E]` requires `T` to be an unmanaged type ([Pointer types](unsafe-code.md#pointer-types)) and `E` to be an expression of type `int`.</span></span> <span data-ttu-id="5c92c-418">建構配置`E * sizeof(T)`位元組從呼叫堆疊，並傳回型別的指標`T*`，新配置的區塊。</span><span class="sxs-lookup"><span data-stu-id="5c92c-418">The construct allocates `E * sizeof(T)` bytes from the call stack and returns a pointer, of type `T*`, to the newly allocated block.</span></span> <span data-ttu-id="5c92c-419">如果`E`是負值，則行為是未定義。</span><span class="sxs-lookup"><span data-stu-id="5c92c-419">If `E` is a negative value, then the behavior is undefined.</span></span> <span data-ttu-id="5c92c-420">如果`E`為零，則會進行任何配置，並傳回的指標，是由實作定義。</span><span class="sxs-lookup"><span data-stu-id="5c92c-420">If `E` is zero, then no allocation is made, and the pointer returned is implementation-defined.</span></span> <span data-ttu-id="5c92c-421">如果不是記憶體不足，無法配置指定大小的區塊`System.StackOverflowException`就會擲回。</span><span class="sxs-lookup"><span data-stu-id="5c92c-421">If there is not enough memory available to allocate a block of the given size, a `System.StackOverflowException` is thrown.</span></span>

<span data-ttu-id="5c92c-422">新配置內容會是記憶體的未定義。</span><span class="sxs-lookup"><span data-stu-id="5c92c-422">The content of the newly allocated memory is undefined.</span></span>

<span data-ttu-id="5c92c-423">堆疊配置的初始設定式中不允許`catch`或是`finally`區塊 ([try 陳述式](statements.md#the-try-statement))。</span><span class="sxs-lookup"><span data-stu-id="5c92c-423">Stack allocation initializers are not permitted in `catch` or `finally` blocks ([The try statement](statements.md#the-try-statement)).</span></span>

<span data-ttu-id="5c92c-424">沒有任何方法，明確釋放出記憶體配置使用`stackalloc`。</span><span class="sxs-lookup"><span data-stu-id="5c92c-424">There is no way to explicitly free memory allocated using `stackalloc`.</span></span> <span data-ttu-id="5c92c-425">該函式成員傳回時，會自動捨棄函式成員的執行期間建立的所有堆疊配置的記憶體區塊。</span><span class="sxs-lookup"><span data-stu-id="5c92c-425">All stack allocated memory blocks created during the execution of a function member are automatically discarded when that function member returns.</span></span> <span data-ttu-id="5c92c-426">這會對應至`alloca`函式，C 中經常發現的延伸模組和C++實作。</span><span class="sxs-lookup"><span data-stu-id="5c92c-426">This corresponds to the `alloca` function, an extension commonly found in C and C++ implementations.</span></span>

<span data-ttu-id="5c92c-427">在範例</span><span class="sxs-lookup"><span data-stu-id="5c92c-427">In the example</span></span>

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

<span data-ttu-id="5c92c-428">`stackalloc`初始設定式會在`IntToString`方法來配置堆疊上的 16 個字元的緩衝區。</span><span class="sxs-lookup"><span data-stu-id="5c92c-428">a `stackalloc` initializer is used in the `IntToString` method to allocate a buffer of 16 characters on the stack.</span></span> <span data-ttu-id="5c92c-429">方法傳回時，緩衝區會自動被捨棄。</span><span class="sxs-lookup"><span data-stu-id="5c92c-429">The buffer is automatically discarded when the method returns.</span></span>

## <a name="dynamic-memory-allocation"></a><span data-ttu-id="5c92c-430">動態記憶體配置</span><span class="sxs-lookup"><span data-stu-id="5c92c-430">Dynamic memory allocation</span></span>

<span data-ttu-id="5c92c-431">除了`stackalloc`運算子，C# 提供任何預先定義的建構來管理非記憶體回收回收的記憶體。</span><span class="sxs-lookup"><span data-stu-id="5c92c-431">Except for the `stackalloc` operator, C# provides no predefined constructs for managing non-garbage collected memory.</span></span> <span data-ttu-id="5c92c-432">這類服務通常會提供支援類別庫，或直接從基礎作業系統匯入。</span><span class="sxs-lookup"><span data-stu-id="5c92c-432">Such services are typically provided by supporting class libraries or imported directly from the underlying operating system.</span></span> <span data-ttu-id="5c92c-433">比方說，`Memory`下列類別示範從 C# 存取堆積函式的基礎作業系統可能方式：</span><span class="sxs-lookup"><span data-stu-id="5c92c-433">For example, the `Memory` class below illustrates how the heap functions of an underlying operating system might be accessed from C#:</span></span>

```csharp
using System;
using System.Runtime.InteropServices;

public unsafe class Memory
{
    // Handle for the process heap. This handle is used in all calls to the
    // HeapXXX APIs in the methods below.
    static int ph = GetProcessHeap();

    // Private instance constructor to prevent instantiation.
    private Memory() {}

    // Allocates a memory block of the given size. The allocated memory is
    // automatically initialized to zero.
    public static void* Alloc(int size) {
        void* result = HeapAlloc(ph, HEAP_ZERO_MEMORY, size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Copies count bytes from src to dst. The source and destination
    // blocks are permitted to overlap.
    public static void Copy(void* src, void* dst, int count) {
        byte* ps = (byte*)src;
        byte* pd = (byte*)dst;
        if (ps > pd) {
            for (; count != 0; count--) *pd++ = *ps++;
        }
        else if (ps < pd) {
            for (ps += count, pd += count; count != 0; count--) *--pd = *--ps;
        }
    }

    // Frees a memory block.
    public static void Free(void* block) {
        if (!HeapFree(ph, 0, block)) throw new InvalidOperationException();
    }

    // Re-allocates a memory block. If the reallocation request is for a
    // larger size, the additional region of memory is automatically
    // initialized to zero.
    public static void* ReAlloc(void* block, int size) {
        void* result = HeapReAlloc(ph, HEAP_ZERO_MEMORY, block, size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Returns the size of a memory block.
    public static int SizeOf(void* block) {
        int result = HeapSize(ph, 0, block);
        if (result == -1) throw new InvalidOperationException();
        return result;
    }

    // Heap API flags
    const int HEAP_ZERO_MEMORY = 0x00000008;

    // Heap API functions
    [DllImport("kernel32")]
    static extern int GetProcessHeap();

    [DllImport("kernel32")]
    static extern void* HeapAlloc(int hHeap, int flags, int size);

    [DllImport("kernel32")]
    static extern bool HeapFree(int hHeap, int flags, void* block);

    [DllImport("kernel32")]
    static extern void* HeapReAlloc(int hHeap, int flags, void* block, int size);

    [DllImport("kernel32")]
    static extern int HeapSize(int hHeap, int flags, void* block);
}
```

<span data-ttu-id="5c92c-434">範例會使用`Memory`類別如下：</span><span class="sxs-lookup"><span data-stu-id="5c92c-434">An example that uses the `Memory` class is given below:</span></span>

```csharp
class Test
{
    static void Main() {
        unsafe {
            byte* buffer = (byte*)Memory.Alloc(256);
            try {
                for (int i = 0; i < 256; i++) buffer[i] = (byte)i;
                byte[] array = new byte[256];
                fixed (byte* p = array) Memory.Copy(buffer, p, 256); 
            }
            finally {
                Memory.Free(buffer);
            }
            for (int i = 0; i < 256; i++) Console.WriteLine(array[i]);
        }
    }
}
```

<span data-ttu-id="5c92c-435">此範例會配置 256 個位元組的記憶體，透過`Memory.Alloc`並增加從 0 到 255 之間的值初始化的記憶體區塊。</span><span class="sxs-lookup"><span data-stu-id="5c92c-435">The example allocates 256 bytes of memory through `Memory.Alloc` and initializes the memory block with values increasing from 0 to 255.</span></span> <span data-ttu-id="5c92c-436">然後會配置 256 元素位元組陣列，並使用`Memory.Copy`複製位元組陣列的記憶體區塊的內容。</span><span class="sxs-lookup"><span data-stu-id="5c92c-436">It then allocates a 256 element byte array and uses `Memory.Copy` to copy the contents of the memory block into the byte array.</span></span> <span data-ttu-id="5c92c-437">最後，使用釋放記憶體區塊`Memory.Free`和位元組陣列的內容會在主控台上的輸出。</span><span class="sxs-lookup"><span data-stu-id="5c92c-437">Finally, the memory block is freed using `Memory.Free` and the contents of the byte array are output on the console.</span></span>
