---
ms.openlocfilehash: d0bb80305ccc755a555cf47a1d010fc4cb9a7bcd
ms.sourcegitcommit: 5688b13e66dd77b224a1223338de9e3b1f66d7f0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/13/2020
ms.locfileid: "79485515"
---
# <a name="readonly-instance-members"></a>Readonly 實例成員

探討問題： <https://github.com/dotnet/csharplang/issues/1710>

## <a name="summary"></a>摘要
[summary]: #summary

提供方法來指定結構上的個別實例成員不會修改狀態，方法與 `readonly struct` 不會指定實例成員修改狀態相同。

值得注意的是，`readonly instance member`！ = `pure instance member`。 `pure` 實例成員保證不會修改任何狀態。 `readonly` 實例成員只保證實例狀態不會被修改。

`readonly struct` 上的所有實例成員都可以隱含地 `readonly instance members`。 在非 readonly 結構上宣告的明確 `readonly instance members` 會以相同的方式運作。 例如，如果您呼叫實例成員（在目前的實例上或在實例的欄位上），則它們仍然會建立隱藏的複本，其本身不是唯讀的。

## <a name="motivation"></a>動機
[motivation]: #motivation

現在，使用者可以建立 `readonly struct` 類型，讓編譯器強制所有欄位都是唯讀的（而不是由任何實例成員修改狀態的擴充功能）。 不過，在某些情況下，您會有一個公開可存取欄位或混合變更和非變動成員的現有 API。 在這些情況下，您無法將類型標記為 `readonly` （這會是重大變更）。

這通常不會有太大的影響，但 `in` 參數的情況除外。 使用非唯讀結構的 `in` 參數時，編譯器會為每個實例成員調用建立參數的複本，因為它無法保證調用不會修改內部狀態。 這可能會導致許多複本，而且整體效能也會比您直接以傳值方式傳遞結構來得差。 如需範例，請參閱[sharplab](https://sharplab.io/#v2:CYLg1APgAgDABFAjAbgLACgNQMxwM4AuATgK4DGBcAagKYUD2RATBgN4ZycK4BmANvQCGlAB5p0XbnH5DKAT3GSOXHNIHC4AGRoA7AOYEAFgGUAjiUFEawZZ3YTJXPTQK3H9x54QB2OAAoROAAqOBEASjgwNy8YvzlguDkwxS8AXzd09EysXCgmOABhOA8VXnVKAFk/AEsdajoCRnyAN0E+EhoIks8oX1b2mgA6bX0jMwsrYEi4fo7h3QMTc0trFM5M1KA==)上的此程式碼

可能發生隱藏複本的一些其他案例包括 `static readonly fields` 和 `literals`。 如果未來受到支援，`blittable constants` 最後會在相同的船上;也就是說，如果結構未標記為 `readonly`，所有目前都一定要有完整複本（在實例成員調用上）。

## <a name="design"></a>設計
[design]: #design

允許使用者指定實例成員為、本身 `readonly`，而且不會修改實例的狀態（當然，編譯器會完成所有適當的驗證）。 例如：

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

Readonly 可以套用至屬性存取子，以指出 `this` 不會在存取子中進行變化。 下列範例具有 readonly setter，因為這些存取子會修改成員欄位的狀態，但不會修改該成員欄位的值。

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

當 `readonly` 套用至屬性語法時，表示會 `readonly`所有存取子。

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

Readonly 只能套用至不會改變包含類型的存取子。

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

Readonly 可以套用至某些自動執行的屬性，但不會有有意義的效果。 無論 `readonly` 關鍵字是否存在，編譯器都會將所有自動執行的 getter 視為 readonly。

```csharp
// Allowed
public readonly int Prop4 { get; }
public int Prop5 { readonly get; }
public int Prop6 { readonly get; set; }

// Not allowed
public readonly int Prop7 { get; set; }
public int Prop8 { get; readonly set; }
```

Readonly 可以套用至手動執行的事件，但不能套用至類似欄位的事件。 Readonly 無法套用到個別事件存取子（新增/移除）。

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

一些其他語法範例：

* Expression 主體成員： `public readonly float ExpressionBodiedMember => (x * x) + (y * y);`
* 泛型條件約束： `public static readonly void GenericMethod<T>(T value) where T : struct { }`

編譯器會如往常般發出實例成員，並且會另外發出編譯器可辨識的屬性，表示實例成員不會修改狀態。 這可有效地使隱藏的 `this` 參數變成 `in T`，而不是 `ref T`。

這可讓使用者安全地呼叫所謂的實例方法，而不需要編譯器進行複製。

其限制包括：

* `readonly` 修飾詞不能套用至靜態方法、函數或析構函式。
* `readonly` 修飾詞不能套用至委派。
* `readonly` 修飾詞不能套用至類別或介面的成員。

## <a name="drawbacks"></a>缺點
[drawbacks]: #drawbacks

現在與 `readonly struct` 方法相同的缺點。 某些程式碼可能仍會造成隱藏的複本。

## <a name="notes"></a>注意
[notes]: #notes

可能也可以使用屬性或另一個關鍵字。

這項提案與（但也是的子集） `functional purity` 和/或 `constant expressions`有些相關，這兩者都有一些現有的提案。
