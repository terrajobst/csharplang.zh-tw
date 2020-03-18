---
ms.openlocfilehash: 9647fff40a1e45bef917f140612ae4e91abea958
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484570"
---
# <a name="lambda-attributes"></a>Lambda 屬性

* [x] 提議
* [] 原型
* [] 執行
* [] 規格

## <a name="summary"></a>摘要
[summary]: #summary

允許將屬性套用至 lambda （和匿名方法）和 lambda/匿名方法參數，因為它們可以在一般方法上。

## <a name="motivation"></a>動機
[motivation]: #motivation

兩個主要動機：

1. 提供分析器在編譯時期看到的中繼資料。
2. 提供反映和工具在執行時間可見的中繼資料。

做為（1）的範例：對於效能敏感的程式碼，能夠讓分析器在為關閉狀態的 lambda 配置結束和委派時旗標。  通常這類程式碼的開發人員會離開其以避免捕捉任何狀態，因此編譯器可以產生靜態方法和方法的可快取委派，而開發人員會確保唯一關閉的狀態是 `this`，讓編譯器至少可以避免配置結束物件。  但是，如果沒有語言支援來限制可能會捕捉到的內容，則很容易不小心關閉狀態。  如果開發人員可以使用屬性來標注 lambda，以指出可以關閉的狀態，就很有價值，例如：

```csharp
[CaptureNone] // can't close over any instance state
[CaptureThis] // can only capture `this` and no other instance state
[CaptureAny] // can close over any instance state
```

然後，可以在不正確地捕捉狀態時，將分析器寫入旗標，例如：

```csharp
var results = collection.Select([CaptureNone](i) => Process(item)); // Analyzer error: [CaptureNone] lambdas captures `this`
...
private U Process(T item) { ... }
```

## <a name="detailed-design"></a>詳細設計
[design]: #detailed-design

- 使用與一般方法相同的屬性語法時，可以在 lambda 或匿名方法的開頭套用屬性，例如：

```csharp
[SomeAttribute(...)] () => { ... }
[SomeAttribute(...)] delegate (int i) { ... }
```

- 為了避免屬性套用至 lambda 方法或其中一個引數時，只有在任何引數周圍使用括弧時，才會使用屬性，例如：

```csharp
[SomeAttribute] i => { ... } // ERROR
[SomeAttribute] (i) => { ... } // Ok
[SomeAttribute] (int i) => { ... } // Ok
```

- 使用匿名方法時，不需要括弧，就能在 `delegate` 關鍵字之前將屬性套用至方法，例如：

```csharp
[SomeAttribute] delegate { ... } // Ok
[SomeAttribute] delegate (int i) => { ... } // Ok
```

- 您可以透過標準的逗點分隔語法或完整屬性語法來套用多個屬性，例如：

```csharp
[FirstAttribute, SecondAttribute] (i) => { ... } // Ok
[FirstAttribute] [SecondAttribute] (i) => { .... } // Ok
```

- 屬性可以套用至匿名方法或 lambda 的參數，但僅限於在任何引數周圍使用括弧時，例如：

```csharp
[SomeAttribute] i => { ... } // ERROR
([SomeAttribute] i) => { .... } // Ok
([SomeAttribute] int i) => { ... } // Ok
([SomeAttribute] i, [SomeOtherAttribute] j) => { ... } // Ok
```

- 您可以使用逗號分隔或完整屬性語法，將多個屬性套用至匿名方法或 lambda 的參數，例如：

```csharp
([FirstAttribute, SecondAttribute] i) => { ... } // Ok
([FirstAttribute] [SecondAttribute] i) => { ... } // Ok
```

- 以 `return`為目標的屬性也可以在 lambda 上使用，例如：

```csharp
([return: SomeAttribute] (i) => { ... }) // Ok
```

- 編譯器會將屬性輸出到產生的方法和這些方法的引數，就如同任何其他方法一樣。

## <a name="drawbacks"></a>缺點
[drawbacks]: #drawbacks

n/a

## <a name="alternatives"></a>替代方案
[alternatives]: #alternatives

n/a

## <a name="unresolved-questions"></a>未解決的問題
[unresolved]: #unresolved-questions

n/a

## <a name="design-meetings"></a>設計會議

n/a