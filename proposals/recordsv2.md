---
ms.openlocfilehash: 5636157ba54e93587847d6f2f7aac2dc675f3112
ms.sourcegitcommit: af27912886f22cda9b98b7769447d85cd9732736
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/09/2019
ms.locfileid: "79484997"
---

# <a name="records-v2"></a><span data-ttu-id="8ec99-101">記錄 v2</span><span class="sxs-lookup"><span data-stu-id="8ec99-101">Records v2</span></span>

<span data-ttu-id="8ec99-102">在過去，我們已將記錄視為能夠使用資料的功能。</span><span class="sxs-lookup"><span data-stu-id="8ec99-102">In the past we've thought about records as a feature to enable working with data.</span></span>

<span data-ttu-id="8ec99-103">「使用資料」是具有許多 facet 的大型群組，因此在隔離的情況下可能會有興趣。</span><span class="sxs-lookup"><span data-stu-id="8ec99-103">"Working with data" is a big group with a number of facets, so it may be interesting to look at each in isolation.</span></span> <span data-ttu-id="8ec99-104">讓我們從今天的記錄範例和部分缺點開始。</span><span class="sxs-lookup"><span data-stu-id="8ec99-104">Let's start by looking at an example of records today and some of its drawbacks.</span></span>

<span data-ttu-id="8ec99-105">比方說，簡單的記錄會定義如下</span><span class="sxs-lookup"><span data-stu-id="8ec99-105">For instance, a simple record would be defined today as follows</span></span>

```C#
public class UserInfo
{
    public string Username { get; set; }
    public string Email { get; set; }
    public bool IsAdmin  { get; set; } = false;
}
```

<span data-ttu-id="8ec99-106">而且會讀取使用方式</span><span class="sxs-lookup"><span data-stu-id="8ec99-106">and the usage would read</span></span>

```C#
void M()
{
    var userInfo = new UserInfo() 
    {
        Username = "andy",
        Email = "angocke@microsoft.com",
        IsAdmin = true
    };
}
```

<span data-ttu-id="8ec99-107">這段程式碼有顯著的優點：</span><span class="sxs-lookup"><span data-stu-id="8ec99-107">There are significant advantages in this code:</span></span>

1. <span data-ttu-id="8ec99-108">定義的版本具有彈性，可以輕鬆地新增或移動屬性</span><span class="sxs-lookup"><span data-stu-id="8ec99-108">The definition is version resilient, properties can easily be added or moved</span></span>
2. <span data-ttu-id="8ec99-109">屬性可以依任何順序設定，而初始化中的名稱一律符合存取子</span><span class="sxs-lookup"><span data-stu-id="8ec99-109">Properties can be set in any order, and the names in the initialization always match the accessors</span></span>
3. <span data-ttu-id="8ec99-110">具有預設值的屬性可以直接略過</span><span class="sxs-lookup"><span data-stu-id="8ec99-110">Properties with default values can simply be skipped</span></span>

<span data-ttu-id="8ec99-111">第一個缺點是屬性現在必須是可變動的。</span><span class="sxs-lookup"><span data-stu-id="8ec99-111">The first flaw is that the properties must now be mutable.</span></span>

# <a name="mutability"></a><span data-ttu-id="8ec99-112">可變動性</span><span class="sxs-lookup"><span data-stu-id="8ec99-112">Mutability</span></span>

<span data-ttu-id="8ec99-113">我們C#想要提供一種方法，在物件初始化運算式中設定 `readonly` 成員。</span><span class="sxs-lookup"><span data-stu-id="8ec99-113">What we'd like is for C# to provide a way to set a `readonly` member in object initializers.</span></span>
<span data-ttu-id="8ec99-114">由於某些類型可能不會在此初始化中設計，因此我們也希望它可以加入宣告。</span><span class="sxs-lookup"><span data-stu-id="8ec99-114">Since some types may not have been designed with this initialization in mind, we'd also like it to be opt-in.</span></span>

<span data-ttu-id="8ec99-115">建議的解決方案是新的修飾詞，`initonly`，可套用至屬性和欄位：</span><span class="sxs-lookup"><span data-stu-id="8ec99-115">The proposed solution is a new modifier, `initonly`, that can be applied to properties and fields:</span></span>

