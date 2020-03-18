---
ms.openlocfilehash: d0bb80305ccc755a555cf47a1d010fc4cb9a7bcd
ms.sourcegitcommit: 5688b13e66dd77b224a1223338de9e3b1f66d7f0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/13/2020
ms.locfileid: "79485515"
---
# <a name="readonly-instance-members"></a><span data-ttu-id="0e89b-101">Readonly 實例成員</span><span class="sxs-lookup"><span data-stu-id="0e89b-101">Readonly Instance Members</span></span>

<span data-ttu-id="0e89b-102">探討問題： <https://github.com/dotnet/csharplang/issues/1710></span><span class="sxs-lookup"><span data-stu-id="0e89b-102">Championed Issue: <https://github.com/dotnet/csharplang/issues/1710></span></span>

## <a name="summary"></a><span data-ttu-id="0e89b-103">摘要</span><span class="sxs-lookup"><span data-stu-id="0e89b-103">Summary</span></span>
[summary]: #summary

<span data-ttu-id="0e89b-104">提供方法來指定結構上的個別實例成員不會修改狀態，方法與 `readonly struct` 不會指定實例成員修改狀態相同。</span><span class="sxs-lookup"><span data-stu-id="0e89b-104">Provide a way to specify individual instance members on a struct do not modify state, in the same way that `readonly struct` specifies no instance members modify state.</span></span>

<span data-ttu-id="0e89b-105">值得注意的是，`readonly instance member`！ = `pure instance member`。</span><span class="sxs-lookup"><span data-stu-id="0e89b-105">It is worth noting that `readonly instance member` != `pure instance member`.</span></span> <span data-ttu-id="0e89b-106">`pure` 實例成員保證不會修改任何狀態。</span><span class="sxs-lookup"><span data-stu-id="0e89b-106">A `pure` instance member guarantees no state will be modified.</span></span> <span data-ttu-id="0e89b-107">`readonly` 實例成員只保證實例狀態不會被修改。</span><span class="sxs-lookup"><span data-stu-id="0e89b-107">A `readonly` instance member only guarantees that instance state will not be modified.</span></span>

<span data-ttu-id="0e89b-108">`readonly struct` 上的所有實例成員都可以隱含地 `readonly instance members`。</span><span class="sxs-lookup"><span data-stu-id="0e89b-108">All instance members on a `readonly struct` could be considered implicitly `readonly instance members`.</span></span> <span data-ttu-id="0e89b-109">在非 readonly 結構上宣告的明確 `readonly instance members` 會以相同的方式運作。</span><span class="sxs-lookup"><span data-stu-id="0e89b-109">Explicit `readonly instance members` declared on non-readonly structs would behave in the same manner.</span></span> <span data-ttu-id="0e89b-110">例如，如果您呼叫實例成員（在目前的實例上或在實例的欄位上），則它們仍然會建立隱藏的複本，其本身不是唯讀的。</span><span class="sxs-lookup"><span data-stu-id="0e89b-110">For example, they would still create hidden copies if you called an instance member (on the current instance or on a field of the instance) which was itself not-readonly.</span></span>

## <a name="motivation"></a><span data-ttu-id="0e89b-111">動機</span><span class="sxs-lookup"><span data-stu-id="0e89b-111">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="0e89b-112">現在，使用者可以建立 `readonly struct` 類型，讓編譯器強制所有欄位都是唯讀的（而不是由任何實例成員修改狀態的擴充功能）。</span><span class="sxs-lookup"><span data-stu-id="0e89b-112">Today, users have the ability to create `readonly struct` types which the compiler enforces that all fields are readonly (and by extension, that no instance members modify the state).</span></span> <span data-ttu-id="0e89b-113">不過，在某些情況下，您會有一個公開可存取欄位或混合變更和非變動成員的現有 API。</span><span class="sxs-lookup"><span data-stu-id="0e89b-113">However, there are some scenarios where you have an existing API that exposes accessible fields or that has a mix of mutating and non-mutating members.</span></span> <span data-ttu-id="0e89b-114">在這些情況下，您無法將類型標記為 `readonly` （這會是重大變更）。</span><span class="sxs-lookup"><span data-stu-id="0e89b-114">Under these circumstances, you cannot mark the type as `readonly` (it would be a breaking change).</span></span>

