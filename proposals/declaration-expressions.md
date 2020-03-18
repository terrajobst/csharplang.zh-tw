---
ms.openlocfilehash: d2064ec1637e50962262c9380281abd5e1711402
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484577"
---
# <a name="declaration-expressions"></a>宣告運算式

支援將宣告指派為運算式。

## <a name="motivation"></a>動機
[motivation]: #motivation

允許在更多情況下于宣告時進行初始化、簡化程式碼，以及允許使用 `var`。

```csharp
SpecialType ReferenceType =>
    (var st = _type.SpecialType).IsValueType() ? SpecialType.None : st;
```

允許 `ref` 引數的宣告，類似于 `out var`。

```csharp
Convert(source, destination, ref List<Diagnostic> diagnostics = null);
```

## <a name="detailed-design"></a>詳細設計
[design]: #detailed-design

運算式已擴充為包含宣告指派。 優先順序與指派相同。

```antlr
expression
    : non_assignment_expression
    | assignment
    | declaration_assignment_expression // new
    ;
declaration_assignment_expression // new
    : declaration_expression '=' local_variable_initializer
    ;
declaration_expression // C# 7.0
    | type variable_designation
    ;
```

宣告指派是單一的本機。

宣告指派運算式的類型是宣告的類型。
如果類型是 `var`，則推斷的類型是初始化運算式的類型。 

宣告指派運算式可以是左值，特別是 `ref` 的引數值。

如果宣告指派運算式宣告實值型別，而且運算式是右值，則運算式的值會是複本。

宣告指派運算式可以宣告 `ref` 本機。
當 `ref` 用於 `ref` 引數中的宣告運算式時，會有一個不明確的情況。
本機變數初始化運算式會判斷宣告是否為 `ref` 本機。

```csharp
F(ref int x = IntFunc());    // int x;
F(ref int y = RefIntFunc()); // ref int y;
```

宣告指派運算式中宣告的區域變數範圍，與 C # 7.0 中對應宣告運算式的範圍相同。

在宣告運算式前面的文字中參考本機，是編譯時間錯誤。

## <a name="alternatives"></a>替代方案
[alternatives]: #alternatives
無變更。 這項功能只是語法上的縮寫。

更多一般序列運算式：請參閱[#377](https://github.com/dotnet/csharplang/issues/377)。

若要允許在更多情況下使用 `var`，請允許 `var` 區域變數的個別宣告和指派，並從所有程式碼路徑的指派推斷類型。

## <a name="see-also"></a>另請參閱
[see-also]: #see-also
請參閱[#595](https://github.com/dotnet/csharplang/issues/595)中的基本宣告運算式。

請參閱[解構](https://github.com/dotnet/roslyn/blob/master/docs/features/deconstruction.md)功能中的解構宣告。
