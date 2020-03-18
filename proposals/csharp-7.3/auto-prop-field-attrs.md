---
ms.openlocfilehash: 2e054c629f71ae37b112300905c3106f9ce977e8
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484661"
---
# <a name="auto-implemented-property-field-targeted-attributes"></a>自動實作為屬性欄位目標屬性

## <a name="summary"></a>摘要
[summary]: #summary

這項功能打算讓開發人員直接將屬性套用至自動實作為屬性的支援欄位。

## <a name="motivation"></a>動機
[motivation]: #motivation

目前無法將屬性套用至自動實作為屬性的支援欄位。  在開發人員必須使用欄位目標屬性的情況下，他們會強制手動宣告欄位，並使用更詳細的屬性語法。  假設在C#事件產生的支援欄位上，一律支援以欄位為目標的屬性，將相同的功能擴充至其屬性家族相互交換是合理的。

## <a name="detailed-design"></a>詳細設計
[design]: #detailed-design

簡單地說，下列是合法C#的，而且不會產生警告：

```csharp
[Serializable]
public class Foo 
{
    [field: NonSerialized]
    public string MySecret { get; set; }
}
```

這會導致以欄位為目標的屬性套用至編譯器產生的支援欄位：

```csharp
[Serializable]
public class Foo 
{
    [NonSerialized]
    private string _mySecretBackingField;
    
    public string MySecret
    {
        get { return _mySecretBackingField; }
        set { _mySecretBackingField = value; }
    }
}
```

如先前所述，這會使與C# 1.0 的事件語法同位檢查，因為下列專案已經是合法的，而且會如預期般運作：

```csharp
[Serializable]
public class Foo
{
    [field: NonSerialized]
    public event EventHandler MyEvent;
}
```

## <a name="drawbacks"></a>缺點
[drawbacks]: #drawbacks

執行此變更有兩個可能的缺點：

1. 嘗試將屬性套用至自動實作為屬性的欄位時，會產生編譯器警告，指出該區塊中的屬性將會被忽略。  如果編譯器已變更為支援這些屬性，則會在後續重新編譯時將它們套用至支援欄位，這可能會在執行時間變更程式的行為。
1. 編譯器目前不會在嘗試將屬性套用至自動實作為屬性的欄位時，驗證其 AttributeUsage 目標。  如果編譯器已變更為支援以欄位為目標的屬性，而且有問題的屬性無法套用至欄位，則編譯器會發出錯誤而非警告，因而中斷組建。

## <a name="alternatives"></a>替代方案
[alternatives]: #alternatives

## <a name="unresolved-questions"></a>未解決的問題
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a>設計會議
