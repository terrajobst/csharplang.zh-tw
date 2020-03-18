---
ms.openlocfilehash: b0d0fa70d90f7493c6c23be576155a77cec36cf8
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/14/2020
ms.locfileid: "79485179"
---
# <a name="default-interface-methods"></a>預設介面方法

* [x] 提議
* [] 原型：[進行中](https://github.com/dotnet/roslyn/blob/master/docs/features/DefaultInterfaceImplementation.md)
* [] 執行：無
* [] 規格：進行中，下方

## <a name="summary"></a>摘要
[summary]: #summary

新增_虛擬擴充方法_的支援：介面中具具體執行的方法。 執行此類介面的類別或結構，必須具有介面方法（由類別或結構所實，或繼承自其基類或介面）的單一_最特定_執行。 虛擬擴充方法可讓 API 作者在未來的版本中，將方法新增至介面，而不會中斷來源或二進位檔與該介面的現有實現的相容性。

這些類似于 JAVA 的「[預設方法](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)」。

（根據可能的執行技術）此功能需要 CLI/CLR 中的對應支援。 利用這項功能的程式無法在舊版平臺上執行。

## <a name="motivation"></a>動機
[motivation]: #motivation

這項功能的主要動機是

- 預設介面方法可讓 API 作者將方法加入至未來版本中的介面，而不中斷來源或二進位檔與該介面的現有實現的相容性。
- 此功能可C#讓與以[Android （JAVA）](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)和[iOS （Swift）](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID267)為目標的 api 交互操作，以支援類似的功能。
- 結果是，新增預設介面執行會提供「特性」語言功能的元素（<https://en.wikipedia.org/wiki/Trait_(computer_programming)>）。 特性已證明是強大的程式設計技巧（<http://scg.unibe.ch/archive/papers/Scha03aTraits.pdf>）。

## <a name="detailed-design"></a>詳細設計
[design]: #detailed-design

介面的語法已擴充為允許

- 宣告常數、運算子、靜態函數和巢狀型別的成員宣告;
- 方法或索引子、屬性或事件存取子的*主體*（也就是「預設」實作為）;
- 宣告靜態欄位、方法、屬性、索引子和事件的成員宣告;
- 使用明確介面執行語法的成員宣告;和
- 明確存取修飾詞（預設存取是 `public`）。

具有主體的成員允許介面在不提供覆寫實作的類別和結構中，為方法提供「預設」的執行。

介面可能不包含實例狀態。 雖然現在允許靜態欄位，但介面中不允許實例欄位。 介面中不支援實例 auto 屬性，因為它們會隱含宣告隱藏的欄位。

靜態和私用方法允許用來執行介面公用 API 的實用重構和程式碼組織。

介面中的方法覆寫必須使用明確的介面執行語法。

在以*variance_annotation*宣告的型別參數範圍內，宣告類別型別、結構型別或列舉型別是錯誤的。  例如，下列 `C` 的宣告是錯誤。

```csharp
interface IOuter<out T>
{
    class C { } // error: class declaration within the scope of variant type parameter 'T'
}
```

### <a name="concrete-methods-in-interfaces"></a>介面中的具體方法

這項功能最簡單的形式是在介面中宣告*具體的方法*，這是具有主體的方法。

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
```

執行此介面的類別不需要實作為其具體的方法。

```csharp
class C : IA { } // OK

IA i = new C();
i.M(); // prints "IA.M"
```

類別 `C` 中 `IA.M` 的最終覆寫是 `M` 在 `IA`中宣告的實體方法。 請注意，類別不會繼承其介面的成員;這項功能不會變更：

```csharp
new C().M(); // error: class 'C' does not contain a member 'M'
```

在介面的實例成員內，`this` 具有封閉式介面的型別。

### <a name="modifiers-in-interfaces"></a>介面中的修飾詞

介面的語法會寬鬆，以允許其成員上的修飾詞。 允許下列各項： `private`、`protected`、`internal`、`public`、`virtual`、`abstract`、`sealed`、`static`、`extern`和 `partial`。

> ***TODO***：檢查有哪些其他修飾詞存在。

除非使用 `sealed` 或 `private` 修飾詞，否則宣告包含主體的介面成員是 `virtual` 成員。 `virtual` 修飾詞可以在函式成員上使用，否則會隱含地 `virtual`。 同樣地，雖然 `abstract` 在沒有主體的介面成員上是預設值，但可以明確指定該修飾詞。 您可以使用 `sealed` 關鍵字來宣告非虛擬成員。

介面的 `private` 或 `sealed` 函式成員沒有主體時，就會發生錯誤。 `private` 的函式成員可能沒有修飾詞 `sealed`。

存取修飾詞可用於允許之所有類型成員的介面成員上。 存取層級 `public` 是預設值，但可以明確指定。

> ***開啟問題：*** 我們需要指定存取修飾詞的確切意義，例如 `protected` 和 `internal`，以及哪些宣告會執行和不覆寫它們（在衍生介面中）或加以執行（在執行介面的類別中）。

介面可以宣告 `static` 成員，包括巢狀型別、方法、索引子、屬性、事件和靜態的函式。 所有介面成員的預設存取層級都是 `public`。

介面可能不會宣告實例的函式、析構函式或欄位。

> ***已關閉的問題：*** 介面中是否允許運算子宣告？ 可能不是轉換運算子，但其他則是什麼？ ***決策***：除了轉換、相等和不等比較運算子*之外*，允許運算子。

> ***已關閉的問題：*** 在隱藏基底介面成員的介面成員宣告上，應該允許 `new` 嗎？ ***決策***：是。

> ***已關閉的問題：*** 目前不允許在介面或其成員上 `partial`。 這需要個別的提案。 ***決策***：是。 <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>

### <a name="overrides-in-interfaces"></a>介面中的覆寫

覆寫宣告（也就是包含 `override` 修飾詞的宣告）可讓程式設計師在編譯器或執行時間不會找到一個的介面中，提供最特定的虛擬成員執行方式。 它也允許從超級介面將抽象成員轉換成衍生介面中的預設成員。 藉由使用介面名稱限定宣告（在此情況下不允許存取修飾詞），允許覆寫宣告*明確*覆寫特定基底介面方法。 不允許隱含覆寫。

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    override void IA.M() { WriteLine("IB.M"); } // explicitly named
}
interface IC : IA
{
    override void M() { WriteLine("IC.M"); } // implicitly named
}
```

介面中的覆寫宣告可能不會 `sealed`宣告。

公用 `virtual` 介面中的函式成員可能會在衍生介面中明確覆寫（藉由在覆寫宣告中以原本宣告方法的介面類別型來限定名稱，以及省略存取修飾詞）。

介面中的 `virtual` 函式成員只能在衍生介面中明確覆寫（非隱含），而不 `public` 的成員只能在類別或結構中明確實作為（非隱含）。 不論是哪一種情況，覆寫或已執行的成員都必須可在覆寫時*存取*。

### <a name="reabstraction"></a>Reabstraction

在介面中宣告的虛擬（具體）方法可能會覆寫為衍生介面中的抽象。

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    abstract void IA.M();
}
class C : IB { } // error: class 'C' does not implement 'IA.M'.
```

`IB.M` 的宣告中不需要 `abstract` 修飾詞（這是介面中的預設值），但在覆寫宣告中可能是明確的做法。

這在衍生介面中很有用，因為方法的預設實作為不適當，而更適當的執行應由實作為類別來提供。

> ***開啟問題：*** 是否應允許 reabstraction？

### <a name="the-most-specific-override-rule"></a>最特定的覆寫規則

在類型或其直接和間接介面中出現的覆寫之間，我們要求每個介面和類別都有*最明確*的覆寫。 *最特定*的覆寫是唯一的覆寫，比其他每個覆寫更明確。 如果沒有覆寫，成員本身會被視為最明確的覆寫。

一個覆寫 `M1` 被視為比另一個覆寫*更明確*`M2` 如果 `M1` 是在類型 `T1`上宣告，`M2` 會在類型 `T2`上宣告，而且兩者都是

1. `T1` 包含其直接或間接介面之間的 `T2`，或
2. `T2` 是介面類別型，但 `T1` 不是介面類別型。

例如：

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    void IA.M() { WriteLine("IB.M"); }
}
interface IC : IA
{
    void IA.M() { WriteLine("IC.M"); }
}
interface ID : IB, IC { } // error: no most specific override for 'IA.M'
abstract class C : IB, IC { } // error: no most specific override for 'IA.M'
abstract class D : IA, IB, IC // ok
{
    public abstract void M();
}

```

