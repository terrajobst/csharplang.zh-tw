---
ms.openlocfilehash: 77c9ffda3a59cc5f3dcc3ec9848600c6c5e03eed
ms.sourcegitcommit: 27487fa0294f4cdb7e5f2478884149e05314fd8a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2019
ms.locfileid: "79484969"
---
# <a name="primary-constructors"></a><span data-ttu-id="047e3-101">主要的函式</span><span class="sxs-lookup"><span data-stu-id="047e3-101">Primary constructors</span></span>

* <span data-ttu-id="047e3-102">[x] 提議</span><span class="sxs-lookup"><span data-stu-id="047e3-102">[x] Proposed</span></span>
* <span data-ttu-id="047e3-103">[] 原型：未啟動</span><span class="sxs-lookup"><span data-stu-id="047e3-103">[ ] Prototype: Not started</span></span>
* <span data-ttu-id="047e3-104">[] 執行：未啟動</span><span class="sxs-lookup"><span data-stu-id="047e3-104">[ ] Implementation: Not started</span></span>
* <span data-ttu-id="047e3-105">[] 規格：未啟動</span><span class="sxs-lookup"><span data-stu-id="047e3-105">[ ] Specification: Not started</span></span>

## <a name="summary"></a><span data-ttu-id="047e3-106">摘要</span><span class="sxs-lookup"><span data-stu-id="047e3-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="047e3-107">類別可以有參數清單，當它們這麼做時，其基類規格可以有引數清單。</span><span class="sxs-lookup"><span data-stu-id="047e3-107">Classes can have a parameter list, and when they do, their base class specification can have an argument list.</span></span>
<span data-ttu-id="047e3-108">主要的函式參數位於整個類別宣告的範圍內，而且如果是由函式成員或匿名函式所捕捉，則會將它們儲存為類別中的私用欄位。</span><span class="sxs-lookup"><span data-stu-id="047e3-108">Primary constructor parameters are in scope throughout the class declaration, and if they are captured by a function member or anonymous function, they are stored as private fields in the class.</span></span>

## <a name="motivation"></a><span data-ttu-id="047e3-109">動機</span><span class="sxs-lookup"><span data-stu-id="047e3-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="047e3-110">在程式初始化程式碼中，通常會有很多的重複使用。</span><span class="sxs-lookup"><span data-stu-id="047e3-110">It is common to have a lot of boilerplate in program initialization code.</span></span> <span data-ttu-id="047e3-111">在一般情況下，會多次提及給定的資料 `x`：</span><span class="sxs-lookup"><span data-stu-id="047e3-111">In the general case, a given piece of data `x` is mentioned many times:</span></span>

- <span data-ttu-id="047e3-112">作為私用欄位 `_x`</span><span class="sxs-lookup"><span data-stu-id="047e3-112">As a private field `_x`</span></span>
- <span data-ttu-id="047e3-113">做為參數 `x` 至函數</span><span class="sxs-lookup"><span data-stu-id="047e3-113">As a parameter `x` to a constructor</span></span>
- <span data-ttu-id="047e3-114">在從函式的參數中，欄位的指派 `_x = x;`</span><span class="sxs-lookup"><span data-stu-id="047e3-114">In an assignment `_x = x;` of the field from the parameter in the constructor</span></span>
- <span data-ttu-id="047e3-115">做為屬性 `X`</span><span class="sxs-lookup"><span data-stu-id="047e3-115">As a property `X`</span></span>
- <span data-ttu-id="047e3-116">在屬性 setter 中 `x = value;`</span><span class="sxs-lookup"><span data-stu-id="047e3-116">In the property setter `x = value;`</span></span>
- <span data-ttu-id="047e3-117">在屬性 getter 中 `return x;`</span><span class="sxs-lookup"><span data-stu-id="047e3-117">In the property getter `return x;`</span></span>

``` c#
class C
{
    private string _x;
    
    public C(string x)
    {
        _x = x;
    }
    public string X
    {
        get => _x;
        set { if (value == null) throw new NullArgumentException(nameof(X)); _x = value; }
    }
}
```

