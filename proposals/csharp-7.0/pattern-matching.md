---
ms.openlocfilehash: 3df21c5816be90387a6cd9242e99ba11f43dfd1c
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/14/2020
ms.locfileid: "79485165"
---
# <a name="pattern-matching-for-c-7"></a><span data-ttu-id="33436-101">7的模式C#比對</span><span class="sxs-lookup"><span data-stu-id="33436-101">Pattern Matching for C# 7</span></span>

<span data-ttu-id="33436-102">模式比對C#延伸模組可從功能性語言啟用許多代數資料類型和模式比對的優點，並以順暢地與基礎語言的風格整合的方式。</span><span class="sxs-lookup"><span data-stu-id="33436-102">Pattern matching extensions for C# enable many of the benefits of algebraic data types and pattern matching from functional languages, but in a way that smoothly integrates with the feel of the underlying language.</span></span> <span data-ttu-id="33436-103">基本功能包括：[記錄類型](https://github.com/dotnet/csharplang/blob/master/proposals/records.md)，這些類型的語義意義會由資料的圖形描述;和模式比對，這是新的運算式表單，可啟用這些資料類型非常簡潔的多層分解。</span><span class="sxs-lookup"><span data-stu-id="33436-103">The basic features are: [record types](https://github.com/dotnet/csharplang/blob/master/proposals/records.md), which are types whose semantic meaning is described by the shape of the data; and pattern matching, which is a new expression form that enables extremely concise multilevel decomposition of these data types.</span></span> <span data-ttu-id="33436-104">這種方法的元素是由程式設計語言[F#](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/p29-syme.pdf "透過輕量語言的可擴充模式比對")和[Scala](https://infoscience.epfl.ch/record/98468/files/MatchingObjectsWithPatterns-TR.pdf "搭配模式來比對物件")中的相關功能所靈感。</span><span class="sxs-lookup"><span data-stu-id="33436-104">Elements of this approach are inspired by related features in the programming languages [F#](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/p29-syme.pdf "Extensible Pattern Matching Via a Lightweight Language") and [Scala](https://infoscience.epfl.ch/record/98468/files/MatchingObjectsWithPatterns-TR.pdf "Matching Objects With Patterns").</span></span>

## <a name="is-expression"></a><span data-ttu-id="33436-105">Is 運算式</span><span class="sxs-lookup"><span data-stu-id="33436-105">Is expression</span></span>

<span data-ttu-id="33436-106">`is` 運算子已擴充，可針對*模式*測試運算式。</span><span class="sxs-lookup"><span data-stu-id="33436-106">The `is` operator is extended to test an expression against a *pattern*.</span></span>

```antlr
relational_expression
    : relational_expression 'is' pattern
    ;
```

<span data-ttu-id="33436-107">這種形式的*relational_expression*除了C#規格中的現有表單之外。</span><span class="sxs-lookup"><span data-stu-id="33436-107">This form of *relational_expression* is in addition to the existing forms in the C# specification.</span></span> <span data-ttu-id="33436-108">如果 `is` token 左邊的*relational_expression*未指定值或沒有類型，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="33436-108">It is a compile-time error if the *relational_expression* to the left of the `is` token does not designate a value or does not have a type.</span></span>

<span data-ttu-id="33436-109">模式的每個*識別碼*都會導入一個新的區域變數，在 `is` 運算子 `true` （也就是*明確指派的值*）之後，*一定會指派*此變數。</span><span class="sxs-lookup"><span data-stu-id="33436-109">Every *identifier* of the pattern introduces a new local variable that is *definitely assigned* after the `is` operator is `true` (i.e. *definitely assigned when true*).</span></span>

> <span data-ttu-id="33436-110">注意：在技術上，`is-expression` 和*constant_pattern*的*類型*之間有一個不明確的，這可能是限定識別碼的有效剖析。</span><span class="sxs-lookup"><span data-stu-id="33436-110">Note: There is technically an ambiguity between *type* in an `is-expression` and *constant_pattern*, either of which might be a valid parse of a qualified identifier.</span></span> <span data-ttu-id="33436-111">我們嘗試將它系結為類型，以與舊版語言相容;只有當該作業失敗時，我們才會將它解析為我們在其他內容中找到的第一件事（必須是常數或型別）。</span><span class="sxs-lookup"><span data-stu-id="33436-111">We try to bind it as a type for compatibility with previous versions of the language; only if that fails do we resolve it as we do in other contexts, to the first thing found (which must be either a constant or a type).</span></span> <span data-ttu-id="33436-112">這種不明確的情況只會出現在 `is` 運算式的右手邊。</span><span class="sxs-lookup"><span data-stu-id="33436-112">This ambiguity is only present on the right-hand-side of an `is` expression.</span></span>

## <a name="patterns"></a><span data-ttu-id="33436-113">模式</span><span class="sxs-lookup"><span data-stu-id="33436-113">Patterns</span></span>

<span data-ttu-id="33436-114">模式會用於 `is` 運算子和*switch_statement*中，以表達要比較傳入資料的資料圖形。</span><span class="sxs-lookup"><span data-stu-id="33436-114">Patterns are used in the `is` operator and in a *switch_statement* to express the shape of data against which incoming data is to be compared.</span></span> <span data-ttu-id="33436-115">模式可能是遞迴的，因此可能會比對部分的資料與子模式。</span><span class="sxs-lookup"><span data-stu-id="33436-115">Patterns may be recursive so that parts of the data may be matched against sub-patterns.</span></span>

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

> <span data-ttu-id="33436-116">注意：在技術上，`is-expression` 和*constant_pattern*的*類型*之間有一個不明確的，這可能是限定識別碼的有效剖析。</span><span class="sxs-lookup"><span data-stu-id="33436-116">Note: There is technically an ambiguity between *type* in an `is-expression` and *constant_pattern*, either of which might be a valid parse of a qualified identifier.</span></span> <span data-ttu-id="33436-117">我們嘗試將它系結為類型，以與舊版語言相容;只有當該作業失敗時，我們才會將它解析為我們在其他內容中找到的第一件事（必須是常數或型別）。</span><span class="sxs-lookup"><span data-stu-id="33436-117">We try to bind it as a type for compatibility with previous versions of the language; only if that fails do we resolve it as we do in other contexts, to the first thing found (which must be either a constant or a type).</span></span> <span data-ttu-id="33436-118">這種不明確的情況只會出現在 `is` 運算式的右手邊。</span><span class="sxs-lookup"><span data-stu-id="33436-118">This ambiguity is only present on the right-hand-side of an `is` expression.</span></span>

