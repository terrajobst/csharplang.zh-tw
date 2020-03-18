---
ms.openlocfilehash: 52b43abd2d8fb56088a68c7169289a63c43ce96f
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484486"
---
# <a name="suppress-emitting-of-localsinit-flag"></a>隱藏發出 `localsinit` 旗標。

* [x] 提議
* [] 原型：未啟動
* [] 執行：未啟動
* [] 規格：未啟動

## <a name="summary"></a>摘要
[summary]: #summary

允許透過 `SkipLocalsInitAttribute` 屬性隱藏發出 `localsinit` 旗標。 

## <a name="motivation"></a>動機
[motivation]: #motivation


### <a name="background"></a>背景
針對不包含參考的每個 CLR 規格區域變數，VM/JIT 不會將其初始化為特定值。 從這類沒有初始化的變數讀取是型別安全的，否則行為是未定義的，而且是特定的執行。 通常未初始化的區域變數會包含目前堆疊框架所佔用的記憶體中所剩的任何值。 這可能會導致不具決定性的行為，並難以重現 bug。 

有兩種方式可以「指派」本機變數： 
- 藉由儲存值或 
- 藉由指定 `localsinit` 旗標，將所有配置的本機記憶體集區所配置的專案，都設為零初始化注意：這包括本機變數和 `stackalloc` 資料。    

不建議使用未初始化的資料，而且不允許在可驗證的程式碼中使用。 雖然您可以透過流程分析的方式來證明它，但允許驗證演算法非常保守，而且只需要設定 `localsinit`。

在C#過去，編譯器會在宣告區域變數的所有方法上發出 `localsinit` 旗標。

雖然C#會採用明確的指派分析，而這比 CLR 規格所需的更C#嚴格（也需要考慮區域變數的範圍），但並不保證產生的程式碼會正式驗證：
- CLR 和C#規則可能不會同意是否將本機當做 `out` 引數傳遞 `use`。
- CLR 和C#規則在已知條件（常數傳播）時，可能不會同意條件式分支的處理。
- CLR 也可能只需要 `localinits`，因為這是允許的。  

### <a name="problem"></a>問題
在高效能應用程式中，強制零初始化的成本可能會很明顯。 使用 `stackalloc` 時，特別明顯。

在某些情況下，JIT 可以在後續的指派中「終止」這類初始化時，省略個別區域變數的初始零初始化。 並非所有 JITs 都這麼做，因此這類優化有其限制。 它不會協助 `stackalloc`。

為了說明問題是 real，有一個已知的 bug，其中不包含任何 `IL` 區域變數的方法不會有 `localsinit` 旗標。 使用者已藉由將 `stackalloc` 放入這類方法中，故意避免初始化成本，而使 bug 遭到惡意探索。 這是儘管沒有 `IL` 的區域變數是不穩定的計量，而且可能會根據 codegen 策略中的變更而有所不同。 Bug 應該是固定的，而且使用者應該會得到更有記載且更可靠的方式來隱藏旗標。 

## <a name="detailed-design"></a>詳細設計

允許將 `System.Runtime.CompilerServices.SkipLocalsInitAttribute` 指定為指示編譯器不要發出 `localsinit` 旗標的方式。
 
最後的結果就是，不能由 JIT 以零初始化區域變數，這在大部分情況下會 unobservable C#。  
除了該 `stackalloc` 資料也不會以零初始化。 這絕對是可觀察的，但也是最能激發的案例。

允許和可辨識的屬性目標為： `Method`、`Property`、`Module`、`Class`、`Struct`、`Interface`、`Constructor`。 不過，編譯器不需要以列出的目標來定義屬性，也不會在意定義屬性的元件。 

在容器上指定屬性時（`class`，`module`，包含嵌套方法的方法，...）時，旗標會影響容器中包含的所有方法。

合成方法會從邏輯容器/擁有者「繼承」旗標。 

旗標只會影響實際方法主體的 codegen 策略。 亦即. 旗標不會影響抽象方法，而且不會傳播至覆寫/執行方法。

這是明確的 **_編譯器功能_** ，而 **_不是語言功能_** 。  
類似于編譯器命令列參數：此功能可控制特定 codegen 策略的執行詳細資料，而且不需要由C#規格所要求。

## <a name="drawbacks"></a>缺點
[drawbacks]: #drawbacks

- 舊的/其他編譯器可能不會接受屬性。
忽略屬性為相容的行為。 只有可能會造成些許效能的衝擊。

- 沒有 `localinits` 旗標的程式碼可能會觸發驗證失敗。
要求這項功能的使用者通常會以可驗證性不在乎。 
 
- 在比個別方法更高的層級套用屬性會有非使用中的效果，這在使用 `stackalloc` 時是可觀察的。 不過，這是最常要求的案例。

## <a name="alternatives"></a>替代方案
[alternatives]: #alternatives

- `unsafe` 內容中宣告方法時，請省略 `localinits` 旗標。 這可能會導致在 `stackalloc`的情況下，不具決定性且危險的行為變更為不確定性。

- 一律省略 `localinits` 旗標。
比上述更糟。

- 除非在方法主體中使用 `stackalloc`，否則請省略 `localinits` 旗標。
不會處理最常要求的案例，也可能會使程式碼無法驗證，而不會有任何選項可還原回。

## <a name="unresolved-questions"></a>未解決的問題
[unresolved]: #unresolved-questions

- 屬性是否實際發出至中繼資料？ 

## <a name="design-meetings"></a>設計會議

還沒有。 