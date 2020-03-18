---
ms.openlocfilehash: fecd5a6c1e0f6c7a7a7beac0e4e60445281c7846
ms.sourcegitcommit: 1b488e76c2c07aafc377bc5e8a7197252c82f425
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2019
ms.locfileid: "79485095"
---
# <a name="static-local-functions"></a>靜態區域函式

## <a name="summary"></a>摘要

支援禁止從封閉範圍捕捉狀態的區域函式。

## <a name="motivation"></a>動機

避免不小心從封入內容捕捉狀態。
允許在需要 `static` 方法的情況下使用區域函式。

## <a name="detailed-design"></a>詳細設計

宣告的區域函式 `static` 無法從封入範圍中捕捉狀態。
因此，在 `static` 區域函式內無法使用封閉範圍中的區域變數、參數和 `this`。

`static` 區域函數無法從隱含或明確 `this` 或 `base` 參考中參考實例成員。

`static` 區域函式可以參考封入範圍中 `static` 的成員。

`static` 區域函式可以參考來自封閉式範圍的 `constant` 定義。

`static` 區域函式中的 `nameof()` 可能會參考來自封閉範圍的區域變數、參數或 `this` 或 `base`。

對於 `static` 和非`static` 的區域函式而言，封閉範圍中 `private` 成員的存取範圍規則都相同。

`static` 區域函式定義會當做中繼資料中的 `static` 方法發出，即使只在委派中使用也是如此。

非`static` 的區域函式或 lambda 可以從封閉的 `static` 區域函數中捕捉狀態，但無法在封閉的 `static` 區域函式外捕捉狀態。

不能在運算式樹狀架構中叫用 `static` 區域函數。

不論是否 `static`區域函式，對區域函數的呼叫都會以 `call` 而不是 `callvirt`發出。

區域函式中呼叫的多載解析不會受到本機函式是否 `static`影響。

從有效程式中的區域函式移除 `static` 修飾詞，並不會變更程式的意義。

## <a name="design-meetings"></a>設計會議

https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-09-10.md#static-local-functions
