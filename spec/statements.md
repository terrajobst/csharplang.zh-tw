---
ms.openlocfilehash: 7248a91976c479dc1b6b64b799639635617a7bec
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704048"
---
# <a name="statements"></a><span data-ttu-id="2c009-101">陳述式</span><span class="sxs-lookup"><span data-stu-id="2c009-101">Statements</span></span>

<span data-ttu-id="2c009-102">C#提供各種語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-102">C# provides a variety of statements.</span></span> <span data-ttu-id="2c009-103">對於以 C 和C++編寫的開發人員而言，大部分的語句都是熟悉的。</span><span class="sxs-lookup"><span data-stu-id="2c009-103">Most of these statements will be familiar to developers who have programmed in C and C++.</span></span>

```antlr
statement
    : labeled_statement
    | declaration_statement
    | embedded_statement
    ;

embedded_statement
    : block
    | empty_statement
    | expression_statement
    | selection_statement
    | iteration_statement
    | jump_statement
    | try_statement
    | checked_statement
    | unchecked_statement
    | lock_statement
    | using_statement
    | yield_statement
    | embedded_statement_unsafe
    ;
```

<span data-ttu-id="2c009-104">*Embedded_statement*語法會用於出現在其他語句中的語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-104">The *embedded_statement* nonterminal is used for statements that appear within other statements.</span></span> <span data-ttu-id="2c009-105">使用*embedded_statement*而不是*語句*，會排除在這些內容中使用宣告語句和標記語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-105">The use of *embedded_statement* rather than *statement* excludes the use of declaration statements and labeled statements in these contexts.</span></span> <span data-ttu-id="2c009-106">範例</span><span class="sxs-lookup"><span data-stu-id="2c009-106">The example</span></span>
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
<span data-ttu-id="2c009-107">會導致編譯時期錯誤，因為 `if` 語句需要*embedded_statement* ，而不是其 if 分支的*語句*。</span><span class="sxs-lookup"><span data-stu-id="2c009-107">results in a compile-time error because an `if` statement requires an *embedded_statement* rather than a *statement* for its if branch.</span></span> <span data-ttu-id="2c009-108">如果允許此程式碼，則會宣告`i`變數，但永遠不會使用它。</span><span class="sxs-lookup"><span data-stu-id="2c009-108">If this code were permitted, then the variable `i` would be declared, but it could never be used.</span></span> <span data-ttu-id="2c009-109">不過，請注意，藉由`i`將宣告放在區塊中，此範例是有效的。</span><span class="sxs-lookup"><span data-stu-id="2c009-109">Note, however, that by placing `i`'s declaration in a block, the example is valid.</span></span>

## <a name="end-points-and-reachability"></a><span data-ttu-id="2c009-110">端點和連線能力</span><span class="sxs-lookup"><span data-stu-id="2c009-110">End points and reachability</span></span>

<span data-ttu-id="2c009-111">每個語句都有一個***結束點***。</span><span class="sxs-lookup"><span data-stu-id="2c009-111">Every statement has an ***end point***.</span></span> <span data-ttu-id="2c009-112">就直覺而言，語句的結束點是緊接在語句後面的位置。</span><span class="sxs-lookup"><span data-stu-id="2c009-112">In intuitive terms, the end point of a statement is the location that immediately follows the statement.</span></span> <span data-ttu-id="2c009-113">複合陳述式的執行規則（包含內嵌語句的語句）會指定當控制項到達內嵌語句的結束點時，所採取的動作。</span><span class="sxs-lookup"><span data-stu-id="2c009-113">The execution rules for composite statements (statements that contain embedded statements) specify the action that is taken when control reaches the end point of an embedded statement.</span></span> <span data-ttu-id="2c009-114">例如，當控制項到達區塊中語句的結束點時，控制權會轉移到區塊中的下一個語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-114">For example, when control reaches the end point of a statement in a block, control is transferred to the next statement in the block.</span></span>

<span data-ttu-id="2c009-115">如果執行可能會觸達語句，則表示語句***可供連線。***</span><span class="sxs-lookup"><span data-stu-id="2c009-115">If a statement can possibly be reached by execution, the statement is said to be ***reachable***.</span></span> <span data-ttu-id="2c009-116">相反地，如果不可能執行語句，則會將***語句視為無法連線。***</span><span class="sxs-lookup"><span data-stu-id="2c009-116">Conversely, if there is no possibility that a statement will be executed, the statement is said to be ***unreachable***.</span></span>

<span data-ttu-id="2c009-117">在範例中</span><span class="sxs-lookup"><span data-stu-id="2c009-117">In the example</span></span>
```csharp
void F() {
    Console.WriteLine("reachable");
    goto Label;
    Console.WriteLine("unreachable");
    Label:
    Console.WriteLine("reachable");
}
```
<span data-ttu-id="2c009-118">因為不可能執行`Console.WriteLine`語句，所以無法連接的第二個調用。</span><span class="sxs-lookup"><span data-stu-id="2c009-118">the second invocation of `Console.WriteLine` is unreachable because there is no possibility that the statement will be executed.</span></span>

<span data-ttu-id="2c009-119">如果編譯器判斷無法連線到語句，就會回報警告。</span><span class="sxs-lookup"><span data-stu-id="2c009-119">A warning is reported if the compiler determines that a statement is unreachable.</span></span> <span data-ttu-id="2c009-120">這對於無法連線的語句而言特別不是錯誤。</span><span class="sxs-lookup"><span data-stu-id="2c009-120">It is specifically not an error for a statement to be unreachable.</span></span>