最特定的覆寫規則可確保程式設計人員在發生衝突時明確解決衝突（也就是菱形繼承所產生的多義性）。

因為我們在介面中支援明確的抽象覆寫，所以我們也可以在類別中這麼做

```csharp
abstract class E : IA, IB, IC // ok
{
    abstract void IA.M();
}
```

> ***開啟問題***：我們是否應該在類別中支援明確的介面抽象覆寫？

此外，如果在類別宣告中，某些介面方法的最特定覆寫是在介面中宣告的抽象覆寫，就會發生錯誤。 這是使用新術語先重新開機的現有規則。

```csharp
interface IF
{
    void M();
}
abstract class F : IF { } // error: 'F' does not implement 'IF.M'
```

在介面中宣告的虛擬屬性可以在某個介面中擁有最明確 `get` 的覆寫，並在不同的介面中，針對其 `set` 存取子使用最特定的覆寫。 這會被視為違反*最特定*的覆寫規則。

### <a name="static-and-private-methods"></a>`static` 和 `private` 方法

因為介面現在可能包含可執行檔程式碼，所以將通用程式碼抽象成私用和靜態方法會很有用。 我們現在會在介面中允許這些。

> ***已關閉的問題***：我們是否應該支援私用方法？ 我們是否應該支援靜態方法？ **決策：是**

