---
ms.openlocfilehash: 2070cf3b3269585055791adc3427cbd134df444d
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484654"
---
# <a name="pattern-based-fixed-statement"></a>模式型 `fixed` 陳述式

## <a name="summary"></a>摘要
[summary]: #summary

引進允許類型參與 `fixed` 語句的模式。 

## <a name="motivation"></a>動機
[motivation]: #motivation

語言提供一個機制來釘選 managed 資料，並取得基礎緩衝區的原生指標。

```csharp
fixed(byte* ptr = byteArray)
{
   // ptr is a native pointer to the first element of the array
   // byteArray is protected from being moved/collected by the GC for the duration of this block 
}

```

可以參與 `fixed` 的類型集合會進行硬式編碼，並限制為數組和 `System.String`。 引進新的基本專案（例如 `ImmutableArray<T>`、`Span<T>`、`Utf8String`）時，硬式編碼「特殊」類型不會進行調整。 

此外，`System.String` 的目前解決方案會依賴相當嚴格的 API。 API 的形狀意指 `System.String` 是連續的物件，它會在物件標頭的固定位移上內嵌 UTF16 編碼的資料。 這種方法在數個可能需要變更基礎配置的提案中發現問題。 最好能夠切換到更有彈性的東西，將 `System.String` 物件與內部標記法分開，以因應非受控 interop 的目的。 

## <a name="detailed-design"></a>詳細設計
[design]: #detailed-design

## <a name="pattern"></a>*模式* ##
以模式為基礎的可行「固定」需要：
-   提供受控參考來釘選實例並初始化指標（最好是相同的參考）
-   明確傳達非受控元素的類型（例如 "char" 代表 "string"）
-   在沒有可參考的情況下，規定「空白」案例中的行為。 
-   不應將 API 作者推送至不會在 `fixed`外損及類型的設計決策。

我認為可以藉由辨識特別命名的 ref 傳回成員來滿足上述條件： `ref [readonly] T GetPinnableReference()`。

為了供 `fixed` 語句使用，必須符合下列條件：

1. 其中只有一個為類型提供的成員。
1. 由 `ref` 或 `ref readonly`傳回。 （允許`readonly`，讓不可變/唯讀類型的作者可以在不新增可在安全程式碼中使用的可寫入 API 的情況下，執行模式）
1. T 是非受控型別。
（因為 `T*` 會變成指標類型。 當「非受控」的概念展開時，限制會自然地展開
1. 當沒有要釘選的資料時，會傳回 managed `nullptr` –可能是傳達是否空序列的最便宜方式。
（請注意，"" 字串會傳回 ' \ 0 ' 的參考，因為字串是以 null 終止）

或者，`#3` 我們可以允許空白案例中的結果為未定義或特定執行。 不過，這可能會使 API 變得更危險，而且容易發生濫用和非預期的相容性負擔。 

## <a name="translation"></a>*翻譯* ##

```csharp
fixed(byte* ptr = thing)
{ 
    // <BODY>
}
```

會成為下列虛擬代碼（並非所有可C#表達的）

```csharp
byte* ptr;
// specially decorated "pinned" IL local slot, not visible to user code.
pinned ref byte _pinned;

try
{
    // NOTE: null check is omitted for value types 
    // NOTE: `thing` is evaluated only once (temporary is introduced if necessary) 
    if (thing != null)
    {
        // obtain and "pin" the reference
        _pinned = ref thing.GetPinnableReference();

        // unsafe cast in IL
        ptr = (byte*)_pinned;
    }
    else
    {
        ptr = default(byte*);
    }

    // <BODY> 
}
finally   // finally can be omitted when not observable
{
    // "unpin" the object
    _pinned = nullptr;
}
```

## <a name="drawbacks"></a>缺點
[drawbacks]: #drawbacks

- GetPinnableReference 只能在 `fixed`中使用，但不會避免在安全程式碼中使用，因此實作項必須記住這一點。

## <a name="alternatives"></a>替代方案
[alternatives]: #alternatives

使用者可以引進 GetPinnableReference 或類似的成員，並將其當做
 
```csharp
fixed(byte* ptr = thing.GetPinnableReference())
{ 
    // <BODY>
}
```

如果需要替代方案，則沒有解決方案可供 `System.String`。

## <a name="unresolved-questions"></a>未解決的問題
[unresolved]: #unresolved-questions

- [] 處於「空白」狀態的行為。 - `nullptr` 或 `undefined`？ 
- [] 應該考慮擴充方法嗎？ 
- [] 如果在 `System.String`上偵測到模式，它是否會獲勝？ 

## <a name="design-meetings"></a>設計會議

還沒有。 
