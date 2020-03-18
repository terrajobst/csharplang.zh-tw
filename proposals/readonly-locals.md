---
ms.openlocfilehash: 922353d043653ddb651534a01f3fb98f85cd756e
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484479"
---
# <a name="readonly-locals-and-parameters"></a>readonly 區域變數和參數

* [x] 提議
* [] 原型
* [] 執行
* [] 規格

## <a name="summary"></a>摘要
[summary]: #summary

允許將區域變數和參數標注為 readonly，以防止這些區域變數和參數的淺層變化。

## <a name="motivation"></a>動機
[motivation]: #motivation

現在，`readonly` 關鍵字可以套用至欄位;這項功能的作用是確保欄位只能在結構中寫入（在靜態欄位的情況下為靜態結構，或在實例欄位的情況下為實例結構），這可協助開發人員不小心覆寫不應修改的狀態，以避免錯誤。 但欄位並不是開發人員想要確保值不會變化的唯一位置。 特別是，通常會建立本機變數來儲存暫時性狀態，而且不小心更新暫時狀態可能會導致錯誤的計算和其他這類錯誤，尤其是在 lambda 中捕捉這類「區域變數」時，就會將這些提升欄位標記為 `readonly`。

## <a name="detailed-design"></a>詳細設計
[design]: #detailed-design

區域變數也會可注釋為 `readonly`，讓編譯器確保它們只在宣告時設定（中C#的特定區域變數已隱含唯讀，例如 ' foreach ' 迴圈中的反覆運算變數或 ' using ' 區塊中使用的變數，但目前開發人員無法將其他區域變數標記為 `readonly`）。 這類 `readonly` 區域變數必須有初始化運算式：

```csharp
readonly long maxBytesToDelete = (stream.LimitBytes - stream.MaxBytes) / 10;
...
maxBytesToDelete = 0; // Error: can't assign to readonly locals outside of declaration
```

而做為 `readonly var`的縮寫，可以使用現有的內容關鍵字 `let`，例如

```csharp
let maxBytesToDelete = (stream.LimitBytes - stream.MaxBytes) / 10;
...
maxBytesToDelete = 0; // Error: can't assign to readonly locals outside of declaration
```

初始化運算式可能沒有任何特殊的條件約束，而且可以是任何目前有效的專案做為區域變數的初始化運算式，例如

```csharp
readonly T data = arg1 ?? arg2;
```

使用 lambda 和閉時，在區域變數上 `readonly` 特別有用。 當匿名方法或 lambda 從封入範圍存取本機狀態時，編譯器會將該狀態捕捉到結束項，這是由「顯示類別」所表示。  所捕捉到的每個「區域」都是此類別中的欄位，但因為編譯器會代表您產生此欄位，所以您不會有機會將它標注為 `readonly`，以防止 lambda 錯誤地寫入「區域」（在引號中，因為它其實不是本機，至少不在產生的 MSIL 中）。 使用 `readonly` 的區域變數，編譯器可以防止 lambda 寫入至本機，這在涉及錯誤寫入的多執行緒案例中特別有用，因為這可能會導致危險但很罕見且難以發現的並行處理錯誤。

```csharp
readonly long index = ...;
Parallel.ForEach(data, item => {
    T element = item[index];
    index = 0; // Error: can't assign to readonly locals outside of declaration
});
```

以特殊形式的本機而言，參數也會可注釋為 `readonly`。 這不會影響方法的呼叫端傳遞至參數的方式（就像沒有限制哪些值可以儲存在 `readonly` 欄位中一樣），但就像任何 `readonly` 本機一樣，編譯器會禁止程式碼在宣告之後寫入參數，這表示方法的主體禁止寫入參數。

```csharp
public void Update(readonly int index = 0) // Default values are ok though not required
{
    ...
    index = 0; // Error: can't assign to readonly parameters
    ...
}
```

`readonly` 參數不會影響編譯器針對該方法發出的簽章/中繼資料，而只會影響編譯器如何處理方法主體的編譯。 例如，基底虛擬方法可能會有 `readonly` 參數，而且該參數在覆寫中可以是可寫入的。

如同欄位，區域變數和參數的 `readonly` 是淺層，會影響儲存位置，但不會對物件圖形進行間接影響。 不過，也就像欄位一樣，在 `readonly` 的本機/參數結構上呼叫方法，實際上會建立結構的複本，並在複本上呼叫方法，以避免 `this`的內部變化。

除非也支援 `ref readonly`，否則無法將 `readonly` 區域變數和參數當做 `ref` 或 `out` 引數傳遞。

## <a name="alternatives"></a>替代方案
[alternatives]: #alternatives

- `val` 可用來做為 `let`的替代縮寫。

## <a name="unresolved-questions"></a>未解決的問題
[unresolved]: #unresolved-questions

- `readonly ref` / `ref readonly` / `readonly ref readonly`：我已將有關如何處理 `ref readonly` 的問題留給這份提案。
- 此提案不會處理 readonly 結構/不可變的類型。 這會保留給個別的提案。

## <a name="design-meetings"></a>設計會議

- 在2015年1月21日簡短討論（<https://github.com/dotnet/roslyn/issues/98>）