> ***開啟問題***：我們是否允許介面方法 `protected` 或 `internal` 或其他存取權？ 若是如此，什麼是語義？ 它們預設是否 `virtual`？ 若是如此，是否有辦法讓它們成為非虛擬？

> ***開啟問題***：如果我們支援靜態方法，我們是否支援（靜態）運算子？

### <a name="base-interface-invocations"></a>基底介面調用

類型中的程式碼，衍生自具有預設方法的介面，可以明確地叫用該介面的「基底」實值。

```csharp
interface I0
{
   void M() { Console.WriteLine("I0"); }
}
interface I1 : I0
{
   override void M() { Console.WriteLine("I1"); }
}
interface I2 : I0
{
   override void M() { Console.WriteLine("I2"); }
}
interface I3 : I1, I2
{
   // an explicit override that invoke's a base interface's default method
   void I0.M() { I2.base.M(); }
}

```

實例（非靜態）方法可以使用 `base(Type).M`的語法來命名，以叫用直接基底介面 nonvirtually 中可存取的實例方法的執行。 當因為鑽石繼承而需要提供覆寫時，會委派給一個特定的基底執行來解決此問題，這項功能就很有用。

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    override void IA.M() { WriteLine("IB.M"); }
}
interface IC : IA
{
    override void IA.M() { WriteLine("IC.M"); }
}

