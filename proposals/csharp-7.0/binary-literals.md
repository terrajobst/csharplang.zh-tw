---
ms.openlocfilehash: 6f6c24e826e9fe9b9e8c97549add1029f00bcf60
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484423"
---
# <a name="binary-literals"></a><span data-ttu-id="c686e-101">二進位常值</span><span class="sxs-lookup"><span data-stu-id="c686e-101">Binary literals</span></span>

<span data-ttu-id="c686e-102">有一個相對常見的要求，可將二進位常C#值新增至和 VB。</span><span class="sxs-lookup"><span data-stu-id="c686e-102">There’s a relatively common request to add binary literals to C# and VB.</span></span> <span data-ttu-id="c686e-103">對於位元遮罩（例如旗標列舉）來說，這似乎真的有用，但它也很適合用於教育目的。</span><span class="sxs-lookup"><span data-stu-id="c686e-103">For bitmasks (e.g. flag enums) this seems genuinely useful, but it would also be great just for educational purposes.</span></span>

<span data-ttu-id="c686e-104">二進位常值看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="c686e-104">Binary literals would look like this:</span></span>

```csharp
int nineteen = 0b10011;
```

<span data-ttu-id="c686e-105">在語法上和語義上它們都與十六進位常值相同，但使用 `b`/`B` 而不是 `x`/`X`，只有數位 `0` 和 `1`，而且在基底2而不是16中進行轉譯。</span><span class="sxs-lookup"><span data-stu-id="c686e-105">Syntactically and semantically they are identical to hexadecimal literals, except for using `b`/`B` instead of `x`/`X`, having only digits `0` and `1` and being interpreted in base 2 instead of 16.</span></span>

<span data-ttu-id="c686e-106">執行這些工作的成本很少，而且對語言的使用者來說，概念上的負擔並不大。</span><span class="sxs-lookup"><span data-stu-id="c686e-106">There’s little cost to implementing these, and little conceptual overhead to users of the language.</span></span>

## <a name="syntax"></a><span data-ttu-id="c686e-107">語法</span><span class="sxs-lookup"><span data-stu-id="c686e-107">Syntax</span></span>

<span data-ttu-id="c686e-108">文法如下所示：</span><span class="sxs-lookup"><span data-stu-id="c686e-108">The grammar would be as follows:</span></span>

```antlr
integer-literal:
    : ...
    | binary-integer-literal
    ;
binary-integer-literal:
    : `0b` binary-digits integer-type-suffix-opt
    | `0B` binary-digits integer-type-suffix-opt
    ;
binary-digits:
    : binary-digit
    | binary-digits binary-digit
    ;
binary-digit:
    : `0`
    | `1`
    ;
```
