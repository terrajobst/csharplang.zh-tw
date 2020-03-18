---
ms.openlocfilehash: 5636157ba54e93587847d6f2f7aac2dc675f3112
ms.sourcegitcommit: af27912886f22cda9b98b7769447d85cd9732736
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/09/2019
ms.locfileid: "79484997"
---

# <a name="records-v2"></a>記錄 v2

在過去，我們已將記錄視為能夠使用資料的功能。

「使用資料」是具有許多 facet 的大型群組，因此在隔離的情況下可能會有興趣。 讓我們從今天的記錄範例和部分缺點開始。

比方說，簡單的記錄會定義如下

```C#
public class UserInfo
{
    public string Username { get; set; }
    public string Email { get; set; }
    public bool IsAdmin  { get; set; } = false;
}
```

而且會讀取使用方式

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

這段程式碼有顯著的優點：

1. 定義的版本具有彈性，可以輕鬆地新增或移動屬性
2. 屬性可以依任何順序設定，而初始化中的名稱一律符合存取子
3. 具有預設值的屬性可以直接略過

第一個缺點是屬性現在必須是可變動的。

# <a name="mutability"></a>可變動性

我們C#想要提供一種方法，在物件初始化運算式中設定 `readonly` 成員。
由於某些類型可能不會在此初始化中設計，因此我們也希望它可以加入宣告。

建議的解決方案是新的修飾詞，`initonly`，可套用至屬性和欄位：

```C#
public class UserInfo
{
    public initonly string Username { get; }
    public initonly string Email { get; }
    public initonly bool IsAdmin { get; } = false;
}
```

這是非常簡單的 codegen：我們剛剛設定了 readonly 欄位。
具體而言，降低的屬性看起來會像這樣：

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

CLR 會考慮將 readonly 欄位設定為無法驗證，但不安全。 若要支援更先進的驗證器，請注意下列規則：唯讀欄位只能在 `initonly` 方法內，或在 CLR 堆疊的新物件上修改，而且尚未透過存放區或方法呼叫來發行。

這應該能解決 `UserInfo` 範例中可變動性的許多問題，而不需要複雜或脆弱的發出策略。 不過，不會出現新的問題：使用變更輕鬆地建立物件。

# <a name="with-ing"></a>With-ing

使用不可變性進行程式設計時，對物件進行的變更是藉由使用變更來建立複本來進行，而不是直接在物件上進行變更。 可惜的是，即使使用目前的樣式記錄， C#也沒有任何便利的方式可以在中執行這項操作。 先前已提議針對實作為該功能的記錄，提供某種類型自動產生的「With」方法。 如果我們有這種機制，請務必使用 `initonly` 成員。 為了達成此目的，我們建議您加入類似物件初始化運算式的 `with` 運算式。 範例用法如下所示：

```C#
var userInfo = new UserInfo() 
{
    Username = "andy",
    Email = "angocke@microsoft.com",
    IsAdmin = true
};
var newUserName = userInfo with { Username = "angocke" };
```

產生的 `newUserName` 物件會是 `userInfo`的複本，`Username` 設定為 "angocke"。
`with` 運算式上的 codegen 也會類似物件初始化運算式：會建立新的物件，然後在方法主體中呼叫 `initonly` `Username` setter。

當然，此處的差異在於，所建立的新物件不是簡單的新物件建立，而是原始物件的複本。 為了提供這種功能，我們要求物件必須提供可提供重複物件的 "With 「函式」。 範例 `With` 的函式看起來像這樣：

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

值得注意的是，`with` 運算式會設定 `initonly` 成員，就像物件初始化運算式一樣，因此若要支援驗證，我們必須確保在設定 `initonly` 成員之前，無法發行物件。 若要強制執行此動作，`WithConstructor` 屬性（或對等的語法）將會對方法強制執行新的規則：所有傳回語句都必須直接包含物件建立運算式，可能是物件初始化運算式。

如果 `With` 的處理常式需要驗證，則使用者可能會引進用來進行驗證的函式，例如

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

與 `With` 相關聯的最後一項複雜度是繼承。 如果您的記錄是可延伸的，您將需要為子類別提供新的 `With`。 這可以透過下列方式來達成：

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

