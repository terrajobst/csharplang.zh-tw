---
ms.openlocfilehash: 994b22f5375d57cfc4c7537c64345a27ddf3e416
ms.sourcegitcommit: 09e0ddec3bb6aa99b7340158bbac86a5a8243b43
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66193876"
---
# <a name="delegates"></a><span data-ttu-id="f9718-101">委派</span><span class="sxs-lookup"><span data-stu-id="f9718-101">Delegates</span></span>

<span data-ttu-id="f9718-102">委派可讓案例的其他語言，例如C++，依照 pascal 命名法和 Modula-解決與函式指標。</span><span class="sxs-lookup"><span data-stu-id="f9718-102">Delegates enable scenarios that other languages—such as C++, Pascal, and Modula -- have addressed with function pointers.</span></span> <span data-ttu-id="f9718-103">不同於C++函式指標，不過，委派完全是物件導向，以及不同的是C++成員函式委派指標封裝物件執行個體和方法。</span><span class="sxs-lookup"><span data-stu-id="f9718-103">Unlike C++ function pointers, however, delegates are fully object oriented, and unlike C++ pointers to member functions, delegates encapsulate both an object instance and a method.</span></span>

<span data-ttu-id="f9718-104">在 delegate 宣告中定義的類別，衍生自類別`System.Delegate`。</span><span class="sxs-lookup"><span data-stu-id="f9718-104">A delegate declaration defines a class that is derived from the class `System.Delegate`.</span></span> <span data-ttu-id="f9718-105">委派執行個體封裝引動過程清單中，這是一或多個方法，每一個都指可呼叫的實體的清單。</span><span class="sxs-lookup"><span data-stu-id="f9718-105">A delegate instance encapsulates an invocation list, which is a list of one or more methods, each of which is referred to as a callable entity.</span></span> <span data-ttu-id="f9718-106">執行個體方法，可呼叫的實體包含的執行個體與該執行個體上的方法。</span><span class="sxs-lookup"><span data-stu-id="f9718-106">For instance methods, a callable entity consists of an instance and a method on that instance.</span></span> <span data-ttu-id="f9718-107">如需靜態方法，可呼叫的實體所組成的一個方法。</span><span class="sxs-lookup"><span data-stu-id="f9718-107">For static methods, a callable entity consists of just a method.</span></span> <span data-ttu-id="f9718-108">叫用委派執行個體使用一組適合的引數會導致每個委派的可呼叫實體，若要使用一組指定的引數叫用。</span><span class="sxs-lookup"><span data-stu-id="f9718-108">Invoking a delegate instance with an appropriate set of arguments causes each of the delegate's callable entities to be invoked with the given set of arguments.</span></span>

