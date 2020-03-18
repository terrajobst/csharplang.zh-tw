---
ms.openlocfilehash: 49720d62c496ff904eacacb31ae29ef97a5aefaa
ms.sourcegitcommit: 8152182f0a477cb3082e625b607262cc459a17f3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/23/2019
ms.locfileid: "79484941"
---
# <a name="records"></a><span data-ttu-id="9473c-101">記錄</span><span class="sxs-lookup"><span data-stu-id="9473c-101">records</span></span>

* <span data-ttu-id="9473c-102">[x] 提議</span><span class="sxs-lookup"><span data-stu-id="9473c-102">[x] Proposed</span></span>
* <span data-ttu-id="9473c-103">[] 原型：[完成](https://github.com/PROTOTYPE_OWNER/roslyn/BRANCH_NAME)</span><span class="sxs-lookup"><span data-stu-id="9473c-103">[ ] Prototype: [Complete](https://github.com/PROTOTYPE_OWNER/roslyn/BRANCH_NAME)</span></span>
* <span data-ttu-id="9473c-104">[] 執行[中：進行中](https://github.com/dotnet/roslyn/BRANCH_NAME)</span><span class="sxs-lookup"><span data-stu-id="9473c-104">[ ] Implementation: [In Progress](https://github.com/dotnet/roslyn/BRANCH_NAME)</span></span>
* <span data-ttu-id="9473c-105">[] 規格：包含的草稿規格</span><span class="sxs-lookup"><span data-stu-id="9473c-105">[ ] Specification: Draft specification enclosed</span></span>

## <a name="summary"></a><span data-ttu-id="9473c-106">摘要</span><span class="sxs-lookup"><span data-stu-id="9473c-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="9473c-107">記錄是一種新的簡化宣告形式C# ，適用于類別和結構類型，可結合一些較簡單功能的優點。</span><span class="sxs-lookup"><span data-stu-id="9473c-107">Records are a new, simplified declaration form for C# class and struct types that combine the benefits of a number of simpler features.</span></span> <span data-ttu-id="9473c-108">我們會描述新的功能（呼叫端-接收者參數和*with-expression*s）、提供記錄宣告的語法和語義，然後提供一些範例。</span><span class="sxs-lookup"><span data-stu-id="9473c-108">We describe the new features (caller-receiver parameters and *with-expression*s), give the syntax and semantics for record declarations, and then provide some examples.</span></span>


## <a name="motivation"></a><span data-ttu-id="9473c-109">動機</span><span class="sxs-lookup"><span data-stu-id="9473c-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="9473c-110">中C#的大量型別宣告，只是類型資料的匯總集合而已。</span><span class="sxs-lookup"><span data-stu-id="9473c-110">A significant number of type declarations in C# are little more than aggregate collections of typed data.</span></span> <span data-ttu-id="9473c-111">可惜的是，宣告這類類型需要大量的重複使用程式碼。</span><span class="sxs-lookup"><span data-stu-id="9473c-111">Unfortunately, declaring such types requires a great deal of boilerplate code.</span></span> <span data-ttu-id="9473c-112">*記錄*提供一種機制來宣告資料類型，方法是描述匯總的成員以及其他程式碼，或與一般的重複使用方式偏差（如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="9473c-112">*Records* provide a mechanism for declaring a datatype by describing the members of the aggregate along with additional code or deviations from the usual boilerplate, if any.</span></span>

<span data-ttu-id="9473c-113">請參閱下面的[範例](#examples)。</span><span class="sxs-lookup"><span data-stu-id="9473c-113">See [Examples](#examples), below.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="9473c-114">詳細設計</span><span class="sxs-lookup"><span data-stu-id="9473c-114">Detailed design</span></span>
[design]: #detailed-design

### <a name="caller-receiver-parameters"></a><span data-ttu-id="9473c-115">呼叫端-接收者參數</span><span class="sxs-lookup"><span data-stu-id="9473c-115">caller-receiver parameters</span></span>

<span data-ttu-id="9473c-116">目前，方法參數的*預設引數*必須是</span><span class="sxs-lookup"><span data-stu-id="9473c-116">Currently a method parameter's *default-argument* must be</span></span> 
- <span data-ttu-id="9473c-117">*常數運算式*;或</span><span class="sxs-lookup"><span data-stu-id="9473c-117">a *constant-expression*; or</span></span>
- <span data-ttu-id="9473c-118">表單 `new S()` 的運算式，其中 `S` 是實值型別;或</span><span class="sxs-lookup"><span data-stu-id="9473c-118">an expression of the form `new S()` where `S` is a value type; or</span></span>
- <span data-ttu-id="9473c-119">表單 `default(S)` 的運算式，其中 `S` 是實數值型別</span><span class="sxs-lookup"><span data-stu-id="9473c-119">an expression of the form `default(S)` where `S` is a value type</span></span>

<span data-ttu-id="9473c-120">我們會擴充此項來新增下列</span><span class="sxs-lookup"><span data-stu-id="9473c-120">We extend this to add the following</span></span>
- <span data-ttu-id="9473c-121">格式為的運算式 `this.Identifier`</span><span class="sxs-lookup"><span data-stu-id="9473c-121">an expression of the form `this.Identifier`</span></span>

<span data-ttu-id="9473c-122">這個新的表單稱為*呼叫者-接收者的預設引數*，只有在滿足下列所有條件時，才允許使用</span><span class="sxs-lookup"><span data-stu-id="9473c-122">This new form is called a *caller-receiver default-argument*, and is allowed only if all of the following are satisfied</span></span>
- <span data-ttu-id="9473c-123">其出現的方法是實例方法;和</span><span class="sxs-lookup"><span data-stu-id="9473c-123">The method in which it appears is an instance method; and</span></span>
- <span data-ttu-id="9473c-124">運算式 `this.Identifier` 系結至封閉類型的實例成員，必須是欄位或屬性;和</span><span class="sxs-lookup"><span data-stu-id="9473c-124">The expression `this.Identifier` binds to an instance member of the enclosing type, which must be either a field or a property; and</span></span>
- <span data-ttu-id="9473c-125">它所系結的成員（以及 `get` 存取子（如果它是屬性），至少會如同方法一樣存取。和</span><span class="sxs-lookup"><span data-stu-id="9473c-125">The member to which it binds (and the `get` accessor, if it is a property) is at least as accessible as the method; and</span></span>
- <span data-ttu-id="9473c-126">`this.Identifier` 的類型可透過身分識別或可為 null 的轉換為參數類型（這是*預設引數*上的現有條件約束）隱含轉換。</span><span class="sxs-lookup"><span data-stu-id="9473c-126">The type of `this.Identifier` is implicitly convertible by an identity or nullable conversion to the type of the parameter (this is an existing constraint on *default-argument*).</span></span>

<span data-ttu-id="9473c-127">當具有*呼叫端預設引數*之對應選擇性參數的函式成員調用中省略引數時，會隱含地傳遞接收者成員的值。</span><span class="sxs-lookup"><span data-stu-id="9473c-127">When an argument is omitted from an invocation of a function member for a corresponding optional parameter with a *caller-receiver default-argument*, the value of the receiver's member is implicitly passed.</span></span> 

> <span data-ttu-id="9473c-128">**設計附注**：呼叫端-接收者參數的主要原因是要支援*with 運算式*。</span><span class="sxs-lookup"><span data-stu-id="9473c-128">**Design Notes**: the main reason for the caller-receiver parameter is to support the *with-expression*.</span></span> <span data-ttu-id="9473c-129">其概念是，您可以宣告如下的方法</span><span class="sxs-lookup"><span data-stu-id="9473c-129">The idea is that you can declare a method like this</span></span>
> ```cs
> class Point
> {
>     public readonly int X;
>     public readonly int Y;
>     public Point With(int x = this.X, int y = this.Y) => new Point(x, y);
>     // etc
> }
> ```
> <span data-ttu-id="9473c-130">然後使用它，如下所示</span><span class="sxs-lookup"><span data-stu-id="9473c-130">and then use it like this</span></span>
> ```cs
>     Point p = new Point(3, 4);
>     p = p.With(x: 1);
> ```
> <span data-ttu-id="9473c-131">若要建立新的 `Point`，如同現有 `Point`，但已變更 `X` 的值。</span><span class="sxs-lookup"><span data-stu-id="9473c-131">To create a new `Point` just like an existing `Point` but with the value of `X` changed.</span></span>
> 
> <span data-ttu-id="9473c-132">這是一個開放問題，不論是否有使用呼叫端接收者參數的支援，都需要加上*運算式*的語法形式，因此我們可以執行此動作，*而*不是*除了* *with-運算式*以外。</span><span class="sxs-lookup"><span data-stu-id="9473c-132">It is an open question whether or not the syntactic form of the *with-expression* is worth adding once we have support for caller-receiver parameters, so it is possible we would do this *instead of* rather than *in addition to* the *with-expression*.</span></span>

- <span data-ttu-id="9473c-133">[]**開啟問題**：針對其他引數評估*呼叫者-接收者的預設引數*的順序為何？</span><span class="sxs-lookup"><span data-stu-id="9473c-133">[ ] **Open issue**: What is the order in which a *caller-receiver default-argument* is evaluated with respect to other arguments?</span></span> <span data-ttu-id="9473c-134">我們是否應該指明它是未指定的？</span><span class="sxs-lookup"><span data-stu-id="9473c-134">Should we say that it is unspecified?</span></span>

### <a name="with-expressions"></a><span data-ttu-id="9473c-135">with-運算式</span><span class="sxs-lookup"><span data-stu-id="9473c-135">with-expressions</span></span>

<span data-ttu-id="9473c-136">建議使用新的運算式表單：</span><span class="sxs-lookup"><span data-stu-id="9473c-136">A new expression form is proposed:</span></span>

```antlr
primary_expression
    : with_expression
    ;

with_expression
    : primary_expression 'with' '{' with_initializer_list '}'
    ;

with_initializer_list
    : with_initializer
    | with_initializer ',' with_initializer_list
    ;

with_initializer
    : identifier '=' expression
    ;
```

<span data-ttu-id="9473c-137">Token `with` 是一個新的內容相關關鍵字。</span><span class="sxs-lookup"><span data-stu-id="9473c-137">The token `with` is a new context-sensitive keyword.</span></span>

<span data-ttu-id="9473c-138">*With_initializer*左邊的每個*識別碼*都必須系結至*with_expression*之*primary_expression*類型的可存取實例欄位或屬性。</span><span class="sxs-lookup"><span data-stu-id="9473c-138">Each *identifier* on the left of a *with_initializer* must bind to an accessible instance field or property of the type of the *primary_expression* of the *with_expression*.</span></span> <span data-ttu-id="9473c-139">指定*with_expression*的這些識別碼之間可能沒有重複的名稱。</span><span class="sxs-lookup"><span data-stu-id="9473c-139">There may be no duplicated name among these identifiers of a given *with_expression*.</span></span>

<span data-ttu-id="9473c-140">表單的*with_expression*</span><span class="sxs-lookup"><span data-stu-id="9473c-140">A *with_expression* of the form</span></span>

> <span data-ttu-id="9473c-141">*e1* `with` `{`*識別碼* = *e2*，... `}`</span><span class="sxs-lookup"><span data-stu-id="9473c-141">*e1* `with` `{` *identifier* = *e2*, ... `}`</span></span>

<span data-ttu-id="9473c-142">會被視為形式的調用</span><span class="sxs-lookup"><span data-stu-id="9473c-142">is treated as an invocation of the form</span></span>

> <span data-ttu-id="9473c-143">*e1*`.With(`*identifier2*`:` *e2*，...`)`</span><span class="sxs-lookup"><span data-stu-id="9473c-143">*e1*`.With(`*identifier2*`:` *e2*, ...`)`</span></span>

<span data-ttu-id="9473c-144">其中，針對 `With` 名為*identifier2 之可存取實例成員的*每個方法，我們會選取 [ *identifier2* ] 做為該方法中第一個參數的名稱，該方法的呼叫端-接收者參數與實例欄位或系結至*識別碼*的屬性相同。</span><span class="sxs-lookup"><span data-stu-id="9473c-144">Where, for each method named `With` that is an accessible instance member of *e1*, we select *identifier2* as the name of the first parameter in that method that has a caller-receiver parameter that is the same member as the instance field or property bound to *identifier*.</span></span> <span data-ttu-id="9473c-145">如果找不到此類參數，就不會考慮該方法。</span><span class="sxs-lookup"><span data-stu-id="9473c-145">If no such parameter can be identified that method is eliminated from consideration.</span></span> <span data-ttu-id="9473c-146">要叫用的方法是透過多載解析從剩餘的候選項目中選取。</span><span class="sxs-lookup"><span data-stu-id="9473c-146">The method to be invoked is selected from among the remaining candidates by overload resolution.</span></span>

> <span data-ttu-id="9473c-147">**設計附注**：指定呼叫端-接收者參數，有許多*with 運算式*的優點都可以使用，而不需要這個特殊的語法形式。</span><span class="sxs-lookup"><span data-stu-id="9473c-147">**Design Notes**: Given caller-receiver parameters, many of the benefits of the *with-expression* are available without this special syntax form.</span></span> <span data-ttu-id="9473c-148">因此，我們會考慮是否需要它。</span><span class="sxs-lookup"><span data-stu-id="9473c-148">We are therefore considering whether or not it is needed.</span></span> <span data-ttu-id="9473c-149">其主要優點是可讓您以欄位和屬性的名稱來進行程式設計，而不是以參數的名稱為依據。</span><span class="sxs-lookup"><span data-stu-id="9473c-149">Its main benefit is allowing one to program in terms of the names of fields and properties, rather than in terms of the names of parameters.</span></span> <span data-ttu-id="9473c-150">如此一來，我們就可以同時改善可讀性和工具品質（例如， *with_expression*的識別碼上的進入定義會導覽至屬性，而不是方法參數）。</span><span class="sxs-lookup"><span data-stu-id="9473c-150">In this way we improve both readability and the quality of tooling (e.g. go-to-definition on the identifier of a *with_expression* would navigate to the property rather than to a method parameter).</span></span>

- <span data-ttu-id="9473c-151">[]**開啟問題**：應該修改此描述以支援擴充方法。</span><span class="sxs-lookup"><span data-stu-id="9473c-151">[ ] **Open issue**: This description should be modified to support extension methods.</span></span>

### <a name="pattern-matching"></a><span data-ttu-id="9473c-152">模式比對</span><span class="sxs-lookup"><span data-stu-id="9473c-152">pattern-matching</span></span>

<span data-ttu-id="9473c-153">請參閱[模式](csharp-8.0/patterns.md#positional-pattern)比對規格，以瞭解 `Deconstruct` 的規格及其與模式比對的關聯性。</span><span class="sxs-lookup"><span data-stu-id="9473c-153">See the [Pattern Matching Specification](csharp-8.0/patterns.md#positional-pattern) for a specification of `Deconstruct` and its relationship to pattern-matching.</span></span>

> <span data-ttu-id="9473c-154">**設計附注**：根據此處所指定的編譯器產生的 `Deconstruct`，以及模式比對的規格，記錄宣告</span><span class="sxs-lookup"><span data-stu-id="9473c-154">**Design Notes**: By virtue of the compiler-generated `Deconstruct` as specified herein, and the specification for pattern-matching, a record declaration</span></span>
> ```cs
> public class Point(int X, int Y);
> ```
> <span data-ttu-id="9473c-155">將支援位置模式比對，如下所示</span><span class="sxs-lookup"><span data-stu-id="9473c-155">will support positional pattern-matching as follows</span></span>
> ```cs
> Point p = new Point(3, 4);
> if (p is Point(3, var y)) { // if X is 3
>     Console.WriteLine(y);   // print Y
> }
> ```

### <a name="record-type-declarations"></a><span data-ttu-id="9473c-156">記錄類型宣告</span><span class="sxs-lookup"><span data-stu-id="9473c-156">record type declarations</span></span>

<span data-ttu-id="9473c-157">已擴充 `class` 或 `struct` 宣告的語法以支援值參數;參數會變成類型的屬性：</span><span class="sxs-lookup"><span data-stu-id="9473c-157">The syntax for a `class` or `struct` declaration is extended to support value parameters; the parameters become properties of the type:</span></span>

```antlr
class_declaration
    : attributes? class_modifiers? 'partial'? 'class' identifier type_parameter_list?
      record_parameters? record_class_base? type_parameter_constraints_clauses? class_body
    ;

struct_declaration
    : attributes? struct_modifiers? 'partial'? 'struct' identifier type_parameter_list?
      record_parameters? struct_interfaces? type_parameter_constraints_clauses? struct_body
    ;

record_class_base
    : class_type record_base_arguments?
    | interface_type_list
    | class_type record_base_arguments? ',' interface_type_list
    ;

record_base_arguments
    : '(' argument_list? ')'
    ;

record_parameters
    : '(' record_parameter_list? ')'
    ;

record_parameter_list
    : record_parameter
    | record_parameter ',' record_parameter_list
    ;

record_parameter
    : attributes? type identifier record_property_name? default_argument?
    ;

record_property_name
    : ':' identifier
    ;

class_body
    : '{' class_member_declarations? '}'
    | ';'
    ;

struct_body
    : '{' struct_members_declarations? '}'
    | ';'
    ;
```

> <span data-ttu-id="9473c-158">**設計附注**：因為記錄類型通常很有用，而不需要在類別主體中明確宣告任何成員，所以我們會修改宣告的語法，讓主體只是分號。</span><span class="sxs-lookup"><span data-stu-id="9473c-158">**Design Notes**: Because record types are often useful without the need for any members explicitly declared in a class-body, we modify the syntax of the declaration to allow a body to be simply a semicolon.</span></span>

<span data-ttu-id="9473c-159">以*記錄參數*宣告的類別（結構）稱為*記錄類別*（*記錄結構*），其中之一是*記錄類型*。</span><span class="sxs-lookup"><span data-stu-id="9473c-159">A class (struct) declared with the *record-parameters* is called a *record class* (*record struct*), either of which is a *record type*.</span></span>

- <span data-ttu-id="9473c-160">[]**開啟問題**：我們必須在文法中包含*primary_constructor_body* ，讓它可以出現在記錄類型宣告中。</span><span class="sxs-lookup"><span data-stu-id="9473c-160">[ ] **Open issue**: We need to include *primary_constructor_body* in the grammar so that it can appear inside a record type declaration.</span></span>
- <span data-ttu-id="9473c-161">[]**開啟問題**：參數名稱的名稱衝突規則為何？</span><span class="sxs-lookup"><span data-stu-id="9473c-161">[ ] **Open issue**: What are the name conflict rules for the parameter names?</span></span> <span data-ttu-id="9473c-162">假設有一個不允許與型別參數或另一個*記錄參數*衝突。</span><span class="sxs-lookup"><span data-stu-id="9473c-162">Presumably one is not allowed to conflict with a type parameter or another *record-parameter*.</span></span>
- <span data-ttu-id="9473c-163">[]**開啟問題**：我們需要指定記錄參數的範圍。</span><span class="sxs-lookup"><span data-stu-id="9473c-163">[ ] **Open issue**: We need to specify the scope of the record-parameters.</span></span> <span data-ttu-id="9473c-164">可以使用它們的位置？</span><span class="sxs-lookup"><span data-stu-id="9473c-164">Where can they be used?</span></span> <span data-ttu-id="9473c-165">大概是在實例欄位初始化運算式中，而且至少*primary_constructor_body* 。</span><span class="sxs-lookup"><span data-stu-id="9473c-165">Presumably within instance field initializers and *primary_constructor_body* at least.</span></span>
- <span data-ttu-id="9473c-166">[]**開啟問題**：記錄類型宣告是否為部分？</span><span class="sxs-lookup"><span data-stu-id="9473c-166">[ ] **Open issue**: Can a record type declaration be partial?</span></span> <span data-ttu-id="9473c-167">若是如此，必須在每個元件上重複參數嗎？</span><span class="sxs-lookup"><span data-stu-id="9473c-167">If so, must the parameters be repeated on each part?</span></span>

#### <a name="members-of-a-record-type"></a><span data-ttu-id="9473c-168">記錄類型的成員</span><span class="sxs-lookup"><span data-stu-id="9473c-168">Members of a record type</span></span>

<span data-ttu-id="9473c-169">除了在*類別主體*中宣告的成員之外，記錄類型還有下列其他成員：</span><span class="sxs-lookup"><span data-stu-id="9473c-169">In addition to the members declared in the *class-body*, a record type has the following additional members:</span></span>

##### <a name="primary-constructor"></a><span data-ttu-id="9473c-170">主要的構造函式</span><span class="sxs-lookup"><span data-stu-id="9473c-170">Primary Constructor</span></span>

<span data-ttu-id="9473c-171">記錄類型具有 `public` 的函式，其簽章對應于類型宣告的值參數。</span><span class="sxs-lookup"><span data-stu-id="9473c-171">A record type has a `public` constructor whose signature corresponds to the value parameters of the type declaration.</span></span> <span data-ttu-id="9473c-172">這稱為類型的*主要*「函式」，並會隱藏隱含宣告的*預設函數*。</span><span class="sxs-lookup"><span data-stu-id="9473c-172">This is called the *primary constructor* for the type, and causes the implicitly declared *default constructor* to be suppressed.</span></span>

<span data-ttu-id="9473c-173">在執行時間，主要的函式</span><span class="sxs-lookup"><span data-stu-id="9473c-173">At runtime the primary constructor</span></span>

* <span data-ttu-id="9473c-174">針對對應至值參數的屬性，初始化編譯器產生的支援欄位（如果這些屬性是由編譯器提供，則為[請參閱 1.1.2](#1.1.2)）;請</span><span class="sxs-lookup"><span data-stu-id="9473c-174">initializes compiler-generated backing fields for the properties corresponding to the value parameters (if these properties are compiler-provided; [see 1.1.2](#1.1.2)); then</span></span>
* <span data-ttu-id="9473c-175">執行出現在*類別主體*中的實例欄位初始化運算式;然後</span><span class="sxs-lookup"><span data-stu-id="9473c-175">executes the instance field initializers appearing in the *class-body*; and then</span></span>
* <span data-ttu-id="9473c-176">叫用基類的構造函式：</span><span class="sxs-lookup"><span data-stu-id="9473c-176">invokes a base class constructor:</span></span>
    * <span data-ttu-id="9473c-177">如果*record_base_arguments*中有引數，則會叫用具有這些引數的多載解析所選取的基底函式;</span><span class="sxs-lookup"><span data-stu-id="9473c-177">If there are arguments in the *record_base_arguments*, a base constructor selected by overload resolution with these arguments is invoked;</span></span>
    * <span data-ttu-id="9473c-178">否則，會叫用不含引數的基底函式。</span><span class="sxs-lookup"><span data-stu-id="9473c-178">Otherwise a base constructor is invoked with no arguments.</span></span>
* <span data-ttu-id="9473c-179">依來源循序執行每個*primary_constructor_body*的主體（如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="9473c-179">executes the body of each *primary_constructor_body*, if any, in source order.</span></span>

- <span data-ttu-id="9473c-180">[]**開啟問題**：我們需要指定該順序，特別是跨部分的編譯單位。</span><span class="sxs-lookup"><span data-stu-id="9473c-180">[ ] **Open issue**: We need to specify that order, particularly across compilation units for partials.</span></span>
- <span data-ttu-id="9473c-181">[]**開啟問題**：我們需要指定每個明確宣告的函式都必須與主要的函數鏈連結。</span><span class="sxs-lookup"><span data-stu-id="9473c-181">[ ] **Open Issue**: We need to specify that every explicitly declared constructor must chain to the primary constructor.</span></span>
- <span data-ttu-id="9473c-182">[]**開啟問題**：是否允許在主要的函式上變更存取修飾詞？</span><span class="sxs-lookup"><span data-stu-id="9473c-182">[ ] **Open issue**: Should it be allowed to change the access modifier on the primary constructor?</span></span>
- <span data-ttu-id="9473c-183">[]**開啟問題**：在記錄結構中，沒有任何記錄參數會發生錯誤？</span><span class="sxs-lookup"><span data-stu-id="9473c-183">[ ] **Open issue**: In a record struct, it is an error for there to be no record parameters?</span></span>

##### <a name="primary-constructor-body"></a><span data-ttu-id="9473c-184">主要的函數主體</span><span class="sxs-lookup"><span data-stu-id="9473c-184">Primary constructor body</span></span>

```antlr
primary_constructor_body
    : attributes? constructor_modifiers? identifier block
    ;
```

<span data-ttu-id="9473c-185">*Primary_constructor_body*只能用在記錄類型宣告中。</span><span class="sxs-lookup"><span data-stu-id="9473c-185">A *primary_constructor_body* may only be used within a record type declaration.</span></span> <span data-ttu-id="9473c-186">*Primary_constructor_body*的*識別碼*應命名其宣告所在的記錄類型。</span><span class="sxs-lookup"><span data-stu-id="9473c-186">The *identifier* of a *primary_constructor_body* shall name the record type in which it is declared.</span></span>

<span data-ttu-id="9473c-187">*Primary_constructor_body*不會自行宣告成員，但可讓程式設計人員提供的屬性，並指定的存取記錄類型的主要程式設計函數。</span><span class="sxs-lookup"><span data-stu-id="9473c-187">The *primary_constructor_body* does not declare a member on its own, but is a way for the programmer to provide attributes for, and specify the access of, a record type's primary constructor.</span></span> <span data-ttu-id="9473c-188">它也可讓程式設計師提供在結構化記錄類型的實例時，將會執行的其他程式碼。</span><span class="sxs-lookup"><span data-stu-id="9473c-188">It also enables the programmer to provide additional code that will be executed when an instance of the record type is constructed.</span></span>

- <span data-ttu-id="9473c-189">[]**開啟問題**：我們應該注意結構預設的函式會略過此。</span><span class="sxs-lookup"><span data-stu-id="9473c-189">[ ] **Open issue**: We should note that a struct default constructor bypasses this.</span></span>
- <span data-ttu-id="9473c-190">[]**開啟問題**：我們應該指定初始化的執行順序。</span><span class="sxs-lookup"><span data-stu-id="9473c-190">[ ] **Open issue**: We should specify the execution order of initialization.</span></span>
- <span data-ttu-id="9473c-191">[]**開啟問題**：在非記錄類型宣告中，我們是否允許像是*primary_constructor_body* （假設沒有屬性和修飾詞），並將它視為實例欄位初始化運算式的程式碼？</span><span class="sxs-lookup"><span data-stu-id="9473c-191">[ ] **Open issue**: Should we allow something like a *primary_constructor_body* (presumably without attributes and modifiers) in a non-record type declaration, and treat it like we would the code of an instance field initializer?</span></span>

##### <a name="properties"></a><span data-ttu-id="9473c-192">屬性</span><span class="sxs-lookup"><span data-stu-id="9473c-192">Properties</span></span>

<span data-ttu-id="9473c-193">對於記錄類型宣告的每個記錄參數，都有一個對應的 `public` 屬性成員，其名稱和類型取自值參數聲明。</span><span class="sxs-lookup"><span data-stu-id="9473c-193">For each record parameter of a record type declaration there is a corresponding `public` property member whose name and type are taken from the value parameter declaration.</span></span> <span data-ttu-id="9473c-194">其名稱是*record_property_name*的*識別碼*（如果有的話）或*record_parameter*的*識別碼*，否則為。</span><span class="sxs-lookup"><span data-stu-id="9473c-194">Its name is the *identifier* of the *record_property_name*, if present, or the *identifier* of the *record_parameter* otherwise.</span></span> <span data-ttu-id="9473c-195">如果沒有具有 `get` 存取子的具象（非抽象）公用屬性，且已明確宣告或繼承此名稱和類型，則編譯器會產生，如下所示：</span><span class="sxs-lookup"><span data-stu-id="9473c-195">If no concrete (i.e. non-abstract) public property with a `get` accessor and with this name and type is explicitly declared or inherited, it is produced by the compiler as follows:</span></span>

* <span data-ttu-id="9473c-196">若為*記錄結構*或 `sealed`*記錄類別*：</span><span class="sxs-lookup"><span data-stu-id="9473c-196">For a *record struct* or a `sealed` *record class*:</span></span>
 * <span data-ttu-id="9473c-197">`private` `readonly` 欄位會產生為 `readonly` 屬性的支援欄位。</span><span class="sxs-lookup"><span data-stu-id="9473c-197">A `private` `readonly` field is produced as a backing field for a `readonly` property.</span></span> <span data-ttu-id="9473c-198">它的值會在結構化期間使用對應的主要函式參數值進行初始化。</span><span class="sxs-lookup"><span data-stu-id="9473c-198">Its value is initialized during construction with the value of the corresponding primary constructor parameter.</span></span>
 * <span data-ttu-id="9473c-199">屬性的 `get` 存取子會實作為傳回支援欄位的值。</span><span class="sxs-lookup"><span data-stu-id="9473c-199">The property's `get` accessor is implemented to return the value of the backing field.</span></span>
 * <span data-ttu-id="9473c-200">會覆寫每個「相符」的已繼承虛擬屬性的 `get` 存取子。</span><span class="sxs-lookup"><span data-stu-id="9473c-200">Each "matching" inherited virtual property's `get` accessor is overridden.</span></span>

> <span data-ttu-id="9473c-201">**設計附注**：換句話說，如果您擴充基類，或實作為宣告公用抽象屬性的介面，且其名稱和類型與記錄參數相同，則會覆寫或執行該屬性。</span><span class="sxs-lookup"><span data-stu-id="9473c-201">**Design notes**: In other words, if you extend a base class or implement an interface that declares a public abstract property with the same name and type as a record parameter, that property is overridden or implemented.</span></span>

- <span data-ttu-id="9473c-202">[]**開啟問題**：明確宣告屬性時，是否可以變更其上的存取修飾詞？</span><span class="sxs-lookup"><span data-stu-id="9473c-202">[ ] **Open issue**: Should it be possible to change the access modifier on a property when it is explicitly declared?</span></span>
- <span data-ttu-id="9473c-203">[]**開啟問題**：是否可以取代屬性的欄位？</span><span class="sxs-lookup"><span data-stu-id="9473c-203">[ ] **Open issue**: Should it be possible to substitute a field for a property?</span></span>

##### <a name="object-methods"></a><span data-ttu-id="9473c-204">物件方法</span><span class="sxs-lookup"><span data-stu-id="9473c-204">Object Methods</span></span>

<span data-ttu-id="9473c-205">若為*記錄結構*或 `sealed`*記錄類別*，除非使用者提供，否則編譯器會產生 `object.GetHashCode()` 和 `object.Equals(object)` 方法的執行。</span><span class="sxs-lookup"><span data-stu-id="9473c-205">For a *record struct* or a `sealed` *record class*, implementations of the methods `object.GetHashCode()` and `object.Equals(object)` are produced by the compiler unless provided by the user.</span></span>

- <span data-ttu-id="9473c-206">[]**開啟問題**：我們應精確指定其執行方式。</span><span class="sxs-lookup"><span data-stu-id="9473c-206">[ ] **Open issue**: We should precisely specify their implementation.</span></span>
- <span data-ttu-id="9473c-207">[]**開啟問題**：我們也應該加入記錄類型的介面 `IEquatable<T>`，並指定要提供的實作為。</span><span class="sxs-lookup"><span data-stu-id="9473c-207">[ ] **Open issue**: We should also add the interface `IEquatable<T>` for the record type and specify that implementations are provided.</span></span>
- <span data-ttu-id="9473c-208">[]**開啟問題**：我們也應該指定我們會執行每個 `IEquatable<T>.Equals`。</span><span class="sxs-lookup"><span data-stu-id="9473c-208">[ ] **Open issue**: We should also specify that we implement every `IEquatable<T>.Equals`.</span></span>
- <span data-ttu-id="9473c-209">[]**開啟問題**：我們應精確指定我們如何解決記錄繼承臉部中的 Equals 問題：特別是如何產生等號比較方法，使其為對稱、可轉移、反身等。</span><span class="sxs-lookup"><span data-stu-id="9473c-209">[ ] **Open issue**: We should specify precisely how we solve the problem of Equals in the face of record inheritance: specifically how we generate equality methods such that they are symmetric, transitive, reflexive, etc.</span></span>
- <span data-ttu-id="9473c-210">[]**開啟問題**：我們已建議為記錄類型執行 `operator ==` 和 `operator !=`。</span><span class="sxs-lookup"><span data-stu-id="9473c-210">[ ] **Open issue**: It has been proposed that we implement `operator ==` and `operator !=` for record types.</span></span>
- <span data-ttu-id="9473c-211">[]**開啟問題**：我們是否應該自動產生 `object.ToString`的執行？</span><span class="sxs-lookup"><span data-stu-id="9473c-211">[ ] **Open issue**: Should we auto-generate an implementation of `object.ToString`?</span></span>

##### `Deconstruct`

<span data-ttu-id="9473c-212">記錄類型具有編譯器產生的 `public` 方法 `void Deconstruct`，除非使用者提供任何簽章。</span><span class="sxs-lookup"><span data-stu-id="9473c-212">A record type has a compiler-generated `public` method `void Deconstruct` unless one with any signature is provided by the user.</span></span> <span data-ttu-id="9473c-213">每個參數都是 `out` 參數，其名稱和類型與記錄類型的對應參數相同。</span><span class="sxs-lookup"><span data-stu-id="9473c-213">Each parameter is an `out` parameter of the same name and type as the corresponding parameter of the record type.</span></span> <span data-ttu-id="9473c-214">編譯器提供的此方法的實值，必須將每個 `out` 參數指派為對應屬性的值。</span><span class="sxs-lookup"><span data-stu-id="9473c-214">The compiler-provided implementation of this method shall assign each `out` parameter with the value of the corresponding property.</span></span>

<span data-ttu-id="9473c-215">如需 `Deconstruct`的語法，請參閱模式比對[規格](csharp-8.0/patterns.md#positional-pattern)。</span><span class="sxs-lookup"><span data-stu-id="9473c-215">See [the pattern-matching specification](csharp-8.0/patterns.md#positional-pattern) for the semantics of `Deconstruct`.</span></span>

##### <a name="with-method"></a><span data-ttu-id="9473c-216">`With` 方法</span><span class="sxs-lookup"><span data-stu-id="9473c-216">`With` method</span></span>

<span data-ttu-id="9473c-217">除非宣告了名為 `With` 的使用者宣告成員，否則記錄類型會具有編譯器提供的方法，名為 `With` 其傳回類型為記錄類型本身，而且包含一個對應于每個*記錄參數*的值參數，其順序與這些參數出現在記錄類型宣告中的順序相同。</span><span class="sxs-lookup"><span data-stu-id="9473c-217">Unless there is a user-declared member named `With` declared, a record type has a compiler-provided method named `With` whose return type is the record type itself, and containing one value parameter corresponding to each *record-parameter* in the same order that these parameters appear in the record type declaration.</span></span> <span data-ttu-id="9473c-218">每個參數都必須有對應屬性的*呼叫端-接收者預設引數*。</span><span class="sxs-lookup"><span data-stu-id="9473c-218">Each parameter shall have a *caller-receiver default-argument* of the corresponding property.</span></span>

<span data-ttu-id="9473c-219">在 `abstract` 記錄類別中，編譯器提供的 `With` 方法是抽象的。</span><span class="sxs-lookup"><span data-stu-id="9473c-219">In an `abstract` record class, the compiler-provided `With` method is abstract.</span></span> <span data-ttu-id="9473c-220">在 record 結構或密封的記錄類別中，編譯器提供的 `With` 方法是 `sealed`。</span><span class="sxs-lookup"><span data-stu-id="9473c-220">In a record struct, or a sealed record class, the compiler-provided `With` method is `sealed`.</span></span> <span data-ttu-id="9473c-221">否則，編譯器提供的 `With` 方法是「虛擬」，而且它的執行應會傳回新的實例，其方式是叫用主要的函式，並將參數當做引數，以從參數建立新的實例，並傳回新的實例。</span><span class="sxs-lookup"><span data-stu-id="9473c-221">Otherwise the compiler-provided `With` method is \`virtual and its implementation shall return a new instance produced by invoking the primary constructor with the parameters as arguments to create a new instance from the parameters, and return that new instance.</span></span>

- <span data-ttu-id="9473c-222">[]**開啟問題**：我們也應該指定在哪些條件下，我們會覆寫或執行繼承的虛擬 `With` 方法，或從實作為介面的 `With` 方法。</span><span class="sxs-lookup"><span data-stu-id="9473c-222">[ ] **Open issue**: We should also specify under what conditions we override or implement inherited virtual `With` methods or `With` methods from implemented interfaces.</span></span>
- <span data-ttu-id="9473c-223">[]**開啟問題**：我們應該會說，當我們繼承非虛擬 `With` 方法時，會發生什麼事。</span><span class="sxs-lookup"><span data-stu-id="9473c-223">[ ] **Open issue**: We should say what happens when we inherit a non-virtual `With` method.</span></span>

> <span data-ttu-id="9473c-224">**設計附注**：因為記錄類型預設為不可變，所以 `With` 方法會提供一種方式來建立新的實例，其與現有的實例相同，但已選取的屬性會指定新的值。</span><span class="sxs-lookup"><span data-stu-id="9473c-224">**Design notes**: Because record types are by default immutable, the `With` method provides a way of creating a new instance that is the same as an existing instance but with selected properties given new values.</span></span> <span data-ttu-id="9473c-225">例如，假設</span><span class="sxs-lookup"><span data-stu-id="9473c-225">For example, given</span></span>
> ```cs
> public class Point(int X, int Y);
> ```
> <span data-ttu-id="9473c-226">有一個編譯器提供的成員</span><span class="sxs-lookup"><span data-stu-id="9473c-226">there is a compiler-provided member</span></span>
> ```cs
>     public virtual Point With(int X = this.X, int Y = this.Y) => new Point(X, Y);
> ```
> <span data-ttu-id="9473c-227">這會啟用記錄類型的變數</span><span class="sxs-lookup"><span data-stu-id="9473c-227">Which enables an variable of the record type</span></span>
> ```cs
> var p = new Point(3, 4);
> ```
> <span data-ttu-id="9473c-228">要取代為具有一或多個屬性不同的實例</span><span class="sxs-lookup"><span data-stu-id="9473c-228">to be replaced with an instance that has one or more properties different</span></span>
> ```cs
>     p = p.With(X: 5);
> ```
> <span data-ttu-id="9473c-229">這也可以使用*with_expression*來表示：</span><span class="sxs-lookup"><span data-stu-id="9473c-229">This can also be expressed using the *with_expression*:</span></span>
> ```cs
>     p = p with { X = 5 };
> ```

### <a name="examples"></a><span data-ttu-id="9473c-230">範例</span><span class="sxs-lookup"><span data-stu-id="9473c-230">Examples</span></span>

#### <a name="compatibility-of-record-types"></a><span data-ttu-id="9473c-231">記錄類型的相容性</span><span class="sxs-lookup"><span data-stu-id="9473c-231">Compatibility of record types</span></span>

<span data-ttu-id="9473c-232">因為程式設計人員可以將成員加入至記錄類型宣告，所以通常可以變更記錄元素的集合，而不會影響現有的用戶端。</span><span class="sxs-lookup"><span data-stu-id="9473c-232">Because the programmer can add members to a record type declaration, it is often possible to change the set of record elements without affecting existing clients.</span></span> <span data-ttu-id="9473c-233">例如，給定記錄類型的初始版本</span><span class="sxs-lookup"><span data-stu-id="9473c-233">For example, given an initial version of a record type</span></span>

```cs
// v1
public class Person(string Name, DateTime DateOfBirth);
```

<span data-ttu-id="9473c-234">記錄類型的新元素可以相容性加入至類型的下一個修訂中，而不會影響二進位或來源的相容性：</span><span class="sxs-lookup"><span data-stu-id="9473c-234">A new element of the record type can be compatibly added in the next revision of the type without affecting binary or source compatibility:</span></span>

```cs
// v2
public class Person(string Name, DateTime DateOfBirth, string HomeTown)
{
    // Note: below operations added to retain binary compatibility with v1
    public Person(string Name, DateTime DateOfBirth) : this(Name, DateOfBirth, string.Empty) {}
    public static void operator is(Person self, out string Name, out DateTime DateOfBirth)
        { Name = self.Name; DateOfBirth = self.DateOfBirth; }
    public Person With(string Name, DateTime DateOfBirth) => new Person(Name, DateOfBirth);
}
```

#### <a name="record-struct-example"></a><span data-ttu-id="9473c-235">record 結構範例</span><span class="sxs-lookup"><span data-stu-id="9473c-235">record struct example</span></span>

<span data-ttu-id="9473c-236">此記錄結構</span><span class="sxs-lookup"><span data-stu-id="9473c-236">This record struct</span></span>

```cs
public struct Pair(object First, object Second);
```

<span data-ttu-id="9473c-237">會轉譯為此程式碼</span><span class="sxs-lookup"><span data-stu-id="9473c-237">is translated to this code</span></span>

```cs
public struct Pair : IEquatable<Pair>
{
    public object First { get; }
    public object Second { get; }
    public Pair(object First, object Second)
    {
        this.First = First;
        this.Second = Second;
    }
    public bool Equals(Pair other) // for IEquatable<Pair>
    {
        return Equals(First, other.First) && Equals(Second, other.Second);
    }
    public override bool Equals(object other)
    {
        return (other as Pair)?.Equals(this) == true;
    }
    public override int GetHashCode()
    {
        return (First?.GetHashCode()*17 + Second?.GetHashCode()).GetValueOrDefault();
    }
    public Pair With(object First = this.First, object Second = this.Second) => new Pair(First, Second);
    public void Deconstruct(out object First, out object Second)
    {
        First = self.First;
        Second = self.Second;
    }
}
```

- <span data-ttu-id="9473c-238">[]**開啟問題**：是否應將 Equals （配對其他）的實作為配對的公用成員？</span><span class="sxs-lookup"><span data-stu-id="9473c-238">[ ] **Open issue**: should the implementation of Equals(Pair other) be a public member of Pair?</span></span>
- <span data-ttu-id="9473c-239">[]**開啟問題**：此 `Equals` 的執行在面對繼承時不對稱。</span><span class="sxs-lookup"><span data-stu-id="9473c-239">[ ] **Open issue**: This implementation of `Equals` is not symmetric in the face of inheritance.</span></span>
- <span data-ttu-id="9473c-240">[]**開啟問題**：記錄是否應宣告 `operator ==` 和 `operator !=`？</span><span class="sxs-lookup"><span data-stu-id="9473c-240">[ ] **Open issue**: Should a record declare `operator ==` and `operator !=`?</span></span>

> <span data-ttu-id="9473c-241">**設計附注**：因為一種記錄類型可以繼承自另一個，而且此 `Equals` 的執行在這種情況下不會是對稱的，所以不正確。</span><span class="sxs-lookup"><span data-stu-id="9473c-241">**Design notes**: Because one record type can inherit from another, and this implementation of `Equals` would not be symmetric in that case, it is not correct.</span></span> <span data-ttu-id="9473c-242">我們建議以這種方式來執行相等：</span><span class="sxs-lookup"><span data-stu-id="9473c-242">We propose to implement equality this way:</span></span>
> ```cs
>     public bool Equals(Pair other) // for IEquatable<Pair>
>     {
>         return other != null && EqualityContract == other.EqualityContract &&
>             Equals(First, other.First) && Equals(Second, other.Second);
>     }
>     protected virtual Type EqualityContract => typeof(Pair);
> ```
> <span data-ttu-id="9473c-243">衍生的記錄會 `override EqualityContract`。</span><span class="sxs-lookup"><span data-stu-id="9473c-243">Derived records would `override EqualityContract`.</span></span> <span data-ttu-id="9473c-244">較不具吸引力的替代方法是限制繼承。</span><span class="sxs-lookup"><span data-stu-id="9473c-244">The less attractive alternative is to restrict inheritance.</span></span>

#### <a name="sealed-record-example"></a><span data-ttu-id="9473c-245">密封記錄範例</span><span class="sxs-lookup"><span data-stu-id="9473c-245">sealed record example</span></span>

<span data-ttu-id="9473c-246">這個密封的記錄類別</span><span class="sxs-lookup"><span data-stu-id="9473c-246">This sealed record class</span></span>

```cs
public sealed class Student(string Name, decimal Gpa);
```

<span data-ttu-id="9473c-247">會轉譯為此程式碼</span><span class="sxs-lookup"><span data-stu-id="9473c-247">is translated into this code</span></span>

```cs
public sealed class Student : IEquatable<Student>
{
    public string Name { get; }
    public decimal Gpa { get; }
    public Student(string Name, decimal Gpa)
    {
        this.Name = Name;
        this.Gpa = Gpa;
    }
    public bool Equals(Student other) // for IEquatable<Student>
    {
        return other != null && Equals(Name, other.Name) && Equals(Gpa, other.Gpa);
    }
    public override bool Equals(object other)
    {
        return this.Equals(other as Student);
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()*17 + Gpa?.GetHashCode()).GetValueOrDefault();
    }
    public Student With(string Name = this.Name, decimal Gpa = this.Gpa) => new Student(Name, Gpa);
    public void Deconstruct(out string Name, out decimal Gpa)
    {
        Name = self.Name;
        Gpa = self.Gpa;
    }
}
```

#### <a name="abstract-record-class-example"></a><span data-ttu-id="9473c-248">抽象記錄類別範例</span><span class="sxs-lookup"><span data-stu-id="9473c-248">abstract record class example</span></span>

<span data-ttu-id="9473c-249">此抽象記錄類別</span><span class="sxs-lookup"><span data-stu-id="9473c-249">This abstract record class</span></span>

```cs
public abstract class Person(string Name);
```

<span data-ttu-id="9473c-250">會轉譯為此程式碼</span><span class="sxs-lookup"><span data-stu-id="9473c-250">is translated into this code</span></span>

```cs
public abstract class Person : IEquatable<Person>
{
    public string Name { get; }
    public Person(string Name)
    {
        this.Name = Name;
    }
    public bool Equals(Person other)
    {
        return other != null && Equals(Name, other.Name);
    }
    public override bool Equals(object other)
    {
        return Equals(other as Person);
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()).GetValueOrDefault();
    }
    public abstract Person With(string Name = this.Name);
    public void Deconstruct(Person self, out string Name)
    {
        Name = self.Name;
    }
}
```

#### <a name="combining-abstract-and-sealed-records"></a><span data-ttu-id="9473c-251">結合抽象和密封記錄</span><span class="sxs-lookup"><span data-stu-id="9473c-251">combining abstract and sealed records</span></span>

<span data-ttu-id="9473c-252">假設 `Person` 上述的抽象記錄類別，則此密封記錄類別</span><span class="sxs-lookup"><span data-stu-id="9473c-252">Given the abstract record class `Person` above, this sealed record class</span></span>

```cs
public sealed class Student(string Name, decimal Gpa) : Person(Name);
```

<span data-ttu-id="9473c-253">會轉譯為此程式碼</span><span class="sxs-lookup"><span data-stu-id="9473c-253">is translated into this code</span></span>

```cs
public sealed class Student : Person, IEquatable<Student>
{
    public override string Name { get; }
    public decimal Gpa { get; }
    public Student(string Name, decimal Gpa) : base(Name)
    {
        this.Name = Name;
        this.Gpa = Gpa;
    }
    public override bool Equals(Student other) // for IEquatable<Student>
    {
        return Equals(Name, other.Name) && Equals(Gpa, other.Gpa);
    }
    public bool Equals(Person other) // for IEquatable<Person>
    {
        return (other as Student)?.Equals(this) == true;
    }
    public override bool Equals(object other)
    {
        return (other as Student)?.Equals(this) == true;
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()*17 + Gpa.GetHashCode()).GetValueOrDefault();
    }
    public Student With(string Name = this.Name, decimal Gpa = this.Gpa) => new Student(Name, Gpa);
    public override Person With(string Name = this.Name) => new Student(Name, Gpa);
    public void Deconstruct(Student self, out string Name, out decimal Gpa)
    {
        Name = self.Name;
        Gpa = self.Gpa;
    }
}
```

## <a name="drawbacks"></a><span data-ttu-id="9473c-254">缺點</span><span class="sxs-lookup"><span data-stu-id="9473c-254">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="9473c-255">就像任何語言功能一樣，我們還必須問您，是否重金換回其他語言的複雜性，以提供給可從功能獲益C#的程式主體。</span><span class="sxs-lookup"><span data-stu-id="9473c-255">As with any language feature, we must question whether the additional complexity to the language is repaid in the additional clarity offered to the body of C# programs that would benefit from the feature.</span></span>

## <a name="alternatives"></a><span data-ttu-id="9473c-256">替代方案</span><span class="sxs-lookup"><span data-stu-id="9473c-256">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="9473c-257">我們已考慮*primary constructors*在6中C#新增主要的函式。</span><span class="sxs-lookup"><span data-stu-id="9473c-257">We considered adding *primary constructors* in C# 6.</span></span> <span data-ttu-id="9473c-258">雖然它們會佔用與此提案相同的語法介面，但我們發現它們的優點是記錄所提供的優勢。</span><span class="sxs-lookup"><span data-stu-id="9473c-258">Although they occupy the same syntactic surface as this proposal, we found that they fell short of the advantages offered by records.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="9473c-259">未解決的問題</span><span class="sxs-lookup"><span data-stu-id="9473c-259">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="9473c-260">提出的問題會出現在提案的主體中。</span><span class="sxs-lookup"><span data-stu-id="9473c-260">Open questions appear in the body of the proposal.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="9473c-261">設計會議</span><span class="sxs-lookup"><span data-stu-id="9473c-261">Design meetings</span></span>

<span data-ttu-id="9473c-262">TBD</span><span class="sxs-lookup"><span data-stu-id="9473c-262">TBD</span></span>
