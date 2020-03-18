---
ms.openlocfilehash: b0d0fa70d90f7493c6c23be576155a77cec36cf8
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/14/2020
ms.locfileid: "79485179"
---
# <a name="default-interface-methods"></a><span data-ttu-id="e9490-101">預設介面方法</span><span class="sxs-lookup"><span data-stu-id="e9490-101">default interface methods</span></span>

* <span data-ttu-id="e9490-102">[x] 提議</span><span class="sxs-lookup"><span data-stu-id="e9490-102">[x] Proposed</span></span>
* <span data-ttu-id="e9490-103">[] 原型：[進行中](https://github.com/dotnet/roslyn/blob/master/docs/features/DefaultInterfaceImplementation.md)</span><span class="sxs-lookup"><span data-stu-id="e9490-103">[ ] Prototype: [In progress](https://github.com/dotnet/roslyn/blob/master/docs/features/DefaultInterfaceImplementation.md)</span></span>
* <span data-ttu-id="e9490-104">[] 執行：無</span><span class="sxs-lookup"><span data-stu-id="e9490-104">[ ] Implementation: None</span></span>
* <span data-ttu-id="e9490-105">[] 規格：進行中，下方</span><span class="sxs-lookup"><span data-stu-id="e9490-105">[ ] Specification: In progress, below</span></span>

## <a name="summary"></a><span data-ttu-id="e9490-106">摘要</span><span class="sxs-lookup"><span data-stu-id="e9490-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="e9490-107">新增_虛擬擴充方法_的支援：介面中具具體執行的方法。</span><span class="sxs-lookup"><span data-stu-id="e9490-107">Add support for _virtual extension methods_ - methods in interfaces with concrete implementations.</span></span> <span data-ttu-id="e9490-108">執行此類介面的類別或結構，必須具有介面方法（由類別或結構所實，或繼承自其基類或介面）的單一_最特定_執行。</span><span class="sxs-lookup"><span data-stu-id="e9490-108">A class or struct that implements such an interface is required to have a single _most specific_ implementation for the interface method, either implemented by the class or struct, or inherited from its base classes or interfaces.</span></span> <span data-ttu-id="e9490-109">虛擬擴充方法可讓 API 作者在未來的版本中，將方法新增至介面，而不會中斷來源或二進位檔與該介面的現有實現的相容性。</span><span class="sxs-lookup"><span data-stu-id="e9490-109">Virtual extension methods enable an API author to add methods to an interface in future versions without breaking source or binary compatibility with existing implementations of that interface.</span></span>

<span data-ttu-id="e9490-110">這些類似于 JAVA 的「[預設方法](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)」。</span><span class="sxs-lookup"><span data-stu-id="e9490-110">These are similar to Java's ["Default Methods"](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html).</span></span>

<span data-ttu-id="e9490-111">（根據可能的執行技術）此功能需要 CLI/CLR 中的對應支援。</span><span class="sxs-lookup"><span data-stu-id="e9490-111">(Based on the likely implementation technique) this feature requires corresponding support in the CLI/CLR.</span></span> <span data-ttu-id="e9490-112">利用這項功能的程式無法在舊版平臺上執行。</span><span class="sxs-lookup"><span data-stu-id="e9490-112">Programs that take advantage of this feature cannot run on earlier versions of the platform.</span></span>

## <a name="motivation"></a><span data-ttu-id="e9490-113">動機</span><span class="sxs-lookup"><span data-stu-id="e9490-113">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="e9490-114">這項功能的主要動機是</span><span class="sxs-lookup"><span data-stu-id="e9490-114">The principal motivations for this feature are</span></span>

- <span data-ttu-id="e9490-115">預設介面方法可讓 API 作者將方法加入至未來版本中的介面，而不中斷來源或二進位檔與該介面的現有實現的相容性。</span><span class="sxs-lookup"><span data-stu-id="e9490-115">Default interface methods enable an API author to add methods to an interface in future versions without breaking source or binary compatibility with existing implementations of that interface.</span></span>
- <span data-ttu-id="e9490-116">此功能可C#讓與以[Android （JAVA）](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)和[iOS （Swift）](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID267)為目標的 api 交互操作，以支援類似的功能。</span><span class="sxs-lookup"><span data-stu-id="e9490-116">The feature enables C# to interoperate with APIs targeting [Android (Java)](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html) and [iOS (Swift)](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID267), which support similar features.</span></span>
- <span data-ttu-id="e9490-117">結果是，新增預設介面執行會提供「特性」語言功能的元素（<https://en.wikipedia.org/wiki/Trait_(computer_programming)>）。</span><span class="sxs-lookup"><span data-stu-id="e9490-117">As it turns out, adding default interface implementations provides the elements of the "traits" language feature (<https://en.wikipedia.org/wiki/Trait_(computer_programming)>).</span></span> <span data-ttu-id="e9490-118">特性已證明是強大的程式設計技巧（<http://scg.unibe.ch/archive/papers/Scha03aTraits.pdf>）。</span><span class="sxs-lookup"><span data-stu-id="e9490-118">Traits have proven to be a powerful programming technique (<http://scg.unibe.ch/archive/papers/Scha03aTraits.pdf>).</span></span>

## <a name="detailed-design"></a><span data-ttu-id="e9490-119">詳細設計</span><span class="sxs-lookup"><span data-stu-id="e9490-119">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="e9490-120">介面的語法已擴充為允許</span><span class="sxs-lookup"><span data-stu-id="e9490-120">The syntax for an interface is extended to permit</span></span>

- <span data-ttu-id="e9490-121">宣告常數、運算子、靜態函數和巢狀型別的成員宣告;</span><span class="sxs-lookup"><span data-stu-id="e9490-121">member declarations that declare constants, operators, static constructors, and nested types;</span></span>
- <span data-ttu-id="e9490-122">方法或索引子、屬性或事件存取子的*主體*（也就是「預設」實作為）;</span><span class="sxs-lookup"><span data-stu-id="e9490-122">a *body* for a method or indexer, property, or event accessor (that is, a "default" implementation);</span></span>
- <span data-ttu-id="e9490-123">宣告靜態欄位、方法、屬性、索引子和事件的成員宣告;</span><span class="sxs-lookup"><span data-stu-id="e9490-123">member declarations that declare static fields, methods, properties, indexers, and events;</span></span>
- <span data-ttu-id="e9490-124">使用明確介面執行語法的成員宣告;和</span><span class="sxs-lookup"><span data-stu-id="e9490-124">member declarations using the explicit interface implementation syntax; and</span></span>
- <span data-ttu-id="e9490-125">明確存取修飾詞（預設存取是 `public`）。</span><span class="sxs-lookup"><span data-stu-id="e9490-125">Explicit access modifiers (the default access is `public`).</span></span>

<span data-ttu-id="e9490-126">具有主體的成員允許介面在不提供覆寫實作的類別和結構中，為方法提供「預設」的執行。</span><span class="sxs-lookup"><span data-stu-id="e9490-126">Members with bodies permit the interface to provide a "default" implementation for the method in classes and structs that do not provide an overriding implementation.</span></span>

<span data-ttu-id="e9490-127">介面可能不包含實例狀態。</span><span class="sxs-lookup"><span data-stu-id="e9490-127">Interfaces may not contain instance state.</span></span> <span data-ttu-id="e9490-128">雖然現在允許靜態欄位，但介面中不允許實例欄位。</span><span class="sxs-lookup"><span data-stu-id="e9490-128">While static fields are now permitted, instance fields are not permitted in interfaces.</span></span> <span data-ttu-id="e9490-129">介面中不支援實例 auto 屬性，因為它們會隱含宣告隱藏的欄位。</span><span class="sxs-lookup"><span data-stu-id="e9490-129">Instance auto-properties are not supported in interfaces, as they would implicitly declare a hidden field.</span></span>

<span data-ttu-id="e9490-130">靜態和私用方法允許用來執行介面公用 API 的實用重構和程式碼組織。</span><span class="sxs-lookup"><span data-stu-id="e9490-130">Static and private methods permit useful refactoring and organization of code used to implement the interface's public API.</span></span>

<span data-ttu-id="e9490-131">介面中的方法覆寫必須使用明確的介面執行語法。</span><span class="sxs-lookup"><span data-stu-id="e9490-131">A method override in an interface must use the explicit interface implementation syntax.</span></span>

<span data-ttu-id="e9490-132">在以*variance_annotation*宣告的型別參數範圍內，宣告類別型別、結構型別或列舉型別是錯誤的。</span><span class="sxs-lookup"><span data-stu-id="e9490-132">It is an error to declare a class type, struct type, or enum type within the scope of a type parameter that was declared with a *variance_annotation*.</span></span>  <span data-ttu-id="e9490-133">例如，下列 `C` 的宣告是錯誤。</span><span class="sxs-lookup"><span data-stu-id="e9490-133">For example, the declaration of `C` below is an error.</span></span>

```csharp
interface IOuter<out T>
{
    class C { } // error: class declaration within the scope of variant type parameter 'T'
}
```

### <a name="concrete-methods-in-interfaces"></a><span data-ttu-id="e9490-134">介面中的具體方法</span><span class="sxs-lookup"><span data-stu-id="e9490-134">Concrete methods in interfaces</span></span>

<span data-ttu-id="e9490-135">這項功能最簡單的形式是在介面中宣告*具體的方法*，這是具有主體的方法。</span><span class="sxs-lookup"><span data-stu-id="e9490-135">The simplest form of this feature is the ability to declare a *concrete method* in an interface, which is a method with a body.</span></span>

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
```

<span data-ttu-id="e9490-136">執行此介面的類別不需要實作為其具體的方法。</span><span class="sxs-lookup"><span data-stu-id="e9490-136">A class that implements this interface need not implement its concrete method.</span></span>

```csharp
class C : IA { } // OK

IA i = new C();
i.M(); // prints "IA.M"
```

<span data-ttu-id="e9490-137">類別 `C` 中 `IA.M` 的最終覆寫是 `M` 在 `IA`中宣告的實體方法。</span><span class="sxs-lookup"><span data-stu-id="e9490-137">The final override for `IA.M` in class `C` is the concrete method `M` declared in `IA`.</span></span> <span data-ttu-id="e9490-138">請注意，類別不會繼承其介面的成員;這項功能不會變更：</span><span class="sxs-lookup"><span data-stu-id="e9490-138">Note that a class does not inherit members from its interfaces; that is not changed by this feature:</span></span>

```csharp
new C().M(); // error: class 'C' does not contain a member 'M'
```

<span data-ttu-id="e9490-139">在介面的實例成員內，`this` 具有封閉式介面的型別。</span><span class="sxs-lookup"><span data-stu-id="e9490-139">Within an instance member of an interface, `this` has the type of the enclosing interface.</span></span>

### <a name="modifiers-in-interfaces"></a><span data-ttu-id="e9490-140">介面中的修飾詞</span><span class="sxs-lookup"><span data-stu-id="e9490-140">Modifiers in interfaces</span></span>

<span data-ttu-id="e9490-141">介面的語法會寬鬆，以允許其成員上的修飾詞。</span><span class="sxs-lookup"><span data-stu-id="e9490-141">The syntax for an interface is relaxed to permit modifiers on its members.</span></span> <span data-ttu-id="e9490-142">允許下列各項： `private`、`protected`、`internal`、`public`、`virtual`、`abstract`、`sealed`、`static`、`extern`和 `partial`。</span><span class="sxs-lookup"><span data-stu-id="e9490-142">The following are permitted: `private`, `protected`, `internal`, `public`, `virtual`, `abstract`, `sealed`, `static`, `extern`, and `partial`.</span></span>

> <span data-ttu-id="e9490-143">***TODO***：檢查有哪些其他修飾詞存在。</span><span class="sxs-lookup"><span data-stu-id="e9490-143">***TODO***: check what other modifiers exist.</span></span>

<span data-ttu-id="e9490-144">除非使用 `sealed` 或 `private` 修飾詞，否則宣告包含主體的介面成員是 `virtual` 成員。</span><span class="sxs-lookup"><span data-stu-id="e9490-144">An interface member whose declaration includes a body is a `virtual` member unless the `sealed` or `private` modifier is used.</span></span> <span data-ttu-id="e9490-145">`virtual` 修飾詞可以在函式成員上使用，否則會隱含地 `virtual`。</span><span class="sxs-lookup"><span data-stu-id="e9490-145">The `virtual` modifier may be used on a function member that would otherwise be implicitly `virtual`.</span></span> <span data-ttu-id="e9490-146">同樣地，雖然 `abstract` 在沒有主體的介面成員上是預設值，但可以明確指定該修飾詞。</span><span class="sxs-lookup"><span data-stu-id="e9490-146">Similarly, although `abstract` is the default on interface members without bodies, that modifier may be given explicitly.</span></span> <span data-ttu-id="e9490-147">您可以使用 `sealed` 關鍵字來宣告非虛擬成員。</span><span class="sxs-lookup"><span data-stu-id="e9490-147">A non-virtual member may be declared using the `sealed` keyword.</span></span>

<span data-ttu-id="e9490-148">介面的 `private` 或 `sealed` 函式成員沒有主體時，就會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="e9490-148">It is an error for a `private` or `sealed` function member of an interface to have no body.</span></span> <span data-ttu-id="e9490-149">`private` 的函式成員可能沒有修飾詞 `sealed`。</span><span class="sxs-lookup"><span data-stu-id="e9490-149">A `private` function member may not have the modifier `sealed`.</span></span>

<span data-ttu-id="e9490-150">存取修飾詞可用於允許之所有類型成員的介面成員上。</span><span class="sxs-lookup"><span data-stu-id="e9490-150">Access modifiers may be used on interface members of all kinds of members that are permitted.</span></span> <span data-ttu-id="e9490-151">存取層級 `public` 是預設值，但可以明確指定。</span><span class="sxs-lookup"><span data-stu-id="e9490-151">The access level `public` is the default but it may be given explicitly.</span></span>

> <span data-ttu-id="e9490-152">***開啟問題：*** 我們需要指定存取修飾詞的確切意義，例如 `protected` 和 `internal`，以及哪些宣告會執行和不覆寫它們（在衍生介面中）或加以執行（在執行介面的類別中）。</span><span class="sxs-lookup"><span data-stu-id="e9490-152">***Open Issue:*** We need to specify the precise meaning of the access modifiers such as `protected` and `internal`, and which declarations do and do not override them (in a derived interface) or implement them (in a class that implements the interface).</span></span>

<span data-ttu-id="e9490-153">介面可以宣告 `static` 成員，包括巢狀型別、方法、索引子、屬性、事件和靜態的函式。</span><span class="sxs-lookup"><span data-stu-id="e9490-153">Interfaces may declare `static` members, including nested types, methods, indexers, properties, events, and static constructors.</span></span> <span data-ttu-id="e9490-154">所有介面成員的預設存取層級都是 `public`。</span><span class="sxs-lookup"><span data-stu-id="e9490-154">The default access level for all interface members is `public`.</span></span>

<span data-ttu-id="e9490-155">介面可能不會宣告實例的函式、析構函式或欄位。</span><span class="sxs-lookup"><span data-stu-id="e9490-155">Interfaces may not declare instance constructors, destructors, or fields.</span></span>

> <span data-ttu-id="e9490-156">***已關閉的問題：*** 介面中是否允許運算子宣告？</span><span class="sxs-lookup"><span data-stu-id="e9490-156">***Closed Issue:*** Should operator declarations be permitted in an interface?</span></span> <span data-ttu-id="e9490-157">可能不是轉換運算子，但其他則是什麼？</span><span class="sxs-lookup"><span data-stu-id="e9490-157">Probably not conversion operators, but what about others?</span></span> <span data-ttu-id="e9490-158">***決策***：除了轉換、相等和不等比較運算子*之外*，允許運算子。</span><span class="sxs-lookup"><span data-stu-id="e9490-158">***Decision***: Operators are permitted *except* for conversion, equality, and inequality operators.</span></span>

> <span data-ttu-id="e9490-159">***已關閉的問題：*** 在隱藏基底介面成員的介面成員宣告上，應該允許 `new` 嗎？</span><span class="sxs-lookup"><span data-stu-id="e9490-159">***Closed Issue:*** Should `new` be permitted on interface member declarations that hide members from base interfaces?</span></span> <span data-ttu-id="e9490-160">***決策***：是。</span><span class="sxs-lookup"><span data-stu-id="e9490-160">***Decision***: Yes.</span></span>

> <span data-ttu-id="e9490-161">***已關閉的問題：*** 目前不允許在介面或其成員上 `partial`。</span><span class="sxs-lookup"><span data-stu-id="e9490-161">***Closed Issue:*** We do not currently permit `partial` on an interface or its members.</span></span> <span data-ttu-id="e9490-162">這需要個別的提案。</span><span class="sxs-lookup"><span data-stu-id="e9490-162">That would require a separate proposal.</span></span> <span data-ttu-id="e9490-163">***決策***：是。</span><span class="sxs-lookup"><span data-stu-id="e9490-163">***Decision***: Yes.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>

### <a name="overrides-in-interfaces"></a><span data-ttu-id="e9490-164">介面中的覆寫</span><span class="sxs-lookup"><span data-stu-id="e9490-164">Overrides in interfaces</span></span>

<span data-ttu-id="e9490-165">覆寫宣告（也就是包含 `override` 修飾詞的宣告）可讓程式設計師在編譯器或執行時間不會找到一個的介面中，提供最特定的虛擬成員執行方式。</span><span class="sxs-lookup"><span data-stu-id="e9490-165">Override declarations (i.e. those containing the `override` modifier) allow the programmer to provide a most specific implementation of a virtual member in an interface where the compiler or runtime would not otherwise find one.</span></span> <span data-ttu-id="e9490-166">它也允許從超級介面將抽象成員轉換成衍生介面中的預設成員。</span><span class="sxs-lookup"><span data-stu-id="e9490-166">It also allows turning an abstract member from a super-interface into a default member in a derived interface.</span></span> <span data-ttu-id="e9490-167">藉由使用介面名稱限定宣告（在此情況下不允許存取修飾詞），允許覆寫宣告*明確*覆寫特定基底介面方法。</span><span class="sxs-lookup"><span data-stu-id="e9490-167">An override declaration is permitted to *explicitly* override a particular base interface method by qualifying the declaration with the interface name (no access modifier is permitted in this case).</span></span> <span data-ttu-id="e9490-168">不允許隱含覆寫。</span><span class="sxs-lookup"><span data-stu-id="e9490-168">Implicit overrides are not permitted.</span></span>

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

<span data-ttu-id="e9490-169">介面中的覆寫宣告可能不會 `sealed`宣告。</span><span class="sxs-lookup"><span data-stu-id="e9490-169">Override declarations in interfaces may not be declared `sealed`.</span></span>

<span data-ttu-id="e9490-170">公用 `virtual` 介面中的函式成員可能會在衍生介面中明確覆寫（藉由在覆寫宣告中以原本宣告方法的介面類別型來限定名稱，以及省略存取修飾詞）。</span><span class="sxs-lookup"><span data-stu-id="e9490-170">Public `virtual` function members in an interface may be overridden in a derived interface explicitly (by qualifying the name in the override declaration with the interface type that originally declared the method, and omitting an access modifier).</span></span>

<span data-ttu-id="e9490-171">介面中的 `virtual` 函式成員只能在衍生介面中明確覆寫（非隱含），而不 `public` 的成員只能在類別或結構中明確實作為（非隱含）。</span><span class="sxs-lookup"><span data-stu-id="e9490-171">`virtual` function members in an interface may only be overridden explicitly (not implicitly) in derived interfaces, and members that are not `public` may only be implemented in a class or struct explicitly (not implicitly).</span></span> <span data-ttu-id="e9490-172">不論是哪一種情況，覆寫或已執行的成員都必須可在覆寫時*存取*。</span><span class="sxs-lookup"><span data-stu-id="e9490-172">In either case, the overridden or implemented member must be *accessible* where it is overridden.</span></span>

### <a name="reabstraction"></a><span data-ttu-id="e9490-173">Reabstraction</span><span class="sxs-lookup"><span data-stu-id="e9490-173">Reabstraction</span></span>

<span data-ttu-id="e9490-174">在介面中宣告的虛擬（具體）方法可能會覆寫為衍生介面中的抽象。</span><span class="sxs-lookup"><span data-stu-id="e9490-174">A virtual (concrete) method declared in an interface may be overridden to be abstract in a derived interface</span></span>

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

<span data-ttu-id="e9490-175">`IB.M` 的宣告中不需要 `abstract` 修飾詞（這是介面中的預設值），但在覆寫宣告中可能是明確的做法。</span><span class="sxs-lookup"><span data-stu-id="e9490-175">The `abstract` modifier is not required in the declaration of `IB.M` (that is the default in interfaces), but it is probably good practice to be explicit in an override declaration.</span></span>

<span data-ttu-id="e9490-176">這在衍生介面中很有用，因為方法的預設實作為不適當，而更適當的執行應由實作為類別來提供。</span><span class="sxs-lookup"><span data-stu-id="e9490-176">This is useful in derived interfaces where the default implementation of a method is inappropriate and a more appropriate implementation should be provided by implementing classes.</span></span>

> <span data-ttu-id="e9490-177">***開啟問題：*** 是否應允許 reabstraction？</span><span class="sxs-lookup"><span data-stu-id="e9490-177">***Open Issue:*** Should reabstraction be permitted?</span></span>

### <a name="the-most-specific-override-rule"></a><span data-ttu-id="e9490-178">最特定的覆寫規則</span><span class="sxs-lookup"><span data-stu-id="e9490-178">The most specific override rule</span></span>

<span data-ttu-id="e9490-179">在類型或其直接和間接介面中出現的覆寫之間，我們要求每個介面和類別都有*最明確*的覆寫。</span><span class="sxs-lookup"><span data-stu-id="e9490-179">We require that every interface and class have a *most specific override* for every virtual member among the overrides appearing in the type or its direct and indirect interfaces.</span></span> <span data-ttu-id="e9490-180">*最特定*的覆寫是唯一的覆寫，比其他每個覆寫更明確。</span><span class="sxs-lookup"><span data-stu-id="e9490-180">The *most specific override* is a unique override that is more specific than every other override.</span></span> <span data-ttu-id="e9490-181">如果沒有覆寫，成員本身會被視為最明確的覆寫。</span><span class="sxs-lookup"><span data-stu-id="e9490-181">If there is no override, the member itself is considered the most specific override.</span></span>

<span data-ttu-id="e9490-182">一個覆寫 `M1` 被視為比另一個覆寫*更明確*`M2` 如果 `M1` 是在類型 `T1`上宣告，`M2` 會在類型 `T2`上宣告，而且兩者都是</span><span class="sxs-lookup"><span data-stu-id="e9490-182">One override `M1` is considered *more specific* than another override `M2` if `M1` is declared on type `T1`, `M2` is declared on type `T2`, and either</span></span>

1. <span data-ttu-id="e9490-183">`T1` 包含其直接或間接介面之間的 `T2`，或</span><span class="sxs-lookup"><span data-stu-id="e9490-183">`T1` contains `T2` among its direct or indirect interfaces, or</span></span>
2. <span data-ttu-id="e9490-184">`T2` 是介面類別型，但 `T1` 不是介面類別型。</span><span class="sxs-lookup"><span data-stu-id="e9490-184">`T2` is an interface type but `T1` is not an interface type.</span></span>

<span data-ttu-id="e9490-185">例如：</span><span class="sxs-lookup"><span data-stu-id="e9490-185">For example:</span></span>

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

<span data-ttu-id="e9490-186">最特定的覆寫規則可確保程式設計人員在發生衝突時明確解決衝突（也就是菱形繼承所產生的多義性）。</span><span class="sxs-lookup"><span data-stu-id="e9490-186">The most specific override rule ensures that a conflict (i.e. an ambiguity arising from diamond inheritance) is resolved explicitly by the programmer at the point where the conflict arises.</span></span>

<span data-ttu-id="e9490-187">因為我們在介面中支援明確的抽象覆寫，所以我們也可以在類別中這麼做</span><span class="sxs-lookup"><span data-stu-id="e9490-187">Because we support explicit abstract overrides in interfaces, we could do so in classes as well</span></span>

```csharp
abstract class E : IA, IB, IC // ok
{
    abstract void IA.M();
}
```

> <span data-ttu-id="e9490-188">***開啟問題***：我們是否應該在類別中支援明確的介面抽象覆寫？</span><span class="sxs-lookup"><span data-stu-id="e9490-188">***Open issue***: should we support explicit interface abstract overrides in classes?</span></span>

<span data-ttu-id="e9490-189">此外，如果在類別宣告中，某些介面方法的最特定覆寫是在介面中宣告的抽象覆寫，就會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="e9490-189">In addition, it is an error if in a class declaration the most specific override of some interface method is an abstract override that was declared in an interface.</span></span> <span data-ttu-id="e9490-190">這是使用新術語先重新開機的現有規則。</span><span class="sxs-lookup"><span data-stu-id="e9490-190">This is an existing rule restated using the new terminology.</span></span>

```csharp
interface IF
{
    void M();
}
abstract class F : IF { } // error: 'F' does not implement 'IF.M'
```

<span data-ttu-id="e9490-191">在介面中宣告的虛擬屬性可以在某個介面中擁有最明確 `get` 的覆寫，並在不同的介面中，針對其 `set` 存取子使用最特定的覆寫。</span><span class="sxs-lookup"><span data-stu-id="e9490-191">It is possible for a virtual property declared in an interface to have a most specific override for its `get` accessor in one interface and a most specific override for its `set` accessor in a different interface.</span></span> <span data-ttu-id="e9490-192">這會被視為違反*最特定*的覆寫規則。</span><span class="sxs-lookup"><span data-stu-id="e9490-192">This is considered a violation of the *most specific override* rule.</span></span>

### <a name="static-and-private-methods"></a><span data-ttu-id="e9490-193">`static` 和 `private` 方法</span><span class="sxs-lookup"><span data-stu-id="e9490-193">`static` and `private` methods</span></span>

<span data-ttu-id="e9490-194">因為介面現在可能包含可執行檔程式碼，所以將通用程式碼抽象成私用和靜態方法會很有用。</span><span class="sxs-lookup"><span data-stu-id="e9490-194">Because interfaces may now contain executable code, it is useful to abstract common code into private and static methods.</span></span> <span data-ttu-id="e9490-195">我們現在會在介面中允許這些。</span><span class="sxs-lookup"><span data-stu-id="e9490-195">We now permit these in interfaces.</span></span>

> <span data-ttu-id="e9490-196">***已關閉的問題***：我們是否應該支援私用方法？</span><span class="sxs-lookup"><span data-stu-id="e9490-196">***Closed issue***: Should we support private methods?</span></span> <span data-ttu-id="e9490-197">我們是否應該支援靜態方法？</span><span class="sxs-lookup"><span data-stu-id="e9490-197">Should we support static methods?</span></span> <span data-ttu-id="e9490-198">**決策：是**</span><span class="sxs-lookup"><span data-stu-id="e9490-198">**Decision: YES**</span></span>

> <span data-ttu-id="e9490-199">***開啟問題***：我們是否允許介面方法 `protected` 或 `internal` 或其他存取權？</span><span class="sxs-lookup"><span data-stu-id="e9490-199">***Open issue***: should we permit interface methods to be `protected` or `internal` or other access?</span></span> <span data-ttu-id="e9490-200">若是如此，什麼是語義？</span><span class="sxs-lookup"><span data-stu-id="e9490-200">If so, what are the semantics?</span></span> <span data-ttu-id="e9490-201">它們預設是否 `virtual`？</span><span class="sxs-lookup"><span data-stu-id="e9490-201">Are they `virtual` by default?</span></span> <span data-ttu-id="e9490-202">若是如此，是否有辦法讓它們成為非虛擬？</span><span class="sxs-lookup"><span data-stu-id="e9490-202">If so, is there a way to make them non-virtual?</span></span>

> <span data-ttu-id="e9490-203">***開啟問題***：如果我們支援靜態方法，我們是否支援（靜態）運算子？</span><span class="sxs-lookup"><span data-stu-id="e9490-203">***Open issue***: If we support static methods, should we support (static) operators?</span></span>

### <a name="base-interface-invocations"></a><span data-ttu-id="e9490-204">基底介面調用</span><span class="sxs-lookup"><span data-stu-id="e9490-204">Base interface invocations</span></span>

<span data-ttu-id="e9490-205">類型中的程式碼，衍生自具有預設方法的介面，可以明確地叫用該介面的「基底」實值。</span><span class="sxs-lookup"><span data-stu-id="e9490-205">Code in a type that derives from an interface with a default method can explicitly invoke that interface's "base" implementation.</span></span>

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

<span data-ttu-id="e9490-206">實例（非靜態）方法可以使用 `base(Type).M`的語法來命名，以叫用直接基底介面 nonvirtually 中可存取的實例方法的執行。</span><span class="sxs-lookup"><span data-stu-id="e9490-206">An instance (nonstatic) method is permitted to invoke the implementation of an accessible instance method in a direct base interface nonvirtually by naming it using the syntax `base(Type).M`.</span></span> <span data-ttu-id="e9490-207">當因為鑽石繼承而需要提供覆寫時，會委派給一個特定的基底執行來解決此問題，這項功能就很有用。</span><span class="sxs-lookup"><span data-stu-id="e9490-207">This is useful when an override that is required to be provided due to diamond inheritance is resolved by delegating to one particular base implementation.</span></span>

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

<span data-ttu-id="e9490-208">使用語法 `base(Type).M`來存取 `virtual` 或 `abstract` 成員時，`Type` 必須包含 `M`的唯一*最特定覆寫*。</span><span class="sxs-lookup"><span data-stu-id="e9490-208">When a `virtual` or `abstract` member is accessed using the syntax `base(Type).M`, it is required that `Type` contains a unique *most specific override* for `M`.</span></span>

### <a name="binding-base-clauses"></a><span data-ttu-id="e9490-209">系結基底子句</span><span class="sxs-lookup"><span data-stu-id="e9490-209">Binding base clauses</span></span>

<span data-ttu-id="e9490-210">介面現在包含類型。</span><span class="sxs-lookup"><span data-stu-id="e9490-210">Interfaces now contain types.</span></span>  <span data-ttu-id="e9490-211">這些類型可在基底子句中當做基底介面使用。</span><span class="sxs-lookup"><span data-stu-id="e9490-211">These types may be used in the base clause as base interfaces.</span></span>  <span data-ttu-id="e9490-212">系結基底子句時，我們可能需要知道一組基底介面來系結這些類型（例如，在其中查閱，以及解析受保護的存取）。</span><span class="sxs-lookup"><span data-stu-id="e9490-212">When binding a base clause, we may need to know the set of base interfaces to bind those types (e.g. to lookup in them and to resolve protected access).</span></span>  <span data-ttu-id="e9490-213">因此，介面的基底子句的意義是迴圈定義的。</span><span class="sxs-lookup"><span data-stu-id="e9490-213">The meaning of an interface's base clause is thus circularly defined.</span></span>  <span data-ttu-id="e9490-214">為了中斷迴圈，我們新增了一個新的語言規則，其對應至已適用于類別的類似規則。</span><span class="sxs-lookup"><span data-stu-id="e9490-214">To break the cycle, we add a new language rules corresponding to a similar rule already in place for classes.</span></span>

<span data-ttu-id="e9490-215">判斷介面*interface_base*的意義時，會暫時假設基底介面是空的。</span><span class="sxs-lookup"><span data-stu-id="e9490-215">While determining the meaning of the *interface_base* of an interface, the base interfaces are temporarily assumed to be empty.</span></span> <span data-ttu-id="e9490-216">這可確保基底子句的意義無法以遞迴方式相依于其本身。</span><span class="sxs-lookup"><span data-stu-id="e9490-216">Intuitively this ensures that the meaning of a base clause cannot recursively depend on itself.</span></span> 

<span data-ttu-id="e9490-217">**我們使用的規則如下：**</span><span class="sxs-lookup"><span data-stu-id="e9490-217">**We used to have the following rules:**</span></span>

<span data-ttu-id="e9490-218">「當類別 B 衍生自類別 A 時，就會發生編譯時期錯誤，而會相依于 B。類別會**直接相依于**其直接基類（如果有的話），而**直接取決於**立即嵌套的~~**類別**~~（如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="e9490-218">"When a class B derives from a class A, it is a compile-time error for A to depend on B. A class **directly depends on** its direct base class (if any) and **directly depends on** the ~~**class**~~ within which it is immediately nested (if any).</span></span> <span data-ttu-id="e9490-219">根據這個定義，類別所相依的一組完整~~**類別**~~是**直接相依于**關聯性的自反和可轉移關閉。」</span><span class="sxs-lookup"><span data-stu-id="e9490-219">Given this definition, the complete set of ~~**classes**~~ upon which a class depends is the reflexive and transitive closure of the **directly depends on** relationship."</span></span>

<span data-ttu-id="e9490-220">這是直接或間接繼承自本身的介面編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="e9490-220">It is a compile-time error for an interface to directly or indirectly inherit from itself.</span></span>
<span data-ttu-id="e9490-221">介面的**基底介面**是明確基底介面和其基底介面。</span><span class="sxs-lookup"><span data-stu-id="e9490-221">The **base interfaces** of an interface are the explicit base interfaces and their base interfaces.</span></span> <span data-ttu-id="e9490-222">換句話說，基底介面集是明確基底介面、其明確基底介面等的完整可轉移關閉。</span><span class="sxs-lookup"><span data-stu-id="e9490-222">In other words, the set of base interfaces is the complete transitive closure of the explicit base interfaces, their explicit base interfaces, and so on.</span></span>

<span data-ttu-id="e9490-223">**我們會調整它們，如下所示：**</span><span class="sxs-lookup"><span data-stu-id="e9490-223">**We are adjusting them as follows:**</span></span>

<span data-ttu-id="e9490-224">當類別 B 衍生自類別 A 時，就會發生編譯時期錯誤，而會相依于 B。類別會**直接相依于**其直接基類（如果有的話），而**直接取決於**立即嵌套的 _**型**_ 別（如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="e9490-224">When a class B derives from a class A, it is a compile-time error for A to depend on B. A class **directly depends on** its direct base class (if any) and **directly depends on** the _**type**_ within which it is immediately nested (if any).</span></span>

<span data-ttu-id="e9490-225">當介面 IB 擴充介面 IA 時，就會發生編譯時期錯誤，因此 IA 會依賴 IB。</span><span class="sxs-lookup"><span data-stu-id="e9490-225">When an interface IB extends an interface IA, it is a compile-time error for IA to depend on IB.</span></span> <span data-ttu-id="e9490-226">介面會**直接相依于**其直接基底介面（如果有的話），而**直接取決於**立即嵌套的類型（如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="e9490-226">An interface **directly depends on** its direct base interfaces (if any) and **directly depends on** the type within which it is immediately nested (if any).</span></span>

<span data-ttu-id="e9490-227">根據這些定義，類型所相依的完整**類型**集合是**直接相依于**關聯性的自反和可轉移關閉。</span><span class="sxs-lookup"><span data-stu-id="e9490-227">Given these definitions, the complete set of **types** upon which a type depends is the reflexive and transitive closure of the **directly depends on** relationship.</span></span>

### <a name="effect-on-existing-programs"></a><span data-ttu-id="e9490-228">對現有程式的影響</span><span class="sxs-lookup"><span data-stu-id="e9490-228">Effect on existing programs</span></span>

<span data-ttu-id="e9490-229">此處提供的規則主要是對現有程式的意義沒有影響。</span><span class="sxs-lookup"><span data-stu-id="e9490-229">The rules presented here are intended to have no effect on the meaning of existing programs.</span></span>

<span data-ttu-id="e9490-230">範例 1：</span><span class="sxs-lookup"><span data-stu-id="e9490-230">Example 1:</span></span>

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

<span data-ttu-id="e9490-231">範例 2：</span><span class="sxs-lookup"><span data-stu-id="e9490-231">Example 2:</span></span>

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

<span data-ttu-id="e9490-232">相同的規則會提供類似的結果，與包含預設介面方法的類似情況有關：</span><span class="sxs-lookup"><span data-stu-id="e9490-232">The same rules give similar results to the analogous situation involving default interface methods:</span></span>

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

> <span data-ttu-id="e9490-233">***已關閉的問題***：確認這是規格的預期結果。</span><span class="sxs-lookup"><span data-stu-id="e9490-233">***Closed issue***: confirm that this is an intended consequence of the specification.</span></span> <span data-ttu-id="e9490-234">**決策：是**</span><span class="sxs-lookup"><span data-stu-id="e9490-234">**Decision: YES**</span></span>

### <a name="runtime-method-resolution"></a><span data-ttu-id="e9490-235">執行時間方法解析</span><span class="sxs-lookup"><span data-stu-id="e9490-235">Runtime method resolution</span></span>

> <span data-ttu-id="e9490-236">***已關閉的問題：*** 在介面預設方法的臉部中，規格應該會描述執行時間方法解析演算法。</span><span class="sxs-lookup"><span data-stu-id="e9490-236">***Closed Issue:*** The spec should describe the runtime method resolution algorithm in the face of interface default methods.</span></span> <span data-ttu-id="e9490-237">我們需要確保此語義與語言語義一致，例如哪些宣告的方法會執行，而且不會覆寫或執行 `internal` 方法。</span><span class="sxs-lookup"><span data-stu-id="e9490-237">We need to ensure that the semantics are consistent with the language semantics, e.g. which declared methods do and do not override or implement an `internal` method.</span></span>

### <a name="clr-support-api"></a><span data-ttu-id="e9490-238">CLR 支援 API</span><span class="sxs-lookup"><span data-stu-id="e9490-238">CLR support API</span></span>

<span data-ttu-id="e9490-239">為了讓編譯器能夠在編譯支援這項功能的執行時間時，偵測到這類執行時間的程式庫會透過 <https://github.com/dotnet/corefx/issues/17116>中所討論的 API 來公告該事實。</span><span class="sxs-lookup"><span data-stu-id="e9490-239">In order for compilers to detect when they are compiling for a runtime that supports this feature, libraries for such runtimes are modified to advertise that fact through the API discussed in <https://github.com/dotnet/corefx/issues/17116>.</span></span> <span data-ttu-id="e9490-240">我們新增</span><span class="sxs-lookup"><span data-stu-id="e9490-240">We add</span></span>

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

> <span data-ttu-id="e9490-241">***開啟問題***：這是*CLR*功能的最佳名稱嗎？</span><span class="sxs-lookup"><span data-stu-id="e9490-241">***Open issue***: Is that the best name for the *CLR* feature?</span></span> <span data-ttu-id="e9490-242">CLR 功能不只是這麼做（例如放寬保護條件約束、支援介面中的覆寫等等）。</span><span class="sxs-lookup"><span data-stu-id="e9490-242">The CLR feature does much more than just that (e.g. relaxes protection constraints, supports overrides in interfaces, etc).</span></span> <span data-ttu-id="e9490-243">也許它應該稱為「介面中的具體方法」或「特性」之類的東西？</span><span class="sxs-lookup"><span data-stu-id="e9490-243">Perhaps it should be called something like "concrete methods in interfaces", or "traits"?</span></span>

### <a name="further-areas-to-be-specified"></a><span data-ttu-id="e9490-244">要指定的進一步區域</span><span class="sxs-lookup"><span data-stu-id="e9490-244">Further areas to be specified</span></span>

- <span data-ttu-id="e9490-245">[] 將預設的介面方法和覆寫加入現有的介面，以將造成的來源和二進位相容性的類型進行類別目錄的分類非常有用。</span><span class="sxs-lookup"><span data-stu-id="e9490-245">[ ] It would be useful to catalog the kinds of source and binary compatibility effects caused by adding default interface methods and overrides to existing interfaces.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="e9490-246">缺點</span><span class="sxs-lookup"><span data-stu-id="e9490-246">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="e9490-247">此提案需要 CLR 規格的協調更新（以支援介面和方法解析中的具體方法）。</span><span class="sxs-lookup"><span data-stu-id="e9490-247">This proposal requires a coordinated update to the CLR specification (to support concrete methods in interfaces and method resolution).</span></span> <span data-ttu-id="e9490-248">因此，它相當「昂貴」，而且可能會與其他也預期需要 CLR 變更的功能相結合。</span><span class="sxs-lookup"><span data-stu-id="e9490-248">It is therefore fairly "expensive" and it may be worth doing in combination with other features that we also anticipate would require CLR changes.</span></span>

## <a name="alternatives"></a><span data-ttu-id="e9490-249">替代方案</span><span class="sxs-lookup"><span data-stu-id="e9490-249">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="e9490-250">無。</span><span class="sxs-lookup"><span data-stu-id="e9490-250">None.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="e9490-251">未解決的問題</span><span class="sxs-lookup"><span data-stu-id="e9490-251">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="e9490-252">在上述的提案中，會提出 Open 問題。</span><span class="sxs-lookup"><span data-stu-id="e9490-252">Open questions are called out throughout the proposal, above.</span></span>
- <span data-ttu-id="e9490-253">另請參閱 <https://github.com/dotnet/csharplang/issues/406> 以取得未解決問題的清單。</span><span class="sxs-lookup"><span data-stu-id="e9490-253">See also <https://github.com/dotnet/csharplang/issues/406> for a list of open questions.</span></span>
- <span data-ttu-id="e9490-254">詳細規格必須描述在執行時間用來選取要叫用之精確方法的解決機制。</span><span class="sxs-lookup"><span data-stu-id="e9490-254">The detailed specification must describe the resolution mechanism used at runtime to select the precise method to be invoked.</span></span>
- <span data-ttu-id="e9490-255">新編譯器所產生並由舊版編譯器所使用之中繼資料的互動，必須詳細地加以處理。</span><span class="sxs-lookup"><span data-stu-id="e9490-255">The interaction of metadata produced by new compilers and consumed by older compilers needs to be worked out in detail.</span></span> <span data-ttu-id="e9490-256">例如，我們需要確保使用的中繼資料標記法不會在介面中加入預設的實值，以在舊版編譯器編譯時中斷執行該介面的現有類別。</span><span class="sxs-lookup"><span data-stu-id="e9490-256">For example, we need to ensure that the metadata representation that we use does not cause the addition of a default implementation in an interface to break an existing class that implements that interface when compiled by an older compiler.</span></span> <span data-ttu-id="e9490-257">這可能會影響我們可以使用的中繼資料標記法。</span><span class="sxs-lookup"><span data-stu-id="e9490-257">This may affect the metadata representation that we can use.</span></span>
- <span data-ttu-id="e9490-258">設計必須考慮與其他語言的互通性，以及其他語言的現有編譯器。</span><span class="sxs-lookup"><span data-stu-id="e9490-258">The design must consider interoperation with other languages and existing compilers for other languages.</span></span>

## <a name="resolved-questions"></a><span data-ttu-id="e9490-259">解決的問題</span><span class="sxs-lookup"><span data-stu-id="e9490-259">Resolved Questions</span></span>

### <a name="abstract-override"></a><span data-ttu-id="e9490-260">抽象覆寫</span><span class="sxs-lookup"><span data-stu-id="e9490-260">Abstract Override</span></span>

<span data-ttu-id="e9490-261">較舊的草稿規格包含「reabstract」繼承方法的能力：</span><span class="sxs-lookup"><span data-stu-id="e9490-261">The earlier draft spec contained the ability to "reabstract" an inherited method:</span></span>

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

<span data-ttu-id="e9490-262">我在2017-03-20 的注意事項中顯示我們決定不允許這種情況。</span><span class="sxs-lookup"><span data-stu-id="e9490-262">My notes for 2017-03-20 showed that we decided not to allow this.</span></span> <span data-ttu-id="e9490-263">不過，它至少有兩個使用案例：</span><span class="sxs-lookup"><span data-stu-id="e9490-263">However, there are at least two use cases for it:</span></span>

1. <span data-ttu-id="e9490-264">JAVA Api，這項功能的某些使用者希望能夠互通，這取決於此工具。</span><span class="sxs-lookup"><span data-stu-id="e9490-264">The Java APIs, with which some users of this feature hope to interoperate, depend on this facility.</span></span>
2. <span data-ttu-id="e9490-265">透過這種方式設計*特性*的優勢。</span><span class="sxs-lookup"><span data-stu-id="e9490-265">Programming with *traits* benefits from this.</span></span> <span data-ttu-id="e9490-266">Reabstraction 是「特性」語言功能（ https://en.wikipedia.org/wiki/Trait_(computer_programming))的其中一個元素。</span><span class="sxs-lookup"><span data-stu-id="e9490-266">Reabstraction is one of the elements of the "traits" language feature (https://en.wikipedia.org/wiki/Trait_(computer_programming)).</span></span> <span data-ttu-id="e9490-267">類別允許下列各項：</span><span class="sxs-lookup"><span data-stu-id="e9490-267">The following is permitted with classes:</span></span>

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

<span data-ttu-id="e9490-268">不幸的是，除非允許，否則無法將此程式碼重構為一組介面（特性）。</span><span class="sxs-lookup"><span data-stu-id="e9490-268">Unfortunately this code cannot be refactored as a set of interfaces (traits) unless this is permitted.</span></span> <span data-ttu-id="e9490-269">根據*貪婪的 Jared 準則*，它應該是允許的。</span><span class="sxs-lookup"><span data-stu-id="e9490-269">By the *Jared principle of greed*, it should be permitted.</span></span>

> <span data-ttu-id="e9490-270">***已關閉的問題：*** 是否應允許 reabstraction？</span><span class="sxs-lookup"><span data-stu-id="e9490-270">***Closed issue:*** Should reabstraction be permitted?</span></span> <span data-ttu-id="e9490-271">肯定我的記事錯誤。</span><span class="sxs-lookup"><span data-stu-id="e9490-271">[YES] My notes were wrong.</span></span> <span data-ttu-id="e9490-272">[LDM 附注](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md)指出介面中允許 reabstraction。</span><span class="sxs-lookup"><span data-stu-id="e9490-272">The [LDM notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md) say that reabstraction is permitted in an interface.</span></span> <span data-ttu-id="e9490-273">不在類別中。</span><span class="sxs-lookup"><span data-stu-id="e9490-273">Not in a class.</span></span>

### <a name="virtual-modifier-vs-sealed-modifier"></a><span data-ttu-id="e9490-274">虛擬修飾詞 vs Sealed 修飾詞</span><span class="sxs-lookup"><span data-stu-id="e9490-274">Virtual Modifier vs Sealed Modifier</span></span>

<span data-ttu-id="e9490-275">從[Aleksey Tsingauz](https://github.com/AlekseyTs)：</span><span class="sxs-lookup"><span data-stu-id="e9490-275">From [Aleksey Tsingauz](https://github.com/AlekseyTs):</span></span>

> <span data-ttu-id="e9490-276">我們決定在介面成員上允許明確陳述的修飾詞，除非有理由不允許其中一部分。</span><span class="sxs-lookup"><span data-stu-id="e9490-276">We decided to allow modifiers explicitly stated on interface members, unless there is a reason to disallow some of them.</span></span> <span data-ttu-id="e9490-277">這在虛擬修飾詞方面有一個有趣的問題。</span><span class="sxs-lookup"><span data-stu-id="e9490-277">This brings an interesting question around virtual modifier.</span></span> <span data-ttu-id="e9490-278">在具有預設執行的成員上是否需要？</span><span class="sxs-lookup"><span data-stu-id="e9490-278">Should it be required on members with default implementation?</span></span>
>
> <span data-ttu-id="e9490-279">我們可以說：</span><span class="sxs-lookup"><span data-stu-id="e9490-279">We could say that:</span></span>
>
> - <span data-ttu-id="e9490-280">如果沒有任何執行中，且未指定虛擬和，則會假設成員是抽象的。</span><span class="sxs-lookup"><span data-stu-id="e9490-280">if there is no implementation and neither virtual, nor sealed are specified, we assume the member is abstract.</span></span>
> - <span data-ttu-id="e9490-281">如果有一個實作為，而且沒有指定抽象，也沒有密封，則我們假設成員是虛擬的。</span><span class="sxs-lookup"><span data-stu-id="e9490-281">if there is an implementation and neither abstract, nor sealed are specified, we assume the member is virtual.</span></span>
> - <span data-ttu-id="e9490-282">必須有 sealed 修飾詞，才能讓方法既不是虛擬，也不是抽象的。</span><span class="sxs-lookup"><span data-stu-id="e9490-282">sealed modifier is required to make a method neither virtual, nor abstract.</span></span>
>
> <span data-ttu-id="e9490-283">或者，我們也可以說虛擬成員需要虛擬修飾詞。</span><span class="sxs-lookup"><span data-stu-id="e9490-283">Alternatively, we could say that virtual modifier is required for a virtual member.</span></span> <span data-ttu-id="e9490-284">也就是，如果有不是以虛擬修飾詞明確標記的「執行」成員，它既不是虛擬，也不是抽象的。</span><span class="sxs-lookup"><span data-stu-id="e9490-284">I.e, if there is a member with implementation not explicitly marked with virtual modifier, it is neither virtual, nor abstract.</span></span> <span data-ttu-id="e9490-285">當方法從類別移至介面時，這個方法可能會提供更好的體驗：</span><span class="sxs-lookup"><span data-stu-id="e9490-285">This approach might provide better experience when a method is moved from a class to an interface:</span></span>
>
> - <span data-ttu-id="e9490-286">抽象方法會保持抽象。</span><span class="sxs-lookup"><span data-stu-id="e9490-286">an abstract method stays abstract.</span></span>
> - <span data-ttu-id="e9490-287">虛擬方法會維持虛擬的狀態。</span><span class="sxs-lookup"><span data-stu-id="e9490-287">a virtual method stays virtual.</span></span>
> - <span data-ttu-id="e9490-288">沒有任何修飾詞的方法不會維持虛擬和抽象。</span><span class="sxs-lookup"><span data-stu-id="e9490-288">a method without any modifier stays neither virtual, nor abstract.</span></span>
> - <span data-ttu-id="e9490-289">sealed 修飾詞不能套用至非覆寫的方法。</span><span class="sxs-lookup"><span data-stu-id="e9490-289">sealed modifier cannot be applied to a method that is not an override.</span></span>
>
> <span data-ttu-id="e9490-290">你覺得怎麼樣？</span><span class="sxs-lookup"><span data-stu-id="e9490-290">What do you think?</span></span>

> <span data-ttu-id="e9490-291">***已關閉的問題：*** 具體的方法（搭配實作為）是否應隱含 `virtual`？</span><span class="sxs-lookup"><span data-stu-id="e9490-291">***Closed Issue:*** Should a concrete method (with implementation) be implicitly `virtual`?</span></span> <span data-ttu-id="e9490-292">肯定</span><span class="sxs-lookup"><span data-stu-id="e9490-292">[YES]</span></span>

<span data-ttu-id="e9490-293">***決策：*** 在 LDM 2017-04-05 中進行：</span><span class="sxs-lookup"><span data-stu-id="e9490-293">***Decisions:*** Made in the LDM 2017-04-05:</span></span>

1. <span data-ttu-id="e9490-294">非虛擬應該透過 `sealed` 或 `private`明確表示。</span><span class="sxs-lookup"><span data-stu-id="e9490-294">non-virtual should be explicitly expressed through `sealed` or `private`.</span></span>
2. <span data-ttu-id="e9490-295">`sealed` 是讓介面實例成員成為非虛擬主體的關鍵字</span><span class="sxs-lookup"><span data-stu-id="e9490-295">`sealed` is the keyword to make interface instance members with bodies non-virtual</span></span>
3. <span data-ttu-id="e9490-296">我們想要允許介面中的所有修飾詞</span><span class="sxs-lookup"><span data-stu-id="e9490-296">We want to allow all modifiers in interfaces</span></span>  
4. <span data-ttu-id="e9490-297">介面成員的預設存取範圍是公用的，包括巢狀型別</span><span class="sxs-lookup"><span data-stu-id="e9490-297">Default accessibility for interface members is public, including nested types</span></span>
5. <span data-ttu-id="e9490-298">介面中的私用函式成員會隱含地密封，而且不允許 `sealed`。</span><span class="sxs-lookup"><span data-stu-id="e9490-298">private function members in interfaces are implicitly sealed, and `sealed` is not permitted on them.</span></span>
6. <span data-ttu-id="e9490-299">私用類別（在介面中）是允許的，而且可以密封，也就是以 sealed 的類別意義來密封。</span><span class="sxs-lookup"><span data-stu-id="e9490-299">Private classes (in interfaces) are permitted and can be sealed, and that means sealed in the class sense of sealed.</span></span>
7. <span data-ttu-id="e9490-300">缺少良好的提案，介面或其成員上仍然不允許部分。</span><span class="sxs-lookup"><span data-stu-id="e9490-300">Absent a good proposal, partial is still not allowed on interfaces or their members.</span></span>

### <a name="binary-compatibility-1"></a><span data-ttu-id="e9490-301">二進位相容性1</span><span class="sxs-lookup"><span data-stu-id="e9490-301">Binary Compatibility 1</span></span>

<span data-ttu-id="e9490-302">當程式庫提供預設的實作為時</span><span class="sxs-lookup"><span data-stu-id="e9490-302">When a library provides a default implementation</span></span>

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

<span data-ttu-id="e9490-303">我們瞭解 `C` 中的 `I1.M` 的執行是 `I1.M`。</span><span class="sxs-lookup"><span data-stu-id="e9490-303">We understand that the implementation of `I1.M` in `C` is `I1.M`.</span></span> <span data-ttu-id="e9490-304">如果包含 `I2` 的元件依下列方式變更並重新編譯，該怎麼辦</span><span class="sxs-lookup"><span data-stu-id="e9490-304">What if the assembly containing `I2` is changed as follows and recompiled</span></span>

```csharp
interface I2 : I1
{
    override void M() { Impl2 }
}
```

<span data-ttu-id="e9490-305">但是 `C` 不會重新編譯。</span><span class="sxs-lookup"><span data-stu-id="e9490-305">but `C` is not recompiled.</span></span> <span data-ttu-id="e9490-306">當程式執行時，會發生什麼事？</span><span class="sxs-lookup"><span data-stu-id="e9490-306">What happens when the program is run?</span></span> <span data-ttu-id="e9490-307">`(C as I1).M()` 的調用</span><span class="sxs-lookup"><span data-stu-id="e9490-307">An invocation of `(C as I1).M()`</span></span>

1. <span data-ttu-id="e9490-308">執行 `I1.M`</span><span class="sxs-lookup"><span data-stu-id="e9490-308">Runs `I1.M`</span></span>
2. <span data-ttu-id="e9490-309">執行 `I2.M`</span><span class="sxs-lookup"><span data-stu-id="e9490-309">Runs `I2.M`</span></span>
3. <span data-ttu-id="e9490-310">擲回某種類型的執行階段錯誤</span><span class="sxs-lookup"><span data-stu-id="e9490-310">Throws some kind of runtime error</span></span>

<span data-ttu-id="e9490-311">***決策：*** 設為2017-04-11： `I2.M`執行，這是在執行時間明確的最明確覆寫。</span><span class="sxs-lookup"><span data-stu-id="e9490-311">***Decision:*** Made 2017-04-11: Runs `I2.M`, which is the unambiguously most specific override at runtime.</span></span>

### <a name="event-accessors-closed"></a><span data-ttu-id="e9490-312">事件存取子（已關閉）</span><span class="sxs-lookup"><span data-stu-id="e9490-312">Event accessors (closed)</span></span>

> <span data-ttu-id="e9490-313">***已關閉的問題：*** 事件是否可以覆寫為 "分段"？</span><span class="sxs-lookup"><span data-stu-id="e9490-313">***Closed Issue:*** Can an event be overridden "piecewise"?</span></span>

<span data-ttu-id="e9490-314">請考慮下列情況：</span><span class="sxs-lookup"><span data-stu-id="e9490-314">Consider this case:</span></span>

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

<span data-ttu-id="e9490-315">不允許此事件的「部分」執行，因為在類別中，事件宣告的語法不允許只有一個存取子。必須同時提供（或兩者皆非）。</span><span class="sxs-lookup"><span data-stu-id="e9490-315">This "partial" implementation of the event is not permitted because, as in a class, the syntax for an event declaration does not permit only one accessor; both (or neither) must be provided.</span></span> <span data-ttu-id="e9490-316">您可以藉由允許語法中的抽象 remove 存取子在不存在主體時隱含抽象化，來完成相同的動作：</span><span class="sxs-lookup"><span data-stu-id="e9490-316">You could accomplish the same thing by permitting the abstract remove accessor in the syntax to be implicitly abstract by the absence of a body:</span></span>

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

<span data-ttu-id="e9490-317">請注意，*這是新的（建議的）語法*。</span><span class="sxs-lookup"><span data-stu-id="e9490-317">Note that *this is a new (proposed) syntax*.</span></span> <span data-ttu-id="e9490-318">在目前的文法中，事件存取子具有強制性主體。</span><span class="sxs-lookup"><span data-stu-id="e9490-318">In the current grammar, event accessors have a mandatory body.</span></span>

> <span data-ttu-id="e9490-319">***已關閉的問題：*** 事件存取子是否可以藉由省略主體（隱含）來抽象化，類似于介面和屬性存取子中的方法在省略主體時的抽象方式？</span><span class="sxs-lookup"><span data-stu-id="e9490-319">***Closed Issue:*** Can an event accessor be (implicitly) abstract by the omission of a body, similarly to the way that methods in interfaces and property accessors are (implicitly) abstract by the omission of a body?</span></span>

<span data-ttu-id="e9490-320">***決策：*** （2017-04-18）否，事件宣告需要兩個具體的存取子（或兩者皆非）。</span><span class="sxs-lookup"><span data-stu-id="e9490-320">***Decision:*** (2017-04-18) No, event declarations require both concrete accessors (or neither).</span></span>

### <a name="reabstraction-in-a-class-closed"></a><span data-ttu-id="e9490-321">在類別中 Reabstraction （已關閉）</span><span class="sxs-lookup"><span data-stu-id="e9490-321">Reabstraction in a Class (closed)</span></span>

<span data-ttu-id="e9490-322">***已關閉的問題：*** 我們應該確認這是允許的（否則新增預設的實作為中斷性變更）：</span><span class="sxs-lookup"><span data-stu-id="e9490-322">***Closed Issue:*** We should confirm that this is permitted (otherwise adding a default implementation would be a breaking change):</span></span>

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

<span data-ttu-id="e9490-323">***決策：*** （2017-04-18）是，將主體新增至介面成員宣告不應中斷 C。</span><span class="sxs-lookup"><span data-stu-id="e9490-323">***Decision:*** (2017-04-18) Yes, adding a body to an interface member declaration shouldn't break C.</span></span>

### <a name="sealed-override-closed"></a><span data-ttu-id="e9490-324">密封覆寫（已關閉）</span><span class="sxs-lookup"><span data-stu-id="e9490-324">Sealed Override (closed)</span></span>

<span data-ttu-id="e9490-325">先前的問題隱含假設 `sealed` 修飾詞可以套用至介面中的 `override`。</span><span class="sxs-lookup"><span data-stu-id="e9490-325">The previous question implicitly assumes that the `sealed` modifier can be applied to an `override` in an interface.</span></span> <span data-ttu-id="e9490-326">這會與草稿規格相抵觸。</span><span class="sxs-lookup"><span data-stu-id="e9490-326">This contradicts the draft specification.</span></span> <span data-ttu-id="e9490-327">是否要允許密封覆寫？</span><span class="sxs-lookup"><span data-stu-id="e9490-327">Do we want to permit sealing an override?</span></span> <span data-ttu-id="e9490-328">應考慮密封的來源和二進位相容性效果。</span><span class="sxs-lookup"><span data-stu-id="e9490-328">Source and binary compatibility effects of sealing should be considered.</span></span>

> <span data-ttu-id="e9490-329">***已關閉的問題：*** 我們是否允許密封覆寫？</span><span class="sxs-lookup"><span data-stu-id="e9490-329">***Closed Issue:*** Should we permit sealing an override?</span></span>

<span data-ttu-id="e9490-330">***決策：*** （2017-04-18）在介面的覆寫上不允許 `sealed`。</span><span class="sxs-lookup"><span data-stu-id="e9490-330">***Decision:*** (2017-04-18) Let's not allowed `sealed` on overrides in interfaces.</span></span> <span data-ttu-id="e9490-331">在介面成員上唯一使用 `sealed`，是在其初始宣告中使其成為非虛擬的。</span><span class="sxs-lookup"><span data-stu-id="e9490-331">The only use of `sealed` on interface members is to make them non-virtual in their initial declaration.</span></span>

### <a name="diamond-inheritance-and-classes-closed"></a><span data-ttu-id="e9490-332">菱形繼承和類別（已關閉）</span><span class="sxs-lookup"><span data-stu-id="e9490-332">Diamond inheritance and classes (closed)</span></span>

<span data-ttu-id="e9490-333">提議的草稿優先于菱形繼承案例中的介面覆寫：</span><span class="sxs-lookup"><span data-stu-id="e9490-333">The draft of the proposal prefers class overrides to interface overrides in diamond inheritance scenarios:</span></span>

> <span data-ttu-id="e9490-334">我們要求每個介面和類別在類型中顯示的覆寫和其直接和間接介面中，都有*最明確*的覆寫。</span><span class="sxs-lookup"><span data-stu-id="e9490-334">We require that every interface and class have a *most specific override* for every interface method among the overrides appearing in the type or its direct and indirect interfaces.</span></span> <span data-ttu-id="e9490-335">*最特定*的覆寫是唯一的覆寫，比其他每個覆寫更明確。</span><span class="sxs-lookup"><span data-stu-id="e9490-335">The *most specific override* is a unique override that is more specific than every other override.</span></span> <span data-ttu-id="e9490-336">如果沒有覆寫，方法本身會被視為最明確的覆寫。</span><span class="sxs-lookup"><span data-stu-id="e9490-336">If there is no override, the method itself is considered the most specific override.</span></span>
>
> <span data-ttu-id="e9490-337">一個覆寫 `M1` 被視為比另一個覆寫*更明確*`M2` 如果 `M1` 是在類型 `T1`上宣告，`M2` 會在類型 `T2`上宣告，而且兩者都是</span><span class="sxs-lookup"><span data-stu-id="e9490-337">One override `M1` is considered *more specific* than another override `M2` if `M1` is declared on type `T1`, `M2` is declared on type `T2`, and either</span></span>
>
> 1. <span data-ttu-id="e9490-338">`T1` 包含其直接或間接介面之間的 `T2`，或</span><span class="sxs-lookup"><span data-stu-id="e9490-338">`T1` contains `T2` among its direct or indirect interfaces, or</span></span>
> 2. <span data-ttu-id="e9490-339">`T2` 是介面類別型，但 `T1` 不是介面類別型。</span><span class="sxs-lookup"><span data-stu-id="e9490-339">`T2` is an interface type but `T1` is not an interface type.</span></span>

<span data-ttu-id="e9490-340">案例如下</span><span class="sxs-lookup"><span data-stu-id="e9490-340">The scenario is this</span></span>

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

<span data-ttu-id="e9490-341">我們應該確認此行為（或其他決定）</span><span class="sxs-lookup"><span data-stu-id="e9490-341">We should confirm this behavior (or decide otherwise)</span></span>

> <span data-ttu-id="e9490-342">***已關閉的問題：*** 確認適用于*大部分特定覆寫*的草稿規格，因為它會套用至混合類別和介面（類別的優先順序高於介面）。</span><span class="sxs-lookup"><span data-stu-id="e9490-342">***Closed Issue:*** Confirm the draft spec, above, for *most specific override* as it applies to mixed classes and interfaces (a class takes priority over an interface).</span></span> <span data-ttu-id="e9490-343">請參閱＜<https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#diamonds-with-classes>＞。</span><span class="sxs-lookup"><span data-stu-id="e9490-343">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#diamonds-with-classes>.</span></span>

### <a name="interface-methods-vs-structs-closed"></a><span data-ttu-id="e9490-344">介面方法 vs 結構（已關閉）</span><span class="sxs-lookup"><span data-stu-id="e9490-344">Interface methods vs structs (closed)</span></span>

<span data-ttu-id="e9490-345">預設介面方法與結構之間有一些不幸的互動。</span><span class="sxs-lookup"><span data-stu-id="e9490-345">There are some unfortunate interactions between default interface methods and structs.</span></span>

```csharp
interface IA
{
    public void M() { }
}
struct S : IA
{
}
```

<span data-ttu-id="e9490-346">請注意，介面成員不會被繼承：</span><span class="sxs-lookup"><span data-stu-id="e9490-346">Note that interface members are not inherited:</span></span>

```csharp
var s = default(S);
s.M(); // error: 'S' does not contain a member 'M'
```

<span data-ttu-id="e9490-347">因此，用戶端必須將結構方塊框為叫用介面方法</span><span class="sxs-lookup"><span data-stu-id="e9490-347">Consequently, the client must box the struct to invoke interface methods</span></span>

```csharp
IA s = default(S); // an S, boxed
s.M(); // ok
```

<span data-ttu-id="e9490-348">以這種方式進行的裝箱，會使 `struct` 類型的主要優點失效。</span><span class="sxs-lookup"><span data-stu-id="e9490-348">Boxing in this way defeats the principal benefits of a `struct` type.</span></span> <span data-ttu-id="e9490-349">此外，任何變化方法都不會有明顯的影響，因為它們是在結構的*封裝複本*上運作：</span><span class="sxs-lookup"><span data-stu-id="e9490-349">Moreover, any mutation methods will have no apparent effect, because they are operating on a *boxed copy* of the struct:</span></span>

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

> <span data-ttu-id="e9490-350">***已關閉的問題：*** 我們可以做的事：</span><span class="sxs-lookup"><span data-stu-id="e9490-350">***Closed Issue:*** What can we do about this:</span></span>
>
> 1. <span data-ttu-id="e9490-351">禁止 `struct` 繼承預設的執行。</span><span class="sxs-lookup"><span data-stu-id="e9490-351">Forbid a `struct` from inheriting a default implementation.</span></span> <span data-ttu-id="e9490-352">在 `struct`中，所有介面方法都會被視為抽象。</span><span class="sxs-lookup"><span data-stu-id="e9490-352">All interface methods would be treated as abstract in a `struct`.</span></span> <span data-ttu-id="e9490-353">之後，我們可能會花一些時間來決定如何讓它更好的效果。</span><span class="sxs-lookup"><span data-stu-id="e9490-353">Then we may take time later to decide how to make it work better.</span></span>
> 2. <span data-ttu-id="e9490-354">提出一些可避免使用裝箱的程式碼產生策略。</span><span class="sxs-lookup"><span data-stu-id="e9490-354">Come up with some kind of code generation strategy that avoids boxing.</span></span> <span data-ttu-id="e9490-355">在 `IB.Increment`之類的方法內，`this` 的類型可能類似于限制為 `IB`的類型參數。</span><span class="sxs-lookup"><span data-stu-id="e9490-355">Inside a method like `IB.Increment`, the type of `this` would perhaps be akin to a type parameter constrained to `IB`.</span></span> <span data-ttu-id="e9490-356">此外，為了避免呼叫端中的裝箱，非抽象方法會繼承自介面。</span><span class="sxs-lookup"><span data-stu-id="e9490-356">In conjunction with that, to avoid boxing in the caller, non-abstract methods would be inherited from interfaces.</span></span> <span data-ttu-id="e9490-357">這可能會大幅增加編譯器和 CLR 執行的工作。</span><span class="sxs-lookup"><span data-stu-id="e9490-357">This may increase compiler and CLR implementation work substantially.</span></span>
> 3. <span data-ttu-id="e9490-358">不必擔心它，而是將它保留為疙瘩。</span><span class="sxs-lookup"><span data-stu-id="e9490-358">Not worry about it and just leave it as a wart.</span></span>
> 4. <span data-ttu-id="e9490-359">還有其他想法嗎？</span><span class="sxs-lookup"><span data-stu-id="e9490-359">Other ideas?</span></span>

<span data-ttu-id="e9490-360">***決策：*** 不必擔心它，而是將它保留為疙瘩。</span><span class="sxs-lookup"><span data-stu-id="e9490-360">***Decision:*** Not worry about it and just leave it as a wart.</span></span> <span data-ttu-id="e9490-361">請參閱＜<https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#structs-and-default-implementations>＞。</span><span class="sxs-lookup"><span data-stu-id="e9490-361">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#structs-and-default-implementations>.</span></span>

### <a name="base-interface-invocations-closed"></a><span data-ttu-id="e9490-362">基底介面調用（已關閉）</span><span class="sxs-lookup"><span data-stu-id="e9490-362">Base interface invocations (closed)</span></span>

<span data-ttu-id="e9490-363">草稿規格建議的語法適用于 JAVA 所啟發的基底介面調用： `Interface.base.M()`。</span><span class="sxs-lookup"><span data-stu-id="e9490-363">The draft spec suggests a syntax for base interface invocations inspired by Java: `Interface.base.M()`.</span></span> <span data-ttu-id="e9490-364">我們必須至少選取初始原型的語法。</span><span class="sxs-lookup"><span data-stu-id="e9490-364">We need to select a syntax, at least for the initial prototype.</span></span> <span data-ttu-id="e9490-365">我的最愛是 `base<Interface>.M()`。</span><span class="sxs-lookup"><span data-stu-id="e9490-365">My favorite is `base<Interface>.M()`.</span></span>

> <span data-ttu-id="e9490-366">***已關閉的問題：*** 基底成員調用的語法為何？</span><span class="sxs-lookup"><span data-stu-id="e9490-366">***Closed Issue:*** What is the syntax for a base member invocation?</span></span>

<span data-ttu-id="e9490-367">***決策：*** 語法為 `base(Interface).M()`。</span><span class="sxs-lookup"><span data-stu-id="e9490-367">***Decision:*** The syntax is `base(Interface).M()`.</span></span> <span data-ttu-id="e9490-368">請參閱＜<https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>＞。</span><span class="sxs-lookup"><span data-stu-id="e9490-368">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>.</span></span> <span data-ttu-id="e9490-369">命名介面必須是基底介面，但不需要是直接基底介面。</span><span class="sxs-lookup"><span data-stu-id="e9490-369">The interface so named must be a base interface, but does not need to be a direct base interface.</span></span>

> <span data-ttu-id="e9490-370">***開啟問題：*** 是否應該在類別成員中允許基底介面調用？</span><span class="sxs-lookup"><span data-stu-id="e9490-370">***Open Issue:*** Should base interface invocations be permitted in class members?</span></span>

<span data-ttu-id="e9490-371">***決策***：是。</span><span class="sxs-lookup"><span data-stu-id="e9490-371">***Decision***: Yes.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>

### <a name="overriding-non-public-interface-members-closed"></a><span data-ttu-id="e9490-372">覆寫非公用介面成員（已關閉）</span><span class="sxs-lookup"><span data-stu-id="e9490-372">Overriding non-public interface members (closed)</span></span>

<span data-ttu-id="e9490-373">在介面中，基底介面的非公用成員會使用 `override` 修飾詞來加以覆寫。</span><span class="sxs-lookup"><span data-stu-id="e9490-373">In an interface, non-public members from base interfaces are overridden using the `override` modifier.</span></span> <span data-ttu-id="e9490-374">如果它是名為包含成員之介面的「明確」覆寫，則會省略存取修飾詞。</span><span class="sxs-lookup"><span data-stu-id="e9490-374">If it is an "explicit" override that names the interface containing the member, the access modifier is omitted.</span></span>

> <span data-ttu-id="e9490-375">***已關閉的問題：*** 如果它是不會命名介面的「隱含」覆寫，則存取修飾詞是否必須相符？</span><span class="sxs-lookup"><span data-stu-id="e9490-375">***Closed Issue:*** If it is an "implicit" override that does not name the interface, does the access modifier have to match?</span></span>

<span data-ttu-id="e9490-376">***決策：*** 只有公用成員可以隱含地覆寫，且存取必須相符。</span><span class="sxs-lookup"><span data-stu-id="e9490-376">***Decision:*** Only public members may be implicitly overridden, and the access must match.</span></span> <span data-ttu-id="e9490-377">請參閱＜<https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>＞。</span><span class="sxs-lookup"><span data-stu-id="e9490-377">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.</span></span>

> <span data-ttu-id="e9490-378">***開啟問題：*** 存取修飾詞是否需要、選擇性或在明確覆寫上省略，例如 `override void IB.M() {}`？</span><span class="sxs-lookup"><span data-stu-id="e9490-378">***Open Issue:*** Is the access modifier required, optional, or omitted on an explicit override such as `override void IB.M() {}`?</span></span>

> <span data-ttu-id="e9490-379">***開啟問題：*** 在明確覆寫（例如 `void IB.M() {}`）上，是否 `override` 必要、選擇性或省略？</span><span class="sxs-lookup"><span data-stu-id="e9490-379">***Open Issue:*** Is `override` required, optional, or omitted on an explicit override such as `void IB.M() {}`?</span></span>

<span data-ttu-id="e9490-380">如何在類別中執行非公用介面成員？</span><span class="sxs-lookup"><span data-stu-id="e9490-380">How does one implement a non-public interface member in a class?</span></span> <span data-ttu-id="e9490-381">也許它必須明確地完成嗎？</span><span class="sxs-lookup"><span data-stu-id="e9490-381">Perhaps it must be done explicitly?</span></span>

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

> <span data-ttu-id="e9490-382">***已關閉的問題：*** 如何在類別中執行非公用介面成員？</span><span class="sxs-lookup"><span data-stu-id="e9490-382">***Closed Issue:*** How does one implement a non-public interface member in a class?</span></span>

<span data-ttu-id="e9490-383">***決策：*** 您只能明確地執行非公用介面成員。</span><span class="sxs-lookup"><span data-stu-id="e9490-383">***Decision:*** You can only implement non-public interface members explicitly.</span></span> <span data-ttu-id="e9490-384">請參閱＜<https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>＞。</span><span class="sxs-lookup"><span data-stu-id="e9490-384">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.</span></span>

<span data-ttu-id="e9490-385">***決策***：介面成員上不允許 `override` 關鍵字。</span><span class="sxs-lookup"><span data-stu-id="e9490-385">***Decision***: No `override` keyword permitted on interface members.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>

### <a name="binary-compatibility-2-closed"></a><span data-ttu-id="e9490-386">二進位相容性2（已關閉）</span><span class="sxs-lookup"><span data-stu-id="e9490-386">Binary Compatibility 2 (closed)</span></span>

<span data-ttu-id="e9490-387">請考慮下列程式碼，其中的每個類型都在不同的元件中</span><span class="sxs-lookup"><span data-stu-id="e9490-387">Consider the following code in which each type is in a separate assembly</span></span>

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

<span data-ttu-id="e9490-388">我們瞭解 `C` 中的 `I1.M` 的執行是 `I2.M`。</span><span class="sxs-lookup"><span data-stu-id="e9490-388">We understand that the implementation of `I1.M` in `C` is `I2.M`.</span></span> <span data-ttu-id="e9490-389">如果包含 `I3` 的元件依下列方式變更並重新編譯，該怎麼辦</span><span class="sxs-lookup"><span data-stu-id="e9490-389">What if the assembly containing `I3` is changed as follows and recompiled</span></span>

```csharp
interface I3 : I1
{
    override void M() { Impl3 }
}
```

<span data-ttu-id="e9490-390">但是 `C` 不會重新編譯。</span><span class="sxs-lookup"><span data-stu-id="e9490-390">but `C` is not recompiled.</span></span> <span data-ttu-id="e9490-391">當程式執行時，會發生什麼事？</span><span class="sxs-lookup"><span data-stu-id="e9490-391">What happens when the program is run?</span></span> <span data-ttu-id="e9490-392">`(C as I1).M()` 的調用</span><span class="sxs-lookup"><span data-stu-id="e9490-392">An invocation of `(C as I1).M()`</span></span>

1. <span data-ttu-id="e9490-393">執行 `I1.M`</span><span class="sxs-lookup"><span data-stu-id="e9490-393">Runs `I1.M`</span></span>
2. <span data-ttu-id="e9490-394">執行 `I2.M`</span><span class="sxs-lookup"><span data-stu-id="e9490-394">Runs `I2.M`</span></span>
3. <span data-ttu-id="e9490-395">執行 `I3.M`</span><span class="sxs-lookup"><span data-stu-id="e9490-395">Runs `I3.M`</span></span>
4. <span data-ttu-id="e9490-396">可能是2或3，有決定性</span><span class="sxs-lookup"><span data-stu-id="e9490-396">Either 2 or 3, deterministically</span></span>
5. <span data-ttu-id="e9490-397">擲回某種類型的執行時間例外狀況</span><span class="sxs-lookup"><span data-stu-id="e9490-397">Throws some kind of runtime exception</span></span>

<span data-ttu-id="e9490-398">***決策***：擲回例外狀況（5）。</span><span class="sxs-lookup"><span data-stu-id="e9490-398">***Decision***: Throw an exception (5).</span></span> <span data-ttu-id="e9490-399">請參閱＜<https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#issues-in-default-interface-methods>＞。</span><span class="sxs-lookup"><span data-stu-id="e9490-399">See <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#issues-in-default-interface-methods>.</span></span>

### <a name="permit-partial-in-interface-closed"></a><span data-ttu-id="e9490-400">允許在介面中 `partial` 嗎？</span><span class="sxs-lookup"><span data-stu-id="e9490-400">Permit `partial` in interface?</span></span> <span data-ttu-id="e9490-401">已</span><span class="sxs-lookup"><span data-stu-id="e9490-401">(closed)</span></span>

<span data-ttu-id="e9490-402">假設介面可能會以類似抽象類別使用方式的方式來使用，將其宣告 `partial`可能會很有用。</span><span class="sxs-lookup"><span data-stu-id="e9490-402">Given that interfaces may be used in ways analogous to the way abstract classes are used, it may be useful to declare them `partial`.</span></span> <span data-ttu-id="e9490-403">這在表面上特別有用。</span><span class="sxs-lookup"><span data-stu-id="e9490-403">This would be particularly useful in the face of generators.</span></span>

> <span data-ttu-id="e9490-404">***提案：*** 移除介面和介面成員無法 `partial`宣告的語言限制。</span><span class="sxs-lookup"><span data-stu-id="e9490-404">***Proposal:*** Remove the language restriction that interfaces and members of interfaces may not be declared `partial`.</span></span>

<span data-ttu-id="e9490-405">***決策***：是。</span><span class="sxs-lookup"><span data-stu-id="e9490-405">***Decision***: Yes.</span></span> <span data-ttu-id="e9490-406">請參閱＜<https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>＞。</span><span class="sxs-lookup"><span data-stu-id="e9490-406">See <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>.</span></span>

### <a name="main-in-an-interface-closed"></a><span data-ttu-id="e9490-407">在介面中 `Main`？</span><span class="sxs-lookup"><span data-stu-id="e9490-407">`Main` in an interface?</span></span> <span data-ttu-id="e9490-408">已</span><span class="sxs-lookup"><span data-stu-id="e9490-408">(closed)</span></span>

> <span data-ttu-id="e9490-409">***開啟問題：*** 介面中的 `static Main` 方法是否為程式的進入點的候選項目？</span><span class="sxs-lookup"><span data-stu-id="e9490-409">***Open Issue:*** Is a `static Main` method in an interface a candidate to be the program's entry point?</span></span>

<span data-ttu-id="e9490-410">***決策***：是。</span><span class="sxs-lookup"><span data-stu-id="e9490-410">***Decision***: Yes.</span></span> <span data-ttu-id="e9490-411">請參閱＜<https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#main-in-an-interface>＞。</span><span class="sxs-lookup"><span data-stu-id="e9490-411">See <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#main-in-an-interface>.</span></span>

### <a name="confirm-intent-to-support-public-non-virtual-methods-closed"></a><span data-ttu-id="e9490-412">確認意圖以支援公用非虛擬方法（已關閉）</span><span class="sxs-lookup"><span data-stu-id="e9490-412">Confirm intent to support public non-virtual methods (closed)</span></span>

<span data-ttu-id="e9490-413">我們是否可以確認（或反轉）在介面中允許非虛擬公用方法的決策？</span><span class="sxs-lookup"><span data-stu-id="e9490-413">Can we please confirm (or reverse) our decision to permit non-virtual public methods in an interface?</span></span>

```csharp
interface IA
{
    public sealed void M() { }
}
```

> <span data-ttu-id="e9490-414">***半關閉問題：*** （2017-04-18）我們認為它會很有用，但會回到此。</span><span class="sxs-lookup"><span data-stu-id="e9490-414">***Semi-Closed Issue:*** (2017-04-18) We think it is going to be useful, but will come back to it.</span></span> <span data-ttu-id="e9490-415">這是心理模型往返區塊。</span><span class="sxs-lookup"><span data-stu-id="e9490-415">This is a mental model tripping block.</span></span>

<span data-ttu-id="e9490-416">***決策***：是。</span><span class="sxs-lookup"><span data-stu-id="e9490-416">***Decision***: Yes.</span></span> <span data-ttu-id="e9490-417"><https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#confirm-that-we-support-public-non-virtual-methods>第 1 課：建立 Windows Azure 儲存體物件{2}。</span><span class="sxs-lookup"><span data-stu-id="e9490-417"><https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#confirm-that-we-support-public-non-virtual-methods>.</span></span>

### <a name="does-an-override-in-an-interface-introduce-a-new-member-closed"></a><span data-ttu-id="e9490-418">介面中的 `override` 會引進新的成員嗎？</span><span class="sxs-lookup"><span data-stu-id="e9490-418">Does an `override` in an interface introduce a new member?</span></span> <span data-ttu-id="e9490-419">已</span><span class="sxs-lookup"><span data-stu-id="e9490-419">(closed)</span></span>

<span data-ttu-id="e9490-420">有幾種方式可以觀察覆寫宣告是否導入了新的成員。</span><span class="sxs-lookup"><span data-stu-id="e9490-420">There are a few ways to observe whether an override declaration introduces a new member or not.</span></span>

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

> <span data-ttu-id="e9490-421">***開啟問題：*** 介面中的覆寫宣告會引進新的成員嗎？</span><span class="sxs-lookup"><span data-stu-id="e9490-421">***Open Issue:*** Does an override declaration in an interface introduce a new member?</span></span> <span data-ttu-id="e9490-422">已</span><span class="sxs-lookup"><span data-stu-id="e9490-422">(closed)</span></span>

<span data-ttu-id="e9490-423">在類別中，覆寫方法在某些感知中是「可見」。</span><span class="sxs-lookup"><span data-stu-id="e9490-423">In a class, an overriding method is "visible" in some senses.</span></span> <span data-ttu-id="e9490-424">例如，其參數的名稱會優先于覆寫方法中的參數名稱。</span><span class="sxs-lookup"><span data-stu-id="e9490-424">For example, the names of its parameters take precedence over the names of parameters in the overridden method.</span></span> <span data-ttu-id="e9490-425">您可以在介面中複製該行為，因為一律會有最明確的覆寫。</span><span class="sxs-lookup"><span data-stu-id="e9490-425">It may be possible to duplicate that behavior in interfaces, as there is always a most specific override.</span></span> <span data-ttu-id="e9490-426">但要複製該行為嗎？</span><span class="sxs-lookup"><span data-stu-id="e9490-426">But do we want to duplicate that behavior?</span></span>

<span data-ttu-id="e9490-427">此外，也可以「覆寫」覆寫方法？</span><span class="sxs-lookup"><span data-stu-id="e9490-427">Also, it is possible to "override" an override method?</span></span> <span data-ttu-id="e9490-428">想法</span><span class="sxs-lookup"><span data-stu-id="e9490-428">[Moot]</span></span>

<span data-ttu-id="e9490-429">***決策***：介面成員上不允許 `override` 關鍵字。</span><span class="sxs-lookup"><span data-stu-id="e9490-429">***Decision***: No `override` keyword permitted on interface members.</span></span> <span data-ttu-id="e9490-430"><https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>第 1 課：建立 Windows Azure 儲存體物件{2}。</span><span class="sxs-lookup"><span data-stu-id="e9490-430"><https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>.</span></span>

### <a name="properties-with-a-private-accessor-closed"></a><span data-ttu-id="e9490-431">具有私用存取子的屬性（已關閉）</span><span class="sxs-lookup"><span data-stu-id="e9490-431">Properties with a private accessor (closed)</span></span>

<span data-ttu-id="e9490-432">我們假設私用成員不是虛擬的，而且不允許虛擬和私用的組合。</span><span class="sxs-lookup"><span data-stu-id="e9490-432">We say that private members are not virtual, and the combination of virtual and private is disallowed.</span></span> <span data-ttu-id="e9490-433">但是關於具有私用存取子的屬性呢？</span><span class="sxs-lookup"><span data-stu-id="e9490-433">But what about a property with a private accessor?</span></span>

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

<span data-ttu-id="e9490-434">這是允許的嗎？</span><span class="sxs-lookup"><span data-stu-id="e9490-434">Is this allowed?</span></span> <span data-ttu-id="e9490-435">`set` 存取子是否 `virtual`？</span><span class="sxs-lookup"><span data-stu-id="e9490-435">Is the `set` accessor here `virtual` or not?</span></span> <span data-ttu-id="e9490-436">可以在可存取的位置覆寫嗎？</span><span class="sxs-lookup"><span data-stu-id="e9490-436">Can it be overridden where it is accessible?</span></span> <span data-ttu-id="e9490-437">下列動作是否會隱含地僅執行 `get` 存取子？</span><span class="sxs-lookup"><span data-stu-id="e9490-437">Does the following implicitly implement only the `get` accessor?</span></span>

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

<span data-ttu-id="e9490-438">下列原因會因為 IA 而發生錯誤。P. set 不是虛擬的，也不能存取？</span><span class="sxs-lookup"><span data-stu-id="e9490-438">Is the following presumably an error because IA.P.set isn't virtual and also because it isn't accessible?</span></span>

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

<span data-ttu-id="e9490-439">***決策***：第一個範例看起來有效，而最後一個則不是。</span><span class="sxs-lookup"><span data-stu-id="e9490-439">***Decision***: The first example looks valid, while the last does not.</span></span> <span data-ttu-id="e9490-440">這會類似已解決其在中的C#運作方式。</span><span class="sxs-lookup"><span data-stu-id="e9490-440">This is resolved analogously to how it already works in C#.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#properties-with-a-private-accessor>

### <a name="base-interface-invocations-round-2-closed"></a><span data-ttu-id="e9490-441">基底介面調用，四捨五入2（已關閉）</span><span class="sxs-lookup"><span data-stu-id="e9490-441">Base Interface Invocations, round 2 (closed)</span></span>

<span data-ttu-id="e9490-442">我們先前的「解決方法」，說明如何處理基底調用，實際上並不提供足夠的表達。</span><span class="sxs-lookup"><span data-stu-id="e9490-442">Our previous "resolution" to how to handle base invocations doesn't actually provide sufficient expressiveness.</span></span> <span data-ttu-id="e9490-443">與 JAVA 不同的是C# ，在和 CLR 中，您必須同時指定包含方法宣告的介面，以及要叫用的實作為位置。</span><span class="sxs-lookup"><span data-stu-id="e9490-443">It turns out that in C# and the CLR, unlike Java, you need to specify both the interface containing the method declaration and the location of the implementation you want to invoke.</span></span>

<span data-ttu-id="e9490-444">我建議在介面中進行基底呼叫的下列語法。</span><span class="sxs-lookup"><span data-stu-id="e9490-444">I propose the following syntax for base calls in interfaces.</span></span> <span data-ttu-id="e9490-445">我不熟悉它，但它說明了什麼語法必須能夠表達：</span><span class="sxs-lookup"><span data-stu-id="e9490-445">I’m not in love with it, but it illustrates what any syntax must be able to express:</span></span>

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

<span data-ttu-id="e9490-446">如果沒有任何不明確的情況，您可以更輕鬆地撰寫</span><span class="sxs-lookup"><span data-stu-id="e9490-446">If there is no ambiguity, you can write it more simply</span></span>

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

<span data-ttu-id="e9490-447">Or</span><span class="sxs-lookup"><span data-stu-id="e9490-447">Or</span></span>

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

<span data-ttu-id="e9490-448">Or</span><span class="sxs-lookup"><span data-stu-id="e9490-448">Or</span></span>

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

<span data-ttu-id="e9490-449">***決策***：決定 `base(N.I1<T>).M(s)`conceding，如果我們有調用系結，稍後可能會發生問題。</span><span class="sxs-lookup"><span data-stu-id="e9490-449">***Decision***: Decided on `base(N.I1<T>).M(s)`, conceding that if we have an invocation binding there may be problem here later on.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md#default-interface-implementations>

### <a name="warning-for-struct-not-implementing-default-method-closed"></a><span data-ttu-id="e9490-450">結構未執行預設方法的警告？</span><span class="sxs-lookup"><span data-stu-id="e9490-450">Warning for struct not implementing default method?</span></span> <span data-ttu-id="e9490-451">已</span><span class="sxs-lookup"><span data-stu-id="e9490-451">(closed)</span></span>

<span data-ttu-id="e9490-452">@vancem 判斷提示，如果實值型別宣告無法覆寫某些介面方法，就應該考慮產生警告，即使它會從介面繼承該方法的執行。</span><span class="sxs-lookup"><span data-stu-id="e9490-452">@vancem asserts that we should seriously consider producing a warning if a value type declaration fails to override some interface method, even if it would inherit an implementation of that method from an interface.</span></span> <span data-ttu-id="e9490-453">因為它會造成裝箱和破壞限制的呼叫。</span><span class="sxs-lookup"><span data-stu-id="e9490-453">Because it causes boxing and undermines constrained calls.</span></span>

<span data-ttu-id="e9490-454">***決策***：這看起來就像是更適合分析器的專案。</span><span class="sxs-lookup"><span data-stu-id="e9490-454">***Decision***: This seems like something more suited for an analyzer.</span></span> <span data-ttu-id="e9490-455">此警告似乎也會產生雜訊，因為即使從未呼叫過預設介面方法，也不會發生任何裝箱。</span><span class="sxs-lookup"><span data-stu-id="e9490-455">It also seems like this warning could be noisy, since it would fire even if the default interface method is never called and no boxing will ever occur.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#warning-for-struct-not-implementing-default-method>

### <a name="interface-static-constructors-closed"></a><span data-ttu-id="e9490-456">介面靜態的函式（已關閉）</span><span class="sxs-lookup"><span data-stu-id="e9490-456">Interface static constructors (closed)</span></span>

<span data-ttu-id="e9490-457">何時會執行介面靜態函式？</span><span class="sxs-lookup"><span data-stu-id="e9490-457">When are interface static constructors run?</span></span>  <span data-ttu-id="e9490-458">目前的 CLI 草稿建議在存取第一個靜態方法或欄位時發生。</span><span class="sxs-lookup"><span data-stu-id="e9490-458">The current CLI draft proposes that it occurs when the first static method or field is accessed.</span></span> <span data-ttu-id="e9490-459">如果沒有這兩個專案，則可能永遠不會執行？</span><span class="sxs-lookup"><span data-stu-id="e9490-459">If there are neither of those then it might never be run??</span></span>

<span data-ttu-id="e9490-460">[2018-10-09 CLR 小組提出「要鏡像我們為 valuetypes 做的事（對每個實例方法的存取權的 .cctor 檢查）」</span><span class="sxs-lookup"><span data-stu-id="e9490-460">[2018-10-09 The CLR team proposes "Going to mirror what we do for valuetypes (cctor check on access to each instance method)"]</span></span>

<span data-ttu-id="e9490-461">***決策***：如果靜態的函式不是 `beforefieldinit`，靜態的函式也會在進入實例方法時執行，在這種情況下，會先執行靜態的函式，然後再存取第一個靜態欄位。</span><span class="sxs-lookup"><span data-stu-id="e9490-461">***Decision***: Static constructors are also run on entry to instance methods, if the static constructor was not `beforefieldinit`, in which case static constructors are run before access to the first static field.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#when-are-interface-static-constructors-run>

## <a name="design-meetings"></a><span data-ttu-id="e9490-462">設計會議</span><span class="sxs-lookup"><span data-stu-id="e9490-462">Design meetings</span></span>

<span data-ttu-id="e9490-463">[2017-03-08 Ldm 會議筆記](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-08.md)
[2017-03-21 LDM 會議筆記](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md)
[2017-03-23 「預設介面方法的 CLR 行為](https://github.com/dotnet/csharplang/blob/master/meetings/2017/CLR-2017-03-23.md)」
[2017-04-05 LDM 會議筆記](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md)
[2017-04-11 ldm 會議筆記](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-11.md)
[2017-04-18 ldm](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md)會議筆記
2017-04-19 [2017-04-19 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md) ldm 會議筆記
[2017-05-17 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-17.md) [2017-05-31 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md) [2017-05-17 ldm會議筆記](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-06-14.md)
[2018-10-17 Ldm 會議筆記](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
[2018-11-14 ldm 會議筆記](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md)

</span><span class="sxs-lookup"><span data-stu-id="e9490-463">[2017-03-08 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-08.md)
[2017-03-21 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md)
[2017-03-23 meeting "CLR Behavior for Default Interface Methods"](https://github.com/dotnet/csharplang/blob/master/meetings/2017/CLR-2017-03-23.md)
[2017-04-05 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md)
[2017-04-11 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-11.md)
[2017-04-18 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md)
[2017-04-19 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md)
[2017-05-17 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-17.md)
[2017-05-31 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md)
[2017-06-14 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-06-14.md)
[2018-10-17 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
[2018-11-14 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md)</span></span>