<span data-ttu-id="047e3-118">對於不需要驗證或計算的屬性，可以使用 auto 屬性來減少冗長，因而減少宣告屬性之明確支援欄位的需求。</span><span class="sxs-lookup"><span data-stu-id="047e3-118">For properties that don't require validation or computation, the tedium can be reduced using auto-properties, thus cutting out the need to declare an explicit backing field for the property.</span></span> <span data-ttu-id="047e3-119">但是，如果您的屬性所需的邏輯超出自動屬性所提供的數目，上述就是您的最佳選擇。</span><span class="sxs-lookup"><span data-stu-id="047e3-119">But if your property requires any sort of logic beyond what an auto-property provides, the above is the best you an do.</span></span>

<span data-ttu-id="047e3-120">主要的函式會改為將方法引數直接放在整個類別的範圍中，藉此減少額外負荷，因此回避需要明確宣告支援欄位。</span><span class="sxs-lookup"><span data-stu-id="047e3-120">Primary constructors instead reduce the overhead by putting constructor arguments directly in scope throughout the class, again obviating the need to explicitly declare a backing field.</span></span> <span data-ttu-id="047e3-121">因此，上述範例會變成：</span><span class="sxs-lookup"><span data-stu-id="047e3-121">Thus, the above example would become:</span></span>

``` c#
class C(string x)
{
    public string X
    {
        get => x;
        set { if (value == null) throw new NullArgumentException(nameof(X)); x = value; }
    }
}
```

<span data-ttu-id="047e3-122">在此範例中，主要的函式會將 `x` 的命名實體數目減少為三到二，回避 `_x` 支援欄位。</span><span class="sxs-lookup"><span data-stu-id="047e3-122">In this example, the primary constructor reduces the number of named entities for `x` from three to two, obviating the `_x` backing field.</span></span> <span data-ttu-id="047e3-123">它會移除三個成員宣告中的兩個（僅保留屬性宣告本身），並減少從八到五個 `x`/`_x``X` /的總次數。</span><span class="sxs-lookup"><span data-stu-id="047e3-123">It removes two out of three member declarations (keeping only the property declaration itself), and reduces the total number of mentions of `x`/`_x`/`X` from eight to five.</span></span>


## <a name="detailed-design"></a><span data-ttu-id="047e3-124">詳細設計</span><span class="sxs-lookup"><span data-stu-id="047e3-124">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="047e3-125">類別可以有參數清單：</span><span class="sxs-lookup"><span data-stu-id="047e3-125">Classes can have a parameter list:</span></span>

``` c#
public class C(int i, string s)
{
    ...
}
```

<span data-ttu-id="047e3-126">參數清單會為類別隱含宣告函式，並具有與類別本身相同的存取範圍。</span><span class="sxs-lookup"><span data-stu-id="047e3-126">The parameter list causes a constructor to be implicitly declared for the class, with the same accessibility as the class itself.</span></span>

``` c#
new C(5, "Hello");
```

<span data-ttu-id="047e3-127">主要的函式參數在整個類別主體的範圍內。</span><span class="sxs-lookup"><span data-stu-id="047e3-127">Primary constructor parameters are in scope throughout the class body.</span></span> <span data-ttu-id="047e3-128">如果這些函式是由函式成員或匿名函式所捕捉，它們就會在類別中儲存為私用欄位。</span><span class="sxs-lookup"><span data-stu-id="047e3-128">If they are captured by a function member or anonymous function, they become stored as private fields in the class.</span></span> <span data-ttu-id="047e3-129">如果它們只在初始化期間使用，則不會儲存在物件中。</span><span class="sxs-lookup"><span data-stu-id="047e3-129">If they are only used during initialization they will not be stored in the object.</span></span>

``` c#
public class C(int i, string s)
{
    int[] a = new int[i]; // i not captured
    public int S => s;    // s captured
}
```

<span data-ttu-id="047e3-130">如果具有主要函式的類別具有基類規格，則可以有引數清單。</span><span class="sxs-lookup"><span data-stu-id="047e3-130">If a class with a primary constructor has a base class specification, that one can have an argument list.</span></span> <span data-ttu-id="047e3-131">這會當做隱含宣告之函式的 `base(...)` 初始化運算式的引數清單。</span><span class="sxs-lookup"><span data-stu-id="047e3-131">This serves as the argument list to a `base(...)` initializer of the implicitly declared constructor.</span></span> <span data-ttu-id="047e3-132">如果未提供引數清單，則會假設為空的。</span><span class="sxs-lookup"><span data-stu-id="047e3-132">If no argument list is provided, an empty one is assumed.</span></span>

