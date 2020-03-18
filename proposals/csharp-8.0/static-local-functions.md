---
ms.openlocfilehash: fecd5a6c1e0f6c7a7a7beac0e4e60445281c7846
ms.sourcegitcommit: 1b488e76c2c07aafc377bc5e8a7197252c82f425
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2019
ms.locfileid: "79485095"
---
# <a name="static-local-functions"></a><span data-ttu-id="1c21b-101">靜態區域函式</span><span class="sxs-lookup"><span data-stu-id="1c21b-101">Static local functions</span></span>

## <a name="summary"></a><span data-ttu-id="1c21b-102">摘要</span><span class="sxs-lookup"><span data-stu-id="1c21b-102">Summary</span></span>

<span data-ttu-id="1c21b-103">支援禁止從封閉範圍捕捉狀態的區域函式。</span><span class="sxs-lookup"><span data-stu-id="1c21b-103">Support local functions that disallow capturing state from the enclosing scope.</span></span>

## <a name="motivation"></a><span data-ttu-id="1c21b-104">動機</span><span class="sxs-lookup"><span data-stu-id="1c21b-104">Motivation</span></span>

<span data-ttu-id="1c21b-105">避免不小心從封入內容捕捉狀態。</span><span class="sxs-lookup"><span data-stu-id="1c21b-105">Avoid unintentionally capturing state from the enclosing context.</span></span>
<span data-ttu-id="1c21b-106">允許在需要 `static` 方法的情況下使用區域函式。</span><span class="sxs-lookup"><span data-stu-id="1c21b-106">Allow local functions to be used in scenarios where a `static` method is required.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="1c21b-107">詳細設計</span><span class="sxs-lookup"><span data-stu-id="1c21b-107">Detailed design</span></span>

<span data-ttu-id="1c21b-108">宣告的區域函式 `static` 無法從封入範圍中捕捉狀態。</span><span class="sxs-lookup"><span data-stu-id="1c21b-108">A local function declared `static` cannot capture state from the enclosing scope.</span></span>
<span data-ttu-id="1c21b-109">因此，在 `static` 區域函式內無法使用封閉範圍中的區域變數、參數和 `this`。</span><span class="sxs-lookup"><span data-stu-id="1c21b-109">As a result, locals, parameters, and `this` from the enclosing scope are not available within a `static` local function.</span></span>

<span data-ttu-id="1c21b-110">`static` 區域函數無法從隱含或明確 `this` 或 `base` 參考中參考實例成員。</span><span class="sxs-lookup"><span data-stu-id="1c21b-110">A `static` local function cannot reference instance members from an implicit or explicit `this` or `base` reference.</span></span>

<span data-ttu-id="1c21b-111">`static` 區域函式可以參考封入範圍中 `static` 的成員。</span><span class="sxs-lookup"><span data-stu-id="1c21b-111">A `static` local function may reference `static` members from the enclosing scope.</span></span>

<span data-ttu-id="1c21b-112">`static` 區域函式可以參考來自封閉式範圍的 `constant` 定義。</span><span class="sxs-lookup"><span data-stu-id="1c21b-112">A `static` local function may reference `constant` definitions from the enclosing scope.</span></span>

<span data-ttu-id="1c21b-113">`static` 區域函式中的 `nameof()` 可能會參考來自封閉範圍的區域變數、參數或 `this` 或 `base`。</span><span class="sxs-lookup"><span data-stu-id="1c21b-113">`nameof()` in a `static` local function may reference locals, parameters, or `this` or `base` from the enclosing scope.</span></span>

<span data-ttu-id="1c21b-114">對於 `static` 和非`static` 的區域函式而言，封閉範圍中 `private` 成員的存取範圍規則都相同。</span><span class="sxs-lookup"><span data-stu-id="1c21b-114">Accessibility rules for `private` members in the enclosing scope are the same for `static` and non-`static` local functions.</span></span>

<span data-ttu-id="1c21b-115">`static` 區域函式定義會當做中繼資料中的 `static` 方法發出，即使只在委派中使用也是如此。</span><span class="sxs-lookup"><span data-stu-id="1c21b-115">A `static` local function definition is emitted as a `static` method in metadata, even if only used in a delegate.</span></span>

<span data-ttu-id="1c21b-116">非`static` 的區域函式或 lambda 可以從封閉的 `static` 區域函數中捕捉狀態，但無法在封閉的 `static` 區域函式外捕捉狀態。</span><span class="sxs-lookup"><span data-stu-id="1c21b-116">A non-`static` local function or lambda can capture state from an enclosing `static` local function but cannot capture state outside the enclosing `static` local function.</span></span>

<span data-ttu-id="1c21b-117">不能在運算式樹狀架構中叫用 `static` 區域函數。</span><span class="sxs-lookup"><span data-stu-id="1c21b-117">A `static` local function cannot be invoked in an expression tree.</span></span>

<span data-ttu-id="1c21b-118">不論是否 `static`區域函式，對區域函數的呼叫都會以 `call` 而不是 `callvirt`發出。</span><span class="sxs-lookup"><span data-stu-id="1c21b-118">A call to a local function is emitted as `call` rather than `callvirt`, regardless of whether the local function is `static`.</span></span>

<span data-ttu-id="1c21b-119">區域函式中呼叫的多載解析不會受到本機函式是否 `static`影響。</span><span class="sxs-lookup"><span data-stu-id="1c21b-119">Overload resolution of a call within a local function not affected by whether the local function is `static`.</span></span>

<span data-ttu-id="1c21b-120">從有效程式中的區域函式移除 `static` 修飾詞，並不會變更程式的意義。</span><span class="sxs-lookup"><span data-stu-id="1c21b-120">Removing the `static` modifier from a local function in a valid program does not change the meaning of the program.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="1c21b-121">設計會議</span><span class="sxs-lookup"><span data-stu-id="1c21b-121">Design meetings</span></span>

https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-09-10.md#static-local-functions
