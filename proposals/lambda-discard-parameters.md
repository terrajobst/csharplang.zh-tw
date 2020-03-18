---
ms.openlocfilehash: 6695664c3d5ca73f66e792b7ce2ec9993aceea05
ms.sourcegitcommit: 42ef673ecc883009c865f8384249881a546df216
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2019
ms.locfileid: "79485151"
---
# <a name="lambda-discard-parameters"></a><span data-ttu-id="a50ba-101">Lambda 捨棄參數</span><span class="sxs-lookup"><span data-stu-id="a50ba-101">Lambda discard parameters</span></span>

## <a name="summary"></a><span data-ttu-id="a50ba-102">摘要</span><span class="sxs-lookup"><span data-stu-id="a50ba-102">Summary</span></span>

<span data-ttu-id="a50ba-103">允許捨棄（`_`）當做 lambda 和匿名方法的參數使用。</span><span class="sxs-lookup"><span data-stu-id="a50ba-103">Allow discards (`_`) to be used as parameters of lambdas and anonymous methods.</span></span>
<span data-ttu-id="a50ba-104">例如：</span><span class="sxs-lookup"><span data-stu-id="a50ba-104">For example:</span></span>
- <span data-ttu-id="a50ba-105">lambda： `(_, _) => 0`、`(int _, int _) => 0`</span><span class="sxs-lookup"><span data-stu-id="a50ba-105">lambdas: `(_, _) => 0`, `(int _, int _) => 0`</span></span>
- <span data-ttu-id="a50ba-106">匿名方法： `delegate(int _, int _) { return 0; }`</span><span class="sxs-lookup"><span data-stu-id="a50ba-106">anonymous methods: `delegate(int _, int _) { return 0; }`</span></span>

## <a name="motivation"></a><span data-ttu-id="a50ba-107">動機</span><span class="sxs-lookup"><span data-stu-id="a50ba-107">Motivation</span></span>

<span data-ttu-id="a50ba-108">未使用的參數不需要命名。</span><span class="sxs-lookup"><span data-stu-id="a50ba-108">Unused parameters do not need to be named.</span></span> <span data-ttu-id="a50ba-109">捨棄的目的是明確的，也就是說，它們是未使用/已捨棄。</span><span class="sxs-lookup"><span data-stu-id="a50ba-109">The intent of discards is clear, i.e. they are unused/discarded.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="a50ba-110">詳細設計</span><span class="sxs-lookup"><span data-stu-id="a50ba-110">Detailed design</span></span>

<span data-ttu-id="a50ba-111">[方法參數](https://github.com/dotnet/csharplang/blob/master/spec/classes.md#method-parameters)在 lambda 或匿名方法的參數清單中，有一個以上名為 `_`的參數，這類參數會捨棄參數。</span><span class="sxs-lookup"><span data-stu-id="a50ba-111">[Method parameters](https://github.com/dotnet/csharplang/blob/master/spec/classes.md#method-parameters) In the parameter list of a lambda or anonymous method with more than one parameter named `_`, such parameters are discard parameters.</span></span>
<span data-ttu-id="a50ba-112">注意：如果單一參數的名稱為 `_` 則為回溯相容性原因的一般參數。</span><span class="sxs-lookup"><span data-stu-id="a50ba-112">Note: if a single parameter is named `_` then it is a regular parameter for backwards compatibility reasons.</span></span>

<span data-ttu-id="a50ba-113">捨棄參數不會對任何範圍引進任何名稱。</span><span class="sxs-lookup"><span data-stu-id="a50ba-113">Discard parameters do not introduce any names to any scopes.</span></span>
<span data-ttu-id="a50ba-114">請注意，這表示它們不會隱藏任何 `_` （底線）名稱。</span><span class="sxs-lookup"><span data-stu-id="a50ba-114">Note this implies they do not cause any `_` (underscore) names to be hidden.</span></span>

<span data-ttu-id="a50ba-115">[簡單名稱](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#simple-names)如果 `K` 為零，且*simple_name*出現在*區塊*內，而且如果*區塊*的（或封閉式*區塊*的）本機變數宣告空間（宣告）包含區域變數、[參數（但](basic-concepts.md#declarations)捨棄參數除外）或具有名稱 `I`的常數，則*simple_name*會參考該區域變數、參數或常數，並將其分類為變數或值。</span><span class="sxs-lookup"><span data-stu-id="a50ba-115">[Simple names](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#simple-names) If `K` is zero and the *simple_name* appears within a *block* and if the *block*'s (or an enclosing *block*'s) local variable declaration space ([Declarations](basic-concepts.md#declarations)) contains a local variable, parameter (with the exception of discard parameters) or constant with name `I`, then the *simple_name* refers to that local variable, parameter or constant and is classified as a variable or value.</span></span>

<span data-ttu-id="a50ba-116">[範圍](https://github.com/dotnet/csharplang/blob/master/spec/basic-concepts.md#scopes)除了捨棄參數之外，在*lambda_expression* （[匿名](expressions.md#anonymous-function-expressions)函式運算式）中宣告的參數範圍是該*lambda_expression*的*anonymous_function_body* ，但捨棄參數除外，在*anonymous_method_expression*中宣告的參數範圍（匿名函式[運算式](expressions.md#anonymous-function-expressions)）是該*anonymous_method_expression*的*區塊*。</span><span class="sxs-lookup"><span data-stu-id="a50ba-116">[Scopes](https://github.com/dotnet/csharplang/blob/master/spec/basic-concepts.md#scopes) With the exception of discard parameters, the scope of a parameter declared in a *lambda_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) is the *anonymous_function_body* of that *lambda_expression* With the exception of discard parameters, the scope of a parameter declared in an *anonymous_method_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) is the *block* of that *anonymous_method_expression*.</span></span>

## <a name="related-spec-sections"></a><span data-ttu-id="a50ba-117">相關的規格區段</span><span class="sxs-lookup"><span data-stu-id="a50ba-117">Related spec sections</span></span>
- [<span data-ttu-id="a50ba-118">對應的參數</span><span class="sxs-lookup"><span data-stu-id="a50ba-118">Corresponding parameters</span></span>](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#corresponding-parameters)