### <a name="declaration-pattern"></a><span data-ttu-id="33436-119">宣告模式</span><span class="sxs-lookup"><span data-stu-id="33436-119">Declaration pattern</span></span>

<span data-ttu-id="33436-120">*Declaration_pattern*會測試運算式是否為指定的類型，並在測試成功時將它轉換成該類型。</span><span class="sxs-lookup"><span data-stu-id="33436-120">The *declaration_pattern* both tests that an expression is of a given type and casts it to that type if the test succeeds.</span></span> <span data-ttu-id="33436-121">如果*simple_designation*是識別碼，則會引進給定識別碼所命名之給定類型的本機變數。</span><span class="sxs-lookup"><span data-stu-id="33436-121">If the *simple_designation* is an identifier, it introduces a local variable of the given type named by the given identifier.</span></span> <span data-ttu-id="33436-122">當模式比對作業的結果為 true 時，就會*明確地指派*該本機變數。</span><span class="sxs-lookup"><span data-stu-id="33436-122">That local variable is *definitely assigned* when the result of the pattern-matching operation is true.</span></span>

```antlr
declaration_pattern
    : type simple_designation
    ;
```

<span data-ttu-id="33436-123">這個運算式的執行時間語義是，它會針對模式中的*類型*，測試左側*relational_expression*運算元的執行時間類型。</span><span class="sxs-lookup"><span data-stu-id="33436-123">The runtime semantic of this expression is that it tests the runtime type of the left-hand *relational_expression* operand against the *type* in the pattern.</span></span> <span data-ttu-id="33436-124">如果它是該執行時間類型（或某些子類型），則會 `true``is operator` 的結果。</span><span class="sxs-lookup"><span data-stu-id="33436-124">If it is of that runtime type (or some subtype), the result of the `is operator` is `true`.</span></span> <span data-ttu-id="33436-125">它會宣告一個名為的新區域變數，該*識別碼*會在 `true`結果時指派左邊運算元的值。</span><span class="sxs-lookup"><span data-stu-id="33436-125">It declares a new local variable named by the *identifier* that is assigned the value of the left-hand operand when the result is `true`.</span></span>

<span data-ttu-id="33436-126">左側和指定類型之靜態類型的某些組合會被視為不相容，並導致編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="33436-126">Certain combinations of static type of the left-hand-side and the given type are considered incompatible and result in compile-time error.</span></span> <span data-ttu-id="33436-127">如果存在識別轉換、隱含的參考轉換、裝箱轉換、明確參考轉換，或從 `E` 到 `T`的取消裝箱轉換，則靜態類型 `E` 的值會被視為與類型 `T` 的*模式相容*。</span><span class="sxs-lookup"><span data-stu-id="33436-127">A value of static type `E` is said to be *pattern compatible* with the type `T` if there exists an identity conversion, an implicit reference conversion, a boxing conversion, an explicit reference conversion, or an unboxing conversion from `E` to `T`.</span></span> <span data-ttu-id="33436-128">如果 `E` 類型的運算式在與其相符的類型模式中的類型模式不相容，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="33436-128">It is a compile-time error if an expression of type `E` is not pattern compatible with the type in a type pattern that it is matched with.</span></span>