```C#
public class UserInfo
{
    public initonly string Username { get; }
    public initonly string Email { get; }
    public initonly bool IsAdmin { get; } = false;
}
```

<span data-ttu-id="8ec99-116">這是非常簡單的 codegen：我們剛剛設定了 readonly 欄位。</span><span class="sxs-lookup"><span data-stu-id="8ec99-116">The codegen for this is surprisingly straight forward: we just set the readonly field.</span></span>
<span data-ttu-id="8ec99-117">具體而言，降低的屬性看起來會像這樣：</span><span class="sxs-lookup"><span data-stu-id="8ec99-117">Specifically, the lowered properties would look like:</span></span>

```C#
public class UserInfo
{
    private readonly string <Backing>_username;
    public string get_Username() => <Backing>_username;
    [return: modreq(initonly)]
    public void set_Username(string value) { <Backing>_username = value; }
    ...
}
```

<span data-ttu-id="8ec99-118">CLR 會考慮將 readonly 欄位設定為無法驗證，但不安全。</span><span class="sxs-lookup"><span data-stu-id="8ec99-118">The CLR considers setting readonly fields to be unverifiable, but not unsafe.</span></span> <span data-ttu-id="8ec99-119">若要支援更先進的驗證器，請注意下列規則：唯讀欄位只能在 `initonly` 方法內，或在 CLR 堆疊的新物件上修改，而且尚未透過存放區或方法呼叫來發行。</span><span class="sxs-lookup"><span data-stu-id="8ec99-119">To support a more advanced verifier, the following rule is proposed: a readonly field can be modified only inside `initonly` methods, or on a new object that is on the CLR stack and has not been published via a store or method call.</span></span>

<span data-ttu-id="8ec99-120">這應該能解決 `UserInfo` 範例中可變動性的許多問題，而不需要複雜或脆弱的發出策略。</span><span class="sxs-lookup"><span data-stu-id="8ec99-120">This should solve many of the problems with mutability in the `UserInfo` example, while not requiring complicated or brittle emit strategies.</span></span> <span data-ttu-id="8ec99-121">不過，不會出現新的問題：使用變更輕鬆地建立物件。</span><span class="sxs-lookup"><span data-stu-id="8ec99-121">However, immutability does present a new problem: easily constructing an object with changes.</span></span>

# <a name="with-ing"></a><span data-ttu-id="8ec99-122">With-ing</span><span class="sxs-lookup"><span data-stu-id="8ec99-122">With-ing</span></span>

<span data-ttu-id="8ec99-123">使用不可變性進行程式設計時，對物件進行的變更是藉由使用變更來建立複本來進行，而不是直接在物件上進行變更。</span><span class="sxs-lookup"><span data-stu-id="8ec99-123">When programming with immutability, making changes to an object is done by constructing a copy with changes instead of making the changes directly on the object.</span></span> <span data-ttu-id="8ec99-124">可惜的是，即使使用目前的樣式記錄， C#也沒有任何便利的方式可以在中執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="8ec99-124">Unfortunately, there's no convenient way to do this in C#, even with current-style records.</span></span> <span data-ttu-id="8ec99-125">先前已提議針對實作為該功能的記錄，提供某種類型自動產生的「With」方法。</span><span class="sxs-lookup"><span data-stu-id="8ec99-125">It's been previously proposed that some kind of autogenerated "With" method be provided for records that implements that functionality.</span></span> <span data-ttu-id="8ec99-126">如果我們有這種機制，請務必使用 `initonly` 成員。</span><span class="sxs-lookup"><span data-stu-id="8ec99-126">If we have such a mechanism, it's important that it work with `initonly` members.</span></span> <span data-ttu-id="8ec99-127">為了達成此目的，我們建議您加入類似物件初始化運算式的 `with` 運算式。</span><span class="sxs-lookup"><span data-stu-id="8ec99-127">To achieve this, it's proposed that we add a `with` expression, analogous to an object initializer.</span></span> <span data-ttu-id="8ec99-128">範例用法如下所示：</span><span class="sxs-lookup"><span data-stu-id="8ec99-128">Sample usage would be as follows:</span></span>

```C#
var userInfo = new UserInfo() 
{
    Username = "andy",
    Email = "angocke@microsoft.com",
    IsAdmin = true
};
var newUserName = userInfo with { Username = "angocke" };
```

