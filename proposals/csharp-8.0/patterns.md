---
ms.openlocfilehash: fad1425e2871f395a2eb1f39faccbc773d88d6a3
ms.sourcegitcommit: da1180f7eacdd5067b32d291a76e6764159e00fe
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2019
ms.locfileid: "79485060"
---
# <a name="recursive-pattern-matching"></a>遞迴模式比對

## <a name="summary"></a>摘要
[summary]: #summary

模式比對C#延伸模組可從功能性語言啟用許多代數資料類型和模式比對的優點，並以順暢地與基礎語言的風格整合的方式。 這種方法的元素是由程式設計語言[F#](http://www.msr-waypoint.net/pubs/79947/p29-syme.pdf "透過輕量語言的可擴充模式比對")和[Scala](https://link.springer.com/content/pdf/10.1007%2F978-3-540-73589-2.pdf "符合具有模式的物件，頁面273")中的相關功能所靈感。

## <a name="detailed-design"></a>詳細設計
[design]: #detailed-design

### <a name="is-expression"></a>Is 運算式

`is` 運算子已擴充，可針對*模式*測試運算式。

```antlr
relational_expression
    : is_pattern_expression
    ;
is_pattern_expression
    : relational_expression 'is' pattern
    ;
```

這種形式的*relational_expression*除了C#規格中的現有表單之外。 如果 `is` token 左邊的*relational_expression*未指定值或沒有類型，就會發生編譯時期錯誤。

模式的每個*識別碼*都會導入一個新的區域變數，在 `is` 運算子 `true` （也就是*明確指派的值*）之後，*一定會指派*此變數。

> 注意：在技術上，`is-expression` 和*constant_pattern*的*類型*之間有一個不明確的，這可能是限定識別碼的有效剖析。 我們嘗試將它系結為類型，以與舊版語言相容;只有當我們在其他內容中執行運算式時，才會解決這個問題，直到第一個發現的（必須是常數或型別）。 這種不明確的情況只會出現在 `is` 運算式的右手邊。

### <a name="patterns"></a>模式

模式可用於*is_pattern*運算子、 *switch_statement*和*switch_expression*中，以表達要比較的傳入資料（我們所謂的輸入值）的資料圖形。 模式可能是遞迴的，因此可能會比對部分的資料與子模式。

```antlr
pattern
    : declaration_pattern
    | constant_pattern
    | var_pattern
    | positional_pattern
    | property_pattern
    | discard_pattern
    ;
declaration_pattern
    : type simple_designation
    ;
constant_pattern
    : expression
    ;
var_pattern
    : 'var' designation
    ;
positional_pattern
    : type? '(' subpatterns? ')' property_subpattern? simple_designation?
    ;
subpatterns
    : subpattern
    | subpattern ',' subpatterns
    ;
subpattern
    : pattern
    | identifier ':' pattern
    ;
property_subpattern
    : '{' subpatterns? '}'
    ;
property_pattern
    : type? property_subpattern simple_designation?
    ;
simple_designation
    : single_variable_designation
    | discard_designation
    ;
discard_pattern
    : '_'
    ;
```

#### <a name="declaration-pattern"></a>宣告模式

```antlr
declaration_pattern
    : type simple_designation
    ;
```

*Declaration_pattern*會測試運算式是否為指定的類型，並在測試成功時將它轉換成該類型。 如果指定是*single_variable_designation*，這可能會導入指定識別碼所命名之給定類型的本機變數。 當模式比對作業的結果是 `true`時，就會*明確地指派*該本機變數。

這個運算式的執行時間語義是，它會針對模式中的*類型*，測試左側*relational_expression*運算元的執行時間類型。  如果它是該執行時間類型（或某些子類型），而不是 `null`，則會 `true``is operator` 的結果。

左側和指定類型之靜態類型的某些組合會被視為不相容，並導致編譯時期錯誤。 如果存在識別轉換、隱含參考轉換、裝箱轉換、明確參考轉換或從 `E` 到 `T`的取消裝箱轉換，或如果其中一個類型是開啟的類型，則靜態類型 `E` 的值會被視為與類型 `T`*模式相容*。 如果 `E` 類型的輸入不是與它所比對之類型模式中的*類型* *模式相容*，就會發生編譯時期錯誤。