``` c#
public class C(int i, string s) : B(s)
{
    ...
}
```
<span data-ttu-id="047e3-133">類別也可以有明確定義的函式，但這些都必須使用 `this(...)` 的初始化運算式。</span><span class="sxs-lookup"><span data-stu-id="047e3-133">The class can have explicitly defined constructors as well, but those all have to use a `this(...)` initializer.</span></span> <span data-ttu-id="047e3-134">這可確保在結構化新的實例時，一律會呼叫主要的函式。</span><span class="sxs-lookup"><span data-stu-id="047e3-134">This ensures that the primary constructor is always called when a new instance is constructed.</span></span>

<span data-ttu-id="047e3-135">類別主體中的所有初始化運算式都會成為所產生之函式中的指派。</span><span class="sxs-lookup"><span data-stu-id="047e3-135">All initializers in the class body will become assignments in the generated constructor.</span></span> <span data-ttu-id="047e3-136">這表示，與其他類別不同的是，初始化運算式會在叫用基底函式*之後*執行，而不是在之前。</span><span class="sxs-lookup"><span data-stu-id="047e3-136">This means that, unlike other classes, initializers will run *after* the base constructor has been invoked, not before.</span></span> <span data-ttu-id="047e3-137">此外，所產生的類別將會包含指派，以初始化為了儲存成員所捕捉的主要函式參數而產生的任何私用欄位。</span><span class="sxs-lookup"><span data-stu-id="047e3-137">In addition, the generated class will contain assignments to initialize any private fields that were generated to store primary constructor parameters that were captured by members.</span></span> <span data-ttu-id="047e3-138">這些成員會重寫成使用私用欄位，而不是參數，其方式類似于 lambda 運算式的「閉」。</span><span class="sxs-lookup"><span data-stu-id="047e3-138">Those members are rewritten to use the private field instead of the parameter in a manner similar to closures for lambda expressions.</span></span> <span data-ttu-id="047e3-139">產生的主要欄位會先初始化，然後以類別中的外觀順序來執行初始化運算式產生的指派。</span><span class="sxs-lookup"><span data-stu-id="047e3-139">The generated primary fields are initialized first, and then the initializer-generated assignments are executed in the order of appearance in the class.</span></span>

<span data-ttu-id="047e3-140">在上述範例中，類別宣告的效果就如同如下所示重寫：</span><span class="sxs-lookup"><span data-stu-id="047e3-140">For the above example, the effect of the class declaration is as if rewritten like this:</span></span>

``` c#
public class C : B
{
    public C(int i, string s) : base(s)
    {
        __s = s;        // store parameter s for captured use
        a = new int[i]; // initialize a
    }
    int __s; // generated field for capture of s
    
    int[] a;
    public int S => __s; // s replaced with captured __s
}
```

<span data-ttu-id="047e3-141">此捕捉的限制與使用 lambda 運算式來捕捉區域變數的方式相似。</span><span class="sxs-lookup"><span data-stu-id="047e3-141">The capture has similar restrictions to the capture of local variables by lambda expressions.</span></span> <span data-ttu-id="047e3-142">比方說，主要的函式中允許 `ref` 和 `out` 參數，但是無法捕捉我的成員主體。</span><span class="sxs-lookup"><span data-stu-id="047e3-142">For instance, `ref` and `out` parameters are allowed in primary constructors, but cannot be captured my member bodies.</span></span>


## <a name="drawbacks"></a><span data-ttu-id="047e3-143">缺點</span><span class="sxs-lookup"><span data-stu-id="047e3-143">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="047e3-144">以粗略的重要性排序。</span><span class="sxs-lookup"><span data-stu-id="047e3-144">In rough order of significance.</span></span>

