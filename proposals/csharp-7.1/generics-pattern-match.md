---
ms.openlocfilehash: 4f66c0f60d05ed6509a1d0843318a71d1b36c351
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484703"
---
# <a name="pattern-matching-with-generics"></a>搭配泛型的模式比對

* [x] 提議
* [] 原型：
* [] 執行：
* [] 規格：

## <a name="summary"></a>摘要
[summary]: #summary

[ C#現有 as 運算子](../../spec/expressions.md#the-as-operator)的規格可讓運算元的類型與指定的類型之間不會有任何轉換（當兩者都是開放式類型時）。 不過，在C# 7 中，`Type identifier` 模式需要在輸入類型與指定類型之間進行轉換。

我們建議您放寬這項功能，並變更 `expression is Type identifier`（除了在C# 7 中允許的情況下允許），也可以在允許 `expression as Type` 時使用。 具體來說，新的案例是運算式的類型或指定的類型是開放式類型的情況。 

## <a name="motivation"></a>動機
[motivation]: #motivation

模式比對應該「明顯」的情況下，目前會無法編譯。 例如，請參閱 https://github.com/dotnet/roslyn/issues/16195。

## <a name="detailed-design"></a>詳細設計
[design]: #detailed-design

我們會變更模式比對規格中的段落（建議的加法以粗體顯示）：

> 左側和指定類型之靜態類型的某些組合會被視為不相容，並導致編譯時期錯誤。 如果存在身分識別轉換、隱含參考轉換、裝箱轉換、明確參考轉換或從 `E` 到 `T`的取消裝箱轉換，**或 `E` 或 `T` 是開啟的類型，** 則靜態類型 `E` 的值會被視為與類型 `T` 的*模式相容*。 如果 `E` 類型的運算式在與其相符的類型模式中的類型模式不相容，就會發生編譯時期錯誤。

## <a name="drawbacks"></a>缺點
[drawbacks]: #drawbacks

無。

## <a name="alternatives"></a>替代方案
[alternatives]: #alternatives

無。

## <a name="unresolved-questions"></a>未解決的問題
[unresolved]: #unresolved-questions

無。

## <a name="design-meetings"></a>設計會議

LDM 認為這個問題，並覺得它是一個 bug 修正層級變更。 我們會將它視為不同的語言功能，因為只要在發行語言後進行變更，就會導致向前不相容。 使用建議的變更時，程式設計人員必須指定語言版本7.1。
