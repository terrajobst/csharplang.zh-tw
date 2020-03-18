---
ms.openlocfilehash: 6f05bbc1365e59d737103b586db9d4a242c6d306
ms.sourcegitcommit: e9afb74cc1abd56db93b4b50bd5e6765e27c1c5d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/22/2019
ms.locfileid: "79485081"
---
# <a name="static-lambdas"></a><span data-ttu-id="d3259-101">靜態 lambda</span><span class="sxs-lookup"><span data-stu-id="d3259-101">Static lambdas</span></span>

## <a name="summary"></a><span data-ttu-id="d3259-102">摘要</span><span class="sxs-lookup"><span data-stu-id="d3259-102">Summary</span></span>

<span data-ttu-id="d3259-103">支援 lambda，不允許從封入範圍中捕捉到狀態。</span><span class="sxs-lookup"><span data-stu-id="d3259-103">Support lambdas that disallow capturing state from the enclosing scope.</span></span>

## <a name="motivation"></a><span data-ttu-id="d3259-104">動機</span><span class="sxs-lookup"><span data-stu-id="d3259-104">Motivation</span></span>

<span data-ttu-id="d3259-105">避免不小心從封入內容捕捉狀態。</span><span class="sxs-lookup"><span data-stu-id="d3259-105">Avoid unintentionally capturing state from the enclosing context.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="d3259-106">詳細設計</span><span class="sxs-lookup"><span data-stu-id="d3259-106">Detailed design</span></span>

<span data-ttu-id="d3259-107">具有 `static` 的 lambda 無法從封閉範圍中捕捉狀態。</span><span class="sxs-lookup"><span data-stu-id="d3259-107">A lambdas with `static` cannot capture state from the enclosing scope.</span></span>
<span data-ttu-id="d3259-108">因此，在 `static` lambda 中無法使用封閉範圍內的區域變數、參數和 `this`。</span><span class="sxs-lookup"><span data-stu-id="d3259-108">As a result, locals, parameters, and `this` from the enclosing scope are not available within a `static` lambda.</span></span>

<span data-ttu-id="d3259-109">`static` lambda 無法參考隱含或明確 `this` 或 `base` 參考中的實例成員。</span><span class="sxs-lookup"><span data-stu-id="d3259-109">A `static` lambda cannot reference instance members from an implicit or explicit `this` or `base` reference.</span></span>

<span data-ttu-id="d3259-110">`static` lambda 可能會參考封入範圍中 `static` 的成員。</span><span class="sxs-lookup"><span data-stu-id="d3259-110">A `static` lambda may reference `static` members from the enclosing scope.</span></span>

<span data-ttu-id="d3259-111">`static` lambda 可能會參考來自封閉式範圍的 `constant` 定義。</span><span class="sxs-lookup"><span data-stu-id="d3259-111">A `static` lambda may reference `constant` definitions from the enclosing scope.</span></span>

<span data-ttu-id="d3259-112">`static` lambda 中的 `nameof()` 可能會參考來自封閉範圍的區域變數、參數或 `this` 或 `base`。</span><span class="sxs-lookup"><span data-stu-id="d3259-112">`nameof()` in a `static` lambda may reference locals, parameters, or `this` or `base` from the enclosing scope.</span></span>

<span data-ttu-id="d3259-113">`static` 和非`static` lambda 的封閉範圍中 `private` 成員的協助工具規則都相同。</span><span class="sxs-lookup"><span data-stu-id="d3259-113">Accessibility rules for `private` members in the enclosing scope are the same for `static` and non-`static` lambdas.</span></span>

<span data-ttu-id="d3259-114">不保證 `static` lambda 定義是否會當做中繼資料中的 `static` 方法發出。</span><span class="sxs-lookup"><span data-stu-id="d3259-114">No guarantee is made as to whether a `static` lambda definition is emitted as a `static` method in metadata.</span></span> <span data-ttu-id="d3259-115">這會留給編譯器執行優化。</span><span class="sxs-lookup"><span data-stu-id="d3259-115">This is left up to the compiler implementation to optimize.</span></span>

<span data-ttu-id="d3259-116">非`static` 的區域函式或 lambda 可以從封入 `static` lambda 捕捉狀態，但無法在封閉的 `static` lambda 外捕捉狀態。</span><span class="sxs-lookup"><span data-stu-id="d3259-116">A non-`static` local function or lambda can capture state from an enclosing `static` lambda but cannot capture state outside the enclosing `static` lambda.</span></span>

<span data-ttu-id="d3259-117">`static` lambda 可以用於運算式樹狀架構中。</span><span class="sxs-lookup"><span data-stu-id="d3259-117">A `static` lambda can be used in an expression tree.</span></span>

<span data-ttu-id="d3259-118">從有效程式中的 lambda 移除 `static` 修飾詞，並不會變更程式的意義。</span><span class="sxs-lookup"><span data-stu-id="d3259-118">Removing the `static` modifier from a lambda in a valid program does not change the meaning of the program.</span></span>