* <span data-ttu-id="047e3-145">提案會使用也已針對位置記錄提議的語法。</span><span class="sxs-lookup"><span data-stu-id="047e3-145">The proposal uses syntax that has also been proposed for positional records.</span></span> <span data-ttu-id="047e3-146">如果我們想要這兩項功能，則需要一些住宿。</span><span class="sxs-lookup"><span data-stu-id="047e3-146">If we desire both features, some accommodation is required.</span></span> <span data-ttu-id="047e3-147">例如</span><span class="sxs-lookup"><span data-stu-id="047e3-147">E.g.</span></span> <span data-ttu-id="047e3-148">已建議記錄上的 `data` 修飾詞。</span><span class="sxs-lookup"><span data-stu-id="047e3-148">a `data` modifier on records has been proposed.</span></span>
* <span data-ttu-id="047e3-149">結構化物件的配置大小較不明顯，因為編譯器會根據類別的完整文字，判斷是否要配置主要的函式參數的欄位。</span><span class="sxs-lookup"><span data-stu-id="047e3-149">The allocation size of constructed objects is less obvious, as the compiler determines whether to allocate a field for a primary constructor parameter based on the full text of the class.</span></span> <span data-ttu-id="047e3-150">這項風險類似于 lambda 運算式的隱含捕獲變數。</span><span class="sxs-lookup"><span data-stu-id="047e3-150">This risk is similar to the implicit capture of variables by lambda expressions.</span></span>
* <span data-ttu-id="047e3-151">常見的誘惑（或意外的模式）可能是在多個繼承層級上捕捉「相同」參數，因為它會傳遞至函式鏈，而不是在基類中明確 allotting 受保護的欄位，導致重複的配置物件中的相同資料。</span><span class="sxs-lookup"><span data-stu-id="047e3-151">A common temptation (or accidental pattern) might be to capture the "same" parameter at multiple levels of inheritance as it is passed up the constructor chain instead of explicitly allotting it a protected field at the base class, leading to duplicated allocations for the same data in objects.</span></span> <span data-ttu-id="047e3-152">這非常類似于現今使用 auto 屬性覆寫自動屬性的風險。</span><span class="sxs-lookup"><span data-stu-id="047e3-152">This is very similar to today's risk of overriding auto-properties with auto-properties.</span></span> 
* <span data-ttu-id="047e3-153">如先前所述，不會有任何可能通常以「函式主體」表示的其他邏輯位置。</span><span class="sxs-lookup"><span data-stu-id="047e3-153">As proposed above, there is no place for additional logic that might usually expressed in constructor bodies.</span></span> <span data-ttu-id="047e3-154">下面的「主要的程式式主體」延伸模組會解決此情況。</span><span class="sxs-lookup"><span data-stu-id="047e3-154">The "Primary constructor bodies" extension below addresses that.</span></span>
* <span data-ttu-id="047e3-155">如提議，執行順序的語義與一般的處理函式稍有不同。</span><span class="sxs-lookup"><span data-stu-id="047e3-155">As proposed, execution order semantics are subtly different than with ordinary constructors.</span></span> <span data-ttu-id="047e3-156">這可能會受到補救，但代價是某些擴充功能提案（特別是「主要的函式主體」）。</span><span class="sxs-lookup"><span data-stu-id="047e3-156">This could probably be remedied, but at the cost of some of the extension proposals (notably "Primary constructor bodies").</span></span>
* <span data-ttu-id="047e3-157">只有在單一的函式可以指定為主要時，此提案才有作用。</span><span class="sxs-lookup"><span data-stu-id="047e3-157">The proposal only works when a single constructor can be designated primary.</span></span>
* <span data-ttu-id="047e3-158">沒有任何方法可以擁有類別的個別存取範圍和主要的函式。</span><span class="sxs-lookup"><span data-stu-id="047e3-158">There is no way to have separate accessibility of the class and the primary constructor.</span></span> <span data-ttu-id="047e3-159">比方說，在公用函式全都委派給一個需要的私用「組建-全部」的函式。</span><span class="sxs-lookup"><span data-stu-id="047e3-159">For instance, in situations where public constructors all delegate to one private "build-it-all" constructor that would be needed.</span></span> <span data-ttu-id="047e3-160">如有需要，稍後可能會建議語法。</span><span class="sxs-lookup"><span data-stu-id="047e3-160">If necessary, syntax could be proposed for that later.</span></span>