class D : IA, IB, IC
{
    void IA.M() { base(IB).M(); }
}
```

使用語法 `base(Type).M`來存取 `virtual` 或 `abstract` 成員時，`Type` 必須包含 `M`的唯一*最特定覆寫*。

### <a name="binding-base-clauses"></a>系結基底子句

介面現在包含類型。  這些類型可在基底子句中當做基底介面使用。  系結基底子句時，我們可能需要知道一組基底介面來系結這些類型（例如，在其中查閱，以及解析受保護的存取）。  因此，介面的基底子句的意義是迴圈定義的。  為了中斷迴圈，我們新增了一個新的語言規則，其對應至已適用于類別的類似規則。

判斷介面*interface_base*的意義時，會暫時假設基底介面是空的。 這可確保基底子句的意義無法以遞迴方式相依于其本身。 

**我們使用的規則如下：**

「當類別 B 衍生自類別 A 時，就會發生編譯時期錯誤，而會相依于 B。類別會**直接相依于**其直接基類（如果有的話），而**直接取決於**立即嵌套的~~**類別**~~（如果有的話）。 根據這個定義，類別所相依的一組完整~~**類別**~~是**直接相依于**關聯性的自反和可轉移關閉。」

這是直接或間接繼承自本身的介面編譯時期錯誤。
介面的**基底介面**是明確基底介面和其基底介面。 換句話說，基底介面集是明確基底介面、其明確基底介面等的完整可轉移關閉。

**我們會調整它們，如下所示：**

當類別 B 衍生自類別 A 時，就會發生編譯時期錯誤，而會相依于 B。類別會**直接相依于**其直接基類（如果有的話），而**直接取決於**立即嵌套的 _**型**_ 別（如果有的話）。

當介面 IB 擴充介面 IA 時，就會發生編譯時期錯誤，因此 IA 會依賴 IB。 介面會**直接相依于**其直接基底介面（如果有的話），而**直接取決於**立即嵌套的類型（如果有的話）。

根據這些定義，類型所相依的完整**類型**集合是**直接相依于**關聯性的自反和可轉移關閉。

### <a name="effect-on-existing-programs"></a>對現有程式的影響

此處提供的規則主要是對現有程式的意義沒有影響。

範例 1：

```csharp
interface IA
{
    void M();
}
class C: IA // Error: IA.M has no concrete most specific override in C
{
    public static void M() { } // method unrelated to 'IA.M' because static
}
```

範例 2：

```csharp
interface IA
{
    void M();
}
class Base: IA
{
    void IA.M() { }
}
class Derived: Base, IA // OK, all interface members have a concrete most specific override
{
    private void M() { } // method unrelated to 'IA.M' because private
}
```

相同的規則會提供類似的結果，與包含預設介面方法的類似情況有關：

```csharp
interface IA
{
    void M() { }
}
class Derived: IA // OK, all interface members have a concrete most specific override
{
    private void M() { } // method unrelated to 'IA.M' because private
}
```

> ***已關閉的問題***：確認這是規格的預期結果。 **決策：是**

### <a name="runtime-method-resolution"></a>執行時間方法解析

> ***已關閉的問題：*** 在介面預設方法的臉部中，規格應該會描述執行時間方法解析演算法。 我們需要確保此語義與語言語義一致，例如哪些宣告的方法會執行，而且不會覆寫或執行 `internal` 方法。

### <a name="clr-support-api"></a>CLR 支援 API

為了讓編譯器能夠在編譯支援這項功能的執行時間時，偵測到這類執行時間的程式庫會透過 <https://github.com/dotnet/corefx/issues/17116>中所討論的 API 來公告該事實。 我們新增

```csharp
namespace System.Runtime.CompilerServices
{
    public static class RuntimeFeature
    {
        // Presence of the field indicates runtime support
        public const string DefaultInterfaceImplementation = nameof(DefaultInterfaceImplementation);
    }
}
```

> ***開啟問題***：這是*CLR*功能的最佳名稱嗎？ CLR 功能不只是這麼做（例如放寬保護條件約束、支援介面中的覆寫等等）。 也許它應該稱為「介面中的具體方法」或「特性」之類的東西？

### <a name="further-areas-to-be-specified"></a>要指定的進一步區域

- [] 將預設的介面方法和覆寫加入現有的介面，以將造成的來源和二進位相容性的類型進行類別目錄的分類非常有用。

## <a name="drawbacks"></a>缺點
[drawbacks]: #drawbacks

此提案需要 CLR 規格的協調更新（以支援介面和方法解析中的具體方法）。 因此，它相當「昂貴」，而且可能會與其他也預期需要 CLR 變更的功能相結合。

## <a name="alternatives"></a>替代方案
[alternatives]: #alternatives

無。

## <a name="unresolved-questions"></a>未解決的問題
[unresolved]: #unresolved-questions

- 在上述的提案中，會提出 Open 問題。
- 另請參閱 <https://github.com/dotnet/csharplang/issues/406> 以取得未解決問題的清單。
- 詳細規格必須描述在執行時間用來選取要叫用之精確方法的解決機制。
- 新編譯器所產生並由舊版編譯器所使用之中繼資料的互動，必須詳細地加以處理。 例如，我們需要確保使用的中繼資料標記法不會在介面中加入預設的實值，以在舊版編譯器編譯時中斷執行該介面的現有類別。 這可能會影響我們可以使用的中繼資料標記法。
- 設計必須考慮與其他語言的互通性，以及其他語言的現有編譯器。

## <a name="resolved-questions"></a>解決的問題

### <a name="abstract-override"></a>抽象覆寫

較舊的草稿規格包含「reabstract」繼承方法的能力：

```csharp
interface IA
{
    void M();
}
interface IB : IA
{
    override void M() { }
}
interface IC : IB
{
    override void M(); // make it abstract again
}
```

我在2017-03-20 的注意事項中顯示我們決定不允許這種情況。 不過，它至少有兩個使用案例：

1. JAVA Api，這項功能的某些使用者希望能夠互通，這取決於此工具。
2. 透過這種方式設計*特性*的優勢。 Reabstraction 是「特性」語言功能（ https://en.wikipedia.org/wiki/Trait_(computer_programming))的其中一個元素。 類別允許下列各項：

```csharp
public abstract class Base
{
    public abstract void M();
}
public abstract class A : Base
{
    public override void M() { }
}
public abstract class B : A
{
    public override abstract void M(); // reabstract Base.M
}
```

不幸的是，除非允許，否則無法將此程式碼重構為一組介面（特性）。 根據*貪婪的 Jared 準則*，它應該是允許的。

> ***已關閉的問題：*** 是否應允許 reabstraction？ 肯定我的記事錯誤。 [LDM 附注](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md)指出介面中允許 reabstraction。 不在類別中。

### <a name="virtual-modifier-vs-sealed-modifier"></a>虛擬修飾詞 vs Sealed 修飾詞

從[Aleksey Tsingauz](https://github.com/AlekseyTs)：

> 我們決定在介面成員上允許明確陳述的修飾詞，除非有理由不允許其中一部分。 這在虛擬修飾詞方面有一個有趣的問題。 在具有預設執行的成員上是否需要？
>
> 我們可以說：
>
> - 如果沒有任何執行中，且未指定虛擬和，則會假設成員是抽象的。
> - 如果有一個實作為，而且沒有指定抽象，也沒有密封，則我們假設成員是虛擬的。
> - 必須有 sealed 修飾詞，才能讓方法既不是虛擬，也不是抽象的。
>
> 或者，我們也可以說虛擬成員需要虛擬修飾詞。 也就是，如果有不是以虛擬修飾詞明確標記的「執行」成員，它既不是虛擬，也不是抽象的。 當方法從類別移至介面時，這個方法可能會提供更好的體驗：
>
> - 抽象方法會保持抽象。
> - 虛擬方法會維持虛擬的狀態。
> - 沒有任何修飾詞的方法不會維持虛擬和抽象。
> - sealed 修飾詞不能套用至非覆寫的方法。
>
> 你覺得怎麼樣？

> ***已關閉的問題：*** 具體的方法（搭配實作為）是否應隱含 `virtual`？ 肯定

***決策：*** 在 LDM 2017-04-05 中進行：

1. 非虛擬應該透過 `sealed` 或 `private`明確表示。
2. `sealed` 是讓介面實例成員成為非虛擬主體的關鍵字
3. 我們想要允許介面中的所有修飾詞  
4. 介面成員的預設存取範圍是公用的，包括巢狀型別
5. 介面中的私用函式成員會隱含地密封，而且不允許 `sealed`。
6. 私用類別（在介面中）是允許的，而且可以密封，也就是以 sealed 的類別意義來密封。
7. 缺少良好的提案，介面或其成員上仍然不允許部分。

### <a name="binary-compatibility-1"></a>二進位相容性1

當程式庫提供預設的實作為時

```csharp
interface I1
{
    void M() { Impl1 }
}
interface I2 : I1
{
}
class C : I2
{
}
```

我們瞭解 `C` 中的 `I1.M` 的執行是 `I1.M`。 如果包含 `I2` 的元件依下列方式變更並重新編譯，該怎麼辦

```csharp
interface I2 : I1
{
    override void M() { Impl2 }
}
```

但是 `C` 不會重新編譯。 當程式執行時，會發生什麼事？ `(C as I1).M()` 的調用

1. 執行 `I1.M`
2. 執行 `I2.M`
3. 擲回某種類型的執行階段錯誤

***決策：*** 設為2017-04-11： `I2.M`執行，這是在執行時間明確的最明確覆寫。

### <a name="event-accessors-closed"></a>事件存取子（已關閉）

> ***已關閉的問題：*** 事件是否可以覆寫為 "分段"？

請考慮下列情況：

```csharp
public interface I1
{
    event T e1;
}
public interface I2 : I1
{
    override event T
    {
        add { }
        // error: "remove" accessor missing
    }
}
```

不允許此事件的「部分」執行，因為在類別中，事件宣告的語法不允許只有一個存取子。必須同時提供（或兩者皆非）。 您可以藉由允許語法中的抽象 remove 存取子在不存在主體時隱含抽象化，來完成相同的動作：

```csharp
public interface I1
{
    event T e1;
}
public interface I2 : I1
{
    override event T
    {
        add { }
        remove; // implicitly abstract
    }
}
```

請注意，*這是新的（建議的）語法*。 在目前的文法中，事件存取子具有強制性主體。

> ***已關閉的問題：*** 事件存取子是否可以藉由省略主體（隱含）來抽象化，類似于介面和屬性存取子中的方法在省略主體時的抽象方式？

***決策：*** （2017-04-18）否，事件宣告需要兩個具體的存取子（或兩者皆非）。

### <a name="reabstraction-in-a-class-closed"></a>在類別中 Reabstraction （已關閉）

***已關閉的問題：*** 我們應該確認這是允許的（否則新增預設的實作為中斷性變更）：

```csharp
interface I1
{
    void M() { }
}
abstract class C : I1
{
    public abstract void M(); // implement I1.M with an abstract method in C
}
```

***決策：*** （2017-04-18）是，將主體新增至介面成員宣告不應中斷 C。

### <a name="sealed-override-closed"></a>密封覆寫（已關閉）

先前的問題隱含假設 `sealed` 修飾詞可以套用至介面中的 `override`。 這會與草稿規格相抵觸。 是否要允許密封覆寫？ 應考慮密封的來源和二進位相容性效果。

> ***已關閉的問題：*** 我們是否允許密封覆寫？

***決策：*** （2017-04-18）在介面的覆寫上不允許 `sealed`。 在介面成員上唯一使用 `sealed`，是在其初始宣告中使其成為非虛擬的。

### <a name="diamond-inheritance-and-classes-closed"></a>菱形繼承和類別（已關閉）

提議的草稿優先于菱形繼承案例中的介面覆寫：

> 我們要求每個介面和類別在類型中顯示的覆寫和其直接和間接介面中，都有*最明確*的覆寫。 *最特定*的覆寫是唯一的覆寫，比其他每個覆寫更明確。 如果沒有覆寫，方法本身會被視為最明確的覆寫。
>
> 一個覆寫 `M1` 被視為比另一個覆寫*更明確*`M2` 如果 `M1` 是在類型 `T1`上宣告，`M2` 會在類型 `T2`上宣告，而且兩者都是
>
> 1. `T1` 包含其直接或間接介面之間的 `T2`，或
> 2. `T2` 是介面類別型，但 `T1` 不是介面類別型。

案例如下

```csharp
interface IA
{
    void M();
}
interface IB : IA
{
    override void M() { WriteLine("IB"); }
}
class Base : IA
{
    void IA.M() { WriteLine("Base"); }
}
class Derived : Base, IB // allowed?
{
    static void Main()
    {
        Ia a = new Derived();
        a.M();           // what does it do?
    }
}
```

我們應該確認此行為（或其他決定）

> ***已關閉的問題：*** 確認適用于*大部分特定覆寫*的草稿規格，因為它會套用至混合類別和介面（類別的優先順序高於介面）。 請參閱＜<https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#diamonds-with-classes>＞。

### <a name="interface-methods-vs-structs-closed"></a>介面方法 vs 結構（已關閉）

預設介面方法與結構之間有一些不幸的互動。

```csharp
interface IA
{
    public void M() { }
}
struct S : IA
{
}
```

請注意，介面成員不會被繼承：

```csharp
var s = default(S);
s.M(); // error: 'S' does not contain a member 'M'
```

因此，用戶端必須將結構方塊框為叫用介面方法

```csharp
IA s = default(S); // an S, boxed
s.M(); // ok
```

以這種方式進行的裝箱，會使 `struct` 類型的主要優點失效。 此外，任何變化方法都不會有明顯的影響，因為它們是在結構的*封裝複本*上運作：

```csharp
interface IB
{
    public void Increment() { P += 1; }
    public int P { get; set; }
}
struct T : IB
{
    public int P { get; set; } // auto-property
}