<span data-ttu-id="f9718-109">有趣且實用的屬性，委派執行個體的是，它不知道或在意它會封裝; 方法的類別最重要的是，這些方法是相容 ([委派宣告](delegates.md#delegate-declarations)) 與委派的型別。</span><span class="sxs-lookup"><span data-stu-id="f9718-109">An interesting and useful property of a delegate instance is that it does not know or care about the classes of the methods it encapsulates; all that matters is that those methods be compatible ([Delegate declarations](delegates.md#delegate-declarations)) with the delegate's type.</span></span> <span data-ttu-id="f9718-110">這可讓委派非常適用於 「 匿名 」 的引動過程。</span><span class="sxs-lookup"><span data-stu-id="f9718-110">This makes delegates perfectly suited for "anonymous" invocation.</span></span>

## <a name="delegate-declarations"></a><span data-ttu-id="f9718-111">委派宣告</span><span class="sxs-lookup"><span data-stu-id="f9718-111">Delegate declarations</span></span>

<span data-ttu-id="f9718-112">A *delegate_declaration*是*type_declaration* ([型別宣告](namespaces.md#type-declarations)) 宣告新的委派類型。</span><span class="sxs-lookup"><span data-stu-id="f9718-112">A *delegate_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new delegate type.</span></span>

```antlr
delegate_declaration
    : attributes? delegate_modifier* 'delegate' return_type
      identifier variant_type_parameter_list?
      '(' formal_parameter_list? ')' type_parameter_constraints_clause* ';'
    ;

delegate_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | delegate_modifier_unsafe
    ;
```

<span data-ttu-id="f9718-113">它是相同的修飾詞在 delegate 宣告中出現多次的編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="f9718-113">It is a compile-time error for the same modifier to appear multiple times in a delegate declaration.</span></span>

<span data-ttu-id="f9718-114">`new`修飾詞僅允許委派宣告另一個類型內，在此情況下它會指定這類委派隱藏繼承的成員相同的名稱，如中所述[的新修飾詞](classes.md#the-new-modifier)。</span><span class="sxs-lookup"><span data-stu-id="f9718-114">The `new` modifier is only permitted on delegates declared within another type, in which case it specifies that such a delegate hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span>

<span data-ttu-id="f9718-115">`public`， `protected`， `internal`，和`private`修飾詞控制的委派型別存取範圍。</span><span class="sxs-lookup"><span data-stu-id="f9718-115">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the delegate type.</span></span> <span data-ttu-id="f9718-116">根據和委派宣告發生所在的內容，這些修飾詞的一些可能不允許 ([宣告存取範圍](basic-concepts.md#declared-accessibility))。</span><span class="sxs-lookup"><span data-stu-id="f9718-116">Depending on the context in which the delegate declaration occurs, some of these modifiers may not be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="f9718-117">委派的型別名稱是*識別碼*。</span><span class="sxs-lookup"><span data-stu-id="f9718-117">The delegate's type name is *identifier*.</span></span>

<span data-ttu-id="f9718-118">選擇性*formal_parameter_list*指定之參數的委派，以及*return_type*表示委派的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="f9718-118">The optional *formal_parameter_list* specifies the parameters of the delegate, and *return_type* indicates the return type of the delegate.</span></span>

<span data-ttu-id="f9718-119">選擇性*variant_type_parameter_list* ([Variant 類型參數清單](interfaces.md#variant-type-parameter-lists)) 指定的委派本身的類型參數。</span><span class="sxs-lookup"><span data-stu-id="f9718-119">The optional *variant_type_parameter_list* ([Variant type parameter lists](interfaces.md#variant-type-parameter-lists)) specifies the type parameters to the delegate itself.</span></span>

<span data-ttu-id="f9718-120">委派類型的傳回型別必須是`void`，或輸出安全 ([變異數安全](interfaces.md#variance-safety))。</span><span class="sxs-lookup"><span data-stu-id="f9718-120">The return type of a delegate type must be either `void`, or output-safe ([Variance safety](interfaces.md#variance-safety)).</span></span>

<span data-ttu-id="f9718-121">委派型別的所有型式參數類型必須是輸入安全。</span><span class="sxs-lookup"><span data-stu-id="f9718-121">All the formal parameter types of a delegate type must be input-safe.</span></span> <span data-ttu-id="f9718-122">此外，任何`out`或`ref`參數類型必須也是輸出安全。</span><span class="sxs-lookup"><span data-stu-id="f9718-122">Additionally, any `out` or `ref` parameter types must also be output-safe.</span></span> <span data-ttu-id="f9718-123">請注意，即使`out`參數必須是輸入安全，因為基礎執行平台的限制。</span><span class="sxs-lookup"><span data-stu-id="f9718-123">Note that even `out` parameters are required to be input-safe, due to a limitation of the underlying execution platform.</span></span>

<span data-ttu-id="f9718-124">C# 中的委派類型是名稱相等結構上相等。</span><span class="sxs-lookup"><span data-stu-id="f9718-124">Delegate types in C# are name equivalent, not structurally equivalent.</span></span> <span data-ttu-id="f9718-125">具體而言，具有相同的參數的兩種不同的委派類型列出，並傳回型別會被視為不同的委派類型。</span><span class="sxs-lookup"><span data-stu-id="f9718-125">Specifically, two different delegate types that have the same parameter lists and return type are considered different delegate types.</span></span> <span data-ttu-id="f9718-126">不過，兩個不同但結構上相等的委派型別的執行個體可能會比較為相等 ([委派等號比較運算子](expressions.md#delegate-equality-operators))。</span><span class="sxs-lookup"><span data-stu-id="f9718-126">However, instances of two distinct but structurally equivalent delegate types may compare as equal ([Delegate equality operators](expressions.md#delegate-equality-operators)).</span></span>

<span data-ttu-id="f9718-127">例如：</span><span class="sxs-lookup"><span data-stu-id="f9718-127">For example:</span></span>

```csharp
delegate int D1(int i, double d);

class A
{
    public static int M1(int a, double b) {...}
}

class B
{
    delegate int D2(int c, double d);
    public static int M1(int f, double g) {...}
    public static void M2(int k, double l) {...}
    public static int M3(int g) {...}
    public static void M4(int g) {...}
}
```

<span data-ttu-id="f9718-128">方法`A.M1`並`B.M1`這兩個委派類型是否相容`D1`和`D2`，因為它們有相同的傳回型別和參數的清單; 不過，這些委派型別是兩個不同的類型，因此它們不是可互換的。</span><span class="sxs-lookup"><span data-stu-id="f9718-128">The methods `A.M1` and `B.M1` are compatible with both the delegate types `D1` and `D2` , since they have the same return type and parameter list; however, these delegate types are two different types, so they are not interchangeable.</span></span> <span data-ttu-id="f9718-129">方法`B.M2`， `B.M3`，並`B.M4`與委派類型不相容`D1`和`D2`，因為它們有不同的傳回型別或參數清單。</span><span class="sxs-lookup"><span data-stu-id="f9718-129">The methods `B.M2`, `B.M3`, and `B.M4` are incompatible with the delegate types `D1` and `D2`, since they have different return types or parameter lists.</span></span>

<span data-ttu-id="f9718-130">其他泛型類型宣告，例如型別引數必須有建立建構的委派型別。</span><span class="sxs-lookup"><span data-stu-id="f9718-130">Like other generic type declarations, type arguments must be given to create a constructed delegate type.</span></span> <span data-ttu-id="f9718-131">參數型別和傳回型別建構的委派型別會建立在委派宣告中，每個類型參數的替代建構的委派類型的對應的型別引數。</span><span class="sxs-lookup"><span data-stu-id="f9718-131">The parameter types and return type of a constructed delegate type are created by substituting, for each type parameter in the delegate declaration, the corresponding type argument of the constructed delegate type.</span></span> <span data-ttu-id="f9718-132">產生的傳回型別和參數類型會用於決定哪些方法與建構的委派型別相容。</span><span class="sxs-lookup"><span data-stu-id="f9718-132">The resulting return type and parameter types are used in determining what methods are compatible with a constructed delegate type.</span></span> <span data-ttu-id="f9718-133">例如: </span><span class="sxs-lookup"><span data-stu-id="f9718-133">For example:</span></span>

```csharp
delegate bool Predicate<T>(T value);

class X
{
    static bool F(int i) {...}
    static bool G(string s) {...}
}
```

<span data-ttu-id="f9718-134">此方法`X.F`相容的委派型別`Predicate<int>`和方法`X.G`與委派型別相容`Predicate<string>`。</span><span class="sxs-lookup"><span data-stu-id="f9718-134">The method `X.F` is compatible with the delegate type `Predicate<int>` and the method `X.G` is compatible with the delegate type `Predicate<string>` .</span></span>

<span data-ttu-id="f9718-135">宣告委派類型的唯一方式是透過*delegate_declaration*。</span><span class="sxs-lookup"><span data-stu-id="f9718-135">The only way to declare a delegate type is via a *delegate_declaration*.</span></span> <span data-ttu-id="f9718-136">委派型別是衍生自的類別類型`System.Delegate`。</span><span class="sxs-lookup"><span data-stu-id="f9718-136">A delegate type is a class type that is derived from `System.Delegate`.</span></span> <span data-ttu-id="f9718-137">委派類型都是隱含`sealed`，因此不允許從委派型別衍生任何型別。</span><span class="sxs-lookup"><span data-stu-id="f9718-137">Delegate types are implicitly `sealed`, so it is not permissible to derive any type from a delegate type.</span></span> <span data-ttu-id="f9718-138">它也不是允許衍生類別的非委派類型，從`System.Delegate`。</span><span class="sxs-lookup"><span data-stu-id="f9718-138">It is also not permissible to derive a non-delegate class type from `System.Delegate`.</span></span> <span data-ttu-id="f9718-139">請注意，`System.Delegate`是委派類型本身不; 它是從中衍生所有的委派類型的類別類型。</span><span class="sxs-lookup"><span data-stu-id="f9718-139">Note that `System.Delegate` is not itself a delegate type; it is a class type from which all delegate types are derived.</span></span>

<span data-ttu-id="f9718-140">C# 提供特殊語法委派具現化和引動過程。</span><span class="sxs-lookup"><span data-stu-id="f9718-140">C# provides special syntax for delegate instantiation and invocation.</span></span> <span data-ttu-id="f9718-141">除了在具現化，可以套用至類別或類別執行個體的任何作業也可以套用至委派的類別或執行個體，分別。</span><span class="sxs-lookup"><span data-stu-id="f9718-141">Except for instantiation, any operation that can be applied to a class or class instance can also be applied to a delegate class or instance, respectively.</span></span> <span data-ttu-id="f9718-142">特別是，就可以存取的成員`System.Delegate`經由一般成員存取語法類型。</span><span class="sxs-lookup"><span data-stu-id="f9718-142">In particular, it is possible to access members of the `System.Delegate` type via the usual member access syntax.</span></span>

<span data-ttu-id="f9718-143">一組的委派執行個體所封裝的方法會呼叫的引動過程清單。</span><span class="sxs-lookup"><span data-stu-id="f9718-143">The set of methods encapsulated by a delegate instance is called an invocation list.</span></span> <span data-ttu-id="f9718-144">建立委派執行個體時 ([委派相容性](delegates.md#delegate-compatibility)) 從單一方法，它會封裝該方法中，且其引動過程清單包含只有一個項目。</span><span class="sxs-lookup"><span data-stu-id="f9718-144">When a delegate instance is created ([Delegate compatibility](delegates.md#delegate-compatibility)) from a single method, it encapsulates that method, and its invocation list contains only one entry.</span></span> <span data-ttu-id="f9718-145">不過，當合併兩個非 null 委派執行個體，其引動過程清單串連-左運算元，則右運算元-的順序，以形成新的引動過程清單，其中包含兩個或多個項目。</span><span class="sxs-lookup"><span data-stu-id="f9718-145">However, when two non-null delegate instances are combined, their invocation lists are concatenated -- in the order left operand then right operand -- to form a new invocation list, which contains two or more entries.</span></span>

<span data-ttu-id="f9718-146">委派結合使用二進位`+`([加法運算子](expressions.md#addition-operator)) 及`+=`運算子 ([複合指派](expressions.md#compound-assignment))。</span><span class="sxs-lookup"><span data-stu-id="f9718-146">Delegates are combined using the binary `+` ([Addition operator](expressions.md#addition-operator)) and `+=` operators ([Compound assignment](expressions.md#compound-assignment)).</span></span> <span data-ttu-id="f9718-147">委派可以移除委派，使用二進位檔的組合`-`([減法運算子](expressions.md#subtraction-operator)) 及`-=`運算子 ([複合指派](expressions.md#compound-assignment))。</span><span class="sxs-lookup"><span data-stu-id="f9718-147">A delegate can be removed from a combination of delegates, using the binary `-` ([Subtraction operator](expressions.md#subtraction-operator)) and `-=` operators ([Compound assignment](expressions.md#compound-assignment)).</span></span> <span data-ttu-id="f9718-148">委派可以進行相等比較 ([委派等號比較運算子](expressions.md#delegate-equality-operators))。</span><span class="sxs-lookup"><span data-stu-id="f9718-148">Delegates can be compared for equality ([Delegate equality operators](expressions.md#delegate-equality-operators)).</span></span>

<span data-ttu-id="f9718-149">下列範例示範數個委派具現化，其對應的引動過程清單：</span><span class="sxs-lookup"><span data-stu-id="f9718-149">The following example shows the instantiation of a number of delegates, and their corresponding invocation lists:</span></span>

```csharp
delegate void D(int x);

class C
{
    public static void M1(int i) {...}
    public static void M2(int i) {...}

}

class Test
{
    static void Main() {
        D cd1 = new D(C.M1);      // M1
        D cd2 = new D(C.M2);      // M2
        D cd3 = cd1 + cd2;        // M1 + M2
        D cd4 = cd3 + cd1;        // M1 + M2 + M1
        D cd5 = cd4 + cd3;        // M1 + M2 + M1 + M1 + M2
    }

}
```

<span data-ttu-id="f9718-150">當`cd1`和`cd2`會具現化，它們每個封裝一種方法。</span><span class="sxs-lookup"><span data-stu-id="f9718-150">When `cd1` and `cd2` are instantiated, they each encapsulate one method.</span></span> <span data-ttu-id="f9718-151">當`cd3`是具現化，它有兩個方法的引動過程清單`M1`和`M2`，依此順序。</span><span class="sxs-lookup"><span data-stu-id="f9718-151">When `cd3` is instantiated, it has an invocation list of two methods, `M1` and `M2`, in that order.</span></span> <span data-ttu-id="f9718-152">`cd4`引動過程清單包含`M1`， `M2`，和`M1`，依此順序。</span><span class="sxs-lookup"><span data-stu-id="f9718-152">`cd4`'s invocation list contains `M1`, `M2`, and `M1`, in that order.</span></span> <span data-ttu-id="f9718-153">最後，`cd5`的引動過程清單包含`M1`， `M2`， `M1`， `M1`，和`M2`，依此順序。</span><span class="sxs-lookup"><span data-stu-id="f9718-153">Finally, `cd5`'s invocation list contains `M1`, `M2`, `M1`, `M1`, and `M2`, in that order.</span></span> <span data-ttu-id="f9718-154">結合 （也會有一樣的移除） 委派的詳細範例，請參閱[委派引動過程](delegates.md#delegate-invocation)。</span><span class="sxs-lookup"><span data-stu-id="f9718-154">For more examples of combining (as well as removing) delegates, see [Delegate invocation](delegates.md#delegate-invocation).</span></span>

## <a name="delegate-compatibility"></a><span data-ttu-id="f9718-155">委派相容性</span><span class="sxs-lookup"><span data-stu-id="f9718-155">Delegate compatibility</span></span>

<span data-ttu-id="f9718-156">方法或委派`M`已***相容***委派`D`符合下列所有項目時：</span><span class="sxs-lookup"><span data-stu-id="f9718-156">A method or delegate `M` is ***compatible*** with a delegate type `D` if all of the following are true:</span></span>

*  <span data-ttu-id="f9718-157">`D` 並`M`有相同數目的參數，並在每個參數`D`具有相同`ref`或`out`中的對應參數的修飾詞`M`。</span><span class="sxs-lookup"><span data-stu-id="f9718-157">`D` and `M` have the same number of parameters, and each parameter in `D` has the same `ref` or `out` modifiers as the corresponding parameter in `M`.</span></span>
*  <span data-ttu-id="f9718-158">針對每個值參數 (含參數`ref`或`out`修飾詞)，身分識別轉換 ([識別轉換](conversions.md#identity-conversion)) 或隱含參考轉換 ([隱含參考轉換](conversions.md#implicit-reference-conversions))在參數型別`D`中與對應參數類型`M`。</span><span class="sxs-lookup"><span data-stu-id="f9718-158">For each value parameter (a parameter with no `ref` or `out` modifier), an identity conversion ([Identity conversion](conversions.md#identity-conversion)) or implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from the parameter type in `D` to the corresponding parameter type in `M`.</span></span>
*  <span data-ttu-id="f9718-159">每個`ref`或是`out`中的參數，參數類型`D`等同於在參數型別`M`。</span><span class="sxs-lookup"><span data-stu-id="f9718-159">For each `ref` or `out` parameter, the parameter type in `D` is the same as the parameter type in `M`.</span></span>
*  <span data-ttu-id="f9718-160">身分識別或隱含參考轉換存在的傳回型別`M`的傳回型別`D`。</span><span class="sxs-lookup"><span data-stu-id="f9718-160">An identity or implicit reference conversion exists from the return type of `M` to the return type of `D`.</span></span>

## <a name="delegate-instantiation"></a><span data-ttu-id="f9718-161">委派具現化</span><span class="sxs-lookup"><span data-stu-id="f9718-161">Delegate instantiation</span></span>

<span data-ttu-id="f9718-162">委派的執行個體由*delegate_creation_expression* ([委派建立運算式](expressions.md#delegate-creation-expressions)) 或轉換成委派類型。</span><span class="sxs-lookup"><span data-stu-id="f9718-162">An instance of a delegate is created by a *delegate_creation_expression* ([Delegate creation expressions](expressions.md#delegate-creation-expressions)) or a conversion to a delegate type.</span></span> <span data-ttu-id="f9718-163">新建立的委派執行個體則會參考為：</span><span class="sxs-lookup"><span data-stu-id="f9718-163">The newly created delegate instance then refers to either:</span></span>

*  <span data-ttu-id="f9718-164">中所參考的靜態方法*delegate_creation_expression*，或</span><span class="sxs-lookup"><span data-stu-id="f9718-164">The static method referenced in the *delegate_creation_expression*, or</span></span>
*  <span data-ttu-id="f9718-165">目標物件 (它不能`null`) 和執行個體中所參考方法*delegate_creation_expression*，或</span><span class="sxs-lookup"><span data-stu-id="f9718-165">The target object (which cannot be `null`) and instance method referenced in the *delegate_creation_expression*, or</span></span>
*  <span data-ttu-id="f9718-166">另一個委派。</span><span class="sxs-lookup"><span data-stu-id="f9718-166">Another delegate.</span></span>

<span data-ttu-id="f9718-167">例如: </span><span class="sxs-lookup"><span data-stu-id="f9718-167">For example:</span></span>

```csharp
delegate void D(int x);

class C
{
    public static void M1(int i) {...}
    public void M2(int i) {...}
}

class Test
{
    static void Main() { 
        D cd1 = new D(C.M1);        // static method
        C t = new C();
        D cd2 = new D(t.M2);        // instance method
        D cd3 = new D(cd2);        // another delegate
    }
}
```

<span data-ttu-id="f9718-168">一旦具現化，委派執行個體一律會參考相同的目標物件和方法。</span><span class="sxs-lookup"><span data-stu-id="f9718-168">Once instantiated, delegate instances always refer to the same target object and method.</span></span> <span data-ttu-id="f9718-169">請記住，當合併兩個委派，或從另一個，新的委派結果，與它自己的引動過程清單，移除的其中一個結合，或移除的委派引動過程清單維持不變。</span><span class="sxs-lookup"><span data-stu-id="f9718-169">Remember, when two delegates are combined, or one is removed from another, a new delegate results with its own invocation list; the invocation lists of the delegates combined or removed remain unchanged.</span></span>

## <a name="delegate-invocation"></a><span data-ttu-id="f9718-170">委派引動過程</span><span class="sxs-lookup"><span data-stu-id="f9718-170">Delegate invocation</span></span>

<span data-ttu-id="f9718-171">C# 提供特殊的語法叫用委派。</span><span class="sxs-lookup"><span data-stu-id="f9718-171">C# provides special syntax for invoking a delegate.</span></span> <span data-ttu-id="f9718-172">叫用的非 null 委派執行個體，其引動過程清單包含一個項目時，它會叫用一個具有相同的引數，它提供，並傳回相同的值參考的方法至方法。</span><span class="sxs-lookup"><span data-stu-id="f9718-172">When a non-null delegate instance whose invocation list contains one entry is invoked, it invokes the one method with the same arguments it was given, and returns the same value as the referred to method.</span></span> <span data-ttu-id="f9718-173">(請參閱[委派引動過程](expressions.md#delegate-invocations)的委派引動過程的詳細資訊。)如果這類委派引動過程中發生例外狀況，該例外狀況下，叫用之方法內未攔截到例外狀況 catch 子句的搜尋會繼續在方法呼叫的委派，視為已直接呼叫該方法委派的方法參考。</span><span class="sxs-lookup"><span data-stu-id="f9718-173">(See [Delegate invocations](expressions.md#delegate-invocations) for detailed information on delegate invocation.) If an exception occurs during the invocation of such a delegate, and that exception is not caught within the method that was invoked, the search for an exception catch clause continues in the method that called the delegate, as if that method had directly called the method to which that delegate referred.</span></span>

<span data-ttu-id="f9718-174">引動過程委派執行個體，其引動過程清單包含多個項目進行的每個方法引動過程清單中，依順序中以同步方式叫用。</span><span class="sxs-lookup"><span data-stu-id="f9718-174">Invocation of a delegate instance whose invocation list contains multiple entries proceeds by invoking each of the methods in the invocation list, synchronously, in order.</span></span> <span data-ttu-id="f9718-175">所謂的每個方法會傳遞引數，如未提供以委派執行個體的同一組。</span><span class="sxs-lookup"><span data-stu-id="f9718-175">Each method so called is passed the same set of arguments as was given to the delegate instance.</span></span> <span data-ttu-id="f9718-176">如果這類委派引動過程包含參考參數 ([參考參數](classes.md#reference-parameters))，每個方法引動過程會發生相同的變數的參考，但會變更該變數，方法引動過程清單中的一個方法才會顯示進一步的方法引動過程清單。</span><span class="sxs-lookup"><span data-stu-id="f9718-176">If such a delegate invocation includes reference parameters ([Reference parameters](classes.md#reference-parameters)), each method invocation will occur with a reference to the same variable; changes to that variable by one method in the invocation list will be visible to methods further down the invocation list.</span></span> <span data-ttu-id="f9718-177">如果委派引動過程包含輸出參數或傳回值，其最終的值會來自清單中的最後一個委派的引動過程。</span><span class="sxs-lookup"><span data-stu-id="f9718-177">If the delegate invocation includes output parameters or a return value, their final value will come from the invocation of the last delegate in the list.</span></span>

<span data-ttu-id="f9718-178">如果在處理這類委派的引動過程期間發生的例外狀況，並叫用之方法內未攔截到例外狀況，例外狀況 catch 子句的搜尋會繼續在方法呼叫的委派，並進一步向下的任何方法不會叫用引動過程清單。</span><span class="sxs-lookup"><span data-stu-id="f9718-178">If an exception occurs during processing of the invocation of such a delegate, and that exception is not caught within the method that was invoked, the search for an exception catch clause continues in the method that called the delegate, and any methods further down the invocation list are not invoked.</span></span>

<span data-ttu-id="f9718-179">嘗試叫用委派執行個體，其值是 null 類型的例外狀況會導致`System.NullReferenceException`。</span><span class="sxs-lookup"><span data-stu-id="f9718-179">Attempting to invoke a delegate instance whose value is null results in an exception of type `System.NullReferenceException`.</span></span>

<span data-ttu-id="f9718-180">下列範例示範如何具現化、 合併、 移除及叫用委派：</span><span class="sxs-lookup"><span data-stu-id="f9718-180">The following example shows how to instantiate, combine, remove, and invoke delegates:</span></span>

```csharp
using System;

delegate void D(int x);

class C
{
    public static void M1(int i) {
        Console.WriteLine("C.M1: " + i);
    }

    public static void M2(int i) {
        Console.WriteLine("C.M2: " + i);
    }

    public void M3(int i) {
        Console.WriteLine("C.M3: " + i);
    }
}

class Test
{
    static void Main() { 
        D cd1 = new D(C.M1);
        cd1(-1);                // call M1

        D cd2 = new D(C.M2);
        cd2(-2);                // call M2

        D cd3 = cd1 + cd2;
        cd3(10);                // call M1 then M2

        cd3 += cd1;
        cd3(20);                // call M1, M2, then M1

        C c = new C();
        D cd4 = new D(c.M3);
        cd3 += cd4;
        cd3(30);                // call M1, M2, M1, then M3

        cd3 -= cd1;             // remove last M1
        cd3(40);                // call M1, M2, then M3

        cd3 -= cd4;
        cd3(50);                // call M1 then M2

        cd3 -= cd2;
        cd3(60);                // call M1

        cd3 -= cd2;             // impossible removal is benign
        cd3(60);                // call M1

        cd3 -= cd1;             // invocation list is empty so cd3 is null

        cd3(70);                // System.NullReferenceException thrown

        cd3 -= cd1;             // impossible removal is benign
    }
}
```

<span data-ttu-id="f9718-181">陳述式中所示`cd3 += cd1;`，委派可以多次出現在引動過程清單中。</span><span class="sxs-lookup"><span data-stu-id="f9718-181">As shown in the statement `cd3 += cd1;`, a delegate can be present in an invocation list multiple times.</span></span> <span data-ttu-id="f9718-182">在此情況下，它會直接叫用一次每次出現。</span><span class="sxs-lookup"><span data-stu-id="f9718-182">In this case, it is simply invoked once per occurrence.</span></span> <span data-ttu-id="f9718-183">在這類引動過程清單，移除該委派時，引動過程清單中的最後一個相符項目是會實際移除。</span><span class="sxs-lookup"><span data-stu-id="f9718-183">In an invocation list such as this, when that delegate is removed, the last occurrence in the invocation list is the one actually removed.</span></span>

<span data-ttu-id="f9718-184">最後一個陳述式中，執行前立即`cd3 -= cd1;`，委派`cd3`指的是空白的引動過程清單。</span><span class="sxs-lookup"><span data-stu-id="f9718-184">Immediately prior to the execution of the final statement, `cd3 -= cd1;`, the delegate `cd3` refers to an empty invocation list.</span></span> <span data-ttu-id="f9718-185">嘗試從空的清單移除委派 （或非空白清單中移除不存在的委派） 不是錯誤。</span><span class="sxs-lookup"><span data-stu-id="f9718-185">Attempting to remove a delegate from an empty list (or to remove a non-existent delegate from a non-empty list) is not an error.</span></span>

<span data-ttu-id="f9718-186">產生的輸出為：</span><span class="sxs-lookup"><span data-stu-id="f9718-186">The output produced is:</span></span>

```
C.M1: -1
C.M2: -2
C.M1: 10
C.M2: 10
C.M1: 20
C.M2: 20
C.M1: 20
C.M1: 30
C.M2: 30
C.M1: 30
C.M3: 30
C.M1: 40
C.M2: 40
C.M3: 40
C.M1: 50
C.M2: 50
C.M1: 60
C.M1: 60
```
