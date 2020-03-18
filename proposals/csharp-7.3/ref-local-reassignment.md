---
ms.openlocfilehash: c1a77d9337ecd16f5ea1c30d18f6422552b0dfb2
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484598"
---
# <a name="ref-local-reassignment"></a><span data-ttu-id="825b5-101">Ref 本機重新指派</span><span class="sxs-lookup"><span data-stu-id="825b5-101">Ref Local Reassignment</span></span>

<span data-ttu-id="825b5-102">在C# 7.3 中，我們新增了重新系結 ref 區域變數或 ref 參數之參考的支援。</span><span class="sxs-lookup"><span data-stu-id="825b5-102">In C# 7.3, we add support for rebinding the referent of a ref local variable or a ref parameter.</span></span>

<span data-ttu-id="825b5-103">我們會將下列新增至 `assignment_operator`的集合中。</span><span class="sxs-lookup"><span data-stu-id="825b5-103">We add the following to the set of `assignment_operator`s.</span></span>

```antlr
assignment_operator
    : '=' 'ref'
    ;
```

<span data-ttu-id="825b5-104">`=ref` 運算子稱為***ref 指派運算子***。</span><span class="sxs-lookup"><span data-stu-id="825b5-104">The `=ref` operator is called the ***ref assignment operator***.</span></span> <span data-ttu-id="825b5-105">這不是*複合指派運算子*。</span><span class="sxs-lookup"><span data-stu-id="825b5-105">It is not a *compound assignment operator*.</span></span> <span data-ttu-id="825b5-106">左運算元必須是系結至 ref 區域變數、ref 參數（不是 `this`）或 out 參數的運算式。</span><span class="sxs-lookup"><span data-stu-id="825b5-106">The left operand must be an expression that binds to a ref local variable, a ref parameter (other than `this`), or an out parameter.</span></span> <span data-ttu-id="825b5-107">右運算元必須是產生左值的運算式，並指定與左運算元相同類型的值。</span><span class="sxs-lookup"><span data-stu-id="825b5-107">The right operand must be an expression that yields an lvalue designating a value of the same type as the left operand.</span></span>

<span data-ttu-id="825b5-108">右運算元必須在 ref 指派的位置明確指派。</span><span class="sxs-lookup"><span data-stu-id="825b5-108">The right operand must be definitely assigned at the point of the ref assignment.</span></span>

<span data-ttu-id="825b5-109">當左運算元系結至 `out` 參數時，如果未在 ref 指派運算子的開頭明確指派 `out` 參數，則會產生錯誤。</span><span class="sxs-lookup"><span data-stu-id="825b5-109">When the left operand binds to an `out` parameter, it is an error if that `out` parameter has not been definitely assigned at the beginning of the ref assignment operator.</span></span>

<span data-ttu-id="825b5-110">如果左運算元是可寫入的 ref （也就是指定了 `ref readonly` 本機或 `in` 參數以外的任何值），則右運算元必須是可寫入的左值。</span><span class="sxs-lookup"><span data-stu-id="825b5-110">If the left operand is a writeable ref (i.e. it designates anything other than a `ref readonly` local or  `in` parameter), then the right operand must be a writeable lvalue.</span></span>

<span data-ttu-id="825b5-111">Ref 指派運算子會產生指派類型的左值。</span><span class="sxs-lookup"><span data-stu-id="825b5-111">The ref assignment operator yields an lvalue of the assigned type.</span></span> <span data-ttu-id="825b5-112">如果左運算元是可寫入的（亦即，不是 `ref readonly` 或 `in`），則可寫入。</span><span class="sxs-lookup"><span data-stu-id="825b5-112">It is writeable if the left operand is writeable (i.e. not `ref readonly` or `in`).</span></span>

<span data-ttu-id="825b5-113">此運算子的安全性規則包括：</span><span class="sxs-lookup"><span data-stu-id="825b5-113">The safety rules for this operator are:</span></span>

- <span data-ttu-id="825b5-114">針對 ref 重新指派 `e1 = ref e2`，`e2` 的*ref 安全對 escape*必須至少與 `e1`的*ref 安全到 escape*的範圍相同。</span><span class="sxs-lookup"><span data-stu-id="825b5-114">For a ref reassignment `e1 = ref e2`, the *ref-safe-to-escape* of `e2` must be at least as wide a scope as the *ref-safe-to-escape* of `e1`.</span></span>

<span data-ttu-id="825b5-115">在 ref 型別的[安全性](../csharp-7.2/span-safety.md)中，會定義*ref-安全對 escape*</span><span class="sxs-lookup"><span data-stu-id="825b5-115">Where *ref-safe-to-escape* is defined in [Safety for ref-like types](../csharp-7.2/span-safety.md)</span></span>
