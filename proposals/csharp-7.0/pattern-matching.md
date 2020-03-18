---
ms.openlocfilehash: 3df21c5816be90387a6cd9242e99ba11f43dfd1c
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/14/2020
ms.locfileid: "79485165"
---
# <a name="pattern-matching-for-c-7"></a>7的模式C#比對

模式比對C#延伸模組可從功能性語言啟用許多代數資料類型和模式比對的優點，並以順暢地與基礎語言的風格整合的方式。 基本功能包括：[記錄類型](https://github.com/dotnet/csharplang/blob/master/proposals/records.md)，這些類型的語義意義會由資料的圖形描述;和模式比對，這是新的運算式表單，可啟用這些資料類型非常簡潔的多層分解。 這種方法的元素是由程式設計語言[F#](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/p29-syme.pdf "透過輕量語言的可擴充模式比對")和[Scala](https://infoscience.epfl.ch/record/98468/files/MatchingObjectsWithPatterns-TR.pdf "搭配模式來比對物件")中的相關功能所靈感。

## <a name="is-expression"></a>Is 運算式

`is` 運算子已擴充，可針對*模式*測試運算式。

```antlr
relational_expression
    : relational_expression 'is' pattern
    ;
```

這種形式的*relational_expression*除了C#規格中的現有表單之外。 如果 `is` token 左邊的*relational_expression*未指定值或沒有類型，就會發生編譯時期錯誤。

模式的每個*識別碼*都會導入一個新的區域變數，在 `is` 運算子 `true` （也就是*明確指派的值*）之後，*一定會指派*此變數。

> 注意：在技術上，`is-expression` 和*constant_pattern*的*類型*之間有一個不明確的，這可能是限定識別碼的有效剖析。 我們嘗試將它系結為類型，以與舊版語言相容;只有當該作業失敗時，我們才會將它解析為我們在其他內容中找到的第一件事（必須是常數或型別）。 這種不明確的情況只會出現在 `is` 運算式的右手邊。

## <a name="patterns"></a>模式

模式會用於 `is` 運算子和*switch_statement*中，以表達要比較傳入資料的資料圖形。 模式可能是遞迴的，因此可能會比對部分的資料與子模式。

```antlr
pattern
    : declaration_pattern
    | constant_pattern
    | var_pattern
    ;

declaration_pattern
    : type simple_designation
    ;

constant_pattern
    : shift_expression
    ;

var_pattern
    : 'var' simple_designation
    ;
```

> 注意：在技術上，`is-expression` 和*constant_pattern*的*類型*之間有一個不明確的，這可能是限定識別碼的有效剖析。 我們嘗試將它系結為類型，以與舊版語言相容;只有當該作業失敗時，我們才會將它解析為我們在其他內容中找到的第一件事（必須是常數或型別）。 這種不明確的情況只會出現在 `is` 運算式的右手邊。

### <a name="declaration-pattern"></a>宣告模式

*Declaration_pattern*會測試運算式是否為指定的類型，並在測試成功時將它轉換成該類型。 如果*simple_designation*是識別碼，則會引進給定識別碼所命名之給定類型的本機變數。 當模式比對作業的結果為 true 時，就會*明確地指派*該本機變數。

```antlr
declaration_pattern
    : type simple_designation
    ;
```

這個運算式的執行時間語義是，它會針對模式中的*類型*，測試左側*relational_expression*運算元的執行時間類型。 如果它是該執行時間類型（或某些子類型），則會 `true``is operator` 的結果。 它會宣告一個名為的新區域變數，該*識別碼*會在 `true`結果時指派左邊運算元的值。

左側和指定類型之靜態類型的某些組合會被視為不相容，並導致編譯時期錯誤。 如果存在識別轉換、隱含的參考轉換、裝箱轉換、明確參考轉換，或從 `E` 到 `T`的取消裝箱轉換，則靜態類型 `E` 的值會被視為與類型 `T` 的*模式相容*。 如果 `E` 類型的運算式在與其相符的類型模式中的類型模式不相容，就會發生編譯時期錯誤。

