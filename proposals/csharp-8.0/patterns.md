---
ms.openlocfilehash: fad1425e2871f395a2eb1f39faccbc773d88d6a3
ms.sourcegitcommit: da1180f7eacdd5067b32d291a76e6764159e00fe
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2019
ms.locfileid: "79485060"
---
# <a name="recursive-pattern-matching"></a><span data-ttu-id="d0242-101">遞迴模式比對</span><span class="sxs-lookup"><span data-stu-id="d0242-101">Recursive Pattern Matching</span></span>

## <a name="summary"></a><span data-ttu-id="d0242-102">摘要</span><span class="sxs-lookup"><span data-stu-id="d0242-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="d0242-103">模式比對C#延伸模組可從功能性語言啟用許多代數資料類型和模式比對的優點，並以順暢地與基礎語言的風格整合的方式。</span><span class="sxs-lookup"><span data-stu-id="d0242-103">Pattern matching extensions for C# enable many of the benefits of algebraic data types and pattern matching from functional languages, but in a way that smoothly integrates with the feel of the underlying language.</span></span> <span data-ttu-id="d0242-104">這種方法的元素是由程式設計語言[F#](http://www.msr-waypoint.net/pubs/79947/p29-syme.pdf "透過輕量語言的可擴充模式比對")和[Scala](https://link.springer.com/content/pdf/10.1007%2F978-3-540-73589-2.pdf "符合具有模式的物件，頁面273")中的相關功能所靈感。</span><span class="sxs-lookup"><span data-stu-id="d0242-104">Elements of this approach are inspired by related features in the programming languages [F#](http://www.msr-waypoint.net/pubs/79947/p29-syme.pdf "Extensible Pattern Matching Via a Lightweight Language") and [Scala](https://link.springer.com/content/pdf/10.1007%2F978-3-540-73589-2.pdf "Matching Objects With Patterns, page 273").</span></span>

## <a name="detailed-design"></a><span data-ttu-id="d0242-105">詳細設計</span><span class="sxs-lookup"><span data-stu-id="d0242-105">Detailed design</span></span>
[design]: #detailed-design

### <a name="is-expression"></a><span data-ttu-id="d0242-106">Is 運算式</span><span class="sxs-lookup"><span data-stu-id="d0242-106">Is Expression</span></span>

<span data-ttu-id="d0242-107">`is` 運算子已擴充，可針對*模式*測試運算式。</span><span class="sxs-lookup"><span data-stu-id="d0242-107">The `is` operator is extended to test an expression against a *pattern*.</span></span>

```antlr
relational_expression
    : is_pattern_expression
    ;
is_pattern_expression
    : relational_expression 'is' pattern
    ;
```

<span data-ttu-id="d0242-108">這種形式的*relational_expression*除了C#規格中的現有表單之外。</span><span class="sxs-lookup"><span data-stu-id="d0242-108">This form of *relational_expression* is in addition to the existing forms in the C# specification.</span></span> <span data-ttu-id="d0242-109">如果 `is` token 左邊的*relational_expression*未指定值或沒有類型，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="d0242-109">It is a compile-time error if the *relational_expression* to the left of the `is` token does not designate a value or does not have a type.</span></span>

<span data-ttu-id="d0242-110">模式的每個*識別碼*都會導入一個新的區域變數，在 `is` 運算子 `true` （也就是*明確指派的值*）之後，*一定會指派*此變數。</span><span class="sxs-lookup"><span data-stu-id="d0242-110">Every *identifier* of the pattern introduces a new local variable that is *definitely assigned* after the `is` operator is `true` (i.e. *definitely assigned when true*).</span></span>

> <span data-ttu-id="d0242-111">注意：在技術上，`is-expression` 和*constant_pattern*的*類型*之間有一個不明確的，這可能是限定識別碼的有效剖析。</span><span class="sxs-lookup"><span data-stu-id="d0242-111">Note: There is technically an ambiguity between *type* in an `is-expression` and *constant_pattern*, either of which might be a valid parse of a qualified identifier.</span></span> <span data-ttu-id="d0242-112">我們嘗試將它系結為類型，以與舊版語言相容;只有當我們在其他內容中執行運算式時，才會解決這個問題，直到第一個發現的（必須是常數或型別）。</span><span class="sxs-lookup"><span data-stu-id="d0242-112">We try to bind it as a type for compatibility with previous versions of the language; only if that fails do we resolve it as we do an expression in other contexts, to the first thing found (which must be either a constant or a type).</span></span> <span data-ttu-id="d0242-113">這種不明確的情況只會出現在 `is` 運算式的右手邊。</span><span class="sxs-lookup"><span data-stu-id="d0242-113">This ambiguity is only present on the right-hand-side of an `is` expression.</span></span>

### <a name="patterns"></a><span data-ttu-id="d0242-114">模式</span><span class="sxs-lookup"><span data-stu-id="d0242-114">Patterns</span></span>

