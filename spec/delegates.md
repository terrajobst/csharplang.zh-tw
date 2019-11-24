---
ms.openlocfilehash: d162d4b6a489032dcdfca9094a39d88fd03d4013
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704091"
---
# <a name="delegates"></a><span data-ttu-id="e33dd-101">委派</span><span class="sxs-lookup"><span data-stu-id="e33dd-101">Delegates</span></span>

<span data-ttu-id="e33dd-102">委派可讓其他語言（例如C++，Pascal 和 Modula）的案例能夠使用函式指標來解決。</span><span class="sxs-lookup"><span data-stu-id="e33dd-102">Delegates enable scenarios that other languages—such as C++, Pascal, and Modula -- have addressed with function pointers.</span></span> <span data-ttu-id="e33dd-103">不過C++ ，不同于函式指標，委派是完全面向物件， C++而且與成員函式的指標不同，委派會封裝物件實例和方法。</span><span class="sxs-lookup"><span data-stu-id="e33dd-103">Unlike C++ function pointers, however, delegates are fully object oriented, and unlike C++ pointers to member functions, delegates encapsulate both an object instance and a method.</span></span>

<span data-ttu-id="e33dd-104">委派宣告會定義衍生自類別的類別 `System.Delegate`。</span><span class="sxs-lookup"><span data-stu-id="e33dd-104">A delegate declaration defines a class that is derived from the class `System.Delegate`.</span></span> <span data-ttu-id="e33dd-105">委派實例會封裝一個調用清單，這是一或多個方法的清單，每個方法都稱為可呼叫的實體。</span><span class="sxs-lookup"><span data-stu-id="e33dd-105">A delegate instance encapsulates an invocation list, which is a list of one or more methods, each of which is referred to as a callable entity.</span></span> <span data-ttu-id="e33dd-106">若是實例方法，可呼叫的實體是由實例和該實例上的方法所組成。</span><span class="sxs-lookup"><span data-stu-id="e33dd-106">For instance methods, a callable entity consists of an instance and a method on that instance.</span></span> <span data-ttu-id="e33dd-107">若為靜態方法，可呼叫的實體只會包含方法。</span><span class="sxs-lookup"><span data-stu-id="e33dd-107">For static methods, a callable entity consists of just a method.</span></span> <span data-ttu-id="e33dd-108">使用一組適當的引數來叫用委派實例，會使每一個委派的可呼叫實體以一組指定的引數叫用。</span><span class="sxs-lookup"><span data-stu-id="e33dd-108">Invoking a delegate instance with an appropriate set of arguments causes each of the delegate's callable entities to be invoked with the given set of arguments.</span></span>

