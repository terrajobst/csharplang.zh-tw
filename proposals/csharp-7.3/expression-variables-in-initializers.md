---
ms.openlocfilehash: a78567594d39fc4e204e12c4f2f247b8d6995c38
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484920"
---
# <a name="expression-variables-in-initializers"></a><span data-ttu-id="eec8e-101">初始設定式中的運算式變數</span><span class="sxs-lookup"><span data-stu-id="eec8e-101">Expression variables in initializers</span></span>

## <a name="summary"></a><span data-ttu-id="eec8e-102">摘要</span><span class="sxs-lookup"><span data-stu-id="eec8e-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="eec8e-103">我們擴充7中C#引進的功能，以允許運算式包含欄位初始化運算式中的運算式變數（out 變數宣告和宣告模式）、屬性初始化運算式、ctor 初始化運算式和查詢子句。</span><span class="sxs-lookup"><span data-stu-id="eec8e-103">We extend the features introduced in C# 7 to permit expressions containing expression variables (out variable declarations and declaration patterns) in field initializers, property initializers, ctor-initializers, and query clauses.</span></span>

## <a name="motivation"></a><span data-ttu-id="eec8e-104">動機</span><span class="sxs-lookup"><span data-stu-id="eec8e-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="eec8e-105">因為缺乏時間，所以這會在C#語言中完成幾個粗略的邊緣。</span><span class="sxs-lookup"><span data-stu-id="eec8e-105">This completes a couple of the rough edges left in the C# language due to lack of time.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="eec8e-106">詳細設計</span><span class="sxs-lookup"><span data-stu-id="eec8e-106">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="eec8e-107">我們會移除限制，以避免在 ctor 初始化運算式中宣告運算式變數（out 變數宣告和宣告模式）。</span><span class="sxs-lookup"><span data-stu-id="eec8e-107">We remove the restriction preventing the declaration of expression variables (out variable declarations and declaration patterns) in a ctor-initializer.</span></span> <span data-ttu-id="eec8e-108">這類宣告的變數會在整個建構函式主體的範圍內。</span><span class="sxs-lookup"><span data-stu-id="eec8e-108">Such a declared variable is in scope throughout the body of the constructor.</span></span>

<span data-ttu-id="eec8e-109">我們會移除限制，防止在欄位或屬性初始化運算式中宣告運算式變數（out 變數宣告和宣告模式）。</span><span class="sxs-lookup"><span data-stu-id="eec8e-109">We remove the restriction preventing the declaration of expression variables (out variable declarations and declaration patterns) in a field or property initializer.</span></span> <span data-ttu-id="eec8e-110">這類宣告的變數會在整個初始化運算式的範圍中。</span><span class="sxs-lookup"><span data-stu-id="eec8e-110">Such a declared variable is in scope throughout the initializing expression.</span></span>

<span data-ttu-id="eec8e-111">我們會移除限制，以防止在轉譯成 lambda 主體的查詢運算式子句中宣告運算式變數（out 變數宣告和宣告模式）。</span><span class="sxs-lookup"><span data-stu-id="eec8e-111">We remove the restriction preventing the declaration of expression variables (out variable declarations and declaration patterns) in a query expression clause that is translated into the body of a lambda.</span></span> <span data-ttu-id="eec8e-112">這類宣告的變數會在查詢子句的整個運算式範圍內。</span><span class="sxs-lookup"><span data-stu-id="eec8e-112">Such a declared variable is in scope throughout that expression of the query clause.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="eec8e-113">缺點</span><span class="sxs-lookup"><span data-stu-id="eec8e-113">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="eec8e-114">無。</span><span class="sxs-lookup"><span data-stu-id="eec8e-114">None.</span></span>

## <a name="alternatives"></a><span data-ttu-id="eec8e-115">替代方案</span><span class="sxs-lookup"><span data-stu-id="eec8e-115">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="eec8e-116">這些內容中所宣告之運算式變數的適當範圍並不明顯，且值得進一步的 LDM 討論。</span><span class="sxs-lookup"><span data-stu-id="eec8e-116">The appropriate scope for expression variables declared in these contexts is not obvious, and deserves further LDM discussion.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="eec8e-117">未解決的問題</span><span class="sxs-lookup"><span data-stu-id="eec8e-117">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="eec8e-118">[] 這些變數的適當範圍為何？</span><span class="sxs-lookup"><span data-stu-id="eec8e-118">[ ] What is the appropriate scope for these variables?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="eec8e-119">設計會議</span><span class="sxs-lookup"><span data-stu-id="eec8e-119">Design meetings</span></span>

<span data-ttu-id="eec8e-120">無。</span><span class="sxs-lookup"><span data-stu-id="eec8e-120">None.</span></span>