<span data-ttu-id="d0242-115">模式可用於*is_pattern*運算子、 *switch_statement*和*switch_expression*中，以表達要比較的傳入資料（我們所謂的輸入值）的資料圖形。</span><span class="sxs-lookup"><span data-stu-id="d0242-115">Patterns are used in the *is_pattern* operator, in a *switch_statement*, and in a *switch_expression* to express the shape of data against which incoming data  (which we call the input value) is to be compared.</span></span> <span data-ttu-id="d0242-116">模式可能是遞迴的，因此可能會比對部分的資料與子模式。</span><span class="sxs-lookup"><span data-stu-id="d0242-116">Patterns may be recursive so that parts of the data may be matched against sub-patterns.</span></span>

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

#### <a name="declaration-pattern"></a><span data-ttu-id="d0242-117">宣告模式</span><span class="sxs-lookup"><span data-stu-id="d0242-117">Declaration Pattern</span></span>

```antlr
declaration_pattern
    : type simple_designation
    ;
```

<span data-ttu-id="d0242-118">*Declaration_pattern*會測試運算式是否為指定的類型，並在測試成功時將它轉換成該類型。</span><span class="sxs-lookup"><span data-stu-id="d0242-118">The *declaration_pattern* both tests that an expression is of a given type and casts it to that type if the test succeeds.</span></span> <span data-ttu-id="d0242-119">如果指定是*single_variable_designation*，這可能會導入指定識別碼所命名之給定類型的本機變數。</span><span class="sxs-lookup"><span data-stu-id="d0242-119">This may introduce a local variable of the given type named by the given identifier, if the designation is a *single_variable_designation*.</span></span> <span data-ttu-id="d0242-120">當模式比對作業的結果是 `true`時，就會*明確地指派*該本機變數。</span><span class="sxs-lookup"><span data-stu-id="d0242-120">That local variable is *definitely assigned* when the result of the pattern-matching operation is `true`.</span></span>

<span data-ttu-id="d0242-121">這個運算式的執行時間語義是，它會針對模式中的*類型*，測試左側*relational_expression*運算元的執行時間類型。</span><span class="sxs-lookup"><span data-stu-id="d0242-121">The runtime semantic of this expression is that it tests the runtime type of the left-hand *relational_expression* operand against the *type* in the pattern.</span></span>  <span data-ttu-id="d0242-122">如果它是該執行時間類型（或某些子類型），而不是 `null`，則會 `true``is operator` 的結果。</span><span class="sxs-lookup"><span data-stu-id="d0242-122">If it is of that runtime type (or some subtype) and not `null`, the result of the `is operator` is `true`.</span></span>

<span data-ttu-id="d0242-123">左側和指定類型之靜態類型的某些組合會被視為不相容，並導致編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="d0242-123">Certain combinations of static type of the left-hand-side and the given type are considered incompatible and result in compile-time error.</span></span> <span data-ttu-id="d0242-124">如果存在識別轉換、隱含參考轉換、裝箱轉換、明確參考轉換或從 `E` 到 `T`的取消裝箱轉換，或如果其中一個類型是開啟的類型，則靜態類型 `E` 的值會被視為與類型 `T`*模式相容*。</span><span class="sxs-lookup"><span data-stu-id="d0242-124">A value of static type `E` is said to be *pattern-compatible* with a type `T` if there exists an identity conversion, an implicit reference conversion, a boxing conversion, an explicit reference conversion, or an unboxing conversion from `E` to `T`, or if one of those types is an open type.</span></span> <span data-ttu-id="d0242-125">如果 `E` 類型的輸入不是與它所比對之類型模式中的*類型* *模式相容*，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="d0242-125">It is a compile-time error if an input of type `E` is not *pattern-compatible* with the *type* in a type pattern that it is matched with.</span></span>

<span data-ttu-id="d0242-126">類型模式適用于執行參考型別的執行時間類型測試，並取代方法</span><span class="sxs-lookup"><span data-stu-id="d0242-126">The type pattern is useful for performing run-time type tests of reference types, and replaces the idiom</span></span>

```csharp
var v = expr as Type;
if (v != null) { // code using v
```

<span data-ttu-id="d0242-127">稍微變得更簡潔</span><span class="sxs-lookup"><span data-stu-id="d0242-127">With the slightly more concise</span></span>

```csharp
if (expr is Type v) { // code using v
```

<span data-ttu-id="d0242-128">如果*型*別是可為 null 的實值型別，就會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="d0242-128">It is an error if *type* is a nullable value type.</span></span>