<span data-ttu-id="8ec99-129">產生的 `newUserName` 物件會是 `userInfo`的複本，`Username` 設定為 "angocke"。</span><span class="sxs-lookup"><span data-stu-id="8ec99-129">The resulting `newUserName` object would be a copy of `userInfo`, with `Username` set to "angocke".</span></span>
<span data-ttu-id="8ec99-130">`with` 運算式上的 codegen 也會類似物件初始化運算式：會建立新的物件，然後在方法主體中呼叫 `initonly` `Username` setter。</span><span class="sxs-lookup"><span data-stu-id="8ec99-130">The codegen on the `with` expression would also be similar to the object initializer: a new object is constructed, and then the `initonly` `Username` setter would be called in the method body.</span></span>

<span data-ttu-id="8ec99-131">當然，此處的差異在於，所建立的新物件不是簡單的新物件建立，而是原始物件的複本。</span><span class="sxs-lookup"><span data-stu-id="8ec99-131">Of course, the difference here is that the new object being constructed is not a simple new object creation, it is a duplicate of the original object.</span></span> <span data-ttu-id="8ec99-132">為了提供這種功能，我們要求物件必須提供可提供重複物件的 "With 「函式」。</span><span class="sxs-lookup"><span data-stu-id="8ec99-132">To provide this functionality, we require that the object provide a "With constructor" that provides a duplicate object.</span></span> <span data-ttu-id="8ec99-133">範例 `With` 的函式看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="8ec99-133">A sample `With` constructor would look like:</span></span>

```C#
class UserInfo
{
    ...
    [WithConstructor] // placeholder syntax, up for debate
    public UserInfo With()
    {
        return new UserInfo() { Username = this.Username, Email = this.Email, IsAdmin = this.IsAdmin };
    }
}
```

<span data-ttu-id="8ec99-134">值得注意的是，`with` 運算式會設定 `initonly` 成員，就像物件初始化運算式一樣，因此若要支援驗證，我們必須確保在設定 `initonly` 成員之前，無法發行物件。</span><span class="sxs-lookup"><span data-stu-id="8ec99-134">Notably, the `with` expression will set `initonly` members, just like the object initializer, so to support verification we must ensure that the object cannot have been published before the `initonly` members are set.</span></span> <span data-ttu-id="8ec99-135">若要強制執行此動作，`WithConstructor` 屬性（或對等的語法）將會對方法強制執行新的規則：所有傳回語句都必須直接包含物件建立運算式，可能是物件初始化運算式。</span><span class="sxs-lookup"><span data-stu-id="8ec99-135">To enforce this, the `WithConstructor` attribute (or equivalent syntax) will enforce a new rule for the method: all return statements must directly contain an object creation expression, possibly with an object initializer.</span></span>

<span data-ttu-id="8ec99-136">如果 `With` 的處理常式需要驗證，則使用者可能會引進用來進行驗證的函式，例如</span><span class="sxs-lookup"><span data-stu-id="8ec99-136">If the `With` constructor requires validation, the user may introduce a constructor to do that validation, e.g.</span></span>

```C#
class UserInfo
{
    ...
    private UserInfo(UserInfo original)
    {
        // validation code
    }
    [WithConstructor]
    public UserInfo With() => new UserInfo(this);
}
```

<span data-ttu-id="8ec99-137">與 `With` 相關聯的最後一項複雜度是繼承。</span><span class="sxs-lookup"><span data-stu-id="8ec99-137">The last piece of complexity associated with `With` is inheritance.</span></span> <span data-ttu-id="8ec99-138">如果您的記錄是可延伸的，您將需要為子類別提供新的 `With`。</span><span class="sxs-lookup"><span data-stu-id="8ec99-138">If your record is extensible, you will need to provide a new `With` for the subclass.</span></span> <span data-ttu-id="8ec99-139">這可以透過下列方式來達成：</span><span class="sxs-lookup"><span data-stu-id="8ec99-139">This can be achieved as follows:</span></span>

```C#
class Base
{
    ...
    protected Base(Base original)
    {
        // validation
    }
    [WithConstructor]
    public virtual Base With() => new Base(this);
}
class Derived : Base
{
    ...
    protected Derived(Derived original)
    : base(original)
    {
        // validation
    }
    [WithConstructor]
    public override Derived With() => new Derived(this);
}
```