請注意這裡的另一個複雜度：若要以衍生型別覆寫 `With` 的處理函式，語言也必須支援覆寫中的協變數傳回類型。
[這裡](https://github.com/dotnet/csharplang/blob/725763343ad44a9251b03814e6897d87fe553769/proposals/covariant-returns.md)已有這項功能的個別提案。

**缺陷**

- `WithConstructor`s 中的所有 return 語句都包含新的物件運算式，因此受到限制。
  流程分析可能會降低這種情況，以確保新的物件不會將方法 escape
- 透過編譯器訣竅支援覆寫中的變異數，將需要存根方法，這會隨著繼承深度而增加 quadratically。 Stub 方法的需求是因為覆寫簽章完全相符的執行時間需求。 如果執行時間需求已放寬，則完全不需要存根方法。
- 使用表單的連鎖的函式，`Type(Type original)` 有效地保留該模式的使用方式。 因為這些函式具有唯一的語義，而且無法重新命名，所以可能會受到限制。


## <a name="wrapping-it-all-up-records"></a>全部包裝：記錄

上述功能可實現之前非常難以的程式設計樣式。 但是，即使有了新的功能，也可能會有相當詳細的資訊，而且容易出錯來為所有專案加上附注 還有幾個專案，像是 Equals 和 GetHashCode，現在已經可以撰寫，這只是費力。
此外，在這些新的基本專案之上執行相等的重大缺點是，結構化相等性應該隨著新資料的資料類型而變更，但是在手動處理時，可能會導致這些專案無法同步。

因此，建議C#支援記錄的新語法，而不是提供新的功能，而是用來設定預設值以及產生設計用於記錄的程式碼。 範例語法看起來會像這樣

```C#
data class UserInfo
{
    public string Username { get; }
    public string Email { get; }
    public bool IsAdmin { get; } = false;
}
```

針對此類別產生的程式碼會將所有公用欄位和自動屬性視為記錄的結構成員。 您可以使用可用於包含或排除成員的新 `RecordMember(bool)` 屬性來自訂記錄成員。 記錄成員預設會 `initonly`，而且會根據記錄成員自動產生類別的相等。 在任何時間點，只要在來源中宣告這些成員的行為，就可以進行自訂。 使用者撰寫的執行會取代所有模式使用方式中的預設實值。

請注意，繼承面是否相等很複雜，但似乎已在[其他記錄提案](records.md)中獲得適當的解決。

## <a name="primary-constructors"></a>主要的函式

先前的記錄提案也在型別本身加入了參數清單的新語法，例如

```C#
class Point(int X, int Y);
```

在新的設計中，參數清單會是一個正C#交的功能，可與記錄完全整合。 如果記錄中包含主要的函式，它會有新的預設值，就像公用欄位和自動屬性：主要的函式中的參數會用來產生具有相同名稱的公用記錄成員屬性。 此外，主要的函式現在可以用來自動產生解構函式。

例如，具有主要函式的下列記錄

```C#
data class Point(int X, int Y);
```

相當於

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

最後產生的會是

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

請注意，我們已使用主要的「函式」來考慮資料類別的另一項資訊：而不是在產生的受保護函式內設定主要欄位，而是委派給主要的函式。 如果 Point 類別具有另一個非主要的記錄成員，例如

```C#
data class Point(int X, int Y)
{
    public int Z { get; }
}
```

然後，這會變更產生的受保護的函式，如下所示：

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

值得注意的是，這並不會回答使用主要的函式來進行記錄的繼承。 例如，

```C#
data class A(int X, int Y);
data class B(int X, int Y, int Z) : A;
```

更明確的方法可能會要求使用基底清單來提供參數清單，而不是以任意方式解析，例如

```C#
data class A(int X, int Y);
data class B(int X, int Y, int Z) : A(X, Y);
```

然後，基底清單中的參數清單會套用至所產生之主要函式中的 `base` 呼叫：

```C#
class B
{
    // ..
    public B(int x, int y, int z)
    : base(x, y)
    // ..
}
```

對於主要的函式在記錄外部可能代表的意義，這項功能仍然開放給進一步的提案。
