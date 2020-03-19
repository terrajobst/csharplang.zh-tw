---
ms.openlocfilehash: 0c8bc2b5072ea7f86189b41a1cdbf2a449661b05
ms.sourcegitcommit: 33a60a1db1d42d21d959acfeb127e647150173aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/18/2020
ms.locfileid: "79509504"
---
# <a name="attributes-on-local-functions"></a><span data-ttu-id="b1e08-101">區域函數上的屬性</span><span class="sxs-lookup"><span data-stu-id="b1e08-101">Attributes on local functions</span></span>

## <a name="attributes"></a><span data-ttu-id="b1e08-102">屬性</span><span class="sxs-lookup"><span data-stu-id="b1e08-102">Attributes</span></span>

<span data-ttu-id="b1e08-103">區域函式宣告現在允許有[屬性](../spec/attributes.md)。</span><span class="sxs-lookup"><span data-stu-id="b1e08-103">Local function declarations are now permitted to have [attributes](../spec/attributes.md).</span></span> <span data-ttu-id="b1e08-104">區域函式上的參數和類型參數也可以具有屬性。</span><span class="sxs-lookup"><span data-stu-id="b1e08-104">Parameters and type parameters on local functions are also allowed to have attributes.</span></span>

<span data-ttu-id="b1e08-105">套用至方法、其參數或其型別參數時，具有指定意義的屬性會分別套用至區域函式、其參數或其型別參數時，具有相同的意義。</span><span class="sxs-lookup"><span data-stu-id="b1e08-105">Attributes with a specified meaning when applied to a method, its parameters, or its type parameters will have the same meaning when applied to a local function, its parameters, or its type parameters, respectively.</span></span>

<span data-ttu-id="b1e08-106">您可以使用 `[ConditionalAttribute]`來裝飾區域函式，使其與[條件式方法](../spec/attributes.md#the-conditional-attribute)具有相同的意義。</span><span class="sxs-lookup"><span data-stu-id="b1e08-106">A local function can be made conditional in the same sense as a [conditional method](../spec/attributes.md#the-conditional-attribute) by decorating it with a `[ConditionalAttribute]`.</span></span> <span data-ttu-id="b1e08-107">條件式區域函數也必須 `static`。</span><span class="sxs-lookup"><span data-stu-id="b1e08-107">A conditional local function must also be `static`.</span></span> <span data-ttu-id="b1e08-108">條件式方法的所有限制也適用于條件式區域函數，包括傳回型別必須 `void`。</span><span class="sxs-lookup"><span data-stu-id="b1e08-108">All restrictions on conditional methods also apply to conditional local functions, including that the return type must be `void`.</span></span>

## <a name="extern"></a><span data-ttu-id="b1e08-109">Extern</span><span class="sxs-lookup"><span data-stu-id="b1e08-109">Extern</span></span>

<span data-ttu-id="b1e08-110">本機函式上現在允許 `extern` 修飾詞。</span><span class="sxs-lookup"><span data-stu-id="b1e08-110">The `extern` modifier is now permitted on local functions.</span></span> <span data-ttu-id="b1e08-111">這使得區域函式在外部與[外部方法](../spec/classes.md#external-methods)相同。</span><span class="sxs-lookup"><span data-stu-id="b1e08-111">This makes the local function external in the same sense as an [external method](../spec/classes.md#external-methods).</span></span>

<span data-ttu-id="b1e08-112">類似于外部方法，外部區域函數的*區域函式主體*必須是分號。</span><span class="sxs-lookup"><span data-stu-id="b1e08-112">Similarly to an external method, the *local-function-body* of an external local function must be a semicolon.</span></span> <span data-ttu-id="b1e08-113">只有外部區域函式允許使用分號*區域函數主體*。</span><span class="sxs-lookup"><span data-stu-id="b1e08-113">A semicolon *local-function-body* is only permitted on an external local function.</span></span> 

<span data-ttu-id="b1e08-114">外部區域函數也必須 `static`。</span><span class="sxs-lookup"><span data-stu-id="b1e08-114">An external local function must also be `static`.</span></span>

## <a name="syntax"></a><span data-ttu-id="b1e08-115">語法</span><span class="sxs-lookup"><span data-stu-id="b1e08-115">Syntax</span></span>

<span data-ttu-id="b1e08-116">[區域函數文法](csharp-7.0/local-functions.md#syntax-grammar)的修改方式如下：</span><span class="sxs-lookup"><span data-stu-id="b1e08-116">The [local functions grammar](csharp-7.0/local-functions.md#syntax-grammar) is modified as follows:</span></span>
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
