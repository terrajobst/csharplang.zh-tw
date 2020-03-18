---
ms.openlocfilehash: 6f05bbc1365e59d737103b586db9d4a242c6d306
ms.sourcegitcommit: e9afb74cc1abd56db93b4b50bd5e6765e27c1c5d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/22/2019
ms.locfileid: "79485081"
---
# <a name="static-lambdas"></a>靜態 lambda

## <a name="summary"></a>摘要

支援 lambda，不允許從封入範圍中捕捉到狀態。

## <a name="motivation"></a>動機

避免不小心從封入內容捕捉狀態。

## <a name="detailed-design"></a>詳細設計

具有 `static` 的 lambda 無法從封閉範圍中捕捉狀態。
因此，在 `static` lambda 中無法使用封閉範圍內的區域變數、參數和 `this`。

`static` lambda 無法參考隱含或明確 `this` 或 `base` 參考中的實例成員。

`static` lambda 可能會參考封入範圍中 `static` 的成員。

`static` lambda 可能會參考來自封閉式範圍的 `constant` 定義。

`static` lambda 中的 `nameof()` 可能會參考來自封閉範圍的區域變數、參數或 `this` 或 `base`。

`static` 和非`static` lambda 的封閉範圍中 `private` 成員的協助工具規則都相同。

不保證 `static` lambda 定義是否會當做中繼資料中的 `static` 方法發出。 這會留給編譯器執行優化。

非`static` 的區域函式或 lambda 可以從封入 `static` lambda 捕捉狀態，但無法在封閉的 `static` lambda 外捕捉狀態。

`static` lambda 可以用於運算式樹狀架構中。

從有效程式中的 lambda 移除 `static` 修飾詞，並不會變更程式的意義。
