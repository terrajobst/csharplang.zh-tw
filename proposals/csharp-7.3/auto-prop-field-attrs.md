---
ms.openlocfilehash: 2e054c629f71ae37b112300905c3106f9ce977e8
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484661"
---
# <a name="auto-implemented-property-field-targeted-attributes"></a><span data-ttu-id="fc470-101">自動實作為屬性欄位目標屬性</span><span class="sxs-lookup"><span data-stu-id="fc470-101">Auto-Implemented Property Field-Targeted Attributes</span></span>

## <a name="summary"></a><span data-ttu-id="fc470-102">摘要</span><span class="sxs-lookup"><span data-stu-id="fc470-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="fc470-103">這項功能打算讓開發人員直接將屬性套用至自動實作為屬性的支援欄位。</span><span class="sxs-lookup"><span data-stu-id="fc470-103">This feature intends to allow developers to apply attributes directly to the backing fields of auto-implemented properties.</span></span>

## <a name="motivation"></a><span data-ttu-id="fc470-104">動機</span><span class="sxs-lookup"><span data-stu-id="fc470-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="fc470-105">目前無法將屬性套用至自動實作為屬性的支援欄位。</span><span class="sxs-lookup"><span data-stu-id="fc470-105">Currently it is not possible to apply attributes to the backing fields of auto-implemented properties.</span></span>  <span data-ttu-id="fc470-106">在開發人員必須使用欄位目標屬性的情況下，他們會強制手動宣告欄位，並使用更詳細的屬性語法。</span><span class="sxs-lookup"><span data-stu-id="fc470-106">In those cases where the developer must use a field-targeting attribute they are forced to declare the field manually and use the more verbose property syntax.</span></span>  <span data-ttu-id="fc470-107">假設在C#事件產生的支援欄位上，一律支援以欄位為目標的屬性，將相同的功能擴充至其屬性家族相互交換是合理的。</span><span class="sxs-lookup"><span data-stu-id="fc470-107">Given that C# has always supported field-targeted attributes on the generated backing field for events it makes sense to extend the same functionality to their property kin.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="fc470-108">詳細設計</span><span class="sxs-lookup"><span data-stu-id="fc470-108">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="fc470-109">簡單地說，下列是合法C#的，而且不會產生警告：</span><span class="sxs-lookup"><span data-stu-id="fc470-109">In short, the following would be legal C# and not produce a warning:</span></span>

```csharp
[Serializable]
public class Foo 
{
    [field: NonSerialized]
    public string MySecret { get; set; }
}
```

<span data-ttu-id="fc470-110">這會導致以欄位為目標的屬性套用至編譯器產生的支援欄位：</span><span class="sxs-lookup"><span data-stu-id="fc470-110">This would result in the field-targeted attributes being applied to the compiler-generated backing field:</span></span>

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

<span data-ttu-id="fc470-111">如先前所述，這會使與C# 1.0 的事件語法同位檢查，因為下列專案已經是合法的，而且會如預期般運作：</span><span class="sxs-lookup"><span data-stu-id="fc470-111">As mentioned, this brings parity with event syntax from C# 1.0 as the following is already legal and behaves as expected:</span></span>

```csharp
[Serializable]
public class Foo
{
    [field: NonSerialized]
    public event EventHandler MyEvent;
}
```

## <a name="drawbacks"></a><span data-ttu-id="fc470-112">缺點</span><span class="sxs-lookup"><span data-stu-id="fc470-112">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="fc470-113">執行此變更有兩個可能的缺點：</span><span class="sxs-lookup"><span data-stu-id="fc470-113">There are two potential drawbacks to implementing this change:</span></span>

1. <span data-ttu-id="fc470-114">嘗試將屬性套用至自動實作為屬性的欄位時，會產生編譯器警告，指出該區塊中的屬性將會被忽略。</span><span class="sxs-lookup"><span data-stu-id="fc470-114">Attempting to apply an attribute to the field of an auto-implemented property produces a compiler warning that the attributes in that block will be ignored.</span></span>  <span data-ttu-id="fc470-115">如果編譯器已變更為支援這些屬性，則會在後續重新編譯時將它們套用至支援欄位，這可能會在執行時間變更程式的行為。</span><span class="sxs-lookup"><span data-stu-id="fc470-115">If the compiler were changed to support those attributes they would be applied to the backing field on a subsequent recompilation which could alter the behavior of the program at runtime.</span></span>
1. <span data-ttu-id="fc470-116">編譯器目前不會在嘗試將屬性套用至自動實作為屬性的欄位時，驗證其 AttributeUsage 目標。</span><span class="sxs-lookup"><span data-stu-id="fc470-116">The compiler does not currently validate the AttributeUsage targets of the attributes when attempting to apply them to the field of the auto-implemented property.</span></span>  <span data-ttu-id="fc470-117">如果編譯器已變更為支援以欄位為目標的屬性，而且有問題的屬性無法套用至欄位，則編譯器會發出錯誤而非警告，因而中斷組建。</span><span class="sxs-lookup"><span data-stu-id="fc470-117">If the compiler were changed to support field-targeted attributes and the attribute in question cannot be applied to a field the compiler would emit an error instead of a warning, breaking the build.</span></span>

## <a name="alternatives"></a><span data-ttu-id="fc470-118">替代方案</span><span class="sxs-lookup"><span data-stu-id="fc470-118">Alternatives</span></span>
[alternatives]: #alternatives

## <a name="unresolved-questions"></a><span data-ttu-id="fc470-119">未解決的問題</span><span class="sxs-lookup"><span data-stu-id="fc470-119">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a><span data-ttu-id="fc470-120">設計會議</span><span class="sxs-lookup"><span data-stu-id="fc470-120">Design meetings</span></span>
