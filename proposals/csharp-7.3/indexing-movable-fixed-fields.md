---
ms.openlocfilehash: 36f3e6204d12c2569b3a55f3a47f58337e8a08e4
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484626"
---
# <a name="indexing-fixed-fields-should-not-require-pinning-regardless-of-the-movableunmovable-context"></a>不論可移動/不可移動的內容為何，索引 `fixed` 欄位都不應該要求釘選。 #

變更具有 bug 修正的大小。 它可以是7.3，而且不會與我們進一步採取的任何方向衝突。
這項變更只是為了讓下列案例即使 `s` 可移動，仍可正常執行。 當 `s` 不是可移動的時，它已經是有效的。 

注意：不論是哪一種情況，它仍然需要 `unsafe` 內容。 可以讀取未初始化的資料，甚至是超出範圍。 這不會變更。

```csharp
unsafe struct S
{
    public fixed int myFixedField[10];
}

class Program
{
    static S s;

    unsafe static void Main()
    {
        int p = s.myFixedField[5]; // indexing fixed-size array fields would be ok
    }
}
```

我在這裡看到的主要「挑戰」是如何說明規格中的放寬。特別是，由於下列情況仍然需要釘選。 （因為 `s` 是可移動的，而我們明確使用欄位做為指標）

```csharp
unsafe struct S
{
    public fixed int myFixedField[10];
}

class Program
{
    static S s;

    unsafe static void Main()
    {
        int* ptr = s.myFixedField; // taking a pointer explicitly still requires pinning.
        int p = ptr[5];
    }
}
```

我們需要在移動時將目標釘選的其中一個原因是我們的程式碼產生策略的成品，我們一律會轉換成非受控指標，因此會強制使用者透過 `fixed` 語句來釘選。 不過，在進行索引編制時，不需要轉換成非受控。 當我們的接收者是 managed 指標的形式時，相同的 unsafe 指標數學也同樣適用。 如果這樣做，就會管理中繼參考（GC 追蹤），而不需要釘選。

變更 https://github.com/dotnet/roslyn/pull/24966 是放寬此需求的原型 PR。
