---
ms.openlocfilehash: a78567594d39fc4e204e12c4f2f247b8d6995c38
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484920"
---
# <a name="expression-variables-in-initializers"></a>初始設定式中的運算式變數

## <a name="summary"></a>摘要
[summary]: #summary

我們擴充7中C#引進的功能，以允許運算式包含欄位初始化運算式中的運算式變數（out 變數宣告和宣告模式）、屬性初始化運算式、ctor 初始化運算式和查詢子句。

## <a name="motivation"></a>動機
[motivation]: #motivation

因為缺乏時間，所以這會在C#語言中完成幾個粗略的邊緣。

## <a name="detailed-design"></a>詳細設計
[design]: #detailed-design

我們會移除限制，以避免在 ctor 初始化運算式中宣告運算式變數（out 變數宣告和宣告模式）。 這類宣告的變數會在整個建構函式主體的範圍內。

我們會移除限制，防止在欄位或屬性初始化運算式中宣告運算式變數（out 變數宣告和宣告模式）。 這類宣告的變數會在整個初始化運算式的範圍中。

我們會移除限制，以防止在轉譯成 lambda 主體的查詢運算式子句中宣告運算式變數（out 變數宣告和宣告模式）。 這類宣告的變數會在查詢子句的整個運算式範圍內。

## <a name="drawbacks"></a>缺點
[drawbacks]: #drawbacks

無。

## <a name="alternatives"></a>替代方案
[alternatives]: #alternatives

這些內容中所宣告之運算式變數的適當範圍並不明顯，且值得進一步的 LDM 討論。

## <a name="unresolved-questions"></a>未解決的問題
[unresolved]: #unresolved-questions

- [] 這些變數的適當範圍為何？

## <a name="design-meetings"></a>設計會議

無。