<span data-ttu-id="8ec99-140">請注意這裡的另一個複雜度：若要以衍生型別覆寫 `With` 的處理函式，語言也必須支援覆寫中的協變數傳回類型。</span><span class="sxs-lookup"><span data-stu-id="8ec99-140">Note one additional piece of complexity here: in order to override the `With` constructor with the derived type the language will also need to support covariant return types in overrides.</span></span>
<span data-ttu-id="8ec99-141">[這裡](https://github.com/dotnet/csharplang/blob/725763343ad44a9251b03814e6897d87fe553769/proposals/covariant-returns.md)已有這項功能的個別提案。</span><span class="sxs-lookup"><span data-stu-id="8ec99-141">There is already a separate proposal for this feature [here](https://github.com/dotnet/csharplang/blob/725763343ad44a9251b03814e6897d87fe553769/proposals/covariant-returns.md).</span></span>

<span data-ttu-id="8ec99-142">**缺陷**</span><span class="sxs-lookup"><span data-stu-id="8ec99-142">**Drawbacks**</span></span>

- <span data-ttu-id="8ec99-143">`WithConstructor`s 中的所有 return 語句都包含新的物件運算式，因此受到限制。</span><span class="sxs-lookup"><span data-stu-id="8ec99-143">Making all return statements in `WithConstructor`s contain new object expressions is restrictive.</span></span>
  <span data-ttu-id="8ec99-144">流程分析可能會降低這種情況，以確保新的物件不會將方法 escape</span><span class="sxs-lookup"><span data-stu-id="8ec99-144">This could be possibly be mitigated by flow analysis that ensures the new object doesn't escape the method</span></span>
- <span data-ttu-id="8ec99-145">透過編譯器訣竅支援覆寫中的變異數，將需要存根方法，這會隨著繼承深度而增加 quadratically。</span><span class="sxs-lookup"><span data-stu-id="8ec99-145">Supporting variance in overrides through compiler tricks will require stub methods, which will grow quadratically with the inheritance depth.</span></span> <span data-ttu-id="8ec99-146">Stub 方法的需求是因為覆寫簽章完全相符的執行時間需求。</span><span class="sxs-lookup"><span data-stu-id="8ec99-146">The need for a stub method is due to a runtime requirement that override signatures match exactly.</span></span> <span data-ttu-id="8ec99-147">如果執行時間需求已放寬，則完全不需要存根方法。</span><span class="sxs-lookup"><span data-stu-id="8ec99-147">If the runtime requirement were loosened, the stub methods would not be required at all.</span></span>
- <span data-ttu-id="8ec99-148">使用表單的連鎖的函式，`Type(Type original)` 有效地保留該模式的使用方式。</span><span class="sxs-lookup"><span data-stu-id="8ec99-148">Using chained constructors of the form `Type(Type original)` effectively reserves that constructor for the use of the pattern.</span></span> <span data-ttu-id="8ec99-149">因為這些函式具有唯一的語義，而且無法重新命名，所以可能會受到限制。</span><span class="sxs-lookup"><span data-stu-id="8ec99-149">Since constructors have unique semantics and cannot be re-named this could be limiting.</span></span>


## <a name="wrapping-it-all-up-records"></a><span data-ttu-id="8ec99-150">全部包裝：記錄</span><span class="sxs-lookup"><span data-stu-id="8ec99-150">Wrapping it all up: Records</span></span>

<span data-ttu-id="8ec99-151">上述功能可實現之前非常難以的程式設計樣式。</span><span class="sxs-lookup"><span data-stu-id="8ec99-151">The above features enable a style of programming that was very difficult before.</span></span> <span data-ttu-id="8ec99-152">但是，即使有了新的功能，也可能會有相當詳細的資訊，而且容易出錯來為所有專案加上附注</span><span class="sxs-lookup"><span data-stu-id="8ec99-152">But even with the new features it could be quite verbose and error prone to annotate everything yourself.</span></span> <span data-ttu-id="8ec99-153">還有幾個專案，像是 Equals 和 GetHashCode，現在已經可以撰寫，這只是費力。</span><span class="sxs-lookup"><span data-stu-id="8ec99-153">There are also a few items, like Equals and GetHashCode, which can already be written today, it's just laborious.</span></span>
<span data-ttu-id="8ec99-154">此外，在這些新的基本專案之上執行相等的重大缺點是，結構化相等性應該隨著新資料的資料類型而變更，但是在手動處理時，可能會導致這些專案無法同步。</span><span class="sxs-lookup"><span data-stu-id="8ec99-154">Moreover, a significant flaw in implementing equality on top of these new primitives is that structural equality is something that should change with your data type as new data is added, but when handling it manually it is likely that these things can get out of sync.</span></span>

<span data-ttu-id="8ec99-155">因此，建議C#支援記錄的新語法，而不是提供新的功能，而是用來設定預設值以及產生設計用於記錄的程式碼。</span><span class="sxs-lookup"><span data-stu-id="8ec99-155">Therefore, it is proposed that C# support new syntax for records, not for providing new features, but for setting defaults and generating code designed for use in records.</span></span> <span data-ttu-id="8ec99-156">範例語法看起來會像這樣</span><span class="sxs-lookup"><span data-stu-id="8ec99-156">Example syntax would look like</span></span>

```C#
data class UserInfo
{
    public string Username { get; }
    public string Email { get; }
    public bool IsAdmin { get; } = false;
}
```

<span data-ttu-id="8ec99-157">針對此類別產生的程式碼會將所有公用欄位和自動屬性視為記錄的結構成員。</span><span class="sxs-lookup"><span data-stu-id="8ec99-157">The generated code for this class would regard all public fields and auto-properties as structural members of the record.</span></span> <span data-ttu-id="8ec99-158">您可以使用可用於包含或排除成員的新 `RecordMember(bool)` 屬性來自訂記錄成員。</span><span class="sxs-lookup"><span data-stu-id="8ec99-158">Record members could be customized using a new `RecordMember(bool)` attribute that could be used to either include or exclude members.</span></span> <span data-ttu-id="8ec99-159">記錄成員預設會 `initonly`，而且會根據記錄成員自動產生類別的相等。</span><span class="sxs-lookup"><span data-stu-id="8ec99-159">Record members would be `initonly` by default and equality would be autogenerated for the class based on the record members.</span></span> <span data-ttu-id="8ec99-160">在任何時間點，只要在來源中宣告這些成員的行為，就可以進行自訂。</span><span class="sxs-lookup"><span data-stu-id="8ec99-160">At any point the behavior of these members could be customized simply by declaring them in source.</span></span> <span data-ttu-id="8ec99-161">使用者撰寫的執行會取代所有模式使用方式中的預設實值。</span><span class="sxs-lookup"><span data-stu-id="8ec99-161">The user-written implementation would replace the default implementation in all pattern usage.</span></span>

<span data-ttu-id="8ec99-162">請注意，繼承面是否相等很複雜，但似乎已在[其他記錄提案](records.md)中獲得適當的解決。</span><span class="sxs-lookup"><span data-stu-id="8ec99-162">Note that equality in the face of inheritance is complex, but seems to have been adequately solved in the [other records proposal](records.md).</span></span>

## <a name="primary-constructors"></a><span data-ttu-id="8ec99-163">主要的函式</span><span class="sxs-lookup"><span data-stu-id="8ec99-163">Primary constructors</span></span>

<span data-ttu-id="8ec99-164">先前的記錄提案也在型別本身加入了參數清單的新語法，例如</span><span class="sxs-lookup"><span data-stu-id="8ec99-164">Previous record proposal have also included a new syntax for a parameter list on the type itself, e.g.</span></span>

```C#
class Point(int X, int Y);
```

<span data-ttu-id="8ec99-165">在新的設計中，參數清單會是一個正C#交的功能，可與記錄完全整合。</span><span class="sxs-lookup"><span data-stu-id="8ec99-165">In the new design, the parameter list would be an orthogonal C# feature, which could be cleanly integrated with records.</span></span> <span data-ttu-id="8ec99-166">如果記錄中包含主要的函式，它會有新的預設值，就像公用欄位和自動屬性：主要的函式中的參數會用來產生具有相同名稱的公用記錄成員屬性。</span><span class="sxs-lookup"><span data-stu-id="8ec99-166">If a primary constructor is included in a record, it would have new defaults, just like public fields and auto-properties: the parameters in the primary constructor would be used to generate public record-member properties with the same name.</span></span> <span data-ttu-id="8ec99-167">此外，主要的函式現在可以用來自動產生解構函式。</span><span class="sxs-lookup"><span data-stu-id="8ec99-167">In addition, the primary constructor could now be used to auto-generate a deconstructor.</span></span>

<span data-ttu-id="8ec99-168">例如，具有主要函式的下列記錄</span><span class="sxs-lookup"><span data-stu-id="8ec99-168">For example, the following record with a primary constructor</span></span>

```C#
data class Point(int X, int Y);
```

<span data-ttu-id="8ec99-169">相當於</span><span class="sxs-lookup"><span data-stu-id="8ec99-169">would be equivalent to</span></span>

```C#
data class Point
{
    public int X { get; }
    public int Y { get; }

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }

    public void Deconstruct(out int X, out int Y)
    {
        X = this.X;
        Y = this.Y;
    }
}
```

<span data-ttu-id="8ec99-170">最後產生的會是</span><span class="sxs-lookup"><span data-stu-id="8ec99-170">and the final generation of the above would be</span></span>

```C#
class Point
{
    public initonly int X { get; }
    public initonly int Y { get; }

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }

    protected Point(Point other)
    : this(other.X, other.Y)
    { }

    [WithConstructor]
    public virtual Point With() => new Point(this);

    public void Deconstruct(out int X, out int Y)
    {
        X = this.X;
        Y = this.Y;
    }

    // Generated equality
}
```

<span data-ttu-id="8ec99-171">請注意，我們已使用主要的「函式」來考慮資料類別的另一項資訊：而不是在產生的受保護函式內設定主要欄位，而是委派給主要的函式。</span><span class="sxs-lookup"><span data-stu-id="8ec99-171">Note that we've taken one other piece of information into account for a data class with a primary constructor: instead of setting the primary fields inside the generated protected constructor, we delegate to the primary constructor.</span></span> <span data-ttu-id="8ec99-172">如果 Point 類別具有另一個非主要的記錄成員，例如</span><span class="sxs-lookup"><span data-stu-id="8ec99-172">If the Point class had another non-primary record member, e.g.</span></span>

```C#
data class Point(int X, int Y)
{
    public int Z { get; }
}
```

<span data-ttu-id="8ec99-173">然後，這會變更產生的受保護的函式，如下所示：</span><span class="sxs-lookup"><span data-stu-id="8ec99-173">then that would change the generated protected constructor as follows:</span></span>

```C#
class Point
{
    // ...
    protected Point(Point other)
    : this(other.X, other.Y)
    {
        Z = other.Z;
    }
    // ...
}
```

<span data-ttu-id="8ec99-174">值得注意的是，這並不會回答使用主要的函式來進行記錄的繼承。</span><span class="sxs-lookup"><span data-stu-id="8ec99-174">Notably, this doesn't answer what to do about inheritance of records with primary constructors.</span></span> <span data-ttu-id="8ec99-175">例如，</span><span class="sxs-lookup"><span data-stu-id="8ec99-175">For instance,</span></span>

```C#
data class A(int X, int Y);
data class B(int X, int Y, int Z) : A;
```

<span data-ttu-id="8ec99-176">更明確的方法可能會要求使用基底清單來提供參數清單，而不是以任意方式解析，例如</span><span class="sxs-lookup"><span data-stu-id="8ec99-176">Rather than resolving in an arbitrary manner, a more explicit approach could require that a parameter list be provided with the base list, e.g.</span></span>

```C#
data class A(int X, int Y);
data class B(int X, int Y, int Z) : A(X, Y);
```

<span data-ttu-id="8ec99-177">然後，基底清單中的參數清單會套用至所產生之主要函式中的 `base` 呼叫：</span><span class="sxs-lookup"><span data-stu-id="8ec99-177">The parameter list in the base list would then be applied to a `base` call in the generated primary constructor:</span></span>

```C#
class B
{
    // ..
    public B(int x, int y, int z)
    : base(x, y)
    // ..
}
```

<span data-ttu-id="8ec99-178">對於主要的函式在記錄外部可能代表的意義，這項功能仍然開放給進一步的提案。</span><span class="sxs-lookup"><span data-stu-id="8ec99-178">As for what a primary constructor could mean outside of a record, that is still open to further proposal.</span></span>