> 注意：[在C# 7.1 中，我們會將此延伸](../csharp-7.1/generics-pattern-match.md)，以允許模式比對作業（如果輸入類型或類型 `T` 是開啟的類型）。 此段落會取代為下列內容：
> 
> 左側和指定類型之靜態類型的某些組合會被視為不相容，並導致編譯時期錯誤。 如果存在身分識別轉換、隱含參考轉換、裝箱轉換、明確參考轉換或從 `E` 到 `T`的取消裝箱轉換，**或 `E` 或 `T` 是開啟的類型**，則靜態類型 `E` 的值會被視為與類型 `T` 的*模式相容*。 如果 `E` 類型的運算式在與其相符的類型模式中的類型模式不相容，就會發生編譯時期錯誤。

宣告模式適用于執行參考型別的執行時間類型測試，並取代方法

```csharp
var v = expr as Type;
if (v != null) { // code using v }
```

稍微變得更簡潔

```csharp
if (expr is Type v) { // code using v }
```

如果*型*別是可為 null 的實值型別，就會發生錯誤。

宣告模式可用來測試可為 null 之類型的值： `Nullable<T>` 類型的值（或已裝箱的 `T`）符合類型模式 `T2 id` 如果值為非 null，且 `T2` 的類型為 `T`，或 `T`的部分基底類型或介面。 例如，在程式碼片段中

```csharp
int? x = 3;
if (x is int v) { // code using v }
```

`if` 語句的條件會在執行時間 `true`，而變數 `v` 會保存區塊內類型 `int` 的值 `3`。

### <a name="constant-pattern"></a>常數模式

```antlr
constant_pattern
    : shift_expression
    ;
```

常數模式會針對常數值測試運算式的值。 常數可以是任何常數運算式，例如常值、宣告 `const` 變數的名稱，或是列舉常數或 `typeof` 運算式。

如果*e*和*c*都是整數類資料類型，則在運算式 `e == c` 的結果 `true`時，會將模式視為相符。

否則，如果 `object.Equals(e, c)` 傳回 `true`，則會將模式視為相符。 在此情況下，如果*e*的靜態類型與常數的類型不*相容*，就會發生編譯時期錯誤。

### <a name="var-pattern"></a>Var 模式

```antlr
var_pattern
    : 'var' simple_designation
    ;
```

運算式*e*符合一律*var_pattern* 。 換句話說，與*var 模式*的比對一律會成功。 如果*simple_designation*是識別碼，則在執行時間， *e*的值會系結至新引進的區域變數。 本機變數的類型是*e*的靜態類型。

如果名稱 `var` 系結至類型，就會發生錯誤。

## <a name="switch-statement"></a>Switch 陳述式

已擴充 `switch` 語句以選取要執行的第一個區塊，其具有與*switch 運算式*相符的關聯模式。

```antlr
switch_label
    : 'case' complex_pattern case_guard? ':'
    | 'case' constant_expression case_guard? ':'
    | 'default' ':'
    ;

case_guard
    : 'when' expression
    ;
```

未定義模式相符的順序。 編譯器允許不按照順序來比對模式，並重複使用已符合模式的結果來計算符合其他模式的結果。

如果有*案例-guard* ，其運算式的類型 `bool`。 它會評估為額外的條件，必須滿足這種情況才會被視為滿足。

如果*switch_label*在執行時間不會有任何作用，就會發生錯誤，因為它的模式是由先前的情況所建立小計。 [TODO：我們應該更精確地瞭解編譯器需要用來達成此判斷的技巧。]

在*switch_label*中宣告的模式變數會在其 case 區塊中明確指派，只有在該案例區塊只包含一個*switch_label*時才會如此。

[TODO：我們應該指定何時可連線到*switch 區塊*。]

### <a name="scope-of-pattern-variables"></a>模式變數的範圍

在模式中宣告的變數範圍如下所示：

- 如果模式是 case 標籤，則變數的範圍會是*案例區塊*。

否則，系統會在*is_pattern*運算式中宣告變數，而且其範圍會以立即包含*is_pattern*運算式之運算式的結構為基礎，如下所示：

- 如果運算式是在運算式主體的 lambda 中，其範圍就是 lambda 的主體。
- 如果運算式位於運算式主體方法或屬性中，其範圍就是方法或屬性的主體。
- 如果運算式位於 `catch` 子句的 `when` 子句中，其範圍就是該 `catch` 子句。
- 如果運算式在*iteration_statement*中，其範圍就只是該語句。
- 否則，如果運算式是在某個其他語句形式中，其範圍就是包含語句的範圍。

為了決定範圍，會將*embedded_statement*視為在其本身的範圍中。 例如， *if_statement*的文法為

``` antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

因此，如果*if_statement*的受控制語句宣告模式變數，其範圍會限制為該*embedded_statement*：

```csharp
if (x) M(y is var z);
```

在此情況下，`z` 的範圍是內嵌的語句 `M(y is var z);`。

其他情況則因其他原因而發生錯誤（例如，在參數的預設值或屬性中，這兩個都是錯誤，因為這些內容都需要常數運算式）。

> [在C# 7.3 中，我們新增了](../csharp-7.3/expression-variables-in-initializers.md)可宣告模式變數的下列內容：
> - 如果運算式是在函式*初始化*運算式中，其範圍會是「函式*初始化*運算式」和「函式的主體」。
> - 如果運算式位於欄位初始化運算式中，其範圍就是其出現所在的*equals_value_clause* 。
> - 如果運算式在指定要轉譯成 lambda 主體的查詢子句中，其範圍就只是該運算式。

## <a name="changes-to-syntactic-disambiguation"></a>語法消除混淆的變更

在某些情況下， C#文法不明確，而語言規格則說明如何解決這些歧義：

> #### <a name="7652-grammar-ambiguities"></a>7.6.5.2 文法多義性
> *簡單名稱*（第7.6.3）和*成員存取*（§7.6.5）的生產可能會使運算式的文法中出現不明確的情況。 例如，語句：
> ```csharp
> F(G<A,B>(7));
> ```
> 可以使用兩個引數（`G < A` 和 `B > (7)`）來解讀為 `F` 的呼叫。 或者，也可以使用一個引數來解讀 `F` 的呼叫，這是使用兩個型別引數和一個一般引數呼叫泛型方法 `G`。

> 如果可以將一連串的 token 剖析（在內容中）為*簡單名稱*（7.6.3）、*成員存取*（第7.6.5），或以*類型引數清單*（第4.4.1）結尾的*指標成員存取*（第18.5.2），則會檢查緊接在結尾 `>` token 後面的權杖。 如果它是下列其中一個
> ```none
> (  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
> ```
> 然後，*類型引數清單*會保留為*簡單名稱*、*成員存取*或*指標成員存取*的一部分，而且會捨棄標記序列的任何其他可能剖析。 否則，即使沒有其他可能的 token 序列剖析，*類型引數清單*也不會被視為*簡單名稱*、*成員存取*或 >*指標成員存取*的一部分。 請注意，在剖析*命名空間或*型別名稱中的型別自*變數清單*（第3.8 節）時，不會套用這些規則。 陳述式
> ```csharp
> F(G<A,B>(7));
> ```
> 根據這項規則，會以一個引數來解讀 `F` 的呼叫，這是使用兩個型別引數和一個一般引數呼叫泛型方法 `G`。 語句
> ```csharp
> F(G < A, B > 7);
> F(G < A, B >> 7);
> ```
> 每個都會以兩個引數來解讀 `F` 的呼叫。 陳述式
> ```csharp
> x = F < A > +y;
> ```
> 將會解讀為小於運算子、大於運算子和一元加號運算子，如同已 `x = (F < A) > (+y)`寫入語句，而不是以*類型引數清單*後面接著二元加號運算子的*簡單名稱*。 在語句中
> ```csharp
> x = y is C<T> + z;
> ```
> `C<T>` 的 token 會以*類型引數清單*的*命名空間或類型名稱*來解讀。

7中C#引進了許多變更，使得這些去除去除規則不再足以處理語言的複雜性。

### <a name="out-variable-declarations"></a>Out 變數宣告

您現在可以在 out 引數中宣告變數：

```csharp
M(out Type name);
```

不過，此類型可能是泛型： 

```csharp
M(out A<B> name);
```

由於引數的語言文法會使用*運算式*，因此此內容會受到去除去除規則的規範。 在此情況下，關閉 `>` 後面接著一個*識別碼*，這不是允許它被視為*類型引數清單*的其中一個標記。 因此，我建議您**將*識別碼*新增至一組權杖，以觸發對*類型引數清單*的**去除混淆。

### <a name="tuples-and-deconstruction-declarations"></a>元組和解構宣告

元組常值會以完全相同的問題來執行。 考慮元組運算式

```csharp
(A < B, C > D, E < F, G > H)
```

在剖析自C#變數清單的舊6個規則底下，這會剖析為具有四個元素的元組，從 `A < B` 開始。 不過，當這種情況出現在解構的左邊時，我們想要由*識別碼*權杖觸發的去除混淆，如上所述：

```csharp
(A<B,C> D, E<F,G> H) = e;
```

這是宣告兩個變數的解構宣告，第一個是類型 `A<B,C>` 且名為 `D`。 換句話說，元組常值包含兩個運算式，其中每一個都是宣告運算式。

為了簡化規格和編譯器，我建議將這個元組常值剖析為兩個元素的元組（不論它是否出現在指派的左側）。 這會是上一節中所述去除混淆的自然結果。

### <a name="pattern-matching"></a>模式比對

模式比對引進了新的內容，其中會出現運算式類型的多義性。 先前 `is` 運算子的右手邊是型別。 現在可以是型別或運算式，如果它是型別，則後面可能接著識別碼。 在技術上，這可以變更現有程式碼的意義：

```csharp
var x = e is T < A > B;
```

這可在 c # 6 規則下剖析為

```csharp
var x = ((e is T) < A) > B;
```

但在 c # 7 規則底下（加上上面提議的去除混淆），會剖析為

```csharp
var x = e is T<A> B;
```

這會宣告 `T<A>`類型的變數 `B`。 幸好，native 和 Roslyn 編譯器有 bug，因此會在 c # 6 程式碼上提供語法錯誤。 因此，這種特定的重大變更不是問題。

模式比對會引進額外的 token，這些權杖應該會驅動明確解決方式來選取類型。 下列現有有效 c # 6 程式碼的範例會中斷，而不會有額外的去除混淆規則：

```csharp
var x = e is A<B> && f;            // &&
var x = e is A<B> || f;            // ||
var x = e is A<B> & f;             // &
var x = e is A<B>[];               // [
```

### <a name="proposed-change-to-the-disambiguation-rule"></a>建議變更去除去除規則

我建議您修改規格，以變更厘清 token 的清單。

>
```none
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```

to

>
```none
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [
```

而且，在某些內容中，我們會將*identifier*視為厘清 token。 這些內容是要辨識的 token 序列緊接在 `is`、`case`或 `out`的其中一個關鍵字前面，或在剖析元組常值的第一個專案（在此情況下，權杖前面會加上 `(` 或 `:`，而識別碼後面接著 `,`）或元組常值的後續元素。

### <a name="modified-disambiguation-rule"></a>修改過的去除規則

修改過的去除混淆規則會類似這樣

> 如果可以將一系列的 token 剖析（在內容中）為*簡單名稱*（§7.6.3）、*成員存取*（第7.6.5），或以*類型引數清單*（第4.4.1）結尾的*指標成員存取*（第18.5.2），則會檢查緊接在結尾 `>` token 後面的標記，以查看是否為
> - 其中一個 `(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [`;或
> - 其中一個關聯式運算子 `<  >  <=  >=  is as`;或
> - 出現在查詢運算式中的內容查詢關鍵字;或
> - 在某些內容中，我們會將*identifier*視為厘清 token。 這些內容是要辨識的 token 順序，其前面會加上其中一個關鍵字 `is`、`case` 或 `out`，或在剖析元組常值的第一個專案時（在此情況下，權杖前面會加上 `(` 或 `:`，而識別碼後面接著 `,`）或元組常值的後續元素。
> 
> 如果下列標記是在此清單中，或這類內容中的識別碼，則*類型引數清單*會保留為*簡單名稱*、*成員存取*或成員*存取*的一部分，而且會捨棄標記序列的任何其他可能剖析。  否則，即使沒有其他可能的 token 序列剖析，也不會將*類型引數清單*視為*簡單名稱*、*成員存取*或成員*存取*的一部分。 請注意，在剖析*命名空間或*型別名稱中的型別自*變數清單*（第3.8 節）時，不會套用這些規則。

### <a name="breaking-changes-due-to-this-proposal"></a>因此提案而造成的重大變更

因為此建議的去除混淆規則，所以不知道任何重大變更。

### <a name="interesting-examples"></a>有趣的範例

以下是這些去除消除規則的一些有趣結果：

運算式 `(A < B, C > D)` 是具有兩個元素的元組，每個都有一個比較。

運算式 `(A<B,C> D, E)` 是具有兩個元素的元組，其中第一個是宣告運算式。

調用 `M(A < B, C > D, E)` 有三個引數。

調用 `M(out A<B,C> D, E)` 有兩個引數，第一個是 `out` 宣告。

運算式 `e is A<B> C` 使用宣告運算式。

Case 標籤 `case A<B> C:` 使用宣告運算式。

## <a name="some-examples-of-pattern-matching"></a>模式比對的一些範例

### <a name="is-as"></a>為-As

我們可以取代此用法

```csharp
var v = expr as Type;   
if (v != null) {
    // code using v
}
```

稍微更精確且直接的

```csharp
if (expr is Type v) {
    // code using v
}
```

### <a name="testing-nullable"></a>測試可為 null

我們可以取代此用法

```csharp
Type? v = x?.y?.z;
if (v.HasValue) {
    var value = v.GetValueOrDefault();
    // code using value
}
```

稍微更精確且直接的

```csharp
if (x?.y?.z is Type value) {
    // code using value
}
```

### <a name="arithmetic-simplification"></a>算術簡化

假設我們定義一組遞迴類型來代表運算式（根據個別的提案）：

```csharp
abstract class Expr;
class X() : Expr;
class Const(double Value) : Expr;
class Add(Expr Left, Expr Right) : Expr;
class Mult(Expr Left, Expr Right) : Expr;
class Neg(Expr Value) : Expr;
```

現在，我們可以定義函式來計算運算式的（unreduced）衍生：

```csharp
Expr Deriv(Expr e)
{
  switch (e) {
    case X(): return Const(1);
    case Const(*): return Const(0);
    case Add(var Left, var Right):
      return Add(Deriv(Left), Deriv(Right));
    case Mult(var Left, var Right):
      return Add(Mult(Deriv(Left), Right), Mult(Left, Deriv(Right)));
    case Neg(var Value):
      return Neg(Deriv(Value));
  }
}
```

運算式 simplifier 會示範位置模式：

```csharp
Expr Simplify(Expr e)
{
  switch (e) {
    case Mult(Const(0), *): return Const(0);
    case Mult(*, Const(0)): return Const(0);
    case Mult(Const(1), var x): return Simplify(x);
    case Mult(var x, Const(1)): return Simplify(x);
    case Mult(Const(var l), Const(var r)): return Const(l*r);
    case Add(Const(0), var x): return Simplify(x);
    case Add(var x, Const(0)): return Simplify(x);
    case Add(Const(var l), Const(var r)): return Const(l+r);
    case Neg(Const(var k)): return Const(-k);
    default: return e;
  }
}
```