類型模式適用于執行參考型別的執行時間類型測試，並取代方法

```csharp
var v = expr as Type;
if (v != null) { // code using v
```

稍微變得更簡潔

```csharp
if (expr is Type v) { // code using v
```

如果*型*別是可為 null 的實值型別，就會發生錯誤。

類型模式可用來測試可為 null 之類型的值： `Nullable<T>` 類型的值（或已裝箱的 `T`）符合類型模式 `T2 id` 如果值為非 null，且 `T2` 的類型為 `T`，或 `T`的部分基底類型或介面。 例如，在程式碼片段中

```csharp
int? x = 3;
if (x is int v) { // code using v
```

`if` 語句的條件會在執行時間 `true`，而變數 `v` 會保存區塊內類型 `int` 的值 `3`。 在區塊之後，變數 `v` 會在範圍內，但不一定會指派。

#### <a name="constant-pattern"></a>常數模式

```antlr
constant_pattern
    : constant_expression
    ;
```

常數模式會針對常數值測試運算式的值。 常數可以是任何常數運算式，例如常值、宣告 `const` 變數的名稱或列舉常數。 當輸入值不是開放式型別時，常數運算式會隱含地轉換成相符運算式的型別。如果輸入值的類型與常數運算式的類型不是*模式相容*，則模式比對作業會是錯誤。

如果 `object.Equals(c, e)` 會傳回 `true`，模式*c*會視為符合轉換後的輸入值*e* 。

我們預期在新撰寫的程式碼中測試 `null` 最常見的方式 `e is null`，因為它無法叫用使用者定義的 `operator==`。

#### <a name="var-pattern"></a>Var 模式

```antlr
var_pattern
    : 'var' designation
    ;
designation
    : simple_designation
    | tuple_designation
    ;
simple_designation
    : single_variable_designation
    | discard_designation
    ;
single_variable_designation
    : identifier
    ;
discard_designation
    : _
    ;
tuple_designation
    : '(' designations? ')'
    ;
designations
    : designation
    | designations ',' designation
    ;
```

如果*指定*為*simple_designation*，則運算式*e*會符合模式。 換句話說，與*var 模式*的比對一律會以*simple_designation*成功。 如果*simple_designation*是*single_variable_designation*，則*e*的值會系結至新引進的本機變數。 本機變數的類型是*e*的靜態類型。

如果*指定*為*tuple_designation*，則模式相當於格式為 `(var`*指定*... `)` 的*positional_pattern* ，其中*指定*的是在*tuple_designation*中找到的。  例如，模式 `var (x, (y, z))` 相當於 `(var x, (var y, var z))`。

如果名稱 `var` 系結至類型，就會發生錯誤。

#### <a name="discard-pattern"></a>捨棄模式

```antlr
discard_pattern
    : '_'
    ;
```

運算式*e*符合模式 `_` always。 換句話說，每個運算式都符合捨棄模式。

捨棄模式不得當做*is_pattern_expression*的模式使用。

#### <a name="positional-pattern"></a>位置模式

位置模式會檢查輸入值是否不 `null`、叫用適當的 `Deconstruct` 方法，並對產生的值執行進一步的模式比對。  當輸入值的型別與包含 `Deconstruct`的型別相同，或者輸入值的型別是元組型別，或者輸入值的型別是 `object` 或 `ITuple`，而且運算式的執行時間型別會執行 `ITuple`時，它也支援類似元組的模式語法（不需要提供型別）。

```antlr
positional_pattern
    : type? '(' subpatterns? ')' property_subpattern? simple_designation?
    ;
subpatterns
    : subpattern
    | subpattern ',' subpatterns
    ;
subpattern
    : pattern
    | identifier ':' pattern
    ;
```

如果省略*類型*，我們會將它視為輸入值的靜態類型。

假設符合輸入值與模式*類型*`(` *subpattern_list* `)`，則會在*類型*中搜尋 `Deconstruct` 的可存取宣告，並使用解構宣告的相同規則來選取其中一個方法，藉以選取該方法。

