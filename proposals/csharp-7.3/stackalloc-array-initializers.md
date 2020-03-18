---
ms.openlocfilehash: 7ea62713416ef82034963aef06f3cb11703342ed
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484605"
---
# <a name="stackalloc-array-initializers"></a>Stackalloc 陣列初始設定式

## <a name="summary"></a>摘要
[summary]: #summary

允許陣列初始化運算式語法與 `stackalloc`搭配使用。

## <a name="motivation"></a>動機
[motivation]: #motivation

一般陣列可以在建立時初始化其元素。 在 `stackalloc` 情況下，這似乎是合理的。

為什麼不允許使用這種語法的問題，`stackalloc` 會經常發生。  
例如，請參閱[#1112](https://github.com/dotnet/csharplang/issues/1112)

## <a name="detailed-design"></a>詳細設計

一般陣列可以透過下列語法建立：

```csharp
new int[3]
new int[3] { 1, 2, 3 }
new int[] { 1, 2, 3 }
new[] { 1, 2, 3 }
```

我們應該允許透過下列各建立堆疊配置的陣列：  

```csharp
stackalloc int[3]               // currently allowed
stackalloc int[3] { 1, 2, 3 }
stackalloc int[] { 1, 2, 3 }
stackalloc[] { 1, 2, 3 }
```

所有案例的語義大致上與陣列相同。  
例如：在最後一個案例中，會從初始化運算式推斷元素型別，而且必須是「非受控」型別。

注意：此功能不會相依于做為 `Span<T>`的目標。 它就像是 `T*` 案例中一樣，因此在 `Span<T>` 的情況下，述詞似乎不合理。  

## <a name="translation"></a>翻譯

單純的實作為在透過一系列的元素指派來建立之後，就可以直接初始化陣列。  

類似于使用陣列的情況，偵測所有或大部分專案都是全像類型的情況，並透過複製所有常數專案的預先建立狀態，來使用更有效率的技術，可能也會是理想的做法。 

## <a name="drawbacks"></a>缺點
[drawbacks]: #drawbacks

## <a name="alternatives"></a>替代方案
[alternatives]: #alternatives

這是方便的功能。 您可以不執行任何動作。

## <a name="unresolved-questions"></a>未解決的問題
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a>設計會議

還沒有。 
