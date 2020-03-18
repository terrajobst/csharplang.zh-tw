---
ms.openlocfilehash: 77c9ffda3a59cc5f3dcc3ec9848600c6c5e03eed
ms.sourcegitcommit: 27487fa0294f4cdb7e5f2478884149e05314fd8a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2019
ms.locfileid: "79484969"
---
# <a name="primary-constructors"></a>主要的函式

* [x] 提議
* [] 原型：未啟動
* [] 執行：未啟動
* [] 規格：未啟動

## <a name="summary"></a>摘要
[summary]: #summary

類別可以有參數清單，當它們這麼做時，其基類規格可以有引數清單。
主要的函式參數位於整個類別宣告的範圍內，而且如果是由函式成員或匿名函式所捕捉，則會將它們儲存為類別中的私用欄位。

## <a name="motivation"></a>動機
[motivation]: #motivation

在程式初始化程式碼中，通常會有很多的重複使用。 在一般情況下，會多次提及給定的資料 `x`：

- 作為私用欄位 `_x`
- 做為參數 `x` 至函數
- 在從函式的參數中，欄位的指派 `_x = x;`
- 做為屬性 `X`
- 在屬性 setter 中 `x = value;`
- 在屬性 getter 中 `return x;`

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

對於不需要驗證或計算的屬性，可以使用 auto 屬性來減少冗長，因而減少宣告屬性之明確支援欄位的需求。 但是，如果您的屬性所需的邏輯超出自動屬性所提供的數目，上述就是您的最佳選擇。

主要的函式會改為將方法引數直接放在整個類別的範圍中，藉此減少額外負荷，因此回避需要明確宣告支援欄位。 因此，上述範例會變成：

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

在此範例中，主要的函式會將 `x` 的命名實體數目減少為三到二，回避 `_x` 支援欄位。 它會移除三個成員宣告中的兩個（僅保留屬性宣告本身），並減少從八到五個 `x`/`_x``X` /的總次數。


## <a name="detailed-design"></a>詳細設計
[design]: #detailed-design

類別可以有參數清單：

``` c#
public class C(int i, string s)
{
    ...
}
```

參數清單會為類別隱含宣告函式，並具有與類別本身相同的存取範圍。

``` c#
new C(5, "Hello");
```

主要的函式參數在整個類別主體的範圍內。 如果這些函式是由函式成員或匿名函式所捕捉，它們就會在類別中儲存為私用欄位。 如果它們只在初始化期間使用，則不會儲存在物件中。

``` c#
public class C(int i, string s)
{
    int[] a = new int[i]; // i not captured
    public int S => s;    // s captured
}
```

如果具有主要函式的類別具有基類規格，則可以有引數清單。 這會當做隱含宣告之函式的 `base(...)` 初始化運算式的引數清單。 如果未提供引數清單，則會假設為空的。

``` c#
public class C(int i, string s) : B(s)
{
    ...
}
```
類別也可以有明確定義的函式，但這些都必須使用 `this(...)` 的初始化運算式。 這可確保在結構化新的實例時，一律會呼叫主要的函式。

類別主體中的所有初始化運算式都會成為所產生之函式中的指派。 這表示，與其他類別不同的是，初始化運算式會在叫用基底函式*之後*執行，而不是在之前。 此外，所產生的類別將會包含指派，以初始化為了儲存成員所捕捉的主要函式參數而產生的任何私用欄位。 這些成員會重寫成使用私用欄位，而不是參數，其方式類似于 lambda 運算式的「閉」。 產生的主要欄位會先初始化，然後以類別中的外觀順序來執行初始化運算式產生的指派。

在上述範例中，類別宣告的效果就如同如下所示重寫：

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

此捕捉的限制與使用 lambda 運算式來捕捉區域變數的方式相似。 比方說，主要的函式中允許 `ref` 和 `out` 參數，但是無法捕捉我的成員主體。


## <a name="drawbacks"></a>缺點
[drawbacks]: #drawbacks

以粗略的重要性排序。