<span data-ttu-id="d0242-129">類型模式可用來測試可為 null 之類型的值： `Nullable<T>` 類型的值（或已裝箱的 `T`）符合類型模式 `T2 id` 如果值為非 null，且 `T2` 的類型為 `T`，或 `T`的部分基底類型或介面。</span><span class="sxs-lookup"><span data-stu-id="d0242-129">The type pattern can be used to test values of nullable types: a value of type `Nullable<T>` (or a boxed `T`) matches a type pattern `T2 id` if the value is non-null and the type of `T2` is `T`, or some base type or interface of `T`.</span></span> <span data-ttu-id="d0242-130">例如，在程式碼片段中</span><span class="sxs-lookup"><span data-stu-id="d0242-130">For example, in the code fragment</span></span>

```csharp
int? x = 3;
if (x is int v) { // code using v
```

<span data-ttu-id="d0242-131">`if` 語句的條件會在執行時間 `true`，而變數 `v` 會保存區塊內類型 `int` 的值 `3`。</span><span class="sxs-lookup"><span data-stu-id="d0242-131">The condition of the `if` statement is `true` at runtime and the variable `v` holds the value `3` of type `int` inside the block.</span></span> <span data-ttu-id="d0242-132">在區塊之後，變數 `v` 會在範圍內，但不一定會指派。</span><span class="sxs-lookup"><span data-stu-id="d0242-132">After the block the variable `v` is in scope but not definitely assigned.</span></span>

#### <a name="constant-pattern"></a><span data-ttu-id="d0242-133">常數模式</span><span class="sxs-lookup"><span data-stu-id="d0242-133">Constant Pattern</span></span>

```antlr
constant_pattern
    : constant_expression
    ;
```

<span data-ttu-id="d0242-134">常數模式會針對常數值測試運算式的值。</span><span class="sxs-lookup"><span data-stu-id="d0242-134">A constant pattern tests the value of an expression against a constant value.</span></span> <span data-ttu-id="d0242-135">常數可以是任何常數運算式，例如常值、宣告 `const` 變數的名稱或列舉常數。</span><span class="sxs-lookup"><span data-stu-id="d0242-135">The constant may be any constant expression, such as a literal, the name of a declared `const` variable, or an enumeration constant.</span></span> <span data-ttu-id="d0242-136">當輸入值不是開放式型別時，常數運算式會隱含地轉換成相符運算式的型別。如果輸入值的類型與常數運算式的類型不是*模式相容*，則模式比對作業會是錯誤。</span><span class="sxs-lookup"><span data-stu-id="d0242-136">When the input value is not an open type, the constant expression is implicitly converted to the type of the matched expression; if the type of the input value is not *pattern-compatible* with the type of the constant expression, the pattern-matching operation is an error.</span></span>

<span data-ttu-id="d0242-137">如果 `object.Equals(c, e)` 會傳回 `true`，模式*c*會視為符合轉換後的輸入值*e* 。</span><span class="sxs-lookup"><span data-stu-id="d0242-137">The pattern *c* is considered matching the converted input value *e* if `object.Equals(c, e)` would return `true`.</span></span>

<span data-ttu-id="d0242-138">我們預期在新撰寫的程式碼中測試 `null` 最常見的方式 `e is null`，因為它無法叫用使用者定義的 `operator==`。</span><span class="sxs-lookup"><span data-stu-id="d0242-138">We expect to see `e is null` as the most common way to test for `null` in newly written code, as it cannot invoke a user-defined `operator==`.</span></span>

#### <a name="var-pattern"></a><span data-ttu-id="d0242-139">Var 模式</span><span class="sxs-lookup"><span data-stu-id="d0242-139">Var Pattern</span></span>

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

<span data-ttu-id="d0242-140">如果*指定*為*simple_designation*，則運算式*e*會符合模式。</span><span class="sxs-lookup"><span data-stu-id="d0242-140">If the *designation* is a *simple_designation*, an expression *e* matches the pattern.</span></span> <span data-ttu-id="d0242-141">換句話說，與*var 模式*的比對一律會以*simple_designation*成功。</span><span class="sxs-lookup"><span data-stu-id="d0242-141">In other words, a match to a *var pattern* always succeeds with a *simple_designation*.</span></span> <span data-ttu-id="d0242-142">如果*simple_designation*是*single_variable_designation*，則*e*的值會系結至新引進的本機變數。</span><span class="sxs-lookup"><span data-stu-id="d0242-142">If the *simple_designation* is a *single_variable_designation*, the value of *e* is bounds to a newly introduced local variable.</span></span> <span data-ttu-id="d0242-143">本機變數的類型是*e*的靜態類型。</span><span class="sxs-lookup"><span data-stu-id="d0242-143">The type of the local variable is the static type of *e*.</span></span>

