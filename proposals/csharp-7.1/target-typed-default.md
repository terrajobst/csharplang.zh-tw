---
ms.openlocfilehash: 1852356b830e29c3537a4de559fef32fd2c2f8b6
ms.sourcegitcommit: f7952cdddf85316a4beec493a0ecc14fcb3241c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/30/2019
ms.locfileid: "79484983"
---
# <a name="target-typed-default-literal"></a>目標型別 "default" 常值

* [x] 提議
* [x] 原型
* [x] 執行
* [] 規格

## <a name="summary"></a>摘要
[summary]: #summary

目標具類型的 `default` 功能是 `default(T)` 運算子的較短形式變體，可省略類型。 它的類型是由目標型別推斷而來。 除此之外，它的行為就像 `default(T)`。

## <a name="motivation"></a>動機
[motivation]: #motivation

主要動機是避免輸入多餘的資訊。

例如，叫用 `void Method(ImmutableArray<SomeType> array)`時，*預設常值*允許 `M(default)` 取代 `M(default(ImmutableArray<SomeType>))`。

這適用于許多案例，例如：

- 宣告區域變數（`ImmutableArray<SomeType> x = default;`）
- 三元運算（`var x = flag ? default : ImmutableArray<SomeType>.Empty;`）
- 在方法和 lambda 中傳回（`return default;`）
- 宣告選擇性參數的預設值（`void Method(ImmutableArray<SomeType> arrayOpt = default)`）
- 在陣列建立運算式中包含預設值（`var x = new[] { default, ImmutableArray.Create(y) };`）


## <a name="detailed-design"></a>詳細設計
[design]: #detailed-design

引進了新的運算式，也就是預設的常*值*。 具有此分類的運算式可以透過*預設的常值轉換*，隱含地轉換成任何類型。 

*預設常值*的類型推斷運作方式與*null*常值相同，不同之處在于允許任何類型（而不只是參考類型）。

這項轉換會產生推斷類型的預設值。

*預設*常值可能會有常數值，視推斷的類型而定。 `const int x = default;` 是合法的，但 `const int? y = default;` 不是。

只要另一個運算元具有類型，*預設常值*可以是等號比較運算子的運算元。 因此 `default == x` 和 `x == default` 都是有效的運算式，但 `default == default` 是不合法的。

## <a name="drawbacks"></a>缺點
[drawbacks]: #drawbacks

次要缺點是，*預設常值*可以用來取代大部分內容中的*null*常值。 有兩個例外狀況 `throw null;` 和 `null == null`，這是允許用於*null*常值，但不是*預設常值*的。

## <a name="alternatives"></a>替代方案
[alternatives]: #alternatives

有幾個需要考慮的替代方案：

- 現狀：此功能不會在其本身的優勢下證明，而開發人員會繼續使用具有明確類型的預設運算子。
- 擴充 null 常值：這是使用 `Nothing`的 VB 方法。 我們可能會允許 `int x = null;`。

## <a name="unresolved-questions"></a>未解決的問題
[unresolved]: #unresolved-questions

- [x] 應該*預設*為使用 as 或*as* *運算子的運算元*？ 答：不允許 `default is T`、允許 `x is default`、允許 `default as RefType` （具有 always-null 警告）

## <a name="design-meetings"></a>設計會議

- [LDM 3/7/2017](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-07.md)
- [LDM 3/28/2017](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-28.md)
- [LDM 5/31/2017](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md#default-in-operators)
