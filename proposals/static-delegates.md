---
ms.openlocfilehash: a8822137c85f449444ed675c6f2912315c041691
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484472"
---
# <a name="static-delegates"></a>靜態委派

* [x] 提議
* [] 原型：未啟動
* [] 執行：未啟動
* [] 規格：未啟動

## <a name="summary"></a>摘要
[summary]: #summary

提供一般用途、輕量的C#回呼功能給語言。

## <a name="motivation"></a>動機
[motivation]: #motivation

目前，使用者能夠使用 `System.Delegate` 類型來建立回呼。 不過，這些都是相當龐大的（例如，需要堆積配置，並一律處理將回呼連結在一起）。

此外，`System.Delegate` 不會提供使用非受控函式指標的最佳互通性，也就是因為不是全像的，而且會在它跨越受控/非受控界限時要求封送處理。

只要稍加調整，我們就可以使用原生程式碼，提供輕量、一般用途及 interops 的新委派類型。

## <a name="detailed-design"></a>詳細設計
[design]: #detailed-design

其中一個會透過下列方式宣告靜態委派：

```C#
static delegate int Func()
```

其中一個可以額外使用類似于 `System.Runtime.InteropServices.UnmanagedFunctionPointer` 的宣告來設定宣告，以便控制呼叫慣例、字串封送處理和設定最後一次錯誤行為。 注意：使用 `System.Runtime.InteropServices.UnmanagedFunctionPointer` 本身將無法執行，因為它僅適用于委派。

宣告會轉譯成編譯器的內部標記法，如下所示

```C#
struct <Func>e__StaticDelegate
{
    IntPtr pFunction;

    static int WellKnownCompilerName();
}
```

也就是說，它是由具有 `IntPtr` 類型之單一成員的結構內部表示（這類結構是可直接完成的，而且不會產生任何堆積配置）：
* 成員包含要做為回呼的函式位址。
* 型別會宣告符合回呼之方法簽章的方法。
* 結構的名稱不應為使用者可建構（與其他內部產生的支援結構相同）。
 * 例如：固定大小緩衝區會產生名稱格式為 `<name>e__FixedBuffer` 的結構（`<`，而 `>` 是識別碼的一部分，並使識別碼不可建構在中C#，但在 IL 中仍然可用）。
* 方法宣告的名稱應該是在所有靜態委派類型上使用的知名名稱（這可讓編譯器知道在判斷簽章時所要尋找的名稱）。

靜態委派的值只能系結至符合回呼簽章的靜態方法。

不支援將回呼連結在一起。

回呼的調用會由 `calli` 指令來執行。

## <a name="drawbacks"></a>缺點
[drawbacks]: #drawbacks

靜態委派不適用於使用一般委派的現有 Api （其中一個必須在相同簽章的一般委派中包裝所謂的靜態委派）。
* 假設 `System.Delegate` 在內部表示為一組 `object` 和 `IntPtr` 欄位（ http://source.dot.net/#System.Private.CoreLib/src/System/Delegate.cs)，則可以允許將靜態委派隱含轉換成具有相符方法簽章的 `System.Delegate`。 您也可以在相反的方向提供明確的轉換，前提是 `System.Delegate` 符合靜態委派的所有需求。

需要額外的工作，才能讓靜態委派在核心架構中立即可用。

## <a name="alternatives"></a>替代方案
[alternatives]: #alternatives

TBD

## <a name="unresolved-questions"></a>未解決的問題
[unresolved]: #unresolved-questions

設計的哪些部分仍待 TBD？

## <a name="design-meetings"></a>設計會議

連結至會影響此提案的設計附注，並針對它們所導致的變更，逐一說明一個句子。


