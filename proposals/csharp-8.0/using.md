---
ms.openlocfilehash: 91afbc3e3412049cd183c36c8035f1862c520413
ms.sourcegitcommit: da1180f7eacdd5067b32d291a76e6764159e00fe
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2019
ms.locfileid: "79485053"
---
# <a name="pattern-based-using-and-using-declarations"></a>「以模式為基礎的 using」和「using 宣告」

## <a name="summary"></a>摘要

此語言會在 `using` 語句周圍加入兩項新功能，以簡化資源管理：除了 `IDisposable`，`using` 應該辨識可處置模式，並將 `using` 宣告新增至語言。

## <a name="motivation"></a>動機

`using` 語句是現今資源管理的有效工具，但它需要相當多的儀式。 具有多個要管理之資源的方法，可以使用一系列的 `using` 語句，在語法上很快。 這種語法的負擔足以讓大部分的程式碼樣式指導方針在此案例中明確地有括弧括住的例外狀況。 

`using` 聲明在此移除大部分的儀式，並C#與其他包含資源管理區塊的語言同等。 此外，以模式為基礎的 `using` 可讓開發人員擴充可參與此處的類型集合。 在許多情況下，不需要建立僅存在於 `using` 語句中使用值的包裝函式類型。 

這些功能一起可讓開發人員簡化和擴充可套用 `using` 的案例。

## <a name="detailed-design"></a>詳細設計 

### <a name="using-declaration"></a>using 宣告

此語言可將 `using` 新增至本機變數宣告中。 這類宣告會與在相同位置的 `using` 語句中宣告變數的效果相同。

```csharp
if (...) 
{ 
   using FileStream f = new FileStream(@"C:\users\jaredpar\using.md");
   // statements
}

// Equivalent to 
if (...) 
{ 
   using (FileStream f = new FileStream(@"C:\users\jaredpar\using.md")) 
   {
    // statements
   }
}
```

`using` 本機的存留期會延伸到其宣告範圍的結尾。 然後，`using` 的區域變數會依照其宣告的反向順序來處置。 

```csharp
{ 
    using var f1 = new FileStream("...");
    using var f2 = new FileStream("..."), f3 = new FileStream("...");
    ...
    // Dispose f3
    // Dispose f2 
    // Dispose f1
}
```

在 `using` 宣告的臉部中，`goto`或任何其他控制流程結構都沒有任何限制。 相反地，程式碼的作用就如同對等的 `using` 語句一樣：

```csharp
{
    using var f1 = new FileStream("...");
  target:
    using var f2 = new FileStream("...");
    if (someCondition) 
    {
        // Causes f2 to be disposed but has no effect on f1
        goto target;
    }
}
```

在 `using` 區域宣告中宣告的區域將隱含為唯讀。 這符合在 `using` 語句中宣告的區域變數行為。 

`using` 宣告的語言文法將如下所示：

```antlr
local-using-declaration:
  using type using-declarators

using-declarators:
  using-declarator
  using-declarators , using-declarator
  
using-declarator:
  identifier = expression
```

`using` 聲明方面的限制：

- 可能不會直接出現在 `case` 標籤內，而是必須在 `case` 標籤內的區塊內。
- 可能不會顯示為 `out` 變數宣告的一部分。 
- 每個宣告子都必須有初始化運算式。
- 區欄位型別必須可以隱含地轉換成 `IDisposable` 或滿足 `using` 模式。

### <a name="pattern-based-using"></a>以模式為基礎的 using

此語言將會加入可處置模式的概念：這是具有可存取的 `Dispose` 實例方法的類型。 符合可處置模式的類型可以參與 `using` 語句或宣告，而不需要執行 `IDisposable`。 

```csharp
class Resource
{ 
    public void Dispose() { ... }
}

using (var r = new Resource())
{
    // statements
}
```

這可讓開發人員在一些新案例中運用 `using`：

- `ref struct`：這些類型無法立即執行介面，因此無法參與 `using` 語句。
- 擴充方法可讓開發人員增強其他元件中的類型，以參與 `using` 語句。

在類型可以隱含地轉換成 `IDisposable` 而且也符合可處置模式的情況下，就會優先使用 `IDisposable`。 雖然這會採用相反的 `foreach` 方式（在介面上慣用的模式），但還是必須提供回溯相容性。

也適用于傳統 `using` 語句的相同限制：在 `using` 中宣告的區域變數是唯讀的，`null` 值不會造成擲回例外狀況，依此類推 .。。程式碼產生將會不同，只有在呼叫 Dispose 之前，不會將轉換成 `IDisposable`：

```csharp
{
      Resource r = new Resource();
      try {
            // statements
      }
      finally {
            if (resource != null) resource.Dispose();
      }
}
```

為了符合可處置的模式，`Dispose` 的方法必須是可存取的、無參數的，而且具有 `void` 的傳回型別。 沒有其他限制。 這明確表示可以在這裡使用擴充方法。

## <a name="considerations"></a>考量

### <a name="case-labels-without-blocks"></a>沒有區塊的案例標籤

`using declaration` 在 `case` 標籤中直接不合法，因為其實際存留期的複雜性。 其中一個可能的解決方法，就是在相同的位置中，為它提供與 `out var` 相同的存留期。 它已被視為功能執行的額外複雜性，而且輕鬆地解決此問題（只要將區塊新增至 `case` 標籤）就不會採用此路由。

## <a name="future-expansions"></a>未來的擴充

### <a name="fixed-locals"></a>已修正區域變數

`fixed` 語句具有 `using` 語句的所有屬性，可讓您擁有 `using` 區域變數的能力。 請考慮將此功能延伸到 `fixed` 區域變數。 存留期和順序規則應該同樣適用于 `using` 和 `fixed`。
