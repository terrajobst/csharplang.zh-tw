---
ms.openlocfilehash: 392d932459ff0a4cb0d6d32c1606f73f9b913c68
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484451"
---
# <a name="covariant-return-types"></a>協變數傳回類型

* [x] 提議
* [] 原型：未啟動
* [] 執行：未啟動
* [] 規格：未啟動

## <a name="summary"></a>摘要
[summary]: #summary

支援_協_變數傳回類型。 具體而言，允許覆寫方法擁有比它所覆寫的方法更多的衍生參考型別。

## <a name="motivation"></a>動機
[motivation]: #motivation

這是程式碼中常見的模式，不同的方法名稱必須產生才能解決語言條件約束，覆寫必須傳回與覆寫方法相同的類型。 如需 Roslyn 程式碼基底的範例，請參閱下面的。

## <a name="detailed-design"></a>詳細設計
[design]: #detailed-design

支援_協_變數傳回類型。 具體而言，允許覆寫方法擁有比它所覆寫的方法更多的衍生參考型別。 這會套用至方法和屬性，並在類別和介面中受到支援。

這在 factory 模式中很有用。 例如，在 Roslyn 程式碼基底中，我們會

``` cs
class Compilation ...
{
    virtual Compilation WithOptions(Options options)...
}
```

``` cs
class CSharpCompilation : Compilation
{
    override CSharpCompilation WithOptions(Options options)...
}
```

這是為了讓編譯器將覆寫方法發出為「新的」虛擬方法，以隱藏基類方法，以及使用衍生類別方法的呼叫來實作為基底類別方法的_橋接器方法_。

## <a name="drawbacks"></a>缺點
[drawbacks]: #drawbacks

- [] 每一種語言變更都必須支付自己的費用。
- [] 我們應確保效能合理，即使在深層繼承階層架構中也一樣
- [] 我們應該確保轉譯策略的成品不會影響語言的語義，即使從舊的編譯器取用新的 IL 也一樣。

## <a name="alternatives"></a>替代方案
[alternatives]: #alternatives

我們可以稍微放寬語言規則，讓來源中的

```csharp
abstract class Cloneable
{
    public abstract Cloneable Clone();
}

class Digit : Cloneable
{
    public override Cloneable Clone()
    {
        return this.Clone();
    }

    public new Digit Clone() // Error: 'Digit' already defines a member called 'Clone' with the same parameter types
    {
        return this;
    }
}
```

## <a name="unresolved-questions"></a>未解決的問題
[unresolved]: #unresolved-questions

- [] 如何使用此功能編譯的 Api 適用于舊版的語言？

## <a name="design-meetings"></a>設計會議

還沒有。 <https://github.com/dotnet/roslyn/issues/357>上有一些討論。