T t = default(T);
Console.WriteLine(t.P); // prints 0
(t as IB).Increment();
Console.WriteLine(t.P); // prints 0
```

> ***已關閉的問題：*** 我們可以做的事：
>
> 1. 禁止 `struct` 繼承預設的執行。 在 `struct`中，所有介面方法都會被視為抽象。 之後，我們可能會花一些時間來決定如何讓它更好的效果。
> 2. 提出一些可避免使用裝箱的程式碼產生策略。 在 `IB.Increment`之類的方法內，`this` 的類型可能類似于限制為 `IB`的類型參數。 此外，為了避免呼叫端中的裝箱，非抽象方法會繼承自介面。 這可能會大幅增加編譯器和 CLR 執行的工作。
> 3. 不必擔心它，而是將它保留為疙瘩。
> 4. 還有其他想法嗎？

***決策：*** 不必擔心它，而是將它保留為疙瘩。 請參閱＜<https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#structs-and-default-implementations>＞。

### <a name="base-interface-invocations-closed"></a>基底介面調用（已關閉）

草稿規格建議的語法適用于 JAVA 所啟發的基底介面調用： `Interface.base.M()`。 我們必須至少選取初始原型的語法。 我的最愛是 `base<Interface>.M()`。

> ***已關閉的問題：*** 基底成員調用的語法為何？

***決策：*** 語法為 `base(Interface).M()`。 請參閱＜<https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>＞。 命名介面必須是基底介面，但不需要是直接基底介面。

> ***開啟問題：*** 是否應該在類別成員中允許基底介面調用？

***決策***：是。 <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>

### <a name="overriding-non-public-interface-members-closed"></a>覆寫非公用介面成員（已關閉）

在介面中，基底介面的非公用成員會使用 `override` 修飾詞來加以覆寫。 如果它是名為包含成員之介面的「明確」覆寫，則會省略存取修飾詞。

> ***已關閉的問題：*** 如果它是不會命名介面的「隱含」覆寫，則存取修飾詞是否必須相符？

***決策：*** 只有公用成員可以隱含地覆寫，且存取必須相符。 請參閱＜<https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>＞。

> ***開啟問題：*** 存取修飾詞是否需要、選擇性或在明確覆寫上省略，例如 `override void IB.M() {}`？

> ***開啟問題：*** 在明確覆寫（例如 `void IB.M() {}`）上，是否 `override` 必要、選擇性或省略？

如何在類別中執行非公用介面成員？ 也許它必須明確地完成嗎？

```csharp
interface IA
{
    internal void MI();
    protected void MP();
}
class C : IA
{
    // are these implementations?
    internal void MI() {}
    protected void MP() {}
}
```

> ***已關閉的問題：*** 如何在類別中執行非公用介面成員？

***決策：*** 您只能明確地執行非公用介面成員。 請參閱＜<https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>＞。

***決策***：介面成員上不允許 `override` 關鍵字。 <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>

### <a name="binary-compatibility-2-closed"></a>二進位相容性2（已關閉）

請考慮下列程式碼，其中的每個類型都在不同的元件中

```csharp
interface I1
{
    void M() { Impl1 }
}
interface I2 : I1
{
    override void M() { Impl2 }
}
interface I3 : I1
{
}
class C : I2, I3
{
}
```

我們瞭解 `C` 中的 `I1.M` 的執行是 `I2.M`。 如果包含 `I3` 的元件依下列方式變更並重新編譯，該怎麼辦

```csharp
interface I3 : I1
{
    override void M() { Impl3 }
}
```

但是 `C` 不會重新編譯。 當程式執行時，會發生什麼事？ `(C as I1).M()` 的調用

1. 執行 `I1.M`
2. 執行 `I2.M`
3. 執行 `I3.M`
4. 可能是2或3，有決定性
5. 擲回某種類型的執行時間例外狀況

***決策***：擲回例外狀況（5）。 請參閱＜<https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#issues-in-default-interface-methods>＞。

### <a name="permit-partial-in-interface-closed"></a>允許在介面中 `partial` 嗎？ 已

假設介面可能會以類似抽象類別使用方式的方式來使用，將其宣告 `partial`可能會很有用。 這在表面上特別有用。

> ***提案：*** 移除介面和介面成員無法 `partial`宣告的語言限制。

***決策***：是。 請參閱＜<https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>＞。

### <a name="main-in-an-interface-closed"></a>在介面中 `Main`？ 已

> ***開啟問題：*** 介面中的 `static Main` 方法是否為程式的進入點的候選項目？

***決策***：是。 請參閱＜<https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#main-in-an-interface>＞。

### <a name="confirm-intent-to-support-public-non-virtual-methods-closed"></a>確認意圖以支援公用非虛擬方法（已關閉）

我們是否可以確認（或反轉）在介面中允許非虛擬公用方法的決策？

```csharp
interface IA
{
    public sealed void M() { }
}
```

> ***半關閉問題：*** （2017-04-18）我們認為它會很有用，但會回到此。 這是心理模型往返區塊。

***決策***：是。 <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#confirm-that-we-support-public-non-virtual-methods>第 1 課：建立 Windows Azure 儲存體物件{2}。

### <a name="does-an-override-in-an-interface-introduce-a-new-member-closed"></a>介面中的 `override` 會引進新的成員嗎？ 已

有幾種方式可以觀察覆寫宣告是否導入了新的成員。

```csharp
interface IA
{
    void M(int x) { }
}
interface IB : IA
{
    override void M(int y) { }
}
interface IC : IB
{
    static void M2()
    {
        M(y: 3); // permitted?
    }
    override void IB.M(int z) { } // permitted? What does it override?
}
```

> ***開啟問題：*** 介面中的覆寫宣告會引進新的成員嗎？ 已

在類別中，覆寫方法在某些感知中是「可見」。 例如，其參數的名稱會優先于覆寫方法中的參數名稱。 您可以在介面中複製該行為，因為一律會有最明確的覆寫。 但要複製該行為嗎？

此外，也可以「覆寫」覆寫方法？ 想法

***決策***：介面成員上不允許 `override` 關鍵字。 <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>第 1 課：建立 Windows Azure 儲存體物件{2}。

### <a name="properties-with-a-private-accessor-closed"></a>具有私用存取子的屬性（已關閉）

我們假設私用成員不是虛擬的，而且不允許虛擬和私用的組合。 但是關於具有私用存取子的屬性呢？

```csharp
interface IA
{
    public virtual int P
    {
        get => 3;
        private set => { }
    }
}
```

這是允許的嗎？ `set` 存取子是否 `virtual`？ 可以在可存取的位置覆寫嗎？ 下列動作是否會隱含地僅執行 `get` 存取子？

```csharp
class C : IA
{
    public int P
    {
        get => 4;
        set { }
    }
}
```

下列原因會因為 IA 而發生錯誤。P. set 不是虛擬的，也不能存取？

```csharp
class C : IA
{
    int IA.P
    {
        get => 4;
        set { }
    }
}
```

***決策***：第一個範例看起來有效，而最後一個則不是。 這會類似已解決其在中的C#運作方式。 <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#properties-with-a-private-accessor>

### <a name="base-interface-invocations-round-2-closed"></a>基底介面調用，四捨五入2（已關閉）

我們先前的「解決方法」，說明如何處理基底調用，實際上並不提供足夠的表達。 與 JAVA 不同的是C# ，在和 CLR 中，您必須同時指定包含方法宣告的介面，以及要叫用的實作為位置。

我建議在介面中進行基底呼叫的下列語法。 我不熟悉它，但它說明了什麼語法必須能夠表達：

```csharp
interface I1 { void M(); }
interface I2 { void M(); }
interface I3 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I4 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I5 : I3, I4
{
    void I1.M()
    {
        base<I3>(I1).M(); // calls I3's implementation of I1.M
        base<I4>(I1).M(); // calls I4's implementation of I1.M
    }
    void I2.M()
    {
        base<I3>(I2).M(); // calls I3's implementation of I2.M
        base<I4>(I2).M(); // calls I4's implementation of I2.M
    }
}
```

如果沒有任何不明確的情況，您可以更輕鬆地撰寫

```csharp
interface I1 { void M(); }
interface I3 : I1 { void I1.M() { } }
interface I4 : I1 { void I1.M() { } }
interface I5 : I3, I4
{
    void I1.M()
    {
        base<I3>.M(); // calls I3's implementation of I1.M
        base<I4>.M(); // calls I4's implementation of I1.M
    }
}
```

Or

```csharp
interface I1 { void M(); }
interface I2 { void M(); }
interface I3 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I5 : I3
{
    void I1.M()
    {
        base(I1).M(); // calls I3's implementation of I1.M
    }
    void I2.M()
    {
        base(I2).M(); // calls I3's implementation of I2.M
    }
}
```

Or

```csharp
interface I1 { void M(); }
interface I3 : I1 { void I1.M() { } }
interface I5 : I3
{
    void I1.M()
    {
        base.M(); // calls I3's implementation of I1.M
    }
}
```

***決策***：決定 `base(N.I1<T>).M(s)`conceding，如果我們有調用系結，稍後可能會發生問題。 <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md#default-interface-implementations>

### <a name="warning-for-struct-not-implementing-default-method-closed"></a>結構未執行預設方法的警告？ 已

@vancem 判斷提示，如果實值型別宣告無法覆寫某些介面方法，就應該考慮產生警告，即使它會從介面繼承該方法的執行。 因為它會造成裝箱和破壞限制的呼叫。

***決策***：這看起來就像是更適合分析器的專案。 此警告似乎也會產生雜訊，因為即使從未呼叫過預設介面方法，也不會發生任何裝箱。 <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#warning-for-struct-not-implementing-default-method>

### <a name="interface-static-constructors-closed"></a>介面靜態的函式（已關閉）

何時會執行介面靜態函式？  目前的 CLI 草稿建議在存取第一個靜態方法或欄位時發生。 如果沒有這兩個專案，則可能永遠不會執行？

[2018-10-09 CLR 小組提出「要鏡像我們為 valuetypes 做的事（對每個實例方法的存取權的 .cctor 檢查）」

***決策***：如果靜態的函式不是 `beforefieldinit`，靜態的函式也會在進入實例方法時執行，在這種情況下，會先執行靜態的函式，然後再存取第一個靜態欄位。 <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#when-are-interface-static-constructors-run>

## <a name="design-meetings"></a>設計會議

[2017-03-08 Ldm 會議筆記](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-08.md)
[2017-03-21 LDM 會議筆記](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md)
[2017-03-23 「預設介面方法的 CLR 行為](https://github.com/dotnet/csharplang/blob/master/meetings/2017/CLR-2017-03-23.md)」
[2017-04-05 LDM 會議筆記](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md)
[2017-04-11 ldm 會議筆記](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-11.md)
[2017-04-18 ldm](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md)會議筆記
2017-04-19 [2017-04-19 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md) ldm 會議筆記
[2017-05-17 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-17.md) [2017-05-31 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md) [2017-05-17 ldm會議筆記](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-06-14.md)
[2018-10-17 Ldm 會議筆記](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
[2018-11-14 ldm 會議筆記](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md)


