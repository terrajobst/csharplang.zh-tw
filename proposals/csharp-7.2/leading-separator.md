---
ms.openlocfilehash: 6ec94aaabb2c52393a87ee450dbae972b6a50bd5
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484710"
---
# <a name="allow-digit-separator-after-0b-or-0x"></a>允許在0b 或0x 之後的數位分隔符號

在C# 7.2 中，我們會擴充數位分隔符號（底線字元）可出現在整數常值中的一組位置。 [從C# 7.0 開始，常值的數位之間允許分隔符號](../csharp-7.0/digit-separators.md)。 現在，在C# 7.2 中，我們也允許在二元或十六進位常值的第一個有效位數之前，在前置詞之後使用數位分隔符號。

```csharp
    123      // permitted in C# 1.0 and later
    1_2_3    // permitted in C# 7.0 and later
    0x1_2_3  // permitted in C# 7.0 and later
    0b101    // binary literals added in C# 7.0
    0b1_0_1  // permitted in C# 7.0 and later

    // in C# 7.2, _ is permitted after the `0x` or `0b`
    0x_1_2   // permitted in C# 7.2 and later
    0b_1_0_1 // permitted in C# 7.2 and later
```

我們不允許十進位整數常值具有前置底線。 權杖（例如 `_123`）是識別碼。
