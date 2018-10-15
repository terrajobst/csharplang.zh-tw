# <a name="statements"></a><span data-ttu-id="0edb8-101">陳述式</span><span class="sxs-lookup"><span data-stu-id="0edb8-101">Statements</span></span>

<span data-ttu-id="0edb8-102">C# 提供各種不同的陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-102">C# provides a variety of statements.</span></span> <span data-ttu-id="0edb8-103">大部分的這些陳述式會熟悉的開發人員有程式設計 C 和 c + + 的經驗。</span><span class="sxs-lookup"><span data-stu-id="0edb8-103">Most of these statements will be familiar to developers who have programmed in C and C++.</span></span>

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

<span data-ttu-id="0edb8-104">*Embedded_statement*非終端項用於出現在其他陳述式的陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-104">The *embedded_statement* nonterminal is used for statements that appear within other statements.</span></span> <span data-ttu-id="0edb8-105">善用*embedded_statement*而非*陳述式*排除的宣告陳述式和標記陳述式，在這些內容中的使用。</span><span class="sxs-lookup"><span data-stu-id="0edb8-105">The use of *embedded_statement* rather than *statement* excludes the use of declaration statements and labeled statements in these contexts.</span></span> <span data-ttu-id="0edb8-106">此範例</span><span class="sxs-lookup"><span data-stu-id="0edb8-106">The example</span></span>
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
<span data-ttu-id="0edb8-107">導致編譯時期錯誤，因為`if`陳述式需要*embedded_statement*而非*陳述式*其如果分支。</span><span class="sxs-lookup"><span data-stu-id="0edb8-107">results in a compile-time error because an `if` statement requires an *embedded_statement* rather than a *statement* for its if branch.</span></span> <span data-ttu-id="0edb8-108">如果此程式碼允許的然後將變數`i`就會被宣告，但它永遠都用。</span><span class="sxs-lookup"><span data-stu-id="0edb8-108">If this code were permitted, then the variable `i` would be declared, but it could never be used.</span></span> <span data-ttu-id="0edb8-109">不過請注意，加上`i`的區塊中的宣告，此範例是有效的。</span><span class="sxs-lookup"><span data-stu-id="0edb8-109">Note, however, that by placing `i`'s declaration in a block, the example is valid.</span></span>

## <a name="end-points-and-reachability"></a><span data-ttu-id="0edb8-110">結束點和連線能力</span><span class="sxs-lookup"><span data-stu-id="0edb8-110">End points and reachability</span></span>

<span data-ttu-id="0edb8-111">每個陳述式已***結束點***。</span><span class="sxs-lookup"><span data-stu-id="0edb8-111">Every statement has an ***end point***.</span></span> <span data-ttu-id="0edb8-112">簡單來說，陳述式的結束點是緊接在後面的陳述式的位置。</span><span class="sxs-lookup"><span data-stu-id="0edb8-112">In intuitive terms, the end point of a statement is the location that immediately follows the statement.</span></span> <span data-ttu-id="0edb8-113">複合陳述式 （包含內嵌的陳述式的陳述式） 執行規則指定控制到達結束點的內嵌陳述式時所要採取的動作。</span><span class="sxs-lookup"><span data-stu-id="0edb8-113">The execution rules for composite statements (statements that contain embedded statements) specify the action that is taken when control reaches the end point of an embedded statement.</span></span> <span data-ttu-id="0edb8-114">比方說，當控制項到達結束點的區塊中的陳述式，控制權會轉移至下一個陳述式區塊中。</span><span class="sxs-lookup"><span data-stu-id="0edb8-114">For example, when control reaches the end point of a statement in a block, control is transferred to the next statement in the block.</span></span>

<span data-ttu-id="0edb8-115">如果陳述式可能會執行觸達，陳述式要***連線到***。</span><span class="sxs-lookup"><span data-stu-id="0edb8-115">If a statement can possibly be reached by execution, the statement is said to be ***reachable***.</span></span> <span data-ttu-id="0edb8-116">相反地，如果沒有，則會執行陳述式不可能，陳述式要***無法連線到***。</span><span class="sxs-lookup"><span data-stu-id="0edb8-116">Conversely, if there is no possibility that a statement will be executed, the statement is said to be ***unreachable***.</span></span>

<span data-ttu-id="0edb8-117">在範例</span><span class="sxs-lookup"><span data-stu-id="0edb8-117">In the example</span></span>
```csharp
void F() {
    Console.WriteLine("reachable");
    goto Label;
    Console.WriteLine("unreachable");
    Label:
    Console.WriteLine("reachable");
}
```
<span data-ttu-id="0edb8-118">第二個引動過程的`Console.WriteLine`是不可能執行到的因為不會執行陳述式可能會發生。</span><span class="sxs-lookup"><span data-stu-id="0edb8-118">the second invocation of `Console.WriteLine` is unreachable because there is no possibility that the statement will be executed.</span></span>

<span data-ttu-id="0edb8-119">如果編譯器判斷陳述式是不可能執行到，則會回報警告。</span><span class="sxs-lookup"><span data-stu-id="0edb8-119">A warning is reported if the compiler determines that a statement is unreachable.</span></span> <span data-ttu-id="0edb8-120">它是特別不是錯誤有無法到達陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-120">It is specifically not an error for a statement to be unreachable.</span></span>

