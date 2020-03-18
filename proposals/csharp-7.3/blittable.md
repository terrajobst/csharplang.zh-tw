---
ms.openlocfilehash: 11e9d21bda9e69be692c5c5f5aee80c2da1894ab
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484619"
---
# <a name="unmanaged-type-constraint"></a>非受控類型條件約束

## <a name="summary"></a>摘要
[summary]: #summary

非受控條件約束功能會將語言強制授與C#語言規格中稱為「非受控類型」的類型類別。 這在第18.2 節中定義為類型，這不是參考型別，而且不包含任何嵌套層級的參考類型欄位。  

## <a name="motivation"></a>動機
[motivation]: #motivation

主要動機是讓您更輕鬆地在中C#撰寫低層級的 interop 程式碼。 非受控型別是 interop 程式碼的其中一個核心構成要素，但是在泛型中缺少支援，就無法在所有非受控型別上建立可重複使用的常式。 相反地，開發人員會被迫針對其媒體櫃中的每個非受控類型，撰寫相同的定案盤子程式碼：

```csharp
int Hash(Point point) { ... } 
int Hash(TimeSpan timeSpan) { ... } 
```

若要啟用這種類型的案例，語言將引進新的條件約束：非受控：

```csharp
void Hash<T>(T value) where T : unmanaged
{
    ...
}
```

只有符合C#語言規格中非受控類型定義的類型，才能符合此條件約束。另一個查看的方法是，類型滿足非受控條件約束，iff 也可以用它做為指標。 

```csharp
Hash(new Point()); // Okay 
Hash(42); // Okay
Hash("hello") // Error: Type string does not satisfy the unmanaged constraint
```

具有非受控條件約束的類型參數可以使用非受控類型的所有可用功能：指標、固定等等。 

```csharp
void Hash<T>(T value) where T : unmanaged
{
    // Okay
    fixed (T* p = &value) 
    { 
        ...
    }
}
```

這個條件約束也可以讓結構化資料和位元組資料流程之間具有有效率的轉換。 這是網路堆疊和序列化層中常見的作業：

```csharp
Span<byte> Convert<T>(ref T value) where T : unmanaged 
{
    ...
}
```

這類常式的優點很有利，因為它們在編譯時期是可證明安全的，而且可以免費配置。  目前的 Interop 作者不能這麼做（即使是在效能非常重要的層級也一樣）。  相反地，他們必須依賴配置具有昂貴執行時間檢查的常式，以確認值的管理是否正確。

## <a name="detailed-design"></a>詳細設計
[design]: #detailed-design

語言會引進名為 `unmanaged`的新條件約束。 為了滿足此條件約束，型別必須是結構，而且該型別的所有欄位都必須屬於下列其中一個類別：

- 具有類型 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`、`decimal`、`bool`、`IntPtr` 或 `UIntPtr`。
- 這是任何 `enum` 類型。
- 這是指標類型。
- 這是 satsifies `unmanaged` 條件約束的使用者定義結構。

編譯器產生的實例欄位（例如，支援自動執行的屬性）也必須符合這些條件約束。 

例如：

```csharp
// Unmanaged type
struct Point 
{ 
    int X;
    int Y {get; set;}
}

// Not an unmanaged type
struct Student 
{ 
    string FirstName;
    string LastName;
}
``` 

`unmanaged` 條件約束無法與 `struct`、`class` 或 `new()`結合。 這種限制衍生自 `unmanaged` 暗示的事實 `struct` 因此其他條件約束並不合理。

`unmanaged` 條件約束並不會由 CLR 強制執行，只有語言才可。 若要防止其他語言的錯誤使用，具有此條件約束的方法將會受到 mod 需求的保護。這會讓其他語言無法使用非受控類型的類型引數。

條件約束中 `unmanaged` 的 token 不是關鍵字，也不是內容關鍵字。 相反地 `var`，它會在該位置進行評估，並將會執行下列其中一個動作：

- 系結至名為 `unmanaged`的使用者定義或參考型別：這將會被視為任何其他命名類型條件約束的處理方式。 
- 系結至 no 型別：這會被視為 `unmanaged` 條件約束。

在此情況下，有一個名為 `unmanaged` 的型別，而且在目前的內容中沒有限定性，就無法使用 `unmanaged` 條件約束。 這等同于功能 `var` 的規則和相同名稱的使用者定義型別。 

## <a name="drawbacks"></a>缺點
[drawbacks]: #drawbacks

這項功能的主要缺點是，它會為少數開發人員提供服務：通常是低層級的程式庫作者或架構。  因此，對於少數的開發人員而言，這是非常寶貴的語言時間。 

不過，這些架構通常是大部分 .NET 應用程式的基礎。  因此，此層級的效能/正確性會在 .NET 生態系統上有 ripple 效應。  這使得功能值得考慮，即使是有限的物件也是如此。

## <a name="alternatives"></a>替代方案
[alternatives]: #alternatives

有幾個需要考慮的替代方案：

- 現狀：此功能不會在其本身的優勢中證明，而開發人員會繼續使用隱含的 opt 行為。

## <a name="questions"></a>問題
[quesions]: #questions

### <a name="metadata-representation"></a>中繼資料標記法

F#語言會編碼簽章檔案中的條件約束， C#這表示無法重複使用其標記法。 必須為此條件約束選擇新的屬性。 此外，具有此條件約束的方法必須受到 mod 需求的保護。

### <a name="blittable-vs-unmanaged"></a>可直接管理的與非受控
此F#語言具有非常[類似的功能](https://docs.microsoft.com/dotnet/articles/fsharp/language-reference/generics/constraints)，使用不受管理的關鍵字。 可直接連線的名稱來自 Midori 中的使用。  可能想要在此尋找優先順序，並改用非受控。 

**解決**方式語言決定使用非受控 

### <a name="verifier"></a>驗證器

是否需要更新驗證器/執行時間，以瞭解如何使用泛型型別參數的指標？  或者，它可以直接在沒有變更的情況下工作嗎？

**解決**方式不需要任何變更。 所有指標類型都只是無法驗證。 

## <a name="design-meetings"></a>設計會議

n/a
