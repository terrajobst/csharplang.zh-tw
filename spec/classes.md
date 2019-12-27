---
ms.openlocfilehash: e0def754174ab8646f9b849abe86d2c375c958b6
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703975"
---
# <a name="classes"></a><span data-ttu-id="2d25f-101">類別</span><span class="sxs-lookup"><span data-stu-id="2d25f-101">Classes</span></span>

<span data-ttu-id="2d25f-102">「類別」（class）是一種資料結構，其中可能包含資料成員（常數和欄位）、函式成員（方法、屬性、事件、索引子、運算子、實例的函式、析構函式和靜態的函式）和巢狀型別。</span><span class="sxs-lookup"><span data-stu-id="2d25f-102">A class is a data structure that may contain data members (constants and fields), function members (methods, properties, events, indexers, operators, instance constructors, destructors and static constructors), and nested types.</span></span> <span data-ttu-id="2d25f-103">類別類型支援繼承，這是衍生類別可以擴充和特殊化基類的機制。</span><span class="sxs-lookup"><span data-stu-id="2d25f-103">Class types support inheritance, a mechanism whereby a derived class can extend and specialize a base class.</span></span>

## <a name="class-declarations"></a><span data-ttu-id="2d25f-104">類別宣告</span><span class="sxs-lookup"><span data-stu-id="2d25f-104">Class declarations</span></span>

<span data-ttu-id="2d25f-105">*Class_declaration*是宣告新類別的*type_declaration* （[型](namespaces.md#type-declarations)別宣告）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-105">A *class_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new class.</span></span>

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

<span data-ttu-id="2d25f-106">*Class_declaration*由一組選擇性的*屬性*（[屬性](attributes.md)）所組成，後面接著一組選擇性的*class_modifier*s （[類別](classes.md#class-modifiers)修飾詞），後面接著一個選擇性的 `partial` 修飾詞，後面接著關鍵字 `class` 和名為類別的*識別碼*，接著是選擇性*的 type_parameter_list 規格*[（](classes.md#type-parameters)[類別基底規格](classes.md#class-base-specification)），接著以一組選擇性的*type_parameter_constraints_clause*s （[類型參數條件約束](classes.md#type-parameter-constraints)），後面接著*class_body* （[類別主體](classes.md#class-body)），並選擇性地後面加上分號。</span><span class="sxs-lookup"><span data-stu-id="2d25f-106">A *class_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *class_modifier*s ([Class modifiers](classes.md#class-modifiers)), followed by an optional `partial` modifier, followed by the keyword `class` and an *identifier* that names the class, followed by an optional *type_parameter_list* ([Type parameters](classes.md#type-parameters)), followed by an optional *class_base* specification ([Class base specification](classes.md#class-base-specification)) , followed by an optional set of *type_parameter_constraints_clause*s ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by a *class_body* ([Class body](classes.md#class-body)), optionally followed by a semicolon.</span></span>

<span data-ttu-id="2d25f-107">類別宣告無法提供*type_parameter_constraints_clause*s，除非它也提供*type_parameter_list*。</span><span class="sxs-lookup"><span data-stu-id="2d25f-107">A class declaration cannot supply *type_parameter_constraints_clause*s unless it also supplies a *type_parameter_list*.</span></span>

<span data-ttu-id="2d25f-108">提供*type_parameter_list*的類別宣告是***泛型類別***宣告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-108">A class declaration that supplies a *type_parameter_list* is a ***generic class declaration***.</span></span> <span data-ttu-id="2d25f-109">此外，任何嵌套在泛型類別宣告或泛型結構宣告內的類別本身都是泛型類別宣告，因為必須提供包含類型的類型參數來建立結構化類型。</span><span class="sxs-lookup"><span data-stu-id="2d25f-109">Additionally, any class nested inside a generic class declaration or a generic struct declaration is itself a generic class declaration, since type parameters for the containing type must be supplied to create a constructed type.</span></span>

### <a name="class-modifiers"></a><span data-ttu-id="2d25f-110">類別修飾詞</span><span class="sxs-lookup"><span data-stu-id="2d25f-110">Class modifiers</span></span>

<span data-ttu-id="2d25f-111">*Class_declaration*可以選擇性地包含一連串的類別修飾詞：</span><span class="sxs-lookup"><span data-stu-id="2d25f-111">A *class_declaration* may optionally include a sequence of class modifiers:</span></span>

```antlr
class_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'abstract'
    | 'sealed'
    | 'static'
    | class_modifier_unsafe
    ;
```

<span data-ttu-id="2d25f-112">在類別宣告中多次出現相同的修飾詞時，會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-112">It is a compile-time error for the same modifier to appear multiple times in a class declaration.</span></span>

<span data-ttu-id="2d25f-113">在嵌套類別上允許使用 `new` 修飾詞。</span><span class="sxs-lookup"><span data-stu-id="2d25f-113">The `new` modifier is permitted on nested classes.</span></span> <span data-ttu-id="2d25f-114">它會指定類別以相同名稱隱藏繼承的成員，如[新修飾](classes.md#the-new-modifier)詞中所述。</span><span class="sxs-lookup"><span data-stu-id="2d25f-114">It specifies that the class hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span> <span data-ttu-id="2d25f-115">在不是嵌套類別宣告的類別宣告上出現 `new` 修飾詞時，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-115">It is a compile-time error for the `new` modifier to appear on a class declaration that is not a nested class declaration.</span></span>

<span data-ttu-id="2d25f-116">`public`、`protected`、`internal`和 `private` 修飾詞會控制類別的存取範圍。</span><span class="sxs-lookup"><span data-stu-id="2d25f-116">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the class.</span></span> <span data-ttu-id="2d25f-117">根據類別宣告發生的內容，可能不允許其中一些修飾詞（宣告的[存取](basic-concepts.md#declared-accessibility)範圍）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-117">Depending on the context in which the class declaration occurs, some of these modifiers may not be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="2d25f-118">`abstract`、`sealed` 和 `static` 修飾詞會在下列各節中討論。</span><span class="sxs-lookup"><span data-stu-id="2d25f-118">The `abstract`, `sealed` and `static` modifiers are discussed in the following sections.</span></span>

#### <a name="abstract-classes"></a><span data-ttu-id="2d25f-119">抽象類別</span><span class="sxs-lookup"><span data-stu-id="2d25f-119">Abstract classes</span></span>

<span data-ttu-id="2d25f-120">`abstract` 修飾詞是用來指出類別不完整，而且僅供做為基類使用。</span><span class="sxs-lookup"><span data-stu-id="2d25f-120">The `abstract` modifier is used to indicate that a class is incomplete and that it is intended to be used only as a base class.</span></span> <span data-ttu-id="2d25f-121">抽象類別與非抽象類別有下列不同之處：</span><span class="sxs-lookup"><span data-stu-id="2d25f-121">An abstract class differs from a non-abstract class in the following ways:</span></span>

*  <span data-ttu-id="2d25f-122">抽象類別無法直接具現化，而且在抽象類別上使用 `new` 運算子時，會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-122">An abstract class cannot be instantiated directly, and it is a compile-time error to use the `new` operator on an abstract class.</span></span> <span data-ttu-id="2d25f-123">雖然可能有編譯時間類型為抽象的變數和值，但這類變數和值一定會 `null`，或包含衍生自抽象類別型之非抽象類別實例的參考。</span><span class="sxs-lookup"><span data-stu-id="2d25f-123">While it is possible to have variables and values whose compile-time types are abstract, such variables and values will necessarily either be `null` or contain references to instances of non-abstract classes derived from the abstract types.</span></span>
*  <span data-ttu-id="2d25f-124">允許抽象類別（但非必要）包含抽象成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-124">An abstract class is permitted (but not required) to contain abstract members.</span></span>
*  <span data-ttu-id="2d25f-125">抽象類別不能是密封的。</span><span class="sxs-lookup"><span data-stu-id="2d25f-125">An abstract class cannot be sealed.</span></span>

<span data-ttu-id="2d25f-126">當非抽象類別衍生自抽象類別時，非抽象類別必須包括所有繼承之抽象成員的實際執行，因而覆寫這些抽象成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-126">When a non-abstract class is derived from an abstract class, the non-abstract class must include actual implementations of all inherited abstract members, thereby overriding those abstract members.</span></span> <span data-ttu-id="2d25f-127">在範例中</span><span class="sxs-lookup"><span data-stu-id="2d25f-127">In the example</span></span>
```csharp
abstract class A
{
    public abstract void F();
}

abstract class B: A
{
    public void G() {}
}

class C: B
{
    public override void F() {
        // actual implementation of F
    }
}
```
<span data-ttu-id="2d25f-128">抽象類別 `A` 引進抽象方法 `F`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-128">the abstract class `A` introduces an abstract method `F`.</span></span> <span data-ttu-id="2d25f-129">類別 `B` 引進了額外的方法 `G`，但因為它並未提供 `F`的執行，所以 `B` 也必須宣告為抽象。</span><span class="sxs-lookup"><span data-stu-id="2d25f-129">Class `B` introduces an additional method `G`, but since it doesn't provide an implementation of `F`, `B` must also be declared abstract.</span></span> <span data-ttu-id="2d25f-130">類別 `C` 會覆寫 `F` 並提供實際的實作為。</span><span class="sxs-lookup"><span data-stu-id="2d25f-130">Class `C` overrides `F` and provides an actual implementation.</span></span> <span data-ttu-id="2d25f-131">因為 `C`中沒有抽象成員，所以允許（但非必要） `C` 為非抽象。</span><span class="sxs-lookup"><span data-stu-id="2d25f-131">Since there are no abstract members in `C`, `C` is permitted (but not required) to be non-abstract.</span></span>

#### <a name="sealed-classes"></a><span data-ttu-id="2d25f-132">密封類別</span><span class="sxs-lookup"><span data-stu-id="2d25f-132">Sealed classes</span></span>

<span data-ttu-id="2d25f-133">`sealed` 修飾詞是用來防止衍生自類別。</span><span class="sxs-lookup"><span data-stu-id="2d25f-133">The `sealed` modifier is used to prevent derivation from a class.</span></span> <span data-ttu-id="2d25f-134">如果將密封類別指定為另一個類別的基類，則會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-134">A compile-time error occurs if a sealed class is specified as the base class of another class.</span></span>

<span data-ttu-id="2d25f-135">密封的類別也不能是抽象類別。</span><span class="sxs-lookup"><span data-stu-id="2d25f-135">A sealed class cannot also be an abstract class.</span></span>

<span data-ttu-id="2d25f-136">`sealed` 修飾詞主要是用來防止非預期的衍生，但它也會啟用特定執行時間優化。</span><span class="sxs-lookup"><span data-stu-id="2d25f-136">The `sealed` modifier is primarily used to prevent unintended derivation, but it also enables certain run-time optimizations.</span></span> <span data-ttu-id="2d25f-137">特別是，因為已知密封的類別永遠不會有任何衍生類別，所以可以將密封類別實例上的虛擬函式成員調用轉換成非虛擬調用。</span><span class="sxs-lookup"><span data-stu-id="2d25f-137">In particular, because a sealed class is known to never have any derived classes, it is possible to transform virtual function member invocations on sealed class instances into non-virtual invocations.</span></span>

#### <a name="static-classes"></a><span data-ttu-id="2d25f-138">靜態類別</span><span class="sxs-lookup"><span data-stu-id="2d25f-138">Static classes</span></span>

<span data-ttu-id="2d25f-139">`static` 修飾詞是用來標示要宣告為***靜態類別***的類別。</span><span class="sxs-lookup"><span data-stu-id="2d25f-139">The `static` modifier is used to mark the class being declared as a ***static class***.</span></span> <span data-ttu-id="2d25f-140">靜態類別無法具現化，不能當做類型使用，而且只能包含靜態成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-140">A static class cannot be instantiated, cannot be used as a type and can contain only static members.</span></span> <span data-ttu-id="2d25f-141">只有靜態類別可以包含擴充方法（[擴充方法](classes.md#extension-methods)）的宣告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-141">Only a static class can contain declarations of extension methods ([Extension methods](classes.md#extension-methods)).</span></span>

<span data-ttu-id="2d25f-142">靜態類別宣告受到下列限制：</span><span class="sxs-lookup"><span data-stu-id="2d25f-142">A static class declaration is subject to the following restrictions:</span></span>

*  <span data-ttu-id="2d25f-143">靜態類別不能包含 `sealed` 或 `abstract` 修飾詞。</span><span class="sxs-lookup"><span data-stu-id="2d25f-143">A static class may not include a `sealed` or `abstract` modifier.</span></span> <span data-ttu-id="2d25f-144">不過，請注意，因為靜態類別無法具現化或衍生自，所以其行為就像是密封和抽象。</span><span class="sxs-lookup"><span data-stu-id="2d25f-144">Note, however, that since a static class cannot be instantiated or derived from, it behaves as if it was both sealed and abstract.</span></span>
*  <span data-ttu-id="2d25f-145">靜態類別不能包含*class_base*規格（[類別基底規格](classes.md#class-base-specification)），而且無法明確指定基類或實作為實介面的清單。</span><span class="sxs-lookup"><span data-stu-id="2d25f-145">A static class may not include a *class_base* specification ([Class base specification](classes.md#class-base-specification)) and cannot explicitly specify a base class or a list of implemented interfaces.</span></span> <span data-ttu-id="2d25f-146">靜態類別隱含繼承自類型 `object`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-146">A static class implicitly inherits from type `object`.</span></span>
*  <span data-ttu-id="2d25f-147">靜態類別只能包含靜態成員（[靜態和實例成員](classes.md#static-and-instance-members)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-147">A static class can only contain static members ([Static and instance members](classes.md#static-and-instance-members)).</span></span> <span data-ttu-id="2d25f-148">請注意，常數和巢狀型別會分類為靜態成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-148">Note that constants and nested types are classified as static members.</span></span>
*  <span data-ttu-id="2d25f-149">靜態類別不能有具有 `protected` 或 `protected internal` 宣告存取範圍的成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-149">A static class cannot have members with `protected` or `protected internal` declared accessibility.</span></span>

<span data-ttu-id="2d25f-150">這是編譯時期錯誤，違反上述任何一項限制。</span><span class="sxs-lookup"><span data-stu-id="2d25f-150">It is a compile-time error to violate any of these restrictions.</span></span>

<span data-ttu-id="2d25f-151">靜態類別沒有實例的函式。</span><span class="sxs-lookup"><span data-stu-id="2d25f-151">A static class has no instance constructors.</span></span> <span data-ttu-id="2d25f-152">您不能在靜態類別中宣告實例的函式，也不會提供靜態類別的預設實例（[default](classes.md#default-constructors)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-152">It is not possible to declare an instance constructor in a static class, and no default instance constructor ([Default constructors](classes.md#default-constructors)) is provided for a static class.</span></span>

<span data-ttu-id="2d25f-153">靜態類別的成員不會自動成為靜態，而且成員宣告必須明確包含 `static` 修飾詞（常數和巢狀型別除外）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-153">The members of a static class are not automatically static, and the member declarations must explicitly include a `static` modifier (except for constants and nested types).</span></span> <span data-ttu-id="2d25f-154">當類別是在靜態外部類別中嵌套時，除非明確包含 `static` 修飾詞，否則，此嵌套類別不是靜態類別。</span><span class="sxs-lookup"><span data-stu-id="2d25f-154">When a class is nested within a static outer class, the nested class is not a static class unless it explicitly includes a `static` modifier.</span></span>

<span data-ttu-id="2d25f-155">__參考靜態類別類型__</span><span class="sxs-lookup"><span data-stu-id="2d25f-155">__Referencing static class types__</span></span>

<span data-ttu-id="2d25f-156">若為，則允許*namespace_or_type_name* （[命名空間和類型名稱](basic-concepts.md#namespace-and-type-names)）參考靜態類別</span><span class="sxs-lookup"><span data-stu-id="2d25f-156">A *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="2d25f-157">*Namespace_or_type_name*是 `T.I`格式的*namespace_or_type_name*中的 `T`，或是</span><span class="sxs-lookup"><span data-stu-id="2d25f-157">The *namespace_or_type_name* is the `T` in a *namespace_or_type_name* of the form `T.I`, or</span></span>
*  <span data-ttu-id="2d25f-158">*Namespace_or_type_name*是表單`T` *typeof_expression* 的（[引數清單](expressions.md#argument-lists)1）中`typeof(T)`的。</span><span class="sxs-lookup"><span data-stu-id="2d25f-158">The *namespace_or_type_name* is the `T` in a *typeof_expression* ([Argument lists](expressions.md#argument-lists)1) of the form `typeof(T)`.</span></span>

<span data-ttu-id="2d25f-159">若為，則允許*primary_expression* （函式[成員](expressions.md#function-members)）參考靜態類別</span><span class="sxs-lookup"><span data-stu-id="2d25f-159">A *primary_expression* ([Function members](expressions.md#function-members)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="2d25f-160">*Primary_expression* `E`是形式 的 *member_access* （[針對動態多載解析的編譯時間檢查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)）`E.I`中的。</span><span class="sxs-lookup"><span data-stu-id="2d25f-160">The *primary_expression* is the `E` in a *member_access* ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) of the form `E.I`.</span></span>

<span data-ttu-id="2d25f-161">在任何其他內容中，都是參考靜態類別的編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-161">In any other context it is a compile-time error to reference a static class.</span></span> <span data-ttu-id="2d25f-162">例如，使用靜態類別做為基類、成員的構成類型（[嵌套](classes.md#nested-types)類型）、泛型型別引數或類型參數條件約束，就會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-162">For example, it is an error for a static class to be used as a base class, a constituent type ([Nested types](classes.md#nested-types)) of a member, a generic type argument, or a type parameter constraint.</span></span> <span data-ttu-id="2d25f-163">同樣地，靜態類別不能用於陣列類型、指標類型、`new` 運算式、cast 運算式、`is` 運算式、`as` 運算式、`sizeof` 運算式或預設值運算式。</span><span class="sxs-lookup"><span data-stu-id="2d25f-163">Likewise, a static class cannot be used in an array type, a pointer type, a `new` expression, a cast expression, an `is` expression, an `as` expression, a `sizeof` expression, or a default value expression.</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="2d25f-164">Partial 修飾詞</span><span class="sxs-lookup"><span data-stu-id="2d25f-164">Partial modifier</span></span>

<span data-ttu-id="2d25f-165">`partial` 修飾詞是用來指出此*class_declaration*是部分類型宣告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-165">The `partial` modifier is used to indicate that this *class_declaration* is a partial type declaration.</span></span> <span data-ttu-id="2d25f-166">在封入命名空間或類型宣告中，多個具有相同名稱的部分類型宣告會結合成一個類型宣告，遵循[部分類型](classes.md#partial-types)中指定的規則。</span><span class="sxs-lookup"><span data-stu-id="2d25f-166">Multiple partial type declarations with the same name within an enclosing namespace or type declaration combine to form one type declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

<span data-ttu-id="2d25f-167">如果在不同的內容中產生或維護這些區段，則在程式文字的個別區段上散發的類別宣告會很有用。</span><span class="sxs-lookup"><span data-stu-id="2d25f-167">Having the declaration of a class distributed over separate segments of program text can be useful if these segments are produced or maintained in different contexts.</span></span> <span data-ttu-id="2d25f-168">例如，類別宣告的其中一個部分可能是電腦產生的，而另一個則是手動撰寫。</span><span class="sxs-lookup"><span data-stu-id="2d25f-168">For instance, one part of a class declaration may be machine generated, whereas the other is manually authored.</span></span> <span data-ttu-id="2d25f-169">這兩者的文字分隔會防止更新與另一個更新衝突。</span><span class="sxs-lookup"><span data-stu-id="2d25f-169">Textual separation of the two prevents updates by one from conflicting with updates by the other.</span></span>

### <a name="type-parameters"></a><span data-ttu-id="2d25f-170">型別參數</span><span class="sxs-lookup"><span data-stu-id="2d25f-170">Type parameters</span></span>

<span data-ttu-id="2d25f-171">型別參數是簡單的識別碼，代表提供用來建立結構化型別之型別引數的預留位置。</span><span class="sxs-lookup"><span data-stu-id="2d25f-171">A type parameter is a simple identifier that denotes a placeholder for a type argument supplied to create a constructed type.</span></span> <span data-ttu-id="2d25f-172">類型參數是稍後將提供之類型的正式預留位置。</span><span class="sxs-lookup"><span data-stu-id="2d25f-172">A type parameter is a formal placeholder for a type that will be supplied later.</span></span> <span data-ttu-id="2d25f-173">相較之下，型別引數（[型別引數](types.md#type-arguments)）是建立結構化型別時，用來取代型別參數的實際型別。</span><span class="sxs-lookup"><span data-stu-id="2d25f-173">By contrast, a type argument ([Type arguments](types.md#type-arguments)) is the actual type that is substituted for the type parameter when a constructed type is created.</span></span>

```antlr
type_parameter_list
    : '<' type_parameters '>'
    ;

type_parameters
    : attributes? type_parameter
    | type_parameters ',' attributes? type_parameter
    ;

type_parameter
    : identifier
    ;
```

<span data-ttu-id="2d25f-174">類別宣告中的每個型別參數都會定義該類別的宣告[空間（宣告](basic-concepts.md#declarations)）中的名稱。</span><span class="sxs-lookup"><span data-stu-id="2d25f-174">Each type parameter in a class declaration defines a name in the declaration space ([Declarations](basic-concepts.md#declarations)) of that class.</span></span> <span data-ttu-id="2d25f-175">因此，它不能與另一個類型參數或該類別中宣告的成員具有相同的名稱。</span><span class="sxs-lookup"><span data-stu-id="2d25f-175">Thus, it cannot have the same name as another type parameter or a member declared in that class.</span></span> <span data-ttu-id="2d25f-176">類型參數不能與類型本身具有相同的名稱。</span><span class="sxs-lookup"><span data-stu-id="2d25f-176">A type parameter cannot have the same name as the type itself.</span></span>

### <a name="class-base-specification"></a><span data-ttu-id="2d25f-177">類別基底規格</span><span class="sxs-lookup"><span data-stu-id="2d25f-177">Class base specification</span></span>

<span data-ttu-id="2d25f-178">類別宣告可以包含*class_base*規格，這會定義類別的直接基類，以及類別所直接實作為介面（[介面](interfaces.md)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-178">A class declaration may include a *class_base* specification, which defines the direct base class of the class and the interfaces ([Interfaces](interfaces.md)) directly implemented by the class.</span></span>

```antlr
class_base
    : ':' class_type
    | ':' interface_type_list
    | ':' class_type ',' interface_type_list
    ;

interface_type_list
    : interface_type (',' interface_type)*
    ;
```

<span data-ttu-id="2d25f-179">在類別宣告中指定的基類可以是結構化類別類型（[結構化類型](types.md#constructed-types)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-179">The base class specified in a class declaration can be a constructed class type ([Constructed types](types.md#constructed-types)).</span></span> <span data-ttu-id="2d25f-180">基類本身不能是型別參數，不過它可以包含範圍內的型別參數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-180">A base class cannot be a type parameter on its own, though it can involve the type parameters that are in scope.</span></span>

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a><span data-ttu-id="2d25f-181">基底類別</span><span class="sxs-lookup"><span data-stu-id="2d25f-181">Base classes</span></span>

<span data-ttu-id="2d25f-182">當*class_type*包含在*class_base*中時，它會指定所宣告之類別的直接基類。</span><span class="sxs-lookup"><span data-stu-id="2d25f-182">When a *class_type* is included in the *class_base*, it specifies the direct base class of the class being declared.</span></span> <span data-ttu-id="2d25f-183">如果類別宣告沒有*class_base*，或*class_base*只列出介面類別型，則會假設直接基底類別為 `object`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-183">If a class declaration has no *class_base*, or if the *class_base* lists only interface types, the direct base class is assumed to be `object`.</span></span> <span data-ttu-id="2d25f-184">類別會繼承其直接基類的成員，如[繼承](classes.md#inheritance)中所述。</span><span class="sxs-lookup"><span data-stu-id="2d25f-184">A class inherits members from its direct base class, as described in [Inheritance](classes.md#inheritance).</span></span>

<span data-ttu-id="2d25f-185">在範例中</span><span class="sxs-lookup"><span data-stu-id="2d25f-185">In the example</span></span>
```csharp
class A {}

class B: A {}
```
<span data-ttu-id="2d25f-186">類別 `A` 稱為 `B`的直接基類，而 `B` 則稱為衍生自 `A`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-186">class `A` is said to be the direct base class of `B`, and `B` is said to be derived from `A`.</span></span> <span data-ttu-id="2d25f-187">由於 `A` 並未明確指定直接基類，因此會隱含地 `object`其直接基類。</span><span class="sxs-lookup"><span data-stu-id="2d25f-187">Since `A` does not explicitly specify a direct base class, its direct base class is implicitly `object`.</span></span>

<span data-ttu-id="2d25f-188">針對已建立的類別類型，如果在泛型類別宣告中指定基類，則會針對基類宣告中的每個*type_parameter* ，以替代結構類型的對應*type_argument*來取得結構化類型的基底類別。</span><span class="sxs-lookup"><span data-stu-id="2d25f-188">For a constructed class type, if a base class is specified in the generic class declaration, the base class of the constructed type is obtained by substituting, for each *type_parameter* in the base class declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="2d25f-189">給定泛型類別宣告</span><span class="sxs-lookup"><span data-stu-id="2d25f-189">Given the generic class declarations</span></span>
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
<span data-ttu-id="2d25f-190">`B<string,int[]>`的結構化型別 `G<int>` 的基類。</span><span class="sxs-lookup"><span data-stu-id="2d25f-190">the base class of the constructed type `G<int>` would be `B<string,int[]>`.</span></span>

<span data-ttu-id="2d25f-191">類別類型的直接基類至少必須與類別類型本身（[存取範圍網域](basic-concepts.md#accessibility-domains)）一樣可以存取。</span><span class="sxs-lookup"><span data-stu-id="2d25f-191">The direct base class of a class type must be at least as accessible as the class type itself ([Accessibility domains](basic-concepts.md#accessibility-domains)).</span></span> <span data-ttu-id="2d25f-192">例如，`public` 類別衍生自 `private` 或 `internal` 類別時，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-192">For example, it is a compile-time error for a `public` class to derive from a `private` or `internal` class.</span></span>

<span data-ttu-id="2d25f-193">類別類型的直接基類不得為下列任何類型： `System.Array`、`System.Delegate`、`System.MulticastDelegate`、`System.Enum`或 `System.ValueType`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-193">The direct base class of a class type must not be any of the following types: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, or `System.ValueType`.</span></span> <span data-ttu-id="2d25f-194">此外，泛型類別宣告不能使用 `System.Attribute` 做為直接或間接基類。</span><span class="sxs-lookup"><span data-stu-id="2d25f-194">Furthermore, a generic class declaration cannot use `System.Attribute` as a direct or indirect base class.</span></span>

<span data-ttu-id="2d25f-195">在決定 `B`的直接基類規格 `A` 的意義時，`B` 的直接基底類別會暫時假設為 `object`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-195">While determining the meaning of the direct base class specification `A` of a class `B`, the direct base class of `B` is temporarily assumed to be `object`.</span></span> <span data-ttu-id="2d25f-196">這可確保基類規格的意義無法以遞迴方式相依于本身。</span><span class="sxs-lookup"><span data-stu-id="2d25f-196">Intuitively this ensures that the meaning of a base class specification cannot recursively depend on itself.</span></span> <span data-ttu-id="2d25f-197">範例：</span><span class="sxs-lookup"><span data-stu-id="2d25f-197">The example:</span></span>
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
<span data-ttu-id="2d25f-198">發生錯誤，因為在基類規格中 `A<C.B>` 會將 `C` 的直接基類視為 `object`，因此（藉由[命名空間和型別名稱](basic-concepts.md#namespace-and-type-names)的規則） `C` 不會被視為具有成員 `B`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-198">is in error since in the base class specification `A<C.B>` the direct base class of `C` is considered to be `object`, and hence (by the rules of [Namespace and type names](basic-concepts.md#namespace-and-type-names))  `C` is not considered to have a member `B`.</span></span>

<span data-ttu-id="2d25f-199">類別類型的基類是直接基類和其基類。</span><span class="sxs-lookup"><span data-stu-id="2d25f-199">The base classes of a class type are the direct base class and its base classes.</span></span> <span data-ttu-id="2d25f-200">換句話說，一組基類是直接基底類別關聯性的可轉移關閉。</span><span class="sxs-lookup"><span data-stu-id="2d25f-200">In other words, the set of base classes is the transitive closure of the direct base class relationship.</span></span> <span data-ttu-id="2d25f-201">參考上述範例，`B` 的基類 `A` 和 `object`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-201">Referring to the example above, the base classes of `B` are `A` and `object`.</span></span> <span data-ttu-id="2d25f-202">在範例中</span><span class="sxs-lookup"><span data-stu-id="2d25f-202">In the example</span></span>
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
<span data-ttu-id="2d25f-203">`D<int>` 的基類 `C<int[]>`、`B<IComparable<int[]>>`、`A`和 `object`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-203">the base classes of `D<int>` are `C<int[]>`, `B<IComparable<int[]>>`, `A`, and `object`.</span></span>

<span data-ttu-id="2d25f-204">除了類別 `object`以外，每個類別類型都只有一個直接基類。</span><span class="sxs-lookup"><span data-stu-id="2d25f-204">Except for class `object`, every class type has exactly one direct base class.</span></span> <span data-ttu-id="2d25f-205">`object` 類別沒有直接基類，而且是所有其他類別的最終基類。</span><span class="sxs-lookup"><span data-stu-id="2d25f-205">The `object` class has no direct base class and is the ultimate base class of all other classes.</span></span>

<span data-ttu-id="2d25f-206">當類別 `B` 衍生自類別 `A`時，`A` 相依于 `B`時，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-206">When a class `B` derives from a class `A`, it is a compile-time error for `A` to depend on `B`.</span></span> <span data-ttu-id="2d25f-207">類別會***直接相依于***其直接基類（如果有的話），而***直接取決於***立即嵌套的類別（如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-207">A class ***directly depends on*** its direct base class (if any) and ***directly depends on*** the class within which it is immediately nested (if any).</span></span> <span data-ttu-id="2d25f-208">根據這個定義，類別所相依的一組完整類別是***直接相依于***關聯性的自反和可轉移關閉。</span><span class="sxs-lookup"><span data-stu-id="2d25f-208">Given this definition, the complete set of classes upon which a class depends is the reflexive and transitive closure of the ***directly depends on*** relationship.</span></span>

<span data-ttu-id="2d25f-209">範例</span><span class="sxs-lookup"><span data-stu-id="2d25f-209">The example</span></span>
```csharp
class A: A {}
```
<span data-ttu-id="2d25f-210">是錯誤的，因為類別相依于本身。</span><span class="sxs-lookup"><span data-stu-id="2d25f-210">is erroneous because the class depends on itself.</span></span> <span data-ttu-id="2d25f-211">同樣地，此範例</span><span class="sxs-lookup"><span data-stu-id="2d25f-211">Likewise, the example</span></span>
```csharp
class A: B {}
class B: C {}
class C: A {}
```
<span data-ttu-id="2d25f-212">發生錯誤，因為類別會迴圈相依于本身。</span><span class="sxs-lookup"><span data-stu-id="2d25f-212">is in error because the classes circularly depend on themselves.</span></span> <span data-ttu-id="2d25f-213">最後，範例</span><span class="sxs-lookup"><span data-stu-id="2d25f-213">Finally, the example</span></span>
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
<span data-ttu-id="2d25f-214">會導致編譯時期錯誤，因為 `A` 相依于 `B.C` （其直接的基類），這取決於 `B` （其立即封入類別），這會迴圈取決於 `A`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-214">results in a compile-time error because `A` depends on `B.C` (its direct base class), which depends on `B` (its immediately enclosing class), which circularly depends on `A`.</span></span>

<span data-ttu-id="2d25f-215">請注意，類別不會相依于其內的類別。</span><span class="sxs-lookup"><span data-stu-id="2d25f-215">Note that a class does not depend on the classes that are nested within it.</span></span> <span data-ttu-id="2d25f-216">在範例中</span><span class="sxs-lookup"><span data-stu-id="2d25f-216">In the example</span></span>
```csharp
class A
{
    class B: A {}
}
```
<span data-ttu-id="2d25f-217">`B` 相依于 `A` （因為 `A` 同時是它的直接基類和其立即封入類別），但 `A` 不依賴 `B` （因為 `B` 不是基類，也不是 `A`的封入類別）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-217">`B` depends on `A` (because `A` is both its direct base class and its immediately enclosing class), but `A` does not depend on `B` (since `B` is neither a base class nor an enclosing class of `A`).</span></span> <span data-ttu-id="2d25f-218">因此，此範例是有效的。</span><span class="sxs-lookup"><span data-stu-id="2d25f-218">Thus, the example is valid.</span></span>

<span data-ttu-id="2d25f-219">不可能從 `sealed` 類別衍生。</span><span class="sxs-lookup"><span data-stu-id="2d25f-219">It is not possible to derive from a `sealed` class.</span></span> <span data-ttu-id="2d25f-220">在範例中</span><span class="sxs-lookup"><span data-stu-id="2d25f-220">In the example</span></span>
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
<span data-ttu-id="2d25f-221">類別 `B` 發生錯誤，因為它會嘗試從 `sealed` 類別 `A`衍生。</span><span class="sxs-lookup"><span data-stu-id="2d25f-221">class `B` is in error because it attempts to derive from the `sealed` class `A`.</span></span>

#### <a name="interface-implementations"></a><span data-ttu-id="2d25f-222">介面實作</span><span class="sxs-lookup"><span data-stu-id="2d25f-222">Interface implementations</span></span>

<span data-ttu-id="2d25f-223">*Class_base*規格可能包括介面類別型的清單，在此情況下，類別會直接執行指定的介面類別型。</span><span class="sxs-lookup"><span data-stu-id="2d25f-223">A *class_base* specification may include a list of interface types, in which case the class is said to directly implement the given interface types.</span></span> <span data-ttu-id="2d25f-224">介面的執行會在[介面實現](interfaces.md#interface-implementations)中進一步討論。</span><span class="sxs-lookup"><span data-stu-id="2d25f-224">Interface implementations are discussed further in [Interface implementations](interfaces.md#interface-implementations).</span></span>

### <a name="type-parameter-constraints"></a><span data-ttu-id="2d25f-225">類型參數條件約束</span><span class="sxs-lookup"><span data-stu-id="2d25f-225">Type parameter constraints</span></span>

<span data-ttu-id="2d25f-226">泛型型別和方法宣告可以選擇性地指定型別參數條件約束，方法是包含*type_parameter_constraints_clause*s。</span><span class="sxs-lookup"><span data-stu-id="2d25f-226">Generic type and method declarations can optionally specify type parameter constraints by including *type_parameter_constraints_clause*s.</span></span>

```antlr
type_parameter_constraints_clause
    : 'where' type_parameter ':' type_parameter_constraints
    ;

type_parameter_constraints
    : primary_constraint
    | secondary_constraints
    | constructor_constraint
    | primary_constraint ',' secondary_constraints
    | primary_constraint ',' constructor_constraint
    | secondary_constraints ',' constructor_constraint
    | primary_constraint ',' secondary_constraints ',' constructor_constraint
    ;

primary_constraint
    : class_type
    | 'class'
    | 'struct'
    ;

secondary_constraints
    : interface_type
    | type_parameter
    | secondary_constraints ',' interface_type
    | secondary_constraints ',' type_parameter
    ;

constructor_constraint
    : 'new' '(' ')'
    ;
```

<span data-ttu-id="2d25f-227">每個*type_parameter_constraints_clause*都包含權杖 `where`，後面接著型別參數的名稱，後面接著冒號和該型別參數的條件約束清單。</span><span class="sxs-lookup"><span data-stu-id="2d25f-227">Each *type_parameter_constraints_clause* consists of the token `where`, followed by the name of a type parameter, followed by a colon and the list of constraints for that type parameter.</span></span> <span data-ttu-id="2d25f-228">每個類型參數最多隻能有一個 `where` 子句，而 `where` 子句可以依任何順序列出。</span><span class="sxs-lookup"><span data-stu-id="2d25f-228">There can be at most one `where` clause for each type parameter, and the `where` clauses can be listed in any order.</span></span> <span data-ttu-id="2d25f-229">就像屬性存取子中的 `get` 和 `set` 權杖一樣，`where` token 不是關鍵字。</span><span class="sxs-lookup"><span data-stu-id="2d25f-229">Like the `get` and `set` tokens in a property accessor, the `where` token is not a keyword.</span></span>

<span data-ttu-id="2d25f-230">在 `where` 子句中提供的條件約束清單，可以依下列順序包含下列任何元件：單一主要條件約束、一個或多個次要條件約束，以及一個 `new()`的函式條件約束。</span><span class="sxs-lookup"><span data-stu-id="2d25f-230">The list of constraints given in a `where` clause can include any of the following components, in this order: a single primary constraint, one or more secondary constraints, and the constructor constraint, `new()`.</span></span>

<span data-ttu-id="2d25f-231">主要條件約束可以是類別類型或***參考類型條件約束***`class` 或 `struct`的實***數值型別條件***約束。</span><span class="sxs-lookup"><span data-stu-id="2d25f-231">A primary constraint can be a class type or the ***reference type constraint*** `class` or the ***value type constraint*** `struct`.</span></span> <span data-ttu-id="2d25f-232">次要條件約束可以是*type_parameter*或*interface_type*。</span><span class="sxs-lookup"><span data-stu-id="2d25f-232">A secondary constraint can be a *type_parameter* or *interface_type*.</span></span>

<span data-ttu-id="2d25f-233">參考型別條件約束會指定用於型別參數的型別引數必須是參考型別。</span><span class="sxs-lookup"><span data-stu-id="2d25f-233">The reference type constraint specifies that a type argument used for the type parameter must be a reference type.</span></span> <span data-ttu-id="2d25f-234">所有類別類型、介面類別型、委派類型、陣列類型，以及已知為參考型別的類型參數（如下面所定義）都符合這個條件約束。</span><span class="sxs-lookup"><span data-stu-id="2d25f-234">All class types, interface types, delegate types, array types, and type parameters known to be a reference type (as defined below) satisfy this constraint.</span></span>

<span data-ttu-id="2d25f-235">實值型別條件約束會指定用於型別參數的型別引數，必須是不可為 null 的實值型別。</span><span class="sxs-lookup"><span data-stu-id="2d25f-235">The value type constraint specifies that a type argument used for the type parameter must be a non-nullable value type.</span></span> <span data-ttu-id="2d25f-236">所有不可為 null 的結構類型、列舉類型，以及具有實數值型別條件約束的類型參數都滿足此條件約束。</span><span class="sxs-lookup"><span data-stu-id="2d25f-236">All non-nullable struct types, enum types, and type parameters having the value type constraint satisfy this constraint.</span></span> <span data-ttu-id="2d25f-237">請注意，雖然分類為實值型別，但可為 null 的型別（[可為 null](types.md#nullable-types)的類型）並不符合實數值型別條件約束。</span><span class="sxs-lookup"><span data-stu-id="2d25f-237">Note that although classified as a value type, a nullable type ([Nullable types](types.md#nullable-types)) does not satisfy the value type constraint.</span></span> <span data-ttu-id="2d25f-238">具有實數值型別條件約束的類型參數，也不能有*constructor_constraint*。</span><span class="sxs-lookup"><span data-stu-id="2d25f-238">A type parameter having the value type constraint cannot also have the *constructor_constraint*.</span></span>

<span data-ttu-id="2d25f-239">指標類型永遠不允許是類型引數，而且不會被視為滿足參考型別或實數值型別條件約束。</span><span class="sxs-lookup"><span data-stu-id="2d25f-239">Pointer types are never allowed to be type arguments and are not considered to satisfy either the reference type or value type constraints.</span></span>

<span data-ttu-id="2d25f-240">如果條件約束是類別型別、介面型別或型別參數，則該型別會指定每個用於該型別參數的型別引數都必須支援的最小「基底型別」。</span><span class="sxs-lookup"><span data-stu-id="2d25f-240">If a constraint is a class type, an interface type, or a type parameter, that type specifies a minimal "base type" that every type argument used for that type parameter must support.</span></span> <span data-ttu-id="2d25f-241">每當使用了結構化型別或泛型方法時，就會在編譯時期針對型別參數的條件約束檢查型別引數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-241">Whenever a constructed type or generic method is used, the type argument is checked against the constraints on the type parameter at compile-time.</span></span> <span data-ttu-id="2d25f-242">提供的型別引數必須滿足滿足條件[約束](types.md#satisfying-constraints)中所述的條件。</span><span class="sxs-lookup"><span data-stu-id="2d25f-242">The type argument supplied must satisfy the conditions described in [Satisfying constraints](types.md#satisfying-constraints).</span></span>

<span data-ttu-id="2d25f-243">*Class_type*條件約束必須符合下列規則：</span><span class="sxs-lookup"><span data-stu-id="2d25f-243">A *class_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="2d25f-244">型別必須是類別型別。</span><span class="sxs-lookup"><span data-stu-id="2d25f-244">The type must be a class type.</span></span>
*  <span data-ttu-id="2d25f-245">類型不得 `sealed`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-245">The type must not be `sealed`.</span></span>
*  <span data-ttu-id="2d25f-246">類型不得為下列其中一種類型： `System.Array`、`System.Delegate`、`System.Enum`或 `System.ValueType`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-246">The type must not be one of the following types: `System.Array`, `System.Delegate`, `System.Enum`, or `System.ValueType`.</span></span>
*  <span data-ttu-id="2d25f-247">類型不得 `object`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-247">The type must not be `object`.</span></span> <span data-ttu-id="2d25f-248">由於所有類型都是衍生自 `object`，因此如果允許的話，這類條件約束就不會有任何作用。</span><span class="sxs-lookup"><span data-stu-id="2d25f-248">Because all types derive from `object`, such a constraint would have no effect if it were permitted.</span></span>
*  <span data-ttu-id="2d25f-249">給定類型參數最多隻能有一個條件約束為類別類型。</span><span class="sxs-lookup"><span data-stu-id="2d25f-249">At most one constraint for a given type parameter can be a class type.</span></span>

<span data-ttu-id="2d25f-250">指定為*interface_type*條件約束的類型必須符合下列規則：</span><span class="sxs-lookup"><span data-stu-id="2d25f-250">A type specified as an *interface_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="2d25f-251">型別必須是介面型別。</span><span class="sxs-lookup"><span data-stu-id="2d25f-251">The type must be an interface type.</span></span>
*  <span data-ttu-id="2d25f-252">在指定的 `where` 子句中，類型不得指定一次以上。</span><span class="sxs-lookup"><span data-stu-id="2d25f-252">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="2d25f-253">不論是哪一種情況，條件約束都可以包含相關聯型別或方法宣告的任何型別參數，做為結構化型別的一部分，而且可能牽涉到宣告的型別。</span><span class="sxs-lookup"><span data-stu-id="2d25f-253">In either case, the constraint can involve any of the type parameters of the associated type or method declaration as part of a constructed type, and can involve the type being declared.</span></span>

<span data-ttu-id="2d25f-254">任何指定為型別參數條件約束的類別或介面型別，都必須至少與所宣告的泛型型別或方法為可存取（[存取範圍條件約束](basic-concepts.md#accessibility-constraints)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-254">Any class or interface type specified as a type parameter constraint must be at least as accessible ([Accessibility constraints](basic-concepts.md#accessibility-constraints)) as the generic type or method being declared.</span></span>

<span data-ttu-id="2d25f-255">指定為*type_parameter*條件約束的類型必須符合下列規則：</span><span class="sxs-lookup"><span data-stu-id="2d25f-255">A type specified as a *type_parameter* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="2d25f-256">型別必須是型別參數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-256">The type must be a type parameter.</span></span>
*  <span data-ttu-id="2d25f-257">在指定的 `where` 子句中，類型不得指定一次以上。</span><span class="sxs-lookup"><span data-stu-id="2d25f-257">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="2d25f-258">此外，型別參數的相依性圖形中也不能有迴圈，其中 dependency 是所定義的可轉移關聯性：</span><span class="sxs-lookup"><span data-stu-id="2d25f-258">In addition there must be no cycles in the dependency graph of type parameters, where dependency is a transitive relation defined by:</span></span>

*  <span data-ttu-id="2d25f-259">如果使用型別參數 `T` 做為型別參數的條件約束 `S` 則 `S`***取決於***`T`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-259">If a type parameter `T` is used as a constraint for type parameter `S` then `S` ***depends on*** `T`.</span></span>
*  <span data-ttu-id="2d25f-260">如果型別參數 `S` 取決於型別參數 `T` 而且 `T` 取決於型別參數 `U` 則 `S` 會***依賴***`U`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-260">If a type parameter `S` depends on a type parameter `T` and `T` depends on a type parameter `U` then `S` ***depends on*** `U`.</span></span>

<span data-ttu-id="2d25f-261">基於此關聯性，類型參數要相依于本身（直接或間接）的編譯階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-261">Given this relation, it is a compile-time error for a type parameter to depend on itself (directly or indirectly).</span></span>

<span data-ttu-id="2d25f-262">所有條件約束在相依型別參數之間必須一致。</span><span class="sxs-lookup"><span data-stu-id="2d25f-262">Any constraints must be consistent among dependent type parameters.</span></span> <span data-ttu-id="2d25f-263">如果型別參數 `S` 取決於型別參數 `T` 則：</span><span class="sxs-lookup"><span data-stu-id="2d25f-263">If type parameter `S` depends on type parameter `T` then:</span></span>

*  <span data-ttu-id="2d25f-264">`T` 不能有實數值型別條件約束。</span><span class="sxs-lookup"><span data-stu-id="2d25f-264">`T` must not have the value type constraint.</span></span> <span data-ttu-id="2d25f-265">否則，`T` 實際上是密封的，因此 `S` 會強制為與 `T`相同的類型，而不需要兩個型別參數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-265">Otherwise, `T` is effectively sealed so `S` would be forced to be the same type as `T`, eliminating the need for two type parameters.</span></span>
*  <span data-ttu-id="2d25f-266">如果 `S` 具有實數值型別條件約束，則 `T` 不能有*class_type*條件約束。</span><span class="sxs-lookup"><span data-stu-id="2d25f-266">If `S` has the value type constraint then `T` must not have a *class_type* constraint.</span></span>
*  <span data-ttu-id="2d25f-267">如果 `S` 具有*class_type*條件約束 `A` 而且 `T` 有*class_type*條件約束，則必須有從 `B` 到 `A` 或從 `B` 到 `B` 的隱含參考轉換的識別轉換或隱含參考轉換。`A`</span><span class="sxs-lookup"><span data-stu-id="2d25f-267">If `S` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>
*  <span data-ttu-id="2d25f-268">如果 `S` 也相依于型別參數 `U` 而且 `U` 具有*class_type*條件約束 `A` 而 `T` 有*class_type*條件約束 `B` 則必須從 `A` 到 `B` 或從 `B` 到 `A`的隱含參考轉換，進行身分識別轉換或隱含參考轉換。</span><span class="sxs-lookup"><span data-stu-id="2d25f-268">If `S` also depends on type parameter `U` and `U` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>

<span data-ttu-id="2d25f-269">`S` 具有實數值型別條件約束，且 `T` 具有參考型別條件約束，則有效。</span><span class="sxs-lookup"><span data-stu-id="2d25f-269">It is valid for `S` to have the value type constraint and `T` to have the reference type constraint.</span></span> <span data-ttu-id="2d25f-270">實際上，這會限制 `T` `System.Object`、`System.ValueType`、`System.Enum`和任何介面類別型的類型。</span><span class="sxs-lookup"><span data-stu-id="2d25f-270">Effectively this limits `T` to the types `System.Object`, `System.ValueType`, `System.Enum`, and any interface type.</span></span>

<span data-ttu-id="2d25f-271">如果型別參數的 `where` 子句包含「函式」條件約束（其具有 `new()`的形式），則可以使用 `new` 運算子來建立型別的實例（[物件建立運算式](expressions.md#object-creation-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-271">If the `where` clause for a type parameter includes a constructor constraint (which has the form `new()`), it is possible to use the `new` operator to create instances of the type ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span> <span data-ttu-id="2d25f-272">任何用於具有「函式」條件約束之型別參數的型別引數，都必須具有公用無參數的處理常式（這個此函式會針對任何實值型別隱含存在），或是具有實值型別條件約束或「函式條件約束[」的型](classes.md#type-parameter-constraints)別參數</span><span class="sxs-lookup"><span data-stu-id="2d25f-272">Any type argument used for a type parameter with a constructor constraint must have a public parameterless constructor (this constructor implicitly exists for any value type) or be a type parameter having the value type constraint or constructor constraint (see [Type parameter constraints](classes.md#type-parameter-constraints) for details).</span></span>

<span data-ttu-id="2d25f-273">以下是條件約束的範例：</span><span class="sxs-lookup"><span data-stu-id="2d25f-273">The following are examples of constraints:</span></span>
```csharp
interface IPrintable
{
    void Print();
}

interface IComparable<T>
{
    int CompareTo(T value);
}

interface IKeyProvider<T>
{
    T GetKey();
}

class Printer<T> where T: IPrintable {...}

class SortedList<T> where T: IComparable<T> {...}

class Dictionary<K,V>
    where K: IComparable<K>
    where V: IPrintable, IKeyProvider<K>, new()
{
    ...
}
```

<span data-ttu-id="2d25f-274">下列範例是錯誤的，因為它會在型別參數的相依性圖形中造成迴圈：</span><span class="sxs-lookup"><span data-stu-id="2d25f-274">The following example is in error because it causes a circularity in the dependency graph of the type parameters:</span></span>
```csharp
class Circular<S,T>
    where S: T
    where T: S                // Error, circularity in dependency graph
{
    ...
}
```

<span data-ttu-id="2d25f-275">下列範例說明其他不正確情況：</span><span class="sxs-lookup"><span data-stu-id="2d25f-275">The following examples illustrate additional invalid situations:</span></span>
```csharp
class Sealed<S,T>
    where S: T
    where T: struct        // Error, T is sealed
{
    ...
}

class A {...}

class B {...}

class Incompat<S,T>
    where S: A, T
    where T: B                // Error, incompatible class-type constraints
{
    ...
}

class StructWithClass<S,T,U>
    where S: struct, T
    where T: U
    where U: A                // Error, A incompatible with struct
{
    ...
}
```

<span data-ttu-id="2d25f-276">類型參數 `T` 的***有效基類***定義如下：</span><span class="sxs-lookup"><span data-stu-id="2d25f-276">The ***effective base class*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="2d25f-277">如果 `T` 沒有主要條件約束或類型參數條件約束，則會 `object`其有效基類。</span><span class="sxs-lookup"><span data-stu-id="2d25f-277">If `T` has no primary constraints or type parameter constraints, its effective base class is `object`.</span></span>
*  <span data-ttu-id="2d25f-278">如果 `T` 具有實數值型別條件約束，則會 `System.ValueType`其有效基類。</span><span class="sxs-lookup"><span data-stu-id="2d25f-278">If `T` has the value type constraint, its effective base class is `System.ValueType`.</span></span>
*  <span data-ttu-id="2d25f-279">如果 `T` 有*class_type*條件約束 `C` 但沒有*type_parameter*條件約束，則會 `C`其有效基類。</span><span class="sxs-lookup"><span data-stu-id="2d25f-279">If `T` has a *class_type* constraint `C` but no *type_parameter* constraints, its effective base class is `C`.</span></span>
*  <span data-ttu-id="2d25f-280">如果 `T` 沒有*class_type*條件約束，但有一或多個*type_parameter*條件約束，其有效基類就是其*type_parameter*條件約束之有效基類集中最包含的類型（[提升轉換運算子](conversions.md#lifted-conversion-operators)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-280">If `T` has no *class_type* constraint but has one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set of effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="2d25f-281">一致性規則可確保這類最包含的類型存在。</span><span class="sxs-lookup"><span data-stu-id="2d25f-281">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="2d25f-282">如果 `T` 同時具備*class_type*條件約束和一或多個*type_parameter*條件約束，其有效基類是包含 `T` 之*class_type*條件約束和其*type_parameter*條件約束之有效基類的集合中最包含的類型（[提升轉換運算子](conversions.md#lifted-conversion-operators)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-282">If `T` has both a *class_type* constraint and one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set consisting of the *class_type* constraint of `T` and the effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="2d25f-283">一致性規則可確保這類最包含的類型存在。</span><span class="sxs-lookup"><span data-stu-id="2d25f-283">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="2d25f-284">如果 `T` 有參考型別條件約束，但沒有*class_type*條件約束，則會 `object`有效的基類。</span><span class="sxs-lookup"><span data-stu-id="2d25f-284">If `T` has the reference type constraint but no *class_type* constraints, its effective base class is `object`.</span></span>

<span data-ttu-id="2d25f-285">基於這些規則的目的，如果 T 具有*value_type*的條件約束 `V`，請改用*class_type*的最特定 `V` 基底類型。</span><span class="sxs-lookup"><span data-stu-id="2d25f-285">For the purpose of these rules, if T has a constraint `V` that is a *value_type*, use instead the most specific base type of `V` that is a *class_type*.</span></span> <span data-ttu-id="2d25f-286">這可能永遠不會發生在明確指定的條件約束中，但當覆寫方法宣告或介面方法的明確執行隱含繼承泛型方法的條件約束時，可能會發生這種情況。</span><span class="sxs-lookup"><span data-stu-id="2d25f-286">This can never happen in an explicitly given constraint, but may occur when the constraints of a generic method are implicitly inherited by an overriding method declaration or an explicit implementation of an interface method.</span></span>

<span data-ttu-id="2d25f-287">這些規則會確保有效的基類一律為*class_type*。</span><span class="sxs-lookup"><span data-stu-id="2d25f-287">These rules ensure that the effective base class is always a *class_type*.</span></span>

<span data-ttu-id="2d25f-288">`T` 類型參數的***有效介面集***定義如下：</span><span class="sxs-lookup"><span data-stu-id="2d25f-288">The ***effective interface set*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="2d25f-289">如果 `T` 沒有*secondary_constraints*，其有效的介面集就是空的。</span><span class="sxs-lookup"><span data-stu-id="2d25f-289">If `T` has no *secondary_constraints*, its effective interface set is empty.</span></span>
*  <span data-ttu-id="2d25f-290">如果 `T` 有*interface_type*條件約束，但沒有*type_parameter*條件約束，其有效的介面集就是它的一組*interface_type*條件約束。</span><span class="sxs-lookup"><span data-stu-id="2d25f-290">If `T` has *interface_type* constraints but no *type_parameter* constraints, its effective interface set is its set of *interface_type* constraints.</span></span>
*  <span data-ttu-id="2d25f-291">如果 `T` 沒有*interface_type*條件約束，但有*type_parameter*條件約束，其有效介面集就是其*type_parameter*條件約束之有效介面集的聯集。</span><span class="sxs-lookup"><span data-stu-id="2d25f-291">If `T` has no *interface_type* constraints but has *type_parameter* constraints, its effective interface set is the union of the effective interface sets of its *type_parameter* constraints.</span></span>
*  <span data-ttu-id="2d25f-292">如果 `T` 同時具備*interface_type*條件約束和*type_parameter*條件約束，其有效的介面集就是其*type_parameter*條件約束集合的聯集*interface_type*條件約束和有效介面集。</span><span class="sxs-lookup"><span data-stu-id="2d25f-292">If `T` has both *interface_type* constraints and *type_parameter* constraints, its effective interface set is the union of its set of *interface_type* constraints and the effective interface sets of its *type_parameter* constraints.</span></span>

<span data-ttu-id="2d25f-293">如果類型參數具有參考型別條件約束，或其有效基類不是 `object` 或 `System.ValueType`，則其為***參考型別***。</span><span class="sxs-lookup"><span data-stu-id="2d25f-293">A type parameter is ***known to be a reference type*** if it has the reference type constraint or its effective base class is not `object` or `System.ValueType`.</span></span>

<span data-ttu-id="2d25f-294">條件約束類型參數類型的值，可以用來存取限制式所隱含的實例成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-294">Values of a constrained type parameter type can be used to access the instance members implied by the constraints.</span></span> <span data-ttu-id="2d25f-295">在範例中</span><span class="sxs-lookup"><span data-stu-id="2d25f-295">In the example</span></span>
```csharp
interface IPrintable
{
    void Print();
}

class Printer<T> where T: IPrintable
{
    void PrintOne(T x) {
        x.Print();
    }
}
```
<span data-ttu-id="2d25f-296">`IPrintable` 的方法可以直接在 `x` 上叫用，因為 `T` 受到限制，一律會執行 `IPrintable`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-296">the methods of `IPrintable` can be invoked directly on `x` because `T` is constrained to always implement `IPrintable`.</span></span>

### <a name="class-body"></a><span data-ttu-id="2d25f-297">類別主體</span><span class="sxs-lookup"><span data-stu-id="2d25f-297">Class body</span></span>

<span data-ttu-id="2d25f-298">類別的*class_body*會定義該類別的成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-298">The *class_body* of a class defines the members of that class.</span></span>

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a><span data-ttu-id="2d25f-299">部分型別</span><span class="sxs-lookup"><span data-stu-id="2d25f-299">Partial types</span></span>

<span data-ttu-id="2d25f-300">類型宣告可以分割成多個***部分類型***宣告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-300">A type declaration can be split across multiple ***partial type declarations***.</span></span> <span data-ttu-id="2d25f-301">類型宣告會依照本節中的規則從其元件加以結構化，此時在程式的編譯時間和執行時間處理的其餘部分期間，會將它視為單一宣告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-301">The type declaration is constructed from its parts by following the rules in this section, whereupon it is treated as a single declaration during the remainder of the compile-time and run-time processing of the program.</span></span>

<span data-ttu-id="2d25f-302">*Class_declaration*， *struct_declaration*或*interface_declaration*如果包含 `partial` 修飾詞，則代表部分類型宣告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-302">A *class_declaration*, *struct_declaration* or *interface_declaration* represents a partial type declaration if it includes a `partial` modifier.</span></span> <span data-ttu-id="2d25f-303">`partial` 不是關鍵字，而且只會作為修飾詞（如果它緊接在 `class`的其中一個關鍵字之前）、`struct` 或 `interface` 在類型宣告中，或在方法宣告中的類型 `void` 之前。</span><span class="sxs-lookup"><span data-stu-id="2d25f-303">`partial` is not a keyword, and only acts as a modifier if it appears immediately before one of the keywords `class`, `struct` or `interface` in a type declaration, or before the type `void` in a method declaration.</span></span> <span data-ttu-id="2d25f-304">在其他內容中，它可以用來做為一般識別碼。</span><span class="sxs-lookup"><span data-stu-id="2d25f-304">In other contexts it can be used as a normal identifier.</span></span>

<span data-ttu-id="2d25f-305">部分類型宣告的每個部分都必須包含 `partial` 修飾詞。</span><span class="sxs-lookup"><span data-stu-id="2d25f-305">Each part of a partial type declaration must include a `partial` modifier.</span></span> <span data-ttu-id="2d25f-306">它必須具有相同的名稱，而且必須在與其他元件相同的命名空間或類型宣告中宣告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-306">It must have the same name  and be declared in the same namespace or type declaration as the other parts.</span></span> <span data-ttu-id="2d25f-307">`partial` 修飾詞表示類型宣告的其他部分可能存在於其他地方，但這類額外的部分不是必要條件。它適用于具有單一宣告的類型，以包含 `partial` 修飾詞。</span><span class="sxs-lookup"><span data-stu-id="2d25f-307">The `partial` modifier indicates that additional parts of the type declaration may exist elsewhere, but the existence of such additional parts is not a requirement; it is valid for a type with a single declaration to include the `partial` modifier.</span></span>

<span data-ttu-id="2d25f-308">部分類型的所有部分都必須一起編譯，使元件可以在編譯時期合併成單一類型宣告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-308">All parts of a partial type must be compiled together such that the parts can be merged at compile-time into a single type declaration.</span></span> <span data-ttu-id="2d25f-309">部分類型特別不允許延伸已編譯的類型。</span><span class="sxs-lookup"><span data-stu-id="2d25f-309">Partial types specifically do not allow already compiled types to be extended.</span></span>

<span data-ttu-id="2d25f-310">您可以使用 `partial` 修飾詞，在多個元件中宣告嵌套的類型。</span><span class="sxs-lookup"><span data-stu-id="2d25f-310">Nested types may be declared in multiple parts by using the `partial` modifier.</span></span> <span data-ttu-id="2d25f-311">一般來說，包含類型也會使用 `partial` 來宣告，而巢狀型別的每個部分都會在包含類型的不同部分中宣告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-311">Typically, the containing type is declared using `partial` as well, and each part of the nested type is declared in a different part of the containing type.</span></span>

<span data-ttu-id="2d25f-312">委派或列舉宣告上不允許 `partial` 修飾詞。</span><span class="sxs-lookup"><span data-stu-id="2d25f-312">The `partial` modifier is not permitted on delegate or enum declarations.</span></span>

### <a name="attributes"></a><span data-ttu-id="2d25f-313">屬性</span><span class="sxs-lookup"><span data-stu-id="2d25f-313">Attributes</span></span>

<span data-ttu-id="2d25f-314">部分類型的屬性是藉由以未指定的順序結合每個元件的屬性來決定。</span><span class="sxs-lookup"><span data-stu-id="2d25f-314">The attributes of a partial type are determined by combining, in an unspecified order, the attributes of each of the parts.</span></span> <span data-ttu-id="2d25f-315">如果屬性放在多個部分，它就相當於在類型上多次指定屬性。</span><span class="sxs-lookup"><span data-stu-id="2d25f-315">If an attribute is placed on multiple parts, it is equivalent to specifying the attribute multiple times on the type.</span></span> <span data-ttu-id="2d25f-316">例如，這兩個部分：</span><span class="sxs-lookup"><span data-stu-id="2d25f-316">For example, the two parts:</span></span>

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
<span data-ttu-id="2d25f-317">相當於如下的宣告：</span><span class="sxs-lookup"><span data-stu-id="2d25f-317">are equivalent to a declaration such as:</span></span>
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

<span data-ttu-id="2d25f-318">類型參數上的屬性會以類似的方式結合。</span><span class="sxs-lookup"><span data-stu-id="2d25f-318">Attributes on type parameters combine in a similar fashion.</span></span>

### <a name="modifiers"></a><span data-ttu-id="2d25f-319">修飾詞</span><span class="sxs-lookup"><span data-stu-id="2d25f-319">Modifiers</span></span>

<span data-ttu-id="2d25f-320">當部分類型宣告包含協助工具規格（`public`、`protected`、`internal`和 `private` 修飾詞）時，必須同意包含協助工具規格的所有其他部分。</span><span class="sxs-lookup"><span data-stu-id="2d25f-320">When a partial type declaration includes an accessibility specification (the `public`, `protected`, `internal`, and `private` modifiers) it must agree with all other parts that include an accessibility specification.</span></span> <span data-ttu-id="2d25f-321">如果部分類型中沒有任何部分包含協助工具規格，則會提供適當的預設存取範圍（已宣告的[存取](basic-concepts.md#declared-accessibility)範圍）給該類型。</span><span class="sxs-lookup"><span data-stu-id="2d25f-321">If no part of a partial type includes an accessibility specification, the type is given the appropriate default accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="2d25f-322">如果巢狀型別的一或多個部分宣告包含 `new` 修飾詞，則如果嵌套的類型隱藏繼承的成員（[透過繼承隱藏](basic-concepts.md#hiding-through-inheritance)），則不會回報警告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-322">If one or more partial declarations of a nested type include a `new` modifier, no warning is reported if the nested type hides an inherited member ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)).</span></span>

<span data-ttu-id="2d25f-323">如果類別的一個或多個部分宣告包含 `abstract` 修飾詞，則會將類別視為抽象（[抽象類別](classes.md#abstract-classes)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-323">If one or more partial declarations of a class include an `abstract` modifier, the class is considered abstract ([Abstract classes](classes.md#abstract-classes)).</span></span> <span data-ttu-id="2d25f-324">否則，類別會被視為非抽象。</span><span class="sxs-lookup"><span data-stu-id="2d25f-324">Otherwise, the class is considered non-abstract.</span></span>

<span data-ttu-id="2d25f-325">如果類別的一個或多個部分宣告包含 `sealed` 修飾詞，則會將類別視為 sealed （[密封類別](classes.md#sealed-classes)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-325">If one or more partial declarations of a class include a `sealed` modifier, the class is considered sealed ([Sealed classes](classes.md#sealed-classes)).</span></span> <span data-ttu-id="2d25f-326">否則，會將類別視為未密封。</span><span class="sxs-lookup"><span data-stu-id="2d25f-326">Otherwise, the class is considered unsealed.</span></span>

<span data-ttu-id="2d25f-327">請注意，類別不能同時為 abstract 和 sealed。</span><span class="sxs-lookup"><span data-stu-id="2d25f-327">Note that a class cannot be both abstract and sealed.</span></span>

<span data-ttu-id="2d25f-328">`unsafe`在部分類型宣告上使用修飾詞時，只有該特定部分會被視為不安全的內容（[不安全的內容](unsafe-code.md#unsafe-contexts)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-328">When the `unsafe` modifier is used on a partial type declaration, only that particular part is considered an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span>

### <a name="type-parameters-and-constraints"></a><span data-ttu-id="2d25f-329">類型參數和條件約束</span><span class="sxs-lookup"><span data-stu-id="2d25f-329">Type parameters and constraints</span></span>

<span data-ttu-id="2d25f-330">如果泛型型別宣告于多個部分，則每個部分都必須陳述型別參數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-330">If a generic type is declared in multiple parts, each part must state the type parameters.</span></span> <span data-ttu-id="2d25f-331">每個元件都必須具有相同數目的類型參數，而且每個類型參數的名稱相同。</span><span class="sxs-lookup"><span data-stu-id="2d25f-331">Each part must have the same number of type parameters, and the same name for each type parameter, in order.</span></span>

<span data-ttu-id="2d25f-332">當部分泛型型別宣告包含條件約束（`where` 子句）時，條件約束必須同意包含條件約束的所有其他部分。</span><span class="sxs-lookup"><span data-stu-id="2d25f-332">When a partial generic type declaration includes constraints (`where` clauses), the constraints must agree with all other parts that include constraints.</span></span> <span data-ttu-id="2d25f-333">具體而言，每個包含條件約束的元件都必須具有相同類型參數集的條件約束，而針對每個類型參數，主要、次要和函式條件約束的集合必須相等。</span><span class="sxs-lookup"><span data-stu-id="2d25f-333">Specifically, each part that includes constraints must have constraints for the same set of type parameters, and for each type parameter the sets of primary, secondary, and constructor constraints must be equivalent.</span></span> <span data-ttu-id="2d25f-334">如果兩組條件約束包含相同的成員，則兩者都是相等的。</span><span class="sxs-lookup"><span data-stu-id="2d25f-334">Two sets of constraints are equivalent if they contain the same members.</span></span> <span data-ttu-id="2d25f-335">如果部分泛型型別沒有任何部分指定類型參數條件約束，則會將類型參數視為不受限制。</span><span class="sxs-lookup"><span data-stu-id="2d25f-335">If no part of a partial generic type specifies type parameter constraints, the type parameters are considered unconstrained.</span></span>

<span data-ttu-id="2d25f-336">範例</span><span class="sxs-lookup"><span data-stu-id="2d25f-336">The example</span></span>
```csharp
partial class Dictionary<K,V>
    where K: IComparable<K>
    where V: IKeyProvider<K>, IPersistable
{
    ...
}

partial class Dictionary<K,V>
    where V: IPersistable, IKeyProvider<K>
    where K: IComparable<K>
{
    ...
}

partial class Dictionary<K,V>
{
    ...
}
```
<span data-ttu-id="2d25f-337">是正確的，因為包含條件約束（前兩個）的元件會分別針對同一組類型參數，有效地指定相同的主要、次要和函式條件約束集合。</span><span class="sxs-lookup"><span data-stu-id="2d25f-337">is correct because those parts that include constraints (the first two) effectively specify the same set of primary, secondary, and constructor constraints for the same set of type parameters, respectively.</span></span>

### <a name="base-class"></a><span data-ttu-id="2d25f-338">基底類別</span><span class="sxs-lookup"><span data-stu-id="2d25f-338">Base class</span></span>

<span data-ttu-id="2d25f-339">當部分類別宣告包含基類規格時，必須同意包含基類規格的所有其他部分。</span><span class="sxs-lookup"><span data-stu-id="2d25f-339">When a partial class declaration includes a base class specification it must agree with all other parts that include a base class specification.</span></span> <span data-ttu-id="2d25f-340">如果部分類別中沒有部分包含基類規格，基底類別會變成 `System.Object` （[基類](classes.md#base-classes)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-340">If no part of a partial class includes a base class specification, the base class becomes `System.Object` ([Base classes](classes.md#base-classes)).</span></span>

### <a name="base-interfaces"></a><span data-ttu-id="2d25f-341">基底介面</span><span class="sxs-lookup"><span data-stu-id="2d25f-341">Base interfaces</span></span>

<span data-ttu-id="2d25f-342">在多個元件中宣告之類型的基底介面集合，是在每個元件上所指定之基底介面的聯集。</span><span class="sxs-lookup"><span data-stu-id="2d25f-342">The set of base interfaces for a type declared in multiple parts is the union of the base interfaces specified on each part.</span></span> <span data-ttu-id="2d25f-343">特定基底介面只能在每個元件上命名一次，但允許多個部分命名為相同的基底介面。</span><span class="sxs-lookup"><span data-stu-id="2d25f-343">A particular base interface may only be named once on each part, but it is permitted for multiple parts to name the same base interface(s).</span></span> <span data-ttu-id="2d25f-344">任何給定基底介面的成員都只能有一個執行。</span><span class="sxs-lookup"><span data-stu-id="2d25f-344">There must only be one implementation of the members of any given base interface.</span></span>

<span data-ttu-id="2d25f-345">在範例中</span><span class="sxs-lookup"><span data-stu-id="2d25f-345">In the example</span></span>
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
<span data-ttu-id="2d25f-346">類別 `C` 的基底介面集是 `IA`、`IB`和 `IC`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-346">the set of base interfaces for class `C` is `IA`, `IB`, and `IC`.</span></span>

<span data-ttu-id="2d25f-347">一般來說，每個部分都會提供在該元件上宣告之介面的執行;不過，這不是必要條件。</span><span class="sxs-lookup"><span data-stu-id="2d25f-347">Typically, each part provides an implementation of the interface(s) declared on that part; however, this is not a requirement.</span></span> <span data-ttu-id="2d25f-348">元件可以針對在不同元件上宣告的介面提供執行：</span><span class="sxs-lookup"><span data-stu-id="2d25f-348">A part may provide the implementation for an interface declared on a different part:</span></span>
```csharp
partial class X
{
    int IComparable.CompareTo(object o) {...}
}

partial class X: IComparable
{
    ...
}
```

### <a name="members"></a><span data-ttu-id="2d25f-349">成員</span><span class="sxs-lookup"><span data-stu-id="2d25f-349">Members</span></span>

<span data-ttu-id="2d25f-350">除了部分方法（[部分方法](classes.md#partial-methods)）之外，在多個部分宣告之類型的成員集合，只是在每個部分宣告的成員集合的聯集。</span><span class="sxs-lookup"><span data-stu-id="2d25f-350">With the exception of partial methods ([Partial methods](classes.md#partial-methods)), the set of members of a type declared in multiple parts is simply the union of the set of members declared in each part.</span></span> <span data-ttu-id="2d25f-351">類型宣告的所有部分的主體會共用相同的宣告空間（宣告[），](basic-concepts.md#declarations)而每個成員的範圍（[範圍](basic-concepts.md#scopes)）都會延伸至所有元件的主體。</span><span class="sxs-lookup"><span data-stu-id="2d25f-351">The bodies of all parts of the type declaration share the same declaration space ([Declarations](basic-concepts.md#declarations)), and the scope of each member ([Scopes](basic-concepts.md#scopes)) extends to the bodies of all the parts.</span></span> <span data-ttu-id="2d25f-352">任何成員的存取範圍定義域一律包含封入類型的所有部分;在一個元件中宣告的 `private` 成員可從另一個部分自由存取。</span><span class="sxs-lookup"><span data-stu-id="2d25f-352">The accessibility domain of any member always includes all the parts of the enclosing type; a `private` member declared in one part is freely accessible from another part.</span></span> <span data-ttu-id="2d25f-353">在類型的多個部分中宣告相同的成員是編譯時期錯誤，除非該成員是具有 `partial` 修飾詞的類型。</span><span class="sxs-lookup"><span data-stu-id="2d25f-353">It is a compile-time error to declare the same member in more than one part of the type, unless that member is a type with the `partial` modifier.</span></span>

```csharp
partial class A
{
    int x;                     // Error, cannot declare x more than once

    partial class Inner        // Ok, Inner is a partial type
    {
        int y;
    }
}

partial class A
{
    int x;                     // Error, cannot declare x more than once

    partial class Inner        // Ok, Inner is a partial type
    {
        int z;
    }
}
```

<span data-ttu-id="2d25f-354">類型中的成員順序很少對C#程式碼很重要，但在與其他語言和環境互動時可能會很重要。</span><span class="sxs-lookup"><span data-stu-id="2d25f-354">The ordering of members within a type is rarely significant to C# code, but may be significant when interfacing with other languages and environments.</span></span> <span data-ttu-id="2d25f-355">在這些情況下，在多個元件中宣告的類型內的成員順序未定義。</span><span class="sxs-lookup"><span data-stu-id="2d25f-355">In these cases, the ordering of members within a type declared in multiple parts is undefined.</span></span>

### <a name="partial-methods"></a><span data-ttu-id="2d25f-356">部分方法</span><span class="sxs-lookup"><span data-stu-id="2d25f-356">Partial methods</span></span>

<span data-ttu-id="2d25f-357">部分方法可以在類型宣告的某個部分中定義，並在另一個元件中執行。</span><span class="sxs-lookup"><span data-stu-id="2d25f-357">Partial methods can be defined in one part of a type declaration and implemented in another.</span></span> <span data-ttu-id="2d25f-358">執行是選擇性的;如果沒有部分執行部分方法，部分方法宣告和其所有呼叫都會從元件組合所產生的類型宣告中移除。</span><span class="sxs-lookup"><span data-stu-id="2d25f-358">The implementation is optional; if no part implements the partial method, the partial method declaration and all calls to it are removed from the type declaration resulting from the combination of the parts.</span></span>

<span data-ttu-id="2d25f-359">部分方法不能定義存取修飾詞，而是隱含 `private`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-359">Partial methods cannot define access modifiers, but are implicitly `private`.</span></span> <span data-ttu-id="2d25f-360">其傳回型別必須 `void`，而且其參數不能有 `out` 修飾詞。</span><span class="sxs-lookup"><span data-stu-id="2d25f-360">Their return type must be `void`, and their parameters cannot have the `out` modifier.</span></span> <span data-ttu-id="2d25f-361">只有當識別碼 `partial` 在 `void` 類型之前出現時，才會被辨識為方法宣告中的特殊關鍵字。否則，它可以用來做為一般識別碼。</span><span class="sxs-lookup"><span data-stu-id="2d25f-361">The identifier `partial` is recognized as a special keyword in a method declaration only if it appears right before the `void` type; otherwise it can be used as a normal identifier.</span></span> <span data-ttu-id="2d25f-362">部分方法無法明確地執行介面方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-362">A partial method cannot explicitly implement interface methods.</span></span>

<span data-ttu-id="2d25f-363">部分方法宣告的類型有兩種：如果方法宣告的主體是分號，則宣告會被視為***定義部分方法***宣告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-363">There are two kinds of partial method declarations: If the body of the method declaration is a semicolon, the declaration is said to be a ***defining partial method declaration***.</span></span> <span data-ttu-id="2d25f-364">如果主體是以*區塊*的形式提供，則宣告會被視為實作為***部分方法***宣告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-364">If the body is given as a *block*, the declaration is said to be an ***implementing partial method declaration***.</span></span> <span data-ttu-id="2d25f-365">在類型宣告的各個部分中，只能有一個定義具有指定簽章的部分方法宣告，而且只能有一個具有指定簽章的執行部分方法宣告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-365">Across the parts of a type declaration there can be only one defining partial method declaration with a given signature, and there can be only one implementing partial method declaration with a given signature.</span></span> <span data-ttu-id="2d25f-366">如果指定了執行部分方法宣告，則必須有對應的定義部分方法宣告，而且宣告必須符合下列內容中所指定的：</span><span class="sxs-lookup"><span data-stu-id="2d25f-366">If an implementing partial method declaration is given, a corresponding defining partial method declaration must exist, and the declarations must match as specified in the following:</span></span>

* <span data-ttu-id="2d25f-367">宣告必須具有相同的修飾詞（雖然不一定是相同的順序）、方法名稱、型別參數數目和參數數目。</span><span class="sxs-lookup"><span data-stu-id="2d25f-367">The declarations must have the same modifiers (although not necessarily in the same order), method name, number of type parameters and number of parameters.</span></span>
* <span data-ttu-id="2d25f-368">宣告中的對應參數必須具有相同的修飾詞（雖然不一定是相同的順序）和相同的類型（類型參數名稱的模數差異）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-368">Corresponding parameters in the declarations must have the same modifiers (although not necessarily in the same order) and the same types (modulo differences in type parameter names).</span></span>
* <span data-ttu-id="2d25f-369">宣告中的對應類型參數必須具有相同的條件約束（類型參數名稱的模數差異）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-369">Corresponding type parameters in the declarations must have the same constraints (modulo differences in type parameter names).</span></span>

<span data-ttu-id="2d25f-370">執行部分方法宣告可以與對應的定義部分方法宣告出現在相同的部分。</span><span class="sxs-lookup"><span data-stu-id="2d25f-370">An implementing partial method declaration can appear in the same part as the corresponding defining partial method declaration.</span></span>

<span data-ttu-id="2d25f-371">只有定義部分方法會參與多載解析。</span><span class="sxs-lookup"><span data-stu-id="2d25f-371">Only a defining partial method participates in overload resolution.</span></span> <span data-ttu-id="2d25f-372">因此，不論是否指定了實做宣告，調用運算式都可以解析為部分方法的調用。</span><span class="sxs-lookup"><span data-stu-id="2d25f-372">Thus, whether or not an implementing declaration is given, invocation expressions may resolve to invocations of the partial method.</span></span> <span data-ttu-id="2d25f-373">因為部分方法一律會傳回 `void`，所以這類調用運算式一律會是運算式語句。</span><span class="sxs-lookup"><span data-stu-id="2d25f-373">Because a partial method always returns `void`, such invocation expressions will always be expression statements.</span></span> <span data-ttu-id="2d25f-374">此外，由於部分方法會隱含 `private`，因此這類語句一律會出現在宣告部分方法之類型宣告的其中一個部分內。</span><span class="sxs-lookup"><span data-stu-id="2d25f-374">Furthermore, because a partial method is implicitly `private`, such statements will always occur within one of the parts of the type declaration within which the partial method is declared.</span></span>

<span data-ttu-id="2d25f-375">如果部分類型宣告中沒有任何部分包含給定部分方法的實作為宣告，則叫用它的任何運算式語句都會直接從結合的類型宣告中移除。</span><span class="sxs-lookup"><span data-stu-id="2d25f-375">If no part of a partial type declaration contains an implementing declaration for a given partial method, any expression statement invoking it is simply removed from the combined type declaration.</span></span> <span data-ttu-id="2d25f-376">因此，叫用運算式（包括任何組成運算式）在執行時間不會有任何作用。</span><span class="sxs-lookup"><span data-stu-id="2d25f-376">Thus the invocation expression, including any constituent expressions, has no effect at run-time.</span></span> <span data-ttu-id="2d25f-377">部分方法本身也會移除，且不會是合併類型宣告的成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-377">The partial method itself is also removed and will not be a member of the combined type declaration.</span></span>

<span data-ttu-id="2d25f-378">如果指定的部分方法有執行宣告，則會保留部分方法的調用。</span><span class="sxs-lookup"><span data-stu-id="2d25f-378">If an implementing declaration exist for a given partial method, the invocations of the partial methods are retained.</span></span> <span data-ttu-id="2d25f-379">部分方法會增加與執行部分方法宣告類似的方法宣告，但不包括下列各項：</span><span class="sxs-lookup"><span data-stu-id="2d25f-379">The partial method gives rise to a method declaration similar to the implementing partial method declaration except for the following:</span></span>

* <span data-ttu-id="2d25f-380">不包含 `partial` 修飾詞</span><span class="sxs-lookup"><span data-stu-id="2d25f-380">The `partial` modifier is not included</span></span>
* <span data-ttu-id="2d25f-381">產生的方法宣告中的屬性是定義的結合屬性和以未指定循序執行的部分方法宣告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-381">The attributes in the resulting method declaration are the combined attributes of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="2d25f-382">不會移除重複專案。</span><span class="sxs-lookup"><span data-stu-id="2d25f-382">Duplicates are not removed.</span></span>
* <span data-ttu-id="2d25f-383">產生的方法宣告之參數上的屬性，是以未指定順序定義和執行部分方法宣告之對應參數的結合屬性。</span><span class="sxs-lookup"><span data-stu-id="2d25f-383">The attributes on the parameters of the resulting method declaration are the combined attributes of the corresponding parameters of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="2d25f-384">不會移除重複專案。</span><span class="sxs-lookup"><span data-stu-id="2d25f-384">Duplicates are not removed.</span></span>

<span data-ttu-id="2d25f-385">如果指定了部分方法 M 的定義宣告，但不是執行宣告，則適用下列限制：</span><span class="sxs-lookup"><span data-stu-id="2d25f-385">If a defining declaration but not an implementing declaration is given for a partial method M, the following restrictions apply:</span></span>

* <span data-ttu-id="2d25f-386">建立方法（[委派建立運算式](expressions.md#delegate-creation-expressions)）的委派時，會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-386">It is a compile-time error to create a delegate to method ([Delegate creation expressions](expressions.md#delegate-creation-expressions)).</span></span>
* <span data-ttu-id="2d25f-387">在轉換成運算式樹狀架構類型的匿名函式（[對運算式樹狀結構類型的匿名函數轉換評估](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)）內，會發生編譯時期 `M` 錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-387">It is a compile-time error to refer to `M` inside an anonymous function that is converted to an expression tree type ([Evaluation of anonymous function conversions to expression tree types](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).</span></span>
* <span data-ttu-id="2d25f-388">出現在 `M` 調用中的運算式不會影響明確指派狀態（[明確指派](variables.md#definite-assignment)），這可能會導致編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-388">Expressions occurring as part of an invocation of `M` do not affect the definite assignment state ([Definite assignment](variables.md#definite-assignment)), which can potentially lead to compile-time errors.</span></span>
* <span data-ttu-id="2d25f-389">`M` 不可以是應用程式的進入點（[應用程式啟動](basic-concepts.md#application-startup)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-389">`M` cannot be the entry point for an application ([Application Startup](basic-concepts.md#application-startup)).</span></span>

<span data-ttu-id="2d25f-390">部分方法可讓類型宣告的其中一個部分自訂另一個部分（例如，由工具產生的）的行為。</span><span class="sxs-lookup"><span data-stu-id="2d25f-390">Partial methods are useful for allowing one part of a type declaration to customize the behavior of another part, e.g., one that is generated by a tool.</span></span> <span data-ttu-id="2d25f-391">請考慮下列部分類別宣告：</span><span class="sxs-lookup"><span data-stu-id="2d25f-391">Consider the following partial class declaration:</span></span>
```csharp
partial class Customer
{
    string name;

    public string Name {
        get { return name; }
        set {
            OnNameChanging(value);
            name = value;
            OnNameChanged();
        }

    }

    partial void OnNameChanging(string newName);

    partial void OnNameChanged();
}
```

<span data-ttu-id="2d25f-392">如果編譯此類別時沒有任何其他元件，則會移除定義的部分方法宣告及其調用，而產生的合併類別宣告將相當於下列內容：</span><span class="sxs-lookup"><span data-stu-id="2d25f-392">If this class is compiled without any other parts, the defining partial method declarations and their invocations will be removed, and the resulting combined class declaration will be equivalent to the following:</span></span>
```csharp
class Customer
{
    string name;

    public string Name {
        get { return name; }
        set { name = value; }
    }
}
```

<span data-ttu-id="2d25f-393">假設有另一個部分是提供給部分方法的實作為宣告的：</span><span class="sxs-lookup"><span data-stu-id="2d25f-393">Assume that another part is given, however, which provides implementing declarations of the partial methods:</span></span>
```csharp
partial class Customer
{
    partial void OnNameChanging(string newName)
    {
        Console.WriteLine("Changing " + name + " to " + newName);
    }

    partial void OnNameChanged()
    {
        Console.WriteLine("Changed to " + name);
    }
}
```

<span data-ttu-id="2d25f-394">然後產生的合併類別宣告將相當於下列內容：</span><span class="sxs-lookup"><span data-stu-id="2d25f-394">Then the resulting combined class declaration will be equivalent to the following:</span></span>
```csharp
class Customer
{
    string name;

    public string Name {
        get { return name; }
        set {
            OnNameChanging(value);
            name = value;
            OnNameChanged();
        }

    }

    void OnNameChanging(string newName)
    {
        Console.WriteLine("Changing " + name + " to " + newName);
    }

    void OnNameChanged()
    {
        Console.WriteLine("Changed to " + name);
    }
}
```

### <a name="name-binding"></a><span data-ttu-id="2d25f-395">名稱系結</span><span class="sxs-lookup"><span data-stu-id="2d25f-395">Name binding</span></span>

<span data-ttu-id="2d25f-396">雖然可延伸類型的每個部分都必須在相同的命名空間內宣告，但這些元件通常會在不同的命名空間宣告中寫入。</span><span class="sxs-lookup"><span data-stu-id="2d25f-396">Although each part of an extensible type must be declared within the same namespace, the parts are typically written within different namespace declarations.</span></span> <span data-ttu-id="2d25f-397">因此，每個元件可能會有不同的 `using` 指示詞（[Using](namespaces.md#using-directives)指示詞）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-397">Thus, different `using` directives ([Using directives](namespaces.md#using-directives)) may be present for each part.</span></span> <span data-ttu-id="2d25f-398">在某個部分中解讀簡單名稱（[類型推斷](expressions.md#type-inference)）時，只會考慮包含該部分之命名空間宣告的 `using` 指示詞。</span><span class="sxs-lookup"><span data-stu-id="2d25f-398">When interpreting simple names ([Type inference](expressions.md#type-inference)) within one part, only the `using` directives of the namespace declaration(s) enclosing that part are considered.</span></span> <span data-ttu-id="2d25f-399">這可能會導致相同的識別碼在不同的部分具有不同的意義：</span><span class="sxs-lookup"><span data-stu-id="2d25f-399">This may result in the same identifier having different meanings in different parts:</span></span>
```csharp
namespace N
{
    using List = System.Collections.ArrayList;

    partial class A
    {
        List x;                // x has type System.Collections.ArrayList
    }
}

namespace N
{
    using List = Widgets.LinkedList;

    partial class A
    {
        List y;                // y has type Widgets.LinkedList
    }
}
```

## <a name="class-members"></a><span data-ttu-id="2d25f-400">類別成員</span><span class="sxs-lookup"><span data-stu-id="2d25f-400">Class members</span></span>

<span data-ttu-id="2d25f-401">類別的成員是由其*class_member_declaration*所引進的成員，以及繼承自直接基類的成員所組成。</span><span class="sxs-lookup"><span data-stu-id="2d25f-401">The members of a class consist of the members introduced by its *class_member_declaration*s and the members inherited from the direct base class.</span></span>

```antlr
class_member_declaration
    : constant_declaration
    | field_declaration
    | method_declaration
    | property_declaration
    | event_declaration
    | indexer_declaration
    | operator_declaration
    | constructor_declaration
    | destructor_declaration
    | static_constructor_declaration
    | type_declaration
    ;
```

<span data-ttu-id="2d25f-402">類別類型的成員會分成下列類別：</span><span class="sxs-lookup"><span data-stu-id="2d25f-402">The members of a class type are divided into the following categories:</span></span>

*  <span data-ttu-id="2d25f-403">常數，表示與類別（[常數](classes.md#constants)）相關聯的常數值。</span><span class="sxs-lookup"><span data-stu-id="2d25f-403">Constants, which represent constant values associated with the class ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="2d25f-404">欄位，也就是類別（[欄位](classes.md#fields)）的變數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-404">Fields, which are the variables of the class ([Fields](classes.md#fields)).</span></span>
*  <span data-ttu-id="2d25f-405">方法，其會執行類別（[方法](classes.md#methods)）可執行檔計算和動作。</span><span class="sxs-lookup"><span data-stu-id="2d25f-405">Methods, which implement the computations and actions that can be performed by the class ([Methods](classes.md#methods)).</span></span>
*  <span data-ttu-id="2d25f-406">屬性，定義已命名的特性，以及與讀取和寫入這些特性（[屬性](classes.md#properties)）相關聯的動作。</span><span class="sxs-lookup"><span data-stu-id="2d25f-406">Properties, which define named characteristics and the actions associated with reading and writing those characteristics ([Properties](classes.md#properties)).</span></span>
*  <span data-ttu-id="2d25f-407">定義可由類別（[事件](classes.md#events)）產生之通知的事件。</span><span class="sxs-lookup"><span data-stu-id="2d25f-407">Events, which define notifications that can be generated by the class ([Events](classes.md#events)).</span></span>
*  <span data-ttu-id="2d25f-408">索引子，允許以相同的方式（語法）將類別的實例編制索引為數組（[索引子](classes.md#indexers)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-408">Indexers, which permit instances of the class to be indexed in the same way (syntactically) as arrays ([Indexers](classes.md#indexers)).</span></span>
*  <span data-ttu-id="2d25f-409">運算子，定義可以套用至類別（[運算子](classes.md#operators)）實例的運算式運算子。</span><span class="sxs-lookup"><span data-stu-id="2d25f-409">Operators, which define the expression operators that can be applied to instances of the class ([Operators](classes.md#operators)).</span></span>
*  <span data-ttu-id="2d25f-410">實例函式，其會執行初始化類別的實例所需的動作（[實例構造](classes.md#instance-constructors)函式）</span><span class="sxs-lookup"><span data-stu-id="2d25f-410">Instance constructors, which implement the actions required to initialize instances of the class ([Instance constructors](classes.md#instance-constructors))</span></span>
*  <span data-ttu-id="2d25f-411">析構函數，其會執行要在類別的實例永久捨棄（[析構函數](classes.md#destructors)）之前執行的動作。</span><span class="sxs-lookup"><span data-stu-id="2d25f-411">Destructors, which implement the actions to be performed before instances of the class are permanently discarded ([Destructors](classes.md#destructors)).</span></span>
*  <span data-ttu-id="2d25f-412">靜態的函式，其會執行初始化類別本身所需的動作（[靜態](classes.md#static-constructors)的函式）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-412">Static constructors, which implement the actions required to initialize the class itself ([Static constructors](classes.md#static-constructors)).</span></span>
*  <span data-ttu-id="2d25f-413">類型，表示類別的本機類型（[巢狀型別](classes.md#nested-types)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-413">Types, which represent the types that are local to the class ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="2d25f-414">可以包含可執行程式碼的成員統稱為類別類型的函式*成員*。</span><span class="sxs-lookup"><span data-stu-id="2d25f-414">Members that can contain executable code are collectively known as the *function members* of the class type.</span></span> <span data-ttu-id="2d25f-415">類別類型的函式成員是該類別類型的方法、屬性、事件、索引子、運算子、實例的函式、析構函式和靜態函式。</span><span class="sxs-lookup"><span data-stu-id="2d25f-415">The function members of a class type are the methods, properties, events, indexers, operators, instance constructors,  destructors, and static constructors of that class type.</span></span>

<span data-ttu-id="2d25f-416">*Class_declaration*會建立新的宣告[空間（宣告](basic-concepts.md#declarations)），而*class_declaration*立即包含的*class_member_declaration*會將新的成員引進此宣告空間。</span><span class="sxs-lookup"><span data-stu-id="2d25f-416">A *class_declaration* creates a new declaration space ([Declarations](basic-concepts.md#declarations)), and the *class_member_declaration*s immediately contained by the *class_declaration* introduce new members into this declaration space.</span></span> <span data-ttu-id="2d25f-417">下列規則適用于*class_member_declaration*s：</span><span class="sxs-lookup"><span data-stu-id="2d25f-417">The following rules apply to *class_member_declaration*s:</span></span>

*  <span data-ttu-id="2d25f-418">實例的函式、析構函式和靜態的函式必須與直接封入類別的名稱相同。</span><span class="sxs-lookup"><span data-stu-id="2d25f-418">Instance constructors, destructors and static constructors must have the same name as the immediately enclosing class.</span></span> <span data-ttu-id="2d25f-419">所有其他成員的名稱都必須與立即封入類別的名稱不同。</span><span class="sxs-lookup"><span data-stu-id="2d25f-419">All other members must have names that differ from the name of the immediately enclosing class.</span></span>
*  <span data-ttu-id="2d25f-420">常數、欄位、屬性、事件或類型的名稱，必須與在相同類別中宣告的所有其他成員名稱不同。</span><span class="sxs-lookup"><span data-stu-id="2d25f-420">The name of a constant, field, property, event, or type must differ from the names of all other members declared in the same class.</span></span>
*  <span data-ttu-id="2d25f-421">方法的名稱必須與在相同類別中宣告之所有其他非方法的名稱不同。</span><span class="sxs-lookup"><span data-stu-id="2d25f-421">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="2d25f-422">此外，方法的簽章（簽章[和](basic-concepts.md#signatures-and-overloading)多載）必須與相同類別中宣告之所有其他方法的簽章不同，而且在相同類別中宣告的兩個方法，不能有不同于 `ref` 和 `out`的簽章。</span><span class="sxs-lookup"><span data-stu-id="2d25f-422">In addition, the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="2d25f-423">實例函式的簽章必須不同于相同類別中宣告之所有其他實例的簽章，而且在相同類別中宣告的兩個函式不能有不同于 `ref` 和 `out`的簽章。</span><span class="sxs-lookup"><span data-stu-id="2d25f-423">The signature of an instance constructor must differ from the signatures of all other instance constructors declared in the same class, and two constructors declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="2d25f-424">索引子的簽章必須與相同類別中宣告之所有其他索引子的簽章不同。</span><span class="sxs-lookup"><span data-stu-id="2d25f-424">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>
*  <span data-ttu-id="2d25f-425">運算子的簽章必須與在相同類別中宣告之所有其他運算子的簽章不同。</span><span class="sxs-lookup"><span data-stu-id="2d25f-425">The signature of an operator must differ from the signatures of all other operators declared in the same class.</span></span>

<span data-ttu-id="2d25f-426">類別類型的繼承成員（[繼承](classes.md#inheritance)）不是類別之宣告空間的一部分。</span><span class="sxs-lookup"><span data-stu-id="2d25f-426">The inherited members of a class type ([Inheritance](classes.md#inheritance)) are not part of the declaration space of a class.</span></span> <span data-ttu-id="2d25f-427">因此，衍生類別可以宣告與繼承成員具有相同名稱或簽章的成員（這實際上會隱藏繼承的成員）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-427">Thus, a derived class is allowed to declare a member with the same name or signature as an inherited member (which in effect hides the inherited member).</span></span>

### <a name="the-instance-type"></a><span data-ttu-id="2d25f-428">實例類型</span><span class="sxs-lookup"><span data-stu-id="2d25f-428">The instance type</span></span>

<span data-ttu-id="2d25f-429">每個類別宣告都有相關聯的系結類型（系結[和未](types.md#bound-and-unbound-types)系結類型），也就是***實例類型***。</span><span class="sxs-lookup"><span data-stu-id="2d25f-429">Each class declaration has an associated bound type ([Bound and unbound types](types.md#bound-and-unbound-types)), the ***instance type***.</span></span> <span data-ttu-id="2d25f-430">針對泛型類別宣告，實例類型的形成方式是從類型宣告建立結構化類型（[結構化類型](types.md#constructed-types)），其中每個提供的類型引數都是對應的類型參數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-430">For a generic class declaration, the instance type is formed by creating a constructed type ([Constructed types](types.md#constructed-types)) from the type declaration, with each of the supplied type arguments being the corresponding type parameter.</span></span> <span data-ttu-id="2d25f-431">由於實例型別會使用型別參數，因此只能在型別參數在範圍內時使用。也就是在類別宣告內。</span><span class="sxs-lookup"><span data-stu-id="2d25f-431">Since the instance type uses the type parameters, it can only be used where the type parameters are in scope; that is, inside the class declaration.</span></span> <span data-ttu-id="2d25f-432">實例類型是在類別宣告內撰寫之程式碼的 `this` 類型。</span><span class="sxs-lookup"><span data-stu-id="2d25f-432">The instance type is the type of `this` for code written inside the class declaration.</span></span> <span data-ttu-id="2d25f-433">針對非泛型類別，實例類型只是宣告的類別。</span><span class="sxs-lookup"><span data-stu-id="2d25f-433">For non-generic classes, the instance type is simply the declared class.</span></span> <span data-ttu-id="2d25f-434">以下顯示數個類別宣告以及其實例類型：</span><span class="sxs-lookup"><span data-stu-id="2d25f-434">The following shows several class declarations along with their instance types:</span></span> 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a><span data-ttu-id="2d25f-435">結構化類型的成員</span><span class="sxs-lookup"><span data-stu-id="2d25f-435">Members of constructed types</span></span>

<span data-ttu-id="2d25f-436">結構化類型的非繼承成員是藉由以成員宣告中的每個*type_parameter*的對應*type_argument*來取代。</span><span class="sxs-lookup"><span data-stu-id="2d25f-436">The non-inherited members of a constructed type are obtained by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="2d25f-437">替代程式是以類型宣告的語義意義為基礎，而不是單純的文字替代。</span><span class="sxs-lookup"><span data-stu-id="2d25f-437">The substitution process is based on the semantic meaning of type declarations, and is not simply textual substitution.</span></span>

<span data-ttu-id="2d25f-438">例如，假設泛型類別宣告</span><span class="sxs-lookup"><span data-stu-id="2d25f-438">For example, given the generic class declaration</span></span>
```csharp
class Gen<T,U>
{
    public T[,] a;
    public void G(int i, T t, Gen<U,T> gt) {...}
    public U Prop { get {...} set {...} }
    public int H(double d) {...}
}
```
<span data-ttu-id="2d25f-439">`Gen<int[],IComparable<string>>` 的結構化類型具有下列成員：</span><span class="sxs-lookup"><span data-stu-id="2d25f-439">the constructed type `Gen<int[],IComparable<string>>` has the following members:</span></span>
```csharp
public int[,][] a;
public void G(int i, int[] t, Gen<IComparable<string>,int[]> gt) {...}
public IComparable<string> Prop { get {...} set {...} }
public int H(double d) {...}
```

<span data-ttu-id="2d25f-440">泛型類別宣告中的成員 `a` 類型 `Gen` 為 "二維陣列 of `T`"，因此上述結構類型中的成員 `a` 類型為 "二維陣列（一維陣列 of `int`"）或 `int[,][]`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-440">The type of the member `a` in the generic class declaration `Gen` is "two-dimensional array of `T`", so the type of the member `a` in the constructed type above is "two-dimensional array of one-dimensional array of `int`", or `int[,][]`.</span></span>

<span data-ttu-id="2d25f-441">在實例函式成員內，`this` 的類型是包含宣告的實例類型（[實例類型](classes.md#the-instance-type)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-441">Within instance function members, the type of `this` is the instance type ([The instance type](classes.md#the-instance-type)) of the containing declaration.</span></span>

<span data-ttu-id="2d25f-442">泛型類別的所有成員都可以直接或做為結構化型別的一部分，使用任何封入類別中的型別參數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-442">All members of a generic class can use type parameters from any enclosing class, either directly or as part of a constructed type.</span></span> <span data-ttu-id="2d25f-443">在執行時間使用特定封閉結構類型（[開放式和封閉式類型](types.md#open-and-closed-types)）時，會將類型參數的每個使用，取代為提供給結構化類型的實際類型引數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-443">When a particular closed constructed type ([Open and closed types](types.md#open-and-closed-types)) is used at run-time, each use of a type parameter is replaced with the actual type argument supplied to the constructed type.</span></span> <span data-ttu-id="2d25f-444">例如：</span><span class="sxs-lookup"><span data-stu-id="2d25f-444">For example:</span></span>
```csharp
class C<V>
{
    public V f1;
    public C<V> f2 = null;

    public C(V x) {
        this.f1 = x;
        this.f2 = this;
    }
}

class Application
{
    static void Main() {
        C<int> x1 = new C<int>(1);
        Console.WriteLine(x1.f1);        // Prints 1

        C<double> x2 = new C<double>(3.1415);
        Console.WriteLine(x2.f1);        // Prints 3.1415
    }
}
```

### <a name="inheritance"></a><span data-ttu-id="2d25f-445">繼承</span><span class="sxs-lookup"><span data-stu-id="2d25f-445">Inheritance</span></span>

<span data-ttu-id="2d25f-446">類別會***繼承***其直接基類類型的成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-446">A class ***inherits*** the members of its direct base class type.</span></span> <span data-ttu-id="2d25f-447">繼承表示，類別會隱含包含其直接基類類型的所有成員，但不包括實例的函式、函式和基類的靜態方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-447">Inheritance means that a class implicitly contains all members of its direct base class type, except for the instance constructors, destructors and static constructors of the base class.</span></span> <span data-ttu-id="2d25f-448">繼承的一些重要層面如下：</span><span class="sxs-lookup"><span data-stu-id="2d25f-448">Some important aspects of inheritance are:</span></span>

*  <span data-ttu-id="2d25f-449">繼承是可轉移的。</span><span class="sxs-lookup"><span data-stu-id="2d25f-449">Inheritance is transitive.</span></span> <span data-ttu-id="2d25f-450">如果 `C` 衍生自 `B`，而 `B` 衍生自 `A`，則 `C` 會繼承 `B` 中宣告的成員，以及 `A`中宣告的成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-450">If `C` is derived from `B`, and `B` is derived from `A`, then `C` inherits the members declared in `B` as well as the members declared in `A`.</span></span>
*  <span data-ttu-id="2d25f-451">衍生類別會擴充其直接基類。</span><span class="sxs-lookup"><span data-stu-id="2d25f-451">A derived class extends its direct base class.</span></span> <span data-ttu-id="2d25f-452">衍生類別可以在其繼承的成員中新增新的成員，但無法移除所繼承成員的定義。</span><span class="sxs-lookup"><span data-stu-id="2d25f-452">A derived class can add new members to those it inherits, but it cannot remove the definition of an inherited member.</span></span>
*  <span data-ttu-id="2d25f-453">實例的函式、析構函式和靜態的函式不會繼承，但是所有其他成員都是，而不論其宣告的存取範圍為何（[成員存取權](basic-concepts.md#member-access)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-453">Instance constructors, destructors, and static constructors are not inherited, but all other members are, regardless of their declared accessibility ([Member access](basic-concepts.md#member-access)).</span></span> <span data-ttu-id="2d25f-454">不過，視其宣告的存取範圍而定，繼承的成員可能無法在衍生類別中存取。</span><span class="sxs-lookup"><span data-stu-id="2d25f-454">However, depending on their declared accessibility, inherited members might not be accessible in a derived class.</span></span>
*  <span data-ttu-id="2d25f-455">衍生類別可以藉由宣告具有相同名稱或簽章的新成員，***隱藏***（[透過繼承](basic-concepts.md#hiding-through-inheritance)）繼承的成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-455">A derived class can ***hide*** ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)) inherited members by declaring new members with the same name or signature.</span></span> <span data-ttu-id="2d25f-456">不過，請注意，隱藏繼承的成員並不會移除該成員，而只是讓該成員直接透過衍生類別來存取。</span><span class="sxs-lookup"><span data-stu-id="2d25f-456">Note however that hiding an inherited member does not remove that member—it merely makes that member inaccessible directly through the derived class.</span></span>
*  <span data-ttu-id="2d25f-457">類別的實例包含一組在類別及其基類中宣告的所有實例欄位，以及從衍生類別類型到其任何基類類型的隱含轉換（[隱含參考轉換](conversions.md#implicit-reference-conversions)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-457">An instance of a class contains a set of all instance fields declared in the class and its base classes, and an implicit conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from a derived class type to any of its base class types.</span></span> <span data-ttu-id="2d25f-458">因此，某些衍生類別之實例的參考可視為其任何基類之實例的參考。</span><span class="sxs-lookup"><span data-stu-id="2d25f-458">Thus, a reference to an instance of some derived class can be treated as a reference to an instance of any of its base classes.</span></span>
*  <span data-ttu-id="2d25f-459">類別可以宣告虛擬方法、屬性和索引子，而衍生類別可以覆寫這些函式成員的執行。</span><span class="sxs-lookup"><span data-stu-id="2d25f-459">A class can declare virtual methods, properties, and indexers, and derived classes can override the implementation of these function members.</span></span> <span data-ttu-id="2d25f-460">這可讓類別展示多型行為，其中由函式成員調用所執行的動作會根據叫用該函式成員之實例的執行時間類型而有所不同。</span><span class="sxs-lookup"><span data-stu-id="2d25f-460">This enables classes to exhibit polymorphic behavior wherein the actions performed by a function member invocation varies depending on the run-time type of the instance through which that function member is invoked.</span></span>

<span data-ttu-id="2d25f-461">已結構化類別類型的繼承成員是直接基底類別類型（[基類](classes.md#base-classes)）的成員，它是藉由以*class_base*規格中對應類型參數的每個出現專案的類型引數取代所找到的。</span><span class="sxs-lookup"><span data-stu-id="2d25f-461">The inherited member of a constructed class type are the members of the immediate base class type ([Base classes](classes.md#base-classes)), which is found by substituting the type arguments of the constructed type for each occurrence of the corresponding type parameters in the *class_base* specification.</span></span> <span data-ttu-id="2d25f-462">接著，這些成員會藉由替代成員宣告中的每個*type_parameter* （對應*type_argument* *class_base*規格）進行轉換。</span><span class="sxs-lookup"><span data-stu-id="2d25f-462">These members, in turn, are transformed by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the *class_base* specification.</span></span>

```csharp
class B<U>
{
    public U F(long index) {...}
}

class D<T>: B<T[]>
{
    public T G(string s) {...}
}
```

<span data-ttu-id="2d25f-463">在上述範例中，`D<int>` 的結構化型別有一個非繼承的成員 `public int G(string s)` 藉由替代型別參數 `T`的型別引數 `int` 取得。</span><span class="sxs-lookup"><span data-stu-id="2d25f-463">In the above example, the constructed type `D<int>` has a non-inherited member `public int G(string s)` obtained by substituting the type argument `int` for the type parameter `T`.</span></span> <span data-ttu-id="2d25f-464">`D<int>` 也有類別宣告 `B`繼承的成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-464">`D<int>` also has an inherited member from the class declaration `B`.</span></span> <span data-ttu-id="2d25f-465">這個繼承的成員是藉由在基類規格 `B<T[]>`中替換 `T` 的 `int`，來判斷 `D<int>` 的 `B<int[]>` 的基類型別。</span><span class="sxs-lookup"><span data-stu-id="2d25f-465">This inherited member is determined by first determining the base class type `B<int[]>` of `D<int>` by substituting `int` for `T` in the base class specification `B<T[]>`.</span></span> <span data-ttu-id="2d25f-466">然後，做為 `B`的型別引數，`int[]` 會在 `public U F(long index)`中用來取代 `U`，產生繼承的成員 `public int[] F(long index)`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-466">Then, as a type argument to `B`, `int[]` is substituted for `U` in `public U F(long index)`, yielding the inherited member `public int[] F(long index)`.</span></span>

### <a name="the-new-modifier"></a><span data-ttu-id="2d25f-467">New 修飾詞</span><span class="sxs-lookup"><span data-stu-id="2d25f-467">The new modifier</span></span>

<span data-ttu-id="2d25f-468">允許*class_member_declaration*宣告與繼承成員具有相同名稱或簽章的成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-468">A *class_member_declaration* is permitted to declare a member with the same name or signature as an inherited member.</span></span> <span data-ttu-id="2d25f-469">發生這種情況時，衍生的類別成員會被視為***隱藏***基類成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-469">When this occurs, the derived class member is said to ***hide*** the base class member.</span></span> <span data-ttu-id="2d25f-470">隱藏繼承的成員不會被視為錯誤，但會導致編譯器發出警告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-470">Hiding an inherited member is not considered an error, but it does cause the compiler to issue a warning.</span></span> <span data-ttu-id="2d25f-471">若要隱藏警告，衍生類別成員的宣告可以包含 `new` 修飾詞，以指出衍生的成員要用來隱藏基底成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-471">To suppress the warning, the declaration of the derived class member can include a `new` modifier to indicate that the derived member is intended to hide the base member.</span></span> <span data-ttu-id="2d25f-472">本主題會在[透過繼承隱藏](basic-concepts.md#hiding-through-inheritance)中進一步討論。</span><span class="sxs-lookup"><span data-stu-id="2d25f-472">This topic is discussed further in [Hiding through inheritance](basic-concepts.md#hiding-through-inheritance).</span></span>

<span data-ttu-id="2d25f-473">如果 `new` 修飾詞包含在不會隱藏繼承成員的宣告中，就會發出該效果的警告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-473">If a `new` modifier is included in a declaration that doesn't hide an inherited member, a warning to that effect is issued.</span></span> <span data-ttu-id="2d25f-474">移除 `new` 修飾詞會隱藏這個警告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-474">This warning is suppressed by removing the `new` modifier.</span></span>

### <a name="access-modifiers"></a><span data-ttu-id="2d25f-475">存取修飾詞</span><span class="sxs-lookup"><span data-stu-id="2d25f-475">Access modifiers</span></span>

<span data-ttu-id="2d25f-476">*Class_member_declaration*可以有五種可能的宣告存取範圍（宣告的[存取](basic-concepts.md#declared-accessibility)範圍）其中之一： `public`、`protected internal`、`protected`、`internal`或 `private`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-476">A *class_member_declaration* can have any one of the five possible kinds of declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal`, or `private`.</span></span> <span data-ttu-id="2d25f-477">除了 `protected internal` 組合之外，指定一個以上的存取修飾詞是編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-477">Except for the `protected internal` combination, it is a compile-time error to specify more than one access modifier.</span></span> <span data-ttu-id="2d25f-478">當*class_member_declaration*不包含任何存取修飾詞時，就會假設 `private`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-478">When a *class_member_declaration* does not include any access modifiers, `private` is assumed.</span></span>

### <a name="constituent-types"></a><span data-ttu-id="2d25f-479">組成類型</span><span class="sxs-lookup"><span data-stu-id="2d25f-479">Constituent types</span></span>

<span data-ttu-id="2d25f-480">成員宣告中使用的類型稱為該成員的構成類型。</span><span class="sxs-lookup"><span data-stu-id="2d25f-480">Types that are used in the declaration of a member are called the constituent types of that member.</span></span> <span data-ttu-id="2d25f-481">可能的構成類型為常數、欄位、屬性、事件或索引子的類型、方法或運算子的傳回類型，以及方法、索引子、運算子或實例的參數類型。</span><span class="sxs-lookup"><span data-stu-id="2d25f-481">Possible constituent types are the type of a constant, field, property, event, or indexer, the return type of a method or operator, and the parameter types of a method, indexer, operator, or instance constructor.</span></span> <span data-ttu-id="2d25f-482">成員的構成類型必須至少與該成員本身一樣可以存取（[存取範圍條件約束](basic-concepts.md#accessibility-constraints)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-482">The constituent types of a member must be at least as accessible as that member itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

### <a name="static-and-instance-members"></a><span data-ttu-id="2d25f-483">靜態和實例成員</span><span class="sxs-lookup"><span data-stu-id="2d25f-483">Static and instance members</span></span>

<span data-ttu-id="2d25f-484">類別的成員可能是***靜態成員***或***實例成員***。</span><span class="sxs-lookup"><span data-stu-id="2d25f-484">Members of a class are either ***static members*** or ***instance members***.</span></span> <span data-ttu-id="2d25f-485">一般來說，將靜態成員視為屬於物件的類別型別和實例成員（類別型別的實例）會很有説明。</span><span class="sxs-lookup"><span data-stu-id="2d25f-485">Generally speaking, it is useful to think of static members as belonging to class types and instance members as belonging to objects (instances of class types).</span></span>

<span data-ttu-id="2d25f-486">當欄位、方法、屬性、事件、運算子或函式宣告包含 `static` 修飾詞時，它會宣告靜態成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-486">When a field, method, property, event, operator, or constructor declaration includes a `static` modifier, it declares a static member.</span></span> <span data-ttu-id="2d25f-487">此外，常數或型別宣告會隱含宣告靜態成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-487">In addition, a constant or type declaration implicitly declares a static member.</span></span> <span data-ttu-id="2d25f-488">靜態成員具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="2d25f-488">Static members have the following characteristics:</span></span>

*  <span data-ttu-id="2d25f-489">當靜態成員 `M` 在表單 `E.M`的*member_access* （[成員存取](expressions.md#member-access)）中被參考時，`E` 必須代表包含 `M`的類型。</span><span class="sxs-lookup"><span data-stu-id="2d25f-489">When a static member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote a type containing `M`.</span></span> <span data-ttu-id="2d25f-490">`E` 表示實例時，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-490">It is a compile-time error for `E` to denote an instance.</span></span>
*  <span data-ttu-id="2d25f-491">靜態欄位可識別指定之封閉式類別類型的所有實例所要共用的一個儲存位置。</span><span class="sxs-lookup"><span data-stu-id="2d25f-491">A static field identifies exactly one storage location to be shared by all instances of a given closed class type.</span></span> <span data-ttu-id="2d25f-492">不論指定之封閉式類別類型的多少個實例已建立，只有一個靜態欄位的複本。</span><span class="sxs-lookup"><span data-stu-id="2d25f-492">No matter how many instances of a given closed class type are created, there is only ever one copy of a static field.</span></span>
*  <span data-ttu-id="2d25f-493">靜態函式成員（方法、屬性、事件、運算子或函式）不會在特定的實例上運作，而且在這類函式成員中參考 `this` 時，會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-493">A static function member (method, property, event, operator, or constructor) does not operate on a specific instance, and it is a compile-time error to refer to `this` in such a function member.</span></span>

<span data-ttu-id="2d25f-494">當欄位、方法、屬性、事件、索引子、函數或析構函式宣告不包含 `static` 修飾詞時，它會宣告實例成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-494">When a field, method, property, event, indexer, constructor, or destructor declaration does not include a `static` modifier, it declares an instance member.</span></span> <span data-ttu-id="2d25f-495">（實例成員有時稱為非靜態成員）。實例成員具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="2d25f-495">(An instance member is sometimes called a non-static member.) Instance members have the following characteristics:</span></span>

*  <span data-ttu-id="2d25f-496">當實例成員 `M` 在表單 `E.M`的*member_access* （[成員存取](expressions.md#member-access)）中被參考時，`E` 必須代表包含 `M`之類型的實例。</span><span class="sxs-lookup"><span data-stu-id="2d25f-496">When an instance member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote an instance of a type containing `M`.</span></span> <span data-ttu-id="2d25f-497">這是 `E` 用來表示類型的系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-497">It is a binding-time error for `E` to denote a type.</span></span>
*  <span data-ttu-id="2d25f-498">類別的每個實例都包含一組個別的類別的實例欄位。</span><span class="sxs-lookup"><span data-stu-id="2d25f-498">Every instance of a class contains a separate set of all instance fields of the class.</span></span>
*  <span data-ttu-id="2d25f-499">實例函式成員（方法、屬性、索引子、實例處理常式或析構函數）會在指定的類別實例上運作，而且這個實例可以當做 `this` （[這項存取](expressions.md#this-access)）來存取。</span><span class="sxs-lookup"><span data-stu-id="2d25f-499">An instance function member (method, property, indexer, instance constructor, or destructor) operates on a given instance of the class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="2d25f-500">下列範例說明用來存取靜態和實例成員的規則：</span><span class="sxs-lookup"><span data-stu-id="2d25f-500">The following example illustrates the rules for accessing static and instance members:</span></span>
```csharp
class Test
{
    int x;
    static int y;

    void F() {
        x = 1;            // Ok, same as this.x = 1
        y = 1;            // Ok, same as Test.y = 1
    }

    static void G() {
        x = 1;            // Error, cannot access this.x
        y = 1;            // Ok, same as Test.y = 1
    }

    static void Main() {
        Test t = new Test();
        t.x = 1;          // Ok
        t.y = 1;          // Error, cannot access static member through instance
        Test.x = 1;       // Error, cannot access instance member through type
        Test.y = 1;       // Ok
    }
}
```

<span data-ttu-id="2d25f-501">`F` 的方法顯示，在實例函式成員中，可以使用*simple_name* （[簡單名稱](expressions.md#simple-names)）來存取實例成員和靜態成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-501">The `F` method shows that in an instance function member, a *simple_name* ([Simple names](expressions.md#simple-names)) can be used to access both instance members and static members.</span></span> <span data-ttu-id="2d25f-502">`G` 方法顯示，在靜態函式成員中，這是編譯時期錯誤，透過*simple_name*存取實例成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-502">The `G` method shows that in a static function member, it is a compile-time error to access an instance member through a *simple_name*.</span></span> <span data-ttu-id="2d25f-503">`Main` 方法會顯示在*member_access* （[成員存取](expressions.md#member-access)）中，實例成員必須透過實例存取，而且靜態成員必須透過類型存取。</span><span class="sxs-lookup"><span data-stu-id="2d25f-503">The `Main` method shows that in a *member_access* ([Member access](expressions.md#member-access)), instance members must be accessed through instances, and static members must be accessed through types.</span></span>

### <a name="nested-types"></a><span data-ttu-id="2d25f-504">巢狀型別</span><span class="sxs-lookup"><span data-stu-id="2d25f-504">Nested types</span></span>

<span data-ttu-id="2d25f-505">在類別或結構宣告內宣告的型別稱為「***嵌套型***別」。</span><span class="sxs-lookup"><span data-stu-id="2d25f-505">A type declared within a class or struct declaration is called a ***nested type***.</span></span> <span data-ttu-id="2d25f-506">在編譯單位或命名空間中宣告的類型稱為***非巢狀型別***。</span><span class="sxs-lookup"><span data-stu-id="2d25f-506">A type that is declared within a compilation unit or namespace is called a ***non-nested type***.</span></span>

<span data-ttu-id="2d25f-507">在範例中</span><span class="sxs-lookup"><span data-stu-id="2d25f-507">In the example</span></span>
```csharp
using System;

class A
{
    class B
    {
        static void F() {
            Console.WriteLine("A.B.F");
        }
    }
}
```
<span data-ttu-id="2d25f-508">類別 `B` 是一個嵌套型別，因為它是在類別 `A`內宣告，而類別 `A` 是非嵌套型別，因為它是在編譯單位內宣告的。</span><span class="sxs-lookup"><span data-stu-id="2d25f-508">class `B` is a nested type because it is declared within class `A`, and class `A` is a non-nested type because it is declared within a compilation unit.</span></span>

#### <a name="fully-qualified-name"></a><span data-ttu-id="2d25f-509">完整名稱</span><span class="sxs-lookup"><span data-stu-id="2d25f-509">Fully qualified name</span></span>

<span data-ttu-id="2d25f-510">巢狀型別的完整限定名稱（[完整](basic-concepts.md#fully-qualified-names)名稱）為 `S.N`，其中 `S` 是宣告類型 `N` 之類型的完整名稱。</span><span class="sxs-lookup"><span data-stu-id="2d25f-510">The fully qualified name ([Fully qualified names](basic-concepts.md#fully-qualified-names)) for a nested type is `S.N` where `S` is the fully qualified name of the type in which type `N` is declared.</span></span>

#### <a name="declared-accessibility"></a><span data-ttu-id="2d25f-511">已宣告存取範圍</span><span class="sxs-lookup"><span data-stu-id="2d25f-511">Declared accessibility</span></span>

<span data-ttu-id="2d25f-512">非巢狀型別可以有 `public` 或 `internal` 宣告的存取範圍，而且預設會 `internal` 宣告存取範圍。</span><span class="sxs-lookup"><span data-stu-id="2d25f-512">Non-nested types can have `public` or `internal` declared accessibility and have `internal` declared accessibility by default.</span></span> <span data-ttu-id="2d25f-513">巢狀型別也可以有這些形式的宣告存取範圍，再加上一或多個其他形式的宣告存取範圍，視包含的類型是否為類別或結構而定：</span><span class="sxs-lookup"><span data-stu-id="2d25f-513">Nested types can have these forms of declared accessibility too, plus one or more additional forms of declared accessibility, depending on whether the containing type is a class or struct:</span></span>

*  <span data-ttu-id="2d25f-514">在類別中宣告的巢狀型別可以有五種形式的宣告存取範圍（`public`、`protected internal`、`protected`、`internal`或 `private`），而且就像其他類別成員一樣，預設為 `private` 宣告的存取範圍。</span><span class="sxs-lookup"><span data-stu-id="2d25f-514">A nested type that is declared in a class can have any of five forms of declared accessibility (`public`, `protected internal`, `protected`, `internal`, or `private`) and, like other class members, defaults to `private` declared accessibility.</span></span>
*  <span data-ttu-id="2d25f-515">在結構中宣告的巢狀型別可以有三種形式的宣告存取範圍（`public`、`internal`或 `private`），而且就像其他結構成員一樣，預設為 `private` 宣告的存取範圍。</span><span class="sxs-lookup"><span data-stu-id="2d25f-515">A nested type that is declared in a struct can have any of three forms of declared accessibility (`public`, `internal`, or `private`) and, like other struct members, defaults to `private` declared accessibility.</span></span>

<span data-ttu-id="2d25f-516">範例</span><span class="sxs-lookup"><span data-stu-id="2d25f-516">The example</span></span>
```csharp
public class List
{
    // Private data structure
    private class Node
    { 
        public object Data;
        public Node Next;

        public Node(object data, Node next) {
            this.Data = data;
            this.Next = next;
        }
    }

    private Node first = null;
    private Node last = null;

    // Public interface
    public void AddToFront(object o) {...}
    public void AddToBack(object o) {...}
    public object RemoveFromFront() {...}
    public object RemoveFromBack() {...}
    public int Count { get {...} }
}
```
<span data-ttu-id="2d25f-517">宣告私用的嵌套類別 `Node`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-517">declares a private nested class `Node`.</span></span>

#### <a name="hiding"></a><span data-ttu-id="2d25f-518">起來</span><span class="sxs-lookup"><span data-stu-id="2d25f-518">Hiding</span></span>

<span data-ttu-id="2d25f-519">巢狀型別可能會隱藏（[名稱隱藏](basic-concepts.md#name-hiding)）基底成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-519">A nested type may hide ([Name hiding](basic-concepts.md#name-hiding)) a base member.</span></span> <span data-ttu-id="2d25f-520">在 nested 類型宣告上允許使用 `new` 修飾詞，因此可以明確表示隱藏。</span><span class="sxs-lookup"><span data-stu-id="2d25f-520">The `new` modifier is permitted on nested type declarations so that hiding can be expressed explicitly.</span></span> <span data-ttu-id="2d25f-521">範例</span><span class="sxs-lookup"><span data-stu-id="2d25f-521">The example</span></span>
```csharp
using System;

class Base
{
    public static void M() {
        Console.WriteLine("Base.M");
    }
}

class Derived: Base 
{
    new public class M 
    {
        public static void F() {
            Console.WriteLine("Derived.M.F");
        }
    }
}

class Test 
{
    static void Main() {
        Derived.M.F();
    }
}
```
<span data-ttu-id="2d25f-522">顯示隱藏 `Base`中所定義之方法 `M` 的嵌套類別 `M`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-522">shows a nested class `M` that hides the method `M` defined in `Base`.</span></span>

#### <a name="this-access"></a><span data-ttu-id="2d25f-523">此存取權</span><span class="sxs-lookup"><span data-stu-id="2d25f-523">this access</span></span>

<span data-ttu-id="2d25f-524">嵌套型別及其包含型別對於*this_access* （[此存取權](expressions.md#this-access)）沒有特殊關聯性。</span><span class="sxs-lookup"><span data-stu-id="2d25f-524">A nested type and its containing type do not have a special relationship with regard to *this_access* ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="2d25f-525">具體而言，巢狀型別中的 `this` 不能用來參考包含類型的實例成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-525">Specifically, `this` within a nested type cannot be used to refer to instance members of the containing type.</span></span> <span data-ttu-id="2d25f-526">當巢狀型別需要存取其包含類型的實例成員時，可以提供包含類型實例的 `this` 做為巢狀型別的函式引數，藉以提供存取權。</span><span class="sxs-lookup"><span data-stu-id="2d25f-526">In cases where a nested type needs access to the instance members of its containing type, access can be provided by providing the `this` for the instance of the containing type as a constructor argument for the nested type.</span></span> <span data-ttu-id="2d25f-527">下列範例</span><span class="sxs-lookup"><span data-stu-id="2d25f-527">The following example</span></span>
```csharp
using System;

class C
{
    int i = 123;

    public void F() {
        Nested n = new Nested(this);
        n.G();
    }

    public class Nested
    {
        C this_c;

        public Nested(C c) {
            this_c = c;
        }

        public void G() {
            Console.WriteLine(this_c.i);
        }
    }
}

class Test
{
    static void Main() {
        C c = new C();
        c.F();
    }
}
```
<span data-ttu-id="2d25f-528">顯示這項技術。</span><span class="sxs-lookup"><span data-stu-id="2d25f-528">shows this technique.</span></span> <span data-ttu-id="2d25f-529">實例 `C` 會建立 `Nested` 的實例，並將它自己的 `this` 傳遞至 `Nested`的函式，以便提供對 `C`實例成員的後續存取。</span><span class="sxs-lookup"><span data-stu-id="2d25f-529">An instance of `C` creates an instance of `Nested` and passes its own `this` to `Nested`'s constructor in order to provide subsequent access to `C`'s instance members.</span></span>

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a><span data-ttu-id="2d25f-530">存取包含類型的私用和受保護成員</span><span class="sxs-lookup"><span data-stu-id="2d25f-530">Access to private and protected members of the containing type</span></span>

<span data-ttu-id="2d25f-531">巢狀型別可以存取其包含類型可存取的所有成員，包括具有 `private` 和 `protected` 宣告存取範圍之包含類型的成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-531">A nested type has access to all of the members that are accessible to its containing type, including members of the containing type that have `private` and `protected` declared accessibility.</span></span> <span data-ttu-id="2d25f-532">範例</span><span class="sxs-lookup"><span data-stu-id="2d25f-532">The example</span></span>
```csharp
using System;

class C 
{
    private static void F() {
        Console.WriteLine("C.F");
    }

    public class Nested 
    {
        public static void G() {
            F();
        }
    }
}

class Test 
{
    static void Main() {
        C.Nested.G();
    }
}
```
<span data-ttu-id="2d25f-533">顯示包含 `Nested`之嵌套類別 `C` 的類別。</span><span class="sxs-lookup"><span data-stu-id="2d25f-533">shows a class `C` that contains a nested class `Nested`.</span></span> <span data-ttu-id="2d25f-534">在 `Nested`中，方法 `G` 會呼叫 `C`中定義的靜態方法 `F`，而 `F` 具有私用的可存取性。</span><span class="sxs-lookup"><span data-stu-id="2d25f-534">Within `Nested`, the method `G` calls the static method `F` defined in `C`, and `F` has private declared accessibility.</span></span>

<span data-ttu-id="2d25f-535">嵌套型別也可以存取其包含型別之基底型別中所定義的受保護成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-535">A nested type also may access protected members defined in a base type of its containing type.</span></span> <span data-ttu-id="2d25f-536">在範例中</span><span class="sxs-lookup"><span data-stu-id="2d25f-536">In the example</span></span>
```csharp
using System;

class Base 
{
    protected void F() {
        Console.WriteLine("Base.F");
    }
}

class Derived: Base 
{
    public class Nested 
    {
        public void G() {
            Derived d = new Derived();
            d.F();        // ok
        }
    }
}

class Test 
{
    static void Main() {
        Derived.Nested n = new Derived.Nested();
        n.G();
    }
}
```
<span data-ttu-id="2d25f-537">此嵌套類別 `Derived.Nested` 透過 `Derived`的實例呼叫，來存取受保護的方法 `F` `Derived`的基類中定義的 `Base`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-537">the nested class `Derived.Nested` accesses the protected method `F` defined in `Derived`'s base class, `Base`, by calling through an instance of `Derived`.</span></span>

#### <a name="nested-types-in-generic-classes"></a><span data-ttu-id="2d25f-538">泛型類別中的巢狀型別</span><span class="sxs-lookup"><span data-stu-id="2d25f-538">Nested types in generic classes</span></span>

<span data-ttu-id="2d25f-539">泛型類別宣告可以包含嵌套的類型宣告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-539">A generic class declaration can contain nested type declarations.</span></span> <span data-ttu-id="2d25f-540">封入類別的型別參數可以在巢狀型別中使用。</span><span class="sxs-lookup"><span data-stu-id="2d25f-540">The type parameters of the enclosing class can be used within the nested types.</span></span> <span data-ttu-id="2d25f-541">巢狀型別宣告可以包含僅適用于巢狀型別的其他類型參數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-541">A nested type declaration can contain additional type parameters that apply only to the nested type.</span></span>

<span data-ttu-id="2d25f-542">泛型類別宣告中包含的每個類型宣告，都是隱含的泛型型別宣告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-542">Every type declaration contained within a generic class declaration is implicitly a generic type declaration.</span></span> <span data-ttu-id="2d25f-543">在泛型型別中撰寫類型的參考時，包含結構化型別（包括它的型別引數）必須命名為。</span><span class="sxs-lookup"><span data-stu-id="2d25f-543">When writing a reference to a type nested within a generic type, the containing constructed type, including its type arguments, must be named.</span></span> <span data-ttu-id="2d25f-544">不過，在外部類別中，可以使用嵌套型別，而不需要限定。在建立嵌套的類型時，可以隱含地使用外部類別的實例類型。</span><span class="sxs-lookup"><span data-stu-id="2d25f-544">However, from within the outer class, the nested type can be used without qualification; the instance type of the outer class can be implicitly used when constructing the nested type.</span></span> <span data-ttu-id="2d25f-545">下列範例顯示三個不同的正確方式，以參考從 `Inner`建立的結構化型別。前兩個是相等的：</span><span class="sxs-lookup"><span data-stu-id="2d25f-545">The following example shows three different correct ways to refer to a constructed type created from `Inner`; the first two are equivalent:</span></span>
```csharp
class Outer<T>
{
    class Inner<U>
    {
        public static void F(T t, U u) {...}
    }

    static void F(T t) {
        Outer<T>.Inner<string>.F(t, "abc");      // These two statements have
        Inner<string>.F(t, "abc");               // the same effect

        Outer<int>.Inner<string>.F(3, "abc");    // This type is different

        Outer.Inner<string>.F(t, "abc");         // Error, Outer needs type arg
    }
}
```

<span data-ttu-id="2d25f-546">雖然這是不正確的程式設計樣式，但嵌套型別中的型別參數可以隱藏在外部型別中宣告的成員或型別參數：</span><span class="sxs-lookup"><span data-stu-id="2d25f-546">Although it is bad programming style, a type parameter in a nested type can hide a member or type parameter declared in the outer type:</span></span>
```csharp
class Outer<T>
{
    class Inner<T>        // Valid, hides Outer's T
    {
        public T t;       // Refers to Inner's T
    }
}
```

### <a name="reserved-member-names"></a><span data-ttu-id="2d25f-547">保留的成員名稱</span><span class="sxs-lookup"><span data-stu-id="2d25f-547">Reserved member names</span></span>

<span data-ttu-id="2d25f-548">為了加速基礎C#執行時間的執行，針對屬於屬性、事件或索引子的每個來源成員宣告，此執行必須根據成員宣告的類型、其名稱和類型來保留兩個方法簽章。</span><span class="sxs-lookup"><span data-stu-id="2d25f-548">To facilitate the underlying C# run-time implementation, for each source member declaration that is a property, event, or indexer, the implementation must reserve two method signatures based on the kind of the member declaration, its name, and its type.</span></span> <span data-ttu-id="2d25f-549">這是一個編譯時期錯誤，讓程式宣告其簽章符合其中一個保留簽章的成員，即使基礎執行時間執行不會使用這些保留區。</span><span class="sxs-lookup"><span data-stu-id="2d25f-549">It is a compile-time error for a program to declare a member whose signature matches one of these reserved signatures, even if the underlying run-time implementation does not make use of these reservations.</span></span>

<span data-ttu-id="2d25f-550">保留的名稱不會引進宣告，因此不會參與成員查閱。</span><span class="sxs-lookup"><span data-stu-id="2d25f-550">The reserved names do not introduce declarations, thus they do not participate in member lookup.</span></span> <span data-ttu-id="2d25f-551">不過，宣告相關聯的保留方法簽章會參與繼承（[繼承](classes.md#inheritance)），而且可以使用 `new` 修飾詞（[新的修飾](classes.md#the-new-modifier)詞）加以隱藏。</span><span class="sxs-lookup"><span data-stu-id="2d25f-551">However, a declaration's associated reserved method signatures do participate in inheritance ([Inheritance](classes.md#inheritance)), and can be hidden with the `new` modifier ([The new modifier](classes.md#the-new-modifier)).</span></span>

<span data-ttu-id="2d25f-552">這些名稱的保留有三個用途：</span><span class="sxs-lookup"><span data-stu-id="2d25f-552">The reservation of these names serves three purposes:</span></span>

*  <span data-ttu-id="2d25f-553">允許基礎執行程式使用一般識別碼作為方法名稱，以取得或設定C#語言功能的存取權。</span><span class="sxs-lookup"><span data-stu-id="2d25f-553">To allow the underlying implementation to use an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="2d25f-554">若要允許其他語言互通，請使用一般識別碼做為 get 或 set 存取C#語言功能的方法名稱。</span><span class="sxs-lookup"><span data-stu-id="2d25f-554">To allow other languages to interoperate using an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="2d25f-555">為了協助確保另一個符合的編譯器所接受的來源可被其他人接受，方法是讓保留成員名稱的細節在C#所有的執行中保持一致。</span><span class="sxs-lookup"><span data-stu-id="2d25f-555">To help ensure that the source accepted by one conforming compiler is accepted by another, by making the specifics of reserved member names consistent across all C# implementations.</span></span>

<span data-ttu-id="2d25f-556">「析構函數」[（析構函式）的](classes.md#destructors)宣告也會保留簽章（[針對析構函數保留的成員名稱](classes.md#member-names-reserved-for-destructors)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-556">The declaration of a destructor ([Destructors](classes.md#destructors)) also causes a signature to be reserved ([Member names reserved for destructors](classes.md#member-names-reserved-for-destructors)).</span></span>

#### <a name="member-names-reserved-for-properties"></a><span data-ttu-id="2d25f-557">保留給屬性的成員名稱</span><span class="sxs-lookup"><span data-stu-id="2d25f-557">Member names reserved for properties</span></span>

<span data-ttu-id="2d25f-558">若是類型`P` 的屬性（[屬性](classes.md#properties)），則會保留下列簽`T`章：</span><span class="sxs-lookup"><span data-stu-id="2d25f-558">For a property `P` ([Properties](classes.md#properties)) of type `T`, the following signatures are reserved:</span></span>

```csharp
T get_P();
void set_P(T value);
```

<span data-ttu-id="2d25f-559">這兩個簽章都會保留，即使屬性是唯讀或僅限寫入。</span><span class="sxs-lookup"><span data-stu-id="2d25f-559">Both signatures are reserved, even if the property is read-only or write-only.</span></span>

<span data-ttu-id="2d25f-560">在範例中</span><span class="sxs-lookup"><span data-stu-id="2d25f-560">In the example</span></span>
```csharp
using System;

class A
{
    public int P {
        get { return 123; }
    }
}

class B: A
{
    new public int get_P() {
        return 456;
    }

    new public void set_P(int value) {
    }
}

class Test
{
    static void Main() {
        B b = new B();
        A a = b;
        Console.WriteLine(a.P);
        Console.WriteLine(b.P);
        Console.WriteLine(b.get_P());
    }
}
```
<span data-ttu-id="2d25f-561">類別 `A` 定義 `P`的唯讀屬性，因此會保留 `get_P` 和 `set_P` 方法的簽章。</span><span class="sxs-lookup"><span data-stu-id="2d25f-561">a class `A` defines a read-only property `P`, thus reserving signatures for `get_P` and `set_P` methods.</span></span> <span data-ttu-id="2d25f-562">類別 `B` 衍生自 `A`，並隱藏這兩個保留的簽章。</span><span class="sxs-lookup"><span data-stu-id="2d25f-562">A class `B` derives from `A` and hides both of these reserved signatures.</span></span> <span data-ttu-id="2d25f-563">此範例會產生輸出：</span><span class="sxs-lookup"><span data-stu-id="2d25f-563">The example produces the output:</span></span>
```console
123
123
456
```

#### <a name="member-names-reserved-for-events"></a><span data-ttu-id="2d25f-564">保留給事件的成員名稱</span><span class="sxs-lookup"><span data-stu-id="2d25f-564">Member names reserved for events</span></span>

<span data-ttu-id="2d25f-565">若為委派`E`類型 [的事件（](classes.md#events)事件`T`），則會保留下列簽章：</span><span class="sxs-lookup"><span data-stu-id="2d25f-565">For an event `E` ([Events](classes.md#events)) of delegate type `T`, the following signatures are reserved:</span></span>
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a><span data-ttu-id="2d25f-566">保留給索引子的成員名稱</span><span class="sxs-lookup"><span data-stu-id="2d25f-566">Member names reserved for indexers</span></span>

<span data-ttu-id="2d25f-567">針對類型 `T` 的索引子（[索引子](classes.md#indexers)）與參數清單 `L`，會保留下列簽章：</span><span class="sxs-lookup"><span data-stu-id="2d25f-567">For an indexer ([Indexers](classes.md#indexers)) of type `T` with parameter-list `L`, the following signatures are reserved:</span></span>
```csharp
T get_Item(L);
void set_Item(L, T value);
```

<span data-ttu-id="2d25f-568">這兩個簽章都會保留，即使索引子是唯讀或僅限寫入。</span><span class="sxs-lookup"><span data-stu-id="2d25f-568">Both signatures are reserved, even if the indexer is read-only or write-only.</span></span>

<span data-ttu-id="2d25f-569">此外，`Item` 的成員名稱是保留的。</span><span class="sxs-lookup"><span data-stu-id="2d25f-569">Furthermore the member name `Item` is reserved.</span></span>

#### <a name="member-names-reserved-for-destructors"></a><span data-ttu-id="2d25f-570">保留給析構函數的成員名稱</span><span class="sxs-lookup"><span data-stu-id="2d25f-570">Member names reserved for destructors</span></span>

<span data-ttu-id="2d25f-571">對於包含「析構函數」（[析構](classes.md#destructors)函式）的類別，會保留下列簽章：</span><span class="sxs-lookup"><span data-stu-id="2d25f-571">For a class containing a destructor ([Destructors](classes.md#destructors)), the following signature is reserved:</span></span>
```csharp
void Finalize();
```

## <a name="constants"></a><span data-ttu-id="2d25f-572">常數</span><span class="sxs-lookup"><span data-stu-id="2d25f-572">Constants</span></span>

<span data-ttu-id="2d25f-573">***常數***是代表常數值的類別成員：可以在編譯時期計算的值。</span><span class="sxs-lookup"><span data-stu-id="2d25f-573">A ***constant*** is a class member that represents a constant value: a value that can be computed at compile-time.</span></span> <span data-ttu-id="2d25f-574">*Constant_declaration*導入了一或多個指定類型的常數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-574">A *constant_declaration* introduces one or more constants of a given type.</span></span>

```antlr
constant_declaration
    : attributes? constant_modifier* 'const' type constant_declarators ';'
    ;

constant_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;

constant_declarators
    : constant_declarator (',' constant_declarator)*
    ;

constant_declarator
    : identifier '=' constant_expression
    ;
```

<span data-ttu-id="2d25f-575">*Constant_declaration*可能包括一組*屬性*（attribute）、`new` 修飾[詞（](attributes.md)新的[修飾](classes.md#the-new-modifier)詞），以及四個存取修飾詞（[存取](classes.md#access-modifiers)修飾詞）的有效組合。</span><span class="sxs-lookup"><span data-stu-id="2d25f-575">A *constant_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span> <span data-ttu-id="2d25f-576">屬性和修飾詞適用于*constant_declaration*所宣告的所有成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-576">The attributes and modifiers apply to all of the members declared by the *constant_declaration*.</span></span> <span data-ttu-id="2d25f-577">即使常數視為靜態成員， *constant_declaration*都不需要也不允許 `static` 修飾詞。</span><span class="sxs-lookup"><span data-stu-id="2d25f-577">Even though constants are considered static members, a *constant_declaration* neither requires nor allows a `static` modifier.</span></span> <span data-ttu-id="2d25f-578">在常數宣告中多次出現相同的修飾詞時，就會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-578">It is an error for the same modifier to appear multiple times in a constant declaration.</span></span>

<span data-ttu-id="2d25f-579">*Constant_declaration*的*類型*會指定宣告引進的成員類型。</span><span class="sxs-lookup"><span data-stu-id="2d25f-579">The *type* of a *constant_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="2d25f-580">類型後面會接著*constant_declarator*的清單，其中每個都會引進新的成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-580">The type is followed by a list of *constant_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="2d25f-581">*Constant_declarator*包含命名成員的*識別碼*，後面接著 "`=`" token，接著是提供成員值的*constant_expression* （[常數運算式](expressions.md#constant-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-581">A *constant_declarator* consists of an *identifier* that names the member, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the member.</span></span>

<span data-ttu-id="2d25f-582">在常數宣告中指定的*類型*必須是 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`、`decimal`、`bool`、`string`、 *enum_type*或*reference_type*。</span><span class="sxs-lookup"><span data-stu-id="2d25f-582">The *type* specified in a constant declaration must be `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`, an *enum_type*, or a *reference_type*.</span></span> <span data-ttu-id="2d25f-583">每個*constant_expression*都必須產生目標型別的值，或可以透過隱含轉換（[隱含](conversions.md#implicit-conversions)轉換）轉換成目標型別的類型。</span><span class="sxs-lookup"><span data-stu-id="2d25f-583">Each *constant_expression* must yield a value of the target type or of a type that can be converted to the target type by an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>

<span data-ttu-id="2d25f-584">常數的*類型*至少必須與常數本身一樣可以存取（[存取範圍條件約束](basic-concepts.md#accessibility-constraints)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-584">The *type* of a constant must be at least as accessible as the constant itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="2d25f-585">您可使用*simple_name* （[簡單名稱](expressions.md#simple-names)）或*member_access* （[成員存取](expressions.md#member-access)），在運算式中取得常數的值。</span><span class="sxs-lookup"><span data-stu-id="2d25f-585">The value of a constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="2d25f-586">常數本身可以參與*constant_expression*。</span><span class="sxs-lookup"><span data-stu-id="2d25f-586">A constant can itself participate in a *constant_expression*.</span></span> <span data-ttu-id="2d25f-587">因此，常數可以用於任何需要*constant_expression*的結構中。</span><span class="sxs-lookup"><span data-stu-id="2d25f-587">Thus, a constant may be used in any construct that requires a *constant_expression*.</span></span> <span data-ttu-id="2d25f-588">這類結構的範例包括 `case` 標籤、`goto case` 語句、`enum` 成員宣告、屬性和其他常數宣告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-588">Examples of such constructs include `case` labels, `goto case` statements, `enum` member declarations, attributes, and other constant declarations.</span></span>

<span data-ttu-id="2d25f-589">如[常數運算式](expressions.md#constant-expressions)中所述， *constant_expression*是可以在編譯時期完整評估的運算式。</span><span class="sxs-lookup"><span data-stu-id="2d25f-589">As described in [Constant expressions](expressions.md#constant-expressions), a *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span> <span data-ttu-id="2d25f-590">因為建立非 null 值的唯一方法是要套用 `new` 運算子，而且在*constant_expression*中不允許 `new` 運算子，所以 reference_type `string`*以外的常數的唯一*可能值為 `null`，而不是 `string` 的*reference_type* 。</span><span class="sxs-lookup"><span data-stu-id="2d25f-590">Since the only way to create a non-null value of a *reference_type* other than `string` is to apply the `new` operator, and since the `new` operator is not permitted in a *constant_expression*, the only possible value for constants of *reference_type*s other than `string` is `null`.</span></span>

<span data-ttu-id="2d25f-591">如果需要常數值的符號名稱，但常數宣告中不允許該值的型別，或是在編譯時期無法以*constant_expression*計算值，則可以改用 `readonly` 欄位（[Readonly 欄位](classes.md#readonly-fields)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-591">When a symbolic name for a constant value is desired, but when the type of that value is not permitted in a constant declaration, or when the value cannot be computed at compile-time by a *constant_expression*, a `readonly` field ([Readonly fields](classes.md#readonly-fields)) may be used instead.</span></span>

<span data-ttu-id="2d25f-592">宣告多個常數的常數宣告相當於多個具有相同屬性、修飾詞和類型之單一常數的宣告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-592">A constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="2d25f-593">例如</span><span class="sxs-lookup"><span data-stu-id="2d25f-593">For example</span></span>
```csharp
class A
{
    public const double X = 1.0, Y = 2.0, Z = 3.0;
}
```
<span data-ttu-id="2d25f-594">相當於</span><span class="sxs-lookup"><span data-stu-id="2d25f-594">is equivalent to</span></span>
```csharp
class A
{
    public const double X = 1.0;
    public const double Y = 2.0;
    public const double Z = 3.0;
}
```

<span data-ttu-id="2d25f-595">只要相依性不是迴圈本質，常數就可以相依于同一個程式內的其他常數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-595">Constants are permitted to depend on other constants within the same program as long as the dependencies are not of a circular nature.</span></span> <span data-ttu-id="2d25f-596">編譯器會自動排文，以適當的順序來評估常數宣告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-596">The compiler automatically arranges to evaluate the constant declarations in the appropriate order.</span></span> <span data-ttu-id="2d25f-597">在範例中</span><span class="sxs-lookup"><span data-stu-id="2d25f-597">In the example</span></span>
```csharp
class A
{
    public const int X = B.Z + 1;
    public const int Y = 10;
}

class B
{
    public const int Z = A.Y + 1;
}
```
<span data-ttu-id="2d25f-598">編譯器會先評估 `A.Y`，然後評估 `B.Z`，最後再評估 `A.X`，產生 `10`、`11`和 `12`的值。</span><span class="sxs-lookup"><span data-stu-id="2d25f-598">the compiler first evaluates `A.Y`, then evaluates `B.Z`, and finally evaluates `A.X`, producing the values `10`, `11`, and `12`.</span></span> <span data-ttu-id="2d25f-599">常數宣告可能取決於其他程式的常數，但這類相依性只能以單向方式進行。</span><span class="sxs-lookup"><span data-stu-id="2d25f-599">Constant declarations may depend on constants from other programs, but such dependencies are only possible in one direction.</span></span> <span data-ttu-id="2d25f-600">參考上述範例，如果 `A` 和 `B` 是在不同的程式中宣告，`A.X` 可能會相依于 `B.Z`，但 `B.Z` 之後可能不會同時相依于 `A.Y`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-600">Referring to the example above, if `A` and `B` were declared in separate programs, it would be possible for `A.X` to depend on `B.Z`, but `B.Z` could then not simultaneously depend on `A.Y`.</span></span>

## <a name="fields"></a><span data-ttu-id="2d25f-601">欄位</span><span class="sxs-lookup"><span data-stu-id="2d25f-601">Fields</span></span>

<span data-ttu-id="2d25f-602">「***欄位***」（field）是代表與物件或類別相關聯之變數的成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-602">A ***field*** is a member that represents a variable associated with an object or class.</span></span> <span data-ttu-id="2d25f-603">*Field_declaration*會引進一或多個指定類型的欄位。</span><span class="sxs-lookup"><span data-stu-id="2d25f-603">A *field_declaration* introduces one or more fields of a given type.</span></span>

```antlr
field_declaration
    : attributes? field_modifier* type variable_declarators ';'
    ;

field_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'readonly'
    | 'volatile'
    | field_modifier_unsafe
    ;

variable_declarators
    : variable_declarator (',' variable_declarator)*
    ;

variable_declarator
    : identifier ('=' variable_initializer)?
    ;

variable_initializer
    : expression
    | array_initializer
    ;
```

<span data-ttu-id="2d25f-604">*Field_declaration*可能包括一組*屬性*（attribute）、`new` 修飾[詞（](attributes.md)新的[修飾](classes.md#the-new-modifier)詞）、四個存取修飾詞（[存取](classes.md#access-modifiers)修飾詞）的有效組合，以及 `static` 修飾詞（[靜態和實例欄位](classes.md#static-and-instance-fields)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-604">A *field_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and a `static` modifier ([Static and instance fields](classes.md#static-and-instance-fields)).</span></span> <span data-ttu-id="2d25f-605">此外， *field_declaration*可能`readonly`包括修飾詞（[Readonly 欄位](classes.md#readonly-fields)`volatile`）或修飾詞（[Volatile 欄位](classes.md#volatile-fields)），但不能同時包含兩者。</span><span class="sxs-lookup"><span data-stu-id="2d25f-605">In addition, a *field_declaration* may include a `readonly` modifier ([Readonly fields](classes.md#readonly-fields)) or a `volatile` modifier ([Volatile fields](classes.md#volatile-fields)) but not both.</span></span> <span data-ttu-id="2d25f-606">屬性和修飾詞適用于*field_declaration*所宣告的所有成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-606">The attributes and modifiers apply to all of the members declared by the *field_declaration*.</span></span> <span data-ttu-id="2d25f-607">在欄位宣告中多次出現相同的修飾詞是錯誤的。</span><span class="sxs-lookup"><span data-stu-id="2d25f-607">It is an error for the same modifier to appear multiple times in a field declaration.</span></span>

<span data-ttu-id="2d25f-608">*Field_declaration*的*類型*會指定宣告引進的成員類型。</span><span class="sxs-lookup"><span data-stu-id="2d25f-608">The *type* of a *field_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="2d25f-609">類型後面會接著*variable_declarator*的清單，其中每個都會引進新的成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-609">The type is followed by a list of *variable_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="2d25f-610">*Variable_declarator*由*識別*該成員的識別碼（選擇性地後面接著 "`=`" token）和*variable_initializer* （[變數初始化運算式](classes.md#variable-initializers)）所組成，以提供該成員的初始值。</span><span class="sxs-lookup"><span data-stu-id="2d25f-610">A *variable_declarator* consists of an *identifier* that names that member, optionally followed by an "`=`" token and a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)) that gives the initial value of that member.</span></span>

<span data-ttu-id="2d25f-611">欄位的*類型*至少必須與欄位本身（[存取範圍條件約束](basic-concepts.md#accessibility-constraints)）一樣可以存取。</span><span class="sxs-lookup"><span data-stu-id="2d25f-611">The *type* of a field must be at least as accessible as the field itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="2d25f-612">欄位的值是在使用*simple_name* （[簡單名稱](expressions.md#simple-names)）或*member_access* （[成員存取](expressions.md#member-access)）的運算式中取得。</span><span class="sxs-lookup"><span data-stu-id="2d25f-612">The value of a field is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="2d25f-613">非唯讀欄位的值是使用*指派*（[指派運算子](expressions.md#assignment-operators)）進行修改。</span><span class="sxs-lookup"><span data-stu-id="2d25f-613">The value of a non-readonly field is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="2d25f-614">您可以使用後置遞增和遞減運算子（後置[遞增和遞減運算子](expressions.md#postfix-increment-and-decrement-operators)）和前置遞增和遞減運算子（[前置遞增](expressions.md#prefix-increment-and-decrement-operators)和遞減運算子）來取得和修改非唯讀欄位的值。</span><span class="sxs-lookup"><span data-stu-id="2d25f-614">The value of a non-readonly field can be both obtained and modified using postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)) and prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="2d25f-615">宣告多個欄位的欄位宣告相當於具有相同屬性、修飾詞和類型之單一欄位的多個宣告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-615">A field declaration that declares multiple fields is equivalent to multiple declarations of single fields with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="2d25f-616">例如</span><span class="sxs-lookup"><span data-stu-id="2d25f-616">For example</span></span>
```csharp
class A
{
    public static int X = 1, Y, Z = 100;
}
```
<span data-ttu-id="2d25f-617">相當於</span><span class="sxs-lookup"><span data-stu-id="2d25f-617">is equivalent to</span></span>
```csharp
class A
{
    public static int X = 1;
    public static int Y;
    public static int Z = 100;
}
```

### <a name="static-and-instance-fields"></a><span data-ttu-id="2d25f-618">靜態和實例欄位</span><span class="sxs-lookup"><span data-stu-id="2d25f-618">Static and instance fields</span></span>

<span data-ttu-id="2d25f-619">當欄位宣告包含 `static` 修飾詞時，宣告所引進的欄位就是***靜態欄位***。</span><span class="sxs-lookup"><span data-stu-id="2d25f-619">When a field declaration includes a `static` modifier, the fields introduced by the declaration are ***static fields***.</span></span> <span data-ttu-id="2d25f-620">當沒有 `static` 的修飾詞時，宣告所引進的欄位就是***實例欄位***。</span><span class="sxs-lookup"><span data-stu-id="2d25f-620">When no `static` modifier is present, the fields introduced by the declaration are ***instance fields***.</span></span> <span data-ttu-id="2d25f-621">靜態欄位和實例欄位是支援的數種變數（[變數](variables.md)） C#中的兩種，有時分別稱為***靜態變數***和***執行個體變數***。</span><span class="sxs-lookup"><span data-stu-id="2d25f-621">Static fields and instance fields are two of the several kinds of variables ([Variables](variables.md)) supported by C#, and at times they are referred to as ***static variables*** and ***instance variables***, respectively.</span></span>

<span data-ttu-id="2d25f-622">靜態欄位不是特定實例的一部分;相反地，它會在封閉式類型（[開放式和封閉式類型](types.md#open-and-closed-types)）的所有實例之間共用。</span><span class="sxs-lookup"><span data-stu-id="2d25f-622">A static field is not part of a specific instance; instead, it is shared amongst all instances of a closed type ([Open and closed types](types.md#open-and-closed-types)).</span></span> <span data-ttu-id="2d25f-623">無論已關閉的類別類型有多少個實例，相關聯的應用程式域只會有一個靜態欄位複本。</span><span class="sxs-lookup"><span data-stu-id="2d25f-623">No matter how many instances of a closed class type are created, there is only ever one copy of a static field for the associated application domain.</span></span>

<span data-ttu-id="2d25f-624">例如：</span><span class="sxs-lookup"><span data-stu-id="2d25f-624">For example:</span></span>
```csharp
class C<V>
{
    static int count = 0;

    public C() {
        count++;
    }

    public static int Count {
        get { return count; }
    }
}

class Application
{
    static void Main() {
        C<int> x1 = new C<int>();
        Console.WriteLine(C<int>.Count);        // Prints 1

        C<double> x2 = new C<double>();
        Console.WriteLine(C<int>.Count);        // Prints 1

        C<int> x3 = new C<int>();
        Console.WriteLine(C<int>.Count);        // Prints 2
    }
}
```

<span data-ttu-id="2d25f-625">實例欄位屬於實例。</span><span class="sxs-lookup"><span data-stu-id="2d25f-625">An instance field belongs to an instance.</span></span> <span data-ttu-id="2d25f-626">具體而言，類別的每個實例都包含該類別的一組個別的實例欄位。</span><span class="sxs-lookup"><span data-stu-id="2d25f-626">Specifically, every instance of a class contains a separate set of all the instance fields of that class.</span></span>

<span data-ttu-id="2d25f-627">在表單 `E.M`的*member_access* （[成員存取](expressions.md#member-access)）中參考欄位時，如果 `M` 是靜態欄位，`E` 必須代表包含 `M`的類型，而且如果 `M` 是實例欄位，則 E 必須代表包含 `M`之類型的實例。</span><span class="sxs-lookup"><span data-stu-id="2d25f-627">When a field is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static field, `E` must denote a type containing `M`, and if `M` is an instance field, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="2d25f-628">靜態和實例成員之間的差異會在[靜態和實例成員](classes.md#static-and-instance-members)中進一步討論。</span><span class="sxs-lookup"><span data-stu-id="2d25f-628">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="readonly-fields"></a><span data-ttu-id="2d25f-629">唯讀欄位</span><span class="sxs-lookup"><span data-stu-id="2d25f-629">Readonly fields</span></span>

<span data-ttu-id="2d25f-630">當*field_declaration*包含 `readonly` 修飾詞時，宣告所引進的欄位就是***唯讀欄位***。</span><span class="sxs-lookup"><span data-stu-id="2d25f-630">When a *field_declaration* includes a `readonly` modifier, the fields introduced by the declaration are ***readonly fields***.</span></span> <span data-ttu-id="2d25f-631">唯讀欄位的直接指派只能做為該宣告的一部分，或在相同類別中的實例的函式或靜態的函數中。</span><span class="sxs-lookup"><span data-stu-id="2d25f-631">Direct assignments to readonly fields can only occur as part of that declaration or in an instance constructor or static constructor in the same class.</span></span> <span data-ttu-id="2d25f-632">（您可以在這些內容中多次指派 readonly 欄位）。具體而言，只有在下列內容中才允許直接指派 `readonly` 欄位：</span><span class="sxs-lookup"><span data-stu-id="2d25f-632">(A readonly field can be assigned to multiple times in these contexts.) Specifically, direct assignments to a `readonly` field are permitted only in the following contexts:</span></span>

*  <span data-ttu-id="2d25f-633">在引進欄位的*variable_declarator*中（藉由在宣告中包含*variable_initializer* ）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-633">In the *variable_declarator* that introduces the field (by including a *variable_initializer* in the declaration).</span></span>
*  <span data-ttu-id="2d25f-634">針對實例欄位，在包含欄位宣告的類別的實例函式中。針對靜態欄位，在包含欄位宣告之類別的靜態函式中。</span><span class="sxs-lookup"><span data-stu-id="2d25f-634">For an instance field, in the instance constructors of the class that contains the field declaration; for a static field, in the static constructor of the class that contains the field declaration.</span></span> <span data-ttu-id="2d25f-635">這些也是唯一有效的內容，可將 `readonly` 欄位當做 `out` 或 `ref` 參數傳遞。</span><span class="sxs-lookup"><span data-stu-id="2d25f-635">These are also the only contexts in which it is valid to pass a `readonly` field as an `out` or `ref` parameter.</span></span>

<span data-ttu-id="2d25f-636">嘗試指派給 `readonly` 欄位，或在任何其他內容中以 `out` 或 `ref` 參數的形式傳遞，會產生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-636">Attempting to assign to a `readonly` field or pass it as an `out` or `ref` parameter in any other context is a compile-time error.</span></span>

#### <a name="using-static-readonly-fields-for-constants"></a><span data-ttu-id="2d25f-637">使用常數的靜態唯讀欄位</span><span class="sxs-lookup"><span data-stu-id="2d25f-637">Using static readonly fields for constants</span></span>

<span data-ttu-id="2d25f-638">當需要常數值的符號名稱，但在 `const` 宣告中不允許值的類型，或在編譯時期無法計算值時，`static readonly` 欄位會很有用。</span><span class="sxs-lookup"><span data-stu-id="2d25f-638">A `static readonly` field is useful when a symbolic name for a constant value is desired, but when the type of the value is not permitted in a `const` declaration, or when the value cannot be computed at compile-time.</span></span> <span data-ttu-id="2d25f-639">在範例中</span><span class="sxs-lookup"><span data-stu-id="2d25f-639">In the example</span></span>
```csharp
public class Color
{
    public static readonly Color Black = new Color(0, 0, 0);
    public static readonly Color White = new Color(255, 255, 255);
    public static readonly Color Red = new Color(255, 0, 0);
    public static readonly Color Green = new Color(0, 255, 0);
    public static readonly Color Blue = new Color(0, 0, 255);

    private byte red, green, blue;

    public Color(byte r, byte g, byte b) {
        red = r;
        green = g;
        blue = b;
    }
}
```
<span data-ttu-id="2d25f-640">`Black`、`White`、`Red`、`Green`和 `Blue` 成員無法宣告為 `const` 成員，因為它們的值無法在編譯時期計算。</span><span class="sxs-lookup"><span data-stu-id="2d25f-640">the `Black`, `White`, `Red`, `Green`, and `Blue` members cannot be declared as `const` members because their values cannot be computed at compile-time.</span></span> <span data-ttu-id="2d25f-641">不過，將它們宣告 `static readonly` 反而會有相同的效果。</span><span class="sxs-lookup"><span data-stu-id="2d25f-641">However, declaring them `static readonly` instead has much the same effect.</span></span>

#### <a name="versioning-of-constants-and-static-readonly-fields"></a><span data-ttu-id="2d25f-642">常數和靜態唯讀欄位的版本控制</span><span class="sxs-lookup"><span data-stu-id="2d25f-642">Versioning of constants and static readonly fields</span></span>

<span data-ttu-id="2d25f-643">常數和 readonly 欄位具有不同的二進位版本控制語義。</span><span class="sxs-lookup"><span data-stu-id="2d25f-643">Constants and readonly fields have different binary versioning semantics.</span></span> <span data-ttu-id="2d25f-644">當運算式參考常數時，常數的值會在編譯時期取得，但當運算式參考 readonly 欄位時，在執行時間之前不會取得欄位的值。</span><span class="sxs-lookup"><span data-stu-id="2d25f-644">When an expression references a constant, the value of the constant is obtained at compile-time, but when an expression references a readonly field, the value of the field is not obtained until run-time.</span></span> <span data-ttu-id="2d25f-645">假設有一個應用程式是由兩個不同的程式所組成：</span><span class="sxs-lookup"><span data-stu-id="2d25f-645">Consider an application that consists of two separate programs:</span></span>
```csharp
using System;

namespace Program1
{
    public class Utils
    {
        public static readonly int X = 1;
    }
}

namespace Program2
{
    class Test
    {
        static void Main() {
            Console.WriteLine(Program1.Utils.X);
        }
    }
}
```

<span data-ttu-id="2d25f-646">`Program1` 和 `Program2` 命名空間代表分別編譯的兩個程式。</span><span class="sxs-lookup"><span data-stu-id="2d25f-646">The `Program1` and `Program2` namespaces denote two programs that are compiled separately.</span></span> <span data-ttu-id="2d25f-647">因為 `Program1.Utils.X` 宣告為靜態唯讀欄位，所以 `Console.WriteLine` 語句的輸出值在編譯時期未知，而是在執行時間取得。</span><span class="sxs-lookup"><span data-stu-id="2d25f-647">Because `Program1.Utils.X` is declared as a static readonly field, the value output by the `Console.WriteLine` statement is not known at compile-time, but rather is obtained at run-time.</span></span> <span data-ttu-id="2d25f-648">因此，如果 `X` 的值已變更，且 `Program1` 重新編譯，則 `Console.WriteLine` 語句會輸出新的值，即使 `Program2` 並未重新編譯也一樣。</span><span class="sxs-lookup"><span data-stu-id="2d25f-648">Thus, if the value of `X` is changed and `Program1` is recompiled, the `Console.WriteLine` statement will output the new value even if `Program2` isn't recompiled.</span></span> <span data-ttu-id="2d25f-649">不過，`X` 是常數，`X` 的值會在編譯 `Program2` 時取得，而且在重新編譯 `Program2` 之前，`Program1` 中的變更不會受到影響。</span><span class="sxs-lookup"><span data-stu-id="2d25f-649">However, had `X` been a constant, the value of `X` would have been obtained at the time `Program2` was compiled, and would remain unaffected by changes in `Program1` until `Program2` is recompiled.</span></span>

### <a name="volatile-fields"></a><span data-ttu-id="2d25f-650">變動欄位</span><span class="sxs-lookup"><span data-stu-id="2d25f-650">Volatile fields</span></span>

<span data-ttu-id="2d25f-651">當*field_declaration*包含 `volatile` 修飾詞時，該宣告引進的欄位就是***變動性欄位***。</span><span class="sxs-lookup"><span data-stu-id="2d25f-651">When a *field_declaration* includes a `volatile` modifier, the fields introduced by that declaration are ***volatile fields***.</span></span>

<span data-ttu-id="2d25f-652">對於非動態欄位而言，重新排列指令的優化技術可能會導致非預期且無法預期的結果，而不需要同步處理（如*lock_statement* （[lock 語句](statements.md#the-lock-statement)）所提供）的多執行緒程式存取欄位。</span><span class="sxs-lookup"><span data-stu-id="2d25f-652">For non-volatile fields, optimization techniques that reorder instructions can lead to unexpected and unpredictable results in multi-threaded programs that access fields without synchronization such as that provided by the *lock_statement* ([The lock statement](statements.md#the-lock-statement)).</span></span> <span data-ttu-id="2d25f-653">這些優化可以由編譯器、執行時間系統或硬體執行。</span><span class="sxs-lookup"><span data-stu-id="2d25f-653">These optimizations can be performed by the compiler, by the run-time system, or by hardware.</span></span> <span data-ttu-id="2d25f-654">對於變動性欄位，這類重新排列優化會受到限制：</span><span class="sxs-lookup"><span data-stu-id="2d25f-654">For volatile fields, such reordering optimizations are restricted:</span></span>

*  <span data-ttu-id="2d25f-655">讀取 volatile 欄位稱為「***變動性讀取***」。</span><span class="sxs-lookup"><span data-stu-id="2d25f-655">A read of a volatile field is called a ***volatile read***.</span></span> <span data-ttu-id="2d25f-656">「變動性讀取」具有「取得語義」;也就是說，在指令序列之後發生的記憶體參照之前，一定會發生這種情況。</span><span class="sxs-lookup"><span data-stu-id="2d25f-656">A volatile read has "acquire semantics"; that is, it is guaranteed to occur prior to any references to memory that occur after it in the instruction sequence.</span></span>
*  <span data-ttu-id="2d25f-657">Volatile 欄位的寫入稱為「***變動性寫入***」。</span><span class="sxs-lookup"><span data-stu-id="2d25f-657">A write of a volatile field is called a ***volatile write***.</span></span> <span data-ttu-id="2d25f-658">Volatile 寫入具有「發行語義」;也就是說，在指令序列中寫入指令之前的任何記憶體參考之後，一定會發生這種情況。</span><span class="sxs-lookup"><span data-stu-id="2d25f-658">A volatile write has "release semantics"; that is, it is guaranteed to happen after any memory references prior to the write instruction in the instruction sequence.</span></span>

<span data-ttu-id="2d25f-659">這些限制可確保所有的執行緒將會觀察任何其他執行緒執行的 volatile 寫入會按照其執行的順序進行。</span><span class="sxs-lookup"><span data-stu-id="2d25f-659">These restrictions ensure that all threads will observe volatile writes performed by any other thread in the order in which they were performed.</span></span> <span data-ttu-id="2d25f-660">不需要有符合的執行方式，就能提供單一的變動性寫入順序，如同所有執行執行緒所看到的一樣。</span><span class="sxs-lookup"><span data-stu-id="2d25f-660">A conforming implementation is not required to provide a single total ordering of volatile writes as seen from all threads of execution.</span></span> <span data-ttu-id="2d25f-661">Volatile 欄位的類型必須是下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="2d25f-661">The type of a volatile field must be one of the following:</span></span>

*  <span data-ttu-id="2d25f-662">*Reference_type*。</span><span class="sxs-lookup"><span data-stu-id="2d25f-662">A *reference_type*.</span></span>
*  <span data-ttu-id="2d25f-663">類型 `byte`、`sbyte`、`short`、`ushort`、`int`、`uint`、`char`、`float`、`bool`、`System.IntPtr`或 `System.UIntPtr`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-663">The type `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`, or `System.UIntPtr`.</span></span>
*  <span data-ttu-id="2d25f-664">具有 `byte`、`sbyte`、`short`、`ushort`、`int`或 `uint`列舉基底類型的*enum_type* 。</span><span class="sxs-lookup"><span data-stu-id="2d25f-664">An *enum_type* having an enum base type of `byte`, `sbyte`, `short`, `ushort`, `int`, or `uint`.</span></span>

<span data-ttu-id="2d25f-665">範例</span><span class="sxs-lookup"><span data-stu-id="2d25f-665">The example</span></span>
```csharp
using System;
using System.Threading;

class Test
{
    public static int result;   
    public static volatile bool finished;

    static void Thread2() {
        result = 143;    
        finished = true; 
    }

    static void Main() {
        finished = false;

        // Run Thread2() in a new thread
        new Thread(new ThreadStart(Thread2)).Start();

        // Wait for Thread2 to signal that it has a result by setting
        // finished to true.
        for (;;) {
            if (finished) {
                Console.WriteLine("result = {0}", result);
                return;
            }
        }
    }
}
```
<span data-ttu-id="2d25f-666">產生下列輸出：</span><span class="sxs-lookup"><span data-stu-id="2d25f-666">produces the output:</span></span>
```console
result = 143
```

<span data-ttu-id="2d25f-667">在此範例中，方法 `Main` 會啟動新的執行緒，以執行 `Thread2`的方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-667">In this example, the method `Main` starts a new thread that runs the method `Thread2`.</span></span> <span data-ttu-id="2d25f-668">這個方法會將值儲存在稱為 `result`的非 volatile 欄位中，然後將 `true` 儲存在 volatile 欄位 `finished`中。</span><span class="sxs-lookup"><span data-stu-id="2d25f-668">This method stores a value into a non-volatile field called `result`, then stores `true` in the volatile field `finished`.</span></span> <span data-ttu-id="2d25f-669">主要執行緒會等候欄位 `finished` 設定為 `true`，然後讀取欄位 `result`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-669">The main thread waits for the field `finished` to be set to `true`, then reads the field `result`.</span></span> <span data-ttu-id="2d25f-670">因為 `finished` 已 `volatile`宣告，所以主執行緒必須從欄位 `result`讀取值 `143`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-670">Since `finished` has been declared `volatile`, the main thread must read the value `143` from the field `result`.</span></span> <span data-ttu-id="2d25f-671">如果未將欄位 `finished` 宣告 `volatile`，則會允許存放區在 `finished`存放區之後，讓主執行緒看到其 `result`，因此主執行緒會從欄位 `0` 讀取值 `result`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-671">If the field `finished` had not been declared `volatile`, then it would be permissible for the store to `result` to be visible to the main thread after the store to `finished`, and hence for the main thread to read the value `0` from the field `result`.</span></span> <span data-ttu-id="2d25f-672">將 `finished` 宣告為 `volatile` 欄位，可防止任何這類不一致的情況。</span><span class="sxs-lookup"><span data-stu-id="2d25f-672">Declaring `finished` as a `volatile` field prevents any such inconsistency.</span></span>

### <a name="field-initialization"></a><span data-ttu-id="2d25f-673">欄位初始化</span><span class="sxs-lookup"><span data-stu-id="2d25f-673">Field initialization</span></span>

<span data-ttu-id="2d25f-674">欄位的初始值（不論是靜態欄位或實例欄位）是欄位類型的預設值（[預設](variables.md#default-values)值）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-674">The initial value of a field, whether it be a static field or an instance field, is the default value ([Default values](variables.md#default-values)) of the field's type.</span></span> <span data-ttu-id="2d25f-675">在此預設初始化發生之前，不可能觀察到欄位的值，而且欄位永遠不會「未初始化」。</span><span class="sxs-lookup"><span data-stu-id="2d25f-675">It is not possible to observe the value of a field before this default initialization has occurred, and a field is thus never "uninitialized".</span></span> <span data-ttu-id="2d25f-676">範例</span><span class="sxs-lookup"><span data-stu-id="2d25f-676">The example</span></span>
```csharp
using System;

class Test
{
    static bool b;
    int i;

    static void Main() {
        Test t = new Test();
        Console.WriteLine("b = {0}, i = {1}", b, t.i);
    }
}
```
<span data-ttu-id="2d25f-677">產生下列輸出</span><span class="sxs-lookup"><span data-stu-id="2d25f-677">produces the output</span></span>
```console
b = False, i = 0
```
<span data-ttu-id="2d25f-678">因為 `b` 和 `i` 都會自動初始化為預設值。</span><span class="sxs-lookup"><span data-stu-id="2d25f-678">because `b` and `i` are both automatically initialized to default values.</span></span>

### <a name="variable-initializers"></a><span data-ttu-id="2d25f-679">變數初始化運算式</span><span class="sxs-lookup"><span data-stu-id="2d25f-679">Variable initializers</span></span>

<span data-ttu-id="2d25f-680">欄位宣告可能包括*variable_initializer*s。</span><span class="sxs-lookup"><span data-stu-id="2d25f-680">Field declarations may include *variable_initializer*s.</span></span> <span data-ttu-id="2d25f-681">對於靜態欄位，變數初始化運算式會對應到在類別初始化期間執行的指派語句。</span><span class="sxs-lookup"><span data-stu-id="2d25f-681">For static fields, variable initializers correspond to assignment statements that are executed during class initialization.</span></span> <span data-ttu-id="2d25f-682">就實例欄位而言，變數初始化運算式會對應至建立類別的實例時所執行的指派語句。</span><span class="sxs-lookup"><span data-stu-id="2d25f-682">For instance fields, variable initializers correspond to assignment statements that are executed when an instance of the class is created.</span></span>

<span data-ttu-id="2d25f-683">範例</span><span class="sxs-lookup"><span data-stu-id="2d25f-683">The example</span></span>
```csharp
using System;

class Test
{
    static double x = Math.Sqrt(2.0);
    int i = 100;
    string s = "Hello";

    static void Main() {
        Test a = new Test();
        Console.WriteLine("x = {0}, i = {1}, s = {2}", x, a.i, a.s);
    }
}
```
<span data-ttu-id="2d25f-684">產生下列輸出</span><span class="sxs-lookup"><span data-stu-id="2d25f-684">produces the output</span></span>
```console
x = 1.4142135623731, i = 100, s = Hello
```
<span data-ttu-id="2d25f-685">因為在執行實例欄位初始化運算式時，會將指派給 `x`，因此會發生靜態欄位初始化運算式，並將其指派給 `i` 並 `s`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-685">because an assignment to `x` occurs when static field initializers execute and assignments to `i` and `s` occur when the instance field initializers execute.</span></span>

<span data-ttu-id="2d25f-686">[欄位初始化](classes.md#field-initialization)中所述的預設值初始化會針對所有欄位進行，包括具有變數初始化運算式的欄位。</span><span class="sxs-lookup"><span data-stu-id="2d25f-686">The default value initialization described in [Field initialization](classes.md#field-initialization) occurs for all fields, including fields that have variable initializers.</span></span> <span data-ttu-id="2d25f-687">因此，在初始化類別時，該類別中的所有靜態欄位都會先初始化為其預設值，然後靜態欄位初始化運算式會以文字循序執行。</span><span class="sxs-lookup"><span data-stu-id="2d25f-687">Thus, when a class is initialized, all static fields in that class are first initialized to their default values, and then the static field initializers are executed in textual order.</span></span> <span data-ttu-id="2d25f-688">同樣地，當建立類別的實例時，該實例中的所有實例欄位都會先初始化為其預設值，然後再以文字循序執行實例欄位初始化運算式。</span><span class="sxs-lookup"><span data-stu-id="2d25f-688">Likewise, when an instance of a class is created, all instance fields in that instance are first initialized to their default values, and then the instance field initializers are executed in textual order.</span></span>

<span data-ttu-id="2d25f-689">具有變數初始化運算式的靜態欄位有可能會以其預設值狀態觀察到。</span><span class="sxs-lookup"><span data-stu-id="2d25f-689">It is possible for static fields with variable initializers to be observed in their default value state.</span></span> <span data-ttu-id="2d25f-690">不過，強烈建議您不要使用這種樣式。</span><span class="sxs-lookup"><span data-stu-id="2d25f-690">However, this is strongly discouraged as a matter of style.</span></span> <span data-ttu-id="2d25f-691">範例</span><span class="sxs-lookup"><span data-stu-id="2d25f-691">The example</span></span>
```csharp
using System;

class Test
{
    static int a = b + 1;
    static int b = a + 1;

    static void Main() {
        Console.WriteLine("a = {0}, b = {1}", a, b);
    }
}
```
<span data-ttu-id="2d25f-692">展示此行為。</span><span class="sxs-lookup"><span data-stu-id="2d25f-692">exhibits this behavior.</span></span> <span data-ttu-id="2d25f-693">雖然 a 和 b 的迴圈定義，但程式是有效的。</span><span class="sxs-lookup"><span data-stu-id="2d25f-693">Despite the circular definitions of a and b, the program is valid.</span></span> <span data-ttu-id="2d25f-694">這會導致輸出</span><span class="sxs-lookup"><span data-stu-id="2d25f-694">It results in the output</span></span>
```console
a = 1, b = 2
```
<span data-ttu-id="2d25f-695">因為靜態欄位 `a` 和 `b` 會先初始化為 `0` （`int`的預設值），才會執行其初始化運算式。</span><span class="sxs-lookup"><span data-stu-id="2d25f-695">because the static fields `a` and `b` are initialized to `0` (the default value for `int`) before their initializers are executed.</span></span> <span data-ttu-id="2d25f-696">當 `a` 的初始化運算式執行時，`b` 的值為零，因此 `a` 會初始化為 `1`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-696">When the initializer for `a` runs, the value of `b` is zero, and so `a` is initialized to `1`.</span></span> <span data-ttu-id="2d25f-697">當 `b` 的初始化運算式執行時，`a` 的值已經 `1`，因此 `b` 會初始化為 `2`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-697">When the initializer for `b` runs, the value of `a` is already `1`, and so `b` is initialized to `2`.</span></span>

#### <a name="static-field-initialization"></a><span data-ttu-id="2d25f-698">靜態欄位初始化</span><span class="sxs-lookup"><span data-stu-id="2d25f-698">Static field initialization</span></span>

<span data-ttu-id="2d25f-699">類別的靜態欄位變數初始化運算式會對應到一系列指派，這些是以它們出現在類別宣告中的文字順序來執行。</span><span class="sxs-lookup"><span data-stu-id="2d25f-699">The static field variable initializers of a class correspond to a sequence of assignments that are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="2d25f-700">如果靜態的函式（[靜態](classes.md#static-constructors)的函式）存在於類別中，則在執行該靜態的函式之前，會立即執行靜態欄位初始化運算式。</span><span class="sxs-lookup"><span data-stu-id="2d25f-700">If a static constructor ([Static constructors](classes.md#static-constructors)) exists in the class, execution of the static field initializers occurs immediately prior to executing that static constructor.</span></span> <span data-ttu-id="2d25f-701">否則，靜態欄位初始化運算式會在第一次使用該類別的靜態欄位之前，在與執行相關的時間執行。</span><span class="sxs-lookup"><span data-stu-id="2d25f-701">Otherwise, the static field initializers are executed at an implementation-dependent time prior to the first use of a static field of that class.</span></span> <span data-ttu-id="2d25f-702">範例</span><span class="sxs-lookup"><span data-stu-id="2d25f-702">The example</span></span>
```csharp
using System;

class Test 
{ 
    static void Main() {
        Console.WriteLine("{0} {1}", B.Y, A.X);
    }

    public static int F(string s) {
        Console.WriteLine(s);
        return 1;
    }
}

class A
{
    public static int X = Test.F("Init A");
}

class B
{
    public static int Y = Test.F("Init B");
}
```
<span data-ttu-id="2d25f-703">可能會產生以下輸出：</span><span class="sxs-lookup"><span data-stu-id="2d25f-703">might produce either the output:</span></span>
```console
Init A
Init B
1 1
```
<span data-ttu-id="2d25f-704">或輸出：</span><span class="sxs-lookup"><span data-stu-id="2d25f-704">or the output:</span></span>
```console
Init B
Init A
1 1
```
<span data-ttu-id="2d25f-705">因為執行 `X`的初始化運算式和 `Y`的初始化運算式可能會以任一順序發生，它們只會在這些欄位的參考之前受到限制。</span><span class="sxs-lookup"><span data-stu-id="2d25f-705">because the execution of `X`'s initializer and `Y`'s initializer could occur in either order; they are only constrained to occur before the references to those fields.</span></span> <span data-ttu-id="2d25f-706">不過，在此範例中：</span><span class="sxs-lookup"><span data-stu-id="2d25f-706">However, in the example:</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        Console.WriteLine("{0} {1}", B.Y, A.X);
    }

    public static int F(string s) {
        Console.WriteLine(s);
        return 1;
    }
}

class A
{
    static A() {}

    public static int X = Test.F("Init A");
}

class B
{
    static B() {}

    public static int Y = Test.F("Init B");
}
```
<span data-ttu-id="2d25f-707">輸出必須是：</span><span class="sxs-lookup"><span data-stu-id="2d25f-707">the output must be:</span></span>
```console
Init B
Init A
1 1
```
<span data-ttu-id="2d25f-708">因為靜態執行檔（如[靜態](classes.md#static-constructors)的函式中所定義）的規則，會提供 `B`的靜態函式（因此 `B`的靜態欄位初始化運算式），必須在 `A`的靜態的程式和欄位初始化運算式之前執行。</span><span class="sxs-lookup"><span data-stu-id="2d25f-708">because the rules for when static constructors execute (as defined in [Static constructors](classes.md#static-constructors)) provide that `B`'s static constructor (and hence `B`'s static field initializers) must run before `A`'s static constructor and field initializers.</span></span>

#### <a name="instance-field-initialization"></a><span data-ttu-id="2d25f-709">實例欄位初始化</span><span class="sxs-lookup"><span data-stu-id="2d25f-709">Instance field initialization</span></span>

<span data-ttu-id="2d25f-710">類別的實例欄位變數初始化運算式，會對應至該類別的任何一個實例函式（「函式[初始化運算式](classes.md#constructor-initializers)」）進入時立即執行的一系列指派。</span><span class="sxs-lookup"><span data-stu-id="2d25f-710">The instance field variable initializers of a class correspond to a sequence of assignments that are executed immediately upon entry to any one of the instance constructors ([Constructor initializers](classes.md#constructor-initializers)) of that class.</span></span> <span data-ttu-id="2d25f-711">變數初始化運算式會以它們出現在類別宣告中的文字順序來執行。</span><span class="sxs-lookup"><span data-stu-id="2d25f-711">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="2d25f-712">類別實例的建立和初始化程式會在[實例](classes.md#instance-constructors)的函式中進一步說明。</span><span class="sxs-lookup"><span data-stu-id="2d25f-712">The class instance creation and initialization process is described further in [Instance constructors](classes.md#instance-constructors).</span></span>

<span data-ttu-id="2d25f-713">實例欄位的變數初始化運算式無法參考所建立的實例。</span><span class="sxs-lookup"><span data-stu-id="2d25f-713">A variable initializer for an instance field cannot reference the instance being created.</span></span> <span data-ttu-id="2d25f-714">因此，在變數初始化運算式中參考 `this` 時，就會發生編譯時期錯誤，因為變數初始化運算式會透過*simple_name*來參考任何實例成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-714">Thus, it is a compile-time error to reference `this` in a variable initializer, as it is a compile-time error for a variable initializer to reference any instance member through a *simple_name*.</span></span> <span data-ttu-id="2d25f-715">在範例中</span><span class="sxs-lookup"><span data-stu-id="2d25f-715">In the example</span></span>
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
<span data-ttu-id="2d25f-716">`y` 的變數初始化運算式會導致編譯時期錯誤，因為它會參考所建立之實例的成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-716">the variable initializer for `y` results in a compile-time error because it references a member of the instance being created.</span></span>

## <a name="methods"></a><span data-ttu-id="2d25f-717">方法</span><span class="sxs-lookup"><span data-stu-id="2d25f-717">Methods</span></span>

<span data-ttu-id="2d25f-718">「方法」是實作物件或類別所能執行之計算或動作的成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-718">A ***method*** is a member that implements a computation or action that can be performed by an object or class.</span></span> <span data-ttu-id="2d25f-719">方法是使用*method_declaration*s 來宣告：</span><span class="sxs-lookup"><span data-stu-id="2d25f-719">Methods are declared using *method_declaration*s:</span></span>

```antlr
method_declaration
    : method_header method_body
    ;

method_header
    : attributes? method_modifier* 'partial'? return_type member_name type_parameter_list?
      '(' formal_parameter_list? ')' type_parameter_constraints_clause*
    ;

method_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | 'async'
    | method_modifier_unsafe
    ;

return_type
    : type
    | 'void'
    ;

member_name
    : identifier
    | interface_type '.' identifier
    ;

method_body
    : block
    | '=>' expression ';'
    | ';'
    ;
```

<span data-ttu-id="2d25f-720">*Method_declaration*可能包括一組*屬性*（attribute）和四個存取修飾[詞（](attributes.md)[存取](classes.md#access-modifiers)修飾詞）的有效組合、`new` （[新的修飾](classes.md#the-new-modifier)詞）、`static` （[靜態和實例方法](classes.md#static-and-instance-methods)）、`virtual` （[虛擬方法](classes.md#virtual-methods)）、`override` （覆[寫方法](classes.md#override-methods)）、`sealed` （[密封方法](classes.md#sealed-methods)）、`abstract` （[抽象方法](classes.md#abstract-methods)），以及 `extern` （[外部方法](classes.md#external-methods)）修飾詞。</span><span class="sxs-lookup"><span data-stu-id="2d25f-720">A *method_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="2d25f-721">如果下列所有條件都成立，宣告就會有有效的修飾片語合：</span><span class="sxs-lookup"><span data-stu-id="2d25f-721">A declaration has a valid combination of modifiers if all of the following are true:</span></span>

*  <span data-ttu-id="2d25f-722">宣告包含有效的存取修飾詞（[存取](classes.md#access-modifiers)修飾詞）組合。</span><span class="sxs-lookup"><span data-stu-id="2d25f-722">The declaration includes a valid combination of access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span>
*  <span data-ttu-id="2d25f-723">宣告不會多次包含相同的修飾詞。</span><span class="sxs-lookup"><span data-stu-id="2d25f-723">The declaration does not include the same modifier multiple times.</span></span>
*  <span data-ttu-id="2d25f-724">宣告最多包含下列其中一個修飾詞： `static`、`virtual`和 `override`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-724">The declaration includes at most one of the following modifiers: `static`, `virtual`, and `override`.</span></span>
*  <span data-ttu-id="2d25f-725">宣告最多包含下列其中一個修飾詞： `new` 和 `override`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-725">The declaration includes at most one of the following modifiers: `new` and `override`.</span></span>
*  <span data-ttu-id="2d25f-726">如果宣告包含 `abstract` 修飾詞，則宣告不會包含下列任何修飾符： `static`、`virtual`、`sealed` 或 `extern`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-726">If the declaration includes the `abstract` modifier, then the declaration does not include any of the following modifiers: `static`, `virtual`, `sealed` or `extern`.</span></span>
*  <span data-ttu-id="2d25f-727">如果宣告包含 `private` 修飾詞，則宣告不會包含下列任何修飾符： `virtual`、`override`或 `abstract`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-727">If the declaration includes the `private` modifier, then the declaration does not include any of the following modifiers: `virtual`, `override`, or `abstract`.</span></span>
*  <span data-ttu-id="2d25f-728">如果宣告包含 `sealed` 修飾詞，則宣告也會包含 `override` 修飾詞。</span><span class="sxs-lookup"><span data-stu-id="2d25f-728">If the declaration includes the `sealed` modifier, then the declaration also includes the `override` modifier.</span></span>
*  <span data-ttu-id="2d25f-729">如果宣告包含 `partial` 修飾詞，則不會包含下列任何修飾詞： `new`、`public`、`protected`、`internal`、`private`、`virtual`、`sealed`、`override`、`abstract`或 `extern`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-729">If the declaration includes the `partial` modifier, then it does not include any of the following modifiers: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override`, `abstract`, or `extern`.</span></span>

<span data-ttu-id="2d25f-730">具有 `async` 修飾詞的方法是非同步函式，並遵循[非同步函數](classes.md#async-functions)中所述的規則。</span><span class="sxs-lookup"><span data-stu-id="2d25f-730">A method that has the `async` modifier is an async function and follows the rules described in [Async functions](classes.md#async-functions).</span></span>

<span data-ttu-id="2d25f-731">方法宣告的*return_type*會指定方法所計算和傳回的數值型別。</span><span class="sxs-lookup"><span data-stu-id="2d25f-731">The *return_type* of a method declaration specifies the type of the value computed and returned by the method.</span></span> <span data-ttu-id="2d25f-732">如果方法沒有傳回值，則會 `void` *return_type* 。</span><span class="sxs-lookup"><span data-stu-id="2d25f-732">The *return_type* is `void` if the method does not return a value.</span></span> <span data-ttu-id="2d25f-733">如果宣告包含 `partial` 修飾詞，則傳回類型必須是 `void`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-733">If the declaration includes the `partial` modifier, then the return type must be `void`.</span></span>

<span data-ttu-id="2d25f-734">*Member_name*指定方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="2d25f-734">The *member_name* specifies the name of the method.</span></span> <span data-ttu-id="2d25f-735">除非方法是明確介面成員執行（[明確介面成員](interfaces.md#explicit-interface-member-implementations)的實作為），否則*member_name*只是一個*識別碼*。</span><span class="sxs-lookup"><span data-stu-id="2d25f-735">Unless the method is an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="2d25f-736">若為明確介面成員的執行，則*member_name*包含*interface_type*後面接著 "`.`" 和*識別碼*。</span><span class="sxs-lookup"><span data-stu-id="2d25f-736">For an explicit interface member implementation, the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="2d25f-737">選擇性的*type_parameter_list*會指定方法的類型參數（[類型參數](classes.md#type-parameters)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-737">The optional *type_parameter_list* specifies the type parameters of the method ([Type parameters](classes.md#type-parameters)).</span></span> <span data-ttu-id="2d25f-738">如果指定了*type_parameter_list* ，則方法是***泛型方法***。</span><span class="sxs-lookup"><span data-stu-id="2d25f-738">If a *type_parameter_list* is specified the method is a ***generic method***.</span></span> <span data-ttu-id="2d25f-739">如果方法具有 `extern` 修飾詞，則無法指定*type_parameter_list* 。</span><span class="sxs-lookup"><span data-stu-id="2d25f-739">If the method has an `extern` modifier, a *type_parameter_list* cannot be specified.</span></span>

<span data-ttu-id="2d25f-740">選擇性的*formal_parameter_list*會指定方法的參數（[方法參數](classes.md#method-parameters)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-740">The optional *formal_parameter_list* specifies the parameters of the method ([Method parameters](classes.md#method-parameters)).</span></span>

<span data-ttu-id="2d25f-741">選擇性的*type_parameter_constraints_clause*會指定個別型別參數（[型別參數條件約束](classes.md#type-parameter-constraints)）的條件約束，而且只有在同時提供*type_parameter_list* ，而且方法沒有 `override` 修飾詞時，才可指定。</span><span class="sxs-lookup"><span data-stu-id="2d25f-741">The optional *type_parameter_constraints_clause*s specify constraints on individual type parameters ([Type parameter constraints](classes.md#type-parameter-constraints)) and may only be specified if a *type_parameter_list* is also supplied, and the method does not have an `override` modifier.</span></span>

<span data-ttu-id="2d25f-742">*Return_type*和方法的*formal_parameter_list*中所參考的每個類型，必須至少與方法本身（[存取範圍條件約束](basic-concepts.md#accessibility-constraints)）相同。</span><span class="sxs-lookup"><span data-stu-id="2d25f-742">The *return_type* and each of the types referenced in the *formal_parameter_list* of a method must be at least as accessible as the method itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="2d25f-743">*Method_body*可以是分號、***語句主體***或***運算式主體***。</span><span class="sxs-lookup"><span data-stu-id="2d25f-743">The *method_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="2d25f-744">語句主體是由*區塊*所組成，它會指定叫用方法時要執行的語句。</span><span class="sxs-lookup"><span data-stu-id="2d25f-744">A statement body consists of a *block*, which specifies the statements to execute when the method is invoked.</span></span> <span data-ttu-id="2d25f-745">運算式主體包含 `=>` 後面接著*運算式*和分號，並代表叫用方法時要執行的單一運算式。</span><span class="sxs-lookup"><span data-stu-id="2d25f-745">An expression body consists of `=>` followed by an *expression* and a semicolon, and denotes a single expression to perform when the method is invoked.</span></span> 

<span data-ttu-id="2d25f-746">對於 `abstract` 和 `extern` 方法， *method_body*只會包含一個分號。</span><span class="sxs-lookup"><span data-stu-id="2d25f-746">For `abstract` and `extern` methods, the *method_body* consists simply of a semicolon.</span></span> <span data-ttu-id="2d25f-747">針對 `partial` 方法， *method_body*可能包含分號、區塊主體或運算式主體。</span><span class="sxs-lookup"><span data-stu-id="2d25f-747">For `partial` methods the *method_body* may consist of either a semicolon, a block body or an expression body.</span></span> <span data-ttu-id="2d25f-748">對於所有其他方法， *method_body*是區塊主體或運算式主體。</span><span class="sxs-lookup"><span data-stu-id="2d25f-748">For all other methods, the *method_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="2d25f-749">如果*method_body*是由分號組成，則宣告可能不會包含 `async` 修飾詞。</span><span class="sxs-lookup"><span data-stu-id="2d25f-749">If the *method_body* consists of a semicolon, then the declaration may not include the `async` modifier.</span></span>

<span data-ttu-id="2d25f-750">方法的名稱、型別參數清單和正式參數清單會定義方法的簽章（簽章[和](basic-concepts.md#signatures-and-overloading)多載）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-750">The name, the type parameter list and the formal parameter list of a method define the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the method.</span></span> <span data-ttu-id="2d25f-751">具體而言，方法的簽章包含其名稱、型別參數的數目，以及其正式參數的數目、修飾詞和類型。</span><span class="sxs-lookup"><span data-stu-id="2d25f-751">Specifically, the signature of a method consists of its name, the number of type parameters and the number, modifiers, and types of its formal parameters.</span></span> <span data-ttu-id="2d25f-752">基於這些目的，在型式參數類型中發生之方法的任何型別參數，都不是以其名稱來識別，而是依其在方法的型別引數清單中的序數位置。傳回型別不是方法簽章的一部分，也不是型別參數或正式參數的名稱。</span><span class="sxs-lookup"><span data-stu-id="2d25f-752">For these purposes, any type parameter of the method that occurs in the type of a formal parameter is identified not by its name, but by its ordinal position in the type argument list of the method.The return type is not part of a method's signature, nor are the names of the type parameters or the formal parameters.</span></span>

<span data-ttu-id="2d25f-753">方法的名稱必須與在相同類別中宣告之所有其他非方法的名稱不同。</span><span class="sxs-lookup"><span data-stu-id="2d25f-753">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="2d25f-754">此外，方法的簽章必須與相同類別中宣告之所有其他方法的簽章不同，而且在相同類別中宣告的兩個方法，不能有不同于 `ref` 和 `out`的簽章。</span><span class="sxs-lookup"><span data-stu-id="2d25f-754">In addition, the signature of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>

<span data-ttu-id="2d25f-755">方法的*type_parameter*在整個*method_declaration*的範圍內，而且可用來在*return_type*、 *method_body*和*type_parameter_constraints_clause*中的整個範圍內形成類型，但不能用在*屬性*中。</span><span class="sxs-lookup"><span data-stu-id="2d25f-755">The method's *type_parameter*s are in scope throughout the *method_declaration*, and can be used to form types throughout that scope in *return_type*, *method_body*, and *type_parameter_constraints_clause*s but not in *attributes*.</span></span>

<span data-ttu-id="2d25f-756">所有正式參數和類型參數都必須有不同的名稱。</span><span class="sxs-lookup"><span data-stu-id="2d25f-756">All formal parameters and type parameters must have different names.</span></span>

### <a name="method-parameters"></a><span data-ttu-id="2d25f-757">方法參數</span><span class="sxs-lookup"><span data-stu-id="2d25f-757">Method parameters</span></span>

<span data-ttu-id="2d25f-758">方法的參數（如果有的話）會由方法的*formal_parameter_list*宣告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-758">The parameters of a method, if any, are declared by the method's *formal_parameter_list*.</span></span>

```antlr
formal_parameter_list
    : fixed_parameters
    | fixed_parameters ',' parameter_array
    | parameter_array
    ;

fixed_parameters
    : fixed_parameter (',' fixed_parameter)*
    ;

fixed_parameter
    : attributes? parameter_modifier? type identifier default_argument?
    ;

default_argument
    : '=' expression
    ;

parameter_modifier
    : 'ref'
    | 'out'
    | 'this'
    ;

parameter_array
    : attributes? 'params' array_type identifier
    ;
```

<span data-ttu-id="2d25f-759">型式參數清單是由一或多個以逗號分隔的參數所組成，其中只有最後一個可以是*parameter_array*。</span><span class="sxs-lookup"><span data-stu-id="2d25f-759">The formal parameter list consists of one or more comma-separated parameters of which only the last may be a *parameter_array*.</span></span>

<span data-ttu-id="2d25f-760">*Fixed_parameter*由一組選擇性的*屬性*（[屬性](attributes.md)）、選擇性的 `ref`、`out` 或 `this` 修飾詞、*類型*、*識別碼*和選擇性*default_argument*所組成。</span><span class="sxs-lookup"><span data-stu-id="2d25f-760">A *fixed_parameter* consists of an optional set of *attributes* ([Attributes](attributes.md)), an optional `ref`, `out` or `this` modifier, a *type*, an *identifier* and an optional *default_argument*.</span></span> <span data-ttu-id="2d25f-761">每個*fixed_parameter*都會宣告具有指定名稱之給定類型的參數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-761">Each *fixed_parameter* declares a parameter of the given type with the given name.</span></span> <span data-ttu-id="2d25f-762">`this` 修飾詞將方法指定為擴充方法，而且只允許用於靜態方法的第一個參數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-762">The `this` modifier designates the method as an extension method and is only allowed on the first parameter of a static method.</span></span> <span data-ttu-id="2d25f-763">擴充方法會進一步說明擴充[方法。](classes.md#extension-methods)</span><span class="sxs-lookup"><span data-stu-id="2d25f-763">Extension methods are further described in [Extension methods](classes.md#extension-methods).</span></span>

<span data-ttu-id="2d25f-764">具有*default_argument*的*fixed_parameter*稱為「***選擇性參數***」，而不含*default_argument*的*fixed_parameter*則是***必要參數***。</span><span class="sxs-lookup"><span data-stu-id="2d25f-764">A *fixed_parameter* with a *default_argument* is known as an ***optional parameter***, whereas a *fixed_parameter* without a *default_argument* is a ***required parameter***.</span></span> <span data-ttu-id="2d25f-765">必要參數可能不會出現在*formal_parameter_list*中的選擇性參數之後。</span><span class="sxs-lookup"><span data-stu-id="2d25f-765">A required parameter may not appear after an optional parameter in a *formal_parameter_list*.</span></span>

<span data-ttu-id="2d25f-766">`ref` 或 `out` 參數不能有*default_argument*。</span><span class="sxs-lookup"><span data-stu-id="2d25f-766">A `ref` or `out` parameter cannot have a *default_argument*.</span></span> <span data-ttu-id="2d25f-767">*Default_argument*中的*運算式*必須是下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="2d25f-767">The *expression* in a *default_argument* must be one of the following:</span></span>

*  <span data-ttu-id="2d25f-768">*constant_expression*</span><span class="sxs-lookup"><span data-stu-id="2d25f-768">a *constant_expression*</span></span>
*  <span data-ttu-id="2d25f-769">表單 `new S()` 的運算式，其中 `S` 是實數值型別</span><span class="sxs-lookup"><span data-stu-id="2d25f-769">an expression of the form `new S()` where `S` is a value type</span></span>
*  <span data-ttu-id="2d25f-770">表單 `default(S)` 的運算式，其中 `S` 是實數值型別</span><span class="sxs-lookup"><span data-stu-id="2d25f-770">an expression of the form `default(S)` where `S` is a value type</span></span>

<span data-ttu-id="2d25f-771">*運算式*必須以身分識別或可為 null 的轉換為參數的類型，以隱含方式轉換。</span><span class="sxs-lookup"><span data-stu-id="2d25f-771">The *expression* must be implicitly convertible by an identity or nullable conversion to the type of the parameter.</span></span>

<span data-ttu-id="2d25f-772">如果在執行部分方法宣告（[部分方法](classes.md#partial-methods)）中發生選擇性參數，則明確的介面成員執行（[明確介面成員](interfaces.md#explicit-interface-member-implementations)實作為），或在單一參數索引子宣告（[索引子](classes.md#indexers)）中，編譯器應該會發出警告，因為這些成員絕對無法以允許省略引數的方式叫用。</span><span class="sxs-lookup"><span data-stu-id="2d25f-772">If optional parameters occur in an implementing partial method declaration ([Partial methods](classes.md#partial-methods)) , an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)) or in a single-parameter indexer declaration ([Indexers](classes.md#indexers)) the compiler should give a warning, since these members can never be invoked in a way that permits arguments to be omitted.</span></span>

<span data-ttu-id="2d25f-773">*Parameter_array*由一組選擇性的*屬性*（[屬性](attributes.md)）、`params` 修飾詞、 *array_type*和*識別碼*所組成。</span><span class="sxs-lookup"><span data-stu-id="2d25f-773">A *parameter_array* consists of an optional set of *attributes* ([Attributes](attributes.md)), a `params` modifier, an *array_type*, and an *identifier*.</span></span> <span data-ttu-id="2d25f-774">參數陣列會宣告具有指定名稱之指定陣列類型的單一參數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-774">A parameter array declares a single parameter of the given array type with the given name.</span></span> <span data-ttu-id="2d25f-775">參數陣列的*array_type*必須是一維陣列類型（[陣列類型](arrays.md#array-types)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-775">The *array_type* of a parameter array must be a single-dimensional array type ([Array types](arrays.md#array-types)).</span></span> <span data-ttu-id="2d25f-776">在方法調用中，參數陣列允許指定給定陣列類型的單一引數，或允許指定陣列元素類型的零個或多個引數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-776">In a method invocation, a parameter array permits either a single argument of the given array type to be specified, or it permits zero or more arguments of the array element type to be specified.</span></span> <span data-ttu-id="2d25f-777">參數陣列中會進一步描述參數[陣列。](classes.md#parameter-arrays)</span><span class="sxs-lookup"><span data-stu-id="2d25f-777">Parameter arrays are described further in [Parameter arrays](classes.md#parameter-arrays).</span></span>

<span data-ttu-id="2d25f-778">*Parameter_array*可能會在選擇性參數之後發生，但不能有預設值--省略*parameter_array*的引數會改為建立空陣列。</span><span class="sxs-lookup"><span data-stu-id="2d25f-778">A *parameter_array* may occur after an optional parameter, but cannot have a default value -- the omission of arguments for a *parameter_array* would instead result in the creation of an empty array.</span></span>

<span data-ttu-id="2d25f-779">下列範例說明不同類型的參數：</span><span class="sxs-lookup"><span data-stu-id="2d25f-779">The following example illustrates different kinds of parameters:</span></span>
```csharp
public void M(
    ref int      i,
    decimal      d,
    bool         b = false,
    bool?        n = false,
    string       s = "Hello",
    object       o = null,
    T            t = default(T),
    params int[] a
) { }
```

<span data-ttu-id="2d25f-780">在 `M`的*formal_parameter_list*中，`i` 是必要的 ref 參數，`d` 是必要的值參數，`b`，`s`，`o` 和 `t` 是選擇性的值參數，而 `a` 是參數陣列。</span><span class="sxs-lookup"><span data-stu-id="2d25f-780">In the *formal_parameter_list* for `M`, `i` is a required ref parameter, `d` is a required value parameter, `b`, `s`, `o` and `t` are optional value parameters and `a` is a parameter array.</span></span>

<span data-ttu-id="2d25f-781">方法宣告會為參數、型別參數和區域變數建立個別的宣告空間。</span><span class="sxs-lookup"><span data-stu-id="2d25f-781">A method declaration creates a separate declaration space for parameters, type parameters and local variables.</span></span> <span data-ttu-id="2d25f-782">名稱會由方法的類型參數清單和方法的型式參數清單和方法的*區塊*中的區域變數宣告，引進此宣告空間中。</span><span class="sxs-lookup"><span data-stu-id="2d25f-782">Names are introduced into this declaration space by the type parameter list and the formal parameter list of the method and by local variable declarations in the *block* of the method.</span></span> <span data-ttu-id="2d25f-783">方法宣告空間的兩個成員都有相同的名稱，這是一項錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-783">It is an error for two members of a method declaration space to have the same name.</span></span> <span data-ttu-id="2d25f-784">方法宣告空間和嵌套宣告空間的區域變數宣告空間，包含具有相同名稱的專案，這是一項錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-784">It is an error for the method declaration space and the local variable declaration space of a nested declaration space to contain elements with the same name.</span></span>

<span data-ttu-id="2d25f-785">方法調用（[方法](expressions.md#method-invocations)叫用）會建立該調用的特定複本，這是方法的型式參數和區域變數的本機變數，而叫用的引數清單會將值或變數參考指派給新建立的正式參數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-785">A method invocation ([Method invocations](expressions.md#method-invocations)) creates a copy, specific to that invocation, of the formal parameters and local variables of the method, and the argument list of the invocation assigns values or variable references to the newly created formal parameters.</span></span> <span data-ttu-id="2d25f-786">在方法的*區塊*內，型式參數可由其在*Simple_name*運算式（[簡單名稱](expressions.md#simple-names)）中的識別碼來參考。</span><span class="sxs-lookup"><span data-stu-id="2d25f-786">Within the *block* of a method, formal parameters can be referenced by their identifiers in *simple_name* expressions ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="2d25f-787">型式參數共有四種：</span><span class="sxs-lookup"><span data-stu-id="2d25f-787">There are four kinds of formal parameters:</span></span>

*  <span data-ttu-id="2d25f-788">未宣告任何修飾詞的值參數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-788">Value parameters, which are declared without any modifiers.</span></span>
*  <span data-ttu-id="2d25f-789">參考參數，使用 `ref` 修飾詞進行宣告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-789">Reference parameters, which are declared with the `ref` modifier.</span></span>
*  <span data-ttu-id="2d25f-790">輸出參數，使用 `out` 修飾詞進行宣告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-790">Output parameters, which are declared with the `out` modifier.</span></span>
*  <span data-ttu-id="2d25f-791">參數陣列，使用 `params` 修飾詞進行宣告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-791">Parameter arrays, which are declared with the `params` modifier.</span></span>

<span data-ttu-id="2d25f-792">如簽章[和](basic-concepts.md#signatures-and-overloading)多載中所述，`ref` 和 `out` 修飾詞都是方法簽章的一部分，但 `params` 修飾詞則不是。</span><span class="sxs-lookup"><span data-stu-id="2d25f-792">As described in [Signatures and overloading](basic-concepts.md#signatures-and-overloading), the `ref` and `out` modifiers are part of a method's signature, but the `params` modifier is not.</span></span>

#### <a name="value-parameters"></a><span data-ttu-id="2d25f-793">值參數</span><span class="sxs-lookup"><span data-stu-id="2d25f-793">Value parameters</span></span>

<span data-ttu-id="2d25f-794">以 no 修飾詞宣告的參數是值參數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-794">A parameter declared with no modifiers is a value parameter.</span></span> <span data-ttu-id="2d25f-795">值參數會對應至本機變數，以從方法調用中提供的對應引數取得其初始值。</span><span class="sxs-lookup"><span data-stu-id="2d25f-795">A value parameter corresponds to a local variable that gets its initial value from the corresponding argument supplied in the method invocation.</span></span>

<span data-ttu-id="2d25f-796">當型式參數是值參數時，方法調用中的對應引數必須是可隱含轉換（[隱含轉換](conversions.md#implicit-conversions)）為正式參數類型的運算式。</span><span class="sxs-lookup"><span data-stu-id="2d25f-796">When a formal parameter is a value parameter, the corresponding argument in a method invocation must be an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the formal parameter type.</span></span>

<span data-ttu-id="2d25f-797">允許方法將新值指派給值參數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-797">A method is permitted to assign new values to a value parameter.</span></span> <span data-ttu-id="2d25f-798">這類指派只會影響值參數所代表的本機儲存位置，而不會影響在方法調用中指定的實際引數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-798">Such assignments only affect the local storage location represented by the value parameter—they have no effect on the actual argument given in the method invocation.</span></span>

#### <a name="reference-parameters"></a><span data-ttu-id="2d25f-799">傳址參數</span><span class="sxs-lookup"><span data-stu-id="2d25f-799">Reference parameters</span></span>

<span data-ttu-id="2d25f-800">使用 `ref` 修飾詞宣告的參數是參考參數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-800">A parameter declared with a `ref` modifier is a reference parameter.</span></span> <span data-ttu-id="2d25f-801">與值參數不同的是，參考參數不會建立新的儲存位置。</span><span class="sxs-lookup"><span data-stu-id="2d25f-801">Unlike a value parameter, a reference parameter does not create a new storage location.</span></span> <span data-ttu-id="2d25f-802">相反地，參考參數代表的儲存位置與指定為方法調用中引數的變數相同。</span><span class="sxs-lookup"><span data-stu-id="2d25f-802">Instead, a reference parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="2d25f-803">當型式參數是參考參數時，方法調用中的對應引數必須包含關鍵字 `ref` 後面接著*variable_reference* （[判斷明確指派的精確規則](variables.md#precise-rules-for-determining-definite-assignment)），其類型與正式參數相同。</span><span class="sxs-lookup"><span data-stu-id="2d25f-803">When a formal parameter is a reference parameter, the corresponding argument in a method invocation must consist of the keyword `ref` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="2d25f-804">必須先明確指派變數，才可以將它當做參考參數傳遞。</span><span class="sxs-lookup"><span data-stu-id="2d25f-804">A variable must be definitely assigned before it can be passed as a reference parameter.</span></span>

<span data-ttu-id="2d25f-805">在方法內，一律會將參考參數視為明確指派。</span><span class="sxs-lookup"><span data-stu-id="2d25f-805">Within a method, a reference parameter is always considered definitely assigned.</span></span>

<span data-ttu-id="2d25f-806">宣告為 iterator （[反覆運算](classes.md#iterators)器）的方法不能有參考參數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-806">A method declared as an iterator ([Iterators](classes.md#iterators)) cannot have reference parameters.</span></span>

<span data-ttu-id="2d25f-807">範例</span><span class="sxs-lookup"><span data-stu-id="2d25f-807">The example</span></span>
```csharp
using System;

class Test
{
    static void Swap(ref int x, ref int y) {
        int temp = x;
        x = y;
        y = temp;
    }

    static void Main() {
        int i = 1, j = 2;
        Swap(ref i, ref j);
        Console.WriteLine("i = {0}, j = {1}", i, j);
    }
}
```
<span data-ttu-id="2d25f-808">產生下列輸出</span><span class="sxs-lookup"><span data-stu-id="2d25f-808">produces the output</span></span>
```console
i = 2, j = 1
```

<span data-ttu-id="2d25f-809">針對 `Main`中的 `Swap` 調用，`x` 代表 `i`，而 `y` 代表 `j`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-809">For the invocation of `Swap` in `Main`, `x` represents `i` and `y` represents `j`.</span></span> <span data-ttu-id="2d25f-810">因此，叫用會影響 `i` 和 `j`的值交換。</span><span class="sxs-lookup"><span data-stu-id="2d25f-810">Thus, the invocation has the effect of swapping the values of `i` and `j`.</span></span>

<span data-ttu-id="2d25f-811">在採用參考參數的方法中，有可能會有多個名稱代表相同的儲存位置。</span><span class="sxs-lookup"><span data-stu-id="2d25f-811">In a method that takes reference parameters it is possible for multiple names to represent the same storage location.</span></span> <span data-ttu-id="2d25f-812">在範例中</span><span class="sxs-lookup"><span data-stu-id="2d25f-812">In the example</span></span>
```csharp
class A
{
    string s;

    void F(ref string a, ref string b) {
        s = "One";
        a = "Two";
        b = "Three";
    }

    void G() {
        F(ref s, ref s);
    }
}
```
<span data-ttu-id="2d25f-813">`G` 中的 `F` 的調用會將 `a` 和 `b`的參考傳遞給 `s`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-813">the invocation of `F` in `G` passes a reference to `s` for both `a` and `b`.</span></span> <span data-ttu-id="2d25f-814">因此，針對該叫用，`s`、`a`和 `b` 的名稱全都參考相同的儲存位置，而三個指派全都會修改實例欄位 `s`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-814">Thus, for that invocation, the names `s`, `a`, and `b` all refer to the same storage location, and the three assignments all modify the instance field `s`.</span></span>

#### <a name="output-parameters"></a><span data-ttu-id="2d25f-815">輸出參數</span><span class="sxs-lookup"><span data-stu-id="2d25f-815">Output parameters</span></span>

<span data-ttu-id="2d25f-816">使用 `out` 修飾詞宣告的參數是輸出參數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-816">A parameter declared with an `out` modifier is an output parameter.</span></span> <span data-ttu-id="2d25f-817">類似于參考參數，輸出參數不會建立新的儲存位置。</span><span class="sxs-lookup"><span data-stu-id="2d25f-817">Similar to a reference parameter, an output parameter does not create a new storage location.</span></span> <span data-ttu-id="2d25f-818">相反地，output 參數代表的儲存位置與指定為方法調用中引數的變數相同。</span><span class="sxs-lookup"><span data-stu-id="2d25f-818">Instead, an output parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="2d25f-819">當型式參數是輸出參數時，方法調用中的對應引數必須包含關鍵字 `out` 後面接著*variable_reference* （[判斷明確指派的精確規則](variables.md#precise-rules-for-determining-definite-assignment)），其類型與正式參數相同。</span><span class="sxs-lookup"><span data-stu-id="2d25f-819">When a formal parameter is an output parameter, the corresponding argument in a method invocation must consist of the keyword `out` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="2d25f-820">變數不一定要先被指派，才可以做為輸出參數來傳遞，但在將變數當做輸出參數傳遞的叫用之後，會將變數視為明確指派。</span><span class="sxs-lookup"><span data-stu-id="2d25f-820">A variable need not be definitely assigned before it can be passed as an output parameter, but following an invocation where a variable was passed as an output parameter, the variable is considered definitely assigned.</span></span>

<span data-ttu-id="2d25f-821">在方法內，如同本機變數，輸出參數一開始會被視為未指派，而且必須在使用其值之前明確指派。</span><span class="sxs-lookup"><span data-stu-id="2d25f-821">Within a method, just like a local variable, an output parameter is initially considered unassigned and must be definitely assigned before its value is used.</span></span>

<span data-ttu-id="2d25f-822">方法的每個輸出參數都必須在方法傳回之前明確指派。</span><span class="sxs-lookup"><span data-stu-id="2d25f-822">Every output parameter of a method must be definitely assigned before the method returns.</span></span>

<span data-ttu-id="2d25f-823">宣告為部分方法（[部分方法](classes.md#partial-methods)）或 Iterator （[反覆運算](classes.md#iterators)器）的方法不能有輸出參數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-823">A method declared as a partial method ([Partial methods](classes.md#partial-methods)) or an iterator ([Iterators](classes.md#iterators)) cannot have output parameters.</span></span>

<span data-ttu-id="2d25f-824">輸出參數通常用於產生多個傳回值的方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-824">Output parameters are typically used in methods that produce multiple return values.</span></span> <span data-ttu-id="2d25f-825">例如：</span><span class="sxs-lookup"><span data-stu-id="2d25f-825">For example:</span></span>
```csharp
using System;

class Test
{
    static void SplitPath(string path, out string dir, out string name) {
        int i = path.Length;
        while (i > 0) {
            char ch = path[i - 1];
            if (ch == '\\' || ch == '/' || ch == ':') break;
            i--;
        }
        dir = path.Substring(0, i);
        name = path.Substring(i);
    }

    static void Main() {
        string dir, name;
        SplitPath("c:\\Windows\\System\\hello.txt", out dir, out name);
        Console.WriteLine(dir);
        Console.WriteLine(name);
    }
}
```

<span data-ttu-id="2d25f-826">此範例會產生輸出：</span><span class="sxs-lookup"><span data-stu-id="2d25f-826">The example produces the output:</span></span>
```console
c:\Windows\System\
hello.txt
```

<span data-ttu-id="2d25f-827">請注意，`dir` 和 `name` 變數可以在傳遞至 `SplitPath`之前解除指派，並在呼叫之後將其視為明確指派。</span><span class="sxs-lookup"><span data-stu-id="2d25f-827">Note that the `dir` and `name` variables can be unassigned before they are passed to `SplitPath`, and that they are considered definitely assigned following the call.</span></span>

#### <a name="parameter-arrays"></a><span data-ttu-id="2d25f-828">參數陣列</span><span class="sxs-lookup"><span data-stu-id="2d25f-828">Parameter arrays</span></span>

<span data-ttu-id="2d25f-829">使用 `params` 修飾詞宣告的參數是參數陣列。</span><span class="sxs-lookup"><span data-stu-id="2d25f-829">A parameter declared with a `params` modifier is a parameter array.</span></span> <span data-ttu-id="2d25f-830">如果正式參數清單包含參數陣列，它必須是清單中的最後一個參數，而且必須是一維陣列類型。</span><span class="sxs-lookup"><span data-stu-id="2d25f-830">If a formal parameter list includes a parameter array, it must be the last parameter in the list and it must be of a single-dimensional array type.</span></span> <span data-ttu-id="2d25f-831">例如，`string[]` 和 `string[][]` 的類型可用來做為參數陣列的類型，但類型 `string[,]` 則不能使用。</span><span class="sxs-lookup"><span data-stu-id="2d25f-831">For example, the types `string[]` and `string[][]` can be used as the type of a parameter array, but the type `string[,]` can not.</span></span> <span data-ttu-id="2d25f-832">您無法將 `params` 修飾詞結合 `ref` 和 `out`的修飾詞。</span><span class="sxs-lookup"><span data-stu-id="2d25f-832">It is not possible to combine the `params` modifier with the modifiers `ref` and `out`.</span></span>

<span data-ttu-id="2d25f-833">參數陣列允許在方法調用中以兩種方式之一來指定引數：</span><span class="sxs-lookup"><span data-stu-id="2d25f-833">A parameter array permits arguments to be specified in one of two ways in a method invocation:</span></span>

*  <span data-ttu-id="2d25f-834">為參數陣列提供的引數可以是可隱含轉換（[隱含轉換](conversions.md#implicit-conversions)）為參數陣列類型的單一運算式。</span><span class="sxs-lookup"><span data-stu-id="2d25f-834">The argument given for a parameter array can be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="2d25f-835">在此情況下，參數陣列的作用會與值參數完全相同。</span><span class="sxs-lookup"><span data-stu-id="2d25f-835">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="2d25f-836">或者，叫用可以為參數陣列指定零個或多個引數，其中每個引數都是隱含轉換（[隱含轉換](conversions.md#implicit-conversions)）為參數陣列之元素類型的運算式。</span><span class="sxs-lookup"><span data-stu-id="2d25f-836">Alternatively, the invocation can specify zero or more arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="2d25f-837">在此情況下，調用會使用對應引數數目的長度來建立參數陣列類型的實例、使用指定的引數值初始化陣列實例的元素，並使用新建立的陣列實例作為實際的引數.</span><span class="sxs-lookup"><span data-stu-id="2d25f-837">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="2d25f-838">除了允許在調用中使用不定數目的引數，參數陣列也會精確地等同于相同類型的值參數（[值](classes.md#value-parameters)參數）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-838">Except for allowing a variable number of arguments in an invocation, a parameter array is precisely equivalent to a value parameter ([Value parameters](classes.md#value-parameters)) of the same type.</span></span>

<span data-ttu-id="2d25f-839">範例</span><span class="sxs-lookup"><span data-stu-id="2d25f-839">The example</span></span>
```csharp
using System;

class Test
{
    static void F(params int[] args) {
        Console.Write("Array contains {0} elements:", args.Length);
        foreach (int i in args) 
            Console.Write(" {0}", i);
        Console.WriteLine();
    }

    static void Main() {
        int[] arr = {1, 2, 3};
        F(arr);
        F(10, 20, 30, 40);
        F();
    }
}
```
<span data-ttu-id="2d25f-840">產生下列輸出</span><span class="sxs-lookup"><span data-stu-id="2d25f-840">produces the output</span></span>
```console
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

<span data-ttu-id="2d25f-841">`F` 的第一個調用只會將陣列 `a` 當做值參數傳遞。</span><span class="sxs-lookup"><span data-stu-id="2d25f-841">The first invocation of `F` simply passes the array `a` as a value parameter.</span></span> <span data-ttu-id="2d25f-842">`F` 的第二個調用會自動以指定的元素值建立四個元素的 `int[]`，並將該陣列實例當做值參數傳遞。</span><span class="sxs-lookup"><span data-stu-id="2d25f-842">The second invocation of `F` automatically creates a four-element `int[]` with the given element values and passes that array instance as a value parameter.</span></span> <span data-ttu-id="2d25f-843">同樣地，`F` 的第三個調用會建立零元素 `int[]`，並將該實例當做值參數傳遞。</span><span class="sxs-lookup"><span data-stu-id="2d25f-843">Likewise, the third invocation of `F` creates a zero-element `int[]` and passes that instance as a value parameter.</span></span> <span data-ttu-id="2d25f-844">第二個和第三個調用相當於撰寫：</span><span class="sxs-lookup"><span data-stu-id="2d25f-844">The second and third invocations are precisely equivalent to writing:</span></span>
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

<span data-ttu-id="2d25f-845">執行多載解析時，具有參數陣列的方法可能會以其一般格式或擴充形式（適用函式[成員](expressions.md#applicable-function-member)）的形式適用。</span><span class="sxs-lookup"><span data-stu-id="2d25f-845">When performing overload resolution, a method with a parameter array may be applicable either in its normal form or in its expanded form ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="2d25f-846">只有當方法的一般形式不適用時，才可以使用方法的展開形式，而且只有在相同類型中尚未宣告與展開表單具有相同簽章的適用方法時，才會提供。</span><span class="sxs-lookup"><span data-stu-id="2d25f-846">The expanded form of a method is available only if the normal form of the method is not applicable and only if an applicable method with the same signature as the expanded form is not already declared in the same type.</span></span>

<span data-ttu-id="2d25f-847">範例</span><span class="sxs-lookup"><span data-stu-id="2d25f-847">The example</span></span>
```csharp
using System;

class Test
{
    static void F(params object[] a) {
        Console.WriteLine("F(object[])");
    }

    static void F() {
        Console.WriteLine("F()");
    }

    static void F(object a0, object a1) {
        Console.WriteLine("F(object,object)");
    }

    static void Main() {
        F();
        F(1);
        F(1, 2);
        F(1, 2, 3);
        F(1, 2, 3, 4);
    }
}
```
<span data-ttu-id="2d25f-848">產生下列輸出</span><span class="sxs-lookup"><span data-stu-id="2d25f-848">produces the output</span></span>
```console
F();
F(object[]);
F(object,object);
F(object[]);
F(object[]);
```

<span data-ttu-id="2d25f-849">在此範例中，具有參數陣列之方法的兩個可能擴充形式已包含在類別中，做為一般方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-849">In the example, two of the possible expanded forms of the method with a parameter array are already included in the class as regular methods.</span></span> <span data-ttu-id="2d25f-850">因此，在執行多載解析時，不會考慮這些展開的表單，而第一個和第三個方法調用會因此選取一般方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-850">These expanded forms are therefore not considered when performing overload resolution, and the first and third method invocations thus select the regular methods.</span></span> <span data-ttu-id="2d25f-851">當類別宣告具有參數陣列的方法時，通常也會將部分擴充形式納入為一般方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-851">When a class declares a method with a parameter array, it is not uncommon to also include some of the expanded forms as regular methods.</span></span> <span data-ttu-id="2d25f-852">如此一來，就可以在叫用具有參數陣列的方法擴充形式時，避免配置所發生的陣列實例。</span><span class="sxs-lookup"><span data-stu-id="2d25f-852">By doing so it is possible to avoid the allocation of an array instance that occurs when an expanded form of a method with a parameter array is invoked.</span></span>

<span data-ttu-id="2d25f-853">當參數陣列的類型是 `object[]`時，方法的一般形式和單一 `object` 參數所花費的格式可能會有不明確的情況。</span><span class="sxs-lookup"><span data-stu-id="2d25f-853">When the type of a parameter array is `object[]`, a potential ambiguity arises between the normal form of the method and the expended form for a single `object` parameter.</span></span> <span data-ttu-id="2d25f-854">不明確的原因是 `object[]` 本身會隱含地轉換成類型 `object`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-854">The reason for the ambiguity is that an `object[]` is itself implicitly convertible to type `object`.</span></span> <span data-ttu-id="2d25f-855">但不明確呈現問題，因為它可以在需要時插入轉換來加以解決。</span><span class="sxs-lookup"><span data-stu-id="2d25f-855">The ambiguity presents no problem, however, since it can be resolved by inserting a cast if needed.</span></span>

<span data-ttu-id="2d25f-856">範例</span><span class="sxs-lookup"><span data-stu-id="2d25f-856">The example</span></span>
```csharp
using System;

class Test
{
    static void F(params object[] args) {
        foreach (object o in args) {
            Console.Write(o.GetType().FullName);
            Console.Write(" ");
        }
        Console.WriteLine();
    }

    static void Main() {
        object[] a = {1, "Hello", 123.456};
        object o = a;
        F(a);
        F((object)a);
        F(o);
        F((object[])o);
    }
}
```
<span data-ttu-id="2d25f-857">產生下列輸出</span><span class="sxs-lookup"><span data-stu-id="2d25f-857">produces the output</span></span>
```console
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

<span data-ttu-id="2d25f-858">在 `F`的第一個和最後一個調用中，`F` 的一般形式是適用的，因為從引數類型到參數類型的隱含轉換（兩者都是 `object[]`的類型）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-858">In the first and last invocations of `F`, the normal form of `F` is applicable because an implicit conversion exists from the argument type to the parameter type (both are of type `object[]`).</span></span> <span data-ttu-id="2d25f-859">因此，多載解析會選取 `F`的一般形式，而引數會當做一般值參數傳遞。</span><span class="sxs-lookup"><span data-stu-id="2d25f-859">Thus, overload resolution selects the normal form of `F`, and the argument is passed as a regular value parameter.</span></span> <span data-ttu-id="2d25f-860">在第二個和第三個調用中，一般的 `F` 形式並不適用，因為引數類型到參數類型的隱含轉換不是（類型 `object` 無法隱含地轉換成類型 `object[]`）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-860">In the second and third invocations, the normal form of `F` is not applicable because no implicit conversion exists from the argument type to the parameter type (type `object` cannot be implicitly converted to type `object[]`).</span></span> <span data-ttu-id="2d25f-861">不過，`F` 的擴充形式是適用的，因此會由多載解析選取。</span><span class="sxs-lookup"><span data-stu-id="2d25f-861">However, the expanded form of `F` is applicable, so it is selected by overload resolution.</span></span> <span data-ttu-id="2d25f-862">因此，呼叫會建立一個專案 `object[]`，而陣列的單一元素會以指定的引數值（其本身是 `object[]`的參考）進行初始化。</span><span class="sxs-lookup"><span data-stu-id="2d25f-862">As a result, a one-element `object[]` is created by the invocation, and the single element of the array is initialized with the given argument value (which itself is a reference to an `object[]`).</span></span>

### <a name="static-and-instance-methods"></a><span data-ttu-id="2d25f-863">靜態和執行個體方法</span><span class="sxs-lookup"><span data-stu-id="2d25f-863">Static and instance methods</span></span>

<span data-ttu-id="2d25f-864">當方法宣告包含 `static` 修飾詞時，該方法會被視為靜態方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-864">When a method declaration includes a `static` modifier, that method is said to be a static method.</span></span> <span data-ttu-id="2d25f-865">當沒有 `static` 的修飾詞時，此方法會被視為實例方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-865">When no `static` modifier is present, the method is said to be an instance method.</span></span>

<span data-ttu-id="2d25f-866">靜態方法不會在特定的實例上運作，而是編譯時期錯誤，以在靜態方法中參考 `this`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-866">A static method does not operate on a specific instance, and it is a compile-time error to refer to `this` in a static method.</span></span>

<span data-ttu-id="2d25f-867">實例方法會在類別的指定實例上運作，而且該實例可以當做 `this` （[此存取](expressions.md#this-access)）來存取。</span><span class="sxs-lookup"><span data-stu-id="2d25f-867">An instance method operates on a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="2d25f-868">當方法在表單 `E.M`的*member_access* （[成員存取](expressions.md#member-access)）中被參考時，如果 `M` 是靜態方法，`E` 必須代表包含 `M`的類型，而且如果 `M` 是實例方法，則 `E` 必須表示包含 `M`之類型的實例。</span><span class="sxs-lookup"><span data-stu-id="2d25f-868">When a method is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static method, `E` must denote a type containing `M`, and if `M` is an instance method, `E` must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="2d25f-869">靜態和實例成員之間的差異會在[靜態和實例成員](classes.md#static-and-instance-members)中進一步討論。</span><span class="sxs-lookup"><span data-stu-id="2d25f-869">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-methods"></a><span data-ttu-id="2d25f-870">虛擬方法</span><span class="sxs-lookup"><span data-stu-id="2d25f-870">Virtual methods</span></span>

<span data-ttu-id="2d25f-871">當實例方法宣告包含 `virtual` 修飾詞時，該方法會被視為虛擬方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-871">When an instance method declaration includes a `virtual` modifier, that method is said to be a virtual method.</span></span> <span data-ttu-id="2d25f-872">當沒有 `virtual` 的修飾詞時，方法就稱為非虛擬方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-872">When no `virtual` modifier is present, the method is said to be a non-virtual method.</span></span>

<span data-ttu-id="2d25f-873">非虛擬方法的實作為不變：不論方法是在宣告它的類別實例或衍生類別的實例上叫用，都是一樣的。</span><span class="sxs-lookup"><span data-stu-id="2d25f-873">The implementation of a non-virtual method is invariant: The implementation is the same whether the method is invoked on an instance of the class in which it is declared or an instance of a derived class.</span></span> <span data-ttu-id="2d25f-874">相反地，衍生類別可以取代虛擬方法的執行。</span><span class="sxs-lookup"><span data-stu-id="2d25f-874">In contrast, the implementation of a virtual method can be superseded by derived classes.</span></span> <span data-ttu-id="2d25f-875">取代繼承虛擬方法的執行程式，稱為覆***寫***該方法（覆[寫方法](classes.md#override-methods)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-875">The process of superseding the implementation of an inherited virtual method is known as ***overriding*** that method ([Override methods](classes.md#override-methods)).</span></span>

<span data-ttu-id="2d25f-876">在虛擬方法調用中，叫用的實例的***執行時間類型***會決定要叫用的實際方法執行。</span><span class="sxs-lookup"><span data-stu-id="2d25f-876">In a virtual method invocation, the ***run-time type*** of the instance for which that invocation takes place determines the actual method implementation to invoke.</span></span> <span data-ttu-id="2d25f-877">在非虛擬方法調用中，實例的***編譯階段類型***是決定因素。</span><span class="sxs-lookup"><span data-stu-id="2d25f-877">In a non-virtual method invocation, the ***compile-time type*** of the instance is the determining factor.</span></span> <span data-ttu-id="2d25f-878">具體而言，當使用引數清單叫用名為 `N` 的方法時，`A` 在具有編譯時間類型 `C` 和執行時間類型 `R` （其中 `R` 是 `C` 或衍生自 `C`的類別）的實例上，則會以下列方式處理調用：</span><span class="sxs-lookup"><span data-stu-id="2d25f-878">In precise terms, when a method named `N` is invoked with an argument list `A` on an instance with a compile-time type `C` and a run-time type `R` (where `R` is either `C` or a class derived from `C`), the invocation is processed as follows:</span></span>

*  <span data-ttu-id="2d25f-879">首先，多載解析會套用至 `C`、`N`和 `A`，以從 `C`所宣告並繼承的方法集合中選取特定的方法 `M`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-879">First, overload resolution is applied to `C`, `N`, and `A`, to select a specific method `M` from the set of methods declared in and inherited by `C`.</span></span> <span data-ttu-id="2d25f-880">這會在[方法調用](expressions.md#method-invocations)中說明。</span><span class="sxs-lookup"><span data-stu-id="2d25f-880">This is described in [Method invocations](expressions.md#method-invocations).</span></span>
*  <span data-ttu-id="2d25f-881">然後，如果 `M` 是非虛擬方法，就會叫用 `M`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-881">Then, if `M` is a non-virtual method, `M` is invoked.</span></span>
*  <span data-ttu-id="2d25f-882">否則，`M` 是虛擬方法，而且會叫用與 `R` 有關的 `M` 最衍生的實。</span><span class="sxs-lookup"><span data-stu-id="2d25f-882">Otherwise, `M` is a virtual method, and the most derived implementation of `M` with respect to `R` is invoked.</span></span>

<span data-ttu-id="2d25f-883">針對類別所宣告或繼承的每個虛擬方法，會有與該類別相關之方法的***最大衍生執行***。</span><span class="sxs-lookup"><span data-stu-id="2d25f-883">For every virtual method declared in or inherited by a class, there exists a ***most derived implementation*** of the method with respect to that class.</span></span> <span data-ttu-id="2d25f-884">與類別 `R` 相關之虛擬方法的最大衍生執行 `M`，如下所示：</span><span class="sxs-lookup"><span data-stu-id="2d25f-884">The most derived implementation of a virtual method `M` with respect to a class `R` is determined as follows:</span></span>

*  <span data-ttu-id="2d25f-885">如果 `R` 包含 `M`的 `virtual` 宣告簡介，則這是 `M`的最衍生執行。</span><span class="sxs-lookup"><span data-stu-id="2d25f-885">If `R` contains the introducing `virtual` declaration of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="2d25f-886">否則，如果 `R` 包含 `M`的 `override`，則這是 `M`的最衍生執行。</span><span class="sxs-lookup"><span data-stu-id="2d25f-886">Otherwise, if `R` contains an `override` of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="2d25f-887">否則，與 `R`的直接基類有關的最多衍生 `M`，`M` 就是與 `R` 的最佳衍生執行。</span><span class="sxs-lookup"><span data-stu-id="2d25f-887">Otherwise, the most derived implementation of `M` with respect to `R` is the same as the most derived implementation of `M` with respect to the direct base class of `R`.</span></span>

<span data-ttu-id="2d25f-888">下列範例說明虛擬和非虛擬方法之間的差異：</span><span class="sxs-lookup"><span data-stu-id="2d25f-888">The following example illustrates the differences between virtual and non-virtual methods:</span></span>
```csharp
using System;

class A
{
    public void F() { Console.WriteLine("A.F"); }

    public virtual void G() { Console.WriteLine("A.G"); }
}

class B: A
{
    new public void F() { Console.WriteLine("B.F"); }

    public override void G() { Console.WriteLine("B.G"); }
}

class Test
{
    static void Main() {
        B b = new B();
        A a = b;
        a.F();
        b.F();
        a.G();
        b.G();
    }
}
```

<span data-ttu-id="2d25f-889">在此範例中，`A` 引進了非虛擬方法 `F` 和 `G`的虛擬方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-889">In the example, `A` introduces a non-virtual method `F` and a virtual method `G`.</span></span> <span data-ttu-id="2d25f-890">類別 `B` 引進新的非虛擬方法 `F`，因此會隱藏繼承的 `F`，也會覆寫繼承的方法 `G`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-890">The class `B` introduces a new non-virtual method `F`, thus hiding the inherited `F`, and also overrides the inherited method `G`.</span></span> <span data-ttu-id="2d25f-891">此範例會產生輸出：</span><span class="sxs-lookup"><span data-stu-id="2d25f-891">The example produces the output:</span></span>
```console
A.F
B.F
B.G
B.G
```

<span data-ttu-id="2d25f-892">請注意，語句 `a.G()` 會叫用 `B.G`，而不是 `A.G`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-892">Notice that the statement `a.G()` invokes `B.G`, not `A.G`.</span></span> <span data-ttu-id="2d25f-893">這是因為實例的執行時間類型（`B`），而不是實例的編譯時間類型（`A`），會決定要叫用的實際方法執行。</span><span class="sxs-lookup"><span data-stu-id="2d25f-893">This is because the run-time type of the instance (which is `B`), not the compile-time type of the instance (which is `A`), determines the actual method implementation to invoke.</span></span>

<span data-ttu-id="2d25f-894">因為允許方法隱藏繼承的方法，所以類別可以包含數個具有相同簽章的虛擬方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-894">Because methods are allowed to hide inherited methods, it is possible for a class to contain several virtual methods with the same signature.</span></span> <span data-ttu-id="2d25f-895">這不會造成不明確的問題，因為所有的衍生方法都是隱藏的。</span><span class="sxs-lookup"><span data-stu-id="2d25f-895">This does not present an ambiguity problem, since all but the most derived method are hidden.</span></span> <span data-ttu-id="2d25f-896">在範例中</span><span class="sxs-lookup"><span data-stu-id="2d25f-896">In the example</span></span>
```csharp
using System;

class A
{
    public virtual void F() { Console.WriteLine("A.F"); }
}

class B: A
{
    public override void F() { Console.WriteLine("B.F"); }
}

class C: B
{
    new public virtual void F() { Console.WriteLine("C.F"); }
}

class D: C
{
    public override void F() { Console.WriteLine("D.F"); }
}

class Test
{
    static void Main() {
        D d = new D();
        A a = d;
        B b = d;
        C c = d;
        a.F();
        b.F();
        c.F();
        d.F();
    }
}
```
<span data-ttu-id="2d25f-897">[`C`] 和 [`D`] 類別包含兩個具有相同簽章的虛擬方法： `A` 所引進的和 `C`所導入的一種。</span><span class="sxs-lookup"><span data-stu-id="2d25f-897">the `C` and `D` classes contain two virtual methods with the same signature: The one introduced by `A` and the one introduced by `C`.</span></span> <span data-ttu-id="2d25f-898">`C` 引進的方法會隱藏繼承自 `A`的方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-898">The method introduced by `C` hides the method inherited from `A`.</span></span> <span data-ttu-id="2d25f-899">因此，`D` 中的覆寫宣告會覆寫由 `C`所導入的方法，而且不可能 `D` 覆寫 `A`所引進的方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-899">Thus, the override declaration in `D` overrides the method introduced by `C`, and it is not possible for `D` to override the method introduced by `A`.</span></span> <span data-ttu-id="2d25f-900">此範例會產生輸出：</span><span class="sxs-lookup"><span data-stu-id="2d25f-900">The example produces the output:</span></span>
```console
B.F
B.F
D.F
D.F
```

<span data-ttu-id="2d25f-901">請注意，透過不會隱藏方法的較小衍生類型來存取 `D` 的實例，就可以叫用隱藏的虛擬方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-901">Note that it is possible to invoke the hidden virtual method by accessing an instance of `D` through a less derived type in which the method is not hidden.</span></span>

### <a name="override-methods"></a><span data-ttu-id="2d25f-902">覆寫方法</span><span class="sxs-lookup"><span data-stu-id="2d25f-902">Override methods</span></span>

<span data-ttu-id="2d25f-903">當實例方法宣告包含 `override` 修飾詞時，此方法會被視為覆***寫方法***。</span><span class="sxs-lookup"><span data-stu-id="2d25f-903">When an instance method declaration includes an `override` modifier, the method is said to be an ***override method***.</span></span> <span data-ttu-id="2d25f-904">Override 方法會以相同的簽章覆寫繼承的虛擬方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-904">An override method overrides an inherited virtual method with the same signature.</span></span> <span data-ttu-id="2d25f-905">虛擬方法宣告會導入新的方法，而覆寫方法宣告則是會提供現有已繼承之虛擬方法的新實作，來將該方法特製化。</span><span class="sxs-lookup"><span data-stu-id="2d25f-905">Whereas a virtual method declaration introduces a new method, an override method declaration specializes an existing inherited virtual method by providing a new implementation of that method.</span></span>

<span data-ttu-id="2d25f-906">`override` 宣告所覆寫的方法稱為覆寫的***基底方法***。</span><span class="sxs-lookup"><span data-stu-id="2d25f-906">The method overridden by an `override` declaration is known as the ***overridden base method***.</span></span> <span data-ttu-id="2d25f-907">如果覆寫方法 `M` 在類別 `C`中宣告，覆寫基底方法的判斷方式是檢查每個基類類型的 `C`，從 `C` 的直接基類類型開始，然後繼續每個連續的直接基類類型，直到指定的基類類型中，至少有一個可存取的方法，其簽章與替代類型引數之後 `M` 的簽章相同。</span><span class="sxs-lookup"><span data-stu-id="2d25f-907">For an override method `M` declared in a class `C`, the overridden base method is determined by examining each base class type of `C`, starting with the direct base class type of `C` and continuing with each successive direct base class type, until in a given base class type at least one accessible method is located which has the same signature as `M` after substitution of type arguments.</span></span> <span data-ttu-id="2d25f-908">為了找出覆寫的基底方法，如果方法已 `public``protected`，則會被視為可存取，如果已 `protected internal`，或 `internal` 並在與 `C`相同的程式中宣告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-908">For the purposes of locating the overridden base method, a method is considered accessible if it is `public`, if it is `protected`, if it is `protected internal`, or if it is `internal` and declared in the same program as `C`.</span></span>

<span data-ttu-id="2d25f-909">如果覆寫宣告的下列所有條件都成立，就會發生編譯時期錯誤：</span><span class="sxs-lookup"><span data-stu-id="2d25f-909">A compile-time error occurs unless all of the following are true for an override declaration:</span></span>

*  <span data-ttu-id="2d25f-910">如上面所述，已覆寫的基底方法可以找到。</span><span class="sxs-lookup"><span data-stu-id="2d25f-910">An overridden base method can be located as described above.</span></span>
*  <span data-ttu-id="2d25f-911">只有一個這種覆寫的基底方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-911">There is exactly one such overridden base method.</span></span> <span data-ttu-id="2d25f-912">只有當基類型別是結構化型別，而其中類型引數的替代方式讓兩個方法的簽章相同時，這項限制才會生效。</span><span class="sxs-lookup"><span data-stu-id="2d25f-912">This restriction has effect only if the base class type is a constructed type where the substitution of type arguments makes the signature of two methods the same.</span></span>
*  <span data-ttu-id="2d25f-913">覆寫的基底方法是虛擬、抽象或覆寫方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-913">The overridden base method is a virtual, abstract, or override method.</span></span> <span data-ttu-id="2d25f-914">換句話說，覆寫的基底方法不可以是靜態或非虛擬的。</span><span class="sxs-lookup"><span data-stu-id="2d25f-914">In other words, the overridden base method cannot be static or non-virtual.</span></span>
*  <span data-ttu-id="2d25f-915">覆寫的基底方法不是密封的方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-915">The overridden base method is not a sealed method.</span></span>
*  <span data-ttu-id="2d25f-916">覆寫方法和覆寫的基底方法具有相同的傳回類型。</span><span class="sxs-lookup"><span data-stu-id="2d25f-916">The override method and the overridden base method have the same return type.</span></span>
*  <span data-ttu-id="2d25f-917">覆寫宣告和覆寫的基底方法具有相同的宣告存取範圍。</span><span class="sxs-lookup"><span data-stu-id="2d25f-917">The override declaration and the overridden base method have the same declared accessibility.</span></span> <span data-ttu-id="2d25f-918">換句話說，覆寫宣告無法變更虛擬方法的存取範圍。</span><span class="sxs-lookup"><span data-stu-id="2d25f-918">In other words, an override declaration cannot change the accessibility of the virtual method.</span></span> <span data-ttu-id="2d25f-919">不過，如果覆寫的基底方法是內部保護的，而且在與包含覆寫方法的元件不同的元件中宣告，則覆寫方法的宣告存取範圍必須受到保護。</span><span class="sxs-lookup"><span data-stu-id="2d25f-919">However, if the overridden base method is protected internal and it is declared in a different assembly than the assembly containing the override method then the override method's declared accessibility must be protected.</span></span>
*  <span data-ttu-id="2d25f-920">覆寫宣告不會指定類型參數條件約束子句。</span><span class="sxs-lookup"><span data-stu-id="2d25f-920">The override declaration does not specify type-parameter-constraints-clauses.</span></span> <span data-ttu-id="2d25f-921">相反地，條件約束會繼承自覆寫的基底方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-921">Instead the constraints are inherited from the overridden base method.</span></span> <span data-ttu-id="2d25f-922">請注意，在覆寫方法中，類型參數的條件約束可能會由繼承之條件約束中的類型引數取代。</span><span class="sxs-lookup"><span data-stu-id="2d25f-922">Note that constraints that are type parameters in the overridden method may be replaced by type arguments in the inherited constraint.</span></span> <span data-ttu-id="2d25f-923">這可能會導致在明確指定（例如實數值型別或密封類型）時不合法的條件約束。</span><span class="sxs-lookup"><span data-stu-id="2d25f-923">This can lead to constraints that are not legal when explicitly specified, such as value types or sealed types.</span></span>

<span data-ttu-id="2d25f-924">下列範例示範泛型類別的覆寫規則的使用方式：</span><span class="sxs-lookup"><span data-stu-id="2d25f-924">The following example demonstrates how the overriding rules work for generic classes:</span></span>
```csharp
abstract class C<T>
{
    public virtual T F() {...}
    public virtual C<T> G() {...}
    public virtual void H(C<T> x) {...}
}

class D: C<string>
{
    public override string F() {...}            // Ok
    public override C<string> G() {...}         // Ok
    public override void H(C<T> x) {...}        // Error, should be C<string>
}

class E<T,U>: C<U>
{
    public override U F() {...}                 // Ok
    public override C<U> G() {...}              // Ok
    public override void H(C<T> x) {...}        // Error, should be C<U>
}
```

<span data-ttu-id="2d25f-925">覆寫宣告可以使用*base_access* （[基底存取](expressions.md#base-access)）來存取覆寫的基底方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-925">An override declaration can access the overridden base method using a *base_access* ([Base access](expressions.md#base-access)).</span></span> <span data-ttu-id="2d25f-926">在範例中</span><span class="sxs-lookup"><span data-stu-id="2d25f-926">In the example</span></span>
```csharp
class A
{
    int x;

    public virtual void PrintFields() {
        Console.WriteLine("x = {0}", x);
    }
}

class B: A
{
    int y;

    public override void PrintFields() {
        base.PrintFields();
        Console.WriteLine("y = {0}", y);
    }
}
```
<span data-ttu-id="2d25f-927">`B` 中的 `base.PrintFields()` 調用會叫用 `A`中所宣告的 `PrintFields` 方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-927">the `base.PrintFields()` invocation in `B` invokes the `PrintFields` method declared in `A`.</span></span> <span data-ttu-id="2d25f-928">*Base_access*會停用虛擬調用機制，而且只會將基底方法視為非虛擬方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-928">A *base_access* disables the virtual invocation mechanism and simply treats the base method as a non-virtual method.</span></span> <span data-ttu-id="2d25f-929">已 `((A)this).PrintFields()`撰寫 `B` 中的調用，它會以遞迴方式叫用 `B`中所宣告的 `PrintFields` 方法，而不是在 `A`中宣告的方法，因為 `PrintFields` 是虛擬的，而 `((A)this)` 的執行時間類型是 `B`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-929">Had the invocation in `B` been written `((A)this).PrintFields()`, it would recursively invoke the `PrintFields` method declared in `B`, not the one declared in `A`, since `PrintFields` is virtual and the run-time type of `((A)this)` is `B`.</span></span>

<span data-ttu-id="2d25f-930">只有包含 `override` 修飾詞，方法才能覆寫另一個方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-930">Only by including an `override` modifier can a method override another method.</span></span> <span data-ttu-id="2d25f-931">在所有其他情況下，與繼承方法具有相同簽章的方法，只會隱藏繼承的方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-931">In all other cases, a method with the same signature as an inherited method simply hides the inherited method.</span></span> <span data-ttu-id="2d25f-932">在範例中</span><span class="sxs-lookup"><span data-stu-id="2d25f-932">In the example</span></span>
```csharp
class A
{
    public virtual void F() {}
}

class B: A
{
    public virtual void F() {}        // Warning, hiding inherited F()
}
```
<span data-ttu-id="2d25f-933">`B` 中的 `F` 方法不包含 `override` 修飾詞，因此不會覆寫 `A`中的 `F` 方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-933">the `F` method in `B` does not include an `override` modifier and therefore does not override the `F` method in `A`.</span></span> <span data-ttu-id="2d25f-934">相反地，`B` 中的 `F` 方法會隱藏 `A`中的方法，而且會回報警告，因為宣告不包含 `new` 修飾詞。</span><span class="sxs-lookup"><span data-stu-id="2d25f-934">Rather, the `F` method in `B` hides the method in `A`, and a warning is reported because the declaration does not include a `new` modifier.</span></span>

<span data-ttu-id="2d25f-935">在範例中</span><span class="sxs-lookup"><span data-stu-id="2d25f-935">In the example</span></span>
```csharp
class A
{
    public virtual void F() {}
}

class B: A
{
    new private void F() {}        // Hides A.F within body of B
}

class C: B
{
    public override void F() {}    // Ok, overrides A.F
}
```
<span data-ttu-id="2d25f-936">`B` 中的 `F` 方法會隱藏繼承自 `A`的虛擬 `F` 方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-936">the `F` method in `B` hides the virtual `F` method inherited from `A`.</span></span> <span data-ttu-id="2d25f-937">因為 `B` 中的新 `F` 具有私用存取權，所以其範圍僅包含 `B` 的類別主體，並不會延伸到 `C`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-937">Since the new `F` in `B` has private access, its scope only includes the class body of `B` and does not extend to `C`.</span></span> <span data-ttu-id="2d25f-938">因此，`C` 中的 `F` 宣告允許覆寫繼承自 `A`的 `F`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-938">Therefore, the declaration of `F` in `C` is permitted to override the `F` inherited from `A`.</span></span>

### <a name="sealed-methods"></a><span data-ttu-id="2d25f-939">密封方法</span><span class="sxs-lookup"><span data-stu-id="2d25f-939">Sealed methods</span></span>

<span data-ttu-id="2d25f-940">當實例方法宣告包含 `sealed` 修飾詞時，該方法會被視為密封的***方法***。</span><span class="sxs-lookup"><span data-stu-id="2d25f-940">When an instance method declaration includes a `sealed` modifier, that method is said to be a ***sealed method***.</span></span> <span data-ttu-id="2d25f-941">如果實例方法宣告包含 `sealed` 修飾詞，它也必須包含 `override` 修飾詞。</span><span class="sxs-lookup"><span data-stu-id="2d25f-941">If an instance method declaration includes the  `sealed` modifier, it must also include the `override` modifier.</span></span> <span data-ttu-id="2d25f-942">使用 `sealed` 修飾詞可防止衍生類別進一步覆寫方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-942">Use of the `sealed` modifier prevents a derived class from further overriding the method.</span></span>

<span data-ttu-id="2d25f-943">在範例中</span><span class="sxs-lookup"><span data-stu-id="2d25f-943">In the example</span></span>
```csharp
using System;

class A
{
    public virtual void F() {
        Console.WriteLine("A.F");
    }

    public virtual void G() {
        Console.WriteLine("A.G");
    }
}

class B: A
{
    sealed override public void F() {
        Console.WriteLine("B.F");
    } 

    override public void G() {
        Console.WriteLine("B.G");
    } 
}

class C: B
{
    override public void G() {
        Console.WriteLine("C.G");
    } 
}
```
<span data-ttu-id="2d25f-944">類別 `B` 提供兩個覆寫方法：具有 `sealed` 修飾詞的 `F` 方法，以及不具有的 `G` 方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-944">the class `B` provides two override methods: an `F` method that has the `sealed` modifier and a `G` method that does not.</span></span> <span data-ttu-id="2d25f-945">`B`使用密封的 `modifier` 可防止 `C` 進一步覆寫 `F`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-945">`B`'s use of the sealed `modifier` prevents `C` from further overriding `F`.</span></span>

### <a name="abstract-methods"></a><span data-ttu-id="2d25f-946">抽象方法</span><span class="sxs-lookup"><span data-stu-id="2d25f-946">Abstract methods</span></span>

<span data-ttu-id="2d25f-947">當實例方法宣告包含 `abstract` 修飾詞時，該方法會被視為***抽象方法***。</span><span class="sxs-lookup"><span data-stu-id="2d25f-947">When an instance method declaration includes an `abstract` modifier, that method is said to be an ***abstract method***.</span></span> <span data-ttu-id="2d25f-948">雖然抽象方法也是隱含的虛擬方法，但它不能有修飾詞 `virtual`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-948">Although an abstract method is implicitly also a virtual method, it cannot have the modifier `virtual`.</span></span>

<span data-ttu-id="2d25f-949">抽象方法宣告引進新的虛擬方法，但不提供該方法的執行。</span><span class="sxs-lookup"><span data-stu-id="2d25f-949">An abstract method declaration introduces a new virtual method but does not provide an implementation of that method.</span></span> <span data-ttu-id="2d25f-950">相反地，非抽象衍生類別必須覆寫該方法，才能提供自己的執行。</span><span class="sxs-lookup"><span data-stu-id="2d25f-950">Instead, non-abstract derived classes are required to provide their own implementation by overriding that method.</span></span> <span data-ttu-id="2d25f-951">因為抽象方法不提供實際的實值，所以抽象方法的*method_body*只會包含一個分號。</span><span class="sxs-lookup"><span data-stu-id="2d25f-951">Because an abstract method provides no actual implementation, the *method_body* of an abstract method simply consists of a semicolon.</span></span>

<span data-ttu-id="2d25f-952">抽象方法宣告只允許用於抽象類別（[抽象類別](classes.md#abstract-classes)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-952">Abstract method declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="2d25f-953">在範例中</span><span class="sxs-lookup"><span data-stu-id="2d25f-953">In the example</span></span>
```csharp
public abstract class Shape
{
    public abstract void Paint(Graphics g, Rectangle r);
}

public class Ellipse: Shape
{
    public override void Paint(Graphics g, Rectangle r) {
        g.DrawEllipse(r);
    }
}

public class Box: Shape
{
    public override void Paint(Graphics g, Rectangle r) {
        g.DrawRect(r);
    }
}
```
<span data-ttu-id="2d25f-954">`Shape` 類別會定義可繪製本身的幾何形狀物件的抽象概念。</span><span class="sxs-lookup"><span data-stu-id="2d25f-954">the `Shape` class defines the abstract notion of a geometrical shape object that can paint itself.</span></span> <span data-ttu-id="2d25f-955">`Paint` 方法是抽象的，因為沒有有意義的預設實值。</span><span class="sxs-lookup"><span data-stu-id="2d25f-955">The `Paint` method is abstract because there is no meaningful default implementation.</span></span> <span data-ttu-id="2d25f-956">`Ellipse` 和 `Box` 類別是具體的 `Shape` 實作為。</span><span class="sxs-lookup"><span data-stu-id="2d25f-956">The `Ellipse` and `Box` classes are concrete `Shape` implementations.</span></span> <span data-ttu-id="2d25f-957">因為這些類別是非抽象的，所以必須覆寫 `Paint` 方法並提供實際的實作為。</span><span class="sxs-lookup"><span data-stu-id="2d25f-957">Because these classes are non-abstract, they are required to override the `Paint` method and provide an actual implementation.</span></span>

<span data-ttu-id="2d25f-958">*Base_access* （[基底存取](expressions.md#base-access)）參考抽象方法時，會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-958">It is a compile-time error for a *base_access* ([Base access](expressions.md#base-access)) to reference an abstract method.</span></span> <span data-ttu-id="2d25f-959">在範例中</span><span class="sxs-lookup"><span data-stu-id="2d25f-959">In the example</span></span>
```csharp
abstract class A
{
    public abstract void F();
}

class B: A
{
    public override void F() {
        base.F();                        // Error, base.F is abstract
    }
}
```
<span data-ttu-id="2d25f-960">`base.F()` 調用會回報編譯時期錯誤，因為它會參考抽象方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-960">a compile-time error is reported for the `base.F()` invocation because it references an abstract method.</span></span>

<span data-ttu-id="2d25f-961">允許抽象方法宣告覆寫虛擬方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-961">An abstract method declaration is permitted to override a virtual method.</span></span> <span data-ttu-id="2d25f-962">這可讓抽象類別在衍生類別中強制重新執行方法，並使方法的原始實作為無法使用。</span><span class="sxs-lookup"><span data-stu-id="2d25f-962">This allows an abstract class to force re-implementation of the method in derived classes, and makes the original implementation of the method unavailable.</span></span> <span data-ttu-id="2d25f-963">在範例中</span><span class="sxs-lookup"><span data-stu-id="2d25f-963">In the example</span></span>
```csharp
using System;

class A
{
    public virtual void F() {
        Console.WriteLine("A.F");
    }
}

abstract class B: A
{
    public abstract override void F();
}

class C: B
{
    public override void F() {
        Console.WriteLine("C.F");
    }
}
```
<span data-ttu-id="2d25f-964">類別 `A` 宣告虛擬方法，類別 `B` 會以抽象方法覆寫這個方法，而類別 `C` 會覆寫抽象方法以提供自己的實作為。</span><span class="sxs-lookup"><span data-stu-id="2d25f-964">class `A` declares a virtual method, class `B` overrides this method with an abstract method, and class `C` overrides the abstract method to provide its own implementation.</span></span>

### <a name="external-methods"></a><span data-ttu-id="2d25f-965">外部方法</span><span class="sxs-lookup"><span data-stu-id="2d25f-965">External methods</span></span>

<span data-ttu-id="2d25f-966">當方法宣告包含 `extern` 修飾詞時，該方法會被視為***外部方法***。</span><span class="sxs-lookup"><span data-stu-id="2d25f-966">When a method declaration includes an `extern` modifier, that method is said to be an ***external method***.</span></span> <span data-ttu-id="2d25f-967">外部方法會在外部執行，通常使用以外的語言C#。</span><span class="sxs-lookup"><span data-stu-id="2d25f-967">External methods are implemented externally, typically using a language other than C#.</span></span> <span data-ttu-id="2d25f-968">因為外部方法宣告不提供實際的實值，所以外部方法的*method_body*只會包含一個分號。</span><span class="sxs-lookup"><span data-stu-id="2d25f-968">Because an external method declaration provides no actual implementation, the *method_body* of an external method simply consists of a semicolon.</span></span> <span data-ttu-id="2d25f-969">外部方法可能不是泛型。</span><span class="sxs-lookup"><span data-stu-id="2d25f-969">An external method may not be generic.</span></span>

<span data-ttu-id="2d25f-970">`extern` 修飾詞通常會與 `DllImport` 屬性（[與 COM 和 Win32 元件](attributes.md#interoperation-with-com-and-win32-components)互通）搭配使用，讓外部方法可以由 Dll （動態連結程式庫）來執行。</span><span class="sxs-lookup"><span data-stu-id="2d25f-970">The `extern` modifier is typically used in conjunction with a `DllImport` attribute ([Interoperation with COM and Win32 components](attributes.md#interoperation-with-com-and-win32-components)), allowing external methods to be implemented by DLLs (Dynamic Link Libraries).</span></span> <span data-ttu-id="2d25f-971">執行環境可支援其他機制，讓您可以提供外部方法的實現。</span><span class="sxs-lookup"><span data-stu-id="2d25f-971">The execution environment may support other mechanisms whereby implementations of external methods can be provided.</span></span>

<span data-ttu-id="2d25f-972">當外部方法包含 `DllImport` 屬性時，方法宣告也必須包含 `static` 修飾詞。</span><span class="sxs-lookup"><span data-stu-id="2d25f-972">When an external method includes a `DllImport` attribute, the method declaration must also include a `static` modifier.</span></span> <span data-ttu-id="2d25f-973">這個範例示範如何使用 `extern` 修飾詞和 `DllImport` 屬性：</span><span class="sxs-lookup"><span data-stu-id="2d25f-973">This example demonstrates the use of the `extern` modifier and the `DllImport` attribute:</span></span>
```csharp
using System.Text;
using System.Security.Permissions;
using System.Runtime.InteropServices;

class Path
{
    [DllImport("kernel32", SetLastError=true)]
    static extern bool CreateDirectory(string name, SecurityAttribute sa);

    [DllImport("kernel32", SetLastError=true)]
    static extern bool RemoveDirectory(string name);

    [DllImport("kernel32", SetLastError=true)]
    static extern int GetCurrentDirectory(int bufSize, StringBuilder buf);

    [DllImport("kernel32", SetLastError=true)]
    static extern bool SetCurrentDirectory(string name);
}
```

### <a name="partial-methods-recap"></a><span data-ttu-id="2d25f-974">部分方法（回顧）</span><span class="sxs-lookup"><span data-stu-id="2d25f-974">Partial methods (recap)</span></span>

<span data-ttu-id="2d25f-975">當方法宣告包含 `partial` 修飾詞時，該方法會被視為***部分方法***。</span><span class="sxs-lookup"><span data-stu-id="2d25f-975">When a method declaration includes a `partial` modifier, that method is said to be a ***partial method***.</span></span> <span data-ttu-id="2d25f-976">部分方法只能宣告為部分類型（[部分類型](classes.md#partial-types)）的成員，而且受到一些限制。</span><span class="sxs-lookup"><span data-stu-id="2d25f-976">Partial methods can only be declared as members of partial types ([Partial types](classes.md#partial-types)), and are subject to a number of restrictions.</span></span> <span data-ttu-id="2d25f-977">部分方法會進一步說明部分[方法。](classes.md#partial-methods)</span><span class="sxs-lookup"><span data-stu-id="2d25f-977">Partial methods are further described in [Partial methods](classes.md#partial-methods).</span></span>

### <a name="extension-methods"></a><span data-ttu-id="2d25f-978">擴充方法</span><span class="sxs-lookup"><span data-stu-id="2d25f-978">Extension methods</span></span>

<span data-ttu-id="2d25f-979">當方法的第一個參數包含 `this` 修飾詞時，該方法會被視為***擴充方法***。</span><span class="sxs-lookup"><span data-stu-id="2d25f-979">When the first parameter of a method includes the `this` modifier, that method is said to be an ***extension method***.</span></span> <span data-ttu-id="2d25f-980">擴充方法只能在非泛型、非嵌套的靜態類別中宣告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-980">Extension methods can only be declared in non-generic, non-nested static classes.</span></span> <span data-ttu-id="2d25f-981">擴充方法的第一個參數不能有 `this`以外的任何修飾詞，且參數類型不能是指標類型。</span><span class="sxs-lookup"><span data-stu-id="2d25f-981">The first parameter of an extension method can have no modifiers other than `this`, and the parameter type cannot be a pointer type.</span></span>

<span data-ttu-id="2d25f-982">以下是宣告兩個擴充方法的靜態類別範例：</span><span class="sxs-lookup"><span data-stu-id="2d25f-982">The following is an example of a static class that declares two extension methods:</span></span>
```csharp
public static class Extensions
{
    public static int ToInt32(this string s) {
        return Int32.Parse(s);
    }

    public static T[] Slice<T>(this T[] source, int index, int count) {
        if (index < 0 || count < 0 || source.Length - index < count)
            throw new ArgumentException();
        T[] result = new T[count];
        Array.Copy(source, index, result, 0, count);
        return result;
    }
}
```

<span data-ttu-id="2d25f-983">擴充方法是一般的靜態方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-983">An extension method is a regular static method.</span></span> <span data-ttu-id="2d25f-984">此外，其封閉式靜態類別在範圍內，可以使用實例方法調用語法（[擴充方法調用](expressions.md#extension-method-invocations)）叫用擴充方法，使用接收者運算式做為第一個引數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-984">In addition, where its enclosing static class is in scope, an extension method can be invoked using instance method invocation syntax ([Extension method invocations](expressions.md#extension-method-invocations)), using the receiver expression as the first argument.</span></span>

<span data-ttu-id="2d25f-985">下列程式會使用上述所宣告的擴充方法：</span><span class="sxs-lookup"><span data-stu-id="2d25f-985">The following program uses the extension methods declared above:</span></span>
```csharp
static class Program
{
    static void Main() {
        string[] strings = { "1", "22", "333", "4444" };
        foreach (string s in strings.Slice(1, 2)) {
            Console.WriteLine(s.ToInt32());
        }
    }
}
```

<span data-ttu-id="2d25f-986">`Slice` 方法可在 `string[]`上取得，而 `ToInt32` 方法則可在 `string`上使用，因為它們已宣告為擴充方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-986">The `Slice` method is available on the `string[]`, and the `ToInt32` method is available on `string`, because they have been declared as extension methods.</span></span> <span data-ttu-id="2d25f-987">使用一般靜態方法呼叫時，程式的意義與下列內容相同：</span><span class="sxs-lookup"><span data-stu-id="2d25f-987">The meaning of the program is the same as the following, using ordinary static method calls:</span></span>
```csharp
static class Program
{
    static void Main() {
        string[] strings = { "1", "22", "333", "4444" };
        foreach (string s in Extensions.Slice(strings, 1, 2)) {
            Console.WriteLine(Extensions.ToInt32(s));
        }
    }
}
```

### <a name="method-body"></a><span data-ttu-id="2d25f-988">方法主體</span><span class="sxs-lookup"><span data-stu-id="2d25f-988">Method body</span></span>

<span data-ttu-id="2d25f-989">方法宣告的*method_body*是由區塊主體、運算式主體或分號所組成。</span><span class="sxs-lookup"><span data-stu-id="2d25f-989">The *method_body* of a method declaration consists of either a block body, an expression body or a semicolon.</span></span>

<span data-ttu-id="2d25f-990">如果傳回型別 `void`，或如果方法是非同步，而且傳回型別為 `System.Threading.Tasks.Task`，則方法的***結果型***別會 `void`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-990">The ***result type*** of a method is `void` if the return type is `void`, or if the method is async and the return type is `System.Threading.Tasks.Task`.</span></span> <span data-ttu-id="2d25f-991">否則，非非同步方法的結果型別就是它的傳回型別，而且具有傳回型別 `System.Threading.Tasks.Task<T>` 之非同步方法的結果型別會 `T`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-991">Otherwise, the result type of a non-async method is its return type, and the result type of an async method with return type `System.Threading.Tasks.Task<T>` is `T`.</span></span>

<span data-ttu-id="2d25f-992">當方法具有 `void` 結果型別和區塊主體時，不允許在區塊中使用 `return` 的語句（[return 語句](statements.md#the-return-statement)）來指定運算式。</span><span class="sxs-lookup"><span data-stu-id="2d25f-992">When a method has a `void` result type and a block body, `return` statements ([The return statement](statements.md#the-return-statement)) in the block are not permitted to specify an expression.</span></span> <span data-ttu-id="2d25f-993">如果 void 方法的區塊執行正常（也就是，控制在方法主體結尾的流程），該方法只會回到其目前的呼叫端。</span><span class="sxs-lookup"><span data-stu-id="2d25f-993">If execution of the block of a void method completes normally (that is, control flows off the end of the method body), that method simply returns to its current caller.</span></span>
    
<span data-ttu-id="2d25f-994">當方法具有 `void` 結果和運算式主體時，運算式 `E` 必須是*statement_expression*，而主體則完全等同于表單 `{ E; }`的區塊主體。</span><span class="sxs-lookup"><span data-stu-id="2d25f-994">When a method has a `void` result and an expression body, the expression `E` must be a *statement_expression*, and the body is exactly equivalent to a block body of the form `{ E; }`.</span></span>
    
<span data-ttu-id="2d25f-995">當方法具有非 void 的結果型別和區塊主體時，區塊中的每個 `return` 語句都必須指定可隱含轉換成結果型別的運算式。</span><span class="sxs-lookup"><span data-stu-id="2d25f-995">When a method has a non-void result type and a block body, each `return` statement in the block must specify an expression that is implicitly convertible to the result type.</span></span> <span data-ttu-id="2d25f-996">值傳回方法之區塊主體的端點不得為可連線。</span><span class="sxs-lookup"><span data-stu-id="2d25f-996">The endpoint of a block body of a value-returning method must not be reachable.</span></span> <span data-ttu-id="2d25f-997">換句話說，在具有區塊主體的值傳回方法中，不允許控制項在方法主體的結尾流動。</span><span class="sxs-lookup"><span data-stu-id="2d25f-997">In other words, in a value-returning method with a block body, control is not permitted to flow off the end of the method body.</span></span>
    
<span data-ttu-id="2d25f-998">當方法具有非 void 的結果型別和運算式主體時，運算式必須可以隱含地轉換成結果型別，而主體則完全等同于表單 `{ return E; }`的區塊主體。</span><span class="sxs-lookup"><span data-stu-id="2d25f-998">When a method has a non-void result type and an expression body, the expression must be implicitly convertible to the result type, and the body is exactly equivalent to a block body of the form `{ return E; }`.</span></span>
    
<span data-ttu-id="2d25f-999">在範例中</span><span class="sxs-lookup"><span data-stu-id="2d25f-999">In the example</span></span>
```csharp
class A
{
    public int F() {}            // Error, return value required

    public int G() {
        return 1;
    }

    public int H(bool b) {
        if (b) {
            return 1;
        }
        else {
            return 0;
        }
    }

    public int I(bool b) => b ? 1 : 0;
}
```
<span data-ttu-id="2d25f-1000">傳回的值 `F` 方法會導致編譯時期錯誤，因為控制項可以在方法主體的結尾處流動。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1000">the value-returning `F` method results in a compile-time error because control can flow off the end of the method body.</span></span> <span data-ttu-id="2d25f-1001">`G` 和 `H` 方法是正確的，因為所有可能的執行路徑都在指定傳回值的 return 語句中結束。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1001">The `G` and `H` methods are correct because all possible execution paths end in a return statement that specifies a return value.</span></span> <span data-ttu-id="2d25f-1002">`I` 方法是正確的，因為它的主體等同于其中只有一個 return 語句的語句區塊。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1002">The `I` method is correct, because its body is equivalent to a statement block with just a single return statement in it.</span></span>

### <a name="method-overloading"></a><span data-ttu-id="2d25f-1003">方法多載</span><span class="sxs-lookup"><span data-stu-id="2d25f-1003">Method overloading</span></span>

<span data-ttu-id="2d25f-1004">方法多載解析規則會在[型別推斷](expressions.md#type-inference)中說明。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1004">The method overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="properties"></a><span data-ttu-id="2d25f-1005">屬性</span><span class="sxs-lookup"><span data-stu-id="2d25f-1005">Properties</span></span>

<span data-ttu-id="2d25f-1006">***屬性***是可存取物件或類別特性的成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1006">A ***property*** is a member that provides access to a characteristic of an object or a class.</span></span> <span data-ttu-id="2d25f-1007">屬性的範例包括字串的長度、字型的大小、視窗的標題、客戶的名稱等等等等。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1007">Examples of properties include the length of a string, the size of a font, the caption of a window, the name of a customer, and so on.</span></span> <span data-ttu-id="2d25f-1008">屬性是欄位的自然延伸，兩者都是具有相關聯類型的命名成員，而存取欄位和屬性的語法則相同。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1008">Properties are a natural extension of fields—both are named members with associated types, and the syntax for accessing fields and properties is the same.</span></span> <span data-ttu-id="2d25f-1009">不過，與欄位不同的是，屬性並不會指示儲存位置。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1009">However, unlike fields, properties do not denote storage locations.</span></span> <span data-ttu-id="2d25f-1010">取而代之的是，屬性會有「存取子」，這些存取子會指定讀取或寫入其值時要執行的陳述式。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1010">Instead, properties have ***accessors*** that specify the statements to be executed when their values are read or written.</span></span> <span data-ttu-id="2d25f-1011">因此，屬性會提供一個機制，讓動作與物件屬性的讀取和寫入產生關聯;此外，它們允許計算這類屬性。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1011">Properties thus provide a mechanism for associating actions with the reading and writing of an object's attributes; furthermore, they permit such attributes to be computed.</span></span>

<span data-ttu-id="2d25f-1012">屬性會使用*property_declaration*s 來宣告：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1012">Properties are declared using *property_declaration*s:</span></span>

```antlr
property_declaration
    : attributes? property_modifier* type member_name property_body
    ;

property_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | property_modifier_unsafe
    ;

property_body
    : '{' accessor_declarations '}' property_initializer?
    | '=>' expression ';'
    ;

property_initializer
    : '=' variable_initializer ';'
    ;
```

<span data-ttu-id="2d25f-1013">*Property_declaration*可能包括一組*屬性*（attribute）和四個存取修飾[詞（](attributes.md)[存取](classes.md#access-modifiers)修飾詞）的有效組合、`new` （[新的修飾](classes.md#the-new-modifier)詞）、`static` （[靜態和實例方法](classes.md#static-and-instance-methods)）、`virtual` （[虛擬方法](classes.md#virtual-methods)）、`override` （覆[寫方法](classes.md#override-methods)）、`sealed` （[密封方法](classes.md#sealed-methods)）、`abstract` （[抽象方法](classes.md#abstract-methods)），以及 `extern` （[外部方法](classes.md#external-methods)）修飾詞。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1013">A *property_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="2d25f-1014">屬性宣告受限於與方法宣告（[方法](classes.md#methods)）相同的規則，但與修飾詞的有效組合有關。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1014">Property declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="2d25f-1015">屬性宣告的*型*別會指定宣告所引進的屬性型別，而*member_name*會指定屬性的名稱。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1015">The *type* of a property declaration specifies the type of the property introduced by the declaration, and the *member_name* specifies the name of the property.</span></span> <span data-ttu-id="2d25f-1016">除非屬性是明確介面成員的執行，否則*member_name*只是一個*識別碼*。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1016">Unless the property is an explicit interface member implementation, the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="2d25f-1017">若為明確介面成員（[明確介面成員](interfaces.md#explicit-interface-member-implementations)的執行），則*member_name*包含*interface_type*後面接著 "`.`" 和*識別碼*。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1017">For an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="2d25f-1018">屬性的*類型*至少必須與屬性本身（[存取範圍條件約束](basic-concepts.md#accessibility-constraints)）一樣可以存取。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1018">The *type* of a property must be at least as accessible as the property itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="2d25f-1019">*Property_body*可能是由***存取子主體***或***運算式主體***所組成。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1019">A *property_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="2d25f-1020">在存取子主體中， *accessor_declarations*（必須包含在 "`{`" 和 "`}`" 標記中），請宣告屬性的存取子（[存取](classes.md#accessors)子）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1020">In an accessor body,  *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="2d25f-1021">存取子會指定與讀取和寫入屬性相關聯的可執行語句。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1021">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="2d25f-1022">包含 `=>` 後面接著*運算式*`E` 和分號的運算式主體，與語句主體 `{ get { return E; } }`完全相等，因此只能用來指定僅限 getter 的屬性，其中 getter 的結果是由單一運算式所指定。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1022">An expression body consisting of `=>` followed by an *expression* `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only properties where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="2d25f-1023">只能為自動執行的屬性（[自動實作為屬性](classes.md#automatically-implemented-properties)）提供*property_initializer* ，並將這類屬性的基礎欄位初始化，使其具有*運算式*所指定的值。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1023">A *property_initializer* may only be given for an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)), and causes the initialization of the underlying field of such properties with the value given by the *expression*.</span></span>

<span data-ttu-id="2d25f-1024">雖然存取屬性的語法與欄位相同，但屬性不會分類為變數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1024">Even though the syntax for accessing a property is the same as that for a field, a property is not classified as a variable.</span></span> <span data-ttu-id="2d25f-1025">因此，不可能將屬性當做 `ref` 或 `out` 引數傳遞。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1025">Thus, it is not possible to pass a property as a `ref` or `out` argument.</span></span>

<span data-ttu-id="2d25f-1026">當屬性宣告包含 `extern` 修飾詞時，屬性稱為***外部屬性***。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1026">When a property declaration includes an `extern` modifier, the property is said to be an ***external property***.</span></span> <span data-ttu-id="2d25f-1027">因為外部屬性宣告不提供實際的實值，所以它的每個*accessor_declarations*都包含一個分號。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1027">Because an external property declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

### <a name="static-and-instance-properties"></a><span data-ttu-id="2d25f-1028">靜態和實例屬性</span><span class="sxs-lookup"><span data-stu-id="2d25f-1028">Static and instance properties</span></span>

<span data-ttu-id="2d25f-1029">當屬性宣告包含 `static` 修飾詞時，屬性稱為***靜態屬性***。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1029">When a property declaration includes a `static` modifier, the property is said to be a ***static property***.</span></span> <span data-ttu-id="2d25f-1030">當沒有 `static` 的修飾詞時，此屬性稱為***實例屬性***。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1030">When no `static` modifier is present, the property is said to be an ***instance property***.</span></span>

<span data-ttu-id="2d25f-1031">靜態屬性未與特定實例相關聯，而且是編譯時期錯誤，可在靜態屬性的存取子中參考 `this`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1031">A static property is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static property.</span></span>

<span data-ttu-id="2d25f-1032">實例屬性與類別的指定實例相關聯，而且該實例可以在該屬性的存取子中以 `this` （[此存取權](expressions.md#this-access)）的方式來存取。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1032">An instance property is associated with a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that property.</span></span>

<span data-ttu-id="2d25f-1033">當屬性在表單 `E.M`的*member_access* （[成員存取](expressions.md#member-access)）中被參考時，如果 `M` 是靜態屬性，`E` 必須代表包含 `M`的類型，而且如果 `M` 是實例屬性，則 E 必須代表包含 `M`之類型的實例。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1033">When a property is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static property, `E` must denote a type containing `M`, and if `M` is an instance property, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="2d25f-1034">靜態和實例成員之間的差異會在[靜態和實例成員](classes.md#static-and-instance-members)中進一步討論。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1034">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="accessors"></a><span data-ttu-id="2d25f-1035">取值</span><span class="sxs-lookup"><span data-stu-id="2d25f-1035">Accessors</span></span>

<span data-ttu-id="2d25f-1036">屬性的*accessor_declarations*會指定與讀取和寫入該屬性相關聯的可執行語句。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1036">The *accessor_declarations* of a property specify the executable statements associated with reading and writing that property.</span></span>

```antlr
accessor_declarations
    : get_accessor_declaration set_accessor_declaration?
    | set_accessor_declaration get_accessor_declaration?
    ;

get_accessor_declaration
    : attributes? accessor_modifier? 'get' accessor_body
    ;

set_accessor_declaration
    : attributes? accessor_modifier? 'set' accessor_body
    ;

accessor_modifier
    : 'protected'
    | 'internal'
    | 'private'
    | 'protected' 'internal'
    | 'internal' 'protected'
    ;

accessor_body
    : block
    | ';'
    ;
```

<span data-ttu-id="2d25f-1037">存取子宣告包含*get_accessor_declaration*、 *set_accessor_declaration*或兩者。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1037">The accessor declarations consist of a *get_accessor_declaration*, a *set_accessor_declaration*, or both.</span></span> <span data-ttu-id="2d25f-1038">每個存取子宣告都包含 `get` 或 `set` 的 token，後面接著選擇性的*accessor_modifier*和*accessor_body*。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1038">Each accessor declaration consists of the token `get` or `set` followed by an optional *accessor_modifier* and an *accessor_body*.</span></span>

<span data-ttu-id="2d25f-1039">使用*accessor_modifier*會受到下列限制：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1039">The use of *accessor_modifier*s is governed by the following restrictions:</span></span>

*  <span data-ttu-id="2d25f-1040">*Accessor_modifier*不能用在介面或明確介面成員的執行中。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1040">An *accessor_modifier* may not be used in an interface or in an explicit interface member implementation.</span></span>
*  <span data-ttu-id="2d25f-1041">對於沒有 `override` 修飾詞的屬性或索引子，只有在屬性或索引子同時具有 `get` 和 `set` 存取子，而且只允許在其中一個存取子上時，才允許*accessor_modifier* 。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1041">For a property or indexer that has no `override` modifier, an *accessor_modifier* is permitted only if the property or indexer has both a `get` and `set` accessor, and then is permitted only on one of those accessors.</span></span>
*  <span data-ttu-id="2d25f-1042">對於包含 `override` 修飾詞的屬性或索引子，存取子必須符合所要覆寫之存取子的*accessor_modifier*（如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1042">For a property or indexer that includes an `override` modifier, an accessor must match the *accessor_modifier*, if any, of the accessor being overridden.</span></span>
*  <span data-ttu-id="2d25f-1043">*Accessor_modifier*必須宣告比屬性或索引子本身的宣告存取範圍嚴格更嚴格的存取範圍。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1043">The *accessor_modifier* must declare an accessibility that is strictly more restrictive than the declared accessibility of the property or indexer itself.</span></span> <span data-ttu-id="2d25f-1044">精確：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1044">To be precise:</span></span>
   * <span data-ttu-id="2d25f-1045">如果屬性或索引子具有 `public`的已宣告存取範圍，則*accessor_modifier*可能是 `protected internal`、`internal`、`protected`或 `private`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1045">If the property or indexer has a declared accessibility of `public`, the *accessor_modifier* may be either `protected internal`, `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="2d25f-1046">如果屬性或索引子具有 `protected internal`的宣告存取範圍， *accessor_modifier*可能是 `internal`、`protected`或 `private`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1046">If the property or indexer has a declared accessibility of `protected internal`, the *accessor_modifier* may be either `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="2d25f-1047">如果屬性或索引子具有 `internal` 或 `protected`的已宣告存取範圍，則必須 `private`*accessor_modifier* 。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1047">If the property or indexer has a declared accessibility of `internal` or `protected`, the *accessor_modifier* must be `private`.</span></span>
   * <span data-ttu-id="2d25f-1048">如果屬性或索引子具有 `private`的宣告存取範圍，則不會使用任何*accessor_modifier* 。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1048">If the property or indexer has a declared accessibility of `private`, no *accessor_modifier* may be used.</span></span>

<span data-ttu-id="2d25f-1049">對於 `abstract` 和 `extern` 屬性而言，指定的每個存取子的*accessor_body*只是一個分號。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1049">For `abstract` and `extern` properties, the *accessor_body* for each accessor specified is simply a semicolon.</span></span> <span data-ttu-id="2d25f-1050">非抽象的非外部屬性可能會有每個*accessor_body*都是分號，在此情況下，它是***自動***實作為屬性（[自動實作為](classes.md#automatically-implemented-properties)屬性）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1050">A non-abstract, non-extern property may have each *accessor_body* be a semicolon, in which case it is an ***automatically implemented property*** ([Automatically implemented properties](classes.md#automatically-implemented-properties)).</span></span> <span data-ttu-id="2d25f-1051">自動執行的屬性至少必須有 get 存取子。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1051">An automatically implemented property must have at least a get accessor.</span></span> <span data-ttu-id="2d25f-1052">對於任何其他非抽象、非外部屬性的存取子， *accessor_body*是一個*區塊*，它會指定叫用對應存取子時要執行的語句。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1052">For the accessors of any other non-abstract, non-extern property, the *accessor_body* is a *block* which specifies the statements to be executed when the corresponding accessor is invoked.</span></span>

<span data-ttu-id="2d25f-1053">`get` 存取子會對應至具有屬性類型傳回值的無參數方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1053">A `get` accessor corresponds to a parameterless method with a return value of the property type.</span></span> <span data-ttu-id="2d25f-1054">除了指派的目標以外，在運算式中參考屬性時，會叫用屬性的 `get` 存取子來計算屬性的值（[運算式的值](expressions.md#values-of-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1054">Except as the target of an assignment, when a property is referenced in an expression, the `get` accessor of the property is invoked to compute the value of the property ([Values of expressions](expressions.md#values-of-expressions)).</span></span> <span data-ttu-id="2d25f-1055">`get` 存取子的主體必須符合[方法主體](classes.md#method-body)中所述之傳回值之方法的規則。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1055">The body of a `get` accessor must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="2d25f-1056">特別是 `get` 存取子主體中的所有 `return` 語句，都必須指定可隱含轉換成屬性類型的運算式。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1056">In particular, all `return` statements in the body of a `get` accessor must specify an expression that is implicitly convertible to the property type.</span></span> <span data-ttu-id="2d25f-1057">此外，也不得連線到 `get` 存取子的端點。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1057">Furthermore, the endpoint of a `get` accessor must not be reachable.</span></span>

<span data-ttu-id="2d25f-1058">`set` 存取子會對應至方法，其中包含屬性類型的單一值參數和 `void` 傳回類型。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1058">A `set` accessor corresponds to a method with a single value parameter of the property type and a `void` return type.</span></span> <span data-ttu-id="2d25f-1059">`set` 存取子的隱含參數一律會命名為 `value`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1059">The implicit parameter of a `set` accessor is always named `value`.</span></span> <span data-ttu-id="2d25f-1060">當屬性當做指派（[指派運算子](expressions.md#assignment-operators)）的目標來參考時，或作為 `++` 或 `--` 的運算元（後置[遞增和遞減運算子](expressions.md#postfix-increment-and-decrement-operators)、[前置遞增和遞減運算子](expressions.md#prefix-increment-and-decrement-operators)），會使用引數（其值是指派的右手邊或 `++` 或 `--` 運算子右邊的運算元）來叫用 `set` 存取子，以提供新的值（[簡單指派](expressions.md#simple-assignment)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1060">When a property is referenced as the target of an assignment ([Assignment operators](expressions.md#assignment-operators)), or as the operand of `++` or `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators), [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), the `set` accessor is invoked with an argument (whose value is that of the right-hand side of the assignment or the operand of the `++` or `--` operator) that provides the new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="2d25f-1061">`set` 存取子的主體必須符合[方法主體](classes.md#method-body)中所述 `void` 方法的規則。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1061">The body of a `set` accessor must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="2d25f-1062">特別是，不允許 `set` 存取子主體中的 `return` 語句指定運算式。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1062">In particular, `return` statements in the `set` accessor body are not permitted to specify an expression.</span></span> <span data-ttu-id="2d25f-1063">由於 `set` 存取子會隱含地擁有名為 `value`的參數，因此，在 `set` 存取子中的區域變數或常數宣告必須有該名稱，才會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1063">Since a `set` accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declaration in a `set` accessor to have that name.</span></span>

<span data-ttu-id="2d25f-1064">根據 `get` 和 `set` 存取子的存在與否，屬性的分類方式如下：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1064">Based on the presence or absence of the `get` and `set` accessors, a property is classified as follows:</span></span>

*  <span data-ttu-id="2d25f-1065">包含 `get` 存取子和 `set` 存取子的屬性稱為***讀寫***屬性。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1065">A property that includes both a `get` accessor and a `set` accessor is said to be a ***read-write*** property.</span></span>
*  <span data-ttu-id="2d25f-1066">只有 `get` 存取子的屬性稱為***唯讀***屬性。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1066">A property that has only a `get` accessor is said to be a ***read-only*** property.</span></span> <span data-ttu-id="2d25f-1067">唯讀屬性為指派的目標時，會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1067">It is a compile-time error for a read-only property to be the target of an assignment.</span></span>
*  <span data-ttu-id="2d25f-1068">只有 `set` 存取子的屬性稱為「僅***限寫入***」屬性。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1068">A property that has only a `set` accessor is said to be a ***write-only*** property.</span></span> <span data-ttu-id="2d25f-1069">除了指派的目標以外，在運算式中參考僅限寫入屬性的編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1069">Except as the target of an assignment, it is a compile-time error to reference a write-only property in an expression.</span></span>

<span data-ttu-id="2d25f-1070">在範例中</span><span class="sxs-lookup"><span data-stu-id="2d25f-1070">In the example</span></span>
```csharp
public class Button: Control
{
    private string caption;

    public string Caption {
        get {
            return caption;
        }
        set {
            if (caption != value) {
                caption = value;
                Repaint();
            }
        }
    }

    public override void Paint(Graphics g, Rectangle r) {
        // Painting code goes here
    }
}
```
<span data-ttu-id="2d25f-1071">`Button` 控制項會宣告公用的 `Caption` 屬性。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1071">the `Button` control declares a public `Caption` property.</span></span> <span data-ttu-id="2d25f-1072">`Caption` 屬性的 `get` 存取子會傳回儲存在私用 `caption` 欄位中的字串。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1072">The `get` accessor of the `Caption` property returns the string stored in the private `caption` field.</span></span> <span data-ttu-id="2d25f-1073">`set` 存取子會檢查新值是否與目前的值不同，如果是，則會儲存新的值，並重新繪製控制項。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1073">The `set` accessor checks if the new value is different from the current value, and if so, it stores the new value and repaints the control.</span></span> <span data-ttu-id="2d25f-1074">屬性通常會遵循上面顯示的模式： `get` 存取子只會傳回儲存在私用欄位中的值，而 `set` 存取子則會修改該私用欄位，然後執行完全更新物件狀態所需的任何其他動作。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1074">Properties often follow the pattern shown above: The `get` accessor simply returns a value stored in a private field, and the `set` accessor modifies that private field and then performs any additional actions required to fully update the state of the object.</span></span>

<span data-ttu-id="2d25f-1075">假設上述的 `Button` 類別，以下是使用 `Caption` 屬性的範例：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1075">Given the `Button` class above, the following is an example of use of the `Caption` property:</span></span>
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

<span data-ttu-id="2d25f-1076">在這裡，`set` 存取子是藉由指派值給屬性來叫用，而 `get` 存取子則是藉由參考運算式中的屬性來叫用。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1076">Here, the `set` accessor is invoked by assigning a value to the property, and the `get` accessor is invoked by referencing the property in an expression.</span></span>

<span data-ttu-id="2d25f-1077">屬性（property）的 `get` 和 `set` 存取子不是不同的成員，而且不可能分別宣告屬性的存取子。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1077">The `get` and `set` accessors of a property are not distinct members, and it is not possible to declare the accessors of a property separately.</span></span> <span data-ttu-id="2d25f-1078">因此，讀寫屬性的兩個存取子不可能有不同的存取範圍。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1078">As such, it is not possible for the two accessors of a read-write property to have different accessibility.</span></span> <span data-ttu-id="2d25f-1079">範例</span><span class="sxs-lookup"><span data-stu-id="2d25f-1079">The example</span></span>
```csharp
class A
{
    private string name;

    public string Name {                // Error, duplicate member name
        get { return name; }
    }

    public string Name {                // Error, duplicate member name
        set { name = value; }
    }
}
```
<span data-ttu-id="2d25f-1080">不會宣告單一讀寫屬性。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1080">does not declare a single read-write property.</span></span> <span data-ttu-id="2d25f-1081">相反地，它會宣告兩個具有相同名稱的屬性，一個是唯讀，一個只寫入。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1081">Rather, it declares two properties with the same name, one read-only and one write-only.</span></span> <span data-ttu-id="2d25f-1082">由於在同一個類別中宣告的兩個成員不能有相同的名稱，因此此範例會導致編譯時期錯誤發生。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1082">Since two members declared in the same class cannot have the same name, the example causes a compile-time error to occur.</span></span>

<span data-ttu-id="2d25f-1083">當衍生類別宣告的屬性與繼承屬性的名稱相同時，衍生的屬性會隱藏與讀取和寫入有關的繼承屬性。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1083">When a derived class declares a property by the same name as an inherited property, the derived property hides the inherited property with respect to both reading and writing.</span></span> <span data-ttu-id="2d25f-1084">在範例中</span><span class="sxs-lookup"><span data-stu-id="2d25f-1084">In the example</span></span>
```csharp
class A
{
    public int P {
        set {...}
    }
}

class B: A
{
    new public int P {
        get {...}
    }
}
```
<span data-ttu-id="2d25f-1085">`B` 中的 `P` 屬性會在讀取和寫入方面隱藏 `A` 中的 `P` 屬性。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1085">the `P` property in `B` hides the `P` property in `A` with respect to both reading and writing.</span></span> <span data-ttu-id="2d25f-1086">因此，在語句中</span><span class="sxs-lookup"><span data-stu-id="2d25f-1086">Thus, in the statements</span></span>
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
<span data-ttu-id="2d25f-1087">`b.P` 的指派會導致回報編譯時期錯誤，因為 `B` 中的唯讀 `P` 屬性會隱藏 `A`中的寫入 `P` 屬性。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1087">the assignment to `b.P` causes a compile-time error to be reported, since the read-only `P` property in `B` hides the write-only `P` property in `A`.</span></span> <span data-ttu-id="2d25f-1088">不過，請注意，轉換可以用來存取隱藏的 `P` 屬性。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1088">Note, however, that a cast can be used to access the hidden `P` property.</span></span>

<span data-ttu-id="2d25f-1089">不同于公用欄位，屬性會提供物件的內部狀態與其公用介面之間的分隔。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1089">Unlike public fields, properties provide a separation between an object's internal state and its public interface.</span></span> <span data-ttu-id="2d25f-1090">請考慮下列範例：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1090">Consider the example:</span></span>
```csharp
class Label
{
    private int x, y;
    private string caption;

    public Label(int x, int y, string caption) {
        this.x = x;
        this.y = y;
        this.caption = caption;
    }

    public int X {
        get { return x; }
    }

    public int Y {
        get { return y; }
    }

    public Point Location {
        get { return new Point(x, y); }
    }

    public string Caption {
        get { return caption; }
    }
}
```

<span data-ttu-id="2d25f-1091">在這裡，`Label` 類別會使用兩個 `int` 欄位（`x` 和 `y`）來儲存其位置。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1091">Here, the `Label` class uses two `int` fields, `x` and `y`, to store its location.</span></span> <span data-ttu-id="2d25f-1092">位置會公開為 `X` 和 `Y` 屬性，以及 `Point`類型的 `Location` 屬性。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1092">The location is publicly exposed both as an `X` and a `Y` property and as a `Location` property of type `Point`.</span></span> <span data-ttu-id="2d25f-1093">如果在未來的 `Label`版本中，將位置儲存為內部 `Point` 會變得更方便，但不會影響類別的公用介面，因此可以進行變更：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1093">If, in a future version of `Label`, it becomes more convenient to store the location as a `Point` internally, the change can be made without affecting the public interface of the class:</span></span>
```csharp
class Label
{
    private Point location;
    private string caption;

    public Label(int x, int y, string caption) {
        this.location = new Point(x, y);
        this.caption = caption;
    }

    public int X {
        get { return location.x; }
    }

    public int Y {
        get { return location.y; }
    }

    public Point Location {
        get { return location; }
    }

    public string Caption {
        get { return caption; }
    }
}
```

<span data-ttu-id="2d25f-1094">`x`，而 `y` 改為 `public readonly` 欄位，就無法對 `Label` 類別進行這類變更。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1094">Had `x` and `y` instead been `public readonly` fields, it would have been impossible to make such a change to the `Label` class.</span></span>

<span data-ttu-id="2d25f-1095">透過屬性公開狀態不一定要比直接公開欄位更有效率。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1095">Exposing state through properties is not necessarily any less efficient than exposing fields directly.</span></span> <span data-ttu-id="2d25f-1096">特別是當屬性不是虛擬，而且只包含少量的程式碼時，執行環境可能會以存取子的實際程式碼來取代存取子的呼叫。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1096">In particular, when a property is non-virtual and contains only a small amount of code, the execution environment may replace calls to accessors with the actual code of the accessors.</span></span> <span data-ttu-id="2d25f-1097">此程式稱為「***內嵌***」，它可讓屬性存取的效率與欄位存取一樣有效率，但仍會保留更多的屬性彈性。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1097">This process is known as ***inlining***, and it makes property access as efficient as field access, yet preserves the increased flexibility of properties.</span></span>

<span data-ttu-id="2d25f-1098">因為叫用 `get` 存取子在概念上相當於讀取欄位的值，所以將其視為不正確的程式設計樣式，以讓 `get` 存取子有明顯的副作用。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1098">Since invoking a `get` accessor is conceptually equivalent to reading the value of a field, it is considered bad programming style for `get` accessors to have observable side-effects.</span></span> <span data-ttu-id="2d25f-1099">在範例中</span><span class="sxs-lookup"><span data-stu-id="2d25f-1099">In the example</span></span>
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
<span data-ttu-id="2d25f-1100">`Next` 屬性的值取決於屬性先前被存取的次數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1100">the value of the `Next` property depends on the number of times the property has previously been accessed.</span></span> <span data-ttu-id="2d25f-1101">因此，存取屬性會產生可觀察的副作用，而屬性應實作為方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1101">Thus, accessing the property produces an observable side-effect, and the property should be implemented as a method instead.</span></span>

<span data-ttu-id="2d25f-1102">`get` 存取子的「沒有副作用」慣例並不表示 `get` 存取子應該一律撰寫成隻傳回儲存在欄位中的值。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1102">The "no side-effects" convention for `get` accessors doesn't mean that `get` accessors should always be written to simply return values stored in fields.</span></span> <span data-ttu-id="2d25f-1103">事實上，`get` 存取子通常會藉由存取多個欄位或叫用方法來計算屬性的值。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1103">Indeed, `get` accessors often compute the value of a property by accessing multiple fields or invoking methods.</span></span> <span data-ttu-id="2d25f-1104">不過，適當設計的 `get` 存取子不會執行任何動作，而導致物件狀態中的可觀察變更。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1104">However, a properly designed `get` accessor performs no actions that cause observable changes in the state of the object.</span></span>

<span data-ttu-id="2d25f-1105">屬性可以用來延遲資源的初始化，直到第一次參考它為止。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1105">Properties can be used to delay initialization of a resource until the moment it is first referenced.</span></span> <span data-ttu-id="2d25f-1106">例如：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1106">For example:</span></span>
```csharp
using System.IO;

public class Console
{
    private static TextReader reader;
    private static TextWriter writer;
    private static TextWriter error;

    public static TextReader In {
        get {
            if (reader == null) {
                reader = new StreamReader(Console.OpenStandardInput());
            }
            return reader;
        }
    }

    public static TextWriter Out {
        get {
            if (writer == null) {
                writer = new StreamWriter(Console.OpenStandardOutput());
            }
            return writer;
        }
    }

    public static TextWriter Error {
        get {
            if (error == null) {
                error = new StreamWriter(Console.OpenStandardError());
            }
            return error;
        }
    }
}
```

<span data-ttu-id="2d25f-1107">`Console` 類別包含三個屬性： `In`、`Out`和 `Error`，分別代表標準的輸入、輸出和錯誤裝置。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1107">The `Console` class contains three properties, `In`, `Out`, and `Error`, that represent the standard input, output, and error devices, respectively.</span></span> <span data-ttu-id="2d25f-1108">藉由將這些成員公開為屬性，`Console` 類別可以延遲其初始化，直到實際使用為止。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1108">By exposing these members as properties, the `Console` class can delay their initialization until they are actually used.</span></span> <span data-ttu-id="2d25f-1109">例如，在第一次參考 `Out` 屬性時，如下所示：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1109">For example, upon first referencing the `Out` property, as in</span></span>
```csharp
Console.Out.WriteLine("hello, world");
```
<span data-ttu-id="2d25f-1110">會建立輸出裝置的基礎 `TextWriter`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1110">the underlying `TextWriter` for the output device is created.</span></span> <span data-ttu-id="2d25f-1111">但是，如果應用程式不會參考 `In` 並 `Error` 屬性，則不會為這些裝置建立任何物件。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1111">But if the application makes no reference to the `In` and `Error` properties, then no objects are created for those devices.</span></span>

### <a name="automatically-implemented-properties"></a><span data-ttu-id="2d25f-1112">自動執行的屬性</span><span class="sxs-lookup"><span data-stu-id="2d25f-1112">Automatically implemented properties</span></span>

<span data-ttu-id="2d25f-1113">自動執行的屬性（簡稱為「***自動」屬性***）是具有僅限分號存取子主體的非抽象非外部屬性。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1113">An automatically implemented property (or ***auto-property*** for short), is a non-abstract non-extern property with semicolon-only accessor bodies.</span></span> <span data-ttu-id="2d25f-1114">Auto 屬性必須有 get 存取子，而且可以選擇性地擁有 set 存取子。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1114">Auto-properties must have a get accessor and can optionally have a set accessor.</span></span>

<span data-ttu-id="2d25f-1115">當屬性指定為自動執行的屬性時，會自動提供屬性的隱藏支援欄位，而且會實作為讀取和寫入該支援欄位的存取子。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1115">When a property is specified as an automatically implemented property, a hidden backing field is automatically available for the property, and the accessors are implemented to read from and write to that backing field.</span></span> <span data-ttu-id="2d25f-1116">如果 auto 屬性沒有 set 存取子，則會將支援欄位視為 `readonly` （[Readonly 欄位](classes.md#readonly-fields)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1116">If the auto-property has no set accessor, the backing field is considered `readonly` ([Readonly fields](classes.md#readonly-fields)).</span></span> <span data-ttu-id="2d25f-1117">就像 `readonly` 欄位一樣，僅限 getter 的 auto 屬性也可以指派給封閉式類別的函式主體。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1117">Just like a `readonly` field, a getter-only auto-property can also be assigned to in the body of a constructor of the enclosing class.</span></span> <span data-ttu-id="2d25f-1118">這類指派會直接指派給屬性的 readonly 支援欄位。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1118">Such an assignment assigns directly to the readonly backing field of the property.</span></span>

<span data-ttu-id="2d25f-1119">Auto 屬性可以選擇性地具有*property_initializer*，這會直接套用至支援欄位做為*variable_initializer* （[變數初始化運算式](classes.md#variable-initializers)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1119">An auto-property may optionally have a *property_initializer*, which is applied directly to the backing field as a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)).</span></span>

<span data-ttu-id="2d25f-1120">下列範例：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1120">The following example:</span></span>
```csharp
public class Point {
    public int X { get; set; } = 0;
    public int Y { get; set; } = 0;
}
```
<span data-ttu-id="2d25f-1121">相當於下列宣告：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1121">is equivalent to the following declaration:</span></span>
```csharp
public class Point {
    private int __x = 0;
    private int __y = 0;
    public int X { get { return __x; } set { __x = value; } }
    public int Y { get { return __y; } set { __y = value; } }
}
```

<span data-ttu-id="2d25f-1122">下列範例：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1122">The following example:</span></span>
```csharp
public class ReadOnlyPoint
{
    public int X { get; }
    public int Y { get; }
    public ReadOnlyPoint(int x, int y) { X = x; Y = y; }
}
```
<span data-ttu-id="2d25f-1123">相當於下列宣告：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1123">is equivalent to the following declaration:</span></span>
```csharp
public class ReadOnlyPoint
{
    private readonly int __x;
    private readonly int __y;
    public int X { get { return __x; } }
    public int Y { get { return __y; } }
    public ReadOnlyPoint(int x, int y) { __x = x; __y = y; }
}
```

<span data-ttu-id="2d25f-1124">請注意，[唯讀] 欄位的指派是合法的，因為它們會出現在此函式中。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1124">Notice that the assignments to the readonly field are legal, because they occur within the constructor.</span></span>


### <a name="accessibility"></a><span data-ttu-id="2d25f-1125">協助工具選項</span><span class="sxs-lookup"><span data-stu-id="2d25f-1125">Accessibility</span></span>

<span data-ttu-id="2d25f-1126">如果存取子具有*accessor_modifier*，存取子的存取範圍定義域（[存取](basic-concepts.md#accessibility-domains)範圍網域）就會使用*accessor_modifier*的宣告存取範圍來決定。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1126">If an accessor has an *accessor_modifier*, the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the accessor is determined using the declared accessibility of the *accessor_modifier*.</span></span> <span data-ttu-id="2d25f-1127">如果存取子沒有*accessor_modifier*，存取子的存取範圍定義域就會從屬性或索引子的宣告存取範圍來決定。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1127">If an accessor does not have an *accessor_modifier*, the accessibility domain of the accessor is determined from the declared accessibility of the property or indexer.</span></span>

<span data-ttu-id="2d25f-1128">*Accessor_modifier*的存在不會影響成員查閱（[運算子](expressions.md#operators)）或多載解析（多載[解析](expressions.md#overload-resolution)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1128">The presence of an *accessor_modifier* never affects member lookup ([Operators](expressions.md#operators)) or overload resolution ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="2d25f-1129">屬性或索引子上的修飾詞一律會決定系結至哪一個屬性或索引子，而不論存取的內容為何。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1129">The modifiers on the property or indexer always determine which property or indexer is bound to, regardless of the context of the access.</span></span>

<span data-ttu-id="2d25f-1130">一旦選取了特定的屬性或索引子，就會使用所涉及之特定存取子的存取範圍定義域來判斷該用法是否有效：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1130">Once a particular property or indexer has been selected, the accessibility domains of the specific accessors involved are used to determine if that usage is valid:</span></span>

*  <span data-ttu-id="2d25f-1131">如果使用方式為值（[運算式的值](expressions.md#values-of-expressions)），則 `get` 存取子必須存在且可供存取。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1131">If the usage is as a value ([Values of expressions](expressions.md#values-of-expressions)), the `get` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="2d25f-1132">如果使用方式是做為簡單指派（[簡單指派](expressions.md#simple-assignment)）的目標，則 `set` 存取子必須存在且可供存取。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1132">If the usage is as the target of a simple assignment ([Simple assignment](expressions.md#simple-assignment)), the `set` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="2d25f-1133">如果使用方式是複合指派（[複合指派](expressions.md#compound-assignment)）的目標，或做為 `++` 或 `--` 運算子（函式[成員](expressions.md#function-members).9，[調用運算式](expressions.md#invocation-expressions)）的目標，則 `get` 存取子和 `set` 存取子必須存在且可供存取。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1133">If the usage is as the target of compound assignment ([Compound assignment](expressions.md#compound-assignment)), or as the target of the `++` or `--` operators ([Function members](expressions.md#function-members).9, [Invocation expressions](expressions.md#invocation-expressions)), both the `get` accessors and the `set` accessor must exist and be accessible.</span></span>

<span data-ttu-id="2d25f-1134">在下列範例中，屬性 `B.Text`會隱藏屬性 `A.Text`，即使在只呼叫 `set` 存取子的內容中也是如此。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1134">In the following example, the property `A.Text` is hidden by the property `B.Text`, even in contexts where only the `set` accessor is called.</span></span> <span data-ttu-id="2d25f-1135">相反地，類別 `M`無法存取屬性 `B.Count`，因此會改用可存取的屬性 `A.Count`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1135">In contrast, the property `B.Count` is not accessible to class `M`, so the accessible property `A.Count` is used instead.</span></span>

```csharp
class A
{
    public string Text {
        get { return "hello"; }
        set { }
    }

    public int Count {
        get { return 5; }
        set { }
    }
}

class B: A
{
    private string text = "goodbye"; 
    private int count = 0;

    new public string Text {
        get { return text; }
        protected set { text = value; }
    }

    new protected int Count { 
        get { return count; }
        set { count = value; }
    }
}

class M
{
    static void Main() {
        B b = new B();
        b.Count = 12;             // Calls A.Count set accessor
        int i = b.Count;          // Calls A.Count get accessor
        b.Text = "howdy";         // Error, B.Text set accessor not accessible
        string s = b.Text;        // Calls B.Text get accessor
    }
}
```

<span data-ttu-id="2d25f-1136">用來執行介面的存取子可能沒有*accessor_modifier*。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1136">An accessor that is used to implement an interface may not have an *accessor_modifier*.</span></span> <span data-ttu-id="2d25f-1137">如果只有一個存取子是用來執行介面，則可以使用*accessor_modifier*來宣告另一個存取子：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1137">If only one accessor is used to implement an interface, the other accessor may be declared with an *accessor_modifier*:</span></span>
```csharp
public interface I
{
    string Prop { get; }
}

public class C: I
{
    public string Prop {
        get { return "April"; }       // Must not have a modifier here
        internal set {...}            // Ok, because I.Prop has no set accessor
    }
}
```

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a><span data-ttu-id="2d25f-1138">虛擬、密封、覆寫和抽象屬性存取子</span><span class="sxs-lookup"><span data-stu-id="2d25f-1138">Virtual, sealed, override, and abstract property accessors</span></span>

<span data-ttu-id="2d25f-1139">`virtual` 屬性宣告會指定屬性的存取子是虛擬的。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1139">A `virtual` property declaration specifies that the accessors of the property are virtual.</span></span> <span data-ttu-id="2d25f-1140">`virtual` 修飾詞會套用到讀寫屬性的兩個存取子，而唯讀屬性的一個存取子則不可能是虛擬的。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1140">The `virtual` modifier applies to both accessors of a read-write property—it is not possible for only one accessor of a read-write property to be virtual.</span></span>

<span data-ttu-id="2d25f-1141">`abstract` 屬性宣告會指定屬性的存取子是虛擬的，但不提供存取子的實際執行。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1141">An `abstract` property declaration specifies that the accessors of the property are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="2d25f-1142">相反地，非抽象衍生類別必須藉由覆寫屬性，為存取子提供自己的執行。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1142">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the property.</span></span> <span data-ttu-id="2d25f-1143">因為抽象屬性宣告的存取子不提供實際的實值，所以它的*accessor_body*只會包含一個分號。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1143">Because an accessor for an abstract property declaration provides no actual implementation, its *accessor_body* simply consists of a semicolon.</span></span>

<span data-ttu-id="2d25f-1144">同時包含 `abstract` 和 `override` 修飾詞的屬性宣告，會指定屬性為抽象，並覆寫基底屬性。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1144">A property declaration that includes both the `abstract` and `override` modifiers specifies that the property is abstract and overrides a base property.</span></span> <span data-ttu-id="2d25f-1145">這類屬性的存取子也是抽象的。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1145">The accessors of such a property are also abstract.</span></span>

<span data-ttu-id="2d25f-1146">抽象屬性宣告只允許用於抽象類別（[抽象類別](classes.md#abstract-classes)）。藉由包含指定 `override` 指示詞的屬性宣告，可以在衍生類別中覆寫繼承的虛擬屬性的存取子。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1146">Abstract property declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).The accessors of an inherited virtual property can be overridden in a derived class by including a property declaration that specifies an `override` directive.</span></span> <span data-ttu-id="2d25f-1147">這就是所謂的覆***寫屬性***宣告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1147">This is known as an ***overriding property declaration***.</span></span> <span data-ttu-id="2d25f-1148">覆寫屬性宣告不會宣告新的屬性。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1148">An overriding property declaration does not declare a new property.</span></span> <span data-ttu-id="2d25f-1149">相反地，它只會特製化現有虛擬屬性的存取子。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1149">Instead, it simply specializes the implementations of the accessors of an existing virtual property.</span></span>

<span data-ttu-id="2d25f-1150">覆寫屬性宣告必須指定與繼承的屬性完全相同的存取範圍修飾詞、類型和名稱。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1150">An overriding property declaration must specify the exact same accessibility modifiers, type, and name as the inherited property.</span></span> <span data-ttu-id="2d25f-1151">如果繼承的屬性只有一個存取子（也就是，如果繼承的屬性是唯讀或僅限寫入），則覆寫屬性必須只包含該存取子。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1151">If the inherited property has only a single accessor (i.e., if the inherited property is read-only or write-only), the overriding property must include only that accessor.</span></span> <span data-ttu-id="2d25f-1152">如果繼承的屬性同時包含這兩個存取子（也就是，如果繼承的屬性是讀寫），則覆寫屬性可以包含單一存取子或兩個存取子。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1152">If the inherited property includes both accessors (i.e., if the inherited property is read-write), the overriding property can include either a single accessor or both accessors.</span></span>

<span data-ttu-id="2d25f-1153">覆寫屬性宣告可能包含 `sealed` 修飾詞。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1153">An overriding property declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="2d25f-1154">使用此修飾詞可防止衍生類別進一步覆寫屬性。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1154">Use of this modifier prevents a derived class from further overriding the property.</span></span> <span data-ttu-id="2d25f-1155">密封屬性的存取子也是密封的。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1155">The accessors of a sealed property are also sealed.</span></span>

<span data-ttu-id="2d25f-1156">除了宣告和調用語法的差異之外，virtual、sealed、override 和 abstract 存取子的行為與虛擬、sealed、override 和 abstract 方法完全相同。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1156">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="2d25f-1157">具體而言，[虛擬方法](classes.md#virtual-methods)、覆[寫方法](classes.md#override-methods)、[密封方法](classes.md#sealed-methods)和[抽象方法](classes.md#abstract-methods)中所述的規則，會如同存取子是對應格式的方法一樣適用：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1157">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form:</span></span>

*  <span data-ttu-id="2d25f-1158">`get` 存取子會對應到無參數的方法，其中包含屬性類型的傳回值，以及與包含屬性相同的修飾詞。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1158">A `get` accessor corresponds to a parameterless method with a return value of the property type and the same modifiers as the containing property.</span></span>
*  <span data-ttu-id="2d25f-1159">`set` 存取子會對應至方法，其中包含屬性類型的單一值參數、`void` 傳回類型，以及與包含屬性相同的修飾詞。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1159">A `set` accessor corresponds to a method with a single value parameter of the property type, a `void` return type, and the same modifiers as the containing property.</span></span>

<span data-ttu-id="2d25f-1160">在範例中</span><span class="sxs-lookup"><span data-stu-id="2d25f-1160">In the example</span></span>
```csharp
abstract class A
{
    int y;

    public virtual int X {
        get { return 0; }
    }

    public virtual int Y {
        get { return y; }
        set { y = value; }
    }

    public abstract int Z { get; set; }
}
```
<span data-ttu-id="2d25f-1161">`X` 是虛擬唯讀屬性，`Y` 是虛擬讀寫屬性，而 `Z` 則是抽象的讀寫屬性。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1161">`X` is a virtual read-only property, `Y` is a virtual read-write property, and `Z` is an abstract read-write property.</span></span> <span data-ttu-id="2d25f-1162">因為 `Z` 是抽象的，所以包含類別 `A` 也必須宣告為抽象。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1162">Because `Z` is abstract, the containing class `A` must also be declared abstract.</span></span>

<span data-ttu-id="2d25f-1163">衍生自 `A` 的類別如下所示：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1163">A class that derives from `A` is show below:</span></span>
```csharp
class B: A
{
    int z;

    public override int X {
        get { return base.X + 1; }
    }

    public override int Y {
        set { base.Y = value < 0? 0: value; }
    }

    public override int Z {
        get { return z; }
        set { z = value; }
    }
}
```

<span data-ttu-id="2d25f-1164">在這裡，`X`、`Y`和 `Z` 的宣告會覆寫屬性宣告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1164">Here, the declarations of `X`, `Y`, and `Z` are overriding property declarations.</span></span> <span data-ttu-id="2d25f-1165">每個屬性宣告完全符合協助工具修飾詞、類型和對應繼承屬性的名稱。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1165">Each property declaration exactly matches the accessibility modifiers, type, and name of the corresponding inherited property.</span></span> <span data-ttu-id="2d25f-1166">`X` 的 `get` 存取子和 `Y` 的 `set` 存取子會使用 `base` 關鍵字來存取繼承的存取子。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1166">The `get` accessor of `X` and the `set` accessor of `Y` use the `base` keyword to access the inherited accessors.</span></span> <span data-ttu-id="2d25f-1167">`Z` 的宣告會覆寫抽象存取子，因此 `B`中沒有任何未處理的抽象函數成員，而 `B` 則允許成為非抽象類別。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1167">The declaration of `Z` overrides both abstract accessors—thus, there are no outstanding abstract function members in `B`, and `B` is permitted to be a non-abstract class.</span></span>

<span data-ttu-id="2d25f-1168">當屬性宣告為 `override`時，所有覆寫的存取子都必須可供覆寫程式碼存取。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1168">When a property is declared as an `override`, any overridden accessors must be accessible to the overriding code.</span></span> <span data-ttu-id="2d25f-1169">此外，屬性或索引子本身和存取子的宣告存取範圍，都必須符合覆寫成員和存取子的。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1169">In addition, the declared accessibility of both the property or indexer itself, and of the accessors, must match that of the overridden member and accessors.</span></span> <span data-ttu-id="2d25f-1170">例如：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1170">For example:</span></span>
```csharp
public class B
{
    public virtual int P {
        protected set {...}
        get {...}
    }
}

public class D: B
{
    public override int P {
        protected set {...}            // Must specify protected here
        get {...}                      // Must not have a modifier here
    }
}
```

## <a name="events"></a><span data-ttu-id="2d25f-1171">事件</span><span class="sxs-lookup"><span data-stu-id="2d25f-1171">Events</span></span>

<span data-ttu-id="2d25f-1172">「***事件***」（event）是可讓物件或類別提供通知的成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1172">An ***event*** is a member that enables an object or class to provide notifications.</span></span> <span data-ttu-id="2d25f-1173">用戶端可以藉由提供***事件處理常式***，附加事件的可執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1173">Clients can attach executable code for events by supplying ***event handlers***.</span></span>

<span data-ttu-id="2d25f-1174">事件是使用*event_declaration*s 來宣告：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1174">Events are declared using *event_declaration*s:</span></span>

```antlr
event_declaration
    : attributes? event_modifier* 'event' type variable_declarators ';'
    | attributes? event_modifier* 'event' type member_name '{' event_accessor_declarations '}'
    ;

event_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | event_modifier_unsafe
    ;

event_accessor_declarations
    : add_accessor_declaration remove_accessor_declaration
    | remove_accessor_declaration add_accessor_declaration
    ;

add_accessor_declaration
    : attributes? 'add' block
    ;

remove_accessor_declaration
    : attributes? 'remove' block
    ;
```

<span data-ttu-id="2d25f-1175">*Event_declaration*可能包括一組*屬性*（attribute）和四個存取修飾[詞（](attributes.md)[存取](classes.md#access-modifiers)修飾詞）的有效組合、`new` （[新的修飾](classes.md#the-new-modifier)詞）、`static` （[靜態和實例方法](classes.md#static-and-instance-methods)）、`virtual` （[虛擬方法](classes.md#virtual-methods)）、`override` （覆[寫方法](classes.md#override-methods)）、`sealed` （[密封方法](classes.md#sealed-methods)）、`abstract` （[抽象方法](classes.md#abstract-methods)），以及 `extern` （[外部方法](classes.md#external-methods)）修飾詞。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1175">An *event_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="2d25f-1176">事件宣告受限於與方法宣告（[方法](classes.md#methods)）相同的規則，如同有效的修飾片語合。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1176">Event declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="2d25f-1177">事件宣告的*類型*必須是*delegate_type* （[參考型別](types.md#reference-types)），而且*Delegate_type*必須至少與事件本身（[存取範圍條件約束](basic-concepts.md#accessibility-constraints)）相同。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1177">The *type* of an event declaration must be a *delegate_type* ([Reference types](types.md#reference-types)), and that *delegate_type* must be at least as accessible as the event itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="2d25f-1178">事件宣告可能包含*event_accessor_declarations*。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1178">An event declaration may include *event_accessor_declarations*.</span></span> <span data-ttu-id="2d25f-1179">不過，如果不是在非外部的非抽象事件中，則編譯器會自動提供它們（[類似欄位的事件](classes.md#field-like-events)）;若是外來事件，存取子會在外部提供。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1179">However, if it does not, for non-extern, non-abstract events, the compiler supplies them automatically ([Field-like events](classes.md#field-like-events)); for extern events, the accessors are provided externally.</span></span>

<span data-ttu-id="2d25f-1180">省略*event_accessor_declarations*的事件宣告會定義一或多個事件，每個*variable_declarator*各一個。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1180">An event declaration that omits *event_accessor_declarations* defines one or more events—one for each of the *variable_declarator*s.</span></span> <span data-ttu-id="2d25f-1181">屬性和修飾詞適用于這類*event_declaration*所宣告的所有成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1181">The attributes and modifiers apply to all of the members declared by such an *event_declaration*.</span></span>

<span data-ttu-id="2d25f-1182">*Event_declaration*要同時包含 `abstract` 修飾詞和以大括弧分隔的*event_accessor_declarations*時，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1182">It is a compile-time error for an *event_declaration* to include both the `abstract` modifier and brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="2d25f-1183">當事件宣告包含 `extern` 修飾詞時，事件稱為「***外來事件***」。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1183">When an event declaration includes an `extern` modifier, the event is said to be an ***external event***.</span></span> <span data-ttu-id="2d25f-1184">因為外來事件宣告不提供實際的實值，所以它會發生錯誤，同時包含 `extern` 修飾詞和*event_accessor_declarations*。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1184">Because an external event declaration provides no actual implementation, it is an error for it to include both the `extern` modifier and *event_accessor_declarations*.</span></span>

<span data-ttu-id="2d25f-1185">如果事件宣告的*variable_declarator*具有 `abstract` 或 `external` 修飾詞以包含*variable_initializer*，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1185">It is a compile-time error for a *variable_declarator* of an event declaration with an `abstract` or `external` modifier to include a *variable_initializer*.</span></span>

<span data-ttu-id="2d25f-1186">事件可用來做為 `+=` 和 `-=` 運算子（[事件指派](expressions.md#event-assignment)）的左運算元。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1186">An event can be used as the left-hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="2d25f-1187">這些運算子分別用來將事件處理常式附加至或，以從事件中移除事件處理常式，而事件的存取修飾詞會控制允許這類作業的內容。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1187">These operators are used, respectively, to attach event handlers to or to remove event handlers from an event, and the access modifiers of the event control the contexts in which such operations are permitted.</span></span>

<span data-ttu-id="2d25f-1188">因為 `+=` 和 `-=` 是在宣告事件的類型外的事件上所允許的唯一作業，所以外部程式碼可以加入和移除事件的處理常式，但無法以任何其他方式取得或修改事件處理常式的基礎清單。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1188">Since `+=` and `-=` are the only operations that are permitted on an event outside the type that declares the event, external code can add and remove handlers for an event, but cannot in any other way obtain or modify the underlying list of event handlers.</span></span>

<span data-ttu-id="2d25f-1189">在表單 `x += y` 或 `x -= y`的作業中，當 `x` 是事件，而參考發生在包含 `x`宣告的類型之外時，作業的結果會有類型 `void` （而不是 `x`的類型，而是在指派後的值為 `x`）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1189">In an operation of the form `x += y` or `x -= y`, when `x` is an event and the reference takes place outside the type that contains the declaration of `x`, the result of the operation has type `void` (as opposed to having the type of `x`, with the value of `x` after the assignment).</span></span> <span data-ttu-id="2d25f-1190">此規則會禁止外部程式碼間接檢查事件的基礎委派。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1190">This rule prohibits external code from indirectly examining the underlying delegate of an event.</span></span>

<span data-ttu-id="2d25f-1191">下列範例顯示如何將事件處理常式附加至 `Button` 類別的實例：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1191">The following example shows how event handlers are attached to instances of the `Button` class:</span></span>
```csharp
public delegate void EventHandler(object sender, EventArgs e);

public class Button: Control
{
    public event EventHandler Click;
}

public class LoginDialog: Form
{
    Button OkButton;
    Button CancelButton;

    public LoginDialog() {
        OkButton = new Button(...);
        OkButton.Click += new EventHandler(OkButtonClick);
        CancelButton = new Button(...);
        CancelButton.Click += new EventHandler(CancelButtonClick);
    }

    void OkButtonClick(object sender, EventArgs e) {
        // Handle OkButton.Click event
    }

    void CancelButtonClick(object sender, EventArgs e) {
        // Handle CancelButton.Click event
    }
}
```

<span data-ttu-id="2d25f-1192">在這裡，`LoginDialog` 實例的程式會建立兩個 `Button` 實例，並將事件處理常式附加至 `Click` 事件。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1192">Here, the `LoginDialog` instance constructor creates two `Button` instances and attaches event handlers to the `Click` events.</span></span>

### <a name="field-like-events"></a><span data-ttu-id="2d25f-1193">類似欄位的事件</span><span class="sxs-lookup"><span data-stu-id="2d25f-1193">Field-like events</span></span>

<span data-ttu-id="2d25f-1194">在包含事件宣告的類別或結構的程式文字內，某些事件可以像欄位一樣使用。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1194">Within the program text of the class or struct that contains the declaration of an event, certain events can be used like fields.</span></span> <span data-ttu-id="2d25f-1195">若要以這種方式使用，事件不得 `abstract` 或 `extern`，而且不得明確包含*event_accessor_declarations*。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1195">To be used in this way, an event must not be `abstract` or `extern`, and must not explicitly include *event_accessor_declarations*.</span></span> <span data-ttu-id="2d25f-1196">這類事件可以在允許欄位的任何內容中使用。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1196">Such an event can be used in any context that permits a field.</span></span> <span data-ttu-id="2d25f-1197">欄位包含委派（[委派](delegates.md)），其參考已加入至事件的事件處理常式清單。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1197">The field contains a delegate ([Delegates](delegates.md)) which refers to the list of event handlers that have been added to the event.</span></span> <span data-ttu-id="2d25f-1198">如果尚未加入任何事件處理常式，則欄位會包含 `null`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1198">If no event handlers have been added, the field contains `null`.</span></span>

<span data-ttu-id="2d25f-1199">在範例中</span><span class="sxs-lookup"><span data-stu-id="2d25f-1199">In the example</span></span>
```csharp
public delegate void EventHandler(object sender, EventArgs e);

public class Button: Control
{
    public event EventHandler Click;

    protected void OnClick(EventArgs e) {
        if (Click != null) Click(this, e);
    }

    public void Reset() {
        Click = null;
    }
}
```
<span data-ttu-id="2d25f-1200">`Click` 會當做 `Button` 類別中的欄位使用。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1200">`Click` is used as a field within the `Button` class.</span></span> <span data-ttu-id="2d25f-1201">如範例所示，您可以在委派調用運算式中檢查、修改和使用欄位。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1201">As the example demonstrates, the field can be examined, modified, and used in delegate invocation expressions.</span></span> <span data-ttu-id="2d25f-1202">`Button` 類別中的 `OnClick` 方法會「引發」 `Click` 事件。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1202">The `OnClick` method in the `Button` class "raises" the `Click` event.</span></span> <span data-ttu-id="2d25f-1203">引發事件的概念完全等同於叫用該事件所代表的委派項目，因此，就引發事件而言，並沒有任何特殊的語言建構。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1203">The notion of raising an event is precisely equivalent to invoking the delegate represented by the event—thus, there are no special language constructs for raising events.</span></span> <span data-ttu-id="2d25f-1204">請注意，委派調用的前面會有一個檢查可確保委派為非 null。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1204">Note that the delegate invocation is preceded by a check that ensures the delegate is non-null.</span></span>

<span data-ttu-id="2d25f-1205">在 `Button` 類別的宣告外，`Click` 成員只能在 `+=` 和 `-=` 運算子的左邊使用，如下所示：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1205">Outside the declaration of the `Button` class, the `Click` member can only be used on the left-hand side of the `+=` and `-=` operators, as in</span></span>
```csharp
b.Click += new EventHandler(...);
```
<span data-ttu-id="2d25f-1206">這會將委派附加至 `Click` 事件的調用清單，而</span><span class="sxs-lookup"><span data-stu-id="2d25f-1206">which appends a delegate to the invocation list of the `Click` event, and</span></span>
```csharp
b.Click -= new EventHandler(...);
```
<span data-ttu-id="2d25f-1207">這會從 `Click` 事件的調用清單中移除委派。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1207">which removes a delegate from the invocation list of the `Click` event.</span></span>

<span data-ttu-id="2d25f-1208">當編譯類似欄位的事件時，編譯器會自動建立儲存區來保存委派，並建立事件的存取子，以便將事件處理常式加入至委派欄位。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1208">When compiling a field-like event, the compiler automatically creates storage to hold the delegate, and creates accessors for the event that add or remove event handlers to the delegate field.</span></span> <span data-ttu-id="2d25f-1209">新增和移除作業是安全線程，而且可能（但並非必要）在針對實例事件的包含物件保留鎖定（[lock 語句](statements.md#the-lock-statement)），或靜態事件的類型物件（[匿名物件建立運算式](expressions.md#anonymous-object-creation-expressions)）時，執行（但不需要）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1209">The addition and removal operations are thread safe, and may (but are not required to) be done while holding the lock ([The lock statement](statements.md#the-lock-statement)) on the containing object for an instance event, or the type object ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) for a static event.</span></span>

<span data-ttu-id="2d25f-1210">因此，實例事件宣告的形式如下：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1210">Thus, an instance event declaration of the form:</span></span>
```csharp
class X
{
    public event D Ev;
}
```
<span data-ttu-id="2d25f-1211">將會編譯成相當於的內容：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1211">will be compiled to something equivalent to:</span></span>
```csharp
class X
{
    private D __Ev;  // field to hold the delegate

    public event D Ev {
        add {
            /* add the delegate in a thread safe way */
        }

        remove {
            /* remove the delegate in a thread safe way */
        }
    }
}
```
<span data-ttu-id="2d25f-1212">在類別 `X`中，`+=` 和 `-=` 運算子左側 `Ev` 的參考會導致叫用 add 和 remove 存取子。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1212">Within the class `X`, references to `Ev` on the left-hand side of the `+=` and `-=` operators cause the add and remove accessors to be invoked.</span></span> <span data-ttu-id="2d25f-1213">`Ev` 的所有其他參考都會編譯為參考隱藏欄位 `__Ev` 而不是（[成員存取](expressions.md#member-access)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1213">All other references to `Ev` are compiled to reference the hidden field `__Ev` instead ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="2d25f-1214">"`__Ev`" 名稱是任意的;隱藏欄位可能會有任何名稱或完全沒有名稱。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1214">The name "`__Ev`" is arbitrary; the hidden field could have any name or no name at all.</span></span>

### <a name="event-accessors"></a><span data-ttu-id="2d25f-1215">事件存取子</span><span class="sxs-lookup"><span data-stu-id="2d25f-1215">Event accessors</span></span>

<span data-ttu-id="2d25f-1216">事件宣告通常會省略*event_accessor_declarations*，如上述的 `Button` 範例所示。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1216">Event declarations typically omit *event_accessor_declarations*, as in the `Button` example above.</span></span> <span data-ttu-id="2d25f-1217">執行此動作的其中一種情況，包括無法接受每個事件的一個欄位的儲存成本。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1217">One situation for doing so involves the case in which the storage cost of one field per event is not acceptable.</span></span> <span data-ttu-id="2d25f-1218">在這種情況下，類別可以包含*event_accessor_declarations* ，並使用私用機制來儲存事件處理常式的清單。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1218">In such cases, a class can include *event_accessor_declarations* and use a private mechanism for storing the list of event handlers.</span></span>

<span data-ttu-id="2d25f-1219">事件的*event_accessor_declarations*會指定與加入和移除事件處理常式相關聯的可執行語句。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1219">The *event_accessor_declarations* of an event specify the executable statements associated with adding and removing event handlers.</span></span>

<span data-ttu-id="2d25f-1220">存取子宣告包含*add_accessor_declaration*和*remove_accessor_declaration*。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1220">The accessor declarations consist of an *add_accessor_declaration* and a *remove_accessor_declaration*.</span></span> <span data-ttu-id="2d25f-1221">每個存取子宣告都包含 `add` 或 `remove` 後面接著*區塊*的 token。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1221">Each accessor declaration consists of the token `add` or `remove` followed by a *block*.</span></span> <span data-ttu-id="2d25f-1222">與*add_accessor_declaration*相關聯的*區塊*會指定在加入事件處理常式時要執行的語句，而與*remove_accessor_declaration*相關聯的*區塊*則會指定移除事件處理常式時要執行的語句。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1222">The *block* associated with an *add_accessor_declaration* specifies the statements to execute when an event handler is added, and the *block* associated with a *remove_accessor_declaration* specifies the statements to execute when an event handler is removed.</span></span>

<span data-ttu-id="2d25f-1223">每個*add_accessor_declaration*和*remove_accessor_declaration*都會對應至具有事件種類之單一值參數和 `void` 傳回類型的方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1223">Each *add_accessor_declaration* and *remove_accessor_declaration* corresponds to a method with a single value parameter of the event type and a `void` return type.</span></span> <span data-ttu-id="2d25f-1224">事件存取子的隱含參數名為 `value`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1224">The implicit parameter of an event accessor is named `value`.</span></span> <span data-ttu-id="2d25f-1225">在事件指派中使用事件時，會使用適當的事件存取子。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1225">When an event is used in an event assignment, the appropriate event accessor is used.</span></span> <span data-ttu-id="2d25f-1226">具體而言，如果指派運算子是 `+=` 則會使用 add 存取子，而且如果指派運算子為 `-=`，則會使用 remove 存取子。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1226">Specifically, if the assignment operator is `+=` then the add accessor is used, and if the assignment operator is `-=` then the remove accessor is used.</span></span> <span data-ttu-id="2d25f-1227">在任一情況下，指派運算子的右手運算元會當做事件存取子的引數使用。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1227">In either case, the right-hand operand of the assignment operator is used as the argument to the event accessor.</span></span> <span data-ttu-id="2d25f-1228">*Add_accessor_declaration*或*remove_accessor_declaration*的區塊必須符合[方法主體](classes.md#method-body)中所述 `void` 方法的規則。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1228">The block of an *add_accessor_declaration* or a *remove_accessor_declaration* must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="2d25f-1229">特別是，這類區塊中的 `return` 語句不允許指定運算式。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1229">In particular, `return` statements in such a block are not permitted to specify an expression.</span></span>

<span data-ttu-id="2d25f-1230">由於事件存取子會隱含地具有名為 `value`的參數，因此，在事件存取子中宣告的區域變數或常數必須有該名稱，才會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1230">Since an event accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declared in an event accessor to have that name.</span></span>

<span data-ttu-id="2d25f-1231">在範例中</span><span class="sxs-lookup"><span data-stu-id="2d25f-1231">In the example</span></span>
```csharp
class Control: Component
{
    // Unique keys for events
    static readonly object mouseDownEventKey = new object();
    static readonly object mouseUpEventKey = new object();

    // Return event handler associated with key
    protected Delegate GetEventHandler(object key) {...}

    // Add event handler associated with key
    protected void AddEventHandler(object key, Delegate handler) {...}

    // Remove event handler associated with key
    protected void RemoveEventHandler(object key, Delegate handler) {...}

    // MouseDown event
    public event MouseEventHandler MouseDown {
        add { AddEventHandler(mouseDownEventKey, value); }
        remove { RemoveEventHandler(mouseDownEventKey, value); }
    }

    // MouseUp event
    public event MouseEventHandler MouseUp {
        add { AddEventHandler(mouseUpEventKey, value); }
        remove { RemoveEventHandler(mouseUpEventKey, value); }
    }

    // Invoke the MouseUp event
    protected void OnMouseUp(MouseEventArgs args) {
        MouseEventHandler handler; 
        handler = (MouseEventHandler)GetEventHandler(mouseUpEventKey);
        if (handler != null)
            handler(this, args);
    }
}
```
<span data-ttu-id="2d25f-1232">`Control` 類別會實作為事件的內部儲存機制。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1232">the `Control` class implements an internal storage mechanism for events.</span></span> <span data-ttu-id="2d25f-1233">`AddEventHandler` 方法會將委派值與索引鍵產生關聯，`GetEventHandler` 方法會傳回目前與索引鍵相關聯的委派，而 `RemoveEventHandler` 方法則會移除委派，做為指定事件的事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1233">The `AddEventHandler` method associates a delegate value with a key, the `GetEventHandler` method returns the delegate currently associated with a key, and the `RemoveEventHandler` method removes a delegate as an event handler for the specified event.</span></span> <span data-ttu-id="2d25f-1234">大概而言，基礎儲存機制的設計，是將 `null` 委派值與金鑰建立關聯不會產生任何費用，因此未處理的事件就不會耗用任何儲存體。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1234">Presumably, the underlying storage mechanism is designed such that there is no cost for associating a `null` delegate value with a key, and thus unhandled events consume no storage.</span></span>

### <a name="static-and-instance-events"></a><span data-ttu-id="2d25f-1235">靜態和實例事件</span><span class="sxs-lookup"><span data-stu-id="2d25f-1235">Static and instance events</span></span>

<span data-ttu-id="2d25f-1236">當事件宣告包含 `static` 修飾詞時，事件稱為***靜態事件***。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1236">When an event declaration includes a `static` modifier, the event is said to be a ***static event***.</span></span> <span data-ttu-id="2d25f-1237">當沒有 `static` 的修飾詞時，事件就稱為「實例」***事件***。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1237">When no `static` modifier is present, the event is said to be an ***instance event***.</span></span>

<span data-ttu-id="2d25f-1238">靜態事件不會與特定實例相關聯，而且會發生編譯時期錯誤，以在靜態事件的存取子中參考 `this`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1238">A static event is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static event.</span></span>

<span data-ttu-id="2d25f-1239">實例事件與類別的指定實例相關聯，而且可以在該事件的存取子中，以 `this` （[此存取權](expressions.md#this-access)）的方式存取這個實例。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1239">An instance event is associated with a given instance of a class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that event.</span></span>

<span data-ttu-id="2d25f-1240">在表單 `E.M`的*member_access* （[成員存取](expressions.md#member-access)）中參考事件時，如果 `M` 是靜態事件，`E` 必須代表包含 `M`的類型，而且如果 `M` 是實例事件，則 E 必須表示包含 `M`之類型的實例。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1240">When an event is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static event, `E` must denote a type containing `M`, and if `M` is an instance event, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="2d25f-1241">靜態和實例成員之間的差異會在[靜態和實例成員](classes.md#static-and-instance-members)中進一步討論。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1241">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a><span data-ttu-id="2d25f-1242">虛擬、密封、覆寫和抽象事件存取子</span><span class="sxs-lookup"><span data-stu-id="2d25f-1242">Virtual, sealed, override, and abstract event accessors</span></span>

<span data-ttu-id="2d25f-1243">`virtual` 事件宣告會指定該事件的存取子是虛擬的。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1243">A `virtual` event declaration specifies that the accessors of that event are virtual.</span></span> <span data-ttu-id="2d25f-1244">`virtual` 修飾詞適用于事件的兩個存取子。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1244">The `virtual` modifier applies to both accessors of an event.</span></span>

<span data-ttu-id="2d25f-1245">`abstract` 事件宣告會指定事件的存取子是虛擬的，但不會提供存取子的實際執行。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1245">An `abstract` event declaration specifies that the accessors of the event are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="2d25f-1246">相反地，非抽象衍生類別必須藉由覆寫事件，為存取子提供自己的執行。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1246">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the event.</span></span> <span data-ttu-id="2d25f-1247">因為抽象事件宣告不提供實際的實值，所以無法提供以大括弧分隔的*event_accessor_declarations*。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1247">Because an abstract event declaration provides no actual implementation, it cannot provide brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="2d25f-1248">同時包含 `abstract` 和 `override` 修飾詞的事件宣告，會指定事件為抽象，並覆寫基底事件。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1248">An event declaration that includes both the `abstract` and `override` modifiers specifies that the event is abstract and overrides a base event.</span></span> <span data-ttu-id="2d25f-1249">這類事件的存取子也是抽象的。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1249">The accessors of such an event are also abstract.</span></span>

<span data-ttu-id="2d25f-1250">抽象事件宣告只允許用於抽象類別（[抽象類別](classes.md#abstract-classes)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1250">Abstract event declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="2d25f-1251">繼承之虛擬事件的存取子可以在衍生類別中覆寫，方法是包含指定 `override` 修飾詞的事件宣告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1251">The accessors of an inherited virtual event can be overridden in a derived class by including an event declaration that specifies an `override` modifier.</span></span> <span data-ttu-id="2d25f-1252">這就是所謂的覆***寫事件***宣告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1252">This is known as an ***overriding event declaration***.</span></span> <span data-ttu-id="2d25f-1253">覆寫事件宣告不會宣告新的事件。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1253">An overriding event declaration does not declare a new event.</span></span> <span data-ttu-id="2d25f-1254">相反地，它只是特製化現有虛擬事件的存取子。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1254">Instead, it simply specializes the implementations of the accessors of an existing virtual event.</span></span>

<span data-ttu-id="2d25f-1255">覆寫事件宣告必須指定與覆寫事件完全相同的存取範圍修飾詞、類型和名稱。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1255">An overriding event declaration must specify the exact same accessibility modifiers, type, and name as the overridden event.</span></span>

<span data-ttu-id="2d25f-1256">覆寫事件宣告可能包含 `sealed` 的修飾詞。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1256">An overriding event declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="2d25f-1257">使用此修飾詞可防止衍生類別進一步覆寫事件。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1257">Use of this modifier prevents a derived class from further overriding the event.</span></span> <span data-ttu-id="2d25f-1258">密封事件的存取子也是密封的。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1258">The accessors of a sealed event are also sealed.</span></span>

<span data-ttu-id="2d25f-1259">覆寫事件宣告要包含 `new` 修飾詞，這是編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1259">It is a compile-time error for an overriding event declaration to include a `new` modifier.</span></span>

<span data-ttu-id="2d25f-1260">除了宣告和調用語法的差異之外，virtual、sealed、override 和 abstract 存取子的行為與虛擬、sealed、override 和 abstract 方法完全相同。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1260">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="2d25f-1261">具體而言，[虛擬方法](classes.md#virtual-methods)、覆[寫方法](classes.md#override-methods)、[密封方法](classes.md#sealed-methods)和[抽象方法](classes.md#abstract-methods)中所述的規則，會如同存取子是對應表單的方法一樣適用。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1261">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form.</span></span> <span data-ttu-id="2d25f-1262">每個存取子都會對應至一個方法，其中具有事件種類的單一值參數、`void` 傳回類型，以及與包含事件相同的修飾詞。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1262">Each accessor corresponds to a method with a single value parameter of the event type, a `void` return type, and the same modifiers as the containing event.</span></span>

## <a name="indexers"></a><span data-ttu-id="2d25f-1263">索引子</span><span class="sxs-lookup"><span data-stu-id="2d25f-1263">Indexers</span></span>

<span data-ttu-id="2d25f-1264">***索引子***是一種成員，可讓物件以與陣列相同的方式進行索引。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1264">An ***indexer*** is a member that enables an object to be indexed in the same way as an array.</span></span> <span data-ttu-id="2d25f-1265">使用*indexer_declaration*s 宣告索引子：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1265">Indexers are declared using *indexer_declaration*s:</span></span>

```antlr
indexer_declaration
    : attributes? indexer_modifier* indexer_declarator indexer_body
    ;

indexer_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | indexer_modifier_unsafe
    ;

indexer_declarator
    : type 'this' '[' formal_parameter_list ']'
    | type interface_type '.' 'this' '[' formal_parameter_list ']'
    ;

indexer_body
    : '{' accessor_declarations '}' 
    | '=>' expression ';'
    ;
```

<span data-ttu-id="2d25f-1266">*Indexer_declaration*可能包括一組*屬性*（attribute）和四個存取修飾[詞（](attributes.md)[存取](classes.md#access-modifiers)修飾詞）的有效組合、`new` （[新的修飾](classes.md#the-new-modifier)詞）、`virtual` （[虛擬方法](classes.md#virtual-methods)）、`override` （覆[寫方法](classes.md#override-methods)）、`sealed` （[密封方法](classes.md#sealed-methods)）、`abstract` （[抽象方法](classes.md#abstract-methods)），以及 `extern` （[外部方法](classes.md#external-methods)）修飾詞。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1266">An *indexer_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="2d25f-1267">索引子宣告受限於與方法宣告（[方法](classes.md#methods)）相同的規則，與修飾詞的有效組合有關，唯一的例外是在索引子宣告上不允許使用 static 修飾詞。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1267">Indexer declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers, with the one exception being that the static modifier is not permitted on an indexer declaration.</span></span>

<span data-ttu-id="2d25f-1268">除了在一種情況下，`virtual`、`override`和 `abstract` 的修飾詞互斥。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1268">The modifiers `virtual`, `override`, and `abstract` are mutually exclusive except in one case.</span></span> <span data-ttu-id="2d25f-1269">`abstract` 和 `override` 修飾詞可以一起使用，讓抽象的索引子可以覆寫虛擬一個。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1269">The `abstract` and `override` modifiers may be used together so that an abstract indexer can override a virtual one.</span></span>

<span data-ttu-id="2d25f-1270">索引子宣告的*類型*會指定宣告所引進之索引子的元素類型。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1270">The *type* of an indexer declaration specifies the element type of the indexer introduced by the declaration.</span></span> <span data-ttu-id="2d25f-1271">除非索引子是明確的介面成員執行，否則*類型*後面會接著關鍵字 `this`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1271">Unless the indexer is an explicit interface member implementation, the *type* is followed by the keyword `this`.</span></span> <span data-ttu-id="2d25f-1272">若為明確介面成員的執行，*類型*後面會接著*interface_type*、"`.`" 和關鍵字 `this`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1272">For an explicit interface member implementation, the *type* is followed by an *interface_type*, a "`.`", and the keyword `this`.</span></span> <span data-ttu-id="2d25f-1273">不同于其他成員，索引子沒有使用者定義的名稱。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1273">Unlike other members, indexers do not have user-defined names.</span></span>

<span data-ttu-id="2d25f-1274">*Formal_parameter_list*指定索引子的參數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1274">The *formal_parameter_list* specifies the parameters of the indexer.</span></span> <span data-ttu-id="2d25f-1275">索引子的型式參數清單會對應至方法（[方法參數](classes.md#method-parameters)）的類型，但至少必須指定一個參數，而且不允許 `ref` 和 `out` 參數修飾詞。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1275">The formal parameter list of an indexer corresponds to that of a method ([Method parameters](classes.md#method-parameters)), except that at least one parameter must be specified, and that the `ref` and `out` parameter modifiers are not permitted.</span></span>

<span data-ttu-id="2d25f-1276">索引子的*類型*和*formal_parameter_list*中所參考的每個類型，必須至少與索引子本身（[存取範圍條件約束](basic-concepts.md#accessibility-constraints)）相同。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1276">The *type* of an indexer and each of the types referenced in the *formal_parameter_list* must be at least as accessible as the indexer itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="2d25f-1277">*Indexer_body*可能是由***存取子主體***或***運算式主體***所組成。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1277">An *indexer_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="2d25f-1278">在存取子主體中， *accessor_declarations*（必須包含在 "`{`" 和 "`}`" 標記中），請宣告屬性的存取子（[存取](classes.md#accessors)子）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1278">In an accessor body, *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="2d25f-1279">存取子會指定與讀取和寫入屬性相關聯的可執行語句。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1279">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="2d25f-1280">運算式主體，其中包含 "`=>`" 後面接著運算式 `E` 而且分號與語句主體 `{ get { return E; } }`完全相等，因此只能用來指定僅限 getter 的索引子，其中 getter 的結果是由單一運算式所指定。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1280">An expression body consisting of "`=>`" followed by an expression `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only indexers where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="2d25f-1281">雖然存取索引子專案的語法與陣列元素的語法相同，但索引子元素不會分類為變數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1281">Even though the syntax for accessing an indexer element is the same as that for an array element, an indexer element is not classified as a variable.</span></span> <span data-ttu-id="2d25f-1282">因此，您無法將索引子專案當做 `ref` 或 `out` 引數傳遞。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1282">Thus, it is not possible to pass an indexer element as a `ref` or `out` argument.</span></span>

<span data-ttu-id="2d25f-1283">索引子的正式參數清單會定義索引子的簽章（簽章[和](basic-concepts.md#signatures-and-overloading)多載）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1283">The formal parameter list of an indexer defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the indexer.</span></span> <span data-ttu-id="2d25f-1284">具體而言，索引子的簽章是由其型式參數的數目和類型所組成。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1284">Specifically, the signature of an indexer consists of the number and types of its formal parameters.</span></span> <span data-ttu-id="2d25f-1285">元素類型和型式參數的名稱不是索引子簽章的一部分。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1285">The element type and names of the formal parameters are not part of an indexer's signature.</span></span>

<span data-ttu-id="2d25f-1286">索引子的簽章必須與相同類別中宣告之所有其他索引子的簽章不同。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1286">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>

<span data-ttu-id="2d25f-1287">索引子和屬性在概念上非常類似，但下列方式不同：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1287">Indexers and properties are very similar in concept, but differ in the following ways:</span></span>

*  <span data-ttu-id="2d25f-1288">屬性是由其名稱所識別，而索引子則是由其簽章來識別。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1288">A property is identified by its name, whereas an indexer is identified by its signature.</span></span>
*  <span data-ttu-id="2d25f-1289">屬性是透過*simple_name* （[簡單名稱](expressions.md#simple-names)）或*member_access* （[成員存取](expressions.md#member-access)）來存取，而索引子元素則是透過*element_access* （[索引子存取](expressions.md#indexer-access)）來存取。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1289">A property is accessed through a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)), whereas an indexer element is accessed through an *element_access* ([Indexer access](expressions.md#indexer-access)).</span></span>
*  <span data-ttu-id="2d25f-1290">屬性可以是 `static` 成員，而索引子一律是實例成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1290">A property can be a `static` member, whereas an indexer is always an instance member.</span></span>
*  <span data-ttu-id="2d25f-1291">屬性的 `get` 存取子會對應到沒有參數的方法，而索引子的 `get` 存取子則會對應至具有與索引子相同的型式參數清單的方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1291">A `get` accessor of a property corresponds to a method with no parameters, whereas a `get` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer.</span></span>
*  <span data-ttu-id="2d25f-1292">屬性的 `set` 存取子會對應至具有名為 `value`之單一參數的方法，而索引子的 `set` 存取子會對應至具有與索引子相同的型式參數清單的方法，再加上名為 `value`的其他參數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1292">A `set` accessor of a property corresponds to a method with a single parameter named `value`, whereas a `set` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer, plus an additional parameter named `value`.</span></span>
*  <span data-ttu-id="2d25f-1293">這是一個編譯時期錯誤，可讓索引子存取子宣告與索引子參數名稱相同的本機變數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1293">It is a compile-time error for an indexer accessor to declare a local variable with the same name as an indexer parameter.</span></span>
*  <span data-ttu-id="2d25f-1294">在覆寫屬性宣告中，繼承的屬性會使用 `base.P`語法來存取，其中 `P` 是屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1294">In an overriding property declaration, the inherited property is accessed using the syntax `base.P`, where `P` is the property name.</span></span> <span data-ttu-id="2d25f-1295">在覆寫索引子宣告中，會使用語法 `base[E]`來存取繼承的索引子，其中 `E` 是運算式的逗號分隔清單。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1295">In an overriding indexer declaration, the inherited indexer is accessed using the syntax `base[E]`, where `E` is a comma separated list of expressions.</span></span>
*  <span data-ttu-id="2d25f-1296">「自動執行的索引子」沒有概念。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1296">There is no concept of an "automatically implemented indexer".</span></span> <span data-ttu-id="2d25f-1297">具有不具有分號存取子的非抽象、非外部索引子是錯誤的。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1297">It is an error to have a non-abstract, non-external indexer with semicolon accessors.</span></span>

<span data-ttu-id="2d25f-1298">除了這些差異之外，在[存取](classes.md#accessors)子和自動實作為[屬性](classes.md#automatically-implemented-properties)中定義的所有規則都適用于索引子存取子以及屬性存取子。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1298">Aside from these differences, all rules defined in [Accessors](classes.md#accessors) and [Automatically implemented properties](classes.md#automatically-implemented-properties) apply to indexer accessors as well as to property accessors.</span></span>

<span data-ttu-id="2d25f-1299">當索引子宣告包含 `extern` 修飾詞時，索引子稱為***外部索引子***。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1299">When an indexer declaration includes an `extern` modifier, the indexer is said to be an ***external indexer***.</span></span> <span data-ttu-id="2d25f-1300">因為外部索引子宣告不提供實際的實作為，所以它的每個*accessor_declarations*都包含一個分號。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1300">Because an external indexer declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

<span data-ttu-id="2d25f-1301">下列範例會宣告一個 `BitArray` 類別，它會執行索引子來存取位陣列中的個別位。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1301">The example below declares a `BitArray` class that implements an indexer for accessing the individual bits in the bit array.</span></span>
```csharp
using System;

class BitArray
{
    int[] bits;
    int length;

    public BitArray(int length) {
        if (length < 0) throw new ArgumentException();
        bits = new int[((length - 1) >> 5) + 1];
        this.length = length;
    }

    public int Length {
        get { return length; }
    }

    public bool this[int index] {
        get {
            if (index < 0 || index >= length) {
                throw new IndexOutOfRangeException();
            }
            return (bits[index >> 5] & 1 << index) != 0;
        }
        set {
            if (index < 0 || index >= length) {
                throw new IndexOutOfRangeException();
            }
            if (value) {
                bits[index >> 5] |= 1 << index;
            }
            else {
                bits[index >> 5] &= ~(1 << index);
            }
        }
    }
}
```

<span data-ttu-id="2d25f-1302">`BitArray` 類別的實例所耗用的記憶體遠低於對應的 `bool[]` （因為前者的每一個值只會佔用一個位，而不是後者的一個位元組），但它允許與 `bool[]`相同的作業。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1302">An instance of the `BitArray` class consumes substantially less memory than a corresponding `bool[]` (since each value of the former occupies only one bit instead of the latter's one byte), but it permits the same operations as a `bool[]`.</span></span>

<span data-ttu-id="2d25f-1303">下列 `CountPrimes` 類別使用 `BitArray` 和傳統 "eratosthenes" 演算法來計算介於1和指定最大值之間的質數數目：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1303">The following `CountPrimes` class uses a `BitArray` and the classical "sieve" algorithm to compute the number of primes between 1 and a given maximum:</span></span>
```csharp
class CountPrimes
{
    static int Count(int max) {
        BitArray flags = new BitArray(max + 1);
        int count = 1;
        for (int i = 2; i <= max; i++) {
            if (!flags[i]) {
                for (int j = i * 2; j <= max; j += i) flags[j] = true;
                count++;
            }
        }
        return count;
    }

    static void Main(string[] args) {
        int max = int.Parse(args[0]);
        int count = Count(max);
        Console.WriteLine("Found {0} primes between 1 and {1}", count, max);
    }
}
```

<span data-ttu-id="2d25f-1304">請注意，用來存取 `BitArray` 元素的語法，與 `bool[]`的語法完全相同。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1304">Note that the syntax for accessing elements of the `BitArray` is precisely the same as for a `bool[]`.</span></span>

<span data-ttu-id="2d25f-1305">下列範例顯示 26 \* 10 格線類別，其中有一個具有兩個參數的索引子。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1305">The following example shows a 26 \* 10 grid class that has an indexer with two parameters.</span></span> <span data-ttu-id="2d25f-1306">第一個參數必須是範圍 a-z 中的大寫或小寫字母，而第二個則必須是範圍0-9 中的整數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1306">The first parameter is required to be an upper- or lowercase letter in the range A-Z, and the second is required to be an integer in the range 0-9.</span></span>

```csharp
using System;

class Grid
{
    const int NumRows = 26;
    const int NumCols = 10;

    int[,] cells = new int[NumRows, NumCols];

    public int this[char c, int col] {
        get {
            c = Char.ToUpper(c);
            if (c < 'A' || c > 'Z') {
                throw new ArgumentException();
            }
            if (col < 0 || col >= NumCols) {
                throw new IndexOutOfRangeException();
            }
            return cells[c - 'A', col];
        }

        set {
            c = Char.ToUpper(c);
            if (c < 'A' || c > 'Z') {
                throw new ArgumentException();
            }
            if (col < 0 || col >= NumCols) {
                throw new IndexOutOfRangeException();
            }
            cells[c - 'A', col] = value;
        }
    }
}
```

### <a name="indexer-overloading"></a><span data-ttu-id="2d25f-1307">索引子多載</span><span class="sxs-lookup"><span data-stu-id="2d25f-1307">Indexer overloading</span></span>

<span data-ttu-id="2d25f-1308">索引子多載解析規則會在[型別推斷](expressions.md#type-inference)中說明。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1308">The indexer overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="operators"></a><span data-ttu-id="2d25f-1309">運算子</span><span class="sxs-lookup"><span data-stu-id="2d25f-1309">Operators</span></span>

<span data-ttu-id="2d25f-1310">「***運算子***」（operator）是一個成員，其定義可以套用至類別實例之運算式運算子的意義。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1310">An ***operator*** is a member that defines the meaning of an expression operator that can be applied to instances of the class.</span></span> <span data-ttu-id="2d25f-1311">運算子是使用*operator_declaration*s 來宣告：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1311">Operators are declared using *operator_declaration*s:</span></span>

```antlr
operator_declaration
    : attributes? operator_modifier+ operator_declarator operator_body
    ;

operator_modifier
    : 'public'
    | 'static'
    | 'extern'
    | operator_modifier_unsafe
    ;

operator_declarator
    : unary_operator_declarator
    | binary_operator_declarator
    | conversion_operator_declarator
    ;

unary_operator_declarator
    : type 'operator' overloadable_unary_operator '(' type identifier ')'
    ;

overloadable_unary_operator
    : '+' | '-' | '!' | '~' | '++' | '--' | 'true' | 'false'
    ;

binary_operator_declarator
    : type 'operator' overloadable_binary_operator '(' type identifier ',' type identifier ')'
    ;

overloadable_binary_operator
    : '+'   | '-'   | '*'   | '/'   | '%'   | '&'   | '|'   | '^'   | '<<'
    | right_shift | '=='  | '!='  | '>'   | '<'   | '>='  | '<='
    ;

conversion_operator_declarator
    : 'implicit' 'operator' type '(' type identifier ')'
    | 'explicit' 'operator' type '(' type identifier ')'
    ;

operator_body
    : block
    | '=>' expression ';'
    | ';'
    ;
```

<span data-ttu-id="2d25f-1312">有三種類別的多載運算子：一元運算子（[一元運算子](classes.md#unary-operators)）、二元運算子（[二元運算子](classes.md#binary-operators)）和轉換運算子（[轉換運算子](classes.md#conversion-operators)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1312">There are three categories of overloadable operators: Unary operators ([Unary operators](classes.md#unary-operators)), binary operators ([Binary operators](classes.md#binary-operators)), and conversion operators ([Conversion operators](classes.md#conversion-operators)).</span></span>

<span data-ttu-id="2d25f-1313">*Operator_body*可以是分號、***語句主體***或***運算式主體***。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1313">The *operator_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="2d25f-1314">語句主體是由*區塊*所組成，它會指定叫用運算子時要執行的語句。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1314">A statement body consists of a *block*, which specifies the statements to execute when the operator is invoked.</span></span> <span data-ttu-id="2d25f-1315">*區塊*必須符合[方法主體](classes.md#method-body)中所述之傳回值方法的規則。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1315">The *block* must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="2d25f-1316">運算式主體包含 `=>` 後面接著運算式和分號，並代表叫用運算子時要執行的單一運算式。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1316">An expression body consists of `=>` followed by an expression and a semicolon, and denotes a single expression to perform when the operator is invoked.</span></span>

<span data-ttu-id="2d25f-1317">針對 `extern` 運算子， *operator_body*只包含一個分號。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1317">For `extern` operators, the *operator_body* consists simply of a semicolon.</span></span> <span data-ttu-id="2d25f-1318">對於所有其他運算子， *operator_body*是區塊主體或運算式主體。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1318">For all other operators, the *operator_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="2d25f-1319">下列規則適用于所有的運算子宣告：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1319">The following rules apply to all operator declarations:</span></span>

*  <span data-ttu-id="2d25f-1320">運算子宣告必須同時包含 `public` 和 `static` 修飾詞。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1320">An operator declaration must include both a `public` and a `static` modifier.</span></span>
*  <span data-ttu-id="2d25f-1321">運算子的參數必須是值參數（[值參數](variables.md#value-parameters)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1321">The parameter(s) of an operator must be value parameters ([Value parameters](variables.md#value-parameters)).</span></span> <span data-ttu-id="2d25f-1322">運算子宣告指定 `ref` 或 `out` 參數時，會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1322">It is a compile-time error for an operator declaration to specify `ref` or `out` parameters.</span></span>
*  <span data-ttu-id="2d25f-1323">運算子（[一元運算子](classes.md#unary-operators)、[二元運算子](classes.md#binary-operators)、[轉換運算子](classes.md#conversion-operators)）的簽章必須與相同類別中宣告之所有其他運算子的簽章不同。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1323">The signature of an operator ([Unary operators](classes.md#unary-operators), [Binary operators](classes.md#binary-operators), [Conversion operators](classes.md#conversion-operators)) must differ from the signatures of all other operators declared in the same class.</span></span>
*  <span data-ttu-id="2d25f-1324">在運算子宣告中參考的所有類型，都必須至少與運算子本身（存取範圍[條件約束](basic-concepts.md#accessibility-constraints)）一樣可以存取。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1324">All types referenced in an operator declaration must be at least as accessible as the operator itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>
*  <span data-ttu-id="2d25f-1325">在運算子宣告中多次出現相同的修飾詞時，會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1325">It is an error for the same modifier to appear multiple times in an operator declaration.</span></span>

<span data-ttu-id="2d25f-1326">每個運算子類別都會有額外的限制，如下列各節所述。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1326">Each operator category imposes additional restrictions, as described in the following sections.</span></span>

<span data-ttu-id="2d25f-1327">就像其他成員一樣，在基類中宣告的運算子會由衍生類別繼承。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1327">Like other members, operators declared in a base class are inherited by derived classes.</span></span> <span data-ttu-id="2d25f-1328">因為運算子宣告一律需要宣告運算子以參與運算子簽章的類別或結構，所以在衍生類別中宣告的運算子不可能隱藏在基類中宣告的運算子。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1328">Because operator declarations always require the class or struct in which the operator is declared to participate in the signature of the operator, it is not possible for an operator declared in a derived class to hide an operator declared in a base class.</span></span> <span data-ttu-id="2d25f-1329">因此，在運算子宣告中，絕對不需要 `new` 修飾詞，因此不允許使用。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1329">Thus, the `new` modifier is never required, and therefore never permitted, in an operator declaration.</span></span>

<span data-ttu-id="2d25f-1330">如需一元和二元運算子的其他資訊，請參閱[運算子](expressions.md#operators)。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1330">Additional information on unary and binary operators can be found in [Operators](expressions.md#operators).</span></span>

<span data-ttu-id="2d25f-1331">您可以在[使用者定義的轉換](conversions.md#user-defined-conversions)中找到轉換運算子的其他資訊。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1331">Additional information on conversion operators can be found in [User-defined conversions](conversions.md#user-defined-conversions).</span></span>

### <a name="unary-operators"></a><span data-ttu-id="2d25f-1332">一元運算子</span><span class="sxs-lookup"><span data-stu-id="2d25f-1332">Unary operators</span></span>

<span data-ttu-id="2d25f-1333">下列規則適用于一元運算子宣告，其中 `T` 代表包含運算子宣告之類別或結構的實例類型：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1333">The following rules apply to unary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="2d25f-1334">一元 `+`、`-`、`!`或 `~` 運算子必須接受類型 `T` 或 `T?` 的單一參數，而且可以傳回任何類型。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1334">A unary `+`, `-`, `!`, or `~` operator must take a single parameter of type `T` or `T?` and can return any type.</span></span>
*  <span data-ttu-id="2d25f-1335">一元 `++` 或 `--` 運算子必須接受類型 `T` 或 `T?` 的單一參數，而且必須傳回該相同類型或從中衍生的類型。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1335">A unary `++` or `--` operator must take a single parameter of type `T` or `T?` and must return that same type or a type derived from it.</span></span>
*  <span data-ttu-id="2d25f-1336">一元 `true` 或 `false` 運算子必須接受類型 `T` 或 `T?` 的單一參數，而且必須傳回類型 `bool`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1336">A unary `true` or `false` operator must take a single parameter of type `T` or `T?` and must return type `bool`.</span></span>

<span data-ttu-id="2d25f-1337">一元運算子的簽章是由運算子 token （`+`、`-`、`!`、`~`、`++`、`--`、`true`或 `false`）和單一型式參數的類型所組成。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1337">The signature of a unary operator consists of the operator token (`+`, `-`, `!`, `~`, `++`, `--`, `true`, or `false`) and the type of the single formal parameter.</span></span> <span data-ttu-id="2d25f-1338">傳回型別不是一元運算子的簽章的一部分，也不是型式參數的名稱。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1338">The return type is not part of a unary operator's signature, nor is the name of the formal parameter.</span></span>

<span data-ttu-id="2d25f-1339">`true` 和 `false` 一元運算子需要成對的宣告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1339">The `true` and `false` unary operators require pair-wise declaration.</span></span> <span data-ttu-id="2d25f-1340">如果類別宣告其中一個運算子，但未同時宣告另一個，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1340">A compile-time error occurs if a class declares one of these operators without also declaring the other.</span></span> <span data-ttu-id="2d25f-1341">`true` 和 `false` 運算子會在[使用者定義的條件式邏輯運算子](expressions.md#user-defined-conditional-logical-operators)和[布林運算式](expressions.md#boolean-expressions)中進一步說明。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1341">The `true` and `false` operators are described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators) and [Boolean expressions](expressions.md#boolean-expressions).</span></span>

<span data-ttu-id="2d25f-1342">下列範例顯示整數向量類別的 `operator ++` 的執行和後續使用方式：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1342">The following example shows an implementation and subsequent usage of `operator ++` for an integer vector class:</span></span>
```csharp
public class IntVector
{
    public IntVector(int length) {...}

    public int Length {...}                 // read-only property

    public int this[int index] {...}        // read-write indexer

    public static IntVector operator ++(IntVector iv) {
        IntVector temp = new IntVector(iv.Length);
        for (int i = 0; i < iv.Length; i++)
            temp[i] = iv[i] + 1;
        return temp;
    }
}

class Test
{
    static void Main() {
        IntVector iv1 = new IntVector(4);    // vector of 4 x 0
        IntVector iv2;

        iv2 = iv1++;    // iv2 contains 4 x 0, iv1 contains 4 x 1
        iv2 = ++iv1;    // iv2 contains 4 x 2, iv1 contains 4 x 2
    }
}
```

<span data-ttu-id="2d25f-1343">請注意，運算子方法如何傳回將1新增至運算元所產生的值，就像後置遞增和遞減運算子（後置[遞增和遞減運算子](expressions.md#postfix-increment-and-decrement-operators)），以及前置遞增和遞減運算子（[前置遞增](expressions.md#prefix-increment-and-decrement-operators)和遞減運算子）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1343">Note how the operator method returns the value produced by adding 1 to the operand, just like the  postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)), and the prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="2d25f-1344">不同于C++中的，此方法不需要直接修改其運算元的值。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1344">Unlike in C++, this method need not modify the value of its operand directly.</span></span> <span data-ttu-id="2d25f-1345">事實上，修改運算元值會違反後置遞增運算子的標準語義。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1345">In fact, modifying the operand value would violate the standard semantics of the postfix increment operator.</span></span>

### <a name="binary-operators"></a><span data-ttu-id="2d25f-1346">二元運算子</span><span class="sxs-lookup"><span data-stu-id="2d25f-1346">Binary operators</span></span>

<span data-ttu-id="2d25f-1347">下列規則適用于二元運算子宣告，其中 `T` 代表包含運算子宣告之類別或結構的實例類型：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1347">The following rules apply to binary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="2d25f-1348">二元非移位運算子必須接受兩個參數，其中至少一個必須具有類型 `T` 或 `T?`，而且可以傳回任何類型。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1348">A binary non-shift operator must take two parameters, at least one of which must have type `T` or `T?`, and can return any type.</span></span>
*  <span data-ttu-id="2d25f-1349">二元 `<<` 或 `>>` 運算子必須接受兩個參數，其中第一個必須有類型 `T` 或 `T?`，而第二個必須具有類型 `int` 或 `int?`，而且可以傳回任何類型。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1349">A binary `<<` or `>>` operator must take two parameters, the first of which must have type `T` or `T?` and the second of which must have type `int` or `int?`, and can return any type.</span></span>

<span data-ttu-id="2d25f-1350">二元運算子的簽章包含運算子 token （`+`、`-`、`*`、`/`、`%`、`&`、`|`、`^`、`<<`、`>>`、`==`、`!=`、`>`、`<`、`>=`或 `<=`）和兩個正式參數的類型。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1350">The signature of a binary operator consists of the operator token (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`, or `<=`) and the types of the two formal parameters.</span></span> <span data-ttu-id="2d25f-1351">傳回類型和型式參數的名稱不是二元運算子簽章的一部分。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1351">The return type and the names of the formal parameters are not part of a binary operator's signature.</span></span>

<span data-ttu-id="2d25f-1352">某些二元運算子需要成對的宣告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1352">Certain binary operators require pair-wise declaration.</span></span> <span data-ttu-id="2d25f-1353">對於成對之任一個運算子的每個宣告，必須有配對的另一個運算子的相符宣告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1353">For every declaration of either operator of a pair, there must be a matching declaration of the other operator of the pair.</span></span> <span data-ttu-id="2d25f-1354">當兩個運算子宣告具有相同的傳回類型，且每個參數都有相同的類型時，就會進行比對。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1354">Two operator declarations match when they have the same return type and the same type for each parameter.</span></span> <span data-ttu-id="2d25f-1355">下列運算子需要成對的宣告：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1355">The following operators require pair-wise declaration:</span></span>

*  <span data-ttu-id="2d25f-1356">`operator ==` 和 `operator !=`</span><span class="sxs-lookup"><span data-stu-id="2d25f-1356">`operator ==` and `operator !=`</span></span>
*  <span data-ttu-id="2d25f-1357">`operator >` 和 `operator <`</span><span class="sxs-lookup"><span data-stu-id="2d25f-1357">`operator >` and `operator <`</span></span>
*  <span data-ttu-id="2d25f-1358">`operator >=` 和 `operator <=`</span><span class="sxs-lookup"><span data-stu-id="2d25f-1358">`operator >=` and `operator <=`</span></span>

### <a name="conversion-operators"></a><span data-ttu-id="2d25f-1359">轉換運算子</span><span class="sxs-lookup"><span data-stu-id="2d25f-1359">Conversion operators</span></span>

<span data-ttu-id="2d25f-1360">轉換運算子宣告導入了***使用者***定義的轉換（[使用者定義](conversions.md#user-defined-conversions)的轉換），這會擴大預先定義的隱含和明確轉換。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1360">A conversion operator declaration introduces a ***user-defined conversion*** ([User-defined conversions](conversions.md#user-defined-conversions)) which augments the pre-defined implicit and explicit conversions.</span></span>

<span data-ttu-id="2d25f-1361">包含 `implicit` 關鍵字的轉換運算子宣告會引進使用者定義的隱含轉換。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1361">A conversion operator declaration that includes the `implicit` keyword introduces a user-defined implicit conversion.</span></span> <span data-ttu-id="2d25f-1362">隱含轉換可能會在各種情況下發生，包括函數成員調用、轉換運算式和指派。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1362">Implicit conversions can occur in a variety of situations, including function member invocations, cast expressions, and assignments.</span></span> <span data-ttu-id="2d25f-1363">這會在[隱含轉換](conversions.md#implicit-conversions)中進一步說明。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1363">This is described further in [Implicit conversions](conversions.md#implicit-conversions).</span></span>

<span data-ttu-id="2d25f-1364">包含 `explicit` 關鍵字的轉換運算子宣告會引進使用者定義的明確轉換。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1364">A conversion operator declaration that includes the `explicit` keyword introduces a user-defined explicit conversion.</span></span> <span data-ttu-id="2d25f-1365">明確轉換可以在 cast 運算式中進行，並在[明確轉換](conversions.md#explicit-conversions)中進一步說明。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1365">Explicit conversions can occur in cast expressions, and are described further in [Explicit conversions](conversions.md#explicit-conversions).</span></span>

<span data-ttu-id="2d25f-1366">轉換運算子會從轉換運算子的參數類型所表示的來源類型，轉換成目標型別（由轉換運算子的傳回類型所表示）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1366">A conversion operator converts from a source type, indicated by the parameter type of the conversion operator, to a target type, indicated by the return type of the conversion operator.</span></span>

<span data-ttu-id="2d25f-1367">對於給定的來源類型 `S` 和目標型別 `T`，如果 `S` 或 `T` 是可為 null 的類型，請讓 `S0` 和 `T0` 參考其基礎類型，否則 `S0` 和 `T0` 會分別等於 `S` 和 `T`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1367">For a given source type `S` and target type `T`, if `S` or `T` are nullable types, let `S0` and `T0` refer to their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span> <span data-ttu-id="2d25f-1368">只有在下列所有條件都成立時，才能使用類別或結構來宣告從來源類型 `S` 至目標型別的轉換 `T`：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1368">A class or struct is permitted to declare a conversion from a source type `S` to a target type `T` only if all of the following are true:</span></span>

*  <span data-ttu-id="2d25f-1369">`S0` 和 `T0` 是不同的類型。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1369">`S0` and `T0` are different types.</span></span>
*  <span data-ttu-id="2d25f-1370">`S0` 或 `T0` 是發生運算子宣告的類別或結構類型。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1370">Either `S0` or `T0` is the class or struct type in which the operator declaration takes place.</span></span>
*  <span data-ttu-id="2d25f-1371">`S0` 或 `T0` 都不是*interface_type*。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1371">Neither `S0` nor `T0` is an *interface_type*.</span></span>
*  <span data-ttu-id="2d25f-1372">除了使用者定義的轉換之外，從 `S` 到 `T` 或從 `T` 到 `S`的轉換不存在。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1372">Excluding user-defined conversions, a conversion does not exist from `S` to `T` or from `T` to `S`.</span></span>

<span data-ttu-id="2d25f-1373">基於這些規則的目的，與 `S` 或 `T` 相關聯的任何類型參數都會被視為與其他類型沒有繼承關係的唯一類型，而且會忽略這些類型參數上的任何條件約束。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1373">For the purposes of these rules, any type parameters associated with `S` or `T` are considered to be unique types that have no inheritance relationship with other types, and any constraints on those type parameters are ignored.</span></span>

<span data-ttu-id="2d25f-1374">在範例中</span><span class="sxs-lookup"><span data-stu-id="2d25f-1374">In the example</span></span>
```csharp
class C<T> {...}

class D<T>: C<T>
{
    public static implicit operator C<int>(D<T> value) {...}      // Ok
    public static implicit operator C<string>(D<T> value) {...}   // Ok
    public static implicit operator C<T>(D<T> value) {...}        // Error
}
```
<span data-ttu-id="2d25f-1375">前兩個運算子宣告是允許的，因為[索引子](classes.md#indexers).3 的目的，分別 `T` 和 `int` 和 `string` 視為唯一沒有關聯性的類型。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1375">the first two operator declarations are permitted because, for the purposes of [Indexers](classes.md#indexers).3, `T` and `int` and `string` respectively are considered unique types with no relationship.</span></span> <span data-ttu-id="2d25f-1376">不過，第三個運算子是錯誤，因為 `C<T>` 是 `D<T>`的基類。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1376">However, the third operator is an error because `C<T>` is the base class of `D<T>`.</span></span>

<span data-ttu-id="2d25f-1377">從第二個規則來看，轉換運算子必須在宣告運算子的類別或結構類型中轉換成或。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1377">From the second rule it follows that a conversion operator must convert either to or from the class or struct type in which the operator is declared.</span></span> <span data-ttu-id="2d25f-1378">例如，類別或結構類型 `C` 定義從 `C` 到 `int` 的轉換，以及從 `int` 到 `C`，但不能從 `int` 到 `bool`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1378">For example, it is possible for a class or struct type `C` to define a conversion from `C` to `int` and from `int` to `C`, but not from `int` to `bool`.</span></span>

<span data-ttu-id="2d25f-1379">不可能直接重新定義預先定義的轉換。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1379">It is not possible to directly redefine a pre-defined conversion.</span></span> <span data-ttu-id="2d25f-1380">因此，轉換運算子不允許從或轉換成 `object`，因為 `object` 與所有其他類型之間已經存在隱含和明確轉換。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1380">Thus, conversion operators are not allowed to convert from or to `object` because implicit and explicit conversions already exist between `object` and all other types.</span></span> <span data-ttu-id="2d25f-1381">同樣地，來源或轉換的目標型別都不能是另一個的基底類型，因為轉換之後就已經存在。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1381">Likewise, neither the source nor the target types of a conversion can be a base type of the other, since a conversion would then already exist.</span></span>

<span data-ttu-id="2d25f-1382">不過，您可以在泛型型別上宣告運算子，針對特定類型引數，指定已經存在做為預先定義轉換的轉換。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1382">However, it is possible to declare operators on generic types that, for particular type arguments, specify conversions that already exist as pre-defined conversions.</span></span> <span data-ttu-id="2d25f-1383">在範例中</span><span class="sxs-lookup"><span data-stu-id="2d25f-1383">In the example</span></span>
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
<span data-ttu-id="2d25f-1384">當型別 `object` 指定為 `T`的型別引數時，第二個運算子會宣告已經存在的轉換（隱含的，因此也是明確的，從任何類型到 `object`類型的轉換都有）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1384">when type `object` is specified as a type argument for `T`, the second operator declares a conversion that already exists (an implicit, and therefore also an explicit, conversion exists from any type to type `object`).</span></span>

<span data-ttu-id="2d25f-1385">如果兩個類型之間存在預先定義的轉換，則會忽略這些類型之間的任何使用者定義轉換。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1385">In cases where a pre-defined conversion exists between two types, any user-defined conversions between those types are ignored.</span></span> <span data-ttu-id="2d25f-1386">尤其是：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1386">Specifically:</span></span>

*  <span data-ttu-id="2d25f-1387">如果從類型 `S` 的預先定義隱含轉換（[隱含轉換](conversions.md#implicit-conversions)）存在於類型 `T`，則會忽略從 `S` 到 `T` 的所有使用者定義轉換（隱含或明確）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1387">If a pre-defined implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from type `S` to type `T`, all user-defined conversions (implicit or explicit) from `S` to `T` are ignored.</span></span>
*  <span data-ttu-id="2d25f-1388">如果從類型 `S` 的預先定義明確轉換（[明確轉換](conversions.md#explicit-conversions)）存在於類型 `T`，則會忽略從 `S` 到 `T` 的任何使用者定義明確轉換。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1388">If a pre-defined explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) exists from type `S` to type `T`, any user-defined explicit conversions from `S` to `T` are ignored.</span></span> <span data-ttu-id="2d25f-1389">此外，</span><span class="sxs-lookup"><span data-stu-id="2d25f-1389">Furthermore:</span></span>

<span data-ttu-id="2d25f-1390">如果 `T` 是介面類別型，則會忽略從 `S` 到 `T` 的使用者定義隱含轉換。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1390">If `T` is an interface type, user-defined implicit conversions from `S` to `T` are ignored.</span></span>

<span data-ttu-id="2d25f-1391">否則，使用者定義的從 `S` 到 `T` 的隱含轉換仍會被視為。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1391">Otherwise, user-defined implicit conversions from `S` to `T` are still considered.</span></span>

<span data-ttu-id="2d25f-1392">針對所有類型但 `object`，上述 `Convertible<T>` 類型所宣告的運算子不會與預先定義的轉換衝突。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1392">For all types but `object`, the operators declared by the `Convertible<T>` type above do not conflict with pre-defined conversions.</span></span> <span data-ttu-id="2d25f-1393">例如：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1393">For example:</span></span>
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

<span data-ttu-id="2d25f-1394">不過，針對型別 `object`，預先定義的轉換會在所有情況下隱藏使用者定義的轉換，但有一個：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1394">However, for type `object`, pre-defined conversions hide the user-defined conversions in all cases but one:</span></span>

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

<span data-ttu-id="2d25f-1395">使用者定義的轉換不允許從或轉換成*interface_type*s。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1395">User-defined conversions are not allowed to convert from or to *interface_type*s.</span></span> <span data-ttu-id="2d25f-1396">特別是，這項限制可確保轉換成*interface_type*時，不會發生任何使用者定義的轉換，而且只有在轉換的物件實際實作為指定的*interface_type*時，才會成功轉換成*interface_type* 。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1396">In particular, this restriction ensures that no user-defined transformations occur when converting to an *interface_type*, and that a conversion to an *interface_type* succeeds only if the object being converted actually implements the specified *interface_type*.</span></span>

<span data-ttu-id="2d25f-1397">轉換運算子的簽章是由來源類型和目標型別所組成。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1397">The signature of a conversion operator consists of the source type and the target type.</span></span> <span data-ttu-id="2d25f-1398">（請注意，這是傳回型別參與簽章的唯一成員形式）。轉換運算子的 `implicit` 或 `explicit` 分類不是操作員簽章的一部分。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1398">(Note that this is the only form of member for which the return type participates in the signature.) The `implicit` or `explicit` classification of a conversion operator is not part of the operator's signature.</span></span> <span data-ttu-id="2d25f-1399">因此，類別或結構無法同時宣告具有相同來源和目標型別的 `implicit` 和 `explicit` 轉換運算子。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1399">Thus, a class or struct cannot declare both an `implicit` and an `explicit` conversion operator with the same source and target types.</span></span>

<span data-ttu-id="2d25f-1400">一般而言，使用者定義的隱含轉換應該設計成永遠不會擲回例外狀況，而且永遠不會遺失資訊。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1400">In general, user-defined implicit conversions should be designed to never throw exceptions and never lose information.</span></span> <span data-ttu-id="2d25f-1401">如果使用者定義的轉換可能會導致例外狀況（例如，因為來源引數超出範圍）或遺失資訊（例如捨棄高序位位），則應該將該轉換定義為明確轉換。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1401">If a user-defined conversion can give rise to exceptions (for example, because the source argument is out of range) or loss of information (such as discarding high-order bits), then that conversion should be defined as an explicit conversion.</span></span>

<span data-ttu-id="2d25f-1402">在範例中</span><span class="sxs-lookup"><span data-stu-id="2d25f-1402">In the example</span></span>
```csharp
using System;

public struct Digit
{
    byte value;

    public Digit(byte value) {
        if (value < 0 || value > 9) throw new ArgumentException();
        this.value = value;
    }

    public static implicit operator byte(Digit d) {
        return d.value;
    }

    public static explicit operator Digit(byte b) {
        return new Digit(b);
    }
}
```
<span data-ttu-id="2d25f-1403">從 `Digit` 到 `byte` 的轉換是隱含的，因為它永遠不會擲回例外狀況或遺失資訊，但從 `byte` 到 `Digit` 的轉換是明確的，因為 `Digit` 只能代表 `byte`的可能值子集。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1403">the conversion from `Digit` to `byte` is implicit because it never throws exceptions or loses information, but the conversion from `byte` to `Digit` is explicit since `Digit` can only represent a subset of the possible values of a `byte`.</span></span>

## <a name="instance-constructors"></a><span data-ttu-id="2d25f-1404">實例構造函式</span><span class="sxs-lookup"><span data-stu-id="2d25f-1404">Instance constructors</span></span>

<span data-ttu-id="2d25f-1405">「執行個體建構函式」是實作將類別執行個體初始化所需之動作的成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1405">An ***instance constructor*** is a member that implements the actions required to initialize an instance of a class.</span></span> <span data-ttu-id="2d25f-1406">實例的函式是使用*constructor_declaration*s 來宣告：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1406">Instance constructors are declared using *constructor_declaration*s:</span></span>

```antlr
constructor_declaration
    : attributes? constructor_modifier* constructor_declarator constructor_body
    ;

constructor_modifier
    : 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'extern'
    | constructor_modifier_unsafe
    ;

constructor_declarator
    : identifier '(' formal_parameter_list? ')' constructor_initializer?
    ;

constructor_initializer
    : ':' 'base' '(' argument_list? ')'
    | ':' 'this' '(' argument_list? ')'
    ;

constructor_body
    : block
    | ';'
    ;
```

<span data-ttu-id="2d25f-1407">*Constructor_declaration*可能包括一組*屬性*（attribute）、四個存取修飾[詞（](attributes.md)[存取](classes.md#access-modifiers)修飾詞）的有效組合，以及 `extern` （[外部方法](classes.md#external-methods)）修飾詞。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1407">A *constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and an `extern` ([External methods](classes.md#external-methods)) modifier.</span></span> <span data-ttu-id="2d25f-1408">不允許將相同的修飾詞多次包含在函式宣告中。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1408">A constructor declaration is not permitted to include the same modifier multiple times.</span></span>

<span data-ttu-id="2d25f-1409">*Constructor_declarator*的*識別碼*必須為宣告實例的函式所在的類別命名。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1409">The *identifier* of a *constructor_declarator* must name the class in which the instance constructor is declared.</span></span> <span data-ttu-id="2d25f-1410">如果指定了任何其他名稱，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1410">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="2d25f-1411">實例參數的選擇性*formal_parameter_list*受限於方法（[方法](classes.md#methods)） *formal_parameter_list*的相同規則。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1411">The optional *formal_parameter_list* of an instance constructor is subject to the same rules as the *formal_parameter_list* of a method ([Methods](classes.md#methods)).</span></span> <span data-ttu-id="2d25f-1412">正式參數清單會定義實例的簽章（簽章[和](basic-concepts.md#signatures-and-overloading)多載），並控制多載解析（[類型推斷](expressions.md#type-inference)）在調用中選取特定實例的程式。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1412">The formal parameter list defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of an instance constructor and governs the process whereby overload resolution ([Type inference](expressions.md#type-inference)) selects a particular instance constructor in an invocation.</span></span>

<span data-ttu-id="2d25f-1413">實例函式的*formal_parameter_list*中所參考的每個型別，都必須至少與此「函數」（[存取範圍條件約束](basic-concepts.md#accessibility-constraints)）一樣可以存取。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1413">Each of the types referenced in the *formal_parameter_list* of an instance constructor must be at least as accessible as the constructor itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="2d25f-1414">選擇性的*constructor_initializer*會指定另一個要叫用的實例函式，然後再執行這個實例的*constructor_body*中提供的語句。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1414">The optional *constructor_initializer* specifies another instance constructor to invoke before executing the statements given in the *constructor_body* of this instance constructor.</span></span> <span data-ttu-id="2d25f-1415">這會在[函數初始化運算式](classes.md#constructor-initializers)中進一步說明。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1415">This is described further in [Constructor initializers](classes.md#constructor-initializers).</span></span>

<span data-ttu-id="2d25f-1416">當函式宣告包含 `extern` 修飾詞時，會將此函式稱為***外部***的「函數」（external）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1416">When a constructor declaration includes an `extern` modifier, the constructor is said to be an ***external constructor***.</span></span> <span data-ttu-id="2d25f-1417">因為外部的函式宣告不提供實際的實作為，所以其*constructor_body*由分號組成。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1417">Because an external constructor declaration provides no actual implementation, its *constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="2d25f-1418">對於所有其他的函式， *constructor_body*包含指定語句的*區塊*，以初始化類別的新實例。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1418">For all other constructors, the *constructor_body* consists of a *block* which specifies the statements to initialize a new instance of the class.</span></span> <span data-ttu-id="2d25f-1419">這會完全對應至具有 `void` 傳回型別（[方法主體](classes.md#method-body)）之實例方法的*區塊*。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1419">This corresponds exactly to the *block* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="2d25f-1420">不會繼承實例的構造函式。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1420">Instance constructors are not inherited.</span></span> <span data-ttu-id="2d25f-1421">因此，除了在類別中實際宣告的類別以外，沒有任何實例的函式。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1421">Thus, a class has no instance constructors other than those actually declared in the class.</span></span> <span data-ttu-id="2d25f-1422">如果類別未包含任何實例的函式宣告，則會自動提供預設的實例（[預設](classes.md#default-constructors)為函式）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1422">If a class contains no instance constructor declarations, a default instance constructor is automatically provided ([Default constructors](classes.md#default-constructors)).</span></span>

<span data-ttu-id="2d25f-1423">實例構造函式是由*object_creation_expression*s （[物件建立運算式](expressions.md#object-creation-expressions)）和*constructor_initializer*s 叫用。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1423">Instance constructors are invoked by *object_creation_expression*s ([Object creation expressions](expressions.md#object-creation-expressions)) and through *constructor_initializer*s.</span></span>

### <a name="constructor-initializers"></a><span data-ttu-id="2d25f-1424">建構函式初始設定式</span><span class="sxs-lookup"><span data-stu-id="2d25f-1424">Constructor initializers</span></span>

<span data-ttu-id="2d25f-1425">所有實例的「函式」（class `object`的對應項除外）會隱含地在*constructor_body*之前，直接包含另一個實例的程式調用。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1425">All instance constructors (except those for class `object`) implicitly include an invocation of another instance constructor immediately before the *constructor_body*.</span></span> <span data-ttu-id="2d25f-1426">隱含叫用的構造函式取決於*constructor_initializer*：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1426">The constructor to implicitly invoke is determined by the *constructor_initializer*:</span></span>

*  <span data-ttu-id="2d25f-1427">`base(argument_list)` 或 `base()` 形式的實例的「版本設定」初始化運算式，將會叫用直接基類的實例函式。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1427">An instance constructor initializer of the form `base(argument_list)` or `base()` causes an instance constructor from the direct base class to be invoked.</span></span> <span data-ttu-id="2d25f-1428">如果存在，則會使用*argument_list*選取該函式，以及多載[解析](expressions.md#overload-resolution)的多載解析規則。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1428">That constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="2d25f-1429">候選實例的集合是由直接基類中包含的所有可存取的實例函式所組成，如果在直接基類中未宣告任何實例的函式，則是預設的函式（[預設](classes.md#default-constructors)的函數）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1429">The set of candidate instance constructors consists of all accessible instance constructors contained in the direct base class, or the default constructor ([Default constructors](classes.md#default-constructors)), if no instance constructors are declared in the direct base class.</span></span> <span data-ttu-id="2d25f-1430">如果這個集合是空的，或者無法識別單一最佳實例的函式，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1430">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span>
*  <span data-ttu-id="2d25f-1431">`this(argument-list)` 或 `this()` 形式的實例的「版本設定」初始化運算式，將會叫用類別本身的實例函式。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1431">An instance constructor initializer of the form `this(argument-list)` or `this()` causes an instance constructor from the class itself to be invoked.</span></span> <span data-ttu-id="2d25f-1432">如果存在，則會使用*argument_list*來選取此函式，以及多載[解析](expressions.md#overload-resolution)的多載解析規則。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1432">The constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="2d25f-1433">候選實例的集合是由類別本身所宣告的所有可存取的實例函式所組成。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1433">The set of candidate instance constructors consists of all accessible instance constructors declared in the class itself.</span></span> <span data-ttu-id="2d25f-1434">如果這個集合是空的，或者無法識別單一最佳實例的函式，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1434">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span> <span data-ttu-id="2d25f-1435">如果實例的「函式宣告」包含會叫用此函式本身的「函數初始化運算式」，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1435">If an instance constructor declaration includes a constructor initializer that invokes the constructor itself, a compile-time error occurs.</span></span>

<span data-ttu-id="2d25f-1436">如果實例的「建立器」沒有任何「函式」初始化運算式，則會以隱含方式提供 `base()` 形式的「程式」（</span><span class="sxs-lookup"><span data-stu-id="2d25f-1436">If an instance constructor has no constructor initializer, a constructor initializer of the form `base()` is implicitly provided.</span></span> <span data-ttu-id="2d25f-1437">因此，表單的具現化函數宣告</span><span class="sxs-lookup"><span data-stu-id="2d25f-1437">Thus, an instance constructor declaration of the form</span></span>
```csharp
C(...) {...}
```
<span data-ttu-id="2d25f-1438">完全等同于</span><span class="sxs-lookup"><span data-stu-id="2d25f-1438">is exactly equivalent to</span></span>
```csharp
C(...): base() {...}
```

<span data-ttu-id="2d25f-1439">實例的「參數」（instance）宣告之*formal_parameter_list*所指定的參數範圍包含該宣告的「函式初始化運算式」。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1439">The scope of the parameters given by the *formal_parameter_list* of an instance constructor declaration includes the constructor initializer of that declaration.</span></span> <span data-ttu-id="2d25f-1440">因此，允許使用函式初始化運算式來存取此函式的參數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1440">Thus, a constructor initializer is permitted to access the parameters of the constructor.</span></span> <span data-ttu-id="2d25f-1441">例如：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1441">For example:</span></span>
```csharp
class A
{
    public A(int x, int y) {}
}

class B: A
{
    public B(int x, int y): base(x + y, x - y) {}
}
```

<span data-ttu-id="2d25f-1442">實例的「函式初始化運算式」無法存取所建立的實例。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1442">An instance constructor initializer cannot access the instance being created.</span></span> <span data-ttu-id="2d25f-1443">因此，在函式初始化運算式的引數運算式中參考 `this` 時，會發生編譯時期錯誤，這是引數運算式透過*simple_name*參考任何實例成員的編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1443">Therefore it is a compile-time error to reference `this` in an argument expression of the constructor initializer, as is it a compile-time error for an argument expression to reference any instance member through a *simple_name*.</span></span>

### <a name="instance-variable-initializers"></a><span data-ttu-id="2d25f-1444">執行個體變數初始化運算式</span><span class="sxs-lookup"><span data-stu-id="2d25f-1444">Instance variable initializers</span></span>

<span data-ttu-id="2d25f-1445">當實例的函式沒有任何函式初始化運算式，或它的格式為 `base(...)`的函式初始化運算式時，該函數會以隱含方式執行其類別中所宣告之實例欄位的*variable_initializer*所指定的初始化。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1445">When an instance constructor has no constructor initializer, or it has a constructor initializer of the form `base(...)`, that constructor implicitly performs the initializations specified by the *variable_initializer*s of the instance fields declared in its class.</span></span> <span data-ttu-id="2d25f-1446">這會對應到在進入函式時，以及在直接基類的隱含調用之前，立即執行的一系列指派。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1446">This corresponds to a sequence of assignments that are executed immediately upon entry to the constructor and before the implicit invocation of the direct base class constructor.</span></span> <span data-ttu-id="2d25f-1447">變數初始化運算式會以它們出現在類別宣告中的文字順序來執行。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1447">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span>

### <a name="constructor-execution"></a><span data-ttu-id="2d25f-1448">執行的函式</span><span class="sxs-lookup"><span data-stu-id="2d25f-1448">Constructor execution</span></span>

<span data-ttu-id="2d25f-1449">變數初始化運算式會轉換成指派語句，而這些指派語句會在叫用基類實例的函式之前執行。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1449">Variable initializers are transformed into assignment statements, and these assignment statements are executed before the invocation of the base class instance constructor.</span></span> <span data-ttu-id="2d25f-1450">這種排序可確保所有實例欄位都是由其變數初始化運算式來初始化，然後才會執行可存取該實例的任何語句。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1450">This ordering ensures that all instance fields are initialized by their variable initializers before any statements that have access to that instance are executed.</span></span>

<span data-ttu-id="2d25f-1451">假設範例</span><span class="sxs-lookup"><span data-stu-id="2d25f-1451">Given the example</span></span>
```csharp
using System;

class A
{
    public A() {
        PrintFields();
    }

    public virtual void PrintFields() {}
}

class B: A
{
    int x = 1;
    int y;

    public B() {
        y = -1;
    }

    public override void PrintFields() {
        Console.WriteLine("x = {0}, y = {1}", x, y);
    }
}
```
<span data-ttu-id="2d25f-1452">當 `new B()` 用來建立 `B`的實例時，會產生下列輸出：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1452">when `new B()` is used to create an instance of `B`, the following output is produced:</span></span>
```console
x = 1, y = 0
```

<span data-ttu-id="2d25f-1453">`x` 的值為1，因為在叫用基類實例的函式之前，會先執行變數初始化運算式。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1453">The value of `x` is 1 because the variable initializer is executed before the base class instance constructor is invoked.</span></span> <span data-ttu-id="2d25f-1454">不過，`y` 的值為0（`int`的預設值），因為在基底類別的函式傳回之前，不會執行指派給 `y`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1454">However, the value of `y` is 0 (the default value of an `int`) because the assignment to `y` is not executed until after the base class constructor returns.</span></span>

<span data-ttu-id="2d25f-1455">將執行個體變數初始化運算式和函式初始化運算式視為在*constructor_body*之前自動插入的語句，會很有説明。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1455">It is useful to think of instance variable initializers and constructor initializers as statements that are automatically inserted before the *constructor_body*.</span></span> <span data-ttu-id="2d25f-1456">範例</span><span class="sxs-lookup"><span data-stu-id="2d25f-1456">The example</span></span>
```csharp
using System;
using System.Collections;

class A
{
    int x = 1, y = -1, count;

    public A() {
        count = 0;
    }

    public A(int n) {
        count = n;
    }
}

class B: A
{
    double sqrt2 = Math.Sqrt(2.0);
    ArrayList items = new ArrayList(100);
    int max;

    public B(): this(100) {
        items.Add("default");
    }

    public B(int n): base(n - 1) {
        max = n;
    }
}
```
<span data-ttu-id="2d25f-1457">包含數個變數初始化運算式;它也包含兩種形式的函式初始化運算式（`base` 和 `this`）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1457">contains several variable initializers; it also contains constructor initializers of both forms (`base` and `this`).</span></span> <span data-ttu-id="2d25f-1458">此範例會對應至如下所示的程式碼，其中每個批註都會指出自動插入的語句（用於自動插入的函式調用的語法無效，但只是用來說明機制）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1458">The example corresponds to the code shown below, where each comment indicates an automatically inserted statement (the syntax used for the automatically inserted constructor invocations isn't valid, but merely serves to illustrate the mechanism).</span></span>

```csharp
using System.Collections;

class A
{
    int x, y, count;

    public A() {
        x = 1;                       // Variable initializer
        y = -1;                      // Variable initializer
        object();                    // Invoke object() constructor
        count = 0;
    }

    public A(int n) {
        x = 1;                       // Variable initializer
        y = -1;                      // Variable initializer
        object();                    // Invoke object() constructor
        count = n;
    }
}

class B: A
{
    double sqrt2;
    ArrayList items;
    int max;

    public B(): this(100) {
        B(100);                      // Invoke B(int) constructor
        items.Add("default");
    }

    public B(int n): base(n - 1) {
        sqrt2 = Math.Sqrt(2.0);      // Variable initializer
        items = new ArrayList(100);  // Variable initializer
        A(n - 1);                    // Invoke A(int) constructor
        max = n;
    }
}
```

### <a name="default-constructors"></a><span data-ttu-id="2d25f-1459">預設建構函式</span><span class="sxs-lookup"><span data-stu-id="2d25f-1459">Default constructors</span></span>

<span data-ttu-id="2d25f-1460">如果類別未包含任何實例的「函式」宣告，則會自動提供預設實例的「函式」。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1460">If a class contains no instance constructor declarations, a default instance constructor is automatically provided.</span></span> <span data-ttu-id="2d25f-1461">該預設的構造函式只會叫用直接基類的無參數的函式。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1461">That default constructor simply invokes the parameterless constructor of the direct base class.</span></span> <span data-ttu-id="2d25f-1462">如果類別是抽象的，則預設的函式的宣告存取範圍會受到保護。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1462">If the class is abstract then the declared accessibility for the default constructor is protected.</span></span> <span data-ttu-id="2d25f-1463">否則，預設的函式的宣告存取範圍是公用的。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1463">Otherwise, the declared accessibility for the default constructor is public.</span></span> <span data-ttu-id="2d25f-1464">因此，預設的函式一律為格式</span><span class="sxs-lookup"><span data-stu-id="2d25f-1464">Thus, the default constructor is always of the form</span></span>

```csharp
protected C(): base() {}
```
<span data-ttu-id="2d25f-1465">或</span><span class="sxs-lookup"><span data-stu-id="2d25f-1465">or</span></span>
```csharp
public C(): base() {}
```
<span data-ttu-id="2d25f-1466">其中 `C` 是類別的名稱。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1466">where `C` is the name of the class.</span></span> <span data-ttu-id="2d25f-1467">如果多載解析無法判斷基類檢查程式初始化運算式的唯一最佳候選，則會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1467">If overload resolution is unable to determine a unique best candidate for the base class constructor initializer then a compile-time error occurs.</span></span>

<span data-ttu-id="2d25f-1468">在範例中</span><span class="sxs-lookup"><span data-stu-id="2d25f-1468">In the example</span></span>
```csharp
class Message
{
    object sender;
    string text;
}
```
<span data-ttu-id="2d25f-1469">會提供預設的函式，因為此類別不包含任何實例的函式宣告。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1469">a default constructor is provided because the class contains no instance constructor declarations.</span></span> <span data-ttu-id="2d25f-1470">因此，此範例完全等同于</span><span class="sxs-lookup"><span data-stu-id="2d25f-1470">Thus, the example is precisely equivalent to</span></span>
```csharp
class Message
{
    object sender;
    string text;

    public Message(): base() {}
}
```

### <a name="private-constructors"></a><span data-ttu-id="2d25f-1471">私用函式</span><span class="sxs-lookup"><span data-stu-id="2d25f-1471">Private constructors</span></span>

<span data-ttu-id="2d25f-1472">當類別 `T` 只宣告私用實例的函式時，`T` 的程式文字以外的類別不可能衍生自 `T` 或直接建立 `T`的實例。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1472">When a class `T` declares only private instance constructors, it is not possible for classes outside the program text of `T` to derive from `T` or to directly create instances of `T`.</span></span> <span data-ttu-id="2d25f-1473">因此，如果類別只包含靜態成員，而不想要具現化，則加入空的私用實例的函式將會導致無法具現化。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1473">Thus, if a class contains only static members and isn't intended to be instantiated, adding an empty private instance constructor will prevent instantiation.</span></span> <span data-ttu-id="2d25f-1474">例如：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1474">For example:</span></span>
```csharp
public class Trig
{
    private Trig() {}        // Prevent instantiation

    public const double PI = 3.14159265358979323846;

    public static double Sin(double x) {...}
    public static double Cos(double x) {...}
    public static double Tan(double x) {...}
}
```

<span data-ttu-id="2d25f-1475">`Trig` 類別會將相關的方法和常數分組，但不打算具現化。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1475">The `Trig` class groups related methods and constants, but is not intended to be instantiated.</span></span> <span data-ttu-id="2d25f-1476">因此，它會宣告單一空白私用實例的函式。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1476">Therefore it declares a single empty private instance constructor.</span></span> <span data-ttu-id="2d25f-1477">至少必須宣告一個實例的函式，以隱藏自動產生預設的函式。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1477">At least one instance constructor must be declared to suppress the automatic generation of a default constructor.</span></span>

### <a name="optional-instance-constructor-parameters"></a><span data-ttu-id="2d25f-1478">選擇性的實例構造函式參數</span><span class="sxs-lookup"><span data-stu-id="2d25f-1478">Optional instance constructor parameters</span></span>

<span data-ttu-id="2d25f-1479">`this(...)` 形式的函式初始化運算式通常會與多載搭配使用，以執行選擇性的實例處理函式參數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1479">The `this(...)` form of constructor initializer is commonly used in conjunction with overloading to implement optional instance constructor parameters.</span></span> <span data-ttu-id="2d25f-1480">在範例中</span><span class="sxs-lookup"><span data-stu-id="2d25f-1480">In the example</span></span>
```csharp
class Text
{
    public Text(): this(0, 0, null) {}

    public Text(int x, int y): this(x, y, null) {}

    public Text(int x, int y, string s) {
        // Actual constructor implementation
    }
}
```
<span data-ttu-id="2d25f-1481">前兩個實例的中繼資料只會提供遺漏引數的預設值。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1481">the first two instance constructors merely provide the default values for the missing arguments.</span></span> <span data-ttu-id="2d25f-1482">兩者都使用 `this(...)` 的「函式初始化運算式」來叫用第三個實例的「函式」，這會實際執行初始化新實例的工作。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1482">Both use a `this(...)` constructor initializer to invoke the third instance constructor, which actually does the work of initializing the new instance.</span></span> <span data-ttu-id="2d25f-1483">這是選擇性的函式參數的效果：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1483">The effect is that of optional constructor parameters:</span></span>
```csharp
Text t1 = new Text();                    // Same as Text(0, 0, null)
Text t2 = new Text(5, 10);               // Same as Text(5, 10, null)
Text t3 = new Text(5, 20, "Hello");
```

## <a name="static-constructors"></a><span data-ttu-id="2d25f-1484">靜態建構函式</span><span class="sxs-lookup"><span data-stu-id="2d25f-1484">Static constructors</span></span>

<span data-ttu-id="2d25f-1485">***靜態***的「函式」是一個成員，它會執行初始化封閉式類別型別的必要動作。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1485">A ***static constructor*** is a member that implements the actions required to initialize a closed class type.</span></span> <span data-ttu-id="2d25f-1486">靜態的函式是使用*static_constructor_declaration*s 來宣告：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1486">Static constructors are declared using *static_constructor_declaration*s:</span></span>

```antlr
static_constructor_declaration
    : attributes? static_constructor_modifiers identifier '(' ')' static_constructor_body
    ;

static_constructor_modifiers
    : 'extern'? 'static'
    | 'static' 'extern'?
    | static_constructor_modifiers_unsafe
    ;

static_constructor_body
    : block
    | ';'
    ;
```

<span data-ttu-id="2d25f-1487">*Static_constructor_declaration*可以包含一組*屬性*（[屬性](attributes.md)）和 `extern` 修飾詞（[外部方法](classes.md#external-methods)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1487">A *static_constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and an `extern` modifier ([External methods](classes.md#external-methods)).</span></span>

<span data-ttu-id="2d25f-1488">*Static_constructor_declaration*的*識別碼*必須命名用來宣告靜態函式的類別。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1488">The *identifier* of a *static_constructor_declaration* must name the class in which the static constructor is declared.</span></span> <span data-ttu-id="2d25f-1489">如果指定了任何其他名稱，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1489">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="2d25f-1490">當靜態的函式宣告包含 `extern` 修飾詞時，靜態的函式會被視為***外部靜態***的函式。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1490">When a static constructor declaration includes an `extern` modifier, the static constructor is said to be an ***external static constructor***.</span></span> <span data-ttu-id="2d25f-1491">因為外部靜態的「函式宣告」不提供實際的實值，所以其*static_constructor_body*由分號組成。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1491">Because an external static constructor declaration provides no actual implementation, its *static_constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="2d25f-1492">針對所有其他的靜態函式宣告， *static_constructor_body*由*區塊*所組成，指定要執行的語句以初始化類別。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1492">For all other static constructor declarations, the *static_constructor_body* consists of a *block* which specifies the statements to execute in order to initialize the class.</span></span> <span data-ttu-id="2d25f-1493">這會完全對應至具有 `void` 傳回類型（[方法主體](classes.md#method-body)）之靜態方法的*method_body* 。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1493">This corresponds exactly to the *method_body* of a static method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="2d25f-1494">靜態的函式不會繼承，也不能直接呼叫。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1494">Static constructors are not inherited, and cannot be called directly.</span></span>

<span data-ttu-id="2d25f-1495">封閉式類別類型的靜態函式在指定的應用程式域中最多隻會執行一次。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1495">The static constructor for a closed class type executes at most once in a given application domain.</span></span> <span data-ttu-id="2d25f-1496">靜態函式的執行會由下列第一個事件所觸發，以在應用程式域內發生：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1496">The execution of a static constructor is triggered by the first of the following events to occur within an application domain:</span></span>

*  <span data-ttu-id="2d25f-1497">建立類別類型的實例。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1497">An instance of the class type is created.</span></span>
*  <span data-ttu-id="2d25f-1498">會參考類別類型的任何靜態成員。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1498">Any of the static members of the class type are referenced.</span></span>

<span data-ttu-id="2d25f-1499">如果類別包含執行開始的 `Main` 方法（[應用程式啟動](basic-concepts.md#application-startup)），則該類別的靜態函式會在呼叫 `Main` 方法之前執行。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1499">If a class contains the `Main` method ([Application Startup](basic-concepts.md#application-startup)) in which execution begins, the static constructor for that class executes before the `Main` method is called.</span></span>

<span data-ttu-id="2d25f-1500">若要初始化新的封閉式類別類型，請先建立該特定封閉式類型的一組新靜態欄位（[靜態和實例欄位](classes.md#static-and-instance-fields)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1500">To initialize a new closed class type, first a new set of static fields ([Static and instance fields](classes.md#static-and-instance-fields)) for that particular closed type is created.</span></span> <span data-ttu-id="2d25f-1501">每個靜態欄位都會初始化為其預設值（[預設值](variables.md#default-values)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1501">Each of the static fields is initialized to its default value ([Default values](variables.md#default-values)).</span></span> <span data-ttu-id="2d25f-1502">接下來，靜態欄位初始化運算式（[靜態欄位初始化](classes.md#static-field-initialization)）會針對那些靜態欄位執行。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1502">Next, the static field initializers ([Static field initialization](classes.md#static-field-initialization)) are executed for those static fields.</span></span> <span data-ttu-id="2d25f-1503">最後，會執行靜態的函式。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1503">Finally, the static constructor is executed.</span></span>

<span data-ttu-id="2d25f-1504">範例</span><span class="sxs-lookup"><span data-stu-id="2d25f-1504">The example</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        A.F();
        B.F();
    }
}

class A
{
    static A() {
        Console.WriteLine("Init A");
    }
    public static void F() {
        Console.WriteLine("A.F");
    }
}

class B
{
    static B() {
        Console.WriteLine("Init B");
    }
    public static void F() {
        Console.WriteLine("B.F");
    }
}
```
<span data-ttu-id="2d25f-1505">必須產生輸出：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1505">must produce the output:</span></span>
```console
Init A
A.F
Init B
B.F
```
<span data-ttu-id="2d25f-1506">因為 `A.F`的呼叫會觸發 `A`的靜態函式的執行，而且 `B`的靜態函式的執行是由 `B.F`的呼叫所觸發。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1506">because the execution of `A`'s static constructor is triggered by the call to `A.F`, and the execution of `B`'s static constructor is triggered by the call to `B.F`.</span></span>

<span data-ttu-id="2d25f-1507">您可以建立迴圈相依性，讓具有變數初始化運算式的靜態欄位得以觀察其預設值狀態。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1507">It is possible to construct circular dependencies that allow static fields with variable initializers to be observed in their default value state.</span></span>

<span data-ttu-id="2d25f-1508">範例</span><span class="sxs-lookup"><span data-stu-id="2d25f-1508">The example</span></span>
```csharp
using System;

class A
{
    public static int X;

    static A() {
        X = B.Y + 1;
    }
}

class B
{
    public static int Y = A.X + 1;

    static B() {}

    static void Main() {
        Console.WriteLine("X = {0}, Y = {1}", A.X, B.Y);
    }
}
```
<span data-ttu-id="2d25f-1509">產生下列輸出</span><span class="sxs-lookup"><span data-stu-id="2d25f-1509">produces the output</span></span>
```console
X = 1, Y = 2
```

<span data-ttu-id="2d25f-1510">若要執行 `Main` 方法，系統會先執行 `B.Y`的初始化運算式，再于類別 `B`的靜態方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1510">To execute the `Main` method, the system first runs the initializer for `B.Y`, prior to class `B`'s static constructor.</span></span> <span data-ttu-id="2d25f-1511">`Y`的初始化運算式會導致 `A`的靜態函式執行，因為參考的是 `A.X` 的值。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1511">`Y`'s initializer causes `A`'s static constructor to be run because the value of `A.X` is referenced.</span></span> <span data-ttu-id="2d25f-1512"> `A` 的靜態函式接著會繼續計算 `X`的值，而這麼做會提取 `Y`的預設值，也就是零。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1512">The static constructor of `A` in turn proceeds to compute the value of `X`, and in doing so fetches the default value of `Y`, which is zero.</span></span> <span data-ttu-id="2d25f-1513">`A.X` 因此會初始化為1。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1513">`A.X` is thus initialized to 1.</span></span> <span data-ttu-id="2d25f-1514">執行 `A`的靜態欄位初始化運算式和靜態處理常式的程式，然後傳回 `Y`初始值的計算，其結果會變成2。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1514">The process of running `A`'s static field initializers and static constructor then completes, returning to the calculation of the initial value of `Y`, the result of which becomes 2.</span></span>

<span data-ttu-id="2d25f-1515">由於靜態的函式只會針對每個封閉的結構化類別類型執行一次，因此，在無法透過條件約束（[類型參數條件約束](classes.md#type-parameter-constraints)）于編譯時期檢查的類型參數上強制執行執行時間檢查是一個方便的位置。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1515">Because the static constructor is executed exactly once for each closed constructed class type, it is a convenient place to enforce run-time checks on the type parameter that cannot be checked at compile-time via constraints ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="2d25f-1516">例如，下列型別會使用靜態的函式來強制型別引數為列舉：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1516">For example, the following type uses a static constructor to enforce that the type argument is an enum:</span></span>
```csharp
class Gen<T> where T: struct
{
    static Gen() {
        if (!typeof(T).IsEnum) {
            throw new ArgumentException("T must be an enum");
        }
    }
}
```

## <a name="destructors"></a><span data-ttu-id="2d25f-1517">解構函式</span><span class="sxs-lookup"><span data-stu-id="2d25f-1517">Destructors</span></span>

<span data-ttu-id="2d25f-1518">「***析構函***式」是一個成員，它會執行銷毀類別的實例所需的動作。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1518">A ***destructor*** is a member that implements the actions required to destruct an instance of a class.</span></span> <span data-ttu-id="2d25f-1519">使用*destructor_declaration*宣告的析構函式：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1519">A destructor is declared using a *destructor_declaration*:</span></span>

```antlr
destructor_declaration
    : attributes? 'extern'? '~' identifier '(' ')' destructor_body
    | destructor_declaration_unsafe
    ;

destructor_body
    : block
    | ';'
    ;
```

<span data-ttu-id="2d25f-1520">*Destructor_declaration*可能包含一組*屬性*（[屬性](attributes.md)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1520">A *destructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="2d25f-1521">*Destructor_declaration*的*識別碼*必須命名為宣告函式的類別。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1521">The *identifier* of a *destructor_declaration* must name the class in which the destructor is declared.</span></span> <span data-ttu-id="2d25f-1522">如果指定了任何其他名稱，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1522">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="2d25f-1523">當析構函式宣告包含 `extern` 修飾詞時，會將此析構函數稱為***外部的析構函***式。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1523">When a destructor declaration includes an `extern` modifier, the destructor is said to be an ***external destructor***.</span></span> <span data-ttu-id="2d25f-1524">因為外部的析構函式宣告不提供實際的實值，所以其*destructor_body*由分號組成。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1524">Because an external destructor declaration provides no actual implementation, its *destructor_body* consists of a semicolon.</span></span> <span data-ttu-id="2d25f-1525">對於所有其他的析構函數， *destructor_body*由*區塊*所組成，其指定要執行的語句，以便銷毀類別的實例。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1525">For all other destructors, the *destructor_body* consists of a *block* which specifies the statements to execute in order to destruct an instance of the class.</span></span> <span data-ttu-id="2d25f-1526">*Destructor_body*與 `void` 傳回型別（[方法主體](classes.md#method-body)）完全對應至實例方法的*method_body* 。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1526">A *destructor_body* corresponds exactly to the *method_body* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="2d25f-1527">不會繼承析構函數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1527">Destructors are not inherited.</span></span> <span data-ttu-id="2d25f-1528">因此，類別沒有任何可在該類別中宣告的析構函數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1528">Thus, a class has no destructors other than the one which may be declared in that class.</span></span>

<span data-ttu-id="2d25f-1529">由於析構函式必須沒有任何參數，因此無法多載，因此一個類別最多隻能有一個析構函式。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1529">Since a destructor is required to have no parameters, it cannot be overloaded, so a class can have, at most, one destructor.</span></span>

<span data-ttu-id="2d25f-1530">系統會自動叫用析構函，而無法明確叫用。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1530">Destructors are invoked automatically, and cannot be invoked explicitly.</span></span> <span data-ttu-id="2d25f-1531">當任何程式碼無法再使用該實例時，實例就會成為可供銷毀的條件。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1531">An instance becomes eligible for destruction when it is no longer possible for any code to use that instance.</span></span> <span data-ttu-id="2d25f-1532">實例的析構函式可能會在實例符合終結條件之後的任何時間執行。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1532">Execution of the destructor for the instance may occur at any time after the instance becomes eligible for destruction.</span></span> <span data-ttu-id="2d25f-1533">解構實例時，會依序呼叫該實例繼承鏈中的析構函數，從最常衍生到最低衍生。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1533">When an instance is destructed, the destructors in that instance's inheritance chain are called, in order, from most derived to least derived.</span></span> <span data-ttu-id="2d25f-1534">可以在任何執行緒上執行一個析構函式。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1534">A destructor may be executed on any thread.</span></span> <span data-ttu-id="2d25f-1535">如需有關控制何時以及如何執行「析構函式」之規則的進一步討論，請參閱[自動記憶體管理](basic-concepts.md#automatic-memory-management)。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1535">For further discussion of the rules that govern when and how a destructor is executed, see [Automatic memory management](basic-concepts.md#automatic-memory-management).</span></span>

<span data-ttu-id="2d25f-1536">範例的輸出</span><span class="sxs-lookup"><span data-stu-id="2d25f-1536">The output of the example</span></span>
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("A's destructor");
    }
}

class B: A
{
    ~B() {
        Console.WriteLine("B's destructor");
    }
}

class Test
{
   static void Main() {
        B b = new B();
        b = null;
        GC.Collect();
        GC.WaitForPendingFinalizers();
   }
}
```
<span data-ttu-id="2d25f-1537">is</span><span class="sxs-lookup"><span data-stu-id="2d25f-1537">is</span></span>
```
B's destructor
A's destructor
```
<span data-ttu-id="2d25f-1538">因為繼承鏈中的析構函數是依順序呼叫，從最常衍生到最不衍生的。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1538">since destructors in an inheritance chain are called in order, from most derived to least derived.</span></span>

<span data-ttu-id="2d25f-1539">藉由覆寫 `System.Object`上 `Finalize` 的虛擬方法，來實作為析構函數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1539">Destructors are implemented by overriding the virtual method `Finalize` on `System.Object`.</span></span> <span data-ttu-id="2d25f-1540">C#不允許程式覆寫此方法，或直接呼叫它（或覆寫）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1540">C# programs are not permitted to override this method or call it (or overrides of it) directly.</span></span> <span data-ttu-id="2d25f-1541">例如，程式</span><span class="sxs-lookup"><span data-stu-id="2d25f-1541">For instance, the program</span></span>
```csharp
class A 
{
    override protected void Finalize() {}    // error

    public void F() {
        this.Finalize();                     // error
    }
}
```
<span data-ttu-id="2d25f-1542">包含兩個錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1542">contains two errors.</span></span>

<span data-ttu-id="2d25f-1543">編譯器的行為就像這個方法和覆寫一樣，完全不存在。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1543">The compiler behaves as if this method, and overrides of it, do not exist at all.</span></span> <span data-ttu-id="2d25f-1544">因此，此程式：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1544">Thus, this program:</span></span>
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
<span data-ttu-id="2d25f-1545">有效，且顯示的方法會隱藏 `System.Object`的 `Finalize` 方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1545">is valid, and the method shown hides `System.Object`'s `Finalize` method.</span></span>

<span data-ttu-id="2d25f-1546">如需從析構函式擲回例外狀況時的行為討論，請參閱[如何處理例外](exceptions.md#how-exceptions-are-handled)狀況。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1546">For a discussion of the behavior when an exception is thrown from a destructor, see [How exceptions are handled](exceptions.md#how-exceptions-are-handled).</span></span>

## <a name="iterators"></a><span data-ttu-id="2d25f-1547">迭代器</span><span class="sxs-lookup"><span data-stu-id="2d25f-1547">Iterators</span></span>

<span data-ttu-id="2d25f-1548">使用 iterator 區塊（[區塊](statements.md#blocks)）所實作用的函式成員（[函數成員](expressions.md#function-members)）稱為***iterator***。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1548">A function member ([Function members](expressions.md#function-members)) implemented using an iterator block ([Blocks](statements.md#blocks)) is called an ***iterator***.</span></span>

<span data-ttu-id="2d25f-1549">只要對應的函式成員的傳回型別是其中一個列舉值介面（[枚舉器介面](classes.md#enumerator-interfaces)）或其中一個可列舉介面（可列舉[介面），](classes.md#enumerable-interfaces)iterator 區塊就可以當做函式成員的主體使用。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1549">An iterator block may be used as the body of a function member as long as the return type of the corresponding function member is one of the enumerator interfaces ([Enumerator interfaces](classes.md#enumerator-interfaces)) or one of the enumerable interfaces ([Enumerable interfaces](classes.md#enumerable-interfaces)).</span></span> <span data-ttu-id="2d25f-1550">它可以是*method_body*、 *operator_body*或*accessor_body*，而事件、實例的函式、靜態的函式和析構函數無法實作為反覆運算器。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1550">It can occur as a *method_body*, *operator_body* or *accessor_body*, whereas events, instance constructors, static constructors and destructors cannot be implemented as iterators.</span></span>

<span data-ttu-id="2d25f-1551">使用 iterator 區塊來實作用函式成員時，函式成員的型式參數清單會發生編譯時期錯誤，以指定任何 `ref` 或 `out` 參數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1551">When a function member is implemented using an iterator block, it is a compile-time error for the formal parameter list of the function member to specify any `ref` or `out` parameters.</span></span>

### <a name="enumerator-interfaces"></a><span data-ttu-id="2d25f-1552">列舉值介面</span><span class="sxs-lookup"><span data-stu-id="2d25f-1552">Enumerator interfaces</span></span>

<span data-ttu-id="2d25f-1553">***列舉值介面***是 `System.Collections.IEnumerator` 的非泛型介面，而且泛型介面 `System.Collections.Generic.IEnumerator<T>`的所有具現化。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1553">The ***enumerator interfaces*** are the non-generic interface `System.Collections.IEnumerator` and all instantiations of the generic interface `System.Collections.Generic.IEnumerator<T>`.</span></span> <span data-ttu-id="2d25f-1554">為了簡潔起見，在本章中，這些介面會分別參考為 `IEnumerator` 和 `IEnumerator<T>`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1554">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerator` and `IEnumerator<T>`, respectively.</span></span>

### <a name="enumerable-interfaces"></a><span data-ttu-id="2d25f-1555">可列舉介面</span><span class="sxs-lookup"><span data-stu-id="2d25f-1555">Enumerable interfaces</span></span>

<span data-ttu-id="2d25f-1556">可列舉***介面***是 `System.Collections.IEnumerable` 的非泛型介面，而且泛型介面 `System.Collections.Generic.IEnumerable<T>`的所有具現化。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1556">The ***enumerable interfaces*** are the non-generic interface `System.Collections.IEnumerable` and all instantiations of the generic interface `System.Collections.Generic.IEnumerable<T>`.</span></span> <span data-ttu-id="2d25f-1557">為了簡潔起見，在本章中，這些介面會分別參考為 `IEnumerable` 和 `IEnumerable<T>`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1557">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerable` and `IEnumerable<T>`, respectively.</span></span>

### <a name="yield-type"></a><span data-ttu-id="2d25f-1558">Yield 類型</span><span class="sxs-lookup"><span data-stu-id="2d25f-1558">Yield type</span></span>

<span data-ttu-id="2d25f-1559">Iterator 會產生一系列的值，這些都是相同的型別。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1559">An iterator produces a sequence of values, all of the same type.</span></span> <span data-ttu-id="2d25f-1560">這個型別稱為 iterator 的***yield 型***別。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1560">This type is called the ***yield type*** of the iterator.</span></span>

*  <span data-ttu-id="2d25f-1561">傳回 `IEnumerator` 或 `IEnumerable` 的反覆運算器產生類型為 `object`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1561">The yield type of an iterator that returns `IEnumerator` or `IEnumerable` is `object`.</span></span>
*  <span data-ttu-id="2d25f-1562">傳回 `IEnumerator<T>` 或 `IEnumerable<T>` 的反覆運算器產生類型為 `T`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1562">The yield type of an iterator that returns `IEnumerator<T>` or `IEnumerable<T>` is `T`.</span></span>

### <a name="enumerator-objects"></a><span data-ttu-id="2d25f-1563">列舉值物件</span><span class="sxs-lookup"><span data-stu-id="2d25f-1563">Enumerator objects</span></span>

<span data-ttu-id="2d25f-1564">當使用 iterator 區塊來執行傳回枚舉器介面類別型的函式成員時，叫用函式成員並不會立即執行 iterator 區塊中的程式碼。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1564">When a function member returning an enumerator interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="2d25f-1565">相反地，會建立並傳回***列舉值物件***。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1565">Instead, an ***enumerator object*** is created and returned.</span></span> <span data-ttu-id="2d25f-1566">這個物件會封裝 iterator 區塊中指定的程式碼，而當叫用列舉值物件的 `MoveNext` 方法時，就會執行 iterator 區塊中的程式碼。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1566">This object encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="2d25f-1567">列舉值物件具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1567">An enumerator object has the following characteristics:</span></span>

*  <span data-ttu-id="2d25f-1568">它會執行 `IEnumerator` 和 `IEnumerator<T>`，其中 `T` 是反覆運算器的產生類型。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1568">It implements `IEnumerator` and `IEnumerator<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="2d25f-1569">它會實作 `System.IDisposable`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1569">It implements `System.IDisposable`.</span></span>
*  <span data-ttu-id="2d25f-1570">它會使用傳遞至函式成員的引數值（如果有）和實例值的複本進行初始化。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1570">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>
*  <span data-ttu-id="2d25f-1571">它有四個可能的狀態： [***之前*** ***]、[執行中]***、[已***暫停***] 和 [***之後***]，***而 [開始于]*** 狀態。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1571">It has four potential states, ***before***, ***running***, ***suspended***, and ***after***, and is initially in the ***before*** state.</span></span>

<span data-ttu-id="2d25f-1572">枚舉器物件通常是編譯器所產生列舉值類別的實例，它會將程式碼封裝在 iterator 區塊中並實作為列舉值介面，但可能會有其他的執行方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1572">An enumerator object is typically an instance of a compiler-generated enumerator class that encapsulates the code in the iterator block and implements the enumerator interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="2d25f-1573">如果列舉值類別是由編譯器產生，則會直接或間接在包含函式成員的類別中嵌套該類別，它將會具有私用存取範圍，而且會有保留給編譯器使用的名稱（[識別碼](lexical-structure.md#identifiers)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1573">If an enumerator class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="2d25f-1574">列舉值物件所執行的介面，可能會比上述指定的數目多。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1574">An enumerator object may implement more interfaces than those specified above.</span></span>

<span data-ttu-id="2d25f-1575">下列各節描述列舉值物件所提供之 `IEnumerable` 和 `IEnumerable<T>` 介面的 `MoveNext`、`Current`和 `Dispose` 成員的確切行為。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1575">The following sections describe the exact behavior of the `MoveNext`, `Current`, and `Dispose` members of the `IEnumerable` and `IEnumerable<T>` interface implementations provided by an enumerator object.</span></span>

<span data-ttu-id="2d25f-1576">請注意，列舉值物件不支援 `IEnumerator.Reset` 方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1576">Note that enumerator objects do not support the `IEnumerator.Reset` method.</span></span> <span data-ttu-id="2d25f-1577">叫用這個方法會導致擲回 `System.NotSupportedException`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1577">Invoking this method causes a `System.NotSupportedException` to be thrown.</span></span>

#### <a name="the-movenext-method"></a><span data-ttu-id="2d25f-1578">MoveNext 方法</span><span class="sxs-lookup"><span data-stu-id="2d25f-1578">The MoveNext method</span></span>

<span data-ttu-id="2d25f-1579">列舉值物件的 `MoveNext` 方法會封裝 iterator 區塊的程式碼。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1579">The `MoveNext` method of an enumerator object encapsulates the code of an iterator block.</span></span> <span data-ttu-id="2d25f-1580">叫用 `MoveNext` 方法會在 iterator 區塊中執行程式碼，並適當地設定列舉值物件的 `Current` 屬性。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1580">Invoking the `MoveNext` method executes code in the iterator block and sets the `Current` property of the enumerator object as appropriate.</span></span> <span data-ttu-id="2d25f-1581">`MoveNext` 所執行的精確動作，取決於叫用 `MoveNext` 時，列舉值物件的狀態：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1581">The precise action performed by `MoveNext` depends on the state of the enumerator object when `MoveNext` is invoked:</span></span>

*  <span data-ttu-id="2d25f-1582">如果列舉值物件的狀態在***之前***，請叫用 `MoveNext`：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1582">If the state of the enumerator object is ***before***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="2d25f-1583">將狀態變更為 [***正在***執行]。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1583">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="2d25f-1584">將反覆運算器區塊的參數（包括 `this`）初始化為初始化枚舉器物件時所儲存的引數值和實例值。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1584">Initializes the parameters (including `this`) of the iterator block to the argument values and instance value saved when the enumerator object was initialized.</span></span>
   * <span data-ttu-id="2d25f-1585">從開始執行反覆運算器區塊，直到執行中斷為止（如下所述）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1585">Executes the iterator block from the beginning until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="2d25f-1586">如果列舉值物件的狀態為 [***正在***執行]，則會未指定叫用 `MoveNext` 的結果。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1586">If the state of the enumerator object is ***running***, the result of invoking `MoveNext` is unspecified.</span></span>
*  <span data-ttu-id="2d25f-1587">如果列舉值物件的狀態為已***暫***止，則叫用 `MoveNext`：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1587">If the state of the enumerator object is ***suspended***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="2d25f-1588">將狀態變更為 [***正在***執行]。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1588">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="2d25f-1589">將所有區域變數和參數的值（包括 this）還原為上次暫停執行反覆運算器區塊時所儲存的值。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1589">Restores the values of all local variables and parameters (including this) to the values saved when execution of the iterator block was last suspended.</span></span> <span data-ttu-id="2d25f-1590">請注意，這些變數所參考的任何物件內容可能會在前一次呼叫 MoveNext 之後變更。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1590">Note that the contents of any objects referenced by these variables may have changed since the previous call to MoveNext.</span></span>
   * <span data-ttu-id="2d25f-1591">在導致執行暫止的 `yield return` 語句之後，繼續執行反覆運算器區塊，直到中斷執行為止（如下所述）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1591">Resumes execution of the iterator block immediately following the `yield return` statement that caused the suspension of execution and continues until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="2d25f-1592">如果列舉值物件的狀態是***after***，叫用 `MoveNext` 會傳回 `false`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1592">If the state of the enumerator object is ***after***, invoking `MoveNext` returns `false`.</span></span>


<span data-ttu-id="2d25f-1593">當 `MoveNext` 執行 iterator 區塊時，可以透過下列四種方式中斷執行：藉由 `yield return` 語句、由 `yield break` 語句、遇到反覆運算器區塊的結尾，以及擲回的例外狀況，並從反覆運算器區塊傳播。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1593">When `MoveNext` executes the iterator block, execution can be interrupted in four ways: By a `yield return` statement, by a `yield break` statement, by encountering the end of the iterator block, and by an exception being thrown and propagated out of the iterator block.</span></span>

*  <span data-ttu-id="2d25f-1594">遇到 `yield return` 語句時（[yield 語句](statements.md#the-yield-statement)）：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1594">When a `yield return` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="2d25f-1595">語句中所指定的運算式會進行評估、隱含地轉換成 yield 型別，以及指派給列舉值物件的 `Current` 屬性。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1595">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
   * <span data-ttu-id="2d25f-1596">反覆運算器主體的執行已暫停。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1596">Execution of the iterator body is suspended.</span></span> <span data-ttu-id="2d25f-1597">所有區域變數和參數的值（包括 `this`）都會儲存起來，就像這個 `yield return` 語句的位置一樣。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1597">The values of all local variables and parameters (including `this`) are saved, as is the location of this `yield return` statement.</span></span> <span data-ttu-id="2d25f-1598">如果 `yield return` 語句位於一個或多個 `try` 區塊內，此時不會執行相關聯的 `finally` 區塊。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1598">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
   * <span data-ttu-id="2d25f-1599">列舉值物件的狀態會變更為 [已***暫停***]。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1599">The state of the enumerator object is changed to ***suspended***.</span></span>
   * <span data-ttu-id="2d25f-1600">`MoveNext` 方法會將 `true` 傳回給其呼叫端，表示反復專案已成功前進到下一個值。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1600">The `MoveNext` method returns `true` to its caller, indicating that the iteration successfully advanced to the next value.</span></span>
*  <span data-ttu-id="2d25f-1601">遇到 `yield break` 語句時（[yield 語句](statements.md#the-yield-statement)）：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1601">When a `yield break` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="2d25f-1602">如果 `yield break` 語句位於一個或多個 `try` 區塊內，則會執行相關聯的 `finally` 區塊。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1602">If the `yield break` statement is within one or more `try` blocks, the associated `finally` blocks are executed.</span></span>
   * <span data-ttu-id="2d25f-1603">列舉值物件的狀態會變更為 [***之後***]。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1603">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="2d25f-1604">`MoveNext` 方法會將 `false` 傳回給其呼叫端，表示反復專案已完成。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1604">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="2d25f-1605">遇到反覆運算器主體的結尾時：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1605">When the end of the iterator body is encountered:</span></span>
   * <span data-ttu-id="2d25f-1606">列舉值物件的狀態會變更為 [***之後***]。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1606">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="2d25f-1607">`MoveNext` 方法會將 `false` 傳回給其呼叫端，表示反復專案已完成。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1607">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="2d25f-1608">當擲回例外狀況並從反覆運算器區塊傳播時：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1608">When an exception is thrown and propagated out of the iterator block:</span></span>
   * <span data-ttu-id="2d25f-1609">反覆運算器主體中適當的 `finally` 區塊，會由例外狀況傳播執行。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1609">Appropriate `finally` blocks in the iterator body will have been executed by the exception propagation.</span></span>
   * <span data-ttu-id="2d25f-1610">列舉值物件的狀態會變更為 [***之後***]。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1610">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="2d25f-1611">例外狀況傳播會繼續到 `MoveNext` 方法的呼叫端。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1611">The exception propagation continues to the caller of the `MoveNext` method.</span></span>

#### <a name="the-current-property"></a><span data-ttu-id="2d25f-1612">目前的屬性</span><span class="sxs-lookup"><span data-stu-id="2d25f-1612">The Current property</span></span>

<span data-ttu-id="2d25f-1613">列舉值物件的 `Current` 屬性會受到 iterator 區塊中 `yield return` 語句的影響。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1613">An enumerator object's `Current` property is affected by `yield return` statements in the iterator block.</span></span>

<span data-ttu-id="2d25f-1614">當枚舉器物件處於***暫停***狀態時，`Current` 的值就是先前呼叫 `MoveNext`所設定的值。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1614">When an enumerator object is in the ***suspended*** state, the value of `Current` is the value set by the previous call to `MoveNext`.</span></span> <span data-ttu-id="2d25f-1615">當列舉值物件處於 [***之前*** ***]、[執行中]*** 或 [***之後***] 狀態時，就不會指定存取 `Current` 的結果。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1615">When an enumerator object is in the ***before***, ***running***, or ***after*** states, the result of accessing `Current` is unspecified.</span></span>

<span data-ttu-id="2d25f-1616">針對具有 `object`以外之 yield 類型的反覆運算器，透過列舉值物件的 `IEnumerable` 執行來存取 `Current` 的結果，會對應到透過列舉值物件的 `IEnumerator<T>` 執行來存取 `Current`，並將結果轉換成 `object`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1616">For an iterator with a yield type other than `object`, the result of accessing `Current` through the enumerator object's `IEnumerable` implementation corresponds to accessing `Current` through the enumerator object's `IEnumerator<T>` implementation and casting the result to `object`.</span></span>

#### <a name="the-dispose-method"></a><span data-ttu-id="2d25f-1617">Dispose 方法</span><span class="sxs-lookup"><span data-stu-id="2d25f-1617">The Dispose method</span></span>

<span data-ttu-id="2d25f-1618">`Dispose` 方法是用來藉由將列舉值物件帶到***after***狀態來清除反復專案。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1618">The `Dispose` method is used to clean up the iteration by bringing the enumerator object to the ***after*** state.</span></span>

*  <span data-ttu-id="2d25f-1619">如果列舉值物件的狀態在***之前***，叫用 `Dispose` 會將狀態變更為 [***之後***]。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1619">If the state of the enumerator object is ***before***, invoking `Dispose` changes the state to ***after***.</span></span>
*  <span data-ttu-id="2d25f-1620">如果列舉值物件的狀態為 [***正在***執行]，則會未指定叫用 `Dispose` 的結果。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1620">If the state of the enumerator object is ***running***, the result of invoking `Dispose` is unspecified.</span></span>
*  <span data-ttu-id="2d25f-1621">如果列舉值物件的狀態為已***暫***止，則叫用 `Dispose`：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1621">If the state of the enumerator object is ***suspended***, invoking `Dispose`:</span></span>
   * <span data-ttu-id="2d25f-1622">將狀態變更為 [***正在***執行]。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1622">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="2d25f-1623">執行任何 finally 區塊，就如同最後一次執行的 `yield return` 語句是 `yield break` 的語句一樣。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1623">Executes any finally blocks as if the last executed `yield return` statement were a `yield break` statement.</span></span> <span data-ttu-id="2d25f-1624">如果這會導致擲回例外狀況，並將其傳播到反覆運算器主體外，則列舉值物件的狀態會設定為***after*** ，而例外狀況會傳播至 `Dispose` 方法的呼叫端。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1624">If this causes an exception to be thrown and propagated out of the iterator body, the state of the enumerator object is set to ***after*** and the exception is propagated to the caller of the `Dispose` method.</span></span>
   * <span data-ttu-id="2d25f-1625">將狀態變更為 [***之後***]。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1625">Changes the state to ***after***.</span></span>
*  <span data-ttu-id="2d25f-1626">如果列舉值物件的狀態是***after***，叫用 `Dispose` 不會有任何影響。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1626">If the state of the enumerator object is ***after***, invoking `Dispose` has no affect.</span></span>

### <a name="enumerable-objects"></a><span data-ttu-id="2d25f-1627">可列舉物件</span><span class="sxs-lookup"><span data-stu-id="2d25f-1627">Enumerable objects</span></span>

<span data-ttu-id="2d25f-1628">當使用 iterator 區塊來執行傳回可列舉介面類別型的函式成員時，叫用函式成員並不會立即執行 iterator 區塊中的程式碼。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1628">When a function member returning an enumerable interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="2d25f-1629">相反地，會建立並傳回可列舉***物件***。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1629">Instead, an ***enumerable object*** is created and returned.</span></span> <span data-ttu-id="2d25f-1630">可列舉物件的 `GetEnumerator` 方法會傳回列舉反覆運算器區塊中指定之程式碼的列舉值物件，而當叫用枚舉器物件的 `MoveNext` 方法時，就會執行 iterator 區塊中的程式碼。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1630">The enumerable object's `GetEnumerator` method returns an enumerator object that encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="2d25f-1631">可列舉物件具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1631">An enumerable object has the following characteristics:</span></span>

*  <span data-ttu-id="2d25f-1632">它會執行 `IEnumerable` 和 `IEnumerable<T>`，其中 `T` 是反覆運算器的產生類型。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1632">It implements `IEnumerable` and `IEnumerable<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="2d25f-1633">它會使用傳遞至函式成員的引數值（如果有）和實例值的複本進行初始化。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1633">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>

<span data-ttu-id="2d25f-1634">可列舉物件通常是編譯器產生之可列舉類別的實例，它會將程式碼封裝在 iterator 區塊中並實作為可列舉介面，但可能的其他方法也是可行的。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1634">An enumerable object is typically an instance of a compiler-generated enumerable class that encapsulates the code in the iterator block and implements the enumerable interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="2d25f-1635">如果可列舉的類別是由編譯器產生，則會直接或間接在包含函式成員的類別中嵌套該類別，它會有私用存取範圍，而且它會有保留供編譯器使用的名稱（[識別碼](lexical-structure.md#identifiers)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1635">If an enumerable class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="2d25f-1636">可列舉物件所執行的介面，可能會比上述指定的數目多。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1636">An enumerable object may implement more interfaces than those specified above.</span></span> <span data-ttu-id="2d25f-1637">特別是，可列舉物件也可以實作為 `IEnumerator` 和 `IEnumerator<T>`，讓它同時做為可列舉和枚舉器。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1637">In particular, an enumerable object may also implement `IEnumerator` and `IEnumerator<T>`, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="2d25f-1638">在這種類型的執行中，第一次叫用可列舉物件的 `GetEnumerator` 方法時，會傳回可列舉物件本身。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1638">In that type of implementation, the first time an enumerable object's `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="2d25f-1639">可列舉物件 `GetEnumerator`的後續調用（如果有的話）會傳回可列舉物件的複本。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1639">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="2d25f-1640">因此，每個傳回的列舉值都有自己的狀態，而其中一個列舉值的變更將不會影響另一個。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1640">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span>

#### <a name="the-getenumerator-method"></a><span data-ttu-id="2d25f-1641">GetEnumerator 方法</span><span class="sxs-lookup"><span data-stu-id="2d25f-1641">The GetEnumerator method</span></span>

<span data-ttu-id="2d25f-1642">可列舉物件提供 `IEnumerable` 和 `IEnumerable<T>` 介面之 `GetEnumerator` 方法的執行。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1642">An enumerable object provides an implementation of the `GetEnumerator` methods of the `IEnumerable` and `IEnumerable<T>` interfaces.</span></span> <span data-ttu-id="2d25f-1643">這兩個 `GetEnumerator` 方法共用一個通用的實值，可取得並傳回可用的列舉值物件。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1643">The two `GetEnumerator` methods share a common implementation that acquires and returns an available enumerator object.</span></span> <span data-ttu-id="2d25f-1644">枚舉器物件會使用初始化可列舉物件時所儲存的引數值和實例值進行初始化，否則列舉值物件會如[列舉值物件](classes.md#enumerator-objects)中所述的方式運作。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1644">The enumerator object is initialized with the argument values and instance value saved when the enumerable object was initialized, but otherwise the enumerator object functions as described in [Enumerator objects](classes.md#enumerator-objects).</span></span>

### <a name="implementation-example"></a><span data-ttu-id="2d25f-1645">執行範例</span><span class="sxs-lookup"><span data-stu-id="2d25f-1645">Implementation example</span></span>

<span data-ttu-id="2d25f-1646">本節描述標準C#結構的可能執行反覆運算器。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1646">This section describes a possible implementation of iterators in terms of standard C# constructs.</span></span> <span data-ttu-id="2d25f-1647">此處所述的執行是以 Microsoft C#編譯器所使用的相同原則為基礎，但它不是強制執行或唯一可行的方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1647">The implementation described here is based on the same principles used by the Microsoft C# compiler, but it is by no means a mandated implementation or the only one possible.</span></span>

<span data-ttu-id="2d25f-1648">下列 `Stack<T>` 類別使用 iterator 來實作為其 `GetEnumerator` 方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1648">The following `Stack<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="2d25f-1649">反覆運算器會以上到下的順序列舉堆疊的元素。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1649">The iterator enumerates the elements of the stack in top to bottom order.</span></span>

```csharp
using System;
using System.Collections;
using System.Collections.Generic;

class Stack<T>: IEnumerable<T>
{
    T[] items;
    int count;

    public void Push(T item) {
        if (items == null) {
            items = new T[4];
        }
        else if (items.Length == count) {
            T[] newItems = new T[count * 2];
            Array.Copy(items, 0, newItems, 0, count);
            items = newItems;
        }
        items[count++] = item;
    }

    public T Pop() {
        T result = items[--count];
        items[count] = default(T);
        return result;
    }

    public IEnumerator<T> GetEnumerator() {
        for (int i = count - 1; i >= 0; --i) yield return items[i];
    }
}
```

<span data-ttu-id="2d25f-1650">`GetEnumerator` 方法可以轉譯為編譯器產生的列舉值類別的具現化，該類別會將程式碼封裝在 iterator 區塊中，如下所示。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1650">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

```csharp
class Stack<T>: IEnumerable<T>
{
    ...

    public IEnumerator<T> GetEnumerator() {
        return new __Enumerator1(this);
    }

    class __Enumerator1: IEnumerator<T>, IEnumerator
    {
        int __state;
        T __current;
        Stack<T> __this;
        int i;

        public __Enumerator1(Stack<T> __this) {
            this.__this = __this;
        }

        public T Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            switch (__state) {
                case 1: goto __state1;
                case 2: goto __state2;
            }
            i = __this.count - 1;
        __loop:
            if (i < 0) goto __state2;
            __current = __this.items[i];
            __state = 1;
            return true;
        __state1:
            --i;
            goto __loop;
        __state2:
            __state = 2;
            return false;
        }

        public void Dispose() {
            __state = 2;
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

<span data-ttu-id="2d25f-1651">在前述的轉譯中，iterator 區塊中的程式碼會轉換成狀態機器，並放在列舉值類別的 `MoveNext` 方法中。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1651">In the preceding translation, the code in the iterator block is turned into a state machine and placed in the `MoveNext` method of the enumerator class.</span></span> <span data-ttu-id="2d25f-1652">此外，本機變數 `i` 會轉換為列舉值物件中的欄位，因此它可以在 `MoveNext`的調用之間繼續存在。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1652">Furthermore, the local variable `i` is turned into a field in the enumerator object so it can continue to exist across invocations of `MoveNext`.</span></span>

<span data-ttu-id="2d25f-1653">下列範例會列印整數1到10的簡單乘法表。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1653">The following example prints a simple multiplication table of the integers 1 through 10.</span></span> <span data-ttu-id="2d25f-1654">範例中的 `FromTo` 方法會傳回可列舉的物件，並使用 iterator 來執行。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1654">The `FromTo` method in the example returns an enumerable object and is implemented using an iterator.</span></span>

```csharp
using System;
using System.Collections.Generic;

class Test
{
    static IEnumerable<int> FromTo(int from, int to) {
        while (from <= to) yield return from++;
    }

    static void Main() {
        IEnumerable<int> e = FromTo(1, 10);
        foreach (int x in e) {
            foreach (int y in e) {
                Console.Write("{0,3} ", x * y);
            }
            Console.WriteLine();
        }
    }
}
```

<span data-ttu-id="2d25f-1655">`FromTo` 方法可以轉譯成編譯器所產生之可列舉類別的具現化，以將程式碼封裝在 iterator 區塊中，如下所示。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1655">The `FromTo` method can be translated into an instantiation of a compiler-generated enumerable class that encapsulates the code in the iterator block, as shown in the following.</span></span>

```csharp
using System;
using System.Threading;
using System.Collections;
using System.Collections.Generic;

class Test
{
    ...

    static IEnumerable<int> FromTo(int from, int to) {
        return new __Enumerable1(from, to);
    }

    class __Enumerable1:
        IEnumerable<int>, IEnumerable,
        IEnumerator<int>, IEnumerator
    {
        int __state;
        int __current;
        int __from;
        int from;
        int to;
        int i;

        public __Enumerable1(int __from, int to) {
            this.__from = __from;
            this.to = to;
        }

        public IEnumerator<int> GetEnumerator() {
            __Enumerable1 result = this;
            if (Interlocked.CompareExchange(ref __state, 1, 0) != 0) {
                result = new __Enumerable1(__from, to);
                result.__state = 1;
            }
            result.from = result.__from;
            return result;
        }

        IEnumerator IEnumerable.GetEnumerator() {
            return (IEnumerator)GetEnumerator();
        }

        public int Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            switch (__state) {
            case 1:
                if (from > to) goto case 2;
                __current = from++;
                __state = 1;
                return true;
            case 2:
                __state = 2;
                return false;
            default:
                throw new InvalidOperationException();
            }
        }

        public void Dispose() {
            __state = 2;
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

<span data-ttu-id="2d25f-1656">可列舉的類別會同時執行可列舉介面和枚舉器介面，讓它同時作為可列舉和枚舉器。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1656">The enumerable class implements both the enumerable interfaces and the enumerator interfaces, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="2d25f-1657">第一次叫用 `GetEnumerator` 方法時，會傳回可列舉物件本身。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1657">The first time the `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="2d25f-1658">可列舉物件 `GetEnumerator`的後續調用（如果有的話）會傳回可列舉物件的複本。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1658">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="2d25f-1659">因此，每個傳回的列舉值都有自己的狀態，而其中一個列舉值的變更將不會影響另一個。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1659">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span> <span data-ttu-id="2d25f-1660">`Interlocked.CompareExchange` 方法是用來確保安全線程作業。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1660">The `Interlocked.CompareExchange` method is used to ensure thread-safe operation.</span></span>

<span data-ttu-id="2d25f-1661">`from` 和 `to` 參數會轉換成可列舉類別中的欄位。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1661">The `from` and `to` parameters are turned into fields in the enumerable class.</span></span> <span data-ttu-id="2d25f-1662">由於 `from` 會在 iterator 區塊中修改，因此會引進額外的 `__from` 欄位，以保存提供給每個列舉值 `from` 的初始值。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1662">Because `from` is modified in the iterator block, an additional `__from` field is introduced to hold the initial value given to `from` in each enumerator.</span></span>

<span data-ttu-id="2d25f-1663">`MoveNext` 方法會在 `0``__state` 時，擲回 `InvalidOperationException`。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1663">The `MoveNext` method throws an `InvalidOperationException` if it is called when `__state` is `0`.</span></span> <span data-ttu-id="2d25f-1664">這可防止在沒有第一次呼叫 `GetEnumerator`的情況下，將可列舉物件當做枚舉器物件使用。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1664">This protects against use of the enumerable object as an enumerator object without first calling `GetEnumerator`.</span></span>

<span data-ttu-id="2d25f-1665">下列範例顯示簡單的樹狀目錄類別。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1665">The following example shows a simple tree class.</span></span> <span data-ttu-id="2d25f-1666">`Tree<T>` 類別會使用 iterator 來執行其 `GetEnumerator` 方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1666">The `Tree<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="2d25f-1667">反覆運算器會以中綴順序列舉樹狀結構的元素。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1667">The iterator enumerates the elements of the tree in infix order.</span></span>

```csharp
using System;
using System.Collections.Generic;

class Tree<T>: IEnumerable<T>
{
    T value;
    Tree<T> left;
    Tree<T> right;

    public Tree(T value, Tree<T> left, Tree<T> right) {
        this.value = value;
        this.left = left;
        this.right = right;
    }

    public IEnumerator<T> GetEnumerator() {
        if (left != null) foreach (T x in left) yield x;
        yield value;
        if (right != null) foreach (T x in right) yield x;
    }
}

class Program
{
    static Tree<T> MakeTree<T>(T[] items, int left, int right) {
        if (left > right) return null;
        int i = (left + right) / 2;
        return new Tree<T>(items[i], 
            MakeTree(items, left, i - 1),
            MakeTree(items, i + 1, right));
    }

    static Tree<T> MakeTree<T>(params T[] items) {
        return MakeTree(items, 0, items.Length - 1);
    }

    // The output of the program is:
    // 1 2 3 4 5 6 7 8 9
    // Mon Tue Wed Thu Fri Sat Sun

    static void Main() {
        Tree<int> ints = MakeTree(1, 2, 3, 4, 5, 6, 7, 8, 9);
        foreach (int i in ints) Console.Write("{0} ", i);
        Console.WriteLine();

        Tree<string> strings = MakeTree(
            "Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun");
        foreach (string s in strings) Console.Write("{0} ", s);
        Console.WriteLine();
    }
}
```

<span data-ttu-id="2d25f-1668">`GetEnumerator` 方法可以轉譯為編譯器產生的列舉值類別的具現化，該類別會將程式碼封裝在 iterator 區塊中，如下所示。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1668">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

```csharp
class Tree<T>: IEnumerable<T>
{
    ...

    public IEnumerator<T> GetEnumerator() {
        return new __Enumerator1(this);
    }

    class __Enumerator1 : IEnumerator<T>, IEnumerator
    {
        Node<T> __this;
        IEnumerator<T> __left, __right;
        int __state;
        T __current;

        public __Enumerator1(Node<T> __this) {
            this.__this = __this;
        }

        public T Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            try {
                switch (__state) {

                case 0:
                    __state = -1;
                    if (__this.left == null) goto __yield_value;
                    __left = __this.left.GetEnumerator();
                    goto case 1;

                case 1:
                    __state = -2;
                    if (!__left.MoveNext()) goto __left_dispose;
                    __current = __left.Current;
                    __state = 1;
                    return true;

                __left_dispose:
                    __state = -1;
                    __left.Dispose();

                __yield_value:
                    __current = __this.value;
                    __state = 2;
                    return true;

                case 2:
                    __state = -1;
                    if (__this.right == null) goto __end;
                    __right = __this.right.GetEnumerator();
                    goto case 3;

                case 3:
                    __state = -3;
                    if (!__right.MoveNext()) goto __right_dispose;
                    __current = __right.Current;
                    __state = 3;
                    return true;

                __right_dispose:
                    __state = -1;
                    __right.Dispose();

                __end:
                    __state = 4;
                    break;

                }
            }
            finally {
                if (__state < 0) Dispose();
            }
            return false;
        }

        public void Dispose() {
            try {
                switch (__state) {

                case 1:
                case -2:
                    __left.Dispose();
                    break;

                case 3:
                case -3:
                    __right.Dispose();
                    break;

                }
            }
            finally {
                __state = 4;
            }
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

<span data-ttu-id="2d25f-1669">在 `foreach` 語句中使用的編譯器產生的而暫存物件會提升至列舉值物件的 `__left` 和 `__right` 欄位。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1669">The compiler generated temporaries used in the `foreach` statements are lifted into the `__left` and `__right` fields of the enumerator object.</span></span> <span data-ttu-id="2d25f-1670">列舉值物件的 `__state` 欄位會經過仔細更新，如此一來，如果擲回例外狀況，就會正確地呼叫正確的 `Dispose()` 方法。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1670">The `__state` field of the enumerator object is carefully updated so that the correct `Dispose()` method will be called correctly if an exception is thrown.</span></span> <span data-ttu-id="2d25f-1671">請注意，您無法使用簡單的 `foreach` 語句來撰寫已轉譯的程式碼。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1671">Note that it is not possible to write the translated code with simple `foreach` statements.</span></span>

## <a name="async-functions"></a><span data-ttu-id="2d25f-1672">Async 函數</span><span class="sxs-lookup"><span data-stu-id="2d25f-1672">Async functions</span></span>

<span data-ttu-id="2d25f-1673">具有 `async` 修飾詞的方法（[方法](classes.md#methods)）或匿名函式（匿名函式[運算式](expressions.md#anonymous-function-expressions)）稱為***非同步函數***。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1673">A method ([Methods](classes.md#methods)) or anonymous function ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) with the `async` modifier is called an ***async function***.</span></span> <span data-ttu-id="2d25f-1674">一般來說，「***非同步***」一詞是用來描述具有 `async` 修飾詞的任何一種函數。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1674">In general, the term ***async*** is used to describe any kind of function that has the `async` modifier.</span></span>

<span data-ttu-id="2d25f-1675">非同步函式的型式參數清單若要指定任何 `ref` 或 `out` 參數，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1675">It is a compile-time error for the formal parameter list of an async function to specify any `ref` or `out` parameters.</span></span>

<span data-ttu-id="2d25f-1676">非同步方法的*return_type*必須是 `void` 或工作***類型***。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1676">The *return_type* of an async method must be either `void` or a ***task type***.</span></span> <span data-ttu-id="2d25f-1677">工作類型是 `System.Threading.Tasks.Task`，而類型是由 `System.Threading.Tasks.Task<T>`所構成。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1677">The task types are `System.Threading.Tasks.Task` and types constructed from `System.Threading.Tasks.Task<T>`.</span></span> <span data-ttu-id="2d25f-1678">為了簡潔起見，在本章中，這些類型分別會當做 `Task` 和 `Task<T>`參考。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1678">For the sake of brevity, in this chapter these types are referenced as `Task` and `Task<T>`, respectively.</span></span> <span data-ttu-id="2d25f-1679">傳回工作類型的非同步方法稱為「工作傳回」（task）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1679">An async method returning a task type is said to be task-returning.</span></span>

<span data-ttu-id="2d25f-1680">工作類型的確切定義是已定義的執行，但從語言的觀點來看，工作類型的狀態為 [不完整]、[成功] 或 [錯誤]。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1680">The exact definition of the task types is implementation defined, but from the language's point of view a task type is in one of the states incomplete, succeeded or faulted.</span></span> <span data-ttu-id="2d25f-1681">錯誤的工作會記錄相關的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1681">A faulted task records a pertinent exception.</span></span> <span data-ttu-id="2d25f-1682">成功的 `Task<T>` 會記錄類型 `T`的結果。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1682">A succeeded `Task<T>` records a result of type `T`.</span></span> <span data-ttu-id="2d25f-1683">工作類型是可等候，因此可以是 await 運算式的運算元（[await 運算式](expressions.md#await-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1683">Task types are awaitable, and can therefore be the operands of await expressions ([Await expressions](expressions.md#await-expressions)).</span></span>

<span data-ttu-id="2d25f-1684">非同步函式呼叫能夠透過其主體中的 await 運算式（[await 運算式](expressions.md#await-expressions)）來暫停評估。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1684">An async function invocation has the ability to suspend evaluation by means of await expressions ([Await expressions](expressions.md#await-expressions)) in its body.</span></span> <span data-ttu-id="2d25f-1685">稍後可以透過繼續***委派***，在暫停 await 運算式的時間點繼續評估。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1685">Evaluation may later be resumed at the point of the suspending await expression by means of a ***resumption delegate***.</span></span> <span data-ttu-id="2d25f-1686">繼續委派的類型為 `System.Action`，而當叫用時，非同步函式呼叫的評估會從停止的 await 運算式繼續進行。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1686">The resumption delegate is of type `System.Action`, and when it is invoked, evaluation of the async function invocation will resume from the await expression where it left off.</span></span> <span data-ttu-id="2d25f-1687">如果函式呼叫從未暫止，或是繼續委派的最新呼叫端，則非同步函式調用的***目前呼叫***端是原始呼叫端。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1687">The ***current caller*** of an async function invocation is the original caller if the function invocation has never been suspended, or the most recent caller of the resumption delegate otherwise.</span></span>

### <a name="evaluation-of-a-task-returning-async-function"></a><span data-ttu-id="2d25f-1688">評估工作傳回的 async 函數</span><span class="sxs-lookup"><span data-stu-id="2d25f-1688">Evaluation of a task-returning async function</span></span>

<span data-ttu-id="2d25f-1689">叫用工作傳回的非同步函式，會產生所傳回工作類型的實例。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1689">Invocation of a task-returning async function causes an instance of the returned task type to be generated.</span></span> <span data-ttu-id="2d25f-1690">這稱為 async 函數***的傳回工作。***</span><span class="sxs-lookup"><span data-stu-id="2d25f-1690">This is called the ***return task*** of the async function.</span></span> <span data-ttu-id="2d25f-1691">工作最初處於不完整的狀態。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1691">The task is initially in an incomplete state.</span></span>

<span data-ttu-id="2d25f-1692">接著會評估 async 函式主體，直到它暫停（藉由到達 await 運算式）或終止為止，此時會將控制項傳回給呼叫端，以及傳回的工作。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1692">The async function body is then evaluated until it is either suspended (by reaching an await expression) or terminates, at which point control is returned to the caller, along with the return task.</span></span>

<span data-ttu-id="2d25f-1693">當 async 函式的主體終止時，傳回的工作會移出不完整的狀態：</span><span class="sxs-lookup"><span data-stu-id="2d25f-1693">When the body of the async function terminates, the return task is moved out of the incomplete state:</span></span>

*  <span data-ttu-id="2d25f-1694">如果函式主體因到達 return 語句或主體結尾而終止，則會在傳回工作中記錄任何結果值，而該工作會進入成功狀態。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1694">If the function body terminates as the result of reaching a return statement or the end of the body, any result value is recorded in the return task, which is put into a succeeded state.</span></span>
*  <span data-ttu-id="2d25f-1695">如果函式主體因未攔截的例外狀況（[throw 語句](statements.md#the-throw-statement)）而終止，則會在傳回錯誤狀態的傳回工作中記錄例外狀況。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1695">If the function body terminates as the result of an uncaught exception ([The throw statement](statements.md#the-throw-statement)) the exception is recorded in the return task which is put into a faulted state.</span></span>

### <a name="evaluation-of-a-void-returning-async-function"></a><span data-ttu-id="2d25f-1696">評估傳回 void 的 async 函數</span><span class="sxs-lookup"><span data-stu-id="2d25f-1696">Evaluation of a void-returning async function</span></span>

<span data-ttu-id="2d25f-1697">如果非同步函式的傳回型別是 `void`，則評估會以下列方式與上述不同：因為不會傳回任何工作，所以函式會改為將完成和例外狀況傳達給目前線程的***同步處理內容***。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1697">If the return type of the async function is `void`, evaluation differs from the above in the following way: Because no task is returned, the function instead communicates completion and exceptions to the current thread's ***synchronization context***.</span></span> <span data-ttu-id="2d25f-1698">同步處理內容的確切定義與實作為相依，但表示目前線程正在執行的「位置」。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1698">The exact definition of synchronization context is implementation-dependent, but is a representation of "where" the current thread is running.</span></span> <span data-ttu-id="2d25f-1699">當評估傳回的非同步函式開始、成功完成，或導致擲回未攔截的例外狀況時，會通知同步處理內容。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1699">The synchronization context is notified when evaluation of a void-returning async function commences, completes successfully, or causes an uncaught exception to be thrown.</span></span>

<span data-ttu-id="2d25f-1700">這可讓內容追蹤在其底下執行的空傳回非同步函式數目，以及決定如何傳播來自它們的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="2d25f-1700">This allows the context to keep track of how many void-returning async functions are running under it, and to decide how to propagate exceptions coming out of them.</span></span>
