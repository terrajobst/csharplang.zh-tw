---
ms.openlocfilehash: 6695664c3d5ca73f66e792b7ce2ec9993aceea05
ms.sourcegitcommit: 42ef673ecc883009c865f8384249881a546df216
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2019
ms.locfileid: "79485151"
---
# <a name="lambda-discard-parameters"></a>Lambda 捨棄參數

## <a name="summary"></a>摘要

允許捨棄（`_`）當做 lambda 和匿名方法的參數使用。
例如：
- lambda： `(_, _) => 0`、`(int _, int _) => 0`
- 匿名方法： `delegate(int _, int _) { return 0; }`

## <a name="motivation"></a>動機

未使用的參數不需要命名。 捨棄的目的是明確的，也就是說，它們是未使用/已捨棄。

## <a name="detailed-design"></a>詳細設計

[方法參數](https://github.com/dotnet/csharplang/blob/master/spec/classes.md#method-parameters)在 lambda 或匿名方法的參數清單中，有一個以上名為 `_`的參數，這類參數會捨棄參數。
注意：如果單一參數的名稱為 `_` 則為回溯相容性原因的一般參數。

捨棄參數不會對任何範圍引進任何名稱。
請注意，這表示它們不會隱藏任何 `_` （底線）名稱。

[簡單名稱](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#simple-names)如果 `K` 為零，且*simple_name*出現在*區塊*內，而且如果*區塊*的（或封閉式*區塊*的）本機變數宣告空間（宣告）包含區域變數、[參數（但](basic-concepts.md#declarations)捨棄參數除外）或具有名稱 `I`的常數，則*simple_name*會參考該區域變數、參數或常數，並將其分類為變數或值。

[範圍](https://github.com/dotnet/csharplang/blob/master/spec/basic-concepts.md#scopes)除了捨棄參數之外，在*lambda_expression* （[匿名](expressions.md#anonymous-function-expressions)函式運算式）中宣告的參數範圍是該*lambda_expression*的*anonymous_function_body* ，但捨棄參數除外，在*anonymous_method_expression*中宣告的參數範圍（匿名函式[運算式](expressions.md#anonymous-function-expressions)）是該*anonymous_method_expression*的*區塊*。

## <a name="related-spec-sections"></a>相關的規格區段
- [對應的參數](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#corresponding-parameters)
