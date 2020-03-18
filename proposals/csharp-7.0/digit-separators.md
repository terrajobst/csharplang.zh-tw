---
ms.openlocfilehash: 5476f4438ad79a26b3615154f789d8ed04cb61aa
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484500"
---
# <a name="digit-separators"></a><span data-ttu-id="31160-101">數字分隔符號</span><span class="sxs-lookup"><span data-stu-id="31160-101">Digit separators</span></span>

<span data-ttu-id="31160-102">能夠將大型數值常值中的數位分組，會有很大的可讀性影響，而且沒有顯著的缺點。</span><span class="sxs-lookup"><span data-stu-id="31160-102">Being able to group digits in large numeric literals would have great readability impact and no significant downside.</span></span> 

<span data-ttu-id="31160-103">加入二進位常值（#215）會增加數值常值的可能性，因此這兩個功能會彼此增強。</span><span class="sxs-lookup"><span data-stu-id="31160-103">Adding binary literals (#215) would increase the likelihood of numeric literals being long, so the two features enhance each other.</span></span> 

<span data-ttu-id="31160-104">我們會遵循 JAVA 和其他專案，並使用底線 `_` 做為數位分隔符號。</span><span class="sxs-lookup"><span data-stu-id="31160-104">We would follow Java and others, and use an underscore `_` as a digit separator.</span></span> <span data-ttu-id="31160-105">它可以在數值常值中的任何位置（除了第一個和最後一個字元以外）發生，因為不同的群組在不同的情況下可能會有意義，特別是針對不同的數值基底：</span><span class="sxs-lookup"><span data-stu-id="31160-105">It would be able to occur everywhere in a numeric literal (except as the first and last character), since different groupings may make sense in different scenarios and especially for different numeric bases:</span></span>

```csharp
int bin = 0b1001_1010_0001_0100;
int hex = 0x1b_a0_44_fe;
int dec = 33_554_432;
int weird = 1_2__3___4____5_____6______7_______8________9;
double real = 1_000.111_1e-1_000;
```

<span data-ttu-id="31160-106">任何數位序列都可以用底線分隔，可能兩個連續數位之間有一個以上的底線。</span><span class="sxs-lookup"><span data-stu-id="31160-106">Any sequence of digits may be separated by underscores, possibly more than one underscore between two consecutive digits.</span></span> <span data-ttu-id="31160-107">它們可用於小數和指數，但遵循上一個規則，它們可能不會出現在小數點旁邊（`10_.0`）、指數位符（`1.1e_1`）或類型規範（`10_f`）旁邊。</span><span class="sxs-lookup"><span data-stu-id="31160-107">They are allowed in decimals as well as exponents, but following the previous rule, they may not appear next to the decimal (`10_.0`), next to the exponent character (`1.1e_1`), or next to the type specifier (`10_f`).</span></span> <span data-ttu-id="31160-108">在二進位和十六進位常值中使用時，它們可能不會緊接在 `0x` 或 `0b`後面。</span><span class="sxs-lookup"><span data-stu-id="31160-108">When used in binary and hexadecimal literals, they may not appear immediately following the `0x` or `0b`.</span></span>

<span data-ttu-id="31160-109">語法很簡單，而且分隔符號不會影響語義，而是只會被忽略。</span><span class="sxs-lookup"><span data-stu-id="31160-109">The syntax is straightforward, and the separators have no semantic impact - they are simply ignored.</span></span>

<span data-ttu-id="31160-110">這有廣泛的價值，而且很容易執行。</span><span class="sxs-lookup"><span data-stu-id="31160-110">This has broad value and is easy to implement.</span></span>