> <span data-ttu-id="33436-129">注意：[在C# 7.1 中，我們會將此延伸](../csharp-7.1/generics-pattern-match.md)，以允許模式比對作業（如果輸入類型或類型 `T` 是開啟的類型）。</span><span class="sxs-lookup"><span data-stu-id="33436-129">Note: [In C# 7.1 we extend this](../csharp-7.1/generics-pattern-match.md) to permit a pattern-matching operation if either the input type or the type `T` is an open type.</span></span> <span data-ttu-id="33436-130">此段落會取代為下列內容：</span><span class="sxs-lookup"><span data-stu-id="33436-130">This paragraph is replaced by the following:</span></span>
> 
> <span data-ttu-id="33436-131">左側和指定類型之靜態類型的某些組合會被視為不相容，並導致編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="33436-131">Certain combinations of static type of the left-hand-side and the given type are considered incompatible and result in compile-time error.</span></span> <span data-ttu-id="33436-132">如果存在身分識別轉換、隱含參考轉換、裝箱轉換、明確參考轉換或從 `E` 到 `T`的取消裝箱轉換，**或 `E` 或 `T` 是開啟的類型**，則靜態類型 `E` 的值會被視為與類型 `T` 的*模式相容*。</span><span class="sxs-lookup"><span data-stu-id="33436-132">A value of static type `E` is said to be *pattern compatible* with the type `T` if there exists an identity conversion, an implicit reference conversion, a boxing conversion, an explicit reference conversion, or an unboxing conversion from `E` to `T`, **or if either `E` or `T` is an open type**.</span></span> <span data-ttu-id="33436-133">如果 `E` 類型的運算式在與其相符的類型模式中的類型模式不相容，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="33436-133">It is a compile-time error if an expression of type `E` is not pattern compatible with the type in a type pattern that it is matched with.</span></span>

<span data-ttu-id="33436-134">宣告模式適用于執行參考型別的執行時間類型測試，並取代方法</span><span class="sxs-lookup"><span data-stu-id="33436-134">The declaration pattern is useful for performing run-time type tests of reference types, and replaces the idiom</span></span>

```csharp
var v = expr as Type;
if (v != null) { // code using v }
```

<span data-ttu-id="33436-135">稍微變得更簡潔</span><span class="sxs-lookup"><span data-stu-id="33436-135">With the slightly more concise</span></span>

```csharp
if (expr is Type v) { // code using v }
```

<span data-ttu-id="33436-136">如果*型*別是可為 null 的實值型別，就會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="33436-136">It is an error if *type* is a nullable value type.</span></span>

<span data-ttu-id="33436-137">宣告模式可用來測試可為 null 之類型的值： `Nullable<T>` 類型的值（或已裝箱的 `T`）符合類型模式 `T2 id` 如果值為非 null，且 `T2` 的類型為 `T`，或 `T`的部分基底類型或介面。</span><span class="sxs-lookup"><span data-stu-id="33436-137">The declaration pattern can be used to test values of nullable types: a value of type `Nullable<T>` (or a boxed `T`) matches a type pattern `T2 id` if the value is non-null and the type of `T2` is `T`, or some base type or interface of `T`.</span></span> <span data-ttu-id="33436-138">例如，在程式碼片段中</span><span class="sxs-lookup"><span data-stu-id="33436-138">For example, in the code fragment</span></span>

```csharp
int? x = 3;
if (x is int v) { // code using v }
```

<span data-ttu-id="33436-139">`if` 語句的條件會在執行時間 `true`，而變數 `v` 會保存區塊內類型 `int` 的值 `3`。</span><span class="sxs-lookup"><span data-stu-id="33436-139">The condition of the `if` statement is `true` at runtime and the variable `v` holds the value `3` of type `int` inside the block.</span></span>

### <a name="constant-pattern"></a><span data-ttu-id="33436-140">常數模式</span><span class="sxs-lookup"><span data-stu-id="33436-140">Constant pattern</span></span>

```antlr
constant_pattern
    : shift_expression
    ;
```

<span data-ttu-id="33436-141">常數模式會針對常數值測試運算式的值。</span><span class="sxs-lookup"><span data-stu-id="33436-141">A constant pattern tests the value of an expression against a constant value.</span></span> <span data-ttu-id="33436-142">常數可以是任何常數運算式，例如常值、宣告 `const` 變數的名稱，或是列舉常數或 `typeof` 運算式。</span><span class="sxs-lookup"><span data-stu-id="33436-142">The constant may be any constant expression, such as a literal, the name of a declared `const` variable, or an enumeration constant, or a `typeof` expression.</span></span>

<span data-ttu-id="33436-143">如果*e*和*c*都是整數類資料類型，則在運算式 `e == c` 的結果 `true`時，會將模式視為相符。</span><span class="sxs-lookup"><span data-stu-id="33436-143">If both *e* and *c* are of integral types, the pattern is considered matched if the result of the expression `e == c` is `true`.</span></span>

<span data-ttu-id="33436-144">否則，如果 `object.Equals(e, c)` 傳回 `true`，則會將模式視為相符。</span><span class="sxs-lookup"><span data-stu-id="33436-144">Otherwise the pattern is considered matching if `object.Equals(e, c)` returns `true`.</span></span> <span data-ttu-id="33436-145">在此情況下，如果*e*的靜態類型與常數的類型不*相容*，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="33436-145">In this case it is a compile-time error if the static type of *e* is not *pattern compatible* with the type of the constant.</span></span>

### <a name="var-pattern"></a><span data-ttu-id="33436-146">Var 模式</span><span class="sxs-lookup"><span data-stu-id="33436-146">Var pattern</span></span>

```antlr
var_pattern
    : 'var' simple_designation
    ;
```

<span data-ttu-id="33436-147">運算式*e*符合一律*var_pattern* 。</span><span class="sxs-lookup"><span data-stu-id="33436-147">An expression *e* matches a *var_pattern* always.</span></span> <span data-ttu-id="33436-148">換句話說，與*var 模式*的比對一律會成功。</span><span class="sxs-lookup"><span data-stu-id="33436-148">In other words, a match to a *var pattern* always succeeds.</span></span> <span data-ttu-id="33436-149">如果*simple_designation*是識別碼，則在執行時間， *e*的值會系結至新引進的區域變數。</span><span class="sxs-lookup"><span data-stu-id="33436-149">If the *simple_designation* is an identifier, then at runtime the value of *e* is bound to a newly introduced local variable.</span></span> <span data-ttu-id="33436-150">本機變數的類型是*e*的靜態類型。</span><span class="sxs-lookup"><span data-stu-id="33436-150">The type of the local variable is the static type of *e*.</span></span>

<span data-ttu-id="33436-151">如果名稱 `var` 系結至類型，就會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="33436-151">It is an error if the name `var` binds to a type.</span></span>

## <a name="switch-statement"></a><span data-ttu-id="33436-152">Switch 陳述式</span><span class="sxs-lookup"><span data-stu-id="33436-152">Switch statement</span></span>

<span data-ttu-id="33436-153">已擴充 `switch` 語句以選取要執行的第一個區塊，其具有與*switch 運算式*相符的關聯模式。</span><span class="sxs-lookup"><span data-stu-id="33436-153">The `switch` statement is extended to select for execution the first block having an associated pattern that matches the *switch expression*.</span></span>

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

<span data-ttu-id="33436-154">未定義模式相符的順序。</span><span class="sxs-lookup"><span data-stu-id="33436-154">The order in which patterns are matched is not defined.</span></span> <span data-ttu-id="33436-155">編譯器允許不按照順序來比對模式，並重複使用已符合模式的結果來計算符合其他模式的結果。</span><span class="sxs-lookup"><span data-stu-id="33436-155">A compiler is permitted to match patterns out of order, and to reuse the results of already matched patterns to compute the result of matching of other patterns.</span></span>

<span data-ttu-id="33436-156">如果有*案例-guard* ，其運算式的類型 `bool`。</span><span class="sxs-lookup"><span data-stu-id="33436-156">If a *case-guard* is present, its expression is of type `bool`.</span></span> <span data-ttu-id="33436-157">它會評估為額外的條件，必須滿足這種情況才會被視為滿足。</span><span class="sxs-lookup"><span data-stu-id="33436-157">It is evaluated as an additional condition that must be satisfied for the case to be considered satisfied.</span></span>

<span data-ttu-id="33436-158">如果*switch_label*在執行時間不會有任何作用，就會發生錯誤，因為它的模式是由先前的情況所建立小計。</span><span class="sxs-lookup"><span data-stu-id="33436-158">It is an error if a *switch_label* can have no effect at runtime because its pattern is subsumed by previous cases.</span></span> <span data-ttu-id="33436-159">[TODO：我們應該更精確地瞭解編譯器需要用來達成此判斷的技巧。]</span><span class="sxs-lookup"><span data-stu-id="33436-159">[TODO: We should be more precise about the techniques the compiler is required to use to reach this judgment.]</span></span>

<span data-ttu-id="33436-160">在*switch_label*中宣告的模式變數會在其 case 區塊中明確指派，只有在該案例區塊只包含一個*switch_label*時才會如此。</span><span class="sxs-lookup"><span data-stu-id="33436-160">A pattern variable declared in a *switch_label* is definitely assigned in its case block if and only if that case block contains precisely one *switch_label*.</span></span>

<span data-ttu-id="33436-161">[TODO：我們應該指定何時可連線到*switch 區塊*。]</span><span class="sxs-lookup"><span data-stu-id="33436-161">[TODO: We should specify when a *switch block* is reachable.]</span></span>

### <a name="scope-of-pattern-variables"></a><span data-ttu-id="33436-162">模式變數的範圍</span><span class="sxs-lookup"><span data-stu-id="33436-162">Scope of pattern variables</span></span>

<span data-ttu-id="33436-163">在模式中宣告的變數範圍如下所示：</span><span class="sxs-lookup"><span data-stu-id="33436-163">The scope of a variable declared in a pattern is as follows:</span></span>

- <span data-ttu-id="33436-164">如果模式是 case 標籤，則變數的範圍會是*案例區塊*。</span><span class="sxs-lookup"><span data-stu-id="33436-164">If the pattern is a case label, then the scope of the variable is the *case block*.</span></span>

<span data-ttu-id="33436-165">否則，系統會在*is_pattern*運算式中宣告變數，而且其範圍會以立即包含*is_pattern*運算式之運算式的結構為基礎，如下所示：</span><span class="sxs-lookup"><span data-stu-id="33436-165">Otherwise the variable is declared in an *is_pattern* expression, and its scope is based on the construct immediately enclosing the expression containing the *is_pattern* expression as follows:</span></span>

- <span data-ttu-id="33436-166">如果運算式是在運算式主體的 lambda 中，其範圍就是 lambda 的主體。</span><span class="sxs-lookup"><span data-stu-id="33436-166">If the expression is in an expression-bodied lambda, its scope is the body of the lambda.</span></span>
- <span data-ttu-id="33436-167">如果運算式位於運算式主體方法或屬性中，其範圍就是方法或屬性的主體。</span><span class="sxs-lookup"><span data-stu-id="33436-167">If the expression is in an expression-bodied method or property, its scope is the body of the method or property.</span></span>
- <span data-ttu-id="33436-168">如果運算式位於 `catch` 子句的 `when` 子句中，其範圍就是該 `catch` 子句。</span><span class="sxs-lookup"><span data-stu-id="33436-168">If the expression is in a `when` clause of a `catch` clause, its scope is that `catch` clause.</span></span>
- <span data-ttu-id="33436-169">如果運算式在*iteration_statement*中，其範圍就只是該語句。</span><span class="sxs-lookup"><span data-stu-id="33436-169">If the expression is in an *iteration_statement*, its scope is just that statement.</span></span>
- <span data-ttu-id="33436-170">否則，如果運算式是在某個其他語句形式中，其範圍就是包含語句的範圍。</span><span class="sxs-lookup"><span data-stu-id="33436-170">Otherwise if the expression is in some other statement form, its scope is the scope containing the statement.</span></span>

<span data-ttu-id="33436-171">為了決定範圍，會將*embedded_statement*視為在其本身的範圍中。</span><span class="sxs-lookup"><span data-stu-id="33436-171">For the purpose of determining the scope, an *embedded_statement* is considered to be in its own scope.</span></span> <span data-ttu-id="33436-172">例如， *if_statement*的文法為</span><span class="sxs-lookup"><span data-stu-id="33436-172">For example, the grammar for an *if_statement* is</span></span>

``` antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

<span data-ttu-id="33436-173">因此，如果*if_statement*的受控制語句宣告模式變數，其範圍會限制為該*embedded_statement*：</span><span class="sxs-lookup"><span data-stu-id="33436-173">So if the controlled statement of an *if_statement* declares a pattern variable, its scope is restricted to that *embedded_statement*:</span></span>

```csharp
if (x) M(y is var z);
```

<span data-ttu-id="33436-174">在此情況下，`z` 的範圍是內嵌的語句 `M(y is var z);`。</span><span class="sxs-lookup"><span data-stu-id="33436-174">In this case the scope of `z` is the embedded statement `M(y is var z);`.</span></span>

<span data-ttu-id="33436-175">其他情況則因其他原因而發生錯誤（例如，在參數的預設值或屬性中，這兩個都是錯誤，因為這些內容都需要常數運算式）。</span><span class="sxs-lookup"><span data-stu-id="33436-175">Other cases are errors for other reasons (e.g. in a parameter's default value or an attribute, both of which are an error because those contexts require a constant expression).</span></span>

> <span data-ttu-id="33436-176">[在C# 7.3 中，我們新增了](../csharp-7.3/expression-variables-in-initializers.md)可宣告模式變數的下列內容：</span><span class="sxs-lookup"><span data-stu-id="33436-176">[In C# 7.3 we added the following contexts](../csharp-7.3/expression-variables-in-initializers.md) in which a pattern variable may be declared:</span></span>
> - <span data-ttu-id="33436-177">如果運算式是在函式*初始化*運算式中，其範圍會是「函式*初始化*運算式」和「函式的主體」。</span><span class="sxs-lookup"><span data-stu-id="33436-177">If the expression is in a *constructor initializer*, its scope is the *constructor initializer* and the constructor's body.</span></span>
> - <span data-ttu-id="33436-178">如果運算式位於欄位初始化運算式中，其範圍就是其出現所在的*equals_value_clause* 。</span><span class="sxs-lookup"><span data-stu-id="33436-178">If the expression is in a field initializer, its scope is the *equals_value_clause* in which it appears.</span></span>
> - <span data-ttu-id="33436-179">如果運算式在指定要轉譯成 lambda 主體的查詢子句中，其範圍就只是該運算式。</span><span class="sxs-lookup"><span data-stu-id="33436-179">If the expression is in a query clause that is specified to be translated into the body of a lambda, its scope is just that expression.</span></span>

## <a name="changes-to-syntactic-disambiguation"></a><span data-ttu-id="33436-180">語法消除混淆的變更</span><span class="sxs-lookup"><span data-stu-id="33436-180">Changes to syntactic disambiguation</span></span>

<span data-ttu-id="33436-181">在某些情況下， C#文法不明確，而語言規格則說明如何解決這些歧義：</span><span class="sxs-lookup"><span data-stu-id="33436-181">There are situations involving generics where the C# grammar is ambiguous, and the language spec says how to resolve those ambiguities:</span></span>

> #### <a name="7652-grammar-ambiguities"></a><span data-ttu-id="33436-182">7.6.5.2 文法多義性</span><span class="sxs-lookup"><span data-stu-id="33436-182">7.6.5.2 Grammar ambiguities</span></span>
> <span data-ttu-id="33436-183">*簡單名稱*（第7.6.3）和*成員存取*（§7.6.5）的生產可能會使運算式的文法中出現不明確的情況。</span><span class="sxs-lookup"><span data-stu-id="33436-183">The productions for *simple-name* (§7.6.3) and *member-access* (§7.6.5) can give rise to ambiguities in the grammar for expressions.</span></span> <span data-ttu-id="33436-184">例如，語句：</span><span class="sxs-lookup"><span data-stu-id="33436-184">For example, the statement:</span></span>
> ```csharp
> F(G<A,B>(7));
> ```
> <span data-ttu-id="33436-185">可以使用兩個引數（`G < A` 和 `B > (7)`）來解讀為 `F` 的呼叫。</span><span class="sxs-lookup"><span data-stu-id="33436-185">could be interpreted as a call to `F` with two arguments, `G < A` and `B > (7)`.</span></span> <span data-ttu-id="33436-186">或者，也可以使用一個引數來解讀 `F` 的呼叫，這是使用兩個型別引數和一個一般引數呼叫泛型方法 `G`。</span><span class="sxs-lookup"><span data-stu-id="33436-186">Alternatively, it could be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span>

> <span data-ttu-id="33436-187">如果可以將一連串的 token 剖析（在內容中）為*簡單名稱*（7.6.3）、*成員存取*（第7.6.5），或以*類型引數清單*（第4.4.1）結尾的*指標成員存取*（第18.5.2），則會檢查緊接在結尾 `>` token 後面的權杖。</span><span class="sxs-lookup"><span data-stu-id="33436-187">If a sequence of tokens can be parsed (in context) as a *simple-name* (§7.6.3), *member-access* (§7.6.5), or *pointer-member-access* (§18.5.2) ending with a *type-argument-list* (§4.4.1), the token immediately following the closing `>` token is examined.</span></span> <span data-ttu-id="33436-188">如果它是下列其中一個</span><span class="sxs-lookup"><span data-stu-id="33436-188">If it is one of</span></span>
> ```none
> (  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
> ```
> <span data-ttu-id="33436-189">然後，*類型引數清單*會保留為*簡單名稱*、*成員存取*或*指標成員存取*的一部分，而且會捨棄標記序列的任何其他可能剖析。</span><span class="sxs-lookup"><span data-stu-id="33436-189">then the *type-argument-list* is retained as part of the *simple-name*, *member-access* or *pointer-member-access* and any other possible parse of the sequence of tokens is discarded.</span></span> <span data-ttu-id="33436-190">否則，即使沒有其他可能的 token 序列剖析，*類型引數清單*也不會被視為*簡單名稱*、*成員存取*或 >*指標成員存取*的一部分。</span><span class="sxs-lookup"><span data-stu-id="33436-190">Otherwise, the *type-argument-list* is not considered to be part of the *simple-name*, *member-access* or > *pointer-member-access*, even if there is no other possible parse of the sequence of tokens.</span></span> <span data-ttu-id="33436-191">請注意，在剖析*命名空間或*型別名稱中的型別自*變數清單*（第3.8 節）時，不會套用這些規則。</span><span class="sxs-lookup"><span data-stu-id="33436-191">Note that these rules are not applied when parsing a *type-argument-list* in a *namespace-or-type-name* (§3.8).</span></span> <span data-ttu-id="33436-192">陳述式</span><span class="sxs-lookup"><span data-stu-id="33436-192">The statement</span></span>
> ```csharp
> F(G<A,B>(7));
> ```
> <span data-ttu-id="33436-193">根據這項規則，會以一個引數來解讀 `F` 的呼叫，這是使用兩個型別引數和一個一般引數呼叫泛型方法 `G`。</span><span class="sxs-lookup"><span data-stu-id="33436-193">will, according to this rule, be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span> <span data-ttu-id="33436-194">語句</span><span class="sxs-lookup"><span data-stu-id="33436-194">The statements</span></span>
> ```csharp
> F(G < A, B > 7);
> F(G < A, B >> 7);
> ```
> <span data-ttu-id="33436-195">每個都會以兩個引數來解讀 `F` 的呼叫。</span><span class="sxs-lookup"><span data-stu-id="33436-195">will each be interpreted as a call to `F` with two arguments.</span></span> <span data-ttu-id="33436-196">陳述式</span><span class="sxs-lookup"><span data-stu-id="33436-196">The statement</span></span>
> ```csharp
> x = F < A > +y;
> ```
> <span data-ttu-id="33436-197">將會解讀為小於運算子、大於運算子和一元加號運算子，如同已 `x = (F < A) > (+y)`寫入語句，而不是以*類型引數清單*後面接著二元加號運算子的*簡單名稱*。</span><span class="sxs-lookup"><span data-stu-id="33436-197">will be interpreted as a less than operator, greater than operator, and unary plus operator, as if the statement had been written `x = (F < A) > (+y)`, instead of as a *simple-name* with a *type-argument-list* followed by a binary plus operator.</span></span> <span data-ttu-id="33436-198">在語句中</span><span class="sxs-lookup"><span data-stu-id="33436-198">In the statement</span></span>
> ```csharp
> x = y is C<T> + z;
> ```
> <span data-ttu-id="33436-199">`C<T>` 的 token 會以*類型引數清單*的*命名空間或類型名稱*來解讀。</span><span class="sxs-lookup"><span data-stu-id="33436-199">the tokens `C<T>` are interpreted as a *namespace-or-type-name* with a *type-argument-list*.</span></span>

<span data-ttu-id="33436-200">7中C#引進了許多變更，使得這些去除去除規則不再足以處理語言的複雜性。</span><span class="sxs-lookup"><span data-stu-id="33436-200">There are a number of changes being introduced in C# 7 that make these disambiguation rules no longer sufficient to handle the complexity of the language.</span></span>

### <a name="out-variable-declarations"></a><span data-ttu-id="33436-201">Out 變數宣告</span><span class="sxs-lookup"><span data-stu-id="33436-201">Out variable declarations</span></span>

<span data-ttu-id="33436-202">您現在可以在 out 引數中宣告變數：</span><span class="sxs-lookup"><span data-stu-id="33436-202">It is now possible to declare a variable in an out argument:</span></span>

```csharp
M(out Type name);
```

<span data-ttu-id="33436-203">不過，此類型可能是泛型：</span><span class="sxs-lookup"><span data-stu-id="33436-203">However, the type may be generic:</span></span> 

```csharp
M(out A<B> name);
```

<span data-ttu-id="33436-204">由於引數的語言文法會使用*運算式*，因此此內容會受到去除去除規則的規範。</span><span class="sxs-lookup"><span data-stu-id="33436-204">Since the language grammar for the argument uses *expression*, this context is subject to the disambiguation rule.</span></span> <span data-ttu-id="33436-205">在此情況下，關閉 `>` 後面接著一個*識別碼*，這不是允許它被視為*類型引數清單*的其中一個標記。</span><span class="sxs-lookup"><span data-stu-id="33436-205">In this case the closing `>` is followed by an *identifier*, which is not one of the tokens that permits it to be treated as a *type-argument-list*.</span></span> <span data-ttu-id="33436-206">因此，我建議您**將*識別碼*新增至一組權杖，以觸發對*類型引數清單*的**去除混淆。</span><span class="sxs-lookup"><span data-stu-id="33436-206">I therefore propose to **add *identifier* to the set of tokens that triggers the disambiguation to a *type-argument-list*.**</span></span>

### <a name="tuples-and-deconstruction-declarations"></a><span data-ttu-id="33436-207">元組和解構宣告</span><span class="sxs-lookup"><span data-stu-id="33436-207">Tuples and deconstruction declarations</span></span>

<span data-ttu-id="33436-208">元組常值會以完全相同的問題來執行。</span><span class="sxs-lookup"><span data-stu-id="33436-208">A tuple literal runs into exactly the same issue.</span></span> <span data-ttu-id="33436-209">考慮元組運算式</span><span class="sxs-lookup"><span data-stu-id="33436-209">Consider the tuple expression</span></span>

```csharp
(A < B, C > D, E < F, G > H)
```

<span data-ttu-id="33436-210">在剖析自C#變數清單的舊6個規則底下，這會剖析為具有四個元素的元組，從 `A < B` 開始。</span><span class="sxs-lookup"><span data-stu-id="33436-210">Under the old C# 6 rules for parsing an argument list, this would parse as a tuple with four elements, starting with `A < B` as the first.</span></span> <span data-ttu-id="33436-211">不過，當這種情況出現在解構的左邊時，我們想要由*識別碼*權杖觸發的去除混淆，如上所述：</span><span class="sxs-lookup"><span data-stu-id="33436-211">However, when this appears on the left of a deconstruction, we want the disambiguation triggered by the *identifier* token as described above:</span></span>

```csharp
(A<B,C> D, E<F,G> H) = e;
```

<span data-ttu-id="33436-212">這是宣告兩個變數的解構宣告，第一個是類型 `A<B,C>` 且名為 `D`。</span><span class="sxs-lookup"><span data-stu-id="33436-212">This is a deconstruction declaration which declares two variables, the first of which is of type `A<B,C>` and named `D`.</span></span> <span data-ttu-id="33436-213">換句話說，元組常值包含兩個運算式，其中每一個都是宣告運算式。</span><span class="sxs-lookup"><span data-stu-id="33436-213">In other words, the tuple literal contains two expressions, each of which is a declaration expression.</span></span>

<span data-ttu-id="33436-214">為了簡化規格和編譯器，我建議將這個元組常值剖析為兩個元素的元組（不論它是否出現在指派的左側）。</span><span class="sxs-lookup"><span data-stu-id="33436-214">For simplicity of the specification and compiler, I propose that this tuple literal be parsed as a two-element tuple wherever it appears (whether or not it appears on the left-hand-side of an assignment).</span></span> <span data-ttu-id="33436-215">這會是上一節中所述去除混淆的自然結果。</span><span class="sxs-lookup"><span data-stu-id="33436-215">That would be a natural result of the disambiguation described in the previous section.</span></span>

### <a name="pattern-matching"></a><span data-ttu-id="33436-216">模式比對</span><span class="sxs-lookup"><span data-stu-id="33436-216">Pattern-matching</span></span>

<span data-ttu-id="33436-217">模式比對引進了新的內容，其中會出現運算式類型的多義性。</span><span class="sxs-lookup"><span data-stu-id="33436-217">Pattern matching introduces a new context where the expression-type ambiguity arises.</span></span> <span data-ttu-id="33436-218">先前 `is` 運算子的右手邊是型別。</span><span class="sxs-lookup"><span data-stu-id="33436-218">Previously the right-hand-side of an `is` operator was a type.</span></span> <span data-ttu-id="33436-219">現在可以是型別或運算式，如果它是型別，則後面可能接著識別碼。</span><span class="sxs-lookup"><span data-stu-id="33436-219">Now it can be a type or expression, and if it is a type it may be followed by an identifier.</span></span> <span data-ttu-id="33436-220">在技術上，這可以變更現有程式碼的意義：</span><span class="sxs-lookup"><span data-stu-id="33436-220">This can, technically, change the meaning of existing code:</span></span>

```csharp
var x = e is T < A > B;
```

<span data-ttu-id="33436-221">這可在 c # 6 規則下剖析為</span><span class="sxs-lookup"><span data-stu-id="33436-221">This could be parsed under C#6 rules as</span></span>

```csharp
var x = ((e is T) < A) > B;
```

<span data-ttu-id="33436-222">但在 c # 7 規則底下（加上上面提議的去除混淆），會剖析為</span><span class="sxs-lookup"><span data-stu-id="33436-222">but under under C#7 rules (with the disambiguation proposed above) would be parsed as</span></span>

```csharp
var x = e is T<A> B;
```

<span data-ttu-id="33436-223">這會宣告 `T<A>`類型的變數 `B`。</span><span class="sxs-lookup"><span data-stu-id="33436-223">which declares a variable `B` of type `T<A>`.</span></span> <span data-ttu-id="33436-224">幸好，native 和 Roslyn 編譯器有 bug，因此會在 c # 6 程式碼上提供語法錯誤。</span><span class="sxs-lookup"><span data-stu-id="33436-224">Fortunately, the native and Roslyn compilers have a bug whereby they give a syntax error on the C#6 code.</span></span> <span data-ttu-id="33436-225">因此，這種特定的重大變更不是問題。</span><span class="sxs-lookup"><span data-stu-id="33436-225">Therefore this particular breaking change is not a concern.</span></span>

<span data-ttu-id="33436-226">模式比對會引進額外的 token，這些權杖應該會驅動明確解決方式來選取類型。</span><span class="sxs-lookup"><span data-stu-id="33436-226">Pattern-matching introduces additional tokens that should drive the ambiguity resolution toward selecting a type.</span></span> <span data-ttu-id="33436-227">下列現有有效 c # 6 程式碼的範例會中斷，而不會有額外的去除混淆規則：</span><span class="sxs-lookup"><span data-stu-id="33436-227">The following examples of existing valid C#6 code would be broken without additional disambiguation rules:</span></span>

```csharp
var x = e is A<B> && f;            // &&
var x = e is A<B> || f;            // ||
var x = e is A<B> & f;             // &
var x = e is A<B>[];               // [
```

### <a name="proposed-change-to-the-disambiguation-rule"></a><span data-ttu-id="33436-228">建議變更去除去除規則</span><span class="sxs-lookup"><span data-stu-id="33436-228">Proposed change to the disambiguation rule</span></span>

<span data-ttu-id="33436-229">我建議您修改規格，以變更厘清 token 的清單。</span><span class="sxs-lookup"><span data-stu-id="33436-229">I propose to revise the specification to change the list of disambiguating tokens from</span></span>

>
```none
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```

<span data-ttu-id="33436-230">to</span><span class="sxs-lookup"><span data-stu-id="33436-230">to</span></span>

>
```none
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [
```

<span data-ttu-id="33436-231">而且，在某些內容中，我們會將*identifier*視為厘清 token。</span><span class="sxs-lookup"><span data-stu-id="33436-231">And, in certain contexts, we treat *identifier* as a disambiguating token.</span></span> <span data-ttu-id="33436-232">這些內容是要辨識的 token 序列緊接在 `is`、`case`或 `out`的其中一個關鍵字前面，或在剖析元組常值的第一個專案（在此情況下，權杖前面會加上 `(` 或 `:`，而識別碼後面接著 `,`）或元組常值的後續元素。</span><span class="sxs-lookup"><span data-stu-id="33436-232">Those contexts are where the sequence of tokens being disambiguated is immediately preceded by one of the keywords `is`, `case`, or `out`, or arises while parsing the first element of a tuple literal (in which case the tokens are preceded by `(` or `:` and the identifier is followed by a `,`) or a subsequent element of a tuple literal.</span></span>

### <a name="modified-disambiguation-rule"></a><span data-ttu-id="33436-233">修改過的去除規則</span><span class="sxs-lookup"><span data-stu-id="33436-233">Modified disambiguation rule</span></span>

<span data-ttu-id="33436-234">修改過的去除混淆規則會類似這樣</span><span class="sxs-lookup"><span data-stu-id="33436-234">The revised disambiguation rule would be something like this</span></span>

> <span data-ttu-id="33436-235">如果可以將一系列的 token 剖析（在內容中）為*簡單名稱*（§7.6.3）、*成員存取*（第7.6.5），或以*類型引數清單*（第4.4.1）結尾的*指標成員存取*（第18.5.2），則會檢查緊接在結尾 `>` token 後面的標記，以查看是否為</span><span class="sxs-lookup"><span data-stu-id="33436-235">If a sequence of tokens can be parsed (in context) as a *simple-name* (§7.6.3), *member-access* (§7.6.5), or *pointer-member-access* (§18.5.2) ending with a *type-argument-list* (§4.4.1), the token immediately following the closing `>` token is examined, to see if it is</span></span>
> - <span data-ttu-id="33436-236">其中一個 `(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [`;或</span><span class="sxs-lookup"><span data-stu-id="33436-236">One of `(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [`; or</span></span>
> - <span data-ttu-id="33436-237">其中一個關聯式運算子 `<  >  <=  >=  is as`;或</span><span class="sxs-lookup"><span data-stu-id="33436-237">One of the relational operators `<  >  <=  >=  is as`; or</span></span>
> - <span data-ttu-id="33436-238">出現在查詢運算式中的內容查詢關鍵字;或</span><span class="sxs-lookup"><span data-stu-id="33436-238">A contextual query keyword appearing inside a query expression; or</span></span>
> - <span data-ttu-id="33436-239">在某些內容中，我們會將*identifier*視為厘清 token。</span><span class="sxs-lookup"><span data-stu-id="33436-239">In certain contexts, we treat *identifier* as a disambiguating token.</span></span> <span data-ttu-id="33436-240">這些內容是要辨識的 token 順序，其前面會加上其中一個關鍵字 `is`、`case` 或 `out`，或在剖析元組常值的第一個專案時（在此情況下，權杖前面會加上 `(` 或 `:`，而識別碼後面接著 `,`）或元組常值的後續元素。</span><span class="sxs-lookup"><span data-stu-id="33436-240">Those contexts are where the sequence of tokens being disambiguated is immediately preceded by one of the keywords `is`, `case` or `out`, or arises while parsing the first element of a tuple literal (in which case the tokens are preceded by `(` or `:` and the identifier is followed by a `,`) or a subsequent element of a tuple literal.</span></span>
> 
> <span data-ttu-id="33436-241">如果下列標記是在此清單中，或這類內容中的識別碼，則*類型引數清單*會保留為*簡單名稱*、*成員存取*或成員*存取*的一部分，而且會捨棄標記序列的任何其他可能剖析。</span><span class="sxs-lookup"><span data-stu-id="33436-241">If the following token is among this list, or an identifier in such a context, then the *type-argument-list* is retained as part of the *simple-name*, *member-access* or  *pointer-member-access* and any other possible parse of the sequence of tokens is discarded.</span></span>  <span data-ttu-id="33436-242">否則，即使沒有其他可能的 token 序列剖析，也不會將*類型引數清單*視為*簡單名稱*、*成員存取*或成員*存取*的一部分。</span><span class="sxs-lookup"><span data-stu-id="33436-242">Otherwise, the *type-argument-list* is not considered to be part of the *simple-name*, *member-access* or *pointer-member-access*, even if there is no other possible parse of the sequence of tokens.</span></span> <span data-ttu-id="33436-243">請注意，在剖析*命名空間或*型別名稱中的型別自*變數清單*（第3.8 節）時，不會套用這些規則。</span><span class="sxs-lookup"><span data-stu-id="33436-243">Note that these rules are not applied when parsing a *type-argument-list* in a *namespace-or-type-name* (§3.8).</span></span>

### <a name="breaking-changes-due-to-this-proposal"></a><span data-ttu-id="33436-244">因此提案而造成的重大變更</span><span class="sxs-lookup"><span data-stu-id="33436-244">Breaking changes due to this proposal</span></span>

<span data-ttu-id="33436-245">因為此建議的去除混淆規則，所以不知道任何重大變更。</span><span class="sxs-lookup"><span data-stu-id="33436-245">No breaking changes are known due to this proposed disambiguation rule.</span></span>

### <a name="interesting-examples"></a><span data-ttu-id="33436-246">有趣的範例</span><span class="sxs-lookup"><span data-stu-id="33436-246">Interesting examples</span></span>

<span data-ttu-id="33436-247">以下是這些去除消除規則的一些有趣結果：</span><span class="sxs-lookup"><span data-stu-id="33436-247">Here are some interesting results of these disambiguation rules:</span></span>

<span data-ttu-id="33436-248">運算式 `(A < B, C > D)` 是具有兩個元素的元組，每個都有一個比較。</span><span class="sxs-lookup"><span data-stu-id="33436-248">The expression `(A < B, C > D)` is a tuple with two elements, each a comparison.</span></span>

<span data-ttu-id="33436-249">運算式 `(A<B,C> D, E)` 是具有兩個元素的元組，其中第一個是宣告運算式。</span><span class="sxs-lookup"><span data-stu-id="33436-249">The expression `(A<B,C> D, E)` is a tuple with two elements, the first of which is a declaration expression.</span></span>

<span data-ttu-id="33436-250">調用 `M(A < B, C > D, E)` 有三個引數。</span><span class="sxs-lookup"><span data-stu-id="33436-250">The invocation `M(A < B, C > D, E)` has three arguments.</span></span>

<span data-ttu-id="33436-251">調用 `M(out A<B,C> D, E)` 有兩個引數，第一個是 `out` 宣告。</span><span class="sxs-lookup"><span data-stu-id="33436-251">The invocation `M(out A<B,C> D, E)` has two arguments, the first of which is an `out` declaration.</span></span>

<span data-ttu-id="33436-252">運算式 `e is A<B> C` 使用宣告運算式。</span><span class="sxs-lookup"><span data-stu-id="33436-252">The expression `e is A<B> C` uses a declaration expression.</span></span>

<span data-ttu-id="33436-253">Case 標籤 `case A<B> C:` 使用宣告運算式。</span><span class="sxs-lookup"><span data-stu-id="33436-253">The case label `case A<B> C:` uses a declaration expression.</span></span>

## <a name="some-examples-of-pattern-matching"></a><span data-ttu-id="33436-254">模式比對的一些範例</span><span class="sxs-lookup"><span data-stu-id="33436-254">Some examples of pattern matching</span></span>

### <a name="is-as"></a><span data-ttu-id="33436-255">為-As</span><span class="sxs-lookup"><span data-stu-id="33436-255">Is-As</span></span>

<span data-ttu-id="33436-256">我們可以取代此用法</span><span class="sxs-lookup"><span data-stu-id="33436-256">We can replace the idiom</span></span>

```csharp
var v = expr as Type;   
if (v != null) {
    // code using v
}
```

<span data-ttu-id="33436-257">稍微更精確且直接的</span><span class="sxs-lookup"><span data-stu-id="33436-257">With the slightly more concise and direct</span></span>

```csharp
if (expr is Type v) {
    // code using v
}
```

### <a name="testing-nullable"></a><span data-ttu-id="33436-258">測試可為 null</span><span class="sxs-lookup"><span data-stu-id="33436-258">Testing nullable</span></span>

<span data-ttu-id="33436-259">我們可以取代此用法</span><span class="sxs-lookup"><span data-stu-id="33436-259">We can replace the idiom</span></span>

```csharp
Type? v = x?.y?.z;
if (v.HasValue) {
    var value = v.GetValueOrDefault();
    // code using value
}
```

<span data-ttu-id="33436-260">稍微更精確且直接的</span><span class="sxs-lookup"><span data-stu-id="33436-260">With the slightly more concise and direct</span></span>

```csharp
if (x?.y?.z is Type value) {
    // code using value
}
```

### <a name="arithmetic-simplification"></a><span data-ttu-id="33436-261">算術簡化</span><span class="sxs-lookup"><span data-stu-id="33436-261">Arithmetic simplification</span></span>

<span data-ttu-id="33436-262">假設我們定義一組遞迴類型來代表運算式（根據個別的提案）：</span><span class="sxs-lookup"><span data-stu-id="33436-262">Suppose we define a set of recursive types to represent expressions (per a separate proposal):</span></span>

```csharp
abstract class Expr;
class X() : Expr;
class Const(double Value) : Expr;
class Add(Expr Left, Expr Right) : Expr;
class Mult(Expr Left, Expr Right) : Expr;
class Neg(Expr Value) : Expr;
```

<span data-ttu-id="33436-263">現在，我們可以定義函式來計算運算式的（unreduced）衍生：</span><span class="sxs-lookup"><span data-stu-id="33436-263">Now we can define a function to compute the (unreduced) derivative of an expression:</span></span>

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

<span data-ttu-id="33436-264">運算式 simplifier 會示範位置模式：</span><span class="sxs-lookup"><span data-stu-id="33436-264">An expression simplifier demonstrates positional patterns:</span></span>

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
