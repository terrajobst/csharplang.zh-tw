# <a name="namespaces"></a><span data-ttu-id="26e47-101">命名空間</span><span class="sxs-lookup"><span data-stu-id="26e47-101">Namespaces</span></span>

<span data-ttu-id="26e47-102">C# 程式組織方式使用命名空間。</span><span class="sxs-lookup"><span data-stu-id="26e47-102">C# programs are organized using namespaces.</span></span> <span data-ttu-id="26e47-103">命名空間用作為程式，「 內部 」 的組織系統和 「 外部 」 的組織系統 — 一種呈現會公開至其他程式的程式項目。</span><span class="sxs-lookup"><span data-stu-id="26e47-103">Namespaces are used both as an "internal" organization system for a program, and as an "external" organization system—a way of presenting program elements that are exposed to other programs.</span></span>

<span data-ttu-id="26e47-104">Using 指示詞 ([Using 指示詞](namespaces.md#using-directives)) 提供給方便使用的命名空間。</span><span class="sxs-lookup"><span data-stu-id="26e47-104">Using directives ([Using directives](namespaces.md#using-directives)) are provided to facilitate the use of namespaces.</span></span>

## <a name="compilation-units"></a><span data-ttu-id="26e47-105">編譯單位</span><span class="sxs-lookup"><span data-stu-id="26e47-105">Compilation units</span></span>

<span data-ttu-id="26e47-106">A *compilation_unit*定義原始程式檔的整體結構。</span><span class="sxs-lookup"><span data-stu-id="26e47-106">A *compilation_unit* defines the overall structure of a source file.</span></span> <span data-ttu-id="26e47-107">編譯單位包含零或多個*using_directive*s 後面接著零或多個*global_attributes*後面接著零或多個*namespace_member_declaration*s.</span><span class="sxs-lookup"><span data-stu-id="26e47-107">A compilation unit consists of zero or more *using_directive*s followed by zero or more *global_attributes* followed by zero or more *namespace_member_declaration*s.</span></span>

```antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? namespace_member_declaration*
    ;
```

<span data-ttu-id="26e47-108">C# 程式包含了一個或多個編譯單位，每一個都包含不同的原始程式檔中。</span><span class="sxs-lookup"><span data-stu-id="26e47-108">A C# program consists of one or more compilation units, each contained in a separate source file.</span></span> <span data-ttu-id="26e47-109">C# 程式編譯時，所有的編譯單位都會一起處理。</span><span class="sxs-lookup"><span data-stu-id="26e47-109">When a C# program is compiled, all of the compilation units are processed together.</span></span> <span data-ttu-id="26e47-110">因此，編譯單位可以彼此相依，可能是以循環方式。</span><span class="sxs-lookup"><span data-stu-id="26e47-110">Thus, compilation units can depend on each other, possibly in a circular fashion.</span></span>

<span data-ttu-id="26e47-111">*Using_directive*秒的編譯單位會影響*global_attributes*並*namespace_member_declaration*該編譯單位中，但有任何作用其他的編譯單位。</span><span class="sxs-lookup"><span data-stu-id="26e47-111">The *using_directive*s of a compilation unit affect the *global_attributes* and *namespace_member_declaration*s of that compilation unit, but have no effect on other compilation units.</span></span>

<span data-ttu-id="26e47-112">*Global_attributes* ([屬性](attributes.md)) 的編譯單位允許的目標組件和模組的屬性規格。</span><span class="sxs-lookup"><span data-stu-id="26e47-112">The *global_attributes* ([Attributes](attributes.md)) of a compilation unit permit the specification of attributes for the target assembly and module.</span></span> <span data-ttu-id="26e47-113">組件和模組做為型別的實體容器。</span><span class="sxs-lookup"><span data-stu-id="26e47-113">Assemblies and modules act as physical containers for types.</span></span> <span data-ttu-id="26e47-114">組件可能包含數個實際分開的模組。</span><span class="sxs-lookup"><span data-stu-id="26e47-114">An assembly may consist of several physically separate modules.</span></span>

<span data-ttu-id="26e47-115">*Namespace_member_declaration*之程式的每個編譯單位參與稱為全域命名空間的單一宣告空間的成員。</span><span class="sxs-lookup"><span data-stu-id="26e47-115">The *namespace_member_declaration*s of each compilation unit of a program contribute members to a single declaration space called the global namespace.</span></span> <span data-ttu-id="26e47-116">例如: </span><span class="sxs-lookup"><span data-stu-id="26e47-116">For example:</span></span>

<span data-ttu-id="26e47-117">檔案`A.cs`:</span><span class="sxs-lookup"><span data-stu-id="26e47-117">File `A.cs`:</span></span>
```csharp
class A {}
```

<span data-ttu-id="26e47-118">檔案`B.cs`:</span><span class="sxs-lookup"><span data-stu-id="26e47-118">File `B.cs`:</span></span>
```csharp
class B {}
```

<span data-ttu-id="26e47-119">兩個編譯單位參與單一全域命名空間，在此情況下宣告兩個類別的完整格式名稱具有`A`和`B`。</span><span class="sxs-lookup"><span data-stu-id="26e47-119">The two compilation units contribute to the single global namespace, in this case declaring two classes with the fully qualified names `A` and `B`.</span></span> <span data-ttu-id="26e47-120">因為兩個編譯單位參與相同的宣告空間，就會發生錯誤如果每一個包含成員具有相同名稱的宣告。</span><span class="sxs-lookup"><span data-stu-id="26e47-120">Because the two compilation units contribute to the same declaration space, it would have been an error if each contained a declaration of a member with the same name.</span></span>

## <a name="namespace-declarations"></a><span data-ttu-id="26e47-121">命名空間宣告</span><span class="sxs-lookup"><span data-stu-id="26e47-121">Namespace declarations</span></span>

<span data-ttu-id="26e47-122">A *namespace_declaration*組成關鍵字`namespace`，後面加上命名空間名稱和主體，或者後面接著一個分號。</span><span class="sxs-lookup"><span data-stu-id="26e47-122">A *namespace_declaration* consists of the keyword `namespace`, followed by a namespace name and body, optionally followed by a semicolon.</span></span>

```antlr
namespace_declaration
    : 'namespace' qualified_identifier namespace_body ';'?
    ;

qualified_identifier
    : identifier ('.' identifier)*
    ;

namespace_body
    : '{' extern_alias_directive* using_directive* namespace_member_declaration* '}'
    ;
```

<span data-ttu-id="26e47-123">A *namespace_declaration*中的最上層宣告可能會發生*compilation_unit*或在另一個成員宣告*namespace_declaration*。</span><span class="sxs-lookup"><span data-stu-id="26e47-123">A *namespace_declaration* may occur as a top-level declaration in a *compilation_unit* or as a member declaration within another *namespace_declaration*.</span></span> <span data-ttu-id="26e47-124">當*namespace_declaration*中的最上層宣告，就會發生*compilation_unit*，命名空間會變成全域命名空間的成員。</span><span class="sxs-lookup"><span data-stu-id="26e47-124">When a *namespace_declaration* occurs as a top-level declaration in a *compilation_unit*, the namespace becomes a member of the global namespace.</span></span> <span data-ttu-id="26e47-125">當*namespace_declaration*就會發生在另一個*namespace_declaration*，內部的命名空間會變成外部命名空間的成員。</span><span class="sxs-lookup"><span data-stu-id="26e47-125">When a *namespace_declaration* occurs within another *namespace_declaration*, the inner namespace becomes a member of the outer namespace.</span></span> <span data-ttu-id="26e47-126">在任一情況下，必須是包含命名空間中的唯一命名空間名稱。</span><span class="sxs-lookup"><span data-stu-id="26e47-126">In either case, the name of a namespace must be unique within the containing namespace.</span></span>

<span data-ttu-id="26e47-127">命名空間都是隱含`public`和命名空間宣告不能包含任何存取修飾詞。</span><span class="sxs-lookup"><span data-stu-id="26e47-127">Namespaces are implicitly `public` and the declaration of a namespace cannot include any access modifiers.</span></span>

<span data-ttu-id="26e47-128">內*namespace_body*，選擇性*using_directive*s 匯入其他命名空間、 類型和成員，以便直接而不是透過限定名稱所參考的名稱。</span><span class="sxs-lookup"><span data-stu-id="26e47-128">Within a *namespace_body*, the optional *using_directive*s import the names of other namespaces, types and members, allowing them to be referenced directly instead of through qualified names.</span></span> <span data-ttu-id="26e47-129">選擇性*namespace_member_declaration*s 提供的命名空間的宣告空間的成員。</span><span class="sxs-lookup"><span data-stu-id="26e47-129">The optional *namespace_member_declaration*s contribute members to the declaration space of the namespace.</span></span> <span data-ttu-id="26e47-130">請注意，所有*using_directive*s 必須出現在成員的任何宣告之前。</span><span class="sxs-lookup"><span data-stu-id="26e47-130">Note that all *using_directive*s must appear before any member declarations.</span></span>

<span data-ttu-id="26e47-131">*Qualified_identifier*的*namespace_declaration*以單一識別項或分隔的識別項的序列可能是 「`.`"語彙基元。</span><span class="sxs-lookup"><span data-stu-id="26e47-131">The *qualified_identifier* of a *namespace_declaration* may be a single identifier or a sequence of identifiers separated by "`.`" tokens.</span></span> <span data-ttu-id="26e47-132">第二個表單允許程式以定義巢狀命名空間，而不需要語彙巢狀數個命名空間宣告。</span><span class="sxs-lookup"><span data-stu-id="26e47-132">The latter form permits a program to define a nested namespace without lexically nesting several namespace declarations.</span></span> <span data-ttu-id="26e47-133">例如，套用至物件的</span><span class="sxs-lookup"><span data-stu-id="26e47-133">For example,</span></span>

```csharp
namespace N1.N2
{
    class A {}

    class B {}
}
```
<span data-ttu-id="26e47-134">在語意上等於</span><span class="sxs-lookup"><span data-stu-id="26e47-134">is semantically equivalent to</span></span>
```csharp
namespace N1
{
    namespace N2
    {
        class A {}

        class B {}
    }
}
```

<span data-ttu-id="26e47-135">命名空間是無限的但兩個具有相同的完整限定名稱的命名空間宣告參與相同的宣告空間 ([宣告](basic-concepts.md#declarations))。</span><span class="sxs-lookup"><span data-stu-id="26e47-135">Namespaces are open-ended, and two namespace declarations with the same fully qualified name contribute to the same declaration space ([Declarations](basic-concepts.md#declarations)).</span></span> <span data-ttu-id="26e47-136">在範例</span><span class="sxs-lookup"><span data-stu-id="26e47-136">In the example</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N1.N2
{
    class B {}
}
```
<span data-ttu-id="26e47-137">上面的兩個命名空間宣告提供給相同的宣告空間，在此情況下宣告兩個類別的完整格式名稱具有`N1.N2.A`和`N1.N2.B`。</span><span class="sxs-lookup"><span data-stu-id="26e47-137">the two namespace declarations above contribute to the same declaration space, in this case declaring two classes with the fully qualified names `N1.N2.A` and `N1.N2.B`.</span></span> <span data-ttu-id="26e47-138">因為這兩種宣告參與相同的宣告空間，就會發生錯誤，如果每個都包含具有相同名稱的成員的宣告。</span><span class="sxs-lookup"><span data-stu-id="26e47-138">Because the two declarations contribute to the same declaration space, it would have been an error if each contained a declaration of a member with the same name.</span></span>

## <a name="extern-aliases"></a><span data-ttu-id="26e47-139">外部別名</span><span class="sxs-lookup"><span data-stu-id="26e47-139">Extern aliases</span></span>

<span data-ttu-id="26e47-140">*Extern_alias_directive*引進識別項，做為命名空間的別名。</span><span class="sxs-lookup"><span data-stu-id="26e47-140">An *extern_alias_directive* introduces an identifier that serves as an alias for a namespace.</span></span> <span data-ttu-id="26e47-141">別名命名空間的規格是外部程式的原始程式碼，而且也適用於巢狀命名空間的別名命名空間。</span><span class="sxs-lookup"><span data-stu-id="26e47-141">The specification of the aliased namespace is external to the source code of the program and applies also to nested namespaces of the aliased namespace.</span></span>

```antlr
extern_alias_directive
    : 'extern' 'alias' identifier ';'
    ;
```

<span data-ttu-id="26e47-142">範圍*extern_alias_directive*擴充*using_directive*s *global_attributes*並*namespace_member_declaration*立即包含編譯單位或命名空間主體的 s。</span><span class="sxs-lookup"><span data-stu-id="26e47-142">The scope of an *extern_alias_directive* extends over the *using_directive*s, *global_attributes* and *namespace_member_declaration*s of its immediately containing compilation unit or namespace body.</span></span>

<span data-ttu-id="26e47-143">在編譯單位或命名空間主體中包含*extern_alias_directive*，所導入的識別項*extern_alias_directive*可用來參考別名命名空間。</span><span class="sxs-lookup"><span data-stu-id="26e47-143">Within a compilation unit or namespace body that contains an *extern_alias_directive*, the identifier introduced by the *extern_alias_directive* can be used to reference the aliased namespace.</span></span> <span data-ttu-id="26e47-144">它是編譯時期錯誤*識別碼*是 word `global`。</span><span class="sxs-lookup"><span data-stu-id="26e47-144">It is a compile-time error for the *identifier* to be the word `global`.</span></span>

<span data-ttu-id="26e47-145">*Extern_alias_directive*讓別名可在特定的編譯單位或命名空間主體中，但它並沒有提供給基礎的宣告空間的任何新成員。</span><span class="sxs-lookup"><span data-stu-id="26e47-145">An *extern_alias_directive* makes an alias available within a particular compilation unit or namespace body, but it does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="26e47-146">亦即*extern_alias_directive*不是可轉移的但相反地，會影響僅編譯單位或命名空間主體中發生。</span><span class="sxs-lookup"><span data-stu-id="26e47-146">In other words, an *extern_alias_directive* is not transitive, but, rather, affects only the compilation unit or namespace body in which it occurs.</span></span>

<span data-ttu-id="26e47-147">下列程式會宣告並使用兩個外部別名`X`和`Y`，每個代表不同的命名空間階層的根的：</span><span class="sxs-lookup"><span data-stu-id="26e47-147">The following program declares and uses two extern aliases, `X` and `Y`, each of which represent the root of a distinct namespace hierarchy:</span></span>
```csharp
extern alias X;
extern alias Y;

class Test
{
    X::N.A a;
    X::N.B b1;
    Y::N.B b2;
    Y::N.C c;
}
```

<span data-ttu-id="26e47-148">這個程式會宣告是否存在外部別名`X`和`Y`，但實際的別名定義是外部程式。</span><span class="sxs-lookup"><span data-stu-id="26e47-148">The program declares the existence of the extern aliases `X` and `Y`, but the actual definitions of the aliases are external to the program.</span></span> <span data-ttu-id="26e47-149">相同具名`N.B`類別現在可做為參考`X.N.B`並`Y.N.B`，或者，您也可以使用命名空間別名限定詞，`X::N.B`和`Y::N.B`。</span><span class="sxs-lookup"><span data-stu-id="26e47-149">The identically named `N.B` classes can now be referenced as `X.N.B` and `Y.N.B`, or, using the namespace alias qualifier, `X::N.B` and `Y::N.B`.</span></span> <span data-ttu-id="26e47-150">如果程式宣告外部別名提供沒有外部的定義，就會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="26e47-150">An error occurs if a program declares an extern alias for which no external definition is provided.</span></span>

## <a name="using-directives"></a><span data-ttu-id="26e47-151">using 指示詞</span><span class="sxs-lookup"><span data-stu-id="26e47-151">Using directives</span></span>

<span data-ttu-id="26e47-152">***Using 指示詞***方便使用的命名空間和其他命名空間中定義的類型。</span><span class="sxs-lookup"><span data-stu-id="26e47-152">***Using directives*** facilitate the use of namespaces and types defined in other namespaces.</span></span> <span data-ttu-id="26e47-153">使用指示詞影響的名稱解析程序*namespace_or_type_name*s ([命名空間和類型名稱](basic-concepts.md#namespace-and-type-names)) 和*simple_name*s ([簡單名稱](expressions.md#simple-names))，但不同於 using 指示詞的宣告，並不會提供基礎的宣告空間的編譯單位或在其中使用的命名空間的新成員。</span><span class="sxs-lookup"><span data-stu-id="26e47-153">Using directives impact the name resolution process of *namespace_or_type_name*s ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) and *simple_name*s ([Simple names](expressions.md#simple-names)), but unlike declarations, using directives do not contribute new members to the underlying declaration spaces of the compilation units or namespaces within which they are used.</span></span>

```antlr
using_directive
    : using_alias_directive
    | using_namespace_directive
    | using_static_directive
    ;
```

<span data-ttu-id="26e47-154">A *using_alias_directive* ([Using 別名指示詞](namespaces.md#using-alias-directives)) 導入了命名空間或類型的別名。</span><span class="sxs-lookup"><span data-stu-id="26e47-154">A *using_alias_directive* ([Using alias directives](namespaces.md#using-alias-directives)) introduces an alias for a namespace or type.</span></span>

<span data-ttu-id="26e47-155">A *using_namespace_directive* ([Using 命名空間指示詞](namespaces.md#using-namespace-directives)) 匯入命名空間的型別成員。</span><span class="sxs-lookup"><span data-stu-id="26e47-155">A *using_namespace_directive* ([Using namespace directives](namespaces.md#using-namespace-directives)) imports the type members of a namespace.</span></span>

<span data-ttu-id="26e47-156">A *using_static_directive* ([Using 靜態指示詞](namespaces.md#using-static-directives)) 匯入巢狀的類型和類型的靜態成員。</span><span class="sxs-lookup"><span data-stu-id="26e47-156">A *using_static_directive* ([Using static directives](namespaces.md#using-static-directives)) imports the nested types and static members of a type.</span></span>

<span data-ttu-id="26e47-157">範圍*using_directive*擴充*namespace_member_declaration*立即包含編譯單位或命名空間主體的 s。</span><span class="sxs-lookup"><span data-stu-id="26e47-157">The scope of a *using_directive* extends over the *namespace_member_declaration*s of its immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="26e47-158">範圍*using_directive*特別不包含其對等*using_directive*s。</span><span class="sxs-lookup"><span data-stu-id="26e47-158">The scope of a *using_directive* specifically does not include its peer *using_directive*s.</span></span> <span data-ttu-id="26e47-159">因此，對等互連*using_directive*s 並不會影響彼此，並在其中寫入的順序不重要。</span><span class="sxs-lookup"><span data-stu-id="26e47-159">Thus, peer *using_directive*s do not affect each other, and the order in which they are written is insignificant.</span></span>

### <a name="using-alias-directives"></a><span data-ttu-id="26e47-160">Using 別名指示詞</span><span class="sxs-lookup"><span data-stu-id="26e47-160">Using alias directives</span></span>

<span data-ttu-id="26e47-161">A *using_alias_directive*導入做為別名的命名空間或型別，直接封入的編譯單位或命名空間主體中的識別項。</span><span class="sxs-lookup"><span data-stu-id="26e47-161">A *using_alias_directive* introduces an identifier that serves as an alias for a namespace or type within the immediately enclosing compilation unit or namespace body.</span></span>

```antlr
using_alias_directive
    : 'using' identifier '=' namespace_or_type_name ';'
    ;
```

<span data-ttu-id="26e47-162">成員宣告中包含的編譯單位或命名空間主體*using_alias_directive*，所導入的識別項*using_alias_directive*可以用來參考指定命名空間或類型。</span><span class="sxs-lookup"><span data-stu-id="26e47-162">Within member declarations in a compilation unit or namespace body that contains a *using_alias_directive*, the identifier introduced by the *using_alias_directive* can be used to reference the given namespace or type.</span></span> <span data-ttu-id="26e47-163">例如: </span><span class="sxs-lookup"><span data-stu-id="26e47-163">For example:</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using A = N1.N2.A;

    class B: A {}
}
```

<span data-ttu-id="26e47-164">上述成員宣告中`N3`命名空間`A`為其別名`N1.N2.A`，因此類別`N3.B`衍生自類別`N1.N2.A`。</span><span class="sxs-lookup"><span data-stu-id="26e47-164">Above, within member declarations in the `N3` namespace, `A` is an alias for `N1.N2.A`, and thus class `N3.B` derives from class `N1.N2.A`.</span></span> <span data-ttu-id="26e47-165">可取得相同的效果，請建立別名`R`for `N1.N2` ，接著參考`R.A`:</span><span class="sxs-lookup"><span data-stu-id="26e47-165">The same effect can be obtained by creating an alias `R` for `N1.N2` and then referencing `R.A`:</span></span>
```csharp
namespace N3
{
    using R = N1.N2;

    class B: R.A {}
}
```

<span data-ttu-id="26e47-166">*識別碼*的*using_alias_directive*編譯單位或立即包含命名空間的宣告空間內必須是唯一*using_alias_directive*.</span><span class="sxs-lookup"><span data-stu-id="26e47-166">The *identifier* of a *using_alias_directive* must be unique within the declaration space of the compilation unit or namespace that immediately contains the *using_alias_directive*.</span></span> <span data-ttu-id="26e47-167">例如: </span><span class="sxs-lookup"><span data-stu-id="26e47-167">For example:</span></span>
```csharp
namespace N3
{
    class A {}
}

namespace N3
{
    using A = N1.N2.A;        // Error, A already exists
}
```

<span data-ttu-id="26e47-168">上述`N3`已包含的成員`A`，因此它是編譯時期錯誤*using_alias_directive*使用該識別項。</span><span class="sxs-lookup"><span data-stu-id="26e47-168">Above, `N3` already contains a member `A`, so it is a compile-time error for a *using_alias_directive* to use that identifier.</span></span> <span data-ttu-id="26e47-169">同樣地，它是兩個以上的編譯時期錯誤*using_alias_directive*相同編譯單位或命名空間主體內宣告相同名稱的別名。</span><span class="sxs-lookup"><span data-stu-id="26e47-169">Likewise, it is a compile-time error for two or more *using_alias_directive*s in the same compilation unit or namespace body to declare aliases by the same name.</span></span>

<span data-ttu-id="26e47-170">A *using_alias_directive*讓別名可在特定的編譯單位或命名空間主體中，但它並沒有提供給基礎的宣告空間的任何新成員。</span><span class="sxs-lookup"><span data-stu-id="26e47-170">A *using_alias_directive* makes an alias available within a particular compilation unit or namespace body, but it does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="26e47-171">亦即*using_alias_directive*並非可轉移而是會影響僅編譯單位或命名空間主體中發生。</span><span class="sxs-lookup"><span data-stu-id="26e47-171">In other words, a *using_alias_directive* is not transitive but rather affects only the compilation unit or namespace body in which it occurs.</span></span> <span data-ttu-id="26e47-172">在範例</span><span class="sxs-lookup"><span data-stu-id="26e47-172">In the example</span></span>
```csharp
namespace N3
{
    using R = N1.N2;
}

namespace N3
{
    class B: R.A {}            // Error, R unknown
}
```
<span data-ttu-id="26e47-173">範圍*using_alias_directive*導入`R`只會延伸到其中它包含，命名空間內的成員宣告讓`R`不明第二個命名空間宣告中。</span><span class="sxs-lookup"><span data-stu-id="26e47-173">the scope of the *using_alias_directive* that introduces `R` only extends to member declarations in the namespace body in which it is contained, so `R` is unknown in the second namespace declaration.</span></span> <span data-ttu-id="26e47-174">不過，放置*using_alias_directive*包含編譯單位會造成要供這兩個命名空間宣告中的別名：</span><span class="sxs-lookup"><span data-stu-id="26e47-174">However, placing the *using_alias_directive* in the containing compilation unit causes the alias to become available within both namespace declarations:</span></span>
```csharp
using R = N1.N2;

namespace N3
{
    class B: R.A {}
}

namespace N3
{
    class C: R.A {}
}
```

<span data-ttu-id="26e47-175">就像一般的成員名稱則是所引進*using_alias_directive*s 會隱藏名稱相似的成員，在巢狀範圍中。</span><span class="sxs-lookup"><span data-stu-id="26e47-175">Just like regular members, names introduced by *using_alias_directive*s are hidden by similarly named members in nested scopes.</span></span> <span data-ttu-id="26e47-176">在範例</span><span class="sxs-lookup"><span data-stu-id="26e47-176">In the example</span></span>
```csharp
using R = N1.N2;

namespace N3
{
    class R {}

    class B: R.A {}        // Error, R has no member A
}
```
<span data-ttu-id="26e47-177">參考`R.A`中的宣告`B`造成編譯時期錯誤，因為`R`是指`N3.R`，而非`N1.N2`。</span><span class="sxs-lookup"><span data-stu-id="26e47-177">the reference to `R.A` in the declaration of `B` causes a compile-time error because `R` refers to `N3.R`, not `N1.N2`.</span></span>

<span data-ttu-id="26e47-178">順序*using_alias_directive*s 會寫入不具有任何重要性，並解決*namespace_or_type_name*所參考*using_alias_directive*不會受到*using_alias_directive*本身或其他*using_directive*立即包含編譯單位或命名空間主體內。</span><span class="sxs-lookup"><span data-stu-id="26e47-178">The order in which *using_alias_directive*s are written has no significance, and resolution of the *namespace_or_type_name* referenced by a *using_alias_directive* is not affected by the *using_alias_directive* itself or by other *using_directive*s in the immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="26e47-179">亦即*namespace_or_type_name*的*using_alias_directive*如同立即包含的編譯單位或命名空間主體有不是解析*using_directive*s。</span><span class="sxs-lookup"><span data-stu-id="26e47-179">In other words, the *namespace_or_type_name* of a *using_alias_directive* is resolved as if the immediately containing compilation unit or namespace body had no *using_directive*s.</span></span> <span data-ttu-id="26e47-180">A *using_alias_directive*不過可能會受到*extern_alias_directive*立即包含編譯單位或命名空間主體內。</span><span class="sxs-lookup"><span data-stu-id="26e47-180">A *using_alias_directive* may however be affected by *extern_alias_directive*s in the immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="26e47-181">在範例</span><span class="sxs-lookup"><span data-stu-id="26e47-181">In the example</span></span>
```csharp
namespace N1.N2 {}

namespace N3
{
    extern alias E;

    using R1 = E.N;        // OK

    using R2 = N1;         // OK

    using R3 = N1.N2;      // OK

    using R4 = R2.N2;      // Error, R2 unknown
}
```
<span data-ttu-id="26e47-182">上次*using_alias_directive*導致編譯時期錯誤，因為它不會受到第一個*using_alias_directive*。</span><span class="sxs-lookup"><span data-stu-id="26e47-182">the last *using_alias_directive* results in a compile-time error because it is not affected by the first *using_alias_directive*.</span></span> <span data-ttu-id="26e47-183">第一個*using_alias_directive*不會導致錯誤自外部別名的範圍`E`包括*using_alias_directive*。</span><span class="sxs-lookup"><span data-stu-id="26e47-183">The first *using_alias_directive* does not result in an error since the scope of the extern alias `E` includes the *using_alias_directive*.</span></span>

<span data-ttu-id="26e47-184">A *using_alias_directive*可以建立任何命名空間或類型，包括在其中出現的命名空間的別名，而該命名空間內的任何命名空間或類型巢狀。</span><span class="sxs-lookup"><span data-stu-id="26e47-184">A *using_alias_directive* can create an alias for any namespace or type, including the namespace within which it appears and any namespace or type nested within that namespace.</span></span>

<span data-ttu-id="26e47-185">透過別名存取命名空間或類型，就會產生完全相同的結果做為透過它的宣告名稱存取該命名空間或類型。</span><span class="sxs-lookup"><span data-stu-id="26e47-185">Accessing a namespace or type through an alias yields exactly the same result as accessing that namespace or type through its declared name.</span></span> <span data-ttu-id="26e47-186">例如，假設</span><span class="sxs-lookup"><span data-stu-id="26e47-186">For example, given</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using R1 = N1;
    using R2 = N1.N2;

    class B
    {
        N1.N2.A a;            // refers to N1.N2.A
        R1.N2.A b;            // refers to N1.N2.A
        R2.A c;               // refers to N1.N2.A
    }
}
```
<span data-ttu-id="26e47-187">名稱`N1.N2.A`， `R1.N2.A`，並`R2.A`對等項目和所有參考的類別完整的名稱是`N1.N2.A`。</span><span class="sxs-lookup"><span data-stu-id="26e47-187">the names `N1.N2.A`, `R1.N2.A`, and `R2.A` are equivalent and all refer to the class whose fully qualified name is `N1.N2.A`.</span></span>

<span data-ttu-id="26e47-188">使用別名時，可以封閉式建構類型的名稱，但無法命名為未繫結的泛型型別宣告未提供類型引數。</span><span class="sxs-lookup"><span data-stu-id="26e47-188">Using aliases can name a closed constructed type, but cannot name an unbound generic type declaration without supplying type arguments.</span></span> <span data-ttu-id="26e47-189">例如: </span><span class="sxs-lookup"><span data-stu-id="26e47-189">For example:</span></span>
```csharp
namespace N1
{
    class A<T>
    {
        class B {}
    }
}

namespace N2
{
    using W = N1.A;          // Error, cannot name unbound generic type

    using X = N1.A.B;        // Error, cannot name unbound generic type

    using Y = N1.A<int>;     // Ok, can name closed constructed type

    using Z<T> = N1.A<T>;    // Error, using alias cannot have type parameters
}
```

### <a name="using-namespace-directives"></a><span data-ttu-id="26e47-190">使用命名空間指示詞</span><span class="sxs-lookup"><span data-stu-id="26e47-190">Using namespace directives</span></span>

<span data-ttu-id="26e47-191">A *using_namespace_directive*匯入到與其直接封入編譯單位或命名空間主體中，包含命名空間中啟用無限制地使用每個類型的識別項的型別。</span><span class="sxs-lookup"><span data-stu-id="26e47-191">A *using_namespace_directive* imports the types contained in a namespace into the immediately enclosing compilation unit or namespace body, enabling the identifier of each type to be used without qualification.</span></span>

```antlr
using_namespace_directive
    : 'using' namespace_name ';'
    ;
```

<span data-ttu-id="26e47-192">成員宣告中包含的編譯單位或命名空間主體*using_namespace_directive*，可以直接參考包含在指定的命名空間中的型別。</span><span class="sxs-lookup"><span data-stu-id="26e47-192">Within member declarations in a compilation unit or namespace body that contains a *using_namespace_directive*, the types contained in the given namespace can be referenced directly.</span></span> <span data-ttu-id="26e47-193">例如: </span><span class="sxs-lookup"><span data-stu-id="26e47-193">For example:</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using N1.N2;

    class B: A {}
}
```

<span data-ttu-id="26e47-194">上述在成員宣告內`N3`命名空間、 類型成員`N1.N2`可直接使用，並因此類別`N3.B`衍生自類別`N1.N2.A`。</span><span class="sxs-lookup"><span data-stu-id="26e47-194">Above, within member declarations in the `N3` namespace, the type members of `N1.N2` are directly available, and thus class `N3.B` derives from class `N1.N2.A`.</span></span>

<span data-ttu-id="26e47-195">A *using_namespace_directive*匯入包含在指定的命名空間的類型，但特別是不會匯入巢狀命名空間。</span><span class="sxs-lookup"><span data-stu-id="26e47-195">A *using_namespace_directive* imports the types contained in the given namespace, but specifically does not import nested namespaces.</span></span> <span data-ttu-id="26e47-196">在範例</span><span class="sxs-lookup"><span data-stu-id="26e47-196">In the example</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using N1;

    class B: N2.A {}        // Error, N2 unknown
}
```
<span data-ttu-id="26e47-197">*using_namespace_directive*中所包含的型別匯`N1`，但不是命名空間巢狀方式置於`N1`。</span><span class="sxs-lookup"><span data-stu-id="26e47-197">the *using_namespace_directive* imports the types contained in `N1`, but not the namespaces nested in `N1`.</span></span> <span data-ttu-id="26e47-198">因此，若要參考`N2.A`中的宣告`B`導致編譯時期錯誤，因為沒有任何成員命名為`N2`範圍內。</span><span class="sxs-lookup"><span data-stu-id="26e47-198">Thus, the reference to `N2.A` in the declaration of `B` results in a compile-time error because no members named `N2` are in scope.</span></span>

<span data-ttu-id="26e47-199">不同於*using_alias_directive*，則*using_namespace_directive*可以匯入識別碼已在封入的編譯單位或命名空間主體中定義的類型。</span><span class="sxs-lookup"><span data-stu-id="26e47-199">Unlike a *using_alias_directive*, a *using_namespace_directive* may import types whose identifiers are already defined within the enclosing compilation unit or namespace body.</span></span> <span data-ttu-id="26e47-200">作用中，名稱匯入*using_namespace_directive*隱藏名稱相似的成員，在封入的編譯單位或命名空間主體中。</span><span class="sxs-lookup"><span data-stu-id="26e47-200">In effect, names imported by a *using_namespace_directive* are hidden by similarly named members in the enclosing compilation unit or namespace body.</span></span> <span data-ttu-id="26e47-201">例如: </span><span class="sxs-lookup"><span data-stu-id="26e47-201">For example:</span></span>
```csharp
namespace N1.N2
{
    class A {}

    class B {}
}

namespace N3
{
    using N1.N2;

    class A {}
}
```

<span data-ttu-id="26e47-202">這裡，在成員宣告內`N3`命名空間`A`是指`N3.A`而非`N1.N2.A`。</span><span class="sxs-lookup"><span data-stu-id="26e47-202">Here, within member declarations in the `N3` namespace, `A` refers to `N3.A` rather than `N1.N2.A`.</span></span>

<span data-ttu-id="26e47-203">當一個以上的命名空間或類型匯入*using_namespace_directive*s 或*using_static_directive*相同編譯單位或命名空間主體中包含相同名稱，也就是參考類型該名稱作為*type_name*會被視為模稜兩可。</span><span class="sxs-lookup"><span data-stu-id="26e47-203">When more than one namespace or type imported by *using_namespace_directive*s or *using_static_directive*s in the same compilation unit or namespace body contain types by the same name, references to that name as a *type_name* are considered ambiguous.</span></span> <span data-ttu-id="26e47-204">在範例</span><span class="sxs-lookup"><span data-stu-id="26e47-204">In the example</span></span>
```csharp
namespace N1
{
    class A {}
}

namespace N2
{
    class A {}
}

namespace N3
{
    using N1;

    using N2;

    class B: A {}                // Error, A is ambiguous
}
```
<span data-ttu-id="26e47-205">兩者`N1`並`N2`包含成員`A`，而且因為`N3`匯入這兩者，參考`A`在`N3`是編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="26e47-205">both `N1` and `N2` contain a member `A`, and because `N3` imports both, referencing `A` in `N3` is a compile-time error.</span></span> <span data-ttu-id="26e47-206">在此情況下，衝突是可解析是透過參考限定`A`，或藉由引進*using_alias_directive* ，會挑選特定`A`。</span><span class="sxs-lookup"><span data-stu-id="26e47-206">In this situation, the conflict can be resolved either through qualification of references to `A`, or by introducing a *using_alias_directive* that picks a particular `A`.</span></span> <span data-ttu-id="26e47-207">例如: </span><span class="sxs-lookup"><span data-stu-id="26e47-207">For example:</span></span>
```csharp
namespace N3
{
    using N1;

    using N2;

    using A = N1.A;

    class B: A {}                // A means N1.A
}
```

<span data-ttu-id="26e47-208">此外，當一個以上的命名空間或類型匯入*using_namespace_directive*s 或*using_static_directive*相同編譯單位或命名空間主體中包含類型或成員相同的名稱，該名稱參考*simple_name*會被視為模稜兩可。</span><span class="sxs-lookup"><span data-stu-id="26e47-208">Furthermore, when more than one namespace or type imported by *using_namespace_directive*s or *using_static_directive*s in the same compilation unit or namespace body contain types or members by the same name, references to that name as a *simple_name* are considered ambiguous.</span></span> <span data-ttu-id="26e47-209">在範例</span><span class="sxs-lookup"><span data-stu-id="26e47-209">In the example</span></span>
```csharp
namespace N1
{
    class A {}
}

class C
{
    public static int A;
}

namespace N2
{
    using N1;
    using static C;

    class B
    {
        void M() 
        { 
            A a = new A();   // Ok, A is unambiguous as a type-name
            A.Equals(2);     // Error, A is ambiguous as a simple-name
        }
    }
}
```
<span data-ttu-id="26e47-210">`N1` 包含型別成員`A`，並`C`包含靜態欄位`A`，而且因為`N2`匯入這兩者，參考`A`做為*simple_name*是模稜兩可和編譯時間發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="26e47-210">`N1` contains a type member `A`, and `C` contains a static field `A`, and because `N2` imports both, referencing `A` as a *simple_name* is ambiguous and a compile-time error.</span></span> 

<span data-ttu-id="26e47-211">像是*using_alias_directive*，則*using_namespace_directive*並沒有提供任何新的成員，在基礎的宣告空間的編譯單位或命名空間，但而是只會影響編譯單位或命名空間主體中出現。</span><span class="sxs-lookup"><span data-stu-id="26e47-211">Like a *using_alias_directive*, a *using_namespace_directive* does not contribute any new members to the underlying declaration space of the compilation unit or namespace, but rather affects only the compilation unit or namespace body in which it appears.</span></span>

<span data-ttu-id="26e47-212">*Namespace_name*所參考*using_namespace_directive*解析為相同的方式*namespace_or_type_name*所參考*using_alias_directive*。</span><span class="sxs-lookup"><span data-stu-id="26e47-212">The *namespace_name* referenced by a *using_namespace_directive* is resolved in the same way as the *namespace_or_type_name* referenced by a *using_alias_directive*.</span></span> <span data-ttu-id="26e47-213">因此， *using_namespace_directive*相同編譯單位或命名空間主體中不會影響彼此，並可以依照任何順序來寫入。</span><span class="sxs-lookup"><span data-stu-id="26e47-213">Thus, *using_namespace_directive*s in the same compilation unit or namespace body do not affect each other and can be written in any order.</span></span>

### <a name="using-static-directives"></a><span data-ttu-id="26e47-214">Using 靜態指示詞</span><span class="sxs-lookup"><span data-stu-id="26e47-214">Using static directives</span></span>

<span data-ttu-id="26e47-215">A *using_static_directive*匯入巢狀型別和靜態成員直接包含在型別宣告成與其直接封入編譯單位或命名空間主體中，啟用 為每個成員和類型的識別碼使用無限制。</span><span class="sxs-lookup"><span data-stu-id="26e47-215">A *using_static_directive* imports the nested types and static members contained directly in a type declaration into the immediately enclosing compilation unit or namespace body, enabling the identifier of each member and type to be used without qualification.</span></span>

```antlr
using_static_directive
    : 'using' 'static' type_name ';'
    ;
```

<span data-ttu-id="26e47-216">成員宣告中包含的編譯單位或命名空間主體*using_static_directive*，存取巢狀類型和靜態成員 （不包括擴充方法） 的宣告中直接包含指定的型別可以直接參考。</span><span class="sxs-lookup"><span data-stu-id="26e47-216">Within member declarations in a compilation unit or namespace body that contains a *using_static_directive*, the accessible nested types and static members (except extension methods) contained directly in the declaration of the given type can be referenced directly.</span></span> <span data-ttu-id="26e47-217">例如: </span><span class="sxs-lookup"><span data-stu-id="26e47-217">For example:</span></span>

```csharp
namespace N1
{
    class A 
    {
        public class B{}
        public static B M(){ return new B(); }
    }
}

namespace N2
{
    using static N1.A;
    class C
    {
        void N() { B b = M(); }
    }
}
```

<span data-ttu-id="26e47-218">上述成員宣告中`N2`命名空間的靜態成員和巢狀型別`N1.A`可以直接使用，因此方法`N`能夠參考`B`和`M`的成員`N1.A`.</span><span class="sxs-lookup"><span data-stu-id="26e47-218">Above, within member declarations in the `N2` namespace, the static members and nested types of `N1.A` are directly available, and thus the method `N` is able to reference both the `B` and `M` members of `N1.A`.</span></span>

<span data-ttu-id="26e47-219">A *using_static_directive*特別不匯入延伸模組方法，直接做為靜態的方法，但可讓這些擴充方法引動過程版本 ([擴充方法引動過程](expressions.md#extension-method-invocations))。</span><span class="sxs-lookup"><span data-stu-id="26e47-219">A *using_static_directive* specifically does not import extension methods directly as static methods, but makes them available for extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="26e47-220">在範例</span><span class="sxs-lookup"><span data-stu-id="26e47-220">In the example</span></span>

```csharp
namespace N1 
{
    static class A 
    {
        public static void M(this string s){}
    }
}

namespace N2
{
    using static N1.A;

    class B
    {
        void N() 
        {
            M("A");      // Error, M unknown
            "B".M();     // Ok, M known as extension method
            N1.A.M("C"); // Ok, fully qualified
        }
    }
}
```
<span data-ttu-id="26e47-221">*using_static_directive*擴充方法會匯入`M`中所包含`N1.A`，但只作為擴充方法。</span><span class="sxs-lookup"><span data-stu-id="26e47-221">the *using_static_directive* imports the extension method `M` contained in `N1.A`, but only as an extension method.</span></span> <span data-ttu-id="26e47-222">因此，第一次參考`M`中的主體`B.N`導致編譯時期錯誤，因為沒有任何成員命名為`M`範圍內。</span><span class="sxs-lookup"><span data-stu-id="26e47-222">Thus, the first reference to `M` in the body of `B.N` results in a compile-time error because no members named `M` are in scope.</span></span>

<span data-ttu-id="26e47-223">A *using_static_directive*只會匯入成員和指定的型別，直接宣告的類型沒有成員和型別宣告基底類別中。</span><span class="sxs-lookup"><span data-stu-id="26e47-223">A *using_static_directive* only imports members and types declared directly in the given type, not members and types declared in base classes.</span></span>

<span data-ttu-id="26e47-224">待辦事項：範例</span><span class="sxs-lookup"><span data-stu-id="26e47-224">TODO: Example</span></span>

<span data-ttu-id="26e47-225">多個之間的模稜兩可*using_namespace_directives*並*using_static_directives*討論[Using 命名空間指示詞](namespaces.md#using-namespace-directives)。</span><span class="sxs-lookup"><span data-stu-id="26e47-225">Ambiguities between multiple *using_namespace_directives* and *using_static_directives* are discussed in [Using namespace directives](namespaces.md#using-namespace-directives).</span></span>

## <a name="namespace-members"></a><span data-ttu-id="26e47-226">命名空間成員</span><span class="sxs-lookup"><span data-stu-id="26e47-226">Namespace members</span></span>

<span data-ttu-id="26e47-227">A *namespace_member_declaration*是*namespace_declaration* ([命名空間宣告](namespaces.md#namespace-declarations)) 或*type_declaration* ([型別宣告](namespaces.md#type-declarations))。</span><span class="sxs-lookup"><span data-stu-id="26e47-227">A *namespace_member_declaration* is either a *namespace_declaration* ([Namespace declarations](namespaces.md#namespace-declarations)) or a *type_declaration* ([Type declarations](namespaces.md#type-declarations)).</span></span>

```antlr
namespace_member_declaration
    : namespace_declaration
    | type_declaration
    ;
```

<span data-ttu-id="26e47-228">編譯單位或命名空間主體可以包含*namespace_member_declaration*s 中，而這類宣告提供給基礎的宣告空間包含的編譯單位或命名空間主體的新成員。</span><span class="sxs-lookup"><span data-stu-id="26e47-228">A compilation unit or a namespace body can contain *namespace_member_declaration*s, and such declarations contribute new members to the underlying declaration space of the containing compilation unit or namespace body.</span></span>

## <a name="type-declarations"></a><span data-ttu-id="26e47-229">型別宣告</span><span class="sxs-lookup"><span data-stu-id="26e47-229">Type declarations</span></span>

<span data-ttu-id="26e47-230">A *type_declaration*是*class_declaration* ([類別宣告](classes.md#class-declarations))，則*struct_declaration* ([結構宣告](structs.md#struct-declarations))，則*interface_declaration* ([介面宣告](interfaces.md#interface-declarations))，則*enum_declaration* ([列舉宣告](enums.md#enum-declarations))，或有*delegate_declaration* ([委派宣告](delegates.md#delegate-declarations))。</span><span class="sxs-lookup"><span data-stu-id="26e47-230">A *type_declaration* is a *class_declaration* ([Class declarations](classes.md#class-declarations)), a *struct_declaration* ([Struct declarations](structs.md#struct-declarations)), an *interface_declaration* ([Interface declarations](interfaces.md#interface-declarations)), an *enum_declaration* ([Enum declarations](enums.md#enum-declarations)), or a *delegate_declaration* ([Delegate declarations](delegates.md#delegate-declarations)).</span></span>

```antlr
type_declaration
    : class_declaration
    | struct_declaration
    | interface_declaration
    | enum_declaration
    | delegate_declaration
    ;
```

<span data-ttu-id="26e47-231">A *type_declaration*為編譯單位的最上層宣告或命名空間、 類別或結構內宣告的成員可能會發生。</span><span class="sxs-lookup"><span data-stu-id="26e47-231">A *type_declaration* can occur as a top-level declaration in a compilation unit or as a member declaration within a namespace, class, or struct.</span></span>

<span data-ttu-id="26e47-232">當型別宣告型別的`T`會顯示新宣告型別的完整的名稱就是為編譯單位的最上層宣告`T`。</span><span class="sxs-lookup"><span data-stu-id="26e47-232">When a type declaration for a type `T` occurs as a top-level declaration in a compilation unit, the fully qualified name of the newly declared type is simply `T`.</span></span> <span data-ttu-id="26e47-233">當型別宣告型別的`T`就會發生的命名空間、 類別或結構的新宣告的型別完整名稱是`N.T`，其中`N`是包含命名空間、 類別或結構的完整的名稱。</span><span class="sxs-lookup"><span data-stu-id="26e47-233">When a type declaration for a type `T` occurs within a namespace, class, or struct, the fully qualified name of the newly declared type is `N.T`, where `N` is the fully qualified name of the containing namespace, class, or struct.</span></span>

<span data-ttu-id="26e47-234">在類別內宣告的型別或結構稱為巢狀型別 ([巢狀型別](classes.md#nested-types))。</span><span class="sxs-lookup"><span data-stu-id="26e47-234">A type declared within a class or struct is called a nested type ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="26e47-235">允許的存取修飾詞和型別宣告的預設存取權取決於宣告進行的內容 ([宣告存取範圍](basic-concepts.md#declared-accessibility)):</span><span class="sxs-lookup"><span data-stu-id="26e47-235">The permitted access modifiers and the default access for a type declaration depend on the context in which the declaration takes place ([Declared accessibility](basic-concepts.md#declared-accessibility)):</span></span>

*  <span data-ttu-id="26e47-236">在編譯單位或命名空間中宣告的型別可以有`public`或`internal`存取。</span><span class="sxs-lookup"><span data-stu-id="26e47-236">Types declared in compilation units or namespaces can have `public` or `internal` access.</span></span> <span data-ttu-id="26e47-237">預設值是`internal`存取。</span><span class="sxs-lookup"><span data-stu-id="26e47-237">The default is `internal` access.</span></span>
*  <span data-ttu-id="26e47-238">在類別中宣告的型別可以有`public`， `protected internal`， `protected`， `internal`，或`private`存取。</span><span class="sxs-lookup"><span data-stu-id="26e47-238">Types declared in classes can have `public`, `protected internal`, `protected`, `internal`, or `private` access.</span></span> <span data-ttu-id="26e47-239">預設值是`private`存取。</span><span class="sxs-lookup"><span data-stu-id="26e47-239">The default is `private` access.</span></span>
*  <span data-ttu-id="26e47-240">在結構中宣告的型別可以有`public`， `internal`，或`private`存取。</span><span class="sxs-lookup"><span data-stu-id="26e47-240">Types declared in structs can have `public`, `internal`, or `private` access.</span></span> <span data-ttu-id="26e47-241">預設值是`private`存取。</span><span class="sxs-lookup"><span data-stu-id="26e47-241">The default is `private` access.</span></span>

## <a name="namespace-alias-qualifiers"></a><span data-ttu-id="26e47-242">命名空間別名限定詞</span><span class="sxs-lookup"><span data-stu-id="26e47-242">Namespace alias qualifiers</span></span>

<span data-ttu-id="26e47-243">***命名空間別名限定詞***`::`讓您能夠保證型別名稱查閱會受到新的類型和成員的簡介。</span><span class="sxs-lookup"><span data-stu-id="26e47-243">The ***namespace alias qualifier*** `::` makes it possible to guarantee that type name lookups are unaffected by the introduction of new types and members.</span></span> <span data-ttu-id="26e47-244">命名空間別名限定詞永遠會出現稱為左側和右側的識別項的兩個識別碼之間。</span><span class="sxs-lookup"><span data-stu-id="26e47-244">The namespace alias qualifier always appears between two identifiers referred to as the left-hand and right-hand identifiers.</span></span> <span data-ttu-id="26e47-245">不同於一般`.`限定詞，左側的識別項的`::`限定詞查詢最多只能為 extern 或使用別名。</span><span class="sxs-lookup"><span data-stu-id="26e47-245">Unlike the regular `.` qualifier, the left-hand identifier of the `::` qualifier is looked up only as an extern or using alias.</span></span>

<span data-ttu-id="26e47-246">A *qualified_alias_member*定義如下：</span><span class="sxs-lookup"><span data-stu-id="26e47-246">A *qualified_alias_member* is defined as follows:</span></span>

```antlr
qualified_alias_member
    : identifier '::' identifier type_argument_list?
    ;
```

<span data-ttu-id="26e47-247">A *qualified_alias_member*可用來當做*namespace_or_type_name* ([命名空間和型別名稱](basic-concepts.md#namespace-and-type-names)) 或中的左運算元為*member_access*([成員存取](expressions.md#member-access))。</span><span class="sxs-lookup"><span data-stu-id="26e47-247">A *qualified_alias_member* can be used as a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) or as the left operand in a *member_access* ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="26e47-248">A *qualified_alias_member*有兩種形式之一：</span><span class="sxs-lookup"><span data-stu-id="26e47-248">A *qualified_alias_member* has one of two forms:</span></span>

*  <span data-ttu-id="26e47-249">`N::I<A1, ..., Ak>`其中`N`並`I`表示識別項，以及`<A1, ..., Ak>`是型別引數清單。</span><span class="sxs-lookup"><span data-stu-id="26e47-249">`N::I<A1, ..., Ak>`, where `N` and `I` represent identifiers, and `<A1, ..., Ak>` is a type argument list.</span></span> <span data-ttu-id="26e47-250">(`K`總是至少一個。)</span><span class="sxs-lookup"><span data-stu-id="26e47-250">(`K` is always at least one.)</span></span>
*  <span data-ttu-id="26e47-251">`N::I`其中`N`和`I`代表識別項。</span><span class="sxs-lookup"><span data-stu-id="26e47-251">`N::I`, where `N` and `I` represent identifiers.</span></span> <span data-ttu-id="26e47-252">(在此情況下，`K`則皆為零。)</span><span class="sxs-lookup"><span data-stu-id="26e47-252">(In this case, `K` is considered to be zero.)</span></span>

<span data-ttu-id="26e47-253">使用這個標記法的意義*qualified_alias_member*判斷方式如下：</span><span class="sxs-lookup"><span data-stu-id="26e47-253">Using this notation, the meaning of a *qualified_alias_member* is determined as follows:</span></span>

*  <span data-ttu-id="26e47-254">如果`N`是識別項`global`，然後搜尋全域命名空間`I`:</span><span class="sxs-lookup"><span data-stu-id="26e47-254">If `N` is the identifier `global`, then the global namespace is searched for `I`:</span></span>
   * <span data-ttu-id="26e47-255">如果全域命名空間包含名為命名空間 `I`並`K`為零，則*qualified_alias_member*指的是該命名空間。</span><span class="sxs-lookup"><span data-stu-id="26e47-255">If the global namespace contains a namespace named `I` and `K` is zero, then the *qualified_alias_member* refers to that namespace.</span></span>
   * <span data-ttu-id="26e47-256">否則，如果全域命名空間包含名為非泛型型別 `I`並`K`為零，則*qualified_alias_member*參考該型別。</span><span class="sxs-lookup"><span data-stu-id="26e47-256">Otherwise, if the global namespace contains a non-generic type named `I` and `K` is zero, then the *qualified_alias_member* refers to that type.</span></span>
   * <span data-ttu-id="26e47-257">否則，如果全域命名空間包含名為的型別 `I`具有`K` 型別參數，則*qualified_alias_member*建構具有指定的型別引數的型別參考。</span><span class="sxs-lookup"><span data-stu-id="26e47-257">Otherwise, if the global namespace contains a type named `I` that has `K` type parameters, then the *qualified_alias_member* refers to that type constructed with the given type arguments.</span></span>
   * <span data-ttu-id="26e47-258">否則，請*qualified_alias_member*是未定義，而且會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="26e47-258">Otherwise, the *qualified_alias_member* is undefined and a compile-time error occurs.</span></span>

*  <span data-ttu-id="26e47-259">否則，開頭的命名空間宣告 ([命名空間宣告](namespaces.md#namespace-declarations)) 立即包含*qualified_alias_member* （如果有的話），繼續使用每個封入的命名空間宣告（如果有的話），並包含編譯單位以作為結束*qualified_alias_member*，直到找到實體為止，會評估下列步驟：</span><span class="sxs-lookup"><span data-stu-id="26e47-259">Otherwise, starting with the namespace declaration ([Namespace declarations](namespaces.md#namespace-declarations)) immediately containing the *qualified_alias_member* (if any), continuing with each enclosing namespace declaration (if any), and ending with the compilation unit containing the *qualified_alias_member*, the following steps are evaluated until an entity is located:</span></span>

   * <span data-ttu-id="26e47-260">如果包含的命名空間宣告或編譯單位*using_alias_directive*建立關聯的`N`類型，則*qualified_alias_member*是未定義和編譯時間會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="26e47-260">If the namespace declaration or compilation unit contains a *using_alias_directive* that associates `N` with a type, then the *qualified_alias_member* is undefined and a compile-time error occurs.</span></span>
   * <span data-ttu-id="26e47-261">否則，如果命名空間宣告或編譯單位包含*extern_alias_directive*或是*using_alias_directive*建立關聯的`N`與命名空間，然後：</span><span class="sxs-lookup"><span data-stu-id="26e47-261">Otherwise, if the namespace declaration or compilation unit contains an *extern_alias_directive* or *using_alias_directive* that associates `N` with a namespace, then:</span></span>
     * <span data-ttu-id="26e47-262">如果命名空間相關聯`N`包含名為命名空間 `I`並`K`為零，則*qualified_alias_member*指的是該命名空間。</span><span class="sxs-lookup"><span data-stu-id="26e47-262">If the namespace associated with `N` contains a namespace named `I` and `K` is zero, then the *qualified_alias_member* refers to that namespace.</span></span>
     * <span data-ttu-id="26e47-263">否則，如果命名空間相關聯`N`包含名為非泛型型別 `I`並`K`為零，則*qualified_alias_member*參考該型別。</span><span class="sxs-lookup"><span data-stu-id="26e47-263">Otherwise, if the namespace associated with `N` contains a non-generic type named `I` and `K` is zero, then the *qualified_alias_member* refers to that type.</span></span>
     * <span data-ttu-id="26e47-264">否則，如果命名空間相關聯`N`包含名為的型別 `I`具有`K` 型別參數，則*qualified_alias_member*指的是以建構類型指定的型別引數。</span><span class="sxs-lookup"><span data-stu-id="26e47-264">Otherwise, if the namespace associated with `N` contains a type named `I` that has `K` type parameters, then the *qualified_alias_member* refers to that type constructed with the given type arguments.</span></span>
     * <span data-ttu-id="26e47-265">否則，請*qualified_alias_member*是未定義，而且會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="26e47-265">Otherwise, the *qualified_alias_member* is undefined and a compile-time error occurs.</span></span>
*  <span data-ttu-id="26e47-266">否則，請*qualified_alias_member*是未定義，而且會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="26e47-266">Otherwise, the *qualified_alias_member* is undefined and a compile-time error occurs.</span></span>

<span data-ttu-id="26e47-267">請注意，使用參考類型的別名的命名空間別名限定詞會使編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="26e47-267">Note that using the namespace alias qualifier with an alias that references a type causes a compile-time error.</span></span> <span data-ttu-id="26e47-268">也請注意，如果識別項 `N`已`global`，則即使沒有別名建立關聯的使用中全域命名空間中，執行查閱`global`與型別或命名空間。</span><span class="sxs-lookup"><span data-stu-id="26e47-268">Also note that if the identifier `N` is `global`, then lookup is performed in the global namespace, even if there is a using alias associating `global` with a type or namespace.</span></span>

### <a name="uniqueness-of-aliases"></a><span data-ttu-id="26e47-269">別名的唯一性</span><span class="sxs-lookup"><span data-stu-id="26e47-269">Uniqueness of aliases</span></span>

<span data-ttu-id="26e47-270">每個編譯單位和命名空間主體具有個別的宣告空間，以讓外部別名和使用別名。</span><span class="sxs-lookup"><span data-stu-id="26e47-270">Each compilation unit and namespace body has a separate declaration space for extern aliases and using aliases.</span></span> <span data-ttu-id="26e47-271">因此，雖然外部別名，或使用別名的名稱必須是唯一的外部別名集合中，並使用別名宣告直接包含的編譯單位或命名空間主體中，別名不允許有相同名稱的型別或命名空間，只要我只能搭配使用 t`::`限定詞。</span><span class="sxs-lookup"><span data-stu-id="26e47-271">Thus, while the name of an extern alias or using alias must be unique within the set of extern aliases and using aliases declared in the immediately containing compilation unit or namespace body, an alias is permitted to have the same name as a type or namespace as long as it is used only with the `::` qualifier.</span></span>

<span data-ttu-id="26e47-272">在範例</span><span class="sxs-lookup"><span data-stu-id="26e47-272">In the example</span></span>
```csharp
namespace N
{
    public class A {}

    public class B {}
}

namespace N
{
    using A = System.IO;

    class X
    {
        A.Stream s1;            // Error, A is ambiguous

        A::Stream s2;           // Ok
    }
}
```
<span data-ttu-id="26e47-273">名稱`A`第二個命名空間主體中有兩個可能的意義，因為這兩個類別`A`並使用別名`A`範圍內。</span><span class="sxs-lookup"><span data-stu-id="26e47-273">the name `A` has two possible meanings in the second namespace body because both the class `A` and the using alias `A` are in scope.</span></span> <span data-ttu-id="26e47-274">基於這個理由，利用`A`限定名稱中`A.Stream`模稜兩可，並會導致發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="26e47-274">For this reason, use of `A` in the qualified name `A.Stream` is ambiguous and causes a compile-time error to occur.</span></span> <span data-ttu-id="26e47-275">不過，使用`A`具有`::`限定詞不是錯誤因為`A`查閱只會為命名空間別名。</span><span class="sxs-lookup"><span data-stu-id="26e47-275">However, use of `A` with the `::` qualifier is not an error because `A` is looked up only as a namespace alias.</span></span>