<span data-ttu-id="d0242-144">如果*指定*為*tuple_designation*，則模式相當於格式為 `(var`*指定*... `)` 的*positional_pattern* ，其中*指定*的是在*tuple_designation*中找到的。</span><span class="sxs-lookup"><span data-stu-id="d0242-144">If the *designation* is a *tuple_designation*, then the pattern is equivalent to a *positional_pattern* of the form `(var` *designation*, ... `)` where the *designation*s are those found within the *tuple_designation*.</span></span>  <span data-ttu-id="d0242-145">例如，模式 `var (x, (y, z))` 相當於 `(var x, (var y, var z))`。</span><span class="sxs-lookup"><span data-stu-id="d0242-145">For example, the pattern `var (x, (y, z))` is equivalent to `(var x, (var y, var z))`.</span></span>

<span data-ttu-id="d0242-146">如果名稱 `var` 系結至類型，就會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="d0242-146">It is an error if the name `var` binds to a type.</span></span>

#### <a name="discard-pattern"></a><span data-ttu-id="d0242-147">捨棄模式</span><span class="sxs-lookup"><span data-stu-id="d0242-147">Discard Pattern</span></span>

```antlr
discard_pattern
    : '_'
    ;
```

<span data-ttu-id="d0242-148">運算式*e*符合模式 `_` always。</span><span class="sxs-lookup"><span data-stu-id="d0242-148">An expression *e* matches the pattern `_` always.</span></span> <span data-ttu-id="d0242-149">換句話說，每個運算式都符合捨棄模式。</span><span class="sxs-lookup"><span data-stu-id="d0242-149">In other words, every expression matches the discard pattern.</span></span>

<span data-ttu-id="d0242-150">捨棄模式不得當做*is_pattern_expression*的模式使用。</span><span class="sxs-lookup"><span data-stu-id="d0242-150">A discard pattern may not be used as the pattern of an *is_pattern_expression*.</span></span>

#### <a name="positional-pattern"></a><span data-ttu-id="d0242-151">位置模式</span><span class="sxs-lookup"><span data-stu-id="d0242-151">Positional Pattern</span></span>

<span data-ttu-id="d0242-152">位置模式會檢查輸入值是否不 `null`、叫用適當的 `Deconstruct` 方法，並對產生的值執行進一步的模式比對。</span><span class="sxs-lookup"><span data-stu-id="d0242-152">A positional pattern checks that the input value is not `null`, invokes an appropriate `Deconstruct` method, and performs further pattern matching on the resulting values.</span></span>  <span data-ttu-id="d0242-153">當輸入值的型別與包含 `Deconstruct`的型別相同，或者輸入值的型別是元組型別，或者輸入值的型別是 `object` 或 `ITuple`，而且運算式的執行時間型別會執行 `ITuple`時，它也支援類似元組的模式語法（不需要提供型別）。</span><span class="sxs-lookup"><span data-stu-id="d0242-153">It also supports a tuple-like pattern syntax (without the type being provided) when the type of the input value is the same as the type containing `Deconstruct`, or if the type of the input value is a tuple type, or if the type of the input value is `object` or `ITuple` and the runtime type of the expression implements `ITuple`.</span></span>

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

<span data-ttu-id="d0242-154">如果省略*類型*，我們會將它視為輸入值的靜態類型。</span><span class="sxs-lookup"><span data-stu-id="d0242-154">If the *type* is omitted, we take it to be the static type of the input value.</span></span>

<span data-ttu-id="d0242-155">假設符合輸入值與模式*類型*`(` *subpattern_list* `)`，則會在*類型*中搜尋 `Deconstruct` 的可存取宣告，並使用解構宣告的相同規則來選取其中一個方法，藉以選取該方法。</span><span class="sxs-lookup"><span data-stu-id="d0242-155">Given a match of an input value to the pattern *type* `(` *subpattern_list* `)`, a method is selected by searching in *type* for accessible declarations of `Deconstruct` and selecting one among them using the same rules as for the deconstruction declaration.</span></span>

<span data-ttu-id="d0242-156">如果*positional_pattern*省略型別、具有沒有*識別碼*的單一子*模式*、沒有任何*property_subpattern* ，而且沒有*simple_designation*，就會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="d0242-156">It is an error if a *positional_pattern* omits the type, has a single *subpattern* without an *identifier*, has no *property_subpattern* and has no *simple_designation*.</span></span> <span data-ttu-id="d0242-157">這會在以括弧括住的*constant_pattern*和*positional_pattern*之間進行厘清。</span><span class="sxs-lookup"><span data-stu-id="d0242-157">This disambiguates between a *constant_pattern* that is parenthesized and a *positional_pattern*.</span></span>

