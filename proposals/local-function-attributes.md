---
ms.openlocfilehash: 0c8bc2b5072ea7f86189b41a1cdbf2a449661b05
ms.sourcegitcommit: 33a60a1db1d42d21d959acfeb127e647150173aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/18/2020
ms.locfileid: "79509504"
---
# <a name="attributes-on-local-functions"></a>區域函數上的屬性

## <a name="attributes"></a>屬性

區域函式宣告現在允許有[屬性](../spec/attributes.md)。 區域函式上的參數和類型參數也可以具有屬性。

套用至方法、其參數或其型別參數時，具有指定意義的屬性會分別套用至區域函式、其參數或其型別參數時，具有相同的意義。

您可以使用 `[ConditionalAttribute]`來裝飾區域函式，使其與[條件式方法](../spec/attributes.md#the-conditional-attribute)具有相同的意義。 條件式區域函數也必須 `static`。 條件式方法的所有限制也適用于條件式區域函數，包括傳回型別必須 `void`。

## <a name="extern"></a>Extern

本機函式上現在允許 `extern` 修飾詞。 這使得區域函式在外部與[外部方法](../spec/classes.md#external-methods)相同。

類似于外部方法，外部區域函數的*區域函式主體*必須是分號。 只有外部區域函式允許使用分號*區域函數主體*。 

外部區域函數也必須 `static`。

## <a name="syntax"></a>語法

[區域函數文法](csharp-7.0/local-functions.md#syntax-grammar)的修改方式如下：
```
local-function-header
    : attributes? local-function-modifiers? return-type identifier type-parameter-list?
        ( formal-parameter-list? ) type-parameter-constraints-clauses
    ;

local-function-modifiers
    : (async | unsafe | static | extern)*
    ;

local-function-body
    : block
    | arrow-expression-body
    | ';'
    ;
```