<span data-ttu-id="0e89b-115">這通常不會有太大的影響，但 `in` 參數的情況除外。</span><span class="sxs-lookup"><span data-stu-id="0e89b-115">This normally doesn't have much impact, except in the case of `in` parameters.</span></span> <span data-ttu-id="0e89b-116">使用非唯讀結構的 `in` 參數時，編譯器會為每個實例成員調用建立參數的複本，因為它無法保證調用不會修改內部狀態。</span><span class="sxs-lookup"><span data-stu-id="0e89b-116">With `in` parameters for non-readonly structs, the compiler will make a copy of the parameter for each instance member invocation, since it cannot guarantee that the invocation does not modify internal state.</span></span> <span data-ttu-id="0e89b-117">這可能會導致許多複本，而且整體效能也會比您直接以傳值方式傳遞結構來得差。</span><span class="sxs-lookup"><span data-stu-id="0e89b-117">This can lead to a multitude of copies and worse overall performance than if you had just passed the struct directly by value.</span></span> <span data-ttu-id="0e89b-118">如需範例，請參閱[sharplab](https://sharplab.io/#v2:CYLg1APgAgDABFAjAbgLACgNQMxwM4AuATgK4DGBcAagKYUD2RATBgN4ZycK4BmANvQCGlAB5p0XbnH5DKAT3GSOXHNIHC4AGRoA7AOYEAFgGUAjiUFEawZZ3YTJXPTQK3H9x54QB2OAAoROAAqOBEASjgwNy8YvzlguDkwxS8AXzd09EysXCgmOABhOA8VXnVKAFk/AEsdajoCRnyAN0E+EhoIks8oX1b2mgA6bX0jMwsrYEi4fo7h3QMTc0trFM5M1KA==)上的此程式碼</span><span class="sxs-lookup"><span data-stu-id="0e89b-118">For an example, see this code on [sharplab](https://sharplab.io/#v2:CYLg1APgAgDABFAjAbgLACgNQMxwM4AuATgK4DGBcAagKYUD2RATBgN4ZycK4BmANvQCGlAB5p0XbnH5DKAT3GSOXHNIHC4AGRoA7AOYEAFgGUAjiUFEawZZ3YTJXPTQK3H9x54QB2OAAoROAAqOBEASjgwNy8YvzlguDkwxS8AXzd09EysXCgmOABhOA8VXnVKAFk/AEsdajoCRnyAN0E+EhoIks8oX1b2mgA6bX0jMwsrYEi4fo7h3QMTc0trFM5M1KA==)</span></span>

<span data-ttu-id="0e89b-119">可能發生隱藏複本的一些其他案例包括 `static readonly fields` 和 `literals`。</span><span class="sxs-lookup"><span data-stu-id="0e89b-119">Some other scenarios where hidden copies can occur include `static readonly fields` and `literals`.</span></span> <span data-ttu-id="0e89b-120">如果未來受到支援，`blittable constants` 最後會在相同的船上;也就是說，如果結構未標記為 `readonly`，所有目前都一定要有完整複本（在實例成員調用上）。</span><span class="sxs-lookup"><span data-stu-id="0e89b-120">If they are supported in the future, `blittable constants` would end up in the same boat; that is they all currently necessitate a full copy (on instance member invocation) if the struct is not marked `readonly`.</span></span>

## <a name="design"></a><span data-ttu-id="0e89b-121">設計</span><span class="sxs-lookup"><span data-stu-id="0e89b-121">Design</span></span>
[design]: #design

<span data-ttu-id="0e89b-122">允許使用者指定實例成員為、本身 `readonly`，而且不會修改實例的狀態（當然，編譯器會完成所有適當的驗證）。</span><span class="sxs-lookup"><span data-stu-id="0e89b-122">Allow a user to specify that an instance member is, itself, `readonly` and does not modify the state of the instance (with all the appropriate verification done by the compiler, of course).</span></span> <span data-ttu-id="0e89b-123">例如：</span><span class="sxs-lookup"><span data-stu-id="0e89b-123">For example:</span></span>