## <a name="alternatives"></a><span data-ttu-id="047e3-161">替代方案</span><span class="sxs-lookup"><span data-stu-id="047e3-161">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="047e3-162">視細節而定，完整位置記錄可能是替代的，或可能與主要的函式並存。</span><span class="sxs-lookup"><span data-stu-id="047e3-162">Full-on positional records may be an alternative, or may coexist with primary constructors, depending on the specifics.</span></span> <span data-ttu-id="047e3-163">在*較少*的案例中，他們可能會有*更多*的縮寫。</span><span class="sxs-lookup"><span data-stu-id="047e3-163">They would allow for *more* abbreviation in a *smaller* number of scenarios.</span></span> <span data-ttu-id="047e3-164">這兩者都很有用，但兩者都可能會大材小用，除非它們可以彼此整齊地進行整合。</span><span class="sxs-lookup"><span data-stu-id="047e3-164">So both are potentially useful, but having both may be overkill, unless they can be somewhat neatly integrated with each other.</span></span>


## <a name="possible-extensions"></a><span data-ttu-id="047e3-165">可能的擴充</span><span class="sxs-lookup"><span data-stu-id="047e3-165">Possible extensions</span></span>
[extensions]: #possible-extensions

<span data-ttu-id="047e3-166">這些是核心提案的變化或新增專案，可能會與它一起考慮，或在之後的階段中被視為有用。</span><span class="sxs-lookup"><span data-stu-id="047e3-166">These are variations or additions to the core proposal that may be considered in conjunction with it, or at a later stage if deemed useful.</span></span>

### <a name="primary-constructor-bodies"></a><span data-ttu-id="047e3-167">主要的函數主體</span><span class="sxs-lookup"><span data-stu-id="047e3-167">Primary constructor bodies</span></span>

<span data-ttu-id="047e3-168">函式本身通常會包含參數驗證邏輯或其他無法以初始化運算式表示的重要初始化程式碼。</span><span class="sxs-lookup"><span data-stu-id="047e3-168">Constructors themselves often contain parameter validation logic or other nontrivial initialization code that cannot be expressed as initializers.</span></span>

<span data-ttu-id="047e3-169">主要的函式可以擴充，以允許語句區塊直接出現在類別主體中。</span><span class="sxs-lookup"><span data-stu-id="047e3-169">Primary constructors could be extended to allow statement blocks to appear directly in the class body.</span></span> <span data-ttu-id="047e3-170">這些語句會在初始化的指派之間的出現時，插入產生的函式中，因此會使用初始化運算式來執行。</span><span class="sxs-lookup"><span data-stu-id="047e3-170">Those statements would be inserted in the generated constructor at the point where they appear between initializing assignments, and would thus be executed interspersed with initializers.</span></span> <span data-ttu-id="047e3-171">例如：</span><span class="sxs-lookup"><span data-stu-id="047e3-171">For instance:</span></span>

``` c#
public class C(int i, string s) : B(s)
{
    {
        if (i < 0) throw new ArgumentOutOfRangeException(nameof(i));
    }
    int[] a = new int[i];
    public int S => s;
}
```

### <a name="initializer-fields-and-initializer-functions"></a><span data-ttu-id="047e3-172">初始化運算式欄位和初始化運算式函式</span><span class="sxs-lookup"><span data-stu-id="047e3-172">Initializer fields and initializer functions</span></span>

<span data-ttu-id="047e3-173">在具有主要函式的類別中，我們可以考慮不含存取範圍修飾詞的欄位和方法宣告，更像本機變數和區域函式：</span><span class="sxs-lookup"><span data-stu-id="047e3-173">In a class with a primary constructor we could consider field and method declarations without accessibility modifiers to be more like local variables and local functions:</span></span>

* <span data-ttu-id="047e3-174">就像主要的函式參數一樣，只有在函式成員中使用「初始化運算式欄位」時，才會將其捕捉到實際的私用欄位中。</span><span class="sxs-lookup"><span data-stu-id="047e3-174">Just like primary constructor parameters the "initializer fields" would only be captured into an actual private field if they were used in function members.</span></span>
* <span data-ttu-id="047e3-175">「初始化運算式函式」只有在其他函式成員中使用時，才會被視為要捕捉主要的「函式參數」和「初始化運算式」欄位。</span><span class="sxs-lookup"><span data-stu-id="047e3-175">The "initializer functions" would only be considered to capture primary constructor parameters and initializer fields if they were themselves used in other function members.</span></span> <span data-ttu-id="047e3-176">如果未捕捉到，可能會以更理想的方式產生，例如區域函式。</span><span class="sxs-lookup"><span data-stu-id="047e3-176">If not captured, they could be generated in a more optimal fashion, like local functions.</span></span>
* <span data-ttu-id="047e3-177">就像主要的函式參數一樣，它們無法透過成員存取來使用，而只是簡單的名稱。</span><span class="sxs-lookup"><span data-stu-id="047e3-177">Just like primary constructor parameters they would not be available via member access, but only as a simple name.</span></span>