如果*positional_pattern*省略型別、具有沒有*識別碼*的單一子*模式*、沒有任何*property_subpattern* ，而且沒有*simple_designation*，就會發生錯誤。 這會在以括弧括住的*constant_pattern*和*positional_pattern*之間進行厘清。

為了將值解壓縮以符合清單中的模式，
- 如果省略了*type* ，而輸入值的類型是元組類型，則 subpatterns 的數目必須與元組的基數相同。 每個元組元素會與對應的子*模式*進行比對，如果所有這些專案都成功，則比對成功。 如果有任何子*模式*具有*識別碼*，則必須在元組類型中的對應位置上命名元組元素。
- 否則，如果適當的 `Deconstruct` 當做*類型*的成員存在，則如果輸入值的類型與*類型*不*相容*，就會發生編譯時期錯誤。 在執行時間，會針對*類型*測試輸入值。 如果此作業失敗，則位置模式比對會失敗。 如果成功，則會將輸入值轉換成這個類型，並使用全新的編譯器產生的變數來叫用 `Deconstruct`，以接收 `out` 參數。 每個收到的值都會針對對應的子*模式*進行比對，如果全部都成功，則比對成功。 如果有任何子*模式*具有*識別碼*，則必須在 `Deconstruct`的對應位置上具名引數。
- 否則，如果省略了*type* ，而輸入值的類型為 `object` 或 `ITuple` 或可轉換成隱含參考轉換 `ITuple` 的某種類型，則*我們會使用*`ITuple`進行比對。
- 否則，模式為編譯時期錯誤。

Subpatterns 在執行時間中的相符順序未指定，而且失敗的比對可能不會嘗試比對所有 subpatterns。

##### <a name="example"></a>範例

這個範例會使用此規格中所述的許多功能

``` c#
    var newState = (GetState(), action, hasKey) switch {
        (DoorState.Closed, Action.Open, _) => DoorState.Opened,
        (DoorState.Opened, Action.Close, _) => DoorState.Closed,
        (DoorState.Closed, Action.Lock, true) => DoorState.Locked,
        (DoorState.Locked, Action.Unlock, true) => DoorState.Closed,
        (var state, _, _) => state };
```

#### <a name="property-pattern"></a>屬性模式

屬性模式會檢查輸入值是否不 `null`，並以遞迴方式符合使用可存取屬性或欄位所解壓縮的值。

```antlr
property_pattern
    : type? property_subpattern simple_designation?
    ;
property_subpattern
    : '{' '}'
    | '{' subpatterns ','? '}'
    ;
```

如果_property_pattern_的任何子_模式_不包含_識別碼_（必須是具有_識別碼_的第二個格式），就會發生錯誤。  最後一個子模式後面的尾端逗號是選擇性的。

請注意，null 檢查模式不屬於簡單的屬性模式。 若要檢查字串 `s` 是否為非 null，您可以撰寫下列任何一種形式

```csharp
if (s is object o) ... // o is of type object
if (s is string x) ... // x is of type string
if (s is {} x) ... // x is of type string
if (s is {}) ...
```

假設符合運算式*e*與模式*類型*`{` *property_pattern_list* `}`，如果運算式*e*與*類型*指定的類型*t*不*相容*，就會發生編譯時期錯誤。 如果類型不存在，我們會將它視為靜態類型*e*。 如果*識別碼*存在，它會宣告類型*類型*的模式變數。 出現在其*property_pattern_list*左邊的每個識別碼，都必須指定可存取的可讀取屬性或*T*的欄位。如果*property_pattern*的*simple_designation*存在，它會定義*T*類型的模式變數。

在執行時間，運算式會針對*T*進行測試。如果這項作業失敗，則屬性模式比對會失敗，且結果會是 `false`。 如果成功，則會讀取每個*property_subpattern*欄位或屬性，並將其值與對應的模式進行比對。 只有在其中任一項的結果都 `false`時，才會 `false` 整個比對的結果。 未指定 subpatterns 的相符順序，而且失敗的比對可能不符合執行時間的所有 subpatterns。 如果比對成功，且*property_pattern*的*simple_designation*是*single_variable_designation*，它會定義指派了相符值的*T*類型變數。

