---
ms.openlocfilehash: aecbf4298053debdb479ad3aaf6e3db468918455
ms.sourcegitcommit: 02b535d712cc9e022290e14d9e0324508b28f231
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/26/2020
ms.locfileid: "79485207"
---
# <a name="nullable-reference-types-specification"></a>可為 null 的參考型別規格

這是進行中的工作-有幾個部分遺失或不完整。 ***

## <a name="syntax"></a>語法

### <a name="nullable-reference-types"></a>可為 Null 的參考型別

可為 null 的參考型別與簡短形式的可為 null 實數值型別具有相同的語法 `T?`，但沒有對應的長格式。

基於規格的目的，目前的 `nullable_type` 生產會重新命名為 `nullable_value_type`，並新增 `nullable_reference_type` 的生產環境：

```antlr
reference_type
    : ...
    | nullable_reference_type
    ;
    
nullable_reference_type
    : non_nullable_reference_type '?'
    ;
    
non_nullable_reference_type
    : type
    ;
```

`nullable_reference_type` 中的 `non_nullable_reference_type` 必須是不可為 null 的參考型別（類別、介面、委派或陣列），或是限制為不可為 null 的參考型別的類型參數（透過 `class` 條件約束，或 `object`以外的類別）。

可為 null 的參考型別不能出現在下列位置：

- 做為基類或介面
- 做為 `member_access` 的接收者
- 做為 `object_creation_expression` 中的 `type`
- 做為 `delegate_creation_expression` 中的 `delegate_type`
- 做為 `is_expression`中的 `type`、`catch_clause` 或 `type_pattern`
- 作為完整介面成員名稱中的 `interface`

警告是在可為 null 注釋內容已停用的 `nullable_reference_type` 上提供。

### <a name="nullable-class-constraint"></a>可為 null 的類別條件約束

`class` 條件約束具有可為 null 的對應 `class?`：

```antlr
primary_constraint
    : ...
    | 'class' '?'
    ;
```

### <a name="the-null-forgiving-operator"></a>Null-容許運算子

修正後 `!` 運算子稱為 null 容許運算子。

```antlr
primary_expression
    : ...
    | null_forgiving_expression
    ;
    
null_forgiving_expression
    : primary_expression '!'
    ;
```

`primary_expression` 必須是參考型別。  

後置 `!` 運算子沒有執行時間效果-它會評估為基礎運算式的結果。 其唯一的角色是變更運算式的 null 狀態，並限制其使用所提供的警告。

### <a name="nullable-implicitly-typed-local-variables"></a>可為 null 的隱含類型區域變數

`var` 推斷參考型別的批註類型。
例如，在 `var s = "";` 會將 `var` 推斷為 `string?`。

### <a name="nullable-compiler-directives"></a>可為 null 的編譯器指示詞

`#nullable` 指示詞會控制可為 null 的注釋和警告內容。

```antlr
pp_directive
    : ...
    | pp_nullable
    ;
    
pp_nullable
    : whitespace? '#' whitespace? 'nullable' whitespace nullable_action pp_new_line
    ;
    
nullable_action
    : 'disable'
    | 'enable'
    | 'restore'
    ;
```

`#pragma warning` 指示詞會展開以允許變更可為 null 的警告內容，並允許在上啟用個別警告，即使它們預設為停用也一樣：

```antlr
pragma_warning_body
    : ...
    | 'warning' whitespace nullable_action whitespace 'nullable'
    ;

warning_action
    : ...
    | 'enable'
    ;
```

請注意，新形式的 `pragma_warning_body` 會使用 `nullable_action`，而不是 `warning_action`。

## <a name="nullable-contexts"></a>可為 Null 內容

原始程式碼的每一行都有*可為 null 的注釋內容*和*可為 null 的警告內容*。 這些控制項會控制可為 null 的注釋是否有效果，以及是否提供 null 屬性的警告。 指定行的注釋內容為*停用*或*啟用*。 指定行的警告內容已*停用*或*啟用*。

這兩個內容可以在專案層級（在C#原始程式碼外部）或原始程式檔內的任何位置，透過 `#nullable` 的預處理器指示詞來指定。 如果未提供專案層級設定，則預設值為*停用*這兩個內容。

`#nullable` 指示詞會控制來源文字中的注釋和警告內容，並優先于專案層級的設定。

指示詞會設定 it 針對後續程式程式碼所控制的內容，直到另一個指示詞覆寫它，或直到原始程式檔結束為止。