<span data-ttu-id="d0242-158">為了將值解壓縮以符合清單中的模式，</span><span class="sxs-lookup"><span data-stu-id="d0242-158">In order to extract the values to match against the patterns in the list,</span></span>
- <span data-ttu-id="d0242-159">如果省略了*type* ，而輸入值的類型是元組類型，則 subpatterns 的數目必須與元組的基數相同。</span><span class="sxs-lookup"><span data-stu-id="d0242-159">If *type* was omitted and the input value's type is a tuple type, then the number of subpatterns is required to be the same as the cardinality of the tuple.</span></span> <span data-ttu-id="d0242-160">每個元組元素會與對應的子*模式*進行比對，如果所有這些專案都成功，則比對成功。</span><span class="sxs-lookup"><span data-stu-id="d0242-160">Each tuple element is matched against the corresponding *subpattern*, and the match succeeds if all of these succeed.</span></span> <span data-ttu-id="d0242-161">如果有任何子*模式*具有*識別碼*，則必須在元組類型中的對應位置上命名元組元素。</span><span class="sxs-lookup"><span data-stu-id="d0242-161">If any *subpattern* has an *identifier*, then that must name a tuple element at the corresponding position in the tuple type.</span></span>
- <span data-ttu-id="d0242-162">否則，如果適當的 `Deconstruct` 當做*類型*的成員存在，則如果輸入值的類型與*類型*不*相容*，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="d0242-162">Otherwise, if a suitable `Deconstruct` exists as a member of *type*, it is a compile-time error if the type of the input value is not *pattern-compatible* with *type*.</span></span> <span data-ttu-id="d0242-163">在執行時間，會針對*類型*測試輸入值。</span><span class="sxs-lookup"><span data-stu-id="d0242-163">At runtime the input value is tested against *type*.</span></span> <span data-ttu-id="d0242-164">如果此作業失敗，則位置模式比對會失敗。</span><span class="sxs-lookup"><span data-stu-id="d0242-164">If this fails then the positional pattern match fails.</span></span> <span data-ttu-id="d0242-165">如果成功，則會將輸入值轉換成這個類型，並使用全新的編譯器產生的變數來叫用 `Deconstruct`，以接收 `out` 參數。</span><span class="sxs-lookup"><span data-stu-id="d0242-165">If it succeeds,  the input value is converted to this type and `Deconstruct` is invoked with fresh compiler-generated variables to receive the `out` parameters.</span></span> <span data-ttu-id="d0242-166">每個收到的值都會針對對應的子*模式*進行比對，如果全部都成功，則比對成功。</span><span class="sxs-lookup"><span data-stu-id="d0242-166">Each value that was received is matched against the corresponding *subpattern*, and the match succeeds if all of these succeed.</span></span> <span data-ttu-id="d0242-167">如果有任何子*模式*具有*識別碼*，則必須在 `Deconstruct`的對應位置上具名引數。</span><span class="sxs-lookup"><span data-stu-id="d0242-167">If any *subpattern* has an *identifier*, then that must name a parameter at the corresponding position of `Deconstruct`.</span></span>
- <span data-ttu-id="d0242-168">否則，如果省略了*type* ，而輸入值的類型為 `object` 或 `ITuple` 或可轉換成隱含參考轉換 `ITuple` 的某種類型，則*我們會使用*`ITuple`進行比對。</span><span class="sxs-lookup"><span data-stu-id="d0242-168">Otherwise if *type* was omitted, and the input value is of type `object` or `ITuple` or some type that can be converted to `ITuple` by an implicit reference conversion, and no *identifier* appears among the subpatterns, then we match using `ITuple`.</span></span>
- <span data-ttu-id="d0242-169">否則，模式為編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="d0242-169">Otherwise the pattern is a compile-time error.</span></span>

<span data-ttu-id="d0242-170">Subpatterns 在執行時間中的相符順序未指定，而且失敗的比對可能不會嘗試比對所有 subpatterns。</span><span class="sxs-lookup"><span data-stu-id="d0242-170">The order in which subpatterns are matched at runtime is unspecified, and a failed match may not attempt to match all subpatterns.</span></span>

##### <a name="example"></a><span data-ttu-id="d0242-171">範例</span><span class="sxs-lookup"><span data-stu-id="d0242-171">Example</span></span>

<span data-ttu-id="d0242-172">這個範例會使用此規格中所述的許多功能</span><span class="sxs-lookup"><span data-stu-id="d0242-172">This example uses many of the features described in this specification</span></span>

``` c#
    var newState = (GetState(), action, hasKey) switch {
        (DoorState.Closed, Action.Open, _) => DoorState.Opened,
        (DoorState.Opened, Action.Close, _) => DoorState.Closed,
        (DoorState.Closed, Action.Lock, true) => DoorState.Locked,
        (DoorState.Locked, Action.Unlock, true) => DoorState.Closed,
        (var state, _, _) => state };
```

#### <a name="property-pattern"></a><span data-ttu-id="d0242-173">屬性模式</span><span class="sxs-lookup"><span data-stu-id="d0242-173">Property Pattern</span></span>

<span data-ttu-id="d0242-174">屬性模式會檢查輸入值是否不 `null`，並以遞迴方式符合使用可存取屬性或欄位所解壓縮的值。</span><span class="sxs-lookup"><span data-stu-id="d0242-174">A property pattern checks that the input value is not `null` and recursively matches values extracted by the use of accessible properties or fields.</span></span>