<span data-ttu-id="e33dd-109">委派實例有一個有趣且實用的屬性，就是它不知道或在意它所封裝之方法的類別;重點在於這些方法與委派的型別相容（[委派](delegates.md#delegate-declarations)宣告）。</span><span class="sxs-lookup"><span data-stu-id="e33dd-109">An interesting and useful property of a delegate instance is that it does not know or care about the classes of the methods it encapsulates; all that matters is that those methods be compatible ([Delegate declarations](delegates.md#delegate-declarations)) with the delegate's type.</span></span> <span data-ttu-id="e33dd-110">這使得委派非常適合「匿名」調用。</span><span class="sxs-lookup"><span data-stu-id="e33dd-110">This makes delegates perfectly suited for "anonymous" invocation.</span></span>

## <a name="delegate-declarations"></a><span data-ttu-id="e33dd-111">委派宣告</span><span class="sxs-lookup"><span data-stu-id="e33dd-111">Delegate declarations</span></span>

<span data-ttu-id="e33dd-112">*Delegate_declaration*是宣告新委派類型的*type_declaration* （[類型](namespaces.md#type-declarations)宣告）。</span><span class="sxs-lookup"><span data-stu-id="e33dd-112">A *delegate_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new delegate type.</span></span>

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

<span data-ttu-id="e33dd-113">在委派宣告中多次出現相同的修飾詞時，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="e33dd-113">It is a compile-time error for the same modifier to appear multiple times in a delegate declaration.</span></span>

<span data-ttu-id="e33dd-114">`new` 修飾詞只允許用於在另一個類型中宣告的委派，在此情況下，它會指定這類委派會隱藏具有相同名稱的繼承成員，如[新修飾](classes.md#the-new-modifier)詞中所述。</span><span class="sxs-lookup"><span data-stu-id="e33dd-114">The `new` modifier is only permitted on delegates declared within another type, in which case it specifies that such a delegate hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span>

<span data-ttu-id="e33dd-115">`public`、`protected`、`internal`和 `private` 修飾詞會控制委派類型的存取範圍。</span><span class="sxs-lookup"><span data-stu-id="e33dd-115">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the delegate type.</span></span> <span data-ttu-id="e33dd-116">根據發生委派宣告的內容，可能不允許其中一些修飾詞（宣告的[存取](basic-concepts.md#declared-accessibility)範圍）。</span><span class="sxs-lookup"><span data-stu-id="e33dd-116">Depending on the context in which the delegate declaration occurs, some of these modifiers may not be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="e33dd-117">委派的類型名稱為*identifier*。</span><span class="sxs-lookup"><span data-stu-id="e33dd-117">The delegate's type name is *identifier*.</span></span>

<span data-ttu-id="e33dd-118">選擇性的*formal_parameter_list*會指定委派的參數，而*return_type*則表示委派的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="e33dd-118">The optional *formal_parameter_list* specifies the parameters of the delegate, and *return_type* indicates the return type of the delegate.</span></span>

<span data-ttu-id="e33dd-119">選擇性的*variant_type_parameter_list* （[variant 型別參數清單](interfaces.md#variant-type-parameter-lists)）指定委派本身的型別參數。</span><span class="sxs-lookup"><span data-stu-id="e33dd-119">The optional *variant_type_parameter_list* ([Variant type parameter lists](interfaces.md#variant-type-parameter-lists)) specifies the type parameters to the delegate itself.</span></span>

<span data-ttu-id="e33dd-120">委派類型的傳回類型必須是 `void`或輸出安全（變異數[安全性](interfaces.md#variance-safety)）。</span><span class="sxs-lookup"><span data-stu-id="e33dd-120">The return type of a delegate type must be either `void`, or output-safe ([Variance safety](interfaces.md#variance-safety)).</span></span>

<span data-ttu-id="e33dd-121">委派類型的所有正式參數類型都必須是輸入安全的。</span><span class="sxs-lookup"><span data-stu-id="e33dd-121">All the formal parameter types of a delegate type must be input-safe.</span></span> <span data-ttu-id="e33dd-122">此外，任何 `out` 或 `ref` 參數類型也必須為輸出安全。</span><span class="sxs-lookup"><span data-stu-id="e33dd-122">Additionally, any `out` or `ref` parameter types must also be output-safe.</span></span> <span data-ttu-id="e33dd-123">請注意，即使 `out` 參數是輸入安全的，因為基礎執行平臺的限制。</span><span class="sxs-lookup"><span data-stu-id="e33dd-123">Note that even `out` parameters are required to be input-safe, due to a limitation of the underlying execution platform.</span></span>

<span data-ttu-id="e33dd-124">中C#的委派類型為對等的名稱，不是結構上的對等用法。</span><span class="sxs-lookup"><span data-stu-id="e33dd-124">Delegate types in C# are name equivalent, not structurally equivalent.</span></span> <span data-ttu-id="e33dd-125">具體而言，有兩個不同的委派類型具有相同的參數清單和傳回類型，會被視為不同的委派類型。</span><span class="sxs-lookup"><span data-stu-id="e33dd-125">Specifically, two different delegate types that have the same parameter lists and return type are considered different delegate types.</span></span> <span data-ttu-id="e33dd-126">不過，兩個不同但結構相等之委派類型的實例，可能會比較為相等（[委派相等運算子](expressions.md#delegate-equality-operators)）。</span><span class="sxs-lookup"><span data-stu-id="e33dd-126">However, instances of two distinct but structurally equivalent delegate types may compare as equal ([Delegate equality operators](expressions.md#delegate-equality-operators)).</span></span>

<span data-ttu-id="e33dd-127">例如：</span><span class="sxs-lookup"><span data-stu-id="e33dd-127">For example:</span></span>

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

<span data-ttu-id="e33dd-128">`A.M1` 和 `B.M1` 的方法與委派類型 `D1` 和 `D2` 相容，因為它們具有相同的傳回類型和參數清單;不過，這些委派類型是兩個不同的類型，因此無法互換。</span><span class="sxs-lookup"><span data-stu-id="e33dd-128">The methods `A.M1` and `B.M1` are compatible with both the delegate types `D1` and `D2` , since they have the same return type and parameter list; however, these delegate types are two different types, so they are not interchangeable.</span></span> <span data-ttu-id="e33dd-129">`B.M2`、`B.M3`和 `B.M4` 的方法與 `D1` 和 `D2`的委派類型不相容，因為它們有不同的傳回類型或參數清單。</span><span class="sxs-lookup"><span data-stu-id="e33dd-129">The methods `B.M2`, `B.M3`, and `B.M4` are incompatible with the delegate types `D1` and `D2`, since they have different return types or parameter lists.</span></span>

<span data-ttu-id="e33dd-130">如同其他泛型型別宣告，必須提供類型引數，才能建立結構化委派類型。</span><span class="sxs-lookup"><span data-stu-id="e33dd-130">Like other generic type declarations, type arguments must be given to create a constructed delegate type.</span></span> <span data-ttu-id="e33dd-131">建立結構化委派類型的參數類型和傳回型別，是藉由以委派宣告中的每個型別參數來取代，這是結構化委派型別的對應型別引數。</span><span class="sxs-lookup"><span data-stu-id="e33dd-131">The parameter types and return type of a constructed delegate type are created by substituting, for each type parameter in the delegate declaration, the corresponding type argument of the constructed delegate type.</span></span> <span data-ttu-id="e33dd-132">產生的傳回型別和參數類型是用來判斷哪些方法與結構化的委派型別相容。</span><span class="sxs-lookup"><span data-stu-id="e33dd-132">The resulting return type and parameter types are used in determining what methods are compatible with a constructed delegate type.</span></span> <span data-ttu-id="e33dd-133">例如：</span><span class="sxs-lookup"><span data-stu-id="e33dd-133">For example:</span></span>

```csharp
delegate bool Predicate<T>(T value);

class X
{
    static bool F(int i) {...}
    static bool G(string s) {...}
}
```

<span data-ttu-id="e33dd-134">`X.F` 的方法與委派類型 `Predicate<int>` 相容，而且方法 `X.G` 與 `Predicate<string>` 的委派類型相容。</span><span class="sxs-lookup"><span data-stu-id="e33dd-134">The method `X.F` is compatible with the delegate type `Predicate<int>` and the method `X.G` is compatible with the delegate type `Predicate<string>` .</span></span>

<span data-ttu-id="e33dd-135">宣告委派類型的唯一方式是透過*delegate_declaration*。</span><span class="sxs-lookup"><span data-stu-id="e33dd-135">The only way to declare a delegate type is via a *delegate_declaration*.</span></span> <span data-ttu-id="e33dd-136">委派類型是衍生自 `System.Delegate`的類別類型。</span><span class="sxs-lookup"><span data-stu-id="e33dd-136">A delegate type is a class type that is derived from `System.Delegate`.</span></span> <span data-ttu-id="e33dd-137">委派類型會隱含 `sealed`，因此不允許從委派類型衍生任何類型。</span><span class="sxs-lookup"><span data-stu-id="e33dd-137">Delegate types are implicitly `sealed`, so it is not permissible to derive any type from a delegate type.</span></span> <span data-ttu-id="e33dd-138">也不允許從 `System.Delegate`衍生非委派類別類型。</span><span class="sxs-lookup"><span data-stu-id="e33dd-138">It is also not permissible to derive a non-delegate class type from `System.Delegate`.</span></span> <span data-ttu-id="e33dd-139">請注意，`System.Delegate` 本身並不是委派類型;它是衍生所有委派類型的類別類型。</span><span class="sxs-lookup"><span data-stu-id="e33dd-139">Note that `System.Delegate` is not itself a delegate type; it is a class type from which all delegate types are derived.</span></span>

<span data-ttu-id="e33dd-140">C#提供委派具現化和調用的特殊語法。</span><span class="sxs-lookup"><span data-stu-id="e33dd-140">C# provides special syntax for delegate instantiation and invocation.</span></span> <span data-ttu-id="e33dd-141">除了具現化以外，可以套用至類別或類別實例的任何作業，也可以分別套用至委派類別或實例。</span><span class="sxs-lookup"><span data-stu-id="e33dd-141">Except for instantiation, any operation that can be applied to a class or class instance can also be applied to a delegate class or instance, respectively.</span></span> <span data-ttu-id="e33dd-142">特別的是，您可以透過一般的成員存取語法來存取 `System.Delegate` 類型的成員。</span><span class="sxs-lookup"><span data-stu-id="e33dd-142">In particular, it is possible to access members of the `System.Delegate` type via the usual member access syntax.</span></span>

<span data-ttu-id="e33dd-143">委派實例所封裝的方法集合稱為「調用清單」。</span><span class="sxs-lookup"><span data-stu-id="e33dd-143">The set of methods encapsulated by a delegate instance is called an invocation list.</span></span> <span data-ttu-id="e33dd-144">從單一方法建立委派實例（[委派相容性](delegates.md#delegate-compatibility)）時，它會封裝該方法，而其調用清單只會包含一個專案。</span><span class="sxs-lookup"><span data-stu-id="e33dd-144">When a delegate instance is created ([Delegate compatibility](delegates.md#delegate-compatibility)) from a single method, it encapsulates that method, and its invocation list contains only one entry.</span></span> <span data-ttu-id="e33dd-145">不過，當結合兩個非 null 委派實例時，會串連其調用清單--在 order left 運算元 then 右運算元--中，以形成新的調用清單，其中包含兩個或多個專案。</span><span class="sxs-lookup"><span data-stu-id="e33dd-145">However, when two non-null delegate instances are combined, their invocation lists are concatenated -- in the order left operand then right operand -- to form a new invocation list, which contains two or more entries.</span></span>

<span data-ttu-id="e33dd-146">委派會使用 binary `+` （[加號](expressions.md#addition-operator)）和 `+=` 運算子（[複合指派](expressions.md#compound-assignment)）結合。</span><span class="sxs-lookup"><span data-stu-id="e33dd-146">Delegates are combined using the binary `+` ([Addition operator](expressions.md#addition-operator)) and `+=` operators ([Compound assignment](expressions.md#compound-assignment)).</span></span> <span data-ttu-id="e33dd-147">您可以使用 binary `-` （[減法運算子](expressions.md#subtraction-operator)）和 `-=` 運算子（[複合指派](expressions.md#compound-assignment)），從委派的組合中移除委派。</span><span class="sxs-lookup"><span data-stu-id="e33dd-147">A delegate can be removed from a combination of delegates, using the binary `-` ([Subtraction operator](expressions.md#subtraction-operator)) and `-=` operators ([Compound assignment](expressions.md#compound-assignment)).</span></span> <span data-ttu-id="e33dd-148">委派可以比較是否相等（[委派相等運算子](expressions.md#delegate-equality-operators)）。</span><span class="sxs-lookup"><span data-stu-id="e33dd-148">Delegates can be compared for equality ([Delegate equality operators](expressions.md#delegate-equality-operators)).</span></span>

<span data-ttu-id="e33dd-149">下列範例顯示一些委派的具現化，以及其對應的調用清單：</span><span class="sxs-lookup"><span data-stu-id="e33dd-149">The following example shows the instantiation of a number of delegates, and their corresponding invocation lists:</span></span>

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

<span data-ttu-id="e33dd-150">當 `cd1` 和 `cd2` 具現化時，它們會分別封裝一種方法。</span><span class="sxs-lookup"><span data-stu-id="e33dd-150">When `cd1` and `cd2` are instantiated, they each encapsulate one method.</span></span> <span data-ttu-id="e33dd-151">當 `cd3` 具現化時，它會有兩個方法的調用清單，`M1` 和 `M2`（依該順序）。</span><span class="sxs-lookup"><span data-stu-id="e33dd-151">When `cd3` is instantiated, it has an invocation list of two methods, `M1` and `M2`, in that order.</span></span> <span data-ttu-id="e33dd-152">`cd4`的調用清單包含以該順序 `M1`、`M2`和 `M1`。</span><span class="sxs-lookup"><span data-stu-id="e33dd-152">`cd4`'s invocation list contains `M1`, `M2`, and `M1`, in that order.</span></span> <span data-ttu-id="e33dd-153">最後，`cd5`的調用清單包含以該順序 `M1`、`M2`、`M1`、`M1`和 `M2`。</span><span class="sxs-lookup"><span data-stu-id="e33dd-153">Finally, `cd5`'s invocation list contains `M1`, `M2`, `M1`, `M1`, and `M2`, in that order.</span></span> <span data-ttu-id="e33dd-154">如需結合（以及移除）委派的更多範例，請參閱[委派調用](delegates.md#delegate-invocation)。</span><span class="sxs-lookup"><span data-stu-id="e33dd-154">For more examples of combining (as well as removing) delegates, see [Delegate invocation](delegates.md#delegate-invocation).</span></span>

## <a name="delegate-compatibility"></a><span data-ttu-id="e33dd-155">委派相容性</span><span class="sxs-lookup"><span data-stu-id="e33dd-155">Delegate compatibility</span></span>

<span data-ttu-id="e33dd-156">如果下列所有條件都成立，則方法或委派 `M` 與委派類型 `D`***相容***：</span><span class="sxs-lookup"><span data-stu-id="e33dd-156">A method or delegate `M` is ***compatible*** with a delegate type `D` if all of the following are true:</span></span>

*  <span data-ttu-id="e33dd-157">`D` 和 `M` 的參數數目相同，而且 `D` 中的每個參數都與 `out` 中的對應參數具有相同的 `ref` 或 `M`修飾詞。</span><span class="sxs-lookup"><span data-stu-id="e33dd-157">`D` and `M` have the same number of parameters, and each parameter in `D` has the same `ref` or `out` modifiers as the corresponding parameter in `M`.</span></span>
*  <span data-ttu-id="e33dd-158">針對每個值參數（不含 `ref` 或 `out` 修飾詞的參數），會從 `D` 中的參數類型，將識別轉換（[識別轉換](conversions.md#identity-conversion)）或隱含參考轉換（[隱含參考](conversions.md#implicit-reference-conversions)轉換）存在於 `M`中的對應參數類型。</span><span class="sxs-lookup"><span data-stu-id="e33dd-158">For each value parameter (a parameter with no `ref` or `out` modifier), an identity conversion ([Identity conversion](conversions.md#identity-conversion)) or implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from the parameter type in `D` to the corresponding parameter type in `M`.</span></span>
*  <span data-ttu-id="e33dd-159">針對每個 `ref` 或 `out` 參數，`D` 中的參數類型與 `M`中的參數類型相同。</span><span class="sxs-lookup"><span data-stu-id="e33dd-159">For each `ref` or `out` parameter, the parameter type in `D` is the same as the parameter type in `M`.</span></span>
*  <span data-ttu-id="e33dd-160">識別或隱含參考轉換存在於 `M` 的傳回型別中，`D`的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="e33dd-160">An identity or implicit reference conversion exists from the return type of `M` to the return type of `D`.</span></span>

## <a name="delegate-instantiation"></a><span data-ttu-id="e33dd-161">委派具現化</span><span class="sxs-lookup"><span data-stu-id="e33dd-161">Delegate instantiation</span></span>

<span data-ttu-id="e33dd-162">委派的實例是由*delegate_creation_expression* （[委派建立運算式](expressions.md#delegate-creation-expressions)）或委派類型的轉換所建立。</span><span class="sxs-lookup"><span data-stu-id="e33dd-162">An instance of a delegate is created by a *delegate_creation_expression* ([Delegate creation expressions](expressions.md#delegate-creation-expressions)) or a conversion to a delegate type.</span></span> <span data-ttu-id="e33dd-163">新建立的委派實例則是指：</span><span class="sxs-lookup"><span data-stu-id="e33dd-163">The newly created delegate instance then refers to either:</span></span>

*  <span data-ttu-id="e33dd-164">*Delegate_creation_expression*中所參考的靜態方法，或</span><span class="sxs-lookup"><span data-stu-id="e33dd-164">The static method referenced in the *delegate_creation_expression*, or</span></span>
*  <span data-ttu-id="e33dd-165">在*delegate_creation_expression*中參考的目標物件（不能是 `null`）和實例方法，或</span><span class="sxs-lookup"><span data-stu-id="e33dd-165">The target object (which cannot be `null`) and instance method referenced in the *delegate_creation_expression*, or</span></span>
*  <span data-ttu-id="e33dd-166">另一個委派。</span><span class="sxs-lookup"><span data-stu-id="e33dd-166">Another delegate.</span></span>

<span data-ttu-id="e33dd-167">例如：</span><span class="sxs-lookup"><span data-stu-id="e33dd-167">For example:</span></span>

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

<span data-ttu-id="e33dd-168">一旦具現化之後，委派實例一律會參考相同的目標物件和方法。</span><span class="sxs-lookup"><span data-stu-id="e33dd-168">Once instantiated, delegate instances always refer to the same target object and method.</span></span> <span data-ttu-id="e33dd-169">請記住，當兩個委派合併，或其中一個被移除時，新的委派結果會有自己的調用清單;結合或移除委派的調用清單會保持不變。</span><span class="sxs-lookup"><span data-stu-id="e33dd-169">Remember, when two delegates are combined, or one is removed from another, a new delegate results with its own invocation list; the invocation lists of the delegates combined or removed remain unchanged.</span></span>

## <a name="delegate-invocation"></a><span data-ttu-id="e33dd-170">委派調用</span><span class="sxs-lookup"><span data-stu-id="e33dd-170">Delegate invocation</span></span>

<span data-ttu-id="e33dd-171">C#提供用來叫用委派的特殊語法。</span><span class="sxs-lookup"><span data-stu-id="e33dd-171">C# provides special syntax for invoking a delegate.</span></span> <span data-ttu-id="e33dd-172">當叫用清單中包含一個專案的非 null 委派實例被叫用時，它會使用指定的相同引數叫用一個方法，並傳回與所參考方法相同的值。</span><span class="sxs-lookup"><span data-stu-id="e33dd-172">When a non-null delegate instance whose invocation list contains one entry is invoked, it invokes the one method with the same arguments it was given, and returns the same value as the referred to method.</span></span> <span data-ttu-id="e33dd-173">（如需委派調用的詳細資訊，請參閱[委派調用](expressions.md#delegate-invocations)）。如果在這類委派的調用期間發生例外狀況，而且該例外狀況不會在叫用的方法內攔截，則搜尋例外狀況 catch 子句會繼續在呼叫委派的方法中，如同該方法直接呼叫該委派所參考的方法一樣。</span><span class="sxs-lookup"><span data-stu-id="e33dd-173">(See [Delegate invocations](expressions.md#delegate-invocations) for detailed information on delegate invocation.) If an exception occurs during the invocation of such a delegate, and that exception is not caught within the method that was invoked, the search for an exception catch clause continues in the method that called the delegate, as if that method had directly called the method to which that delegate referred.</span></span>

<span data-ttu-id="e33dd-174">叫用其調用清單包含多個專案的委派實例，會依照順序以同步方式叫用調用清單中的每個方法繼續進行。</span><span class="sxs-lookup"><span data-stu-id="e33dd-174">Invocation of a delegate instance whose invocation list contains multiple entries proceeds by invoking each of the methods in the invocation list, synchronously, in order.</span></span> <span data-ttu-id="e33dd-175">呼叫的每個方法都會傳遞與指定給委派實例相同的引數集。</span><span class="sxs-lookup"><span data-stu-id="e33dd-175">Each method so called is passed the same set of arguments as was given to the delegate instance.</span></span> <span data-ttu-id="e33dd-176">如果這類委派調用包含參考參數（[參考參數](classes.md#reference-parameters)），則會發生具有相同變數參考的每個方法調用;此變數在調用清單中的方法變更，將會在調用清單中進一步向下顯示。</span><span class="sxs-lookup"><span data-stu-id="e33dd-176">If such a delegate invocation includes reference parameters ([Reference parameters](classes.md#reference-parameters)), each method invocation will occur with a reference to the same variable; changes to that variable by one method in the invocation list will be visible to methods further down the invocation list.</span></span> <span data-ttu-id="e33dd-177">如果委派調用包含輸出參數或傳回值，其最終值會來自清單中最後一個委派的調用。</span><span class="sxs-lookup"><span data-stu-id="e33dd-177">If the delegate invocation includes output parameters or a return value, their final value will come from the invocation of the last delegate in the list.</span></span>

<span data-ttu-id="e33dd-178">如果在處理這類委派的調用期間發生例外狀況，而且該例外狀況不會在叫用的方法內攔截，則搜尋例外狀況 catch 子句會繼續在呼叫委派的方法中，並于後續的任何方法中執行不會叫用調用清單。</span><span class="sxs-lookup"><span data-stu-id="e33dd-178">If an exception occurs during processing of the invocation of such a delegate, and that exception is not caught within the method that was invoked, the search for an exception catch clause continues in the method that called the delegate, and any methods further down the invocation list are not invoked.</span></span>

<span data-ttu-id="e33dd-179">嘗試叫用值為 null 的委派實例，會導致 `System.NullReferenceException`類型的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="e33dd-179">Attempting to invoke a delegate instance whose value is null results in an exception of type `System.NullReferenceException`.</span></span>

<span data-ttu-id="e33dd-180">下列範例示範如何具現化、結合、移除和叫用委派：</span><span class="sxs-lookup"><span data-stu-id="e33dd-180">The following example shows how to instantiate, combine, remove, and invoke delegates:</span></span>

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

<span data-ttu-id="e33dd-181">如 `cd3 += cd1;`的語句所示，一個委派可以多次出現在調用清單中。</span><span class="sxs-lookup"><span data-stu-id="e33dd-181">As shown in the statement `cd3 += cd1;`, a delegate can be present in an invocation list multiple times.</span></span> <span data-ttu-id="e33dd-182">在此情況下，它只會在每次發生時叫用一次。</span><span class="sxs-lookup"><span data-stu-id="e33dd-182">In this case, it is simply invoked once per occurrence.</span></span> <span data-ttu-id="e33dd-183">在這類調用清單中，移除該委派時，在調用清單中最後一次出現的專案就是實際移除的。</span><span class="sxs-lookup"><span data-stu-id="e33dd-183">In an invocation list such as this, when that delegate is removed, the last occurrence in the invocation list is the one actually removed.</span></span>

<span data-ttu-id="e33dd-184">在執行最後一個語句之前，`cd3 -= cd1;`，委派 `cd3` 會參考空的調用清單。</span><span class="sxs-lookup"><span data-stu-id="e33dd-184">Immediately prior to the execution of the final statement, `cd3 -= cd1;`, the delegate `cd3` refers to an empty invocation list.</span></span> <span data-ttu-id="e33dd-185">嘗試從空白清單中移除委派（或從非空白清單中移除不存在的委派）並不是錯誤。</span><span class="sxs-lookup"><span data-stu-id="e33dd-185">Attempting to remove a delegate from an empty list (or to remove a non-existent delegate from a non-empty list) is not an error.</span></span>

<span data-ttu-id="e33dd-186">產生的輸出為：</span><span class="sxs-lookup"><span data-stu-id="e33dd-186">The output produced is:</span></span>

```console
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