指示詞的效果如下所示：

- `#nullable disable`：將可為 null 注釋和警告內容設定為*停用*
- `#nullable enable`：將可為 null 注釋和警告內容設定為*已啟用*
- `#nullable restore`：將可為 null 的注釋和警告內容還原至專案設定
- `#nullable disable annotations`：將可為 null 注釋內容設定為*停用*
- `#nullable enable annotations`：將可為 null 注釋內容設定為*已啟用*
- `#nullable restore annotations`：將可為 null 注釋內容還原至專案設定
- `#nullable disable warnings`：將可為 null 警告內容設定為*停用*
- `#nullable enable warnings`：將可為 null 警告內容設定為*已啟用*
- `#nullable restore warnings`：將可為 null 警告內容還原至專案設定

## <a name="nullability-of-types"></a>型別的可 NULL 性

給定的類型可以有四個 nullabilities 的其中一個：*遺忘式*、*不可為 null*、 *nullable*和*unknown*。 

*不可為 null*和*未知*的類型可能會在指派可能的 `null` 值時造成警告。 不過，*遺忘式*和*nullable*型別都是「可指派*null*」，而且可以在沒有警告的情況下指派 `null` 值。 

*遺忘式*和*不可為 null*類型可以解除引用或指派，而不會出現警告。 不過， *nullable*和*unknown*類型的值都是「*null*」，而且可能會在解除引用或指派時造成警告，而不會有適當的 null 檢查。 

Null 產生類型的*預設 null 狀態*為「可能是 null」。 非 null 產生類型的預設 null 狀態為「非 null」。

類型和在判斷其 null 屬性中所發生的可為 null 注釋內容的種類：

- 不可為 null 數值型別 `S` 一律為*不可為 null*
- 可為 null 的實數值型別 `S?` 一律*可為 null*
- 已*停用*注釋內容中 `C` 的 unannotated 參考型別為*遺忘式*
- *已啟用*注釋內容中 `C` 的 unannotated 參考型別為*不可為 null*
- 可為 null 的參考型別 `C?` 可為*null* （但可能會在*停用*的注釋內容中產生警告）

類型參數會另外將其條件約束納入考慮：

- 類型參數 `T`，其中所有條件約束（如果有的話）為 null 產生類型（*可為 null*和*未知*），或 `class?` 條件約束*不明*
- 類型參數 `T`，其中至少有一個條件約束為*遺忘式*或*不可為 null* ，或其中一個 `struct` 或 `class` 條件約束為
    - *已停*用注釋內容中的*遺忘式*
    - *已啟用*注釋內容中的*不可為 null*
- 可為 null 的型別參數 `T?`，其中至少有一個 `T`的條件約束為*遺忘式*或*不可為 null* ，或其中一個 `struct` 或 `class` 條件約束為
    - *停*用的注釋內容中*為 nullable* （但產生警告）
    - *啟用*的注釋內容中*可為 null*

`T`的型別參數，只有在 `T` 已知為實值型別或已知為引用型別時，才允許 `T?`。

### <a name="oblivious-vs-nonnullable"></a>遺忘式與不可為 null

當類型的最後一個 token 在該內容中時，會將 `type` 視為在指定的注釋內容中發生。

在原始程式碼中 `C` 指定的參考型別是否會解讀為遺忘式或不可為 null，取決於該原始程式碼的注釋內容。 但是一旦建立之後，它就會被視為該類型的一部分，並在替代泛型型別引數時，使用它來傳送。 就像在型別上有 `?` 的注釋一樣，但不會出現。

## <a name="constraints"></a>條件約束

可為 null 的參考型別可以當做泛型條件約束使用。 此外 `object` 現在也有效，做為明確的條件約束。 缺少條件約束現在等同于 `object?` 條件約束（而不是 `object`），但（不同于之前的 `object`） `object?` 不會被禁止為明確條件約束。

`class?` 是表示「可能可為 null 的參考型別」的新條件約束，而 `class` 則代表 "不可為 null reference type"。

型別引數或條件約束的 null 屬性不會影響型別是否符合條件約束，但目前的情況（可為 null 的實值型別不符合 `struct` 條件約束）除外。 不過，如果類型引數不符合條件約束的 null 屬性需求，可能會提供警告。

## <a name="null-state-and-null-tracking"></a>Null 狀態和 null 追蹤