```antlr
property_pattern
    : type? property_subpattern simple_designation?
    ;
property_subpattern
    : '{' '}'
    | '{' subpatterns ','? '}'
    ;
```

<span data-ttu-id="d0242-175">如果_property_pattern_的任何子_模式_不包含_識別碼_（必須是具有_識別碼_的第二個格式），就會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="d0242-175">It is an error if any _subpattern_ of a _property_pattern_ does not contain an _identifier_ (it must be of the second form, which has an _identifier_).</span></span>  <span data-ttu-id="d0242-176">最後一個子模式後面的尾端逗號是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="d0242-176">A trailing comma after the last subpattern is optional.</span></span>

<span data-ttu-id="d0242-177">請注意，null 檢查模式不屬於簡單的屬性模式。</span><span class="sxs-lookup"><span data-stu-id="d0242-177">Note that a null-checking pattern falls out of a trivial property pattern.</span></span> <span data-ttu-id="d0242-178">若要檢查字串 `s` 是否為非 null，您可以撰寫下列任何一種形式</span><span class="sxs-lookup"><span data-stu-id="d0242-178">To check if the string `s` is non-null, you can write any of the following forms</span></span>

```csharp
if (s is object o) ... // o is of type object
if (s is string x) ... // x is of type string
if (s is {} x) ... // x is of type string
if (s is {}) ...
```

<span data-ttu-id="d0242-179">假設符合運算式*e*與模式*類型*`{` *property_pattern_list* `}`，如果運算式*e*與*類型*指定的類型*t*不*相容*，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="d0242-179">Given a match of an expression *e* to the pattern *type* `{` *property_pattern_list* `}`, it is a compile-time error if the expression *e* is not *pattern-compatible* with the type *T* designated by *type*.</span></span> <span data-ttu-id="d0242-180">如果類型不存在，我們會將它視為靜態類型*e*。</span><span class="sxs-lookup"><span data-stu-id="d0242-180">If the type is absent, we take it to be the static type of *e*.</span></span> <span data-ttu-id="d0242-181">如果*識別碼*存在，它會宣告類型*類型*的模式變數。</span><span class="sxs-lookup"><span data-stu-id="d0242-181">If the *identifier* is present, it declares a pattern variable of type *type*.</span></span> <span data-ttu-id="d0242-182">出現在其*property_pattern_list*左邊的每個識別碼，都必須指定可存取的可讀取屬性或*T*的欄位。如果*property_pattern*的*simple_designation*存在，它會定義*T*類型的模式變數。</span><span class="sxs-lookup"><span data-stu-id="d0242-182">Each of the identifiers appearing on the left-hand-side of its *property_pattern_list* must designate an accessible readable property or field of *T*. If the *simple_designation* of the *property_pattern* is present, it defines a pattern variable of type *T*.</span></span>

<span data-ttu-id="d0242-183">在執行時間，運算式會針對*T*進行測試。如果這項作業失敗，則屬性模式比對會失敗，且結果會是 `false`。</span><span class="sxs-lookup"><span data-stu-id="d0242-183">At runtime, the expression is tested against *T*. If this fails then the property pattern match fails and the result is `false`.</span></span> <span data-ttu-id="d0242-184">如果成功，則會讀取每個*property_subpattern*欄位或屬性，並將其值與對應的模式進行比對。</span><span class="sxs-lookup"><span data-stu-id="d0242-184">If it succeeds, then each *property_subpattern* field or property is read and its value matched against its corresponding pattern.</span></span> <span data-ttu-id="d0242-185">只有在其中任一項的結果都 `false`時，才會 `false` 整個比對的結果。</span><span class="sxs-lookup"><span data-stu-id="d0242-185">The result of the whole match is `false` only if the result of any of these is `false`.</span></span> <span data-ttu-id="d0242-186">未指定 subpatterns 的相符順序，而且失敗的比對可能不符合執行時間的所有 subpatterns。</span><span class="sxs-lookup"><span data-stu-id="d0242-186">The order in which subpatterns are matched is not specified, and a failed match may not match all subpatterns at runtime.</span></span> <span data-ttu-id="d0242-187">如果比對成功，且*property_pattern*的*simple_designation*是*single_variable_designation*，它會定義指派了相符值的*T*類型變數。</span><span class="sxs-lookup"><span data-stu-id="d0242-187">If the match succeeds and the *simple_designation* of the *property_pattern* is a *single_variable_designation*, it defines a variable of type *T* that is assigned the matched value.</span></span>

> <span data-ttu-id="d0242-188">注意：屬性模式可以用來搭配匿名型別進行模式比對。</span><span class="sxs-lookup"><span data-stu-id="d0242-188">Note: The property pattern can be used to pattern-match with anonymous types.</span></span>