<span data-ttu-id="0edb8-121">若要判斷是否可連線的特定陳述式或結束點，編譯器會執行資料流程分析，根據每個陳述式所定義的可執行性規則。</span><span class="sxs-lookup"><span data-stu-id="0edb8-121">To determine whether a particular statement or end point is reachable, the compiler performs flow analysis according to the reachability rules defined for each statement.</span></span> <span data-ttu-id="0edb8-122">流量分析會考量常數運算式的值 ([常數運算式](expressions.md#constant-expressions))，以控制行為的陳述式，但不是會考慮非常數運算式的可能值。</span><span class="sxs-lookup"><span data-stu-id="0edb8-122">The flow analysis takes into account the values of constant expressions ([Constant expressions](expressions.md#constant-expressions)) that control the behavior of statements, but the possible values of non-constant expressions are not considered.</span></span> <span data-ttu-id="0edb8-123">換句話說，為了控制流程分析的詳細資訊，是非常數運算式，指定型別的被視為該類型的任何可能的值。</span><span class="sxs-lookup"><span data-stu-id="0edb8-123">In other words, for purposes of control flow analysis, a non-constant expression of a given type is considered to have any possible value of that type.</span></span>

<span data-ttu-id="0edb8-124">在範例</span><span class="sxs-lookup"><span data-stu-id="0edb8-124">In the example</span></span>
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
<span data-ttu-id="0edb8-125">布林值運算式`if`陳述式是常數運算式，因為這兩個運算元的`==`運算子都是常數。</span><span class="sxs-lookup"><span data-stu-id="0edb8-125">the boolean expression of the `if` statement is a constant expression because both operands of the `==` operator are constants.</span></span> <span data-ttu-id="0edb8-126">因為在編譯時期評估常數的運算式，產生值`false`，則`Console.WriteLine`引動過程會被視為無法連線。</span><span class="sxs-lookup"><span data-stu-id="0edb8-126">As the constant expression is evaluated at compile-time, producing the value `false`, the `Console.WriteLine` invocation is considered unreachable.</span></span> <span data-ttu-id="0edb8-127">不過，如果`i`已變更為本機變數</span><span class="sxs-lookup"><span data-stu-id="0edb8-127">However, if `i` is changed to be a local variable</span></span>
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
<span data-ttu-id="0edb8-128">`Console.WriteLine`引動過程會被視為可以連線，即使事實上，它將永遠不會執行。</span><span class="sxs-lookup"><span data-stu-id="0edb8-128">the `Console.WriteLine` invocation is considered reachable, even though, in reality, it will never be executed.</span></span>

<span data-ttu-id="0edb8-129">*區塊*函式的成員一律會視為連線。</span><span class="sxs-lookup"><span data-stu-id="0edb8-129">The *block* of a function member is always considered reachable.</span></span> <span data-ttu-id="0edb8-130">藉由後續評估每個陳述式區塊中的可執行性規則，您可以判斷任何指定的陳述式的連線能力。</span><span class="sxs-lookup"><span data-stu-id="0edb8-130">By successively evaluating the reachability rules of each statement in a block, the reachability of any given statement can be determined.</span></span>

<span data-ttu-id="0edb8-131">在範例</span><span class="sxs-lookup"><span data-stu-id="0edb8-131">In the example</span></span>
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
<span data-ttu-id="0edb8-132">第二個連線能力`Console.WriteLine`判斷方式如下：</span><span class="sxs-lookup"><span data-stu-id="0edb8-132">the reachability of the second `Console.WriteLine` is determined as follows:</span></span>

*  <span data-ttu-id="0edb8-133">第一個`Console.WriteLine`運算式陳述式因為區塊`F`方法可連線。</span><span class="sxs-lookup"><span data-stu-id="0edb8-133">The first `Console.WriteLine` expression statement is reachable because the block of the `F` method is reachable.</span></span>
*  <span data-ttu-id="0edb8-134">第一個結束點`Console.WriteLine`運算式陳述式是可連線，因為該陳述式連接。</span><span class="sxs-lookup"><span data-stu-id="0edb8-134">The end point of the first `Console.WriteLine` expression statement is reachable because that statement is reachable.</span></span>
*  <span data-ttu-id="0edb8-135">`if`陳述式是可連線，因為結束點的第一個`Console.WriteLine`運算式陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-135">The `if` statement is reachable because the end point of the first `Console.WriteLine` expression statement is reachable.</span></span>
*  <span data-ttu-id="0edb8-136">第二個`Console.WriteLine`運算式陳述式因為的布林運算式`if`陳述式並沒有常數值`false`。</span><span class="sxs-lookup"><span data-stu-id="0edb8-136">The second `Console.WriteLine` expression statement is reachable because the boolean expression of the `if` statement does not have the constant value `false`.</span></span>

<span data-ttu-id="0edb8-137">有兩種情況，就可陳述式的結束點的編譯時期錯誤：</span><span class="sxs-lookup"><span data-stu-id="0edb8-137">There are two situations in which it is a compile-time error for the end point of a statement to be reachable:</span></span>

*  <span data-ttu-id="0edb8-138">因為`switch`陳述式不允許下一節中，切換至 「 繼續 」 參數區段，它是陳述式清單，可連線的 switch 區段的結束點的編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="0edb8-138">Because the `switch` statement does not permit a switch section to "fall through" to the next switch section, it is a compile-time error for the end point of the statement list of a switch section to be reachable.</span></span> <span data-ttu-id="0edb8-139">如果發生此錯誤，它通常是表示，`break`遺漏陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-139">If this error occurs, it is typically an indication that a `break` statement is missing.</span></span>
*  <span data-ttu-id="0edb8-140">它會計算要連線到值的函式成員的區塊的結束點的編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="0edb8-140">It is a compile-time error for the end point of the block of a function member that computes a value to be reachable.</span></span> <span data-ttu-id="0edb8-141">如果發生此錯誤，通常就表示，`return`遺漏陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-141">If this error occurs, it typically is an indication that a `return` statement is missing.</span></span>

## <a name="blocks"></a><span data-ttu-id="0edb8-142">區塊</span><span class="sxs-lookup"><span data-stu-id="0edb8-142">Blocks</span></span>

<span data-ttu-id="0edb8-143">「區塊」可允許在許可單一陳述式的內容中撰寫多個陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-143">A *block* permits multiple statements to be written in contexts where a single statement is allowed.</span></span>

```antlr
block
    : '{' statement_list? '}'
    ;
```

<span data-ttu-id="0edb8-144">A*區塊*包含選擇性*statement_list* ([陳述式會列出](statements.md#statement-lists))、 大括號括住。</span><span class="sxs-lookup"><span data-stu-id="0edb8-144">A *block* consists of an optional *statement_list* ([Statement lists](statements.md#statement-lists)), enclosed in braces.</span></span> <span data-ttu-id="0edb8-145">如果省略陳述式清單，則區塊稱為空白。</span><span class="sxs-lookup"><span data-stu-id="0edb8-145">If the statement list is omitted, the block is said to be empty.</span></span>

<span data-ttu-id="0edb8-146">區塊可能包含宣告陳述式 ([宣告陳述式](statements.md#declaration-statements))。</span><span class="sxs-lookup"><span data-stu-id="0edb8-146">A block may contain declaration statements ([Declaration statements](statements.md#declaration-statements)).</span></span> <span data-ttu-id="0edb8-147">本機變數或常數的範圍內的區塊中宣告是區塊。</span><span class="sxs-lookup"><span data-stu-id="0edb8-147">The scope of a local variable or constant declared in a block is the block.</span></span>

<span data-ttu-id="0edb8-148">區塊會執行，如下所示：</span><span class="sxs-lookup"><span data-stu-id="0edb8-148">A block is executed as follows:</span></span>

*  <span data-ttu-id="0edb8-149">如果區塊是空的則控制權會轉移到區塊的結束點。</span><span class="sxs-lookup"><span data-stu-id="0edb8-149">If the block is empty, control is transferred to the end point of the block.</span></span>
*  <span data-ttu-id="0edb8-150">如果區塊不是空的則控制權會轉移到陳述式清單中。</span><span class="sxs-lookup"><span data-stu-id="0edb8-150">If the block is not empty, control is transferred to the statement list.</span></span> <span data-ttu-id="0edb8-151">時，並控制到達結束點的陳述式清單，控制權會轉移到區塊的結束點。</span><span class="sxs-lookup"><span data-stu-id="0edb8-151">When and if control reaches the end point of the statement list, control is transferred to the end point of the block.</span></span>

<span data-ttu-id="0edb8-152">如果區塊本身是可連線到區塊的陳述式清單。</span><span class="sxs-lookup"><span data-stu-id="0edb8-152">The statement list of a block is reachable if the block itself is reachable.</span></span>

<span data-ttu-id="0edb8-153">如果區塊是空白，或結束點的陳述式清單是可連線到區塊的結束點。</span><span class="sxs-lookup"><span data-stu-id="0edb8-153">The end point of a block is reachable if the block is empty or if the end point of the statement list is reachable.</span></span>

<span data-ttu-id="0edb8-154">A*區塊*，其中包含一或多個`yield`陳述式 ([yield 陳述式](statements.md#the-yield-statement)) 呼叫迭代器區塊。</span><span class="sxs-lookup"><span data-stu-id="0edb8-154">A *block* that contains one or more `yield` statements ([The yield statement](statements.md#the-yield-statement)) is called an iterator block.</span></span> <span data-ttu-id="0edb8-155">迭代器區塊用來實作的迭代器的函式成員 ([迭代器](classes.md#iterators))。</span><span class="sxs-lookup"><span data-stu-id="0edb8-155">Iterator blocks are used to implement function members as iterators ([Iterators](classes.md#iterators)).</span></span> <span data-ttu-id="0edb8-156">一些其他的限制將套用至迭代器區塊：</span><span class="sxs-lookup"><span data-stu-id="0edb8-156">Some additional restrictions apply to iterator blocks:</span></span>

*  <span data-ttu-id="0edb8-157">它是編譯時期錯誤`return`才會出現在迭代器區塊的陳述式 (但`yield return`允許陳述式)。</span><span class="sxs-lookup"><span data-stu-id="0edb8-157">It is a compile-time error for a `return` statement to appear in an iterator block (but `yield return` statements are permitted).</span></span>
*  <span data-ttu-id="0edb8-158">它是迭代器區塊包含不安全的內容的編譯時間錯誤 ([Unsafe 內容](unsafe-code.md#unsafe-contexts))。</span><span class="sxs-lookup"><span data-stu-id="0edb8-158">It is a compile-time error for an iterator block to contain an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span> <span data-ttu-id="0edb8-159">迭代器區塊一律會定義安全的內容中，即使其宣告為巢狀方式置於不安全的內容。</span><span class="sxs-lookup"><span data-stu-id="0edb8-159">An iterator block always defines a safe context, even when its declaration is nested in an unsafe context.</span></span>

### <a name="statement-lists"></a><span data-ttu-id="0edb8-160">陳述式清單</span><span class="sxs-lookup"><span data-stu-id="0edb8-160">Statement lists</span></span>

<span data-ttu-id="0edb8-161">A***陳述式清單***順序中編寫的一或多個陳述式所組成。</span><span class="sxs-lookup"><span data-stu-id="0edb8-161">A ***statement list*** consists of one or more statements written in sequence.</span></span> <span data-ttu-id="0edb8-162">陳述式清單中發生*區塊*s ([區塊](statements.md#blocks)) 並在*switch_block*s ([switch 陳述式](statements.md#the-switch-statement))。</span><span class="sxs-lookup"><span data-stu-id="0edb8-162">Statement lists occur in *block*s ([Blocks](statements.md#blocks)) and in *switch_block*s ([The switch statement](statements.md#the-switch-statement)).</span></span>

```antlr
statement_list
    : statement+
    ;
```

<span data-ttu-id="0edb8-163">將控制權傳輸至第一個陳述式時，會執行陳述式清單。</span><span class="sxs-lookup"><span data-stu-id="0edb8-163">A statement list is executed by transferring control to the first statement.</span></span> <span data-ttu-id="0edb8-164">時，並控制到達結束點，陳述式，控制權會轉移至下一個陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-164">When and if control reaches the end point of a statement, control is transferred to the next statement.</span></span> <span data-ttu-id="0edb8-165">時，並控制到達結束點的最後一個陳述式，控制權會轉移至陳述式清單的結束點。</span><span class="sxs-lookup"><span data-stu-id="0edb8-165">When and if control reaches the end point of the last statement, control is transferred to the end point of the statement list.</span></span>

<span data-ttu-id="0edb8-166">陳述式清單中的陳述式是可連線，如果至少下列其中一項條件成立：</span><span class="sxs-lookup"><span data-stu-id="0edb8-166">A statement in a statement list is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="0edb8-167">陳述式是第一個陳述式，且連線到本身，陳述式清單。</span><span class="sxs-lookup"><span data-stu-id="0edb8-167">The statement is the first statement and the statement list itself is reachable.</span></span>
*  <span data-ttu-id="0edb8-168">連線到前面的陳述式結束點。</span><span class="sxs-lookup"><span data-stu-id="0edb8-168">The end point of the preceding statement is reachable.</span></span>
*  <span data-ttu-id="0edb8-169">陳述式是加上標籤的陳述式和標籤由可存取參考`goto`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-169">The statement is a labeled statement and the label is referenced by a reachable `goto` statement.</span></span>

<span data-ttu-id="0edb8-170">如果在清單中的最後一個陳述式結束點連線到連線到結束點的陳述式清單。</span><span class="sxs-lookup"><span data-stu-id="0edb8-170">The end point of a statement list is reachable if the end point of the last statement in the list is reachable.</span></span>

## <a name="the-empty-statement"></a><span data-ttu-id="0edb8-171">空陳述式</span><span class="sxs-lookup"><span data-stu-id="0edb8-171">The empty statement</span></span>

<span data-ttu-id="0edb8-172">*Empty_statement*不執行任何動作。</span><span class="sxs-lookup"><span data-stu-id="0edb8-172">An *empty_statement* does nothing.</span></span>

```antlr
empty_statement
    : ';'
    ;
```

<span data-ttu-id="0edb8-173">沒有任何作業的內容中執行需要一個陳述式的情況時，會使用空的陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-173">An empty statement is used when there are no operations to perform in a context where a statement is required.</span></span>

<span data-ttu-id="0edb8-174">執行空白的陳述式只會將控制權傳輸至陳述式的結束點。</span><span class="sxs-lookup"><span data-stu-id="0edb8-174">Execution of an empty statement simply transfers control to the end point of the statement.</span></span> <span data-ttu-id="0edb8-175">因此，空的陳述式結束點是空的陳述式是否可連線到。</span><span class="sxs-lookup"><span data-stu-id="0edb8-175">Thus, the end point of an empty statement is reachable if the empty statement is reachable.</span></span>

<span data-ttu-id="0edb8-176">寫入時，就可以使用空的陳述式`while`null 主體陳述式：</span><span class="sxs-lookup"><span data-stu-id="0edb8-176">An empty statement can be used when writing a `while` statement with a null body:</span></span>
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

<span data-ttu-id="0edb8-177">此外，空的陳述式可用來宣告結尾之前的標籤 「`}`"區塊的：</span><span class="sxs-lookup"><span data-stu-id="0edb8-177">Also, an empty statement can be used to declare a label just before the closing "`}`" of a block:</span></span>
```csharp
void F() {
    ...
    if (done) goto exit;
    ...
    exit: ;
}
```

## <a name="labeled-statements"></a><span data-ttu-id="0edb8-178">標記陳述式</span><span class="sxs-lookup"><span data-stu-id="0edb8-178">Labeled statements</span></span>

<span data-ttu-id="0edb8-179">A *labeled_statement*允許要加上標籤的陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-179">A *labeled_statement* permits a statement to be prefixed by a label.</span></span> <span data-ttu-id="0edb8-180">標記陳述式允許在區塊內，但不是允許做為內嵌的陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-180">Labeled statements are permitted in blocks, but are not permitted as embedded statements.</span></span>

```antlr
labeled_statement
    : identifier ':' statement
    ;
```

<span data-ttu-id="0edb8-181">Labeled 陳述式使用指定的名稱會宣告一個標籤*識別碼*。</span><span class="sxs-lookup"><span data-stu-id="0edb8-181">A labeled statement declares a label with the name given by the *identifier*.</span></span> <span data-ttu-id="0edb8-182">標籤的範圍是整個區塊標籤宣告，包括所有巢狀區塊。</span><span class="sxs-lookup"><span data-stu-id="0edb8-182">The scope of a label is the whole block in which the label is declared, including any nested blocks.</span></span> <span data-ttu-id="0edb8-183">它是具有重疊範圍的相同名稱的兩個標籤的編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="0edb8-183">It is a compile-time error for two labels with the same name to have overlapping scopes.</span></span>

<span data-ttu-id="0edb8-184">您可以從參考的標籤`goto`陳述式 ([goto 陳述式](statements.md#the-goto-statement)) 範圍內的標籤。</span><span class="sxs-lookup"><span data-stu-id="0edb8-184">A label can be referenced from `goto` statements ([The goto statement](statements.md#the-goto-statement)) within the scope of the label.</span></span> <span data-ttu-id="0edb8-185">這表示`goto`陳述式可以傳輸控制區塊內和區塊，但永遠不會分成區塊。</span><span class="sxs-lookup"><span data-stu-id="0edb8-185">This means that `goto` statements can transfer control within blocks and out of blocks, but never into blocks.</span></span>

<span data-ttu-id="0edb8-186">標籤會有自己的宣告空間，而且不會干擾其他識別項。</span><span class="sxs-lookup"><span data-stu-id="0edb8-186">Labels have their own declaration space and do not interfere with other identifiers.</span></span> <span data-ttu-id="0edb8-187">此範例</span><span class="sxs-lookup"><span data-stu-id="0edb8-187">The example</span></span>
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
<span data-ttu-id="0edb8-188">無效，而且會使用名稱`x`做為參數和標籤。</span><span class="sxs-lookup"><span data-stu-id="0edb8-188">is valid and uses the name `x` as both a parameter and a label.</span></span>

<span data-ttu-id="0edb8-189">加上標籤的陳述式的執行就相當於下列標籤陳述式執行。</span><span class="sxs-lookup"><span data-stu-id="0edb8-189">Execution of a labeled statement corresponds exactly to execution of the statement following the label.</span></span>

<span data-ttu-id="0edb8-190">除了一般控制流程所提供的連線能力，加上標籤的陳述式是如果標籤參考可連線到`goto`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-190">In addition to the reachability provided by normal flow of control, a labeled statement is reachable if the label is referenced by a reachable `goto` statement.</span></span> <span data-ttu-id="0edb8-191">(例外狀況： 如果`goto`陳述式位於`try`包含`finally`區塊，並加上標籤的陳述式已超出`try`，和結束點的`finally`區塊是無法連上，然後加上標籤的陳述式不是無法從可連線到`goto`陳述式。)</span><span class="sxs-lookup"><span data-stu-id="0edb8-191">(Exception: If a `goto` statement is inside a `try` that includes a `finally` block, and the labeled statement is outside the `try`, and the end point of the `finally` block is unreachable, then the labeled statement is not reachable from that `goto` statement.)</span></span>

## <a name="declaration-statements"></a><span data-ttu-id="0edb8-192">宣告陳述式</span><span class="sxs-lookup"><span data-stu-id="0edb8-192">Declaration statements</span></span>

<span data-ttu-id="0edb8-193">A *declaration_statement*宣告本機變數或常數。</span><span class="sxs-lookup"><span data-stu-id="0edb8-193">A *declaration_statement* declares a local variable or constant.</span></span> <span data-ttu-id="0edb8-194">宣告陳述式允許在區塊內，但不是允許做為內嵌的陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-194">Declaration statements are permitted in blocks, but are not permitted as embedded statements.</span></span>

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a><span data-ttu-id="0edb8-195">本機變數宣告</span><span class="sxs-lookup"><span data-stu-id="0edb8-195">Local variable declarations</span></span>

<span data-ttu-id="0edb8-196">A *local_variable_declaration*會宣告一個或多個本機變數。</span><span class="sxs-lookup"><span data-stu-id="0edb8-196">A *local_variable_declaration* declares one or more local variables.</span></span>

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

<span data-ttu-id="0edb8-197">*Local_variable_type*的*local_variable_declaration*直接指定變數的宣告所引進的類型，或具有識別項表示`var`，應該根據初始設定式推斷類型。</span><span class="sxs-lookup"><span data-stu-id="0edb8-197">The *local_variable_type* of a *local_variable_declaration* either directly specifies the type of the variables introduced by the declaration, or indicates with the identifier `var` that the type should be inferred based on an initializer.</span></span> <span data-ttu-id="0edb8-198">類型後面接著一份*local_variable_declarator*s，其中每一個導入了新的變數。</span><span class="sxs-lookup"><span data-stu-id="0edb8-198">The type is followed by a list of *local_variable_declarator*s, each of which introduces a new variable.</span></span> <span data-ttu-id="0edb8-199">A *local_variable_declarator*組成*識別項*可命名變數，並且選擇性地加 」`=`"語彙基元和*local_variable_initializer*提供變數的初始值。</span><span class="sxs-lookup"><span data-stu-id="0edb8-199">A *local_variable_declarator* consists of an *identifier* that names the variable, optionally followed by an "`=`" token and a *local_variable_initializer* that gives the initial value of the variable.</span></span>

<span data-ttu-id="0edb8-200">在區域變數宣告的內容中，識別碼 var 做為內容的關鍵字 ([關鍵字](lexical-structure.md#keywords))。當*local_variable_type*指定為`var`且沒有名為的型別`var`是在範圍內，宣告是***隱含類型區域變數宣告***，其型別是從相關聯的初始設定式運算式的型別推斷。</span><span class="sxs-lookup"><span data-stu-id="0edb8-200">In the context of a local variable declaration, the identifier var acts as a contextual keyword ([Keywords](lexical-structure.md#keywords)).When the *local_variable_type* is specified as `var` and no type named `var` is in scope, the declaration is an ***implicitly typed local variable declaration***, whose type is inferred from the type of the associated initializer expression.</span></span> <span data-ttu-id="0edb8-201">隱含類型區域變數宣告受限於下列限制：</span><span class="sxs-lookup"><span data-stu-id="0edb8-201">Implicitly typed local variable declarations are subject to the following restrictions:</span></span>

*  <span data-ttu-id="0edb8-202">*Local_variable_declaration*不能包含多個*local_variable_declarator*s。</span><span class="sxs-lookup"><span data-stu-id="0edb8-202">The *local_variable_declaration* cannot include multiple *local_variable_declarator*s.</span></span>
*  <span data-ttu-id="0edb8-203">*Local_variable_declarator*必須包含*local_variable_initializer*。</span><span class="sxs-lookup"><span data-stu-id="0edb8-203">The *local_variable_declarator* must include a *local_variable_initializer*.</span></span>
*  <span data-ttu-id="0edb8-204">*Local_variable_initializer*必須*運算式*。</span><span class="sxs-lookup"><span data-stu-id="0edb8-204">The *local_variable_initializer* must be an *expression*.</span></span>
*  <span data-ttu-id="0edb8-205">初始設定式*運算式*類型必須是編譯時期。</span><span class="sxs-lookup"><span data-stu-id="0edb8-205">The initializer *expression* must have a compile-time type.</span></span>
*  <span data-ttu-id="0edb8-206">初始設定式*運算式*宣告的變數本身不能參考</span><span class="sxs-lookup"><span data-stu-id="0edb8-206">The initializer *expression* cannot refer to the declared variable itself</span></span>

<span data-ttu-id="0edb8-207">不正確隱含類型區域變數宣告的範例如下：</span><span class="sxs-lookup"><span data-stu-id="0edb8-207">The following are examples of incorrect implicitly typed local variable declarations:</span></span>

```csharp
var x;               // Error, no initializer to infer type from
var y = {1, 2, 3};   // Error, array initializer not permitted
var z = null;        // Error, null does not have a type
var u = x => x + 1;  // Error, anonymous functions do not have a type
var v = v++;         // Error, initializer cannot refer to variable itself
```

<span data-ttu-id="0edb8-208">在運算式中使用，取得區域變數的值*simple_name* ([簡單名稱](expressions.md#simple-names))，並使用修改的本機變數值*指派*([指派運算子](expressions.md#assignment-operators))。</span><span class="sxs-lookup"><span data-stu-id="0edb8-208">The value of a local variable is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)), and the value of a local variable is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="0edb8-209">必須明確地指派本機變數 ([明確指派](variables.md#definite-assignment)) 在其中取得它的值是每個位置。</span><span class="sxs-lookup"><span data-stu-id="0edb8-209">A local variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) at each location where its value is obtained.</span></span>

<span data-ttu-id="0edb8-210">範圍中宣告的區域變數*local_variable_declaration*是區塊中宣告已發生。</span><span class="sxs-lookup"><span data-stu-id="0edb8-210">The scope of a local variable declared in a *local_variable_declaration* is the block in which the declaration occurs.</span></span> <span data-ttu-id="0edb8-211">它是錯誤，請參閱之前的文字位置中的區域變數*local_variable_declarator*的區域變數。</span><span class="sxs-lookup"><span data-stu-id="0edb8-211">It is an error to refer to a local variable in a textual position that precedes the *local_variable_declarator* of the local variable.</span></span> <span data-ttu-id="0edb8-212">本機變數的範圍，它是宣告另一個本機變數或常數具有相同名稱的編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="0edb8-212">Within the scope of a local variable, it is a compile-time error to declare another local variable or constant with the same name.</span></span>

<span data-ttu-id="0edb8-213">區域變數宣告會宣告多個變數，相當於多個具有相同類型的單一變數宣告。</span><span class="sxs-lookup"><span data-stu-id="0edb8-213">A local variable declaration that declares multiple variables is equivalent to multiple declarations of single variables with the same type.</span></span> <span data-ttu-id="0edb8-214">此外，在本機變數中宣告的變數初始設定式，就相當於宣告之後，立即插入在指派陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-214">Furthermore, a variable initializer in a local variable declaration corresponds exactly to an assignment statement that is inserted immediately after the declaration.</span></span>

<span data-ttu-id="0edb8-215">此範例</span><span class="sxs-lookup"><span data-stu-id="0edb8-215">The example</span></span>
```csharp
void F() {
    int x = 1, y, z = x * 2;
}
```
<span data-ttu-id="0edb8-216">完全對應</span><span class="sxs-lookup"><span data-stu-id="0edb8-216">corresponds exactly to</span></span>
```csharp
void F() {
    int x; x = 1;
    int y;
    int z; z = x * 2;
}
```

<span data-ttu-id="0edb8-217">隱含類型區域變數宣告中，所宣告的本機變數的類型會是用來將變數初始化運算式的類型相同。</span><span class="sxs-lookup"><span data-stu-id="0edb8-217">In an implicitly typed local variable declaration, the type of the local variable being declared is taken to be the same as the type of the expression used to initialize the variable.</span></span> <span data-ttu-id="0edb8-218">例如: </span><span class="sxs-lookup"><span data-stu-id="0edb8-218">For example:</span></span>
```csharp
var i = 5;
var s = "Hello";
var d = 1.0;
var numbers = new int[] {1, 2, 3};
var orders = new Dictionary<int,Order>();
```

<span data-ttu-id="0edb8-219">隱含類型區域變數宣告上述是相當於下列明確類型宣告：</span><span class="sxs-lookup"><span data-stu-id="0edb8-219">The implicitly typed local variable declarations above are precisely equivalent to the following explicitly typed declarations:</span></span>
```csharp
int i = 5;
string s = "Hello";
double d = 1.0;
int[] numbers = new int[] {1, 2, 3};
Dictionary<int,Order> orders = new Dictionary<int,Order>();
```

### <a name="local-constant-declarations"></a><span data-ttu-id="0edb8-220">區域常數宣告</span><span class="sxs-lookup"><span data-stu-id="0edb8-220">Local constant declarations</span></span>

<span data-ttu-id="0edb8-221">A *local_constant_declaration*宣告一或多個區域的常數。</span><span class="sxs-lookup"><span data-stu-id="0edb8-221">A *local_constant_declaration* declares one or more local constants.</span></span>

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

<span data-ttu-id="0edb8-222">*型別*的*local_constant_declaration*指定的宣告所引進的常數類型。</span><span class="sxs-lookup"><span data-stu-id="0edb8-222">The *type* of a *local_constant_declaration* specifies the type of the constants introduced by the declaration.</span></span> <span data-ttu-id="0edb8-223">類型後面接著一份*constant_declarator*s，其中每一個導入了新的常數。</span><span class="sxs-lookup"><span data-stu-id="0edb8-223">The type is followed by a list of *constant_declarator*s, each of which introduces a new constant.</span></span> <span data-ttu-id="0edb8-224">A *constant_declarator*組成*識別項*該名稱的常數，後面加上 「`=`"語彙基元，後面接著*constant_expression* ([常數運算式](expressions.md#constant-expressions)) 提供常數值。</span><span class="sxs-lookup"><span data-stu-id="0edb8-224">A *constant_declarator* consists of an *identifier* that names the constant, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the constant.</span></span>

<span data-ttu-id="0edb8-225">*型別*並*constant_expression*的區域常數宣告必須遵循相同的常數成員宣告的規則 ([常數](classes.md#constants))。</span><span class="sxs-lookup"><span data-stu-id="0edb8-225">The *type* and *constant_expression* of a local constant declaration must follow the same rules as those of a constant member declaration ([Constants](classes.md#constants)).</span></span>

<span data-ttu-id="0edb8-226">本機常數的值取得在運算式中使用*simple_name* ([簡單名稱](expressions.md#simple-names))。</span><span class="sxs-lookup"><span data-stu-id="0edb8-226">The value of a local constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="0edb8-227">本機常數的範圍是區塊中宣告已發生。</span><span class="sxs-lookup"><span data-stu-id="0edb8-227">The scope of a local constant is the block in which the declaration occurs.</span></span> <span data-ttu-id="0edb8-228">它是錯誤之前的文字位置的區域常數是指其*constant_declarator*。</span><span class="sxs-lookup"><span data-stu-id="0edb8-228">It is an error to refer to a local constant in a textual position that precedes its *constant_declarator*.</span></span> <span data-ttu-id="0edb8-229">範圍內的區域常數，它可以是宣告另一個本機變數或常數具有相同名稱的編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="0edb8-229">Within the scope of a local constant, it is a compile-time error to declare another local variable or constant with the same name.</span></span>

<span data-ttu-id="0edb8-230">在區域常數宣告會宣告多個常數，相當於單一常數的多個具有相同類型的宣告。</span><span class="sxs-lookup"><span data-stu-id="0edb8-230">A local constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same type.</span></span>

## <a name="expression-statements"></a><span data-ttu-id="0edb8-231">運算式陳述式</span><span class="sxs-lookup"><span data-stu-id="0edb8-231">Expression statements</span></span>

<span data-ttu-id="0edb8-232">*Expression_statement*評估指定的運算式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-232">An *expression_statement* evaluates a given expression.</span></span> <span data-ttu-id="0edb8-233">值計算運算式，如果有的話，將會被捨棄。</span><span class="sxs-lookup"><span data-stu-id="0edb8-233">The value computed by the expression, if any, is discarded.</span></span>

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

<span data-ttu-id="0edb8-234">並非所有運算式都允許做為陳述式中。</span><span class="sxs-lookup"><span data-stu-id="0edb8-234">Not all expressions are permitted as statements.</span></span> <span data-ttu-id="0edb8-235">特別是運算式，例如在`x + y`和`x == 1`，只是計算值 （這將會被捨棄）、 不允許做為陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-235">In particular, expressions such as `x + y` and `x == 1` that merely compute a value (which will be discarded), are not permitted as statements.</span></span>

<span data-ttu-id="0edb8-236">執行*expression_statement*會評估包含的運算式，並再將控制權傳輸至結束點*expression_statement*。</span><span class="sxs-lookup"><span data-stu-id="0edb8-236">Execution of an *expression_statement* evaluates the contained expression and then transfers control to the end point of the *expression_statement*.</span></span> <span data-ttu-id="0edb8-237">結束點*expression_statement*連線，如果該*expression_statement*連線。</span><span class="sxs-lookup"><span data-stu-id="0edb8-237">The end point of an *expression_statement* is reachable if that *expression_statement* is reachable.</span></span>

## <a name="selection-statements"></a><span data-ttu-id="0edb8-238">選取範圍陳述式</span><span class="sxs-lookup"><span data-stu-id="0edb8-238">Selection statements</span></span>

<span data-ttu-id="0edb8-239">選取範圍陳述式選取其中一個可能根據某個運算式的值來執行的陳述式的數目。</span><span class="sxs-lookup"><span data-stu-id="0edb8-239">Selection statements select one of a number of possible statements for execution based on the value of some expression.</span></span>

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a><span data-ttu-id="0edb8-240">If 陳述式</span><span class="sxs-lookup"><span data-stu-id="0edb8-240">The if statement</span></span>

<span data-ttu-id="0edb8-241">`if`陳述式選取根據布林運算式的值來執行的陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-241">The `if` statement selects a statement for execution based on the value of a boolean expression.</span></span>

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

<span data-ttu-id="0edb8-242">`else`部分是語彙最接近的前面加上相關聯`if`所允許的語法。</span><span class="sxs-lookup"><span data-stu-id="0edb8-242">An `else` part is associated with the lexically nearest preceding `if` that is allowed by the syntax.</span></span> <span data-ttu-id="0edb8-243">因此，`if`表單的陳述式</span><span class="sxs-lookup"><span data-stu-id="0edb8-243">Thus, an `if` statement of the form</span></span>
```csharp
if (x) if (y) F(); else G();
```
<span data-ttu-id="0edb8-244">相當於</span><span class="sxs-lookup"><span data-stu-id="0edb8-244">is equivalent to</span></span>
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

<span data-ttu-id="0edb8-245">`if`陳述式，如下所示：</span><span class="sxs-lookup"><span data-stu-id="0edb8-245">An `if` statement is executed as follows:</span></span>

*  <span data-ttu-id="0edb8-246">*Boolean_expression* ([布林運算式](expressions.md#boolean-expressions)) 進行評估。</span><span class="sxs-lookup"><span data-stu-id="0edb8-246">The *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span>
*  <span data-ttu-id="0edb8-247">如果布林運算式會產生`true`，控制權會轉移到第一個內嵌的陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-247">If the boolean expression yields `true`, control is transferred to the first embedded statement.</span></span> <span data-ttu-id="0edb8-248">時，並控制到達結束點，該陳述式，控制權會轉移至結束點`if`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-248">When and if control reaches the end point of that statement, control is transferred to the end point of the `if` statement.</span></span>
*  <span data-ttu-id="0edb8-249">如果布林運算式會產生`false`而如果`else`一部份存在，則控制權會轉移到第二個內嵌的陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-249">If the boolean expression yields `false` and if an `else` part is present, control is transferred to the second embedded statement.</span></span> <span data-ttu-id="0edb8-250">時，並控制到達結束點，該陳述式，控制權會轉移至結束點`if`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-250">When and if control reaches the end point of that statement, control is transferred to the end point of the `if` statement.</span></span>
*  <span data-ttu-id="0edb8-251">如果布林運算式會產生`false`而如果`else`組件不存在，控制權會轉移至結束點`if`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-251">If the boolean expression yields `false` and if an `else` part is not present, control is transferred to the end point of the `if` statement.</span></span>

<span data-ttu-id="0edb8-252">第一個內嵌的陳述式`if`陳述式是連線到如果`if`陳述式和布林值的運算式不是常數值`false`。</span><span class="sxs-lookup"><span data-stu-id="0edb8-252">The first embedded statement of an `if` statement is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `false`.</span></span>

<span data-ttu-id="0edb8-253">第二個內嵌的陳述式`if`陳述式，如果有的話，會連線到如果`if`陳述式和布林值的運算式不是常數值`true`。</span><span class="sxs-lookup"><span data-stu-id="0edb8-253">The second embedded statement of an `if` statement, if present, is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

<span data-ttu-id="0edb8-254">結束點`if`陳述式是連線到其內嵌的陳述式中至少一個結束點是否可連線。</span><span class="sxs-lookup"><span data-stu-id="0edb8-254">The end point of an `if` statement is reachable if the end point of at least one of its embedded statements is reachable.</span></span> <span data-ttu-id="0edb8-255">此外，結束點`if`陳述式沒有`else`部分是連線到如果`if`陳述式和布林值的運算式不是常數值`true`。</span><span class="sxs-lookup"><span data-stu-id="0edb8-255">In addition, the end point of an `if` statement with no `else` part is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-switch-statement"></a><span data-ttu-id="0edb8-256">Switch 陳述式</span><span class="sxs-lookup"><span data-stu-id="0edb8-256">The switch statement</span></span>

<span data-ttu-id="0edb8-257">執行選取的 switch 陳述式具有對應至對 switch 運算式的值相關聯的參數標籤的陳述式清單。</span><span class="sxs-lookup"><span data-stu-id="0edb8-257">The switch statement selects for execution a statement list having an associated switch label that corresponds to the value of the switch expression.</span></span>

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

<span data-ttu-id="0edb8-258">A *switch_statement*組成關鍵字`switch`，後面接著括號運算式 （稱為 switch 運算式），再接著*switch_block*。</span><span class="sxs-lookup"><span data-stu-id="0edb8-258">A *switch_statement* consists of the keyword `switch`, followed by a parenthesized expression (called the switch expression), followed by a *switch_block*.</span></span> <span data-ttu-id="0edb8-259">*Switch_block*包含零或多個*switch_section*s，大括號括住。</span><span class="sxs-lookup"><span data-stu-id="0edb8-259">The *switch_block* consists of zero or more *switch_section*s, enclosed in braces.</span></span> <span data-ttu-id="0edb8-260">每個*switch_section*包含一個或多個*switch_label*s 後面*statement_list* ([陳述式會列出](statements.md#statement-lists))。</span><span class="sxs-lookup"><span data-stu-id="0edb8-260">Each *switch_section* consists of one or more *switch_label*s followed by a *statement_list* ([Statement lists](statements.md#statement-lists)).</span></span>

<span data-ttu-id="0edb8-261">***型別來控管***的`switch`陳述式會建立 switch 運算式的方法。</span><span class="sxs-lookup"><span data-stu-id="0edb8-261">The ***governing type*** of a `switch` statement is established by the switch expression.</span></span>

*  <span data-ttu-id="0edb8-262">對 switch 運算式的類型是否`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `bool`， `char`， `string`，或*enum_type*，或如果它是可為 null 的型別對應至其中一個類型，則這是在管理類型`switch`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-262">If the type of the switch expression is `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `bool`, `char`, `string`, or an *enum_type*, or if it is the nullable type corresponding to one of these types, then that is the governing type of the `switch` statement.</span></span>
*  <span data-ttu-id="0edb8-263">否則，只有一個使用者定義的隱含轉換 ([使用者定義轉換](conversions.md#user-defined-conversions)) 控管類型的下列可能的其中一個 switch 運算式的類型必須存在： `sbyte`， `byte`， `short``ushort`， `int`， `uint`， `long`， `ulong`， `char`， `string`，或為 null 的類型對應至其中一種類型。</span><span class="sxs-lookup"><span data-stu-id="0edb8-263">Otherwise, exactly one user-defined implicit conversion ([User-defined conversions](conversions.md#user-defined-conversions)) must exist from the type of the switch expression to one of the following possible governing types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `string`, or,  a nullable type corresponding to one of those types.</span></span>
*  <span data-ttu-id="0edb8-264">否則，如果沒有這類隱含的轉換存在，或存在多個這類隱含轉換，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="0edb8-264">Otherwise, if no such implicit conversion exists, or if more than one such implicit conversion exists, a compile-time error occurs.</span></span>

<span data-ttu-id="0edb8-265">每個常數的運算式`case`標籤必須表示為隱含轉換的值 ([隱含轉換](conversions.md#implicit-conversions)) 的控管的型別`switch`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-265">The constant expression of each `case` label must denote a value that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the governing type of the `switch` statement.</span></span> <span data-ttu-id="0edb8-266">如果兩個或多個，就會發生編譯時期錯誤`case`標籤在同一個`switch`陳述式指定相同的常數值。</span><span class="sxs-lookup"><span data-stu-id="0edb8-266">A compile-time error occurs if two or more `case` labels in the same `switch` statement specify the same constant value.</span></span>

<span data-ttu-id="0edb8-267">可以有最多一個`default`switch 陳述式中的標籤。</span><span class="sxs-lookup"><span data-stu-id="0edb8-267">There can be at most one `default` label in a switch statement.</span></span>

<span data-ttu-id="0edb8-268">A`switch`陳述式，如下所示：</span><span class="sxs-lookup"><span data-stu-id="0edb8-268">A `switch` statement is executed as follows:</span></span>

*  <span data-ttu-id="0edb8-269">Switch 運算式會評估，並控管的型別轉換。</span><span class="sxs-lookup"><span data-stu-id="0edb8-269">The switch expression is evaluated and converted to the governing type.</span></span>
*  <span data-ttu-id="0edb8-270">如果在指定的其中一個常數`case`標籤在同一個`switch`陳述式是對 switch 運算式的值相等，控制權會轉移到下列相符的陳述式清單`case`標籤。</span><span class="sxs-lookup"><span data-stu-id="0edb8-270">If one of the constants specified in a `case` label in the same `switch` statement is equal to the value of the switch expression, control is transferred to the statement list following the matched `case` label.</span></span>
*  <span data-ttu-id="0edb8-271">如果沒有在指定的常數`case`標籤在同一個`switch`陳述式會等於 switch 運算式的值，才`default`標籤存在，則控制權會轉移到之後的陳述式清單`default`標籤。</span><span class="sxs-lookup"><span data-stu-id="0edb8-271">If none of the constants specified in `case` labels in the same `switch` statement is equal to the value of the switch expression, and if a `default` label is present, control is transferred to the statement list following the `default` label.</span></span>
*  <span data-ttu-id="0edb8-272">如果沒有任何常數中指定`case`標籤在同一個`switch`陳述式會等於 switch 運算式的值，如果沒有`default`標籤存在，則控制權會轉移至結束點`switch`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-272">If none of the constants specified in `case` labels in the same `switch` statement is equal to the value of the switch expression, and if no `default` label is present, control is transferred to the end point of the `switch` statement.</span></span>

<span data-ttu-id="0edb8-273">如果陳述式清單的參數區段的結束點連線到，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="0edb8-273">If the end point of the statement list of a switch section is reachable, a compile-time error occurs.</span></span> <span data-ttu-id="0edb8-274">這稱為 「 不落入 」 規則。</span><span class="sxs-lookup"><span data-stu-id="0edb8-274">This is known as the "no fall through" rule.</span></span> <span data-ttu-id="0edb8-275">此範例</span><span class="sxs-lookup"><span data-stu-id="0edb8-275">The example</span></span>
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
<span data-ttu-id="0edb8-276">因為沒有 switch 區段具有可聯繫的端點，則會有效。</span><span class="sxs-lookup"><span data-stu-id="0edb8-276">is valid because no switch section has a reachable end point.</span></span> <span data-ttu-id="0edb8-277">不同於 C 和 c + + 中，執行的 switch 區段不允許 「 繼續 」 至下一步 的 參數 區段中，與範例</span><span class="sxs-lookup"><span data-stu-id="0edb8-277">Unlike C and C++, execution of a switch section is not permitted to "fall through" to the next switch section, and the example</span></span>
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
<span data-ttu-id="0edb8-278">會導致編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="0edb8-278">results in a compile-time error.</span></span> <span data-ttu-id="0edb8-279">執行的 switch 區段時接著執行另一個交換器區段，明確`goto case`或`goto default`必須使用陳述式：</span><span class="sxs-lookup"><span data-stu-id="0edb8-279">When execution of a switch section is to be followed by execution of another switch section, an explicit `goto case` or `goto default` statement must be used:</span></span>
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

<span data-ttu-id="0edb8-280">允許使用多個標籤*switch_section*。</span><span class="sxs-lookup"><span data-stu-id="0edb8-280">Multiple labels are permitted in a *switch_section*.</span></span> <span data-ttu-id="0edb8-281">此範例</span><span class="sxs-lookup"><span data-stu-id="0edb8-281">The example</span></span>
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
<span data-ttu-id="0edb8-282">是有效的。</span><span class="sxs-lookup"><span data-stu-id="0edb8-282">is valid.</span></span> <span data-ttu-id="0edb8-283">範例沒有違反 「 不落入 」 規則，因為標籤`case 2:`並`default:`都屬於相同*switch_section*。</span><span class="sxs-lookup"><span data-stu-id="0edb8-283">The example does not violate the "no fall through" rule because the labels `case 2:` and `default:` are part of the same *switch_section*.</span></span>

<span data-ttu-id="0edb8-284">「 不落入 」 規則會防止發生在 C 和 c + + 中的 bug 的一般類別時`break`不小心省略陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-284">The "no fall through" rule prevents a common class of bugs that occur in C and C++ when `break` statements are accidentally omitted.</span></span> <span data-ttu-id="0edb8-285">此外，因為這項規則的參數區段`switch`陳述式可以任意重新排列而不會影響陳述式的行為。</span><span class="sxs-lookup"><span data-stu-id="0edb8-285">In addition, because of this rule, the switch sections of a `switch` statement can be arbitrarily rearranged without affecting the behavior of the statement.</span></span> <span data-ttu-id="0edb8-286">例如，區段`switch`可以反轉上述陳述式，而不會影響陳述式的行為：</span><span class="sxs-lookup"><span data-stu-id="0edb8-286">For example, the sections of the `switch` statement above can be reversed without affecting the behavior of the statement:</span></span>
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

<span data-ttu-id="0edb8-287">陳述式清單的參數區段通常結尾`break`， `goto case`，或`goto default`允許陳述式，但是呈現的陳述式清單的結束點無法連線到任何建構。</span><span class="sxs-lookup"><span data-stu-id="0edb8-287">The statement list of a switch section typically ends in a `break`, `goto case`, or `goto default` statement, but any construct that renders the end point of the statement list unreachable is permitted.</span></span> <span data-ttu-id="0edb8-288">例如，`while`控制的布林運算式的陳述式`true`已知永遠不會觸達其結束點。</span><span class="sxs-lookup"><span data-stu-id="0edb8-288">For example, a `while` statement controlled by the boolean expression `true` is known to never reach its end point.</span></span> <span data-ttu-id="0edb8-289">同樣地，`throw`或`return`陳述式會一律傳送其他位置的控制項，並永遠不會到達其結束點。</span><span class="sxs-lookup"><span data-stu-id="0edb8-289">Likewise, a `throw` or `return` statement always transfers control elsewhere and never reaches its end point.</span></span> <span data-ttu-id="0edb8-290">因此，下列範例是有效的：</span><span class="sxs-lookup"><span data-stu-id="0edb8-290">Thus, the following example is valid:</span></span>
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

<span data-ttu-id="0edb8-291">控管的型別`switch`陳述式可能是型別`string`。</span><span class="sxs-lookup"><span data-stu-id="0edb8-291">The governing type of a `switch` statement may be the type `string`.</span></span> <span data-ttu-id="0edb8-292">例如: </span><span class="sxs-lookup"><span data-stu-id="0edb8-292">For example:</span></span>
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

<span data-ttu-id="0edb8-293">像是字串等號比較運算子 ([字串等號比較運算子](expressions.md#string-equality-operators))，則`switch`陳述式會區分大小寫和參數的運算式字串完全相符時，才會執行指定的參數區段`case`標籤常數。</span><span class="sxs-lookup"><span data-stu-id="0edb8-293">Like the string equality operators ([String equality operators](expressions.md#string-equality-operators)), the `switch` statement is case sensitive and will execute a given switch section only if the switch expression string exactly matches a `case` label constant.</span></span>

<span data-ttu-id="0edb8-294">當控管的輸入`switch`陳述式`string`，值`null`允許做為 case 標籤常數。</span><span class="sxs-lookup"><span data-stu-id="0edb8-294">When the governing type of a `switch` statement is `string`, the value `null` is permitted as a case label constant.</span></span>

<span data-ttu-id="0edb8-295">*Statement_list*之*switch_block*可能包含宣告陳述式 ([宣告陳述式](statements.md#declaration-statements))。</span><span class="sxs-lookup"><span data-stu-id="0edb8-295">The *statement_list*s of a *switch_block* may contain declaration statements ([Declaration statements](statements.md#declaration-statements)).</span></span> <span data-ttu-id="0edb8-296">本機變數或常數的範圍中的 switch 區塊宣告是 switch 區塊。</span><span class="sxs-lookup"><span data-stu-id="0edb8-296">The scope of a local variable or constant declared in a switch block is the switch block.</span></span>

<span data-ttu-id="0edb8-297">指定的參數區段的陳述式清單是連線到如果`switch`陳述式是連線到，而且下列其中一項條件成立：</span><span class="sxs-lookup"><span data-stu-id="0edb8-297">The statement list of a given switch section is reachable if the `switch` statement is reachable and at least one of the following is true:</span></span>

*  <span data-ttu-id="0edb8-298">Switch 運算式是非常數的值。</span><span class="sxs-lookup"><span data-stu-id="0edb8-298">The switch expression is a non-constant value.</span></span>
*  <span data-ttu-id="0edb8-299">Switch 運算式會比對的常數值`case`參數區段中的標籤。</span><span class="sxs-lookup"><span data-stu-id="0edb8-299">The switch expression is a constant value that matches a `case` label in the switch section.</span></span>
*  <span data-ttu-id="0edb8-300">Switch 運算式是常數值不符合任何`case`標籤和 [參數] 區段包含`default`標籤。</span><span class="sxs-lookup"><span data-stu-id="0edb8-300">The switch expression is a constant value that doesn't match any `case` label, and the switch section contains the `default` label.</span></span>
*  <span data-ttu-id="0edb8-301">可存取參考的參數區段的參數標籤`goto case`或`goto default`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-301">A switch label of the switch section is referenced by a reachable `goto case` or `goto default` statement.</span></span>

<span data-ttu-id="0edb8-302">結束點`switch`陳述式是可連線，如果下列其中一項條件成立：</span><span class="sxs-lookup"><span data-stu-id="0edb8-302">The end point of a `switch` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="0edb8-303">`switch`陳述式包含可存取`break`陳述式，結束`switch`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-303">The `switch` statement contains a reachable `break` statement that exits the `switch` statement.</span></span>
*  <span data-ttu-id="0edb8-304">`switch`陳述式是可連線，switch 運算式是非常數的值，但不含任何`default`標籤已存在。</span><span class="sxs-lookup"><span data-stu-id="0edb8-304">The `switch` statement is reachable, the switch expression is a non-constant value, and no `default` label is present.</span></span>
*  <span data-ttu-id="0edb8-305">`switch`陳述式並連線到，switch 運算式不符合任何常數值`case`標籤，但不含任何`default`標籤已存在。</span><span class="sxs-lookup"><span data-stu-id="0edb8-305">The `switch` statement is reachable, the switch expression is a constant value that doesn't match any `case` label, and no `default` label is present.</span></span>

## <a name="iteration-statements"></a><span data-ttu-id="0edb8-306">反覆運算陳述式</span><span class="sxs-lookup"><span data-stu-id="0edb8-306">Iteration statements</span></span>

<span data-ttu-id="0edb8-307">反覆運算陳述式會重複執行內嵌陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-307">Iteration statements repeatedly execute an embedded statement.</span></span>

```antlr
iteration_statement
    : while_statement
    | do_statement
    | for_statement
    | foreach_statement
    ;
```

### <a name="the-while-statement"></a><span data-ttu-id="0edb8-308">While 陳述式</span><span class="sxs-lookup"><span data-stu-id="0edb8-308">The while statement</span></span>

<span data-ttu-id="0edb8-309">`while`陳述式有條件地執行內嵌陳述式零或多次。</span><span class="sxs-lookup"><span data-stu-id="0edb8-309">The `while` statement conditionally executes an embedded statement zero or more times.</span></span>

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

<span data-ttu-id="0edb8-310">A`while`陳述式，如下所示：</span><span class="sxs-lookup"><span data-stu-id="0edb8-310">A `while` statement is executed as follows:</span></span>

*  <span data-ttu-id="0edb8-311">*Boolean_expression* ([布林運算式](expressions.md#boolean-expressions)) 進行評估。</span><span class="sxs-lookup"><span data-stu-id="0edb8-311">The *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span>
*  <span data-ttu-id="0edb8-312">如果布林運算式會產生`true`，控制權會轉移到內嵌的陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-312">If the boolean expression yields `true`, control is transferred to the embedded statement.</span></span> <span data-ttu-id="0edb8-313">時，並控制到達內嵌的陳述式的結束點 (可能是從執行`continue`陳述式)，控制權會轉移到開頭`while`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-313">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), control is transferred to the beginning of the `while` statement.</span></span>
*  <span data-ttu-id="0edb8-314">如果布林運算式會產生`false`，控制權會轉移至結束點`while`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-314">If the boolean expression yields `false`, control is transferred to the end point of the `while` statement.</span></span>

<span data-ttu-id="0edb8-315">內嵌的陳述式內`while`陳述式中，`break`陳述式 ([break 陳述式](statements.md#the-break-statement)) 可用來將控制權移轉給結束點`while`（因而結束反覆項目內嵌的陳述式陳述式），以及`continue`陳述式 ([continue 陳述式](statements.md#the-continue-statement)) 可用來將控制權移轉給內嵌的陳述式的結束點 (藉此來執行的另一個反覆項目`while`陳述式)。</span><span class="sxs-lookup"><span data-stu-id="0edb8-315">Within the embedded statement of a `while` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `while` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement (thus performing another iteration of the `while` statement).</span></span>

<span data-ttu-id="0edb8-316">內嵌的陳述式的`while`陳述式是連線到如果`while`陳述式和布林值的運算式不是常數值`false`。</span><span class="sxs-lookup"><span data-stu-id="0edb8-316">The embedded statement of a `while` statement is reachable if the `while` statement is reachable and the boolean expression does not have the constant value `false`.</span></span>

<span data-ttu-id="0edb8-317">結束點`while`陳述式是可連線，如果下列其中一項條件成立：</span><span class="sxs-lookup"><span data-stu-id="0edb8-317">The end point of a `while` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="0edb8-318">`while`陳述式包含可存取`break`陳述式，結束`while`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-318">The `while` statement contains a reachable `break` statement that exits the `while` statement.</span></span>
*  <span data-ttu-id="0edb8-319">`while`陳述式和布林值的運算式不是常數值`true`。</span><span class="sxs-lookup"><span data-stu-id="0edb8-319">The `while` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-do-statement"></a><span data-ttu-id="0edb8-320">Do 陳述式</span><span class="sxs-lookup"><span data-stu-id="0edb8-320">The do statement</span></span>

<span data-ttu-id="0edb8-321">`do`陳述式有條件地執行內嵌陳述式一次以上。</span><span class="sxs-lookup"><span data-stu-id="0edb8-321">The `do` statement conditionally executes an embedded statement one or more times.</span></span>

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

<span data-ttu-id="0edb8-322">A`do`陳述式，如下所示：</span><span class="sxs-lookup"><span data-stu-id="0edb8-322">A `do` statement is executed as follows:</span></span>

*  <span data-ttu-id="0edb8-323">控制權會轉移到內嵌的陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-323">Control is transferred to the embedded statement.</span></span>
*  <span data-ttu-id="0edb8-324">時，並控制到達內嵌的陳述式的結束點 (可能是從執行`continue`陳述式)，則*boolean_expression* ([布林運算式](expressions.md#boolean-expressions)) 會評估。</span><span class="sxs-lookup"><span data-stu-id="0edb8-324">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), the *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span> <span data-ttu-id="0edb8-325">如果布林運算式會產生`true`，控制權會轉移到開頭`do`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-325">If the boolean expression yields `true`, control is transferred to the beginning of the `do` statement.</span></span> <span data-ttu-id="0edb8-326">否則，控制權會轉移到結束點`do`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-326">Otherwise, control is transferred to the end point of the `do` statement.</span></span>

<span data-ttu-id="0edb8-327">內嵌的陳述式內`do`陳述式中，`break`陳述式 ([break 陳述式](statements.md#the-break-statement)) 可用來將控制權移轉給結束點`do`（因而結束反覆項目內嵌的陳述式陳述式），以及`continue`陳述式 ([continue 陳述式](statements.md#the-continue-statement)) 可用來將控制權移轉給內嵌的陳述式的結束點。</span><span class="sxs-lookup"><span data-stu-id="0edb8-327">Within the embedded statement of a `do` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `do` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement.</span></span>

<span data-ttu-id="0edb8-328">內嵌的陳述式的`do`陳述式是連線到如果`do`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-328">The embedded statement of a `do` statement is reachable if the `do` statement is reachable.</span></span>

<span data-ttu-id="0edb8-329">結束點`do`陳述式是可連線，如果下列其中一項條件成立：</span><span class="sxs-lookup"><span data-stu-id="0edb8-329">The end point of a `do` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="0edb8-330">`do`陳述式包含可存取`break`陳述式，結束`do`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-330">The `do` statement contains a reachable `break` statement that exits the `do` statement.</span></span>
*  <span data-ttu-id="0edb8-331">內嵌的陳述式的結束點是連線和布林值的運算式不是常數值`true`。</span><span class="sxs-lookup"><span data-stu-id="0edb8-331">The end point of the embedded statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-for-statement"></a><span data-ttu-id="0edb8-332">陳述式</span><span class="sxs-lookup"><span data-stu-id="0edb8-332">The for statement</span></span>

<span data-ttu-id="0edb8-333">`for`陳述式會評估初始化運算式的順序，然後，條件為 true，重複執行內嵌陳述式，並評估一系列的反覆項目運算式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-333">The `for` statement evaluates a sequence of initialization expressions and then, while a condition is true, repeatedly executes an embedded statement and evaluates a sequence of iteration expressions.</span></span>

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

<span data-ttu-id="0edb8-334">*For_initializer*，如果有的話，包含*local_variable_declaration* ([區域變數宣告](statements.md#local-variable-declarations)) 或一份*statement_運算式*s ([運算式陳述式](statements.md#expression-statements)) 以逗號分隔。</span><span class="sxs-lookup"><span data-stu-id="0edb8-334">The *for_initializer*, if present, consists of either a *local_variable_declaration* ([Local variable declarations](statements.md#local-variable-declarations)) or a list of *statement_expression*s ([Expression statements](statements.md#expression-statements)) separated by commas.</span></span> <span data-ttu-id="0edb8-335">所宣告的本機變數的範圍*for_initializer*開始*local_variable_declarator*變數並延伸到內嵌的陳述式結尾。</span><span class="sxs-lookup"><span data-stu-id="0edb8-335">The scope of a local variable declared by a *for_initializer* starts at the *local_variable_declarator* for the variable and extends to the end of the embedded statement.</span></span> <span data-ttu-id="0edb8-336">其範圍包括*for_condition*並*for_iterator*。</span><span class="sxs-lookup"><span data-stu-id="0edb8-336">The scope includes the *for_condition* and the *for_iterator*.</span></span>

<span data-ttu-id="0edb8-337">*For_condition*，如果有的話，必須*boolean_expression* ([布林運算式](expressions.md#boolean-expressions))。</span><span class="sxs-lookup"><span data-stu-id="0edb8-337">The *for_condition*, if present, must be a *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)).</span></span>

<span data-ttu-id="0edb8-338">*For_iterator*，如果有的話，包含一份*statement_expression*s ([運算式陳述式](statements.md#expression-statements)) 以逗號分隔。</span><span class="sxs-lookup"><span data-stu-id="0edb8-338">The *for_iterator*, if present, consists of a list of *statement_expression*s ([Expression statements](statements.md#expression-statements)) separated by commas.</span></span>

<span data-ttu-id="0edb8-339">FOR 陳述式會執行，如下所示：</span><span class="sxs-lookup"><span data-stu-id="0edb8-339">A for statement is executed as follows:</span></span>

*  <span data-ttu-id="0edb8-340">如果*for_initializer*顯示出來，變數的初始設定式，或者將它們寫入的順序會執行陳述式的運算式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-340">If a *for_initializer* is present, the variable initializers or statement expressions are executed in the order they are written.</span></span> <span data-ttu-id="0edb8-341">此步驟只會執行一次。</span><span class="sxs-lookup"><span data-stu-id="0edb8-341">This step is only performed once.</span></span>
*  <span data-ttu-id="0edb8-342">如果*for_condition*已存在，它會進行評估。</span><span class="sxs-lookup"><span data-stu-id="0edb8-342">If a *for_condition* is present, it is evaluated.</span></span>
*  <span data-ttu-id="0edb8-343">如果*for_condition*不存在，或評估會產生`true`，控制權會轉移到內嵌的陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-343">If the *for_condition* is not present or if the evaluation yields `true`, control is transferred to the embedded statement.</span></span> <span data-ttu-id="0edb8-344">時，並控制到達內嵌的陳述式的結束點 (可能是從執行`continue`陳述式)，運算式*for_iterator*，如果任何項目，會評估的順序，而則是另一個反覆項目執行，開始評估*for_condition*上述步驟中。</span><span class="sxs-lookup"><span data-stu-id="0edb8-344">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), the expressions of the *for_iterator*, if any, are evaluated in sequence, and then another iteration is performed, starting with evaluation of the *for_condition* in the step above.</span></span>
*  <span data-ttu-id="0edb8-345">如果*for_condition*存在且評估會產生`false`，控制權會轉移至結束點`for`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-345">If the *for_condition* is present and the evaluation yields `false`, control is transferred to the end point of the `for` statement.</span></span>

<span data-ttu-id="0edb8-346">內嵌的陳述式內`for`陳述式中，`break`陳述式 ([break 陳述式](statements.md#the-break-statement)) 可用來將控制權移轉給結束點`for`（因而結束反覆項目內嵌的陳述式陳述式），以及`continue`陳述式 ([continue 陳述式](statements.md#the-continue-statement)) 可用來將控制權移轉給內嵌的陳述式的結束點 (因此執行*for_iterator*和執行另一個反覆項目`for`陳述式，從開始*for_condition*)。</span><span class="sxs-lookup"><span data-stu-id="0edb8-346">Within the embedded statement of a `for` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `for` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement (thus executing the *for_iterator* and performing another iteration of the `for` statement, starting with the *for_condition*).</span></span>

<span data-ttu-id="0edb8-347">內嵌的陳述式的`for`陳述式是可連線，如果下列其中一項條件成立：</span><span class="sxs-lookup"><span data-stu-id="0edb8-347">The embedded statement of a `for` statement is reachable if one of the following is true:</span></span>

*  <span data-ttu-id="0edb8-348">`for`陳述式和 no *for_condition*存在。</span><span class="sxs-lookup"><span data-stu-id="0edb8-348">The `for` statement is reachable and no *for_condition* is present.</span></span>
*  <span data-ttu-id="0edb8-349">`for`陳述式是連線到與*for_condition*存在且不需要的常數值`false`。</span><span class="sxs-lookup"><span data-stu-id="0edb8-349">The `for` statement is reachable and a *for_condition* is present and does not have the constant value `false`.</span></span>

<span data-ttu-id="0edb8-350">結束點`for`陳述式是可連線，如果下列其中一項條件成立：</span><span class="sxs-lookup"><span data-stu-id="0edb8-350">The end point of a `for` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="0edb8-351">`for`陳述式包含可存取`break`陳述式，結束`for`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-351">The `for` statement contains a reachable `break` statement that exits the `for` statement.</span></span>
*  <span data-ttu-id="0edb8-352">`for`陳述式是連線到與*for_condition*存在且不需要的常數值`true`。</span><span class="sxs-lookup"><span data-stu-id="0edb8-352">The `for` statement is reachable and a *for_condition* is present and does not have the constant value `true`.</span></span>

### <a name="the-foreach-statement"></a><span data-ttu-id="0edb8-353">Foreach 陳述式</span><span class="sxs-lookup"><span data-stu-id="0edb8-353">The foreach statement</span></span>

<span data-ttu-id="0edb8-354">`foreach`陳述式會列舉執行內嵌陳述式的集合中的每個項目集合中的項目。</span><span class="sxs-lookup"><span data-stu-id="0edb8-354">The `foreach` statement enumerates the elements of a collection, executing an embedded statement for each element of the collection.</span></span>

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

<span data-ttu-id="0edb8-355">*型別*並*識別項*的`foreach`陳述式宣告***反覆運算變數***陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-355">The *type* and *identifier* of a `foreach` statement declare the ***iteration variable*** of the statement.</span></span> <span data-ttu-id="0edb8-356">如果`var`識別項指定為*local_variable_type*，且沒有名為的型別`var`是在範圍內，反覆運算變數即為***隱含型別的反覆運算變數***，其類型會視為的項目類型和`foreach`陳述式中的，依下列指定。</span><span class="sxs-lookup"><span data-stu-id="0edb8-356">If the `var` identifier is given as the *local_variable_type*, and no type named `var` is in scope, the iteration variable is said to be an ***implicitly typed iteration variable***, and its type is taken to be the element type of the `foreach` statement, as specified below.</span></span> <span data-ttu-id="0edb8-357">反覆運算變數對應到唯讀區域變數，包括內嵌的陳述式的範圍。</span><span class="sxs-lookup"><span data-stu-id="0edb8-357">The iteration variable corresponds to a read-only local variable with a scope that extends over the embedded statement.</span></span> <span data-ttu-id="0edb8-358">在執行期間`foreach`陳述式中，反覆運算變數代表目前正在執行反覆項目集合項目。</span><span class="sxs-lookup"><span data-stu-id="0edb8-358">During execution of a `foreach` statement, the iteration variable represents the collection element for which an iteration is currently being performed.</span></span> <span data-ttu-id="0edb8-359">如果內嵌的陳述式嘗試修改反覆項目變數，就會發生編譯時期錯誤 (透過指派或`++`並`--`運算子)，或傳遞的反覆項目變數`ref`或`out`參數。</span><span class="sxs-lookup"><span data-stu-id="0edb8-359">A compile-time error occurs if the embedded statement attempts to modify the iteration variable (via assignment or the `++` and `--` operators) or pass the iteration variable as a `ref` or `out` parameter.</span></span>

<span data-ttu-id="0edb8-360">在下列命令，為求簡潔， `IEnumerable`， `IEnumerator`，`IEnumerable<T>`並`IEnumerator<T>`命名空間中的對應類型是指`System.Collections`和`System.Collections.Generic`。</span><span class="sxs-lookup"><span data-stu-id="0edb8-360">In the following, for brevity, `IEnumerable`, `IEnumerator`, `IEnumerable<T>` and `IEnumerator<T>` refer to the corresponding types in the namespaces `System.Collections` and `System.Collections.Generic`.</span></span>

<span data-ttu-id="0edb8-361">編譯時間處理 foreach 陳述式會先判斷***集合型別***，***列舉值型別***並***項目型別***運算式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-361">The compile-time processing of a foreach statement first determines the ***collection type***, ***enumerator type*** and ***element type*** of the expression.</span></span> <span data-ttu-id="0edb8-362">這項判斷程序進行，如下所示：</span><span class="sxs-lookup"><span data-stu-id="0edb8-362">This determination proceeds as follows:</span></span>

*  <span data-ttu-id="0edb8-363">如果型別`X`的*運算式*是陣列型別，則會從隱含參考轉換`X`來`IEnumerable`介面 (因為`System.Array`實作這個介面)。</span><span class="sxs-lookup"><span data-stu-id="0edb8-363">If the type `X` of *expression* is an array type then there is an implicit reference conversion from `X` to the `IEnumerable` interface (since `System.Array` implements this interface).</span></span> <span data-ttu-id="0edb8-364">***集合型別***是`IEnumerable`介面***列舉值型別***是`IEnumerator`介面和***項目型別***的項目類型陣列類型`X`。</span><span class="sxs-lookup"><span data-stu-id="0edb8-364">The ***collection type*** is the `IEnumerable` interface, the ***enumerator type*** is the `IEnumerator` interface and the ***element type*** is the element type of the array type `X`.</span></span>
*  <span data-ttu-id="0edb8-365">如果型別`X`的*運算式*是`dynamic`的隱含轉換就*運算式*至`IEnumerable`介面 ([隱含的動態轉換](conversions.md#implicit-dynamic-conversions))。</span><span class="sxs-lookup"><span data-stu-id="0edb8-365">If the type `X` of *expression* is `dynamic` then there is an implicit conversion from *expression* to the `IEnumerable` interface ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions)).</span></span> <span data-ttu-id="0edb8-366">***集合型別***是`IEnumerable`介面並***列舉值型別***是`IEnumerator`介面。</span><span class="sxs-lookup"><span data-stu-id="0edb8-366">The ***collection type*** is the `IEnumerable` interface and the ***enumerator type*** is the `IEnumerator` interface.</span></span> <span data-ttu-id="0edb8-367">如果`var`識別項指定為*local_variable_type*則***項目型別***是`dynamic`，否則就是`object`。</span><span class="sxs-lookup"><span data-stu-id="0edb8-367">If the `var` identifier is given as the *local_variable_type* then the ***element type*** is `dynamic`, otherwise it is `object`.</span></span>
*  <span data-ttu-id="0edb8-368">否則，請判斷是否型別`X`具有適當`GetEnumerator`方法：</span><span class="sxs-lookup"><span data-stu-id="0edb8-368">Otherwise, determine whether the type `X` has an appropriate `GetEnumerator` method:</span></span>
   * <span data-ttu-id="0edb8-369">對類型的成員查閱`X`識別碼`GetEnumerator`不使用類型引數。</span><span class="sxs-lookup"><span data-stu-id="0edb8-369">Perform member lookup on the type `X` with identifier `GetEnumerator` and no type arguments.</span></span> <span data-ttu-id="0edb8-370">如果成員查詢不會產生相符項目，會產生模稜兩可，或會產生相符項目不是方法群組，請檢查可列舉的介面，如下所述。</span><span class="sxs-lookup"><span data-stu-id="0edb8-370">If the member lookup does not produce a match, or it produces an ambiguity, or produces a match that is not a method group, check for an enumerable interface as described below.</span></span> <span data-ttu-id="0edb8-371">如果成員查詢產生的任何項目以外的方法群組或沒有相符項目，就會發出警告建議。</span><span class="sxs-lookup"><span data-stu-id="0edb8-371">It is recommended that a warning be issued if member lookup produces anything except a method group or no match.</span></span>
   * <span data-ttu-id="0edb8-372">執行使用產生的方法群組和空白的引數清單的多載解析。</span><span class="sxs-lookup"><span data-stu-id="0edb8-372">Perform overload resolution using the resulting method group and an empty argument list.</span></span> <span data-ttu-id="0edb8-373">如果多載解析會導致沒有適用的方法，導致模稜兩可，或產生單一的最佳方法，但是該方法是靜態或並非公用，檢查可列舉的介面，如下所述。</span><span class="sxs-lookup"><span data-stu-id="0edb8-373">If overload resolution results in no applicable methods, results in an ambiguity, or results in a single best method but that method is either static or not public, check for an enumerable interface as described below.</span></span> <span data-ttu-id="0edb8-374">如果多載解析產生的任何項目以外的模稜兩可的公用執行個體方法或沒有適用的方法，就會發出警告建議。</span><span class="sxs-lookup"><span data-stu-id="0edb8-374">It is recommended that a warning be issued if overload resolution produces anything except an unambiguous public instance method or no applicable methods.</span></span>
   * <span data-ttu-id="0edb8-375">如果傳回型別`E`的`GetEnumerator`方法不是類別、 結構或介面類型時，錯誤會產生並採取任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="0edb8-375">If the return type `E` of the `GetEnumerator` method is not a class, struct or interface type, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="0edb8-376">成員查詢會針對`E`具有識別碼`Current`不使用類型引數。</span><span class="sxs-lookup"><span data-stu-id="0edb8-376">Member lookup is performed on `E` with the identifier `Current` and no type arguments.</span></span> <span data-ttu-id="0edb8-377">如果成員查詢產生沒有相符項目，就會產生錯誤，或結果不是 已允許讀取的公用執行個體屬性，會產生錯誤並採取任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="0edb8-377">If the member lookup produces no match, the result is an error, or the result is anything except a public instance property that permits reading, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="0edb8-378">成員查詢會針對`E`具有識別碼`MoveNext`不使用類型引數。</span><span class="sxs-lookup"><span data-stu-id="0edb8-378">Member lookup is performed on `E` with the identifier `MoveNext` and no type arguments.</span></span> <span data-ttu-id="0edb8-379">如果成員查詢未產生符合項，發生錯誤，或結果方法群組以外的任何項目，就會發生錯誤，也會採取任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="0edb8-379">If the member lookup produces no match, the result is an error, or the result is anything except a method group, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="0edb8-380">多載解析會對空的引數清單的方法群組。</span><span class="sxs-lookup"><span data-stu-id="0edb8-380">Overload resolution is performed on the method group with an empty argument list.</span></span> <span data-ttu-id="0edb8-381">如果沒有適用的方法，導致模稜兩可或在單一的最佳方法，但該方法的結果中的多載解析結果是靜態或並非公用，或其傳回型別不是`bool`，會產生錯誤並採取任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="0edb8-381">If overload resolution results in no applicable methods, results in an ambiguity, or results in a single best method but that method is either static or not public, or its return type is not `bool`, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="0edb8-382">***集合型別***是`X`，則***列舉值型別***是`E`，和***項目型別***是種`Current`屬性。</span><span class="sxs-lookup"><span data-stu-id="0edb8-382">The ***collection type*** is `X`, the ***enumerator type*** is `E`, and the ***element type*** is the type of the `Current` property.</span></span>

*  <span data-ttu-id="0edb8-383">否則，請檢查可列舉的介面：</span><span class="sxs-lookup"><span data-stu-id="0edb8-383">Otherwise, check for an enumerable interface:</span></span>
   * <span data-ttu-id="0edb8-384">所有類型，如果`Ti`包括不的隱含轉換`X`要`IEnumerable<Ti>`，沒有唯一的型別`T`使得`T`不是`dynamic`和其他所有`Ti`沒有隱含轉換`IEnumerable<T>`來`IEnumerable<Ti>`，則***集合型別***介面`IEnumerable<T>`，則***列舉值類型***介面`IEnumerator<T>`，而***項目型別***是`T`。</span><span class="sxs-lookup"><span data-stu-id="0edb8-384">If among all the types `Ti` for which there is an implicit conversion from `X` to `IEnumerable<Ti>`, there is a unique type `T` such that `T` is not `dynamic` and for all the other `Ti` there is an implicit conversion from `IEnumerable<T>` to `IEnumerable<Ti>`, then the ***collection type*** is the interface `IEnumerable<T>`, the ***enumerator type*** is the interface `IEnumerator<T>`, and the ***element type*** is `T`.</span></span>
   * <span data-ttu-id="0edb8-385">否則，如果有多個這類的型別`T`，然後會產生錯誤並採取任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="0edb8-385">Otherwise, if there is more than one such type `T`, then an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="0edb8-386">否則，如果沒有隱含轉換`X`要`System.Collections.IEnumerable`介面，則***集合型別***是這個介面，***列舉值類型***介面`System.Collections.IEnumerator`，而***項目型別***是`object`。</span><span class="sxs-lookup"><span data-stu-id="0edb8-386">Otherwise, if there is an implicit conversion from `X` to the `System.Collections.IEnumerable` interface, then the ***collection type*** is this interface, the ***enumerator type*** is the interface `System.Collections.IEnumerator`, and the ***element type*** is `object`.</span></span>
   * <span data-ttu-id="0edb8-387">否則，會產生錯誤並採取任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="0edb8-387">Otherwise, an error is produced and no further steps are taken.</span></span>

<span data-ttu-id="0edb8-388">上述步驟中，如果成功，就會明確產生集合型別`C`，列舉值型別`E`和 項目型別`T`。</span><span class="sxs-lookup"><span data-stu-id="0edb8-388">The above steps, if successful, unambiguously produce a collection type `C`, enumerator type `E` and element type `T`.</span></span> <span data-ttu-id="0edb8-389">Foreach 陳述式的格式</span><span class="sxs-lookup"><span data-stu-id="0edb8-389">A foreach statement of the form</span></span>
```csharp
foreach (V v in x) embedded_statement
```
<span data-ttu-id="0edb8-390">會展開為：</span><span class="sxs-lookup"><span data-stu-id="0edb8-390">is then expanded to:</span></span>
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

<span data-ttu-id="0edb8-391">變數`e`看見或存取運算式不是`x`或內嵌的陳述式或程式的任何其他原始程式碼。</span><span class="sxs-lookup"><span data-stu-id="0edb8-391">The variable `e` is not visible to or accessible to the expression `x` or the embedded statement or any other source code of the program.</span></span> <span data-ttu-id="0edb8-392">變數`v`是唯讀，在內嵌的陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-392">The variable `v` is read-only in the embedded statement.</span></span> <span data-ttu-id="0edb8-393">如果沒有明確的轉換 ([明確轉換](conversions.md#explicit-conversions)) 從`T`（項目類型） 來`V`( *local_variable_type* foreach 陳述式中)，會產生錯誤也採取任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="0edb8-393">If there is not an explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) from `T` (the element type) to `V` (the *local_variable_type* in the foreach statement), an error is produced and no further steps are taken.</span></span> <span data-ttu-id="0edb8-394">如果`x`具有值`null`、`System.NullReferenceException`會在執行階段擲回。</span><span class="sxs-lookup"><span data-stu-id="0edb8-394">If `x` has the value `null`, a `System.NullReferenceException` is thrown at run-time.</span></span>

<span data-ttu-id="0edb8-395">實作允許以不同的方式實作指定的 foreach 陳述式，基於效能考量，例如，只要是與上述延伸一致的行為。</span><span class="sxs-lookup"><span data-stu-id="0edb8-395">An implementation is permitted to implement a given foreach-statement differently, e.g. for performance reasons, as long as the behavior is consistent with the above expansion.</span></span>

<span data-ttu-id="0edb8-396">放置`v`內 while 迴圈是很重要的擷取所發生的任何匿名函式的方式*embedded_statement*。</span><span class="sxs-lookup"><span data-stu-id="0edb8-396">The placement of `v` inside the while loop is important for how it is captured by any anonymous function occurring in the *embedded_statement*.</span></span>

<span data-ttu-id="0edb8-397">例如: </span><span class="sxs-lookup"><span data-stu-id="0edb8-397">For example:</span></span>
```csharp
int[] values = { 7, 9, 13 };
Action f = null;

foreach (var value in values)
{
    if (f == null) f = () => Console.WriteLine("First value: " + value);
}

f();
```
<span data-ttu-id="0edb8-398">如果`v`deklarovalo 之外 while 迴圈中，它會共用所有的反覆項目，以及它的值之後迴圈會是最後的值， `13`，這是什麼引動過程的`f`會列印。</span><span class="sxs-lookup"><span data-stu-id="0edb8-398">If `v` was declared outside of the while loop, it would be shared among all iterations, and its value after the for loop would be the final value, `13`, which is what the invocation of `f` would print.</span></span> <span data-ttu-id="0edb8-399">相反地，因為每個反覆項目有它自己的變數`v`，其中一個擷取`f`第一次反覆運算都會繼續保存值`7`，也就是會列印。</span><span class="sxs-lookup"><span data-stu-id="0edb8-399">Instead, because each iteration has its own variable `v`, the one captured by `f` in the first iteration will continue to hold the value `7`, which is what will be printed.</span></span> <span data-ttu-id="0edb8-400">(注意： 舊版 C# 宣告`v`外部的 while 迴圈。)</span><span class="sxs-lookup"><span data-stu-id="0edb8-400">(Note: earlier versions of C# declared `v` outside of the while loop.)</span></span>

<span data-ttu-id="0edb8-401">本文最後的區塊建構根據下列步驟：</span><span class="sxs-lookup"><span data-stu-id="0edb8-401">The body of the finally block is constructed according to the following steps:</span></span>

*  <span data-ttu-id="0edb8-402">如果沒有隱含轉換`E`至`System.IDisposable`介面，然後</span><span class="sxs-lookup"><span data-stu-id="0edb8-402">If there is an implicit conversion from `E` to the `System.IDisposable` interface, then</span></span>
   *  <span data-ttu-id="0edb8-403">如果`E`不可為 null 的實值型別則 finally 子句會展開以語意相等的：</span><span class="sxs-lookup"><span data-stu-id="0edb8-403">If `E` is a non-nullable value type then the finally clause is expanded to the semantic equivalent  of:</span></span>

      ```csharp
      finally {
          ((System.IDisposable)e).Dispose();
      }
      ```

   *  <span data-ttu-id="0edb8-404">否則 finally 子句會展開以語意相等的：</span><span class="sxs-lookup"><span data-stu-id="0edb8-404">Otherwise the finally clause is expanded to the semantic equivalent of:</span></span>

      ```csharp
      finally {
          if (e != null) ((System.IDisposable)e).Dispose();
      }
      ```

   <span data-ttu-id="0edb8-405">差異在於，如果`E`是實值型別或實值類型，然後轉型的具現化的型別參數`e`到`System.IDisposable`不會發生 boxing。</span><span class="sxs-lookup"><span data-stu-id="0edb8-405">except that if `E` is a value type, or a type parameter instantiated to a value type, then the cast of `e` to `System.IDisposable` will not cause boxing to occur.</span></span>

*  <span data-ttu-id="0edb8-406">否則，如果`E`是密封型別，finally 子句會展開為空白區塊：</span><span class="sxs-lookup"><span data-stu-id="0edb8-406">Otherwise, if `E` is a sealed type, the finally clause is expanded to an empty block:</span></span>

   ```csharp
   finally {
   }
   ```

*  <span data-ttu-id="0edb8-407">否則，最後的子句會展開以：</span><span class="sxs-lookup"><span data-stu-id="0edb8-407">Otherwise, the finally clause is expanded to:</span></span>

   ```csharp
   finally {
       System.IDisposable d = e as System.IDisposable;
       if (d != null) d.Dispose();
   }
   ```    

   <span data-ttu-id="0edb8-408">區域變數`d`不可見或存取的任何使用者程式碼。</span><span class="sxs-lookup"><span data-stu-id="0edb8-408">The local variable `d` is not visible to or accessible to any user code.</span></span> <span data-ttu-id="0edb8-409">特別是，它不會衝突與任何其他變數其範圍，包括 finally 區塊。</span><span class="sxs-lookup"><span data-stu-id="0edb8-409">In particular, it does not conflict with any other variable whose scope includes the finally block.</span></span>

<span data-ttu-id="0edb8-410">順序`foreach`周遊陣列的元素，如下所示： 的一維陣列項目會以遞增索引順序周遊，從索引`0`和結束索引`Length - 1`。</span><span class="sxs-lookup"><span data-stu-id="0edb8-410">The order in which `foreach` traverses the elements of an array, is as follows: For single-dimensional arrays elements are traversed in increasing index order, starting with index `0` and ending with index `Length - 1`.</span></span> <span data-ttu-id="0edb8-411">多維陣列，最右邊的維度的索引會開始遞增，則下一步 的左的維度，依此類推到左邊，就會周遊一個項目。</span><span class="sxs-lookup"><span data-stu-id="0edb8-411">For multi-dimensional arrays, elements are traversed such that the indices of the rightmost dimension are increased first, then the next left dimension, and so on to the left.</span></span>

<span data-ttu-id="0edb8-412">下列範例會列印出在二維陣列中，項目順序中的每個值：</span><span class="sxs-lookup"><span data-stu-id="0edb8-412">The following example prints out each value in a two-dimensional array, in element order:</span></span>
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
<span data-ttu-id="0edb8-413">產生的輸出如下所示：</span><span class="sxs-lookup"><span data-stu-id="0edb8-413">The output produced is as follows:</span></span>
```csharp
1.2 2.3 3.4 4.5 5.6 6.7 7.8 8.9
```

<span data-ttu-id="0edb8-414">在範例</span><span class="sxs-lookup"><span data-stu-id="0edb8-414">In the example</span></span>
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
<span data-ttu-id="0edb8-415">型別`n`就會推斷`int`的項目類型`numbers`。</span><span class="sxs-lookup"><span data-stu-id="0edb8-415">the type of `n` is inferred to be `int`, the element type of `numbers`.</span></span>

## <a name="jump-statements"></a><span data-ttu-id="0edb8-416">跳躍陳述式</span><span class="sxs-lookup"><span data-stu-id="0edb8-416">Jump statements</span></span>

<span data-ttu-id="0edb8-417">跳躍陳述式無條件地將控制權轉移。</span><span class="sxs-lookup"><span data-stu-id="0edb8-417">Jump statements unconditionally transfer control.</span></span>

```antlr
jump_statement
    : break_statement
    | continue_statement
    | goto_statement
    | return_statement
    | throw_statement
    ;
```

<span data-ttu-id="0edb8-418">跳躍陳述式將控制權傳輸至位置稱為***目標***跳躍陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-418">The location to which a jump statement transfers control is called the ***target*** of the jump statement.</span></span>

<span data-ttu-id="0edb8-419">當跳躍陳述式，就會發生在區塊內，該跳躍陳述式的目標是在區塊之外，即稱為跳躍陳述式***結束***區塊。</span><span class="sxs-lookup"><span data-stu-id="0edb8-419">When a jump statement occurs within a block, and the target of that jump statement is outside that block, the jump statement is said to ***exit*** the block.</span></span> <span data-ttu-id="0edb8-420">跳躍陳述式可能會傳送出區塊的控制項，而它永遠不會將控制權轉移到區塊。</span><span class="sxs-lookup"><span data-stu-id="0edb8-420">While a jump statement may transfer control out of a block, it can never transfer control into a block.</span></span>

<span data-ttu-id="0edb8-421">跳躍陳述式的執行複雜的中介`try`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-421">Execution of jump statements is complicated by the presence of intervening `try` statements.</span></span> <span data-ttu-id="0edb8-422">如果沒有這類`try`陳述式，跳躍陳述式無條件地將控制權從跳躍陳述式到其目標。</span><span class="sxs-lookup"><span data-stu-id="0edb8-422">In the absence of such `try` statements, a jump statement unconditionally transfers control from the jump statement to its target.</span></span> <span data-ttu-id="0edb8-423">如果存在這類的中介`try`陳述式中，執行將會更複雜。</span><span class="sxs-lookup"><span data-stu-id="0edb8-423">In the presence of such intervening `try` statements, execution is more complex.</span></span> <span data-ttu-id="0edb8-424">如果跳躍陳述式結束一個或多個`try`區塊相關聯`finally`區塊，控制最初交給`finally`最內層區塊`try`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-424">If the jump statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="0edb8-425">時，並控制到達結束點`finally`區塊中，控制傳輸至`finally`區塊下一步 的封入`try`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-425">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="0edb8-426">此程序會重複直到`finally`的所有區塊中介`try`陳述式執行。</span><span class="sxs-lookup"><span data-stu-id="0edb8-426">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>

<span data-ttu-id="0edb8-427">在範例</span><span class="sxs-lookup"><span data-stu-id="0edb8-427">In the example</span></span>
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
<span data-ttu-id="0edb8-428">`finally`兩個相關聯的區塊`try`控制權會轉移到跳躍陳述式的目標之前，會執行陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-428">the `finally` blocks associated with two `try` statements are executed before control is transferred to the target of the jump statement.</span></span>

<span data-ttu-id="0edb8-429">產生的輸出如下所示：</span><span class="sxs-lookup"><span data-stu-id="0edb8-429">The output produced is as follows:</span></span>
```
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a><span data-ttu-id="0edb8-430">Break 陳述式</span><span class="sxs-lookup"><span data-stu-id="0edb8-430">The break statement</span></span>

<span data-ttu-id="0edb8-431">`break`陳述式會結束最內層`switch`， `while`， `do`， `for`，或`foreach`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-431">The `break` statement exits the nearest enclosing `switch`, `while`, `do`, `for`, or `foreach` statement.</span></span>

```antlr
break_statement
    : 'break' ';'
    ;
```

<span data-ttu-id="0edb8-432">目標`break`陳述式會結束點的最接近的封閉式`switch`， `while`， `do`， `for`，或`foreach`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-432">The target of a `break` statement is the end point of the nearest enclosing `switch`, `while`, `do`, `for`, or `foreach` statement.</span></span> <span data-ttu-id="0edb8-433">如果`break`陳述式不加`switch`， `while`， `do`， `for`，或`foreach`陳述式，會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="0edb8-433">If a `break` statement is not enclosed by a `switch`, `while`, `do`, `for`, or `foreach` statement, a compile-time error occurs.</span></span>

<span data-ttu-id="0edb8-434">當多個`switch`， `while`， `do`， `for`，或`foreach`陳述式巢狀置於彼此`break`陳述式僅適用於最內層的陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-434">When multiple `switch`, `while`, `do`, `for`, or `foreach` statements are nested within each other, a `break` statement applies only to the innermost statement.</span></span> <span data-ttu-id="0edb8-435">跨多個巢狀層級，將控制項傳遞至`goto`陳述式 ([goto 陳述式](statements.md#the-goto-statement)) 必須使用。</span><span class="sxs-lookup"><span data-stu-id="0edb8-435">To transfer control across multiple nesting levels, a `goto` statement ([The goto statement](statements.md#the-goto-statement)) must be used.</span></span>

<span data-ttu-id="0edb8-436">A`break`陳述式無法結束`finally`區塊 ([try 陳述式](statements.md#the-try-statement))。</span><span class="sxs-lookup"><span data-stu-id="0edb8-436">A `break` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="0edb8-437">當`break`陳述式內，就會發生`finally`封鎖的目標`break`陳述式必須在相同`finally`封鎖; 否則會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="0edb8-437">When a `break` statement occurs within a `finally` block, the target of the `break` statement must be within the same `finally` block; otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="0edb8-438">A`break`陳述式，如下所示：</span><span class="sxs-lookup"><span data-stu-id="0edb8-438">A `break` statement is executed as follows:</span></span>

*  <span data-ttu-id="0edb8-439">如果`break`陳述式會結束一個或多個`try`區塊相關聯`finally`區塊，控制項一開始會傳送到`finally`的最內層區塊`try`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-439">If the `break` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="0edb8-440">時，並控制到達結束點`finally`區塊中，控制傳輸至`finally`區塊下一步 的封入`try`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-440">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="0edb8-441">此程序會重複直到`finally`的所有區塊中介`try`陳述式執行。</span><span class="sxs-lookup"><span data-stu-id="0edb8-441">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="0edb8-442">控制權會轉移到目標的`break`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-442">Control is transferred to the target of the `break` statement.</span></span>

<span data-ttu-id="0edb8-443">因為`break`陳述式無條件地將控制權傳輸其他位置、 結束點`break`陳述式絕不會是可連線。</span><span class="sxs-lookup"><span data-stu-id="0edb8-443">Because a `break` statement unconditionally transfers control elsewhere, the end point of a `break` statement is never reachable.</span></span>

### <a name="the-continue-statement"></a><span data-ttu-id="0edb8-444">Continue 陳述式</span><span class="sxs-lookup"><span data-stu-id="0edb8-444">The continue statement</span></span>

<span data-ttu-id="0edb8-445">`continue`陳述式開始執行新的反覆查看的最接近的封閉式`while`， `do`， `for`，或`foreach`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-445">The `continue` statement starts a new iteration of the nearest enclosing `while`, `do`, `for`, or `foreach` statement.</span></span>

```antlr
continue_statement
    : 'continue' ';'
    ;
```

<span data-ttu-id="0edb8-446">目標`continue`陳述式是內嵌的陳述式的最接近的封閉式終點`while`， `do`， `for`，或`foreach`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-446">The target of a `continue` statement is the end point of the embedded statement of the nearest enclosing `while`, `do`, `for`, or `foreach` statement.</span></span> <span data-ttu-id="0edb8-447">如果`continue`陳述式不加`while`， `do`， `for`，或`foreach`陳述式，會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="0edb8-447">If a `continue` statement is not enclosed by a `while`, `do`, `for`, or `foreach` statement, a compile-time error occurs.</span></span>

<span data-ttu-id="0edb8-448">當多個`while`， `do`， `for`，或`foreach`陳述式巢狀置於彼此`continue`陳述式僅適用於最內層的陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-448">When multiple `while`, `do`, `for`, or `foreach` statements are nested within each other, a `continue` statement applies only to the innermost statement.</span></span> <span data-ttu-id="0edb8-449">跨多個巢狀層級，將控制項傳遞至`goto`陳述式 ([goto 陳述式](statements.md#the-goto-statement)) 必須使用。</span><span class="sxs-lookup"><span data-stu-id="0edb8-449">To transfer control across multiple nesting levels, a `goto` statement ([The goto statement](statements.md#the-goto-statement)) must be used.</span></span>

<span data-ttu-id="0edb8-450">A`continue`陳述式無法結束`finally`區塊 ([try 陳述式](statements.md#the-try-statement))。</span><span class="sxs-lookup"><span data-stu-id="0edb8-450">A `continue` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="0edb8-451">當`continue`陳述式內，就會發生`finally`封鎖的目標`continue`陳述式必須在相同`finally`封鎖; 否則會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="0edb8-451">When a `continue` statement occurs within a `finally` block, the target of the `continue` statement must be within the same `finally` block; otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="0edb8-452">A`continue`陳述式，如下所示：</span><span class="sxs-lookup"><span data-stu-id="0edb8-452">A `continue` statement is executed as follows:</span></span>

*  <span data-ttu-id="0edb8-453">如果`continue`陳述式會結束一個或多個`try`區塊相關聯`finally`區塊，控制項一開始會傳送到`finally`的最內層區塊`try`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-453">If the `continue` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="0edb8-454">時，並控制到達結束點`finally`區塊中，控制傳輸至`finally`區塊下一步 的封入`try`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-454">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="0edb8-455">此程序會重複直到`finally`的所有區塊中介`try`陳述式執行。</span><span class="sxs-lookup"><span data-stu-id="0edb8-455">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="0edb8-456">控制權會轉移到目標的`continue`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-456">Control is transferred to the target of the `continue` statement.</span></span>

<span data-ttu-id="0edb8-457">因為`continue`陳述式無條件地將控制權傳輸其他位置、 結束點`continue`陳述式絕不會是可連線。</span><span class="sxs-lookup"><span data-stu-id="0edb8-457">Because a `continue` statement unconditionally transfers control elsewhere, the end point of a `continue` statement is never reachable.</span></span>

### <a name="the-goto-statement"></a><span data-ttu-id="0edb8-458">goto 陳述式</span><span class="sxs-lookup"><span data-stu-id="0edb8-458">The goto statement</span></span>

<span data-ttu-id="0edb8-459">`goto`陳述式將控制權傳輸至標籤來標記的陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-459">The `goto` statement transfers control to a statement that is marked by a label.</span></span>

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

<span data-ttu-id="0edb8-460">目標`goto`*識別碼*陳述式是以指定標記的標記陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-460">The target of a `goto` *identifier* statement is the labeled statement with the given label.</span></span> <span data-ttu-id="0edb8-461">如果不存在具有指定名稱的標籤，這是在目前的函式成員，或如果`goto`陳述式不是範圍內的標籤，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="0edb8-461">If a label with the given name does not exist in the current function member, or if the `goto` statement is not within the scope of the label, a compile-time error occurs.</span></span> <span data-ttu-id="0edb8-462">此規則允許使用`goto`陳述式來轉移控制項巢狀範圍，但不用到巢狀範圍。</span><span class="sxs-lookup"><span data-stu-id="0edb8-462">This rule permits the use of a `goto` statement to transfer control out of a nested scope, but not into a nested scope.</span></span> <span data-ttu-id="0edb8-463">在範例</span><span class="sxs-lookup"><span data-stu-id="0edb8-463">In the example</span></span>
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
<span data-ttu-id="0edb8-464">`goto`陳述式用來傳送超出巢狀範圍的控制項。</span><span class="sxs-lookup"><span data-stu-id="0edb8-464">a `goto` statement is used to transfer control out of a nested scope.</span></span>

<span data-ttu-id="0edb8-465">目標`goto case`陳述式是在直接封入陳述式清單`switch`陳述式 ([switch 陳述式](statements.md#the-switch-statement))，其中包含`case`使用指定的常數值的標籤。</span><span class="sxs-lookup"><span data-stu-id="0edb8-465">The target of a `goto case` statement is the statement list in the immediately enclosing `switch` statement ([The switch statement](statements.md#the-switch-statement)), which contains a `case` label with the given constant value.</span></span> <span data-ttu-id="0edb8-466">如果`goto case`陳述式不加`switch`陳述式中，如果*constant_expression*並未隱含表示可轉換 ([隱含轉換](conversions.md#implicit-conversions)) 控管的型別最接近的封閉式`switch`陳述式，或如果最接近的封閉式`switch`陳述式不包含`case`使用指定的常數值，標籤就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="0edb8-466">If the `goto case` statement is not enclosed by a `switch` statement, if the *constant_expression* is not implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the governing type of the nearest enclosing `switch` statement, or if the nearest enclosing `switch` statement does not contain a `case` label with the given constant value, a compile-time error occurs.</span></span>

<span data-ttu-id="0edb8-467">目標`goto default`陳述式是在直接封入陳述式清單`switch`陳述式 ([switch 陳述式](statements.md#the-switch-statement))，其中包含`default`標籤。</span><span class="sxs-lookup"><span data-stu-id="0edb8-467">The target of a `goto default` statement is the statement list in the immediately enclosing `switch` statement ([The switch statement](statements.md#the-switch-statement)), which contains a `default` label.</span></span> <span data-ttu-id="0edb8-468">如果`goto default`陳述式不加`switch`陳述式，或如果最接近的封閉式`switch`陳述式不包含`default`加上標籤，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="0edb8-468">If the `goto default` statement is not enclosed by a `switch` statement, or if the nearest enclosing `switch` statement does not contain a `default` label, a compile-time error occurs.</span></span>

<span data-ttu-id="0edb8-469">A`goto`陳述式無法結束`finally`區塊 ([try 陳述式](statements.md#the-try-statement))。</span><span class="sxs-lookup"><span data-stu-id="0edb8-469">A `goto` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="0edb8-470">當`goto`陳述式內，就會發生`finally`封鎖的目標`goto`陳述式必須在相同`finally`區塊，否則會發生編譯時期錯誤或。</span><span class="sxs-lookup"><span data-stu-id="0edb8-470">When a `goto` statement occurs within a `finally` block, the target of the `goto` statement must be within the same `finally` block, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="0edb8-471">A`goto`陳述式，如下所示：</span><span class="sxs-lookup"><span data-stu-id="0edb8-471">A `goto` statement is executed as follows:</span></span>

*  <span data-ttu-id="0edb8-472">如果`goto`陳述式會結束一個或多個`try`區塊相關聯`finally`區塊，控制項一開始會傳送到`finally`的最內層區塊`try`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-472">If the `goto` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="0edb8-473">時，並控制到達結束點`finally`區塊中，控制傳輸至`finally`區塊下一步 的封入`try`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-473">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="0edb8-474">此程序會重複直到`finally`的所有區塊中介`try`陳述式執行。</span><span class="sxs-lookup"><span data-stu-id="0edb8-474">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="0edb8-475">控制權會轉移到目標的`goto`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-475">Control is transferred to the target of the `goto` statement.</span></span>

<span data-ttu-id="0edb8-476">因為`goto`陳述式無條件地將控制權傳輸其他位置、 結束點`goto`陳述式絕不會是可連線。</span><span class="sxs-lookup"><span data-stu-id="0edb8-476">Because a `goto` statement unconditionally transfers control elsewhere, the end point of a `goto` statement is never reachable.</span></span>

### <a name="the-return-statement"></a><span data-ttu-id="0edb8-477">Return 陳述式</span><span class="sxs-lookup"><span data-stu-id="0edb8-477">The return statement</span></span>

<span data-ttu-id="0edb8-478">`return`陳述式將控制權傳回給目前的呼叫端函式所在的`return`陳述式隨即出現。</span><span class="sxs-lookup"><span data-stu-id="0edb8-478">The `return` statement returns control to the current caller of the function in which the `return` statement appears.</span></span>

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

<span data-ttu-id="0edb8-479">A`return`但沒有運算式的陳述式只能用於不會計算值時，也就是方法的結果類型的函式成員 ([方法主體](classes.md#method-body)) `void`，則`set`屬性存取子或索引子`add`和`remove`存取子的事件、 執行個體建構函式、 靜態建構函式或解構函式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-479">A `return` statement with no expression can be used only in a function member that does not compute a value, that is, a method with the result type ([Method body](classes.md#method-body)) `void`, the `set` accessor of a property or indexer, the `add` and `remove` accessors of an event, an instance constructor, a static constructor, or a destructor.</span></span>

<span data-ttu-id="0edb8-480">A`return`陳述式的運算式只能在計算值，也就是具有非 void 結果類型、 方法的函式成員`get`存取子的屬性或索引子或使用者定義的運算子。</span><span class="sxs-lookup"><span data-stu-id="0edb8-480">A `return` statement with an expression can only be used in a function member that computes a value, that is, a method with a non-void result type, the `get` accessor of a property or indexer, or a user-defined operator.</span></span> <span data-ttu-id="0edb8-481">隱含的轉換 ([隱含轉換](conversions.md#implicit-conversions)) 必須存在的運算式類型包含函式成員的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="0edb8-481">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) must exist from the type of the expression to the return type of the containing function member.</span></span>

<span data-ttu-id="0edb8-482">陳述式也可以使用匿名函式運算式的主體中傳回 ([匿名函式運算式](expressions.md#anonymous-function-expressions))，並參與，判斷哪些轉換存在這些函式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-482">Return statements can also be used in the body of anonymous function expressions ([Anonymous function expressions](expressions.md#anonymous-function-expressions)), and participate in determining which conversions exist for those functions.</span></span>

<span data-ttu-id="0edb8-483">它是編譯時期錯誤`return`陳述式才會出現在`finally`區塊 ([try 陳述式](statements.md#the-try-statement))。</span><span class="sxs-lookup"><span data-stu-id="0edb8-483">It is a compile-time error for a `return` statement to appear in a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span>

<span data-ttu-id="0edb8-484">A`return`陳述式，如下所示：</span><span class="sxs-lookup"><span data-stu-id="0edb8-484">A `return` statement is executed as follows:</span></span>

*  <span data-ttu-id="0edb8-485">如果`return`陳述式會指定運算式中，會評估運算式，而且產生的值已轉換為包含函式的傳回類型的隱含轉換。</span><span class="sxs-lookup"><span data-stu-id="0edb8-485">If the `return` statement specifies an expression, the expression is evaluated and the resulting value is converted to the return type of the containing function by an implicit conversion.</span></span> <span data-ttu-id="0edb8-486">轉換的結果會成為函式所產生的結果值。</span><span class="sxs-lookup"><span data-stu-id="0edb8-486">The result of the conversion becomes the result value produced by the function.</span></span>
*  <span data-ttu-id="0edb8-487">如果`return`陳述式會包含一或多個`try`或是`catch`區塊相關聯`finally`區塊，控制項一開始會傳送到`finally`最內層區塊`try`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-487">If the `return` statement is enclosed by one or more `try` or `catch` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="0edb8-488">時，並控制到達結束點`finally`區塊中，控制傳輸至`finally`區塊下一步 的封入`try`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-488">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="0edb8-489">此程序會重複直到`finally`區塊的所有封入`try`陳述式執行。</span><span class="sxs-lookup"><span data-stu-id="0edb8-489">This process is repeated until the `finally` blocks of all enclosing `try` statements have been executed.</span></span>
*  <span data-ttu-id="0edb8-490">如果包含的函式不是非同步函式，控制權會傳回結果值，以及包含函式的呼叫端，如果有的話。</span><span class="sxs-lookup"><span data-stu-id="0edb8-490">If the containing function is not an async function, control is returned to the caller of the containing function along with the result value, if any.</span></span>
*  <span data-ttu-id="0edb8-491">如果包含的函式是 async 函式，控制權給目前的呼叫端，而且結果值，如果有的話，會記錄在傳回的工作中所述 ([列舉程式介面](classes.md#enumerator-interfaces))。</span><span class="sxs-lookup"><span data-stu-id="0edb8-491">If the containing function is an async function, control is returned to the current caller, and the result value, if any, is recorded in the return task as described in ([Enumerator interfaces](classes.md#enumerator-interfaces)).</span></span>

<span data-ttu-id="0edb8-492">因為`return`陳述式無條件地將控制權傳輸其他位置、 結束點`return`陳述式絕不會是可連線。</span><span class="sxs-lookup"><span data-stu-id="0edb8-492">Because a `return` statement unconditionally transfers control elsewhere, the end point of a `return` statement is never reachable.</span></span>

### <a name="the-throw-statement"></a><span data-ttu-id="0edb8-493">Throw 陳述式</span><span class="sxs-lookup"><span data-stu-id="0edb8-493">The throw statement</span></span>

<span data-ttu-id="0edb8-494">`throw`陳述式會擲回的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="0edb8-494">The `throw` statement throws an exception.</span></span>

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

<span data-ttu-id="0edb8-495">A`throw`與運算式的陳述式會擲回的評估運算式所產生的值。</span><span class="sxs-lookup"><span data-stu-id="0edb8-495">A `throw` statement with an expression throws the value produced by evaluating the expression.</span></span> <span data-ttu-id="0edb8-496">運算式必須將類別類型的值表示`System.Exception`，類別類型衍生自`System.Exception`型別參數類型具有或`System.Exception`（或子類別） 為有效的基底類別。</span><span class="sxs-lookup"><span data-stu-id="0edb8-496">The expression must denote a value of the class type `System.Exception`, of a class type that derives from `System.Exception` or of a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span> <span data-ttu-id="0edb8-497">如果運算式的評估會產生`null`、`System.NullReferenceException`會改為擲回。</span><span class="sxs-lookup"><span data-stu-id="0edb8-497">If evaluation of the expression produces `null`, a `System.NullReferenceException` is thrown instead.</span></span>

<span data-ttu-id="0edb8-498">A`throw`但沒有運算式的陳述式可以只能用於`catch`封鎖，請在此情況下該陳述式重新擲回例外狀況正在處理由該`catch`區塊。</span><span class="sxs-lookup"><span data-stu-id="0edb8-498">A `throw` statement with no expression can be used only in a `catch` block, in which case that statement re-throws the exception that is currently being handled by that `catch` block.</span></span>

<span data-ttu-id="0edb8-499">因為`throw`陳述式無條件地將控制權傳輸其他位置、 結束點`throw`陳述式絕不會是可連線。</span><span class="sxs-lookup"><span data-stu-id="0edb8-499">Because a `throw` statement unconditionally transfers control elsewhere, the end point of a `throw` statement is never reachable.</span></span>

<span data-ttu-id="0edb8-500">擲回例外狀況時，控制權會轉移到第一個`catch`子句中為封入`try`可以處理的例外狀況的陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-500">When an exception is thrown, control is transferred to the first `catch` clause in an enclosing `try` statement that can handle the exception.</span></span> <span data-ttu-id="0edb8-501">從點將控制權傳輸至適當的例外狀況處理常式所擲回的例外狀況的點進行的程序稱為***例外狀況傳播***。</span><span class="sxs-lookup"><span data-stu-id="0edb8-501">The process that takes place from the point of the exception being thrown to the point of transferring control to a suitable exception handler is known as ***exception propagation***.</span></span> <span data-ttu-id="0edb8-502">重複評估直到下列步驟所組成的例外狀況傳播`catch`找到符合的例外狀況的子句。</span><span class="sxs-lookup"><span data-stu-id="0edb8-502">Propagation of an exception consists of repeatedly evaluating the following steps until a `catch` clause that matches the exception is found.</span></span> <span data-ttu-id="0edb8-503">在此說明中，***擲回點***一開始是例外狀況時的位置。</span><span class="sxs-lookup"><span data-stu-id="0edb8-503">In this description, the ***throw point*** is initially the location at which the exception is thrown.</span></span>

*  <span data-ttu-id="0edb8-504">在目前的函式成員中，每個`try`擲回點封入陳述式會檢查。</span><span class="sxs-lookup"><span data-stu-id="0edb8-504">In the current function member, each `try` statement that encloses the throw point is examined.</span></span> <span data-ttu-id="0edb8-505">每個陳述式`S`，從最內層`try`陳述式，直到最外層`try`陳述式，會評估下列步驟：</span><span class="sxs-lookup"><span data-stu-id="0edb8-505">For each statement `S`, starting with the innermost `try` statement and ending with the outermost `try` statement, the following steps are evaluated:</span></span>

   * <span data-ttu-id="0edb8-506">如果`try`區塊`S`圍住擲回點與 S 有一或多個`catch`子句，`catch`子句會檢查中出現的順序來尋找適合的例外狀況，根據規則中指定的處理常式一節[try 陳述式](statements.md#the-try-statement)。</span><span class="sxs-lookup"><span data-stu-id="0edb8-506">If the `try` block of `S` encloses the throw point and if S has one or more `catch` clauses, the `catch` clauses are examined in order of appearance to locate a suitable handler for the exception, according to the rules specified in Section [The try statement](statements.md#the-try-statement).</span></span> <span data-ttu-id="0edb8-507">如果相符`catch`子句，藉由將控制權傳輸至該區塊完成例外狀況傳播`catch`子句。</span><span class="sxs-lookup"><span data-stu-id="0edb8-507">If a matching `catch` clause is located, the exception propagation is completed by transferring control to the block of that `catch` clause.</span></span>

   * <span data-ttu-id="0edb8-508">否則，如果`try`區塊或`catch`區塊`S`圍住擲回點而如果`S`有`finally`區塊中，控制傳輸至`finally`區塊。</span><span class="sxs-lookup"><span data-stu-id="0edb8-508">Otherwise, if the `try` block or a `catch` block of `S` encloses the throw point and if `S` has a `finally` block, control is transferred to the `finally` block.</span></span> <span data-ttu-id="0edb8-509">如果`finally`區塊擲回另一個例外狀況，終止目前的例外狀況的處理。</span><span class="sxs-lookup"><span data-stu-id="0edb8-509">If the `finally` block throws another exception, processing of the current exception is terminated.</span></span> <span data-ttu-id="0edb8-510">否則，當控制項到達結束點`finally`區塊，目前的例外狀況的處理會繼續。</span><span class="sxs-lookup"><span data-stu-id="0edb8-510">Otherwise, when control reaches the end point of the `finally` block, processing of the current exception is continued.</span></span>

*  <span data-ttu-id="0edb8-511">如果目前的函式引動過程中找不到的例外狀況處理常式，已終止的函式引動過程，並發生下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="0edb8-511">If an exception handler was not located in the current function invocation, the function invocation is terminated, and one of the following occurs:</span></span>

   * <span data-ttu-id="0edb8-512">如果目前的函式為非同步，上述步驟會使用對應至從中叫用函式成員的陳述式擲回點重複函式的呼叫端。</span><span class="sxs-lookup"><span data-stu-id="0edb8-512">If the current function is non-async, the steps above are repeated for the caller of the function with a throw point corresponding to the statement from which the function member was invoked.</span></span>

   * <span data-ttu-id="0edb8-513">如果目前的函式是 async 和傳回工作，例外狀況會記錄在傳回的工作中，放入發生錯誤或已取消狀態中所述[列舉程式介面](classes.md#enumerator-interfaces)。</span><span class="sxs-lookup"><span data-stu-id="0edb8-513">If the current function is async and task-returning, the exception is recorded in the return task, which is put into a faulted or cancelled state as described in [Enumerator interfaces](classes.md#enumerator-interfaces).</span></span>

   * <span data-ttu-id="0edb8-514">如果目前的函式是 async 和傳回 void，目前執行緒的同步處理內容會通知中所述[可列舉的介面](classes.md#enumerable-interfaces)。</span><span class="sxs-lookup"><span data-stu-id="0edb8-514">If the current function is async and void-returning, the synchronization context of the current thread is notified as described in [Enumerable interfaces](classes.md#enumerable-interfaces).</span></span>

*  <span data-ttu-id="0edb8-515">如果例外狀況處理終止目前的執行緒中所有函式的成員引動過程，指出執行緒有例外狀況，沒有處理常式接著執行緒就會自行結束。</span><span class="sxs-lookup"><span data-stu-id="0edb8-515">If the exception processing terminates all function member invocations in the current thread, indicating that the thread has no handler for the exception, then the thread is itself terminated.</span></span> <span data-ttu-id="0edb8-516">這類終止的影響是由實作定義。</span><span class="sxs-lookup"><span data-stu-id="0edb8-516">The impact of such termination is implementation-defined.</span></span>

## <a name="the-try-statement"></a><span data-ttu-id="0edb8-517">Try 陳述式</span><span class="sxs-lookup"><span data-stu-id="0edb8-517">The try statement</span></span>

<span data-ttu-id="0edb8-518">`try`陳述式所提供的機制來攔截區塊的執行期間發生的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="0edb8-518">The `try` statement provides a mechanism for catching exceptions that occur during execution of a block.</span></span> <span data-ttu-id="0edb8-519">此外，`try`陳述式讓您能夠指定程式碼一律會執行，當控制離開區塊`try`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-519">Furthermore, the `try` statement provides the ability to specify a block of code that is always executed when control leaves the `try` statement.</span></span>

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

<span data-ttu-id="0edb8-520">有三種可能的形式`try`陳述式：</span><span class="sxs-lookup"><span data-stu-id="0edb8-520">There are three possible forms of `try` statements:</span></span>

*  <span data-ttu-id="0edb8-521">A`try`區塊後面接著一或多個`catch`區塊。</span><span class="sxs-lookup"><span data-stu-id="0edb8-521">A `try` block followed by one or more `catch` blocks.</span></span>
*  <span data-ttu-id="0edb8-522">A`try`區塊後面`finally`區塊。</span><span class="sxs-lookup"><span data-stu-id="0edb8-522">A `try` block followed by a `finally` block.</span></span>
*  <span data-ttu-id="0edb8-523">A`try`區塊後面接著一或多個`catch`區塊後面`finally`區塊。</span><span class="sxs-lookup"><span data-stu-id="0edb8-523">A `try` block followed by one or more `catch` blocks followed by a `finally` block.</span></span>

<span data-ttu-id="0edb8-524">當`catch`子句會指定*exception_specifier*的類型必須是`System.Exception`，型別衍生自`System.Exception`或型別參數具有`System.Exception`（或子類別） 作為其有效基底類別。</span><span class="sxs-lookup"><span data-stu-id="0edb8-524">When a `catch` clause specifies an *exception_specifier*, the type must be `System.Exception`, a type that derives from `System.Exception` or a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span>

<span data-ttu-id="0edb8-525">時`catch`子句會指定這兩*exception_specifier*具有*識別碼*，則***例外狀況變數***宣告指定名稱和類型。</span><span class="sxs-lookup"><span data-stu-id="0edb8-525">When a `catch` clause specifies both an *exception_specifier* with an *identifier*, an ***exception variable*** of the given name and type is declared.</span></span> <span data-ttu-id="0edb8-526">例外狀況變數對應至範圍涵蓋的本機變數`catch`子句。</span><span class="sxs-lookup"><span data-stu-id="0edb8-526">The exception variable corresponds to a local variable with a scope that extends over the `catch` clause.</span></span> <span data-ttu-id="0edb8-527">在執行期間*exception_filter*並*區塊*，例外狀況變數代表目前正在處理的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="0edb8-527">During execution of the *exception_filter* and *block*, the exception variable represents the exception currently being handled.</span></span> <span data-ttu-id="0edb8-528">明確設定檢查的目的而言，例外狀況變數會被視為在其整個範圍中明確指派。</span><span class="sxs-lookup"><span data-stu-id="0edb8-528">For purposes of definite assignment checking, the exception variable is considered definitely assigned in its entire scope.</span></span>

<span data-ttu-id="0edb8-529">除非`catch`子句會包含例外狀況變數名稱，就無法存取在篩選中的例外狀況物件和`catch`區塊。</span><span class="sxs-lookup"><span data-stu-id="0edb8-529">Unless a `catch` clause includes an exception variable name, it is impossible to access the exception object in the filter and `catch` block.</span></span>

<span data-ttu-id="0edb8-530">A`catch`未指定的子句*exception_specifier*稱為一般`catch`子句。</span><span class="sxs-lookup"><span data-stu-id="0edb8-530">A `catch` clause that does not specify an *exception_specifier* is called a general `catch` clause.</span></span>

<span data-ttu-id="0edb8-531">有些程式語言可能支援的例外狀況，不是可表示為物件衍生自`System.Exception`，不過這類例外狀況可能永遠不會產生 C# 程式碼。</span><span class="sxs-lookup"><span data-stu-id="0edb8-531">Some programming languages may support exceptions that are not representable as an object derived from `System.Exception`, although such exceptions could never be generated by C# code.</span></span> <span data-ttu-id="0edb8-532">一般`catch`子句可用來攔截這類例外狀況。</span><span class="sxs-lookup"><span data-stu-id="0edb8-532">A general `catch` clause may be used to catch such exceptions.</span></span> <span data-ttu-id="0edb8-533">因此，一般`catch`子句會從指定的型別語意不相同`System.Exception`，在於前者可能也攔截例外狀況的其他語言。</span><span class="sxs-lookup"><span data-stu-id="0edb8-533">Thus, a general `catch` clause is semantically different from one that specifies the type `System.Exception`, in that the former may also catch exceptions from other languages.</span></span>

<span data-ttu-id="0edb8-534">若要找出例外狀況處理常式`catch`語彙順序進行檢查的子句。</span><span class="sxs-lookup"><span data-stu-id="0edb8-534">In order to locate a handler for an exception, `catch` clauses are examined in lexical order.</span></span> <span data-ttu-id="0edb8-535">如果`catch`子句指定型別，但沒有例外狀況篩選條件，用於更新的版本是編譯時期錯誤`catch`子句在同一個`try`陳述式，以指定的類型相同，或衍生自輸入。</span><span class="sxs-lookup"><span data-stu-id="0edb8-535">If a `catch` clause specifies a type but no exception filter, it is a compile-time error for a later `catch` clause in the same `try` statement to specify a type that is the same as, or is derived from, that type.</span></span> <span data-ttu-id="0edb8-536">如果`catch`子句會指定沒有型別和任何篩選條件，它必須是最後一個`catch`子句，`try`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-536">If a `catch` clause specifies no type and no filter, it must be the last `catch` clause for that `try` statement.</span></span>

<span data-ttu-id="0edb8-537">內`catch`區塊中，`throw`陳述式 ([throw 陳述式](statements.md#the-throw-statement)) 沒有運算式可以用來重新擲回已攔截的例外狀況`catch`區塊。</span><span class="sxs-lookup"><span data-stu-id="0edb8-537">Within a `catch` block, a `throw` statement ([The throw statement](statements.md#the-throw-statement)) with no expression can be used to re-throw the exception that was caught by the `catch` block.</span></span> <span data-ttu-id="0edb8-538">例外狀況變數的指派不會改變會重新擲回的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="0edb8-538">Assignments to an exception variable do not alter the exception that is re-thrown.</span></span>

<span data-ttu-id="0edb8-539">在範例</span><span class="sxs-lookup"><span data-stu-id="0edb8-539">In the example</span></span>
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
<span data-ttu-id="0edb8-540">此方法`F`攔截到例外狀況、 一些診斷資訊寫入主控台，改變例外狀況變數和重新擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="0edb8-540">the method `F` catches an exception, writes some diagnostic information to the console, alters the exception variable, and re-throws the exception.</span></span> <span data-ttu-id="0edb8-541">重新擲回的例外狀況會是原始的例外狀況，如此所產生的輸出：</span><span class="sxs-lookup"><span data-stu-id="0edb8-541">The exception that is re-thrown is the original exception, so the output produced is:</span></span>
```
Exception in F: G
Exception in Main: G
```

<span data-ttu-id="0edb8-542">如果第一個 catch 區塊擲回`e`而不是重新擲回目前例外狀況，產生的輸出應如下：</span><span class="sxs-lookup"><span data-stu-id="0edb8-542">If the first catch block had thrown `e` instead of rethrowing the current exception, the output produced would be as follows:</span></span>
```csharp
Exception in F: G
Exception in Main: F
```

<span data-ttu-id="0edb8-543">它是編譯時期錯誤`break`， `continue`，或`goto`陳述式時的控制權轉移`finally`區塊。</span><span class="sxs-lookup"><span data-stu-id="0edb8-543">It is a compile-time error for a `break`, `continue`, or `goto` statement to transfer control out of a `finally` block.</span></span> <span data-ttu-id="0edb8-544">當`break`， `continue`，或`goto`陳述式中發生`finally`區塊中，陳述式的目標必須是在相同`finally`區塊，或否則會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="0edb8-544">When a `break`, `continue`, or `goto` statement occurs in a `finally` block, the target of the statement must be within the same `finally` block, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="0edb8-545">它是編譯時期錯誤`return`陳述式中發生`finally`區塊。</span><span class="sxs-lookup"><span data-stu-id="0edb8-545">It is a compile-time error for a `return` statement to occur in a `finally` block.</span></span>

<span data-ttu-id="0edb8-546">A`try`陳述式，如下所示：</span><span class="sxs-lookup"><span data-stu-id="0edb8-546">A `try` statement is executed as follows:</span></span>

*  <span data-ttu-id="0edb8-547">控制權會轉移到`try`區塊。</span><span class="sxs-lookup"><span data-stu-id="0edb8-547">Control is transferred to the `try` block.</span></span>
*  <span data-ttu-id="0edb8-548">時，並控制到達結束點`try`區塊：</span><span class="sxs-lookup"><span data-stu-id="0edb8-548">When and if control reaches the end point of the `try` block:</span></span>
   *  <span data-ttu-id="0edb8-549">如果`try`陳述式有`finally`區塊中，`finally`區塊執行。</span><span class="sxs-lookup"><span data-stu-id="0edb8-549">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
   *  <span data-ttu-id="0edb8-550">控制權會轉移至結束點`try`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-550">Control is transferred to the end point of the `try` statement.</span></span>

*  <span data-ttu-id="0edb8-551">如果例外狀況會傳播到`try`陳述式執行期間`try`區塊：</span><span class="sxs-lookup"><span data-stu-id="0edb8-551">If an exception is propagated to the `try` statement during execution of the `try` block:</span></span>
   *  <span data-ttu-id="0edb8-552">`catch`子句中，如果有的話，會檢查中出現的順序來尋找適合的處理常式，例外狀況。</span><span class="sxs-lookup"><span data-stu-id="0edb8-552">The `catch` clauses, if any, are examined in order of appearance to locate a suitable handler for the exception.</span></span> <span data-ttu-id="0edb8-553">如果`catch`子句未指定類型，或指定的例外狀況型別或基底類型的例外狀況類型：</span><span class="sxs-lookup"><span data-stu-id="0edb8-553">If a `catch` clause does not specify a type, or specifies the exception type or a base type of the exception type:</span></span>
      *  <span data-ttu-id="0edb8-554">如果`catch`子句宣告的例外狀況變數、 例外狀況物件指派給例外狀況變數。</span><span class="sxs-lookup"><span data-stu-id="0edb8-554">If the `catch` clause declares an exception variable, the exception object is assigned to the exception variable.</span></span>
      *  <span data-ttu-id="0edb8-555">如果`catch`子句宣告例外狀況篩選條件，對篩選進行評估。</span><span class="sxs-lookup"><span data-stu-id="0edb8-555">If the `catch` clause declares an exception filter, the filter is evaluated.</span></span> <span data-ttu-id="0edb8-556">如果評估為`false`，catch 子句不是相符項目，則會繼續搜尋透過任何後續`catch`適當的處理常式的子句。</span><span class="sxs-lookup"><span data-stu-id="0edb8-556">If it evaluates to `false`, the catch clause is not a match, and the search continues through any subsequent `catch` clauses for a suitable handler.</span></span>
      *  <span data-ttu-id="0edb8-557">否則，請`catch`子句會被視為相符項目，而且控制權會轉移至對應的`catch`區塊。</span><span class="sxs-lookup"><span data-stu-id="0edb8-557">Otherwise, the `catch` clause is considered a match, and control is transferred to the matching `catch` block.</span></span>
      *  <span data-ttu-id="0edb8-558">時，並控制到達結束點`catch`區塊：</span><span class="sxs-lookup"><span data-stu-id="0edb8-558">When and if control reaches the end point of the `catch` block:</span></span>
         * <span data-ttu-id="0edb8-559">如果`try`陳述式有`finally`區塊中，`finally`區塊執行。</span><span class="sxs-lookup"><span data-stu-id="0edb8-559">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
         * <span data-ttu-id="0edb8-560">控制權會轉移至結束點`try`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-560">Control is transferred to the end point of the `try` statement.</span></span>
      *  <span data-ttu-id="0edb8-561">如果例外狀況會傳播到`try`陳述式執行期間`catch`區塊：</span><span class="sxs-lookup"><span data-stu-id="0edb8-561">If an exception is propagated to the `try` statement during execution of the `catch` block:</span></span>
         *  <span data-ttu-id="0edb8-562">如果`try`陳述式有`finally`區塊中，`finally`區塊執行。</span><span class="sxs-lookup"><span data-stu-id="0edb8-562">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
         *  <span data-ttu-id="0edb8-563">例外狀況會傳播到下一步 的封入`try`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-563">The exception is propagated to the next enclosing `try` statement.</span></span>
   *  <span data-ttu-id="0edb8-564">如果`try`陳述式沒有`catch`子句或者如果沒有任何`catch`子句與相符的例外狀況：</span><span class="sxs-lookup"><span data-stu-id="0edb8-564">If the `try` statement has no `catch` clauses or if no `catch` clause matches the exception:</span></span>
      *  <span data-ttu-id="0edb8-565">如果`try`陳述式有`finally`區塊中，`finally`區塊執行。</span><span class="sxs-lookup"><span data-stu-id="0edb8-565">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
      *  <span data-ttu-id="0edb8-566">例外狀況會傳播到下一步 的封入`try`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-566">The exception is propagated to the next enclosing `try` statement.</span></span>

<span data-ttu-id="0edb8-567">陳述式`finally`當控制離開區塊一律會執行`try`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-567">The statements of a `finally` block are always executed when control leaves a `try` statement.</span></span> <span data-ttu-id="0edb8-568">這適用於控制傳輸是否正常執行，因為執行的結果就會發生`break`， `continue`， `goto`，或`return`陳述式，或由於傳播出的例外狀況`try`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-568">This is true whether the control transfer occurs as a result of normal execution, as a result of executing a `break`, `continue`, `goto`, or `return` statement, or as a result of propagating an exception out of the `try` statement.</span></span>

<span data-ttu-id="0edb8-569">如果在執行期間擲回例外狀況`finally`區塊中，並不會攔截內相同的 finally 區塊，例外狀況會傳播到下一步 的封入`try`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-569">If an exception is thrown during execution of a `finally` block, and is not caught within the same finally block, the exception is propagated to the next enclosing `try` statement.</span></span> <span data-ttu-id="0edb8-570">如果另一個例外狀況傳播的過程中，該例外狀況將會遺失。</span><span class="sxs-lookup"><span data-stu-id="0edb8-570">If another exception was in the process of being propagated, that exception is lost.</span></span> <span data-ttu-id="0edb8-571">討論的傳播例外狀況程序的描述中進一步`throw`陳述式 ([throw 陳述式](statements.md#the-throw-statement))。</span><span class="sxs-lookup"><span data-stu-id="0edb8-571">The process of propagating an exception is discussed further in the description of the `throw` statement ([The throw statement](statements.md#the-throw-statement)).</span></span>

<span data-ttu-id="0edb8-572">`try`區塊`try`陳述式是連線到如果`try`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-572">The `try` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="0edb8-573">A`catch`區塊`try`陳述式是連線到如果`try`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-573">A `catch` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="0edb8-574">`finally`區塊`try`陳述式是連線到如果`try`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-574">The `finally` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="0edb8-575">結束點`try`陳述式是連線到兩個項目時：</span><span class="sxs-lookup"><span data-stu-id="0edb8-575">The end point of a `try` statement is reachable if both of the following are true:</span></span>

*  <span data-ttu-id="0edb8-576">終點`try`區塊是連線或結束點的至少一個`catch`區塊可連線。</span><span class="sxs-lookup"><span data-stu-id="0edb8-576">The end point of the `try` block is reachable or the end point of at least one `catch` block is reachable.</span></span>
*  <span data-ttu-id="0edb8-577">如果`finally`區塊已存在，結束點`finally`區塊可連線。</span><span class="sxs-lookup"><span data-stu-id="0edb8-577">If a `finally` block is present, the end point of the `finally` block is reachable.</span></span>

## <a name="the-checked-and-unchecked-statements"></a><span data-ttu-id="0edb8-578">Checked 與 unchecked 陳述式</span><span class="sxs-lookup"><span data-stu-id="0edb8-578">The checked and unchecked statements</span></span>

<span data-ttu-id="0edb8-579">`checked`並`unchecked`陳述式可用來控制***溢位檢查內容***整數型別算術運算和轉換。</span><span class="sxs-lookup"><span data-stu-id="0edb8-579">The `checked` and `unchecked` statements are used to control the ***overflow checking context*** for integral-type arithmetic operations and conversions.</span></span>

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

<span data-ttu-id="0edb8-580">`checked`陳述式會導致中的所有運算式*區塊*要檢查的內容中評估並`unchecked`陳述式會都導致中的所有運算式*區塊*来進行評估unchecked 的內容。</span><span class="sxs-lookup"><span data-stu-id="0edb8-580">The `checked` statement causes all expressions in the *block* to be evaluated in a checked context, and the `unchecked` statement causes all expressions in the *block* to be evaluated in an unchecked context.</span></span>

<span data-ttu-id="0edb8-581">`checked`並`unchecked`陳述式是相當於`checked`並`unchecked`運算子 ([checked 與 unchecked 運算子](expressions.md#the-checked-and-unchecked-operators))，不同之處在於他們對區塊，而不是運算式.</span><span class="sxs-lookup"><span data-stu-id="0edb8-581">The `checked` and `unchecked` statements are precisely equivalent to the `checked` and `unchecked` operators ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)), except that they operate on blocks instead of expressions.</span></span>

## <a name="the-lock-statement"></a><span data-ttu-id="0edb8-582">Lock 陳述式</span><span class="sxs-lookup"><span data-stu-id="0edb8-582">The lock statement</span></span>

<span data-ttu-id="0edb8-583">`lock`陳述式會取得指定物件的互斥鎖定、 執行陳述式，並再釋放鎖定。</span><span class="sxs-lookup"><span data-stu-id="0edb8-583">The `lock` statement obtains the mutual-exclusion lock for a given object, executes a statement, and then releases the lock.</span></span>

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

<span data-ttu-id="0edb8-584">運算式`lock`陳述式必須將已知類型的值表示*reference_type*。</span><span class="sxs-lookup"><span data-stu-id="0edb8-584">The expression of a `lock` statement must denote a value of a type known to be a *reference_type*.</span></span> <span data-ttu-id="0edb8-585">沒有任何隱含 boxing 轉換 ([Boxing 轉換](conversions.md#boxing-conversions)) 的運算式執行曾執行`lock`陳述式，因此是要表示的值運算式的編譯時期錯誤*value_type*.</span><span class="sxs-lookup"><span data-stu-id="0edb8-585">No implicit boxing conversion ([Boxing conversions](conversions.md#boxing-conversions)) is ever performed for the expression of a `lock` statement, and thus it is a compile-time error for the expression to denote a value of a *value_type*.</span></span>

<span data-ttu-id="0edb8-586">A`lock`表單的陳述式</span><span class="sxs-lookup"><span data-stu-id="0edb8-586">A `lock` statement of the form</span></span>
```csharp
lock (x) ...
```
<span data-ttu-id="0edb8-587">何處`x`是的運算式*reference_type*，就相當於</span><span class="sxs-lookup"><span data-stu-id="0edb8-587">where `x` is an expression of a *reference_type*, is precisely equivalent to</span></span>
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
<span data-ttu-id="0edb8-588">但只會評估 `x` 一次。</span><span class="sxs-lookup"><span data-stu-id="0edb8-588">except that `x` is only evaluated once.</span></span>

<span data-ttu-id="0edb8-589">持有的互斥鎖定時，在相同的執行緒中執行的程式碼亦可取得再釋放鎖定。</span><span class="sxs-lookup"><span data-stu-id="0edb8-589">While a mutual-exclusion lock is held, code executing in the same execution thread can also obtain and release the lock.</span></span> <span data-ttu-id="0edb8-590">不過，其他執行緒中執行的程式碼會封鎖取得鎖定，直到釋放鎖定為止。</span><span class="sxs-lookup"><span data-stu-id="0edb8-590">However, code executing in other threads is blocked from obtaining the lock until the lock is released.</span></span>

<span data-ttu-id="0edb8-591">鎖定`System.Type`不建議您若要同步處理靜態資料的存取權的物件。</span><span class="sxs-lookup"><span data-stu-id="0edb8-591">Locking `System.Type` objects in order to synchronize access to static data is not recommended.</span></span> <span data-ttu-id="0edb8-592">其他程式碼可能會鎖定在相同的型別，可能會導致死結。</span><span class="sxs-lookup"><span data-stu-id="0edb8-592">Other code might lock on the same type, which can result in deadlock.</span></span> <span data-ttu-id="0edb8-593">更好的方法是藉由鎖定私用靜態物件同步處理靜態資料的存取。</span><span class="sxs-lookup"><span data-stu-id="0edb8-593">A better approach is to synchronize access to static data by locking a private static object.</span></span> <span data-ttu-id="0edb8-594">例如: </span><span class="sxs-lookup"><span data-stu-id="0edb8-594">For example:</span></span>
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

## <a name="the-using-statement"></a><span data-ttu-id="0edb8-595">using 陳述式</span><span class="sxs-lookup"><span data-stu-id="0edb8-595">The using statement</span></span>

<span data-ttu-id="0edb8-596">`using`陳述式會取得一或多個資源、 執行陳述式，然後處置的資源。</span><span class="sxs-lookup"><span data-stu-id="0edb8-596">The `using` statement obtains one or more resources, executes a statement, and then disposes of the resource.</span></span>

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

<span data-ttu-id="0edb8-597">A ***resource***是類別或結構實作`System.IDisposable`，其中包含名為單一無參數方法`Dispose`。</span><span class="sxs-lookup"><span data-stu-id="0edb8-597">A ***resource*** is a class or struct that implements `System.IDisposable`, which includes a single parameterless method named `Dispose`.</span></span> <span data-ttu-id="0edb8-598">使用資源的程式碼可以呼叫`Dispose`表示不再需要資源時。</span><span class="sxs-lookup"><span data-stu-id="0edb8-598">Code that is using a resource can call `Dispose` to indicate that the resource is no longer needed.</span></span> <span data-ttu-id="0edb8-599">如果`Dispose`未呼叫，然後自動處置最後發生由於記憶體回收。</span><span class="sxs-lookup"><span data-stu-id="0edb8-599">If `Dispose` is not called, then automatic disposal eventually occurs as a consequence of garbage collection.</span></span>

<span data-ttu-id="0edb8-600">如果格式*resource_acquisition*是*local_variable_declaration*然後的型別*local_variable_declaration*必須是`dynamic`或型別可隱含地轉換為`System.IDisposable`。</span><span class="sxs-lookup"><span data-stu-id="0edb8-600">If the form of *resource_acquisition* is *local_variable_declaration* then the type of the *local_variable_declaration* must be either `dynamic` or a type that can be implicitly converted to `System.IDisposable`.</span></span> <span data-ttu-id="0edb8-601">如果格式*resource_acquisition*是*運算式*則此運算式必須是隱含地轉換成`System.IDisposable`。</span><span class="sxs-lookup"><span data-stu-id="0edb8-601">If the form of *resource_acquisition* is *expression* then this expression must be implicitly convertible to `System.IDisposable`.</span></span>

<span data-ttu-id="0edb8-602">在宣告區域變數*resource_acquisition*是唯讀的並必須包含初始設定式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-602">Local variables declared in a *resource_acquisition* are read-only, and must include an initializer.</span></span> <span data-ttu-id="0edb8-603">如果內嵌的陳述式嘗試修改這些本機變數，就會發生編譯時期錯誤 (透過指派或`++`並`--`運算子)、 取得位址，或將其做為傳遞`ref`或`out`參數。</span><span class="sxs-lookup"><span data-stu-id="0edb8-603">A compile-time error occurs if the embedded statement attempts to modify these local variables (via assignment or the `++` and `--` operators) , take the address of them, or pass them as `ref` or `out` parameters.</span></span>

<span data-ttu-id="0edb8-604">A`using`陳述式會轉譯成三個部分︰ 取得、 使用量以及各種可供使用。</span><span class="sxs-lookup"><span data-stu-id="0edb8-604">A `using` statement is translated into three parts: acquisition, usage, and disposal.</span></span> <span data-ttu-id="0edb8-605">使用資源以隱含方式住`try`陳述式，其中包含`finally`子句。</span><span class="sxs-lookup"><span data-stu-id="0edb8-605">Usage of the resource is implicitly enclosed in a `try` statement that includes a `finally` clause.</span></span> <span data-ttu-id="0edb8-606">這`finally`子句處置的資源。</span><span class="sxs-lookup"><span data-stu-id="0edb8-606">This `finally` clause disposes of the resource.</span></span> <span data-ttu-id="0edb8-607">如果`null`取得資源，則不需要呼叫`Dispose`進行，而且會擲回任何例外狀況。</span><span class="sxs-lookup"><span data-stu-id="0edb8-607">If a `null` resource is acquired, then no call to `Dispose` is made, and no exception is thrown.</span></span> <span data-ttu-id="0edb8-608">如果資源是型別`dynamic`則會以動態方式轉換透過隱含的動態轉換 ([隱含的動態轉換](conversions.md#implicit-dynamic-conversions)) 來`IDisposable`併購，為了確保在轉換期間成功之前的使用量和處置。</span><span class="sxs-lookup"><span data-stu-id="0edb8-608">If the resource is of type `dynamic` it is dynamically converted through an implicit dynamic conversion ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions)) to `IDisposable` during acquisition in order to ensure that the conversion is successful before the usage and disposal.</span></span>

<span data-ttu-id="0edb8-609">A`using`表單的陳述式</span><span class="sxs-lookup"><span data-stu-id="0edb8-609">A `using` statement of the form</span></span>
```csharp
using (ResourceType resource = expression) statement
```
<span data-ttu-id="0edb8-610">對應至其中的三個可能的擴充。</span><span class="sxs-lookup"><span data-stu-id="0edb8-610">corresponds to one of three possible expansions.</span></span> <span data-ttu-id="0edb8-611">當`ResourceType`不可為 null 的實值型別中，展開</span><span class="sxs-lookup"><span data-stu-id="0edb8-611">When `ResourceType` is a non-nullable value type, the expansion is</span></span>
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

<span data-ttu-id="0edb8-612">否則，當`ResourceType`而不是可為 null 的實值型別或參考型別`dynamic`，擴充</span><span class="sxs-lookup"><span data-stu-id="0edb8-612">Otherwise, when `ResourceType` is a nullable value type or a reference type other than `dynamic`, the expansion is</span></span>
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

<span data-ttu-id="0edb8-613">否則，當`ResourceType`是`dynamic`，擴充</span><span class="sxs-lookup"><span data-stu-id="0edb8-613">Otherwise, when `ResourceType` is `dynamic`, the expansion is</span></span>
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

<span data-ttu-id="0edb8-614">其中一個擴充中，`resource`變數是唯讀，在內嵌的陳述式，和`d`變數是在中，無法存取或不可見，內嵌的陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-614">In either expansion, the `resource` variable is read-only in the embedded statement, and the `d` variable is inaccessible in, and invisible to, the embedded statement.</span></span>

<span data-ttu-id="0edb8-615">實作允許以不同的方式實作指定 using 陳述式，基於效能考量，例如，只要是與上述延伸一致的行為。</span><span class="sxs-lookup"><span data-stu-id="0edb8-615">An implementation is permitted to implement a given using-statement differently, e.g. for performance reasons, as long as the behavior is consistent with the above expansion.</span></span>

<span data-ttu-id="0edb8-616">A`using`表單的陳述式</span><span class="sxs-lookup"><span data-stu-id="0edb8-616">A `using` statement of the form</span></span>
```csharp
using (expression) statement
```
<span data-ttu-id="0edb8-617">具有相同的三個可能展開。</span><span class="sxs-lookup"><span data-stu-id="0edb8-617">has the same three possible expansions.</span></span> <span data-ttu-id="0edb8-618">在此情況下`ResourceType`是隱含的編譯時期型別`expression`，如果有的話。</span><span class="sxs-lookup"><span data-stu-id="0edb8-618">In this case `ResourceType` is implicitly the compile-time type of the `expression`, if it has one.</span></span> <span data-ttu-id="0edb8-619">否則介面`IDisposable`本身做為`ResourceType`。</span><span class="sxs-lookup"><span data-stu-id="0edb8-619">Otherwise the interface `IDisposable` itself is used as the `ResourceType`.</span></span> <span data-ttu-id="0edb8-620">`resource`變數是在中，無法存取或不可見，內嵌的陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-620">The `resource` variable is inaccessible in, and invisible to, the embedded statement.</span></span>

<span data-ttu-id="0edb8-621">當*resource_acquisition*採用的形式*local_variable_declaration*，就能夠取得指定類型的多項資源。</span><span class="sxs-lookup"><span data-stu-id="0edb8-621">When a *resource_acquisition* takes the form of a *local_variable_declaration*, it is possible to acquire multiple resources of a given type.</span></span> <span data-ttu-id="0edb8-622">A`using`表單的陳述式</span><span class="sxs-lookup"><span data-stu-id="0edb8-622">A `using` statement of the form</span></span>
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
<span data-ttu-id="0edb8-623">精確地相當於一連串的巢狀`using`陳述式：</span><span class="sxs-lookup"><span data-stu-id="0edb8-623">is precisely equivalent to a sequence of nested `using` statements:</span></span>
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

<span data-ttu-id="0edb8-624">下列範例會建立名為`log.txt`並寫入檔案中的兩行文字。</span><span class="sxs-lookup"><span data-stu-id="0edb8-624">The example below creates a file named `log.txt` and writes two lines of text to the file.</span></span> <span data-ttu-id="0edb8-625">此範例會開啟該相同的檔案進行讀取，並將包含的行文字複製到主控台。</span><span class="sxs-lookup"><span data-stu-id="0edb8-625">The example then opens that same file for reading and copies the contained lines of text to the console.</span></span>
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

<span data-ttu-id="0edb8-626">由於`TextWriter`並`TextReader`類別會實作`IDisposable`介面，可以使用範例`using`陳述式，以確保基礎檔案會正確關閉下列寫入或讀取作業。</span><span class="sxs-lookup"><span data-stu-id="0edb8-626">Since the `TextWriter` and `TextReader` classes implement the `IDisposable` interface, the example can use `using` statements to ensure that the underlying file is properly closed following the write or read operations.</span></span>

## <a name="the-yield-statement"></a><span data-ttu-id="0edb8-627">Yield 陳述式</span><span class="sxs-lookup"><span data-stu-id="0edb8-627">The yield statement</span></span>

<span data-ttu-id="0edb8-628">`yield`陳述式會在 iterator 區塊 ([區塊](statements.md#blocks)) 來產生列舉值物件的值 ([列舉值物件](classes.md#enumerator-objects)) 或可列舉的物件 ([的可列舉物件](classes.md#enumerable-objects))迭代器，或表示反覆項目結束。</span><span class="sxs-lookup"><span data-stu-id="0edb8-628">The `yield` statement is used in an iterator block ([Blocks](statements.md#blocks)) to yield a value to the enumerator object ([Enumerator objects](classes.md#enumerator-objects)) or enumerable object ([Enumerable objects](classes.md#enumerable-objects)) of an iterator or to signal the end of the iteration.</span></span>

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

<span data-ttu-id="0edb8-629">`yield` 不是保留的字;它具有特殊意義之前，立即使用時，才`return`或`break`關鍵字。</span><span class="sxs-lookup"><span data-stu-id="0edb8-629">`yield` is not a reserved word; it has special meaning only when used immediately before a `return` or `break` keyword.</span></span> <span data-ttu-id="0edb8-630">在其他內容中，`yield`可用來當做識別項。</span><span class="sxs-lookup"><span data-stu-id="0edb8-630">In other contexts, `yield` can be used as an identifier.</span></span>

<span data-ttu-id="0edb8-631">有幾項限制，在何處`yield`陳述式可以出現，如下列所述。</span><span class="sxs-lookup"><span data-stu-id="0edb8-631">There are several restrictions on where a `yield` statement can appear, as described in the following.</span></span>

*  <span data-ttu-id="0edb8-632">它是編譯時期錯誤`yield`（的任一形式） 的陳述式才會出現外*method_body*， *operator_body*或*accessor_body*</span><span class="sxs-lookup"><span data-stu-id="0edb8-632">It is a compile-time error for a `yield` statement (of either form) to appear outside a *method_body*, *operator_body* or *accessor_body*</span></span>
*  <span data-ttu-id="0edb8-633">它是編譯時期錯誤`yield`（的任一形式） 的陳述式以匿名函式中出現。</span><span class="sxs-lookup"><span data-stu-id="0edb8-633">It is a compile-time error for a `yield` statement (of either form) to appear inside an anonymous function.</span></span>
*  <span data-ttu-id="0edb8-634">它是編譯時期錯誤`yield`（的任一形式） 的陳述式才會出現在`finally`子句`try`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-634">It is a compile-time error for a `yield` statement (of either form) to appear in the `finally` clause of a `try` statement.</span></span>
*  <span data-ttu-id="0edb8-635">它是編譯時期錯誤`yield return`陳述式出現在任何地方`try`陳述式，其中包含所有`catch`子句。</span><span class="sxs-lookup"><span data-stu-id="0edb8-635">It is a compile-time error for a `yield return` statement to appear anywhere in a `try` statement that contains any `catch` clauses.</span></span>

<span data-ttu-id="0edb8-636">下列範例示範一些有效和無效使用`yield`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-636">The following example shows some valid and invalid uses of `yield` statements.</span></span>

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

<span data-ttu-id="0edb8-637">隱含的轉換 ([隱含轉換](conversions.md#implicit-conversions)) 中的運算式的型別必須存在於`yield return`陳述式來產生型別 ([產生類型](classes.md#yield-type)) 迭代器。</span><span class="sxs-lookup"><span data-stu-id="0edb8-637">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) must exist from the type of the expression in the `yield return` statement to the yield type ([Yield type](classes.md#yield-type)) of the iterator.</span></span>

<span data-ttu-id="0edb8-638">A`yield return`陳述式，如下所示：</span><span class="sxs-lookup"><span data-stu-id="0edb8-638">A `yield return` statement is executed as follows:</span></span>

*  <span data-ttu-id="0edb8-639">陳述式中指定的運算式會評估，以隱含方式轉換產生的型別，並指派給`Current`列舉值物件的屬性。</span><span class="sxs-lookup"><span data-stu-id="0edb8-639">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
*  <span data-ttu-id="0edb8-640">迭代器區塊執行的已暫停。</span><span class="sxs-lookup"><span data-stu-id="0edb8-640">Execution of the iterator block is suspended.</span></span> <span data-ttu-id="0edb8-641">如果`yield return`陳述式位於一或多個`try`區塊中使用，相關聯`finally`區塊不會執行這一次。</span><span class="sxs-lookup"><span data-stu-id="0edb8-641">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
*  <span data-ttu-id="0edb8-642">`MoveNext`列舉值物件的方法會傳回`true`給其呼叫端，指出列舉值物件成功地前移至下一個項目。</span><span class="sxs-lookup"><span data-stu-id="0edb8-642">The `MoveNext` method of the enumerator object returns `true` to its caller, indicating that the enumerator object successfully advanced to the next item.</span></span>

<span data-ttu-id="0edb8-643">Enumerator 物件的下一個呼叫`MoveNext`方法繼續執行從上次已中暫止的迭代器區塊。</span><span class="sxs-lookup"><span data-stu-id="0edb8-643">The next call to the enumerator object's `MoveNext` method resumes execution of the iterator block from where it was last suspended.</span></span>

<span data-ttu-id="0edb8-644">A`yield break`陳述式，如下所示：</span><span class="sxs-lookup"><span data-stu-id="0edb8-644">A `yield break` statement is executed as follows:</span></span>

*  <span data-ttu-id="0edb8-645">如果`yield break`陳述式會包含一或多個`try`區塊相關聯`finally`區塊，控制項一開始會傳送到`finally`的最內層區塊`try`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-645">If the `yield break` statement is enclosed by one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="0edb8-646">時，並控制到達結束點`finally`區塊中，控制傳輸至`finally`區塊下一步 的封入`try`陳述式。</span><span class="sxs-lookup"><span data-stu-id="0edb8-646">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="0edb8-647">此程序會重複直到`finally`區塊的所有封入`try`陳述式執行。</span><span class="sxs-lookup"><span data-stu-id="0edb8-647">This process is repeated until the `finally` blocks of all enclosing `try` statements have been executed.</span></span>
*  <span data-ttu-id="0edb8-648">程式控制權回到呼叫端的迭代器區塊。</span><span class="sxs-lookup"><span data-stu-id="0edb8-648">Control is returned to the caller of the iterator block.</span></span> <span data-ttu-id="0edb8-649">這是`MoveNext`方法或`Dispose`列舉值物件的方法。</span><span class="sxs-lookup"><span data-stu-id="0edb8-649">This is either the `MoveNext` method or `Dispose` method of the enumerator object.</span></span>

<span data-ttu-id="0edb8-650">因為`yield break`陳述式無條件地將控制權傳輸其他位置、 結束點`yield break`陳述式絕不會是可連線。</span><span class="sxs-lookup"><span data-stu-id="0edb8-650">Because a `yield break` statement unconditionally transfers control elsewhere, the end point of a `yield break` statement is never reachable.</span></span>