指定來源位置中的每個運算式都有*null 狀態*，指出是否認為它可能會評估為 null。 Null 狀態是「不是 null」或「可能是 null」。 Null 狀態是用來判斷是否應該提供有關 null 不安全轉換和取值的警告。

### <a name="null-tracking-for-variables"></a>變數的 Null 追蹤

對於表示變數或屬性的特定運算式，會根據指派的專案，在發生次數之間追蹤 null 狀態，並在其上執行測試，並在兩者之間進行控制流程。 這類似于針對變數追蹤明確指派的方式。 追蹤的運算式是下列形式的其中一種：

```antlr
tracked_expression
    : simple_name
    | this
    | base
    | tracked_expression '.' identifier
    ;
```

其中識別碼代表欄位或屬性。

***描述 null 狀態轉換，類似于明確指派***

### <a name="null-state-for-expressions"></a>運算式的 Null 狀態

運算式的 null 狀態衍生自其表單和類型，以及其相關變數的 null 狀態。

### <a name="literals"></a>常值

`null` 常值的 null 狀態為「可能是 null」。 要轉換成已知不是不可為 null 數值型別之類型的 `default` 常值的 null 狀態是「可能是 null」。 任何其他常值的 null 狀態為「不是 null」。

### <a name="simple-names"></a>簡單名稱

如果 `simple_name` 未分類為值，其 null 狀態會是「不是 null」。 否則，它是追蹤的運算式，而其 null 狀態是其在此來源位置的追蹤 null 狀態。

### <a name="member-access"></a>成員存取

如果 `member_access` 未分類為值，其 null 狀態會是「不是 null」。 否則，如果它是追蹤的運算式，其 null 狀態就是其在此來源位置的追蹤 null 狀態。 否則，其 null 狀態為其類型的預設 null 狀態。

### <a name="invocation-expressions"></a>叫用運算式

如果 `invocation_expression` 叫用使用一或多個屬性所宣告的成員來進行特殊的 null 行為，則 null 狀態會由這些屬性決定。 否則，運算式的 null 狀態就是其類型的預設 null 狀態。

### <a name="element-access"></a>元素存取

如果 `element_access` 叫用使用一或多個屬性（attribute）宣告的索引子以進行特殊的 null 行為，則 null 狀態會由這些屬性所決定。 否則，運算式的 null 狀態就是其類型的預設 null 狀態。

### <a name="base-access"></a>基底存取

如果 `B` 表示封入類型的基底類型，則 `base.I` 與 `((B)this).I` 具有相同的 null 狀態，而且 `base[E]` 與 `((B)this)[E]`具有相同的 null 狀態。

### <a name="default-expressions"></a>預設運算式

如果已知 `T` 為不可為 null 數值型別，`default(T)` 具有 null 狀態「非 null」。 否則，它會有 null 狀態「可能是 null」。

### <a name="null-conditional-expressions"></a>Null-條件運算式

`null_conditional_expression` 具有 null 狀態「可能是 null」。

### <a name="cast-expressions"></a>Cast 運算式

如果 cast 運算式 `(T)E` 叫用使用者定義的轉換，則運算式的 null 狀態就是其類型的預設 null 狀態。 否則，如果 `T` 是 null （*可為*null 或*未知*），則 null 狀態會是「可能是 null」。 否則，null 狀態會與 `E`的 null 狀態相同。

### <a name="await-expressions"></a>Await 運算式

`await E` 的 null 狀態是其類型的預設 null 狀態。

### <a name="the-as-operator"></a>`as` 運算子

`as` 運算式的狀態為「可能是 null」。

### <a name="the-null-coalescing-operator"></a>Null 聯合運算子

`E1 ?? E2` 具有與 `E2` 相同的 null 狀態

### <a name="the-conditional-operator"></a>條件運算子

如果 `E2` 和 `E3` 的 null 狀態為 "not null"，則 `E1 ? E2 : E3` 的 null 狀態為 "not null"。 否則就是「可能是 null」。

### <a name="query-expressions"></a>查詢運算式

查詢運算式的 null 狀態是其類型的預設 null 狀態。

### <a name="assignment-operators"></a>指派運算子

套用任何隱含轉換之後，`E1 = E2` 和 `E1 op= E2` 與 `E2` 具有相同的 null 狀態。

### <a name="unary-and-binary-operators"></a>一元和二元運算子

如果一元或二元運算子會叫用一個或多個屬性所宣告的使用者定義運算子，以進行特殊的 null 行為，則 null 狀態會由這些屬性決定。 否則，運算式的 null 狀態就是其類型的預設 null 狀態。

