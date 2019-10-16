---
ms.openlocfilehash: f61039abd6bd557ac0ea625e6aac1c8bafa57b02
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704081"
---
# <a name="expressions"></a><span data-ttu-id="6aeee-101">運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-101">Expressions</span></span>

<span data-ttu-id="6aeee-102">運算式是運算子和運算元的序列。</span><span class="sxs-lookup"><span data-stu-id="6aeee-102">An expression is a sequence of operators and operands.</span></span> <span data-ttu-id="6aeee-103">本章定義運算元和運算子的語法、評估順序，以及運算式的意義。</span><span class="sxs-lookup"><span data-stu-id="6aeee-103">This chapter defines the syntax, order of evaluation of operands and operators, and meaning of expressions.</span></span>

## <a name="expression-classifications"></a><span data-ttu-id="6aeee-104">運算式分類</span><span class="sxs-lookup"><span data-stu-id="6aeee-104">Expression classifications</span></span>

<span data-ttu-id="6aeee-105">運算式分類為下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="6aeee-105">An expression is classified as one of the following:</span></span>

*  <span data-ttu-id="6aeee-106">一個值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-106">A value.</span></span> <span data-ttu-id="6aeee-107">每個值都有關聯型別。</span><span class="sxs-lookup"><span data-stu-id="6aeee-107">Every value has an associated type.</span></span>
*  <span data-ttu-id="6aeee-108">變數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-108">A variable.</span></span> <span data-ttu-id="6aeee-109">每個變數都有相關聯的類型，亦即變數的宣告類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-109">Every variable has an associated type, namely the declared type of the variable.</span></span>
*  <span data-ttu-id="6aeee-110">命名空間。</span><span class="sxs-lookup"><span data-stu-id="6aeee-110">A namespace.</span></span> <span data-ttu-id="6aeee-111">具有此分類的運算式只能出現在*member_access*的左側（[成員存取權](expressions.md#member-access)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-111">An expression with this classification can only appear as the left hand side of a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="6aeee-112">在任何其他內容中，分類為命名空間的運算式會導致編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-112">In any other context, an expression classified as a namespace causes a compile-time error.</span></span>
*  <span data-ttu-id="6aeee-113">類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-113">A type.</span></span> <span data-ttu-id="6aeee-114">具有此分類的運算式只能顯示為*member_access* （[成員存取](expressions.md#member-access)）的左邊，或做為 `as` 運算子（[as 運算子](expressions.md#the-as-operator)）的運算元、`is` 運算子（[is 運算子](expressions.md#the-is-operator)）或 `typeof` 運算子（[typeof 運算子](expressions.md#the-typeof-operator)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-114">An expression with this classification can only appear as the left hand side of a *member_access* ([Member access](expressions.md#member-access)), or as an operand for the `as` operator ([The as operator](expressions.md#the-as-operator)), the `is` operator ([The is operator](expressions.md#the-is-operator)), or the `typeof` operator ([The typeof operator](expressions.md#the-typeof-operator)).</span></span> <span data-ttu-id="6aeee-115">在任何其他內容中，分類為類型的運算式會導致編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-115">In any other context, an expression classified as a type causes a compile-time error.</span></span>
*  <span data-ttu-id="6aeee-116">方法群組，這是由成員查閱（[成員查閱](expressions.md#member-lookup)）所產生的一組多載方法。</span><span class="sxs-lookup"><span data-stu-id="6aeee-116">A method group, which is a set of overloaded methods resulting from a member lookup ([Member lookup](expressions.md#member-lookup)).</span></span> <span data-ttu-id="6aeee-117">方法群組可以有相關聯的實例運算式和關聯的類型引數清單。</span><span class="sxs-lookup"><span data-stu-id="6aeee-117">A method group may have an associated instance expression and an associated type argument list.</span></span> <span data-ttu-id="6aeee-118">叫用實例方法時，評估實例運算式的結果會成為 `this` （[此存取](expressions.md#this-access)）所表示的實例。</span><span class="sxs-lookup"><span data-stu-id="6aeee-118">When an instance method is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="6aeee-119">*Invocation_expression* （[調用運算式](expressions.md#invocation-expressions)）、 *delegate_creation_expression* （[委派建立運算式](expressions.md#delegate-creation-expressions)）和 is 運算子的左邊都允許方法群組，而且可以隱含地轉換成相容的委派類型（[方法群組轉換](conversions.md#method-group-conversions)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-119">A method group is permitted in an *invocation_expression* ([Invocation expressions](expressions.md#invocation-expressions)) , a *delegate_creation_expression* ([Delegate creation expressions](expressions.md#delegate-creation-expressions)) and as the left hand side of an is operator, and can be implicitly converted to a compatible delegate type ([Method group conversions](conversions.md#method-group-conversions)).</span></span> <span data-ttu-id="6aeee-120">在任何其他內容中，分類為方法群組的運算式會導致編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-120">In any other context, an expression classified as a method group causes a compile-time error.</span></span>
*  <span data-ttu-id="6aeee-121">Null 常值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-121">A null literal.</span></span> <span data-ttu-id="6aeee-122">具有此分類的運算式可以隱含地轉換成參考型別或可為 null 的類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-122">An expression with this classification can be implicitly converted to a reference type or nullable type.</span></span>
*  <span data-ttu-id="6aeee-123">匿名函式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-123">An anonymous function.</span></span> <span data-ttu-id="6aeee-124">具有此分類的運算式可以隱含地轉換成相容的委派類型或運算式樹狀架構類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-124">An expression with this classification can be implicitly converted to a compatible delegate type or expression tree type.</span></span>
*  <span data-ttu-id="6aeee-125">屬性存取。</span><span class="sxs-lookup"><span data-stu-id="6aeee-125">A property access.</span></span> <span data-ttu-id="6aeee-126">每個屬性存取都有相關聯的類型，即屬性的類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-126">Every property access has an associated type, namely the type of the property.</span></span> <span data-ttu-id="6aeee-127">此外，屬性存取可能會有相關聯的實例運算式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-127">Furthermore, a property access may have an associated instance expression.</span></span> <span data-ttu-id="6aeee-128">叫用實例屬性存取的存取子（`get` 或 @no__t 1）時，評估實例運算式的結果會成為 `this` （[此存取](expressions.md#this-access)）所表示的實例。</span><span class="sxs-lookup"><span data-stu-id="6aeee-128">When an accessor (the `get` or `set` block) of an instance property access is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)).</span></span>
*  <span data-ttu-id="6aeee-129">事件存取。</span><span class="sxs-lookup"><span data-stu-id="6aeee-129">An event access.</span></span> <span data-ttu-id="6aeee-130">每個事件存取都有相關聯的類型，亦即事件的類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-130">Every event access has an associated type, namely the type of the event.</span></span> <span data-ttu-id="6aeee-131">此外，事件存取可能會有相關聯的實例運算式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-131">Furthermore, an event access may have an associated instance expression.</span></span> <span data-ttu-id="6aeee-132">事件存取可能會顯示為 `+=` 和 @no__t 1 運算子（[事件指派](expressions.md#event-assignment)）的左運算元。</span><span class="sxs-lookup"><span data-stu-id="6aeee-132">An event access may appear as the left hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="6aeee-133">在任何其他內容中，歸類為事件存取的運算式會導致編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-133">In any other context, an expression classified as an event access causes a compile-time error.</span></span>
*  <span data-ttu-id="6aeee-134">索引子存取。</span><span class="sxs-lookup"><span data-stu-id="6aeee-134">An indexer access.</span></span> <span data-ttu-id="6aeee-135">每個索引子存取都有相關聯的類型，也就是索引子的元素類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-135">Every indexer access has an associated type, namely the element type of the indexer.</span></span> <span data-ttu-id="6aeee-136">此外，索引子存取具有相關聯的實例運算式和相關聯的引數清單。</span><span class="sxs-lookup"><span data-stu-id="6aeee-136">Furthermore, an indexer access has an associated instance expression and an associated argument list.</span></span> <span data-ttu-id="6aeee-137">叫用索引子存取的存取子（`get` 或 @no__t 1）時，評估實例運算式的結果會成為 `this` （[此存取](expressions.md#this-access)）所表示的實例，而且評估引數清單的結果會變成調用的參數清單。</span><span class="sxs-lookup"><span data-stu-id="6aeee-137">When an accessor (the `get` or `set` block) of an indexer access is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)), and the result of evaluating the argument list becomes the parameter list of the invocation.</span></span>
*  <span data-ttu-id="6aeee-138">這裡.</span><span class="sxs-lookup"><span data-stu-id="6aeee-138">Nothing.</span></span> <span data-ttu-id="6aeee-139">當運算式為傳回型別為 `void` 的方法調用時，就會發生這種情況。</span><span class="sxs-lookup"><span data-stu-id="6aeee-139">This occurs when the expression is an invocation of a method with a return type of `void`.</span></span> <span data-ttu-id="6aeee-140">分類為沒有任何內容的運算式僅適用于*statement_expression*的內容（[運算式語句](statements.md#expression-statements)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-140">An expression classified as nothing is only valid in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>

<span data-ttu-id="6aeee-141">運算式的最終結果絕對不是命名空間、類型、方法群組或事件存取。</span><span class="sxs-lookup"><span data-stu-id="6aeee-141">The final result of an expression is never a namespace, type, method group, or event access.</span></span> <span data-ttu-id="6aeee-142">如先前所述，這些運算式類別是只在特定內容中允許的中繼結構。</span><span class="sxs-lookup"><span data-stu-id="6aeee-142">Rather, as noted above, these categories of expressions are intermediate constructs that are only permitted in certain contexts.</span></span>

<span data-ttu-id="6aeee-143">屬性存取或索引子存取一律會藉由執行*get 存取*子或*set 存取*子的調用，重新分類為值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-143">A property access or indexer access is always reclassified as a value by performing an invocation of the *get accessor* or the *set accessor*.</span></span> <span data-ttu-id="6aeee-144">特定存取子是由屬性或索引子存取的內容所決定：如果存取是指派的目標，則會叫用*set 存取*子來指派新的值（[簡單指派](expressions.md#simple-assignment)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-144">The particular accessor is determined by the context of the property or indexer access: If the access is the target of an assignment, the *set accessor* is invoked to assign a new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="6aeee-145">否則，會叫用*get 存取*子來取得目前的值（[運算式的值](expressions.md#values-of-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-145">Otherwise, the *get accessor* is invoked to obtain the current value ([Values of expressions](expressions.md#values-of-expressions)).</span></span>

### <a name="values-of-expressions"></a><span data-ttu-id="6aeee-146">運算式的值</span><span class="sxs-lookup"><span data-stu-id="6aeee-146">Values of expressions</span></span>

<span data-ttu-id="6aeee-147">大部分牽涉到運算式的結構，最後都需要運算式來表示***值***。</span><span class="sxs-lookup"><span data-stu-id="6aeee-147">Most of the constructs that involve an expression ultimately require the expression to denote a ***value***.</span></span> <span data-ttu-id="6aeee-148">在這種情況下，如果實際運算式表示命名空間、類型、方法群組，或沒有任何值，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-148">In such cases, if the actual expression denotes a namespace, a type, a method group, or nothing, a compile-time error occurs.</span></span> <span data-ttu-id="6aeee-149">不過，如果運算式表示屬性存取、索引子存取或變數，則會隱含地取代屬性、索引子或變數的值：</span><span class="sxs-lookup"><span data-stu-id="6aeee-149">However, if the expression denotes a property access, an indexer access, or a variable, the value of the property, indexer, or variable is implicitly substituted:</span></span>

*  <span data-ttu-id="6aeee-150">變數的值只是目前儲存在變數所識別之儲存位置中的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-150">The value of a variable is simply the value currently stored in the storage location identified by the variable.</span></span> <span data-ttu-id="6aeee-151">您必須先將變數視為明確指派（[明確指派](variables.md#definite-assignment)），才能取得其值，否則會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-151">A variable must be considered definitely assigned ([Definite assignment](variables.md#definite-assignment)) before its value can be obtained, or otherwise a compile-time error occurs.</span></span>
*  <span data-ttu-id="6aeee-152">屬性存取運算式的值是藉由叫用屬性的*get 存取*子取得。</span><span class="sxs-lookup"><span data-stu-id="6aeee-152">The value of a property access expression is obtained by invoking the *get accessor* of the property.</span></span> <span data-ttu-id="6aeee-153">如果屬性沒有*get 存取*子，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-153">If the property has no *get accessor*, a compile-time error occurs.</span></span> <span data-ttu-id="6aeee-154">否則，會執行函式成員調用（動態多載[解析的編譯階段檢查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)），而調用的結果會成為屬性存取運算式的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-154">Otherwise, a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is performed, and the result of the invocation becomes the value of the property access expression.</span></span>
*  <span data-ttu-id="6aeee-155">索引子存取運算式的值是藉由叫用索引子的*get 存取*子取得。</span><span class="sxs-lookup"><span data-stu-id="6aeee-155">The value of an indexer access expression is obtained by invoking the *get accessor* of the indexer.</span></span> <span data-ttu-id="6aeee-156">如果索引子沒有*get 存取*子，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-156">If the indexer has no *get accessor*, a compile-time error occurs.</span></span> <span data-ttu-id="6aeee-157">否則，會使用與索引子存取運算式相關聯的引數清單來執行函式成員調用（動態多載[解析的編譯時間檢查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)），而叫用的結果會成為索引子存取的值。運算式.</span><span class="sxs-lookup"><span data-stu-id="6aeee-157">Otherwise, a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is performed with the argument list associated with the indexer access expression, and the result of the invocation becomes the value of the indexer access expression.</span></span>

## <a name="static-and-dynamic-binding"></a><span data-ttu-id="6aeee-158">靜態和動態繫結</span><span class="sxs-lookup"><span data-stu-id="6aeee-158">Static and Dynamic Binding</span></span>

<span data-ttu-id="6aeee-159">根據組成運算式的類型或值（引數、運算元、接收者）來判斷作業意義的程式，通常稱為「系結」（ ***binding***）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-159">The process of determining the meaning of an operation based on the type or value of constituent expressions (arguments, operands, receivers) is often referred to as ***binding***.</span></span> <span data-ttu-id="6aeee-160">例如，方法呼叫的意義是根據接收者和引數的類型來決定。</span><span class="sxs-lookup"><span data-stu-id="6aeee-160">For instance the meaning of a method call is determined based on the type of the receiver and arguments.</span></span> <span data-ttu-id="6aeee-161">運算子的意義取決於其運算元的類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-161">The meaning of an operator is determined based on the type of its operands.</span></span>

<span data-ttu-id="6aeee-162">在C#中，作業的意義通常是在編譯時期根據其組成運算式的編譯時間類型來決定。</span><span class="sxs-lookup"><span data-stu-id="6aeee-162">In C# the meaning of an operation is usually determined at compile-time, based on the compile-time type of its constituent expressions.</span></span> <span data-ttu-id="6aeee-163">同樣地，如果運算式包含錯誤，則會偵測到錯誤並由編譯器回報。</span><span class="sxs-lookup"><span data-stu-id="6aeee-163">Likewise, if an expression contains an error, the error is detected and reported by the compiler.</span></span> <span data-ttu-id="6aeee-164">這種方法稱為***靜態***系結。</span><span class="sxs-lookup"><span data-stu-id="6aeee-164">This approach is known as ***static binding***.</span></span>

<span data-ttu-id="6aeee-165">不過，如果運算式是動態運算式（也就是類型 `dynamic`），則表示它所參與的任何系結都應該根據其執行時間類型（也就是它在執行時間所代表之物件的實際類型），而不是它所擁有的類型。編譯時間。</span><span class="sxs-lookup"><span data-stu-id="6aeee-165">However, if an expression is a dynamic expression (i.e. has the type `dynamic`) this indicates that any binding that it participates in should be based on its run-time type (i.e. the actual type of the object it denotes at run-time) rather than the type it has at compile-time.</span></span> <span data-ttu-id="6aeee-166">因此，這類作業的系結會延後，直到執行程式時發生作業的時間。</span><span class="sxs-lookup"><span data-stu-id="6aeee-166">The binding of such an operation is therefore deferred until the time where the operation is to be executed during the running of the program.</span></span> <span data-ttu-id="6aeee-167">這稱為「***動態繫結***」。</span><span class="sxs-lookup"><span data-stu-id="6aeee-167">This is referred to as ***dynamic binding***.</span></span>

<span data-ttu-id="6aeee-168">當作業動態繫結時，編譯器不會執行任何檢查。</span><span class="sxs-lookup"><span data-stu-id="6aeee-168">When an operation is dynamically bound, little or no checking is performed by the compiler.</span></span> <span data-ttu-id="6aeee-169">相反地，如果執行時間系結失敗，則會在執行時間將錯誤報表為例外狀況。</span><span class="sxs-lookup"><span data-stu-id="6aeee-169">Instead if the run-time binding fails, errors are reported as exceptions at run-time.</span></span>

<span data-ttu-id="6aeee-170">中C#的下列作業受限於系結：</span><span class="sxs-lookup"><span data-stu-id="6aeee-170">The following operations in C# are subject to binding:</span></span>

*  <span data-ttu-id="6aeee-171">成員存取： `e.M`</span><span class="sxs-lookup"><span data-stu-id="6aeee-171">Member access: `e.M`</span></span>
*  <span data-ttu-id="6aeee-172">方法調用： `e.M(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="6aeee-172">Method invocation: `e.M(e1, ..., eN)`</span></span>
*  <span data-ttu-id="6aeee-173">委派調用： `e(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="6aeee-173">Delegate invocation:`e(e1, ..., eN)`</span></span>
*  <span data-ttu-id="6aeee-174">元素存取： `e[e1, ..., eN]`</span><span class="sxs-lookup"><span data-stu-id="6aeee-174">Element access: `e[e1, ..., eN]`</span></span>
*  <span data-ttu-id="6aeee-175">物件建立： `new C(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="6aeee-175">Object creation: `new C(e1, ..., eN)`</span></span>
*  <span data-ttu-id="6aeee-176">多載的一元運算子： `+`，`-`，`!`，`~`，`++`，`--`，`true`，`false`</span><span class="sxs-lookup"><span data-stu-id="6aeee-176">Overloaded unary operators: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`</span></span>
*  <span data-ttu-id="6aeee-177">多載的二元運算子： `+`、`-`、`*`、`/`、`%`、`&`、`&&`、`|`、`||`、`??`、0、1、2、3、4、7、8</span><span class="sxs-lookup"><span data-stu-id="6aeee-177">Overloaded binary operators: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<`, `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`</span></span>
*  <span data-ttu-id="6aeee-178">指派運算子： `=`、`+=`、`-=`、`*=`、`/=`、`%=`、`&=`、`|=`、`^=`、`<<=`、0</span><span class="sxs-lookup"><span data-stu-id="6aeee-178">Assignment operators: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`</span></span>
*  <span data-ttu-id="6aeee-179">隱含和明確轉換</span><span class="sxs-lookup"><span data-stu-id="6aeee-179">Implicit and explicit conversions</span></span>

<span data-ttu-id="6aeee-180">當不涉及動態運算式時， C#預設為靜態系結，這表示選取進程中會使用組成運算式的編譯階段類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-180">When no dynamic expressions are involved, C# defaults to static binding, which means that the compile-time types of constituent expressions are used in the selection process.</span></span> <span data-ttu-id="6aeee-181">不過，以上所列作業中的其中一個組成運算式是動態運算式時，則會改為動態系結運算。</span><span class="sxs-lookup"><span data-stu-id="6aeee-181">However, when one of the constituent expressions in the operations listed above is a dynamic expression, the operation is instead dynamically bound.</span></span>

### <a name="binding-time"></a><span data-ttu-id="6aeee-182">裝訂-時間</span><span class="sxs-lookup"><span data-stu-id="6aeee-182">Binding-time</span></span>

<span data-ttu-id="6aeee-183">靜態系結會在編譯時期進行，而動態系結會在執行時間進行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-183">Static binding takes place at compile-time, whereas dynamic binding takes place at run-time.</span></span> <span data-ttu-id="6aeee-184">在下列各節中，「系結時間」一詞指的是編譯時間或執行時間，***端***視系結髮生的時間而定。</span><span class="sxs-lookup"><span data-stu-id="6aeee-184">In the following sections, the term ***binding-time*** refers to either compile-time or run-time, depending on when the binding takes place.</span></span>

<span data-ttu-id="6aeee-185">下列範例說明靜態和動態系結和系結時間的概念：</span><span class="sxs-lookup"><span data-stu-id="6aeee-185">The following example illustrates the notions of static and dynamic binding and of binding-time:</span></span>
```csharp
object  o = 5;
dynamic d = 5;

Console.WriteLine(5);  // static  binding to Console.WriteLine(int)
Console.WriteLine(o);  // static  binding to Console.WriteLine(object)
Console.WriteLine(d);  // dynamic binding to Console.WriteLine(int)
```

<span data-ttu-id="6aeee-186">前兩個呼叫會以靜態方式系結：根據其引數的編譯時間類型來挑選 `Console.WriteLine` 的多載。</span><span class="sxs-lookup"><span data-stu-id="6aeee-186">The first two calls are statically bound: the overload of `Console.WriteLine` is picked based on the compile-time type of their argument.</span></span> <span data-ttu-id="6aeee-187">因此，系結時間為編譯時間。</span><span class="sxs-lookup"><span data-stu-id="6aeee-187">Thus, the binding-time is compile-time.</span></span>

<span data-ttu-id="6aeee-188">第三個呼叫會以動態方式系結：根據其引數的執行時間類型來挑選 `Console.WriteLine` 的多載。</span><span class="sxs-lookup"><span data-stu-id="6aeee-188">The third call is dynamically bound: the overload of `Console.WriteLine` is picked based on the run-time type of its argument.</span></span> <span data-ttu-id="6aeee-189">之所以會發生這種情況，是因為引數是動態運算式--其編譯時期型別為 `dynamic`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-189">This happens because the argument is a dynamic expression -- its compile-time type is `dynamic`.</span></span> <span data-ttu-id="6aeee-190">因此，第三個呼叫的系結時間是執行時間。</span><span class="sxs-lookup"><span data-stu-id="6aeee-190">Thus, the binding-time for the third call is run-time.</span></span>

### <a name="dynamic-binding"></a><span data-ttu-id="6aeee-191">動態繫結</span><span class="sxs-lookup"><span data-stu-id="6aeee-191">Dynamic binding</span></span>

<span data-ttu-id="6aeee-192">動態系結的目的是允許C#程式與***動態物件***互動，也就是不遵循C#類型系統一般規則的物件。</span><span class="sxs-lookup"><span data-stu-id="6aeee-192">The purpose of dynamic binding is to allow C# programs to interact with ***dynamic objects***, i.e. objects that do not follow the normal rules of the C# type system.</span></span> <span data-ttu-id="6aeee-193">動態物件可能是來自具有不同類型系統之其他程式設計語言的物件，也可能是以程式設計方式設定的物件，以針對不同的作業執行自己的系結語義。</span><span class="sxs-lookup"><span data-stu-id="6aeee-193">Dynamic objects may be objects from other programming languages with different types systems, or they may be objects that are programmatically setup to implement their own binding semantics for different operations.</span></span>

<span data-ttu-id="6aeee-194">動態物件用來實作為其本身的語義的機制是定義的。</span><span class="sxs-lookup"><span data-stu-id="6aeee-194">The mechanism by which a dynamic object implements its own semantics is implementation defined.</span></span> <span data-ttu-id="6aeee-195">定義了指定的介面----由動態物件所實作為，以通知C#執行時間其具有特殊的語義。</span><span class="sxs-lookup"><span data-stu-id="6aeee-195">A given interface -- again implementation defined -- is implemented by dynamic objects to signal to the C# run-time that they have special semantics.</span></span> <span data-ttu-id="6aeee-196">因此，每當動態物件上的作業動態系結時，它們自己的系結語義，而C#不是本檔中所指定的，就會接管。</span><span class="sxs-lookup"><span data-stu-id="6aeee-196">Thus, whenever operations on a dynamic object are dynamically bound, their own binding semantics, rather than those of C# as specified in this document, take over.</span></span>

<span data-ttu-id="6aeee-197">雖然動態系結的目的是要允許與動態物件互通， C#但允許所有物件上的動態系結，不論它們是否為動態的。</span><span class="sxs-lookup"><span data-stu-id="6aeee-197">While the purpose of dynamic binding is to allow interoperation with dynamic objects, C# allows dynamic binding on all objects, whether they are dynamic or not.</span></span> <span data-ttu-id="6aeee-198">這樣可以更順暢地整合動態物件，因為它們的作業結果可能本身不是動態物件，而是程式設計人員在編譯時期的未知型別。</span><span class="sxs-lookup"><span data-stu-id="6aeee-198">This allows for a smoother integration of dynamic objects, as the results of operations on them may not themselves be dynamic objects, but are still of a type unknown to the programmer at compile-time.</span></span> <span data-ttu-id="6aeee-199">此外，動態繫結也有助於消除容易出錯的反映型程式碼，即使沒有任何物件是動態物件也是如此。</span><span class="sxs-lookup"><span data-stu-id="6aeee-199">Also dynamic binding can help eliminate error-prone reflection-based code even when no objects involved are dynamic objects.</span></span>

<span data-ttu-id="6aeee-200">下列各節將針對語言中的每個結構，精確地描述套用動態系結時的架構、編譯時間檢查（如果有的話），以及編譯時間結果和運算式分類。</span><span class="sxs-lookup"><span data-stu-id="6aeee-200">The following sections describe for each construct in the language exactly when dynamic binding is applied, what compile time checking -- if any -- is applied, and what the compile-time result and expression classification is.</span></span>

### <a name="types-of-constituent-expressions"></a><span data-ttu-id="6aeee-201">組成運算式的類型</span><span class="sxs-lookup"><span data-stu-id="6aeee-201">Types of constituent expressions</span></span>

<span data-ttu-id="6aeee-202">當作業以靜態方式系結時，構成運算式的類型（例如接收者、引數、索引或運算元）一律視為該運算式的編譯時間類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-202">When an operation is statically bound, the type of a constituent expression (e.g. a receiver, an argument, an index or an operand) is always considered to be the compile-time type of that expression.</span></span>

<span data-ttu-id="6aeee-203">當作業動態系結時，會根據組成運算式的編譯時間類型，以不同的方式來決定組成運算式的類型：</span><span class="sxs-lookup"><span data-stu-id="6aeee-203">When an operation is dynamically bound, the type of a constituent expression is determined in different ways depending on the compile-time type of the constituent expression:</span></span>

*  <span data-ttu-id="6aeee-204">編譯時期類型 `dynamic` 的構成運算式，會被視為具有運算式在執行時間評估為的實際數值型別。</span><span class="sxs-lookup"><span data-stu-id="6aeee-204">A constituent expression of compile-time type `dynamic` is considered to have the type of the actual value that the expression evaluates to at runtime</span></span>
*  <span data-ttu-id="6aeee-205">其編譯時期型別為型別參數的組成運算式，會被視為具有型別參數在執行時間系結的型別</span><span class="sxs-lookup"><span data-stu-id="6aeee-205">A constituent expression whose compile-time type is a type parameter is considered to have the type which the type parameter is bound to at runtime</span></span>
*  <span data-ttu-id="6aeee-206">否則，構成運算式會被視為具有其編譯時期型別。</span><span class="sxs-lookup"><span data-stu-id="6aeee-206">Otherwise the constituent expression is considered to have its compile-time type.</span></span>

## <a name="operators"></a><span data-ttu-id="6aeee-207">運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-207">Operators</span></span>

<span data-ttu-id="6aeee-208">運算式是由***運算元***和***運算子***所構成。</span><span class="sxs-lookup"><span data-stu-id="6aeee-208">Expressions are constructed from ***operands*** and ***operators***.</span></span> <span data-ttu-id="6aeee-209">運算式的運算子會指出要將哪些運算套用到運算元。</span><span class="sxs-lookup"><span data-stu-id="6aeee-209">The operators of an expression indicate which operations to apply to the operands.</span></span> <span data-ttu-id="6aeee-210">運算子範例包括 `+`、`-`、`*`、`/` 及 `new`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-210">Examples of operators include `+`, `-`, `*`, `/`, and `new`.</span></span> <span data-ttu-id="6aeee-211">運算元範例包括常值、欄位、區域變數及運算式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-211">Examples of operands include literals, fields, local variables, and expressions.</span></span>

<span data-ttu-id="6aeee-212">運算子有三種類型：</span><span class="sxs-lookup"><span data-stu-id="6aeee-212">There are three kinds of operators:</span></span>

*  <span data-ttu-id="6aeee-213">一元運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-213">Unary operators.</span></span> <span data-ttu-id="6aeee-214">一元運算子會採用一個運算元，並使用前置詞標記法（例如 `--x`）或後置標記法（例如 `x++`）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-214">The unary operators take one operand and use either prefix notation (such as `--x`) or postfix notation (such as `x++`).</span></span>
*  <span data-ttu-id="6aeee-215">二元運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-215">Binary operators.</span></span> <span data-ttu-id="6aeee-216">二元運算子接受兩個運算元，並使用中置標記法（例如 `x + y`）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-216">The binary operators take two operands and all use infix notation (such as `x + y`).</span></span>
*  <span data-ttu-id="6aeee-217">三元運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-217">Ternary operator.</span></span> <span data-ttu-id="6aeee-218">只有一個三元運算子 `?:`）存在;它會採用三個運算元，並使用中綴標記法（`c ? x : y`）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-218">Only one ternary operator, `?:`, exists; it takes three operands and uses infix notation (`c ? x : y`).</span></span>

<span data-ttu-id="6aeee-219">運算式中運算子的評估順序是由運算子（[運算子優先順序和關聯](expressions.md#operator-precedence-and-associativity)性）的***優先順序***和***關聯***性所決定。</span><span class="sxs-lookup"><span data-stu-id="6aeee-219">The order of evaluation of operators in an expression is determined by the ***precedence*** and ***associativity*** of the operators ([Operator precedence and associativity](expressions.md#operator-precedence-and-associativity)).</span></span>

<span data-ttu-id="6aeee-220">運算式中的運算元會由左至右評估。</span><span class="sxs-lookup"><span data-stu-id="6aeee-220">Operands in an expression are evaluated from left to right.</span></span> <span data-ttu-id="6aeee-221">例如，在 `F(i) + G(i++) * H(i)` 中，會使用 `i` 的舊值來呼叫方法 `F`，然後以 `i` 的舊值呼叫方法 `G`，最後，會以 `i` 的新值呼叫方法 `H`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-221">For example, in `F(i) + G(i++) * H(i)`, method `F` is called using the old value of `i`, then method `G` is called with the old value of `i`, and, finally, method `H` is called with the new value of `i`.</span></span> <span data-ttu-id="6aeee-222">這與運算子優先順序不同，而且不相關。</span><span class="sxs-lookup"><span data-stu-id="6aeee-222">This is separate from and unrelated to operator precedence.</span></span>

<span data-ttu-id="6aeee-223">部分運算子可以***多載***。</span><span class="sxs-lookup"><span data-stu-id="6aeee-223">Certain operators can be ***overloaded***.</span></span> <span data-ttu-id="6aeee-224">運算子多載可針對一或兩個運算元屬於使用者定義的類別或結構類型（[運算子](expressions.md#operator-overloading)多載）的作業，指定使用者定義的運算子執行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-224">Operator overloading permits user-defined operator implementations to be specified for operations where one or both of the operands are of a user-defined class or struct type ([Operator overloading](expressions.md#operator-overloading)).</span></span>

### <a name="operator-precedence-and-associativity"></a><span data-ttu-id="6aeee-225">運算子優先順序和關聯性</span><span class="sxs-lookup"><span data-stu-id="6aeee-225">Operator precedence and associativity</span></span>

<span data-ttu-id="6aeee-226">當運算式包含多個運算子時，運算子的「優先順序」會控制評估個別運算子的順序。</span><span class="sxs-lookup"><span data-stu-id="6aeee-226">When an expression contains multiple operators, the ***precedence*** of the operators controls the order in which the individual operators are evaluated.</span></span> <span data-ttu-id="6aeee-227">例如，運算式 `x + y * z` 會評估為 `x + (y * z)`，因為 @no__t 2 運算子的優先順序高於 binary `+` 運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-227">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the binary `+` operator.</span></span> <span data-ttu-id="6aeee-228">運算子的優先順序是由其相關聯文法生產的定義所建立。</span><span class="sxs-lookup"><span data-stu-id="6aeee-228">The precedence of an operator is established by the definition of its associated grammar production.</span></span> <span data-ttu-id="6aeee-229">例如， *additive_expression*是由一連串的*multiplicative_expression*組成，以 `+` 或 @no__t 3 的運算子分隔，因此，`+` 和 `-` 運算子的優先順序高於 `*`、`/` 和 `%` 個運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-229">For example, an *additive_expression* consists of a sequence of *multiplicative_expression*s separated by `+` or `-` operators, thus giving the `+` and `-` operators lower precedence than the `*`, `/`, and `%` operators.</span></span>

<span data-ttu-id="6aeee-230">下表依優先順序從最高到最低的順序來匯總所有運算子：</span><span class="sxs-lookup"><span data-stu-id="6aeee-230">The following table summarizes all operators in order of precedence from highest to lowest:</span></span>

| <span data-ttu-id="6aeee-231">__區段__</span><span class="sxs-lookup"><span data-stu-id="6aeee-231">__Section__</span></span>                                                                                   | <span data-ttu-id="6aeee-232">__分類__</span><span class="sxs-lookup"><span data-stu-id="6aeee-232">__Category__</span></span>                | <span data-ttu-id="6aeee-233">__運算子__</span><span class="sxs-lookup"><span data-stu-id="6aeee-233">__Operators__</span></span> | 
|-----------------------------------------------------------------------------------------------|-----------------------------|---------------|
| [<span data-ttu-id="6aeee-234">主要運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-234">Primary expressions</span></span>](expressions.md#primary-expressions)                                     | <span data-ttu-id="6aeee-235">主要</span><span class="sxs-lookup"><span data-stu-id="6aeee-235">Primary</span></span>                     | <span data-ttu-id="6aeee-236">`x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate`</span><span class="sxs-lookup"><span data-stu-id="6aeee-236">`x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate`</span></span> | 
| [<span data-ttu-id="6aeee-237">一元運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-237">Unary operators</span></span>](expressions.md#unary-operators)                                             | <span data-ttu-id="6aeee-238">一元</span><span class="sxs-lookup"><span data-stu-id="6aeee-238">Unary</span></span>                       | <span data-ttu-id="6aeee-239">`+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x`</span><span class="sxs-lookup"><span data-stu-id="6aeee-239">`+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x`</span></span> | 
| [<span data-ttu-id="6aeee-240">算術運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-240">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)                                   | <span data-ttu-id="6aeee-241">乘法類 (Multiplicative)</span><span class="sxs-lookup"><span data-stu-id="6aeee-241">Multiplicative</span></span>              | <span data-ttu-id="6aeee-242">`*`  `/`  `%`</span><span class="sxs-lookup"><span data-stu-id="6aeee-242">`*`  `/`  `%`</span></span> | 
| [<span data-ttu-id="6aeee-243">算術運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-243">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)                                   | <span data-ttu-id="6aeee-244">加法類 (Additive)</span><span class="sxs-lookup"><span data-stu-id="6aeee-244">Additive</span></span>                    | <span data-ttu-id="6aeee-245">`+`  `-`</span><span class="sxs-lookup"><span data-stu-id="6aeee-245">`+`  `-`</span></span>      | 
| [<span data-ttu-id="6aeee-246">移位運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-246">Shift operators</span></span>](expressions.md#shift-operators)                                             | <span data-ttu-id="6aeee-247">Shift</span><span class="sxs-lookup"><span data-stu-id="6aeee-247">Shift</span></span>                       | <span data-ttu-id="6aeee-248">`<<`  `>>`</span><span class="sxs-lookup"><span data-stu-id="6aeee-248">`<<`  `>>`</span></span>    | 
| [<span data-ttu-id="6aeee-249">關係和類型測試運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-249">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators) | <span data-ttu-id="6aeee-250">關係和型別測試</span><span class="sxs-lookup"><span data-stu-id="6aeee-250">Relational and type testing</span></span> | <span data-ttu-id="6aeee-251">`<`  `>`  `<=`  `>=`  `is`  `as`</span><span class="sxs-lookup"><span data-stu-id="6aeee-251">`<`  `>`  `<=`  `>=`  `is`  `as`</span></span> | 
| [<span data-ttu-id="6aeee-252">關係和類型測試運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-252">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators) | <span data-ttu-id="6aeee-253">相等</span><span class="sxs-lookup"><span data-stu-id="6aeee-253">Equality</span></span>                    | <span data-ttu-id="6aeee-254">`==`  `!=`</span><span class="sxs-lookup"><span data-stu-id="6aeee-254">`==`  `!=`</span></span>    | 
| [<span data-ttu-id="6aeee-255">邏輯運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-255">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="6aeee-256">邏輯 AND</span><span class="sxs-lookup"><span data-stu-id="6aeee-256">Logical AND</span></span>                 | `&`           | 
| [<span data-ttu-id="6aeee-257">邏輯運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-257">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="6aeee-258">邏輯 XOR</span><span class="sxs-lookup"><span data-stu-id="6aeee-258">Logical XOR</span></span>                 | `^`           | 
| [<span data-ttu-id="6aeee-259">邏輯運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-259">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="6aeee-260">邏輯 OR</span><span class="sxs-lookup"><span data-stu-id="6aeee-260">Logical OR</span></span>                  | <code>&#124;</code>           |
| [<span data-ttu-id="6aeee-261">條件邏輯運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-261">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)                 | <span data-ttu-id="6aeee-262">條件式 AND</span><span class="sxs-lookup"><span data-stu-id="6aeee-262">Conditional AND</span></span>             | `&&`          | 
| [<span data-ttu-id="6aeee-263">條件邏輯運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-263">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)                 | <span data-ttu-id="6aeee-264">條件式 OR</span><span class="sxs-lookup"><span data-stu-id="6aeee-264">Conditional OR</span></span>              | <code>&#124;&#124;</code>          | 
| [<span data-ttu-id="6aeee-265">Null 聯合運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-265">The null coalescing operator</span></span>](expressions.md#the-null-coalescing-operator)                   | <span data-ttu-id="6aeee-266">Null 聯合</span><span class="sxs-lookup"><span data-stu-id="6aeee-266">Null coalescing</span></span>             | `??`          | 
| [<span data-ttu-id="6aeee-267">條件運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-267">Conditional operator</span></span>](expressions.md#conditional-operator)                                   | <span data-ttu-id="6aeee-268">條件式</span><span class="sxs-lookup"><span data-stu-id="6aeee-268">Conditional</span></span>                 | `?:`          | 
| <span data-ttu-id="6aeee-269">[指派運算子](expressions.md#assignment-operators)，[匿名函數運算式](expressions.md#anonymous-function-expressions)</span><span class="sxs-lookup"><span data-stu-id="6aeee-269">[Assignment operators](expressions.md#assignment-operators), [Anonymous function expressions](expressions.md#anonymous-function-expressions)</span></span>  | <span data-ttu-id="6aeee-270">指派和 lambda 運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-270">Assignment and lambda expression</span></span> | <span data-ttu-id="6aeee-271">`=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>`</span><span class="sxs-lookup"><span data-stu-id="6aeee-271">`=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>`</span></span> | 

<span data-ttu-id="6aeee-272">當兩個具有相同優先順序的運算子之間發生運算元時，運算子的關聯性會控制作業的執行順序：</span><span class="sxs-lookup"><span data-stu-id="6aeee-272">When an operand occurs between two operators with the same precedence, the associativity of the operators controls the order in which the operations are performed:</span></span>

*  <span data-ttu-id="6aeee-273">除了指派運算子和 null 聯合運算子之外，所有二元運算子都是***左關聯***的，這表示作業是由左至右執行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-273">Except for the assignment operators and the null coalescing operator, all binary operators are ***left-associative***, meaning that operations are performed from left to right.</span></span> <span data-ttu-id="6aeee-274">例如，`x + y + z` 會判斷值為 `(x + y) + z`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-274">For example, `x + y + z` is evaluated as `(x + y) + z`.</span></span>
*  <span data-ttu-id="6aeee-275">指派運算子、null 聯合運算子和條件運算子（`?:`）是***靠右關聯***的，這表示作業是由右至左執行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-275">The assignment operators, the null coalescing operator and the conditional operator (`?:`) are ***right-associative***, meaning that operations are performed from right to left.</span></span> <span data-ttu-id="6aeee-276">例如，`x = y = z` 會判斷值為 `x = (y = z)`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-276">For example, `x = y = z` is evaluated as `x = (y = z)`.</span></span>

<span data-ttu-id="6aeee-277">您可以使用括弧來控制優先順序和關聯性。</span><span class="sxs-lookup"><span data-stu-id="6aeee-277">Precedence and associativity can be controlled using parentheses.</span></span> <span data-ttu-id="6aeee-278">例如，`x + y * z` 會先將 `y` 乘以 `z`，然後再將結果加到 `x`，而 `(x + y) * z` 則會先將 `x` 與 `y` 相加，然後再將結果乘以 `z`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-278">For example, `x + y * z` first multiplies `y` by `z` and then adds the result to `x`, but `(x + y) * z` first adds `x` and `y` and then multiplies the result by `z`.</span></span>

### <a name="operator-overloading"></a><span data-ttu-id="6aeee-279">運算子多載</span><span class="sxs-lookup"><span data-stu-id="6aeee-279">Operator overloading</span></span>

<span data-ttu-id="6aeee-280">所有一元和二元運算子都有預先定義的實作為可在任何運算式中自動使用的。</span><span class="sxs-lookup"><span data-stu-id="6aeee-280">All unary and binary operators have predefined implementations that are automatically available in any expression.</span></span> <span data-ttu-id="6aeee-281">除了預先定義的執行之外，也可以在類別和結構（[運算子](classes.md#operators)）中包含 `operator` 宣告，藉以引進使用者定義的實作為功能。</span><span class="sxs-lookup"><span data-stu-id="6aeee-281">In addition to the predefined implementations, user-defined implementations can be introduced by including `operator` declarations in classes and structs ([Operators](classes.md#operators)).</span></span> <span data-ttu-id="6aeee-282">使用者定義的運算子實作為優先于預先定義的運算子實現：只有在沒有適用的使用者定義運算子實作為時，才會考慮預先定義的運算子實作為，如[一元運算子](expressions.md#unary-operator-overload-resolution)多載解析和[二元運算子](expressions.md#binary-operator-overload-resolution)多載解析中所述。</span><span class="sxs-lookup"><span data-stu-id="6aeee-282">User-defined operator implementations always take precedence over predefined operator implementations: Only when no applicable user-defined operator implementations exist will the predefined operator implementations be considered, as described in [Unary operator overload resolution](expressions.md#unary-operator-overload-resolution) and [Binary operator overload resolution](expressions.md#binary-operator-overload-resolution).</span></span>

<span data-ttu-id="6aeee-283">可多載的***一元運算子***為：</span><span class="sxs-lookup"><span data-stu-id="6aeee-283">The ***overloadable unary operators*** are:</span></span>
```csharp
+   -   !   ~   ++   --   true   false
```

<span data-ttu-id="6aeee-284">雖然 `true` 和 `false` 不會在運算式中明確使用（因此不會包含在[運算子優先順序和關聯](expressions.md#operator-precedence-and-associativity)性的優先順序資料表中），但它們會被視為運算子，因為它們是在數個運算式中叫用的內容：布林運算式（[布林運算式](expressions.md#boolean-expressions)）和包含條件式（[條件運算子](expressions.md#conditional-operator)）的運算式，以及條件式邏輯運算子（[條件式邏輯運算子](expressions.md#conditional-logical-operators)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-284">Although `true` and `false` are not used explicitly in expressions (and therefore are not included in the precedence table in [Operator precedence and associativity](expressions.md#operator-precedence-and-associativity)), they are considered operators because they are invoked in several expression contexts: boolean expressions ([Boolean expressions](expressions.md#boolean-expressions)) and expressions involving the conditional ([Conditional operator](expressions.md#conditional-operator)), and conditional logical operators ([Conditional logical operators](expressions.md#conditional-logical-operators)).</span></span>

<span data-ttu-id="6aeee-285">可多載的***二元運算子***為：</span><span class="sxs-lookup"><span data-stu-id="6aeee-285">The ***overloadable binary operators*** are:</span></span>
```csharp
+   -   *   /   %   &   |   ^   <<   >>   ==   !=   >   <   >=   <=
```

<span data-ttu-id="6aeee-286">只有上列運算子可以多載。</span><span class="sxs-lookup"><span data-stu-id="6aeee-286">Only the operators listed above can be overloaded.</span></span> <span data-ttu-id="6aeee-287">特別是，您無法多載成員存取、方法叫用或 `=`、`&&`、`||`、`??`、`?:`、`=>`、`checked`、`unchecked`、`new`、`typeof`、0、1 和 2 運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-287">In particular, it is not possible to overload member access, method invocation, or the `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, `default`, `as`, and `is` operators.</span></span>

<span data-ttu-id="6aeee-288">二元運算子多載時，對應的指派運算子 (若有) 也會隱含地多載。</span><span class="sxs-lookup"><span data-stu-id="6aeee-288">When a binary operator is overloaded, the corresponding assignment operator, if any, is also implicitly overloaded.</span></span> <span data-ttu-id="6aeee-289">例如，運算子的多載 `*` 也是運算子 `*=` 的多載。</span><span class="sxs-lookup"><span data-stu-id="6aeee-289">For example, an overload of operator `*` is also an overload of operator `*=`.</span></span> <span data-ttu-id="6aeee-290">這會在[複合指派](expressions.md#compound-assignment)中進一步說明。</span><span class="sxs-lookup"><span data-stu-id="6aeee-290">This is described further in [Compound assignment](expressions.md#compound-assignment).</span></span> <span data-ttu-id="6aeee-291">請注意，指派運算子本身（`=`）無法多載。</span><span class="sxs-lookup"><span data-stu-id="6aeee-291">Note that the assignment operator itself (`=`) cannot be overloaded.</span></span> <span data-ttu-id="6aeee-292">指派一律會對變數執行簡單的位複製值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-292">An assignment always performs a simple bit-wise copy of a value into a variable.</span></span>

<span data-ttu-id="6aeee-293">轉換作業（例如 `(T)x`）是藉由提供使用者定義的轉換（[使用者定義的轉換](conversions.md#user-defined-conversions)）而多載。</span><span class="sxs-lookup"><span data-stu-id="6aeee-293">Cast operations, such as `(T)x`, are overloaded by providing user-defined conversions ([User-defined conversions](conversions.md#user-defined-conversions)).</span></span>

<span data-ttu-id="6aeee-294">元素存取（例如 `a[x]`）不會被視為可多載運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-294">Element access, such as `a[x]`, is not considered an overloadable operator.</span></span> <span data-ttu-id="6aeee-295">而是透過索引子（[索引子](classes.md#indexers)）來支援使用者定義的索引。</span><span class="sxs-lookup"><span data-stu-id="6aeee-295">Instead, user-defined indexing is supported through indexers ([Indexers](classes.md#indexers)).</span></span>

<span data-ttu-id="6aeee-296">在運算式中，運算子是使用運算子標記法來參考，而在宣告中，運算子則是使用功能性標記法來參考。</span><span class="sxs-lookup"><span data-stu-id="6aeee-296">In expressions, operators are referenced using operator notation, and in declarations, operators are referenced using functional notation.</span></span> <span data-ttu-id="6aeee-297">下表顯示一元和二元運算子的運算子和功能標記法之間的關聯性。</span><span class="sxs-lookup"><span data-stu-id="6aeee-297">The following table shows the relationship between operator and functional notations for unary and binary operators.</span></span> <span data-ttu-id="6aeee-298">在第一個專案中， *op*代表任何多載的一元前置運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-298">In the first entry, *op* denotes any overloadable unary prefix operator.</span></span> <span data-ttu-id="6aeee-299">在第二個專案中， *op*表示一元後置 `++` 和 @no__t 2 運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-299">In the second entry, *op* denotes the unary postfix `++` and `--` operators.</span></span> <span data-ttu-id="6aeee-300">在第三個專案中， *op*表示任何可多載的二元運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-300">In the third entry, *op* denotes any overloadable binary operator.</span></span>


| <span data-ttu-id="6aeee-301">__運算子標記法__</span><span class="sxs-lookup"><span data-stu-id="6aeee-301">__Operator notation__</span></span> | <span data-ttu-id="6aeee-302">__功能標記法__</span><span class="sxs-lookup"><span data-stu-id="6aeee-302">__Functional notation__</span></span> |
|-----------------------|-------------------------|
| `op x`                | `operator op(x)`        | 
| `x op`                | `operator op(x)`        | 
| `x op y`              | `operator op(x,y)`      | 

<span data-ttu-id="6aeee-303">使用者定義的運算子宣告一律需要至少其中一個參數屬於包含運算子宣告的類別或結構類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-303">User-defined operator declarations always require at least one of the parameters to be of the class or struct type that contains the operator declaration.</span></span> <span data-ttu-id="6aeee-304">因此，使用者定義的運算子不可能具有與預先定義之運算子相同的簽章。</span><span class="sxs-lookup"><span data-stu-id="6aeee-304">Thus, it is not possible for a user-defined operator to have the same signature as a predefined operator.</span></span>

<span data-ttu-id="6aeee-305">使用者定義的運算子宣告無法修改運算子的語法、優先順序或關聯性。</span><span class="sxs-lookup"><span data-stu-id="6aeee-305">User-defined operator declarations cannot modify the syntax, precedence, or associativity of an operator.</span></span> <span data-ttu-id="6aeee-306">例如，`/` 運算子一律是二元運算子，一律具有在[運算子優先順序和關聯](expressions.md#operator-precedence-and-associativity)性中指定的優先順序層級，而且一律為左關聯。</span><span class="sxs-lookup"><span data-stu-id="6aeee-306">For example, the `/` operator is always a binary operator, always has the precedence level specified in [Operator precedence and associativity](expressions.md#operator-precedence-and-associativity), and is always left-associative.</span></span>

<span data-ttu-id="6aeee-307">雖然使用者定義的運算子可以執行其所 pleases 的任何計算，但強烈建議您不要採用以直覺方式產生的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-307">While it is possible for a user-defined operator to perform any computation it pleases, implementations that produce results other than those that are intuitively expected are strongly discouraged.</span></span> <span data-ttu-id="6aeee-308">例如，`operator ==` 的執行應該比較兩個運算元是否相等，並傳回適當的 @no__t 1 結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-308">For example, an implementation of `operator ==` should compare the two operands for equality and return an appropriate `bool` result.</span></span>

<span data-ttu-id="6aeee-309">透過[條件式邏輯運算子](expressions.md#conditional-logical-operators)在[主要運算式](expressions.md#primary-expressions)中個別運算子的描述會指定運算子的預先定義的執行，以及適用于每個運算子的任何其他規則。</span><span class="sxs-lookup"><span data-stu-id="6aeee-309">The descriptions of individual operators in [Primary expressions](expressions.md#primary-expressions) through [Conditional logical operators](expressions.md#conditional-logical-operators) specify the predefined implementations of the operators and any additional rules that apply to each operator.</span></span> <span data-ttu-id="6aeee-310">這些描述會使用***一元運算子***多載解析、***二元運算子***多載解析和***數值提升***的詞彙，其定義可在下列各節中找到。</span><span class="sxs-lookup"><span data-stu-id="6aeee-310">The descriptions make use of the terms ***unary operator overload resolution***, ***binary operator overload resolution***, and ***numeric promotion***, definitions of which are found in the following sections.</span></span>

### <a name="unary-operator-overload-resolution"></a><span data-ttu-id="6aeee-311">一元運算子多載解析</span><span class="sxs-lookup"><span data-stu-id="6aeee-311">Unary operator overload resolution</span></span>

<span data-ttu-id="6aeee-312">格式為 `op x` 或 `x op` 的作業，其中 `op` 是可多載的一元運算子，而 `x` 是類型 `X` 的運算式，其處理方式如下：</span><span class="sxs-lookup"><span data-stu-id="6aeee-312">An operation of the form `op x` or `x op`, where `op` is an overloadable unary operator, and `x` is an expression of type `X`, is processed as follows:</span></span>

*  <span data-ttu-id="6aeee-313">@No__t-0 針對作業所提供的候選使用者定義運算子集合 `operator op(x)` 是使用[候選使用者定義運算子](expressions.md#candidate-user-defined-operators)的規則來決定。</span><span class="sxs-lookup"><span data-stu-id="6aeee-313">The set of candidate user-defined operators provided by `X` for the operation `operator op(x)` is determined using the rules of [Candidate user-defined operators](expressions.md#candidate-user-defined-operators).</span></span>
*  <span data-ttu-id="6aeee-314">如果候選使用者定義的運算子集合不是空的，則這會成為作業的候選運算子集。</span><span class="sxs-lookup"><span data-stu-id="6aeee-314">If the set of candidate user-defined operators is not empty, then this becomes the set of candidate operators for the operation.</span></span> <span data-ttu-id="6aeee-315">否則，預先定義的一元 @no__t 0 的實作為，包括其提升形式，會變成作業的候選運算子集合。</span><span class="sxs-lookup"><span data-stu-id="6aeee-315">Otherwise, the predefined unary `operator op` implementations, including their lifted forms, become the set of candidate operators for the operation.</span></span> <span data-ttu-id="6aeee-316">指定運算子的預先定義實作為運算子（[主要運算式](expressions.md#primary-expressions)和[一元運算子](expressions.md#unary-operators)）的描述。</span><span class="sxs-lookup"><span data-stu-id="6aeee-316">The predefined implementations of a given operator are specified in the description of the operator ([Primary expressions](expressions.md#primary-expressions) and [Unary operators](expressions.md#unary-operators)).</span></span>
*  <span data-ttu-id="6aeee-317">多載解析的多載解析[規則會套用](expressions.md#overload-resolution)至候選運算子集合，以選取與引數清單 `(x)` 相關的最佳運算子，而此運算子會成為多載解析程式的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-317">The overload resolution rules of [Overload resolution](expressions.md#overload-resolution) are applied to the set of candidate operators to select the best operator with respect to the argument list `(x)`, and this operator becomes the result of the overload resolution process.</span></span> <span data-ttu-id="6aeee-318">如果多載解析無法選取單一最佳運算子，則會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-318">If overload resolution fails to select a single best operator, a binding-time error occurs.</span></span>

### <a name="binary-operator-overload-resolution"></a><span data-ttu-id="6aeee-319">二元運算子多載解析</span><span class="sxs-lookup"><span data-stu-id="6aeee-319">Binary operator overload resolution</span></span>

<span data-ttu-id="6aeee-320">格式為 `x op y` 的作業，其中 `op` 是可多載的二元運算子，而 `x` 是類型 `X` 的運算式，而 `y` 是類型 `Y` 的運算式，其處理方式如下：</span><span class="sxs-lookup"><span data-stu-id="6aeee-320">An operation of the form `x op y`, where `op` is an overloadable binary operator, `x` is an expression of type `X`, and `y` is an expression of type `Y`, is processed as follows:</span></span>

*  <span data-ttu-id="6aeee-321">已決定作業 `operator op(x,y)` 所提供的候選使用者定義運算子集合 `X` 和 `Y`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-321">The set of candidate user-defined operators provided by `X` and `Y` for the operation `operator op(x,y)` is determined.</span></span> <span data-ttu-id="6aeee-322">集合是由 `X` 所提供之候選運算子的聯集，以及 `Y` 所提供的候選運算子組成，每一個都是使用[候選使用者定義運算子](expressions.md#candidate-user-defined-operators)的規則來決定。</span><span class="sxs-lookup"><span data-stu-id="6aeee-322">The set consists of the union of the candidate operators provided by `X` and the candidate operators provided by `Y`, each determined using the rules of [Candidate user-defined operators](expressions.md#candidate-user-defined-operators).</span></span> <span data-ttu-id="6aeee-323">如果 `X` 和 `Y` 是相同的類型，或如果 `X` 和 `Y` 衍生自通用基底類型，則共用候選運算子只會出現在結合的集合一次。</span><span class="sxs-lookup"><span data-stu-id="6aeee-323">If `X` and `Y` are the same type, or if `X` and `Y` are derived from a common base type, then shared candidate operators only occur in the combined set once.</span></span>
*  <span data-ttu-id="6aeee-324">如果候選使用者定義的運算子集合不是空的，則這會成為作業的候選運算子集。</span><span class="sxs-lookup"><span data-stu-id="6aeee-324">If the set of candidate user-defined operators is not empty, then this becomes the set of candidate operators for the operation.</span></span> <span data-ttu-id="6aeee-325">否則，預先定義的二進位 @no__t 0 的實作為，包括其提升的形式，就會變成作業的候選運算子集。</span><span class="sxs-lookup"><span data-stu-id="6aeee-325">Otherwise, the predefined binary `operator op` implementations, including their lifted forms,  become the set of candidate operators for the operation.</span></span> <span data-ttu-id="6aeee-326">指定運算子的預先定義實作為運算子（透過[條件式邏輯運算子](expressions.md#conditional-logical-operators)的[算術運算子](expressions.md#arithmetic-operators)）的描述。</span><span class="sxs-lookup"><span data-stu-id="6aeee-326">The predefined implementations of a given operator are specified in the description of the operator ([Arithmetic operators](expressions.md#arithmetic-operators) through [Conditional logical operators](expressions.md#conditional-logical-operators)).</span></span> <span data-ttu-id="6aeee-327">針對預先定義的列舉和委派運算子，唯一會考慮的運算子是由列舉或委派型別所定義，這是其中一個運算元的系結時間型別。</span><span class="sxs-lookup"><span data-stu-id="6aeee-327">For predefined enum and delegate operators, the only operators considered are those defined by an enum or delegate type that is the binding-time type of one of the operands.</span></span>
*  <span data-ttu-id="6aeee-328">多載解析的多載解析[規則會套用](expressions.md#overload-resolution)至候選運算子集合，以選取與引數清單 `(x,y)` 相關的最佳運算子，而此運算子會成為多載解析程式的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-328">The overload resolution rules of [Overload resolution](expressions.md#overload-resolution) are applied to the set of candidate operators to select the best operator with respect to the argument list `(x,y)`, and this operator becomes the result of the overload resolution process.</span></span> <span data-ttu-id="6aeee-329">如果多載解析無法選取單一最佳運算子，則會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-329">If overload resolution fails to select a single best operator, a binding-time error occurs.</span></span>

### <a name="candidate-user-defined-operators"></a><span data-ttu-id="6aeee-330">候選使用者定義的運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-330">Candidate user-defined operators</span></span>

<span data-ttu-id="6aeee-331">假設類型 `T`，而作業 `operator op(A)`，其中 `op` 是可多載的運算子，而 `A` 是引數清單，則由 `T` 為 @no__t 所提供的候選使用者定義運算子集合如下所示：</span><span class="sxs-lookup"><span data-stu-id="6aeee-331">Given a type `T` and an operation `operator op(A)`, where `op` is an overloadable operator and `A` is an argument list, the set of candidate user-defined operators provided by `T` for `operator op(A)` is determined as follows:</span></span>

*  <span data-ttu-id="6aeee-332">判斷類型 `T0`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-332">Determine the type `T0`.</span></span> <span data-ttu-id="6aeee-333">如果 `T` 是可為 null 的型別，`T0` 是其基礎型別，否則 `T0` 等於 `T`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-333">If `T` is a nullable type, `T0` is its underlying type, otherwise `T0` is equal to `T`.</span></span>
*  <span data-ttu-id="6aeee-334">對於 `T0` 中的所有 `operator op` 宣告，以及這類運算子的所有提升形式，如果至少有一個運算子[適用于](expressions.md#applicable-function-member)引數清單 `A`，則候選運算子集合會包含所有這類的@no__t 中適用的運算子-4。</span><span class="sxs-lookup"><span data-stu-id="6aeee-334">For all `operator op` declarations in `T0` and all lifted forms of such operators, if at least one operator is applicable ([Applicable function member](expressions.md#applicable-function-member)) with respect to the argument list `A`, then the set of candidate operators consists of all such applicable operators in `T0`.</span></span>
*  <span data-ttu-id="6aeee-335">否則，如果 `T0` 是 `object`，候選運算子的集合就會是空的。</span><span class="sxs-lookup"><span data-stu-id="6aeee-335">Otherwise, if `T0` is `object`, the set of candidate operators is empty.</span></span>
*  <span data-ttu-id="6aeee-336">否則，`T0` 所提供的候選運算子集合就是由 `T0` 的直接基類所提供的候選運算子集合; 如果 `T0` 是類型參數，則為 `T0` 的有效基底類別。</span><span class="sxs-lookup"><span data-stu-id="6aeee-336">Otherwise, the set of candidate operators provided by `T0` is the set of candidate operators provided by the direct base class of `T0`, or the effective base class of `T0` if `T0` is a type parameter.</span></span>

### <a name="numeric-promotions"></a><span data-ttu-id="6aeee-337">數值升級</span><span class="sxs-lookup"><span data-stu-id="6aeee-337">Numeric promotions</span></span>

<span data-ttu-id="6aeee-338">數值提升包含自動執行預先定義的一元和二元數值運算子之運算元的某些隱含轉換。</span><span class="sxs-lookup"><span data-stu-id="6aeee-338">Numeric promotion consists of automatically performing certain implicit conversions of the operands of the predefined unary and binary numeric operators.</span></span> <span data-ttu-id="6aeee-339">數值提升並不是不同的機制，而是將多載解析套用至預先定義之運算子的效果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-339">Numeric promotion is not a distinct mechanism, but rather an effect of applying overload resolution to the predefined operators.</span></span> <span data-ttu-id="6aeee-340">數值升級特別不會影響使用者定義運算子的評估，雖然可以實作為使用者定義的運算子來呈現類似的效果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-340">Numeric promotion specifically does not affect evaluation of user-defined operators, although user-defined operators can be implemented to exhibit similar effects.</span></span>

<span data-ttu-id="6aeee-341">作為數值升級的範例，請考慮二元 `*` 運算子的預先定義的執行：</span><span class="sxs-lookup"><span data-stu-id="6aeee-341">As an example of numeric promotion, consider the predefined implementations of the binary `*` operator:</span></span>

```csharp
int operator *(int x, int y);
uint operator *(uint x, uint y);
long operator *(long x, long y);
ulong operator *(ulong x, ulong y);
float operator *(float x, float y);
double operator *(double x, double y);
decimal operator *(decimal x, decimal y);
```

<span data-ttu-id="6aeee-342">當多載解析規則（多載[解析](expressions.md#overload-resolution)）套用至這組運算子時，其效果會從運算元類型中選取隱含轉換存在的第一個運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-342">When overload resolution rules ([Overload resolution](expressions.md#overload-resolution)) are applied to this set of operators, the effect is to select the first of the operators for which implicit conversions exist from the operand types.</span></span> <span data-ttu-id="6aeee-343">例如，針對作業 `b * s`，其中 `b` 是 `byte`，而 `s` 是 `short`，多載解析會選取 `operator *(int,int)` 做為最佳運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-343">For example, for the operation `b * s`, where `b` is a `byte` and `s` is a `short`, overload resolution selects `operator *(int,int)` as the best operator.</span></span> <span data-ttu-id="6aeee-344">因此，效果是 `b` 和 `s` 會轉換成 `int`，而結果的類型會 `int`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-344">Thus, the effect is that `b` and `s` are converted to `int`, and the type of the result is `int`.</span></span> <span data-ttu-id="6aeee-345">同樣地，針對作業 `i * d`，其中 `i` 是 `int`，而 `d` 是 @no__t 4，多載解析會選取 `operator *(double,double)` 做為最佳運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-345">Likewise, for the operation `i * d`, where `i` is an `int` and `d` is a `double`, overload resolution selects `operator *(double,double)` as the best operator.</span></span>

#### <a name="unary-numeric-promotions"></a><span data-ttu-id="6aeee-346">一元數值提升</span><span class="sxs-lookup"><span data-stu-id="6aeee-346">Unary numeric promotions</span></span>

<span data-ttu-id="6aeee-347">預先定義的 `+`、`-` 和 @no__t 2 一元運算子的運算元會進行一元數值升級。</span><span class="sxs-lookup"><span data-stu-id="6aeee-347">Unary numeric promotion occurs for the operands of the predefined `+`, `-`, and `~` unary operators.</span></span> <span data-ttu-id="6aeee-348">一元數值提升只包含將類型的運算元（`sbyte`、`byte`、`short`、`ushort` 或 `char` 轉換為類型 `int`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-348">Unary numeric promotion simply consists of converting operands of type `sbyte`, `byte`, `short`, `ushort`, or `char` to type `int`.</span></span> <span data-ttu-id="6aeee-349">此外，對於一元 `-` 運算子，一元數值提升會將類型 `uint` 的運算元轉換為類型 `long`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-349">Additionally, for the unary `-` operator, unary numeric promotion converts operands of type `uint` to type `long`.</span></span>

#### <a name="binary-numeric-promotions"></a><span data-ttu-id="6aeee-350">二進位數值升級</span><span class="sxs-lookup"><span data-stu-id="6aeee-350">Binary numeric promotions</span></span>

<span data-ttu-id="6aeee-351">預先定義的 `+`、`-`、`*`、`/`、`%`、`&`、`|`、`^`、`==`、`!=`、0、1、2 和 3 二元運算子的運算元，會進行二進位數值升級。</span><span class="sxs-lookup"><span data-stu-id="6aeee-351">Binary numeric promotion occurs for the operands of the predefined `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, `>`, `<`, `>=`, and `<=` binary operators.</span></span> <span data-ttu-id="6aeee-352">二進位數值提升會隱含地將兩個運算元轉換成一般類型，如果是非關聯式運算子，也會變成作業的結果類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-352">Binary numeric promotion implicitly converts both operands to a common type which, in case of the non-relational operators, also becomes the result type of the operation.</span></span> <span data-ttu-id="6aeee-353">二進位數值提升包含套用下列規則，順序如下所示：</span><span class="sxs-lookup"><span data-stu-id="6aeee-353">Binary numeric promotion consists of applying the following rules, in the order they appear here:</span></span>

*  <span data-ttu-id="6aeee-354">如果任一個運算元的類型為 `decimal`，則會將另一個運算元轉換成類型 `decimal`; 如果另一個運算元的類型為 `float` 或 `double`，則會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-354">If either operand is of type `decimal`, the other operand is converted to type `decimal`, or a binding-time error occurs if the other operand is of type `float` or `double`.</span></span>
*  <span data-ttu-id="6aeee-355">否則，如果任一運算元的類型為 `double`，則會將另一個運算元轉換成類型 `double`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-355">Otherwise, if either operand is of type `double`, the other operand is converted to type `double`.</span></span>
*  <span data-ttu-id="6aeee-356">否則，如果任一運算元的類型為 `float`，則會將另一個運算元轉換成類型 `float`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-356">Otherwise, if either operand is of type `float`, the other operand is converted to type `float`.</span></span>
*  <span data-ttu-id="6aeee-357">否則，如果任一運算元的類型為 `ulong`，則會將另一個運算元轉換成類型 `ulong`，或如果另一個運算元的類型為 `sbyte`、`short`、`int` 或 `long`，就會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-357">Otherwise, if either operand is of type `ulong`, the other operand is converted to type `ulong`, or a binding-time error occurs if the other operand is of type `sbyte`, `short`, `int`, or `long`.</span></span>
*  <span data-ttu-id="6aeee-358">否則，如果任一運算元的類型為 `long`，則會將另一個運算元轉換成類型 `long`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-358">Otherwise, if either operand is of type `long`, the other operand is converted to type `long`.</span></span>
*  <span data-ttu-id="6aeee-359">否則，如果任一運算元的類型為 `uint`，而另一個運算元的類型為 `sbyte`、`short` 或 `int`，這兩個運算元都會轉換成類型 `long`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-359">Otherwise, if either operand is of type `uint` and the other operand is of type `sbyte`, `short`, or `int`, both operands are converted to type `long`.</span></span>
*  <span data-ttu-id="6aeee-360">否則，如果任一運算元的類型為 `uint`，則會將另一個運算元轉換成類型 `uint`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-360">Otherwise, if either operand is of type `uint`, the other operand is converted to type `uint`.</span></span>
*  <span data-ttu-id="6aeee-361">否則，兩個運算元都會轉換成類型 `int`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-361">Otherwise, both operands are converted to type `int`.</span></span>

<span data-ttu-id="6aeee-362">請注意，第一個規則不允許混用 `decimal` 類型與 `double` 和 @no__t 2 類型的任何作業。</span><span class="sxs-lookup"><span data-stu-id="6aeee-362">Note that the first rule disallows any operations that mix the `decimal` type with the `double` and `float` types.</span></span> <span data-ttu-id="6aeee-363">此規則的原因是，`decimal` 類型與 `double` 和 @no__t 2 類型之間沒有隱含轉換。</span><span class="sxs-lookup"><span data-stu-id="6aeee-363">The rule follows from the fact that there are no implicit conversions between the `decimal` type and the `double` and `float` types.</span></span>

<span data-ttu-id="6aeee-364">另請注意，當另一個運算元屬於帶正負號的整數類資料類型時，運算元不可能屬於類型 `ulong`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-364">Also note that it is not possible for an operand to be of type `ulong` when the other operand is of a signed integral type.</span></span> <span data-ttu-id="6aeee-365">原因是沒有可代表完整範圍 `ulong` 的整數類資料類型，以及帶正負號的整數類資料類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-365">The reason is that no integral type exists that can represent the full range of `ulong` as well as the signed integral types.</span></span>

<span data-ttu-id="6aeee-366">在上述兩種情況下，cast 運算式可以用來將一個運算元明確轉換成與另一個運算元相容的類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-366">In both of the above cases, a cast expression can be used to explicitly convert one operand to a type that is compatible with the other operand.</span></span>

<span data-ttu-id="6aeee-367">在範例中</span><span class="sxs-lookup"><span data-stu-id="6aeee-367">In the example</span></span>
```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (1.0 + percent / 100.0);
}
```
<span data-ttu-id="6aeee-368">發生系結時錯誤，因為 `decimal` 無法乘以 `double`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-368">a binding-time error occurs because a `decimal` cannot be multiplied by a `double`.</span></span> <span data-ttu-id="6aeee-369">錯誤的解決方式是將第二個運算元明確轉換為 `decimal`，如下所示：</span><span class="sxs-lookup"><span data-stu-id="6aeee-369">The error is resolved by explicitly converting the second operand to `decimal`, as follows:</span></span>

```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (decimal)(1.0 + percent / 100.0);
}
```

### <a name="lifted-operators"></a><span data-ttu-id="6aeee-370">提升運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-370">Lifted operators</span></span>

<span data-ttu-id="6aeee-371">***提升運算子***允許在不可為 null 的實數值型別上運作的預先定義和使用者定義運算子，也可以搭配這些類型的可為 null 形式來使用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-371">***Lifted operators*** permit predefined and user-defined operators that operate on non-nullable value types to also be used with nullable forms of those types.</span></span> <span data-ttu-id="6aeee-372">提升運算子是根據符合特定需求的預先定義和使用者定義運算子所建立，如下所述：</span><span class="sxs-lookup"><span data-stu-id="6aeee-372">Lifted operators are constructed from predefined and user-defined operators that meet certain requirements, as described in the following:</span></span>

*   <span data-ttu-id="6aeee-373">一元運算子的</span><span class="sxs-lookup"><span data-stu-id="6aeee-373">For the unary operators</span></span>

    ```csharp
    +  ++  -  --  !  ~
    ```

    <span data-ttu-id="6aeee-374">如果運算元和結果類型都是不可為 null 的實數值型別，則會有一種形式的運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-374">a lifted form of an operator exists if the operand and result types are both non-nullable value types.</span></span> <span data-ttu-id="6aeee-375">藉由將單一的 `?` 修飾詞加入運算元和結果類型，即可建立此提升形式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-375">The lifted form is constructed by adding a single `?` modifier to the operand and result types.</span></span> <span data-ttu-id="6aeee-376">如果運算元為 null，則提升運算子會產生 null 值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-376">The lifted operator produces a null value if the operand is null.</span></span> <span data-ttu-id="6aeee-377">否則，提升運算子會解除包裝運算元、套用基礎運算子，並將結果包裝起來。</span><span class="sxs-lookup"><span data-stu-id="6aeee-377">Otherwise, the lifted operator unwraps the operand, applies the underlying operator, and wraps the result.</span></span>

*   <span data-ttu-id="6aeee-378">二元運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-378">For the binary operators</span></span>

    ```csharp
    +  -  *  /  %  &  |  ^  <<  >>
    ```

    <span data-ttu-id="6aeee-379">如果運算元和結果類型都是不可為 null 的實數值型別，則會有一種形式的運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-379">a lifted form of an operator exists if the operand and result types are all non-nullable value types.</span></span> <span data-ttu-id="6aeee-380">藉由將單一的 `?` 修飾詞加入至每個運算元和結果類型，即可建立此提升格式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-380">The lifted form is constructed by adding a single `?` modifier to each operand and result type.</span></span> <span data-ttu-id="6aeee-381">如果其中一個或兩個運算元都是 null （例外狀況是 `bool?` 類型的 @no__t 0 和 @no__t 1 運算子，則會產生 null 值，如[布林邏輯運算子](expressions.md#boolean-logical-operators)中所述）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-381">The lifted operator produces a null value if one or both operands are null (an exception being the `&` and `|` operators of the `bool?` type, as described in [Boolean logical operators](expressions.md#boolean-logical-operators)).</span></span> <span data-ttu-id="6aeee-382">否則，提升運算子會解除包裝運算元、套用基礎運算子，並將結果包裝起來。</span><span class="sxs-lookup"><span data-stu-id="6aeee-382">Otherwise, the lifted operator unwraps the operands, applies the underlying operator, and wraps the result.</span></span>

*   <span data-ttu-id="6aeee-383">適用于等號比較運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-383">For the equality operators</span></span>

    ```csharp
    ==  !=
    ```

    <span data-ttu-id="6aeee-384">如果運算元類型既不是可為 null 的實數值型別，且結果類型為 `bool`，則運算子的形式會存在。</span><span class="sxs-lookup"><span data-stu-id="6aeee-384">a lifted form of an operator exists if the operand types are both non-nullable value types and if the result type is `bool`.</span></span> <span data-ttu-id="6aeee-385">藉由將單一的 `?` 修飾詞加入至每個運算元類型，即可形成提升的形式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-385">The lifted form is constructed by adding a single `?` modifier to each operand type.</span></span> <span data-ttu-id="6aeee-386">提升運算子會將兩個 null 值視為相等，並將 null 值與任何非 null 值不相等。</span><span class="sxs-lookup"><span data-stu-id="6aeee-386">The lifted operator considers two null values equal, and a null value unequal to any non-null value.</span></span> <span data-ttu-id="6aeee-387">如果兩個運算元都不是 null，則提升的運算子會解除包裝運算元並套用基礎運算子，以產生 @no__t 0 的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-387">If both operands are non-null, the lifted operator unwraps the operands and applies the underlying operator to produce the `bool` result.</span></span>

*   <span data-ttu-id="6aeee-388">針對關聯式運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-388">For the relational operators</span></span>

    ```csharp
    <  >  <=  >=
    ```

    <span data-ttu-id="6aeee-389">如果運算元類型既不是可為 null 的實數值型別，且結果類型為 `bool`，則運算子的形式會存在。</span><span class="sxs-lookup"><span data-stu-id="6aeee-389">a lifted form of an operator exists if the operand types are both non-nullable value types and if the result type is `bool`.</span></span> <span data-ttu-id="6aeee-390">藉由將單一的 `?` 修飾詞加入至每個運算元類型，即可形成提升的形式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-390">The lifted form is constructed by adding a single `?` modifier to each operand type.</span></span> <span data-ttu-id="6aeee-391">如果一個或兩個運算元都是 null，則提升運算子會產生 `false` 的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-391">The lifted operator produces the value `false` if one or both operands are null.</span></span> <span data-ttu-id="6aeee-392">否則，提升運算子會解除包裝運算元並套用基礎運算子，以產生 @no__t 0 的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-392">Otherwise, the lifted operator unwraps the operands and applies the underlying operator to produce the `bool` result.</span></span>

## <a name="member-lookup"></a><span data-ttu-id="6aeee-393">成員查閱</span><span class="sxs-lookup"><span data-stu-id="6aeee-393">Member lookup</span></span>

<span data-ttu-id="6aeee-394">成員查閱是一種程式，會決定類型內容中名稱的意義。</span><span class="sxs-lookup"><span data-stu-id="6aeee-394">A member lookup is the process whereby the meaning of a name in the context of a type is determined.</span></span> <span data-ttu-id="6aeee-395">成員查詢可以做為評估運算式中的*simple_name* （[簡單名稱](expressions.md#simple-names)）或*member_access* （[成員存取](expressions.md#member-access)）的一部分。</span><span class="sxs-lookup"><span data-stu-id="6aeee-395">A member lookup can occur as part of evaluating a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)) in an expression.</span></span> <span data-ttu-id="6aeee-396">如果*simple_name*或*member_access*是做為*invocation_expression* （[方法調用](expressions.md#method-invocations)）的*primary_expression* ，則會叫用該成員。</span><span class="sxs-lookup"><span data-stu-id="6aeee-396">If the *simple_name* or *member_access* occurs as the *primary_expression* of an *invocation_expression* ([Method invocations](expressions.md#method-invocations)), the member is said to be invoked.</span></span>

<span data-ttu-id="6aeee-397">如果成員是方法或事件，或者如果它是委派類型（[委派](delegates.md)）或類型 `dynamic` （[動態類型](types.md#the-dynamic-type)）的常數、欄位或屬性，則該成員會被視為*invocable*。</span><span class="sxs-lookup"><span data-stu-id="6aeee-397">If a member is a method or event, or if it is a constant, field or property of either a delegate type ([Delegates](delegates.md)) or the type `dynamic` ([The dynamic type](types.md#the-dynamic-type)), then the member is said to be *invocable*.</span></span>

<span data-ttu-id="6aeee-398">成員查閱不僅會考慮成員的名稱，也會考慮成員所擁有之類型參數的數目，以及是否可存取該成員。</span><span class="sxs-lookup"><span data-stu-id="6aeee-398">Member lookup considers not only the name of a member but also the number of type parameters the member has and whether the member is accessible.</span></span> <span data-ttu-id="6aeee-399">基於成員查詢的目的，泛型方法和嵌套泛型型別具有在其個別宣告中指出的類型參數數目，而且所有其他成員都有零型別參數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-399">For the purposes of member lookup, generic methods and nested generic types have the number of type parameters indicated in their respective declarations and all other members have zero type parameters.</span></span>

<span data-ttu-id="6aeee-400">在類型 @ no__t-3 中，名稱 @ no__t-0 的成員查閱與 `K` @ no__t-2type 參數的處理方式如下：</span><span class="sxs-lookup"><span data-stu-id="6aeee-400">A member lookup of a name `N` with `K` type parameters in a type `T` is processed as follows:</span></span>

*  <span data-ttu-id="6aeee-401">首先，會決定一組名為 @ no__t-0 的可存取成員：</span><span class="sxs-lookup"><span data-stu-id="6aeee-401">First, a set of accessible members named `N` is determined:</span></span>
    * <span data-ttu-id="6aeee-402">如果 `T` 是型別參數，則集合會是每個指定為 @ no__t-3 之主要條件約束或次要條件約束（[類型參數條件約束](classes.md#type-parameter-constraints)）的可存取成員集合的聯集，以及一組`object` 中名為 @ no__t-4 的可存取成員。</span><span class="sxs-lookup"><span data-stu-id="6aeee-402">If `T` is a type parameter, then the set is the union of the sets of accessible members named `N` in each of the types specified as a primary constraint or secondary constraint ([Type parameter constraints](classes.md#type-parameter-constraints)) for `T`, along with the set of accessible members named `N` in `object`.</span></span>
    * <span data-ttu-id="6aeee-403">否則，此集合是由 @ no__t-2 中名為 @ no__t-1 的所有可存取（[成員存取](basic-concepts.md#member-access)）成員所組成，包括繼承的成員和在 `object` 中名為 @ no__t-3 的可存取成員。</span><span class="sxs-lookup"><span data-stu-id="6aeee-403">Otherwise, the set consists of all accessible ([Member access](basic-concepts.md#member-access)) members named `N` in `T`, including inherited members and the accessible members named `N` in `object`.</span></span> <span data-ttu-id="6aeee-404">如果 `T` 是結構化型別，則會藉由替代類型引數來取得成員集合，如[結構化類型的成員](classes.md#members-of-constructed-types)中所述。</span><span class="sxs-lookup"><span data-stu-id="6aeee-404">If `T` is a constructed type, the set of members is obtained by substituting type arguments as described in [Members of constructed types](classes.md#members-of-constructed-types).</span></span> <span data-ttu-id="6aeee-405">包含 `override` 修飾詞的成員會從集合中排除。</span><span class="sxs-lookup"><span data-stu-id="6aeee-405">Members that include an `override` modifier are excluded from the set.</span></span>
*  <span data-ttu-id="6aeee-406">接下來，如果 `K` 為零，則會移除其宣告包含類型參數的所有巢狀型別。</span><span class="sxs-lookup"><span data-stu-id="6aeee-406">Next, if `K` is zero, all nested types whose declarations include type parameters are removed.</span></span> <span data-ttu-id="6aeee-407">如果 `K` 不是零，則會移除具有不同類型參數數目的所有成員。</span><span class="sxs-lookup"><span data-stu-id="6aeee-407">If `K` is not zero, all members with a different number of type parameters are removed.</span></span> <span data-ttu-id="6aeee-408">請注意，當 `K` 為零時，不會移除具有型別參數的方法，因為型別推斷程式（[型別推斷](expressions.md#type-inference)）可能能夠推斷型別引數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-408">Note that when `K` is zero, methods having type parameters are not removed, since the type inference process ([Type inference](expressions.md#type-inference)) might be able to infer the type arguments.</span></span>
*  <span data-ttu-id="6aeee-409">接下來，如果叫用該成員，則會從*集合中移除*所有非*invocable*的成員。</span><span class="sxs-lookup"><span data-stu-id="6aeee-409">Next, if the member is *invoked*, all non-*invocable* members are removed from the set.</span></span>
*  <span data-ttu-id="6aeee-410">接下來，會從集合中移除其他成員所隱藏的成員。</span><span class="sxs-lookup"><span data-stu-id="6aeee-410">Next, members that are hidden by other members are removed from the set.</span></span> <span data-ttu-id="6aeee-411">針對集合中的每個成員 `S.M`，其中 `S` 是宣告成員 @ no__t-2 的類型，會套用下列規則：</span><span class="sxs-lookup"><span data-stu-id="6aeee-411">For every member `S.M` in the set, where `S` is the type in which the member `M` is declared, the following rules are applied:</span></span>
    * <span data-ttu-id="6aeee-412">如果 `M` 是常數、欄位、屬性、事件或列舉成員，則會從集合中移除 `S` 之基底類型中宣告的所有成員。</span><span class="sxs-lookup"><span data-stu-id="6aeee-412">If `M` is a constant, field, property, event, or enumeration member, then all members declared in a base type of `S` are removed from the set.</span></span>
    * <span data-ttu-id="6aeee-413">如果 `M` 是類型宣告，則會從集合中移除 `S` 之基底類型中宣告的所有非類型，而且具有相同數目之類型 @no__t 參數的所有類型宣告，都會從集合 @no__t 中移除。</span><span class="sxs-lookup"><span data-stu-id="6aeee-413">If `M` is a type declaration, then all non-types declared in a base type of `S` are removed from the set, and all type declarations with the same number of type parameters as `M` declared in a base type of `S` are removed from the set.</span></span>
    * <span data-ttu-id="6aeee-414">如果 `M` 是方法，則會從集合中移除 `S` 之基底類型中宣告的所有非方法成員。</span><span class="sxs-lookup"><span data-stu-id="6aeee-414">If `M` is a method, then all non-method members declared in a base type of `S` are removed from the set.</span></span>
*  <span data-ttu-id="6aeee-415">接下來，會從集合中移除類別成員所隱藏的介面成員。</span><span class="sxs-lookup"><span data-stu-id="6aeee-415">Next, interface members that are hidden by class members are removed from the set.</span></span> <span data-ttu-id="6aeee-416">只有當 `T` 是型別參數，而且 `T` 同時具有 `object` 和非空白有效介面集（[型別參數條件約束](classes.md#type-parameter-constraints)）以外的有效基類時，這個步驟才有作用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-416">This step only has an effect if `T` is a type parameter and `T` has both an effective base class other than `object` and a non-empty effective interface set ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="6aeee-417">針對集合中的每個成員 `S.M`，其中 `S` 是宣告成員 `M` 的類型，如果 `S` 是 `object` 以外的類別宣告，則會套用下列規則：</span><span class="sxs-lookup"><span data-stu-id="6aeee-417">For every member `S.M` in the set, where `S` is the type in which the member `M` is declared, the following rules are applied if `S` is a class declaration other than `object`:</span></span>
    * <span data-ttu-id="6aeee-418">如果 `M` 是常數、欄位、屬性、事件、列舉成員或類型宣告，則會從集合中移除在介面宣告中宣告的所有成員。</span><span class="sxs-lookup"><span data-stu-id="6aeee-418">If `M` is a constant, field, property, event, enumeration member, or type declaration, then all members declared in an interface declaration are removed from the set.</span></span>
    * <span data-ttu-id="6aeee-419">如果 `M` 是方法，則會從集合中移除在介面宣告中宣告的所有非方法成員，並從集合中移除所有具有相同簽章的方法，並將其宣告為在介面宣告中宣告的 `M`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-419">If `M` is a method, then all non-method members declared in an interface declaration are removed from the set, and all methods with the same signature as `M` declared in an interface declaration are removed from the set.</span></span>
*  <span data-ttu-id="6aeee-420">最後，移除隱藏的成員後，就會決定查閱的結果：</span><span class="sxs-lookup"><span data-stu-id="6aeee-420">Finally, having removed hidden members, the result of the lookup is determined:</span></span>
    * <span data-ttu-id="6aeee-421">如果集合是由不是方法的單一成員所組成，則這個成員就是查閱的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-421">If the set consists of a single member that is not a method, then this member is the result of the lookup.</span></span>
    * <span data-ttu-id="6aeee-422">否則，如果集合僅包含方法，則這個方法群組就是查閱的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-422">Otherwise, if the set contains only methods, then this group of methods is the result of the lookup.</span></span>
    * <span data-ttu-id="6aeee-423">否則，查閱是不明確的，而且會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-423">Otherwise, the lookup is ambiguous, and a binding-time error occurs.</span></span>

<span data-ttu-id="6aeee-424">針對類型參數和介面以外類型中的成員查閱，以及在介面中為嚴格單一繼承的成員查閱（繼承鏈中的每個介面都只有零或一個直接基底介面），查閱規則的效果為只有衍生的成員會隱藏具有相同名稱或簽章的基底成員。</span><span class="sxs-lookup"><span data-stu-id="6aeee-424">For member lookups in types other than type parameters and interfaces, and member lookups in interfaces that are strictly single-inheritance (each interface in the inheritance chain has exactly zero or one direct base interface), the effect of the lookup rules is simply that derived members hide base members with the same name or signature.</span></span> <span data-ttu-id="6aeee-425">這類的單一繼承查閱永遠不會造成混淆。</span><span class="sxs-lookup"><span data-stu-id="6aeee-425">Such single-inheritance lookups are never ambiguous.</span></span> <span data-ttu-id="6aeee-426">在多個繼承介面中，成員查閱可能引發的多義性會在[介面成員存取](interfaces.md#interface-member-access)中說明。</span><span class="sxs-lookup"><span data-stu-id="6aeee-426">The ambiguities that can possibly arise from member lookups in multiple-inheritance interfaces are described in [Interface member access](interfaces.md#interface-member-access).</span></span>

### <a name="base-types"></a><span data-ttu-id="6aeee-427">基底類型</span><span class="sxs-lookup"><span data-stu-id="6aeee-427">Base types</span></span>

<span data-ttu-id="6aeee-428">基於成員查閱的目的，會將類型 `T` 視為具有下列基底類型：</span><span class="sxs-lookup"><span data-stu-id="6aeee-428">For purposes of member lookup, a type `T` is considered to have the following base types:</span></span>

*  <span data-ttu-id="6aeee-429">如果 `T` 是 `object`，則 `T` 沒有基底類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-429">If `T` is `object`, then `T` has no base type.</span></span>
*  <span data-ttu-id="6aeee-430">如果 `T` 是*enum_type*，則 `T` 的基底類型為類別類型 `System.Enum`、`System.ValueType` 和 `object`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-430">If `T` is an *enum_type*, the base types of `T` are the class types `System.Enum`, `System.ValueType`, and `object`.</span></span>
*  <span data-ttu-id="6aeee-431">如果 `T` 是*struct_type*，則 `T` 的基底類型是 `System.ValueType` 和 `object` 的類別類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-431">If `T` is a *struct_type*, the base types of `T` are the class types `System.ValueType` and `object`.</span></span>
*  <span data-ttu-id="6aeee-432">如果 `T` 是*class_type*，則 `T` 的基底類型是 `T` 的基類，包括類別類型 `object`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-432">If `T` is a *class_type*, the base types of `T` are the base classes of `T`, including the class type `object`.</span></span>
*  <span data-ttu-id="6aeee-433">如果 `T` 是*interface_type*，則 `T` 的基底類型是 `T` 的基底介面，而類別類型 `object`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-433">If `T` is an *interface_type*, the base types of `T` are the base interfaces of `T` and the class type `object`.</span></span>
*  <span data-ttu-id="6aeee-434">如果 `T` 是*array_type*，則 `T` 的基底類型是 `System.Array` 和 `object` 的類別類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-434">If `T` is an *array_type*, the base types of `T` are the class types `System.Array` and `object`.</span></span>
*  <span data-ttu-id="6aeee-435">如果 `T` 是*delegate_type*，則 `T` 的基底類型是 `System.Delegate` 和 `object` 的類別類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-435">If `T` is a *delegate_type*, the base types of `T` are the class types `System.Delegate` and `object`.</span></span>

## <a name="function-members"></a><span data-ttu-id="6aeee-436">函數成員</span><span class="sxs-lookup"><span data-stu-id="6aeee-436">Function members</span></span>

<span data-ttu-id="6aeee-437">函數成員是包含可執行語句的成員。</span><span class="sxs-lookup"><span data-stu-id="6aeee-437">Function members are members that contain executable statements.</span></span> <span data-ttu-id="6aeee-438">函式成員一律是類型的成員，而且不能是命名空間的成員。</span><span class="sxs-lookup"><span data-stu-id="6aeee-438">Function members are always members of types and cannot be members of namespaces.</span></span> <span data-ttu-id="6aeee-439">C#定義下列函數成員類別：</span><span class="sxs-lookup"><span data-stu-id="6aeee-439">C# defines the following categories of function members:</span></span>

*  <span data-ttu-id="6aeee-440">方法</span><span class="sxs-lookup"><span data-stu-id="6aeee-440">Methods</span></span>
*  <span data-ttu-id="6aeee-441">屬性</span><span class="sxs-lookup"><span data-stu-id="6aeee-441">Properties</span></span>
*  <span data-ttu-id="6aeee-442">Events</span><span class="sxs-lookup"><span data-stu-id="6aeee-442">Events</span></span>
*  <span data-ttu-id="6aeee-443">索引子</span><span class="sxs-lookup"><span data-stu-id="6aeee-443">Indexers</span></span>
*  <span data-ttu-id="6aeee-444">使用者定義的運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-444">User-defined operators</span></span>
*  <span data-ttu-id="6aeee-445">實例構造函式</span><span class="sxs-lookup"><span data-stu-id="6aeee-445">Instance constructors</span></span>
*  <span data-ttu-id="6aeee-446">靜態建構函式</span><span class="sxs-lookup"><span data-stu-id="6aeee-446">Static constructors</span></span>
*  <span data-ttu-id="6aeee-447">解構函式</span><span class="sxs-lookup"><span data-stu-id="6aeee-447">Destructors</span></span>

<span data-ttu-id="6aeee-448">除了析構函式和靜態函式（無法明確叫用）以外，函數成員中包含的語句會透過函式成員調用來執行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-448">Except for destructors and static constructors (which cannot be invoked explicitly), the statements contained in function members are executed through function member invocations.</span></span> <span data-ttu-id="6aeee-449">撰寫函式成員調用的實際語法取決於特定的函式成員分類。</span><span class="sxs-lookup"><span data-stu-id="6aeee-449">The actual syntax for writing a function member invocation depends on the particular function member category.</span></span>

<span data-ttu-id="6aeee-450">函式成員調用的引數清單（[引數](expressions.md#argument-lists)清單）提供函式成員之參數的實際值或變數參考。</span><span class="sxs-lookup"><span data-stu-id="6aeee-450">The argument list ([Argument lists](expressions.md#argument-lists)) of a function member invocation provides actual values or variable references for the parameters of the function member.</span></span>

<span data-ttu-id="6aeee-451">叫用泛型方法可能會採用型別推斷來判斷要傳遞給方法的型別引數集合。</span><span class="sxs-lookup"><span data-stu-id="6aeee-451">Invocations of generic methods may employ type inference to determine the set of type arguments to pass to the method.</span></span> <span data-ttu-id="6aeee-452">此程式會在[型別推斷](expressions.md#type-inference)中說明。</span><span class="sxs-lookup"><span data-stu-id="6aeee-452">This process is described in [Type inference](expressions.md#type-inference).</span></span>

<span data-ttu-id="6aeee-453">調用方法、索引子、運算子和實例的函式會採用多載解析，以判斷要叫用的一組候選函式成員。</span><span class="sxs-lookup"><span data-stu-id="6aeee-453">Invocations of methods, indexers, operators and instance constructors employ overload resolution to determine which of a candidate set of function members to invoke.</span></span> <span data-ttu-id="6aeee-454">此程式會在多載[解析](expressions.md#overload-resolution)中說明。</span><span class="sxs-lookup"><span data-stu-id="6aeee-454">This process is described in [Overload resolution](expressions.md#overload-resolution).</span></span>

<span data-ttu-id="6aeee-455">一旦在系結階段識別特定的函式成員（可能是透過多載解析），就會在動態多載[解析的編譯階段檢查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)中說明叫用函式成員的實際執行時間程式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-455">Once a particular function member has been identified at binding-time, possibly through overload resolution, the actual run-time process of invoking the function member is described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="6aeee-456">下表摘要說明可明確叫用的六個函式成員類別的結構中所發生的處理。</span><span class="sxs-lookup"><span data-stu-id="6aeee-456">The following table summarizes the processing that takes place in constructs involving the six categories of function members that can be explicitly invoked.</span></span> <span data-ttu-id="6aeee-457">在資料表中，`e`，`x`，`y`，而 `value` 表示分類為變數或值的運算式，`T` 表示分類為類型的運算式，`F` 是方法的簡單名稱，而 `P` 是屬性的簡單名稱。</span><span class="sxs-lookup"><span data-stu-id="6aeee-457">In the table, `e`, `x`, `y`, and `value` indicate expressions classified as variables or values, `T` indicates an expression classified as a type, `F` is the simple name of a method, and `P` is the simple name of a property.</span></span>


| <span data-ttu-id="6aeee-458">__建構__</span><span class="sxs-lookup"><span data-stu-id="6aeee-458">__Construct__</span></span>     | <span data-ttu-id="6aeee-459">__範例__</span><span class="sxs-lookup"><span data-stu-id="6aeee-459">__Example__</span></span>    | <span data-ttu-id="6aeee-460">__描述__</span><span class="sxs-lookup"><span data-stu-id="6aeee-460">__Description__</span></span> |
|-------------------|----------------|-----------------|
| <span data-ttu-id="6aeee-461">方法引動過程</span><span class="sxs-lookup"><span data-stu-id="6aeee-461">Method invocation</span></span> | `F(x,y)`       | <span data-ttu-id="6aeee-462">會套用多載解析，以在包含的類別或結構中選取 `F` 的最佳方法。</span><span class="sxs-lookup"><span data-stu-id="6aeee-462">Overload resolution is applied to select the best method `F` in the containing class or struct.</span></span> <span data-ttu-id="6aeee-463">方法是使用引數清單 `(x,y)` 來叫用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-463">The method is invoked with the argument list `(x,y)`.</span></span> <span data-ttu-id="6aeee-464">如果方法不 `static`，實例運算式就會 `this`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-464">If the method is not `static`, the instance expression is `this`.</span></span> | 
|                   | `T.F(x,y)`     | <span data-ttu-id="6aeee-465">會套用多載解析，以選取類別或結構 `T` 中 `F` 的最佳方法。</span><span class="sxs-lookup"><span data-stu-id="6aeee-465">Overload resolution is applied to select the best method `F` in the class or struct `T`.</span></span> <span data-ttu-id="6aeee-466">如果方法不 `static`，就會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-466">A binding-time error occurs if the method is not `static`.</span></span> <span data-ttu-id="6aeee-467">方法是使用引數清單 `(x,y)` 來叫用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-467">The method is invoked with the argument list `(x,y)`.</span></span> | 
|                   | `e.F(x,y)`     | <span data-ttu-id="6aeee-468">會套用多載解析，以在 `e` 的類型所指定的類別、結構或介面中選取最佳方法 F。</span><span class="sxs-lookup"><span data-stu-id="6aeee-468">Overload resolution is applied to select the best method F in the class, struct, or interface given by the type of `e`.</span></span> <span data-ttu-id="6aeee-469">如果方法 `static`，就會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-469">A binding-time error occurs if the method is `static`.</span></span> <span data-ttu-id="6aeee-470">方法是使用實例運算式來叫用 `e`，而引數清單 `(x,y)`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-470">The method is invoked with the instance expression `e` and the argument list `(x,y)`.</span></span> | 
| <span data-ttu-id="6aeee-471">屬性存取</span><span class="sxs-lookup"><span data-stu-id="6aeee-471">Property access</span></span>   | `P`            | <span data-ttu-id="6aeee-472">會叫用包含類別或結構中，`P` 之屬性的 `get` 存取子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-472">The `get` accessor of the property `P` in the containing class or struct is invoked.</span></span> <span data-ttu-id="6aeee-473">如果 `P` 是寫入時，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-473">A compile-time error occurs if `P` is write-only.</span></span> <span data-ttu-id="6aeee-474">如果 `P` 不是 `static`，則實例運算式會 `this`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-474">If `P` is not `static`, the instance expression is `this`.</span></span> | 
|                   | `P = value`    | <span data-ttu-id="6aeee-475">在包含類別或結構中，`P` 之屬性的 `set` 存取子，會以引數清單 `(value)` 來叫用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-475">The `set` accessor of the property `P` in the containing class or struct is invoked with the argument list `(value)`.</span></span> <span data-ttu-id="6aeee-476">如果 `P` 是唯讀的，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-476">A compile-time error occurs if `P` is read-only.</span></span> <span data-ttu-id="6aeee-477">如果 `P` 不是 `static`，則實例運算式會 `this`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-477">If `P` is not `static`, the instance expression is `this`.</span></span> | 
|                   | `T.P`          | <span data-ttu-id="6aeee-478">會叫用類別或結構 `T` 中 `P` 之屬性的 `get` 存取子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-478">The `get` accessor of the property `P` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="6aeee-479">如果 `P` 未 `static`，或 `P` 為僅限寫入，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-479">A compile-time error occurs if `P` is not `static` or if `P` is write-only.</span></span> | 
|                   | `T.P = value`  | <span data-ttu-id="6aeee-480">在類別或結構 `T` 中，`P` 之屬性的 `set` 存取子會以引數清單 `(value)` 叫用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-480">The `set` accessor of the property `P` in the class or struct `T` is invoked with the argument list `(value)`.</span></span> <span data-ttu-id="6aeee-481">如果 `P` 未 `static` 或 `P` 為唯讀，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-481">A compile-time error occurs if `P` is not `static` or if `P` is read-only.</span></span> | 
|                   | `e.P`          | <span data-ttu-id="6aeee-482">在 `e` 類型所指定的類別、結構或介面中，`P` 之屬性的 `get` 存取子，會以實例運算式 `e` 來叫用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-482">The `get` accessor of the property `P` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="6aeee-483">如果 `P` 是 `static` 或 `P` 為僅限寫入，則會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-483">A binding-time error occurs if `P` is `static` or if `P` is write-only.</span></span> | 
|                   | `e.P = value`  | <span data-ttu-id="6aeee-484">在 `e` 類型所指定的類別、結構或介面中，`P` 之屬性的 `set` 存取子，會以實例運算式 `e` 和引數清單 `(value)` 來叫用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-484">The `set` accessor of the property `P` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e` and the argument list `(value)`.</span></span> <span data-ttu-id="6aeee-485">如果 `P` `static` 或 `P` 為唯讀，就會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-485">A binding-time error occurs if `P` is `static` or if `P` is read-only.</span></span> | 
| <span data-ttu-id="6aeee-486">事件存取</span><span class="sxs-lookup"><span data-stu-id="6aeee-486">Event access</span></span>      | `E += value`   | <span data-ttu-id="6aeee-487">叫用包含類別或結構中事件 `E` 的 `add` 存取子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-487">The `add` accessor of the event `E` in the containing class or struct is invoked.</span></span> <span data-ttu-id="6aeee-488">如果 `E` 不是靜態的，則實例運算式會 `this`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-488">If `E` is not static, the instance expression is `this`.</span></span> | 
|                   | `E -= value`   | <span data-ttu-id="6aeee-489">叫用包含類別或結構中事件 `E` 的 `remove` 存取子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-489">The `remove` accessor of the event `E` in the containing class or struct is invoked.</span></span> <span data-ttu-id="6aeee-490">如果 `E` 不是靜態的，則實例運算式會 `this`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-490">If `E` is not static, the instance expression is `this`.</span></span> | 
|                   | `T.E += value` | <span data-ttu-id="6aeee-491">叫用類別或結構 `T` 中事件 `E` 的 `add` 存取子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-491">The `add` accessor of the event `E` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="6aeee-492">如果 `E` 不是靜態的，就會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-492">A binding-time error occurs if `E` is not static.</span></span> | 
|                   | `T.E -= value` | <span data-ttu-id="6aeee-493">叫用類別或結構 `T` 中事件 `E` 的 `remove` 存取子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-493">The `remove` accessor of the event `E` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="6aeee-494">如果 `E` 不是靜態的，就會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-494">A binding-time error occurs if `E` is not static.</span></span> | 
|                   | `e.E += value` | <span data-ttu-id="6aeee-495">在 `e` 類型所指定的類別、結構或介面中，事件 `E` 的 `add` 存取子是以實例運算式 `e` 來叫用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-495">The `add` accessor of the event `E` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="6aeee-496">如果 `E` 是靜態的，就會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-496">A binding-time error occurs if `E` is static.</span></span> | 
|                   | `e.E -= value` | <span data-ttu-id="6aeee-497">在 `e` 類型所指定的類別、結構或介面中，事件 `E` 的 `remove` 存取子是以實例運算式 `e` 來叫用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-497">The `remove` accessor of the event `E` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="6aeee-498">如果 `E` 是靜態的，就會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-498">A binding-time error occurs if `E` is static.</span></span> | 
| <span data-ttu-id="6aeee-499">索引子存取</span><span class="sxs-lookup"><span data-stu-id="6aeee-499">Indexer access</span></span>    | `e[x,y]`       | <span data-ttu-id="6aeee-500">會套用多載解析，以選取 e 類型所指定之類別、結構或介面中的最佳索引子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-500">Overload resolution is applied to select the best indexer in the class, struct, or interface given by the type of e.</span></span> <span data-ttu-id="6aeee-501">索引子的 `get` 存取子是利用實例運算式來叫用 `e`，而引數清單 `(x,y)`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-501">The `get` accessor of the indexer is invoked with the instance expression `e` and the argument list `(x,y)`.</span></span> <span data-ttu-id="6aeee-502">如果索引子為僅限寫入，則會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-502">A binding-time error occurs if the indexer is write-only.</span></span> | 
|                   | `e[x,y] = value` | <span data-ttu-id="6aeee-503">會套用多載解析，以在 `e` 的類型所指定的類別、結構或介面中選取最佳索引子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-503">Overload resolution is applied to select the best indexer in the class, struct, or interface given by the type of `e`.</span></span> <span data-ttu-id="6aeee-504">索引子的 `set` 存取子是利用實例運算式來叫用 `e`，而引數清單 `(x,y,value)`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-504">The `set` accessor of the indexer is invoked with the instance expression `e` and the argument list `(x,y,value)`.</span></span> <span data-ttu-id="6aeee-505">如果索引子是唯讀的，就會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-505">A binding-time error occurs if the indexer is read-only.</span></span> | 
| <span data-ttu-id="6aeee-506">運算子調用</span><span class="sxs-lookup"><span data-stu-id="6aeee-506">Operator invocation</span></span> | `-x`         | <span data-ttu-id="6aeee-507">會套用多載解析，以在 `x` 類型所指定的類別或結構中選取最佳的一元運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-507">Overload resolution is applied to select the best unary operator in the class or struct given by the type of `x`.</span></span> <span data-ttu-id="6aeee-508">使用引數清單叫用選取的運算子 `(x)`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-508">The selected operator is invoked with the argument list `(x)`.</span></span> | 
|                     | `x + y`      | <span data-ttu-id="6aeee-509">會套用多載解析，以在 `x` 和 `y` 類型所指定的類別或結構中選取最佳的二元運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-509">Overload resolution is applied to select the best binary operator in the classes or structs given by the types of `x` and `y`.</span></span> <span data-ttu-id="6aeee-510">使用引數清單叫用選取的運算子 `(x,y)`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-510">The selected operator is invoked with the argument list `(x,y)`.</span></span> | 
| <span data-ttu-id="6aeee-511">實例的函式呼叫</span><span class="sxs-lookup"><span data-stu-id="6aeee-511">Instance constructor invocation</span></span> | `new T(x,y)` | <span data-ttu-id="6aeee-512">會套用多載解析，以在類別或結構中選取最佳實例的分析 `T`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-512">Overload resolution is applied to select the best instance constructor in the class or struct `T`.</span></span> <span data-ttu-id="6aeee-513">使用引數清單叫用實例的函式，`(x,y)`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-513">The instance constructor is invoked with the argument list `(x,y)`.</span></span> | 

### <a name="argument-lists"></a><span data-ttu-id="6aeee-514">引數清單</span><span class="sxs-lookup"><span data-stu-id="6aeee-514">Argument lists</span></span>

<span data-ttu-id="6aeee-515">每個函式成員和委派調用都包含引數清單，可為函式成員的參數提供實際的值或變數參考。</span><span class="sxs-lookup"><span data-stu-id="6aeee-515">Every function member and delegate invocation includes an argument list which provides actual values or variable references for the parameters of the function member.</span></span> <span data-ttu-id="6aeee-516">指定函式成員調用之引數清單的語法，取決於函式成員分類：</span><span class="sxs-lookup"><span data-stu-id="6aeee-516">The syntax for specifying the argument list of a function member invocation depends on the function member category:</span></span>

*  <span data-ttu-id="6aeee-517">若為實例的構造函式、方法、索引子和委派，引數會指定為*argument_list*，如下所述。</span><span class="sxs-lookup"><span data-stu-id="6aeee-517">For instance constructors, methods, indexers and delegates, the arguments are specified as an *argument_list*, as described below.</span></span> <span data-ttu-id="6aeee-518">針對索引子，叫用 `set` 存取子時，引數清單會另外包含指定為指派運算子之右運算元的運算式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-518">For indexers, when invoking the `set` accessor, the argument list additionally includes the expression specified as the right operand of the assignment operator.</span></span>
*  <span data-ttu-id="6aeee-519">對於屬性而言，叫用 `get` 存取子時，引數清單是空的，而且在叫用 `set` 存取子時，是由指定為指派運算子右運算元的運算式所組成。</span><span class="sxs-lookup"><span data-stu-id="6aeee-519">For properties, the argument list is empty when invoking the `get` accessor, and consists of the expression specified as the right operand of the assignment operator when invoking the `set` accessor.</span></span>
*  <span data-ttu-id="6aeee-520">對於事件，引數清單是由指定為 `+=` 或 `-=` 運算子右運算元的運算式所組成。</span><span class="sxs-lookup"><span data-stu-id="6aeee-520">For events, the argument list consists of the expression specified as the right operand of the `+=` or `-=` operator.</span></span>
*  <span data-ttu-id="6aeee-521">針對使用者定義的運算子，引數清單是由一元運算子的單一運算元或二元運算子的兩個運算元所組成。</span><span class="sxs-lookup"><span data-stu-id="6aeee-521">For user-defined operators, the argument list consists of the single operand of the unary operator or the two operands of the binary operator.</span></span>

<span data-ttu-id="6aeee-522">屬性（[屬性](classes.md#properties)）、事件（[事件](classes.md#events)）和使用者定義運算子（[運算子](classes.md#operators)）的引數一律會當做值參數（[值參數](classes.md#value-parameters)）傳遞。</span><span class="sxs-lookup"><span data-stu-id="6aeee-522">The arguments of properties ([Properties](classes.md#properties)), events ([Events](classes.md#events)), and user-defined operators ([Operators](classes.md#operators)) are always passed as value parameters ([Value parameters](classes.md#value-parameters)).</span></span> <span data-ttu-id="6aeee-523">索引子（[索引](classes.md#indexers)器）的引數一律會當做值參數（[值參數](classes.md#value-parameters)）或參數陣列（[參數陣列](classes.md#parameter-arrays)）傳遞。</span><span class="sxs-lookup"><span data-stu-id="6aeee-523">The arguments of indexers ([Indexers](classes.md#indexers)) are always passed as value parameters ([Value parameters](classes.md#value-parameters)) or parameter arrays ([Parameter arrays](classes.md#parameter-arrays)).</span></span> <span data-ttu-id="6aeee-524">這些函數成員分類不支援參考和輸出參數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-524">Reference and output parameters are not supported for these categories of function members.</span></span>

<span data-ttu-id="6aeee-525">實例函式、方法、索引子或委派調用的引數會指定為*argument_list*：</span><span class="sxs-lookup"><span data-stu-id="6aeee-525">The arguments of an instance constructor, method, indexer or delegate invocation are specified as an *argument_list*:</span></span>

```antlr
argument_list
    : argument (',' argument)*
    ;

argument
    : argument_name? argument_value
    ;

argument_name
    : identifier ':'
    ;

argument_value
    : expression
    | 'ref' variable_reference
    | 'out' variable_reference
    ;
```

<span data-ttu-id="6aeee-526">*Argument_list*是由一個或多個*引數*所組成，並以逗號分隔。</span><span class="sxs-lookup"><span data-stu-id="6aeee-526">An *argument_list* consists of one or more *argument*s, separated by commas.</span></span> <span data-ttu-id="6aeee-527">每個引數都包含一個選擇性的*argument_name* ，後面接著*argument_value*。</span><span class="sxs-lookup"><span data-stu-id="6aeee-527">Each argument consists of an optional  *argument_name* followed by an *argument_value*.</span></span> <span data-ttu-id="6aeee-528">具有*argument_name*的*引數*稱為「***具名引數***」，而不含 argument_name*的自*變數是***位置引數***。</span><span class="sxs-lookup"><span data-stu-id="6aeee-528">An *argument* with an *argument_name* is referred to as a ***named argument***, whereas an *argument* without an *argument_name* is a ***positional argument***.</span></span> <span data-ttu-id="6aeee-529">位置引數會出現在*argument_list*中的具名引數後面，這是錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-529">It is an error for a positional argument to appear after a named argument in an *argument_list*.</span></span>

<span data-ttu-id="6aeee-530">*Argument_value*可以採用下列其中一種形式：</span><span class="sxs-lookup"><span data-stu-id="6aeee-530">The *argument_value* can take one of the following forms:</span></span>

*  <span data-ttu-id="6aeee-531">*運算式*，表示引數會當做值參數（[值參數](classes.md#value-parameters)）傳遞。</span><span class="sxs-lookup"><span data-stu-id="6aeee-531">An *expression*, indicating that the argument is passed as a value parameter ([Value parameters](classes.md#value-parameters)).</span></span>
*  <span data-ttu-id="6aeee-532">關鍵字 `ref` 後面接著*variable_reference* （[變數參考](variables.md#variable-references)），表示引數會當做參考參數（[參考參數](classes.md#reference-parameters)）傳遞。</span><span class="sxs-lookup"><span data-stu-id="6aeee-532">The keyword `ref` followed by a *variable_reference* ([Variable references](variables.md#variable-references)), indicating that the argument is passed as a reference parameter ([Reference parameters](classes.md#reference-parameters)).</span></span> <span data-ttu-id="6aeee-533">必須先明確指派變數（[明確指派](variables.md#definite-assignment)），才可以將它當做參考參數傳遞。</span><span class="sxs-lookup"><span data-stu-id="6aeee-533">A variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before it can be passed as a reference parameter.</span></span> <span data-ttu-id="6aeee-534">關鍵字 `out` 後面接著*variable_reference* （[變數參考](variables.md#variable-references)），表示引數會當做輸出參數（[輸出參數](classes.md#output-parameters)）傳遞。</span><span class="sxs-lookup"><span data-stu-id="6aeee-534">The keyword `out` followed by a *variable_reference* ([Variable references](variables.md#variable-references)), indicating that the argument is passed as an output parameter ([Output parameters](classes.md#output-parameters)).</span></span> <span data-ttu-id="6aeee-535">在將變數當做輸出參數傳遞的函式成員調用之後，會將變數視為明確指派（[明確指派](variables.md#definite-assignment)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-535">A variable is considered definitely assigned ([Definite assignment](variables.md#definite-assignment)) following a function member invocation in which the variable is passed as an output parameter.</span></span>

#### <a name="corresponding-parameters"></a><span data-ttu-id="6aeee-536">對應的參數</span><span class="sxs-lookup"><span data-stu-id="6aeee-536">Corresponding parameters</span></span>

<span data-ttu-id="6aeee-537">針對引數清單中的每個引數，在所叫用的函式成員或委派中必須有對應的參數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-537">For each argument in an argument list there has to be a corresponding parameter in the function member or delegate being invoked.</span></span>

<span data-ttu-id="6aeee-538">下列所使用的參數清單的決定方式如下：</span><span class="sxs-lookup"><span data-stu-id="6aeee-538">The parameter list used in the following is determined as follows:</span></span>

*  <span data-ttu-id="6aeee-539">針對在類別中定義的虛擬方法和索引子，會從函式成員的最特定宣告或覆寫中挑選參數清單，從接收者的靜態類型開始，然後搜尋其基類。</span><span class="sxs-lookup"><span data-stu-id="6aeee-539">For virtual methods and indexers defined in classes, the parameter list is picked from the most specific declaration or override of the function member, starting with the static type of the receiver, and searching through its base classes.</span></span>
*  <span data-ttu-id="6aeee-540">如果是介面方法和索引子，則會從介面型別開始並搜尋基底介面，以從最特定的成員定義來挑選參數清單。</span><span class="sxs-lookup"><span data-stu-id="6aeee-540">For interface methods and indexers, the parameter list is picked form the most specific definition of the member, starting with the interface type and searching through the base interfaces.</span></span> <span data-ttu-id="6aeee-541">如果找不到唯一的參數清單，則會構造具有無法存取名稱的參數清單，而且不會建立選擇性參數，因此叫用不能使用已命名的參數或省略選擇性的引數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-541">If no unique parameter list is found, a parameter list with inaccessible names and no optional parameters is constructed, so that invocations cannot use named parameters or omit optional arguments.</span></span>
*  <span data-ttu-id="6aeee-542">對於部分方法，會使用定義部分方法宣告的參數清單。</span><span class="sxs-lookup"><span data-stu-id="6aeee-542">For partial methods, the parameter list of the defining partial method declaration is used.</span></span>
*  <span data-ttu-id="6aeee-543">對於所有其他函式成員和委派，只有一個參數清單，這是使用的一個。</span><span class="sxs-lookup"><span data-stu-id="6aeee-543">For all other function members and delegates there is only a single parameter list, which is the one used.</span></span>

<span data-ttu-id="6aeee-544">引數或參數的位置會定義為引數清單或參數清單中之前的引數或參數的數目。</span><span class="sxs-lookup"><span data-stu-id="6aeee-544">The position of an argument or parameter is defined as the number of arguments or parameters preceding it in the argument list or parameter list.</span></span>

<span data-ttu-id="6aeee-545">函式成員引數的對應參數建立方式如下：</span><span class="sxs-lookup"><span data-stu-id="6aeee-545">The corresponding parameters for function member arguments are established as follows:</span></span>

*  <span data-ttu-id="6aeee-546">實例函式、方法、索引子和委派的*argument_list*中的引數：</span><span class="sxs-lookup"><span data-stu-id="6aeee-546">Arguments in the *argument_list* of instance constructors, methods, indexers and delegates:</span></span>
    * <span data-ttu-id="6aeee-547">位置引數，在參數清單中的相同位置發生固定參數時，會對應至該參數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-547">A positional argument where a fixed parameter occurs at the same position in the parameter list corresponds to that parameter.</span></span>
    * <span data-ttu-id="6aeee-548">具有以其一般格式叫用之參數陣列的函式成員的位置引數，會對應到參數陣列，這必須在參數清單中的相同位置。</span><span class="sxs-lookup"><span data-stu-id="6aeee-548">A positional argument of a function member with a parameter array invoked in its normal form corresponds to the parameter  array, which must occur at the same position in the parameter list.</span></span>
    * <span data-ttu-id="6aeee-549">函式成員的位置引數，其中包含以其展開格式叫用的參數陣列，其中不會在參數清單中的相同位置發生固定參數，而會對應至參數陣列中的元素。</span><span class="sxs-lookup"><span data-stu-id="6aeee-549">A positional argument of a function member with a parameter array invoked in its expanded form, where no fixed parameter occurs at the same position in the parameter list, corresponds to an element in the parameter array.</span></span>
    * <span data-ttu-id="6aeee-550">具名引數會對應至參數清單中相同名稱的參數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-550">A named argument corresponds to the parameter of the same name in the parameter list.</span></span>
    * <span data-ttu-id="6aeee-551">針對索引子，叫用 `set` 存取子時，指定為指派運算子右運算元的運算式會對應至 @no__t 2 存取子宣告的隱含 `value` 參數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-551">For indexers, when invoking the `set` accessor, the expression specified as the right operand of the assignment operator corresponds to the implicit `value` parameter of the `set` accessor declaration.</span></span>
*  <span data-ttu-id="6aeee-552">對於屬性，當叫用 `get` 存取子時，不會有任何引數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-552">For properties, when invoking the `get` accessor there are no arguments.</span></span> <span data-ttu-id="6aeee-553">叫用 @no__t 0 存取子時，指定為指派運算子右運算元的運算式會對應至 @no__t 2 存取子宣告的隱含 `value` 參數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-553">When invoking the `set` accessor, the expression specified as the right operand of the assignment operator corresponds to the implicit `value` parameter of the `set` accessor declaration.</span></span>
*  <span data-ttu-id="6aeee-554">若為使用者定義的一元運算子（包括轉換），單一運算元會對應至運算子宣告的單一參數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-554">For user-defined unary operators (including conversions), the single operand corresponds to the single parameter of the operator declaration.</span></span>
*  <span data-ttu-id="6aeee-555">若為使用者定義的二進位運算子，左邊的運算元會對應到第一個參數，右運算元則對應至運算子宣告的第二個參數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-555">For user-defined binary operators, the left operand corresponds to the first parameter, and the right operand corresponds to the second parameter of the operator declaration.</span></span>

#### <a name="run-time-evaluation-of-argument-lists"></a><span data-ttu-id="6aeee-556">引數清單的執行時間評估</span><span class="sxs-lookup"><span data-stu-id="6aeee-556">Run-time evaluation of argument lists</span></span>

<span data-ttu-id="6aeee-557">在執行時間處理函式成員調用（動態多載[解析的編譯階段檢查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)）期間，引數清單的運算式或變數參考會依序從左至右評估，如下所示：</span><span class="sxs-lookup"><span data-stu-id="6aeee-557">During the run-time processing of a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), the expressions or variable references of an argument list are evaluated in order, from left to right, as follows:</span></span>

*  <span data-ttu-id="6aeee-558">針對值參數，會評估引數運算式，並執行對應參數類型的隱含轉換（[隱含](conversions.md#implicit-conversions)轉換）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-558">For a value parameter, the argument expression is evaluated and an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to the corresponding parameter type is performed.</span></span> <span data-ttu-id="6aeee-559">產生的值會變成函數成員調用中 value 參數的初始值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-559">The resulting value becomes the initial value of the value parameter in the function member invocation.</span></span>
*  <span data-ttu-id="6aeee-560">針對參考或輸出參數，會評估變數參考，而產生的儲存位置會變成函式成員調用中的參數所代表的儲存位置。</span><span class="sxs-lookup"><span data-stu-id="6aeee-560">For a reference or output parameter, the variable reference is evaluated and the resulting storage location becomes the storage location represented by the parameter in the function member invocation.</span></span> <span data-ttu-id="6aeee-561">如果當做參考或輸出參數提供的變數參考是*reference_type*的陣列元素，則會執行執行時間檢查，以確保陣列的元素類型與參數的類型相同。</span><span class="sxs-lookup"><span data-stu-id="6aeee-561">If the variable reference given as a reference or output parameter is an array element of a *reference_type*, a run-time check is performed to ensure that the element type of the array is identical to the type of the parameter.</span></span> <span data-ttu-id="6aeee-562">如果此檢查失敗，則會擲回 `System.ArrayTypeMismatchException`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-562">If this check fails, a `System.ArrayTypeMismatchException` is thrown.</span></span>

<span data-ttu-id="6aeee-563">方法、索引子和實例的函式可能會將其最右邊的參數宣告為參數陣列（[參數陣列](classes.md#parameter-arrays)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-563">Methods, indexers, and instance constructors may declare their right-most parameter to be a parameter array ([Parameter arrays](classes.md#parameter-arrays)).</span></span> <span data-ttu-id="6aeee-564">這類函式成員會以其一般格式叫用，或根據適用的（適用的函式[成員](expressions.md#applicable-function-member)）展開的形式來叫用：</span><span class="sxs-lookup"><span data-stu-id="6aeee-564">Such function members are invoked either in their normal form or in their expanded form depending on which is applicable ([Applicable function member](expressions.md#applicable-function-member)):</span></span>

*  <span data-ttu-id="6aeee-565">以一般格式叫用具有參數陣列的函式成員時，為參數陣列提供的引數必須是可隱含轉換（[隱含轉換](conversions.md#implicit-conversions)）為參數陣列類型的單一運算式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-565">When a function member with a parameter array is invoked in its normal form, the argument given for the parameter array must be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="6aeee-566">在此情況下，參數陣列的作用會與值參數完全相同。</span><span class="sxs-lookup"><span data-stu-id="6aeee-566">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="6aeee-567">以擴充的格式叫用具有參數陣列的函式成員時，叫用必須為參數陣列指定零個或多個位置引數，其中每個引數都是可隱含轉換的運算式（[隱含轉換](conversions.md#implicit-conversions)）至參數陣列的元素類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-567">When a function member with a parameter array is invoked in its expanded form, the invocation must specify zero or more positional arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="6aeee-568">在此情況下，調用會使用對應引數數目的長度來建立參數陣列類型的實例、使用指定的引數值初始化陣列實例的元素，並使用新建立的陣列實例作為實際的引數.</span><span class="sxs-lookup"><span data-stu-id="6aeee-568">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="6aeee-569">引數清單的運算式一律會依照寫入的順序進行評估。</span><span class="sxs-lookup"><span data-stu-id="6aeee-569">The expressions of an argument list are always evaluated in the order they are written.</span></span> <span data-ttu-id="6aeee-570">因此，範例</span><span class="sxs-lookup"><span data-stu-id="6aeee-570">Thus, the example</span></span>
```csharp
class Test
{
    static void F(int x, int y = -1, int z = -2) {
        System.Console.WriteLine("x = {0}, y = {1}, z = {2}", x, y, z);
    }

    static void Main() {
        int i = 0;
        F(i++, i++, i++);
        F(z: i++, x: i++);
    }
}
```
<span data-ttu-id="6aeee-571">產生下列輸出</span><span class="sxs-lookup"><span data-stu-id="6aeee-571">produces the output</span></span>
```console
x = 0, y = 1, z = 2
x = 4, y = -1, z = 3
```

<span data-ttu-id="6aeee-572">陣列的共同變異數規則（[陣列共變數](arrays.md#array-covariance)）允許陣列類型的值 `A[]` 是陣列類型實例的參考 `B[]`，但前提是從 `B` 到 `A` 都有隱含的參考轉換。</span><span class="sxs-lookup"><span data-stu-id="6aeee-572">The array co-variance rules ([Array covariance](arrays.md#array-covariance)) permit a value of an array type `A[]` to be a reference to an instance of an array type `B[]`, provided an implicit reference conversion exists from `B` to `A`.</span></span> <span data-ttu-id="6aeee-573">基於這些規則，當*reference_type*的 array 元素當做參考或輸出參數傳遞時，必須執行執行時間檢查，以確保陣列的實際元素類型與參數相同。</span><span class="sxs-lookup"><span data-stu-id="6aeee-573">Because of these rules, when an array element of a *reference_type* is passed as a reference or output parameter, a run-time check is required to ensure that the actual element type of the array is identical to that of the parameter.</span></span> <span data-ttu-id="6aeee-574">在範例中</span><span class="sxs-lookup"><span data-stu-id="6aeee-574">In the example</span></span>
```csharp
class Test
{
    static void F(ref object x) {...}

    static void Main() {
        object[] a = new object[10];
        object[] b = new string[10];
        F(ref a[0]);        // Ok
        F(ref b[1]);        // ArrayTypeMismatchException
    }
}
```
<span data-ttu-id="6aeee-575">`F` 的第二個調用會導致擲回 `System.ArrayTypeMismatchException`，因為 `b` 的實際元素類型是 `string`，而不是 `object`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-575">the second invocation of `F` causes a `System.ArrayTypeMismatchException` to be thrown because the actual element type of `b` is `string` and not `object`.</span></span>

<span data-ttu-id="6aeee-576">以擴充的形式叫用具有參數陣列的函式成員時，會將調用的處理方式，與使用陣列初始化運算式（[陣列建立運算式](expressions.md#array-creation-expressions)）插入展開的參數前後相同。</span><span class="sxs-lookup"><span data-stu-id="6aeee-576">When a function member with a parameter array is invoked in its expanded form, the invocation is processed exactly as if an array creation expression with an array initializer ([Array creation expressions](expressions.md#array-creation-expressions)) was inserted around the expanded parameters.</span></span> <span data-ttu-id="6aeee-577">例如，假設宣告</span><span class="sxs-lookup"><span data-stu-id="6aeee-577">For example, given the declaration</span></span>
```csharp
void F(int x, int y, params object[] args);
```
<span data-ttu-id="6aeee-578">下列方法的展開形式調用</span><span class="sxs-lookup"><span data-stu-id="6aeee-578">the following invocations of the expanded form of the method</span></span>
```csharp
F(10, 20);
F(10, 20, 30, 40);
F(10, 20, 1, "hello", 3.0);
```
<span data-ttu-id="6aeee-579">完全對應至</span><span class="sxs-lookup"><span data-stu-id="6aeee-579">correspond exactly to</span></span>
```csharp
F(10, 20, new object[] {});
F(10, 20, new object[] {30, 40});
F(10, 20, new object[] {1, "hello", 3.0});
```

<span data-ttu-id="6aeee-580">特別要注意的是，當參數陣列指定了零個引數時，就會建立空陣列。</span><span class="sxs-lookup"><span data-stu-id="6aeee-580">In particular, note that an empty array is created when there are zero arguments given for the parameter array.</span></span>

<span data-ttu-id="6aeee-581">當具有對應選擇性參數的函式成員省略引數時，會隱含地傳遞函數成員宣告的預設引數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-581">When arguments are omitted from a function member with corresponding optional parameters, the default arguments of the function member declaration are implicitly passed.</span></span> <span data-ttu-id="6aeee-582">因為這些一律是常數，所以其評估不會影響其餘引數的評估順序。</span><span class="sxs-lookup"><span data-stu-id="6aeee-582">Because these are always constant, their evaluation will not impact the evaluation order of the remaining arguments.</span></span>

### <a name="type-inference"></a><span data-ttu-id="6aeee-583">型別推斷</span><span class="sxs-lookup"><span data-stu-id="6aeee-583">Type inference</span></span>

<span data-ttu-id="6aeee-584">呼叫泛型方法時，若未指定型別引數，***型別推斷***程式會嘗試推斷呼叫的型別引數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-584">When a generic method is called without specifying type arguments, a ***type inference*** process attempts to infer type arguments for the call.</span></span> <span data-ttu-id="6aeee-585">型別推斷的存在可讓您使用更方便的語法來呼叫泛型方法，並可讓程式設計人員避免指定多餘的型別資訊。</span><span class="sxs-lookup"><span data-stu-id="6aeee-585">The presence of type inference allows a more convenient syntax to be used for calling a generic method, and allows the programmer to avoid specifying redundant type information.</span></span> <span data-ttu-id="6aeee-586">例如，假設有方法宣告：</span><span class="sxs-lookup"><span data-stu-id="6aeee-586">For example, given the method declaration:</span></span>
```csharp
class Chooser
{
    static Random rand = new Random();

    public static T Choose<T>(T first, T second) {
        return (rand.Next(2) == 0)? first: second;
    }
}
```
<span data-ttu-id="6aeee-587">不需要明確指定類型引數，就可以叫用 @no__t 0 方法：</span><span class="sxs-lookup"><span data-stu-id="6aeee-587">it is possible to invoke the `Choose` method without explicitly specifying a type argument:</span></span>
```csharp
int i = Chooser.Choose(5, 213);                 // Calls Choose<int>

string s = Chooser.Choose("foo", "bar");        // Calls Choose<string>
```

<span data-ttu-id="6aeee-588">透過型別推斷，類型引數 `int` 和 `string` 會從方法的引數中決定。</span><span class="sxs-lookup"><span data-stu-id="6aeee-588">Through type inference, the type arguments `int` and `string` are determined from the arguments to the method.</span></span>

<span data-ttu-id="6aeee-589">型別推斷會在方法調用（[方法](expressions.md#method-invocations)調用）的系結時間處理中發生，並在叫用的多載解析步驟之前進行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-589">Type inference occurs as part of the binding-time processing of a method invocation ([Method invocations](expressions.md#method-invocations)) and takes place before the overload resolution step of the invocation.</span></span> <span data-ttu-id="6aeee-590">在方法調用中指定特定方法群組，而且未在方法調用中指定任何類型引數時，類型推斷會套用至方法群組中的每個泛型方法。</span><span class="sxs-lookup"><span data-stu-id="6aeee-590">When a particular method group is specified in a method invocation, and no type arguments are specified as part of the method invocation, type inference is applied to each generic method in the method group.</span></span> <span data-ttu-id="6aeee-591">如果型別推斷成功，則會使用推斷的型別引數來決定後續多載解析的引數類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-591">If type inference succeeds, then the inferred type arguments are used to determine the types of arguments for subsequent overload resolution.</span></span> <span data-ttu-id="6aeee-592">如果多載解析選擇要叫用的泛型方法，則會使用推斷的型別引數做為叫用的實際型別引數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-592">If overload resolution chooses a generic method as the one to invoke, then the inferred type arguments are used as the actual type arguments for the invocation.</span></span> <span data-ttu-id="6aeee-593">如果特定方法的型別推斷失敗，則該方法不會參與多載解析。</span><span class="sxs-lookup"><span data-stu-id="6aeee-593">If type inference for a particular method fails, that method does not participate in overload resolution.</span></span> <span data-ttu-id="6aeee-594">型別推斷本身的失敗不會造成系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-594">The failure of type inference, in and of itself, does not cause a binding-time error.</span></span> <span data-ttu-id="6aeee-595">不過，當多載解析找不到任何適用的方法時，通常會導致系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-595">However, it often leads to a binding-time error when overload resolution then fails to find any applicable methods.</span></span>

<span data-ttu-id="6aeee-596">如果提供的引數數目與方法中的參數數目不同，則推斷會立即失敗。</span><span class="sxs-lookup"><span data-stu-id="6aeee-596">If the supplied number of arguments is different than the number of parameters in the method, then inference immediately fails.</span></span> <span data-ttu-id="6aeee-597">否則，假設泛型方法具有下列簽章：</span><span class="sxs-lookup"><span data-stu-id="6aeee-597">Otherwise, assume that the generic method has the following signature:</span></span>
```csharp
Tr M<X1,...,Xn>(T1 x1, ..., Tm xm)
```

<span data-ttu-id="6aeee-598">使用表單 `M(E1...Em)` 的方法呼叫時，類型推斷的工作是針對每個類型 @no__t 參數 `S1...Sn` 尋找唯一的類型引數，以便讓呼叫 `M<S1...Sn>(E1...Em)` 變成有效。</span><span class="sxs-lookup"><span data-stu-id="6aeee-598">With a method call of the form `M(E1...Em)` the task of type inference is to find unique type arguments `S1...Sn` for each of the type parameters `X1...Xn` so that the call `M<S1...Sn>(E1...Em)` becomes valid.</span></span>

<span data-ttu-id="6aeee-599">在推斷的過程中，每個型別參數 `Xi` 會固定為特定型別 @no__t- *2 或未*與一組相關聯的*界限*一併*修復*。</span><span class="sxs-lookup"><span data-stu-id="6aeee-599">During the process of inference each type parameter `Xi` is either *fixed* to a particular type `Si` or *unfixed* with an associated set of *bounds*.</span></span> <span data-ttu-id="6aeee-600">每個範圍都是 `T` 的類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-600">Each of the bounds is some type `T`.</span></span> <span data-ttu-id="6aeee-601">一開始，每個類型變數 `Xi` 不會與一組空的界限一併修復。</span><span class="sxs-lookup"><span data-stu-id="6aeee-601">Initially each type variable `Xi` is unfixed with an empty set of bounds.</span></span>

<span data-ttu-id="6aeee-602">型別推斷會在階段中進行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-602">Type inference takes place in phases.</span></span> <span data-ttu-id="6aeee-603">每個階段都會根據上一個階段的結果，嘗試推斷更多型別變數的型別引數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-603">Each phase will try to infer type arguments for more type variables based on the findings of the previous phase.</span></span> <span data-ttu-id="6aeee-604">第一個階段會進行界限的初始推斷，而第二個階段會將類型變數修正為特定類型，並推斷進一步的範圍。</span><span class="sxs-lookup"><span data-stu-id="6aeee-604">The first phase makes some initial inferences of bounds, whereas the second phase fixes type variables to specific types and infers further bounds.</span></span> <span data-ttu-id="6aeee-605">第二個階段可能必須重複數次。</span><span class="sxs-lookup"><span data-stu-id="6aeee-605">The second phase may have to be repeated a number of times.</span></span>

<span data-ttu-id="6aeee-606">*注意：* 只有在呼叫泛型方法時，才會進行型別推斷。</span><span class="sxs-lookup"><span data-stu-id="6aeee-606">*Note:* Type inference takes place not only when a generic method is called.</span></span> <span data-ttu-id="6aeee-607">方法群組轉換的型別推斷會在[方法群組的轉換](expressions.md#type-inference-for-conversion-of-method-groups)和尋找[一組運算式](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)的最佳一般型別中加以說明，並找出一組運算式的最佳常見型別。</span><span class="sxs-lookup"><span data-stu-id="6aeee-607">Type inference for conversion of method groups is described in [Type inference for conversion of method groups](expressions.md#type-inference-for-conversion-of-method-groups) and finding the best common type of a set of expressions is described in [Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).</span></span>

#### <a name="the-first-phase"></a><span data-ttu-id="6aeee-608">第一個階段</span><span class="sxs-lookup"><span data-stu-id="6aeee-608">The first phase</span></span>

<span data-ttu-id="6aeee-609">針對每個方法引數 `Ei`：</span><span class="sxs-lookup"><span data-stu-id="6aeee-609">For each of the method arguments `Ei`:</span></span>

*   <span data-ttu-id="6aeee-610">如果 `Ei` 是匿名函式，則*明確的參數型別推斷*（[明確的參數型別推斷](expressions.md#explicit-parameter-type-inferences)）是從 `Ei` 建立到 `Ti`</span><span class="sxs-lookup"><span data-stu-id="6aeee-610">If `Ei` is an anonymous function, an *explicit parameter type inference* ([Explicit parameter type inferences](expressions.md#explicit-parameter-type-inferences)) is made from `Ei` to `Ti`</span></span>
*   <span data-ttu-id="6aeee-611">否則，如果 `Ei` 的類型 `U`，而 `xi` 是值參數，則會*從*`U`*到*`Ti` 進行*較低限制的推斷*。</span><span class="sxs-lookup"><span data-stu-id="6aeee-611">Otherwise, if `Ei` has a type `U` and `xi` is a value parameter then a *lower-bound inference* is made *from* `U` *to* `Ti`.</span></span>
*   <span data-ttu-id="6aeee-612">否則，如果 `Ei` 的類型 `U`，而 `xi` 是 `ref` 或 `out` 參數，則會*從*`U`*到*`Ti` 進行*完全推斷*。</span><span class="sxs-lookup"><span data-stu-id="6aeee-612">Otherwise, if `Ei` has a type `U` and `xi` is a `ref` or `out` parameter then an *exact inference* is made *from* `U` *to* `Ti`.</span></span>
*   <span data-ttu-id="6aeee-613">否則，就不會對這個引數進行推斷。</span><span class="sxs-lookup"><span data-stu-id="6aeee-613">Otherwise, no inference is made for this argument.</span></span>


#### <a name="the-second-phase"></a><span data-ttu-id="6aeee-614">第二個階段</span><span class="sxs-lookup"><span data-stu-id="6aeee-614">The second phase</span></span>

<span data-ttu-id="6aeee-615">第二個階段會繼續進行，如下所示：</span><span class="sxs-lookup"><span data-stu-id="6aeee-615">The second phase proceeds as follows:</span></span>

*   <span data-ttu-id="6aeee-616">所有*未固定的類型變數*`Xi`，但不相依*于*（相依[性）任何](expressions.md#dependence)`Xj` 的修正（[修正](expressions.md#fixing)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-616">All *unfixed* type variables `Xi` which do not *depend on* ([Dependence](expressions.md#dependence)) any `Xj` are fixed ([Fixing](expressions.md#fixing)).</span></span>
*   <span data-ttu-id="6aeee-617">如果不存在這類類型變數，*則會針對*下列所有保留的所有未*固定類型變數*`Xi`：</span><span class="sxs-lookup"><span data-stu-id="6aeee-617">If no such type variables exist, all *unfixed* type variables `Xi` are *fixed* for which all of the following hold:</span></span>
    *   <span data-ttu-id="6aeee-618">至少有一個類型變數 `Xj`，相依于 `Xi`</span><span class="sxs-lookup"><span data-stu-id="6aeee-618">There is at least one type variable `Xj` that depends on `Xi`</span></span>
    *   <span data-ttu-id="6aeee-619">`Xi` 具有非空白的界限集合</span><span class="sxs-lookup"><span data-stu-id="6aeee-619">`Xi` has a non-empty set of bounds</span></span>
*   <span data-ttu-id="6aeee-620">如果沒有這類類型變數，而且仍然有未*固定*的類型變數，型別推斷就會失敗。</span><span class="sxs-lookup"><span data-stu-id="6aeee-620">If no such type variables exist and there are still *unfixed* type variables, type inference fails.</span></span>
*   <span data-ttu-id="6aeee-621">否則，如果沒有任何進一步的未*固定*類型變數存在，型別推斷就會成功。</span><span class="sxs-lookup"><span data-stu-id="6aeee-621">Otherwise, if no further *unfixed* type variables exist, type inference succeeds.</span></span>
*   <span data-ttu-id="6aeee-622">否則，所有引數 `Ei` 搭配對應的參數類型 `Ti`，其中*輸出類型*（[輸出類型](expressions.md#output-types)）包含未*固定*的類型變數 `Xj`，但*輸入類型*（[輸入類型](expressions.md#input-types)）不是， *輸出類型推斷*（[輸出類型推斷](expressions.md#output-type-inferences)）是*從*1*到*3 進行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-622">Otherwise, for all arguments `Ei` with corresponding parameter type `Ti` where the *output types* ([Output types](expressions.md#output-types)) contain *unfixed* type variables `Xj` but the *input types* ([Input types](expressions.md#input-types)) do not, an *output type inference* ([Output type inferences](expressions.md#output-type-inferences)) is made *from* `Ei` *to* `Ti`.</span></span> <span data-ttu-id="6aeee-623">然後會重複第二個階段。</span><span class="sxs-lookup"><span data-stu-id="6aeee-623">Then the second phase is repeated.</span></span>

#### <a name="input-types"></a><span data-ttu-id="6aeee-624">輸入類型</span><span class="sxs-lookup"><span data-stu-id="6aeee-624">Input types</span></span>

<span data-ttu-id="6aeee-625">如果 `E` 是方法群組或隱含型別匿名函式，而 `T` 是委派型別或運算式樹狀架構型別，則 `T` 的所有參數型別都是*型*別為 `T` 的*輸入*型別 `E`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-625">If `E` is a method group or implicitly typed anonymous function and `T` is a delegate type or expression tree type then all the parameter types of `T` are *input types* of `E` *with type* `T`.</span></span>

####  <a name="output-types"></a><span data-ttu-id="6aeee-626">輸出類型</span><span class="sxs-lookup"><span data-stu-id="6aeee-626">Output types</span></span>

<span data-ttu-id="6aeee-627">如果 `E` 是方法群組或匿名函式，而 `T` 是委派型別或運算式樹狀架構型別，則 `T` 的傳回型別就是*類型*`T` 的*輸出型別為*`E`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-627">If `E` is a method group or an anonymous function and `T` is a delegate type or expression tree type then the return type of `T` is an *output type of* `E` *with type* `T`.</span></span>

#### <a name="dependence"></a><span data-ttu-id="6aeee-628">夜間</span><span class="sxs-lookup"><span data-stu-id="6aeee-628">Dependence</span></span>

<span data-ttu-id="6aeee-629">未*固定的類型變數*`Xi` 會*直接相依于*未固定的類型變數 `Xj`，如果適用于具有類型 `Tk` 的部分引數 `Ek`，類型 `Xj`，`Tk` 的*輸入類型*發生，而 0 則發生在類型為 3 的 2 輸出類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-629">An *unfixed* type variable `Xi` *depends directly on* an unfixed type variable `Xj` if for some argument `Ek` with type `Tk` `Xj` occurs in an *input type* of `Ek` with type `Tk` and `Xi` occurs in an *output type* of `Ek` with type `Tk`.</span></span>

<span data-ttu-id="6aeee-630">`Xj`*取決*于 `Xi`，如果 `Xj`*直接相依于*`Xi`，或 `Xi`*直接相依于*`Xk`，而 `Xk` 則*取決*于 1。</span><span class="sxs-lookup"><span data-stu-id="6aeee-630">`Xj` *depends on* `Xi` if `Xj` *depends directly on* `Xi` or if `Xi` *depends directly on* `Xk` and `Xk` *depends on* `Xj`.</span></span> <span data-ttu-id="6aeee-631">因此「相依于」是可轉移的，但不是「直接相依于」的反的關閉。</span><span class="sxs-lookup"><span data-stu-id="6aeee-631">Thus "depends on" is the transitive but not reflexive closure of "depends directly on".</span></span>

#### <a name="output-type-inferences"></a><span data-ttu-id="6aeee-632">輸出類型推斷</span><span class="sxs-lookup"><span data-stu-id="6aeee-632">Output type inferences</span></span>

<span data-ttu-id="6aeee-633">*輸出類型推斷*是*從*運算式 `E` 建立*為*類型 `T`，方法如下：</span><span class="sxs-lookup"><span data-stu-id="6aeee-633">An *output type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>

*  <span data-ttu-id="6aeee-634">如果 `E` 是具有推斷傳回類型的匿名函式 `U` （[推斷傳回類型](expressions.md#inferred-return-type)），而 `T` 是具有傳回類型 `Tb` 的委派類型或運算式樹狀結構類型，則為*下限推斷*（[下限推斷](expressions.md#lower-bound-inferences)）是*從*`U`*到*0 進行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-634">If `E` is an anonymous function with inferred return type  `U` ([Inferred return type](expressions.md#inferred-return-type)) and `T` is a delegate type or expression tree type with return type `Tb`, then a *lower-bound inference* ([Lower-bound inferences](expressions.md#lower-bound-inferences)) is made *from* `U` *to* `Tb`.</span></span>
*  <span data-ttu-id="6aeee-635">否則，如果 `E` 是方法群組，而 `T` 是具有參數類型的委派類型或運算式樹狀結構類型 `T1...Tk` 和傳回類型 `Tb`，且類型 `T1...Tk` 的多載解析為 `E`，則會產生傳回類型 `U` 的單一方法。之後，會*從*`U`*到*1，進行*較低*系結的推斷。</span><span class="sxs-lookup"><span data-stu-id="6aeee-635">Otherwise, if `E` is a method group and `T` is a delegate type or expression tree type with parameter types `T1...Tk` and return type `Tb`, and overload resolution of `E` with the types `T1...Tk` yields a single method with return type `U`, then a *lower-bound inference* is made *from* `U` *to* `Tb`.</span></span>
*  <span data-ttu-id="6aeee-636">否則，如果 `E` 是類型 `U` 的運算式，則會*從*`U`*到*`T` 進行*較低*系結的推斷。</span><span class="sxs-lookup"><span data-stu-id="6aeee-636">Otherwise, if `E` is an expression with type `U`, then a *lower-bound inference* is made *from* `U` *to* `T`.</span></span>
*  <span data-ttu-id="6aeee-637">否則，就不會進行推斷。</span><span class="sxs-lookup"><span data-stu-id="6aeee-637">Otherwise, no inferences are made.</span></span>

#### <a name="explicit-parameter-type-inferences"></a><span data-ttu-id="6aeee-638">明確參數型別推斷</span><span class="sxs-lookup"><span data-stu-id="6aeee-638">Explicit parameter type inferences</span></span>

<span data-ttu-id="6aeee-639">*明確的參數型別推斷*是*從*運算式 `E` 建立*為*型別 `T`，方法如下：</span><span class="sxs-lookup"><span data-stu-id="6aeee-639">An *explicit parameter type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>

*  <span data-ttu-id="6aeee-640">如果 `E` 是明確類型的匿名函式，且參數類型為 `U1...Uk`，而且 `T` 是具有參數類型 `V1...Vk` 的委派類型或運算式樹狀結構類型，然後針對每個`Ui` 進行 *完全推斷*（[確切推斷](expressions.md#exact-inferences)）*從*`Ui`*到*對應的 0。</span><span class="sxs-lookup"><span data-stu-id="6aeee-640">If `E` is an explicitly typed anonymous function with parameter types `U1...Uk` and `T` is a delegate type or expression tree type with parameter types `V1...Vk` then for each `Ui` an *exact inference* ([Exact inferences](expressions.md#exact-inferences)) is made *from* `Ui` *to* the corresponding `Vi`.</span></span>

#### <a name="exact-inferences"></a><span data-ttu-id="6aeee-641">確切推斷</span><span class="sxs-lookup"><span data-stu-id="6aeee-641">Exact inferences</span></span>

<span data-ttu-id="6aeee-642">*從*類型 `U`*到*類型的*確切推斷*，`V`，如下所示：</span><span class="sxs-lookup"><span data-stu-id="6aeee-642">An *exact inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="6aeee-643">如果 `V` 是其中一個未*修復*的 `Xi`，則 `U` 新增至 `Xi` 的確切界限集合。</span><span class="sxs-lookup"><span data-stu-id="6aeee-643">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of exact bounds for `Xi`.</span></span>

*  <span data-ttu-id="6aeee-644">否則，會藉由檢查是否有下列情況，來判斷 `V1...Vk` 和 `U1...Uk`：</span><span class="sxs-lookup"><span data-stu-id="6aeee-644">Otherwise, sets `V1...Vk` and `U1...Uk` are determined by checking if any of the following cases apply:</span></span>

   *  <span data-ttu-id="6aeee-645">`V` 是陣列類型 `V1[...]`，而 `U` 是相同次序 `U1[...]` 的陣列類型</span><span class="sxs-lookup"><span data-stu-id="6aeee-645">`V` is an array type `V1[...]` and `U` is an array type `U1[...]`  of the same rank</span></span>
   *  <span data-ttu-id="6aeee-646">`V` 是類型 `V1?`，而 `U` 是類型 `U1?`</span><span class="sxs-lookup"><span data-stu-id="6aeee-646">`V` is the type `V1?` and `U` is the type `U1?`</span></span>
   *  <span data-ttu-id="6aeee-647">`V` 是結構化型別 `C<V1...Vk>`and `U` 是結構化型別 `C<U1...Uk>`</span><span class="sxs-lookup"><span data-stu-id="6aeee-647">`V` is a constructed type `C<V1...Vk>`and `U` is a constructed type `C<U1...Uk>`</span></span>

   <span data-ttu-id="6aeee-648">如果套用上述任何一種情況，則會*從*每個 @no__t *-2 對*對應的 `Vi` 進行*完全推斷*。</span><span class="sxs-lookup"><span data-stu-id="6aeee-648">If any of these cases apply then an *exact inference* is made *from* each `Ui` *to* the corresponding `Vi`.</span></span>

*  <span data-ttu-id="6aeee-649">否則不會進行推斷。</span><span class="sxs-lookup"><span data-stu-id="6aeee-649">Otherwise no inferences are made.</span></span>

#### <a name="lower-bound-inferences"></a><span data-ttu-id="6aeee-650">下限推斷</span><span class="sxs-lookup"><span data-stu-id="6aeee-650">Lower-bound inferences</span></span>

<span data-ttu-id="6aeee-651">*從*類型 `U`*到*類型 `V` 的*下限推斷*，如下所示：</span><span class="sxs-lookup"><span data-stu-id="6aeee-651">A *lower-bound inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="6aeee-652">如果 `V` 是其中一個未*修復*的 `Xi`，則 `U` 新增至 `Xi` 的下限集合。</span><span class="sxs-lookup"><span data-stu-id="6aeee-652">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of lower bounds for `Xi`.</span></span>
*  <span data-ttu-id="6aeee-653">否則，如果 `V` 是類型 `V1?`and `U` 是 `U1?` 的類型，則會從 `U1` 到 `V1` 進行較低的系結推斷。</span><span class="sxs-lookup"><span data-stu-id="6aeee-653">Otherwise, if `V` is the type `V1?`and `U` is the type `U1?` then a lower bound inference is made from `U1` to `V1`.</span></span>
*  <span data-ttu-id="6aeee-654">否則，會藉由檢查是否有下列情況，來判斷 `U1...Uk` 和 `V1...Vk`：</span><span class="sxs-lookup"><span data-stu-id="6aeee-654">Otherwise, sets `U1...Uk` and `V1...Vk` are determined by checking if any of the following cases apply:</span></span>
   *  <span data-ttu-id="6aeee-655">`V` 是陣列類型 `V1[...]`，而 `U` 是相同次序的陣列類型 `U1[...]` （或其有效基底類型為 `U1[...]` 的類型參數）</span><span class="sxs-lookup"><span data-stu-id="6aeee-655">`V` is an array type `V1[...]` and `U` is an array type `U1[...]` (or a type parameter whose effective base type is `U1[...]`) of the same rank</span></span>
   *  <span data-ttu-id="6aeee-656">`V` 是 `IEnumerable<V1>` 的其中一個，`ICollection<V1>` 或 `IList<V1>`，而 `U` 是一維陣列類型 `U1[]` （或有效基底類型為 `U1[]` 的類型參數）</span><span class="sxs-lookup"><span data-stu-id="6aeee-656">`V` is one of `IEnumerable<V1>`, `ICollection<V1>` or `IList<V1>` and `U` is a one-dimensional array type `U1[]`(or a type parameter whose effective base type is `U1[]`)</span></span>
   *  <span data-ttu-id="6aeee-657">`V` 是結構化的類別、結構、介面或委派類型 `C<V1...Vk>`，而且有一個唯一類型 `C<U1...Uk>`，例如 `U` （或者，如果 `U` 是類型參數、其有效的基類或其有效介面集的任何成員）與、繼承自（直接或間接），或執行（直接或間接） `C<U1...Uk>`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-657">`V` is a constructed class, struct, interface or delegate type `C<V1...Vk>` and there is a unique type `C<U1...Uk>` such that `U` (or, if `U` is a type parameter, its effective base class or any member of its effective interface set) is identical to, inherits from (directly or indirectly), or implements (directly or indirectly) `C<U1...Uk>`.</span></span>

      <span data-ttu-id="6aeee-658">（「唯一性」限制表示在案例介面中 `C<T> {} class U: C<X>, C<Y> {}`，從 `U` 推斷到 `C<T>` 時，不會進行推斷，因為 `U1` 可能會 `X` 或 `Y`）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-658">(The "uniqueness" restriction means that in the case interface `C<T> {} class U: C<X>, C<Y> {}`, then no inference is made when inferring from `U` to `C<T>` because `U1` could be `X` or `Y`.)</span></span>

   <span data-ttu-id="6aeee-659">如果套用上述任何一種情況，則會*從*每個 @no__t *-1 對*對應的 `Vi` 進行推斷，如下所示：</span><span class="sxs-lookup"><span data-stu-id="6aeee-659">If any of these cases apply then an inference is made *from* each `Ui` *to* the corresponding `Vi` as follows:</span></span>

   *  <span data-ttu-id="6aeee-660">如果 `Ui` 已知不是參考型別，則會進行*完全推斷*</span><span class="sxs-lookup"><span data-stu-id="6aeee-660">If `Ui` is not known to be a reference type then an *exact inference* is made</span></span>
   *  <span data-ttu-id="6aeee-661">否則，如果 `U` 是陣列類型，則會進行*較低界限的推斷*</span><span class="sxs-lookup"><span data-stu-id="6aeee-661">Otherwise, if `U` is an array type then a *lower-bound inference* is made</span></span>
   *  <span data-ttu-id="6aeee-662">否則，如果 `V` 是 `C<V1...Vk>`，則推斷取決於 `C` 的第 i 個類型參數：</span><span class="sxs-lookup"><span data-stu-id="6aeee-662">Otherwise, if `V` is `C<V1...Vk>` then inference depends on the i-th type parameter of `C`:</span></span>
      *  <span data-ttu-id="6aeee-663">如果它是協變數，則會進行*較低界限的推斷*。</span><span class="sxs-lookup"><span data-stu-id="6aeee-663">If it is covariant then a *lower-bound inference* is made.</span></span>
      *  <span data-ttu-id="6aeee-664">如果它是逆變性，則會進行*上限推斷*。</span><span class="sxs-lookup"><span data-stu-id="6aeee-664">If it is contravariant then an *upper-bound inference* is made.</span></span>
      *  <span data-ttu-id="6aeee-665">如果它是不變的，則會進行*完全推斷*。</span><span class="sxs-lookup"><span data-stu-id="6aeee-665">If it is invariant then an *exact inference* is made.</span></span>
*  <span data-ttu-id="6aeee-666">否則，就不會進行推斷。</span><span class="sxs-lookup"><span data-stu-id="6aeee-666">Otherwise, no inferences are made.</span></span>

#### <a name="upper-bound-inferences"></a><span data-ttu-id="6aeee-667">上限推斷</span><span class="sxs-lookup"><span data-stu-id="6aeee-667">Upper-bound inferences</span></span>

<span data-ttu-id="6aeee-668">*從*類型 `U`*到*類型 `V` 的*上限推斷*，如下所示：</span><span class="sxs-lookup"><span data-stu-id="6aeee-668">An *upper-bound inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="6aeee-669">如果 `V` 是其中一個未*修復*的 `Xi`，則會將 `U` 新增至 `Xi` 的上限。</span><span class="sxs-lookup"><span data-stu-id="6aeee-669">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of upper bounds for `Xi`.</span></span>
*  <span data-ttu-id="6aeee-670">否則，會藉由檢查是否有下列情況，來判斷 `V1...Vk` 和 `U1...Uk`：</span><span class="sxs-lookup"><span data-stu-id="6aeee-670">Otherwise, sets `V1...Vk` and `U1...Uk` are determined by checking if any of the following cases apply:</span></span>
   *  <span data-ttu-id="6aeee-671">`U` 是陣列類型 `U1[...]`，而 `V` 是相同次序 `V1[...]` 的陣列類型</span><span class="sxs-lookup"><span data-stu-id="6aeee-671">`U` is an array type `U1[...]` and `V` is an array type `V1[...]` of the same rank</span></span>
   *  <span data-ttu-id="6aeee-672">`U` 是 `IEnumerable<Ue>` 的其中一個，`ICollection<Ue>` 或 `IList<Ue>`，而 `V` 是一維陣列類型 `Ve[]`</span><span class="sxs-lookup"><span data-stu-id="6aeee-672">`U` is one of `IEnumerable<Ue>`, `ICollection<Ue>` or `IList<Ue>` and `V` is a one-dimensional array type `Ve[]`</span></span>
   *  <span data-ttu-id="6aeee-673">`U` 是類型 `U1?`，而 `V` 是類型 `V1?`</span><span class="sxs-lookup"><span data-stu-id="6aeee-673">`U` is the type `U1?` and `V` is the type `V1?`</span></span>
   *  <span data-ttu-id="6aeee-674">`U` 是結構化的類別、結構、介面或委派類型 `C<U1...Uk>`，而 `V` 是與相同的類別、結構、介面或委派類型、繼承自（直接或間接），或直接或間接執行唯一類型 `C<V1...Vk>`</span><span class="sxs-lookup"><span data-stu-id="6aeee-674">`U` is constructed class, struct, interface or delegate type `C<U1...Uk>` and `V` is a class, struct, interface or delegate type which is identical to, inherits from (directly or indirectly), or implements (directly or indirectly) a unique type `C<V1...Vk>`</span></span>

      <span data-ttu-id="6aeee-675">（「唯一性」限制表示如果我們有 `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`，則從 `C<U1>` 推斷到 `V<Q>` 時，不會進行推斷。</span><span class="sxs-lookup"><span data-stu-id="6aeee-675">(The "uniqueness" restriction means that if we have `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, then no inference is made when inferring from `C<U1>` to `V<Q>`.</span></span> <span data-ttu-id="6aeee-676">推斷不會從 `U1` 到 `X<Q>` 或 `Y<Q>` 進行。）</span><span class="sxs-lookup"><span data-stu-id="6aeee-676">Inferences are not made from `U1` to either `X<Q>` or `Y<Q>`.)</span></span>

   <span data-ttu-id="6aeee-677">如果套用上述任何一種情況，則會*從*每個 @no__t *-1 對*對應的 `Vi` 進行推斷，如下所示：</span><span class="sxs-lookup"><span data-stu-id="6aeee-677">If any of these cases apply then an inference is made *from* each `Ui` *to* the corresponding `Vi` as follows:</span></span>
   *  <span data-ttu-id="6aeee-678">如果 `Ui` 已知不是參考型別，則會進行*完全推斷*</span><span class="sxs-lookup"><span data-stu-id="6aeee-678">If  `Ui` is not known to be a reference type then an *exact inference* is made</span></span>
   *  <span data-ttu-id="6aeee-679">否則，如果 `V` 是陣列類型，則會進行*上限推斷*</span><span class="sxs-lookup"><span data-stu-id="6aeee-679">Otherwise, if `V` is an array type then an *upper-bound inference* is made</span></span>
   *  <span data-ttu-id="6aeee-680">否則，如果 `U` 是 `C<U1...Uk>`，則推斷取決於 `C` 的第 i 個類型參數：</span><span class="sxs-lookup"><span data-stu-id="6aeee-680">Otherwise, if `U` is `C<U1...Uk>` then inference depends on the i-th type parameter of `C`:</span></span>
      *  <span data-ttu-id="6aeee-681">如果它是協變數，則會進行*上限推斷*。</span><span class="sxs-lookup"><span data-stu-id="6aeee-681">If it is covariant then an *upper-bound inference* is made.</span></span>
      *  <span data-ttu-id="6aeee-682">如果是反變數，則會進行*較低界限的推斷*。</span><span class="sxs-lookup"><span data-stu-id="6aeee-682">If it is contravariant then a *lower-bound inference* is made.</span></span>
      *  <span data-ttu-id="6aeee-683">如果它是不變的，則會進行*完全推斷*。</span><span class="sxs-lookup"><span data-stu-id="6aeee-683">If it is invariant then an *exact inference* is made.</span></span>
*  <span data-ttu-id="6aeee-684">否則，就不會進行推斷。</span><span class="sxs-lookup"><span data-stu-id="6aeee-684">Otherwise, no inferences are made.</span></span>   

#### <a name="fixing"></a><span data-ttu-id="6aeee-685">固定</span><span class="sxs-lookup"><span data-stu-id="6aeee-685">Fixing</span></span>

<span data-ttu-id="6aeee-686">具有一組界限的未*固定*類型變數 `Xi`，如下*所示：*</span><span class="sxs-lookup"><span data-stu-id="6aeee-686">An *unfixed* type variable `Xi` with a set of bounds is *fixed* as follows:</span></span>

*  <span data-ttu-id="6aeee-687">一組*候選類型*`Uj` 會當做 `Xi` 之界限集合中的所有類型一併啟動。</span><span class="sxs-lookup"><span data-stu-id="6aeee-687">The set of *candidate types* `Uj` starts out as the set of all types in the set of bounds for `Xi`.</span></span>
*  <span data-ttu-id="6aeee-688">然後，我們會依次檢查 `Xi` 的每個系結：針對每個完全系結 `U` （`Xi`），`Uj` 的所有類型都不完全相同，`U` 則會從候選集合中移除。</span><span class="sxs-lookup"><span data-stu-id="6aeee-688">We then examine each bound for `Xi` in turn: For each exact bound `U` of `Xi` all types `Uj` which are not identical to `U` are removed from the candidate set.</span></span> <span data-ttu-id="6aeee-689">針對每個下限 @no__t-`Xi` 的所有類型 `Uj`，其中*不*會從候選集合中移除 `U` 的隱含轉換。</span><span class="sxs-lookup"><span data-stu-id="6aeee-689">For each lower bound `U` of `Xi` all types `Uj` to which there is *not* an implicit conversion from `U` are removed from the candidate set.</span></span> <span data-ttu-id="6aeee-690">針對每個上限 @no__t-`Xi` 的所有類型 @no__t- *2，其中*沒有隱含轉換成 `U` 會從候選集合中移除。</span><span class="sxs-lookup"><span data-stu-id="6aeee-690">For each upper bound `U` of `Xi` all types `Uj` from which there is *not* an implicit conversion to `U` are removed from the candidate set.</span></span>
*  <span data-ttu-id="6aeee-691">如果剩餘的候選類型 `Uj`，則會有唯一的類型 `V`，其中隱含地轉換為所有其他候選類型，然後 `Xi` 固定為 `V`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-691">If among the remaining candidate types `Uj` there is a unique type `V` from which there is an implicit conversion to all the other candidate types, then `Xi` is fixed to `V`.</span></span>
*  <span data-ttu-id="6aeee-692">否則，型別推斷會失敗。</span><span class="sxs-lookup"><span data-stu-id="6aeee-692">Otherwise, type inference fails.</span></span>

#### <a name="inferred-return-type"></a><span data-ttu-id="6aeee-693">推斷的傳回型別</span><span class="sxs-lookup"><span data-stu-id="6aeee-693">Inferred return type</span></span>

<span data-ttu-id="6aeee-694">匿名函式的推斷傳回型別 `F` 會在型別推斷和多載解析期間使用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-694">The inferred return type of an anonymous function `F` is used during type inference and overload resolution.</span></span> <span data-ttu-id="6aeee-695">只能針對所有參數類型為已知的匿名函式來判斷推斷的傳回類型，因為它們是明確指定的，可透過匿名函式轉換來提供，或在封閉式泛型的型別推斷期間推斷方法調用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-695">The inferred return type can only be determined for an anonymous function where all parameter types are known, either because they are explicitly given, provided through an anonymous function conversion or inferred during type inference on an enclosing generic method invocation.</span></span>

<span data-ttu-id="6aeee-696">***推斷的結果類型***的判斷方式如下：</span><span class="sxs-lookup"><span data-stu-id="6aeee-696">The ***inferred result type*** is determined as follows:</span></span>

*  <span data-ttu-id="6aeee-697">如果 `F` 的主體是具有型別的*運算式*，則 `F` 的推斷結果型別就是該運算式的型別。</span><span class="sxs-lookup"><span data-stu-id="6aeee-697">If the body of `F` is an *expression* that has a type, then the inferred result type of `F` is the type of that expression.</span></span>
*  <span data-ttu-id="6aeee-698">如果 `F` 的主體是*區塊*，而且區塊的 @no__t 2 語句中的運算式集合具有最佳一般類型 `T` （[尋找一組運算式的最佳一般類型](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)），則會 `T` 推斷結果 @no__t 類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-698">If the body of `F` is a *block* and the set of expressions in the block's `return` statements has a best common type `T` ([Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), then the inferred result type of `F` is `T`.</span></span>
*  <span data-ttu-id="6aeee-699">否則，將無法推斷 `F` 的結果類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-699">Otherwise, a result type cannot be inferred for `F`.</span></span>

<span data-ttu-id="6aeee-700">推斷的傳回***型***別的判斷方式如下：</span><span class="sxs-lookup"><span data-stu-id="6aeee-700">The ***inferred return type*** is determined as follows:</span></span>

*  <span data-ttu-id="6aeee-701">如果 `F` 是非同步，且 `F` 的主體是分類為無任何（[運算式分類](expressions.md#expression-classifications)）的運算式，或沒有 return 語句的語句區塊具有運算式，則推斷的傳回類型會 `System.Threading.Tasks.Task`</span><span class="sxs-lookup"><span data-stu-id="6aeee-701">If `F` is async and the body of `F` is either an expression classified as nothing ([Expression classifications](expressions.md#expression-classifications)), or a statement block where no return statements have expressions, the inferred return type is `System.Threading.Tasks.Task`</span></span>
*  <span data-ttu-id="6aeee-702">如果 `F` 是非同步，而且有推斷的結果型別 `T`，則推斷的傳回型別會 `System.Threading.Tasks.Task<T>`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-702">If `F` is async and has an inferred result type `T`, the inferred return type is `System.Threading.Tasks.Task<T>`.</span></span>
*  <span data-ttu-id="6aeee-703">如果 `F` 是非非同步，而且有推斷的結果型別 `T`，則推斷的傳回型別會 `T`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-703">If `F` is non-async and has an inferred result type `T`, the inferred return type is `T`.</span></span>
*  <span data-ttu-id="6aeee-704">否則，將無法推斷 `F` 的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="6aeee-704">Otherwise a return type cannot be inferred for `F`.</span></span>

<span data-ttu-id="6aeee-705">做為涉及匿名函式的型別推斷範例，請考慮在 `System.Linq.Enumerable` 類別中宣告的 @no__t 0 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="6aeee-705">As an example of type inference involving anonymous functions, consider the `Select` extension method declared in the `System.Linq.Enumerable` class:</span></span>
```csharp
namespace System.Linq
{
    public static class Enumerable
    {
        public static IEnumerable<TResult> Select<TSource,TResult>(
            this IEnumerable<TSource> source,
            Func<TSource,TResult> selector)
        {
            foreach (TSource element in source) yield return selector(element);
        }
    }
}
```

<span data-ttu-id="6aeee-706">假設已使用 `using` 子句匯入 `System.Linq` 命名空間，且給定的類別 `Customer`，並具有類型 `string` 的 `Name` 屬性，則可以使用 @no__t 5 方法來選取客戶清單的名稱：</span><span class="sxs-lookup"><span data-stu-id="6aeee-706">Assuming the `System.Linq` namespace was imported with a `using` clause, and given a class `Customer` with a `Name` property of type `string`, the `Select` method can be used to select the names of a list of customers:</span></span>
```csharp
List<Customer> customers = GetCustomerList();
IEnumerable<string> names = customers.Select(c => c.Name);
```

<span data-ttu-id="6aeee-707">@No__t-1 的擴充方法叫用（[擴充方法調用](expressions.md#extension-method-invocations)）是藉由將調用重新撰寫為靜態方法調用來處理：</span><span class="sxs-lookup"><span data-stu-id="6aeee-707">The extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)) of `Select` is processed by rewriting the invocation to a static method invocation:</span></span>
```csharp
IEnumerable<string> names = Enumerable.Select(customers, c => c.Name);
```

<span data-ttu-id="6aeee-708">由於未明確指定類型引數，因此會使用型別推斷來推斷型別引數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-708">Since type arguments were not explicitly specified, type inference is used to infer the type arguments.</span></span> <span data-ttu-id="6aeee-709">首先，`customers` 引數與 `source` 參數相關，並將 `T` 推斷為 `Customer`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-709">First, the `customers` argument is related to the `source` parameter, inferring `T` to be `Customer`.</span></span> <span data-ttu-id="6aeee-710">然後，使用上述的匿名函式型別推斷程式，`c` 的類型 `Customer`，而運算式 `c.Name` 則與 `selector` 參數的傳回類型相關，推斷 `S` 為 `string`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-710">Then, using the anonymous function type inference process described above, `c` is given type `Customer`, and the expression `c.Name` is related to the return type of the `selector` parameter, inferring `S` to be `string`.</span></span> <span data-ttu-id="6aeee-711">因此，調用相當於</span><span class="sxs-lookup"><span data-stu-id="6aeee-711">Thus, the invocation is equivalent to</span></span>
```csharp
Sequence.Select<Customer,string>(customers, (Customer c) => c.Name)
```
<span data-ttu-id="6aeee-712">而結果的類型為 `IEnumerable<string>`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-712">and the result is of type `IEnumerable<string>`.</span></span>

<span data-ttu-id="6aeee-713">下列範例示範匿名函式型別推斷如何在泛型方法調用中的引數之間，使用「flow」型別資訊。</span><span class="sxs-lookup"><span data-stu-id="6aeee-713">The following example demonstrates how anonymous function type inference allows type information to "flow" between arguments in a generic method invocation.</span></span> <span data-ttu-id="6aeee-714">假設有方法：</span><span class="sxs-lookup"><span data-stu-id="6aeee-714">Given the method:</span></span>
```csharp
static Z F<X,Y,Z>(X value, Func<X,Y> f1, Func<Y,Z> f2) {
    return f2(f1(value));
}
```

<span data-ttu-id="6aeee-715">調用的型別推斷：</span><span class="sxs-lookup"><span data-stu-id="6aeee-715">Type inference for the invocation:</span></span>
```csharp
double seconds = F("1:15:30", s => TimeSpan.Parse(s), t => t.TotalSeconds);
```
<span data-ttu-id="6aeee-716">繼續進行，如下所示：第一，`"1:15:30"` 的引數與 @no__t 1 參數相關，推斷 `X` 為 `string`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-716">proceeds as follows: First, the argument `"1:15:30"` is related to the `value` parameter, inferring `X` to be `string`.</span></span> <span data-ttu-id="6aeee-717">然後，第一個匿名函式的參數 `s`，提供推斷的型別 `string`，而運算式 `TimeSpan.Parse(s)` 與 `f1` 的傳回型別相關，推斷 `Y` 為 `System.TimeSpan`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-717">Then, the parameter of the first anonymous function, `s`, is given the inferred type `string`, and the expression `TimeSpan.Parse(s)` is related to the return type of `f1`, inferring `Y` to be `System.TimeSpan`.</span></span> <span data-ttu-id="6aeee-718">最後，第二個匿名函式的參數 `t`，提供推斷的型別 `System.TimeSpan`，而運算式 `t.TotalSeconds` 與 `f2` 的傳回型別相關，推斷 `Z` 為 `double`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-718">Finally, the parameter of the second anonymous function, `t`, is given the inferred type `System.TimeSpan`, and the expression `t.TotalSeconds` is related to the return type of `f2`, inferring `Z` to be `double`.</span></span> <span data-ttu-id="6aeee-719">因此，叫用的結果會是類型 `double`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-719">Thus, the result of the invocation is of type `double`.</span></span>

#### <a name="type-inference-for-conversion-of-method-groups"></a><span data-ttu-id="6aeee-720">方法群組轉換的型別推斷</span><span class="sxs-lookup"><span data-stu-id="6aeee-720">Type inference for conversion of method groups</span></span>

<span data-ttu-id="6aeee-721">類似于泛型方法的呼叫，當包含泛型方法 `M` 的方法群組轉換成給定的委派類型時，也必須套用型別推斷，`D` （[方法群組轉換](conversions.md#method-group-conversions)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-721">Similar to calls of generic methods, type inference must also be applied when a method group `M` containing a generic method is converted to a given delegate type `D` ([Method group conversions](conversions.md#method-group-conversions)).</span></span> <span data-ttu-id="6aeee-722">提供方法</span><span class="sxs-lookup"><span data-stu-id="6aeee-722">Given a method</span></span>
```csharp
Tr M<X1...Xn>(T1 x1 ... Tm xm)
```
<span data-ttu-id="6aeee-723">和方法群組 `M` 被指派給委派型別 `D`，型別推斷的工作是尋找 `S1...Sn` 的型別引數，讓運算式：</span><span class="sxs-lookup"><span data-stu-id="6aeee-723">and the method group `M` being assigned to the delegate type `D` the task of type inference is to find type arguments `S1...Sn` so that the expression:</span></span>
```csharp
M<S1...Sn>
```
<span data-ttu-id="6aeee-724">會變成與 `D` 相容（[委派](delegates.md#delegate-declarations)宣告）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-724">becomes compatible ([Delegate declarations](delegates.md#delegate-declarations)) with `D`.</span></span>

<span data-ttu-id="6aeee-725">不同于泛型方法呼叫的類型推斷演算法，在此情況下，只有引數*類型*，沒有引數*運算式*。</span><span class="sxs-lookup"><span data-stu-id="6aeee-725">Unlike the type inference algorithm for generic method calls, in this case there are only argument *types*, no argument *expressions*.</span></span> <span data-ttu-id="6aeee-726">特別的是，沒有任何匿名函式，因此不需要推斷的多個階段。</span><span class="sxs-lookup"><span data-stu-id="6aeee-726">In particular, there are no anonymous functions and hence no need for multiple phases of inference.</span></span>

<span data-ttu-id="6aeee-727">相反地，所有 `Xi` 都會視為未*固定*，而且會*從*`D` 的每個引數型別 `Uj`*到*對應的參數類型（`M` 的 @no__t）進行*較低*的系結。</span><span class="sxs-lookup"><span data-stu-id="6aeee-727">Instead, all `Xi` are considered *unfixed*, and a *lower-bound inference* is made *from* each argument type `Uj` of `D` *to* the corresponding parameter type `Tj` of `M`.</span></span> <span data-ttu-id="6aeee-728">如果 `Xi` 找不到任何界限，則型別推斷會失敗。</span><span class="sxs-lookup"><span data-stu-id="6aeee-728">If for any of the `Xi` no bounds were found, type inference fails.</span></span> <span data-ttu-id="6aeee-729">否則，所有 `Xi` 都會*固定*為對應的 `Si`，也就是型別推斷的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-729">Otherwise, all `Xi` are *fixed* to corresponding `Si`, which are the result of type inference.</span></span>

#### <a name="finding-the-best-common-type-of-a-set-of-expressions"></a><span data-ttu-id="6aeee-730">尋找一組運算式的最佳一般類型</span><span class="sxs-lookup"><span data-stu-id="6aeee-730">Finding the best common type of a set of expressions</span></span>

<span data-ttu-id="6aeee-731">在某些情況下，必須為一組運算式推斷一般類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-731">In some cases, a common type needs to be inferred for a set of expressions.</span></span> <span data-ttu-id="6aeee-732">特別的是，以這種方式找到隱含類型陣列的元素類型，以及具有*區塊*主體的匿名函式的傳回類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-732">In particular, the element types of implicitly typed arrays and the return types of anonymous functions with *block* bodies are found in this way.</span></span>

<span data-ttu-id="6aeee-733">直覺上，假設有一組運算式 `E1...Em`，則此推斷應該相當於呼叫方法</span><span class="sxs-lookup"><span data-stu-id="6aeee-733">Intuitively, given a set of expressions `E1...Em` this inference should be equivalent to calling a method</span></span>
```csharp
Tr M<X>(X x1 ... X xm)
```
<span data-ttu-id="6aeee-734">具有 `Ei` 做為引數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-734">with the `Ei` as arguments.</span></span>

<span data-ttu-id="6aeee-735">更精確地說，推斷是以未*固定*的類型變數 `X` 開始。</span><span class="sxs-lookup"><span data-stu-id="6aeee-735">More precisely, the inference starts out with an *unfixed* type variable `X`.</span></span> <span data-ttu-id="6aeee-736">接著會*從*每個 `Ei`*到*`X` 進行*輸出類型推斷*。</span><span class="sxs-lookup"><span data-stu-id="6aeee-736">*Output type inferences* are then made *from* each `Ei` *to* `X`.</span></span> <span data-ttu-id="6aeee-737">最後，`X` 是*固定*的，而且如果成功，產生的型別 `S` 就是運算式所產生的最佳一般型別。</span><span class="sxs-lookup"><span data-stu-id="6aeee-737">Finally, `X` is *fixed* and, if successful, the resulting type `S` is the resulting best common type for the expressions.</span></span> <span data-ttu-id="6aeee-738">如果沒有這類 `S` 存在，則運算式沒有最佳的一般類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-738">If no such `S` exists, the expressions have no best common type.</span></span>

### <a name="overload-resolution"></a><span data-ttu-id="6aeee-739">多載解析</span><span class="sxs-lookup"><span data-stu-id="6aeee-739">Overload resolution</span></span>

<span data-ttu-id="6aeee-740">多載解析是一種系結時間機制，可讓您選取要叫用的最佳函式成員，方法是指定引數清單和一組候選函數成員。</span><span class="sxs-lookup"><span data-stu-id="6aeee-740">Overload resolution is a binding-time mechanism for selecting the best function member to invoke given an argument list and a set of candidate function members.</span></span> <span data-ttu-id="6aeee-741">多載解析會選取函式成員，以在內C#的下列不同內容中叫用：</span><span class="sxs-lookup"><span data-stu-id="6aeee-741">Overload resolution selects the function member to invoke in the following distinct contexts within C#:</span></span>

*  <span data-ttu-id="6aeee-742">在*invocation_expression* （[方法調用](expressions.md#method-invocations)）中名為的方法的調用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-742">Invocation of a method named in an *invocation_expression* ([Method invocations](expressions.md#method-invocations)).</span></span>
*  <span data-ttu-id="6aeee-743">在*object_creation_expression* （[物件建立運算式](expressions.md#object-creation-expressions)）中名為的實例函式的調用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-743">Invocation of an instance constructor named in an *object_creation_expression* ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span>
*  <span data-ttu-id="6aeee-744">透過*element_access* （專案[存取](expressions.md#element-access)）調用索引子存取子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-744">Invocation of an indexer accessor through an *element_access* ([Element access](expressions.md#element-access)).</span></span>
*  <span data-ttu-id="6aeee-745">運算式中參考之預先定義或使用者定義的運算子（[一元運算子](expressions.md#unary-operator-overload-resolution)多載解析和[二元運算子](expressions.md#binary-operator-overload-resolution)多載解析）的調用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-745">Invocation of a predefined or user-defined operator referenced in an expression ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution) and [Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)).</span></span>

<span data-ttu-id="6aeee-746">這些內容中的每一個都會以其獨特的方式定義一組候選函式成員和引數清單，如以上所列的詳細說明。</span><span class="sxs-lookup"><span data-stu-id="6aeee-746">Each of these contexts defines the set of candidate function members and the list of arguments in its own unique way, as described in detail in the sections listed above.</span></span> <span data-ttu-id="6aeee-747">例如，方法調用的候選集合不包含標記為 `override` （[成員查閱](expressions.md#member-lookup)）的方法，而且如果衍生類別中的任何方法適用（[方法調用](expressions.md#method-invocations)），則基類中的方法不是候選項目。</span><span class="sxs-lookup"><span data-stu-id="6aeee-747">For example, the set of candidates for a method invocation does not include methods marked `override` ([Member lookup](expressions.md#member-lookup)), and methods in a base class are not candidates if any method in a derived class is applicable ([Method invocations](expressions.md#method-invocations)).</span></span>

<span data-ttu-id="6aeee-748">一旦識別出候選函式成員和引數清單之後，在所有情況下，最佳函式成員的選取範圍都相同：</span><span class="sxs-lookup"><span data-stu-id="6aeee-748">Once the candidate function members and the argument list have been identified, the selection of the best function member is the same in all cases:</span></span>

*  <span data-ttu-id="6aeee-749">假設有一組適用的候選函式成員，就會找到該集合中的最佳函式成員。</span><span class="sxs-lookup"><span data-stu-id="6aeee-749">Given the set of applicable candidate function members, the best function member in that set is located.</span></span> <span data-ttu-id="6aeee-750">如果集合僅包含一個函式成員，則該函式成員就是最佳的函式成員。</span><span class="sxs-lookup"><span data-stu-id="6aeee-750">If the set contains only one function member, then that function member is the best function member.</span></span> <span data-ttu-id="6aeee-751">否則，最佳函式成員就是一個優於指定引數清單的所有其他函式成員的函式成員，前提是每個函式成員都會使用更好的函式中的規則來與其他所有函式成員進行比較[成員](expressions.md#better-function-member)。</span><span class="sxs-lookup"><span data-stu-id="6aeee-751">Otherwise, the best function member is the one function member that is better than all other function members with respect to the given argument list, provided that each function member is compared to all other function members using the rules in [Better function member](expressions.md#better-function-member).</span></span> <span data-ttu-id="6aeee-752">如果沒有一個比所有其他函式成員更好的函式成員，則函式成員調用是不明確的，而且會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-752">If there is not exactly one function member that is better than all other function members, then the function member invocation is ambiguous and a binding-time error occurs.</span></span>

<span data-ttu-id="6aeee-753">下列章節會定義詞彙適用函式***成員***和***更好***的函式成員的確切意義。</span><span class="sxs-lookup"><span data-stu-id="6aeee-753">The following sections define the exact meanings of the terms ***applicable function member*** and ***better function member***.</span></span>

#### <a name="applicable-function-member"></a><span data-ttu-id="6aeee-754">適用的函式成員</span><span class="sxs-lookup"><span data-stu-id="6aeee-754">Applicable function member</span></span>

<span data-ttu-id="6aeee-755">當下列所有條件都成立時，函式成員就會被視為適用于引數清單 `A` 的***函數成員***：</span><span class="sxs-lookup"><span data-stu-id="6aeee-755">A function member is said to be an ***applicable function member*** with respect to an argument list `A` when all of the following are true:</span></span>

*  <span data-ttu-id="6aeee-756">@No__t-0 中的每個引數都會對應至函式成員宣告中的參數（如[對應的參數](expressions.md#corresponding-parameters)所述），而任何不含引數的參數都是選擇性參數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-756">Each argument in `A` corresponds to a parameter in the function member declaration as described in [Corresponding parameters](expressions.md#corresponding-parameters), and any parameter to which no argument corresponds is an optional parameter.</span></span>
*  <span data-ttu-id="6aeee-757">針對 `A` 中的每個引數，引數的參數傳遞模式（亦即，值、`ref` 或 `out`）等同于對應參數的參數傳遞模式，以及</span><span class="sxs-lookup"><span data-stu-id="6aeee-757">For each argument in `A`, the parameter passing mode of the argument (i.e., value, `ref`, or `out`) is identical to the parameter passing mode of the corresponding parameter, and</span></span>
   *  <span data-ttu-id="6aeee-758">針對值參數或參數陣列，隱含轉換（[隱含轉換](conversions.md#implicit-conversions)）會從引數到對應參數的類型，或</span><span class="sxs-lookup"><span data-stu-id="6aeee-758">for a value parameter or a parameter array, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from the argument to the type of the corresponding parameter, or</span></span>
   *  <span data-ttu-id="6aeee-759">若為 `ref` 或 @no__t 1 參數，引數的類型與對應參數的類型相同。</span><span class="sxs-lookup"><span data-stu-id="6aeee-759">for a `ref` or `out` parameter, the type of the argument is identical to the type of the corresponding parameter.</span></span> <span data-ttu-id="6aeee-760">畢竟，`ref` 或 @no__t 1 參數是已傳遞之引數的別名。</span><span class="sxs-lookup"><span data-stu-id="6aeee-760">After all, a `ref` or `out` parameter is an alias for the argument passed.</span></span>

<span data-ttu-id="6aeee-761">對於包含參數陣列的函式成員，如果函式成員適用于上述規則，則會被視為適用于其***一般形式***。</span><span class="sxs-lookup"><span data-stu-id="6aeee-761">For a function member that includes a parameter array, if the function member is applicable by the above rules, it is said to be applicable in its ***normal form***.</span></span> <span data-ttu-id="6aeee-762">如果包含參數陣列的函式成員不適用其一般格式，則函式成員可能會改為適用于其***擴充形式***：</span><span class="sxs-lookup"><span data-stu-id="6aeee-762">If a function member that includes a parameter array is not applicable in its normal form, the function member may instead be applicable in its ***expanded form***:</span></span>

*  <span data-ttu-id="6aeee-763">藉由將函式成員宣告中的參數陣列取代為參數陣列之元素類型的零個或多個值參數，讓引數清單中的引數數目 `A` 符合總數目，來構成展開的表單。的參數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-763">The expanded form is constructed by replacing the parameter array in the function member declaration with zero or more value parameters of the element type of the parameter array such that the number of arguments in the argument list `A` matches the total number of parameters.</span></span> <span data-ttu-id="6aeee-764">如果 `A` 的引數數目比函式成員宣告中的固定參數數少，則無法構造函式成員的展開形式，因此不適用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-764">If `A` has fewer arguments than the number of fixed parameters in the function member declaration, the expanded form of the function member cannot be constructed and is thus not applicable.</span></span>
*  <span data-ttu-id="6aeee-765">否則，如果 `A` 中的每個引數，引數的參數傳遞模式與對應參數的參數傳遞模式相同，則適用展開的表單。</span><span class="sxs-lookup"><span data-stu-id="6aeee-765">Otherwise, the expanded form is applicable if for each argument in `A` the parameter passing mode of the argument is identical to the parameter passing mode of the corresponding parameter, and</span></span>
   *  <span data-ttu-id="6aeee-766">若為 fixed 值參數或擴充所建立的值參數，隱含轉換（[隱含轉換](conversions.md#implicit-conversions)）會從引數的類型存在對應的參數類型中，或</span><span class="sxs-lookup"><span data-stu-id="6aeee-766">for a fixed value parameter or a value parameter created by the expansion, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from the type of the argument to the type of the corresponding parameter, or</span></span>
   *  <span data-ttu-id="6aeee-767">若為 `ref` 或 @no__t 1 參數，引數的類型與對應參數的類型相同。</span><span class="sxs-lookup"><span data-stu-id="6aeee-767">for a `ref` or `out` parameter, the type of the argument is identical to the type of the corresponding parameter.</span></span>

#### <a name="better-function-member"></a><span data-ttu-id="6aeee-768">更好的函式成員</span><span class="sxs-lookup"><span data-stu-id="6aeee-768">Better function member</span></span>

<span data-ttu-id="6aeee-769">為了決定較好的函式成員，會將移除的引數清單結構化，其中只包含引數運算式本身出現在原始引數清單中的順序。</span><span class="sxs-lookup"><span data-stu-id="6aeee-769">For the purposes of determining the better function member, a stripped-down argument list A is constructed containing just the argument expressions themselves in the order they appear in the original argument list.</span></span>

<span data-ttu-id="6aeee-770">每個候選函式成員的參數清單會以下列方式進行結構化：</span><span class="sxs-lookup"><span data-stu-id="6aeee-770">Parameter lists for each of the candidate function members are constructed in the following way:</span></span>

*  <span data-ttu-id="6aeee-771">如果函式成員僅適用于展開的形式，則會使用展開的表單。</span><span class="sxs-lookup"><span data-stu-id="6aeee-771">The expanded form is used if the function member was applicable only in the expanded form.</span></span>
*  <span data-ttu-id="6aeee-772">不含對應引數的選擇性參數會從參數清單中移除</span><span class="sxs-lookup"><span data-stu-id="6aeee-772">Optional parameters with no corresponding arguments are removed from the parameter list</span></span>
*  <span data-ttu-id="6aeee-773">這些參數會重新排序，使其出現在引數清單中與對應引數相同的位置。</span><span class="sxs-lookup"><span data-stu-id="6aeee-773">The parameters are reordered so that they occur at the same position as the corresponding argument in the argument list.</span></span>

<span data-ttu-id="6aeee-774">假設有一個引數清單 `A`，其中包含一組引數運算式 `{E1, E2, ..., En}`，以及兩個適用的函式成員 `Mp` 和 `Mq` （參數類型 `{P1, P2, ..., Pn}` 和 `{Q1, Q2, ..., Qn}`），則 `Mp` 定義為比 `Mq`***更好***的函式成員（如果</span><span class="sxs-lookup"><span data-stu-id="6aeee-774">Given an argument list `A` with a set of argument expressions `{E1, E2, ..., En}` and two applicable function members `Mp` and `Mq` with parameter types `{P1, P2, ..., Pn}` and `{Q1, Q2, ..., Qn}`, `Mp` is defined to be a ***better function member*** than `Mq` if</span></span>

*  <span data-ttu-id="6aeee-775">對於每個引數而言，從 `Ex` 到 `Qx` 的隱含轉換不會優於從 `Ex` 到 `Px` 的隱含轉換，而是</span><span class="sxs-lookup"><span data-stu-id="6aeee-775">for each argument, the implicit conversion from `Ex` to `Qx` is not better than the implicit conversion from `Ex` to `Px`, and</span></span>
*  <span data-ttu-id="6aeee-776">針對至少一個引數，從 `Ex` 到 `Px` 的轉換比從 `Ex` 轉換為 `Qx` 更好。</span><span class="sxs-lookup"><span data-stu-id="6aeee-776">for at least one argument, the conversion from `Ex` to `Px` is better than the conversion from `Ex` to `Qx`.</span></span>

<span data-ttu-id="6aeee-777">執行此評估時，如果 `Mp` 或 `Mq` 適用于其展開的格式，則 `Px` 或 `Qx` 指的是參數清單的展開形式中的參數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-777">When performing this evaluation, if `Mp` or `Mq` is applicable in its expanded form, then `Px` or `Qx` refers to a parameter in the expanded form of the parameter list.</span></span>

<span data-ttu-id="6aeee-778">如果參數類型 sequence @ no__t-0 和 `{Q1, Q2, ..., Qn}` 相等（也就是每個 `Pi` 都有對應的 `Qi`）的身分識別轉換，則會依序套用下列系結規則，以判斷更好的函式成員。</span><span class="sxs-lookup"><span data-stu-id="6aeee-778">In case the parameter type sequences `{P1, P2, ..., Pn}` and `{Q1, Q2, ..., Qn}` are equivalent (i.e. each `Pi` has an identity conversion to the corresponding `Qi`), the following tie-breaking rules are applied, in order, to determine the better function member.</span></span>

*  <span data-ttu-id="6aeee-779">如果 `Mp` 是非泛型方法，而 `Mq` 是泛型方法，則 `Mp` 比 `Mq` 更好。</span><span class="sxs-lookup"><span data-stu-id="6aeee-779">If `Mp` is a non-generic method and `Mq` is a generic method, then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="6aeee-780">否則，如果 `Mp` 適用于其正常格式，且 `Mq` 具有 @no__t 2 陣列，而且只適用于其展開的表單，則 `Mp` 會優於 `Mq`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-780">Otherwise, if `Mp` is applicable in its normal form and `Mq` has a `params` array and is applicable only in its expanded form, then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="6aeee-781">否則，如果 `Mp` 的宣告參數超過 `Mq`，則 `Mp` 比 `Mq` 更好。</span><span class="sxs-lookup"><span data-stu-id="6aeee-781">Otherwise, if `Mp` has more declared parameters than `Mq`, then `Mp` is better than `Mq`.</span></span> <span data-ttu-id="6aeee-782">如果這兩個方法都有 @no__t 0 陣列，而且只適用于其展開的形式，就會發生這種情況。</span><span class="sxs-lookup"><span data-stu-id="6aeee-782">This can occur if both methods have `params` arrays and are applicable only in their expanded forms.</span></span>
*  <span data-ttu-id="6aeee-783">否則，如果 `Mp` 的所有參數都有對應的引數，而預設引數必須在 `Mq` 中取代為至少一個選擇性參數，則 `Mp` 比 `Mq` 更好。</span><span class="sxs-lookup"><span data-stu-id="6aeee-783">Otherwise if all parameters of `Mp` have a corresponding argument whereas default arguments need to be substituted for at least one optional parameter in `Mq` then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="6aeee-784">否則，如果 `Mp` 的參數類型比 `Mq` 更特定，則 `Mp` 比 `Mq` 更好。</span><span class="sxs-lookup"><span data-stu-id="6aeee-784">Otherwise, if `Mp` has more specific parameter types than `Mq`, then `Mp` is better than `Mq`.</span></span> <span data-ttu-id="6aeee-785">Let `{R1, R2, ..., Rn}` 和 `{S1, S2, ..., Sn}` 代表未具現化和未展開的參數類型（`Mp` 和 `Mq`）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-785">Let `{R1, R2, ..., Rn}` and `{S1, S2, ..., Sn}` represent the uninstantiated and unexpanded parameter types of `Mp` and `Mq`.</span></span> <span data-ttu-id="6aeee-786">`Mp` 的參數類型比 `Mq` 更明確，如果每個參數的 `Rx` 不是小於 `Sx`，而且至少有一個參數，則 `Rx` 比 `Sx` 更具體：</span><span class="sxs-lookup"><span data-stu-id="6aeee-786">`Mp`'s parameter types are more specific than `Mq`'s if, for each parameter, `Rx` is not less specific than `Sx`, and, for at least one parameter, `Rx` is more specific than `Sx`:</span></span>
   *  <span data-ttu-id="6aeee-787">型別參數比非型別參數較不明確。</span><span class="sxs-lookup"><span data-stu-id="6aeee-787">A type parameter is less specific than a non-type parameter.</span></span>
   *  <span data-ttu-id="6aeee-788">如果至少有一個型別引數較明確，而且沒有任何型別引數比另一個中的對應型別引數更明確，則結構化型別會以遞迴方式比另一個結構化型別更明確（具有相同的型別引數數目）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-788">Recursively, a constructed type is more specific than another constructed type (with the same number of type arguments) if at least one type argument is more specific and no type argument is less specific than the corresponding type argument in the other.</span></span>
   *  <span data-ttu-id="6aeee-789">如果第一個陣列類型的專案類型比第二個數組類型的專案類型更明確，則陣列型別會比另一個陣列型別更明確（具有相同的維度數目）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-789">An array type is more specific than another array type (with the same number of dimensions) if the element type of the first is more specific than the element type of the second.</span></span>
*  <span data-ttu-id="6aeee-790">否則，如果一個成員是一個非提升運算子，另一個則是提升運算子，則不會提升。</span><span class="sxs-lookup"><span data-stu-id="6aeee-790">Otherwise if one member is a non-lifted operator and  the other is a lifted operator, the non-lifted one is better.</span></span>
*  <span data-ttu-id="6aeee-791">否則，函數成員都不會更好。</span><span class="sxs-lookup"><span data-stu-id="6aeee-791">Otherwise, neither function member is better.</span></span>

#### <a name="better-conversion-from-expression"></a><span data-ttu-id="6aeee-792">從運算式進行更好的轉換</span><span class="sxs-lookup"><span data-stu-id="6aeee-792">Better conversion from expression</span></span>

<span data-ttu-id="6aeee-793">假設隱含轉換 `C1`，會從 `E` 的運算式轉換成 `T1` 的類型，以及從運算式 `E` 轉換為類型 `T2` 的隱含轉換 `C2`，`C1` 是比 `C2`***更佳的轉換***（如果 `E` 不完全符合 0，而且至少有下列其中一個保存：</span><span class="sxs-lookup"><span data-stu-id="6aeee-793">Given an implicit conversion `C1` that converts from an expression `E` to a type `T1`, and an implicit conversion `C2` that converts from an expression `E` to a type `T2`, `C1` is a ***better conversion*** than `C2` if `E` does not exactly match `T2` and at least one of the following holds:</span></span>

* <span data-ttu-id="6aeee-794">`E` 完全符合 `T1` （[完全相符的運算式](expressions.md#exactly-matching-expression)）</span><span class="sxs-lookup"><span data-stu-id="6aeee-794">`E` exactly matches `T1` ([Exactly matching Expression](expressions.md#exactly-matching-expression))</span></span>
* <span data-ttu-id="6aeee-795">`T1` 是比 `T2` 更佳的轉換目標（[較佳的轉換目標](expressions.md#better-conversion-target)）</span><span class="sxs-lookup"><span data-stu-id="6aeee-795">`T1` is a better conversion target than `T2` ([Better conversion target](expressions.md#better-conversion-target))</span></span>

#### <a name="exactly-matching-expression"></a><span data-ttu-id="6aeee-796">完全相符的運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-796">Exactly matching Expression</span></span>

<span data-ttu-id="6aeee-797">假設運算式 `E`，而類型 `T`，而如果下列其中一項保留，`E` 則完全符合 `T`：</span><span class="sxs-lookup"><span data-stu-id="6aeee-797">Given an expression `E` and a type `T`, `E` exactly matches `T` if one of the following holds:</span></span>

*  <span data-ttu-id="6aeee-798">`E` 的類型 `S`，而從 `S` 到 @no__t 的識別轉換已存在</span><span class="sxs-lookup"><span data-stu-id="6aeee-798">`E` has a type `S`, and an identity conversion exists from `S` to `T`</span></span>
*  <span data-ttu-id="6aeee-799">`E` 是匿名函式，`T` 是委派類型 `D` 或運算式樹狀架構類型 `Expression<D>` 和下列其中一個保留：</span><span class="sxs-lookup"><span data-stu-id="6aeee-799">`E` is an anonymous function, `T` is either a delegate type `D` or an expression tree type `Expression<D>` and one of the following holds:</span></span>
   *  <span data-ttu-id="6aeee-800">在 `D` （推斷的傳回[型](expressions.md#inferred-return-type)別）的參數清單內容中，`E` 已有推斷的傳回型別 @no__t，而且從 `X` 到 `D` 的傳回型別都有識別轉換。</span><span class="sxs-lookup"><span data-stu-id="6aeee-800">An inferred return type `X` exists for `E` in the context of the parameter list of `D` ([Inferred return type](expressions.md#inferred-return-type)), and an identity conversion exists from `X` to the return type of `D`</span></span>
   *  <span data-ttu-id="6aeee-801">@No__t-0 是非非同步，而且 `D` 的傳回型別 `Y` 或 `E` 是非同步，而且 `D` 的傳回型別 `Task<Y>`，而下列其中一個保留：</span><span class="sxs-lookup"><span data-stu-id="6aeee-801">Either `E` is non-async and `D` has a return type `Y` or `E` is async and `D` has a return type `Task<Y>`, and one of the following holds:</span></span>
      * <span data-ttu-id="6aeee-802">@No__t-0 的主體是完全符合 `Y` 的運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-802">The body of `E` is an expression that exactly matches `Y`</span></span>
      * <span data-ttu-id="6aeee-803">@No__t-0 的主體是語句區塊，其中每個傳回語句都會傳回完全符合 `Y` 的運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-803">The body of `E` is a statement block where every return statement returns an expression that exactly matches `Y`</span></span>

#### <a name="better-conversion-target"></a><span data-ttu-id="6aeee-804">較佳的轉換目標</span><span class="sxs-lookup"><span data-stu-id="6aeee-804">Better conversion target</span></span>

<span data-ttu-id="6aeee-805">假設有兩種不同的類型 `T1` 和 `T2`，如果沒有從 `T2` 到 `T1` 的隱含轉換，則 `T1` 是更佳的轉換 @no__t 目標，而且至少有下列其中一個保留：</span><span class="sxs-lookup"><span data-stu-id="6aeee-805">Given two different types `T1` and `T2`, `T1` is a better conversion target than `T2` if no implicit conversion from `T2` to `T1` exists, and at least one of the following holds:</span></span>

*  <span data-ttu-id="6aeee-806">從 `T1` 到 `T2` 的隱含轉換已存在</span><span class="sxs-lookup"><span data-stu-id="6aeee-806">An implicit conversion from `T1` to `T2` exists</span></span>
*  <span data-ttu-id="6aeee-807">`T1` 是委派類型 `D1` 或運算式樹狀結構類型 `Expression<D1>`，`T2` 是委派類型 `D2` 或運算式樹狀結構類型 `Expression<D2>`，`D1` 的傳回類型 `S1` 和下列其中一個保存:</span><span class="sxs-lookup"><span data-stu-id="6aeee-807">`T1` is either a delegate type `D1` or an expression tree type `Expression<D1>`, `T2` is either a delegate type `D2` or an expression tree type `Expression<D2>`, `D1` has a return type `S1` and one of the following holds:</span></span>
   * <span data-ttu-id="6aeee-808">`D2` 傳回 void</span><span class="sxs-lookup"><span data-stu-id="6aeee-808">`D2` is void returning</span></span>
   * <span data-ttu-id="6aeee-809">`D2` 的傳回型別 `S2`，而 `S1` 是比 `S2` 更好的轉換目標</span><span class="sxs-lookup"><span data-stu-id="6aeee-809">`D2` has a return type `S2`, and `S1` is a better conversion target than `S2`</span></span>
*  <span data-ttu-id="6aeee-810">`T1` 是 `Task<S1>`，`T2` `Task<S2>`，而 `S1` 是比 `S2` 更佳的轉換目標</span><span class="sxs-lookup"><span data-stu-id="6aeee-810">`T1` is `Task<S1>`, `T2` is `Task<S2>`, and `S1` is a better conversion target than `S2`</span></span>
*  <span data-ttu-id="6aeee-811">`T1` 是 `S1` 或 `S1?`，其中 `S1` 是帶正負號的整數類資料類型，而 `T2` 是 `S2` 或 `S2?`，其中 `S2` 是不帶正負號的整數類資料類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-811">`T1` is `S1` or `S1?` where `S1` is a signed integral type, and `T2` is `S2` or `S2?` where `S2` is an unsigned integral type.</span></span> <span data-ttu-id="6aeee-812">尤其是：</span><span class="sxs-lookup"><span data-stu-id="6aeee-812">Specifically:</span></span>
   * <span data-ttu-id="6aeee-813">`S1` 是 `sbyte`，而 `S2` `byte`、`ushort`、`uint` 或 `ulong`</span><span class="sxs-lookup"><span data-stu-id="6aeee-813">`S1` is `sbyte` and `S2` is `byte`, `ushort`, `uint`, or `ulong`</span></span>
   * <span data-ttu-id="6aeee-814">`S1` 是 `short`，而 `S2` `ushort`、`uint` 或 `ulong`</span><span class="sxs-lookup"><span data-stu-id="6aeee-814">`S1` is `short` and `S2` is `ushort`, `uint`, or `ulong`</span></span>
   * <span data-ttu-id="6aeee-815">`S1` 是 `int`，而 `S2` `uint`，或 `ulong`</span><span class="sxs-lookup"><span data-stu-id="6aeee-815">`S1` is `int` and `S2` is `uint`, or `ulong`</span></span>
   * <span data-ttu-id="6aeee-816">`S1` 是 `long`，而 `S2` `ulong`</span><span class="sxs-lookup"><span data-stu-id="6aeee-816">`S1` is `long` and `S2` is `ulong`</span></span>

#### <a name="overloading-in-generic-classes"></a><span data-ttu-id="6aeee-817">泛型類別中的多載</span><span class="sxs-lookup"><span data-stu-id="6aeee-817">Overloading in generic classes</span></span>

<span data-ttu-id="6aeee-818">雖然宣告的簽章必須是唯一的，但類型引數的替代可能會產生相同的簽章。</span><span class="sxs-lookup"><span data-stu-id="6aeee-818">While signatures as declared must be unique, it is possible that substitution of type arguments results in identical signatures.</span></span> <span data-ttu-id="6aeee-819">在這種情況下，上述多載解析的中斷規則會挑選最特定的成員。</span><span class="sxs-lookup"><span data-stu-id="6aeee-819">In such cases, the tie-breaking rules of overload resolution above will pick the most specific member.</span></span>

<span data-ttu-id="6aeee-820">下列範例會根據此規則顯示有效和不正確多載：</span><span class="sxs-lookup"><span data-stu-id="6aeee-820">The following examples show overloads that are valid and invalid according to this rule:</span></span>

```csharp
interface I1<T> {...}

interface I2<T> {...}

class G1<U>
{
    int F1(U u);                  // Overload resolution for G<int>.F1
    int F1(int i);                // will pick non-generic

    void F2(I1<U> a);             // Valid overload
    void F2(I2<U> a);
}

class G2<U,V>
{
    void F3(U u, V v);            // Valid, but overload resolution for
    void F3(V v, U u);            // G2<int,int>.F3 will fail

    void F4(U u, I1<V> v);        // Valid, but overload resolution for    
    void F4(I1<V> v, U u);        // G2<I1<int>,int>.F4 will fail

    void F5(U u1, I1<V> v2);      // Valid overload
    void F5(V v1, U u2);

    void F6(ref U u);             // valid overload
    void F6(out V v);
}
```

### <a name="compile-time-checking-of-dynamic-overload-resolution"></a><span data-ttu-id="6aeee-821">動態多載解析的編譯階段檢查</span><span class="sxs-lookup"><span data-stu-id="6aeee-821">Compile-time checking of dynamic overload resolution</span></span>

<span data-ttu-id="6aeee-822">對於大部分動態系結的作業而言，解析的可能候選項目集在編譯時期是未知的。</span><span class="sxs-lookup"><span data-stu-id="6aeee-822">For most dynamically bound operations the set of possible candidates for resolution is unknown at compile-time.</span></span> <span data-ttu-id="6aeee-823">不過在某些情況下，候選集合在編譯時期是已知的：</span><span class="sxs-lookup"><span data-stu-id="6aeee-823">In certain cases, however the candidate set is known at compile-time:</span></span>

*  <span data-ttu-id="6aeee-824">使用動態引數的靜態方法呼叫</span><span class="sxs-lookup"><span data-stu-id="6aeee-824">Static method calls with dynamic arguments</span></span>
*  <span data-ttu-id="6aeee-825">接收者不是動態運算式的實例方法呼叫</span><span class="sxs-lookup"><span data-stu-id="6aeee-825">Instance method calls where the receiver is not a dynamic expression</span></span>
*  <span data-ttu-id="6aeee-826">接收者不是動態運算式的索引子呼叫</span><span class="sxs-lookup"><span data-stu-id="6aeee-826">Indexer calls where the receiver is not a dynamic expression</span></span>
*  <span data-ttu-id="6aeee-827">使用動態引數的函式呼叫</span><span class="sxs-lookup"><span data-stu-id="6aeee-827">Constructor calls with dynamic arguments</span></span>

<span data-ttu-id="6aeee-828">在這些情況下，會針對每個候選項目執行有限的編譯時間檢查，以查看是否有任何可能會在執行時間套用。這項檢查包含下列步驟：</span><span class="sxs-lookup"><span data-stu-id="6aeee-828">In these cases a limited compile-time check is performed for each candidate to see if any of them could possibly apply at run-time.This check consists of the following steps:</span></span>

*  <span data-ttu-id="6aeee-829">部分型別推斷：任何不是直接或間接相依于類型 `dynamic` 之引數的型別引數，都是使用[型別推斷](expressions.md#type-inference)的規則來推斷。</span><span class="sxs-lookup"><span data-stu-id="6aeee-829">Partial type inference: Any type argument that does not depend directly or indirectly on an argument of type `dynamic` is inferred using the rules of [Type inference](expressions.md#type-inference).</span></span> <span data-ttu-id="6aeee-830">其餘的類型引數不明。</span><span class="sxs-lookup"><span data-stu-id="6aeee-830">The remaining type arguments are unknown.</span></span>
*  <span data-ttu-id="6aeee-831">部分適用性檢查：系統會根據適用的函式[成員](expressions.md#applicable-function-member)檢查適用性，但忽略其類型未知的參數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-831">Partial applicability check: Applicability is checked according to [Applicable function member](expressions.md#applicable-function-member), but ignoring parameters whose types are unknown.</span></span>
*  <span data-ttu-id="6aeee-832">如果沒有候選項通過這項測試，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-832">If no candidate passes this test, a compile-time error occurs.</span></span>

### <a name="function-member-invocation"></a><span data-ttu-id="6aeee-833">函數成員調用</span><span class="sxs-lookup"><span data-stu-id="6aeee-833">Function member invocation</span></span>

<span data-ttu-id="6aeee-834">本節說明在執行時間執行特定函式成員時所發生的程式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-834">This section describes the process that takes place at run-time to invoke a particular function member.</span></span> <span data-ttu-id="6aeee-835">假設系結時間進程已經判斷要叫用的特定成員，可能的方法是將多載解析套用至一組候選函式成員。</span><span class="sxs-lookup"><span data-stu-id="6aeee-835">It is assumed that a binding-time process has already determined the particular member to invoke, possibly by applying overload resolution to a set of candidate function members.</span></span>

<span data-ttu-id="6aeee-836">為了描述叫用程式，函式成員分成兩個類別：</span><span class="sxs-lookup"><span data-stu-id="6aeee-836">For purposes of describing the invocation process, function members are divided into two categories:</span></span>

*  <span data-ttu-id="6aeee-837">靜態函式成員。</span><span class="sxs-lookup"><span data-stu-id="6aeee-837">Static function members.</span></span> <span data-ttu-id="6aeee-838">這些是實例的構造函式、靜態方法、靜態屬性存取子和使用者定義的運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-838">These are instance constructors, static methods, static property accessors, and user-defined operators.</span></span> <span data-ttu-id="6aeee-839">靜態函式成員一律為非虛擬。</span><span class="sxs-lookup"><span data-stu-id="6aeee-839">Static function members are always non-virtual.</span></span>
*  <span data-ttu-id="6aeee-840">實例函式成員。</span><span class="sxs-lookup"><span data-stu-id="6aeee-840">Instance function members.</span></span> <span data-ttu-id="6aeee-841">這些是實例方法、實例屬性存取子和索引子存取子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-841">These are instance methods, instance property accessors, and indexer accessors.</span></span> <span data-ttu-id="6aeee-842">實例函式成員為非虛擬或虛擬，而且一律會在特定實例上叫用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-842">Instance function members are either non-virtual or virtual, and are always invoked on a particular instance.</span></span> <span data-ttu-id="6aeee-843">實例是由實例運算式所計算，並可在函式成員中以 `this` （[此存取權](expressions.md#this-access)）的形式存取。</span><span class="sxs-lookup"><span data-stu-id="6aeee-843">The instance is computed by an instance expression, and it becomes accessible within the function member as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="6aeee-844">函式成員調用的執行時間處理包含下列步驟，其中 `M` 是函數成員，而如果 `M` 是實例成員，則 `E` 是實例運算式：</span><span class="sxs-lookup"><span data-stu-id="6aeee-844">The run-time processing of a function member invocation consists of the following steps, where `M` is the function member and, if `M` is an instance member, `E` is the instance expression:</span></span>

*  <span data-ttu-id="6aeee-845">如果 `M` 是靜態函式成員：</span><span class="sxs-lookup"><span data-stu-id="6aeee-845">If `M` is a static function member:</span></span>
   * <span data-ttu-id="6aeee-846">引數清單會依照[引數](expressions.md#argument-lists)清單中的說明進行評估。</span><span class="sxs-lookup"><span data-stu-id="6aeee-846">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="6aeee-847">已叫用 `M`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-847">`M` is invoked.</span></span>

*  <span data-ttu-id="6aeee-848">如果 `M` 是在*value_type*中宣告的實例函式成員：</span><span class="sxs-lookup"><span data-stu-id="6aeee-848">If `M` is an instance function member declared in a *value_type*:</span></span>
   * <span data-ttu-id="6aeee-849">`E` 會進行評估。</span><span class="sxs-lookup"><span data-stu-id="6aeee-849">`E` is evaluated.</span></span> <span data-ttu-id="6aeee-850">如果此評估導致例外狀況，則不會執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="6aeee-850">If this evaluation causes an exception, then no further steps are executed.</span></span>
   * <span data-ttu-id="6aeee-851">如果 `E` 未分類為變數，則會建立 `E` 之類型的暫存區域變數，並將 `E` 的值指派給該變數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-851">If `E` is not classified as a variable, then a temporary local variable of `E`'s type is created and the value of `E` is assigned to that variable.</span></span> <span data-ttu-id="6aeee-852">`E` 接著會重新分類為該暫存區域變數的參考。</span><span class="sxs-lookup"><span data-stu-id="6aeee-852">`E` is then reclassified as a reference to that temporary local variable.</span></span> <span data-ttu-id="6aeee-853">暫存變數可在 `M` 內以 `this` 的形式存取，但不能以任何其他方式存取。</span><span class="sxs-lookup"><span data-stu-id="6aeee-853">The temporary variable is accessible as `this` within `M`, but not in any other way.</span></span> <span data-ttu-id="6aeee-854">因此，只有當 `E` 是真正的變數時，呼叫者可以觀察 `M` 對 `this` 所做的變更。</span><span class="sxs-lookup"><span data-stu-id="6aeee-854">Thus, only when `E` is a true variable is it possible for the caller to observe the changes that `M` makes to `this`.</span></span>
   * <span data-ttu-id="6aeee-855">引數清單會依照[引數](expressions.md#argument-lists)清單中的說明進行評估。</span><span class="sxs-lookup"><span data-stu-id="6aeee-855">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="6aeee-856">已叫用 `M`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-856">`M` is invoked.</span></span> <span data-ttu-id="6aeee-857">@No__t-0 所參考的變數會成為 `this` 所參考的變數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-857">The variable referenced by `E` becomes the variable referenced by `this`.</span></span>

*  <span data-ttu-id="6aeee-858">如果 `M` 是在*reference_type*中宣告的實例函式成員：</span><span class="sxs-lookup"><span data-stu-id="6aeee-858">If `M` is an instance function member declared in a *reference_type*:</span></span>
   * <span data-ttu-id="6aeee-859">`E` 會進行評估。</span><span class="sxs-lookup"><span data-stu-id="6aeee-859">`E` is evaluated.</span></span> <span data-ttu-id="6aeee-860">如果此評估導致例外狀況，則不會執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="6aeee-860">If this evaluation causes an exception, then no further steps are executed.</span></span>
   * <span data-ttu-id="6aeee-861">引數清單會依照[引數](expressions.md#argument-lists)清單中的說明進行評估。</span><span class="sxs-lookup"><span data-stu-id="6aeee-861">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="6aeee-862">如果 `E` 的類型是*value_type*，則會執行「裝箱轉換」（[裝箱](types.md#boxing-conversions)轉換）來將 `E` 轉換成類型 `object`，而在下列步驟中，`E` 會視為 `object` 類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-862">If the type of `E` is a *value_type*, a boxing conversion ([Boxing conversions](types.md#boxing-conversions)) is performed to convert `E` to type `object`, and `E` is considered to be of type `object` in the following steps.</span></span> <span data-ttu-id="6aeee-863">在此情況下，`M` 只能是 `System.Object` 的成員。</span><span class="sxs-lookup"><span data-stu-id="6aeee-863">In this case, `M` could only be a member of `System.Object`.</span></span>
   * <span data-ttu-id="6aeee-864">已檢查 `E` 的值是否有效。</span><span class="sxs-lookup"><span data-stu-id="6aeee-864">The value of `E` is checked to be valid.</span></span> <span data-ttu-id="6aeee-865">如果 `E` 的值為 `null`，則會擲回 `System.NullReferenceException`，且不會執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="6aeee-865">If the value of `E` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
   * <span data-ttu-id="6aeee-866">會決定要叫用的函式成員實作為：</span><span class="sxs-lookup"><span data-stu-id="6aeee-866">The function member implementation to invoke is determined:</span></span>
     * <span data-ttu-id="6aeee-867">如果 `E` 的系結時間類型是介面，則叫用的函式成員就是由 `E` 所參考之實例的執行時間類型所提供的 `M` 的執行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-867">If the binding-time type of `E` is an interface, the function member to invoke is the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span> <span data-ttu-id="6aeee-868">這個函式成員的決定方式是套用介面對應規則（[介面對應](interfaces.md#interface-mapping)），以判斷由 `E` 所參考之實例的執行時間類型所提供的 `M` 的執行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-868">This function member is determined by applying the interface mapping rules ([Interface mapping](interfaces.md#interface-mapping)) to determine the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span>
     * <span data-ttu-id="6aeee-869">否則，如果 `M` 是虛擬函式成員，則叫用的函式成員就是由 `E` 所參考之實例的執行時間類型所提供的 `M` 的執行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-869">Otherwise, if `M` is a virtual function member, the function member to invoke is the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span> <span data-ttu-id="6aeee-870">此函式成員的決定方式，是套用用來判斷 `M` 之最衍生的實（[虛擬方法](classes.md#virtual-methods)）的規則（相對於 `E` 所參考之實例的執行時間型別）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-870">This function member is determined by applying the rules for determining the most derived implementation ([Virtual methods](classes.md#virtual-methods)) of `M` with respect to the run-time type of the instance referenced by `E`.</span></span>
     * <span data-ttu-id="6aeee-871">否則，`M` 是非虛擬函式成員，而要叫用的函式成員是 `M` 本身。</span><span class="sxs-lookup"><span data-stu-id="6aeee-871">Otherwise, `M` is a non-virtual function member, and the function member to invoke is `M` itself.</span></span>
   * <span data-ttu-id="6aeee-872">叫用上述步驟中判斷的函式成員實作為。</span><span class="sxs-lookup"><span data-stu-id="6aeee-872">The function member implementation determined in the step above is invoked.</span></span> <span data-ttu-id="6aeee-873">@No__t-0 所參考的物件會變成 `this` 所參考的物件。</span><span class="sxs-lookup"><span data-stu-id="6aeee-873">The object referenced by `E` becomes the object referenced by `this`.</span></span>

#### <a name="invocations-on-boxed-instances"></a><span data-ttu-id="6aeee-874">在已裝箱實例上的調用</span><span class="sxs-lookup"><span data-stu-id="6aeee-874">Invocations on boxed instances</span></span>

<span data-ttu-id="6aeee-875">在下列情況中，您可以透過*value_type*的盒裝實例來叫用在*value_type*中實作為的函式成員：</span><span class="sxs-lookup"><span data-stu-id="6aeee-875">A function member implemented in a *value_type* can be invoked through a boxed instance of that *value_type* in the following situations:</span></span>

*  <span data-ttu-id="6aeee-876">當函式成員為繼承自類型 `object` 之方法的 `override`，而且是透過類型 `object` 的實例運算式來叫用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-876">When the function member is an `override` of a method inherited from type `object` and is invoked through an instance expression of type `object`.</span></span>
*  <span data-ttu-id="6aeee-877">當函式成員是介面函式成員的實作為，而且是透過*interface_type*的實例運算式來叫用時。</span><span class="sxs-lookup"><span data-stu-id="6aeee-877">When the function member is an implementation of an interface function member and is invoked through an instance expression of an *interface_type*.</span></span>
*  <span data-ttu-id="6aeee-878">當函式成員透過委派叫用時。</span><span class="sxs-lookup"><span data-stu-id="6aeee-878">When the function member is invoked through a delegate.</span></span>

<span data-ttu-id="6aeee-879">在這些情況下，會將盒裝實例視為包含*value_type*的變數，而這個變數會變成函式成員調用中 `this` 所參考的變數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-879">In these situations, the boxed instance is considered to contain a variable of the *value_type*, and this variable becomes the variable referenced by `this` within the function member invocation.</span></span> <span data-ttu-id="6aeee-880">特別是，這表示當函式成員在已裝箱的實例上叫用時，函式成員可能會修改包含在已加入盒裝實例中的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-880">In particular, this means that when a function member is invoked on a boxed instance, it is possible for the function member to modify the value contained in the boxed instance.</span></span>

## <a name="primary-expressions"></a><span data-ttu-id="6aeee-881">主要運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-881">Primary expressions</span></span>

<span data-ttu-id="6aeee-882">主要運算式包含最簡單的運算式形式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-882">Primary expressions include the simplest forms of expressions.</span></span>

```antlr
primary_expression
    : primary_no_array_creation_expression
    | array_creation_expression
    ;

primary_no_array_creation_expression
    : literal
    | interpolated_string_expression
    | simple_name
    | parenthesized_expression
    | member_access
    | invocation_expression
    | element_access
    | this_access
    | base_access
    | post_increment_expression
    | post_decrement_expression
    | object_creation_expression
    | delegate_creation_expression
    | anonymous_object_creation_expression
    | typeof_expression
    | checked_expression
    | unchecked_expression
    | default_value_expression
    | nameof_expression
    | anonymous_method_expression
    | primary_no_array_creation_expression_unsafe
    ;
```

<span data-ttu-id="6aeee-883">主要運算式會分割成*array_creation_expression*s 和*primary_no_array_creation_expression*s。</span><span class="sxs-lookup"><span data-stu-id="6aeee-883">Primary expressions are divided between *array_creation_expression*s and *primary_no_array_creation_expression*s.</span></span> <span data-ttu-id="6aeee-884">以這種方式來處理陣列建立運算式，而不是將它與其他簡單的運算式形式一起列出，而是讓文法不允許可能令人混淆的程式碼，例如</span><span class="sxs-lookup"><span data-stu-id="6aeee-884">Treating array-creation-expression in this way, rather than listing it along with the other simple expression forms, enables the grammar to disallow potentially confusing code such as</span></span>
```csharp
object o = new int[3][1];
```
<span data-ttu-id="6aeee-885">否則會解讀為</span><span class="sxs-lookup"><span data-stu-id="6aeee-885">which would otherwise be interpreted as</span></span>
```csharp
object o = (new int[3])[1];
```

### <a name="literals"></a><span data-ttu-id="6aeee-886">常值</span><span class="sxs-lookup"><span data-stu-id="6aeee-886">Literals</span></span>

<span data-ttu-id="6aeee-887">由*常*值（[常](lexical-structure.md#literals)值）組成的*primary_expression*會分類為值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-887">A *primary_expression* that consists of a *literal* ([Literals](lexical-structure.md#literals)) is classified as a value.</span></span>


### <a name="interpolated-strings"></a><span data-ttu-id="6aeee-888">插入字串</span><span class="sxs-lookup"><span data-stu-id="6aeee-888">Interpolated strings</span></span>

<span data-ttu-id="6aeee-889">*Interpolated_string_expression*是由 @no__t 1 號後面接著一般或逐字字串常值所組成，其中的洞會以 `{` 和 `}` 分隔，並括住運算式和格式設定規格。</span><span class="sxs-lookup"><span data-stu-id="6aeee-889">An *interpolated_string_expression* consists of a `$` sign followed by a regular or verbatim string literal, wherein holes, delimited by `{` and `}`, enclose expressions and formatting specifications.</span></span> <span data-ttu-id="6aeee-890">字串插值運算式是指已細分為個別 token 的*interpolated_string_literal*結果，如插入[字串常](lexical-structure.md#interpolated-string-literals)值中所述。</span><span class="sxs-lookup"><span data-stu-id="6aeee-890">An interpolated string expression is the result of an *interpolated_string_literal* that has been broken up into individual tokens, as described in [Interpolated string literals](lexical-structure.md#interpolated-string-literals).</span></span>

```antlr
interpolated_string_expression
    : '$' interpolated_regular_string
    | '$' interpolated_verbatim_string
    ;

interpolated_regular_string
    : interpolated_regular_string_whole
    | interpolated_regular_string_start interpolated_regular_string_body interpolated_regular_string_end
    ;

interpolated_regular_string_body
    : interpolation (interpolated_regular_string_mid interpolation)*
    ;

interpolation
    : expression
    | expression ',' constant_expression
    ;

interpolated_verbatim_string
    : interpolated_verbatim_string_whole
    | interpolated_verbatim_string_start interpolated_verbatim_string_body interpolated_verbatim_string_end
    ;

interpolated_verbatim_string_body
    : interpolation (interpolated_verbatim_string_mid interpolation)+
    ;
```

<span data-ttu-id="6aeee-891">插補中的*constant_expression*必須隱含轉換成 `int`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-891">The *constant_expression* in an interpolation must have an implicit conversion to `int`.</span></span>

<span data-ttu-id="6aeee-892">*Interpolated_string_expression*會分類為值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-892">An *interpolated_string_expression* is classified as a value.</span></span> <span data-ttu-id="6aeee-893">如果它會立即轉換為 `System.IFormattable`，或 `System.FormattableString`，並使用隱含的插入字串轉換（隱含的插入[字串轉換](conversions.md#implicit-interpolated-string-conversions)），則字串插值運算式會具有該類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-893">If it is immediately converted to `System.IFormattable` or `System.FormattableString` with an implicit interpolated string conversion ([Implicit interpolated string conversions](conversions.md#implicit-interpolated-string-conversions)), the interpolated string expression has that type.</span></span> <span data-ttu-id="6aeee-894">否則，它的類型為 `string`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-894">Otherwise, it has the type `string`.</span></span>

<span data-ttu-id="6aeee-895">如果字串插值的類型為 `System.IFormattable` 或 `System.FormattableString`，則其意義就是對 `System.Runtime.CompilerServices.FormattableStringFactory.Create` 的呼叫。</span><span class="sxs-lookup"><span data-stu-id="6aeee-895">If the type of an interpolated string is `System.IFormattable` or `System.FormattableString`, the meaning is a call to `System.Runtime.CompilerServices.FormattableStringFactory.Create`.</span></span> <span data-ttu-id="6aeee-896">如果類型為 `string`，則運算式的意義就是 `string.Format` 的呼叫。</span><span class="sxs-lookup"><span data-stu-id="6aeee-896">If the type is `string`, the meaning of the expression is a call to `string.Format`.</span></span> <span data-ttu-id="6aeee-897">在這兩種情況下，呼叫的引數清單都包含一個格式字串常值，其中包含每個插補的預留位置，以及對應至預留位置之每個運算式的引數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-897">In both cases, the argument list of the call consists of a format string literal with placeholders for each interpolation, and an argument for each expression corresponding to the place holders.</span></span>

<span data-ttu-id="6aeee-898">格式字串常值的結構如下所示，其中 `N` 是*interpolated_string_expression*中的插補數目：</span><span class="sxs-lookup"><span data-stu-id="6aeee-898">The format string literal is constructed as follows, where `N` is the number of interpolations in the *interpolated_string_expression*:</span></span>

*  <span data-ttu-id="6aeee-899">如果*interpolated_regular_string_whole*或*interpolated_verbatim_string_whole*遵循 `$` 號，則格式字串常值就是該 token。</span><span class="sxs-lookup"><span data-stu-id="6aeee-899">If an *interpolated_regular_string_whole* or an *interpolated_verbatim_string_whole* follows the `$` sign, then the format string literal is that token.</span></span>
*  <span data-ttu-id="6aeee-900">否則，格式字串常值包含：</span><span class="sxs-lookup"><span data-stu-id="6aeee-900">Otherwise, the format string literal consists of:</span></span> 
   *  <span data-ttu-id="6aeee-901">首先是*interpolated_regular_string_start*或*interpolated_verbatim_string_start*</span><span class="sxs-lookup"><span data-stu-id="6aeee-901">First the *interpolated_regular_string_start* or *interpolated_verbatim_string_start*</span></span>
   *  <span data-ttu-id="6aeee-902">然後針對每個數位 `I` 從 `0` 到 `N-1`：</span><span class="sxs-lookup"><span data-stu-id="6aeee-902">Then for each number `I` from `0` to `N-1`:</span></span> 
      * <span data-ttu-id="6aeee-903">@No__t-0 的十進位標記法</span><span class="sxs-lookup"><span data-stu-id="6aeee-903">The decimal representation of `I`</span></span>
      * <span data-ttu-id="6aeee-904">然後，如果對應的*插補*具有*constant_expression*，則為 `,` （逗號），後面接著*constant_expression*值的十進位標記法。</span><span class="sxs-lookup"><span data-stu-id="6aeee-904">Then, if the corresponding *interpolation* has a *constant_expression*, a `,` (comma) followed by the decimal representation of the value of the *constant_expression*</span></span>
      * <span data-ttu-id="6aeee-905">然後，緊接在對應插補後面的*interpolated_regular_string_mid*、 *interpolated_regular_string_end*、 *interpolated_verbatim_string_mid*或*interpolated_verbatim_string_end* 。</span><span class="sxs-lookup"><span data-stu-id="6aeee-905">Then the *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* or *interpolated_verbatim_string_end* immediately following the corresponding interpolation.</span></span>

<span data-ttu-id="6aeee-906">後續引數就是*插補*（如果有的話）中的*運算式*（如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-906">The subsequent arguments are simply the *expressions* from the *interpolations* (if any), in order.</span></span>

<span data-ttu-id="6aeee-907">TODO：範例。</span><span class="sxs-lookup"><span data-stu-id="6aeee-907">TODO: examples.</span></span>


### <a name="simple-names"></a><span data-ttu-id="6aeee-908">簡單名稱</span><span class="sxs-lookup"><span data-stu-id="6aeee-908">Simple names</span></span>

<span data-ttu-id="6aeee-909">*Simple_name*是由識別碼組成，選擇性地後面接著類型引數清單：</span><span class="sxs-lookup"><span data-stu-id="6aeee-909">A *simple_name* consists of an identifier, optionally followed by a type argument list:</span></span>

```antlr
simple_name
    : identifier type_argument_list?
    ;
```

<span data-ttu-id="6aeee-910">*Simple_name*的格式為 `I` 或表單 `I<A1,...,Ak>`，其中 `I` 是單一識別碼，而 `<A1,...,Ak>` 是選擇性的*type_argument_list*。</span><span class="sxs-lookup"><span data-stu-id="6aeee-910">A *simple_name* is either of the form `I` or of the form `I<A1,...,Ak>`, where `I` is a single identifier and `<A1,...,Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="6aeee-911">未指定*type_argument_list*時，請考慮 `K` 為零。</span><span class="sxs-lookup"><span data-stu-id="6aeee-911">When no *type_argument_list* is specified, consider `K` to be zero.</span></span> <span data-ttu-id="6aeee-912">*Simple_name*的評估和分類方式如下：</span><span class="sxs-lookup"><span data-stu-id="6aeee-912">The *simple_name* is evaluated and classified as follows:</span></span>

*  <span data-ttu-id="6aeee-913">如果 `K` 為零，且*simple_name*出現在*區塊*內，而且如果*區塊*的（或封閉*區塊*的）區域變數宣告空間（[宣告）包含](basic-concepts.md#declarations)區域變數、參數或常數，並具有名稱 @ no__t-6，然後*simple_name*會參考該區域變數、參數或常數，並分類為變數或值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-913">If `K` is zero and the *simple_name* appears within a *block* and if the *block*'s (or an enclosing *block*'s) local variable declaration space ([Declarations](basic-concepts.md#declarations)) contains a local variable, parameter or constant with name `I`, then the *simple_name* refers to that local variable, parameter or constant and is classified as a variable or value.</span></span>
*  <span data-ttu-id="6aeee-914">如果 `K` 為零，且*simple_name*出現在泛型方法宣告的主體內，而且如果該宣告包含名稱為 @ no__t-2 的類型參數，則*simple_name*會參考該類型參數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-914">If `K` is zero and the *simple_name* appears within the body of a generic method declaration and if that declaration includes a type parameter with name `I`, then the *simple_name* refers to that type parameter.</span></span>
*  <span data-ttu-id="6aeee-915">否則，針對每個實例類型 @ no__t-0 （[實例類型](classes.md#the-instance-type)），開頭為立即封入類型宣告的實例類型，並繼續進行每個封入類別或結構宣告的實例類型（如果有的話）：</span><span class="sxs-lookup"><span data-stu-id="6aeee-915">Otherwise, for each instance type `T` ([The instance type](classes.md#the-instance-type)), starting with the instance type of the immediately enclosing type declaration and continuing with the instance type of each enclosing class or struct declaration (if any):</span></span>
   *  <span data-ttu-id="6aeee-916">如果 `K` 為零，且 `T` 的宣告包含名稱為 @ no__t-2 的類型參數，則*simple_name*會參考該類型參數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-916">If `K` is zero and the declaration of `T` includes a type parameter with name `I`, then the *simple_name* refers to that type parameter.</span></span>
   *  <span data-ttu-id="6aeee-917">否則，如果 `T` 中的 `I` 的成員查閱（[成員查閱](expressions.md#member-lookup)）包含 `K` @ no__t-4type 引數，會產生符合的結果：</span><span class="sxs-lookup"><span data-stu-id="6aeee-917">Otherwise, if a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `T` with `K` type arguments produces a match:</span></span>
      * <span data-ttu-id="6aeee-918">如果 `T` 是立即封入類別或結構型別的實例型別，而且查閱識別一或多個方法，則結果會是具有 `this` 之相關聯實例運算式的方法群組。</span><span class="sxs-lookup"><span data-stu-id="6aeee-918">If `T` is the instance type of the immediately enclosing class or struct type and the lookup identifies one or more methods, the result is a method group with an associated instance expression of `this`.</span></span> <span data-ttu-id="6aeee-919">如果指定了類型引數清單，則會使用它來呼叫泛型方法（[方法調用](expressions.md#method-invocations)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-919">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
      * <span data-ttu-id="6aeee-920">否則，如果 `T` 是立即封入類別或結構型別的實例型別，如果查閱識別實例成員，而且參考發生在實例函式的主體、實例方法或實例存取子中，則結果與 `this.I` 格式的成員存取權（[成員存取](expressions.md#member-access)）相同。</span><span class="sxs-lookup"><span data-stu-id="6aeee-920">Otherwise, if `T` is the instance type of the immediately enclosing class or struct type, if the lookup identifies an instance member, and if the reference occurs within the body of an instance constructor, an instance method, or an instance accessor, the result is the same as a member access ([Member access](expressions.md#member-access)) of the form `this.I`.</span></span> <span data-ttu-id="6aeee-921">只有當 `K` 為零時，才會發生這種情況。</span><span class="sxs-lookup"><span data-stu-id="6aeee-921">This can only happen when `K` is zero.</span></span>
      * <span data-ttu-id="6aeee-922">否則，結果會與 `T.I` 或 `T.I<A1,...,Ak>` 格式的成員存取（[成員存取](expressions.md#member-access)）相同。</span><span class="sxs-lookup"><span data-stu-id="6aeee-922">Otherwise, the result is the same as a member access ([Member access](expressions.md#member-access)) of the form `T.I` or `T.I<A1,...,Ak>`.</span></span> <span data-ttu-id="6aeee-923">在此情況下，這是*simple_name*參考實例成員的系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-923">In this case, it is a binding-time error for the *simple_name* to refer to an instance member.</span></span>

*  <span data-ttu-id="6aeee-924">否則，針對每個命名空間 @ no__t-0，從*simple_name*發生所在的命名空間開始，繼續進行每個封入命名空間（如果有的話），並以全域命名空間結束，直到實體找到之後，才會評估下列步驟：</span><span class="sxs-lookup"><span data-stu-id="6aeee-924">Otherwise, for each namespace `N`, starting with the namespace in which the *simple_name* occurs, continuing with each enclosing namespace (if any), and ending with the global namespace, the following steps are evaluated until an entity is located:</span></span>
   *  <span data-ttu-id="6aeee-925">如果 `K` 為零，且 `I` 是 @ no__t-2 中的命名空間名稱，則：</span><span class="sxs-lookup"><span data-stu-id="6aeee-925">If `K` is zero and `I` is the name of a namespace in `N`, then:</span></span>
      * <span data-ttu-id="6aeee-926">如果發生*simple_name*的位置是以 `N` 的命名空間宣告括住，且命名空間宣告包含*extern_alias_directive*或*using_alias_directive* ，而該名稱與 @ no__t-4 的關聯命名空間或類型，則*simple_name*會不明確，且會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-926">If the location where the *simple_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *simple_name* is ambiguous and a compile-time error occurs.</span></span>
      * <span data-ttu-id="6aeee-927">否則， *simple_name*會參考 `N` 中名為 `I` 的命名空間。</span><span class="sxs-lookup"><span data-stu-id="6aeee-927">Otherwise, the *simple_name* refers to the namespace named `I` in `N`.</span></span>
   *  <span data-ttu-id="6aeee-928">否則，如果 `N` 包含名稱為 @ no__t-1 的可存取類型和 `K` @ no__t-3type 參數，則：</span><span class="sxs-lookup"><span data-stu-id="6aeee-928">Otherwise, if `N` contains an accessible type having name `I` and `K` type parameters, then:</span></span>
      * <span data-ttu-id="6aeee-929">如果 `K` 為零，且*simple_name*發生所在的位置是以 `N` 的命名空間宣告括住，且命名空間宣告包含*extern_alias_directive*或*using_alias_directive* ，而該關聯名稱 @ no__t-5，具有命名空間或類型，則*simple_name*是不明確的，而且會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-929">If `K` is zero and the location where the *simple_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *simple_name* is ambiguous and a compile-time error occurs.</span></span>
      * <span data-ttu-id="6aeee-930">否則， *namespace_or_type_name*會參考以指定的型別引數所構成的型別。</span><span class="sxs-lookup"><span data-stu-id="6aeee-930">Otherwise, the *namespace_or_type_name* refers to the type constructed with the given type arguments.</span></span>
   *  <span data-ttu-id="6aeee-931">否則，如果發生*simple_name*的位置是以 @ no__t-1 的命名空間宣告括住：</span><span class="sxs-lookup"><span data-stu-id="6aeee-931">Otherwise, if the location where the *simple_name* occurs is enclosed by a namespace declaration for `N`:</span></span>
      * <span data-ttu-id="6aeee-932">如果 `K` 為零，且命名空間宣告包含*extern_alias_directive*或*using_alias_directive* ，而此名稱與已匯入的命名空間或類型建立關聯，則*simple_name*會參考該命名空間或型.</span><span class="sxs-lookup"><span data-stu-id="6aeee-932">If `K` is zero and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with an imported namespace or type, then the *simple_name* refers to that namespace or type.</span></span>
      * <span data-ttu-id="6aeee-933">否則，如果命名空間宣告的*using_namespace_directive*s 和*using_static_directive*所匯入的命名空間和類型宣告只包含一個可存取的類型，或具有名稱 @ no__ 的非延伸靜態成員，則為。t-2 和 `K` @ no__t-4type 參數，然後*simple_name*會參考以給定類型引數所建立的類型或成員。</span><span class="sxs-lookup"><span data-stu-id="6aeee-933">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_static_directive*s of the namespace declaration contain exactly one accessible type or non-extension static member having name `I` and `K` type parameters, then the *simple_name* refers to that type or member constructed with the given type arguments.</span></span>
      * <span data-ttu-id="6aeee-934">否則，如果命名空間宣告的*using_namespace_directive*所匯入的命名空間和類型包含一個以上的可存取類型或非擴充方法靜態成員，且名稱為 @ no__t-1，而 `K` @ no__t-3type 參數，然後， *simple_name*會不明確，而且會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-934">Otherwise, if the namespaces and types imported by the *using_namespace_directive*s of the namespace declaration contain more than one accessible type or non-extension-method static member having name `I` and `K` type parameters, then the *simple_name* is ambiguous and an error occurs.</span></span>

   <span data-ttu-id="6aeee-935">請注意，整個步驟與*namespace_or_type_name* （[命名空間和型別名稱](basic-concepts.md#namespace-and-type-names)）處理中的對應步驟完全平行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-935">Note that this entire step is exactly parallel to the corresponding step in the processing of a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)).</span></span>

*  <span data-ttu-id="6aeee-936">否則， *simple_name*會是未定義的，而且會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-936">Otherwise, the *simple_name* is undefined and a compile-time error occurs.</span></span>


### <a name="parenthesized-expressions"></a><span data-ttu-id="6aeee-937">以括弧括住的運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-937">Parenthesized expressions</span></span>

<span data-ttu-id="6aeee-938">*Parenthesized_expression*是由括在括弧中的*運算式*所組成。</span><span class="sxs-lookup"><span data-stu-id="6aeee-938">A *parenthesized_expression* consists of an *expression* enclosed in parentheses.</span></span>

```antlr
parenthesized_expression
    : '(' expression ')'
    ;
```

<span data-ttu-id="6aeee-939">*Parenthesized_expression*是藉由評估括弧內的*運算式*來進行評估。</span><span class="sxs-lookup"><span data-stu-id="6aeee-939">A *parenthesized_expression* is evaluated by evaluating the *expression* within the parentheses.</span></span> <span data-ttu-id="6aeee-940">如果括弧內的*運算式*代表命名空間或類型，則會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-940">If the *expression* within the parentheses denotes a namespace or type, a compile-time error occurs.</span></span> <span data-ttu-id="6aeee-941">否則， *parenthesized_expression*的結果會是包含*運算式*的評估結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-941">Otherwise, the result of the *parenthesized_expression* is the result of the evaluation of the contained *expression*.</span></span>

### <a name="member-access"></a><span data-ttu-id="6aeee-942">成員存取</span><span class="sxs-lookup"><span data-stu-id="6aeee-942">Member access</span></span>

<span data-ttu-id="6aeee-943">*Member_access*是由*primary_expression*、 *predefined_type*或*qualified_alias_member*所組成，後面接著 "`.`" token，後面接著一個*識別碼*，然後選擇性地加上*type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="6aeee-943">A *member_access* consists of a *primary_expression*, a *predefined_type*, or a *qualified_alias_member*, followed by a "`.`" token, followed by an *identifier*, optionally followed by a *type_argument_list*.</span></span>

```antlr
member_access
    : primary_expression '.' identifier type_argument_list?
    | predefined_type '.' identifier type_argument_list?
    | qualified_alias_member '.' identifier
    ;

predefined_type
    : 'bool'   | 'byte'  | 'char'  | 'decimal' | 'double' | 'float' | 'int' | 'long'
    | 'object' | 'sbyte' | 'short' | 'string'  | 'uint'   | 'ulong' | 'ushort'
    ;
```

<span data-ttu-id="6aeee-944">*Qualified_alias_member*生產環境是在[命名空間別名限定詞](namespaces.md#namespace-alias-qualifiers)中定義。</span><span class="sxs-lookup"><span data-stu-id="6aeee-944">The *qualified_alias_member* production is defined in [Namespace alias qualifiers](namespaces.md#namespace-alias-qualifiers).</span></span>

<span data-ttu-id="6aeee-945">*Member_access*的格式為 `E.I` 或表單 `E.I<A1, ..., Ak>`，其中 `E` 是主要運算式，`I` 是單一識別碼，而 `<A1, ..., Ak>` 是選擇性的*type_argument_list*。</span><span class="sxs-lookup"><span data-stu-id="6aeee-945">A *member_access* is either of the form `E.I` or of the form `E.I<A1, ..., Ak>`, where `E` is a primary-expression, `I` is a single identifier and `<A1, ..., Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="6aeee-946">未指定*type_argument_list*時，請考慮 `K` 為零。</span><span class="sxs-lookup"><span data-stu-id="6aeee-946">When no *type_argument_list* is specified, consider `K` to be zero.</span></span>

<span data-ttu-id="6aeee-947">具有類型 `dynamic` 之*primary_expression*的*member_access*會動態繫結（[動態](expressions.md#dynamic-binding)系結）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-947">A *member_access* with a *primary_expression* of type `dynamic` is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="6aeee-948">在此情況下，編譯器會將成員存取分類為 `dynamic` 類型的屬性存取。</span><span class="sxs-lookup"><span data-stu-id="6aeee-948">In this case the compiler classifies the member access as a property access of type `dynamic`.</span></span> <span data-ttu-id="6aeee-949">下列規則會在執行時間套用*member_access*的意義，並使用執行時間類型，而不是*primary_expression*的編譯時間類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-949">The rules below to determine the meaning of the *member_access* are then applied at run-time, using the run-time type instead of the compile-time type of the *primary_expression*.</span></span> <span data-ttu-id="6aeee-950">如果此執行時間分類會導致方法群組，則成員存取必須是*invocation_expression*的*primary_expression* 。</span><span class="sxs-lookup"><span data-stu-id="6aeee-950">If this run-time classification leads to a method group, then the member access must be the *primary_expression* of an *invocation_expression*.</span></span>

<span data-ttu-id="6aeee-951">*Member_access*的評估和分類方式如下：</span><span class="sxs-lookup"><span data-stu-id="6aeee-951">The *member_access* is evaluated and classified as follows:</span></span>

*  <span data-ttu-id="6aeee-952">如果 `K` 為零，且 `E` 是命名空間，而 `E` 包含名為 @ no__t-3 的嵌套命名空間，則結果會是該命名空間。</span><span class="sxs-lookup"><span data-stu-id="6aeee-952">If `K` is zero and `E` is a namespace and `E` contains a nested namespace with name `I`, then the result is that namespace.</span></span>
*  <span data-ttu-id="6aeee-953">否則，如果 `E` 是命名空間，而 `E` 包含具有 name @ no__t-2 和 `K` @ no__t-4type 參數的可存取型別，則結果會是以給定型別引數所構成的型別。</span><span class="sxs-lookup"><span data-stu-id="6aeee-953">Otherwise, if `E` is a namespace and `E` contains an accessible type having name `I` and `K` type parameters, then the result is that type constructed with the given type arguments.</span></span>
*  <span data-ttu-id="6aeee-954">如果 `E` 是分類為類型的*predefined_type*或*primary_expression* ，則為，如果 `E` 不是類型參數，而且 `E` 中 `I` 的成員查閱（[成員查閱](expressions.md#member-lookup)）包含 `K` @ no__t-8type 參數會產生相符項，然後 `E.I` 進行評估，並將其分類如下：</span><span class="sxs-lookup"><span data-stu-id="6aeee-954">If `E` is a *predefined_type* or a *primary_expression* classified as a type, if `E` is not a type parameter, and if a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `E` with `K` type parameters produces a match, then `E.I` is evaluated and classified as follows:</span></span>
   *  <span data-ttu-id="6aeee-955">如果 `I` 識別型別，則結果會是以給定型別引數所構成的型別。</span><span class="sxs-lookup"><span data-stu-id="6aeee-955">If `I` identifies a type, then the result is that type constructed with the given type arguments.</span></span>
   *  <span data-ttu-id="6aeee-956">如果 `I` 識別一個或多個方法，則結果會是沒有相關聯實例運算式的方法群組。</span><span class="sxs-lookup"><span data-stu-id="6aeee-956">If `I` identifies one or more methods, then the result is a method group with no associated instance expression.</span></span> <span data-ttu-id="6aeee-957">如果指定了類型引數清單，則會使用它來呼叫泛型方法（[方法調用](expressions.md#method-invocations)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-957">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
   *  <span data-ttu-id="6aeee-958">如果 `I` 識別 @no__t 1 屬性，則結果會是沒有相關聯實例運算式的屬性存取。</span><span class="sxs-lookup"><span data-stu-id="6aeee-958">If `I` identifies a `static` property, then the result is a property access with no associated instance expression.</span></span>
   *  <span data-ttu-id="6aeee-959">如果 `I` 識別 `static` 欄位：</span><span class="sxs-lookup"><span data-stu-id="6aeee-959">If `I` identifies a `static` field:</span></span>
      * <span data-ttu-id="6aeee-960">如果欄位是 `readonly`，而參考發生在宣告欄位之類別或結構的靜態函式外，則結果會是值，也就是 @ no__t-2 中靜態欄位 @ no__t-1 的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-960">If the field is `readonly` and the reference occurs outside the static constructor of the class or struct in which the field is declared, then the result is a value, namely the value of the static field `I` in `E`.</span></span>
      * <span data-ttu-id="6aeee-961">否則，結果會是變數，亦即 @ no__t-1 中的靜態欄位 @ no__t-0。</span><span class="sxs-lookup"><span data-stu-id="6aeee-961">Otherwise, the result is a variable, namely the static field `I` in `E`.</span></span>
   *  <span data-ttu-id="6aeee-962">如果 `I` 則識別 `static` 事件：</span><span class="sxs-lookup"><span data-stu-id="6aeee-962">If `I` identifies a `static` event:</span></span>
      * <span data-ttu-id="6aeee-963">如果參考發生在宣告事件的類別或結構中，而事件是在沒有*event_accessor_declarations* （[事件](classes.md#events)）的情況下宣告，則 `E.I` 的處理方式完全如同 `I` 是靜態欄位一樣。</span><span class="sxs-lookup"><span data-stu-id="6aeee-963">If the reference occurs within the class or struct in which the event is declared, and the event was declared without *event_accessor_declarations* ([Events](classes.md#events)), then `E.I` is processed exactly as if `I` were a static field.</span></span>
      * <span data-ttu-id="6aeee-964">否則，結果會是沒有相關聯實例運算式的事件存取。</span><span class="sxs-lookup"><span data-stu-id="6aeee-964">Otherwise, the result is an event access with no associated instance expression.</span></span>
   *  <span data-ttu-id="6aeee-965">如果 `I` 識別常數，則結果會是值，也就是該常數的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-965">If `I` identifies a constant, then the result is a value, namely the value of that constant.</span></span>
    * <span data-ttu-id="6aeee-966">如果 `I` 識別列舉成員，則結果會是值，也就是該列舉成員的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-966">If `I` identifies an enumeration member, then the result is a value, namely the value of that enumeration member.</span></span>
    * <span data-ttu-id="6aeee-967">否則，`E.I` 是不正確成員參考，而且發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-967">Otherwise, `E.I` is an invalid member reference, and a compile-time error occurs.</span></span>
*  <span data-ttu-id="6aeee-968">如果 `E` 是屬性存取、索引子存取、變數或值，則為 @ no__t-1 的類型，而 @no__t-@no__t 4 中 `I` 的成員查閱（[成員查閱](expressions.md#member-lookup)）會產生相符項，則會評估 `E.I`，並分類如下：</span><span class="sxs-lookup"><span data-stu-id="6aeee-968">If `E` is a property access, indexer access, variable, or value, the type of which is `T`, and a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `T` with `K` type arguments produces a match, then `E.I` is evaluated and classified as follows:</span></span>
   *  <span data-ttu-id="6aeee-969">首先，如果 `E` 是屬性或索引子存取，則會取得屬性或索引子存取的值（[運算式的值](expressions.md#values-of-expressions)），而 `E` 則會重新分類為值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-969">First, if `E` is a property or indexer access, then the value of the property or indexer access is obtained ([Values of expressions](expressions.md#values-of-expressions)) and `E` is reclassified as a value.</span></span>
   *  <span data-ttu-id="6aeee-970">如果 `I` 識別一個或多個方法，則結果會是具有相關聯實例運算式 `E` 的方法群組。</span><span class="sxs-lookup"><span data-stu-id="6aeee-970">If `I` identifies one or more methods, then the result is a method group with an associated instance expression of `E`.</span></span> <span data-ttu-id="6aeee-971">如果指定了類型引數清單，則會使用它來呼叫泛型方法（[方法調用](expressions.md#method-invocations)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-971">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
   *  <span data-ttu-id="6aeee-972">如果 `I` 可識別實例屬性，</span><span class="sxs-lookup"><span data-stu-id="6aeee-972">If `I` identifies an instance property,</span></span>
      * <span data-ttu-id="6aeee-973">如果 `E` 是 `this`，`I` 會識別自動執行的屬性（[自動實作為屬性](classes.md#automatically-implemented-properties)），而不使用 setter，而參考會發生在類別或結構類型的實例函式中（`T`），然後結果為變數，也就是 `this` 所指定 `T` 的實例中，`I` 所指定之 auto 屬性的隱藏支援欄位。</span><span class="sxs-lookup"><span data-stu-id="6aeee-973">If `E` is `this`, `I` identifies an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)) without a setter, and the reference occurs within an instance constructor for a class or struct type `T`, then the result is a variable, namely the hidden backing field for the auto-property given by `I` in the instance of `T` given by `this`.</span></span>
      * <span data-ttu-id="6aeee-974">否則，結果會是具有 @ no__t-0 相關聯實例運算式的屬性存取。</span><span class="sxs-lookup"><span data-stu-id="6aeee-974">Otherwise, the result is a property access with an associated instance expression of `E`.</span></span>
   *  <span data-ttu-id="6aeee-975">如果 `T` 是*class_type* ，而 `I` 則識別該*class_type*的實例欄位：</span><span class="sxs-lookup"><span data-stu-id="6aeee-975">If `T` is a *class_type* and `I` identifies an instance field of that *class_type*:</span></span>
      * <span data-ttu-id="6aeee-976">如果 `E` 的值為 `null`，則會擲回 `System.NullReferenceException`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-976">If the value of `E` is `null`, then a `System.NullReferenceException` is thrown.</span></span>
      * <span data-ttu-id="6aeee-977">否則，如果欄位是 `readonly`，而參考發生在宣告欄位之類別的實例函式之外，則結果會是值，也就是 @ no__t-2 所參考物件中 @ no__t-1 欄位的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-977">Otherwise, if the field is `readonly` and the reference occurs outside an instance constructor of the class in which the field is declared, then the result is a value, namely the value of the field `I` in the object referenced by `E`.</span></span>
      * <span data-ttu-id="6aeee-978">否則，結果會是變數，亦即 @ no__t-1 所參考物件中的 @ no__t-0 欄位。</span><span class="sxs-lookup"><span data-stu-id="6aeee-978">Otherwise, the result is a variable, namely the field `I` in the object referenced by `E`.</span></span>
   *  <span data-ttu-id="6aeee-979">如果 `T` 是*struct_type* ，而 `I` 則識別該*struct_type*的實例欄位：</span><span class="sxs-lookup"><span data-stu-id="6aeee-979">If `T` is a *struct_type* and `I` identifies an instance field of that *struct_type*:</span></span>
      * <span data-ttu-id="6aeee-980">如果 `E` 是值，或如果欄位是 `readonly`，而參考出現在宣告該欄位之結構的實例函式之外，則結果會是值，也就是 @ no__t-3 所指定的結構實例中，@ no__t-2 欄位的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-980">If `E` is a value, or if the field is `readonly` and the reference occurs outside an instance constructor of the struct in which the field is declared, then the result is a value, namely the value of the field `I` in the struct instance given by `E`.</span></span>
      * <span data-ttu-id="6aeee-981">否則，結果會是變數，也就是 @ no__t-1 所指定的結構實例中的 @ no__t-0 欄位。</span><span class="sxs-lookup"><span data-stu-id="6aeee-981">Otherwise, the result is a variable, namely the field `I` in the struct instance given by `E`.</span></span>
   *  <span data-ttu-id="6aeee-982">如果 `I` 可識別實例事件：</span><span class="sxs-lookup"><span data-stu-id="6aeee-982">If `I` identifies an instance event:</span></span>
      * <span data-ttu-id="6aeee-983">如果參考發生在宣告事件的類別或結構中，且事件是在沒有*event_accessor_declarations* （[事件](classes.md#events)）的情況下宣告，而且參考不會做為 `+=` 或 `-=` 運算子的左邊，然後 `E.I` 的處理方式，就如同 `I` 是實例欄位一樣。</span><span class="sxs-lookup"><span data-stu-id="6aeee-983">If the reference occurs within the class or struct in which the event is declared, and the event was declared without *event_accessor_declarations* ([Events](classes.md#events)), and the reference does not occur as the left-hand side of a `+=` or `-=` operator, then `E.I` is processed exactly as if `I` was an instance field.</span></span>
      * <span data-ttu-id="6aeee-984">否則，結果會是具有 @ no__t-0 相關聯實例運算式的事件存取。</span><span class="sxs-lookup"><span data-stu-id="6aeee-984">Otherwise, the result is an event access with an associated instance expression of `E`.</span></span>
*  <span data-ttu-id="6aeee-985">否則，會嘗試處理 `E.I` 做為擴充方法調用（[擴充方法](expressions.md#extension-method-invocations)調用）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-985">Otherwise, an attempt is made to process `E.I` as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="6aeee-986">如果失敗，`E.I` 是不正確成員參考，而且發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-986">If this fails, `E.I` is an invalid member reference, and a binding-time error occurs.</span></span>

#### <a name="identical-simple-names-and-type-names"></a><span data-ttu-id="6aeee-987">相同的簡單名稱和類型名稱</span><span class="sxs-lookup"><span data-stu-id="6aeee-987">Identical simple names and type names</span></span>

<span data-ttu-id="6aeee-988">在格式 `E.I` 的成員存取中，如果 `E` 是單一識別碼，而且 `E` 做為*simple_name* （[簡單名稱](expressions.md#simple-names)）的意義是常數、欄位、屬性、區域變數或參數，其類型與 `E` 的意義相同。*type_name* （[命名空間和類型名稱](basic-concepts.md#namespace-and-type-names)），則允許 `E` 的可能意義。</span><span class="sxs-lookup"><span data-stu-id="6aeee-988">In a member access of the form `E.I`, if `E` is a single identifier, and if the meaning of `E` as a *simple_name* ([Simple names](expressions.md#simple-names)) is a constant, field, property, local variable, or parameter with the same type as the meaning of `E` as a *type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)), then both possible meanings of `E` are permitted.</span></span> <span data-ttu-id="6aeee-989">@No__t-0 的兩個可能意義永遠不明確，因為在這兩種情況下，`I` 必須是 `E` 類型的成員。</span><span class="sxs-lookup"><span data-stu-id="6aeee-989">The two possible meanings of `E.I` are never ambiguous, since `I` must necessarily be a member of the type `E` in both cases.</span></span> <span data-ttu-id="6aeee-990">換句話說，此規則只允許存取原本發生編譯時期錯誤的 `E` 的靜態成員和巢狀型別。</span><span class="sxs-lookup"><span data-stu-id="6aeee-990">In other words, the rule simply permits access to the static members and nested types of `E` where a compile-time error would otherwise have occurred.</span></span> <span data-ttu-id="6aeee-991">例如:</span><span class="sxs-lookup"><span data-stu-id="6aeee-991">For example:</span></span>
```csharp
struct Color
{
    public static readonly Color White = new Color(...);
    public static readonly Color Black = new Color(...);

    public Color Complement() {...}
}

class A
{
    public Color Color;                // Field Color of type Color

    void F() {
        Color = Color.Black;           // References Color.Black static member
        Color = Color.Complement();    // Invokes Complement() on Color field
    }

    static void G() {
        Color c = Color.White;         // References Color.White static member
    }
}
```

#### <a name="grammar-ambiguities"></a><span data-ttu-id="6aeee-992">文法多義性</span><span class="sxs-lookup"><span data-stu-id="6aeee-992">Grammar ambiguities</span></span>

<span data-ttu-id="6aeee-993">*Simple_name* （[簡單名稱](expressions.md#simple-names)）和*member_access* （[成員存取](expressions.md#member-access)）的生產可能會使運算式的文法中的多義性增加。</span><span class="sxs-lookup"><span data-stu-id="6aeee-993">The productions for *simple_name* ([Simple names](expressions.md#simple-names)) and *member_access* ([Member access](expressions.md#member-access)) can give rise to ambiguities in the grammar for expressions.</span></span> <span data-ttu-id="6aeee-994">例如，語句：</span><span class="sxs-lookup"><span data-stu-id="6aeee-994">For example, the statement:</span></span>
```csharp
F(G<A,B>(7));
```
<span data-ttu-id="6aeee-995">可以使用兩個引數（`G < A` 和 `B > (7)`）來解讀為 `F` 的呼叫。</span><span class="sxs-lookup"><span data-stu-id="6aeee-995">could be interpreted as a call to `F` with two arguments, `G < A` and `B > (7)`.</span></span> <span data-ttu-id="6aeee-996">或者，它可以解讀為 `F` 的呼叫，其中包含一個引數，這是呼叫具有兩個型別引數和一個一般引數的泛型方法 @ no__t-1。</span><span class="sxs-lookup"><span data-stu-id="6aeee-996">Alternatively, it could be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span>

<span data-ttu-id="6aeee-997">如果可以將一連串的 token 剖析（在內容中）為*simple_name* （[簡單名稱](expressions.md#simple-names)）、 *member_access* （[成員存取](expressions.md#member-access)），或*pointer_member_access* （[指標成員存取](unsafe-code.md#pointer-member-access)）以*type_argument_ 結尾list* （[類型引數](types.md#type-arguments)），會檢查緊接在結尾 `>` token 後面的 token。</span><span class="sxs-lookup"><span data-stu-id="6aeee-997">If a sequence of tokens can be parsed (in context) as a *simple_name* ([Simple names](expressions.md#simple-names)), *member_access* ([Member access](expressions.md#member-access)), or *pointer_member_access* ([Pointer member access](unsafe-code.md#pointer-member-access)) ending with a *type_argument_list* ([Type arguments](types.md#type-arguments)), the token immediately following the closing `>` token is examined.</span></span> <span data-ttu-id="6aeee-998">如果它是下列其中一個</span><span class="sxs-lookup"><span data-stu-id="6aeee-998">If it is one of</span></span>
```csharp
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```
<span data-ttu-id="6aeee-999">然後， *type_argument_list*會保留為*simple_name*、 *member_access*或*pointer_member_access*的一部分，而且會捨棄標記序列的任何其他可能剖析。</span><span class="sxs-lookup"><span data-stu-id="6aeee-999">then the *type_argument_list* is retained as part of the *simple_name*, *member_access* or *pointer_member_access* and any other possible parse of the sequence of tokens is discarded.</span></span> <span data-ttu-id="6aeee-1000">否則，即使沒有其他可能的 token 序列剖析，也不會將*type_argument_list*視為*simple_name*、 *member_access*或*pointer_member_access*的一部分。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1000">Otherwise, the *type_argument_list* is not considered to be part of the *simple_name*, *member_access* or *pointer_member_access*, even if there is no other possible parse of the sequence of tokens.</span></span> <span data-ttu-id="6aeee-1001">請注意，剖析*namespace_or_type_name*中的*type_argument_list* （[命名空間和型別名稱](basic-concepts.md#namespace-and-type-names)）時，不會套用這些規則。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1001">Note that these rules are not applied when parsing a *type_argument_list* in a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)).</span></span> <span data-ttu-id="6aeee-1002">陳述式</span><span class="sxs-lookup"><span data-stu-id="6aeee-1002">The statement</span></span>
```csharp
F(G<A,B>(7));
```
<span data-ttu-id="6aeee-1003">根據此規則，會以一個引數來解讀 `F` 的呼叫，這是使用兩個類型引數和一個一般引數呼叫泛型方法 `G`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1003">will, according to this rule, be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span> <span data-ttu-id="6aeee-1004">語句</span><span class="sxs-lookup"><span data-stu-id="6aeee-1004">The statements</span></span>
```csharp
F(G < A, B > 7);
F(G < A, B >> 7);
```
<span data-ttu-id="6aeee-1005">每個都會以兩個引數的方式，解讀為 `F` 的呼叫。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1005">will each be interpreted as a call to `F` with two arguments.</span></span> <span data-ttu-id="6aeee-1006">陳述式</span><span class="sxs-lookup"><span data-stu-id="6aeee-1006">The statement</span></span>
```csharp
x = F < A > +y;
```
<span data-ttu-id="6aeee-1007">將會解讀為小於運算子、大於運算子和一元加號運算子，如同語句已撰寫 `x = (F < A) > (+y)`，而不是*type_argument_list*後面接著二元加號運算子的*simple_name* 。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1007">will be interpreted as a less than operator, greater than operator, and unary plus operator, as if the statement had been written `x = (F < A) > (+y)`, instead of as a *simple_name* with a *type_argument_list* followed by a binary plus operator.</span></span> <span data-ttu-id="6aeee-1008">在語句中</span><span class="sxs-lookup"><span data-stu-id="6aeee-1008">In the statement</span></span>
```csharp
x = y is C<T> + z;
```
<span data-ttu-id="6aeee-1009">`C<T>` 的權杖會以*type_argument_list*的形式轉譯為*namespace_or_type_name* 。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1009">the tokens `C<T>` are interpreted as a *namespace_or_type_name* with a *type_argument_list*.</span></span>

### <a name="invocation-expressions"></a><span data-ttu-id="6aeee-1010">叫用運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-1010">Invocation expressions</span></span>

<span data-ttu-id="6aeee-1011">*Invocation_expression*是用來叫用方法。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1011">An *invocation_expression* is used to invoke a method.</span></span>

```antlr
invocation_expression
    : primary_expression '(' argument_list? ')'
    ;
```

<span data-ttu-id="6aeee-1012">如果至少有下列其中一個保留，則*invocation_expression*會動態繫結（[動態](expressions.md#dynamic-binding)系結）：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1012">An *invocation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) if at least one of the following holds:</span></span>

* <span data-ttu-id="6aeee-1013">*Primary_expression*的編譯階段類型 `dynamic`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1013">The *primary_expression* has compile-time type `dynamic`.</span></span>
* <span data-ttu-id="6aeee-1014">選擇性*argument_list*的至少一個引數具有編譯時間類型 `dynamic`，而*primary_expression*沒有委派類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1014">At least one argument of the optional *argument_list* has compile-time type `dynamic` and the *primary_expression* does not have a delegate type.</span></span>

<span data-ttu-id="6aeee-1015">在此情況下，編譯器會將*invocation_expression*分類為 `dynamic` 類型的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1015">In this case the compiler classifies the *invocation_expression* as a value of type `dynamic`.</span></span> <span data-ttu-id="6aeee-1016">下列規則會在執行時間套用*invocation_expression*的意義，並使用執行時間類型，而不是具有編譯時間類型之*primary_expression*和引數的編譯時間類型 `dynamic`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1016">The rules below to determine the meaning of the *invocation_expression* are then applied at run-time, using the run-time type instead of the compile-time type of those of the *primary_expression* and arguments which have the compile-time type `dynamic`.</span></span> <span data-ttu-id="6aeee-1017">如果*primary_expression*沒有編譯時間類型 `dynamic`，則方法調用會進行有限的編譯時間檢查，如動態多載[解析的編譯階段檢查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)中所述。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1017">If the *primary_expression* does not have compile-time type `dynamic`, then the method invocation undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="6aeee-1018">*Invocation_expression*的*primary_expression*必須是方法群組或*delegate_type*的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1018">The *primary_expression* of an *invocation_expression* must be a method group or a value of a *delegate_type*.</span></span> <span data-ttu-id="6aeee-1019">如果*primary_expression*是方法群組，則*invocation_expression*是方法調用（[方法調用](expressions.md#method-invocations)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1019">If the *primary_expression* is a method group, the *invocation_expression* is a method invocation ([Method invocations](expressions.md#method-invocations)).</span></span> <span data-ttu-id="6aeee-1020">如果*primary_expression*是*delegate_type*的值，則*invocation_expression*會是委派調用（[委派調用](expressions.md#delegate-invocations)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1020">If the *primary_expression* is a value of a *delegate_type*, the *invocation_expression* is a delegate invocation ([Delegate invocations](expressions.md#delegate-invocations)).</span></span> <span data-ttu-id="6aeee-1021">如果*primary_expression*不是方法群組，也不是*delegate_type*的值，則會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1021">If the *primary_expression* is neither a method group nor a value of a *delegate_type*, a binding-time error occurs.</span></span>

<span data-ttu-id="6aeee-1022">選擇性的*argument_list* （[引數清單](expressions.md#argument-lists)）提供方法之參數的值或變數參考。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1022">The optional *argument_list* ([Argument lists](expressions.md#argument-lists)) provides values or variable references for the parameters of the method.</span></span>

<span data-ttu-id="6aeee-1023">評估*invocation_expression*的結果分類如下：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1023">The result of evaluating an *invocation_expression* is classified as follows:</span></span>

*  <span data-ttu-id="6aeee-1024">如果*invocation_expression*叫用傳回 `void` 的方法或委派，則結果為「無」。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1024">If the *invocation_expression* invokes a method or delegate that returns `void`, the result is nothing.</span></span> <span data-ttu-id="6aeee-1025">只有在*statement_expression* （[運算式語句](statements.md#expression-statements)）的內容中，或做為*lambda_expression* （匿名函式[運算式](expressions.md#anonymous-function-expressions)）的主體時，才允許分類為任何內容的運算式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1025">An expression that is classified as nothing is permitted only in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)) or as the body of a *lambda_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)).</span></span> <span data-ttu-id="6aeee-1026">否則會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1026">Otherwise a binding-time error occurs.</span></span>
*  <span data-ttu-id="6aeee-1027">否則，結果會是方法或委派所傳回之類型的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1027">Otherwise, the result is a value of the type returned by the method or delegate.</span></span>

#### <a name="method-invocations"></a><span data-ttu-id="6aeee-1028">方法調用</span><span class="sxs-lookup"><span data-stu-id="6aeee-1028">Method invocations</span></span>

<span data-ttu-id="6aeee-1029">針對方法調用， *invocation_expression*的*primary_expression*必須是方法群組。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1029">For a method invocation, the *primary_expression* of the *invocation_expression* must be a method group.</span></span> <span data-ttu-id="6aeee-1030">方法群組會識別要叫用的一個方法，或用來選擇要叫用之特定方法的多載方法集合。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1030">The method group identifies the one method to invoke or the set of overloaded methods from which to choose a specific method to invoke.</span></span> <span data-ttu-id="6aeee-1031">在後者的情況下，判斷要叫用的特定方法是根據*argument_list*中引數類型所提供的內容。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1031">In the latter case, determination of the specific method to invoke is based on the context provided by the types of the arguments in the *argument_list*.</span></span>

<span data-ttu-id="6aeee-1032">格式的方法叫用的系結時間處理 `M(A)`，其中 `M` 是方法群組（可能包括*type_argument_list*），而 `A` 是選擇性的*argument_list*，由下列步驟所組成：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1032">The binding-time processing of a method invocation of the form `M(A)`, where `M` is a method group (possibly including a *type_argument_list*), and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="6aeee-1033">方法調用的一組候選方法會被結構化。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1033">The set of candidate methods for the method invocation is constructed.</span></span> <span data-ttu-id="6aeee-1034">針對與方法群組 `F` 相關聯的每個方法 `M`：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1034">For each method `F` associated with the method group `M`:</span></span>
   *  <span data-ttu-id="6aeee-1035">如果 `F` 是非泛型，`F` 是候選的時機：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1035">If `F` is non-generic, `F` is a candidate when:</span></span>
      * <span data-ttu-id="6aeee-1036">`M` 沒有類型引數清單，且</span><span class="sxs-lookup"><span data-stu-id="6aeee-1036">`M` has no type argument list, and</span></span>
      * <span data-ttu-id="6aeee-1037">`F` 適用于 `A` （適用的函式[成員](expressions.md#applicable-function-member)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1037">`F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
   *  <span data-ttu-id="6aeee-1038">如果 `F` 是泛型，而 `M` 沒有類型引數清單，則在下列情況下，`F` 是候選項：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1038">If `F` is generic and `M` has no type argument list, `F` is a candidate when:</span></span>
      * <span data-ttu-id="6aeee-1039">型別推斷（[型別推斷](expressions.md#type-inference)）成功、推斷呼叫的型別引數清單，以及</span><span class="sxs-lookup"><span data-stu-id="6aeee-1039">Type inference ([Type inference](expressions.md#type-inference)) succeeds, inferring a list of type arguments for the call, and</span></span>
      * <span data-ttu-id="6aeee-1040">一旦推斷的型別引數取代對應的方法型別參數，F 的參數清單中的所有結構化型別都滿足其條件約束（[滿足條件約束](types.md#satisfying-constraints)），而 `F` 的參數清單則適用于關於 `A` （適用的函式[成員](expressions.md#applicable-function-member)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1040">Once the inferred type arguments are substituted for the corresponding method type parameters, all constructed types in the parameter list of F satisfy their constraints ([Satisfying constraints](types.md#satisfying-constraints)), and the parameter list of `F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
   *  <span data-ttu-id="6aeee-1041">如果 `F` 是泛型，而 `M` 包含類型引數清單，則在下列情況下，`F` 是候選項：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1041">If `F` is generic and `M` includes a type argument list, `F` is a candidate when:</span></span>
      * <span data-ttu-id="6aeee-1042">`F` 與類型引數清單中所提供的方法類型參數數目相同，而</span><span class="sxs-lookup"><span data-stu-id="6aeee-1042">`F` has the same number of method type parameters as were supplied in the type argument list, and</span></span>
      * <span data-ttu-id="6aeee-1043">一旦型別引數取代對應的方法型別參數，F 的參數清單中的所有結構化型別都滿足其條件約束（[滿足條件約束](types.md#satisfying-constraints)），而 `F` 的參數清單則適用于`A` （[適用](expressions.md#applicable-function-member)的函式成員）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1043">Once the type arguments are substituted for the corresponding method type parameters, all constructed types in the parameter list of F satisfy their constraints ([Satisfying constraints](types.md#satisfying-constraints)), and the parameter list of `F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
*  <span data-ttu-id="6aeee-1044">候選方法集合會縮減為僅包含來自最多衍生類型的方法：針對集合中 `C.F` 的每個方法，其中 `C` 是宣告方法 `F` 的類型，則會從集合中移除 `C` 的基底類型中宣告的所有方法。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1044">The set of candidate methods is reduced to contain only methods from the most derived types: For each method `C.F` in the set, where `C` is the type in which the method `F` is declared, all methods declared in a base type of `C` are removed from the set.</span></span> <span data-ttu-id="6aeee-1045">此外，如果 `C` 是 `object` 以外的類別類型，則在介面類別型中宣告的所有方法都會從集合中移除。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1045">Furthermore, if `C` is a class type other than `object`, all methods declared in an interface type are removed from the set.</span></span> <span data-ttu-id="6aeee-1046">（第二個規則只有在方法群組是在具有 object 以外的有效基類和非空白的有效介面集的類型參數上進行成員查詢的結果時才會影響）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1046">(This latter rule only has affect when the method group was the result of a member lookup on a type parameter having an effective base class other than object and a non-empty effective interface set.)</span></span>
*  <span data-ttu-id="6aeee-1047">如果產生的候選方法集合是空的，則會放棄沿著下列步驟進行的進一步處理，並改為嘗試將調用當做擴充方法調用（[擴充方法](expressions.md#extension-method-invocations)調用）來處理。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1047">If the resulting set of candidate methods is empty, then further processing along the following steps are abandoned, and instead an attempt is made to process the invocation as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="6aeee-1048">如果失敗，則不存在任何適用的方法，而且會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1048">If this fails, then no applicable methods exist, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="6aeee-1049">候選方法集合的最佳方法是使用多載[解析](expressions.md#overload-resolution)的多載解析規則來識別。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1049">The best method of the set of candidate methods is identified using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="6aeee-1050">如果無法識別單一最佳方法，則方法調用不明確，且會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1050">If a single best method cannot be identified, the method invocation is ambiguous, and a binding-time error occurs.</span></span> <span data-ttu-id="6aeee-1051">執行多載解析時，泛型方法的參數會在取代對應方法類型參數的類型引數（提供或推斷）之後考慮。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1051">When performing overload resolution, the parameters of a generic method are considered after substituting the type arguments (supplied or inferred) for the corresponding method type parameters.</span></span>
*  <span data-ttu-id="6aeee-1052">會執行所選最佳方法的最終驗證：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1052">Final validation of the chosen best method is performed:</span></span>
   * <span data-ttu-id="6aeee-1053">方法會在方法群組的內容中進行驗證：如果最佳方法是靜態方法，則方法群組必須從*simple_name*或*member_access*透過類型產生。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1053">The method is validated in the context of the method group: If the best method is a static method, the method group must have resulted from a *simple_name* or a *member_access* through a type.</span></span> <span data-ttu-id="6aeee-1054">如果最佳方法是實例方法，則方法群組必須從*simple_name*、透過變數或值的*member_access* ，或*base_access*產生。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1054">If the best method is an instance method, the method group must have resulted from a *simple_name*, a *member_access* through a variable or value, or a *base_access*.</span></span> <span data-ttu-id="6aeee-1055">如果這兩項需求都不成立，就會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1055">If neither of these requirements is true, a binding-time error occurs.</span></span>
   * <span data-ttu-id="6aeee-1056">如果最佳方法是泛型方法，則會根據泛型方法上所宣告的條件約束（[滿足條件約束](types.md#satisfying-constraints)）來檢查類型引數（提供或推斷）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1056">If the best method is a generic method, the type arguments (supplied or inferred) are checked against the constraints ([Satisfying constraints](types.md#satisfying-constraints)) declared on the generic method.</span></span> <span data-ttu-id="6aeee-1057">如果任何類型引數不符合類型參數上的對應條件約束，就會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1057">If any type argument does not satisfy the corresponding constraint(s) on the type parameter, a binding-time error occurs.</span></span>

<span data-ttu-id="6aeee-1058">當方法在系結時間由上述步驟選取並驗證之後，就會根據動態多載[解析的編譯時間檢查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)中所述的函式成員調用規則來處理實際的執行時間調用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1058">Once a method has been selected and validated at binding-time by the above steps, the actual run-time invocation is processed according to the rules of function member invocation described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="6aeee-1059">上述解析規則的直覺效果如下所示：若要找出方法叫用所叫用的特定方法，請從方法調用所指示的類型開始，然後繼續繼承鏈，直到找到至少一個適用、可存取的非覆寫方法宣告為止。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1059">The intuitive effect of the resolution rules described above is as follows: To locate the particular method invoked by a method invocation, start with the type indicated by the method invocation and proceed up the inheritance chain until at least one applicable, accessible, non-override method declaration is found.</span></span> <span data-ttu-id="6aeee-1060">然後在該類型中宣告的適用、可存取、非覆寫方法集合上執行類型推斷和多載解析，並藉由選取來叫用方法。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1060">Then perform type inference and overload resolution on the set of applicable, accessible, non-override methods declared in that type and invoke the method thus selected.</span></span> <span data-ttu-id="6aeee-1061">如果找不到任何方法，請改為嘗試將調用當做擴充方法叫用來處理。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1061">If no method was found, try instead to process the invocation as an extension method invocation.</span></span>

#### <a name="extension-method-invocations"></a><span data-ttu-id="6aeee-1062">擴充方法調用</span><span class="sxs-lookup"><span data-stu-id="6aeee-1062">Extension method invocations</span></span>

<span data-ttu-id="6aeee-1063">在方法調用中（在[盒裝實例上的調用](expressions.md#invocations-on-boxed-instances)），其中一個形式</span><span class="sxs-lookup"><span data-stu-id="6aeee-1063">In a method invocation ([Invocations on boxed instances](expressions.md#invocations-on-boxed-instances)) of one of the forms</span></span>
```csharp
expr . identifier ( )

expr . identifier ( args )

expr . identifier < typeargs > ( )

expr . identifier < typeargs > ( args )
```
<span data-ttu-id="6aeee-1064">如果調用的正常處理找不到適用的方法，就會嘗試將此結構當做擴充方法調用來處理。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1064">if the normal processing of the invocation finds no applicable methods, an attempt is made to process the construct as an extension method invocation.</span></span> <span data-ttu-id="6aeee-1065">如果*expr*或任何引數*的編譯*時間類型 `dynamic`，將不會套用擴充方法。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1065">If *expr* or any of the *args* has compile-time type `dynamic`, extension methods will not apply.</span></span>

<span data-ttu-id="6aeee-1066">目標是找出最佳的*type_name* `C`，這樣就可以進行對應的靜態方法調用：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1066">The objective is to find the best *type_name* `C`, so that the corresponding static method invocation can take place:</span></span>
```csharp
C . identifier ( expr )

C . identifier ( expr , args )

C . identifier < typeargs > ( expr )

C . identifier < typeargs > ( expr , args )
```

<span data-ttu-id="6aeee-1067">擴充方法 `Ci.Mj` 符合下列***條件***：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1067">An extension method `Ci.Mj` is ***eligible*** if:</span></span>

*  <span data-ttu-id="6aeee-1068">`Ci` 是非泛型、非嵌套的類別</span><span class="sxs-lookup"><span data-stu-id="6aeee-1068">`Ci` is a non-generic, non-nested class</span></span>
*  <span data-ttu-id="6aeee-1069">@No__t-0 的名稱為*identifier*</span><span class="sxs-lookup"><span data-stu-id="6aeee-1069">The name of `Mj` is *identifier*</span></span>
*  <span data-ttu-id="6aeee-1070">`Mj` 可供存取，並適用于以靜態方法形式套用至引數的情況，如上所示</span><span class="sxs-lookup"><span data-stu-id="6aeee-1070">`Mj` is accessible and applicable when applied to the arguments as a static method as shown above</span></span>
*  <span data-ttu-id="6aeee-1071">從*expr*到 `Mj` 的第一個參數類型的隱含識別、參考或裝箱轉換存在。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1071">An implicit identity, reference or boxing conversion exists from *expr* to the type of the first parameter of `Mj`.</span></span>

<span data-ttu-id="6aeee-1072">@No__t-0 的搜尋會繼續進行，如下所示：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1072">The search for `C` proceeds as follows:</span></span>

*  <span data-ttu-id="6aeee-1073">從最接近的封入命名空間宣告開始，繼續進行每個封入的命名空間宣告，並以包含的編譯單位結束，後續嘗試會尋找一組候選的延伸方法：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1073">Starting with the closest enclosing namespace declaration, continuing with each enclosing namespace declaration, and ending with the containing compilation unit, successive attempts are made to find a candidate set of extension methods:</span></span>
   * <span data-ttu-id="6aeee-1074">如果指定的命名空間或編譯單位直接包含具有合格擴充方法的非泛型型別宣告 `Ci` `Mj`，則這些擴充方法的集合就是候選集合。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1074">If the given namespace or compilation unit directly contains non-generic type declarations `Ci` with eligible extension methods `Mj`, then the set of those extension methods is the candidate set.</span></span>
   * <span data-ttu-id="6aeee-1075">如果在指定的命名空間或編譯單位中，由*using_static_declarations*匯入的類型 `Ci` 並直接宣告于*using_namespace_directive*所匯入的命名空間中，則直接包含符合資格的擴充方法 `Mj`，然後這些擴充方法的集合是候選集合。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1075">If types `Ci` imported by *using_static_declarations* and directly declared in namespaces imported by *using_namespace_directive*s in the given namespace or compilation unit directly contain eligible extension methods `Mj`, then the set of those extension methods is the candidate set.</span></span>
*  <span data-ttu-id="6aeee-1076">如果在任何封閉式命名空間宣告或編譯單位中找不到候選集合，則會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1076">If no candidate set is found in any enclosing namespace declaration or compilation unit, a compile-time error occurs.</span></span>
*  <span data-ttu-id="6aeee-1077">否則，多載解析會套用至候選集合，如（多載[解析](expressions.md#overload-resolution)）中所述。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1077">Otherwise, overload resolution is applied to the candidate set as described in ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="6aeee-1078">如果找不到單一的最佳方法，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1078">If no single best method is found, a compile-time error occurs.</span></span>
*  <span data-ttu-id="6aeee-1079">`C` 是將最佳方法宣告為擴充方法的類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1079">`C` is the type within which the best method is declared as an extension method.</span></span>

<span data-ttu-id="6aeee-1080">使用 `C` 做為目標時，方法呼叫接著會當做靜態方法調用（動態多載[解析的編譯時間檢查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)）來處理。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1080">Using `C` as a target, the method call is then processed as a static method invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span>

<span data-ttu-id="6aeee-1081">上述規則表示實例方法的優先順序高於擴充方法，內部命名空間宣告中提供的擴充方法優先順序高於外部命名空間宣告中提供的擴充方法，以及該延伸模組直接在命名空間中宣告的方法，其優先順序高於使用 namespace 指示詞彙入到同一個命名空間中的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1081">The preceding rules mean that instance methods take precedence over extension methods, that extension methods available in inner namespace declarations take precedence over extension methods available in outer namespace declarations, and that extension methods declared directly in a namespace take precedence over extension methods imported into that same namespace with a using namespace directive.</span></span> <span data-ttu-id="6aeee-1082">例如:</span><span class="sxs-lookup"><span data-stu-id="6aeee-1082">For example:</span></span>
```csharp
public static class E
{
    public static void F(this object obj, int i) { }

    public static void F(this object obj, string s) { }
}

class A { }

class B
{
    public void F(int i) { }
}

class C
{
    public void F(object obj) { }
}

class X
{
    static void Test(A a, B b, C c) {
        a.F(1);              // E.F(object, int)
        a.F("hello");        // E.F(object, string)

        b.F(1);              // B.F(int)
        b.F("hello");        // E.F(object, string)

        c.F(1);              // C.F(object)
        c.F("hello");        // C.F(object)
    }
}
```

<span data-ttu-id="6aeee-1083">在範例中，`B` 的方法優先于第一個擴充方法，而 @no__t 1 的方法優先于這兩個擴充方法。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1083">In the example, `B`'s method takes precedence over the first extension method, and `C`'s method takes precedence over both extension methods.</span></span>

```csharp
public static class C
{
    public static void F(this int i) { Console.WriteLine("C.F({0})", i); }
    public static void G(this int i) { Console.WriteLine("C.G({0})", i); }
    public static void H(this int i) { Console.WriteLine("C.H({0})", i); }
}

namespace N1
{
    public static class D
    {
        public static void F(this int i) { Console.WriteLine("D.F({0})", i); }
        public static void G(this int i) { Console.WriteLine("D.G({0})", i); }
    }
}

namespace N2
{
    using N1;

    public static class E
    {
        public static void F(this int i) { Console.WriteLine("E.F({0})", i); }
    }

    class Test
    {
        static void Main(string[] args)
        {
            1.F();
            2.G();
            3.H();
        }
    }
}
```

<span data-ttu-id="6aeee-1084">此範例的輸出為：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1084">The output of this example is:</span></span>
```console
E.F(1)
D.G(2)
C.H(3)
```
<span data-ttu-id="6aeee-1085">`D.G` 優先于 `C.G`，而 `E.F` 優先于 `D.F` 和 `C.F`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1085">`D.G` takes precedence over `C.G`, and `E.F` takes precedence over both `D.F` and `C.F`.</span></span>

#### <a name="delegate-invocations"></a><span data-ttu-id="6aeee-1086">委派調用</span><span class="sxs-lookup"><span data-stu-id="6aeee-1086">Delegate invocations</span></span>

<span data-ttu-id="6aeee-1087">對於委派調用， *invocation_expression*的*primary_expression*必須是*delegate_type*的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1087">For a delegate invocation, the *primary_expression* of the *invocation_expression* must be a value of a *delegate_type*.</span></span> <span data-ttu-id="6aeee-1088">此外，將*delegate_type*視為具有與*delegate_type*相同之參數清單的函式成員， *delegate_type*必須是適用于 argument_ 的相關（[適用于](expressions.md#applicable-function-member)函式成員） *invocation_expression*的清單。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1088">Furthermore, considering the *delegate_type* to be a function member with the same parameter list as the *delegate_type*, the *delegate_type* must be applicable ([Applicable function member](expressions.md#applicable-function-member)) with respect to the *argument_list* of the *invocation_expression*.</span></span>

<span data-ttu-id="6aeee-1089">@No__t-0 的委派調用的執行時間處理，其中 `D` 是*delegate_type*的*primary_expression* ，而 `A` 是選擇性的*argument_list*，由下列步驟組成：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1089">The run-time processing of a delegate invocation of the form `D(A)`, where `D` is a *primary_expression* of a *delegate_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="6aeee-1090">`D` 會進行評估。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1090">`D` is evaluated.</span></span> <span data-ttu-id="6aeee-1091">如果此評估導致例外狀況，則不會執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1091">If this evaluation causes an exception, no further steps are executed.</span></span>
*  <span data-ttu-id="6aeee-1092">已檢查 `D` 的值是否有效。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1092">The value of `D` is checked to be valid.</span></span> <span data-ttu-id="6aeee-1093">如果 `D` 的值為 `null`，則會擲回 `System.NullReferenceException`，且不會執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1093">If the value of `D` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="6aeee-1094">否則，`D` 是委派實例的參考。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1094">Otherwise, `D` is a reference to a delegate instance.</span></span> <span data-ttu-id="6aeee-1095">函式成員調用（動態多載[解析的編譯階段檢查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)）會在委派的調用清單中的每個可呼叫實體上執行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1095">Function member invocations ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) are performed on each of the callable entities in the invocation list of the delegate.</span></span> <span data-ttu-id="6aeee-1096">對於由實例和實例方法所組成的可呼叫實體，調用的實例是包含在可呼叫實體中的實例。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1096">For callable entities consisting of an instance and instance method, the instance for the invocation is the instance contained in the callable entity.</span></span>

### <a name="element-access"></a><span data-ttu-id="6aeee-1097">元素存取</span><span class="sxs-lookup"><span data-stu-id="6aeee-1097">Element access</span></span>

<span data-ttu-id="6aeee-1098">*Element_access*是由*primary_no_array_creation_expression*所組成，後面接著 "`[`" 權杖，後面接著*argument_list*，後面接著 "`]`" token。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1098">An *element_access* consists of a *primary_no_array_creation_expression*, followed by a "`[`" token, followed by an *argument_list*, followed by a "`]`" token.</span></span> <span data-ttu-id="6aeee-1099">*Argument_list*是由一個或多個*引數*所組成，並以逗號分隔。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1099">The *argument_list* consists of one or more *argument*s, separated by commas.</span></span>

```antlr
element_access
    : primary_no_array_creation_expression '[' expression_list ']'
    ;
```

<span data-ttu-id="6aeee-1100">*Element_access*的*argument_list*不能包含 `ref` 或 @no__t 3 引數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1100">The *argument_list* of an *element_access* is not allowed to contain `ref` or `out` arguments.</span></span>

<span data-ttu-id="6aeee-1101">如果至少有下列其中一個保留，則*element_access*會動態繫結（[動態](expressions.md#dynamic-binding)系結）：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1101">An *element_access* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) if at least one of the following holds:</span></span>

* <span data-ttu-id="6aeee-1102">*Primary_no_array_creation_expression*的編譯階段類型 `dynamic`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1102">The *primary_no_array_creation_expression* has compile-time type `dynamic`.</span></span>
* <span data-ttu-id="6aeee-1103">*Argument_list*至少有一個運算式的編譯階段類型 `dynamic`，而*primary_no_array_creation_expression*沒有陣列類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1103">At least one expression of the *argument_list* has compile-time type `dynamic` and the *primary_no_array_creation_expression* does not have an array type.</span></span>

<span data-ttu-id="6aeee-1104">在此情況下，編譯器會將*element_access*分類為 `dynamic` 類型的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1104">In this case the compiler classifies the *element_access* as a value of type `dynamic`.</span></span> <span data-ttu-id="6aeee-1105">下列規則會在執行時間套用*element_access*的意義，並使用執行時間類型，而不是*primary_no_array_creation_expression*和*argument_list*的編譯時間類型。具有編譯時間類型 `dynamic` 的運算式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1105">The rules below to determine the meaning of the *element_access* are then applied at run-time, using the run-time type instead of the compile-time type of those of the *primary_no_array_creation_expression* and *argument_list* expressions which have the compile-time type `dynamic`.</span></span> <span data-ttu-id="6aeee-1106">如果*primary_no_array_creation_expression*沒有編譯時間類型 `dynamic`，則專案存取會進行有限的編譯時間檢查，如動態多載[解析的編譯階段檢查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)中所述。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1106">If the *primary_no_array_creation_expression* does not have compile-time type `dynamic`, then the element access undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="6aeee-1107">如果*element_access*的*primary_no_array_creation_expression*是*array_type*的值，則*element_access*是陣列存取（[陣列存取](expressions.md#array-access)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1107">If the *primary_no_array_creation_expression* of an *element_access* is a value of an *array_type*, the *element_access* is an array access ([Array access](expressions.md#array-access)).</span></span> <span data-ttu-id="6aeee-1108">否則， *primary_no_array_creation_expression*必須是具有一或多個索引子成員之類別、結構或介面類別型的變數或值，在此情況下， *element_access*是索引子存取（[索引子存取](expressions.md#indexer-access)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1108">Otherwise, the *primary_no_array_creation_expression* must be a variable or value of a class, struct, or interface type that has one or more indexer members, in which case the *element_access* is an indexer access ([Indexer access](expressions.md#indexer-access)).</span></span>

#### <a name="array-access"></a><span data-ttu-id="6aeee-1109">陣列存取</span><span class="sxs-lookup"><span data-stu-id="6aeee-1109">Array access</span></span>

<span data-ttu-id="6aeee-1110">針對陣列存取， *element_access*的*primary_no_array_creation_expression*必須是*array_type*的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1110">For an array access, the *primary_no_array_creation_expression* of the *element_access* must be a value of an *array_type*.</span></span> <span data-ttu-id="6aeee-1111">此外，陣列存取的*argument_list*不允許包含具名引數。*Argument_list*中的運算式數目必須與*array_type*的次序相同，而且每個運算式的類型都必須是 `int`、`uint`、`long`、`ulong`，或是必須隱含地轉換成一或多個這些類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1111">Furthermore, the *argument_list* of an array access is not allowed to contain named arguments.The number of expressions in the *argument_list* must be the same as the rank of the *array_type*, and each expression must be of type `int`, `uint`, `long`, `ulong`, or must be implicitly convertible to one or more of these types.</span></span>

<span data-ttu-id="6aeee-1112">評估陣列存取的結果是陣列之元素類型的變數，也就是由*argument_list*中的運算式值所選取的陣列元素。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1112">The result of evaluating an array access is a variable of the element type of the array, namely the array element selected by the value(s) of the expression(s) in the *argument_list*.</span></span>

<span data-ttu-id="6aeee-1113">@No__t-0 格式之陣列存取的執行時間處理，其中 `P` 是*array_type*的*primary_no_array_creation_expression* ，而 `A` 是*argument_list*，其中包含下列步驟：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1113">The run-time processing of an array access of the form `P[A]`, where `P` is a *primary_no_array_creation_expression* of an *array_type* and `A` is an *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="6aeee-1114">`P` 會進行評估。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1114">`P` is evaluated.</span></span> <span data-ttu-id="6aeee-1115">如果此評估導致例外狀況，則不會執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1115">If this evaluation causes an exception, no further steps are executed.</span></span>
*  <span data-ttu-id="6aeee-1116">*Argument_list*的索引運算式會依序從左至右進行評估。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1116">The index expressions of the *argument_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="6aeee-1117">遵循每個索引運算式的評估，會執行隱含轉換（[隱含](conversions.md#implicit-conversions)轉換）為下列其中一種類型： `int`，`uint`，`long`，`ulong`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1117">Following evaluation of each index expression, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to one of the following types is performed: `int`, `uint`, `long`, `ulong`.</span></span> <span data-ttu-id="6aeee-1118">選擇此清單中的第一個類型，其隱含轉換存在。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1118">The first type in this list for which an implicit conversion exists is chosen.</span></span> <span data-ttu-id="6aeee-1119">例如，如果索引運算式的類型 `short`，則會執行隱含轉換成 `int`，因為從 `short` 隱含轉換到 `int`，以及從 `short` 到 `long` 都是可行的。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1119">For instance, if the index expression is of type `short` then an implicit conversion to `int` is performed, since implicit conversions from `short` to `int` and from `short` to `long` are possible.</span></span> <span data-ttu-id="6aeee-1120">如果索引運算式的評估或後續的隱含轉換造成例外狀況，則不會評估進一步的索引運算式，也不會執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1120">If evaluation of an index expression or the subsequent implicit conversion causes an exception, then no further index expressions are evaluated and no further steps are executed.</span></span>
*  <span data-ttu-id="6aeee-1121">已檢查 `P` 的值是否有效。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1121">The value of `P` is checked to be valid.</span></span> <span data-ttu-id="6aeee-1122">如果 `P` 的值為 `null`，則會擲回 `System.NullReferenceException`，且不會執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1122">If the value of `P` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="6aeee-1123">*Argument_list*中每個運算式的值會根據 `P` 所參考陣列實例的每個維度的實際界限來進行檢查。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1123">The value of each expression in the *argument_list* is checked against the actual bounds of each dimension of the array instance referenced by `P`.</span></span> <span data-ttu-id="6aeee-1124">如果一個或多個值超出範圍，則會擲回 `System.IndexOutOfRangeException`，而且不會執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1124">If one or more values are out of range, a `System.IndexOutOfRangeException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="6aeee-1125">索引運算式所指定之陣列元素的位置會計算出來，而此位置會成為陣列存取的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1125">The location of the array element given by the index expression(s) is computed, and this location becomes the result of the array access.</span></span>

#### <a name="indexer-access"></a><span data-ttu-id="6aeee-1126">索引子存取</span><span class="sxs-lookup"><span data-stu-id="6aeee-1126">Indexer access</span></span>

<span data-ttu-id="6aeee-1127">針對索引子存取， *element_access*的*primary_no_array_creation_expression*必須是類別、結構或介面類別型的變數或值，而且此類型必須執行一或多個適用于的索引子*element_access*的*argument_list* 。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1127">For an indexer access, the *primary_no_array_creation_expression* of the *element_access* must be a variable or value of a class, struct, or interface type, and this type must implement one or more indexers that are applicable with respect to the *argument_list* of the *element_access*.</span></span>

<span data-ttu-id="6aeee-1128">格式為的索引子存取的系結時間處理 `P[A]`，其中 `P` 是類別、結構或介面類別型的*primary_no_array_creation_expression* `T`，而 `A` 是*argument_list*，其中包含下列各項步驟</span><span class="sxs-lookup"><span data-stu-id="6aeee-1128">The binding-time processing of an indexer access of the form `P[A]`, where `P` is a *primary_no_array_creation_expression* of a class, struct, or interface type `T`, and `A` is an *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="6aeee-1129">@No__t-0 所提供的索引子集合已被結構化。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1129">The set of indexers provided by `T` is constructed.</span></span> <span data-ttu-id="6aeee-1130">此集合包含在 `T` 中宣告的所有索引子，或不是 @no__t 2 宣告的 `T` 基底類型，而且可在目前的內容中存取（[成員存取](basic-concepts.md#member-access)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1130">The set consists of all indexers declared in `T` or a base type of `T` that are not `override` declarations and are accessible in the current context ([Member access](basic-concepts.md#member-access)).</span></span>
*  <span data-ttu-id="6aeee-1131">此集合會縮減為適用且不會被其他索引子隱藏的索引子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1131">The set is reduced to those indexers that are applicable and not hidden by other indexers.</span></span> <span data-ttu-id="6aeee-1132">下列規則會套用至集合中的每個索引子 `S.I`，其中 `S` 是宣告索引子 `I` 的類型：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1132">The following rules are applied to each indexer `S.I` in the set, where `S` is the type in which the indexer `I` is declared:</span></span>
   * <span data-ttu-id="6aeee-1133">如果 `I` 不適用於 `A` （適用的函式[成員](expressions.md#applicable-function-member)），則會從集合中移除 `I`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1133">If `I` is not applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)), then `I` is removed from the set.</span></span>
   * <span data-ttu-id="6aeee-1134">如果 `I` 適用于 `A` （適用的函式[成員](expressions.md#applicable-function-member)），則會從集合中移除 `S` 的基底類型中宣告的所有索引子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1134">If `I` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)), then all indexers declared in a base type of `S` are removed from the set.</span></span>
   * <span data-ttu-id="6aeee-1135">如果 `I` 適用于 `A` （適用的函式[成員](expressions.md#applicable-function-member)），而 `S` 是 `object` 以外的類別類型，則會從集合中移除在介面中宣告的所有索引子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1135">If `I` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)) and `S` is a class type other than `object`, all indexers declared in an interface are removed from the set.</span></span>
*  <span data-ttu-id="6aeee-1136">如果候選索引子的結果集是空的，則不會有適用的索引子存在，而且會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1136">If the resulting set of candidate indexers is empty, then no applicable indexers exist, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="6aeee-1137">候選索引子集合的最佳索引子是使用多載[解析](expressions.md#overload-resolution)的多載解析規則來識別。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1137">The best indexer of the set of candidate indexers is identified using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="6aeee-1138">如果無法識別單一的最佳索引子，則索引子存取是不明確的，而且會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1138">If a single best indexer cannot be identified, the indexer access is ambiguous, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="6aeee-1139">*Argument_list*的索引運算式會依序從左至右進行評估。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1139">The index expressions of the *argument_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="6aeee-1140">處理索引子存取的結果是分類為索引子存取的運算式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1140">The result of processing the indexer access is an expression classified as an indexer access.</span></span> <span data-ttu-id="6aeee-1141">索引子存取運算式會參考上述步驟中所決定的索引子，並具有 `P` 的相關聯實例運算式，以及 `A` 的相關聯引數清單。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1141">The indexer access expression references the indexer determined in the step above, and has an associated instance expression of `P` and an associated argument list of `A`.</span></span>

<span data-ttu-id="6aeee-1142">視使用的內容而定，索引子存取會導致*get 存取*子或索引子的*set 存取*子的調用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1142">Depending on the context in which it is used, an indexer access causes invocation of either the *get accessor* or the *set accessor* of the indexer.</span></span> <span data-ttu-id="6aeee-1143">如果索引子存取是指派的目標，則會叫用*set 存取*子來指派新的值（[簡單指派](expressions.md#simple-assignment)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1143">If the indexer access is the target of an assignment, the *set accessor* is invoked to assign a new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="6aeee-1144">在所有其他情況下，會叫用*get 存取*子來取得目前的值（[運算式的值](expressions.md#values-of-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1144">In all other cases, the *get accessor* is invoked to obtain the current value ([Values of expressions](expressions.md#values-of-expressions)).</span></span>

### <a name="this-access"></a><span data-ttu-id="6aeee-1145">此存取權</span><span class="sxs-lookup"><span data-stu-id="6aeee-1145">This access</span></span>

<span data-ttu-id="6aeee-1146">*This_access*是由保留字 `this` 所組成。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1146">A *this_access* consists of the reserved word `this`.</span></span>

```antlr
this_access
    : 'this'
    ;
```

<span data-ttu-id="6aeee-1147">只有在實例的函式、實例方法或實例存取子的*區塊*中，才允許*this_access* 。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1147">A *this_access* is permitted only in the *block* of an instance constructor, an instance method, or an instance accessor.</span></span> <span data-ttu-id="6aeee-1148">它具有下列其中一種意義：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1148">It has one of the following meanings:</span></span>

*  <span data-ttu-id="6aeee-1149">當 `this` 用於類別之實例函式的*primary_expression*內時，它會分類為值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1149">When `this` is used in a *primary_expression* within an instance constructor of a class, it is classified as a value.</span></span> <span data-ttu-id="6aeee-1150">值的類型是發生使用之類別的實例類型（[實例類型](classes.md#the-instance-type)），而值則是所要建立之物件的參考。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1150">The type of the value is the instance type ([The instance type](classes.md#the-instance-type)) of the class within which the usage occurs, and the value is a reference to the object being constructed.</span></span>
*  <span data-ttu-id="6aeee-1151">當 `this` 用於類別之實例方法或實例存取子中的*primary_expression*時，它會分類為值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1151">When `this` is used in a *primary_expression* within an instance method or instance accessor of a class, it is classified as a value.</span></span> <span data-ttu-id="6aeee-1152">值的類型是發生使用之類別的實例類型（[實例類型](classes.md#the-instance-type)），而值則是對其叫用方法或存取子之物件的參考。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1152">The type of the value is the instance type ([The instance type](classes.md#the-instance-type)) of the class within which the usage occurs, and the value is a reference to the object for which the method or accessor was invoked.</span></span>
*  <span data-ttu-id="6aeee-1153">當 `this` 用於結構之實例函式中的*primary_expression*時，它會分類為變數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1153">When `this` is used in a *primary_expression* within an instance constructor of a struct, it is classified as a variable.</span></span> <span data-ttu-id="6aeee-1154">變數的類型是發生使用之結構的實例類型（[實例類型](classes.md#the-instance-type)），而變數則代表所要建立的結構。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1154">The type of the variable is the instance type ([The instance type](classes.md#the-instance-type)) of the struct within which the usage occurs, and the variable represents the struct being constructed.</span></span> <span data-ttu-id="6aeee-1155">結構之實例函式的 `this` 變數的行為與結構類型的 @no__t 1 參數完全相同，特別是這表示變數必須在實例的每個執行路徑中明確指派。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1155">The `this` variable of an instance constructor of a struct behaves exactly the same as an `out` parameter of the struct type—in particular, this means that the variable must be definitely assigned in every execution path of the instance constructor.</span></span>
*  <span data-ttu-id="6aeee-1156">當 `this` 用於結構之實例方法或實例存取子中的*primary_expression*時，它會分類為變數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1156">When `this` is used in a *primary_expression* within an instance method or instance accessor of a struct, it is classified as a variable.</span></span> <span data-ttu-id="6aeee-1157">變數的類型是發生使用之結構的實例類型（[實例類型](classes.md#the-instance-type)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1157">The type of the variable is the instance type ([The instance type](classes.md#the-instance-type)) of the struct within which the usage occurs.</span></span>
   * <span data-ttu-id="6aeee-1158">如果方法或存取子不是反覆運算器（[iterator），](classes.md#iterators)則 @no__t 1 變數代表已叫用方法或存取子的結構，且其行為與結構類型的 `ref` 參數完全相同。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1158">If the method or accessor is not an iterator ([Iterators](classes.md#iterators)), the `this` variable represents the struct for which the method or accessor was invoked, and behaves exactly the same as a `ref` parameter of the struct type.</span></span>
   * <span data-ttu-id="6aeee-1159">如果方法或存取子是反覆運算器，則 `this` 變數代表已叫用方法或存取子的結構複本，其行為與結構類型的值參數完全相同。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1159">If the method or accessor is an iterator, the `this` variable represents a copy of the struct for which the method or accessor was invoked, and behaves exactly the same as a value parameter of the struct type.</span></span>

<span data-ttu-id="6aeee-1160">在上述內容中的*primary_expression*中使用 `this`，這是編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1160">Use of `this` in a *primary_expression* in a context other than the ones listed above is a compile-time error.</span></span> <span data-ttu-id="6aeee-1161">特別的是，在靜態方法、靜態屬性存取子或欄位宣告的*variable_initializer*中，不可能參考 `this`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1161">In particular, it is not possible to refer to `this` in a static method, a static property accessor, or in a *variable_initializer* of a field declaration.</span></span>

### <a name="base-access"></a><span data-ttu-id="6aeee-1162">基底存取</span><span class="sxs-lookup"><span data-stu-id="6aeee-1162">Base access</span></span>

<span data-ttu-id="6aeee-1163">*Base_access*包含保留字 `base`，後面接著 "`.`" 權杖和識別碼或以方括弧括住的*argument_list* ：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1163">A *base_access* consists of the reserved word `base` followed by either a "`.`" token and an identifier or an *argument_list* enclosed in square brackets:</span></span>

```antlr
base_access
    : 'base' '.' identifier
    | 'base' '[' expression_list ']'
    ;
```

<span data-ttu-id="6aeee-1164">*Base_access*是用來存取在目前類別或結構中以類似方式命名的成員所隱藏的基類成員。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1164">A *base_access* is used to access base class members that are hidden by similarly named members in the current class or struct.</span></span> <span data-ttu-id="6aeee-1165">只有在實例的函式、實例方法或實例存取子的*區塊*中，才允許*base_access* 。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1165">A *base_access* is permitted only in the *block* of an instance constructor, an instance method, or an instance accessor.</span></span> <span data-ttu-id="6aeee-1166">當 `base.I` 發生在類別或結構中時，`I` 必須代表該類別或結構之基類的成員。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1166">When `base.I` occurs in a class or struct, `I` must denote a member of the base class of that class or struct.</span></span> <span data-ttu-id="6aeee-1167">同樣地，當類別中出現 `base[E]` 時，適用的索引子必須存在於基類中。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1167">Likewise, when `base[E]` occurs in a class, an applicable indexer must exist in the base class.</span></span>

<span data-ttu-id="6aeee-1168">在系結階段，`base.I` 和 `base[E]` 格式的*base_access*運算式，其評估方式就如同撰寫 `((B)this).I` 和 `((B)this)[E]`，其中 `B` 是結構發生所在之類別或結構的基類（base class）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1168">At binding-time, *base_access* expressions of the form `base.I` and `base[E]` are evaluated exactly as if they were written `((B)this).I` and `((B)this)[E]`, where `B` is the base class of the class or struct in which the construct occurs.</span></span> <span data-ttu-id="6aeee-1169">因此，`base.I` 和 `base[E]` 對應至 `this.I` 和 `this[E]`，但 `this` 會視為基類的實例。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1169">Thus, `base.I` and `base[E]` correspond to `this.I` and `this[E]`, except `this` is viewed as an instance of the base class.</span></span>

<span data-ttu-id="6aeee-1170">當*base_access*參考虛擬函式成員（方法、屬性或索引子）時，會在執行時間（動態多載[解析的編譯階段檢查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)）中判斷要叫用的函式成員。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1170">When a *base_access* references a virtual function member (a method, property, or indexer), the determination of which function member to invoke at run-time ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is changed.</span></span> <span data-ttu-id="6aeee-1171">所叫用的函式成員，是藉由尋找與 `B` 相關之函式成員的最高衍生實（[虛擬方法](classes.md#virtual-methods)）來決定（而不是針對 `this` 的執行時間類型，如同平常在非基底存取中）.</span><span class="sxs-lookup"><span data-stu-id="6aeee-1171">The function member that is invoked is determined by finding the most derived implementation ([Virtual methods](classes.md#virtual-methods)) of the function member with respect to `B` (instead of with respect to the run-time type of `this`, as would be usual in a non-base access).</span></span> <span data-ttu-id="6aeee-1172">因此，在 @no__t 1 函式成員的 @no__t 0 內， *base_access*可以用來叫用函式成員的繼承函數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1172">Thus, within an `override` of a `virtual` function member, a *base_access* can be used to invoke the inherited implementation of the function member.</span></span> <span data-ttu-id="6aeee-1173">如果*base_access*所參考的函式成員是抽象的，則會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1173">If the function member referenced by a *base_access* is abstract, a binding-time error occurs.</span></span>

### <a name="postfix-increment-and-decrement-operators"></a><span data-ttu-id="6aeee-1174">後置遞增和遞減運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-1174">Postfix increment and decrement operators</span></span>

```antlr
post_increment_expression
    : primary_expression '++'
    ;

post_decrement_expression
    : primary_expression '--'
    ;
```

<span data-ttu-id="6aeee-1175">後置遞增或遞減運算的運算元必須是分類為變數、屬性存取或索引子存取的運算式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1175">The operand of a postfix increment or decrement operation must be an expression classified as a variable, a property access, or an indexer access.</span></span> <span data-ttu-id="6aeee-1176">運算的結果是與運算元相同類型的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1176">The result of the operation is a value of the same type as the operand.</span></span>

<span data-ttu-id="6aeee-1177">如果*primary_expression*的編譯時期類型 `dynamic`，則運算子會動態繫結（[動態](expressions.md#dynamic-binding)系結）， *post_increment_expression*或*post_decrement_expression*的編譯時間類型 `dynamic`和下列規則會在執行時間使用*primary_expression*的執行時間類型來套用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1177">If the *primary_expression* has the compile-time type `dynamic` then the operator is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), the *post_increment_expression* or *post_decrement_expression* has the compile-time type `dynamic` and the following rules are applied at run-time using the run-time type of the *primary_expression*.</span></span>

<span data-ttu-id="6aeee-1178">如果後置遞增或遞減運算的運算元是屬性或索引子存取，則屬性或索引子必須同時具有 `get` 和 @no__t 1 存取子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1178">If the operand of a postfix increment or decrement operation is a property or indexer access, the property or indexer must have both a `get` and a `set` accessor.</span></span> <span data-ttu-id="6aeee-1179">如果不是這種情況，就會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1179">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="6aeee-1180">一元運算子多載解析（[一元運算子](expressions.md#unary-operator-overload-resolution)多載解析）適用于選取特定的運算子執行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1180">Unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="6aeee-1181">預先定義的 `++` 和 @no__t 1 運算子存在於下列類型： `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`--`0、1、2、3 和任何列舉類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1181">Predefined `++` and `--` operators exist for the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, and any enum type.</span></span> <span data-ttu-id="6aeee-1182">預先定義的 `++` 運算子會傳回將1加入運算元所產生的值，而預先定義的 @no__t 1 運算子會傳回從運算元減去1所產生的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1182">The predefined `++` operators return the value produced by adding 1 to the operand, and the predefined `--` operators return the value produced by subtracting 1 from the operand.</span></span> <span data-ttu-id="6aeee-1183">在 @no__t 0 內容中，如果此加法或減法的結果超出結果類型的範圍，而結果類型是整數類型或列舉類型，則會擲回 `System.OverflowException`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1183">In a `checked` context, if the result of this addition or subtraction is outside the range of the result type and the result type is an integral type or enum type, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="6aeee-1184">@No__t-0 或 `x--` 的後置遞增或遞減運算的執行時間處理包含下列步驟：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1184">The run-time processing of a postfix increment or decrement operation of the form `x++` or `x--` consists of the following steps:</span></span>

*   <span data-ttu-id="6aeee-1185">如果 `x` 分類為變數：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1185">If `x` is classified as a variable:</span></span>
    * <span data-ttu-id="6aeee-1186">`x` 會進行評估以產生變數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1186">`x` is evaluated to produce the variable.</span></span>
    * <span data-ttu-id="6aeee-1187">會儲存 `x` 的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1187">The value of `x` is saved.</span></span>
    * <span data-ttu-id="6aeee-1188">會叫用選取的運算子，並將 `x` 的儲存值當做其引數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1188">The selected operator is invoked with the saved value of `x` as its argument.</span></span>
    * <span data-ttu-id="6aeee-1189">運算子所傳回的值會儲存在評估 `x` 所指定的位置。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1189">The value returned by the operator is stored in the location given by the evaluation of `x`.</span></span>
    * <span data-ttu-id="6aeee-1190">儲存的值 `x` 會成為運算的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1190">The saved value of `x` becomes the result of the operation.</span></span>
*   <span data-ttu-id="6aeee-1191">如果 `x` 已分類為屬性或索引子存取：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1191">If `x` is classified as a property or indexer access:</span></span>
    * <span data-ttu-id="6aeee-1192">實例運算式（如果 `x` 不是 `static`），而且會評估與 `x` 相關聯的引數清單（如果 `x` 是索引子存取），而結果會用於後續的 `get` 和 `set` 存取子調用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1192">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `get` and `set` accessor invocations.</span></span>
    * <span data-ttu-id="6aeee-1193">會叫用 `x` 的 `get` 存取子，並儲存傳回的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1193">The `get` accessor of `x` is invoked and the returned value is saved.</span></span>
    * <span data-ttu-id="6aeee-1194">會叫用選取的運算子，並將 `x` 的儲存值當做其引數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1194">The selected operator is invoked with the saved value of `x` as its argument.</span></span>
    * <span data-ttu-id="6aeee-1195">@No__t-1 的 `set` 存取子是以運算子所傳回的值做為其 `value` 引數來叫用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1195">The `set` accessor of `x` is invoked with the value returned by the operator as its `value` argument.</span></span>
    * <span data-ttu-id="6aeee-1196">儲存的值 `x` 會成為運算的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1196">The saved value of `x` becomes the result of the operation.</span></span>

<span data-ttu-id="6aeee-1197">@No__t-0 和 @no__t 1 運算子也支援前置詞標記法（[前置遞增和遞減運算子](expressions.md#prefix-increment-and-decrement-operators)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1197">The `++` and `--` operators also support prefix notation ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="6aeee-1198">一般來說，`x++` 或 `x--` 的結果是作業之前的 `x` 的值，而 `++x` 或 `--x` 的結果是作業之後的 `x` 的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1198">Typically, the result of `x++` or `x--` is the value of `x` before the operation, whereas the result of `++x` or `--x` is the value of `x` after the operation.</span></span> <span data-ttu-id="6aeee-1199">不論是哪一種情況，在作業之後，`x` 本身會有相同的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1199">In either case, `x` itself has the same value after the operation.</span></span>

<span data-ttu-id="6aeee-1200">您可以使用後置或首碼標記法來叫用 `operator ++` 或 @no__t 1 的執行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1200">An `operator ++` or `operator --` implementation can be invoked using either postfix or prefix notation.</span></span> <span data-ttu-id="6aeee-1201">這兩種標記法不可能有個別的運算子實現。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1201">It is not possible to have separate operator implementations for the two notations.</span></span>

### <a name="the-new-operator"></a><span data-ttu-id="6aeee-1202">new 運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-1202">The new operator</span></span>

<span data-ttu-id="6aeee-1203">@No__t-0 運算子可用來建立類型的新實例。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1203">The `new` operator is used to create new instances of types.</span></span>

<span data-ttu-id="6aeee-1204">有三種形式的 @no__t 0 運算式：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1204">There are three forms of `new` expressions:</span></span>

*  <span data-ttu-id="6aeee-1205">物件建立運算式是用來建立類別類型和實數值型別的新實例。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1205">Object creation expressions are used to create new instances of class types and value types.</span></span>
*  <span data-ttu-id="6aeee-1206">陣列建立運算式是用來建立陣列類型的新實例。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1206">Array creation expressions are used to create new instances of array types.</span></span>
*  <span data-ttu-id="6aeee-1207">委派建立運算式是用來建立委派類型的新實例。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1207">Delegate creation expressions are used to create new instances of delegate types.</span></span>

<span data-ttu-id="6aeee-1208">@No__t-0 運算子意指建立類型的實例，但不一定會隱含記憶體的動態配置。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1208">The `new` operator implies creation of an instance of a type, but does not necessarily imply dynamic allocation of memory.</span></span> <span data-ttu-id="6aeee-1209">特別是，實數值型別的實例不需要超過其所在變數的額外記憶體，而且當 `new` 用來建立實數值型別的實例時，不會發生動態配置。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1209">In particular, instances of value types require no additional memory beyond the variables in which they reside, and no dynamic allocations occur when `new` is used to create instances of value types.</span></span>

#### <a name="object-creation-expressions"></a><span data-ttu-id="6aeee-1210">物件建立運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-1210">Object creation expressions</span></span>

<span data-ttu-id="6aeee-1211">*Object_creation_expression*是用來建立*class_type*或*value_type*的新實例。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1211">An *object_creation_expression* is used to create a new instance of a *class_type* or a *value_type*.</span></span>

```antlr
object_creation_expression
    : 'new' type '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;

object_or_collection_initializer
    : object_initializer
    | collection_initializer
    ;
```

<span data-ttu-id="6aeee-1212">*Object_creation_expression*的*類型*必須是*class_type*、 *value_type*或*type_parameter*。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1212">The *type* of an *object_creation_expression* must be a *class_type*, a *value_type* or a *type_parameter*.</span></span> <span data-ttu-id="6aeee-1213">*類型*不可以是 `abstract` *class_type*。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1213">The *type* cannot be an `abstract` *class_type*.</span></span>

<span data-ttu-id="6aeee-1214">只有當*類型*為*class_type*或*struct_type*時，才允許選擇性的*argument_list* （[引數清單](expressions.md#argument-lists)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1214">The optional *argument_list* ([Argument lists](expressions.md#argument-lists)) is permitted only if the *type* is a *class_type* or a *struct_type*.</span></span>

<span data-ttu-id="6aeee-1215">物件建立運算式可以省略函式引數清單並括住括弧，前提是它包含物件初始化運算式或集合初始化運算式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1215">An object creation expression can omit the constructor argument list and enclosing parentheses provided it includes an object initializer or collection initializer.</span></span> <span data-ttu-id="6aeee-1216">省略函式引數清單和括住的括弧，相當於指定空的引數清單。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1216">Omitting the constructor argument list and enclosing parentheses is equivalent to specifying an empty argument list.</span></span>

<span data-ttu-id="6aeee-1217">處理包含物件初始化運算式或集合初始化運算式的物件建立運算式，是由第一次處理實例的函式，然後處理物件初始化運算式所指定的成員或專案初始化而組成（[物件初始化運算式](expressions.md#object-initializers)）或集合初始化運算式（[集合初始化運算式](expressions.md#collection-initializers)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1217">Processing of an object creation expression that includes an object initializer or collection initializer consists of first processing the instance constructor and then processing the member or element initializations specified by the object initializer ([Object initializers](expressions.md#object-initializers)) or collection initializer ([Collection initializers](expressions.md#collection-initializers)).</span></span>

<span data-ttu-id="6aeee-1218">如果選擇性*argument_list*中的任何引數具有編譯時期類型 `dynamic`，則*object_creation_expression*會動態繫結（[動態](expressions.md#dynamic-binding)系結），而下列規則會在執行時間使用執行時間套用具有編譯時間類型 `dynamic` 之*argument_list*引數的類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1218">If any of the arguments in the optional *argument_list* has the compile-time type `dynamic` then the *object_creation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) and the following rules are applied at run-time using the run-time type of those arguments of the *argument_list* that have the compile time type `dynamic`.</span></span> <span data-ttu-id="6aeee-1219">不過，建立物件時，會進行有限的編譯時間檢查，如動態多載[解析的編譯階段檢查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)中所述。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1219">However, the object creation undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="6aeee-1220">*Object_creation_expression*格式的系結時間處理 `new T(A)`，其中 `T` 是*class_type*或*value_type* ，而 `A` 是選擇性的*argument_list*，由下列步驟組成：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1220">The binding-time processing of an *object_creation_expression* of the form `new T(A)`, where `T` is a *class_type* or a *value_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*   <span data-ttu-id="6aeee-1221">如果 `T` 是*value_type* ，且 `A` 不存在：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1221">If `T` is a *value_type* and `A` is not present:</span></span>
    * <span data-ttu-id="6aeee-1222">*Object_creation_expression*是預設的函式呼叫。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1222">The *object_creation_expression* is a default constructor invocation.</span></span> <span data-ttu-id="6aeee-1223">*Object_creation_expression*的結果是 `T` 類型的值，也就是在[system.object 類型](types.md#the-systemvaluetype-type)中定義的 `T` 的預設值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1223">The result of the *object_creation_expression* is a value of type `T`, namely the default value for `T` as defined in [The System.ValueType type](types.md#the-systemvaluetype-type).</span></span>
*   <span data-ttu-id="6aeee-1224">否則，如果 `T` 是*type_parameter* ，且 `A` 不存在：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1224">Otherwise, if `T` is a *type_parameter* and `A` is not present:</span></span>
    * <span data-ttu-id="6aeee-1225">如果未為 `T` 指定實數值型別條件約束或函式條件約束（[類型參數條件約束](classes.md#type-parameter-constraints)），則會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1225">If no value type constraint or constructor constraint ([Type parameter constraints](classes.md#type-parameter-constraints)) has been specified for `T`, a binding-time error occurs.</span></span>
    * <span data-ttu-id="6aeee-1226">*Object_creation_expression*的結果是型別參數已系結的執行時間型別值，也就是叫用該型別的預設函式的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1226">The result of the *object_creation_expression* is a value of the run-time type that the type parameter has been bound to, namely the result of invoking the default constructor of that type.</span></span> <span data-ttu-id="6aeee-1227">執行時間類型可以是參考型別或實數值型別。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1227">The run-time type may be a reference type or a value type.</span></span>
*   <span data-ttu-id="6aeee-1228">否則，如果 `T` 是*class_type*或*struct_type*：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1228">Otherwise, if `T` is a *class_type* or a *struct_type*:</span></span>
    * <span data-ttu-id="6aeee-1229">如果 `T` 是 `abstract` *class_type*，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1229">If `T` is an `abstract` *class_type*, a compile-time error occurs.</span></span>
    * <span data-ttu-id="6aeee-1230">要叫用的實例函式是使用多載[解析](expressions.md#overload-resolution)的多載解析規則來決定。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1230">The instance constructor to invoke is determined using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="6aeee-1231">候選實例的集合是由 `T` 中宣告的所有可存取的實例函式所組成，這些是在 `A` （適用的函式[成員](expressions.md#applicable-function-member)）方面適用的。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1231">The set of candidate instance constructors consists of all accessible instance constructors declared in `T` which are applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="6aeee-1232">如果候選實例的集合是空的，或者無法識別單一最佳實例的函式，則會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1232">If the set of candidate instance constructors is empty, or if a single best instance constructor cannot be identified, a binding-time error occurs.</span></span>
    * <span data-ttu-id="6aeee-1233">*Object_creation_expression*的結果是 `T` 類型的值，也就是叫用上述步驟中所決定之實例的函式所產生的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1233">The result of the *object_creation_expression* is a value of type `T`, namely the value produced by invoking the instance constructor determined in the step above.</span></span>
*  <span data-ttu-id="6aeee-1234">否則， *object_creation_expression*會無效，而且會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1234">Otherwise, the *object_creation_expression* is invalid, and a binding-time error occurs.</span></span>

<span data-ttu-id="6aeee-1235">即使*object_creation_expression*是動態系結的，編譯時間類型仍然 `T`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1235">Even if the *object_creation_expression* is dynamically bound, the compile-time type is still `T`.</span></span>

<span data-ttu-id="6aeee-1236">*Object_creation_expression*表單的執行時間處理 `new T(A)`，其中 `T` 是*class_type*或*struct_type* ，而 `A` 是選擇性的*argument_list*，由下列步驟組成：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1236">The run-time processing of an *object_creation_expression* of the form `new T(A)`, where `T` is *class_type* or a *struct_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*   <span data-ttu-id="6aeee-1237">如果 `T` 是*class_type*：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1237">If `T` is a *class_type*:</span></span>
    * <span data-ttu-id="6aeee-1238">已配置類別 `T` 的新實例。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1238">A new instance of class `T` is allocated.</span></span> <span data-ttu-id="6aeee-1239">如果沒有足夠的記憶體可配置新的實例，則會擲回 `System.OutOfMemoryException`，而且不會執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1239">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="6aeee-1240">新實例的所有欄位都會初始化為其預設值（[預設值](variables.md#default-values)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1240">All fields of the new instance are initialized to their default values ([Default values](variables.md#default-values)).</span></span>
    * <span data-ttu-id="6aeee-1241">根據函式成員調用的規則叫用實例的函式（[動態多載解析的編譯時間檢查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1241">The instance constructor is invoked according to the rules of function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span> <span data-ttu-id="6aeee-1242">新配置之實例的參考會自動傳遞至實例的「函式」，而且可以從該該函數內部存取實例，如 `this`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1242">A reference to the newly allocated instance is automatically passed to the instance constructor and the instance can be accessed from within that constructor as `this`.</span></span>
*   <span data-ttu-id="6aeee-1243">如果 `T` 是*struct_type*：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1243">If `T` is a *struct_type*:</span></span>
    * <span data-ttu-id="6aeee-1244">@No__t-0 類型的實例是藉由配置暫存區域變數來建立。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1244">An instance of type `T` is created by allocating a temporary local variable.</span></span> <span data-ttu-id="6aeee-1245">由於*struct_type*的實例函式必須明確地將值指派給所建立之實例的每個欄位，因此不需要初始化暫存變數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1245">Since an instance constructor of a *struct_type* is required to definitely assign a value to each field of the instance being created, no initialization of the temporary variable is necessary.</span></span>
    * <span data-ttu-id="6aeee-1246">根據函式成員調用的規則叫用實例的函式（[動態多載解析的編譯時間檢查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1246">The instance constructor is invoked according to the rules of function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span> <span data-ttu-id="6aeee-1247">新配置之實例的參考會自動傳遞至實例的「函式」，而且可以從該該函數內部存取實例，如 `this`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1247">A reference to the newly allocated instance is automatically passed to the instance constructor and the instance can be accessed from within that constructor as `this`.</span></span>

#### <a name="object-initializers"></a><span data-ttu-id="6aeee-1248">物件初始設定式</span><span class="sxs-lookup"><span data-stu-id="6aeee-1248">Object initializers</span></span>

<span data-ttu-id="6aeee-1249">***物件初始化運算式***會指定物件的零個或多個欄位、屬性或索引元素的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1249">An ***object initializer*** specifies values for zero or more fields, properties or indexed elements of an object.</span></span>

```antlr
object_initializer
    : '{' member_initializer_list? '}'
    | '{' member_initializer_list ',' '}'
    ;

member_initializer_list
    : member_initializer (',' member_initializer)*
    ;

member_initializer
    : initializer_target '=' initializer_value
    ;

initializer_target
    : identifier
    | '[' argument_list ']'
    ;

initializer_value
    : expression
    | object_or_collection_initializer
    ;
```

<span data-ttu-id="6aeee-1250">物件初始化運算式是由成員初始化運算式的序列所組成，並以 `{` 和 @no__t 1 標記括住，並以逗號分隔。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1250">An object initializer consists of a sequence of member initializers, enclosed by `{` and `}` tokens and separated by commas.</span></span> <span data-ttu-id="6aeee-1251">每個*member_initializer*都會指定初始化的目標。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1251">Each *member_initializer* designates a target for the initialization.</span></span> <span data-ttu-id="6aeee-1252">*識別碼*必須命名要初始化之物件的可存取欄位或屬性，而以方括弧括住的*argument_list*必須在要初始化的物件上指定可存取索引子的引數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1252">An *identifier* must name an accessible field or property of the object being initialized, whereas an *argument_list* enclosed in square brackets must specify arguments for an accessible indexer on the object being initialized.</span></span> <span data-ttu-id="6aeee-1253">針對相同的欄位或屬性，物件初始化運算式包含一個以上的成員初始化運算式是錯誤的。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1253">It is an error for an object initializer to include more than one member initializer for the same field or property.</span></span>

<span data-ttu-id="6aeee-1254">每個*initializer_target*後面會加上等號，以及運算式、物件初始化運算式或集合初始化運算式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1254">Each *initializer_target* is followed by an equals sign and either an expression, an object initializer or a collection initializer.</span></span> <span data-ttu-id="6aeee-1255">物件初始化運算式中的運算式不可能參考新建立的物件。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1255">It is not possible for expressions within the object initializer to refer to the newly created object it is initializing.</span></span>

<span data-ttu-id="6aeee-1256">成員初始化運算式，可在處理等號之後，使用與目標指派（[簡單指派](expressions.md#simple-assignment)）相同的方式來指定運算式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1256">A member initializer that specifies an expression after the equals sign is processed in the same way as an assignment ([Simple assignment](expressions.md#simple-assignment)) to the target.</span></span>

<span data-ttu-id="6aeee-1257">成員初始化運算式，指定等號之後的物件初始化運算式是一個***嵌套物件初始化運算式***，也就是初始化内嵌物件。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1257">A member initializer that specifies an object initializer after the equals sign is a ***nested object initializer***, i.e. an initialization of an embedded object.</span></span> <span data-ttu-id="6aeee-1258">並不是將新值指派給欄位或屬性，而是將嵌套物件初始化運算式中的指派視為欄位或屬性成員的指派。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1258">Instead of assigning a new value to the field or property, the assignments in the nested object initializer are treated as assignments to members of the field or property.</span></span> <span data-ttu-id="6aeee-1259">無法將嵌套物件初始化運算式套用至具有實數值型別的屬性，或套用至具有實數值型別的唯讀欄位。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1259">Nested object initializers cannot be applied to properties with a value type, or to read-only fields with a value type.</span></span>

<span data-ttu-id="6aeee-1260">在等號之後指定集合初始化運算式的成員初始化運算式是內嵌集合的初始化。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1260">A member initializer that specifies a collection initializer after the equals sign is an initialization of an embedded collection.</span></span> <span data-ttu-id="6aeee-1261">不需要將新的集合指派給目標欄位、屬性或索引子，在初始化運算式中提供的專案會加入至目標所參考的集合中。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1261">Instead of assigning a new collection to the target field, property or indexer, the elements given in the initializer are added to the collection referenced by the target.</span></span> <span data-ttu-id="6aeee-1262">目標必須是符合[集合初始化運算式](expressions.md#collection-initializers)中所指定之需求的集合類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1262">The target must be of a collection type that satisfies the requirements specified in [Collection initializers](expressions.md#collection-initializers).</span></span>

<span data-ttu-id="6aeee-1263">索引初始化運算式的引數一律只會評估一次。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1263">The arguments to an index initializer will always be evaluated exactly once.</span></span> <span data-ttu-id="6aeee-1264">因此，即使引數最終不會使用（例如，因為是空的嵌套初始化運算式），也會評估它們的副作用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1264">Thus, even if the arguments end up never getting used (e.g. because of an empty nested initializer), they will be evaluated for their side effects.</span></span>

<span data-ttu-id="6aeee-1265">下列類別代表具有兩個座標的點：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1265">The following class represents a point with two coordinates:</span></span>
```csharp
public class Point
{
    int x, y;

    public int X { get { return x; } set { x = value; } }
    public int Y { get { return y; } set { y = value; } }
}
```

<span data-ttu-id="6aeee-1266">@No__t-0 的實例可以建立和初始化，如下所示：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1266">An instance of `Point` can be created and initialized as follows:</span></span>
```csharp
Point a = new Point { X = 0, Y = 1 };
```
<span data-ttu-id="6aeee-1267">其作用與</span><span class="sxs-lookup"><span data-stu-id="6aeee-1267">which has the same effect as</span></span>
```csharp
Point __a = new Point();
__a.X = 0;
__a.Y = 1; 
Point a = __a;
```
<span data-ttu-id="6aeee-1268">其中 `__a` 是其他不可見且無法存取的暫存變數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1268">where `__a` is an otherwise invisible and inaccessible temporary variable.</span></span> <span data-ttu-id="6aeee-1269">下列類別代表從兩個點建立的矩形：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1269">The following class represents a rectangle created from two points:</span></span>
```csharp
public class Rectangle
{
    Point p1, p2;

    public Point P1 { get { return p1; } set { p1 = value; } }
    public Point P2 { get { return p2; } set { p2 = value; } }
}
```

<span data-ttu-id="6aeee-1270">@No__t-0 的實例可以建立和初始化，如下所示：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1270">An instance of `Rectangle` can be created and initialized as follows:</span></span>
```csharp
Rectangle r = new Rectangle {
    P1 = new Point { X = 0, Y = 1 },
    P2 = new Point { X = 2, Y = 3 }
};
```
<span data-ttu-id="6aeee-1271">其作用與</span><span class="sxs-lookup"><span data-stu-id="6aeee-1271">which has the same effect as</span></span>
```csharp
Rectangle __r = new Rectangle();
Point __p1 = new Point();
__p1.X = 0;
__p1.Y = 1;
__r.P1 = __p1;
Point __p2 = new Point();
__p2.X = 2;
__p2.Y = 3;
__r.P2 = __p2; 
Rectangle r = __r;
```
<span data-ttu-id="6aeee-1272">其中 `__r`，`__p1` 和 `__p2` 是暫時的變數，在其他情況下不可見且無法存取。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1272">where `__r`, `__p1` and `__p2` are temporary variables that are otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="6aeee-1273">如果 `Rectangle` 的函式會配置兩個內嵌的 `Point` 個實例</span><span class="sxs-lookup"><span data-stu-id="6aeee-1273">If `Rectangle`'s constructor allocates the two embedded `Point` instances</span></span>
```csharp
public class Rectangle
{
    Point p1 = new Point();
    Point p2 = new Point();

    public Point P1 { get { return p1; } }
    public Point P2 { get { return p2; } }
}
```
<span data-ttu-id="6aeee-1274">下列結構可用來初始化內嵌的 `Point` 實例，而不是指派新的實例：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1274">the following construct can be used to initialize the embedded `Point` instances instead of assigning new instances:</span></span>
```csharp
Rectangle r = new Rectangle {
    P1 = { X = 0, Y = 1 },
    P2 = { X = 2, Y = 3 }
};
```
<span data-ttu-id="6aeee-1275">其作用與</span><span class="sxs-lookup"><span data-stu-id="6aeee-1275">which has the same effect as</span></span>
```csharp
Rectangle __r = new Rectangle();
__r.P1.X = 0;
__r.P1.Y = 1;
__r.P2.X = 2;
__r.P2.Y = 3;
Rectangle r = __r;
```

<span data-ttu-id="6aeee-1276">假設有適當的 C 定義，則下列範例：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1276">Given an appropriate definition of C, the following example:</span></span>
```csharp
var c = new C {
    x = true,
    y = { a = "Hello" },
    z = { 1, 2, 3 },
    ["x"] = 5,
    [0,0] = { "a", "b" },
    [1,2] = {}
};
```
<span data-ttu-id="6aeee-1277">相當於這一系列的指派：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1277">is equivalent to this series of assignments:</span></span>
```csharp
C __c = new C();
__c.x = true;
__c.y.a = "Hello";
__c.z.Add(1); 
__c.z.Add(2);
__c.z.Add(3);
string __i1 = "x";
__c[__i1] = 5;
int __i2 = 0, __i3 = 0;
__c[__i2,__i3].Add("a");
__c[__i2,__i3].Add("b");
int __i4 = 1, __i5 = 2;
var c = __c;
```
<span data-ttu-id="6aeee-1278">其中 `__c` 等，是產生不可見且無法存取原始程式碼的變數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1278">where `__c`, etc., are generated variables that are invisible and inaccessible to the source code.</span></span> <span data-ttu-id="6aeee-1279">請注意，`[0,0]` 的引數只會評估一次，而 `[1,2]` 的引數則會評估一次，即使從未用過它們也一樣。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1279">Note that the arguments for `[0,0]` are evaluated only once, and the arguments for `[1,2]` are evaluated once even though they are never used.</span></span>

#### <a name="collection-initializers"></a><span data-ttu-id="6aeee-1280">集合初始設定式</span><span class="sxs-lookup"><span data-stu-id="6aeee-1280">Collection initializers</span></span>

<span data-ttu-id="6aeee-1281">集合初始化運算式會指定集合的元素。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1281">A collection initializer specifies the elements of a collection.</span></span>

```antlr
collection_initializer
    : '{' element_initializer_list '}'
    | '{' element_initializer_list ',' '}'
    ;

element_initializer_list
    : element_initializer (',' element_initializer)*
    ;

element_initializer
    : non_assignment_expression
    | '{' expression_list '}'
    ;

expression_list
    : expression (',' expression)*
    ;
```

<span data-ttu-id="6aeee-1282">集合初始化運算式是由一系列的專案初始化運算式所組成，並以 `{` 和 @no__t 1 標記括住，並以逗號分隔。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1282">A collection initializer consists of a sequence of element initializers, enclosed by `{` and `}` tokens and separated by commas.</span></span> <span data-ttu-id="6aeee-1283">每個專案初始化運算式都會指定要加入至要初始化之集合物件的專案，並包含以 `{` 和 @no__t 1 token 括住，並以逗號分隔的運算式清單。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1283">Each element initializer specifies an element to be added to the collection object being initialized, and consists of a list of expressions enclosed by `{` and `}` tokens and separated by commas.</span></span>  <span data-ttu-id="6aeee-1284">您可以不使用大括弧來撰寫單一運算式專案初始化運算式，但不能是指派運算式，以避免與成員初始化運算式不明確。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1284">A single-expression element initializer can be written without braces, but cannot then be an assignment expression, to avoid ambiguity with member initializers.</span></span> <span data-ttu-id="6aeee-1285">*Non_assignment_expression*生產環境是在[expression](expressions.md#expression)中定義。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1285">The *non_assignment_expression* production is defined in [Expression](expressions.md#expression).</span></span>

<span data-ttu-id="6aeee-1286">以下是包含集合初始化運算式的物件建立運算式範例：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1286">The following is an example of an object creation expression that includes a collection initializer:</span></span>
```csharp
List<int> digits = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
```

<span data-ttu-id="6aeee-1287">套用集合初始化運算式的集合物件，必須是實 `System.Collections.IEnumerable` 的類型，否則會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1287">The collection object to which a collection initializer is applied must be of a type that implements `System.Collections.IEnumerable` or a compile-time error occurs.</span></span> <span data-ttu-id="6aeee-1288">對於順序中的每個指定元素，集合初始化運算式會在目標物件上叫用 `Add` 方法，其中包含專案初始化運算式的運算式清單做為引數清單，並針對每個調用套用一般成員查閱和多載解析。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1288">For each specified element in order, the collection initializer invokes an `Add` method on the target object with the expression list of the element initializer as argument list, applying normal member lookup and overload resolution for each invocation.</span></span> <span data-ttu-id="6aeee-1289">因此，集合物件必須有適用的實例或擴充方法，且每個專案初始化運算式的名稱 `Add`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1289">Thus, the collection object must have an applicable instance or extension method with the name `Add` for each element initializer.</span></span>

<span data-ttu-id="6aeee-1290">下列類別代表連絡人，其中包含名稱和電話號碼清單：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1290">The following class represents a contact with a name and a list of phone numbers:</span></span>
```csharp
public class Contact
{
    string name;
    List<string> phoneNumbers = new List<string>();

    public string Name { get { return name; } set { name = value; } }

    public List<string> PhoneNumbers { get { return phoneNumbers; } }
}
```

<span data-ttu-id="6aeee-1291">@No__t-0 可以建立和初始化，如下所示：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1291">A `List<Contact>` can be created and initialized as follows:</span></span>
```csharp
var contacts = new List<Contact> {
    new Contact {
        Name = "Chris Smith",
        PhoneNumbers = { "206-555-0101", "425-882-8080" }
    },
    new Contact {
        Name = "Bob Harris",
        PhoneNumbers = { "650-555-0199" }
    }
};
```
<span data-ttu-id="6aeee-1292">其作用與</span><span class="sxs-lookup"><span data-stu-id="6aeee-1292">which has the same effect as</span></span>
```csharp
var __clist = new List<Contact>();
Contact __c1 = new Contact();
__c1.Name = "Chris Smith";
__c1.PhoneNumbers.Add("206-555-0101");
__c1.PhoneNumbers.Add("425-882-8080");
__clist.Add(__c1);
Contact __c2 = new Contact();
__c2.Name = "Bob Harris";
__c2.PhoneNumbers.Add("650-555-0199");
__clist.Add(__c2);
var contacts = __clist;
```
<span data-ttu-id="6aeee-1293">其中 `__clist`，`__c1` 和 `__c2` 是暫時的變數，在其他情況下不可見且無法存取。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1293">where `__clist`, `__c1` and `__c2` are temporary variables that are otherwise invisible and inaccessible.</span></span>

#### <a name="array-creation-expressions"></a><span data-ttu-id="6aeee-1294">陣列建立運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-1294">Array creation expressions</span></span>

<span data-ttu-id="6aeee-1295">*Array_creation_expression*是用來建立*array_type*的新實例。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1295">An *array_creation_expression* is used to create a new instance of an *array_type*.</span></span>

```antlr
array_creation_expression
    : 'new' non_array_type '[' expression_list ']' rank_specifier* array_initializer?
    | 'new' array_type array_initializer
    | 'new' rank_specifier array_initializer
    ;
```

<span data-ttu-id="6aeee-1296">第一個表單的陣列建立運算式會配置從運算式清單中刪除每個個別運算式所產生之類型的陣列實例。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1296">An array creation expression of the first form allocates an array instance of the type that results from deleting each of the individual expressions from the expression list.</span></span> <span data-ttu-id="6aeee-1297">例如，陣列建立運算式 `new int[10,20]` 會產生類型的陣列實例 `int[,]`，而陣列建立運算式 `new int[10][,]` 則會產生類型為 `int[][,]` 的陣列。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1297">For example, the array creation expression `new int[10,20]` produces an array instance of type `int[,]`, and the array creation expression `new int[10][,]` produces an array of type `int[][,]`.</span></span> <span data-ttu-id="6aeee-1298">運算式清單中的每個運算式都必須是 `int`、`uint`、`long` 或 `ulong` 的類型，或可隱含轉換成這些類型的一或多個。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1298">Each expression in the expression list must be of type `int`, `uint`, `long`, or `ulong`, or implicitly convertible to one or more of these types.</span></span> <span data-ttu-id="6aeee-1299">每個運算式的值會決定新配置之陣列實例中對應維度的長度。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1299">The value of each expression determines the length of the corresponding dimension in the newly allocated array instance.</span></span> <span data-ttu-id="6aeee-1300">因為陣列維度的長度必須是非負值，所以在運算式清單中有一個*constant_expression*具有負值的編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1300">Since the length of an array dimension must be nonnegative, it is a compile-time error to have a *constant_expression* with a negative value in the expression list.</span></span>

<span data-ttu-id="6aeee-1301">除非在不安全的內容（[unsafe](unsafe-code.md#unsafe-contexts)內容）中，否則不會指定陣列的版面配置。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1301">Except in an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)), the layout of arrays is unspecified.</span></span>

<span data-ttu-id="6aeee-1302">如果第一個表單的陣列建立運算式包含陣列初始化運算式，則運算式清單中的每個運算式都必須是常數，而且運算式清單所指定的次序和維度長度必須符合陣列初始化運算式的順序。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1302">If an array creation expression of the first form includes an array initializer, each expression in the expression list must be a constant and the rank and dimension lengths specified by the expression list must match those of the array initializer.</span></span>

<span data-ttu-id="6aeee-1303">在第二個或第三個表單的陣列建立運算式中，指定陣列類型或次序規範的順位必須符合陣列初始化運算式的次序。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1303">In an array creation expression of the second or third form, the rank of the specified array type or rank specifier must match that of the array initializer.</span></span> <span data-ttu-id="6aeee-1304">個別的維度長度是從陣列初始化運算式的每個對應嵌套層級中的專案數推斷而來。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1304">The individual dimension lengths are inferred from the number of elements in each of the corresponding nesting levels of the array initializer.</span></span> <span data-ttu-id="6aeee-1305">因此，運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-1305">Thus, the expression</span></span>
```csharp
new int[,] {{0, 1}, {2, 3}, {4, 5}}
```
<span data-ttu-id="6aeee-1306">完全對應至</span><span class="sxs-lookup"><span data-stu-id="6aeee-1306">exactly corresponds to</span></span>
```csharp
new int[3, 2] {{0, 1}, {2, 3}, {4, 5}}
```

<span data-ttu-id="6aeee-1307">第三個表單的陣列建立運算式稱為***隱含類型陣列建立運算式***。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1307">An array creation expression of the third form is referred to as an ***implicitly typed array creation expression***.</span></span> <span data-ttu-id="6aeee-1308">它類似于第二個表單，不同之處在于陣列的元素類型並未明確指定，而是判斷為數組初始化運算式中一組運算式的最佳一般類型（[尋找一組運算式的最佳一般類型](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1308">It is similar to the second form, except that the element type of the array is not explicitly given, but determined as the best common type ([Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) of the set of expressions in the array initializer.</span></span> <span data-ttu-id="6aeee-1309">若是多維陣列（也就是其中一個*rank_specifier*至少包含一個逗號），這個集合會由在 nested *array_initializer*s 中找到的所有*運算式*組成。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1309">For a multidimensional array, i.e., one where the *rank_specifier* contains at least one comma, this set comprises all *expression*s found in nested *array_initializer*s.</span></span>

<span data-ttu-id="6aeee-1310">陣列初始化運算式中的進一步[說明。](arrays.md#array-initializers)</span><span class="sxs-lookup"><span data-stu-id="6aeee-1310">Array initializers are described further in [Array initializers](arrays.md#array-initializers).</span></span>

<span data-ttu-id="6aeee-1311">評估陣列建立運算式的結果會分類為值，也就是新配置陣列實例的參考。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1311">The result of evaluating an array creation expression is classified as a value, namely a reference to the newly allocated array instance.</span></span> <span data-ttu-id="6aeee-1312">陣列建立運算式的執行時間處理是由下列步驟所組成：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1312">The run-time processing of an array creation expression consists of the following steps:</span></span>

*  <span data-ttu-id="6aeee-1313">*Expression_list*的維度長度運算式會依序從左至右評估。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1313">The dimension length expressions of the *expression_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="6aeee-1314">評估每個運算式之後，會執行隱含轉換（[隱含](conversions.md#implicit-conversions)轉換）為下列其中一種類型： `int`，`uint`，`long`，`ulong`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1314">Following evaluation of each expression, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to one of the following types is performed: `int`, `uint`, `long`, `ulong`.</span></span> <span data-ttu-id="6aeee-1315">選擇此清單中的第一個類型，其隱含轉換存在。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1315">The first type in this list for which an implicit conversion exists is chosen.</span></span> <span data-ttu-id="6aeee-1316">如果運算式的評估或後續的隱含轉換造成例外狀況，則不會評估進一步的運算式，也不會執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1316">If evaluation of an expression or the subsequent implicit conversion causes an exception, then no further expressions are evaluated and no further steps are executed.</span></span>
*  <span data-ttu-id="6aeee-1317">維度長度的計算值會依照下列方式進行驗證。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1317">The computed values for the dimension lengths are validated as follows.</span></span> <span data-ttu-id="6aeee-1318">如果一或多個值小於零，則會擲回 `System.OverflowException`，而且不會執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1318">If one or more of the values are less than zero, a `System.OverflowException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="6aeee-1319">系統會配置具有給定維度長度的陣列實例。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1319">An array instance with the given dimension lengths is allocated.</span></span> <span data-ttu-id="6aeee-1320">如果沒有足夠的記憶體可配置新的實例，則會擲回 `System.OutOfMemoryException`，而且不會執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1320">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="6aeee-1321">新陣列實例的所有元素都會初始化為其預設值（[預設值](variables.md#default-values)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1321">All elements of the new array instance are initialized to their default values ([Default values](variables.md#default-values)).</span></span>
*  <span data-ttu-id="6aeee-1322">如果陣列建立運算式包含陣列初始化運算式，則會評估陣列初始化運算式中的每個運算式，並將其指派給其對應的陣列元素。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1322">If the array creation expression contains an array initializer, then each expression in the array initializer is evaluated and assigned to its corresponding array element.</span></span> <span data-ttu-id="6aeee-1323">評估和指派會依照運算式在陣列初始化運算式中撰寫的順序來執行，換言之，專案會以遞增的索引順序初始化，而最右邊的維度會先遞增。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1323">The evaluations and assignments are performed in the order the expressions are written in the array initializer—in other words, elements are initialized in increasing index order, with the rightmost dimension increasing first.</span></span> <span data-ttu-id="6aeee-1324">如果給定運算式的評估或後續的對應陣列元素指派導致例外狀況，則不會初始化進一步的元素（因此其餘的專案將會有其預設值）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1324">If evaluation of a given expression or the subsequent assignment to the corresponding array element causes an exception, then no further elements are initialized (and the remaining elements will thus have their default values).</span></span>

<span data-ttu-id="6aeee-1325">陣列建立運算式允許以陣列類型的元素具現化陣列，但這類陣列的元素必須手動初始化。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1325">An array creation expression permits instantiation of an array with elements of an array type, but the elements of such an array must be manually initialized.</span></span> <span data-ttu-id="6aeee-1326">例如，語句</span><span class="sxs-lookup"><span data-stu-id="6aeee-1326">For example, the statement</span></span>
```csharp
int[][] a = new int[100][];
```
<span data-ttu-id="6aeee-1327">建立具有100元素類型 `int[]` 的一維陣列。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1327">creates a single-dimensional array with 100 elements of type `int[]`.</span></span> <span data-ttu-id="6aeee-1328">每個元素的初始值為 `null`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1328">The initial value of each element is `null`.</span></span> <span data-ttu-id="6aeee-1329">相同的陣列建立運算式不可能同時具現化子陣列和語句</span><span class="sxs-lookup"><span data-stu-id="6aeee-1329">It is not possible for the same array creation expression to also instantiate the sub-arrays, and the statement</span></span>
```csharp
int[][] a = new int[100][5];        // Error
```
<span data-ttu-id="6aeee-1330">會導致編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1330">results in a compile-time error.</span></span> <span data-ttu-id="6aeee-1331">子陣列的具現化必須改為手動執行，如下所示：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1331">Instantiation of the sub-arrays must instead be performed manually, as in</span></span>
```csharp
int[][] a = new int[100][];
for (int i = 0; i < 100; i++) a[i] = new int[5];
```

<span data-ttu-id="6aeee-1332">當陣列的陣列具有「矩形」圖形時，也就是當子陣列的長度都相同時，使用多維度陣列會更有效率。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1332">When an array of arrays has a "rectangular" shape, that is when the sub-arrays are all of the same length, it is more efficient to use a multi-dimensional array.</span></span> <span data-ttu-id="6aeee-1333">在上述範例中，陣列陣列的具現化會建立101物件，一個是外部陣列和100個子陣列。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1333">In the example above, instantiation of the array of arrays creates 101 objects—one outer array and 100 sub-arrays.</span></span> <span data-ttu-id="6aeee-1334">相反地，</span><span class="sxs-lookup"><span data-stu-id="6aeee-1334">In contrast,</span></span>
```csharp
int[,] = new int[100, 5];
```
<span data-ttu-id="6aeee-1335">只建立單一物件（二維陣列），並在單一語句中完成配置。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1335">creates only a single object, a two-dimensional array, and accomplishes the allocation in a single statement.</span></span>

<span data-ttu-id="6aeee-1336">以下是隱含類型陣列建立運算式的範例：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1336">The following are examples of implicitly typed array creation expressions:</span></span>
```csharp
var a = new[] { 1, 10, 100, 1000 };                       // int[]

var b = new[] { 1, 1.5, 2, 2.5 };                         // double[]

var c = new[,] { { "hello", null }, { "world", "!" } };   // string[,]

var d = new[] { 1, "one", 2, "two" };                     // Error
```

<span data-ttu-id="6aeee-1337">最後一個運算式會造成編譯時期錯誤，因為 `int` 或 `string` 不會隱含地轉換成另一個，所以沒有最佳的一般類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1337">The last expression causes a compile-time error because neither `int` nor `string` is implicitly convertible to the other, and so there is no best common type.</span></span> <span data-ttu-id="6aeee-1338">在此情況下，必須使用明確類型的陣列建立運算式，例如，指定要 `object[]` 的類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1338">An explicitly typed array creation expression must be used in this case, for example specifying the type to be `object[]`.</span></span> <span data-ttu-id="6aeee-1339">或者，其中一個元素可以轉換成一般基底類型，然後再成為推斷的元素類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1339">Alternatively, one of the elements can be cast to a common base type, which would then become the inferred element type.</span></span>

<span data-ttu-id="6aeee-1340">隱含類型陣列建立運算式可以與匿名物件初始化運算式（[匿名物件建立運算式](expressions.md#anonymous-object-creation-expressions)）結合，以建立匿名型別的資料結構。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1340">Implicitly typed array creation expressions can be combined with anonymous object initializers ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) to create anonymously typed data structures.</span></span> <span data-ttu-id="6aeee-1341">例如:</span><span class="sxs-lookup"><span data-stu-id="6aeee-1341">For example:</span></span>
```csharp
var contacts = new[] {
    new {
        Name = "Chris Smith",
        PhoneNumbers = new[] { "206-555-0101", "425-882-8080" }
    },
    new {
        Name = "Bob Harris",
        PhoneNumbers = new[] { "650-555-0199" }
    }
};
```

#### <a name="delegate-creation-expressions"></a><span data-ttu-id="6aeee-1342">委派建立運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-1342">Delegate creation expressions</span></span>

<span data-ttu-id="6aeee-1343">*Delegate_creation_expression*是用來建立*delegate_type*的新實例。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1343">A *delegate_creation_expression* is used to create a new instance of a *delegate_type*.</span></span>

```antlr
delegate_creation_expression
    : 'new' delegate_type '(' expression ')'
    ;
```

<span data-ttu-id="6aeee-1344">委派建立運算式的引數必須是方法群組、匿名函數或編譯時間類型的值 `dynamic` 或*delegate_type*。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1344">The argument of a delegate creation expression must be a method group, an anonymous function or a value of either the compile time type `dynamic` or a *delegate_type*.</span></span> <span data-ttu-id="6aeee-1345">如果引數是方法群組，它會識別要建立委派之物件的方法，以及實例方法的。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1345">If the argument is a method group, it identifies the method and, for an instance method, the object for which to create a delegate.</span></span> <span data-ttu-id="6aeee-1346">如果引數是匿名函式，它會直接定義委派目標的參數和方法主體。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1346">If the argument is an anonymous function it directly defines the parameters and method body of the delegate target.</span></span> <span data-ttu-id="6aeee-1347">如果引數是值，它會識別要建立複本的委派實例。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1347">If the argument is a value it identifies a delegate instance of which to create a copy.</span></span>

<span data-ttu-id="6aeee-1348">如果*運算式*的編譯時期類型 `dynamic`，則*delegate_creation_expression*會動態繫結（[動態](expressions.md#dynamic-binding)系結），而下列規則會在執行時間使用*運算式*的執行時間類型套用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1348">If the *expression* has the compile-time type `dynamic`, the *delegate_creation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), and the rules below are applied at run-time using the run-time type of the *expression*.</span></span> <span data-ttu-id="6aeee-1349">否則，規則會在編譯時期套用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1349">Otherwise the rules are applied at compile-time.</span></span>

<span data-ttu-id="6aeee-1350">*Delegate_creation_expression*表單的系結時間處理 `new D(E)`，其中 `D` 是一個*delegate_type* ，而 `E` 是一個*運算式*，其中包含下列步驟：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1350">The binding-time processing of a *delegate_creation_expression* of the form `new D(E)`, where `D` is a *delegate_type* and `E` is an *expression*, consists of the following steps:</span></span>

*  <span data-ttu-id="6aeee-1351">如果 `E` 是方法群組，則委派建立運算式的處理方式與從 `E` 到 `D` 的方法群組轉換（[方法群組轉換](conversions.md#method-group-conversions)）相同。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1351">If `E` is a method group, the delegate creation expression is processed in the same way as a method group conversion ([Method group conversions](conversions.md#method-group-conversions)) from `E` to `D`.</span></span>
*  <span data-ttu-id="6aeee-1352">如果 `E` 是匿名函式，則委派建立運算式的處理方式與從 `E` 到 `D` 的匿名函式轉換（[匿名函數轉換](conversions.md#anonymous-function-conversions)）相同。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1352">If `E` is an anonymous function, the delegate creation expression is processed in the same way as an anonymous function conversion ([Anonymous function conversions](conversions.md#anonymous-function-conversions)) from `E` to `D`.</span></span>
*  <span data-ttu-id="6aeee-1353">如果 `E` 是值，則 `E` 必須與 `D` 相容（[委派](delegates.md#delegate-declarations)宣告），而結果會參考與 `E` 相同的調用清單之類型 `D` 的新建立委派。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1353">If `E` is a value, `E` must be compatible ([Delegate declarations](delegates.md#delegate-declarations)) with `D`, and the result is a reference to a newly created delegate of type `D` that refers to the same invocation list as `E`.</span></span> <span data-ttu-id="6aeee-1354">如果 `E` 與 `D` 不相容，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1354">If `E` is not compatible with `D`, a compile-time error occurs.</span></span>

<span data-ttu-id="6aeee-1355">*Delegate_creation_expression*表單的執行時間處理 `new D(E)`，其中 `D` 是一個*delegate_type* ，而 `E` 是一個*運算式*，其中包含下列步驟：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1355">The run-time processing of a *delegate_creation_expression* of the form `new D(E)`, where `D` is a *delegate_type* and `E` is an *expression*, consists of the following steps:</span></span>

*   <span data-ttu-id="6aeee-1356">如果 `E` 是方法群組，則委派建立運算式會評估為從 `E` 到 `D` 的方法群組轉換（[方法群組轉換](conversions.md#method-group-conversions)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1356">If `E` is a method group, the delegate creation expression is evaluated as a method group conversion ([Method group conversions](conversions.md#method-group-conversions)) from `E` to `D`.</span></span>
*   <span data-ttu-id="6aeee-1357">如果 `E` 是匿名函式，則委派建立會評估為從 `E` 到 `D` （[匿名函數轉換](conversions.md#anonymous-function-conversions)）的匿名函式轉換。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1357">If `E` is an anonymous function, the delegate creation is evaluated as an anonymous function conversion from `E` to `D` ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>
*   <span data-ttu-id="6aeee-1358">如果 `E` 是*delegate_type*的值：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1358">If `E` is a value of a *delegate_type*:</span></span>
    * <span data-ttu-id="6aeee-1359">`E` 會進行評估。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1359">`E` is evaluated.</span></span> <span data-ttu-id="6aeee-1360">如果此評估導致例外狀況，則不會執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1360">If this evaluation causes an exception, no further steps are executed.</span></span>
    * <span data-ttu-id="6aeee-1361">如果 `E` 的值為 `null`，則會擲回 `System.NullReferenceException`，且不會執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1361">If the value of `E` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="6aeee-1362">已配置委派類型 `D` 的新實例。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1362">A new instance of the delegate type `D` is allocated.</span></span> <span data-ttu-id="6aeee-1363">如果沒有足夠的記憶體可配置新的實例，則會擲回 `System.OutOfMemoryException`，而且不會執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1363">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="6aeee-1364">新的委派實例會使用與 `E` 所指定的委派實例相同的調用清單進行初始化。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1364">The new delegate instance is initialized with the same invocation list as the delegate instance given by `E`.</span></span>

<span data-ttu-id="6aeee-1365">委派的調用清單是在委派具現化時決定，然後在委派的整個存留期間保持不變。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1365">The invocation list of a delegate is determined when the delegate is instantiated and then remains constant for the entire lifetime of the delegate.</span></span> <span data-ttu-id="6aeee-1366">換句話說，一旦建立委派之後，就不能變更它的目標可呼叫實體。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1366">In other words, it is not possible to change the target callable entities of a delegate once it has been created.</span></span> <span data-ttu-id="6aeee-1367">合併兩個委派時，或從另一個（[委派](delegates.md#delegate-declarations)宣告）中移除一個委派時，會產生新的委派結果;沒有任何現有的委派內容已變更。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1367">When two delegates are combined or one is removed from another ([Delegate declarations](delegates.md#delegate-declarations)), a new delegate results; no existing delegate has its contents changed.</span></span>

<span data-ttu-id="6aeee-1368">您不能建立參考屬性、索引子、使用者定義的運算子、實例的程式，或靜態函式的委派。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1368">It is not possible to create a delegate that refers to a property, indexer, user-defined operator, instance constructor, destructor, or static constructor.</span></span>

<span data-ttu-id="6aeee-1369">如上所述，從方法群組建立委派時，委派的正式參數清單和傳回型別會決定要選取的多載方法。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1369">As described above, when a delegate is created from a method group, the formal parameter list and return type of the delegate determine which of the overloaded methods to select.</span></span> <span data-ttu-id="6aeee-1370">在範例中</span><span class="sxs-lookup"><span data-stu-id="6aeee-1370">In the example</span></span>
```csharp
delegate double DoubleFunc(double x);

class A
{
    DoubleFunc f = new DoubleFunc(Square);

    static float Square(float x) {
        return x * x;
    }

    static double Square(double x) {
        return x * x;
    }
}
```
<span data-ttu-id="6aeee-1371">`A.f` 欄位會使用參考第二個 `Square` 方法的委派進行初始化，因為該方法完全符合 `DoubleFunc` 的正式參數清單和傳回類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1371">the `A.f` field is initialized with a delegate that refers to the second `Square` method because that method exactly matches the formal parameter list and return type of `DoubleFunc`.</span></span> <span data-ttu-id="6aeee-1372">如果第二個 `Square` 方法不存在，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1372">Had the second `Square` method not been present, a compile-time error would have occurred.</span></span>

#### <a name="anonymous-object-creation-expressions"></a><span data-ttu-id="6aeee-1373">匿名物件建立運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-1373">Anonymous object creation expressions</span></span>

<span data-ttu-id="6aeee-1374">*Anonymous_object_creation_expression*是用來建立匿名型別的物件。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1374">An *anonymous_object_creation_expression* is used to create an object of an anonymous type.</span></span>

```antlr
anonymous_object_creation_expression
    : 'new' anonymous_object_initializer
    ;

anonymous_object_initializer
    : '{' member_declarator_list? '}'
    | '{' member_declarator_list ',' '}'
    ;

member_declarator_list
    : member_declarator (',' member_declarator)*
    ;

member_declarator
    : simple_name
    | member_access
    | base_access
    | null_conditional_member_access
    | identifier '=' expression
    ;
```

<span data-ttu-id="6aeee-1375">匿名物件初始化運算式會宣告匿名型別，並傳回該型別的實例。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1375">An anonymous object initializer declares an anonymous type and returns an instance of that type.</span></span> <span data-ttu-id="6aeee-1376">匿名型別是直接繼承自 `object` 的無類型類別類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1376">An anonymous type is a nameless class type that inherits directly from `object`.</span></span> <span data-ttu-id="6aeee-1377">匿名型別的成員是從用來建立型別實例的匿名物件初始化運算式推斷的一系列唯讀屬性。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1377">The members of an anonymous type are a sequence of read-only properties inferred from the anonymous object initializer used to create an instance of the type.</span></span> <span data-ttu-id="6aeee-1378">具體而言，就是表單的匿名物件初始化運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-1378">Specifically, an anonymous object initializer of the form</span></span>
```csharp
new { p1 = e1, p2 = e2, ..., pn = en }
```
<span data-ttu-id="6aeee-1379">宣告表單的匿名型別</span><span class="sxs-lookup"><span data-stu-id="6aeee-1379">declares an anonymous type of the form</span></span>
```csharp
class __Anonymous1
{
    private readonly T1 f1;
    private readonly T2 f2;
    ...
    private readonly Tn fn;

    public __Anonymous1(T1 a1, T2 a2, ..., Tn an) {
        f1 = a1;
        f2 = a2;
        ...
        fn = an;
    }

    public T1 p1 { get { return f1; } }
    public T2 p2 { get { return f2; } }
    ...
    public Tn pn { get { return fn; } }

    public override bool Equals(object __o) { ... }
    public override int GetHashCode() { ... }
}
```
<span data-ttu-id="6aeee-1380">其中，每個 `Tx` 是對應運算式的類型 `ex`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1380">where each `Tx` is the type of the corresponding expression `ex`.</span></span> <span data-ttu-id="6aeee-1381">*Member_declarator*中使用的運算式必須具有類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1381">The expression used in a *member_declarator* must have a type.</span></span> <span data-ttu-id="6aeee-1382">因此，如果*member_declarator*中的運算式是 null 或匿名函式，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1382">Thus, it is a compile-time error for an expression in a *member_declarator* to be null or an anonymous function.</span></span> <span data-ttu-id="6aeee-1383">此外，也會發生編譯時期錯誤，讓運算式具有不安全的型別。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1383">It is also a compile-time error for the expression to have an unsafe type.</span></span>

<span data-ttu-id="6aeee-1384">匿名型別的名稱和參數的 `Equals` 方法是由編譯器自動產生的，而且無法在程式文字中參考。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1384">The names of an anonymous type and of the parameter to its `Equals` method are automatically generated by the compiler and cannot be referenced in program text.</span></span>

<span data-ttu-id="6aeee-1385">在相同的程式中，兩個匿名物件初始化運算式會以相同的順序指定相同名稱和編譯時間類型的一系列屬性，將會產生相同匿名型別的實例。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1385">Within the same program, two anonymous object initializers that specify a sequence of properties of the same names and compile-time types in the same order will produce instances of the same anonymous type.</span></span>

<span data-ttu-id="6aeee-1386">在範例中</span><span class="sxs-lookup"><span data-stu-id="6aeee-1386">In the example</span></span>
```csharp
var p1 = new { Name = "Lawnmower", Price = 495.00 };
var p2 = new { Name = "Shovel", Price = 26.95 };
p1 = p2;
```
<span data-ttu-id="6aeee-1387">因為 `p1` 和 `p2` 是相同的匿名型別，所以允許在最後一行上進行指派。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1387">the assignment on the last line is permitted because `p1` and `p2` are of the same anonymous type.</span></span>

<span data-ttu-id="6aeee-1388">匿名型別上的 `Equals` 和 @no__t 1 方法會覆寫繼承自 `object` 的方法，並根據屬性的 `Equals` 和 `GetHashcode` 來定義，因此，只有在其所有屬性都是時，相同匿名型別的兩個實例才會相等等於.</span><span class="sxs-lookup"><span data-stu-id="6aeee-1388">The `Equals` and `GetHashcode` methods on anonymous types override the methods inherited from `object`, and are defined in terms of the `Equals` and `GetHashcode` of the properties, so that two instances of the same anonymous type are equal if and only if all their properties are equal.</span></span>

<span data-ttu-id="6aeee-1389">成員宣告子可以縮寫為簡單名稱（[類型推斷](expressions.md#type-inference)）、成員存取（動態多載[解析的編譯時間檢查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)）、基底存取（[基底存取](expressions.md#base-access)）或 null 條件成員存取（[Null-條件運算式做為投影初始化運算式](expressions.md#null-conditional-expressions-as-projection-initializers)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1389">A member declarator can be abbreviated to a simple name ([Type inference](expressions.md#type-inference)), a member access ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), a base access ([Base access](expressions.md#base-access)) or a null-conditional member access ([Null-conditional expressions as projection initializers](expressions.md#null-conditional-expressions-as-projection-initializers)).</span></span> <span data-ttu-id="6aeee-1390">這稱為「***投射初始化運算式***」，而且是宣告的縮寫，而且會指派給具有相同名稱的屬性。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1390">This is called a ***projection initializer*** and is shorthand for a declaration of and assignment to a property with the same name.</span></span> <span data-ttu-id="6aeee-1391">具體而言，表單的成員宣告子</span><span class="sxs-lookup"><span data-stu-id="6aeee-1391">Specifically, member declarators of the forms</span></span>
```csharp
identifier
expr.identifier
```
<span data-ttu-id="6aeee-1392">會分別精確地等同于下列各項：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1392">are precisely equivalent to the following, respectively:</span></span>
```csharp
identifier = identifier
identifier = expr.identifier
```

<span data-ttu-id="6aeee-1393">因此，在投射初始化運算式中，此*識別碼*會同時選取值和要指派值的欄位或屬性。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1393">Thus, in a projection initializer the *identifier* selects both the value and the field or property to which the value is assigned.</span></span> <span data-ttu-id="6aeee-1394">簡單來說，投射初始化運算式專案不只是值，也是值的名稱。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1394">Intuitively, a projection initializer projects not just a value, but also the name of the value.</span></span>

### <a name="the-typeof-operator"></a><span data-ttu-id="6aeee-1395">Typeof 運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-1395">The typeof operator</span></span>

<span data-ttu-id="6aeee-1396">@No__t-0 運算子可用來取得類型的 `System.Type` 物件。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1396">The `typeof` operator is used to obtain the `System.Type` object for a type.</span></span>

```antlr
typeof_expression
    : 'typeof' '(' type ')'
    | 'typeof' '(' unbound_type_name ')'
    | 'typeof' '(' 'void' ')'
    ;

unbound_type_name
    : identifier generic_dimension_specifier?
    | identifier '::' identifier generic_dimension_specifier?
    | unbound_type_name '.' identifier generic_dimension_specifier?
    ;

generic_dimension_specifier
    : '<' comma* '>'
    ;

comma
    : ','
    ;
```

<span data-ttu-id="6aeee-1397">第一種形式的*typeof_expression*是由 @no__t 1 關鍵字後面接著加上括弧的*類型*所組成。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1397">The first form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized *type*.</span></span> <span data-ttu-id="6aeee-1398">此表單的運算式結果為所指定類型的 `System.Type` 物件。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1398">The result of an expression of this form is the `System.Type` object for the indicated type.</span></span> <span data-ttu-id="6aeee-1399">針對任何指定的類型，只有一個 @no__t 0 物件。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1399">There is only one `System.Type` object for any given type.</span></span> <span data-ttu-id="6aeee-1400">這表示針對 @ no__t-0 類型，`typeof(T) == typeof(T)` 一律為 true。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1400">This means that for a type `T`, `typeof(T) == typeof(T)` is always true.</span></span> <span data-ttu-id="6aeee-1401">*類型*不能 `dynamic`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1401">The *type* cannot be `dynamic`.</span></span>

<span data-ttu-id="6aeee-1402">第二種形式的*typeof_expression*是由 @no__t 1 關鍵字後面接著加上括弧的*unbound_type_name*所組成。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1402">The second form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized *unbound_type_name*.</span></span> <span data-ttu-id="6aeee-1403">*Unbound_type_name*與*type_name* （[命名空間和型別名稱](basic-concepts.md#namespace-and-type-names)）非常類似，不同之處在于*unbound_type_name*包含*generic_dimension_specifier*s，其中*type_name*包含*type_argument_list*s。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1403">An *unbound_type_name* is very similar to a *type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) except that an *unbound_type_name* contains *generic_dimension_specifier*s where a *type_name* contains *type_argument_list*s.</span></span> <span data-ttu-id="6aeee-1404">當*typeof_expression*的運算元為符合*unbound_type_name*和*type_name*之文法的 token 序列時，亦即它不包含*generic_dimension_specifier*或*type_argument_list*，權杖的順序會視為*type_name*。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1404">When the operand of a *typeof_expression* is a sequence of tokens that satisfies the grammars of both *unbound_type_name* and *type_name*, namely when it contains neither a *generic_dimension_specifier* nor a *type_argument_list*, the sequence of tokens is considered to be a *type_name*.</span></span> <span data-ttu-id="6aeee-1405">*Unbound_type_name*的意義是依照下列方式決定：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1405">The meaning of an *unbound_type_name* is determined as follows:</span></span>

*  <span data-ttu-id="6aeee-1406">將標記序列轉換成*type_name* ，方法是以具有相同數目之逗號和關鍵字 `object` 的*type_argument_list*取代每個*type_argument*的每個*generic_dimension_specifier* 。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1406">Convert the sequence of tokens to a *type_name* by replacing each *generic_dimension_specifier* with a *type_argument_list* having the same number of commas and the keyword `object` as each *type_argument*.</span></span>
*  <span data-ttu-id="6aeee-1407">評估產生的*type_name*，同時忽略所有類型參數條件約束。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1407">Evaluate the resulting *type_name*, while ignoring all type parameter constraints.</span></span>
*  <span data-ttu-id="6aeee-1408">*Unbound_type_name*會解析為與產生的結構化類型（系結[和未](types.md#bound-and-unbound-types)系結類型）相關聯的未系結泛型型別。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1408">The *unbound_type_name* resolves to the unbound generic type associated with the resulting constructed type ([Bound and unbound types](types.md#bound-and-unbound-types)).</span></span>

<span data-ttu-id="6aeee-1409">*Typeof_expression*的結果是產生的未系結泛型型別的 @no__t 1 物件。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1409">The result of the *typeof_expression* is the `System.Type` object for the resulting unbound generic type.</span></span>

<span data-ttu-id="6aeee-1410">第三種形式的*typeof_expression*是由 @no__t 1 關鍵字後面接著加上括弧的 `void` 關鍵字所組成。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1410">The third form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized `void` keyword.</span></span> <span data-ttu-id="6aeee-1411">這個表單的運算式結果是代表缺少類型的 @no__t 0 物件。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1411">The result of an expression of this form is the `System.Type` object that represents the absence of a type.</span></span> <span data-ttu-id="6aeee-1412">@No__t-0 傳回的類型物件與針對任何類型傳回的類型物件不同。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1412">The type object returned by `typeof(void)` is distinct from the type object returned for any type.</span></span> <span data-ttu-id="6aeee-1413">這個特殊類型物件在類別庫中很有用，可讓您以語言反映到方法，而這些方法希望能夠用來表示任何方法的傳回型別，包括 void 方法，`System.Type` 的實例。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1413">This special type object is useful in class libraries that allow reflection onto methods in the language, where those methods wish to have a way to represent the return type of any method, including void methods, with an instance of `System.Type`.</span></span>

<span data-ttu-id="6aeee-1414">@No__t-0 運算子可用於類型參數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1414">The `typeof` operator can be used on a type parameter.</span></span> <span data-ttu-id="6aeee-1415">結果為系結至型別參數之執行時間類型的 `System.Type` 物件。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1415">The result is the `System.Type` object for the run-time type that was bound to the type parameter.</span></span> <span data-ttu-id="6aeee-1416">@No__t-0 運算子也可以用於結構化類型或未系結的泛型型別（系結[和未](types.md#bound-and-unbound-types)系結的類型）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1416">The `typeof` operator can also be used on a constructed type or an unbound generic type ([Bound and unbound types](types.md#bound-and-unbound-types)).</span></span> <span data-ttu-id="6aeee-1417">未系結泛型型別的 `System.Type` 物件與實例類型的 `System.Type` 物件不同。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1417">The `System.Type` object for an unbound generic type is not the same as the `System.Type` object of the instance type.</span></span> <span data-ttu-id="6aeee-1418">實例型別在執行時間一律是封閉的已結構化型別，因此其 @no__t 0 物件取決於使用中的執行時間型別引數，而未系結的泛型型別沒有型別引數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1418">The instance type is always a closed constructed type at run-time so its `System.Type` object depends on the run-time type arguments in use, while the unbound generic type has no type arguments.</span></span>

<span data-ttu-id="6aeee-1419">範例</span><span class="sxs-lookup"><span data-stu-id="6aeee-1419">The example</span></span>
```csharp
using System;

class X<T>
{
    public static void PrintTypes() {
        Type[] t = {
            typeof(int),
            typeof(System.Int32),
            typeof(string),
            typeof(double[]),
            typeof(void),
            typeof(T),
            typeof(X<T>),
            typeof(X<X<T>>),
            typeof(X<>)
        };
        for (int i = 0; i < t.Length; i++) {
            Console.WriteLine(t[i]);
        }
    }
}

class Test
{
    static void Main() {
        X<int>.PrintTypes();
    }
}
```
<span data-ttu-id="6aeee-1420">會產生下列輸出：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1420">produces the following output:</span></span>
```console
System.Int32
System.Int32
System.String
System.Double[]
System.Void
System.Int32
X`1[System.Int32]
X`1[X`1[System.Int32]]
X`1[T]
```

<span data-ttu-id="6aeee-1421">請注意，`int` 和 `System.Int32` 是相同的類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1421">Note that `int` and `System.Int32` are the same type.</span></span>

<span data-ttu-id="6aeee-1422">另請注意，`typeof(X<>)` 的結果不會相依于類型引數，但 `typeof(X<T>)` 的結果則會執行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1422">Also note that the result of `typeof(X<>)` does not depend on the type argument but the result of `typeof(X<T>)` does.</span></span>

### <a name="the-checked-and-unchecked-operators"></a><span data-ttu-id="6aeee-1423">checked 和 unchecked 運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-1423">The checked and unchecked operators</span></span>

<span data-ttu-id="6aeee-1424">@No__t-0 和 @no__t 1 運算子可用來控制整數類型算數運算和轉換的***溢位檢查內容***。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1424">The `checked` and `unchecked` operators are used to control the ***overflow checking context*** for integral-type arithmetic operations and conversions.</span></span>

```antlr
checked_expression
    : 'checked' '(' expression ')'
    ;

unchecked_expression
    : 'unchecked' '(' expression ')'
    ;
```

<span data-ttu-id="6aeee-1425">@No__t-0 運算子會在檢查的內容中評估包含的運算式，而 `unchecked` 運算子會在未檢查的內容中評估包含的運算式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1425">The `checked` operator evaluates the contained expression in a checked context, and the `unchecked` operator evaluates the contained expression in an unchecked context.</span></span> <span data-ttu-id="6aeee-1426">*Checked_expression*或*unchecked_expression*完全對應于*parenthesized_expression* （加上[括弧的運算式](expressions.md#parenthesized-expressions)），不同之處在于包含的運算式會在給定的溢位檢查內容中評估.</span><span class="sxs-lookup"><span data-stu-id="6aeee-1426">A *checked_expression* or *unchecked_expression* corresponds exactly to a *parenthesized_expression* ([Parenthesized expressions](expressions.md#parenthesized-expressions)), except that the contained expression is evaluated in the given overflow checking context.</span></span>

<span data-ttu-id="6aeee-1427">溢位檢查內容也可以透過 `checked` 和 `unchecked` 語句（[checked 和 unchecked 語句](statements.md#the-checked-and-unchecked-statements)）來控制。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1427">The overflow checking context can also be controlled through the `checked` and `unchecked` statements ([The checked and unchecked statements](statements.md#the-checked-and-unchecked-statements)).</span></span>

<span data-ttu-id="6aeee-1428">下列作業會受到 `checked` 和 @no__t 1 運算子和語句所建立的溢位檢查內容所影響：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1428">The following operations are affected by the overflow checking context established by the `checked` and `unchecked` operators and statements:</span></span>

*  <span data-ttu-id="6aeee-1429">當運算元為整數類資料類型時，預先定義的 `++` 和 @no__t 1 一元運算子（後置[遞增和遞減運算子](expressions.md#postfix-increment-and-decrement-operators)和[前置遞增和遞減運算子](expressions.md#prefix-increment-and-decrement-operators)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1429">The predefined `++` and `--` unary operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), when the operand is of an integral type.</span></span>
*  <span data-ttu-id="6aeee-1430">當運算元為整數類資料類型時，預先定義的 `-` 一元運算子（[一元減號運算子](expressions.md#unary-minus-operator)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1430">The predefined `-` unary operator ([Unary minus operator](expressions.md#unary-minus-operator)), when the operand is of an integral type.</span></span>
*  <span data-ttu-id="6aeee-1431">當兩個運算元都是整數類資料類型時，預先定義的 `+`、`-`、@no__t 2 和 @no__t 3 二元運算子（[算術運算子](expressions.md#arithmetic-operators)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1431">The predefined `+`, `-`, `*`, and `/` binary operators ([Arithmetic operators](expressions.md#arithmetic-operators)), when both operands are of integral types.</span></span>
*  <span data-ttu-id="6aeee-1432">明確數值轉換（[明確數值轉換](conversions.md#explicit-numeric-conversions)），從一個整數類型到另一個整數類型，或從 `float` 或 `double` 到整數類資料類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1432">Explicit numeric conversions ([Explicit numeric conversions](conversions.md#explicit-numeric-conversions)) from one integral type to another integral type, or from `float` or `double` to an integral type.</span></span>

<span data-ttu-id="6aeee-1433">當上述其中一個作業產生的結果太大，而無法在目的地類型中表示時，執行作業的內容會控制產生的行為：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1433">When one of the above operations produce a result that is too large to represent in the destination type, the context in which the operation is performed controls the resulting behavior:</span></span>

*  <span data-ttu-id="6aeee-1434">在 @no__t 0 內容中，如果作業是常數運算式（[常數運算式](expressions.md#constant-expressions)），就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1434">In a `checked` context, if the operation is a constant expression ([Constant expressions](expressions.md#constant-expressions)), a compile-time error occurs.</span></span> <span data-ttu-id="6aeee-1435">否則，當作業在執行時間執行時，就會擲回 @no__t 0。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1435">Otherwise, when the operation is performed at run-time, a `System.OverflowException` is thrown.</span></span>
*  <span data-ttu-id="6aeee-1436">在 @no__t 0 內容中，會捨棄不符合目的地類型的任何高序位位，以截斷結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1436">In an `unchecked` context, the result is truncated by discarding any high-order bits that do not fit in the destination type.</span></span>

<span data-ttu-id="6aeee-1437">若為非常數運算式（在執行時間評估的運算式）未以任何 `checked` 或 @no__t 1 的運算子或語句括住，除非外部因素（例如編譯器參數和，所以預設溢位檢查內容會 @no__t 2執行環境設定）呼叫 `checked` 評估。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1437">For non-constant expressions (expressions that are evaluated at run-time) that are not enclosed by any `checked` or `unchecked` operators or statements, the default overflow checking context is `unchecked` unless external factors (such as compiler switches and execution environment configuration) call for `checked` evaluation.</span></span>

<span data-ttu-id="6aeee-1438">對於常數運算式（可在編譯時期完整評估的運算式），預設溢位檢查內容一律會 `checked`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1438">For constant expressions (expressions that can be fully evaluated at compile-time), the default overflow checking context is always `checked`.</span></span> <span data-ttu-id="6aeee-1439">除非常數運算式明確地放在 `unchecked` 內容中，否則在編譯時間評估運算式期間發生的溢出，一律會造成編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1439">Unless a constant expression is explicitly placed in an `unchecked` context, overflows that occur during the compile-time evaluation of the expression always cause compile-time errors.</span></span>

<span data-ttu-id="6aeee-1440">匿名函式的主體不會受到 `checked` 或在發生匿名函數的 @no__t 1 內容影響。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1440">The body of an anonymous function is not affected by `checked` or `unchecked` contexts in which the anonymous function occurs.</span></span>

<span data-ttu-id="6aeee-1441">在範例中</span><span class="sxs-lookup"><span data-stu-id="6aeee-1441">In the example</span></span>
```csharp
class Test
{
    static readonly int x = 1000000;
    static readonly int y = 1000000;

    static int F() {
        return checked(x * y);      // Throws OverflowException
    }

    static int G() {
        return unchecked(x * y);    // Returns -727379968
    }

    static int H() {
        return x * y;               // Depends on default
    }
}
```
<span data-ttu-id="6aeee-1442">因為沒有任何運算式可以在編譯時期評估，所以不會報告任何編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1442">no compile-time errors are reported since neither of the expressions can be evaluated at compile-time.</span></span> <span data-ttu-id="6aeee-1443">在執行時間，`F` 方法會擲回 `System.OverflowException`，而 @no__t 2 方法會傳回-727379968 （超出範圍結果的32位）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1443">At run-time, the `F` method throws a `System.OverflowException`, and the `G` method returns -727379968 (the lower 32 bits of the out-of-range result).</span></span> <span data-ttu-id="6aeee-1444">@No__t-0 方法的行為取決於編譯的預設溢位檢查內容，但它與 `F` 相同，或與 `G` 相同。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1444">The behavior of the `H` method depends on the default overflow checking context for the compilation, but it is either the same as `F` or the same as `G`.</span></span>

<span data-ttu-id="6aeee-1445">在範例中</span><span class="sxs-lookup"><span data-stu-id="6aeee-1445">In the example</span></span>
```csharp
class Test
{
    const int x = 1000000;
    const int y = 1000000;

    static int F() {
        return checked(x * y);      // Compile error, overflow
    }

    static int G() {
        return unchecked(x * y);    // Returns -727379968
    }

    static int H() {
        return x * y;               // Compile error, overflow
    }
}
```
<span data-ttu-id="6aeee-1446">評估 `F` 中的常數運算式時發生的溢位，而 `H` 會導致回報編譯時期錯誤，因為運算式是在 @no__t 2 內容中進行評估。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1446">the overflows that occur when evaluating the constant expressions in `F` and `H` cause compile-time errors to be reported because the expressions are evaluated in a `checked` context.</span></span> <span data-ttu-id="6aeee-1447">在 `G` 中評估常數運算式時也會發生溢位，但由於評估會在 @no__t 1 內容中進行，因此不會報告溢位。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1447">An overflow also occurs when evaluating the constant expression in `G`, but since the evaluation takes place in an `unchecked` context, the overflow is not reported.</span></span>

<span data-ttu-id="6aeee-1448">@No__t-0 和 @no__t 1 運算子只會影響以程式形式包含在「`(`」和「`)`」標記內之作業的溢位檢查內容。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1448">The `checked` and `unchecked` operators only affect the overflow checking context for those operations that are textually contained within the "`(`" and "`)`" tokens.</span></span> <span data-ttu-id="6aeee-1449">運算子在評估包含的運算式時，不會對叫用的函式成員造成影響。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1449">The operators have no effect on function members that are invoked as a result of evaluating the contained expression.</span></span> <span data-ttu-id="6aeee-1450">在範例中</span><span class="sxs-lookup"><span data-stu-id="6aeee-1450">In the example</span></span>
```csharp
class Test
{
    static int Multiply(int x, int y) {
        return x * y;
    }

    static int F() {
        return checked(Multiply(1000000, 1000000));
    }
}
```
<span data-ttu-id="6aeee-1451">在 `F` 中使用 `checked` 並不會影響 `Multiply` 中 `x * y` 的評估，因此會在預設溢位檢查內容中評估 `x * y`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1451">the use of `checked` in `F` does not affect the evaluation of `x * y` in `Multiply`, so `x * y` is evaluated in the default overflow checking context.</span></span>

<span data-ttu-id="6aeee-1452">當以十六進位標記法撰寫帶正負號整數類型的常數時，`unchecked` 運算子會很方便。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1452">The `unchecked` operator is convenient when writing constants of the signed integral types in hexadecimal notation.</span></span> <span data-ttu-id="6aeee-1453">例如:</span><span class="sxs-lookup"><span data-stu-id="6aeee-1453">For example:</span></span>
```csharp
class Test
{
    public const int AllBits = unchecked((int)0xFFFFFFFF);

    public const int HighBit = unchecked((int)0x80000000);
}
```

<span data-ttu-id="6aeee-1454">上述兩個十六進位常數的類型都是 `uint`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1454">Both of the hexadecimal constants above are of type `uint`.</span></span> <span data-ttu-id="6aeee-1455">因為常數不在 `int` 範圍之外，而沒有 `unchecked` 運算子，所以轉換成 `int` 會產生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1455">Because the constants are outside the `int` range, without the `unchecked` operator, the casts to `int` would produce compile-time errors.</span></span>

<span data-ttu-id="6aeee-1456">@No__t 0 和 @no__t 1 的運算子和語句，可讓程式設計人員控制某些數值計算的某些層面。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1456">The `checked` and `unchecked` operators and statements allow programmers to control certain aspects of some numeric calculations.</span></span> <span data-ttu-id="6aeee-1457">不過，某些數值運算子的行為取決於其運算元的資料類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1457">However, the behavior of some numeric operators depends on their operands' data types.</span></span> <span data-ttu-id="6aeee-1458">例如，兩個小數位數一律會導致溢位例外狀況，即使在明確 `unchecked` 的結構內也是如此。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1458">For example, multiplying two decimals always results in an exception on overflow even within an explicitly `unchecked` construct.</span></span> <span data-ttu-id="6aeee-1459">同樣地，即使在明確 `checked` 的結構中，將兩個浮點數相乘也不會造成溢位例外狀況。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1459">Similarly, multiplying two floats never results in an exception on overflow even within an explicitly `checked` construct.</span></span> <span data-ttu-id="6aeee-1460">此外，其他運算子絕不會受到檢查模式的影響，不論是預設或明確。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1460">In addition, other operators are never affected by the mode of checking, whether default or explicit.</span></span>

### <a name="default-value-expressions"></a><span data-ttu-id="6aeee-1461">預設值運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-1461">Default value expressions</span></span>

<span data-ttu-id="6aeee-1462">預設值運算式用來取得類型的預設值（[預設](variables.md#default-values)值）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1462">A default value expression is used to obtain the default value ([Default values](variables.md#default-values)) of a type.</span></span> <span data-ttu-id="6aeee-1463">通常會使用預設值運算式做為型別參數，因為如果型別參數是實值型別或參考型別，它可能不是已知的。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1463">Typically a default value expression is used for type parameters, since it may not be known if the type parameter is a value type or a reference type.</span></span> <span data-ttu-id="6aeee-1464">（除非已知型別參數是引用型別，否則不會從 `null` 常值轉換為型別參數）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1464">(No conversion exists from the `null` literal to a type parameter unless the type parameter is known to be a reference type.)</span></span>

```antlr
default_value_expression
    : 'default' '(' type ')'
    ;
```

<span data-ttu-id="6aeee-1465">如果*default_value_expression*中的*型*別在執行時間評估為引用型別，則結果會是轉換成該型別的 `null`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1465">If the *type* in a *default_value_expression* evaluates at run-time to a reference type, the result is `null` converted to that type.</span></span> <span data-ttu-id="6aeee-1466">如果*default_value_expression*中的*型*別在執行時間評估為實值型別，則結果會是*Value_type*的預設值（[預設的函數](types.md#default-constructors)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1466">If the *type* in a *default_value_expression* evaluates at run-time to a value type, the result is the *value_type*'s default value ([Default constructors](types.md#default-constructors)).</span></span>

<span data-ttu-id="6aeee-1467">如果類型是參考型別或已知為參考型別（[類型參數條件約束](classes.md#type-parameter-constraints)）的類型參數，則*default_value_expression*是常數運算式（[常數運算式](expressions.md#constant-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1467">A *default_value_expression* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) if the type is a reference type or a type parameter that is known to be a reference type ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="6aeee-1468">此外，如果類型為下列其中一個實數值型別，則*default_value_expression*是常數運算式： `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、0、1、2、3或任何列舉類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1468">In addition, a *default_value_expression* is a constant expression if the type is one of the following value types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, or any enumeration type.</span></span>


### <a name="nameof-expressions"></a><span data-ttu-id="6aeee-1469">Nameof 運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-1469">Nameof expressions</span></span>

<span data-ttu-id="6aeee-1470">*Nameof_expression*是用來取得程式實體的名稱，做為常數位串。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1470">A *nameof_expression* is used to obtain the name of a program entity as a constant string.</span></span>

```antlr
nameof_expression
    : 'nameof' '(' named_entity ')'
    ;

named_entity
    : simple_name
    | named_entity_target '.' identifier type_argument_list?
    ;

named_entity_target
    : 'this'
    | 'base'
    | named_entity 
    | predefined_type 
    | qualified_alias_member
    ;
```

<span data-ttu-id="6aeee-1471">文法說， *named_entity*運算元一律是運算式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1471">Grammatically speaking, the *named_entity* operand is always an expression.</span></span> <span data-ttu-id="6aeee-1472">因為 `nameof` 不是保留的關鍵字，所以 nameof 運算式在語法上與簡單名稱 `nameof` 的調用一律是不明確的。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1472">Because `nameof` is not a reserved keyword, a nameof expression is always syntactically ambiguous with an invocation of the simple name `nameof`.</span></span> <span data-ttu-id="6aeee-1473">基於相容性考慮，如果名稱的名稱查閱（[簡單名稱](expressions.md#simple-names)） `nameof` 成功，則會將運算式視為*invocation_expression* ，不論調用是否合法。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1473">For compatibility reasons, if a name lookup ([Simple names](expressions.md#simple-names)) of the name `nameof` succeeds, the expression is treated as an *invocation_expression* -- regardless of whether the invocation is legal.</span></span> <span data-ttu-id="6aeee-1474">否則就是*nameof_expression*。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1474">Otherwise it is a *nameof_expression*.</span></span>

<span data-ttu-id="6aeee-1475">*Nameof_expression*的*named_entity*意義就是運算式的意義;也就是*simple_name*、 *base_access*或*member_access*。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1475">The meaning of the *named_entity* of a *nameof_expression* is the meaning of it as an expression; that is, either as a *simple_name*, a *base_access* or a *member_access*.</span></span> <span data-ttu-id="6aeee-1476">不過，在[簡單名稱](expressions.md#simple-names)和[成員存取](expressions.md#member-access)中所述的查閱會導致錯誤，因為在靜態內容中找到實例成員，因此*nameof_expression*不會產生這類錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1476">However, where the lookup described in [Simple names](expressions.md#simple-names) and [Member access](expressions.md#member-access) results in an error because an instance member was found in a static context, a *nameof_expression* produces no such error.</span></span>

<span data-ttu-id="6aeee-1477">這是*named_entity*的編譯時期錯誤，指定方法群組具有*type_argument_list*。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1477">It is a compile-time error for a *named_entity* designating a method group to have a *type_argument_list*.</span></span> <span data-ttu-id="6aeee-1478">當*named_entity_target*的類型 `dynamic` 時，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1478">It is a compile time error for a *named_entity_target* to have the type `dynamic`.</span></span>

<span data-ttu-id="6aeee-1479">*Nameof_expression*是類型 `string` 的常數運算式，在執行時間沒有任何作用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1479">A *nameof_expression* is a constant expression of type `string`, and has no effect at runtime.</span></span> <span data-ttu-id="6aeee-1480">具體而言，它的*named_entity*不會進行評估，而且會因明確指派分析（[簡單運算式的一般規則](variables.md#general-rules-for-simple-expressions)）而被忽略。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1480">Specifically, its *named_entity* is not evaluated, and is ignored for the purposes of definite assignment analysis ([General rules for simple expressions](variables.md#general-rules-for-simple-expressions)).</span></span> <span data-ttu-id="6aeee-1481">其值是選擇性的最後*type_argument_list*之前*named_entity*的最後一個識別碼，以下列方式轉換：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1481">Its value is the last identifier of the *named_entity* before the optional final *type_argument_list*, transformed in the following way:</span></span>

* <span data-ttu-id="6aeee-1482">已移除前置`@`詞 "" （如果使用的話）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1482">The prefix "`@`", if used, is removed.</span></span>
* <span data-ttu-id="6aeee-1483">每個*unicode_escape_sequence*都會轉換成其對應的 unicode 字元。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1483">Each *unicode_escape_sequence* is transformed into its corresponding Unicode character.</span></span>
* <span data-ttu-id="6aeee-1484">任何*formatting_characters*都會移除。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1484">Any *formatting_characters* are removed.</span></span>

<span data-ttu-id="6aeee-1485">這些是在測試識別碼之間是否相等時，在[識別碼](lexical-structure.md#identifiers)中套用的相同轉換。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1485">These are the same transformations applied in [Identifiers](lexical-structure.md#identifiers) when testing equality between identifiers.</span></span>

<span data-ttu-id="6aeee-1486">TODO：範例</span><span class="sxs-lookup"><span data-stu-id="6aeee-1486">TODO: examples</span></span>

### <a name="anonymous-method-expressions"></a><span data-ttu-id="6aeee-1487">匿名方法運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-1487">Anonymous method expressions</span></span>

<span data-ttu-id="6aeee-1488">*Anonymous_method_expression*是定義匿名函式的兩個方法之一。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1488">An *anonymous_method_expression* is one of two ways of defining an anonymous function.</span></span> <span data-ttu-id="6aeee-1489">這些會在[匿名函數運算式](expressions.md#anonymous-function-expressions)中進一步說明。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1489">These are further described in [Anonymous function expressions](expressions.md#anonymous-function-expressions).</span></span>

## <a name="unary-operators"></a><span data-ttu-id="6aeee-1490">一元運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-1490">Unary operators</span></span>

<span data-ttu-id="6aeee-1491">@No__t-0、`+`、`-`、`!`、`~`、@no__t 5、`--`、cast 和 `await` 運算子稱為一元運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1491">The `?`, `+`, `-`, `!`, `~`, `++`, `--`, cast, and `await` operators are called the unary operators.</span></span>

```antlr
unary_expression
    : primary_expression
    | null_conditional_expression
    | '+' unary_expression
    | '-' unary_expression
    | '!' unary_expression
    | '~' unary_expression
    | pre_increment_expression
    | pre_decrement_expression
    | cast_expression
    | await_expression
    | unary_expression_unsafe
    ;
```

<span data-ttu-id="6aeee-1492">如果*unary_expression*的運算元具有編譯時期類型 `dynamic`，則會動態繫結（[動態](expressions.md#dynamic-binding)系結）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1492">If the operand of a *unary_expression* has the compile-time type `dynamic`, it is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="6aeee-1493">在此情況下， *unary_expression*的編譯時期類型為 `dynamic`，而下面所述的解析將會在執行時間使用運算元的執行時間類型進行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1493">In this case the compile-time type of the *unary_expression* is `dynamic`, and the resolution described below will take place at run-time using the run-time type of the operand.</span></span>

### <a name="null-conditional-operator"></a><span data-ttu-id="6aeee-1494">Null-條件運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-1494">Null-conditional operator</span></span>

<span data-ttu-id="6aeee-1495">Null 條件運算子只有在該運算元不是 null 時，才會將作業清單套用至其運算元。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1495">The null-conditional operator applies a list of operations to its operand only if that operand is non-null.</span></span> <span data-ttu-id="6aeee-1496">否則，套用運算子的結果為 `null`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1496">Otherwise the result of applying the operator is `null`.</span></span>

```antlr
null_conditional_expression
    : primary_expression null_conditional_operations
    ;

null_conditional_operations
    : null_conditional_operations? '?' '.' identifier type_argument_list?
    | null_conditional_operations? '?' '[' argument_list ']'
    | null_conditional_operations '.' identifier type_argument_list?
    | null_conditional_operations '[' argument_list ']'
    | null_conditional_operations '(' argument_list? ')'
    ;
```

<span data-ttu-id="6aeee-1497">作業的清單可以包含成員存取和專案存取作業（本身可能是 null 條件），以及調用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1497">The list of operations can include member access and element access operations (which may themselves be null-conditional), as well as invocation.</span></span>

<span data-ttu-id="6aeee-1498">例如，運算式 `a.b?[0]?.c()` 是*primary_expression* `a.b` 和*null_conditional_operations* `?[0]` （null 條件式元素存取）的*null_conditional_expression* ，`?.c` （null-條件式成員存取）和 `()` （調用）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1498">For example, the expression `a.b?[0]?.c()` is a *null_conditional_expression* with a *primary_expression* `a.b` and *null_conditional_operations* `?[0]` (null-conditional element access), `?.c` (null-conditional member access) and `()` (invocation).</span></span>

<span data-ttu-id="6aeee-1499">對於具有*primary_expression* `P` 的*null_conditional_expression* `E`，讓 `E0` 是以全形方式從 `E` 的每個*null_conditional_operations*中移除前置 `?` 的運算式有一個。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1499">For a *null_conditional_expression* `E` with a *primary_expression* `P`, let `E0` be the expression obtained by textually removing the leading `?` from each of the *null_conditional_operations* of `E` that have one.</span></span> <span data-ttu-id="6aeee-1500">在概念上，`E0` 是運算式，如果沒有任何由 `?` 代表的 null 檢查，就會尋找 `null`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1500">Conceptually, `E0` is the expression that will be evaluated if none of the null checks represented by the `?`s do find a `null`.</span></span>

<span data-ttu-id="6aeee-1501">此外，讓 `E1` 是以全形方式從 `E` 中的第一個*null_conditional_operations*中移除前置 `?` 的運算式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1501">Also, let `E1` be the expression obtained by textually removing the leading `?` from just the first of the *null_conditional_operations* in `E`.</span></span> <span data-ttu-id="6aeee-1502">這可能會導致*主要運算式*（如果只有一個 `?`）或另一個*null_conditional_expression*。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1502">This may lead to a *primary-expression* (if there was just one `?`) or to another *null_conditional_expression*.</span></span>

<span data-ttu-id="6aeee-1503">例如，如果 `E` 是運算式 `a.b?[0]?.c()`，則 `E0` 是運算式 `a.b[0].c()`，而 `E1` 是運算式 `a.b[0]?.c()`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1503">For example, if `E` is the expression `a.b?[0]?.c()`, then `E0` is the expression `a.b[0].c()` and `E1` is the expression `a.b[0]?.c()`.</span></span>

<span data-ttu-id="6aeee-1504">如果 `E0` 分類為 [無]，則 `E` 分類為 [無]。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1504">If `E0` is classified as nothing, then `E` is classified as nothing.</span></span> <span data-ttu-id="6aeee-1505">否則，會將 E 分類為值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1505">Otherwise E is classified as a value.</span></span>

<span data-ttu-id="6aeee-1506">`E0` 和 `E1` 是用來判斷 `E` 的意義：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1506">`E0` and `E1` are used to determine the meaning of `E`:</span></span>

*  <span data-ttu-id="6aeee-1507">如果 `E` 發生為*statement_expression* ，則 `E` 的意義與語句相同</span><span class="sxs-lookup"><span data-stu-id="6aeee-1507">If `E` occurs as a *statement_expression* the meaning of `E` is the same as the statement</span></span>

   ```csharp
   if ((object)P != null) E1;
   ```

   <span data-ttu-id="6aeee-1508">但只會評估 P 一次。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1508">except that P is evaluated only once.</span></span>

*  <span data-ttu-id="6aeee-1509">否則，如果 `E0` 分類為不會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1509">Otherwise, if `E0` is classified as nothing a compile-time error occurs.</span></span>

*  <span data-ttu-id="6aeee-1510">否則，讓 `T0` 是 `E0` 的類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1510">Otherwise, let `T0` be the type of `E0`.</span></span>

   *  <span data-ttu-id="6aeee-1511">如果 `T0` 是不知道是參考型別或不可為 null 的實數值型別的類型參數，則會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1511">If `T0` is a type parameter that is not known to be a reference type or a non-nullable value type, a compile-time error occurs.</span></span>

   *  <span data-ttu-id="6aeee-1512">如果 `T0` 是不可為 null 的實數值型別，則 `E` 的類型會 `T0?`，而 `E` 的意義則與</span><span class="sxs-lookup"><span data-stu-id="6aeee-1512">If `T0` is a non-nullable value type, then the type of `E` is `T0?`, and the meaning of `E` is the same as</span></span>

      ```csharp
      ((object)P == null) ? (T0?)null : E1
      ```

      <span data-ttu-id="6aeee-1513">除了 `P` 只會評估一次。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1513">except that `P` is evaluated only once.</span></span>

   *  <span data-ttu-id="6aeee-1514">否則，E 的類型為 T0，而 E 的意義則與</span><span class="sxs-lookup"><span data-stu-id="6aeee-1514">Otherwise the type of E is T0, and the meaning of E is the same as</span></span>

      ```csharp
      ((object)P == null) ? null : E1
      ```

      <span data-ttu-id="6aeee-1515">除了 `P` 只會評估一次。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1515">except that `P` is evaluated only once.</span></span>

<span data-ttu-id="6aeee-1516">如果 `E1` 本身就是*null_conditional_expression*，則會再次套用這些規則、將 @no__t 的測試加以嵌套，直到沒有進一步的 `?` 為止，而且運算式已減少到主要運算式 `E0` 為止。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1516">If `E1` is itself a *null_conditional_expression*, then these rules are applied again, nesting the tests for `null` until there are no further `?`'s, and the expression has been reduced all the way down to the primary-expression `E0`.</span></span>

<span data-ttu-id="6aeee-1517">例如，如果運算式 `a.b?[0]?.c()` 會當做語句運算式發生，如同語句中的一樣：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1517">For example, if the expression `a.b?[0]?.c()` occurs as a statement-expression, as in the statement:</span></span>
```csharp
a.b?[0]?.c();
```
<span data-ttu-id="6aeee-1518">其意義相當於：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1518">its meaning is equivalent to:</span></span>
```csharp
if (a.b != null) a.b[0]?.c();
```
<span data-ttu-id="6aeee-1519">這同樣相當於：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1519">which again is equivalent to:</span></span>
```csharp
if (a.b != null) if (a.b[0] != null) a.b[0].c();
```
<span data-ttu-id="6aeee-1520">除了 `a.b` 和 `a.b[0]` 只會評估一次。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1520">Except that `a.b` and `a.b[0]` are evaluated only once.</span></span>

<span data-ttu-id="6aeee-1521">如果它發生在使用其值的內容中，如下所示：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1521">If it occurs in a context where its value is used, as in:</span></span>
```csharp
var x = a.b?[0]?.c();
```
<span data-ttu-id="6aeee-1522">而且假設最後一個調用的型別不是不可為 null 的實值型別，其意義就相當於：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1522">and assuming that the type of the final invocation is not a non-nullable value type, its meaning is equivalent to:</span></span>
```csharp
var x = (a.b == null) ? null : (a.b[0] == null) ? null : a.b[0].c();
```
<span data-ttu-id="6aeee-1523">除了 `a.b` 和 `a.b[0]` 只會評估一次。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1523">except that `a.b` and `a.b[0]` are evaluated only once.</span></span>

#### <a name="null-conditional-expressions-as-projection-initializers"></a><span data-ttu-id="6aeee-1524">Null-條件運算式做為投影初始化運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-1524">Null-conditional expressions as projection initializers</span></span>

<span data-ttu-id="6aeee-1525">Null 條件運算式僅允許作為*anonymous_object_creation_expression* （[匿名物件建立運算式](expressions.md#anonymous-object-creation-expressions)）中的*member_declarator* （如果其結尾為（選擇性的 null 條件式）成員存取）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1525">A null-conditional expression is only allowed as a *member_declarator* in an *anonymous_object_creation_expression* ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) if it ends with an (optionally null-conditional) member access.</span></span> <span data-ttu-id="6aeee-1526">文法而言，這項需求可以表示為：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1526">Grammatically, this requirement can be expressed as:</span></span>

```antlr
null_conditional_member_access
    : primary_expression null_conditional_operations? '?' '.' identifier type_argument_list?
    | primary_expression null_conditional_operations '.' identifier type_argument_list?
    ;
```

<span data-ttu-id="6aeee-1527">這是上述*null_conditional_expression*文法的特殊案例。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1527">This is a special case of the grammar for *null_conditional_expression* above.</span></span> <span data-ttu-id="6aeee-1528">在[匿名物件建立運算式](expressions.md#anonymous-object-creation-expressions)中， *member_declarator*的生產環境只會包含*null_conditional_member_access*。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1528">The production for *member_declarator* in [Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions) then includes only *null_conditional_member_access*.</span></span>

#### <a name="null-conditional-expressions-as-statement-expressions"></a><span data-ttu-id="6aeee-1529">Null-條件運算式做為語句運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-1529">Null-conditional expressions as statement expressions</span></span>

<span data-ttu-id="6aeee-1530">Null 條件運算式僅允許做為*statement_expression* （[運算式語句](statements.md#expression-statements)）（如果它以調用結尾）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1530">A null-conditional expression is only allowed as a *statement_expression* ([Expression statements](statements.md#expression-statements)) if it ends with an invocation.</span></span> <span data-ttu-id="6aeee-1531">文法而言，這項需求可以表示為：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1531">Grammatically, this requirement can be expressed as:</span></span>

```antlr
null_conditional_invocation_expression
    : primary_expression null_conditional_operations '(' argument_list? ')'
    ;
```

<span data-ttu-id="6aeee-1532">這是上述*null_conditional_expression*文法的特殊案例。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1532">This is a special case of the grammar for *null_conditional_expression* above.</span></span> <span data-ttu-id="6aeee-1533">在[運算式語句](statements.md#expression-statements)中， *statement_expression*的生產環境只會包含*null_conditional_invocation_expression*。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1533">The production for *statement_expression* in [Expression statements](statements.md#expression-statements) then includes only *null_conditional_invocation_expression*.</span></span>


### <a name="unary-plus-operator"></a><span data-ttu-id="6aeee-1534">一元加號運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-1534">Unary plus operator</span></span>

<span data-ttu-id="6aeee-1535">若為格式為 `+x` 的作業，則會套用一元運算子多載解析（[一元運算子](expressions.md#unary-operator-overload-resolution)多載解析）來選取特定的運算子實作為運算。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1535">For an operation of the form `+x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="6aeee-1536">運算元會轉換為所選運算子的參數類型，而結果的類型會是運算子的傳回類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1536">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="6aeee-1537">預先定義的一元加號運算子如下：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1537">The predefined unary plus operators are:</span></span>

```csharp
int operator +(int x);
uint operator +(uint x);
long operator +(long x);
ulong operator +(ulong x);
float operator +(float x);
double operator +(double x);
decimal operator +(decimal x);
```

<span data-ttu-id="6aeee-1538">針對每個運算子，結果只是運算元的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1538">For each of these operators, the result is simply the value of the operand.</span></span>

### <a name="unary-minus-operator"></a><span data-ttu-id="6aeee-1539">一元減號運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-1539">Unary minus operator</span></span>

<span data-ttu-id="6aeee-1540">若為格式為 `-x` 的作業，則會套用一元運算子多載解析（[一元運算子](expressions.md#unary-operator-overload-resolution)多載解析）來選取特定的運算子實作為運算。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1540">For an operation of the form `-x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="6aeee-1541">運算元會轉換為所選運算子的參數類型，而結果的類型會是運算子的傳回類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1541">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="6aeee-1542">預先定義的負運算子如下：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1542">The predefined negation operators are:</span></span>

*  <span data-ttu-id="6aeee-1543">整數否定：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1543">Integer negation:</span></span>

   ```csharp
   int operator -(int x);
   long operator -(long x);
   ```

   <span data-ttu-id="6aeee-1544">計算結果的方式是從零減去 `x`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1544">The result is computed by subtracting `x` from zero.</span></span> <span data-ttu-id="6aeee-1545">如果 `x` 的值是運算元類型的最小可顯示值（`int` 為-2 ^ 31，而 `long` 為-2 ^ 63），則不會在運算元類型內顯示 `x` 的數學否定。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1545">If the value of `x` is the smallest representable value of the operand type (-2^31 for `int` or -2^63 for `long`), then the mathematical negation of `x` is not representable within the operand type.</span></span> <span data-ttu-id="6aeee-1546">如果這發生在 `checked` 內容中，則會擲回 `System.OverflowException`;如果在 `unchecked` 內容中發生，則結果會是運算元的值，而且不會報告溢位。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1546">If this occurs within a `checked` context, a `System.OverflowException` is thrown; if it occurs within an `unchecked` context, the result is the value of the operand and the overflow is not reported.</span></span>

   <span data-ttu-id="6aeee-1547">如果負運算子的運算元屬於類型 `uint`，它會轉換成類型 `long`，而結果的類型會 `long`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1547">If the operand of the negation operator is of type `uint`, it is converted to type `long`, and the type of the result is `long`.</span></span> <span data-ttu-id="6aeee-1548">例外狀況是 @no__t 允許將0值-2147483648 （-2 ^ 31）寫入為十進位整數常值（[整數常](lexical-structure.md#integer-literals)值）的規則。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1548">An exception is the rule that permits the `int` value -2147483648 (-2^31) to be written as a decimal integer literal ([Integer literals](lexical-structure.md#integer-literals)).</span></span>

   <span data-ttu-id="6aeee-1549">如果負運算子的運算元屬於類型 `ulong`，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1549">If the operand of the negation operator is of type `ulong`, a compile-time error occurs.</span></span> <span data-ttu-id="6aeee-1550">例外狀況是 @no__t 允許將0值-9223372036854775808 （-2 ^ 63）寫入為十進位整數常值（[整數常](lexical-structure.md#integer-literals)值）的規則。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1550">An exception is the rule that permits the `long` value -9223372036854775808 (-2^63) to be written as a decimal integer literal ([Integer literals](lexical-structure.md#integer-literals)).</span></span>

*  <span data-ttu-id="6aeee-1551">浮點否定：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1551">Floating-point negation:</span></span>

   ```csharp
   float operator -(float x);
   double operator -(double x);
   ```

   <span data-ttu-id="6aeee-1552">結果為 `x` 的值，其正負號相反。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1552">The result is the value of `x` with its sign inverted.</span></span> <span data-ttu-id="6aeee-1553">如果 `x` 是 NaN，則結果也是 NaN。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1553">If `x` is NaN, the result is also NaN.</span></span>

*  <span data-ttu-id="6aeee-1554">十進位數否定：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1554">Decimal negation:</span></span>

   ```csharp
   decimal operator -(decimal x);
   ```

   <span data-ttu-id="6aeee-1555">計算結果的方式是從零減去 `x`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1555">The result is computed by subtracting `x` from zero.</span></span> <span data-ttu-id="6aeee-1556">十進位否定相當於使用類型 `System.Decimal` 的一元減號運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1556">Decimal negation is equivalent to using the unary minus operator of type `System.Decimal`.</span></span>

### <a name="logical-negation-operator"></a><span data-ttu-id="6aeee-1557">邏輯否定運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-1557">Logical negation operator</span></span>

<span data-ttu-id="6aeee-1558">若為格式為 `!x` 的作業，則會套用一元運算子多載解析（[一元運算子](expressions.md#unary-operator-overload-resolution)多載解析）來選取特定的運算子實作為運算。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1558">For an operation of the form `!x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="6aeee-1559">運算元會轉換為所選運算子的參數類型，而結果的類型會是運算子的傳回類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1559">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="6aeee-1560">只有一個預先定義的邏輯否定運算子存在：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1560">Only one predefined logical negation operator exists:</span></span>
```csharp
bool operator !(bool x);
```

<span data-ttu-id="6aeee-1561">這個運算子會計算運算元的邏輯否定：如果運算元 `true`，則結果為 `false`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1561">This operator computes the logical negation of the operand: If the operand is `true`, the result is `false`.</span></span> <span data-ttu-id="6aeee-1562">如果運算元 `false`，則結果為 `true`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1562">If the operand is `false`, the result is `true`.</span></span>

### <a name="bitwise-complement-operator"></a><span data-ttu-id="6aeee-1563">位補數運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-1563">Bitwise complement operator</span></span>

<span data-ttu-id="6aeee-1564">若為格式為 `~x` 的作業，則會套用一元運算子多載解析（[一元運算子](expressions.md#unary-operator-overload-resolution)多載解析）來選取特定的運算子實作為運算。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1564">For an operation of the form `~x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="6aeee-1565">運算元會轉換為所選運算子的參數類型，而結果的類型會是運算子的傳回類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1565">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="6aeee-1566">預先定義的位補數運算子如下：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1566">The predefined bitwise complement operators are:</span></span>
```csharp
int operator ~(int x);
uint operator ~(uint x);
long operator ~(long x);
ulong operator ~(ulong x);
```

<span data-ttu-id="6aeee-1567">針對每個運算子，運算的結果是 `x` 的位補數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1567">For each of these operators, the result of the operation is the bitwise complement of `x`.</span></span>

<span data-ttu-id="6aeee-1568">每個列舉類型 `E` 會隱含地提供下列位補數運算子：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1568">Every enumeration type `E` implicitly provides the following bitwise complement operator:</span></span>

```csharp
E operator ~(E x);
```

<span data-ttu-id="6aeee-1569">評估 `~x` 的結果，其中 `x` 是列舉類型的運算式，`E`，基礎類型 `U`，與評估 `(E)(~(U)x)` 完全相同，不同之處在于 `E` 的轉換一律會如同在 `unchecked` 內容中一樣執行（[Checked 和 unchecked 運算子](expressions.md#the-checked-and-unchecked-operators)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1569">The result of evaluating `~x`, where `x` is an expression of an enumeration type `E` with an underlying type `U`, is exactly the same as evaluating `(E)(~(U)x)`, except that the conversion to `E` is always performed as if in an `unchecked` context ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)).</span></span>

### <a name="prefix-increment-and-decrement-operators"></a><span data-ttu-id="6aeee-1570">前置遞增和遞減運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-1570">Prefix increment and decrement operators</span></span>

```antlr
pre_increment_expression
    : '++' unary_expression
    ;

pre_decrement_expression
    : '--' unary_expression
    ;
```

<span data-ttu-id="6aeee-1571">前置遞增或遞減運算的運算元必須是分類為變數、屬性存取或索引子存取的運算式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1571">The operand of a prefix increment or decrement operation must be an expression classified as a variable, a property access, or an indexer access.</span></span> <span data-ttu-id="6aeee-1572">運算的結果是與運算元相同類型的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1572">The result of the operation is a value of the same type as the operand.</span></span>

<span data-ttu-id="6aeee-1573">如果前置詞遞增或遞減運算的運算元是屬性或索引子存取，則屬性或索引子必須同時具有 `get` 和 @no__t 1 存取子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1573">If the operand of a prefix increment or decrement operation is a property or indexer access, the property or indexer must have both a `get` and a `set` accessor.</span></span> <span data-ttu-id="6aeee-1574">如果不是這種情況，就會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1574">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="6aeee-1575">一元運算子多載解析（[一元運算子](expressions.md#unary-operator-overload-resolution)多載解析）適用于選取特定的運算子執行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1575">Unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="6aeee-1576">預先定義的 `++` 和 @no__t 1 運算子存在於下列類型： `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`--`0、1、2、3 和任何列舉類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1576">Predefined `++` and `--` operators exist for the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, and any enum type.</span></span> <span data-ttu-id="6aeee-1577">預先定義的 `++` 運算子會傳回將1加入運算元所產生的值，而預先定義的 @no__t 1 運算子會傳回從運算元減去1所產生的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1577">The predefined `++` operators return the value produced by adding 1 to the operand, and the predefined `--` operators return the value produced by subtracting 1 from the operand.</span></span> <span data-ttu-id="6aeee-1578">在 @no__t 0 內容中，如果此加法或減法的結果超出結果類型的範圍，而結果類型是整數類型或列舉類型，則會擲回 `System.OverflowException`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1578">In a `checked` context, if the result of this addition or subtraction is outside the range of the result type and the result type is an integral type or enum type, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="6aeee-1579">@No__t-0 或 `--x` 格式之前置遞增或遞減運算的執行時間處理包含下列步驟：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1579">The run-time processing of a prefix increment or decrement operation of the form `++x` or `--x` consists of the following steps:</span></span>

*   <span data-ttu-id="6aeee-1580">如果 `x` 分類為變數：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1580">If `x` is classified as a variable:</span></span>
    * <span data-ttu-id="6aeee-1581">`x` 會進行評估以產生變數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1581">`x` is evaluated to produce the variable.</span></span>
    * <span data-ttu-id="6aeee-1582">會叫用選取的運算子，其值為 `x` 做為其引數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1582">The selected operator is invoked with the value of `x` as its argument.</span></span>
    * <span data-ttu-id="6aeee-1583">運算子所傳回的值會儲存在評估 `x` 所指定的位置。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1583">The value returned by the operator is stored in the location given by the evaluation of `x`.</span></span>
    * <span data-ttu-id="6aeee-1584">運算子傳回的值會變成運算的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1584">The value returned by the operator becomes the result of the operation.</span></span>
*   <span data-ttu-id="6aeee-1585">如果 `x` 已分類為屬性或索引子存取：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1585">If `x` is classified as a property or indexer access:</span></span>
    * <span data-ttu-id="6aeee-1586">實例運算式（如果 `x` 不是 `static`），而且會評估與 `x` 相關聯的引數清單（如果 `x` 是索引子存取），而結果會用於後續的 `get` 和 `set` 存取子調用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1586">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `get` and `set` accessor invocations.</span></span>
    * <span data-ttu-id="6aeee-1587">已叫用 `x` 的 `get` 存取子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1587">The `get` accessor of `x` is invoked.</span></span>
    * <span data-ttu-id="6aeee-1588">所選取的運算子會以 `get` 存取子所傳回的值做為其引數來叫用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1588">The selected operator is invoked with the value returned by the `get` accessor as its argument.</span></span>
    * <span data-ttu-id="6aeee-1589">@No__t-1 的 `set` 存取子是以運算子所傳回的值做為其 `value` 引數來叫用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1589">The `set` accessor of `x` is invoked with the value returned by the operator as its `value` argument.</span></span>
    * <span data-ttu-id="6aeee-1590">運算子傳回的值會變成運算的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1590">The value returned by the operator becomes the result of the operation.</span></span>

<span data-ttu-id="6aeee-1591">@No__t-0 和 @no__t 1 運算子也支援後置標記法（後置[遞增和遞減運算子](expressions.md#postfix-increment-and-decrement-operators)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1591">The `++` and `--` operators also support postfix notation ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="6aeee-1592">一般來說，`x++` 或 `x--` 的結果是作業之前的 `x` 的值，而 `++x` 或 `--x` 的結果是作業之後的 `x` 的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1592">Typically, the result of `x++` or `x--` is the value of `x` before the operation, whereas the result of `++x` or `--x` is the value of `x` after the operation.</span></span> <span data-ttu-id="6aeee-1593">不論是哪一種情況，在作業之後，`x` 本身會有相同的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1593">In either case, `x` itself has the same value after the operation.</span></span>

<span data-ttu-id="6aeee-1594">您可以使用後置或首碼標記法來叫用 `operator++` 或 @no__t 1 的執行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1594">An `operator++` or `operator--` implementation can be invoked using either postfix or prefix notation.</span></span> <span data-ttu-id="6aeee-1595">這兩種標記法不可能有個別的運算子實現。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1595">It is not possible to have separate operator implementations for the two notations.</span></span>

### <a name="cast-expressions"></a><span data-ttu-id="6aeee-1596">Cast 運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-1596">Cast expressions</span></span>

<span data-ttu-id="6aeee-1597">*Cast_expression*是用來將運算式明確轉換成指定的型別。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1597">A *cast_expression* is used to explicitly convert an expression to a given type.</span></span>

```antlr
cast_expression
    : '(' type ')' unary_expression
    ;
```

<span data-ttu-id="6aeee-1598">格式為 `(T)E` 的*cast_expression* ，其中 `T` 為*類型*，而 `E` 是*unary_expression*，會執行 `E` 值到類型 `T` 的明確轉換（[明確](conversions.md#explicit-conversions)轉換）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1598">A *cast_expression* of the form `(T)E`, where `T` is a *type* and `E` is a *unary_expression*, performs an explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) of the value of `E` to type `T`.</span></span> <span data-ttu-id="6aeee-1599">如果沒有從 `E` 到 `T` 的明確轉換存在，就會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1599">If no explicit conversion exists from `E` to `T`, a binding-time error occurs.</span></span> <span data-ttu-id="6aeee-1600">否則，結果會是明確轉換所產生的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1600">Otherwise, the result is the value produced by the explicit conversion.</span></span> <span data-ttu-id="6aeee-1601">結果一律會分類為值，即使 `E` 代表一個變數也一樣。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1601">The result is always classified as a value, even if `E` denotes a variable.</span></span>

<span data-ttu-id="6aeee-1602">*Cast_expression*的文法會導致某些語法上的多義性。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1602">The grammar for a *cast_expression* leads to certain syntactic ambiguities.</span></span> <span data-ttu-id="6aeee-1603">例如，運算式 `(x)-y` 可以解讀為*cast_expression* （`-y` 轉型為類型 `x`），或轉譯為與*parenthesized_expression*結合的*additive_expression* （這會計算 `x - y)` 的值）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1603">For example, the expression `(x)-y` could either be interpreted as a *cast_expression* (a cast of `-y` to type `x`) or as an *additive_expression* combined with a *parenthesized_expression* (which computes the value `x - y)`.</span></span>

<span data-ttu-id="6aeee-1604">若要解決*cast_expression*的歧義，有下列規則存在：只有在下列其中一項條件成立時，才會將括在括弧中的一或多個*token*s （[空白字元](lexical-structure.md#white-space)）序列視為*cast_expression*的開頭：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1604">To resolve *cast_expression* ambiguities, the following rule exists: A sequence of one or more *token*s ([White space](lexical-structure.md#white-space)) enclosed in parentheses is considered the start of a *cast_expression* only if at least one of the following are true:</span></span>

*  <span data-ttu-id="6aeee-1605">Token 的順序是*型*別的正確文法，而不是*運算式*。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1605">The sequence of tokens is correct grammar for a *type*, but not for an *expression*.</span></span>
*  <span data-ttu-id="6aeee-1606">Token 的順序是正確的*類型*文法，而緊接在右括弧後面的標記是「`~`」、「`!`」標記、token 「`(`」、「*識別碼*」（[Unicode 字元逸出序列）](lexical-structure.md#unicode-character-escape-sequences)）、*常*值（[常](lexical-structure.md#literals)值）或任何*關鍵字*（[關鍵字](lexical-structure.md#keywords)），但 0 和 1 除外。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1606">The sequence of tokens is correct grammar for a *type*, and the token immediately following the closing parentheses is the token "`~`", the token "`!`", the token "`(`", an *identifier* ([Unicode character escape sequences](lexical-structure.md#unicode-character-escape-sequences)), a *literal* ([Literals](lexical-structure.md#literals)), or any *keyword* ([Keywords](lexical-structure.md#keywords)) except `as` and `is`.</span></span>

<span data-ttu-id="6aeee-1607">上述「正確的文法」一詞表示標記的順序必須符合特定文法生產。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1607">The term "correct grammar" above means only that the sequence of tokens must conform to the particular grammatical production.</span></span> <span data-ttu-id="6aeee-1608">它特別不會考慮任何組成識別碼的實際意義。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1608">It specifically does not consider the actual meaning of any constituent identifiers.</span></span> <span data-ttu-id="6aeee-1609">例如，如果 `x`，而 `y` 是識別碼，則 `x.y` 是類型的正確文法，即使 `x.y` 實際上不代表類型也是一樣。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1609">For example, if `x` and `y` are identifiers, then `x.y` is correct grammar for a type, even if `x.y` doesn't actually denote a type.</span></span>

<span data-ttu-id="6aeee-1610">從去除去除的規則，如果 `x` 和 `y` 是識別碼，`(x)y`，`(x)(y)`，而 `(x)(-y)` 是*cast_expression*s，但 `(x)-y` 則不是，即使 `x` 識別類型也是如此。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1610">From the disambiguation rule it follows that, if `x` and `y` are identifiers, `(x)y`, `(x)(y)`, and `(x)(-y)` are *cast_expression*s, but `(x)-y` is not, even if `x` identifies a type.</span></span> <span data-ttu-id="6aeee-1611">不過，如果 `x` 是識別預先定義之類型的關鍵字（例如 `int`），則所有四種形式都是*cast_expression*的（因為這類關鍵字本身不可能是運算式）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1611">However, if `x` is a keyword that identifies a predefined type (such as `int`), then all four forms are *cast_expression*s (because such a keyword could not possibly be an expression by itself).</span></span>

### <a name="await-expressions"></a><span data-ttu-id="6aeee-1612">Await 運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-1612">Await expressions</span></span>

<span data-ttu-id="6aeee-1613">Await 運算子是用來暫止封閉式非同步函式的評估，直到運算元所代表的非同步作業完成為止。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1613">The await operator is used to suspend evaluation of the enclosing async function until the asynchronous operation represented by the operand has completed.</span></span>

```antlr
await_expression
    : 'await' unary_expression
    ;
```

<span data-ttu-id="6aeee-1614">只有在非同步函式（[反覆運算](classes.md#iterators)器）的主體中，才允許使用*await_expression* 。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1614">An *await_expression* is only allowed in the body of an async function ([Iterators](classes.md#iterators)).</span></span> <span data-ttu-id="6aeee-1615">在最接近的封閉式非同步函式中，下列位置可能不會發生*await_expression* ：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1615">Within the nearest enclosing async function, an *await_expression* may not occur in these places:</span></span>

*  <span data-ttu-id="6aeee-1616">在嵌套的（非非同步）匿名函式內</span><span class="sxs-lookup"><span data-stu-id="6aeee-1616">Inside a nested (non-async) anonymous function</span></span>
*  <span data-ttu-id="6aeee-1617">在*lock_statement*的區塊內</span><span class="sxs-lookup"><span data-stu-id="6aeee-1617">Inside the block of a *lock_statement*</span></span>
*  <span data-ttu-id="6aeee-1618">在不安全的內容中</span><span class="sxs-lookup"><span data-stu-id="6aeee-1618">In an unsafe context</span></span>

<span data-ttu-id="6aeee-1619">請注意， *await_expression*不能出現在*q*內的大部分地方，因為這些是以語法方式轉換為使用非非同步 lambda 運算式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1619">Note that an *await_expression* cannot occur in most places within a *query_expression*, because those are syntactically transformed to use non-async lambda expressions.</span></span>

<span data-ttu-id="6aeee-1620">在非同步函式內部，`await` 不能當做識別碼使用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1620">Inside of an async function, `await` cannot be used as an identifier.</span></span> <span data-ttu-id="6aeee-1621">因此，await 運算式和涉及識別碼的各種運算式之間不會有任何語法不明確。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1621">There is therefore no syntactic ambiguity between await-expressions and various expressions involving identifiers.</span></span> <span data-ttu-id="6aeee-1622">在非同步函式的外部，`await` 會作為一般識別碼。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1622">Outside of async functions, `await` acts as a normal identifier.</span></span>

<span data-ttu-id="6aeee-1623">*Await_expression*的運算元稱為「工作」（ ***task***）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1623">The operand of an *await_expression* is called the ***task***.</span></span> <span data-ttu-id="6aeee-1624">它代表在評估*await_expression*時，不一定會完成的非同步作業。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1624">It represents an asynchronous operation that may or may not be complete at the time the *await_expression* is evaluated.</span></span> <span data-ttu-id="6aeee-1625">Await 運算子的目的是暫停封閉式非同步函式的執行，直到等候的工作完成為止，然後取得其結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1625">The purpose of the await operator is to suspend execution of the enclosing async function until the awaited task is complete, and then obtain its outcome.</span></span>

#### <a name="awaitable-expressions"></a><span data-ttu-id="6aeee-1626">可等候運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-1626">Awaitable expressions</span></span>

<span data-ttu-id="6aeee-1627">Await 運算式的工作必須是***可等候***。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1627">The task of an await expression is required to be ***awaitable***.</span></span> <span data-ttu-id="6aeee-1628">如果下列其中一個保留，則會可等候運算式 `t`：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1628">An expression `t` is awaitable if one of the following holds:</span></span>

*  <span data-ttu-id="6aeee-1629">`t` 是編譯時間類型 `dynamic`</span><span class="sxs-lookup"><span data-stu-id="6aeee-1629">`t` is of compile time type `dynamic`</span></span>
*  <span data-ttu-id="6aeee-1630">`t` 有一個可存取的實例或擴充方法，稱為 `GetAwaiter`，沒有參數且沒有型別參數，以及下列所有內容都是 `A` 的傳回類型：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1630">`t` has an accessible instance or extension method called `GetAwaiter` with no parameters and no type parameters, and a return type `A` for which all of the following hold:</span></span>
   * <span data-ttu-id="6aeee-1631">`A` 會將介面實作為 `System.Runtime.CompilerServices.INotifyCompletion` （如果是簡單明瞭，則為 `INotifyCompletion`）</span><span class="sxs-lookup"><span data-stu-id="6aeee-1631">`A` implements the interface `System.Runtime.CompilerServices.INotifyCompletion` (hereafter known as `INotifyCompletion` for brevity)</span></span>
   * <span data-ttu-id="6aeee-1632">`A` 具有可存取、可讀取的實例屬性 `IsCompleted` 類型的 `bool`</span><span class="sxs-lookup"><span data-stu-id="6aeee-1632">`A` has an accessible, readable instance property `IsCompleted` of type `bool`</span></span>
   * <span data-ttu-id="6aeee-1633">`A` 有一個可存取的實例方法 `GetResult`，沒有參數，而且沒有型別參數</span><span class="sxs-lookup"><span data-stu-id="6aeee-1633">`A` has an accessible instance method `GetResult` with no parameters and no type parameters</span></span>

<span data-ttu-id="6aeee-1634">@No__t-0 方法的目的是要取得工作的***awaiter*** 。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1634">The purpose of the `GetAwaiter` method is to obtain an ***awaiter*** for the task.</span></span> <span data-ttu-id="6aeee-1635">@No__t-0 的類型稱為 await 運算式的***awaiter 類型***。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1635">The type `A` is called the ***awaiter type*** for the await expression.</span></span>

<span data-ttu-id="6aeee-1636">@No__t-0 屬性的目的是要判斷工作是否已完成。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1636">The purpose of the `IsCompleted` property is to determine if the task is already complete.</span></span> <span data-ttu-id="6aeee-1637">若是如此，就不需要暫停評估。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1637">If so, there is no need to suspend evaluation.</span></span>

<span data-ttu-id="6aeee-1638">@No__t-0 方法的目的是將「接續」註冊至工作;也就是工作完成後，將會叫用的委派（類型 `System.Action`）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1638">The purpose of the `INotifyCompletion.OnCompleted` method is to sign up a "continuation" to the task; i.e. a delegate (of type `System.Action`) that will be invoked once the task is complete.</span></span>

<span data-ttu-id="6aeee-1639">@No__t-0 方法的目的是要在工作完成後取得其結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1639">The purpose of the `GetResult` method is to obtain the outcome of the task once it is complete.</span></span> <span data-ttu-id="6aeee-1640">這項結果可能是成功完成（可能包含結果值），也可能是由 `GetResult` 方法擲回的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1640">This outcome may be successful completion, possibly with a result value, or it may be an exception which is thrown by the `GetResult` method.</span></span>

#### <a name="classification-of-await-expressions"></a><span data-ttu-id="6aeee-1641">Await 運算式的分類</span><span class="sxs-lookup"><span data-stu-id="6aeee-1641">Classification of await expressions</span></span>

<span data-ttu-id="6aeee-1642">運算式 `await t` 的分類方式與運算式 `(t).GetAwaiter().GetResult()` 相同。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1642">The expression `await t` is classified the same way as the expression `(t).GetAwaiter().GetResult()`.</span></span> <span data-ttu-id="6aeee-1643">因此，如果 `GetResult` 的傳回類型是 `void`，則*await_expression*會分類為不是任何內容。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1643">Thus, if the return type of `GetResult` is `void`, the *await_expression* is classified as nothing.</span></span> <span data-ttu-id="6aeee-1644">如果它具有非 void 傳回型別 `T`，則會將*await_expression*分類為 `T` 類型的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1644">If it has a non-void return type `T`, the *await_expression* is classified as a value of type `T`.</span></span>

#### <a name="runtime-evaluation-of-await-expressions"></a><span data-ttu-id="6aeee-1645">Await 運算式的執行時間評估</span><span class="sxs-lookup"><span data-stu-id="6aeee-1645">Runtime evaluation of await expressions</span></span>

<span data-ttu-id="6aeee-1646">在執行時間，會評估運算式 `await t`，如下所示：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1646">At runtime, the expression `await t` is evaluated as follows:</span></span>

*  <span data-ttu-id="6aeee-1647">Awaiter `a` 是藉由評估運算式 `(t).GetAwaiter()` 取得。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1647">An awaiter `a` is obtained by evaluating the expression `(t).GetAwaiter()`.</span></span>
*  <span data-ttu-id="6aeee-1648">@No__t-0 `b` 是藉由評估運算式 `(a).IsCompleted` 來取得。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1648">A `bool` `b` is obtained by evaluating the expression `(a).IsCompleted`.</span></span>
*  <span data-ttu-id="6aeee-1649">如果 `b` 是 `false`，則評估取決於 `a` 是否會 `System.Runtime.CompilerServices.ICriticalNotifyCompletion` 來執行介面（如果是簡單明瞭，則稱為 `ICriticalNotifyCompletion`）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1649">If `b` is `false` then evaluation depends on whether `a` implements the interface `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (hereafter known as `ICriticalNotifyCompletion` for brevity).</span></span> <span data-ttu-id="6aeee-1650">這種檢查會在系結階段完成;例如，如果 `a` 的編譯時間類型 `dynamic`，而在編譯時期則為，否則為。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1650">This check is done at binding time; i.e. at runtime if `a` has the compile time type `dynamic`, and at compile time otherwise.</span></span> <span data-ttu-id="6aeee-1651">Let `r` 代表繼續委派（[反覆運算](classes.md#iterators)器）：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1651">Let `r` denote the resumption delegate ([Iterators](classes.md#iterators)):</span></span>
    * <span data-ttu-id="6aeee-1652">如果 `a` 不會執行 `ICriticalNotifyCompletion`，則會評估運算式 `(a as (INotifyCompletion)).OnCompleted(r)`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1652">If `a` does not implement `ICriticalNotifyCompletion`, then the expression `(a as (INotifyCompletion)).OnCompleted(r)` is evaluated.</span></span>
    * <span data-ttu-id="6aeee-1653">如果 `a` 執行 `ICriticalNotifyCompletion`，則會評估運算式 `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1653">If `a` does implement `ICriticalNotifyCompletion`, then the expression `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` is evaluated.</span></span>
    * <span data-ttu-id="6aeee-1654">然後會暫止評估，並將控制權傳回給 async 函數目前的呼叫端。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1654">Evaluation is then suspended, and control is returned to the current caller of the async function.</span></span>
*  <span data-ttu-id="6aeee-1655">緊接在之後（如果 `b` `true`），或在稍後叫用繼續委派時（如果 `b` `false`），則會評估運算式 `(a).GetResult()`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1655">Either immediately after (if `b` was `true`), or upon later invocation of the resumption delegate (if `b` was `false`), the expression `(a).GetResult()` is evaluated.</span></span> <span data-ttu-id="6aeee-1656">如果它傳回值，該值就是*await_expression*的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1656">If it returns a value, that value is the result of the *await_expression*.</span></span> <span data-ttu-id="6aeee-1657">否則，結果為 [無]。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1657">Otherwise the result is nothing.</span></span>

<span data-ttu-id="6aeee-1658">Awaiter 的介面方法 `INotifyCompletion.OnCompleted` 和 `ICriticalNotifyCompletion.UnsafeOnCompleted` 的執行應該會導致最多隻能叫用一次委派 `r`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1658">An awaiter's implementation of the interface methods `INotifyCompletion.OnCompleted` and `ICriticalNotifyCompletion.UnsafeOnCompleted` should cause the delegate `r` to be invoked at most once.</span></span> <span data-ttu-id="6aeee-1659">否則，封閉式非同步函式的行為會是未定義的。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1659">Otherwise, the behavior of the enclosing async function is undefined.</span></span>

## <a name="arithmetic-operators"></a><span data-ttu-id="6aeee-1660">算術運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-1660">Arithmetic operators</span></span>

<span data-ttu-id="6aeee-1661">@No__t-0、`/`、`%`、`+` 和 @no__t 4 運算子稱為算術運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1661">The `*`, `/`, `%`, `+`, and `-` operators are called the arithmetic operators.</span></span>

```antlr
multiplicative_expression
    : unary_expression
    | multiplicative_expression '*' unary_expression
    | multiplicative_expression '/' unary_expression
    | multiplicative_expression '%' unary_expression
    ;

additive_expression
    : multiplicative_expression
    | additive_expression '+' multiplicative_expression
    | additive_expression '-' multiplicative_expression
    ;
```

<span data-ttu-id="6aeee-1662">如果算術運算子的運算元具有編譯時期類型 `dynamic`，則運算式會動態繫結（[動態](expressions.md#dynamic-binding)系結）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1662">If an operand of an arithmetic operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="6aeee-1663">在此情況下，運算式的編譯時期類型為 `dynamic`，而以下所述的解決將會在執行時間使用具有編譯時間類型 `dynamic` 之運算元的執行時間類型進行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1663">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

### <a name="multiplication-operator"></a><span data-ttu-id="6aeee-1664">乘法運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-1664">Multiplication operator</span></span>

<span data-ttu-id="6aeee-1665">若為格式為 `x * y` 的作業，則會套用二元運算子多載解析（[二元運算子](expressions.md#binary-operator-overload-resolution)多載解析）來選取特定的運算子執行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1665">For an operation of the form `x * y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="6aeee-1666">運算元會轉換成所選運算子的參數類型，而結果的類型會是運算子的傳回類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1666">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="6aeee-1667">預先定義的乘法運算子如下所示。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1667">The predefined multiplication operators are listed below.</span></span> <span data-ttu-id="6aeee-1668">運算子會計算 `x` 和 `y` 的乘積。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1668">The operators all compute the product of `x` and `y`.</span></span>

*  <span data-ttu-id="6aeee-1669">整數乘法：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1669">Integer multiplication:</span></span>

   ```csharp
   int operator *(int x, int y);
   uint operator *(uint x, uint y);
   long operator *(long x, long y);
   ulong operator *(ulong x, ulong y);
   ```

   <span data-ttu-id="6aeee-1670">在 `checked` 內容中，如果產品超出結果類型的範圍，則會擲回 `System.OverflowException`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1670">In a `checked` context, if the product is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="6aeee-1671">在 `unchecked` 內容中，不會報告溢位，且會捨棄超出結果型別範圍的任何重要高序位位。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1671">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>


*  <span data-ttu-id="6aeee-1672">浮點乘法：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1672">Floating-point multiplication:</span></span>

   ```csharp
   float operator *(float x, float y);
   double operator *(double x, double y);
   ```

   <span data-ttu-id="6aeee-1673">產品是根據 IEEE 754 算術的規則來計算。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1673">The product is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="6aeee-1674">下表列出非零的有限值、零、無限大和 NaN 的所有可能組合的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1674">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="6aeee-1675">在資料表中，`x`，而 `y` 是正的有限值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1675">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="6aeee-1676">`z` 是 `x * y` 的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1676">`z` is the result of `x * y`.</span></span> <span data-ttu-id="6aeee-1677">如果結果對目的地類型而言太大，`z` 為無限大。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1677">If the result is too large for the destination type, `z` is infinity.</span></span> <span data-ttu-id="6aeee-1678">如果結果太小而無法用於目的地類型，`z` 為零。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1678">If the result is too small for the destination type, `z` is zero.</span></span>

   |      |      |      |     |     |      |      |     |
   |:----:|-----:|:----:|:---:|:---:|:----:|:----:|:----|
   |      | <span data-ttu-id="6aeee-1679">\+ y</span><span class="sxs-lookup"><span data-stu-id="6aeee-1679">+y</span></span>   | <span data-ttu-id="6aeee-1680">-y</span><span class="sxs-lookup"><span data-stu-id="6aeee-1680">-y</span></span>   | <span data-ttu-id="6aeee-1681">+0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1681">+0</span></span>  | <span data-ttu-id="6aeee-1682">-0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1682">-0</span></span>  | <span data-ttu-id="6aeee-1683">+inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1683">+inf</span></span> | <span data-ttu-id="6aeee-1684">-inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1684">-inf</span></span> | <span data-ttu-id="6aeee-1685">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1685">NaN</span></span> | 
   | <span data-ttu-id="6aeee-1686">+x</span><span class="sxs-lookup"><span data-stu-id="6aeee-1686">+x</span></span>   | <span data-ttu-id="6aeee-1687">+z</span><span class="sxs-lookup"><span data-stu-id="6aeee-1687">+z</span></span>   | <span data-ttu-id="6aeee-1688">-z</span><span class="sxs-lookup"><span data-stu-id="6aeee-1688">-z</span></span>   | <span data-ttu-id="6aeee-1689">+0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1689">+0</span></span>  | <span data-ttu-id="6aeee-1690">-0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1690">-0</span></span>  | <span data-ttu-id="6aeee-1691">+inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1691">+inf</span></span> | <span data-ttu-id="6aeee-1692">-inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1692">-inf</span></span> | <span data-ttu-id="6aeee-1693">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1693">NaN</span></span> | 
   | <span data-ttu-id="6aeee-1694">-x</span><span class="sxs-lookup"><span data-stu-id="6aeee-1694">-x</span></span>   | <span data-ttu-id="6aeee-1695">-z</span><span class="sxs-lookup"><span data-stu-id="6aeee-1695">-z</span></span>   | <span data-ttu-id="6aeee-1696">+z</span><span class="sxs-lookup"><span data-stu-id="6aeee-1696">+z</span></span>   | <span data-ttu-id="6aeee-1697">-0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1697">-0</span></span>  | <span data-ttu-id="6aeee-1698">+0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1698">+0</span></span>  | <span data-ttu-id="6aeee-1699">-inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1699">-inf</span></span> | <span data-ttu-id="6aeee-1700">+inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1700">+inf</span></span> | <span data-ttu-id="6aeee-1701">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1701">NaN</span></span> | 
   | <span data-ttu-id="6aeee-1702">+0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1702">+0</span></span>   | <span data-ttu-id="6aeee-1703">+0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1703">+0</span></span>   | <span data-ttu-id="6aeee-1704">-0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1704">-0</span></span>   | <span data-ttu-id="6aeee-1705">+0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1705">+0</span></span>  | <span data-ttu-id="6aeee-1706">-0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1706">-0</span></span>  | <span data-ttu-id="6aeee-1707">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1707">NaN</span></span>  | <span data-ttu-id="6aeee-1708">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1708">NaN</span></span>  | <span data-ttu-id="6aeee-1709">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1709">NaN</span></span> | 
   | <span data-ttu-id="6aeee-1710">-0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1710">-0</span></span>   | <span data-ttu-id="6aeee-1711">-0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1711">-0</span></span>   | <span data-ttu-id="6aeee-1712">+0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1712">+0</span></span>   | <span data-ttu-id="6aeee-1713">-0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1713">-0</span></span>  | <span data-ttu-id="6aeee-1714">+0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1714">+0</span></span>  | <span data-ttu-id="6aeee-1715">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1715">NaN</span></span>  | <span data-ttu-id="6aeee-1716">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1716">NaN</span></span>  | <span data-ttu-id="6aeee-1717">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1717">NaN</span></span> | 
   | <span data-ttu-id="6aeee-1718">+inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1718">+inf</span></span> | <span data-ttu-id="6aeee-1719">+inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1719">+inf</span></span> | <span data-ttu-id="6aeee-1720">-inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1720">-inf</span></span> | <span data-ttu-id="6aeee-1721">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1721">NaN</span></span> | <span data-ttu-id="6aeee-1722">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1722">NaN</span></span> | <span data-ttu-id="6aeee-1723">+inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1723">+inf</span></span> | <span data-ttu-id="6aeee-1724">-inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1724">-inf</span></span> | <span data-ttu-id="6aeee-1725">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1725">NaN</span></span> | 
   | <span data-ttu-id="6aeee-1726">-inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1726">-inf</span></span> | <span data-ttu-id="6aeee-1727">-inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1727">-inf</span></span> | <span data-ttu-id="6aeee-1728">+inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1728">+inf</span></span> | <span data-ttu-id="6aeee-1729">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1729">NaN</span></span> | <span data-ttu-id="6aeee-1730">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1730">NaN</span></span> | <span data-ttu-id="6aeee-1731">-inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1731">-inf</span></span> | <span data-ttu-id="6aeee-1732">+inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1732">+inf</span></span> | <span data-ttu-id="6aeee-1733">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1733">NaN</span></span> | 
   | <span data-ttu-id="6aeee-1734">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1734">NaN</span></span>  | <span data-ttu-id="6aeee-1735">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1735">NaN</span></span>  | <span data-ttu-id="6aeee-1736">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1736">NaN</span></span>  | <span data-ttu-id="6aeee-1737">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1737">NaN</span></span> | <span data-ttu-id="6aeee-1738">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1738">NaN</span></span> | <span data-ttu-id="6aeee-1739">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1739">NaN</span></span>  | <span data-ttu-id="6aeee-1740">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1740">NaN</span></span>  | <span data-ttu-id="6aeee-1741">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1741">NaN</span></span> | 

*  <span data-ttu-id="6aeee-1742">Decimal 乘法：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1742">Decimal multiplication:</span></span>

   ```csharp
   decimal operator *(decimal x, decimal y);
   ```

   <span data-ttu-id="6aeee-1743">如果產生的值太大，而無法以 `decimal` 格式表示，則會擲回 `System.OverflowException`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1743">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="6aeee-1744">如果結果值太小而無法以 `decimal` 格式表示，則結果會是零。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1744">If the result value is too small to represent in the `decimal` format, the result is zero.</span></span> <span data-ttu-id="6aeee-1745">在任何進位之前，結果的小數位數是兩個運算元之刻度的總和。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1745">The scale of the result, before any rounding, is the sum of the scales of the two operands.</span></span>

   <span data-ttu-id="6aeee-1746">Decimal 乘法相當於使用類型 `System.Decimal` 的乘法運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1746">Decimal multiplication is equivalent to using the multiplication operator of type `System.Decimal`.</span></span>


### <a name="division-operator"></a><span data-ttu-id="6aeee-1747">除法運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-1747">Division operator</span></span>

<span data-ttu-id="6aeee-1748">若為格式為 `x / y` 的作業，則會套用二元運算子多載解析（[二元運算子](expressions.md#binary-operator-overload-resolution)多載解析）來選取特定的運算子執行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1748">For an operation of the form `x / y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="6aeee-1749">運算元會轉換成所選運算子的參數類型，而結果的類型會是運算子的傳回類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1749">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="6aeee-1750">預先定義的除法運算子如下所示。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1750">The predefined division operators are listed below.</span></span> <span data-ttu-id="6aeee-1751">運算子會計算 `x` 和 `y` 的商。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1751">The operators all compute the quotient of `x` and `y`.</span></span>

*  <span data-ttu-id="6aeee-1752">整數除法：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1752">Integer division:</span></span>

   ```csharp
   int operator /(int x, int y);
   uint operator /(uint x, uint y);
   long operator /(long x, long y);
   ulong operator /(ulong x, ulong y);
   ```

   <span data-ttu-id="6aeee-1753">如果右運算元的值為零，則會擲回 @no__t 0。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1753">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span>

   <span data-ttu-id="6aeee-1754">除法會將結果向零四捨五入。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1754">The division rounds the result towards zero.</span></span> <span data-ttu-id="6aeee-1755">因此，結果的絕對值是最大的可能整數，小於或等於兩個運算元的商的絕對值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1755">Thus the absolute value of the result is the largest possible integer that is less than or equal to the absolute value of the quotient of the two operands.</span></span> <span data-ttu-id="6aeee-1756">當兩個運算元具有相同的正負號，而且兩個運算元具有相反正負號時，結果為零或正數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1756">The result is zero or positive when the two operands have the same sign and zero or negative when the two operands have opposite signs.</span></span>

   <span data-ttu-id="6aeee-1757">如果左運算元是最小的可顯示 `int` 或 `long` 值，右運算元是 `-1`，則會發生溢位。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1757">If the left operand is the smallest representable `int` or `long` value and the right operand is `-1`, an overflow occurs.</span></span> <span data-ttu-id="6aeee-1758">在 `checked` 內容中，這會擲回 `System.ArithmeticException` （或其子類別）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1758">In a `checked` context, this causes a `System.ArithmeticException` (or a subclass thereof) to be thrown.</span></span> <span data-ttu-id="6aeee-1759">在 `unchecked` 內容中，它會定義為是否擲回 `System.ArithmeticException` （或其子類別），否則會以左運算元的結果值來未報告溢位。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1759">In an `unchecked` context, it is implementation-defined as to whether a `System.ArithmeticException` (or a subclass thereof) is thrown or the overflow goes unreported with the resulting value being that of the left operand.</span></span>

*  <span data-ttu-id="6aeee-1760">浮點除法：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1760">Floating-point division:</span></span>

   ```csharp
   float operator /(float x, float y);
   double operator /(double x, double y);
   ```

   <span data-ttu-id="6aeee-1761">這些商是根據 IEEE 754 算術的規則來計算。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1761">The quotient is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="6aeee-1762">下表列出非零的有限值、零、無限大和 NaN 的所有可能組合的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1762">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="6aeee-1763">在資料表中，`x`，而 `y` 是正的有限值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1763">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="6aeee-1764">`z` 是 `x / y` 的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1764">`z` is the result of `x / y`.</span></span> <span data-ttu-id="6aeee-1765">如果結果對目的地類型而言太大，`z` 為無限大。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1765">If the result is too large for the destination type, `z` is infinity.</span></span> <span data-ttu-id="6aeee-1766">如果結果太小而無法用於目的地類型，`z` 為零。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1766">If the result is too small for the destination type, `z` is zero.</span></span>

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="6aeee-1767">\+ y</span><span class="sxs-lookup"><span data-stu-id="6aeee-1767">+y</span></span>   | <span data-ttu-id="6aeee-1768">-y</span><span class="sxs-lookup"><span data-stu-id="6aeee-1768">-y</span></span>   | <span data-ttu-id="6aeee-1769">+0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1769">+0</span></span>   | <span data-ttu-id="6aeee-1770">-0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1770">-0</span></span>   | <span data-ttu-id="6aeee-1771">+inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1771">+inf</span></span> | <span data-ttu-id="6aeee-1772">-inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1772">-inf</span></span> | <span data-ttu-id="6aeee-1773">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1773">NaN</span></span>  | 
   | <span data-ttu-id="6aeee-1774">+x</span><span class="sxs-lookup"><span data-stu-id="6aeee-1774">+x</span></span>   | <span data-ttu-id="6aeee-1775">+z</span><span class="sxs-lookup"><span data-stu-id="6aeee-1775">+z</span></span>   | <span data-ttu-id="6aeee-1776">-z</span><span class="sxs-lookup"><span data-stu-id="6aeee-1776">-z</span></span>   | <span data-ttu-id="6aeee-1777">+inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1777">+inf</span></span> | <span data-ttu-id="6aeee-1778">-inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1778">-inf</span></span> | <span data-ttu-id="6aeee-1779">+0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1779">+0</span></span>   | <span data-ttu-id="6aeee-1780">-0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1780">-0</span></span>   | <span data-ttu-id="6aeee-1781">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1781">NaN</span></span>  | 
   | <span data-ttu-id="6aeee-1782">-x</span><span class="sxs-lookup"><span data-stu-id="6aeee-1782">-x</span></span>   | <span data-ttu-id="6aeee-1783">-z</span><span class="sxs-lookup"><span data-stu-id="6aeee-1783">-z</span></span>   | <span data-ttu-id="6aeee-1784">+z</span><span class="sxs-lookup"><span data-stu-id="6aeee-1784">+z</span></span>   | <span data-ttu-id="6aeee-1785">-inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1785">-inf</span></span> | <span data-ttu-id="6aeee-1786">+inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1786">+inf</span></span> | <span data-ttu-id="6aeee-1787">-0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1787">-0</span></span>   | <span data-ttu-id="6aeee-1788">+0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1788">+0</span></span>   | <span data-ttu-id="6aeee-1789">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1789">NaN</span></span>  | 
   | <span data-ttu-id="6aeee-1790">+0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1790">+0</span></span>   | <span data-ttu-id="6aeee-1791">+0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1791">+0</span></span>   | <span data-ttu-id="6aeee-1792">-0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1792">-0</span></span>   | <span data-ttu-id="6aeee-1793">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1793">NaN</span></span>  | <span data-ttu-id="6aeee-1794">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1794">NaN</span></span>  | <span data-ttu-id="6aeee-1795">+0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1795">+0</span></span>   | <span data-ttu-id="6aeee-1796">-0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1796">-0</span></span>   | <span data-ttu-id="6aeee-1797">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1797">NaN</span></span>  | 
   | <span data-ttu-id="6aeee-1798">-0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1798">-0</span></span>   | <span data-ttu-id="6aeee-1799">-0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1799">-0</span></span>   | <span data-ttu-id="6aeee-1800">+0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1800">+0</span></span>   | <span data-ttu-id="6aeee-1801">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1801">NaN</span></span>  | <span data-ttu-id="6aeee-1802">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1802">NaN</span></span>  | <span data-ttu-id="6aeee-1803">-0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1803">-0</span></span>   | <span data-ttu-id="6aeee-1804">+0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1804">+0</span></span>   | <span data-ttu-id="6aeee-1805">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1805">NaN</span></span>  | 
   | <span data-ttu-id="6aeee-1806">+inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1806">+inf</span></span> | <span data-ttu-id="6aeee-1807">+inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1807">+inf</span></span> | <span data-ttu-id="6aeee-1808">-inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1808">-inf</span></span> | <span data-ttu-id="6aeee-1809">+inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1809">+inf</span></span> | <span data-ttu-id="6aeee-1810">-inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1810">-inf</span></span> | <span data-ttu-id="6aeee-1811">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1811">NaN</span></span>  | <span data-ttu-id="6aeee-1812">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1812">NaN</span></span>  | <span data-ttu-id="6aeee-1813">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1813">NaN</span></span>  | 
   | <span data-ttu-id="6aeee-1814">-inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1814">-inf</span></span> | <span data-ttu-id="6aeee-1815">-inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1815">-inf</span></span> | <span data-ttu-id="6aeee-1816">+inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1816">+inf</span></span> | <span data-ttu-id="6aeee-1817">-inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1817">-inf</span></span> | <span data-ttu-id="6aeee-1818">+inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1818">+inf</span></span> | <span data-ttu-id="6aeee-1819">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1819">NaN</span></span>  | <span data-ttu-id="6aeee-1820">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1820">NaN</span></span>  | <span data-ttu-id="6aeee-1821">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1821">NaN</span></span>  | 
   | <span data-ttu-id="6aeee-1822">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1822">NaN</span></span>  | <span data-ttu-id="6aeee-1823">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1823">NaN</span></span>  | <span data-ttu-id="6aeee-1824">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1824">NaN</span></span>  | <span data-ttu-id="6aeee-1825">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1825">NaN</span></span>  | <span data-ttu-id="6aeee-1826">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1826">NaN</span></span>  | <span data-ttu-id="6aeee-1827">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1827">NaN</span></span>  | <span data-ttu-id="6aeee-1828">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1828">NaN</span></span>  | <span data-ttu-id="6aeee-1829">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1829">NaN</span></span>  | 

*  <span data-ttu-id="6aeee-1830">小數除法：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1830">Decimal division:</span></span>

   ```csharp
   decimal operator /(decimal x, decimal y);
   ```

   <span data-ttu-id="6aeee-1831">如果右運算元的值為零，則會擲回 @no__t 0。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1831">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span> <span data-ttu-id="6aeee-1832">如果產生的值太大，而無法以 `decimal` 格式表示，則會擲回 `System.OverflowException`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1832">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="6aeee-1833">如果結果值太小而無法以 `decimal` 格式表示，則結果會是零。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1833">If the result value is too small to represent in the `decimal` format, the result is zero.</span></span> <span data-ttu-id="6aeee-1834">結果的規模是最小的尺規，會將結果與最接近的可顯示十進位值保持為實際的數學結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1834">The scale of the result is the smallest scale that will preserve a result equal to the nearest representable decimal value to the true mathematical result.</span></span>

   <span data-ttu-id="6aeee-1835">小數除法相當於使用 `System.Decimal` 類型的除法運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1835">Decimal division is equivalent to using the division operator of type `System.Decimal`.</span></span>


### <a name="remainder-operator"></a><span data-ttu-id="6aeee-1836">餘數運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-1836">Remainder operator</span></span>

<span data-ttu-id="6aeee-1837">若為格式為 `x % y` 的作業，則會套用二元運算子多載解析（[二元運算子](expressions.md#binary-operator-overload-resolution)多載解析）來選取特定的運算子執行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1837">For an operation of the form `x % y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="6aeee-1838">運算元會轉換成所選運算子的參數類型，而結果的類型會是運算子的傳回類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1838">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="6aeee-1839">預先定義的餘數運算子如下所示。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1839">The predefined remainder operators are listed below.</span></span> <span data-ttu-id="6aeee-1840">運算子會計算 `x` 和 `y` 之間相除的餘數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1840">The operators all compute the remainder of the division between `x` and `y`.</span></span>

*  <span data-ttu-id="6aeee-1841">整數餘數：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1841">Integer remainder:</span></span>

   ```csharp
   int operator %(int x, int y);
   uint operator %(uint x, uint y);
   long operator %(long x, long y);
   ulong operator %(ulong x, ulong y);
   ```

   <span data-ttu-id="6aeee-1842">@No__t-0 的結果是由 `x - (x / y) * y` 產生的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1842">The result of `x % y` is the value produced by `x - (x / y) * y`.</span></span> <span data-ttu-id="6aeee-1843">如果 `y` 為零，則會擲回 `System.DivideByZeroException`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1843">If `y` is zero, a `System.DivideByZeroException` is thrown.</span></span>

   <span data-ttu-id="6aeee-1844">如果左運算元是最小的 `int` 或 `long` 值，右運算元是 `-1`，則會擲回 `System.OverflowException`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1844">If the left operand is the smallest `int` or `long` value and the right operand is `-1`, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="6aeee-1845">在沒有任何情況下 `x % y` 會擲回例外狀況，其中 `x / y` 不會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1845">In no case does `x % y` throw an exception where `x / y` would not throw an exception.</span></span>

*  <span data-ttu-id="6aeee-1846">浮點餘數：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1846">Floating-point remainder:</span></span>

   ```csharp
   float operator %(float x, float y);
   double operator %(double x, double y);
   ```

   <span data-ttu-id="6aeee-1847">下表列出非零的有限值、零、無限大和 NaN 的所有可能組合的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1847">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="6aeee-1848">在資料表中，`x`，而 `y` 是正的有限值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1848">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="6aeee-1849">`z` 是 `x % y` 的結果，並計算為 `x - n * y`，其中 `n` 是小於或等於 `x / y` 的最大可能整數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1849">`z` is the result of `x % y` and is computed as `x - n * y`, where `n` is the largest possible integer that is less than or equal to `x / y`.</span></span> <span data-ttu-id="6aeee-1850">這種計算餘數的方法類似于用於整數運算元的，但與 IEEE 754 定義不同（其中 `n` 是最接近 `x / y` 的整數）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1850">This method of computing the remainder is analogous to that used for integer operands, but differs from the IEEE 754 definition (in which `n` is the integer closest to `x / y`).</span></span>

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="6aeee-1851">\+ y</span><span class="sxs-lookup"><span data-stu-id="6aeee-1851">+y</span></span>   | <span data-ttu-id="6aeee-1852">-y</span><span class="sxs-lookup"><span data-stu-id="6aeee-1852">-y</span></span>   | <span data-ttu-id="6aeee-1853">+0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1853">+0</span></span>   | <span data-ttu-id="6aeee-1854">-0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1854">-0</span></span>   | <span data-ttu-id="6aeee-1855">+inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1855">+inf</span></span> | <span data-ttu-id="6aeee-1856">-inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1856">-inf</span></span> | <span data-ttu-id="6aeee-1857">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1857">NaN</span></span>  | 
   | <span data-ttu-id="6aeee-1858">+x</span><span class="sxs-lookup"><span data-stu-id="6aeee-1858">+x</span></span>   | <span data-ttu-id="6aeee-1859">+z</span><span class="sxs-lookup"><span data-stu-id="6aeee-1859">+z</span></span>   | <span data-ttu-id="6aeee-1860">+z</span><span class="sxs-lookup"><span data-stu-id="6aeee-1860">+z</span></span>   | <span data-ttu-id="6aeee-1861">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1861">NaN</span></span>  | <span data-ttu-id="6aeee-1862">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1862">NaN</span></span>  | <span data-ttu-id="6aeee-1863">x</span><span class="sxs-lookup"><span data-stu-id="6aeee-1863">x</span></span>    | <span data-ttu-id="6aeee-1864">x</span><span class="sxs-lookup"><span data-stu-id="6aeee-1864">x</span></span>    | <span data-ttu-id="6aeee-1865">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1865">NaN</span></span>  | 
   | <span data-ttu-id="6aeee-1866">-x</span><span class="sxs-lookup"><span data-stu-id="6aeee-1866">-x</span></span>   | <span data-ttu-id="6aeee-1867">-z</span><span class="sxs-lookup"><span data-stu-id="6aeee-1867">-z</span></span>   | <span data-ttu-id="6aeee-1868">-z</span><span class="sxs-lookup"><span data-stu-id="6aeee-1868">-z</span></span>   | <span data-ttu-id="6aeee-1869">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1869">NaN</span></span>  | <span data-ttu-id="6aeee-1870">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1870">NaN</span></span>  | <span data-ttu-id="6aeee-1871">-x</span><span class="sxs-lookup"><span data-stu-id="6aeee-1871">-x</span></span>   | <span data-ttu-id="6aeee-1872">-x</span><span class="sxs-lookup"><span data-stu-id="6aeee-1872">-x</span></span>   | <span data-ttu-id="6aeee-1873">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1873">NaN</span></span>  | 
   | <span data-ttu-id="6aeee-1874">+0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1874">+0</span></span>   | <span data-ttu-id="6aeee-1875">+0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1875">+0</span></span>   | <span data-ttu-id="6aeee-1876">+0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1876">+0</span></span>   | <span data-ttu-id="6aeee-1877">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1877">NaN</span></span>  | <span data-ttu-id="6aeee-1878">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1878">NaN</span></span>  | <span data-ttu-id="6aeee-1879">+0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1879">+0</span></span>   | <span data-ttu-id="6aeee-1880">+0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1880">+0</span></span>   | <span data-ttu-id="6aeee-1881">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1881">NaN</span></span>  | 
   | <span data-ttu-id="6aeee-1882">-0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1882">-0</span></span>   | <span data-ttu-id="6aeee-1883">-0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1883">-0</span></span>   | <span data-ttu-id="6aeee-1884">-0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1884">-0</span></span>   | <span data-ttu-id="6aeee-1885">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1885">NaN</span></span>  | <span data-ttu-id="6aeee-1886">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1886">NaN</span></span>  | <span data-ttu-id="6aeee-1887">-0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1887">-0</span></span>   | <span data-ttu-id="6aeee-1888">-0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1888">-0</span></span>   | <span data-ttu-id="6aeee-1889">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1889">NaN</span></span>  | 
   | <span data-ttu-id="6aeee-1890">+inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1890">+inf</span></span> | <span data-ttu-id="6aeee-1891">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1891">NaN</span></span>  | <span data-ttu-id="6aeee-1892">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1892">NaN</span></span>  | <span data-ttu-id="6aeee-1893">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1893">NaN</span></span>  | <span data-ttu-id="6aeee-1894">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1894">NaN</span></span>  | <span data-ttu-id="6aeee-1895">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1895">NaN</span></span>  | <span data-ttu-id="6aeee-1896">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1896">NaN</span></span>  | <span data-ttu-id="6aeee-1897">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1897">NaN</span></span>  | 
   | <span data-ttu-id="6aeee-1898">-inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1898">-inf</span></span> | <span data-ttu-id="6aeee-1899">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1899">NaN</span></span>  | <span data-ttu-id="6aeee-1900">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1900">NaN</span></span>  | <span data-ttu-id="6aeee-1901">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1901">NaN</span></span>  | <span data-ttu-id="6aeee-1902">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1902">NaN</span></span>  | <span data-ttu-id="6aeee-1903">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1903">NaN</span></span>  | <span data-ttu-id="6aeee-1904">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1904">NaN</span></span>  | <span data-ttu-id="6aeee-1905">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1905">NaN</span></span>  | 
   | <span data-ttu-id="6aeee-1906">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1906">NaN</span></span>  | <span data-ttu-id="6aeee-1907">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1907">NaN</span></span>  | <span data-ttu-id="6aeee-1908">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1908">NaN</span></span>  | <span data-ttu-id="6aeee-1909">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1909">NaN</span></span>  | <span data-ttu-id="6aeee-1910">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1910">NaN</span></span>  | <span data-ttu-id="6aeee-1911">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1911">NaN</span></span>  | <span data-ttu-id="6aeee-1912">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1912">NaN</span></span>  | <span data-ttu-id="6aeee-1913">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1913">NaN</span></span>  | 

*  <span data-ttu-id="6aeee-1914">小數餘數：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1914">Decimal remainder:</span></span>

   ```csharp
   decimal operator %(decimal x, decimal y);
   ```

   <span data-ttu-id="6aeee-1915">如果右運算元的值為零，則會擲回 @no__t 0。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1915">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span> <span data-ttu-id="6aeee-1916">在任何進位之前，結果的小數值會大於兩個運算元的刻度，而結果的正負號（如果不是零）則與 `x` 相同。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1916">The scale of the result, before any rounding, is the larger of the scales of the two operands, and the sign of the result, if non-zero, is the same as that of `x`.</span></span>

   <span data-ttu-id="6aeee-1917">Decimal 餘數相當於使用 `System.Decimal` 類型的餘數運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1917">Decimal remainder is equivalent to using the remainder operator of type `System.Decimal`.</span></span>


### <a name="addition-operator"></a><span data-ttu-id="6aeee-1918">加法運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-1918">Addition operator</span></span>

<span data-ttu-id="6aeee-1919">若為格式為 `x + y` 的作業，則會套用二元運算子多載解析（[二元運算子](expressions.md#binary-operator-overload-resolution)多載解析）來選取特定的運算子執行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1919">For an operation of the form `x + y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="6aeee-1920">運算元會轉換成所選運算子的參數類型，而結果的類型會是運算子的傳回類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1920">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="6aeee-1921">預先定義的加法運算子如下所示。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1921">The predefined addition operators are listed below.</span></span> <span data-ttu-id="6aeee-1922">針對數值和列舉類型，預先定義的加法運算子會計算兩個運算元的總和。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1922">For numeric and enumeration types, the predefined addition operators compute the sum of the two operands.</span></span> <span data-ttu-id="6aeee-1923">當其中一個或兩個運算元的類型為 string 時，預先定義的加法運算子會串連運算元的字串標記法。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1923">When one or both operands are of type string, the predefined addition operators concatenate the string representation of the operands.</span></span>

*  <span data-ttu-id="6aeee-1924">整數加法：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1924">Integer addition:</span></span>

   ```csharp
   int operator +(int x, int y);
   uint operator +(uint x, uint y);
   long operator +(long x, long y);
   ulong operator +(ulong x, ulong y);
   ```

   <span data-ttu-id="6aeee-1925">在 `checked` 內容中，如果總和超出結果類型的範圍，則會擲回 `System.OverflowException`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1925">In a `checked` context, if the sum is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="6aeee-1926">在 `unchecked` 內容中，不會報告溢位，且會捨棄超出結果型別範圍的任何重要高序位位。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1926">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>

*  <span data-ttu-id="6aeee-1927">浮點加法：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1927">Floating-point addition:</span></span>

   ```csharp
   float operator +(float x, float y);
   double operator +(double x, double y);
   ```

   <span data-ttu-id="6aeee-1928">總和是根據 IEEE 754 算術的規則來計算。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1928">The sum is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="6aeee-1929">下表列出非零的有限值、零、無限大和 NaN 的所有可能組合的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1929">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="6aeee-1930">在資料表中，`x` 和 `y` 是非零的有限值，而 `z` 則是 `x + y` 的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1930">In the table, `x` and `y` are nonzero finite values, and `z` is the result of `x + y`.</span></span> <span data-ttu-id="6aeee-1931">如果 `x` 和 `y` 具有相同的大小，但正負號相反，`z` 則為正數零。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1931">If `x` and `y` have the same magnitude but opposite signs, `z` is positive zero.</span></span> <span data-ttu-id="6aeee-1932">如果 `x + y` 太大而無法在目的地類型中呈現，`z` 就是具有與 `x + y` 相同正負號的無限大。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1932">If `x + y` is too large to represent in the destination type, `z` is an infinity with the same sign as `x + y`.</span></span>

   |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="6aeee-1933">y</span><span class="sxs-lookup"><span data-stu-id="6aeee-1933">y</span></span>    | <span data-ttu-id="6aeee-1934">+0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1934">+0</span></span>   | <span data-ttu-id="6aeee-1935">-0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1935">-0</span></span>   | <span data-ttu-id="6aeee-1936">+inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1936">+inf</span></span> | <span data-ttu-id="6aeee-1937">-inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1937">-inf</span></span> | <span data-ttu-id="6aeee-1938">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1938">NaN</span></span>  | 
   | <span data-ttu-id="6aeee-1939">x</span><span class="sxs-lookup"><span data-stu-id="6aeee-1939">x</span></span>    | <span data-ttu-id="6aeee-1940">z</span><span class="sxs-lookup"><span data-stu-id="6aeee-1940">z</span></span>    | <span data-ttu-id="6aeee-1941">x</span><span class="sxs-lookup"><span data-stu-id="6aeee-1941">x</span></span>    | <span data-ttu-id="6aeee-1942">x</span><span class="sxs-lookup"><span data-stu-id="6aeee-1942">x</span></span>    | <span data-ttu-id="6aeee-1943">+inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1943">+inf</span></span> | <span data-ttu-id="6aeee-1944">-inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1944">-inf</span></span> | <span data-ttu-id="6aeee-1945">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1945">NaN</span></span>  | 
   | <span data-ttu-id="6aeee-1946">+0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1946">+0</span></span>   | <span data-ttu-id="6aeee-1947">y</span><span class="sxs-lookup"><span data-stu-id="6aeee-1947">y</span></span>    | <span data-ttu-id="6aeee-1948">+0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1948">+0</span></span>   | <span data-ttu-id="6aeee-1949">+0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1949">+0</span></span>   | <span data-ttu-id="6aeee-1950">+inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1950">+inf</span></span> | <span data-ttu-id="6aeee-1951">-inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1951">-inf</span></span> | <span data-ttu-id="6aeee-1952">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1952">NaN</span></span>  | 
   | <span data-ttu-id="6aeee-1953">-0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1953">-0</span></span>   | <span data-ttu-id="6aeee-1954">y</span><span class="sxs-lookup"><span data-stu-id="6aeee-1954">y</span></span>    | <span data-ttu-id="6aeee-1955">+0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1955">+0</span></span>   | <span data-ttu-id="6aeee-1956">-0</span><span class="sxs-lookup"><span data-stu-id="6aeee-1956">-0</span></span>   | <span data-ttu-id="6aeee-1957">+inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1957">+inf</span></span> | <span data-ttu-id="6aeee-1958">-inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1958">-inf</span></span> | <span data-ttu-id="6aeee-1959">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1959">NaN</span></span>  | 
   | <span data-ttu-id="6aeee-1960">+inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1960">+inf</span></span> | <span data-ttu-id="6aeee-1961">+inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1961">+inf</span></span> | <span data-ttu-id="6aeee-1962">+inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1962">+inf</span></span> | <span data-ttu-id="6aeee-1963">+inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1963">+inf</span></span> | <span data-ttu-id="6aeee-1964">+inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1964">+inf</span></span> | <span data-ttu-id="6aeee-1965">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1965">NaN</span></span>  | <span data-ttu-id="6aeee-1966">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1966">NaN</span></span>  | 
   | <span data-ttu-id="6aeee-1967">-inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1967">-inf</span></span> | <span data-ttu-id="6aeee-1968">-inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1968">-inf</span></span> | <span data-ttu-id="6aeee-1969">-inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1969">-inf</span></span> | <span data-ttu-id="6aeee-1970">-inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1970">-inf</span></span> | <span data-ttu-id="6aeee-1971">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1971">NaN</span></span>  | <span data-ttu-id="6aeee-1972">-inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-1972">-inf</span></span> | <span data-ttu-id="6aeee-1973">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1973">NaN</span></span>  | 
   | <span data-ttu-id="6aeee-1974">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1974">NaN</span></span>  | <span data-ttu-id="6aeee-1975">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1975">NaN</span></span>  | <span data-ttu-id="6aeee-1976">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1976">NaN</span></span>  | <span data-ttu-id="6aeee-1977">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1977">NaN</span></span>  | <span data-ttu-id="6aeee-1978">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1978">NaN</span></span>  | <span data-ttu-id="6aeee-1979">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1979">NaN</span></span>  | <span data-ttu-id="6aeee-1980">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-1980">NaN</span></span>  | 

*  <span data-ttu-id="6aeee-1981">十進位加法：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1981">Decimal addition:</span></span>

   ```csharp
   decimal operator +(decimal x, decimal y);
   ```

   <span data-ttu-id="6aeee-1982">如果產生的值太大，而無法以 `decimal` 格式表示，則會擲回 `System.OverflowException`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1982">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="6aeee-1983">在任何進位之前，結果的小數值會大於兩個運算元的刻度。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1983">The scale of the result, before any rounding, is the larger of the scales of the two operands.</span></span>

   <span data-ttu-id="6aeee-1984">Decimal 加法相當於使用類型 `System.Decimal` 的加法運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1984">Decimal addition is equivalent to using the addition operator of type `System.Decimal`.</span></span>

*  <span data-ttu-id="6aeee-1985">列舉加法。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1985">Enumeration addition.</span></span> <span data-ttu-id="6aeee-1986">每個列舉型別都會隱含提供下列預先定義的運算子，其中 `E` 是列舉型別，而 `U` 是 `E` 的基礎型別：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1986">Every enumeration type implicitly provides the following predefined operators, where `E` is the enum type, and `U` is the underlying type of `E`:</span></span>

   ```csharp
   E operator +(E x, U y);
   E operator +(U x, E y);
   ```

   <span data-ttu-id="6aeee-1987">在執行時間，這些運算子會完全依照 `(E)((U)x + (U)y)` 進行評估。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1987">At run-time these operators are evaluated exactly as `(E)((U)x + (U)y)`.</span></span>

*  <span data-ttu-id="6aeee-1988">字串串連：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1988">String concatenation:</span></span>

   ```csharp
   string operator +(string x, string y);
   string operator +(string x, object y);
   string operator +(object x, string y);
   ```

   <span data-ttu-id="6aeee-1989">Binary `+` 運算子的這些多載會執行字串串連。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1989">These overloads of the binary `+` operator perform string concatenation.</span></span> <span data-ttu-id="6aeee-1990">如果字串串連的運算元 `null`，則會替代空字串。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1990">If an operand of string concatenation is `null`, an empty string is substituted.</span></span> <span data-ttu-id="6aeee-1991">否則，任何非字串引數都會藉由叫用從類型 `object` 繼承的虛擬 `ToString` 方法，轉換成其字串表示。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1991">Otherwise, any non-string argument is converted to its string representation by invoking the virtual `ToString` method inherited from type `object`.</span></span> <span data-ttu-id="6aeee-1992">如果 `ToString` 傳回 `null`，則會替代空字串。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1992">If `ToString` returns `null`, an empty string is substituted.</span></span>

   ```csharp
   using System;
   
   class Test
   {
       static void Main() {
           string s = null;
           Console.WriteLine("s = >" + s + "<");        // displays s = ><
           int i = 1;
           Console.WriteLine("i = " + i);               // displays i = 1
           float f = 1.2300E+15F;
           Console.WriteLine("f = " + f);               // displays f = 1.23E+15
           decimal d = 2.900m;
           Console.WriteLine("d = " + d);               // displays d = 2.900
       }
   }
   ```

   字串串連運算子的結果是由左運算元的字元，後面接著右運算元的字元所組成的字串。 字串串連運算子絕對不會傳回 0 @no__t 值。 <span data-ttu-id="6aeee-1995">如果沒有足夠的記憶體可用來配置產生的字串，則可能會擲回 `System.OutOfMemoryException`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1995">A `System.OutOfMemoryException` may be thrown if there is not enough memory available to allocate the resulting string.</span></span>

*  <span data-ttu-id="6aeee-1996">委派組合。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1996">Delegate combination.</span></span> <span data-ttu-id="6aeee-1997">每個委派類型都會隱含提供下列預先定義的運算子，其中 `D` 是委派類型：</span><span class="sxs-lookup"><span data-stu-id="6aeee-1997">Every delegate type implicitly provides the following predefined operator, where `D` is the delegate type:</span></span>

   ```csharp
   D operator +(D x, D y);
   ```

   <span data-ttu-id="6aeee-1998">二元 `+` 運算子會在兩個運算元都屬於 `D` 的某些委派類型時，執行委派組合。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1998">The binary `+` operator performs delegate combination when both operands are of some delegate type `D`.</span></span> <span data-ttu-id="6aeee-1999">（如果運算元有不同的委派類型，就會發生系結時錯誤）。如果第一個運算元 `null`，則作業的結果會是第二個運算元的值（即使也是 `null`）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-1999">(If the operands have different delegate types, a binding-time error occurs.) If the first operand is `null`, the result of the operation is the value of the second operand (even if that is also `null`).</span></span> <span data-ttu-id="6aeee-2000">否則，如果第二個運算元 `null`，則作業的結果會是第一個運算元的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2000">Otherwise, if the second operand is `null`, then the result of the operation is the value of the first operand.</span></span> <span data-ttu-id="6aeee-2001">否則，作業的結果會是新的委派實例，當叫用時，會叫用第一個運算元，然後叫用第二個運算元。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2001">Otherwise, the result of the operation is a new delegate instance that, when invoked, invokes the first operand and then invokes the second operand.</span></span> <span data-ttu-id="6aeee-2002">如需委派組合的範例，請參閱[減法運算子](expressions.md#subtraction-operator)和[委派調用](delegates.md#delegate-invocation)。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2002">For examples of delegate combination, see [Subtraction operator](expressions.md#subtraction-operator) and [Delegate invocation](delegates.md#delegate-invocation).</span></span> <span data-ttu-id="6aeee-2003">因為 `System.Delegate` 不是委派類型，所以不會為其定義 `operator` @ no__t-2。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2003">Since `System.Delegate` is not a delegate type, `operator` `+` is not defined for it.</span></span>

### <a name="subtraction-operator"></a><span data-ttu-id="6aeee-2004">減法運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-2004">Subtraction operator</span></span>

<span data-ttu-id="6aeee-2005">若為格式為 `x - y` 的作業，則會套用二元運算子多載解析（[二元運算子](expressions.md#binary-operator-overload-resolution)多載解析）來選取特定的運算子執行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2005">For an operation of the form `x - y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="6aeee-2006">運算元會轉換成所選運算子的參數類型，而結果的類型會是運算子的傳回類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2006">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="6aeee-2007">預先定義的減法運算子如下所示。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2007">The predefined subtraction operators are listed below.</span></span> <span data-ttu-id="6aeee-2008">運算子全都會從 `x` 減去 `y`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2008">The operators all subtract `y` from `x`.</span></span>

*  <span data-ttu-id="6aeee-2009">整數減法：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2009">Integer subtraction:</span></span>

   ```csharp
   int operator -(int x, int y);
   uint operator -(uint x, uint y);
   long operator -(long x, long y);
   ulong operator -(ulong x, ulong y);
   ```

   <span data-ttu-id="6aeee-2010">在 @no__t 0 內容中，如果差異超出結果類型的範圍，則會擲回 `System.OverflowException`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2010">In a `checked` context, if the difference is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="6aeee-2011">在 `unchecked` 內容中，不會報告溢位，且會捨棄超出結果型別範圍的任何重要高序位位。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2011">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>

*  <span data-ttu-id="6aeee-2012">浮點減法：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2012">Floating-point subtraction:</span></span>

   ```csharp
   float operator -(float x, float y);
   double operator -(double x, double y);
   ```

   <span data-ttu-id="6aeee-2013">差異是根據 IEEE 754 算術的規則來計算。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2013">The difference is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="6aeee-2014">下表列出非零的有限值、零、無限大和 Nan 的所有可能組合的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2014">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaNs.</span></span> <span data-ttu-id="6aeee-2015">在資料表中，`x` 和 `y` 是非零的有限值，而 `z` 則是 `x - y` 的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2015">In the table, `x` and `y` are nonzero finite values, and `z` is the result of `x - y`.</span></span> <span data-ttu-id="6aeee-2016">如果 `x` 且 `y` 相等，`z` 為正零。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2016">If `x` and `y` are equal, `z` is positive zero.</span></span> <span data-ttu-id="6aeee-2017">如果 `x - y` 太大而無法在目的地類型中呈現，`z` 就是具有與 `x - y` 相同正負號的無限大。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2017">If `x - y` is too large to represent in the destination type, `z` is an infinity with the same sign as `x - y`.</span></span>

   |      |      |      |      |      |      |     |
   |:----:|:----:|:----:|:----:|:----:|:----:|:---:|
   |      | <span data-ttu-id="6aeee-2018">y</span><span class="sxs-lookup"><span data-stu-id="6aeee-2018">y</span></span>    | <span data-ttu-id="6aeee-2019">+0</span><span class="sxs-lookup"><span data-stu-id="6aeee-2019">+0</span></span>   | <span data-ttu-id="6aeee-2020">-0</span><span class="sxs-lookup"><span data-stu-id="6aeee-2020">-0</span></span>   | <span data-ttu-id="6aeee-2021">+inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-2021">+inf</span></span> | <span data-ttu-id="6aeee-2022">-inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-2022">-inf</span></span> | <span data-ttu-id="6aeee-2023">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-2023">NaN</span></span> | 
   | <span data-ttu-id="6aeee-2024">x</span><span class="sxs-lookup"><span data-stu-id="6aeee-2024">x</span></span>    | <span data-ttu-id="6aeee-2025">z</span><span class="sxs-lookup"><span data-stu-id="6aeee-2025">z</span></span>    | <span data-ttu-id="6aeee-2026">x</span><span class="sxs-lookup"><span data-stu-id="6aeee-2026">x</span></span>    | <span data-ttu-id="6aeee-2027">x</span><span class="sxs-lookup"><span data-stu-id="6aeee-2027">x</span></span>    | <span data-ttu-id="6aeee-2028">-inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-2028">-inf</span></span> | <span data-ttu-id="6aeee-2029">+inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-2029">+inf</span></span> | <span data-ttu-id="6aeee-2030">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-2030">NaN</span></span> | 
   | <span data-ttu-id="6aeee-2031">+0</span><span class="sxs-lookup"><span data-stu-id="6aeee-2031">+0</span></span>   | <span data-ttu-id="6aeee-2032">-y</span><span class="sxs-lookup"><span data-stu-id="6aeee-2032">-y</span></span>   | <span data-ttu-id="6aeee-2033">+0</span><span class="sxs-lookup"><span data-stu-id="6aeee-2033">+0</span></span>   | <span data-ttu-id="6aeee-2034">+0</span><span class="sxs-lookup"><span data-stu-id="6aeee-2034">+0</span></span>   | <span data-ttu-id="6aeee-2035">-inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-2035">-inf</span></span> | <span data-ttu-id="6aeee-2036">+inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-2036">+inf</span></span> | <span data-ttu-id="6aeee-2037">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-2037">NaN</span></span> | 
   | <span data-ttu-id="6aeee-2038">-0</span><span class="sxs-lookup"><span data-stu-id="6aeee-2038">-0</span></span>   | <span data-ttu-id="6aeee-2039">-y</span><span class="sxs-lookup"><span data-stu-id="6aeee-2039">-y</span></span>   | <span data-ttu-id="6aeee-2040">-0</span><span class="sxs-lookup"><span data-stu-id="6aeee-2040">-0</span></span>   | <span data-ttu-id="6aeee-2041">+0</span><span class="sxs-lookup"><span data-stu-id="6aeee-2041">+0</span></span>   | <span data-ttu-id="6aeee-2042">-inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-2042">-inf</span></span> | <span data-ttu-id="6aeee-2043">+inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-2043">+inf</span></span> | <span data-ttu-id="6aeee-2044">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-2044">NaN</span></span> | 
   | <span data-ttu-id="6aeee-2045">+inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-2045">+inf</span></span> | <span data-ttu-id="6aeee-2046">+inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-2046">+inf</span></span> | <span data-ttu-id="6aeee-2047">+inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-2047">+inf</span></span> | <span data-ttu-id="6aeee-2048">+inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-2048">+inf</span></span> | <span data-ttu-id="6aeee-2049">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-2049">NaN</span></span>  | <span data-ttu-id="6aeee-2050">+inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-2050">+inf</span></span> | <span data-ttu-id="6aeee-2051">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-2051">NaN</span></span> | 
   | <span data-ttu-id="6aeee-2052">-inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-2052">-inf</span></span> | <span data-ttu-id="6aeee-2053">-inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-2053">-inf</span></span> | <span data-ttu-id="6aeee-2054">-inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-2054">-inf</span></span> | <span data-ttu-id="6aeee-2055">-inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-2055">-inf</span></span> | <span data-ttu-id="6aeee-2056">-inf</span><span class="sxs-lookup"><span data-stu-id="6aeee-2056">-inf</span></span> | <span data-ttu-id="6aeee-2057">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-2057">NaN</span></span>  | <span data-ttu-id="6aeee-2058">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-2058">NaN</span></span> | 
   | <span data-ttu-id="6aeee-2059">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-2059">NaN</span></span>  | <span data-ttu-id="6aeee-2060">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-2060">NaN</span></span>  | <span data-ttu-id="6aeee-2061">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-2061">NaN</span></span>  | <span data-ttu-id="6aeee-2062">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-2062">NaN</span></span>  | <span data-ttu-id="6aeee-2063">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-2063">NaN</span></span>  | <span data-ttu-id="6aeee-2064">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-2064">NaN</span></span>  | <span data-ttu-id="6aeee-2065">NaN</span><span class="sxs-lookup"><span data-stu-id="6aeee-2065">NaN</span></span> | 

*  <span data-ttu-id="6aeee-2066">十進位減法：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2066">Decimal subtraction:</span></span>

   ```csharp
   decimal operator -(decimal x, decimal y);
   ```

   <span data-ttu-id="6aeee-2067">如果產生的值太大，而無法以 `decimal` 格式表示，則會擲回 `System.OverflowException`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2067">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="6aeee-2068">在任何進位之前，結果的小數值會大於兩個運算元的刻度。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2068">The scale of the result, before any rounding, is the larger of the scales of the two operands.</span></span>

   <span data-ttu-id="6aeee-2069">十進位減法相當於使用類型 `System.Decimal` 的減法運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2069">Decimal subtraction is equivalent to using the subtraction operator of type `System.Decimal`.</span></span>

*  <span data-ttu-id="6aeee-2070">列舉減法。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2070">Enumeration subtraction.</span></span> <span data-ttu-id="6aeee-2071">每個列舉型別都會隱含提供下列預先定義的運算子，其中 `E` 是列舉型別，而 `U` 是 `E` 的基礎型別：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2071">Every enumeration type implicitly provides the following predefined operator, where `E` is the enum type, and `U` is the underlying type of `E`:</span></span>

   ```csharp
   U operator -(E x, E y);
   ```

   <span data-ttu-id="6aeee-2072">這個運算子的評估方式會與 `(U)((U)x - (U)y)` 完全相同。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2072">This operator is evaluated exactly as `(U)((U)x - (U)y)`.</span></span> <span data-ttu-id="6aeee-2073">換句話說，運算子會計算 `x` 的序數值和 `y` 之間的差異，而結果的類型則是列舉的基礎類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2073">In other words, the operator computes the difference between the ordinal values of `x` and `y`, and the type of the result is the underlying type of the enumeration.</span></span>

   ```csharp
   E operator -(E x, U y);
   ```

   <span data-ttu-id="6aeee-2074">這個運算子的評估方式會與 `(E)((U)x - y)` 完全相同。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2074">This operator is evaluated exactly as `(E)((U)x - y)`.</span></span> <span data-ttu-id="6aeee-2075">換句話說，運算子會從列舉的基礎類型減去值，產生列舉的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2075">In other words, the operator subtracts a value from the underlying type of the enumeration, yielding a value of the enumeration.</span></span>

*  <span data-ttu-id="6aeee-2076">委派移除。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2076">Delegate removal.</span></span> <span data-ttu-id="6aeee-2077">每個委派類型都會隱含提供下列預先定義的運算子，其中 `D` 是委派類型：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2077">Every delegate type implicitly provides the following predefined operator, where `D` is the delegate type:</span></span>

   ```csharp
   D operator -(D x, D y);
   ```

   <span data-ttu-id="6aeee-2078">二元 `-` 運算子會在兩個運算元都屬於 `D` 的某些委派類型時，執行委派移除。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2078">The binary `-` operator performs delegate removal when both operands are of some delegate type `D`.</span></span> <span data-ttu-id="6aeee-2079">如果運算元有不同的委派類型，就會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2079">If the operands have different delegate types, a binding-time error occurs.</span></span> <span data-ttu-id="6aeee-2080">如果第一個運算元是 `null`，則作業的結果是 `null`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2080">If the first operand is `null`, the result of the operation is `null`.</span></span> <span data-ttu-id="6aeee-2081">否則，如果第二個運算元 `null`，則作業的結果會是第一個運算元的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2081">Otherwise, if the second operand is `null`, then the result of the operation is the value of the first operand.</span></span> <span data-ttu-id="6aeee-2082">否則，這兩個運算元代表具有一或多個專案的調用清單（[委派](delegates.md#delegate-declarations)宣告），而結果會是新的調用清單，其中包含第一個運算元的清單，其中已移除第二個運算元的專案，但前提是第二個運算元的清單是第一個的適當連續子清單。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2082">Otherwise, both operands represent invocation lists ([Delegate declarations](delegates.md#delegate-declarations)) having one or more entries, and the result is a new invocation list consisting of the first operand's list with the second operand's entries removed from it, provided the second operand's list is a proper contiguous sublist of the first's.</span></span>     <span data-ttu-id="6aeee-2083">（若要判斷子清單是否相等，對應的專案會針對委派等號比較運算子（[委派相等運算子](expressions.md#delegate-equality-operators)）進行比對。）否則，結果會是左運算元的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2083">(To determine sublist equality, corresponding entries are compared as for the delegate equality operator ([Delegate equality operators](expressions.md#delegate-equality-operators)).) Otherwise, the result is the value of the left operand.</span></span> <span data-ttu-id="6aeee-2084">進程中的兩個運算元清單都不會變更。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2084">Neither of the operands' lists is changed in the process.</span></span> <span data-ttu-id="6aeee-2085">如果第二個運算元的清單符合第一個運算元清單中連續專案的多個 sublists，則會移除連續專案的最右邊相符子清單。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2085">If the second operand's list matches multiple sublists of contiguous entries in the first operand's list, the right-most matching sublist of contiguous entries is removed.</span></span> <span data-ttu-id="6aeee-2086">如果移除導致空白清單，則結果是 `null`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2086">If removal results in an empty list, the result is `null`.</span></span> <span data-ttu-id="6aeee-2087">例如:</span><span class="sxs-lookup"><span data-stu-id="6aeee-2087">For example:</span></span>

   ```csharp
   delegate void D(int x);
   
   class C
   {
       public static void M1(int i) { /* ... */ }
       public static void M2(int i) { /* ... */ }
   }

   class Test
   {
       static void Main() { 
           D cd1 = new D(C.M1);
           D cd2 = new D(C.M2);
           D cd3 = cd1 + cd2 + cd2 + cd1;   // M1 + M2 + M2 + M1
           cd3 -= cd1;                      // => M1 + M2 + M2
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd1 + cd2;                // => M2 + M1
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd2 + cd2;                // => M1 + M1
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd2 + cd1;                // => M1 + M2
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd1 + cd1;                // => M1 + M2 + M2 + M1
       }
   }
   ```

## <a name="shift-operators"></a><span data-ttu-id="6aeee-2088">移位運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-2088">Shift operators</span></span>

<span data-ttu-id="6aeee-2089">@No__t-0 和 @no__t 1 運算子可用來執行位移位作業。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2089">The `<<` and `>>` operators are used to perform bit shifting operations.</span></span>

```antlr
shift_expression
    : additive_expression
    | shift_expression '<<' additive_expression
    | shift_expression right_shift additive_expression
    ;
```

<span data-ttu-id="6aeee-2090">如果*shift_expression*的運算元的編譯階段類型 `dynamic`，則運算式會動態繫結（[動態](expressions.md#dynamic-binding)系結）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2090">If an operand of a *shift_expression* has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="6aeee-2091">在此情況下，運算式的編譯時期類型為 `dynamic`，而以下所述的解決將會在執行時間使用具有編譯時間類型 `dynamic` 之運算元的執行時間類型進行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2091">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="6aeee-2092">若為 `x << count` 或 `x >> count` 格式的作業，則會套用二元運算子多載解析（[二元運算子](expressions.md#binary-operator-overload-resolution)多載解析）來選取特定的運算子執行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2092">For an operation of the form `x << count` or `x >> count`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="6aeee-2093">運算元會轉換成所選運算子的參數類型，而結果的類型會是運算子的傳回類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2093">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="6aeee-2094">當宣告多載移位運算子時，第一個運算元的類型必須一律是包含運算子宣告的類別或結構，而第二個運算元的類型必須一律為 `int`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2094">When declaring an overloaded shift operator, the type of the first operand must always be the class or struct containing the operator declaration, and the type of the second operand must always be `int`.</span></span>

<span data-ttu-id="6aeee-2095">預先定義的移位運算子如下所示。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2095">The predefined shift operators are listed below.</span></span>

*  <span data-ttu-id="6aeee-2096">左移：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2096">Shift left:</span></span>

   ```csharp
   int operator <<(int x, int count);
   uint operator <<(uint x, int count);
   long operator <<(long x, int count);
   ulong operator <<(ulong x, int count);
   ```

   <span data-ttu-id="6aeee-2097">@No__t-0 運算子會將 `x` 向所計算的位數向左移位，如下所述。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2097">The `<<` operator shifts `x` left by a number of bits computed as described below.</span></span>

   <span data-ttu-id="6aeee-2098">在 `x` 的結果類型範圍外的高序位位會被捨棄，其餘的位會向左移位，而低序位的空白位位置會設定為零。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2098">The high-order bits outside the range of the result type of `x` are discarded, the remaining bits are shifted left, and the low-order empty bit positions are set to zero.</span></span>

*  <span data-ttu-id="6aeee-2099">右移：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2099">Shift right:</span></span>

   ```csharp
   int operator >>(int x, int count);
   uint operator >>(uint x, int count);
   long operator >>(long x, int count);
   ulong operator >>(ulong x, int count);
   ```

   <span data-ttu-id="6aeee-2100">@No__t-0 運算子會將 `x` 右移數個計算的位數，如下所述。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2100">The `>>` operator shifts `x` right by a number of bits computed as described below.</span></span>

   <span data-ttu-id="6aeee-2101">當 `x` 的類型為 `int` 或 `long` 時，會捨棄 `x` 的低序位位，其餘的位則會向右移，而如果 `x` 是負數，則高序位空白位位置會設定為零。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2101">When `x` is of type `int` or `long`, the low-order bits of `x` are discarded, the remaining bits are shifted right, and the high-order empty bit positions are set to zero if `x` is non-negative and set to one if `x` is negative.</span></span>

   <span data-ttu-id="6aeee-2102">當 `x` 的類型為 `uint` 或 `ulong` 時，會捨棄 `x` 的低序位位，其餘的位則會向右移動，而高序位的空白位位置會設定為零。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2102">When `x` is of type `uint` or `ulong`, the low-order bits of `x` are discarded, the remaining bits are shifted right, and the high-order empty bit positions are set to zero.</span></span>

<span data-ttu-id="6aeee-2103">針對預先定義的運算子，會計算要移位的位數，如下所示：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2103">For the predefined operators, the number of bits to shift is computed as follows:</span></span>

*  <span data-ttu-id="6aeee-2104">當 `x` 的類型是 `int` 或 `uint` 時，移位元數目是由 `count` 的低序位五位所提供。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2104">When the type of `x` is `int` or `uint`, the shift count is given by the low-order five bits of `count`.</span></span> <span data-ttu-id="6aeee-2105">換句話說，位移計數是從 `count & 0x1F` 計算而來。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2105">In other words, the shift count is computed from `count & 0x1F`.</span></span>
*  <span data-ttu-id="6aeee-2106">當 `x` 的類型是 `long` 或 `ulong` 時，移位元數目是由 `count` 的低序位六位所提供。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2106">When the type of `x` is `long` or `ulong`, the shift count is given by the low-order six bits of `count`.</span></span> <span data-ttu-id="6aeee-2107">換句話說，位移計數是從 `count & 0x3F` 計算而來。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2107">In other words, the shift count is computed from `count & 0x3F`.</span></span>

<span data-ttu-id="6aeee-2108">如果產生的移位元數目為零，移位運算子只會傳回 `x` 的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2108">If the resulting shift count is zero, the shift operators simply return the value of `x`.</span></span>

<span data-ttu-id="6aeee-2109">移位作業永遠不會造成溢位，並在 `checked` 和 @no__t 1 內容中產生相同的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2109">Shift operations never cause overflows and produce the same results in `checked` and `unchecked` contexts.</span></span>

<span data-ttu-id="6aeee-2110">當 `>>` 運算子的左運算元屬於帶正負號的整數類資料類型時，運算子會執行算術移位，其中運算元的最高有效位（正負號位）值會傳播至高序位空白位位置。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2110">When the left operand of the `>>` operator is of a signed integral type, the operator performs an arithmetic shift right wherein the value of the most significant bit (the sign bit) of the operand is propagated to the high-order empty bit positions.</span></span> <span data-ttu-id="6aeee-2111">當 `>>` 運算子的左運算元是不帶正負號的整數類資料類型時，運算子會執行邏輯移位，其中高順序的空白位位置一律會設定為零。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2111">When the left operand of the `>>` operator is of an unsigned integral type, the operator performs a logical shift right wherein high-order empty bit positions are always set to zero.</span></span> <span data-ttu-id="6aeee-2112">若要執行從運算元類型推斷而來的相反運算，可以使用明確轉換。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2112">To perform the opposite operation of that inferred from the operand type, explicit casts can be used.</span></span> <span data-ttu-id="6aeee-2113">例如，如果 `x` 是 `int` 類型的變數，則作業 `unchecked((int)((uint)x >> y))` 會執行 `x` 的邏輯移位許可權。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2113">For example, if `x` is a variable of type `int`, the operation `unchecked((int)((uint)x >> y))` performs a logical shift right of `x`.</span></span>

## <a name="relational-and-type-testing-operators"></a><span data-ttu-id="6aeee-2114">關係和類型測試運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-2114">Relational and type-testing operators</span></span>

<span data-ttu-id="6aeee-2115">@No__t-0，`!=`，`<`，`>`，`<=`，`>=`，`is` 和 @no__t 7 運算子稱為關聯式和類型測試運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2115">The `==`, `!=`, `<`, `>`, `<=`, `>=`, `is` and `as` operators are called the relational and type-testing operators.</span></span>

```antlr
relational_expression
    : shift_expression
    | relational_expression '<' shift_expression
    | relational_expression '>' shift_expression
    | relational_expression '<=' shift_expression
    | relational_expression '>=' shift_expression
    | relational_expression 'is' type
    | relational_expression 'as' type
    ;

equality_expression
    : relational_expression
    | equality_expression '==' relational_expression
    | equality_expression '!=' relational_expression
    ;
```

<span data-ttu-id="6aeee-2116">[Is 運算子](expressions.md#the-is-operator)中描述了 `is` 運算子，而 `as` 運算子則會在[as 運算子](expressions.md#the-as-operator)中描述。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2116">The `is` operator is described in [The is operator](expressions.md#the-is-operator) and the `as` operator is described in [The as operator](expressions.md#the-as-operator).</span></span>

<span data-ttu-id="6aeee-2117">@No__t-0，`!=`，`<`，`>`，`<=` 和 @no__t 5 運算子為***比較運算子***。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2117">The `==`, `!=`, `<`, `>`, `<=` and `>=` operators are ***comparison operators***.</span></span>

<span data-ttu-id="6aeee-2118">如果比較運算子的運算元具有編譯時期類型 `dynamic`，則運算式會動態繫結（[動態](expressions.md#dynamic-binding)系結）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2118">If an operand of a comparison operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="6aeee-2119">在此情況下，運算式的編譯時期類型為 `dynamic`，而以下所述的解決將會在執行時間使用具有編譯時間類型 `dynamic` 之運算元的執行時間類型進行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2119">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="6aeee-2120">若為 `x` *op* `y` 格式的作業，其中*op*是比較運算子，則會套用多載解析（[二元運算子](expressions.md#binary-operator-overload-resolution)多載解析）來選取特定的運算子執行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2120">For an operation of the form `x` *op* `y`, where *op* is a comparison operator, overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="6aeee-2121">運算元會轉換成所選運算子的參數類型，而結果的類型會是運算子的傳回類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2121">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="6aeee-2122">下列各節將說明預先定義的比較運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2122">The predefined comparison operators are described in the following sections.</span></span> <span data-ttu-id="6aeee-2123">所有預先定義的比較運算子都會傳回類型 `bool` 的結果，如下表所述。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2123">All predefined comparison operators return a result of type `bool`, as described in the following table.</span></span>


| <span data-ttu-id="6aeee-2124">__運營__</span><span class="sxs-lookup"><span data-stu-id="6aeee-2124">__Operation__</span></span> | <span data-ttu-id="6aeee-2125">__結果__</span><span class="sxs-lookup"><span data-stu-id="6aeee-2125">__Result__</span></span>                                                       |
|---------------|------------------------------------------------------------------|
| `x == y`      | <span data-ttu-id="6aeee-2126">`true`，如果 `x` 等於 `y`，則為，否則為 `false`</span><span class="sxs-lookup"><span data-stu-id="6aeee-2126">`true` if `x` is equal to `y`, `false` otherwise</span></span>                 | 
| `x != y`      | <span data-ttu-id="6aeee-2127">`true`，如果 `x` 不等於 `y`，則為，否則為 `false`</span><span class="sxs-lookup"><span data-stu-id="6aeee-2127">`true` if `x` is not equal to `y`, `false` otherwise</span></span>             | 
| `x < y`       | <span data-ttu-id="6aeee-2128">`true`，如果 `x` 小於 `y`，則為，否則為 `false`</span><span class="sxs-lookup"><span data-stu-id="6aeee-2128">`true` if `x` is less than `y`, `false` otherwise</span></span>                | 
| `x > y`       | <span data-ttu-id="6aeee-2129">`true`，如果 `x` 大於 `y`，則為，否則為 `false`</span><span class="sxs-lookup"><span data-stu-id="6aeee-2129">`true` if `x` is greater than `y`, `false` otherwise</span></span>             | 
| `x <= y`      | <span data-ttu-id="6aeee-2130">`true`，如果 `x` 小於或等於 `y`，則為，否則為 `false`</span><span class="sxs-lookup"><span data-stu-id="6aeee-2130">`true` if `x` is less than or equal to `y`, `false` otherwise</span></span>    | 
| `x >= y`      | <span data-ttu-id="6aeee-2131">`true`，如果 `x` 大於或等於 `y`，則為，否則為 `false`</span><span class="sxs-lookup"><span data-stu-id="6aeee-2131">`true` if `x` is greater than or equal to `y`, `false` otherwise</span></span> | 

### <a name="integer-comparison-operators"></a><span data-ttu-id="6aeee-2132">整數比較運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-2132">Integer comparison operators</span></span>

<span data-ttu-id="6aeee-2133">預先定義的整數比較運算子如下：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2133">The predefined integer comparison operators are:</span></span>
```csharp
bool operator ==(int x, int y);
bool operator ==(uint x, uint y);
bool operator ==(long x, long y);
bool operator ==(ulong x, ulong y);

bool operator !=(int x, int y);
bool operator !=(uint x, uint y);
bool operator !=(long x, long y);
bool operator !=(ulong x, ulong y);

bool operator <(int x, int y);
bool operator <(uint x, uint y);
bool operator <(long x, long y);
bool operator <(ulong x, ulong y);

bool operator >(int x, int y);
bool operator >(uint x, uint y);
bool operator >(long x, long y);
bool operator >(ulong x, ulong y);

bool operator <=(int x, int y);
bool operator <=(uint x, uint y);
bool operator <=(long x, long y);
bool operator <=(ulong x, ulong y);

bool operator >=(int x, int y);
bool operator >=(uint x, uint y);
bool operator >=(long x, long y);
bool operator >=(ulong x, ulong y);
```

<span data-ttu-id="6aeee-2134">這些運算子會比較兩個整數運算元的數值，並傳回表示特定關聯為 `true` 或 `false` 的 @no__t 0 值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2134">Each of these operators compares the numeric values of the two integer operands and returns a `bool` value that indicates whether the particular relation is `true` or `false`.</span></span>

### <a name="floating-point-comparison-operators"></a><span data-ttu-id="6aeee-2135">浮點比較運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-2135">Floating-point comparison operators</span></span>

<span data-ttu-id="6aeee-2136">預先定義的浮點比較運算子如下：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2136">The predefined floating-point comparison operators are:</span></span>
```csharp
bool operator ==(float x, float y);
bool operator ==(double x, double y);

bool operator !=(float x, float y);
bool operator !=(double x, double y);

bool operator <(float x, float y);
bool operator <(double x, double y);

bool operator >(float x, float y);
bool operator >(double x, double y);

bool operator <=(float x, float y);
bool operator <=(double x, double y);

bool operator >=(float x, float y);
bool operator >=(double x, double y);
```

<span data-ttu-id="6aeee-2137">運算子會根據 IEEE 754 標準的規則來比較運算元：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2137">The operators compare the operands according to the rules of the IEEE 754 standard:</span></span>

*  <span data-ttu-id="6aeee-2138">如果任一個運算元是 NaN，則結果為 `true` `!=` 以外的所有運算子都是 `false`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2138">If either operand is NaN, the result is `false` for all operators except `!=`, for which the result is `true`.</span></span> <span data-ttu-id="6aeee-2139">對於任何兩個運算元，`x != y` 一律會產生與 `!(x == y)` 相同的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2139">For any two operands, `x != y` always produces the same result as `!(x == y)`.</span></span> <span data-ttu-id="6aeee-2140">不過，當其中一個或兩個運算元都是 NaN 時，`<`、`>`、`<=` 和 @no__t 3 運算子並不會產生與相反運算子的邏輯否定相同的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2140">However, when one or both operands are NaN, the `<`, `>`, `<=`, and `>=` operators do not produce the same results as the logical negation of the opposite operator.</span></span> <span data-ttu-id="6aeee-2141">例如，如果 `x`，而 `y` 是 NaN，則 `x < y` 會 `false`，但 `!(x >= y)` 為 `true`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2141">For example, if either of `x` and `y` is NaN, then `x < y` is `false`, but `!(x >= y)` is `true`.</span></span>
*  <span data-ttu-id="6aeee-2142">當運算元都不是 NaN 時，運算子會比較兩個浮點運算元的值與排序有關</span><span class="sxs-lookup"><span data-stu-id="6aeee-2142">When neither operand is NaN, the operators compare the values of the two floating-point operands with respect to the ordering</span></span>

   ```csharp
   -inf < -max < ... < -min < -0.0 == +0.0 < +min < ... < +max < +inf
   ```

   <span data-ttu-id="6aeee-2143">其中 `min` 和 `max` 是最小和最大的正有限值，可以用指定的浮點格式表示。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2143">where `min` and `max` are the smallest and largest positive finite values that can be represented in the given floating-point format.</span></span> <span data-ttu-id="6aeee-2144">此順序的值得注意的效果如下：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2144">Notable effects of this ordering are:</span></span>
   * <span data-ttu-id="6aeee-2145">負和正零會視為相等。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2145">Negative and positive zeros are considered equal.</span></span>
   * <span data-ttu-id="6aeee-2146">負無限大會被視為小於所有其他值，但等於另一個負無限大。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2146">A negative infinity is considered less than all other values, but equal to another negative infinity.</span></span>
   * <span data-ttu-id="6aeee-2147">正無限大會視為大於所有其他值，但等於另一個正無限大。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2147">A positive infinity is considered greater than all other values, but equal to another positive infinity.</span></span>

### <a name="decimal-comparison-operators"></a><span data-ttu-id="6aeee-2148">小數比較運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-2148">Decimal comparison operators</span></span>

<span data-ttu-id="6aeee-2149">預先定義的十進位比較運算子如下：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2149">The predefined decimal comparison operators are:</span></span>
```csharp
bool operator ==(decimal x, decimal y);
bool operator !=(decimal x, decimal y);
bool operator <(decimal x, decimal y);
bool operator >(decimal x, decimal y);
bool operator <=(decimal x, decimal y);
bool operator >=(decimal x, decimal y);
```

<span data-ttu-id="6aeee-2150">這些運算子會比較兩個十進位運算元的數值，並傳回表示特定關聯為 `true` 或 `false` 的 @no__t 0 值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2150">Each of these operators compares the numeric values of the two decimal operands and returns a `bool` value that indicates whether the particular relation is `true` or `false`.</span></span> <span data-ttu-id="6aeee-2151">每個小數比較相當於使用類型 `System.Decimal` 的對應關聯式或等號比較運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2151">Each decimal comparison is equivalent to using the corresponding relational or equality operator of type `System.Decimal`.</span></span>

### <a name="boolean-equality-operators"></a><span data-ttu-id="6aeee-2152">布林等號比較運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-2152">Boolean equality operators</span></span>

<span data-ttu-id="6aeee-2153">預先定義的布林等號比較運算子如下：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2153">The predefined boolean equality operators are:</span></span>
```csharp
bool operator ==(bool x, bool y);
bool operator !=(bool x, bool y);
```

<span data-ttu-id="6aeee-2154">如果 `x` 和 `y` 都 `true`，或 `x` 和 `y` 都 `false`，則 `==` 的結果會 `true`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2154">The result of `==` is `true` if both `x` and `y` are `true` or if both `x` and `y` are `false`.</span></span> <span data-ttu-id="6aeee-2155">否則，結果為 `false`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2155">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="6aeee-2156">如果 `x` 和 `y` 都 `true`，或 `x` 和 `y` 都 `false`，則 `!=` 的結果會 `false`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2156">The result of `!=` is `false` if both `x` and `y` are `true` or if both `x` and `y` are `false`.</span></span> <span data-ttu-id="6aeee-2157">否則，結果為 `true`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2157">Otherwise, the result is `true`.</span></span> <span data-ttu-id="6aeee-2158">當運算元的類型為 `bool` 時，`!=` 運算子會產生與 @no__t 2 運算子相同的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2158">When the operands are of type `bool`, the `!=` operator produces the same result as the `^` operator.</span></span>

### <a name="enumeration-comparison-operators"></a><span data-ttu-id="6aeee-2159">列舉比較運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-2159">Enumeration comparison operators</span></span>

<span data-ttu-id="6aeee-2160">每個列舉型別都會隱含提供下列預先定義的比較運算子：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2160">Every enumeration type implicitly provides the following predefined comparison operators:</span></span>
```csharp
bool operator ==(E x, E y);
bool operator !=(E x, E y);
bool operator <(E x, E y);
bool operator >(E x, E y);
bool operator <=(E x, E y);
bool operator >=(E x, E y);
```

<span data-ttu-id="6aeee-2161">評估 `x op y` 的結果，其中 `x` 和 `y` 是列舉 @no__t 類型的運算式，且其基礎類型為 `U`，而 `op` 是其中一個比較運算子，與評估 `((U)x) op ((U)y)` 完全相同。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2161">The result of evaluating `x op y`, where `x` and `y` are expressions of an enumeration type `E` with an underlying type `U`, and `op` is one of the comparison operators, is exactly the same as evaluating `((U)x) op ((U)y)`.</span></span> <span data-ttu-id="6aeee-2162">換句話說，列舉型別比較運算子只會比較兩個運算元的基礎整數值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2162">In other words, the enumeration type comparison operators simply compare the underlying integral values of the two operands.</span></span>

### <a name="reference-type-equality-operators"></a><span data-ttu-id="6aeee-2163">參考型別等號比較運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-2163">Reference type equality operators</span></span>

<span data-ttu-id="6aeee-2164">預先定義的參考型別等號比較運算子如下：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2164">The predefined reference type equality operators are:</span></span>
```csharp
bool operator ==(object x, object y);
bool operator !=(object x, object y);
```

<span data-ttu-id="6aeee-2165">運算子會傳回比較這兩個參考是否相等或不相等的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2165">The operators return the result of comparing the two references for equality or non-equality.</span></span>

<span data-ttu-id="6aeee-2166">由於預先定義的參考型別等號比較運算子接受類型為 `object` 的運算元，因此會套用至未宣告適用 `operator ==` 和 @no__t 2 成員的所有類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2166">Since the predefined reference type equality operators accept operands of type `object`, they apply to all types that do not declare applicable `operator ==` and `operator !=` members.</span></span> <span data-ttu-id="6aeee-2167">相反地，任何適用的使用者定義的等號比較運算子，實際上都會隱藏預先定義的參考型別等號比較運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2167">Conversely, any applicable user-defined equality operators effectively hide the predefined reference type equality operators.</span></span>

<span data-ttu-id="6aeee-2168">預先定義的參考型別等號比較運算子需要下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2168">The predefined reference type equality operators require one of the following:</span></span>

*  <span data-ttu-id="6aeee-2169">這兩個運算元都是已知為*reference_type*或常值 `null` 之類型的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2169">Both operands are a value of a type known to be a *reference_type* or the literal `null`.</span></span> <span data-ttu-id="6aeee-2170">此外，從任一運算元的類型到另一個運算元的類型，都有明確的參考轉換（[明確參考](conversions.md#explicit-reference-conversions)轉換）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2170">Furthermore, an explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) exists from the type of either operand to the type of the other operand.</span></span>
*  <span data-ttu-id="6aeee-2171">一個運算元是類型 @no__t 0 的值，其中 `T` 是*type_parameter* ，而另一個運算元則是 literal `null`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2171">One operand is a value of type `T` where `T` is a *type_parameter* and the other operand is the literal `null`.</span></span> <span data-ttu-id="6aeee-2172">此外 `T` 不具有實數值型別條件約束。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2172">Furthermore `T` does not have the value type constraint.</span></span>

<span data-ttu-id="6aeee-2173">除非其中一個條件為 true，否則會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2173">Unless one of these conditions are true, a binding-time error occurs.</span></span> <span data-ttu-id="6aeee-2174">這些規則的值得注意的含意如下：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2174">Notable implications of these rules are:</span></span>

*  <span data-ttu-id="6aeee-2175">使用預先定義的參考型別等號比較運算子，在系結時，比較已知不同的兩個參考，是一種系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2175">It is a binding-time error to use the predefined reference type equality operators to compare two references that are known to be different at binding-time.</span></span> <span data-ttu-id="6aeee-2176">例如，如果運算元的系結時間類型是兩個類別類型 `A` 並 `B`，而且如果沒有 `A` 或 `B` 衍生自另一個，則這兩個運算元就無法參考相同的物件。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2176">For example, if the binding-time types of the operands are two class types `A` and `B`, and if neither `A` nor `B` derives from the other, then it would be impossible for the two operands to reference the same object.</span></span> <span data-ttu-id="6aeee-2177">因此，作業會被視為系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2177">Thus, the operation is considered a binding-time error.</span></span>
*  <span data-ttu-id="6aeee-2178">預先定義的參考型別相等運算子不允許比較實數值型別運算元。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2178">The predefined reference type equality operators do not permit value type operands to be compared.</span></span> <span data-ttu-id="6aeee-2179">因此，除非結構型別宣告它自己的等號比較運算子，否則不可能比較該結構型別的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2179">Therefore, unless a struct type declares its own equality operators, it is not possible to compare values of that struct type.</span></span>
*  <span data-ttu-id="6aeee-2180">預先定義的參考型別等號比較運算子永遠不會對其運算元執行「裝箱」作業。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2180">The predefined reference type equality operators never cause boxing operations to occur for their operands.</span></span> <span data-ttu-id="6aeee-2181">執行這類的裝箱作業是沒有意義的，因為對新配置的已裝箱實例的參考，一定會與其他所有參考不同。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2181">It would be meaningless to perform such boxing operations, since references to the newly allocated boxed instances would necessarily differ from all other references.</span></span>
*  <span data-ttu-id="6aeee-2182">如果類型參數類型的運算元 `T` 與 `null` 進行比較，而且 `T` 的執行時間類型是實數值型別，則比較的結果會是 `false`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2182">If an operand of a type parameter type `T` is compared to `null`, and the run-time type of `T` is a value type, the result of the comparison is `false`.</span></span>

<span data-ttu-id="6aeee-2183">下列範例會檢查不受條件約束之類型參數類型的引數是否 `null`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2183">The following example checks whether an argument of an unconstrained type parameter type is `null`.</span></span>
```csharp
class C<T>
{
    void F(T x) {
        if (x == null) throw new ArgumentNullException();
        ...
    }
}
```

<span data-ttu-id="6aeee-2184">即使 `T` 可以代表實值型別，也允許 `x == null` 結構，而且當 `T` 是實值型別時，只會將結果定義為 `false`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2184">The `x == null` construct is permitted even though `T` could represent a value type, and the result is simply defined to be `false` when `T` is a value type.</span></span>

<span data-ttu-id="6aeee-2185">針對格式為 `x == y` 或 `x != y` 的作業，如果有任何適用的 `operator ==` 或 `operator !=` 存在，運算子多載解析（[二元運算子](expressions.md#binary-operator-overload-resolution)多載解析）規則就會選取該運算子，而不是預先定義的參考型別相等操作.</span><span class="sxs-lookup"><span data-stu-id="6aeee-2185">For an operation of the form `x == y` or `x != y`, if any applicable `operator ==` or `operator !=` exists, the operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) rules will select that operator instead of the predefined reference type equality operator.</span></span> <span data-ttu-id="6aeee-2186">不過，一律可以藉由明確地將一個或兩個運算元轉換為 `object` 的類型，來選取預先定義的參考型別等號比較運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2186">However, it is always possible to select the predefined reference type equality operator by explicitly casting one or both of the operands to type `object`.</span></span> <span data-ttu-id="6aeee-2187">範例</span><span class="sxs-lookup"><span data-stu-id="6aeee-2187">The example</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        string s = "Test";
        string t = string.Copy(s);
        Console.WriteLine(s == t);
        Console.WriteLine((object)s == t);
        Console.WriteLine(s == (object)t);
        Console.WriteLine((object)s == (object)t);
    }
}
```
<span data-ttu-id="6aeee-2188">產生下列輸出</span><span class="sxs-lookup"><span data-stu-id="6aeee-2188">produces the output</span></span>
```console
True
False
False
False
```

<span data-ttu-id="6aeee-2189">@No__t-0 和 @no__t 1 變數指的是兩個不同的 `string` 實例，其中包含相同的字元。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2189">The `s` and `t` variables refer to two distinct `string` instances containing the same characters.</span></span> <span data-ttu-id="6aeee-2190">第一個比較輸出 `True`，因為當兩個運算元的類型都是 `string` 時，會選取預先定義的字串相等運算子（[字串等號比較運算子](expressions.md#string-equality-operators)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2190">The first comparison outputs `True` because the predefined string equality operator ([String equality operators](expressions.md#string-equality-operators)) is selected when both operands are of type `string`.</span></span> <span data-ttu-id="6aeee-2191">其餘的比較會將所有輸出 `False`，因為當其中一個或兩個運算元的類型為 `object` 時，會選取預先定義的參考型別等號比較運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2191">The remaining comparisons all output `False` because the predefined reference type equality operator is selected when one or both of the operands are of type `object`.</span></span>

<span data-ttu-id="6aeee-2192">請注意，上述技術對實值型別沒有意義。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2192">Note that the above technique is not meaningful for value types.</span></span> <span data-ttu-id="6aeee-2193">範例</span><span class="sxs-lookup"><span data-stu-id="6aeee-2193">The example</span></span>
```csharp
class Test
{
    static void Main() {
        int i = 123;
        int j = 123;
        System.Console.WriteLine((object)i == (object)j);
    }
}
```
<span data-ttu-id="6aeee-2194">輸出 `False`，因為轉換會建立兩個不同的已裝箱 `int` 值實例的參考。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2194">outputs `False` because the casts create references to two separate instances of boxed `int` values.</span></span>

### <a name="string-equality-operators"></a><span data-ttu-id="6aeee-2195">字串等號比較運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-2195">String equality operators</span></span>

<span data-ttu-id="6aeee-2196">預先定義的字串等號比較運算子如下：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2196">The predefined string equality operators are:</span></span>
```csharp
bool operator ==(string x, string y);
bool operator !=(string x, string y);
```

<span data-ttu-id="6aeee-2197">當下列其中一項為真時，會將兩個 @no__t 0 的值視為相等：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2197">Two `string` values are considered equal when one of the following is true:</span></span>

*  <span data-ttu-id="6aeee-2198">這兩個值都是 `null`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2198">Both values are `null`.</span></span>
*  <span data-ttu-id="6aeee-2199">對於在每個字元位置具有相同長度和相同字元的字串實例而言，這兩個值都是非 null 參考。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2199">Both values are non-null references to string instances that have identical lengths and identical characters in each character position.</span></span>

<span data-ttu-id="6aeee-2200">字串相等運算子會比較字串值，而不是字串參考。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2200">The string equality operators compare string values rather than string references.</span></span> <span data-ttu-id="6aeee-2201">當兩個不同的字串實例包含完全相同的字元序列時，字串的值會相等，但參考則不同。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2201">When two separate string instances contain the exact same sequence of characters, the values of the strings are equal, but the references are different.</span></span> <span data-ttu-id="6aeee-2202">如[參考型別等號比較運算子](expressions.md#reference-type-equality-operators)中所述，參考型別相等運算子可以用來比較字串參考，而不是字串值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2202">As described in [Reference type equality operators](expressions.md#reference-type-equality-operators), the reference type equality operators can be used to compare string references instead of string values.</span></span>

### <a name="delegate-equality-operators"></a><span data-ttu-id="6aeee-2203">委派等號比較運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-2203">Delegate equality operators</span></span>

<span data-ttu-id="6aeee-2204">每個委派類型都會隱含提供下列預先定義的比較運算子：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2204">Every delegate type implicitly provides the following predefined comparison operators:</span></span>

```csharp
bool operator ==(System.Delegate x, System.Delegate y);
bool operator !=(System.Delegate x, System.Delegate y);
```

<span data-ttu-id="6aeee-2205">兩個委派實例會視為相同，如下所示：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2205">Two delegate instances are considered equal as follows:</span></span>

*  <span data-ttu-id="6aeee-2206">如果其中一個委派實例 `null`，只有在兩者都 `null` 時，才會相等。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2206">If either of the delegate instances is `null`, they are equal if and only if both are `null`.</span></span>
*  <span data-ttu-id="6aeee-2207">如果委派有不同的執行時間類型，它們絕對不會相等。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2207">If the delegates have different run-time type they are never equal.</span></span>
*  <span data-ttu-id="6aeee-2208">如果這兩個委派實例都有調用清單（[委派](delegates.md#delegate-declarations)宣告），只有在其調用清單的長度相同，而且其中一個調用清單中的每個專案都相等（如下面所定義）時，才會有相等的對應依序在其他的調用清單中輸入專案。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2208">If both of the delegate instances have an invocation list ([Delegate declarations](delegates.md#delegate-declarations)), those instances are equal if and only if their invocation lists are the same length, and each entry in one's invocation list is equal (as defined below) to the corresponding entry, in order, in the other's invocation list.</span></span>

<span data-ttu-id="6aeee-2209">下列規則會管理調用清單專案的相等性：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2209">The following rules govern the equality of invocation list entries:</span></span>

*  <span data-ttu-id="6aeee-2210">如果兩個調用清單專案都參考相同的靜態方法，則專案相等。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2210">If two invocation list entries both refer to the same static method then the entries are equal.</span></span>
*  <span data-ttu-id="6aeee-2211">如果兩個調用清單專案都參考相同目標物件上的相同非靜態方法（如參考等號比較運算子所定義），則專案會相等。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2211">If two invocation list entries both refer to the same non-static method on the same target object (as defined by the reference equality operators) then the entries are equal.</span></span>
*  <span data-ttu-id="6aeee-2212">允許（但非必要）在*具有相同（* 可能是空的）集的已捕捉外部變數實例的評估所產生的調用清單專案，必須是等於.</span><span class="sxs-lookup"><span data-stu-id="6aeee-2212">Invocation list entries produced from evaluation of semantically identical *anonymous_method_expression*s or *lambda_expression*s with the same (possibly empty) set of captured outer variable instances are permitted (but not required) to be equal.</span></span>

### <a name="equality-operators-and-null"></a><span data-ttu-id="6aeee-2213">等號比較運算子和 null</span><span class="sxs-lookup"><span data-stu-id="6aeee-2213">Equality operators and null</span></span>

<span data-ttu-id="6aeee-2214">@No__t-0 和 @no__t 1 運算子允許一個運算元成為可為 null 類型的值，另一個運算元則是 @no__t 2 常值，即使作業沒有預先定義或使用者定義的運算子（unlifted 或提升形式）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2214">The `==` and `!=` operators permit one operand to be a value of a nullable type and the other to be the `null` literal, even if no predefined or user-defined operator (in unlifted or lifted form) exists for the operation.</span></span>

<span data-ttu-id="6aeee-2215">適用于其中一種形式的操作</span><span class="sxs-lookup"><span data-stu-id="6aeee-2215">For an operation of one of the forms</span></span>
```csharp
x == null
null == x
x != null
null != x
```
<span data-ttu-id="6aeee-2216">其中 `x` 是可為 null 之型別的運算式，如果運算子多載解析（[二元運算子](expressions.md#binary-operator-overload-resolution)多載解析）找不到適用的運算子，則會改為從 `x` 的 `HasValue` 屬性計算結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2216">where `x` is an expression of a nullable type, if operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) fails to find an applicable operator, the result is instead computed from the `HasValue` property of `x`.</span></span> <span data-ttu-id="6aeee-2217">具體而言，前兩個表單會轉譯為 `!x.HasValue`，最後兩個表單會轉譯成 `x.HasValue`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2217">Specifically, the first two forms are translated into `!x.HasValue`, and last two forms are translated into `x.HasValue`.</span></span>

### <a name="the-is-operator"></a><span data-ttu-id="6aeee-2218">Is 運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-2218">The is operator</span></span>

<span data-ttu-id="6aeee-2219">@No__t-0 運算子可用來動態檢查物件的執行時間類型是否與指定的類型相容。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2219">The `is` operator is used to dynamically check if the run-time type of an object is compatible with a given type.</span></span> <span data-ttu-id="6aeee-2220">作業的結果 `E is T`，其中 `E` 是運算式，而 `T` 是類型，是布林值，指出是否可以透過參考轉換、裝箱轉換或取消裝箱轉換，成功將 @no__t 3 轉換成類型 `T`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2220">The result of the operation `E is T`, where `E` is an expression and `T` is a type, is a boolean value indicating whether `E` can successfully be converted to type `T` by a reference conversion, a boxing conversion, or an unboxing conversion.</span></span> <span data-ttu-id="6aeee-2221">在將類型引數取代為所有類型參數之後，會依照下列方式評估作業：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2221">The operation is evaluated as follows, after type arguments have been substituted for all type parameters:</span></span>

*  <span data-ttu-id="6aeee-2222">如果 `E` 是匿名函式，則會發生編譯時期錯誤</span><span class="sxs-lookup"><span data-stu-id="6aeee-2222">If `E` is an anonymous function, a compile-time error occurs</span></span>
*  <span data-ttu-id="6aeee-2223">如果 `E` 是方法群組或 `null` 常值，則如果 `E` 的類型是參考型別或可為 null 的類型，而且 `E` 的值是 null，則結果為 false。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2223">If `E` is a method group or the `null` literal, of if the type of `E` is a reference type or a nullable type and the value of `E` is null, the result is false.</span></span>
*  <span data-ttu-id="6aeee-2224">否則，讓 `D` 代表 `E` 的動態類型，如下所示：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2224">Otherwise, let `D` represent the dynamic type of `E` as follows:</span></span>
   * <span data-ttu-id="6aeee-2225">如果 `E` 的類型是參考型別，`D` 就是 `E` 的實例參考的執行時間類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2225">If the type of `E` is a reference type, `D` is the run-time type of the instance reference by `E`.</span></span>
   * <span data-ttu-id="6aeee-2226">如果 `E` 的類型是可為 null 的類型，則 `D` 是該可為 null 類型的基礎類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2226">If the type of `E` is a nullable type, `D` is the underlying type of that nullable type.</span></span>
   * <span data-ttu-id="6aeee-2227">如果 `E` 的類型是不可為 null 的實數值型別，`D` 就是 `E` 的類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2227">If the type of `E` is a non-nullable value type, `D` is the type of `E`.</span></span>
*  <span data-ttu-id="6aeee-2228">作業的結果取決於 `D` 和 `T`，如下所示：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2228">The result of the operation depends on `D` and `T` as follows:</span></span>
   * <span data-ttu-id="6aeee-2229">如果 `T` 是參考型別，如果 `D` 和 `T` 是相同的類型，則結果為 true，如果 `D` 是參考型別，而且從 `D` 到 `T` 的隱含參考轉換已存在，或 `D` 是實數值型別，而從 `D` 進行的裝箱轉換`T` 存在。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2229">If `T` is a reference type, the result is true if `D` and `T` are the same type, if `D` is a reference type and an implicit reference conversion from `D` to `T` exists, or if `D` is a value type and a boxing conversion from `D` to `T` exists.</span></span>
   * <span data-ttu-id="6aeee-2230">如果 `T` 是可為 null 的型別，則如果 `D` 是 `T` 的基礎類型，則結果為 true。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2230">If `T` is a nullable type, the result is true if `D` is the underlying type of `T`.</span></span>
   * <span data-ttu-id="6aeee-2231">如果 `T` 是不可為 null 的實值型別，則如果 `D` 和 `T` 的類型相同，則結果為 true。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2231">If `T` is a non-nullable value type, the result is true if `D` and `T` are the same type.</span></span>
   * <span data-ttu-id="6aeee-2232">否則，結果為 false。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2232">Otherwise, the result is false.</span></span>

<span data-ttu-id="6aeee-2233">請注意，`is` 運算子不會考慮使用者定義的轉換。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2233">Note that user defined conversions, are not considered by the `is` operator.</span></span>

### <a name="the-as-operator"></a><span data-ttu-id="6aeee-2234">As 運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-2234">The as operator</span></span>

<span data-ttu-id="6aeee-2235">@No__t-0 運算子可用來將值明確轉換為指定的參考型別或可為 null 的型別。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2235">The `as` operator is used to explicitly convert a value to a given reference type or nullable type.</span></span> <span data-ttu-id="6aeee-2236">不同于 cast 運算式（[cast](expressions.md#cast-expressions)運算式），@no__t 1 運算子絕對不會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2236">Unlike a cast expression ([Cast expressions](expressions.md#cast-expressions)), the `as` operator never throws an exception.</span></span> <span data-ttu-id="6aeee-2237">相反地，如果不可能指定的轉換，則產生的值會 `null`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2237">Instead, if the indicated conversion is not possible, the resulting value is `null`.</span></span>

<span data-ttu-id="6aeee-2238">在格式 `E as T` 的作業中，`E` 必須是運算式，而且 `T` 必須是參考型別、已知為參考型別的類型參數，或可為 null 的類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2238">In an operation of the form `E as T`, `E` must be an expression and `T` must be a reference type, a type parameter known to be a reference type, or a nullable type.</span></span> <span data-ttu-id="6aeee-2239">此外，下列至少一項必須為 true，否則會發生編譯時期錯誤：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2239">Furthermore, at least one of the following must be true, or otherwise a compile-time error occurs:</span></span>

*  <span data-ttu-id="6aeee-2240">身分識別（身分[識別轉換](conversions.md#identity-conversion)）、隱含可為 Null （[隱含 nullable 轉換](conversions.md#implicit-nullable-conversions)）、隱含參考（[隱含參考轉換](conversions.md#implicit-reference-conversions)）、裝箱（[裝箱轉換](conversions.md#boxing-conversions)）、可為 null 的明確（[可為 null轉換](conversions.md#explicit-nullable-conversions)）、明確參考（[明確參考轉換](conversions.md#explicit-reference-conversions)）或取消裝箱（[取消裝箱轉換](conversions.md#unboxing-conversions)）從 `E` 到 `T` 都有轉換。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2240">An identity ([Identity conversion](conversions.md#identity-conversion)), implicit nullable ([Implicit nullable conversions](conversions.md#implicit-nullable-conversions)), implicit reference ([Implicit reference conversions](conversions.md#implicit-reference-conversions)), boxing ([Boxing conversions](conversions.md#boxing-conversions)), explicit nullable ([Explicit nullable conversions](conversions.md#explicit-nullable-conversions)), explicit reference ([Explicit reference conversions](conversions.md#explicit-reference-conversions)), or unboxing ([Unboxing conversions](conversions.md#unboxing-conversions)) conversion exists from `E` to `T`.</span></span>
*  <span data-ttu-id="6aeee-2241">@No__t-0 或 `T` 的類型是開啟的類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2241">The type of `E` or `T` is an open type.</span></span>
*  <span data-ttu-id="6aeee-2242">`E` 是 `null` 常值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2242">`E` is the `null` literal.</span></span>

<span data-ttu-id="6aeee-2243">如果 `E` 的編譯時期類型未 `dynamic`，則作業 `E as T` 會產生與相同的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2243">If the compile-time type of `E` is not `dynamic`, the operation `E as T` produces the same result as</span></span>
```csharp
E is T ? (T)(E) : (T)null
```
<span data-ttu-id="6aeee-2244">但只會評估 `E` 一次。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2244">except that `E` is only evaluated once.</span></span> <span data-ttu-id="6aeee-2245">編譯器可以將 `E as T` 優化，以最多執行一次動態類型檢查，而不是上述擴充所隱含的兩個動態類型檢查。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2245">The compiler can be expected to optimize `E as T` to perform at most one dynamic type check as opposed to the two dynamic type checks implied by the expansion above.</span></span>

<span data-ttu-id="6aeee-2246">如果 `E` 的編譯階段類型是 `dynamic`，則與 cast 運算子不同的是，`as` 運算子不會動態繫結（[動態](expressions.md#dynamic-binding)系結）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2246">If the compile-time type of `E` is `dynamic`, unlike the cast operator the `as` operator is not dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="6aeee-2247">因此，在此情況下的擴充是：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2247">Therefore the expansion in this case is:</span></span>
```csharp
E is T ? (T)(object)(E) : (T)null
```

<span data-ttu-id="6aeee-2248">請注意，某些轉換（例如使用者定義的轉換）無法與 `as` 運算子搭配使用，應該改用 cast 運算式來執行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2248">Note that some conversions, such as user defined conversions, are not possible with the `as` operator and should instead be performed using cast expressions.</span></span>

<span data-ttu-id="6aeee-2249">在範例中</span><span class="sxs-lookup"><span data-stu-id="6aeee-2249">In the example</span></span>
```csharp
class X
{

    public string F(object o) {
        return o as string;        // OK, string is a reference type
    }

    public T G<T>(object o) where T: Attribute {
        return o as T;             // Ok, T has a class constraint
    }

    public U H<U>(object o) {
        return o as U;             // Error, U is unconstrained 
    }
}
```
<span data-ttu-id="6aeee-2250">`G` 的型別參數 `T` 已知為引用型別，因為它有類別條件約束。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2250">the type parameter `T` of `G` is known to be a reference type, because it has the class constraint.</span></span> <span data-ttu-id="6aeee-2251">類型參數 `U`，但不 `H`;因此，不允許在 `H` 中使用 `as` 運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2251">The type parameter `U` of `H` is not however; hence the use of the `as` operator in `H` is disallowed.</span></span>

## <a name="logical-operators"></a><span data-ttu-id="6aeee-2252">邏輯運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-2252">Logical operators</span></span>

<span data-ttu-id="6aeee-2253">@No__t-0、`^` 和 @no__t 2 運算子稱為邏輯運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2253">The `&`, `^`, and `|` operators are called the logical operators.</span></span>

```antlr
and_expression
    : equality_expression
    | and_expression '&' equality_expression
    ;

exclusive_or_expression
    : and_expression
    | exclusive_or_expression '^' and_expression
    ;

inclusive_or_expression
    : exclusive_or_expression
    | inclusive_or_expression '|' exclusive_or_expression
    ;
```

<span data-ttu-id="6aeee-2254">如果邏輯運算子的運算元具有編譯時期類型 `dynamic`，則運算式會動態繫結（[動態](expressions.md#dynamic-binding)系結）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2254">If an operand of a logical operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="6aeee-2255">在此情況下，運算式的編譯時期類型為 `dynamic`，而以下所述的解決將會在執行時間使用具有編譯時間類型 `dynamic` 之運算元的執行時間類型進行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2255">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="6aeee-2256">若為格式為 `x op y` 的作業，其中 `op` 是其中一個邏輯運算子，則會套用多載解析（[二元運算子](expressions.md#binary-operator-overload-resolution)多載解析）來選取特定的運算子執行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2256">For an operation of the form `x op y`, where `op` is one of the logical operators, overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="6aeee-2257">運算元會轉換成所選運算子的參數類型，而結果的類型會是運算子的傳回類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2257">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="6aeee-2258">下列各節將說明預先定義的邏輯運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2258">The predefined logical operators are described in the following sections.</span></span>

### <a name="integer-logical-operators"></a><span data-ttu-id="6aeee-2259">整數邏輯運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-2259">Integer logical operators</span></span>

<span data-ttu-id="6aeee-2260">預先定義的整數邏輯運算子如下：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2260">The predefined integer logical operators are:</span></span>
```csharp
int operator &(int x, int y);
uint operator &(uint x, uint y);
long operator &(long x, long y);
ulong operator &(ulong x, ulong y);

int operator |(int x, int y);
uint operator |(uint x, uint y);
long operator |(long x, long y);
ulong operator |(ulong x, ulong y);

int operator ^(int x, int y);
uint operator ^(uint x, uint y);
long operator ^(long x, long y);
ulong operator ^(ulong x, ulong y);
```

<span data-ttu-id="6aeee-2261">@No__t-0 運算子會計算兩個運算元的位邏輯 `AND`，而 @no__t 2 運算子會計算兩個運算元的位邏輯 `OR`，而 `^` 運算子會計算兩個運算元的位邏輯獨佔 `OR`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2261">The `&` operator computes the bitwise logical `AND` of the two operands, the `|` operator computes the bitwise logical `OR` of the two operands, and the `^` operator computes the bitwise logical exclusive `OR` of the two operands.</span></span> <span data-ttu-id="6aeee-2262">這些作業不可能發生溢位。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2262">No overflows are possible from these operations.</span></span>

### <a name="enumeration-logical-operators"></a><span data-ttu-id="6aeee-2263">列舉邏輯運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-2263">Enumeration logical operators</span></span>

<span data-ttu-id="6aeee-2264">每個列舉類型 `E` 會隱含提供下列預先定義的邏輯運算子：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2264">Every enumeration type `E` implicitly provides the following predefined logical operators:</span></span>

```csharp
E operator &(E x, E y);
E operator |(E x, E y);
E operator ^(E x, E y);
```

<span data-ttu-id="6aeee-2265">評估 `x op y` 的結果，其中 `x` 和 `y` 是列舉 @no__t 類型的運算式，且其基礎類型為 `U`，而 `op` 是其中一個邏輯運算子，與評估 `(E)((U)x op (U)y)` 完全相同。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2265">The result of evaluating `x op y`, where `x` and `y` are expressions of an enumeration type `E` with an underlying type `U`, and `op` is one of the logical operators, is exactly the same as evaluating `(E)((U)x op (U)y)`.</span></span> <span data-ttu-id="6aeee-2266">換句話說，列舉型別邏輯運算子只會在兩個運算元的基礎型別上執行邏輯運算。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2266">In other words, the enumeration type logical operators simply perform the logical operation on the underlying type of the two operands.</span></span>

### <a name="boolean-logical-operators"></a><span data-ttu-id="6aeee-2267">布林值邏輯運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-2267">Boolean logical operators</span></span>

<span data-ttu-id="6aeee-2268">預先定義的布林邏輯運算子如下：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2268">The predefined boolean logical operators are:</span></span>
```csharp
bool operator &(bool x, bool y);
bool operator |(bool x, bool y);
bool operator ^(bool x, bool y);
```

<span data-ttu-id="6aeee-2269">若 `x` 及 `y` 皆為 `true`，那麼 `x & y` 的結果會是 `true`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2269">The result of `x & y` is `true` if both `x` and `y` are `true`.</span></span> <span data-ttu-id="6aeee-2270">否則，結果為 `false`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2270">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="6aeee-2271">如果 `x` 或 `y` 是 `true`，則 `x | y` 的結果會 `true`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2271">The result of `x | y` is `true` if either `x` or `y` is `true`.</span></span> <span data-ttu-id="6aeee-2272">否則，結果為 `false`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2272">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="6aeee-2273">如果 `x` 是 `true`，且 `y` 為 `false`，則 `x ^ y` 的結果會 `true`，`y` `true`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2273">The result of `x ^ y` is `true` if `x` is `true` and `y` is `false`, or `x` is `false` and `y` is `true`.</span></span> <span data-ttu-id="6aeee-2274">否則，結果為 `false`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2274">Otherwise, the result is `false`.</span></span> <span data-ttu-id="6aeee-2275">當運算元的類型為 `bool` 時，`^` 運算子會計算與 @no__t 2 運算子相同的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2275">When the operands are of type `bool`, the `^` operator computes the same result as the `!=` operator.</span></span>

### <a name="nullable-boolean-logical-operators"></a><span data-ttu-id="6aeee-2276">可為 null 的布林值邏輯運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-2276">Nullable boolean logical operators</span></span>

<span data-ttu-id="6aeee-2277">可為 null 的布林值類型 `bool?` 可以代表三個值（`true`、`false` 和 `null`），而且在概念上類似于 SQL 中用於布林運算式的三個數值型別。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2277">The nullable boolean type `bool?` can represent three values, `true`, `false`, and `null`, and is conceptually similar to the three-valued type used for boolean expressions in SQL.</span></span> <span data-ttu-id="6aeee-2278">為了確保 `&` 和 @no__t 1 @no__t 運算子所產生的結果與 SQL 的三值邏輯一致，會提供下列預先定義的運算子：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2278">To ensure that the results produced by the `&` and `|` operators for `bool?` operands are consistent with SQL's three-valued logic, the following predefined operators are provided:</span></span>

```csharp
bool? operator &(bool? x, bool? y);
bool? operator |(bool? x, bool? y);
```

<span data-ttu-id="6aeee-2279">下表列出這些運算子針對所有值組合所產生的結果，`true`、`false` 和 `null`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2279">The following table lists the results produced by these operators for all combinations of the values `true`, `false`, and `null`.</span></span>

| `x`     | `y`     | `x & y` | <code>x &#124; y</code> |
|:-------:|:-------:|:-------:|:-------:|
| `true`  | `true`  | `true`  | `true`  | 
| `true`  | `false` | `false` | `true`  | 
| `true`  | `null`  | `null`  | `true`  | 
| `false` | `true`  | `false` | `true`  | 
| `false` | `false` | `false` | `false` | 
| `false` | `null`  | `false` | `null`  | 
| `null`  | `true`  | `null`  | `true`  | 
| `null`  | `false` | `false` | `null`  | 
| `null`  | `null`  | `null`  | `null`  | 

## <a name="conditional-logical-operators"></a><span data-ttu-id="6aeee-2280">條件邏輯運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-2280">Conditional logical operators</span></span>

<span data-ttu-id="6aeee-2281">@No__t-0 和 @no__t 1 運算子稱為「條件式邏輯運算子」。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2281">The `&&` and `||` operators are called the conditional logical operators.</span></span> <span data-ttu-id="6aeee-2282">它們也稱為「最小運算」邏輯運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2282">They are also called the "short-circuiting" logical operators.</span></span>

```antlr
conditional_and_expression
    : inclusive_or_expression
    | conditional_and_expression '&&' inclusive_or_expression
    ;

conditional_or_expression
    : conditional_and_expression
    | conditional_or_expression '||' conditional_and_expression
    ;
```

<span data-ttu-id="6aeee-2283">@No__t-0 和 @no__t 1 運算子是 `&` 和 `|` 運算子的條件式版本：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2283">The `&&` and `||` operators are conditional versions of the `&` and `|` operators:</span></span>

*  <span data-ttu-id="6aeee-2284">作業 `x && y` 對應至作業 `x & y`，但只有在 `x` 不 `false` 時，才會評估 `y`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2284">The operation `x && y` corresponds to the operation `x & y`, except that `y` is evaluated only if `x` is not `false`.</span></span>
*  <span data-ttu-id="6aeee-2285">作業 `x || y` 對應至作業 `x | y`，但只有在 `x` 不 `true` 時，才會評估 `y`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2285">The operation `x || y` corresponds to the operation `x | y`, except that `y` is evaluated only if `x` is not `true`.</span></span>

<span data-ttu-id="6aeee-2286">如果條件式邏輯運算子的運算元具有編譯時期類型 `dynamic`，則運算式會動態繫結（[動態](expressions.md#dynamic-binding)系結）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2286">If an operand of a conditional logical operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="6aeee-2287">在此情況下，運算式的編譯時期類型為 `dynamic`，而以下所述的解決將會在執行時間使用具有編譯時間類型 `dynamic` 之運算元的執行時間類型進行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2287">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="6aeee-2288">@No__t-0 或 `x || y` 格式的作業會藉由套用多載解析（[二元運算子](expressions.md#binary-operator-overload-resolution)多載解析）來處理，如同作業是寫入 `x & y` 或 `x | y`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2288">An operation of the form `x && y` or `x || y` is processed by applying overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) as if the operation was written `x & y` or `x | y`.</span></span> <span data-ttu-id="6aeee-2289">請</span><span class="sxs-lookup"><span data-stu-id="6aeee-2289">Then,</span></span>

*  <span data-ttu-id="6aeee-2290">如果多載解析找不到單一最佳運算子，或如果多載解析選取其中一個預先定義的整數邏輯運算子，則會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2290">If overload resolution fails to find a single best operator, or if overload resolution selects one of the predefined integer logical operators, a binding-time error occurs.</span></span>
*  <span data-ttu-id="6aeee-2291">否則，如果選取的運算子是其中一個預先定義的布林邏輯運算子（[布林邏輯運算子](expressions.md#boolean-logical-operators)）或可為 null 的布林邏輯運算子（[可為 null 的布林邏輯運算子](expressions.md#nullable-boolean-logical-operators)），則會依照中[的說明來處理作業。布林條件邏輯運算子](expressions.md#boolean-conditional-logical-operators)。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2291">Otherwise, if the selected operator is one of the predefined boolean logical operators ([Boolean logical operators](expressions.md#boolean-logical-operators)) or nullable boolean logical operators ([Nullable boolean logical operators](expressions.md#nullable-boolean-logical-operators)), the operation is processed as described in [Boolean conditional logical operators](expressions.md#boolean-conditional-logical-operators).</span></span>
*  <span data-ttu-id="6aeee-2292">否則，選取的運算子就是使用者定義的運算子，而且會依照[使用者定義的條件式邏輯運算子](expressions.md#user-defined-conditional-logical-operators)中的說明來處理作業。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2292">Otherwise, the selected operator is a user-defined operator, and the operation is processed as described in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators).</span></span>

<span data-ttu-id="6aeee-2293">不可能直接多載條件式邏輯運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2293">It is not possible to directly overload the conditional logical operators.</span></span> <span data-ttu-id="6aeee-2294">不過，因為條件式邏輯運算子是根據一般邏輯運算子來評估，所以一般邏輯運算子的多載是，具有特定限制，也會視為條件式邏輯運算子的多載。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2294">However, because the conditional logical operators are evaluated in terms of the regular logical operators, overloads of the regular logical operators are, with certain restrictions, also considered overloads of the conditional logical operators.</span></span> <span data-ttu-id="6aeee-2295">這會在[使用者定義的條件式邏輯運算子](expressions.md#user-defined-conditional-logical-operators)中進一步說明。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2295">This is described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators).</span></span>

### <a name="boolean-conditional-logical-operators"></a><span data-ttu-id="6aeee-2296">布林條件邏輯運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-2296">Boolean conditional logical operators</span></span>

<span data-ttu-id="6aeee-2297">當 `&&` 或 `||` 的運算元屬於類型 `bool`，或如果運算元的類型未定義適用的 `operator &` 或 `operator |`，但確實將隱含轉換定義為 `bool`，則會以下列方式處理作業：:</span><span class="sxs-lookup"><span data-stu-id="6aeee-2297">When the operands of `&&` or `||` are of type `bool`, or when the operands are of types that do not define an applicable `operator &` or `operator |`, but do define implicit conversions to `bool`, the operation is processed as follows:</span></span>

*  <span data-ttu-id="6aeee-2298">@No__t-0 的作業會評估為 `x ? y : false`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2298">The operation `x && y` is evaluated as `x ? y : false`.</span></span> <span data-ttu-id="6aeee-2299">換句話說，`x` 會先進行評估，並轉換成類型 `bool`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2299">In other words, `x` is first evaluated and converted to type `bool`.</span></span> <span data-ttu-id="6aeee-2300">然後，如果 `x` 是 `true`，則會評估 `y` 並轉換成類型 `bool`，這會變成作業的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2300">Then, if `x` is `true`, `y` is evaluated and converted to type `bool`, and this becomes the result of the operation.</span></span> <span data-ttu-id="6aeee-2301">否則，作業的結果會 `false`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2301">Otherwise, the result of the operation is `false`.</span></span>
*  <span data-ttu-id="6aeee-2302">@No__t-0 的作業會評估為 `x ? true : y`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2302">The operation `x || y` is evaluated as `x ? true : y`.</span></span> <span data-ttu-id="6aeee-2303">換句話說，`x` 會先進行評估，並轉換成類型 `bool`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2303">In other words, `x` is first evaluated and converted to type `bool`.</span></span> <span data-ttu-id="6aeee-2304">然後，如果 `x` `true`，作業的結果會是 `true`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2304">Then, if `x` is `true`, the result of the operation is `true`.</span></span> <span data-ttu-id="6aeee-2305">否則，`y` 會進行評估並轉換成類型 `bool`，而這會成為作業的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2305">Otherwise, `y` is evaluated and converted to type `bool`, and this becomes the result of the operation.</span></span>

### <a name="user-defined-conditional-logical-operators"></a><span data-ttu-id="6aeee-2306">使用者定義條件式邏輯運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-2306">User-defined conditional logical operators</span></span>

<span data-ttu-id="6aeee-2307">當 `&&` 或 `||` 的運算元屬於宣告適用的使用者定義 `operator &` 或 `operator |` 的類型時，下列兩項都必須為 true，其中 `T` 是所選取之運算子的宣告類型：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2307">When the operands of `&&` or `||` are of types that declare an applicable user-defined `operator &` or `operator |`, both of the following must be true, where `T` is the type in which the selected operator is declared:</span></span>

*  <span data-ttu-id="6aeee-2308">傳回型別和所選運算子之每個參數的型別都必須 `T`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2308">The return type and the type of each parameter of the selected operator must be `T`.</span></span> <span data-ttu-id="6aeee-2309">換句話說，運算子必須計算 `T` 類型之兩個運算元的邏輯 @no__t 0 或邏輯 `OR`，而且必須傳回類型 `T` 的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2309">In other words, the operator must compute the logical `AND` or the logical `OR` of two operands of type `T`, and must return a result of type `T`.</span></span>
*  <span data-ttu-id="6aeee-2310">`T` 必須包含 `operator true` 和 `operator false` 的宣告。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2310">`T` must contain declarations of `operator true` and `operator false`.</span></span>

<span data-ttu-id="6aeee-2311">如果未滿足上述任一項需求，就會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2311">A binding-time error occurs if either of these requirements is not satisfied.</span></span> <span data-ttu-id="6aeee-2312">否則，會藉由結合使用者定義的 `operator true` 或 `operator false` 與所選使用者定義的運算子，來評估 `&&` 或 @no__t 1 作業：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2312">Otherwise, the `&&` or `||` operation is evaluated by combining the user-defined `operator true` or `operator false` with the selected user-defined operator:</span></span>

*  <span data-ttu-id="6aeee-2313">@No__t-0 的作業會評估為 `T.false(x) ? x : T.&(x, y)`，其中 `T.false(x)` 是叫用 `T` 中所宣告之 `operator false` 的調用，而 `T.&(x, y)` 是所選 `operator &` 的調用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2313">The operation `x && y` is evaluated as `T.false(x) ? x : T.&(x, y)`, where `T.false(x)` is an invocation of the `operator false` declared in `T`, and `T.&(x, y)` is an invocation of the selected `operator &`.</span></span> <span data-ttu-id="6aeee-2314">換句話說，`x` 會先進行評估，並在結果上叫用 `operator false`，以判斷 `x` 是否肯定為 false。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2314">In other words, `x` is first evaluated and `operator false` is invoked on the result to determine if `x` is definitely false.</span></span> <span data-ttu-id="6aeee-2315">然後，如果 `x` 肯定為 false，則作業的結果會是先前針對 `x` 計算的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2315">Then, if `x` is definitely false, the result of the operation is the value previously computed for `x`.</span></span> <span data-ttu-id="6aeee-2316">否則，會評估 `y`，並在先前針對 `x` 計算的值上叫用選取的 `operator &`，並針對 `y` 計算的值，產生作業的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2316">Otherwise, `y` is evaluated, and the selected `operator &` is invoked on the value previously computed for `x` and the value computed for `y` to produce the result of the operation.</span></span>
*  <span data-ttu-id="6aeee-2317">@No__t-0 的作業會評估為 `T.true(x) ? x : T.|(x, y)`，其中 `T.true(x)` 是叫用 `T` 中所宣告之 `operator true` 的調用，而 `T.|(x,y)` 是所選 `operator|` 的調用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2317">The operation `x || y` is evaluated as `T.true(x) ? x : T.|(x, y)`, where `T.true(x)` is an invocation of the `operator true` declared in `T`, and `T.|(x,y)` is an invocation of the selected `operator|`.</span></span> <span data-ttu-id="6aeee-2318">換句話說，`x` 會先進行評估，並在結果上叫用 `operator true`，以判斷 `x` 是否肯定為 true。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2318">In other words, `x` is first evaluated and `operator true` is invoked on the result to determine if `x` is definitely true.</span></span> <span data-ttu-id="6aeee-2319">然後，如果 `x` 肯定為 true，則作業的結果會是先前針對 `x` 計算的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2319">Then, if `x` is definitely true, the result of the operation is the value previously computed for `x`.</span></span> <span data-ttu-id="6aeee-2320">否則，會評估 `y`，並在先前針對 `x` 計算的值上叫用選取的 `operator |`，並針對 `y` 計算的值，產生作業的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2320">Otherwise, `y` is evaluated, and the selected `operator |` is invoked on the value previously computed for `x` and the value computed for `y` to produce the result of the operation.</span></span>

<span data-ttu-id="6aeee-2321">在上述任一作業中，`x` 所指定的運算式只會評估一次，而由 `y` 指定的運算式則不會進行評估或只評估一次。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2321">In either of these operations, the expression given by `x` is only evaluated once, and the expression given by `y` is either not evaluated or evaluated exactly once.</span></span>

<span data-ttu-id="6aeee-2322">如需實 `operator true` 和 `operator false` 之類型的範例，請參閱[資料庫布林值類型](structs.md#database-boolean-type)。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2322">For an example of a type that implements `operator true` and `operator false`, see [Database boolean type](structs.md#database-boolean-type).</span></span>

## <a name="the-null-coalescing-operator"></a><span data-ttu-id="6aeee-2323">Null 聯合運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-2323">The null coalescing operator</span></span>

<span data-ttu-id="6aeee-2324">@No__t-0 運算子稱為 null 聯合運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2324">The `??` operator is called the null coalescing operator.</span></span>

```antlr
null_coalescing_expression
    : conditional_or_expression
    | conditional_or_expression '??' null_coalescing_expression
    ;
```

<span data-ttu-id="6aeee-2325">格式為 `a ?? b` 的 null 聯合運算式，必須是可為 null 的類型或參考型別的 `a`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2325">A null coalescing expression of the form `a ?? b` requires `a` to be of a nullable type or reference type.</span></span> <span data-ttu-id="6aeee-2326">如果 `a` 為非 null，則 `a ?? b` 的結果會 `a`;否則，結果為 `b`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2326">If `a` is non-null, the result of `a ?? b` is `a`; otherwise, the result is `b`.</span></span> <span data-ttu-id="6aeee-2327">只有在 `a` 為 null 時，作業才會評估 `b`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2327">The operation evaluates `b` only if `a` is null.</span></span>

<span data-ttu-id="6aeee-2328">Null 聯合運算子是靠右關聯的，這表示作業會從右至左分組。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2328">The null coalescing operator is right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="6aeee-2329">例如，格式為 `a ?? b ?? c` 的運算式會評估為 `a ?? (b ?? c)`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2329">For example, an expression of the form `a ?? b ?? c` is evaluated as `a ?? (b ?? c)`.</span></span> <span data-ttu-id="6aeee-2330">一般來說，格式為 `E1 ?? E2 ?? ... ?? En` 的運算式會傳回非 null 的第一個運算元，如果所有運算元都是 null，則傳回 null。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2330">In general terms, an expression of the form `E1 ?? E2 ?? ... ?? En` returns the first of the operands that is non-null, or null if all operands are null.</span></span>

<span data-ttu-id="6aeee-2331">運算式 `a ?? b` 的類型取決於運算元上可用的隱含轉換。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2331">The type of the expression `a ?? b` depends on which implicit conversions are available on the operands.</span></span> <span data-ttu-id="6aeee-2332">依照喜好設定的順序，`a ?? b` 的類型是 `A0`、`A` 或 `B`，其中 `A` 是 `a` 的類型（假設 `a` 具有類型），`B` 是 `b` 的類型（假設 `b` 具有類型）如果 2 是可為 null 的型別，則 0 是 1 的基礎類型，否則為 3。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2332">In order of preference, the type of `a ?? b` is `A0`, `A`, or `B`, where `A` is the type of `a` (provided that `a` has a type), `B` is the type of `b` (provided that `b` has a type), and `A0` is the underlying type of `A` if `A` is a nullable type, or `A` otherwise.</span></span> <span data-ttu-id="6aeee-2333">具體而言，`a ?? b` 的處理方式如下：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2333">Specifically, `a ?? b` is processed as follows:</span></span>

*  <span data-ttu-id="6aeee-2334">如果 `A` 存在，而且不是可為 null 的型別或參考型別，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2334">If `A` exists and is not a nullable type or a reference type, a compile-time error occurs.</span></span>
*  <span data-ttu-id="6aeee-2335">如果 `b` 是動態運算式，則結果類型為 `dynamic`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2335">If `b` is a dynamic expression, the result type is `dynamic`.</span></span> <span data-ttu-id="6aeee-2336">在執行時間，會先評估 `a`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2336">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="6aeee-2337">如果 `a` 不是 null，`a` 會轉換為 dynamic，而這會變成結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2337">If `a` is not null, `a` is converted to dynamic, and this becomes the result.</span></span> <span data-ttu-id="6aeee-2338">否則，會評估 `b`，這會變成結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2338">Otherwise, `b` is evaluated, and this becomes the result.</span></span>
*  <span data-ttu-id="6aeee-2339">否則，如果 `A` 存在，而且是可為 null 的型別，且從 `b` 到 `A0` 都有隱含的轉換，則結果型別會 `A0`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2339">Otherwise, if `A` exists and is a nullable type and an implicit conversion exists from `b` to `A0`, the result type is `A0`.</span></span> <span data-ttu-id="6aeee-2340">在執行時間，會先評估 `a`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2340">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="6aeee-2341">如果 `a` 不是 null，`a` 會解除包裝為類型 `A0`，而這會變成結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2341">If `a` is not null, `a` is unwrapped to type `A0`, and this becomes the result.</span></span> <span data-ttu-id="6aeee-2342">否則，`b` 會進行評估並轉換成類型 `A0`，而這會變成結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2342">Otherwise, `b` is evaluated and converted to type `A0`, and this becomes the result.</span></span>
*  <span data-ttu-id="6aeee-2343">否則，如果 `A` 存在，且從 `b` 到 `A` 的隱含轉換存在，則結果類型為 `A`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2343">Otherwise, if `A` exists and an implicit conversion exists from `b` to `A`, the result type is `A`.</span></span> <span data-ttu-id="6aeee-2344">在執行時間，會先評估 `a`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2344">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="6aeee-2345">如果 `a` 不是 null，`a` 就會變成結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2345">If `a` is not null, `a` becomes the result.</span></span> <span data-ttu-id="6aeee-2346">否則，`b` 會進行評估並轉換成類型 `A`，而這會變成結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2346">Otherwise, `b` is evaluated and converted to type `A`, and this becomes the result.</span></span>
*  <span data-ttu-id="6aeee-2347">否則，如果 `b` 的類型 `B`，而從 `a` 到 `B` 的隱含轉換存在，則結果類型為 `B`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2347">Otherwise, if `b` has a type `B` and an implicit conversion exists from `a` to `B`, the result type is `B`.</span></span> <span data-ttu-id="6aeee-2348">在執行時間，會先評估 `a`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2348">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="6aeee-2349">如果 `a` 不是 null，`a` 會解除包裝為類型 `A0` （如果 `A` 存在且可為 null）並轉換成類型 `B`，這會變成結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2349">If `a` is not null, `a` is unwrapped to type `A0` (if `A` exists and is nullable) and converted to type `B`, and this becomes the result.</span></span> <span data-ttu-id="6aeee-2350">否則，會評估 `b`，並成為結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2350">Otherwise, `b` is evaluated and becomes the result.</span></span>
*  <span data-ttu-id="6aeee-2351">否則，`a` 和 `b` 不相容，且發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2351">Otherwise, `a` and `b` are incompatible, and a compile-time error occurs.</span></span>

## <a name="conditional-operator"></a><span data-ttu-id="6aeee-2352">條件運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-2352">Conditional operator</span></span>

<span data-ttu-id="6aeee-2353">@No__t-0 運算子稱為「條件運算子」。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2353">The `?:` operator is called the conditional operator.</span></span> <span data-ttu-id="6aeee-2354">有時候也稱為三元運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2354">It is at times also called the ternary operator.</span></span>

```antlr
conditional_expression
    : null_coalescing_expression
    | null_coalescing_expression '?' expression ':' expression
    ;
```

<span data-ttu-id="6aeee-2355">格式為 `b ? x : y` 的條件運算式會先評估 `b` 的條件。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2355">A conditional expression of the form `b ? x : y` first evaluates the condition `b`.</span></span> <span data-ttu-id="6aeee-2356">然後，如果 `b` 是 `true`，則會評估 `x`，並成為作業的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2356">Then, if `b` is `true`, `x` is evaluated and becomes the result of the operation.</span></span> <span data-ttu-id="6aeee-2357">否則，`y` 會進行評估，並成為作業的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2357">Otherwise, `y` is evaluated and becomes the result of the operation.</span></span> <span data-ttu-id="6aeee-2358">條件運算式永遠不會同時評估 `x` 和 `y`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2358">A conditional expression never evaluates both `x` and `y`.</span></span>

<span data-ttu-id="6aeee-2359">條件運算子是右向關聯，表示作業會從右至左分組。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2359">The conditional operator is right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="6aeee-2360">例如，格式為 `a ? b : c ? d : e` 的運算式會評估為 `a ? b : (c ? d : e)`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2360">For example, an expression of the form `a ? b : c ? d : e` is evaluated as `a ? b : (c ? d : e)`.</span></span>

<span data-ttu-id="6aeee-2361">@No__t-0 運算子的第一個運算元必須是可以隱含地轉換成 `bool` 的運算式，或是實 `operator true` 之型別的運算式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2361">The first operand of the `?:` operator must be an expression that can be implicitly converted to `bool`, or an expression of a type that implements `operator true`.</span></span> <span data-ttu-id="6aeee-2362">如果這兩項需求都不符合，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2362">If neither of these requirements is satisfied, a compile-time error occurs.</span></span>

<span data-ttu-id="6aeee-2363">@No__t-2 運算子的第二個和第三個運算元（`x` 和 `y`）會控制條件運算式的類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2363">The second and third operands, `x` and `y`, of the `?:` operator control the type of the conditional expression.</span></span>

*  <span data-ttu-id="6aeee-2364">如果 `x` 的類型 `X`，而且 `y` 的類型 `Y`，則</span><span class="sxs-lookup"><span data-stu-id="6aeee-2364">If `x` has type `X` and `y` has type `Y` then</span></span>
   * <span data-ttu-id="6aeee-2365">如果隱含轉換（[隱含轉換](conversions.md#implicit-conversions)）從 `X` 存在到 `Y`，但不是從 `Y` 到 `X`，則 `Y` 是條件運算式的類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2365">If an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from `X` to `Y`, but not from `Y` to `X`, then `Y` is the type of the conditional expression.</span></span>
   * <span data-ttu-id="6aeee-2366">如果隱含轉換（[隱含轉換](conversions.md#implicit-conversions)）從 `Y` 存在到 `X`，但不是從 `X` 到 `Y`，則 `X` 是條件運算式的類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2366">If an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from `Y` to `X`, but not from `X` to `Y`, then `X` is the type of the conditional expression.</span></span>
   * <span data-ttu-id="6aeee-2367">否則，就無法判斷運算式類型，且會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2367">Otherwise, no expression type can be determined, and a compile-time error occurs.</span></span>
*  <span data-ttu-id="6aeee-2368">如果只有其中一個 `x` 且 `y` 具有型別，而且 `x` 和 `y` 都可以隱含地轉換成該型別，則這就是條件運算式的型別。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2368">If only one of `x` and `y` has a type, and both `x` and `y`, of are implicitly convertible to that type, then that is the type of the conditional expression.</span></span>
*  <span data-ttu-id="6aeee-2369">否則，就無法判斷運算式類型，且會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2369">Otherwise, no expression type can be determined, and a compile-time error occurs.</span></span>

<span data-ttu-id="6aeee-2370">@No__t-0 格式之條件運算式的執行時間處理是由下列步驟所組成：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2370">The run-time processing of a conditional expression of the form `b ? x : y` consists of the following steps:</span></span>

*  <span data-ttu-id="6aeee-2371">首先會評估 `b`，並決定 `b` 的 `bool` 值：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2371">First, `b` is evaluated, and the `bool` value of `b` is determined:</span></span>
   * <span data-ttu-id="6aeee-2372">如果從 `b` 的類型到 `bool` 的隱含轉換存在，則會執行這項隱含轉換，以產生 @no__t 2 的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2372">If an implicit conversion from the type of `b` to `bool` exists, then this implicit conversion is performed to produce a `bool` value.</span></span>
   * <span data-ttu-id="6aeee-2373">否則，會叫用 `b` 類型所定義的 `operator true`，以產生 @no__t 2 的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2373">Otherwise, the `operator true` defined by the type of `b` is invoked to produce a `bool` value.</span></span>
*  <span data-ttu-id="6aeee-2374">如果上述步驟所產生的 `bool` 值是 `true`，則會評估 `x` 並轉換成條件運算式的類型，這會成為條件運算式的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2374">If the `bool` value produced by the step above is `true`, then `x` is evaluated and converted to the type of the conditional expression, and this becomes the result of the conditional expression.</span></span>
*  <span data-ttu-id="6aeee-2375">否則，`y` 會進行評估並轉換成條件運算式的類型，而這會成為條件運算式的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2375">Otherwise, `y` is evaluated and converted to the type of the conditional expression, and this becomes the result of the conditional expression.</span></span>

## <a name="anonymous-function-expressions"></a><span data-ttu-id="6aeee-2376">匿名函數運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-2376">Anonymous function expressions</span></span>

<span data-ttu-id="6aeee-2377">***匿名函數***是表示「內嵌」方法定義的運算式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2377">An ***anonymous function*** is an expression that represents an "in-line" method definition.</span></span> <span data-ttu-id="6aeee-2378">匿名函數本身並沒有值或類型，但是可以轉換成相容的委派或運算式樹狀架構類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2378">An anonymous function does not have a value or type in and of itself, but is convertible to a compatible delegate or expression tree type.</span></span> <span data-ttu-id="6aeee-2379">匿名函數轉換的評估取決於轉換的目標型別：如果它是委派型別，則轉換會評估為參考匿名函式所定義之方法的委派值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2379">The evaluation of an anonymous function conversion depends on the target type of the conversion: If it is a delegate type, the conversion evaluates to a delegate value referencing the method which the anonymous function defines.</span></span> <span data-ttu-id="6aeee-2380">如果是運算式樹狀架構型別，則轉換會評估為運算式樹狀架構，其代表方法的結構做為物件結構。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2380">If it is an expression tree type, the conversion evaluates to an expression tree which represents the structure of the method as an object structure.</span></span>

<span data-ttu-id="6aeee-2381">基於歷史原因，匿名函式有兩個語法類別，也就是*lambda_expression*s 和*anonymous_method_expression*s。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2381">For historical reasons there are two syntactic flavors of anonymous functions, namely *lambda_expression*s and *anonymous_method_expression*s.</span></span> <span data-ttu-id="6aeee-2382">對於幾乎所有的用途而言， *lambda_expression*的比*anonymous_method_expression*更精簡且易懂，而是以語言提供回溯相容性。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2382">For almost all purposes, *lambda_expression*s are more concise and expressive than *anonymous_method_expression*s, which remain in the language for backwards compatibility.</span></span>

```antlr
lambda_expression
    : anonymous_function_signature '=>' anonymous_function_body
    ;

anonymous_method_expression
    : 'delegate' explicit_anonymous_function_signature? block
    ;

anonymous_function_signature
    : explicit_anonymous_function_signature
    | implicit_anonymous_function_signature
    ;

explicit_anonymous_function_signature
    : '(' explicit_anonymous_function_parameter_list? ')'
    ;

explicit_anonymous_function_parameter_list
    : explicit_anonymous_function_parameter (',' explicit_anonymous_function_parameter)*
    ;

explicit_anonymous_function_parameter
    : anonymous_function_parameter_modifier? type identifier
    ;

anonymous_function_parameter_modifier
    : 'ref'
    | 'out'
    ;

implicit_anonymous_function_signature
    : '(' implicit_anonymous_function_parameter_list? ')'
    | implicit_anonymous_function_parameter
    ;

implicit_anonymous_function_parameter_list
    : implicit_anonymous_function_parameter (',' implicit_anonymous_function_parameter)*
    ;

implicit_anonymous_function_parameter
    : identifier
    ;

anonymous_function_body
    : expression
    | block
    ;
```

<span data-ttu-id="6aeee-2383">@No__t-0 運算子與指派（`=`）具有相同的優先順序，而且為右向關聯。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2383">The `=>` operator has the same precedence as assignment (`=`) and is right-associative.</span></span>

<span data-ttu-id="6aeee-2384">具有 `async` 修飾詞的匿名函數是非同步函式，並遵循[反覆運算](classes.md#iterators)器中所述的規則。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2384">An anonymous function with the `async` modifier is an async function and follows the rules described in [Iterators](classes.md#iterators).</span></span>

<span data-ttu-id="6aeee-2385">*Lambda_expression*形式的匿名函式的參數可以是明確或隱含類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2385">The parameters of an anonymous function in the form of a *lambda_expression* can be explicitly or implicitly typed.</span></span> <span data-ttu-id="6aeee-2386">在明確類型的參數清單中，會明確陳述每個參數的類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2386">In an explicitly typed parameter list, the type of each parameter is explicitly stated.</span></span> <span data-ttu-id="6aeee-2387">在隱含型別參數清單中，參數的型別是從匿名函式發生的內容推斷而來，具體來說，當匿名函式轉換成相容的委派型別或運算式樹狀架構型別時，該型別會提供參數類型（[匿名函數轉換](conversions.md#anonymous-function-conversions)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2387">In an implicitly typed parameter list, the types of the parameters are inferred from the context in which the anonymous function occurs—specifically, when the anonymous function is converted to a compatible delegate type or expression tree type, that type provides the parameter types ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>

<span data-ttu-id="6aeee-2388">在具有單一隱含型別參數的匿名函式中，括弧可能會從參數清單中省略。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2388">In an anonymous function with a single, implicitly typed parameter, the parentheses may be omitted from the parameter list.</span></span> <span data-ttu-id="6aeee-2389">換句話說，此表單的匿名函式</span><span class="sxs-lookup"><span data-stu-id="6aeee-2389">In other words, an anonymous function of the form</span></span>
```csharp
( param ) => expr
```
<span data-ttu-id="6aeee-2390">可以縮寫為</span><span class="sxs-lookup"><span data-stu-id="6aeee-2390">can be abbreviated to</span></span>
```csharp
param => expr
```

<span data-ttu-id="6aeee-2391">匿名函式的參數清單（以*anonymous_method_expression*的形式）是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2391">The parameter list of an anonymous function in the form of an *anonymous_method_expression* is optional.</span></span> <span data-ttu-id="6aeee-2392">如果有指定，則必須明確地輸入參數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2392">If given, the parameters must be explicitly typed.</span></span> <span data-ttu-id="6aeee-2393">如果不是，則匿名函式可以轉換成具有任何參數清單的委派，而不包含 `out` 參數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2393">If not, the anonymous function is convertible to a delegate with any parameter list not containing `out` parameters.</span></span>

<span data-ttu-id="6aeee-2394">匿名函式的*區塊*主體可以連接（[結束點和](statements.md#end-points-and-reachability)可連線性），除非匿名函式發生在無法連接的語句內。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2394">A *block* body of an anonymous function is reachable ([End points and reachability](statements.md#end-points-and-reachability)) unless the anonymous function occurs inside an unreachable statement.</span></span>

<span data-ttu-id="6aeee-2395">下列是匿名函式的一些範例：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2395">Some examples of anonymous functions follow below:</span></span>

```csharp
x => x + 1                              // Implicitly typed, expression body
x => { return x + 1; }                  // Implicitly typed, statement body
(int x) => x + 1                        // Explicitly typed, expression body
(int x) => { return x + 1; }            // Explicitly typed, statement body
(x, y) => x * y                         // Multiple parameters
() => Console.WriteLine()               // No parameters
async (t1,t2) => await t1 + await t2    // Async
delegate (int x) { return x + 1; }      // Anonymous method expression
delegate { return 1 + 1; }              // Parameter list omitted
```

<span data-ttu-id="6aeee-2396">除了下列重點以外， *lambda_expression*s 和*anonymous_method_expression*s 的行為是相同的：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2396">The behavior of *lambda_expression*s and *anonymous_method_expression*s is the same except for the following points:</span></span>

*  <span data-ttu-id="6aeee-2397">*anonymous_method_expression*s 允許完全省略參數清單，產生可轉換性以委派任何值參數清單的類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2397">*anonymous_method_expression*s permit the parameter list to be omitted entirely, yielding convertibility to delegate types of any list of value parameters.</span></span>
*  <span data-ttu-id="6aeee-2398">*lambda_expression*s 允許省略和推斷參數類型，而*anonymous_method_expression*s 則需要明確陳述參數類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2398">*lambda_expression*s permit parameter types to be omitted and inferred whereas *anonymous_method_expression*s require parameter types to be explicitly stated.</span></span>
*  <span data-ttu-id="6aeee-2399">*Lambda_expression*的主體可以是運算式或語句區塊，而*anonymous_method_expression*的主體必須是語句區塊。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2399">The body of a *lambda_expression* can be an expression or a statement block whereas the body of an *anonymous_method_expression* must be a statement block.</span></span>
*  <span data-ttu-id="6aeee-2400">只有*lambda_expression*可以轉換成相容的運算式樹狀架構類型（[運算式樹狀架構類型](types.md#expression-tree-types)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2400">Only *lambda_expression*s have conversions to compatible expression tree types ([Expression tree types](types.md#expression-tree-types)).</span></span>

### <a name="anonymous-function-signatures"></a><span data-ttu-id="6aeee-2401">匿名函式簽章</span><span class="sxs-lookup"><span data-stu-id="6aeee-2401">Anonymous function signatures</span></span>

<span data-ttu-id="6aeee-2402">匿名函式的選擇性*anonymous_function_signature*會定義匿名函數的名稱，以及選擇性的型式參數類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2402">The optional *anonymous_function_signature* of an anonymous function defines the names and optionally the types of the formal parameters for the anonymous function.</span></span> <span data-ttu-id="6aeee-2403">匿名函數的參數範圍是*anonymous_function_body*。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2403">The scope of the parameters of the anonymous function is the *anonymous_function_body*.</span></span> <span data-ttu-id="6aeee-2404">（[範圍](basic-concepts.md#scopes)）連同參數清單（如果有指定），匿名方法主體會構成宣告空間（[宣告）。](basic-concepts.md#declarations)</span><span class="sxs-lookup"><span data-stu-id="6aeee-2404">([Scopes](basic-concepts.md#scopes)) Together with the parameter list (if given) the anonymous-method-body constitutes a declaration space ([Declarations](basic-concepts.md#declarations)).</span></span> <span data-ttu-id="6aeee-2405">因此，匿名函式的參數名稱會發生編譯時期錯誤，以符合其範圍包含*anonymous_method_expression*或*lambda_expression*的本機變數、本機常數或參數的名稱。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2405">It is thus a compile-time error for the name of a parameter of the anonymous function to match the name of a local variable, local constant or parameter whose scope includes the *anonymous_method_expression* or *lambda_expression*.</span></span>

<span data-ttu-id="6aeee-2406">如果匿名函式具有*explicit_anonymous_function_signature*，則一組相容的委派類型和運算式樹狀結構類型，會限制為具有相同順序的相同參數類型和修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2406">If an anonymous function has an *explicit_anonymous_function_signature*, then the set of compatible delegate types and expression tree types is restricted to those that have the same parameter types and modifiers in the same order.</span></span> <span data-ttu-id="6aeee-2407">相較于方法群組轉換（[方法群組轉換](conversions.md#method-group-conversions)），不支援匿名函式參數類型的逆變變異。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2407">In contrast to method group conversions ([Method group conversions](conversions.md#method-group-conversions)), contra-variance of anonymous function parameter types is not supported.</span></span> <span data-ttu-id="6aeee-2408">如果匿名函式沒有*anonymous_function_signature*，則相容的委派型別和運算式樹狀架構型別集合會限制為沒有 @no__t 1 參數的類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2408">If an anonymous function does not have an *anonymous_function_signature*, then the set of compatible delegate types and expression tree types is restricted to those that have no `out` parameters.</span></span>

<span data-ttu-id="6aeee-2409">請注意， *anonymous_function_signature*不能包含屬性或參數陣列。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2409">Note that an *anonymous_function_signature* cannot include attributes or a parameter array.</span></span> <span data-ttu-id="6aeee-2410">不過， *anonymous_function_signature*可能會與參數清單包含參數陣列的委派型別相容。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2410">Nevertheless, an *anonymous_function_signature* may be compatible with a delegate type whose parameter list contains a parameter array.</span></span>

<span data-ttu-id="6aeee-2411">另請注意，即使相容，轉換成運算式樹狀架構類型仍然可能會在編譯時期（[運算式樹狀架構類型](types.md#expression-tree-types)）失敗。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2411">Note also that conversion to an expression tree type, even if compatible, may still fail at compile-time ([Expression tree types](types.md#expression-tree-types)).</span></span>

### <a name="anonymous-function-bodies"></a><span data-ttu-id="6aeee-2412">匿名函數主體</span><span class="sxs-lookup"><span data-stu-id="6aeee-2412">Anonymous function bodies</span></span>

<span data-ttu-id="6aeee-2413">匿名函式的主體（*運算式*或*區塊*）受限於下列規則：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2413">The body (*expression* or *block*) of an anonymous function is subject to the following rules:</span></span>

*  <span data-ttu-id="6aeee-2414">如果匿名函式包含簽章，則簽章中指定的參數可在本文中取得。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2414">If the anonymous function includes a signature, the parameters specified in the signature are available in the body.</span></span> <span data-ttu-id="6aeee-2415">如果匿名函式沒有簽章，它可以轉換成具有參數（匿名函式[轉換](conversions.md#anonymous-function-conversions)）的委派類型或運算式類型，但無法在主體中存取參數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2415">If the anonymous function has no signature it can be converted to a delegate type or expression type having parameters ([Anonymous function conversions](conversions.md#anonymous-function-conversions)), but the parameters cannot be accessed in the body.</span></span>
*  <span data-ttu-id="6aeee-2416">除了最接近之封閉式匿名函式的簽章（如果有的話）中所指定的 `ref` 或 @no__t 1 參數以外，本文的編譯時期錯誤會存取 `ref` 或 `out` 參數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2416">Except for `ref` or `out` parameters specified in the signature (if any) of the nearest enclosing anonymous function, it is a compile-time error for the body to access a `ref` or `out` parameter.</span></span>
*  <span data-ttu-id="6aeee-2417">當 `this` 的類型是結構類型時，本文所要存取的編譯時期錯誤 `this`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2417">When the type of `this` is a struct type, it is a compile-time error for the body to access `this`.</span></span> <span data-ttu-id="6aeee-2418">無論存取是明確的（如 `this.x`）或隱性（如 `x`，其中 `x` 是結構的實例成員）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2418">This is true whether the access is explicit (as in `this.x`) or implicit (as in `x` where `x` is an instance member of the struct).</span></span> <span data-ttu-id="6aeee-2419">此規則只會禁止這類存取，而且不會影響成員查閱結果是否在結構的成員中。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2419">This rule simply prohibits such access and does not affect whether member lookup results in a member of the struct.</span></span>
*  <span data-ttu-id="6aeee-2420">主體可以存取匿名函數的外部變數（[外部變數](expressions.md#outer-variables)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2420">The body has access to the outer variables ([Outer variables](expressions.md#outer-variables)) of the anonymous function.</span></span> <span data-ttu-id="6aeee-2421">外部變數的存取將會參考在評估*lambda_expression*或*anonymous_method_expression*時（匿名函式[運算式的評估](expressions.md#evaluation-of-anonymous-function-expressions)）所使用的變數實例。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2421">Access of an outer variable will reference the instance of the variable that is active at the time the *lambda_expression* or *anonymous_method_expression* is evaluated ([Evaluation of anonymous function expressions](expressions.md#evaluation-of-anonymous-function-expressions)).</span></span>
*  <span data-ttu-id="6aeee-2422">本文所包含的 `goto` 語句、`break` 語句或 `continue` 語句（其目標不在主體或包含匿名函式的主體內）中，會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2422">It is a compile-time error for the body to contain a `goto` statement, `break` statement, or `continue` statement whose target is outside the body or within the body of a contained anonymous function.</span></span>
*  <span data-ttu-id="6aeee-2423">主體中的 @no__t 0 語句會從最接近的封閉式匿名函式（而不是從封入函式成員）的調用中傳回控制權。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2423">A `return` statement in the body returns control from an invocation of the nearest enclosing anonymous function, not from the enclosing function member.</span></span> <span data-ttu-id="6aeee-2424">在 `return` 語句中指定的運算式，必須可以隱含地轉換成最接近的封入*lambda_expression*或*anonymous_method_expression*轉換的委派類型或運算式樹狀架構類型的傳回類型（[匿名函數轉換](conversions.md#anonymous-function-conversions)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2424">An expression specified in a `return` statement must be implicitly convertible to the return type of the delegate type or expression tree type to which the nearest enclosing *lambda_expression* or *anonymous_method_expression* is converted ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>

<span data-ttu-id="6aeee-2425">無論是否有任何方法可執行匿名函式的區塊，而不是透過*lambda_expression*或*anonymous_method_expression*的評估和調用，都可以明確指定。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2425">It is explicitly unspecified whether there is any way to execute the block of an anonymous function other than through evaluation and invocation of the *lambda_expression* or *anonymous_method_expression*.</span></span> <span data-ttu-id="6aeee-2426">特別是，編譯器可以藉由合成一或多個已命名的方法或類型，來選擇實作用中的匿名函式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2426">In particular, the compiler may choose to implement an anonymous function by synthesizing one or more named methods or types.</span></span> <span data-ttu-id="6aeee-2427">任何這類合成元素的名稱都必須是保留給編譯器使用的格式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2427">The names of any such synthesized elements must be of a form reserved for compiler use.</span></span>

### <a name="overload-resolution-and-anonymous-functions"></a><span data-ttu-id="6aeee-2428">多載解析和匿名函式</span><span class="sxs-lookup"><span data-stu-id="6aeee-2428">Overload resolution and anonymous functions</span></span>

<span data-ttu-id="6aeee-2429">引數清單中的匿名函數會參與型別推斷和多載解析。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2429">Anonymous functions in an argument list participate in type inference and overload resolution.</span></span> <span data-ttu-id="6aeee-2430">如需確切的規則，請參閱[型別推斷](expressions.md#type-inference)和多載[解析](expressions.md#overload-resolution)。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2430">Please refer to [Type inference](expressions.md#type-inference) and [Overload resolution](expressions.md#overload-resolution) for the exact rules.</span></span>

<span data-ttu-id="6aeee-2431">下列範例說明匿名函數在多載解析上的效果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2431">The following example illustrates the effect of anonymous functions on overload resolution.</span></span>

```csharp
class ItemList<T>: List<T>
{
    public int Sum(Func<T,int> selector) {
        int sum = 0;
        foreach (T item in this) sum += selector(item);
        return sum;
    }

    public double Sum(Func<T,double> selector) {
        double sum = 0;
        foreach (T item in this) sum += selector(item);
        return sum;
    }
}
```

<span data-ttu-id="6aeee-2432">@No__t 0 類別有兩個 @no__t 1 方法。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2432">The `ItemList<T>` class has two `Sum` methods.</span></span> <span data-ttu-id="6aeee-2433">每個都採用 `selector` 引數，這會將值從清單專案中解壓縮到總和。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2433">Each takes a `selector` argument, which extracts the value to sum over from a list item.</span></span> <span data-ttu-id="6aeee-2434">已解壓縮的值可以是 `int` 或 `double`，而產生的總和同樣可能是 `int` 或 `double`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2434">The extracted value can be either an `int` or a `double` and the resulting sum is likewise either an `int` or a `double`.</span></span>

<span data-ttu-id="6aeee-2435">例如，您可以使用 `Sum` 方法，從訂單中的詳細資料行清單計算總和。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2435">The `Sum` methods could for example be used to compute sums from a list of detail lines in an order.</span></span>

```csharp
class Detail
{
    public int UnitCount;
    public double UnitPrice;
    ...
}

void ComputeSums() {
    ItemList<Detail> orderDetails = GetOrderDetails(...);
    int totalUnits = orderDetails.Sum(d => d.UnitCount);
    double orderTotal = orderDetails.Sum(d => d.UnitPrice * d.UnitCount);
    ...
}
```

<span data-ttu-id="6aeee-2436">在第一次叫用 `orderDetails.Sum` 時，@no__t 1 方法都適用，因為匿名函式 `d => d. UnitCount` 與 `Func<Detail,int>` 和 `Func<Detail,double>` 相容。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2436">In the first invocation of `orderDetails.Sum`, both `Sum` methods are applicable because the anonymous function `d => d. UnitCount` is compatible with both `Func<Detail,int>` and `Func<Detail,double>`.</span></span> <span data-ttu-id="6aeee-2437">不過，多載解析會挑選第一個 `Sum` 方法，因為轉換成 `Func<Detail,int>` 比轉換成 `Func<Detail,double>` 的效果更好。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2437">However, overload resolution picks the first `Sum` method because the conversion to `Func<Detail,int>` is better than the conversion to `Func<Detail,double>`.</span></span>

<span data-ttu-id="6aeee-2438">在 `orderDetails.Sum` 的第二個調用中，只有第二個 `Sum` 方法適用，因為匿名函式 `d => d.UnitPrice * d.UnitCount` 會產生類型 `double` 的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2438">In the second invocation of `orderDetails.Sum`, only the second `Sum` method is applicable because the anonymous function `d => d.UnitPrice * d.UnitCount` produces a value of type `double`.</span></span> <span data-ttu-id="6aeee-2439">因此，多載解析會針對該調用挑選第二個 `Sum` 方法。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2439">Thus, overload resolution picks the second `Sum` method for that invocation.</span></span>

### <a name="anonymous-functions-and-dynamic-binding"></a><span data-ttu-id="6aeee-2440">匿名函數和動態繫結</span><span class="sxs-lookup"><span data-stu-id="6aeee-2440">Anonymous functions and dynamic binding</span></span>

<span data-ttu-id="6aeee-2441">匿名函式不可以是動態繫結作業的接收者、引數或運算元。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2441">An anonymous function cannot be a receiver, argument or operand of a dynamically bound operation.</span></span>

### <a name="outer-variables"></a><span data-ttu-id="6aeee-2442">外部變數</span><span class="sxs-lookup"><span data-stu-id="6aeee-2442">Outer variables</span></span>

<span data-ttu-id="6aeee-2443">其範圍包含*lambda_expression*或*anonymous_method_expression*的任何本機變數、值參數或參數陣列，都稱為匿名函式的***外部變數***。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2443">Any local variable, value parameter, or parameter array whose scope includes the *lambda_expression* or *anonymous_method_expression* is called an ***outer variable*** of the anonymous function.</span></span> <span data-ttu-id="6aeee-2444">在類別的實例函式成員中，@no__t 0 值會視為值參數，而且是函數成員中包含之任何匿名函數的外部變數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2444">In an instance function member of a class, the `this` value is considered a value parameter and is an outer variable of any anonymous function contained within the function member.</span></span>

#### <a name="captured-outer-variables"></a><span data-ttu-id="6aeee-2445">已捕捉的外部變數</span><span class="sxs-lookup"><span data-stu-id="6aeee-2445">Captured outer variables</span></span>

<span data-ttu-id="6aeee-2446">當匿名函式參考外部變數時，會將外部變數視為匿名函數所***捕捉***的。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2446">When an outer variable is referenced by an anonymous function, the outer variable is said to have been ***captured*** by the anonymous function.</span></span> <span data-ttu-id="6aeee-2447">通常，本機變數的存留期僅限於執行與它相關聯的區塊或語句（[區域變數](variables.md#local-variables)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2447">Ordinarily, the lifetime of a local variable is limited to execution of the block or statement with which it is associated ([Local variables](variables.md#local-variables)).</span></span> <span data-ttu-id="6aeee-2448">不過，已捕捉外部變數的存留期至少會延伸到從匿名函式建立的委派或運算式樹狀結構，才符合垃圾收集的資格。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2448">However, the lifetime of a captured outer variable is extended at least until the delegate or expression tree created from the anonymous function becomes eligible for garbage collection.</span></span>

<span data-ttu-id="6aeee-2449">在範例中</span><span class="sxs-lookup"><span data-stu-id="6aeee-2449">In the example</span></span>
```csharp
using System;

delegate int D();

class Test
{
    static D F() {
        int x = 0;
        D result = () => ++x;
        return result;
    }

    static void Main() {
        D d = F();
        Console.WriteLine(d());
        Console.WriteLine(d());
        Console.WriteLine(d());
    }
}
```
<span data-ttu-id="6aeee-2450">本機變數 `x` 是由匿名函式所捕捉，而 `x` 的存留期至少會擴充，直到從 `F` 傳回的委派符合垃圾收集的資格為止（這不會發生到程式的最結尾為止）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2450">the local variable `x` is captured by the anonymous function, and the lifetime of `x` is extended at least until the delegate returned from `F` becomes eligible for garbage collection (which doesn't happen until the very end of the program).</span></span> <span data-ttu-id="6aeee-2451">由於每個匿名函式的叫用都是在 `x` 的相同實例上運作，因此範例的輸出會是：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2451">Since each invocation of the anonymous function operates on the same instance of `x`, the output of the example is:</span></span>
```console
1
2
3
```

<span data-ttu-id="6aeee-2452">當匿名函式捕捉到本機變數或值參數時，區域變數或參數不會再被視為固定變數（[固定和可移動變數](unsafe-code.md#fixed-and-moveable-variables)），而是會被視為可移動變數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2452">When a local variable or a value parameter is captured by an anonymous function, the local variable or parameter is no longer considered to be a fixed variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)), but is instead considered to be a moveable variable.</span></span> <span data-ttu-id="6aeee-2453">因此，任何採用已捕捉外部變數位址的 `unsafe` 程式碼，都必須先使用 `fixed` 語句來修正變數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2453">Thus any `unsafe` code that takes the address of a captured outer variable must first use the `fixed` statement to fix the variable.</span></span>

<span data-ttu-id="6aeee-2454">請注意，與 uncaptured 變數不同的是，已捕捉的區域變數可以同時公開給多個執行執行緒。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2454">Note that unlike an uncaptured variable, a captured local variable can be simultaneously exposed to multiple threads of execution.</span></span>

#### <a name="instantiation-of-local-variables"></a><span data-ttu-id="6aeee-2455">區域變數的具現化</span><span class="sxs-lookup"><span data-stu-id="6aeee-2455">Instantiation of local variables</span></span>

<span data-ttu-id="6aeee-2456">當執行進入變數的範圍時，會將本機變數視為具現***化***。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2456">A local variable is considered to be ***instantiated*** when execution enters the scope of the variable.</span></span> <span data-ttu-id="6aeee-2457">例如，當叫用下列方法時，區域變數 `x` 會具現化並初始化三次，也就是迴圈的每個反覆運算一次。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2457">For example, when the following method is invoked, the local variable `x` is instantiated and initialized three times—once for each iteration of the loop.</span></span>

```csharp
static void F() {
    for (int i = 0; i < 3; i++) {
        int x = i * 2 + 1;
        ...
    }
}
```

<span data-ttu-id="6aeee-2458">不過，將 `x` 的宣告移至迴圈外，會產生 `x` 的單一具現化：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2458">However, moving the declaration of `x` outside the loop results in a single instantiation of `x`:</span></span>
```csharp
static void F() {
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        ...
    }
}
```

<span data-ttu-id="6aeee-2459">未捕捉到時，無法確切觀察本機變數的具現化方式，因為具現化的存留期不相鄰，所以每個具現化都可能只使用相同的儲存位置。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2459">When not captured, there is no way to observe exactly how often a local variable is instantiated—because the lifetimes of the instantiations are disjoint, it is possible for each instantiation to simply use the same storage location.</span></span> <span data-ttu-id="6aeee-2460">不過，當匿名函式捕捉到本機變數時，具現化的效果會變得很明顯。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2460">However, when an anonymous function captures a local variable, the effects of instantiation become apparent.</span></span>

<span data-ttu-id="6aeee-2461">範例</span><span class="sxs-lookup"><span data-stu-id="6aeee-2461">The example</span></span>
```csharp
using System;

delegate void D();

class Test
{
    static D[] F() {
        D[] result = new D[3];
        for (int i = 0; i < 3; i++) {
            int x = i * 2 + 1;
            result[i] = () => { Console.WriteLine(x); };
        }
        return result;
    }

    static void Main() {
        foreach (D d in F()) d();
    }
}
```
<span data-ttu-id="6aeee-2462">產生下列輸出：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2462">produces the output:</span></span>
```console
1
3
5
```

<span data-ttu-id="6aeee-2463">不過，當 `x` 的宣告移至迴圈外時：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2463">However, when the declaration of `x` is moved outside the loop:</span></span>
```csharp
static D[] F() {
    D[] result = new D[3];
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        result[i] = () => { Console.WriteLine(x); };
    }
    return result;
}
```
<span data-ttu-id="6aeee-2464">輸出為：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2464">the output is:</span></span>
```console
5
5
5
```

<span data-ttu-id="6aeee-2465">如果 for 迴圈宣告反復專案變數，該變數本身會被視為在迴圈外部宣告。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2465">If a for-loop declares an iteration variable, that variable itself is considered to be declared outside of the loop.</span></span> <span data-ttu-id="6aeee-2466">因此，如果此範例已變更為捕捉反覆運算變數本身：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2466">Thus, if the example is changed to capture the iteration variable itself:</span></span>

```csharp
static D[] F() {
    D[] result = new D[3];
    for (int i = 0; i < 3; i++) {
        result[i] = () => { Console.WriteLine(i); };
    }
    return result;
}
```
<span data-ttu-id="6aeee-2467">只會捕捉反覆運算變數的一個實例，這會產生輸出：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2467">only one instance of the iteration variable is captured, which produces the output:</span></span>
```console
3
3
3
```

<span data-ttu-id="6aeee-2468">匿名函式委派可以共用一些已捕捉的變數，但卻有其他的實例。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2468">It is possible for anonymous function delegates to share some captured variables yet have separate instances of others.</span></span> <span data-ttu-id="6aeee-2469">例如，如果 `F` 變更為</span><span class="sxs-lookup"><span data-stu-id="6aeee-2469">For example, if `F` is changed to</span></span>
```csharp
static D[] F() {
    D[] result = new D[3];
    int x = 0;
    for (int i = 0; i < 3; i++) {
        int y = 0;
        result[i] = () => { Console.WriteLine("{0} {1}", ++x, ++y); };
    }
    return result;
}
```
<span data-ttu-id="6aeee-2470">這三個委派會捕捉相同的 `x` 實例，但 `y` 的個別實例，輸出如下：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2470">the three delegates capture the same instance of `x` but separate instances of `y`, and the output is:</span></span>
```console
1 1
2 1
3 1
```

<span data-ttu-id="6aeee-2471">個別的匿名函式可以抓取外部變數的相同實例。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2471">Separate anonymous functions can capture the same instance of an outer variable.</span></span> <span data-ttu-id="6aeee-2472">在下列範例中：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2472">In the example:</span></span>
```csharp
using System;

delegate void Setter(int value);

delegate int Getter();

class Test
{
    static void Main() {
        int x = 0;
        Setter s = (int value) => { x = value; };
        Getter g = () => { return x; };
        s(5);
        Console.WriteLine(g());
        s(10);
        Console.WriteLine(g());
    }
}
```
<span data-ttu-id="6aeee-2473">這兩個匿名函式會 `x` 來捕捉相同的本機變數實例，因此可以透過該變數「通訊」。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2473">the two anonymous functions capture the same instance of the local variable `x`, and they can thus "communicate" through that variable.</span></span> <span data-ttu-id="6aeee-2474">範例的輸出為：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2474">The output of the example is:</span></span>
```console
5
10
```

### <a name="evaluation-of-anonymous-function-expressions"></a><span data-ttu-id="6aeee-2475">評估匿名函數運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-2475">Evaluation of anonymous function expressions</span></span>

<span data-ttu-id="6aeee-2476">匿名函數 `F` 必須一律轉換成委派類型 `D` 或運算式樹狀架構類型 `E`，不論是直接或透過執行委派建立運算式 `new D(F)`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2476">An anonymous function `F` must always be converted to a delegate type `D` or an expression tree type `E`, either directly or through the execution of a delegate creation expression `new D(F)`.</span></span> <span data-ttu-id="6aeee-2477">此轉換會判斷匿名函式的結果，如[匿名函數轉換](conversions.md#anonymous-function-conversions)中所述。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2477">This conversion determines the result of the anonymous function, as described in [Anonymous function conversions](conversions.md#anonymous-function-conversions).</span></span>

## <a name="query-expressions"></a><span data-ttu-id="6aeee-2478">查詢運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-2478">Query expressions</span></span>

<span data-ttu-id="6aeee-2479">***查詢運算式***針對類似關聯式和階層式查詢語言（例如 SQL 和 XQuery）的查詢提供了語言整合的語法。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2479">***Query expressions*** provide a language integrated syntax for queries that is similar to relational and hierarchical query languages such as SQL and XQuery.</span></span>

```antlr
query_expression
    : from_clause query_body
    ;

from_clause
    : 'from' type? identifier 'in' expression
    ;

query_body
    : query_body_clauses? select_or_group_clause query_continuation?
    ;

query_body_clauses
    : query_body_clause
    | query_body_clauses query_body_clause
    ;

query_body_clause
    : from_clause
    | let_clause
    | where_clause
    | join_clause
    | join_into_clause
    | orderby_clause
    ;

let_clause
    : 'let' identifier '=' expression
    ;

where_clause
    : 'where' boolean_expression
    ;

join_clause
    : 'join' type? identifier 'in' expression 'on' expression 'equals' expression
    ;

join_into_clause
    : 'join' type? identifier 'in' expression 'on' expression 'equals' expression 'into' identifier
    ;

orderby_clause
    : 'orderby' orderings
    ;

orderings
    : ordering (',' ordering)*
    ;

ordering
    : expression ordering_direction?
    ;

ordering_direction
    : 'ascending'
    | 'descending'
    ;

select_or_group_clause
    : select_clause
    | group_clause
    ;

select_clause
    : 'select' expression
    ;

group_clause
    : 'group' expression 'by' expression
    ;

query_continuation
    : 'into' identifier query_body
    ;
```

<span data-ttu-id="6aeee-2480">查詢運算式的開頭為 `from` 子句，並以 `select` 或 @no__t 2 子句結束。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2480">A query expression begins with a `from` clause and ends with either a `select` or `group` clause.</span></span> <span data-ttu-id="6aeee-2481">初始的 `from` 子句後面可以接著零個或多個 `from`、@no__t 2、`where`、`join` 或 `orderby` 子句。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2481">The initial `from` clause can be followed by zero or more `from`, `let`, `where`, `join` or `orderby` clauses.</span></span> <span data-ttu-id="6aeee-2482">每個 @no__t 0 子句都是一個產生器的產生器，其範圍***變數***會超出***序列***的元素範圍。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2482">Each `from` clause is a generator introducing a ***range variable*** which ranges over the elements of a ***sequence***.</span></span> <span data-ttu-id="6aeee-2483">每個 @no__t 0 子句都會引進一個範圍變數，代表以先前範圍變數計算的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2483">Each `let` clause introduces a range variable representing a value computed by means of previous range variables.</span></span> <span data-ttu-id="6aeee-2484">每個 @no__t 0 子句都是一個篩選準則，會從結果中排除專案。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2484">Each `where` clause is a filter that excludes items from the result.</span></span> <span data-ttu-id="6aeee-2485">每個 @no__t 0 子句都會比較來源序列的指定索引鍵與另一個序列的索引鍵，並產生相符的配對。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2485">Each `join` clause compares specified keys of the source sequence with keys of another sequence, yielding matching pairs.</span></span> <span data-ttu-id="6aeee-2486">每個 @no__t 0 子句都會根據指定的準則重新排序專案。最後的 `select` 或 @no__t 2 子句會根據範圍變數來指定結果的形狀。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2486">Each `orderby` clause reorders items according to specified criteria.The final `select` or `group` clause specifies the shape of the result in terms of the range variables.</span></span> <span data-ttu-id="6aeee-2487">最後，@no__t 0 子句可以在後續查詢中將一個查詢的結果視為產生器，藉以「拼接」查詢。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2487">Finally, an `into` clause can be used to "splice" queries by treating the results of one query as a generator in a subsequent query.</span></span>

### <a name="ambiguities-in-query-expressions"></a><span data-ttu-id="6aeee-2488">查詢運算式中的多義性</span><span class="sxs-lookup"><span data-stu-id="6aeee-2488">Ambiguities in query expressions</span></span>

<span data-ttu-id="6aeee-2489">查詢運算式包含多個「內容關鍵字」，也就是在指定內容中有特殊意義的識別碼。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2489">Query expressions contain a number of "contextual keywords", i.e., identifiers that have special meaning in a given context.</span></span> <span data-ttu-id="6aeee-2490">具體而言，這些是 `from`、`where`、`join`、`on`、`equals`、`into`、`let`、`orderby`、`ascending`、`descending`、0、1 和 2。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2490">Specifically these are `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` and `by`.</span></span> <span data-ttu-id="6aeee-2491">為了避免因混合使用這些識別碼做為關鍵字或簡單名稱而造成的查詢運算式不明確，這些識別碼會在查詢運算式中的任何位置發生時被視為關鍵字。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2491">In order to avoid ambiguities in query expressions caused by mixed use of these identifiers as keywords or simple names, these identifiers are considered keywords when occurring anywhere within a query expression.</span></span>

<span data-ttu-id="6aeee-2492">基於此目的，查詢運算式是以 "`from identifier`" 開頭，後面接著任何 token （"`;`"、"`=`" 或 "`,`" 除外）的運算式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2492">For this purpose, a query expression is any expression that starts with "`from identifier`" followed by any token except "`;`", "`=`" or "`,`".</span></span>

<span data-ttu-id="6aeee-2493">為了在查詢運算式中使用這些單字做為識別碼，它們前面可以加上 "`@`" （[識別碼](lexical-structure.md#identifiers)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2493">In order to use these words as identifiers within a query expression, they can be prefixed with "`@`" ([Identifiers](lexical-structure.md#identifiers)).</span></span>

### <a name="query-expression-translation"></a><span data-ttu-id="6aeee-2494">查詢運算式轉譯</span><span class="sxs-lookup"><span data-stu-id="6aeee-2494">Query expression translation</span></span>

<span data-ttu-id="6aeee-2495">此C#語言不會指定查詢運算式的執行語法。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2495">The C# language does not specify the execution semantics of query expressions.</span></span> <span data-ttu-id="6aeee-2496">相反地，查詢運算式會轉譯成符合*查詢運算式模式*之方法的調用（[查詢運算式模式](expressions.md#the-query-expression-pattern)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2496">Rather, query expressions are translated into invocations of methods that adhere to the *query expression pattern* ([The query expression pattern](expressions.md#the-query-expression-pattern)).</span></span> <span data-ttu-id="6aeee-2497">具體而言，查詢運算式會轉譯為名為 `Where`、`Select`、`SelectMany`、`Join`、`GroupJoin`、`OrderBy`、`OrderByDescending`、`ThenBy`、`ThenByDescending`、`GroupBy` 和 0 的方法調用。這些方法預期會有特定的簽章和結果類型，如[查詢運算式模式](expressions.md#the-query-expression-pattern)中所述。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2497">Specifically, query expressions are translated into invocations of methods named `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy`, and `Cast`.These methods are expected to have particular signatures and result types, as described in [The query expression pattern](expressions.md#the-query-expression-pattern).</span></span> <span data-ttu-id="6aeee-2498">這些方法可以是所查詢物件的實例方法，或是物件外部的擴充方法，並會執行查詢的實際執行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2498">These methods can be instance methods of the object being queried or extension methods that are external to the object, and they implement the actual execution of the query.</span></span>

<span data-ttu-id="6aeee-2499">從查詢運算式到方法調用的轉譯，是在執行任何類型系結或多載解析之前發生的語法對應。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2499">The translation from query expressions to method invocations is a syntactic mapping that occurs before any type binding or overload resolution has been performed.</span></span> <span data-ttu-id="6aeee-2500">翻譯保證的語法正確，但不保證會產生語義正確C#的程式碼。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2500">The translation is guaranteed to be syntactically correct, but it is not guaranteed to produce semantically correct C# code.</span></span> <span data-ttu-id="6aeee-2501">在查詢運算式的轉譯之後，所產生的方法調用會當做一般的方法叫用來處理，而這可能會發現錯誤，例如，如果方法不存在、引數的類型錯誤，或方法為泛型，則為。型別推斷失敗。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2501">Following translation of query expressions, the resulting method invocations are processed as regular method invocations, and this may in turn uncover errors, for example if the methods do not exist, if arguments have wrong types, or if the methods are generic and type inference fails.</span></span>

<span data-ttu-id="6aeee-2502">查詢運算式的處理方式是重複套用下列翻譯，直到無法進一步縮減為止。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2502">A query expression is processed by repeatedly applying the following translations until no further reductions are possible.</span></span> <span data-ttu-id="6aeee-2503">翻譯會依照應用程式的順序列出：每一節都假設先前章節中的翻譯已徹底執行，一旦用盡後，就不會在處理相同的查詢運算式時再次使用區段。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2503">The translations are listed in order of application: each section assumes that the translations in the preceding sections have been performed exhaustively, and once exhausted, a section will not later be revisited in the processing of the same query expression.</span></span>

<span data-ttu-id="6aeee-2504">查詢運算式中不允許指派至範圍變數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2504">Assignment to range variables is not allowed in query expressions.</span></span> <span data-ttu-id="6aeee-2505">C#不過，允許不一定會強制執行這種限制，因為這有時候可能無法使用此處所提供的語法轉譯配置。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2505">However a C# implementation is permitted to not always enforce this restriction, since this may sometimes not be possible with the syntactic translation scheme presented here.</span></span>

<span data-ttu-id="6aeee-2506">某些翻譯會插入範圍變數，其中具有以 `*` 表示的透明識別碼。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2506">Certain translations inject range variables with transparent identifiers denoted by `*`.</span></span> <span data-ttu-id="6aeee-2507">透明識別碼的特殊屬性會在[透明識別碼](expressions.md#transparent-identifiers)中進一步討論。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2507">The special properties of transparent identifiers are discussed further in [Transparent identifiers](expressions.md#transparent-identifiers).</span></span>

#### <a name="select-and-groupby-clauses-with-continuations"></a><span data-ttu-id="6aeee-2508">具有接續的 Select 和 groupby 子句</span><span class="sxs-lookup"><span data-stu-id="6aeee-2508">Select and groupby clauses with continuations</span></span>

<span data-ttu-id="6aeee-2509">具有接續的查詢運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-2509">A query expression with a continuation</span></span>
```csharp
from ... into x ...
```
<span data-ttu-id="6aeee-2510">會轉譯為</span><span class="sxs-lookup"><span data-stu-id="6aeee-2510">is translated into</span></span>
```csharp
from x in ( from ... ) ...
```

<span data-ttu-id="6aeee-2511">下列各節中的翻譯假設查詢沒有任何 @no__t 0 的接續。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2511">The translations in the following sections assume that queries have no `into` continuations.</span></span>

<span data-ttu-id="6aeee-2512">範例</span><span class="sxs-lookup"><span data-stu-id="6aeee-2512">The example</span></span>
```csharp
from c in customers
group c by c.Country into g
select new { Country = g.Key, CustCount = g.Count() }
```
<span data-ttu-id="6aeee-2513">會轉譯為</span><span class="sxs-lookup"><span data-stu-id="6aeee-2513">is translated into</span></span>
```csharp
from g in
    from c in customers
    group c by c.Country
select new { Country = g.Key, CustCount = g.Count() }
```
<span data-ttu-id="6aeee-2514">最終的轉譯是</span><span class="sxs-lookup"><span data-stu-id="6aeee-2514">the final translation of which is</span></span>
```csharp
customers.
GroupBy(c => c.Country).
Select(g => new { Country = g.Key, CustCount = g.Count() })
```

#### <a name="explicit-range-variable-types"></a><span data-ttu-id="6aeee-2515">明確範圍變數類型</span><span class="sxs-lookup"><span data-stu-id="6aeee-2515">Explicit range variable types</span></span>

<span data-ttu-id="6aeee-2516">明確指定範圍變數類型的 @no__t 0 子句</span><span class="sxs-lookup"><span data-stu-id="6aeee-2516">A `from` clause that explicitly specifies a range variable type</span></span>
```csharp
from T x in e
```
<span data-ttu-id="6aeee-2517">會轉譯為</span><span class="sxs-lookup"><span data-stu-id="6aeee-2517">is translated into</span></span>
```csharp
from x in ( e ) . Cast < T > ( )
```

<span data-ttu-id="6aeee-2518">明確指定範圍變數類型的 @no__t 0 子句</span><span class="sxs-lookup"><span data-stu-id="6aeee-2518">A `join` clause that explicitly specifies a range variable type</span></span>
```csharp
join T x in e on k1 equals k2
```
<span data-ttu-id="6aeee-2519">會轉譯為</span><span class="sxs-lookup"><span data-stu-id="6aeee-2519">is translated into</span></span>
```csharp
join x in ( e ) . Cast < T > ( ) on k1 equals k2
```

<span data-ttu-id="6aeee-2520">下列各節中的翻譯假設查詢沒有明確的範圍變數類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2520">The translations in the following sections assume that queries have no explicit range variable types.</span></span>

<span data-ttu-id="6aeee-2521">範例</span><span class="sxs-lookup"><span data-stu-id="6aeee-2521">The example</span></span>
```csharp
from Customer c in customers
where c.City == "London"
select c
```
<span data-ttu-id="6aeee-2522">會轉譯為</span><span class="sxs-lookup"><span data-stu-id="6aeee-2522">is translated into</span></span>
```csharp
from c in customers.Cast<Customer>()
where c.City == "London"
select c
```
<span data-ttu-id="6aeee-2523">最終的轉譯是</span><span class="sxs-lookup"><span data-stu-id="6aeee-2523">the final translation of which is</span></span>
```csharp
customers.
Cast<Customer>().
Where(c => c.City == "London")
```

<span data-ttu-id="6aeee-2524">明確範圍變數類型有助於查詢實作為非泛型 @no__t 0 介面的集合，而不是泛型的 @no__t 1 介面。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2524">Explicit range variable types are useful for querying collections that implement the non-generic `IEnumerable` interface, but not the generic `IEnumerable<T>` interface.</span></span> <span data-ttu-id="6aeee-2525">在上述範例中，如果 `customers` 的類型 `ArrayList`，就會發生這種情況。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2525">In the example above, this would be the case if `customers` were of type `ArrayList`.</span></span>

#### <a name="degenerate-query-expressions"></a><span data-ttu-id="6aeee-2526">退化查詢運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-2526">Degenerate query expressions</span></span>

<span data-ttu-id="6aeee-2527">表單的查詢運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-2527">A query expression of the form</span></span>
```csharp
from x in e select x
```
<span data-ttu-id="6aeee-2528">會轉譯為</span><span class="sxs-lookup"><span data-stu-id="6aeee-2528">is translated into</span></span>
```csharp
( e ) . Select ( x => x )
```

<span data-ttu-id="6aeee-2529">範例</span><span class="sxs-lookup"><span data-stu-id="6aeee-2529">The example</span></span>
```csharp
from c in customers
select c
```
<span data-ttu-id="6aeee-2530">會轉譯為</span><span class="sxs-lookup"><span data-stu-id="6aeee-2530">is translated into</span></span>
```csharp
customers.Select(c => c)
```

<span data-ttu-id="6aeee-2531">「退化查詢運算式」（完整）會選取來源的元素。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2531">A degenerate query expression is one that trivially selects the elements of the source.</span></span> <span data-ttu-id="6aeee-2532">翻譯的較新階段會移除其他轉譯步驟引進的退化查詢，方法是將它們取代為其來源。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2532">A later phase of the translation removes degenerate queries introduced by other translation steps by replacing them with their source.</span></span> <span data-ttu-id="6aeee-2533">不過，請務必確保查詢運算式的結果永遠不是來源物件本身，因為這會向查詢的用戶端顯示來源的類型和識別。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2533">It is important however to ensure that the result of a query expression is never the source object itself, as that would reveal the type and identity of the source to the client of the query.</span></span> <span data-ttu-id="6aeee-2534">因此，此步驟會在來源上明確呼叫 `Select`，以保護直接在原始程式碼中寫入的退化查詢。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2534">Therefore this step protects degenerate queries written directly in source code by explicitly calling `Select` on the source.</span></span> <span data-ttu-id="6aeee-2535">然後，由 @no__t 0 和其他查詢運算子的實作者負責，以確保這些方法永遠不會傳回來源物件本身。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2535">It is then up to the implementers of `Select` and other query operators to ensure that these methods never return the source object itself.</span></span>

#### <a name="from-let-where-join-and-orderby-clauses"></a><span data-ttu-id="6aeee-2536">From、let、where、join 和 orderby 子句</span><span class="sxs-lookup"><span data-stu-id="6aeee-2536">From, let, where, join and orderby clauses</span></span>

<span data-ttu-id="6aeee-2537">含有第二個 `from` 子句且後面接著 `select` 子句的查詢運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-2537">A query expression with a second `from` clause followed by a `select` clause</span></span>
```csharp
from x1 in e1
from x2 in e2
select v
```
<span data-ttu-id="6aeee-2538">會轉譯為</span><span class="sxs-lookup"><span data-stu-id="6aeee-2538">is translated into</span></span>
```csharp
( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => v )
```

<span data-ttu-id="6aeee-2539">含有第二個 `from` 子句的查詢運算式，後面接著 `select` 子句以外的專案：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2539">A query expression with a second `from` clause followed by something other than a `select` clause:</span></span>

```csharp
from x1 in e1
from x2 in e2
...
```
<span data-ttu-id="6aeee-2540">會轉譯為</span><span class="sxs-lookup"><span data-stu-id="6aeee-2540">is translated into</span></span>
```csharp
from * in ( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => new { x1 , x2 } )
...
```

<span data-ttu-id="6aeee-2541">具有 `let` 子句的查詢運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-2541">A query expression with a `let` clause</span></span>
```csharp
from x in e
let y = f
...
```
<span data-ttu-id="6aeee-2542">會轉譯為</span><span class="sxs-lookup"><span data-stu-id="6aeee-2542">is translated into</span></span>
```csharp
from * in ( e ) . Select ( x => new { x , y = f } )
...
```

<span data-ttu-id="6aeee-2543">具有 `where` 子句的查詢運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-2543">A query expression with a `where` clause</span></span>
```csharp
from x in e
where f
...
```
<span data-ttu-id="6aeee-2544">會轉譯為</span><span class="sxs-lookup"><span data-stu-id="6aeee-2544">is translated into</span></span>
```csharp
from x in ( e ) . Where ( x => f )
...
```

<span data-ttu-id="6aeee-2545">具有 `join` 子句的查詢運算式，但不含 `into`，後面接著 `select` 子句</span><span class="sxs-lookup"><span data-stu-id="6aeee-2545">A query expression with a `join` clause without an `into` followed by a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
select v
```
<span data-ttu-id="6aeee-2546">會轉譯為</span><span class="sxs-lookup"><span data-stu-id="6aeee-2546">is translated into</span></span>
```csharp
( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => v )
```

<span data-ttu-id="6aeee-2547">具有 `join` 子句的查詢運算式，但不含 `into`，後面接著 `select` 子句以外的專案</span><span class="sxs-lookup"><span data-stu-id="6aeee-2547">A query expression with a `join` clause without an `into` followed by something other than a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
...
```
<span data-ttu-id="6aeee-2548">會轉譯為</span><span class="sxs-lookup"><span data-stu-id="6aeee-2548">is translated into</span></span>
```csharp
from * in ( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => new { x1 , x2 })
...
```

<span data-ttu-id="6aeee-2549">具有 `join` 子句的查詢運算式，其中 `into`，後面接著 `select` 子句</span><span class="sxs-lookup"><span data-stu-id="6aeee-2549">A query expression with a `join` clause with an `into` followed by a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
select v
```
<span data-ttu-id="6aeee-2550">會轉譯為</span><span class="sxs-lookup"><span data-stu-id="6aeee-2550">is translated into</span></span>
```csharp
( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => v )
```

<span data-ttu-id="6aeee-2551">具有 `join` 子句的查詢運算式，其中 `into`，後面接著 `select` 子句以外的某個專案</span><span class="sxs-lookup"><span data-stu-id="6aeee-2551">A query expression with a `join` clause with an `into` followed by something other than a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
...
```
<span data-ttu-id="6aeee-2552">會轉譯為</span><span class="sxs-lookup"><span data-stu-id="6aeee-2552">is translated into</span></span>
```csharp
from * in ( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => new { x1 , g })
...
```

<span data-ttu-id="6aeee-2553">具有 `orderby` 子句的查詢運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-2553">A query expression with an `orderby` clause</span></span>
```csharp
from x in e
orderby k1 , k2 , ..., kn
...
```
<span data-ttu-id="6aeee-2554">會轉譯為</span><span class="sxs-lookup"><span data-stu-id="6aeee-2554">is translated into</span></span>
```csharp
from x in ( e ) . 
OrderBy ( x => k1 ) . 
ThenBy ( x => k2 ) .
... .
ThenBy ( x => kn )
...
```

<span data-ttu-id="6aeee-2555">如果排序子句指定 `descending` 方向指標，則會改為產生 `OrderByDescending` 或 `ThenByDescending` 的調用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2555">If an ordering clause specifies a `descending` direction indicator, an invocation of `OrderByDescending` or `ThenByDescending` is produced instead.</span></span>

<span data-ttu-id="6aeee-2556">下列翻譯假設沒有 `let`、`where`、`join` 或 `orderby` 子句，而且在每個查詢運算式中都不能超過一個初始的 `from` 子句。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2556">The following translations assume that there are no `let`, `where`, `join` or `orderby` clauses, and no more than the one initial `from` clause in each query expression.</span></span>

<span data-ttu-id="6aeee-2557">範例</span><span class="sxs-lookup"><span data-stu-id="6aeee-2557">The example</span></span>
```csharp
from c in customers
from o in c.Orders
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="6aeee-2558">會轉譯為</span><span class="sxs-lookup"><span data-stu-id="6aeee-2558">is translated into</span></span>
```csharp
customers.
SelectMany(c => c.Orders,
     (c,o) => new { c.Name, o.OrderID, o.Total }
)
```

<span data-ttu-id="6aeee-2559">範例</span><span class="sxs-lookup"><span data-stu-id="6aeee-2559">The example</span></span>
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="6aeee-2560">會轉譯為</span><span class="sxs-lookup"><span data-stu-id="6aeee-2560">is translated into</span></span>
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="6aeee-2561">最終的轉譯是</span><span class="sxs-lookup"><span data-stu-id="6aeee-2561">the final translation of which is</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.OrderID, x.o.Total })
```
<span data-ttu-id="6aeee-2562">其中 `x` 是編譯器產生的識別碼，在其他情況下不可見且無法存取。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2562">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="6aeee-2563">範例</span><span class="sxs-lookup"><span data-stu-id="6aeee-2563">The example</span></span>
```csharp
from o in orders
let t = o.Details.Sum(d => d.UnitPrice * d.Quantity)
where t >= 1000
select new { o.OrderID, Total = t }
```
<span data-ttu-id="6aeee-2564">會轉譯為</span><span class="sxs-lookup"><span data-stu-id="6aeee-2564">is translated into</span></span>
```csharp
from * in orders.
    Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) })
where t >= 1000 
select new { o.OrderID, Total = t }
```
<span data-ttu-id="6aeee-2565">最終的轉譯是</span><span class="sxs-lookup"><span data-stu-id="6aeee-2565">the final translation of which is</span></span>
```csharp
orders.
Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) }).
Where(x => x.t >= 1000).
Select(x => new { x.o.OrderID, Total = x.t })
```
<span data-ttu-id="6aeee-2566">其中 `x` 是編譯器產生的識別碼，在其他情況下不可見且無法存取。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2566">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="6aeee-2567">範例</span><span class="sxs-lookup"><span data-stu-id="6aeee-2567">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
select new { c.Name, o.OrderDate, o.Total }
```
<span data-ttu-id="6aeee-2568">會轉譯為</span><span class="sxs-lookup"><span data-stu-id="6aeee-2568">is translated into</span></span>
```csharp
customers.Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c.Name, o.OrderDate, o.Total })
```

<span data-ttu-id="6aeee-2569">範例</span><span class="sxs-lookup"><span data-stu-id="6aeee-2569">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID into co
let n = co.Count()
where n >= 10
select new { c.Name, OrderCount = n }
```
<span data-ttu-id="6aeee-2570">會轉譯為</span><span class="sxs-lookup"><span data-stu-id="6aeee-2570">is translated into</span></span>
```csharp
from * in customers.
    GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
        (c, co) => new { c, co })
let n = co.Count()
where n >= 10 
select new { c.Name, OrderCount = n }
```
<span data-ttu-id="6aeee-2571">最終的轉譯是</span><span class="sxs-lookup"><span data-stu-id="6aeee-2571">the final translation of which is</span></span>
```csharp
customers.
GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
    (c, co) => new { c, co }).
Select(x => new { x, n = x.co.Count() }).
Where(y => y.n >= 10).
Select(y => new { y.x.c.Name, OrderCount = y.n)
```
<span data-ttu-id="6aeee-2572">其中 `x` 和 `y` 是編譯器產生的識別碼，在其他情況下不可見且無法存取。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2572">where `x` and `y` are compiler generated identifiers that are otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="6aeee-2573">範例</span><span class="sxs-lookup"><span data-stu-id="6aeee-2573">The example</span></span>
```csharp
from o in orders
orderby o.Customer.Name, o.Total descending
select o
```
<span data-ttu-id="6aeee-2574">具有最終翻譯</span><span class="sxs-lookup"><span data-stu-id="6aeee-2574">has the final translation</span></span>
```csharp
orders.
OrderBy(o => o.Customer.Name).
ThenByDescending(o => o.Total)
```

#### <a name="select-clauses"></a><span data-ttu-id="6aeee-2575">選取子句</span><span class="sxs-lookup"><span data-stu-id="6aeee-2575">Select clauses</span></span>

<span data-ttu-id="6aeee-2576">表單的查詢運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-2576">A query expression of the form</span></span>
```csharp
from x in e select v
```
<span data-ttu-id="6aeee-2577">會轉譯為</span><span class="sxs-lookup"><span data-stu-id="6aeee-2577">is translated into</span></span>
```csharp
( e ) . Select ( x => v )
```
<span data-ttu-id="6aeee-2578">除了 v 是識別碼 x 以外，翻譯只是</span><span class="sxs-lookup"><span data-stu-id="6aeee-2578">except when v is the identifier x, the translation is simply</span></span>
```csharp
( e )
```

<span data-ttu-id="6aeee-2579">例如：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2579">For example</span></span>
```csharp
from c in customers.Where(c => c.City == "London")
select c
```
<span data-ttu-id="6aeee-2580">只會轉譯成</span><span class="sxs-lookup"><span data-stu-id="6aeee-2580">is simply translated into</span></span>
```csharp
customers.Where(c => c.City == "London")
```

#### <a name="groupby-clauses"></a><span data-ttu-id="6aeee-2581">Groupby 子句</span><span class="sxs-lookup"><span data-stu-id="6aeee-2581">Groupby clauses</span></span>

<span data-ttu-id="6aeee-2582">表單的查詢運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-2582">A query expression of the form</span></span>
```csharp
from x in e group v by k
```
<span data-ttu-id="6aeee-2583">會轉譯為</span><span class="sxs-lookup"><span data-stu-id="6aeee-2583">is translated into</span></span>
```csharp
( e ) . GroupBy ( x => k , x => v )
```
<span data-ttu-id="6aeee-2584">除了 v 為識別碼 x 以外，轉譯為</span><span class="sxs-lookup"><span data-stu-id="6aeee-2584">except when v is the identifier x, the translation is</span></span>
```csharp
( e ) . GroupBy ( x => k )
```

<span data-ttu-id="6aeee-2585">範例</span><span class="sxs-lookup"><span data-stu-id="6aeee-2585">The example</span></span>
```csharp
from c in customers
group c.Name by c.Country
```
<span data-ttu-id="6aeee-2586">會轉譯為</span><span class="sxs-lookup"><span data-stu-id="6aeee-2586">is translated into</span></span>
```csharp
customers.
GroupBy(c => c.Country, c => c.Name)
```

#### <a name="transparent-identifiers"></a><span data-ttu-id="6aeee-2587">透明識別碼</span><span class="sxs-lookup"><span data-stu-id="6aeee-2587">Transparent identifiers</span></span>

<span data-ttu-id="6aeee-2588">某些翻譯會插入範圍變數，其中具有以 `*` 表示的***透明識別碼***。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2588">Certain translations inject range variables with ***transparent identifiers*** denoted by `*`.</span></span> <span data-ttu-id="6aeee-2589">透明識別碼不是適當的語言功能;它們只存在於查詢運算式轉譯程式中的中繼步驟。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2589">Transparent identifiers are not a proper language feature; they exist only as an intermediate step in the query expression translation process.</span></span>

<span data-ttu-id="6aeee-2590">當查詢轉譯插入透明識別碼時，進一步的轉譯步驟會將透明識別碼傳播至匿名函式和匿名物件初始化運算式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2590">When a query translation injects a transparent identifier, further translation steps propagate the transparent identifier into anonymous functions and anonymous object initializers.</span></span> <span data-ttu-id="6aeee-2591">在這些內容中，透明識別碼具有下列行為：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2591">In those contexts, transparent identifiers have the following behavior:</span></span>

*  <span data-ttu-id="6aeee-2592">當透明識別碼當做匿名函式中的參數出現時，相關聯匿名型別的成員會自動在匿名函式主體的範圍內。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2592">When a transparent identifier occurs as a parameter in an anonymous function, the members of the associated anonymous type are automatically in scope in the body of the anonymous function.</span></span>
*  <span data-ttu-id="6aeee-2593">當具有透明識別碼的成員在範圍內時，該成員的成員也會在範圍中。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2593">When a member with a transparent identifier is in scope, the members of that member are in scope as well.</span></span>
*  <span data-ttu-id="6aeee-2594">當透明識別碼當做匿名物件初始化運算式中的成員宣告子出現時，它會引進具有透明識別碼的成員。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2594">When a transparent identifier occurs as a member declarator in an anonymous object initializer, it introduces a member with a transparent identifier.</span></span>
*  <span data-ttu-id="6aeee-2595">在上述的轉譯步驟中，透明識別碼一律會與匿名型別一起導入，並將多個範圍變數當做單一物件的成員來捕捉。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2595">In the translation steps described above, transparent identifiers are always introduced together with anonymous types, with the intent of capturing multiple range variables as members of a single object.</span></span> <span data-ttu-id="6aeee-2596">的執行C#允許使用與匿名型別不同的機制，將多個範圍變數群組在一起。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2596">An implementation of C# is permitted to use a different mechanism than anonymous types to group together multiple range variables.</span></span> <span data-ttu-id="6aeee-2597">下列轉譯範例假設使用匿名型別，並顯示如何將透明識別碼轉譯出來。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2597">The following translation examples assume that anonymous types are used, and show how transparent identifiers can be translated away.</span></span>

<span data-ttu-id="6aeee-2598">範例</span><span class="sxs-lookup"><span data-stu-id="6aeee-2598">The example</span></span>
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.Total }
```
<span data-ttu-id="6aeee-2599">會轉譯為</span><span class="sxs-lookup"><span data-stu-id="6aeee-2599">is translated into</span></span>
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.Total }
```

<span data-ttu-id="6aeee-2600">這會進一步轉譯成</span><span class="sxs-lookup"><span data-stu-id="6aeee-2600">which is further translated into</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(* => o.Total).
Select(* => new { c.Name, o.Total })
```
<span data-ttu-id="6aeee-2601">在清除透明識別碼時，相當於</span><span class="sxs-lookup"><span data-stu-id="6aeee-2601">which, when transparent identifiers are erased, is equivalent to</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.Total })
```
<span data-ttu-id="6aeee-2602">其中 `x` 是編譯器產生的識別碼，在其他情況下不可見且無法存取。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2602">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="6aeee-2603">範例</span><span class="sxs-lookup"><span data-stu-id="6aeee-2603">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
<span data-ttu-id="6aeee-2604">會轉譯為</span><span class="sxs-lookup"><span data-stu-id="6aeee-2604">is translated into</span></span>
```csharp
from * in customers.
    Join(orders, c => c.CustomerID, o => o.CustomerID, 
        (c, o) => new { c, o })
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
<span data-ttu-id="6aeee-2605">進一步縮減為</span><span class="sxs-lookup"><span data-stu-id="6aeee-2605">which is further reduced to</span></span>
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID, (c, o) => new { c, o }).
Join(details, * => o.OrderID, d => d.OrderID, (*, d) => new { *, d }).
Join(products, * => d.ProductID, p => p.ProductID, (*, p) => new { *, p }).
Select(* => new { c.Name, o.OrderDate, p.ProductName })
```
<span data-ttu-id="6aeee-2606">最終的轉譯是</span><span class="sxs-lookup"><span data-stu-id="6aeee-2606">the final translation of which is</span></span>
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c, o }).
Join(details, x => x.o.OrderID, d => d.OrderID,
    (x, d) => new { x, d }).
Join(products, y => y.d.ProductID, p => p.ProductID,
    (y, p) => new { y, p }).
Select(z => new { z.y.x.c.Name, z.y.x.o.OrderDate, z.p.ProductName })
```
<span data-ttu-id="6aeee-2607">其中 `x`，`y`，而 `z` 是編譯器產生的識別碼，在其他情況下不可見且無法存取。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2607">where `x`, `y`, and `z` are compiler generated identifiers that are otherwise invisible and inaccessible.</span></span>

### <a name="the-query-expression-pattern"></a><span data-ttu-id="6aeee-2608">查詢運算式模式</span><span class="sxs-lookup"><span data-stu-id="6aeee-2608">The query expression pattern</span></span>

<span data-ttu-id="6aeee-2609">***查詢運算式模式***會建立一種方法模式，讓型別可以實作為支援查詢運算式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2609">The ***Query expression pattern*** establishes a pattern of methods that types can implement to support query expressions.</span></span> <span data-ttu-id="6aeee-2610">由於查詢運算式會透過語法對應轉譯為方法調用，因此，型別在實作為查詢運算式模式的方式上具有相當大的彈性。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2610">Because query expressions are translated to method invocations by means of a syntactic mapping, types have considerable flexibility in how they implement the query expression pattern.</span></span> <span data-ttu-id="6aeee-2611">例如，模式的方法可以實作為實例方法或擴充方法，因為兩者具有相同的調用語法，而且方法可以要求委派或運算式樹狀架構，因為匿名函式可轉換成兩者。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2611">For example, the methods of the pattern can be implemented as instance methods or as extension methods because the two have the same invocation syntax, and the methods can request delegates or expression trees because anonymous functions are convertible to both.</span></span>

<span data-ttu-id="6aeee-2612">支援查詢運算式模式的泛型型別 `C<T>` 的建議圖形如下所示。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2612">The recommended shape of a generic type `C<T>` that supports the query expression pattern is shown below.</span></span> <span data-ttu-id="6aeee-2613">泛型型別是用來說明參數和結果類型之間的適當關聯性，但也可以為非泛型型別執行模式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2613">A generic type is used in order to illustrate the proper relationships between parameter and result types, but it is possible to implement the pattern for non-generic types as well.</span></span>

```csharp
delegate R Func<T1,R>(T1 arg1);

delegate R Func<T1,T2,R>(T1 arg1, T2 arg2);

class C
{
    public C<T> Cast<T>();
}

class C<T> : C
{
    public C<T> Where(Func<T,bool> predicate);

    public C<U> Select<U>(Func<T,U> selector);

    public C<V> SelectMany<U,V>(Func<T,C<U>> selector,
        Func<T,U,V> resultSelector);

    public C<V> Join<U,K,V>(C<U> inner, Func<T,K> outerKeySelector,
        Func<U,K> innerKeySelector, Func<T,U,V> resultSelector);

    public C<V> GroupJoin<U,K,V>(C<U> inner, Func<T,K> outerKeySelector,
        Func<U,K> innerKeySelector, Func<T,C<U>,V> resultSelector);

    public O<T> OrderBy<K>(Func<T,K> keySelector);

    public O<T> OrderByDescending<K>(Func<T,K> keySelector);

    public C<G<K,T>> GroupBy<K>(Func<T,K> keySelector);

    public C<G<K,E>> GroupBy<K,E>(Func<T,K> keySelector,
        Func<T,E> elementSelector);
}

class O<T> : C<T>
{
    public O<T> ThenBy<K>(Func<T,K> keySelector);

    public O<T> ThenByDescending<K>(Func<T,K> keySelector);
}

class G<K,T> : C<T>
{
    public K Key { get; }
}
```

<span data-ttu-id="6aeee-2614">上述方法會使用泛型委派類型 `Func<T1,R>` 並 `Func<T1,T2,R>`，但它們也可以在參數和結果類型中，使用具有相同關聯性的其他委派或運算式樹狀架構類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2614">The methods above use the generic delegate types `Func<T1,R>` and `Func<T1,T2,R>`, but they could equally well have used other delegate or expression tree types with the same relationships in parameter and result types.</span></span>

<span data-ttu-id="6aeee-2615">請注意 `C<T>` 和 `O<T>` 之間的建議關聯性，這可確保 @no__t 2 和 @no__t 3 方法僅適用于 `OrderBy` 或 `OrderByDescending` 的結果。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2615">Notice the recommended relationship between `C<T>` and `O<T>` which ensures that the `ThenBy` and `ThenByDescending` methods are available only on the result of an `OrderBy` or `OrderByDescending`.</span></span> <span data-ttu-id="6aeee-2616">另請注意，`GroupBy` 的結果（序列序列，其中每個內部序列都有額外的 `Key` 屬性）的建議形式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2616">Also notice the recommended shape of the result of `GroupBy` -- a sequence of sequences, where each inner sequence has an additional `Key` property.</span></span>

<span data-ttu-id="6aeee-2617">@No__t-0 命名空間會針對任何實作為 @no__t 1 介面的型別，提供查詢運算子模式的執行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2617">The `System.Linq` namespace provides an implementation of the query operator pattern for any type that implements the `System.Collections.Generic.IEnumerable<T>` interface.</span></span>

## <a name="assignment-operators"></a><span data-ttu-id="6aeee-2618">指派運算子</span><span class="sxs-lookup"><span data-stu-id="6aeee-2618">Assignment operators</span></span>

<span data-ttu-id="6aeee-2619">指派運算子會將新值指派給變數、屬性、事件或索引子元素。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2619">The assignment operators assign a new value to a variable, a property, an event, or an indexer element.</span></span>

```antlr
assignment
    : unary_expression assignment_operator expression
    ;

assignment_operator
    : '='
    | '+='
    | '-='
    | '*='
    | '/='
    | '%='
    | '&='
    | '|='
    | '^='
    | '<<='
    | right_shift_assignment
    ;
```

<span data-ttu-id="6aeee-2620">指派的左運算元必須是分類為變數、屬性存取、索引子存取或事件存取的運算式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2620">The left operand of an assignment must be an expression classified as a variable, a property access, an indexer access, or an event access.</span></span>

<span data-ttu-id="6aeee-2621">@No__t-0 運算子稱為「***簡單指派運算子***」。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2621">The `=` operator is called the ***simple assignment operator***.</span></span> <span data-ttu-id="6aeee-2622">它會將右運算元的值指派給左運算元所指定的變數、屬性或索引子元素。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2622">It assigns the value of the right operand to the variable, property, or indexer element given by the left operand.</span></span> <span data-ttu-id="6aeee-2623">簡單指派運算子的左運算元可能不是事件存取權（除非如[類似欄位的事件](classes.md#field-like-events)中所述）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2623">The left operand of the simple assignment operator may not be an event access (except as described in [Field-like events](classes.md#field-like-events)).</span></span> <span data-ttu-id="6aeee-2624">簡單指派運算子會在[簡單指派](expressions.md#simple-assignment)中說明。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2624">The simple assignment operator is described in [Simple assignment](expressions.md#simple-assignment).</span></span>

<span data-ttu-id="6aeee-2625">@No__t-0 運算子以外的指派運算子稱為***複合指派運算子***。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2625">The assignment operators other than the `=` operator are called the ***compound assignment operators***.</span></span> <span data-ttu-id="6aeee-2626">這些運算子會對兩個運算元執行指示的作業，然後將產生的值指派給左運算元所指定的變數、屬性或索引子元素。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2626">These operators perform the indicated operation on the two operands, and then assign the resulting value to the variable, property, or indexer element given by the left operand.</span></span> <span data-ttu-id="6aeee-2627">複合指派中會描述複合指派[運算子。](expressions.md#compound-assignment)</span><span class="sxs-lookup"><span data-stu-id="6aeee-2627">The compound assignment operators are described in [Compound assignment](expressions.md#compound-assignment).</span></span>

<span data-ttu-id="6aeee-2628">以事件存取運算式做為左運算元的 `+=` 和 @no__t 1 運算子，稱為「*事件指派運算子*」。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2628">The `+=` and `-=` operators with an event access expression as the left operand are called the *event assignment operators*.</span></span> <span data-ttu-id="6aeee-2629">沒有其他指派運算子有效，並以事件存取做為左運算元。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2629">No other assignment operator is valid with an event access as the left operand.</span></span> <span data-ttu-id="6aeee-2630">事件指派運算子會在[事件指派](expressions.md#event-assignment)中說明。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2630">The event assignment operators are described in [Event assignment](expressions.md#event-assignment).</span></span>

<span data-ttu-id="6aeee-2631">指派運算子是靠右關聯的，這表示作業會從右至左分組。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2631">The assignment operators are right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="6aeee-2632">例如，格式為 `a = b = c` 的運算式會評估為 `a = (b = c)`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2632">For example, an expression of the form `a = b = c` is evaluated as `a = (b = c)`.</span></span>

### <a name="simple-assignment"></a><span data-ttu-id="6aeee-2633">單一指派</span><span class="sxs-lookup"><span data-stu-id="6aeee-2633">Simple assignment</span></span>

<span data-ttu-id="6aeee-2634">@No__t-0 運算子稱為「簡單指派運算子」。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2634">The `=` operator is called the simple assignment operator.</span></span>

<span data-ttu-id="6aeee-2635">如果簡單指派的左運算元的格式為 `E.P`，或 `E[Ei]`，其中 `E` 的編譯時間類型 `dynamic`，則指派會動態繫結（[動態](expressions.md#dynamic-binding)系結）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2635">If the left operand of a simple assignment is of the form `E.P` or `E[Ei]` where `E` has the compile-time type `dynamic`, then the assignment is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="6aeee-2636">在此情況下，指派運算式的編譯時期類型為 `dynamic`，而以下所述的解決將會在執行時間根據 `E` 的執行時間類型進行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2636">In this case the compile-time type of the assignment expression is `dynamic`, and the resolution described below will take place at run-time based on the run-time type of `E`.</span></span>

<span data-ttu-id="6aeee-2637">在簡單的指派中，右運算元必須是可隱含轉換為左運算元類型的運算式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2637">In a simple assignment, the right operand must be an expression that is implicitly convertible to the type of the left operand.</span></span> <span data-ttu-id="6aeee-2638">作業會將右運算元的值指派給左運算元所指定的變數、屬性或索引子元素。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2638">The operation assigns the value of the right operand to the variable, property, or indexer element given by the left operand.</span></span>

<span data-ttu-id="6aeee-2639">簡單指派運算式的結果就是指派給左運算元的值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2639">The result of a simple assignment expression is the value assigned to the left operand.</span></span> <span data-ttu-id="6aeee-2640">結果的類型與左運算元相同，而且一律會分類為值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2640">The result has the same type as the left operand and is always classified as a value.</span></span>

<span data-ttu-id="6aeee-2641">如果左運算元是屬性或索引子存取，則屬性或索引子必須具有 @no__t 0 存取子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2641">If the left operand is a property or indexer access, the property or indexer must have a `set` accessor.</span></span> <span data-ttu-id="6aeee-2642">如果不是這種情況，就會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2642">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="6aeee-2643">@No__t-0 的簡單指派格式的執行時間處理包含下列步驟：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2643">The run-time processing of a simple assignment of the form `x = y` consists of the following steps:</span></span>

*  <span data-ttu-id="6aeee-2644">如果 `x` 分類為變數：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2644">If `x` is classified as a variable:</span></span>
   * <span data-ttu-id="6aeee-2645">`x` 會進行評估以產生變數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2645">`x` is evaluated to produce the variable.</span></span>
   * <span data-ttu-id="6aeee-2646">`y` 會進行評估，並在必要時，透過隱含轉換（[隱含](conversions.md#implicit-conversions)轉換）轉換成 `x` 的類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2646">`y` is evaluated and, if required, converted to the type of `x` through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
   * <span data-ttu-id="6aeee-2647">如果 `x` 所指定的變數是*reference_type*的陣列元素，則會執行執行時間檢查，以確保針對 `y` 計算的值與 `x` 是元素的陣列實例相容。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2647">If the variable given by `x` is an array element of a *reference_type*, a run-time check is performed to ensure that the value computed for `y` is compatible with the array instance of which `x` is an element.</span></span> <span data-ttu-id="6aeee-2648">如果 `y` 是 `null`，或如果隱含參考轉換（[隱含參考轉換](conversions.md#implicit-reference-conversions)）是從所 @no__t 參考之實例的實際類型（也就是包含 `x` 的陣列實例的實際元素類型）存在，則檢查會成功。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2648">The check succeeds if `y` is `null`, or if an implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from the actual type of the instance referenced by `y` to the actual element type of the array instance containing `x`.</span></span> <span data-ttu-id="6aeee-2649">否則會擲回 `System.ArrayTypeMismatchException`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2649">Otherwise, a `System.ArrayTypeMismatchException` is thrown.</span></span>
   * <span data-ttu-id="6aeee-2650">評估和轉換 `y` 所產生的值會儲存到評估 `x` 所提供的位置。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2650">The value resulting from the evaluation and conversion of `y` is stored into the location given by the evaluation of `x`.</span></span>
*  <span data-ttu-id="6aeee-2651">如果 `x` 已分類為屬性或索引子存取：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2651">If `x` is classified as a property or indexer access:</span></span>
   * <span data-ttu-id="6aeee-2652">實例運算式（如果 `x` 不是 `static`），而且會評估與 `x` 相關聯的引數清單（如果 `x` 是索引子存取），而結果會用於後續的 @no__t 4 存取子調用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2652">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `set` accessor invocation.</span></span>
   * <span data-ttu-id="6aeee-2653">`y` 會進行評估，並在必要時，透過隱含轉換（[隱含](conversions.md#implicit-conversions)轉換）轉換成 `x` 的類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2653">`y` is evaluated and, if required, converted to the type of `x` through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
   * <span data-ttu-id="6aeee-2654">會叫用 `x` 的 `set` 存取子，其值是以 `y` 為其 `value` 引數計算。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2654">The `set` accessor of `x` is invoked with the value computed for `y` as its `value` argument.</span></span>

<span data-ttu-id="6aeee-2655">陣列的共同變異數規則（[陣列共變數](arrays.md#array-covariance)）允許陣列類型的值 `A[]` 是陣列類型實例的參考 `B[]`，但前提是從 `B` 到 `A` 都有隱含的參考轉換。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2655">The array co-variance rules ([Array covariance](arrays.md#array-covariance)) permit a value of an array type `A[]` to be a reference to an instance of an array type `B[]`, provided an implicit reference conversion exists from `B` to `A`.</span></span> <span data-ttu-id="6aeee-2656">由於這些規則的緣故，指派給*reference_type*的 array 元素需要執行時間檢查，以確保所指派的值與陣列實例相容。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2656">Because of these rules, assignment to an array element of a *reference_type* requires a run-time check to ensure that the value being assigned is compatible with the array instance.</span></span> <span data-ttu-id="6aeee-2657">在範例中</span><span class="sxs-lookup"><span data-stu-id="6aeee-2657">In the example</span></span>
```csharp
string[] sa = new string[10];
object[] oa = sa;

oa[0] = null;               // Ok
oa[1] = "Hello";            // Ok
oa[2] = new ArrayList();    // ArrayTypeMismatchException
```
<span data-ttu-id="6aeee-2658">最後一次指派會導致擲回 `System.ArrayTypeMismatchException`，因為 `ArrayList` 的實例不能儲存在 `string[]` 的元素中。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2658">the last assignment causes a `System.ArrayTypeMismatchException` to be thrown because an instance of `ArrayList` cannot be stored in an element of a `string[]`.</span></span>

<span data-ttu-id="6aeee-2659">當*struct_type*中宣告的屬性或索引子是指派的目標時，與屬性或索引子存取相關聯的實例運算式必須分類為變數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2659">When a property or indexer declared in a *struct_type* is the target of an assignment, the instance expression associated with the property or indexer access must be classified as a variable.</span></span> <span data-ttu-id="6aeee-2660">如果實例運算式分類為值，則會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2660">If the instance expression is classified as a value, a binding-time error occurs.</span></span> <span data-ttu-id="6aeee-2661">因為有[成員存取權](expressions.md#member-access)，所以相同的規則也適用于欄位。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2661">Because of [Member access](expressions.md#member-access), the same rule also applies to fields.</span></span>

<span data-ttu-id="6aeee-2662">假設宣告如下：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2662">Given the declarations:</span></span>
```csharp
struct Point
{
    int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int X {
        get { return x; }
        set { x = value; }
    }

    public int Y {
        get { return y; }
        set { y = value; }
    }
}

struct Rectangle
{
    Point a, b;

    public Rectangle(Point a, Point b) {
        this.a = a;
        this.b = b;
    }

    public Point A {
        get { return a; }
        set { a = value; }
    }

    public Point B {
        get { return b; }
        set { b = value; }
    }
}
```
<span data-ttu-id="6aeee-2663">在範例中</span><span class="sxs-lookup"><span data-stu-id="6aeee-2663">in the example</span></span>
```csharp
Point p = new Point();
p.X = 100;
p.Y = 100;
Rectangle r = new Rectangle();
r.A = new Point(10, 10);
r.B = p;
```
<span data-ttu-id="6aeee-2664">允許 `p.X`、`p.Y`、`r.A` 和 `r.B` 的指派，因為 `p` 和 `r` 是變數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2664">the assignments to `p.X`, `p.Y`, `r.A`, and `r.B` are permitted because `p` and `r` are variables.</span></span> <span data-ttu-id="6aeee-2665">不過，在此範例中</span><span class="sxs-lookup"><span data-stu-id="6aeee-2665">However, in the example</span></span>
```csharp
Rectangle r = new Rectangle();
r.A.X = 10;
r.A.Y = 10;
r.B.X = 100;
r.B.Y = 100;
```
<span data-ttu-id="6aeee-2666">指派全都無效，因為 `r.A`，而 `r.B` 不是變數。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2666">the assignments are all invalid, since `r.A` and `r.B` are not variables.</span></span>

### <a name="compound-assignment"></a><span data-ttu-id="6aeee-2667">複合指派</span><span class="sxs-lookup"><span data-stu-id="6aeee-2667">Compound assignment</span></span>

<span data-ttu-id="6aeee-2668">如果複合指派的左運算元的格式為 `E.P`，或 `E[Ei]`，其中 `E` 的編譯時間類型 `dynamic`，則指派會動態繫結（[動態](expressions.md#dynamic-binding)系結）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2668">If the left operand of a compound assignment is of the form `E.P` or `E[Ei]` where `E` has the compile-time type `dynamic`, then the assignment is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="6aeee-2669">在此情況下，指派運算式的編譯時期類型為 `dynamic`，而以下所述的解決將會在執行時間根據 `E` 的執行時間類型進行。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2669">In this case the compile-time type of the assignment expression is `dynamic`, and the resolution described below will take place at run-time based on the run-time type of `E`.</span></span>

<span data-ttu-id="6aeee-2670">@No__t-0 格式的作業會藉由套用二元運算子多載解析（[二元運算子](expressions.md#binary-operator-overload-resolution)多載解析）來處理，如同作業是寫入 `x op y`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2670">An operation of the form `x op= y` is processed by applying binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) as if the operation was written `x op y`.</span></span> <span data-ttu-id="6aeee-2671">請</span><span class="sxs-lookup"><span data-stu-id="6aeee-2671">Then,</span></span>

*  <span data-ttu-id="6aeee-2672">如果選取之運算子的傳回型別可以隱含地轉換為 `x` 的型別，則作業會評估為 `x = x op y`，但只會評估 `x`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2672">If the return type of the selected operator is implicitly convertible to the type of `x`, the operation is evaluated as `x = x op y`, except that `x` is evaluated only once.</span></span>
*  <span data-ttu-id="6aeee-2673">否則，如果選取的運算子是預先定義的運算子，且選取之運算子的傳回型別明確轉換成 `x` 的型別，而且 `y` 可以隱含地轉換成 `x` 的型別，或運算子是移位運算子之後，會將作業評估為 `x = (T)(x op y)`，其中 `T` 是 `x` 的類型，但只會評估 `x` 一次。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2673">Otherwise, if the selected operator is a predefined operator, if the return type of the selected operator is explicitly convertible to the type of `x`, and if `y` is implicitly convertible to the type of `x` or the operator is a shift operator, then the operation is evaluated as `x = (T)(x op y)`, where `T` is the type of `x`, except that `x` is evaluated only once.</span></span>
*  <span data-ttu-id="6aeee-2674">否則，複合指派會無效，而且會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2674">Otherwise, the compound assignment is invalid, and a binding-time error occurs.</span></span>

<span data-ttu-id="6aeee-2675">「僅評估一次」一詞表示在評估 `x op y` 時，會暫時儲存 `x` 之任何組成運算式的結果，然後在執行指派至 `x` 時重複使用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2675">The term "evaluated only once" means that in the evaluation of `x op y`, the results of any constituent expressions of `x` are temporarily saved and then reused when performing the assignment to `x`.</span></span> <span data-ttu-id="6aeee-2676">例如，在指派中 `A()[B()] += C()`，其中 `A` 是傳回 `int[]` 的方法，而 `B` 和 `C` 是傳回 `int` 的方法，則只會叫用一次方法，順序為 `A`，`B`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2676">For example, in the assignment `A()[B()] += C()`, where `A` is a method returning `int[]`, and `B` and `C` are methods returning `int`, the methods are invoked only once, in the order `A`, `B`, `C`.</span></span>

<span data-ttu-id="6aeee-2677">當複合指派的左運算元是屬性存取或索引子存取時，屬性或索引子必須同時具有 @no__t 0 存取子和 @no__t 1 存取子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2677">When the left operand of a compound assignment is a property access or indexer access, the property or indexer must have both a `get` accessor and a `set` accessor.</span></span> <span data-ttu-id="6aeee-2678">如果不是這種情況，就會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2678">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="6aeee-2679">上述第二個規則允許在某些內容中，將 `x op= y` 評估為 `x = (T)(x op y)`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2679">The second rule above permits `x op= y` to be evaluated as `x = (T)(x op y)` in certain contexts.</span></span> <span data-ttu-id="6aeee-2680">當左運算元的類型為 `sbyte`、`byte`、`short`、`ushort` 或 `char` 時，此規則就會使用預先定義的運算子做為複合運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2680">The rule exists such that the predefined operators can be used as compound operators when the left operand is of type `sbyte`, `byte`, `short`, `ushort`, or `char`.</span></span> <span data-ttu-id="6aeee-2681">即使這兩個引數都屬於其中一種類型，預先定義的運算子還是會產生類型 `int` 的結果，如[二進位數值升級](expressions.md#binary-numeric-promotions)中所述。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2681">Even when both arguments are of one of those types, the predefined operators produce a result of type `int`, as described in [Binary numeric promotions](expressions.md#binary-numeric-promotions).</span></span> <span data-ttu-id="6aeee-2682">因此，在沒有轉換的情況下，不可能將結果指派給左運算元。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2682">Thus, without a cast it would not be possible to assign the result to the left operand.</span></span>

<span data-ttu-id="6aeee-2683">針對預先定義的運算子，規則的直覺效果是，如果同時允許同時 `x op y` 和 `x = y`，則允許 `x op= y`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2683">The intuitive effect of the rule for predefined operators is simply that `x op= y` is permitted if both of `x op y` and `x = y` are permitted.</span></span> <span data-ttu-id="6aeee-2684">在範例中</span><span class="sxs-lookup"><span data-stu-id="6aeee-2684">In the example</span></span>
```csharp
byte b = 0;
char ch = '\0';
int i = 0;

b += 1;             // Ok
b += 1000;          // Error, b = 1000 not permitted
b += i;             // Error, b = i not permitted
b += (byte)i;       // Ok

ch += 1;            // Error, ch = 1 not permitted
ch += (char)1;      // Ok
```
<span data-ttu-id="6aeee-2685">每個錯誤的直覺原因是對應的簡單指派也會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2685">the intuitive reason for each error is that a corresponding simple assignment would also have been an error.</span></span>

<span data-ttu-id="6aeee-2686">這也表示複合指派作業支援提升作業。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2686">This also means that compound assignment operations support lifted operations.</span></span> <span data-ttu-id="6aeee-2687">在範例中</span><span class="sxs-lookup"><span data-stu-id="6aeee-2687">In the example</span></span>
```csharp
int? i = 0;
i += 1;             // Ok
```
<span data-ttu-id="6aeee-2688">使用提升的運算子 `+(int?,int?)`。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2688">the lifted operator `+(int?,int?)` is used.</span></span>

### <a name="event-assignment"></a><span data-ttu-id="6aeee-2689">事件指派</span><span class="sxs-lookup"><span data-stu-id="6aeee-2689">Event assignment</span></span>

<span data-ttu-id="6aeee-2690">如果 `+=` 或 @no__t 1 運算子的左運算元分類為事件存取，則會評估運算式，如下所示：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2690">If the left operand of a `+=` or `-=` operator is classified as an event access, then the expression is evaluated as follows:</span></span>

*  <span data-ttu-id="6aeee-2691">會評估事件存取的實例運算式（如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2691">The instance expression, if any, of the event access is evaluated.</span></span>
*  <span data-ttu-id="6aeee-2692">系統會評估 `+=` 或 `-=` 運算子的右運算元，並在必要時，透過隱含轉換（[隱含](conversions.md#implicit-conversions)轉換）轉換為左運算元的類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2692">The right operand of the `+=` or `-=` operator is evaluated, and, if required, converted to the type of the left operand through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
*  <span data-ttu-id="6aeee-2693">系統會叫用事件的事件存取子，其中包含由右運算元組成的引數清單、在評估之後，以及必要的轉換。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2693">An event accessor of the event is invoked, with argument list consisting of the right operand, after evaluation and, if necessary, conversion.</span></span> <span data-ttu-id="6aeee-2694">如果運算子 `+=`，則會叫用 `add` 存取子;如果運算子為 `-=`，則會叫用 `remove` 存取子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2694">If the operator was `+=`, the `add` accessor is invoked; if the operator was `-=`, the `remove` accessor is invoked.</span></span>

<span data-ttu-id="6aeee-2695">事件指派運算式不會產生值。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2695">An event assignment expression does not yield a value.</span></span> <span data-ttu-id="6aeee-2696">因此，事件指派運算式只會在*statement_expression* （[運算式語句](statements.md#expression-statements)）的內容中有效。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2696">Thus, an event assignment expression is valid only in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>

## <a name="expression"></a><span data-ttu-id="6aeee-2697">運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-2697">Expression</span></span>

<span data-ttu-id="6aeee-2698">*運算式*可以是*non_assignment_expression*或*指派*。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2698">An *expression* is either a *non_assignment_expression* or an *assignment*.</span></span>

```antlr
expression
    : non_assignment_expression
    | assignment
    ;

non_assignment_expression
    : conditional_expression
    | lambda_expression
    | query_expression
    ;
```

## <a name="constant-expressions"></a><span data-ttu-id="6aeee-2699">常數運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-2699">Constant expressions</span></span>

<span data-ttu-id="6aeee-2700">*Constant_expression*是可在編譯時期完整評估的運算式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2700">A *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span>

```antlr
constant_expression
    : expression
    ;
```

<span data-ttu-id="6aeee-2701">常數運算式必須是 `null` 常值，或具有下列其中一種類型的值： `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、0、1、2、3、4、5 或任何列舉類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2701">A constant expression must be the `null` literal or a value with one of  the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `object`, `string`, or any enumeration type.</span></span> <span data-ttu-id="6aeee-2702">常數運算式中只允許下列結構：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2702">Only the following constructs are permitted in constant expressions:</span></span>

*  <span data-ttu-id="6aeee-2703">常值（包括 `null` 常值）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2703">Literals (including the `null` literal).</span></span>
*  <span data-ttu-id="6aeee-2704">對類別和結構類型的 `const` 成員的參考。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2704">References to `const` members of class and struct types.</span></span>
*  <span data-ttu-id="6aeee-2705">列舉類型之成員的參考。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2705">References to members of enumeration types.</span></span>
*  <span data-ttu-id="6aeee-2706">@No__t 0 個參數或區域變數的參考</span><span class="sxs-lookup"><span data-stu-id="6aeee-2706">References to `const` parameters or local variables</span></span>
*  <span data-ttu-id="6aeee-2707">以括弧括住的子運算式，本身就是常數運算式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2707">Parenthesized sub-expressions, which are themselves constant expressions.</span></span>
*  <span data-ttu-id="6aeee-2708">Cast 運算式，前提是目標型別是上列其中一種類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2708">Cast expressions, provided the target type is one of the types listed above.</span></span>
*  <span data-ttu-id="6aeee-2709">`checked` 和 @no__t 1 運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-2709">`checked` and `unchecked` expressions</span></span>
*  <span data-ttu-id="6aeee-2710">預設值運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-2710">Default value expressions</span></span>
*  <span data-ttu-id="6aeee-2711">Nameof 運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-2711">Nameof expressions</span></span>
*  <span data-ttu-id="6aeee-2712">預先定義的 `+`、`-`、@no__t 2 和 @no__t 3 一元運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2712">The predefined `+`, `-`, `!`, and `~` unary operators.</span></span>
*  <span data-ttu-id="6aeee-2713">預先定義的 `+`，`-`、`*`、`/`、`%`、`<<`、`>>`、`&`、`|`、`^`、0、1、2、3、4、5、6 和 7 二元運算子，前提是每個運算元都是上列類型。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2713">The predefined `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, `&&`, `||`, `==`, `!=`, `<`, `>`, `<=`, and `>=` binary operators, provided each operand is of a type listed above.</span></span>
*  <span data-ttu-id="6aeee-2714">@No__t-0 條件運算子。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2714">The `?:` conditional operator.</span></span>

<span data-ttu-id="6aeee-2715">常數運算式中允許下列轉換：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2715">The following conversions are permitted in constant expressions:</span></span>

*  <span data-ttu-id="6aeee-2716">身分識別轉換</span><span class="sxs-lookup"><span data-stu-id="6aeee-2716">Identity conversions</span></span>
*  <span data-ttu-id="6aeee-2717">數值轉換</span><span class="sxs-lookup"><span data-stu-id="6aeee-2717">Numeric conversions</span></span>
*  <span data-ttu-id="6aeee-2718">列舉轉換</span><span class="sxs-lookup"><span data-stu-id="6aeee-2718">Enumeration conversions</span></span>
*  <span data-ttu-id="6aeee-2719">常數運算式轉換</span><span class="sxs-lookup"><span data-stu-id="6aeee-2719">Constant expression conversions</span></span>
*  <span data-ttu-id="6aeee-2720">隱含和明確的參考轉換，前提是轉換的來源是會評估為 null 值的常數運算式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2720">Implicit and explicit reference conversions, provided that the source of the conversions is a constant expression that evaluates to the null value.</span></span>

<span data-ttu-id="6aeee-2721">常數運算式中不允許其他轉換，包括非 null 值的裝箱、取消裝箱和隱含參考轉換。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2721">Other conversions including boxing, unboxing and implicit reference conversions of non-null values are not permitted in constant expressions.</span></span> <span data-ttu-id="6aeee-2722">例如:</span><span class="sxs-lookup"><span data-stu-id="6aeee-2722">For example:</span></span>
```csharp
class C {
    const object i = 5;         // error: boxing conversion not permitted
    const object str = "hello"; // error: implicit reference conversion
}
```
<span data-ttu-id="6aeee-2723">i 的初始化是錯誤的，因為需要進行裝箱轉換。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2723">the initialization of i is an error because a boxing conversion is required.</span></span> <span data-ttu-id="6aeee-2724">Str 的初始化是錯誤，因為需要非 null 值的隱含參考轉換。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2724">The initialization of str is an error because an implicit reference conversion from a non-null value is required.</span></span>

<span data-ttu-id="6aeee-2725">每當運算式滿足上述需求時，就會在編譯時期評估運算式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2725">Whenever an expression fulfills the requirements listed above, the expression is evaluated at compile-time.</span></span> <span data-ttu-id="6aeee-2726">即使運算式是包含非常數結構之較大運算式的子運算式，也是如此。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2726">This is true even if the expression is a sub-expression of a larger expression that contains non-constant constructs.</span></span>

<span data-ttu-id="6aeee-2727">常數運算式的編譯時間評估會使用與非常數運算式的執行時間評估相同的規則，但在執行時間評估會擲回例外狀況的情況下，編譯時間評估會導致發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2727">The compile-time evaluation of constant expressions uses the same rules as run-time evaluation of non-constant expressions, except that where run-time evaluation would have thrown an exception, compile-time evaluation causes a compile-time error to occur.</span></span>

<span data-ttu-id="6aeee-2728">除非常數運算式明確地放在 @no__t 0 內容中，否則在運算式的編譯時間評估期間，整數類型算術作業和轉換期間發生的溢位，一律會導致編譯時期錯誤（[常數運算式](expressions.md#constant-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2728">Unless a constant expression is explicitly placed in an `unchecked` context, overflows that occur in integral-type arithmetic operations and conversions during the compile-time evaluation of the expression always cause compile-time errors ([Constant expressions](expressions.md#constant-expressions)).</span></span>

<span data-ttu-id="6aeee-2729">常數運算式會出現在下面所列的內容中。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2729">Constant expressions occur in the contexts listed below.</span></span> <span data-ttu-id="6aeee-2730">在這些內容中，如果無法在編譯時期完整評估運算式，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2730">In these contexts, a compile-time error occurs if an expression cannot be fully evaluated at compile-time.</span></span>

*  <span data-ttu-id="6aeee-2731">常數宣告（[常數](classes.md#constants)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2731">Constant declarations ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="6aeee-2732">列舉成員宣告（[列舉成員](enums.md#enum-members)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2732">Enumeration member declarations ([Enum members](enums.md#enum-members)).</span></span>
*  <span data-ttu-id="6aeee-2733">正式參數清單的預設引數（[方法參數](classes.md#method-parameters)）</span><span class="sxs-lookup"><span data-stu-id="6aeee-2733">Default arguments of formal parameter lists ([Method parameters](classes.md#method-parameters))</span></span>
*  <span data-ttu-id="6aeee-2734">`switch` 語句的 `case` 標籤（[switch 語句](statements.md#the-switch-statement)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2734">`case` labels of a `switch` statement ([The switch statement](statements.md#the-switch-statement)).</span></span>
*  <span data-ttu-id="6aeee-2735">`goto case` 語句（[goto 語句](statements.md#the-goto-statement)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2735">`goto case` statements ([The goto statement](statements.md#the-goto-statement)).</span></span>
*  <span data-ttu-id="6aeee-2736">陣列建立運算式中的維度長度（[陣列建立運算式](expressions.md#array-creation-expressions)），其中包含初始化運算式。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2736">Dimension lengths in an array creation expression ([Array creation expressions](expressions.md#array-creation-expressions)) that includes an initializer.</span></span>
*  <span data-ttu-id="6aeee-2737">屬性（[屬性](attributes.md)）。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2737">Attributes ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="6aeee-2738">隱含的常數運算式轉換（[隱含常數運算式轉換](conversions.md#implicit-constant-expression-conversions)）允許將類型 `int` 的常數運算式轉換成 `sbyte`、`byte`、`short`、`ushort`、`uint` 或 `ulong`，前提是提供的值為常數運算式位於目的地類型的範圍內。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2738">An implicit constant expression conversion ([Implicit constant expression conversions](conversions.md#implicit-constant-expression-conversions)) permits a constant expression of type `int` to be converted to `sbyte`, `byte`, `short`, `ushort`, `uint`, or `ulong`, provided the value of the constant expression is within the range of the destination type.</span></span>

## <a name="boolean-expressions"></a><span data-ttu-id="6aeee-2739">布林運算式</span><span class="sxs-lookup"><span data-stu-id="6aeee-2739">Boolean expressions</span></span>

<span data-ttu-id="6aeee-2740">*Boolean_expression*是產生類型 `bool` 之結果的運算式。在特定內容中，直接或透過 `operator true` 的應用程式，如下所指定。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2740">A *boolean_expression* is an expression that yields a result of type `bool`; either directly or through application of `operator true` in certain contexts as specified in the following.</span></span>

```antlr
boolean_expression
    : expression
    ;
```

<span data-ttu-id="6aeee-2741">*If_statement* （[if 語句](statements.md#the-if-statement)）、 *while_statement* （[while 語句](statements.md#the-while-statement)）、 *do_statement* （[do 語句](statements.md#the-do-statement)）或*for_statement* （適用于的控制條件運算式）[語句](statements.md#the-for-statement)）是*boolean_expression*。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2741">The controlling conditional expression of an *if_statement* ([The if statement](statements.md#the-if-statement)), *while_statement* ([The while statement](statements.md#the-while-statement)), *do_statement* ([The do statement](statements.md#the-do-statement)), or *for_statement* ([The for statement](statements.md#the-for-statement)) is a *boolean_expression*.</span></span> <span data-ttu-id="6aeee-2742">@No__t-0 運算子（[條件運算子](expressions.md#conditional-operator)）的控制條件運算式會遵循與*boolean_expression*相同的規則，但會將運算子優先順序的原因分類為*conditional_or_expression*。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2742">The controlling conditional expression of the `?:` operator ([Conditional operator](expressions.md#conditional-operator)) follows the same rules as a *boolean_expression*, but for reasons of operator precedence is classified as a *conditional_or_expression*.</span></span>

<span data-ttu-id="6aeee-2743">需要*boolean_expression* `E`，才能產生類型 `bool` 的值，如下所示：</span><span class="sxs-lookup"><span data-stu-id="6aeee-2743">A *boolean_expression* `E` is required to be able to produce a value of type `bool`, as follows:</span></span>

*  <span data-ttu-id="6aeee-2744">如果 `E` 在執行時間會隱含地轉換成 `bool`，則會在應用程式套用隱含轉換。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2744">If `E` is implicitly convertible to `bool` then at runtime that implicit conversion is applied.</span></span>
*  <span data-ttu-id="6aeee-2745">否則，一元運算子多載解析（[一元運算子](expressions.md#unary-operator-overload-resolution)多載解析）是用來在 `E` 上尋找運算子 `true` 的唯一最佳執行，而該實作為在執行時間套用。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2745">Otherwise, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is used to find a unique best implementation of operator `true` on `E`, and that implementation is applied at runtime.</span></span>
*  <span data-ttu-id="6aeee-2746">如果找不到這類運算子，則會發生系結時錯誤。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2746">If no such operator is found, a binding-time error occurs.</span></span>

<span data-ttu-id="6aeee-2747">[資料庫布林值類型](structs.md#database-boolean-type)中的 `DBBool` 結構類型提供了實 `operator true` 和 `operator false` 的類型範例。</span><span class="sxs-lookup"><span data-stu-id="6aeee-2747">The `DBBool` struct type in [Database boolean type](structs.md#database-boolean-type) provides an example of a type that implements `operator true` and `operator false`.</span></span>