##### <a name="example"></a><span data-ttu-id="d0242-189">範例</span><span class="sxs-lookup"><span data-stu-id="d0242-189">Example</span></span>

```csharp
if (o is string { Length: 5 } s)
```

### <a name="switch-expression"></a><span data-ttu-id="d0242-190">Switch 運算式</span><span class="sxs-lookup"><span data-stu-id="d0242-190">Switch Expression</span></span>

<span data-ttu-id="d0242-191">加入*switch_expression* ，以支援運算式內容 `switch`like 的語義。</span><span class="sxs-lookup"><span data-stu-id="d0242-191">A *switch_expression* is added to support `switch`-like semantics for an expression context.</span></span>

<span data-ttu-id="d0242-192">C#語言語法會隨著下列語法生產而增強：</span><span class="sxs-lookup"><span data-stu-id="d0242-192">The C# language syntax is augmented with the following syntactic productions:</span></span>

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

<span data-ttu-id="d0242-193">不允許*switch_expression*做為*expression_statement*。</span><span class="sxs-lookup"><span data-stu-id="d0242-193">The *switch_expression* is not permitted as an *expression_statement*.</span></span>

> <span data-ttu-id="d0242-194">我們想要在未來的修訂中放寬這項功能。</span><span class="sxs-lookup"><span data-stu-id="d0242-194">We are looking at relaxing this in a future revision.</span></span>

<span data-ttu-id="d0242-195">如果這類類型存在，而且 switch 運算式的每個 arm 中的運算式可以隱含地轉換成該類型，則*switch_expression*的類型就是出現在*switch_expression_arm*之 `=>` token 右邊的[*最常見運算式類型*](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions)。</span><span class="sxs-lookup"><span data-stu-id="d0242-195">The type of the *switch_expression* is the [*best common type*](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) of the expressions appearing to the right of the `=>` tokens of the *switch_expression_arm*s if such a type exists and the expression in every arm of the switch expression can be implicitly converted to that type.</span></span>  <span data-ttu-id="d0242-196">此外，我們新增了新的*切換運算式轉換*，這是從 switch 運算式到每個類型的預先定義隱含轉換，`T` 從每個 arm 的運算式隱含轉換到 `T`。</span><span class="sxs-lookup"><span data-stu-id="d0242-196">In addition, we add a new *switch expression conversion*, which is a predefined implicit conversion from a switch expression to every type `T` for which there exists an implicit conversion from each arm's expression to `T`.</span></span>

<span data-ttu-id="d0242-197">如果部分*switch_expression_arm*的模式無法影響結果，就會發生錯誤，因為先前的模式和防護一定會相符。</span><span class="sxs-lookup"><span data-stu-id="d0242-197">It is an error if some *switch_expression_arm*'s pattern cannot affect the result because some previous pattern and guard will always match.</span></span>

<span data-ttu-id="d0242-198">如果 switch 運算式的某些 arm 會處理其輸入的每個值，則 switch 運算式會被視為*完整*的。</span><span class="sxs-lookup"><span data-stu-id="d0242-198">A switch expression is said to be *exhaustive* if some arm of the switch expression handles every value of its input.</span></span>  <span data-ttu-id="d0242-199">如果 switch 運算式不是*完整*的，編譯器應該會產生警告。</span><span class="sxs-lookup"><span data-stu-id="d0242-199">The compiler shall produce a warning if a switch expression is not *exhaustive*.</span></span>

<span data-ttu-id="d0242-200">在執行時間， *switch_expression*的結果是第一個*switch_expression_arm* *switch_expression*左邊的運算式與*switch_expression_arm*的模式相符之*運算式*的值，而*case_guard* *的 switch_expression_arm* （如果有的話）則會評估為 [`true`]。</span><span class="sxs-lookup"><span data-stu-id="d0242-200">At runtime, the result of the *switch_expression* is the value of the *expression* of the first *switch_expression_arm* for which the expression on the left-hand-side of the *switch_expression* matches the *switch_expression_arm*'s pattern, and for which the *case_guard* of the *switch_expression_arm*, if present, evaluates to `true`.</span></span> <span data-ttu-id="d0242-201">如果沒有這類*switch_expression_arm*， *switch_expression*就會擲回例外狀況 `System.Runtime.CompilerServices.SwitchExpressionException`的實例。</span><span class="sxs-lookup"><span data-stu-id="d0242-201">If there is no such *switch_expression_arm*, the *switch_expression* throws an instance of the exception `System.Runtime.CompilerServices.SwitchExpressionException`.</span></span>

### <a name="optional-parens-when-switching-on-a-tuple-literal"></a><span data-ttu-id="d0242-202">在元組常值上切換時的選擇性括弧</span><span class="sxs-lookup"><span data-stu-id="d0242-202">Optional parens when switching on a tuple literal</span></span>