***在字串和委派上執行二進位 `+` 特別有用？***

### <a name="expressions-that-propagate-null-state"></a>傳播 null 狀態的運算式

`(E)`、`checked(E)` 和 `unchecked(E)` 都具有與 `E`相同的 null 狀態。

### <a name="expressions-that-are-never-null"></a>永不為 null 的運算式

下列運算式形式的 null 狀態一律為 "not null"：

- `this` access
- 字串插值
- `new` 運算式（物件、委派、匿名物件和陣列建立運算式）
- `typeof` 運算式
- `nameof` 運算式
- 匿名函式（匿名方法和 lambda 運算式）
- null-容許運算式
- `is` 運算式

## <a name="type-inference"></a>型別推斷

### <a name="type-inference-for-var"></a>`var` 的型別推斷

針對以 `var` 宣告的區域變數所推斷的類型，會由初始化運算式的 null 狀態來通知。

```csharp
var x = E;
```

如果 `E` 的類型是可為 null 的參考型別 `C?` 而且 `E` 的 null 狀態為 "not null"，則會 `C`為 `x` 推斷的類型。 否則，推斷的類型會是 `E`的類型。

針對 `x` 所推斷之類型的 null 屬性會根據 `var`的注釋內容來決定，就如同在該位置明確指定類型一樣。

### <a name="type-inference-for-var"></a>`var?` 的型別推斷

針對以 `var?` 宣告的區域變數所推斷的類型，與初始化運算式的 null 狀態無關。

```csharp
var? x = E;
```

如果 `E` 的型別 `T` 是可為 null 的實值型別或可為 null 的參考型別，則會 `T`為 `x` 推斷的型別。 否則，如果 `T` 是不可為 null 實值型別，`S` 推斷的型別會 `S?`。 否則，如果 `T` 是不可為 null 參考型別，`C` 推斷的型別會 `C?`。 否則，宣告是不合法的。

針對 `x` 所推斷之型別的 null 屬性一律為*nullable*。

### <a name="generic-type-inference"></a>泛型型別推斷

泛型型別推斷已增強，可協助決定推斷的參考型別是否應為可為 null。 這是最好的做法，而且本身不會產生警告，但當所選多載的推斷類型套用至引數時，可能會導致可為 null 的警告。

型別推斷不依賴傳入類型的注釋內容。 相反地，會推斷 `type`，它會從它的「原本就」（如果已明確表示），取得自己的注釋內容。 這會將型別推斷的角色底線，以方便您自行撰寫。

更精確地說，推斷的型別引數的注釋內容是 token 的內容，此標記後面會接著 `<...>` 型別參數清單，還有一個。也就是所呼叫的泛型方法名稱。 針對轉譯為這類呼叫的查詢運算式，會從產生呼叫之查詢子句的初始內容關鍵字取得內容。

### <a name="the-first-phase"></a>第一個階段

可為 null 的參考型別會流入初始運算式的界限，如下所述。 此外，也會引進兩種新的界限，也就是 `null` 和 `default`。 其目的是要執行輸入運算式中出現的 `null` 或 `default`，這可能會導致推斷的類型可為 null，即使不是也一樣。 這甚至適用于可為 null 的實*值*型別，其增強為在推斷程式中挑選「null」。

決定要在第一個階段中新增的界限，其增強方式如下：

如果 `Ei` 的引數具有參考型別，則用於推斷的型別 `U` 會根據 `Ei` 的 null 狀態以及其宣告的型別而定：
- 如果宣告的類型是不可為 null 參考型別 `U0` 或可為 null 的參考型別 `U0?` 則為
    - 如果 `Ei` 的 null 狀態為 "not null"，則 `U` 會 `U0`
    - 如果 `Ei` 的 null 狀態是「可能是 null」，則 `U` 會 `U0?`
- 否則，如果 `Ei` 有已宣告的型別，`U` 就是該型別
- 否則，如果 `Ei` `null`，則 `U` 是特殊界限 `null`
- 否則，如果 `Ei` `default`，則 `U` 是特殊界限 `default`
- 否則不會進行推斷。

### <a name="exact-upper-bound-and-lower-bound-inferences"></a>精確、上限和下限推斷

