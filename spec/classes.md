# <a name="classes"></a><span data-ttu-id="6fe1a-101">類別</span><span class="sxs-lookup"><span data-stu-id="6fe1a-101">Classes</span></span>

<span data-ttu-id="6fe1a-102">類別是資料結構，其中可能包含資料成員 （常數和欄位），函式成員 （方法、 屬性、 事件、 索引子、 運算子、 執行個體建構函式、 解構函式和靜態建構函式） 和巢狀型別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-102">A class is a data structure that may contain data members (constants and fields), function members (methods, properties, events, indexers, operators, instance constructors, destructors and static constructors), and nested types.</span></span> <span data-ttu-id="6fe1a-103">類別類型支援繼承，衍生的類別可以延伸及特製化的基底類別的其中一種機制。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-103">Class types support inheritance, a mechanism whereby a derived class can extend and specialize a base class.</span></span>

## <a name="class-declarations"></a><span data-ttu-id="6fe1a-104">類別宣告</span><span class="sxs-lookup"><span data-stu-id="6fe1a-104">Class declarations</span></span>

<span data-ttu-id="6fe1a-105">A *class_declaration*是*type_declaration* ([型別宣告](namespaces.md#type-declarations)) 宣告的新類別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-105">A *class_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new class.</span></span>

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

<span data-ttu-id="6fe1a-106">A *class_declaration*組成的一組選擇性*屬性*([屬性](attributes.md))，後面接著一組選擇性的*class_modifier*s ([類別修飾詞](classes.md#class-modifiers))，後面接著選擇性`partial`修飾詞，後面接著關鍵字`class`並*識別碼*可命名的類別，後面接著選擇性*type_parameter_list* ([型別參數](classes.md#type-parameters))，後面接著選擇性*class_base*規格 ([類別的基底規格](classes.md#class-base-specification))，後面接著一組選擇性*type_parameter_constraints_clause*s ([類型參數條件約束](classes.md#type-parameter-constraints))，後面接著*class_body* ([類別主體](classes.md#class-body))，或者後面接著一個分號。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-106">A *class_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *class_modifier*s ([Class modifiers](classes.md#class-modifiers)), followed by an optional `partial` modifier, followed by the keyword `class` and an *identifier* that names the class, followed by an optional *type_parameter_list* ([Type parameters](classes.md#type-parameters)), followed by an optional *class_base* specification ([Class base specification](classes.md#class-base-specification)) , followed by an optional set of *type_parameter_constraints_clause*s ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by a *class_body* ([Class body](classes.md#class-body)), optionally followed by a semicolon.</span></span>

<span data-ttu-id="6fe1a-107">在類別宣告無法提供*type_parameter_constraints_clause*s 除非還會提供*type_parameter_list*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-107">A class declaration cannot supply *type_parameter_constraints_clause*s unless it also supplies a *type_parameter_list*.</span></span>

<span data-ttu-id="6fe1a-108">提供的類別宣告*type_parameter_list*是***泛型類別宣告***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-108">A class declaration that supplies a *type_parameter_list* is a ***generic class declaration***.</span></span> <span data-ttu-id="6fe1a-109">此外，巢狀在泛型類別宣告或泛型結構宣告內的任何類別本身是泛型類別宣告，因為建立建構的型別都必須提供包含類型的型別參數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-109">Additionally, any class nested inside a generic class declaration or a generic struct declaration is itself a generic class declaration, since type parameters for the containing type must be supplied to create a constructed type.</span></span>

### <a name="class-modifiers"></a><span data-ttu-id="6fe1a-110">類別修飾詞</span><span class="sxs-lookup"><span data-stu-id="6fe1a-110">Class modifiers</span></span>

<span data-ttu-id="6fe1a-111">A *class_declaration*可以選擇性地包含一連串的類別修飾詞：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-111">A *class_declaration* may optionally include a sequence of class modifiers:</span></span>

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

<span data-ttu-id="6fe1a-112">它是在類別宣告中出現多次相同的修飾詞的編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-112">It is a compile-time error for the same modifier to appear multiple times in a class declaration.</span></span>

<span data-ttu-id="6fe1a-113">`new`巢狀類別使用修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-113">The `new` modifier is permitted on nested classes.</span></span> <span data-ttu-id="6fe1a-114">它會指定類別隱藏繼承的成員，以相同的名稱，如中所述[的新修飾詞](classes.md#the-new-modifier)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-114">It specifies that the class hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span> <span data-ttu-id="6fe1a-115">它是編譯時期錯誤`new`出現在類別宣告內，不是巢狀的類別宣告修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-115">It is a compile-time error for the `new` modifier to appear on a class declaration that is not a nested class declaration.</span></span>

<span data-ttu-id="6fe1a-116">`public`， `protected`， `internal`，和`private`修飾詞可以控制類別的存取範圍。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-116">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the class.</span></span> <span data-ttu-id="6fe1a-117">根據在類別宣告發生所在的內容，這些修飾詞的一些可能不允許 ([宣告存取範圍](basic-concepts.md#declared-accessibility))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-117">Depending on the context in which the class declaration occurs, some of these modifiers may not be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="6fe1a-118">`abstract`，`sealed`和`static`修飾詞會在下列各節中討論。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-118">The `abstract`, `sealed` and `static` modifiers are discussed in the following sections.</span></span>

#### <a name="abstract-classes"></a><span data-ttu-id="6fe1a-119">抽象類別</span><span class="sxs-lookup"><span data-stu-id="6fe1a-119">Abstract classes</span></span>

<span data-ttu-id="6fe1a-120">`abstract`修飾詞用來指出類別是不完整，且它用來只當做基底類別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-120">The `abstract` modifier is used to indicate that a class is incomplete and that it is intended to be used only as a base class.</span></span> <span data-ttu-id="6fe1a-121">自非抽象類別的抽象類別是在利用下列方式有所不同：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-121">An abstract class differs from a non-abstract class in the following ways:</span></span>

*  <span data-ttu-id="6fe1a-122">抽象類別無法直接執行個體化，而且使用編譯時間錯誤`new`運算子在抽象類別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-122">An abstract class cannot be instantiated directly, and it is a compile-time error to use the `new` operator on an abstract class.</span></span> <span data-ttu-id="6fe1a-123">雖然您可以將變數和其編譯時期型別是抽象的值，這類變數和值一定是會`null`或包含衍生自抽象類型的非抽象類別的執行個體的參考。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-123">While it is possible to have variables and values whose compile-time types are abstract, such variables and values will necessarily either be `null` or contain references to instances of non-abstract classes derived from the abstract types.</span></span>
*  <span data-ttu-id="6fe1a-124">抽象類別是允許 （但非必要） 包含抽象成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-124">An abstract class is permitted (but not required) to contain abstract members.</span></span>
*  <span data-ttu-id="6fe1a-125">無法密封抽象類別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-125">An abstract class cannot be sealed.</span></span>

<span data-ttu-id="6fe1a-126">當非抽象類別衍生自抽象類別時，非抽象類別必須包含所有繼承抽象成員，藉此覆寫這些抽象成員的實際實作。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-126">When a non-abstract class is derived from an abstract class, the non-abstract class must include actual implementations of all inherited abstract members, thereby overriding those abstract members.</span></span> <span data-ttu-id="6fe1a-127">在範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-127">In the example</span></span>
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
<span data-ttu-id="6fe1a-128">抽象類別`A`導入了抽象方法`F`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-128">the abstract class `A` introduces an abstract method `F`.</span></span> <span data-ttu-id="6fe1a-129">類別`B`導入了另一種方法`G`，因為它並未提供的實作，但`F`，`B`也必須宣告為抽象。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-129">Class `B` introduces an additional method `G`, but since it doesn't provide an implementation of `F`, `B` must also be declared abstract.</span></span> <span data-ttu-id="6fe1a-130">類別`C`會覆寫`F`，並提供實際的實作。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-130">Class `C` overrides `F` and provides an actual implementation.</span></span> <span data-ttu-id="6fe1a-131">因為沒有在抽象成員`C`， `C` ，允許 （但非必要） 為非抽象。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-131">Since there are no abstract members in `C`, `C` is permitted (but not required) to be non-abstract.</span></span>

#### <a name="sealed-classes"></a><span data-ttu-id="6fe1a-132">密封的類別</span><span class="sxs-lookup"><span data-stu-id="6fe1a-132">Sealed classes</span></span>

<span data-ttu-id="6fe1a-133">`sealed`修飾詞用來防止衍生的類別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-133">The `sealed` modifier is used to prevent derivation from a class.</span></span> <span data-ttu-id="6fe1a-134">如果密封的類別指定為另一個類別的基底類別，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-134">A compile-time error occurs if a sealed class is specified as the base class of another class.</span></span>

<span data-ttu-id="6fe1a-135">密封的類別不能也是抽象類別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-135">A sealed class cannot also be an abstract class.</span></span>

<span data-ttu-id="6fe1a-136">`sealed`修飾詞主要用來防止意外的衍生，但它也可讓執行階段的特定最佳化。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-136">The `sealed` modifier is primarily used to prevent unintended derivation, but it also enables certain run-time optimizations.</span></span> <span data-ttu-id="6fe1a-137">特別是，由於密封的類別永遠不會有任何衍生的類別，就可以將密封的類別執行個體上的虛擬函式成員引動過程轉換成非虛擬的引動過程。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-137">In particular, because a sealed class is known to never have any derived classes, it is possible to transform virtual function member invocations on sealed class instances into non-virtual invocations.</span></span>

#### <a name="static-classes"></a><span data-ttu-id="6fe1a-138">靜態類別</span><span class="sxs-lookup"><span data-stu-id="6fe1a-138">Static classes</span></span>

<span data-ttu-id="6fe1a-139">`static`修飾詞用來將被宣告為類別標記***靜態類別***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-139">The `static` modifier is used to mark the class being declared as a ***static class***.</span></span> <span data-ttu-id="6fe1a-140">靜態類別無法具現化不能用做為類型，可以只包含靜態成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-140">A static class cannot be instantiated, cannot be used as a type and can contain only static members.</span></span> <span data-ttu-id="6fe1a-141">只有靜態類別可以包含宣告的擴充方法 ([擴充方法](classes.md#extension-methods))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-141">Only a static class can contain declarations of extension methods ([Extension methods](classes.md#extension-methods)).</span></span>

<span data-ttu-id="6fe1a-142">靜態類別宣告受限於下列限制：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-142">A static class declaration is subject to the following restrictions:</span></span>

*  <span data-ttu-id="6fe1a-143">靜態類別不能包含`sealed`或`abstract`修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-143">A static class may not include a `sealed` or `abstract` modifier.</span></span> <span data-ttu-id="6fe1a-144">不過請注意，因為無法具現化或衍生自靜態類別，其行為就如同它已密封且抽象。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-144">Note, however, that since a static class cannot be instantiated or derived from, it behaves as if it was both sealed and abstract.</span></span>
*  <span data-ttu-id="6fe1a-145">靜態類別不能包含*class_base*規格 ([類別的基底規格](classes.md#class-base-specification)) 並無法明確指定的基底類別或實作介面的清單。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-145">A static class may not include a *class_base* specification ([Class base specification](classes.md#class-base-specification)) and cannot explicitly specify a base class or a list of implemented interfaces.</span></span> <span data-ttu-id="6fe1a-146">靜態類別隱含繼承自型別`object`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-146">A static class implicitly inherits from type `object`.</span></span>
*  <span data-ttu-id="6fe1a-147">靜態類別只能包含靜態成員 ([靜態和執行個體成員](classes.md#static-and-instance-members))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-147">A static class can only contain static members ([Static and instance members](classes.md#static-and-instance-members)).</span></span> <span data-ttu-id="6fe1a-148">請注意，會將常數和巢狀型別歸類為靜態成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-148">Note that constants and nested types are classified as static members.</span></span>
*  <span data-ttu-id="6fe1a-149">靜態類別不能有具有成員`protected`或`protected internal`宣告存取範圍。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-149">A static class cannot have members with `protected` or `protected internal` declared accessibility.</span></span>

<span data-ttu-id="6fe1a-150">它是編譯時期錯誤違反任何限制。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-150">It is a compile-time error to violate any of these restrictions.</span></span>

<span data-ttu-id="6fe1a-151">靜態類別有任何執行個體建構函式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-151">A static class has no instance constructors.</span></span> <span data-ttu-id="6fe1a-152">不可能在靜態類別中，執行個體建構函式和任何預設執行個體建構函式宣告 ([預設建構函式](classes.md#default-constructors)) 提供靜態類別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-152">It is not possible to declare an instance constructor in a static class, and no default instance constructor ([Default constructors](classes.md#default-constructors)) is provided for a static class.</span></span>

<span data-ttu-id="6fe1a-153">靜態類別的成員自動靜態的而且非成員宣告必須明確包含`static`修飾詞 （除了常數和巢狀型別）。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-153">The members of a static class are not automatically static, and the member declarations must explicitly include a `static` modifier (except for constants and nested types).</span></span> <span data-ttu-id="6fe1a-154">類別是巢狀結構，靜態的外部類別內，巢狀的類別不是靜態類別，除非它明確包含`static`修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-154">When a class is nested within a static outer class, the nested class is not a static class unless it explicitly includes a `static` modifier.</span></span>

<span data-ttu-id="6fe1a-155">__參考靜態類別類型__</span><span class="sxs-lookup"><span data-stu-id="6fe1a-155">__Referencing static class types__</span></span>

<span data-ttu-id="6fe1a-156">A *namespace_or_type_name* ([命名空間和型別名稱](basic-concepts.md#namespace-and-type-names)) 可以參考靜態類別</span><span class="sxs-lookup"><span data-stu-id="6fe1a-156">A *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="6fe1a-157">*Namespace_or_type_name*是`T`中*namespace_or_type_name*格式的`T.I`，或</span><span class="sxs-lookup"><span data-stu-id="6fe1a-157">The *namespace_or_type_name* is the `T` in a *namespace_or_type_name* of the form `T.I`, or</span></span>
*  <span data-ttu-id="6fe1a-158">*Namespace_or_type_name*是`T`中*typeof_expression* ([引數清單](expressions.md#argument-lists)1) 的形式`typeof(T)`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-158">The *namespace_or_type_name* is the `T` in a *typeof_expression* ([Argument lists](expressions.md#argument-lists)1) of the form `typeof(T)`.</span></span>

<span data-ttu-id="6fe1a-159">A *primary_expression* ([函式成員](expressions.md#function-members)) 可以參考靜態類別</span><span class="sxs-lookup"><span data-stu-id="6fe1a-159">A *primary_expression* ([Function members](expressions.md#function-members)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="6fe1a-160">*Primary_expression*是`E`中*member_access* ([編譯時期檢查動態的多載解析](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) 的形式`E.I`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-160">The *primary_expression* is the `E` in a *member_access* ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) of the form `E.I`.</span></span>

<span data-ttu-id="6fe1a-161">在任何其他內容中它是參考靜態類別的編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-161">In any other context it is a compile-time error to reference a static class.</span></span> <span data-ttu-id="6fe1a-162">比方說，它是靜態類別，以用做為基底類別，構成類型的錯誤 ([巢狀型別](classes.md#nested-types)) 成員 」、 「 泛用型別引數，或 「 型別參數條件約束。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-162">For example, it is an error for a static class to be used as a base class, a constituent type ([Nested types](classes.md#nested-types)) of a member, a generic type argument, or a type parameter constraint.</span></span> <span data-ttu-id="6fe1a-163">同樣地，靜態類別不能在陣列類型、 指標類型`new`運算式，轉型運算式，`is`運算式`as`運算式，`sizeof`運算式或預設值運算式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-163">Likewise, a static class cannot be used in an array type, a pointer type, a `new` expression, a cast expression, an `is` expression, an `as` expression, a `sizeof` expression, or a default value expression.</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="6fe1a-164">Partial 修飾詞</span><span class="sxs-lookup"><span data-stu-id="6fe1a-164">Partial modifier</span></span>

<span data-ttu-id="6fe1a-165">`partial`修飾詞用來指示這個*class_declaration*是部分的型別宣告。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-165">The `partial` modifier is used to indicate that this *class_declaration* is a partial type declaration.</span></span> <span data-ttu-id="6fe1a-166">構成一個型別宣告，結合多個具有相同的名稱，在封入的命名空間或類型宣告中的部分型別宣告，下列規則中指定[部分型別](classes.md#partial-types)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-166">Multiple partial type declarations with the same name within an enclosing namespace or type declaration combine to form one type declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

<span data-ttu-id="6fe1a-167">讓分散在不同區段的程式文字類別的宣告可以是很有用，如果這些區段會產生或保存在不同的內容。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-167">Having the declaration of a class distributed over separate segments of program text can be useful if these segments are produced or maintained in different contexts.</span></span> <span data-ttu-id="6fe1a-168">比方說，在類別宣告的其中一部分可能是電腦產生，，而其他手動撰寫。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-168">For instance, one part of a class declaration may be machine generated, whereas the other is manually authored.</span></span> <span data-ttu-id="6fe1a-169">兩個文字分隔會防止一個之間發生衝突的更新，其他更新。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-169">Textual separation of the two prevents updates by one from conflicting with updates by the other.</span></span>

### <a name="type-parameters"></a><span data-ttu-id="6fe1a-170">型別參數</span><span class="sxs-lookup"><span data-stu-id="6fe1a-170">Type parameters</span></span>

<span data-ttu-id="6fe1a-171">型別參數是簡單的識別項，表示型別引數提供給建立建構的類型的預留位置。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-171">A type parameter is a simple identifier that denotes a placeholder for a type argument supplied to create a constructed type.</span></span> <span data-ttu-id="6fe1a-172">型別參數是一種類型，稍後會提供正式預留位置。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-172">A type parameter is a formal placeholder for a type that will be supplied later.</span></span> <span data-ttu-id="6fe1a-173">相較之下，型別引數 ([型別引數](types.md#type-arguments)) 用來替代類型參數建構的型別建立時的實際類型。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-173">By contrast, a type argument ([Type arguments](types.md#type-arguments)) is the actual type that is substituted for the type parameter when a constructed type is created.</span></span>

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

<span data-ttu-id="6fe1a-174">在類別宣告中的每個類型參數的宣告空間中定義的名稱 ([宣告](basic-concepts.md#declarations)) 該類別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-174">Each type parameter in a class declaration defines a name in the declaration space ([Declarations](basic-concepts.md#declarations)) of that class.</span></span> <span data-ttu-id="6fe1a-175">因此，它不能有另一個類型參數名稱相同，或在該類別中宣告的成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-175">Thus, it cannot have the same name as another type parameter or a member declared in that class.</span></span> <span data-ttu-id="6fe1a-176">型別參數不能有型別本身相同的名稱。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-176">A type parameter cannot have the same name as the type itself.</span></span>

### <a name="class-base-specification"></a><span data-ttu-id="6fe1a-177">類別的基底規格</span><span class="sxs-lookup"><span data-stu-id="6fe1a-177">Class base specification</span></span>

<span data-ttu-id="6fe1a-178">類別宣告可能包含*class_base*規格，其中定義的類別和介面的直接基底類別 ([介面](interfaces.md)) 直接由類別實作。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-178">A class declaration may include a *class_base* specification, which defines the direct base class of the class and the interfaces ([Interfaces](interfaces.md)) directly implemented by the class.</span></span>

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

<span data-ttu-id="6fe1a-179">類別宣告中指定的基底類別可以是建構的類別類型 ([建構類型](types.md#constructed-types))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-179">The base class specified in a class declaration can be a constructed class type ([Constructed types](types.md#constructed-types)).</span></span> <span data-ttu-id="6fe1a-180">基底類別不能，型別參數，但它可以包含在範圍中的型別參數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-180">A base class cannot be a type parameter on its own, though it can involve the type parameters that are in scope.</span></span>

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a><span data-ttu-id="6fe1a-181">基底類別</span><span class="sxs-lookup"><span data-stu-id="6fe1a-181">Base classes</span></span>

<span data-ttu-id="6fe1a-182">當*class_type*納入*class_base*，它會指定所要宣告類別的直接基底類別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-182">When a *class_type* is included in the *class_base*, it specifies the direct base class of the class being declared.</span></span> <span data-ttu-id="6fe1a-183">如果類別宣告不含任何*class_base*，或如果*class_base*清單只有介面類型、 直接的基底類別會假設為`object`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-183">If a class declaration has no *class_base*, or if the *class_base* lists only interface types, the direct base class is assumed to be `object`.</span></span> <span data-ttu-id="6fe1a-184">類別會繼承自其直接基底類別成員中所述[繼承](classes.md#inheritance)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-184">A class inherits members from its direct base class, as described in [Inheritance](classes.md#inheritance).</span></span>

<span data-ttu-id="6fe1a-185">在範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-185">In the example</span></span>
```csharp
class A {}

class B: A {}
```
<span data-ttu-id="6fe1a-186">類別`A`要直接基底類別`B`，並`B`即為衍生自`A`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-186">class `A` is said to be the direct base class of `B`, and `B` is said to be derived from `A`.</span></span> <span data-ttu-id="6fe1a-187">由於`A`沒有未明確指定 直接基底類別，其直接基底類別會以隱含方式`object`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-187">Since `A` does not explicitly specify a direct base class, its direct base class is implicitly `object`.</span></span>

<span data-ttu-id="6fe1a-188">建構的類別類型，如果在泛型類別宣告中，指定基底類別建構類型的基底類別是由取代來取得，每個*type_parameter*在基底類別宣告中，對應*type_argument*建構的類型。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-188">For a constructed class type, if a base class is specified in the generic class declaration, the base class of the constructed type is obtained by substituting, for each *type_parameter* in the base class declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="6fe1a-189">指定泛型類別宣告</span><span class="sxs-lookup"><span data-stu-id="6fe1a-189">Given the generic class declarations</span></span>
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
<span data-ttu-id="6fe1a-190">建構類型的基底類別`G<int>`會是`B<string,int[]>`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-190">the base class of the constructed type `G<int>` would be `B<string,int[]>`.</span></span>

<span data-ttu-id="6fe1a-191">類別類型的直接基底類別必須至少像一樣地存取自己的類別型別 ([存取範圍定義域](basic-concepts.md#accessibility-domains))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-191">The direct base class of a class type must be at least as accessible as the class type itself ([Accessibility domains](basic-concepts.md#accessibility-domains)).</span></span> <span data-ttu-id="6fe1a-192">比方說，它是編譯時期錯誤`public`類別來衍生自`private`或`internal`類別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-192">For example, it is a compile-time error for a `public` class to derive from a `private` or `internal` class.</span></span>

<span data-ttu-id="6fe1a-193">類別類型的直接基底類別不能任何下列類型： `System.Array`， `System.Delegate`， `System.MulticastDelegate`， `System.Enum`，或`System.ValueType`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-193">The direct base class of a class type must not be any of the following types: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, or `System.ValueType`.</span></span> <span data-ttu-id="6fe1a-194">此外，無法使用泛型類別宣告`System.Attribute`為直接或間接基底類別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-194">Furthermore, a generic class declaration cannot use `System.Attribute` as a direct or indirect base class.</span></span>

<span data-ttu-id="6fe1a-195">在決定直接基底類別規格的意義`A`類別的`B`，直接基底類別`B`暫時會假設為`object`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-195">While determining the meaning of the direct base class specification `A` of a class `B`, the direct base class of `B` is temporarily assumed to be `object`.</span></span> <span data-ttu-id="6fe1a-196">這確保基底類別規格的意義不能以遞迴方式的直覺的方式取決於本身。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-196">Intuitively this ensures that the meaning of a base class specification cannot recursively depend on itself.</span></span> <span data-ttu-id="6fe1a-197">範例：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-197">The example:</span></span>
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
<span data-ttu-id="6fe1a-198">處於錯誤，因為在基底類別規格`A<C.B>`直接基底類別`C`會被視為`object`，，因此 (的規則所[命名空間和型別名稱](basic-concepts.md#namespace-and-type-names))`C`不會被視為有一個成員`B`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-198">is in error since in the base class specification `A<C.B>` the direct base class of `C` is considered to be `object`, and hence (by the rules of [Namespace and type names](basic-concepts.md#namespace-and-type-names))  `C` is not considered to have a member `B`.</span></span>

<span data-ttu-id="6fe1a-199">類別類型的基底類別是直接基底類別和其基底類別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-199">The base classes of a class type are the direct base class and its base classes.</span></span> <span data-ttu-id="6fe1a-200">換句話說，一組基底類別是直接基底類別的關聯性可轉移關閉。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-200">In other words, the set of base classes is the transitive closure of the direct base class relationship.</span></span> <span data-ttu-id="6fe1a-201">指的上述基底類別的範例`B`都`A`和`object`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-201">Referring to the example above, the base classes of `B` are `A` and `object`.</span></span> <span data-ttu-id="6fe1a-202">在範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-202">In the example</span></span>
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
<span data-ttu-id="6fe1a-203">基底類別`D<int>`都`C<int[]>`， `B<IComparable<int[]>>`， `A`，和`object`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-203">the base classes of `D<int>` are `C<int[]>`, `B<IComparable<int[]>>`, `A`, and `object`.</span></span>

<span data-ttu-id="6fe1a-204">除了類別`object`，每個類別類型有一個直接的基底類別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-204">Except for class `object`, every class type has exactly one direct base class.</span></span> <span data-ttu-id="6fe1a-205">`object`類別沒有直接的基底類別，而且所有其他類別的 ultimate 基底類別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-205">The `object` class has no direct base class and is the ultimate base class of all other classes.</span></span>

<span data-ttu-id="6fe1a-206">當類別`B`衍生自類別`A`，它是編譯時期錯誤`A`取決於`B`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-206">When a class `B` derives from a class `A`, it is a compile-time error for `A` to depend on `B`.</span></span> <span data-ttu-id="6fe1a-207">類別***直接相依於***其直接基底類別 （如果有的話） 和***直接相依於***所在它巢狀的類別 （如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-207">A class ***directly depends on*** its direct base class (if any) and ***directly depends on*** the class within which it is immediately nested (if any).</span></span> <span data-ttu-id="6fe1a-208">指定此定義，一組完整的類別所相依的類別是的自反和轉移結束***直接相依於***關聯性。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-208">Given this definition, the complete set of classes upon which a class depends is the reflexive and transitive closure of the ***directly depends on*** relationship.</span></span>

<span data-ttu-id="6fe1a-209">此範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-209">The example</span></span>
```csharp
class A: A {}
```
<span data-ttu-id="6fe1a-210">因為類別取決於本身，則為錯誤。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-210">is erroneous because the class depends on itself.</span></span> <span data-ttu-id="6fe1a-211">同樣地，範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-211">Likewise, the example</span></span>
```csharp
class A: B {}
class B: C {}
class C: A {}
```
<span data-ttu-id="6fe1a-212">因為類別循環相依在本身是錯誤。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-212">is in error because the classes circularly depend on themselves.</span></span> <span data-ttu-id="6fe1a-213">最後，範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-213">Finally, the example</span></span>
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
<span data-ttu-id="6fe1a-214">導致編譯時期錯誤，因為`A`而定`B.C`（其直接基底類別），這取決於`B`（其立即封入類別），循環相依於`A`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-214">results in a compile-time error because `A` depends on `B.C` (its direct base class), which depends on `B` (its immediately enclosing class), which circularly depends on `A`.</span></span>

<span data-ttu-id="6fe1a-215">請注意，類別不會依賴巢狀類別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-215">Note that a class does not depend on the classes that are nested within it.</span></span> <span data-ttu-id="6fe1a-216">在範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-216">In the example</span></span>
```csharp
class A
{
    class B: A {}
}
```
<span data-ttu-id="6fe1a-217">`B` 取決於`A`(因為`A`同時為其直接基底類別和其立即封入類別)，但`A`不需依賴`B`(因為`B`不是基底類別或封入類別的`A`).</span><span class="sxs-lookup"><span data-stu-id="6fe1a-217">`B` depends on `A` (because `A` is both its direct base class and its immediately enclosing class), but `A` does not depend on `B` (since `B` is neither a base class nor an enclosing class of `A`).</span></span> <span data-ttu-id="6fe1a-218">因此，此範例是有效的。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-218">Thus, the example is valid.</span></span>

<span data-ttu-id="6fe1a-219">不可能衍生自`sealed`類別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-219">It is not possible to derive from a `sealed` class.</span></span> <span data-ttu-id="6fe1a-220">在範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-220">In the example</span></span>
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
<span data-ttu-id="6fe1a-221">類別`B`處於錯誤，因為它會嘗試將衍生自`sealed`類別`A`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-221">class `B` is in error because it attempts to derive from the `sealed` class `A`.</span></span>

#### <a name="interface-implementations"></a><span data-ttu-id="6fe1a-222">介面實作</span><span class="sxs-lookup"><span data-stu-id="6fe1a-222">Interface implementations</span></span>

<span data-ttu-id="6fe1a-223">A *class_base*規格可能包含一份介面類型，在其中情況下，類別即直接實作特定的介面型別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-223">A *class_base* specification may include a list of interface types, in which case the class is said to directly implement the given interface types.</span></span> <span data-ttu-id="6fe1a-224">介面實作討論中進一步[介面實作](interfaces.md#interface-implementations)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-224">Interface implementations are discussed further in [Interface implementations](interfaces.md#interface-implementations).</span></span>

### <a name="type-parameter-constraints"></a><span data-ttu-id="6fe1a-225">類型參數條件約束</span><span class="sxs-lookup"><span data-stu-id="6fe1a-225">Type parameter constraints</span></span>

<span data-ttu-id="6fe1a-226">泛型型別和方法的宣告可以選擇性地指定類型參數條件約束，藉以*type_parameter_constraints_clause*s。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-226">Generic type and method declarations can optionally specify type parameter constraints by including *type_parameter_constraints_clause*s.</span></span>

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

<span data-ttu-id="6fe1a-227">每個*type_parameter_constraints_clause*組成權杖`where`，後面接著型別參數的名稱，後面接著冒號和該型別參數條件約束的清單。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-227">Each *type_parameter_constraints_clause* consists of the token `where`, followed by the name of a type parameter, followed by a colon and the list of constraints for that type parameter.</span></span> <span data-ttu-id="6fe1a-228">有最多可達一`where`子句，每個類型參數，而`where`子句可以依照任意順序列出。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-228">There can be at most one `where` clause for each type parameter, and the `where` clauses can be listed in any order.</span></span> <span data-ttu-id="6fe1a-229">像是`get`並`set`屬性存取子中的語彙基元`where`語彙基元不是關鍵字。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-229">Like the `get` and `set` tokens in a property accessor, the `where` token is not a keyword.</span></span>

<span data-ttu-id="6fe1a-230">指定的條件約束清單`where`子句可以包含任何下列的元件，依此順序： 單一主要的條件約束、 一或多個次要條件約束，以及建構函式條件約束`new()`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-230">The list of constraints given in a `where` clause can include any of the following components, in this order: a single primary constraint, one or more secondary constraints, and the constructor constraint, `new()`.</span></span>

<span data-ttu-id="6fe1a-231">主要的條件約束可以是類別類型或***參考類型條件約束***`class`或***值類型條件約束*** `struct`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-231">A primary constraint can be a class type or the ***reference type constraint*** `class` or the ***value type constraint*** `struct`.</span></span> <span data-ttu-id="6fe1a-232">第二個條件約束*type_parameter*或是*interface_type*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-232">A secondary constraint can be a *type_parameter* or *interface_type*.</span></span>

<span data-ttu-id="6fe1a-233">參考類型條件約束指定型別參數使用的類型引數必須是參考型別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-233">The reference type constraint specifies that a type argument used for the type parameter must be a reference type.</span></span> <span data-ttu-id="6fe1a-234">所有類別類型、 介面類型、 委派類型、 陣列型別，以及已知為參考型別 （如有定義如下） 的型別參數都符合此條件約束。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-234">All class types, interface types, delegate types, array types, and type parameters known to be a reference type (as defined below) satisfy this constraint.</span></span>

<span data-ttu-id="6fe1a-235">實值類型條件約束指定型別參數使用的類型引數必須是不可為 null 的實值型別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-235">The value type constraint specifies that a type argument used for the type parameter must be a non-nullable value type.</span></span> <span data-ttu-id="6fe1a-236">所有不可為 null 的結構類型、 列舉型別和實值類型條件約束的型別參數符合此條件約束。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-236">All non-nullable struct types, enum types, and type parameters having the value type constraint satisfy this constraint.</span></span> <span data-ttu-id="6fe1a-237">請注意，雖然歸類為實值型別，可為 null 的型別 ([可為 Null 的型別](types.md#nullable-types)) 不符合的值類型條件約束。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-237">Note that although classified as a value type, a nullable type ([Nullable types](types.md#nullable-types)) does not satisfy the value type constraint.</span></span> <span data-ttu-id="6fe1a-238">也不能有具有實值類型條件約束的型別參數*constructor_constraint*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-238">A type parameter having the value type constraint cannot also have the *constructor_constraint*.</span></span>

<span data-ttu-id="6fe1a-239">指標類型永遠不允許為型別引數，而且不會被視為符合其中一個的參考類型或值類型條件約束。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-239">Pointer types are never allowed to be type arguments and are not considered to satisfy either the reference type or value type constraints.</span></span>

<span data-ttu-id="6fe1a-240">如果類別型別、 介面型別或型別參數條件約束，該型別指定最小 「 基底類型 」 必須支援該類型參數所使用的每個類型引數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-240">If a constraint is a class type, an interface type, or a type parameter, that type specifies a minimal "base type" that every type argument used for that type parameter must support.</span></span> <span data-ttu-id="6fe1a-241">每次使用建構的類型或泛型方法時，針對在編譯時期型別參數的條件約束檢查的類型引數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-241">Whenever a constructed type or generic method is used, the type argument is checked against the constraints on the type parameter at compile-time.</span></span> <span data-ttu-id="6fe1a-242">提供的型別引數必須滿足的條件中所述[滿足條件約束](types.md#satisfying-constraints)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-242">The type argument supplied must satisfy the conditions described in [Satisfying constraints](types.md#satisfying-constraints).</span></span>

<span data-ttu-id="6fe1a-243">A *class_type*條件約束必須滿足下列規則：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-243">A *class_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="6fe1a-244">類型必須是類別類型。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-244">The type must be a class type.</span></span>
*  <span data-ttu-id="6fe1a-245">型別不能`sealed`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-245">The type must not be `sealed`.</span></span>
*  <span data-ttu-id="6fe1a-246">類型必須不是下列類型之一： `System.Array`， `System.Delegate`， `System.Enum`，或`System.ValueType`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-246">The type must not be one of the following types: `System.Array`, `System.Delegate`, `System.Enum`, or `System.ValueType`.</span></span>
*  <span data-ttu-id="6fe1a-247">型別不能`object`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-247">The type must not be `object`.</span></span> <span data-ttu-id="6fe1a-248">因為所有的型別衍生自`object`，如果允許這類條件約束會有任何作用。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-248">Because all types derive from `object`, such a constraint would have no effect if it were permitted.</span></span>
*  <span data-ttu-id="6fe1a-249">最多一個條件約束指定型別參數可以是類別類型。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-249">At most one constraint for a given type parameter can be a class type.</span></span>

<span data-ttu-id="6fe1a-250">為指定的型別*interface_type*條件約束必須滿足下列規則：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-250">A type specified as an *interface_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="6fe1a-251">類型必須是介面類型。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-251">The type must be an interface type.</span></span>
*  <span data-ttu-id="6fe1a-252">型別必須不超過一次指定在給定`where`子句。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-252">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="6fe1a-253">在任一情況下，可以包含任何相關聯的類型或方法宣告做為建構的類型，一部分的型別參數條件約束，而且可能是正在宣告的型別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-253">In either case, the constraint can involve any of the type parameters of the associated type or method declaration as part of a constructed type, and can involve the type being declared.</span></span>

<span data-ttu-id="6fe1a-254">指定為型別參數條件約束必須是至少一樣地存取任何類別或介面類型 ([協助工具的條件約束](basic-concepts.md#accessibility-constraints)) 做為泛型類型或方法已宣告。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-254">Any class or interface type specified as a type parameter constraint must be at least as accessible ([Accessibility constraints](basic-concepts.md#accessibility-constraints)) as the generic type or method being declared.</span></span>

<span data-ttu-id="6fe1a-255">為指定的型別*type_parameter*條件約束必須滿足下列規則：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-255">A type specified as a *type_parameter* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="6fe1a-256">類型必須是型別參數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-256">The type must be a type parameter.</span></span>
*  <span data-ttu-id="6fe1a-257">型別必須不超過一次指定在給定`where`子句。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-257">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="6fe1a-258">此外中必須有任何循環相依性圖形的型別參數，其中的相依性是以可轉移的關聯所定義：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-258">In addition there must be no cycles in the dependency graph of type parameters, where dependency is a transitive relation defined by:</span></span>

*  <span data-ttu-id="6fe1a-259">如果型別參數`T`用作型別參數條件約束`S`再`S`***取決於*** `T`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-259">If a type parameter `T` is used as a constraint for type parameter `S` then `S` ***depends on*** `T`.</span></span>
*  <span data-ttu-id="6fe1a-260">如果型別參數`S`型別參數而定`T`並`T`取決於型別參數`U`然後`S`***取決於*** `U`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-260">If a type parameter `S` depends on a type parameter `T` and `T` depends on a type parameter `U` then `S` ***depends on*** `U`.</span></span>

<span data-ttu-id="6fe1a-261">指定這個關聯性，就 （直接或間接） 相依於本身的類型參數的編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-261">Given this relation, it is a compile-time error for a type parameter to depend on itself (directly or indirectly).</span></span>

<span data-ttu-id="6fe1a-262">任何條件約束必須是相依類型參數一致。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-262">Any constraints must be consistent among dependent type parameters.</span></span> <span data-ttu-id="6fe1a-263">如果型別參數`S`取決於型別參數`T`然後：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-263">If type parameter `S` depends on type parameter `T` then:</span></span>

*  <span data-ttu-id="6fe1a-264">`T` 不能有實值類型條件約束。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-264">`T` must not have the value type constraint.</span></span> <span data-ttu-id="6fe1a-265">否則，請`T`實際上會密封，因此`S`會強制讓相同的型別`T`，不必將兩個類型參數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-265">Otherwise, `T` is effectively sealed so `S` would be forced to be the same type as `T`, eliminating the need for two type parameters.</span></span>
*  <span data-ttu-id="6fe1a-266">如果`S`有值類型條件約束，然後`T`不能*class_type*條件約束。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-266">If `S` has the value type constraint then `T` must not have a *class_type* constraint.</span></span>
*  <span data-ttu-id="6fe1a-267">如果`S`已經*class_type*條件約束`A`並`T`具有*class_type*條件約束`B`則必須是身分識別轉換或隱含參考轉換`A`要`B`或從隱含參考轉換`B`至`A`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-267">If `S` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>
*  <span data-ttu-id="6fe1a-268">如果`S`也取決於型別參數`U`並`U`已*class_type*條件約束`A`並`T`具有*class_type*條件約束`B`則必須是識別轉換或從隱含參考轉換`A`要`B`或從隱含參考轉換`B`到`A`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-268">If `S` also depends on type parameter `U` and `U` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>

<span data-ttu-id="6fe1a-269">它是適用於`S`有實值類型條件約束和`T`有參考類型條件約束。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-269">It is valid for `S` to have the value type constraint and `T` to have the reference type constraint.</span></span> <span data-ttu-id="6fe1a-270">這會限制實際上`T`的型別`System.Object`， `System.ValueType`， `System.Enum`，以及任何介面類型。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-270">Effectively this limits `T` to the types `System.Object`, `System.ValueType`, `System.Enum`, and any interface type.</span></span>

<span data-ttu-id="6fe1a-271">如果`where`型別參數的子句會包含建構函式條件約束 (其中包含表單`new()`)，就可以使用`new`運算子來建立型別的執行個體 ([物件建立運算式](expressions.md#object-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="6fe1a-271">If the `where` clause for a type parameter includes a constructor constraint (which has the form `new()`), it is possible to use the `new` operator to create instances of the type ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span> <span data-ttu-id="6fe1a-272">任何類型使用的引數的建構函式條件約束的型別參數必須有公用無參數建構函式 （這個建構函式會隱含地存在任何實值類型） 或類型參數各有實值類型條件約束或建構函式條件約束 （請參閱[類型參數條件約束](classes.md#type-parameter-constraints)如需詳細資訊)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-272">Any type argument used for a type parameter with a constructor constraint must have a public parameterless constructor (this constructor implicitly exists for any value type) or be a type parameter having the value type constraint or constructor constraint (see [Type parameter constraints](classes.md#type-parameter-constraints) for details).</span></span>

<span data-ttu-id="6fe1a-273">條件約束的範例如下：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-273">The following are examples of constraints:</span></span>
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

<span data-ttu-id="6fe1a-274">下列範例會將發生錯誤，因為它造成循環相依性圖形的型別參數：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-274">The following example is in error because it causes a circularity in the dependency graph of the type parameters:</span></span>
```csharp
class Circular<S,T>
    where S: T
    where T: S                // Error, circularity in dependency graph
{
    ...
}
```

<span data-ttu-id="6fe1a-275">下列範例會說明其他無效的情況：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-275">The following examples illustrate additional invalid situations:</span></span>
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

<span data-ttu-id="6fe1a-276">***有效基底類別***型別參數的`T`定義如下：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-276">The ***effective base class*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="6fe1a-277">如果`T`沒有主要的條件約束或類型參數條件約束，其有效的基底類別是`object`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-277">If `T` has no primary constraints or type parameter constraints, its effective base class is `object`.</span></span>
*  <span data-ttu-id="6fe1a-278">如果`T`具有值的類型條件約束，其有效的基底類別是`System.ValueType`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-278">If `T` has the value type constraint, its effective base class is `System.ValueType`.</span></span>
*  <span data-ttu-id="6fe1a-279">如果`T`已經*class_type*條件約束`C`但沒有*type_parameter*條件約束，其有效的基底類別是`C`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-279">If `T` has a *class_type* constraint `C` but no *type_parameter* constraints, its effective base class is `C`.</span></span>
*  <span data-ttu-id="6fe1a-280">如果`T`沒有任何*class_type*條件約束，但有一或多個*type_parameter*條件約束，其有效的基底類別是最置於包含的型別 ([提昇的轉換運算子](conversions.md#lifted-conversion-operators)) 中的一組的有效基底類別其*type_parameter*條件約束。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-280">If `T` has no *class_type* constraint but has one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set of effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="6fe1a-281">一致性規則確保這類最置於包含的類型存在。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-281">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="6fe1a-282">如果`T`同時具有*class_type*條件約束和一或多個*type_parameter*條件約束，其有效的基底類別是最置於包含的型別 ([提昇的轉換運算子](conversions.md#lifted-conversion-operators)) 所組成的集中*class_type*條件約束`T`和有效的基底類別的其*type_parameter*條件約束。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-282">If `T` has both a *class_type* constraint and one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set consisting of the *class_type* constraint of `T` and the effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="6fe1a-283">一致性規則確保這類最置於包含的類型存在。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-283">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="6fe1a-284">如果`T`具有參考類型條件約束，但沒有*class_type*條件約束，其有效的基底類別是`object`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-284">If `T` has the reference type constraint but no *class_type* constraints, its effective base class is `object`.</span></span>

<span data-ttu-id="6fe1a-285">為了這些規則，如果 T 具有條件約束`V`也就是說*value_type*，改為使用最特定的基底類型的`V`也就是說*class_type*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-285">For the purpose of these rules, if T has a constraint `V` that is a *value_type*, use instead the most specific base type of `V` that is a *class_type*.</span></span> <span data-ttu-id="6fe1a-286">這可以永遠不會發生在明確指定的條件約束，但可能會發生時的條件約束的泛型方法會隱含地繼承覆寫的方法宣告或明確實作介面方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-286">This can never happen in an explicitly given constraint, but may occur when the constraints of a generic method are implicitly inherited by an overriding method declaration or an explicit implementation of an interface method.</span></span>

<span data-ttu-id="6fe1a-287">這些規則確保有效的基底類別永遠*class_type*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-287">These rules ensure that the effective base class is always a *class_type*.</span></span>

<span data-ttu-id="6fe1a-288">***有效的介面組***型別參數的`T`定義如下：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-288">The ***effective interface set*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="6fe1a-289">如果`T`沒有任何*secondary_constraints*，其有效的介面集是空的。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-289">If `T` has no *secondary_constraints*, its effective interface set is empty.</span></span>
*  <span data-ttu-id="6fe1a-290">如果`T`已經*interface_type*條件約束，但沒有*type_parameter*條件約束，其有效的介面集是它的一組*interface_type*條件約束。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-290">If `T` has *interface_type* constraints but no *type_parameter* constraints, its effective interface set is its set of *interface_type* constraints.</span></span>
*  <span data-ttu-id="6fe1a-291">如果`T`沒有任何*interface_type*條件約束但具有*type_parameter*條件約束，其有效的介面組是有效的介面集合的聯集其*type_參數*條件約束。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-291">If `T` has no *interface_type* constraints but has *type_parameter* constraints, its effective interface set is the union of the effective interface sets of its *type_parameter* constraints.</span></span>
*  <span data-ttu-id="6fe1a-292">如果`T`同時具有*interface_type*條件約束並*type_parameter*條件約束，其有效的介面集是它的一組的聯集*interface_type*條件約束和有效的介面組及其*type_parameter*條件約束。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-292">If `T` has both *interface_type* constraints and *type_parameter* constraints, its effective interface set is the union of its set of *interface_type* constraints and the effective interface sets of its *type_parameter* constraints.</span></span>

<span data-ttu-id="6fe1a-293">型別參數是***已知為參考型別***如果它具有參考類型條件約束，或不是有效的基底類別`object`或`System.ValueType`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-293">A type parameter is ***known to be a reference type*** if it has the reference type constraint or its effective base class is not `object` or `System.ValueType`.</span></span>

<span data-ttu-id="6fe1a-294">受條件約束的型別參數類型的值可用來存取隱含的條件約束的執行個體成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-294">Values of a constrained type parameter type can be used to access the instance members implied by the constraints.</span></span> <span data-ttu-id="6fe1a-295">在範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-295">In the example</span></span>
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
<span data-ttu-id="6fe1a-296">方法`IPrintable`可叫用直接`x`因為`T`一律實作受限於`IPrintable`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-296">the methods of `IPrintable` can be invoked directly on `x` because `T` is constrained to always implement `IPrintable`.</span></span>

### <a name="class-body"></a><span data-ttu-id="6fe1a-297">在類別主體</span><span class="sxs-lookup"><span data-stu-id="6fe1a-297">Class body</span></span>

<span data-ttu-id="6fe1a-298">*Class_body*類別會定義該類別的成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-298">The *class_body* of a class defines the members of that class.</span></span>

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a><span data-ttu-id="6fe1a-299">部分型別</span><span class="sxs-lookup"><span data-stu-id="6fe1a-299">Partial types</span></span>

<span data-ttu-id="6fe1a-300">型別宣告可跨多個分割***部分的型別宣告***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-300">A type declaration can be split across multiple ***partial type declarations***.</span></span> <span data-ttu-id="6fe1a-301">型別宣告是從建構其組件所遵循的規則，在此區段中，但它會被視為程式的編譯時期和執行階段處理期間的其餘部分的單一宣告。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-301">The type declaration is constructed from its parts by following the rules in this section, whereupon it is treated as a single declaration during the remainder of the compile-time and run-time processing of the program.</span></span>

<span data-ttu-id="6fe1a-302">A *class_declaration*， *struct_declaration*或是*interface_declaration*表示部分類型宣告，如果它包含`partial`修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-302">A *class_declaration*, *struct_declaration* or *interface_declaration* represents a partial type declaration if it includes a `partial` modifier.</span></span> <span data-ttu-id="6fe1a-303">`partial` 不能是關鍵字，以及如果它出現在其中一個關鍵字之前，只會做為修飾詞`class`，`struct`或是`interface`在類型宣告中，或在型別前面`void`方法宣告中。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-303">`partial` is not a keyword, and only acts as a modifier if it appears immediately before one of the keywords `class`, `struct` or `interface` in a type declaration, or before the type `void` in a method declaration.</span></span> <span data-ttu-id="6fe1a-304">在其他內容中可以當做它正常的識別項。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-304">In other contexts it can be used as a normal identifier.</span></span>

<span data-ttu-id="6fe1a-305">部分型別宣告的每個部分必須包含`partial`修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-305">Each part of a partial type declaration must include a `partial` modifier.</span></span> <span data-ttu-id="6fe1a-306">它必須具有相同的名稱，並在相同的命名空間或其他組件的型別宣告中宣告。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-306">It must have the same name  and be declared in the same namespace or type declaration as the other parts.</span></span> <span data-ttu-id="6fe1a-307">`partial`修飾詞表示的型別宣告的其他部分可能存在其他地方，但這類額外的組件是否存在，就不需要，它是適用於具有單一宣告的型別，以包含`partial`修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-307">The `partial` modifier indicates that additional parts of the type declaration may exist elsewhere, but the existence of such additional parts is not a requirement; it is valid for a type with a single declaration to include the `partial` modifier.</span></span>

<span data-ttu-id="6fe1a-308">使組件可以在編譯階段合併至單一類型宣告部分類型的所有組件必須一起編譯。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-308">All parts of a partial type must be compiled together such that the parts can be merged at compile-time into a single type declaration.</span></span> <span data-ttu-id="6fe1a-309">部分型別特別不允許已編譯的型別擴充。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-309">Partial types specifically do not allow already compiled types to be extended.</span></span>

<span data-ttu-id="6fe1a-310">巢狀型別可能會在多個部分宣告使用`partial`修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-310">Nested types may be declared in multiple parts by using the `partial` modifier.</span></span> <span data-ttu-id="6fe1a-311">一般而言，使用宣告包含型別`partial`自如，以及每個部分的巢狀型別是包含類型的不同組件中宣告。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-311">Typically, the containing type is declared using `partial` as well, and each part of the nested type is declared in a different part of the containing type.</span></span>

<span data-ttu-id="6fe1a-312">`partial`委派或列舉宣告上不允許修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-312">The `partial` modifier is not permitted on delegate or enum declarations.</span></span>

### <a name="attributes"></a><span data-ttu-id="6fe1a-313">屬性</span><span class="sxs-lookup"><span data-stu-id="6fe1a-313">Attributes</span></span>

<span data-ttu-id="6fe1a-314">在部分類型的屬性取決於未指定的順序，結合的各部分的屬性。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-314">The attributes of a partial type are determined by combining, in an unspecified order, the attributes of each of the parts.</span></span> <span data-ttu-id="6fe1a-315">如果屬性位於多個部分，它就相當於型別上多次指定屬性。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-315">If an attribute is placed on multiple parts, it is equivalent to specifying the attribute multiple times on the type.</span></span> <span data-ttu-id="6fe1a-316">例如，兩個部分：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-316">For example, the two parts:</span></span>

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
<span data-ttu-id="6fe1a-317">相當於宣告這類：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-317">are equivalent to a declaration such as:</span></span>
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

<span data-ttu-id="6fe1a-318">型別參數的屬性結合以類似的方式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-318">Attributes on type parameters combine in a similar fashion.</span></span>

### <a name="modifiers"></a><span data-ttu-id="6fe1a-319">修飾詞</span><span class="sxs-lookup"><span data-stu-id="6fe1a-319">Modifiers</span></span>

<span data-ttu-id="6fe1a-320">當部分的型別宣告包含協助工具規格 ( `public`， `protected`， `internal`，和`private`修飾詞) 它必須與所有其他部分，包括網頁可及性規格一致。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-320">When a partial type declaration includes an accessibility specification (the `public`, `protected`, `internal`, and `private` modifiers) it must agree with all other parts that include an accessibility specification.</span></span> <span data-ttu-id="6fe1a-321">如果在部分類型的任何部分不包含協助工具規格，為型別指定適當的預設存取範圍 ([宣告存取範圍](basic-concepts.md#declared-accessibility))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-321">If no part of a partial type includes an accessibility specification, the type is given the appropriate default accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="6fe1a-322">如果包含的巢狀類型的一或多個部分宣告`new`修飾詞，如果巢狀型別會隱藏繼承的成員，則會報告任何警告 ([經由繼承隱藏](basic-concepts.md#hiding-through-inheritance))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-322">If one or more partial declarations of a nested type include a `new` modifier, no warning is reported if the nested type hides an inherited member ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)).</span></span>

<span data-ttu-id="6fe1a-323">如果一或多個部分宣告的類別包含`abstract`修飾詞，此類別會被視為抽象 ([抽象類別](classes.md#abstract-classes))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-323">If one or more partial declarations of a class include an `abstract` modifier, the class is considered abstract ([Abstract classes](classes.md#abstract-classes)).</span></span> <span data-ttu-id="6fe1a-324">否則，該類別被視為非抽象。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-324">Otherwise, the class is considered non-abstract.</span></span>

<span data-ttu-id="6fe1a-325">如果一或多個部分宣告的類別包含`sealed`修飾詞，此類別會被視為密封 ([密封類別](classes.md#sealed-classes))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-325">If one or more partial declarations of a class include a `sealed` modifier, the class is considered sealed ([Sealed classes](classes.md#sealed-classes)).</span></span> <span data-ttu-id="6fe1a-326">否則，該類別就會視為未密封。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-326">Otherwise, the class is considered unsealed.</span></span>

<span data-ttu-id="6fe1a-327">請注意，類別不能同時為 abstract 和 sealed。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-327">Note that a class cannot be both abstract and sealed.</span></span>

<span data-ttu-id="6fe1a-328">當`unsafe`修飾詞用在部分類型宣告中，只有該特定的組件會被視為不安全的內容 ([Unsafe 內容](unsafe-code.md#unsafe-contexts))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-328">When the `unsafe` modifier is used on a partial type declaration, only that particular part is considered an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span>

### <a name="type-parameters-and-constraints"></a><span data-ttu-id="6fe1a-329">型別參數和條件約束</span><span class="sxs-lookup"><span data-stu-id="6fe1a-329">Type parameters and constraints</span></span>

<span data-ttu-id="6fe1a-330">如果多個組件中宣告為泛型型別，則每個組件必須狀態的型別參數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-330">If a generic type is declared in multiple parts, each part must state the type parameters.</span></span> <span data-ttu-id="6fe1a-331">每個組件必須有相同數目的型別參數，以及每個類型參數，相同的名稱順序。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-331">Each part must have the same number of type parameters, and the same name for each type parameter, in order.</span></span>

<span data-ttu-id="6fe1a-332">當部分的泛型型別宣告包含條件約束 (`where`子句)，條件約束必須與包含條件約束的所有其他部分一致。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-332">When a partial generic type declaration includes constraints (`where` clauses), the constraints must agree with all other parts that include constraints.</span></span> <span data-ttu-id="6fe1a-333">具體來說，每個部分都包含條件約束必須相同集合的型別參數的條件約束，而且每個類型參數的主要、 次要資料庫，且建構函式條件約束必須相等。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-333">Specifically, each part that includes constraints must have constraints for the same set of type parameters, and for each type parameter the sets of primary, secondary, and constructor constraints must be equivalent.</span></span> <span data-ttu-id="6fe1a-334">條件約束的兩個集合相等如果它們包含相同的成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-334">Two sets of constraints are equivalent if they contain the same members.</span></span> <span data-ttu-id="6fe1a-335">如果部分的泛型型別不屬於指定類型參數條件約束，參數會被視為未受限制的型別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-335">If no part of a partial generic type specifies type parameter constraints, the type parameters are considered unconstrained.</span></span>

<span data-ttu-id="6fe1a-336">此範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-336">The example</span></span>
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
<span data-ttu-id="6fe1a-337">完全正確，因為這些組件，有效地包含條件約束 （前兩個） 指定的相同設定的主要、 次要和型別參數，一組相同的建構函式條件約束分別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-337">is correct because those parts that include constraints (the first two) effectively specify the same set of primary, secondary, and constructor constraints for the same set of type parameters, respectively.</span></span>

### <a name="base-class"></a><span data-ttu-id="6fe1a-338">基底類別</span><span class="sxs-lookup"><span data-stu-id="6fe1a-338">Base class</span></span>

<span data-ttu-id="6fe1a-339">部分類別宣告包含基底類別規格時，它必須與包含指定的基底類別的所有其他部分保持一致。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-339">When a partial class declaration includes a base class specification it must agree with all other parts that include a base class specification.</span></span> <span data-ttu-id="6fe1a-340">如果部分類別的任何部分不包含基底類別規格，就會變成基底類別`System.Object`([基底類別](classes.md#base-classes))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-340">If no part of a partial class includes a base class specification, the base class becomes `System.Object` ([Base classes](classes.md#base-classes)).</span></span>

### <a name="base-interfaces"></a><span data-ttu-id="6fe1a-341">基底介面</span><span class="sxs-lookup"><span data-stu-id="6fe1a-341">Base interfaces</span></span>

<span data-ttu-id="6fe1a-342">在多個部分宣告類型的基底介面的集合是在每個組件上指定的基底介面的聯集。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-342">The set of base interfaces for a type declared in multiple parts is the union of the base interfaces specified on each part.</span></span> <span data-ttu-id="6fe1a-343">在特定的基底介面只可以命名為一次在每個部分，但它允許多個組件，以便名稱相同的基底介面。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-343">A particular base interface may only be named once on each part, but it is permitted for multiple parts to name the same base interface(s).</span></span> <span data-ttu-id="6fe1a-344">只能有一個實作，任何指定的基底介面的成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-344">There must only be one implementation of the members of any given base interface.</span></span>

<span data-ttu-id="6fe1a-345">在範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-345">In the example</span></span>
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
<span data-ttu-id="6fe1a-346">類別的基底介面的集合`C`已`IA`， `IB`，和`IC`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-346">the set of base interfaces for class `C` is `IA`, `IB`, and `IC`.</span></span>

<span data-ttu-id="6fe1a-347">通常，每個組件會提供一個在該部分; 宣告的介面實作不過，這不是需求。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-347">Typically, each part provides an implementation of the interface(s) declared on that part; however, this is not a requirement.</span></span> <span data-ttu-id="6fe1a-348">組件可能會提供在不同的部分宣告介面的實作：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-348">A part may provide the implementation for an interface declared on a different part:</span></span>
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

### <a name="members"></a><span data-ttu-id="6fe1a-349">成員</span><span class="sxs-lookup"><span data-stu-id="6fe1a-349">Members</span></span>

<span data-ttu-id="6fe1a-350">除了部分方法 ([部分方法](classes.md#partial-methods))，在多個部分宣告類型的成員就會直接在每個組件中宣告的成員集合的聯集。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-350">With the exception of partial methods ([Partial methods](classes.md#partial-methods)), the set of members of a type declared in multiple parts is simply the union of the set of members declared in each part.</span></span> <span data-ttu-id="6fe1a-351">型別宣告的所有部分的主體會共用相同的宣告空間 ([宣告](basic-concepts.md#declarations))，以及每個成員的範圍 ([範圍](basic-concepts.md#scopes)) 延伸至所有部分的內文。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-351">The bodies of all parts of the type declaration share the same declaration space ([Declarations](basic-concepts.md#declarations)), and the scope of each member ([Scopes](basic-concepts.md#scopes)) extends to the bodies of all the parts.</span></span> <span data-ttu-id="6fe1a-352">任一成員的存取範圍定義域一定會包含封入類型; 的所有組件`private`一個組件中宣告的成員可以自由存取另一個組件。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-352">The accessibility domain of any member always includes all the parts of the enclosing type; a `private` member declared in one part is freely accessible from another part.</span></span> <span data-ttu-id="6fe1a-353">它是宣告相同的成員類型的多個組件中的編譯時期錯誤，除非該成員是具有的型別`partial`修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-353">It is a compile-time error to declare the same member in more than one part of the type, unless that member is a type with the `partial` modifier.</span></span>

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

<span data-ttu-id="6fe1a-354">型別中的成員的排序是對 C# 程式碼，很少有重大影響，但配合其他語言及環境時可能會很大。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-354">The ordering of members within a type is rarely significant to C# code, but may be significant when interfacing with other languages and environments.</span></span> <span data-ttu-id="6fe1a-355">在這些情況下，在多個部分宣告的型別中的成員順序是未定義。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-355">In these cases, the ordering of members within a type declared in multiple parts is undefined.</span></span>

### <a name="partial-methods"></a><span data-ttu-id="6fe1a-356">部分方法</span><span class="sxs-lookup"><span data-stu-id="6fe1a-356">Partial methods</span></span>

<span data-ttu-id="6fe1a-357">部分方法可以定義的型別宣告某個部分，並在另一個實作。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-357">Partial methods can be defined in one part of a type declaration and implemented in another.</span></span> <span data-ttu-id="6fe1a-358">實作是選擇性的。如果沒有任何部分實作部分方法，與部分方法宣告和所有的呼叫，它會從組件的組合所產生的型別宣告移除。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-358">The implementation is optional; if no part implements the partial method, the partial method declaration and all calls to it are removed from the type declaration resulting from the combination of the parts.</span></span>

<span data-ttu-id="6fe1a-359">部分方法不能定義存取修飾詞，但是會以隱含方式`private`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-359">Partial methods cannot define access modifiers, but are implicitly `private`.</span></span> <span data-ttu-id="6fe1a-360">其傳回類型必須是`void`，而且它們的參數不能有`out`修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-360">Their return type must be `void`, and their parameters cannot have the `out` modifier.</span></span> <span data-ttu-id="6fe1a-361">識別項`partial`似乎之前時，才能夠辨識為特殊關鍵字方法宣告中`void`類型; 否則為一般識別項使用。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-361">The identifier `partial` is recognized as a special keyword in a method declaration only if it appears right before the `void` type; otherwise it can be used as a normal identifier.</span></span> <span data-ttu-id="6fe1a-362">部分方法無法明確實作介面方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-362">A partial method cannot explicitly implement interface methods.</span></span>

<span data-ttu-id="6fe1a-363">有兩種類型的部分方法宣告：如果方法宣告的主體是分號，宣告即為***定義部分方法宣告***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-363">There are two kinds of partial method declarations: If the body of the method declaration is a semicolon, the declaration is said to be a ***defining partial method declaration***.</span></span> <span data-ttu-id="6fe1a-364">如果主體指定為*區塊*，宣告要***實作部分方法宣告***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-364">If the body is given as a *block*, the declaration is said to be an ***implementing partial method declaration***.</span></span> <span data-ttu-id="6fe1a-365">各個型別宣告的組件可以有只有一個定義部分方法宣告，使用指定的簽章，而且可以有只有一個實作部分方法宣告，使用指定的簽章。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-365">Across the parts of a type declaration there can be only one defining partial method declaration with a given signature, and there can be only one implementing partial method declaration with a given signature.</span></span> <span data-ttu-id="6fe1a-366">如果有實作部分方法宣告，對應的定義部分方法宣告必須存在，而且宣告必須符合為指定下列：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-366">If an implementing partial method declaration is given, a corresponding defining partial method declaration must exist, and the declarations must match as specified in the following:</span></span>

* <span data-ttu-id="6fe1a-367">宣告必須有相同修飾詞 （雖然不一定是相同的順序），方法名稱、 型別參數數目和參數的數目。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-367">The declarations must have the same modifiers (although not necessarily in the same order), method name, number of type parameters and number of parameters.</span></span>
* <span data-ttu-id="6fe1a-368">對應的參數，在宣告中必須有相同修飾詞 （雖然不一定是在相同的順序） 和相同的型別 （模數型別參數名稱中的差異）。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-368">Corresponding parameters in the declarations must have the same modifiers (although not necessarily in the same order) and the same types (modulo differences in type parameter names).</span></span>
* <span data-ttu-id="6fe1a-369">在宣告中的對應型別參數必須有相同的條件約束 （模數型別參數名稱中的差異）。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-369">Corresponding type parameters in the declarations must have the same constraints (modulo differences in type parameter names).</span></span>

<span data-ttu-id="6fe1a-370">實作部分方法宣告可以出現在對應的定義部分方法宣告為相同的組件。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-370">An implementing partial method declaration can appear in the same part as the corresponding defining partial method declaration.</span></span>

<span data-ttu-id="6fe1a-371">只有一個定義的部分方法會參與多載解析。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-371">Only a defining partial method participates in overload resolution.</span></span> <span data-ttu-id="6fe1a-372">因此，不論是否指定實作的宣告，引動過程運算式可能會解析成部分方法的引動過程。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-372">Thus, whether or not an implementing declaration is given, invocation expressions may resolve to invocations of the partial method.</span></span> <span data-ttu-id="6fe1a-373">因為部分方法一律會傳回`void`，這類引動過程運算式一律會運算式陳述式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-373">Because a partial method always returns `void`, such invocation expressions will always be expression statements.</span></span> <span data-ttu-id="6fe1a-374">此外，因為部分方法會以隱含方式`private`，這類陳述式一定會發生在其中宣告部分方法的型別宣告的組件的其中一個內。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-374">Furthermore, because a partial method is implicitly `private`, such statements will always occur within one of the parts of the type declaration within which the partial method is declared.</span></span>

<span data-ttu-id="6fe1a-375">如果部分的型別宣告的任何部分不包含指定的部分方法的實作宣告，叫用它的任何運算式陳述式只會移除從組合的型別宣告。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-375">If no part of a partial type declaration contains an implementing declaration for a given partial method, any expression statement invoking it is simply removed from the combined type declaration.</span></span> <span data-ttu-id="6fe1a-376">因此引動過程運算式，包括任何構成的運算式，在執行階段沒有任何作用。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-376">Thus the invocation expression, including any constituent expressions, has no effect at run-time.</span></span> <span data-ttu-id="6fe1a-377">部分方法本身也會一併移除，並不會合併的類型宣告的成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-377">The partial method itself is also removed and will not be a member of the combined type declaration.</span></span>

<span data-ttu-id="6fe1a-378">如果指定的部分方法實作的宣告存在，則會保留部分方法的引動過程。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-378">If an implementing declaration exist for a given partial method, the invocations of the partial methods are retained.</span></span> <span data-ttu-id="6fe1a-379">部分方法會造成在方法宣告類似於實作的部分方法宣告，但下列除外：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-379">The partial method gives rise to a method declaration similar to the implementing partial method declaration except for the following:</span></span>

* <span data-ttu-id="6fe1a-380">`partial`修飾詞不包含</span><span class="sxs-lookup"><span data-stu-id="6fe1a-380">The `partial` modifier is not included</span></span>
* <span data-ttu-id="6fe1a-381">產生的方法宣告中的屬性會結合的屬性定義和實作部分方法宣告中未指定的順序。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-381">The attributes in the resulting method declaration are the combined attributes of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="6fe1a-382">不會移除重複項目。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-382">Duplicates are not removed.</span></span>
* <span data-ttu-id="6fe1a-383">產生的方法宣告的參數上的屬性會定義和實作部分方法宣告的對應參數結合的屬性中未指定的順序。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-383">The attributes on the parameters of the resulting method declaration are the combined attributes of the corresponding parameters of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="6fe1a-384">不會移除重複項目。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-384">Duplicates are not removed.</span></span>

<span data-ttu-id="6fe1a-385">如果定義宣告，但不是實作宣告為部分方法 M 指定，就會適用下列限制：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-385">If a defining declaration but not an implementing declaration is given for a partial method M, the following restrictions apply:</span></span>

* <span data-ttu-id="6fe1a-386">它是編譯時期錯誤建立方法的委派 ([委派建立運算式](expressions.md#delegate-creation-expressions))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-386">It is a compile-time error to create a delegate to method ([Delegate creation expressions](expressions.md#delegate-creation-expressions)).</span></span>
* <span data-ttu-id="6fe1a-387">它是編譯時期錯誤是指`M`內的匿名函式會轉換成運算式樹狀架構型別 ([匿名函式轉換成運算式樹狀架構類型的評估](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-387">It is a compile-time error to refer to `M` inside an anonymous function that is converted to an expression tree type ([Evaluation of anonymous function conversions to expression tree types](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).</span></span>
* <span data-ttu-id="6fe1a-388">運算式發生過程的引動過程`M`不會影響明確指派狀態 ([明確指派](variables.md#definite-assignment))，這可能會導致編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-388">Expressions occurring as part of an invocation of `M` do not affect the definite assignment state ([Definite assignment](variables.md#definite-assignment)), which can potentially lead to compile-time errors.</span></span>
* <span data-ttu-id="6fe1a-389">`M` 不能為應用程式的進入點 ([應用程式啟動](basic-concepts.md#application-startup))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-389">`M` cannot be the entry point for an application ([Application Startup](basic-concepts.md#application-startup)).</span></span>

<span data-ttu-id="6fe1a-390">部分方法可用於讓自訂行為的另一個組件，例如，一個由工具產生的型別宣告的一部分。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-390">Partial methods are useful for allowing one part of a type declaration to customize the behavior of another part, e.g., one that is generated by a tool.</span></span> <span data-ttu-id="6fe1a-391">請考慮下列的部分類別宣告：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-391">Consider the following partial class declaration:</span></span>
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

<span data-ttu-id="6fe1a-392">如果此類別編譯而不需要任何其他組件時，將會移除定義的部分方法宣告和其引動過程，和所產生之組合的類別宣告就相當於下列：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-392">If this class is compiled without any other parts, the defining partial method declarations and their invocations will be removed, and the resulting combined class declaration will be equivalent to the following:</span></span>
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

<span data-ttu-id="6fe1a-393">假設，另一個組件提供，不過，它提供的部分方法實作的宣告：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-393">Assume that another part is given, however, which provides implementing declarations of the partial methods:</span></span>
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

<span data-ttu-id="6fe1a-394">然後產生結合的類別宣告會相當於下列：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-394">Then the resulting combined class declaration will be equivalent to the following:</span></span>
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

### <a name="name-binding"></a><span data-ttu-id="6fe1a-395">名稱繫結</span><span class="sxs-lookup"><span data-stu-id="6fe1a-395">Name binding</span></span>

<span data-ttu-id="6fe1a-396">雖然每個部分的可延伸的型別必須相同的命名空間內宣告，組件通常是撰寫在不同的命名空間宣告中。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-396">Although each part of an extensible type must be declared within the same namespace, the parts are typically written within different namespace declarations.</span></span> <span data-ttu-id="6fe1a-397">因此，不同`using`指示詞 ([Using 指示詞](namespaces.md#using-directives)) 可能會存在於每個部分。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-397">Thus, different `using` directives ([Using directives](namespaces.md#using-directives)) may be present for each part.</span></span> <span data-ttu-id="6fe1a-398">當解譯的簡單名稱 ([型別推斷](expressions.md#type-inference)) 一組件內，只有`using`封入該部分之命名空間宣告的指示詞會被視為。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-398">When interpreting simple names ([Type inference](expressions.md#type-inference)) within one part, only the `using` directives of the namespace declaration(s) enclosing that part are considered.</span></span> <span data-ttu-id="6fe1a-399">這可能會導致相同的識別項不同的組件中具有不同的意義：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-399">This may result in the same identifier having different meanings in different parts:</span></span>
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

## <a name="class-members"></a><span data-ttu-id="6fe1a-400">類別成員</span><span class="sxs-lookup"><span data-stu-id="6fe1a-400">Class members</span></span>

<span data-ttu-id="6fe1a-401">類別的成員包含所引入的成員及其*class_member_declaration*從直接基底類別繼承的 s 和成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-401">The members of a class consist of the members introduced by its *class_member_declaration*s and the members inherited from the direct base class.</span></span>

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

<span data-ttu-id="6fe1a-402">類別類型的成員分成下列類別：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-402">The members of a class type are divided into the following categories:</span></span>

*  <span data-ttu-id="6fe1a-403">常數，表示與類別相關聯的常數值 ([常數](classes.md#constants))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-403">Constants, which represent constant values associated with the class ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="6fe1a-404">欄位，也就是類別的變數 ([欄位](classes.md#fields))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-404">Fields, which are the variables of the class ([Fields](classes.md#fields)).</span></span>
*  <span data-ttu-id="6fe1a-405">實作的計算和類別可以執行動作的方法 ([方法](classes.md#methods))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-405">Methods, which implement the computations and actions that can be performed by the class ([Methods](classes.md#methods)).</span></span>
*  <span data-ttu-id="6fe1a-406">屬性，定義名為特性以及讀取和寫入這些特性相關聯的動作 ([屬性](classes.md#properties))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-406">Properties, which define named characteristics and the actions associated with reading and writing those characteristics ([Properties](classes.md#properties)).</span></span>
*  <span data-ttu-id="6fe1a-407">定義可以類別所產生的通知的事件 ([事件](classes.md#events))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-407">Events, which define notifications that can be generated by the class ([Events](classes.md#events)).</span></span>
*  <span data-ttu-id="6fe1a-408">索引子，允許相同的方式編製索引類別的執行個體 （語法） 做為陣列 ([索引子](classes.md#indexers))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-408">Indexers, which permit instances of the class to be indexed in the same way (syntactically) as arrays ([Indexers](classes.md#indexers)).</span></span>
*  <span data-ttu-id="6fe1a-409">運算子定義可套用至類別的執行個體的運算式運算子 ([運算子](classes.md#operators))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-409">Operators, which define the expression operators that can be applied to instances of the class ([Operators](classes.md#operators)).</span></span>
*  <span data-ttu-id="6fe1a-410">執行個體建構函式，實作初始化類別的執行個體所需的動作 ([執行個體建構函式](classes.md#instance-constructors))</span><span class="sxs-lookup"><span data-stu-id="6fe1a-410">Instance constructors, which implement the actions required to initialize instances of the class ([Instance constructors](classes.md#instance-constructors))</span></span>
*  <span data-ttu-id="6fe1a-411">解構函式，實作會永久捨棄類別執行個體之前要執行的動作 ([解構函式](classes.md#destructors))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-411">Destructors, which implement the actions to be performed before instances of the class are permanently discarded ([Destructors](classes.md#destructors)).</span></span>
*  <span data-ttu-id="6fe1a-412">靜態建構函式，實作初始化類別本身所需的動作 ([靜態建構函式](classes.md#static-constructors))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-412">Static constructors, which implement the actions required to initialize the class itself ([Static constructors](classes.md#static-constructors)).</span></span>
*  <span data-ttu-id="6fe1a-413">類型，代表本機類別的型別 ([巢狀型別](classes.md#nested-types))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-413">Types, which represent the types that are local to the class ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="6fe1a-414">可以包含可執行程式碼的成員統稱為*函式成員*類別類型。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-414">Members that can contain executable code are collectively known as the *function members* of the class type.</span></span> <span data-ttu-id="6fe1a-415">函式成員的類別類型是方法、 屬性、 事件、 索引子、 運算子、 執行個體建構函式、 解構函式和該類別類型的靜態建構函式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-415">The function members of a class type are the methods, properties, events, indexers, operators, instance constructors,  destructors, and static constructors of that class type.</span></span>

<span data-ttu-id="6fe1a-416">A *class_declaration*建立新的宣告空間 ([宣告](basic-concepts.md#declarations))，而*class_member_declaration*立即所包含的 s*類別_declaration*這個宣告空間中引入新的成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-416">A *class_declaration* creates a new declaration space ([Declarations](basic-concepts.md#declarations)), and the *class_member_declaration*s immediately contained by the *class_declaration* introduce new members into this declaration space.</span></span> <span data-ttu-id="6fe1a-417">下列規則適用於*class_member_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="6fe1a-417">The following rules apply to *class_member_declaration*s:</span></span>

*  <span data-ttu-id="6fe1a-418">執行個體建構函式、 解構函式和靜態建構函式必須立即封入類別名稱相同。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-418">Instance constructors, destructors and static constructors must have the same name as the immediately enclosing class.</span></span> <span data-ttu-id="6fe1a-419">所有其他成員必須具有與立即封入類別名稱不同的名稱。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-419">All other members must have names that differ from the name of the immediately enclosing class.</span></span>
*  <span data-ttu-id="6fe1a-420">常數、 欄位、 屬性、 事件或類型的名稱必須與其他相同類別中宣告的所有成員的名稱不同。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-420">The name of a constant, field, property, event, or type must differ from the names of all other members declared in the same class.</span></span>
*  <span data-ttu-id="6fe1a-421">方法的名稱必須與所有其他非方法相同的類別中宣告的名稱不同。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-421">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="6fe1a-422">此外，簽章 ([簽章和多載](basic-concepts.md#signatures-and-overloading)) 的方法必須不同於在相同類別中，宣告的所有其他方法的簽章，在相同類別中宣告的兩種方法可能沒有完全不同的簽章藉由`ref`和`out`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-422">In addition, the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="6fe1a-423">執行個體建構函式的簽章必須不同於在相同類別中，宣告的所有其他執行個體建構函式的簽章，並在相同類別中宣告的兩個建構函式不能單獨由不同的簽章`ref`和`out`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-423">The signature of an instance constructor must differ from the signatures of all other instance constructors declared in the same class, and two constructors declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="6fe1a-424">索引子的簽章必須與在相同類別中宣告的其他所有索引子的簽章不同。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-424">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>
*  <span data-ttu-id="6fe1a-425">運算子的簽章必須與其他相同類別中宣告的所有運算子的簽章不同。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-425">The signature of an operator must differ from the signatures of all other operators declared in the same class.</span></span>

<span data-ttu-id="6fe1a-426">類別類型的繼承的成員 ([繼承](classes.md#inheritance)) 不是類別的宣告空間的一部分。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-426">The inherited members of a class type ([Inheritance](classes.md#inheritance)) are not part of the declaration space of a class.</span></span> <span data-ttu-id="6fe1a-427">因此，在衍生的類別可以宣告具有相同的名稱或簽章的繼承成員 （這實際上會隱藏繼承的成員）。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-427">Thus, a derived class is allowed to declare a member with the same name or signature as an inherited member (which in effect hides the inherited member).</span></span>

### <a name="the-instance-type"></a><span data-ttu-id="6fe1a-428">執行個體類型</span><span class="sxs-lookup"><span data-stu-id="6fe1a-428">The instance type</span></span>

<span data-ttu-id="6fe1a-429">每個類別宣告都有相關聯的繫結型別 ([繫結和解除繫結型別](types.md#bound-and-unbound-types))，則***執行個體類型***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-429">Each class declaration has an associated bound type ([Bound and unbound types](types.md#bound-and-unbound-types)), the ***instance type***.</span></span> <span data-ttu-id="6fe1a-430">對於泛型類別宣告，藉由建立建構的型別構成的執行個體類型 ([建構類型](types.md#constructed-types)) 型別宣告中，從與每個提供的型別所對應的引數型別參數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-430">For a generic class declaration, the instance type is formed by creating a constructed type ([Constructed types](types.md#constructed-types)) from the type declaration, with each of the supplied type arguments being the corresponding type parameter.</span></span> <span data-ttu-id="6fe1a-431">由於執行個體類型使用的型別參數，它僅適用於在範圍內; 所在的型別參數也就是類別宣告中。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-431">Since the instance type uses the type parameters, it can only be used where the type parameters are in scope; that is, inside the class declaration.</span></span> <span data-ttu-id="6fe1a-432">執行個體型別是種`this`類別宣告內撰寫的程式碼。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-432">The instance type is the type of `this` for code written inside the class declaration.</span></span> <span data-ttu-id="6fe1a-433">非泛型類別的執行個體類型會是只是宣告的類別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-433">For non-generic classes, the instance type is simply the declared class.</span></span> <span data-ttu-id="6fe1a-434">下列範例顯示數個類別宣告，以及其執行個體類型：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-434">The following shows several class declarations along with their instance types:</span></span> 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a><span data-ttu-id="6fe1a-435">建構類型的成員</span><span class="sxs-lookup"><span data-stu-id="6fe1a-435">Members of constructed types</span></span>

<span data-ttu-id="6fe1a-436">透過替代，每個所取得的是非繼承的成員建構的型別*type_parameter*在成員宣告中，對應*type_argument*建構的類型。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-436">The non-inherited members of a constructed type are obtained by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="6fe1a-437">替代程序為基礎的型別宣告語意意義，而且不只是文字的替代。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-437">The substitution process is based on the semantic meaning of type declarations, and is not simply textual substitution.</span></span>

<span data-ttu-id="6fe1a-438">例如，指定泛型類別宣告</span><span class="sxs-lookup"><span data-stu-id="6fe1a-438">For example, given the generic class declaration</span></span>
```csharp
class Gen<T,U>
{
    public T[,] a;
    public void G(int i, T t, Gen<U,T> gt) {...}
    public U Prop { get {...} set {...} }
    public int H(double d) {...}
}
```
<span data-ttu-id="6fe1a-439">建構的型別`Gen<int[],IComparable<string>>`具有下列成員：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-439">the constructed type `Gen<int[],IComparable<string>>` has the following members:</span></span>
```csharp
public int[,][] a;
public void G(int i, int[] t, Gen<IComparable<string>,int[]> gt) {...}
public IComparable<string> Prop { get {...} set {...} }
public int H(double d) {...}
```

<span data-ttu-id="6fe1a-440">成員的型別`a`泛型類別宣告中`Gen`是 「 二維陣列`T`"，因此成員的型別`a`上述建構的類型是 「 二維陣列的一維陣列`int`"，或`int[,][]`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-440">The type of the member `a` in the generic class declaration `Gen` is "two-dimensional array of `T`", so the type of the member `a` in the constructed type above is "two-dimensional array of one-dimensional array of `int`", or `int[,][]`.</span></span>

<span data-ttu-id="6fe1a-441">在執行個體函式成員的型別`this`的執行個體類型 ([執行個體類型](classes.md#the-instance-type)) 包含宣告。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-441">Within instance function members, the type of `this` is the instance type ([The instance type](classes.md#the-instance-type)) of the containing declaration.</span></span>

<span data-ttu-id="6fe1a-442">泛型類別的所有成員可以都使用從任何的封入類別的型別參數，直接或做為建構類型的一部分。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-442">All members of a generic class can use type parameters from any enclosing class, either directly or as part of a constructed type.</span></span> <span data-ttu-id="6fe1a-443">使用特定的關閉建構的型別 ([開放和封閉類型](types.md#open-and-closed-types)) 用在執行階段，每次使用的型別參數會取代實際的類型引數提供給建構類型。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-443">When a particular closed constructed type ([Open and closed types](types.md#open-and-closed-types)) is used at run-time, each use of a type parameter is replaced with the actual type argument supplied to the constructed type.</span></span> <span data-ttu-id="6fe1a-444">例如: </span><span class="sxs-lookup"><span data-stu-id="6fe1a-444">For example:</span></span>
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

### <a name="inheritance"></a><span data-ttu-id="6fe1a-445">繼承</span><span class="sxs-lookup"><span data-stu-id="6fe1a-445">Inheritance</span></span>

<span data-ttu-id="6fe1a-446">類別***繼承***其直接基底類別型別的成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-446">A class ***inherits*** the members of its direct base class type.</span></span> <span data-ttu-id="6fe1a-447">繼承意謂類別以隱含方式包含其直接基底類別型別，但執行個體建構函式、 解構函式和基底類別的靜態建構函式除外的所有成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-447">Inheritance means that a class implicitly contains all members of its direct base class type, except for the instance constructors, destructors and static constructors of the base class.</span></span> <span data-ttu-id="6fe1a-448">繼承的一些重要層面是：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-448">Some important aspects of inheritance are:</span></span>

*  <span data-ttu-id="6fe1a-449">繼承可以轉移。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-449">Inheritance is transitive.</span></span> <span data-ttu-id="6fe1a-450">如果`C`衍生自`B`，並`B`衍生自`A`，然後`C`繼承中宣告的成員`B`中所宣告的成員以及`A`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-450">If `C` is derived from `B`, and `B` is derived from `A`, then `C` inherits the members declared in `B` as well as the members declared in `A`.</span></span>
*  <span data-ttu-id="6fe1a-451">在衍生的類別會擴充其直接基底類別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-451">A derived class extends its direct base class.</span></span> <span data-ttu-id="6fe1a-452">衍生類別可以在其繼承的成員中新增新的成員，但無法移除所繼承成員的定義。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-452">A derived class can add new members to those it inherits, but it cannot remove the definition of an inherited member.</span></span>
*  <span data-ttu-id="6fe1a-453">不會繼承執行個體建構函式、 解構函式和靜態建構函式，但所有其他成員，不論其已宣告存取範圍 ([成員存取](basic-concepts.md#member-access))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-453">Instance constructors, destructors, and static constructors are not inherited, but all other members are, regardless of their declared accessibility ([Member access](basic-concepts.md#member-access)).</span></span> <span data-ttu-id="6fe1a-454">不過，根據其宣告的存取範圍，繼承的成員可能無法在衍生類別中存取。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-454">However, depending on their declared accessibility, inherited members might not be accessible in a derived class.</span></span>
*  <span data-ttu-id="6fe1a-455">在衍生的類別可以***隱藏***([經由繼承隱藏](basic-concepts.md#hiding-through-inheritance)) 藉由宣告具有相同的名稱或簽章的新成員的繼承成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-455">A derived class can ***hide*** ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)) inherited members by declaring new members with the same name or signature.</span></span> <span data-ttu-id="6fe1a-456">請注意，不過，隱藏繼承的成員不會移除該成員 — 它只會讓該成員無法存取直接透過衍生的類別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-456">Note however that hiding an inherited member does not remove that member—it merely makes that member inaccessible directly through the derived class.</span></span>
*  <span data-ttu-id="6fe1a-457">類別的執行個體包含一組宣告的類別和其基底類別，以及隱含轉換中的所有執行個體欄位 ([隱含參考轉換](conversions.md#implicit-reference-conversions)) 至其任何基底類別型別存在從衍生的類別的型別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-457">An instance of a class contains a set of all instance fields declared in the class and its base classes, and an implicit conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from a derived class type to any of its base class types.</span></span> <span data-ttu-id="6fe1a-458">因此，某些衍生類別的執行個體的參考可以視為任何其基底類別的執行個體的參考。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-458">Thus, a reference to an instance of some derived class can be treated as a reference to an instance of any of its base classes.</span></span>
*  <span data-ttu-id="6fe1a-459">類別可宣告虛擬方法、 屬性和索引子和衍生的類別可以覆寫這些函式成員的實作。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-459">A class can declare virtual methods, properties, and indexers, and derived classes can override the implementation of these function members.</span></span> <span data-ttu-id="6fe1a-460">這可讓類別來呈現多型行為，其中函式成員引動過程所執行的動作，將根據叫用該函式成員的執行個體的執行階段類型而有所差異。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-460">This enables classes to exhibit polymorphic behavior wherein the actions performed by a function member invocation varies depending on the run-time type of the instance through which that function member is invoked.</span></span>

<span data-ttu-id="6fe1a-461">建構的類別類型的繼承的成員是直接基底類別型別的成員 ([基底類別](classes.md#base-classes))，其位所得到的建構類型的類型引數對應型別的每個項目中的參數*class_base*規格。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-461">The inherited member of a constructed class type are the members of the immediate base class type ([Base classes](classes.md#base-classes)), which is found by substituting the type arguments of the constructed type for each occurrence of the corresponding type parameters in the *class_base* specification.</span></span> <span data-ttu-id="6fe1a-462">接著，這些成員，會轉換以替代，每個*type_parameter*在成員宣告中，對應*type_argument*的*class_base*規格。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-462">These members, in turn, are transformed by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the *class_base* specification.</span></span>

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

<span data-ttu-id="6fe1a-463">在上述範例中，建構的型別`D<int>`有一個非繼承的成員`public int G(string s)`取得所得到的類型引數`int`做為 type 參數`T`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-463">In the above example, the constructed type `D<int>` has a non-inherited member `public int G(string s)` obtained by substituting the type argument `int` for the type parameter `T`.</span></span> <span data-ttu-id="6fe1a-464">`D<int>` 也有繼承的成員，從類別宣告`B`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-464">`D<int>` also has an inherited member from the class declaration `B`.</span></span> <span data-ttu-id="6fe1a-465">此繼承的成員由前，先判斷在基底類別型別`B<int[]>`的`D<int>`替代`int`如`T`基底類別規格中`B<T[]>`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-465">This inherited member is determined by first determining the base class type `B<int[]>` of `D<int>` by substituting `int` for `T` in the base class specification `B<T[]>`.</span></span> <span data-ttu-id="6fe1a-466">然後，作為類型引數`B`，`int[]`用來替代`U`中`public U F(long index)`，產生繼承的成員`public int[] F(long index)`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-466">Then, as a type argument to `B`, `int[]` is substituted for `U` in `public U F(long index)`, yielding the inherited member `public int[] F(long index)`.</span></span>

### <a name="the-new-modifier"></a><span data-ttu-id="6fe1a-467">New 修飾詞</span><span class="sxs-lookup"><span data-stu-id="6fe1a-467">The new modifier</span></span>

<span data-ttu-id="6fe1a-468">A *class_member_declaration*允許宣告具有相同的名稱或簽章的成員，做為繼承的成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-468">A *class_member_declaration* is permitted to declare a member with the same name or signature as an inherited member.</span></span> <span data-ttu-id="6fe1a-469">當發生這種情況時，即稱為衍生的類別成員***隱藏***基底類別成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-469">When this occurs, the derived class member is said to ***hide*** the base class member.</span></span> <span data-ttu-id="6fe1a-470">隱藏繼承的成員不被視為錯誤，但會造成編譯器將發出警告。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-470">Hiding an inherited member is not considered an error, but it does cause the compiler to issue a warning.</span></span> <span data-ttu-id="6fe1a-471">若要隱藏警告，請在衍生的類別成員的宣告可以包含`new`修飾詞來表示衍生的成員要隱藏基底的成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-471">To suppress the warning, the declaration of the derived class member can include a `new` modifier to indicate that the derived member is intended to hide the base member.</span></span> <span data-ttu-id="6fe1a-472">本主題會討論中進一步[經由繼承隱藏](basic-concepts.md#hiding-through-inheritance)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-472">This topic is discussed further in [Hiding through inheritance](basic-concepts.md#hiding-through-inheritance).</span></span>

<span data-ttu-id="6fe1a-473">如果`new`修飾詞加入的宣告，也不會隱藏繼承的成員，該效果會發出警告。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-473">If a `new` modifier is included in a declaration that doesn't hide an inherited member, a warning to that effect is issued.</span></span> <span data-ttu-id="6fe1a-474">藉由移除隱藏這個警告`new`修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-474">This warning is suppressed by removing the `new` modifier.</span></span>

### <a name="access-modifiers"></a><span data-ttu-id="6fe1a-475">存取修飾詞</span><span class="sxs-lookup"><span data-stu-id="6fe1a-475">Access modifiers</span></span>

<span data-ttu-id="6fe1a-476">A *class_member_declaration*可以有五種可能的宣告存取任何的範圍一個 ([宣告存取範圍](basic-concepts.md#declared-accessibility)): `public`， `protected internal`， `protected`， `internal`或`private`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-476">A *class_member_declaration* can have any one of the five possible kinds of declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal`, or `private`.</span></span> <span data-ttu-id="6fe1a-477">除了`protected internal`組合，它會指定一個以上的存取修飾詞的編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-477">Except for the `protected internal` combination, it is a compile-time error to specify more than one access modifier.</span></span> <span data-ttu-id="6fe1a-478">當*class_member_declaration*不包含任何存取修飾詞，`private`假設。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-478">When a *class_member_declaration* does not include any access modifiers, `private` is assumed.</span></span>

### <a name="constituent-types"></a><span data-ttu-id="6fe1a-479">構成類型</span><span class="sxs-lookup"><span data-stu-id="6fe1a-479">Constituent types</span></span>

<span data-ttu-id="6fe1a-480">成員的宣告中所使用的類型稱為組成的型別，該成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-480">Types that are used in the declaration of a member are called the constituent types of that member.</span></span> <span data-ttu-id="6fe1a-481">可能構成類型是常數、 欄位、 屬性、 事件或索引子的型別、 方法或運算子的傳回型別和方法、 索引子、 運算子或執行個體建構函式的參數類型。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-481">Possible constituent types are the type of a constant, field, property, event, or indexer, the return type of a method or operator, and the parameter types of a method, indexer, operator, or instance constructor.</span></span> <span data-ttu-id="6fe1a-482">必須至少像一樣地存取該成員本身的成員組成的型別 ([協助工具的條件約束](basic-concepts.md#accessibility-constraints))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-482">The constituent types of a member must be at least as accessible as that member itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

### <a name="static-and-instance-members"></a><span data-ttu-id="6fe1a-483">靜態和執行個體的成員</span><span class="sxs-lookup"><span data-stu-id="6fe1a-483">Static and instance members</span></span>

<span data-ttu-id="6fe1a-484">類別的成員都是***靜態成員***或是***執行個體成員***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-484">Members of a class are either ***static members*** or ***instance members***.</span></span> <span data-ttu-id="6fe1a-485">一般而言，最好將靜態成員屬於類別型別和物件 （類別類型的執行個體） 的執行個體成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-485">Generally speaking, it is useful to think of static members as belonging to class types and instance members as belonging to objects (instances of class types).</span></span>

<span data-ttu-id="6fe1a-486">當欄位、 方法、 屬性、 事件、 運算子或建構函式的宣告包含`static`修飾詞，它會宣告為靜態成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-486">When a field, method, property, event, operator, or constructor declaration includes a `static` modifier, it declares a static member.</span></span> <span data-ttu-id="6fe1a-487">此外，常數或型別宣告隱含宣告的靜態成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-487">In addition, a constant or type declaration implicitly declares a static member.</span></span> <span data-ttu-id="6fe1a-488">靜態成員具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-488">Static members have the following characteristics:</span></span>

*  <span data-ttu-id="6fe1a-489">靜態成員時`M`簡稱*member_access* ([成員存取](expressions.md#member-access)) 形式的`E.M`，`E`必須表示型別，其中`M`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-489">When a static member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote a type containing `M`.</span></span> <span data-ttu-id="6fe1a-490">它是編譯時期錯誤`E`代表執行個體。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-490">It is a compile-time error for `E` to denote an instance.</span></span>
*  <span data-ttu-id="6fe1a-491">靜態欄位只能識別一個儲存體位置的特定已關閉的類別類型的所有執行個體共用。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-491">A static field identifies exactly one storage location to be shared by all instances of a given closed class type.</span></span> <span data-ttu-id="6fe1a-492">不論建立多少個特定的已關閉的類別類型執行個體，會只有一份靜態欄位。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-492">No matter how many instances of a given closed class type are created, there is only ever one copy of a static field.</span></span>
*  <span data-ttu-id="6fe1a-493">靜態函式成員 （方法、 屬性、 事件、 運算子或建構函式） 不能進行特定的執行個體，並參考編譯時期錯誤`this`這類函式成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-493">A static function member (method, property, event, operator, or constructor) does not operate on a specific instance, and it is a compile-time error to refer to `this` in such a function member.</span></span>

<span data-ttu-id="6fe1a-494">欄位、 方法、 屬性、 事件、 索引子、 建構函式或解構函式宣告不包括的當`static`修飾詞，它會宣告執行個體成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-494">When a field, method, property, event, indexer, constructor, or destructor declaration does not include a `static` modifier, it declares an instance member.</span></span> <span data-ttu-id="6fe1a-495">（執行個體成員有時也稱為非靜態成員。）執行個體成員具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-495">(An instance member is sometimes called a non-static member.) Instance members have the following characteristics:</span></span>

*  <span data-ttu-id="6fe1a-496">當執行個體成員`M`中參考*member_access* ([成員存取](expressions.md#member-access)) 形式的`E.M`，`E`必須代表包含型別的執行個體`M`.</span><span class="sxs-lookup"><span data-stu-id="6fe1a-496">When an instance member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote an instance of a type containing `M`.</span></span> <span data-ttu-id="6fe1a-497">它會繫結時間錯誤`E`代表型別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-497">It is a binding-time error for `E` to denote a type.</span></span>
*  <span data-ttu-id="6fe1a-498">每個類別執行個體包含一組個別的類別的所有執行個體欄位。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-498">Every instance of a class contains a separate set of all instance fields of the class.</span></span>
*  <span data-ttu-id="6fe1a-499">指定類別的執行個體作業 （方法、 屬性、 索引子、 執行個體建構函式或解構函式） 執行個體函式成員，而且可以當做這個執行個體`this`([這項存取](expressions.md#this-access))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-499">An instance function member (method, property, indexer, instance constructor, or destructor) operates on a given instance of the class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="6fe1a-500">下列範例說明的規則，來存取靜態和執行個體成員：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-500">The following example illustrates the rules for accessing static and instance members:</span></span>
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

<span data-ttu-id="6fe1a-501">`F`方法所示範的是，在執行個體函式成員*simple_name* ([簡單名稱](expressions.md#simple-names)) 可用來存取執行個體成員和靜態成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-501">The `F` method shows that in an instance function member, a *simple_name* ([Simple names](expressions.md#simple-names)) can be used to access both instance members and static members.</span></span> <span data-ttu-id="6fe1a-502">`G`方法顯示在靜態函式成員中，它是編譯時期錯誤，以存取透過執行個體成員*simple_name*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-502">The `G` method shows that in a static function member, it is a compile-time error to access an instance member through a *simple_name*.</span></span> <span data-ttu-id="6fe1a-503">`Main`方法會顯示在*member_access* ([成員存取](expressions.md#member-access))、 執行個體成員必須透過執行個體和靜態成員必須透過型別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-503">The `Main` method shows that in a *member_access* ([Member access](expressions.md#member-access)), instance members must be accessed through instances, and static members must be accessed through types.</span></span>

### <a name="nested-types"></a><span data-ttu-id="6fe1a-504">巢狀型別</span><span class="sxs-lookup"><span data-stu-id="6fe1a-504">Nested types</span></span>

<span data-ttu-id="6fe1a-505">在類別或結構宣告內宣告的型別稱為***巢狀類型***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-505">A type declared within a class or struct declaration is called a ***nested type***.</span></span> <span data-ttu-id="6fe1a-506">在編譯單位或命名空間中宣告的型別稱為***非巢狀型別***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-506">A type that is declared within a compilation unit or namespace is called a ***non-nested type***.</span></span>

<span data-ttu-id="6fe1a-507">在範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-507">In the example</span></span>
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
<span data-ttu-id="6fe1a-508">類別`B`是巢狀的類型，因為它在類別中宣告`A`，和類別`A`是非巢狀類型，因為它宣告為編譯單位中。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-508">class `B` is a nested type because it is declared within class `A`, and class `A` is a non-nested type because it is declared within a compilation unit.</span></span>

#### <a name="fully-qualified-name"></a><span data-ttu-id="6fe1a-509">完整格式的名稱</span><span class="sxs-lookup"><span data-stu-id="6fe1a-509">Fully qualified name</span></span>

<span data-ttu-id="6fe1a-510">完整格式的名稱 ([完整名稱](basic-concepts.md#fully-qualified-names)) 為巢狀的類型`S.N`其中`S`是哪種類型中的型別完整格式的名稱`N`宣告。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-510">The fully qualified name ([Fully qualified names](basic-concepts.md#fully-qualified-names)) for a nested type is `S.N` where `S` is the fully qualified name of the type in which type `N` is declared.</span></span>

#### <a name="declared-accessibility"></a><span data-ttu-id="6fe1a-511">已宣告存取範圍</span><span class="sxs-lookup"><span data-stu-id="6fe1a-511">Declared accessibility</span></span>

<span data-ttu-id="6fe1a-512">非巢狀型別可以有`public`或是`internal`宣告存取範圍，而且有`internal`預設宣告存取範圍。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-512">Non-nested types can have `public` or `internal` declared accessibility and have `internal` declared accessibility by default.</span></span> <span data-ttu-id="6fe1a-513">巢狀型別可以有這些形式的宣告存取範圍，再加上一個或多個額外的宣告存取範圍，根據包含的類型是類別或結構形式：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-513">Nested types can have these forms of declared accessibility too, plus one or more additional forms of declared accessibility, depending on whether the containing type is a class or struct:</span></span>

*  <span data-ttu-id="6fe1a-514">類別中宣告的巢狀的類型可以具有任何已宣告存取範圍的五種 (`public`， `protected internal`， `protected`， `internal`，或`private`) 和其他類別和成員一樣，預設值為`private`宣告存取範圍。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-514">A nested type that is declared in a class can have any of five forms of declared accessibility (`public`, `protected internal`, `protected`, `internal`, or `private`) and, like other class members, defaults to `private` declared accessibility.</span></span>
*  <span data-ttu-id="6fe1a-515">巢狀的類型宣告的結構可以具有任何三種形式的宣告存取範圍 (`public`， `internal`，或`private`) 且如同其他的結構成員，預設值為`private`宣告存取範圍。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-515">A nested type that is declared in a struct can have any of three forms of declared accessibility (`public`, `internal`, or `private`) and, like other struct members, defaults to `private` declared accessibility.</span></span>

<span data-ttu-id="6fe1a-516">此範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-516">The example</span></span>
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
<span data-ttu-id="6fe1a-517">宣告私用巢狀的類別`Node`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-517">declares a private nested class `Node`.</span></span>

#### <a name="hiding"></a><span data-ttu-id="6fe1a-518">隱藏</span><span class="sxs-lookup"><span data-stu-id="6fe1a-518">Hiding</span></span>

<span data-ttu-id="6fe1a-519">可能會隱藏巢狀的類型 ([名稱隱藏](basic-concepts.md#name-hiding)) 基底的成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-519">A nested type may hide ([Name hiding](basic-concepts.md#name-hiding)) a base member.</span></span> <span data-ttu-id="6fe1a-520">`new` ，以便可以明確表示隱藏巢狀的類型宣告使用修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-520">The `new` modifier is permitted on nested type declarations so that hiding can be expressed explicitly.</span></span> <span data-ttu-id="6fe1a-521">此範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-521">The example</span></span>
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
<span data-ttu-id="6fe1a-522">顯示巢狀的類別`M`，以隱藏方法`M`中所定義`Base`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-522">shows a nested class `M` that hides the method `M` defined in `Base`.</span></span>

#### <a name="this-access"></a><span data-ttu-id="6fe1a-523">此存取權</span><span class="sxs-lookup"><span data-stu-id="6fe1a-523">this access</span></span>

<span data-ttu-id="6fe1a-524">巢狀的類型和其包含類型沒有特殊的關聯性方面*this_access* ([此存取權](expressions.md#this-access))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-524">A nested type and its containing type do not have a special relationship with regard to *this_access* ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="6fe1a-525">具體來說，`this`內巢狀類型不能用來參考包含類型的執行個體成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-525">Specifically, `this` within a nested type cannot be used to refer to instance members of the containing type.</span></span> <span data-ttu-id="6fe1a-526">在巢狀的類型需要存取其包含類型的執行個體成員的位置的情況下，才能提供存取藉由提供`this`作為巢狀類型的建構函式引數包含型別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-526">In cases where a nested type needs access to the instance members of its containing type, access can be provided by providing the `this` for the instance of the containing type as a constructor argument for the nested type.</span></span> <span data-ttu-id="6fe1a-527">下列範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-527">The following example</span></span>
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
<span data-ttu-id="6fe1a-528">顯示這項技術。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-528">shows this technique.</span></span> <span data-ttu-id="6fe1a-529">執行個體`C`建立的執行個體`Nested`，並傳遞它自己`this`要`Nested`的建構函式，以便後續存取`C`的執行個體成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-529">An instance of `C` creates an instance of `Nested` and passes its own `this` to `Nested`'s constructor in order to provide subsequent access to `C`'s instance members.</span></span>

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a><span data-ttu-id="6fe1a-530">包含型別的 private 和 protected 成員的存取權</span><span class="sxs-lookup"><span data-stu-id="6fe1a-530">Access to private and protected members of the containing type</span></span>

<span data-ttu-id="6fe1a-531">巢狀型別具有存取權的所有成員都可以存取其包含的類型，包括已包含的型別成員`private`和`protected`宣告存取範圍。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-531">A nested type has access to all of the members that are accessible to its containing type, including members of the containing type that have `private` and `protected` declared accessibility.</span></span> <span data-ttu-id="6fe1a-532">此範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-532">The example</span></span>
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
<span data-ttu-id="6fe1a-533">顯示的類別`C`，其中包含巢狀的類別`Nested`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-533">shows a class `C` that contains a nested class `Nested`.</span></span> <span data-ttu-id="6fe1a-534">內`Nested`，方法`G`呼叫靜態方法`F`中定義`C`，和`F`具有私用宣告存取範圍。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-534">Within `Nested`, the method `G` calls the static method `F` defined in `C`, and `F` has private declared accessibility.</span></span>

<span data-ttu-id="6fe1a-535">巢狀型別也可以存取其包含類型的基底類型中定義的受保護的成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-535">A nested type also may access protected members defined in a base type of its containing type.</span></span> <span data-ttu-id="6fe1a-536">在範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-536">In the example</span></span>
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
<span data-ttu-id="6fe1a-537">巢狀的類別`Derived.Nested`存取受保護的方法`F`中定義`Derived`的基底類別`Base`、 藉由呼叫的執行個體`Derived`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-537">the nested class `Derived.Nested` accesses the protected method `F` defined in `Derived`'s base class, `Base`, by calling through an instance of `Derived`.</span></span>

#### <a name="nested-types-in-generic-classes"></a><span data-ttu-id="6fe1a-538">泛型類別中的巢狀的類型</span><span class="sxs-lookup"><span data-stu-id="6fe1a-538">Nested types in generic classes</span></span>

<span data-ttu-id="6fe1a-539">泛型類別宣告可以包含巢狀的類型宣告。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-539">A generic class declaration can contain nested type declarations.</span></span> <span data-ttu-id="6fe1a-540">在封入類別的型別參數可用於巢狀型別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-540">The type parameters of the enclosing class can be used within the nested types.</span></span> <span data-ttu-id="6fe1a-541">巢狀的類型宣告可以包含其他的類型參數僅適用於巢狀型別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-541">A nested type declaration can contain additional type parameters that apply only to the nested type.</span></span>

<span data-ttu-id="6fe1a-542">包含在泛型類別宣告內的每個型別宣告為隱含泛型型別宣告。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-542">Every type declaration contained within a generic class declaration is implicitly a generic type declaration.</span></span> <span data-ttu-id="6fe1a-543">當撰寫型別的參考巢狀泛型類型內時，包含建構的類型，包括其型別引數，必須具有名稱。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-543">When writing a reference to a type nested within a generic type, the containing constructed type, including its type arguments, must be named.</span></span> <span data-ttu-id="6fe1a-544">不過，從在外部類別中，巢狀型別可不搭配限定性條件;建構巢狀型別時，就可以隱含地使用外部類別的執行個體類型。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-544">However, from within the outer class, the nested type can be used without qualification; the instance type of the outer class can be implicitly used when constructing the nested type.</span></span> <span data-ttu-id="6fe1a-545">下列範例示範三種不同的正確方式來參考建構的型別，從建立`Inner`; 前兩個相等：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-545">The following example shows three different correct ways to refer to a constructed type created from `Inner`; the first two are equivalent:</span></span>
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

<span data-ttu-id="6fe1a-546">雖然不正確，但程式設計風格，巢狀型別中的類型參數，即可隱藏成員或型別中的外部類型宣告的參數：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-546">Although it is bad programming style, a type parameter in a nested type can hide a member or type parameter declared in the outer type:</span></span>
```csharp
class Outer<T>
{
    class Inner<T>        // Valid, hides Outer's T
    {
        public T t;       // Refers to Inner's T
    }
}
```

### <a name="reserved-member-names"></a><span data-ttu-id="6fe1a-547">保留的成員名稱</span><span class="sxs-lookup"><span data-stu-id="6fe1a-547">Reserved member names</span></span>

<span data-ttu-id="6fe1a-548">基礎 C# 執行階段實作中，每個來源成員宣告的屬性、 事件或索引子，以便實作都必須保留兩個方法簽章的類型成員宣告，它的名稱和其類型為基礎。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-548">To facilitate the underlying C# run-time implementation, for each source member declaration that is a property, event, or indexer, the implementation must reserve two method signatures based on the kind of the member declaration, its name, and its type.</span></span> <span data-ttu-id="6fe1a-549">它是編譯時期錯誤宣告的成員，其簽章符合其中一種保留簽章，即使基礎實作中，執行階段不會讓程式使用的保留區。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-549">It is a compile-time error for a program to declare a member whose signature matches one of these reserved signatures, even if the underlying run-time implementation does not make use of these reservations.</span></span>

<span data-ttu-id="6fe1a-550">保留的名稱不會產生宣告，因此它們不會參與成員查詢。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-550">The reserved names do not introduce declarations, thus they do not participate in member lookup.</span></span> <span data-ttu-id="6fe1a-551">不過，在宣告的相關聯保留簽章會參與繼承的方法 ([繼承](classes.md#inheritance))，而且可以使用隱藏`new`修飾詞 ([new 修飾詞](classes.md#the-new-modifier))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-551">However, a declaration's associated reserved method signatures do participate in inheritance ([Inheritance](classes.md#inheritance)), and can be hidden with the `new` modifier ([The new modifier](classes.md#the-new-modifier)).</span></span>

<span data-ttu-id="6fe1a-552">保留這些名稱有三種用途：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-552">The reservation of these names serves three purposes:</span></span>

*  <span data-ttu-id="6fe1a-553">若要允許使用一般的識別項為 get 方法名稱，或將存取設定為 C# 語言功能的基礎實作。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-553">To allow the underlying implementation to use an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="6fe1a-554">若要讓其他語言交互操作，使用一般的識別項為 get 方法名稱，或將存取設定為 C# 語言功能。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-554">To allow other languages to interoperate using an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="6fe1a-555">為了協助確保來源接受一個合格的編譯器接受由另一個，藉由保留成員的詳細資訊名稱一致跨所有 C# 實作。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-555">To help ensure that the source accepted by one conforming compiler is accepted by another, by making the specifics of reserved member names consistent across all C# implementations.</span></span>

<span data-ttu-id="6fe1a-556">解構函式宣告 ([解構函式](classes.md#destructors)) 也會導致保留的簽章 ([保留為解構函式的成員名稱](classes.md#member-names-reserved-for-destructors))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-556">The declaration of a destructor ([Destructors](classes.md#destructors)) also causes a signature to be reserved ([Member names reserved for destructors](classes.md#member-names-reserved-for-destructors)).</span></span>

#### <a name="member-names-reserved-for-properties"></a><span data-ttu-id="6fe1a-557">保留給屬性的成員名稱</span><span class="sxs-lookup"><span data-stu-id="6fe1a-557">Member names reserved for properties</span></span>

<span data-ttu-id="6fe1a-558">屬性`P`([屬性](classes.md#properties)) 的型別`T`，會保留下列簽章：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-558">For a property `P` ([Properties](classes.md#properties)) of type `T`, the following signatures are reserved:</span></span>

```csharp
T get_P();
void set_P(T value);
```

<span data-ttu-id="6fe1a-559">這兩個簽章都會保留，即使屬性是唯讀或唯寫。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-559">Both signatures are reserved, even if the property is read-only or write-only.</span></span>

<span data-ttu-id="6fe1a-560">在範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-560">In the example</span></span>
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
<span data-ttu-id="6fe1a-561">類別`A`唯讀屬性會定義`P`，因此保留簽章`get_P`和`set_P`方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-561">a class `A` defines a read-only property `P`, thus reserving signatures for `get_P` and `set_P` methods.</span></span> <span data-ttu-id="6fe1a-562">類別`B`衍生自`A`並隱藏這兩個這些保留的簽章。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-562">A class `B` derives from `A` and hides both of these reserved signatures.</span></span> <span data-ttu-id="6fe1a-563">此範例會產生輸出：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-563">The example produces the output:</span></span>
```
123
123
456
```

#### <a name="member-names-reserved-for-events"></a><span data-ttu-id="6fe1a-564">保留事件的成員名稱</span><span class="sxs-lookup"><span data-stu-id="6fe1a-564">Member names reserved for events</span></span>

<span data-ttu-id="6fe1a-565">事件`E`([事件](classes.md#events)) 的委派型別`T`，會保留下列簽章：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-565">For an event `E` ([Events](classes.md#events)) of delegate type `T`, the following signatures are reserved:</span></span>
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a><span data-ttu-id="6fe1a-566">保留給索引子的成員名稱</span><span class="sxs-lookup"><span data-stu-id="6fe1a-566">Member names reserved for indexers</span></span>

<span data-ttu-id="6fe1a-567">索引子 ([索引子](classes.md#indexers)) 的型別`T`使用的參數清單`L`，會保留下列簽章：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-567">For an indexer ([Indexers](classes.md#indexers)) of type `T` with parameter-list `L`, the following signatures are reserved:</span></span>
```csharp
T get_Item(L);
void set_Item(L, T value);
```

<span data-ttu-id="6fe1a-568">這兩個簽章都會保留，即使這個索引子會是唯讀或唯寫。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-568">Both signatures are reserved, even if the indexer is read-only or write-only.</span></span>

<span data-ttu-id="6fe1a-569">此外成員名稱`Item`已保留。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-569">Furthermore the member name `Item` is reserved.</span></span>

#### <a name="member-names-reserved-for-destructors"></a><span data-ttu-id="6fe1a-570">保留的解構函式的成員名稱</span><span class="sxs-lookup"><span data-stu-id="6fe1a-570">Member names reserved for destructors</span></span>

<span data-ttu-id="6fe1a-571">類別包含解構函式 ([解構函式](classes.md#destructors))，會保留下列簽章：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-571">For a class containing a destructor ([Destructors](classes.md#destructors)), the following signature is reserved:</span></span>
```csharp
void Finalize();
```

## <a name="constants"></a><span data-ttu-id="6fe1a-572">常數</span><span class="sxs-lookup"><span data-stu-id="6fe1a-572">Constants</span></span>

<span data-ttu-id="6fe1a-573">A***常數***類別成員，表示為常數值： 可以在編譯時期計算的值。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-573">A ***constant*** is a class member that represents a constant value: a value that can be computed at compile-time.</span></span> <span data-ttu-id="6fe1a-574">A *constant_declaration*導入了一個或多個指定類型的常數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-574">A *constant_declaration* introduces one or more constants of a given type.</span></span>

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

<span data-ttu-id="6fe1a-575">A *constant_declaration*可能包含一組*屬性*([屬性](attributes.md))，則`new`修飾詞 ([new 修飾詞](classes.md#the-new-modifier))，和有效的四種存取修飾詞組合 ([存取修飾詞](classes.md#access-modifiers))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-575">A *constant_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span> <span data-ttu-id="6fe1a-576">屬性和修飾詞套用至所宣告的成員全部*constant_declaration*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-576">The attributes and modifiers apply to all of the members declared by the *constant_declaration*.</span></span> <span data-ttu-id="6fe1a-577">即使常數會被視為靜態成員而言*constant_declaration*都不需要也不允許`static`修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-577">Even though constants are considered static members, a *constant_declaration* neither requires nor allows a `static` modifier.</span></span> <span data-ttu-id="6fe1a-578">它是相同的修飾詞在常數宣告中出現多次錯誤。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-578">It is an error for the same modifier to appear multiple times in a constant declaration.</span></span>

<span data-ttu-id="6fe1a-579">*型別*的*constant_declaration*指定宣告引入的成員型別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-579">The *type* of a *constant_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="6fe1a-580">類型後面接著一份*constant_declarator*s，其中每一個導入了新的成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-580">The type is followed by a list of *constant_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="6fe1a-581">A *constant_declarator*組成*識別項*可命名成員，後面接著"`=`"語彙基元，後面接著*constant_expression* ([常數運算式](expressions.md#constant-expressions)) 提供成員的值。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-581">A *constant_declarator* consists of an *identifier* that names the member, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the member.</span></span>

<span data-ttu-id="6fe1a-582">*型別*常數中指定的宣告必須能`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char`，`float`， `double`， `decimal`， `bool`， `string`，則*enum_type*，或有*reference_type*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-582">The *type* specified in a constant declaration must be `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`, an *enum_type*, or a *reference_type*.</span></span> <span data-ttu-id="6fe1a-583">每個*constant_expression*必須使用 yield 產生值的目標型別或可以隱含的轉換方式轉換成目標類型的類型 ([隱含轉換](conversions.md#implicit-conversions))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-583">Each *constant_expression* must yield a value of the target type or of a type that can be converted to the target type by an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>

<span data-ttu-id="6fe1a-584">*型別*必須至少像常數本身那樣的常數 ([存取範圍條件約束](basic-concepts.md#accessibility-constraints))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-584">The *type* of a constant must be at least as accessible as the constant itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="6fe1a-585">取得在運算式中使用的常數值*simple_name* ([簡單名稱](expressions.md#simple-names)) 或*member_access* ([成員存取](expressions.md#member-access))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-585">The value of a constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="6fe1a-586">常數可以本身參與*constant_expression*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-586">A constant can itself participate in a *constant_expression*.</span></span> <span data-ttu-id="6fe1a-587">因此，可能在任何需要的建構中使用常數*constant_expression*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-587">Thus, a constant may be used in any construct that requires a *constant_expression*.</span></span> <span data-ttu-id="6fe1a-588">這類建構的範例包括`case`標籤`goto case`陳述式，`enum`成員宣告、 屬性和其他常數宣告。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-588">Examples of such constructs include `case` labels, `goto case` statements, `enum` member declarations, attributes, and other constant declarations.</span></span>

<span data-ttu-id="6fe1a-589">中所述[常數運算式](expressions.md#constant-expressions)，則*constant_expression*是可以在編譯時期完整評估的運算式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-589">As described in [Constant expressions](expressions.md#constant-expressions), a *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span> <span data-ttu-id="6fe1a-590">之後建立的非 null 值的唯一辦法*reference_type*以外`string`是套用`new`運算子，而由於`new`中不允許運算子*constant_運算式*，唯一可能的值的常數*reference_type*以外的 s`string`是`null`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-590">Since the only way to create a non-null value of a *reference_type* other than `string` is to apply the `new` operator, and since the `new` operator is not permitted in a *constant_expression*, the only possible value for constants of *reference_type*s other than `string` is `null`.</span></span>

<span data-ttu-id="6fe1a-591">當想要的常數值的符號名稱，但該值類型時不允許在常數宣告中，或當無法在編譯時期所計算的值*constant_expression*、`readonly`欄位 （[唯讀欄位](classes.md#readonly-fields)) 可能會改為使用。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-591">When a symbolic name for a constant value is desired, but when the type of that value is not permitted in a constant declaration, or when the value cannot be computed at compile-time by a *constant_expression*, a `readonly` field ([Readonly fields](classes.md#readonly-fields)) may be used instead.</span></span>

<span data-ttu-id="6fe1a-592">常數宣告會宣告多個常數，相當於具有相同的屬性、 修飾詞與類型的單一常數的多個宣告。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-592">A constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="6fe1a-593">例如</span><span class="sxs-lookup"><span data-stu-id="6fe1a-593">For example</span></span>
```csharp
class A
{
    public const double X = 1.0, Y = 2.0, Z = 3.0;
}
```
<span data-ttu-id="6fe1a-594">相當於</span><span class="sxs-lookup"><span data-stu-id="6fe1a-594">is equivalent to</span></span>
```csharp
class A
{
    public const double X = 1.0;
    public const double Y = 2.0;
    public const double Z = 3.0;
}
```

<span data-ttu-id="6fe1a-595">常數可以相依於同一個程式內的其他常數，只要相依性不是循環的性質。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-595">Constants are permitted to depend on other constants within the same program as long as the dependencies are not of a circular nature.</span></span> <span data-ttu-id="6fe1a-596">編譯器會自動排列要評估的常數宣告中適當的順序。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-596">The compiler automatically arranges to evaluate the constant declarations in the appropriate order.</span></span> <span data-ttu-id="6fe1a-597">在範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-597">In the example</span></span>
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
<span data-ttu-id="6fe1a-598">編譯器會先評估`A.Y`，則會評估`B.Z`，然後最後評估`A.X`，產生的值`10`， `11`，和`12`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-598">the compiler first evaluates `A.Y`, then evaluates `B.Z`, and finally evaluates `A.X`, producing the values `10`, `11`, and `12`.</span></span> <span data-ttu-id="6fe1a-599">常數宣告可能相依於其他程式中的常數，但這類相依性時，才可以在一個方向。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-599">Constant declarations may depend on constants from other programs, but such dependencies are only possible in one direction.</span></span> <span data-ttu-id="6fe1a-600">參考上述範例中，如果`A`並`B`已宣告於不同的程式，它是可行的`A.X`取決於`B.Z`，但`B.Z`不同時可能接著取決於`A.Y`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-600">Referring to the example above, if `A` and `B` were declared in separate programs, it would be possible for `A.X` to depend on `B.Z`, but `B.Z` could then not simultaneously depend on `A.Y`.</span></span>

## <a name="fields"></a><span data-ttu-id="6fe1a-601">欄位</span><span class="sxs-lookup"><span data-stu-id="6fe1a-601">Fields</span></span>

<span data-ttu-id="6fe1a-602">A***欄位***成員表示的物件或類別相關聯的變數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-602">A ***field*** is a member that represents a variable associated with an object or class.</span></span> <span data-ttu-id="6fe1a-603">A *field_declaration*導入了一個或多個指定類型的欄位。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-603">A *field_declaration* introduces one or more fields of a given type.</span></span>

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

<span data-ttu-id="6fe1a-604">A *field_declaration*可能包含一組*屬性*([屬性](attributes.md))，則`new`修飾詞 ([new 修飾詞](classes.md#the-new-modifier))、有效的四種存取修飾詞組合 ([存取修飾詞](classes.md#access-modifiers))，以及`static`修飾詞 ([靜態和執行個體欄位](classes.md#static-and-instance-fields))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-604">A *field_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and a `static` modifier ([Static and instance fields](classes.md#static-and-instance-fields)).</span></span> <span data-ttu-id="6fe1a-605">颾魤 ㄛ *field_declaration*可能會包含`readonly`修飾詞 ([唯讀欄位](classes.md#readonly-fields)) 或`volatile`修飾詞 ([Volatile 欄位](classes.md#volatile-fields))，但非兩者。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-605">In addition, a *field_declaration* may include a `readonly` modifier ([Readonly fields](classes.md#readonly-fields)) or a `volatile` modifier ([Volatile fields](classes.md#volatile-fields)) but not both.</span></span> <span data-ttu-id="6fe1a-606">屬性和修飾詞套用至所宣告的成員全部*field_declaration*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-606">The attributes and modifiers apply to all of the members declared by the *field_declaration*.</span></span> <span data-ttu-id="6fe1a-607">它是相同的修飾詞，若要在欄位宣告中出現多次錯誤。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-607">It is an error for the same modifier to appear multiple times in a field declaration.</span></span>

<span data-ttu-id="6fe1a-608">*型別*的*field_declaration*指定宣告引入的成員型別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-608">The *type* of a *field_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="6fe1a-609">類型後面接著一份*variable_declarator*s，其中每一個導入了新的成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-609">The type is followed by a list of *variable_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="6fe1a-610">A *variable_declarator*組成*識別項*可命名該成員，並且選擇性地加 」`=`"語彙基元和*variable_initializer* ([變數初始設定式](classes.md#variable-initializers))，可讓該成員的初始值。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-610">A *variable_declarator* consists of an *identifier* that names that member, optionally followed by an "`=`" token and a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)) that gives the initial value of that member.</span></span>

<span data-ttu-id="6fe1a-611">*型別*必須至少像欄位本身那樣的欄位 ([存取範圍條件約束](basic-concepts.md#accessibility-constraints))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-611">The *type* of a field must be at least as accessible as the field itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="6fe1a-612">取得欄位的值中使用運算式*simple_name* ([簡單名稱](expressions.md#simple-names)) 或*member_access* ([成員存取](expressions.md#member-access))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-612">The value of a field is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="6fe1a-613">使用修改的非唯讀欄位的值*指派*([指派運算子](expressions.md#assignment-operators))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-613">The value of a non-readonly field is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="6fe1a-614">非唯讀欄位的值可以是取得和使用修改後置遞增和遞減運算子 ([後置遞增和遞減運算子](expressions.md#postfix-increment-and-decrement-operators)) 和前置遞增和遞減運算子 ([前置詞遞增和遞減運算子](expressions.md#prefix-increment-and-decrement-operators))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-614">The value of a non-readonly field can be both obtained and modified using postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)) and prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="6fe1a-615">欄位宣告會宣告多個欄位相當於具有相同的屬性、 修飾詞與類型的單一欄位的多個宣告。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-615">A field declaration that declares multiple fields is equivalent to multiple declarations of single fields with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="6fe1a-616">例如</span><span class="sxs-lookup"><span data-stu-id="6fe1a-616">For example</span></span>
```csharp
class A
{
    public static int X = 1, Y, Z = 100;
}
```
<span data-ttu-id="6fe1a-617">相當於</span><span class="sxs-lookup"><span data-stu-id="6fe1a-617">is equivalent to</span></span>
```csharp
class A
{
    public static int X = 1;
    public static int Y;
    public static int Z = 100;
}
```

### <a name="static-and-instance-fields"></a><span data-ttu-id="6fe1a-618">靜態和執行個體欄位</span><span class="sxs-lookup"><span data-stu-id="6fe1a-618">Static and instance fields</span></span>

<span data-ttu-id="6fe1a-619">當欄位宣告包含`static`修飾詞，宣告所引進的欄位都***靜態欄位***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-619">When a field declaration includes a `static` modifier, the fields introduced by the declaration are ***static fields***.</span></span> <span data-ttu-id="6fe1a-620">若未`static`修飾詞存在，則宣告所引進的欄位***執行個體欄位***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-620">When no `static` modifier is present, the fields introduced by the declaration are ***instance fields***.</span></span> <span data-ttu-id="6fe1a-621">靜態欄位和執行個體欄位是一種變數的兩個 ([變數](variables.md)) 支援 C# 中，而且有時候它們會稱為***靜態變數***和***執行個體變數***分別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-621">Static fields and instance fields are two of the several kinds of variables ([Variables](variables.md)) supported by C#, and at times they are referred to as ***static variables*** and ***instance variables***, respectively.</span></span>

<span data-ttu-id="6fe1a-622">靜態欄位不是特定的執行個體; 的一部分相反地，它會在封閉類型的所有執行個體間共用 ([開放和封閉類型](types.md#open-and-closed-types))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-622">A static field is not part of a specific instance; instead, it is shared amongst all instances of a closed type ([Open and closed types](types.md#open-and-closed-types)).</span></span> <span data-ttu-id="6fe1a-623">不論建立多少個已關閉的類別類型執行個體，沒有相關聯的應用程式定義域的靜態欄位的只有一個複本。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-623">No matter how many instances of a closed class type are created, there is only ever one copy of a static field for the associated application domain.</span></span>

<span data-ttu-id="6fe1a-624">例如: </span><span class="sxs-lookup"><span data-stu-id="6fe1a-624">For example:</span></span>
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

<span data-ttu-id="6fe1a-625">執行個體欄位所屬的執行個體。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-625">An instance field belongs to an instance.</span></span> <span data-ttu-id="6fe1a-626">具體來說，每個類別執行個體包含一組個別的該類別的所有執行個體欄位。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-626">Specifically, every instance of a class contains a separate set of all the instance fields of that class.</span></span>

<span data-ttu-id="6fe1a-627">欄位中的參考時*member_access* ([成員存取](expressions.md#member-access)) 的表單`E.M`時，如果`M`是靜態欄位，`E`必須表示型別，其中`M`如果`M`是執行個體欄位，E 必須代表執行個體的型別，其中`M`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-627">When a field is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static field, `E` must denote a type containing `M`, and if `M` is an instance field, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="6fe1a-628">靜態之間的差異和執行個體成員討論中進一步[靜態和執行個體成員](classes.md#static-and-instance-members)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-628">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="readonly-fields"></a><span data-ttu-id="6fe1a-629">唯讀欄位</span><span class="sxs-lookup"><span data-stu-id="6fe1a-629">Readonly fields</span></span>

<span data-ttu-id="6fe1a-630">當*field_declaration*包含`readonly`修飾詞，宣告所引進的欄位會***唯讀欄位***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-630">When a *field_declaration* includes a `readonly` modifier, the fields introduced by the declaration are ***readonly fields***.</span></span> <span data-ttu-id="6fe1a-631">將唯讀欄位的直接指派只能當做執行個體建構函式或靜態的建構函式中相同的類別中宣告的一部分。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-631">Direct assignments to readonly fields can only occur as part of that declaration or in an instance constructor or static constructor in the same class.</span></span> <span data-ttu-id="6fe1a-632">（將唯讀欄位可以指派給多個時間在這些內容中。）具體來說，直接指派至`readonly`欄位允許只在下列情況：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-632">(A readonly field can be assigned to multiple times in these contexts.) Specifically, direct assignments to a `readonly` field are permitted only in the following contexts:</span></span>

*  <span data-ttu-id="6fe1a-633">在  *variable_declarator*導入的欄位 (加*variable_initializer*宣告中)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-633">In the *variable_declarator* that introduces the field (by including a *variable_initializer* in the declaration).</span></span>
*  <span data-ttu-id="6fe1a-634">執行個體欄位，在包含欄位宣告; 類別的執行個體建構函式靜態欄位，則是在包含欄位宣告類別的靜態建構函式中進行。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-634">For an instance field, in the instance constructors of the class that contains the field declaration; for a static field, in the static constructor of the class that contains the field declaration.</span></span> <span data-ttu-id="6fe1a-635">這些也是它會將有效的唯一內容`readonly`欄位做為`out`或`ref`參數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-635">These are also the only contexts in which it is valid to pass a `readonly` field as an `out` or `ref` parameter.</span></span>

<span data-ttu-id="6fe1a-636">嘗試將指派給`readonly`欄位，或將它傳遞為`out`或`ref`在任何其他內容中的參數是編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-636">Attempting to assign to a `readonly` field or pass it as an `out` or `ref` parameter in any other context is a compile-time error.</span></span>

#### <a name="using-static-readonly-fields-for-constants"></a><span data-ttu-id="6fe1a-637">使用靜態唯讀欄位常數</span><span class="sxs-lookup"><span data-stu-id="6fe1a-637">Using static readonly fields for constants</span></span>

<span data-ttu-id="6fe1a-638">A`static readonly`欄位非常有用，當想要的常數值的符號名稱，但當值的型別中不允許`const`宣告，或當無法在編譯時期計算的值。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-638">A `static readonly` field is useful when a symbolic name for a constant value is desired, but when the type of the value is not permitted in a `const` declaration, or when the value cannot be computed at compile-time.</span></span> <span data-ttu-id="6fe1a-639">在範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-639">In the example</span></span>
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
<span data-ttu-id="6fe1a-640">`Black`， `White`， `Red`， `Green`，並`Blue`成員不可以宣告為`const`成員因為其值無法在編譯時期計算。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-640">the `Black`, `White`, `Red`, `Green`, and `Blue` members cannot be declared as `const` members because their values cannot be computed at compile-time.</span></span> <span data-ttu-id="6fe1a-641">不過，宣告就`static readonly`改為具有許多相同的效果。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-641">However, declaring them `static readonly` instead has much the same effect.</span></span>

#### <a name="versioning-of-constants-and-static-readonly-fields"></a><span data-ttu-id="6fe1a-642">常數和靜態唯讀欄位的版本控制</span><span class="sxs-lookup"><span data-stu-id="6fe1a-642">Versioning of constants and static readonly fields</span></span>

<span data-ttu-id="6fe1a-643">常數和唯讀欄位有不同的二進位版本設定語意。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-643">Constants and readonly fields have different binary versioning semantics.</span></span> <span data-ttu-id="6fe1a-644">當運算式參考常數時，在編譯時期取得常數的值，但是當運算式參考到唯讀欄位，欄位的值不會取得執行階段。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-644">When an expression references a constant, the value of the constant is obtained at compile-time, but when an expression references a readonly field, the value of the field is not obtained until run-time.</span></span> <span data-ttu-id="6fe1a-645">請考慮這兩個不同的程式所組成的應用程式：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-645">Consider an application that consists of two separate programs:</span></span>
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

<span data-ttu-id="6fe1a-646">`Program1`和`Program2`命名空間表示這兩個分開編譯的程式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-646">The `Program1` and `Program2` namespaces denote two programs that are compiled separately.</span></span> <span data-ttu-id="6fe1a-647">因為`Program1.Utils.X`宣告為靜態唯讀欄位的值輸出`Console.WriteLine`陳述式會在編譯時期不知道，但已取得在執行階段。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-647">Because `Program1.Utils.X` is declared as a static readonly field, the value output by the `Console.WriteLine` statement is not known at compile-time, but rather is obtained at run-time.</span></span> <span data-ttu-id="6fe1a-648">因此，如果值`X`變更並`Program1`重新編譯時，`Console.WriteLine`陳述式會將新的值，即使`Program2`未重新編譯。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-648">Thus, if the value of `X` is changed and `Program1` is recompiled, the `Console.WriteLine` statement will output the new value even if `Program2` isn't recompiled.</span></span> <span data-ttu-id="6fe1a-649">不過，有`X`已常數的值`X`會取得當時`Program2`編譯，並不會受到影響的變更`Program1`直到`Program2`重新編譯。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-649">However, had `X` been a constant, the value of `X` would have been obtained at the time `Program2` was compiled, and would remain unaffected by changes in `Program1` until `Program2` is recompiled.</span></span>

### <a name="volatile-fields"></a><span data-ttu-id="6fe1a-650">Volatile 欄位</span><span class="sxs-lookup"><span data-stu-id="6fe1a-650">Volatile fields</span></span>

<span data-ttu-id="6fe1a-651">當*field_declaration*包含`volatile`修飾詞，該宣告所引進的欄位會***volatile 欄位***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-651">When a *field_declaration* includes a `volatile` modifier, the fields introduced by that declaration are ***volatile fields***.</span></span>

<span data-ttu-id="6fe1a-652">對於靜態欄位，重新排列指示的最佳化技術可能會導致非預期且無法預期的結果，可存取欄位不需要同步處理，例如提供的多執行緒程式中*lock_statement* ([Lock 陳述式](statements.md#the-lock-statement))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-652">For non-volatile fields, optimization techniques that reorder instructions can lead to unexpected and unpredictable results in multi-threaded programs that access fields without synchronization such as that provided by the *lock_statement* ([The lock statement](statements.md#the-lock-statement)).</span></span> <span data-ttu-id="6fe1a-653">編譯器、 執行階段系統中，或硬體，可以執行這些最佳化。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-653">These optimizations can be performed by the compiler, by the run-time system, or by hardware.</span></span> <span data-ttu-id="6fe1a-654">Volatile 欄位，這種重新最佳化會限制：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-654">For volatile fields, such reordering optimizations are restricted:</span></span>

*  <span data-ttu-id="6fe1a-655">Volatile 欄位讀取稱為***暫時性讀取***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-655">A read of a volatile field is called a ***volatile read***.</span></span> <span data-ttu-id="6fe1a-656">暫時性讀取具有 「 取得語意 」;也就是說，它保證記憶體發生之後，在指令序列中的任何參考之前發生。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-656">A volatile read has "acquire semantics"; that is, it is guaranteed to occur prior to any references to memory that occur after it in the instruction sequence.</span></span>
*  <span data-ttu-id="6fe1a-657">Volatile 欄位的寫入稱為***暫時性寫入***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-657">A write of a volatile field is called a ***volatile write***.</span></span> <span data-ttu-id="6fe1a-658">暫時性寫入有 「 發行語意 」;也就是說，它保證任何的記憶體參考之前在指令序列中的寫入指令之後發生。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-658">A volatile write has "release semantics"; that is, it is guaranteed to happen after any memory references prior to the write instruction in the instruction sequence.</span></span>

<span data-ttu-id="6fe1a-659">這些限制可確保所有的執行緒將會觀察任何其他執行緒執行的 volatile 寫入會按照其執行的順序進行。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-659">These restrictions ensure that all threads will observe volatile writes performed by any other thread in the order in which they were performed.</span></span> <span data-ttu-id="6fe1a-660">合格的實作不是需要提供的單一總排序的變動性寫入所有執行緒的執行中所示。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-660">A conforming implementation is not required to provide a single total ordering of volatile writes as seen from all threads of execution.</span></span> <span data-ttu-id="6fe1a-661">Volatile 欄位的類型必須是下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-661">The type of a volatile field must be one of the following:</span></span>

*  <span data-ttu-id="6fe1a-662">A *reference_type*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-662">A *reference_type*.</span></span>
*  <span data-ttu-id="6fe1a-663">型別`byte`， `sbyte`， `short`， `ushort`， `int`， `uint`， `char`， `float`， `bool`， `System.IntPtr`，或` System.UIntPtr`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-663">The type `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`, or` System.UIntPtr`.</span></span>
*  <span data-ttu-id="6fe1a-664">*Enum_type*的列舉基底型別`byte`， `sbyte`， `short`， `ushort`， `int`，或`uint`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-664">An *enum_type* having an enum base type of `byte`, `sbyte`, `short`, `ushort`, `int`, or `uint`.</span></span>

<span data-ttu-id="6fe1a-665">此範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-665">The example</span></span>
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
<span data-ttu-id="6fe1a-666">產生下列輸出：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-666">produces the output:</span></span>
```
result = 143
```

<span data-ttu-id="6fe1a-667">在此範例中，此方法`Main`啟動新的執行緒執行方法`Thread2`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-667">In this example, the method `Main` starts a new thread that runs the method `Thread2`.</span></span> <span data-ttu-id="6fe1a-668">這個方法會將值儲存到名為的靜態欄位`result`，然後將儲存`true`volatile 欄位在`finished`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-668">This method stores a value into a non-volatile field called `result`, then stores `true` in the volatile field `finished`.</span></span> <span data-ttu-id="6fe1a-669">在主執行緒等候欄位`finished`設為`true`，然後讀取欄位`result`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-669">The main thread waits for the field `finished` to be set to `true`, then reads the field `result`.</span></span> <span data-ttu-id="6fe1a-670">由於`finished`已宣告`volatile`，在主執行緒必須讀取值`143`從欄位`result`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-670">Since `finished` has been declared `volatile`, the main thread must read the value `143` from the field `result`.</span></span> <span data-ttu-id="6fe1a-671">如果欄位`finished`未宣告`volatile`，則會是允許的儲存庫至`result`之後的存放區會被主執行緒看見`finished`，，因此讀取值之主執行緒`0`從欄位`result`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-671">If the field `finished` had not been declared `volatile`, then it would be permissible for the store to `result` to be visible to the main thread after the store to `finished`, and hence for the main thread to read the value `0` from the field `result`.</span></span> <span data-ttu-id="6fe1a-672">宣告`finished`做為`volatile`欄位會防止任何這類不一致。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-672">Declaring `finished` as a `volatile` field prevents any such inconsistency.</span></span>

### <a name="field-initialization"></a><span data-ttu-id="6fe1a-673">欄位初始設定</span><span class="sxs-lookup"><span data-stu-id="6fe1a-673">Field initialization</span></span>

<span data-ttu-id="6fe1a-674">初始欄位的值，它是靜態欄位或執行個體欄位，是否為預設值 ([預設值](variables.md#default-values)) 的欄位的類型。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-674">The initial value of a field, whether it be a static field or an instance field, is the default value ([Default values](variables.md#default-values)) of the field's type.</span></span> <span data-ttu-id="6fe1a-675">您不可能再發生此預設值初始化，而且欄位因此永遠不會 「 未初始化 」 看到欄位的值。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-675">It is not possible to observe the value of a field before this default initialization has occurred, and a field is thus never "uninitialized".</span></span> <span data-ttu-id="6fe1a-676">此範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-676">The example</span></span>
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
<span data-ttu-id="6fe1a-677">產生下列輸出</span><span class="sxs-lookup"><span data-stu-id="6fe1a-677">produces the output</span></span>
```
b = False, i = 0
```
<span data-ttu-id="6fe1a-678">因為`b`和`i`都會自動初始化為預設值。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-678">because `b` and `i` are both automatically initialized to default values.</span></span>

### <a name="variable-initializers"></a><span data-ttu-id="6fe1a-679">變數初始設定式</span><span class="sxs-lookup"><span data-stu-id="6fe1a-679">Variable initializers</span></span>

<span data-ttu-id="6fe1a-680">欄位宣告可能包含*variable_initializer*s。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-680">Field declarations may include *variable_initializer*s.</span></span> <span data-ttu-id="6fe1a-681">變數初始設定式靜態欄位，對應類別初始設定期間所執行的指派陳述式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-681">For static fields, variable initializers correspond to assignment statements that are executed during class initialization.</span></span> <span data-ttu-id="6fe1a-682">執行個體欄位，變數的初始設定式會對應至類別的執行個體建立時執行的指派陳述式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-682">For instance fields, variable initializers correspond to assignment statements that are executed when an instance of the class is created.</span></span>

<span data-ttu-id="6fe1a-683">此範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-683">The example</span></span>
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
<span data-ttu-id="6fe1a-684">產生下列輸出</span><span class="sxs-lookup"><span data-stu-id="6fe1a-684">produces the output</span></span>
```
x = 1.4142135623731, i = 100, s = Hello
```
<span data-ttu-id="6fe1a-685">因為指派給`x`就會發生靜態欄位初始設定式的執行時，並指派給`i`和`s`執行個體欄位初始設定式執行時，會發生。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-685">because an assignment to `x` occurs when static field initializers execute and assignments to `i` and `s` occur when the instance field initializers execute.</span></span>

<span data-ttu-id="6fe1a-686">中所述的預設值，初始化[欄位初始化](classes.md#field-initialization)針對所有欄位，包括有變數的初始設定式的欄位，就會發生。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-686">The default value initialization described in [Field initialization](classes.md#field-initialization) occurs for all fields, including fields that have variable initializers.</span></span> <span data-ttu-id="6fe1a-687">因此，初始化類別時，該類別中的所有靜態欄位會先初始化為其預設值，並接著靜態欄位初始設定式來執行文字順序。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-687">Thus, when a class is initialized, all static fields in that class are first initialized to their default values, and then the static field initializers are executed in textual order.</span></span> <span data-ttu-id="6fe1a-688">同樣地，建立類別的執行個體時，該執行個體中的所有執行個體欄位會先初始化為其預設值，並接著執行個體欄位初始設定式來執行文字順序。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-688">Likewise, when an instance of a class is created, all instance fields in that instance are first initialized to their default values, and then the instance field initializers are executed in textual order.</span></span>

<span data-ttu-id="6fe1a-689">您可使用變數的初始設定式的預設值狀態的靜態欄位。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-689">It is possible for static fields with variable initializers to be observed in their default value state.</span></span> <span data-ttu-id="6fe1a-690">不過，這是樣式的強烈建議您不要。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-690">However, this is strongly discouraged as a matter of style.</span></span> <span data-ttu-id="6fe1a-691">此範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-691">The example</span></span>
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
<span data-ttu-id="6fe1a-692">示範此行為。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-692">exhibits this behavior.</span></span> <span data-ttu-id="6fe1a-693">儘管的循環定義，而 b，程式是有效。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-693">Despite the circular definitions of a and b, the program is valid.</span></span> <span data-ttu-id="6fe1a-694">它會導致輸出</span><span class="sxs-lookup"><span data-stu-id="6fe1a-694">It results in the output</span></span>
```
a = 1, b = 2
```
<span data-ttu-id="6fe1a-695">因為靜態欄位`a`並`b`初始化為`0`(的預設值為`int`) 會執行其初始設定式之前。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-695">because the static fields `a` and `b` are initialized to `0` (the default value for `int`) before their initializers are executed.</span></span> <span data-ttu-id="6fe1a-696">時的初始設定式`a`執行時，值`b`為零，因此`a`初始化為`1`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-696">When the initializer for `a` runs, the value of `b` is zero, and so `a` is initialized to `1`.</span></span> <span data-ttu-id="6fe1a-697">時的初始設定式`b`執行時，值`a`已經`1`，，因此`b`會初始化為`2`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-697">When the initializer for `b` runs, the value of `a` is already `1`, and so `b` is initialized to `2`.</span></span>

#### <a name="static-field-initialization"></a><span data-ttu-id="6fe1a-698">靜態欄位初始設定</span><span class="sxs-lookup"><span data-stu-id="6fe1a-698">Static field initialization</span></span>

<span data-ttu-id="6fe1a-699">類別的靜態欄位變數初始設定式會對應至一系列以其出現在類別宣告中的文字順序所執行的設定。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-699">The static field variable initializers of a class correspond to a sequence of assignments that are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="6fe1a-700">如果靜態建構函式 ([靜態建構函式](classes.md#static-constructors)) 存在於類別中的靜態欄位初始設定式會執行之前執行該靜態建構函式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-700">If a static constructor ([Static constructors](classes.md#static-constructors)) exists in the class, execution of the static field initializers occurs immediately prior to executing that static constructor.</span></span> <span data-ttu-id="6fe1a-701">否則靜態欄位初始設定式會執行該類別的靜態欄位的第一次使用之前視實作而定時間。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-701">Otherwise, the static field initializers are executed at an implementation-dependent time prior to the first use of a static field of that class.</span></span> <span data-ttu-id="6fe1a-702">此範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-702">The example</span></span>
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
<span data-ttu-id="6fe1a-703">可能會產生下列輸出：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-703">might produce either the output:</span></span>
```
Init A
Init B
1 1
```
<span data-ttu-id="6fe1a-704">或下列輸出：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-704">or the output:</span></span>
```
Init B
Init A
1 1
```
<span data-ttu-id="6fe1a-705">因為執行`X`的初始設定式和`Y`的初始設定式可以按照任何順序發生; 它們只受到這些欄位的參考之前發生。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-705">because the execution of `X`'s initializer and `Y`'s initializer could occur in either order; they are only constrained to occur before the references to those fields.</span></span> <span data-ttu-id="6fe1a-706">不過，在此範例中：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-706">However, in the example:</span></span>
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
<span data-ttu-id="6fe1a-707">必須是輸出：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-707">the output must be:</span></span>
```
Init B
Init A
1 1
```
<span data-ttu-id="6fe1a-708">因為靜態建構函式時執行的規則 (中所定義[靜態建構函式](classes.md#static-constructors)) 提供`B`的靜態建構函式 (因此`B`的靜態欄位初始設定式) 必須先執行才能`A`的靜態建構函式和欄位初始設定式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-708">because the rules for when static constructors execute (as defined in [Static constructors](classes.md#static-constructors)) provide that `B`'s static constructor (and hence `B`'s static field initializers) must run before `A`'s static constructor and field initializers.</span></span>

#### <a name="instance-field-initialization"></a><span data-ttu-id="6fe1a-709">執行個體欄位初始設定</span><span class="sxs-lookup"><span data-stu-id="6fe1a-709">Instance field initialization</span></span>

<span data-ttu-id="6fe1a-710">執行個體欄位變數初始設定式的類別對應至一系列的項目至其中的執行個體建構函式時立即執行的設定 ([建構函式初始設定式](classes.md#constructor-initializers)) 該類別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-710">The instance field variable initializers of a class correspond to a sequence of assignments that are executed immediately upon entry to any one of the instance constructors ([Constructor initializers](classes.md#constructor-initializers)) of that class.</span></span> <span data-ttu-id="6fe1a-711">變數的初始設定式會以其出現在類別宣告中的文字順序執行。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-711">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="6fe1a-712">類別執行個體建立和初始化程序所述進一步[執行個體建構函式](classes.md#instance-constructors)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-712">The class instance creation and initialization process is described further in [Instance constructors](classes.md#instance-constructors).</span></span>

<span data-ttu-id="6fe1a-713">執行個體欄位的變數初始設定式無法參考所建立的執行個體。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-713">A variable initializer for an instance field cannot reference the instance being created.</span></span> <span data-ttu-id="6fe1a-714">因此，它是編譯時期錯誤參考`this`中的變數初始設定式，因為它是參考透過任何執行個體成員變數初始設定式的編譯時期錯誤*simple_name*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-714">Thus, it is a compile-time error to reference `this` in a variable initializer, as it is a compile-time error for a variable initializer to reference any instance member through a *simple_name*.</span></span> <span data-ttu-id="6fe1a-715">在範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-715">In the example</span></span>
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
<span data-ttu-id="6fe1a-716">變數初始設定式`y`導致編譯時期錯誤，因為它參考所建立的執行個體的成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-716">the variable initializer for `y` results in a compile-time error because it references a member of the instance being created.</span></span>

## <a name="methods"></a><span data-ttu-id="6fe1a-717">方法</span><span class="sxs-lookup"><span data-stu-id="6fe1a-717">Methods</span></span>

<span data-ttu-id="6fe1a-718">「方法」是實作物件或類別所能執行之計算或動作的成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-718">A ***method*** is a member that implements a computation or action that can be performed by an object or class.</span></span> <span data-ttu-id="6fe1a-719">方法使用宣告*method_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="6fe1a-719">Methods are declared using *method_declaration*s:</span></span>

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

<span data-ttu-id="6fe1a-720">A *method_declaration*可能包含一組*屬性*([屬性](attributes.md)) 和有效的四種存取修飾詞組合 ([存取修飾詞](classes.md#access-modifiers))，則`new`([的新修飾詞](classes.md#the-new-modifier))， `static` ([靜態和執行個體方法](classes.md#static-and-instance-methods))， `virtual` ([虛擬方法](classes.md#virtual-methods))， `override` ([覆寫方法](classes.md#override-methods))， `sealed` ([密封方法](classes.md#sealed-methods))， `abstract` ([抽象方法](classes.md#abstract-methods))，以及`extern` ([外部方法](classes.md#external-methods)) 修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-720">A *method_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="6fe1a-721">下列所有動作，則為 true 時，宣告就會擁有有效的修飾詞組合：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-721">A declaration has a valid combination of modifiers if all of the following are true:</span></span>

*  <span data-ttu-id="6fe1a-722">宣告包含有效的存取修飾詞組合 ([存取修飾詞](classes.md#access-modifiers))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-722">The declaration includes a valid combination of access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span>
*  <span data-ttu-id="6fe1a-723">宣告不包含相同的修飾詞多次。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-723">The declaration does not include the same modifier multiple times.</span></span>
*  <span data-ttu-id="6fe1a-724">宣告包含最多有一個下列修飾詞： `static`， `virtual`，和`override`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-724">The declaration includes at most one of the following modifiers: `static`, `virtual`, and `override`.</span></span>
*  <span data-ttu-id="6fe1a-725">宣告包含最多有一個下列修飾詞：`new`和`override`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-725">The declaration includes at most one of the following modifiers: `new` and `override`.</span></span>
*  <span data-ttu-id="6fe1a-726">如果宣告包含`abstract`修飾詞，則宣告不包含任何下列修飾詞： `static`， `virtual`，`sealed`或`extern`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-726">If the declaration includes the `abstract` modifier, then the declaration does not include any of the following modifiers: `static`, `virtual`, `sealed` or `extern`.</span></span>
*  <span data-ttu-id="6fe1a-727">如果宣告包含`private`修飾詞，則宣告不包含任何下列修飾詞： `virtual`， `override`，或`abstract`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-727">If the declaration includes the `private` modifier, then the declaration does not include any of the following modifiers: `virtual`, `override`, or `abstract`.</span></span>
*  <span data-ttu-id="6fe1a-728">如果宣告包含`sealed`修飾詞，則宣告也包含`override`修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-728">If the declaration includes the `sealed` modifier, then the declaration also includes the `override` modifier.</span></span>
*  <span data-ttu-id="6fe1a-729">如果宣告包含`partial`修飾詞，則它不包含任何下列修飾詞： `new`， `public`， `protected`， `internal`， `private`， `virtual`， `sealed`， `override``abstract`，或`extern`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-729">If the declaration includes the `partial` modifier, then it does not include any of the following modifiers: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override`, `abstract`, or `extern`.</span></span>

<span data-ttu-id="6fe1a-730">方法具有`async`修飾詞是非同步函式，並且遵循規則中所述[非同步函式](classes.md#async-functions)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-730">A method that has the `async` modifier is an async function and follows the rules described in [Async functions](classes.md#async-functions).</span></span>

<span data-ttu-id="6fe1a-731">*Return_type*方法的宣告會指定方法所計算並傳回值的型別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-731">The *return_type* of a method declaration specifies the type of the value computed and returned by the method.</span></span> <span data-ttu-id="6fe1a-732">*Return_type*是`void`如果方法沒有傳回值。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-732">The *return_type* is `void` if the method does not return a value.</span></span> <span data-ttu-id="6fe1a-733">如果宣告包含`partial`修飾詞，則傳回型別必須可`void`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-733">If the declaration includes the `partial` modifier, then the return type must be `void`.</span></span>

<span data-ttu-id="6fe1a-734">*Member_name*指定方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-734">The *member_name* specifies the name of the method.</span></span> <span data-ttu-id="6fe1a-735">除非該方法是明確的介面成員實作 ([明確介面成員實作](interfaces.md#explicit-interface-member-implementations))，則*member_name*是只要*識別碼*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-735">Unless the method is an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="6fe1a-736">明確介面成員實作，如*member_name*組成*interface_type*後面加上"`.`"和*識別碼*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-736">For an explicit interface member implementation, the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="6fe1a-737">選擇性*type_parameter_list*指定之方法的型別參數 ([型別參數](classes.md#type-parameters))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-737">The optional *type_parameter_list* specifies the type parameters of the method ([Type parameters](classes.md#type-parameters)).</span></span> <span data-ttu-id="6fe1a-738">如果*type_parameter_list*的方法指定***泛型方法***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-738">If a *type_parameter_list* is specified the method is a ***generic method***.</span></span> <span data-ttu-id="6fe1a-739">如果方法沒有`extern`修飾詞*type_parameter_list*不能指定。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-739">If the method has an `extern` modifier, a *type_parameter_list* cannot be specified.</span></span>

<span data-ttu-id="6fe1a-740">選擇性*formal_parameter_list*指定之方法的參數 ([方法參數](classes.md#method-parameters))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-740">The optional *formal_parameter_list* specifies the parameters of the method ([Method parameters](classes.md#method-parameters)).</span></span>

<span data-ttu-id="6fe1a-741">選擇性*type_parameter_constraints_clause*指定個別的型別參數的條件約束 ([類型參數條件約束](classes.md#type-parameter-constraints))，才可指定如果和*type_parameter_清單*也提供，而且此方法沒有`override`修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-741">The optional *type_parameter_constraints_clause*s specify constraints on individual type parameters ([Type parameter constraints](classes.md#type-parameter-constraints)) and may only be specified if a *type_parameter_list* is also supplied, and the method does not have an `override` modifier.</span></span>

<span data-ttu-id="6fe1a-742">*Return_type*和每個參考中的型別*formal_parameter_list*必須至少像方法本身那樣的方法 ([存取範圍條件約束](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="6fe1a-742">The *return_type* and each of the types referenced in the *formal_parameter_list* of a method must be at least as accessible as the method itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="6fe1a-743">*Method_body*是其中一個分號***陳述式主體***或是***運算式主體***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-743">The *method_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="6fe1a-744">陳述式主體所組成*區塊*，指定要叫用方法時執行的陳述式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-744">A statement body consists of a *block*, which specifies the statements to execute when the method is invoked.</span></span> <span data-ttu-id="6fe1a-745">運算式主體所組成`=>`後面接著*運算式*和分號則代表叫用方法時要執行的單一運算式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-745">An expression body consists of `=>` followed by an *expression* and a semicolon, and denotes a single expression to perform when the method is invoked.</span></span> 

<span data-ttu-id="6fe1a-746">針對`abstract`並`extern`方法*method_body*只包含一個分號。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-746">For `abstract` and `extern` methods, the *method_body* consists simply of a semicolon.</span></span> <span data-ttu-id="6fe1a-747">針對`partial`方法*method_body*可能包含分號、 區塊主體或運算式的主體。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-747">For `partial` methods the *method_body* may consist of either a semicolon, a block body or an expression body.</span></span> <span data-ttu-id="6fe1a-748">如需所有其他的方法*method_body*運算式主體或區塊的主體。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-748">For all other methods, the *method_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="6fe1a-749">如果*method_body*包含一個分號，然後宣告不能包含`async`修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-749">If the *method_body* consists of a semicolon, then the declaration may not include the `async` modifier.</span></span>

<span data-ttu-id="6fe1a-750">名稱、 型別參數清單和方法的型式參數清單定義的簽章 ([簽章和多載](basic-concepts.md#signatures-and-overloading)) 的方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-750">The name, the type parameter list and the formal parameter list of a method define the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the method.</span></span> <span data-ttu-id="6fe1a-751">具體而言，方法的簽章包含其名稱、 型別參數的數目、 修飾詞和其型式參數類型的數目。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-751">Specifically, the signature of a method consists of its name, the number of type parameters and the number, modifiers, and types of its formal parameters.</span></span> <span data-ttu-id="6fe1a-752">對於這些用途，不是由其名稱，但類型引數清單中之方法的序數位置識別之方法的型式參數的型別，就會發生的任何型別參數。傳回的型別不是方法簽章的一部分，也是類型參數或型式參數的名稱。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-752">For these purposes, any type parameter of the method that occurs in the type of a formal parameter is identified not by its name, but by its ordinal position in the type argument list of the method.The return type is not part of a method's signature, nor are the names of the type parameters or the formal parameters.</span></span>

<span data-ttu-id="6fe1a-753">方法的名稱必須與所有其他非方法相同的類別中宣告的名稱不同。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-753">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="6fe1a-754">此外，方法的簽章必須不同於在相同類別中，宣告的所有其他方法的簽章，而且在相同類別中宣告的兩種方法不能單獨由不同的簽章`ref`和`out`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-754">In addition, the signature of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>

<span data-ttu-id="6fe1a-755">方法的*type_parameter*會在整個範圍中的 s *method_declaration*，而且可用來在該範圍中的表單類型*return_type*， *method_body*，並*type_parameter_constraints_clause*s 中使用，但是*屬性*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-755">The method's *type_parameter*s are in scope throughout the *method_declaration*, and can be used to form types throughout that scope in *return_type*, *method_body*, and *type_parameter_constraints_clause*s but not in *attributes*.</span></span>

<span data-ttu-id="6fe1a-756">所有的型式參數和型別參數必須有不同的名稱。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-756">All formal parameters and type parameters must have different names.</span></span>

### <a name="method-parameters"></a><span data-ttu-id="6fe1a-757">方法參數</span><span class="sxs-lookup"><span data-stu-id="6fe1a-757">Method parameters</span></span>

<span data-ttu-id="6fe1a-758">方法的參數如果有的話，所宣告的方法*formal_parameter_list*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-758">The parameters of a method, if any, are declared by the method's *formal_parameter_list*.</span></span>

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

<span data-ttu-id="6fe1a-759">型式參數清單包含一或多個以逗號分隔的參數的可能只有最後一個*parameter_array*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-759">The formal parameter list consists of one or more comma-separated parameters of which only the last may be a *parameter_array*.</span></span>

<span data-ttu-id="6fe1a-760">A *fixed_parameter*組成的一組選擇性*屬性*([屬性](attributes.md))，選擇性`ref`，`out`或`this`修飾詞，*型別*，則*識別項*，以及一個選擇性*default_argument*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-760">A *fixed_parameter* consists of an optional set of *attributes* ([Attributes](attributes.md)), an optional `ref`, `out` or `this` modifier, a *type*, an *identifier* and an optional *default_argument*.</span></span> <span data-ttu-id="6fe1a-761">每個*fixed_parameter*宣告指定的型別，具有指定名稱的參數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-761">Each *fixed_parameter* declares a parameter of the given type with the given name.</span></span> <span data-ttu-id="6fe1a-762">`this`修飾詞將方法指定為擴充方法，而且只允許一種靜態方法的第一個參數上。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-762">The `this` modifier designates the method as an extension method and is only allowed on the first parameter of a static method.</span></span> <span data-ttu-id="6fe1a-763">擴充方法會更詳盡的說明[擴充方法](classes.md#extension-methods)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-763">Extension methods are further described in [Extension methods](classes.md#extension-methods).</span></span>

<span data-ttu-id="6fe1a-764">A *fixed_parameter*具有*default_argument*稱為***選擇性參數***，而*fixed_parameter*沒有*default_argument*是***必要的參數***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-764">A *fixed_parameter* with a *default_argument* is known as an ***optional parameter***, whereas a *fixed_parameter* without a *default_argument* is a ***required parameter***.</span></span> <span data-ttu-id="6fe1a-765">必要的參數中的選擇性參數之後可能不會出現*formal_parameter_list*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-765">A required parameter may not appear after an optional parameter in a *formal_parameter_list*.</span></span>

<span data-ttu-id="6fe1a-766">A`ref`或是`out`參數不能有*default_argument*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-766">A `ref` or `out` parameter cannot have a *default_argument*.</span></span> <span data-ttu-id="6fe1a-767">*運算式*中*default_argument*必須是下列其中之一：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-767">The *expression* in a *default_argument* must be one of the following:</span></span>

*  <span data-ttu-id="6fe1a-768">*constant_expression*</span><span class="sxs-lookup"><span data-stu-id="6fe1a-768">a *constant_expression*</span></span>
*  <span data-ttu-id="6fe1a-769">格式的運算式`new S()`其中`S`是實值類型</span><span class="sxs-lookup"><span data-stu-id="6fe1a-769">an expression of the form `new S()` where `S` is a value type</span></span>
*  <span data-ttu-id="6fe1a-770">格式的運算式`default(S)`其中`S`是實值類型</span><span class="sxs-lookup"><span data-stu-id="6fe1a-770">an expression of the form `default(S)` where `S` is a value type</span></span>

<span data-ttu-id="6fe1a-771">*運算式*必須由身分識別或可為 null 轉換成的類型參數的隱含轉換。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-771">The *expression* must be implicitly convertible by an identity or nullable conversion to the type of the parameter.</span></span>

<span data-ttu-id="6fe1a-772">如果選擇性的參數會出現在實作部分方法宣告 ([部分方法](classes.md#partial-methods))，是明確的介面成員實作 ([明確介面成員實作](interfaces.md#explicit-interface-member-implementations)) 或單一參數的索引子宣告 ([索引子](classes.md#indexers)) 編譯器應該提供一則警告，因為可以永遠不會允許可省略的引數的方式叫用這些成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-772">If optional parameters occur in an implementing partial method declaration ([Partial methods](classes.md#partial-methods)) , an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)) or in a single-parameter indexer declaration ([Indexers](classes.md#indexers)) the compiler should give a warning, since these members can never be invoked in a way that permits arguments to be omitted.</span></span>

<span data-ttu-id="6fe1a-773">A *parameter_array*組成的一組選擇性*屬性*([屬性](attributes.md))，則`params`修飾詞， *array_type*，並*識別碼*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-773">A *parameter_array* consists of an optional set of *attributes* ([Attributes](attributes.md)), a `params` modifier, an *array_type*, and an *identifier*.</span></span> <span data-ttu-id="6fe1a-774">參數陣列宣告具有指定名稱的指定的陣列類型的單一參數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-774">A parameter array declares a single parameter of the given array type with the given name.</span></span> <span data-ttu-id="6fe1a-775">*Array_type*參數的陣列必須是單一維度陣列型別 ([陣列型別](arrays.md#array-types))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-775">The *array_type* of a parameter array must be a single-dimensional array type ([Array types](arrays.md#array-types)).</span></span> <span data-ttu-id="6fe1a-776">在方法引動過程中，參數陣列可允許其中一個單一引數指定之陣列型別的指定，或允許零或多個陣列項目型別指定的引數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-776">In a method invocation, a parameter array permits either a single argument of the given array type to be specified, or it permits zero or more arguments of the array element type to be specified.</span></span> <span data-ttu-id="6fe1a-777">參數陣列中所述進一步[參數陣列](classes.md#parameter-arrays)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-777">Parameter arrays are described further in [Parameter arrays](classes.md#parameter-arrays).</span></span>

<span data-ttu-id="6fe1a-778">A *parameter_array*可能會發生後的選擇性參數，但不能有預設值--省略的引數*parameter_array*而是會導致建立空的陣列。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-778">A *parameter_array* may occur after an optional parameter, but cannot have a default value -- the omission of arguments for a *parameter_array* would instead result in the creation of an empty array.</span></span>

<span data-ttu-id="6fe1a-779">下列範例說明不同類型的參數：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-779">The following example illustrates different kinds of parameters:</span></span>
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

<span data-ttu-id="6fe1a-780">在  *formal_parameter_list*如`M`，`i`是必要的 ref 參數，`d`是必要的值參數， `b`， `s`，`o`和`t`是選擇性的值的參數和`a`是參數陣列。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-780">In the *formal_parameter_list* for `M`, `i` is a required ref parameter, `d` is a required value parameter, `b`, `s`, `o` and `t` are optional value parameters and `a` is a parameter array.</span></span>

<span data-ttu-id="6fe1a-781">在方法宣告會建立個別的宣告空間參數，型別參數和區域變數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-781">A method declaration creates a separate declaration space for parameters, type parameters and local variables.</span></span> <span data-ttu-id="6fe1a-782">型別參數清單和方法的型式參數清單和區域變數宣告中，名稱會導入此宣告空間*區塊*的方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-782">Names are introduced into this declaration space by the type parameter list and the formal parameter list of the method and by local variable declarations in the *block* of the method.</span></span> <span data-ttu-id="6fe1a-783">它是方法的宣告空間的兩個成員都有相同名稱的錯誤。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-783">It is an error for two members of a method declaration space to have the same name.</span></span> <span data-ttu-id="6fe1a-784">它是方法的宣告空間和巢狀的宣告空間，以包含項目具有相同名稱的本機變數的宣告空間錯誤。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-784">It is an error for the method declaration space and the local variable declaration space of a nested declaration space to contain elements with the same name.</span></span>

<span data-ttu-id="6fe1a-785">方法引動過程 ([方法引動過程](expressions.md#method-invocations)) 建立複本，該引動過程的特定、 型式參數和區域變數、 方法和引動過程的引數清單中，指派值或變數參考新建立的型式參數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-785">A method invocation ([Method invocations](expressions.md#method-invocations)) creates a copy, specific to that invocation, of the formal parameters and local variables of the method, and the argument list of the invocation assigns values or variable references to the newly created formal parameters.</span></span> <span data-ttu-id="6fe1a-786">內*區塊*可以在其識別項所參考的方法，型式參數*simple_name*運算式 ([簡單名稱](expressions.md#simple-names))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-786">Within the *block* of a method, formal parameters can be referenced by their identifiers in *simple_name* expressions ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="6fe1a-787">有四種型式參數：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-787">There are four kinds of formal parameters:</span></span>

*  <span data-ttu-id="6fe1a-788">值參數，而不需要任何修飾詞宣告。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-788">Value parameters, which are declared without any modifiers.</span></span>
*  <span data-ttu-id="6fe1a-789">參考參數，以宣告`ref`修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-789">Reference parameters, which are declared with the `ref` modifier.</span></span>
*  <span data-ttu-id="6fe1a-790">輸出參數，以宣告`out`修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-790">Output parameters, which are declared with the `out` modifier.</span></span>
*  <span data-ttu-id="6fe1a-791">參數陣列，以宣告`params`修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-791">Parameter arrays, which are declared with the `params` modifier.</span></span>

<span data-ttu-id="6fe1a-792">中所述[簽章和多載](basic-concepts.md#signatures-and-overloading)，則`ref`並`out`修飾詞是方法簽章的一部分，但`params`修飾詞不是。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-792">As described in [Signatures and overloading](basic-concepts.md#signatures-and-overloading), the `ref` and `out` modifiers are part of a method's signature, but the `params` modifier is not.</span></span>

#### <a name="value-parameters"></a><span data-ttu-id="6fe1a-793">值參數</span><span class="sxs-lookup"><span data-stu-id="6fe1a-793">Value parameters</span></span>

<span data-ttu-id="6fe1a-794">以任何修飾詞宣告的參數是實值參數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-794">A parameter declared with no modifiers is a value parameter.</span></span> <span data-ttu-id="6fe1a-795">實值參數相當於從對應的方法引動過程中提供的引數取得其初始值的區域變數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-795">A value parameter corresponds to a local variable that gets its initial value from the corresponding argument supplied in the method invocation.</span></span>

<span data-ttu-id="6fe1a-796">實值參數的型式參數時，為方法引動過程中對應的引數必須是可以隱含轉換的運算式 ([隱含轉換](conversions.md#implicit-conversions)) 成正式參數類型。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-796">When a formal parameter is a value parameter, the corresponding argument in a method invocation must be an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the formal parameter type.</span></span>

<span data-ttu-id="6fe1a-797">方法允許將新的值指派給實值參數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-797">A method is permitted to assign new values to a value parameter.</span></span> <span data-ttu-id="6fe1a-798">這類指派只會影響值參數所代表的本機儲存體位置，它們不影響實際方法引動過程中所提供的引數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-798">Such assignments only affect the local storage location represented by the value parameter—they have no effect on the actual argument given in the method invocation.</span></span>

#### <a name="reference-parameters"></a><span data-ttu-id="6fe1a-799">傳址參數</span><span class="sxs-lookup"><span data-stu-id="6fe1a-799">Reference parameters</span></span>

<span data-ttu-id="6fe1a-800">參數宣告`ref`修飾詞為參考參數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-800">A parameter declared with a `ref` modifier is a reference parameter.</span></span> <span data-ttu-id="6fe1a-801">不同於實值參數，參考參數不會建立新的存放位置。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-801">Unlike a value parameter, a reference parameter does not create a new storage location.</span></span> <span data-ttu-id="6fe1a-802">相反地，參考參數會表示為給定的方法引動過程中的引數變數相同的儲存位置。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-802">Instead, a reference parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="6fe1a-803">參考參數的型式參數時，對應的引數，方法引動過程中必須包含關鍵字`ref`後面接著*variable_reference* ([來判斷精確的規則明確指派](variables.md#precise-rules-for-determining-definite-assignment)) 的型式參數相同類型。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-803">When a formal parameter is a reference parameter, the corresponding argument in a method invocation must consist of the keyword `ref` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="6fe1a-804">之前將它傳遞做為參考參數，就必須明確指派變數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-804">A variable must be definitely assigned before it can be passed as a reference parameter.</span></span>

<span data-ttu-id="6fe1a-805">在方法中，參考參數一律會視為已明確指派。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-805">Within a method, a reference parameter is always considered definitely assigned.</span></span>

<span data-ttu-id="6fe1a-806">方法宣告為迭代器 ([迭代器](classes.md#iterators)) 不能有參考參數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-806">A method declared as an iterator ([Iterators](classes.md#iterators)) cannot have reference parameters.</span></span>

<span data-ttu-id="6fe1a-807">此範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-807">The example</span></span>
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
<span data-ttu-id="6fe1a-808">產生下列輸出</span><span class="sxs-lookup"><span data-stu-id="6fe1a-808">produces the output</span></span>
```
i = 2, j = 1
```

<span data-ttu-id="6fe1a-809">引動過程`Swap`中`Main`，`x`代表`i`並`y`代表`j`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-809">For the invocation of `Swap` in `Main`, `x` represents `i` and `y` represents `j`.</span></span> <span data-ttu-id="6fe1a-810">因此，引動過程的效果的值的交換`i`和`j`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-810">Thus, the invocation has the effect of swapping the values of `i` and `j`.</span></span>

<span data-ttu-id="6fe1a-811">在方法中會參考參數，您有多個代表相同的儲存位置的名稱。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-811">In a method that takes reference parameters it is possible for multiple names to represent the same storage location.</span></span> <span data-ttu-id="6fe1a-812">在範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-812">In the example</span></span>
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
<span data-ttu-id="6fe1a-813">引動過程`F`中`G`的參照傳遞`s`兩者`a`和`b`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-813">the invocation of `F` in `G` passes a reference to `s` for both `a` and `b`.</span></span> <span data-ttu-id="6fe1a-814">因此，針對該的引動過程中，而名稱`s`， `a`，並`b`全都參考相同的儲存體位置，而所有的三個指派修改執行個體欄位`s`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-814">Thus, for that invocation, the names `s`, `a`, and `b` all refer to the same storage location, and the three assignments all modify the instance field `s`.</span></span>

#### <a name="output-parameters"></a><span data-ttu-id="6fe1a-815">輸出參數</span><span class="sxs-lookup"><span data-stu-id="6fe1a-815">Output parameters</span></span>

<span data-ttu-id="6fe1a-816">參數宣告`out`修飾詞是一個 output 參數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-816">A parameter declared with an `out` modifier is an output parameter.</span></span> <span data-ttu-id="6fe1a-817">類似於參考參數，輸出參數不會建立新的存放位置。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-817">Similar to a reference parameter, an output parameter does not create a new storage location.</span></span> <span data-ttu-id="6fe1a-818">相反地，輸出參數會表示為給定的方法引動過程中的引數變數相同的儲存位置。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-818">Instead, an output parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="6fe1a-819">輸出參數的型式參數時，對應的引數，方法引動過程中必須包含關鍵字`out`後面接著*variable_reference* ([來判斷精確的規則明確指派](variables.md#precise-rules-for-determining-definite-assignment)) 的型式參數相同類型。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-819">When a formal parameter is an output parameter, the corresponding argument in a method invocation must consist of the keyword `out` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="6fe1a-820">變數必須未明確指派之前將它傳遞為 output 參數，但下列其中一個變數傳遞做為輸出參數的引動過程，變數會被視為已明確指派。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-820">A variable need not be definitely assigned before it can be passed as an output parameter, but following an invocation where a variable was passed as an output parameter, the variable is considered definitely assigned.</span></span>

<span data-ttu-id="6fe1a-821">在方法內，就像在本機變數，output 參數一開始被視為未設定，並且必須確實指派，才能使用其值。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-821">Within a method, just like a local variable, an output parameter is initially considered unassigned and must be definitely assigned before its value is used.</span></span>

<span data-ttu-id="6fe1a-822">在方法傳回之前，就必須明確指派方法的每個輸出參數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-822">Every output parameter of a method must be definitely assigned before the method returns.</span></span>

<span data-ttu-id="6fe1a-823">方法宣告為部分方法 ([部分方法](classes.md#partial-methods)) 或迭代器 ([迭代器](classes.md#iterators)) 不能有輸出參數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-823">A method declared as a partial method ([Partial methods](classes.md#partial-methods)) or an iterator ([Iterators](classes.md#iterators)) cannot have output parameters.</span></span>

<span data-ttu-id="6fe1a-824">輸出參數通常用於會產生多個傳回值的方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-824">Output parameters are typically used in methods that produce multiple return values.</span></span> <span data-ttu-id="6fe1a-825">例如: </span><span class="sxs-lookup"><span data-stu-id="6fe1a-825">For example:</span></span>
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

<span data-ttu-id="6fe1a-826">此範例會產生輸出：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-826">The example produces the output:</span></span>
```
c:\Windows\System\
hello.txt
```

<span data-ttu-id="6fe1a-827">請注意，`dir`並`name`變數可以是未指派，才能傳遞至`SplitPath`，而，它們會被視為已明確指派接在呼叫。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-827">Note that the `dir` and `name` variables can be unassigned before they are passed to `SplitPath`, and that they are considered definitely assigned following the call.</span></span>

#### <a name="parameter-arrays"></a><span data-ttu-id="6fe1a-828">參數陣列</span><span class="sxs-lookup"><span data-stu-id="6fe1a-828">Parameter arrays</span></span>

<span data-ttu-id="6fe1a-829">參數宣告`params`修飾詞是參數陣列。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-829">A parameter declared with a `params` modifier is a parameter array.</span></span> <span data-ttu-id="6fe1a-830">如果型式參數清單包括參數陣列，它必須是在清單中的最後一個參數，它必須是單一維度陣列型別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-830">If a formal parameter list includes a parameter array, it must be the last parameter in the list and it must be of a single-dimensional array type.</span></span> <span data-ttu-id="6fe1a-831">例如，類型`string[]`並`string[][]`可用來當做陣列的型別參數，但類型`string[,]`不可以。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-831">For example, the types `string[]` and `string[][]` can be used as the type of a parameter array, but the type `string[,]` can not.</span></span> <span data-ttu-id="6fe1a-832">不可以結合`params`修飾詞與修飾詞`ref`和`out`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-832">It is not possible to combine the `params` modifier with the modifiers `ref` and `out`.</span></span>

<span data-ttu-id="6fe1a-833">參數陣列會允許在下列其中一種方法的引動過程中指定的引數：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-833">A parameter array permits arguments to be specified in one of two ways in a method invocation:</span></span>

*  <span data-ttu-id="6fe1a-834">指定參數陣列可以是隱含轉換的單一運算式的引數 ([隱含轉換](conversions.md#implicit-conversions)) 為參數陣列型別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-834">The argument given for a parameter array can be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="6fe1a-835">在此情況下，參數陣列的行為很類似值的參數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-835">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="6fe1a-836">或者，引動過程可以指定為參數陣列，其中每個引數是可以隱含地轉換運算式的零或多個引數 ([隱含轉換](conversions.md#implicit-conversions)) 參數陣列的項目型別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-836">Alternatively, the invocation can specify zero or more arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="6fe1a-837">在此情況下，叫用建立具有相對應的引數的長度參數陣列型別的執行個體初始化為指定的引數的值，將陣列執行個體的項目，做為新建立的陣列執行個體的實際引數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-837">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="6fe1a-838">除了允許引數數目可變的引動過程中，參數陣列就相當於實值參數 ([實值參數](classes.md#value-parameters)) 型別相同。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-838">Except for allowing a variable number of arguments in an invocation, a parameter array is precisely equivalent to a value parameter ([Value parameters](classes.md#value-parameters)) of the same type.</span></span>

<span data-ttu-id="6fe1a-839">此範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-839">The example</span></span>
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
<span data-ttu-id="6fe1a-840">產生下列輸出</span><span class="sxs-lookup"><span data-stu-id="6fe1a-840">produces the output</span></span>
```
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

<span data-ttu-id="6fe1a-841">第一個引動過程`F`只會將陣列傳遞`a`做為值參數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-841">The first invocation of `F` simply passes the array `a` as a value parameter.</span></span> <span data-ttu-id="6fe1a-842">第二個引動過程`F`會自動建立四個元素`int[]`與的指定項目值陣列做為值參數的執行個體的傳遞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-842">The second invocation of `F` automatically creates a four-element `int[]` with the given element values and passes that array instance as a value parameter.</span></span> <span data-ttu-id="6fe1a-843">同樣地，第三個引動過程`F`會建立零個元素`int[]`，並將該執行個體傳遞做為值參數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-843">Likewise, the third invocation of `F` creates a zero-element `int[]` and passes that instance as a value parameter.</span></span> <span data-ttu-id="6fe1a-844">第二個和第三個引動過程是相當於寫入項目：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-844">The second and third invocations are precisely equivalent to writing:</span></span>
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

<span data-ttu-id="6fe1a-845">含有參數陣列的方法執行的多載解析時，可能適用的一般形式或在展開的形式 ([適用的函式成員](expressions.md#applicable-function-member))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-845">When performing overload resolution, a method with a parameter array may be applicable either in its normal form or in its expanded form ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="6fe1a-846">只有當方法的一般形式不適用，而且只有在具有相同的簽章和擴充形式的適用方法未在相同的型別中已經宣告使用展開的形式的方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-846">The expanded form of a method is available only if the normal form of the method is not applicable and only if an applicable method with the same signature as the expanded form is not already declared in the same type.</span></span>

<span data-ttu-id="6fe1a-847">此範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-847">The example</span></span>
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
<span data-ttu-id="6fe1a-848">產生下列輸出</span><span class="sxs-lookup"><span data-stu-id="6fe1a-848">produces the output</span></span>
```
F();
F(object[]);
F(object,object);
F(object[]);
F(object[]);
```

<span data-ttu-id="6fe1a-849">在範例中，兩個含有參數陣列的方法可能展開的形式中已包含此類別做為一般方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-849">In the example, two of the possible expanded forms of the method with a parameter array are already included in the class as regular methods.</span></span> <span data-ttu-id="6fe1a-850">這些擴充的形式因此時不會考慮執行多載解析，因此選取第一個和第三個方法引動過程的一般方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-850">These expanded forms are therefore not considered when performing overload resolution, and the first and third method invocations thus select the regular methods.</span></span> <span data-ttu-id="6fe1a-851">當類別宣告含有參數陣列的方法時，它不常見也包含一些一般方法為展開的形式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-851">When a class declares a method with a parameter array, it is not uncommon to also include some of the expanded forms as regular methods.</span></span> <span data-ttu-id="6fe1a-852">這樣可避免將陣列的配置會叫用擴充方法以參數陣列的形式時，就會發生的執行個體。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-852">By doing so it is possible to avoid the allocation of an array instance that occurs when an expanded form of a method with a parameter array is invoked.</span></span>

<span data-ttu-id="6fe1a-853">參數陣列的型別時`object[]`，潛在的模稜兩可，就會發生之方法的一般形式與單一的擴充的形式之間`object`參數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-853">When the type of a parameter array is `object[]`, a potential ambiguity arises between the normal form of the method and the expended form for a single `object` parameter.</span></span> <span data-ttu-id="6fe1a-854">模稜兩可的原因在於`object[]`本身是隱含轉換成類型`object`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-854">The reason for the ambiguity is that an `object[]` is itself implicitly convertible to type `object`.</span></span> <span data-ttu-id="6fe1a-855">模稜兩可不會有問題，不過，因為它可以解決插入所需的型別轉換。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-855">The ambiguity presents no problem, however, since it can be resolved by inserting a cast if needed.</span></span>

<span data-ttu-id="6fe1a-856">此範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-856">The example</span></span>
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
<span data-ttu-id="6fe1a-857">產生下列輸出</span><span class="sxs-lookup"><span data-stu-id="6fe1a-857">produces the output</span></span>
```
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

<span data-ttu-id="6fe1a-858">中的第一個和最後一個引動過程`F`的一般形式`F`適合，因為隱含轉換存在引數類型的參數類型 (兩者都是型別的`object[]`)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-858">In the first and last invocations of `F`, the normal form of `F` is applicable because an implicit conversion exists from the argument type to the parameter type (both are of type `object[]`).</span></span> <span data-ttu-id="6fe1a-859">因此，多載解析會選取的一般形式`F`，而且引數會當做一般的實值參數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-859">Thus, overload resolution selects the normal form of `F`, and the argument is passed as a regular value parameter.</span></span> <span data-ttu-id="6fe1a-860">在第二個和第三個引動過程的一般形式`F`不適用，因為沒有隱含轉換存在引數類型的參數型別 (型別`object`無法以隱含方式轉換為類型`object[]`)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-860">In the second and third invocations, the normal form of `F` is not applicable because no implicit conversion exists from the argument type to the parameter type (type `object` cannot be implicitly converted to type `object[]`).</span></span> <span data-ttu-id="6fe1a-861">不過的展開的表單`F`是適用的因此多載解析會選取它。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-861">However, the expanded form of `F` is applicable, so it is selected by overload resolution.</span></span> <span data-ttu-id="6fe1a-862">如此一來，一個項目`object[]`由引動過程，和指定的引數的值進行初始化陣列的單一項目 (而其本身是參考`object[]`)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-862">As a result, a one-element `object[]` is created by the invocation, and the single element of the array is initialized with the given argument value (which itself is a reference to an `object[]`).</span></span>

### <a name="static-and-instance-methods"></a><span data-ttu-id="6fe1a-863">靜態和執行個體方法</span><span class="sxs-lookup"><span data-stu-id="6fe1a-863">Static and instance methods</span></span>

<span data-ttu-id="6fe1a-864">當方法宣告包含`static`修飾詞，方法即稱為是靜態方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-864">When a method declaration includes a `static` modifier, that method is said to be a static method.</span></span> <span data-ttu-id="6fe1a-865">若未`static`修飾詞存在，方法要執行個體方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-865">When no `static` modifier is present, the method is said to be an instance method.</span></span>

<span data-ttu-id="6fe1a-866">靜態方法無法運作以特定的執行個體和它是編譯時期錯誤是指`this`中的靜態方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-866">A static method does not operate on a specific instance, and it is a compile-time error to refer to `this` in a static method.</span></span>

<span data-ttu-id="6fe1a-867">指定類別的執行個體上運作的執行個體方法，且該執行個體可存取做為`this`([這項存取](expressions.md#this-access))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-867">An instance method operates on a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="6fe1a-868">方法中的參考時*member_access* ([成員存取](expressions.md#member-access)) 的表單`E.M`時，如果`M`是靜態方法，`E`必須代表已包含類型`M`，而且如果`M`是執行個體方法，`E`必須代表執行個體的型別，其中`M`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-868">When a method is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static method, `E` must denote a type containing `M`, and if `M` is an instance method, `E` must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="6fe1a-869">靜態之間的差異和執行個體成員討論中進一步[靜態和執行個體成員](classes.md#static-and-instance-members)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-869">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-methods"></a><span data-ttu-id="6fe1a-870">虛擬方法</span><span class="sxs-lookup"><span data-stu-id="6fe1a-870">Virtual methods</span></span>

<span data-ttu-id="6fe1a-871">當執行個體方法宣告包含`virtual`修飾詞，方法即稱為是虛擬的方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-871">When an instance method declaration includes a `virtual` modifier, that method is said to be a virtual method.</span></span> <span data-ttu-id="6fe1a-872">若未`virtual`修飾詞存在，則此方法即為非虛擬方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-872">When no `virtual` modifier is present, the method is said to be a non-virtual method.</span></span>

<span data-ttu-id="6fe1a-873">非虛擬方法的實作是不變：是否在類別的執行個體上叫用方法的實作是相同的宣告或衍生類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-873">The implementation of a non-virtual method is invariant: The implementation is the same whether the method is invoked on an instance of the class in which it is declared or an instance of a derived class.</span></span> <span data-ttu-id="6fe1a-874">相反地，虛擬方法的實作可以取代由衍生類別中。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-874">In contrast, the implementation of a virtual method can be superseded by derived classes.</span></span> <span data-ttu-id="6fe1a-875">取代繼承虛擬方法的實作程序稱為***覆寫***該方法 ([覆寫方法](classes.md#override-methods))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-875">The process of superseding the implementation of an inherited virtual method is known as ***overriding*** that method ([Override methods](classes.md#override-methods)).</span></span>

<span data-ttu-id="6fe1a-876">中的虛擬方法引動過程中，***執行階段類型***引動過程的執行個體的位置會決定要叫用的實際方法實作。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-876">In a virtual method invocation, the ***run-time type*** of the instance for which that invocation takes place determines the actual method implementation to invoke.</span></span> <span data-ttu-id="6fe1a-877">在非虛擬方法引動過程中，***編譯時期型別***執行個體是決定因素。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-877">In a non-virtual method invocation, the ***compile-time type*** of the instance is the determining factor.</span></span> <span data-ttu-id="6fe1a-878">準確地說，當方法的名稱為`N`的引數清單叫用`A`編譯時期型別執行個體上`C`和執行階段型別`R`(其中`R`是`C`或衍生的類別從`C`)，叫用處理，如下所示：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-878">In precise terms, when a method named `N` is invoked with an argument list `A` on an instance with a compile-time type `C` and a run-time type `R` (where `R` is either `C` or a class derived from `C`), the invocation is processed as follows:</span></span>

*  <span data-ttu-id="6fe1a-879">首先，多載解析會套`C`， `N`，並`A`，以選取特定的方法`M`從一組方法中宣告，而且繼承`C`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-879">First, overload resolution is applied to `C`, `N`, and `A`, to select a specific method `M` from the set of methods declared in and inherited by `C`.</span></span> <span data-ttu-id="6fe1a-880">這中所述[方法引動過程](expressions.md#method-invocations)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-880">This is described in [Method invocations](expressions.md#method-invocations).</span></span>
*  <span data-ttu-id="6fe1a-881">然後，如果`M`為非虛擬方法，`M`叫用。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-881">Then, if `M` is a non-virtual method, `M` is invoked.</span></span>
*  <span data-ttu-id="6fe1a-882">否則，請`M`虛擬方法，且最具衍生性的實作`M`相對於`R`叫用。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-882">Otherwise, `M` is a virtual method, and the most derived implementation of `M` with respect to `R` is invoked.</span></span>

<span data-ttu-id="6fe1a-883">對於每種虛擬方法中宣告或繼承的類別存在***最常衍生的實作***相對於該類別的方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-883">For every virtual method declared in or inherited by a class, there exists a ***most derived implementation*** of the method with respect to that class.</span></span> <span data-ttu-id="6fe1a-884">虛擬方法的最具衍生性的實作`M`相對於類別`R`判斷方式如下：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-884">The most derived implementation of a virtual method `M` with respect to a class `R` is determined as follows:</span></span>

*  <span data-ttu-id="6fe1a-885">如果`R`包含簡介`virtual`deklarace `M`，則這是最具衍生性的實作`M`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-885">If `R` contains the introducing `virtual` declaration of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="6fe1a-886">否則，如果`R`包含`override`的`M`，則這是最具衍生性的實作`M`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-886">Otherwise, if `R` contains an `override` of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="6fe1a-887">最多衍生實作的否則為`M`相對於`R`等同於最具衍生性的實作`M`方面的直接基底類別`R`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-887">Otherwise, the most derived implementation of `M` with respect to `R` is the same as the most derived implementation of `M` with respect to the direct base class of `R`.</span></span>

<span data-ttu-id="6fe1a-888">下列範例說明虛擬和非虛擬方法之間的差異：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-888">The following example illustrates the differences between virtual and non-virtual methods:</span></span>
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

<span data-ttu-id="6fe1a-889">在範例中，`A`引進了非虛擬方法`F`和虛擬方法`G`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-889">In the example, `A` introduces a non-virtual method `F` and a virtual method `G`.</span></span> <span data-ttu-id="6fe1a-890">此類別`B`引進一個新的非虛擬方法`F`，因此隱藏已繼承`F`，也會覆寫繼承的方法和`G`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-890">The class `B` introduces a new non-virtual method `F`, thus hiding the inherited `F`, and also overrides the inherited method `G`.</span></span> <span data-ttu-id="6fe1a-891">此範例會產生輸出：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-891">The example produces the output:</span></span>
```
A.F
B.F
B.G
B.G
```

<span data-ttu-id="6fe1a-892">請注意，此陳述式`a.G()`叫用`B.G`，而非`A.G`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-892">Notice that the statement `a.G()` invokes `B.G`, not `A.G`.</span></span> <span data-ttu-id="6fe1a-893">這是因為執行階段類型的執行個體 (即`B`)，不執行個體的編譯時間類型 (也就是`A`)，判斷要叫用的實際方法實作。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-893">This is because the run-time type of the instance (which is `B`), not the compile-time type of the instance (which is `A`), determines the actual method implementation to invoke.</span></span>

<span data-ttu-id="6fe1a-894">方法可以隱藏繼承的方法，因為它可能會包含數個具有相同的簽章的虛擬方法的類別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-894">Because methods are allowed to hide inherited methods, it is possible for a class to contain several virtual methods with the same signature.</span></span> <span data-ttu-id="6fe1a-895">這不會出現發生模稜兩可的問題，因為所有但最具衍生性的方法會隱藏。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-895">This does not present an ambiguity problem, since all but the most derived method are hidden.</span></span> <span data-ttu-id="6fe1a-896">在範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-896">In the example</span></span>
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
<span data-ttu-id="6fe1a-897">`C`和`D`類別包含具有相同的簽章的兩個虛擬方法：其中一個所引入`A`所導入一個`C`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-897">the `C` and `D` classes contain two virtual methods with the same signature: The one introduced by `A` and the one introduced by `C`.</span></span> <span data-ttu-id="6fe1a-898">所導入的方法`C`隱藏繼承自方法`A`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-898">The method introduced by `C` hides the method inherited from `A`.</span></span> <span data-ttu-id="6fe1a-899">因此，在覆寫宣告`D`所引入的方法就會覆寫`C`，而且不可能`D`覆寫所引入的方法`A`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-899">Thus, the override declaration in `D` overrides the method introduced by `C`, and it is not possible for `D` to override the method introduced by `A`.</span></span> <span data-ttu-id="6fe1a-900">此範例會產生輸出：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-900">The example produces the output:</span></span>
```
B.F
B.F
D.F
D.F
```

<span data-ttu-id="6fe1a-901">請注意，它可以存取的執行個體來叫用虛擬方法隱藏的`D`透過更少衍生型別所在的方法不會隱藏。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-901">Note that it is possible to invoke the hidden virtual method by accessing an instance of `D` through a less derived type in which the method is not hidden.</span></span>

### <a name="override-methods"></a><span data-ttu-id="6fe1a-902">覆寫方法</span><span class="sxs-lookup"><span data-stu-id="6fe1a-902">Override methods</span></span>

<span data-ttu-id="6fe1a-903">當執行個體方法宣告包含`override`修飾詞，方法即稱為***覆寫方法***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-903">When an instance method declaration includes an `override` modifier, the method is said to be an ***override method***.</span></span> <span data-ttu-id="6fe1a-904">覆寫方法會覆寫具有相同的簽章的繼承虛擬方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-904">An override method overrides an inherited virtual method with the same signature.</span></span> <span data-ttu-id="6fe1a-905">虛擬方法宣告會導入新的方法，而覆寫方法宣告則是會提供現有已繼承之虛擬方法的新實作，來將該方法特製化。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-905">Whereas a virtual method declaration introduces a new method, an override method declaration specializes an existing inherited virtual method by providing a new implementation of that method.</span></span>

<span data-ttu-id="6fe1a-906">藉由覆寫的方法`override`宣告就所謂***覆寫基底方法***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-906">The method overridden by an `override` declaration is known as the ***overridden base method***.</span></span> <span data-ttu-id="6fe1a-907">覆寫方法`M`類別中宣告`C`，覆寫的基底方法會檢查每個基底類別型別來判斷`C`開始的直接基底類別型別，`C`並繼續每個後續直接基底類別型別，直到至少一個可存取的方法是位於其指定的基底類別型別中具有相同的簽章`M`之後替代型別引數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-907">For an override method `M` declared in a class `C`, the overridden base method is determined by examining each base class type of `C`, starting with the direct base class type of `C` and continuing with each successive direct base class type, until in a given base class type at least one accessible method is located which has the same signature as `M` after substitution of type arguments.</span></span> <span data-ttu-id="6fe1a-908">尋找覆寫的基底方法的目的，方法會被視為它是否可存取`public`，則為`protected`，則為`protected internal`，或者它是否`internal`並且在相同的程式，做為宣告`C`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-908">For the purposes of locating the overridden base method, a method is considered accessible if it is `public`, if it is `protected`, if it is `protected internal`, or if it is `internal` and declared in the same program as `C`.</span></span>

<span data-ttu-id="6fe1a-909">除非下列所有條件皆成立的覆寫宣告，就會發生編譯時期錯誤：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-909">A compile-time error occurs unless all of the following are true for an override declaration:</span></span>

*  <span data-ttu-id="6fe1a-910">覆寫的基底方法可以位於上面所述。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-910">An overridden base method can be located as described above.</span></span>
*  <span data-ttu-id="6fe1a-911">沒有一個這類覆寫基底方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-911">There is exactly one such overridden base method.</span></span> <span data-ttu-id="6fe1a-912">只有在基底類別型別是建構的型別其中的型別引數替代讓兩個方法的簽章相同，這項限制會沒有作用。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-912">This restriction has effect only if the base class type is a constructed type where the substitution of type arguments makes the signature of two methods the same.</span></span>
*  <span data-ttu-id="6fe1a-913">覆寫基底方法，是虛擬、 抽象或覆寫方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-913">The overridden base method is a virtual, abstract, or override method.</span></span> <span data-ttu-id="6fe1a-914">換句話說，靜態或非虛擬，不能覆寫的基底方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-914">In other words, the overridden base method cannot be static or non-virtual.</span></span>
*  <span data-ttu-id="6fe1a-915">覆寫的基底方法不是密封的方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-915">The overridden base method is not a sealed method.</span></span>
*  <span data-ttu-id="6fe1a-916">覆寫方法，並覆寫的基底方法有相同的傳回類型。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-916">The override method and the overridden base method have the same return type.</span></span>
*  <span data-ttu-id="6fe1a-917">覆寫宣告和覆寫的基底方法有相同的宣告存取範圍。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-917">The override declaration and the overridden base method have the same declared accessibility.</span></span> <span data-ttu-id="6fe1a-918">換句話說，覆寫宣告無法變更虛擬方法的存取範圍。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-918">In other words, an override declaration cannot change the accessibility of the virtual method.</span></span> <span data-ttu-id="6fe1a-919">不過，如果受到內部覆寫的基底方法宣告，且它不同的組件中非組件包含的覆寫方法，則覆寫方法的宣告存取範圍必須受到保護。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-919">However, if the overridden base method is protected internal and it is declared in a different assembly than the assembly containing the override method then the override method's declared accessibility must be protected.</span></span>
*  <span data-ttu-id="6fe1a-920">覆寫宣告未指定類型參數的條件約束子句。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-920">The override declaration does not specify type-parameter-constraints-clauses.</span></span> <span data-ttu-id="6fe1a-921">改為條件約束被繼承自基底方法，覆寫。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-921">Instead the constraints are inherited from the overridden base method.</span></span> <span data-ttu-id="6fe1a-922">請注意，覆寫方法中的類型參數的條件約束可能會由繼承的條件約束中的型別引數取代。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-922">Note that constraints that are type parameters in the overridden method may be replaced by type arguments in the inherited constraint.</span></span> <span data-ttu-id="6fe1a-923">這可能會導致不合法時明確指定，例如實值類型或密封的類型的條件約束。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-923">This can lead to constraints that are not legal when explicitly specified, such as value types or sealed types.</span></span>

<span data-ttu-id="6fe1a-924">下列範例會示範適用於泛型類別的覆寫規則的運作方式：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-924">The following example demonstrates how the overriding rules work for generic classes:</span></span>
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

<span data-ttu-id="6fe1a-925">覆寫宣告可以存取覆寫基底方法使用*base_access* ([基底存取](expressions.md#base-access))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-925">An override declaration can access the overridden base method using a *base_access* ([Base access](expressions.md#base-access)).</span></span> <span data-ttu-id="6fe1a-926">在範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-926">In the example</span></span>
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
<span data-ttu-id="6fe1a-927">`base.PrintFields()`中的引動過程`B`叫用`PrintFields`方法宣告中`A`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-927">the `base.PrintFields()` invocation in `B` invokes the `PrintFields` method declared in `A`.</span></span> <span data-ttu-id="6fe1a-928">A *base_access*停用虛擬引動過程的機制，並只將基底方法視為非虛擬方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-928">A *base_access* disables the virtual invocation mechanism and simply treats the base method as a non-virtual method.</span></span> <span data-ttu-id="6fe1a-929">在只有引動過程`B`已寫入`((A)this).PrintFields()`，它會以遞迴方式叫用`PrintFields`方法宣告中`B`，不是宣告中`A`，因為`PrintFields`是虛擬和執行階段類型的`((A)this)`是`B`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-929">Had the invocation in `B` been written `((A)this).PrintFields()`, it would recursively invoke the `PrintFields` method declared in `B`, not the one declared in `A`, since `PrintFields` is virtual and the run-time type of `((A)this)` is `B`.</span></span>

<span data-ttu-id="6fe1a-930">只要包括`override`修飾詞可以方法覆寫另一種方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-930">Only by including an `override` modifier can a method override another method.</span></span> <span data-ttu-id="6fe1a-931">在其他情況下，使用相同的簽章，做為繼承的方法，只會隱藏繼承的方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-931">In all other cases, a method with the same signature as an inherited method simply hides the inherited method.</span></span> <span data-ttu-id="6fe1a-932">在範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-932">In the example</span></span>
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
<span data-ttu-id="6fe1a-933">`F`方法中的`B`不包含`override`修飾詞，因此不會覆寫`F`中的方法`A`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-933">the `F` method in `B` does not include an `override` modifier and therefore does not override the `F` method in `A`.</span></span> <span data-ttu-id="6fe1a-934">相反地，`F`方法中的`B`中的方法會隱藏`A`，並且回報警告，因為宣告不包括`new`修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-934">Rather, the `F` method in `B` hides the method in `A`, and a warning is reported because the declaration does not include a `new` modifier.</span></span>

<span data-ttu-id="6fe1a-935">在範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-935">In the example</span></span>
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
<span data-ttu-id="6fe1a-936">`F`方法中的`B`隱藏虛擬`F`方法繼承自`A`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-936">the `F` method in `B` hides the virtual `F` method inherited from `A`.</span></span> <span data-ttu-id="6fe1a-937">因為新`F`中`B`具有私人存取權，其範圍只包含的類別主體`B`並不會延伸到`C`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-937">Since the new `F` in `B` has private access, its scope only includes the class body of `B` and does not extend to `C`.</span></span> <span data-ttu-id="6fe1a-938">因此，宣告`F`中`C`允許覆寫`F`繼承自`A`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-938">Therefore, the declaration of `F` in `C` is permitted to override the `F` inherited from `A`.</span></span>

### <a name="sealed-methods"></a><span data-ttu-id="6fe1a-939">密封的方法</span><span class="sxs-lookup"><span data-stu-id="6fe1a-939">Sealed methods</span></span>

<span data-ttu-id="6fe1a-940">當執行個體方法宣告包含`sealed`修飾詞，方法即為***密封方法***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-940">When an instance method declaration includes a `sealed` modifier, that method is said to be a ***sealed method***.</span></span> <span data-ttu-id="6fe1a-941">如果執行個體方法宣告包含`sealed`修飾詞，也必須包含`override`修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-941">If an instance method declaration includes the  `sealed` modifier, it must also include the `override` modifier.</span></span> <span data-ttu-id="6fe1a-942">使用`sealed`修飾詞可防止在衍生的類別進一步覆寫方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-942">Use of the `sealed` modifier prevents a derived class from further overriding the method.</span></span>

<span data-ttu-id="6fe1a-943">在範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-943">In the example</span></span>
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
<span data-ttu-id="6fe1a-944">類別`B`提供兩個覆寫的方法：`F`具有方法`sealed`修飾詞和`G`不的方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-944">the class `B` provides two override methods: an `F` method that has the `sealed` modifier and a `G` method that does not.</span></span> <span data-ttu-id="6fe1a-945">`B`使用。 密封`modifier`可防止`C`進一步地覆寫`F`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-945">`B`'s use of the sealed `modifier` prevents `C` from further overriding `F`.</span></span>

### <a name="abstract-methods"></a><span data-ttu-id="6fe1a-946">抽象方法</span><span class="sxs-lookup"><span data-stu-id="6fe1a-946">Abstract methods</span></span>

<span data-ttu-id="6fe1a-947">當執行個體方法宣告包含`abstract`修飾詞，方法即為***抽象方法***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-947">When an instance method declaration includes an `abstract` modifier, that method is said to be an ***abstract method***.</span></span> <span data-ttu-id="6fe1a-948">雖然抽象方法也會隱含地為虛擬方法，它不能有修飾詞`virtual`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-948">Although an abstract method is implicitly also a virtual method, it cannot have the modifier `virtual`.</span></span>

<span data-ttu-id="6fe1a-949">抽象方法宣告會引進一個新的虛擬方法，但不是提供該方法的實作。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-949">An abstract method declaration introduces a new virtual method but does not provide an implementation of that method.</span></span> <span data-ttu-id="6fe1a-950">相反地，非抽象衍生的類別必須提供自己的實作，藉由覆寫該方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-950">Instead, non-abstract derived classes are required to provide their own implementation by overriding that method.</span></span> <span data-ttu-id="6fe1a-951">抽象方法不提供任何實際的實作，因為*method_body*的抽象方法僅包含一個分號。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-951">Because an abstract method provides no actual implementation, the *method_body* of an abstract method simply consists of a semicolon.</span></span>

<span data-ttu-id="6fe1a-952">抽象方法宣告只允許出現在抽象類別 ([抽象類別](classes.md#abstract-classes))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-952">Abstract method declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="6fe1a-953">在範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-953">In the example</span></span>
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
<span data-ttu-id="6fe1a-954">`Shape`類別會定義可以自我繪製幾何形狀物件的抽象概念。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-954">the `Shape` class defines the abstract notion of a geometrical shape object that can paint itself.</span></span> <span data-ttu-id="6fe1a-955">`Paint`方法是抽象的因為沒有任何有意義的預設實作。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-955">The `Paint` method is abstract because there is no meaningful default implementation.</span></span> <span data-ttu-id="6fe1a-956">`Ellipse`並`Box`類別是具體`Shape`實作。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-956">The `Ellipse` and `Box` classes are concrete `Shape` implementations.</span></span> <span data-ttu-id="6fe1a-957">因為這些類別是抽象的它們必須覆寫`Paint`方法，並提供實際的實作。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-957">Because these classes are non-abstract, they are required to override the `Paint` method and provide an actual implementation.</span></span>

<span data-ttu-id="6fe1a-958">它是編譯時期錯誤*base_access* ([基底存取](expressions.md#base-access)) 來參考抽象方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-958">It is a compile-time error for a *base_access* ([Base access](expressions.md#base-access)) to reference an abstract method.</span></span> <span data-ttu-id="6fe1a-959">在範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-959">In the example</span></span>
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
<span data-ttu-id="6fe1a-960">編譯時期錯誤報告之`base.F()`引動過程因為它參考了抽象方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-960">a compile-time error is reported for the `base.F()` invocation because it references an abstract method.</span></span>

<span data-ttu-id="6fe1a-961">抽象方法宣告，並允許覆寫虛擬方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-961">An abstract method declaration is permitted to override a virtual method.</span></span> <span data-ttu-id="6fe1a-962">這允許強制重新實作的方法在衍生類別中，抽象類別，並會使原始方法的實作無法使用。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-962">This allows an abstract class to force re-implementation of the method in derived classes, and makes the original implementation of the method unavailable.</span></span> <span data-ttu-id="6fe1a-963">在範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-963">In the example</span></span>
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
<span data-ttu-id="6fe1a-964">類別`A`宣告虛擬方法，而類別`B`覆寫此方法使用的抽象方法與類別`C`覆寫抽象的方法，以提供它自己的實作。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-964">class `A` declares a virtual method, class `B` overrides this method with an abstract method, and class `C` overrides the abstract method to provide its own implementation.</span></span>

### <a name="external-methods"></a><span data-ttu-id="6fe1a-965">外部方法</span><span class="sxs-lookup"><span data-stu-id="6fe1a-965">External methods</span></span>

<span data-ttu-id="6fe1a-966">當方法宣告包含`extern`修飾詞，方法即為***外部方法***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-966">When a method declaration includes an `extern` modifier, that method is said to be an ***external method***.</span></span> <span data-ttu-id="6fe1a-967">外部方法會實作在外部，通常會使用 C# 以外的語言。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-967">External methods are implemented externally, typically using a language other than C#.</span></span> <span data-ttu-id="6fe1a-968">外部方法宣告未提供實際實作，因為*method_body*的外部方法僅包含一個分號。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-968">Because an external method declaration provides no actual implementation, the *method_body* of an external method simply consists of a semicolon.</span></span> <span data-ttu-id="6fe1a-969">外部方法不可以是泛型。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-969">An external method may not be generic.</span></span>

<span data-ttu-id="6fe1a-970">`extern`修飾詞通常用於搭配`DllImport`屬性 ([COM 與 Win32 元件的互通性](attributes.md#interoperation-with-com-and-win32-components))，可讓 Dll （動態連結程式庫） 所實作的外部方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-970">The `extern` modifier is typically used in conjunction with a `DllImport` attribute ([Interoperation with COM and Win32 components](attributes.md#interoperation-with-com-and-win32-components)), allowing external methods to be implemented by DLLs (Dynamic Link Libraries).</span></span> <span data-ttu-id="6fe1a-971">執行環境可能支援讓外部方法的實作可以提供其他機制。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-971">The execution environment may support other mechanisms whereby implementations of external methods can be provided.</span></span>

<span data-ttu-id="6fe1a-972">當外部方法包含`DllImport`屬性，方法宣告必須也會包含`static`修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-972">When an external method includes a `DllImport` attribute, the method declaration must also include a `static` modifier.</span></span> <span data-ttu-id="6fe1a-973">此範例示範如何使用`extern`修飾詞和`DllImport`屬性：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-973">This example demonstrates the use of the `extern` modifier and the `DllImport` attribute:</span></span>
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

### <a name="partial-methods-recap"></a><span data-ttu-id="6fe1a-974">部分方法 （回顧）</span><span class="sxs-lookup"><span data-stu-id="6fe1a-974">Partial methods (recap)</span></span>

<span data-ttu-id="6fe1a-975">當方法宣告包含`partial`修飾詞，方法即為***部分方法***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-975">When a method declaration includes a `partial` modifier, that method is said to be a ***partial method***.</span></span> <span data-ttu-id="6fe1a-976">部分方法只可以宣告為部分類型的成員 ([部分型別](classes.md#partial-types))，而且可能會有所限制的數目。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-976">Partial methods can only be declared as members of partial types ([Partial types](classes.md#partial-types)), and are subject to a number of restrictions.</span></span> <span data-ttu-id="6fe1a-977">部分方法進一步詳述於[部分方法](classes.md#partial-methods)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-977">Partial methods are further described in [Partial methods](classes.md#partial-methods).</span></span>

### <a name="extension-methods"></a><span data-ttu-id="6fe1a-978">擴充方法</span><span class="sxs-lookup"><span data-stu-id="6fe1a-978">Extension methods</span></span>

<span data-ttu-id="6fe1a-979">當方法的第一個參數包含`this`修飾詞，方法即為***擴充方法***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-979">When the first parameter of a method includes the `this` modifier, that method is said to be an ***extension method***.</span></span> <span data-ttu-id="6fe1a-980">只能在非泛型、 非巢狀的靜態類別中宣告擴充方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-980">Extension methods can only be declared in non-generic, non-nested static classes.</span></span> <span data-ttu-id="6fe1a-981">擴充方法的第一個參數可以有任何修飾詞以外`this`，以及參數類型不能是指標類型。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-981">The first parameter of an extension method can have no modifiers other than `this`, and the parameter type cannot be a pointer type.</span></span>

<span data-ttu-id="6fe1a-982">靜態類別會宣告兩個擴充方法的範例如下：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-982">The following is an example of a static class that declares two extension methods:</span></span>
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

<span data-ttu-id="6fe1a-983">擴充方法是一般的靜態方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-983">An extension method is a regular static method.</span></span> <span data-ttu-id="6fe1a-984">此外，當其封入的靜態類別是在範圍內時，擴充方法可以叫用執行個體方法引動過程語法 ([擴充方法引動過程](expressions.md#extension-method-invocations))，做為第一個引數使用的接收者運算式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-984">In addition, where its enclosing static class is in scope, an extension method can be invoked using instance method invocation syntax ([Extension method invocations](expressions.md#extension-method-invocations)), using the receiver expression as the first argument.</span></span>

<span data-ttu-id="6fe1a-985">下列程式會使用上面所宣告的擴充方法：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-985">The following program uses the extension methods declared above:</span></span>
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

<span data-ttu-id="6fe1a-986">`Slice`方法可用於`string[]`，而`ToInt32`方法位於`string`，因為它們已宣告為擴充方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-986">The `Slice` method is available on the `string[]`, and the `ToInt32` method is available on `string`, because they have been declared as extension methods.</span></span> <span data-ttu-id="6fe1a-987">程式的意義等同於下列，並使用一般的靜態方法呼叫：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-987">The meaning of the program is the same as the following, using ordinary static method calls:</span></span>
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

### <a name="method-body"></a><span data-ttu-id="6fe1a-988">方法主體</span><span class="sxs-lookup"><span data-stu-id="6fe1a-988">Method body</span></span>

<span data-ttu-id="6fe1a-989">*Method_body*方法的宣告包含區塊的主體、 運算式主體或分號。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-989">The *method_body* of a method declaration consists of either a block body, an expression body or a semicolon.</span></span>

<span data-ttu-id="6fe1a-990">***結果型別***是一種方法`void`的傳回型別是否`void`，或如果是非同步方法，而且傳回型別是`System.Threading.Tasks.Task`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-990">The ***result type*** of a method is `void` if the return type is `void`, or if the method is async and the return type is `System.Threading.Tasks.Task`.</span></span> <span data-ttu-id="6fe1a-991">否則，非同步方法的結果型別是其傳回類型和非同步方法的結果型別，以傳回型別`System.Threading.Tasks.Task<T>`是`T`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-991">Otherwise, the result type of a non-async method is its return type, and the result type of an async method with return type `System.Threading.Tasks.Task<T>` is `T`.</span></span>

<span data-ttu-id="6fe1a-992">當方法具有`void`型別和區塊的主體中，會造成`return`陳述式 ([return 陳述式](statements.md#the-return-statement)) 區塊中不允許以指定的運算式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-992">When a method has a `void` result type and a block body, `return` statements ([The return statement](statements.md#the-return-statement)) in the block are not permitted to specify an expression.</span></span> <span data-ttu-id="6fe1a-993">如果正常完成之區塊的 void 方法的執行 （也就是控制流程離開方法主體的結尾），方法只會傳回其目前的呼叫端。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-993">If execution of the block of a void method completes normally (that is, control flows off the end of the method body), that method simply returns to its current caller.</span></span>
    
<span data-ttu-id="6fe1a-994">當方法具有`void`結果與運算式主體中，運算式`E`必須*statement_expression*，且主體為完全相當於表單的區塊主體`{ E; }`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-994">When a method has a `void` result and an expression body, the expression `E` must be a *statement_expression*, and the body is exactly equivalent to a block body of the form `{ E; }`.</span></span>
    
<span data-ttu-id="6fe1a-995">當方法具有非 void 結果型別和區塊的主體，每個`return`區塊中的陳述式必須指定為隱含地轉換成的結果類型的運算式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-995">When a method has a non-void result type and a block body, each `return` statement in the block must specify an expression that is implicitly convertible to the result type.</span></span> <span data-ttu-id="6fe1a-996">值，傳回方法的區塊主體的端點不能連線。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-996">The endpoint of a block body of a value-returning method must not be reachable.</span></span> <span data-ttu-id="6fe1a-997">換句話說，在區塊內文傳回值的方法，控制項不允許方法主體的結尾。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-997">In other words, in a value-returning method with a block body, control is not permitted to flow off the end of the method body.</span></span>
    
<span data-ttu-id="6fe1a-998">當方法具有非 void 結果型別和運算式主體時，運算式必須隱含地轉換成的結果型別，而主體就完全相當於表單的區塊主體`{ return E; }`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-998">When a method has a non-void result type and an expression body, the expression must be implicitly convertible to the result type, and the body is exactly equivalent to a block body of the form `{ return E; }`.</span></span>
    
<span data-ttu-id="6fe1a-999">在範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-999">In the example</span></span>
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
<span data-ttu-id="6fe1a-1000">傳回值的`F`方法會產生編譯時期錯誤，因為控制項可以離開方法主體的結尾。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1000">the value-returning `F` method results in a compile-time error because control can flow off the end of the method body.</span></span> <span data-ttu-id="6fe1a-1001">`G`和`H`方法是否正確，因為所有可能的執行路徑結束於指定的傳回值的 return 陳述式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1001">The `G` and `H` methods are correct because all possible execution paths end in a return statement that specifies a return value.</span></span> <span data-ttu-id="6fe1a-1002">`I`方法是否正確，因為其主體，相當於只是單一 return 陳述式中的陳述式區塊。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1002">The `I` method is correct, because its body is equivalent to a statement block with just a single return statement in it.</span></span>

### <a name="method-overloading"></a><span data-ttu-id="6fe1a-1003">方法多載</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1003">Method overloading</span></span>

<span data-ttu-id="6fe1a-1004">方法多載解析規則所述[型別推斷](expressions.md#type-inference)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1004">The method overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="properties"></a><span data-ttu-id="6fe1a-1005">屬性</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1005">Properties</span></span>

<span data-ttu-id="6fe1a-1006">A***屬性***成員可存取物件或類別的特性。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1006">A ***property*** is a member that provides access to a characteristic of an object or a class.</span></span> <span data-ttu-id="6fe1a-1007">屬性的範例包括字型、 標題的視窗中，一位客戶的名稱的大小字串的長度等等。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1007">Examples of properties include the length of a string, the size of a font, the caption of a window, the name of a customer, and so on.</span></span> <span data-ttu-id="6fe1a-1008">屬性是欄位的自然延伸，兩者都具有具名成員相關聯的型別，且存取欄位和屬性的語法相同。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1008">Properties are a natural extension of fields—both are named members with associated types, and the syntax for accessing fields and properties is the same.</span></span> <span data-ttu-id="6fe1a-1009">不過，與欄位不同的是，屬性並不會指示儲存位置。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1009">However, unlike fields, properties do not denote storage locations.</span></span> <span data-ttu-id="6fe1a-1010">取而代之的是，屬性會有「存取子」，這些存取子會指定讀取或寫入其值時要執行的陳述式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1010">Instead, properties have ***accessors*** that specify the statements to be executed when their values are read or written.</span></span> <span data-ttu-id="6fe1a-1011">屬性會因此提供的機制，來將動作關聯的讀取和寫入物件的屬性;此外，它們允許這類屬性，來計算。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1011">Properties thus provide a mechanism for associating actions with the reading and writing of an object's attributes; furthermore, they permit such attributes to be computed.</span></span>

<span data-ttu-id="6fe1a-1012">屬性使用宣告*property_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1012">Properties are declared using *property_declaration*s:</span></span>

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

<span data-ttu-id="6fe1a-1013">A *property_declaration*可能包含一組*屬性*([屬性](attributes.md)) 和有效的四種存取修飾詞組合 ([存取修飾詞](classes.md#access-modifiers))，則`new`([的新修飾詞](classes.md#the-new-modifier))， `static` ([靜態和執行個體方法](classes.md#static-and-instance-methods))， `virtual` ([虛擬方法](classes.md#virtual-methods))， `override` ([覆寫方法](classes.md#override-methods))， `sealed` ([密封方法](classes.md#sealed-methods))， `abstract` ([抽象方法](classes.md#abstract-methods))，以及`extern` ([外部方法](classes.md#external-methods)) 修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1013">A *property_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="6fe1a-1014">屬性宣告受限於相同的方法宣告規則 ([方法](classes.md#methods)) 方面的修飾詞的有效組合。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1014">Property declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="6fe1a-1015">*型別*屬性的宣告會指定類型的宣告所引進的屬性和*member_name*指定屬性的名稱。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1015">The *type* of a property declaration specifies the type of the property introduced by the declaration, and the *member_name* specifies the name of the property.</span></span> <span data-ttu-id="6fe1a-1016">屬性是明確的介面成員實作，除非*member_name*是只要*識別項*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1016">Unless the property is an explicit interface member implementation, the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="6fe1a-1017">明確介面成員實作 ([明確介面成員實作](interfaces.md#explicit-interface-member-implementations))，則*member_name*組成*interface_type*後面加上"`.`」 和 「*識別碼*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1017">For an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="6fe1a-1018">*型別*必須至少像一樣地存取屬性本身的屬性 ([存取範圍條件約束](basic-concepts.md#accessibility-constraints))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1018">The *type* of a property must be at least as accessible as the property itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="6fe1a-1019">A *property_body*可能是組成***存取子主體***或是***運算式主體***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1019">A *property_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="6fe1a-1020">在存取子主體中， *accessor_declarations*，這必須括在 「`{`"和"`}`"權杖宣告存取子 ([存取子](classes.md#accessors)) 的屬性。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1020">In an accessor body,  *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="6fe1a-1021">存取子會指定可執行讀取和寫入的屬性相關聯的陳述式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1021">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="6fe1a-1022">組成的運算式主體`=>`後面接著*運算式*`E`分號就完全相當於陳述式主體和`{ get { return E; } }`，因此只可用來指定僅限 getter 和其中的 getter 結果為單一運算式所指定的屬性。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1022">An expression body consisting of `=>` followed by an *expression* `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only properties where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="6fe1a-1023">A *property_initializer*只可能會提供給自動實作的屬性 ([自動實作屬性](classes.md#automatically-implemented-properties))，並造成這類的基礎欄位的初始化具有所指定值的屬性*運算式*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1023">A *property_initializer* may only be given for an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)), and causes the initialization of the underlying field of such properties with the value given by the *expression*.</span></span>

<span data-ttu-id="6fe1a-1024">即使存取屬性的語法是相同的欄位，屬性不會分類為變數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1024">Even though the syntax for accessing a property is the same as that for a field, a property is not classified as a variable.</span></span> <span data-ttu-id="6fe1a-1025">因此，不可能將屬性當做`ref`或`out`引數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1025">Thus, it is not possible to pass a property as a `ref` or `out` argument.</span></span>

<span data-ttu-id="6fe1a-1026">當屬性宣告包含`extern`修飾詞，此屬性要***external 屬性***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1026">When a property declaration includes an `extern` modifier, the property is said to be an ***external property***.</span></span> <span data-ttu-id="6fe1a-1027">因為外部屬性宣告未不提供任何實際的實作，每個及其*accessor_declarations*只包含一個分號。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1027">Because an external property declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

### <a name="static-and-instance-properties"></a><span data-ttu-id="6fe1a-1028">靜態和執行個體的屬性</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1028">Static and instance properties</span></span>

<span data-ttu-id="6fe1a-1029">當屬性宣告包含`static`修飾詞，此屬性要***靜態屬性***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1029">When a property declaration includes a `static` modifier, the property is said to be a ***static property***.</span></span> <span data-ttu-id="6fe1a-1030">若未`static`修飾詞存在，則此屬性要***執行個體屬性***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1030">When no `static` modifier is present, the property is said to be an ***instance property***.</span></span>

<span data-ttu-id="6fe1a-1031">靜態屬性相關聯的特定執行個體，並不是編譯時期錯誤，以指向`this`在靜態屬性的存取子。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1031">A static property is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static property.</span></span>

<span data-ttu-id="6fe1a-1032">執行個體屬性與指定類別的執行個體相關聯，且該執行個體可存取為`this`([這項存取](expressions.md#this-access)) 中的該屬性存取子。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1032">An instance property is associated with a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that property.</span></span>

<span data-ttu-id="6fe1a-1033">屬性中的參考時*member_access* ([成員存取](expressions.md#member-access)) 的形式`E.M`時，如果`M`是靜態屬性，`E`必須代表已包含類型`M`，而且如果`M`是一個執行個體的屬性，E 必須代表執行個體的型別，其中`M`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1033">When a property is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static property, `E` must denote a type containing `M`, and if `M` is an instance property, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="6fe1a-1034">靜態之間的差異和執行個體成員討論中進一步[靜態和執行個體成員](classes.md#static-and-instance-members)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1034">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="accessors"></a><span data-ttu-id="6fe1a-1035">存取子</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1035">Accessors</span></span>

<span data-ttu-id="6fe1a-1036">*Accessor_declarations*屬性指定的讀取和寫入該屬性相關聯的可執行陳述式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1036">The *accessor_declarations* of a property specify the executable statements associated with reading and writing that property.</span></span>

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

<span data-ttu-id="6fe1a-1037">存取子宣告組成*get_accessor_declaration*，則*set_accessor_declaration*，或兩者。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1037">The accessor declarations consist of a *get_accessor_declaration*, a *set_accessor_declaration*, or both.</span></span> <span data-ttu-id="6fe1a-1038">每個存取子宣告所組成的語彙基元`get`或`set`後面接著選擇性*accessor_modifier*並*accessor_body*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1038">Each accessor declaration consists of the token `get` or `set` followed by an optional *accessor_modifier* and an *accessor_body*.</span></span>

<span data-ttu-id="6fe1a-1039">善用*accessor_modifier*s 受到下列限制：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1039">The use of *accessor_modifier*s is governed by the following restrictions:</span></span>

*  <span data-ttu-id="6fe1a-1040">*Accessor_modifier*可能不適用於在介面或明確介面成員實作。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1040">An *accessor_modifier* may not be used in an interface or in an explicit interface member implementation.</span></span>
*  <span data-ttu-id="6fe1a-1041">屬性或沒有任何索引子`override`修飾詞， *accessor_modifier*屬性或索引子同時具有時，才允許`get`和`set`存取子，然後才允許的其中之一，存取子。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1041">For a property or indexer that has no `override` modifier, an *accessor_modifier* is permitted only if the property or indexer has both a `get` and `set` accessor, and then is permitted only on one of those accessors.</span></span>
*  <span data-ttu-id="6fe1a-1042">屬性或包含的索引子`override`修飾詞，存取子必須符合*accessor_modifier*、 如果有的話，要覆寫的存取子。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1042">For a property or indexer that includes an `override` modifier, an accessor must match the *accessor_modifier*, if any, of the accessor being overridden.</span></span>
*  <span data-ttu-id="6fe1a-1043">*Accessor_modifier*必須宣告更嚴格限制比屬性或索引子本身的宣告存取範圍的存取範圍。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1043">The *accessor_modifier* must declare an accessibility that is strictly more restrictive than the declared accessibility of the property or indexer itself.</span></span> <span data-ttu-id="6fe1a-1044">若要明確地說：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1044">To be precise:</span></span>
   * <span data-ttu-id="6fe1a-1045">如果屬性或索引子已宣告存取範圍`public`，則*accessor_modifier*可以是`protected internal`， `internal`， `protected`，或`private`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1045">If the property or indexer has a declared accessibility of `public`, the *accessor_modifier* may be either `protected internal`, `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="6fe1a-1046">如果屬性或索引子已宣告存取範圍`protected internal`，則*accessor_modifier*可以是`internal`， `protected`，或`private`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1046">If the property or indexer has a declared accessibility of `protected internal`, the *accessor_modifier* may be either `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="6fe1a-1047">如果屬性或索引子已宣告存取範圍`internal`或是`protected`，則*accessor_modifier*必須是`private`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1047">If the property or indexer has a declared accessibility of `internal` or `protected`, the *accessor_modifier* must be `private`.</span></span>
   * <span data-ttu-id="6fe1a-1048">如果屬性或索引子已宣告存取範圍`private`，沒有*accessor_modifier*可用。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1048">If the property or indexer has a declared accessibility of `private`, no *accessor_modifier* may be used.</span></span>

<span data-ttu-id="6fe1a-1049">針對`abstract`並`extern`屬性， *accessor_body*指定每個存取子是只是分號。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1049">For `abstract` and `extern` properties, the *accessor_body* for each accessor specified is simply a semicolon.</span></span> <span data-ttu-id="6fe1a-1050">非抽象，非 extern 屬性可能會有每個*accessor_body*是分號，在此情況下很***自動實作屬性***([自動實作的屬性](classes.md#automatically-implemented-properties)).</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1050">A non-abstract, non-extern property may have each *accessor_body* be a semicolon, in which case it is an ***automatically implemented property*** ([Automatically implemented properties](classes.md#automatically-implemented-properties)).</span></span> <span data-ttu-id="6fe1a-1051">自動實作的屬性必須有至少一個 get 存取子。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1051">An automatically implemented property must have at least a get accessor.</span></span> <span data-ttu-id="6fe1a-1052">任何其他非抽象，非外部屬性，存取子*accessor_body*是*區塊*指定對應的存取子會叫用時要執行的陳述式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1052">For the accessors of any other non-abstract, non-extern property, the *accessor_body* is a *block* which specifies the statements to be executed when the corresponding accessor is invoked.</span></span>

<span data-ttu-id="6fe1a-1053">A`get`存取子對應到無參數的方法具有傳回值的屬性型別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1053">A `get` accessor corresponds to a parameterless method with a return value of the property type.</span></span> <span data-ttu-id="6fe1a-1054">在運算式中，參考屬性時，做為目標的指派，除非`get`屬性存取子會叫用來計算屬性的值 ([運算式的值](expressions.md#values-of-expressions))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1054">Except as the target of an assignment, when a property is referenced in an expression, the `get` accessor of the property is invoked to compute the value of the property ([Values of expressions](expressions.md#values-of-expressions)).</span></span> <span data-ttu-id="6fe1a-1055">主體`get`存取子必須符合的規則，傳回值的方法中所述[方法主體](classes.md#method-body)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1055">The body of a `get` accessor must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="6fe1a-1056">特別是，所有`return`陳述式的主體中`get`存取子必須指定為隱含轉換為屬性類型的運算式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1056">In particular, all `return` statements in the body of a `get` accessor must specify an expression that is implicitly convertible to the property type.</span></span> <span data-ttu-id="6fe1a-1057">此外，端點`get`存取子不能連線。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1057">Furthermore, the endpoint of a `get` accessor must not be reachable.</span></span>

<span data-ttu-id="6fe1a-1058">A`set`存取子會對應至具有單一值參數的屬性類型的方法和`void`傳回型別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1058">A `set` accessor corresponds to a method with a single value parameter of the property type and a `void` return type.</span></span> <span data-ttu-id="6fe1a-1059">隱含參數`set`存取子的名稱永遠`value`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1059">The implicit parameter of a `set` accessor is always named `value`.</span></span> <span data-ttu-id="6fe1a-1060">當指派的目標為參考屬性 ([指派運算子](expressions.md#assignment-operators))，或作為運算元`++`或`--`([後置遞增和遞減運算子](expressions.md#postfix-increment-and-decrement-operators)， [前置遞增和遞減運算子](expressions.md#prefix-increment-and-decrement-operators))，則`set`的引數叫用存取子 (其值是指派的右邊的運算元`++`或`--`運算子)，提供新值 ([簡單指派](expressions.md#simple-assignment))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1060">When a property is referenced as the target of an assignment ([Assignment operators](expressions.md#assignment-operators)), or as the operand of `++` or `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators), [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), the `set` accessor is invoked with an argument (whose value is that of the right-hand side of the assignment or the operand of the `++` or `--` operator) that provides the new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="6fe1a-1061">主體`set`存取子必須符合的規則`void`方法中所述[方法主體](classes.md#method-body)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1061">The body of a `set` accessor must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="6fe1a-1062">特別是，`return`中的陳述式`set`存取子主體不允許指定的運算式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1062">In particular, `return` statements in the `set` accessor body are not permitted to specify an expression.</span></span> <span data-ttu-id="6fe1a-1063">由於`set`存取子會隱含地擁有名為的參數`value`，它是本機變數或常數宣告中的編譯時期錯誤`set`具有該名稱的存取子。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1063">Since a `set` accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declaration in a `set` accessor to have that name.</span></span>

<span data-ttu-id="6fe1a-1064">根據存在與否`get`和`set`存取子，屬性分類如下：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1064">Based on the presence or absence of the `get` and `set` accessors, a property is classified as follows:</span></span>

*  <span data-ttu-id="6fe1a-1065">此屬性，同時包含`get`存取子和`set`存取子要***讀寫***屬性。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1065">A property that includes both a `get` accessor and a `set` accessor is said to be a ***read-write*** property.</span></span>
*  <span data-ttu-id="6fe1a-1066">此屬性，只有`get`存取子要***唯讀***屬性。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1066">A property that has only a `get` accessor is said to be a ***read-only*** property.</span></span> <span data-ttu-id="6fe1a-1067">它是唯讀的屬性，若要指派的目標的編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1067">It is a compile-time error for a read-only property to be the target of an assignment.</span></span>
*  <span data-ttu-id="6fe1a-1068">此屬性，只有`set`存取子要***唯寫***屬性。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1068">A property that has only a `set` accessor is said to be a ***write-only*** property.</span></span> <span data-ttu-id="6fe1a-1069">除了作為指派目標，它是參考運算式中的唯寫屬性的編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1069">Except as the target of an assignment, it is a compile-time error to reference a write-only property in an expression.</span></span>

<span data-ttu-id="6fe1a-1070">在範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1070">In the example</span></span>
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
<span data-ttu-id="6fe1a-1071">`Button`控制宣告公用`Caption`屬性。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1071">the `Button` control declares a public `Caption` property.</span></span> <span data-ttu-id="6fe1a-1072">`get`存取子`Caption`屬性會傳回儲存在私用的字串`caption`欄位。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1072">The `get` accessor of the `Caption` property returns the string stored in the private `caption` field.</span></span> <span data-ttu-id="6fe1a-1073">`set`存取子會檢查新的值是否不同於目前的值，如果是的話，它會儲存新的值，並重新繪製控制項。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1073">The `set` accessor checks if the new value is different from the current value, and if so, it stores the new value and repaints the control.</span></span> <span data-ttu-id="6fe1a-1074">屬性通常會遵循的模式，如上所示：`get`存取子只會傳回值，這個值儲存在私用欄位，而`set`存取子會修改該私用欄位，然後執行 完整更新物件的狀態所需的任何其他動作。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1074">Properties often follow the pattern shown above: The `get` accessor simply returns a value stored in a private field, and the `set` accessor modifies that private field and then performs any additional actions required to fully update the state of the object.</span></span>

<span data-ttu-id="6fe1a-1075">給定`Button`上述的類別，以下是範例使用`Caption`屬性：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1075">Given the `Button` class above, the following is an example of use of the `Caption` property:</span></span>
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

<span data-ttu-id="6fe1a-1076">在這裡，`set`存取子會將值指派給屬性，叫用和`get`存取子會叫用參考運算式中的屬性。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1076">Here, the `set` accessor is invoked by assigning a value to the property, and the `get` accessor is invoked by referencing the property in an expression.</span></span>

<span data-ttu-id="6fe1a-1077">`get`和`set`屬性存取子不是不同的成員，而且不可以宣告子的屬性分開。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1077">The `get` and `set` accessors of a property are not distinct members, and it is not possible to declare the accessors of a property separately.</span></span> <span data-ttu-id="6fe1a-1078">因此，不可能的讀 / 寫屬性的兩個存取子具有不同的存取範圍。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1078">As such, it is not possible for the two accessors of a read-write property to have different accessibility.</span></span> <span data-ttu-id="6fe1a-1079">此範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1079">The example</span></span>
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
<span data-ttu-id="6fe1a-1080">未宣告的單一讀寫屬性。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1080">does not declare a single read-write property.</span></span> <span data-ttu-id="6fe1a-1081">相反地，它會宣告兩個具有相同名稱，其中一個唯讀的屬性，另一個唯寫。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1081">Rather, it declares two properties with the same name, one read-only and one write-only.</span></span> <span data-ttu-id="6fe1a-1082">因為在相同類別中宣告的兩個成員不能有相同的名稱，此範例會導致發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1082">Since two members declared in the same class cannot have the same name, the example causes a compile-time error to occur.</span></span>

<span data-ttu-id="6fe1a-1083">當在衍生的類別繼承的屬性名稱相同的宣告屬性時，衍生的屬性會隱藏繼承的屬性，相對於讀取和寫入。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1083">When a derived class declares a property by the same name as an inherited property, the derived property hides the inherited property with respect to both reading and writing.</span></span> <span data-ttu-id="6fe1a-1084">在範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1084">In the example</span></span>
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
<span data-ttu-id="6fe1a-1085">`P`中的屬性`B`隱藏`P`屬性中的`A`相對於讀取和寫入。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1085">the `P` property in `B` hides the `P` property in `A` with respect to both reading and writing.</span></span> <span data-ttu-id="6fe1a-1086">因此，在陳述式</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1086">Thus, in the statements</span></span>
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
<span data-ttu-id="6fe1a-1087">指派給`b.P`會導致編譯時期錯誤回報，因為唯讀`P`中的屬性`B`隱藏唯`P`中的屬性`A`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1087">the assignment to `b.P` causes a compile-time error to be reported, since the read-only `P` property in `B` hides the write-only `P` property in `A`.</span></span> <span data-ttu-id="6fe1a-1088">不過請注意，型別轉換，可用來存取的隱藏`P`屬性。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1088">Note, however, that a cast can be used to access the hidden `P` property.</span></span>

<span data-ttu-id="6fe1a-1089">不同於公用欄位，屬性會提供物件的內部狀態和其公用介面之間的分隔。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1089">Unlike public fields, properties provide a separation between an object's internal state and its public interface.</span></span> <span data-ttu-id="6fe1a-1090">此範例，請考慮：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1090">Consider the example:</span></span>
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

<span data-ttu-id="6fe1a-1091">在這裡，`Label`類別會使用兩個`int`欄位`x`和`y`來儲存它的位置。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1091">Here, the `Label` class uses two `int` fields, `x` and `y`, to store its location.</span></span> <span data-ttu-id="6fe1a-1092">位置是公開公開兩種形式`X`並`Y`屬性並做為`Location`型別的屬性`Point`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1092">The location is publicly exposed both as an `X` and a `Y` property and as a `Location` property of type `Point`.</span></span> <span data-ttu-id="6fe1a-1093">如果在未來的版本`Label`，會變得更方便地儲存位置設定為`Point`就內部而言，進行變更，而不會影響該類別的公用介面：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1093">If, in a future version of `Label`, it becomes more convenient to store the location as a `Point` internally, the change can be made without affecting the public interface of the class:</span></span>
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

<span data-ttu-id="6fe1a-1094">有`x`並`y`改為已`public readonly`欄位，就會進行這類變更不可能`Label`類別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1094">Had `x` and `y` instead been `public readonly` fields, it would have been impossible to make such a change to the `Label` class.</span></span>

<span data-ttu-id="6fe1a-1095">公開透過屬性的狀態不一定比直接公開欄位會比較有效率。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1095">Exposing state through properties is not necessarily any less efficient than exposing fields directly.</span></span> <span data-ttu-id="6fe1a-1096">特別的是，當屬性為非虛擬的且包含只有少量的程式碼，執行環境可能會取代呼叫存取子的存取子的實際程式碼。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1096">In particular, when a property is non-virtual and contains only a small amount of code, the execution environment may replace calls to accessors with the actual code of the accessors.</span></span> <span data-ttu-id="6fe1a-1097">此程序稱為***內嵌***，以及它可以讓屬性存取的效率不如欄位存取權，但會保留屬性的較大彈性。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1097">This process is known as ***inlining***, and it makes property access as efficient as field access, yet preserves the increased flexibility of properties.</span></span>

<span data-ttu-id="6fe1a-1098">因為叫用`get`存取子是概念上相當於讀取欄位的值，則會視為不良程式設計樣式`get`存取子具有可預見的副作用。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1098">Since invoking a `get` accessor is conceptually equivalent to reading the value of a field, it is considered bad programming style for `get` accessors to have observable side-effects.</span></span> <span data-ttu-id="6fe1a-1099">在範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1099">In the example</span></span>
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
<span data-ttu-id="6fe1a-1100">值`Next`屬性相依於先前存取該屬性的次數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1100">the value of the `Next` property depends on the number of times the property has previously been accessed.</span></span> <span data-ttu-id="6fe1a-1101">因此，存取的屬性會產生副作用，而屬性應該改為實作做為方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1101">Thus, accessing the property produces an observable side-effect, and the property should be implemented as a method instead.</span></span>

<span data-ttu-id="6fe1a-1102">「 沒有任何副作用 」 慣例`get`存取子不表示`get`一律應該寫入存取子只會傳回儲存在欄位的值。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1102">The "no side-effects" convention for `get` accessors doesn't mean that `get` accessors should always be written to simply return values stored in fields.</span></span> <span data-ttu-id="6fe1a-1103">事實上，`get`通常存取多個欄位，或叫用方法計算的屬性值的存取子。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1103">Indeed, `get` accessors often compute the value of a property by accessing multiple fields or invoking methods.</span></span> <span data-ttu-id="6fe1a-1104">不過，設計得當`get`存取子會執行任何動作都可觀察的變更會導致物件的狀態。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1104">However, a properly designed `get` accessor performs no actions that cause observable changes in the state of the object.</span></span>

<span data-ttu-id="6fe1a-1105">屬性可用來延遲初始設定的資源之前先參考它的時刻。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1105">Properties can be used to delay initialization of a resource until the moment it is first referenced.</span></span> <span data-ttu-id="6fe1a-1106">例如: </span><span class="sxs-lookup"><span data-stu-id="6fe1a-1106">For example:</span></span>
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

<span data-ttu-id="6fe1a-1107">`Console`類別包含三個屬性， `In`， `Out`，和`Error`，分別代表標準輸入、 輸出和錯誤的裝置。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1107">The `Console` class contains three properties, `In`, `Out`, and `Error`, that represent the standard input, output, and error devices, respectively.</span></span> <span data-ttu-id="6fe1a-1108">藉由公開為屬性，這些成員`Console`類別可延遲其初始化，直到實際使用。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1108">By exposing these members as properties, the `Console` class can delay their initialization until they are actually used.</span></span> <span data-ttu-id="6fe1a-1109">例如，在第一次參考時`Out`屬性，如下所示</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1109">For example, upon first referencing the `Out` property, as in</span></span>
```csharp
Console.Out.WriteLine("hello, world");
```
<span data-ttu-id="6fe1a-1110">基礎`TextWriter`會建立輸出裝置。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1110">the underlying `TextWriter` for the output device is created.</span></span> <span data-ttu-id="6fe1a-1111">但是，如果應用程式會參考在沒有`In`和`Error`屬性，則沒有任何物件會針對這些裝置。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1111">But if the application makes no reference to the `In` and `Error` properties, then no objects are created for those devices.</span></span>

### <a name="automatically-implemented-properties"></a><span data-ttu-id="6fe1a-1112">自動實作的屬性</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1112">Automatically implemented properties</span></span>

<span data-ttu-id="6fe1a-1113">自動實作的屬性 (或***auto 屬性***簡稱)，是以分號專用存取子主體的非抽象非外部屬性。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1113">An automatically implemented property (or ***auto-property*** for short), is a non-abstract non-extern property with semicolon-only accessor bodies.</span></span> <span data-ttu-id="6fe1a-1114">Auto 屬性必須有 get 存取子，而且可以選擇性地包含 set 存取子。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1114">Auto-properties must have a get accessor and can optionally have a set accessor.</span></span>

<span data-ttu-id="6fe1a-1115">為自動實作屬性指定屬性時，隱藏的支援欄位會自動提供給屬性，並實作存取子來讀取和寫入至該支援欄位。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1115">When a property is specified as an automatically implemented property, a hidden backing field is automatically available for the property, and the accessors are implemented to read from and write to that backing field.</span></span> <span data-ttu-id="6fe1a-1116">如果自動屬性沒有 set 存取子，支援欄位會被視為`readonly`([唯讀欄位](classes.md#readonly-fields))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1116">If the auto-property has no set accessor, the backing field is considered `readonly` ([Readonly fields](classes.md#readonly-fields)).</span></span> <span data-ttu-id="6fe1a-1117">就像`readonly` 欄位中，僅限 getter 的 auto 屬性也可以指派至封入類別的建構函式主體中。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1117">Just like a `readonly` field, a getter-only auto-property can also be assigned to in the body of a constructor of the enclosing class.</span></span> <span data-ttu-id="6fe1a-1118">這類指派則會直接對唯讀支援欄位的屬性。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1118">Such an assignment assigns directly to the readonly backing field of the property.</span></span>

<span data-ttu-id="6fe1a-1119">Auto 屬性可能會有*property_initializer*，這會直接套用至做為支援欄位*variable_initializer* ([區域變數初始設定式](classes.md#variable-initializers)).</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1119">An auto-property may optionally have a *property_initializer*, which is applied directly to the backing field as a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)).</span></span>

<span data-ttu-id="6fe1a-1120">下列範例：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1120">The following example:</span></span>
```csharp
public class Point {
    public int X { get; set; } = 0;
    public int Y { get; set; } = 0;
}
```
<span data-ttu-id="6fe1a-1121">相當於下列宣告：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1121">is equivalent to the following declaration:</span></span>
```csharp
public class Point {
    private int __x = 0;
    private int __y = 0;
    public int X { get { return __x; } set { __x = value; } }
    public int Y { get { return __y; } set { __y = value; } }
}
```

<span data-ttu-id="6fe1a-1122">下列範例：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1122">The following example:</span></span>
```csharp
public class ReadOnlyPoint
{
    public int X { get; }
    public int Y { get; }
    public ReadOnlyPoint(int x, int y) { X = x; Y = y; }
}
```
<span data-ttu-id="6fe1a-1123">相當於下列宣告：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1123">is equivalent to the following declaration:</span></span>
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

<span data-ttu-id="6fe1a-1124">請注意，指派給唯讀欄位合法的因為它們發生在建構函式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1124">Notice that the assignments to the readonly field are legal, because they occur within the constructor.</span></span>


### <a name="accessibility"></a><span data-ttu-id="6fe1a-1125">協助工具選項</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1125">Accessibility</span></span>

<span data-ttu-id="6fe1a-1126">如果存取子已*accessor_modifier*，存取範圍定義域 ([存取範圍定義域](basic-concepts.md#accessibility-domains)) 的存取子用來決定的宣告存取範圍*accessor_modifier*.</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1126">If an accessor has an *accessor_modifier*, the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the accessor is determined using the declared accessibility of the *accessor_modifier*.</span></span> <span data-ttu-id="6fe1a-1127">如果存取子沒有*accessor_modifier*，存取子的存取範圍定義域由屬性或索引子的宣告存取範圍。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1127">If an accessor does not have an *accessor_modifier*, the accessibility domain of the accessor is determined from the declared accessibility of the property or indexer.</span></span>

<span data-ttu-id="6fe1a-1128">出現與否*accessor_modifier*永遠不會影響成員查閱 ([運算子](expressions.md#operators)) 或多載解析 ([多載解析](expressions.md#overload-resolution))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1128">The presence of an *accessor_modifier* never affects member lookup ([Operators](expressions.md#operators)) or overload resolution ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="6fe1a-1129">在屬性或索引子的修飾詞，請務必判斷哪一個屬性或索引子繫結，不論存取的內容。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1129">The modifiers on the property or indexer always determine which property or indexer is bound to, regardless of the context of the access.</span></span>

<span data-ttu-id="6fe1a-1130">一旦選取特定的屬性或索引子，相關的特定存取子的存取範圍定義域用來判斷是否為有效的使用方式：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1130">Once a particular property or indexer has been selected, the accessibility domains of the specific accessors involved are used to determine if that usage is valid:</span></span>

*  <span data-ttu-id="6fe1a-1131">如果使用量做為值 ([運算式的值](expressions.md#values-of-expressions))，則`get`存取子必須存在且可存取。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1131">If the usage is as a value ([Values of expressions](expressions.md#values-of-expressions)), the `get` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="6fe1a-1132">如果使用量為目標的簡單指派 ([簡單指派](expressions.md#simple-assignment))，則`set`存取子必須存在且可存取。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1132">If the usage is as the target of a simple assignment ([Simple assignment](expressions.md#simple-assignment)), the `set` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="6fe1a-1133">其使用方式是否為目標的複合指派 ([複合指派](expressions.md#compound-assignment))，或做為目標的`++`或是`--`運算子 ([函式成員](expressions.md#function-members).9， [引動過程運算式](expressions.md#invocation-expressions))，這兩個`get`存取子和`set`存取子必須存在且可存取。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1133">If the usage is as the target of compound assignment ([Compound assignment](expressions.md#compound-assignment)), or as the target of the `++` or `--` operators ([Function members](expressions.md#function-members).9, [Invocation expressions](expressions.md#invocation-expressions)), both the `get` accessors and the `set` accessor must exist and be accessible.</span></span>

<span data-ttu-id="6fe1a-1134">在下列範例中，屬性`A.Text`隱藏屬性所`B.Text`即使是在內容中在只有`set`呼叫存取子。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1134">In the following example, the property `A.Text` is hidden by the property `B.Text`, even in contexts where only the `set` accessor is called.</span></span> <span data-ttu-id="6fe1a-1135">相較之下，屬性`B.Count`不是類別存取`M`，因此可存取的屬性`A.Count`改為使用。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1135">In contrast, the property `B.Count` is not accessible to class `M`, so the accessible property `A.Count` is used instead.</span></span>

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

<span data-ttu-id="6fe1a-1136">用來實作介面的存取子不能*accessor_modifier*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1136">An accessor that is used to implement an interface may not have an *accessor_modifier*.</span></span> <span data-ttu-id="6fe1a-1137">如果只有一個存取子用來實作介面，可能會以宣告其他存取子*accessor_modifier*:</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1137">If only one accessor is used to implement an interface, the other accessor may be declared with an *accessor_modifier*:</span></span>
```csharp
public interface I
{
    string Prop { get; }
}

public class C: I
{
    public Prop {
        get { return "April"; }       // Must not have a modifier here
        internal set {...}            // Ok, because I.Prop has no set accessor
    }
}
```

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a><span data-ttu-id="6fe1a-1138">虛擬、 密封、 覆寫及抽象屬性存取子</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1138">Virtual, sealed, override, and abstract property accessors</span></span>

<span data-ttu-id="6fe1a-1139">A`virtual`屬性宣告可讓您指定的屬性存取子是虛擬。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1139">A `virtual` property declaration specifies that the accessors of the property are virtual.</span></span> <span data-ttu-id="6fe1a-1140">`virtual`修飾詞套用至兩個讀寫屬性存取子，不可能的讀 / 寫屬性只能有一個存取子為虛擬。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1140">The `virtual` modifier applies to both accessors of a read-write property—it is not possible for only one accessor of a read-write property to be virtual.</span></span>

<span data-ttu-id="6fe1a-1141">`abstract`屬性宣告指定的屬性存取子是虛擬的但不是提供存取子的實際實作。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1141">An `abstract` property declaration specifies that the accessors of the property are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="6fe1a-1142">相反地，非抽象衍生的類別才能覆寫屬性提供自己的存取子的實作。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1142">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the property.</span></span> <span data-ttu-id="6fe1a-1143">因為抽象屬性宣告的存取子不提供任何實際的實作，其*accessor_body*僅包含一個分號。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1143">Because an accessor for an abstract property declaration provides no actual implementation, its *accessor_body* simply consists of a semicolon.</span></span>

<span data-ttu-id="6fe1a-1144">同時包含屬性宣告`abstract`和`override`修飾詞指定的屬性是抽象的並會覆寫基底屬性。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1144">A property declaration that includes both the `abstract` and `override` modifiers specifies that the property is abstract and overrides a base property.</span></span> <span data-ttu-id="6fe1a-1145">這類屬性的存取子也是抽象的。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1145">The accessors of such a property are also abstract.</span></span>

<span data-ttu-id="6fe1a-1146">抽象屬性宣告只允許在抽象類別 ([抽象類別](classes.md#abstract-classes))。繼承的虛擬屬性存取子可以覆寫衍生類別中包含指定的屬性宣告`override`指示詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1146">Abstract property declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).The accessors of an inherited virtual property can be overridden in a derived class by including a property declaration that specifies an `override` directive.</span></span> <span data-ttu-id="6fe1a-1147">這就所謂***覆寫屬性宣告***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1147">This is known as an ***overriding property declaration***.</span></span> <span data-ttu-id="6fe1a-1148">覆寫的屬性宣告並不會宣告新的屬性。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1148">An overriding property declaration does not declare a new property.</span></span> <span data-ttu-id="6fe1a-1149">相反地，它只會指定現有的虛擬屬性存取子的實作。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1149">Instead, it simply specializes the implementations of the accessors of an existing virtual property.</span></span>

<span data-ttu-id="6fe1a-1150">覆寫的屬性宣告必須指定和繼承的屬性完全相同的存取範圍修飾詞、 類型和名稱。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1150">An overriding property declaration must specify the exact same accessibility modifiers, type, and name as the inherited property.</span></span> <span data-ttu-id="6fe1a-1151">如果繼承的屬性只有單一存取子 （亦即，如果繼承的屬性是唯讀或唯寫），覆寫的屬性必須包含該存取子。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1151">If the inherited property has only a single accessor (i.e., if the inherited property is read-only or write-only), the overriding property must include only that accessor.</span></span> <span data-ttu-id="6fe1a-1152">如果繼承的屬性會包含兩個存取子 （亦即，如果繼承的屬性是讀寫），覆寫屬性可以包含單一存取子，或是這兩個存取子。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1152">If the inherited property includes both accessors (i.e., if the inherited property is read-write), the overriding property can include either a single accessor or both accessors.</span></span>

<span data-ttu-id="6fe1a-1153">覆寫的屬性宣告可能包含`sealed`修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1153">An overriding property declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="6fe1a-1154">使用此修飾詞可防止在衍生的類別進一步覆寫屬性。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1154">Use of this modifier prevents a derived class from further overriding the property.</span></span> <span data-ttu-id="6fe1a-1155">密封屬性存取子也被密封的。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1155">The accessors of a sealed property are also sealed.</span></span>

<span data-ttu-id="6fe1a-1156">除了宣告和引動過程之間的差異語法、 虛擬、 sealed、 override、 和抽象方法和行為完全相同虛擬、 密封、 覆寫和抽象方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1156">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="6fe1a-1157">規則中所述的具體來說，[虛擬方法](classes.md#virtual-methods)，[覆寫方法](classes.md#override-methods)，[密封方法](classes.md#sealed-methods)，以及[抽象方法](classes.md#abstract-methods)套用如同存取子所對應之表單的方法：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1157">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form:</span></span>

*  <span data-ttu-id="6fe1a-1158">A`get`存取子對應的無參數方法具有傳回值的屬性類型和相同的修飾詞和包含的屬性。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1158">A `get` accessor corresponds to a parameterless method with a return value of the property type and the same modifiers as the containing property.</span></span>
*  <span data-ttu-id="6fe1a-1159">A`set`存取子對應至具有單一值參數的屬性型別方法`void`傳回型別和相同的修飾詞和包含的屬性。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1159">A `set` accessor corresponds to a method with a single value parameter of the property type, a `void` return type, and the same modifiers as the containing property.</span></span>

<span data-ttu-id="6fe1a-1160">在範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1160">In the example</span></span>
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
<span data-ttu-id="6fe1a-1161">`X` 是虛擬的唯讀屬性，`Y`是虛擬的讀寫屬性，以及`Z`是抽象的讀 / 寫屬性。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1161">`X` is a virtual read-only property, `Y` is a virtual read-write property, and `Z` is an abstract read-write property.</span></span> <span data-ttu-id="6fe1a-1162">因為`Z`是抽象的包含類別`A`也必須宣告為抽象。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1162">Because `Z` is abstract, the containing class `A` must also be declared abstract.</span></span>

<span data-ttu-id="6fe1a-1163">類別衍生自`A`會如下所示：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1163">A class that derives from `A` is show below:</span></span>
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

<span data-ttu-id="6fe1a-1164">這裡的宣告`X`， `Y`，和`Z`會覆寫屬性宣告。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1164">Here, the declarations of `X`, `Y`, and `Z` are overriding property declarations.</span></span> <span data-ttu-id="6fe1a-1165">如果每個屬性宣告的存取範圍修飾詞、 類型和對應的繼承屬性的名稱完全符合。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1165">Each property declaration exactly matches the accessibility modifiers, type, and name of the corresponding inherited property.</span></span> <span data-ttu-id="6fe1a-1166">`get`存取子`X`並`set`存取子`Y`使用`base`存取繼承的存取子關鍵字。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1166">The `get` accessor of `X` and the `set` accessor of `Y` use the `base` keyword to access the inherited accessors.</span></span> <span data-ttu-id="6fe1a-1167">宣告`Z`覆寫兩個抽象存取子，因此，會在沒有未處理的抽象函式成員`B`，和`B`允許非抽象類別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1167">The declaration of `Z` overrides both abstract accessors—thus, there are no outstanding abstract function members in `B`, and `B` is permitted to be a non-abstract class.</span></span>

<span data-ttu-id="6fe1a-1168">當屬性宣告為`override`，覆寫的任何存取子必須覆寫的程式碼存取。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1168">When a property is declared as an `override`, any overridden accessors must be accessible to the overriding code.</span></span> <span data-ttu-id="6fe1a-1169">此外，宣告的屬性或索引子本身的子範圍，必須符合覆寫的成員和存取子。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1169">In addition, the declared accessibility of both the property or indexer itself, and of the accessors, must match that of the overridden member and accessors.</span></span> <span data-ttu-id="6fe1a-1170">例如: </span><span class="sxs-lookup"><span data-stu-id="6fe1a-1170">For example:</span></span>
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

## <a name="events"></a><span data-ttu-id="6fe1a-1171">事件</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1171">Events</span></span>

<span data-ttu-id="6fe1a-1172">***事件***是可讓物件或類別，以提供通知的成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1172">An ***event*** is a member that enables an object or class to provide notifications.</span></span> <span data-ttu-id="6fe1a-1173">用戶端可附加事件的可執行程式碼，藉由提供***事件處理常式***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1173">Clients can attach executable code for events by supplying ***event handlers***.</span></span>

<span data-ttu-id="6fe1a-1174">事件使用宣告*event_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1174">Events are declared using *event_declaration*s:</span></span>

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

<span data-ttu-id="6fe1a-1175">*Event_declaration*可能包含一組*屬性*([屬性](attributes.md)) 和有效的四種存取修飾詞組合 ([存取修飾詞](classes.md#access-modifiers))，則`new`([的新修飾詞](classes.md#the-new-modifier))， `static` ([靜態和執行個體方法](classes.md#static-and-instance-methods))， `virtual` ([虛擬方法](classes.md#virtual-methods))， `override` ([覆寫方法](classes.md#override-methods))， `sealed` ([密封方法](classes.md#sealed-methods))， `abstract` ([抽象方法](classes.md#abstract-methods))，以及`extern` ([外部方法](classes.md#external-methods)) 修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1175">An *event_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="6fe1a-1176">事件宣告受限於相同的方法宣告規則 ([方法](classes.md#methods)) 方面的修飾詞的有效組合。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1176">Event declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="6fe1a-1177">*型別*事件的宣告必須是*delegate_type* ([參考型別](types.md#reference-types))，且*delegate_type*必須至少為為事件本身的存取 ([協助工具的條件約束](basic-concepts.md#accessibility-constraints))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1177">The *type* of an event declaration must be a *delegate_type* ([Reference types](types.md#reference-types)), and that *delegate_type* must be at least as accessible as the event itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="6fe1a-1178">事件宣告可能包含*event_accessor_declarations*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1178">An event declaration may include *event_accessor_declarations*.</span></span> <span data-ttu-id="6fe1a-1179">不過，如果沒有，非外部連結的非抽象事件，則編譯器會提供這些自動 ([欄位型事件](classes.md#field-like-events)); 外部事件存取子會從外部提供。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1179">However, if it does not, for non-extern, non-abstract events, the compiler supplies them automatically ([Field-like events](classes.md#field-like-events)); for extern events, the accessors are provided externally.</span></span>

<span data-ttu-id="6fe1a-1180">事件宣告省略*event_accessor_declarations*定義一或多個事件，各供一個*variable_declarator*s。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1180">An event declaration that omits *event_accessor_declarations* defines one or more events—one for each of the *variable_declarator*s.</span></span> <span data-ttu-id="6fe1a-1181">屬性和修飾詞套用至所有這類宣告的成員*event_declaration*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1181">The attributes and modifiers apply to all of the members declared by such an *event_declaration*.</span></span>

<span data-ttu-id="6fe1a-1182">它是編譯時期錯誤*event_declaration*同時包含`abstract`修飾詞和大括號分隔*event_accessor_declarations*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1182">It is a compile-time error for an *event_declaration* to include both the `abstract` modifier and brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="6fe1a-1183">當事件宣告包含`extern`修飾詞，此事件要***外部事件***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1183">When an event declaration includes an `extern` modifier, the event is said to be an ***external event***.</span></span> <span data-ttu-id="6fe1a-1184">因為外部事件宣告未不提供任何實際的實作，是它同時包含錯誤`extern`修飾詞和*event_accessor_declarations*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1184">Because an external event declaration provides no actual implementation, it is an error for it to include both the `extern` modifier and *event_accessor_declarations*.</span></span>

<span data-ttu-id="6fe1a-1185">它是編譯時期錯誤*variable_declarator*的事件宣告`abstract`或`external`修飾詞，以包含*variable_initializer*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1185">It is a compile-time error for a *variable_declarator* of an event declaration with an `abstract` or `external` modifier to include a *variable_initializer*.</span></span>

<span data-ttu-id="6fe1a-1186">事件可用來當做左運算元`+=`並`-=`運算子 ([事件指派](expressions.md#event-assignment))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1186">An event can be used as the left-hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="6fe1a-1187">這些運算子使用時，分別附加事件處理常式，或移除事件處理常式事件，以及事件的存取修飾詞控制允許使用這類作業的內容。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1187">These operators are used, respectively, to attach event handlers to or to remove event handlers from an event, and the access modifiers of the event control the contexts in which such operations are permitted.</span></span>

<span data-ttu-id="6fe1a-1188">由於`+=`和`-=`是唯一允許外部宣告的事件、 外部程式碼的型別事件的作業可以新增和移除事件處理常式，但不能以任何其他方式取得或修改事件的基礎清單處理常式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1188">Since `+=` and `-=` are the only operations that are permitted on an event outside the type that declares the event, external code can add and remove handlers for an event, but cannot in any other way obtain or modify the underlying list of event handlers.</span></span>

<span data-ttu-id="6fe1a-1189">在作業中的表單`x += y`或`x -= y`，當`x`是事件，並且包含宣告的型別之外的參考發生`x`，作業的結果的類型`void`（而不需要型別`x`，其值為`x`指派之後)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1189">In an operation of the form `x += y` or `x -= y`, when `x` is an event and the reference takes place outside the type that contains the declaration of `x`, the result of the operation has type `void` (as opposed to having the type of `x`, with the value of `x` after the assignment).</span></span> <span data-ttu-id="6fe1a-1190">此規則可禁止外部程式碼間接檢查事件的基礎委派。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1190">This rule prohibits external code from indirectly examining the underlying delegate of an event.</span></span>

<span data-ttu-id="6fe1a-1191">下列範例示範如何將事件處理常式附加到的執行個體`Button`類別：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1191">The following example shows how event handlers are attached to instances of the `Button` class:</span></span>
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

<span data-ttu-id="6fe1a-1192">在這裡，`LoginDialog`執行個體建構函式會建立兩個`Button`執行個體，並將附加事件處理常式`Click`事件。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1192">Here, the `LoginDialog` instance constructor creates two `Button` instances and attaches event handlers to the `Click` events.</span></span>

### <a name="field-like-events"></a><span data-ttu-id="6fe1a-1193">欄位型事件</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1193">Field-like events</span></span>

<span data-ttu-id="6fe1a-1194">在類別或結構，其中包含事件的宣告的程式文字，某些事件可用欄位與欄位類似。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1194">Within the program text of the class or struct that contains the declaration of an event, certain events can be used like fields.</span></span> <span data-ttu-id="6fe1a-1195">若要使用這種方式，事件不能`abstract`或`extern`，而且必須明確包含*event_accessor_declarations*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1195">To be used in this way, an event must not be `abstract` or `extern`, and must not explicitly include *event_accessor_declarations*.</span></span> <span data-ttu-id="6fe1a-1196">這種情況下可以用於任何允許欄位的內容。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1196">Such an event can be used in any context that permits a field.</span></span> <span data-ttu-id="6fe1a-1197">欄位包含的委派 ([委派](delegates.md)) 就是指已加入至事件的事件處理常式的清單。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1197">The field contains a delegate ([Delegates](delegates.md)) which refers to the list of event handlers that have been added to the event.</span></span> <span data-ttu-id="6fe1a-1198">如果尚未新增任何事件處理常式，欄位會包含`null`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1198">If no event handlers have been added, the field contains `null`.</span></span>

<span data-ttu-id="6fe1a-1199">在範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1199">In the example</span></span>
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
<span data-ttu-id="6fe1a-1200">`Click` 使用中的欄位為`Button`類別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1200">`Click` is used as a field within the `Button` class.</span></span> <span data-ttu-id="6fe1a-1201">如範例所示，該欄位可以檢查、 修改，並委派引動過程運算式中使用。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1201">As the example demonstrates, the field can be examined, modified, and used in delegate invocation expressions.</span></span> <span data-ttu-id="6fe1a-1202">`OnClick`方法中的`Button`類別 「 引發 」`Click`事件。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1202">The `OnClick` method in the `Button` class "raises" the `Click` event.</span></span> <span data-ttu-id="6fe1a-1203">引發事件的概念完全等同於叫用該事件所代表的委派項目，因此，就引發事件而言，並沒有任何特殊的語言建構。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1203">The notion of raising an event is precisely equivalent to invoking the delegate represented by the event—thus, there are no special language constructs for raising events.</span></span> <span data-ttu-id="6fe1a-1204">請注意，委派引動過程會加上核取，以確保委派為非 null。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1204">Note that the delegate invocation is preceded by a check that ensures the delegate is non-null.</span></span>

<span data-ttu-id="6fe1a-1205">宣告之外`Button`類別，`Click`成員僅適用於在左手邊`+=`和`-=`運算子，如下所示</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1205">Outside the declaration of the `Button` class, the `Click` member can only be used on the left-hand side of the `+=` and `-=` operators, as in</span></span>
```csharp
b.Click += new EventHandler(...);
```
<span data-ttu-id="6fe1a-1206">將附加委派的引動過程清單`Click`事件，以及</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1206">which appends a delegate to the invocation list of the `Click` event, and</span></span>
```csharp
b.Click -= new EventHandler(...);
```
<span data-ttu-id="6fe1a-1207">以從引動過程清單移除委派`Click`事件。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1207">which removes a delegate from the invocation list of the `Click` event.</span></span>

<span data-ttu-id="6fe1a-1208">在編譯時的欄位型事件，編譯器會自動建立儲存空間來保存此委派，，並建立事件，新增或移除事件處理常式委派欄位，存取子。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1208">When compiling a field-like event, the compiler automatically creates storage to hold the delegate, and creates accessors for the event that add or remove event handlers to the delegate field.</span></span> <span data-ttu-id="6fe1a-1209">新增和移除作業執行緒安全，並可能 （但並不需要） 時完成持有鎖定 ([lock 陳述式](statements.md#the-lock-statement)) 執行個體事件，包含的物件或型別物件 ([匿名物件建立運算式](expressions.md#anonymous-object-creation-expressions)) 靜態事件。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1209">The addition and removal operations are thread safe, and may (but are not required to) be done while holding the lock ([The lock statement](statements.md#the-lock-statement)) on the containing object for an instance event, or the type object ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) for a static event.</span></span>

<span data-ttu-id="6fe1a-1210">因此，執行個體事件宣告的形式：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1210">Thus, an instance event declaration of the form:</span></span>
```csharp
class X
{
    public event D Ev;
}
```
<span data-ttu-id="6fe1a-1211">將會編譯為相等的項目：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1211">will be compiled to something equivalent to:</span></span>
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
<span data-ttu-id="6fe1a-1212">在類別內`X`，參考`Ev`的左手邊`+=`和`-=`運算子造成新增和移除要叫用的存取子。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1212">Within the class `X`, references to `Ev` on the left-hand side of the `+=` and `-=` operators cause the add and remove accessors to be invoked.</span></span> <span data-ttu-id="6fe1a-1213">所有其他參考`Ev`編譯為參考的隱藏的欄位`__Ev`改為 ([成員存取](expressions.md#member-access))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1213">All other references to `Ev` are compiled to reference the hidden field `__Ev` instead ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="6fe1a-1214">名稱"`__Ev`"是任意; 隱藏的欄位可能會有任何名稱不完全。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1214">The name "`__Ev`" is arbitrary; the hidden field could have any name or no name at all.</span></span>

### <a name="event-accessors"></a><span data-ttu-id="6fe1a-1215">事件存取子</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1215">Event accessors</span></span>

<span data-ttu-id="6fe1a-1216">事件宣告通常會忽略*event_accessor_declarations*，如下所示`Button`上述範例。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1216">Event declarations typically omit *event_accessor_declarations*, as in the `Button` example above.</span></span> <span data-ttu-id="6fe1a-1217">一種情況下，執行此步驟涉及在其中一個欄位，每個事件的儲存體成本不是可接受的情況。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1217">One situation for doing so involves the case in which the storage cost of one field per event is not acceptable.</span></span> <span data-ttu-id="6fe1a-1218">在此情況下，類別可以包含*event_accessor_declarations* ，並使用私用機制來儲存事件處理常式的清單。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1218">In such cases, a class can include *event_accessor_declarations* and use a private mechanism for storing the list of event handlers.</span></span>

<span data-ttu-id="6fe1a-1219">*Event_accessor_declarations*事件的指定可執行的陳述式加入和移除事件處理常式相關聯。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1219">The *event_accessor_declarations* of an event specify the executable statements associated with adding and removing event handlers.</span></span>

<span data-ttu-id="6fe1a-1220">存取子宣告組成*add_accessor_declaration*並*remove_accessor_declaration*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1220">The accessor declarations consist of an *add_accessor_declaration* and a *remove_accessor_declaration*.</span></span> <span data-ttu-id="6fe1a-1221">每個存取子宣告所組成的語彙基元`add`或是`remove`後面*區塊*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1221">Each accessor declaration consists of the token `add` or `remove` followed by a *block*.</span></span> <span data-ttu-id="6fe1a-1222">*區塊*聯*add_accessor_declaration*指定的陳述式執行時加入事件處理常式，而*區塊*相關聯*remove_accessor_declaration*指定移除事件處理常式時要執行的陳述式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1222">The *block* associated with an *add_accessor_declaration* specifies the statements to execute when an event handler is added, and the *block* associated with a *remove_accessor_declaration* specifies the statements to execute when an event handler is removed.</span></span>

<span data-ttu-id="6fe1a-1223">每個*add_accessor_declaration*並*remove_accessor_declaration*對應至具有事件類型的單一值參數的方法和`void`傳回型別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1223">Each *add_accessor_declaration* and *remove_accessor_declaration* corresponds to a method with a single value parameter of the event type and a `void` return type.</span></span> <span data-ttu-id="6fe1a-1224">事件存取子的隱含參數之所以名為`value`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1224">The implicit parameter of an event accessor is named `value`.</span></span> <span data-ttu-id="6fe1a-1225">當事件指派使用事件時，會使用適當的事件存取子。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1225">When an event is used in an event assignment, the appropriate event accessor is used.</span></span> <span data-ttu-id="6fe1a-1226">具體來說，如果指派運算子`+=`則會使用的 add 存取子，，和指派運算子是`-=`則會使用 remove 存取子。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1226">Specifically, if the assignment operator is `+=` then the add accessor is used, and if the assignment operator is `-=` then the remove accessor is used.</span></span> <span data-ttu-id="6fe1a-1227">在任一情況下，指派運算子的右運算元做為事件存取子的引數中。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1227">In either case, the right-hand operand of the assignment operator is used as the argument to the event accessor.</span></span> <span data-ttu-id="6fe1a-1228">區塊*add_accessor_declaration*或是*remove_accessor_declaration*必須符合的規則`void`中所述的方法[方法主體](classes.md#method-body)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1228">The block of an *add_accessor_declaration* or a *remove_accessor_declaration* must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="6fe1a-1229">特別是，`return`這類區塊中的陳述式不允許指定的運算式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1229">In particular, `return` statements in such a block are not permitted to specify an expression.</span></span>

<span data-ttu-id="6fe1a-1230">因為事件存取子會隱含地擁有名為的參數`value`，本機變數或常數宣告中的事件存取子，具有這個名稱是編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1230">Since an event accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declared in an event accessor to have that name.</span></span>

<span data-ttu-id="6fe1a-1231">在範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1231">In the example</span></span>
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
<span data-ttu-id="6fe1a-1232">`Control`類別會實作事件的內部儲存機制。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1232">the `Control` class implements an internal storage mechanism for events.</span></span> <span data-ttu-id="6fe1a-1233">`AddEventHandler`方法會將委派值關聯的索引鍵`GetEventHandler`方法會傳回目前的索引鍵相關聯的委派和`RemoveEventHandler`方法會移除與指定的事件的事件處理常式的委派。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1233">The `AddEventHandler` method associates a delegate value with a key, the `GetEventHandler` method returns the delegate currently associated with a key, and the `RemoveEventHandler` method removes a delegate as an event handler for the specified event.</span></span> <span data-ttu-id="6fe1a-1234">我們可以假設，基礎儲存機制的設計可讓沒有任何相關聯的成本`null`委派以索引鍵的值，並因此未處理的事件取用任何儲存體。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1234">Presumably, the underlying storage mechanism is designed such that there is no cost for associating a `null` delegate value with a key, and thus unhandled events consume no storage.</span></span>

### <a name="static-and-instance-events"></a><span data-ttu-id="6fe1a-1235">靜態和執行個體的事件</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1235">Static and instance events</span></span>

<span data-ttu-id="6fe1a-1236">當事件宣告包含`static`修飾詞，此事件要***靜態事件***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1236">When an event declaration includes a `static` modifier, the event is said to be a ***static event***.</span></span> <span data-ttu-id="6fe1a-1237">若未`static`修飾詞存在，則此事件要***執行個體事件***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1237">When no `static` modifier is present, the event is said to be an ***instance event***.</span></span>

<span data-ttu-id="6fe1a-1238">靜態事件相關聯的特定執行個體，並不是編譯時期錯誤，以指向`this`中的靜態事件存取子。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1238">A static event is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static event.</span></span>

<span data-ttu-id="6fe1a-1239">執行個體事件與指定類別的執行個體相關聯，而且可以當做這個執行個體`this`([這項存取](expressions.md#this-access)) 中的該事件存取子。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1239">An instance event is associated with a given instance of a class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that event.</span></span>

<span data-ttu-id="6fe1a-1240">事件中的參考時*member_access* ([成員存取](expressions.md#member-access)) 的形式`E.M`時，如果`M`是靜態事件，`E`必須代表已包含類型`M`，而且如果`M`是執行個體的事件，E 必須代表執行個體的型別，其中`M`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1240">When an event is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static event, `E` must denote a type containing `M`, and if `M` is an instance event, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="6fe1a-1241">靜態之間的差異和執行個體成員討論中進一步[靜態和執行個體成員](classes.md#static-and-instance-members)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1241">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a><span data-ttu-id="6fe1a-1242">虛擬、 密封、 覆寫及抽象事件存取子</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1242">Virtual, sealed, override, and abstract event accessors</span></span>

<span data-ttu-id="6fe1a-1243">A`virtual`事件宣告可讓您指定的事件存取子是虛擬。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1243">A `virtual` event declaration specifies that the accessors of that event are virtual.</span></span> <span data-ttu-id="6fe1a-1244">`virtual`修飾詞套用至兩個事件存取子。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1244">The `virtual` modifier applies to both accessors of an event.</span></span>

<span data-ttu-id="6fe1a-1245">`abstract`事件宣告指定的事件存取子是虛擬的但不是提供存取子的實際實作。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1245">An `abstract` event declaration specifies that the accessors of the event are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="6fe1a-1246">相反地，非抽象衍生的類別必須提供自己的實作存取子的覆寫事件。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1246">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the event.</span></span> <span data-ttu-id="6fe1a-1247">抽象事件宣告未提供實際實作，因為它無法提供大括號分隔*event_accessor_declarations*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1247">Because an abstract event declaration provides no actual implementation, it cannot provide brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="6fe1a-1248">同時包含事件宣告`abstract`和`override`修飾詞指定的事件是抽象的而且會覆寫基底的事件。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1248">An event declaration that includes both the `abstract` and `override` modifiers specifies that the event is abstract and overrides a base event.</span></span> <span data-ttu-id="6fe1a-1249">這類事件存取子也是抽象的。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1249">The accessors of such an event are also abstract.</span></span>

<span data-ttu-id="6fe1a-1250">抽象事件宣告中只允許出現在抽象類別 ([抽象類別](classes.md#abstract-classes))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1250">Abstract event declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="6fe1a-1251">繼承的虛擬事件的存取子可以覆寫衍生類別中包含指定的事件宣告`override`修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1251">The accessors of an inherited virtual event can be overridden in a derived class by including an event declaration that specifies an `override` modifier.</span></span> <span data-ttu-id="6fe1a-1252">這就所謂***覆寫事件宣告***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1252">This is known as an ***overriding event declaration***.</span></span> <span data-ttu-id="6fe1a-1253">覆寫的事件宣告並不會宣告新的事件。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1253">An overriding event declaration does not declare a new event.</span></span> <span data-ttu-id="6fe1a-1254">相反地，它只會指定現有的虛擬活動的存取子的實作。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1254">Instead, it simply specializes the implementations of the accessors of an existing virtual event.</span></span>

<span data-ttu-id="6fe1a-1255">覆寫的事件宣告必須指定完全相同的存取範圍修飾詞、 類型和名稱為覆寫的事件。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1255">An overriding event declaration must specify the exact same accessibility modifiers, type, and name as the overridden event.</span></span>

<span data-ttu-id="6fe1a-1256">覆寫的事件宣告可能包含`sealed`修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1256">An overriding event declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="6fe1a-1257">使用此修飾詞可防止在衍生的類別進一步覆寫事件。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1257">Use of this modifier prevents a derived class from further overriding the event.</span></span> <span data-ttu-id="6fe1a-1258">密封的事件存取子也被密封的。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1258">The accessors of a sealed event are also sealed.</span></span>

<span data-ttu-id="6fe1a-1259">它會覆寫事件宣告中包含的編譯時期錯誤`new`修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1259">It is a compile-time error for an overriding event declaration to include a `new` modifier.</span></span>

<span data-ttu-id="6fe1a-1260">除了宣告和引動過程之間的差異語法、 虛擬、 sealed、 override、 和抽象方法和行為完全相同虛擬、 密封、 覆寫和抽象方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1260">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="6fe1a-1261">規則中所述的具體來說，[虛擬方法](classes.md#virtual-methods)，[覆寫方法](classes.md#override-methods)，[密封方法](classes.md#sealed-methods)，以及[抽象方法](classes.md#abstract-methods)套用如同存取子所對應之表單的方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1261">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form.</span></span> <span data-ttu-id="6fe1a-1262">每個存取子對應至具有單一值參數的事件類型，方法`void`傳回型別，並與包含事件的相同修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1262">Each accessor corresponds to a method with a single value parameter of the event type, a `void` return type, and the same modifiers as the containing event.</span></span>

## <a name="indexers"></a><span data-ttu-id="6fe1a-1263">索引子</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1263">Indexers</span></span>

<span data-ttu-id="6fe1a-1264">***Indexer***是讓物件可以做為陣列相同的方式編製索引的成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1264">An ***indexer*** is a member that enables an object to be indexed in the same way as an array.</span></span> <span data-ttu-id="6fe1a-1265">使用宣告索引子*indexer_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1265">Indexers are declared using *indexer_declaration*s:</span></span>

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

<span data-ttu-id="6fe1a-1266">*Indexer_declaration*可能包含一組*屬性*([屬性](attributes.md)) 和有效的四種存取修飾詞組合 ([存取修飾詞](classes.md#access-modifiers))，則`new`([的新修飾詞](classes.md#the-new-modifier))， `virtual` ([虛擬方法](classes.md#virtual-methods))， `override` ([覆寫方法](classes.md#override-methods))， `sealed` ([密封方法](classes.md#sealed-methods))， `abstract` ([抽象方法](classes.md#abstract-methods))，以及`extern`([外部方法](classes.md#external-methods)) 修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1266">An *indexer_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="6fe1a-1267">索引子宣告受限於相同的方法宣告規則 ([方法](classes.md#methods)) 方面的修飾詞的有效組合，有一個例外狀況，在於 static 修飾詞不允許在索引子宣告。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1267">Indexer declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers, with the one exception being that the static modifier is not permitted on an indexer declaration.</span></span>

<span data-ttu-id="6fe1a-1268">修飾詞`virtual`， `override`，和`abstract`互斥除了在其中一個案例。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1268">The modifiers `virtual`, `override`, and `abstract` are mutually exclusive except in one case.</span></span> <span data-ttu-id="6fe1a-1269">`abstract`和`override`以便抽象索引子可以覆寫虛擬介面修飾詞也可能會一起使用。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1269">The `abstract` and `override` modifiers may be used together so that an abstract indexer can override a virtual one.</span></span>

<span data-ttu-id="6fe1a-1270">*型別*索引子的宣告會指定索引子，宣告所引進的項目類型。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1270">The *type* of an indexer declaration specifies the element type of the indexer introduced by the declaration.</span></span> <span data-ttu-id="6fe1a-1271">索引子是明確介面成員實作，除非*型別*後面跟著關鍵字`this`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1271">Unless the indexer is an explicit interface member implementation, the *type* is followed by the keyword `this`.</span></span> <span data-ttu-id="6fe1a-1272">明確介面成員實作，*型別*後面*interface_type*、 「`.`"，和關鍵字`this`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1272">For an explicit interface member implementation, the *type* is followed by an *interface_type*, a "`.`", and the keyword `this`.</span></span> <span data-ttu-id="6fe1a-1273">不同於其他成員中，索引子不會有使用者定義的名稱。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1273">Unlike other members, indexers do not have user-defined names.</span></span>

<span data-ttu-id="6fe1a-1274">*Formal_parameter_list*指定的索引子的參數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1274">The *formal_parameter_list* specifies the parameters of the indexer.</span></span> <span data-ttu-id="6fe1a-1275">索引子的型式參數清單對應至方法的 ([方法的參數](classes.md#method-parameters))，但必須指定至少一個參數，且`ref`和`out`不允許參數修飾詞.</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1275">The formal parameter list of an indexer corresponds to that of a method ([Method parameters](classes.md#method-parameters)), except that at least one parameter must be specified, and that the `ref` and `out` parameter modifiers are not permitted.</span></span>

<span data-ttu-id="6fe1a-1276">*型別*的索引子和每個參考中的型別*formal_parameter_list*必須是至少一樣地存取索引子本身 ([存取範圍條件約束](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1276">The *type* of an indexer and each of the types referenced in the *formal_parameter_list* must be at least as accessible as the indexer itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="6fe1a-1277">*Indexer_body*可能是組成***存取子主體***或是***運算式主體***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1277">An *indexer_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="6fe1a-1278">在存取子主體中， *accessor_declarations*，這必須括在 「`{`"和"`}`"權杖宣告存取子 ([存取子](classes.md#accessors)) 的屬性。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1278">In an accessor body, *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="6fe1a-1279">存取子會指定可執行讀取和寫入的屬性相關聯的陳述式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1279">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="6fe1a-1280">組成的運算式主體 」`=>`"接著一運算式`E`分號就完全相當於陳述式主體和`{ get { return E; } }`，並因此只可用來指定僅限 getter 的索引子的 getter 結果的所在根據單一運算式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1280">An expression body consisting of "`=>`" followed by an expression `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only indexers where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="6fe1a-1281">即使存取索引子元素的語法是相同的陣列項目，索引子元素不會分類為變數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1281">Even though the syntax for accessing an indexer element is the same as that for an array element, an indexer element is not classified as a variable.</span></span> <span data-ttu-id="6fe1a-1282">因此，不可能傳遞的索引子元素當做`ref`或`out`引數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1282">Thus, it is not possible to pass an indexer element as a `ref` or `out` argument.</span></span>

<span data-ttu-id="6fe1a-1283">索引子的型式參數清單會定義簽章 ([簽章和多載](basic-concepts.md#signatures-and-overloading)) 的索引子。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1283">The formal parameter list of an indexer defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the indexer.</span></span> <span data-ttu-id="6fe1a-1284">具體而言，索引子的簽章所組成的數目和其型式參數的類型。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1284">Specifically, the signature of an indexer consists of the number and types of its formal parameters.</span></span> <span data-ttu-id="6fe1a-1285">項目型別和型式參數名稱不是索引子的簽章的一部分。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1285">The element type and names of the formal parameters are not part of an indexer's signature.</span></span>

<span data-ttu-id="6fe1a-1286">索引子的簽章必須與在相同類別中宣告的其他所有索引子的簽章不同。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1286">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>

<span data-ttu-id="6fe1a-1287">索引子和屬性在概念上，非常類似，但在下列方面也不同：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1287">Indexers and properties are very similar in concept, but differ in the following ways:</span></span>

*  <span data-ttu-id="6fe1a-1288">屬性會識別的名稱，而索引子由其簽章。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1288">A property is identified by its name, whereas an indexer is identified by its signature.</span></span>
*  <span data-ttu-id="6fe1a-1289">屬性透過存取*simple_name* ([簡單名稱](expressions.md#simple-names)) 或*member_access* ([成員存取](expressions.md#member-access))，而索引子項目經由*element_access* ([索引子存取](expressions.md#indexer-access))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1289">A property is accessed through a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)), whereas an indexer element is accessed through an *element_access* ([Indexer access](expressions.md#indexer-access)).</span></span>
*  <span data-ttu-id="6fe1a-1290">屬性可以是`static`成員，而索引子是一律執行個體成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1290">A property can be a `static` member, whereas an indexer is always an instance member.</span></span>
*  <span data-ttu-id="6fe1a-1291">A`get`存取子的屬性會對應至使用任何參數，方法，而`get`索引子的存取子會對應至具有相同的型式參數清單與索引子的方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1291">A `get` accessor of a property corresponds to a method with no parameters, whereas a `get` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer.</span></span>
*  <span data-ttu-id="6fe1a-1292">A`set`屬性存取子對應至方法，與具有單一參數具名`value`，而`set`索引子的存取子會對應至具有相同索引子，再加上額外的參數的型式參數清單的方法名為`value`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1292">A `set` accessor of a property corresponds to a method with a single parameter named `value`, whereas a `set` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer, plus an additional parameter named `value`.</span></span>
*  <span data-ttu-id="6fe1a-1293">它是索引子存取子宣告本機變數具有相同名稱做為索引子參數的編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1293">It is a compile-time error for an indexer accessor to declare a local variable with the same name as an indexer parameter.</span></span>
*  <span data-ttu-id="6fe1a-1294">在 覆寫的屬性宣告，繼承的屬性可使用語法`base.P`，其中`P`是屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1294">In an overriding property declaration, the inherited property is accessed using the syntax `base.P`, where `P` is the property name.</span></span> <span data-ttu-id="6fe1a-1295">在覆寫索引子宣告中，繼承的索引子來存取使用語法`base[E]`，其中`E`是運算式的逗號分隔清單。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1295">In an overriding indexer declaration, the inherited indexer is accessed using the syntax `base[E]`, where `E` is a comma separated list of expressions.</span></span>
*  <span data-ttu-id="6fe1a-1296">沒有 「 自動實作索引子 」 的概念。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1296">There is no concept of an "automatically implemented indexer".</span></span> <span data-ttu-id="6fe1a-1297">它是有非抽象，非外部的索引子，以分號存取子的錯誤。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1297">It is an error to have a non-abstract, non-external indexer with semicolon accessors.</span></span>

<span data-ttu-id="6fe1a-1298">除了這些差異，定義中的所有規則[存取子](classes.md#accessors)並[自動實作屬性](classes.md#automatically-implemented-properties)套用索引子存取子以及屬性存取子。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1298">Aside from these differences, all rules defined in [Accessors](classes.md#accessors) and [Automatically implemented properties](classes.md#automatically-implemented-properties) apply to indexer accessors as well as to property accessors.</span></span>

<span data-ttu-id="6fe1a-1299">當包含索引子宣告`extern`修飾詞，即為索引子***外部的索引子***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1299">When an indexer declaration includes an `extern` modifier, the indexer is said to be an ***external indexer***.</span></span> <span data-ttu-id="6fe1a-1300">因為外部索引子宣告未不提供任何實際的實作，每個及其*accessor_declarations*只包含一個分號。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1300">Because an external indexer declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

<span data-ttu-id="6fe1a-1301">下列範例會宣告`BitArray`實作索引子來存取個別位元的位元陣列中的類別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1301">The example below declares a `BitArray` class that implements an indexer for accessing the individual bits in the bit array.</span></span>
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

<span data-ttu-id="6fe1a-1302">執行個體`BitArray`類別會取用頻寬極小的記憶體比對應`bool[]`（因為前者的每個值佔而不是只包含一個位元，後者的一個位元組），但它允許在相同的作業為`bool[]`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1302">An instance of the `BitArray` class consumes substantially less memory than a corresponding `bool[]` (since each value of the former occupies only one bit instead of the latter's one byte), but it permits the same operations as a `bool[]`.</span></span>

<span data-ttu-id="6fe1a-1303">下列`CountPrimes`類別會使用`BitArray`和傳統 「 sieve 」 演算法來計算介於 1 到指定的最大的質數的數目：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1303">The following `CountPrimes` class uses a `BitArray` and the classical "sieve" algorithm to compute the number of primes between 1 and a given maximum:</span></span>
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

<span data-ttu-id="6fe1a-1304">請注意，來存取的項目語法`BitArray`正是相同`bool[]`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1304">Note that the syntax for accessing elements of the `BitArray` is precisely the same as for a `bool[]`.</span></span>

<span data-ttu-id="6fe1a-1305">下列範例顯示具有兩個參數的索引子的 26 \* 10 方格類別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1305">The following example shows a 26 \* 10 grid class that has an indexer with two parameters.</span></span> <span data-ttu-id="6fe1a-1306">第一個參數必須是大寫或小寫字母 A 到 Z、 範圍內，第二個必須是範圍 0-9 中的整數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1306">The first parameter is required to be an upper- or lowercase letter in the range A-Z, and the second is required to be an integer in the range 0-9.</span></span>

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

### <a name="indexer-overloading"></a><span data-ttu-id="6fe1a-1307">索引子多載</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1307">Indexer overloading</span></span>

<span data-ttu-id="6fe1a-1308">索引子多載解析規則所述[型別推斷](expressions.md#type-inference)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1308">The indexer overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="operators"></a><span data-ttu-id="6fe1a-1309">運算子</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1309">Operators</span></span>

<span data-ttu-id="6fe1a-1310">***運算子***定義可套用至類別的執行個體的運算式運算子的意義的成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1310">An ***operator*** is a member that defines the meaning of an expression operator that can be applied to instances of the class.</span></span> <span data-ttu-id="6fe1a-1311">使用宣告運算子*operator_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1311">Operators are declared using *operator_declaration*s:</span></span>

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

<span data-ttu-id="6fe1a-1312">有三種類別的多載運算子：一元運算子 ([一元運算子](classes.md#unary-operators))，二元運算子 ([二元運算子](classes.md#binary-operators))，並轉換運算子 ([轉換運算子](classes.md#conversion-operators))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1312">There are three categories of overloadable operators: Unary operators ([Unary operators](classes.md#unary-operators)), binary operators ([Binary operators](classes.md#binary-operators)), and conversion operators ([Conversion operators](classes.md#conversion-operators)).</span></span>

<span data-ttu-id="6fe1a-1313">*Operator_body*是其中一個分號***陳述式主體***或是***運算式主體***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1313">The *operator_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="6fe1a-1314">陳述式主體所組成*區塊*，指定要執行時叫用運算子的陳述式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1314">A statement body consists of a *block*, which specifies the statements to execute when the operator is invoked.</span></span> <span data-ttu-id="6fe1a-1315">*區塊*必須符合的傳回值的規則中所述的方法[方法主體](classes.md#method-body)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1315">The *block* must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="6fe1a-1316">運算式主體包含`=>`後面的運算式和分號，並代表單一運算式時，運算子會叫用執行。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1316">An expression body consists of `=>` followed by an expression and a semicolon, and denotes a single expression to perform when the operator is invoked.</span></span>

<span data-ttu-id="6fe1a-1317">針對`extern`運算子*operator_body*只包含一個分號。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1317">For `extern` operators, the *operator_body* consists simply of a semicolon.</span></span> <span data-ttu-id="6fe1a-1318">所有其他運算子，如*operator_body*運算式主體或區塊的主體。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1318">For all other operators, the *operator_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="6fe1a-1319">下列規則適用於所有運算子的宣告：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1319">The following rules apply to all operator declarations:</span></span>

*  <span data-ttu-id="6fe1a-1320">運算子宣告必須同時包含`public`和`static`修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1320">An operator declaration must include both a `public` and a `static` modifier.</span></span>
*  <span data-ttu-id="6fe1a-1321">運算子的參數必須是實值參數 ([實值參數](variables.md#value-parameters))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1321">The parameter(s) of an operator must be value parameters ([Value parameters](variables.md#value-parameters)).</span></span> <span data-ttu-id="6fe1a-1322">它是運算子宣告，以指定的編譯時期錯誤`ref`或`out`參數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1322">It is a compile-time error for an operator declaration to specify `ref` or `out` parameters.</span></span>
*  <span data-ttu-id="6fe1a-1323">運算子的簽章 ([一元運算子](classes.md#unary-operators)，[二元運算子](classes.md#binary-operators)，[轉換運算子](classes.md#conversion-operators)) 必須不同於其他宣告中的所有運算子的簽章相同的類別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1323">The signature of an operator ([Unary operators](classes.md#unary-operators), [Binary operators](classes.md#binary-operators), [Conversion operators](classes.md#conversion-operators)) must differ from the signatures of all other operators declared in the same class.</span></span>
*  <span data-ttu-id="6fe1a-1324">運算子宣告中所參考的所有類型都必須至少像運算子本身那樣 ([協助工具的條件約束](basic-concepts.md#accessibility-constraints))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1324">All types referenced in an operator declaration must be at least as accessible as the operator itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>
*  <span data-ttu-id="6fe1a-1325">它是相同的修飾詞在運算子宣告中出現多次錯誤。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1325">It is an error for the same modifier to appear multiple times in an operator declaration.</span></span>

<span data-ttu-id="6fe1a-1326">每個操作員類別目錄都會加入額外的限制，如同下列各節所述。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1326">Each operator category imposes additional restrictions, as described in the following sections.</span></span>

<span data-ttu-id="6fe1a-1327">如同其他成員，在衍生類別會繼承基底類別中宣告運算子。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1327">Like other members, operators declared in a base class are inherited by derived classes.</span></span> <span data-ttu-id="6fe1a-1328">運算子宣告一律需要類別或結構宣告運算子的簽章中加入該運算子，因為它不可能在衍生類別中宣告的運算子，隱藏在基底類別中宣告的運算子。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1328">Because operator declarations always require the class or struct in which the operator is declared to participate in the signature of the operator, it is not possible for an operator declared in a derived class to hide an operator declared in a base class.</span></span> <span data-ttu-id="6fe1a-1329">因此，`new`永遠不會有需要，並因此也不允許出現在運算子宣告修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1329">Thus, the `new` modifier is never required, and therefore never permitted, in an operator declaration.</span></span>

<span data-ttu-id="6fe1a-1330">一元和二元運算子的其他資訊可在[運算子](expressions.md#operators)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1330">Additional information on unary and binary operators can be found in [Operators](expressions.md#operators).</span></span>

<span data-ttu-id="6fe1a-1331">轉換運算子的其他資訊可在[使用者定義轉換](conversions.md#user-defined-conversions)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1331">Additional information on conversion operators can be found in [User-defined conversions](conversions.md#user-defined-conversions).</span></span>

### <a name="unary-operators"></a><span data-ttu-id="6fe1a-1332">一元運算子</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1332">Unary operators</span></span>

<span data-ttu-id="6fe1a-1333">下列規則適用於一元運算子的宣告，其中`T`表示類別或結構，其中包含運算子宣告的執行個體類型：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1333">The following rules apply to unary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="6fe1a-1334">一元`+`， `-`， `!`，或`~`運算子必須採用單一參數的型別`T`或`T?`並可傳回任何類型。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1334">A unary `+`, `-`, `!`, or `~` operator must take a single parameter of type `T` or `T?` and can return any type.</span></span>
*  <span data-ttu-id="6fe1a-1335">一元`++`或是`--`運算子必須採用單一參數型別的`T`或`T?`而且必須傳回相同的型別或型別衍生自它。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1335">A unary `++` or `--` operator must take a single parameter of type `T` or `T?` and must return that same type or a type derived from it.</span></span>
*  <span data-ttu-id="6fe1a-1336">一元`true`或是`false`運算子必須採用單一參數型別的`T`或`T?`而且必須傳回型別`bool`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1336">A unary `true` or `false` operator must take a single parameter of type `T` or `T?` and must return type `bool`.</span></span>

<span data-ttu-id="6fe1a-1337">一元運算子的簽章所組成的運算子語彙基元 (`+`， `-`， `!`， `~`， `++`， `--`， `true`，或`false`) 和單一的型式參數的型別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1337">The signature of a unary operator consists of the operator token (`+`, `-`, `!`, `~`, `++`, `--`, `true`, or `false`) and the type of the single formal parameter.</span></span> <span data-ttu-id="6fe1a-1338">傳回的型別不是一元運算子的簽章的一部分，也是型式參數的名稱。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1338">The return type is not part of a unary operator's signature, nor is the name of the formal parameter.</span></span>

<span data-ttu-id="6fe1a-1339">`true`和`false`一元運算子需要成對的宣告。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1339">The `true` and `false` unary operators require pair-wise declaration.</span></span> <span data-ttu-id="6fe1a-1340">如果類別是下列運算子之一不需要也宣告其他宣告，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1340">A compile-time error occurs if a class declares one of these operators without also declaring the other.</span></span> <span data-ttu-id="6fe1a-1341">`true`並`false`運算子會中將進一步說明[使用者定義的條件式邏輯運算子](expressions.md#user-defined-conditional-logical-operators)並[布林運算式](expressions.md#boolean-expressions)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1341">The `true` and `false` operators are described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators) and [Boolean expressions](expressions.md#boolean-expressions).</span></span>

<span data-ttu-id="6fe1a-1342">下列範例顯示實作和後續使用量的`operator ++`整數向量類別：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1342">The following example shows an implementation and subsequent usage of `operator ++` for an integer vector class:</span></span>
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

<span data-ttu-id="6fe1a-1343">請注意如何運算子方法會傳回值所產生的加 1 的運算元，如同後置遞增和遞減運算子 ([後置遞增和遞減運算子](expressions.md#postfix-increment-and-decrement-operators))，以及前置遞增和遞減運算子 ([前置遞增和遞減運算子](expressions.md#prefix-increment-and-decrement-operators))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1343">Note how the operator method returns the value produced by adding 1 to the operand, just like the  postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)), and the prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="6fe1a-1344">不同於 c + +，這個方法需要其運算元的值直接修改。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1344">Unlike in C++, this method need not modify the value of its operand directly.</span></span> <span data-ttu-id="6fe1a-1345">事實上，修改的運算元值違反標準後置遞增運算子的語意。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1345">In fact, modifying the operand value would violate the standard semantics of the postfix increment operator.</span></span>

### <a name="binary-operators"></a><span data-ttu-id="6fe1a-1346">二元運算子</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1346">Binary operators</span></span>

<span data-ttu-id="6fe1a-1347">下列規則適用於二元運算子的宣告，其中`T`表示類別或結構，其中包含運算子宣告的執行個體類型：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1347">The following rules apply to binary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="6fe1a-1348">二進位非移位運算子必須採用兩個參數，至少其中之一必須具有型別`T`或`T?`，並可傳回任何類型。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1348">A binary non-shift operator must take two parameters, at least one of which must have type `T` or `T?`, and can return any type.</span></span>
*  <span data-ttu-id="6fe1a-1349">二進位`<<`或`>>`運算子必須採用兩個參數，其中第一個型別必須`T`或`T?`，其中第二個型別必須`int`或`int?`，並可傳回任何類型。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1349">A binary `<<` or `>>` operator must take two parameters, the first of which must have type `T` or `T?` and the second of which must have type `int` or `int?`, and can return any type.</span></span>

<span data-ttu-id="6fe1a-1350">二元運算子的簽章所組成的運算子語彙基元 (`+`， `-`， `*`， `/`， `%`， `&`， `|`， `^`， `<<`， `>>`，`==`， `!=`， `>`， `<`， `>=`，或`<=`) 和兩個型式參數的類型。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1350">The signature of a binary operator consists of the operator token (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`, or `<=`) and the types of the two formal parameters.</span></span> <span data-ttu-id="6fe1a-1351">傳回的型別和型式參數的名稱不是二元運算子的簽章的一部分。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1351">The return type and the names of the formal parameters are not part of a binary operator's signature.</span></span>

<span data-ttu-id="6fe1a-1352">某些二元運算子需要成對的宣告。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1352">Certain binary operators require pair-wise declaration.</span></span> <span data-ttu-id="6fe1a-1353">運算子對其中之一的每個宣告，必須有相符配對的其他運算子的宣告。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1353">For every declaration of either operator of a pair, there must be a matching declaration of the other operator of the pair.</span></span> <span data-ttu-id="6fe1a-1354">它們有相同的傳回類型與每個參數類型時，就會符合兩個運算子的宣告。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1354">Two operator declarations match when they have the same return type and the same type for each parameter.</span></span> <span data-ttu-id="6fe1a-1355">下列運算子需要成對的宣告：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1355">The following operators require pair-wise declaration:</span></span>

*  <span data-ttu-id="6fe1a-1356">`operator ==` 和 `operator !=`</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1356">`operator ==` and `operator !=`</span></span>
*  <span data-ttu-id="6fe1a-1357">`operator >` 和 `operator <`</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1357">`operator >` and `operator <`</span></span>
*  <span data-ttu-id="6fe1a-1358">`operator >=` 和 `operator <=`</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1358">`operator >=` and `operator <=`</span></span>

### <a name="conversion-operators"></a><span data-ttu-id="6fe1a-1359">轉換運算子</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1359">Conversion operators</span></span>

<span data-ttu-id="6fe1a-1360">轉換運算子宣告會引入***使用者定義的轉換***([使用者定義轉換](conversions.md#user-defined-conversions)) 的增強了預先定義的隱含和明確轉換。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1360">A conversion operator declaration introduces a ***user-defined conversion*** ([User-defined conversions](conversions.md#user-defined-conversions)) which augments the pre-defined implicit and explicit conversions.</span></span>

<span data-ttu-id="6fe1a-1361">包含的轉換運算子宣告`implicit`關鍵字導入了使用者定義的隱含轉換。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1361">A conversion operator declaration that includes the `implicit` keyword introduces a user-defined implicit conversion.</span></span> <span data-ttu-id="6fe1a-1362">在許多情況下，包括函式成員引動過程、 轉型運算式，以及指派可能是隱含轉換。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1362">Implicit conversions can occur in a variety of situations, including function member invocations, cast expressions, and assignments.</span></span> <span data-ttu-id="6fe1a-1363">這是更進一步的說明[隱含轉換](conversions.md#implicit-conversions)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1363">This is described further in [Implicit conversions](conversions.md#implicit-conversions).</span></span>

<span data-ttu-id="6fe1a-1364">包含的轉換運算子宣告`explicit`關鍵字導入了使用者定義的明確轉換。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1364">A conversion operator declaration that includes the `explicit` keyword introduces a user-defined explicit conversion.</span></span> <span data-ttu-id="6fe1a-1365">明確轉換可能會發生在轉型運算式，並會進一步說明，在[明確轉換](conversions.md#explicit-conversions)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1365">Explicit conversions can occur in cast expressions, and are described further in [Explicit conversions](conversions.md#explicit-conversions).</span></span>

<span data-ttu-id="6fe1a-1366">轉換運算子，將轉換成目標型別，以轉換運算子的傳回型別轉換運算子的參數類型所指定來源型別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1366">A conversion operator converts from a source type, indicated by the parameter type of the conversion operator, to a target type, indicated by the return type of the conversion operator.</span></span>

<span data-ttu-id="6fe1a-1367">指定的來源型別的`S`和目標型別`T`，如果`S`或`T`是可為 null 的型別，讓`S0`並`T0`指其基礎類型，否則為`S0`和`T0`是相等`S`和`T`分別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1367">For a given source type `S` and target type `T`, if `S` or `T` are nullable types, let `S0` and `T0` refer to their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span> <span data-ttu-id="6fe1a-1368">類別或結構，並允許宣告從來源類型轉換`S`成目標型別`T`只有當下列所有項目為真：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1368">A class or struct is permitted to declare a conversion from a source type `S` to a target type `T` only if all of the following are true:</span></span>

*  <span data-ttu-id="6fe1a-1369">`S0` 和`T0`是不同的型別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1369">`S0` and `T0` are different types.</span></span>
*  <span data-ttu-id="6fe1a-1370">請`S0`或`T0`是運算子宣告進行的類別或結構類型。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1370">Either `S0` or `T0` is the class or struct type in which the operator declaration takes place.</span></span>
*  <span data-ttu-id="6fe1a-1371">既不`S0`也`T0`是*interface_type*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1371">Neither `S0` nor `T0` is an *interface_type*.</span></span>
*  <span data-ttu-id="6fe1a-1372">排除使用者定義的轉換，轉換沒有從`S`來`T`或來自`T`至`S`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1372">Excluding user-defined conversions, a conversion does not exist from `S` to `T` or from `T` to `S`.</span></span>

<span data-ttu-id="6fe1a-1373">基於這些規則的目的，任何型別參數相關聯`S`或`T`會被視為具有其他類型，任何繼承關聯性和參數會忽略這些型別上的任何條件約束的唯一型別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1373">For the purposes of these rules, any type parameters associated with `S` or `T` are considered to be unique types that have no inheritance relationship with other types, and any constraints on those type parameters are ignored.</span></span>

<span data-ttu-id="6fe1a-1374">在範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1374">In the example</span></span>
```csharp
class C<T> {...}

class D<T>: C<T>
{
    public static implicit operator C<int>(D<T> value) {...}      // Ok
    public static implicit operator C<string>(D<T> value) {...}   // Ok
    public static implicit operator C<T>(D<T> value) {...}        // Error
}
```
<span data-ttu-id="6fe1a-1375">第一次的兩個運算子宣告都受到允許，因為目的[索引子](classes.md#indexers).3`T`並`int`和`string`分別會被視為唯一的型別沒有關聯性。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1375">the first two operator declarations are permitted because, for the purposes of [Indexers](classes.md#indexers).3, `T` and `int` and `string` respectively are considered unique types with no relationship.</span></span> <span data-ttu-id="6fe1a-1376">不過，第三個運算子是錯誤，因為`C<T>`的基底類別的`D<T>`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1376">However, the third operator is an error because `C<T>` is the base class of `D<T>`.</span></span>

<span data-ttu-id="6fe1a-1377">從第二個規則，轉換運算子必須轉換至或自宣告該運算子的類別或結構類型。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1377">From the second rule it follows that a conversion operator must convert either to or from the class or struct type in which the operator is declared.</span></span> <span data-ttu-id="6fe1a-1378">比方說，就可以對類別或結構類型`C`來定義轉換`C`來`int`進出`int`來`C`，而不是從`int`至`bool`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1378">For example, it is possible for a class or struct type `C` to define a conversion from `C` to `int` and from `int` to `C`, but not from `int` to `bool`.</span></span>

<span data-ttu-id="6fe1a-1379">您不可能直接重新定義預先定義的轉換。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1379">It is not possible to directly redefine a pre-defined conversion.</span></span> <span data-ttu-id="6fe1a-1380">因此，轉換運算子不會允許將轉換至或從中`object`存在於之間的隱含和明確轉換因為`object`和所有其他型別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1380">Thus, conversion operators are not allowed to convert from or to `object` because implicit and explicit conversions already exist between `object` and all other types.</span></span> <span data-ttu-id="6fe1a-1381">同樣地，來源和轉換的目標型別都不可以是基底類型的另一個，因為轉換之前就已經存在。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1381">Likewise, neither the source nor the target types of a conversion can be a base type of the other, since a conversion would then already exist.</span></span>

<span data-ttu-id="6fe1a-1382">不過，就可以宣告泛型型別上，對於特定類型引數，指定已存在的轉換為預先定義的轉換的運算子。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1382">However, it is possible to declare operators on generic types that, for particular type arguments, specify conversions that already exist as pre-defined conversions.</span></span> <span data-ttu-id="6fe1a-1383">在範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1383">In the example</span></span>
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
<span data-ttu-id="6fe1a-1384">類型時`object`指定為型別引數`T`，第二個運算子會將宣告轉換已經存在 (隱含的因此也可以是明確轉換存在從任何型別型別`object`)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1384">when type `object` is specified as a type argument for `T`, the second operator declares a conversion that already exists (an implicit, and therefore also an explicit, conversion exists from any type to type `object`).</span></span>

<span data-ttu-id="6fe1a-1385">在兩個類型之間的預先定義的轉換存在的情況下，會忽略任何使用者定義類型之間轉換這些。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1385">In cases where a pre-defined conversion exists between two types, any user-defined conversions between those types are ignored.</span></span> <span data-ttu-id="6fe1a-1386">尤其是：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1386">Specifically:</span></span>

*  <span data-ttu-id="6fe1a-1387">如果預先定義的隱含轉換 ([隱含轉換](conversions.md#implicit-conversions)) 從類型`S`鍵入`T`，所有使用者定義轉換 （隱含或明確） 從`S`至`T`都會被忽略。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1387">If a pre-defined implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from type `S` to type `T`, all user-defined conversions (implicit or explicit) from `S` to `T` are ignored.</span></span>
*  <span data-ttu-id="6fe1a-1388">如果預先定義的明確轉換 ([明確轉換](conversions.md#explicit-conversions)) 從類型`S`鍵入`T`，從，任何使用者定義的明確轉換`S`至`T`都會被忽略。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1388">If a pre-defined explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) exists from type `S` to type `T`, any user-defined explicit conversions from `S` to `T` are ignored.</span></span> <span data-ttu-id="6fe1a-1389">此外：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1389">Furthermore:</span></span>

<span data-ttu-id="6fe1a-1390">如果`T`介面類型時，使用者定義的隱含地轉換`S`到`T`都會被忽略。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1390">If `T` is an interface type, user-defined implicit conversions from `S` to `T` are ignored.</span></span>

<span data-ttu-id="6fe1a-1391">否則，使用者定義的隱含轉換，從`S`至`T`仍會視為。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1391">Otherwise, user-defined implicit conversions from `S` to `T` are still considered.</span></span>

<span data-ttu-id="6fe1a-1392">所有類型，但`object`，由宣告運算子`Convertible<T>`上述類型預先定義的轉換不會衝突。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1392">For all types but `object`, the operators declared by the `Convertible<T>` type above do not conflict with pre-defined conversions.</span></span> <span data-ttu-id="6fe1a-1393">例如: </span><span class="sxs-lookup"><span data-stu-id="6fe1a-1393">For example:</span></span>
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

<span data-ttu-id="6fe1a-1394">不過，對於型別`object`，預先定義的轉換會隱藏在所有情況下，但是其中一種使用者定義的轉換：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1394">However, for type `object`, pre-defined conversions hide the user-defined conversions in all cases but one:</span></span>

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

<span data-ttu-id="6fe1a-1395">使用者定義的轉換不允許將轉換至或從中*interface_type*s。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1395">User-defined conversions are not allowed to convert from or to *interface_type*s.</span></span> <span data-ttu-id="6fe1a-1396">特別的是，這項限制可確保沒有使用者定義轉換發生時將轉換成*interface_type*，且以轉換成*interface_type*才會成功的物件正在轉換實際實作指定*interface_type*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1396">In particular, this restriction ensures that no user-defined transformations occur when converting to an *interface_type*, and that a conversion to an *interface_type* succeeds only if the object being converted actually implements the specified *interface_type*.</span></span>

<span data-ttu-id="6fe1a-1397">轉換運算子的簽章是由來源類型和目標型別所組成。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1397">The signature of a conversion operator consists of the source type and the target type.</span></span> <span data-ttu-id="6fe1a-1398">（請注意這是為其傳回類型會參與簽章的成員的唯一形式）。`implicit`或`explicit`轉換運算子的分類不是運算子的簽章的一部分。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1398">(Note that this is the only form of member for which the return type participates in the signature.) The `implicit` or `explicit` classification of a conversion operator is not part of the operator's signature.</span></span> <span data-ttu-id="6fe1a-1399">因此，類別或結構不能宣告兩者`implicit`和`explicit`具有相同的來源和目標類型的轉換運算子。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1399">Thus, a class or struct cannot declare both an `implicit` and an `explicit` conversion operator with the same source and target types.</span></span>

<span data-ttu-id="6fe1a-1400">一般情況下，應該永遠不會擲回例外狀況，並不會遺失資訊設計使用者定義的隱含轉換。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1400">In general, user-defined implicit conversions should be designed to never throw exceptions and never lose information.</span></span> <span data-ttu-id="6fe1a-1401">如果使用者定義的轉換可能會導致大量例外狀況 （例如，因為來源引數超出範圍） 或遺失的資訊 （例如捨棄高序位位元），則該轉換應該定義為明確的轉換。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1401">If a user-defined conversion can give rise to exceptions (for example, because the source argument is out of range) or loss of information (such as discarding high-order bits), then that conversion should be defined as an explicit conversion.</span></span>

<span data-ttu-id="6fe1a-1402">在範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1402">In the example</span></span>
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
<span data-ttu-id="6fe1a-1403">從轉換`Digit`要`byte`是隱含的因為它永遠不會擲回例外狀況或遺失的詳細資訊，但從轉換`byte`要`Digit`是因為明確`Digit`只能表示可能的子集值`byte`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1403">the conversion from `Digit` to `byte` is implicit because it never throws exceptions or loses information, but the conversion from `byte` to `Digit` is explicit since `Digit` can only represent a subset of the possible values of a `byte`.</span></span>

## <a name="instance-constructors"></a><span data-ttu-id="6fe1a-1404">執行個體建構函式</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1404">Instance constructors</span></span>

<span data-ttu-id="6fe1a-1405">「執行個體建構函式」是實作將類別執行個體初始化所需之動作的成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1405">An ***instance constructor*** is a member that implements the actions required to initialize an instance of a class.</span></span> <span data-ttu-id="6fe1a-1406">執行個體建構函式使用宣告*constructor_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1406">Instance constructors are declared using *constructor_declaration*s:</span></span>

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

<span data-ttu-id="6fe1a-1407">A *constructor_declaration*可能包含一組*屬性*([屬性](attributes.md))，是有效的四種存取修飾詞組合 ([存取修飾詞](classes.md#access-modifiers))，以及`extern`([外部方法](classes.md#external-methods)) 修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1407">A *constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and an `extern` ([External methods](classes.md#external-methods)) modifier.</span></span> <span data-ttu-id="6fe1a-1408">建構函式宣告不允許多次包含相同的修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1408">A constructor declaration is not permitted to include the same modifier multiple times.</span></span>

<span data-ttu-id="6fe1a-1409">*識別碼*的*constructor_declarator*必須命名的執行個體建構函式宣告所在的類別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1409">The *identifier* of a *constructor_declarator* must name the class in which the instance constructor is declared.</span></span> <span data-ttu-id="6fe1a-1410">如果指定的任何其他名稱，則會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1410">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="6fe1a-1411">選擇性*formal_parameter_list*執行個體的建構函式受限於相同的規則*formal_parameter_list*方法的 ([方法](classes.md#methods))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1411">The optional *formal_parameter_list* of an instance constructor is subject to the same rules as the *formal_parameter_list* of a method ([Methods](classes.md#methods)).</span></span> <span data-ttu-id="6fe1a-1412">型式參數清單會定義簽章 ([簽章和多載](basic-concepts.md#signatures-and-overloading)) 的執行個體建構函式，並且讓管理程序多載解析 ([型別推斷](expressions.md#type-inference)) 選取特定用引動過程中的執行個體建構函式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1412">The formal parameter list defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of an instance constructor and governs the process whereby overload resolution ([Type inference](expressions.md#type-inference)) selects a particular instance constructor in an invocation.</span></span>

<span data-ttu-id="6fe1a-1413">每個參考中的型別*formal_parameter_list*執行個體的建構函式必須是至少一樣地存取自己的建構函式 ([存取範圍條件約束](basic-concepts.md#accessibility-constraints))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1413">Each of the types referenced in the *formal_parameter_list* of an instance constructor must be at least as accessible as the constructor itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="6fe1a-1414">選擇性*constructor_initializer*指定執行中的陳述式之前叫用的另一個執行個體建構函式*constructor_body*這個執行個體建構函式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1414">The optional *constructor_initializer* specifies another instance constructor to invoke before executing the statements given in the *constructor_body* of this instance constructor.</span></span> <span data-ttu-id="6fe1a-1415">這是更進一步的說明[建構函式初始設定式](classes.md#constructor-initializers)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1415">This is described further in [Constructor initializers](classes.md#constructor-initializers).</span></span>

<span data-ttu-id="6fe1a-1416">當建構函式宣告包含`extern`修飾詞，建構函式即為***外部的建構函式***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1416">When a constructor declaration includes an `extern` modifier, the constructor is said to be an ***external constructor***.</span></span> <span data-ttu-id="6fe1a-1417">因為外部的建構函式宣告未不提供任何實際的實作，其*constructor_body*只包含一個分號。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1417">Because an external constructor declaration provides no actual implementation, its *constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="6fe1a-1418">所有其他的建構函式，如*constructor_body*組成*區塊*指定的陳述式來初始化類別的新執行個體。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1418">For all other constructors, the *constructor_body* consists of a *block* which specifies the statements to initialize a new instance of the class.</span></span> <span data-ttu-id="6fe1a-1419">這就相當於*區塊*的執行個體方法`void`傳回型別 ([方法主體](classes.md#method-body))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1419">This corresponds exactly to the *block* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="6fe1a-1420">執行個體建構函式不會繼承。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1420">Instance constructors are not inherited.</span></span> <span data-ttu-id="6fe1a-1421">因此，類別具有以外實際上在類別中宣告的任何執行個體建構函式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1421">Thus, a class has no instance constructors other than those actually declared in the class.</span></span> <span data-ttu-id="6fe1a-1422">如果類別未不包含任何執行個體建構函式宣告，會自動提供的預設執行個體建構函式 ([預設建構函式](classes.md#default-constructors))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1422">If a class contains no instance constructor declarations, a default instance constructor is automatically provided ([Default constructors](classes.md#default-constructors)).</span></span>

<span data-ttu-id="6fe1a-1423">執行個體建構函式會叫用*object_creation_expression*s ([物件建立運算式](expressions.md#object-creation-expressions)) 及透過*constructor_initializer*s。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1423">Instance constructors are invoked by *object_creation_expression*s ([Object creation expressions](expressions.md#object-creation-expressions)) and through *constructor_initializer*s.</span></span>

### <a name="constructor-initializers"></a><span data-ttu-id="6fe1a-1424">建構函式初始設定式</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1424">Constructor initializers</span></span>

<span data-ttu-id="6fe1a-1425">所有執行個體建構函式 (但不包括類別`object`)，隱含地包含另一個執行個體建構函式的引動過程正前方*constructor_body*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1425">All instance constructors (except those for class `object`) implicitly include an invocation of another instance constructor immediately before the *constructor_body*.</span></span> <span data-ttu-id="6fe1a-1426">隱含地叫用建構函式由*constructor_initializer*:</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1426">The constructor to implicitly invoke is determined by the *constructor_initializer*:</span></span>

*  <span data-ttu-id="6fe1a-1427">表單的執行個體建構函式初始設定式`base(argument_list)`或`base()`會導致叫用直接基底類別的執行個體建構函式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1427">An instance constructor initializer of the form `base(argument_list)` or `base()` causes an instance constructor from the direct base class to be invoked.</span></span> <span data-ttu-id="6fe1a-1428">使用選取該建構函式*argument_list*如果存在且多載解析規則[多載解析](expressions.md#overload-resolution)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1428">That constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="6fe1a-1429">候選項目執行個體建構函式的集合，包含所有可存取的執行個體建構函式的直接基底類別中所包含的預設建構函式 ([預設建構函式](classes.md#default-constructors))，如果沒有執行個體建構函式宣告中直接基底類別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1429">The set of candidate instance constructors consists of all accessible instance constructors contained in the direct base class, or the default constructor ([Default constructors](classes.md#default-constructors)), if no instance constructors are declared in the direct base class.</span></span> <span data-ttu-id="6fe1a-1430">如果這個集合是空的或如果找不到單一最佳的執行個體建構函式，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1430">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span>
*  <span data-ttu-id="6fe1a-1431">表單的執行個體建構函式初始設定式`this(argument-list)`或`this()`會導致從本身要叫用類別的執行個體建構函式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1431">An instance constructor initializer of the form `this(argument-list)` or `this()` causes an instance constructor from the class itself to be invoked.</span></span> <span data-ttu-id="6fe1a-1432">使用選取的建構函式*argument_list*如果存在且多載解析規則[多載解析](expressions.md#overload-resolution)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1432">The constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="6fe1a-1433">候選項目執行個體建構函式的集合是由所有可存取的執行個體建構函式宣告中類別本身所組成。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1433">The set of candidate instance constructors consists of all accessible instance constructors declared in the class itself.</span></span> <span data-ttu-id="6fe1a-1434">如果這個集合是空的或如果找不到單一最佳的執行個體建構函式，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1434">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span> <span data-ttu-id="6fe1a-1435">如果執行個體建構函式宣告包含本身的建構函式會叫用的建構函式初始設定式，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1435">If an instance constructor declaration includes a constructor initializer that invokes the constructor itself, a compile-time error occurs.</span></span>

<span data-ttu-id="6fe1a-1436">如果執行個體建構函式有沒有建構函式初始設定式，在表單的建構函式初始設定式`base()`以隱含方式提供。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1436">If an instance constructor has no constructor initializer, a constructor initializer of the form `base()` is implicitly provided.</span></span> <span data-ttu-id="6fe1a-1437">因此，表單執行個體建構函式宣告</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1437">Thus, an instance constructor declaration of the form</span></span>
```csharp
C(...) {...}
```
<span data-ttu-id="6fe1a-1438">就完全相當於</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1438">is exactly equivalent to</span></span>
```csharp
C(...): base() {...}
```

<span data-ttu-id="6fe1a-1439">所指定之參數的範圍*formal_parameter_list*的執行個體建構函式的宣告包含宣告的建構函式初始設定式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1439">The scope of the parameters given by the *formal_parameter_list* of an instance constructor declaration includes the constructor initializer of that declaration.</span></span> <span data-ttu-id="6fe1a-1440">因此，建構函式初始設定式可以存取的建構函式的參數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1440">Thus, a constructor initializer is permitted to access the parameters of the constructor.</span></span> <span data-ttu-id="6fe1a-1441">例如: </span><span class="sxs-lookup"><span data-stu-id="6fe1a-1441">For example:</span></span>
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

<span data-ttu-id="6fe1a-1442">執行個體建構函式初始設定式無法存取所建立的執行個體。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1442">An instance constructor initializer cannot access the instance being created.</span></span> <span data-ttu-id="6fe1a-1443">因此它是編譯時期錯誤參考`this`建構函式初始設定式的引數運算式，做為是它的任何執行個體透過參考成員的引數運算式的編譯時期錯誤*simple_name*.</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1443">Therefore it is a compile-time error to reference `this` in an argument expression of the constructor initializer, as is it a compile-time error for an argument expression to reference any instance member through a *simple_name*.</span></span>

### <a name="instance-variable-initializers"></a><span data-ttu-id="6fe1a-1444">執行個體變數初始設定式</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1444">Instance variable initializers</span></span>

<span data-ttu-id="6fe1a-1445">當執行個體建構函式有沒有建構函式初始設定式，或它有表單的建構函式初始設定式`base(...)`，該建構函式會隱含地執行所指定的初始化*variable_initializer*的 s在其類別中，宣告的執行個體欄位。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1445">When an instance constructor has no constructor initializer, or it has a constructor initializer of the form `base(...)`, that constructor implicitly performs the initializations specified by the *variable_initializer*s of the instance fields declared in its class.</span></span> <span data-ttu-id="6fe1a-1446">這會對應到一系列的項目建構函式，以及直接基底類別建構函式的隱含引動過程之前立即執行的設定。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1446">This corresponds to a sequence of assignments that are executed immediately upon entry to the constructor and before the implicit invocation of the direct base class constructor.</span></span> <span data-ttu-id="6fe1a-1447">變數的初始設定式會以其出現在類別宣告中的文字順序執行。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1447">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span>

### <a name="constructor-execution"></a><span data-ttu-id="6fe1a-1448">建構函式執行</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1448">Constructor execution</span></span>

<span data-ttu-id="6fe1a-1449">變數初始設定式會轉換成指派陳述式，以及這些指派陳述式會在基底類別的執行個體建構函式的引動過程之前執行。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1449">Variable initializers are transformed into assignment statements, and these assignment statements are executed before the invocation of the base class instance constructor.</span></span> <span data-ttu-id="6fe1a-1450">這種排序可確保能夠存取該執行個體的任何陳述式會在執行之前，會由其區域變數初始設定式初始化所有執行個體欄位。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1450">This ordering ensures that all instance fields are initialized by their variable initializers before any statements that have access to that instance are executed.</span></span>

<span data-ttu-id="6fe1a-1451">提供的範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1451">Given the example</span></span>
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
<span data-ttu-id="6fe1a-1452">當`new B()`用來建立的執行個體`B`，會產生下列輸出：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1452">when `new B()` is used to create an instance of `B`, the following output is produced:</span></span>
```
x = 1, y = 0
```

<span data-ttu-id="6fe1a-1453">值`x`為 1，因為變數的初始設定式會在基底類別的執行個體建構函式會叫用之前執行。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1453">The value of `x` is 1 because the variable initializer is executed before the base class instance constructor is invoked.</span></span> <span data-ttu-id="6fe1a-1454">不過，值`y`為 0 (預設值`int`) 因為指派給`y`之前不會執行基底類別建構函式傳回之後。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1454">However, the value of `y` is 0 (the default value of an `int`) because the assignment to `y` is not executed until after the base class constructor returns.</span></span>

<span data-ttu-id="6fe1a-1455">您最好將視為自動插入之前的陳述式執行個體變數初始設定式和建構函式初始設定式*constructor_body*。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1455">It is useful to think of instance variable initializers and constructor initializers as statements that are automatically inserted before the *constructor_body*.</span></span> <span data-ttu-id="6fe1a-1456">此範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1456">The example</span></span>
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
<span data-ttu-id="6fe1a-1457">包含數個變數初始設定式;它也包含這兩種形式的建構函式初始設定式 (`base`和`this`)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1457">contains several variable initializers; it also contains constructor initializers of both forms (`base` and `this`).</span></span> <span data-ttu-id="6fe1a-1458">此範例相當於程式碼如下所示，其中每個註解指出會自動插入的陳述式 （用於自動插入建構函式引動過程的語法無效，但只提供可說明此機制）。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1458">The example corresponds to the code shown below, where each comment indicates an automatically inserted statement (the syntax used for the automatically inserted constructor invocations isn't valid, but merely serves to illustrate the mechanism).</span></span>

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

### <a name="default-constructors"></a><span data-ttu-id="6fe1a-1459">預設建構函式</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1459">Default constructors</span></span>

<span data-ttu-id="6fe1a-1460">如果類別未不包含任何執行個體建構函式宣告，會自動提供的預設執行個體建構函式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1460">If a class contains no instance constructor declarations, a default instance constructor is automatically provided.</span></span> <span data-ttu-id="6fe1a-1461">該預設建構函式只會叫用直接基底類別的無參數建構函式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1461">That default constructor simply invokes the parameterless constructor of the direct base class.</span></span> <span data-ttu-id="6fe1a-1462">如果類別是抽象的是受保護的預設建構函式的宣告存取範圍。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1462">If the class is abstract then the declared accessibility for the default constructor is protected.</span></span> <span data-ttu-id="6fe1a-1463">否則，預設建構函式的宣告存取範圍是公用的。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1463">Otherwise, the declared accessibility for the default constructor is public.</span></span> <span data-ttu-id="6fe1a-1464">因此，預設建構函式一律會是表單</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1464">Thus, the default constructor is always of the form</span></span>

```csharp
protected C(): base() {}
```
<span data-ttu-id="6fe1a-1465">或</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1465">or</span></span>
```csharp
public C(): base() {}
```
<span data-ttu-id="6fe1a-1466">其中`C`是類別的名稱。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1466">where `C` is the name of the class.</span></span> <span data-ttu-id="6fe1a-1467">如果多載解析無法判斷基底類別建構函式初始設定式的唯一最佳候選項目，則會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1467">If overload resolution is unable to determine a unique best candidate for the base class constructor initializer then a compile-time error occurs.</span></span>

<span data-ttu-id="6fe1a-1468">在範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1468">In the example</span></span>
```csharp
class Message
{
    object sender;
    string text;
}
```
<span data-ttu-id="6fe1a-1469">提供的預設建構函式，因為這個類別不包含任何執行個體建構函式宣告。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1469">a default constructor is provided because the class contains no instance constructor declarations.</span></span> <span data-ttu-id="6fe1a-1470">因此，範例就相當於</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1470">Thus, the example is precisely equivalent to</span></span>
```csharp
class Message
{
    object sender;
    string text;

    public Message(): base() {}
}
```

### <a name="private-constructors"></a><span data-ttu-id="6fe1a-1471">私用建構函式</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1471">Private constructors</span></span>

<span data-ttu-id="6fe1a-1472">當類別`T`宣告只有私用執行個體建構函式，它無法進行類別外的程式文字`T`衍生自`T`，或直接建立的執行個體`T`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1472">When a class `T` declares only private instance constructors, it is not possible for classes outside the program text of `T` to derive from `T` or to directly create instances of `T`.</span></span> <span data-ttu-id="6fe1a-1473">因此，如果類別只包含靜態成員，而且不想要具現化，加入空白的私用執行個體建構函式會造成具現化。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1473">Thus, if a class contains only static members and isn't intended to be instantiated, adding an empty private instance constructor will prevent instantiation.</span></span> <span data-ttu-id="6fe1a-1474">例如: </span><span class="sxs-lookup"><span data-stu-id="6fe1a-1474">For example:</span></span>
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

<span data-ttu-id="6fe1a-1475">`Trig`類別群組相關的方法與常數，但不是要具現化。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1475">The `Trig` class groups related methods and constants, but is not intended to be instantiated.</span></span> <span data-ttu-id="6fe1a-1476">因此它會宣告單一空白的私用執行個體建構函式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1476">Therefore it declares a single empty private instance constructor.</span></span> <span data-ttu-id="6fe1a-1477">必須宣告至少一個執行個體建構函式，來隱藏自動產生預設建構函式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1477">At least one instance constructor must be declared to suppress the automatic generation of a default constructor.</span></span>

### <a name="optional-instance-constructor-parameters"></a><span data-ttu-id="6fe1a-1478">選擇性的執行個體建構函式參數</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1478">Optional instance constructor parameters</span></span>

<span data-ttu-id="6fe1a-1479">`this(...)`形式的建構函式初始設定式通常用於搭配多載實作選擇性的執行個體建構函式參數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1479">The `this(...)` form of constructor initializer is commonly used in conjunction with overloading to implement optional instance constructor parameters.</span></span> <span data-ttu-id="6fe1a-1480">在範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1480">In the example</span></span>
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
<span data-ttu-id="6fe1a-1481">第一次的兩個執行個體建構函式只是提供遺漏的引數的預設值。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1481">the first two instance constructors merely provide the default values for the missing arguments.</span></span> <span data-ttu-id="6fe1a-1482">兩者都使用`this(...)`建構函式初始設定式來叫用第三個執行個體建構函式，它會實際初始化新執行個體的工作。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1482">Both use a `this(...)` constructor initializer to invoke the third instance constructor, which actually does the work of initializing the new instance.</span></span> <span data-ttu-id="6fe1a-1483">效果是選擇性的建構函式參數：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1483">The effect is that of optional constructor parameters:</span></span>
```csharp
Text t1 = new Text();                    // Same as Text(0, 0, null)
Text t2 = new Text(5, 10);               // Same as Text(5, 10, null)
Text t3 = new Text(5, 20, "Hello");
```

## <a name="static-constructors"></a><span data-ttu-id="6fe1a-1484">靜態建構函式</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1484">Static constructors</span></span>

<span data-ttu-id="6fe1a-1485">A***靜態建構函式***是實作初始化封閉的類別型別所需之動作的成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1485">A ***static constructor*** is a member that implements the actions required to initialize a closed class type.</span></span> <span data-ttu-id="6fe1a-1486">靜態建構函式使用宣告*static_constructor_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1486">Static constructors are declared using *static_constructor_declaration*s:</span></span>

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

<span data-ttu-id="6fe1a-1487">A *static_constructor_declaration*可能包含一組*屬性*([屬性](attributes.md)) 以及`extern`修飾詞 ([外部方法](classes.md#external-methods)).</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1487">A *static_constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and an `extern` modifier ([External methods](classes.md#external-methods)).</span></span>

<span data-ttu-id="6fe1a-1488">*識別碼*的*static_constructor_declaration*必須命名為在靜態建構函式宣告的類別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1488">The *identifier* of a *static_constructor_declaration* must name the class in which the static constructor is declared.</span></span> <span data-ttu-id="6fe1a-1489">如果指定的任何其他名稱，則會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1489">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="6fe1a-1490">當靜態建構函式宣告包含`extern`修飾詞，靜態建構函式即為***外部的靜態建構函式***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1490">When a static constructor declaration includes an `extern` modifier, the static constructor is said to be an ***external static constructor***.</span></span> <span data-ttu-id="6fe1a-1491">因為外部的靜態建構函式宣告未不提供任何實際的實作，其*static_constructor_body*只包含一個分號。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1491">Because an external static constructor declaration provides no actual implementation, its *static_constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="6fe1a-1492">所有其他的靜態建構函式宣告中， *static_constructor_body*組成*區塊*指定要執行以便初始化類別的陳述式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1492">For all other static constructor declarations, the *static_constructor_body* consists of a *block* which specifies the statements to execute in order to initialize the class.</span></span> <span data-ttu-id="6fe1a-1493">這就相當於*method_body*使用的靜態方法`void`傳回型別 ([方法主體](classes.md#method-body))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1493">This corresponds exactly to the *method_body* of a static method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="6fe1a-1494">靜態建構函式不會繼承，並不能直接呼叫。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1494">Static constructors are not inherited, and cannot be called directly.</span></span>

<span data-ttu-id="6fe1a-1495">已關閉的類別類型的靜態建構函式最多一次執行指定的應用程式定義域中。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1495">The static constructor for a closed class type executes at most once in a given application domain.</span></span> <span data-ttu-id="6fe1a-1496">靜態建構函式的執行是由應用程式定義域中發生下列事件的第一個觸發：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1496">The execution of a static constructor is triggered by the first of the following events to occur within an application domain:</span></span>

*  <span data-ttu-id="6fe1a-1497">建立類別類型的執行個體。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1497">An instance of the class type is created.</span></span>
*  <span data-ttu-id="6fe1a-1498">參考的任何類別類型的靜態成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1498">Any of the static members of the class type are referenced.</span></span>

<span data-ttu-id="6fe1a-1499">如果類別包含`Main`方法 ([應用程式啟動](basic-concepts.md#application-startup)) 中執行開始時，靜態建構函式之前執行該類別的`Main`呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1499">If a class contains the `Main` method ([Application Startup](basic-concepts.md#application-startup)) in which execution begins, the static constructor for that class executes before the `Main` method is called.</span></span>

<span data-ttu-id="6fe1a-1500">若要初始化新的已關閉的類別類型，第一次一組新的靜態欄位 ([靜態和執行個體欄位](classes.md#static-and-instance-fields)) 會建立該特定的封閉型別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1500">To initialize a new closed class type, first a new set of static fields ([Static and instance fields](classes.md#static-and-instance-fields)) for that particular closed type is created.</span></span> <span data-ttu-id="6fe1a-1501">每個靜態欄位初始化為其預設值 ([預設值](variables.md#default-values))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1501">Each of the static fields is initialized to its default value ([Default values](variables.md#default-values)).</span></span> <span data-ttu-id="6fe1a-1502">下一步，靜態欄位初始設定式 ([靜態欄位初始化](classes.md#static-field-initialization)) 會執行這些靜態欄位。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1502">Next, the static field initializers ([Static field initialization](classes.md#static-field-initialization)) are executed for those static fields.</span></span> <span data-ttu-id="6fe1a-1503">最後，靜態建構函式會執行。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1503">Finally, the static constructor is executed.</span></span>

<span data-ttu-id="6fe1a-1504">此範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1504">The example</span></span>
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
<span data-ttu-id="6fe1a-1505">必須產生的輸出：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1505">must produce the output:</span></span>
```
Init A
A.F
Init B
B.F
```
<span data-ttu-id="6fe1a-1506">因為執行`A`的靜態建構函式的呼叫所觸發`A.F`，及執行`B`的靜態建構函式的呼叫所觸發`B.F`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1506">because the execution of `A`'s static constructor is triggered by the call to `A.F`, and the execution of `B`'s static constructor is triggered by the call to `B.F`.</span></span>

<span data-ttu-id="6fe1a-1507">就可以建構允許使用變數的初始設定式的預設值狀態的靜態欄位的循環相依性。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1507">It is possible to construct circular dependencies that allow static fields with variable initializers to be observed in their default value state.</span></span>

<span data-ttu-id="6fe1a-1508">此範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1508">The example</span></span>
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
<span data-ttu-id="6fe1a-1509">產生下列輸出</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1509">produces the output</span></span>
```
X = 1, Y = 2
```

<span data-ttu-id="6fe1a-1510">若要執行`Main`方法，系統第一次執行的初始設定式`B.Y`類別之前，`B`的靜態建構函式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1510">To execute the `Main` method, the system first runs the initializer for `B.Y`, prior to class `B`'s static constructor.</span></span> <span data-ttu-id="6fe1a-1511">`Y`初始設定式會導致`A`的執行，因為靜態建構函式的值`A.X`參考。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1511">`Y`'s initializer causes `A`'s static constructor to be run because the value of `A.X` is referenced.</span></span> <span data-ttu-id="6fe1a-1512">靜態建構函式 `A`接著會繼續計算的值 `X`，並在此情況下的預設值是提取 `Y`，這是零。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1512">The static constructor of `A` in turn proceeds to compute the value of `X`, and in doing so fetches the default value of `Y`, which is zero.</span></span> <span data-ttu-id="6fe1a-1513">`A.X` 因此會初始化為 1。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1513">`A.X` is thus initialized to 1.</span></span> <span data-ttu-id="6fe1a-1514">執行的程序`A`的靜態欄位初始設定式和靜態建構函式，然後完成時，傳回的初始值計算 `Y`，其結果就會變成 2。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1514">The process of running `A`'s static field initializers and static constructor then completes, returning to the calculation of the initial value of `Y`, the result of which becomes 2.</span></span>

<span data-ttu-id="6fe1a-1515">針對每個封閉式建構的類別型別，正好一次執行靜態建構函式，因為它是方便的地方，以強制執行無法在編譯時期透過條件約束檢查的型別參數的執行階段檢查 ([型別參數條件約束](classes.md#type-parameter-constraints))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1515">Because the static constructor is executed exactly once for each closed constructed class type, it is a convenient place to enforce run-time checks on the type parameter that cannot be checked at compile-time via constraints ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="6fe1a-1516">例如，下列類型會使用靜態建構函式來強制執行型別引數是列舉：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1516">For example, the following type uses a static constructor to enforce that the type argument is an enum:</span></span>
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

## <a name="destructors"></a><span data-ttu-id="6fe1a-1517">解構函式</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1517">Destructors</span></span>

<span data-ttu-id="6fe1a-1518">A***解構函式***成員實作來解構類別的執行個體所需的動作。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1518">A ***destructor*** is a member that implements the actions required to destruct an instance of a class.</span></span> <span data-ttu-id="6fe1a-1519">使用宣告解構函式*destructor_declaration*:</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1519">A destructor is declared using a *destructor_declaration*:</span></span>

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

<span data-ttu-id="6fe1a-1520">A *destructor_declaration*可能包含一組*屬性*([屬性](attributes.md))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1520">A *destructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="6fe1a-1521">*識別碼*的*destructor_declaration*宣告解構函式的類別必須命名。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1521">The *identifier* of a *destructor_declaration* must name the class in which the destructor is declared.</span></span> <span data-ttu-id="6fe1a-1522">如果指定的任何其他名稱，則會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1522">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="6fe1a-1523">當解構函式宣告包含`extern`修飾詞，解構函式即為***外部的解構函式***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1523">When a destructor declaration includes an `extern` modifier, the destructor is said to be an ***external destructor***.</span></span> <span data-ttu-id="6fe1a-1524">因為外部的解構函式宣告未不提供任何實際的實作，其*destructor_body*只包含一個分號。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1524">Because an external destructor declaration provides no actual implementation, its *destructor_body* consists of a semicolon.</span></span> <span data-ttu-id="6fe1a-1525">對於所有其他的解構函式， *destructor_body*組成*區塊*指定要執行以便解構類別的執行個體的陳述式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1525">For all other destructors, the *destructor_body* consists of a *block* which specifies the statements to execute in order to destruct an instance of the class.</span></span> <span data-ttu-id="6fe1a-1526">A *destructor_body*就相當於*method_body*的執行個體方法`void`傳回型別 ([方法主體](classes.md#method-body))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1526">A *destructor_body* corresponds exactly to the *method_body* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="6fe1a-1527">解構函式不會繼承。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1527">Destructors are not inherited.</span></span> <span data-ttu-id="6fe1a-1528">因此，類別也有可能會在該類別宣告以外的任何解構函式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1528">Thus, a class has no destructors other than the one which may be declared in that class.</span></span>

<span data-ttu-id="6fe1a-1529">解構函式需要不有任何參數，因為它不能多載的情況，因此類別可以有，最多一個解構函式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1529">Since a destructor is required to have no parameters, it cannot be overloaded, so a class can have, at most, one destructor.</span></span>

<span data-ttu-id="6fe1a-1530">解構函式會自動叫用，並無法明確地叫用。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1530">Destructors are invoked automatically, and cannot be invoked explicitly.</span></span> <span data-ttu-id="6fe1a-1531">已不再使用該執行個體的任何程式碼時，適合進行解構執行個體。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1531">An instance becomes eligible for destruction when it is no longer possible for any code to use that instance.</span></span> <span data-ttu-id="6fe1a-1532">執行執行個體的解構函式可能會發生在任何時間之後的執行個體會變成適合進行解構。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1532">Execution of the destructor for the instance may occur at any time after the instance becomes eligible for destruction.</span></span> <span data-ttu-id="6fe1a-1533">當解構執行個體時，該執行個體的繼承鏈結中的解構函式呼叫時，依序以最多衍生至最少衍生。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1533">When an instance is destructed, the destructors in that instance's inheritance chain are called, in order, from most derived to least derived.</span></span> <span data-ttu-id="6fe1a-1534">解構函式可能會在任何執行緒上執行。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1534">A destructor may be executed on any thread.</span></span> <span data-ttu-id="6fe1a-1535">如進一步討論管理何時及如何執行解構函式的規則，請參閱[自動記憶體管理](basic-concepts.md#automatic-memory-management)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1535">For further discussion of the rules that govern when and how a destructor is executed, see [Automatic memory management](basic-concepts.md#automatic-memory-management).</span></span>

<span data-ttu-id="6fe1a-1536">此範例的輸出</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1536">The output of the example</span></span>
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
<span data-ttu-id="6fe1a-1537">是</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1537">is</span></span>
```
B's destructor
A's destructor
```
<span data-ttu-id="6fe1a-1538">因為順序，會呼叫解構函式中的繼承鏈結以最多衍生至最少衍生。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1538">since destructors in an inheritance chain are called in order, from most derived to least derived.</span></span>

<span data-ttu-id="6fe1a-1539">解構函式會藉由覆寫虛擬方法實作`Finalize`上`System.Object`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1539">Destructors are implemented by overriding the virtual method `Finalize` on `System.Object`.</span></span> <span data-ttu-id="6fe1a-1540">C# 程式不允許覆寫這個方法，或呼叫 （或它的覆寫） 直接。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1540">C# programs are not permitted to override this method or call it (or overrides of it) directly.</span></span> <span data-ttu-id="6fe1a-1541">比方說，程式</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1541">For instance, the program</span></span>
```csharp
class A 
{
    override protected void Finalize() {}    // error

    public void F() {
        this.Finalize();                     // error
    }
}
```
<span data-ttu-id="6fe1a-1542">包含兩個錯誤。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1542">contains two errors.</span></span>

<span data-ttu-id="6fe1a-1543">編譯器行為就如同此方法，並覆寫它，進行根本不存在。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1543">The compiler behaves as if this method, and overrides of it, do not exist at all.</span></span> <span data-ttu-id="6fe1a-1544">因此，此程式：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1544">Thus, this program:</span></span>
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
<span data-ttu-id="6fe1a-1545">有效，並顯示隱藏的方法`System.Object`的`Finalize`方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1545">is valid, and the method shown hides `System.Object`'s `Finalize` method.</span></span>

<span data-ttu-id="6fe1a-1546">如需行為的討論的解構函式擲回例外狀況時，請參閱[如何處理例外狀況](exceptions.md#how-exceptions-are-handled)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1546">For a discussion of the behavior when an exception is thrown from a destructor, see [How exceptions are handled](exceptions.md#how-exceptions-are-handled).</span></span>

## <a name="iterators"></a><span data-ttu-id="6fe1a-1547">迭代器</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1547">Iterators</span></span>

<span data-ttu-id="6fe1a-1548">函式成員 ([函式成員](expressions.md#function-members)) 實作使用迭代器區塊 ([區塊](statements.md#blocks)) 稱為***迭代器***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1548">A function member ([Function members](expressions.md#function-members)) implemented using an iterator block ([Blocks](statements.md#blocks)) is called an ***iterator***.</span></span>

<span data-ttu-id="6fe1a-1549">迭代器區塊可能會用做函式成員的主體，只要對應的函式成員的傳回型別是其中一個列舉程式介面 ([列舉程式介面](classes.md#enumerator-interfaces)) 或其中一個可列舉的介面 ([可列舉的介面](classes.md#enumerable-interfaces))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1549">An iterator block may be used as the body of a function member as long as the return type of the corresponding function member is one of the enumerator interfaces ([Enumerator interfaces](classes.md#enumerator-interfaces)) or one of the enumerable interfaces ([Enumerable interfaces](classes.md#enumerable-interfaces)).</span></span> <span data-ttu-id="6fe1a-1550">它可能是以*method_body*， *operator_body*或是*accessor_body*，而事件、 執行個體建構函式、 靜態建構函式和解構函式不可為實作為迭代器。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1550">It can occur as a *method_body*, *operator_body* or *accessor_body*, whereas events, instance constructors, static constructors and destructors cannot be implemented as iterators.</span></span>

<span data-ttu-id="6fe1a-1551">使用迭代器區塊來實作函式成員時，它是型式參數清單中指定任何函式成員的編譯時期錯誤`ref`或`out`參數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1551">When a function member is implemented using an iterator block, it is a compile-time error for the formal parameter list of the function member to specify any `ref` or `out` parameters.</span></span>

### <a name="enumerator-interfaces"></a><span data-ttu-id="6fe1a-1552">列舉程式介面</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1552">Enumerator interfaces</span></span>

<span data-ttu-id="6fe1a-1553">***列舉程式介面***為非泛型介面`System.Collections.IEnumerator`及 泛型介面的所有具現化`System.Collections.Generic.IEnumerator<T>`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1553">The ***enumerator interfaces*** are the non-generic interface `System.Collections.IEnumerator` and all instantiations of the generic interface `System.Collections.Generic.IEnumerator<T>`.</span></span> <span data-ttu-id="6fe1a-1554">為了保持簡潔，在這一章中這些介面會當做`IEnumerator`和`IEnumerator<T>`分別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1554">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerator` and `IEnumerator<T>`, respectively.</span></span>

### <a name="enumerable-interfaces"></a><span data-ttu-id="6fe1a-1555">可列舉的介面</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1555">Enumerable interfaces</span></span>

<span data-ttu-id="6fe1a-1556">***可列舉的介面***為非泛型介面`System.Collections.IEnumerable`及 泛型介面的所有具現化`System.Collections.Generic.IEnumerable<T>`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1556">The ***enumerable interfaces*** are the non-generic interface `System.Collections.IEnumerable` and all instantiations of the generic interface `System.Collections.Generic.IEnumerable<T>`.</span></span> <span data-ttu-id="6fe1a-1557">為了保持簡潔，在這一章中這些介面會當做`IEnumerable`和`IEnumerable<T>`分別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1557">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerable` and `IEnumerable<T>`, respectively.</span></span>

### <a name="yield-type"></a><span data-ttu-id="6fe1a-1558">產生類型</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1558">Yield type</span></span>

<span data-ttu-id="6fe1a-1559">迭代器會產生一連串的值，所有型別相同。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1559">An iterator produces a sequence of values, all of the same type.</span></span> <span data-ttu-id="6fe1a-1560">這種類型稱為***產生類型***迭代器。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1560">This type is called the ***yield type*** of the iterator.</span></span>

*  <span data-ttu-id="6fe1a-1561">傳回迭代器的 yield 類型`IEnumerator`或是`IEnumerable`是`object`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1561">The yield type of an iterator that returns `IEnumerator` or `IEnumerable` is `object`.</span></span>
*  <span data-ttu-id="6fe1a-1562">傳回迭代器的 yield 類型`IEnumerator<T>`或是`IEnumerable<T>`是`T`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1562">The yield type of an iterator that returns `IEnumerator<T>` or `IEnumerable<T>` is `T`.</span></span>

### <a name="enumerator-objects"></a><span data-ttu-id="6fe1a-1563">列舉值物件</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1563">Enumerator objects</span></span>

<span data-ttu-id="6fe1a-1564">使用迭代器區塊來實作介面型別傳回列舉值的函式成員時，叫用函式成員不會立即執行程式碼中的迭代器區塊。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1564">When a function member returning an enumerator interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="6fe1a-1565">相反地，***列舉值物件***會建立並傳回。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1565">Instead, an ***enumerator object*** is created and returned.</span></span> <span data-ttu-id="6fe1a-1566">此物件會封裝在迭代器區塊中，指定的程式碼和的迭代器區塊中的程式碼會執行當 enumerator 物件的`MoveNext`叫用方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1566">This object encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="6fe1a-1567">列舉值物件具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1567">An enumerator object has the following characteristics:</span></span>

*  <span data-ttu-id="6fe1a-1568">它會實作`IEnumerator`並`IEnumerator<T>`，其中`T`iterator 的 yield 類型。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1568">It implements `IEnumerator` and `IEnumerator<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="6fe1a-1569">它會實作 `System.IDisposable`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1569">It implements `System.IDisposable`.</span></span>
*  <span data-ttu-id="6fe1a-1570">（如果有的話），它會初始化引數值的複本和執行個體的值傳遞至函式成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1570">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>
*  <span data-ttu-id="6fe1a-1571">它有四個可能的狀態：***之前***，***執行***，***暫止***，以及***之後***，和一開始處於***之前***狀態。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1571">It has four potential states, ***before***, ***running***, ***suspended***, and ***after***, and is initially in the ***before*** state.</span></span>

<span data-ttu-id="6fe1a-1572">列舉值物件通常是封裝在迭代器區塊中的程式碼，並實作列舉程式介面中，編譯器產生的列舉值類別的執行個體，但可能會有其他的實作方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1572">An enumerator object is typically an instance of a compiler-generated enumerator class that encapsulates the code in the iterator block and implements the enumerator interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="6fe1a-1573">如果列舉值類別由編譯器產生的該類別巢狀，直接或間接包含函式成員的類別中會有私用存取範圍，而它必須保留供編譯器使用的名稱 ([識別碼](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1573">If an enumerator class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="6fe1a-1574">列舉值物件可以實作比上述指定的多個介面。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1574">An enumerator object may implement more interfaces than those specified above.</span></span>

<span data-ttu-id="6fe1a-1575">下列各節描述的確切行為`MoveNext`， `Current`，並`Dispose`的成員`IEnumerable`和`IEnumerable<T>`介面列舉值物件所提供的實作。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1575">The following sections describe the exact behavior of the `MoveNext`, `Current`, and `Dispose` members of the `IEnumerable` and `IEnumerable<T>` interface implementations provided by an enumerator object.</span></span>

<span data-ttu-id="6fe1a-1576">請注意，不支援列舉值物件`IEnumerator.Reset`方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1576">Note that enumerator objects do not support the `IEnumerator.Reset` method.</span></span> <span data-ttu-id="6fe1a-1577">叫用這個方法會導致`System.NotSupportedException`擲回。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1577">Invoking this method causes a `System.NotSupportedException` to be thrown.</span></span>

#### <a name="the-movenext-method"></a><span data-ttu-id="6fe1a-1578">MoveNext 方法</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1578">The MoveNext method</span></span>

<span data-ttu-id="6fe1a-1579">`MoveNext`方法的列舉值物件封裝的迭代器區塊的程式碼。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1579">The `MoveNext` method of an enumerator object encapsulates the code of an iterator block.</span></span> <span data-ttu-id="6fe1a-1580">叫用`MoveNext`方法會執行程式碼中的迭代器區塊並將設定`Current`適當的列舉值物件的屬性。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1580">Invoking the `MoveNext` method executes code in the iterator block and sets the `Current` property of the enumerator object as appropriate.</span></span> <span data-ttu-id="6fe1a-1581">所執行的精確動作`MoveNext`取決於列舉值物件的狀態時`MoveNext`叫用：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1581">The precise action performed by `MoveNext` depends on the state of the enumerator object when `MoveNext` is invoked:</span></span>

*  <span data-ttu-id="6fe1a-1582">如果列舉值物件的狀態***之前***叫用、 `MoveNext`:</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1582">If the state of the enumerator object is ***before***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="6fe1a-1583">狀態變更為***執行***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1583">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="6fe1a-1584">初始化參數 (包括`this`) 的引數值和列舉值物件初始化時，所儲存的執行個體值的迭代器區塊。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1584">Initializes the parameters (including `this`) of the iterator block to the argument values and instance value saved when the enumerator object was initialized.</span></span>
   * <span data-ttu-id="6fe1a-1585">執行從開頭的迭代器區塊，直到執行將會中斷 （如下所述）。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1585">Executes the iterator block from the beginning until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="6fe1a-1586">如果列舉值物件的狀態***執行***，叫用的結果`MoveNext`未指定。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1586">If the state of the enumerator object is ***running***, the result of invoking `MoveNext` is unspecified.</span></span>
*  <span data-ttu-id="6fe1a-1587">如果列舉值物件的狀態***暫止***叫用、 `MoveNext`:</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1587">If the state of the enumerator object is ***suspended***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="6fe1a-1588">狀態變更為***執行***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1588">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="6fe1a-1589">迭代器區塊執行的最後暫停時，儲存的值來還原所有的本機變數和參數 （包括這） 的值。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1589">Restores the values of all local variables and parameters (including this) to the values saved when execution of the iterator block was last suspended.</span></span> <span data-ttu-id="6fe1a-1590">請注意，這些變數所參考的任何物件的內容可能已變更，因為前一個呼叫 MoveNext。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1590">Note that the contents of any objects referenced by these variables may have changed since the previous call to MoveNext.</span></span>
   * <span data-ttu-id="6fe1a-1591">繼續執行的正後方的迭代器區塊`yield return`陳述式，造成執行暫停並繼續進行，直到執行將會中斷 （如下所述）。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1591">Resumes execution of the iterator block immediately following the `yield return` statement that caused the suspension of execution and continues until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="6fe1a-1592">如果列舉值物件的狀態***之後***、 叫用`MoveNext`傳回`false`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1592">If the state of the enumerator object is ***after***, invoking `MoveNext` returns `false`.</span></span>


<span data-ttu-id="6fe1a-1593">當`MoveNext`執行迭代器區塊中，可以中斷執行，以四種方式：藉由`yield return`陳述式、 藉由`yield break`陳述式遇到區塊的結尾迭代器，以及例外狀況被擲回並傳播出迭代器區塊。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1593">When `MoveNext` executes the iterator block, execution can be interrupted in four ways: By a `yield return` statement, by a `yield break` statement, by encountering the end of the iterator block, and by an exception being thrown and propagated out of the iterator block.</span></span>

*  <span data-ttu-id="6fe1a-1594">當`yield return`遇到陳述式 ([yield 陳述式](statements.md#the-yield-statement)):</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1594">When a `yield return` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="6fe1a-1595">陳述式中指定的運算式會評估，以隱含方式轉換產生的型別，並指派給`Current`列舉值物件的屬性。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1595">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
   * <span data-ttu-id="6fe1a-1596">暫停執行的迭代器主體。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1596">Execution of the iterator body is suspended.</span></span> <span data-ttu-id="6fe1a-1597">所有本機變數和參數的值 (包括`this`) 會儲存，因為這個位置`yield return`陳述式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1597">The values of all local variables and parameters (including `this`) are saved, as is the location of this `yield return` statement.</span></span> <span data-ttu-id="6fe1a-1598">如果`yield return`陳述式位於一或多個`try`區塊中使用，相關聯`finally`區塊不會執行這一次。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1598">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
   * <span data-ttu-id="6fe1a-1599">列舉值物件的狀態會變為***暫止***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1599">The state of the enumerator object is changed to ***suspended***.</span></span>
   * <span data-ttu-id="6fe1a-1600">`MoveNext`方法會傳回`true`給其呼叫端，表示在反覆項目成功地前移至下一個值。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1600">The `MoveNext` method returns `true` to its caller, indicating that the iteration successfully advanced to the next value.</span></span>
*  <span data-ttu-id="6fe1a-1601">當`yield break`遇到陳述式 ([yield 陳述式](statements.md#the-yield-statement)):</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1601">When a `yield break` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="6fe1a-1602">如果`yield break`陳述式位於一或多個`try`區塊中使用，相關聯`finally`區塊會執行。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1602">If the `yield break` statement is within one or more `try` blocks, the associated `finally` blocks are executed.</span></span>
   * <span data-ttu-id="6fe1a-1603">列舉值物件的狀態會變為***之後***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1603">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="6fe1a-1604">`MoveNext`方法會傳回`false`給其呼叫端，表示在反覆項目已完成。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1604">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="6fe1a-1605">當遇到的迭代器主體結尾：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1605">When the end of the iterator body is encountered:</span></span>
   * <span data-ttu-id="6fe1a-1606">列舉值物件的狀態會變為***之後***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1606">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="6fe1a-1607">`MoveNext`方法會傳回`false`給其呼叫端，表示在反覆項目已完成。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1607">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="6fe1a-1608">當例外狀況會擲回，並傳播出迭代器區塊：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1608">When an exception is thrown and propagated out of the iterator block:</span></span>
   * <span data-ttu-id="6fe1a-1609">適當`finally`迭代器主體中的區塊將已執行的例外狀況傳播。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1609">Appropriate `finally` blocks in the iterator body will have been executed by the exception propagation.</span></span>
   * <span data-ttu-id="6fe1a-1610">列舉值物件的狀態會變為***之後***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1610">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="6fe1a-1611">例外狀況傳播的呼叫端會繼續`MoveNext`方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1611">The exception propagation continues to the caller of the `MoveNext` method.</span></span>

#### <a name="the-current-property"></a><span data-ttu-id="6fe1a-1612">目前的屬性</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1612">The Current property</span></span>

<span data-ttu-id="6fe1a-1613">列舉值物件的`Current`屬性會受到`yield return`迭代器區塊中的陳述式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1613">An enumerator object's `Current` property is affected by `yield return` statements in the iterator block.</span></span>

<span data-ttu-id="6fe1a-1614">當列舉值物件處於***暫止***狀態時，值`Current`是先前呼叫所設定的值`MoveNext`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1614">When an enumerator object is in the ***suspended*** state, the value of `Current` is the value set by the previous call to `MoveNext`.</span></span> <span data-ttu-id="6fe1a-1615">當列舉值物件處於***之前***，***執行***，或***之後***所述，存取的結果`Current`未指定。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1615">When an enumerator object is in the ***before***, ***running***, or ***after*** states, the result of accessing `Current` is unspecified.</span></span>

<span data-ttu-id="6fe1a-1616">迭代器與 yield 類型並非`object`，存取的結果`Current`透過列舉值物件的`IEnumerable`實作對應至存取`Current`透過列舉值物件的`IEnumerator<T>`實作，並將轉換結果`object`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1616">For an iterator with a yield type other than `object`, the result of accessing `Current` through the enumerator object's `IEnumerable` implementation corresponds to accessing `Current` through the enumerator object's `IEnumerator<T>` implementation and casting the result to `object`.</span></span>

#### <a name="the-dispose-method"></a><span data-ttu-id="6fe1a-1617">Dispose 方法</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1617">The Dispose method</span></span>

<span data-ttu-id="6fe1a-1618">`Dispose`方法用來清除反覆項目列舉值物件帶入***之後***狀態。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1618">The `Dispose` method is used to clean up the iteration by bringing the enumerator object to the ***after*** state.</span></span>

*  <span data-ttu-id="6fe1a-1619">如果列舉值物件的狀態***之前***、 叫用`Dispose`狀態變更為***之後***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1619">If the state of the enumerator object is ***before***, invoking `Dispose` changes the state to ***after***.</span></span>
*  <span data-ttu-id="6fe1a-1620">如果列舉值物件的狀態***執行***，叫用的結果`Dispose`未指定。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1620">If the state of the enumerator object is ***running***, the result of invoking `Dispose` is unspecified.</span></span>
*  <span data-ttu-id="6fe1a-1621">如果列舉值物件的狀態***暫止***叫用、 `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1621">If the state of the enumerator object is ***suspended***, invoking `Dispose`:</span></span>
   * <span data-ttu-id="6fe1a-1622">狀態變更為***執行***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1622">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="6fe1a-1623">執行任何 finally 區塊，如果上次執行的`yield return`陳述式已`yield break`陳述式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1623">Executes any finally blocks as if the last executed `yield return` statement were a `yield break` statement.</span></span> <span data-ttu-id="6fe1a-1624">如果這導致例外狀況擲回並傳播到迭代器主體之外，列舉值物件的狀態設為***之後***例外狀況會傳播給呼叫端`Dispose`方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1624">If this causes an exception to be thrown and propagated out of the iterator body, the state of the enumerator object is set to ***after*** and the exception is propagated to the caller of the `Dispose` method.</span></span>
   * <span data-ttu-id="6fe1a-1625">狀態變更為***之後***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1625">Changes the state to ***after***.</span></span>
*  <span data-ttu-id="6fe1a-1626">如果列舉值物件的狀態***之後***叫用、`Dispose`沒有任何作用。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1626">If the state of the enumerator object is ***after***, invoking `Dispose` has no affect.</span></span>

### <a name="enumerable-objects"></a><span data-ttu-id="6fe1a-1627">可列舉的物件</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1627">Enumerable objects</span></span>

<span data-ttu-id="6fe1a-1628">使用迭代器區塊來實作傳回可列舉的介面類型的函式成員時，叫用函式成員不會立即執行程式碼中的迭代器區塊。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1628">When a function member returning an enumerable interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="6fe1a-1629">相反地，***可列舉物件***會建立並傳回。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1629">Instead, an ***enumerable object*** is created and returned.</span></span> <span data-ttu-id="6fe1a-1630">可列舉的物件`GetEnumerator`方法會傳回列舉值物件，封裝的程式碼區塊中指定迭代器，而且執行中的迭代器區塊的程式碼，就會發生時 enumerator 物件的`MoveNext`叫用方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1630">The enumerable object's `GetEnumerator` method returns an enumerator object that encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="6fe1a-1631">可列舉的物件具有下列特性：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1631">An enumerable object has the following characteristics:</span></span>

*  <span data-ttu-id="6fe1a-1632">它會實作`IEnumerable`並`IEnumerable<T>`，其中`T`iterator 的 yield 類型。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1632">It implements `IEnumerable` and `IEnumerable<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="6fe1a-1633">（如果有的話），它會初始化引數值的複本和執行個體的值傳遞至函式成員。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1633">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>

<span data-ttu-id="6fe1a-1634">可列舉的物件通常是類別的執行個體編譯器產生可列舉的封裝中的迭代器區塊的程式碼，並實作可列舉的介面，但可能會有其他的實作方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1634">An enumerable object is typically an instance of a compiler-generated enumerable class that encapsulates the code in the iterator block and implements the enumerable interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="6fe1a-1635">如果由編譯器產生的可列舉的類別，該類別巢狀，直接或間接包含函式成員的類別中會有私用存取範圍，而且會有保留供編譯器使用的名稱 ([識別碼](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1635">If an enumerable class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="6fe1a-1636">可列舉物件可以實作比上述指定的多個介面。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1636">An enumerable object may implement more interfaces than those specified above.</span></span> <span data-ttu-id="6fe1a-1637">特別是，也會實作可列舉物件`IEnumerator`和`IEnumerator<T>`，讓它做為可列舉和列舉程式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1637">In particular, an enumerable object may also implement `IEnumerator` and `IEnumerator<T>`, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="6fe1a-1638">在這類的實作中，第一次可列舉物件的`GetEnumerator`叫用方法時，會傳回可列舉物件本身。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1638">In that type of implementation, the first time an enumerable object's `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="6fe1a-1639">後續的引動過程之可列舉物件`GetEnumerator`，如果任何項目，傳回可列舉物件的複本。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1639">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="6fe1a-1640">因此，每個傳回的列舉程式有自己的狀態，和一個列舉值的變更將不會影響另一個。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1640">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span>

#### <a name="the-getenumerator-method"></a><span data-ttu-id="6fe1a-1641">GetEnumerator 方法</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1641">The GetEnumerator method</span></span>

<span data-ttu-id="6fe1a-1642">可列舉物件會提供實作`GetEnumerator`方法`IEnumerable`和`IEnumerable<T>`介面。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1642">An enumerable object provides an implementation of the `GetEnumerator` methods of the `IEnumerable` and `IEnumerable<T>` interfaces.</span></span> <span data-ttu-id="6fe1a-1643">這兩個`GetEnumerator`方法共用通用的實作，取得並傳回可用的列舉值物件。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1643">The two `GetEnumerator` methods share a common implementation that acquires and returns an available enumerator object.</span></span> <span data-ttu-id="6fe1a-1644">列舉值物件會以引數的值初始化和執行個體的可列舉物件時初始化，否則儲存值的列舉值物件函式中所述[列舉值物件](classes.md#enumerator-objects)。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1644">The enumerator object is initialized with the argument values and instance value saved when the enumerable object was initialized, but otherwise the enumerator object functions as described in [Enumerator objects](classes.md#enumerator-objects).</span></span>

### <a name="implementation-example"></a><span data-ttu-id="6fe1a-1645">實作範例</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1645">Implementation example</span></span>

<span data-ttu-id="6fe1a-1646">本節說明可能的實作，根據標準的 C# 建構迭代器。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1646">This section describes a possible implementation of iterators in terms of standard C# constructs.</span></span> <span data-ttu-id="6fe1a-1647">此處所述的實作根據 Microsoft C# 編譯器，使用相同的原則，但它不是託管的實作或只有一個可能。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1647">The implementation described here is based on the same principles used by the Microsoft C# compiler, but it is by no means a mandated implementation or the only one possible.</span></span>

<span data-ttu-id="6fe1a-1648">下列`Stack<T>`類別會實作其`GetEnumerator`使用迭代器的方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1648">The following `Stack<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="6fe1a-1649">迭代器會列舉由上而下順序中的堆疊項的目。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1649">The iterator enumerates the elements of the stack in top to bottom order.</span></span>

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

<span data-ttu-id="6fe1a-1650">`GetEnumerator`方法可以轉譯為封裝的程式碼在迭代器區塊中，編譯器產生的列舉值類別的具現化，如下列所示。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1650">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

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

<span data-ttu-id="6fe1a-1651">在上述的轉譯，迭代器區塊中的程式碼會轉換成狀態機器，並放置於`MoveNext`列舉值類別的方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1651">In the preceding translation, the code in the iterator block is turned into a state machine and placed in the `MoveNext` method of the enumerator class.</span></span> <span data-ttu-id="6fe1a-1652">此外，區域變數`i`會轉換成列舉值物件中的欄位中，因此它可以繼續存在的引動過程之間`MoveNext`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1652">Furthermore, the local variable `i` is turned into a field in the enumerator object so it can continue to exist across invocations of `MoveNext`.</span></span>

<span data-ttu-id="6fe1a-1653">下列範例會列印整數 1 到 10 的簡單乘法表。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1653">The following example prints a simple multiplication table of the integers 1 through 10.</span></span> <span data-ttu-id="6fe1a-1654">`FromTo`在範例中的方法會傳回可列舉的物件，並使用迭代器實作。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1654">The `FromTo` method in the example returns an enumerable object and is implemented using an iterator.</span></span>

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

<span data-ttu-id="6fe1a-1655">`FromTo`方法可以轉譯為編譯器產生 enumerable 類別會封裝在迭代器區塊中，程式碼的具現化，如下列所示。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1655">The `FromTo` method can be translated into an instantiation of a compiler-generated enumerable class that encapsulates the code in the iterator block, as shown in the following.</span></span>

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

<span data-ttu-id="6fe1a-1656">Enumerable 類別會實作可列舉的介面和列舉程式介面，讓它做為可列舉和列舉程式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1656">The enumerable class implements both the enumerable interfaces and the enumerator interfaces, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="6fe1a-1657">第一次`GetEnumerator`叫用方法時，會傳回可列舉物件本身。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1657">The first time the `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="6fe1a-1658">後續的引動過程之可列舉物件`GetEnumerator`，如果任何項目，傳回可列舉物件的複本。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1658">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="6fe1a-1659">因此，每個傳回的列舉程式有自己的狀態，和一個列舉值的變更將不會影響另一個。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1659">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span> <span data-ttu-id="6fe1a-1660">`Interlocked.CompareExchange`方法用來確保安全執行緒作業。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1660">The `Interlocked.CompareExchange` method is used to ensure thread-safe operation.</span></span>

<span data-ttu-id="6fe1a-1661">`from`和`to`參數會轉換成 enumerable 類別中的欄位。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1661">The `from` and `to` parameters are turned into fields in the enumerable class.</span></span> <span data-ttu-id="6fe1a-1662">因為`from`修改在迭代器區塊中，額外`__from`引進欄位來保存初始值給`from`中每個列舉值。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1662">Because `from` is modified in the iterator block, an additional `__from` field is introduced to hold the initial value given to `from` in each enumerator.</span></span>

<span data-ttu-id="6fe1a-1663">`MoveNext`方法會擲回`InvalidOperationException`如果它時，會呼叫`__state`是`0`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1663">The `MoveNext` method throws an `InvalidOperationException` if it is called when `__state` is `0`.</span></span> <span data-ttu-id="6fe1a-1664">這樣可以防止作為列舉值物件，但是未先呼叫之可列舉物件`GetEnumerator`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1664">This protects against use of the enumerable object as an enumerator object without first calling `GetEnumerator`.</span></span>

<span data-ttu-id="6fe1a-1665">下列範例顯示簡單的樹狀目錄類別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1665">The following example shows a simple tree class.</span></span> <span data-ttu-id="6fe1a-1666">`Tree<T>`類別會實作其`GetEnumerator`使用迭代器的方法。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1666">The `Tree<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="6fe1a-1667">迭代器列舉順序中置中的樹狀結構的項目。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1667">The iterator enumerates the elements of the tree in infix order.</span></span>

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

<span data-ttu-id="6fe1a-1668">`GetEnumerator`方法可以轉譯為封裝的程式碼在迭代器區塊中，編譯器產生的列舉值類別的具現化，如下列所示。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1668">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

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

<span data-ttu-id="6fe1a-1669">中所使用的編譯器產生暫存`foreach`陳述式會將提取`__left`和`__right`列舉值物件的欄位。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1669">The compiler generated temporaries used in the `foreach` statements are lifted into the `__left` and `__right` fields of the enumerator object.</span></span> <span data-ttu-id="6fe1a-1670">`__state`仔細更新列舉值物件的欄位，讓正確`Dispose()`如果擲回例外狀況，則方法也會正確地呼叫。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1670">The `__state` field of the enumerator object is carefully updated so that the correct `Dispose()` method will be called correctly if an exception is thrown.</span></span> <span data-ttu-id="6fe1a-1671">請注意，不可以撰寫具有簡單轉譯的程式碼`foreach`陳述式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1671">Note that it is not possible to write the translated code with simple `foreach` statements.</span></span>

## <a name="async-functions"></a><span data-ttu-id="6fe1a-1672">非同步函式</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1672">Async functions</span></span>

<span data-ttu-id="6fe1a-1673">方法 ([方法](classes.md#methods)) 或匿名函式 ([匿名函式運算式](expressions.md#anonymous-function-expressions)) 與`async`修飾詞稱為***非同步函式***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1673">A method ([Methods](classes.md#methods)) or anonymous function ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) with the `async` modifier is called an ***async function***.</span></span> <span data-ttu-id="6fe1a-1674">一般情況下，詞彙***非同步***用來描述任何一種函式具有`async`修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1674">In general, the term ***async*** is used to describe any kind of function that has the `async` modifier.</span></span>

<span data-ttu-id="6fe1a-1675">它是編譯時期錯誤，如需非同步函式指定任何的型式參數清單`ref`或`out`參數。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1675">It is a compile-time error for the formal parameter list of an async function to specify any `ref` or `out` parameters.</span></span>

<span data-ttu-id="6fe1a-1676">*Return_type*非同步方法必須是其中一個`void`或是***工作類型***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1676">The *return_type* of an async method must be either `void` or a ***task type***.</span></span> <span data-ttu-id="6fe1a-1677">工作類型是`System.Threading.Tasks.Task`及來自建構型別的`System.Threading.Tasks.Task<T>`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1677">The task types are `System.Threading.Tasks.Task` and types constructed from `System.Threading.Tasks.Task<T>`.</span></span> <span data-ttu-id="6fe1a-1678">為了保持簡潔，在這一章中這些類型會當做`Task`和`Task<T>`分別。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1678">For the sake of brevity, in this chapter these types are referenced as `Task` and `Task<T>`, respectively.</span></span> <span data-ttu-id="6fe1a-1679">非同步方法傳回的工作類型是要傳回工作。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1679">An async method returning a task type is said to be task-returning.</span></span>

<span data-ttu-id="6fe1a-1680">工作類型的確切定義實作所定義，但是從語言的觀點來看工作型別為其中一個狀態不完整，成功或發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1680">The exact definition of the task types is implementation defined, but from the language's point of view a task type is in one of the states incomplete, succeeded or faulted.</span></span> <span data-ttu-id="6fe1a-1681">發生錯誤的工作記錄相關的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1681">A faulted task records a pertinent exception.</span></span> <span data-ttu-id="6fe1a-1682">成功`Task<T>`記錄類型之結果的`T`。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1682">A succeeded `Task<T>` records a result of type `T`.</span></span> <span data-ttu-id="6fe1a-1683">工作類型是即時資訊，並因此可能的運算元 await 運算式 ([Await 運算式](expressions.md#await-expressions))。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1683">Task types are awaitable, and can therefore be the operands of await expressions ([Await expressions](expressions.md#await-expressions)).</span></span>

<span data-ttu-id="6fe1a-1684">非同步函式引動過程都能夠暫止評估透過 await 運算式 ([Await 運算式](expressions.md#await-expressions)) 在其主體中。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1684">An async function invocation has the ability to suspend evaluation by means of await expressions ([Await expressions](expressions.md#await-expressions)) in its body.</span></span> <span data-ttu-id="6fe1a-1685">評估可能會在稍後繼續在暫止 await 運算式，藉由***繼續委派***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1685">Evaluation may later be resumed at the point of the suspending await expression by means of a ***resumption delegate***.</span></span> <span data-ttu-id="6fe1a-1686">繼續委派是型別`System.Action`，並叫用它時，非同步函式引動過程的評估會繼續從中斷處 await 運算式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1686">The resumption delegate is of type `System.Action`, and when it is invoked, evaluation of the async function invocation will resume from the await expression where it left off.</span></span> <span data-ttu-id="6fe1a-1687">***目前的呼叫端***非同步函式引動過程因原始呼叫端，如果永遠不會暫止的函式引動過程或繼續委派的最新呼叫端。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1687">The ***current caller*** of an async function invocation is the original caller if the function invocation has never been suspended, or the most recent caller of the resumption delegate otherwise.</span></span>

### <a name="evaluation-of-a-task-returning-async-function"></a><span data-ttu-id="6fe1a-1688">工作傳回非同步函式的評估</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1688">Evaluation of a task-returning async function</span></span>

<span data-ttu-id="6fe1a-1689">工作傳回非同步函式的引動過程會導致傳回的工作類型，產生的執行個體。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1689">Invocation of a task-returning async function causes an instance of the returned task type to be generated.</span></span> <span data-ttu-id="6fe1a-1690">這就叫做***傳回的工作***的非同步函式。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1690">This is called the ***return task*** of the async function.</span></span> <span data-ttu-id="6fe1a-1691">工作一開始處於不完整的狀態。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1691">The task is initially in an incomplete state.</span></span>

<span data-ttu-id="6fe1a-1692">非同步函式主體再評估直到它被擱置 （藉由到達 await 運算式） 或終止時，在這點控制回到呼叫端，以及傳回的工作。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1692">The async function body is then evaluated until it is either suspended (by reaching an await expression) or terminates, at which point control is returned to the caller, along with the return task.</span></span>

<span data-ttu-id="6fe1a-1693">當非同步函式主體結束時，傳回的工作移出時未完成的狀態：</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1693">When the body of the async function terminates, the return task is moved out of the incomplete state:</span></span>

*  <span data-ttu-id="6fe1a-1694">如果因達到的 return 陳述式或主體的結尾，終止函式主體，任何結果值會記錄在傳回的工作中，放入成功的狀態。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1694">If the function body terminates as the result of reaching a return statement or the end of the body, any result value is recorded in the return task, which is put into a succeeded state.</span></span>
*  <span data-ttu-id="6fe1a-1695">如果函式主體會終止無法攔截的例外狀況的結果 ([throw 陳述式](statements.md#the-throw-statement)) 例外狀況會記錄在傳回工作進入錯誤狀態。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1695">If the function body terminates as the result of an uncaught exception ([The throw statement](statements.md#the-throw-statement)) the exception is recorded in the return task which is put into a faulted state.</span></span>

### <a name="evaluation-of-a-void-returning-async-function"></a><span data-ttu-id="6fe1a-1696">傳回 void 的非同步函式的評估</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1696">Evaluation of a void-returning async function</span></span>

<span data-ttu-id="6fe1a-1697">如果非同步函式的傳回型別是`void`，評估不同於上述方式如下：完成及目前的執行緒例外狀況會不傳回任何工作，因為函式改用通訊***同步處理內容***。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1697">If the return type of the async function is `void`, evaluation differs from the above in the following way: Because no task is returned, the function instead communicates completion and exceptions to the current thread's ***synchronization context***.</span></span> <span data-ttu-id="6fe1a-1698">視實作而定，同步處理內容的確切定義，但代表目前的執行緒執行的"where"。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1698">The exact definition of synchronization context is implementation-dependent, but is a representation of "where" the current thread is running.</span></span> <span data-ttu-id="6fe1a-1699">傳回 void 的非同步函式的評估開始、 成功完成，或導致擲回未攔截到例外狀況時，會收到通知的同步處理內容。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1699">The synchronization context is notified when evaluation of a void-returning async function commences, completes successfully, or causes an uncaught exception to be thrown.</span></span>

<span data-ttu-id="6fe1a-1700">這可讓追蹤多少傳回 void 的非同步函式會在其下方執行的並決定如何傳播來自它們的例外狀況的內容。</span><span class="sxs-lookup"><span data-stu-id="6fe1a-1700">This allows the context to keep track of how many void-returning async functions are running under it, and to decide how to propagate exceptions coming out of them.</span></span>