<span data-ttu-id="d0242-203">若要使用*switch_statement*來切換元組常值，您必須撰寫看似重複的括弧</span><span class="sxs-lookup"><span data-stu-id="d0242-203">In order to switch on a tuple literal using the *switch_statement*, you have to write what appear to be redundant parens</span></span>

```csharp
switch ((a, b))
{
```

<span data-ttu-id="d0242-204">允許</span><span class="sxs-lookup"><span data-stu-id="d0242-204">To permit</span></span>

```csharp
switch (a, b)
{
```

<span data-ttu-id="d0242-205">當正在切換的運算式是元組常值時，switch 語句的括弧是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="d0242-205">the parentheses of the switch statement are optional when the expression being switched on is a tuple literal.</span></span>

### <a name="order-of-evaluation-in-pattern-matching"></a><span data-ttu-id="d0242-206">模式比對中的評估順序</span><span class="sxs-lookup"><span data-stu-id="d0242-206">Order of evaluation in pattern-matching</span></span>

<span data-ttu-id="d0242-207">提供編譯器彈性來重新排列模式比對期間所執行的作業，可允許用來改善模式比對效率的彈性。</span><span class="sxs-lookup"><span data-stu-id="d0242-207">Giving the compiler flexibility in reordering the operations executed during pattern-matching can permit flexibility that can be used to improve the efficiency of pattern-matching.</span></span> <span data-ttu-id="d0242-208">（未強制的）需求是在模式中存取的屬性和解構方法都必須是「純」（無副作用、等冪等）。</span><span class="sxs-lookup"><span data-stu-id="d0242-208">The (unenforced) requirement would be that properties accessed in a pattern, and the Deconstruct methods, are required to be "pure" (side-effect free, idempotent, etc).</span></span> <span data-ttu-id="d0242-209">這並不表示我們會將純度新增為語言概念，只是為了讓編譯器能夠彈性地重新排列作業。</span><span class="sxs-lookup"><span data-stu-id="d0242-209">That doesn't mean that we would add purity as a language concept, only that we would allow the compiler flexibility in reordering operations.</span></span>

<span data-ttu-id="d0242-210">**解決方式 2018-04-04 LDM**：已確認：允許編譯器重新排序 `ITuple`中的 `Deconstruct`、屬性存取和調用的呼叫，而且可能會假設傳回的值與多個呼叫相同。</span><span class="sxs-lookup"><span data-stu-id="d0242-210">**Resolution 2018-04-04 LDM**: confirmed: the compiler is permitted to reorder calls to `Deconstruct`, property accesses, and invocations of methods in `ITuple`, and may assume that returned values are the same from multiple calls.</span></span> <span data-ttu-id="d0242-211">編譯器不應叫用不會影響結果的函式，因此，在未來對編譯器所產生的評估順序進行任何變更之前，我們會非常謹慎。</span><span class="sxs-lookup"><span data-stu-id="d0242-211">The compiler should not invoke functions that cannot affect the result, and we will be very careful before making any changes to the compiler-generated order of evaluation in the future.</span></span>

### <a name="some-possible-optimizations"></a><span data-ttu-id="d0242-212">一些可能的優化</span><span class="sxs-lookup"><span data-stu-id="d0242-212">Some Possible Optimizations</span></span>

<span data-ttu-id="d0242-213">模式比對的編譯可以利用模式的通用部分。</span><span class="sxs-lookup"><span data-stu-id="d0242-213">The compilation of pattern matching can take advantage of common parts of patterns.</span></span> <span data-ttu-id="d0242-214">例如，如果*switch_statement*中的兩個連續模式的頂層型別測試是相同的型別，則產生的程式碼可以略過第二個模式的型別測試。</span><span class="sxs-lookup"><span data-stu-id="d0242-214">For example, if the top-level type test of two successive patterns in a *switch_statement* is the same type, the generated code can skip the type test for the second pattern.</span></span>

<span data-ttu-id="d0242-215">當某些模式為整數或字串時，編譯器會產生它針對舊版語言中的 switch 語句所產生的相同類型程式碼。</span><span class="sxs-lookup"><span data-stu-id="d0242-215">When some of the patterns are integers or strings, the compiler can generate the same kind of code it generates for a switch-statement in earlier versions of the language.</span></span>

<span data-ttu-id="d0242-216">如需這些優化類型的詳細資訊，請參閱[[Scott And Ramsey （2000）]](https://www.cs.tufts.edu/~nr/cs257/archive/norman-ramsey/match.pdf "符合編譯啟發學習法的時機為何？")。</span><span class="sxs-lookup"><span data-stu-id="d0242-216">For more on these kinds of optimizations, see [[Scott and Ramsey (2000)]](https://www.cs.tufts.edu/~nr/cs257/archive/norman-ramsey/match.pdf "When Do Match-Compilation Heuristics Matter?").</span></span>
