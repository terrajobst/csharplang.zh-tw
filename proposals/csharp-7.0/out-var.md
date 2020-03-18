---
ms.openlocfilehash: a0d80afc47e9f0073237db9b8d7a4f0b045c1b0b
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484507"
---
# <a name="out-variable-declarations"></a><span data-ttu-id="92177-101">Out 變數宣告</span><span class="sxs-lookup"><span data-stu-id="92177-101">Out variable declarations</span></span>

<span data-ttu-id="92177-102">*Out 變數*宣告功能可讓變數在當做 `out` 引數傳遞的位置進行宣告。</span><span class="sxs-lookup"><span data-stu-id="92177-102">The *out variable declaration* feature enables a variable to be declared at the location that it is being passed as an `out` argument.</span></span>

```antlr
argument_value
    : 'out' type identifier
    | ...
    ;
```

<span data-ttu-id="92177-103">以這種方式宣告的變數稱為*out 變數*。</span><span class="sxs-lookup"><span data-stu-id="92177-103">A variable declared this way is called an *out variable*.</span></span> <span data-ttu-id="92177-104">您可以將內容關鍵字 `var` 用於變數的類型。</span><span class="sxs-lookup"><span data-stu-id="92177-104">You may use the contextual keyword `var` for the variable's type.</span></span> <span data-ttu-id="92177-105">範圍會與透過模式比對所引進的*模式變數*相同。</span><span class="sxs-lookup"><span data-stu-id="92177-105">The scope will be the same as for a *pattern-variable* introduced via pattern-matching.</span></span>

<span data-ttu-id="92177-106">根據語言規格（區段7.6.7 元素存取），元素存取（索引運算式）的引數清單不包含 ref 或 out 引數。</span><span class="sxs-lookup"><span data-stu-id="92177-106">According to Language Specification (section 7.6.7 Element access) the argument-list of an element-access (indexing expression) does not contain ref or out arguments.</span></span> <span data-ttu-id="92177-107">不過，編譯器會針對各種案例（例如，在接受 `out`的中繼資料中宣告的索引子）允許它們。</span><span class="sxs-lookup"><span data-stu-id="92177-107">However, they are permitted by the compiler for various scenarios, for example indexers declared in metadata that accept `out`.</span></span>

<span data-ttu-id="92177-108">在 argument_value 所引進的區域變數範圍內，在其宣告之前的文字位置中參考該區域變數是編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="92177-108">Within the scope of a local variable introduced by an argument_value, it is a compile-time error to refer to that local variable in a textual position that precedes its declaration.</span></span>

<span data-ttu-id="92177-109">在立即包含其宣告的同一個引數清單中，參考隱含類型（§8.5.1） out 變數也是錯誤。</span><span class="sxs-lookup"><span data-stu-id="92177-109">It is also an error to reference an implicitly-typed (§8.5.1) out variable in the same argument list that immediately contains its declaration.</span></span>

<span data-ttu-id="92177-110">多載解析的修改方式如下：</span><span class="sxs-lookup"><span data-stu-id="92177-110">Overload resolution is modified as follows:</span></span>

<span data-ttu-id="92177-111">我們會加入新的轉換：</span><span class="sxs-lookup"><span data-stu-id="92177-111">We add a new conversion:</span></span>

> <span data-ttu-id="92177-112">*從運算式轉換*成每個型別都有一個從隱含型別 out 變數宣告到每種類型。</span><span class="sxs-lookup"><span data-stu-id="92177-112">There is a *conversion from expression* from an implicitly-typed out variable declaration to every type.</span></span>

<span data-ttu-id="92177-113">還要</span><span class="sxs-lookup"><span data-stu-id="92177-113">Also</span></span>

> <span data-ttu-id="92177-114">明確類型 out 變數引數的類型是宣告的類型。</span><span class="sxs-lookup"><span data-stu-id="92177-114">The type of an explicitly-typed out variable argument is the declared type.</span></span>

<span data-ttu-id="92177-115">和</span><span class="sxs-lookup"><span data-stu-id="92177-115">and</span></span>

> <span data-ttu-id="92177-116">隱含類型的 out 變數引數沒有類型。</span><span class="sxs-lookup"><span data-stu-id="92177-116">An implicitly-typed out variable argument has no type.</span></span>

<span data-ttu-id="92177-117">從運算式*轉換*成隱含類型的 out 變數宣告，並不會比*從運算式*進行任何其他轉換更好。</span><span class="sxs-lookup"><span data-stu-id="92177-117">The *conversion from expression* from an implicitly-typed out variable declaration is not considered better than any other *conversion from expression*.</span></span>

<span data-ttu-id="92177-118">隱含類型 out 變數的類型是多載解析所選取之方法簽章中對應參數的類型。</span><span class="sxs-lookup"><span data-stu-id="92177-118">The type of an implicitly-typed out variable is the type of the corresponding parameter in the signature of the method selected by overload resolution.</span></span>

<span data-ttu-id="92177-119">新增了新的語法節點 `DeclarationExpressionSyntax`，以代表 out var 引數中的宣告。</span><span class="sxs-lookup"><span data-stu-id="92177-119">The new syntax node `DeclarationExpressionSyntax` is added to represent the declaration in an out var argument.</span></span>
