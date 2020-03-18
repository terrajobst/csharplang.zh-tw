---
ms.openlocfilehash: a4b0fbbc600eaf1e705ad8e6bd9fcecb44100761
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484437"
---
# <a name="local-functions"></a><span data-ttu-id="507ef-101">區域函式</span><span class="sxs-lookup"><span data-stu-id="507ef-101">Local functions</span></span>

<span data-ttu-id="507ef-102">我們擴充C#以支援區塊範圍中的函式宣告。</span><span class="sxs-lookup"><span data-stu-id="507ef-102">We extend C# to support the declaration of functions in block scope.</span></span> <span data-ttu-id="507ef-103">區域函式可以使用（capture）自封閉範圍中的變數。</span><span class="sxs-lookup"><span data-stu-id="507ef-103">Local functions may use (capture) variables from the enclosing scope.</span></span>

<span data-ttu-id="507ef-104">編譯器會使用流程分析來偵測區域函式在指派值之前所使用的變數。</span><span class="sxs-lookup"><span data-stu-id="507ef-104">The compiler uses flow analysis to detect which variables a local function uses before assigning it a value.</span></span> <span data-ttu-id="507ef-105">函式的每個呼叫都需要明確指派這類變數。</span><span class="sxs-lookup"><span data-stu-id="507ef-105">Every call of the function requires such variables to be definitely assigned.</span></span> <span data-ttu-id="507ef-106">同樣地，編譯器會決定要在傳回時明確指派的變數。</span><span class="sxs-lookup"><span data-stu-id="507ef-106">Similarly the compiler determines which variables are definitely assigned on return.</span></span> <span data-ttu-id="507ef-107">叫用區域函數之後，這類變數會被視為明確指派。</span><span class="sxs-lookup"><span data-stu-id="507ef-107">Such variables are considered definitely assigned after the local function is invoked.</span></span>

<span data-ttu-id="507ef-108">在定義之前，可以從詞法點呼叫區域函式。</span><span class="sxs-lookup"><span data-stu-id="507ef-108">Local functions may be called from a lexical point before its definition.</span></span> <span data-ttu-id="507ef-109">區域函式宣告語句不會在無法連線時造成警告。</span><span class="sxs-lookup"><span data-stu-id="507ef-109">Local function declaration statements do not cause a warning when they are not reachable.</span></span>

<span data-ttu-id="507ef-110">TODO：_寫入規格_</span><span class="sxs-lookup"><span data-stu-id="507ef-110">TODO: _WRITE SPEC_</span></span>

## <a name="syntax-grammar"></a><span data-ttu-id="507ef-111">語法文法</span><span class="sxs-lookup"><span data-stu-id="507ef-111">Syntax grammar</span></span>

<span data-ttu-id="507ef-112">這個文法是以與目前規格文法的差異來表示。</span><span class="sxs-lookup"><span data-stu-id="507ef-112">This grammar is represented as a diff from the current spec grammar.</span></span>

```diff
declaration-statement
    : local-variable-declaration ';'
    | local-constant-declaration ';'
+   | local-function-declaration
    ;

+local-function-declaration
+   : local-function-header local-function-body
+   ;

+local-function-header
+   : local-function-modifiers? return-type identifier type-parameter-list?
+       ( formal-parameter-list? ) type-parameter-constraints-clauses
+   ;

+local-function-modifiers
+   : (async | unsafe)
+   ;

+local-function-body
+   : block
+   | arrow-expression-body
+   ;
```

<span data-ttu-id="507ef-113">區域函式可以使用在封閉範圍中定義的變數。</span><span class="sxs-lookup"><span data-stu-id="507ef-113">Local functions may use variables defined in the enclosing scope.</span></span> <span data-ttu-id="507ef-114">目前的執行要求在區域函式中讀取的每個變數都必須明確指派，如同在其定義點執列區域函數一樣。</span><span class="sxs-lookup"><span data-stu-id="507ef-114">The current implementation requires that every variable read inside a local function be definitely assigned, as if executing the local function at its point of definition.</span></span> <span data-ttu-id="507ef-115">此外，區域函式定義必須在任何使用點上「執行」。</span><span class="sxs-lookup"><span data-stu-id="507ef-115">Also, the local function definition must have been "executed" at any use point.</span></span>

<span data-ttu-id="507ef-116">在實驗之後（例如，無法定義兩個互相遞迴的區域函式），我們自修改了我們希望明確指派的運作方式。</span><span class="sxs-lookup"><span data-stu-id="507ef-116">After experimenting with that a bit (for example, it is not possible to define two mutually recursive local functions), we've since revised how we want the definite assignment to work.</span></span> <span data-ttu-id="507ef-117">修訂（尚未實作為）是在區域函式的每次叫用時，都必須明確指派所有讀取于區域函式中的本機變數。</span><span class="sxs-lookup"><span data-stu-id="507ef-117">The revision (not yet implemented) is that all local variables read in a local function must be definitely assigned at each invocation of the local function.</span></span> <span data-ttu-id="507ef-118">這其實比聽起來更微妙，而且還有一堆工作可以讓它正常執行。</span><span class="sxs-lookup"><span data-stu-id="507ef-118">That's actually more subtle than it sounds, and there is a bunch of work remaining to make it work.</span></span> <span data-ttu-id="507ef-119">完成後，您就能夠將您的區域函式移至其封閉區塊的結尾。</span><span class="sxs-lookup"><span data-stu-id="507ef-119">Once it is done you'll be able to move your local functions to the end of its enclosing block.</span></span>

<span data-ttu-id="507ef-120">新的明確指派規則與推斷區域函式的傳回型別不相容，因此我們可能會移除推斷傳回型別的支援。</span><span class="sxs-lookup"><span data-stu-id="507ef-120">The new definite assignment rules are incompatible with inferring the return type of a local function, so we'll likely be removing support for inferring the return type.</span></span>

<span data-ttu-id="507ef-121">除非您將區域函式轉換成委派，否則會在實數值型別的框架中進行捕捉。</span><span class="sxs-lookup"><span data-stu-id="507ef-121">Unless you convert a local function to a delegate, capturing is done into frames that are value types.</span></span> <span data-ttu-id="507ef-122">這表示您不會因為使用區域函式與捕捉而產生任何 GC 壓力。</span><span class="sxs-lookup"><span data-stu-id="507ef-122">That means you don't get any GC pressure from using local functions with capturing.</span></span>

### <a name="reachability"></a><span data-ttu-id="507ef-123">達</span><span class="sxs-lookup"><span data-stu-id="507ef-123">Reachability</span></span>

<span data-ttu-id="507ef-124">我們將新增至規格</span><span class="sxs-lookup"><span data-stu-id="507ef-124">We add to the spec</span></span>

> <span data-ttu-id="507ef-125">語句主體 lambda 運算式或區域函式的主體會被視為可連接。</span><span class="sxs-lookup"><span data-stu-id="507ef-125">The body of a statement-bodied lambda expression or local function is considered reachable.</span></span>
