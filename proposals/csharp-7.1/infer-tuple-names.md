---
ms.openlocfilehash: 25e95b3ab8c384a7e66e59a7f9422cc9699074d7
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484717"
---
# <a name="infer-tuple-names-aka-tuple-projection-initializers"></a>推斷元組名稱（也稱為）。 元組投影初始化運算式）

## <a name="summary"></a>摘要
[summary]: #summary

在許多常見的情況下，這項功能可省略和推斷元組專案名稱。 例如，您可以從 `(x.f1, x?.f2)`推斷元素名稱 "f1" 和 "f2"，而不是輸入 `(f1: x.f1, f2: x?.f2)`。

這會使匿名型別的行為更加相似，這會允許在建立期間推斷成員名稱。 例如，`new { x.f1, y?.f2 }` 宣告成員 "f1" 和 "f2"。

在 LINQ 中使用元組時，這特別有用：

```csharp
// "c" and "result" have element names "f1" and "f2"
var result = list.Select(c => (c.f1, c.f2)).Where(t => t.f2 == 1); 
```

## <a name="detailed-design"></a>詳細設計
[design]: #detailed-design

變更有兩個部分：

1.  嘗試為沒有明確名稱的每個元組元素推斷候選名稱：
    -   使用與匿名型別的名稱推斷相同的規則。
        - 在C#中，這可允許三種情況： `y` （識別碼）、`x.y` （簡單成員存取）和 `x?.y` （條件式存取）。
        - 在 VB 中，這可允許其他情況，例如 `x.y()`。
    -   拒絕保留的元組名稱（在中C#區分大小寫，在 VB 中則不區分大小寫），因為它們被禁止或已隱含。 例如 `ItemN`、`Rest`和 `ToString`。
    -   如果任何候選名稱重複（區分大小寫C#，在 VB 中則不區分大小寫），則會捨棄這些候選項目，
2.  在轉換期間（檢查和警告從元組常值中卸載名稱），推斷的名稱不會產生任何警告。 這可避免中斷現有的元組程式碼。

請注意，處理重複專案的規則與匿名型別的規則不同。 比方說，`new { x.f1, x.f1 }` 會產生錯誤，但仍然允許 `(x.f1, x.f1)` （不含任何推斷的名稱）。 這可避免中斷現有的元組程式碼。

為求一致，相同的適用于解構指派所產生的元組（ C#在中）：

```csharp
// tuple has element names "f1" and "f2" 
var tuple = ((x.f1, x?.f2) = (1, 2));
```

同樣也適用于 VB 元組，使用 VB 特有的規則從運算式推斷名稱，並不區分大小寫的名稱比較。

使用C# 7.1 編譯器（或更新版本）搭配語言版本 "7.0" 時，將會推斷專案名稱（儘管功能無法使用），但會有嘗試存取的使用網站錯誤。 這會限制新增新程式碼，稍後會面臨相容性問題（如下所述）。

## <a name="drawbacks"></a>缺點
[drawbacks]: #drawbacks

主要缺點是這會引進C# 7.0 的相容性中斷：

```csharp
Action y = () => M();
var t = (x: x, y);
t.y(); // this might have previously picked up an extension method called “y”, but would now call the lambda.
```

相容性委員會發現這項中斷是可接受的，因為它會受到限制，而時間範圍自C#元組（7.0）則短。

## <a name="references"></a>參考
- [LDM 4 月4日2017](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md#tuple-names)
- [Github 討論](https://github.com/dotnet/csharplang/issues/370)（感謝 @alrz 讓此問題上線）
- [元組設計](https://github.com/dotnet/roslyn/blob/master/docs/features/tuples.md)
