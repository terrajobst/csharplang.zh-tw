---
ms.openlocfilehash: ac2b233eb703b5eea3bd2dfdbeeadd7494b0c695
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484668"
---
# <a name="non-trailing-named-arguments"></a>非後置具名引數

## <a name="summary"></a>摘要
[summary]: #summary
允許在非尾端位置使用具名引數，只要它們是用於正確的位置即可。 例如： `DoSomething(isEmployed:true, name, age);` 。

## <a name="motivation"></a>動機
[motivation]: #motivation

主要動機是避免輸入多餘的資訊。 通常會將作為常值的引數命名（例如 `null`，`true`），以便用來闡明程式碼，而不是以不按照順序的方式傳遞引數。
目前不允許（`CS1738`），除非下列所有引數也都命名為。

```csharp
DoSomething(isEmployed:true, name, age); // currently disallowed, even though all arguments are in position
// CS1738 "Named argument specifications must appear after all fixed arguments have been specified"
```

一些其他範例：
```csharp
public void DoSomething(bool isEmployed, string personName, int personAge) { ... }

DoSomething(isEmployed:true, name, age); // currently CS1738, but would become legal
DoSomething(true, personName:name, age); // currently CS1738, but would become legal
DoSomething(name, isEmployed:true, age); // remains illegal
DoSomething(name, age, isEmployed:true); // remains illegal
DoSomething(true, personAge:age, personName:name); // already legal
```

這也可以搭配 params 使用：
```csharp
public class Task
{
    public static Task When(TaskStatus all, TaskStatus any, params Task[] tasks);
}
Task.When(all: TaskStatus.RanToCompletion, any: TaskStatus.Faulted, task1, task2)
```

## <a name="detailed-design"></a>詳細設計
[design]: #detailed-design

在§7.5.1 版（引數清單）中，規格目前顯示：
> 具有*引數名稱*的*引數*稱為__具名引數__，而沒有*引數名稱*的*引數*則是__位置引數__。 在*引數清單*中的具名引數後面出現位置引數時，會發生錯誤。

建議您移除此錯誤，並更新規則以尋找引數的對應參數（§7.5.1.1）：

引數中的引數-實例的函式、方法、索引子和委派的清單：
- [現有規則]
- 未命名的引數會在超出位置的具名引數或具名引數引數之後，對應到沒有參數。

特別是，這會防止使用 `M(c: false, valueB);`叫用 `void M(bool a = true, bool b = true, bool c = true, );`。 第一個引數會使用不在位置（引數會在第一個位置使用，但名為 "c" 的參數會在第三個位置），因此下列引數應該命名為。

換句話說，只有在名稱和位置導致尋找相同的對應參數時，才允許非尾端的具名引數。

## <a name="drawbacks"></a>缺點
[drawbacks]: #drawbacks

此提議會 exacerbates 具有多載解析中具名引數的現有微妙差異。 例如：

```csharp
void M(int x, int y) { }
void M<T>(T y, int x) { }

void M2()
{
    M(3, 4);
    M(y: 3, x: 4); // Invokes M(int, int)
    M(y: 3, 4); // Invokes M<T>(T, int)
}
```

您現在可以藉由交換參數來取得這種情況：

```csharp
void M(int y, int x) { }
void M<T>(int x, T y) { }

void M2()
{
    M(3, 4);
    M(x: 3, y: 4); // Invokes M(int, int)
    M(3, y: 4); // Invokes M<T>(int, T)
}
```

同樣地，如果您有兩個方法 `void M(int a, int b)` 和 `void M(int x, string y)`，則不被叫用的調用 `M(x: 1, 2)` 將會根據第二個多載產生診斷（「無法從 ' int ' 轉換為 ' string '」）。 當具名引數用於尾端位置時，這個問題已經存在。

## <a name="alternatives"></a>替代方案
[alternatives]: #alternatives

有幾個需要考慮的替代方案：

- 現狀
- 提供 IDE 協助，讓您在中間輸入特定名稱時，填滿尾端引數的所有名稱。

這兩種情況都能從更詳細的資訊，因為它們會引進多個具名引數，即使在引數清單的開頭只需要一個常值的名稱。

## <a name="unresolved-questions"></a>未解決的問題
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a>設計會議
[ldm]: #ldm
此功能已在5月 2017 16 日的 LDM 中簡短討論，並以原則進行核准（「確定」移至「提案」/「原型」）。 它也會在2017年6月28日簡短討論。

與探討問題相關的初始討論 https://github.com/dotnet/csharplang/issues/518 https://github.com/dotnet/csharplang/issues/570