```csharp
public struct Vector2
{
    public float x;
    public float y;

    public readonly float GetLengthReadonly()
    {
        return MathF.Sqrt(LengthSquared);
    }

    public float GetLength()
    {
        return MathF.Sqrt(LengthSquared);
    }

    public readonly float GetLengthIllegal()
    {
        var tmp = MathF.Sqrt(LengthSquared);

        x = tmp;    // Compiler error, cannot write x
        y = tmp;    // Compiler error, cannot write y

        return tmp;
    }

    public float LengthSquared
    {
        readonly get
        {
            return (x * x) +
                   (y * y);
        }
    }
}

public static class MyClass
{
    public static float ExistingBehavior(in Vector2 vector)
    {
        // This code causes a hidden copy, the compiler effectively emits:
        //    var tmpVector = vector;
        //    return tmpVector.GetLength();
        //
        // This is done because the compiler doesn't know that `GetLength()`
        // won't mutate `vector`.

        return vector.GetLength();
    }

    public static float ReadonlyBehavior(in Vector2 vector)
    {
        // This code is emitted exactly as listed. There are no hidden
        // copies as the `readonly` modifier indicates that the method
        // won't mutate `vector`.

        return vector.GetLengthReadonly();
    }
}
```

<span data-ttu-id="0e89b-124">Readonly 可以套用至屬性存取子，以指出 `this` 不會在存取子中進行變化。</span><span class="sxs-lookup"><span data-stu-id="0e89b-124">Readonly can be applied to property accessors to indicate that `this` will not be mutated in the accessor.</span></span> <span data-ttu-id="0e89b-125">下列範例具有 readonly setter，因為這些存取子會修改成員欄位的狀態，但不會修改該成員欄位的值。</span><span class="sxs-lookup"><span data-stu-id="0e89b-125">The following examples have readonly setters because those accessors modify the state of member field, but do not modify the value of that member field.</span></span>

```csharp
public int Prop1
{
    readonly get
    {
        return this._store["Prop1"];
    }
    readonly set
    {
        this._store["Prop1"] = value;
    }
}
```

<span data-ttu-id="0e89b-126">當 `readonly` 套用至屬性語法時，表示會 `readonly`所有存取子。</span><span class="sxs-lookup"><span data-stu-id="0e89b-126">When `readonly` is applied to the property syntax, it means that all accessors are `readonly`.</span></span>

```csharp
public readonly int Prop2
{
    get
    {
        return this._store["Prop2"];
    }
    set
    {
        this._store["Prop2"] = value;
    }
}
```

<span data-ttu-id="0e89b-127">Readonly 只能套用至不會改變包含類型的存取子。</span><span class="sxs-lookup"><span data-stu-id="0e89b-127">Readonly can only be applied to accessors which do not mutate the containing type.</span></span>

```csharp
public int Prop3
{
    readonly get
    {
        return this._prop3;
    }
    set
    {
        this._prop3 = value;
    }
}
```

<span data-ttu-id="0e89b-128">Readonly 可以套用至某些自動執行的屬性，但不會有有意義的效果。</span><span class="sxs-lookup"><span data-stu-id="0e89b-128">Readonly can be applied to some auto-implemented properties, but it won't have a meaningful effect.</span></span> <span data-ttu-id="0e89b-129">無論 `readonly` 關鍵字是否存在，編譯器都會將所有自動執行的 getter 視為 readonly。</span><span class="sxs-lookup"><span data-stu-id="0e89b-129">The compiler will treat all auto-implemented getters as readonly whether or not the `readonly` keyword is present.</span></span>

```csharp
// Allowed
public readonly int Prop4 { get; }
public int Prop5 { readonly get; }
public int Prop6 { readonly get; set; }

// Not allowed
public readonly int Prop7 { get; set; }
public int Prop8 { get; readonly set; }
```

<span data-ttu-id="0e89b-130">Readonly 可以套用至手動執行的事件，但不能套用至類似欄位的事件。</span><span class="sxs-lookup"><span data-stu-id="0e89b-130">Readonly can be applied to manually-implemented events, but not field-like events.</span></span> <span data-ttu-id="0e89b-131">Readonly 無法套用到個別事件存取子（新增/移除）。</span><span class="sxs-lookup"><span data-stu-id="0e89b-131">Readonly cannot be applied to individual event accessors (add/remove).</span></span>

