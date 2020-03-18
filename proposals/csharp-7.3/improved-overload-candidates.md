---
ms.openlocfilehash: 833ea0469149cbd434e8950e844740a3adb4d386
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484591"
---
# <a name="improved-overload-candidates"></a><span data-ttu-id="2f4e3-101">改進的多載候選項目</span><span class="sxs-lookup"><span data-stu-id="2f4e3-101">Improved overload candidates</span></span>

## <a name="summary"></a><span data-ttu-id="2f4e3-102">摘要</span><span class="sxs-lookup"><span data-stu-id="2f4e3-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="2f4e3-103">多載解析規則已在幾乎每個C#語言更新中更新，以改善程式設計人員的體驗，並讓不明確的調用選取「明顯」的選擇。</span><span class="sxs-lookup"><span data-stu-id="2f4e3-103">The overload resolution rules have been updated in nearly every C# language update to improve the experience for programmers, making ambiguous invocations select the "obvious" choice.</span></span> <span data-ttu-id="2f4e3-104">這必須謹慎進行，以維持回溯相容性，但因為我們通常會解決錯誤案例的問題，所以這些增強功能通常會正常地處理。</span><span class="sxs-lookup"><span data-stu-id="2f4e3-104">This has to be done carefully to preserve backward compatibility, but since we are usually resolving what would otherwise be error cases, these enhancements usually work out nicely.</span></span>

1. <span data-ttu-id="2f4e3-105">當方法群組同時包含實例和靜態成員時，我們會捨棄實例成員（如果叫用時沒有實例接收器或內容），並捨棄靜態成員（如果與實例接收者一起叫用的話）。</span><span class="sxs-lookup"><span data-stu-id="2f4e3-105">When a method group contains both instance and static members, we discard the instance members if invoked without an instance receiver or context, and discard the static members if invoked with an instance receiver.</span></span> <span data-ttu-id="2f4e3-106">當沒有接收者時，我們只會在靜態內容中包含靜態成員，否則會包括靜態和實例成員。</span><span class="sxs-lookup"><span data-stu-id="2f4e3-106">When there is no receiver, we include only static members in a static context, otherwise both static and instance members.</span></span> <span data-ttu-id="2f4e3-107">當接收者因為色彩色彩的情況而不明確實例或類型時，我們會同時包含這兩者。</span><span class="sxs-lookup"><span data-stu-id="2f4e3-107">When the receiver is ambiguously an instance or type due to a color-color situation, we include both.</span></span> <span data-ttu-id="2f4e3-108">無法使用隱含這個實例接收者的靜態內容，包括未定義此的成員主體，例如靜態成員，以及無法使用這個的位置，例如欄位初始化運算式和函式初始化運算式。</span><span class="sxs-lookup"><span data-stu-id="2f4e3-108">A static context, where an implicit this instance receiver cannot be used, includes the body of members where no this is defined, such as static members, as well as places where this cannot be used, such as field initializers and constructor-initializers.</span></span>
2. <span data-ttu-id="2f4e3-109">當方法群組包含某些型別引數不滿足其限制式的泛型方法時，這些成員將會從候選集合中移除。</span><span class="sxs-lookup"><span data-stu-id="2f4e3-109">When a method group contains some generic methods whose type arguments do not satisfy their constraints, these members are removed from the candidate set.</span></span>
3. <span data-ttu-id="2f4e3-110">針對方法群組轉換，其傳回型別與委派的傳回型別不符合的候選方法，將會從集合中移除。</span><span class="sxs-lookup"><span data-stu-id="2f4e3-110">For a method group conversion, candidate methods whose return type doesn't match up with the delegate's return type are removed from the set.</span></span>
