---
ms.openlocfilehash: f238a711e710bbac7f5b7400fa938bd85dec00c6
ms.sourcegitcommit: 5278336b61519956240a6f7d83bcb4322019e032
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/10/2020
ms.locfileid: "79485249"
---
# <a name="support-for--and--on-tuple-types"></a>支援元組類型上的 = = 和！ =

允許運算式 `t1 == t2` 其中 `t1` 和 `t2` 是相同基數的元組或可為 null 的元組類型，並大致評估為 `temp1.Item1 == temp2.Item1 && temp1.Item2 == temp2.Item2` （假設 `var temp1 = t1; var temp2 = t2;`）。

相反地，它會允許 `t1 != t2`，並將其評估為 `temp1.Item1 != temp2.Item1 || temp1.Item2 != temp2.Item2`。

在可為 null 的情況下，會使用 `temp1.HasValue` 和 `temp2.HasValue` 的額外檢查。 例如，`nullableT1 == nullableT2` 會評估為 `temp1.HasValue == temp2.HasValue ? (temp1.HasValue ? ... : true) : false`。

當元素的比較傳回非 bool 的結果時（例如，當使用非 bool 使用者定義的 `operator ==` 或 `operator !=`，或在動態比較中）時，該結果將會轉換成 `bool`，或透過 `operator true` 或 `operator false` 執行以取得 `bool`。 元組比較一律會最後傳回 `bool`。

C# 7.2 之後，這類程式碼就會產生錯誤（`error CS0019: Operator '==' cannot be applied to operands of type '(...)' and '(...)'`），除非有使用者定義的 `operator==`。

## <a name="details"></a>詳細資料

系結 `==` （或 `!=`）運算子時，現有的規則包括：（1）動態案例、（2）多載解析，以及（3）失敗。
此提議會在（1）和（2）之間加入一個元組案例：如果比較運算子的兩個運算元都是元組（具有元組類型或為元組常值）且具有相符的基數，則比較會執行元素取向。 這個元組相等也會提升至可為 null 的元組。

兩個運算元（如果是元組常值，則為其元素）會依序從左至右進行評估。 接著會使用每一對元素做為運算元，以遞迴方式系結運算子 `==` （或 `!=`）。 任何具有編譯時間類型的元素 `dynamic` 都會造成錯誤。 這些元素的比較結果會當做條件式和（或）運算子鏈中的運算元使用。

例如，在 `(int, (int, int)) t1, t2;`的內容中，`t1 == (1, (2, 3))` 會評估為 `temp1.Item1 == temp2.Item1 && temp1.Item2.Item1 == temp2.Item2.Item1 && temp2.Item2.Item2 == temp2.Item2.Item2`。

當使用元組常值做為運算元（在任一端）時，它會接收以元素取向轉換所形成的已轉換的元組類型，這會在系結運算子 `==` （或 `!=`）元素時引進。 

例如，在 `(1L, 2, "hello") == (1, 2L, null)`中，兩個元組常值的轉換類型為 `(long, long, string)`，而第二個常值沒有自然類型。


### <a name="deconstruction-and-conversions-to-tuple"></a>元組的解構和轉換
在 `(a, b) == x`中，`x` 可以解構至兩個元素的事實並不扮演角色。 這可能會在未來的提案中，但它會提出有關 `x == y` 的問題（這是簡單的比較或元素的比較，如果是使用什麼基數？）。
同樣地，對元組的轉換也不扮演任何角色。

### <a name="tuple-element-names"></a>元組元素名稱

轉換元組常值時，我們會在常值中提供明確的元組元素名稱時發出警告，但不符合目標元組元素名稱。
我們在元組比較中使用相同的規則，因此假設 `(int a, int b) t` 我們會在 `t == (c, d: 0)`中 `d` 警告。

### <a name="non-bool-element-wise-comparison-results"></a>非 bool 元素的比較結果

如果元素的比較在元組相等中是動態的，我們會使用運算子 `false` 的動態調用，並否定以取得 `bool`，並繼續進行進一步的元素比較。 

如果元素的比較會傳回元組相等中的其他非 bool 類型，則有兩種情況：
- 如果非 bool 類型轉換成 `bool`，我們會套用該轉換。
- 如果沒有這種轉換，但型別具有運算子 `false`，我們將使用它，並將結果否定。

在元組不等比較中，會套用相同的規則，但我們會使用運算子 `true` （不含否定），而不是運算子 `false`。

這些規則類似于在 `if` 語句中使用非 bool 類型的規則，以及一些其他現有的內容。

## <a name="evaluation-order-and-special-cases"></a>評估順序和特殊案例
左側值會先評估，然後再評估右邊的值，然後從左至右（包括轉換，以及根據條件式和/或運算子的現有規則，儘早結束）進行元素的比較。

例如，如果從類型 `A` 轉換成類型 `B` 和方法 `(A, A) GetTuple()`，則評估 `(new A(1), (new B(2), new B(3))) == (new B(4), GetTuple())` 表示：
- `new A(1)`
- `new B(2)`
- `new B(3)`
- `new B(4)`
- `GetTuple()`
- 接著會評估元素取向的轉換和比較和條件式邏輯（將 `new A(1)` 轉換為類型 `B`，然後與 `new B(4)`進行比較，依此類推）。

### <a name="comparing-null-to-null"></a>比較 `null` 與 `null`

這是一般比較的特殊案例，它會執行元組比較。 允許 `null == null` 比較，而且 `null` 常值不會取得任何類型。
在元組相等中，這表示也允許 `(0, null) == (0, null)`，且 `null` 和元組常值不會取得類型。

### <a name="comparing-a-nullable-struct-to-null-without-operator"></a>比較可為 null 的結構與 `null`，而不 `operator==`

這是一般比較的另一個特殊案例，它會執行元組比較。
如果您有沒有 `operator==`的 `struct S`，則會允許 `(S?)x == null` 比較，並將其轉譯為 `((S?).x).HasValue`。
在元組相等中，會套用相同的規則，因此允許 `(0, (S?)x) == (0, null)`。

## <a name="compatibility"></a>相容性

如果有人使用比較運算子的執行來撰寫自己的 `ValueTuple` 類型，則先前已藉由多載解析來挑選它。 但由於新的元組大小寫在多載解析之前，我們會使用元組比較來處理此案例，而不是依賴使用者定義的比較。

----

與[關聯式和類型測試運算子](../../spec/expressions.md#relational-and-type-testing-operators)相關，與[#190](https://github.com/dotnet/csharplang/issues/190)相關聯
