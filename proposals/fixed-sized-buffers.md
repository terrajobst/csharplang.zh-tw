---
ms.openlocfilehash: b51f27b2f58fd19851c80beb9cedcbd32b80b165
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484584"
---
# <a name="fixed-sized-buffers"></a>固定大小的緩衝區

* [x] 提議
* [] 原型：未啟動
* [] 執行：未啟動
* [] 規格：未啟動

## <a name="summary"></a>摘要
[summary]: #summary

提供一般用途和安全的機制，將固定大小的緩衝區宣告為C#語言。

## <a name="motivation"></a>動機
[motivation]: #motivation

目前，使用者能夠在不安全的內容中建立固定大小的緩衝區。 不過，這需要使用者處理指標、手動執行界限檢查，而且只支援一組有限的類型（`bool`、`byte`、`char`、`short`、`int`、`long`、`sbyte`、`ushort`、`uint`、`ulong`、`float`和 `double`）。

最常見的抱怨是，固定大小的緩衝區無法在安全的程式碼中編制索引。 第二個無法使用更多類型。

有幾個次要調整，我們可以提供一般用途的固定大小緩衝區，以支援任何類型、可用於安全內容中，以及執行自動界限檢查。

## <a name="detailed-design"></a>詳細設計
[design]: #detailed-design

其中一個會透過下列方式宣告安全的固定大小緩衝區：

```csharp
public fixed DXGI_RGB GammaCurve[1025];
```

宣告會轉譯成編譯器的內部標記法，如下所示

```csharp
[FixedBuffer(typeof(DXGI_RGB), 1024)]
public ConsoleApp1.<Buffer>e__FixedBuffer_1024<DXGI_RGB> GammaCurve;

// Pack = 0 is the default packing and should result in indexable layout.
[CompilerGenerated, UnsafeValueType, StructLayout(LayoutKind.Sequential, Pack = 0)]
struct <Buffer>e__FixedBuffer_1024<T>
{
    private T _e0;
    private T _e1;
    // _e2 ... _e1023
    private T _e1024;

    public ref T this[int index] => ref (uint)index <= 1024u ?
                                         ref RefAdd<T>(ref _e0, index):
                                         throw new IndexOutOfRange();
}
```

因為這類固定大小的緩衝區不再需要使用 `fixed`，所以允許任何元素類型是合理的。  

> 注意：仍會支援 `fixed`，但只有在元素類型為 `blittable`

## <a name="drawbacks"></a>缺點
[drawbacks]: #drawbacks

* 回溯相容性可能會有一些挑戰，但假設現有的固定大小緩衝區僅適用于基本類型的選取專案，則編譯器可能會在使用者將固定緩衝區視為時，繼續「單純」滑鼠.
* 不相容的結構可能需要使用稍微不同的 `v2` 編碼來隱藏舊編譯器中的欄位。
* 在泛型型別的 IL 規格中，未妥善定義封裝。 雖然此方法應該可行，但我們仍會相鄰未記載的行為。 我們應該將其記載，並確定其他 JITs （例如 Mono）具有相同的行為。
* 為每個長度指定不同的類型（可能是 `readonly` 欄位的另一個）會對中繼資料造成影響。 它會受限於給定應用程式中不同大小的陣列數目。
* `ref` math 無法正式驗證（因為它不安全）。 我們需要尋找更新驗證規則的方法，以瞭解我們的使用方式良好。

## <a name="alternatives"></a>替代方案
[alternatives]: #alternatives

手動宣告您的結構，並使用不安全的程式碼來建立索引子。

## <a name="unresolved-questions"></a>未解決的問題
[unresolved]: #unresolved-questions

- 我們是否應該允許 `readonly`？  （使用 readonly 索引子）
- 我們應該允許陣列初始化運算式嗎？
- 是否需要 `fixed` 關鍵字？
- `foreach`?
- 只有結構中的實例欄位？

## <a name="design-meetings"></a>設計會議

連結至會影響此提案的設計附注，並針對它們所導致的變更，逐一說明一個句子。