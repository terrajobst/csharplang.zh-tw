---
ms.openlocfilehash: 0a09585f4f885647230354c66a2449abb7ef1f44
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229541"
---
# <a name="interfaces"></a><span data-ttu-id="b1004-101">介面</span><span class="sxs-lookup"><span data-stu-id="b1004-101">Interfaces</span></span>

<span data-ttu-id="b1004-102">介面定義合約。</span><span class="sxs-lookup"><span data-stu-id="b1004-102">An interface defines a contract.</span></span> <span data-ttu-id="b1004-103">類別或結構實作介面必須遵守其合約。</span><span class="sxs-lookup"><span data-stu-id="b1004-103">A class or struct that implements an interface must adhere to its contract.</span></span> <span data-ttu-id="b1004-104">介面可以繼承自多個基底介面，以及類別或結構可以實作多個介面。</span><span class="sxs-lookup"><span data-stu-id="b1004-104">An interface may inherit from multiple base interfaces, and a class or struct may implement multiple interfaces.</span></span>

<span data-ttu-id="b1004-105">介面可以包含方法、 屬性、 事件和索引子。</span><span class="sxs-lookup"><span data-stu-id="b1004-105">Interfaces can contain methods, properties, events, and indexers.</span></span> <span data-ttu-id="b1004-106">介面本身不提供它所定義之成員的實作。</span><span class="sxs-lookup"><span data-stu-id="b1004-106">The interface itself does not provide implementations for the members that it defines.</span></span> <span data-ttu-id="b1004-107">介面只會指定必須提供的類別或結構實作介面成員。</span><span class="sxs-lookup"><span data-stu-id="b1004-107">The interface merely specifies the members that must be supplied by classes or structs that implement the interface.</span></span>

## <a name="interface-declarations"></a><span data-ttu-id="b1004-108">介面宣告</span><span class="sxs-lookup"><span data-stu-id="b1004-108">Interface declarations</span></span>