*從*類型 `U` 推斷*到*類型 `V`，如果 `V` 是可為 null 的參考型別 `V0?`，則會使用 `V0`，而不是在下列子句中 `V`。
- 如果 `V` 是其中一個未固定的類型變數，`U` 會加入為精確、上限或下限，如同之前一樣
- 否則，如果 `U` `null` 或 `default`，則不會進行任何推斷
- 否則，如果 `U` 是可為 null 的參考型別 `U0?`，則會使用 `U0`，而不是後續子句中的 `U`。

基本上，其本質上是直接與其中一個非固定類型變數相關的 null 屬性會保留在其界限內。 另一方面，若要推斷會進一步遞迴進入來源和目標型別，則會忽略 null 屬性。 它不一定會相符，但如果沒有，則會在稍後選擇並套用多載時發出警告。

### <a name="fixing"></a>修正

此規格目前並不會說明當多個界限互相轉換，但不同時，會發生什麼事。 這可能會發生在 `object` 和 `dynamic`之間，而不同于元素名稱的元組類型之間，兩者之間的類型之間，而且現在也會在參考型別的 `C` 和 `C?` 之間進行。

此外，我們還需要將 "null" 從輸入運算式傳播到結果型別。 

為了處理這些情況，我們新增了更多階段來修正，這現在是：

1. 將所有界限中的所有類型收集為候選項目，從所有可為 null 的參考型別移除 `?`
2. 根據精確、較低和上限的需求來排除候選人（保留 `null` 和 `default` 界限）
3. 排除沒有隱含轉換至所有其他候選項目的候選項目
4. 如果其餘的候選項目都不會彼此進行身分識別轉換，則型別推斷會失敗
5. *合併*剩餘的候選項目，如下所述
6. 如果產生的候選項是參考型別或不可為 null 值型別，而且*所有*的確切界限或*任何*下限都是可為 null 的實數值型別、可為 null 的參考型別、`null` 或 `default`，則 `?` 會加入至產生的候選項，使其成為可為 null 的實數值型別或參考型別。

兩個候選類型之間會描述*合併*。 它是可轉移和交換的，因此可以使用相同的最終結果，以任何順序合併候選項目。 如果兩個候選型別無法彼此轉換，則未定義。

*Merge*函數接受兩個候選類型和一個方向（ *+* 或 *-* ）：

- *Merge*（`T`、`T`、 *d*） = t
- *Merge*（`S`、`T?`、 *+* ） = *merge*（`S?`、`T`、 *+* ） = *merge*（`S`，`T`， *+* ）`?`
- *Merge*（`S`，`T?`， *-* ） = *merge*（`S?`，`T`， *-* ） = *merge*（`S`，`T`， *-* ）
- *Merge*（`C<S1,...,Sn>`，`C<T1,...,Tn>`， *+* ） = `C<`*merge*（`S1`，`T1`， *d1*）`,...,`*merge*（`Sn`，`Tn`， *dn*）`>`，*其中*
    - 如果 `C<...>` 的 `i`' 第一個類型參數為協變數，則 `di` =  *+*
    - 如果 `C<...>` 的 `i`' 第一個類型參數為非變異或不變，則 `di` =  *-*
- *Merge*（`C<S1,...,Sn>`，`C<T1,...,Tn>`， *-* ） = `C<`*merge*（`S1`，`T1`， *d1*）`,...,`*merge*（`Sn`，`Tn`， *dn*）`>`，*其中*
    - 如果 `C<...>` 的 `i`' 第一個類型參數為協變數，則 `di` =  *-*
    - 如果 `C<...>` 的 `i`' 第一個類型參數為非變異或不變，則 `di` =  *+*
- *Merge*（`(S1 s1,..., Sn sn)`、`(T1 t1,..., Tn tn)`、 *d*） = `(`*merge*（`S1`、`T1`、 *d*）`n1,...,`*merge*（`Sn`，`Tn`， *d*） `nn)`，*其中*
    - 如果 `si` 和 `ti` 不同，或兩者都不存在，則 `ni` 不存在
    - 如果 `si` 和 `ti` 相同，則會 `si` `ni`
- *Merge*（`object`，`dynamic`） = *merge*（`dynamic`，`object`） = `dynamic`

## <a name="warnings"></a>警告

### <a name="potential-null-assignment"></a>可能的 null 指派

### <a name="potential-null-dereference"></a>可能的 null 取值

### <a name="constraint-nullability-mismatch"></a>條件約束 null 屬性不相符

### <a name="nullable-types-in-disabled-annotation-context"></a>已停用注釋內容中可為 null 的類型

## <a name="attributes-for-special-null-behavior"></a>特殊 null 行為的屬性