<span data-ttu-id="2c009-121">為了判斷是否可連線到特定的語句或端點，編譯器會根據針對每個語句所定義的可連線性規則來執行流程分析。</span><span class="sxs-lookup"><span data-stu-id="2c009-121">To determine whether a particular statement or end point is reachable, the compiler performs flow analysis according to the reachability rules defined for each statement.</span></span> <span data-ttu-id="2c009-122">流程分析會考慮常數運算式（[常數運算式](expressions.md#constant-expressions)）的值，以控制語句的行為，但不會考慮非常數運算式的可能值。</span><span class="sxs-lookup"><span data-stu-id="2c009-122">The flow analysis takes into account the values of constant expressions ([Constant expressions](expressions.md#constant-expressions)) that control the behavior of statements, but the possible values of non-constant expressions are not considered.</span></span> <span data-ttu-id="2c009-123">換句話說，針對控制流程分析的用途，會將指定類型的非常數運算式視為具有該類型的任何可能值。</span><span class="sxs-lookup"><span data-stu-id="2c009-123">In other words, for purposes of control flow analysis, a non-constant expression of a given type is considered to have any possible value of that type.</span></span>

<span data-ttu-id="2c009-124">在範例中</span><span class="sxs-lookup"><span data-stu-id="2c009-124">In the example</span></span>
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
<span data-ttu-id="2c009-125">`if`語句的布林運算式是常數運算式，因為`==`運算子的兩個運算元都是常數。</span><span class="sxs-lookup"><span data-stu-id="2c009-125">the boolean expression of the `if` statement is a constant expression because both operands of the `==` operator are constants.</span></span> <span data-ttu-id="2c009-126">當常數運算式在編譯時期進行評估時，如果產生值`false`，則會將`Console.WriteLine`調用視為無法存取。</span><span class="sxs-lookup"><span data-stu-id="2c009-126">As the constant expression is evaluated at compile-time, producing the value `false`, the `Console.WriteLine` invocation is considered unreachable.</span></span> <span data-ttu-id="2c009-127">不過，如果`i`變更為本機變數</span><span class="sxs-lookup"><span data-stu-id="2c009-127">However, if `i` is changed to be a local variable</span></span>
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
<span data-ttu-id="2c009-128">叫用會被視為可連線，但實際上永遠不會執行此調用。`Console.WriteLine`</span><span class="sxs-lookup"><span data-stu-id="2c009-128">the `Console.WriteLine` invocation is considered reachable, even though, in reality, it will never be executed.</span></span>

<span data-ttu-id="2c009-129">函式成員的*區塊*一律視為可連線。</span><span class="sxs-lookup"><span data-stu-id="2c009-129">The *block* of a function member is always considered reachable.</span></span> <span data-ttu-id="2c009-130">藉由連續評估區塊中每個語句的可連線性規則，就可以判斷任何給定語句的可連線性。</span><span class="sxs-lookup"><span data-stu-id="2c009-130">By successively evaluating the reachability rules of each statement in a block, the reachability of any given statement can be determined.</span></span>

<span data-ttu-id="2c009-131">在範例中</span><span class="sxs-lookup"><span data-stu-id="2c009-131">In the example</span></span>
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
<span data-ttu-id="2c009-132">第二個`Console.WriteLine`的可連線性決定如下：</span><span class="sxs-lookup"><span data-stu-id="2c009-132">the reachability of the second `Console.WriteLine` is determined as follows:</span></span>

*  <span data-ttu-id="2c009-133">可以連線`Console.WriteLine`到第一個運算式語句，因為可以連接`F`到方法的區塊。</span><span class="sxs-lookup"><span data-stu-id="2c009-133">The first `Console.WriteLine` expression statement is reachable because the block of the `F` method is reachable.</span></span>
*  <span data-ttu-id="2c009-134">可以連線到第一個`Console.WriteLine`運算式語句的結束點，因為可以連接該語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-134">The end point of the first `Console.WriteLine` expression statement is reachable because that statement is reachable.</span></span>
*  <span data-ttu-id="2c009-135">可以`if`連線到語句，因為可連線到第一個`Console.WriteLine`運算式語句的結束點。</span><span class="sxs-lookup"><span data-stu-id="2c009-135">The `if` statement is reachable because the end point of the first `Console.WriteLine` expression statement is reachable.</span></span>
*  <span data-ttu-id="2c009-136">第二`Console.WriteLine`個運算式語句可以連接，因為`if`語句的布林運算式沒有常數值`false`。</span><span class="sxs-lookup"><span data-stu-id="2c009-136">The second `Console.WriteLine` expression statement is reachable because the boolean expression of the `if` statement does not have the constant value `false`.</span></span>

<span data-ttu-id="2c009-137">在兩種情況下，如果語句的結束點可供存取，就會發生編譯時期錯誤：</span><span class="sxs-lookup"><span data-stu-id="2c009-137">There are two situations in which it is a compile-time error for the end point of a statement to be reachable:</span></span>

*  <span data-ttu-id="2c009-138">`switch`因為語句不允許參數區段「落到」下一個參數區段，所以參數區段之語句清單的結束點就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2c009-138">Because the `switch` statement does not permit a switch section to "fall through" to the next switch section, it is a compile-time error for the end point of the statement list of a switch section to be reachable.</span></span> <span data-ttu-id="2c009-139">如果發生此錯誤，通常表示`break`遺漏語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-139">If this error occurs, it is typically an indication that a `break` statement is missing.</span></span>
*  <span data-ttu-id="2c009-140">如果函式成員的區塊結束點會計算可連線的值，則會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2c009-140">It is a compile-time error for the end point of the block of a function member that computes a value to be reachable.</span></span> <span data-ttu-id="2c009-141">如果發生此錯誤，通常`return`表示遺漏語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-141">If this error occurs, it typically is an indication that a `return` statement is missing.</span></span>

## <a name="blocks"></a><span data-ttu-id="2c009-142">區塊</span><span class="sxs-lookup"><span data-stu-id="2c009-142">Blocks</span></span>

<span data-ttu-id="2c009-143">「區塊」可允許在許可單一陳述式的內容中撰寫多個陳述式。</span><span class="sxs-lookup"><span data-stu-id="2c009-143">A *block* permits multiple statements to be written in contexts where a single statement is allowed.</span></span>

```antlr
block
    : '{' statement_list? '}'
    ;
```

<span data-ttu-id="2c009-144">*區塊*是由選擇性的*statement_list* （[語句清單](statements.md#statement-lists)）所組成，以大括弧括住。</span><span class="sxs-lookup"><span data-stu-id="2c009-144">A *block* consists of an optional *statement_list* ([Statement lists](statements.md#statement-lists)), enclosed in braces.</span></span> <span data-ttu-id="2c009-145">如果省略語句清單，則區塊會被視為空白。</span><span class="sxs-lookup"><span data-stu-id="2c009-145">If the statement list is omitted, the block is said to be empty.</span></span>

<span data-ttu-id="2c009-146">區塊可以包含宣告語句（宣告[語句](statements.md#declaration-statements)）。</span><span class="sxs-lookup"><span data-stu-id="2c009-146">A block may contain declaration statements ([Declaration statements](statements.md#declaration-statements)).</span></span> <span data-ttu-id="2c009-147">區塊中宣告的區域變數或常數的範圍是區塊。</span><span class="sxs-lookup"><span data-stu-id="2c009-147">The scope of a local variable or constant declared in a block is the block.</span></span>

<span data-ttu-id="2c009-148">區塊的執行方式如下：</span><span class="sxs-lookup"><span data-stu-id="2c009-148">A block is executed as follows:</span></span>

*  <span data-ttu-id="2c009-149">如果區塊是空的，控制權就會傳送到區塊的結束點。</span><span class="sxs-lookup"><span data-stu-id="2c009-149">If the block is empty, control is transferred to the end point of the block.</span></span>
*  <span data-ttu-id="2c009-150">如果區塊不是空的，控制權就會傳送至語句清單。</span><span class="sxs-lookup"><span data-stu-id="2c009-150">If the block is not empty, control is transferred to the statement list.</span></span> <span data-ttu-id="2c009-151">當控制項到達語句清單的終點時，控制權會轉移到區塊的結束點。</span><span class="sxs-lookup"><span data-stu-id="2c009-151">When and if control reaches the end point of the statement list, control is transferred to the end point of the block.</span></span>

<span data-ttu-id="2c009-152">如果可以連線到區塊本身，則可以連接到區塊的語句清單。</span><span class="sxs-lookup"><span data-stu-id="2c009-152">The statement list of a block is reachable if the block itself is reachable.</span></span>

<span data-ttu-id="2c009-153">如果區塊是空的或語句清單的結束點可供連線，則區塊的結束點可供連線。</span><span class="sxs-lookup"><span data-stu-id="2c009-153">The end point of a block is reachable if the block is empty or if the end point of the statement list is reachable.</span></span>

<span data-ttu-id="2c009-154">包含一個或多個`yield`語句（[yield 語句](statements.md#the-yield-statement)）的區塊稱為 iterator 區塊。</span><span class="sxs-lookup"><span data-stu-id="2c009-154">A *block* that contains one or more `yield` statements ([The yield statement](statements.md#the-yield-statement)) is called an iterator block.</span></span> <span data-ttu-id="2c009-155">Iterator 區塊是用來將函式成員當做反覆運算器（[反覆運算](classes.md#iterators)器）來執行。</span><span class="sxs-lookup"><span data-stu-id="2c009-155">Iterator blocks are used to implement function members as iterators ([Iterators](classes.md#iterators)).</span></span> <span data-ttu-id="2c009-156">有一些額外的限制適用于 iterator 區塊：</span><span class="sxs-lookup"><span data-stu-id="2c009-156">Some additional restrictions apply to iterator blocks:</span></span>

*  <span data-ttu-id="2c009-157">`return`語句會出現在 iterator 區塊中（但`yield return`允許語句），這是編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2c009-157">It is a compile-time error for a `return` statement to appear in an iterator block (but `yield return` statements are permitted).</span></span>
*  <span data-ttu-id="2c009-158">反覆運算器區塊包含不安全內容（[不安全](unsafe-code.md#unsafe-contexts)的內容）時，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2c009-158">It is a compile-time error for an iterator block to contain an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span> <span data-ttu-id="2c009-159">反覆運算器區塊一律會定義安全的內容，即使其宣告是嵌套在不安全的內容中。</span><span class="sxs-lookup"><span data-stu-id="2c009-159">An iterator block always defines a safe context, even when its declaration is nested in an unsafe context.</span></span>

### <a name="statement-lists"></a><span data-ttu-id="2c009-160">語句清單</span><span class="sxs-lookup"><span data-stu-id="2c009-160">Statement lists</span></span>

<span data-ttu-id="2c009-161">***語句清單***是由一或多個依序寫入的語句所組成。</span><span class="sxs-lookup"><span data-stu-id="2c009-161">A ***statement list*** consists of one or more statements written in sequence.</span></span> <span data-ttu-id="2c009-162">語句清單會出現在*區塊*s （[區塊](statements.md#blocks)）和*switch_block*s （[switch 語句](statements.md#the-switch-statement)）中。</span><span class="sxs-lookup"><span data-stu-id="2c009-162">Statement lists occur in *block*s ([Blocks](statements.md#blocks)) and in *switch_block*s ([The switch statement](statements.md#the-switch-statement)).</span></span>

```antlr
statement_list
    : statement+
    ;
```

<span data-ttu-id="2c009-163">語句清單是藉由將控制項傳輸至第一個語句來執行。</span><span class="sxs-lookup"><span data-stu-id="2c009-163">A statement list is executed by transferring control to the first statement.</span></span> <span data-ttu-id="2c009-164">當控制項到達語句的結束點時，控制權會轉移到下一個語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-164">When and if control reaches the end point of a statement, control is transferred to the next statement.</span></span> <span data-ttu-id="2c009-165">當控制項到達最後一個語句的終點時，控制權會轉移到語句清單的結束點。</span><span class="sxs-lookup"><span data-stu-id="2c009-165">When and if control reaches the end point of the last statement, control is transferred to the end point of the statement list.</span></span>

<span data-ttu-id="2c009-166">如果下列至少一項為真，則語句清單中的語句可連線：</span><span class="sxs-lookup"><span data-stu-id="2c009-166">A statement in a statement list is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="2c009-167">語句是第一個語句，而語句清單本身是可連線的。</span><span class="sxs-lookup"><span data-stu-id="2c009-167">The statement is the first statement and the statement list itself is reachable.</span></span>
*  <span data-ttu-id="2c009-168">可以觸達上述語句的結束點。</span><span class="sxs-lookup"><span data-stu-id="2c009-168">The end point of the preceding statement is reachable.</span></span>
*  <span data-ttu-id="2c009-169">語句是一個標記的語句，而標籤是由`goto`可連接的語句所參考。</span><span class="sxs-lookup"><span data-stu-id="2c009-169">The statement is a labeled statement and the label is referenced by a reachable `goto` statement.</span></span>

<span data-ttu-id="2c009-170">如果清單中的最後一個語句的結束點可供連線，則語句清單的結束點就會是可到達的。</span><span class="sxs-lookup"><span data-stu-id="2c009-170">The end point of a statement list is reachable if the end point of the last statement in the list is reachable.</span></span>

## <a name="the-empty-statement"></a><span data-ttu-id="2c009-171">空陳述式</span><span class="sxs-lookup"><span data-stu-id="2c009-171">The empty statement</span></span>

<span data-ttu-id="2c009-172">*Empty_statement*不會執行任何操作。</span><span class="sxs-lookup"><span data-stu-id="2c009-172">An *empty_statement* does nothing.</span></span>

```antlr
empty_statement
    : ';'
    ;
```

<span data-ttu-id="2c009-173">當需要語句的內容中沒有要執行的作業時，會使用空的語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-173">An empty statement is used when there are no operations to perform in a context where a statement is required.</span></span>

<span data-ttu-id="2c009-174">執行空的語句只會將控制權轉移到語句的結束點。</span><span class="sxs-lookup"><span data-stu-id="2c009-174">Execution of an empty statement simply transfers control to the end point of the statement.</span></span> <span data-ttu-id="2c009-175">因此，如果可以連線到空的語句，就可以觸達空語句的結束點。</span><span class="sxs-lookup"><span data-stu-id="2c009-175">Thus, the end point of an empty statement is reachable if the empty statement is reachable.</span></span>

<span data-ttu-id="2c009-176">在撰寫`while`具有 null 主體的語句時，可以使用空的語句：</span><span class="sxs-lookup"><span data-stu-id="2c009-176">An empty statement can be used when writing a `while` statement with a null body:</span></span>
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

<span data-ttu-id="2c009-177">此外，空的語句也可以用來在區塊的結尾 "`}`" 之前宣告標籤：</span><span class="sxs-lookup"><span data-stu-id="2c009-177">Also, an empty statement can be used to declare a label just before the closing "`}`" of a block:</span></span>
```csharp
void F() {
    ...
    if (done) goto exit;
    ...
    exit: ;
}
```

## <a name="labeled-statements"></a><span data-ttu-id="2c009-178">標記陳述式</span><span class="sxs-lookup"><span data-stu-id="2c009-178">Labeled statements</span></span>

<span data-ttu-id="2c009-179">*Labeled_statement*允許語句前面加上標籤。</span><span class="sxs-lookup"><span data-stu-id="2c009-179">A *labeled_statement* permits a statement to be prefixed by a label.</span></span> <span data-ttu-id="2c009-180">區塊中允許標記的語句，但不允許做為內嵌語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-180">Labeled statements are permitted in blocks, but are not permitted as embedded statements.</span></span>

```antlr
labeled_statement
    : identifier ':' statement
    ;
```

<span data-ttu-id="2c009-181">標記的語句會宣告具有*識別碼*所指定之名稱的標籤。</span><span class="sxs-lookup"><span data-stu-id="2c009-181">A labeled statement declares a label with the name given by the *identifier*.</span></span> <span data-ttu-id="2c009-182">標籤的範圍是宣告標籤的整個區塊，包括任何嵌套的區塊。</span><span class="sxs-lookup"><span data-stu-id="2c009-182">The scope of a label is the whole block in which the label is declared, including any nested blocks.</span></span> <span data-ttu-id="2c009-183">這是兩個具有相同名稱的標籤，具有重迭範圍的編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2c009-183">It is a compile-time error for two labels with the same name to have overlapping scopes.</span></span>

<span data-ttu-id="2c009-184">標籤可以從`goto`標籤範圍內的語句（[goto 語句](statements.md#the-goto-statement)）參考。</span><span class="sxs-lookup"><span data-stu-id="2c009-184">A label can be referenced from `goto` statements ([The goto statement](statements.md#the-goto-statement)) within the scope of the label.</span></span> <span data-ttu-id="2c009-185">這表示`goto`語句可以在區塊內和區塊外傳輸控制項，但絕不會進入區塊。</span><span class="sxs-lookup"><span data-stu-id="2c009-185">This means that `goto` statements can transfer control within blocks and out of blocks, but never into blocks.</span></span>

<span data-ttu-id="2c009-186">標籤有自己的宣告空間，而且不會干擾其他識別碼。</span><span class="sxs-lookup"><span data-stu-id="2c009-186">Labels have their own declaration space and do not interfere with other identifiers.</span></span> <span data-ttu-id="2c009-187">範例</span><span class="sxs-lookup"><span data-stu-id="2c009-187">The example</span></span>
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
<span data-ttu-id="2c009-188">是有效的，而且使用`x`名稱同時做為參數和標籤。</span><span class="sxs-lookup"><span data-stu-id="2c009-188">is valid and uses the name `x` as both a parameter and a label.</span></span>

<span data-ttu-id="2c009-189">執行標籤的語句時，會完全對應到在標籤之後執行語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-189">Execution of a labeled statement corresponds exactly to execution of the statement following the label.</span></span>

<span data-ttu-id="2c009-190">除了一般控制流程所提供的可連線性之外，如果可連接的`goto`語句參考標籤，則可連接標記的語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-190">In addition to the reachability provided by normal flow of control, a labeled statement is reachable if the label is referenced by a reachable `goto` statement.</span></span> <span data-ttu-id="2c009-191">異常`try` `try` `finally`如果語句是在包含區塊的內，而且加上標籤的語句位於之外，而且無法連線到`finally`區塊的結束點，則無法從存取標記的語句`goto`該`goto`語句）。</span><span class="sxs-lookup"><span data-stu-id="2c009-191">(Exception: If a `goto` statement is inside a `try` that includes a `finally` block, and the labeled statement is outside the `try`, and the end point of the `finally` block is unreachable, then the labeled statement is not reachable from that `goto` statement.)</span></span>

## <a name="declaration-statements"></a><span data-ttu-id="2c009-192">宣告陳述式</span><span class="sxs-lookup"><span data-stu-id="2c009-192">Declaration statements</span></span>

<span data-ttu-id="2c009-193">*Declaration_statement*會宣告本機變數或常數。</span><span class="sxs-lookup"><span data-stu-id="2c009-193">A *declaration_statement* declares a local variable or constant.</span></span> <span data-ttu-id="2c009-194">區塊中允許宣告語句，但不允許做為內嵌語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-194">Declaration statements are permitted in blocks, but are not permitted as embedded statements.</span></span>

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a><span data-ttu-id="2c009-195">區域變數宣告</span><span class="sxs-lookup"><span data-stu-id="2c009-195">Local variable declarations</span></span>

<span data-ttu-id="2c009-196">*Local_variable_declaration*會宣告一或多個本機變數。</span><span class="sxs-lookup"><span data-stu-id="2c009-196">A *local_variable_declaration* declares one or more local variables.</span></span>

```antlr
local_variable_declaration
    : local_variable_type local_variable_declarators
    ;

local_variable_type
    : type
    | 'var'
    ;

local_variable_declarators
    : local_variable_declarator
    | local_variable_declarators ',' local_variable_declarator
    ;

local_variable_declarator
    : identifier
    | identifier '=' local_variable_initializer
    ;

local_variable_initializer
    : expression
    | array_initializer
    | local_variable_initializer_unsafe
    ;
```

<span data-ttu-id="2c009-197">*Local_variable_declaration*的*local_variable_type*會直接指定宣告所引進的變數類型，或指示識別碼 `var`，該類型應根據初始化運算式來推斷。</span><span class="sxs-lookup"><span data-stu-id="2c009-197">The *local_variable_type* of a *local_variable_declaration* either directly specifies the type of the variables introduced by the declaration, or indicates with the identifier `var` that the type should be inferred based on an initializer.</span></span> <span data-ttu-id="2c009-198">類型後面接著*local_variable_declarator*的清單，其中每個都會引進新的變數。</span><span class="sxs-lookup"><span data-stu-id="2c009-198">The type is followed by a list of *local_variable_declarator*s, each of which introduces a new variable.</span></span> <span data-ttu-id="2c009-199">*Local_variable_declarator*包含命名變數的*識別碼*，並可選擇性地後面加上 "`=`" 權杖和*local_variable_initializer* ，以提供變數的初始值。</span><span class="sxs-lookup"><span data-stu-id="2c009-199">A *local_variable_declarator* consists of an *identifier* that names the variable, optionally followed by an "`=`" token and a *local_variable_initializer* that gives the initial value of the variable.</span></span>

<span data-ttu-id="2c009-200">在本機變數宣告的內容中，識別碼 var 會當做內容關鍵字（[關鍵字](lexical-structure.md#keywords)）。當*local_variable_type*指定為 `var`，而且沒有任何名為 `var` 的類型在範圍內時，宣告就是隱含類型的***區域變數***宣告，其類型是從相關聯的初始化運算式運算式的類型推斷而來。</span><span class="sxs-lookup"><span data-stu-id="2c009-200">In the context of a local variable declaration, the identifier var acts as a contextual keyword ([Keywords](lexical-structure.md#keywords)).When the *local_variable_type* is specified as `var` and no type named `var` is in scope, the declaration is an ***implicitly typed local variable declaration***, whose type is inferred from the type of the associated initializer expression.</span></span> <span data-ttu-id="2c009-201">隱含類型的區域變數宣告受到下列限制：</span><span class="sxs-lookup"><span data-stu-id="2c009-201">Implicitly typed local variable declarations are subject to the following restrictions:</span></span>

*  <span data-ttu-id="2c009-202">*Local_variable_declaration*不能包含多個*local_variable_declarator*s。</span><span class="sxs-lookup"><span data-stu-id="2c009-202">The *local_variable_declaration* cannot include multiple *local_variable_declarator*s.</span></span>
*  <span data-ttu-id="2c009-203">*Local_variable_declarator*必須包含*local_variable_initializer*。</span><span class="sxs-lookup"><span data-stu-id="2c009-203">The *local_variable_declarator* must include a *local_variable_initializer*.</span></span>
*  <span data-ttu-id="2c009-204">*Local_variable_initializer*必須是*運算式*。</span><span class="sxs-lookup"><span data-stu-id="2c009-204">The *local_variable_initializer* must be an *expression*.</span></span>
*  <span data-ttu-id="2c009-205">初始化*運算式運算式*必須有編譯時期類型。</span><span class="sxs-lookup"><span data-stu-id="2c009-205">The initializer *expression* must have a compile-time type.</span></span>
*  <span data-ttu-id="2c009-206">初始化*運算式運算式*不能參考宣告的變數本身</span><span class="sxs-lookup"><span data-stu-id="2c009-206">The initializer *expression* cannot refer to the declared variable itself</span></span>

<span data-ttu-id="2c009-207">以下是不正確的隱含類型區域變數宣告範例：</span><span class="sxs-lookup"><span data-stu-id="2c009-207">The following are examples of incorrect implicitly typed local variable declarations:</span></span>

```csharp
var x;               // Error, no initializer to infer type from
var y = {1, 2, 3};   // Error, array initializer not permitted
var z = null;        // Error, null does not have a type
var u = x => x + 1;  // Error, anonymous functions do not have a type
var v = v++;         // Error, initializer cannot refer to variable itself
```

<span data-ttu-id="2c009-208">區域變數的值是使用*simple_name* （[簡單名稱](expressions.md#simple-names)）在運算式中取得，而區域變數的值則是使用*指派*（[指派運算子](expressions.md#assignment-operators)）來修改。</span><span class="sxs-lookup"><span data-stu-id="2c009-208">The value of a local variable is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)), and the value of a local variable is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="2c009-209">在取得其值的每個位置，都必須明確指派本機變數（[明確](variables.md#definite-assignment)指派）。</span><span class="sxs-lookup"><span data-stu-id="2c009-209">A local variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) at each location where its value is obtained.</span></span>

<span data-ttu-id="2c009-210">在*local_variable_declaration*中宣告的本機變數範圍是宣告發生所在的區塊。</span><span class="sxs-lookup"><span data-stu-id="2c009-210">The scope of a local variable declared in a *local_variable_declaration* is the block in which the declaration occurs.</span></span> <span data-ttu-id="2c009-211">在本機變數的*local_variable_declarator*之前，參考位於文字位置的區域變數是錯誤的。</span><span class="sxs-lookup"><span data-stu-id="2c009-211">It is an error to refer to a local variable in a textual position that precedes the *local_variable_declarator* of the local variable.</span></span> <span data-ttu-id="2c009-212">在區域變數的範圍內，宣告具有相同名稱的另一個本機變數或常數會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2c009-212">Within the scope of a local variable, it is a compile-time error to declare another local variable or constant with the same name.</span></span>

<span data-ttu-id="2c009-213">宣告多個變數的區域變數宣告相當於多個具有相同類型之單一變數的宣告。</span><span class="sxs-lookup"><span data-stu-id="2c009-213">A local variable declaration that declares multiple variables is equivalent to multiple declarations of single variables with the same type.</span></span> <span data-ttu-id="2c009-214">此外，區域變數宣告中的變數初始化運算式會完全對應至緊接在宣告之後插入的指派語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-214">Furthermore, a variable initializer in a local variable declaration corresponds exactly to an assignment statement that is inserted immediately after the declaration.</span></span>

<span data-ttu-id="2c009-215">範例</span><span class="sxs-lookup"><span data-stu-id="2c009-215">The example</span></span>
```csharp
void F() {
    int x = 1, y, z = x * 2;
}
```
<span data-ttu-id="2c009-216">完全對應至</span><span class="sxs-lookup"><span data-stu-id="2c009-216">corresponds exactly to</span></span>
```csharp
void F() {
    int x; x = 1;
    int y;
    int z; z = x * 2;
}
```

<span data-ttu-id="2c009-217">在隱含類型的區域變數宣告中，所宣告的區域變數類型會被視為與用來初始化變數的運算式類型相同。</span><span class="sxs-lookup"><span data-stu-id="2c009-217">In an implicitly typed local variable declaration, the type of the local variable being declared is taken to be the same as the type of the expression used to initialize the variable.</span></span> <span data-ttu-id="2c009-218">例如:</span><span class="sxs-lookup"><span data-stu-id="2c009-218">For example:</span></span>
```csharp
var i = 5;
var s = "Hello";
var d = 1.0;
var numbers = new int[] {1, 2, 3};
var orders = new Dictionary<int,Order>();
```

<span data-ttu-id="2c009-219">上述隱含型別區域變數宣告相當於下列明確類型的宣告：</span><span class="sxs-lookup"><span data-stu-id="2c009-219">The implicitly typed local variable declarations above are precisely equivalent to the following explicitly typed declarations:</span></span>
```csharp
int i = 5;
string s = "Hello";
double d = 1.0;
int[] numbers = new int[] {1, 2, 3};
Dictionary<int,Order> orders = new Dictionary<int,Order>();
```

### <a name="local-constant-declarations"></a><span data-ttu-id="2c009-220">區域常數宣告</span><span class="sxs-lookup"><span data-stu-id="2c009-220">Local constant declarations</span></span>

<span data-ttu-id="2c009-221">*Local_constant_declaration*會宣告一個或多個本機常數。</span><span class="sxs-lookup"><span data-stu-id="2c009-221">A *local_constant_declaration* declares one or more local constants.</span></span>

```antlr
local_constant_declaration
    : 'const' type constant_declarators
    ;

constant_declarators
    : constant_declarator (',' constant_declarator)*
    ;

constant_declarator
    : identifier '=' constant_expression
    ;
```

<span data-ttu-id="2c009-222">*Local_constant_declaration*的*類型*會指定宣告所引進的常數類型。</span><span class="sxs-lookup"><span data-stu-id="2c009-222">The *type* of a *local_constant_declaration* specifies the type of the constants introduced by the declaration.</span></span> <span data-ttu-id="2c009-223">類型後面接著*constant_declarator*的清單，其中每個都會引進新的常數。</span><span class="sxs-lookup"><span data-stu-id="2c009-223">The type is followed by a list of *constant_declarator*s, each of which introduces a new constant.</span></span> <span data-ttu-id="2c009-224">*Constant_declarator*包含命名常數的*識別碼*，後面接著 "`=`" token，接著是提供常數值的*constant_expression* （[常數運算式](expressions.md#constant-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="2c009-224">A *constant_declarator* consists of an *identifier* that names the constant, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the constant.</span></span>

<span data-ttu-id="2c009-225">區域常數值宣告的*類型*和*constant_expression*必須遵循與常數成員宣告（[常數](classes.md#constants)）相同的規則。</span><span class="sxs-lookup"><span data-stu-id="2c009-225">The *type* and *constant_expression* of a local constant declaration must follow the same rules as those of a constant member declaration ([Constants](classes.md#constants)).</span></span>

<span data-ttu-id="2c009-226">本機常數的值是使用*simple_name* （[簡單名稱](expressions.md#simple-names)）在運算式中取得。</span><span class="sxs-lookup"><span data-stu-id="2c009-226">The value of a local constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="2c009-227">本機常數的範圍是宣告發生所在的區塊。</span><span class="sxs-lookup"><span data-stu-id="2c009-227">The scope of a local constant is the block in which the declaration occurs.</span></span> <span data-ttu-id="2c009-228">在*constant_declarator*之前的文字位置中參考本機常數是錯誤的。</span><span class="sxs-lookup"><span data-stu-id="2c009-228">It is an error to refer to a local constant in a textual position that precedes its *constant_declarator*.</span></span> <span data-ttu-id="2c009-229">在本機常數的範圍內，宣告具有相同名稱的另一個本機變數或常數的編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2c009-229">Within the scope of a local constant, it is a compile-time error to declare another local variable or constant with the same name.</span></span>

<span data-ttu-id="2c009-230">宣告多個常數的本機常數宣告相當於多個具有相同類型之單一常數的宣告。</span><span class="sxs-lookup"><span data-stu-id="2c009-230">A local constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same type.</span></span>

## <a name="expression-statements"></a><span data-ttu-id="2c009-231">運算式陳述式</span><span class="sxs-lookup"><span data-stu-id="2c009-231">Expression statements</span></span>

<span data-ttu-id="2c009-232">*Expression_statement*會評估指定的運算式。</span><span class="sxs-lookup"><span data-stu-id="2c009-232">An *expression_statement* evaluates a given expression.</span></span> <span data-ttu-id="2c009-233">會捨棄運算式所計算的值（如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="2c009-233">The value computed by the expression, if any, is discarded.</span></span>

```antlr
expression_statement
    : statement_expression ';'
    ;

statement_expression
    : invocation_expression
    | null_conditional_invocation_expression
    | object_creation_expression
    | assignment
    | post_increment_expression
    | post_decrement_expression
    | pre_increment_expression
    | pre_decrement_expression
    | await_expression
    ;
```

<span data-ttu-id="2c009-234">並非所有運算式都允許當做語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-234">Not all expressions are permitted as statements.</span></span> <span data-ttu-id="2c009-235">特別是，不允許只`x + y`計算`x == 1`值（將被捨棄）的運算式，做為語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-235">In particular, expressions such as `x + y` and `x == 1` that merely compute a value (which will be discarded), are not permitted as statements.</span></span>

<span data-ttu-id="2c009-236">執行*expression_statement*會評估包含的運算式，然後將控制權轉移至*expression_statement*的結束點。</span><span class="sxs-lookup"><span data-stu-id="2c009-236">Execution of an *expression_statement* evaluates the contained expression and then transfers control to the end point of the *expression_statement*.</span></span> <span data-ttu-id="2c009-237">如果可以連線到該*expression_statement* ，就可以連線到*expression_statement*的結束點。</span><span class="sxs-lookup"><span data-stu-id="2c009-237">The end point of an *expression_statement* is reachable if that *expression_statement* is reachable.</span></span>

## <a name="selection-statements"></a><span data-ttu-id="2c009-238">選取範圍陳述式</span><span class="sxs-lookup"><span data-stu-id="2c009-238">Selection statements</span></span>

<span data-ttu-id="2c009-239">選取範圍語句根據某個運算式的值，選取其中一個可能的語句來執行。</span><span class="sxs-lookup"><span data-stu-id="2c009-239">Selection statements select one of a number of possible statements for execution based on the value of some expression.</span></span>

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a><span data-ttu-id="2c009-240">If 語句</span><span class="sxs-lookup"><span data-stu-id="2c009-240">The if statement</span></span>

<span data-ttu-id="2c009-241">`if`語句會根據布林運算式的值來選取要執行的語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-241">The `if` statement selects a statement for execution based on the value of a boolean expression.</span></span>

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

<span data-ttu-id="2c009-242">元件會與語法允許的最接近前面`if`的程式關聯。 `else`</span><span class="sxs-lookup"><span data-stu-id="2c009-242">An `else` part is associated with the lexically nearest preceding `if` that is allowed by the syntax.</span></span> <span data-ttu-id="2c009-243">因此， `if`表單的語句</span><span class="sxs-lookup"><span data-stu-id="2c009-243">Thus, an `if` statement of the form</span></span>
```csharp
if (x) if (y) F(); else G();
```
<span data-ttu-id="2c009-244">相當於</span><span class="sxs-lookup"><span data-stu-id="2c009-244">is equivalent to</span></span>
```csharp
if (x) {
    if (y) {
        F();
    }
    else {
        G();
    }
}
```

<span data-ttu-id="2c009-245">`if`語句的執行方式如下：</span><span class="sxs-lookup"><span data-stu-id="2c009-245">An `if` statement is executed as follows:</span></span>

*  <span data-ttu-id="2c009-246">會評估*boolean_expression* （[布林運算式](expressions.md#boolean-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="2c009-246">The *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span>
*  <span data-ttu-id="2c009-247">如果布林運算式產生`true`，控制權就會傳送至第一個內嵌語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-247">If the boolean expression yields `true`, control is transferred to the first embedded statement.</span></span> <span data-ttu-id="2c009-248">當控制項到達該語句的結束點時，控制權就會傳送至`if`語句的結束點。</span><span class="sxs-lookup"><span data-stu-id="2c009-248">When and if control reaches the end point of that statement, control is transferred to the end point of the `if` statement.</span></span>
*  <span data-ttu-id="2c009-249">如果布林運算式產生`false` ，而且如果有某個`else`部分存在，控制權就會傳送至第二個內嵌語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-249">If the boolean expression yields `false` and if an `else` part is present, control is transferred to the second embedded statement.</span></span> <span data-ttu-id="2c009-250">當控制項到達該語句的結束點時，控制權就會傳送至`if`語句的結束點。</span><span class="sxs-lookup"><span data-stu-id="2c009-250">When and if control reaches the end point of that statement, control is transferred to the end point of the `if` statement.</span></span>
*  <span data-ttu-id="2c009-251">如果布林運算式產生`false` ，而且`else`如果元件不存在，控制權就會傳送至`if`語句的結束點。</span><span class="sxs-lookup"><span data-stu-id="2c009-251">If the boolean expression yields `false` and if an `else` part is not present, control is transferred to the end point of the `if` statement.</span></span>

<span data-ttu-id="2c009-252">如果可以連線到`if` `if`語句，而且布林運算式沒有常數值`false`，則可以連接語句的第一個內嵌語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-252">The first embedded statement of an `if` statement is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `false`.</span></span>

<span data-ttu-id="2c009-253">`if`語句的第二個內嵌語句（如果有的話）可以連接到`if`語句，而且布林運算式沒有常數值`true`。</span><span class="sxs-lookup"><span data-stu-id="2c009-253">The second embedded statement of an `if` statement, if present, is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

<span data-ttu-id="2c009-254">如果至少有一個內嵌`if`語句的端點可供存取，則語句的結束點可供連線。</span><span class="sxs-lookup"><span data-stu-id="2c009-254">The end point of an `if` statement is reachable if the end point of at least one of its embedded statements is reachable.</span></span> <span data-ttu-id="2c009-255">此外，如果可以`if`連線到`if`語句，而且布林運算式`else`沒有常數值`true`，就可以連接沒有部分之語句的結束點。</span><span class="sxs-lookup"><span data-stu-id="2c009-255">In addition, the end point of an `if` statement with no `else` part is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-switch-statement"></a><span data-ttu-id="2c009-256">Switch 語句</span><span class="sxs-lookup"><span data-stu-id="2c009-256">The switch statement</span></span>

<span data-ttu-id="2c009-257">Switch 語句會選取執行語句清單，其中包含對應至 switch 運算式值的相關聯參數標籤。</span><span class="sxs-lookup"><span data-stu-id="2c009-257">The switch statement selects for execution a statement list having an associated switch label that corresponds to the value of the switch expression.</span></span>

```antlr
switch_statement
    : 'switch' '(' expression ')' switch_block
    ;

switch_block
    : '{' switch_section* '}'
    ;

switch_section
    : switch_label+ statement_list
    ;

switch_label
    : 'case' constant_expression ':'
    | 'default' ':'
    ;
```

<span data-ttu-id="2c009-258">*Switch_statement*是由關鍵字 `switch` 組成，後面加上括弧括住的運算式（稱為 switch 運算式），後面接著*switch_block*。</span><span class="sxs-lookup"><span data-stu-id="2c009-258">A *switch_statement* consists of the keyword `switch`, followed by a parenthesized expression (called the switch expression), followed by a *switch_block*.</span></span> <span data-ttu-id="2c009-259">*Switch_block*是由零個或多個*switch_section*組成，以大括弧括住。</span><span class="sxs-lookup"><span data-stu-id="2c009-259">The *switch_block* consists of zero or more *switch_section*s, enclosed in braces.</span></span> <span data-ttu-id="2c009-260">每個*switch_section*都包含一個或多個*switch_label*，後面接著*statement_list* （[語句清單](statements.md#statement-lists)）。</span><span class="sxs-lookup"><span data-stu-id="2c009-260">Each *switch_section* consists of one or more *switch_label*s followed by a *statement_list* ([Statement lists](statements.md#statement-lists)).</span></span>

<span data-ttu-id="2c009-261">`switch`語句的***管理類型***是由 switch 運算式所建立。</span><span class="sxs-lookup"><span data-stu-id="2c009-261">The ***governing type*** of a `switch` statement is established by the switch expression.</span></span>

*  <span data-ttu-id="2c009-262">如果 switch 運算式的類型為 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`bool`、`char`、0 或*enum_type*，或者它是對應于其中一種類型的可為 null 的類型，那就是 2 語句的管理類型。</span><span class="sxs-lookup"><span data-stu-id="2c009-262">If the type of the switch expression is `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `bool`, `char`, `string`, or an *enum_type*, or if it is the nullable type corresponding to one of these types, then that is the governing type of the `switch` statement.</span></span>
*  <span data-ttu-id="2c009-263">否則，只有一個使用者定義的隱含轉換（[使用者定義的轉換](conversions.md#user-defined-conversions)），必須從 switch 運算式的類型，到下列其中一種可能的管理類型： `sbyte`、 `byte`、 `short`、 `ushort`、 `int`、 `uint`、 、`long` 、`char`、或，這是對應至其中一種類型的可為 null 類型。 `string` `ulong`</span><span class="sxs-lookup"><span data-stu-id="2c009-263">Otherwise, exactly one user-defined implicit conversion ([User-defined conversions](conversions.md#user-defined-conversions)) must exist from the type of the switch expression to one of the following possible governing types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `string`, or,  a nullable type corresponding to one of those types.</span></span>
*  <span data-ttu-id="2c009-264">否則，如果不存在這類隱含轉換，或如果有多個這類隱含轉換存在，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2c009-264">Otherwise, if no such implicit conversion exists, or if more than one such implicit conversion exists, a compile-time error occurs.</span></span>

<span data-ttu-id="2c009-265">每個`case`標籤的常數運算式都必須代表可隱含轉換（[隱含轉換](conversions.md#implicit-conversions)）為`switch`語句之治理類型的值。</span><span class="sxs-lookup"><span data-stu-id="2c009-265">The constant expression of each `case` label must denote a value that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the governing type of the `switch` statement.</span></span> <span data-ttu-id="2c009-266">如果相同`case` `switch`語句中有兩個或多個標籤指定相同的常數值，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2c009-266">A compile-time error occurs if two or more `case` labels in the same `switch` statement specify the same constant value.</span></span>

<span data-ttu-id="2c009-267">Switch 語句中最多隻能`default`有一個標籤。</span><span class="sxs-lookup"><span data-stu-id="2c009-267">There can be at most one `default` label in a switch statement.</span></span>

<span data-ttu-id="2c009-268">`switch`語句的執行方式如下：</span><span class="sxs-lookup"><span data-stu-id="2c009-268">A `switch` statement is executed as follows:</span></span>

*  <span data-ttu-id="2c009-269">會評估 switch 運算式，並將其轉換為管理類型。</span><span class="sxs-lookup"><span data-stu-id="2c009-269">The switch expression is evaluated and converted to the governing type.</span></span>
*  <span data-ttu-id="2c009-270">如果相同`case` `switch`語句的標籤中指定的其中一個常數等於 switch 運算式的值，則會將控制權轉移到符合`case`的標籤後面的語句清單。</span><span class="sxs-lookup"><span data-stu-id="2c009-270">If one of the constants specified in a `case` label in the same `switch` statement is equal to the value of the switch expression, control is transferred to the statement list following the matched `case` label.</span></span>
*  <span data-ttu-id="2c009-271">`case`如果相同`switch`語句中的標籤中指定的常數都不等於 switch `default`運算式的值，而且如果有標籤，則`default`會將控制權轉移至語句清單，並遵循標誌.</span><span class="sxs-lookup"><span data-stu-id="2c009-271">If none of the constants specified in `case` labels in the same `switch` statement is equal to the value of the switch expression, and if a `default` label is present, control is transferred to the statement list following the `default` label.</span></span>
*  <span data-ttu-id="2c009-272">`case`如果相同`switch`語句的標籤中指定的常數都不等於 switch 運算式的值，而且如果沒有`default`標籤，則控制權`switch`會傳送至語句的結束點。</span><span class="sxs-lookup"><span data-stu-id="2c009-272">If none of the constants specified in `case` labels in the same `switch` statement is equal to the value of the switch expression, and if no `default` label is present, control is transferred to the end point of the `switch` statement.</span></span>

<span data-ttu-id="2c009-273">如果可以連線到 switch 區段之語句清單的結束點，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2c009-273">If the end point of the statement list of a switch section is reachable, a compile-time error occurs.</span></span> <span data-ttu-id="2c009-274">這就是所謂的「不流經」規則。</span><span class="sxs-lookup"><span data-stu-id="2c009-274">This is known as the "no fall through" rule.</span></span> <span data-ttu-id="2c009-275">範例</span><span class="sxs-lookup"><span data-stu-id="2c009-275">The example</span></span>
```csharp
switch (i) {
case 0:
    CaseZero();
    break;
case 1:
    CaseOne();
    break;
default:
    CaseOthers();
    break;
}
```
<span data-ttu-id="2c009-276">有效，因為沒有參數區段具有可連線的結束點。</span><span class="sxs-lookup"><span data-stu-id="2c009-276">is valid because no switch section has a reachable end point.</span></span> <span data-ttu-id="2c009-277">不同于 C C++和，switch 區段的執行不允許「落在」到下一個參數區段，而範例</span><span class="sxs-lookup"><span data-stu-id="2c009-277">Unlike C and C++, execution of a switch section is not permitted to "fall through" to the next switch section, and the example</span></span>
```csharp
switch (i) {
case 0:
    CaseZero();
case 1:
    CaseZeroOrOne();
default:
    CaseAny();
}
```
<span data-ttu-id="2c009-278">會導致編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2c009-278">results in a compile-time error.</span></span> <span data-ttu-id="2c009-279">執行 switch 區段之後，執行另一個參數區段時，必須使用明確`goto case`或`goto default`語句：</span><span class="sxs-lookup"><span data-stu-id="2c009-279">When execution of a switch section is to be followed by execution of another switch section, an explicit `goto case` or `goto default` statement must be used:</span></span>
```csharp
switch (i) {
case 0:
    CaseZero();
    goto case 1;
case 1:
    CaseZeroOrOne();
    goto default;
default:
    CaseAny();
    break;
}
```

<span data-ttu-id="2c009-280">*Switch_section*中允許多個標籤。</span><span class="sxs-lookup"><span data-stu-id="2c009-280">Multiple labels are permitted in a *switch_section*.</span></span> <span data-ttu-id="2c009-281">範例</span><span class="sxs-lookup"><span data-stu-id="2c009-281">The example</span></span>
```csharp
switch (i) {
case 0:
    CaseZero();
    break;
case 1:
    CaseOne();
    break;
case 2:
default:
    CaseTwo();
    break;
}
```
<span data-ttu-id="2c009-282">有效。</span><span class="sxs-lookup"><span data-stu-id="2c009-282">is valid.</span></span> <span data-ttu-id="2c009-283">此範例不會違反「不流經」規則，因為標籤 `case 2:`，而 `default:` 是相同*switch_section*的一部分。</span><span class="sxs-lookup"><span data-stu-id="2c009-283">The example does not violate the "no fall through" rule because the labels `case 2:` and `default:` are part of the same *switch_section*.</span></span>

<span data-ttu-id="2c009-284">「無範圍」規則可防止 C 中發生的常見錯誤類別，以及C++ `break`不小心省略語句的情況。</span><span class="sxs-lookup"><span data-stu-id="2c009-284">The "no fall through" rule prevents a common class of bugs that occur in C and C++ when `break` statements are accidentally omitted.</span></span> <span data-ttu-id="2c009-285">此外，由於這項規則， `switch`語句的 switch 區段可以任意重新排列，而不會影響語句的行為。</span><span class="sxs-lookup"><span data-stu-id="2c009-285">In addition, because of this rule, the switch sections of a `switch` statement can be arbitrarily rearranged without affecting the behavior of the statement.</span></span> <span data-ttu-id="2c009-286">例如，上述`switch`語句的區段可以反轉，而不會影響語句的行為：</span><span class="sxs-lookup"><span data-stu-id="2c009-286">For example, the sections of the `switch` statement above can be reversed without affecting the behavior of the statement:</span></span>
```csharp
switch (i) {
default:
    CaseAny();
    break;
case 1:
    CaseZeroOrOne();
    goto default;
case 0:
    CaseZero();
    goto case 1;
}
```

<span data-ttu-id="2c009-287">Switch 區段的語句清單通常會在`break`、 `goto case`或`goto default`語句中結束，但不允許任何呈現語句清單之結束點的結構。</span><span class="sxs-lookup"><span data-stu-id="2c009-287">The statement list of a switch section typically ends in a `break`, `goto case`, or `goto default` statement, but any construct that renders the end point of the statement list unreachable is permitted.</span></span> <span data-ttu-id="2c009-288">例如， `while`已知由布林運算式`true`所控制的語句，絕對不會到達其結束點。</span><span class="sxs-lookup"><span data-stu-id="2c009-288">For example, a `while` statement controlled by the boolean expression `true` is known to never reach its end point.</span></span> <span data-ttu-id="2c009-289">同樣地， `throw`或`return`語句一律會將控制權轉移到別處，而且永遠不會到達其結束點。</span><span class="sxs-lookup"><span data-stu-id="2c009-289">Likewise, a `throw` or `return` statement always transfers control elsewhere and never reaches its end point.</span></span> <span data-ttu-id="2c009-290">因此，下列範例是有效的：</span><span class="sxs-lookup"><span data-stu-id="2c009-290">Thus, the following example is valid:</span></span>
```csharp
switch (i) {
case 0:
    while (true) F();
case 1:
    throw new ArgumentException();
case 2:
    return;
}
```

<span data-ttu-id="2c009-291">`switch`語句的管理類型可以是類型`string`。</span><span class="sxs-lookup"><span data-stu-id="2c009-291">The governing type of a `switch` statement may be the type `string`.</span></span> <span data-ttu-id="2c009-292">例如:</span><span class="sxs-lookup"><span data-stu-id="2c009-292">For example:</span></span>
```csharp
void DoCommand(string command) {
    switch (command.ToLower()) {
    case "run":
        DoRun();
        break;
    case "save":
        DoSave();
        break;
    case "quit":
        DoQuit();
        break;
    default:
        InvalidCommand(command);
        break;
    }
}
```

<span data-ttu-id="2c009-293">如同字串等號比較運算子（[字串等號比較](expressions.md#string-equality-operators)運算子`switch` ），語句會區分大小寫，而且只有在 switch 運算式`case`字串完全符合標籤常數時，才會執行指定的參數區段。</span><span class="sxs-lookup"><span data-stu-id="2c009-293">Like the string equality operators ([String equality operators](expressions.md#string-equality-operators)), the `switch` statement is case sensitive and will execute a given switch section only if the switch expression string exactly matches a `case` label constant.</span></span>

<span data-ttu-id="2c009-294">當`switch`語句的管理類型為`string`時，會允許此`null`值作為 case 標籤常數。</span><span class="sxs-lookup"><span data-stu-id="2c009-294">When the governing type of a `switch` statement is `string`, the value `null` is permitted as a case label constant.</span></span>

<span data-ttu-id="2c009-295">*Switch_block*的*statement_list*可能包含宣告語句（宣告[語句](statements.md#declaration-statements)）。</span><span class="sxs-lookup"><span data-stu-id="2c009-295">The *statement_list*s of a *switch_block* may contain declaration statements ([Declaration statements](statements.md#declaration-statements)).</span></span> <span data-ttu-id="2c009-296">在 switch 區塊中宣告的區域變數或常數的範圍是 switch 區塊。</span><span class="sxs-lookup"><span data-stu-id="2c009-296">The scope of a local variable or constant declared in a switch block is the switch block.</span></span>

<span data-ttu-id="2c009-297">如果可以連線到`switch`語句，而且至少有下列其中一項為真，則可以連接指定參數區段的語句清單：</span><span class="sxs-lookup"><span data-stu-id="2c009-297">The statement list of a given switch section is reachable if the `switch` statement is reachable and at least one of the following is true:</span></span>

*  <span data-ttu-id="2c009-298">Switch 運算式是一個非常數值。</span><span class="sxs-lookup"><span data-stu-id="2c009-298">The switch expression is a non-constant value.</span></span>
*  <span data-ttu-id="2c009-299">Switch 運算式是與 switch 區段中的`case`標籤相符的常數值。</span><span class="sxs-lookup"><span data-stu-id="2c009-299">The switch expression is a constant value that matches a `case` label in the switch section.</span></span>
*  <span data-ttu-id="2c009-300">Switch 運算式是不符合任何`case`標籤的常數值，而 switch 區段`default`包含標籤。</span><span class="sxs-lookup"><span data-stu-id="2c009-300">The switch expression is a constant value that doesn't match any `case` label, and the switch section contains the `default` label.</span></span>
*  <span data-ttu-id="2c009-301">Switch 區段的切換標籤是由`goto case`可連接的或`goto default`語句所參考。</span><span class="sxs-lookup"><span data-stu-id="2c009-301">A switch label of the switch section is referenced by a reachable `goto case` or `goto default` statement.</span></span>

<span data-ttu-id="2c009-302">如果下列至少一項`switch`為真，則語句的結束點可供連線：</span><span class="sxs-lookup"><span data-stu-id="2c009-302">The end point of a `switch` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="2c009-303">語句包含可連接`switch`的`break`語句，它會結束語句。 `switch`</span><span class="sxs-lookup"><span data-stu-id="2c009-303">The `switch` statement contains a reachable `break` statement that exits the `switch` statement.</span></span>
*  <span data-ttu-id="2c009-304">可以`switch`連線到語句，switch 運算式為非常數值，且沒有任何`default`標籤存在。</span><span class="sxs-lookup"><span data-stu-id="2c009-304">The `switch` statement is reachable, the switch expression is a non-constant value, and no `default` label is present.</span></span>
*  <span data-ttu-id="2c009-305">可以`switch`連線到語句，switch 運算式是不符合任何`case`標籤的常數值，而且沒有任何`default`標籤存在。</span><span class="sxs-lookup"><span data-stu-id="2c009-305">The `switch` statement is reachable, the switch expression is a constant value that doesn't match any `case` label, and no `default` label is present.</span></span>

## <a name="iteration-statements"></a><span data-ttu-id="2c009-306">反覆運算陳述式</span><span class="sxs-lookup"><span data-stu-id="2c009-306">Iteration statements</span></span>

<span data-ttu-id="2c009-307">反覆運算語句會重複執行內嵌語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-307">Iteration statements repeatedly execute an embedded statement.</span></span>

```antlr
iteration_statement
    : while_statement
    | do_statement
    | for_statement
    | foreach_statement
    ;
```

### <a name="the-while-statement"></a><span data-ttu-id="2c009-308">While 語句</span><span class="sxs-lookup"><span data-stu-id="2c009-308">The while statement</span></span>

<span data-ttu-id="2c009-309">`while`語句有條件地執行內嵌語句零次或多次。</span><span class="sxs-lookup"><span data-stu-id="2c009-309">The `while` statement conditionally executes an embedded statement zero or more times.</span></span>

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

<span data-ttu-id="2c009-310">`while`語句的執行方式如下：</span><span class="sxs-lookup"><span data-stu-id="2c009-310">A `while` statement is executed as follows:</span></span>

*  <span data-ttu-id="2c009-311">會評估*boolean_expression* （[布林運算式](expressions.md#boolean-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="2c009-311">The *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span>
*  <span data-ttu-id="2c009-312">如果布林運算式產生`true`，控制權就會傳送至內嵌語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-312">If the boolean expression yields `true`, control is transferred to the embedded statement.</span></span> <span data-ttu-id="2c009-313">當和（如果控制）到達內嵌語句的結束點（可能是從`continue`語句執行）時，控制權就會傳送至`while`語句的開頭。</span><span class="sxs-lookup"><span data-stu-id="2c009-313">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), control is transferred to the beginning of the `while` statement.</span></span>
*  <span data-ttu-id="2c009-314">如果布林運算式產生`false`，控制權就會傳送至`while`語句的結束點。</span><span class="sxs-lookup"><span data-stu-id="2c009-314">If the boolean expression yields `false`, control is transferred to the end point of the `while` statement.</span></span>

<span data-ttu-id="2c009-315">在`while`語句的內嵌語句中`break` ，可以使用語句（[break 語句](statements.md#the-break-statement)）將`while`控制項傳送至語句的結束點（因此會結束內嵌語句的反覆運算），而`continue`語句（[continue 語句](statements.md#the-continue-statement)）可用來將控制權轉移到內嵌語句的結束點（因而執行`while`語句的另一個反復專案）。</span><span class="sxs-lookup"><span data-stu-id="2c009-315">Within the embedded statement of a `while` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `while` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement (thus performing another iteration of the `while` statement).</span></span>

<span data-ttu-id="2c009-316">如果可以連線到`while` `while`語句，而且布林運算式沒有常數值`false`，則可以連接語句的內嵌語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-316">The embedded statement of a `while` statement is reachable if the `while` statement is reachable and the boolean expression does not have the constant value `false`.</span></span>

<span data-ttu-id="2c009-317">如果下列至少一項`while`為真，則語句的結束點可供連線：</span><span class="sxs-lookup"><span data-stu-id="2c009-317">The end point of a `while` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="2c009-318">語句包含可連接`while`的`break`語句，它會結束語句。 `while`</span><span class="sxs-lookup"><span data-stu-id="2c009-318">The `while` statement contains a reachable `break` statement that exits the `while` statement.</span></span>
*  <span data-ttu-id="2c009-319">可以`while`連線到語句，且布林運算式沒有常數值。 `true`</span><span class="sxs-lookup"><span data-stu-id="2c009-319">The `while` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-do-statement"></a><span data-ttu-id="2c009-320">Do 語句</span><span class="sxs-lookup"><span data-stu-id="2c009-320">The do statement</span></span>

<span data-ttu-id="2c009-321">`do`語句會有條件地執行內嵌語句一次或多次。</span><span class="sxs-lookup"><span data-stu-id="2c009-321">The `do` statement conditionally executes an embedded statement one or more times.</span></span>

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

<span data-ttu-id="2c009-322">`do`語句的執行方式如下：</span><span class="sxs-lookup"><span data-stu-id="2c009-322">A `do` statement is executed as follows:</span></span>

*  <span data-ttu-id="2c009-323">控制項會傳送至內嵌語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-323">Control is transferred to the embedded statement.</span></span>
*  <span data-ttu-id="2c009-324">當控制項到達內嵌語句的結束點（可能是從執行 `continue` 語句）時，就會評估*boolean_expression* （[布林運算式](expressions.md#boolean-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="2c009-324">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), the *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span> <span data-ttu-id="2c009-325">如果布林運算式產生`true`，控制權就會傳送至`do`語句的開頭。</span><span class="sxs-lookup"><span data-stu-id="2c009-325">If the boolean expression yields `true`, control is transferred to the beginning of the `do` statement.</span></span> <span data-ttu-id="2c009-326">否則，控制權會轉移到`do`語句的結束點。</span><span class="sxs-lookup"><span data-stu-id="2c009-326">Otherwise, control is transferred to the end point of the `do` statement.</span></span>

<span data-ttu-id="2c009-327">在`do`語句的內嵌語句中`break` ，可以使用語句（[break 語句](statements.md#the-break-statement)）將`do`控制項傳送至語句的結束點（因此會結束內嵌語句的反覆運算），而`continue`語句（[continue 語句](statements.md#the-continue-statement)）可用來將控制權轉移到內嵌語句的結束點。</span><span class="sxs-lookup"><span data-stu-id="2c009-327">Within the embedded statement of a `do` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `do` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement.</span></span>

<span data-ttu-id="2c009-328">如果可以連線到`do` `do`語句，則可以連接語句的內嵌語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-328">The embedded statement of a `do` statement is reachable if the `do` statement is reachable.</span></span>

<span data-ttu-id="2c009-329">如果下列至少一項`do`為真，則語句的結束點可供連線：</span><span class="sxs-lookup"><span data-stu-id="2c009-329">The end point of a `do` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="2c009-330">語句包含可連接`do`的`break`語句，它會結束語句。 `do`</span><span class="sxs-lookup"><span data-stu-id="2c009-330">The `do` statement contains a reachable `break` statement that exits the `do` statement.</span></span>
*  <span data-ttu-id="2c009-331">內嵌語句的結束點可供連線，且布林運算式沒有常數值`true`。</span><span class="sxs-lookup"><span data-stu-id="2c009-331">The end point of the embedded statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-for-statement"></a><span data-ttu-id="2c009-332">For 語句</span><span class="sxs-lookup"><span data-stu-id="2c009-332">The for statement</span></span>

<span data-ttu-id="2c009-333">`for`語句會評估初始化運算式的序列，然後，當條件為 true 時，會重複執行內嵌語句，並評估反覆運算運算式的序列。</span><span class="sxs-lookup"><span data-stu-id="2c009-333">The `for` statement evaluates a sequence of initialization expressions and then, while a condition is true, repeatedly executes an embedded statement and evaluates a sequence of iteration expressions.</span></span>

```antlr
for_statement
    : 'for' '(' for_initializer? ';' for_condition? ';' for_iterator? ')' embedded_statement
    ;

for_initializer
    : local_variable_declaration
    | statement_expression_list
    ;

for_condition
    : boolean_expression
    ;

for_iterator
    : statement_expression_list
    ;

statement_expression_list
    : statement_expression (',' statement_expression)*
    ;
```

<span data-ttu-id="2c009-334">*For_initializer*（如果有的話）是由*local_variable_declaration* （[區域變數](statements.md#local-variable-declarations)宣告）或*statement_expression*s （[expression 語句](statements.md#expression-statements)）清單（以逗號分隔）所組成。</span><span class="sxs-lookup"><span data-stu-id="2c009-334">The *for_initializer*, if present, consists of either a *local_variable_declaration* ([Local variable declarations](statements.md#local-variable-declarations)) or a list of *statement_expression*s ([Expression statements](statements.md#expression-statements)) separated by commas.</span></span> <span data-ttu-id="2c009-335">*For_initializer*所宣告的區域變數範圍會從變數的*local_variable_declarator*開始，並延伸至內嵌語句的結尾。</span><span class="sxs-lookup"><span data-stu-id="2c009-335">The scope of a local variable declared by a *for_initializer* starts at the *local_variable_declarator* for the variable and extends to the end of the embedded statement.</span></span> <span data-ttu-id="2c009-336">範圍包含*for_condition*和*for_iterator*。</span><span class="sxs-lookup"><span data-stu-id="2c009-336">The scope includes the *for_condition* and the *for_iterator*.</span></span>

<span data-ttu-id="2c009-337">*For_condition*（如果有的話）必須是*boolean_expression* （[布林運算式](expressions.md#boolean-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="2c009-337">The *for_condition*, if present, must be a *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)).</span></span>

<span data-ttu-id="2c009-338">*For_iterator*（如果有的話）包含以逗號分隔的*Statement_expression*s （[expression 語句](statements.md#expression-statements)）清單。</span><span class="sxs-lookup"><span data-stu-id="2c009-338">The *for_iterator*, if present, consists of a list of *statement_expression*s ([Expression statements](statements.md#expression-statements)) separated by commas.</span></span>

<span data-ttu-id="2c009-339">For 語句的執行方式如下：</span><span class="sxs-lookup"><span data-stu-id="2c009-339">A for statement is executed as follows:</span></span>

*  <span data-ttu-id="2c009-340">如果*for_initializer*存在，變數初始化運算式或語句運算式就會依撰寫的循序執行。</span><span class="sxs-lookup"><span data-stu-id="2c009-340">If a *for_initializer* is present, the variable initializers or statement expressions are executed in the order they are written.</span></span> <span data-ttu-id="2c009-341">此步驟只會執行一次。</span><span class="sxs-lookup"><span data-stu-id="2c009-341">This step is only performed once.</span></span>
*  <span data-ttu-id="2c009-342">如果*for_condition*存在，則會進行評估。</span><span class="sxs-lookup"><span data-stu-id="2c009-342">If a *for_condition* is present, it is evaluated.</span></span>
*  <span data-ttu-id="2c009-343">如果*for_condition*不存在，或評估產生 `true`，則會將控制權轉移到內嵌語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-343">If the *for_condition* is not present or if the evaluation yields `true`, control is transferred to the embedded statement.</span></span> <span data-ttu-id="2c009-344">當控制項到達內嵌語句的結束點（可能是從執行 @no__t 0 的語句）時，會依序評估*for_iterator*的運算式（如果有的話），然後再執行另一個反復專案，從上述步驟中的*for_condition*評估。</span><span class="sxs-lookup"><span data-stu-id="2c009-344">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), the expressions of the *for_iterator*, if any, are evaluated in sequence, and then another iteration is performed, starting with evaluation of the *for_condition* in the step above.</span></span>
*  <span data-ttu-id="2c009-345">如果*for_condition*存在，且評估產生 `false`，則控制權會轉移至 @no__t 2 語句的結束點。</span><span class="sxs-lookup"><span data-stu-id="2c009-345">If the *for_condition* is present and the evaluation yields `false`, control is transferred to the end point of the `for` statement.</span></span>

<span data-ttu-id="2c009-346">在 `for` 語句的 embedded 語句中，@no__t 1 語句（[break 語句](statements.md#the-break-statement)）可以用來將控制權轉移至 `for` 語句的結束點（因此，會結束內嵌語句的反復專案）和 `continue` 語句（[Continue 語句](statements.md#the-continue-statement)）可用來將控制權轉移到內嵌語句的結束點（因此，從*for_condition*開始，執行*for_iterator*並執行 `for` 語句的另一個反復專案）。</span><span class="sxs-lookup"><span data-stu-id="2c009-346">Within the embedded statement of a `for` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `for` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement (thus executing the *for_iterator* and performing another iteration of the `for` statement, starting with the *for_condition*).</span></span>

<span data-ttu-id="2c009-347">如果下列其中一項`for`為真，就可以觸達語句的內嵌語句：</span><span class="sxs-lookup"><span data-stu-id="2c009-347">The embedded statement of a `for` statement is reachable if one of the following is true:</span></span>

*  <span data-ttu-id="2c009-348">@No__t-0 語句可供連線，而且沒有任何*for_condition*存在。</span><span class="sxs-lookup"><span data-stu-id="2c009-348">The `for` statement is reachable and no *for_condition* is present.</span></span>
*  <span data-ttu-id="2c009-349">@No__t-0 語句可供連線，而且有*for_condition*存在，而且沒有常數值 `false`。</span><span class="sxs-lookup"><span data-stu-id="2c009-349">The `for` statement is reachable and a *for_condition* is present and does not have the constant value `false`.</span></span>

<span data-ttu-id="2c009-350">如果下列至少一項`for`為真，則語句的結束點可供連線：</span><span class="sxs-lookup"><span data-stu-id="2c009-350">The end point of a `for` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="2c009-351">語句包含可連接`for`的`break`語句，它會結束語句。 `for`</span><span class="sxs-lookup"><span data-stu-id="2c009-351">The `for` statement contains a reachable `break` statement that exits the `for` statement.</span></span>
*  <span data-ttu-id="2c009-352">@No__t-0 語句可供連線，而且有*for_condition*存在，而且沒有常數值 `true`。</span><span class="sxs-lookup"><span data-stu-id="2c009-352">The `for` statement is reachable and a *for_condition* is present and does not have the constant value `true`.</span></span>

### <a name="the-foreach-statement"></a><span data-ttu-id="2c009-353">Foreach 語句</span><span class="sxs-lookup"><span data-stu-id="2c009-353">The foreach statement</span></span>

<span data-ttu-id="2c009-354">`foreach`語句會列舉集合的元素，並針對集合的每個專案執行內嵌語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-354">The `foreach` statement enumerates the elements of a collection, executing an embedded statement for each element of the collection.</span></span>

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

<span data-ttu-id="2c009-355">`foreach`語句的*類型*和*識別碼*會宣告語句的***反覆運算變數***。</span><span class="sxs-lookup"><span data-stu-id="2c009-355">The *type* and *identifier* of a `foreach` statement declare the ***iteration variable*** of the statement.</span></span> <span data-ttu-id="2c009-356">如果 @no__t 0 的識別碼指定為*local_variable_type*，而且沒有任何名為 `var` 的類型在範圍內，則反復專案變數就會被視為***隱含類型的反復專案變數***，而且其類型會被視為 `foreach` 的元素類型。語句，如下所示。</span><span class="sxs-lookup"><span data-stu-id="2c009-356">If the `var` identifier is given as the *local_variable_type*, and no type named `var` is in scope, the iteration variable is said to be an ***implicitly typed iteration variable***, and its type is taken to be the element type of the `foreach` statement, as specified below.</span></span> <span data-ttu-id="2c009-357">反覆運算變數會對應到唯讀區域變數，其範圍會延伸到內嵌語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-357">The iteration variable corresponds to a read-only local variable with a scope that extends over the embedded statement.</span></span> <span data-ttu-id="2c009-358">在`foreach`語句執行期間，反覆運算變數代表目前正在執行反復專案的集合元素。</span><span class="sxs-lookup"><span data-stu-id="2c009-358">During execution of a `foreach` statement, the iteration variable represents the collection element for which an iteration is currently being performed.</span></span> <span data-ttu-id="2c009-359">如果內嵌的語句嘗試修改反復專案變數（透過指派`++`或和`--`運算子），或將反覆運算變數`ref`當做或`out`參數傳遞，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2c009-359">A compile-time error occurs if the embedded statement attempts to modify the iteration variable (via assignment or the `++` and `--` operators) or pass the iteration variable as a `ref` or `out` parameter.</span></span>

<span data-ttu-id="2c009-360">在下列中，為了簡單起見`IEnumerable`， `IEnumerator` `IEnumerable<T>` 、和`IEnumerator<T>`會參考命名空間`System.Collections`和`System.Collections.Generic`中的對應類型。</span><span class="sxs-lookup"><span data-stu-id="2c009-360">In the following, for brevity, `IEnumerable`, `IEnumerator`, `IEnumerable<T>` and `IEnumerator<T>` refer to the corresponding types in the namespaces `System.Collections` and `System.Collections.Generic`.</span></span>

<span data-ttu-id="2c009-361">Foreach 語句的編譯時間處理會先決定運算式的***集合類型***、***列舉數值型別***和***元素類型***。</span><span class="sxs-lookup"><span data-stu-id="2c009-361">The compile-time processing of a foreach statement first determines the ***collection type***, ***enumerator type*** and ***element type*** of the expression.</span></span> <span data-ttu-id="2c009-362">這項決定會繼續進行，如下所示：</span><span class="sxs-lookup"><span data-stu-id="2c009-362">This determination proceeds as follows:</span></span>

*  <span data-ttu-id="2c009-363">如果運算式的`X`類型是陣列類型，則會有從`X`到`IEnumerable`介面的隱含參考轉換（因為`System.Array`會執行此介面）。</span><span class="sxs-lookup"><span data-stu-id="2c009-363">If the type `X` of *expression* is an array type then there is an implicit reference conversion from `X` to the `IEnumerable` interface (since `System.Array` implements this interface).</span></span> <span data-ttu-id="2c009-364">***集合類型*** `IEnumerable`為介面，***列舉數值型別***為`IEnumerator`介面，而***元素類型***為數組類型`X`的元素類型。</span><span class="sxs-lookup"><span data-stu-id="2c009-364">The ***collection type*** is the `IEnumerable` interface, the ***enumerator type*** is the `IEnumerator` interface and the ***element type*** is the element type of the array type `X`.</span></span>
*  <span data-ttu-id="2c009-365">如果運算式的`X`類型 `dynamic`是，則會隱含地從*運算式*轉換成`IEnumerable`介面（隱含的[動態轉換](conversions.md#implicit-dynamic-conversions)）。</span><span class="sxs-lookup"><span data-stu-id="2c009-365">If the type `X` of *expression* is `dynamic` then there is an implicit conversion from *expression* to the `IEnumerable` interface ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions)).</span></span> <span data-ttu-id="2c009-366">***集合類型***為`IEnumerable`介面，而列舉值***類型***為`IEnumerator`介面。</span><span class="sxs-lookup"><span data-stu-id="2c009-366">The ***collection type*** is the `IEnumerable` interface and the ***enumerator type*** is the `IEnumerator` interface.</span></span> <span data-ttu-id="2c009-367">如果 `var` 識別碼指定為*local_variable_type* ，則***元素類型***會 `dynamic`，否則會 `object`。</span><span class="sxs-lookup"><span data-stu-id="2c009-367">If the `var` identifier is given as the *local_variable_type* then the ***element type*** is `dynamic`, otherwise it is `object`.</span></span>
*  <span data-ttu-id="2c009-368">否則，請判斷類型`X`是否具有適當`GetEnumerator`的方法：</span><span class="sxs-lookup"><span data-stu-id="2c009-368">Otherwise, determine whether the type `X` has an appropriate `GetEnumerator` method:</span></span>
   * <span data-ttu-id="2c009-369">在具有識別碼`GetEnumerator`和沒有類型`X`引數的類型上執行成員查閱。</span><span class="sxs-lookup"><span data-stu-id="2c009-369">Perform member lookup on the type `X` with identifier `GetEnumerator` and no type arguments.</span></span> <span data-ttu-id="2c009-370">如果成員查閱不會產生符合的結果，或產生不明確的結果，或產生不是方法群組的比對，請檢查是否有可列舉的介面，如下所述。</span><span class="sxs-lookup"><span data-stu-id="2c009-370">If the member lookup does not produce a match, or it produces an ambiguity, or produces a match that is not a method group, check for an enumerable interface as described below.</span></span> <span data-ttu-id="2c009-371">如果成員查閱除了方法群組以外，或沒有相符專案以外，建議您發出警告。</span><span class="sxs-lookup"><span data-stu-id="2c009-371">It is recommended that a warning be issued if member lookup produces anything except a method group or no match.</span></span>
   * <span data-ttu-id="2c009-372">使用產生的方法群組和空的引數清單來執行多載解析。</span><span class="sxs-lookup"><span data-stu-id="2c009-372">Perform overload resolution using the resulting method group and an empty argument list.</span></span> <span data-ttu-id="2c009-373">如果多載解析不會產生適用的方法、導致不明確的情況，或產生單一最佳方法，但該方法是靜態或非公用的，請檢查是否有可列舉的介面，如下所述。</span><span class="sxs-lookup"><span data-stu-id="2c009-373">If overload resolution results in no applicable methods, results in an ambiguity, or results in a single best method but that method is either static or not public, check for an enumerable interface as described below.</span></span> <span data-ttu-id="2c009-374">如果多載解析除了明確的公用實例方法以外，或沒有適用的方法，建議您發出警告。</span><span class="sxs-lookup"><span data-stu-id="2c009-374">It is recommended that a warning be issued if overload resolution produces anything except an unambiguous public instance method or no applicable methods.</span></span>
   * <span data-ttu-id="2c009-375">如果`GetEnumerator`方法的傳回`E`型別不是類別、結構或介面型別，就會產生錯誤，而且不會採取任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="2c009-375">If the return type `E` of the `GetEnumerator` method is not a class, struct or interface type, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="2c009-376">成員查閱是在上`E`執行，且`Current`具有識別碼，而且沒有類型引數。</span><span class="sxs-lookup"><span data-stu-id="2c009-376">Member lookup is performed on `E` with the identifier `Current` and no type arguments.</span></span> <span data-ttu-id="2c009-377">如果成員查閱不會產生相符專案，則結果會是錯誤，或結果是除了允許讀取的公用實例屬性以外的任何專案，會產生錯誤，而且不會採取任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="2c009-377">If the member lookup produces no match, the result is an error, or the result is anything except a public instance property that permits reading, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="2c009-378">成員查閱是在上`E`執行，且`MoveNext`具有識別碼，而且沒有類型引數。</span><span class="sxs-lookup"><span data-stu-id="2c009-378">Member lookup is performed on `E` with the identifier `MoveNext` and no type arguments.</span></span> <span data-ttu-id="2c009-379">如果成員查閱不會產生相符專案，則結果會是錯誤，或結果是除了方法群組以外的任何專案，會產生錯誤，而且不會採取進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="2c009-379">If the member lookup produces no match, the result is an error, or the result is anything except a method group, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="2c009-380">多載解析是在具有空白引數清單的方法群組上執行。</span><span class="sxs-lookup"><span data-stu-id="2c009-380">Overload resolution is performed on the method group with an empty argument list.</span></span> <span data-ttu-id="2c009-381">如果多載解析不會產生適用的方法、導致不明確的結果，或產生單一最佳方法，但該方法是靜態或非公用的，或其傳回型`bool`別不是，則會產生錯誤，而且不會採取任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="2c009-381">If overload resolution results in no applicable methods, results in an ambiguity, or results in a single best method but that method is either static or not public, or its return type is not `bool`, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="2c009-382">***集合型***別為`X`，***列舉值型***別`E`為， `Current`而***元素型***別為屬性的型別。</span><span class="sxs-lookup"><span data-stu-id="2c009-382">The ***collection type*** is `X`, the ***enumerator type*** is `E`, and the ***element type*** is the type of the `Current` property.</span></span>

*  <span data-ttu-id="2c009-383">否則，請檢查可列舉的介面：</span><span class="sxs-lookup"><span data-stu-id="2c009-383">Otherwise, check for an enumerable interface:</span></span>
   * <span data-ttu-id="2c009-384">如果`Ti`所有類型之間有隱含的從`X`轉換成`IEnumerable<Ti>`的型別`T` ，則會有唯一的類型`T` ，這不`dynamic`是，而且所有其他`Ti`都有從`IEnumerable<T>`到`IEnumerator<T>` `IEnumerable<T>`的隱含轉換，則集合類型為介面、列舉數值型別為介面，而元素類型為`IEnumerable<Ti>` `T`.</span><span class="sxs-lookup"><span data-stu-id="2c009-384">If among all the types `Ti` for which there is an implicit conversion from `X` to `IEnumerable<Ti>`, there is a unique type `T` such that `T` is not `dynamic` and for all the other `Ti` there is an implicit conversion from `IEnumerable<T>` to `IEnumerable<Ti>`, then the ***collection type*** is the interface `IEnumerable<T>`, the ***enumerator type*** is the interface `IEnumerator<T>`, and the ***element type*** is `T`.</span></span>
   * <span data-ttu-id="2c009-385">否則，如果有多個這種類型`T`，則會產生錯誤，而且不會採取任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="2c009-385">Otherwise, if there is more than one such type `T`, then an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="2c009-386">否則，如果有`X`從`System.Collections.IEnumerable`到介面的隱含轉換，則***集合類型***為此介面、***列舉數值型別***為介面`System.Collections.IEnumerator`，而***元素類型***為`object`.</span><span class="sxs-lookup"><span data-stu-id="2c009-386">Otherwise, if there is an implicit conversion from `X` to the `System.Collections.IEnumerable` interface, then the ***collection type*** is this interface, the ***enumerator type*** is the interface `System.Collections.IEnumerator`, and the ***element type*** is `object`.</span></span>
   * <span data-ttu-id="2c009-387">否則會產生錯誤，而且不會採取任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="2c009-387">Otherwise, an error is produced and no further steps are taken.</span></span>

<span data-ttu-id="2c009-388">如果成功，上述步驟會明確產生集合類型`C`、列舉數值型別`E`和元素類型`T`。</span><span class="sxs-lookup"><span data-stu-id="2c009-388">The above steps, if successful, unambiguously produce a collection type `C`, enumerator type `E` and element type `T`.</span></span> <span data-ttu-id="2c009-389">表單的 foreach 語句</span><span class="sxs-lookup"><span data-stu-id="2c009-389">A foreach statement of the form</span></span>
```csharp
foreach (V v in x) embedded_statement
```
<span data-ttu-id="2c009-390">接著會展開為：</span><span class="sxs-lookup"><span data-stu-id="2c009-390">is then expanded to:</span></span>
```csharp
{
    E e = ((C)(x)).GetEnumerator();
    try {
        while (e.MoveNext()) {
            V v = (V)(T)e.Current;
            embedded_statement
        }
    }
    finally {
        ... // Dispose e
    }
}
```

<span data-ttu-id="2c009-391">`e` 運算式`x`或內嵌語句或程式的任何其他原始程式碼都看不到或無法存取變數。</span><span class="sxs-lookup"><span data-stu-id="2c009-391">The variable `e` is not visible to or accessible to the expression `x` or the embedded statement or any other source code of the program.</span></span> <span data-ttu-id="2c009-392">此變數`v`在內嵌語句中是唯讀的。</span><span class="sxs-lookup"><span data-stu-id="2c009-392">The variable `v` is read-only in the embedded statement.</span></span> <span data-ttu-id="2c009-393">如果沒有從 `T` （專案類型）到 `V` （foreach 語句中的*local_variable_type* ）的明確轉換（[明確](conversions.md#explicit-conversions)轉換），就會產生錯誤，而且不會採取任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="2c009-393">If there is not an explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) from `T` (the element type) to `V` (the *local_variable_type* in the foreach statement), an error is produced and no further steps are taken.</span></span> <span data-ttu-id="2c009-394">如果`x`的值`null`為， `System.NullReferenceException`則會在執行時間擲回。</span><span class="sxs-lookup"><span data-stu-id="2c009-394">If `x` has the value `null`, a `System.NullReferenceException` is thrown at run-time.</span></span>

<span data-ttu-id="2c009-395">實作為執行指定的 foreach 語句有不同的方式，例如基於效能的考慮，前提是該行為與上述擴充一致。</span><span class="sxs-lookup"><span data-stu-id="2c009-395">An implementation is permitted to implement a given foreach-statement differently, e.g. for performance reasons, as long as the behavior is consistent with the above expansion.</span></span>

<span data-ttu-id="2c009-396">While 迴圈內 `v` 的位置，對於*embedded_statement*中發生的任何匿名函式而言，都很重要。</span><span class="sxs-lookup"><span data-stu-id="2c009-396">The placement of `v` inside the while loop is important for how it is captured by any anonymous function occurring in the *embedded_statement*.</span></span>

<span data-ttu-id="2c009-397">例如:</span><span class="sxs-lookup"><span data-stu-id="2c009-397">For example:</span></span>
```csharp
int[] values = { 7, 9, 13 };
Action f = null;

foreach (var value in values)
{
    if (f == null) f = () => Console.WriteLine("First value: " + value);
}

f();
```
<span data-ttu-id="2c009-398">如果`v`是在 while 迴圈外宣告，則會在所有反復專案之間共用，而在 for 迴圈之後的值會是最後的`13`值，這`f`就是的調用會列印的結果。</span><span class="sxs-lookup"><span data-stu-id="2c009-398">If `v` was declared outside of the while loop, it would be shared among all iterations, and its value after the for loop would be the final value, `13`, which is what the invocation of `f` would print.</span></span> <span data-ttu-id="2c009-399">相反地，因為每個反復專案都`v`有自己的變數， `f`所以第一個反復專案中所捕捉的會`7`繼續保留值，這就是要列印的內容。</span><span class="sxs-lookup"><span data-stu-id="2c009-399">Instead, because each iteration has its own variable `v`, the one captured by `f` in the first iteration will continue to hold the value `7`, which is what will be printed.</span></span> <span data-ttu-id="2c009-400">（注意：在 while 迴圈C# `v`外宣告的舊版。）</span><span class="sxs-lookup"><span data-stu-id="2c009-400">(Note: earlier versions of C# declared `v` outside of the while loop.)</span></span>

<span data-ttu-id="2c009-401">Finally 區塊的主體會根據下列步驟來進行結構化：</span><span class="sxs-lookup"><span data-stu-id="2c009-401">The body of the finally block is constructed according to the following steps:</span></span>

*  <span data-ttu-id="2c009-402">如果有從`E` `System.IDisposable`到介面的隱含轉換，則</span><span class="sxs-lookup"><span data-stu-id="2c009-402">If there is an implicit conversion from `E` to the `System.IDisposable` interface, then</span></span>
   *  <span data-ttu-id="2c009-403">如果`E`是不可為 null 的實數值型別，則 finally 子句會展開為對等的語義：</span><span class="sxs-lookup"><span data-stu-id="2c009-403">If `E` is a non-nullable value type then the finally clause is expanded to the semantic equivalent  of:</span></span>

      ```csharp
      finally {
          ((System.IDisposable)e).Dispose();
      }
      ```

   *  <span data-ttu-id="2c009-404">否則，finally 子句會展開為的語義對應項：</span><span class="sxs-lookup"><span data-stu-id="2c009-404">Otherwise the finally clause is expanded to the semantic equivalent of:</span></span>

      ```csharp
      finally {
          if (e != null) ((System.IDisposable)e).Dispose();
      }
      ```

   <span data-ttu-id="2c009-405">但如果`E`是實值型別，或具現化為實值型別的`e`型別參數，則將`System.IDisposable`轉換成將不會造成裝箱。</span><span class="sxs-lookup"><span data-stu-id="2c009-405">except that if `E` is a value type, or a type parameter instantiated to a value type, then the cast of `e` to `System.IDisposable` will not cause boxing to occur.</span></span>

*  <span data-ttu-id="2c009-406">否則，如果`E`是密封型別，則 finally 子句會展開為空的區塊：</span><span class="sxs-lookup"><span data-stu-id="2c009-406">Otherwise, if `E` is a sealed type, the finally clause is expanded to an empty block:</span></span>

   ```csharp
   finally {
   }
   ```

*  <span data-ttu-id="2c009-407">否則，finally 子句會展開為：</span><span class="sxs-lookup"><span data-stu-id="2c009-407">Otherwise, the finally clause is expanded to:</span></span>

   ```csharp
   finally {
       System.IDisposable d = e as System.IDisposable;
       if (d != null) d.Dispose();
   }
   ```    

   <span data-ttu-id="2c009-408">任何使用者程式`d`代碼都看不到或無法存取本機變數。</span><span class="sxs-lookup"><span data-stu-id="2c009-408">The local variable `d` is not visible to or accessible to any user code.</span></span> <span data-ttu-id="2c009-409">特別的是，它不會與範圍包含 finally 區塊的任何其他變數發生衝突。</span><span class="sxs-lookup"><span data-stu-id="2c009-409">In particular, it does not conflict with any other variable whose scope includes the finally block.</span></span>

<span data-ttu-id="2c009-410">`foreach`遍歷陣列元素的順序如下所示：對於一維陣列元素，會以遞增的索引順序來進行遍歷， `0`從索引開始， `Length - 1`並以索引結尾。</span><span class="sxs-lookup"><span data-stu-id="2c009-410">The order in which `foreach` traverses the elements of an array, is as follows: For single-dimensional arrays elements are traversed in increasing index order, starting with index `0` and ending with index `Length - 1`.</span></span> <span data-ttu-id="2c009-411">若為多維陣列，則會將專案進行遍歷，讓最右邊維度的索引先增加，然後是下一個左邊的維度，依此類推。</span><span class="sxs-lookup"><span data-stu-id="2c009-411">For multi-dimensional arrays, elements are traversed such that the indices of the rightmost dimension are increased first, then the next left dimension, and so on to the left.</span></span>

<span data-ttu-id="2c009-412">下列範例會依照元素順序，在二維陣列中列印每個值：</span><span class="sxs-lookup"><span data-stu-id="2c009-412">The following example prints out each value in a two-dimensional array, in element order:</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        double[,] values = {
            {1.2, 2.3, 3.4, 4.5},
            {5.6, 6.7, 7.8, 8.9}
        };

        foreach (double elementValue in values)
            Console.Write("{0} ", elementValue);

        Console.WriteLine();
    }
}
```
<span data-ttu-id="2c009-413">產生的輸出如下所示：</span><span class="sxs-lookup"><span data-stu-id="2c009-413">The output produced is as follows:</span></span>
```console
1.2 2.3 3.4 4.5 5.6 6.7 7.8 8.9
```

<span data-ttu-id="2c009-414">在範例中</span><span class="sxs-lookup"><span data-stu-id="2c009-414">In the example</span></span>
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
<span data-ttu-id="2c009-415">的類型`n`會推斷`int`為`numbers`，其為的元素類型。</span><span class="sxs-lookup"><span data-stu-id="2c009-415">the type of `n` is inferred to be `int`, the element type of `numbers`.</span></span>

## <a name="jump-statements"></a><span data-ttu-id="2c009-416">跳躍陳述式</span><span class="sxs-lookup"><span data-stu-id="2c009-416">Jump statements</span></span>

<span data-ttu-id="2c009-417">跳躍語句會無條件地傳輸控制權。</span><span class="sxs-lookup"><span data-stu-id="2c009-417">Jump statements unconditionally transfer control.</span></span>

```antlr
jump_statement
    : break_statement
    | continue_statement
    | goto_statement
    | return_statement
    | throw_statement
    ;
```

<span data-ttu-id="2c009-418">跳躍語句傳送控制項的位置稱為跳躍語句的***目標***。</span><span class="sxs-lookup"><span data-stu-id="2c009-418">The location to which a jump statement transfers control is called the ***target*** of the jump statement.</span></span>

<span data-ttu-id="2c009-419">當跳躍語句出現在區塊內，而且該跳躍陳述式的目標位於該區塊外時，跳躍語句就會被視為結束區塊。</span><span class="sxs-lookup"><span data-stu-id="2c009-419">When a jump statement occurs within a block, and the target of that jump statement is outside that block, the jump statement is said to ***exit*** the block.</span></span> <span data-ttu-id="2c009-420">雖然跳躍語句可能會將控制項從區塊轉移出來，但它無法將控制權轉移到區塊中。</span><span class="sxs-lookup"><span data-stu-id="2c009-420">While a jump statement may transfer control out of a block, it can never transfer control into a block.</span></span>

<span data-ttu-id="2c009-421">執行跳躍語句會因為中間`try`語句的存在而變得很複雜。</span><span class="sxs-lookup"><span data-stu-id="2c009-421">Execution of jump statements is complicated by the presence of intervening `try` statements.</span></span> <span data-ttu-id="2c009-422">如果沒有這類`try`語句，跳躍語句會無條件地將控制權從跳躍語句轉移到其目標。</span><span class="sxs-lookup"><span data-stu-id="2c009-422">In the absence of such `try` statements, a jump statement unconditionally transfers control from the jump statement to its target.</span></span> <span data-ttu-id="2c009-423">在存在這類中間`try`語句時，執行會更複雜。</span><span class="sxs-lookup"><span data-stu-id="2c009-423">In the presence of such intervening `try` statements, execution is more complex.</span></span> <span data-ttu-id="2c009-424">如果跳躍語句結束一個或多個`try`具有相關聯`finally`區塊的區塊， `finally`則控制權一開始會傳送到最`try`內層語句的區塊。</span><span class="sxs-lookup"><span data-stu-id="2c009-424">If the jump statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="2c009-425">當控制項到達`finally`區塊的結束點時，控制權會轉移`finally`到下一個封入`try`語句的區塊。</span><span class="sxs-lookup"><span data-stu-id="2c009-425">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="2c009-426">這個程式會重複執行， `finally`直到所有中間`try`語句的區塊都已執行為止。</span><span class="sxs-lookup"><span data-stu-id="2c009-426">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>

<span data-ttu-id="2c009-427">在範例中</span><span class="sxs-lookup"><span data-stu-id="2c009-427">In the example</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        while (true) {
            try {
                try {
                    Console.WriteLine("Before break");
                    break;
                }
                finally {
                    Console.WriteLine("Innermost finally block");
                }
            }
            finally {
                Console.WriteLine("Outermost finally block");
            }
        }
        Console.WriteLine("After break");
    }
}
```
<span data-ttu-id="2c009-428">在`finally`控制權轉移至跳躍`try`語句的目標之前，會執行與兩個語句相關聯的區塊。</span><span class="sxs-lookup"><span data-stu-id="2c009-428">the `finally` blocks associated with two `try` statements are executed before control is transferred to the target of the jump statement.</span></span>

<span data-ttu-id="2c009-429">產生的輸出如下所示：</span><span class="sxs-lookup"><span data-stu-id="2c009-429">The output produced is as follows:</span></span>
```console
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a><span data-ttu-id="2c009-430">Break 語句</span><span class="sxs-lookup"><span data-stu-id="2c009-430">The break statement</span></span>

<span data-ttu-id="2c009-431">`for` `do` `switch` `while`語句會結束最接近的封閉式、、、或`foreach`語句。 `break`</span><span class="sxs-lookup"><span data-stu-id="2c009-431">The `break` statement exits the nearest enclosing `switch`, `while`, `do`, `for`, or `foreach` statement.</span></span>

```antlr
break_statement
    : 'break' ';'
    ;
```

<span data-ttu-id="2c009-432">`break`語句的目標是`switch`最接近之封入、 `for` `while` `do`、、或`foreach`語句的結束點。</span><span class="sxs-lookup"><span data-stu-id="2c009-432">The target of a `break` statement is the end point of the nearest enclosing `switch`, `while`, `do`, `for`, or `foreach` statement.</span></span> <span data-ttu-id="2c009-433">`switch` `while`如果語句不是`for`以、、 `do`、或`foreach`語句括住，就會發生編譯時期錯誤。 `break`</span><span class="sxs-lookup"><span data-stu-id="2c009-433">If a `break` statement is not enclosed by a `switch`, `while`, `do`, `for`, or `foreach` statement, a compile-time error occurs.</span></span>

<span data-ttu-id="2c009-434">當多`switch`個`while` `break` 、、、或`foreach`語句彼此嵌套時，語句只會套用到最內層的語句。 `do` `for`</span><span class="sxs-lookup"><span data-stu-id="2c009-434">When multiple `switch`, `while`, `do`, `for`, or `foreach` statements are nested within each other, a `break` statement applies only to the innermost statement.</span></span> <span data-ttu-id="2c009-435">若要在多個嵌套層級之間`goto`傳送控制，必須使用語句（[goto 語句](statements.md#the-goto-statement)）。</span><span class="sxs-lookup"><span data-stu-id="2c009-435">To transfer control across multiple nesting levels, a `goto` statement ([The goto statement](statements.md#the-goto-statement)) must be used.</span></span>

<span data-ttu-id="2c009-436">語句無法結束區塊（[try 語句）。](statements.md#the-try-statement) `finally` `break`</span><span class="sxs-lookup"><span data-stu-id="2c009-436">A `break` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="2c009-437">當語句在區塊內發生時`break` ，語句的目標必須在相同`finally`的區塊內，否則會發生編譯時期錯誤。 `finally` `break`</span><span class="sxs-lookup"><span data-stu-id="2c009-437">When a `break` statement occurs within a `finally` block, the target of the `break` statement must be within the same `finally` block; otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="2c009-438">`break`語句的執行方式如下：</span><span class="sxs-lookup"><span data-stu-id="2c009-438">A `break` statement is executed as follows:</span></span>

*  <span data-ttu-id="2c009-439">`try` `finally` `finally` `try`如果語句結束一個或多個具有相關聯區塊的區塊，則控制權一開始會傳送到最內層語句的區塊。 `break`</span><span class="sxs-lookup"><span data-stu-id="2c009-439">If the `break` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="2c009-440">當控制項到達`finally`區塊的結束點時，控制權會轉移`finally`到下一個封入`try`語句的區塊。</span><span class="sxs-lookup"><span data-stu-id="2c009-440">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="2c009-441">這個程式會重複執行， `finally`直到所有中間`try`語句的區塊都已執行為止。</span><span class="sxs-lookup"><span data-stu-id="2c009-441">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="2c009-442">控制權會傳送至`break`語句的目標。</span><span class="sxs-lookup"><span data-stu-id="2c009-442">Control is transferred to the target of the `break` statement.</span></span>

<span data-ttu-id="2c009-443">因為語句會無條件地將控制權轉移到別處，所以永遠`break`無法連線到語句的結束點。 `break`</span><span class="sxs-lookup"><span data-stu-id="2c009-443">Because a `break` statement unconditionally transfers control elsewhere, the end point of a `break` statement is never reachable.</span></span>

### <a name="the-continue-statement"></a><span data-ttu-id="2c009-444">Continue 語句</span><span class="sxs-lookup"><span data-stu-id="2c009-444">The continue statement</span></span>

<span data-ttu-id="2c009-445">`do` `while` `foreach` `for`語句會啟動最接近之封閉式、、或語句的新反復專案。 `continue`</span><span class="sxs-lookup"><span data-stu-id="2c009-445">The `continue` statement starts a new iteration of the nearest enclosing `while`, `do`, `for`, or `foreach` statement.</span></span>

```antlr
continue_statement
    : 'continue' ';'
    ;
```

<span data-ttu-id="2c009-446">`continue`語句的目標是最接近之封閉式`while`、 `do`、 `for`或`foreach`語句之內嵌語句的結束點。</span><span class="sxs-lookup"><span data-stu-id="2c009-446">The target of a `continue` statement is the end point of the embedded statement of the nearest enclosing `while`, `do`, `for`, or `foreach` statement.</span></span> <span data-ttu-id="2c009-447">`do` `for`如果語句不是以`while`、、或`foreach`語句括住，就會發生編譯時期錯誤。 `continue`</span><span class="sxs-lookup"><span data-stu-id="2c009-447">If a `continue` statement is not enclosed by a `while`, `do`, `for`, or `foreach` statement, a compile-time error occurs.</span></span>

<span data-ttu-id="2c009-448">當多`while`個`do`、 `for`、 `continue`或`foreach`語句彼此嵌套時，語句只會套用到最內層的語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-448">When multiple `while`, `do`, `for`, or `foreach` statements are nested within each other, a `continue` statement applies only to the innermost statement.</span></span> <span data-ttu-id="2c009-449">若要在多個嵌套層級之間`goto`傳送控制，必須使用語句（[goto 語句](statements.md#the-goto-statement)）。</span><span class="sxs-lookup"><span data-stu-id="2c009-449">To transfer control across multiple nesting levels, a `goto` statement ([The goto statement](statements.md#the-goto-statement)) must be used.</span></span>

<span data-ttu-id="2c009-450">語句無法結束區塊（[try 語句）。](statements.md#the-try-statement) `finally` `continue`</span><span class="sxs-lookup"><span data-stu-id="2c009-450">A `continue` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="2c009-451">當語句在區塊內發生時`continue` ，語句的目標必須在相同`finally`的區塊內，否則會發生編譯時期錯誤。 `finally` `continue`</span><span class="sxs-lookup"><span data-stu-id="2c009-451">When a `continue` statement occurs within a `finally` block, the target of the `continue` statement must be within the same `finally` block; otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="2c009-452">`continue`語句的執行方式如下：</span><span class="sxs-lookup"><span data-stu-id="2c009-452">A `continue` statement is executed as follows:</span></span>

*  <span data-ttu-id="2c009-453">`try` `finally` `finally` `try`如果語句結束一個或多個具有相關聯區塊的區塊，則控制權一開始會傳送到最內層語句的區塊。 `continue`</span><span class="sxs-lookup"><span data-stu-id="2c009-453">If the `continue` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="2c009-454">當控制項到達`finally`區塊的結束點時，控制權會轉移`finally`到下一個封入`try`語句的區塊。</span><span class="sxs-lookup"><span data-stu-id="2c009-454">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="2c009-455">這個程式會重複執行， `finally`直到所有中間`try`語句的區塊都已執行為止。</span><span class="sxs-lookup"><span data-stu-id="2c009-455">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="2c009-456">控制權會傳送至`continue`語句的目標。</span><span class="sxs-lookup"><span data-stu-id="2c009-456">Control is transferred to the target of the `continue` statement.</span></span>

<span data-ttu-id="2c009-457">因為語句會無條件地將控制權轉移到別處，所以永遠`continue`無法連線到語句的結束點。 `continue`</span><span class="sxs-lookup"><span data-stu-id="2c009-457">Because a `continue` statement unconditionally transfers control elsewhere, the end point of a `continue` statement is never reachable.</span></span>

### <a name="the-goto-statement"></a><span data-ttu-id="2c009-458">goto 陳述式</span><span class="sxs-lookup"><span data-stu-id="2c009-458">The goto statement</span></span>

<span data-ttu-id="2c009-459">`goto`語句會將控制項傳輸至以標籤標記的語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-459">The `goto` statement transfers control to a statement that is marked by a label.</span></span>

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

<span data-ttu-id="2c009-460">*Identifier*語句的目標`goto`是具有指定標籤的標記語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-460">The target of a `goto` *identifier* statement is the labeled statement with the given label.</span></span> <span data-ttu-id="2c009-461">如果目前的函式成員中沒有指定名稱的標籤，或如果`goto`語句不在標籤的範圍內，則會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2c009-461">If a label with the given name does not exist in the current function member, or if the `goto` statement is not within the scope of the label, a compile-time error occurs.</span></span> <span data-ttu-id="2c009-462">此規則允許使用`goto`語句，將控制項從嵌套的範圍（但不是）轉換成嵌套的範圍。</span><span class="sxs-lookup"><span data-stu-id="2c009-462">This rule permits the use of a `goto` statement to transfer control out of a nested scope, but not into a nested scope.</span></span> <span data-ttu-id="2c009-463">在範例中</span><span class="sxs-lookup"><span data-stu-id="2c009-463">In the example</span></span>
```csharp
using System;

class Test
{
    static void Main(string[] args) {
        string[,] table = {
            {"Red", "Blue", "Green"},
            {"Monday", "Wednesday", "Friday"}
        };

        foreach (string str in args) {
            int row, colm;
            for (row = 0; row <= 1; ++row)
                for (colm = 0; colm <= 2; ++colm)
                    if (str == table[row,colm])
                         goto done;

            Console.WriteLine("{0} not found", str);
            continue;
    done:
            Console.WriteLine("Found {0} at [{1}][{2}]", str, row, colm);
        }
    }
}
```
<span data-ttu-id="2c009-464">`goto`語句是用來將控制項從嵌套的範圍中轉移出來。</span><span class="sxs-lookup"><span data-stu-id="2c009-464">a `goto` statement is used to transfer control out of a nested scope.</span></span>

<span data-ttu-id="2c009-465">`goto case`語句的目標是`switch`立即封入語句（ `case` [switch 語句](statements.md#the-switch-statement)）中的語句清單，其中包含具有給定常數值的標籤。</span><span class="sxs-lookup"><span data-stu-id="2c009-465">The target of a `goto case` statement is the statement list in the immediately enclosing `switch` statement ([The switch statement](statements.md#the-switch-statement)), which contains a `case` label with the given constant value.</span></span> <span data-ttu-id="2c009-466">如果 @no__t 0 的語句不是以 @no__t 1 的語句括住，則為，如果*constant_expression*無法隱含轉換（[隱含轉換](conversions.md#implicit-conversions)）為最接近的封閉式 `switch` 語句的治理類型，或最接近的封入`switch` 語句不包含具有給定常數值的 `case` 標籤，發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2c009-466">If the `goto case` statement is not enclosed by a `switch` statement, if the *constant_expression* is not implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the governing type of the nearest enclosing `switch` statement, or if the nearest enclosing `switch` statement does not contain a `case` label with the given constant value, a compile-time error occurs.</span></span>

<span data-ttu-id="2c009-467">`goto default`語句的目標是`switch`立即封入語句（ `default` [switch 語句](statements.md#the-switch-statement)）中的語句清單，其中包含標籤。</span><span class="sxs-lookup"><span data-stu-id="2c009-467">The target of a `goto default` statement is the statement list in the immediately enclosing `switch` statement ([The switch statement](statements.md#the-switch-statement)), which contains a `default` label.</span></span> <span data-ttu-id="2c009-468">如果語句未以`switch`語句括住，或最接近的封閉式`switch`語句不包含`default`標籤，則會發生編譯時期錯誤。 `goto default`</span><span class="sxs-lookup"><span data-stu-id="2c009-468">If the `goto default` statement is not enclosed by a `switch` statement, or if the nearest enclosing `switch` statement does not contain a `default` label, a compile-time error occurs.</span></span>

<span data-ttu-id="2c009-469">語句無法結束區塊（[try 語句）。](statements.md#the-try-statement) `finally` `goto`</span><span class="sxs-lookup"><span data-stu-id="2c009-469">A `goto` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="2c009-470">當語句在區塊內發生時`goto` ，語句的目標必須在相同`finally`的區塊內，否則會發生編譯時期錯誤。 `finally` `goto`</span><span class="sxs-lookup"><span data-stu-id="2c009-470">When a `goto` statement occurs within a `finally` block, the target of the `goto` statement must be within the same `finally` block, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="2c009-471">`goto`語句的執行方式如下：</span><span class="sxs-lookup"><span data-stu-id="2c009-471">A `goto` statement is executed as follows:</span></span>

*  <span data-ttu-id="2c009-472">`try` `finally` `finally` `try`如果語句結束一個或多個具有相關聯區塊的區塊，則控制權一開始會傳送到最內層語句的區塊。 `goto`</span><span class="sxs-lookup"><span data-stu-id="2c009-472">If the `goto` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="2c009-473">當控制項到達`finally`區塊的結束點時，控制權會轉移`finally`到下一個封入`try`語句的區塊。</span><span class="sxs-lookup"><span data-stu-id="2c009-473">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="2c009-474">這個程式會重複執行， `finally`直到所有中間`try`語句的區塊都已執行為止。</span><span class="sxs-lookup"><span data-stu-id="2c009-474">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="2c009-475">控制權會傳送至`goto`語句的目標。</span><span class="sxs-lookup"><span data-stu-id="2c009-475">Control is transferred to the target of the `goto` statement.</span></span>

<span data-ttu-id="2c009-476">因為語句會無條件地將控制權轉移到別處，所以永遠`goto`無法連線到語句的結束點。 `goto`</span><span class="sxs-lookup"><span data-stu-id="2c009-476">Because a `goto` statement unconditionally transfers control elsewhere, the end point of a `goto` statement is never reachable.</span></span>

### <a name="the-return-statement"></a><span data-ttu-id="2c009-477">Return 語句</span><span class="sxs-lookup"><span data-stu-id="2c009-477">The return statement</span></span>

<span data-ttu-id="2c009-478">語句會將控制權傳回給出現`return`語句之函數的目前呼叫端。 `return`</span><span class="sxs-lookup"><span data-stu-id="2c009-478">The `return` statement returns control to the current caller of the function in which the `return` statement appears.</span></span>

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

<span data-ttu-id="2c009-479">`add` `void` `set`  `return`沒有運算式的語句只能用在不會計算值的函式成員中，也就是具有結果類型（[方法主體](classes.md#method-body)）的方法、屬性或索引子的存取子、事件`remove`的存取子、實例的函數、靜態的函式或析構函數。</span><span class="sxs-lookup"><span data-stu-id="2c009-479">A `return` statement with no expression can be used only in a function member that does not compute a value, that is, a method with the result type ([Method body](classes.md#method-body)) `void`, the `set` accessor of a property or indexer, the `add` and `remove` accessors of an event, an instance constructor, a static constructor, or a destructor.</span></span>

<span data-ttu-id="2c009-480">具有運算式的`get` 語句只能在計算值的函式成員中使用，也就是具有非void結果類型的方法、屬性或索引子的存取子，或使用者`return`定義的運算子。</span><span class="sxs-lookup"><span data-stu-id="2c009-480">A `return` statement with an expression can only be used in a function member that computes a value, that is, a method with a non-void result type, the `get` accessor of a property or indexer, or a user-defined operator.</span></span> <span data-ttu-id="2c009-481">隱含轉換（[隱含](conversions.md#implicit-conversions)轉換）必須存在於運算式的類型與包含函式成員的傳回類型之間。</span><span class="sxs-lookup"><span data-stu-id="2c009-481">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) must exist from the type of the expression to the return type of the containing function member.</span></span>

<span data-ttu-id="2c009-482">Return 語句也可以在匿名函式運算式的主體中使用（[匿名](expressions.md#anonymous-function-expressions)函式運算式），並參與判斷哪些函式有哪些轉換存在。</span><span class="sxs-lookup"><span data-stu-id="2c009-482">Return statements can also be used in the body of anonymous function expressions ([Anonymous function expressions](expressions.md#anonymous-function-expressions)), and participate in determining which conversions exist for those functions.</span></span>

<span data-ttu-id="2c009-483">`return`語句出現`finally`在區塊（[try 語句](statements.md#the-try-statement)）中時，會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2c009-483">It is a compile-time error for a `return` statement to appear in a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span>

<span data-ttu-id="2c009-484">`return`語句的執行方式如下：</span><span class="sxs-lookup"><span data-stu-id="2c009-484">A `return` statement is executed as follows:</span></span>

*  <span data-ttu-id="2c009-485">`return`如果語句指定運算式，則會評估運算式，並將產生的值轉換為包含函式的傳回類型（藉由隱含轉換）。</span><span class="sxs-lookup"><span data-stu-id="2c009-485">If the `return` statement specifies an expression, the expression is evaluated and the resulting value is converted to the return type of the containing function by an implicit conversion.</span></span> <span data-ttu-id="2c009-486">轉換的結果會成為函式所產生的結果值。</span><span class="sxs-lookup"><span data-stu-id="2c009-486">The result of the conversion becomes the result value produced by the function.</span></span>
*  <span data-ttu-id="2c009-487">`finally` `catch` `try` `finally`如果語句是以一或多個具有相關聯區塊的區塊括住，則控制項一開始會傳送到最`try`內層語句的區塊。 `return`</span><span class="sxs-lookup"><span data-stu-id="2c009-487">If the `return` statement is enclosed by one or more `try` or `catch` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="2c009-488">當控制項到達`finally`區塊的結束點時，控制權會轉移`finally`到下一個封入`try`語句的區塊。</span><span class="sxs-lookup"><span data-stu-id="2c009-488">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="2c009-489">此程式會重複執行， `finally`直到所有封閉式`try`語句的區塊都已執行為止。</span><span class="sxs-lookup"><span data-stu-id="2c009-489">This process is repeated until the `finally` blocks of all enclosing `try` statements have been executed.</span></span>
*  <span data-ttu-id="2c009-490">如果包含函數不是非同步函式，則會將控制權傳回給包含函式的呼叫端，以及結果值（如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="2c009-490">If the containing function is not an async function, control is returned to the caller of the containing function along with the result value, if any.</span></span>
*  <span data-ttu-id="2c009-491">如果包含函數是非同步函式，控制權會傳回給目前的呼叫端，而結果值（如果有的話）則會記錄在傳回工作中，如（[枚舉器介面](classes.md#enumerator-interfaces)）中所述。</span><span class="sxs-lookup"><span data-stu-id="2c009-491">If the containing function is an async function, control is returned to the current caller, and the result value, if any, is recorded in the return task as described in ([Enumerator interfaces](classes.md#enumerator-interfaces)).</span></span>

<span data-ttu-id="2c009-492">因為語句會無條件地將控制權轉移到別處，所以永遠`return`無法連線到語句的結束點。 `return`</span><span class="sxs-lookup"><span data-stu-id="2c009-492">Because a `return` statement unconditionally transfers control elsewhere, the end point of a `return` statement is never reachable.</span></span>

### <a name="the-throw-statement"></a><span data-ttu-id="2c009-493">Throw 語句</span><span class="sxs-lookup"><span data-stu-id="2c009-493">The throw statement</span></span>

<span data-ttu-id="2c009-494">`throw`語句會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="2c009-494">The `throw` statement throws an exception.</span></span>

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

<span data-ttu-id="2c009-495">具有運算式的語句會擲回評估運算式所產生的值。 `throw`</span><span class="sxs-lookup"><span data-stu-id="2c009-495">A `throw` statement with an expression throws the value produced by evaluating the expression.</span></span> <span data-ttu-id="2c009-496">運算式必須代表類別類型的值`System.Exception`，其衍生自`System.Exception`或具有`System.Exception` （或其子類別）做為其有效基類的類型參數類型。</span><span class="sxs-lookup"><span data-stu-id="2c009-496">The expression must denote a value of the class type `System.Exception`, of a class type that derives from `System.Exception` or of a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span> <span data-ttu-id="2c009-497">如果運算式的評估產生`null` `System.NullReferenceException` ，會改為擲回。</span><span class="sxs-lookup"><span data-stu-id="2c009-497">If evaluation of the expression produces `null`, a `System.NullReferenceException` is thrown instead.</span></span>

<span data-ttu-id="2c009-498">沒有運算式的`catch` `catch`語句只能用在區塊中，在此情況下，語句會重新擲回該區塊目前正在處理的例外狀況。 `throw`</span><span class="sxs-lookup"><span data-stu-id="2c009-498">A `throw` statement with no expression can be used only in a `catch` block, in which case that statement re-throws the exception that is currently being handled by that `catch` block.</span></span>

<span data-ttu-id="2c009-499">因為語句會無條件地將控制權轉移到別處，所以永遠`throw`無法連線到語句的結束點。 `throw`</span><span class="sxs-lookup"><span data-stu-id="2c009-499">Because a `throw` statement unconditionally transfers control elsewhere, the end point of a `throw` statement is never reachable.</span></span>

<span data-ttu-id="2c009-500">當擲回例外狀況時，控制權會傳送至封`catch`入`try`語句中可處理例外狀況的第一個子句。</span><span class="sxs-lookup"><span data-stu-id="2c009-500">When an exception is thrown, control is transferred to the first `catch` clause in an enclosing `try` statement that can handle the exception.</span></span> <span data-ttu-id="2c009-501">從例外狀況點到將控制項傳輸至適當的例外狀況處理常式所擲回的進程稱為「***例外狀況傳播***」。</span><span class="sxs-lookup"><span data-stu-id="2c009-501">The process that takes place from the point of the exception being thrown to the point of transferring control to a suitable exception handler is known as ***exception propagation***.</span></span> <span data-ttu-id="2c009-502">例外狀況的傳播包含重複評估下列步驟，直到找到符合`catch`例外狀況的子句為止。</span><span class="sxs-lookup"><span data-stu-id="2c009-502">Propagation of an exception consists of repeatedly evaluating the following steps until a `catch` clause that matches the exception is found.</span></span> <span data-ttu-id="2c009-503">在此描述中，***擲回點***是一開始擲回例外狀況的位置。</span><span class="sxs-lookup"><span data-stu-id="2c009-503">In this description, the ***throw point*** is initially the location at which the exception is thrown.</span></span>

*  <span data-ttu-id="2c009-504">在目前的函式成員中`try` ，會檢查括住擲回點的每個語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-504">In the current function member, each `try` statement that encloses the throw point is examined.</span></span> <span data-ttu-id="2c009-505">針對每個`S`語句，從最內層`try`的語句開始，並以`try`最外層的語句結束，會評估下列步驟：</span><span class="sxs-lookup"><span data-stu-id="2c009-505">For each statement `S`, starting with the innermost `try` statement and ending with the outermost `try` statement, the following steps are evaluated:</span></span>

   * <span data-ttu-id="2c009-506">`catch`如果的`try`區塊包含擲回點，且如果有一或多個子句，則`catch`會依照外觀的順序檢查子句，以根據中指定的規則來尋找適當的例外狀況處理常式`S`一節[try 語句](statements.md#the-try-statement)。</span><span class="sxs-lookup"><span data-stu-id="2c009-506">If the `try` block of `S` encloses the throw point and if S has one or more `catch` clauses, the `catch` clauses are examined in order of appearance to locate a suitable handler for the exception, according to the rules specified in Section [The try statement](statements.md#the-try-statement).</span></span> <span data-ttu-id="2c009-507">如果找到相符`catch`的子句，就會藉由將控制權轉移至該`catch`子句的區塊來完成例外狀況傳播。</span><span class="sxs-lookup"><span data-stu-id="2c009-507">If a matching `catch` clause is located, the exception propagation is completed by transferring control to the block of that `catch` clause.</span></span>

   * <span data-ttu-id="2c009-508">`try`否則，如果區塊`catch`或的`S`區塊包含擲回點，而且如果`S`有`finally`區塊，控制權就會傳送至`finally`區塊。</span><span class="sxs-lookup"><span data-stu-id="2c009-508">Otherwise, if the `try` block or a `catch` block of `S` encloses the throw point and if `S` has a `finally` block, control is transferred to the `finally` block.</span></span> <span data-ttu-id="2c009-509">`finally`如果區塊擲回另一個例外狀況，則會終止處理目前的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="2c009-509">If the `finally` block throws another exception, processing of the current exception is terminated.</span></span> <span data-ttu-id="2c009-510">否則，當控制項到達`finally`區塊的結束點時，就會繼續處理目前的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="2c009-510">Otherwise, when control reaches the end point of the `finally` block, processing of the current exception is continued.</span></span>

*  <span data-ttu-id="2c009-511">如果例外狀況處理常式不在目前的函式呼叫中，則函式調用會終止，併發生下列其中一種情況：</span><span class="sxs-lookup"><span data-stu-id="2c009-511">If an exception handler was not located in the current function invocation, the function invocation is terminated, and one of the following occurs:</span></span>

   * <span data-ttu-id="2c009-512">如果目前的函式是非非同步，則會針對函式的呼叫者重複上述步驟，其擲回點對應于叫用函式成員的語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-512">If the current function is non-async, the steps above are repeated for the caller of the function with a throw point corresponding to the statement from which the function member was invoked.</span></span>

   * <span data-ttu-id="2c009-513">如果目前的函式為非同步和工作傳回，則會將例外狀況記錄在傳回工作中，此工作會放入「錯誤」或「已取消」狀態，如[列舉值介面](classes.md#enumerator-interfaces)中所述。</span><span class="sxs-lookup"><span data-stu-id="2c009-513">If the current function is async and task-returning, the exception is recorded in the return task, which is put into a faulted or cancelled state as described in [Enumerator interfaces](classes.md#enumerator-interfaces).</span></span>

   * <span data-ttu-id="2c009-514">如果目前的函式為 async 且傳回 void，則會通知目前線程的同步處理內容，如可列舉[介面](classes.md#enumerable-interfaces)中所述。</span><span class="sxs-lookup"><span data-stu-id="2c009-514">If the current function is async and void-returning, the synchronization context of the current thread is notified as described in [Enumerable interfaces](classes.md#enumerable-interfaces).</span></span>

*  <span data-ttu-id="2c009-515">如果例外狀況處理終止目前線程中的所有函式成員調用，表示執行緒沒有例外狀況的處理常式，則執行緒本身就會終止。</span><span class="sxs-lookup"><span data-stu-id="2c009-515">If the exception processing terminates all function member invocations in the current thread, indicating that the thread has no handler for the exception, then the thread is itself terminated.</span></span> <span data-ttu-id="2c009-516">這類終止的影響是「實作為定義」。</span><span class="sxs-lookup"><span data-stu-id="2c009-516">The impact of such termination is implementation-defined.</span></span>

## <a name="the-try-statement"></a><span data-ttu-id="2c009-517">Try 語句</span><span class="sxs-lookup"><span data-stu-id="2c009-517">The try statement</span></span>

<span data-ttu-id="2c009-518">`try`語句提供一個機制來攔截執行區塊期間所發生的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="2c009-518">The `try` statement provides a mechanism for catching exceptions that occur during execution of a block.</span></span> <span data-ttu-id="2c009-519">此外， `try`語句還能指定當控制項`try`離開語句時，一律會執行的程式碼區塊。</span><span class="sxs-lookup"><span data-stu-id="2c009-519">Furthermore, the `try` statement provides the ability to specify a block of code that is always executed when control leaves the `try` statement.</span></span>

```antlr
try_statement
    : 'try' block catch_clause+
    | 'try' block finally_clause
    | 'try' block catch_clause+ finally_clause
    ;

catch_clause
    : 'catch' exception_specifier? exception_filter?  block
    ;

exception_specifier
    : '(' type identifier? ')'
    ;

exception_filter
    : 'when' '(' expression ')'
    ;

finally_clause
    : 'finally' block
    ;
```

<span data-ttu-id="2c009-520">語句有三種可能的`try`形式：</span><span class="sxs-lookup"><span data-stu-id="2c009-520">There are three possible forms of `try` statements:</span></span>

*  <span data-ttu-id="2c009-521">後面`try`接著一或多個`catch`區塊的區塊。</span><span class="sxs-lookup"><span data-stu-id="2c009-521">A `try` block followed by one or more `catch` blocks.</span></span>
*  <span data-ttu-id="2c009-522">區塊後面接著`finally`區塊。 `try`</span><span class="sxs-lookup"><span data-stu-id="2c009-522">A `try` block followed by a `finally` block.</span></span>
*  <span data-ttu-id="2c009-523">後面接著一個或多個`catch`區塊`finally`的區塊，後面接著一個區塊。`try`</span><span class="sxs-lookup"><span data-stu-id="2c009-523">A `try` block followed by one or more `catch` blocks followed by a `finally` block.</span></span>

<span data-ttu-id="2c009-524">當 `catch` 子句指定*exception_specifier*時，類型必須 `System.Exception`、衍生自 `System.Exception` 的類型，或具有 `System.Exception` （或其子類別）做為其有效基類的類型參數類型。</span><span class="sxs-lookup"><span data-stu-id="2c009-524">When a `catch` clause specifies an *exception_specifier*, the type must be `System.Exception`, a type that derives from `System.Exception` or a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span>

<span data-ttu-id="2c009-525">當 @no__t 0 子句同時指定具有*識別碼*的*exception_specifier*時，會宣告給定名稱和類型的***例外狀況變數***。</span><span class="sxs-lookup"><span data-stu-id="2c009-525">When a `catch` clause specifies both an *exception_specifier* with an *identifier*, an ***exception variable*** of the given name and type is declared.</span></span> <span data-ttu-id="2c009-526">例外狀況變數會對應至範圍中的區域變數，該變數會透過`catch`子句擴充。</span><span class="sxs-lookup"><span data-stu-id="2c009-526">The exception variable corresponds to a local variable with a scope that extends over the `catch` clause.</span></span> <span data-ttu-id="2c009-527">在執行*exception_filter*和*區塊*時，例外狀況變數代表目前正在處理的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="2c009-527">During execution of the *exception_filter* and *block*, the exception variable represents the exception currently being handled.</span></span> <span data-ttu-id="2c009-528">基於明確指派檢查的目的，會將例外狀況變數視為在其整個範圍中明確指派。</span><span class="sxs-lookup"><span data-stu-id="2c009-528">For purposes of definite assignment checking, the exception variable is considered definitely assigned in its entire scope.</span></span>

<span data-ttu-id="2c009-529">除非子句包含例外狀況變數名稱，否則無法存取篩選和`catch`區塊中的例外狀況物件。 `catch`</span><span class="sxs-lookup"><span data-stu-id="2c009-529">Unless a `catch` clause includes an exception variable name, it is impossible to access the exception object in the filter and `catch` block.</span></span>

<span data-ttu-id="2c009-530">未指定*exception_specifier*的 @no__t 0 子句稱為一般的 `catch` 子句。</span><span class="sxs-lookup"><span data-stu-id="2c009-530">A `catch` clause that does not specify an *exception_specifier* is called a general `catch` clause.</span></span>

<span data-ttu-id="2c009-531">某些程式設計語言可能會支援無法以衍生自`System.Exception`的物件形式表示的例外狀況，雖然程式C#代碼不會產生這類例外狀況。</span><span class="sxs-lookup"><span data-stu-id="2c009-531">Some programming languages may support exceptions that are not representable as an object derived from `System.Exception`, although such exceptions could never be generated by C# code.</span></span> <span data-ttu-id="2c009-532">General `catch`子句可用來攔截這類例外狀況。</span><span class="sxs-lookup"><span data-stu-id="2c009-532">A general `catch` clause may be used to catch such exceptions.</span></span> <span data-ttu-id="2c009-533">因此，一般`catch`子句在語義上與指定類型`System.Exception`的不同，前者也會攔截來自其他語言的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="2c009-533">Thus, a general `catch` clause is semantically different from one that specifies the type `System.Exception`, in that the former may also catch exceptions from other languages.</span></span>

<span data-ttu-id="2c009-534">為了找出例外狀況的處理常式， `catch`會以詞法順序檢查子句。</span><span class="sxs-lookup"><span data-stu-id="2c009-534">In order to locate a handler for an exception, `catch` clauses are examined in lexical order.</span></span> <span data-ttu-id="2c009-535">如果子句指定類型，但沒有例外狀況篩選準則，則在相同`try`語句中，後面`catch`的子句會發生編譯時期錯誤，以指定與該類型相同或衍生自的類型。 `catch`</span><span class="sxs-lookup"><span data-stu-id="2c009-535">If a `catch` clause specifies a type but no exception filter, it is a compile-time error for a later `catch` clause in the same `try` statement to specify a type that is the same as, or is derived from, that type.</span></span> <span data-ttu-id="2c009-536">如果子句未指定型別，而且沒有篩選準則，它就必須`catch`是該`try`語句的最後一個子句。 `catch`</span><span class="sxs-lookup"><span data-stu-id="2c009-536">If a `catch` clause specifies no type and no filter, it must be the last `catch` clause for that `try` statement.</span></span>

<span data-ttu-id="2c009-537">在區塊內`throw` ，沒有運算式的語句（[throw 語句](statements.md#the-throw-statement)）可以用來重新擲回`catch`區塊攔截到的例外狀況。 `catch`</span><span class="sxs-lookup"><span data-stu-id="2c009-537">Within a `catch` block, a `throw` statement ([The throw statement](statements.md#the-throw-statement)) with no expression can be used to re-throw the exception that was caught by the `catch` block.</span></span> <span data-ttu-id="2c009-538">對例外狀況變數的指派不會改變被重新擲回的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="2c009-538">Assignments to an exception variable do not alter the exception that is re-thrown.</span></span>

<span data-ttu-id="2c009-539">在範例中</span><span class="sxs-lookup"><span data-stu-id="2c009-539">In the example</span></span>
```csharp
using System;

class Test
{
    static void F() {
        try {
            G();
        }
        catch (Exception e) {
            Console.WriteLine("Exception in F: " + e.Message);
            e = new Exception("F");
            throw;                // re-throw
        }
    }

    static void G() {
        throw new Exception("G");
    }

    static void Main() {
        try {
            F();
        }
        catch (Exception e) {
            Console.WriteLine("Exception in Main: " + e.Message);
        }
    }
}
```
<span data-ttu-id="2c009-540">方法`F`會攔截例外狀況、將一些診斷資訊寫入主控台、改變例外狀況變數，然後重新擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="2c009-540">the method `F` catches an exception, writes some diagnostic information to the console, alters the exception variable, and re-throws the exception.</span></span> <span data-ttu-id="2c009-541">重新擲回的例外狀況是原始的例外狀況，因此產生的輸出為：</span><span class="sxs-lookup"><span data-stu-id="2c009-541">The exception that is re-thrown is the original exception, so the output produced is:</span></span>
```console
Exception in F: G
Exception in Main: G
```

<span data-ttu-id="2c009-542">如果第一個 catch 區塊`e`已擲回，而不是重新擲回目前的例外狀況，則產生的輸出會如下所示：</span><span class="sxs-lookup"><span data-stu-id="2c009-542">If the first catch block had thrown `e` instead of rethrowing the current exception, the output produced would be as follows:</span></span>
```console
Exception in F: G
Exception in Main: F
```

<span data-ttu-id="2c009-543">當`break`、或`finally` `continue`語句將控制權轉移到區塊時，就會發生編譯時期錯誤。`goto`</span><span class="sxs-lookup"><span data-stu-id="2c009-543">It is a compile-time error for a `break`, `continue`, or `goto` statement to transfer control out of a `finally` block.</span></span> <span data-ttu-id="2c009-544">`break`當、 `continue` `finally`或語句出現在區塊中時，語句的目標必須在相同的區塊內，否則就會發生編譯時期錯誤。`finally` `goto`</span><span class="sxs-lookup"><span data-stu-id="2c009-544">When a `break`, `continue`, or `goto` statement occurs in a `finally` block, the target of the statement must be within the same `finally` block, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="2c009-545">在`return` 區塊`finally`中發生語句時，會產生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2c009-545">It is a compile-time error for a `return` statement to occur in a `finally` block.</span></span>

<span data-ttu-id="2c009-546">`try`語句的執行方式如下：</span><span class="sxs-lookup"><span data-stu-id="2c009-546">A `try` statement is executed as follows:</span></span>

*  <span data-ttu-id="2c009-547">控制權會傳送至`try`區塊。</span><span class="sxs-lookup"><span data-stu-id="2c009-547">Control is transferred to the `try` block.</span></span>
*  <span data-ttu-id="2c009-548">當控制項到達`try`區塊的結束點時：</span><span class="sxs-lookup"><span data-stu-id="2c009-548">When and if control reaches the end point of the `try` block:</span></span>
   *  <span data-ttu-id="2c009-549">如果語句具有區塊，則會執行`finally`區塊。 `finally` `try`</span><span class="sxs-lookup"><span data-stu-id="2c009-549">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
   *  <span data-ttu-id="2c009-550">控制權會傳送至`try`語句的結束點。</span><span class="sxs-lookup"><span data-stu-id="2c009-550">Control is transferred to the end point of the `try` statement.</span></span>

*  <span data-ttu-id="2c009-551">如果在`try`區塊執行期間將例外`try`狀況傳播至語句：</span><span class="sxs-lookup"><span data-stu-id="2c009-551">If an exception is propagated to the `try` statement during execution of the `try` block:</span></span>
   *  <span data-ttu-id="2c009-552">`catch`子句（如果有的話）會依照外觀的順序來檢查，以找出適用于例外狀況的處理常式。</span><span class="sxs-lookup"><span data-stu-id="2c009-552">The `catch` clauses, if any, are examined in order of appearance to locate a suitable handler for the exception.</span></span> <span data-ttu-id="2c009-553">`catch`如果子句未指定類型，或指定例外狀況類型或例外狀況類型的基底類型：</span><span class="sxs-lookup"><span data-stu-id="2c009-553">If a `catch` clause does not specify a type, or specifies the exception type or a base type of the exception type:</span></span>
      *  <span data-ttu-id="2c009-554">`catch`如果子句宣告例外狀況變數，則會將例外狀況物件指派給例外狀況變數。</span><span class="sxs-lookup"><span data-stu-id="2c009-554">If the `catch` clause declares an exception variable, the exception object is assigned to the exception variable.</span></span>
      *  <span data-ttu-id="2c009-555">`catch`如果子句宣告例外狀況篩選準則，就會評估篩選準則。</span><span class="sxs-lookup"><span data-stu-id="2c009-555">If the `catch` clause declares an exception filter, the filter is evaluated.</span></span> <span data-ttu-id="2c009-556">如果評估為`false`，則 catch 子句不相符，而且搜尋會繼續進行適當處理常式的任何`catch`後續子句。</span><span class="sxs-lookup"><span data-stu-id="2c009-556">If it evaluates to `false`, the catch clause is not a match, and the search continues through any subsequent `catch` clauses for a suitable handler.</span></span>
      *  <span data-ttu-id="2c009-557">否則， `catch`子句會被視為相符，而且控制權會轉移至相符`catch`的區塊。</span><span class="sxs-lookup"><span data-stu-id="2c009-557">Otherwise, the `catch` clause is considered a match, and control is transferred to the matching `catch` block.</span></span>
      *  <span data-ttu-id="2c009-558">當控制項到達`catch`區塊的結束點時：</span><span class="sxs-lookup"><span data-stu-id="2c009-558">When and if control reaches the end point of the `catch` block:</span></span>
         * <span data-ttu-id="2c009-559">如果語句具有區塊，則會執行`finally`區塊。 `finally` `try`</span><span class="sxs-lookup"><span data-stu-id="2c009-559">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
         * <span data-ttu-id="2c009-560">控制權會傳送至`try`語句的結束點。</span><span class="sxs-lookup"><span data-stu-id="2c009-560">Control is transferred to the end point of the `try` statement.</span></span>
      *  <span data-ttu-id="2c009-561">如果在`catch`區塊執行期間將例外`try`狀況傳播至語句：</span><span class="sxs-lookup"><span data-stu-id="2c009-561">If an exception is propagated to the `try` statement during execution of the `catch` block:</span></span>
         *  <span data-ttu-id="2c009-562">如果語句具有區塊，則會執行`finally`區塊。 `finally` `try`</span><span class="sxs-lookup"><span data-stu-id="2c009-562">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
         *  <span data-ttu-id="2c009-563">例外狀況會傳播至下一個封閉式`try`語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-563">The exception is propagated to the next enclosing `try` statement.</span></span>
   *  <span data-ttu-id="2c009-564">如果語句沒有子句，或如果沒有`catch`子句符合此例外狀況： `catch` `try`</span><span class="sxs-lookup"><span data-stu-id="2c009-564">If the `try` statement has no `catch` clauses or if no `catch` clause matches the exception:</span></span>
      *  <span data-ttu-id="2c009-565">如果語句具有區塊，則會執行`finally`區塊。 `finally` `try`</span><span class="sxs-lookup"><span data-stu-id="2c009-565">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
      *  <span data-ttu-id="2c009-566">例外狀況會傳播至下一個封閉式`try`語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-566">The exception is propagated to the next enclosing `try` statement.</span></span>

<span data-ttu-id="2c009-567">當控制項`finally` `try`離開語句時，一律會執行區塊的語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-567">The statements of a `finally` block are always executed when control leaves a `try` statement.</span></span> <span data-ttu-id="2c009-568">無論是因為執行`break`、 `continue`、 `goto`或`return`語句而造成控制轉移，或是因為將例外狀況傳播到`try`以外的原因而造成的，都是如此。句.</span><span class="sxs-lookup"><span data-stu-id="2c009-568">This is true whether the control transfer occurs as a result of normal execution, as a result of executing a `break`, `continue`, `goto`, or `return` statement, or as a result of propagating an exception out of the `try` statement.</span></span>

<span data-ttu-id="2c009-569">如果在`finally`區塊執行期間擲回例外狀況，而且不是在相同的 finally 區塊內攔截到，則會將例外狀況傳播至`try`下一個封入語句。</span><span class="sxs-lookup"><span data-stu-id="2c009-569">If an exception is thrown during execution of a `finally` block, and is not caught within the same finally block, the exception is propagated to the next enclosing `try` statement.</span></span> <span data-ttu-id="2c009-570">如果正在傳播另一個例外狀況，則會遺失該例外狀況。</span><span class="sxs-lookup"><span data-stu-id="2c009-570">If another exception was in the process of being propagated, that exception is lost.</span></span> <span data-ttu-id="2c009-571">傳播例外狀況的程式會在`throw`語句的描述中進一步討論（[throw 語句](statements.md#the-throw-statement)）。</span><span class="sxs-lookup"><span data-stu-id="2c009-571">The process of propagating an exception is discussed further in the description of the `throw` statement ([The throw statement](statements.md#the-throw-statement)).</span></span>

<span data-ttu-id="2c009-572">如果`try`可以連線到`try` `try`語句，則可以連接語句的區塊。</span><span class="sxs-lookup"><span data-stu-id="2c009-572">The `try` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="2c009-573">如果`catch`可以連線到`try` `try`語句，就可以連線到語句的區塊。</span><span class="sxs-lookup"><span data-stu-id="2c009-573">A `catch` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="2c009-574">如果`finally`可以連線到`try` `try`語句，則可以連接語句的區塊。</span><span class="sxs-lookup"><span data-stu-id="2c009-574">The `finally` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="2c009-575">如果下列兩個條件`try`都成立，就可以觸達語句的結束點：</span><span class="sxs-lookup"><span data-stu-id="2c009-575">The end point of a `try` statement is reachable if both of the following are true:</span></span>

*  <span data-ttu-id="2c009-576">可以連線到`try`區塊的結束點，或至少有一個`catch`區塊的端點可供連線。</span><span class="sxs-lookup"><span data-stu-id="2c009-576">The end point of the `try` block is reachable or the end point of at least one `catch` block is reachable.</span></span>
*  <span data-ttu-id="2c009-577">如果有`finally`區塊存在，就可以連線到區塊的結束點。 `finally`</span><span class="sxs-lookup"><span data-stu-id="2c009-577">If a `finally` block is present, the end point of the `finally` block is reachable.</span></span>

## <a name="the-checked-and-unchecked-statements"></a><span data-ttu-id="2c009-578">Checked 和 unchecked 語句</span><span class="sxs-lookup"><span data-stu-id="2c009-578">The checked and unchecked statements</span></span>

<span data-ttu-id="2c009-579">和語句是用來控制整數型別算數運算和轉換的***溢位檢查內容。*** `checked` `unchecked`</span><span class="sxs-lookup"><span data-stu-id="2c009-579">The `checked` and `unchecked` statements are used to control the ***overflow checking context*** for integral-type arithmetic operations and conversions.</span></span>

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

<span data-ttu-id="2c009-580">語句會在檢查的`unchecked`內容中評估*區塊*中的所有運算式，而語句會導致區塊中的所有運算式在未檢查的內容中進行評估。 `checked`</span><span class="sxs-lookup"><span data-stu-id="2c009-580">The `checked` statement causes all expressions in the *block* to be evaluated in a checked context, and the `unchecked` statement causes all expressions in the *block* to be evaluated in an unchecked context.</span></span>

<span data-ttu-id="2c009-581">`checked`和語句`unchecked`相當于和`unchecked`運算子（[checked 和 unchecked 運算子](expressions.md#the-checked-and-unchecked-operators)），不同之處在于它們會在區塊上運作，而不是運算式。 `checked`</span><span class="sxs-lookup"><span data-stu-id="2c009-581">The `checked` and `unchecked` statements are precisely equivalent to the `checked` and `unchecked` operators ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)), except that they operate on blocks instead of expressions.</span></span>

## <a name="the-lock-statement"></a><span data-ttu-id="2c009-582">Lock 語句</span><span class="sxs-lookup"><span data-stu-id="2c009-582">The lock statement</span></span>

<span data-ttu-id="2c009-583">`lock`語句會取得指定物件的互斥鎖定、執行語句，然後釋放鎖定。</span><span class="sxs-lookup"><span data-stu-id="2c009-583">The `lock` statement obtains the mutual-exclusion lock for a given object, executes a statement, and then releases the lock.</span></span>

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

<span data-ttu-id="2c009-584">@No__t-0 語句的運算式必須代表已知為*reference_type*之類型的值。</span><span class="sxs-lookup"><span data-stu-id="2c009-584">The expression of a `lock` statement must denote a value of a type known to be a *reference_type*.</span></span> <span data-ttu-id="2c009-585">在 `lock` 語句的運算式中，不會執行隱含的裝箱轉換（[裝箱轉換](conversions.md#boxing-conversions)），因此該運算式會發生編譯時期錯誤，表示*value_type*的值。</span><span class="sxs-lookup"><span data-stu-id="2c009-585">No implicit boxing conversion ([Boxing conversions](conversions.md#boxing-conversions)) is ever performed for the expression of a `lock` statement, and thus it is a compile-time error for the expression to denote a value of a *value_type*.</span></span>

<span data-ttu-id="2c009-586">表單的語句`lock`</span><span class="sxs-lookup"><span data-stu-id="2c009-586">A `lock` statement of the form</span></span>
```csharp
lock (x) ...
```
<span data-ttu-id="2c009-587">其中 `x` 是*reference_type*的運算式，它會精確地等同于</span><span class="sxs-lookup"><span data-stu-id="2c009-587">where `x` is an expression of a *reference_type*, is precisely equivalent to</span></span>
```csharp
bool __lockWasTaken = false;
try {
    System.Threading.Monitor.Enter(x, ref __lockWasTaken);
    ...
}
finally {
    if (__lockWasTaken) System.Threading.Monitor.Exit(x);
}
```
<span data-ttu-id="2c009-588">但只會評估 `x` 一次。</span><span class="sxs-lookup"><span data-stu-id="2c009-588">except that `x` is only evaluated once.</span></span>

<span data-ttu-id="2c009-589">持有互斥鎖定時，在相同執行執行緒中執行的程式碼也可以取得和釋放鎖定。</span><span class="sxs-lookup"><span data-stu-id="2c009-589">While a mutual-exclusion lock is held, code executing in the same execution thread can also obtain and release the lock.</span></span> <span data-ttu-id="2c009-590">不過，在其他執行緒中執行的程式碼會被封鎖而無法取得鎖定，直到釋放鎖定為止。</span><span class="sxs-lookup"><span data-stu-id="2c009-590">However, code executing in other threads is blocked from obtaining the lock until the lock is released.</span></span>

<span data-ttu-id="2c009-591">不`System.Type`建議鎖定物件來同步處理靜態資料的存取。</span><span class="sxs-lookup"><span data-stu-id="2c009-591">Locking `System.Type` objects in order to synchronize access to static data is not recommended.</span></span> <span data-ttu-id="2c009-592">其他程式碼可能會鎖定相同的類型，這可能會導致鎖死。</span><span class="sxs-lookup"><span data-stu-id="2c009-592">Other code might lock on the same type, which can result in deadlock.</span></span> <span data-ttu-id="2c009-593">較好的方法是藉由鎖定私用靜態物件來同步處理靜態資料的存取。</span><span class="sxs-lookup"><span data-stu-id="2c009-593">A better approach is to synchronize access to static data by locking a private static object.</span></span> <span data-ttu-id="2c009-594">例如:</span><span class="sxs-lookup"><span data-stu-id="2c009-594">For example:</span></span>
```csharp
class Cache
{
    private static readonly object synchronizationObject = new object();

    public static void Add(object x) {
        lock (Cache.synchronizationObject) {
            ...
        }
    }

    public static void Remove(object x) {
        lock (Cache.synchronizationObject) {
            ...
        }
    }
}
```

## <a name="the-using-statement"></a><span data-ttu-id="2c009-595">using 陳述式</span><span class="sxs-lookup"><span data-stu-id="2c009-595">The using statement</span></span>

<span data-ttu-id="2c009-596">`using`語句會取得一或多個資源、執行語句，然後處置資源。</span><span class="sxs-lookup"><span data-stu-id="2c009-596">The `using` statement obtains one or more resources, executes a statement, and then disposes of the resource.</span></span>

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

<span data-ttu-id="2c009-597">***資源***是執行的類別或結構`System.IDisposable`，其中包含名為`Dispose`的單一無參數方法。</span><span class="sxs-lookup"><span data-stu-id="2c009-597">A ***resource*** is a class or struct that implements `System.IDisposable`, which includes a single parameterless method named `Dispose`.</span></span> <span data-ttu-id="2c009-598">使用資源的程式碼可以呼叫`Dispose` ，以指出不再需要資源。</span><span class="sxs-lookup"><span data-stu-id="2c009-598">Code that is using a resource can call `Dispose` to indicate that the resource is no longer needed.</span></span> <span data-ttu-id="2c009-599">如果`Dispose`未呼叫，則自動處置最後會成為垃圾收集的結果。</span><span class="sxs-lookup"><span data-stu-id="2c009-599">If `Dispose` is not called, then automatic disposal eventually occurs as a consequence of garbage collection.</span></span>

<span data-ttu-id="2c009-600">如果*resource_acquisition*的格式為*local_variable_declaration* ，則*local_variable_declaration*的類型必須是 `dynamic` 或可以隱含地轉換成 `System.IDisposable` 的類型。</span><span class="sxs-lookup"><span data-stu-id="2c009-600">If the form of *resource_acquisition* is *local_variable_declaration* then the type of the *local_variable_declaration* must be either `dynamic` or a type that can be implicitly converted to `System.IDisposable`.</span></span> <span data-ttu-id="2c009-601">如果*resource_acquisition*的形式為*expression* ，則此運算式必須可以隱含地轉換為 `System.IDisposable`。</span><span class="sxs-lookup"><span data-stu-id="2c009-601">If the form of *resource_acquisition* is *expression* then this expression must be implicitly convertible to `System.IDisposable`.</span></span>

<span data-ttu-id="2c009-602">在*resource_acquisition*中宣告的區域變數是唯讀的，而且必須包含初始化運算式。</span><span class="sxs-lookup"><span data-stu-id="2c009-602">Local variables declared in a *resource_acquisition* are read-only, and must include an initializer.</span></span> <span data-ttu-id="2c009-603">如果內嵌語句嘗試修改這些本機變數（透過指派`++`或和`--`運算子）、接受其位址`ref` ，或將它們當做或`out`參數傳遞，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2c009-603">A compile-time error occurs if the embedded statement attempts to modify these local variables (via assignment or the `++` and `--` operators) , take the address of them, or pass them as `ref` or `out` parameters.</span></span>

<span data-ttu-id="2c009-604">`using`語句會轉譯成三個部分： [取得]、[使用方式] 和 [處置]。</span><span class="sxs-lookup"><span data-stu-id="2c009-604">A `using` statement is translated into three parts: acquisition, usage, and disposal.</span></span> <span data-ttu-id="2c009-605">資源的使用會隱含地包含在`try` `finally`包含子句的語句中。</span><span class="sxs-lookup"><span data-stu-id="2c009-605">Usage of the resource is implicitly enclosed in a `try` statement that includes a `finally` clause.</span></span> <span data-ttu-id="2c009-606">此`finally`子句會處置資源。</span><span class="sxs-lookup"><span data-stu-id="2c009-606">This `finally` clause disposes of the resource.</span></span> <span data-ttu-id="2c009-607">如果取得`Dispose`資源，則不會對進行呼叫，也不會擲回任何例外狀況。 `null`</span><span class="sxs-lookup"><span data-stu-id="2c009-607">If a `null` resource is acquired, then no call to `Dispose` is made, and no exception is thrown.</span></span> <span data-ttu-id="2c009-608">如果資源的類型`dynamic`為，則會在取得期間透過隱含動態轉換（[隱含動態](conversions.md#implicit-dynamic-conversions)轉換）動態`IDisposable`轉換成，以確保在使用方式之前轉換成功供.</span><span class="sxs-lookup"><span data-stu-id="2c009-608">If the resource is of type `dynamic` it is dynamically converted through an implicit dynamic conversion ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions)) to `IDisposable` during acquisition in order to ensure that the conversion is successful before the usage and disposal.</span></span>

<span data-ttu-id="2c009-609">表單的語句`using`</span><span class="sxs-lookup"><span data-stu-id="2c009-609">A `using` statement of the form</span></span>
```csharp
using (ResourceType resource = expression) statement
```
<span data-ttu-id="2c009-610">對應至三個可能擴充的其中一個。</span><span class="sxs-lookup"><span data-stu-id="2c009-610">corresponds to one of three possible expansions.</span></span> <span data-ttu-id="2c009-611">當`ResourceType`是不可為 null 的實數值型別時，展開為</span><span class="sxs-lookup"><span data-stu-id="2c009-611">When `ResourceType` is a non-nullable value type, the expansion is</span></span>
```csharp
{
    ResourceType resource = expression;
    try {
        statement;
    }
    finally {
        ((IDisposable)resource).Dispose();
    }
}
```

<span data-ttu-id="2c009-612">否則，當`ResourceType`是可為 null 的實值型別或以外`dynamic`的引用型別時，展開就是</span><span class="sxs-lookup"><span data-stu-id="2c009-612">Otherwise, when `ResourceType` is a nullable value type or a reference type other than `dynamic`, the expansion is</span></span>
```csharp
{
    ResourceType resource = expression;
    try {
        statement;
    }
    finally {
        if (resource != null) ((IDisposable)resource).Dispose();
    }
}
```

<span data-ttu-id="2c009-613">否則，當`ResourceType`為`dynamic`時，展開為</span><span class="sxs-lookup"><span data-stu-id="2c009-613">Otherwise, when `ResourceType` is `dynamic`, the expansion is</span></span>
```csharp
{
    ResourceType resource = expression;
    IDisposable d = (IDisposable)resource;
    try {
        statement;
    }
    finally {
        if (d != null) d.Dispose();
    }
}
```

<span data-ttu-id="2c009-614">在任一擴充中， `resource`此變數在內嵌語句中都是唯讀的， `d`而且內嵌語句中的變數無法存取，且不會對其隱藏。</span><span class="sxs-lookup"><span data-stu-id="2c009-614">In either expansion, the `resource` variable is read-only in the embedded statement, and the `d` variable is inaccessible in, and invisible to, the embedded statement.</span></span>

<span data-ttu-id="2c009-615">執行時，您可以使用不同的方式來執行指定的 using 語句，例如基於效能的考慮，前提是該行為與上述展開一致。</span><span class="sxs-lookup"><span data-stu-id="2c009-615">An implementation is permitted to implement a given using-statement differently, e.g. for performance reasons, as long as the behavior is consistent with the above expansion.</span></span>

<span data-ttu-id="2c009-616">表單的語句`using`</span><span class="sxs-lookup"><span data-stu-id="2c009-616">A `using` statement of the form</span></span>
```csharp
using (expression) statement
```
<span data-ttu-id="2c009-617">有三個可能的擴充。</span><span class="sxs-lookup"><span data-stu-id="2c009-617">has the same three possible expansions.</span></span> <span data-ttu-id="2c009-618">在此情況`ResourceType`下`expression`，會隱含地編譯時間型別（如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="2c009-618">In this case `ResourceType` is implicitly the compile-time type of the `expression`, if it has one.</span></span> <span data-ttu-id="2c009-619">否則，介面`IDisposable`本身會當做使用。 `ResourceType`</span><span class="sxs-lookup"><span data-stu-id="2c009-619">Otherwise the interface `IDisposable` itself is used as the `ResourceType`.</span></span> <span data-ttu-id="2c009-620">在內嵌的語句中，變數無法在中存取，而且不會隱藏。`resource`</span><span class="sxs-lookup"><span data-stu-id="2c009-620">The `resource` variable is inaccessible in, and invisible to, the embedded statement.</span></span>

<span data-ttu-id="2c009-621">當*resource_acquisition*採用*local_variable_declaration*的形式時，可以取得指定類型的多個資源。</span><span class="sxs-lookup"><span data-stu-id="2c009-621">When a *resource_acquisition* takes the form of a *local_variable_declaration*, it is possible to acquire multiple resources of a given type.</span></span> <span data-ttu-id="2c009-622">表單的語句`using`</span><span class="sxs-lookup"><span data-stu-id="2c009-622">A `using` statement of the form</span></span>
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
<span data-ttu-id="2c009-623">會精確地等同于一系列的`using`嵌套語句：</span><span class="sxs-lookup"><span data-stu-id="2c009-623">is precisely equivalent to a sequence of nested `using` statements:</span></span>
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

<span data-ttu-id="2c009-624">下列範例會建立名為`log.txt`的檔案，並將兩行文字寫入檔案。</span><span class="sxs-lookup"><span data-stu-id="2c009-624">The example below creates a file named `log.txt` and writes two lines of text to the file.</span></span> <span data-ttu-id="2c009-625">然後，此範例會開啟相同的檔案來讀取，並將包含的文字行複製到主控台。</span><span class="sxs-lookup"><span data-stu-id="2c009-625">The example then opens that same file for reading and copies the contained lines of text to the console.</span></span>
```csharp
using System;
using System.IO;

class Test
{
    static void Main() {
        using (TextWriter w = File.CreateText("log.txt")) {
            w.WriteLine("This is line one");
            w.WriteLine("This is line two");
        }

        using (TextReader r = File.OpenText("log.txt")) {
            string s;
            while ((s = r.ReadLine()) != null) {
                Console.WriteLine(s);
            }

        }
    }
}
```

<span data-ttu-id="2c009-626">`IDisposable` `using`由於和類別`TextReader`會實作為介面，因此此範例可以使用語句來確保基礎檔案在寫入或讀取作業之後正確地關閉。 `TextWriter`</span><span class="sxs-lookup"><span data-stu-id="2c009-626">Since the `TextWriter` and `TextReader` classes implement the `IDisposable` interface, the example can use `using` statements to ensure that the underlying file is properly closed following the write or read operations.</span></span>

## <a name="the-yield-statement"></a><span data-ttu-id="2c009-627">Yield 語句</span><span class="sxs-lookup"><span data-stu-id="2c009-627">The yield statement</span></span>

<span data-ttu-id="2c009-628">語句會用於反覆運算器區塊（[區塊](statements.md#blocks)），以產生反覆運算器的值給枚舉器物件（[枚舉器物件](classes.md#enumerator-objects)）或可列舉物件（[可列舉物件](classes.md#enumerable-objects)），或表示反復專案結束。`yield`</span><span class="sxs-lookup"><span data-stu-id="2c009-628">The `yield` statement is used in an iterator block ([Blocks](statements.md#blocks)) to yield a value to the enumerator object ([Enumerator objects](classes.md#enumerator-objects)) or enumerable object ([Enumerable objects](classes.md#enumerable-objects)) of an iterator or to signal the end of the iteration.</span></span>

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

<span data-ttu-id="2c009-629">`yield`不是保留字;只有當緊接在`return`或`break`關鍵字之前使用時，它才有特殊意義。</span><span class="sxs-lookup"><span data-stu-id="2c009-629">`yield` is not a reserved word; it has special meaning only when used immediately before a `return` or `break` keyword.</span></span> <span data-ttu-id="2c009-630">在其他內容中`yield` ，可以當做識別碼使用。</span><span class="sxs-lookup"><span data-stu-id="2c009-630">In other contexts, `yield` can be used as an identifier.</span></span>

<span data-ttu-id="2c009-631">`yield`語句可能出現的位置有幾項限制，如下所述。</span><span class="sxs-lookup"><span data-stu-id="2c009-631">There are several restrictions on where a `yield` statement can appear, as described in the following.</span></span>

*  <span data-ttu-id="2c009-632">在*method_body*、 *operator_body*或*accessor_body*外出現的 @no__t 0 語句（任一形式）時，會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2c009-632">It is a compile-time error for a `yield` statement (of either form) to appear outside a *method_body*, *operator_body* or *accessor_body*</span></span>
*  <span data-ttu-id="2c009-633">在匿名函式中出現的`yield`語句（其中一種形式）會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2c009-633">It is a compile-time error for a `yield` statement (of either form) to appear inside an anonymous function.</span></span>
*  <span data-ttu-id="2c009-634">`yield`語句的`finally` 子句`try` （其中一種形式）會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2c009-634">It is a compile-time error for a `yield` statement (of either form) to appear in the `finally` clause of a `try` statement.</span></span>
*  <span data-ttu-id="2c009-635">`yield return`語句`catch` 出現`try`在包含任何子句之語句中的任何位置時，會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="2c009-635">It is a compile-time error for a `yield return` statement to appear anywhere in a `try` statement that contains any `catch` clauses.</span></span>

<span data-ttu-id="2c009-636">下列範例顯示語句的`yield`一些有效和無效用法。</span><span class="sxs-lookup"><span data-stu-id="2c009-636">The following example shows some valid and invalid uses of `yield` statements.</span></span>

```csharp
delegate IEnumerable<int> D();

IEnumerator<int> GetEnumerator() {
    try {
        yield return 1;        // Ok
        yield break;           // Ok
    }
    finally {
        yield return 2;        // Error, yield in finally
        yield break;           // Error, yield in finally
    }

    try {
        yield return 3;        // Error, yield return in try...catch
        yield break;           // Ok
    }
    catch {
        yield return 4;        // Error, yield return in try...catch
        yield break;           // Ok
    }

    D d = delegate { 
        yield return 5;        // Error, yield in an anonymous function
    }; 
}

int MyMethod() {
    yield return 1;            // Error, wrong return type for an iterator block
}
```

<span data-ttu-id="2c009-637">隱含轉換（[隱含](conversions.md#implicit-conversions)轉換）必須存在於`yield return`語句中的運算式類型到反覆運算器的產生類型（[yield 類型](classes.md#yield-type)）。</span><span class="sxs-lookup"><span data-stu-id="2c009-637">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) must exist from the type of the expression in the `yield return` statement to the yield type ([Yield type](classes.md#yield-type)) of the iterator.</span></span>

<span data-ttu-id="2c009-638">`yield return`語句的執行方式如下：</span><span class="sxs-lookup"><span data-stu-id="2c009-638">A `yield return` statement is executed as follows:</span></span>

*  <span data-ttu-id="2c009-639">語句中所指定的運算式會進行評估、隱含地轉換成 yield 型別，以及指派`Current`給列舉值物件的屬性。</span><span class="sxs-lookup"><span data-stu-id="2c009-639">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
*  <span data-ttu-id="2c009-640">反覆運算器區塊的執行已暫停。</span><span class="sxs-lookup"><span data-stu-id="2c009-640">Execution of the iterator block is suspended.</span></span> <span data-ttu-id="2c009-641">如果語句位於一個或多個`try`區塊內，此時不`finally`會執行相關聯的區塊。 `yield return`</span><span class="sxs-lookup"><span data-stu-id="2c009-641">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
*  <span data-ttu-id="2c009-642">列舉`MoveNext` 值`true`物件的方法會傳回其呼叫端，表示枚舉器物件已成功前進到下一個專案。</span><span class="sxs-lookup"><span data-stu-id="2c009-642">The `MoveNext` method of the enumerator object returns `true` to its caller, indicating that the enumerator object successfully advanced to the next item.</span></span>

<span data-ttu-id="2c009-643">下一次呼叫列舉值物件的`MoveNext`方法時，會繼續從上次暫停的位置執行反覆運算器區塊。</span><span class="sxs-lookup"><span data-stu-id="2c009-643">The next call to the enumerator object's `MoveNext` method resumes execution of the iterator block from where it was last suspended.</span></span>

<span data-ttu-id="2c009-644">`yield break`語句的執行方式如下：</span><span class="sxs-lookup"><span data-stu-id="2c009-644">A `yield break` statement is executed as follows:</span></span>

*  <span data-ttu-id="2c009-645">`try` `finally` `finally` `try`如果語句是以一或多個具有相關聯區塊的區塊括住，則控制項一開始會傳送到最內層語句的區塊。 `yield break`</span><span class="sxs-lookup"><span data-stu-id="2c009-645">If the `yield break` statement is enclosed by one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="2c009-646">當控制項到達`finally`區塊的結束點時，控制權會轉移`finally`到下一個封入`try`語句的區塊。</span><span class="sxs-lookup"><span data-stu-id="2c009-646">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="2c009-647">此程式會重複執行， `finally`直到所有封閉式`try`語句的區塊都已執行為止。</span><span class="sxs-lookup"><span data-stu-id="2c009-647">This process is repeated until the `finally` blocks of all enclosing `try` statements have been executed.</span></span>
*  <span data-ttu-id="2c009-648">控制項會傳回 iterator 區塊的呼叫端。</span><span class="sxs-lookup"><span data-stu-id="2c009-648">Control is returned to the caller of the iterator block.</span></span> <span data-ttu-id="2c009-649">這可以是列舉`MoveNext`值物件`Dispose`的方法或方法。</span><span class="sxs-lookup"><span data-stu-id="2c009-649">This is either the `MoveNext` method or `Dispose` method of the enumerator object.</span></span>

<span data-ttu-id="2c009-650">因為語句會無條件地將控制權轉移到別處，所以永遠`yield break`無法連線到語句的結束點。 `yield break`</span><span class="sxs-lookup"><span data-stu-id="2c009-650">Because a `yield break` statement unconditionally transfers control elsewhere, the end point of a `yield break` statement is never reachable.</span></span>