<span data-ttu-id="b1004-109">*Interface_declaration*是*type_declaration* ([型別宣告](namespaces.md#type-declarations))，宣告了新的介面型別。</span><span class="sxs-lookup"><span data-stu-id="b1004-109">An *interface_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new interface type.</span></span>

```antlr
interface_declaration
    : attributes? interface_modifier* 'partial'? 'interface'
      identifier variant_type_parameter_list? interface_base?
      type_parameter_constraints_clause* interface_body ';'?
    ;
```

<span data-ttu-id="b1004-110">*Interface_declaration*組成的一組選擇性*屬性*([屬性](attributes.md))，後面接著一組選擇性的*interface_modifier*s ([介面修飾詞](interfaces.md#interface-modifiers))，後面接著選擇性`partial`修飾詞，後面接著關鍵字`interface`並*識別碼*可命名介面後面接著選擇性*variant_type_parameter_list*規格 ([Variant 類型參數清單](interfaces.md#variant-type-parameter-lists))，後面接著選擇性*interface_base*規格 ([的基底介面](interfaces.md#base-interfaces))，後面接著選擇性*type_parameter_constraints_clause*s 規格 ([類型參數條件約束](classes.md#type-parameter-constraints))後面接著*interface_body* ([介面主體](interfaces.md#interface-body))，或者後面接著一個分號。</span><span class="sxs-lookup"><span data-stu-id="b1004-110">An *interface_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *interface_modifier*s ([Interface modifiers](interfaces.md#interface-modifiers)), followed by an optional `partial` modifier, followed by the keyword `interface` and an *identifier* that names the interface, followed by an optional *variant_type_parameter_list* specification ([Variant type parameter lists](interfaces.md#variant-type-parameter-lists)), followed by an optional *interface_base* specification ([Base interfaces](interfaces.md#base-interfaces)), followed by an optional *type_parameter_constraints_clause*s specification ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by an *interface_body* ([Interface body](interfaces.md#interface-body)), optionally followed by a semicolon.</span></span>

### <a name="interface-modifiers"></a><span data-ttu-id="b1004-111">介面修飾詞</span><span class="sxs-lookup"><span data-stu-id="b1004-111">Interface modifiers</span></span>

<span data-ttu-id="b1004-112">*Interface_declaration*可以選擇性地包含一連串的介面修飾詞：</span><span class="sxs-lookup"><span data-stu-id="b1004-112">An *interface_declaration* may optionally include a sequence of interface modifiers:</span></span>

```antlr
interface_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | interface_modifier_unsafe
    ;
```

<span data-ttu-id="b1004-113">它是在 interface 宣告中出現多次相同的修飾詞的編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="b1004-113">It is a compile-time error for the same modifier to appear multiple times in an interface declaration.</span></span>

<span data-ttu-id="b1004-114">`new`修飾詞只能在類別內定義的介面上。</span><span class="sxs-lookup"><span data-stu-id="b1004-114">The `new` modifier is only permitted on interfaces defined within a class.</span></span> <span data-ttu-id="b1004-115">它會指定介面會隱藏繼承的成員，以相同的名稱，如中所述[的新修飾詞](classes.md#the-new-modifier)。</span><span class="sxs-lookup"><span data-stu-id="b1004-115">It specifies that the interface hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span>

<span data-ttu-id="b1004-116">`public`， `protected`， `internal`，和`private`修飾詞控制介面的存取範圍。</span><span class="sxs-lookup"><span data-stu-id="b1004-116">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the interface.</span></span> <span data-ttu-id="b1004-117">根據介面宣告發生所在的內容，只有部分這些修飾詞可能會允許 ([宣告存取範圍](basic-concepts.md#declared-accessibility))。</span><span class="sxs-lookup"><span data-stu-id="b1004-117">Depending on the context in which the interface declaration occurs, only some of these modifiers may be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="b1004-118">Partial 修飾詞</span><span class="sxs-lookup"><span data-stu-id="b1004-118">Partial modifier</span></span>

<span data-ttu-id="b1004-119">`partial`修飾詞表示這*interface_declaration*是部分的型別宣告。</span><span class="sxs-lookup"><span data-stu-id="b1004-119">The `partial` modifier indicates that this *interface_declaration* is a partial type declaration.</span></span> <span data-ttu-id="b1004-120">在封入命名空間或類型宣告同名的多個部分的介面宣告結合構成一個介面宣告，下列規則中指定[部分型別](classes.md#partial-types)。</span><span class="sxs-lookup"><span data-stu-id="b1004-120">Multiple partial interface declarations with the same name within an enclosing namespace or type declaration combine to form one interface declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

### <a name="variant-type-parameter-lists"></a><span data-ttu-id="b1004-121">Variant 類型參數清單</span><span class="sxs-lookup"><span data-stu-id="b1004-121">Variant type parameter lists</span></span>

<span data-ttu-id="b1004-122">Variant 類型參數清單只能出現在介面和委派類型。</span><span class="sxs-lookup"><span data-stu-id="b1004-122">Variant type parameter lists can only occur on interface and delegate types.</span></span> <span data-ttu-id="b1004-123">從一般的差異*type_parameter_list*s 是選擇性*variance_annotation*上每個類型參數。</span><span class="sxs-lookup"><span data-stu-id="b1004-123">The difference from ordinary *type_parameter_list*s is the optional *variance_annotation* on each type parameter.</span></span>

```antlr
variant_type_parameter_list
    : '<' variant_type_parameters '>'
    ;

variant_type_parameters
    : attributes? variance_annotation? type_parameter
    | variant_type_parameters ',' attributes? variance_annotation? type_parameter
    ;

variance_annotation
    : 'in'
    | 'out'
    ;
```

<span data-ttu-id="b1004-124">如果變異數附註`out`，該型別參數要***covariant***。</span><span class="sxs-lookup"><span data-stu-id="b1004-124">If the variance annotation is `out`, the type parameter is said to be ***covariant***.</span></span> <span data-ttu-id="b1004-125">如果變異數附註`in`，該型別參數要***contravariant***。</span><span class="sxs-lookup"><span data-stu-id="b1004-125">If the variance annotation is `in`, the type parameter is said to be ***contravariant***.</span></span> <span data-ttu-id="b1004-126">如果沒有任何變異數附註，該型別參數要***非變異***。</span><span class="sxs-lookup"><span data-stu-id="b1004-126">If there is no variance annotation, the type parameter is said to be ***invariant***.</span></span>

<span data-ttu-id="b1004-127">在範例</span><span class="sxs-lookup"><span data-stu-id="b1004-127">In the example</span></span>
```csharp
interface C<out X, in Y, Z> 
{
  X M(Y y);
  Z P { get; set; }
}
```
<span data-ttu-id="b1004-128">`X` 是 covariant，`Y`是 contravariant 和`Z`為非變異。</span><span class="sxs-lookup"><span data-stu-id="b1004-128">`X` is covariant, `Y` is contravariant and `Z` is invariant.</span></span>

#### <a name="variance-safety"></a><span data-ttu-id="b1004-129">變異數的安全</span><span class="sxs-lookup"><span data-stu-id="b1004-129">Variance safety</span></span>

<span data-ttu-id="b1004-130">類型參數清單中的類型的變異數附註的相符項目會限制型別可能會發生型別宣告內的位置。</span><span class="sxs-lookup"><span data-stu-id="b1004-130">The occurrence of variance annotations in the type parameter list of a type restricts the places where types can occur within the type declaration.</span></span>

<span data-ttu-id="b1004-131">型別`T`已***輸出 unsafe***如果下列其中一種保留：</span><span class="sxs-lookup"><span data-stu-id="b1004-131">A type `T` is ***output-unsafe*** if one of the following holds:</span></span>

*  <span data-ttu-id="b1004-132">`T` 為 contravariant 類型參數。</span><span class="sxs-lookup"><span data-stu-id="b1004-132">`T` is a contravariant type parameter</span></span>
*  <span data-ttu-id="b1004-133">`T` 是輸出不安全的項目類型的陣列類型</span><span class="sxs-lookup"><span data-stu-id="b1004-133">`T` is an array type with an output-unsafe element type</span></span>
*  <span data-ttu-id="b1004-134">`T` 是介面或委派的型別`S<A1,...,Ak>`建構自泛型型別的`S<X1,...,Xk>`至少一個 where`Ai`下列其中一種保留：</span><span class="sxs-lookup"><span data-stu-id="b1004-134">`T` is an interface or delegate type `S<A1,...,Ak>` constructed from a generic type `S<X1,...,Xk>` where for at least one `Ai` one of the following holds:</span></span>
   * <span data-ttu-id="b1004-135">`Xi` 共變性或非變異值和`Ai`輸出不安全。</span><span class="sxs-lookup"><span data-stu-id="b1004-135">`Xi` is covariant or invariant and `Ai` is output-unsafe.</span></span>
   * <span data-ttu-id="b1004-136">`Xi` 逆變性或非變異值和`Ai`是輸入安全。</span><span class="sxs-lookup"><span data-stu-id="b1004-136">`Xi` is contravariant or invariant and `Ai` is input-safe.</span></span>
   
<span data-ttu-id="b1004-137">型別`T`已***輸入 unsafe***如果下列其中一種保留：</span><span class="sxs-lookup"><span data-stu-id="b1004-137">A type `T` is ***input-unsafe*** if one of the following holds:</span></span>

*  <span data-ttu-id="b1004-138">`T` 為 covariant 類型參數。</span><span class="sxs-lookup"><span data-stu-id="b1004-138">`T` is a covariant type parameter</span></span>
*  <span data-ttu-id="b1004-139">`T` 是不安全的輸入項目類型的陣列類型</span><span class="sxs-lookup"><span data-stu-id="b1004-139">`T` is an array type with an input-unsafe element type</span></span>
*  <span data-ttu-id="b1004-140">`T` 是介面或委派的型別`S<A1,...,Ak>`建構自泛型型別的`S<X1,...,Xk>`至少一個 where`Ai`下列其中一種保留：</span><span class="sxs-lookup"><span data-stu-id="b1004-140">`T` is an interface or delegate type `S<A1,...,Ak>` constructed from a generic type `S<X1,...,Xk>` where for at least one `Ai` one of the following holds:</span></span>
   * <span data-ttu-id="b1004-141">`Xi` 共變性或非變異值和`Ai`輸入不安全。</span><span class="sxs-lookup"><span data-stu-id="b1004-141">`Xi` is covariant or invariant and `Ai` is input-unsafe.</span></span>
   * <span data-ttu-id="b1004-142">`Xi` 逆變性或非變異值和`Ai`輸出不安全。</span><span class="sxs-lookup"><span data-stu-id="b1004-142">`Xi` is contravariant or invariant and `Ai` is output-unsafe.</span></span>

<span data-ttu-id="b1004-143">直接易懂的方式，輸出不安全的型別中的輸出位置，禁止使用，並禁止到輸入位置的輸入不安全的類型。</span><span class="sxs-lookup"><span data-stu-id="b1004-143">Intuitively, an output-unsafe type is prohibited in an output position, and an input-unsafe type is prohibited in an input position.</span></span>

<span data-ttu-id="b1004-144">型別是***輸出安全***如果不是輸出不安全，並***輸入安全***如果不是輸入不安全。</span><span class="sxs-lookup"><span data-stu-id="b1004-144">A type is ***output-safe*** if it is not output-unsafe, and ***input-safe*** if it is not input-unsafe.</span></span>

#### <a name="variance-conversion"></a><span data-ttu-id="b1004-145">變異數轉換</span><span class="sxs-lookup"><span data-stu-id="b1004-145">Variance conversion</span></span>

<span data-ttu-id="b1004-146">變異數附註的目的是提供較寬鬆 （但仍型別安全） 轉換成介面和委派類型。</span><span class="sxs-lookup"><span data-stu-id="b1004-146">The purpose of variance annotations is to provide for more lenient (but still type safe) conversions to interface and delegate types.</span></span> <span data-ttu-id="b1004-147">這個最後的隱含定義 ([隱含轉換](conversions.md#implicit-conversions)) 和明確轉換 ([明確轉換](conversions.md#explicit-conversions)) 讓使用變異數可轉換性，其定義，如下所示的概念：</span><span class="sxs-lookup"><span data-stu-id="b1004-147">To this end the definitions of implicit ([Implicit conversions](conversions.md#implicit-conversions)) and explicit conversions ([Explicit conversions](conversions.md#explicit-conversions)) make use of the notion of variance-convertibility, which is defined as follows:</span></span>

<span data-ttu-id="b1004-148">型別`T<A1,...,An>`可轉換變異數的型別`T<B1,...,Bn>`如果`T`介面或委派型別宣告具有 variant 類型參數`T<X1,...,Xn>`，和每個 variant 類型參數`Xi`下列其中之一保留：</span><span class="sxs-lookup"><span data-stu-id="b1004-148">A type `T<A1,...,An>` is variance-convertible to a type `T<B1,...,Bn>` if `T` is either an interface or a delegate type declared with the variant type parameters `T<X1,...,Xn>`, and for each variant type parameter `Xi` one of the following holds:</span></span>

*  <span data-ttu-id="b1004-149">`Xi` 是 covariant，且是隱含的參考或識別轉換從`Ai`至 `Bi`</span><span class="sxs-lookup"><span data-stu-id="b1004-149">`Xi` is covariant and an implicit reference or identity conversion exists from `Ai` to `Bi`</span></span>
*  <span data-ttu-id="b1004-150">`Xi` 而且遠端系統 contravariant 的隱含參考或識別轉換存在從`Bi`至 `Ai`</span><span class="sxs-lookup"><span data-stu-id="b1004-150">`Xi` is contravariant and an implicit reference or identity conversion exists from `Bi` to `Ai`</span></span>
*  <span data-ttu-id="b1004-151">`Xi` 是不區分及身分識別轉換存在從`Ai`至 `Bi`</span><span class="sxs-lookup"><span data-stu-id="b1004-151">`Xi` is invariant and an identity conversion exists from `Ai` to `Bi`</span></span>

### <a name="base-interfaces"></a><span data-ttu-id="b1004-152">基底介面</span><span class="sxs-lookup"><span data-stu-id="b1004-152">Base interfaces</span></span>

<span data-ttu-id="b1004-153">介面可以繼承自零或多個介面型別，稱為***明確的基底介面***的介面。</span><span class="sxs-lookup"><span data-stu-id="b1004-153">An interface can inherit from zero or more interface types, which are called the ***explicit base interfaces*** of the interface.</span></span> <span data-ttu-id="b1004-154">當介面有一或多個明確的基底介面時，該介面，宣告中的介面識別碼接著冒號和逗號分隔的基底介面型別清單。</span><span class="sxs-lookup"><span data-stu-id="b1004-154">When an interface has one or more explicit base interfaces, then in the declaration of that interface, the interface identifier is followed by a colon and a comma separated list of base interface types.</span></span>

```antlr
interface_base
    : ':' interface_type_list
    ;
```

<span data-ttu-id="b1004-155">建構的介面類型，明確的基底介面明確的基底介面宣告的泛型型別宣告，並取代，每個形成*type_parameter*基底介面中宣告對應*type_argument*建構的類型。</span><span class="sxs-lookup"><span data-stu-id="b1004-155">For a constructed interface type, the explicit base interfaces are formed by taking the explicit base interface declarations on the generic type declaration, and substituting, for each *type_parameter* in the base interface declaration, the corresponding *type_argument* of the constructed type.</span></span>

<span data-ttu-id="b1004-156">明確的基底介面的介面必須至少像一樣地存取介面本身 ([協助工具的條件約束](basic-concepts.md#accessibility-constraints))。</span><span class="sxs-lookup"><span data-stu-id="b1004-156">The explicit base interfaces of an interface must be at least as accessible as the interface itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span> <span data-ttu-id="b1004-157">比方說，它是指定的編譯時期錯誤`private`或是`internal`介面中*interface_base*的`public`介面。</span><span class="sxs-lookup"><span data-stu-id="b1004-157">For example, it is a compile-time error to specify a `private` or `internal` interface in the *interface_base* of a `public` interface.</span></span>

<span data-ttu-id="b1004-158">它是介面，以直接或間接繼承自它自己的編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="b1004-158">It is a compile-time error for an interface to directly or indirectly inherit from itself.</span></span>

<span data-ttu-id="b1004-159">***的基底介面***介面是明確的基底介面和其基底介面。</span><span class="sxs-lookup"><span data-stu-id="b1004-159">The ***base interfaces*** of an interface are the explicit base interfaces and their base interfaces.</span></span> <span data-ttu-id="b1004-160">換句話說，基底介面的集合會是完整的可轉移關閉的明確的基底介面，其明確的基底介面，以及等等。</span><span class="sxs-lookup"><span data-stu-id="b1004-160">In other words, the set of base interfaces is the complete transitive closure of the explicit base interfaces, their explicit base interfaces, and so on.</span></span> <span data-ttu-id="b1004-161">介面會繼承其基底介面的所有成員。</span><span class="sxs-lookup"><span data-stu-id="b1004-161">An interface inherits all members of its base interfaces.</span></span> <span data-ttu-id="b1004-162">在範例</span><span class="sxs-lookup"><span data-stu-id="b1004-162">In the example</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

interface IComboBox: ITextBox, IListBox {}
```
<span data-ttu-id="b1004-163">基底介面`IComboBox`都`IControl`， `ITextBox`，和`IListBox`。</span><span class="sxs-lookup"><span data-stu-id="b1004-163">the base interfaces of `IComboBox` are `IControl`, `ITextBox`, and `IListBox`.</span></span>

<span data-ttu-id="b1004-164">亦即`IComboBox`上述介面繼承成員`SetText`並`SetItems`以及`Paint`。</span><span class="sxs-lookup"><span data-stu-id="b1004-164">In other words, the `IComboBox` interface above inherits members `SetText` and `SetItems` as well as `Paint`.</span></span>

<span data-ttu-id="b1004-165">介面的每個基底介面都必須是輸出安全 ([變異數安全](interfaces.md#variance-safety))。</span><span class="sxs-lookup"><span data-stu-id="b1004-165">Every base interface of an interface must be output-safe ([Variance safety](interfaces.md#variance-safety)).</span></span> <span data-ttu-id="b1004-166">類別或結構也會隱含地實作介面會實作所有介面的基底介面。</span><span class="sxs-lookup"><span data-stu-id="b1004-166">A class or struct that implements an interface also implicitly implements all of the interface's base interfaces.</span></span>

### <a name="interface-body"></a><span data-ttu-id="b1004-167">介面主體內</span><span class="sxs-lookup"><span data-stu-id="b1004-167">Interface body</span></span>

<span data-ttu-id="b1004-168">*Interface_body*介面會定義介面的成員。</span><span class="sxs-lookup"><span data-stu-id="b1004-168">The *interface_body* of an interface defines the members of the interface.</span></span>

```antlr
interface_body
    : '{' interface_member_declaration* '}'
    ;
```

## <a name="interface-members"></a><span data-ttu-id="b1004-169">介面成員</span><span class="sxs-lookup"><span data-stu-id="b1004-169">Interface members</span></span>

<span data-ttu-id="b1004-170">介面的成員會繼承自基底介面的成員，介面本身所宣告的成員。</span><span class="sxs-lookup"><span data-stu-id="b1004-170">The members of an interface are the members inherited from the base interfaces and the members declared by the interface itself.</span></span>

```antlr
interface_member_declaration
    : interface_method_declaration
    | interface_property_declaration
    | interface_event_declaration
    | interface_indexer_declaration
    ;
```

<span data-ttu-id="b1004-171">介面宣告可以宣告為零或多個成員。</span><span class="sxs-lookup"><span data-stu-id="b1004-171">An interface declaration may declare zero or more members.</span></span> <span data-ttu-id="b1004-172">介面成員必須是方法、 屬性、 事件或索引子。</span><span class="sxs-lookup"><span data-stu-id="b1004-172">The members of an interface must be methods, properties, events, or indexers.</span></span> <span data-ttu-id="b1004-173">介面不能包含常數、 欄位、 運算子、 執行個體建構函式、 解構函式或類型，也不介面可以包含任何類型的靜態成員。</span><span class="sxs-lookup"><span data-stu-id="b1004-173">An interface cannot contain constants, fields, operators, instance constructors, destructors, or types, nor can an interface contain static members of any kind.</span></span>

<span data-ttu-id="b1004-174">所有介面成員以隱含方式都具有公用存取。</span><span class="sxs-lookup"><span data-stu-id="b1004-174">All interface members implicitly have public access.</span></span> <span data-ttu-id="b1004-175">它是介面成員宣告，包括任何修飾詞的編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="b1004-175">It is a compile-time error for interface member declarations to include any modifiers.</span></span> <span data-ttu-id="b1004-176">特別是，介面成員不可以宣告和修飾詞`abstract`， `public`， `protected`， `internal`， `private`， `virtual`， `override`，或`static`。</span><span class="sxs-lookup"><span data-stu-id="b1004-176">In particular, interfaces members cannot be declared with the modifiers `abstract`, `public`, `protected`, `internal`, `private`, `virtual`, `override`, or `static`.</span></span>

<span data-ttu-id="b1004-177">此範例</span><span class="sxs-lookup"><span data-stu-id="b1004-177">The example</span></span>
```csharp
public delegate void StringListEvent(IStringList sender);

public interface IStringList
{
    void Add(string s);
    int Count { get; }
    event StringListEvent Changed;
    string this[int index] { get; set; }
}
```
<span data-ttu-id="b1004-178">宣告介面，包含其中每個可能的成員種類：方法、 屬性、 事件和索引子。</span><span class="sxs-lookup"><span data-stu-id="b1004-178">declares an interface that contains one each of the possible kinds of members: A method, a property, an event, and an indexer.</span></span>

<span data-ttu-id="b1004-179">*Interface_declaration*建立新的宣告空間 ([宣告](basic-concepts.md#declarations))，而*interface_member_declaration*立即包含s*interface_declaration*這個宣告空間中引入新的成員。</span><span class="sxs-lookup"><span data-stu-id="b1004-179">An *interface_declaration* creates a new declaration space ([Declarations](basic-concepts.md#declarations)), and the *interface_member_declaration*s immediately contained by the *interface_declaration* introduce new members into this declaration space.</span></span> <span data-ttu-id="b1004-180">下列規則適用於*interface_member_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="b1004-180">The following rules apply to *interface_member_declaration*s:</span></span>

*  <span data-ttu-id="b1004-181">方法的名稱必須與不同的所有屬性和事件在相同的介面中宣告的名稱。</span><span class="sxs-lookup"><span data-stu-id="b1004-181">The name of a method must differ from the names of all properties and events declared in the same interface.</span></span> <span data-ttu-id="b1004-182">此外，簽章 ([簽章和多載](basic-concepts.md#signatures-and-overloading)) 的方法必須與相同的介面中宣告的所有其他方法的簽章不同，在相同的介面中宣告的兩種方法可能沒有簽章，差異在於完全`ref`和`out`。</span><span class="sxs-lookup"><span data-stu-id="b1004-182">In addition, the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of a method must differ from the signatures of all other methods declared in the same interface, and two methods declared in the same interface may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="b1004-183">屬性或事件的名稱必須與相同介面中宣告的所有其他成員的名稱不同。</span><span class="sxs-lookup"><span data-stu-id="b1004-183">The name of a property or event must differ from the names of all other members declared in the same interface.</span></span>
*  <span data-ttu-id="b1004-184">索引子的簽章必須與相同介面中所宣告之其他所有索引子的簽章不同。</span><span class="sxs-lookup"><span data-stu-id="b1004-184">The signature of an indexer must differ from the signatures of all other indexers declared in the same interface.</span></span>

<span data-ttu-id="b1004-185">介面的繼承的成員是特別不屬於介面的宣告空間的一部分。</span><span class="sxs-lookup"><span data-stu-id="b1004-185">The inherited members of an interface are specifically not part of the declaration space of the interface.</span></span> <span data-ttu-id="b1004-186">因此，介面可以宣告具有相同的名稱或簽章的成員，做為繼承的成員。</span><span class="sxs-lookup"><span data-stu-id="b1004-186">Thus, an interface is allowed to declare a member with the same name or signature as an inherited member.</span></span> <span data-ttu-id="b1004-187">當發生這種情況時，衍生的介面成員即隱藏基底介面成員。</span><span class="sxs-lookup"><span data-stu-id="b1004-187">When this occurs, the derived interface member is said to hide the base interface member.</span></span> <span data-ttu-id="b1004-188">隱藏繼承的成員不被視為錯誤，但會造成編譯器將發出警告。</span><span class="sxs-lookup"><span data-stu-id="b1004-188">Hiding an inherited member is not considered an error, but it does cause the compiler to issue a warning.</span></span> <span data-ttu-id="b1004-189">若要隱藏警告，衍生的介面成員的宣告必須包含`new`修飾詞來表示衍生的成員要隱藏基底的成員。</span><span class="sxs-lookup"><span data-stu-id="b1004-189">To suppress the warning, the declaration of the derived interface member must include a `new` modifier to indicate that the derived member is intended to hide the base member.</span></span> <span data-ttu-id="b1004-190">本主題會討論中進一步[經由繼承隱藏](basic-concepts.md#hiding-through-inheritance)。</span><span class="sxs-lookup"><span data-stu-id="b1004-190">This topic is discussed further in [Hiding through inheritance](basic-concepts.md#hiding-through-inheritance).</span></span>

<span data-ttu-id="b1004-191">如果`new`修飾詞加入的宣告，也不會隱藏繼承的成員，來指出發出警告。</span><span class="sxs-lookup"><span data-stu-id="b1004-191">If a `new` modifier is included in a declaration that doesn't hide an inherited member, a warning is issued to that effect.</span></span> <span data-ttu-id="b1004-192">藉由移除隱藏這個警告`new`修飾詞。</span><span class="sxs-lookup"><span data-stu-id="b1004-192">This warning is suppressed by removing the `new` modifier.</span></span>

<span data-ttu-id="b1004-193">請注意，在類別成員`object`則不會，嚴格地說，任何介面的成員 ([介面成員](interfaces.md#interface-members))。</span><span class="sxs-lookup"><span data-stu-id="b1004-193">Note that the members in class `object` are not, strictly speaking, members of any interface ([Interface members](interfaces.md#interface-members)).</span></span> <span data-ttu-id="b1004-194">不過，在類別成員`object`可透過任何介面類型中進行成員查詢 ([成員查閱](expressions.md#member-lookup))。</span><span class="sxs-lookup"><span data-stu-id="b1004-194">However, the members in class `object` are available via member lookup in any interface type ([Member lookup](expressions.md#member-lookup)).</span></span>

### <a name="interface-methods"></a><span data-ttu-id="b1004-195">介面方法</span><span class="sxs-lookup"><span data-stu-id="b1004-195">Interface methods</span></span>

<span data-ttu-id="b1004-196">介面方法使用宣告*interface_method_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="b1004-196">Interface methods are declared using *interface_method_declaration*s:</span></span>

```antlr
interface_method_declaration
    : attributes? 'new'? return_type identifier type_parameter_list
      '(' formal_parameter_list? ')' type_parameter_constraints_clause* ';'
    ;
```

<span data-ttu-id="b1004-197">*屬性*， *return_type*，*識別碼*，以及*formal_parameter_list*的介面方法宣告都具有相同這表示的類別中的方法宣告 ([方法](classes.md#methods))。</span><span class="sxs-lookup"><span data-stu-id="b1004-197">The *attributes*, *return_type*, *identifier*, and *formal_parameter_list* of an interface method declaration have the same meaning as those of a method declaration in a class ([Methods](classes.md#methods)).</span></span> <span data-ttu-id="b1004-198">若要指定方法的主體中，不允許介面方法宣告，並宣告因此一律會以分號結束。</span><span class="sxs-lookup"><span data-stu-id="b1004-198">An interface method declaration is not permitted to specify a method body, and the declaration therefore always ends with a semicolon.</span></span>

<span data-ttu-id="b1004-199">介面方法的每個型式參數類型必須是輸入安全 ([變異數安全](interfaces.md#variance-safety))，而且傳回型別必須是`void`或輸出安全。</span><span class="sxs-lookup"><span data-stu-id="b1004-199">Each formal parameter type of an interface method must be input-safe ([Variance safety](interfaces.md#variance-safety)), and the return type must be either `void` or output-safe.</span></span> <span data-ttu-id="b1004-200">此外，每個類別類型條件約束、 介面類型條件約束和方法的任何型別參數的類型參數條件約束必須是輸入安全。</span><span class="sxs-lookup"><span data-stu-id="b1004-200">Furthermore, each class type constraint, interface type constraint and type parameter constraint on any type parameter of the method must be input-safe.</span></span>

<span data-ttu-id="b1004-201">這些規則確保任何 covariant 或 contravariant 的介面的使用方式會保留型別安全。</span><span class="sxs-lookup"><span data-stu-id="b1004-201">These rules ensure that any covariant or contravariant usage of the interface remains type-safe.</span></span> <span data-ttu-id="b1004-202">例如，套用至物件的</span><span class="sxs-lookup"><span data-stu-id="b1004-202">For example,</span></span>
```csharp
interface I<out T> { void M<U>() where U : T; }
```
<span data-ttu-id="b1004-203">不合法因為的使用方式`T`的型別參數條件約束為`U`不是輸入安全。</span><span class="sxs-lookup"><span data-stu-id="b1004-203">is illegal because the usage of `T` as a type parameter constraint on `U` is not input-safe.</span></span>

<span data-ttu-id="b1004-204">這項限制已不存在有可能違反類型安全，以下列方式：</span><span class="sxs-lookup"><span data-stu-id="b1004-204">Were this restriction not in place it would be possible to violate type safety in the following manner:</span></span>
```csharp
class B {}
class D : B{}
class E : B {}
class C : I<D> { public void M<U>() {...} }
...
I<B> b = new C();
b.M<E>();
```
<span data-ttu-id="b1004-205">這是實際的呼叫`C.M<E>`。</span><span class="sxs-lookup"><span data-stu-id="b1004-205">This is actually a call to `C.M<E>`.</span></span> <span data-ttu-id="b1004-206">但是這種呼叫需要`E`衍生自`D`，因此這裡違反類型安全。</span><span class="sxs-lookup"><span data-stu-id="b1004-206">But that call requires that `E` derive from `D`, so type safety would be violated here.</span></span>

### <a name="interface-properties"></a><span data-ttu-id="b1004-207">介面內容</span><span class="sxs-lookup"><span data-stu-id="b1004-207">Interface properties</span></span>

<span data-ttu-id="b1004-208">介面屬性使用宣告*interface_property_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="b1004-208">Interface properties are declared using *interface_property_declaration*s:</span></span>

```antlr
interface_property_declaration
    : attributes? 'new'? type identifier '{' interface_accessors '}'
    ;

interface_accessors
    : attributes? 'get' ';'
    | attributes? 'set' ';'
    | attributes? 'get' ';' attributes? 'set' ';'
    | attributes? 'set' ';' attributes? 'get' ';'
    ;
```

<span data-ttu-id="b1004-209">*屬性*，*型別*，並*識別碼*介面屬性宣告的類別中有意義的屬性宣告相同 ([屬性](classes.md#properties))。</span><span class="sxs-lookup"><span data-stu-id="b1004-209">The *attributes*, *type*, and *identifier* of an interface property declaration have the same meaning as those of a property declaration in a class ([Properties](classes.md#properties)).</span></span>

<span data-ttu-id="b1004-210">介面屬性宣告的存取子對應的類別屬性宣告存取子 ([存取子](classes.md#accessors))，不同之處在於存取子主體必須是分號。</span><span class="sxs-lookup"><span data-stu-id="b1004-210">The accessors of an interface property declaration correspond to the accessors of a class property declaration ([Accessors](classes.md#accessors)), except that the accessor body must always be a semicolon.</span></span> <span data-ttu-id="b1004-211">因此，存取子只是指出屬性是讀寫、 唯讀或唯寫。</span><span class="sxs-lookup"><span data-stu-id="b1004-211">Thus, the accessors simply indicate whether the property is read-write, read-only, or write-only.</span></span>

<span data-ttu-id="b1004-212">介面屬性的類型必須是輸出-安全，如果沒有 get 存取子，以及必須輸入為安全，如果沒有 set 存取子。</span><span class="sxs-lookup"><span data-stu-id="b1004-212">The type of an interface property must be output-safe if there is a get accessor, and must be input-safe if there is a set accessor.</span></span>

### <a name="interface-events"></a><span data-ttu-id="b1004-213">介面事件</span><span class="sxs-lookup"><span data-stu-id="b1004-213">Interface events</span></span>

<span data-ttu-id="b1004-214">使用宣告介面事件*interface_event_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="b1004-214">Interface events are declared using *interface_event_declaration*s:</span></span>

```antlr
interface_event_declaration
    : attributes? 'new'? 'event' type identifier ';'
    ;
```

<span data-ttu-id="b1004-215">*屬性*，*型別*，並*識別碼*介面事件宣告的類別中有相同的意義相同的事件宣告 ([事件](classes.md#events)).</span><span class="sxs-lookup"><span data-stu-id="b1004-215">The *attributes*, *type*, and *identifier* of an interface event declaration have the same meaning as those of an event declaration in a class ([Events](classes.md#events)).</span></span>

<span data-ttu-id="b1004-216">介面事件的型別必須輸入為安全。</span><span class="sxs-lookup"><span data-stu-id="b1004-216">The type of an interface event must be input-safe.</span></span>

### <a name="interface-indexers"></a><span data-ttu-id="b1004-217">介面索引子</span><span class="sxs-lookup"><span data-stu-id="b1004-217">Interface indexers</span></span>

<span data-ttu-id="b1004-218">使用宣告介面索引子*interface_indexer_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="b1004-218">Interface indexers are declared using *interface_indexer_declaration*s:</span></span>

```antlr
interface_indexer_declaration
    : attributes? 'new'? type 'this' '[' formal_parameter_list ']' '{' interface_accessors '}'
    ;
```

<span data-ttu-id="b1004-219">*屬性*，*型別*，並*formal_parameter_list*介面索引子宣告的有意義相同的索引子宣告中的類別 ([索引子](classes.md#indexers))。</span><span class="sxs-lookup"><span data-stu-id="b1004-219">The *attributes*, *type*, and *formal_parameter_list* of an interface indexer declaration have the same meaning as those of an indexer declaration in a class ([Indexers](classes.md#indexers)).</span></span>

<span data-ttu-id="b1004-220">介面索引子宣告的存取子對應的類別索引子宣告存取子 ([索引子](classes.md#indexers))，不同之處在於存取子主體必須是分號。</span><span class="sxs-lookup"><span data-stu-id="b1004-220">The accessors of an interface indexer declaration correspond to the accessors of a class indexer declaration ([Indexers](classes.md#indexers)), except that the accessor body must always be a semicolon.</span></span> <span data-ttu-id="b1004-221">因此，存取子只是表示索引子是讀寫、 唯讀或唯寫。</span><span class="sxs-lookup"><span data-stu-id="b1004-221">Thus, the accessors simply indicate whether the indexer is read-write, read-only, or write-only.</span></span>

<span data-ttu-id="b1004-222">介面索引子的型式參數類型必須是輸入安全。</span><span class="sxs-lookup"><span data-stu-id="b1004-222">All the formal parameter types of an interface indexer must be input-safe .</span></span> <span data-ttu-id="b1004-223">此外，任何`out`或`ref`型式參數類型必須也是輸出安全。</span><span class="sxs-lookup"><span data-stu-id="b1004-223">In addition, any `out` or `ref` formal parameter types must also be output-safe.</span></span> <span data-ttu-id="b1004-224">請注意，即使`out`參數必須是輸入安全，因為基礎執行平台的限制。</span><span class="sxs-lookup"><span data-stu-id="b1004-224">Note that even `out` parameters are required to be input-safe, due to a limitation of the underlying execution platform.</span></span>

<span data-ttu-id="b1004-225">介面索引子的型別必須是輸出-安全，如果沒有 get 存取子，以及必須輸入為安全，如果沒有 set 存取子。</span><span class="sxs-lookup"><span data-stu-id="b1004-225">The type of an interface indexer must be output-safe if there is a get accessor, and must be input-safe if there is a set accessor.</span></span>

### <a name="interface-member-access"></a><span data-ttu-id="b1004-226">介面成員存取</span><span class="sxs-lookup"><span data-stu-id="b1004-226">Interface member access</span></span>

<span data-ttu-id="b1004-227">成員存取透過存取介面成員 ([成員存取](expressions.md#member-access)) 和索引子存取 ([索引子存取](expressions.md#indexer-access)) 形式的運算式`I.M`並`I[A]`，其中`I`介面類型時，`M`是方法、 屬性或事件，該介面型別，以及`A`是索引子引數清單。</span><span class="sxs-lookup"><span data-stu-id="b1004-227">Interface members are accessed through member access ([Member access](expressions.md#member-access)) and indexer access ([Indexer access](expressions.md#indexer-access)) expressions of the form `I.M` and `I[A]`, where `I` is an interface type, `M` is a method, property, or event of that interface type, and `A` is an indexer argument list.</span></span>

<span data-ttu-id="b1004-228">絕對的介面的單一繼承 （每個介面繼承鏈結中的有剛好零個或一個直接的基底介面）、 成員查詢的影響 ([成員查閱](expressions.md#member-lookup))，方法引動過程 ([方法引動過程](expressions.md#method-invocations))，和索引子存取 ([索引子存取](expressions.md#indexer-access)) 規則並完全與類別和結構相同：更多衍生成員隱藏較少衍生的成員具有相同名稱或簽章。</span><span class="sxs-lookup"><span data-stu-id="b1004-228">For interfaces that are strictly single-inheritance (each interface in the inheritance chain has exactly zero or one direct base interface), the effects of the member lookup ([Member lookup](expressions.md#member-lookup)), method invocation ([Method invocations](expressions.md#method-invocations)), and indexer access ([Indexer access](expressions.md#indexer-access)) rules are exactly the same as for classes and structs: More derived members hide less derived members with the same name or signature.</span></span> <span data-ttu-id="b1004-229">不過，如多重繼承的介面，可能會發生模稜兩可，若有兩個或多個不相關的基底介面宣告具有相同的名稱或簽章的成員。</span><span class="sxs-lookup"><span data-stu-id="b1004-229">However, for multiple-inheritance interfaces, ambiguities can occur when two or more unrelated base interfaces declare members with the same name or signature.</span></span> <span data-ttu-id="b1004-230">本節說明幾個範例，這種情況。</span><span class="sxs-lookup"><span data-stu-id="b1004-230">This section shows several examples of such situations.</span></span> <span data-ttu-id="b1004-231">在所有情況下，明確轉換 （cast） 可用來解決模稜兩可。</span><span class="sxs-lookup"><span data-stu-id="b1004-231">In all cases, explicit casts can be used to resolve the ambiguities.</span></span>

<span data-ttu-id="b1004-232">在範例</span><span class="sxs-lookup"><span data-stu-id="b1004-232">In the example</span></span>
```csharp
interface IList
{
    int Count { get; set; }
}

interface ICounter
{
    void Count(int i);
}

interface IListCounter: IList, ICounter {}

class C
{
    void Test(IListCounter x) {
        x.Count(1);                  // Error
        x.Count = 1;                 // Error
        ((IList)x).Count = 1;        // Ok, invokes IList.Count.set
        ((ICounter)x).Count(1);      // Ok, invokes ICounter.Count
    }
}
```
<span data-ttu-id="b1004-233">前兩個陳述式會造成編譯時期錯誤，因為成員查閱 ([成員查閱](expressions.md#member-lookup)) 的`Count`在`IListCounter`模稜兩可。</span><span class="sxs-lookup"><span data-stu-id="b1004-233">the first two statements cause compile-time errors because the member lookup ([Member lookup](expressions.md#member-lookup)) of `Count` in `IListCounter` is ambiguous.</span></span> <span data-ttu-id="b1004-234">如範例所示，透過轉型來解決模稜兩可`x`成適當的基底介面型別。</span><span class="sxs-lookup"><span data-stu-id="b1004-234">As illustrated by the example, the ambiguity is resolved by casting `x` to the appropriate base interface type.</span></span> <span data-ttu-id="b1004-235">這類轉換不需要執行階段成本 — 它們只包含做為衍生程度更小的類型在編譯時期檢視的執行個體。</span><span class="sxs-lookup"><span data-stu-id="b1004-235">Such casts have no run-time costs—they merely consist of viewing the instance as a less derived type at compile-time.</span></span>

<span data-ttu-id="b1004-236">在範例</span><span class="sxs-lookup"><span data-stu-id="b1004-236">In the example</span></span>
```csharp
interface IInteger
{
    void Add(int i);
}

interface IDouble
{
    void Add(double d);
}

interface INumber: IInteger, IDouble {}

class C
{
    void Test(INumber n) {
        n.Add(1);                // Invokes IInteger.Add
        n.Add(1.0);              // Only IDouble.Add is applicable
        ((IInteger)n).Add(1);    // Only IInteger.Add is a candidate
        ((IDouble)n).Add(1);     // Only IDouble.Add is a candidate
    }
}
```
<span data-ttu-id="b1004-237">引動過程`n.Add(1)`選取`IInteger.Add`藉由套用的多載解析規則[多載解析](expressions.md#overload-resolution)。</span><span class="sxs-lookup"><span data-stu-id="b1004-237">the invocation `n.Add(1)` selects `IInteger.Add` by applying the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="b1004-238">同樣的引動過程`n.Add(1.0)`選取`IDouble.Add`。</span><span class="sxs-lookup"><span data-stu-id="b1004-238">Similarly the invocation `n.Add(1.0)` selects `IDouble.Add`.</span></span> <span data-ttu-id="b1004-239">當插入明確的轉型時，是只有一個候選項目方法，並因此沒有模稜兩可。</span><span class="sxs-lookup"><span data-stu-id="b1004-239">When explicit casts are inserted, there is only one candidate method, and thus no ambiguity.</span></span>

<span data-ttu-id="b1004-240">在範例</span><span class="sxs-lookup"><span data-stu-id="b1004-240">In the example</span></span>
```csharp
interface IBase
{
    void F(int i);
}

interface ILeft: IBase
{
    new void F(int i);
}

interface IRight: IBase
{
    void G();
}

interface IDerived: ILeft, IRight {}

class A
{
    void Test(IDerived d) {
        d.F(1);                 // Invokes ILeft.F
        ((IBase)d).F(1);        // Invokes IBase.F
        ((ILeft)d).F(1);        // Invokes ILeft.F
        ((IRight)d).F(1);       // Invokes IBase.F
    }
}
```
<span data-ttu-id="b1004-241">`IBase.F`藉由隱藏成員`ILeft.F`成員。</span><span class="sxs-lookup"><span data-stu-id="b1004-241">the `IBase.F` member is hidden by the `ILeft.F` member.</span></span> <span data-ttu-id="b1004-242">引動過程`d.F(1)`因此會選取`ILeft.F`，即使`IBase.F`似乎不會逐步引導的存取路徑中隱藏`IRight`。</span><span class="sxs-lookup"><span data-stu-id="b1004-242">The invocation `d.F(1)` thus selects `ILeft.F`, even though `IBase.F` appears to not be hidden in the access path that leads through `IRight`.</span></span>

<span data-ttu-id="b1004-243">多重繼承的介面中隱藏的直覺式規則只是這個：如果成員隱藏的任何存取路徑中，它會隱藏所有的存取路徑中。</span><span class="sxs-lookup"><span data-stu-id="b1004-243">The intuitive rule for hiding in multiple-inheritance interfaces is simply this: If a member is hidden in any access path, it is hidden in all access paths.</span></span> <span data-ttu-id="b1004-244">因為存取路徑，從`IDerived`來`ILeft`要`IBase`隱藏`IBase.F`，從的存取路徑中的成員也在隱藏`IDerived`來`IRight`來`IBase`。</span><span class="sxs-lookup"><span data-stu-id="b1004-244">Because the access path from `IDerived` to `ILeft` to `IBase` hides `IBase.F`, the member is also hidden in the access path from `IDerived` to `IRight` to `IBase`.</span></span>

## <a name="fully-qualified-interface-member-names"></a><span data-ttu-id="b1004-245">完整的介面成員名稱</span><span class="sxs-lookup"><span data-stu-id="b1004-245">Fully qualified interface member names</span></span>

<span data-ttu-id="b1004-246">介面成員有時是由其***完整限定的名稱***。</span><span class="sxs-lookup"><span data-stu-id="b1004-246">An interface member is sometimes referred to by its ***fully qualified name***.</span></span> <span data-ttu-id="b1004-247">介面成員的完整的名稱是由在其中成員是宣告，後面接著一個點，再加上該成員名稱的介面名稱所組成。</span><span class="sxs-lookup"><span data-stu-id="b1004-247">The fully qualified name of an interface member consists of the name of the interface in which the member is declared, followed by a dot, followed by the name of the member.</span></span> <span data-ttu-id="b1004-248">成員的完整限定的名稱參考該成員所宣告的介面。</span><span class="sxs-lookup"><span data-stu-id="b1004-248">The fully qualified name of a member references the interface in which the member is declared.</span></span> <span data-ttu-id="b1004-249">例如，假設宣告</span><span class="sxs-lookup"><span data-stu-id="b1004-249">For example, given the declarations</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}
```
<span data-ttu-id="b1004-250">完整限定的名稱`Paint`是`IControl.Paint`和 完整格式的名稱`SetText`是`ITextBox.SetText`。</span><span class="sxs-lookup"><span data-stu-id="b1004-250">the fully qualified name of `Paint` is `IControl.Paint` and the fully qualified name of `SetText` is `ITextBox.SetText`.</span></span>

<span data-ttu-id="b1004-251">在上述範例中，它不可以參考`Paint`做為`ITextBox.Paint`。</span><span class="sxs-lookup"><span data-stu-id="b1004-251">In the example above, it is not possible to refer to `Paint` as `ITextBox.Paint`.</span></span>

<span data-ttu-id="b1004-252">命名空間的一部分介面時，介面成員的完整的名稱包括命名空間名稱。</span><span class="sxs-lookup"><span data-stu-id="b1004-252">When an interface is part of a namespace, the fully qualified name of an interface member includes the namespace name.</span></span> <span data-ttu-id="b1004-253">例如</span><span class="sxs-lookup"><span data-stu-id="b1004-253">For example</span></span>
```csharp
namespace System
{
    public interface ICloneable
    {
        object Clone();
    }
}
```

<span data-ttu-id="b1004-254">這裡，完整限定的名稱`Clone`方法是`System.ICloneable.Clone`。</span><span class="sxs-lookup"><span data-stu-id="b1004-254">Here, the fully qualified name of the `Clone` method is `System.ICloneable.Clone`.</span></span>

## <a name="interface-implementations"></a><span data-ttu-id="b1004-255">介面實作</span><span class="sxs-lookup"><span data-stu-id="b1004-255">Interface implementations</span></span>

<span data-ttu-id="b1004-256">可由類別和結構實作介面。</span><span class="sxs-lookup"><span data-stu-id="b1004-256">Interfaces may be implemented by classes and structs.</span></span> <span data-ttu-id="b1004-257">若要表示類別或結構直接實作的介面，基底類別清單的類別或結構中包含的介面識別項。</span><span class="sxs-lookup"><span data-stu-id="b1004-257">To indicate that a class or struct directly implements an interface, the interface identifier is included in the base class list of the class or struct.</span></span> <span data-ttu-id="b1004-258">例如: </span><span class="sxs-lookup"><span data-stu-id="b1004-258">For example:</span></span>
```csharp
interface ICloneable
{
    object Clone();
}

interface IComparable
{
    int CompareTo(object other);
}

class ListEntry: ICloneable, IComparable
{
    public object Clone() {...}
    public int CompareTo(object other) {...}
}
```

<span data-ttu-id="b1004-259">類別或結構，直接也可以直接實作的介面會實作所有介面的基底介面的隱含。</span><span class="sxs-lookup"><span data-stu-id="b1004-259">A class or struct that directly implements an interface also directly implements all of the interface's base interfaces implicitly.</span></span> <span data-ttu-id="b1004-260">這是 true，即使類別或結構並未明確列出基底類別清單中的所有基底介面。</span><span class="sxs-lookup"><span data-stu-id="b1004-260">This is true even if the class or struct doesn't explicitly list all base interfaces in the base class list.</span></span> <span data-ttu-id="b1004-261">例如: </span><span class="sxs-lookup"><span data-stu-id="b1004-261">For example:</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

class TextBox: ITextBox
{
    public void Paint() {...}
    public void SetText(string text) {...}
}
```

<span data-ttu-id="b1004-262">在這裡，類別`TextBox`會實作`IControl`和`ITextBox`。</span><span class="sxs-lookup"><span data-stu-id="b1004-262">Here, class `TextBox` implements both `IControl` and `ITextBox`.</span></span>

<span data-ttu-id="b1004-263">當類別`C`直接實作一個介面，衍生自 C 的所有類別也隱含實作介面。</span><span class="sxs-lookup"><span data-stu-id="b1004-263">When a class `C` directly implements an interface, all classes derived from C also implement the interface implicitly.</span></span> <span data-ttu-id="b1004-264">類別宣告中指定的基底介面可以是建構的介面型別 ([建構類型](types.md#constructed-types))。</span><span class="sxs-lookup"><span data-stu-id="b1004-264">The base interfaces specified in a class declaration can be constructed interface types ([Constructed types](types.md#constructed-types)).</span></span> <span data-ttu-id="b1004-265">基底介面不能，型別參數，但它可以包含在範圍中的型別參數。</span><span class="sxs-lookup"><span data-stu-id="b1004-265">A base interface cannot be a type parameter on its own, though it can involve the type parameters that are in scope.</span></span> <span data-ttu-id="b1004-266">下列程式碼說明如何實作類別和擴充建構的類型：</span><span class="sxs-lookup"><span data-stu-id="b1004-266">The following code illustrates how a class can implement and extend constructed types:</span></span>
```csharp
class C<U,V> {}

interface I1<V> {}

class D: C<string,int>, I1<string> {}

class E<T>: C<int,T>, I1<T> {}
```

<span data-ttu-id="b1004-267">泛型類別宣告基底介面都必須滿足唯一性規則中所述[實作的介面的唯一性](interfaces.md#uniqueness-of-implemented-interfaces)。</span><span class="sxs-lookup"><span data-stu-id="b1004-267">The base interfaces of a generic class declaration must satisfy the uniqueness rule described in [Uniqueness of implemented interfaces](interfaces.md#uniqueness-of-implemented-interfaces).</span></span>

### <a name="explicit-interface-member-implementations"></a><span data-ttu-id="b1004-268">明確介面成員實作</span><span class="sxs-lookup"><span data-stu-id="b1004-268">Explicit interface member implementations</span></span>

<span data-ttu-id="b1004-269">為了實作介面，類別或結構可以宣告***明確介面成員實作***。</span><span class="sxs-lookup"><span data-stu-id="b1004-269">For purposes of implementing interfaces, a class or struct may declare ***explicit interface member implementations***.</span></span> <span data-ttu-id="b1004-270">明確介面成員實作是方法、 屬性、 事件或索引子的宣告參考完整的介面成員名稱。</span><span class="sxs-lookup"><span data-stu-id="b1004-270">An explicit interface member implementation is a method, property, event, or indexer declaration that references a fully qualified interface member name.</span></span> <span data-ttu-id="b1004-271">例如</span><span class="sxs-lookup"><span data-stu-id="b1004-271">For example</span></span>
```csharp
interface IList<T>
{
    T[] GetElements();
}

interface IDictionary<K,V>
{
    V this[K key];
    void Add(K key, V value);
}

class List<T>: IList<T>, IDictionary<int,T>
{
    T[] IList<T>.GetElements() {...}
    T IDictionary<int,T>.this[int index] {...}
    void IDictionary<int,T>.Add(int index, T value) {...}
}
```

<span data-ttu-id="b1004-272">以下`IDictionary<int,T>.this`和`IDictionary<int,T>.Add`是明確介面成員實作。</span><span class="sxs-lookup"><span data-stu-id="b1004-272">Here `IDictionary<int,T>.this` and `IDictionary<int,T>.Add` are explicit interface member implementations.</span></span>

<span data-ttu-id="b1004-273">在某些情況下，可能無法適用於實作的類別，在此案例介面成員可能會使用實作明確介面成員實作介面成員的名稱。</span><span class="sxs-lookup"><span data-stu-id="b1004-273">In some cases, the name of an interface member may not be appropriate for the implementing class, in which case the interface member may be implemented using explicit interface member implementation.</span></span> <span data-ttu-id="b1004-274">類別實作檔案抽象概念，例如，可能會實作`Close`釋出的檔案資源的影響，而且實作的成員函式`Dispose`方法`IDisposable`介面使用明確的介面成員的實作：</span><span class="sxs-lookup"><span data-stu-id="b1004-274">A class implementing a file abstraction, for example, would likely implement a `Close` member function that has the effect of releasing the file resource, and implement the `Dispose` method of the `IDisposable` interface using explicit interface member implementation:</span></span>
```csharp
interface IDisposable
{
    void Dispose();
}

class MyFile: IDisposable
{
    void IDisposable.Dispose() {
        Close();
    }

    public void Close() {
        // Do what's necessary to close the file
        System.GC.SuppressFinalize(this);
    }
}
```

<span data-ttu-id="b1004-275">您不可能透過方法引動過程、 屬性存取或索引子存取中的完整限定名稱存取的明確介面成員實作。</span><span class="sxs-lookup"><span data-stu-id="b1004-275">It is not possible to access an explicit interface member implementation through its fully qualified name in a method invocation, property access, or indexer access.</span></span> <span data-ttu-id="b1004-276">明確介面成員實作只能透過介面執行個體，來存取，並在此情況下直接參考其成員名稱。</span><span class="sxs-lookup"><span data-stu-id="b1004-276">An explicit interface member implementation can only be accessed through an interface instance, and is in that case referenced simply by its member name.</span></span>

<span data-ttu-id="b1004-277">它是編譯時期錯誤，包括存取修飾詞，明確介面成員實作，它是編譯時期錯誤包含修飾詞`abstract`， `virtual`， `override`，或`static`。</span><span class="sxs-lookup"><span data-stu-id="b1004-277">It is a compile-time error for an explicit interface member implementation to include access modifiers, and it is a compile-time error to include the modifiers `abstract`, `virtual`, `override`, or `static`.</span></span>

<span data-ttu-id="b1004-278">明確介面成員實作各有不同的協助工具比其他成員的特性。</span><span class="sxs-lookup"><span data-stu-id="b1004-278">Explicit interface member implementations have different accessibility characteristics than other members.</span></span> <span data-ttu-id="b1004-279">因為永遠不會存取其完整名稱中的方法引動過程或屬性存取透過明確介面成員實作，它們是在某種意義上私用。</span><span class="sxs-lookup"><span data-stu-id="b1004-279">Because explicit interface member implementations are never accessible through their fully qualified name in a method invocation or a property access, they are in a sense private.</span></span> <span data-ttu-id="b1004-280">不過，它們可以透過介面執行個體，因為它們是在某種意義上也是公用。</span><span class="sxs-lookup"><span data-stu-id="b1004-280">However, since they can be accessed through an interface instance, they are in a sense also public.</span></span>

<span data-ttu-id="b1004-281">明確介面成員實作有兩個主要的用途：</span><span class="sxs-lookup"><span data-stu-id="b1004-281">Explicit interface member implementations serve two primary purposes:</span></span>

*  <span data-ttu-id="b1004-282">因為無法存取類別或結構的執行個體透過明確介面成員實作，它們就會允許要排除之公用介面的類別或結構的介面實作。</span><span class="sxs-lookup"><span data-stu-id="b1004-282">Because explicit interface member implementations are not accessible through class or struct instances, they allow interface implementations to be excluded from the public interface of a class or struct.</span></span> <span data-ttu-id="b1004-283">此方法特別有用的類別或結構實作不感興趣的取用者，該類別或結構的內部介面。</span><span class="sxs-lookup"><span data-stu-id="b1004-283">This is particularly useful when a class or struct implements an internal interface that is of no interest to a consumer of that class or struct.</span></span>
*  <span data-ttu-id="b1004-284">明確介面成員實作可讓有相同的簽章的介面成員的去除混淆。</span><span class="sxs-lookup"><span data-stu-id="b1004-284">Explicit interface member implementations allow disambiguation of interface members with the same signature.</span></span> <span data-ttu-id="b1004-285">不含明確介面成員實作它是類別或結構不可能有不同的實作的介面具有相同的簽章的成員，並傳回型別，做為它是不可能的類別或結構有任何實作在所有具有相同的簽章，但具有不同的介面成員的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="b1004-285">Without explicit interface member implementations it would be impossible for a class or struct to have different implementations of interface members with the same signature and return type, as would it be impossible for a class or struct to have any implementation at all of interface members with the same signature but with different return types.</span></span>

<span data-ttu-id="b1004-286">有效明確介面成員實作，類別或結構必須命名為其基底類別清單中，包含的成員，其完整的名稱、 類型和參數類型完全相符的明確介面成員的介面實作。</span><span class="sxs-lookup"><span data-stu-id="b1004-286">For an explicit interface member implementation to be valid, the class or struct must name an interface in its base class list that contains a member whose fully qualified name, type, and parameter types exactly match those of the explicit interface member implementation.</span></span> <span data-ttu-id="b1004-287">因此，在下列類別</span><span class="sxs-lookup"><span data-stu-id="b1004-287">Thus, in the following class</span></span>
```csharp
class Shape: ICloneable
{
    object ICloneable.Clone() {...}
    int IComparable.CompareTo(object other) {...}    // invalid
}
```
<span data-ttu-id="b1004-288">deklarace`IComparable.CompareTo`導致編譯時期錯誤，因為`IComparable`基底類別清單中未列出`Shape`並不是基底介面的`ICloneable`。</span><span class="sxs-lookup"><span data-stu-id="b1004-288">the declaration of `IComparable.CompareTo` results in a compile-time error because `IComparable` is not listed in the base class list of `Shape` and is not a base interface of `ICloneable`.</span></span> <span data-ttu-id="b1004-289">同樣地，在宣告中</span><span class="sxs-lookup"><span data-stu-id="b1004-289">Likewise, in the declarations</span></span>
```csharp
class Shape: ICloneable
{
    object ICloneable.Clone() {...}
}

class Ellipse: Shape
{
    object ICloneable.Clone() {...}    // invalid
}
```
<span data-ttu-id="b1004-290">deklarace`ICloneable.Clone`中`Ellipse`導致編譯時期錯誤，因為`ICloneable`基底類別清單中未明確列出`Ellipse`。</span><span class="sxs-lookup"><span data-stu-id="b1004-290">the declaration of `ICloneable.Clone` in `Ellipse` results in a compile-time error because `ICloneable` is not explicitly listed in the base class list of `Ellipse`.</span></span>

<span data-ttu-id="b1004-291">介面成員的完整的名稱必須參考該成員已宣告的介面。</span><span class="sxs-lookup"><span data-stu-id="b1004-291">The fully qualified name of an interface member must reference the interface in which the member was declared.</span></span> <span data-ttu-id="b1004-292">因此，在宣告中</span><span class="sxs-lookup"><span data-stu-id="b1004-292">Thus, in the declarations</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

class TextBox: ITextBox
{
    void IControl.Paint() {...}
    void ITextBox.SetText(string text) {...}
}
```
<span data-ttu-id="b1004-293">明確介面成員實作`Paint`必須寫成`IControl.Paint`。</span><span class="sxs-lookup"><span data-stu-id="b1004-293">the explicit interface member implementation of `Paint` must be written as `IControl.Paint`.</span></span>

### <a name="uniqueness-of-implemented-interfaces"></a><span data-ttu-id="b1004-294">實作介面的唯一性</span><span class="sxs-lookup"><span data-stu-id="b1004-294">Uniqueness of implemented interfaces</span></span>

<span data-ttu-id="b1004-295">泛型型別宣告所實作的介面必須維持唯一的所有可能的建構型別。</span><span class="sxs-lookup"><span data-stu-id="b1004-295">The interfaces implemented by a generic type declaration must remain unique for all possible constructed types.</span></span> <span data-ttu-id="b1004-296">而不需要這項規則，就無法判斷要呼叫的特定建構類型的正確方法。</span><span class="sxs-lookup"><span data-stu-id="b1004-296">Without this rule, it would be impossible to determine the correct method to call for certain constructed types.</span></span> <span data-ttu-id="b1004-297">例如，假設允許泛型類別宣告的可寫入，如下所示：</span><span class="sxs-lookup"><span data-stu-id="b1004-297">For example, suppose a generic class declaration were permitted to be written as follows:</span></span>
```csharp
interface I<T>
{
    void F();
}

class X<U,V>: I<U>, I<V>                    // Error: I<U> and I<V> conflict
{
    void I<U>.F() {...}
    void I<V>.F() {...}
}
```

<span data-ttu-id="b1004-298">在允許這種情況，它會無法判斷哪一個程式碼執行在下列情況：</span><span class="sxs-lookup"><span data-stu-id="b1004-298">Were this permitted, it would be impossible to determine which code to execute in the following case:</span></span>
```csharp
I<int> x = new X<int,int>();
x.F();
```

<span data-ttu-id="b1004-299">若要判斷泛型類型宣告的介面清單是否有效，執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="b1004-299">To determine if the interface list of a generic type declaration is valid, the following steps are performed:</span></span>

*  <span data-ttu-id="b1004-300">可讓`L`是直接在泛型類別、 結構或介面宣告中指定的介面清單`C`。</span><span class="sxs-lookup"><span data-stu-id="b1004-300">Let `L` be the list of interfaces directly specified in a generic class, struct, or interface declaration `C`.</span></span>
*  <span data-ttu-id="b1004-301">若要新增`L`任何基底介面的介面已經在`L`。</span><span class="sxs-lookup"><span data-stu-id="b1004-301">Add to `L` any base interfaces of the interfaces already in `L`.</span></span>
*  <span data-ttu-id="b1004-302">移除任何重複項目從`L`。</span><span class="sxs-lookup"><span data-stu-id="b1004-302">Remove any duplicates from `L`.</span></span>
*  <span data-ttu-id="b1004-303">如果任何可能會建構從建立的型別`C`型別引數會替代至之後會`L`，會導致在兩個介面`L`完全一樣的宣告`C`無效。</span><span class="sxs-lookup"><span data-stu-id="b1004-303">If any possible constructed type created from `C` would, after type arguments are substituted into `L`, cause two interfaces in `L` to be identical, then the declaration of `C` is invalid.</span></span> <span data-ttu-id="b1004-304">決定所有可能的建構型別時，不會考慮條件約束宣告。</span><span class="sxs-lookup"><span data-stu-id="b1004-304">Constraint declarations are not considered when determining all possible constructed types.</span></span>

<span data-ttu-id="b1004-305">在類別宣告`X`介面清單上方`L`組成`I<U>`和`I<V>`。</span><span class="sxs-lookup"><span data-stu-id="b1004-305">In the class declaration `X` above, the interface list `L` consists of `I<U>` and `I<V>`.</span></span> <span data-ttu-id="b1004-306">宣告無效，因為任何建構的型別`U`和`V`正在相同的型別會導致這兩個介面是相同的型別。</span><span class="sxs-lookup"><span data-stu-id="b1004-306">The declaration is invalid because any constructed type with `U` and `V` being the same type would cause these two interfaces to be identical types.</span></span>

<span data-ttu-id="b1004-307">您可在整合不同的繼承層級指定的介面：</span><span class="sxs-lookup"><span data-stu-id="b1004-307">It is possible for interfaces specified at different inheritance levels to unify:</span></span>
```csharp
interface I<T>
{
    void F();
}

class Base<U>: I<U>
{
    void I<U>.F() {...}
}

class Derived<U,V>: Base<U>, I<V>    // Ok
{
    void I<V>.F() {...}
}
```

<span data-ttu-id="b1004-308">此程式碼無效，即使`Derived<U,V>`會實作`I<U>`和`I<V>`。</span><span class="sxs-lookup"><span data-stu-id="b1004-308">This code is valid even though `Derived<U,V>` implements both `I<U>` and `I<V>`.</span></span> <span data-ttu-id="b1004-309">程式碼</span><span class="sxs-lookup"><span data-stu-id="b1004-309">The code</span></span>
```csharp
I<int> x = new Derived<int,int>();
x.F();
```
<span data-ttu-id="b1004-310">叫用的方法`Derived`，因為`Derived<int,int>`有效地重新實作`I<int>`([重新實作的介面](interfaces.md#interface-re-implementation))。</span><span class="sxs-lookup"><span data-stu-id="b1004-310">invokes the method in `Derived`, since `Derived<int,int>` effectively re-implements `I<int>` ([Interface re-implementation](interfaces.md#interface-re-implementation)).</span></span>

### <a name="implementation-of-generic-methods"></a><span data-ttu-id="b1004-311">泛型方法的實作</span><span class="sxs-lookup"><span data-stu-id="b1004-311">Implementation of generic methods</span></span>

<span data-ttu-id="b1004-312">當泛型方法會隱含地實作介面方法，指定每個方法類型參數必須是兩個宣告中的對等 （之後任何介面類型會以適當的型別引數取代參數），其中的條件約束方法型別參數被識別序數位置，由左到右。</span><span class="sxs-lookup"><span data-stu-id="b1004-312">When a generic method implicitly implements an interface method, the constraints given for each method type parameter must be equivalent in both declarations (after any interface type parameters are replaced with the appropriate type arguments), where method type parameters are identified by ordinal positions, left to right.</span></span>

<span data-ttu-id="b1004-313">當泛型方法明確實作介面方法時，不過，不允許條件約束實作的方法。</span><span class="sxs-lookup"><span data-stu-id="b1004-313">When a generic method explicitly implements an interface method, however, no constraints are allowed on the implementing method.</span></span> <span data-ttu-id="b1004-314">相反地，條件約束被繼承自介面方法</span><span class="sxs-lookup"><span data-stu-id="b1004-314">Instead, the constraints are inherited from the interface method</span></span>

```csharp
interface I<A,B,C>
{
    void F<T>(T t) where T: A;
    void G<T>(T t) where T: B;
    void H<T>(T t) where T: C;
}

class C: I<object,C,string>
{
    public void F<T>(T t) {...}                    // Ok
    public void G<T>(T t) where T: C {...}         // Ok
    public void H<T>(T t) where T: string {...}    // Error
}
```

<span data-ttu-id="b1004-315">此方法`C.F<T>`隱含地實作`I<object,C,string>.F<T>`。</span><span class="sxs-lookup"><span data-stu-id="b1004-315">The method `C.F<T>` implicitly implements `I<object,C,string>.F<T>`.</span></span> <span data-ttu-id="b1004-316">在此情況下，`C.F<T>`不需要 （也不會允許） 指定的條件約束`T:object`因為`object`是隱含的所有類型參數條件約束。</span><span class="sxs-lookup"><span data-stu-id="b1004-316">In this case, `C.F<T>` is not required (nor permitted) to specify the constraint `T:object` since `object` is an implicit constraint on all type parameters.</span></span> <span data-ttu-id="b1004-317">此方法`C.G<T>`隱含地實作`I<object,C,string>.G<T>`因為條件約束符合那些在介面中之後介面型別參數會取代對應的型別引數。</span><span class="sxs-lookup"><span data-stu-id="b1004-317">The method `C.G<T>` implicitly implements `I<object,C,string>.G<T>` because the constraints match those in the interface, after the interface type parameters are replaced with the corresponding type arguments.</span></span> <span data-ttu-id="b1004-318">方法的條件約束`C.H<T>`是錯誤，因為密封類型 (`string`在此情況下) 無法作為條件約束。</span><span class="sxs-lookup"><span data-stu-id="b1004-318">The constraint for method `C.H<T>` is an error because sealed types (`string` in this case) cannot be used as constraints.</span></span> <span data-ttu-id="b1004-319">省略此條件約束會也會是個錯誤，因為隱含的介面方法實作的條件約束，才能符合。</span><span class="sxs-lookup"><span data-stu-id="b1004-319">Omitting the constraint would also be an error since constraints of implicit interface method implementations are required to match.</span></span> <span data-ttu-id="b1004-320">因此，就無法以隱含方式實作`I<object,C,string>.H<T>`。</span><span class="sxs-lookup"><span data-stu-id="b1004-320">Thus, it is impossible to implicitly implement `I<object,C,string>.H<T>`.</span></span> <span data-ttu-id="b1004-321">這個介面方法只可以使用明確介面成員實作來實作：</span><span class="sxs-lookup"><span data-stu-id="b1004-321">This interface method can only be implemented using an explicit interface member implementation:</span></span>
```csharp
class C: I<object,C,string>
{
    ...

    public void H<U>(U u) where U: class {...}

    void I<object,C,string>.H<T>(T t) {
        string s = t;    // Ok
        H<T>(t);
    }
}
```

<span data-ttu-id="b1004-322">在此範例中，明確介面成員實作會叫用具有嚴格較弱的條件約束的公用方法。</span><span class="sxs-lookup"><span data-stu-id="b1004-322">In this example, the explicit interface member implementation invokes a public method having strictly weaker constraints.</span></span> <span data-ttu-id="b1004-323">請注意，從指派`t`要`s`無效，因為`T`繼承的條件約束`T:string`，即使此條件約束不是在原始程式碼中表示。</span><span class="sxs-lookup"><span data-stu-id="b1004-323">Note that the assignment from `t` to `s` is valid since `T` inherits a constraint of `T:string`, even though this constraint is not expressible in source code.</span></span>

### <a name="interface-mapping"></a><span data-ttu-id="b1004-324">介面對應</span><span class="sxs-lookup"><span data-stu-id="b1004-324">Interface mapping</span></span>

<span data-ttu-id="b1004-325">類別或結構必須提供的介面的類別或結構的基底類別清單中列出的所有成員的實作。</span><span class="sxs-lookup"><span data-stu-id="b1004-325">A class or struct must provide implementations of all members of the interfaces that are listed in the base class list of the class or struct.</span></span> <span data-ttu-id="b1004-326">尋找實作介面成員實作的類別或結構中的程序稱為***介面對應***。</span><span class="sxs-lookup"><span data-stu-id="b1004-326">The process of locating implementations of interface members in an implementing class or struct is known as ***interface mapping***.</span></span>

<span data-ttu-id="b1004-327">類別或結構的介面對應`C`找出每個成員的基底類別清單中指定每個介面的實作`C`。</span><span class="sxs-lookup"><span data-stu-id="b1004-327">Interface mapping for a class or struct `C` locates an implementation for each member of each interface specified in the base class list of `C`.</span></span> <span data-ttu-id="b1004-328">在特定介面成員的實作`I.M`，其中`I`是在其中的介面成員`M`宣告，會檢查每個類別或結構來判斷`S`開始，`C`和針對每個後續的基底類別的重複`C`，直到找到符合的：</span><span class="sxs-lookup"><span data-stu-id="b1004-328">The implementation of a particular interface member `I.M`, where `I` is the interface in which the member `M` is declared, is determined by examining each class or struct `S`, starting with `C` and repeating for each successive base class of `C`, until a match is located:</span></span>

*  <span data-ttu-id="b1004-329">如果`S`包含相符的明確介面成員實作宣告`I`並`M`，則這個成員是實作`I.M`。</span><span class="sxs-lookup"><span data-stu-id="b1004-329">If `S` contains a declaration of an explicit interface member implementation that matches `I` and `M`, then this member is the implementation of `I.M`.</span></span>
*  <span data-ttu-id="b1004-330">否則，如果`S`包含的非靜態的公用成員符合宣告`M`，則這個成員是實作`I.M`。</span><span class="sxs-lookup"><span data-stu-id="b1004-330">Otherwise, if `S` contains a declaration of a non-static public member that matches `M`, then this member is the implementation of `I.M`.</span></span> <span data-ttu-id="b1004-331">如果超過一個成員的相符項目並未指定哪一個成員是實作`I.M`。</span><span class="sxs-lookup"><span data-stu-id="b1004-331">If more than one member matches, it is unspecified which member is the implementation of `I.M`.</span></span> <span data-ttu-id="b1004-332">如果只可以發生這種情況`S`為建構的類型，其中泛型型別中宣告的兩個成員具有不同的簽章，但型別引數，使其簽章相同。</span><span class="sxs-lookup"><span data-stu-id="b1004-332">This situation can only occur if `S` is a constructed type where the two members as declared in the generic type have different signatures, but the type arguments make their signatures identical.</span></span>

<span data-ttu-id="b1004-333">如果實作找不到指定的基底類別清單中的所有介面的所有成員，就會發生編譯時期錯誤`C`。</span><span class="sxs-lookup"><span data-stu-id="b1004-333">A compile-time error occurs if implementations cannot be located for all members of all interfaces specified in the base class list of `C`.</span></span> <span data-ttu-id="b1004-334">請注意，介面的成員會包含這些成員繼承自基底介面。</span><span class="sxs-lookup"><span data-stu-id="b1004-334">Note that the members of an interface include those members that are inherited from base interfaces.</span></span>

<span data-ttu-id="b1004-335">介面對應，類別成員的目的而言`A`符合介面成員`B`時：</span><span class="sxs-lookup"><span data-stu-id="b1004-335">For purposes of interface mapping, a class member `A` matches an interface member `B` when:</span></span>

*  <span data-ttu-id="b1004-336">`A` 並`B`方法，且名稱、 類型、 和型式參數清單`A`和`B`完全相同。</span><span class="sxs-lookup"><span data-stu-id="b1004-336">`A` and `B` are methods, and the name, type, and formal parameter lists of `A` and `B` are identical.</span></span>
*  <span data-ttu-id="b1004-337">`A` 和`B`是屬性的名稱和型別`A`並`B`互異，和`A`具有相同的存取子實作為`B`(`A`不允許有其他存取子，如果它不是明確的介面成員實作）。</span><span class="sxs-lookup"><span data-stu-id="b1004-337">`A` and `B` are properties, the name and type of `A` and `B` are identical, and `A` has the same accessors as `B` (`A` is permitted to have additional accessors if it is not an explicit interface member implementation).</span></span>
*  <span data-ttu-id="b1004-338">`A` 並`B`是事件，及名稱和型別`A`和`B`完全相同。</span><span class="sxs-lookup"><span data-stu-id="b1004-338">`A` and `B` are events, and the name and type of `A` and `B` are identical.</span></span>
*  <span data-ttu-id="b1004-339">`A` 和`B`是索引子的型別和型式參數清單`A`並`B`互異，和`A`具有相同的存取子實作為`B`(`A`不允許有其他存取子，如果不是明確介面成員實作）。</span><span class="sxs-lookup"><span data-stu-id="b1004-339">`A` and `B` are indexers, the type and formal parameter lists of `A` and `B` are identical, and `A` has the same accessors as `B` (`A` is permitted to have additional accessors if it is not an explicit interface member implementation).</span></span>

<span data-ttu-id="b1004-340">值得注意的介面對應演算法的含意如下：</span><span class="sxs-lookup"><span data-stu-id="b1004-340">Notable implications of the interface mapping algorithm are:</span></span>

*  <span data-ttu-id="b1004-341">明確介面成員實作優先於其他相同類別或結構中的成員時決定實作介面成員的類別或結構成員。</span><span class="sxs-lookup"><span data-stu-id="b1004-341">Explicit interface member implementations take precedence over other members in the same class or struct when determining the class or struct member that implements an interface member.</span></span>
*  <span data-ttu-id="b1004-342">或都不非公用靜態成員參與介面對應。</span><span class="sxs-lookup"><span data-stu-id="b1004-342">Neither non-public nor static members participate in interface mapping.</span></span>

<span data-ttu-id="b1004-343">在範例</span><span class="sxs-lookup"><span data-stu-id="b1004-343">In the example</span></span>
```csharp
interface ICloneable
{
    object Clone();
}

class C: ICloneable
{
    object ICloneable.Clone() {...}
    public object Clone() {...}
}
```
<span data-ttu-id="b1004-344">`ICloneable.Clone`隸屬`C`變成實作`Clone`在`ICloneable`因為明確介面成員實作的優先順序高於其他成員。</span><span class="sxs-lookup"><span data-stu-id="b1004-344">the `ICloneable.Clone` member of `C` becomes the implementation of `Clone` in `ICloneable` because explicit interface member implementations take precedence over other members.</span></span>

<span data-ttu-id="b1004-345">如果類別或結構實作了兩個或多個介面，包含具有相同名稱、 類型和參數類型的成員，就可以將每個介面成員拖曳到單一類別或結構成員。</span><span class="sxs-lookup"><span data-stu-id="b1004-345">If a class or struct implements two or more interfaces containing a member with the same name, type, and parameter types, it is possible to map each of those interface members onto a single class or struct member.</span></span> <span data-ttu-id="b1004-346">例如</span><span class="sxs-lookup"><span data-stu-id="b1004-346">For example</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface IForm
{
    void Paint();
}

class Page: IControl, IForm
{
    public void Paint() {...}
}
```

<span data-ttu-id="b1004-347">在這裡，`Paint`這兩種`IControl`並`IForm`會對應到`Paint`中的方法`Page`。</span><span class="sxs-lookup"><span data-stu-id="b1004-347">Here, the `Paint` methods of both `IControl` and `IForm` are mapped onto the `Paint` method in `Page`.</span></span> <span data-ttu-id="b1004-348">當然也很可能有兩種方法的另一個明確介面成員實作。</span><span class="sxs-lookup"><span data-stu-id="b1004-348">It is of course also possible to have separate explicit interface member implementations for the two methods.</span></span>

<span data-ttu-id="b1004-349">如果類別或結構實作介面，包含隱藏的成員，則某些成員必須一定實作透過明確介面成員實作。</span><span class="sxs-lookup"><span data-stu-id="b1004-349">If a class or struct implements an interface that contains hidden members, then some members must necessarily be implemented through explicit interface member implementations.</span></span> <span data-ttu-id="b1004-350">例如</span><span class="sxs-lookup"><span data-stu-id="b1004-350">For example</span></span>
```csharp
interface IBase
{
    int P { get; }
}

interface IDerived: IBase
{
    new int P();
}
```

<span data-ttu-id="b1004-351">此介面的實作需要至少一個明確介面成員實作，並會採用下列格式的其中一個</span><span class="sxs-lookup"><span data-stu-id="b1004-351">An implementation of this interface would require at least one explicit interface member implementation, and would take one of the following forms</span></span>
```csharp
class C: IDerived
{
    int IBase.P { get {...} }
    int IDerived.P() {...}
}

class C: IDerived
{
    public int P { get {...} }
    int IDerived.P() {...}
}

class C: IDerived
{
    int IBase.P { get {...} }
    public int P() {...}
}
```

<span data-ttu-id="b1004-352">當類別實作多個具有相同的基底介面的介面時，可以有只有一個實作的基底介面。</span><span class="sxs-lookup"><span data-stu-id="b1004-352">When a class implements multiple interfaces that have the same base interface, there can be only one implementation of the base interface.</span></span> <span data-ttu-id="b1004-353">在範例</span><span class="sxs-lookup"><span data-stu-id="b1004-353">In the example</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

class ComboBox: IControl, ITextBox, IListBox
{
    void IControl.Paint() {...}
    void ITextBox.SetText(string text) {...}
    void IListBox.SetItems(string[] items) {...}
}
```
<span data-ttu-id="b1004-354">不可以有不同的實作，如`IControl`在基底類別清單中，名為`IControl`繼承`ITextBox`，而`IControl`繼承`IListBox`。</span><span class="sxs-lookup"><span data-stu-id="b1004-354">it is not possible to have separate implementations for the `IControl` named in the base class list, the `IControl` inherited by `ITextBox`, and the `IControl` inherited by `IListBox`.</span></span> <span data-ttu-id="b1004-355">事實上，沒有這些介面的個別身分識別的概念。</span><span class="sxs-lookup"><span data-stu-id="b1004-355">Indeed, there is no notion of a separate identity for these interfaces.</span></span> <span data-ttu-id="b1004-356">相反地，實作`ITextBox`和`IListBox`共用相同的實作`IControl`，和`ComboBox`只會被視為實作三個介面， `IControl`， `ITextBox`，和`IListBox`。</span><span class="sxs-lookup"><span data-stu-id="b1004-356">Rather, the implementations of `ITextBox` and `IListBox` share the same implementation of `IControl`, and `ComboBox` is simply considered to implement three interfaces, `IControl`, `ITextBox`, and `IListBox`.</span></span>

<span data-ttu-id="b1004-357">基底類別的成員會參與介面對應。</span><span class="sxs-lookup"><span data-stu-id="b1004-357">The members of a base class participate in interface mapping.</span></span> <span data-ttu-id="b1004-358">在範例</span><span class="sxs-lookup"><span data-stu-id="b1004-358">In the example</span></span>
```csharp
interface Interface1
{
    void F();
}

class Class1
{
    public void F() {}
    public void G() {}
}

class Class2: Class1, Interface1
{
    new public void G() {}
}
```
<span data-ttu-id="b1004-359">此方法`F`中`Class1`用於`Class2`的實作`Interface1`。</span><span class="sxs-lookup"><span data-stu-id="b1004-359">the method `F` in `Class1` is used in `Class2`'s implementation of `Interface1`.</span></span>

### <a name="interface-implementation-inheritance"></a><span data-ttu-id="b1004-360">介面實作繼承</span><span class="sxs-lookup"><span data-stu-id="b1004-360">Interface implementation inheritance</span></span>

<span data-ttu-id="b1004-361">類別會繼承其基底類別所提供的所有介面實作。</span><span class="sxs-lookup"><span data-stu-id="b1004-361">A class inherits all interface implementations provided by its base classes.</span></span>

<span data-ttu-id="b1004-362">而不需要明確地***重新實作***介面衍生的類別無法以任何方式改變它繼承自其基底類別的介面對應。</span><span class="sxs-lookup"><span data-stu-id="b1004-362">Without explicitly ***re-implementing*** an interface, a derived class cannot in any way alter the interface mappings it inherits from its base classes.</span></span> <span data-ttu-id="b1004-363">例如，在宣告中</span><span class="sxs-lookup"><span data-stu-id="b1004-363">For example, in the declarations</span></span>
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    public void Paint() {...}
}

class TextBox: Control
{
    new public void Paint() {...}
}
```
<span data-ttu-id="b1004-364">`Paint`方法中的`TextBox`隱藏`Paint`中的方法`Control`，但不會更改對應`Control.Paint`拖曳至`IControl.Paint`，和呼叫`Paint`透過類別執行個體和介面執行個體將有下列效果</span><span class="sxs-lookup"><span data-stu-id="b1004-364">the `Paint` method in `TextBox` hides the `Paint` method in `Control`, but it does not alter the mapping of `Control.Paint` onto `IControl.Paint`, and calls to `Paint` through class instances and interface instances will have the following effects</span></span>
```csharp
Control c = new Control();
TextBox t = new TextBox();
IControl ic = c;
IControl it = t;
c.Paint();            // invokes Control.Paint();
t.Paint();            // invokes TextBox.Paint();
ic.Paint();           // invokes Control.Paint();
it.Paint();           // invokes Control.Paint();
```

<span data-ttu-id="b1004-365">不過，當介面方法對應到類別中的虛擬方法時，就可以為衍生的類別，以覆寫虛擬方法，然後修改介面的實作。</span><span class="sxs-lookup"><span data-stu-id="b1004-365">However, when an interface method is mapped onto a virtual method in a class, it is possible for derived classes to override the virtual method and alter the implementation of the interface.</span></span> <span data-ttu-id="b1004-366">例如，重寫上述宣告</span><span class="sxs-lookup"><span data-stu-id="b1004-366">For example, rewriting the declarations above to</span></span>
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    public virtual void Paint() {...}
}

class TextBox: Control
{
    public override void Paint() {...}
}
```
<span data-ttu-id="b1004-367">現在可觀察到下列的效果</span><span class="sxs-lookup"><span data-stu-id="b1004-367">the following effects will now be observed</span></span>
```csharp
Control c = new Control();
TextBox t = new TextBox();
IControl ic = c;
IControl it = t;
c.Paint();            // invokes Control.Paint();
t.Paint();            // invokes TextBox.Paint();
ic.Paint();           // invokes Control.Paint();
it.Paint();           // invokes TextBox.Paint();
```

<span data-ttu-id="b1004-368">明確介面成員實作不能宣告為虛擬，因為它不可以覆寫明確介面成員實作。</span><span class="sxs-lookup"><span data-stu-id="b1004-368">Since explicit interface member implementations cannot be declared virtual, it is not possible to override an explicit interface member implementation.</span></span> <span data-ttu-id="b1004-369">不過，它是完全適用於呼叫另一個方法，明確介面成員實作，而且，其他方法可以宣告為虛擬化來允許衍生類別覆寫它。</span><span class="sxs-lookup"><span data-stu-id="b1004-369">However, it is perfectly valid for an explicit interface member implementation to call another method, and that other method can be declared virtual to allow derived classes to override it.</span></span> <span data-ttu-id="b1004-370">例如</span><span class="sxs-lookup"><span data-stu-id="b1004-370">For example</span></span>
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    void IControl.Paint() { PaintControl(); }
    protected virtual void PaintControl() {...}
}

class TextBox: Control
{
    protected override void PaintControl() {...}
}
```

<span data-ttu-id="b1004-371">在這裡，衍生自`Control`可以是專為實作`IControl.Paint`藉由覆寫`PaintControl`方法。</span><span class="sxs-lookup"><span data-stu-id="b1004-371">Here, classes derived from `Control` can specialize the implementation of `IControl.Paint` by overriding the `PaintControl` method.</span></span>

### <a name="interface-re-implementation"></a><span data-ttu-id="b1004-372">介面重新實作</span><span class="sxs-lookup"><span data-stu-id="b1004-372">Interface re-implementation</span></span>

<span data-ttu-id="b1004-373">繼承的介面實作的類別允許***重新實作***加在基底類別清單中的介面。</span><span class="sxs-lookup"><span data-stu-id="b1004-373">A class that inherits an interface implementation is permitted to ***re-implement*** the interface by including it in the base class list.</span></span>

<span data-ttu-id="b1004-374">介面重新實作能遵循完全相同介面對應的規則初始介面的實作。</span><span class="sxs-lookup"><span data-stu-id="b1004-374">A re-implementation of an interface follows exactly the same interface mapping rules as an initial implementation of an interface.</span></span> <span data-ttu-id="b1004-375">因此，繼承的介面對應沒有雅致的介面對應建立重新實作介面的任何作用。</span><span class="sxs-lookup"><span data-stu-id="b1004-375">Thus, the inherited interface mapping has no effect whatsoever on the interface mapping established for the re-implementation of the interface.</span></span> <span data-ttu-id="b1004-376">例如，在宣告中</span><span class="sxs-lookup"><span data-stu-id="b1004-376">For example, in the declarations</span></span>
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    void IControl.Paint() {...}
}

class MyControl: Control, IControl
{
    public void Paint() {}
}
```
<span data-ttu-id="b1004-377">事實上，`Control`對應`IControl.Paint`拖曳至`Control.IControl.Paint`不會影響在重新實作`MyControl`，哪一個 map`IControl.Paint`拖曳至`MyControl.Paint`。</span><span class="sxs-lookup"><span data-stu-id="b1004-377">the fact that `Control` maps `IControl.Paint` onto `Control.IControl.Paint` doesn't affect the re-implementation in `MyControl`, which maps `IControl.Paint` onto `MyControl.Paint`.</span></span>

<span data-ttu-id="b1004-378">繼承的公用成員宣告和繼承的明確介面成員宣告加入重新實作介面的介面對應程序。</span><span class="sxs-lookup"><span data-stu-id="b1004-378">Inherited public member declarations and inherited explicit interface member declarations participate in the interface mapping process for re-implemented interfaces.</span></span> <span data-ttu-id="b1004-379">例如</span><span class="sxs-lookup"><span data-stu-id="b1004-379">For example</span></span>
```csharp
interface IMethods
{
    void F();
    void G();
    void H();
    void I();
}

class Base: IMethods
{
    void IMethods.F() {}
    void IMethods.G() {}
    public void H() {}
    public void I() {}
}

class Derived: Base, IMethods
{
    public void F() {}
    void IMethods.H() {}
}
```

<span data-ttu-id="b1004-380">這裡，實作`IMethods`中`Derived`對應至介面方法`Derived.F`， `Base.IMethods.G`， `Derived.IMethods.H`，和`Base.I`。</span><span class="sxs-lookup"><span data-stu-id="b1004-380">Here, the implementation of `IMethods` in `Derived` maps the interface methods onto `Derived.F`, `Base.IMethods.G`, `Derived.IMethods.H`, and `Base.I`.</span></span>

<span data-ttu-id="b1004-381">當類別實作介面，它會隱含也會實作所有介面的基底介面。</span><span class="sxs-lookup"><span data-stu-id="b1004-381">When a class implements an interface, it implicitly also implements all of that interface's base interfaces.</span></span> <span data-ttu-id="b1004-382">同樣地，重新實作也是介面的隱含的重新實作所有介面的基底介面。</span><span class="sxs-lookup"><span data-stu-id="b1004-382">Likewise, a re-implementation of an interface is also implicitly a re-implementation of all of the interface's base interfaces.</span></span> <span data-ttu-id="b1004-383">例如</span><span class="sxs-lookup"><span data-stu-id="b1004-383">For example</span></span>
```csharp
interface IBase
{
    void F();
}

interface IDerived: IBase
{
    void G();
}

class C: IDerived
{
    void IBase.F() {...}
    void IDerived.G() {...}
}

class D: C, IDerived
{
    public void F() {...}
    public void G() {...}
}
```

<span data-ttu-id="b1004-384">這裡，重新實作`IDerived`也重新實作`IBase`對應`IBase.F`拖曳至`D.F`。</span><span class="sxs-lookup"><span data-stu-id="b1004-384">Here, the re-implementation of `IDerived` also re-implements `IBase`, mapping `IBase.F` onto `D.F`.</span></span>

### <a name="abstract-classes-and-interfaces"></a><span data-ttu-id="b1004-385">抽象類別和介面</span><span class="sxs-lookup"><span data-stu-id="b1004-385">Abstract classes and interfaces</span></span>

<span data-ttu-id="b1004-386">如同非抽象類別，抽象類別必須提供的介面的類別的基底類別清單中列出的所有成員的實作。</span><span class="sxs-lookup"><span data-stu-id="b1004-386">Like a non-abstract class, an abstract class must provide implementations of all members of the interfaces that are listed in the base class list of the class.</span></span> <span data-ttu-id="b1004-387">不過，抽象類別可以對應至抽象方法的介面方法。</span><span class="sxs-lookup"><span data-stu-id="b1004-387">However, an abstract class is permitted to map interface methods onto abstract methods.</span></span> <span data-ttu-id="b1004-388">例如</span><span class="sxs-lookup"><span data-stu-id="b1004-388">For example</span></span>
```csharp
interface IMethods
{
    void F();
    void G();
}

abstract class C: IMethods
{
    public abstract void F();
    public abstract void G();
}
```

<span data-ttu-id="b1004-389">這裡，實作`IMethods`對應`F`並`G`至抽象方法，這必須覆寫在非抽象類別衍生自`C`。</span><span class="sxs-lookup"><span data-stu-id="b1004-389">Here, the implementation of `IMethods` maps `F` and `G` onto abstract methods, which must be overridden in non-abstract classes that derive from `C`.</span></span>

<span data-ttu-id="b1004-390">請注意，明確介面成員實作都不能是抽象的但明確介面成員實作當然可以呼叫抽象方法。</span><span class="sxs-lookup"><span data-stu-id="b1004-390">Note that explicit interface member implementations cannot be abstract, but explicit interface member implementations are of course permitted to call abstract methods.</span></span> <span data-ttu-id="b1004-391">例如</span><span class="sxs-lookup"><span data-stu-id="b1004-391">For example</span></span>
```csharp
interface IMethods
{
    void F();
    void G();
}

abstract class C: IMethods
{
    void IMethods.F() { FF(); }
    void IMethods.G() { GG(); }
    protected abstract void FF();
    protected abstract void GG();
}
```

<span data-ttu-id="b1004-392">這裡，衍生自非抽象類別`C`才能覆寫`FF`並`GG`，因此能提供的實際實作`IMethods`。</span><span class="sxs-lookup"><span data-stu-id="b1004-392">Here, non-abstract classes that derive from `C` would be required to override `FF` and `GG`, thus providing the actual implementation of `IMethods`.</span></span>
