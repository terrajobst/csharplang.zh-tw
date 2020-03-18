---
ms.openlocfilehash: 2532a24e867930d2f27614f19c77585dbce80dfa
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484542"
---
# <a name="throw-expression"></a><span data-ttu-id="d5ea4-101">Throw 運算式</span><span class="sxs-lookup"><span data-stu-id="d5ea4-101">Throw expression</span></span>

<span data-ttu-id="d5ea4-102">我們會擴充要包含的運算式表單集合</span><span class="sxs-lookup"><span data-stu-id="d5ea4-102">We extend the set of expression forms to include</span></span>

```antlr
throw_expression
    : 'throw' null_coalescing_expression
    ;

null_coalescing_expression
    : throw_expression
    ;
```

<span data-ttu-id="d5ea4-103">類型規則如下所示：</span><span class="sxs-lookup"><span data-stu-id="d5ea4-103">The type rules are as follows:</span></span>

- <span data-ttu-id="d5ea4-104">*Throw_expression*沒有類型。</span><span class="sxs-lookup"><span data-stu-id="d5ea4-104">A *throw_expression* has no type.</span></span>
- <span data-ttu-id="d5ea4-105">*Throw_expression*可以透過隱含轉換，轉換成每一種類型。</span><span class="sxs-lookup"><span data-stu-id="d5ea4-105">A *throw_expression* is convertible to every type by an implicit conversion.</span></span>

<span data-ttu-id="d5ea4-106">*Throw 運算式會擲*回評估*null_coalescing_expression*所產生的值，這必須代表衍生自 `System.Exception` 的類別型別值 `System.Exception`，或是具有 `System.Exception` （或其子類別）做為其有效基類的型別參數型別。</span><span class="sxs-lookup"><span data-stu-id="d5ea4-106">A *throw expression* throws the value produced by evaluating the *null_coalescing_expression*, which must denote a value of the class type `System.Exception`, of a class type that derives from `System.Exception` or of a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span> <span data-ttu-id="d5ea4-107">如果運算式的評估產生 `null`，會改為擲回 `System.NullReferenceException`。</span><span class="sxs-lookup"><span data-stu-id="d5ea4-107">If evaluation of the expression produces `null`, a `System.NullReferenceException` is thrown instead.</span></span>

<span data-ttu-id="d5ea4-108">在執行時間評估*throw 運算式*的行為與[針對*throw 語句*所指定](../../spec/statements.md#the-throw-statement)的行為相同。</span><span class="sxs-lookup"><span data-stu-id="d5ea4-108">The behavior at runtime of the evaluation of a *throw expression* is the same [as specified for a *throw statement*](../../spec/statements.md#the-throw-statement).</span></span>

<span data-ttu-id="d5ea4-109">流程分析規則如下所示：</span><span class="sxs-lookup"><span data-stu-id="d5ea4-109">The flow-analysis rules are as follows:</span></span>

- <span data-ttu-id="d5ea4-110">針對每個*變數 v*，在*throw_expression* iff 之前，會*null_coalescing_expression*先明確指派*v* ，而不是在*throw_expression*之前明確指派。</span><span class="sxs-lookup"><span data-stu-id="d5ea4-110">For every variable *v*, *v* is definitely assigned before the *null_coalescing_expression* of a *throw_expression* iff it is definitely assigned before the *throw_expression*.</span></span>
- <span data-ttu-id="d5ea4-111">針對每個變數*v*，會在*throw_expression*之後明確指派*v* 。</span><span class="sxs-lookup"><span data-stu-id="d5ea4-111">For every variable *v*, *v* is definitely assigned after *throw_expression*.</span></span>

<span data-ttu-id="d5ea4-112">只有下列語法內容允許*throw 運算式*：</span><span class="sxs-lookup"><span data-stu-id="d5ea4-112">A *throw expression* is permitted in only the following syntactic contexts:</span></span>
- <span data-ttu-id="d5ea4-113">做為三元條件運算子的第二個或第三個運算元 `?:`</span><span class="sxs-lookup"><span data-stu-id="d5ea4-113">As the second or third operand of a ternary conditional operator `?:`</span></span>
- <span data-ttu-id="d5ea4-114">當做 null 聯合運算子的第二個運算元 `??`</span><span class="sxs-lookup"><span data-stu-id="d5ea4-114">As the second operand of a null coalescing operator `??`</span></span>
- <span data-ttu-id="d5ea4-115">做為運算式主體 lambda 或方法的主體。</span><span class="sxs-lookup"><span data-stu-id="d5ea4-115">As the body of an expression-bodied lambda or method.</span></span>