```csharp
// Allowed
public readonly event Action<EventArgs> Event1
{
    add { }
    remove { }
}

// Not allowed
public readonly event Action<EventArgs> Event2;
public event Action<EventArgs> Event3
{
    readonly add { }
    readonly remove { }
}
public static readonly event Event4
{
    add { }
    remove { }
}
```

<span data-ttu-id="0e89b-132">一些其他語法範例：</span><span class="sxs-lookup"><span data-stu-id="0e89b-132">Some other syntax examples:</span></span>

* <span data-ttu-id="0e89b-133">Expression 主體成員： `public readonly float ExpressionBodiedMember => (x * x) + (y * y);`</span><span class="sxs-lookup"><span data-stu-id="0e89b-133">Expression bodied members: `public readonly float ExpressionBodiedMember => (x * x) + (y * y);`</span></span>
* <span data-ttu-id="0e89b-134">泛型條件約束： `public static readonly void GenericMethod<T>(T value) where T : struct { }`</span><span class="sxs-lookup"><span data-stu-id="0e89b-134">Generic constraints: `public static readonly void GenericMethod<T>(T value) where T : struct { }`</span></span>

<span data-ttu-id="0e89b-135">編譯器會如往常般發出實例成員，並且會另外發出編譯器可辨識的屬性，表示實例成員不會修改狀態。</span><span class="sxs-lookup"><span data-stu-id="0e89b-135">The compiler would emit the instance member, as usual, and would additionally emit a compiler recognized attribute indicating that the instance member does not modify state.</span></span> <span data-ttu-id="0e89b-136">這可有效地使隱藏的 `this` 參數變成 `in T`，而不是 `ref T`。</span><span class="sxs-lookup"><span data-stu-id="0e89b-136">This effectively causes the hidden `this` parameter to become `in T` instead of `ref T`.</span></span>

<span data-ttu-id="0e89b-137">這可讓使用者安全地呼叫所謂的實例方法，而不需要編譯器進行複製。</span><span class="sxs-lookup"><span data-stu-id="0e89b-137">This would allow the user to safely call said instance method without the compiler needing to make a copy.</span></span>

<span data-ttu-id="0e89b-138">其限制包括：</span><span class="sxs-lookup"><span data-stu-id="0e89b-138">The restrictions would include:</span></span>

* <span data-ttu-id="0e89b-139">`readonly` 修飾詞不能套用至靜態方法、函數或析構函式。</span><span class="sxs-lookup"><span data-stu-id="0e89b-139">The `readonly` modifier cannot be applied to static methods, constructors or destructors.</span></span>
* <span data-ttu-id="0e89b-140">`readonly` 修飾詞不能套用至委派。</span><span class="sxs-lookup"><span data-stu-id="0e89b-140">The `readonly` modifier cannot be applied to delegates.</span></span>
* <span data-ttu-id="0e89b-141">`readonly` 修飾詞不能套用至類別或介面的成員。</span><span class="sxs-lookup"><span data-stu-id="0e89b-141">The `readonly` modifier cannot be applied to members of class or interface.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="0e89b-142">缺點</span><span class="sxs-lookup"><span data-stu-id="0e89b-142">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="0e89b-143">現在與 `readonly struct` 方法相同的缺點。</span><span class="sxs-lookup"><span data-stu-id="0e89b-143">Same drawbacks as exist with `readonly struct` methods today.</span></span> <span data-ttu-id="0e89b-144">某些程式碼可能仍會造成隱藏的複本。</span><span class="sxs-lookup"><span data-stu-id="0e89b-144">Certain code may still cause hidden copies.</span></span>

## <a name="notes"></a><span data-ttu-id="0e89b-145">注意</span><span class="sxs-lookup"><span data-stu-id="0e89b-145">Notes</span></span>
[notes]: #notes

<span data-ttu-id="0e89b-146">可能也可以使用屬性或另一個關鍵字。</span><span class="sxs-lookup"><span data-stu-id="0e89b-146">Using an attribute or another keyword may also be possible.</span></span>

<span data-ttu-id="0e89b-147">這項提案與（但也是的子集） `functional purity` 和/或 `constant expressions`有些相關，這兩者都有一些現有的提案。</span><span class="sxs-lookup"><span data-stu-id="0e89b-147">This proposal is somewhat related to (but is more a subset of) `functional purity` and/or `constant expressions`, both of which have had some existing proposals.</span></span>