> 注意：屬性模式可以用來搭配匿名型別進行模式比對。

##### <a name="example"></a>範例

```csharp
if (o is string { Length: 5 } s)
```

### <a name="switch-expression"></a>Switch 運算式

加入*switch_expression* ，以支援運算式內容 `switch`like 的語義。

C#語言語法會隨著下列語法生產而增強：

```antlr
multiplicative_expression
    : switch_expression
    | multiplicative_expression '*' switch_expression
    | multiplicative_expression '/' switch_expression
    | multiplicative_expression '%' switch_expression
    ;
switch_expression
    : range_expression 'switch' '{' '}'
    | range_expression 'switch' '{' switch_expression_arms ','? '}'
    ;
switch_expression_arms
    : switch_expression_arm
    | switch_expression_arms ',' switch_expression_arm
    ;
switch_expression_arm
    : pattern case_guard? '=>' expression
    ;
case_guard
    : 'when' null_coalescing_expression
    ;
```

不允許*switch_expression*做為*expression_statement*。

> 我們想要在未來的修訂中放寬這項功能。

如果這類類型存在，而且 switch 運算式的每個 arm 中的運算式可以隱含地轉換成該類型，則*switch_expression*的類型就是出現在*switch_expression_arm*之 `=>` token 右邊的[*最常見運算式類型*](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions)。  此外，我們新增了新的*切換運算式轉換*，這是從 switch 運算式到每個類型的預先定義隱含轉換，`T` 從每個 arm 的運算式隱含轉換到 `T`。

如果部分*switch_expression_arm*的模式無法影響結果，就會發生錯誤，因為先前的模式和防護一定會相符。

如果 switch 運算式的某些 arm 會處理其輸入的每個值，則 switch 運算式會被視為*完整*的。  如果 switch 運算式不是*完整*的，編譯器應該會產生警告。

在執行時間， *switch_expression*的結果是第一個*switch_expression_arm* *switch_expression*左邊的運算式與*switch_expression_arm*的模式相符之*運算式*的值，而*case_guard* *的 switch_expression_arm* （如果有的話）則會評估為 [`true`]。 如果沒有這類*switch_expression_arm*， *switch_expression*就會擲回例外狀況 `System.Runtime.CompilerServices.SwitchExpressionException`的實例。

### <a name="optional-parens-when-switching-on-a-tuple-literal"></a>在元組常值上切換時的選擇性括弧

若要使用*switch_statement*來切換元組常值，您必須撰寫看似重複的括弧

```csharp
switch ((a, b))
{
```

允許

```csharp
switch (a, b)
{
```

當正在切換的運算式是元組常值時，switch 語句的括弧是選擇性的。

### <a name="order-of-evaluation-in-pattern-matching"></a>模式比對中的評估順序

提供編譯器彈性來重新排列模式比對期間所執行的作業，可允許用來改善模式比對效率的彈性。 （未強制的）需求是在模式中存取的屬性和解構方法都必須是「純」（無副作用、等冪等）。 這並不表示我們會將純度新增為語言概念，只是為了讓編譯器能夠彈性地重新排列作業。

**解決方式 2018-04-04 LDM**：已確認：允許編譯器重新排序 `ITuple`中的 `Deconstruct`、屬性存取和調用的呼叫，而且可能會假設傳回的值與多個呼叫相同。 編譯器不應叫用不會影響結果的函式，因此，在未來對編譯器所產生的評估順序進行任何變更之前，我們會非常謹慎。

### <a name="some-possible-optimizations"></a>一些可能的優化

模式比對的編譯可以利用模式的通用部分。 例如，如果*switch_statement*中的兩個連續模式的頂層型別測試是相同的型別，則產生的程式碼可以略過第二個模式的型別測試。

當某些模式為整數或字串時，編譯器會產生它針對舊版語言中的 switch 語句所產生的相同類型程式碼。

如需這些優化類型的詳細資訊，請參閱[[Scott And Ramsey （2000）]](https://www.cs.tufts.edu/~nr/cs257/archive/norman-ramsey/match.pdf "符合編譯啟發學習法的時機為何？")。
