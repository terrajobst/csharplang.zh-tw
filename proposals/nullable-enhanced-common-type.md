---
ms.openlocfilehash: 032cb8590a0b6e83f8ab6201e10720f1b254c605
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484528"
---
# <a name="nullable-enhanced-common-type"></a>可為 null 增強的一般類型

* [x] 提議
* [] 原型：無
* [] 執行：無
* [] 規格：請參閱下文

## <a name="summary"></a>摘要
[summary]: #summary

在某些情況下，目前的一般類型演算法結果會以直覺方式呈現，並導致程式設計人員加入類似于程式碼的重複轉換。 透過這項變更，運算式（例如 `condition ? 1 : null`）會產生 `int?`類型的值，而運算式（例如 `condition ? x : 1.0`，其中 `x` 的類型 `int?` 會產生 `double?`類型的值。

## <a name="motivation"></a>動機
[motivation]: #motivation

這是常見的程式設計人員所造成的原因，例如不必要的未定案程式碼。

## <a name="detailed-design"></a>詳細設計
[design]: #detailed-design

我們會修改用來[尋找一組運算式之最佳一般類型](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions)的規格，以影響下列情況：

- 如果一個運算式是不可為 null 的實值型別 `T`，另一個則是 null 常值，則結果會是 `T?`類型。
- 如果一個運算式是可為 null 的實值型別 `T?` 而且另一個是 `U`的實值型別，而且有從 `T` 到 `U`的隱含轉換，則結果會是 `U?`型別。

這應該會影響語言的下列各方面：

- [三元運算式](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#conditional-operator)
- 隱含類型[陣列建立運算式](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#array-creation-expressions)
- 推斷型別推斷的[lambda 傳回型](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#inferred-return-type)別
- 涉及泛型的案例，例如叫用 `M<T>(T a, T b)` 做為 `M(1, null)`。

更精確的說，我們會變更規格的下列區段（以粗體插入、刪除刪除）：

> #### <a name="output-type-inferences"></a>輸出類型推斷
> 
> *輸出類型推斷*是*從*運算式 `E`*到*類型 `T` 以下列方式進行：
> 
> *  如果 `E` 是具有推斷傳回類型的匿名函式 `U`[（推斷](expressions.md#inferred-return-type)的傳回型別），而 `T` 是具有傳回型別 `Tb`的委派型別或運算式樹狀結構型別，則會*從*`U`*到*`Tb`進行*較低*的系結推斷（[較低](expressions.md#lower-bound-inferences)系結）。
> *  否則，如果 `E` 是方法群組，而 `T` 是具有參數類型 `T1...Tk` 和傳回類型 `Tb`的委派類型或運算式樹狀結構類型，而且 `E` 具有類型 `T1...Tk` 的多載解析會產生傳回類型 `U`的單一方法，則會*從*`U`*到*`Tb`進行*下限推斷*。
> *  \* * 否則，如果 `E` 是可為 null 的實數值型別 `U?`的運算式，則會從 `U`*到*`T` 進行*下限推斷*，並將*null*系結加入 `T`*中*。 **
> *  否則，如果 `E` 是具有類型 `U`的運算式，則會*從*`U`*到*`T`進行*較低*系結的推斷。
> *  **否則，如果 `E` 是具有值 `null`的常數運算式，則會將*null*系結新增至 `T`** 
> *  否則，就不會進行推斷。

> #### <a name="fixing"></a>修正
> 
> 未*固定的類型變數*`Xi` 具有一組界限，如下*所示：*
> 
> *  *候選類型*的集合 `Uj` 一開始就是 `Xi`界限集合中的所有類型集合。
> *  接著，我們會檢查每個 `Xi` 的系結：對於 `Xi` 所有與 `U` 不完全相同的 `U` 之每個完全系結的 `Uj`，會從候選集合中移除。 對於 `Xi` 的每個下限 `U`，會從候選集合中移除*不*是從 `U` 隱含轉換的所有類型 `Uj`。 針對 `Xi` 的每個上限 `U`，*不*會從候選集合中移除隱含轉換成 `U` 的所有類型 `Uj`。
> *  如果剩餘的候選類型 `Uj` 有一個唯一的類型 `V`，其中隱含轉換至所有其他候選類型，則~~`Xi` 會固定到 `V`。~~
>     -  **如果 `V` 是實值型別，且 `Xi`有*null*系結，則 `Xi` 會固定為 `V?`**
>     -  **否則 `Xi` 會固定為 `V`**
> *  否則，型別推斷會失敗。

## <a name="drawbacks"></a>缺點
[drawbacks]: #drawbacks

此提案可能會有一些不相容的問題。

## <a name="alternatives"></a>替代方案
[alternatives]: #alternatives

無。

## <a name="unresolved-questions"></a>未解決的問題
[unresolved]: #unresolved-questions

- [] 此提案引進的不相容性嚴重性為何（如果有的話），以及如何仲裁？

## <a name="design-meetings"></a>設計會議

無。
