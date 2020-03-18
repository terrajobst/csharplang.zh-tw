---
ms.openlocfilehash: aa0c233e7739ae7a0a7752251aae6f89df18ba93
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484514"
---
# <a name="c-language-proposals"></a>C#語言提案

語言提案是即時檔，描述指定語言功能的目前考慮。

提案*可以是作用中*、*非*作用中、已*拒絕*或已*完成*。 作用*中提案會*直接儲存在提案資料夾中、*非*作用中和已*拒絕*的提案會儲存在[非](proposals/inactive)作用中和已[拒絕](proposals/rejected)的子資料夾中，而*完成*的提案會封存在對應至其所屬語言版本的資料夾中。

## <a name="lifetime-of-a-proposal"></a>提案的存留期

當語言設計團隊認為它可能會在一天內加入這種語言時，就會開始使用提案。 一般來說，它會開始作用*中，但*如果我們想要在目前不想要處理的想法上進行討論，則提案也可以從*非*使用中的子資料夾開始。 如果我們想要記錄我們不想做的事，提案可能會直接從*拒絕*的狀態開始。 比方說，如果不可能執行熱門和週期性的要求，我們可以將其視為拒絕的提案。

本提案可能會從[討論問題](https://github.com/dotnet/csharplang/labels/Discussion)中的想法開始，也可能是來自語言設計會議的討論，或是從其他許多方面抵達。 主要的是，設計團隊認為應該這麼做，而且有其他人願意撰寫它。 通常，語言設計團隊的成員會將自己指派為功能的冠軍，受[擁護者問題](https://github.com/dotnet/csharplang/labels/Proposal%20champion)的追蹤。 擁護者會負責透過設計程式來移動提案。

如果提案正透過設計和實施到即將推出的版本進行 *，則會*有作用中的提議。 一旦完全*完成*（也就是，已將一個執行合併到一個版本中，並已指定該功能），就會將它移到對應至其發行的子目錄中。

如果某項功能不太可能使其成為語言，例如，因為它證明不可行，似乎不會加入足夠的值，或只是不適合語言，就會[遭到拒絕](proposals/rejected)。 如果功能有合理的承諾，但目前未排定優先順序，則可能會宣告為[非](proposals/inactive)使用中，以避免造成主要資料夾雜亂。 在非作用中或已遭拒絕的提案上進行工作，並于稍後 isdefunct，是很好的事。 類別目錄會反映目前的設計意圖。

## <a name="nature-of-a-proposal"></a>提案的本質

提案應遵循[提案範本](proposal-template.md)。 良好的提案應如下：

- 配合語言的一般精神和美觀。
- 不為現有的功能引進稍微替代的語法。
- 為一組清楚的使用者新增許多價值。
- 不會大幅增加語言的複雜性，特別是針對新的使用者。  

## <a name="discussion-of-proposals"></a>提案討論

意見反應和討論會在[討論問題](https://github.com/dotnet/csharplang/labels/Discussion)中進行。 將新的提案新增至提案資料夾時，應由擁護者或提案作者在討論問題中宣佈。 

 