* 提案會使用也已針對位置記錄提議的語法。 如果我們想要這兩項功能，則需要一些住宿。 例如 已建議記錄上的 `data` 修飾詞。
* 結構化物件的配置大小較不明顯，因為編譯器會根據類別的完整文字，判斷是否要配置主要的函式參數的欄位。 這項風險類似于 lambda 運算式的隱含捕獲變數。
* 常見的誘惑（或意外的模式）可能是在多個繼承層級上捕捉「相同」參數，因為它會傳遞至函式鏈，而不是在基類中明確 allotting 受保護的欄位，導致重複的配置物件中的相同資料。 這非常類似于現今使用 auto 屬性覆寫自動屬性的風險。 
* 如先前所述，不會有任何可能通常以「函式主體」表示的其他邏輯位置。 下面的「主要的程式式主體」延伸模組會解決此情況。
* 如提議，執行順序的語義與一般的處理函式稍有不同。 這可能會受到補救，但代價是某些擴充功能提案（特別是「主要的函式主體」）。
* 只有在單一的函式可以指定為主要時，此提案才有作用。
* 沒有任何方法可以擁有類別的個別存取範圍和主要的函式。 比方說，在公用函式全都委派給一個需要的私用「組建-全部」的函式。 如有需要，稍後可能會建議語法。


## <a name="alternatives"></a>替代方案
[alternatives]: #alternatives

視細節而定，完整位置記錄可能是替代的，或可能與主要的函式並存。 在*較少*的案例中，他們可能會有*更多*的縮寫。 這兩者都很有用，但兩者都可能會大材小用，除非它們可以彼此整齊地進行整合。


## <a name="possible-extensions"></a>可能的擴充
[extensions]: #possible-extensions

這些是核心提案的變化或新增專案，可能會與它一起考慮，或在之後的階段中被視為有用。

### <a name="primary-constructor-bodies"></a>主要的函數主體

函式本身通常會包含參數驗證邏輯或其他無法以初始化運算式表示的重要初始化程式碼。

主要的函式可以擴充，以允許語句區塊直接出現在類別主體中。 這些語句會在初始化的指派之間的出現時，插入產生的函式中，因此會使用初始化運算式來執行。 例如：

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

### <a name="initializer-fields-and-initializer-functions"></a>初始化運算式欄位和初始化運算式函式

在具有主要函式的類別中，我們可以考慮不含存取範圍修飾詞的欄位和方法宣告，更像本機變數和區域函式：

* 就像主要的函式參數一樣，只有在函式成員中使用「初始化運算式欄位」時，才會將其捕捉到實際的私用欄位中。
* 「初始化運算式函式」只有在其他函式成員中使用時，才會被視為要捕捉主要的「函式參數」和「初始化運算式」欄位。 如果未捕捉到，可能會以更理想的方式產生，例如區域函式。
* 就像主要的函式參數一樣，它們無法透過成員存取來使用，而只是簡單的名稱。

這可以用於僅與初始化相關的暫存變數和 helper 函式：

``` c#
public class C(string s)
{
    int size = s.Length;             // not captured
    int[] Create() => new int[size]; // not captured
    int[] a = Create();
    ...
}
```

這可能太微妙，特別是因為其他地方沒有存取範圍修飾詞只是表示 `private`。 

### <a name="initializer-statements"></a>初始化運算式語句

上述至延伸模組的根式組合，就是直接允許類別主體中的語句。 這類語句與上面所述的交錯式的函數主體完全相同，不同之處在于它們不需要包含在 `{ }`中。 為了充分發揮效用，"local" 變數和 helper 函式也必須在類別的最上層，以在上述延伸模組「初始化運算式欄位和初始化運算式函式」中探索的方式來表示。


### <a name="member-access"></a>成員存取

核心提案會將主要的函式參數視為只能參考簡單名稱的參數。 另一個替代方式是讓它們被視為私用欄位（也就是具有成員存取權），*即使*有時不會以欄位的形式產生。 這可讓它們在以本機變數加上陰影時被視為 `this.x`，並從不同的實例存取為 `other.x`。

如果套用至「初始化運算式欄位和初始化運算式函式」延伸模組，這也會降低這些與一般私用成員不同的程度。 唯一的差別在於，如果只在初始化期間使用，編譯器就可以自由地從物件中省略它們。