<span data-ttu-id="047e3-178">這可以用於僅與初始化相關的暫存變數和 helper 函式：</span><span class="sxs-lookup"><span data-stu-id="047e3-178">This could be used for temporary variables and helper functions that are only relevant to initialization:</span></span>

``` c#
public class C(string s)
{
    int size = s.Length;             // not captured
    int[] Create() => new int[size]; // not captured
    int[] a = Create();
    ...
}
```

<span data-ttu-id="047e3-179">這可能太微妙，特別是因為其他地方沒有存取範圍修飾詞只是表示 `private`。</span><span class="sxs-lookup"><span data-stu-id="047e3-179">This may be too subtle, especially since the absence of accessibility modifiers elsewhere simply means `private`.</span></span> 

### <a name="initializer-statements"></a><span data-ttu-id="047e3-180">初始化運算式語句</span><span class="sxs-lookup"><span data-stu-id="047e3-180">Initializer statements</span></span>

<span data-ttu-id="047e3-181">上述至延伸模組的根式組合，就是直接允許類別主體中的語句。</span><span class="sxs-lookup"><span data-stu-id="047e3-181">A radical combination of the above to extensions would be to simply allow statements directly in the class body.</span></span> <span data-ttu-id="047e3-182">這類語句與上面所述的交錯式的函數主體完全相同，不同之處在于它們不需要包含在 `{ }`中。</span><span class="sxs-lookup"><span data-stu-id="047e3-182">Such statements are exactly as the interspersed constructor bodies proposed above, except they don't need to be enclosed in `{ }`.</span></span> <span data-ttu-id="047e3-183">為了充分發揮效用，"local" 變數和 helper 函式也必須在類別的最上層，以在上述延伸模組「初始化運算式欄位和初始化運算式函式」中探索的方式來表示。</span><span class="sxs-lookup"><span data-stu-id="047e3-183">For this to be sufficiently useful, "local" variables and helper functions would need to also be expressible at the top level of the class, in the manner explored in the extension "Initializer fields and initializer functions" above.</span></span>


### <a name="member-access"></a><span data-ttu-id="047e3-184">成員存取</span><span class="sxs-lookup"><span data-stu-id="047e3-184">Member access</span></span>

<span data-ttu-id="047e3-185">核心提案會將主要的函式參數視為只能參考簡單名稱的參數。</span><span class="sxs-lookup"><span data-stu-id="047e3-185">The core proposal treats primary constructor parameters as parameters that can only be referred as simple names.</span></span> <span data-ttu-id="047e3-186">另一個替代方式是讓它們被視為私用欄位（也就是具有成員存取權），*即使*有時不會以欄位的形式產生。</span><span class="sxs-lookup"><span data-stu-id="047e3-186">An alternative is to allow them to be referenced as if they were private fields, i.e. with a member access, *even* if they are sometimes not generated as fields.</span></span> <span data-ttu-id="047e3-187">這可讓它們在以本機變數加上陰影時被視為 `this.x`，並從不同的實例存取為 `other.x`。</span><span class="sxs-lookup"><span data-stu-id="047e3-187">This would allow them to be referenced as `this.x` when shadowed by local variables, and accessed from a different instance as `other.x`.</span></span>

<span data-ttu-id="047e3-188">如果套用至「初始化運算式欄位和初始化運算式函式」延伸模組，這也會降低這些與一般私用成員不同的程度。</span><span class="sxs-lookup"><span data-stu-id="047e3-188">If applied to the "initializer fields and initializer functions" extension this would also reduce the degree to which those were different from ordinary private members.</span></span> <span data-ttu-id="047e3-189">唯一的差別在於，如果只在初始化期間使用，編譯器就可以自由地從物件中省略它們。</span><span class="sxs-lookup"><span data-stu-id="047e3-189">The only difference would then be that the compiler is free to elide them from the object if only used during initialization.</span></span>

