---
ms.openlocfilehash: 066c300d4c2baa8749e132730ecd48275e2957f7
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "64489005"
---
# <a name="expressions"></a><span data-ttu-id="abcc0-101">運算式</span><span class="sxs-lookup"><span data-stu-id="abcc0-101">Expressions</span></span>

<span data-ttu-id="abcc0-102">運算式是運算子和運算元的序列。</span><span class="sxs-lookup"><span data-stu-id="abcc0-102">An expression is a sequence of operators and operands.</span></span> <span data-ttu-id="abcc0-103">這一章中定義的語法，順序評估運算元和運算子和運算式的意義。</span><span class="sxs-lookup"><span data-stu-id="abcc0-103">This chapter defines the syntax, order of evaluation of operands and operators, and meaning of expressions.</span></span>

## <a name="expression-classifications"></a><span data-ttu-id="abcc0-104">運算式分類</span><span class="sxs-lookup"><span data-stu-id="abcc0-104">Expression classifications</span></span>

<span data-ttu-id="abcc0-105">運算式分類為下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="abcc0-105">An expression is classified as one of the following:</span></span>

*  <span data-ttu-id="abcc0-106">一個值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-106">A value.</span></span> <span data-ttu-id="abcc0-107">每個值都有關聯型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-107">Every value has an associated type.</span></span>
*  <span data-ttu-id="abcc0-108">變數中。</span><span class="sxs-lookup"><span data-stu-id="abcc0-108">A variable.</span></span> <span data-ttu-id="abcc0-109">每個變數都有關聯的型別，也就是變數的宣告型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-109">Every variable has an associated type, namely the declared type of the variable.</span></span>
*  <span data-ttu-id="abcc0-110">命名空間。</span><span class="sxs-lookup"><span data-stu-id="abcc0-110">A namespace.</span></span> <span data-ttu-id="abcc0-111">此分類的運算式只能出現的左手邊*member_access* ([成員存取](expressions.md#member-access))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-111">An expression with this classification can only appear as the left hand side of a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="abcc0-112">在任何其他內容中，運算式分類為命名空間會造成編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-112">In any other context, an expression classified as a namespace causes a compile-time error.</span></span>
*  <span data-ttu-id="abcc0-113">類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-113">A type.</span></span> <span data-ttu-id="abcc0-114">此分類的運算式只能出現的左手邊*member_access* ([成員存取](expressions.md#member-access))，或做為運算元`as`運算子 ([As 運算子](expressions.md#the-as-operator))，則`is`運算子 ([是運算子](expressions.md#the-is-operator))，或有`typeof`運算子 ([typeof 運算子](expressions.md#the-typeof-operator))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-114">An expression with this classification can only appear as the left hand side of a *member_access* ([Member access](expressions.md#member-access)), or as an operand for the `as` operator ([The as operator](expressions.md#the-as-operator)), the `is` operator ([The is operator](expressions.md#the-is-operator)), or the `typeof` operator ([The typeof operator](expressions.md#the-typeof-operator)).</span></span> <span data-ttu-id="abcc0-115">在任何其他內容中，歸類為類型的運算式會導致編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-115">In any other context, an expression classified as a type causes a compile-time error.</span></span>
*  <span data-ttu-id="abcc0-116">方法群組，也就是一組多載的方法所產生的成員查閱 ([成員查閱](expressions.md#member-lookup))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-116">A method group, which is a set of overloaded methods resulting from a member lookup ([Member lookup](expressions.md#member-lookup)).</span></span> <span data-ttu-id="abcc0-117">方法群組可能會有一個相關聯的執行個體運算式和相關聯的類型引數清單。</span><span class="sxs-lookup"><span data-stu-id="abcc0-117">A method group may have an associated instance expression and an associated type argument list.</span></span> <span data-ttu-id="abcc0-118">叫用執行個體方法時，該執行個體運算式的評估結果會成為所代表的執行個體`this`([這項存取](expressions.md#this-access))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-118">When an instance method is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="abcc0-119">方法會允許群組中*invocation_expression* ([引動過程運算式](expressions.md#invocation-expressions))，則*delegate_creation_expression* ([委派建立運算式](expressions.md#delegate-creation-expressions)) 以及左手邊的運算子，且可以隱含地轉換成相容的委派類型 ([方法群組轉換](conversions.md#method-group-conversions))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-119">A method group is permitted in an *invocation_expression* ([Invocation expressions](expressions.md#invocation-expressions)) , a *delegate_creation_expression* ([Delegate creation expressions](expressions.md#delegate-creation-expressions)) and as the left hand side of an is operator, and can be implicitly converted to a compatible delegate type ([Method group conversions](conversions.md#method-group-conversions)).</span></span> <span data-ttu-id="abcc0-120">在任何其他內容中，分類為方法群組運算式會導致編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-120">In any other context, an expression classified as a method group causes a compile-time error.</span></span>
*  <span data-ttu-id="abcc0-121">Null 常值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-121">A null literal.</span></span> <span data-ttu-id="abcc0-122">此分類的運算式可以隱含地轉換為參考型別或可為 null 的型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-122">An expression with this classification can be implicitly converted to a reference type or nullable type.</span></span>
*  <span data-ttu-id="abcc0-123">匿名函式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-123">An anonymous function.</span></span> <span data-ttu-id="abcc0-124">此分類的運算式可以隱含地轉換為相容的委派型別或運算式樹狀架構型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-124">An expression with this classification can be implicitly converted to a compatible delegate type or expression tree type.</span></span>
*  <span data-ttu-id="abcc0-125">屬性存取。</span><span class="sxs-lookup"><span data-stu-id="abcc0-125">A property access.</span></span> <span data-ttu-id="abcc0-126">每個屬性存取具有相關聯的類型，也就是屬性的型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-126">Every property access has an associated type, namely the type of the property.</span></span> <span data-ttu-id="abcc0-127">此外，屬性存取可能會有相關聯的執行個體上的運算式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-127">Furthermore, a property access may have an associated instance expression.</span></span> <span data-ttu-id="abcc0-128">當存取子 (`get`或`set`區塊) 的執行個體叫用屬性存取、 執行個體運算式的評估結果會成為所代表的執行個體`this`([此存取權](expressions.md#this-access))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-128">When an accessor (the `get` or `set` block) of an instance property access is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)).</span></span>
*  <span data-ttu-id="abcc0-129">事件的存取。</span><span class="sxs-lookup"><span data-stu-id="abcc0-129">An event access.</span></span> <span data-ttu-id="abcc0-130">每個事件存取具有相關聯的類型，也就是事件的型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-130">Every event access has an associated type, namely the type of the event.</span></span> <span data-ttu-id="abcc0-131">此外，事件存取可能會有一個相關聯的執行個體運算式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-131">Furthermore, an event access may have an associated instance expression.</span></span> <span data-ttu-id="abcc0-132">事件存取可能會顯示為左運算元`+=`並`-=`運算子 ([事件指派](expressions.md#event-assignment))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-132">An event access may appear as the left hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="abcc0-133">在任何其他內容中，歸類為事件存取的運算式會導致編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-133">In any other context, an expression classified as an event access causes a compile-time error.</span></span>
*  <span data-ttu-id="abcc0-134">索引子存取。</span><span class="sxs-lookup"><span data-stu-id="abcc0-134">An indexer access.</span></span> <span data-ttu-id="abcc0-135">每個索引子存取具有相關聯的類型，也就是索引子的元素型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-135">Every indexer access has an associated type, namely the element type of the indexer.</span></span> <span data-ttu-id="abcc0-136">此外，索引子存取有相關聯的執行個體運算式和相關聯的引數清單。</span><span class="sxs-lookup"><span data-stu-id="abcc0-136">Furthermore, an indexer access has an associated instance expression and an associated argument list.</span></span> <span data-ttu-id="abcc0-137">當存取子 (`get`或`set`區塊) 的索引子叫用的存取，該執行個體運算式的評估結果會成為所代表的執行個體`this`([此存取權](expressions.md#this-access))，和結果評估引數清單，就會變成引動過程的參數清單。</span><span class="sxs-lookup"><span data-stu-id="abcc0-137">When an accessor (the `get` or `set` block) of an indexer access is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)), and the result of evaluating the argument list becomes the parameter list of the invocation.</span></span>
*  <span data-ttu-id="abcc0-138">沒有項目。</span><span class="sxs-lookup"><span data-stu-id="abcc0-138">Nothing.</span></span> <span data-ttu-id="abcc0-139">此運算式就會發生叫用方法的傳回類型為`void`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-139">This occurs when the expression is an invocation of a method with a return type of `void`.</span></span> <span data-ttu-id="abcc0-140">將分類為不只是有效的內容中的運算式*statement_expression* ([運算式陳述式](statements.md#expression-statements))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-140">An expression classified as nothing is only valid in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>

<span data-ttu-id="abcc0-141">運算式的最終結果不命名空間、 類型、 方法群組或事件存取。</span><span class="sxs-lookup"><span data-stu-id="abcc0-141">The final result of an expression is never a namespace, type, method group, or event access.</span></span> <span data-ttu-id="abcc0-142">相反地，如先前所述，這些類別的運算式都是只允許在特定內容的中繼建構。</span><span class="sxs-lookup"><span data-stu-id="abcc0-142">Rather, as noted above, these categories of expressions are intermediate constructs that are only permitted in certain contexts.</span></span>

<span data-ttu-id="abcc0-143">屬性存取或索引子存取一律分類為值所執行的引動過程*get 存取子*或*set 存取子*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-143">A property access or indexer access is always reclassified as a value by performing an invocation of the *get accessor* or the *set accessor*.</span></span> <span data-ttu-id="abcc0-144">特定的存取子取決於屬性或索引子存取的內容：如果存取指派，目標*set 存取子*會叫用來指派新值 ([簡單指派](expressions.md#simple-assignment))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-144">The particular accessor is determined by the context of the property or indexer access: If the access is the target of an assignment, the *set accessor* is invoked to assign a new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="abcc0-145">否則，請*get 存取子*會叫用來取得目前的值 ([運算式的值](expressions.md#values-of-expressions))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-145">Otherwise, the *get accessor* is invoked to obtain the current value ([Values of expressions](expressions.md#values-of-expressions)).</span></span>

### <a name="values-of-expressions"></a><span data-ttu-id="abcc0-146">運算式的值</span><span class="sxs-lookup"><span data-stu-id="abcc0-146">Values of expressions</span></span>

<span data-ttu-id="abcc0-147">大部分的包含運算式的結構，最後都會要求運算式表示***值***。</span><span class="sxs-lookup"><span data-stu-id="abcc0-147">Most of the constructs that involve an expression ultimately require the expression to denote a ***value***.</span></span> <span data-ttu-id="abcc0-148">在此情況下，如果實際的運算式表示命名空間、 類型、 方法群組，或執行任何動作，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-148">In such cases, if the actual expression denotes a namespace, a type, a method group, or nothing, a compile-time error occurs.</span></span> <span data-ttu-id="abcc0-149">不過，如果運算式表示屬性存取、 索引子存取或變數，就會隱含取代屬性、 索引子或變數的值：</span><span class="sxs-lookup"><span data-stu-id="abcc0-149">However, if the expression denotes a property access, an indexer access, or a variable, the value of the property, indexer, or variable is implicitly substituted:</span></span>

*  <span data-ttu-id="abcc0-150">變數的值是只將目前儲存變數所識別之儲存體位置中的值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-150">The value of a variable is simply the value currently stored in the storage location identified by the variable.</span></span> <span data-ttu-id="abcc0-151">變數必須被視為已明確指派 ([明確指派](variables.md#definite-assignment)) 才可以取得其值，或否則會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-151">A variable must be considered definitely assigned ([Definite assignment](variables.md#definite-assignment)) before its value can be obtained, or otherwise a compile-time error occurs.</span></span>
*  <span data-ttu-id="abcc0-152">屬性存取運算式的值藉由叫用*get 存取子*的屬性。</span><span class="sxs-lookup"><span data-stu-id="abcc0-152">The value of a property access expression is obtained by invoking the *get accessor* of the property.</span></span> <span data-ttu-id="abcc0-153">如果屬性不含任何*get 存取子*，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-153">If the property has no *get accessor*, a compile-time error occurs.</span></span> <span data-ttu-id="abcc0-154">否則，函式成員引動過程 ([編譯時期檢查動態的多載解析](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) 執行時，和引動過程的結果會成為屬性存取運算式的值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-154">Otherwise, a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is performed, and the result of the invocation becomes the value of the property access expression.</span></span>
*  <span data-ttu-id="abcc0-155">索引子存取運算式的值藉由叫用*get 存取子*的索引子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-155">The value of an indexer access expression is obtained by invoking the *get accessor* of the indexer.</span></span> <span data-ttu-id="abcc0-156">如果索引子不含任何*get 存取子*，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-156">If the indexer has no *get accessor*, a compile-time error occurs.</span></span> <span data-ttu-id="abcc0-157">否則函式成員引動過程 ([編譯時期檢查動態的多載解析](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) 會使用引數來執行清單與運算式相關聯索引子的存取，並叫用的結果會成為值索引子存取運算式中。</span><span class="sxs-lookup"><span data-stu-id="abcc0-157">Otherwise, a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is performed with the argument list associated with the indexer access expression, and the result of the invocation becomes the value of the indexer access expression.</span></span>

## <a name="static-and-dynamic-binding"></a><span data-ttu-id="abcc0-158">靜態和動態繫結</span><span class="sxs-lookup"><span data-stu-id="abcc0-158">Static and Dynamic Binding</span></span>

<span data-ttu-id="abcc0-159">判斷作業的類型或值的引數、 運算元 （接收者） 組成的運算式為基礎的意義的程序通常稱為***繫結***。</span><span class="sxs-lookup"><span data-stu-id="abcc0-159">The process of determining the meaning of an operation based on the type or value of constituent expressions (arguments, operands, receivers) is often referred to as ***binding***.</span></span> <span data-ttu-id="abcc0-160">執行個體方法呼叫的意義取決於接收者和引數的型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-160">For instance the meaning of a method call is determined based on the type of the receiver and arguments.</span></span> <span data-ttu-id="abcc0-161">運算子的意義取決於其運算元的類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-161">The meaning of an operator is determined based on the type of its operands.</span></span>

<span data-ttu-id="abcc0-162">在 C# 中作業的意義通常用來決定在編譯時期，根據其構成的運算式的編譯時間類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-162">In C# the meaning of an operation is usually determined at compile-time, based on the compile-time type of its constituent expressions.</span></span> <span data-ttu-id="abcc0-163">同樣地，如果運算式包含錯誤，錯誤會偵測到並由編譯器所回報。</span><span class="sxs-lookup"><span data-stu-id="abcc0-163">Likewise, if an expression contains an error, the error is detected and reported by the compiler.</span></span> <span data-ttu-id="abcc0-164">這種方法就所謂***靜態繫結***。</span><span class="sxs-lookup"><span data-stu-id="abcc0-164">This approach is known as ***static binding***.</span></span>

<span data-ttu-id="abcc0-165">不過，如果運算式是動態運算式 (也就是具有類型`dynamic`) 這表示它所參與的任何繫結應該根據其執行階段類型 （也就是在執行階段所代表的是物件的實際類型），而不是在型別編譯時間。</span><span class="sxs-lookup"><span data-stu-id="abcc0-165">However, if an expression is a dynamic expression (i.e. has the type `dynamic`) this indicates that any binding that it participates in should be based on its run-time type (i.e. the actual type of the object it denotes at run-time) rather than the type it has at compile-time.</span></span> <span data-ttu-id="abcc0-166">這類作業的繫結會因此延遲，直到程式執行期間要執行此作業的時間。</span><span class="sxs-lookup"><span data-stu-id="abcc0-166">The binding of such an operation is therefore deferred until the time where the operation is to be executed during the running of the program.</span></span> <span data-ttu-id="abcc0-167">這指***動態繫結***。</span><span class="sxs-lookup"><span data-stu-id="abcc0-167">This is referred to as ***dynamic binding***.</span></span>

<span data-ttu-id="abcc0-168">動態繫結作業，幾乎沒有任何檢查是由編譯器執行。</span><span class="sxs-lookup"><span data-stu-id="abcc0-168">When an operation is dynamically bound, little or no checking is performed by the compiler.</span></span> <span data-ttu-id="abcc0-169">改為執行階段繫結時，為在執行階段的例外狀況會回報錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-169">Instead if the run-time binding fails, errors are reported as exceptions at run-time.</span></span>

<span data-ttu-id="abcc0-170">繫結受限於 C# 中的下列作業：</span><span class="sxs-lookup"><span data-stu-id="abcc0-170">The following operations in C# are subject to binding:</span></span>

*  <span data-ttu-id="abcc0-171">成員存取： `e.M`</span><span class="sxs-lookup"><span data-stu-id="abcc0-171">Member access: `e.M`</span></span>
*  <span data-ttu-id="abcc0-172">方法引動過程： `e.M(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="abcc0-172">Method invocation: `e.M(e1, ..., eN)`</span></span>
*  <span data-ttu-id="abcc0-173">委派引動過程：`e(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="abcc0-173">Delegate invocation:`e(e1, ..., eN)`</span></span>
*  <span data-ttu-id="abcc0-174">項目存取權： `e[e1, ..., eN]`</span><span class="sxs-lookup"><span data-stu-id="abcc0-174">Element access: `e[e1, ..., eN]`</span></span>
*  <span data-ttu-id="abcc0-175">建立物件： `new C(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="abcc0-175">Object creation: `new C(e1, ..., eN)`</span></span>
*  <span data-ttu-id="abcc0-176">多載一元運算子： `+`， `-`， `!`， `~`， `++`， `--`， `true`， `false`</span><span class="sxs-lookup"><span data-stu-id="abcc0-176">Overloaded unary operators: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`</span></span>
*  <span data-ttu-id="abcc0-177">多載二元運算子： `+`， `-`， `*`， `/`， `%`， `&`， `&&`， `|`， `||`， `??`， `^`， `<<`, `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`</span><span class="sxs-lookup"><span data-stu-id="abcc0-177">Overloaded binary operators: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<`, `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`</span></span>
*  <span data-ttu-id="abcc0-178">指派運算子： `=`， `+=`， `-=`， `*=`， `/=`， `%=`， `&=`， `|=`， `^=`， `<<=`， `>>=`</span><span class="sxs-lookup"><span data-stu-id="abcc0-178">Assignment operators: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`</span></span>
*  <span data-ttu-id="abcc0-179">隱含和明確轉換</span><span class="sxs-lookup"><span data-stu-id="abcc0-179">Implicit and explicit conversions</span></span>

<span data-ttu-id="abcc0-180">當涉及動態運算式時，C# 預設為靜態繫結，這表示構成的運算式的編譯時期型別會在選取程序。</span><span class="sxs-lookup"><span data-stu-id="abcc0-180">When no dynamic expressions are involved, C# defaults to static binding, which means that the compile-time types of constituent expressions are used in the selection process.</span></span> <span data-ttu-id="abcc0-181">不過，構成的運算式在上面所列的作業中的其中一個是動態運算式時，作業會改為動態繫結。</span><span class="sxs-lookup"><span data-stu-id="abcc0-181">However, when one of the constituent expressions in the operations listed above is a dynamic expression, the operation is instead dynamically bound.</span></span>

### <a name="binding-time"></a><span data-ttu-id="abcc0-182">繫結時間</span><span class="sxs-lookup"><span data-stu-id="abcc0-182">Binding-time</span></span>

<span data-ttu-id="abcc0-183">靜態繫結會放置在編譯時期，而動態繫結會在執行階段的位置。</span><span class="sxs-lookup"><span data-stu-id="abcc0-183">Static binding takes place at compile-time, whereas dynamic binding takes place at run-time.</span></span> <span data-ttu-id="abcc0-184">在下列章節中，詞彙***繫結時間***指的是編譯時期或執行階段，根據繫結何時會發生。</span><span class="sxs-lookup"><span data-stu-id="abcc0-184">In the following sections, the term ***binding-time*** refers to either compile-time or run-time, depending on when the binding takes place.</span></span>

<span data-ttu-id="abcc0-185">下列範例說明靜態和動態繫結和繫結時間的概念：</span><span class="sxs-lookup"><span data-stu-id="abcc0-185">The following example illustrates the notions of static and dynamic binding and of binding-time:</span></span>
```csharp
object  o = 5;
dynamic d = 5;

Console.WriteLine(5);  // static  binding to Console.WriteLine(int)
Console.WriteLine(o);  // static  binding to Console.WriteLine(object)
Console.WriteLine(d);  // dynamic binding to Console.WriteLine(int)
```

<span data-ttu-id="abcc0-186">前兩個呼叫會以靜態方式繫結： 多載`Console.WriteLine`會挑出根據其引數的編譯時間類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-186">The first two calls are statically bound: the overload of `Console.WriteLine` is picked based on the compile-time type of their argument.</span></span> <span data-ttu-id="abcc0-187">因此，繫結時間是編譯時期。</span><span class="sxs-lookup"><span data-stu-id="abcc0-187">Thus, the binding-time is compile-time.</span></span>

<span data-ttu-id="abcc0-188">第三個呼叫動態繫結： 多載`Console.WriteLine`會選擇根據執行階段型別，其引數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-188">The third call is dynamically bound: the overload of `Console.WriteLine` is picked based on the run-time type of its argument.</span></span> <span data-ttu-id="abcc0-189">這是因為引數是動態的運算式--其編譯時期型別是`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-189">This happens because the argument is a dynamic expression -- its compile-time type is `dynamic`.</span></span> <span data-ttu-id="abcc0-190">因此，第三個呼叫繫結時間是執行階段。</span><span class="sxs-lookup"><span data-stu-id="abcc0-190">Thus, the binding-time for the third call is run-time.</span></span>

### <a name="dynamic-binding"></a><span data-ttu-id="abcc0-191">動態繫結</span><span class="sxs-lookup"><span data-stu-id="abcc0-191">Dynamic binding</span></span>

<span data-ttu-id="abcc0-192">動態繫結的目的是以允許 C# 程式互動***動態物件***，也就是不遵循 C# 的一般規則的物件型別系統。</span><span class="sxs-lookup"><span data-stu-id="abcc0-192">The purpose of dynamic binding is to allow C# programs to interact with ***dynamic objects***, i.e. objects that do not follow the normal rules of the C# type system.</span></span> <span data-ttu-id="abcc0-193">動態物件可能會從其他程式設計語言，與不同類型系統的物件，或者可能是以程式設計的方式安裝程式，以實作自己的繫結語意不同作業的物件。</span><span class="sxs-lookup"><span data-stu-id="abcc0-193">Dynamic objects may be objects from other programming languages with different types systems, or they may be objects that are programmatically setup to implement their own binding semantics for different operations.</span></span>

<span data-ttu-id="abcc0-194">供動態物件會實作本身語意的機制是實作所定義。</span><span class="sxs-lookup"><span data-stu-id="abcc0-194">The mechanism by which a dynamic object implements its own semantics is implementation defined.</span></span> <span data-ttu-id="abcc0-195">指定的介面--重新定義的實作，被實作的動態物件來表示 C# 執行階段，它們會有特殊的語意。</span><span class="sxs-lookup"><span data-stu-id="abcc0-195">A given interface -- again implementation defined -- is implemented by dynamic objects to signal to the C# run-time that they have special semantics.</span></span> <span data-ttu-id="abcc0-196">因此，動態繫結的動態物件上的作業，只要他們自己的繫結語意，而不是 C# 中本文件中，所指定的接管。</span><span class="sxs-lookup"><span data-stu-id="abcc0-196">Thus, whenever operations on a dynamic object are dynamically bound, their own binding semantics, rather than those of C# as specified in this document, take over.</span></span>

<span data-ttu-id="abcc0-197">動態繫結的目的是允許使用動態物件互通性，而 C# 可讓動態繫結上的所有物件，無論或不是動態。</span><span class="sxs-lookup"><span data-stu-id="abcc0-197">While the purpose of dynamic binding is to allow interoperation with dynamic objects, C# allows dynamic binding on all objects, whether they are dynamic or not.</span></span> <span data-ttu-id="abcc0-198">這可讓順暢整合的動態物件，在其上作業的結果可能本身不是動態物件，但仍是程式設計師在編譯時期未知的類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-198">This allows for a smoother integration of dynamic objects, as the results of operations on them may not themselves be dynamic objects, but are still of a type unknown to the programmer at compile-time.</span></span> <span data-ttu-id="abcc0-199">也可協助動態繫結，消除出錯反映架構的程式碼，即使在沒有所牽涉的物件是動態的物件。</span><span class="sxs-lookup"><span data-stu-id="abcc0-199">Also dynamic binding can help eliminate error-prone reflection-based code even when no objects involved are dynamic objects.</span></span>

<span data-ttu-id="abcc0-200">完全動態繫結套用時，什麼編譯時間檢查，如果套用任何-，以及編譯時間結果與運算式分類為下列各節說明在語言中的每個建構。</span><span class="sxs-lookup"><span data-stu-id="abcc0-200">The following sections describe for each construct in the language exactly when dynamic binding is applied, what compile time checking -- if any -- is applied, and what the compile-time result and expression classification is.</span></span>

### <a name="types-of-constituent-expressions"></a><span data-ttu-id="abcc0-201">構成運算式的型別</span><span class="sxs-lookup"><span data-stu-id="abcc0-201">Types of constituent expressions</span></span>

<span data-ttu-id="abcc0-202">當作業以靜態方式繫結時，構成的運算式 （例如，接收端、 引數、 索引或運算元） 的型別一律會視為是編譯時期型別，該運算式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-202">When an operation is statically bound, the type of a constituent expression (e.g. a receiver, an argument, an index or an operand) is always considered to be the compile-time type of that expression.</span></span>

<span data-ttu-id="abcc0-203">當動態繫結作業時，構成運算式的類型決定以不同的方式，取決於構成運算式的編譯時間類型：</span><span class="sxs-lookup"><span data-stu-id="abcc0-203">When an operation is dynamically bound, the type of a constituent expression is determined in different ways depending on the compile-time type of the constituent expression:</span></span>

*  <span data-ttu-id="abcc0-204">編譯時期型別構成運算式`dynamic`被視為具有在執行階段運算式評估為實際值的型別</span><span class="sxs-lookup"><span data-stu-id="abcc0-204">A constituent expression of compile-time type `dynamic` is considered to have the type of the actual value that the expression evaluates to at runtime</span></span>
*  <span data-ttu-id="abcc0-205">其編譯時期型別為型別參數的構成運算式被視為具有型別參數在繫結至執行階段的型別</span><span class="sxs-lookup"><span data-stu-id="abcc0-205">A constituent expression whose compile-time type is a type parameter is considered to have the type which the type parameter is bound to at runtime</span></span>
*  <span data-ttu-id="abcc0-206">否則構成的運算式被視為具有其編譯時期型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-206">Otherwise the constituent expression is considered to have its compile-time type.</span></span>

## <a name="operators"></a><span data-ttu-id="abcc0-207">運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-207">Operators</span></span>

<span data-ttu-id="abcc0-208">運算式由組成***運算元***並***運算子***。</span><span class="sxs-lookup"><span data-stu-id="abcc0-208">Expressions are constructed from ***operands*** and ***operators***.</span></span> <span data-ttu-id="abcc0-209">運算式的運算子會指出要將哪些運算套用到運算元。</span><span class="sxs-lookup"><span data-stu-id="abcc0-209">The operators of an expression indicate which operations to apply to the operands.</span></span> <span data-ttu-id="abcc0-210">運算子範例包括 `+`、`-`、`*`、`/` 及 `new`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-210">Examples of operators include `+`, `-`, `*`, `/`, and `new`.</span></span> <span data-ttu-id="abcc0-211">運算元範例包括常值、欄位、區域變數及運算式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-211">Examples of operands include literals, fields, local variables, and expressions.</span></span>

<span data-ttu-id="abcc0-212">有三種類型的運算子：</span><span class="sxs-lookup"><span data-stu-id="abcc0-212">There are three kinds of operators:</span></span>

*  <span data-ttu-id="abcc0-213">一元運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-213">Unary operators.</span></span> <span data-ttu-id="abcc0-214">一元運算子一個運算元，並使用其中一個前置標記法 (例如`--x`) 或後置標記法 (例如`x++`)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-214">The unary operators take one operand and use either prefix notation (such as `--x`) or postfix notation (such as `x++`).</span></span>
*  <span data-ttu-id="abcc0-215">二元運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-215">Binary operators.</span></span> <span data-ttu-id="abcc0-216">二元運算子需要兩個運算元和所有使用中置標記法 (例如`x + y`)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-216">The binary operators take two operands and all use infix notation (such as `x + y`).</span></span>
*  <span data-ttu-id="abcc0-217">三元運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-217">Ternary operator.</span></span> <span data-ttu-id="abcc0-218">只能有一個三元運算子`?:`，存在，它會採用三個運算元，並使用中置標記法 (`c ? x : y`)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-218">Only one ternary operator, `?:`, exists; it takes three operands and uses infix notation (`c ? x : y`).</span></span>

<span data-ttu-id="abcc0-219">取決於運算式中運算子的評估順序***優先順序***並***關聯性***一個運算子 ([運算子優先順序和關聯性](expressions.md#operator-precedence-and-associativity)).</span><span class="sxs-lookup"><span data-stu-id="abcc0-219">The order of evaluation of operators in an expression is determined by the ***precedence*** and ***associativity*** of the operators ([Operator precedence and associativity](expressions.md#operator-precedence-and-associativity)).</span></span>

<span data-ttu-id="abcc0-220">在運算式中的運算元是從左到右評估。</span><span class="sxs-lookup"><span data-stu-id="abcc0-220">Operands in an expression are evaluated from left to right.</span></span> <span data-ttu-id="abcc0-221">例如，在`F(i) + G(i++) * H(i)`，方法`F`使用的舊值來呼叫`i`，然後方法`G`呼叫的舊值`i`，和，最後，方法`H`新值呼叫`i`.</span><span class="sxs-lookup"><span data-stu-id="abcc0-221">For example, in `F(i) + G(i++) * H(i)`, method `F` is called using the old value of `i`, then method `G` is called with the old value of `i`, and, finally, method `H` is called with the new value of `i`.</span></span> <span data-ttu-id="abcc0-222">這是分開並不相關的運算子優先順序。</span><span class="sxs-lookup"><span data-stu-id="abcc0-222">This is separate from and unrelated to operator precedence.</span></span>

<span data-ttu-id="abcc0-223">可以是特定運算子***多載***。</span><span class="sxs-lookup"><span data-stu-id="abcc0-223">Certain operators can be ***overloaded***.</span></span> <span data-ttu-id="abcc0-224">運算子多載允許使用者定義運算子實作，其指定作業的一或兩個運算元屬於使用者定義的類別或結構類型 ([運算子多載](expressions.md#operator-overloading))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-224">Operator overloading permits user-defined operator implementations to be specified for operations where one or both of the operands are of a user-defined class or struct type ([Operator overloading](expressions.md#operator-overloading)).</span></span>

### <a name="operator-precedence-and-associativity"></a><span data-ttu-id="abcc0-225">運算子優先順序和關聯性</span><span class="sxs-lookup"><span data-stu-id="abcc0-225">Operator precedence and associativity</span></span>

<span data-ttu-id="abcc0-226">當運算式包含多個運算子時，運算子的「優先順序」會控制評估個別運算子的順序。</span><span class="sxs-lookup"><span data-stu-id="abcc0-226">When an expression contains multiple operators, the ***precedence*** of the operators controls the order in which the individual operators are evaluated.</span></span> <span data-ttu-id="abcc0-227">例如，運算式`x + y * z`評估為`x + (y * z)`因為`*`運算子的優先順序高於二進位檔`+`運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-227">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the binary `+` operator.</span></span> <span data-ttu-id="abcc0-228">運算子的優先順序是由其相關聯的文法生產環境的定義建立的。</span><span class="sxs-lookup"><span data-stu-id="abcc0-228">The precedence of an operator is established by the definition of its associated grammar production.</span></span> <span data-ttu-id="abcc0-229">例如， *additive_expression*組成的序列*multiplicative_expression*s 分隔`+`或`-`運算子，因此讓`+`和`-`運算子較低的優先順序高於`*`， `/`，和`%`運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-229">For example, an *additive_expression* consists of a sequence of *multiplicative_expression*s separated by `+` or `-` operators, thus giving the `+` and `-` operators lower precedence than the `*`, `/`, and `%` operators.</span></span>

<span data-ttu-id="abcc0-230">下表摘要說明所有運算子的優先順序從最高到低排列順序：</span><span class="sxs-lookup"><span data-stu-id="abcc0-230">The following table summarizes all operators in order of precedence from highest to lowest:</span></span>

| <span data-ttu-id="abcc0-231">__區段__</span><span class="sxs-lookup"><span data-stu-id="abcc0-231">__Section__</span></span>                                                                                   | <span data-ttu-id="abcc0-232">__分類__</span><span class="sxs-lookup"><span data-stu-id="abcc0-232">__Category__</span></span>                | <span data-ttu-id="abcc0-233">__運算子__</span><span class="sxs-lookup"><span data-stu-id="abcc0-233">__Operators__</span></span> | 
|-----------------------------------------------------------------------------------------------|-----------------------------|---------------|
| [<span data-ttu-id="abcc0-234">主要運算式</span><span class="sxs-lookup"><span data-stu-id="abcc0-234">Primary expressions</span></span>](expressions.md#primary-expressions)                                     | <span data-ttu-id="abcc0-235">主要</span><span class="sxs-lookup"><span data-stu-id="abcc0-235">Primary</span></span>                     | <span data-ttu-id="abcc0-236">`x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate`</span><span class="sxs-lookup"><span data-stu-id="abcc0-236">`x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate`</span></span> | 
| [<span data-ttu-id="abcc0-237">一元運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-237">Unary operators</span></span>](expressions.md#unary-operators)                                             | <span data-ttu-id="abcc0-238">一元</span><span class="sxs-lookup"><span data-stu-id="abcc0-238">Unary</span></span>                       | <span data-ttu-id="abcc0-239">`+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x`</span><span class="sxs-lookup"><span data-stu-id="abcc0-239">`+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x`</span></span> | 
| [<span data-ttu-id="abcc0-240">算術運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-240">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)                                   | <span data-ttu-id="abcc0-241">乘法類 (Multiplicative)</span><span class="sxs-lookup"><span data-stu-id="abcc0-241">Multiplicative</span></span>              | <span data-ttu-id="abcc0-242">`*`  `/`  `%`</span><span class="sxs-lookup"><span data-stu-id="abcc0-242">`*`  `/`  `%`</span></span> | 
| [<span data-ttu-id="abcc0-243">算術運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-243">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)                                   | <span data-ttu-id="abcc0-244">加法類 (Additive)</span><span class="sxs-lookup"><span data-stu-id="abcc0-244">Additive</span></span>                    | <span data-ttu-id="abcc0-245">`+`  `-`</span><span class="sxs-lookup"><span data-stu-id="abcc0-245">`+`  `-`</span></span>      | 
| [<span data-ttu-id="abcc0-246">移位運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-246">Shift operators</span></span>](expressions.md#shift-operators)                                             | <span data-ttu-id="abcc0-247">Shift</span><span class="sxs-lookup"><span data-stu-id="abcc0-247">Shift</span></span>                       | <span data-ttu-id="abcc0-248">`<<`  `>>`</span><span class="sxs-lookup"><span data-stu-id="abcc0-248">`<<`  `>>`</span></span>    | 
| [<span data-ttu-id="abcc0-249">關係和類型測試運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-249">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators) | <span data-ttu-id="abcc0-250">關係和型別測試</span><span class="sxs-lookup"><span data-stu-id="abcc0-250">Relational and type testing</span></span> | <span data-ttu-id="abcc0-251">`<`  `>`  `<=`  `>=`  `is`  `as`</span><span class="sxs-lookup"><span data-stu-id="abcc0-251">`<`  `>`  `<=`  `>=`  `is`  `as`</span></span> | 
| [<span data-ttu-id="abcc0-252">關係和類型測試運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-252">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators) | <span data-ttu-id="abcc0-253">相等</span><span class="sxs-lookup"><span data-stu-id="abcc0-253">Equality</span></span>                    | <span data-ttu-id="abcc0-254">`==`  `!=`</span><span class="sxs-lookup"><span data-stu-id="abcc0-254">`==`  `!=`</span></span>    | 
| [<span data-ttu-id="abcc0-255">邏輯運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-255">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="abcc0-256">邏輯 AND</span><span class="sxs-lookup"><span data-stu-id="abcc0-256">Logical AND</span></span>                 | `&`           | 
| [<span data-ttu-id="abcc0-257">邏輯運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-257">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="abcc0-258">邏輯 XOR</span><span class="sxs-lookup"><span data-stu-id="abcc0-258">Logical XOR</span></span>                 | `^`           | 
| [<span data-ttu-id="abcc0-259">邏輯運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-259">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="abcc0-260">邏輯 OR</span><span class="sxs-lookup"><span data-stu-id="abcc0-260">Logical OR</span></span>                  | <code>&#124;</code>           |
| [<span data-ttu-id="abcc0-261">條件邏輯運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-261">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)                 | <span data-ttu-id="abcc0-262">條件式 AND</span><span class="sxs-lookup"><span data-stu-id="abcc0-262">Conditional AND</span></span>             | `&&`          | 
| [<span data-ttu-id="abcc0-263">條件邏輯運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-263">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)                 | <span data-ttu-id="abcc0-264">條件式 OR</span><span class="sxs-lookup"><span data-stu-id="abcc0-264">Conditional OR</span></span>              | <code>&#124;&#124;</code>          | 
| [<span data-ttu-id="abcc0-265">Null 聯合運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-265">The null coalescing operator</span></span>](expressions.md#the-null-coalescing-operator)                   | <span data-ttu-id="abcc0-266">Null 聯合</span><span class="sxs-lookup"><span data-stu-id="abcc0-266">Null coalescing</span></span>             | `??`          | 
| [<span data-ttu-id="abcc0-267">條件運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-267">Conditional operator</span></span>](expressions.md#conditional-operator)                                   | <span data-ttu-id="abcc0-268">條件式</span><span class="sxs-lookup"><span data-stu-id="abcc0-268">Conditional</span></span>                 | `?:`          | 
| <span data-ttu-id="abcc0-269">[指派運算子](expressions.md#assignment-operators)，[匿名函式運算式](expressions.md#anonymous-function-expressions)</span><span class="sxs-lookup"><span data-stu-id="abcc0-269">[Assignment operators](expressions.md#assignment-operators), [Anonymous function expressions](expressions.md#anonymous-function-expressions)</span></span>  | <span data-ttu-id="abcc0-270">指派和 lambda 運算式</span><span class="sxs-lookup"><span data-stu-id="abcc0-270">Assignment and lambda expression</span></span> | <span data-ttu-id="abcc0-271">`=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>`</span><span class="sxs-lookup"><span data-stu-id="abcc0-271">`=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>`</span></span> | 

<span data-ttu-id="abcc0-272">介於兩個具有相同優先順序的運算子的運算元時，運算子的順序關聯性會控制所執行之作業的順序：</span><span class="sxs-lookup"><span data-stu-id="abcc0-272">When an operand occurs between two operators with the same precedence, the associativity of the operators controls the order in which the operations are performed:</span></span>

*  <span data-ttu-id="abcc0-273">除了指派運算子和 null 聯合運算子，所有二元運算子都***左向***，也就是說，作業將會從左到右。</span><span class="sxs-lookup"><span data-stu-id="abcc0-273">Except for the assignment operators and the null coalescing operator, all binary operators are ***left-associative***, meaning that operations are performed from left to right.</span></span> <span data-ttu-id="abcc0-274">例如，`x + y + z` 會判斷值為 `(x + y) + z`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-274">For example, `x + y + z` is evaluated as `(x + y) + z`.</span></span>
*  <span data-ttu-id="abcc0-275">指派運算子、 null 聯合運算子和條件運算子 (`?:`) 是***右向關聯***，亦即在由右至左執行運算。</span><span class="sxs-lookup"><span data-stu-id="abcc0-275">The assignment operators, the null coalescing operator and the conditional operator (`?:`) are ***right-associative***, meaning that operations are performed from right to left.</span></span> <span data-ttu-id="abcc0-276">例如，`x = y = z` 會判斷值為 `x = (y = z)`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-276">For example, `x = y = z` is evaluated as `x = (y = z)`.</span></span>

<span data-ttu-id="abcc0-277">您可以使用括弧來控制優先順序和關聯性。</span><span class="sxs-lookup"><span data-stu-id="abcc0-277">Precedence and associativity can be controlled using parentheses.</span></span> <span data-ttu-id="abcc0-278">例如，`x + y * z` 會先將 `y` 乘以 `z`，然後再將結果加到 `x`，而 `(x + y) * z` 則會先將 `x` 與 `y` 相加，然後再將結果乘以 `z`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-278">For example, `x + y * z` first multiplies `y` by `z` and then adds the result to `x`, but `(x + y) * z` first adds `x` and `y` and then multiplies the result by `z`.</span></span>

### <a name="operator-overloading"></a><span data-ttu-id="abcc0-279">運算子多載</span><span class="sxs-lookup"><span data-stu-id="abcc0-279">Operator overloading</span></span>

<span data-ttu-id="abcc0-280">所有的一元和二元運算子有預先定義的實作，會自動出現在任何運算式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-280">All unary and binary operators have predefined implementations that are automatically available in any expression.</span></span> <span data-ttu-id="abcc0-281">除了預先定義的實作中，使用者定義的實作可以引進加`operator`類別和結構中的宣告 ([運算子](classes.md#operators))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-281">In addition to the predefined implementations, user-defined implementations can be introduced by including `operator` declarations in classes and structs ([Operators](classes.md#operators)).</span></span> <span data-ttu-id="abcc0-282">使用者定義運算子實作，一律優先於預先定義的運算子實作：只有當沒有適用的使用者定義運算子實作預先定義的運算子會考慮實作，如中所述[一元運算子多載解析](expressions.md#unary-operator-overload-resolution)和[二元運算子多載解析](expressions.md#binary-operator-overload-resolution)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-282">User-defined operator implementations always take precedence over predefined operator implementations: Only when no applicable user-defined operator implementations exist will the predefined operator implementations be considered, as described in [Unary operator overload resolution](expressions.md#unary-operator-overload-resolution) and [Binary operator overload resolution](expressions.md#binary-operator-overload-resolution).</span></span>

<span data-ttu-id="abcc0-283">***多載的一元運算子***是：</span><span class="sxs-lookup"><span data-stu-id="abcc0-283">The ***overloadable unary operators*** are:</span></span>
```csharp
+   -   !   ~   ++   --   true   false
```

<span data-ttu-id="abcc0-284">雖然`true`和`false`不會在運算式中明確使用 (並因此不包含在中，針對優先順序表格[運算子優先順序和關聯性](expressions.md#operator-precedence-and-associativity))，它們被視為運算子，因為它們是在數個運算式內容中叫用： 布林運算式 ([布林運算式](expressions.md#boolean-expressions)) 和包含條件式運算式 ([條件運算子](expressions.md#conditional-operator))，和條件式邏輯運算子 ([條件式邏輯運算子](expressions.md#conditional-logical-operators))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-284">Although `true` and `false` are not used explicitly in expressions (and therefore are not included in the precedence table in [Operator precedence and associativity](expressions.md#operator-precedence-and-associativity)), they are considered operators because they are invoked in several expression contexts: boolean expressions ([Boolean expressions](expressions.md#boolean-expressions)) and expressions involving the conditional ([Conditional operator](expressions.md#conditional-operator)), and conditional logical operators ([Conditional logical operators](expressions.md#conditional-logical-operators)).</span></span>

<span data-ttu-id="abcc0-285">***多載的二元運算子***是：</span><span class="sxs-lookup"><span data-stu-id="abcc0-285">The ***overloadable binary operators*** are:</span></span>
```csharp
+   -   *   /   %   &   |   ^   <<   >>   ==   !=   >   <   >=   <=
```

<span data-ttu-id="abcc0-286">只有將上述的運算子可以多載。</span><span class="sxs-lookup"><span data-stu-id="abcc0-286">Only the operators listed above can be overloaded.</span></span> <span data-ttu-id="abcc0-287">特別是，不可能多載成員存取，方法引動過程，或是`=`， `&&`， `||`， `??`， `?:`， `=>`， `checked`， `unchecked`， `new`， `typeof`， `default`， `as`，和`is`運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-287">In particular, it is not possible to overload member access, method invocation, or the `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, `default`, `as`, and `is` operators.</span></span>

<span data-ttu-id="abcc0-288">二元運算子多載時，對應的指派運算子 (若有) 也會隱含地多載。</span><span class="sxs-lookup"><span data-stu-id="abcc0-288">When a binary operator is overloaded, the corresponding assignment operator, if any, is also implicitly overloaded.</span></span> <span data-ttu-id="abcc0-289">比方說，運算子多載`*`也是運算子多載`*=`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-289">For example, an overload of operator `*` is also an overload of operator `*=`.</span></span> <span data-ttu-id="abcc0-290">這是更進一步的說明[複合指派](expressions.md#compound-assignment)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-290">This is described further in [Compound assignment](expressions.md#compound-assignment).</span></span> <span data-ttu-id="abcc0-291">請注意，指派運算子本身 (`=`) 無法多載。</span><span class="sxs-lookup"><span data-stu-id="abcc0-291">Note that the assignment operator itself (`=`) cannot be overloaded.</span></span> <span data-ttu-id="abcc0-292">一律指派執行簡單的位元值副本放入變數中。</span><span class="sxs-lookup"><span data-stu-id="abcc0-292">An assignment always performs a simple bit-wise copy of a value into a variable.</span></span>

<span data-ttu-id="abcc0-293">轉換作業，例如`(T)x`，藉由提供使用者定義轉換都多載 ([使用者定義轉換](conversions.md#user-defined-conversions))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-293">Cast operations, such as `(T)x`, are overloaded by providing user-defined conversions ([User-defined conversions](conversions.md#user-defined-conversions)).</span></span>

<span data-ttu-id="abcc0-294">項目的存取，例如`a[x]`，不是可多載的運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-294">Element access, such as `a[x]`, is not considered an overloadable operator.</span></span> <span data-ttu-id="abcc0-295">相反地，使用者定義編製索引支援透過索引子 ([索引子](classes.md#indexers))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-295">Instead, user-defined indexing is supported through indexers ([Indexers](classes.md#indexers)).</span></span>

<span data-ttu-id="abcc0-296">在運算式中，運算子會參考使用運算子表示法，而且在宣告中，運算子會使用參考函式標記法。</span><span class="sxs-lookup"><span data-stu-id="abcc0-296">In expressions, operators are referenced using operator notation, and in declarations, operators are referenced using functional notation.</span></span> <span data-ttu-id="abcc0-297">下表顯示運算子的一元和二元運算子的功能標記法之間的關聯性。</span><span class="sxs-lookup"><span data-stu-id="abcc0-297">The following table shows the relationship between operator and functional notations for unary and binary operators.</span></span> <span data-ttu-id="abcc0-298">在第一個項目時， *op*代表任何多載的一元前置運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-298">In the first entry, *op* denotes any overloadable unary prefix operator.</span></span> <span data-ttu-id="abcc0-299">在第二個項目時， *op*代表一元後置`++`和`--`運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-299">In the second entry, *op* denotes the unary postfix `++` and `--` operators.</span></span> <span data-ttu-id="abcc0-300">在第三個項目時， *op*代表任何多載的二元運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-300">In the third entry, *op* denotes any overloadable binary operator.</span></span>


| <span data-ttu-id="abcc0-301">__運算子標記法__</span><span class="sxs-lookup"><span data-stu-id="abcc0-301">__Operator notation__</span></span> | <span data-ttu-id="abcc0-302">__函式標記法__</span><span class="sxs-lookup"><span data-stu-id="abcc0-302">__Functional notation__</span></span> |
|-----------------------|-------------------------|
| `op x`                | `operator op(x)`        | 
| `x op`                | `operator op(x)`        | 
| `x op y`              | `operator op(x,y)`      | 

<span data-ttu-id="abcc0-303">使用者定義運算子的宣告一律需要至少一個包含運算子宣告類別或結構類型的參數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-303">User-defined operator declarations always require at least one of the parameters to be of the class or struct type that contains the operator declaration.</span></span> <span data-ttu-id="abcc0-304">因此，不可能的使用者定義的運算子具有相同的簽章，做為預先定義的運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-304">Thus, it is not possible for a user-defined operator to have the same signature as a predefined operator.</span></span>

<span data-ttu-id="abcc0-305">語法、 優先順序或運算子的順序關聯性，無法修改使用者定義運算子的宣告。</span><span class="sxs-lookup"><span data-stu-id="abcc0-305">User-defined operator declarations cannot modify the syntax, precedence, or associativity of an operator.</span></span> <span data-ttu-id="abcc0-306">例如，`/`運算子一律是二元運算子，一律具有的優先順序層級中指定[運算子優先順序和關聯性](expressions.md#operator-precedence-and-associativity)，而且永遠都是左關聯性。</span><span class="sxs-lookup"><span data-stu-id="abcc0-306">For example, the `/` operator is always a binary operator, always has the precedence level specified in [Operator precedence and associativity](expressions.md#operator-precedence-and-associativity), and is always left-associative.</span></span>

<span data-ttu-id="abcc0-307">雖然您可以執行任何計算為所欲為的使用者定義運算子，實作會產生不是預期的結果是強烈建議您不要。</span><span class="sxs-lookup"><span data-stu-id="abcc0-307">While it is possible for a user-defined operator to perform any computation it pleases, implementations that produce results other than those that are intuitively expected are strongly discouraged.</span></span> <span data-ttu-id="abcc0-308">比方說，實作`operator ==`應該會比較兩個運算元是否相等，並傳回適當`bool`結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-308">For example, an implementation of `operator ==` should compare the two operands for equality and return an appropriate `bool` result.</span></span>

<span data-ttu-id="abcc0-309">中的個別運算子的說明[主要運算式](expressions.md#primary-expressions)透過[條件式邏輯運算子](expressions.md#conditional-logical-operators)指定預先定義的運算子和套用的任何其他規則實作每個運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-309">The descriptions of individual operators in [Primary expressions](expressions.md#primary-expressions) through [Conditional logical operators](expressions.md#conditional-logical-operators) specify the predefined implementations of the operators and any additional rules that apply to each operator.</span></span> <span data-ttu-id="abcc0-310">描述能夠使用的詞彙***一元運算子多載解析***，***二元運算子多載解析***，並***數值升級***，這是定義在下列各節中找到。</span><span class="sxs-lookup"><span data-stu-id="abcc0-310">The descriptions make use of the terms ***unary operator overload resolution***, ***binary operator overload resolution***, and ***numeric promotion***, definitions of which are found in the following sections.</span></span>

### <a name="unary-operator-overload-resolution"></a><span data-ttu-id="abcc0-311">一元運算子多載解析</span><span class="sxs-lookup"><span data-stu-id="abcc0-311">Unary operator overload resolution</span></span>

<span data-ttu-id="abcc0-312">表單的作業`op x`或`x op`，其中`op`是可多載的一元運算子，以及`x`這類型的運算式`X`，處理，如下所示：</span><span class="sxs-lookup"><span data-stu-id="abcc0-312">An operation of the form `op x` or `x op`, where `op` is an overloadable unary operator, and `x` is an expression of type `X`, is processed as follows:</span></span>

*  <span data-ttu-id="abcc0-313">一組所提供的候選使用者定義運算子`X`作業`operator op(x)`使用的規則決定[候選使用者定義運算子](expressions.md#candidate-user-defined-operators)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-313">The set of candidate user-defined operators provided by `X` for the operation `operator op(x)` is determined using the rules of [Candidate user-defined operators](expressions.md#candidate-user-defined-operators).</span></span>
*  <span data-ttu-id="abcc0-314">如果一組候選使用者定義運算子不是空的則這會變成一組作業的候選項目運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-314">If the set of candidate user-defined operators is not empty, then this becomes the set of candidate operators for the operation.</span></span> <span data-ttu-id="abcc0-315">否則，預先定義的一元`operator op`實作，包括其提昇的形式，將會成為一組作業的候選項目運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-315">Otherwise, the predefined unary `operator op` implementations, including their lifted forms, become the set of candidate operators for the operation.</span></span> <span data-ttu-id="abcc0-316">運算子的說明中指定的給定操作員的預先定義的實作 ([主要運算式](expressions.md#primary-expressions)並[一元運算子](expressions.md#unary-operators))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-316">The predefined implementations of a given operator are specified in the description of the operator ([Primary expressions](expressions.md#primary-expressions) and [Unary operators](expressions.md#unary-operators)).</span></span>
*  <span data-ttu-id="abcc0-317">多載解析規則[多載解析](expressions.md#overload-resolution)套用至一組選取最佳的運算子，根據引數清單的候選項目運算子`(x)`，而這個運算子多載的結果解析程序。</span><span class="sxs-lookup"><span data-stu-id="abcc0-317">The overload resolution rules of [Overload resolution](expressions.md#overload-resolution) are applied to the set of candidate operators to select the best operator with respect to the argument list `(x)`, and this operator becomes the result of the overload resolution process.</span></span> <span data-ttu-id="abcc0-318">如果選取單一最佳的運算子無法多載解析，繫結階段會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-318">If overload resolution fails to select a single best operator, a binding-time error occurs.</span></span>

### <a name="binary-operator-overload-resolution"></a><span data-ttu-id="abcc0-319">二元運算子多載解析</span><span class="sxs-lookup"><span data-stu-id="abcc0-319">Binary operator overload resolution</span></span>

<span data-ttu-id="abcc0-320">表單的作業`x op y`，其中`op`是一個多載的二元運算子`x`是類型的運算式`X`，和`y`是類型的運算式`Y`，處理時，如下所示：</span><span class="sxs-lookup"><span data-stu-id="abcc0-320">An operation of the form `x op y`, where `op` is an overloadable binary operator, `x` is an expression of type `X`, and `y` is an expression of type `Y`, is processed as follows:</span></span>

*  <span data-ttu-id="abcc0-321">一組所提供的候選使用者定義運算子`X`並`Y`作業`operator op(x,y)`決定。</span><span class="sxs-lookup"><span data-stu-id="abcc0-321">The set of candidate user-defined operators provided by `X` and `Y` for the operation `operator op(x,y)` is determined.</span></span> <span data-ttu-id="abcc0-322">集合所組成的聯集所提供的候選項目運算子`X`以及所提供的候選項目運算子`Y`，每個使用的規則決定[候選使用者定義運算子](expressions.md#candidate-user-defined-operators)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-322">The set consists of the union of the candidate operators provided by `X` and the candidate operators provided by `Y`, each determined using the rules of [Candidate user-defined operators](expressions.md#candidate-user-defined-operators).</span></span> <span data-ttu-id="abcc0-323">如果`X`並`Y`都是相同的類型，或如果`X`和`Y`衍生自一般基底型別，則共用的候選運算子只會發生組合中一次。</span><span class="sxs-lookup"><span data-stu-id="abcc0-323">If `X` and `Y` are the same type, or if `X` and `Y` are derived from a common base type, then shared candidate operators only occur in the combined set once.</span></span>
*  <span data-ttu-id="abcc0-324">如果一組候選使用者定義運算子不是空的則這會變成一組作業的候選項目運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-324">If the set of candidate user-defined operators is not empty, then this becomes the set of candidate operators for the operation.</span></span> <span data-ttu-id="abcc0-325">否則，預先定義的二進位檔`operator op`實作，包括其提昇的形式，將會成為一組作業的候選項目運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-325">Otherwise, the predefined binary `operator op` implementations, including their lifted forms,  become the set of candidate operators for the operation.</span></span> <span data-ttu-id="abcc0-326">運算子的說明中指定的給定操作員的預先定義的實作 ([算術運算子](expressions.md#arithmetic-operators)透過[條件式邏輯運算子](expressions.md#conditional-logical-operators))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-326">The predefined implementations of a given operator are specified in the description of the operator ([Arithmetic operators](expressions.md#arithmetic-operators) through [Conditional logical operators](expressions.md#conditional-logical-operators)).</span></span> <span data-ttu-id="abcc0-327">預先定義的列舉和委派運算子，視為唯一運算子是定義列舉或委派的型別，來為其中一個運算元的繫結階段類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-327">For predefined enum and delegate operators, the only operators considered are those defined by an enum or delegate type that is the binding-time type of one of the operands.</span></span>
*  <span data-ttu-id="abcc0-328">多載解析規則[多載解析](expressions.md#overload-resolution)套用至一組選取最佳的運算子，根據引數清單的候選項目運算子`(x,y)`，而這個運算子多載的結果解析程序。</span><span class="sxs-lookup"><span data-stu-id="abcc0-328">The overload resolution rules of [Overload resolution](expressions.md#overload-resolution) are applied to the set of candidate operators to select the best operator with respect to the argument list `(x,y)`, and this operator becomes the result of the overload resolution process.</span></span> <span data-ttu-id="abcc0-329">如果選取單一最佳的運算子無法多載解析，繫結階段會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-329">If overload resolution fails to select a single best operator, a binding-time error occurs.</span></span>

### <a name="candidate-user-defined-operators"></a><span data-ttu-id="abcc0-330">候選使用者定義運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-330">Candidate user-defined operators</span></span>

<span data-ttu-id="abcc0-331">提供型別的`T`和作業`operator op(A)`，其中`op`是可多載的運算子和`A`是所提供的使用者定義運算子的引數清單中，候選項目組`T`的`operator op(A)`取決於如下所示：</span><span class="sxs-lookup"><span data-stu-id="abcc0-331">Given a type `T` and an operation `operator op(A)`, where `op` is an overloadable operator and `A` is an argument list, the set of candidate user-defined operators provided by `T` for `operator op(A)` is determined as follows:</span></span>

*  <span data-ttu-id="abcc0-332">判斷型別`T0`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-332">Determine the type `T0`.</span></span> <span data-ttu-id="abcc0-333">如果`T`為 null 的型別`T0`是其基礎類型，否則為`T0`等於`T`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-333">If `T` is a nullable type, `T0` is its underlying type, otherwise `T0` is equal to `T`.</span></span>
*  <span data-ttu-id="abcc0-334">針對所有`operator op`中的宣告`T0`和所有提昇形式的這類運算子，如果至少一個運算子時適用 ([適用的函式成員](expressions.md#applicable-function-member)) 引數清單`A`，一組候選運算子包含在所有適用的運算子`T0`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-334">For all `operator op` declarations in `T0` and all lifted forms of such operators, if at least one operator is applicable ([Applicable function member](expressions.md#applicable-function-member)) with respect to the argument list `A`, then the set of candidate operators consists of all such applicable operators in `T0`.</span></span>
*  <span data-ttu-id="abcc0-335">否則，如果`T0`是`object`，一組候選項目運算子是空的。</span><span class="sxs-lookup"><span data-stu-id="abcc0-335">Otherwise, if `T0` is `object`, the set of candidate operators is empty.</span></span>
*  <span data-ttu-id="abcc0-336">一組候選項目運算子所提供的否則為`T0`是一組直接基底類別所提供的候選項目運算子`T0`，或有效基底類別`T0`如果`T0`是型別參數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-336">Otherwise, the set of candidate operators provided by `T0` is the set of candidate operators provided by the direct base class of `T0`, or the effective base class of `T0` if `T0` is a type parameter.</span></span>

### <a name="numeric-promotions"></a><span data-ttu-id="abcc0-337">數字的促銷活動</span><span class="sxs-lookup"><span data-stu-id="abcc0-337">Numeric promotions</span></span>

<span data-ttu-id="abcc0-338">數字的促銷活動包含自動執行某些隱含轉換的預先定義的一元和二元數值運算子的運算元。</span><span class="sxs-lookup"><span data-stu-id="abcc0-338">Numeric promotion consists of automatically performing certain implicit conversions of the operands of the predefined unary and binary numeric operators.</span></span> <span data-ttu-id="abcc0-339">數值升級並不是不同的機制，而是套用預先定義的運算子多載解析的效果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-339">Numeric promotion is not a distinct mechanism, but rather an effect of applying overload resolution to the predefined operators.</span></span> <span data-ttu-id="abcc0-340">數值升級特別不會影響評估使用者定義的運算子，雖然可以實作使用者定義的運算子，以呈現類似的效果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-340">Numeric promotion specifically does not affect evaluation of user-defined operators, although user-defined operators can be implemented to exhibit similar effects.</span></span>

<span data-ttu-id="abcc0-341">數字的促銷活動的範例，請考慮預先定義的實作，二進位檔的`*`運算子：</span><span class="sxs-lookup"><span data-stu-id="abcc0-341">As an example of numeric promotion, consider the predefined implementations of the binary `*` operator:</span></span>

```csharp
int operator *(int x, int y);
uint operator *(uint x, uint y);
long operator *(long x, long y);
ulong operator *(ulong x, ulong y);
float operator *(float x, float y);
double operator *(double x, double y);
decimal operator *(decimal x, decimal y);
```

<span data-ttu-id="abcc0-342">當多載解析規則 ([多載解析](expressions.md#overload-resolution)) 會套用至這個集合的運算子，就可以為選取有隱含轉換運算子的第一個運算元類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-342">When overload resolution rules ([Overload resolution](expressions.md#overload-resolution)) are applied to this set of operators, the effect is to select the first of the operators for which implicit conversions exist from the operand types.</span></span> <span data-ttu-id="abcc0-343">例如，對於作業`b * s`，其中`b`是`byte`並`s`是`short`，多載解析會選取`operator *(int,int)`做最佳的運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-343">For example, for the operation `b * s`, where `b` is a `byte` and `s` is a `short`, overload resolution selects `operator *(int,int)` as the best operator.</span></span> <span data-ttu-id="abcc0-344">因此，結果會是，`b`並`s`轉換成`int`，且結果類型是`int`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-344">Thus, the effect is that `b` and `s` are converted to `int`, and the type of the result is `int`.</span></span> <span data-ttu-id="abcc0-345">同樣地，用於操作`i * d`，其中`i`是`int`並`d`是`double`，多載解析會選取`operator *(double,double)`做最佳的運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-345">Likewise, for the operation `i * d`, where `i` is an `int` and `d` is a `double`, overload resolution selects `operator *(double,double)` as the best operator.</span></span>

#### <a name="unary-numeric-promotions"></a><span data-ttu-id="abcc0-346">一元數值促銷活動</span><span class="sxs-lookup"><span data-stu-id="abcc0-346">Unary numeric promotions</span></span>

<span data-ttu-id="abcc0-347">一元數值升級，就會發生的預先定義的運算元`+`， `-`，和`~`一元運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-347">Unary numeric promotion occurs for the operands of the predefined `+`, `-`, and `~` unary operators.</span></span> <span data-ttu-id="abcc0-348">一元數值升級僅包含轉換類型的運算元`sbyte`， `byte`， `short`， `ushort`，或`char`輸入`int`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-348">Unary numeric promotion simply consists of converting operands of type `sbyte`, `byte`, `short`, `ushort`, or `char` to type `int`.</span></span> <span data-ttu-id="abcc0-349">此外，對於一元`-`運算子，一元 （unary） 數值升級會將轉換類型的運算元`uint`輸入`long`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-349">Additionally, for the unary `-` operator, unary numeric promotion converts operands of type `uint` to type `long`.</span></span>

#### <a name="binary-numeric-promotions"></a><span data-ttu-id="abcc0-350">二進位數字的促銷活動</span><span class="sxs-lookup"><span data-stu-id="abcc0-350">Binary numeric promotions</span></span>

<span data-ttu-id="abcc0-351">二進位數值升級，就會發生的預先定義的運算元`+`， `-`， `*`， `/`， `%`， `&`， `|`， `^`， `==`， `!=`，`>`， `<`， `>=`，和`<=`二元運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-351">Binary numeric promotion occurs for the operands of the predefined `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, `>`, `<`, `>=`, and `<=` binary operators.</span></span> <span data-ttu-id="abcc0-352">二進位數值升級會將兩個運算元隱含轉換成一般類型會發生的非關聯式運算子，也變得作業的結果類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-352">Binary numeric promotion implicitly converts both operands to a common type which, in case of the non-relational operators, also becomes the result type of the operation.</span></span> <span data-ttu-id="abcc0-353">二進位數字的促銷活動包含套用下列規則，會顯示在這裡的順序：</span><span class="sxs-lookup"><span data-stu-id="abcc0-353">Binary numeric promotion consists of applying the following rules, in the order they appear here:</span></span>

*  <span data-ttu-id="abcc0-354">如果任一個運算元的類型`decimal`，另一個運算元會轉換成類型`decimal`，或如果另一個運算元的類型，就會發生繫結時間錯誤`float`或`double`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-354">If either operand is of type `decimal`, the other operand is converted to type `decimal`, or a binding-time error occurs if the other operand is of type `float` or `double`.</span></span>
*  <span data-ttu-id="abcc0-355">否則，如果任一個運算元是型別的`double`，另一個運算元會轉換成類型`double`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-355">Otherwise, if either operand is of type `double`, the other operand is converted to type `double`.</span></span>
*  <span data-ttu-id="abcc0-356">否則，如果任一個運算元是型別的`float`，另一個運算元會轉換成類型`float`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-356">Otherwise, if either operand is of type `float`, the other operand is converted to type `float`.</span></span>
*  <span data-ttu-id="abcc0-357">否則，如果任一個運算元是型別的`ulong`，另一個運算元會轉換成類型`ulong`，或如果另一個運算元的類型，就會發生繫結時間錯誤`sbyte`， `short`， `int`，或`long`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-357">Otherwise, if either operand is of type `ulong`, the other operand is converted to type `ulong`, or a binding-time error occurs if the other operand is of type `sbyte`, `short`, `int`, or `long`.</span></span>
*  <span data-ttu-id="abcc0-358">否則，如果任一個運算元是型別的`long`，另一個運算元會轉換成類型`long`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-358">Otherwise, if either operand is of type `long`, the other operand is converted to type `long`.</span></span>
*  <span data-ttu-id="abcc0-359">否則，如果任一個運算元是型別的`uint`以及另一個運算元為類型`sbyte`， `short`，或`int`，這兩個運算元都轉換成輸入`long`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-359">Otherwise, if either operand is of type `uint` and the other operand is of type `sbyte`, `short`, or `int`, both operands are converted to type `long`.</span></span>
*  <span data-ttu-id="abcc0-360">否則，如果任一個運算元是型別的`uint`，另一個運算元會轉換成類型`uint`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-360">Otherwise, if either operand is of type `uint`, the other operand is converted to type `uint`.</span></span>
*  <span data-ttu-id="abcc0-361">否則，這兩個運算元會轉換成輸入`int`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-361">Otherwise, both operands are converted to type `int`.</span></span>

<span data-ttu-id="abcc0-362">第一個規則不允許混用任何作業的附註`decimal`型別與`double`和`float`型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-362">Note that the first rule disallows any operations that mix the `decimal` type with the `double` and `float` types.</span></span> <span data-ttu-id="abcc0-363">此規則的事實之間沒有隱含轉換，會遵循`decimal`型別和`double`和`float`類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-363">The rule follows from the fact that there are no implicit conversions between the `decimal` type and the `double` and `float` types.</span></span>

<span data-ttu-id="abcc0-364">也請注意，不可以是類型的運算元`ulong`當另一個運算元是帶正負號的整數類資料類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-364">Also note that it is not possible for an operand to be of type `ulong` when the other operand is of a signed integral type.</span></span> <span data-ttu-id="abcc0-365">原因是任何整數類資料類型是否存在，可以代表各種`ulong`以及帶正負號的整數類資料類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-365">The reason is that no integral type exists that can represent the full range of `ulong` as well as the signed integral types.</span></span>

<span data-ttu-id="abcc0-366">在兩個上述所有情況下，轉型運算式可用來明確地將一個運算元轉換成相容於另一個運算元的類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-366">In both of the above cases, a cast expression can be used to explicitly convert one operand to a type that is compatible with the other operand.</span></span>

<span data-ttu-id="abcc0-367">在範例</span><span class="sxs-lookup"><span data-stu-id="abcc0-367">In the example</span></span>
```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (1.0 + percent / 100.0);
}
```
<span data-ttu-id="abcc0-368">發生繫結時間錯誤的原因`decimal`不能乘以`double`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-368">a binding-time error occurs because a `decimal` cannot be multiplied by a `double`.</span></span> <span data-ttu-id="abcc0-369">明確的轉換的第二個運算元可解決此錯誤`decimal`、，如下所示：</span><span class="sxs-lookup"><span data-stu-id="abcc0-369">The error is resolved by explicitly converting the second operand to `decimal`, as follows:</span></span>

```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (decimal)(1.0 + percent / 100.0);
}
```

### <a name="lifted-operators"></a><span data-ttu-id="abcc0-370">提昇的運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-370">Lifted operators</span></span>

<span data-ttu-id="abcc0-371">***運算子會消除***允許對不可為 null 的實值型別也可搭配可為 null 的表單，這些類型的預先定義和使用者定義運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-371">***Lifted operators*** permit predefined and user-defined operators that operate on non-nullable value types to also be used with nullable forms of those types.</span></span> <span data-ttu-id="abcc0-372">提昇的運算子建構從預先定義和使用者定義的運算子，以符合特定需求，如下列所述：</span><span class="sxs-lookup"><span data-stu-id="abcc0-372">Lifted operators are constructed from predefined and user-defined operators that meet certain requirements, as described in the following:</span></span>

*   <span data-ttu-id="abcc0-373">針對一元運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-373">For the unary operators</span></span>

    ```csharp
    +  ++  -  --  !  ~
    ```

    <span data-ttu-id="abcc0-374">運算子的消除的形式存在，如果運算元和結果的型別是這兩個不可為 null 的實值型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-374">a lifted form of an operator exists if the operand and result types are both non-nullable value types.</span></span> <span data-ttu-id="abcc0-375">提昇形式是藉由將單一`?`運算元和結果的類型修飾詞。</span><span class="sxs-lookup"><span data-stu-id="abcc0-375">The lifted form is constructed by adding a single `?` modifier to the operand and result types.</span></span> <span data-ttu-id="abcc0-376">如果運算元是 null，提昇的運算子會產生 null 值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-376">The lifted operator produces a null value if the operand is null.</span></span> <span data-ttu-id="abcc0-377">否則為提昇的運算子，運算元會解除包裝成為、 適用於基礎的運算子，並將結果包裝。</span><span class="sxs-lookup"><span data-stu-id="abcc0-377">Otherwise, the lifted operator unwraps the operand, applies the underlying operator, and wraps the result.</span></span>

*   <span data-ttu-id="abcc0-378">以二元運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-378">For the binary operators</span></span>

    ```csharp
    +  -  *  /  %  &  |  ^  <<  >>
    ```

    <span data-ttu-id="abcc0-379">運算子的消除的形式存在，如果運算元和結果的類型是所有不可為 null 的實值型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-379">a lifted form of an operator exists if the operand and result types are all non-nullable value types.</span></span> <span data-ttu-id="abcc0-380">提昇形式是藉由將單一`?`每個運算元和結果的類型修飾詞。</span><span class="sxs-lookup"><span data-stu-id="abcc0-380">The lifted form is constructed by adding a single `?` modifier to each operand and result type.</span></span> <span data-ttu-id="abcc0-381">提昇的運算子會產生 null 值，如果有一個或兩個運算元都是 null (例外狀況正在`&`並`|`操作員`bool?`類型，如中所述[布林邏輯運算子](expressions.md#boolean-logical-operators))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-381">The lifted operator produces a null value if one or both operands are null (an exception being the `&` and `|` operators of the `bool?` type, as described in [Boolean logical operators](expressions.md#boolean-logical-operators)).</span></span> <span data-ttu-id="abcc0-382">否則為提昇的運算子，運算元會解除包裝成為、 適用於基礎的運算子，並將結果包裝。</span><span class="sxs-lookup"><span data-stu-id="abcc0-382">Otherwise, the lifted operator unwraps the operands, applies the underlying operator, and wraps the result.</span></span>

*   <span data-ttu-id="abcc0-383">等號比較運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-383">For the equality operators</span></span>

    ```csharp
    ==  !=
    ```

    <span data-ttu-id="abcc0-384">運算子的消除的形式存在，如果運算元類型不可為 null 的實值型別和結果型別是否`bool`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-384">a lifted form of an operator exists if the operand types are both non-nullable value types and if the result type is `bool`.</span></span> <span data-ttu-id="abcc0-385">提昇形式是藉由將單一`?`每個運算元的類型修飾詞。</span><span class="sxs-lookup"><span data-stu-id="abcc0-385">The lifted form is constructed by adding a single `?` modifier to each operand type.</span></span> <span data-ttu-id="abcc0-386">提昇的運算子會視為兩個 null 值相等，而且 null 值給任何非 null 值不相等。</span><span class="sxs-lookup"><span data-stu-id="abcc0-386">The lifted operator considers two null values equal, and a null value unequal to any non-null value.</span></span> <span data-ttu-id="abcc0-387">如果兩個運算元都為非 null，提昇的運算子會解除包裝成為運算元和適用於基礎的運算子，以產生`bool`結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-387">If both operands are non-null, the lifted operator unwraps the operands and applies the underlying operator to produce the `bool` result.</span></span>

*   <span data-ttu-id="abcc0-388">針對關係運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-388">For the relational operators</span></span>

    ```csharp
    <  >  <=  >=
    ```

    <span data-ttu-id="abcc0-389">運算子的消除的形式存在，如果運算元類型不可為 null 的實值型別和結果型別是否`bool`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-389">a lifted form of an operator exists if the operand types are both non-nullable value types and if the result type is `bool`.</span></span> <span data-ttu-id="abcc0-390">提昇形式是藉由將單一`?`每個運算元的類型修飾詞。</span><span class="sxs-lookup"><span data-stu-id="abcc0-390">The lifted form is constructed by adding a single `?` modifier to each operand type.</span></span> <span data-ttu-id="abcc0-391">提昇的運算子會產生值`false`如果一個或兩個運算元都是 null。</span><span class="sxs-lookup"><span data-stu-id="abcc0-391">The lifted operator produces the value `false` if one or both operands are null.</span></span> <span data-ttu-id="abcc0-392">提昇的運算子，否則會解除包裝成為運算元和適用於基礎的運算子，以產生`bool`結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-392">Otherwise, the lifted operator unwraps the operands and applies the underlying operator to produce the `bool` result.</span></span>

## <a name="member-lookup"></a><span data-ttu-id="abcc0-393">成員查詢</span><span class="sxs-lookup"><span data-stu-id="abcc0-393">Member lookup</span></span>

<span data-ttu-id="abcc0-394">成員查詢是名稱的藉此類型的內容中的意義取決於程序。</span><span class="sxs-lookup"><span data-stu-id="abcc0-394">A member lookup is the process whereby the meaning of a name in the context of a type is determined.</span></span> <span data-ttu-id="abcc0-395">成員查詢可能會發生評估的一部分*simple_name* ([簡單名稱](expressions.md#simple-names)) 或*member_access* ([成員存取](expressions.md#member-access)) 中運算式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-395">A member lookup can occur as part of evaluating a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)) in an expression.</span></span> <span data-ttu-id="abcc0-396">如果*simple_name*或是*member_access*當做*primary_expression*的*invocation_expression* ([方法引動過程](expressions.md#method-invocations))，該成員就會叫用。</span><span class="sxs-lookup"><span data-stu-id="abcc0-396">If the *simple_name* or *member_access* occurs as the *primary_expression* of an *invocation_expression* ([Method invocations](expressions.md#method-invocations)), the member is said to be invoked.</span></span>

<span data-ttu-id="abcc0-397">如果成員是方法或事件，或者它是常數、 欄位或屬性的委派型別 ([委派](delegates.md)) 或型別`dynamic`([動態型別](types.md#the-dynamic-type))，則該成員就是*invocable*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-397">If a member is a method or event, or if it is a constant, field or property of either a delegate type ([Delegates](delegates.md)) or the type `dynamic` ([The dynamic type](types.md#the-dynamic-type)), then the member is said to be *invocable*.</span></span>

<span data-ttu-id="abcc0-398">成員查詢會視為不只有名稱的成員，但成員的型別參數和成員是否可存取的數目。</span><span class="sxs-lookup"><span data-stu-id="abcc0-398">Member lookup considers not only the name of a member but also the number of type parameters the member has and whether the member is accessible.</span></span> <span data-ttu-id="abcc0-399">成員查詢而言泛型方法，而巢狀泛型類型有其各自的宣告所示的型別參數的數目，以及所有其他成員都有零個類型參數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-399">For the purposes of member lookup, generic methods and nested generic types have the number of type parameters indicated in their respective declarations and all other members have zero type parameters.</span></span>

<span data-ttu-id="abcc0-400">成員名稱的查閱 `N`具有`K` 型別參數的型別中 `T`處理，如下所示：</span><span class="sxs-lookup"><span data-stu-id="abcc0-400">A member lookup of a name `N` with `K` type parameters in a type `T` is processed as follows:</span></span>

*  <span data-ttu-id="abcc0-401">首先，將可存取的成員命名 `N`取決於：</span><span class="sxs-lookup"><span data-stu-id="abcc0-401">First, a set of accessible members named `N` is determined:</span></span>
    * <span data-ttu-id="abcc0-402">如果`T`是型別參數，則集合是可存取的成員命名集的聯集 `N`中每個類型指定為主要的條件約束或次要的條件約束 ([類型參數條件約束](classes.md#type-parameter-constraints)) 的 `T`，以及可存取的成員命名集 `N`在`object`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-402">If `T` is a type parameter, then the set is the union of the sets of accessible members named `N` in each of the types specified as a primary constraint or secondary constraint ([Type parameter constraints](classes.md#type-parameter-constraints)) for `T`, along with the set of accessible members named `N` in `object`.</span></span>
    * <span data-ttu-id="abcc0-403">否則集合，包含所有可存取的 ([成員存取](basic-concepts.md#member-access)) 成員命名 `N`中 `T`，包括繼承的成員，以及可存取的成員命名 `N`在`object`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-403">Otherwise, the set consists of all accessible ([Member access](basic-concepts.md#member-access)) members named `N` in `T`, including inherited members and the accessible members named `N` in `object`.</span></span> <span data-ttu-id="abcc0-404">如果`T`是建構的型別，取得之成員的集合中所述，以替代型別引數[建構的型別成員](classes.md#members-of-constructed-types)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-404">If `T` is a constructed type, the set of members is obtained by substituting type arguments as described in [Members of constructed types](classes.md#members-of-constructed-types).</span></span> <span data-ttu-id="abcc0-405">其成員包括`override`修飾詞會集中排除。</span><span class="sxs-lookup"><span data-stu-id="abcc0-405">Members that include an `override` modifier are excluded from the set.</span></span>
*  <span data-ttu-id="abcc0-406">接下來，如果`K`為零，所有巢狀中移除其宣告包含型別參數的型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-406">Next, if `K` is zero, all nested types whose declarations include type parameters are removed.</span></span> <span data-ttu-id="abcc0-407">如果`K`不是零，具有不同數目的型別參數會移除所有成員。</span><span class="sxs-lookup"><span data-stu-id="abcc0-407">If `K` is not zero, all members with a different number of type parameters are removed.</span></span> <span data-ttu-id="abcc0-408">請注意，當`K`為零，方法有的類型參數不會移除，因為型別推斷程序 ([型別推斷](expressions.md#type-inference)) 或許能夠推斷類型引數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-408">Note that when `K` is zero, methods having type parameters are not removed, since the type inference process ([Type inference](expressions.md#type-inference)) might be able to infer the type arguments.</span></span>
*  <span data-ttu-id="abcc0-409">下一步，如果成員是*叫用*中，所有非-*invocable*成員會從集合中移除。</span><span class="sxs-lookup"><span data-stu-id="abcc0-409">Next, if the member is *invoked*, all non-*invocable* members are removed from the set.</span></span>
*  <span data-ttu-id="abcc0-410">接下來，會隱藏其他成員的成員會從集合移除。</span><span class="sxs-lookup"><span data-stu-id="abcc0-410">Next, members that are hidden by other members are removed from the set.</span></span> <span data-ttu-id="abcc0-411">每位成員`S.M`集中，其中`S`是在其中的類型成員 `M`宣告，會套用下列規則：</span><span class="sxs-lookup"><span data-stu-id="abcc0-411">For every member `S.M` in the set, where `S` is the type in which the member `M` is declared, the following rules are applied:</span></span>
    * <span data-ttu-id="abcc0-412">如果`M`是常數、 欄位、 屬性、 事件或列舉成員，則基底類型中宣告的所有成員`S`從集合中移除。</span><span class="sxs-lookup"><span data-stu-id="abcc0-412">If `M` is a constant, field, property, event, or enumeration member, then all members declared in a base type of `S` are removed from the set.</span></span>
    * <span data-ttu-id="abcc0-413">如果`M`是型別宣告，則所有非類型的基底類型中宣告`S`會從集合中移除所有輸入具有相同數目的型別參數宣告`M`基底類型中宣告`S`會移除從集合中。</span><span class="sxs-lookup"><span data-stu-id="abcc0-413">If `M` is a type declaration, then all non-types declared in a base type of `S` are removed from the set, and all type declarations with the same number of type parameters as `M` declared in a base type of `S` are removed from the set.</span></span>
    * <span data-ttu-id="abcc0-414">如果`M`是一種方法，則基底類型中宣告的所有非方法成員`S`從集合中移除。</span><span class="sxs-lookup"><span data-stu-id="abcc0-414">If `M` is a method, then all non-method members declared in a base type of `S` are removed from the set.</span></span>
*  <span data-ttu-id="abcc0-415">接下來，會隱藏類別成員的介面成員會從集合移除。</span><span class="sxs-lookup"><span data-stu-id="abcc0-415">Next, interface members that are hidden by class members are removed from the set.</span></span> <span data-ttu-id="abcc0-416">此步驟只會有作用`T`是型別參數和`T`以外的其他具有這兩個的有效基底類別`object`和設定的非空白有效的介面 ([類型參數條件約束](classes.md#type-parameter-constraints))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-416">This step only has an effect if `T` is a type parameter and `T` has both an effective base class other than `object` and a non-empty effective interface set ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="abcc0-417">每位成員`S.M`集中，其中`S`是在其中的類型成員`M`宣告，下列規則適用於`S`而不是類別宣告`object`:</span><span class="sxs-lookup"><span data-stu-id="abcc0-417">For every member `S.M` in the set, where `S` is the type in which the member `M` is declared, the following rules are applied if `S` is a class declaration other than `object`:</span></span>
    * <span data-ttu-id="abcc0-418">如果`M`是常數、 欄位、 屬性、 事件、 列舉成員或型別宣告，則會從集合中移除所有在 interface 宣告中宣告的成員。</span><span class="sxs-lookup"><span data-stu-id="abcc0-418">If `M` is a constant, field, property, event, enumeration member, or type declaration, then all members declared in an interface declaration are removed from the set.</span></span>
    * <span data-ttu-id="abcc0-419">如果`M`是一種方法，則在 interface 宣告中宣告的所有非方法成員集合，與具有相同的簽章的所有方法中移除`M`宣告介面中宣告從集合中移除。</span><span class="sxs-lookup"><span data-stu-id="abcc0-419">If `M` is a method, then all non-method members declared in an interface declaration are removed from the set, and all methods with the same signature as `M` declared in an interface declaration are removed from the set.</span></span>
*  <span data-ttu-id="abcc0-420">最後，移除隱藏的成員，該查詢的結果決定：</span><span class="sxs-lookup"><span data-stu-id="abcc0-420">Finally, having removed hidden members, the result of the lookup is determined:</span></span>
    * <span data-ttu-id="abcc0-421">如果集合包含不是方法的單一成員，這個成員是該查詢的結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-421">If the set consists of a single member that is not a method, then this member is the result of the lookup.</span></span>
    * <span data-ttu-id="abcc0-422">否則，如果集合包含只有方法，然後這群方法就會是該查詢的結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-422">Otherwise, if the set contains only methods, then this group of methods is the result of the lookup.</span></span>
    * <span data-ttu-id="abcc0-423">否則，查閱模稜兩可，並在繫結階段錯誤發生。</span><span class="sxs-lookup"><span data-stu-id="abcc0-423">Otherwise, the lookup is ambiguous, and a binding-time error occurs.</span></span>

<span data-ttu-id="abcc0-424">型別參數和介面，以外的型別中的成員查閱和嚴格的單一繼承的介面中的成員查閱 （繼承鏈結中的每個介面具有剛好零個或一個直接基底介面），查閱規則的效果只是指衍生成員隱藏基底成員具有相同的名稱或簽章。</span><span class="sxs-lookup"><span data-stu-id="abcc0-424">For member lookups in types other than type parameters and interfaces, and member lookups in interfaces that are strictly single-inheritance (each interface in the inheritance chain has exactly zero or one direct base interface), the effect of the lookup rules is simply that derived members hide base members with the same name or signature.</span></span> <span data-ttu-id="abcc0-425">這種單一繼承查閱是永遠不會模稜兩可。</span><span class="sxs-lookup"><span data-stu-id="abcc0-425">Such single-inheritance lookups are never ambiguous.</span></span> <span data-ttu-id="abcc0-426">模稜兩可可能造成多個繼承介面成員查閱中所述[介面成員存取](interfaces.md#interface-member-access)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-426">The ambiguities that can possibly arise from member lookups in multiple-inheritance interfaces are described in [Interface member access](interfaces.md#interface-member-access).</span></span>

### <a name="base-types"></a><span data-ttu-id="abcc0-427">基底類型</span><span class="sxs-lookup"><span data-stu-id="abcc0-427">Base types</span></span>

<span data-ttu-id="abcc0-428">為了使用成員查詢，為型別`T`被視為具有下列基底類型：</span><span class="sxs-lookup"><span data-stu-id="abcc0-428">For purposes of member lookup, a type `T` is considered to have the following base types:</span></span>

*  <span data-ttu-id="abcc0-429">如果`T`已`object`，然後`T`沒有基底類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-429">If `T` is `object`, then `T` has no base type.</span></span>
*  <span data-ttu-id="abcc0-430">如果`T`是*enum_type*，基底型別的`T`是類別型別`System.Enum`， `System.ValueType`，和`object`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-430">If `T` is an *enum_type*, the base types of `T` are the class types `System.Enum`, `System.ValueType`, and `object`.</span></span>
*  <span data-ttu-id="abcc0-431">如果`T`是*struct_type*，基底型別的`T`是類別型別`System.ValueType`和`object`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-431">If `T` is a *struct_type*, the base types of `T` are the class types `System.ValueType` and `object`.</span></span>
*  <span data-ttu-id="abcc0-432">如果`T`是*class_type*，基底型別的`T`的基底類別`T`，包括的類別型別`object`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-432">If `T` is a *class_type*, the base types of `T` are the base classes of `T`, including the class type `object`.</span></span>
*  <span data-ttu-id="abcc0-433">如果`T`是*interface_type*，基底型別的`T`的基底介面`T`類別型別和`object`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-433">If `T` is an *interface_type*, the base types of `T` are the base interfaces of `T` and the class type `object`.</span></span>
*  <span data-ttu-id="abcc0-434">如果`T`是*array_type*，基底型別的`T`是類別型別`System.Array`和`object`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-434">If `T` is an *array_type*, the base types of `T` are the class types `System.Array` and `object`.</span></span>
*  <span data-ttu-id="abcc0-435">如果`T`是*delegate_type*，基底型別的`T`是類別型別`System.Delegate`和`object`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-435">If `T` is a *delegate_type*, the base types of `T` are the class types `System.Delegate` and `object`.</span></span>

## <a name="function-members"></a><span data-ttu-id="abcc0-436">函式成員</span><span class="sxs-lookup"><span data-stu-id="abcc0-436">Function members</span></span>

<span data-ttu-id="abcc0-437">函式成員是包含可執行的陳述式的成員。</span><span class="sxs-lookup"><span data-stu-id="abcc0-437">Function members are members that contain executable statements.</span></span> <span data-ttu-id="abcc0-438">函式成員皆為型別的成員，而且不能命名空間的成員。</span><span class="sxs-lookup"><span data-stu-id="abcc0-438">Function members are always members of types and cannot be members of namespaces.</span></span> <span data-ttu-id="abcc0-439">C# 定義函式成員的下列的類別：</span><span class="sxs-lookup"><span data-stu-id="abcc0-439">C# defines the following categories of function members:</span></span>

*  <span data-ttu-id="abcc0-440">方法</span><span class="sxs-lookup"><span data-stu-id="abcc0-440">Methods</span></span>
*  <span data-ttu-id="abcc0-441">屬性</span><span class="sxs-lookup"><span data-stu-id="abcc0-441">Properties</span></span>
*  <span data-ttu-id="abcc0-442">事件</span><span class="sxs-lookup"><span data-stu-id="abcc0-442">Events</span></span>
*  <span data-ttu-id="abcc0-443">索引子</span><span class="sxs-lookup"><span data-stu-id="abcc0-443">Indexers</span></span>
*  <span data-ttu-id="abcc0-444">使用者定義的運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-444">User-defined operators</span></span>
*  <span data-ttu-id="abcc0-445">執行個體建構函式</span><span class="sxs-lookup"><span data-stu-id="abcc0-445">Instance constructors</span></span>
*  <span data-ttu-id="abcc0-446">靜態建構函式</span><span class="sxs-lookup"><span data-stu-id="abcc0-446">Static constructors</span></span>
*  <span data-ttu-id="abcc0-447">解構函式</span><span class="sxs-lookup"><span data-stu-id="abcc0-447">Destructors</span></span>

<span data-ttu-id="abcc0-448">除了解構函式和靜態建構函式 （這無法明確地叫用），函式成員中包含的陳述式會執行透過函式成員引動過程。</span><span class="sxs-lookup"><span data-stu-id="abcc0-448">Except for destructors and static constructors (which cannot be invoked explicitly), the statements contained in function members are executed through function member invocations.</span></span> <span data-ttu-id="abcc0-449">實際的語法撰寫的函式成員引動過程取決於特定函式成員分類。</span><span class="sxs-lookup"><span data-stu-id="abcc0-449">The actual syntax for writing a function member invocation depends on the particular function member category.</span></span>

<span data-ttu-id="abcc0-450">引數清單 ([引數清單](expressions.md#argument-lists)) 函式成員的引動過程提供實際的值或變數參考參數的函式成員。</span><span class="sxs-lookup"><span data-stu-id="abcc0-450">The argument list ([Argument lists](expressions.md#argument-lists)) of a function member invocation provides actual values or variable references for the parameters of the function member.</span></span>

<span data-ttu-id="abcc0-451">泛型方法的引動過程可能會採用型別推斷來判斷要傳遞至方法的型別引數的集合。</span><span class="sxs-lookup"><span data-stu-id="abcc0-451">Invocations of generic methods may employ type inference to determine the set of type arguments to pass to the method.</span></span> <span data-ttu-id="abcc0-452">此程序所述[型別推斷](expressions.md#type-inference)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-452">This process is described in [Type inference](expressions.md#type-inference).</span></span>

<span data-ttu-id="abcc0-453">引動過程的方法、 索引子、 運算子和執行個體建構函式會採用多載解析，以決定哪一組候選函式成員叫用。</span><span class="sxs-lookup"><span data-stu-id="abcc0-453">Invocations of methods, indexers, operators and instance constructors employ overload resolution to determine which of a candidate set of function members to invoke.</span></span> <span data-ttu-id="abcc0-454">此程序所述[多載解析](expressions.md#overload-resolution)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-454">This process is described in [Overload resolution](expressions.md#overload-resolution).</span></span>

<span data-ttu-id="abcc0-455">識別特定函式成員繫結時間之後, 可能是透過多載解析，叫用函式成員的實際執行階段程序所述[編譯時期檢查動態的多載解析](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="abcc0-455">Once a particular function member has been identified at binding-time, possibly through overload resolution, the actual run-time process of invoking the function member is described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="abcc0-456">下表摘要說明的處理會涉及可以明確叫用的函式成員的六個類別的建構中的位置。</span><span class="sxs-lookup"><span data-stu-id="abcc0-456">The following table summarizes the processing that takes place in constructs involving the six categories of function members that can be explicitly invoked.</span></span> <span data-ttu-id="abcc0-457">在資料表中， `e`， `x`， `y`，和`value`表示分類為變數或值的運算式`T`指出運算式分類為型別，`F`是簡單的方法，而名稱`P`是簡單的屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="abcc0-457">In the table, `e`, `x`, `y`, and `value` indicate expressions classified as variables or values, `T` indicates an expression classified as a type, `F` is the simple name of a method, and `P` is the simple name of a property.</span></span>


| <span data-ttu-id="abcc0-458">__建構__</span><span class="sxs-lookup"><span data-stu-id="abcc0-458">__Construct__</span></span>     | <span data-ttu-id="abcc0-459">__範例__</span><span class="sxs-lookup"><span data-stu-id="abcc0-459">__Example__</span></span>    | <span data-ttu-id="abcc0-460">__描述__</span><span class="sxs-lookup"><span data-stu-id="abcc0-460">__Description__</span></span> |
|-------------------|----------------|-----------------|
| <span data-ttu-id="abcc0-461">方法引動過程</span><span class="sxs-lookup"><span data-stu-id="abcc0-461">Method invocation</span></span> | `F(x,y)`       | <span data-ttu-id="abcc0-462">多載解析會選取最佳的方式套用`F`中包含的類別或結構。</span><span class="sxs-lookup"><span data-stu-id="abcc0-462">Overload resolution is applied to select the best method `F` in the containing class or struct.</span></span> <span data-ttu-id="abcc0-463">使用引數清單叫用方法`(x,y)`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-463">The method is invoked with the argument list `(x,y)`.</span></span> <span data-ttu-id="abcc0-464">如果方法不是`static`，此執行個體運算式`this`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-464">If the method is not `static`, the instance expression is `this`.</span></span> | 
|                   | `T.F(x,y)`     | <span data-ttu-id="abcc0-465">多載解析會選取最佳的方式套用`F`類別或結構中`T`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-465">Overload resolution is applied to select the best method `F` in the class or struct `T`.</span></span> <span data-ttu-id="abcc0-466">如果方法不是，就會發生的繫結時間錯誤`static`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-466">A binding-time error occurs if the method is not `static`.</span></span> <span data-ttu-id="abcc0-467">使用引數清單叫用方法`(x,y)`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-467">The method is invoked with the argument list `(x,y)`.</span></span> | 
|                   | `e.F(x,y)`     | <span data-ttu-id="abcc0-468">多載解析套用至類別、 結構或介面的型別所指定在選取最佳的方法 F `e`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-468">Overload resolution is applied to select the best method F in the class, struct, or interface given by the type of `e`.</span></span> <span data-ttu-id="abcc0-469">如果方法是，就會發生繫結時間錯誤`static`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-469">A binding-time error occurs if the method is `static`.</span></span> <span data-ttu-id="abcc0-470">方法會叫用執行個體運算式`e`和引數清單`(x,y)`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-470">The method is invoked with the instance expression `e` and the argument list `(x,y)`.</span></span> | 
| <span data-ttu-id="abcc0-471">屬性存取</span><span class="sxs-lookup"><span data-stu-id="abcc0-471">Property access</span></span>   | `P`            | <span data-ttu-id="abcc0-472">`get`屬性存取子`P`中包含的類別或結構會叫用。</span><span class="sxs-lookup"><span data-stu-id="abcc0-472">The `get` accessor of the property `P` in the containing class or struct is invoked.</span></span> <span data-ttu-id="abcc0-473">如果，就會發生編譯時期錯誤`P`是唯寫。</span><span class="sxs-lookup"><span data-stu-id="abcc0-473">A compile-time error occurs if `P` is write-only.</span></span> <span data-ttu-id="abcc0-474">如果`P`不是`static`，此執行個體運算式`this`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-474">If `P` is not `static`, the instance expression is `this`.</span></span> | 
|                   | `P = value`    | <span data-ttu-id="abcc0-475">`set`屬性存取子`P`中包含的類別或結構會叫用具有引數清單`(value)`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-475">The `set` accessor of the property `P` in the containing class or struct is invoked with the argument list `(value)`.</span></span> <span data-ttu-id="abcc0-476">如果，就會發生編譯時期錯誤`P`處於唯讀狀態。</span><span class="sxs-lookup"><span data-stu-id="abcc0-476">A compile-time error occurs if `P` is read-only.</span></span> <span data-ttu-id="abcc0-477">如果`P`不是`static`，此執行個體運算式`this`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-477">If `P` is not `static`, the instance expression is `this`.</span></span> | 
|                   | `T.P`          | <span data-ttu-id="abcc0-478">`get`屬性存取子`P`類別或結構中`T`叫用。</span><span class="sxs-lookup"><span data-stu-id="abcc0-478">The `get` accessor of the property `P` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="abcc0-479">如果，就會發生編譯時期錯誤`P`不是`static`或者`P`是唯寫。</span><span class="sxs-lookup"><span data-stu-id="abcc0-479">A compile-time error occurs if `P` is not `static` or if `P` is write-only.</span></span> | 
|                   | `T.P = value`  | <span data-ttu-id="abcc0-480">`set`屬性存取子`P`類別或結構中`T`引數清單叫用`(value)`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-480">The `set` accessor of the property `P` in the class or struct `T` is invoked with the argument list `(value)`.</span></span> <span data-ttu-id="abcc0-481">如果，就會發生編譯時期錯誤`P`不是`static`或者`P`處於唯讀狀態。</span><span class="sxs-lookup"><span data-stu-id="abcc0-481">A compile-time error occurs if `P` is not `static` or if `P` is read-only.</span></span> | 
|                   | `e.P`          | <span data-ttu-id="abcc0-482">`get`屬性存取子`P`在類別、 結構或介面的型別所給定`e`叫用執行個體運算式`e`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-482">The `get` accessor of the property `P` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="abcc0-483">如果繫結時間錯誤就會發生`P`是`static`或者`P`是唯寫。</span><span class="sxs-lookup"><span data-stu-id="abcc0-483">A binding-time error occurs if `P` is `static` or if `P` is write-only.</span></span> | 
|                   | `e.P = value`  | <span data-ttu-id="abcc0-484">`set`屬性存取子`P`在類別、 結構或介面的型別所給定`e`叫用執行個體運算式`e`和引數清單`(value)`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-484">The `set` accessor of the property `P` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e` and the argument list `(value)`.</span></span> <span data-ttu-id="abcc0-485">如果繫結時間錯誤就會發生`P`是`static`或者`P`處於唯讀狀態。</span><span class="sxs-lookup"><span data-stu-id="abcc0-485">A binding-time error occurs if `P` is `static` or if `P` is read-only.</span></span> | 
| <span data-ttu-id="abcc0-486">事件存取</span><span class="sxs-lookup"><span data-stu-id="abcc0-486">Event access</span></span>      | `E += value`   | <span data-ttu-id="abcc0-487">`add`事件存取子`E`中包含的類別或結構會叫用。</span><span class="sxs-lookup"><span data-stu-id="abcc0-487">The `add` accessor of the event `E` in the containing class or struct is invoked.</span></span> <span data-ttu-id="abcc0-488">如果`E`為非靜態的該執行個體運算式是`this`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-488">If `E` is not static, the instance expression is `this`.</span></span> | 
|                   | `E -= value`   | <span data-ttu-id="abcc0-489">`remove`事件存取子`E`中包含的類別或結構會叫用。</span><span class="sxs-lookup"><span data-stu-id="abcc0-489">The `remove` accessor of the event `E` in the containing class or struct is invoked.</span></span> <span data-ttu-id="abcc0-490">如果`E`為非靜態的該執行個體運算式是`this`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-490">If `E` is not static, the instance expression is `this`.</span></span> | 
|                   | `T.E += value` | <span data-ttu-id="abcc0-491">`add`事件存取子`E`類別或結構中`T`叫用。</span><span class="sxs-lookup"><span data-stu-id="abcc0-491">The `add` accessor of the event `E` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="abcc0-492">若時期繫結錯誤`E`不是靜態。</span><span class="sxs-lookup"><span data-stu-id="abcc0-492">A binding-time error occurs if `E` is not static.</span></span> | 
|                   | `T.E -= value` | <span data-ttu-id="abcc0-493">`remove`事件存取子`E`類別或結構中`T`叫用。</span><span class="sxs-lookup"><span data-stu-id="abcc0-493">The `remove` accessor of the event `E` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="abcc0-494">若時期繫結錯誤`E`不是靜態。</span><span class="sxs-lookup"><span data-stu-id="abcc0-494">A binding-time error occurs if `E` is not static.</span></span> | 
|                   | `e.E += value` | <span data-ttu-id="abcc0-495">`add`事件存取子`E`在類別、 結構或介面的型別所給定`e`叫用執行個體運算式`e`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-495">The `add` accessor of the event `E` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="abcc0-496">若時期繫結錯誤`E`是靜態的。</span><span class="sxs-lookup"><span data-stu-id="abcc0-496">A binding-time error occurs if `E` is static.</span></span> | 
|                   | `e.E -= value` | <span data-ttu-id="abcc0-497">`remove`事件存取子`E`在類別、 結構或介面的型別所給定`e`叫用執行個體運算式`e`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-497">The `remove` accessor of the event `E` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="abcc0-498">若時期繫結錯誤`E`是靜態的。</span><span class="sxs-lookup"><span data-stu-id="abcc0-498">A binding-time error occurs if `E` is static.</span></span> | 
| <span data-ttu-id="abcc0-499">索引子存取</span><span class="sxs-lookup"><span data-stu-id="abcc0-499">Indexer access</span></span>    | `e[x,y]`       | <span data-ttu-id="abcc0-500">多載解析會套用至類別、 結構或 e 的型別所提供的介面中選取最佳的索引子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-500">Overload resolution is applied to select the best indexer in the class, struct, or interface given by the type of e.</span></span> <span data-ttu-id="abcc0-501">`get`的索引子的存取子會叫用執行個體運算式`e`和引數清單`(x,y)`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-501">The `get` accessor of the indexer is invoked with the instance expression `e` and the argument list `(x,y)`.</span></span> <span data-ttu-id="abcc0-502">如果這個索引子是唯寫，就會發生的繫結階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-502">A binding-time error occurs if the indexer is write-only.</span></span> | 
|                   | `e[x,y] = value` | <span data-ttu-id="abcc0-503">多載解析會套用至類別、 結構或介面的型別所指定在選取最佳的索引子`e`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-503">Overload resolution is applied to select the best indexer in the class, struct, or interface given by the type of `e`.</span></span> <span data-ttu-id="abcc0-504">`set`的索引子的存取子會叫用執行個體運算式`e`和引數清單`(x,y,value)`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-504">The `set` accessor of the indexer is invoked with the instance expression `e` and the argument list `(x,y,value)`.</span></span> <span data-ttu-id="abcc0-505">如果索引子會是唯讀的就會發生的繫結階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-505">A binding-time error occurs if the indexer is read-only.</span></span> | 
| <span data-ttu-id="abcc0-506">運算子的引動過程</span><span class="sxs-lookup"><span data-stu-id="abcc0-506">Operator invocation</span></span> | `-x`         | <span data-ttu-id="abcc0-507">多載解析會套用至類別或型別所指定的結構中選取最佳的一元運算子`x`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-507">Overload resolution is applied to select the best unary operator in the class or struct given by the type of `x`.</span></span> <span data-ttu-id="abcc0-508">選取的運算子會叫用具有引數清單`(x)`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-508">The selected operator is invoked with the argument list `(x)`.</span></span> | 
|                     | `x + y`      | <span data-ttu-id="abcc0-509">多載解析套用至類別或類型的所指定的結構中選取最佳的二元運算子`x`和`y`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-509">Overload resolution is applied to select the best binary operator in the classes or structs given by the types of `x` and `y`.</span></span> <span data-ttu-id="abcc0-510">選取的運算子會叫用具有引數清單`(x,y)`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-510">The selected operator is invoked with the argument list `(x,y)`.</span></span> | 
| <span data-ttu-id="abcc0-511">執行個體建構函式引動過程</span><span class="sxs-lookup"><span data-stu-id="abcc0-511">Instance constructor invocation</span></span> | `new T(x,y)` | <span data-ttu-id="abcc0-512">多載解析會套用至類別或結構中選取最佳的執行個體建構函式`T`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-512">Overload resolution is applied to select the best instance constructor in the class or struct `T`.</span></span> <span data-ttu-id="abcc0-513">執行個體建構函式會叫用具有引數清單`(x,y)`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-513">The instance constructor is invoked with the argument list `(x,y)`.</span></span> | 

### <a name="argument-lists"></a><span data-ttu-id="abcc0-514">引數清單</span><span class="sxs-lookup"><span data-stu-id="abcc0-514">Argument lists</span></span>

<span data-ttu-id="abcc0-515">每個函式成員和委派叫用包含可提供實際的值或變數參考參數的函式成員的引數清單。</span><span class="sxs-lookup"><span data-stu-id="abcc0-515">Every function member and delegate invocation includes an argument list which provides actual values or variable references for the parameters of the function member.</span></span> <span data-ttu-id="abcc0-516">指定的函式成員引動過程的引數清單的語法取決於函式成員分類：</span><span class="sxs-lookup"><span data-stu-id="abcc0-516">The syntax for specifying the argument list of a function member invocation depends on the function member category:</span></span>

*  <span data-ttu-id="abcc0-517">執行個體建構函式、 方法、 索引子和委派的引數會指定為*argument_list*，如下所述。</span><span class="sxs-lookup"><span data-stu-id="abcc0-517">For instance constructors, methods, indexers and delegates, the arguments are specified as an *argument_list*, as described below.</span></span> <span data-ttu-id="abcc0-518">針對索引子，叫用時`set`存取子的引數清單此外也包含為指派運算子的右運算元所指定的運算式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-518">For indexers, when invoking the `set` accessor, the argument list additionally includes the expression specified as the right operand of the assignment operator.</span></span>
*  <span data-ttu-id="abcc0-519">屬性，引數清單是空的時叫用`get`存取子，並叫用時，為指派運算子的右運算元指定的運算式所組成`set`存取子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-519">For properties, the argument list is empty when invoking the `get` accessor, and consists of the expression specified as the right operand of the assignment operator when invoking the `set` accessor.</span></span>
*  <span data-ttu-id="abcc0-520">事件引數清單所組成的右運算元為指定的運算式`+=`或`-=`運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-520">For events, the argument list consists of the expression specified as the right operand of the `+=` or `-=` operator.</span></span>
*  <span data-ttu-id="abcc0-521">使用者定義的運算子，引數清單包含單一運算子的運算元一元或二元運算子的兩個運算元。</span><span class="sxs-lookup"><span data-stu-id="abcc0-521">For user-defined operators, the argument list consists of the single operand of the unary operator or the two operands of the binary operator.</span></span>

<span data-ttu-id="abcc0-522">屬性的引數 ([屬性](classes.md#properties))，事件 ([事件](classes.md#events))，和使用者定義運算子 ([運算子](classes.md#operators)) 一定會傳遞做為值參數 ([值參數](classes.md#value-parameters))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-522">The arguments of properties ([Properties](classes.md#properties)), events ([Events](classes.md#events)), and user-defined operators ([Operators](classes.md#operators)) are always passed as value parameters ([Value parameters](classes.md#value-parameters)).</span></span> <span data-ttu-id="abcc0-523">索引子的引數 ([索引子](classes.md#indexers)) 一定會傳遞做為值參數 ([實值參數](classes.md#value-parameters)) 或參數陣列 ([參數陣列](classes.md#parameter-arrays))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-523">The arguments of indexers ([Indexers](classes.md#indexers)) are always passed as value parameters ([Value parameters](classes.md#value-parameters)) or parameter arrays ([Parameter arrays](classes.md#parameter-arrays)).</span></span> <span data-ttu-id="abcc0-524">這些函式成員的類別不支援的參考和輸出參數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-524">Reference and output parameters are not supported for these categories of function members.</span></span>

<span data-ttu-id="abcc0-525">執行個體建構函式、 方法、 索引子或委派引動過程的引數會指定為*argument_list*:</span><span class="sxs-lookup"><span data-stu-id="abcc0-525">The arguments of an instance constructor, method, indexer or delegate invocation are specified as an *argument_list*:</span></span>

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

<span data-ttu-id="abcc0-526">*Argument_list*包含一個或多個*引數*s，並以逗號分隔。</span><span class="sxs-lookup"><span data-stu-id="abcc0-526">An *argument_list* consists of one or more *argument*s, separated by commas.</span></span> <span data-ttu-id="abcc0-527">每個引數所組成的選擇性*數名稱*後面*argument_value*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-527">Each argument consists of an optional  *argument_name* followed by an *argument_value*.</span></span> <span data-ttu-id="abcc0-528">*引數*具有*數名稱*指***具名引數***，而*引數*沒有*數名稱*已***位置引數***。</span><span class="sxs-lookup"><span data-stu-id="abcc0-528">An *argument* with an *argument_name* is referred to as a ***named argument***, whereas an *argument* without an *argument_name* is a ***positional argument***.</span></span> <span data-ttu-id="abcc0-529">它是位置引數中的具名引數之後顯示的錯誤*argument_list*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-529">It is an error for a positional argument to appear after a named argument in an *argument_list*.</span></span>

<span data-ttu-id="abcc0-530">*Argument_value*可以採用下列格式之一：</span><span class="sxs-lookup"><span data-stu-id="abcc0-530">The *argument_value* can take one of the following forms:</span></span>

*  <span data-ttu-id="abcc0-531">*運算式*，表示引數會傳遞做為值參數 ([實值參數](classes.md#value-parameters))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-531">An *expression*, indicating that the argument is passed as a value parameter ([Value parameters](classes.md#value-parameters)).</span></span>
*  <span data-ttu-id="abcc0-532">關鍵字`ref`後面接著*variable_reference* ([變數參考](variables.md#variable-references))，表示引數會傳遞做為參考參數 ([參考參數](classes.md#reference-parameters)).</span><span class="sxs-lookup"><span data-stu-id="abcc0-532">The keyword `ref` followed by a *variable_reference* ([Variable references](variables.md#variable-references)), indicating that the argument is passed as a reference parameter ([Reference parameters](classes.md#reference-parameters)).</span></span> <span data-ttu-id="abcc0-533">變數必須明確進行指派 ([明確指派](variables.md#definite-assignment)) 將它傳遞做為參考參數之前。</span><span class="sxs-lookup"><span data-stu-id="abcc0-533">A variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before it can be passed as a reference parameter.</span></span> <span data-ttu-id="abcc0-534">關鍵字`out`後面接著*variable_reference* ([變數參考](variables.md#variable-references))，表示引數會傳遞做為輸出參數 ([輸出參數](classes.md#output-parameters)).</span><span class="sxs-lookup"><span data-stu-id="abcc0-534">The keyword `out` followed by a *variable_reference* ([Variable references](variables.md#variable-references)), indicating that the argument is passed as an output parameter ([Output parameters](classes.md#output-parameters)).</span></span> <span data-ttu-id="abcc0-535">變數會被視為已明確指派 ([明確指派](variables.md#definite-assignment)) 遵循變數被當做輸出參數的函式成員引動過程。</span><span class="sxs-lookup"><span data-stu-id="abcc0-535">A variable is considered definitely assigned ([Definite assignment](variables.md#definite-assignment)) following a function member invocation in which the variable is passed as an output parameter.</span></span>

#### <a name="corresponding-parameters"></a><span data-ttu-id="abcc0-536">對應的參數</span><span class="sxs-lookup"><span data-stu-id="abcc0-536">Corresponding parameters</span></span>

<span data-ttu-id="abcc0-537">每個引數的引數清單中應該有對應的參數所叫用委派的函式成員中。</span><span class="sxs-lookup"><span data-stu-id="abcc0-537">For each argument in an argument list there has to be a corresponding parameter in the function member or delegate being invoked.</span></span>

<span data-ttu-id="abcc0-538">使用下列的參數清單的判斷方式如下：</span><span class="sxs-lookup"><span data-stu-id="abcc0-538">The parameter list used in the following is determined as follows:</span></span>

*  <span data-ttu-id="abcc0-539">虛擬方法和類別中定義的索引子，參數清單會挑出最明確的宣告，或覆寫函式成員，開始的接收者，靜態類型，並搜尋其基底類別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-539">For virtual methods and indexers defined in classes, the parameter list is picked from the most specific declaration or override of the function member, starting with the static type of the receiver, and searching through its base classes.</span></span>
*  <span data-ttu-id="abcc0-540">介面方法和索引子，參數清單會挑出形成最明確的定義開頭的介面型別和基底介面中搜尋的成員。</span><span class="sxs-lookup"><span data-stu-id="abcc0-540">For interface methods and indexers, the parameter list is picked form the most specific definition of the member, starting with the interface type and searching through the base interfaces.</span></span> <span data-ttu-id="abcc0-541">如果沒有唯一的參數清單中找到，建構使用無法存取名稱以及任何選擇性參數的參數清單，以便引動過程不能使用具名的參數，或省略的選擇性引數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-541">If no unique parameter list is found, a parameter list with inaccessible names and no optional parameters is constructed, so that invocations cannot use named parameters or omit optional arguments.</span></span>
*  <span data-ttu-id="abcc0-542">部分方法，會使用定義的部分方法宣告的參數清單。</span><span class="sxs-lookup"><span data-stu-id="abcc0-542">For partial methods, the parameter list of the defining partial method declaration is used.</span></span>
*  <span data-ttu-id="abcc0-543">針對所有其他函式成員與委派沒有只有單一參數清單時，也就是使用一個。</span><span class="sxs-lookup"><span data-stu-id="abcc0-543">For all other function members and delegates there is only a single parameter list, which is the one used.</span></span>

<span data-ttu-id="abcc0-544">引數或參數的位置會定義為引數或引數清單或參數清單中在它前面的參數數目。</span><span class="sxs-lookup"><span data-stu-id="abcc0-544">The position of an argument or parameter is defined as the number of arguments or parameters preceding it in the argument list or parameter list.</span></span>

<span data-ttu-id="abcc0-545">建立函式成員的引數的對應參數，如下所示：</span><span class="sxs-lookup"><span data-stu-id="abcc0-545">The corresponding parameters for function member arguments are established as follows:</span></span>

*  <span data-ttu-id="abcc0-546">中的引數*argument_list*的執行個體建構函式、 方法、 索引子和委派：</span><span class="sxs-lookup"><span data-stu-id="abcc0-546">Arguments in the *argument_list* of instance constructors, methods, indexers and delegates:</span></span>
    * <span data-ttu-id="abcc0-547">固定的參數在參數清單中相同的位置發生的所在位置的引數會對應至該參數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-547">A positional argument where a fixed parameter occurs at the same position in the parameter list corresponds to that parameter.</span></span>
    * <span data-ttu-id="abcc0-548">含有參數陣列叫用的一般形式的函式成員的位置引數會對應至參數陣列，它必須出現在參數清單中的相同位置。</span><span class="sxs-lookup"><span data-stu-id="abcc0-548">A positional argument of a function member with a parameter array invoked in its normal form corresponds to the parameter  array, which must occur at the same position in the parameter list.</span></span>
    * <span data-ttu-id="abcc0-549">含有參數陣列叫用其展開的形式，其中沒有固定的參數就會發生在相同的位置參數清單中，函式成員的位置引數會對應至參數陣列中的項目。</span><span class="sxs-lookup"><span data-stu-id="abcc0-549">A positional argument of a function member with a parameter array invoked in its expanded form, where no fixed parameter occurs at the same position in the parameter list, corresponds to an element in the parameter array.</span></span>
    * <span data-ttu-id="abcc0-550">具名引數會對應至參數清單中的相同名稱的參數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-550">A named argument corresponds to the parameter of the same name in the parameter list.</span></span>
    * <span data-ttu-id="abcc0-551">針對索引子，叫用時`set`存取子，指定對應至隱含的指派運算子的右運算元，運算式`value`參數`set`存取子宣告。</span><span class="sxs-lookup"><span data-stu-id="abcc0-551">For indexers, when invoking the `set` accessor, the expression specified as the right operand of the assignment operator corresponds to the implicit `value` parameter of the `set` accessor declaration.</span></span>
*  <span data-ttu-id="abcc0-552">屬性，叫用時`get`引數，就有存取子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-552">For properties, when invoking the `get` accessor there are no arguments.</span></span> <span data-ttu-id="abcc0-553">當叫用`set`存取子，指定對應至隱含的指派運算子的右運算元，運算式`value`參數`set`存取子宣告。</span><span class="sxs-lookup"><span data-stu-id="abcc0-553">When invoking the `set` accessor, the expression specified as the right operand of the assignment operator corresponds to the implicit `value` parameter of the `set` accessor declaration.</span></span>
*  <span data-ttu-id="abcc0-554">使用者定義的一元運算子 （包括轉換），在單一運算元對應運算子宣告單一參數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-554">For user-defined unary operators (including conversions), the single operand corresponds to the single parameter of the operator declaration.</span></span>
*  <span data-ttu-id="abcc0-555">使用者定義的二元運算子的左的運算元對應到第一個參數，而右運算元對應運算子宣告的第二個參數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-555">For user-defined binary operators, the left operand corresponds to the first parameter, and the right operand corresponds to the second parameter of the operator declaration.</span></span>

#### <a name="run-time-evaluation-of-argument-lists"></a><span data-ttu-id="abcc0-556">執行階段評估的引數清單</span><span class="sxs-lookup"><span data-stu-id="abcc0-556">Run-time evaluation of argument lists</span></span>

<span data-ttu-id="abcc0-557">在執行階段的處理函式成員引動過程期間 ([編譯時期檢查動態的多載解析](expressions.md#compile-time-checking-of-dynamic-overload-resolution))，運算式或變數的參考，引數清單的評估順序，從左到右，做為如下所示：</span><span class="sxs-lookup"><span data-stu-id="abcc0-557">During the run-time processing of a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), the expressions or variable references of an argument list are evaluated in order, from left to right, as follows:</span></span>

*  <span data-ttu-id="abcc0-558">引數運算式會評估為值參數和隱含的轉換 ([隱含轉換](conversions.md#implicit-conversions)) 相對應的參數來執行類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-558">For a value parameter, the argument expression is evaluated and an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to the corresponding parameter type is performed.</span></span> <span data-ttu-id="abcc0-559">產生的值會變成函式成員引動過程中的 value 參數的初始值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-559">The resulting value becomes the initial value of the value parameter in the function member invocation.</span></span>
*  <span data-ttu-id="abcc0-560">參考或輸出參數，在評估變數的參考而產生的儲存位置會變成函式成員引動過程中的參數所表示的儲存位置。</span><span class="sxs-lookup"><span data-stu-id="abcc0-560">For a reference or output parameter, the variable reference is evaluated and the resulting storage location becomes the storage location represented by the parameter in the function member invocation.</span></span> <span data-ttu-id="abcc0-561">如果變數參考做為參考或輸出參數所指定之陣列元素*reference_type*，執行階段會執行檢查以確保陣列的元素型別參數的型別相同。</span><span class="sxs-lookup"><span data-stu-id="abcc0-561">If the variable reference given as a reference or output parameter is an array element of a *reference_type*, a run-time check is performed to ensure that the element type of the array is identical to the type of the parameter.</span></span> <span data-ttu-id="abcc0-562">如果這項檢查失敗，`System.ArrayTypeMismatchException`就會擲回。</span><span class="sxs-lookup"><span data-stu-id="abcc0-562">If this check fails, a `System.ArrayTypeMismatchException` is thrown.</span></span>

<span data-ttu-id="abcc0-563">方法、 索引子、 使用和執行個體建構函式可以宣告它們的最右邊的參數，參數陣列 ([參數陣列](classes.md#parameter-arrays))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-563">Methods, indexers, and instance constructors may declare their right-most parameter to be a parameter array ([Parameter arrays](classes.md#parameter-arrays)).</span></span> <span data-ttu-id="abcc0-564">以一般形式或以其展開的形式，取決於適用於叫用這類函式成員 ([適用的函式成員](expressions.md#applicable-function-member)):</span><span class="sxs-lookup"><span data-stu-id="abcc0-564">Such function members are invoked either in their normal form or in their expanded form depending on which is applicable ([Applicable function member](expressions.md#applicable-function-member)):</span></span>

*  <span data-ttu-id="abcc0-565">一般形式叫用含有參數陣列的函式成員時，提供給參數陣列引數必須是可以隱含轉換的單一運算式 ([隱含轉換](conversions.md#implicit-conversions)) 為參數陣列型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-565">When a function member with a parameter array is invoked in its normal form, the argument given for the parameter array must be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="abcc0-566">在此情況下，參數陣列的行為很類似值的參數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-566">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="abcc0-567">叫用其展開的形式叫用含有參數陣列的函式成員時，必須指定為參數陣列，其中每個引數是可以隱含地轉換運算式的零或多個位置引數 ([隱含轉換](conversions.md#implicit-conversions)) 參數陣列的項目型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-567">When a function member with a parameter array is invoked in its expanded form, the invocation must specify zero or more positional arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="abcc0-568">在此情況下，叫用建立具有相對應的引數的長度參數陣列型別的執行個體初始化為指定的引數的值，將陣列執行個體的項目，做為新建立的陣列執行個體的實際引數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-568">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="abcc0-569">引數清單的運算式一律會將它們寫入順序進行評估。</span><span class="sxs-lookup"><span data-stu-id="abcc0-569">The expressions of an argument list are always evaluated in the order they are written.</span></span> <span data-ttu-id="abcc0-570">因此，範例</span><span class="sxs-lookup"><span data-stu-id="abcc0-570">Thus, the example</span></span>
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
<span data-ttu-id="abcc0-571">產生下列輸出</span><span class="sxs-lookup"><span data-stu-id="abcc0-571">produces the output</span></span>
```
x = 0, y = 1, z = 2
x = 4, y = -1, z = 3
```

<span data-ttu-id="abcc0-572">陣列共同變異數規則 ([陣列共變數](arrays.md#array-covariance)) 允許值的陣列型別`A[]`是陣列型別的執行個體的參考`B[]`，前提是隱含參考轉換存在從`B`至`A`.</span><span class="sxs-lookup"><span data-stu-id="abcc0-572">The array co-variance rules ([Array covariance](arrays.md#array-covariance)) permit a value of an array type `A[]` to be a reference to an instance of an array type `B[]`, provided an implicit reference conversion exists from `B` to `A`.</span></span> <span data-ttu-id="abcc0-573">因為這些規則，當陣列元素的*reference_type*傳遞做為參考或輸出參數，執行階段檢查，才能確保陣列的實際項目型別完全相同的參數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-573">Because of these rules, when an array element of a *reference_type* is passed as a reference or output parameter, a run-time check is required to ensure that the actual element type of the array is identical to that of the parameter.</span></span> <span data-ttu-id="abcc0-574">在範例</span><span class="sxs-lookup"><span data-stu-id="abcc0-574">In the example</span></span>
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
<span data-ttu-id="abcc0-575">第二個引動過程`F`會導致`System.ArrayTypeMismatchException`因為實際的項目類型的擲回`b`是`string`而非`object`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-575">the second invocation of `F` causes a `System.ArrayTypeMismatchException` to be thrown because the actual element type of `b` is `string` and not `object`.</span></span>

<span data-ttu-id="abcc0-576">在展開的形式叫用含有參數陣列的函式成員時，引動過程處理完全如同陣列建立運算式中使用陣列初始設定式 ([陣列建立運算式](expressions.md#array-creation-expressions)) 周圍插入展開的參數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-576">When a function member with a parameter array is invoked in its expanded form, the invocation is processed exactly as if an array creation expression with an array initializer ([Array creation expressions](expressions.md#array-creation-expressions)) was inserted around the expanded parameters.</span></span> <span data-ttu-id="abcc0-577">例如，假設宣告</span><span class="sxs-lookup"><span data-stu-id="abcc0-577">For example, given the declaration</span></span>
```csharp
void F(int x, int y, params object[] args);
```
<span data-ttu-id="abcc0-578">展開的形式的方法中的下列引動過程</span><span class="sxs-lookup"><span data-stu-id="abcc0-578">the following invocations of the expanded form of the method</span></span>
```csharp
F(10, 20);
F(10, 20, 30, 40);
F(10, 20, 1, "hello", 3.0);
```
<span data-ttu-id="abcc0-579">實際上會對應至</span><span class="sxs-lookup"><span data-stu-id="abcc0-579">correspond exactly to</span></span>
```csharp
F(10, 20, new object[] {});
F(10, 20, new object[] {30, 40});
F(10, 20, new object[] {1, "hello", 3.0});
```

<span data-ttu-id="abcc0-580">特別是，請注意，給定的參數陣列引數時，會建立空的陣列。</span><span class="sxs-lookup"><span data-stu-id="abcc0-580">In particular, note that an empty array is created when there are zero arguments given for the parameter array.</span></span>

<span data-ttu-id="abcc0-581">當從對應的選擇性參數的函式成員，會省略引數時，函式成員宣告的預設引數會以隱含方式傳遞。</span><span class="sxs-lookup"><span data-stu-id="abcc0-581">When arguments are omitted from a function member with corresponding optional parameters, the default arguments of the function member declaration are implicitly passed.</span></span> <span data-ttu-id="abcc0-582">由於這些一律是常數，則其評估不會影響其餘引數的評估順序。</span><span class="sxs-lookup"><span data-stu-id="abcc0-582">Because these are always constant, their evaluation will not impact the evaluation order of the remaining arguments.</span></span>

### <a name="type-inference"></a><span data-ttu-id="abcc0-583">型別推斷</span><span class="sxs-lookup"><span data-stu-id="abcc0-583">Type inference</span></span>

<span data-ttu-id="abcc0-584">未指定類型引數，呼叫泛型方法時***型別推斷***程序會嘗試推斷呼叫的型別引數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-584">When a generic method is called without specifying type arguments, a ***type inference*** process attempts to infer type arguments for the call.</span></span> <span data-ttu-id="abcc0-585">型別推斷的目前狀態可讓更方便的語法，以設定用來呼叫泛型方法，並可以讓程式設計師，若要避免指定備援類型資訊。</span><span class="sxs-lookup"><span data-stu-id="abcc0-585">The presence of type inference allows a more convenient syntax to be used for calling a generic method, and allows the programmer to avoid specifying redundant type information.</span></span> <span data-ttu-id="abcc0-586">例如，假設方法宣告：</span><span class="sxs-lookup"><span data-stu-id="abcc0-586">For example, given the method declaration:</span></span>
```csharp
class Chooser
{
    static Random rand = new Random();

    public static T Choose<T>(T first, T second) {
        return (rand.Next(2) == 0)? first: second;
    }
}
```
<span data-ttu-id="abcc0-587">可叫用`Choose`不需明確指定型別引數的方法：</span><span class="sxs-lookup"><span data-stu-id="abcc0-587">it is possible to invoke the `Choose` method without explicitly specifying a type argument:</span></span>
```csharp
int i = Chooser.Choose(5, 213);                 // Calls Choose<int>

string s = Chooser.Choose("foo", "bar");        // Calls Choose<string>
```

<span data-ttu-id="abcc0-588">透過型別推斷，型別引數`int`和`string`方法取決於從引數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-588">Through type inference, the type arguments `int` and `string` are determined from the arguments to the method.</span></span>

<span data-ttu-id="abcc0-589">型別推斷，就會發生的繫結階段處理的方法引動過程的一部分 ([方法引動過程](expressions.md#method-invocations)) 和引動過程多載解析步驟之前會發生。</span><span class="sxs-lookup"><span data-stu-id="abcc0-589">Type inference occurs as part of the binding-time processing of a method invocation ([Method invocations](expressions.md#method-invocations)) and takes place before the overload resolution step of the invocation.</span></span> <span data-ttu-id="abcc0-590">當未指定特定方法群組中的方法引動過程，和任何類型引數會指定為方法引動過程的一部分時，型別推斷會套用到方法群組中每個泛型方法。</span><span class="sxs-lookup"><span data-stu-id="abcc0-590">When a particular method group is specified in a method invocation, and no type arguments are specified as part of the method invocation, type inference is applied to each generic method in the method group.</span></span> <span data-ttu-id="abcc0-591">如果型別推斷成功，則會使用以判斷後續多載解析的引數型別推斷的型別引數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-591">If type inference succeeds, then the inferred type arguments are used to determine the types of arguments for subsequent overload resolution.</span></span> <span data-ttu-id="abcc0-592">如果多載解析會選擇做為要叫用泛型方法，則會使用推斷的型別引數為實際型別引數的引動過程。</span><span class="sxs-lookup"><span data-stu-id="abcc0-592">If overload resolution chooses a generic method as the one to invoke, then the inferred type arguments are used as the actual type arguments for the invocation.</span></span> <span data-ttu-id="abcc0-593">如果特定方法的型別推斷失敗，該方法不會參與多載解析。</span><span class="sxs-lookup"><span data-stu-id="abcc0-593">If type inference for a particular method fails, that method does not participate in overload resolution.</span></span> <span data-ttu-id="abcc0-594">失敗的型別推斷，本身，不會繫結時間錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-594">The failure of type inference, in and of itself, does not cause a binding-time error.</span></span> <span data-ttu-id="abcc0-595">不過，它通常會導致繫結錯誤時若要尋找任何適用的方法就會多載解析失敗。</span><span class="sxs-lookup"><span data-stu-id="abcc0-595">However, it often leads to a binding-time error when overload resolution then fails to find any applicable methods.</span></span>

<span data-ttu-id="abcc0-596">如果提供的引數是不同的方法中的參數數目，則推斷立即失敗。</span><span class="sxs-lookup"><span data-stu-id="abcc0-596">If the supplied number of arguments is different than the number of parameters in the method, then inference immediately fails.</span></span> <span data-ttu-id="abcc0-597">否則，假設泛型的方法具有下列簽章：</span><span class="sxs-lookup"><span data-stu-id="abcc0-597">Otherwise, assume that the generic method has the following signature:</span></span>
```csharp
Tr M<X1,...,Xn>(T1 x1, ..., Tm xm)
```

<span data-ttu-id="abcc0-598">方法呼叫的表單`M(E1...Em)`的型別推斷工作是要尋找唯一的型別引數`S1...Sn`每個類型參數`X1...Xn`以便呼叫`M<S1...Sn>(E1...Em)`變成有效。</span><span class="sxs-lookup"><span data-stu-id="abcc0-598">With a method call of the form `M(E1...Em)` the task of type inference is to find unique type arguments `S1...Sn` for each of the type parameters `X1...Xn` so that the call `M<S1...Sn>(E1...Em)` becomes valid.</span></span>

<span data-ttu-id="abcc0-599">推斷程序期間，每個類型參數`Xi`是*修正*成特定的型別`Si`或*unfixed*具有一組相關聯的*界限*.</span><span class="sxs-lookup"><span data-stu-id="abcc0-599">During the process of inference each type parameter `Xi` is either *fixed* to a particular type `Si` or *unfixed* with an associated set of *bounds*.</span></span> <span data-ttu-id="abcc0-600">每個範圍都是某種類型`T`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-600">Each of the bounds is some type `T`.</span></span> <span data-ttu-id="abcc0-601">一開始，每個型別變數`Xi`是 unfixed 與範圍的空集合。</span><span class="sxs-lookup"><span data-stu-id="abcc0-601">Initially each type variable `Xi` is unfixed with an empty set of bounds.</span></span>

<span data-ttu-id="abcc0-602">型別推斷會採用階段中的位置。</span><span class="sxs-lookup"><span data-stu-id="abcc0-602">Type inference takes place in phases.</span></span> <span data-ttu-id="abcc0-603">每個階段會嘗試推斷類型引數的多個類型的變數，根據前一個階段的結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-603">Each phase will try to infer type arguments for more type variables based on the findings of the previous phase.</span></span> <span data-ttu-id="abcc0-604">第一個階段會讓一些初始推斷的範圍中，而第二個階段可以修正特定類型的型別變數，並推測進一步界限。</span><span class="sxs-lookup"><span data-stu-id="abcc0-604">The first phase makes some initial inferences of bounds, whereas the second phase fixes type variables to specific types and infers further bounds.</span></span> <span data-ttu-id="abcc0-605">第二個階段可能是重複次數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-605">The second phase may have to be repeated a number of times.</span></span>

<span data-ttu-id="abcc0-606">*注意：* 型別推斷就會發生不只會呼叫泛型方法。</span><span class="sxs-lookup"><span data-stu-id="abcc0-606">*Note:* Type inference takes place not only when a generic method is called.</span></span> <span data-ttu-id="abcc0-607">所述的方法群組轉換的型別推斷[的方法群組轉換的型別推斷](expressions.md#type-inference-for-conversion-of-method-groups)尋找一組運算式的最常用的型別述[找出一組常見的最佳類型運算式的](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-607">Type inference for conversion of method groups is described in [Type inference for conversion of method groups](expressions.md#type-inference-for-conversion-of-method-groups) and finding the best common type of a set of expressions is described in [Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).</span></span>

#### <a name="the-first-phase"></a><span data-ttu-id="abcc0-608">第一個階段</span><span class="sxs-lookup"><span data-stu-id="abcc0-608">The first phase</span></span>

<span data-ttu-id="abcc0-609">針對每個方法引數的`Ei`:</span><span class="sxs-lookup"><span data-stu-id="abcc0-609">For each of the method arguments `Ei`:</span></span>

*   <span data-ttu-id="abcc0-610">如果`Ei`是匿名的函式*明確的參數型別推斷*([明確的參數型別推斷](expressions.md#explicit-parameter-type-inferences)) 從進行`Ei`至 `Ti`</span><span class="sxs-lookup"><span data-stu-id="abcc0-610">If `Ei` is an anonymous function, an *explicit parameter type inference* ([Explicit parameter type inferences](expressions.md#explicit-parameter-type-inferences)) is made from `Ei` to `Ti`</span></span>
*   <span data-ttu-id="abcc0-611">否則，如果`Ei`具有型別的`U`並`xi`是實值參數則會顯示*下限推斷*進行*從* `U` *至*`Ti`.</span><span class="sxs-lookup"><span data-stu-id="abcc0-611">Otherwise, if `Ei` has a type `U` and `xi` is a value parameter then a *lower-bound inference* is made *from* `U` *to* `Ti`.</span></span>
*   <span data-ttu-id="abcc0-612">否則，如果`Ei`具有型別的`U`並`xi`是`ref`或`out`參數則*精確推斷*進行*從* `U`*要* `Ti`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-612">Otherwise, if `Ei` has a type `U` and `xi` is a `ref` or `out` parameter then an *exact inference* is made *from* `U` *to* `Ti`.</span></span>
*   <span data-ttu-id="abcc0-613">否則，無法推斷會將這個引數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-613">Otherwise, no inference is made for this argument.</span></span>


#### <a name="the-second-phase"></a><span data-ttu-id="abcc0-614">第二個階段</span><span class="sxs-lookup"><span data-stu-id="abcc0-614">The second phase</span></span>

<span data-ttu-id="abcc0-615">第二個階段如下所示：</span><span class="sxs-lookup"><span data-stu-id="abcc0-615">The second phase proceeds as follows:</span></span>

*   <span data-ttu-id="abcc0-616">所有*unfixed*類型變數`Xi`哪些則否*取決於*([相依性](expressions.md#dependence)) 任何`Xj`固定的 ([正在修復](expressions.md#fixing)).</span><span class="sxs-lookup"><span data-stu-id="abcc0-616">All *unfixed* type variables `Xi` which do not *depend on* ([Dependence](expressions.md#dependence)) any `Xj` are fixed ([Fixing](expressions.md#fixing)).</span></span>
*   <span data-ttu-id="abcc0-617">如果存在這類的型別變數，全部*unfixed*類型變數`Xi`會*修正*，下列全部保留：</span><span class="sxs-lookup"><span data-stu-id="abcc0-617">If no such type variables exist, all *unfixed* type variables `Xi` are *fixed* for which all of the following hold:</span></span>
    *   <span data-ttu-id="abcc0-618">至少一個型別變數`Xj`相依於 `Xi`</span><span class="sxs-lookup"><span data-stu-id="abcc0-618">There is at least one type variable `Xj` that depends on `Xi`</span></span>
    *   <span data-ttu-id="abcc0-619">`Xi` 具有非空白集的範圍</span><span class="sxs-lookup"><span data-stu-id="abcc0-619">`Xi` has a non-empty set of bounds</span></span>
*   <span data-ttu-id="abcc0-620">如果沒有這類的型別變數存在，而且還有*unfixed*類型變數、 類型推斷失敗。</span><span class="sxs-lookup"><span data-stu-id="abcc0-620">If no such type variables exist and there are still *unfixed* type variables, type inference fails.</span></span>
*   <span data-ttu-id="abcc0-621">否則，如果有任何進一步*unfixed*有型別變數、 型別推斷會成功。</span><span class="sxs-lookup"><span data-stu-id="abcc0-621">Otherwise, if no further *unfixed* type variables exist, type inference succeeds.</span></span>
*   <span data-ttu-id="abcc0-622">否則，所有引數`Ei`與對應參數類型`Ti`所在*輸出類型*([輸出類型](expressions.md#output-types)) 包含*unfixed*輸入變數`Xj`但*輸入型別*([的輸入類型](expressions.md#input-types)) 未這麼做，*輸出型別推斷*([輸出型別推斷](expressions.md#output-type-inferences)) 由*從* `Ei` *來* `Ti`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-622">Otherwise, for all arguments `Ei` with corresponding parameter type `Ti` where the *output types* ([Output types](expressions.md#output-types)) contain *unfixed* type variables `Xj` but the *input types* ([Input types](expressions.md#input-types)) do not, an *output type inference* ([Output type inferences](expressions.md#output-type-inferences)) is made *from* `Ei` *to* `Ti`.</span></span> <span data-ttu-id="abcc0-623">然後，系統會重複第二個階段。</span><span class="sxs-lookup"><span data-stu-id="abcc0-623">Then the second phase is repeated.</span></span>

#### <a name="input-types"></a><span data-ttu-id="abcc0-624">輸入的類型</span><span class="sxs-lookup"><span data-stu-id="abcc0-624">Input types</span></span>

<span data-ttu-id="abcc0-625">如果`E`是方法群組或隱含型別的匿名函式與`T`則委派型別或運算式樹狀架構類型的所有參數類型`T`會*的輸入類型*的`E` *與類型* `T`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-625">If `E` is a method group or implicitly typed anonymous function and `T` is a delegate type or expression tree type then all the parameter types of `T` are *input types* of `E` *with type* `T`.</span></span>

####  <a name="output-types"></a><span data-ttu-id="abcc0-626">輸出類型</span><span class="sxs-lookup"><span data-stu-id="abcc0-626">Output types</span></span>

<span data-ttu-id="abcc0-627">如果`E`方法群組或匿名函式與`T`是委派型別或運算式樹狀架構型別則傳回型別`T`是*輸出的型別* `E` *類型* `T`.</span><span class="sxs-lookup"><span data-stu-id="abcc0-627">If `E` is a method group or an anonymous function and `T` is a delegate type or expression tree type then the return type of `T` is an *output type of* `E` *with type* `T`.</span></span>

#### <a name="dependence"></a><span data-ttu-id="abcc0-628">相依性</span><span class="sxs-lookup"><span data-stu-id="abcc0-628">Dependence</span></span>

<span data-ttu-id="abcc0-629">*Unfixed*類型變數`Xi`*直接相依於*unfixed 型別變數`Xj`某些引數的 if`Ek`類型`Tk` `Xj`中，就會發生*輸入類型*的`Ek`類型`Tk`並`Xi`中發生*輸出類型*的`Ek`類型`Tk`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-629">An *unfixed* type variable `Xi` *depends directly on* an unfixed type variable `Xj` if for some argument `Ek` with type `Tk` `Xj` occurs in an *input type* of `Ek` with type `Tk` and `Xi` occurs in an *output type* of `Ek` with type `Tk`.</span></span>

<span data-ttu-id="abcc0-630">`Xj` *取決於*`Xi`如果`Xj`*直接相依於*`Xi`或者`Xi`*直接相依於*`Xk`和`Xk`*而定* `Xj`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-630">`Xj` *depends on* `Xi` if `Xj` *depends directly on* `Xi` or if `Xi` *depends directly on* `Xk` and `Xk` *depends on* `Xj`.</span></span> <span data-ttu-id="abcc0-631">因此 「 相依於 」 是 「 相依於直接 」 的可轉移的但不是 （reflexive relationship） 結束。</span><span class="sxs-lookup"><span data-stu-id="abcc0-631">Thus "depends on" is the transitive but not reflexive closure of "depends directly on".</span></span>

#### <a name="output-type-inferences"></a><span data-ttu-id="abcc0-632">輸出型別推斷</span><span class="sxs-lookup"><span data-stu-id="abcc0-632">Output type inferences</span></span>

<span data-ttu-id="abcc0-633">*輸出型別推斷*更為*從*運算式`E`*至*類型`T`方式如下：</span><span class="sxs-lookup"><span data-stu-id="abcc0-633">An *output type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>

*  <span data-ttu-id="abcc0-634">如果`E`推斷傳回類型為匿名函式`U`([推斷傳回型別](expressions.md#inferred-return-type)) 和`T`是委派類型或傳回類型的運算式樹狀架構類型`Tb`，然後*下限推斷*([下限推斷](expressions.md#lower-bound-inferences)) 進行*從* `U` *到* `Tb`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-634">If `E` is an anonymous function with inferred return type  `U` ([Inferred return type](expressions.md#inferred-return-type)) and `T` is a delegate type or expression tree type with return type `Tb`, then a *lower-bound inference* ([Lower-bound inferences](expressions.md#lower-bound-inferences)) is made *from* `U` *to* `Tb`.</span></span>
*  <span data-ttu-id="abcc0-635">否則，如果`E`是方法群組和`T`是委派型別或參數類型的運算式樹狀架構型`T1...Tk`和傳回型別`Tb`，和的多載解析`E`類型`T1...Tk`產生單一方法的傳回型別`U`，則會顯示*下限推斷*進行*從* `U` *至* `Tb`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-635">Otherwise, if `E` is a method group and `T` is a delegate type or expression tree type with parameter types `T1...Tk` and return type `Tb`, and overload resolution of `E` with the types `T1...Tk` yields a single method with return type `U`, then a *lower-bound inference* is made *from* `U` *to* `Tb`.</span></span>
*  <span data-ttu-id="abcc0-636">否則，如果`E`是一個運算式與型別`U`，則會顯示*下限推斷*進行*從* `U` *到* `T`.</span><span class="sxs-lookup"><span data-stu-id="abcc0-636">Otherwise, if `E` is an expression with type `U`, then a *lower-bound inference* is made *from* `U` *to* `T`.</span></span>
*  <span data-ttu-id="abcc0-637">否則，會不進行任何推斷。</span><span class="sxs-lookup"><span data-stu-id="abcc0-637">Otherwise, no inferences are made.</span></span>

#### <a name="explicit-parameter-type-inferences"></a><span data-ttu-id="abcc0-638">明確的參數型別推斷</span><span class="sxs-lookup"><span data-stu-id="abcc0-638">Explicit parameter type inferences</span></span>

<span data-ttu-id="abcc0-639">*明確的參數型別推斷*更為*從*運算式`E`*至*類型`T`方式如下：</span><span class="sxs-lookup"><span data-stu-id="abcc0-639">An *explicit parameter type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>

*  <span data-ttu-id="abcc0-640">如果`E`是以參數類型的明確型別的匿名函式`U1...Uk`並`T`是委派型別或參數類型的運算式樹狀架構類型`V1...Vk`然後針對每個`Ui`*確切推斷*([精確推斷](expressions.md#exact-inferences)) 進行*從* `Ui` *至*對應`Vi`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-640">If `E` is an explicitly typed anonymous function with parameter types `U1...Uk` and `T` is a delegate type or expression tree type with parameter types `V1...Vk` then for each `Ui` an *exact inference* ([Exact inferences](expressions.md#exact-inferences)) is made *from* `Ui` *to* the corresponding `Vi`.</span></span>

#### <a name="exact-inferences"></a><span data-ttu-id="abcc0-641">精確推斷</span><span class="sxs-lookup"><span data-stu-id="abcc0-641">Exact inferences</span></span>

<span data-ttu-id="abcc0-642">*精確推斷\*\*從*型別的`U`*至*類型`V`為止，如下所示：</span><span class="sxs-lookup"><span data-stu-id="abcc0-642">An *exact inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="abcc0-643">如果`V`是其中一個*unfixed* `Xi`然後`U`新增至確切繫結的一組`Xi`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-643">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of exact bounds for `Xi`.</span></span>

*  <span data-ttu-id="abcc0-644">否則，會設定`V1...Vk`和`U1...Uk`取決於檢查如果下列任何情況下套用：</span><span class="sxs-lookup"><span data-stu-id="abcc0-644">Otherwise, sets `V1...Vk` and `U1...Uk` are determined by checking if any of the following cases apply:</span></span>

   *  <span data-ttu-id="abcc0-645">`V` 陣列型別`V1[...]`並`U`是陣列類型`U1[...]`相同的順序</span><span class="sxs-lookup"><span data-stu-id="abcc0-645">`V` is an array type `V1[...]` and `U` is an array type `U1[...]`  of the same rank</span></span>
   *  <span data-ttu-id="abcc0-646">`V` 是的型別`V1?`和`U`是類型 `U1?`</span><span class="sxs-lookup"><span data-stu-id="abcc0-646">`V` is the type `V1?` and `U` is the type `U1?`</span></span>
   *  <span data-ttu-id="abcc0-647">`V` 是建構的型別`C<V1...Vk>`和`U`為建構的類型 `C<U1...Uk>`</span><span class="sxs-lookup"><span data-stu-id="abcc0-647">`V` is a constructed type `C<V1...Vk>`and `U` is a constructed type `C<U1...Uk>`</span></span>

   <span data-ttu-id="abcc0-648">如果任何這些情況下再*精確推斷*進行*從*每個`Ui`*至*對應`Vi`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-648">If any of these cases apply then an *exact inference* is made *from* each `Ui` *to* the corresponding `Vi`.</span></span>

*  <span data-ttu-id="abcc0-649">否則，系統會不進行任何推斷。</span><span class="sxs-lookup"><span data-stu-id="abcc0-649">Otherwise no inferences are made.</span></span>

#### <a name="lower-bound-inferences"></a><span data-ttu-id="abcc0-650">下限推斷</span><span class="sxs-lookup"><span data-stu-id="abcc0-650">Lower-bound inferences</span></span>

<span data-ttu-id="abcc0-651">A*下限推斷\*\*從*類型`U`*至*類型`V`為止，如下所示：</span><span class="sxs-lookup"><span data-stu-id="abcc0-651">A *lower-bound inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="abcc0-652">如果`V`是其中一個*unfixed* `Xi`然後`U`新增至集合的下限`Xi`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-652">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of lower bounds for `Xi`.</span></span>
*  <span data-ttu-id="abcc0-653">否則，如果`V`是型別`V1?`並`U`是型別`U1?`則會進行從下限推斷`U1`至`V1`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-653">Otherwise, if `V` is the type `V1?`and `U` is the type `U1?` then a lower bound inference is made from `U1` to `V1`.</span></span>
*  <span data-ttu-id="abcc0-654">否則，會設定`U1...Uk`和`V1...Vk`取決於檢查如果下列任何情況下套用：</span><span class="sxs-lookup"><span data-stu-id="abcc0-654">Otherwise, sets `U1...Uk` and `V1...Vk` are determined by checking if any of the following cases apply:</span></span>
   *  <span data-ttu-id="abcc0-655">`V` 陣列型別`V1[...]`並`U`是陣列類型`U1[...]`(或型別參數的有效基底型別`U1[...]`) 相同的順序</span><span class="sxs-lookup"><span data-stu-id="abcc0-655">`V` is an array type `V1[...]` and `U` is an array type `U1[...]` (or a type parameter whose effective base type is `U1[...]`) of the same rank</span></span>
   *  <span data-ttu-id="abcc0-656">`V` 是其中一個`IEnumerable<V1>`，`ICollection<V1>`或是`IList<V1>`並`U`是一維陣列類型`U1[]`(或類型參數的有效基底型別`U1[]`)</span><span class="sxs-lookup"><span data-stu-id="abcc0-656">`V` is one of `IEnumerable<V1>`, `ICollection<V1>` or `IList<V1>` and `U` is a one-dimensional array type `U1[]`(or a type parameter whose effective base type is `U1[]`)</span></span>
   *  <span data-ttu-id="abcc0-657">`V` 是建構的類別、 結構、 介面或委派型別`C<V1...Vk>`，而且沒有唯一的型別`C<U1...Uk>`使得`U`(或者，如果`U`型別參數，其有效的基底類別或其有效的介面集的任何成員) 是完全一樣，繼承自 （直接或間接），或實作 （直接或間接） `C<U1...Uk>`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-657">`V` is a constructed class, struct, interface or delegate type `C<V1...Vk>` and there is a unique type `C<U1...Uk>` such that `U` (or, if `U` is a type parameter, its effective base class or any member of its effective interface set) is identical to, inherits from (directly or indirectly), or implements (directly or indirectly) `C<U1...Uk>`.</span></span>

      <span data-ttu-id="abcc0-658">("唯一性 」 限制表示大小寫的介面中`C<T> {} class U: C<X>, C<Y> {}`，然後從推斷時，才無法推斷`U`來`C<T>`因為`U1`可能`X`或`Y`。)</span><span class="sxs-lookup"><span data-stu-id="abcc0-658">(The "uniqueness" restriction means that in the case interface `C<T> {} class U: C<X>, C<Y> {}`, then no inference is made when inferring from `U` to `C<T>` because `U1` could be `X` or `Y`.)</span></span>

   <span data-ttu-id="abcc0-659">如果任何這些情況下則會進行推斷*從*每個`Ui`*來*對應`Vi`，如下所示：</span><span class="sxs-lookup"><span data-stu-id="abcc0-659">If any of these cases apply then an inference is made *from* each `Ui` *to* the corresponding `Vi` as follows:</span></span>

   *  <span data-ttu-id="abcc0-660">如果`Ui`不已知為參考型別就*精確推斷*進行</span><span class="sxs-lookup"><span data-stu-id="abcc0-660">If `Ui` is not known to be a reference type then an *exact inference* is made</span></span>
   *  <span data-ttu-id="abcc0-661">否則，如果`U`是陣列類型則會顯示*下限推斷*進行</span><span class="sxs-lookup"><span data-stu-id="abcc0-661">Otherwise, if `U` is an array type then a *lower-bound inference* is made</span></span>
   *  <span data-ttu-id="abcc0-662">否則，如果`V`已`C<V1...Vk>`推斷而定的第 i 個類型參數則`C`:</span><span class="sxs-lookup"><span data-stu-id="abcc0-662">Otherwise, if `V` is `C<V1...Vk>` then inference depends on the i-th type parameter of `C`:</span></span>
      *  <span data-ttu-id="abcc0-663">如果它是 covariant 則會顯示*下限推斷*為止。</span><span class="sxs-lookup"><span data-stu-id="abcc0-663">If it is covariant then a *lower-bound inference* is made.</span></span>
      *  <span data-ttu-id="abcc0-664">如果其為 contravariant 就*上限推斷*為止。</span><span class="sxs-lookup"><span data-stu-id="abcc0-664">If it is contravariant then an *upper-bound inference* is made.</span></span>
      *  <span data-ttu-id="abcc0-665">如果非變異就*精確推斷*進行。</span><span class="sxs-lookup"><span data-stu-id="abcc0-665">If it is invariant then an *exact inference* is made.</span></span>
*  <span data-ttu-id="abcc0-666">否則，會不進行任何推斷。</span><span class="sxs-lookup"><span data-stu-id="abcc0-666">Otherwise, no inferences are made.</span></span>

#### <a name="upper-bound-inferences"></a><span data-ttu-id="abcc0-667">上限推斷</span><span class="sxs-lookup"><span data-stu-id="abcc0-667">Upper-bound inferences</span></span>

<span data-ttu-id="abcc0-668">*上限推斷\*\*從*型別的`U`*至*類型`V`為止，如下所示：</span><span class="sxs-lookup"><span data-stu-id="abcc0-668">An *upper-bound inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="abcc0-669">如果`V`是其中一個*unfixed* `Xi`然後`U`加到上限的集合`Xi`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-669">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of upper bounds for `Xi`.</span></span>
*  <span data-ttu-id="abcc0-670">否則，會設定`V1...Vk`和`U1...Uk`取決於檢查如果下列任何情況下套用：</span><span class="sxs-lookup"><span data-stu-id="abcc0-670">Otherwise, sets `V1...Vk` and `U1...Uk` are determined by checking if any of the following cases apply:</span></span>
   *  <span data-ttu-id="abcc0-671">`U` 陣列型別`U1[...]`並`V`是陣列類型`V1[...]`相同的順序</span><span class="sxs-lookup"><span data-stu-id="abcc0-671">`U` is an array type `U1[...]` and `V` is an array type `V1[...]` of the same rank</span></span>
   *  <span data-ttu-id="abcc0-672">`U` 是其中一個`IEnumerable<Ue>`，`ICollection<Ue>`或是`IList<Ue>`和`V`是一維陣列類型 `Ve[]`</span><span class="sxs-lookup"><span data-stu-id="abcc0-672">`U` is one of `IEnumerable<Ue>`, `ICollection<Ue>` or `IList<Ue>` and `V` is a one-dimensional array type `Ve[]`</span></span>
   *  <span data-ttu-id="abcc0-673">`U` 是的型別`U1?`和`V`是類型 `V1?`</span><span class="sxs-lookup"><span data-stu-id="abcc0-673">`U` is the type `U1?` and `V` is the type `V1?`</span></span>
   *  <span data-ttu-id="abcc0-674">`U` 建構的類別、 結構、 介面或委派的型別`C<U1...Uk>`和`V`是類別、 結構、 介面或委派的類型，也就是完全一樣，繼承自 （直接或間接） 或實作 （直接或間接） 唯一的型別 `C<V1...Vk>`</span><span class="sxs-lookup"><span data-stu-id="abcc0-674">`U` is constructed class, struct, interface or delegate type `C<U1...Uk>` and `V` is a class, struct, interface or delegate type which is identical to, inherits from (directly or indirectly), or implements (directly or indirectly) a unique type `C<V1...Vk>`</span></span>

      <span data-ttu-id="abcc0-675">(「 唯一性 」 限制表示，如果我們有`interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`，然後從推斷時，才無法推斷`C<U1>`來`V<Q>`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-675">(The "uniqueness" restriction means that if we have `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, then no inference is made when inferring from `C<U1>` to `V<Q>`.</span></span> <span data-ttu-id="abcc0-676">推斷不會從`U1`設為`X<Q>`或`Y<Q>`。)</span><span class="sxs-lookup"><span data-stu-id="abcc0-676">Inferences are not made from `U1` to either `X<Q>` or `Y<Q>`.)</span></span>

   <span data-ttu-id="abcc0-677">如果任何這些情況下則會進行推斷*從*每個`Ui`*來*對應`Vi`，如下所示：</span><span class="sxs-lookup"><span data-stu-id="abcc0-677">If any of these cases apply then an inference is made *from* each `Ui` *to* the corresponding `Vi` as follows:</span></span>
   *  <span data-ttu-id="abcc0-678">如果`Ui`不已知為參考型別就*精確推斷*進行</span><span class="sxs-lookup"><span data-stu-id="abcc0-678">If  `Ui` is not known to be a reference type then an *exact inference* is made</span></span>
   *  <span data-ttu-id="abcc0-679">否則，如果`V`陣列型別就*上限推斷*進行</span><span class="sxs-lookup"><span data-stu-id="abcc0-679">Otherwise, if `V` is an array type then an *upper-bound inference* is made</span></span>
   *  <span data-ttu-id="abcc0-680">否則，如果`U`已`C<U1...Uk>`推斷而定的第 i 個類型參數則`C`:</span><span class="sxs-lookup"><span data-stu-id="abcc0-680">Otherwise, if `U` is `C<U1...Uk>` then inference depends on the i-th type parameter of `C`:</span></span>
      *  <span data-ttu-id="abcc0-681">如果它是 covariant 就*上限推斷*為止。</span><span class="sxs-lookup"><span data-stu-id="abcc0-681">If it is covariant then an *upper-bound inference* is made.</span></span>
      *  <span data-ttu-id="abcc0-682">如果這是逆變性則會顯示*下限推斷*進行。</span><span class="sxs-lookup"><span data-stu-id="abcc0-682">If it is contravariant then a *lower-bound inference* is made.</span></span>
      *  <span data-ttu-id="abcc0-683">如果非變異就*精確推斷*進行。</span><span class="sxs-lookup"><span data-stu-id="abcc0-683">If it is invariant then an *exact inference* is made.</span></span>
*  <span data-ttu-id="abcc0-684">否則，會不進行任何推斷。</span><span class="sxs-lookup"><span data-stu-id="abcc0-684">Otherwise, no inferences are made.</span></span>   

#### <a name="fixing"></a><span data-ttu-id="abcc0-685">修正</span><span class="sxs-lookup"><span data-stu-id="abcc0-685">Fixing</span></span>

<span data-ttu-id="abcc0-686">*Unfixed*類型變數`Xi`界限的一組會*修正*，如下所示：</span><span class="sxs-lookup"><span data-stu-id="abcc0-686">An *unfixed* type variable `Xi` with a set of bounds is *fixed* as follows:</span></span>

*  <span data-ttu-id="abcc0-687">一組*候選型別*`Uj`界限的集合中的所有類型的集合一開始都`Xi`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-687">The set of *candidate types* `Uj` starts out as the set of all types in the set of bounds for `Xi`.</span></span>
*  <span data-ttu-id="abcc0-688">然後，我們檢驗每個繫結的`Xi`依次：每個實際的繫結`U`的`Xi`所有型別`Uj`並不是等於`U`從候選項目集合中移除。</span><span class="sxs-lookup"><span data-stu-id="abcc0-688">We then examine each bound for `Xi` in turn: For each exact bound `U` of `Xi` all types `Uj` which are not identical to `U` are removed from the candidate set.</span></span> <span data-ttu-id="abcc0-689">針對每個下限`U`的`Xi`所有型別`Uj`若其中有要*不*的隱含轉換`U`從候選項目集合中移除。</span><span class="sxs-lookup"><span data-stu-id="abcc0-689">For each lower bound `U` of `Xi` all types `Uj` to which there is *not* an implicit conversion from `U` are removed from the candidate set.</span></span> <span data-ttu-id="abcc0-690">針對每個的上限`U`的`Xi`所有型別`Uj`從它*不*隱含轉換成`U`從候選項目集合中移除。</span><span class="sxs-lookup"><span data-stu-id="abcc0-690">For each upper bound `U` of `Xi` all types `Uj` from which there is *not* an implicit conversion to `U` are removed from the candidate set.</span></span>
*  <span data-ttu-id="abcc0-691">如果在剩餘的候選項目類型之間`Uj`沒有唯一的型別`V`包括不的隱含轉換為所有其他的候選項目類型，然後從`Xi`會固定為`V`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-691">If among the remaining candidate types `Uj` there is a unique type `V` from which there is an implicit conversion to all the other candidate types, then `Xi` is fixed to `V`.</span></span>
*  <span data-ttu-id="abcc0-692">否則，型別推斷會失敗。</span><span class="sxs-lookup"><span data-stu-id="abcc0-692">Otherwise, type inference fails.</span></span>

#### <a name="inferred-return-type"></a><span data-ttu-id="abcc0-693">推斷傳回型別</span><span class="sxs-lookup"><span data-stu-id="abcc0-693">Inferred return type</span></span>

<span data-ttu-id="abcc0-694">推斷傳回類型的匿名函式`F`會使用於型別推斷和多載解析。</span><span class="sxs-lookup"><span data-stu-id="abcc0-694">The inferred return type of an anonymous function `F` is used during type inference and overload resolution.</span></span> <span data-ttu-id="abcc0-695">所有的參數型別已知，可能是因為它們已明確指定，其中提供透過匿名函式轉換或推斷泛型上為封入型別推斷期間的匿名函式只可以判斷推斷傳回型別方法引動過程。</span><span class="sxs-lookup"><span data-stu-id="abcc0-695">The inferred return type can only be determined for an anonymous function where all parameter types are known, either because they are explicitly given, provided through an anonymous function conversion or inferred during type inference on an enclosing generic method invocation.</span></span>

<span data-ttu-id="abcc0-696">***推斷結果型別***判斷方式如下：</span><span class="sxs-lookup"><span data-stu-id="abcc0-696">The ***inferred result type*** is determined as follows:</span></span>

*  <span data-ttu-id="abcc0-697">如果主體`F`已*運算式*具有一個型別，則推斷的結果型別`F`是該運算式的型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-697">If the body of `F` is an *expression* that has a type, then the inferred result type of `F` is the type of that expression.</span></span>
*  <span data-ttu-id="abcc0-698">如果主體`F`是*區塊*和運算式中的區塊集`return`陳述式有最佳的一般型別`T`([找出最佳的常見類型的一組運算式](expressions.md#finding-the-best-common-type-of-a-set-of-expressions))，然後的推斷的結果型別`F`是`T`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-698">If the body of `F` is a *block* and the set of expressions in the block's `return` statements has a best common type `T` ([Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), then the inferred result type of `F` is `T`.</span></span>
*  <span data-ttu-id="abcc0-699">否則，結果型別無法推斷`F`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-699">Otherwise, a result type cannot be inferred for `F`.</span></span>

<span data-ttu-id="abcc0-700">***推斷傳回型別***判斷方式如下：</span><span class="sxs-lookup"><span data-stu-id="abcc0-700">The ***inferred return type*** is determined as follows:</span></span>

*  <span data-ttu-id="abcc0-701">如果`F`且非同步的主體`F`是歸類為執行任何動作的運算式 ([運算式分類](expressions.md#expression-classifications))，或陳述式區塊，其中沒有 return 陳述式有運算式，推斷傳回類型 `System.Threading.Tasks.Task`</span><span class="sxs-lookup"><span data-stu-id="abcc0-701">If `F` is async and the body of `F` is either an expression classified as nothing ([Expression classifications](expressions.md#expression-classifications)), or a statement block where no return statements have expressions, the inferred return type is `System.Threading.Tasks.Task`</span></span>
*  <span data-ttu-id="abcc0-702">如果`F`非同步處理，且具有推斷的結果型別`T`，推斷傳回類型為`System.Threading.Tasks.Task<T>`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-702">If `F` is async and has an inferred result type `T`, the inferred return type is `System.Threading.Tasks.Task<T>`.</span></span>
*  <span data-ttu-id="abcc0-703">如果`F`非同步，且具有推斷的結果型別`T`，推斷傳回類型為`T`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-703">If `F` is non-async and has an inferred result type `T`, the inferred return type is `T`.</span></span>
*  <span data-ttu-id="abcc0-704">否則傳回的型別無法推斷`F`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-704">Otherwise a return type cannot be inferred for `F`.</span></span>

<span data-ttu-id="abcc0-705">涉及匿名函式的型別推斷的範例，請考慮`Select`擴充方法中宣告`System.Linq.Enumerable`類別：</span><span class="sxs-lookup"><span data-stu-id="abcc0-705">As an example of type inference involving anonymous functions, consider the `Select` extension method declared in the `System.Linq.Enumerable` class:</span></span>
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

<span data-ttu-id="abcc0-706">假設`System.Linq`與已匯入命名空間`using`子句，並假設類別`Customer`與`Name`型別的屬性`string`，`Select`方法可用來選取一份客戶清單的名稱：</span><span class="sxs-lookup"><span data-stu-id="abcc0-706">Assuming the `System.Linq` namespace was imported with a `using` clause, and given a class `Customer` with a `Name` property of type `string`, the `Select` method can be used to select the names of a list of customers:</span></span>
```csharp
List<Customer> customers = GetCustomerList();
IEnumerable<string> names = customers.Select(c => c.Name);
```

<span data-ttu-id="abcc0-707">擴充方法引動過程 ([擴充方法引動過程](expressions.md#extension-method-invocations)) 的`Select`處理重寫至靜態方法的引動過程的引動過程：</span><span class="sxs-lookup"><span data-stu-id="abcc0-707">The extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)) of `Select` is processed by rewriting the invocation to a static method invocation:</span></span>
```csharp
IEnumerable<string> names = Enumerable.Select(customers, c => c.Name);
```

<span data-ttu-id="abcc0-708">因為未明確指定類型引數，型別推斷來推斷類型引數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-708">Since type arguments were not explicitly specified, type inference is used to infer the type arguments.</span></span> <span data-ttu-id="abcc0-709">首先，`customers`引數與相關`source`參數，推斷`T`要`Customer`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-709">First, the `customers` argument is related to the `source` parameter, inferring `T` to be `Customer`.</span></span> <span data-ttu-id="abcc0-710">然後，使用匿名函式輸入上面所述的推斷程序`c`為 object 型別`Customer`，和運算式`c.Name`相關的傳回型別`selector`參數，推斷`S`要`string`.</span><span class="sxs-lookup"><span data-stu-id="abcc0-710">Then, using the anonymous function type inference process described above, `c` is given type `Customer`, and the expression `c.Name` is related to the return type of the `selector` parameter, inferring `S` to be `string`.</span></span> <span data-ttu-id="abcc0-711">因此，此引動過程相當於</span><span class="sxs-lookup"><span data-stu-id="abcc0-711">Thus, the invocation is equivalent to</span></span>
```csharp
Sequence.Select<Customer,string>(customers, (Customer c) => c.Name)
```
<span data-ttu-id="abcc0-712">結果會是類型`IEnumerable<string>`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-712">and the result is of type `IEnumerable<string>`.</span></span>

<span data-ttu-id="abcc0-713">下列範例示範如何匿名函式型別推斷可讓 「 順著 」 中的泛型方法引動過程的引數之間的型別資訊。</span><span class="sxs-lookup"><span data-stu-id="abcc0-713">The following example demonstrates how anonymous function type inference allows type information to "flow" between arguments in a generic method invocation.</span></span> <span data-ttu-id="abcc0-714">有了方法：</span><span class="sxs-lookup"><span data-stu-id="abcc0-714">Given the method:</span></span>
```csharp
static Z F<X,Y,Z>(X value, Func<X,Y> f1, Func<Y,Z> f2) {
    return f2(f1(value));
}
```

<span data-ttu-id="abcc0-715">引動過程的型別推斷：</span><span class="sxs-lookup"><span data-stu-id="abcc0-715">Type inference for the invocation:</span></span>
```csharp
double seconds = F("1:15:30", s => TimeSpan.Parse(s), t => t.TotalSeconds);
```
<span data-ttu-id="abcc0-716">繼續進行，如下所示：首先，引數`"1:15:30"`與相關`value`參數，推斷`X`要`string`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-716">proceeds as follows: First, the argument `"1:15:30"` is related to the `value` parameter, inferring `X` to be `string`.</span></span> <span data-ttu-id="abcc0-717">接著，第一個匿名函式的參數`s`，指定推斷的型別`string`，和運算式`TimeSpan.Parse(s)`相關的傳回型別`f1`、 推斷`Y`要`System.TimeSpan`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-717">Then, the parameter of the first anonymous function, `s`, is given the inferred type `string`, and the expression `TimeSpan.Parse(s)` is related to the return type of `f1`, inferring `Y` to be `System.TimeSpan`.</span></span> <span data-ttu-id="abcc0-718">最後，第二個匿名函式的參數`t`，指定推斷的型別`System.TimeSpan`，和運算式`t.TotalSeconds`相關的傳回型別`f2`、 推斷`Z`要`double`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-718">Finally, the parameter of the second anonymous function, `t`, is given the inferred type `System.TimeSpan`, and the expression `t.TotalSeconds` is related to the return type of `f2`, inferring `Z` to be `double`.</span></span> <span data-ttu-id="abcc0-719">因此，此引動過程的結果屬於類型`double`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-719">Thus, the result of the invocation is of type `double`.</span></span>

#### <a name="type-inference-for-conversion-of-method-groups"></a><span data-ttu-id="abcc0-720">方法群組轉換的型別推斷</span><span class="sxs-lookup"><span data-stu-id="abcc0-720">Type inference for conversion of method groups</span></span>

<span data-ttu-id="abcc0-721">與泛型方法的呼叫相似，型別推斷也必須套用方法群組時`M`包含泛型方法會轉換成指定的委派型別`D`([方法群組轉換](conversions.md#method-group-conversions))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-721">Similar to calls of generic methods, type inference must also be applied when a method group `M` containing a generic method is converted to a given delegate type `D` ([Method group conversions](conversions.md#method-group-conversions)).</span></span> <span data-ttu-id="abcc0-722">指定方法</span><span class="sxs-lookup"><span data-stu-id="abcc0-722">Given a method</span></span>
```csharp
Tr M<X1...Xn>(T1 x1 ... Tm xm)
```
<span data-ttu-id="abcc0-723">和方法群組`M`指派給委派型別`D`的型別推斷工作是要尋找型別引數`S1...Sn`以便運算式：</span><span class="sxs-lookup"><span data-stu-id="abcc0-723">and the method group `M` being assigned to the delegate type `D` the task of type inference is to find type arguments `S1...Sn` so that the expression:</span></span>
```csharp
M<S1...Sn>
```
<span data-ttu-id="abcc0-724">會變成相容 ([委派宣告](delegates.md#delegate-declarations)) 與`D`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-724">becomes compatible ([Delegate declarations](delegates.md#delegate-declarations)) with `D`.</span></span>

<span data-ttu-id="abcc0-725">不同於一般方法呼叫的型別推斷演算法，在此情況下有只有引數*型別*，沒有引數*運算式*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-725">Unlike the type inference algorithm for generic method calls, in this case there are only argument *types*, no argument *expressions*.</span></span> <span data-ttu-id="abcc0-726">特別是，沒有匿名函式，因此不需要推斷的多個階段。</span><span class="sxs-lookup"><span data-stu-id="abcc0-726">In particular, there are no anonymous functions and hence no need for multiple phases of inference.</span></span>

<span data-ttu-id="abcc0-727">相反地，所有`Xi`會被視為*unfixed*，以及*下限推斷*進行*從*每個引數類型`Uj`的`D`*要*的對應參數類型`Tj`的`M`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-727">Instead, all `Xi` are considered *unfixed*, and a *lower-bound inference* is made *from* each argument type `Uj` of `D` *to* the corresponding parameter type `Tj` of `M`.</span></span> <span data-ttu-id="abcc0-728">如果任一`Xi`找不到任何界限、 型別推斷失敗。</span><span class="sxs-lookup"><span data-stu-id="abcc0-728">If for any of the `Xi` no bounds were found, type inference fails.</span></span> <span data-ttu-id="abcc0-729">否則，所有`Xi`會*固定*對應`Si`，是型別推斷的結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-729">Otherwise, all `Xi` are *fixed* to corresponding `Si`, which are the result of type inference.</span></span>

#### <a name="finding-the-best-common-type-of-a-set-of-expressions"></a><span data-ttu-id="abcc0-730">找出一組運算式的最常見類型</span><span class="sxs-lookup"><span data-stu-id="abcc0-730">Finding the best common type of a set of expressions</span></span>

<span data-ttu-id="abcc0-731">在某些情況下，常見的類型必須為一組運算式推斷。</span><span class="sxs-lookup"><span data-stu-id="abcc0-731">In some cases, a common type needs to be inferred for a set of expressions.</span></span> <span data-ttu-id="abcc0-732">在特定的隱含類型陣列的項目類型，使用匿名函式的傳回型別*區塊*主體可在這種方式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-732">In particular, the element types of implicitly typed arrays and the return types of anonymous functions with *block* bodies are found in this way.</span></span>

<span data-ttu-id="abcc0-733">直接易懂的方式，提供一組運算式`E1...Em`此推斷應該是相當於呼叫方法</span><span class="sxs-lookup"><span data-stu-id="abcc0-733">Intuitively, given a set of expressions `E1...Em` this inference should be equivalent to calling a method</span></span>
```csharp
Tr M<X>(X x1 ... X xm)
```
<span data-ttu-id="abcc0-734">使用`Ei`做為引數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-734">with the `Ei` as arguments.</span></span>

<span data-ttu-id="abcc0-735">更精確地說，推斷開頭*unfixed*類型變數`X`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-735">More precisely, the inference starts out with an *unfixed* type variable `X`.</span></span> <span data-ttu-id="abcc0-736">*輸出型別推斷*就會*從*每個`Ei`*來* `X`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-736">*Output type inferences* are then made *from* each `Ei` *to* `X`.</span></span> <span data-ttu-id="abcc0-737">最後，`X`已*修正*而且，如果成功，所產生的輸入`S`是運算式結果最常見的類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-737">Finally, `X` is *fixed* and, if successful, the resulting type `S` is the resulting best common type for the expressions.</span></span> <span data-ttu-id="abcc0-738">如果沒有這種`S`存在，這些運算式具有不常見的最佳類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-738">If no such `S` exists, the expressions have no best common type.</span></span>

### <a name="overload-resolution"></a><span data-ttu-id="abcc0-739">多載解析</span><span class="sxs-lookup"><span data-stu-id="abcc0-739">Overload resolution</span></span>

<span data-ttu-id="abcc0-740">多載解析會選取最佳函式成員叫用指定的引數清單和一組候選函式成員的繫結階段機制。</span><span class="sxs-lookup"><span data-stu-id="abcc0-740">Overload resolution is a binding-time mechanism for selecting the best function member to invoke given an argument list and a set of candidate function members.</span></span> <span data-ttu-id="abcc0-741">多載解析會選取要在 C# 中的下列不同內容中叫用的函式成員：</span><span class="sxs-lookup"><span data-stu-id="abcc0-741">Overload resolution selects the function member to invoke in the following distinct contexts within C#:</span></span>

*  <span data-ttu-id="abcc0-742">在名為方法引動過程*invocation_expression* ([方法引動過程](expressions.md#method-invocations))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-742">Invocation of a method named in an *invocation_expression* ([Method invocations](expressions.md#method-invocations)).</span></span>
*  <span data-ttu-id="abcc0-743">在中命名的執行個體建構函式的引動過程*object_creation_expression* ([物件建立運算式](expressions.md#object-creation-expressions))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-743">Invocation of an instance constructor named in an *object_creation_expression* ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span>
*  <span data-ttu-id="abcc0-744">透過索引子存取子的引動過程*element_access* ([項目存取](expressions.md#element-access))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-744">Invocation of an indexer accessor through an *element_access* ([Element access](expressions.md#element-access)).</span></span>
*  <span data-ttu-id="abcc0-745">在運算式中參考的預先定義或使用者定義運算子的引動過程 ([一元運算子多載解析](expressions.md#unary-operator-overload-resolution)並[二元運算子多載解析](expressions.md#binary-operator-overload-resolution))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-745">Invocation of a predefined or user-defined operator referenced in an expression ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution) and [Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)).</span></span>

<span data-ttu-id="abcc0-746">每個這些內容定義一組候選函式成員及引數清單在它自己的唯一方式，如上面所列各節中的詳細資料中所述。</span><span class="sxs-lookup"><span data-stu-id="abcc0-746">Each of these contexts defines the set of candidate function members and the list of arguments in its own unique way, as described in detail in the sections listed above.</span></span> <span data-ttu-id="abcc0-747">比方說，方法引動過程的候選項目集不包含標記的方法`override`([成員查閱](expressions.md#member-lookup))，以及基底類別中的方法不是候選項目是否適用於在衍生類別中的任何方法 ([方法引動過程](expressions.md#method-invocations))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-747">For example, the set of candidates for a method invocation does not include methods marked `override` ([Member lookup](expressions.md#member-lookup)), and methods in a base class are not candidates if any method in a derived class is applicable ([Method invocations](expressions.md#method-invocations)).</span></span>

<span data-ttu-id="abcc0-748">一旦識別候選函式成員 」 和 「 引數清單，選取最佳函式成員是在所有情況下相同的：</span><span class="sxs-lookup"><span data-stu-id="abcc0-748">Once the candidate function members and the argument list have been identified, the selection of the best function member is the same in all cases:</span></span>

*  <span data-ttu-id="abcc0-749">指定適用的候選函式成員的集合，最佳的函式成員，因為位於集。</span><span class="sxs-lookup"><span data-stu-id="abcc0-749">Given the set of applicable candidate function members, the best function member in that set is located.</span></span> <span data-ttu-id="abcc0-750">如果集合包含只有一個函式成員，該函式成員就會是最佳的函式成員。</span><span class="sxs-lookup"><span data-stu-id="abcc0-750">If the set contains only one function member, then that function member is the best function member.</span></span> <span data-ttu-id="abcc0-751">否則，最佳成員就是優於相對於指定的引數清單中，所有其他函式成員，前提是每個函式成員相較於使用中的規則中其他函式成員的一個函式成員[更好的函式成員](expressions.md#better-function-member)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-751">Otherwise, the best function member is the one function member that is better than all other function members with respect to the given argument list, provided that each function member is compared to all other function members using the rules in [Better function member](expressions.md#better-function-member).</span></span> <span data-ttu-id="abcc0-752">如果不是正好一個優於其他所有函式成員的函式成員，然後函式成員引動過程模稜兩可，並在繫結階段錯誤發生。</span><span class="sxs-lookup"><span data-stu-id="abcc0-752">If there is not exactly one function member that is better than all other function members, then the function member invocation is ambiguous and a binding-time error occurs.</span></span>

<span data-ttu-id="abcc0-753">下列章節會定義詞彙的確切意義***適用的函式成員***並***更好的函式成員***。</span><span class="sxs-lookup"><span data-stu-id="abcc0-753">The following sections define the exact meanings of the terms ***applicable function member*** and ***better function member***.</span></span>

#### <a name="applicable-function-member"></a><span data-ttu-id="abcc0-754">可用的函式成員</span><span class="sxs-lookup"><span data-stu-id="abcc0-754">Applicable function member</span></span>

<span data-ttu-id="abcc0-755">函式成員即為***適用的函式成員***引數清單而言`A`當所有下列條件成立：</span><span class="sxs-lookup"><span data-stu-id="abcc0-755">A function member is said to be an ***applicable function member*** with respect to an argument list `A` when all of the following are true:</span></span>

*  <span data-ttu-id="abcc0-756">在每個引數`A`對應至函式成員宣告中的參數中所述[相對應參數](expressions.md#corresponding-parameters)，沒有引數對應任何參數是選擇性的參數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-756">Each argument in `A` corresponds to a parameter in the function member declaration as described in [Corresponding parameters](expressions.md#corresponding-parameters), and any parameter to which no argument corresponds is an optional parameter.</span></span>
*  <span data-ttu-id="abcc0-757">中的每個引數`A`、 參數傳遞的引數的模式 (亦即，值`ref`，或`out`) 等同於相對應的參數，參數傳遞模式和</span><span class="sxs-lookup"><span data-stu-id="abcc0-757">For each argument in `A`, the parameter passing mode of the argument (i.e., value, `ref`, or `out`) is identical to the parameter passing mode of the corresponding parameter, and</span></span>
   *  <span data-ttu-id="abcc0-758">value 參數或參數陣列，隱含的轉換 ([隱含轉換](conversions.md#implicit-conversions)) 從引數存在之對應參數的型別或</span><span class="sxs-lookup"><span data-stu-id="abcc0-758">for a value parameter or a parameter array, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from the argument to the type of the corresponding parameter, or</span></span>
   *  <span data-ttu-id="abcc0-759">針對`ref`或`out`參數的引數類型是與對應參數的型別相同。</span><span class="sxs-lookup"><span data-stu-id="abcc0-759">for a `ref` or `out` parameter, the type of the argument is identical to the type of the corresponding parameter.</span></span> <span data-ttu-id="abcc0-760">畢竟`ref`或`out`參數是傳遞的引數的別名。</span><span class="sxs-lookup"><span data-stu-id="abcc0-760">After all, a `ref` or `out` parameter is an alias for the argument passed.</span></span>

<span data-ttu-id="abcc0-761">包含的參數陣列的函式成員函式成員是否適用上述規則，它稱為適用於其***一般形式***。</span><span class="sxs-lookup"><span data-stu-id="abcc0-761">For a function member that includes a parameter array, if the function member is applicable by the above rules, it is said to be applicable in its ***normal form***.</span></span> <span data-ttu-id="abcc0-762">如果不適用一般形式，包括參數陣列的函式成員，函式成員而可能適用於其***展開表單***:</span><span class="sxs-lookup"><span data-stu-id="abcc0-762">If a function member that includes a parameter array is not applicable in its normal form, the function member may instead be applicable in its ***expanded form***:</span></span>

*  <span data-ttu-id="abcc0-763">擴充的形式的建構函式成員宣告中的參數陣列取代零或更多值參數的項目型別參數的陣列，例如該引數清單中的引數數目`A`符合總計參數的數目。</span><span class="sxs-lookup"><span data-stu-id="abcc0-763">The expanded form is constructed by replacing the parameter array in the function member declaration with zero or more value parameters of the element type of the parameter array such that the number of arguments in the argument list `A` matches the total number of parameters.</span></span> <span data-ttu-id="abcc0-764">如果`A`函式成員宣告中有比固定的參數數目的引數數目少，無法建構函式成員的展開的表單，並因此並不適用。</span><span class="sxs-lookup"><span data-stu-id="abcc0-764">If `A` has fewer arguments than the number of fixed parameters in the function member declaration, the expanded form of the function member cannot be constructed and is thus not applicable.</span></span>
*  <span data-ttu-id="abcc0-765">否則，請展開的形式是適用於每個引數中如果`A`引數的參數傳遞模式完全相同之對應參數的參數傳遞模式和</span><span class="sxs-lookup"><span data-stu-id="abcc0-765">Otherwise, the expanded form is applicable if for each argument in `A` the parameter passing mode of the argument is identical to the parameter passing mode of the corresponding parameter, and</span></span>
   *  <span data-ttu-id="abcc0-766">固定的值參數，或建立的擴充，也就是隱含的轉換值參數 ([隱含轉換](conversions.md#implicit-conversions)) 之對應參數的型別引數的型別或</span><span class="sxs-lookup"><span data-stu-id="abcc0-766">for a fixed value parameter or a value parameter created by the expansion, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from the type of the argument to the type of the corresponding parameter, or</span></span>
   *  <span data-ttu-id="abcc0-767">針對`ref`或`out`參數的引數類型是與對應參數的型別相同。</span><span class="sxs-lookup"><span data-stu-id="abcc0-767">for a `ref` or `out` parameter, the type of the argument is identical to the type of the corresponding parameter.</span></span>

#### <a name="better-function-member"></a><span data-ttu-id="abcc0-768">更好的函式成員</span><span class="sxs-lookup"><span data-stu-id="abcc0-768">Better function member</span></span>

<span data-ttu-id="abcc0-769">為了判斷更好的函式成員，精簡的引數清單的會建構包含只是引數中的運算式本身原始的引數清單中出現的順序。</span><span class="sxs-lookup"><span data-stu-id="abcc0-769">For the purposes of determining the better function member, a stripped-down argument list A is constructed containing just the argument expressions themselves in the order they appear in the original argument list.</span></span>

<span data-ttu-id="abcc0-770">參數會列出每個候選函式成員建構方式如下：</span><span class="sxs-lookup"><span data-stu-id="abcc0-770">Parameter lists for each of the candidate function members are constructed in the following way:</span></span>

*  <span data-ttu-id="abcc0-771">只適用於展開的形式的函式成員時，會使用展開的形式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-771">The expanded form is used if the function member was applicable only in the expanded form.</span></span>
*  <span data-ttu-id="abcc0-772">沒有對應的引數的選擇性參數是從參數清單中移除</span><span class="sxs-lookup"><span data-stu-id="abcc0-772">Optional parameters with no corresponding arguments are removed from the parameter list</span></span>
*  <span data-ttu-id="abcc0-773">使這些備份在做為對應的引數的相同位置的引數清單中，已重新排列參數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-773">The parameters are reordered so that they occur at the same position as the corresponding argument in the argument list.</span></span>

<span data-ttu-id="abcc0-774">指定引數清單`A`與一組引數運算式`{E1, E2, ..., En}`和兩個可用的函式成員`Mp`並`Mq`與參數型別`{P1, P2, ..., Pn}`和`{Q1, Q2, ..., Qn}`，`Mp`會定義為***更好的函式成員***比`Mq`如果</span><span class="sxs-lookup"><span data-stu-id="abcc0-774">Given an argument list `A` with a set of argument expressions `{E1, E2, ..., En}` and two applicable function members `Mp` and `Mq` with parameter types `{P1, P2, ..., Pn}` and `{Q1, Q2, ..., Qn}`, `Mp` is defined to be a ***better function member*** than `Mq` if</span></span>

*  <span data-ttu-id="abcc0-775">每個引數，從隱含轉換`Ex`要`Qx`不是優於隱含方式轉換`Ex`到`Px`，和</span><span class="sxs-lookup"><span data-stu-id="abcc0-775">for each argument, the implicit conversion from `Ex` to `Qx` is not better than the implicit conversion from `Ex` to `Px`, and</span></span>
*  <span data-ttu-id="abcc0-776">至少一個引數，從轉換`Ex`要`Px`優於從轉換`Ex`至`Qx`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-776">for at least one argument, the conversion from `Ex` to `Px` is better than the conversion from `Ex` to `Qx`.</span></span>

<span data-ttu-id="abcc0-777">如果執行這項評估時`Mp`或`Mq`是適用於其展開的形式，則`Px`或`Qx`指的是參數清單的展開表單中的參數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-777">When performing this evaluation, if `Mp` or `Mq` is applicable in its expanded form, then `Px` or `Qx` refers to a parameter in the expanded form of the parameter list.</span></span>

<span data-ttu-id="abcc0-778">如果參數型別序列 `{P1, P2, ..., Pn}`和`{Q1, Q2, ..., Qn}`是相等的 (也就是每個`Pi`具有身分識別轉換至對應`Qi`)，套用下列繫結分行規則，以順序，以判斷好函式成員。</span><span class="sxs-lookup"><span data-stu-id="abcc0-778">In case the parameter type sequences `{P1, P2, ..., Pn}` and `{Q1, Q2, ..., Qn}` are equivalent (i.e. each `Pi` has an identity conversion to the corresponding `Qi`), the following tie-breaking rules are applied, in order, to determine the better function member.</span></span>

*  <span data-ttu-id="abcc0-779">如果`Mp`是一種非泛型方法和`Mq`是泛型方法，則`Mp`優於`Mq`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-779">If `Mp` is a non-generic method and `Mq` is a generic method, then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="abcc0-780">否則，如果`Mp`適用於一般形式及`Mq`已`params`陣列，且只適用於其展開的形式，然後`Mp`優於`Mq`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-780">Otherwise, if `Mp` is applicable in its normal form and `Mq` has a `params` array and is applicable only in its expanded form, then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="abcc0-781">否則，如果`Mp`具有多個宣告參數多於`Mq`，然後`Mp`優於`Mq`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-781">Otherwise, if `Mp` has more declared parameters than `Mq`, then `Mp` is better than `Mq`.</span></span> <span data-ttu-id="abcc0-782">這可能會發生這兩種方法有`params`陣列且只適用於其展開的形式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-782">This can occur if both methods have `params` arrays and are applicable only in their expanded forms.</span></span>
*  <span data-ttu-id="abcc0-783">否則如果所有參數的`Mp`有相對應的引數，而需要用來取代中至少一個選擇性參數的預設引數`Mq`再`Mp`優於`Mq`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-783">Otherwise if all parameters of `Mp` have a corresponding argument whereas default arguments need to be substituted for at least one optional parameter in `Mq` then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="abcc0-784">否則，如果`Mp`具有更特定的參數型別比`Mq`，然後`Mp`優於`Mq`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-784">Otherwise, if `Mp` has more specific parameter types than `Mq`, then `Mp` is better than `Mq`.</span></span> <span data-ttu-id="abcc0-785">可讓`{R1, R2, ..., Rn}`並`{S1, S2, ..., Sn}`代表的未具現化及未展開的參數型別`Mp`和`Mq`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-785">Let `{R1, R2, ..., Rn}` and `{S1, S2, ..., Sn}` represent the uninstantiated and unexpanded parameter types of `Mp` and `Mq`.</span></span> <span data-ttu-id="abcc0-786">`Mp`參數型別都比更明確`Mq`的每個參數，如果`Rx`不是較不特定比`Sx`，和至少一個參數，如`Rx`比更特定`Sx`:</span><span class="sxs-lookup"><span data-stu-id="abcc0-786">`Mp`'s parameter types are more specific than `Mq`'s if, for each parameter, `Rx` is not less specific than `Sx`, and, for at least one parameter, `Rx` is more specific than `Sx`:</span></span>
   *  <span data-ttu-id="abcc0-787">較不特定非類型參數的型別參數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-787">A type parameter is less specific than a non-type parameter.</span></span>
   *  <span data-ttu-id="abcc0-788">以遞迴方式，建構的型別是具體比另一個建構的類型 （具有型別引數數目相同） 至少一個類型引數是否更明確，而且沒有型別引數比對應的型別引數，在其他較不明確。</span><span class="sxs-lookup"><span data-stu-id="abcc0-788">Recursively, a constructed type is more specific than another constructed type (with the same number of type arguments) if at least one type argument is more specific and no type argument is less specific than the corresponding type argument in the other.</span></span>
   *  <span data-ttu-id="abcc0-789">如果第一個項目型別是比第二個項目類型更特定，陣列型別是 （具有相同的維度數目） 的另一個陣列型別比更明確。</span><span class="sxs-lookup"><span data-stu-id="abcc0-789">An array type is more specific than another array type (with the same number of dimensions) if the element type of the first is more specific than the element type of the second.</span></span>
*  <span data-ttu-id="abcc0-790">否則如果一個成員是提昇的運算子，另一個是提昇的運算子，非提昇的是較佳。</span><span class="sxs-lookup"><span data-stu-id="abcc0-790">Otherwise if one member is a non-lifted operator and  the other is a lifted operator, the non-lifted one is better.</span></span>
*  <span data-ttu-id="abcc0-791">否則，都不函式成員較佳。</span><span class="sxs-lookup"><span data-stu-id="abcc0-791">Otherwise, neither function member is better.</span></span>

#### <a name="better-conversion-from-expression"></a><span data-ttu-id="abcc0-792">更好轉換運算式</span><span class="sxs-lookup"><span data-stu-id="abcc0-792">Better conversion from expression</span></span>

<span data-ttu-id="abcc0-793">提供的隱含轉換`C1`可將運算式的轉換`E`型別`T1`，並隱含地轉換`C2`可將運算式的轉換`E`型別`T2`， `C1`已***更好的轉換***比`C2`如果`E`不完全符合`T2`，至少下列其中之一會裝載：</span><span class="sxs-lookup"><span data-stu-id="abcc0-793">Given an implicit conversion `C1` that converts from an expression `E` to a type `T1`, and an implicit conversion `C2` that converts from an expression `E` to a type `T2`, `C1` is a ***better conversion*** than `C2` if `E` does not exactly match `T2` and at least one of the following holds:</span></span>

* <span data-ttu-id="abcc0-794">`E` 完全符合`T1`([完全比對運算式](expressions.md#exactly-matching-expression))</span><span class="sxs-lookup"><span data-stu-id="abcc0-794">`E` exactly matches `T1` ([Exactly matching Expression](expressions.md#exactly-matching-expression))</span></span>
* <span data-ttu-id="abcc0-795">`T1` 是更好的轉換目標比`T2`([更好的轉換目標](expressions.md#better-conversion-target))</span><span class="sxs-lookup"><span data-stu-id="abcc0-795">`T1` is a better conversion target than `T2` ([Better conversion target](expressions.md#better-conversion-target))</span></span>

#### <a name="exactly-matching-expression"></a><span data-ttu-id="abcc0-796">完全比對運算式</span><span class="sxs-lookup"><span data-stu-id="abcc0-796">Exactly matching Expression</span></span>

<span data-ttu-id="abcc0-797">指定運算式`E`和型別`T`，`E`完全符合`T`如果下列其中一種保留：</span><span class="sxs-lookup"><span data-stu-id="abcc0-797">Given an expression `E` and a type `T`, `E` exactly matches `T` if one of the following holds:</span></span>

*  <span data-ttu-id="abcc0-798">`E` 具有類型`S`，且從身分識別轉換`S`至 `T`</span><span class="sxs-lookup"><span data-stu-id="abcc0-798">`E` has a type `S`, and an identity conversion exists from `S` to `T`</span></span>
*  <span data-ttu-id="abcc0-799">`E` 是匿名的函式`T`是委派型別`D`或運算式樹狀架構類型`Expression<D>`和下列其中一種保留：</span><span class="sxs-lookup"><span data-stu-id="abcc0-799">`E` is an anonymous function, `T` is either a delegate type `D` or an expression tree type `Expression<D>` and one of the following holds:</span></span>
   *  <span data-ttu-id="abcc0-800">推斷的傳回型別`X`存在於`E`的參數清單的內容中`D`([推斷傳回型別](expressions.md#inferred-return-type))，且從身分識別轉換`X`的傳回型別 `D`</span><span class="sxs-lookup"><span data-stu-id="abcc0-800">An inferred return type `X` exists for `E` in the context of the parameter list of `D` ([Inferred return type](expressions.md#inferred-return-type)), and an identity conversion exists from `X` to the return type of `D`</span></span>
   *  <span data-ttu-id="abcc0-801">任一`E`是非同步和`D`的傳回型別`Y`或`E`是非同步和`D`的傳回型別`Task<Y>`，而且下列其中一種保存：</span><span class="sxs-lookup"><span data-stu-id="abcc0-801">Either `E` is non-async and `D` has a return type `Y` or `E` is async and `D` has a return type `Task<Y>`, and one of the following holds:</span></span>
      * <span data-ttu-id="abcc0-802">主體`E`是一種運算式完全相符 `Y`</span><span class="sxs-lookup"><span data-stu-id="abcc0-802">The body of `E` is an expression that exactly matches `Y`</span></span>
      * <span data-ttu-id="abcc0-803">主體`E`是陳述式區塊，其中每個傳回陳述式會傳回的運算式也完全相符項目 `Y`</span><span class="sxs-lookup"><span data-stu-id="abcc0-803">The body of `E` is a statement block where every return statement returns an expression that exactly matches `Y`</span></span>

#### <a name="better-conversion-target"></a><span data-ttu-id="abcc0-804">更好的轉換目標</span><span class="sxs-lookup"><span data-stu-id="abcc0-804">Better conversion target</span></span>

<span data-ttu-id="abcc0-805">提供兩種不同類型`T1`並`T2`，`T1`是更好的轉換目標比`T2`如果沒有隱含的轉換，從`T2`至`T1`存在，而且至少下列其中之一會保存：</span><span class="sxs-lookup"><span data-stu-id="abcc0-805">Given two different types `T1` and `T2`, `T1` is a better conversion target than `T2` if no implicit conversion from `T2` to `T1` exists, and at least one of the following holds:</span></span>

*  <span data-ttu-id="abcc0-806">從的隱含轉換`T1`至`T2`存在</span><span class="sxs-lookup"><span data-stu-id="abcc0-806">An implicit conversion from `T1` to `T2` exists</span></span>
*  <span data-ttu-id="abcc0-807">`T1` 是委派型別`D1`或運算式樹狀架構型別`Expression<D1>`，`T2`是委派型別`D2`或運算式樹狀架構型別`Expression<D2>`，`D1`的傳回型別`S1`以及其中一個下列保留：</span><span class="sxs-lookup"><span data-stu-id="abcc0-807">`T1` is either a delegate type `D1` or an expression tree type `Expression<D1>`, `T2` is either a delegate type `D2` or an expression tree type `Expression<D2>`, `D1` has a return type `S1` and one of the following holds:</span></span>
   * <span data-ttu-id="abcc0-808">`D2` 傳回 void</span><span class="sxs-lookup"><span data-stu-id="abcc0-808">`D2` is void returning</span></span>
   * <span data-ttu-id="abcc0-809">`D2` 傳回型別`S2`，和`S1`是更好的轉換目標比 `S2`</span><span class="sxs-lookup"><span data-stu-id="abcc0-809">`D2` has a return type `S2`, and `S1` is a better conversion target than `S2`</span></span>
*  <span data-ttu-id="abcc0-810">`T1` 已`Task<S1>`，`T2`是`Task<S2>`，和`S1`是更好的轉換目標比 `S2`</span><span class="sxs-lookup"><span data-stu-id="abcc0-810">`T1` is `Task<S1>`, `T2` is `Task<S2>`, and `S1` is a better conversion target than `S2`</span></span>
*  <span data-ttu-id="abcc0-811">`T1` 是`S1`或`S1?`其中`S1`是帶正負號的整數類資料類型，以及`T2`會`S2`或`S2?`其中`S2`是不帶正負號的整數類資料類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-811">`T1` is `S1` or `S1?` where `S1` is a signed integral type, and `T2` is `S2` or `S2?` where `S2` is an unsigned integral type.</span></span> <span data-ttu-id="abcc0-812">尤其是：</span><span class="sxs-lookup"><span data-stu-id="abcc0-812">Specifically:</span></span>
   * <span data-ttu-id="abcc0-813">`S1` 已`sbyte`並`S2`是`byte`， `ushort`， `uint`，或 `ulong`</span><span class="sxs-lookup"><span data-stu-id="abcc0-813">`S1` is `sbyte` and `S2` is `byte`, `ushort`, `uint`, or `ulong`</span></span>
   * <span data-ttu-id="abcc0-814">`S1` 已`short`並`S2`是`ushort`， `uint`，或 `ulong`</span><span class="sxs-lookup"><span data-stu-id="abcc0-814">`S1` is `short` and `S2` is `ushort`, `uint`, or `ulong`</span></span>
   * <span data-ttu-id="abcc0-815">`S1` 已`int`並`S2`是`uint`，或 `ulong`</span><span class="sxs-lookup"><span data-stu-id="abcc0-815">`S1` is `int` and `S2` is `uint`, or `ulong`</span></span>
   * <span data-ttu-id="abcc0-816">`S1` 已`long`和`S2`是 `ulong`</span><span class="sxs-lookup"><span data-stu-id="abcc0-816">`S1` is `long` and `S2` is `ulong`</span></span>

#### <a name="overloading-in-generic-classes"></a><span data-ttu-id="abcc0-817">多載泛型類別中</span><span class="sxs-lookup"><span data-stu-id="abcc0-817">Overloading in generic classes</span></span>

<span data-ttu-id="abcc0-818">雖然宣告的簽章必須是唯一的就可以替代型別引數會導致相同的簽章。</span><span class="sxs-lookup"><span data-stu-id="abcc0-818">While signatures as declared must be unique, it is possible that substitution of type arguments results in identical signatures.</span></span> <span data-ttu-id="abcc0-819">在此情況下，上述的多載解析的繫結分行規則將會挑選最特定的成員。</span><span class="sxs-lookup"><span data-stu-id="abcc0-819">In such cases, the tie-breaking rules of overload resolution above will pick the most specific member.</span></span>

<span data-ttu-id="abcc0-820">下列範例會顯示有效和無效根據這個規則的多載：</span><span class="sxs-lookup"><span data-stu-id="abcc0-820">The following examples show overloads that are valid and invalid according to this rule:</span></span>

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

### <a name="compile-time-checking-of-dynamic-overload-resolution"></a><span data-ttu-id="abcc0-821">編譯時期檢查動態的多載解析</span><span class="sxs-lookup"><span data-stu-id="abcc0-821">Compile-time checking of dynamic overload resolution</span></span>

<span data-ttu-id="abcc0-822">最動態繫結的作業是在編譯時期未知組可能的候選項目進行解析。</span><span class="sxs-lookup"><span data-stu-id="abcc0-822">For most dynamically bound operations the set of possible candidates for resolution is unknown at compile-time.</span></span> <span data-ttu-id="abcc0-823">不過在某些情況下，候選集合是已知在編譯時間：</span><span class="sxs-lookup"><span data-stu-id="abcc0-823">In certain cases, however the candidate set is known at compile-time:</span></span>

*  <span data-ttu-id="abcc0-824">與動態引數的靜態方法呼叫</span><span class="sxs-lookup"><span data-stu-id="abcc0-824">Static method calls with dynamic arguments</span></span>
*  <span data-ttu-id="abcc0-825">其中接收者不是動態運算式的執行個體方法呼叫</span><span class="sxs-lookup"><span data-stu-id="abcc0-825">Instance method calls where the receiver is not a dynamic expression</span></span>
*  <span data-ttu-id="abcc0-826">索引子呼叫，其中接收者不是動態運算式</span><span class="sxs-lookup"><span data-stu-id="abcc0-826">Indexer calls where the receiver is not a dynamic expression</span></span>
*  <span data-ttu-id="abcc0-827">與動態引數的建構函式呼叫</span><span class="sxs-lookup"><span data-stu-id="abcc0-827">Constructor calls with dynamic arguments</span></span>

<span data-ttu-id="abcc0-828">在這些情況下，有限的編譯時間檢查會針對每個候選項目，以查看 是否其中任何可能無法套用在執行階段執行的。這項檢查包含下列步驟：</span><span class="sxs-lookup"><span data-stu-id="abcc0-828">In these cases a limited compile-time check is performed for each candidate to see if any of them could possibly apply at run-time.This check consists of the following steps:</span></span>

*  <span data-ttu-id="abcc0-829">部分型別推斷：任何類型引數，而不會直接或間接的型別引數`dynamic`使用的規則會推斷[型別推斷](expressions.md#type-inference)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-829">Partial type inference: Any type argument that does not depend directly or indirectly on an argument of type `dynamic` is inferred using the rules of [Type inference](expressions.md#type-inference).</span></span> <span data-ttu-id="abcc0-830">其餘的型別引數是未知的。</span><span class="sxs-lookup"><span data-stu-id="abcc0-830">The remaining type arguments are unknown.</span></span>
*  <span data-ttu-id="abcc0-831">部分的適用性檢查：根據檢查適用性[適用的函式成員](expressions.md#applicable-function-member)，但忽略其類型是未知的參數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-831">Partial applicability check: Applicability is checked according to [Applicable function member](expressions.md#applicable-function-member), but ignoring parameters whose types are unknown.</span></span>
*  <span data-ttu-id="abcc0-832">如果沒有候選人通過此測試，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-832">If no candidate passes this test, a compile-time error occurs.</span></span>

### <a name="function-member-invocation"></a><span data-ttu-id="abcc0-833">函式成員引動過程</span><span class="sxs-lookup"><span data-stu-id="abcc0-833">Function member invocation</span></span>

<span data-ttu-id="abcc0-834">本章節描述的程序發生在執行階段叫用的特定函式成員。</span><span class="sxs-lookup"><span data-stu-id="abcc0-834">This section describes the process that takes place at run-time to invoke a particular function member.</span></span> <span data-ttu-id="abcc0-835">它會假設階段繫結程序已經決定要叫用，可能是藉由套用多載解析，以一組候選函式成員的特定成員。</span><span class="sxs-lookup"><span data-stu-id="abcc0-835">It is assumed that a binding-time process has already determined the particular member to invoke, possibly by applying overload resolution to a set of candidate function members.</span></span>

<span data-ttu-id="abcc0-836">描述叫用處理序的目的而言，函式成員分成兩個類別：</span><span class="sxs-lookup"><span data-stu-id="abcc0-836">For purposes of describing the invocation process, function members are divided into two categories:</span></span>

*  <span data-ttu-id="abcc0-837">靜態函式成員。</span><span class="sxs-lookup"><span data-stu-id="abcc0-837">Static function members.</span></span> <span data-ttu-id="abcc0-838">這些是執行個體建構函式、 靜態方法，靜態屬性存取子和使用者定義的運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-838">These are instance constructors, static methods, static property accessors, and user-defined operators.</span></span> <span data-ttu-id="abcc0-839">靜態函式成員一律為非虛擬的。</span><span class="sxs-lookup"><span data-stu-id="abcc0-839">Static function members are always non-virtual.</span></span>
*  <span data-ttu-id="abcc0-840">執行個體函式成員。</span><span class="sxs-lookup"><span data-stu-id="abcc0-840">Instance function members.</span></span> <span data-ttu-id="abcc0-841">這些是執行個體方法 」、 「 執行個體屬性存取子和 「 索引子存取子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-841">These are instance methods, instance property accessors, and indexer accessors.</span></span> <span data-ttu-id="abcc0-842">執行個體函式成員非虛擬或虛擬的並一律在特定的執行個體上叫用。</span><span class="sxs-lookup"><span data-stu-id="abcc0-842">Instance function members are either non-virtual or virtual, and are always invoked on a particular instance.</span></span> <span data-ttu-id="abcc0-843">執行個體運算式，會計算執行個體，並內為該函式成員存取變得`this`([這項存取](expressions.md#this-access))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-843">The instance is computed by an instance expression, and it becomes accessible within the function member as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="abcc0-844">執行階段的處理函式成員引動過程包含下列步驟中，其中`M`是函式成員而且，如果`M`是執行個體成員，只有`E`是執行個體運算式：</span><span class="sxs-lookup"><span data-stu-id="abcc0-844">The run-time processing of a function member invocation consists of the following steps, where `M` is the function member and, if `M` is an instance member, `E` is the instance expression:</span></span>

*  <span data-ttu-id="abcc0-845">如果`M`是靜態函式成員：</span><span class="sxs-lookup"><span data-stu-id="abcc0-845">If `M` is a static function member:</span></span>
   * <span data-ttu-id="abcc0-846">當引數清單中所述，會評估[引數清單](expressions.md#argument-lists)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-846">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="abcc0-847">`M` 會叫用。</span><span class="sxs-lookup"><span data-stu-id="abcc0-847">`M` is invoked.</span></span>

*  <span data-ttu-id="abcc0-848">如果`M`執行個體函式成員宣告中*value_type*:</span><span class="sxs-lookup"><span data-stu-id="abcc0-848">If `M` is an instance function member declared in a *value_type*:</span></span>
   * <span data-ttu-id="abcc0-849">`E` 會評估。</span><span class="sxs-lookup"><span data-stu-id="abcc0-849">`E` is evaluated.</span></span> <span data-ttu-id="abcc0-850">如果此評估會產生例外狀況，則會不執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="abcc0-850">If this evaluation causes an exception, then no further steps are executed.</span></span>
   * <span data-ttu-id="abcc0-851">如果`E`不會分類為變數，則的暫存區域變數`E`的型別建立，而`E`指派給該變數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-851">If `E` is not classified as a variable, then a temporary local variable of `E`'s type is created and the value of `E` is assigned to that variable.</span></span> <span data-ttu-id="abcc0-852">`E` 然後分類為該暫存的本機變數的參考。</span><span class="sxs-lookup"><span data-stu-id="abcc0-852">`E` is then reclassified as a reference to that temporary local variable.</span></span> <span data-ttu-id="abcc0-853">暫存變數是那樣`this`內`M`，但不是在任何其他方式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-853">The temporary variable is accessible as `this` within `M`, but not in any other way.</span></span> <span data-ttu-id="abcc0-854">因此，只有當`E`是的則為 true 的變數是讓呼叫端所作的變更，`M`對`this`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-854">Thus, only when `E` is a true variable is it possible for the caller to observe the changes that `M` makes to `this`.</span></span>
   * <span data-ttu-id="abcc0-855">當引數清單中所述，會評估[引數清單](expressions.md#argument-lists)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-855">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="abcc0-856">`M` 會叫用。</span><span class="sxs-lookup"><span data-stu-id="abcc0-856">`M` is invoked.</span></span> <span data-ttu-id="abcc0-857">所參考的變數`E`變成所參考的變數`this`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-857">The variable referenced by `E` becomes the variable referenced by `this`.</span></span>

*  <span data-ttu-id="abcc0-858">如果`M`執行個體函式成員宣告中*reference_type*:</span><span class="sxs-lookup"><span data-stu-id="abcc0-858">If `M` is an instance function member declared in a *reference_type*:</span></span>
   * <span data-ttu-id="abcc0-859">`E` 會評估。</span><span class="sxs-lookup"><span data-stu-id="abcc0-859">`E` is evaluated.</span></span> <span data-ttu-id="abcc0-860">如果此評估會產生例外狀況，則會不執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="abcc0-860">If this evaluation causes an exception, then no further steps are executed.</span></span>
   * <span data-ttu-id="abcc0-861">當引數清單中所述，會評估[引數清單](expressions.md#argument-lists)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-861">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="abcc0-862">如果類型`E`是*value_type*，boxing 轉換 ([Boxing 轉換](types.md#boxing-conversions)) 會執行轉換`E`輸入`object`，和`E`會被視為必須是類型`object`在下列步驟。</span><span class="sxs-lookup"><span data-stu-id="abcc0-862">If the type of `E` is a *value_type*, a boxing conversion ([Boxing conversions](types.md#boxing-conversions)) is performed to convert `E` to type `object`, and `E` is considered to be of type `object` in the following steps.</span></span> <span data-ttu-id="abcc0-863">在此情況下，`M`僅可屬於`System.Object`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-863">In this case, `M` could only be a member of `System.Object`.</span></span>
   * <span data-ttu-id="abcc0-864">值`E`檢查為有效。</span><span class="sxs-lookup"><span data-stu-id="abcc0-864">The value of `E` is checked to be valid.</span></span> <span data-ttu-id="abcc0-865">如果值`E`已`null`、`System.NullReferenceException`就會擲回，而且會執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="abcc0-865">If the value of `E` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
   * <span data-ttu-id="abcc0-866">決定要叫用函式成員實作：</span><span class="sxs-lookup"><span data-stu-id="abcc0-866">The function member implementation to invoke is determined:</span></span>
     * <span data-ttu-id="abcc0-867">如果繫結階段類型`E`是一種介面，來叫用的函式成員是實作`M`所參考的執行個體的執行階段類型提供`E`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-867">If the binding-time type of `E` is an interface, the function member to invoke is the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span> <span data-ttu-id="abcc0-868">此函式成員由套用的介面對應規則 ([介面對應](interfaces.md#interface-mapping)) 來判斷實作`M`所參考的執行個體的執行階段類型提供`E`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-868">This function member is determined by applying the interface mapping rules ([Interface mapping](interfaces.md#interface-mapping)) to determine the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span>
     * <span data-ttu-id="abcc0-869">否則，如果`M`是虛擬函式成員，來叫用的函式成員是實作`M`所參考的執行個體的執行階段類型提供`E`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-869">Otherwise, if `M` is a virtual function member, the function member to invoke is the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span> <span data-ttu-id="abcc0-870">此函式成員由套用規則來決定最具衍生性的實作 ([虛擬方法](classes.md#virtual-methods)) 的`M`相對於所參考的執行個體的執行階段類型`E`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-870">This function member is determined by applying the rules for determining the most derived implementation ([Virtual methods](classes.md#virtual-methods)) of `M` with respect to the run-time type of the instance referenced by `E`.</span></span>
     * <span data-ttu-id="abcc0-871">否則，請`M`非虛擬函式成員，並叫用的函式成員`M`本身。</span><span class="sxs-lookup"><span data-stu-id="abcc0-871">Otherwise, `M` is a non-virtual function member, and the function member to invoke is `M` itself.</span></span>
   * <span data-ttu-id="abcc0-872">在上述步驟中決定的成員函式實作會叫用。</span><span class="sxs-lookup"><span data-stu-id="abcc0-872">The function member implementation determined in the step above is invoked.</span></span> <span data-ttu-id="abcc0-873">所參考的物件`E`變成所參考的物件`this`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-873">The object referenced by `E` becomes the object referenced by `this`.</span></span>

#### <a name="invocations-on-boxed-instances"></a><span data-ttu-id="abcc0-874">經過 boxing 處理的執行個體上的引動過程</span><span class="sxs-lookup"><span data-stu-id="abcc0-874">Invocations on boxed instances</span></span>

<span data-ttu-id="abcc0-875">函式成員中實作*value_type*透過經過 boxing 處理的執行個體，就可以叫用*value_type*在下列情況：</span><span class="sxs-lookup"><span data-stu-id="abcc0-875">A function member implemented in a *value_type* can be invoked through a boxed instance of that *value_type* in the following situations:</span></span>

*  <span data-ttu-id="abcc0-876">函式成員時`override`的方法繼承自型別的`object`，並叫用類型的執行個體運算式透過`object`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-876">When the function member is an `override` of a method inherited from type `object` and is invoked through an instance expression of type `object`.</span></span>
*  <span data-ttu-id="abcc0-877">當函式成員的是實作介面函式成員的位置，並透過執行個體運算式的叫用*interface_type*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-877">When the function member is an implementation of an interface function member and is invoked through an instance expression of an *interface_type*.</span></span>
*  <span data-ttu-id="abcc0-878">透過委派叫用函式成員時。</span><span class="sxs-lookup"><span data-stu-id="abcc0-878">When the function member is invoked through a delegate.</span></span>

<span data-ttu-id="abcc0-879">在這些情況下，已封裝的執行個體視為包含的變數*value_type*，而這個變數會變成所參考的變數`this`內函式成員引動過程。</span><span class="sxs-lookup"><span data-stu-id="abcc0-879">In these situations, the boxed instance is considered to contain a variable of the *value_type*, and this variable becomes the variable referenced by `this` within the function member invocation.</span></span> <span data-ttu-id="abcc0-880">特別是，這表示，已封裝的執行個體上叫用函式成員時，可能會修改已封裝的執行個體中包含的值的函式成員。</span><span class="sxs-lookup"><span data-stu-id="abcc0-880">In particular, this means that when a function member is invoked on a boxed instance, it is possible for the function member to modify the value contained in the boxed instance.</span></span>

## <a name="primary-expressions"></a><span data-ttu-id="abcc0-881">主要運算式</span><span class="sxs-lookup"><span data-stu-id="abcc0-881">Primary expressions</span></span>

<span data-ttu-id="abcc0-882">主要運算式包含最簡單的形式的運算式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-882">Primary expressions include the simplest forms of expressions.</span></span>

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

<span data-ttu-id="abcc0-883">主要運算式分屬*array_creation_expression*s 並*primary_no_array_creation_expression*s。</span><span class="sxs-lookup"><span data-stu-id="abcc0-883">Primary expressions are divided between *array_creation_expression*s and *primary_no_array_creation_expression*s.</span></span> <span data-ttu-id="abcc0-884">如此一來，將陣列建立運算式，而非列出的其他簡單運算式的表單，以及可讓這類不允許可能令人混淆的程式碼的文法</span><span class="sxs-lookup"><span data-stu-id="abcc0-884">Treating array-creation-expression in this way, rather than listing it along with the other simple expression forms, enables the grammar to disallow potentially confusing code such as</span></span>
```csharp
object o = new int[3][1];
```
<span data-ttu-id="abcc0-885">這會否則會被解譯為</span><span class="sxs-lookup"><span data-stu-id="abcc0-885">which would otherwise be interpreted as</span></span>
```csharp
object o = (new int[3])[1];
```

### <a name="literals"></a><span data-ttu-id="abcc0-886">常值</span><span class="sxs-lookup"><span data-stu-id="abcc0-886">Literals</span></span>

<span data-ttu-id="abcc0-887">A *primary_expression*這是組成*常值*([常值](lexical-structure.md#literals)) 會分類為值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-887">A *primary_expression* that consists of a *literal* ([Literals](lexical-structure.md#literals)) is classified as a value.</span></span>


### <a name="interpolated-strings"></a><span data-ttu-id="abcc0-888">插入字串</span><span class="sxs-lookup"><span data-stu-id="abcc0-888">Interpolated strings</span></span>

<span data-ttu-id="abcc0-889">*Interpolated_string_expression*組成`$`正負號的後面接著定期或逐字字串常值，其中漏洞，分隔`{`和`}`、 括住的運算式和格式化規格。</span><span class="sxs-lookup"><span data-stu-id="abcc0-889">An *interpolated_string_expression* consists of a `$` sign followed by a regular or verbatim string literal, wherein holes, delimited by `{` and `}`, enclose expressions and formatting specifications.</span></span> <span data-ttu-id="abcc0-890">字串插值的運算式是結果*interpolated_string_literal* ，具有已分割成個別的語彙基元中所述[插補字串常值](lexical-structure.md#interpolated-string-literals)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-890">An interpolated string expression is the result of an *interpolated_string_literal* that has been broken up into individual tokens, as described in [Interpolated string literals](lexical-structure.md#interpolated-string-literals).</span></span>

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

<span data-ttu-id="abcc0-891">*Constant_expression*插補中必須有隱含轉換成`int`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-891">The *constant_expression* in an interpolation must have an implicit conversion to `int`.</span></span>

<span data-ttu-id="abcc0-892">*Interpolated_string_expression*會分類為值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-892">An *interpolated_string_expression* is classified as a value.</span></span> <span data-ttu-id="abcc0-893">如果立即轉換成`System.IFormattable`或是`System.FormattableString`隱含的字串插值轉換 ([隱含插補字串轉換](conversions.md#implicit-interpolated-string-conversions))，字串插值的運算式有該型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-893">If it is immediately converted to `System.IFormattable` or `System.FormattableString` with an implicit interpolated string conversion ([Implicit interpolated string conversions](conversions.md#implicit-interpolated-string-conversions)), the interpolated string expression has that type.</span></span> <span data-ttu-id="abcc0-894">否則，它具有類型`string`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-894">Otherwise, it has the type `string`.</span></span>

<span data-ttu-id="abcc0-895">字串插值的類型是否`System.IFormattable`或是`System.FormattableString`，意思是呼叫`System.Runtime.CompilerServices.FormattableStringFactory.Create`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-895">If the type of an interpolated string is `System.IFormattable` or `System.FormattableString`, the meaning is a call to `System.Runtime.CompilerServices.FormattableStringFactory.Create`.</span></span> <span data-ttu-id="abcc0-896">如果類型是`string`，運算式的意義是呼叫`string.Format`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-896">If the type is `string`, the meaning of the expression is a call to `string.Format`.</span></span> <span data-ttu-id="abcc0-897">在這兩種情況下，呼叫的引數清單包含格式字串常值預留位置的每個插補，與每個對應到預留位置的運算式的引數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-897">In both cases, the argument list of the call consists of a format string literal with placeholders for each interpolation, and an argument for each expression corresponding to the place holders.</span></span>

<span data-ttu-id="abcc0-898">格式字串常值的建構如下，其中`N`中的內插補點數目*interpolated_string_expression*:</span><span class="sxs-lookup"><span data-stu-id="abcc0-898">The format string literal is constructed as follows, where `N` is the number of interpolations in the *interpolated_string_expression*:</span></span>

*  <span data-ttu-id="abcc0-899">如果*interpolated_regular_string_whole*該*interpolated_verbatim_string_whole*遵循`$`登入，則格式字串常值是該語彙基元。</span><span class="sxs-lookup"><span data-stu-id="abcc0-899">If an *interpolated_regular_string_whole* or an *interpolated_verbatim_string_whole* follows the `$` sign, then the format string literal is that token.</span></span>
*  <span data-ttu-id="abcc0-900">否則，格式字串常值所組成：</span><span class="sxs-lookup"><span data-stu-id="abcc0-900">Otherwise, the format string literal consists of:</span></span> 
   *  <span data-ttu-id="abcc0-901">第一個*interpolated_regular_string_start*或*interpolated_verbatim_string_start*</span><span class="sxs-lookup"><span data-stu-id="abcc0-901">First the *interpolated_regular_string_start* or *interpolated_verbatim_string_start*</span></span>
   *  <span data-ttu-id="abcc0-902">然後針對每一個數字`I`從`0`到`N-1`:</span><span class="sxs-lookup"><span data-stu-id="abcc0-902">Then for each number `I` from `0` to `N-1`:</span></span> 
      * <span data-ttu-id="abcc0-903">十進位表示法 `I`</span><span class="sxs-lookup"><span data-stu-id="abcc0-903">The decimal representation of `I`</span></span>
      * <span data-ttu-id="abcc0-904">之後，如果對應*插補*已*constant_expression*，則`,`（逗號） 後面的值的十進位表示法*constant_expression*</span><span class="sxs-lookup"><span data-stu-id="abcc0-904">Then, if the corresponding *interpolation* has a *constant_expression*, a `,` (comma) followed by the decimal representation of the value of the *constant_expression*</span></span>
      * <span data-ttu-id="abcc0-905">然後*interpolated_regular_string_mid*， *interpolated_regular_string_end*， *interpolated_verbatim_string_mid*或*interpolated_verbatim_string_end*緊接著對應的插補。</span><span class="sxs-lookup"><span data-stu-id="abcc0-905">Then the *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* or *interpolated_verbatim_string_end* immediately following the corresponding interpolation.</span></span>

<span data-ttu-id="abcc0-906">後續的引數是只要*運算式*從*內插補點*（如果有的話），在順序中。</span><span class="sxs-lookup"><span data-stu-id="abcc0-906">The subsequent arguments are simply the *expressions* from the *interpolations* (if any), in order.</span></span>

<span data-ttu-id="abcc0-907">TODO： 範例。</span><span class="sxs-lookup"><span data-stu-id="abcc0-907">TODO: examples.</span></span>


### <a name="simple-names"></a><span data-ttu-id="abcc0-908">簡單名稱</span><span class="sxs-lookup"><span data-stu-id="abcc0-908">Simple names</span></span>

<span data-ttu-id="abcc0-909">A *simple_name*識別項，並且選擇性地加型別引數清單所組成：</span><span class="sxs-lookup"><span data-stu-id="abcc0-909">A *simple_name* consists of an identifier, optionally followed by a type argument list:</span></span>

```antlr
simple_name
    : identifier type_argument_list?
    ;
```

<span data-ttu-id="abcc0-910">A *simple_name*是其中一種形式`I`，或格式`I<A1,...,Ak>`，其中`I`是以單一識別項和`<A1,...,Ak>`是選擇性*type_argument_list*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-910">A *simple_name* is either of the form `I` or of the form `I<A1,...,Ak>`, where `I` is a single identifier and `<A1,...,Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="abcc0-911">若未*type_argument_list*會指定，請考慮`K`為零。</span><span class="sxs-lookup"><span data-stu-id="abcc0-911">When no *type_argument_list* is specified, consider `K` to be zero.</span></span> <span data-ttu-id="abcc0-912">*Simple_name*評估和分類，如下所示：</span><span class="sxs-lookup"><span data-stu-id="abcc0-912">The *simple_name* is evaluated and classified as follows:</span></span>

*  <span data-ttu-id="abcc0-913">如果`K`為零， *simple_name*內會出現*區塊*如果*區塊*的 (或為封入*區塊*的) 本機變數宣告空間 ([宣告](basic-concepts.md#declarations)) 包含本機變數、 參數或常數名稱 `I`，然後在*simple_name*參考該區域變數參數或常數，而分類為變數或值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-913">If `K` is zero and the *simple_name* appears within a *block* and if the *block*'s (or an enclosing *block*'s) local variable declaration space ([Declarations](basic-concepts.md#declarations)) contains a local variable, parameter or constant with name `I`, then the *simple_name* refers to that local variable, parameter or constant and is classified as a variable or value.</span></span>
*  <span data-ttu-id="abcc0-914">如果`K`為零， *simple_name*出現在泛型方法宣告的主體內，如果宣告包含名稱的型別參數 `I`，則*simple_name*指的是該型別參數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-914">If `K` is zero and the *simple_name* appears within the body of a generic method declaration and if that declaration includes a type parameter with name `I`, then the *simple_name* refers to that type parameter.</span></span>
*  <span data-ttu-id="abcc0-915">否則，對於每個執行個體型別 `T`([執行個體類型](classes.md#the-instance-type))，開始立即封入型別宣告的執行個體類型，並繼續進行的每個封入類別或結構的執行個體類型宣告 （如果有的話）：</span><span class="sxs-lookup"><span data-stu-id="abcc0-915">Otherwise, for each instance type `T` ([The instance type](classes.md#the-instance-type)), starting with the instance type of the immediately enclosing type declaration and continuing with the instance type of each enclosing class or struct declaration (if any):</span></span>
   *  <span data-ttu-id="abcc0-916">如果`K`是零，宣告`T`包含名稱的型別參數 `I`，然後在*simple_name*指的是該型別參數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-916">If `K` is zero and the declaration of `T` includes a type parameter with name `I`, then the *simple_name* refers to that type parameter.</span></span>
   *  <span data-ttu-id="abcc0-917">否則，如果成員查閱 ([成員查閱](expressions.md#member-lookup)) 的`I`中`T`具有`K` 型別引數會產生相符項目：</span><span class="sxs-lookup"><span data-stu-id="abcc0-917">Otherwise, if a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `T` with `K` type arguments produces a match:</span></span>
      * <span data-ttu-id="abcc0-918">如果`T`與其直接封入類別或結構類型的執行個體類型和查閱找出一或多個方法，結果是具有相關聯的執行個體運算式的方法群組`this`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-918">If `T` is the instance type of the immediately enclosing class or struct type and the lookup identifies one or more methods, the result is a method group with an associated instance expression of `this`.</span></span> <span data-ttu-id="abcc0-919">如果指定型別引數清單，則用來呼叫泛型方法 ([方法引動過程](expressions.md#method-invocations))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-919">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
      * <span data-ttu-id="abcc0-920">否則，如果`T`是立即封入類別或結構類型的執行個體類型，如果查閱識別執行個體成員，且參考發生的執行個體建構函式、 執行個體方法或執行個體存取子的主體內結果是成員存取相同 ([成員存取](expressions.md#member-access)) 格式的`this.I`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-920">Otherwise, if `T` is the instance type of the immediately enclosing class or struct type, if the lookup identifies an instance member, and if the reference occurs within the body of an instance constructor, an instance method, or an instance accessor, the result is the same as a member access ([Member access](expressions.md#member-access)) of the form `this.I`.</span></span> <span data-ttu-id="abcc0-921">這只會發生時`K`為零。</span><span class="sxs-lookup"><span data-stu-id="abcc0-921">This can only happen when `K` is zero.</span></span>
      * <span data-ttu-id="abcc0-922">否則，結果就是成員存取相同 ([成員存取](expressions.md#member-access)) 的表單`T.I`或`T.I<A1,...,Ak>`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-922">Otherwise, the result is the same as a member access ([Member access](expressions.md#member-access)) of the form `T.I` or `T.I<A1,...,Ak>`.</span></span> <span data-ttu-id="abcc0-923">在此情況下，它會繫結時間錯誤*simple_name*參考執行個體成員。</span><span class="sxs-lookup"><span data-stu-id="abcc0-923">In this case, it is a binding-time error for the *simple_name* to refer to an instance member.</span></span>

*  <span data-ttu-id="abcc0-924">否則，對於每個命名空間 `N`，從命名空間，其中*simple_name*發生，請繼續進行與每個封入命名空間 （如果有的話），和全域命名空間中，做為結尾，則下列步驟評估直到找到實體為止：</span><span class="sxs-lookup"><span data-stu-id="abcc0-924">Otherwise, for each namespace `N`, starting with the namespace in which the *simple_name* occurs, continuing with each enclosing namespace (if any), and ending with the global namespace, the following steps are evaluated until an entity is located:</span></span>
   *  <span data-ttu-id="abcc0-925">如果`K`為零並`I`中的命名空間名稱 `N`，然後：</span><span class="sxs-lookup"><span data-stu-id="abcc0-925">If `K` is zero and `I` is the name of a namespace in `N`, then:</span></span>
      * <span data-ttu-id="abcc0-926">如果位置所在*simple_name*就會發生加上命名空間宣告`N`且命名空間宣告包含*extern_alias_directive*或*using_alias_directive* ，將名稱產生關聯 `I`與命名空間或類型，則*simple_name*模稜兩可，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-926">If the location where the *simple_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *simple_name* is ambiguous and a compile-time error occurs.</span></span>
      * <span data-ttu-id="abcc0-927">否則，請*simple_name*名為命名空間是指`I`在`N`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-927">Otherwise, the *simple_name* refers to the namespace named `I` in `N`.</span></span>
   *  <span data-ttu-id="abcc0-928">否則，如果`N`包含可存取的型別具有名稱 `I`並`K` 型別參數，然後：</span><span class="sxs-lookup"><span data-stu-id="abcc0-928">Otherwise, if `N` contains an accessible type having name `I` and `K` type parameters, then:</span></span>
      * <span data-ttu-id="abcc0-929">如果`K`是零，位置所在*simple_name*就會發生加上命名空間宣告`N`和命名空間宣告包含*extern_alias_directive*或*using_alias_directive* ，將名稱產生關聯 `I`命名空間或類型，則*simple_name*模稜兩可並發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-929">If `K` is zero and the location where the *simple_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *simple_name* is ambiguous and a compile-time error occurs.</span></span>
      * <span data-ttu-id="abcc0-930">否則，請*namespace_or_type_name*指的是使用指定的型別引數建構的類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-930">Otherwise, the *namespace_or_type_name* refers to the type constructed with the given type arguments.</span></span>
   *  <span data-ttu-id="abcc0-931">否則，如果位置所在*simple_name*就會發生加上命名空間宣告 `N`:</span><span class="sxs-lookup"><span data-stu-id="abcc0-931">Otherwise, if the location where the *simple_name* occurs is enclosed by a namespace declaration for `N`:</span></span>
      * <span data-ttu-id="abcc0-932">如果`K`為零，而命名空間宣告包含*extern_alias_directive*或是*using_alias_directive*名稱建立關聯的 `I`與匯入的命名空間或型別，則*simple_name*參考到該命名空間或型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-932">If `K` is zero and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with an imported namespace or type, then the *simple_name* refers to that namespace or type.</span></span>
      * <span data-ttu-id="abcc0-933">否則，如果所匯入的命名空間和類型的宣告*using_namespace_directive*s 並*using_static_directive*的命名空間宣告包含一個可存取的型別或不可擴充的靜態成員具有名稱 `I`並`K` 型別參數，則*simple_name*參考該類型或成員建構具有指定的型別引數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-933">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_static_directive*s of the namespace declaration contain exactly one accessible type or non-extension static member having name `I` and `K` type parameters, then the *simple_name* refers to that type or member constructed with the given type arguments.</span></span>
      * <span data-ttu-id="abcc0-934">否則，如果匯入的命名空間和類型*using_namespace_directive*的命名空間宣告包含一個以上可存取的型別或非擴充方法的靜態成員具有名稱 `I`並`K` 型別參數，則*simple_name*模稜兩可並發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-934">Otherwise, if the namespaces and types imported by the *using_namespace_directive*s of the namespace declaration contain more than one accessible type or non-extension-method static member having name `I` and `K` type parameters, then the *simple_name* is ambiguous and an error occurs.</span></span>

   <span data-ttu-id="abcc0-935">請注意，這整個步驟是在處理對應的步驟完全平行*namespace_or_type_name* ([命名空間和型別名稱](basic-concepts.md#namespace-and-type-names))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-935">Note that this entire step is exactly parallel to the corresponding step in the processing of a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)).</span></span>

*  <span data-ttu-id="abcc0-936">否則，請*simple_name*是未定義，而且會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-936">Otherwise, the *simple_name* is undefined and a compile-time error occurs.</span></span>


### <a name="parenthesized-expressions"></a><span data-ttu-id="abcc0-937">括號運算式</span><span class="sxs-lookup"><span data-stu-id="abcc0-937">Parenthesized expressions</span></span>

<span data-ttu-id="abcc0-938">A *parenthesized_expression*組成*運算式*括號括住。</span><span class="sxs-lookup"><span data-stu-id="abcc0-938">A *parenthesized_expression* consists of an *expression* enclosed in parentheses.</span></span>

```antlr
parenthesized_expression
    : '(' expression ')'
    ;
```

<span data-ttu-id="abcc0-939">A *parenthesized_expression*評估藉由評估*運算式*括號內。</span><span class="sxs-lookup"><span data-stu-id="abcc0-939">A *parenthesized_expression* is evaluated by evaluating the *expression* within the parentheses.</span></span> <span data-ttu-id="abcc0-940">如果*運算式*括號內代表命名空間或型別，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-940">If the *expression* within the parentheses denotes a namespace or type, a compile-time error occurs.</span></span> <span data-ttu-id="abcc0-941">否則，結果*parenthesized_expression*是包含評估的結果*運算式*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-941">Otherwise, the result of the *parenthesized_expression* is the result of the evaluation of the contained *expression*.</span></span>

### <a name="member-access"></a><span data-ttu-id="abcc0-942">成員存取</span><span class="sxs-lookup"><span data-stu-id="abcc0-942">Member access</span></span>

<span data-ttu-id="abcc0-943">A *member_access*組成*primary_expression*，則*predefined_type*，或*qualified_alias_member*，後面接著"`.`「 語彙基元，後面接著*識別項*，選擇性地接著*type_argument_list*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-943">A *member_access* consists of a *primary_expression*, a *predefined_type*, or a *qualified_alias_member*, followed by a "`.`" token, followed by an *identifier*, optionally followed by a *type_argument_list*.</span></span>

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

<span data-ttu-id="abcc0-944">*Qualified_alias_member*中所定義的生產環境[命名空間別名限定詞](namespaces.md#namespace-alias-qualifiers)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-944">The *qualified_alias_member* production is defined in [Namespace alias qualifiers](namespaces.md#namespace-alias-qualifiers).</span></span>

<span data-ttu-id="abcc0-945">A *member_access*是其中一種形式`E.I`，或格式`E.I<A1, ..., Ak>`，其中`E`是主要運算式`I`是以單一識別項和`<A1, ..., Ak>`是選擇性*type_argument_list*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-945">A *member_access* is either of the form `E.I` or of the form `E.I<A1, ..., Ak>`, where `E` is a primary-expression, `I` is a single identifier and `<A1, ..., Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="abcc0-946">若未*type_argument_list*會指定，請考慮`K`為零。</span><span class="sxs-lookup"><span data-stu-id="abcc0-946">When no *type_argument_list* is specified, consider `K` to be zero.</span></span>

<span data-ttu-id="abcc0-947">A *member_access*具有*primary_expression*型別的`dynamic`動態繫結 ([動態繫結](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-947">A *member_access* with a *primary_expression* of type `dynamic` is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="abcc0-948">在此情況下，編譯器會將成員存取分類為類型的屬性存取`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-948">In this case the compiler classifies the member access as a property access of type `dynamic`.</span></span> <span data-ttu-id="abcc0-949">規則來判斷的意義*member_access*接著會在執行階段，而不編譯時期類型使用的執行階段型別套用*primary_expression*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-949">The rules below to determine the meaning of the *member_access* are then applied at run-time, using the run-time type instead of the compile-time type of the *primary_expression*.</span></span> <span data-ttu-id="abcc0-950">如果此執行階段分類會導致方法群組，則必須是成員存取*primary_expression*的*invocation_expression*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-950">If this run-time classification leads to a method group, then the member access must be the *primary_expression* of an *invocation_expression*.</span></span>

<span data-ttu-id="abcc0-951">*Member_access*評估和分類，如下所示：</span><span class="sxs-lookup"><span data-stu-id="abcc0-951">The *member_access* is evaluated and classified as follows:</span></span>

*  <span data-ttu-id="abcc0-952">如果`K`為零並`E`是命名空間並`E`包含具有名稱的巢狀命名空間 `I`，則結果為該命名空間。</span><span class="sxs-lookup"><span data-stu-id="abcc0-952">If `K` is zero and `E` is a namespace and `E` contains a nested namespace with name `I`, then the result is that namespace.</span></span>
*  <span data-ttu-id="abcc0-953">否則，如果`E`是命名空間並`E`包含可存取的型別具有名稱 `I`並`K` 型別參數，則結果為使用指定的型別引數建構的型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-953">Otherwise, if `E` is a namespace and `E` contains an accessible type having name `I` and `K` type parameters, then the result is that type constructed with the given type arguments.</span></span>
*  <span data-ttu-id="abcc0-954">如果`E`是*predefined_type*或*primary_expression*分類為型別，如果`E`不是類型參數，而且如果成員查閱 ([成員查閱](expressions.md#member-lookup))`I`中`E`具有`K` 型別參數會產生相符項目，然後`E.I`評估和分類，如下所示：</span><span class="sxs-lookup"><span data-stu-id="abcc0-954">If `E` is a *predefined_type* or a *primary_expression* classified as a type, if `E` is not a type parameter, and if a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `E` with `K` type parameters produces a match, then `E.I` is evaluated and classified as follows:</span></span>
   *  <span data-ttu-id="abcc0-955">如果`I`識別類型，則結果為使用指定的型別引數建構的型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-955">If `I` identifies a type, then the result is that type constructed with the given type arguments.</span></span>
   *  <span data-ttu-id="abcc0-956">如果`I`識別一或多個方法，則結果為沒有相關聯的執行個體運算式的方法群組。</span><span class="sxs-lookup"><span data-stu-id="abcc0-956">If `I` identifies one or more methods, then the result is a method group with no associated instance expression.</span></span> <span data-ttu-id="abcc0-957">如果指定型別引數清單，則用來呼叫泛型方法 ([方法引動過程](expressions.md#method-invocations))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-957">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
   *  <span data-ttu-id="abcc0-958">如果`I`識別`static`屬性，則結果是沒有相關聯的執行個體運算式的屬性存取。</span><span class="sxs-lookup"><span data-stu-id="abcc0-958">If `I` identifies a `static` property, then the result is a property access with no associated instance expression.</span></span>
   *  <span data-ttu-id="abcc0-959">如果`I`識別`static`欄位：</span><span class="sxs-lookup"><span data-stu-id="abcc0-959">If `I` identifies a `static` field:</span></span>
      * <span data-ttu-id="abcc0-960">如果欄位是`readonly`和參考，就會發生的類別或結構中宣告的欄位，靜態建構函式之外，則結果為一個值，也就是靜態欄位的值 `I`在 `E`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-960">If the field is `readonly` and the reference occurs outside the static constructor of the class or struct in which the field is declared, then the result is a value, namely the value of the static field `I` in `E`.</span></span>
      * <span data-ttu-id="abcc0-961">否則，結果就是一個變數，也就是靜態欄位 `I`在 `E`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-961">Otherwise, the result is a variable, namely the static field `I` in `E`.</span></span>
   *  <span data-ttu-id="abcc0-962">如果`I`識別`static`事件：</span><span class="sxs-lookup"><span data-stu-id="abcc0-962">If `I` identifies a `static` event:</span></span>
      * <span data-ttu-id="abcc0-963">如果參考就會發生在類別或結構中宣告事件，並在宣告事件時未*event_accessor_declarations* ([事件](classes.md#events))，然後`E.I`完全處理如同`I`靜態欄位。</span><span class="sxs-lookup"><span data-stu-id="abcc0-963">If the reference occurs within the class or struct in which the event is declared, and the event was declared without *event_accessor_declarations* ([Events](classes.md#events)), then `E.I` is processed exactly as if `I` were a static field.</span></span>
      * <span data-ttu-id="abcc0-964">否則，結果就是事件存取沒有相關聯的執行個體的運算式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-964">Otherwise, the result is an event access with no associated instance expression.</span></span>
   *  <span data-ttu-id="abcc0-965">如果`I`識別常數，則結果為一個值，也就是該常數的值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-965">If `I` identifies a constant, then the result is a value, namely the value of that constant.</span></span>
    * <span data-ttu-id="abcc0-966">如果`I`識別列舉成員，則結果為一個值，也就是該列舉成員的值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-966">If `I` identifies an enumeration member, then the result is a value, namely the value of that enumeration member.</span></span>
    * <span data-ttu-id="abcc0-967">否則，`E.I`是無效的成員參考，則會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-967">Otherwise, `E.I` is an invalid member reference, and a compile-time error occurs.</span></span>
*  <span data-ttu-id="abcc0-968">如果`E`是屬性存取、 索引子存取、 變數或值的類型是 `T`，和成員查閱 ([成員查閱](expressions.md#member-lookup)) 的`I`中`T`具有`K`  型別引數會產生相符項目，然後`E.I`評估和分類，如下所示：</span><span class="sxs-lookup"><span data-stu-id="abcc0-968">If `E` is a property access, indexer access, variable, or value, the type of which is `T`, and a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `T` with `K` type arguments produces a match, then `E.I` is evaluated and classified as follows:</span></span>
   *  <span data-ttu-id="abcc0-969">首先，如果`E`是屬性或索引子存取，則屬性的值，或取得索引子存取 ([運算式的值](expressions.md#values-of-expressions)) 和`E`分類為值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-969">First, if `E` is a property or indexer access, then the value of the property or indexer access is obtained ([Values of expressions](expressions.md#values-of-expressions)) and `E` is reclassified as a value.</span></span>
   *  <span data-ttu-id="abcc0-970">如果`I`識別一或多個方法，則結果為方法群組有相關聯的執行個體運算式`E`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-970">If `I` identifies one or more methods, then the result is a method group with an associated instance expression of `E`.</span></span> <span data-ttu-id="abcc0-971">如果指定型別引數清單，則用來呼叫泛型方法 ([方法引動過程](expressions.md#method-invocations))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-971">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
   *  <span data-ttu-id="abcc0-972">如果`I`識別一個執行個體的屬性，</span><span class="sxs-lookup"><span data-stu-id="abcc0-972">If `I` identifies an instance property,</span></span>
      * <span data-ttu-id="abcc0-973">如果`E`已`this`，`I`識別自動實作的屬性 ([自動實作屬性](classes.md#automatically-implemented-properties)) 沒有 setter 和參考，就會發生的執行個體建構函式類別或結構的型別`T`，則結果為變數時，也就是隱藏的支援欄位所指定之自動屬性`I`執行個體中`T`藉由指定`this`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-973">If `E` is `this`, `I` identifies an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)) without a setter, and the reference occurs within an instance constructor for a class or struct type `T`, then the result is a variable, namely the hidden backing field for the auto-property given by `I` in the instance of `T` given by `this`.</span></span>
      * <span data-ttu-id="abcc0-974">否則，結果就是具有相關聯的執行個體運算式的屬性存取 `E`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-974">Otherwise, the result is a property access with an associated instance expression of `E`.</span></span>
   *  <span data-ttu-id="abcc0-975">如果`T`已*class_type*並`I`識別該執行個體欄位*class_type*:</span><span class="sxs-lookup"><span data-stu-id="abcc0-975">If `T` is a *class_type* and `I` identifies an instance field of that *class_type*:</span></span>
      * <span data-ttu-id="abcc0-976">如果值`E`已`null`，則會顯示`System.NullReferenceException`就會擲回。</span><span class="sxs-lookup"><span data-stu-id="abcc0-976">If the value of `E` is `null`, then a `System.NullReferenceException` is thrown.</span></span>
      * <span data-ttu-id="abcc0-977">否則，如果欄位是`readonly`和參考，就會發生在宣告的欄位，該類別的執行個體建構函式之外，則結果為一個值，也就是欄位的值 `I`中所參考的物件 `E`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-977">Otherwise, if the field is `readonly` and the reference occurs outside an instance constructor of the class in which the field is declared, then the result is a value, namely the value of the field `I` in the object referenced by `E`.</span></span>
      * <span data-ttu-id="abcc0-978">否則，結果就是一個變數，也就是欄位 `I`中所參考的物件 `E`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-978">Otherwise, the result is a variable, namely the field `I` in the object referenced by `E`.</span></span>
   *  <span data-ttu-id="abcc0-979">如果`T`已*struct_type*並`I`識別該執行個體欄位*struct_type*:</span><span class="sxs-lookup"><span data-stu-id="abcc0-979">If `T` is a *struct_type* and `I` identifies an instance field of that *struct_type*:</span></span>
      * <span data-ttu-id="abcc0-980">如果`E`是一個值，或如果欄位是`readonly`和參考，就會發生在宣告的欄位，該結構的執行個體建構函式之外，則結果為一個值，也就是欄位的值 `I`中所指定的結構執行個體 `E`.</span><span class="sxs-lookup"><span data-stu-id="abcc0-980">If `E` is a value, or if the field is `readonly` and the reference occurs outside an instance constructor of the struct in which the field is declared, then the result is a value, namely the value of the field `I` in the struct instance given by `E`.</span></span>
      * <span data-ttu-id="abcc0-981">否則，結果就是一個變數，也就是欄位 `I`中所指定的結構執行個體 `E`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-981">Otherwise, the result is a variable, namely the field `I` in the struct instance given by `E`.</span></span>
   *  <span data-ttu-id="abcc0-982">如果`I`識別執行個體的事件：</span><span class="sxs-lookup"><span data-stu-id="abcc0-982">If `I` identifies an instance event:</span></span>
      * <span data-ttu-id="abcc0-983">如果參考就會發生在類別或結構中宣告事件，並在宣告事件時未*event_accessor_declarations* ([事件](classes.md#events))，並為不會發生參考左手邊`+=`或是`-=`運算子，然後`E.I`完全處理如同`I`已執行個體欄位。</span><span class="sxs-lookup"><span data-stu-id="abcc0-983">If the reference occurs within the class or struct in which the event is declared, and the event was declared without *event_accessor_declarations* ([Events](classes.md#events)), and the reference does not occur as the left-hand side of a `+=` or `-=` operator, then `E.I` is processed exactly as if `I` was an instance field.</span></span>
      * <span data-ttu-id="abcc0-984">否則，結果就是具有相關聯的執行個體運算式的事件存取 `E`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-984">Otherwise, the result is an event access with an associated instance expression of `E`.</span></span>
*  <span data-ttu-id="abcc0-985">否則，嘗試處理`E.I`作為擴充方法引動過程 ([擴充方法引動過程](expressions.md#extension-method-invocations))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-985">Otherwise, an attempt is made to process `E.I` as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="abcc0-986">如果失敗，`E.I`是無效的成員參考，並在繫結階段錯誤發生。</span><span class="sxs-lookup"><span data-stu-id="abcc0-986">If this fails, `E.I` is an invalid member reference, and a binding-time error occurs.</span></span>

#### <a name="identical-simple-names-and-type-names"></a><span data-ttu-id="abcc0-987">相同簡單名稱和類型名稱</span><span class="sxs-lookup"><span data-stu-id="abcc0-987">Identical simple names and type names</span></span>

<span data-ttu-id="abcc0-988">在表單的成員存取`E.I`，如果`E`是單一的識別項，而且如果的意義`E`做為*simple_name* ([簡單名稱](expressions.md#simple-names)) 是常數、 欄位、 屬性，本機變數或參數類型與相同類型的意義`E`做為*type_name* ([命名空間和型別名稱](basic-concepts.md#namespace-and-type-names))，則這兩個可能的意思的`E`是允許使用。</span><span class="sxs-lookup"><span data-stu-id="abcc0-988">In a member access of the form `E.I`, if `E` is a single identifier, and if the meaning of `E` as a *simple_name* ([Simple names](expressions.md#simple-names)) is a constant, field, property, local variable, or parameter with the same type as the meaning of `E` as a *type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)), then both possible meanings of `E` are permitted.</span></span> <span data-ttu-id="abcc0-989">兩個可能的意思`E.I`永遠都不會模稜兩可，因為`I`一定必須是型別的成員`E`這兩種情況。</span><span class="sxs-lookup"><span data-stu-id="abcc0-989">The two possible meanings of `E.I` are never ambiguous, since `I` must necessarily be a member of the type `E` in both cases.</span></span> <span data-ttu-id="abcc0-990">換句話說，此規則僅允許存取的靜態成員和巢狀型別`E`在已經發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-990">In other words, the rule simply permits access to the static members and nested types of `E` where a compile-time error would otherwise have occurred.</span></span> <span data-ttu-id="abcc0-991">例如: </span><span class="sxs-lookup"><span data-stu-id="abcc0-991">For example:</span></span>
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

#### <a name="grammar-ambiguities"></a><span data-ttu-id="abcc0-992">文法模稜兩可</span><span class="sxs-lookup"><span data-stu-id="abcc0-992">Grammar ambiguities</span></span>

<span data-ttu-id="abcc0-993">針對生產*simple_name* ([簡單名稱](expressions.md#simple-names)) 和*member_access* ([成員存取](expressions.md#member-access)) 可以常理的語意模糊運算式的文法。</span><span class="sxs-lookup"><span data-stu-id="abcc0-993">The productions for *simple_name* ([Simple names](expressions.md#simple-names)) and *member_access* ([Member access](expressions.md#member-access)) can give rise to ambiguities in the grammar for expressions.</span></span> <span data-ttu-id="abcc0-994">例如，陳述式：</span><span class="sxs-lookup"><span data-stu-id="abcc0-994">For example, the statement:</span></span>
```
F(G<A,B>(7));
```
<span data-ttu-id="abcc0-995">無法解譯為呼叫`F`兩個引數時，`G < A`和`B > (7)`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-995">could be interpreted as a call to `F` with two arguments, `G < A` and `B > (7)`.</span></span> <span data-ttu-id="abcc0-996">或者，它可解譯為呼叫`F`一個引數，也就是泛型方法的呼叫 `G`具有兩個類型引數和一個一般引數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-996">Alternatively, it could be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span>

<span data-ttu-id="abcc0-997">如果語彙基元序列中可剖析 （內容） 作為*simple_name* ([簡單名稱](expressions.md#simple-names))， *member_access* ([成員存取](expressions.md#member-access))，或*pointer_member_access* ([指標成員存取](unsafe-code.md#pointer-member-access)) 以結束*type_argument_list* ([型別引數](types.md#type-arguments))，緊接在結尾的語彙基元`>`語彙基元會進行檢查。</span><span class="sxs-lookup"><span data-stu-id="abcc0-997">If a sequence of tokens can be parsed (in context) as a *simple_name* ([Simple names](expressions.md#simple-names)), *member_access* ([Member access](expressions.md#member-access)), or *pointer_member_access* ([Pointer member access](unsafe-code.md#pointer-member-access)) ending with a *type_argument_list* ([Type arguments](types.md#type-arguments)), the token immediately following the closing `>` token is examined.</span></span> <span data-ttu-id="abcc0-998">如果它是其中一個</span><span class="sxs-lookup"><span data-stu-id="abcc0-998">If it is one of</span></span>
```csharp
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```
<span data-ttu-id="abcc0-999">然後*type_argument_list*保留為一部分*simple_name*， *member_access*或*pointer_member_access*以及任何其他可能的剖析的語彙基元順序會被捨棄。</span><span class="sxs-lookup"><span data-stu-id="abcc0-999">then the *type_argument_list* is retained as part of the *simple_name*, *member_access* or *pointer_member_access* and any other possible parse of the sequence of tokens is discarded.</span></span> <span data-ttu-id="abcc0-1000">否則，請*type_argument_list*不是要納入*simple_name*， *member_access*或*pointer_member_access*，即使沒有任何其他可能的語彙基元序列的剖析。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1000">Otherwise, the *type_argument_list* is not considered to be part of the *simple_name*, *member_access* or *pointer_member_access*, even if there is no other possible parse of the sequence of tokens.</span></span> <span data-ttu-id="abcc0-1001">請注意，這些規則不會套用時剖析*type_argument_list*中*namespace_or_type_name* ([命名空間和型別名稱](basic-concepts.md#namespace-and-type-names))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1001">Note that these rules are not applied when parsing a *type_argument_list* in a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)).</span></span> <span data-ttu-id="abcc0-1002">陳述式</span><span class="sxs-lookup"><span data-stu-id="abcc0-1002">The statement</span></span>
```csharp
F(G<A,B>(7));
```
<span data-ttu-id="abcc0-1003">將根據這項規則，會解譯為呼叫`F`一個引數，也就是泛型方法的呼叫`G`具有兩個類型引數和一個一般引數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1003">will, according to this rule, be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span> <span data-ttu-id="abcc0-1004">陳述式</span><span class="sxs-lookup"><span data-stu-id="abcc0-1004">The statements</span></span>
```csharp
F(G < A, B > 7);
F(G < A, B >> 7);
```
<span data-ttu-id="abcc0-1005">將每個會解譯為呼叫`F`兩個引數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1005">will each be interpreted as a call to `F` with two arguments.</span></span> <span data-ttu-id="abcc0-1006">陳述式</span><span class="sxs-lookup"><span data-stu-id="abcc0-1006">The statement</span></span>
```csharp
x = F < A > +y;
```
<span data-ttu-id="abcc0-1007">將會視為已撰寫的陳述式必須解譯為小於運算子，大於運算子和一元加號運算子`x = (F < A) > (+y)`，而不是做為*simple_name*具有*type_argument_list*後面接著二元加法運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1007">will be interpreted as a less than operator, greater than operator, and unary plus operator, as if the statement had been written `x = (F < A) > (+y)`, instead of as a *simple_name* with a *type_argument_list* followed by a binary plus operator.</span></span> <span data-ttu-id="abcc0-1008">陳述式中</span><span class="sxs-lookup"><span data-stu-id="abcc0-1008">In the statement</span></span>
```csharp
x = y is C<T> + z;
```
<span data-ttu-id="abcc0-1009">語彙基元`C<T>`解譯為*namespace_or_type_name*具有*type_argument_list*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1009">the tokens `C<T>` are interpreted as a *namespace_or_type_name* with a *type_argument_list*.</span></span>

### <a name="invocation-expressions"></a><span data-ttu-id="abcc0-1010">叫用運算式</span><span class="sxs-lookup"><span data-stu-id="abcc0-1010">Invocation expressions</span></span>

<span data-ttu-id="abcc0-1011">*Invocation_expression*用來叫用方法。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1011">An *invocation_expression* is used to invoke a method.</span></span>

```antlr
invocation_expression
    : primary_expression '(' argument_list? ')'
    ;
```

<span data-ttu-id="abcc0-1012">*Invocation_expression*動態繫結 ([動態繫結](expressions.md#dynamic-binding)) 如果至少下列其中之一為：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1012">An *invocation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) if at least one of the following holds:</span></span>

* <span data-ttu-id="abcc0-1013">*Primary_expression*編譯時期類型`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1013">The *primary_expression* has compile-time type `dynamic`.</span></span>
* <span data-ttu-id="abcc0-1014">至少一個引數的選擇性*argument_list*已編譯時期型別`dynamic`並*primary_expression*沒有委派型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1014">At least one argument of the optional *argument_list* has compile-time type `dynamic` and the *primary_expression* does not have a delegate type.</span></span>

<span data-ttu-id="abcc0-1015">在此情況下，編譯器會將分類*invocation_expression*類型的值為`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1015">In this case the compiler classifies the *invocation_expression* as a value of type `dynamic`.</span></span> <span data-ttu-id="abcc0-1016">規則來判斷的意義*invocation_expression*接著會在執行階段，使用執行階段類型，而不編譯時期型別，這些的套用*primary_expression*和已在編譯時期型別引數`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1016">The rules below to determine the meaning of the *invocation_expression* are then applied at run-time, using the run-time type instead of the compile-time type of those of the *primary_expression* and arguments which have the compile-time type `dynamic`.</span></span> <span data-ttu-id="abcc0-1017">如果*primary_expression*沒有編譯時期型別`dynamic`，則方法叫用在中所述，就會進行有限的編譯時間檢查[編譯時期檢查動態的多載解析](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="abcc0-1017">If the *primary_expression* does not have compile-time type `dynamic`, then the method invocation undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="abcc0-1018">*Primary_expression*的*invocation_expression*必須是方法群組或值*delegate_type*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1018">The *primary_expression* of an *invocation_expression* must be a method group or a value of a *delegate_type*.</span></span> <span data-ttu-id="abcc0-1019">如果*primary_expression*是方法群組*invocation_expression*是方法的引動過程 ([方法引動過程](expressions.md#method-invocations))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1019">If the *primary_expression* is a method group, the *invocation_expression* is a method invocation ([Method invocations](expressions.md#method-invocations)).</span></span> <span data-ttu-id="abcc0-1020">如果*primary_expression*的值*delegate_type*，則*invocation_expression*是委派引動過程 ([委派引動過程](expressions.md#delegate-invocations)).</span><span class="sxs-lookup"><span data-stu-id="abcc0-1020">If the *primary_expression* is a value of a *delegate_type*, the *invocation_expression* is a delegate invocation ([Delegate invocations](expressions.md#delegate-invocations)).</span></span> <span data-ttu-id="abcc0-1021">如果*primary_expression*是方法群組和值都不*delegate_type*，則繫結階段會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1021">If the *primary_expression* is neither a method group nor a value of a *delegate_type*, a binding-time error occurs.</span></span>

<span data-ttu-id="abcc0-1022">選擇性*argument_list* ([引數清單](expressions.md#argument-lists)) 方法的參數提供值或變數參考。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1022">The optional *argument_list* ([Argument lists](expressions.md#argument-lists)) provides values or variable references for the parameters of the method.</span></span>

<span data-ttu-id="abcc0-1023">評估的結果*invocation_expression*分類如下：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1023">The result of evaluating an *invocation_expression* is classified as follows:</span></span>

*  <span data-ttu-id="abcc0-1024">如果*invocation_expression*叫用方法或委派，會傳回`void`，結果就是執行任何動作。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1024">If the *invocation_expression* invokes a method or delegate that returns `void`, the result is nothing.</span></span> <span data-ttu-id="abcc0-1025">運算式分類為不允許的內容中，只有*statement_expression* ([運算式陳述式](statements.md#expression-statements)) 或作為主體*lambda_expression*([匿名函式運算式](expressions.md#anonymous-function-expressions))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1025">An expression that is classified as nothing is permitted only in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)) or as the body of a *lambda_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)).</span></span> <span data-ttu-id="abcc0-1026">否則就會發生的繫結階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1026">Otherwise a binding-time error occurs.</span></span>
*  <span data-ttu-id="abcc0-1027">否則，結果就是方法或委派傳回類型的值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1027">Otherwise, the result is a value of the type returned by the method or delegate.</span></span>

#### <a name="method-invocations"></a><span data-ttu-id="abcc0-1028">方法引動過程</span><span class="sxs-lookup"><span data-stu-id="abcc0-1028">Method invocations</span></span>

<span data-ttu-id="abcc0-1029">方法引動過程中， *primary_expression*的*invocation_expression*必須是方法群組。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1029">For a method invocation, the *primary_expression* of the *invocation_expression* must be a method group.</span></span> <span data-ttu-id="abcc0-1030">叫用的一個方法或一組可供選擇要叫用的特定方法的多載方法，就會識別方法群組。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1030">The method group identifies the one method to invoke or the set of overloaded methods from which to choose a specific method to invoke.</span></span> <span data-ttu-id="abcc0-1031">在後者的情況下，判斷特定的方法來叫用為基礎的類型中的引數所提供的內容*argument_list*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1031">In the latter case, determination of the specific method to invoke is based on the context provided by the types of the arguments in the *argument_list*.</span></span>

<span data-ttu-id="abcc0-1032">繫結階段處理表單的方法引動過程`M(A)`，其中`M`是方法群組 (可能包括*type_argument_list*)，並`A`是選擇性*argument_清單*，包含下列步驟：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1032">The binding-time processing of a method invocation of the form `M(A)`, where `M` is a method group (possibly including a *type_argument_list*), and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="abcc0-1033">建構一組方法引動過程的候選項目方法。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1033">The set of candidate methods for the method invocation is constructed.</span></span> <span data-ttu-id="abcc0-1034">每個方法`F`方法群組相關聯`M`:</span><span class="sxs-lookup"><span data-stu-id="abcc0-1034">For each method `F` associated with the method group `M`:</span></span>
   *  <span data-ttu-id="abcc0-1035">如果`F`為非泛型`F`是候選時：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1035">If `F` is non-generic, `F` is a candidate when:</span></span>
      * <span data-ttu-id="abcc0-1036">`M` 沒有型別引數清單，以及</span><span class="sxs-lookup"><span data-stu-id="abcc0-1036">`M` has no type argument list, and</span></span>
      * <span data-ttu-id="abcc0-1037">`F` 相對於適用於`A`([適用的函式成員](expressions.md#applicable-function-member))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1037">`F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
   *  <span data-ttu-id="abcc0-1038">如果`F`是泛型並`M`沒有型別引數清單中，`F`是候選時：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1038">If `F` is generic and `M` has no type argument list, `F` is a candidate when:</span></span>
      * <span data-ttu-id="abcc0-1039">型別推斷 ([型別推斷](expressions.md#type-inference)) 成功，推斷的型別引數呼叫，清單和</span><span class="sxs-lookup"><span data-stu-id="abcc0-1039">Type inference ([Type inference](expressions.md#type-inference)) succeeds, inferring a list of type arguments for the call, and</span></span>
      * <span data-ttu-id="abcc0-1040">所有建構型別參數清單中的 F 一旦推斷的型別引數會取代對應的方法類型參數，滿足其條件約束 ([滿足條件約束](types.md#satisfying-constraints))，並的參數清單`F`相對於適用於`A`([適用的函式成員](expressions.md#applicable-function-member))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1040">Once the inferred type arguments are substituted for the corresponding method type parameters, all constructed types in the parameter list of F satisfy their constraints ([Satisfying constraints](types.md#satisfying-constraints)), and the parameter list of `F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
   *  <span data-ttu-id="abcc0-1041">如果`F`是泛型並`M`包含型別引數清單，`F`是候選時：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1041">If `F` is generic and `M` includes a type argument list, `F` is a candidate when:</span></span>
      * <span data-ttu-id="abcc0-1042">`F` 在類型引數清單中，提供了有相同數目的方法類型參數與</span><span class="sxs-lookup"><span data-stu-id="abcc0-1042">`F` has the same number of method type parameters as were supplied in the type argument list, and</span></span>
      * <span data-ttu-id="abcc0-1043">所有建構型別參數清單中的 F 型別引數會取代對應的方法類型參數，一旦滿足其條件約束 ([滿足條件約束](types.md#satisfying-constraints))，以及參數清單`F`相對於適用於`A`([適用的函式成員](expressions.md#applicable-function-member))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1043">Once the type arguments are substituted for the corresponding method type parameters, all constructed types in the parameter list of F satisfy their constraints ([Satisfying constraints](types.md#satisfying-constraints)), and the parameter list of `F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
*  <span data-ttu-id="abcc0-1044">一組候選項目方法會減少到只包含方法的最具衍生性的類型：每個方法`C.F`集中，其中`C`是在其中的型別方法`F`宣告，基底類型中宣告的所有方法`C`從集合中移除。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1044">The set of candidate methods is reduced to contain only methods from the most derived types: For each method `C.F` in the set, where `C` is the type in which the method `F` is declared, all methods declared in a base type of `C` are removed from the set.</span></span> <span data-ttu-id="abcc0-1045">此外，如果`C`而不是類別類型`object`，介面型別中宣告的所有方法會從集合中都移除。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1045">Furthermore, if `C` is a class type other than `object`, all methods declared in an interface type are removed from the set.</span></span> <span data-ttu-id="abcc0-1046">（這個第二個規則只能有影響方法群組時需要有效的基底類別以外的物件和設定的非空白有效的介面的型別參數的成員查閱的結果）。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1046">(This latter rule only has affect when the method group was the result of a member lookup on a type parameter having an effective base class other than object and a non-empty effective interface set.)</span></span>
*  <span data-ttu-id="abcc0-1047">如果候選項目方法的結果集是空的再進一步處理沿著下列步驟會放棄，並改為嘗試處理為擴充方法引動過程的引動過程 ([擴充方法引動過程](expressions.md#extension-method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="abcc0-1047">If the resulting set of candidate methods is empty, then further processing along the following steps are abandoned, and instead an attempt is made to process the invocation as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="abcc0-1048">如果失敗，則不適用的方法存在，並在繫結階段錯誤發生。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1048">If this fails, then no applicable methods exist, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="abcc0-1049">一組候選項目方法的最佳方法用的多載解析規則來識別[多載解析](expressions.md#overload-resolution)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1049">The best method of the set of candidate methods is identified using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="abcc0-1050">如果找不到單一的最佳方法，方法引動過程模稜兩可，並在繫結階段錯誤發生。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1050">If a single best method cannot be identified, the method invocation is ambiguous, and a binding-time error occurs.</span></span> <span data-ttu-id="abcc0-1051">執行多載解析時，泛型方法的參數後，會考慮替代的型別引數 （提供或推斷） 相對應的方法型別參數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1051">When performing overload resolution, the parameters of a generic method are considered after substituting the type arguments (supplied or inferred) for the corresponding method type parameters.</span></span>
*  <span data-ttu-id="abcc0-1052">最終驗證所選的最佳方法被執行：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1052">Final validation of the chosen best method is performed:</span></span>
   * <span data-ttu-id="abcc0-1053">方法會進行驗證的方法群組內容中：最好的方法是靜態方法，如果方法群組必須起因*simple_name*或是*member_access*透過型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1053">The method is validated in the context of the method group: If the best method is a static method, the method group must have resulted from a *simple_name* or a *member_access* through a type.</span></span> <span data-ttu-id="abcc0-1054">最好的方法是執行個體方法，如果方法群組必須起因*simple_name*，則*member_access*透過變數或值，或*base_access*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1054">If the best method is an instance method, the method group must have resulted from a *simple_name*, a *member_access* through a variable or value, or a *base_access*.</span></span> <span data-ttu-id="abcc0-1055">如果這些需求都為 true，則繫結階段會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1055">If neither of these requirements is true, a binding-time error occurs.</span></span>
   * <span data-ttu-id="abcc0-1056">最好的方法是泛型方法，如果型別引數 （提供或推斷） 會針對條件約束檢查 ([滿足條件約束](types.md#satisfying-constraints)) 宣告之泛型方法。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1056">If the best method is a generic method, the type arguments (supplied or inferred) are checked against the constraints ([Satisfying constraints](types.md#satisfying-constraints)) declared on the generic method.</span></span> <span data-ttu-id="abcc0-1057">如果任何類型引數未滿足型別參數上的對應條件約束，繫結階段會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1057">If any type argument does not satisfy the corresponding constraint(s) on the type parameter, a binding-time error occurs.</span></span>

<span data-ttu-id="abcc0-1058">當方法有選取，而且繫結時間經過上述步驟時，根據函式成員引動過程中所述的規則處理實際的執行階段引動過程[編譯時期檢查動態的多載解析](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="abcc0-1058">Once a method has been selected and validated at binding-time by the above steps, the actual run-time invocation is processed according to the rules of function member invocation described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="abcc0-1059">上面所述的解析規則的直覺式的效果如下所示：若要尋找特定的方法叫用的方法引動過程，開頭的方法引動過程所表示的類型，並繼續繼承鏈結，直到找到至少一個適用、 存取、 非覆寫方法宣告。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1059">The intuitive effect of the resolution rules described above is as follows: To locate the particular method invoked by a method invocation, start with the type indicated by the method invocation and proceed up the inheritance chain until at least one applicable, accessible, non-override method declaration is found.</span></span> <span data-ttu-id="abcc0-1060">然後執行型別推斷和多載解析將適用、 存取、 非覆寫的方法，該類型中宣告的集合，並叫用選取的方法。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1060">Then perform type inference and overload resolution on the set of applicable, accessible, non-override methods declared in that type and invoke the method thus selected.</span></span> <span data-ttu-id="abcc0-1061">如果找不到任何方法，請嘗試改為處理為擴充方法引動過程的引動過程。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1061">If no method was found, try instead to process the invocation as an extension method invocation.</span></span>

#### <a name="extension-method-invocations"></a><span data-ttu-id="abcc0-1062">擴充方法引動過程</span><span class="sxs-lookup"><span data-stu-id="abcc0-1062">Extension method invocations</span></span>

<span data-ttu-id="abcc0-1063">方法引動過程中 ([經過 boxing 處理的執行個體上的引動過程](expressions.md#invocations-on-boxed-instances)) 的其中一種格式</span><span class="sxs-lookup"><span data-stu-id="abcc0-1063">In a method invocation ([Invocations on boxed instances](expressions.md#invocations-on-boxed-instances)) of one of the forms</span></span>
```csharp
expr . identifier ( )

expr . identifier ( args )

expr . identifier < typeargs > ( )

expr . identifier < typeargs > ( args )
```
<span data-ttu-id="abcc0-1064">如果叫用的正常處理發現沒有適用的方法，會嘗試處理為擴充方法引動過程的建構。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1064">if the normal processing of the invocation finds no applicable methods, an attempt is made to process the construct as an extension method invocation.</span></span> <span data-ttu-id="abcc0-1065">如果*expr*或任何*args*編譯時期類型`dynamic`，將不會套用的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1065">If *expr* or any of the *args* has compile-time type `dynamic`, extension methods will not apply.</span></span>

<span data-ttu-id="abcc0-1066">本節目標是要找出最*type_name* `C`，如此一來，可以進行對應的靜態方法引動過程：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1066">The objective is to find the best *type_name* `C`, so that the corresponding static method invocation can take place:</span></span>
```csharp
C . identifier ( expr )

C . identifier ( expr , args )

C . identifier < typeargs > ( expr )

C . identifier < typeargs > ( expr , args )
```

<span data-ttu-id="abcc0-1067">擴充方法`Ci.Mj`已***合格***如果：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1067">An extension method `Ci.Mj` is ***eligible*** if:</span></span>

*  <span data-ttu-id="abcc0-1068">`Ci` 非泛型、 非巢狀類別</span><span class="sxs-lookup"><span data-stu-id="abcc0-1068">`Ci` is a non-generic, non-nested class</span></span>
*  <span data-ttu-id="abcc0-1069">名稱`Mj`是*識別碼*</span><span class="sxs-lookup"><span data-stu-id="abcc0-1069">The name of `Mj` is *identifier*</span></span>
*  <span data-ttu-id="abcc0-1070">`Mj` 是可存取且適用於如上所示，套用至引數作為靜態方法</span><span class="sxs-lookup"><span data-stu-id="abcc0-1070">`Mj` is accessible and applicable when applied to the arguments as a static method as shown above</span></span>
*  <span data-ttu-id="abcc0-1071">隱含的身分識別、 參考或 boxing 轉換存在*expr*的第一個參數的型別`Mj`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1071">An implicit identity, reference or boxing conversion exists from *expr* to the type of the first parameter of `Mj`.</span></span>

<span data-ttu-id="abcc0-1072">搜尋`C`程序如下：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1072">The search for `C` proceeds as follows:</span></span>

*  <span data-ttu-id="abcc0-1073">開頭為最接近封入命名空間宣告，每個封入的命名空間宣告，再繼續，並以包含編譯單位中，作為結束連續嘗試尋找一組候選的擴充方法：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1073">Starting with the closest enclosing namespace declaration, continuing with each enclosing namespace declaration, and ending with the containing compilation unit, successive attempts are made to find a candidate set of extension methods:</span></span>
   * <span data-ttu-id="abcc0-1074">如果指定的命名空間或編譯單位就會直接包含非泛型型別宣告`Ci`符合資格的擴充方法與`Mj`，則這些擴充方法的集合是候選集合。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1074">If the given namespace or compilation unit directly contains non-generic type declarations `Ci` with eligible extension methods `Mj`, then the set of those extension methods is the candidate set.</span></span>
   * <span data-ttu-id="abcc0-1075">如果類型`Ci`匯入*using_static_declarations*並直接匯入的命名空間中宣告*using_namespace_directive*指定命名空間或編譯單位中直接的 s包含符合資格的擴充方法`Mj`，則這些擴充方法的集合是候選集合。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1075">If types `Ci` imported by *using_static_declarations* and directly declared in namespaces imported by *using_namespace_directive*s in the given namespace or compilation unit directly contain eligible extension methods `Mj`, then the set of those extension methods is the candidate set.</span></span>
*  <span data-ttu-id="abcc0-1076">如果找到任何封入的命名空間宣告或編譯單位中沒有候選項目集，則會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1076">If no candidate set is found in any enclosing namespace declaration or compilation unit, a compile-time error occurs.</span></span>
*  <span data-ttu-id="abcc0-1077">多載解析候選版本中所述，設定的套用，否則為 ([多載解析](expressions.md#overload-resolution))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1077">Otherwise, overload resolution is applied to the candidate set as described in ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="abcc0-1078">如果找不到任何單一的最佳方法，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1078">If no single best method is found, a compile-time error occurs.</span></span>
*  <span data-ttu-id="abcc0-1079">`C` 是在其中的最佳方法宣告為擴充方法的類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1079">`C` is the type within which the best method is declared as an extension method.</span></span>

<span data-ttu-id="abcc0-1080">使用`C`做為目標，方法呼叫接著會處理當做靜態方法的引動過程 ([編譯時期檢查動態的多載解析](expressions.md#compile-time-checking-of-dynamic-overload-resolution))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1080">Using `C` as a target, the method call is then processed as a static method invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span>

<span data-ttu-id="abcc0-1081">上述規則表示，執行個體方法優先於延伸方法、，內部命名空間宣告所提供的延伸模組方法優先於外部命名空間宣告和該延伸模組中可用的擴充方法直接在命名空間中宣告方法的優先順序高於匯入到該相同的命名空間中使用的擴充方法的命名空間指示詞。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1081">The preceding rules mean that instance methods take precedence over extension methods, that extension methods available in inner namespace declarations take precedence over extension methods available in outer namespace declarations, and that extension methods declared directly in a namespace take precedence over extension methods imported into that same namespace with a using namespace directive.</span></span> <span data-ttu-id="abcc0-1082">例如：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1082">For example:</span></span>
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

<span data-ttu-id="abcc0-1083">在範例中，`B`的方法都優先於第一個擴充方法，和`C`的方法的優先順序高於這兩個擴充方法。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1083">In the example, `B`'s method takes precedence over the first extension method, and `C`'s method takes precedence over both extension methods.</span></span>

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

<span data-ttu-id="abcc0-1084">此範例的輸出為：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1084">The output of this example is:</span></span>
```
E.F(1)
D.G(2)
C.H(3)
```
<span data-ttu-id="abcc0-1085">`D.G` 優先順序高於`C.G`，並`E.F`優先於兩者`D.F`和`C.F`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1085">`D.G` takes precedence over `C.G`, and `E.F` takes precedence over both `D.F` and `C.F`.</span></span>

#### <a name="delegate-invocations"></a><span data-ttu-id="abcc0-1086">委派引動過程</span><span class="sxs-lookup"><span data-stu-id="abcc0-1086">Delegate invocations</span></span>

<span data-ttu-id="abcc0-1087">委派引動過程中， *primary_expression*的*invocation_expression*的值必須*delegate_type*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1087">For a delegate invocation, the *primary_expression* of the *invocation_expression* must be a value of a *delegate_type*.</span></span> <span data-ttu-id="abcc0-1088">此外，考慮*delegate_type*是相同的參數清單的函式成員*delegate_type*，則*delegate_type*必須適用於 （[適用的函式成員](expressions.md#applicable-function-member)) 相對於*argument_list*的*invocation_expression*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1088">Furthermore, considering the *delegate_type* to be a function member with the same parameter list as the *delegate_type*, the *delegate_type* must be applicable ([Applicable function member](expressions.md#applicable-function-member)) with respect to the *argument_list* of the *invocation_expression*.</span></span>

<span data-ttu-id="abcc0-1089">執行階段處理表單的委派引動過程`D(A)`，其中`D`是*primary_expression*的*delegate_type*並`A`是選擇性的*argument_list*，包含下列步驟：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1089">The run-time processing of a delegate invocation of the form `D(A)`, where `D` is a *primary_expression* of a *delegate_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="abcc0-1090">`D` 會評估。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1090">`D` is evaluated.</span></span> <span data-ttu-id="abcc0-1091">如果此評估會產生例外狀況，則會不執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1091">If this evaluation causes an exception, no further steps are executed.</span></span>
*  <span data-ttu-id="abcc0-1092">值`D`檢查為有效。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1092">The value of `D` is checked to be valid.</span></span> <span data-ttu-id="abcc0-1093">如果值`D`已`null`、`System.NullReferenceException`就會擲回，而且會執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1093">If the value of `D` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="abcc0-1094">否則，`D`委派執行個體的參考。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1094">Otherwise, `D` is a reference to a delegate instance.</span></span> <span data-ttu-id="abcc0-1095">函式成員叫用 ([編譯時期檢查動態的多載解析](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) 會在每個委派引動過程清單中的可呼叫實體上執行。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1095">Function member invocations ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) are performed on each of the callable entities in the invocation list of the delegate.</span></span> <span data-ttu-id="abcc0-1096">對於可呼叫的實體，其中包含執行個體和執行個體方法，叫用執行個體是可呼叫的實體中所包含的執行個體。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1096">For callable entities consisting of an instance and instance method, the instance for the invocation is the instance contained in the callable entity.</span></span>

### <a name="element-access"></a><span data-ttu-id="abcc0-1097">項目存取</span><span class="sxs-lookup"><span data-stu-id="abcc0-1097">Element access</span></span>

<span data-ttu-id="abcc0-1098">*Element_access*組成*primary_no_array_creation_expression*，後面接著"`[`"語彙基元，後面接著*argument_list*，後面接著"`]`"token。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1098">An *element_access* consists of a *primary_no_array_creation_expression*, followed by a "`[`" token, followed by an *argument_list*, followed by a "`]`" token.</span></span> <span data-ttu-id="abcc0-1099">*Argument_list*包含一個或多個*引數*s，並以逗號分隔。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1099">The *argument_list* consists of one or more *argument*s, separated by commas.</span></span>

```antlr
element_access
    : primary_no_array_creation_expression '[' expression_list ']'
    ;
```

<span data-ttu-id="abcc0-1100">*Argument_list*的*element_access*不允許包含`ref`或`out`引數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1100">The *argument_list* of an *element_access* is not allowed to contain `ref` or `out` arguments.</span></span>

<span data-ttu-id="abcc0-1101">*Element_access*動態繫結 ([動態繫結](expressions.md#dynamic-binding)) 如果至少下列其中之一為：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1101">An *element_access* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) if at least one of the following holds:</span></span>

* <span data-ttu-id="abcc0-1102">*Primary_no_array_creation_expression*編譯時期類型`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1102">The *primary_no_array_creation_expression* has compile-time type `dynamic`.</span></span>
* <span data-ttu-id="abcc0-1103">至少一個運算式*argument_list*已編譯時期型別`dynamic`並*primary_no_array_creation_expression*沒有陣列型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1103">At least one expression of the *argument_list* has compile-time type `dynamic` and the *primary_no_array_creation_expression* does not have an array type.</span></span>

<span data-ttu-id="abcc0-1104">在此情況下，編譯器會將分類*element_access*類型的值為`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1104">In this case the compiler classifies the *element_access* as a value of type `dynamic`.</span></span> <span data-ttu-id="abcc0-1105">規則來判斷的意義*element_access*接著會在執行階段，使用執行階段類型，而不編譯時期型別，這些的套用*primary_no_array_creation_expression*並*argument_list*運算式有在編譯時期型別`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1105">The rules below to determine the meaning of the *element_access* are then applied at run-time, using the run-time type instead of the compile-time type of those of the *primary_no_array_creation_expression* and *argument_list* expressions which have the compile-time type `dynamic`.</span></span> <span data-ttu-id="abcc0-1106">如果*primary_no_array_creation_expression*沒有編譯時期型別`dynamic`，然後項目存取在中所述，就會進行有限的編譯時間檢查[編譯時期檢查動態多載解析](expressions.md#compile-time-checking-of-dynamic-overload-resolution)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1106">If the *primary_no_array_creation_expression* does not have compile-time type `dynamic`, then the element access undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="abcc0-1107">如果*primary_no_array_creation_expression*的*element_access*的值*array_type*，則*element_access*是陣列存取 ([陣列存取](expressions.md#array-access))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1107">If the *primary_no_array_creation_expression* of an *element_access* is a value of an *array_type*, the *element_access* is an array access ([Array access](expressions.md#array-access)).</span></span> <span data-ttu-id="abcc0-1108">否則，請*primary_no_array_creation_expression*必須是變數或值的類別、 結構或介面型別具有一或多個索引子的成員，在此情況下*element_access*是索引子存取 ([索引子存取](expressions.md#indexer-access))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1108">Otherwise, the *primary_no_array_creation_expression* must be a variable or value of a class, struct, or interface type that has one or more indexer members, in which case the *element_access* is an indexer access ([Indexer access](expressions.md#indexer-access)).</span></span>

#### <a name="array-access"></a><span data-ttu-id="abcc0-1109">陣列存取</span><span class="sxs-lookup"><span data-stu-id="abcc0-1109">Array access</span></span>

<span data-ttu-id="abcc0-1110">存取陣列*primary_no_array_creation_expression*的*element_access*的值必須*array_type*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1110">For an array access, the *primary_no_array_creation_expression* of the *element_access* must be a value of an *array_type*.</span></span> <span data-ttu-id="abcc0-1111">此外， *argument_list*陣列的存取不允許包含具名引數。中的運算式數目*argument_list*必須是相同的陣序規範*array_type*，和每個運算式必須是型別`int`， `uint`， `long`， `ulong`，或必須是隱含地轉換成一或多個這些型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1111">Furthermore, the *argument_list* of an array access is not allowed to contain named arguments.The number of expressions in the *argument_list* must be the same as the rank of the *array_type*, and each expression must be of type `int`, `uint`, `long`, `ulong`, or must be implicitly convertible to one or more of these types.</span></span>

<span data-ttu-id="abcc0-1112">評估陣列存取的結果是陣列，也就是陣列項目中之運算式的值所選取的項目類型的變數*argument_list*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1112">The result of evaluating an array access is a variable of the element type of the array, namely the array element selected by the value(s) of the expression(s) in the *argument_list*.</span></span>

<span data-ttu-id="abcc0-1113">陣列存取表單的執行階段處理`P[A]`，其中`P`是*primary_no_array_creation_expression*的*array_type*並`A`是*argument_list*，包含下列步驟：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1113">The run-time processing of an array access of the form `P[A]`, where `P` is a *primary_no_array_creation_expression* of an *array_type* and `A` is an *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="abcc0-1114">`P` 會評估。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1114">`P` is evaluated.</span></span> <span data-ttu-id="abcc0-1115">如果此評估會產生例外狀況，則會不執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1115">If this evaluation causes an exception, no further steps are executed.</span></span>
*  <span data-ttu-id="abcc0-1116">索引運算式*argument_list*的評估順序，從左到右。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1116">The index expressions of the *argument_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="abcc0-1117">遵循每個索引運算式時，隱含轉換的評估結果 ([隱含轉換](conversions.md#implicit-conversions)) 執行下列類型的其中一個： `int`， `uint`， `long`， `ulong`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1117">Following evaluation of each index expression, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to one of the following types is performed: `int`, `uint`, `long`, `ulong`.</span></span> <span data-ttu-id="abcc0-1118">第一個類型的隱含轉換存在，這份清單中選擇。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1118">The first type in this list for which an implicit conversion exists is chosen.</span></span> <span data-ttu-id="abcc0-1119">比方說，索引運算式屬於類型`short`然後隱含轉換成`int`會執行，因為隱含轉換，從`short`來`int`進出`short`至`long`可能會有。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1119">For instance, if the index expression is of type `short` then an implicit conversion to `int` is performed, since implicit conversions from `short` to `int` and from `short` to `long` are possible.</span></span> <span data-ttu-id="abcc0-1120">如果評估的索引運算式或後續的隱含轉換導致例外狀況，然後沒有進一步的索引運算式會評估，並不再執行的步驟。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1120">If evaluation of an index expression or the subsequent implicit conversion causes an exception, then no further index expressions are evaluated and no further steps are executed.</span></span>
*  <span data-ttu-id="abcc0-1121">值`P`檢查為有效。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1121">The value of `P` is checked to be valid.</span></span> <span data-ttu-id="abcc0-1122">如果值`P`已`null`、`System.NullReferenceException`就會擲回，而且會執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1122">If the value of `P` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="abcc0-1123">在每個運算式的值*argument_list*會針對每個維度的陣列執行個體所參考的實際界限檢查`P`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1123">The value of each expression in the *argument_list* is checked against the actual bounds of each dimension of the array instance referenced by `P`.</span></span> <span data-ttu-id="abcc0-1124">如果一或多個值超出範圍，`System.IndexOutOfRangeException`就會擲回，而且會執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1124">If one or more values are out of range, a `System.IndexOutOfRangeException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="abcc0-1125">計算指定之索引運算式的陣列元素的位置，及此位置會變成陣列存取的結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1125">The location of the array element given by the index expression(s) is computed, and this location becomes the result of the array access.</span></span>

#### <a name="indexer-access"></a><span data-ttu-id="abcc0-1126">索引子存取</span><span class="sxs-lookup"><span data-stu-id="abcc0-1126">Indexer access</span></span>

<span data-ttu-id="abcc0-1127">索引子存取，如*primary_no_array_creation_expression*的*element_access*必須是變數或值的類別、 結構或介面型別，而且此型別必須實作一或多個適用於與索引子*argument_list*的*element_access*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1127">For an indexer access, the *primary_no_array_creation_expression* of the *element_access* must be a variable or value of a class, struct, or interface type, and this type must implement one or more indexers that are applicable with respect to the *argument_list* of the *element_access*.</span></span>

<span data-ttu-id="abcc0-1128">索引子存取表單的繫結時間處理`P[A]`，其中`P`是*primary_no_array_creation_expression*的類別、 結構或介面型別`T`，和`A`是*argument_list*，包含下列步驟：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1128">The binding-time processing of an indexer access of the form `P[A]`, where `P` is a *primary_no_array_creation_expression* of a class, struct, or interface type `T`, and `A` is an *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="abcc0-1129">所提供的索引子組`T`建構。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1129">The set of indexers provided by `T` is constructed.</span></span> <span data-ttu-id="abcc0-1130">集合，包含的所有宣告索引子`T`或基底型別`T`不是`override`宣告和可存取在目前內容中 ([成員存取](basic-concepts.md#member-access))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1130">The set consists of all indexers declared in `T` or a base type of `T` that are not `override` declarations and are accessible in the current context ([Member access](basic-concepts.md#member-access)).</span></span>
*  <span data-ttu-id="abcc0-1131">集合會縮減為那些索引子可適用且不是隱藏的其他索引子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1131">The set is reduced to those indexers that are applicable and not hidden by other indexers.</span></span> <span data-ttu-id="abcc0-1132">下列規則會套用至每個索引子`S.I`集中，其中`S`是要在其中類型索引子`I`宣告：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1132">The following rules are applied to each indexer `S.I` in the set, where `S` is the type in which the indexer `I` is declared:</span></span>
   * <span data-ttu-id="abcc0-1133">如果`I`不適用相對於`A`([適用的函式成員](expressions.md#applicable-function-member))，然後`I`從集合中移除。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1133">If `I` is not applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)), then `I` is removed from the set.</span></span>
   * <span data-ttu-id="abcc0-1134">如果`I`相對於適用於`A`([適用的函式成員](expressions.md#applicable-function-member))，然後將所有索引子宣告基底類型中`S`從集合中移除。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1134">If `I` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)), then all indexers declared in a base type of `S` are removed from the set.</span></span>
   * <span data-ttu-id="abcc0-1135">如果`I`相對於適用於`A`([適用的函式成員](expressions.md#applicable-function-member)) 和`S`而不是類別類型`object`，介面中宣告的所有索引子會從集合中移除。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1135">If `I` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)) and `S` is a class type other than `object`, all indexers declared in an interface are removed from the set.</span></span>
*  <span data-ttu-id="abcc0-1136">如果候選索引子的結果集是空的則不適用的索引子存在，並繫結階段發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1136">If the resulting set of candidate indexers is empty, then no applicable indexers exist, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="abcc0-1137">候選索引子一組最佳的索引子用的多載解析規則來識別[多載解析](expressions.md#overload-resolution)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1137">The best indexer of the set of candidate indexers is identified using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="abcc0-1138">如果找不到單一最佳的索引子，索引子存取模稜兩可，並在繫結階段錯誤發生。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1138">If a single best indexer cannot be identified, the indexer access is ambiguous, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="abcc0-1139">索引運算式*argument_list*的評估順序，從左到右。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1139">The index expressions of the *argument_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="abcc0-1140">處理索引子存取的結果是歸類為索引子存取的運算式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1140">The result of processing the indexer access is an expression classified as an indexer access.</span></span> <span data-ttu-id="abcc0-1141">索引子存取運算式參考的索引子，在上述步驟中決定，而且沒有相關聯的執行個體的運算式`P`和相關聯的引數清單的`A`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1141">The indexer access expression references the indexer determined in the step above, and has an associated instance expression of `P` and an associated argument list of `A`.</span></span>

<span data-ttu-id="abcc0-1142">根據在其中使用的內容，索引子存取造成的其中一個引動過程*get 存取子*或*set 存取子*的索引子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1142">Depending on the context in which it is used, an indexer access causes invocation of either the *get accessor* or the *set accessor* of the indexer.</span></span> <span data-ttu-id="abcc0-1143">索引子存取做為目標的指派，如果*set 存取子*會叫用來指派新值 ([簡單指派](expressions.md#simple-assignment))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1143">If the indexer access is the target of an assignment, the *set accessor* is invoked to assign a new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="abcc0-1144">在其他情況下， *get 存取子*會叫用來取得目前的值 ([運算式的值](expressions.md#values-of-expressions))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1144">In all other cases, the *get accessor* is invoked to obtain the current value ([Values of expressions](expressions.md#values-of-expressions)).</span></span>

### <a name="this-access"></a><span data-ttu-id="abcc0-1145">此存取權</span><span class="sxs-lookup"><span data-stu-id="abcc0-1145">This access</span></span>

<span data-ttu-id="abcc0-1146">A *this_access*包含保留字`this`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1146">A *this_access* consists of the reserved word `this`.</span></span>

```antlr
this_access
    : 'this'
    ;
```

<span data-ttu-id="abcc0-1147">A *this_access*只有在允許*區塊*的執行個體建構函式、 執行個體方法，或執行個體存取子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1147">A *this_access* is permitted only in the *block* of an instance constructor, an instance method, or an instance accessor.</span></span> <span data-ttu-id="abcc0-1148">它具有下列意義的其中一個：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1148">It has one of the following meanings:</span></span>

*  <span data-ttu-id="abcc0-1149">當`this`用於*primary_expression*內類別的執行個體建構函式，它會分類為值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1149">When `this` is used in a *primary_expression* within an instance constructor of a class, it is classified as a value.</span></span> <span data-ttu-id="abcc0-1150">值的類型是此執行個體類型 ([的執行個體類型](classes.md#the-instance-type)) 的類別，在其中使用方式，就會發生，而值是所建構物件的參考。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1150">The type of the value is the instance type ([The instance type](classes.md#the-instance-type)) of the class within which the usage occurs, and the value is a reference to the object being constructed.</span></span>
*  <span data-ttu-id="abcc0-1151">當`this`用於*primary_expression*內的執行個體方法或類別的執行個體存取子，它會分類為值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1151">When `this` is used in a *primary_expression* within an instance method or instance accessor of a class, it is classified as a value.</span></span> <span data-ttu-id="abcc0-1152">值的類型是此執行個體類型 ([的執行個體類型](classes.md#the-instance-type)) 的類別內的使用方式，就會發生，且值為已叫用方法或存取子時物件的參考。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1152">The type of the value is the instance type ([The instance type](classes.md#the-instance-type)) of the class within which the usage occurs, and the value is a reference to the object for which the method or accessor was invoked.</span></span>
*  <span data-ttu-id="abcc0-1153">當`this`用於*primary_expression*結構的執行個體建構函式，它會被歸類為變數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1153">When `this` is used in a *primary_expression* within an instance constructor of a struct, it is classified as a variable.</span></span> <span data-ttu-id="abcc0-1154">變數的類型是此執行個體類型 ([的執行個體類型](classes.md#the-instance-type)) 在其中使用方式，就會發生，且該變數代表所建構之結構的結構。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1154">The type of the variable is the instance type ([The instance type](classes.md#the-instance-type)) of the struct within which the usage occurs, and the variable represents the struct being constructed.</span></span> <span data-ttu-id="abcc0-1155">`this`結構的執行個體建構函式的變數的行為完全相同`out`結構型別的參數 — 特別是，這表示，必須在執行個體的每個執行路徑中明確指派變數建構函式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1155">The `this` variable of an instance constructor of a struct behaves exactly the same as an `out` parameter of the struct type—in particular, this means that the variable must be definitely assigned in every execution path of the instance constructor.</span></span>
*  <span data-ttu-id="abcc0-1156">當`this`用於*primary_expression*內的執行個體方法或結構的執行個體存取子，它會被歸類為變數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1156">When `this` is used in a *primary_expression* within an instance method or instance accessor of a struct, it is classified as a variable.</span></span> <span data-ttu-id="abcc0-1157">變數的類型是此執行個體類型 ([的執行個體類型](classes.md#the-instance-type)) 的使用狀況中的結構。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1157">The type of the variable is the instance type ([The instance type](classes.md#the-instance-type)) of the struct within which the usage occurs.</span></span>
   * <span data-ttu-id="abcc0-1158">如果方法或存取子不是迭代器 ([迭代器](classes.md#iterators))，則`this`變數代表的方法或存取子為叫用與行為完全相同的結構`ref`結構類型的參數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1158">If the method or accessor is not an iterator ([Iterators](classes.md#iterators)), the `this` variable represents the struct for which the method or accessor was invoked, and behaves exactly the same as a `ref` parameter of the struct type.</span></span>
   * <span data-ttu-id="abcc0-1159">如果方法或存取子是迭代器，`this`變數代表的結構，並以其方法或存取子為叫用的行為與實值參數的結構類型完全相同複本。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1159">If the method or accessor is an iterator, the `this` variable represents a copy of the struct for which the method or accessor was invoked, and behaves exactly the same as a value parameter of the struct type.</span></span>

<span data-ttu-id="abcc0-1160">利用`this`中*primary_expression*上面所列以外的內容中是編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1160">Use of `this` in a *primary_expression* in a context other than the ones listed above is a compile-time error.</span></span> <span data-ttu-id="abcc0-1161">特別是，不可以參考`this`中的靜態方法，靜態屬性存取子，或在*variable_initializer*欄位宣告。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1161">In particular, it is not possible to refer to `this` in a static method, a static property accessor, or in a *variable_initializer* of a field declaration.</span></span>

### <a name="base-access"></a><span data-ttu-id="abcc0-1162">基底存取</span><span class="sxs-lookup"><span data-stu-id="abcc0-1162">Base access</span></span>

<span data-ttu-id="abcc0-1163">A *base_access*包含保留字`base`後面 」`.`"語彙基元和的識別碼或*argument_list*以方括弧括住：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1163">A *base_access* consists of the reserved word `base` followed by either a "`.`" token and an identifier or an *argument_list* enclosed in square brackets:</span></span>

```antlr
base_access
    : 'base' '.' identifier
    | 'base' '[' expression_list ']'
    ;
```

<span data-ttu-id="abcc0-1164">A *base_access*用來存取目前的類別或結構中同樣的具名成員隱藏基底類別成員。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1164">A *base_access* is used to access base class members that are hidden by similarly named members in the current class or struct.</span></span> <span data-ttu-id="abcc0-1165">A *base_access*只有在允許*區塊*的執行個體建構函式、 執行個體方法，或執行個體存取子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1165">A *base_access* is permitted only in the *block* of an instance constructor, an instance method, or an instance accessor.</span></span> <span data-ttu-id="abcc0-1166">當`base.I`類別或結構中，就會發生`I`必須代表該類別或結構的基底類別的成員。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1166">When `base.I` occurs in a class or struct, `I` must denote a member of the base class of that class or struct.</span></span> <span data-ttu-id="abcc0-1167">同樣地，當`base[E]`，就會發生在類別中，適當的索引子必須存在於基底類別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1167">Likewise, when `base[E]` occurs in a class, an applicable indexer must exist in the base class.</span></span>

<span data-ttu-id="abcc0-1168">在繫結階段*base_access*形式的運算式`base.I`並`base[E]`會進行評估，完全如同它們以寫入`((B)this).I`和`((B)this)[E]`，其中`B`之類別的基底類別或在其中建構，就會發生的結構。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1168">At binding-time, *base_access* expressions of the form `base.I` and `base[E]` are evaluated exactly as if they were written `((B)this).I` and `((B)this)[E]`, where `B` is the base class of the class or struct in which the construct occurs.</span></span> <span data-ttu-id="abcc0-1169">因此，`base.I`並`base[E]`對應至`this.I`並`this[E]`，除了`this`視為基底類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1169">Thus, `base.I` and `base[E]` correspond to `this.I` and `this[E]`, except `this` is viewed as an instance of the base class.</span></span>

<span data-ttu-id="abcc0-1170">當*base_access*參考其中判斷函式成員，才能在執行階段叫用的虛擬函式成員 （方法、 屬性或索引子），([編譯時期檢查動態的多載解析](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) 已變更。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1170">When a *base_access* references a virtual function member (a method, property, or indexer), the determination of which function member to invoke at run-time ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is changed.</span></span> <span data-ttu-id="abcc0-1171">叫用的函式成員由尋找最具衍生性的實作 ([虛擬方法](classes.md#virtual-methods)) 函式成員相對於`B`(而不是相關的執行階段類型`this`，做為一般非基底存取）。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1171">The function member that is invoked is determined by finding the most derived implementation ([Virtual methods](classes.md#virtual-methods)) of the function member with respect to `B` (instead of with respect to the run-time type of `this`, as would be usual in a non-base access).</span></span> <span data-ttu-id="abcc0-1172">因此，在`override`的`virtual`函式成員*base_access*可以用來叫用函式成員的繼承的實作。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1172">Thus, within an `override` of a `virtual` function member, a *base_access* can be used to invoke the inherited implementation of the function member.</span></span> <span data-ttu-id="abcc0-1173">如果所參考的函式成員*base_access*是抽象的則繫結階段會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1173">If the function member referenced by a *base_access* is abstract, a binding-time error occurs.</span></span>

### <a name="postfix-increment-and-decrement-operators"></a><span data-ttu-id="abcc0-1174">後置遞增和遞減運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-1174">Postfix increment and decrement operators</span></span>

```antlr
post_increment_expression
    : primary_expression '++'
    ;

post_decrement_expression
    : primary_expression '--'
    ;
```

<span data-ttu-id="abcc0-1175">運算元的後置遞增或遞減作業必須是運算式分類為變數、 屬性存取或索引子存取。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1175">The operand of a postfix increment or decrement operation must be an expression classified as a variable, a property access, or an indexer access.</span></span> <span data-ttu-id="abcc0-1176">作業的結果是相同的型別運算元的值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1176">The result of the operation is a value of the same type as the operand.</span></span>

<span data-ttu-id="abcc0-1177">如果*primary_expression*已在編譯時期型別`dynamic`然後運算子動態繫結 ([動態繫結](expressions.md#dynamic-binding))，則*post_increment_expression*或是*post_decrement_expression*已在編譯時期型別`dynamic`，並在執行階段使用的執行階段型別套用下列規則*primary_expression*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1177">If the *primary_expression* has the compile-time type `dynamic` then the operator is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), the *post_increment_expression* or *post_decrement_expression* has the compile-time type `dynamic` and the following rules are applied at run-time using the run-time type of the *primary_expression*.</span></span>

<span data-ttu-id="abcc0-1178">如果後置的運算元遞增或遞減運算是屬性或索引子存取、 屬性或索引子必須兩者`get`和`set`存取子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1178">If the operand of a postfix increment or decrement operation is a property or indexer access, the property or indexer must have both a `get` and a `set` accessor.</span></span> <span data-ttu-id="abcc0-1179">如果這不是這樣，繫結階段會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1179">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="abcc0-1180">一元運算子多載解析 ([一元運算子多載解析](expressions.md#unary-operator-overload-resolution)) 套用至選取的特定運算子實作。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1180">Unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="abcc0-1181">預先定義的`++`並`--`運算子有下列類型： `sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char``float`， `double`， `decimal`，以及任何列舉類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1181">Predefined `++` and `--` operators exist for the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, and any enum type.</span></span> <span data-ttu-id="abcc0-1182">預先定義`++`運算子會傳回運算元，而預先定義上加 1，所產生的值`--`運算子會傳回所產生的減去 1 的運算元的值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1182">The predefined `++` operators return the value produced by adding 1 to the operand, and the predefined `--` operators return the value produced by subtracting 1 from the operand.</span></span> <span data-ttu-id="abcc0-1183">在 `checked`情況下，如果這個加法或減法運算的結果超出結果型別的範圍和結果型別是整數類資料類型或列舉型別，`System.OverflowException`就會擲回。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1183">In a `checked` context, if the result of this addition or subtraction is outside the range of the result type and the result type is an integral type or enum type, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="abcc0-1184">執行階段的處理後置遞增或遞減運算的表單`x++`或`x--`包含下列步驟：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1184">The run-time processing of a postfix increment or decrement operation of the form `x++` or `x--` consists of the following steps:</span></span>

*   <span data-ttu-id="abcc0-1185">如果`x`分類為變數：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1185">If `x` is classified as a variable:</span></span>
    * <span data-ttu-id="abcc0-1186">`x` 在評估後產生的變數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1186">`x` is evaluated to produce the variable.</span></span>
    * <span data-ttu-id="abcc0-1187">值`x`儲存。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1187">The value of `x` is saved.</span></span>
    * <span data-ttu-id="abcc0-1188">選取的運算子會叫用儲存的值是`x`作為其引數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1188">The selected operator is invoked with the saved value of `x` as its argument.</span></span>
    * <span data-ttu-id="abcc0-1189">運算子的傳回值會儲存在評估所指定的位置`x`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1189">The value returned by the operator is stored in the location given by the evaluation of `x`.</span></span>
    * <span data-ttu-id="abcc0-1190">已儲存的值`x`變成作業的結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1190">The saved value of `x` becomes the result of the operation.</span></span>
*   <span data-ttu-id="abcc0-1191">如果`x`歸類為屬性或索引子的存取：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1191">If `x` is classified as a property or indexer access:</span></span>
    * <span data-ttu-id="abcc0-1192">執行個體運算式 (如果`x`不是`static`) 和引數清單 (如果`x`是索引子存取) 相關聯`x`會進行評估，而且結果會用於後續`get`和`set`存取子引動過程。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1192">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `get` and `set` accessor invocations.</span></span>
    * <span data-ttu-id="abcc0-1193">`get`存取子`x`叫用，並儲存傳回的值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1193">The `get` accessor of `x` is invoked and the returned value is saved.</span></span>
    * <span data-ttu-id="abcc0-1194">選取的運算子會叫用儲存的值是`x`作為其引數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1194">The selected operator is invoked with the saved value of `x` as its argument.</span></span>
    * <span data-ttu-id="abcc0-1195">`set`存取子`x`會使用該運算子作為所傳回的值叫用其`value`引數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1195">The `set` accessor of `x` is invoked with the value returned by the operator as its `value` argument.</span></span>
    * <span data-ttu-id="abcc0-1196">已儲存的值`x`變成作業的結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1196">The saved value of `x` becomes the result of the operation.</span></span>

<span data-ttu-id="abcc0-1197">`++`並`--`運算子也支援前置標記法 ([前置遞增和遞減運算子](expressions.md#prefix-increment-and-decrement-operators))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1197">The `++` and `--` operators also support prefix notation ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="abcc0-1198">一般而言，結果`x++`或`x--`的值`x`作業之前，而結果`++x`或`--x`的值`x`作業之後。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1198">Typically, the result of `x++` or `x--` is the value of `x` before the operation, whereas the result of `++x` or `--x` is the value of `x` after the operation.</span></span> <span data-ttu-id="abcc0-1199">在任一情況下，`x`本身具有相同的值在作業之後。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1199">In either case, `x` itself has the same value after the operation.</span></span>

<span data-ttu-id="abcc0-1200">`operator ++`或`operator --`實作可以使用來叫用前置詞或後置標記法。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1200">An `operator ++` or `operator --` implementation can be invoked using either postfix or prefix notation.</span></span> <span data-ttu-id="abcc0-1201">您不可以有兩種標記法的個別運算子實作。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1201">It is not possible to have separate operator implementations for the two notations.</span></span>

### <a name="the-new-operator"></a><span data-ttu-id="abcc0-1202">new 運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-1202">The new operator</span></span>

<span data-ttu-id="abcc0-1203">`new`運算子用來建立新的執行個體的型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1203">The `new` operator is used to create new instances of types.</span></span>

<span data-ttu-id="abcc0-1204">有三種形式的`new`運算式：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1204">There are three forms of `new` expressions:</span></span>

*  <span data-ttu-id="abcc0-1205">物件建立運算式用來建立新的執行個體的類別型別和實值型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1205">Object creation expressions are used to create new instances of class types and value types.</span></span>
*  <span data-ttu-id="abcc0-1206">陣列建立運算式用來建立新的執行個體的陣列類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1206">Array creation expressions are used to create new instances of array types.</span></span>
*  <span data-ttu-id="abcc0-1207">委派建立運算式用來建立委派的新執行個體類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1207">Delegate creation expressions are used to create new instances of delegate types.</span></span>

<span data-ttu-id="abcc0-1208">`new`運算子隱含建立的類型執行個體，但不一定表示動態配置的記憶體。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1208">The `new` operator implies creation of an instance of a type, but does not necessarily imply dynamic allocation of memory.</span></span> <span data-ttu-id="abcc0-1209">尤其，實值型別的執行個體需要任何額外的記憶體超過變數所在，和任何動態配置發生時`new`用來建立實值型別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1209">In particular, instances of value types require no additional memory beyond the variables in which they reside, and no dynamic allocations occur when `new` is used to create instances of value types.</span></span>

#### <a name="object-creation-expressions"></a><span data-ttu-id="abcc0-1210">物件建立運算式</span><span class="sxs-lookup"><span data-stu-id="abcc0-1210">Object creation expressions</span></span>

<span data-ttu-id="abcc0-1211">*Object_creation_expression*用來建立的新執行個體*class_type*或是*value_type*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1211">An *object_creation_expression* is used to create a new instance of a *class_type* or a *value_type*.</span></span>

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

<span data-ttu-id="abcc0-1212">*型別*的*object_creation_expression*必須是*class_type*，則*value_type*或*type_parameter*.</span><span class="sxs-lookup"><span data-stu-id="abcc0-1212">The *type* of an *object_creation_expression* must be a *class_type*, a *value_type* or a *type_parameter*.</span></span> <span data-ttu-id="abcc0-1213">*型別*不得`abstract` *class_type*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1213">The *type* cannot be an `abstract` *class_type*.</span></span>

<span data-ttu-id="abcc0-1214">選擇性*argument_list* ([引數清單](expressions.md#argument-lists))，才允許*型別*是*class_type*或*struct_型別*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1214">The optional *argument_list* ([Argument lists](expressions.md#argument-lists)) is permitted only if the *type* is a *class_type* or a *struct_type*.</span></span>

<span data-ttu-id="abcc0-1215">物件建立運算式可以省略建構函式引數清單和封閉的括號提供它包含的物件初始設定式 」 或 「 集合初始設定式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1215">An object creation expression can omit the constructor argument list and enclosing parentheses provided it includes an object initializer or collection initializer.</span></span> <span data-ttu-id="abcc0-1216">省略建構函式引數清單和封入括號相當於指定空白的引數清單。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1216">Omitting the constructor argument list and enclosing parentheses is equivalent to specifying an empty argument list.</span></span>

<span data-ttu-id="abcc0-1217">處理的物件建立運算式，其中包含的物件初始設定式 」 或 「 集合初始設定式包含第一次處理的執行個體建構函式，然後再處理成員或項目初始設定指定的物件初始設定式 ([物件初始設定式](expressions.md#object-initializers)) 或集合初始設定式 ([集合初始設定式](expressions.md#collection-initializers))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1217">Processing of an object creation expression that includes an object initializer or collection initializer consists of first processing the instance constructor and then processing the member or element initializations specified by the object initializer ([Object initializers](expressions.md#object-initializers)) or collection initializer ([Collection initializers](expressions.md#collection-initializers)).</span></span>

<span data-ttu-id="abcc0-1218">如果任一項中選擇性的引數*argument_list*已在編譯時期型別`dynamic`則*object_creation_expression*動態繫結 ([動態繫結](expressions.md#dynamic-binding))而下列規則會套用在執行階段使用的這些引數的執行階段型別*argument_list*具有的編譯時間類型`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1218">If any of the arguments in the optional *argument_list* has the compile-time type `dynamic` then the *object_creation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) and the following rules are applied at run-time using the run-time type of those arguments of the *argument_list* that have the compile time type `dynamic`.</span></span> <span data-ttu-id="abcc0-1219">不過，在物件的建立就會進行有限的編譯時間檢查中所述[編譯時期檢查動態的多載解析](expressions.md#compile-time-checking-of-dynamic-overload-resolution)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1219">However, the object creation undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="abcc0-1220">繫結時間處理*object_creation_expression*的表單`new T(A)`，其中`T`是*class_type*或*value_type*並`A`是選擇性*argument_list*，包含下列步驟：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1220">The binding-time processing of an *object_creation_expression* of the form `new T(A)`, where `T` is a *class_type* or a *value_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*   <span data-ttu-id="abcc0-1221">如果`T`已*value_type*和`A`不存在：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1221">If `T` is a *value_type* and `A` is not present:</span></span>
    * <span data-ttu-id="abcc0-1222">*Object_creation_expression*是預設建構函式引動過程。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1222">The *object_creation_expression* is a default constructor invocation.</span></span> <span data-ttu-id="abcc0-1223">結果*object_creation_expression*是類型的值`T`，也就是預設值`T`中所定義[System.ValueType 類型](types.md#the-systemvaluetype-type)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1223">The result of the *object_creation_expression* is a value of type `T`, namely the default value for `T` as defined in [The System.ValueType type](types.md#the-systemvaluetype-type).</span></span>
*   <span data-ttu-id="abcc0-1224">否則，如果`T`已*type_parameter*和`A`不存在：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1224">Otherwise, if `T` is a *type_parameter* and `A` is not present:</span></span>
    * <span data-ttu-id="abcc0-1225">如果沒有實值類型條件約束或建構函式條件約束 ([類型參數條件約束](classes.md#type-parameter-constraints)) 已指定為`T`，則繫結階段會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1225">If no value type constraint or constructor constraint ([Type parameter constraints](classes.md#type-parameter-constraints)) has been specified for `T`, a binding-time error occurs.</span></span>
    * <span data-ttu-id="abcc0-1226">結果*object_creation_expression*是值型別的執行階段已繫結的型別參數，也就是叫用該類型的預設建構函式的結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1226">The result of the *object_creation_expression* is a value of the run-time type that the type parameter has been bound to, namely the result of invoking the default constructor of that type.</span></span> <span data-ttu-id="abcc0-1227">執行階段型別可能是參考型別或實值型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1227">The run-time type may be a reference type or a value type.</span></span>
*   <span data-ttu-id="abcc0-1228">否則，如果`T`已*class_type*或是*struct_type*:</span><span class="sxs-lookup"><span data-stu-id="abcc0-1228">Otherwise, if `T` is a *class_type* or a *struct_type*:</span></span>
    * <span data-ttu-id="abcc0-1229">如果`T`已`abstract` *class_type*，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1229">If `T` is an `abstract` *class_type*, a compile-time error occurs.</span></span>
    * <span data-ttu-id="abcc0-1230">使用的多載解析規則來決定要叫用的執行個體建構函式[多載解析](expressions.md#overload-resolution)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1230">The instance constructor to invoke is determined using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="abcc0-1231">候選項目執行個體建構函式的集合，包含的所有可存取的執行個體建構函式中宣告`T`這是相對於適用於`A`([適用的函式成員](expressions.md#applicable-function-member))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1231">The set of candidate instance constructors consists of all accessible instance constructors declared in `T` which are applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="abcc0-1232">如果候選項目執行個體建構函式的資料集是空的或如果找不到單一最佳的執行個體建構函式，繫結階段會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1232">If the set of candidate instance constructors is empty, or if a single best instance constructor cannot be identified, a binding-time error occurs.</span></span>
    * <span data-ttu-id="abcc0-1233">結果*object_creation_expression*是類型的值`T`，也就是叫用上述步驟中決定的執行個體建構函式所產生的值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1233">The result of the *object_creation_expression* is a value of type `T`, namely the value produced by invoking the instance constructor determined in the step above.</span></span>
*  <span data-ttu-id="abcc0-1234">否則，請*object_creation_expression*無效，並在繫結階段錯誤發生。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1234">Otherwise, the *object_creation_expression* is invalid, and a binding-time error occurs.</span></span>

<span data-ttu-id="abcc0-1235">即使*object_creation_expression*動態繫結，在編譯時期型別仍`T`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1235">Even if the *object_creation_expression* is dynamically bound, the compile-time type is still `T`.</span></span>

<span data-ttu-id="abcc0-1236">執行階段處理*object_creation_expression*的表單`new T(A)`，其中`T`會*class_type*或*struct_type*和`A`是選擇性*argument_list*，包含下列步驟：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1236">The run-time processing of an *object_creation_expression* of the form `new T(A)`, where `T` is *class_type* or a *struct_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*   <span data-ttu-id="abcc0-1237">如果`T`已*class_type*:</span><span class="sxs-lookup"><span data-stu-id="abcc0-1237">If `T` is a *class_type*:</span></span>
    * <span data-ttu-id="abcc0-1238">類別的新執行個體`T`配置。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1238">A new instance of class `T` is allocated.</span></span> <span data-ttu-id="abcc0-1239">如果不是記憶體不足，無法配置新的執行個體，`System.OutOfMemoryException`就會擲回，而且會執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1239">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="abcc0-1240">新執行個體的所有欄位都初始化為其預設值 ([預設值](variables.md#default-values))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1240">All fields of the new instance are initialized to their default values ([Default values](variables.md#default-values)).</span></span>
    * <span data-ttu-id="abcc0-1241">執行個體建構函式會叫用根據函式成員引動過程的規則 ([編譯時期檢查動態的多載解析](expressions.md#compile-time-checking-of-dynamic-overload-resolution))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1241">The instance constructor is invoked according to the rules of function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span> <span data-ttu-id="abcc0-1242">新配置的執行個體的參考會自動傳遞給執行個體建構函式，並可以從存取的執行個體，做為該建構函式內`this`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1242">A reference to the newly allocated instance is automatically passed to the instance constructor and the instance can be accessed from within that constructor as `this`.</span></span>
*   <span data-ttu-id="abcc0-1243">如果`T`已*struct_type*:</span><span class="sxs-lookup"><span data-stu-id="abcc0-1243">If `T` is a *struct_type*:</span></span>
    * <span data-ttu-id="abcc0-1244">型別的執行個體`T`由配置暫時的區域變數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1244">An instance of type `T` is created by allocating a temporary local variable.</span></span> <span data-ttu-id="abcc0-1245">之後的執行個體建構函式*struct_type*才能明確地將值指派給每個欄位所建立的暫存變數未初始化是必要的執行個體。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1245">Since an instance constructor of a *struct_type* is required to definitely assign a value to each field of the instance being created, no initialization of the temporary variable is necessary.</span></span>
    * <span data-ttu-id="abcc0-1246">執行個體建構函式會叫用根據函式成員引動過程的規則 ([編譯時期檢查動態的多載解析](expressions.md#compile-time-checking-of-dynamic-overload-resolution))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1246">The instance constructor is invoked according to the rules of function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span> <span data-ttu-id="abcc0-1247">新配置的執行個體的參考會自動傳遞給執行個體建構函式，並可以從存取的執行個體，做為該建構函式內`this`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1247">A reference to the newly allocated instance is automatically passed to the instance constructor and the instance can be accessed from within that constructor as `this`.</span></span>

#### <a name="object-initializers"></a><span data-ttu-id="abcc0-1248">物件初始設定式</span><span class="sxs-lookup"><span data-stu-id="abcc0-1248">Object initializers</span></span>

<span data-ttu-id="abcc0-1249">***物件初始設定式***指定零或多個欄位、 屬性或索引的項目之物件的值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1249">An ***object initializer*** specifies values for zero or more fields, properties or indexed elements of an object.</span></span>

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

<span data-ttu-id="abcc0-1250">物件初始設定包含成員初始設定式，用括住的一連串`{`和`}`語彙基元，並以逗號分隔。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1250">An object initializer consists of a sequence of member initializers, enclosed by `{` and `}` tokens and separated by commas.</span></span> <span data-ttu-id="abcc0-1251">每個*member_initializer*指定初始設定的目標。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1251">Each *member_initializer* designates a target for the initialization.</span></span> <span data-ttu-id="abcc0-1252">*識別碼*必須命名為可存取的欄位或屬性的物件初始化時，而*argument_list*括在方括號必須指定可存取索引子的引數上正在初始化的物件。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1252">An *identifier* must name an accessible field or property of the object being initialized, whereas an *argument_list* enclosed in square brackets must specify arguments for an accessible indexer on the object being initialized.</span></span> <span data-ttu-id="abcc0-1253">它是包含一個以上的成員初始設定式相同的欄位或屬性的物件初始設定式的錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1253">It is an error for an object initializer to include more than one member initializer for the same field or property.</span></span>

<span data-ttu-id="abcc0-1254">每個*initializer_target*後面號和運算式、 物件初始設定式或集合初始設定式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1254">Each *initializer_target* is followed by an equals sign and either an expression, an object initializer or a collection initializer.</span></span> <span data-ttu-id="abcc0-1255">您不可能以指向新建立的物件，它會初始化物件初始設定式內的運算式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1255">It is not possible for expressions within the object initializer to refer to the newly created object it is initializing.</span></span>

<span data-ttu-id="abcc0-1256">指定運算式中指派的相同方式來處理在等號之後的成員初始設定式 ([簡單指派](expressions.md#simple-assignment)) 為目標。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1256">A member initializer that specifies an expression after the equals sign is processed in the same way as an assignment ([Simple assignment](expressions.md#simple-assignment)) to the target.</span></span>

<span data-ttu-id="abcc0-1257">指定的物件初始設定式，等號之後的成員初始設定式***巢狀的物件初始設定式***，也就是，內嵌物件的初始化。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1257">A member initializer that specifies an object initializer after the equals sign is a ***nested object initializer***, i.e. an initialization of an embedded object.</span></span> <span data-ttu-id="abcc0-1258">而不是將新的值指派給欄位或屬性中，巢狀的物件初始設定式中的指派會被視為指派的欄位或屬性的成員。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1258">Instead of assigning a new value to the field or property, the assignments in the nested object initializer are treated as assignments to members of the field or property.</span></span> <span data-ttu-id="abcc0-1259">無法套用巢狀的物件初始設定式，屬性具有實值類型，或搭配實值類型的唯讀欄位。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1259">Nested object initializers cannot be applied to properties with a value type, or to read-only fields with a value type.</span></span>

<span data-ttu-id="abcc0-1260">指定等號後面的 「 集合初始設定式的成員初始設定式是內嵌集合初始化。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1260">A member initializer that specifies a collection initializer after the equals sign is an initialization of an embedded collection.</span></span> <span data-ttu-id="abcc0-1261">而不是將新的集合指派給目標欄位、 屬性或索引子，指定初始設定式中的項目會新增至目標所參考的集合。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1261">Instead of assigning a new collection to the target field, property or indexer, the elements given in the initializer are added to the collection referenced by the target.</span></span> <span data-ttu-id="abcc0-1262">目標必須是符合指定需求的集合型別[集合初始設定式](expressions.md#collection-initializers)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1262">The target must be of a collection type that satisfies the requirements specified in [Collection initializers](expressions.md#collection-initializers).</span></span>

<span data-ttu-id="abcc0-1263">索引初始設定式的引數一律會評估一次。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1263">The arguments to an index initializer will always be evaluated exactly once.</span></span> <span data-ttu-id="abcc0-1264">因此，即使引數會最後會永遠不會取得使用 （例如因為空的巢狀初始設定式），因此會評估其副作用。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1264">Thus, even if the arguments end up never getting used (e.g. because of an empty nested initializer), they will be evaluated for their side effects.</span></span>

<span data-ttu-id="abcc0-1265">下列類別代表具有兩個座標的點：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1265">The following class represents a point with two coordinates:</span></span>
```csharp
public class Point
{
    int x, y;

    public int X { get { return x; } set { x = value; } }
    public int Y { get { return y; } set { y = value; } }
}
```

<span data-ttu-id="abcc0-1266">執行個體`Point`可以建立和初始化，如下所示：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1266">An instance of `Point` can be created and initialized as follows:</span></span>
```csharp
Point a = new Point { X = 0, Y = 1 };
```
<span data-ttu-id="abcc0-1267">具有相同的效果</span><span class="sxs-lookup"><span data-stu-id="abcc0-1267">which has the same effect as</span></span>
```csharp
Point __a = new Point();
__a.X = 0;
__a.Y = 1; 
Point a = __a;
```
<span data-ttu-id="abcc0-1268">其中`__a`是否則不可見，而且無法存取暫存變數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1268">where `__a` is an otherwise invisible and inaccessible temporary variable.</span></span> <span data-ttu-id="abcc0-1269">下列類別表示建立兩個點的矩形：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1269">The following class represents a rectangle created from two points:</span></span>
```csharp
public class Rectangle
{
    Point p1, p2;

    public Point P1 { get { return p1; } set { p1 = value; } }
    public Point P2 { get { return p2; } set { p2 = value; } }
}
```

<span data-ttu-id="abcc0-1270">執行個體`Rectangle`可以建立和初始化，如下所示：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1270">An instance of `Rectangle` can be created and initialized as follows:</span></span>
```csharp
Rectangle r = new Rectangle {
    P1 = new Point { X = 0, Y = 1 },
    P2 = new Point { X = 2, Y = 3 }
};
```
<span data-ttu-id="abcc0-1271">具有相同的效果</span><span class="sxs-lookup"><span data-stu-id="abcc0-1271">which has the same effect as</span></span>
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
<span data-ttu-id="abcc0-1272">何處`__r`，`__p1`和`__p2`是暫存變數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1272">where `__r`, `__p1` and `__p2` are temporary variables that are otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="abcc0-1273">如果`Rectangle`的建構函式會配置兩個內嵌`Point`執行個體</span><span class="sxs-lookup"><span data-stu-id="abcc0-1273">If `Rectangle`'s constructor allocates the two embedded `Point` instances</span></span>
```csharp
public class Rectangle
{
    Point p1 = new Point();
    Point p2 = new Point();

    public Point P1 { get { return p1; } }
    public Point P2 { get { return p2; } }
}
```
<span data-ttu-id="abcc0-1274">下列建構可以用來初始化內嵌`Point`而不是指派新的執行個體的執行個體：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1274">the following construct can be used to initialize the embedded `Point` instances instead of assigning new instances:</span></span>
```csharp
Rectangle r = new Rectangle {
    P1 = { X = 0, Y = 1 },
    P2 = { X = 2, Y = 3 }
};
```
<span data-ttu-id="abcc0-1275">具有相同的效果</span><span class="sxs-lookup"><span data-stu-id="abcc0-1275">which has the same effect as</span></span>
```csharp
Rectangle __r = new Rectangle();
__r.P1.X = 0;
__r.P1.Y = 1;
__r.P2.X = 2;
__r.P2.Y = 3;
Rectangle r = __r;
```

<span data-ttu-id="abcc0-1276">如果已指定 C，下列範例的適當定義：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1276">Given an appropriate definition of C, the following example:</span></span>
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
<span data-ttu-id="abcc0-1277">相當於這一系列的 指派：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1277">is equivalent to this series of assignments:</span></span>
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
<span data-ttu-id="abcc0-1278">其中`__c`等，是不可見且無法存取原始程式碼的產生的變數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1278">where `__c`, etc., are generated variables that are invisible and inaccessible to the source code.</span></span> <span data-ttu-id="abcc0-1279">請注意，引數`[0,0]`評估一次，且引數`[1,2]`即使它們永遠不會用一次評估。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1279">Note that the arguments for `[0,0]` are evaluated only once, and the arguments for `[1,2]` are evaluated once even though they are never used.</span></span>

#### <a name="collection-initializers"></a><span data-ttu-id="abcc0-1280">集合初始設定式</span><span class="sxs-lookup"><span data-stu-id="abcc0-1280">Collection initializers</span></span>

<span data-ttu-id="abcc0-1281">集合初始設定式中指定之集合的元素。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1281">A collection initializer specifies the elements of a collection.</span></span>

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

<span data-ttu-id="abcc0-1282">集合初始設定式所組成的項目初始設定式，用括住的一連串`{`和`}`語彙基元，並以逗號分隔。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1282">A collection initializer consists of a sequence of element initializers, enclosed by `{` and `}` tokens and separated by commas.</span></span> <span data-ttu-id="abcc0-1283">每個項目初始設定式指定要加入至要初始化的集合物件的項目，並且所括住的運算式清單所組成`{`和`}`語彙基元，並以逗號分隔。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1283">Each element initializer specifies an element to be added to the collection object being initialized, and consists of a list of expressions enclosed by `{` and `}` tokens and separated by commas.</span></span>  <span data-ttu-id="abcc0-1284">單一運算式項目初始設定式可以撰寫沒有大括號，但不能再作為指派運算式，若要避免成員初始設定式模稜兩可。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1284">A single-expression element initializer can be written without braces, but cannot then be an assignment expression, to avoid ambiguity with member initializers.</span></span> <span data-ttu-id="abcc0-1285">*Non_assignment_expression*中所定義的生產環境[運算式](expressions.md#expression)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1285">The *non_assignment_expression* production is defined in [Expression](expressions.md#expression).</span></span>

<span data-ttu-id="abcc0-1286">包含集合初始設定式的物件建立運算式的範例如下：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1286">The following is an example of an object creation expression that includes a collection initializer:</span></span>
```csharp
List<int> digits = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
```

<span data-ttu-id="abcc0-1287">要套用的集合初始設定式的集合物件必須實作的型別`System.Collections.IEnumerable`或發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1287">The collection object to which a collection initializer is applied must be of a type that implements `System.Collections.IEnumerable` or a compile-time error occurs.</span></span> <span data-ttu-id="abcc0-1288">每個指定項目順序，集合初始設定式會叫用`Add`目標上的方法作為引數清單中，套用一般成員查閱物件的項目初始設定式的運算式清單，並多載解析每次叫用。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1288">For each specified element in order, the collection initializer invokes an `Add` method on the target object with the expression list of the element initializer as argument list, applying normal member lookup and overload resolution for each invocation.</span></span> <span data-ttu-id="abcc0-1289">因此，集合物件必須有適用的執行個體或擴充方法名稱`Add`每個項目初始設定式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1289">Thus, the collection object must have an applicable instance or extension method with the name `Add` for each element initializer.</span></span>

<span data-ttu-id="abcc0-1290">下列類別代表連絡人名稱和電話號碼清單：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1290">The following class represents a contact with a name and a list of phone numbers:</span></span>
```csharp
public class Contact
{
    string name;
    List<string> phoneNumbers = new List<string>();

    public string Name { get { return name; } set { name = value; } }

    public List<string> PhoneNumbers { get { return phoneNumbers; } }
}
```

<span data-ttu-id="abcc0-1291">A`List<Contact>`可以建立和初始化，如下所示：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1291">A `List<Contact>` can be created and initialized as follows:</span></span>
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
<span data-ttu-id="abcc0-1292">具有相同的效果</span><span class="sxs-lookup"><span data-stu-id="abcc0-1292">which has the same effect as</span></span>
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
<span data-ttu-id="abcc0-1293">何處`__clist`，`__c1`和`__c2`是暫存變數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1293">where `__clist`, `__c1` and `__c2` are temporary variables that are otherwise invisible and inaccessible.</span></span>

#### <a name="array-creation-expressions"></a><span data-ttu-id="abcc0-1294">陣列建立運算式</span><span class="sxs-lookup"><span data-stu-id="abcc0-1294">Array creation expressions</span></span>

<span data-ttu-id="abcc0-1295">*Array_creation_expression*用來建立的新執行個體*array_type*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1295">An *array_creation_expression* is used to create a new instance of an *array_type*.</span></span>

```antlr
array_creation_expression
    : 'new' non_array_type '[' expression_list ']' rank_specifier* array_initializer?
    | 'new' array_type array_initializer
    | 'new' rank_specifier array_initializer
    ;
```

<span data-ttu-id="abcc0-1296">陣列建立運算式的第一種形式會配置陣列類型的執行個體所產生的刪除運算式清單中的每個個別的運算式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1296">An array creation expression of the first form allocates an array instance of the type that results from deleting each of the individual expressions from the expression list.</span></span> <span data-ttu-id="abcc0-1297">例如，陣列建立運算式`new int[10,20]`產生的型別陣列執行個體`int[,]`，和陣列建立運算式`new int[10][,]`會產生型別的陣列`int[][,]`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1297">For example, the array creation expression `new int[10,20]` produces an array instance of type `int[,]`, and the array creation expression `new int[10][,]` produces an array of type `int[][,]`.</span></span> <span data-ttu-id="abcc0-1298">運算式清單中的每一個運算式必須是型別`int`， `uint`， `long`，或`ulong`，或隱含地轉換成一或多個這些型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1298">Each expression in the expression list must be of type `int`, `uint`, `long`, or `ulong`, or implicitly convertible to one or more of these types.</span></span> <span data-ttu-id="abcc0-1299">每個運算式的值會決定新配置的陣列執行個體中之相對應維度的長度。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1299">The value of each expression determines the length of the corresponding dimension in the newly allocated array instance.</span></span> <span data-ttu-id="abcc0-1300">因為陣列維度的長度必須為非負的它是編譯時期錯誤*constant_expression*以負數值，運算式清單中。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1300">Since the length of an array dimension must be nonnegative, it is a compile-time error to have a *constant_expression* with a negative value in the expression list.</span></span>

<span data-ttu-id="abcc0-1301">除非在不安全的內容 ([Unsafe 內容](unsafe-code.md#unsafe-contexts))，未指定陣列的配置。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1301">Except in an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)), the layout of arrays is unspecified.</span></span>

<span data-ttu-id="abcc0-1302">如果陣列建立運算式的第一個表單包含陣列初始設定式，在運算式清單中的每個運算式必須是常數，而且運算式清單所指定的順位和維度長度必須符合的陣列初始設定式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1302">If an array creation expression of the first form includes an array initializer, each expression in the expression list must be a constant and the rank and dimension lengths specified by the expression list must match those of the array initializer.</span></span>

<span data-ttu-id="abcc0-1303">在第二個或第三個表單的陣列建立運算式，指定的陣列型別或陣序規範的陣序規範必須符合的陣列初始設定式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1303">In an array creation expression of the second or third form, the rank of the specified array type or rank specifier must match that of the array initializer.</span></span> <span data-ttu-id="abcc0-1304">個別的維度長度會推斷從每個對應的巢狀層級的陣列初始設定式中的項目數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1304">The individual dimension lengths are inferred from the number of elements in each of the corresponding nesting levels of the array initializer.</span></span> <span data-ttu-id="abcc0-1305">因此，運算式</span><span class="sxs-lookup"><span data-stu-id="abcc0-1305">Thus, the expression</span></span>
```csharp
new int[,] {{0, 1}, {2, 3}, {4, 5}}
```
<span data-ttu-id="abcc0-1306">若要完全對應</span><span class="sxs-lookup"><span data-stu-id="abcc0-1306">exactly corresponds to</span></span>
```csharp
new int[3, 2] {{0, 1}, {2, 3}, {4, 5}}
```

<span data-ttu-id="abcc0-1307">陣列建立運算式的第三種形式指***隱含型別陣列建立運算式***。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1307">An array creation expression of the third form is referred to as an ***implicitly typed array creation expression***.</span></span> <span data-ttu-id="abcc0-1308">它是類似於第二種形式中，不同之處在於陣列的項目類型並未明確指定，但決定做為最常見的類型 ([找出一組運算式的最常見類型](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) 集合的陣列中的運算式初始設定式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1308">It is similar to the second form, except that the element type of the array is not explicitly given, but determined as the best common type ([Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) of the set of expressions in the array initializer.</span></span> <span data-ttu-id="abcc0-1309">多維度的陣列，也就是那個*rank_specifier*包含至少一個逗號，這個集合包含所有*運算式*s 中找到的巢狀*array_initializer*s。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1309">For a multidimensional array, i.e., one where the *rank_specifier* contains at least one comma, this set comprises all *expression*s found in nested *array_initializer*s.</span></span>

<span data-ttu-id="abcc0-1310">陣列初始設定式中所述進一步[陣列初始設定式](arrays.md#array-initializers)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1310">Array initializers are described further in [Array initializers](arrays.md#array-initializers).</span></span>

<span data-ttu-id="abcc0-1311">評估陣列建立運算式的結果會分類為值時，也就是新配置的陣列執行個體的參考。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1311">The result of evaluating an array creation expression is classified as a value, namely a reference to the newly allocated array instance.</span></span> <span data-ttu-id="abcc0-1312">執行階段處理的陣列建立運算式包含下列步驟：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1312">The run-time processing of an array creation expression consists of the following steps:</span></span>

*  <span data-ttu-id="abcc0-1313">維度長度運算式*expression_list*的評估順序，從左到右。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1313">The dimension length expressions of the *expression_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="abcc0-1314">遵循每個運算式，隱含轉換的評估結果 ([隱含轉換](conversions.md#implicit-conversions)) 執行下列類型的其中一個： `int`， `uint`， `long`， `ulong`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1314">Following evaluation of each expression, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to one of the following types is performed: `int`, `uint`, `long`, `ulong`.</span></span> <span data-ttu-id="abcc0-1315">第一個類型的隱含轉換存在，這份清單中選擇。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1315">The first type in this list for which an implicit conversion exists is chosen.</span></span> <span data-ttu-id="abcc0-1316">如果評估的運算式或後續的隱含轉換導致例外狀況，然後會評估任何進一步的運算式，並執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1316">If evaluation of an expression or the subsequent implicit conversion causes an exception, then no further expressions are evaluated and no further steps are executed.</span></span>
*  <span data-ttu-id="abcc0-1317">維度長度的計算的值會進行驗證，如下所示。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1317">The computed values for the dimension lengths are validated as follows.</span></span> <span data-ttu-id="abcc0-1318">如果一或多個值都小於零，`System.OverflowException`就會擲回，而且會執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1318">If one or more of the values are less than zero, a `System.OverflowException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="abcc0-1319">配置指定的維度長度的陣列執行個體。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1319">An array instance with the given dimension lengths is allocated.</span></span> <span data-ttu-id="abcc0-1320">如果不是記憶體不足，無法配置新的執行個體，`System.OutOfMemoryException`就會擲回，而且會執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1320">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="abcc0-1321">將新的陣列執行個體的所有項目都初始化為其預設值 ([預設值](variables.md#default-values))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1321">All elements of the new array instance are initialized to their default values ([Default values](variables.md#default-values)).</span></span>
*  <span data-ttu-id="abcc0-1322">如果陣列建立運算式中包含的陣列初始設定式，陣列初始設定式中的每個運算式評估，並指派給其對應的陣列項目。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1322">If the array creation expression contains an array initializer, then each expression in the array initializer is evaluated and assigned to its corresponding array element.</span></span> <span data-ttu-id="abcc0-1323">評估和指派運算式以陣列初始設定式的順序執行 — 亦即，項目都初始化遞增索引順序，與第一次增加最右邊的維度。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1323">The evaluations and assignments are performed in the order the expressions are written in the array initializer—in other words, elements are initialized in increasing index order, with the rightmost dimension increasing first.</span></span> <span data-ttu-id="abcc0-1324">如果評估指定的運算式或後續指派給對應的陣列項目會導致例外狀況，然後會初始化任何進一步的項目 （和其餘的項目將會因此有其預設值）。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1324">If evaluation of a given expression or the subsequent assignment to the corresponding array element causes an exception, then no further elements are initialized (and the remaining elements will thus have their default values).</span></span>

<span data-ttu-id="abcc0-1325">陣列建立運算式允許具現化陣列的項目陣列型別，但必須以手動方式初始化的這類陣列項目。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1325">An array creation expression permits instantiation of an array with elements of an array type, but the elements of such an array must be manually initialized.</span></span> <span data-ttu-id="abcc0-1326">例如，陳述式</span><span class="sxs-lookup"><span data-stu-id="abcc0-1326">For example, the statement</span></span>
```csharp
int[][] a = new int[100][];
```
<span data-ttu-id="abcc0-1327">使用 100 個元素的類型建立的一維陣列`int[]`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1327">creates a single-dimensional array with 100 elements of type `int[]`.</span></span> <span data-ttu-id="abcc0-1328">每個元素的初始值是`null`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1328">The initial value of each element is `null`.</span></span> <span data-ttu-id="abcc0-1329">它並不適用相同的陣列建立運算式，也和具現化子陣列，該陳述式</span><span class="sxs-lookup"><span data-stu-id="abcc0-1329">It is not possible for the same array creation expression to also instantiate the sub-arrays, and the statement</span></span>
```csharp
int[][] a = new int[100][5];        // Error
```
<span data-ttu-id="abcc0-1330">會導致編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1330">results in a compile-time error.</span></span> <span data-ttu-id="abcc0-1331">必須改為手動，如下所示執行具現化的子陣列</span><span class="sxs-lookup"><span data-stu-id="abcc0-1331">Instantiation of the sub-arrays must instead be performed manually, as in</span></span>
```csharp
int[][] a = new int[100][];
for (int i = 0; i < 100; i++) a[i] = new int[5];
```

<span data-ttu-id="abcc0-1332">陣列的陣列時的 「 矩形 」 圖形，子陣列全都是長度的相同時，它是長度的使用多維陣列更有效率。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1332">When an array of arrays has a "rectangular" shape, that is when the sub-arrays are all of the same length, it is more efficient to use a multi-dimensional array.</span></span> <span data-ttu-id="abcc0-1333">在上述範例中，具現化一維陣列的陣列建立 101 物件 — 一個是外部陣列和 100 的子陣列。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1333">In the example above, instantiation of the array of arrays creates 101 objects—one outer array and 100 sub-arrays.</span></span> <span data-ttu-id="abcc0-1334">相反地，</span><span class="sxs-lookup"><span data-stu-id="abcc0-1334">In contrast,</span></span>
```csharp
int[,] = new int[100, 5];
```
<span data-ttu-id="abcc0-1335">會建立只有單一物件，一個二維陣列，並完成在單一陳述式中的配置。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1335">creates only a single object, a two-dimensional array, and accomplishes the allocation in a single statement.</span></span>

<span data-ttu-id="abcc0-1336">隱含類型的陣列建立運算式的範例如下：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1336">The following are examples of implicitly typed array creation expressions:</span></span>
```csharp
var a = new[] { 1, 10, 100, 1000 };                       // int[]

var b = new[] { 1, 1.5, 2, 2.5 };                         // double[]

var c = new[,] { { "hello", null }, { "world", "!" } };   // string[,]

var d = new[] { 1, "one", 2, "two" };                     // Error
```

<span data-ttu-id="abcc0-1337">最後一個運算式會造成編譯時期錯誤，因為未`int`也不`string`是隱含地轉換成另一個，而因此那里不是最常見輸入。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1337">The last expression causes a compile-time error because neither `int` nor `string` is implicitly convertible to the other, and so there is no best common type.</span></span> <span data-ttu-id="abcc0-1338">明確類型的陣列建立運算式必須使用在此情況下，例如指定的型別是`object[]`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1338">An explicitly typed array creation expression must be used in this case, for example specifying the type to be `object[]`.</span></span> <span data-ttu-id="abcc0-1339">或者，其中一個項目可以轉換成一般的基底類型，它就會成為推斷項目類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1339">Alternatively, one of the elements can be cast to a common base type, which would then become the inferred element type.</span></span>

<span data-ttu-id="abcc0-1340">隱含類型的陣列建立運算式可以結合匿名物件初始設定式 ([匿名物件建立運算式](expressions.md#anonymous-object-creation-expressions)) 來建立匿名型別資料結構。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1340">Implicitly typed array creation expressions can be combined with anonymous object initializers ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) to create anonymously typed data structures.</span></span> <span data-ttu-id="abcc0-1341">例如: </span><span class="sxs-lookup"><span data-stu-id="abcc0-1341">For example:</span></span>
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

#### <a name="delegate-creation-expressions"></a><span data-ttu-id="abcc0-1342">委派建立運算式</span><span class="sxs-lookup"><span data-stu-id="abcc0-1342">Delegate creation expressions</span></span>

<span data-ttu-id="abcc0-1343">A *delegate_creation_expression*用來建立的新執行個體*delegate_type*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1343">A *delegate_creation_expression* is used to create a new instance of a *delegate_type*.</span></span>

```antlr
delegate_creation_expression
    : 'new' delegate_type '(' expression ')'
    ;
```

<span data-ttu-id="abcc0-1344">委派建立運算式的引數必須是方法群組、 匿名函式或編譯時間類型的值`dynamic`或是*delegate_type*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1344">The argument of a delegate creation expression must be a method group, an anonymous function or a value of either the compile time type `dynamic` or a *delegate_type*.</span></span> <span data-ttu-id="abcc0-1345">如果引數的方法群組，它會識別的方法和執行個體方法，為其建立委派的物件。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1345">If the argument is a method group, it identifies the method and, for an instance method, the object for which to create a delegate.</span></span> <span data-ttu-id="abcc0-1346">如果引數的匿名函式直接定義委派目標方法主體與參數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1346">If the argument is an anonymous function it directly defines the parameters and method body of the delegate target.</span></span> <span data-ttu-id="abcc0-1347">如果引數是值，它會識別用來建立複本的委派執行個體。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1347">If the argument is a value it identifies a delegate instance of which to create a copy.</span></span>

<span data-ttu-id="abcc0-1348">如果*運算式*已在編譯時期型別`dynamic`，則*delegate_creation_expression*動態繫結 ([動態繫結](expressions.md#dynamic-binding))，以及下列規則在執行階段使用的執行階段型別套用*運算式*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1348">If the *expression* has the compile-time type `dynamic`, the *delegate_creation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), and the rules below are applied at run-time using the run-time type of the *expression*.</span></span> <span data-ttu-id="abcc0-1349">否則就會在編譯時期套用規則。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1349">Otherwise the rules are applied at compile-time.</span></span>

<span data-ttu-id="abcc0-1350">繫結時間處理*delegate_creation_expression*的表單`new D(E)`，其中`D`是*delegate_type*並`E`是*運算式*，包含下列步驟：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1350">The binding-time processing of a *delegate_creation_expression* of the form `new D(E)`, where `D` is a *delegate_type* and `E` is an *expression*, consists of the following steps:</span></span>

*  <span data-ttu-id="abcc0-1351">如果`E`是方法群組，為方法群組轉換相同的方式處理委派建立運算式 ([方法群組轉換](conversions.md#method-group-conversions)) 從`E`到`D`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1351">If `E` is a method group, the delegate creation expression is processed in the same way as a method group conversion ([Method group conversions](conversions.md#method-group-conversions)) from `E` to `D`.</span></span>
*  <span data-ttu-id="abcc0-1352">如果`E`是匿名的函式的匿名函式轉換為相同的方式處理委派建立運算式 ([匿名函式轉換](conversions.md#anonymous-function-conversions)) 從`E`到`D`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1352">If `E` is an anonymous function, the delegate creation expression is processed in the same way as an anonymous function conversion ([Anonymous function conversions](conversions.md#anonymous-function-conversions)) from `E` to `D`.</span></span>
*  <span data-ttu-id="abcc0-1353">如果`E`是一個值，`E`必須是相容 ([委派宣告](delegates.md#delegate-declarations)) 與`D`，且結果為新建立之型別的委派的參考`D`參考相同的引動過程列為`E`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1353">If `E` is a value, `E` must be compatible ([Delegate declarations](delegates.md#delegate-declarations)) with `D`, and the result is a reference to a newly created delegate of type `D` that refers to the same invocation list as `E`.</span></span> <span data-ttu-id="abcc0-1354">如果`E`與不相容`D`，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1354">If `E` is not compatible with `D`, a compile-time error occurs.</span></span>

<span data-ttu-id="abcc0-1355">執行階段處理*delegate_creation_expression*的表單`new D(E)`，其中`D`是*delegate_type*並`E`是*運算式*，包含下列步驟：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1355">The run-time processing of a *delegate_creation_expression* of the form `new D(E)`, where `D` is a *delegate_type* and `E` is an *expression*, consists of the following steps:</span></span>

*   <span data-ttu-id="abcc0-1356">如果`E`是方法群組，則委派建立運算式會評估為方法群組轉換 ([方法群組轉換](conversions.md#method-group-conversions)) 從`E`到`D`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1356">If `E` is a method group, the delegate creation expression is evaluated as a method group conversion ([Method group conversions](conversions.md#method-group-conversions)) from `E` to `D`.</span></span>
*   <span data-ttu-id="abcc0-1357">如果`E`是匿名的函式委派建立的評估方式的匿名函式轉換`E`要`D`([匿名函式轉換](conversions.md#anonymous-function-conversions))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1357">If `E` is an anonymous function, the delegate creation is evaluated as an anonymous function conversion from `E` to `D` ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>
*   <span data-ttu-id="abcc0-1358">如果`E`的值*delegate_type*:</span><span class="sxs-lookup"><span data-stu-id="abcc0-1358">If `E` is a value of a *delegate_type*:</span></span>
    * <span data-ttu-id="abcc0-1359">`E` 會評估。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1359">`E` is evaluated.</span></span> <span data-ttu-id="abcc0-1360">如果此評估會產生例外狀況，則會不執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1360">If this evaluation causes an exception, no further steps are executed.</span></span>
    * <span data-ttu-id="abcc0-1361">如果值`E`已`null`、`System.NullReferenceException`就會擲回，而且會執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1361">If the value of `E` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="abcc0-1362">委派類型的新執行個體`D`配置。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1362">A new instance of the delegate type `D` is allocated.</span></span> <span data-ttu-id="abcc0-1363">如果不是記憶體不足，無法配置新的執行個體，`System.OutOfMemoryException`就會擲回，而且會執行任何進一步的步驟。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1363">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="abcc0-1364">使用相同的引動過程清單，為給定的委派執行個體初始化新的委派執行個體`E`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1364">The new delegate instance is initialized with the same invocation list as the delegate instance given by `E`.</span></span>

<span data-ttu-id="abcc0-1365">委派具現化，並接著會維持不變的委派的整個存留期間時，會決定委派的引動過程清單。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1365">The invocation list of a delegate is determined when the delegate is instantiated and then remains constant for the entire lifetime of the delegate.</span></span> <span data-ttu-id="abcc0-1366">換句話說，不可以變更委派的目標可呼叫實體，一旦建立之後。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1366">In other words, it is not possible to change the target callable entities of a delegate once it has been created.</span></span> <span data-ttu-id="abcc0-1367">當合併兩個委派，或移除其中一個是 ([委派宣告](delegates.md#delegate-declarations))，產生新的委派，任何現有的委派，並變更其內容。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1367">When two delegates are combined or one is removed from another ([Delegate declarations](delegates.md#delegate-declarations)), a new delegate results; no existing delegate has its contents changed.</span></span>

<span data-ttu-id="abcc0-1368">您不可以建立委派，指屬性、 索引子、 使用者定義的運算子、 執行個體建構函式、 解構函式或靜態建構函式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1368">It is not possible to create a delegate that refers to a property, indexer, user-defined operator, instance constructor, destructor, or static constructor.</span></span>

<span data-ttu-id="abcc0-1369">如上面所述，當建立委派從方法群組中，型式參數清單和委派的傳回型別，判斷其中一個多載的方法，來選取。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1369">As described above, when a delegate is created from a method group, the formal parameter list and return type of the delegate determine which of the overloaded methods to select.</span></span> <span data-ttu-id="abcc0-1370">在範例</span><span class="sxs-lookup"><span data-stu-id="abcc0-1370">In the example</span></span>
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
<span data-ttu-id="abcc0-1371">`A.f`欄位會初始化的委派，其中第二個是指`Square`方法的型式參數清單和傳回型別，該方法完全比對因為`DoubleFunc`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1371">the `A.f` field is initialized with a delegate that refers to the second `Square` method because that method exactly matches the formal parameter list and return type of `DoubleFunc`.</span></span> <span data-ttu-id="abcc0-1372">有第二個`Square`尚未存在的方法，會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1372">Had the second `Square` method not been present, a compile-time error would have occurred.</span></span>

#### <a name="anonymous-object-creation-expressions"></a><span data-ttu-id="abcc0-1373">匿名物件建立運算式</span><span class="sxs-lookup"><span data-stu-id="abcc0-1373">Anonymous object creation expressions</span></span>

<span data-ttu-id="abcc0-1374">*Anonymous_object_creation_expression*用來建立匿名型別的物件。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1374">An *anonymous_object_creation_expression* is used to create an object of an anonymous type.</span></span>

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

<span data-ttu-id="abcc0-1375">匿名物件初始設定式宣告匿名型別，並傳回該類型的執行個體。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1375">An anonymous object initializer declares an anonymous type and returns an instance of that type.</span></span> <span data-ttu-id="abcc0-1376">匿名型別是無名稱的類別型別直接繼承自`object`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1376">An anonymous type is a nameless class type that inherits directly from `object`.</span></span> <span data-ttu-id="abcc0-1377">匿名類型的成員是推斷的匿名物件初始設定式用來建立型別的執行個體的唯讀屬性的序列。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1377">The members of an anonymous type are a sequence of read-only properties inferred from the anonymous object initializer used to create an instance of the type.</span></span> <span data-ttu-id="abcc0-1378">具體來說，匿名物件初始設定式的格式</span><span class="sxs-lookup"><span data-stu-id="abcc0-1378">Specifically, an anonymous object initializer of the form</span></span>
```csharp
new { p1 = e1, p2 = e2, ..., pn = en }
```
<span data-ttu-id="abcc0-1379">宣告匿名類型的表單</span><span class="sxs-lookup"><span data-stu-id="abcc0-1379">declares an anonymous type of the form</span></span>
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
<span data-ttu-id="abcc0-1380">其中每個`Tx`是對應的運算式的型別`ex`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1380">where each `Tx` is the type of the corresponding expression `ex`.</span></span> <span data-ttu-id="abcc0-1381">所用的運算式*member_declarator*必須具有型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1381">The expression used in a *member_declarator* must have a type.</span></span> <span data-ttu-id="abcc0-1382">因此，它是在運算式的編譯時期錯誤*member_declarator*為 null 或匿名函式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1382">Thus, it is a compile-time error for an expression in a *member_declarator* to be null or an anonymous function.</span></span> <span data-ttu-id="abcc0-1383">它也是要有不安全類型的運算式的編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1383">It is also a compile-time error for the expression to have an unsafe type.</span></span>

<span data-ttu-id="abcc0-1384">匿名型別和參數的名稱及其`Equals`方法透過編譯器自動產生，而且無法參考的程式文字中。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1384">The names of an anonymous type and of the parameter to its `Equals` method are automatically generated by the compiler and cannot be referenced in program text.</span></span>

<span data-ttu-id="abcc0-1385">在相同的程式中，兩個匿名物件初始設定式的相同順序指定屬性的編譯時間類型與相同名稱的序列將會產生相同的匿名型別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1385">Within the same program, two anonymous object initializers that specify a sequence of properties of the same names and compile-time types in the same order will produce instances of the same anonymous type.</span></span>

<span data-ttu-id="abcc0-1386">在範例</span><span class="sxs-lookup"><span data-stu-id="abcc0-1386">In the example</span></span>
```csharp
var p1 = new { Name = "Lawnmower", Price = 495.00 };
var p2 = new { Name = "Shovel", Price = 26.95 };
p1 = p2;
```
<span data-ttu-id="abcc0-1387">指派的最後一行受到允許，因為`p1`和`p2`都屬於相同匿名型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1387">the assignment on the last line is permitted because `p1` and `p2` are of the same anonymous type.</span></span>

<span data-ttu-id="abcc0-1388">`Equals`和`GetHashcode`匿名型別上的方法覆寫繼承自方法`object`，，而且會定義的形式`Equals`和`GetHashcode`屬性，以便相同匿名型別的兩個執行個體是否相等如果而且，只有在其所有屬性都都相等。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1388">The `Equals` and `GetHashcode` methods on anonymous types override the methods inherited from `object`, and are defined in terms of the `Equals` and `GetHashcode` of the properties, so that two instances of the same anonymous type are equal if and only if all their properties are equal.</span></span>

<span data-ttu-id="abcc0-1389">成員宣告子可縮寫成簡單名稱 ([型別推斷](expressions.md#type-inference))，成員存取 ([編譯時期檢查動態的多載解析](expressions.md#compile-time-checking-of-dynamic-overload-resolution))，基底的存取 ([基底存取](expressions.md#base-access))或 null 條件成員存取 ([Null 條件運算式為投影初始設定式](expressions.md#null-conditional-expressions-as-projection-initializers))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1389">A member declarator can be abbreviated to a simple name ([Type inference](expressions.md#type-inference)), a member access ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), a base access ([Base access](expressions.md#base-access)) or a null-conditional member access ([Null-conditional expressions as projection initializers](expressions.md#null-conditional-expressions-as-projection-initializers)).</span></span> <span data-ttu-id="abcc0-1390">這就叫做***投影初始設定式***和是宣告和指派的屬性具有相同名稱的速記。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1390">This is called a ***projection initializer*** and is shorthand for a declaration of and assignment to a property with the same name.</span></span> <span data-ttu-id="abcc0-1391">具體而言，一種格式的成員宣告子</span><span class="sxs-lookup"><span data-stu-id="abcc0-1391">Specifically, member declarators of the forms</span></span>
```csharp
identifier
expr.identifier
```
<span data-ttu-id="abcc0-1392">分別是相當於下列：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1392">are precisely equivalent to the following, respectively:</span></span>
```csharp
identifier = identifier
identifier = expr.identifier
```

<span data-ttu-id="abcc0-1393">因此，在規劃初始設定式*識別碼*選取同時值和欄位或屬性值會指派。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1393">Thus, in a projection initializer the *identifier* selects both the value and the field or property to which the value is assigned.</span></span> <span data-ttu-id="abcc0-1394">可想而知，投影初始設定式的專案不只是一個值，但也是值的名稱。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1394">Intuitively, a projection initializer projects not just a value, but also the name of the value.</span></span>

### <a name="the-typeof-operator"></a><span data-ttu-id="abcc0-1395">Typeof 運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-1395">The typeof operator</span></span>

<span data-ttu-id="abcc0-1396">`typeof`運算子用來取得`System.Type`型別的物件。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1396">The `typeof` operator is used to obtain the `System.Type` object for a type.</span></span>

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

<span data-ttu-id="abcc0-1397">第一種形式*typeof_expression*組成`typeof`關鍵字後面接著括號括住*類型*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1397">The first form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized *type*.</span></span> <span data-ttu-id="abcc0-1398">這種形式的運算式的結果是`System.Type`指定型別的物件。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1398">The result of an expression of this form is the `System.Type` object for the indicated type.</span></span> <span data-ttu-id="abcc0-1399">只有一個`System.Type`任何指定類型的物件。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1399">There is only one `System.Type` object for any given type.</span></span> <span data-ttu-id="abcc0-1400">這表示型別的 `T`，`typeof(T) == typeof(T)`一定是 true。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1400">This means that for a type `T`, `typeof(T) == typeof(T)` is always true.</span></span> <span data-ttu-id="abcc0-1401">*型別*不能是`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1401">The *type* cannot be `dynamic`.</span></span>

<span data-ttu-id="abcc0-1402">第二個形式*typeof_expression*組成`typeof`關鍵字後面接著括號括住*unbound_type_name*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1402">The second form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized *unbound_type_name*.</span></span> <span data-ttu-id="abcc0-1403">*Unbound_type_name*非常類似於*type_name* ([命名空間和型別名稱](basic-concepts.md#namespace-and-type-names)) 不同之處在於*unbound_type_name*包含*generic_dimension_specifier*s 所在*type_name*包含*type_argument_list*s。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1403">An *unbound_type_name* is very similar to a *type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) except that an *unbound_type_name* contains *generic_dimension_specifier*s where a *type_name* contains *type_argument_list*s.</span></span> <span data-ttu-id="abcc0-1404">當的運算元*typeof_expression*是一連串的語彙基元滿足兩個文法*unbound_type_name*並*type_name*，也就是當它包含既不*generic_dimension_specifier*也不是*type_argument_list*，語彙基元順序會被視為*type_name*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1404">When the operand of a *typeof_expression* is a sequence of tokens that satisfies the grammars of both *unbound_type_name* and *type_name*, namely when it contains neither a *generic_dimension_specifier* nor a *type_argument_list*, the sequence of tokens is considered to be a *type_name*.</span></span> <span data-ttu-id="abcc0-1405">意義*unbound_type_name*判斷方式如下：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1405">The meaning of an *unbound_type_name* is determined as follows:</span></span>

*  <span data-ttu-id="abcc0-1406">轉換到的語彙基元順序*type_name*取代每個*generic_dimension_specifier*具有*type_argument_list*有相同數目的逗號，關鍵字`object`為每個*type_argument*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1406">Convert the sequence of tokens to a *type_name* by replacing each *generic_dimension_specifier* with a *type_argument_list* having the same number of commas and the keyword `object` as each *type_argument*.</span></span>
*  <span data-ttu-id="abcc0-1407">評估所產生的*type_name*，但略過所有的類型參數條件約束。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1407">Evaluate the resulting *type_name*, while ignoring all type parameter constraints.</span></span>
*  <span data-ttu-id="abcc0-1408">*Unbound_type_name*會解析為未繫結的泛型型別，以產生建構的型別相關聯 ([繫結和解除繫結類型](types.md#bound-and-unbound-types))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1408">The *unbound_type_name* resolves to the unbound generic type associated with the resulting constructed type ([Bound and unbound types](types.md#bound-and-unbound-types)).</span></span>

<span data-ttu-id="abcc0-1409">結果*typeof_expression*是`System.Type`物件所產生的未繫結的泛型型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1409">The result of the *typeof_expression* is the `System.Type` object for the resulting unbound generic type.</span></span>

<span data-ttu-id="abcc0-1410">第三種*typeof_expression*組成`typeof`關鍵字後面接著括號括住`void`關鍵字。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1410">The third form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized `void` keyword.</span></span> <span data-ttu-id="abcc0-1411">這種形式的運算式的結果是`System.Type`物件，表示為型別不存在。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1411">The result of an expression of this form is the `System.Type` object that represents the absence of a type.</span></span> <span data-ttu-id="abcc0-1412">所傳回的型別物件`typeof(void)`會傳回任何類型的型別物件有所區別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1412">The type object returned by `typeof(void)` is distinct from the type object returned for any type.</span></span> <span data-ttu-id="abcc0-1413">這個特殊的型別物件適合類別庫可讓方法上的反映在語言中，這些方法要想辦法代表包含 void 方法的執行個體的任何方法的傳回型別中`System.Type`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1413">This special type object is useful in class libraries that allow reflection onto methods in the language, where those methods wish to have a way to represent the return type of any method, including void methods, with an instance of `System.Type`.</span></span>

<span data-ttu-id="abcc0-1414">`typeof`運算子可以用在類型參數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1414">The `typeof` operator can be used on a type parameter.</span></span> <span data-ttu-id="abcc0-1415">結果是`System.Type`已繫結至型別參數的執行階段類型的物件。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1415">The result is the `System.Type` object for the run-time type that was bound to the type parameter.</span></span> <span data-ttu-id="abcc0-1416">`typeof`運算子也可以用在建構的型別或繫結的泛型型別 ([繫結和解除繫結類型](types.md#bound-and-unbound-types))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1416">The `typeof` operator can also be used on a constructed type or an unbound generic type ([Bound and unbound types](types.md#bound-and-unbound-types)).</span></span> <span data-ttu-id="abcc0-1417">`System.Type`物件中繫結的泛型型別不是相同`System.Type`執行個體類型的物件。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1417">The `System.Type` object for an unbound generic type is not the same as the `System.Type` object of the instance type.</span></span> <span data-ttu-id="abcc0-1418">執行個體類型一定是在執行階段的封閉式建構的類型因此其`System.Type`物件取決於執行階段型別引數，在使用中，而未繫結的泛型型別沒有任何型別引數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1418">The instance type is always a closed constructed type at run-time so its `System.Type` object depends on the run-time type arguments in use, while the unbound generic type has no type arguments.</span></span>

<span data-ttu-id="abcc0-1419">此範例</span><span class="sxs-lookup"><span data-stu-id="abcc0-1419">The example</span></span>
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
<span data-ttu-id="abcc0-1420">會產生下列輸出：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1420">produces the following output:</span></span>
```
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

<span data-ttu-id="abcc0-1421">請注意，`int`和`System.Int32`都是相同的型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1421">Note that `int` and `System.Int32` are the same type.</span></span>

<span data-ttu-id="abcc0-1422">也請注意，結果`typeof(X<>)`不相依於型別引數，但結果`typeof(X<T>)`沒有。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1422">Also note that the result of `typeof(X<>)` does not depend on the type argument but the result of `typeof(X<T>)` does.</span></span>

### <a name="the-checked-and-unchecked-operators"></a><span data-ttu-id="abcc0-1423">checked 和 unchecked 運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-1423">The checked and unchecked operators</span></span>

<span data-ttu-id="abcc0-1424">`checked`並`unchecked`運算子可用來控制***溢位檢查內容***整數型別算術運算和轉換。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1424">The `checked` and `unchecked` operators are used to control the ***overflow checking context*** for integral-type arithmetic operations and conversions.</span></span>

```antlr
checked_expression
    : 'checked' '(' expression ')'
    ;

unchecked_expression
    : 'unchecked' '(' expression ')'
    ;
```

<span data-ttu-id="abcc0-1425">`checked`運算子會評估在 checked 內容中，包含的運算式和`unchecked`運算子會評估在 unchecked 內容中包含的運算式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1425">The `checked` operator evaluates the contained expression in a checked context, and the `unchecked` operator evaluates the contained expression in an unchecked context.</span></span> <span data-ttu-id="abcc0-1426">A *checked_expression*或是*unchecked_expression*就相當於*parenthesized_expression* ([括號括住運算式](expressions.md#parenthesized-expressions))，但包含的運算式會評估指定的溢位檢查內容中。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1426">A *checked_expression* or *unchecked_expression* corresponds exactly to a *parenthesized_expression* ([Parenthesized expressions](expressions.md#parenthesized-expressions)), except that the contained expression is evaluated in the given overflow checking context.</span></span>

<span data-ttu-id="abcc0-1427">也可以透過控制溢位檢查內容`checked`並`unchecked`陳述式 ([checked 與 unchecked 陳述式](statements.md#the-checked-and-unchecked-statements))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1427">The overflow checking context can also be controlled through the `checked` and `unchecked` statements ([The checked and unchecked statements](statements.md#the-checked-and-unchecked-statements)).</span></span>

<span data-ttu-id="abcc0-1428">下列作業會受到溢位檢查所建立的內容`checked`和`unchecked`運算子和陳述式：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1428">The following operations are affected by the overflow checking context established by the `checked` and `unchecked` operators and statements:</span></span>

*  <span data-ttu-id="abcc0-1429">預先定義`++`並`--`一元運算子 ([後置遞增和遞減運算子](expressions.md#postfix-increment-and-decrement-operators)並[前置遞增和遞減運算子](expressions.md#prefix-increment-and-decrement-operators))，如果運算元為整數的型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1429">The predefined `++` and `--` unary operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), when the operand is of an integral type.</span></span>
*  <span data-ttu-id="abcc0-1430">預先定義`-`一元運算子 ([一元減號運算子](expressions.md#unary-minus-operator))，如果運算元為整數類資料類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1430">The predefined `-` unary operator ([Unary minus operator](expressions.md#unary-minus-operator)), when the operand is of an integral type.</span></span>
*  <span data-ttu-id="abcc0-1431">預先定義`+`， `-`， `*`，以及`/`二元運算子 ([算術運算子](expressions.md#arithmetic-operators))，當這兩個運算元都是整數類資料類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1431">The predefined `+`, `-`, `*`, and `/` binary operators ([Arithmetic operators](expressions.md#arithmetic-operators)), when both operands are of integral types.</span></span>
*  <span data-ttu-id="abcc0-1432">明確數值轉換 ([明確數值轉換](conversions.md#explicit-numeric-conversions)) 從一種整數類資料類型為另一個整數型別，或從`float`或`double`為整數類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1432">Explicit numeric conversions ([Explicit numeric conversions](conversions.md#explicit-numeric-conversions)) from one integral type to another integral type, or from `float` or `double` to an integral type.</span></span>

<span data-ttu-id="abcc0-1433">當上述作業的其中一個產生的結果太大，無法代表的目的型別，此作業執行的控制項的內容中產生的行為：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1433">When one of the above operations produce a result that is too large to represent in the destination type, the context in which the operation is performed controls the resulting behavior:</span></span>

*  <span data-ttu-id="abcc0-1434">在 `checked`情況下，如果作業是常數運算式 ([常數運算式](expressions.md#constant-expressions))，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1434">In a `checked` context, if the operation is a constant expression ([Constant expressions](expressions.md#constant-expressions)), a compile-time error occurs.</span></span> <span data-ttu-id="abcc0-1435">否則，當作業在執行階段`System.OverflowException`就會擲回。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1435">Otherwise, when the operation is performed at run-time, a `System.OverflowException` is thrown.</span></span>
*  <span data-ttu-id="abcc0-1436">在 `unchecked`內容中，結果會被截斷並捨棄任何高序位位元不適合目的類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1436">In an `unchecked` context, the result is truncated by discarding any high-order bits that do not fit in the destination type.</span></span>

<span data-ttu-id="abcc0-1437">非常數運算式 （在執行階段評估的運算式），不需加上任何`checked`或是`unchecked`運算子或陳述式，預設值溢位檢查內容是`unchecked`除非外部因素 （例如編譯器參數和執行環境組態） 呼叫`checked`評估。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1437">For non-constant expressions (expressions that are evaluated at run-time) that are not enclosed by any `checked` or `unchecked` operators or statements, the default overflow checking context is `unchecked` unless external factors (such as compiler switches and execution environment configuration) call for `checked` evaluation.</span></span>

<span data-ttu-id="abcc0-1438">對於常數運算式 （可以在編譯時期完整評估的運算式），預設值溢位檢查內容一律是`checked`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1438">For constant expressions (expressions that can be fully evaluated at compile-time), the default overflow checking context is always `checked`.</span></span> <span data-ttu-id="abcc0-1439">除非常數運算式明確放入`unchecked`內容中，運算式一律產生編譯時期評估期間發生溢位會造成編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1439">Unless a constant expression is explicitly placed in an `unchecked` context, overflows that occur during the compile-time evaluation of the expression always cause compile-time errors.</span></span>

<span data-ttu-id="abcc0-1440">匿名函式的主體不會受到`checked`或`unchecked`匿名函式會發生的內容。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1440">The body of an anonymous function is not affected by `checked` or `unchecked` contexts in which the anonymous function occurs.</span></span>

<span data-ttu-id="abcc0-1441">在範例</span><span class="sxs-lookup"><span data-stu-id="abcc0-1441">In the example</span></span>
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
<span data-ttu-id="abcc0-1442">因為沒有一個運算式可在編譯時期評估，則會不報告任何編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1442">no compile-time errors are reported since neither of the expressions can be evaluated at compile-time.</span></span> <span data-ttu-id="abcc0-1443">在執行階段`F`方法會擲回`System.OverflowException`，和`G`方法會傳回-727379968 （較低 32 位元範圍外的結果）。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1443">At run-time, the `F` method throws a `System.OverflowException`, and the `G` method returns -727379968 (the lower 32 bits of the out-of-range result).</span></span> <span data-ttu-id="abcc0-1444">行為`H`方法取決於預設的溢位檢查內容進行編譯，但它是與相同`F`或相同`G`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1444">The behavior of the `H` method depends on the default overflow checking context for the compilation, but it is either the same as `F` or the same as `G`.</span></span>

<span data-ttu-id="abcc0-1445">在範例</span><span class="sxs-lookup"><span data-stu-id="abcc0-1445">In the example</span></span>
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
<span data-ttu-id="abcc0-1446">評估中的常數運算式時，會發生溢位`F`並`H`會導致編譯時期錯誤，因為運算式會評估`checked`內容。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1446">the overflows that occur when evaluating the constant expressions in `F` and `H` cause compile-time errors to be reported because the expressions are evaluated in a `checked` context.</span></span> <span data-ttu-id="abcc0-1447">評估中的常數運算式時，也會發生溢位`G`，但因為評估會發生在`unchecked`內容，則不會報告溢位。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1447">An overflow also occurs when evaluating the constant expression in `G`, but since the evaluation takes place in an `unchecked` context, the overflow is not reported.</span></span>

<span data-ttu-id="abcc0-1448">`checked`並`unchecked`運算子只會影響溢位檢查文字包含在這些作業的內容 」`(`"和"`)`"語彙基元。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1448">The `checked` and `unchecked` operators only affect the overflow checking context for those operations that are textually contained within the "`(`" and "`)`" tokens.</span></span> <span data-ttu-id="abcc0-1449">運算子會有不會影響結果包含的運算式的評估會叫用的函式成員。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1449">The operators have no effect on function members that are invoked as a result of evaluating the contained expression.</span></span> <span data-ttu-id="abcc0-1450">在範例</span><span class="sxs-lookup"><span data-stu-id="abcc0-1450">In the example</span></span>
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
<span data-ttu-id="abcc0-1451">善用`checked`中`F`不會影響評估`x * y`中`Multiply`，因此`x * y`預設溢位檢查內容中評估。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1451">the use of `checked` in `F` does not affect the evaluation of `x * y` in `Multiply`, so `x * y` is evaluated in the default overflow checking context.</span></span>

<span data-ttu-id="abcc0-1452">`unchecked`十六進位標記法撰寫的帶正負號的整數類資料類型的常數時，運算子會很方便。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1452">The `unchecked` operator is convenient when writing constants of the signed integral types in hexadecimal notation.</span></span> <span data-ttu-id="abcc0-1453">例如: </span><span class="sxs-lookup"><span data-stu-id="abcc0-1453">For example:</span></span>
```csharp
class Test
{
    public const int AllBits = unchecked((int)0xFFFFFFFF);

    public const int HighBit = unchecked((int)0x80000000);
}
```

<span data-ttu-id="abcc0-1454">兩個以上十六進位常數的類型是`uint`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1454">Both of the hexadecimal constants above are of type `uint`.</span></span> <span data-ttu-id="abcc0-1455">因為常數之外`int`範圍，不含`unchecked`運算子、 型別轉換成`int`會產生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1455">Because the constants are outside the `int` range, without the `unchecked` operator, the casts to `int` would produce compile-time errors.</span></span>

<span data-ttu-id="abcc0-1456">`checked`和`unchecked`運算子和陳述式可讓程式設計人員控制某些方面的一些數值計算。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1456">The `checked` and `unchecked` operators and statements allow programmers to control certain aspects of some numeric calculations.</span></span> <span data-ttu-id="abcc0-1457">不過，有些數值運算子的行為會取決於其運算元的資料類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1457">However, the behavior of some numeric operators depends on their operands' data types.</span></span> <span data-ttu-id="abcc0-1458">比方說，乘以兩個小數位數永遠會導致溢位時例外狀況甚至內明確`unchecked`建構。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1458">For example, multiplying two decimals always results in an exception on overflow even within an explicitly `unchecked` construct.</span></span> <span data-ttu-id="abcc0-1459">同樣地，乘以兩個浮點數從未結果溢位時例外狀況甚至內明確`checked`建構。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1459">Similarly, multiplying two floats never results in an exception on overflow even within an explicitly `checked` construct.</span></span> <span data-ttu-id="abcc0-1460">此外，其他運算子永遠不會受到所檢查的模式，不管是預設或明確。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1460">In addition, other operators are never affected by the mode of checking, whether default or explicit.</span></span>

### <a name="default-value-expressions"></a><span data-ttu-id="abcc0-1461">預設值運算式</span><span class="sxs-lookup"><span data-stu-id="abcc0-1461">Default value expressions</span></span>

<span data-ttu-id="abcc0-1462">預設值運算式用來取得預設值 ([預設值](variables.md#default-values)) 的型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1462">A default value expression is used to obtain the default value ([Default values](variables.md#default-values)) of a type.</span></span> <span data-ttu-id="abcc0-1463">通常預設值運算式用於型別參數，因為它可能不知道如果型別參數是實值類型或參考型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1463">Typically a default value expression is used for type parameters, since it may not be known if the type parameter is a value type or a reference type.</span></span> <span data-ttu-id="abcc0-1464">(沒有任何轉換存在從`null`常值的型別參數的型別參數已知為參考型別，除非。)</span><span class="sxs-lookup"><span data-stu-id="abcc0-1464">(No conversion exists from the `null` literal to a type parameter unless the type parameter is known to be a reference type.)</span></span>

```antlr
default_value_expression
    : 'default' '(' type ')'
    ;
```

<span data-ttu-id="abcc0-1465">如果*型別*中*default_value_expression*評估結果是在執行階段參考類型，`null`轉換成該類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1465">If the *type* in a *default_value_expression* evaluates at run-time to a reference type, the result is `null` converted to that type.</span></span> <span data-ttu-id="abcc0-1466">如果*型別*中*default_value_expression*評估結果是在執行階段實值型別的*value_type*的預設值 ([預設建構函式](types.md#default-constructors))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1466">If the *type* in a *default_value_expression* evaluates at run-time to a value type, the result is the *value_type*'s default value ([Default constructors](types.md#default-constructors)).</span></span>

<span data-ttu-id="abcc0-1467">A *default_value_expression*是常數運算式 ([常數運算式](expressions.md#constant-expressions)) 如果類型是參考型別或型別參數就是參考型別 ([型別參數條件約束](classes.md#type-parameter-constraints))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1467">A *default_value_expression* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) if the type is a reference type or a type parameter that is known to be a reference type ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="abcc0-1468">颾魤 ㄛ *default_value_expression*是常數運算式，如果類型是其中一個下列的實值型別： `sbyte`， `byte`， `short`， `ushort`， `int`， `uint`，`long`， `ulong`， `char`， `float`， `double`， `decimal`， `bool`，或任何列舉類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1468">In addition, a *default_value_expression* is a constant expression if the type is one of the following value types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, or any enumeration type.</span></span>


### <a name="nameof-expressions"></a><span data-ttu-id="abcc0-1469">Nameof 運算式</span><span class="sxs-lookup"><span data-stu-id="abcc0-1469">Nameof expressions</span></span>

<span data-ttu-id="abcc0-1470">A *nameof_expression*用來取得做為常數字串程式實體的名稱。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1470">A *nameof_expression* is used to obtain the name of a program entity as a constant string.</span></span>

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

<span data-ttu-id="abcc0-1471">文法而言， *named_entity*運算元一律是運算式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1471">Grammatically speaking, the *named_entity* operand is always an expression.</span></span> <span data-ttu-id="abcc0-1472">因為`nameof`不能是保留的關鍵字，nameof 運算式永遠是在語法上模稜兩可的簡單名稱的引動過程`nameof`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1472">Because `nameof` is not a reserved keyword, a nameof expression is always syntactically ambiguous with an invocation of the simple name `nameof`.</span></span> <span data-ttu-id="abcc0-1473">基於相容性考量，如果名稱查閱 ([簡單名稱](expressions.md#simple-names)) 的名稱`nameof`成功，運算式會被視為*invocation_expression* -不論是否在引動過程合法的。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1473">For compatibility reasons, if a name lookup ([Simple names](expressions.md#simple-names)) of the name `nameof` succeeds, the expression is treated as an *invocation_expression* -- regardless of whether the invocation is legal.</span></span> <span data-ttu-id="abcc0-1474">否則，它會*nameof_expression*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1474">Otherwise it is a *nameof_expression*.</span></span>

<span data-ttu-id="abcc0-1475">意義*named_entity*的*nameof_expression*是它的意義，做為運算式; 也就是其中一個作為*simple_name*、 *base_access*或是*member_access*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1475">The meaning of the *named_entity* of a *nameof_expression* is the meaning of it as an expression; that is, either as a *simple_name*, a *base_access* or a *member_access*.</span></span> <span data-ttu-id="abcc0-1476">不過，其中在查閱中所述[簡單名稱](expressions.md#simple-names)並[成員存取](expressions.md#member-access)導致錯誤，因為在靜態內容中，找不到執行個體成員*nameof_expression*會產生任何這類錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1476">However, where the lookup described in [Simple names](expressions.md#simple-names) and [Member access](expressions.md#member-access) results in an error because an instance member was found in a static context, a *nameof_expression* produces no such error.</span></span>

<span data-ttu-id="abcc0-1477">它是編譯時期錯誤*named_entity*指定為方法群組*type_argument_list*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1477">It is a compile-time error for a *named_entity* designating a method group to have a *type_argument_list*.</span></span> <span data-ttu-id="abcc0-1478">它是編譯時期錯誤，如*named_entity_target*具有類型`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1478">It is a compile time error for a *named_entity_target* to have the type `dynamic`.</span></span>

<span data-ttu-id="abcc0-1479">A *nameof_expression*是常數運算式的型別`string`，而且在執行階段沒有任何作用。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1479">A *nameof_expression* is a constant expression of type `string`, and has no effect at runtime.</span></span> <span data-ttu-id="abcc0-1480">具體而言，其*named_entity*則不會評估，而且會忽略明確設定分析的目的 ([簡單運算式的一般規則](variables.md#general-rules-for-simple-expressions))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1480">Specifically, its *named_entity* is not evaluated, and is ignored for the purposes of definite assignment analysis ([General rules for simple expressions](variables.md#general-rules-for-simple-expressions)).</span></span> <span data-ttu-id="abcc0-1481">其值是最後一個識別項*named_entity*之前選擇性最終*type_argument_list*已轉換的方式如下：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1481">Its value is the last identifier of the *named_entity* before the optional final *type_argument_list*, transformed in the following way:</span></span>

* <span data-ttu-id="abcc0-1482">前置詞"`@`」，如果使用，會移除。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1482">The prefix "`@`", if used, is removed.</span></span>
* <span data-ttu-id="abcc0-1483">每個*unicode_escape_sequence*轉換成其對應的 Unicode 字元。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1483">Each *unicode_escape_sequence* is transformed into its corresponding Unicode character.</span></span>
* <span data-ttu-id="abcc0-1484">任何*formatting_characters*會移除。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1484">Any *formatting_characters* are removed.</span></span>

<span data-ttu-id="abcc0-1485">這些都是相同的轉換中套用[識別碼](lexical-structure.md#identifiers)測試識別項之間的等號比較時。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1485">These are the same transformations applied in [Identifiers](lexical-structure.md#identifiers) when testing equality between identifiers.</span></span>

<span data-ttu-id="abcc0-1486">TODO： 範例</span><span class="sxs-lookup"><span data-stu-id="abcc0-1486">TODO: examples</span></span>

### <a name="anonymous-method-expressions"></a><span data-ttu-id="abcc0-1487">匿名方法運算式</span><span class="sxs-lookup"><span data-stu-id="abcc0-1487">Anonymous method expressions</span></span>

<span data-ttu-id="abcc0-1488">*Anonymous_method_expression*是下列其中一種定義匿名函式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1488">An *anonymous_method_expression* is one of two ways of defining an anonymous function.</span></span> <span data-ttu-id="abcc0-1489">進一步說明[匿名函式運算式](expressions.md#anonymous-function-expressions)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1489">These are further described in [Anonymous function expressions](expressions.md#anonymous-function-expressions).</span></span>

## <a name="unary-operators"></a><span data-ttu-id="abcc0-1490">一元運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-1490">Unary operators</span></span>

<span data-ttu-id="abcc0-1491">`?`， `+`， `-`， `!`， `~`， `++`， `--`、 轉型和`await`運算子都稱為一元 （unary） 運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1491">The `?`, `+`, `-`, `!`, `~`, `++`, `--`, cast, and `await` operators are called the unary operators.</span></span>

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

<span data-ttu-id="abcc0-1492">如果運算元*unary_expression*已在編譯時期型別`dynamic`，動態地繫結 ([動態繫結](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1492">If the operand of a *unary_expression* has the compile-time type `dynamic`, it is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="abcc0-1493">在此情況下產生編譯時期類型*unary_expression*是`dynamic`，並如下所述的解析度會發生在執行階段使用的執行階段類型的運算元。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1493">In this case the compile-time type of the *unary_expression* is `dynamic`, and the resolution described below will take place at run-time using the run-time type of the operand.</span></span>

### <a name="null-conditional-operator"></a><span data-ttu-id="abcc0-1494">Null 條件運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-1494">Null-conditional operator</span></span>

<span data-ttu-id="abcc0-1495">Null 條件運算子只適用於一份作業其運算元，運算元非 null。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1495">The null-conditional operator applies a list of operations to its operand only if that operand is non-null.</span></span> <span data-ttu-id="abcc0-1496">否則套用運算子的結果就是`null`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1496">Otherwise the result of applying the operator is `null`.</span></span>

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

<span data-ttu-id="abcc0-1497">成員存取和項目存取作業 （其本身可能是 null 條件），以及引動過程，可以包含的作業清單。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1497">The list of operations can include member access and element access operations (which may themselves be null-conditional), as well as invocation.</span></span>

<span data-ttu-id="abcc0-1498">例如，運算式`a.b?[0]?.c()`是*null_conditional_expression*具有*primary_expression* `a.b`並*null_conditional_operations* `?[0]` （null 條件項目存取）， `?.c` （null 條件成員存取） 和`()`（引動過程）。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1498">For example, the expression `a.b?[0]?.c()` is a *null_conditional_expression* with a *primary_expression* `a.b` and *null_conditional_operations* `?[0]` (null-conditional element access), `?.c` (null-conditional member access) and `()` (invocation).</span></span>

<span data-ttu-id="abcc0-1499">針對*null_conditional_expression* `E`使用*primary_expression* `P`讓`E0`是由賦予移除前置運算式`?`從每個*null_conditional_operations*的`E`，有一個。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1499">For a *null_conditional_expression* `E` with a *primary_expression* `P`, let `E0` be the expression obtained by textually removing the leading `?` from each of the *null_conditional_operations* of `E` that have one.</span></span> <span data-ttu-id="abcc0-1500">就概念而言，`E0`之運算式。 如果沒有任何 null 檢查以表示將會評估`?`發現 s `null`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1500">Conceptually, `E0` is the expression that will be evaluated if none of the null checks represented by the `?`s do find a `null`.</span></span>

<span data-ttu-id="abcc0-1501">此外，可讓`E1`是由賦予移除前置運算式`?`只從的第一個*null_conditional_operations*在`E`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1501">Also, let `E1` be the expression obtained by textually removing the leading `?` from just the first of the *null_conditional_operations* in `E`.</span></span> <span data-ttu-id="abcc0-1502">這可能會導致*主要運算式*(如果有的話就`?`) 或另一個*null_conditional_expression*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1502">This may lead to a *primary-expression* (if there was just one `?`) or to another *null_conditional_expression*.</span></span>

<span data-ttu-id="abcc0-1503">比方說，如果`E`是運算式`a.b?[0]?.c()`，然後`E0`運算式`a.b[0].c()`並`E1`運算式`a.b[0]?.c()`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1503">For example, if `E` is the expression `a.b?[0]?.c()`, then `E0` is the expression `a.b[0].c()` and `E1` is the expression `a.b[0]?.c()`.</span></span>

<span data-ttu-id="abcc0-1504">如果`E0`歸類為任何內容，然後`E`歸類為執行任何動作。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1504">If `E0` is classified as nothing, then `E` is classified as nothing.</span></span> <span data-ttu-id="abcc0-1505">否則將 E 分類為值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1505">Otherwise E is classified as a value.</span></span>

<span data-ttu-id="abcc0-1506">`E0` 並`E1`用來判斷的意義`E`:</span><span class="sxs-lookup"><span data-stu-id="abcc0-1506">`E0` and `E1` are used to determine the meaning of `E`:</span></span>

*  <span data-ttu-id="abcc0-1507">如果`E`當做*statement_expression*的意義`E`等同於陳述式</span><span class="sxs-lookup"><span data-stu-id="abcc0-1507">If `E` occurs as a *statement_expression* the meaning of `E` is the same as the statement</span></span>

   ```csharp
   if ((object)P != null) E1;
   ```

   <span data-ttu-id="abcc0-1508">不同之處在於 P 只評估一次。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1508">except that P is evaluated only once.</span></span>

*  <span data-ttu-id="abcc0-1509">否則，如果`E0`歸類為執行任何動作就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1509">Otherwise, if `E0` is classified as nothing a compile-time error occurs.</span></span>

*  <span data-ttu-id="abcc0-1510">否則，讓`T0`的型別`E0`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1510">Otherwise, let `T0` be the type of `E0`.</span></span>

   *  <span data-ttu-id="abcc0-1511">如果`T0`是型別參數就不是是參考型別或不可為 null 的實值型別，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1511">If `T0` is a type parameter that is not known to be a reference type or a non-nullable value type, a compile-time error occurs.</span></span>

   *  <span data-ttu-id="abcc0-1512">如果`T0`不可為 null 的實值型別，則該類`E`是`T0?`，和意義`E`相同</span><span class="sxs-lookup"><span data-stu-id="abcc0-1512">If `T0` is a non-nullable value type, then the type of `E` is `T0?`, and the meaning of `E` is the same as</span></span>

      ```csharp
      ((object)P == null) ? (T0?)null : E1
      ```

      <span data-ttu-id="abcc0-1513">不同之處在於`P`只評估一次。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1513">except that `P` is evaluated only once.</span></span>

   *  <span data-ttu-id="abcc0-1514">否則 E 的型別 T0，而 E 的意義相同</span><span class="sxs-lookup"><span data-stu-id="abcc0-1514">Otherwise the type of E is T0, and the meaning of E is the same as</span></span>

      ```csharp
      ((object)P == null) ? null : E1
      ```

      <span data-ttu-id="abcc0-1515">不同之處在於`P`只評估一次。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1515">except that `P` is evaluated only once.</span></span>

<span data-ttu-id="abcc0-1516">如果`E1`本身*null_conditional_expression*，然後這些規則會套用同樣地，巢狀的測試`null`直到沒有其他進一步`?`的並一路向下精簡之後所得的運算式主要運算式`E0`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1516">If `E1` is itself a *null_conditional_expression*, then these rules are applied again, nesting the tests for `null` until there are no further `?`'s, and the expression has been reduced all the way down to the primary-expression `E0`.</span></span>

<span data-ttu-id="abcc0-1517">例如，如果運算式`a.b?[0]?.c()`當做陳述式運算式，如陳述式所示：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1517">For example, if the expression `a.b?[0]?.c()` occurs as a statement-expression, as in the statement:</span></span>
```csharp
a.b?[0]?.c();
```
<span data-ttu-id="abcc0-1518">其意義等同於：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1518">its meaning is equivalent to:</span></span>
```csharp
if (a.b != null) a.b[0]?.c();
```
<span data-ttu-id="abcc0-1519">這一次是相當於項目：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1519">which again is equivalent to:</span></span>
```csharp
if (a.b != null) if (a.b[0] != null) a.b[0].c();
```
<span data-ttu-id="abcc0-1520">不同之處在於`a.b`和`a.b[0]`只評估一次。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1520">Except that `a.b` and `a.b[0]` are evaluated only once.</span></span>

<span data-ttu-id="abcc0-1521">如果發生在其中使用其值，如下所示的內容：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1521">If it occurs in a context where its value is used, as in:</span></span>
```csharp
var x = a.b?[0]?.c();
```
<span data-ttu-id="abcc0-1522">而且假設的最後一個引動過程類型不是不可為 null 的實值型別，其意義相當於：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1522">and assuming that the type of the final invocation is not a non-nullable value type, its meaning is equivalent to:</span></span>
```csharp
var x = (a.b == null) ? null : (a.b[0] == null) ? null : a.b[0].c();
```
<span data-ttu-id="abcc0-1523">不同之處在於`a.b`和`a.b[0]`只評估一次。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1523">except that `a.b` and `a.b[0]` are evaluated only once.</span></span>

#### <a name="null-conditional-expressions-as-projection-initializers"></a><span data-ttu-id="abcc0-1524">Null 條件運算式為投影初始設定式</span><span class="sxs-lookup"><span data-stu-id="abcc0-1524">Null-conditional expressions as projection initializers</span></span>

<span data-ttu-id="abcc0-1525">Null 條件運算式只允許作為*member_declarator*中*anonymous_object_creation_expression* ([匿名物件建立運算式](expressions.md#anonymous-object-creation-expressions)) 如果它的結尾 （選擇性地 null-條件） 成員存取。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1525">A null-conditional expression is only allowed as a *member_declarator* in an *anonymous_object_creation_expression* ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) if it ends with an (optionally null-conditional) member access.</span></span> <span data-ttu-id="abcc0-1526">文法，這項需求可以表示為：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1526">Grammatically, this requirement can be expressed as:</span></span>

```antlr
null_conditional_member_access
    : primary_expression null_conditional_operations? '?' '.' identifier type_argument_list?
    | primary_expression null_conditional_operations '.' identifier type_argument_list?
    ;
```

<span data-ttu-id="abcc0-1527">這是特殊形式的文法*null_conditional_expression*上方。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1527">This is a special case of the grammar for *null_conditional_expression* above.</span></span> <span data-ttu-id="abcc0-1528">針對生產*member_declarator*中[匿名物件建立運算式](expressions.md#anonymous-object-creation-expressions)然後僅包括*null_conditional_member_access*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1528">The production for *member_declarator* in [Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions) then includes only *null_conditional_member_access*.</span></span>

#### <a name="null-conditional-expressions-as-statement-expressions"></a><span data-ttu-id="abcc0-1529">Null 條件運算式，做為陳述式運算式</span><span class="sxs-lookup"><span data-stu-id="abcc0-1529">Null-conditional expressions as statement expressions</span></span>

<span data-ttu-id="abcc0-1530">Null 條件運算式只允許作為*statement_expression* ([運算式陳述式](statements.md#expression-statements)) 如果到最後的引動過程。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1530">A null-conditional expression is only allowed as a *statement_expression* ([Expression statements](statements.md#expression-statements)) if it ends with an invocation.</span></span> <span data-ttu-id="abcc0-1531">文法，這項需求可以表示為：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1531">Grammatically, this requirement can be expressed as:</span></span>

```antlr
null_conditional_invocation_expression
    : primary_expression null_conditional_operations '(' argument_list? ')'
    ;
```

<span data-ttu-id="abcc0-1532">這是特殊形式的文法*null_conditional_expression*上方。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1532">This is a special case of the grammar for *null_conditional_expression* above.</span></span> <span data-ttu-id="abcc0-1533">針對生產*statement_expression*中[運算式陳述式](statements.md#expression-statements)然後僅包括*null_conditional_invocation_expression*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1533">The production for *statement_expression* in [Expression statements](statements.md#expression-statements) then includes only *null_conditional_invocation_expression*.</span></span>


### <a name="unary-plus-operator"></a><span data-ttu-id="abcc0-1534">一元加號運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-1534">Unary plus operator</span></span>

<span data-ttu-id="abcc0-1535">作業的表單`+x`，一元運算子多載解析 ([一元運算子多載解析](expressions.md#unary-operator-overload-resolution)) 套用至選取的特定運算子實作。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1535">For an operation of the form `+x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="abcc0-1536">一個運算元轉換成所選的運算子的參數類型和結果的類型是運算子的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1536">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="abcc0-1537">預先定義的一元加法運算子如下：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1537">The predefined unary plus operators are:</span></span>

```csharp
int operator +(int x);
uint operator +(uint x);
long operator +(long x);
ulong operator +(ulong x);
float operator +(float x);
double operator +(double x);
decimal operator +(decimal x);
```

<span data-ttu-id="abcc0-1538">針對每個這些運算子，結果會是運算元的值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1538">For each of these operators, the result is simply the value of the operand.</span></span>

### <a name="unary-minus-operator"></a><span data-ttu-id="abcc0-1539">一元減號運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-1539">Unary minus operator</span></span>

<span data-ttu-id="abcc0-1540">作業的表單`-x`，一元運算子多載解析 ([一元運算子多載解析](expressions.md#unary-operator-overload-resolution)) 套用至選取的特定運算子實作。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1540">For an operation of the form `-x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="abcc0-1541">一個運算元轉換成所選的運算子的參數類型和結果的類型是運算子的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1541">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="abcc0-1542">預先定義的否定運算子包括︰</span><span class="sxs-lookup"><span data-stu-id="abcc0-1542">The predefined negation operators are:</span></span>

*  <span data-ttu-id="abcc0-1543">整數否定：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1543">Integer negation:</span></span>

   ```csharp
   int operator -(int x);
   long operator -(long x);
   ```

   <span data-ttu-id="abcc0-1544">結果計算減去`x`從零。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1544">The result is computed by subtracting `x` from zero.</span></span> <span data-ttu-id="abcc0-1545">如果值的`x`是最小的運算元類型可代表值 (-2 ^31 個`int`或-2 ^63 `long`)，然後數學否定`x`不是可代表運算元的型別中。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1545">If the value of of `x` is the smallest representable value of the operand type (-2^31 for `int` or -2^63 for `long`), then the mathematical negation of `x` is not representable within the operand type.</span></span> <span data-ttu-id="abcc0-1546">如果這發生在`checked`內容中，`System.OverflowException`就會擲回; 內發生`unchecked`內容中，結果是運算元的值，且不會報告溢位。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1546">If this occurs within a `checked` context, a `System.OverflowException` is thrown; if it occurs within an `unchecked` context, the result is the value of the operand and the overflow is not reported.</span></span>

   <span data-ttu-id="abcc0-1547">如果負運算子的運算元為類型`uint`，它會轉換成類型`long`，且結果類型是`long`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1547">If the operand of the negation operator is of type `uint`, it is converted to type `long`, and the type of the result is `long`.</span></span> <span data-ttu-id="abcc0-1548">例外狀況是允許的規則`int`值介於-2147483648 (-2 ^31) 撰寫為十進位整數常值 ([整數常值](lexical-structure.md#integer-literals))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1548">An exception is the rule that permits the `int` value -2147483648 (-2^31) to be written as a decimal integer literal ([Integer literals](lexical-structure.md#integer-literals)).</span></span>

   <span data-ttu-id="abcc0-1549">如果負運算子的運算元為類型`ulong`，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1549">If the operand of the negation operator is of type `ulong`, a compile-time error occurs.</span></span> <span data-ttu-id="abcc0-1550">例外狀況是允許的規則`long`值-9223372036854775808 (-2 ^63) 撰寫為十進位整數常值 ([整數常值](lexical-structure.md#integer-literals))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1550">An exception is the rule that permits the `long` value -9223372036854775808 (-2^63) to be written as a decimal integer literal ([Integer literals](lexical-structure.md#integer-literals)).</span></span>

*  <span data-ttu-id="abcc0-1551">浮點數的否定：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1551">Floating-point negation:</span></span>

   ```csharp
   float operator -(float x);
   double operator -(double x);
   ```

   <span data-ttu-id="abcc0-1552">結果是值`x`反轉正負號。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1552">The result is the value of `x` with its sign inverted.</span></span> <span data-ttu-id="abcc0-1553">如果`x`為 NaN，結果也是 NaN。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1553">If `x` is NaN, the result is also NaN.</span></span>

*  <span data-ttu-id="abcc0-1554">十進位否定：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1554">Decimal negation:</span></span>

   ```csharp
   decimal operator -(decimal x);
   ```

   <span data-ttu-id="abcc0-1555">結果計算減去`x`從零。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1555">The result is computed by subtracting `x` from zero.</span></span> <span data-ttu-id="abcc0-1556">十進位否定是相當於使用一元減號運算子型別的`System.Decimal`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1556">Decimal negation is equivalent to using the unary minus operator of type `System.Decimal`.</span></span>

### <a name="logical-negation-operator"></a><span data-ttu-id="abcc0-1557">邏輯否定運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-1557">Logical negation operator</span></span>

<span data-ttu-id="abcc0-1558">作業的表單`!x`，一元運算子多載解析 ([一元運算子多載解析](expressions.md#unary-operator-overload-resolution)) 套用至選取的特定運算子實作。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1558">For an operation of the form `!x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="abcc0-1559">一個運算元轉換成所選的運算子的參數類型和結果的類型是運算子的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1559">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="abcc0-1560">只能有一個預先定義的邏輯負運算子存在：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1560">Only one predefined logical negation operator exists:</span></span>
```csharp
bool operator !(bool x);
```

<span data-ttu-id="abcc0-1561">這個運算子會計算運算元的邏輯否定：如果運算元`true`，結果是`false`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1561">This operator computes the logical negation of the operand: If the operand is `true`, the result is `false`.</span></span> <span data-ttu-id="abcc0-1562">如果運算元`false`，結果是`true`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1562">If the operand is `false`, the result is `true`.</span></span>

### <a name="bitwise-complement-operator"></a><span data-ttu-id="abcc0-1563">位元補充運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-1563">Bitwise complement operator</span></span>

<span data-ttu-id="abcc0-1564">作業的表單`~x`，一元運算子多載解析 ([一元運算子多載解析](expressions.md#unary-operator-overload-resolution)) 套用至選取的特定運算子實作。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1564">For an operation of the form `~x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="abcc0-1565">一個運算元轉換成所選的運算子的參數類型和結果的類型是運算子的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1565">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="abcc0-1566">是預先定義的位元補數運算子：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1566">The predefined bitwise complement operators are:</span></span>
```csharp
int operator ~(int x);
uint operator ~(uint x);
long operator ~(long x);
ulong operator ~(ulong x);
```

<span data-ttu-id="abcc0-1567">針對每個這些運算子，作業的結果是位元補數`x`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1567">For each of these operators, the result of the operation is the bitwise complement of `x`.</span></span>

<span data-ttu-id="abcc0-1568">每個列舉類型`E`都會隱含地提供下列的位元補數運算子：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1568">Every enumeration type `E` implicitly provides the following bitwise complement operator:</span></span>

```csharp
E operator ~(E x);
```

<span data-ttu-id="abcc0-1569">評估的結果`~x`，其中`x`是列舉型別的運算式`E`基礎型別`U`，正是評估相同`(E)(~(U)x)`不同之處在於，轉換為`E`是一律會以執行在 if`unchecked`內容 ([checked 與 unchecked 運算子](expressions.md#the-checked-and-unchecked-operators))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1569">The result of evaluating `~x`, where `x` is an expression of an enumeration type `E` with an underlying type `U`, is exactly the same as evaluating `(E)(~(U)x)`, except that the conversion to `E` is always performed as if in an `unchecked` context ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)).</span></span>

### <a name="prefix-increment-and-decrement-operators"></a><span data-ttu-id="abcc0-1570">前置遞增和遞減運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-1570">Prefix increment and decrement operators</span></span>

```antlr
pre_increment_expression
    : '++' unary_expression
    ;

pre_decrement_expression
    : '--' unary_expression
    ;
```

<span data-ttu-id="abcc0-1571">運算元的前置遞增或遞減作業必須是運算式分類為變數、 屬性存取或索引子存取。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1571">The operand of a prefix increment or decrement operation must be an expression classified as a variable, a property access, or an indexer access.</span></span> <span data-ttu-id="abcc0-1572">作業的結果是相同的型別運算元的值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1572">The result of the operation is a value of the same type as the operand.</span></span>

<span data-ttu-id="abcc0-1573">如果前置詞的運算元遞增或遞減運算是屬性或索引子存取、 屬性或索引子必須兩者`get`和`set`存取子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1573">If the operand of a prefix increment or decrement operation is a property or indexer access, the property or indexer must have both a `get` and a `set` accessor.</span></span> <span data-ttu-id="abcc0-1574">如果這不是這樣，繫結階段會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1574">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="abcc0-1575">一元運算子多載解析 ([一元運算子多載解析](expressions.md#unary-operator-overload-resolution)) 套用至選取的特定運算子實作。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1575">Unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="abcc0-1576">預先定義的`++`並`--`運算子有下列類型： `sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char``float`， `double`， `decimal`，以及任何列舉類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1576">Predefined `++` and `--` operators exist for the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, and any enum type.</span></span> <span data-ttu-id="abcc0-1577">預先定義`++`運算子會傳回運算元，而預先定義上加 1，所產生的值`--`運算子會傳回所產生的減去 1 的運算元的值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1577">The predefined `++` operators return the value produced by adding 1 to the operand, and the predefined `--` operators return the value produced by subtracting 1 from the operand.</span></span> <span data-ttu-id="abcc0-1578">在 `checked`情況下，如果這個加法或減法運算的結果超出結果型別的範圍和結果型別是整數類資料類型或列舉型別，`System.OverflowException`就會擲回。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1578">In a `checked` context, if the result of this addition or subtraction is outside the range of the result type and the result type is an integral type or enum type, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="abcc0-1579">執行階段的處理前置遞增或遞減運算的表單`++x`或`--x`包含下列步驟：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1579">The run-time processing of a prefix increment or decrement operation of the form `++x` or `--x` consists of the following steps:</span></span>

*   <span data-ttu-id="abcc0-1580">如果`x`分類為變數：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1580">If `x` is classified as a variable:</span></span>
    * <span data-ttu-id="abcc0-1581">`x` 在評估後產生的變數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1581">`x` is evaluated to produce the variable.</span></span>
    * <span data-ttu-id="abcc0-1582">選取的運算子會叫用的值與`x`作為其引數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1582">The selected operator is invoked with the value of `x` as its argument.</span></span>
    * <span data-ttu-id="abcc0-1583">運算子的傳回值會儲存在評估所指定的位置`x`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1583">The value returned by the operator is stored in the location given by the evaluation of `x`.</span></span>
    * <span data-ttu-id="abcc0-1584">運算子的傳回值就是作業的結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1584">The value returned by the operator becomes the result of the operation.</span></span>
*   <span data-ttu-id="abcc0-1585">如果`x`歸類為屬性或索引子的存取：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1585">If `x` is classified as a property or indexer access:</span></span>
    * <span data-ttu-id="abcc0-1586">執行個體運算式 (如果`x`不是`static`) 和引數清單 (如果`x`是索引子存取) 相關聯`x`會進行評估，而且結果會用於後續`get`和`set`存取子引動過程。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1586">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `get` and `set` accessor invocations.</span></span>
    * <span data-ttu-id="abcc0-1587">`get`存取子`x`叫用。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1587">The `get` accessor of `x` is invoked.</span></span>
    * <span data-ttu-id="abcc0-1588">所傳回的值叫用所選的運算子`get`存取子實作為其引數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1588">The selected operator is invoked with the value returned by the `get` accessor as its argument.</span></span>
    * <span data-ttu-id="abcc0-1589">`set`存取子`x`會使用該運算子作為所傳回的值叫用其`value`引數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1589">The `set` accessor of `x` is invoked with the value returned by the operator as its `value` argument.</span></span>
    * <span data-ttu-id="abcc0-1590">運算子的傳回值就是作業的結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1590">The value returned by the operator becomes the result of the operation.</span></span>

<span data-ttu-id="abcc0-1591">`++`並`--`運算子也支援後置標記法 ([後置遞增和遞減運算子](expressions.md#postfix-increment-and-decrement-operators))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1591">The `++` and `--` operators also support postfix notation ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="abcc0-1592">一般而言，結果`x++`或`x--`的值`x`作業之前，而結果`++x`或`--x`的值`x`作業之後。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1592">Typically, the result of `x++` or `x--` is the value of `x` before the operation, whereas the result of `++x` or `--x` is the value of `x` after the operation.</span></span> <span data-ttu-id="abcc0-1593">在任一情況下，`x`本身具有相同的值在作業之後。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1593">In either case, `x` itself has the same value after the operation.</span></span>

<span data-ttu-id="abcc0-1594">`operator++`或`operator--`實作可以使用來叫用前置詞或後置標記法。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1594">An `operator++` or `operator--` implementation can be invoked using either postfix or prefix notation.</span></span> <span data-ttu-id="abcc0-1595">您不可以有兩種標記法的個別運算子實作。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1595">It is not possible to have separate operator implementations for the two notations.</span></span>

### <a name="cast-expressions"></a><span data-ttu-id="abcc0-1596">Cast 運算式</span><span class="sxs-lookup"><span data-stu-id="abcc0-1596">Cast expressions</span></span>

<span data-ttu-id="abcc0-1597">A *cast_expression*用來明確地將運算式轉換成指定的型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1597">A *cast_expression* is used to explicitly convert an expression to a given type.</span></span>

```antlr
cast_expression
    : '(' type ')' unary_expression
    ;
```

<span data-ttu-id="abcc0-1598">A *cast_expression*的表單`(T)E`，其中`T`是*型別*並`E`是*unary_expression*，執行明確轉換 ([明確轉換](conversions.md#explicit-conversions)) 之值的`E`輸入`T`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1598">A *cast_expression* of the form `(T)E`, where `T` is a *type* and `E` is a *unary_expression*, performs an explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) of the value of `E` to type `T`.</span></span> <span data-ttu-id="abcc0-1599">如果沒有任何明確的轉換存在從`E`至`T`，則繫結階段會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1599">If no explicit conversion exists from `E` to `T`, a binding-time error occurs.</span></span> <span data-ttu-id="abcc0-1600">否則，結果就是明確的轉換所產生的值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1600">Otherwise, the result is the value produced by the explicit conversion.</span></span> <span data-ttu-id="abcc0-1601">結果一律會分類為值時，即使`E`代表變數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1601">The result is always classified as a value, even if `E` denotes a variable.</span></span>

<span data-ttu-id="abcc0-1602">文法*cast_expression*通往特定語法模稜兩可。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1602">The grammar for a *cast_expression* leads to certain syntactic ambiguities.</span></span> <span data-ttu-id="abcc0-1603">例如，運算式`(x)-y`可能是解譯為*cast_expression* (轉型`-y`鍵入`x`) 或*additive_expression*結合*parenthesized_expression* (它會計算值`x - y)`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1603">For example, the expression `(x)-y` could either be interpreted as a *cast_expression* (a cast of `-y` to type `x`) or as an *additive_expression* combined with a *parenthesized_expression* (which computes the value `x - y)`.</span></span>

<span data-ttu-id="abcc0-1604">若要解決*cast_expression*模稜兩可，下列規則存在：一或多個序列*語彙基元*s ([泛空白字元](lexical-structure.md#white-space)) 括在括號會被視為開頭*cast_expression*至少下列其中一項條件成立時，才：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1604">To resolve *cast_expression* ambiguities, the following rule exists: A sequence of one or more *token*s ([White space](lexical-structure.md#white-space)) enclosed in parentheses is considered the start of a *cast_expression* only if at least one of the following are true:</span></span>

*  <span data-ttu-id="abcc0-1605">語彙基元順序是正確文法*型別*，但並不適合*運算式*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1605">The sequence of tokens is correct grammar for a *type*, but not for an *expression*.</span></span>
*  <span data-ttu-id="abcc0-1606">語彙基元順序是正確文法*型別*，並緊接在右括號的語彙基元是語彙基元"`~`"，語彙基元"`!`"，語彙基元"`(`"、 *識別項*([Unicode 字元的逸出序列](lexical-structure.md#unicode-character-escape-sequences))，則*常值*([常值](lexical-structure.md#literals))，或有任何*關鍵字*([關鍵字](lexical-structure.md#keywords)) 除了`as`和`is`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1606">The sequence of tokens is correct grammar for a *type*, and the token immediately following the closing parentheses is the token "`~`", the token "`!`", the token "`(`", an *identifier* ([Unicode character escape sequences](lexical-structure.md#unicode-character-escape-sequences)), a *literal* ([Literals](lexical-structure.md#literals)), or any *keyword* ([Keywords](lexical-structure.md#keywords)) except `as` and `is`.</span></span>

<span data-ttu-id="abcc0-1607">上面提到的 「 正確文法 」 表示只語彙基元順序必須符合特定的文法解析。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1607">The term "correct grammar" above means only that the sequence of tokens must conform to the particular grammatical production.</span></span> <span data-ttu-id="abcc0-1608">特別是不會考慮任何組成識別項的實際意義。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1608">It specifically does not consider the actual meaning of any constituent identifiers.</span></span> <span data-ttu-id="abcc0-1609">例如，如果`x`並`y`是識別項，然後`x.y`是正確的文法，對於類型，即使`x.y`實際上並不代表型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1609">For example, if `x` and `y` are identifiers, then `x.y` is correct grammar for a type, even if `x.y` doesn't actually denote a type.</span></span>

<span data-ttu-id="abcc0-1610">從去除混淆規則，如果`x`並`y`是識別項， `(x)y`， `(x)(y)`，和`(x)(-y)`會*cast_expression*s，但`(x)-y`不是，即使`x`識別的型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1610">From the disambiguation rule it follows that, if `x` and `y` are identifiers, `(x)y`, `(x)(y)`, and `(x)(-y)` are *cast_expression*s, but `(x)-y` is not, even if `x` identifies a type.</span></span> <span data-ttu-id="abcc0-1611">不過，如果`x`是識別預先定義的類型的關鍵字 (例如`int`)，則所有的四種形式*cast_expression*s （因為這類關鍵字本身不可能會是運算式）。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1611">However, if `x` is a keyword that identifies a predefined type (such as `int`), then all four forms are *cast_expression*s (because such a keyword could not possibly be an expression by itself).</span></span>

### <a name="await-expressions"></a><span data-ttu-id="abcc0-1612">Await 運算式</span><span class="sxs-lookup"><span data-stu-id="abcc0-1612">Await expressions</span></span>

<span data-ttu-id="abcc0-1613">Await 運算子用來暫停封入的非同步函式的評估，直到在運算元所表示的非同步作業已完成。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1613">The await operator is used to suspend evaluation of the enclosing async function until the asynchronous operation represented by the operand has completed.</span></span>

```antlr
await_expression
    : 'await' unary_expression
    ;
```

<span data-ttu-id="abcc0-1614">*Await_expression*只能在非同步函式主體中 ([迭代器](classes.md#iterators))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1614">An *await_expression* is only allowed in the body of an async function ([Iterators](classes.md#iterators)).</span></span> <span data-ttu-id="abcc0-1615">中最內層非同步函式， *await_expression*可能不會發生在下列位置：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1615">Within the nearest enclosing async function, an *await_expression* may not occur in these places:</span></span>

*  <span data-ttu-id="abcc0-1616">在巢狀 （非同步） 的匿名函式</span><span class="sxs-lookup"><span data-stu-id="abcc0-1616">Inside a nested (non-async) anonymous function</span></span>
*  <span data-ttu-id="abcc0-1617">區塊內*lock_statement*</span><span class="sxs-lookup"><span data-stu-id="abcc0-1617">Inside the block of a *lock_statement*</span></span>
*  <span data-ttu-id="abcc0-1618">在 unsafe 內容中</span><span class="sxs-lookup"><span data-stu-id="abcc0-1618">In an unsafe context</span></span>

<span data-ttu-id="abcc0-1619">請注意， *await_expression*內的大部分位置中不會發生*query_expression>*，因為這些語法轉換成使用非同步 lambda 運算式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1619">Note that an *await_expression* cannot occur in most places within a *query_expression*, because those are syntactically transformed to use non-async lambda expressions.</span></span>

<span data-ttu-id="abcc0-1620">在非同步函式內`await`不能當做識別項。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1620">Inside of an async function, `await` cannot be used as an identifier.</span></span> <span data-ttu-id="abcc0-1621">因此，await 運算式與包含識別碼的各種運算式之間沒有語法模稜兩可。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1621">There is therefore no syntactic ambiguity between await-expressions and various expressions involving identifiers.</span></span> <span data-ttu-id="abcc0-1622">非同步函式中，外部`await`做為一般識別項。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1622">Outside of async functions, `await` acts as a normal identifier.</span></span>

<span data-ttu-id="abcc0-1623">運算元*await_expression*稱為***工作***。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1623">The operand of an *await_expression* is called the ***task***.</span></span> <span data-ttu-id="abcc0-1624">它代表非同步作業，可能會或可能不完整當時*await_expression*評估。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1624">It represents an asynchronous operation that may or may not be complete at the time the *await_expression* is evaluated.</span></span> <span data-ttu-id="abcc0-1625">Await 運算子的目的是暫止的封入的非同步函式的執行，直到等候的工作完成，並取得其結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1625">The purpose of the await operator is to suspend execution of the enclosing async function until the awaited task is complete, and then obtain its outcome.</span></span>

#### <a name="awaitable-expressions"></a><span data-ttu-id="abcc0-1626">可等候的運算式</span><span class="sxs-lookup"><span data-stu-id="abcc0-1626">Awaitable expressions</span></span>

<span data-ttu-id="abcc0-1627">Await 運算式的工作，一定要***awaitable***。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1627">The task of an await expression is required to be ***awaitable***.</span></span> <span data-ttu-id="abcc0-1628">運算式`t`不必是即時資訊，如果下列其中一種保留：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1628">An expression `t` is awaitable if one of the following holds:</span></span>

*  <span data-ttu-id="abcc0-1629">`t` 編譯時間類型 `dynamic`</span><span class="sxs-lookup"><span data-stu-id="abcc0-1629">`t` is of compile time type `dynamic`</span></span>
*  <span data-ttu-id="abcc0-1630">`t` 具有可存取執行個體或擴充的方法呼叫`GetAwaiter`沒有參數並沒有類型參數，且傳回類型與`A`，下列全部保留：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1630">`t` has an accessible instance or extension method called `GetAwaiter` with no parameters and no type parameters, and a return type `A` for which all of the following hold:</span></span>
   * <span data-ttu-id="abcc0-1631">`A` 實作介面`System.Runtime.CompilerServices.INotifyCompletion`(以下稱為`INotifyCompletion`為求簡單明瞭)</span><span class="sxs-lookup"><span data-stu-id="abcc0-1631">`A` implements the interface `System.Runtime.CompilerServices.INotifyCompletion` (hereafter known as `INotifyCompletion` for brevity)</span></span>
   * <span data-ttu-id="abcc0-1632">`A` 具有可供存取的可讀取的執行個體屬性`IsCompleted`的型別 `bool`</span><span class="sxs-lookup"><span data-stu-id="abcc0-1632">`A` has an accessible, readable instance property `IsCompleted` of type `bool`</span></span>
   * <span data-ttu-id="abcc0-1633">`A` 具有可存取的執行個體的方法`GetResult`沒有任何參數，沒有類型參數</span><span class="sxs-lookup"><span data-stu-id="abcc0-1633">`A` has an accessible instance method `GetResult` with no parameters and no type parameters</span></span>

<span data-ttu-id="abcc0-1634">目的`GetAwaiter`方法，是取得***awaiter***工作。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1634">The purpose of the `GetAwaiter` method is to obtain an ***awaiter*** for the task.</span></span> <span data-ttu-id="abcc0-1635">型別`A`稱為***awaiter 的型別***await 運算式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1635">The type `A` is called the ***awaiter type*** for the await expression.</span></span>

<span data-ttu-id="abcc0-1636">目的`IsCompleted`屬性是要判斷工作是否已經完成。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1636">The purpose of the `IsCompleted` property is to determine if the task is already complete.</span></span> <span data-ttu-id="abcc0-1637">若是如此，不是需要暫停評估。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1637">If so, there is no need to suspend evaluation.</span></span>

<span data-ttu-id="abcc0-1638">目的`INotifyCompletion.OnCompleted`方法是註冊 「 接續 」 工作，也就是委派 (型別的`System.Action`)，將會叫用完成工作之後。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1638">The purpose of the `INotifyCompletion.OnCompleted` method is to sign up a "continuation" to the task; i.e. a delegate (of type `System.Action`) that will be invoked once the task is complete.</span></span>

<span data-ttu-id="abcc0-1639">目的`GetResult`方法是完成後，取得工作的結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1639">The purpose of the `GetResult` method is to obtain the outcome of the task once it is complete.</span></span> <span data-ttu-id="abcc0-1640">此結果可能會成功完成時，可能與結果值，或它可能會擲回的例外狀況`GetResult`方法。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1640">This outcome may be successful completion, possibly with a result value, or it may be an exception which is thrown by the `GetResult` method.</span></span>

#### <a name="classification-of-await-expressions"></a><span data-ttu-id="abcc0-1641">分類的 await 運算式</span><span class="sxs-lookup"><span data-stu-id="abcc0-1641">Classification of await expressions</span></span>

<span data-ttu-id="abcc0-1642">運算式`await t`運算式的相同方式來分類`(t).GetAwaiter().GetResult()`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1642">The expression `await t` is classified the same way as the expression `(t).GetAwaiter().GetResult()`.</span></span> <span data-ttu-id="abcc0-1643">因此，如果傳回類型`GetResult`是`void`，則*await_expression*歸類為執行任何動作。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1643">Thus, if the return type of `GetResult` is `void`, the *await_expression* is classified as nothing.</span></span> <span data-ttu-id="abcc0-1644">如果它具有非 void 傳回型別`T`，則*await_expression*分類類型的值為`T`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1644">If it has a non-void return type `T`, the *await_expression* is classified as a value of type `T`.</span></span>

#### <a name="runtime-evaluation-of-await-expressions"></a><span data-ttu-id="abcc0-1645">執行階段評估的 await 運算式</span><span class="sxs-lookup"><span data-stu-id="abcc0-1645">Runtime evaluation of await expressions</span></span>

<span data-ttu-id="abcc0-1646">在執行階段，運算式`await t`評估如下：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1646">At runtime, the expression `await t` is evaluated as follows:</span></span>

*  <span data-ttu-id="abcc0-1647">Awaiter`a`來評估運算式取得`(t).GetAwaiter()`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1647">An awaiter `a` is obtained by evaluating the expression `(t).GetAwaiter()`.</span></span>
*  <span data-ttu-id="abcc0-1648">A `bool` `b`所評估的運算式取得`(a).IsCompleted`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1648">A `bool` `b` is obtained by evaluating the expression `(a).IsCompleted`.</span></span>
*  <span data-ttu-id="abcc0-1649">如果`b`已`false`然後評估取決於是否`a`實作介面`System.Runtime.CompilerServices.ICriticalNotifyCompletion`(以下稱為`ICriticalNotifyCompletion`為求簡單明瞭)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1649">If `b` is `false` then evaluation depends on whether `a` implements the interface `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (hereafter known as `ICriticalNotifyCompletion` for brevity).</span></span> <span data-ttu-id="abcc0-1650">這項檢查是在繫結時間;也就是在執行階段若`a`編譯時間類型`dynamic`，而是在編譯階段則。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1650">This check is done at binding time; i.e. at runtime if `a` has the compile time type `dynamic`, and at compile time otherwise.</span></span> <span data-ttu-id="abcc0-1651">可讓`r`表示繼續委派 ([迭代器](classes.md#iterators)):</span><span class="sxs-lookup"><span data-stu-id="abcc0-1651">Let `r` denote the resumption delegate ([Iterators](classes.md#iterators)):</span></span>
    * <span data-ttu-id="abcc0-1652">如果`a`不會實作`ICriticalNotifyCompletion`，然後運算式`(a as (INotifyCompletion)).OnCompleted(r)`評估。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1652">If `a` does not implement `ICriticalNotifyCompletion`, then the expression `(a as (INotifyCompletion)).OnCompleted(r)` is evaluated.</span></span>
    * <span data-ttu-id="abcc0-1653">如果`a`未實作`ICriticalNotifyCompletion`，然後運算式`(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)`評估。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1653">If `a` does implement `ICriticalNotifyCompletion`, then the expression `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` is evaluated.</span></span>
    * <span data-ttu-id="abcc0-1654">評估已然後暫停，而控制權給目前的呼叫端的非同步函式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1654">Evaluation is then suspended, and control is returned to the current caller of the async function.</span></span>
*  <span data-ttu-id="abcc0-1655">其中一個後立即 (如果`b`已`true`)，或是在稍後繼續委派引動過程 (如果`b`已`false`)，運算式`(a).GetResult()`評估。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1655">Either immediately after (if `b` was `true`), or upon later invocation of the resumption delegate (if `b` was `false`), the expression `(a).GetResult()` is evaluated.</span></span> <span data-ttu-id="abcc0-1656">如果傳回值，該值是結果*await_expression*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1656">If it returns a value, that value is the result of the *await_expression*.</span></span> <span data-ttu-id="abcc0-1657">否則結果就是執行任何動作。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1657">Otherwise the result is nothing.</span></span>

<span data-ttu-id="abcc0-1658">實作介面方法的 awaiter`INotifyCompletion.OnCompleted`和`ICriticalNotifyCompletion.UnsafeOnCompleted`應該會造成委派`r`最多一次叫用。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1658">An awaiter's implementation of the interface methods `INotifyCompletion.OnCompleted` and `ICriticalNotifyCompletion.UnsafeOnCompleted` should cause the delegate `r` to be invoked at most once.</span></span> <span data-ttu-id="abcc0-1659">否則，封入的非同步函式的行為未定義。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1659">Otherwise, the behavior of the enclosing async function is undefined.</span></span>

## <a name="arithmetic-operators"></a><span data-ttu-id="abcc0-1660">算術運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-1660">Arithmetic operators</span></span>

<span data-ttu-id="abcc0-1661">`*`， `/`， `%`， `+`，和`-`運算子都稱為算術的運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1661">The `*`, `/`, `%`, `+`, and `-` operators are called the arithmetic operators.</span></span>

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

<span data-ttu-id="abcc0-1662">算術運算子的運算元是否在編譯時期型別`dynamic`，然後動態地繫結運算式 ([動態繫結](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1662">If an operand of an arithmetic operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="abcc0-1663">運算式的編譯時期型別是在此情況下`dynamic`，如下所述的解析度會發生在執行階段使用這些已編譯時間類型的運算元的執行階段型別和`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1663">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

### <a name="multiplication-operator"></a><span data-ttu-id="abcc0-1664">乘法運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-1664">Multiplication operator</span></span>

<span data-ttu-id="abcc0-1665">作業的表單`x * y`，二元運算子多載解析 ([二元運算子多載解析](expressions.md#binary-operator-overload-resolution)) 套用至選取的特定運算子實作。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1665">For an operation of the form `x * y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="abcc0-1666">運算元會轉換成所選的運算子的參數類型和結果的類型是運算子的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1666">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="abcc0-1667">以下列出預先定義的乘法運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1667">The predefined multiplication operators are listed below.</span></span> <span data-ttu-id="abcc0-1668">這些運算子都會計算乘積`x`和`y`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1668">The operators all compute the product of `x` and `y`.</span></span>

*  <span data-ttu-id="abcc0-1669">整數乘法：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1669">Integer multiplication:</span></span>

   ```csharp
   int operator *(int x, int y);
   uint operator *(uint x, uint y);
   long operator *(long x, long y);
   ulong operator *(ulong x, ulong y);
   ```

   <span data-ttu-id="abcc0-1670">在 `checked`情況下，如果產品是範圍之外的結果型別`System.OverflowException`就會擲回。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1670">In a `checked` context, if the product is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="abcc0-1671">在 `unchecked`內容中，不會報告溢位，並捨棄任何重大高序位位元的範圍之外的結果型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1671">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>


*  <span data-ttu-id="abcc0-1672">浮點數的乘法：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1672">Floating-point multiplication:</span></span>

   ```csharp
   float operator *(float x, float y);
   double operator *(double x, double y);
   ```

   <span data-ttu-id="abcc0-1673">根據 IEEE 754 算術的規則會計算乘積。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1673">The product is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="abcc0-1674">下表列出的非零的有限值的所有可能組合、 零、 無限及 NaN 的結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1674">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="abcc0-1675">在資料表中，`x`和`y`是有限的正值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1675">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="abcc0-1676">`z` 結果的`x * y`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1676">`z` is the result of `x * y`.</span></span> <span data-ttu-id="abcc0-1677">如果結果太大的目的型別，如`z`是無限大。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1677">If the result is too large for the destination type, `z` is infinity.</span></span> <span data-ttu-id="abcc0-1678">如果結果太小的目的型別，`z`為零。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1678">If the result is too small for the destination type, `z` is zero.</span></span>

   |      |      |      |     |     |      |      |     |
   |:----:|-----:|:----:|:---:|:---:|:----:|:----:|:----|
   |      | <span data-ttu-id="abcc0-1679">+y</span><span class="sxs-lookup"><span data-stu-id="abcc0-1679">+y</span></span>   | <span data-ttu-id="abcc0-1680">-y</span><span class="sxs-lookup"><span data-stu-id="abcc0-1680">-y</span></span>   | <span data-ttu-id="abcc0-1681">+0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1681">+0</span></span>  | <span data-ttu-id="abcc0-1682">-0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1682">-0</span></span>  | <span data-ttu-id="abcc0-1683">+inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1683">+inf</span></span> | <span data-ttu-id="abcc0-1684">-inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1684">-inf</span></span> | <span data-ttu-id="abcc0-1685">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1685">NaN</span></span> | 
   | <span data-ttu-id="abcc0-1686">+x</span><span class="sxs-lookup"><span data-stu-id="abcc0-1686">+x</span></span>   | <span data-ttu-id="abcc0-1687">+z</span><span class="sxs-lookup"><span data-stu-id="abcc0-1687">+z</span></span>   | <span data-ttu-id="abcc0-1688">-z</span><span class="sxs-lookup"><span data-stu-id="abcc0-1688">-z</span></span>   | <span data-ttu-id="abcc0-1689">+0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1689">+0</span></span>  | <span data-ttu-id="abcc0-1690">-0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1690">-0</span></span>  | <span data-ttu-id="abcc0-1691">+inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1691">+inf</span></span> | <span data-ttu-id="abcc0-1692">-inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1692">-inf</span></span> | <span data-ttu-id="abcc0-1693">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1693">NaN</span></span> | 
   | <span data-ttu-id="abcc0-1694">-x</span><span class="sxs-lookup"><span data-stu-id="abcc0-1694">-x</span></span>   | <span data-ttu-id="abcc0-1695">-z</span><span class="sxs-lookup"><span data-stu-id="abcc0-1695">-z</span></span>   | <span data-ttu-id="abcc0-1696">+z</span><span class="sxs-lookup"><span data-stu-id="abcc0-1696">+z</span></span>   | <span data-ttu-id="abcc0-1697">-0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1697">-0</span></span>  | <span data-ttu-id="abcc0-1698">+0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1698">+0</span></span>  | <span data-ttu-id="abcc0-1699">-inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1699">-inf</span></span> | <span data-ttu-id="abcc0-1700">+inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1700">+inf</span></span> | <span data-ttu-id="abcc0-1701">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1701">NaN</span></span> | 
   | <span data-ttu-id="abcc0-1702">+0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1702">+0</span></span>   | <span data-ttu-id="abcc0-1703">+0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1703">+0</span></span>   | <span data-ttu-id="abcc0-1704">-0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1704">-0</span></span>   | <span data-ttu-id="abcc0-1705">+0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1705">+0</span></span>  | <span data-ttu-id="abcc0-1706">-0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1706">-0</span></span>  | <span data-ttu-id="abcc0-1707">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1707">NaN</span></span>  | <span data-ttu-id="abcc0-1708">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1708">NaN</span></span>  | <span data-ttu-id="abcc0-1709">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1709">NaN</span></span> | 
   | <span data-ttu-id="abcc0-1710">-0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1710">-0</span></span>   | <span data-ttu-id="abcc0-1711">-0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1711">-0</span></span>   | <span data-ttu-id="abcc0-1712">+0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1712">+0</span></span>   | <span data-ttu-id="abcc0-1713">-0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1713">-0</span></span>  | <span data-ttu-id="abcc0-1714">+0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1714">+0</span></span>  | <span data-ttu-id="abcc0-1715">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1715">NaN</span></span>  | <span data-ttu-id="abcc0-1716">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1716">NaN</span></span>  | <span data-ttu-id="abcc0-1717">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1717">NaN</span></span> | 
   | <span data-ttu-id="abcc0-1718">+inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1718">+inf</span></span> | <span data-ttu-id="abcc0-1719">+inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1719">+inf</span></span> | <span data-ttu-id="abcc0-1720">-inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1720">-inf</span></span> | <span data-ttu-id="abcc0-1721">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1721">NaN</span></span> | <span data-ttu-id="abcc0-1722">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1722">NaN</span></span> | <span data-ttu-id="abcc0-1723">+inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1723">+inf</span></span> | <span data-ttu-id="abcc0-1724">-inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1724">-inf</span></span> | <span data-ttu-id="abcc0-1725">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1725">NaN</span></span> | 
   | <span data-ttu-id="abcc0-1726">-inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1726">-inf</span></span> | <span data-ttu-id="abcc0-1727">-inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1727">-inf</span></span> | <span data-ttu-id="abcc0-1728">+inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1728">+inf</span></span> | <span data-ttu-id="abcc0-1729">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1729">NaN</span></span> | <span data-ttu-id="abcc0-1730">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1730">NaN</span></span> | <span data-ttu-id="abcc0-1731">-inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1731">-inf</span></span> | <span data-ttu-id="abcc0-1732">+inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1732">+inf</span></span> | <span data-ttu-id="abcc0-1733">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1733">NaN</span></span> | 
   | <span data-ttu-id="abcc0-1734">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1734">NaN</span></span>  | <span data-ttu-id="abcc0-1735">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1735">NaN</span></span>  | <span data-ttu-id="abcc0-1736">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1736">NaN</span></span>  | <span data-ttu-id="abcc0-1737">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1737">NaN</span></span> | <span data-ttu-id="abcc0-1738">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1738">NaN</span></span> | <span data-ttu-id="abcc0-1739">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1739">NaN</span></span>  | <span data-ttu-id="abcc0-1740">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1740">NaN</span></span>  | <span data-ttu-id="abcc0-1741">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1741">NaN</span></span> | 

*  <span data-ttu-id="abcc0-1742">十進位乘法：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1742">Decimal multiplication:</span></span>

   ```csharp
   decimal operator *(decimal x, decimal y);
   ```

   <span data-ttu-id="abcc0-1743">產生的值是否太大，無法在代表`decimal`格式，`System.OverflowException`就會擲回。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1743">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="abcc0-1744">如果結果值太小而無法在代表`decimal`格式時，結果會是零。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1744">If the result value is too small to represent in the `decimal` format, the result is zero.</span></span> <span data-ttu-id="abcc0-1745">結果，在任何進位之前的小數位數是個兩個運算元的總和。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1745">The scale of the result, before any rounding, is the sum of the scales of the two operands.</span></span>

   <span data-ttu-id="abcc0-1746">十進位乘法相當於使用乘法運算子型別的`System.Decimal`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1746">Decimal multiplication is equivalent to using the multiplication operator of type `System.Decimal`.</span></span>


### <a name="division-operator"></a><span data-ttu-id="abcc0-1747">除法運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-1747">Division operator</span></span>

<span data-ttu-id="abcc0-1748">作業的表單`x / y`，二元運算子多載解析 ([二元運算子多載解析](expressions.md#binary-operator-overload-resolution)) 套用至選取的特定運算子實作。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1748">For an operation of the form `x / y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="abcc0-1749">運算元會轉換成所選的運算子的參數類型和結果的類型是運算子的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1749">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="abcc0-1750">以下列出預先定義的除法運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1750">The predefined division operators are listed below.</span></span> <span data-ttu-id="abcc0-1751">這些運算子都會計算商數`x`和`y`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1751">The operators all compute the quotient of `x` and `y`.</span></span>

*  <span data-ttu-id="abcc0-1752">整數除法運算：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1752">Integer division:</span></span>

   ```csharp
   int operator /(int x, int y);
   uint operator /(uint x, uint y);
   long operator /(long x, long y);
   ulong operator /(ulong x, ulong y);
   ```

   <span data-ttu-id="abcc0-1753">如果右運算元的值為零，`System.DivideByZeroException`就會擲回。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1753">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span>

   <span data-ttu-id="abcc0-1754">除法運算會無條件推向零結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1754">The division rounds the result towards zero.</span></span> <span data-ttu-id="abcc0-1755">因此結果的絕對值是商數的小於或等於兩個運算元絕對值的最大可能整數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1755">Thus the absolute value of the result is the largest possible integer that is less than or equal to the absolute value of the quotient of the two operands.</span></span> <span data-ttu-id="abcc0-1756">當兩個運算元具有相同的正負號和零或負的兩個運算元是相反的符號時，結果會是零或正數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1756">The result is zero or positive when the two operands have the same sign and zero or negative when the two operands have opposite signs.</span></span>

   <span data-ttu-id="abcc0-1757">如果左的運算元是可代表最小`int`或是`long`值和右運算元是`-1`，就會發生溢位。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1757">If the left operand is the smallest representable `int` or `long` value and the right operand is `-1`, an overflow occurs.</span></span> <span data-ttu-id="abcc0-1758">在 `checked`內容，這會導致`System.ArithmeticException`（或子類別） 將會擲回。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1758">In a `checked` context, this causes a `System.ArithmeticException` (or a subclass thereof) to be thrown.</span></span> <span data-ttu-id="abcc0-1759">在`unchecked`內容中，它是由實作定義是否`System.ArithmeticException`（或子類別） 會擲回或產生的值為，左邊運算元的附帶未回報的溢位。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1759">In an `unchecked` context, it is implementation-defined as to whether a `System.ArithmeticException` (or a subclass thereof) is thrown or the overflow goes unreported with the resulting value being that of the left operand.</span></span>

*  <span data-ttu-id="abcc0-1760">浮點的除數：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1760">Floating-point division:</span></span>

   ```csharp
   float operator /(float x, float y);
   double operator /(double x, double y);
   ```

   <span data-ttu-id="abcc0-1761">根據 IEEE 754 算術的規則計算商數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1761">The quotient is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="abcc0-1762">下表列出的非零的有限值的所有可能組合、 零、 無限及 NaN 的結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1762">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="abcc0-1763">在資料表中，`x`和`y`是有限的正值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1763">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="abcc0-1764">`z` 結果的`x / y`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1764">`z` is the result of `x / y`.</span></span> <span data-ttu-id="abcc0-1765">如果結果太大的目的型別，如`z`是無限大。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1765">If the result is too large for the destination type, `z` is infinity.</span></span> <span data-ttu-id="abcc0-1766">如果結果太小的目的型別，`z`為零。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1766">If the result is too small for the destination type, `z` is zero.</span></span>

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="abcc0-1767">+y</span><span class="sxs-lookup"><span data-stu-id="abcc0-1767">+y</span></span>   | <span data-ttu-id="abcc0-1768">-y</span><span class="sxs-lookup"><span data-stu-id="abcc0-1768">-y</span></span>   | <span data-ttu-id="abcc0-1769">+0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1769">+0</span></span>   | <span data-ttu-id="abcc0-1770">-0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1770">-0</span></span>   | <span data-ttu-id="abcc0-1771">+inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1771">+inf</span></span> | <span data-ttu-id="abcc0-1772">-inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1772">-inf</span></span> | <span data-ttu-id="abcc0-1773">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1773">NaN</span></span>  | 
   | <span data-ttu-id="abcc0-1774">+x</span><span class="sxs-lookup"><span data-stu-id="abcc0-1774">+x</span></span>   | <span data-ttu-id="abcc0-1775">+z</span><span class="sxs-lookup"><span data-stu-id="abcc0-1775">+z</span></span>   | <span data-ttu-id="abcc0-1776">-z</span><span class="sxs-lookup"><span data-stu-id="abcc0-1776">-z</span></span>   | <span data-ttu-id="abcc0-1777">+inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1777">+inf</span></span> | <span data-ttu-id="abcc0-1778">-inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1778">-inf</span></span> | <span data-ttu-id="abcc0-1779">+0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1779">+0</span></span>   | <span data-ttu-id="abcc0-1780">-0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1780">-0</span></span>   | <span data-ttu-id="abcc0-1781">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1781">NaN</span></span>  | 
   | <span data-ttu-id="abcc0-1782">-x</span><span class="sxs-lookup"><span data-stu-id="abcc0-1782">-x</span></span>   | <span data-ttu-id="abcc0-1783">-z</span><span class="sxs-lookup"><span data-stu-id="abcc0-1783">-z</span></span>   | <span data-ttu-id="abcc0-1784">+z</span><span class="sxs-lookup"><span data-stu-id="abcc0-1784">+z</span></span>   | <span data-ttu-id="abcc0-1785">-inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1785">-inf</span></span> | <span data-ttu-id="abcc0-1786">+inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1786">+inf</span></span> | <span data-ttu-id="abcc0-1787">-0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1787">-0</span></span>   | <span data-ttu-id="abcc0-1788">+0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1788">+0</span></span>   | <span data-ttu-id="abcc0-1789">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1789">NaN</span></span>  | 
   | <span data-ttu-id="abcc0-1790">+0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1790">+0</span></span>   | <span data-ttu-id="abcc0-1791">+0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1791">+0</span></span>   | <span data-ttu-id="abcc0-1792">-0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1792">-0</span></span>   | <span data-ttu-id="abcc0-1793">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1793">NaN</span></span>  | <span data-ttu-id="abcc0-1794">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1794">NaN</span></span>  | <span data-ttu-id="abcc0-1795">+0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1795">+0</span></span>   | <span data-ttu-id="abcc0-1796">-0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1796">-0</span></span>   | <span data-ttu-id="abcc0-1797">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1797">NaN</span></span>  | 
   | <span data-ttu-id="abcc0-1798">-0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1798">-0</span></span>   | <span data-ttu-id="abcc0-1799">-0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1799">-0</span></span>   | <span data-ttu-id="abcc0-1800">+0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1800">+0</span></span>   | <span data-ttu-id="abcc0-1801">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1801">NaN</span></span>  | <span data-ttu-id="abcc0-1802">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1802">NaN</span></span>  | <span data-ttu-id="abcc0-1803">-0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1803">-0</span></span>   | <span data-ttu-id="abcc0-1804">+0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1804">+0</span></span>   | <span data-ttu-id="abcc0-1805">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1805">NaN</span></span>  | 
   | <span data-ttu-id="abcc0-1806">+inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1806">+inf</span></span> | <span data-ttu-id="abcc0-1807">+inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1807">+inf</span></span> | <span data-ttu-id="abcc0-1808">-inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1808">-inf</span></span> | <span data-ttu-id="abcc0-1809">+inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1809">+inf</span></span> | <span data-ttu-id="abcc0-1810">-inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1810">-inf</span></span> | <span data-ttu-id="abcc0-1811">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1811">NaN</span></span>  | <span data-ttu-id="abcc0-1812">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1812">NaN</span></span>  | <span data-ttu-id="abcc0-1813">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1813">NaN</span></span>  | 
   | <span data-ttu-id="abcc0-1814">-inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1814">-inf</span></span> | <span data-ttu-id="abcc0-1815">-inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1815">-inf</span></span> | <span data-ttu-id="abcc0-1816">+inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1816">+inf</span></span> | <span data-ttu-id="abcc0-1817">-inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1817">-inf</span></span> | <span data-ttu-id="abcc0-1818">+inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1818">+inf</span></span> | <span data-ttu-id="abcc0-1819">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1819">NaN</span></span>  | <span data-ttu-id="abcc0-1820">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1820">NaN</span></span>  | <span data-ttu-id="abcc0-1821">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1821">NaN</span></span>  | 
   | <span data-ttu-id="abcc0-1822">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1822">NaN</span></span>  | <span data-ttu-id="abcc0-1823">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1823">NaN</span></span>  | <span data-ttu-id="abcc0-1824">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1824">NaN</span></span>  | <span data-ttu-id="abcc0-1825">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1825">NaN</span></span>  | <span data-ttu-id="abcc0-1826">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1826">NaN</span></span>  | <span data-ttu-id="abcc0-1827">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1827">NaN</span></span>  | <span data-ttu-id="abcc0-1828">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1828">NaN</span></span>  | <span data-ttu-id="abcc0-1829">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1829">NaN</span></span>  | 

*  <span data-ttu-id="abcc0-1830">十進位部門：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1830">Decimal division:</span></span>

   ```csharp
   decimal operator /(decimal x, decimal y);
   ```

   <span data-ttu-id="abcc0-1831">如果右運算元的值為零，`System.DivideByZeroException`就會擲回。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1831">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span> <span data-ttu-id="abcc0-1832">產生的值是否太大，無法在代表`decimal`格式，`System.OverflowException`就會擲回。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1832">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="abcc0-1833">如果結果值太小而無法在代表`decimal`格式時，結果會是零。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1833">If the result value is too small to represent in the `decimal` format, the result is zero.</span></span> <span data-ttu-id="abcc0-1834">結果的小數位數是保留的結果會等於最小小數位數最接近的可代表的十進位值，則為 true 的數學結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1834">The scale of the result is the smallest scale that will preserve a result equal to the nearest representable decimal value to the true mathematical result.</span></span>

   <span data-ttu-id="abcc0-1835">十進位的除法就相當於使用此類型的除法運算子`System.Decimal`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1835">Decimal division is equivalent to using the division operator of type `System.Decimal`.</span></span>


### <a name="remainder-operator"></a><span data-ttu-id="abcc0-1836">餘數運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-1836">Remainder operator</span></span>

<span data-ttu-id="abcc0-1837">作業的表單`x % y`，二元運算子多載解析 ([二元運算子多載解析](expressions.md#binary-operator-overload-resolution)) 套用至選取的特定運算子實作。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1837">For an operation of the form `x % y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="abcc0-1838">運算元會轉換成所選的運算子的參數類型和結果的類型是運算子的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1838">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="abcc0-1839">以下列出預先定義的餘數運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1839">The predefined remainder operators are listed below.</span></span> <span data-ttu-id="abcc0-1840">這些運算子都會計算之間除法的餘數`x`和`y`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1840">The operators all compute the remainder of the division between `x` and `y`.</span></span>

*  <span data-ttu-id="abcc0-1841">整數餘數：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1841">Integer remainder:</span></span>

   ```csharp
   int operator %(int x, int y);
   uint operator %(uint x, uint y);
   long operator %(long x, long y);
   ulong operator %(ulong x, ulong y);
   ```

   <span data-ttu-id="abcc0-1842">結果`x % y`由所產生值`x - (x / y) * y`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1842">The result of `x % y` is the value produced by `x - (x / y) * y`.</span></span> <span data-ttu-id="abcc0-1843">如果`y`為零，`System.DivideByZeroException`就會擲回。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1843">If `y` is zero, a `System.DivideByZeroException` is thrown.</span></span>

   <span data-ttu-id="abcc0-1844">如果左的運算元是最小`int`或是`long`值和右運算元是`-1`、`System.OverflowException`就會擲回。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1844">If the left operand is the smallest `int` or `long` value and the right operand is `-1`, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="abcc0-1845">在任何情況下並未`x % y`擲回例外狀況，`x / y`不會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1845">In no case does `x % y` throw an exception where `x / y` would not throw an exception.</span></span>

*  <span data-ttu-id="abcc0-1846">浮點數餘數：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1846">Floating-point remainder:</span></span>

   ```csharp
   float operator %(float x, float y);
   double operator %(double x, double y);
   ```

   <span data-ttu-id="abcc0-1847">下表列出的非零的有限值的所有可能組合、 零、 無限及 NaN 的結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1847">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="abcc0-1848">在資料表中，`x`和`y`是有限的正值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1848">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="abcc0-1849">`z` 是的結果`x % y`並為計算`x - n * y`，其中`n`小於或等於最大可能整數`x / y`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1849">`z` is the result of `x % y` and is computed as `x - n * y`, where `n` is the largest possible integer that is less than or equal to `x / y`.</span></span> <span data-ttu-id="abcc0-1850">計算餘數的這個方法相當於使用整數運算元，但不同於 IEEE 754 定義 (所在`n`是最接近的整數`x / y`)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1850">This method of computing the remainder is analogous to that used for integer operands, but differs from the IEEE 754 definition (in which `n` is the integer closest to `x / y`).</span></span>

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="abcc0-1851">+y</span><span class="sxs-lookup"><span data-stu-id="abcc0-1851">+y</span></span>   | <span data-ttu-id="abcc0-1852">-y</span><span class="sxs-lookup"><span data-stu-id="abcc0-1852">-y</span></span>   | <span data-ttu-id="abcc0-1853">+0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1853">+0</span></span>   | <span data-ttu-id="abcc0-1854">-0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1854">-0</span></span>   | <span data-ttu-id="abcc0-1855">+inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1855">+inf</span></span> | <span data-ttu-id="abcc0-1856">-inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1856">-inf</span></span> | <span data-ttu-id="abcc0-1857">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1857">NaN</span></span>  | 
   | <span data-ttu-id="abcc0-1858">+x</span><span class="sxs-lookup"><span data-stu-id="abcc0-1858">+x</span></span>   | <span data-ttu-id="abcc0-1859">+z</span><span class="sxs-lookup"><span data-stu-id="abcc0-1859">+z</span></span>   | <span data-ttu-id="abcc0-1860">+z</span><span class="sxs-lookup"><span data-stu-id="abcc0-1860">+z</span></span>   | <span data-ttu-id="abcc0-1861">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1861">NaN</span></span>  | <span data-ttu-id="abcc0-1862">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1862">NaN</span></span>  | <span data-ttu-id="abcc0-1863">x</span><span class="sxs-lookup"><span data-stu-id="abcc0-1863">x</span></span>    | <span data-ttu-id="abcc0-1864">x</span><span class="sxs-lookup"><span data-stu-id="abcc0-1864">x</span></span>    | <span data-ttu-id="abcc0-1865">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1865">NaN</span></span>  | 
   | <span data-ttu-id="abcc0-1866">-x</span><span class="sxs-lookup"><span data-stu-id="abcc0-1866">-x</span></span>   | <span data-ttu-id="abcc0-1867">-z</span><span class="sxs-lookup"><span data-stu-id="abcc0-1867">-z</span></span>   | <span data-ttu-id="abcc0-1868">-z</span><span class="sxs-lookup"><span data-stu-id="abcc0-1868">-z</span></span>   | <span data-ttu-id="abcc0-1869">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1869">NaN</span></span>  | <span data-ttu-id="abcc0-1870">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1870">NaN</span></span>  | <span data-ttu-id="abcc0-1871">-x</span><span class="sxs-lookup"><span data-stu-id="abcc0-1871">-x</span></span>   | <span data-ttu-id="abcc0-1872">-x</span><span class="sxs-lookup"><span data-stu-id="abcc0-1872">-x</span></span>   | <span data-ttu-id="abcc0-1873">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1873">NaN</span></span>  | 
   | <span data-ttu-id="abcc0-1874">+0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1874">+0</span></span>   | <span data-ttu-id="abcc0-1875">+0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1875">+0</span></span>   | <span data-ttu-id="abcc0-1876">+0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1876">+0</span></span>   | <span data-ttu-id="abcc0-1877">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1877">NaN</span></span>  | <span data-ttu-id="abcc0-1878">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1878">NaN</span></span>  | <span data-ttu-id="abcc0-1879">+0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1879">+0</span></span>   | <span data-ttu-id="abcc0-1880">+0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1880">+0</span></span>   | <span data-ttu-id="abcc0-1881">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1881">NaN</span></span>  | 
   | <span data-ttu-id="abcc0-1882">-0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1882">-0</span></span>   | <span data-ttu-id="abcc0-1883">-0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1883">-0</span></span>   | <span data-ttu-id="abcc0-1884">-0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1884">-0</span></span>   | <span data-ttu-id="abcc0-1885">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1885">NaN</span></span>  | <span data-ttu-id="abcc0-1886">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1886">NaN</span></span>  | <span data-ttu-id="abcc0-1887">-0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1887">-0</span></span>   | <span data-ttu-id="abcc0-1888">-0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1888">-0</span></span>   | <span data-ttu-id="abcc0-1889">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1889">NaN</span></span>  | 
   | <span data-ttu-id="abcc0-1890">+inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1890">+inf</span></span> | <span data-ttu-id="abcc0-1891">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1891">NaN</span></span>  | <span data-ttu-id="abcc0-1892">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1892">NaN</span></span>  | <span data-ttu-id="abcc0-1893">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1893">NaN</span></span>  | <span data-ttu-id="abcc0-1894">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1894">NaN</span></span>  | <span data-ttu-id="abcc0-1895">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1895">NaN</span></span>  | <span data-ttu-id="abcc0-1896">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1896">NaN</span></span>  | <span data-ttu-id="abcc0-1897">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1897">NaN</span></span>  | 
   | <span data-ttu-id="abcc0-1898">-inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1898">-inf</span></span> | <span data-ttu-id="abcc0-1899">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1899">NaN</span></span>  | <span data-ttu-id="abcc0-1900">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1900">NaN</span></span>  | <span data-ttu-id="abcc0-1901">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1901">NaN</span></span>  | <span data-ttu-id="abcc0-1902">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1902">NaN</span></span>  | <span data-ttu-id="abcc0-1903">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1903">NaN</span></span>  | <span data-ttu-id="abcc0-1904">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1904">NaN</span></span>  | <span data-ttu-id="abcc0-1905">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1905">NaN</span></span>  | 
   | <span data-ttu-id="abcc0-1906">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1906">NaN</span></span>  | <span data-ttu-id="abcc0-1907">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1907">NaN</span></span>  | <span data-ttu-id="abcc0-1908">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1908">NaN</span></span>  | <span data-ttu-id="abcc0-1909">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1909">NaN</span></span>  | <span data-ttu-id="abcc0-1910">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1910">NaN</span></span>  | <span data-ttu-id="abcc0-1911">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1911">NaN</span></span>  | <span data-ttu-id="abcc0-1912">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1912">NaN</span></span>  | <span data-ttu-id="abcc0-1913">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1913">NaN</span></span>  | 

*  <span data-ttu-id="abcc0-1914">十進位的其餘部分：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1914">Decimal remainder:</span></span>

   ```csharp
   decimal operator %(decimal x, decimal y);
   ```

   <span data-ttu-id="abcc0-1915">如果右運算元的值為零，`System.DivideByZeroException`就會擲回。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1915">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span> <span data-ttu-id="abcc0-1916">較大的兩個運算元，標尺的小數位數的結果，在任何進位之前且正負號的結果，如果不是零，等同於的`x`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1916">The scale of the result, before any rounding, is the larger of the scales of the two operands, and the sign of the result, if non-zero, is the same as that of `x`.</span></span>

   <span data-ttu-id="abcc0-1917">十進位的其餘部分就相當於使用餘數運算子的型別`System.Decimal`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1917">Decimal remainder is equivalent to using the remainder operator of type `System.Decimal`.</span></span>


### <a name="addition-operator"></a><span data-ttu-id="abcc0-1918">加法運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-1918">Addition operator</span></span>

<span data-ttu-id="abcc0-1919">作業的表單`x + y`，二元運算子多載解析 ([二元運算子多載解析](expressions.md#binary-operator-overload-resolution)) 套用至選取的特定運算子實作。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1919">For an operation of the form `x + y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="abcc0-1920">運算元會轉換成所選的運算子的參數類型和結果的類型是運算子的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1920">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="abcc0-1921">以下列出預先定義的加法運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1921">The predefined addition operators are listed below.</span></span> <span data-ttu-id="abcc0-1922">對於數值和列舉型別，預先定義的加法運算子計算兩個運算元的總和。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1922">For numeric and enumeration types, the predefined addition operators compute the sum of the two operands.</span></span> <span data-ttu-id="abcc0-1923">當一個或兩個運算元字串類型時，預先定義的加法運算子會串連運算元的字串表示。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1923">When one or both operands are of type string, the predefined addition operators concatenate the string representation of the operands.</span></span>

*  <span data-ttu-id="abcc0-1924">整數加法：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1924">Integer addition:</span></span>

   ```csharp
   int operator +(int x, int y);
   uint operator +(uint x, uint y);
   long operator +(long x, long y);
   ulong operator +(ulong x, ulong y);
   ```

   <span data-ttu-id="abcc0-1925">在 `checked`情況下，如果總和超出範圍的結果型別，`System.OverflowException`就會擲回。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1925">In a `checked` context, if the sum is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="abcc0-1926">在 `unchecked`內容中，不會報告溢位，並捨棄任何重大高序位位元的範圍之外的結果型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1926">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>

*  <span data-ttu-id="abcc0-1927">浮點加法：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1927">Floating-point addition:</span></span>

   ```csharp
   float operator +(float x, float y);
   double operator +(double x, double y);
   ```

   <span data-ttu-id="abcc0-1928">根據 IEEE 754 算術的規則會計算總和。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1928">The sum is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="abcc0-1929">下表列出的非零的有限值的所有可能組合、 零、 無限及 NaN 的結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1929">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="abcc0-1930">在資料表中，`x`並`y`是非零的有限值，並`z`結果`x + y`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1930">In the table, `x` and `y` are nonzero finite values, and `z` is the result of `x + y`.</span></span> <span data-ttu-id="abcc0-1931">如果`x`並`y`具有相同大小但正負號，相反`z`是正零。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1931">If `x` and `y` have the same magnitude but opposite signs, `z` is positive zero.</span></span> <span data-ttu-id="abcc0-1932">如果`x + y`太大，目的型別，表示`z`是使用相同的簽章，為無限大`x + y`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1932">If `x + y` is too large to represent in the destination type, `z` is an infinity with the same sign as `x + y`.</span></span>

   |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="abcc0-1933">y</span><span class="sxs-lookup"><span data-stu-id="abcc0-1933">y</span></span>    | <span data-ttu-id="abcc0-1934">+0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1934">+0</span></span>   | <span data-ttu-id="abcc0-1935">-0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1935">-0</span></span>   | <span data-ttu-id="abcc0-1936">+inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1936">+inf</span></span> | <span data-ttu-id="abcc0-1937">-inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1937">-inf</span></span> | <span data-ttu-id="abcc0-1938">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1938">NaN</span></span>  | 
   | <span data-ttu-id="abcc0-1939">x</span><span class="sxs-lookup"><span data-stu-id="abcc0-1939">x</span></span>    | <span data-ttu-id="abcc0-1940">z</span><span class="sxs-lookup"><span data-stu-id="abcc0-1940">z</span></span>    | <span data-ttu-id="abcc0-1941">x</span><span class="sxs-lookup"><span data-stu-id="abcc0-1941">x</span></span>    | <span data-ttu-id="abcc0-1942">x</span><span class="sxs-lookup"><span data-stu-id="abcc0-1942">x</span></span>    | <span data-ttu-id="abcc0-1943">+inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1943">+inf</span></span> | <span data-ttu-id="abcc0-1944">-inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1944">-inf</span></span> | <span data-ttu-id="abcc0-1945">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1945">NaN</span></span>  | 
   | <span data-ttu-id="abcc0-1946">+0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1946">+0</span></span>   | <span data-ttu-id="abcc0-1947">y</span><span class="sxs-lookup"><span data-stu-id="abcc0-1947">y</span></span>    | <span data-ttu-id="abcc0-1948">+0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1948">+0</span></span>   | <span data-ttu-id="abcc0-1949">+0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1949">+0</span></span>   | <span data-ttu-id="abcc0-1950">+inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1950">+inf</span></span> | <span data-ttu-id="abcc0-1951">-inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1951">-inf</span></span> | <span data-ttu-id="abcc0-1952">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1952">NaN</span></span>  | 
   | <span data-ttu-id="abcc0-1953">-0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1953">-0</span></span>   | <span data-ttu-id="abcc0-1954">y</span><span class="sxs-lookup"><span data-stu-id="abcc0-1954">y</span></span>    | <span data-ttu-id="abcc0-1955">+0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1955">+0</span></span>   | <span data-ttu-id="abcc0-1956">-0</span><span class="sxs-lookup"><span data-stu-id="abcc0-1956">-0</span></span>   | <span data-ttu-id="abcc0-1957">+inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1957">+inf</span></span> | <span data-ttu-id="abcc0-1958">-inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1958">-inf</span></span> | <span data-ttu-id="abcc0-1959">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1959">NaN</span></span>  | 
   | <span data-ttu-id="abcc0-1960">+inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1960">+inf</span></span> | <span data-ttu-id="abcc0-1961">+inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1961">+inf</span></span> | <span data-ttu-id="abcc0-1962">+inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1962">+inf</span></span> | <span data-ttu-id="abcc0-1963">+inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1963">+inf</span></span> | <span data-ttu-id="abcc0-1964">+inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1964">+inf</span></span> | <span data-ttu-id="abcc0-1965">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1965">NaN</span></span>  | <span data-ttu-id="abcc0-1966">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1966">NaN</span></span>  | 
   | <span data-ttu-id="abcc0-1967">-inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1967">-inf</span></span> | <span data-ttu-id="abcc0-1968">-inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1968">-inf</span></span> | <span data-ttu-id="abcc0-1969">-inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1969">-inf</span></span> | <span data-ttu-id="abcc0-1970">-inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1970">-inf</span></span> | <span data-ttu-id="abcc0-1971">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1971">NaN</span></span>  | <span data-ttu-id="abcc0-1972">-inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-1972">-inf</span></span> | <span data-ttu-id="abcc0-1973">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1973">NaN</span></span>  | 
   | <span data-ttu-id="abcc0-1974">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1974">NaN</span></span>  | <span data-ttu-id="abcc0-1975">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1975">NaN</span></span>  | <span data-ttu-id="abcc0-1976">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1976">NaN</span></span>  | <span data-ttu-id="abcc0-1977">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1977">NaN</span></span>  | <span data-ttu-id="abcc0-1978">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1978">NaN</span></span>  | <span data-ttu-id="abcc0-1979">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1979">NaN</span></span>  | <span data-ttu-id="abcc0-1980">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-1980">NaN</span></span>  | 

*  <span data-ttu-id="abcc0-1981">十進位的加法：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1981">Decimal addition:</span></span>

   ```csharp
   decimal operator +(decimal x, decimal y);
   ```

   <span data-ttu-id="abcc0-1982">產生的值是否太大，無法在代表`decimal`格式，`System.OverflowException`就會擲回。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1982">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="abcc0-1983">結果，在任何進位之前的標尺是較大的兩個運算元的標尺。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1983">The scale of the result, before any rounding, is the larger of the scales of the two operands.</span></span>

   <span data-ttu-id="abcc0-1984">十進位新增相當於使用的型別，加法運算子`System.Decimal`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1984">Decimal addition is equivalent to using the addition operator of type `System.Decimal`.</span></span>

*  <span data-ttu-id="abcc0-1985">列舉型別加入。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1985">Enumeration addition.</span></span> <span data-ttu-id="abcc0-1986">每個列舉類型都會隱含地提供下列預先定義的運算子，其中`E`是列舉類型，並`U`為基礎的類型`E`:</span><span class="sxs-lookup"><span data-stu-id="abcc0-1986">Every enumeration type implicitly provides the following predefined operators, where `E` is the enum type, and `U` is the underlying type of `E`:</span></span>

   ```csharp
   E operator +(E x, U y);
   E operator +(U x, E y);
   ```

   <span data-ttu-id="abcc0-1987">在執行階段評估這些運算子完全相同`(E)((U)x + (U)y)`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1987">At run-time these operators are evaluated exactly as `(E)((U)x + (U)y)`.</span></span>

*  <span data-ttu-id="abcc0-1988">字串串連：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1988">String concatenation:</span></span>

   ```csharp
   string operator +(string x, string y);
   string operator +(string x, object y);
   string operator +(object x, string y);
   ```

   <span data-ttu-id="abcc0-1989">這些多載的二進位檔`+`運算子執行字串串連。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1989">These overloads of the binary `+` operator perform string concatenation.</span></span> <span data-ttu-id="abcc0-1990">如果運算元字串串連的`null`，空字串用來替代。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1990">If an operand of string concatenation is `null`, an empty string is substituted.</span></span> <span data-ttu-id="abcc0-1991">否則，任何非字串引數會轉換為其字串表示所叫用的虛擬`ToString`方法繼承自型別的`object`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1991">Otherwise, any non-string argument is converted to its string representation by invoking the virtual `ToString` method inherited from type `object`.</span></span> <span data-ttu-id="abcc0-1992">如果`ToString`傳回`null`，空字串用來替代。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1992">If `ToString` returns `null`, an empty string is substituted.</span></span>

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

   字串串連運算子的結果是後面接著的字元，右邊運算元的左運算元的字元所組成的字串。 字串串連運算子永遠不會傳回`null`值。 <span data-ttu-id="abcc0-1995">A`System.OutOfMemoryException`如果記憶體不足，無法配置所產生的字串可能會擲回。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1995">A `System.OutOfMemoryException` may be thrown if there is not enough memory available to allocate the resulting string.</span></span>

*  <span data-ttu-id="abcc0-1996">委派組合。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1996">Delegate combination.</span></span> <span data-ttu-id="abcc0-1997">每個委派型別都會隱含地提供下列預先定義的運算子，其中`D`是委派型別：</span><span class="sxs-lookup"><span data-stu-id="abcc0-1997">Every delegate type implicitly provides the following predefined operator, where `D` is the delegate type:</span></span>

   ```csharp
   D operator +(D x, D y);
   ```

   <span data-ttu-id="abcc0-1998">二進位`+`這兩個運算元都是相同委派型別時，運算子會執行委派組合`D`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1998">The binary `+` operator performs delegate combination when both operands are of some delegate type `D`.</span></span> <span data-ttu-id="abcc0-1999">（如果運算元有不同的委派類型時，繫結階段會發生錯誤。）如果在第一個運算元`null`，此作業的結果是第二個運算元的值 (即使這也是`null`)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-1999">(If the operands have different delegate types, a binding-time error occurs.) If the first operand is `null`, the result of the operation is the value of the second operand (even if that is also `null`).</span></span> <span data-ttu-id="abcc0-2000">否則，如果第二個運算元是`null`，則作業的結果為第一個運算元的值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2000">Otherwise, if the second operand is `null`, then the result of the operation is the value of the first operand.</span></span> <span data-ttu-id="abcc0-2001">否則，作業的結果就是新的委派執行個體，當叫用時，會叫用第一個運算元而再叫用第二個運算元。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2001">Otherwise, the result of the operation is a new delegate instance that, when invoked, invokes the first operand and then invokes the second operand.</span></span> <span data-ttu-id="abcc0-2002">例如委派組合的詳細資訊，請參閱[減法運算子](expressions.md#subtraction-operator)並[委派引動過程](delegates.md#delegate-invocation)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2002">For examples of delegate combination, see [Subtraction operator](expressions.md#subtraction-operator) and [Delegate invocation](delegates.md#delegate-invocation).</span></span> <span data-ttu-id="abcc0-2003">由於`System.Delegate`不是委派型別`operator` `+`未定義它。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2003">Since `System.Delegate` is not a delegate type, `operator` `+` is not defined for it.</span></span>

### <a name="subtraction-operator"></a><span data-ttu-id="abcc0-2004">減法運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-2004">Subtraction operator</span></span>

<span data-ttu-id="abcc0-2005">作業的表單`x - y`，二元運算子多載解析 ([二元運算子多載解析](expressions.md#binary-operator-overload-resolution)) 套用至選取的特定運算子實作。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2005">For an operation of the form `x - y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="abcc0-2006">運算元會轉換成所選的運算子的參數類型和結果的類型是運算子的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2006">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="abcc0-2007">以下列出預先定義的減法運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2007">The predefined subtraction operators are listed below.</span></span> <span data-ttu-id="abcc0-2008">所有減運算子`y`從`x`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2008">The operators all subtract `y` from `x`.</span></span>

*  <span data-ttu-id="abcc0-2009">整數減法：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2009">Integer subtraction:</span></span>

   ```csharp
   int operator -(int x, int y);
   uint operator -(uint x, uint y);
   long operator -(long x, long y);
   ulong operator -(ulong x, ulong y);
   ```

   <span data-ttu-id="abcc0-2010">在 `checked`情況下，差異在於範圍之外的結果型別，如果`System.OverflowException`就會擲回。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2010">In a `checked` context, if the difference is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="abcc0-2011">在 `unchecked`內容中，不會報告溢位，並捨棄任何重大高序位位元的範圍之外的結果型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2011">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>

*  <span data-ttu-id="abcc0-2012">浮點數的減法運算：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2012">Floating-point subtraction:</span></span>

   ```csharp
   float operator -(float x, float y);
   double operator -(double x, double y);
   ```

   <span data-ttu-id="abcc0-2013">根據 IEEE 754 算術的規則被計算的差異。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2013">The difference is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="abcc0-2014">下表列出非零的有限值的所有可能組合、 零、 無限及 Nan 的結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2014">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaNs.</span></span> <span data-ttu-id="abcc0-2015">在資料表中，`x`並`y`是非零的有限值，並`z`結果`x - y`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2015">In the table, `x` and `y` are nonzero finite values, and `z` is the result of `x - y`.</span></span> <span data-ttu-id="abcc0-2016">如果`x`並`y`相等，`z`是正零。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2016">If `x` and `y` are equal, `z` is positive zero.</span></span> <span data-ttu-id="abcc0-2017">如果`x - y`太大，目的型別，表示`z`是使用相同的簽章，為無限大`x - y`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2017">If `x - y` is too large to represent in the destination type, `z` is an infinity with the same sign as `x - y`.</span></span>

   |      |      |      |      |      |      |     |
   |:----:|:----:|:----:|:----:|:----:|:----:|:---:|
   |      | <span data-ttu-id="abcc0-2018">y</span><span class="sxs-lookup"><span data-stu-id="abcc0-2018">y</span></span>    | <span data-ttu-id="abcc0-2019">+0</span><span class="sxs-lookup"><span data-stu-id="abcc0-2019">+0</span></span>   | <span data-ttu-id="abcc0-2020">-0</span><span class="sxs-lookup"><span data-stu-id="abcc0-2020">-0</span></span>   | <span data-ttu-id="abcc0-2021">+inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-2021">+inf</span></span> | <span data-ttu-id="abcc0-2022">-inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-2022">-inf</span></span> | <span data-ttu-id="abcc0-2023">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-2023">NaN</span></span> | 
   | <span data-ttu-id="abcc0-2024">x</span><span class="sxs-lookup"><span data-stu-id="abcc0-2024">x</span></span>    | <span data-ttu-id="abcc0-2025">z</span><span class="sxs-lookup"><span data-stu-id="abcc0-2025">z</span></span>    | <span data-ttu-id="abcc0-2026">x</span><span class="sxs-lookup"><span data-stu-id="abcc0-2026">x</span></span>    | <span data-ttu-id="abcc0-2027">x</span><span class="sxs-lookup"><span data-stu-id="abcc0-2027">x</span></span>    | <span data-ttu-id="abcc0-2028">-inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-2028">-inf</span></span> | <span data-ttu-id="abcc0-2029">+inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-2029">+inf</span></span> | <span data-ttu-id="abcc0-2030">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-2030">NaN</span></span> | 
   | <span data-ttu-id="abcc0-2031">+0</span><span class="sxs-lookup"><span data-stu-id="abcc0-2031">+0</span></span>   | <span data-ttu-id="abcc0-2032">-y</span><span class="sxs-lookup"><span data-stu-id="abcc0-2032">-y</span></span>   | <span data-ttu-id="abcc0-2033">+0</span><span class="sxs-lookup"><span data-stu-id="abcc0-2033">+0</span></span>   | <span data-ttu-id="abcc0-2034">+0</span><span class="sxs-lookup"><span data-stu-id="abcc0-2034">+0</span></span>   | <span data-ttu-id="abcc0-2035">-inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-2035">-inf</span></span> | <span data-ttu-id="abcc0-2036">+inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-2036">+inf</span></span> | <span data-ttu-id="abcc0-2037">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-2037">NaN</span></span> | 
   | <span data-ttu-id="abcc0-2038">-0</span><span class="sxs-lookup"><span data-stu-id="abcc0-2038">-0</span></span>   | <span data-ttu-id="abcc0-2039">-y</span><span class="sxs-lookup"><span data-stu-id="abcc0-2039">-y</span></span>   | <span data-ttu-id="abcc0-2040">-0</span><span class="sxs-lookup"><span data-stu-id="abcc0-2040">-0</span></span>   | <span data-ttu-id="abcc0-2041">+0</span><span class="sxs-lookup"><span data-stu-id="abcc0-2041">+0</span></span>   | <span data-ttu-id="abcc0-2042">-inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-2042">-inf</span></span> | <span data-ttu-id="abcc0-2043">+inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-2043">+inf</span></span> | <span data-ttu-id="abcc0-2044">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-2044">NaN</span></span> | 
   | <span data-ttu-id="abcc0-2045">+inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-2045">+inf</span></span> | <span data-ttu-id="abcc0-2046">+inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-2046">+inf</span></span> | <span data-ttu-id="abcc0-2047">+inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-2047">+inf</span></span> | <span data-ttu-id="abcc0-2048">+inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-2048">+inf</span></span> | <span data-ttu-id="abcc0-2049">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-2049">NaN</span></span>  | <span data-ttu-id="abcc0-2050">+inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-2050">+inf</span></span> | <span data-ttu-id="abcc0-2051">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-2051">NaN</span></span> | 
   | <span data-ttu-id="abcc0-2052">-inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-2052">-inf</span></span> | <span data-ttu-id="abcc0-2053">-inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-2053">-inf</span></span> | <span data-ttu-id="abcc0-2054">-inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-2054">-inf</span></span> | <span data-ttu-id="abcc0-2055">-inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-2055">-inf</span></span> | <span data-ttu-id="abcc0-2056">-inf</span><span class="sxs-lookup"><span data-stu-id="abcc0-2056">-inf</span></span> | <span data-ttu-id="abcc0-2057">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-2057">NaN</span></span>  | <span data-ttu-id="abcc0-2058">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-2058">NaN</span></span> | 
   | <span data-ttu-id="abcc0-2059">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-2059">NaN</span></span>  | <span data-ttu-id="abcc0-2060">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-2060">NaN</span></span>  | <span data-ttu-id="abcc0-2061">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-2061">NaN</span></span>  | <span data-ttu-id="abcc0-2062">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-2062">NaN</span></span>  | <span data-ttu-id="abcc0-2063">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-2063">NaN</span></span>  | <span data-ttu-id="abcc0-2064">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-2064">NaN</span></span>  | <span data-ttu-id="abcc0-2065">NaN</span><span class="sxs-lookup"><span data-stu-id="abcc0-2065">NaN</span></span> | 

*  <span data-ttu-id="abcc0-2066">十進位的減法運算：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2066">Decimal subtraction:</span></span>

   ```csharp
   decimal operator -(decimal x, decimal y);
   ```

   <span data-ttu-id="abcc0-2067">產生的值是否太大，無法在代表`decimal`格式，`System.OverflowException`就會擲回。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2067">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="abcc0-2068">結果，在任何進位之前的標尺是較大的兩個運算元的標尺。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2068">The scale of the result, before any rounding, is the larger of the scales of the two operands.</span></span>

   <span data-ttu-id="abcc0-2069">十進位減法相當於使用此類型的減法運算子`System.Decimal`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2069">Decimal subtraction is equivalent to using the subtraction operator of type `System.Decimal`.</span></span>

*  <span data-ttu-id="abcc0-2070">列舉型別減法。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2070">Enumeration subtraction.</span></span> <span data-ttu-id="abcc0-2071">每個列舉類型都會隱含地提供下列預先定義的運算子，其中`E`是列舉類型，並`U`為基礎的類型`E`:</span><span class="sxs-lookup"><span data-stu-id="abcc0-2071">Every enumeration type implicitly provides the following predefined operator, where `E` is the enum type, and `U` is the underlying type of `E`:</span></span>

   ```csharp
   U operator -(E x, E y);
   ```

   <span data-ttu-id="abcc0-2072">這個運算子會評估完全為`(U)((U)x - (U)y)`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2072">This operator is evaluated exactly as `(U)((U)x - (U)y)`.</span></span> <span data-ttu-id="abcc0-2073">換句話說，運算子會計算的值之間的差異`x`和`y`，且結果類型是列舉的基礎類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2073">In other words, the operator computes the difference between the ordinal values of `x` and `y`, and the type of the result is the underlying type of the enumeration.</span></span>

   ```csharp
   E operator -(E x, U y);
   ```

   <span data-ttu-id="abcc0-2074">這個運算子會評估完全為`(E)((U)x - y)`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2074">This operator is evaluated exactly as `(E)((U)x - y)`.</span></span> <span data-ttu-id="abcc0-2075">換句話說，運算子減去值之基礎類型的列舉型別，產生的列舉值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2075">In other words, the operator subtracts a value from the underlying type of the enumeration, yielding a value of the enumeration.</span></span>

*  <span data-ttu-id="abcc0-2076">委派移除。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2076">Delegate removal.</span></span> <span data-ttu-id="abcc0-2077">每個委派型別都會隱含地提供下列預先定義的運算子，其中`D`是委派型別：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2077">Every delegate type implicitly provides the following predefined operator, where `D` is the delegate type:</span></span>

   ```csharp
   D operator -(D x, D y);
   ```

   <span data-ttu-id="abcc0-2078">二進位`-`這兩個運算元都是相同委派型別時，運算子會執行委派移除`D`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2078">The binary `-` operator performs delegate removal when both operands are of some delegate type `D`.</span></span> <span data-ttu-id="abcc0-2079">如果運算元有不同的委派類型時，繫結階段會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2079">If the operands have different delegate types, a binding-time error occurs.</span></span> <span data-ttu-id="abcc0-2080">如果在第一個運算元`null`，此作業的結果是`null`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2080">If the first operand is `null`, the result of the operation is `null`.</span></span> <span data-ttu-id="abcc0-2081">否則，如果第二個運算元是`null`，則作業的結果為第一個運算元的值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2081">Otherwise, if the second operand is `null`, then the result of the operation is the value of the first operand.</span></span> <span data-ttu-id="abcc0-2082">否則，這兩個運算元都代表引動過程清單 ([委派宣告](delegates.md#delegate-declarations)) 具有一或多個項目，以及結果是新的引動過程清單移除的第二個運算元的項目所組成的第一個運算元的清單它提供第二個運算元的清單是適當的連續子清單的第一個。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2082">Otherwise, both operands represent invocation lists ([Delegate declarations](delegates.md#delegate-declarations)) having one or more entries, and the result is a new invocation list consisting of the first operand's list with the second operand's entries removed from it, provided the second operand's list is a proper contiguous sublist of the first's.</span></span>     <span data-ttu-id="abcc0-2083">(若要判斷子清單相等，對應的項目會比較和委派等號比較運算子 ([委派等號比較運算子](expressions.md#delegate-equality-operators))。)否則，結果就是左運算元的值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2083">(To determine sublist equality, corresponding entries are compared as for the delegate equality operator ([Delegate equality operators](expressions.md#delegate-equality-operators)).) Otherwise, the result is the value of the left operand.</span></span> <span data-ttu-id="abcc0-2084">任一運算元的清單會變更處理序中。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2084">Neither of the operands' lists is changed in the process.</span></span> <span data-ttu-id="abcc0-2085">如果第二個運算元的清單會比對多個的之子清單的第一個運算元的清單中的連續項目，則會移除最右邊的相符子清單的連續項目。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2085">If the second operand's list matches multiple sublists of contiguous entries in the first operand's list, the right-most matching sublist of contiguous entries is removed.</span></span> <span data-ttu-id="abcc0-2086">如果移除會導致空的清單，則結果是`null`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2086">If removal results in an empty list, the result is `null`.</span></span> <span data-ttu-id="abcc0-2087">例如: </span><span class="sxs-lookup"><span data-stu-id="abcc0-2087">For example:</span></span>

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

## <a name="shift-operators"></a><span data-ttu-id="abcc0-2088">移位運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-2088">Shift operators</span></span>

<span data-ttu-id="abcc0-2089">`<<`和`>>`運算子用來執行位元移位運算。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2089">The `<<` and `>>` operators are used to perform bit shifting operations.</span></span>

```antlr
shift_expression
    : additive_expression
    | shift_expression '<<' additive_expression
    | shift_expression right_shift additive_expression
    ;
```

<span data-ttu-id="abcc0-2090">如果運算元*shift_expression*已在編譯時期型別`dynamic`，然後動態地繫結運算式 ([動態繫結](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2090">If an operand of a *shift_expression* has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="abcc0-2091">運算式的編譯時期型別是在此情況下`dynamic`，如下所述的解析度會發生在執行階段使用這些已編譯時間類型的運算元的執行階段型別和`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2091">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="abcc0-2092">作業的表單`x << count`或是`x >> count`，二元運算子多載解析 ([二元運算子多載解析](expressions.md#binary-operator-overload-resolution)) 套用至選取的特定運算子實作。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2092">For an operation of the form `x << count` or `x >> count`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="abcc0-2093">運算元會轉換成所選的運算子的參數類型和結果的類型是運算子的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2093">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="abcc0-2094">宣告多載的移位運算子，第一個運算元的類型必須一律是類別或結構，包含運算子宣告中，而且第二個運算元的類型必須永遠`int`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2094">When declaring an overloaded shift operator, the type of the first operand must always be the class or struct containing the operator declaration, and the type of the second operand must always be `int`.</span></span>

<span data-ttu-id="abcc0-2095">以下列出預先定義的移位運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2095">The predefined shift operators are listed below.</span></span>

*  <span data-ttu-id="abcc0-2096">提早測試：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2096">Shift left:</span></span>

   ```csharp
   int operator <<(int x, int count);
   uint operator <<(uint x, int count);
   long operator <<(long x, int count);
   ulong operator <<(ulong x, int count);
   ```

   <span data-ttu-id="abcc0-2097">`<<`運算子的排班`x`左旋轉的位元數計算，如下所述。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2097">The `<<` operator shifts `x` left by a number of bits computed as described below.</span></span>

   <span data-ttu-id="abcc0-2098">高序位位元的範圍之外的結果型別的`x`會捨棄其餘的位元會向左移位，而的低序位空的位元位置會設定為零。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2098">The high-order bits outside the range of the result type of `x` are discarded, the remaining bits are shifted left, and the low-order empty bit positions are set to zero.</span></span>

*  <span data-ttu-id="abcc0-2099">向右移位：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2099">Shift right:</span></span>

   ```csharp
   int operator >>(int x, int count);
   uint operator >>(uint x, int count);
   long operator >>(long x, int count);
   ulong operator >>(ulong x, int count);
   ```

   <span data-ttu-id="abcc0-2100">`>>`運算子的排班`x`權限的位元數計算，如下所述。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2100">The `>>` operator shifts `x` right by a number of bits computed as described below.</span></span>

   <span data-ttu-id="abcc0-2101">當`x`屬於型別`int`或`long`的低序位位元`x`會捨棄，其餘的位元會向右移位，而且的高序位空的位元位置會設定為零，如果`x`是非負值，而且設定為一個`x`為負數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2101">When `x` is of type `int` or `long`, the low-order bits of `x` are discarded, the remaining bits are shifted right, and the high-order empty bit positions are set to zero if `x` is non-negative and set to one if `x` is negative.</span></span>

   <span data-ttu-id="abcc0-2102">當`x`屬於型別`uint`或`ulong`的低序位位元`x`會捨棄其餘的位元會向右移位，和的高序位空的位元位置會設定為零。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2102">When `x` is of type `uint` or `ulong`, the low-order bits of `x` are discarded, the remaining bits are shifted right, and the high-order empty bit positions are set to zero.</span></span>

<span data-ttu-id="abcc0-2103">預先定義的運算子，移位的位元數字的計算方式如下：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2103">For the predefined operators, the number of bits to shift is computed as follows:</span></span>

*  <span data-ttu-id="abcc0-2104">當的型別`x`是`int`或`uint`，則指定移位計數是低序位五個位元的`count`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2104">When the type of `x` is `int` or `uint`, the shift count is given by the low-order five bits of `count`.</span></span> <span data-ttu-id="abcc0-2105">換句話說，移位計數從計算`count & 0x1F`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2105">In other words, the shift count is computed from `count & 0x1F`.</span></span>
*  <span data-ttu-id="abcc0-2106">當的型別`x`是`long`或是`ulong`的低序位六位元指定移位計數是`count`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2106">When the type of `x` is `long` or `ulong`, the shift count is given by the low-order six bits of `count`.</span></span> <span data-ttu-id="abcc0-2107">換句話說，移位計數從計算`count & 0x3F`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2107">In other words, the shift count is computed from `count & 0x3F`.</span></span>

<span data-ttu-id="abcc0-2108">如果產生的移位計數為零，移位運算子只會傳回的值`x`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2108">If the resulting shift count is zero, the shift operators simply return the value of `x`.</span></span>

<span data-ttu-id="abcc0-2109">移位作業絕不會造成溢位，並產生相同的結果中`checked`和`unchecked`內容。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2109">Shift operations never cause overflows and produce the same results in `checked` and `unchecked` contexts.</span></span>

<span data-ttu-id="abcc0-2110">當左的運算元`>>`運算子不帶正負號的整數類資料類型，運算子就會執行算術右運算元的最大顯著性位元 （正負號位元） 值會傳播其中至空白高序位的位元位置。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2110">When the left operand of the `>>` operator is of a signed integral type, the operator performs an arithmetic shift right wherein the value of the most significant bit (the sign bit) of the operand is propagated to the high-order empty bit positions.</span></span> <span data-ttu-id="abcc0-2111">當左的運算元`>>`運算子是的不帶正負號的整數類資料類型時，運算子就會執行邏輯向右移位，其中空高序位的位元位置一律會設定為零。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2111">When the left operand of the `>>` operator is of an unsigned integral type, the operator performs a logical shift right wherein high-order empty bit positions are always set to zero.</span></span> <span data-ttu-id="abcc0-2112">若要執行作業的運算元類型推斷相反，可以使用明確的轉換。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2112">To perform the opposite operation of that inferred from the operand type, explicit casts can be used.</span></span> <span data-ttu-id="abcc0-2113">比方說，如果`x`類型的變數`int`，此作業`unchecked((int)((uint)x >> y))`就會執行邏輯的`x`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2113">For example, if `x` is a variable of type `int`, the operation `unchecked((int)((uint)x >> y))` performs a logical shift right of `x`.</span></span>

## <a name="relational-and-type-testing-operators"></a><span data-ttu-id="abcc0-2114">關係和類型測試運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-2114">Relational and type-testing operators</span></span>

<span data-ttu-id="abcc0-2115">`==`， `!=`， `<`， `>`， `<=`， `>=`，`is`和`as`運算子都稱為關係和類型測試運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2115">The `==`, `!=`, `<`, `>`, `<=`, `>=`, `is` and `as` operators are called the relational and type-testing operators.</span></span>

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

<span data-ttu-id="abcc0-2116">`is`運算子所述[是運算子](expressions.md#the-is-operator)並`as`運算子所述[As 運算子](expressions.md#the-as-operator)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2116">The `is` operator is described in [The is operator](expressions.md#the-is-operator) and the `as` operator is described in [The as operator](expressions.md#the-as-operator).</span></span>

<span data-ttu-id="abcc0-2117">`==`， `!=`， `<`， `>`，`<=`並`>=`運算子***比較運算子***。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2117">The `==`, `!=`, `<`, `>`, `<=` and `>=` operators are ***comparison operators***.</span></span>

<span data-ttu-id="abcc0-2118">比較運算子的運算元是否在編譯時期型別`dynamic`，然後動態地繫結運算式 ([動態繫結](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2118">If an operand of a comparison operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="abcc0-2119">運算式的編譯時期型別是在此情況下`dynamic`，如下所述的解析度會發生在執行階段使用這些已編譯時間類型的運算元的執行階段型別和`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2119">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="abcc0-2120">作業的表單`x` *op* `y`，其中*op*是一個比較運算子，而多載解析 ([二元運算子多載解析](expressions.md#binary-operator-overload-resolution)) 套用至選取的特定運算子實作。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2120">For an operation of the form `x` *op* `y`, where *op* is a comparison operator, overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="abcc0-2121">運算元會轉換成所選的運算子的參數類型和結果的類型是運算子的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2121">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="abcc0-2122">預先定義的比較運算子是由下列各節所述。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2122">The predefined comparison operators are described in the following sections.</span></span> <span data-ttu-id="abcc0-2123">所有預先定義的比較運算子傳回類型之結果的`bool`下, 表中所述。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2123">All predefined comparison operators return a result of type `bool`, as described in the following table.</span></span>


| <span data-ttu-id="abcc0-2124">__Operation__</span><span class="sxs-lookup"><span data-stu-id="abcc0-2124">__Operation__</span></span> | <span data-ttu-id="abcc0-2125">__結果__</span><span class="sxs-lookup"><span data-stu-id="abcc0-2125">__Result__</span></span>                                                       |
|---------------|------------------------------------------------------------------|
| `x == y`      | <span data-ttu-id="abcc0-2126">`true` 如果`x`等於`y`，`false`否則</span><span class="sxs-lookup"><span data-stu-id="abcc0-2126">`true` if `x` is equal to `y`, `false` otherwise</span></span>                 | 
| `x != y`      | <span data-ttu-id="abcc0-2127">`true` 如果`x`不等於`y`，`false`否則</span><span class="sxs-lookup"><span data-stu-id="abcc0-2127">`true` if `x` is not equal to `y`, `false` otherwise</span></span>             | 
| `x < y`       | <span data-ttu-id="abcc0-2128">`true` 如果`x`是小於`y`，`false`否則</span><span class="sxs-lookup"><span data-stu-id="abcc0-2128">`true` if `x` is less than `y`, `false` otherwise</span></span>                | 
| `x > y`       | <span data-ttu-id="abcc0-2129">`true` 如果`x`大於`y`，`false`否則</span><span class="sxs-lookup"><span data-stu-id="abcc0-2129">`true` if `x` is greater than `y`, `false` otherwise</span></span>             | 
| `x <= y`      | <span data-ttu-id="abcc0-2130">`true` 如果`x`小於或等於`y`，`false`否則</span><span class="sxs-lookup"><span data-stu-id="abcc0-2130">`true` if `x` is less than or equal to `y`, `false` otherwise</span></span>    | 
| `x >= y`      | <span data-ttu-id="abcc0-2131">`true` 如果`x`大於或等於`y`，`false`否則</span><span class="sxs-lookup"><span data-stu-id="abcc0-2131">`true` if `x` is greater than or equal to `y`, `false` otherwise</span></span> | 

### <a name="integer-comparison-operators"></a><span data-ttu-id="abcc0-2132">整數比較運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-2132">Integer comparison operators</span></span>

<span data-ttu-id="abcc0-2133">預先定義的整數比較運算子包括︰</span><span class="sxs-lookup"><span data-stu-id="abcc0-2133">The predefined integer comparison operators are:</span></span>
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

<span data-ttu-id="abcc0-2134">每個運算子比較兩個整數運算元和傳回的數值`bool`值，指出特定的關聯性是否`true`或`false`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2134">Each of these operators compares the numeric values of the two integer operands and returns a `bool` value that indicates whether the particular relation is `true` or `false`.</span></span>

### <a name="floating-point-comparison-operators"></a><span data-ttu-id="abcc0-2135">浮點數的比較運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-2135">Floating-point comparison operators</span></span>

<span data-ttu-id="abcc0-2136">預先定義的比較運算子包括︰</span><span class="sxs-lookup"><span data-stu-id="abcc0-2136">The predefined floating-point comparison operators are:</span></span>
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

<span data-ttu-id="abcc0-2137">運算子會比較運算元根據 IEEE 754 標準的規則：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2137">The operators compare the operands according to the rules of the IEEE 754 standard:</span></span>

*  <span data-ttu-id="abcc0-2138">如果任一個運算元是 NaN，則結果是`false`以外的所有運算子`!=`，其結果為`true`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2138">If either operand is NaN, the result is `false` for all operators except `!=`, for which the result is `true`.</span></span> <span data-ttu-id="abcc0-2139">對於任何兩個運算元，`x != y`一定會產生相同結果`!(x == y)`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2139">For any two operands, `x != y` always produces the same result as `!(x == y)`.</span></span> <span data-ttu-id="abcc0-2140">不過，當一或兩個運算元都是 NaN， `<`， `>`， `<=`，和`>=`運算子不會產生相反的運算子的邏輯否定與相同的結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2140">However, when one or both operands are NaN, the `<`, `>`, `<=`, and `>=` operators do not produce the same results as the logical negation of the opposite operator.</span></span> <span data-ttu-id="abcc0-2141">例如，如果有任一個的`x`並`y`是 NaN，則`x < y`是`false`，但`!(x >= y)`是`true`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2141">For example, if either of `x` and `y` is NaN, then `x < y` is `false`, but `!(x >= y)` is `true`.</span></span>
*  <span data-ttu-id="abcc0-2142">當兩個運算元是 NaN 時，運算子會比較兩個浮點運算元相對於排序的值</span><span class="sxs-lookup"><span data-stu-id="abcc0-2142">When neither operand is NaN, the operators compare the values of the two floating-point operands with respect to the ordering</span></span>

   ```
   -inf < -max < ... < -min < -0.0 == +0.0 < +min < ... < +max < +inf
   ```

   <span data-ttu-id="abcc0-2143">何處`min`和`max`會最小和最大正數有限值表示指定的浮點格式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2143">where `min` and `max` are the smallest and largest positive finite values that can be represented in the given floating-point format.</span></span> <span data-ttu-id="abcc0-2144">值得注意的這個順序的影響如下：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2144">Notable effects of this ordering are:</span></span>
   * <span data-ttu-id="abcc0-2145">負向及正零會視為相等。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2145">Negative and positive zeros are considered equal.</span></span>
   * <span data-ttu-id="abcc0-2146">負的無限值會被視為小於比所有其他值，但等於另一個負無限大。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2146">A negative infinity is considered less than all other values, but equal to another negative infinity.</span></span>
   * <span data-ttu-id="abcc0-2147">大於所有其他值，但等於另一個正無限大，則會被視為無限大的正數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2147">A positive infinity is considered greater than all other values, but equal to another positive infinity.</span></span>

### <a name="decimal-comparison-operators"></a><span data-ttu-id="abcc0-2148">十進位的比較運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-2148">Decimal comparison operators</span></span>

<span data-ttu-id="abcc0-2149">預先定義的十進位比較運算子包括︰</span><span class="sxs-lookup"><span data-stu-id="abcc0-2149">The predefined decimal comparison operators are:</span></span>
```csharp
bool operator ==(decimal x, decimal y);
bool operator !=(decimal x, decimal y);
bool operator <(decimal x, decimal y);
bool operator >(decimal x, decimal y);
bool operator <=(decimal x, decimal y);
bool operator >=(decimal x, decimal y);
```

<span data-ttu-id="abcc0-2150">每個運算子比較兩個十進位運算元和傳回數值`bool`值，指出特定的關聯性是否`true`或`false`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2150">Each of these operators compares the numeric values of the two decimal operands and returns a `bool` value that indicates whether the particular relation is `true` or `false`.</span></span> <span data-ttu-id="abcc0-2151">每個小數點的比較就相當於使用的對應關聯式或類型的等號比較運算子`System.Decimal`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2151">Each decimal comparison is equivalent to using the corresponding relational or equality operator of type `System.Decimal`.</span></span>

### <a name="boolean-equality-operators"></a><span data-ttu-id="abcc0-2152">布林值等號比較運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-2152">Boolean equality operators</span></span>

<span data-ttu-id="abcc0-2153">預先定義的布林等號比較運算子包括︰</span><span class="sxs-lookup"><span data-stu-id="abcc0-2153">The predefined boolean equality operators are:</span></span>
```csharp
bool operator ==(bool x, bool y);
bool operator !=(bool x, bool y);
```

<span data-ttu-id="abcc0-2154">結果`==`是`true`如果兩個`x`並`y`會`true`或如果兩個`x`並`y`是`false`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2154">The result of `==` is `true` if both `x` and `y` are `true` or if both `x` and `y` are `false`.</span></span> <span data-ttu-id="abcc0-2155">否則，結果為 `false`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2155">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="abcc0-2156">結果`!=`是`false`如果兩個`x`並`y`會`true`或如果兩個`x`並`y`是`false`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2156">The result of `!=` is `false` if both `x` and `y` are `true` or if both `x` and `y` are `false`.</span></span> <span data-ttu-id="abcc0-2157">否則，結果為 `true`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2157">Otherwise, the result is `true`.</span></span> <span data-ttu-id="abcc0-2158">當運算元都是型別`bool`，則`!=`運算子會產生相同結果`^`運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2158">When the operands are of type `bool`, the `!=` operator produces the same result as the `^` operator.</span></span>

### <a name="enumeration-comparison-operators"></a><span data-ttu-id="abcc0-2159">列舉型別比較運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-2159">Enumeration comparison operators</span></span>

<span data-ttu-id="abcc0-2160">每個列舉類型都會隱含地提供下列預先定義的比較運算子：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2160">Every enumeration type implicitly provides the following predefined comparison operators:</span></span>
```csharp
bool operator ==(E x, E y);
bool operator !=(E x, E y);
bool operator <(E x, E y);
bool operator >(E x, E y);
bool operator <=(E x, E y);
bool operator >=(E x, E y);
```

<span data-ttu-id="abcc0-2161">評估的結果`x op y`，其中`x`並`y`是列舉型別的運算式`E`基礎型別`U`，和`op`是其中一個比較運算子，是完全相同評估`((U)x) op ((U)y)`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2161">The result of evaluating `x op y`, where `x` and `y` are expressions of an enumeration type `E` with an underlying type `U`, and `op` is one of the comparison operators, is exactly the same as evaluating `((U)x) op ((U)y)`.</span></span> <span data-ttu-id="abcc0-2162">換句話說，列舉型別比較運算子只會比較兩個運算元的基礎整數值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2162">In other words, the enumeration type comparison operators simply compare the underlying integral values of the two operands.</span></span>

### <a name="reference-type-equality-operators"></a><span data-ttu-id="abcc0-2163">參考型別等號比較運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-2163">Reference type equality operators</span></span>

<span data-ttu-id="abcc0-2164">預先定義的參考類型的等號比較運算子包括︰</span><span class="sxs-lookup"><span data-stu-id="abcc0-2164">The predefined reference type equality operators are:</span></span>
```csharp
bool operator ==(object x, object y);
bool operator !=(object x, object y);
```

<span data-ttu-id="abcc0-2165">運算子會傳回比較兩個參考相等或不等號比較的結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2165">The operators return the result of comparing the two references for equality or non-equality.</span></span>

<span data-ttu-id="abcc0-2166">因為預先定義的參考類型的等號比較運算子接受類型的運算元`object`，它們適用於所有類型的未宣告適用`operator ==`和`operator !=`成員。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2166">Since the predefined reference type equality operators accept operands of type `object`, they apply to all types that do not declare applicable `operator ==` and `operator !=` members.</span></span> <span data-ttu-id="abcc0-2167">相反地，任何適用於使用者定義的等號比較運算子可以有效地隱藏預先定義的參考類型的等號比較運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2167">Conversely, any applicable user-defined equality operators effectively hide the predefined reference type equality operators.</span></span>

<span data-ttu-id="abcc0-2168">預先定義的參考類型的等號比較運算子需要下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2168">The predefined reference type equality operators require one of the following:</span></span>

*  <span data-ttu-id="abcc0-2169">這兩個運算元都是已知型別的值*reference_type*常值或`null`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2169">Both operands are a value of a type known to be a *reference_type* or the literal `null`.</span></span> <span data-ttu-id="abcc0-2170">此外，明確參考轉換 ([明確參考轉換](conversions.md#explicit-reference-conversions)) 將另一個運算元的型別有任一個運算元的類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2170">Furthermore, an explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) exists from the type of either operand to the type of the other operand.</span></span>
*  <span data-ttu-id="abcc0-2171">有一個運算元是值型別的`T`所在`T`是*type_parameter*另一個運算元為常值和`null`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2171">One operand is a value of type `T` where `T` is a *type_parameter* and the other operand is the literal `null`.</span></span> <span data-ttu-id="abcc0-2172">此外`T`沒有實值類型條件約束。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2172">Furthermore `T` does not have the value type constraint.</span></span>

<span data-ttu-id="abcc0-2173">除非符合下列條件之一成立，繫結階段會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2173">Unless one of these conditions are true, a binding-time error occurs.</span></span> <span data-ttu-id="abcc0-2174">值得注意的影響，這些規則是：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2174">Notable implications of these rules are:</span></span>

*  <span data-ttu-id="abcc0-2175">它是繫結時間錯誤使用預先定義的參考類型的等號比較運算子來比較兩個已知為不同繫結時間的參考。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2175">It is a binding-time error to use the predefined reference type equality operators to compare two references that are known to be different at binding-time.</span></span> <span data-ttu-id="abcc0-2176">比方說，如果繫結時間類型的運算元都是兩個類別類型`A`並`B`，和如果既未`A`也不`B`自另一個，則會是不可能的兩個運算元參考到相同的物件。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2176">For example, if the binding-time types of the operands are two class types `A` and `B`, and if neither `A` nor `B` derives from the other, then it would be impossible for the two operands to reference the same object.</span></span> <span data-ttu-id="abcc0-2177">因此，此作業會被視為一個繫結錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2177">Thus, the operation is considered a binding-time error.</span></span>
*  <span data-ttu-id="abcc0-2178">預先定義的參考類型的等號比較運算子不允許的值進行比較的型別運算元。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2178">The predefined reference type equality operators do not permit value type operands to be compared.</span></span> <span data-ttu-id="abcc0-2179">因此，除非結構類型會宣告它自己的等號比較運算子，不可能比較該結構類型的值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2179">Therefore, unless a struct type declares its own equality operators, it is not possible to compare values of that struct type.</span></span>
*  <span data-ttu-id="abcc0-2180">預先定義的參考類型的等號比較運算子永遠不會導致發生其運算元的 boxing 作業。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2180">The predefined reference type equality operators never cause boxing operations to occur for their operands.</span></span> <span data-ttu-id="abcc0-2181">它會執行這類的 boxing 作業，因為新配置已封裝的執行個體參考一定會不同於其他的所有參考意義。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2181">It would be meaningless to perform such boxing operations, since references to the newly allocated boxed instances would necessarily differ from all other references.</span></span>
*  <span data-ttu-id="abcc0-2182">如果型別參數類型的運算元`T`相較於`null`，和執行階段類型`T`實值型別比較的結果是`false`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2182">If an operand of a type parameter type `T` is compared to `null`, and the run-time type of `T` is a value type, the result of the comparison is `false`.</span></span>

<span data-ttu-id="abcc0-2183">下列範例會檢查未受限制的型別參數類型的引數是否`null`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2183">The following example checks whether an argument of an unconstrained type parameter type is `null`.</span></span>
```csharp
class C<T>
{
    void F(T x) {
        if (x == null) throw new ArgumentNullException();
        ...
    }
}
```

<span data-ttu-id="abcc0-2184">`x == null`即使允許建構`T`可能代表實值類型，以及結果只定義為`false`當`T`是實值類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2184">The `x == null` construct is permitted even though `T` could represent a value type, and the result is simply defined to be `false` when `T` is a value type.</span></span>

<span data-ttu-id="abcc0-2185">作業的表單`x == y`或`x != y`，如果任何適用`operator ==`或`operator !=`存在，運算子多載解析 ([二元運算子多載解析](expressions.md#binary-operator-overload-resolution)) 規則將會選取，而不是預先定義的參考類型的等號比較運算子的運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2185">For an operation of the form `x == y` or `x != y`, if any applicable `operator ==` or `operator !=` exists, the operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) rules will select that operator instead of the predefined reference type equality operator.</span></span> <span data-ttu-id="abcc0-2186">不過，它就一定能夠選取預先定義的參考類型的等號比較運算子，明確轉換一或兩個輸入運算元`object`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2186">However, it is always possible to select the predefined reference type equality operator by explicitly casting one or both of the operands to type `object`.</span></span> <span data-ttu-id="abcc0-2187">此範例</span><span class="sxs-lookup"><span data-stu-id="abcc0-2187">The example</span></span>
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
<span data-ttu-id="abcc0-2188">產生下列輸出</span><span class="sxs-lookup"><span data-stu-id="abcc0-2188">produces the output</span></span>
```
True
False
False
False
```

<span data-ttu-id="abcc0-2189">`s`並`t`變數參考兩個不同`string`包含相同字元的執行個體。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2189">The `s` and `t` variables refer to two distinct `string` instances containing the same characters.</span></span> <span data-ttu-id="abcc0-2190">第一個比較輸出`True`因為預先定義的字串等號比較運算子 ([字串等號比較運算子](expressions.md#string-equality-operators)) 這兩個運算元屬於型別時，會選取`string`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2190">The first comparison outputs `True` because the predefined string equality operator ([String equality operators](expressions.md#string-equality-operators)) is selected when both operands are of type `string`.</span></span> <span data-ttu-id="abcc0-2191">所有其他的比較輸出`False`因為一或兩個運算元屬於類型時，會選取預先定義的參考類型的等號比較運算子`object`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2191">The remaining comparisons all output `False` because the predefined reference type equality operator is selected when one or both of the operands are of type `object`.</span></span>

<span data-ttu-id="abcc0-2192">請注意，上述的技巧並不會對實值型別有意義。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2192">Note that the above technique is not meaningful for value types.</span></span> <span data-ttu-id="abcc0-2193">此範例</span><span class="sxs-lookup"><span data-stu-id="abcc0-2193">The example</span></span>
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
<span data-ttu-id="abcc0-2194">輸出`False`因為轉換會建立兩個不同的執行個體的參考進行 boxed 處理`int`值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2194">outputs `False` because the casts create references to two separate instances of boxed `int` values.</span></span>

### <a name="string-equality-operators"></a><span data-ttu-id="abcc0-2195">字串等號比較運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-2195">String equality operators</span></span>

<span data-ttu-id="abcc0-2196">預先定義的字串等號比較運算子包括︰</span><span class="sxs-lookup"><span data-stu-id="abcc0-2196">The predefined string equality operators are:</span></span>
```csharp
bool operator ==(string x, string y);
bool operator !=(string x, string y);
```

<span data-ttu-id="abcc0-2197">兩個`string`值都視為相等，當下列其中一項條件成立：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2197">Two `string` values are considered equal when one of the following is true:</span></span>

*  <span data-ttu-id="abcc0-2198">這兩個值是`null`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2198">Both values are `null`.</span></span>
*  <span data-ttu-id="abcc0-2199">這兩個值是在每個字元的位置中有相同的長度，以及完全相同字元的字串執行個體的非 null 參考。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2199">Both values are non-null references to string instances that have identical lengths and identical characters in each character position.</span></span>

<span data-ttu-id="abcc0-2200">字串等號比較運算子比較字串值，而不是字串的參考。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2200">The string equality operators compare string values rather than string references.</span></span> <span data-ttu-id="abcc0-2201">當兩個不同的字串執行個體包含完全相同的字元序列時，字串的值相等，但參考不同。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2201">When two separate string instances contain the exact same sequence of characters, the values of the strings are equal, but the references are different.</span></span> <span data-ttu-id="abcc0-2202">中所述[參考型別等號比較運算子](expressions.md#reference-type-equality-operators)，參考類型的等號比較運算子可用來比較字串的參考，而不是字串值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2202">As described in [Reference type equality operators](expressions.md#reference-type-equality-operators), the reference type equality operators can be used to compare string references instead of string values.</span></span>

### <a name="delegate-equality-operators"></a><span data-ttu-id="abcc0-2203">委派等號比較運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-2203">Delegate equality operators</span></span>

<span data-ttu-id="abcc0-2204">每個委派類型以隱含方式提供下列預先定義的比較運算子：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2204">Every delegate type implicitly provides the following predefined comparison operators:</span></span>

```csharp
bool operator ==(System.Delegate x, System.Delegate y);
bool operator !=(System.Delegate x, System.Delegate y);
```

<span data-ttu-id="abcc0-2205">兩個委派執行個體視為相等，如下所示：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2205">Two delegate instances are considered equal as follows:</span></span>

*  <span data-ttu-id="abcc0-2206">如果委派執行個體的任何一種`null`，它們是否相等的如果且只有兩者都是`null`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2206">If either of the delegate instances is `null`, they are equal if and only if both are `null`.</span></span>
*  <span data-ttu-id="abcc0-2207">如果委派具有不同的執行階段型別就是永遠不會相等。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2207">If the delegates have different run-time type they are never equal.</span></span>
*  <span data-ttu-id="abcc0-2208">如果這兩個委派執行個體具有引動過程清單 ([委派宣告](delegates.md#delegate-declarations))，這些執行個體相等，如果且只有其引動過程清單都有相同的長度，並在其中的引動過程清單中的每個項目是否相等 （如下列所定義）對應中的項目，其他的引動過程清單中的順序。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2208">If both of the delegate instances have an invocation list ([Delegate declarations](delegates.md#delegate-declarations)), those instances are equal if and only if their invocation lists are the same length, and each entry in one's invocation list is equal (as defined below) to the corresponding entry, in order, in the other's invocation list.</span></span>

<span data-ttu-id="abcc0-2209">下列規則可管理引動過程清單項目的相等：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2209">The following rules govern the equality of invocation list entries:</span></span>

*  <span data-ttu-id="abcc0-2210">如果兩個引動過程清單項目這兩個參考相同的靜態方法，則項目相等。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2210">If two invocation list entries both refer to the same static method then the entries are equal.</span></span>
*  <span data-ttu-id="abcc0-2211">如果兩個引動過程清單項目這兩個參考相同的非靜態方法，在相同的目標物件上 （如參考等號比較運算子所定義） 項目相等。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2211">If two invocation list entries both refer to the same non-static method on the same target object (as defined by the reference equality operators) then the entries are equal.</span></span>
*  <span data-ttu-id="abcc0-2212">引動過程清單項目所產生的相同語意的評估*anonymous_method_expression*s 或*lambda_expression*與相同 （可能是空的） 擷取外部變數的集合執行個體所允許 （但非必要） 才會相等。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2212">Invocation list entries produced from evaluation of semantically identical *anonymous_method_expression*s or *lambda_expression*s with the same (possibly empty) set of captured outer variable instances are permitted (but not required) to be equal.</span></span>

### <a name="equality-operators-and-null"></a><span data-ttu-id="abcc0-2213">等號比較運算子和 null</span><span class="sxs-lookup"><span data-stu-id="abcc0-2213">Equality operators and null</span></span>

<span data-ttu-id="abcc0-2214">`==`並`!=`運算子允許一個運算元是值為 null 的型別和其他為`null`常值，即使沒有預先定義或使用者定義的運算子 （使用或提昇的形式） 存在的作業。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2214">The `==` and `!=` operators permit one operand to be a value of a nullable type and the other to be the `null` literal, even if no predefined or user-defined operator (in unlifted or lifted form) exists for the operation.</span></span>

<span data-ttu-id="abcc0-2215">其中一種形式的作業</span><span class="sxs-lookup"><span data-stu-id="abcc0-2215">For an operation of one of the forms</span></span>
```csharp
x == null
null == x
x != null
null != x
```
<span data-ttu-id="abcc0-2216">何處`x`類型的運算式可為 null，則運算子多載解析 ([二元運算子多載解析](expressions.md#binary-operator-overload-resolution)) 尋找適用的運算子，結果就無法改為從計算`HasValue`屬性`x`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2216">where `x` is an expression of a nullable type, if operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) fails to find an applicable operator, the result is instead computed from the `HasValue` property of `x`.</span></span> <span data-ttu-id="abcc0-2217">具體來說前, 兩個形式會轉譯成`!x.HasValue`，以及最後兩種形式會轉譯成`x.HasValue`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2217">Specifically, the first two forms are translated into `!x.HasValue`, and last two forms are translated into `x.HasValue`.</span></span>

### <a name="the-is-operator"></a><span data-ttu-id="abcc0-2218">是運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-2218">The is operator</span></span>

<span data-ttu-id="abcc0-2219">`is`運算子用來以動態方式檢查 執行階段類型的物件是否與指定的型別相容。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2219">The `is` operator is used to dynamically check if the run-time type of an object is compatible with a given type.</span></span> <span data-ttu-id="abcc0-2220">作業的結果`E is T`，其中`E`是運算式和`T`是類型為布林值，這個值表示是否`E`成功轉換為類型`T`透過參考轉換、 boxing轉換或 unboxing 轉換。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2220">The result of the operation `E is T`, where `E` is an expression and `T` is a type, is a boolean value indicating whether `E` can successfully be converted to type `T` by a reference conversion, a boxing conversion, or an unboxing conversion.</span></span> <span data-ttu-id="abcc0-2221">作業會在之後的所有型別參數已取代型別引數，如下所示，評估：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2221">The operation is evaluated as follows, after type arguments have been substituted for all type parameters:</span></span>

*  <span data-ttu-id="abcc0-2222">如果`E`是匿名函式，就會發生編譯時期錯誤</span><span class="sxs-lookup"><span data-stu-id="abcc0-2222">If `E` is an anonymous function, a compile-time error occurs</span></span>
*  <span data-ttu-id="abcc0-2223">如果`E`是方法群組或`null`常值，if 的型別`E`是參考型別或可為 null 的型別和值的`E`是 null，結果為 false。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2223">If `E` is a method group or the `null` literal, of if the type of `E` is a reference type or a nullable type and the value of `E` is null, the result is false.</span></span>
*  <span data-ttu-id="abcc0-2224">否則，讓`D`代表的動態類型`E`，如下所示：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2224">Otherwise, let `D` represent the dynamic type of `E` as follows:</span></span>
   * <span data-ttu-id="abcc0-2225">如果類型`E`是參考型別`D`之執行個體參考的執行階段類型`E`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2225">If the type of `E` is a reference type, `D` is the run-time type of the instance reference by `E`.</span></span>
   * <span data-ttu-id="abcc0-2226">如果類型`E`是可為 null 的型別，`D`是該可為 null 的型別的基礎型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2226">If the type of `E` is a nullable type, `D` is the underlying type of that nullable type.</span></span>
   * <span data-ttu-id="abcc0-2227">如果類型`E`不可為 null 的實值型別，`D`是種`E`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2227">If the type of `E` is a non-nullable value type, `D` is the type of `E`.</span></span>
*  <span data-ttu-id="abcc0-2228">作業的結果會取決於`D`和`T`，如下所示：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2228">The result of the operation depends on `D` and `T` as follows:</span></span>
   * <span data-ttu-id="abcc0-2229">如果`T`是參考型別，結果為 true 如果`D`並`T`是相同的型別，如果`D`是參考類型，從隱含參考轉換`D`來`T`存在，或如果`D`是實值類型和 boxing 轉換`D`至`T`存在。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2229">If `T` is a reference type, the result is true if `D` and `T` are the same type, if `D` is a reference type and an implicit reference conversion from `D` to `T` exists, or if `D` is a value type and a boxing conversion from `D` to `T` exists.</span></span>
   * <span data-ttu-id="abcc0-2230">如果`T`是可為 null 的型別，結果為 true 如果`D`則是基礎型別`T`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2230">If `T` is a nullable type, the result is true if `D` is the underlying type of `T`.</span></span>
   * <span data-ttu-id="abcc0-2231">如果`T`不可為 null 的實值型別，則結果為 true 如果`D`和`T`都是相同的型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2231">If `T` is a non-nullable value type, the result is true if `D` and `T` are the same type.</span></span>
   * <span data-ttu-id="abcc0-2232">否則，結果就是 false。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2232">Otherwise, the result is false.</span></span>

<span data-ttu-id="abcc0-2233">請注意，使用者定義轉換，不會考慮`is`運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2233">Note that user defined conversions, are not considered by the `is` operator.</span></span>

### <a name="the-as-operator"></a><span data-ttu-id="abcc0-2234">As 運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-2234">The as operator</span></span>

<span data-ttu-id="abcc0-2235">`as`運算子用來明確地將值轉換為指定的參考型別或可為 null 的型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2235">The `as` operator is used to explicitly convert a value to a given reference type or nullable type.</span></span> <span data-ttu-id="abcc0-2236">不同於轉型運算式 ([轉型運算式](expressions.md#cast-expressions))，則`as`運算子絕不會擲回的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2236">Unlike a cast expression ([Cast expressions](expressions.md#cast-expressions)), the `as` operator never throws an exception.</span></span> <span data-ttu-id="abcc0-2237">相反地，如果指定的轉換不可行，產生的值是`null`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2237">Instead, if the indicated conversion is not possible, the resulting value is `null`.</span></span>

<span data-ttu-id="abcc0-2238">在作業中的表單`E as T`，`E`必須是運算式和`T`必須是參考型別，已知為參考類型或可為 null 類型的型別參數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2238">In an operation of the form `E as T`, `E` must be an expression and `T` must be a reference type, a type parameter known to be a reference type, or a nullable type.</span></span> <span data-ttu-id="abcc0-2239">此外，至少一個下列必須為 true，或否則會發生編譯時期錯誤：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2239">Furthermore, at least one of the following must be true, or otherwise a compile-time error occurs:</span></span>

*  <span data-ttu-id="abcc0-2240">身分識別 ([身分識別轉換](conversions.md#identity-conversion))、 隱含可為 null ([可為 null 的隱含轉換](conversions.md#implicit-nullable-conversions))，隱含參考 ([隱含參考轉換](conversions.md#implicit-reference-conversions))，boxing ([Boxing 轉換](conversions.md#boxing-conversions))、 明確可為 null ([明確可為 null 的轉換](conversions.md#explicit-nullable-conversions))，明確參考 ([明確參考轉換](conversions.md#explicit-reference-conversions))，或 unboxing ([Unboxing 轉換](conversions.md#unboxing-conversions)) 轉換存在`E`至`T`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2240">An identity ([Identity conversion](conversions.md#identity-conversion)), implicit nullable ([Implicit nullable conversions](conversions.md#implicit-nullable-conversions)), implicit reference ([Implicit reference conversions](conversions.md#implicit-reference-conversions)), boxing ([Boxing conversions](conversions.md#boxing-conversions)), explicit nullable ([Explicit nullable conversions](conversions.md#explicit-nullable-conversions)), explicit reference ([Explicit reference conversions](conversions.md#explicit-reference-conversions)), or unboxing ([Unboxing conversions](conversions.md#unboxing-conversions)) conversion exists from `E` to `T`.</span></span>
*  <span data-ttu-id="abcc0-2241">型別`E`或`T`是開啟型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2241">The type of `E` or `T` is an open type.</span></span>
*  <span data-ttu-id="abcc0-2242">`E` 是`null`常值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2242">`E` is the `null` literal.</span></span>

<span data-ttu-id="abcc0-2243">編譯時間類型，是否`E`不是`dynamic`，此作業`E as T`產生相同結果</span><span class="sxs-lookup"><span data-stu-id="abcc0-2243">If the compile-time type of `E` is not `dynamic`, the operation `E as T` produces the same result as</span></span>
```csharp
E is T ? (T)(E) : (T)null
```
<span data-ttu-id="abcc0-2244">但只會評估 `E` 一次。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2244">except that `E` is only evaluated once.</span></span> <span data-ttu-id="abcc0-2245">可預期的編譯器最佳化`E as T`執行最多一個動態型別檢查而不是上述擴充所隱含的兩個動態型別檢查。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2245">The compiler can be expected to optimize `E as T` to perform at most one dynamic type check as opposed to the two dynamic type checks implied by the expansion above.</span></span>

<span data-ttu-id="abcc0-2246">編譯時間類型，是否`E`已`dynamic`，不像轉型運算子`as`運算子沒有動態繫結 ([動態繫結](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2246">If the compile-time type of `E` is `dynamic`, unlike the cast operator the `as` operator is not dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="abcc0-2247">因此擴充在此情況下是：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2247">Therefore the expansion in this case is:</span></span>
```csharp
E is T ? (T)(object)(E) : (T)null
```

<span data-ttu-id="abcc0-2248">請注意，某些轉換，例如使用者定義轉換，即無法在`as`運算子，而且應該改為執行使用轉型運算式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2248">Note that some conversions, such as user defined conversions, are not possible with the `as` operator and should instead be performed using cast expressions.</span></span>

<span data-ttu-id="abcc0-2249">在範例</span><span class="sxs-lookup"><span data-stu-id="abcc0-2249">In the example</span></span>
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
<span data-ttu-id="abcc0-2250">型別參數`T`的`G`已知為參考類型，因為它具有類別條件約束。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2250">the type parameter `T` of `G` is known to be a reference type, because it has the class constraint.</span></span> <span data-ttu-id="abcc0-2251">型別參數`U`的`H`不是不過，因此使用`as`中的運算子`H`不被允許。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2251">The type parameter `U` of `H` is not however; hence the use of the `as` operator in `H` is disallowed.</span></span>

## <a name="logical-operators"></a><span data-ttu-id="abcc0-2252">邏輯運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-2252">Logical operators</span></span>

<span data-ttu-id="abcc0-2253">`&`， `^`，和`|`運算子都稱為邏輯運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2253">The `&`, `^`, and `|` operators are called the logical operators.</span></span>

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

<span data-ttu-id="abcc0-2254">邏輯運算子的運算元是否在編譯時期型別`dynamic`，然後動態地繫結運算式 ([動態繫結](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2254">If an operand of a logical operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="abcc0-2255">運算式的編譯時期型別是在此情況下`dynamic`，如下所述的解析度會發生在執行階段使用這些已編譯時間類型的運算元的執行階段型別和`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2255">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="abcc0-2256">作業的表單`x op y`，其中`op`是其中一個邏輯運算子，也就是多載解析 ([二元運算子多載解析](expressions.md#binary-operator-overload-resolution)) 套用至選取的特定運算子實作。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2256">For an operation of the form `x op y`, where `op` is one of the logical operators, overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="abcc0-2257">運算元會轉換成所選的運算子的參數類型和結果的類型是運算子的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2257">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="abcc0-2258">預先定義的邏輯運算子會由下列各節所述。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2258">The predefined logical operators are described in the following sections.</span></span>

### <a name="integer-logical-operators"></a><span data-ttu-id="abcc0-2259">整數的邏輯運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-2259">Integer logical operators</span></span>

<span data-ttu-id="abcc0-2260">預先定義之的整數的邏輯運算子包括︰</span><span class="sxs-lookup"><span data-stu-id="abcc0-2260">The predefined integer logical operators are:</span></span>
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

<span data-ttu-id="abcc0-2261">`&`運算子會計算位元邏輯`AND`兩個運算元`|`運算子會計算位元邏輯`OR`兩個運算元和`^`運算子計算位元邏輯互斥`OR`兩個運算元。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2261">The `&` operator computes the bitwise logical `AND` of the two operands, the `|` operator computes the bitwise logical `OR` of the two operands, and the `^` operator computes the bitwise logical exclusive `OR` of the two operands.</span></span> <span data-ttu-id="abcc0-2262">從這些作業可能不會有任何溢位。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2262">No overflows are possible from these operations.</span></span>

### <a name="enumeration-logical-operators"></a><span data-ttu-id="abcc0-2263">列舉邏輯運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-2263">Enumeration logical operators</span></span>

<span data-ttu-id="abcc0-2264">每個列舉類型`E`都會隱含地提供下列預先定義的邏輯運算子：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2264">Every enumeration type `E` implicitly provides the following predefined logical operators:</span></span>

```csharp
E operator &(E x, E y);
E operator |(E x, E y);
E operator ^(E x, E y);
```

<span data-ttu-id="abcc0-2265">評估的結果`x op y`，其中`x`並`y`是列舉型別的運算式`E`基礎型別`U`，和`op`是其中一個邏輯運算子，是完全相同評估`(E)((U)x op (U)y)`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2265">The result of evaluating `x op y`, where `x` and `y` are expressions of an enumeration type `E` with an underlying type `U`, and `op` is one of the logical operators, is exactly the same as evaluating `(E)((U)x op (U)y)`.</span></span> <span data-ttu-id="abcc0-2266">換句話說，列舉型別邏輯運算子只會執行之基礎類型的兩個運算元的邏輯作業。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2266">In other words, the enumeration type logical operators simply perform the logical operation on the underlying type of the two operands.</span></span>

### <a name="boolean-logical-operators"></a><span data-ttu-id="abcc0-2267">布林值邏輯運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-2267">Boolean logical operators</span></span>

<span data-ttu-id="abcc0-2268">預先定義的布林邏輯運算子包括︰</span><span class="sxs-lookup"><span data-stu-id="abcc0-2268">The predefined boolean logical operators are:</span></span>
```csharp
bool operator &(bool x, bool y);
bool operator |(bool x, bool y);
bool operator ^(bool x, bool y);
```

<span data-ttu-id="abcc0-2269">若 `x` 及 `y` 皆為 `true`，那麼 `x & y` 的結果會是 `true`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2269">The result of `x & y` is `true` if both `x` and `y` are `true`.</span></span> <span data-ttu-id="abcc0-2270">否則，結果為 `false`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2270">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="abcc0-2271">結果`x | y`已`true`如果`x`或是`y`是`true`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2271">The result of `x | y` is `true` if either `x` or `y` is `true`.</span></span> <span data-ttu-id="abcc0-2272">否則，結果為 `false`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2272">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="abcc0-2273">結果`x ^ y`是`true`如果`x`是`true`並`y`是`false`，或`x`是`false`並`y`是`true`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2273">The result of `x ^ y` is `true` if `x` is `true` and `y` is `false`, or `x` is `false` and `y` is `true`.</span></span> <span data-ttu-id="abcc0-2274">否則，結果為 `false`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2274">Otherwise, the result is `false`.</span></span> <span data-ttu-id="abcc0-2275">當運算元都是型別`bool`，則`^`運算子會計算為相同的結果`!=`運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2275">When the operands are of type `bool`, the `^` operator computes the same result as the `!=` operator.</span></span>

### <a name="nullable-boolean-logical-operators"></a><span data-ttu-id="abcc0-2276">可為 null 的布林邏輯運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-2276">Nullable boolean logical operators</span></span>

<span data-ttu-id="abcc0-2277">可為 null 的布林型別`bool?`可以代表三個值， `true`， `false`，和`null`，並在概念上類似 SQL 中的布林值運算式使用三種值的類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2277">The nullable boolean type `bool?` can represent three values, `true`, `false`, and `null`, and is conceptually similar to the three-valued type used for boolean expressions in SQL.</span></span> <span data-ttu-id="abcc0-2278">所產生的結果時，確保`&`並`|`運算子`bool?`運算元都與 SQL 的三個值的邏輯一致的會提供下列預先定義的運算子：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2278">To ensure that the results produced by the `&` and `|` operators for `bool?` operands are consistent with SQL's three-valued logic, the following predefined operators are provided:</span></span>

```csharp
bool? operator &(bool? x, bool? y);
bool? operator |(bool? x, bool? y);
```

<span data-ttu-id="abcc0-2279">下表列出這些運算子的所有值的組合所產生的結果`true`， `false`，和`null`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2279">The following table lists the results produced by these operators for all combinations of the values `true`, `false`, and `null`.</span></span>

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

## <a name="conditional-logical-operators"></a><span data-ttu-id="abcc0-2280">條件邏輯運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-2280">Conditional logical operators</span></span>

<span data-ttu-id="abcc0-2281">`&&`和`||`運算子稱為條件邏輯運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2281">The `&&` and `||` operators are called the conditional logical operators.</span></span> <span data-ttu-id="abcc0-2282">它們也稱為 「 最少運算 」 的邏輯運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2282">They are also called the "short-circuiting" logical operators.</span></span>

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

<span data-ttu-id="abcc0-2283">`&&`並`||`是條件式版本運算子`&`和`|`運算子：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2283">The `&&` and `||` operators are conditional versions of the `&` and `|` operators:</span></span>

*  <span data-ttu-id="abcc0-2284">操作`x && y`對應至作業`x & y`，差異在於`y`才會求`x`不是`false`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2284">The operation `x && y` corresponds to the operation `x & y`, except that `y` is evaluated only if `x` is not `false`.</span></span>
*  <span data-ttu-id="abcc0-2285">操作`x || y`對應至作業`x | y`，差異在於`y`才會求`x`不是`true`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2285">The operation `x || y` corresponds to the operation `x | y`, except that `y` is evaluated only if `x` is not `true`.</span></span>

<span data-ttu-id="abcc0-2286">條件式邏輯運算子的運算元是否在編譯時期型別`dynamic`，然後動態地繫結運算式 ([動態繫結](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2286">If an operand of a conditional logical operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="abcc0-2287">運算式的編譯時期型別是在此情況下`dynamic`，如下所述的解析度會發生在執行階段使用這些已編譯時間類型的運算元的執行階段型別和`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2287">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="abcc0-2288">表單的作業`x && y`或`x || y`處理藉由套用多載解析 ([二元運算子多載解析](expressions.md#binary-operator-overload-resolution))，如果作業寫入`x & y`或`x | y`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2288">An operation of the form `x && y` or `x || y` is processed by applying overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) as if the operation was written `x & y` or `x | y`.</span></span> <span data-ttu-id="abcc0-2289">然後，</span><span class="sxs-lookup"><span data-stu-id="abcc0-2289">Then,</span></span>

*  <span data-ttu-id="abcc0-2290">如果多載解析失敗，若要尋找單一最佳的運算子，或如果多載解析會選取其中一個預先定義之的整數的邏輯運算子，就會發生繫結時間錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2290">If overload resolution fails to find a single best operator, or if overload resolution selects one of the predefined integer logical operators, a binding-time error occurs.</span></span>
*  <span data-ttu-id="abcc0-2291">否則，如果選取的運算子是其中一個預先定義的布林邏輯運算子 ([布林值的邏輯運算子](expressions.md#boolean-logical-operators)) 或可為 null 的布林邏輯運算子 ([可為 Null 的布林邏輯運算子](expressions.md#nullable-boolean-logical-operators))，作業處理中所述[，則為 True 的條件式邏輯運算子](expressions.md#boolean-conditional-logical-operators)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2291">Otherwise, if the selected operator is one of the predefined boolean logical operators ([Boolean logical operators](expressions.md#boolean-logical-operators)) or nullable boolean logical operators ([Nullable boolean logical operators](expressions.md#nullable-boolean-logical-operators)), the operation is processed as described in [Boolean conditional logical operators](expressions.md#boolean-conditional-logical-operators).</span></span>
*  <span data-ttu-id="abcc0-2292">否則，所選的運算子是使用者定義的運算子，而且則會處理作業中所述[使用者定義的條件式邏輯運算子](expressions.md#user-defined-conditional-logical-operators)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2292">Otherwise, the selected operator is a user-defined operator, and the operation is processed as described in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators).</span></span>

<span data-ttu-id="abcc0-2293">您不可能直接多載條件邏輯運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2293">It is not possible to directly overload the conditional logical operators.</span></span> <span data-ttu-id="abcc0-2294">但是，因為條件式邏輯運算子會評估規則的邏輯運算子方面，多載一般邏輯運算子，具有某些限制，也可以視為條件式邏輯運算子的多載。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2294">However, because the conditional logical operators are evaluated in terms of the regular logical operators, overloads of the regular logical operators are, with certain restrictions, also considered overloads of the conditional logical operators.</span></span> <span data-ttu-id="abcc0-2295">這是更進一步的說明[使用者定義的條件式邏輯運算子](expressions.md#user-defined-conditional-logical-operators)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2295">This is described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators).</span></span>

### <a name="boolean-conditional-logical-operators"></a><span data-ttu-id="abcc0-2296">布林值的條件式邏輯運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-2296">Boolean conditional logical operators</span></span>

<span data-ttu-id="abcc0-2297">時的運算元`&&`或`||`的型別`bool`，或當運算元都是未定義在適用的型別`operator &`或`operator |`，但定義隱含地轉換成`bool`，作業處理方式，如下所示：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2297">When the operands of `&&` or `||` are of type `bool`, or when the operands are of types that do not define an applicable `operator &` or `operator |`, but do define implicit conversions to `bool`, the operation is processed as follows:</span></span>

*  <span data-ttu-id="abcc0-2298">操作`x && y`評估為`x ? y : false`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2298">The operation `x && y` is evaluated as `x ? y : false`.</span></span> <span data-ttu-id="abcc0-2299">亦即`x`第一次評估，以及轉換成型別`bool`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2299">In other words, `x` is first evaluated and converted to type `bool`.</span></span> <span data-ttu-id="abcc0-2300">然後，如果`x`是`true`，`y`評估，以及轉換成型別`bool`，並成為運算的結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2300">Then, if `x` is `true`, `y` is evaluated and converted to type `bool`, and this becomes the result of the operation.</span></span> <span data-ttu-id="abcc0-2301">否則，作業的結果就是`false`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2301">Otherwise, the result of the operation is `false`.</span></span>
*  <span data-ttu-id="abcc0-2302">操作`x || y`評估為`x ? true : y`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2302">The operation `x || y` is evaluated as `x ? true : y`.</span></span> <span data-ttu-id="abcc0-2303">亦即`x`第一次評估，以及轉換成型別`bool`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2303">In other words, `x` is first evaluated and converted to type `bool`.</span></span> <span data-ttu-id="abcc0-2304">然後，如果`x`是`true`，此作業的結果是`true`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2304">Then, if `x` is `true`, the result of the operation is `true`.</span></span> <span data-ttu-id="abcc0-2305">否則，請`y`評估，以及轉換成型別`bool`，並成為運算的結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2305">Otherwise, `y` is evaluated and converted to type `bool`, and this becomes the result of the operation.</span></span>

### <a name="user-defined-conditional-logical-operators"></a><span data-ttu-id="abcc0-2306">使用者定義的條件式邏輯運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-2306">User-defined conditional logical operators</span></span>

<span data-ttu-id="abcc0-2307">當的運算元`&&`或`||`由宣告適用之型別的使用者定義`operator &`或`operator |`，這兩個項目必須為 true，其中`T`是在宣告選取的運算子的類型：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2307">When the operands of `&&` or `||` are of types that declare an applicable user-defined `operator &` or `operator |`, both of the following must be true, where `T` is the type in which the selected operator is declared:</span></span>

*  <span data-ttu-id="abcc0-2308">傳回的型別和所選運算子的每個參數的型別必須是`T`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2308">The return type and the type of each parameter of the selected operator must be `T`.</span></span> <span data-ttu-id="abcc0-2309">換句話說，操作員必須計算邏輯`AND`或邏輯`OR`兩個運算元的型別`T`，而且必須傳回類型之結果的`T`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2309">In other words, the operator must compute the logical `AND` or the logical `OR` of two operands of type `T`, and must return a result of type `T`.</span></span>
*  <span data-ttu-id="abcc0-2310">`T` 必須包含的宣告`operator true`和`operator false`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2310">`T` must contain declarations of `operator true` and `operator false`.</span></span>

<span data-ttu-id="abcc0-2311">如果不符合任一需求，就會發生的繫結階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2311">A binding-time error occurs if either of these requirements is not satisfied.</span></span> <span data-ttu-id="abcc0-2312">否則，請`&&`或`||`作業會藉由結合使用者定義評估`operator true`或`operator false`與選取的使用者定義運算子：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2312">Otherwise, the `&&` or `||` operation is evaluated by combining the user-defined `operator true` or `operator false` with the selected user-defined operator:</span></span>

*  <span data-ttu-id="abcc0-2313">作業`x && y`評估為`T.false(x) ? x : T.&(x, y)`，其中`T.false(x)`是的引動過程`operator false`中所宣告`T`，和`T.&(x, y)`是所選取的引動過程`operator &`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2313">The operation `x && y` is evaluated as `T.false(x) ? x : T.&(x, y)`, where `T.false(x)` is an invocation of the `operator false` declared in `T`, and `T.&(x, y)` is an invocation of the selected `operator &`.</span></span> <span data-ttu-id="abcc0-2314">也就是說，`x`就會先評估並`operator false`來判斷結果上叫用`x`是明確地 false。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2314">In other words, `x` is first evaluated and `operator false` is invoked on the result to determine if `x` is definitely false.</span></span> <span data-ttu-id="abcc0-2315">然後，如果`x`明確地為 false，作業的結果就是先前計算的值是`x`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2315">Then, if `x` is definitely false, the result of the operation is the value previously computed for `x`.</span></span> <span data-ttu-id="abcc0-2316">否則`y`評估時，與所選取`operator &`先前的計算值上叫用`x`和用於計算的值`y`產生作業的結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2316">Otherwise, `y` is evaluated, and the selected `operator &` is invoked on the value previously computed for `x` and the value computed for `y` to produce the result of the operation.</span></span>
*  <span data-ttu-id="abcc0-2317">作業`x || y`評估為`T.true(x) ? x : T.|(x, y)`，其中`T.true(x)`是的引動過程`operator true`中所宣告`T`，和`T.|(x,y)`是所選取的引動過程`operator|`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2317">The operation `x || y` is evaluated as `T.true(x) ? x : T.|(x, y)`, where `T.true(x)` is an invocation of the `operator true` declared in `T`, and `T.|(x,y)` is an invocation of the selected `operator|`.</span></span> <span data-ttu-id="abcc0-2318">換句話說，`x`就會先評估並`operator true`來判斷結果上叫用`x`為絕對，則為 true。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2318">In other words, `x` is first evaluated and `operator true` is invoked on the result to determine if `x` is definitely true.</span></span> <span data-ttu-id="abcc0-2319">然後，如果`x`為絕對，則為 true，此作業的結果是先前的計算值`x`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2319">Then, if `x` is definitely true, the result of the operation is the value previously computed for `x`.</span></span> <span data-ttu-id="abcc0-2320">否則`y`評估時，與所選取`operator |`先前的計算值上叫用`x`和用於計算的值`y`產生作業的結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2320">Otherwise, `y` is evaluated, and the selected `operator |` is invoked on the value previously computed for `x` and the value computed for `y` to produce the result of the operation.</span></span>

<span data-ttu-id="abcc0-2321">在這些作業時，所指定的運算式`x`是只評估一次，而所指定的運算式`y`無法評估或評估一次。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2321">In either of these operations, the expression given by `x` is only evaluated once, and the expression given by `y` is either not evaluated or evaluated exactly once.</span></span>

<span data-ttu-id="abcc0-2322">如需範例實作的型別`operator true`並`operator false`，請參閱[資料庫，則為 true 的型別](structs.md#database-boolean-type)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2322">For an example of a type that implements `operator true` and `operator false`, see [Database boolean type](structs.md#database-boolean-type).</span></span>

## <a name="the-null-coalescing-operator"></a><span data-ttu-id="abcc0-2323">Null 聯合運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-2323">The null coalescing operator</span></span>

<span data-ttu-id="abcc0-2324">`??`運算子稱為 null 聯合運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2324">The `??` operator is called the null coalescing operator.</span></span>

```antlr
null_coalescing_expression
    : conditional_or_expression
    | conditional_or_expression '??' null_coalescing_expression
    ;
```

<span data-ttu-id="abcc0-2325">格式的 null 聯合運算式`a ?? b`需要`a`可為 null 的型別或參考類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2325">A null coalescing expression of the form `a ?? b` requires `a` to be of a nullable type or reference type.</span></span> <span data-ttu-id="abcc0-2326">如果`a`為非 null 的結果`a ?? b`是`a`; 否則結果是`b`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2326">If `a` is non-null, the result of `a ?? b` is `a`; otherwise, the result is `b`.</span></span> <span data-ttu-id="abcc0-2327">此作業會評估`b`只有當`a`為 null。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2327">The operation evaluates `b` only if `a` is null.</span></span>

<span data-ttu-id="abcc0-2328">Null 聯合運算子是右向關聯，表示由右至左的分組作業。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2328">The null coalescing operator is right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="abcc0-2329">例如，以下格式的運算式`a ?? b ?? c`評估為`a ?? (b ?? c)`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2329">For example, an expression of the form `a ?? b ?? c` is evaluated as `a ?? (b ?? c)`.</span></span> <span data-ttu-id="abcc0-2330">一般情況下搜尋文字格式的運算式`E1 ?? E2 ?? ... ?? En`會傳回運算元的第一個非 null，或如果所有運算元都是 null 時則為 null。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2330">In general terms, an expression of the form `E1 ?? E2 ?? ... ?? En` returns the first of the operands that is non-null, or null if all operands are null.</span></span>

<span data-ttu-id="abcc0-2331">運算式的型別`a ?? b`取決於在運算元上可用的隱含轉換。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2331">The type of the expression `a ?? b` depends on which implicit conversions are available on the operands.</span></span> <span data-ttu-id="abcc0-2332">中依偏好程度的型別`a ?? b`是`A0`， `A`，或`B`，其中`A`是種`a`(前提是`a`具有型別)，`B`是種`b`(前提`b`具有型別)，並`A0`是基礎類型`A`如果`A`是可為 null 的型別，或`A`否則。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2332">In order of preference, the type of `a ?? b` is `A0`, `A`, or `B`, where `A` is the type of `a` (provided that `a` has a type), `B` is the type of `b` (provided that `b` has a type), and `A0` is the underlying type of `A` if `A` is a nullable type, or `A` otherwise.</span></span> <span data-ttu-id="abcc0-2333">具體來說，`a ?? b`處理，如下所示：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2333">Specifically, `a ?? b` is processed as follows:</span></span>

*  <span data-ttu-id="abcc0-2334">如果`A`存在，而且不是 null 的型別或參考類型，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2334">If `A` exists and is not a nullable type or a reference type, a compile-time error occurs.</span></span>
*  <span data-ttu-id="abcc0-2335">如果`b`是動態的運算式，結果型別是`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2335">If `b` is a dynamic expression, the result type is `dynamic`.</span></span> <span data-ttu-id="abcc0-2336">在執行階段`a`先進行評估。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2336">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="abcc0-2337">如果`a`不是 null，`a`轉換為動態，並成為結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2337">If `a` is not null, `a` is converted to dynamic, and this becomes the result.</span></span> <span data-ttu-id="abcc0-2338">否則，`b`評估時，這就成為結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2338">Otherwise, `b` is evaluated, and this becomes the result.</span></span>
*  <span data-ttu-id="abcc0-2339">否則，如果`A`存在於可為 null 的型別且隱含的轉換`b`要`A0`，結果型別是`A0`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2339">Otherwise, if `A` exists and is a nullable type and an implicit conversion exists from `b` to `A0`, the result type is `A0`.</span></span> <span data-ttu-id="abcc0-2340">在執行階段`a`先進行評估。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2340">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="abcc0-2341">如果`a`不是 null，`a`會解除包裝以輸入`A0`，而這會成為結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2341">If `a` is not null, `a` is unwrapped to type `A0`, and this becomes the result.</span></span> <span data-ttu-id="abcc0-2342">否則，請`b`評估，以及轉換為類型`A0`，而這會變成結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2342">Otherwise, `b` is evaluated and converted to type `A0`, and this becomes the result.</span></span>
*  <span data-ttu-id="abcc0-2343">否則，如果`A`存在且隱含的轉換從`b`要`A`，結果型別是`A`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2343">Otherwise, if `A` exists and an implicit conversion exists from `b` to `A`, the result type is `A`.</span></span> <span data-ttu-id="abcc0-2344">在執行階段`a`先進行評估。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2344">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="abcc0-2345">如果`a`不是 null，`a`產生結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2345">If `a` is not null, `a` becomes the result.</span></span> <span data-ttu-id="abcc0-2346">否則，請`b`評估，以及轉換為類型`A`，而這會變成結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2346">Otherwise, `b` is evaluated and converted to type `A`, and this becomes the result.</span></span>
*  <span data-ttu-id="abcc0-2347">否則，如果`b`具有型別的`B`且隱含的轉換從`a`要`B`，結果型別是`B`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2347">Otherwise, if `b` has a type `B` and an implicit conversion exists from `a` to `B`, the result type is `B`.</span></span> <span data-ttu-id="abcc0-2348">在執行階段`a`先進行評估。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2348">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="abcc0-2349">如果`a`不是 null，`a`會解除包裝以輸入`A0`(如果`A`存在，而且可為 null) 和轉換成輸入`B`，而這會成為結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2349">If `a` is not null, `a` is unwrapped to type `A0` (if `A` exists and is nullable) and converted to type `B`, and this becomes the result.</span></span> <span data-ttu-id="abcc0-2350">否則，`b`評估，並產生結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2350">Otherwise, `b` is evaluated and becomes the result.</span></span>
*  <span data-ttu-id="abcc0-2351">否則，請`a`和`b`不相容，以及編譯時期錯誤，就會發生。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2351">Otherwise, `a` and `b` are incompatible, and a compile-time error occurs.</span></span>

## <a name="conditional-operator"></a><span data-ttu-id="abcc0-2352">條件運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-2352">Conditional operator</span></span>

<span data-ttu-id="abcc0-2353">`?:`運算子稱為條件運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2353">The `?:` operator is called the conditional operator.</span></span> <span data-ttu-id="abcc0-2354">它有時也稱為三元運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2354">It is at times also called the ternary operator.</span></span>

```antlr
conditional_expression
    : null_coalescing_expression
    | null_coalescing_expression '?' expression ':' expression
    ;
```

<span data-ttu-id="abcc0-2355">條件運算式的形式`b ? x : y`條件會先評估`b`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2355">A conditional expression of the form `b ? x : y` first evaluates the condition `b`.</span></span> <span data-ttu-id="abcc0-2356">然後，如果`b`已`true`，`x`評估，並產生作業的結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2356">Then, if `b` is `true`, `x` is evaluated and becomes the result of the operation.</span></span> <span data-ttu-id="abcc0-2357">否則，`y`評估，並產生作業的結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2357">Otherwise, `y` is evaluated and becomes the result of the operation.</span></span> <span data-ttu-id="abcc0-2358">條件運算式永遠不會評估兩者`x`和`y`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2358">A conditional expression never evaluates both `x` and `y`.</span></span>

<span data-ttu-id="abcc0-2359">條件運算子是右向關聯，表示由右至左的分組作業。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2359">The conditional operator is right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="abcc0-2360">例如，以下格式的運算式`a ? b : c ? d : e`評估為`a ? b : (c ? d : e)`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2360">For example, an expression of the form `a ? b : c ? d : e` is evaluated as `a ? b : (c ? d : e)`.</span></span>

<span data-ttu-id="abcc0-2361">第一個運算元`?:`運算子必須可以隱含地轉換成運算式`bool`，或實作型別的運算式`operator true`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2361">The first operand of the `?:` operator must be an expression that can be implicitly converted to `bool`, or an expression of a type that implements `operator true`.</span></span> <span data-ttu-id="abcc0-2362">如果滿足這些需求都沒有，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2362">If neither of these requirements is satisfied, a compile-time error occurs.</span></span>

<span data-ttu-id="abcc0-2363">第二個和第三個運算元`x`並`y`的`?:`運算子控制的條件運算式的類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2363">The second and third operands, `x` and `y`, of the `?:` operator control the type of the conditional expression.</span></span>

*  <span data-ttu-id="abcc0-2364">如果`x`具有類型`X`並`y`具有類型`Y`然後</span><span class="sxs-lookup"><span data-stu-id="abcc0-2364">If `x` has type `X` and `y` has type `Y` then</span></span>
   * <span data-ttu-id="abcc0-2365">如果隱含的轉換 ([隱含轉換](conversions.md#implicit-conversions)) 從`X`來`Y`，而不是從`Y`來`X`，然後`Y`是條件運算式的類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2365">If an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from `X` to `Y`, but not from `Y` to `X`, then `Y` is the type of the conditional expression.</span></span>
   * <span data-ttu-id="abcc0-2366">如果隱含的轉換 ([隱含轉換](conversions.md#implicit-conversions)) 從`Y`來`X`，而不是從`X`來`Y`，然後`X`是條件運算式的類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2366">If an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from `Y` to `X`, but not from `X` to `Y`, then `X` is the type of the conditional expression.</span></span>
   * <span data-ttu-id="abcc0-2367">否則，可以判斷任何運算式類型，並就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2367">Otherwise, no expression type can be determined, and a compile-time error occurs.</span></span>
*  <span data-ttu-id="abcc0-2368">如果只有其中一個`x`及`y`具有一個型別，並同時`x`和`y`的會隱含地轉換成該類型，則這是條件式運算式的類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2368">If only one of `x` and `y` has a type, and both `x` and `y`, of are implicitly convertible to that type, then that is the type of the conditional expression.</span></span>
*  <span data-ttu-id="abcc0-2369">否則，可以判斷任何運算式類型，並就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2369">Otherwise, no expression type can be determined, and a compile-time error occurs.</span></span>

<span data-ttu-id="abcc0-2370">條件運算式的形式執行階段處理`b ? x : y`包含下列步驟：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2370">The run-time processing of a conditional expression of the form `b ? x : y` consists of the following steps:</span></span>

*  <span data-ttu-id="abcc0-2371">第一次，`b`會評估，而`bool`的值`b`取決於：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2371">First, `b` is evaluated, and the `bool` value of `b` is determined:</span></span>
   * <span data-ttu-id="abcc0-2372">如果類型的隱含轉換`b`要`bool`存在，則會執行這項隱含轉換，來產生`bool`值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2372">If an implicit conversion from the type of `b` to `bool` exists, then this implicit conversion is performed to produce a `bool` value.</span></span>
   * <span data-ttu-id="abcc0-2373">否則，請`operator true`的類型所定義`b`叫用以產生`bool`值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2373">Otherwise, the `operator true` defined by the type of `b` is invoked to produce a `bool` value.</span></span>
*  <span data-ttu-id="abcc0-2374">如果`bool`上述步驟所產生的值是`true`，然後`x`評估，以及轉換類型的條件運算式中，這就成為條件式運算式的結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2374">If the `bool` value produced by the step above is `true`, then `x` is evaluated and converted to the type of the conditional expression, and this becomes the result of the conditional expression.</span></span>
*  <span data-ttu-id="abcc0-2375">否則，`y`評估，以及轉換類型的條件運算式中，這就成為條件式運算式的結果。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2375">Otherwise, `y` is evaluated and converted to the type of the conditional expression, and this becomes the result of the conditional expression.</span></span>

## <a name="anonymous-function-expressions"></a><span data-ttu-id="abcc0-2376">匿名函式運算式</span><span class="sxs-lookup"><span data-stu-id="abcc0-2376">Anonymous function expressions</span></span>

<span data-ttu-id="abcc0-2377">***匿名函式***是代表"line"方法定義的運算式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2377">An ***anonymous function*** is an expression that represents an "in-line" method definition.</span></span> <span data-ttu-id="abcc0-2378">匿名函式沒有值或型別本身，但轉換成相容的委派或運算式樹狀結構類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2378">An anonymous function does not have a value or type in and of itself, but is convertible to a compatible delegate or expression tree type.</span></span> <span data-ttu-id="abcc0-2379">匿名函式轉換的評估取決於轉換的目標類型：如果是委派類型，轉換就會評估委派值參考的匿名函式定義的方法。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2379">The evaluation of an anonymous function conversion depends on the target type of the conversion: If it is a delegate type, the conversion evaluates to a delegate value referencing the method which the anonymous function defines.</span></span> <span data-ttu-id="abcc0-2380">如果運算式樹狀架構類型，轉換會評估為運算式樹狀架構表示的方法，做為物件結構的結構。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2380">If it is an expression tree type, the conversion evaluates to an expression tree which represents the structure of the method as an object structure.</span></span>

<span data-ttu-id="abcc0-2381">基於歷史原因有兩種語法匿名函式，也就是*lambda_expression*s 並*anonymous_method_expression*s。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2381">For historical reasons there are two syntactic flavors of anonymous functions, namely *lambda_expression*s and *anonymous_method_expression*s.</span></span> <span data-ttu-id="abcc0-2382">幾乎所有基於*lambda_expression*會更簡潔易懂，比*anonymous_method_expression*s，保留的語言中回溯相容性。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2382">For almost all purposes, *lambda_expression*s are more concise and expressive than *anonymous_method_expression*s, which remain in the language for backwards compatibility.</span></span>

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

<span data-ttu-id="abcc0-2383">`=>`運算子具有相同的優先順序，與指派 (`=`) 和是右向關聯。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2383">The `=>` operator has the same precedence as assignment (`=`) and is right-associative.</span></span>

<span data-ttu-id="abcc0-2384">使用匿名函式`async`修飾詞是非同步函式，並且遵循規則中所述[迭代器](classes.md#iterators)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2384">An anonymous function with the `async` modifier is an async function and follows the rules described in [Iterators](classes.md#iterators).</span></span>

<span data-ttu-id="abcc0-2385">匿名函式的形式的參數*lambda_expression*可以明確或隱含型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2385">The parameters of an anonymous function in the form of a *lambda_expression* can be explicitly or implicitly typed.</span></span> <span data-ttu-id="abcc0-2386">在明確類型的參數清單中，明確指定每個參數的型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2386">In an explicitly typed parameter list, the type of each parameter is explicitly stated.</span></span> <span data-ttu-id="abcc0-2387">隱含型別的參數清單中，參數型別會推斷的匿名函式發生的內容從 — 特別是，當匿名函式轉換成相容的委派型別或型別提供運算式樹狀架構類型參數類型 ([匿名函式轉換](conversions.md#anonymous-function-conversions))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2387">In an implicitly typed parameter list, the types of the parameters are inferred from the context in which the anonymous function occurs—specifically, when the anonymous function is converted to a compatible delegate type or expression tree type, that type provides the parameter types ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>

<span data-ttu-id="abcc0-2388">在匿名函式具有單一、 隱含型別參數，可能會從參數清單省略括弧。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2388">In an anonymous function with a single, implicitly typed parameter, the parentheses may be omitted from the parameter list.</span></span> <span data-ttu-id="abcc0-2389">換句話說，表單匿名函式</span><span class="sxs-lookup"><span data-stu-id="abcc0-2389">In other words, an anonymous function of the form</span></span>
```csharp
( param ) => expr
```
<span data-ttu-id="abcc0-2390">可以縮寫成</span><span class="sxs-lookup"><span data-stu-id="abcc0-2390">can be abbreviated to</span></span>
```csharp
param => expr
```

<span data-ttu-id="abcc0-2391">參數清單的匿名函式的形式*anonymous_method_expression*是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2391">The parameter list of an anonymous function in the form of an *anonymous_method_expression* is optional.</span></span> <span data-ttu-id="abcc0-2392">如果指定的話，必須明確型別參數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2392">If given, the parameters must be explicitly typed.</span></span> <span data-ttu-id="abcc0-2393">如果不是，匿名函式轉換成委派，以使用任何參數的清單，不包含`out`參數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2393">If not, the anonymous function is convertible to a delegate with any parameter list not containing `out` parameters.</span></span>

<span data-ttu-id="abcc0-2394">A*區塊*匿名函式的主體是連線到 ([結束點和可執行性](statements.md#end-points-and-reachability)) 除非匿名函式，就會發生無法到達陳述式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2394">A *block* body of an anonymous function is reachable ([End points and reachability](statements.md#end-points-and-reachability)) unless the anonymous function occurs inside an unreachable statement.</span></span>

<span data-ttu-id="abcc0-2395">匿名函式的一些範例，請遵循下列：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2395">Some examples of anonymous functions follow below:</span></span>

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

<span data-ttu-id="abcc0-2396">行為*lambda_expression*s 並*anonymous_method_expression*s 是除了下列幾點：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2396">The behavior of *lambda_expression*s and *anonymous_method_expression*s is the same except for the following points:</span></span>

*  <span data-ttu-id="abcc0-2397">*anonymous_method_expression*s 允許參數清單，請完全省略產生可轉換成委派類型的任何值參數的清單。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2397">*anonymous_method_expression*s permit the parameter list to be omitted entirely, yielding convertibility to delegate types of any list of value parameters.</span></span>
*  <span data-ttu-id="abcc0-2398">*lambda_expression*s 允許省略和推斷而來的參數型別*anonymous_method_expression*s 需要明確指示的參數類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2398">*lambda_expression*s permit parameter types to be omitted and inferred whereas *anonymous_method_expression*s require parameter types to be explicitly stated.</span></span>
*  <span data-ttu-id="abcc0-2399">主體*lambda_expression*可以是運算式或陳述式區塊而主體*anonymous_method_expression*必須是陳述式區塊。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2399">The body of a *lambda_expression* can be an expression or a statement block whereas the body of an *anonymous_method_expression* must be a statement block.</span></span>
*  <span data-ttu-id="abcc0-2400">只有*lambda_expression*有相容的運算式樹狀架構類型的轉換 ([運算式樹狀架構類型](types.md#expression-tree-types))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2400">Only *lambda_expression*s have conversions to compatible expression tree types ([Expression tree types](types.md#expression-tree-types)).</span></span>

### <a name="anonymous-function-signatures"></a><span data-ttu-id="abcc0-2401">匿名函式簽章</span><span class="sxs-lookup"><span data-stu-id="abcc0-2401">Anonymous function signatures</span></span>

<span data-ttu-id="abcc0-2402">選擇性*anonymous_function_signature*匿名函式的定義名稱和選擇性的匿名函式的型式參數類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2402">The optional *anonymous_function_signature* of an anonymous function defines the names and optionally the types of the formal parameters for the anonymous function.</span></span> <span data-ttu-id="abcc0-2403">匿名函式參數的範圍是*anonymous_function_body*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2403">The scope of the parameters of the anonymous function is the *anonymous_function_body*.</span></span> <span data-ttu-id="abcc0-2404">([領域](basic-concepts.md#scopes)) 參數清單 （如果有） 以及匿名-方法-主體構成宣告空間 ([宣告](basic-concepts.md#declarations))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2404">([Scopes](basic-concepts.md#scopes)) Together with the parameter list (if given) the anonymous-method-body constitutes a declaration space ([Declarations](basic-concepts.md#declarations)).</span></span> <span data-ttu-id="abcc0-2405">因此是匿名函式，以符合本機變數、 區域常數或其範圍，包括的參數名稱的參數名稱的編譯時期錯誤*anonymous_method_expression*或*lambda_運算式*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2405">It is thus a compile-time error for the name of a parameter of the anonymous function to match the name of a local variable, local constant or parameter whose scope includes the *anonymous_method_expression* or *lambda_expression*.</span></span>

<span data-ttu-id="abcc0-2406">匿名函式是否*explicit_anonymous_function_signature*，則一組相容的委派型別和運算式樹狀架構類型僅限於具有相同參數類型和修飾詞會以相同的順序。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2406">If an anonymous function has an *explicit_anonymous_function_signature*, then the set of compatible delegate types and expression tree types is restricted to those that have the same parameter types and modifiers in the same order.</span></span> <span data-ttu-id="abcc0-2407">相較於方法群組轉換 ([方法群組轉換](conversions.md#method-group-conversions))，不支援 /yield 匿名函式參數類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2407">In contrast to method group conversions ([Method group conversions](conversions.md#method-group-conversions)), contra-variance of anonymous function parameter types is not supported.</span></span> <span data-ttu-id="abcc0-2408">如果匿名函式沒有*anonymous_function_signature*，則一組相容的委派型別和運算式樹狀架構類型僅限於那些沒有`out`參數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2408">If an anonymous function does not have an *anonymous_function_signature*, then the set of compatible delegate types and expression tree types is restricted to those that have no `out` parameters.</span></span>

<span data-ttu-id="abcc0-2409">請注意， *anonymous_function_signature*不能包含屬性或參數陣列。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2409">Note that an *anonymous_function_signature* cannot include attributes or a parameter array.</span></span> <span data-ttu-id="abcc0-2410">不過， *anonymous_function_signature*可能相容的參數清單包含參數陣列的委派類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2410">Nevertheless, an *anonymous_function_signature* may be compatible with a delegate type whose parameter list contains a parameter array.</span></span>

<span data-ttu-id="abcc0-2411">也請注意，轉換為運算式樹狀結構類型，即使相容，仍然可能會在編譯階段失敗 ([運算式樹狀架構類型](types.md#expression-tree-types))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2411">Note also that conversion to an expression tree type, even if compatible, may still fail at compile-time ([Expression tree types](types.md#expression-tree-types)).</span></span>

### <a name="anonymous-function-bodies"></a><span data-ttu-id="abcc0-2412">匿名函式主體</span><span class="sxs-lookup"><span data-stu-id="abcc0-2412">Anonymous function bodies</span></span>

<span data-ttu-id="abcc0-2413">主體 (*運算式*或是*區塊*) 的匿名函式會受限於下列規則：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2413">The body (*expression* or *block*) of an anonymous function is subject to the following rules:</span></span>

*  <span data-ttu-id="abcc0-2414">如果匿名函式包含簽章，就有一個簽章中指定的參數可在本文中。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2414">If the anonymous function includes a signature, the parameters specified in the signature are available in the body.</span></span> <span data-ttu-id="abcc0-2415">如果匿名函式沒有簽章可以轉換為委派型別或具有參數的運算式型別 ([匿名函式轉換](conversions.md#anonymous-function-conversions))，但無法存取參數，在主體中。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2415">If the anonymous function has no signature it can be converted to a delegate type or expression type having parameters ([Anonymous function conversions](conversions.md#anonymous-function-conversions)), but the parameters cannot be accessed in the body.</span></span>
*  <span data-ttu-id="abcc0-2416">除了`ref`或是`out`參數的最內層的簽章 （如果有的話） 中指定匿名函式，它是主體存取的編譯時期錯誤`ref`或`out`參數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2416">Except for `ref` or `out` parameters specified in the signature (if any) of the nearest enclosing anonymous function, it is a compile-time error for the body to access a `ref` or `out` parameter.</span></span>
*  <span data-ttu-id="abcc0-2417">當的型別`this`是結構類型時，它是主體存取的編譯時期錯誤`this`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2417">When the type of `this` is a struct type, it is a compile-time error for the body to access `this`.</span></span> <span data-ttu-id="abcc0-2418">這是，則為 true 的存取是否明確 (依照`this.x`) 或隱含 (中`x`其中`x`是結構的執行個體成員)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2418">This is true whether the access is explicit (as in `this.x`) or implicit (as in `x` where `x` is an instance member of the struct).</span></span> <span data-ttu-id="abcc0-2419">此規則只是禁止這類存取權，並不會影響成員查詢結果中的結構成員。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2419">This rule simply prohibits such access and does not affect whether member lookup results in a member of the struct.</span></span>
*  <span data-ttu-id="abcc0-2420">此主體可存取外部變數 ([外部變數](expressions.md#outer-variables)) 的匿名函式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2420">The body has access to the outer variables ([Outer variables](expressions.md#outer-variables)) of the anonymous function.</span></span> <span data-ttu-id="abcc0-2421">外部變數的存取會參考此變數會是作用中時的執行個體*lambda_expression*或是*anonymous_method_expression*評估 ([評估匿名函式運算式](expressions.md#evaluation-of-anonymous-function-expressions))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2421">Access of an outer variable will reference the instance of the variable that is active at the time the *lambda_expression* or *anonymous_method_expression* is evaluated ([Evaluation of anonymous function expressions](expressions.md#evaluation-of-anonymous-function-expressions)).</span></span>
*  <span data-ttu-id="abcc0-2422">本文包含的編譯時期錯誤就`goto`陳述式中，`break`陳述式，或`continue`其目標是主體之外，或包含匿名函式主體內的陳述式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2422">It is a compile-time error for the body to contain a `goto` statement, `break` statement, or `continue` statement whose target is outside the body or within the body of a contained anonymous function.</span></span>
*  <span data-ttu-id="abcc0-2423">A`return`主體中的陳述式會傳回控制項從最接近的封閉式的引動過程不是來自封入函式成員的匿名函式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2423">A `return` statement in the body returns control from an invocation of the nearest enclosing anonymous function, not from the enclosing function member.</span></span> <span data-ttu-id="abcc0-2424">中指定的運算式`return`陳述式必須是隱含地轉換成委派類型的運算式樹狀架構型別，要傳回的型別最接近的封閉式*lambda_expression*或*anonymous_method_expression*轉換 ([匿名函式轉換](conversions.md#anonymous-function-conversions))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2424">An expression specified in a `return` statement must be implicitly convertible to the return type of the delegate type or expression tree type to which the nearest enclosing *lambda_expression* or *anonymous_method_expression* is converted ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>

<span data-ttu-id="abcc0-2425">是否沒有任何方法可以執行而不透過評估和引動過程的匿名函式的區塊並未明確指定*lambda_expression*或*anonymous_method_expression*.</span><span class="sxs-lookup"><span data-stu-id="abcc0-2425">It is explicitly unspecified whether there is any way to execute the block of an anonymous function other than through evaluation and invocation of the *lambda_expression* or *anonymous_method_expression*.</span></span> <span data-ttu-id="abcc0-2426">特別是，編譯器可能會選擇實作的匿名函式，藉由合成一或多個具名方法或類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2426">In particular, the compiler may choose to implement an anonymous function by synthesizing one or more named methods or types.</span></span> <span data-ttu-id="abcc0-2427">保留供編譯器使用必須是格式的這類合成的項目的名稱。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2427">The names of any such synthesized elements must be of a form reserved for compiler use.</span></span>

### <a name="overload-resolution-and-anonymous-functions"></a><span data-ttu-id="abcc0-2428">多載解析和匿名函式</span><span class="sxs-lookup"><span data-stu-id="abcc0-2428">Overload resolution and anonymous functions</span></span>

<span data-ttu-id="abcc0-2429">匿名函式引數清單中的加入型別推斷和多載解析。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2429">Anonymous functions in an argument list participate in type inference and overload resolution.</span></span> <span data-ttu-id="abcc0-2430">請參閱[型別推斷](expressions.md#type-inference)並[多載解析](expressions.md#overload-resolution)的確切規則。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2430">Please refer to [Type inference](expressions.md#type-inference) and [Overload resolution](expressions.md#overload-resolution) for the exact rules.</span></span>

<span data-ttu-id="abcc0-2431">下列範例說明多載解析匿名函式的影響。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2431">The following example illustrates the effect of anonymous functions on overload resolution.</span></span>

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

<span data-ttu-id="abcc0-2432">`ItemList<T>`類別有兩個`Sum`方法。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2432">The `ItemList<T>` class has two `Sum` methods.</span></span> <span data-ttu-id="abcc0-2433">每個處理`selector`引數，從清單項目透過擷取值總和。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2433">Each takes a `selector` argument, which extracts the value to sum over from a list item.</span></span> <span data-ttu-id="abcc0-2434">所擷取的值可以是`int`或是`double`與產生的加總同樣`int`或`double`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2434">The extracted value can be either an `int` or a `double` and the resulting sum is likewise either an `int` or a `double`.</span></span>

<span data-ttu-id="abcc0-2435">`Sum`方法，例如可用來計算從清單中訂單的詳細資料行的總和。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2435">The `Sum` methods could for example be used to compute sums from a list of detail lines in an order.</span></span>

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

<span data-ttu-id="abcc0-2436">中的第一個引動過程`orderDetails.Sum`，這兩個`Sum`方法都適用因為匿名函式`d => d. UnitCount`適用於兩者`Func<Detail,int>`和`Func<Detail,double>`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2436">In the first invocation of `orderDetails.Sum`, both `Sum` methods are applicable because the anonymous function `d => d. UnitCount` is compatible with both `Func<Detail,int>` and `Func<Detail,double>`.</span></span> <span data-ttu-id="abcc0-2437">不過，多載解析會挑選第一個`Sum`方法因為轉換成`Func<Detail,int>`優於轉換為`Func<Detail,double>`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2437">However, overload resolution picks the first `Sum` method because the conversion to `Func<Detail,int>` is better than the conversion to `Func<Detail,double>`.</span></span>

<span data-ttu-id="abcc0-2438">中的第二個引動過程`orderDetails.Sum`，而第二個`Sum`方法就適用因為匿名函式`d => d.UnitPrice * d.UnitCount`會產生型別的值`double`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2438">In the second invocation of `orderDetails.Sum`, only the second `Sum` method is applicable because the anonymous function `d => d.UnitPrice * d.UnitCount` produces a value of type `double`.</span></span> <span data-ttu-id="abcc0-2439">因此，多載解析會挑選第二個`Sum`方法的引動過程。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2439">Thus, overload resolution picks the second `Sum` method for that invocation.</span></span>

### <a name="anonymous-functions-and-dynamic-binding"></a><span data-ttu-id="abcc0-2440">匿名函式和動態繫結</span><span class="sxs-lookup"><span data-stu-id="abcc0-2440">Anonymous functions and dynamic binding</span></span>

<span data-ttu-id="abcc0-2441">匿名函式不能是接收者、 引數或動態繫結作業的運算元。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2441">An anonymous function cannot be a receiver, argument or operand of a dynamically bound operation.</span></span>

### <a name="outer-variables"></a><span data-ttu-id="abcc0-2442">外部變數</span><span class="sxs-lookup"><span data-stu-id="abcc0-2442">Outer variables</span></span>

<span data-ttu-id="abcc0-2443">任何本機變數、 值參數或其範圍，包括參數陣列*lambda_expression*或是*anonymous_method_expression*稱為***外部變數***匿名函式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2443">Any local variable, value parameter, or parameter array whose scope includes the *lambda_expression* or *anonymous_method_expression* is called an ***outer variable*** of the anonymous function.</span></span> <span data-ttu-id="abcc0-2444">在類別的執行個體函式成員`this`值會被視為實值參數，且任何包含在函式成員內的匿名函式的外部變數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2444">In an instance function member of a class, the `this` value is considered a value parameter and is an outer variable of any anonymous function contained within the function member.</span></span>

#### <a name="captured-outer-variables"></a><span data-ttu-id="abcc0-2445">擷取外部變數</span><span class="sxs-lookup"><span data-stu-id="abcc0-2445">Captured outer variables</span></span>

<span data-ttu-id="abcc0-2446">當外部變數的匿名函式所參考時，即稱為外部變數已被***擷取***匿名函式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2446">When an outer variable is referenced by an anonymous function, the outer variable is said to have been ***captured*** by the anonymous function.</span></span> <span data-ttu-id="abcc0-2447">一般情況下，本機變數的存留期間受限於區塊或陳述式與它相關聯的執行 ([區域變數](variables.md#local-variables))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2447">Ordinarily, the lifetime of a local variable is limited to execution of the block or statement with which it is associated ([Local variables](variables.md#local-variables)).</span></span> <span data-ttu-id="abcc0-2448">不過，擷取的外部變數的存留期至少延長到委派，或從匿名函式建立的運算式樹狀架構就適合進行記憶體回收。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2448">However, the lifetime of a captured outer variable is extended at least until the delegate or expression tree created from the anonymous function becomes eligible for garbage collection.</span></span>

<span data-ttu-id="abcc0-2449">在範例</span><span class="sxs-lookup"><span data-stu-id="abcc0-2449">In the example</span></span>
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
<span data-ttu-id="abcc0-2450">區域變數`x`擷取匿名函式，及其的存留期`x`至少會延伸到傳回的委派之前`F`就適合進行記憶體回收 （不會發生的最尾端之前程式）。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2450">the local variable `x` is captured by the anonymous function, and the lifetime of `x` is extended at least until the delegate returned from `F` becomes eligible for garbage collection (which doesn't happen until the very end of the program).</span></span> <span data-ttu-id="abcc0-2451">因為相同的執行個體上運作的匿名函式每次叫用`x`，此範例的輸出：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2451">Since each invocation of the anonymous function operates on the same instance of `x`, the output of the example is:</span></span>
```
1
2
3
```

<span data-ttu-id="abcc0-2452">當匿名函式擷取的本機變數或值參數時，本機變數或參數不再被視為是固定的變數 ([固定和可移動變數](unsafe-code.md#fixed-and-moveable-variables))，但會被視為是 moveable變數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2452">When a local variable or a value parameter is captured by an anonymous function, the local variable or parameter is no longer considered to be a fixed variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)), but is instead considered to be a moveable variable.</span></span> <span data-ttu-id="abcc0-2453">因此，任何`unsafe`程式碼會擷取外部變數的位址，必須先使用`fixed`修正變數的陳述式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2453">Thus any `unsafe` code that takes the address of a captured outer variable must first use the `fixed` statement to fix the variable.</span></span>

<span data-ttu-id="abcc0-2454">請注意，不同於 uncaptured 的變數，擷取本機變數可以同時公開至執行多個執行緒。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2454">Note that unlike an uncaptured variable, a captured local variable can be simultaneously exposed to multiple threads of execution.</span></span>

#### <a name="instantiation-of-local-variables"></a><span data-ttu-id="abcc0-2455">具現化的本機變數</span><span class="sxs-lookup"><span data-stu-id="abcc0-2455">Instantiation of local variables</span></span>

<span data-ttu-id="abcc0-2456">本機變數會被視為***具現化***執行時進入變數的範圍。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2456">A local variable is considered to be ***instantiated*** when execution enters the scope of the variable.</span></span> <span data-ttu-id="abcc0-2457">例如，下列方法叫用時，本機變數`x`是具現化並初始化三次，一次迴圈的每個反覆項目。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2457">For example, when the following method is invoked, the local variable `x` is instantiated and initialized three times—once for each iteration of the loop.</span></span>

```csharp
static void F() {
    for (int i = 0; i < 3; i++) {
        int x = i * 2 + 1;
        ...
    }
}
```

<span data-ttu-id="abcc0-2458">不過，移動的宣告`x`之外的單一具現化的迴圈導致`x`:</span><span class="sxs-lookup"><span data-stu-id="abcc0-2458">However, moving the declaration of `x` outside the loop results in a single instantiation of `x`:</span></span>
```csharp
static void F() {
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        ...
    }
}
```

<span data-ttu-id="abcc0-2459">不擷取，就無法觀察區域變數具現化的方式通常 — 因為具現化的存留期是否不相交，就可以為每個具現化，只要使用相同的儲存位置。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2459">When not captured, there is no way to observe exactly how often a local variable is instantiated—because the lifetimes of the instantiations are disjoint, it is possible for each instantiation to simply use the same storage location.</span></span> <span data-ttu-id="abcc0-2460">不過，當匿名函式擷取區域變數，具現化的效果會變得明顯。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2460">However, when an anonymous function captures a local variable, the effects of instantiation become apparent.</span></span>

<span data-ttu-id="abcc0-2461">此範例</span><span class="sxs-lookup"><span data-stu-id="abcc0-2461">The example</span></span>
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
<span data-ttu-id="abcc0-2462">產生下列輸出：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2462">produces the output:</span></span>
```
1
3
5
```

<span data-ttu-id="abcc0-2463">不過，當宣告`x`移到迴圈外：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2463">However, when the declaration of `x` is moved outside the loop:</span></span>
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
<span data-ttu-id="abcc0-2464">輸出為：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2464">the output is:</span></span>
```
5
5
5
```

<span data-ttu-id="abcc0-2465">如果 for 迴圈中宣告的反覆項目變數，該變數本身視為在迴圈外宣告。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2465">If a for-loop declares an iteration variable, that variable itself is considered to be declared outside of the loop.</span></span> <span data-ttu-id="abcc0-2466">因此，如果此範例會變更為擷取反覆項目變數本身：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2466">Thus, if the example is changed to capture the iteration variable itself:</span></span>

```csharp
static D[] F() {
    D[] result = new D[3];
    for (int i = 0; i < 3; i++) {
        result[i] = () => { Console.WriteLine(i); };
    }
    return result;
}
```
<span data-ttu-id="abcc0-2467">已擷取的反覆運算變數只有一個執行個體，產生下列輸出：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2467">only one instance of the iteration variable is captured, which produces the output:</span></span>
```
3
3
3
```

<span data-ttu-id="abcc0-2468">您有要分享一些擷取的變數，但有不同的執行個體的其他人的匿名函式委派。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2468">It is possible for anonymous function delegates to share some captured variables yet have separate instances of others.</span></span> <span data-ttu-id="abcc0-2469">例如，如果`F`變更為</span><span class="sxs-lookup"><span data-stu-id="abcc0-2469">For example, if `F` is changed to</span></span>
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
<span data-ttu-id="abcc0-2470">三個委派擷取相同的執行個體`x`但不同的執行個體`y`，輸出則：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2470">the three delegates capture the same instance of `x` but separate instances of `y`, and the output is:</span></span>
```
1 1
2 1
3 1
```

<span data-ttu-id="abcc0-2471">個別的匿名函式可以擷取相同的執行個體的外部變數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2471">Separate anonymous functions can capture the same instance of an outer variable.</span></span> <span data-ttu-id="abcc0-2472">在下列範例中：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2472">In the example:</span></span>
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
<span data-ttu-id="abcc0-2473">兩個的匿名函式擷取相同的執行個體的本機變數`x`，以及他們可以因此 「 通訊 」 透過該變數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2473">the two anonymous functions capture the same instance of the local variable `x`, and they can thus "communicate" through that variable.</span></span> <span data-ttu-id="abcc0-2474">此範例的輸出為：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2474">The output of the example is:</span></span>
```
5
10
```

### <a name="evaluation-of-anonymous-function-expressions"></a><span data-ttu-id="abcc0-2475">匿名函式運算式的評估</span><span class="sxs-lookup"><span data-stu-id="abcc0-2475">Evaluation of anonymous function expressions</span></span>

<span data-ttu-id="abcc0-2476">匿名函式`F`永遠必須轉換成委派類型`D`或運算式樹狀架構型別`E`，直接或透過委派建立運算式執行`new D(F)`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2476">An anonymous function `F` must always be converted to a delegate type `D` or an expression tree type `E`, either directly or through the execution of a delegate creation expression `new D(F)`.</span></span> <span data-ttu-id="abcc0-2477">這項轉換會決定的匿名函式結果中所述[匿名函式轉換](conversions.md#anonymous-function-conversions)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2477">This conversion determines the result of the anonymous function, as described in [Anonymous function conversions](conversions.md#anonymous-function-conversions).</span></span>

## <a name="query-expressions"></a><span data-ttu-id="abcc0-2478">查詢運算式</span><span class="sxs-lookup"><span data-stu-id="abcc0-2478">Query expressions</span></span>

<span data-ttu-id="abcc0-2479">***查詢運算式***提供語言整合式的語法類似於關聯式和階層式查詢的語言，例如 SQL 和 XQuery 的查詢。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2479">***Query expressions*** provide a language integrated syntax for queries that is similar to relational and hierarchical query languages such as SQL and XQuery.</span></span>

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

<span data-ttu-id="abcc0-2480">查詢運算式的開頭`from`子句和以結束`select`或`group`子句。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2480">A query expression begins with a `from` clause and ends with either a `select` or `group` clause.</span></span> <span data-ttu-id="abcc0-2481">初始`from`子句後面可以接著零或多個`from`， `let`， `where`，`join`或`orderby`子句。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2481">The initial `from` clause can be followed by zero or more `from`, `let`, `where`, `join` or `orderby` clauses.</span></span> <span data-ttu-id="abcc0-2482">每個`from`子句是產生器簡介***範圍變數***範圍中的項目***順序***。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2482">Each `from` clause is a generator introducing a ***range variable*** which ranges over the elements of a ***sequence***.</span></span> <span data-ttu-id="abcc0-2483">每個`let`子句會導入代表值，計算透過前一個範圍變數的範圍變數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2483">Each `let` clause introduces a range variable representing a value computed by means of previous range variables.</span></span> <span data-ttu-id="abcc0-2484">每個`where`子句是篩選器，從結果排除項目。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2484">Each `where` clause is a filter that excludes items from the result.</span></span> <span data-ttu-id="abcc0-2485">每個`join`子句將比較指定的索引鍵的來源序列的其他順序，產生符合的配對的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2485">Each `join` clause compares specified keys of the source sequence with keys of another sequence, yielding matching pairs.</span></span> <span data-ttu-id="abcc0-2486">每個`orderby`子句重新排列項目，根據指定的準則。最終`select`或`group`子句指定結果的範圍變數的形狀。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2486">Each `orderby` clause reorders items according to specified criteria.The final `select` or `group` clause specifies the shape of the result in terms of the range variables.</span></span> <span data-ttu-id="abcc0-2487">最後，`into`子句可以用來 「 接合 」 查詢，藉由將一個查詢的結果為在後續查詢產生器。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2487">Finally, an `into` clause can be used to "splice" queries by treating the results of one query as a generator in a subsequent query.</span></span>

### <a name="ambiguities-in-query-expressions"></a><span data-ttu-id="abcc0-2488">查詢運算式中的模稜兩可</span><span class="sxs-lookup"><span data-stu-id="abcc0-2488">Ambiguities in query expressions</span></span>

<span data-ttu-id="abcc0-2489">查詢運算式包含 「 內容關鍵字 」，亦即，給定的內容中具有特殊意義的識別項數目。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2489">Query expressions contain a number of "contextual keywords", i.e., identifiers that have special meaning in a given context.</span></span> <span data-ttu-id="abcc0-2490">特別是這些`from`， `where`， `join`， `on`， `equals`， `into`， `let`， `orderby`， `ascending`， `descending`， `select`，`group`和`by`.</span><span class="sxs-lookup"><span data-stu-id="abcc0-2490">Specifically these are `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` and `by`.</span></span> <span data-ttu-id="abcc0-2491">為了避免因混合使用這些識別碼，作為關鍵字或簡單名稱的查詢運算式中的模稜兩可，這些識別項都會被視為關鍵字的查詢運算式中發生的任何位置。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2491">In order to avoid ambiguities in query expressions caused by mixed use of these identifiers as keywords or simple names, these identifiers are considered keywords when occurring anywhere within a query expression.</span></span>

<span data-ttu-id="abcc0-2492">基於此目的，查詢運算式是以開頭的任何運算式 」`from identifier`"後面接著以外的任何權杖 」`;`"，"`=`「 或 」`,`"。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2492">For this purpose, a query expression is any expression that starts with "`from identifier`" followed by any token except "`;`", "`=`" or "`,`".</span></span>

<span data-ttu-id="abcc0-2493">若要使用這些字做為查詢運算式中的識別項，它們可以在前面加上"`@`」 ([識別碼](lexical-structure.md#identifiers))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2493">In order to use these words as identifiers within a query expression, they can be prefixed with "`@`" ([Identifiers](lexical-structure.md#identifiers)).</span></span>

### <a name="query-expression-translation"></a><span data-ttu-id="abcc0-2494">查詢運算式轉譯</span><span class="sxs-lookup"><span data-stu-id="abcc0-2494">Query expression translation</span></span>

<span data-ttu-id="abcc0-2495">C# 語言不會指定查詢運算式的執行語意。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2495">The C# language does not specify the execution semantics of query expressions.</span></span> <span data-ttu-id="abcc0-2496">相反地，查詢運算式會轉譯成符合方法的引動過程*查詢運算式模式*([查詢運算式模式](expressions.md#the-query-expression-pattern))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2496">Rather, query expressions are translated into invocations of methods that adhere to the *query expression pattern* ([The query expression pattern](expressions.md#the-query-expression-pattern)).</span></span> <span data-ttu-id="abcc0-2497">具體而言，查詢運算式會轉譯成方法，名稱為叫用`Where`， `Select`， `SelectMany`， `Join`， `GroupJoin`， `OrderBy`， `OrderByDescending`， `ThenBy`， `ThenByDescending`， `GroupBy`，和`Cast`。這些方法都應該有特定的簽章和結果的型別中所述[查詢運算式模式](expressions.md#the-query-expression-pattern)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2497">Specifically, query expressions are translated into invocations of methods named `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy`, and `Cast`.These methods are expected to have particular signatures and result types, as described in [The query expression pattern](expressions.md#the-query-expression-pattern).</span></span> <span data-ttu-id="abcc0-2498">這些方法可以是所查詢之物件的執行個體方法或擴充方法外部物件，並在實作查詢的實際執行。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2498">These methods can be instance methods of the object being queried or extension methods that are external to the object, and they implement the actual execution of the query.</span></span>

<span data-ttu-id="abcc0-2499">從查詢運算式轉譯為方法引動過程語法的對應，就會發生任何型別繫結之前或執行多載解析。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2499">The translation from query expressions to method invocations is a syntactic mapping that occurs before any type binding or overload resolution has been performed.</span></span> <span data-ttu-id="abcc0-2500">轉譯可以保證語法正確，但它不會保證能夠產生語意正確的 C# 程式碼。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2500">The translation is guaranteed to be syntactically correct, but it is not guaranteed to produce semantically correct C# code.</span></span> <span data-ttu-id="abcc0-2501">查詢運算式的轉譯，產生的方法引動過程處理為一般方法引動過程，而這又可能會發現錯誤，例如，如果方法不存在，如果引數有錯誤的類型，或如果方法是泛型和型別推斷失敗。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2501">Following translation of query expressions, the resulting method invocations are processed as regular method invocations, and this may in turn uncover errors, for example if the methods do not exist, if arguments have wrong types, or if the methods are generic and type inference fails.</span></span>

<span data-ttu-id="abcc0-2502">查詢運算式是由重複套用下列的翻譯之前可能不會有任何進一步簡化, 處理。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2502">A query expression is processed by repeatedly applying the following translations until no further reductions are possible.</span></span> <span data-ttu-id="abcc0-2503">轉譯會依照順序的應用程式： 每個區段假設已徹底，執行上述章節中的翻譯，且之後用完，區段會不稍後可再次回到相同的查詢運算式的處理中。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2503">The translations are listed in order of application: each section assumes that the translations in the preceding sections have been performed exhaustively, and once exhausted, a section will not later be revisited in the processing of the same query expression.</span></span>

<span data-ttu-id="abcc0-2504">查詢運算式中不允許指派給範圍變數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2504">Assignment to range variables is not allowed in query expressions.</span></span> <span data-ttu-id="abcc0-2505">不過 C# 實作允許不一定強制執行這項限制，因為這有時不可能與此處所提供的語法轉譯配置。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2505">However a C# implementation is permitted to not always enforce this restriction, since this may sometimes not be possible with the syntactic translation scheme presented here.</span></span>

<span data-ttu-id="abcc0-2506">特定翻譯插入且透明的識別項所表示的範圍變數`*`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2506">Certain translations inject range variables with transparent identifiers denoted by `*`.</span></span> <span data-ttu-id="abcc0-2507">透明識別項的特殊屬性會討論中進一步[透明識別項](expressions.md#transparent-identifiers)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2507">The special properties of transparent identifiers are discussed further in [Transparent identifiers](expressions.md#transparent-identifiers).</span></span>

#### <a name="select-and-groupby-clauses-with-continuations"></a><span data-ttu-id="abcc0-2508">接續的定序的選取和 groupby 子句</span><span class="sxs-lookup"><span data-stu-id="abcc0-2508">Select and groupby clauses with continuations</span></span>

<span data-ttu-id="abcc0-2509">查詢運算式與接續</span><span class="sxs-lookup"><span data-stu-id="abcc0-2509">A query expression with a continuation</span></span>
```csharp
from ... into x ...
```
<span data-ttu-id="abcc0-2510">轉譯成</span><span class="sxs-lookup"><span data-stu-id="abcc0-2510">is translated into</span></span>
```csharp
from x in ( from ... ) ...
```

<span data-ttu-id="abcc0-2511">下列各節中的轉譯會假設查詢沒有任何`into`接續。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2511">The translations in the following sections assume that queries have no `into` continuations.</span></span>

<span data-ttu-id="abcc0-2512">此範例</span><span class="sxs-lookup"><span data-stu-id="abcc0-2512">The example</span></span>
```csharp
from c in customers
group c by c.Country into g
select new { Country = g.Key, CustCount = g.Count() }
```
<span data-ttu-id="abcc0-2513">轉譯成</span><span class="sxs-lookup"><span data-stu-id="abcc0-2513">is translated into</span></span>
```csharp
from g in
    from c in customers
    group c by c.Country
select new { Country = g.Key, CustCount = g.Count() }
```
<span data-ttu-id="abcc0-2514">最終的翻譯</span><span class="sxs-lookup"><span data-stu-id="abcc0-2514">the final translation of which is</span></span>
```csharp
customers.
GroupBy(c => c.Country).
Select(g => new { Country = g.Key, CustCount = g.Count() })
```

#### <a name="explicit-range-variable-types"></a><span data-ttu-id="abcc0-2515">明確的範圍變數型別</span><span class="sxs-lookup"><span data-stu-id="abcc0-2515">Explicit range variable types</span></span>

<span data-ttu-id="abcc0-2516">A`from`子句來明確指定範圍變數的類型</span><span class="sxs-lookup"><span data-stu-id="abcc0-2516">A `from` clause that explicitly specifies a range variable type</span></span>
```csharp
from T x in e
```
<span data-ttu-id="abcc0-2517">轉譯成</span><span class="sxs-lookup"><span data-stu-id="abcc0-2517">is translated into</span></span>
```csharp
from x in ( e ) . Cast < T > ( )
```

<span data-ttu-id="abcc0-2518">A`join`子句來明確指定範圍變數的類型</span><span class="sxs-lookup"><span data-stu-id="abcc0-2518">A `join` clause that explicitly specifies a range variable type</span></span>
```
join T x in e on k1 equals k2
```
<span data-ttu-id="abcc0-2519">轉譯成</span><span class="sxs-lookup"><span data-stu-id="abcc0-2519">is translated into</span></span>
```
join x in ( e ) . Cast < T > ( ) on k1 equals k2
```

<span data-ttu-id="abcc0-2520">下列各節中的轉譯會假設查詢有沒有明確的範圍變數的型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2520">The translations in the following sections assume that queries have no explicit range variable types.</span></span>

<span data-ttu-id="abcc0-2521">此範例</span><span class="sxs-lookup"><span data-stu-id="abcc0-2521">The example</span></span>
```csharp
from Customer c in customers
where c.City == "London"
select c
```
<span data-ttu-id="abcc0-2522">轉譯成</span><span class="sxs-lookup"><span data-stu-id="abcc0-2522">is translated into</span></span>
```csharp
from c in customers.Cast<Customer>()
where c.City == "London"
select c
```
<span data-ttu-id="abcc0-2523">最終的翻譯</span><span class="sxs-lookup"><span data-stu-id="abcc0-2523">the final translation of which is</span></span>
```csharp
customers.
Cast<Customer>().
Where(c => c.City == "London")
```

<span data-ttu-id="abcc0-2524">明確的範圍變數型別可用於查詢實作非泛型集合`IEnumerable`介面，但不是泛型`IEnumerable<T>`介面。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2524">Explicit range variable types are useful for querying collections that implement the non-generic `IEnumerable` interface, but not the generic `IEnumerable<T>` interface.</span></span> <span data-ttu-id="abcc0-2525">在上述範例中，這會是則`customers`類型的`ArrayList`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2525">In the example above, this would be the case if `customers` were of type `ArrayList`.</span></span>

#### <a name="degenerate-query-expressions"></a><span data-ttu-id="abcc0-2526">變質的查詢運算式</span><span class="sxs-lookup"><span data-stu-id="abcc0-2526">Degenerate query expressions</span></span>

<span data-ttu-id="abcc0-2527">格式的查詢運算式</span><span class="sxs-lookup"><span data-stu-id="abcc0-2527">A query expression of the form</span></span>
```csharp
from x in e select x
```
<span data-ttu-id="abcc0-2528">轉譯成</span><span class="sxs-lookup"><span data-stu-id="abcc0-2528">is translated into</span></span>
```csharp
( e ) . Select ( x => x )
```

<span data-ttu-id="abcc0-2529">此範例</span><span class="sxs-lookup"><span data-stu-id="abcc0-2529">The example</span></span>
```csharp
from c in customers
select c
```
<span data-ttu-id="abcc0-2530">轉譯成</span><span class="sxs-lookup"><span data-stu-id="abcc0-2530">is translated into</span></span>
```csharp
customers.Select(c => c)
```

<span data-ttu-id="abcc0-2531">變質的查詢運算式是來源的透過極簡方式選取項目。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2531">A degenerate query expression is one that trivially selects the elements of the source.</span></span> <span data-ttu-id="abcc0-2532">翻譯的後期移除變質導入由其他轉譯工作順序步驟，取代為其來源的查詢。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2532">A later phase of the translation removes degenerate queries introduced by other translation steps by replacing them with their source.</span></span> <span data-ttu-id="abcc0-2533">務必以確保查詢的結果運算式絕不會是來源物件本身，不過，會顯示類型和來源的身分識別給用戶端的查詢。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2533">It is important however to ensure that the result of a query expression is never the source object itself, as that would reveal the type and identity of the source to the client of the query.</span></span> <span data-ttu-id="abcc0-2534">因此這個步驟可保護撰寫直接在原始程式碼中明確呼叫的變質查詢`Select`來源上的資料。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2534">Therefore this step protects degenerate queries written directly in source code by explicitly calling `Select` on the source.</span></span> <span data-ttu-id="abcc0-2535">然後是由實作者的`Select`和其他查詢運算子，以確保這些方法會永遠不會傳回本身的來源物件。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2535">It is then up to the implementers of `Select` and other query operators to ensure that these methods never return the source object itself.</span></span>

#### <a name="from-let-where-join-and-orderby-clauses"></a><span data-ttu-id="abcc0-2536">From、 let、 where、 聯結和 orderby 子句</span><span class="sxs-lookup"><span data-stu-id="abcc0-2536">From, let, where, join and orderby clauses</span></span>

<span data-ttu-id="abcc0-2537">第二個查詢運算式`from`子句後面`select`子句</span><span class="sxs-lookup"><span data-stu-id="abcc0-2537">A query expression with a second `from` clause followed by a `select` clause</span></span>
```csharp
from x1 in e1
from x2 in e2
select v
```
<span data-ttu-id="abcc0-2538">轉譯成</span><span class="sxs-lookup"><span data-stu-id="abcc0-2538">is translated into</span></span>
```csharp
( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => v )
```

<span data-ttu-id="abcc0-2539">第二個查詢運算式`from`子句後面的項目以外的其他`select`子句：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2539">A query expression with a second `from` clause followed by something other than a `select` clause:</span></span>

```csharp
from x1 in e1
from x2 in e2
...
```
<span data-ttu-id="abcc0-2540">轉譯成</span><span class="sxs-lookup"><span data-stu-id="abcc0-2540">is translated into</span></span>
```csharp
from * in ( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => new { x1 , x2 } )
...
```

<span data-ttu-id="abcc0-2541">使用查詢運算式`let`子句</span><span class="sxs-lookup"><span data-stu-id="abcc0-2541">A query expression with a `let` clause</span></span>
```csharp
from x in e
let y = f
...
```
<span data-ttu-id="abcc0-2542">轉譯成</span><span class="sxs-lookup"><span data-stu-id="abcc0-2542">is translated into</span></span>
```csharp
from * in ( e ) . Select ( x => new { x , y = f } )
...
```

<span data-ttu-id="abcc0-2543">使用查詢運算式`where`子句</span><span class="sxs-lookup"><span data-stu-id="abcc0-2543">A query expression with a `where` clause</span></span>
```csharp
from x in e
where f
...
```
<span data-ttu-id="abcc0-2544">轉譯成</span><span class="sxs-lookup"><span data-stu-id="abcc0-2544">is translated into</span></span>
```csharp
from x in ( e ) . Where ( x => f )
...
```

<span data-ttu-id="abcc0-2545">使用查詢運算式`join`子句但不含`into`後面`select`子句</span><span class="sxs-lookup"><span data-stu-id="abcc0-2545">A query expression with a `join` clause without an `into` followed by a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
select v
```
<span data-ttu-id="abcc0-2546">轉譯成</span><span class="sxs-lookup"><span data-stu-id="abcc0-2546">is translated into</span></span>
```csharp
( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => v )
```

<span data-ttu-id="abcc0-2547">使用查詢運算式`join`子句但不含`into`後面的項目以外的其他`select`子句</span><span class="sxs-lookup"><span data-stu-id="abcc0-2547">A query expression with a `join` clause without an `into` followed by something other than a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
...
```
<span data-ttu-id="abcc0-2548">轉譯成</span><span class="sxs-lookup"><span data-stu-id="abcc0-2548">is translated into</span></span>
```csharp
from * in ( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => new { x1 , x2 })
...
```

<span data-ttu-id="abcc0-2549">使用查詢運算式`join`子句`into`後面`select`子句</span><span class="sxs-lookup"><span data-stu-id="abcc0-2549">A query expression with a `join` clause with an `into` followed by a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
select v
```
<span data-ttu-id="abcc0-2550">轉譯成</span><span class="sxs-lookup"><span data-stu-id="abcc0-2550">is translated into</span></span>
```csharp
( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => v )
```

<span data-ttu-id="abcc0-2551">使用查詢運算式`join`子句`into`後面的項目以外的其他`select`子句</span><span class="sxs-lookup"><span data-stu-id="abcc0-2551">A query expression with a `join` clause with an `into` followed by something other than a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
...
```
<span data-ttu-id="abcc0-2552">轉譯成</span><span class="sxs-lookup"><span data-stu-id="abcc0-2552">is translated into</span></span>
```csharp
from * in ( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => new { x1 , g })
...
```

<span data-ttu-id="abcc0-2553">使用查詢運算式`orderby`子句</span><span class="sxs-lookup"><span data-stu-id="abcc0-2553">A query expression with an `orderby` clause</span></span>
```csharp
from x in e
orderby k1 , k2 , ..., kn
...
```
<span data-ttu-id="abcc0-2554">轉譯成</span><span class="sxs-lookup"><span data-stu-id="abcc0-2554">is translated into</span></span>
```csharp
from x in ( e ) . 
OrderBy ( x => k1 ) . 
ThenBy ( x => k2 ) .
... .
ThenBy ( x => kn )
...
```

<span data-ttu-id="abcc0-2555">如果排序子句會指定`descending`方向指標、 的引動過程`OrderByDescending`或`ThenByDescending`而產生。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2555">If an ordering clause specifies a `descending` direction indicator, an invocation of `OrderByDescending` or `ThenByDescending` is produced instead.</span></span>

<span data-ttu-id="abcc0-2556">下列轉譯是假設沒有任何`let`， `where`，`join`或`orderby`子句，而且不超過一個初始`from`中每個查詢運算式子句。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2556">The following translations assume that there are no `let`, `where`, `join` or `orderby` clauses, and no more than the one initial `from` clause in each query expression.</span></span>

<span data-ttu-id="abcc0-2557">此範例</span><span class="sxs-lookup"><span data-stu-id="abcc0-2557">The example</span></span>
```csharp
from c in customers
from o in c.Orders
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="abcc0-2558">轉譯成</span><span class="sxs-lookup"><span data-stu-id="abcc0-2558">is translated into</span></span>
```csharp
customers.
SelectMany(c => c.Orders,
     (c,o) => new { c.Name, o.OrderID, o.Total }
)
```

<span data-ttu-id="abcc0-2559">此範例</span><span class="sxs-lookup"><span data-stu-id="abcc0-2559">The example</span></span>
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="abcc0-2560">轉譯成</span><span class="sxs-lookup"><span data-stu-id="abcc0-2560">is translated into</span></span>
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="abcc0-2561">最終的翻譯</span><span class="sxs-lookup"><span data-stu-id="abcc0-2561">the final translation of which is</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.OrderID, x.o.Total })
```
<span data-ttu-id="abcc0-2562">其中`x`是編譯器產生識別碼，否則是不可見的且無法存取。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2562">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="abcc0-2563">此範例</span><span class="sxs-lookup"><span data-stu-id="abcc0-2563">The example</span></span>
```csharp
from o in orders
let t = o.Details.Sum(d => d.UnitPrice * d.Quantity)
where t >= 1000
select new { o.OrderID, Total = t }
```
<span data-ttu-id="abcc0-2564">轉譯成</span><span class="sxs-lookup"><span data-stu-id="abcc0-2564">is translated into</span></span>
```csharp
from * in orders.
    Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) })
where t >= 1000 
select new { o.OrderID, Total = t }
```
<span data-ttu-id="abcc0-2565">最終的翻譯</span><span class="sxs-lookup"><span data-stu-id="abcc0-2565">the final translation of which is</span></span>
```csharp
orders.
Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) }).
Where(x => x.t >= 1000).
Select(x => new { x.o.OrderID, Total = x.t })
```
<span data-ttu-id="abcc0-2566">其中`x`是編譯器產生識別碼，否則是不可見的且無法存取。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2566">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="abcc0-2567">此範例</span><span class="sxs-lookup"><span data-stu-id="abcc0-2567">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
select new { c.Name, o.OrderDate, o.Total }
```
<span data-ttu-id="abcc0-2568">轉譯成</span><span class="sxs-lookup"><span data-stu-id="abcc0-2568">is translated into</span></span>
```csharp
customers.Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c.Name, o.OrderDate, o.Total })
```

<span data-ttu-id="abcc0-2569">此範例</span><span class="sxs-lookup"><span data-stu-id="abcc0-2569">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID into co
let n = co.Count()
where n >= 10
select new { c.Name, OrderCount = n }
```
<span data-ttu-id="abcc0-2570">轉譯成</span><span class="sxs-lookup"><span data-stu-id="abcc0-2570">is translated into</span></span>
```csharp
from * in customers.
    GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
        (c, co) => new { c, co })
let n = co.Count()
where n >= 10 
select new { c.Name, OrderCount = n }
```
<span data-ttu-id="abcc0-2571">最終的翻譯</span><span class="sxs-lookup"><span data-stu-id="abcc0-2571">the final translation of which is</span></span>
```csharp
customers.
GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
    (c, co) => new { c, co }).
Select(x => new { x, n = x.co.Count() }).
Where(y => y.n >= 10).
Select(y => new { y.x.c.Name, OrderCount = y.n)
```
<span data-ttu-id="abcc0-2572">何處`x`和`y`是編譯器產生的識別項，否則為不可見的且無法存取。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2572">where `x` and `y` are compiler generated identifiers that are otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="abcc0-2573">此範例</span><span class="sxs-lookup"><span data-stu-id="abcc0-2573">The example</span></span>
```csharp
from o in orders
orderby o.Customer.Name, o.Total descending
select o
```
<span data-ttu-id="abcc0-2574">具有最終轉譯</span><span class="sxs-lookup"><span data-stu-id="abcc0-2574">has the final translation</span></span>
```csharp
orders.
OrderBy(o => o.Customer.Name).
ThenByDescending(o => o.Total)
```

#### <a name="select-clauses"></a><span data-ttu-id="abcc0-2575">Select 子句</span><span class="sxs-lookup"><span data-stu-id="abcc0-2575">Select clauses</span></span>

<span data-ttu-id="abcc0-2576">格式的查詢運算式</span><span class="sxs-lookup"><span data-stu-id="abcc0-2576">A query expression of the form</span></span>
```csharp
from x in e select v
```
<span data-ttu-id="abcc0-2577">轉譯成</span><span class="sxs-lookup"><span data-stu-id="abcc0-2577">is translated into</span></span>
```csharp
( e ) . Select ( x => v )
```
<span data-ttu-id="abcc0-2578">除了當 v 是識別項 x，轉譯就是</span><span class="sxs-lookup"><span data-stu-id="abcc0-2578">except when v is the identifier x, the translation is simply</span></span>
```csharp
( e )
```

<span data-ttu-id="abcc0-2579">例如</span><span class="sxs-lookup"><span data-stu-id="abcc0-2579">For example</span></span>
```csharp
from c in customers.Where(c => c.City == "London")
select c
```
<span data-ttu-id="abcc0-2580">只會轉譯成</span><span class="sxs-lookup"><span data-stu-id="abcc0-2580">is simply translated into</span></span>
```csharp
customers.Where(c => c.City == "London")
```

#### <a name="groupby-clauses"></a><span data-ttu-id="abcc0-2581">Groupby 子句</span><span class="sxs-lookup"><span data-stu-id="abcc0-2581">Groupby clauses</span></span>

<span data-ttu-id="abcc0-2582">格式的查詢運算式</span><span class="sxs-lookup"><span data-stu-id="abcc0-2582">A query expression of the form</span></span>
```csharp
from x in e group v by k
```
<span data-ttu-id="abcc0-2583">轉譯成</span><span class="sxs-lookup"><span data-stu-id="abcc0-2583">is translated into</span></span>
```csharp
( e ) . GroupBy ( x => k , x => v )
```
<span data-ttu-id="abcc0-2584">除了當 v 是識別項 x，翻譯就是</span><span class="sxs-lookup"><span data-stu-id="abcc0-2584">except when v is the identifier x, the translation is</span></span>
```csharp
( e ) . GroupBy ( x => k )
```

<span data-ttu-id="abcc0-2585">此範例</span><span class="sxs-lookup"><span data-stu-id="abcc0-2585">The example</span></span>
```csharp
from c in customers
group c.Name by c.Country
```
<span data-ttu-id="abcc0-2586">轉譯成</span><span class="sxs-lookup"><span data-stu-id="abcc0-2586">is translated into</span></span>
```csharp
customers.
GroupBy(c => c.Country, c => c.Name)
```

#### <a name="transparent-identifiers"></a><span data-ttu-id="abcc0-2587">透明識別項</span><span class="sxs-lookup"><span data-stu-id="abcc0-2587">Transparent identifiers</span></span>

<span data-ttu-id="abcc0-2588">特定翻譯插入具有範圍變數***透明識別項***所表示`*`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2588">Certain translations inject range variables with ***transparent identifiers*** denoted by `*`.</span></span> <span data-ttu-id="abcc0-2589">透明識別項而不是適當的語言功能;它們只能以查詢運算式轉譯程序的中繼步驟形式存在。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2589">Transparent identifiers are not a proper language feature; they exist only as an intermediate step in the query expression translation process.</span></span>

<span data-ttu-id="abcc0-2590">當查詢轉譯會插入透明的識別碼時，進一步翻譯步驟將傳播透明的識別項到匿名函式和匿名物件初始設定式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2590">When a query translation injects a transparent identifier, further translation steps propagate the transparent identifier into anonymous functions and anonymous object initializers.</span></span> <span data-ttu-id="abcc0-2591">在這些內容中，透明識別項會有下列行為：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2591">In those contexts, transparent identifiers have the following behavior:</span></span>

*  <span data-ttu-id="abcc0-2592">當做匿名函式中的參數是透明的識別項時，相關聯的匿名型別的成員會自動在範圍內的匿名函式主體中。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2592">When a transparent identifier occurs as a parameter in an anonymous function, the members of the associated anonymous type are automatically in scope in the body of the anonymous function.</span></span>
*  <span data-ttu-id="abcc0-2593">在範圍內的成員具有透明的識別碼時，該成員的成員是在範圍內以及。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2593">When a member with a transparent identifier is in scope, the members of that member are in scope as well.</span></span>
*  <span data-ttu-id="abcc0-2594">為匿名物件初始設定式中的成員宣告子是透明的識別項時，它引入了具有透明識別項的成員。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2594">When a transparent identifier occurs as a member declarator in an anonymous object initializer, it introduces a member with a transparent identifier.</span></span>
*  <span data-ttu-id="abcc0-2595">在上面所述的轉換步驟，透明的識別碼會永遠引進了以及匿名型別，目的是擷取多個範圍變數做為單一物件的成員。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2595">In the translation steps described above, transparent identifiers are always introduced together with anonymous types, with the intent of capturing multiple range variables as members of a single object.</span></span> <span data-ttu-id="abcc0-2596">允許使用不同的機制，比匿名型別群組在一起的多個範圍變數的 C# 實作。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2596">An implementation of C# is permitted to use a different mechanism than anonymous types to group together multiple range variables.</span></span> <span data-ttu-id="abcc0-2597">下列的轉譯範例假設使用時，匿名型別，以及顯示如何透明識別項可以立即轉譯。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2597">The following translation examples assume that anonymous types are used, and show how transparent identifiers can be translated away.</span></span>

<span data-ttu-id="abcc0-2598">此範例</span><span class="sxs-lookup"><span data-stu-id="abcc0-2598">The example</span></span>
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.Total }
```
<span data-ttu-id="abcc0-2599">轉譯成</span><span class="sxs-lookup"><span data-stu-id="abcc0-2599">is translated into</span></span>
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.Total }
```

<span data-ttu-id="abcc0-2600">然後再轉譯成</span><span class="sxs-lookup"><span data-stu-id="abcc0-2600">which is further translated into</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(* => o.Total).
Select(* => new { c.Name, o.Total })
```
<span data-ttu-id="abcc0-2601">當透明的識別碼就會清除，相當於</span><span class="sxs-lookup"><span data-stu-id="abcc0-2601">which, when transparent identifiers are erased, is equivalent to</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.Total })
```
<span data-ttu-id="abcc0-2602">其中`x`是編譯器產生識別碼，否則是不可見的且無法存取。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2602">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="abcc0-2603">此範例</span><span class="sxs-lookup"><span data-stu-id="abcc0-2603">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
<span data-ttu-id="abcc0-2604">轉譯成</span><span class="sxs-lookup"><span data-stu-id="abcc0-2604">is translated into</span></span>
```csharp
from * in customers.
    Join(orders, c => c.CustomerID, o => o.CustomerID, 
        (c, o) => new { c, o })
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
<span data-ttu-id="abcc0-2605">這會進一步縮減為</span><span class="sxs-lookup"><span data-stu-id="abcc0-2605">which is further reduced to</span></span>
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID, (c, o) => new { c, o }).
Join(details, * => o.OrderID, d => d.OrderID, (*, d) => new { *, d }).
Join(products, * => d.ProductID, p => p.ProductID, (*, p) => new { *, p }).
Select(* => new { c.Name, o.OrderDate, p.ProductName })
```
<span data-ttu-id="abcc0-2606">最終的翻譯</span><span class="sxs-lookup"><span data-stu-id="abcc0-2606">the final translation of which is</span></span>
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
<span data-ttu-id="abcc0-2607">何處`x`， `y`，和`z`是編譯器產生的識別項，否則為不可見的且無法存取。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2607">where `x`, `y`, and `z` are compiler generated identifiers that are otherwise invisible and inaccessible.</span></span>

### <a name="the-query-expression-pattern"></a><span data-ttu-id="abcc0-2608">查詢運算式模式</span><span class="sxs-lookup"><span data-stu-id="abcc0-2608">The query expression pattern</span></span>

<span data-ttu-id="abcc0-2609">***查詢運算式模式***建立模式的類型可以實作以支援查詢運算式的方法。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2609">The ***Query expression pattern*** establishes a pattern of methods that types can implement to support query expressions.</span></span> <span data-ttu-id="abcc0-2610">查詢運算式會轉譯為方法引動過程中，透過語法的對應，因為類型會有極大的彈性，在實作查詢運算式模式的方式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2610">Because query expressions are translated to method invocations by means of a syntactic mapping, types have considerable flexibility in how they implement the query expression pattern.</span></span> <span data-ttu-id="abcc0-2611">比方說，模式的方法可實作為執行個體方法或擴充方法因為這兩個具有相同的引動過程語法中，而且方法可以要求委派或運算式樹狀架構，因為匿名函式會轉換成這兩者。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2611">For example, the methods of the pattern can be implemented as instance methods or as extension methods because the two have the same invocation syntax, and the methods can request delegates or expression trees because anonymous functions are convertible to both.</span></span>

<span data-ttu-id="abcc0-2612">泛型類型的建議的圖形`C<T>`，支援的查詢運算式模式如下所示。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2612">The recommended shape of a generic type `C<T>` that supports the query expression pattern is shown below.</span></span> <span data-ttu-id="abcc0-2613">泛型型別用來說明之間參數和結果的型別，適當的關聯性，但可以實作非泛型型別模式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2613">A generic type is used in order to illustrate the proper relationships between parameter and result types, but it is possible to implement the pattern for non-generic types as well.</span></span>

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

<span data-ttu-id="abcc0-2614">上述方法使用泛型委派類型`Func<T1,R>`和`Func<T1,T2,R>`，但它們可能也同樣使用過其他委派或運算式樹狀架構型別參數和結果的類型中相同的關聯性。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2614">The methods above use the generic delegate types `Func<T1,R>` and `Func<T1,T2,R>`, but they could equally well have used other delegate or expression tree types with the same relationships in parameter and result types.</span></span>

<span data-ttu-id="abcc0-2615">請注意建議的關聯性之間`C<T>`和`O<T>`可確保`ThenBy`和`ThenByDescending`方法都只位於結果`OrderBy`或`OrderByDescending`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2615">Notice the recommended relationship between `C<T>` and `O<T>` which ensures that the `ThenBy` and `ThenByDescending` methods are available only on the result of an `OrderBy` or `OrderByDescending`.</span></span> <span data-ttu-id="abcc0-2616">也請注意結果的建議的圖形`GroupBy`-一連串的序列，其中每個內部序列都有額外`Key`屬性。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2616">Also notice the recommended shape of the result of `GroupBy` -- a sequence of sequences, where each inner sequence has an additional `Key` property.</span></span>

<span data-ttu-id="abcc0-2617">`System.Linq`命名空間會實作任何類型提供的查詢運算子的模式實作`System.Collections.Generic.IEnumerable<T>`介面。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2617">The `System.Linq` namespace provides an implementation of the query operator pattern for any type that implements the `System.Collections.Generic.IEnumerable<T>` interface.</span></span>

## <a name="assignment-operators"></a><span data-ttu-id="abcc0-2618">指派運算子</span><span class="sxs-lookup"><span data-stu-id="abcc0-2618">Assignment operators</span></span>

<span data-ttu-id="abcc0-2619">指派運算子會將新的值指派給變數、 屬性、 事件或索引子項目。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2619">The assignment operators assign a new value to a variable, a property, an event, or an indexer element.</span></span>

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

<span data-ttu-id="abcc0-2620">指派的左的運算元必須是運算式分類為變數、 屬性存取、 索引子存取或事件存取。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2620">The left operand of an assignment must be an expression classified as a variable, a property access, an indexer access, or an event access.</span></span>

<span data-ttu-id="abcc0-2621">`=`呼叫運算子***簡單指派運算子***。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2621">The `=` operator is called the ***simple assignment operator***.</span></span> <span data-ttu-id="abcc0-2622">它會將右運算元的值指派給左運算元所指定的變數、 屬性或索引子的項目中。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2622">It assigns the value of the right operand to the variable, property, or indexer element given by the left operand.</span></span> <span data-ttu-id="abcc0-2623">簡單指派運算子的左的運算元可能無法存取事件 (除了中所述[欄位型事件](classes.md#field-like-events))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2623">The left operand of the simple assignment operator may not be an event access (except as described in [Field-like events](classes.md#field-like-events)).</span></span> <span data-ttu-id="abcc0-2624">簡單指派運算子所述[簡單指派](expressions.md#simple-assignment)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2624">The simple assignment operator is described in [Simple assignment](expressions.md#simple-assignment).</span></span>

<span data-ttu-id="abcc0-2625">指派運算子以外`=`運算子會呼叫***複合指派運算子***。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2625">The assignment operators other than the `=` operator are called the ***compound assignment operators***.</span></span> <span data-ttu-id="abcc0-2626">這些運算子會執行兩個運算元中，指定的作業，並再將產生的值指派給左運算元所指定的變數、 屬性或索引子的項目。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2626">These operators perform the indicated operation on the two operands, and then assign the resulting value to the variable, property, or indexer element given by the left operand.</span></span> <span data-ttu-id="abcc0-2627">複合指派運算子所述[複合指派](expressions.md#compound-assignment)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2627">The compound assignment operators are described in [Compound assignment](expressions.md#compound-assignment).</span></span>

<span data-ttu-id="abcc0-2628">`+=`並`-=`事件存取運算式為左運算元的運算子都稱為*事件指派運算子*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2628">The `+=` and `-=` operators with an event access expression as the left operand are called the *event assignment operators*.</span></span> <span data-ttu-id="abcc0-2629">沒有其他的指派運算子是與事件存取有效的做為左運算元。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2629">No other assignment operator is valid with an event access as the left operand.</span></span> <span data-ttu-id="abcc0-2630">事件指派運算子所述[事件指派](expressions.md#event-assignment)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2630">The event assignment operators are described in [Event assignment](expressions.md#event-assignment).</span></span>

<span data-ttu-id="abcc0-2631">指派運算子是右向關聯，表示由右至左的分組作業。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2631">The assignment operators are right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="abcc0-2632">例如，以下格式的運算式`a = b = c`評估為`a = (b = c)`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2632">For example, an expression of the form `a = b = c` is evaluated as `a = (b = c)`.</span></span>

### <a name="simple-assignment"></a><span data-ttu-id="abcc0-2633">單一指派</span><span class="sxs-lookup"><span data-stu-id="abcc0-2633">Simple assignment</span></span>

<span data-ttu-id="abcc0-2634">`=`運算子稱為簡單指派運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2634">The `=` operator is called the simple assignment operator.</span></span>

<span data-ttu-id="abcc0-2635">簡單指派的左的運算元是否屬於表單`E.P`或`E[Ei]`其中`E`已編譯時期型別`dynamic`，然後指派動態繫結 ([動態繫結](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2635">If the left operand of a simple assignment is of the form `E.P` or `E[Ei]` where `E` has the compile-time type `dynamic`, then the assignment is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="abcc0-2636">在此案例是指派運算式的編譯時期型別`dynamic`，並如下所述的解析度會在執行階段根據執行階段類型的進行`E`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2636">In this case the compile-time type of the assignment expression is `dynamic`, and the resolution described below will take place at run-time based on the run-time type of `E`.</span></span>

<span data-ttu-id="abcc0-2637">在簡單的指派，右運算元必須隱含轉換為左運算元的類型的運算式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2637">In a simple assignment, the right operand must be an expression that is implicitly convertible to the type of the left operand.</span></span> <span data-ttu-id="abcc0-2638">作業會將右運算元的值指派給左運算元所指定的變數、 屬性或索引子的項目。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2638">The operation assigns the value of the right operand to the variable, property, or indexer element given by the left operand.</span></span>

<span data-ttu-id="abcc0-2639">簡單指派運算式的結果會指派給左運算元的值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2639">The result of a simple assignment expression is the value assigned to the left operand.</span></span> <span data-ttu-id="abcc0-2640">結果會具有左運算元相同的型別，而且一律會分類為值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2640">The result has the same type as the left operand and is always classified as a value.</span></span>

<span data-ttu-id="abcc0-2641">如果左的運算元是屬性或索引子的存取，必須有屬性或索引子`set`存取子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2641">If the left operand is a property or indexer access, the property or indexer must have a `set` accessor.</span></span> <span data-ttu-id="abcc0-2642">如果這不是這樣，繫結階段會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2642">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="abcc0-2643">執行階段處理表單的簡單指派`x = y`包含下列步驟：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2643">The run-time processing of a simple assignment of the form `x = y` consists of the following steps:</span></span>

*  <span data-ttu-id="abcc0-2644">如果`x`分類為變數：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2644">If `x` is classified as a variable:</span></span>
   * <span data-ttu-id="abcc0-2645">`x` 在評估後產生的變數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2645">`x` is evaluated to produce the variable.</span></span>
   * <span data-ttu-id="abcc0-2646">`y` 是評估後，如有需要，轉換成的型別`x`透過隱含的轉換 ([隱含轉換](conversions.md#implicit-conversions))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2646">`y` is evaluated and, if required, converted to the type of `x` through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
   * <span data-ttu-id="abcc0-2647">如果所指定變數`x`是陣列項目*reference_type*，則執行階段會檢查以確保針對計算的值`y`適用於陣列執行個體，其中`x`是項目。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2647">If the variable given by `x` is an array element of a *reference_type*, a run-time check is performed to ensure that the value computed for `y` is compatible with the array instance of which `x` is an element.</span></span> <span data-ttu-id="abcc0-2648">檢查成功，如果`y`已`null`，或如果隱含參考轉換 ([隱含參考轉換](conversions.md#implicit-reference-conversions)) 執行個體所參考的實際型別`y`實際的項目型別陣列執行個體，包含`x`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2648">The check succeeds if `y` is `null`, or if an implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from the actual type of the instance referenced by `y` to the actual element type of the array instance containing `x`.</span></span> <span data-ttu-id="abcc0-2649">否則會擲回 `System.ArrayTypeMismatchException`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2649">Otherwise, a `System.ArrayTypeMismatchException` is thrown.</span></span>
   * <span data-ttu-id="abcc0-2650">評估和轉換所產生的值`y`的評估所指定的位置儲存`x`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2650">The value resulting from the evaluation and conversion of `y` is stored into the location given by the evaluation of `x`.</span></span>
*  <span data-ttu-id="abcc0-2651">如果`x`歸類為屬性或索引子的存取：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2651">If `x` is classified as a property or indexer access:</span></span>
   * <span data-ttu-id="abcc0-2652">執行個體運算式 (如果`x`不是`static`) 和引數清單 (如果`x`是索引子存取) 相關聯`x`會進行評估，而且結果會用於後續`set`存取子引動過程。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2652">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `set` accessor invocation.</span></span>
   * <span data-ttu-id="abcc0-2653">`y` 是評估後，如有需要，轉換成的型別`x`透過隱含的轉換 ([隱含轉換](conversions.md#implicit-conversions))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2653">`y` is evaluated and, if required, converted to the type of `x` through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
   * <span data-ttu-id="abcc0-2654">`set`存取子`x`叫用的計算值`y`做為其`value`引數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2654">The `set` accessor of `x` is invoked with the value computed for `y` as its `value` argument.</span></span>

<span data-ttu-id="abcc0-2655">陣列共同變異數規則 ([陣列共變數](arrays.md#array-covariance)) 允許值的陣列型別`A[]`是陣列型別的執行個體的參考`B[]`，前提是隱含參考轉換存在從`B`至`A`.</span><span class="sxs-lookup"><span data-stu-id="abcc0-2655">The array co-variance rules ([Array covariance](arrays.md#array-covariance)) permit a value of an array type `A[]` to be a reference to an instance of an array type `B[]`, provided an implicit reference conversion exists from `B` to `A`.</span></span> <span data-ttu-id="abcc0-2656">因為這些規則中，指派的陣列項目的*reference_type*需要執行階段檢查，以確保相容於陣列執行個體被指派的值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2656">Because of these rules, assignment to an array element of a *reference_type* requires a run-time check to ensure that the value being assigned is compatible with the array instance.</span></span> <span data-ttu-id="abcc0-2657">在範例</span><span class="sxs-lookup"><span data-stu-id="abcc0-2657">In the example</span></span>
```csharp
string[] sa = new string[10];
object[] oa = sa;

oa[0] = null;               // Ok
oa[1] = "Hello";            // Ok
oa[2] = new ArrayList();    // ArrayTypeMismatchException
```
<span data-ttu-id="abcc0-2658">過去的指派會導致`System.ArrayTypeMismatchException`因為擲回的執行個體`ArrayList`中的項目無法儲存`string[]`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2658">the last assignment causes a `System.ArrayTypeMismatchException` to be thrown because an instance of `ArrayList` cannot be stored in an element of a `string[]`.</span></span>

<span data-ttu-id="abcc0-2659">當屬性或索引子宣告中*struct_type*指派的目標，執行個體運算式相關聯的屬性或索引子存取必須分類為變數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2659">When a property or indexer declared in a *struct_type* is the target of an assignment, the instance expression associated with the property or indexer access must be classified as a variable.</span></span> <span data-ttu-id="abcc0-2660">如果執行個體運算式分類為值，繫結階段會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2660">If the instance expression is classified as a value, a binding-time error occurs.</span></span> <span data-ttu-id="abcc0-2661">由於[成員存取](expressions.md#member-access)，相同的規則也套用至欄位。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2661">Because of [Member access](expressions.md#member-access), the same rule also applies to fields.</span></span>

<span data-ttu-id="abcc0-2662">宣告為例：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2662">Given the declarations:</span></span>
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
<span data-ttu-id="abcc0-2663">在範例</span><span class="sxs-lookup"><span data-stu-id="abcc0-2663">in the example</span></span>
```csharp
Point p = new Point();
p.X = 100;
p.Y = 100;
Rectangle r = new Rectangle();
r.A = new Point(10, 10);
r.B = p;
```
<span data-ttu-id="abcc0-2664">若要指派`p.X`， `p.Y`， `r.A`，以及`r.B`已允許，因為`p`和`r`是變數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2664">the assignments to `p.X`, `p.Y`, `r.A`, and `r.B` are permitted because `p` and `r` are variables.</span></span> <span data-ttu-id="abcc0-2665">不過，在此範例</span><span class="sxs-lookup"><span data-stu-id="abcc0-2665">However, in the example</span></span>
```csharp
Rectangle r = new Rectangle();
r.A.X = 10;
r.A.Y = 10;
r.B.X = 100;
r.B.Y = 100;
```
<span data-ttu-id="abcc0-2666">指派的設定全部無效，因為`r.A`和`r.B`並不是變數。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2666">the assignments are all invalid, since `r.A` and `r.B` are not variables.</span></span>

### <a name="compound-assignment"></a><span data-ttu-id="abcc0-2667">複合指派</span><span class="sxs-lookup"><span data-stu-id="abcc0-2667">Compound assignment</span></span>

<span data-ttu-id="abcc0-2668">複合指派的左的運算元是否屬於表單`E.P`或`E[Ei]`其中`E`已編譯時期型別`dynamic`，然後指派動態繫結 ([動態繫結](expressions.md#dynamic-binding))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2668">If the left operand of a compound assignment is of the form `E.P` or `E[Ei]` where `E` has the compile-time type `dynamic`, then the assignment is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="abcc0-2669">在此案例是指派運算式的編譯時期型別`dynamic`，並如下所述的解析度會在執行階段根據執行階段類型的進行`E`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2669">In this case the compile-time type of the assignment expression is `dynamic`, and the resolution described below will take place at run-time based on the run-time type of `E`.</span></span>

<span data-ttu-id="abcc0-2670">表單的作業`x op= y`處理所套用的二元運算子多載解析 ([二元運算子多載解析](expressions.md#binary-operator-overload-resolution))，如果作業寫入`x op y`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2670">An operation of the form `x op= y` is processed by applying binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) as if the operation was written `x op y`.</span></span> <span data-ttu-id="abcc0-2671">然後，</span><span class="sxs-lookup"><span data-stu-id="abcc0-2671">Then,</span></span>

*  <span data-ttu-id="abcc0-2672">如果選取之運算子的傳回型別是隱含地轉換成的型別`x`，作業評估為`x = x op y`，差異在於`x`只評估一次。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2672">If the return type of the selected operator is implicitly convertible to the type of `x`, the operation is evaluated as `x = x op y`, except that `x` is evaluated only once.</span></span>
*  <span data-ttu-id="abcc0-2673">否則，如果選取的運算子是一個預先定義的運算子，明確地轉換成的型別所選運算子的傳回型別是否`x`，而且如果`y`隱含地轉換成的型別`x`或操作員移位運算子，則作業會評估為`x = (T)(x op y)`，其中`T`是種`x`，只不過`x`只評估一次。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2673">Otherwise, if the selected operator is a predefined operator, if the return type of the selected operator is explicitly convertible to the type of `x`, and if `y` is implicitly convertible to the type of `x` or the operator is a shift operator, then the operation is evaluated as `x = (T)(x op y)`, where `T` is the type of `x`, except that `x` is evaluated only once.</span></span>
*  <span data-ttu-id="abcc0-2674">否則，複合指派無效，並在繫結階段錯誤發生。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2674">Otherwise, the compound assignment is invalid, and a binding-time error occurs.</span></span>

<span data-ttu-id="abcc0-2675">「 只評估一次 」 一詞是指，在評估的`x op y`，任何構成運算式中的結果`x`會暫時儲存並重複使用時執行指派給`x`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2675">The term "evaluated only once" means that in the evaluation of `x op y`, the results of any constituent expressions of `x` are temporarily saved and then reused when performing the assignment to `x`.</span></span> <span data-ttu-id="abcc0-2676">比方說，在 [指派] `A()[B()] += C()`，其中`A`是方法，傳回`int[]`，和`B`並`C`方法傳回`int`，在順序中的 「 方法叫用一次， `A`，`B`, `C`.</span><span class="sxs-lookup"><span data-stu-id="abcc0-2676">For example, in the assignment `A()[B()] += C()`, where `A` is a method returning `int[]`, and `B` and `C` are methods returning `int`, the methods are invoked only once, in the order `A`, `B`, `C`.</span></span>

<span data-ttu-id="abcc0-2677">複合指派的左的運算元是存取屬性或索引子存取，當屬性或索引子必須兩者`get`存取子和`set`存取子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2677">When the left operand of a compound assignment is a property access or indexer access, the property or indexer must have both a `get` accessor and a `set` accessor.</span></span> <span data-ttu-id="abcc0-2678">如果這不是這樣，繫結階段會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2678">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="abcc0-2679">第二項規則允許上述`x op= y`評估為`x = (T)(x op y)`在特定內容中。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2679">The second rule above permits `x op= y` to be evaluated as `x = (T)(x op y)` in certain contexts.</span></span> <span data-ttu-id="abcc0-2680">預先定義的運算子可用來當做複合運算子的左的運算元的類型時，有規則`sbyte`， `byte`， `short`， `ushort`，或`char`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2680">The rule exists such that the predefined operators can be used as compound operators when the left operand is of type `sbyte`, `byte`, `short`, `ushort`, or `char`.</span></span> <span data-ttu-id="abcc0-2681">即使兩個引數的其中一種類型，預先定義的運算子會產生類型的結果`int`中所述[二進位數字提升](expressions.md#binary-numeric-promotions)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2681">Even when both arguments are of one of those types, the predefined operators produce a result of type `int`, as described in [Binary numeric promotions](expressions.md#binary-numeric-promotions).</span></span> <span data-ttu-id="abcc0-2682">因此，未使用轉型其就無法將結果指派給左運算元。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2682">Thus, without a cast it would not be possible to assign the result to the left operand.</span></span>

<span data-ttu-id="abcc0-2683">預先定義的運算子規則的直覺式作用在於只要`x op= y`如果兩個允許的`x op y`和`x = y`允許。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2683">The intuitive effect of the rule for predefined operators is simply that `x op= y` is permitted if both of `x op y` and `x = y` are permitted.</span></span> <span data-ttu-id="abcc0-2684">在範例</span><span class="sxs-lookup"><span data-stu-id="abcc0-2684">In the example</span></span>
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
<span data-ttu-id="abcc0-2685">針對每個錯誤的直覺化的原因是，對應的簡單指派也會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2685">the intuitive reason for each error is that a corresponding simple assignment would also have been an error.</span></span>

<span data-ttu-id="abcc0-2686">這也表示複合指派作業支援提昇的作業。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2686">This also means that compound assignment operations support lifted operations.</span></span> <span data-ttu-id="abcc0-2687">在範例</span><span class="sxs-lookup"><span data-stu-id="abcc0-2687">In the example</span></span>
```csharp
int? i = 0;
i += 1;             // Ok
```
<span data-ttu-id="abcc0-2688">提昇的運算子`+(int?,int?)`用。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2688">the lifted operator `+(int?,int?)` is used.</span></span>

### <a name="event-assignment"></a><span data-ttu-id="abcc0-2689">事件指派</span><span class="sxs-lookup"><span data-stu-id="abcc0-2689">Event assignment</span></span>

<span data-ttu-id="abcc0-2690">如果左的運算元`+=`或`-=`運算子會分類為事件的存取，則運算式會評估，如下所示：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2690">If the left operand of a `+=` or `-=` operator is classified as an event access, then the expression is evaluated as follows:</span></span>

*  <span data-ttu-id="abcc0-2691">執行個體，如果有的話，事件存取會評估運算式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2691">The instance expression, if any, of the event access is evaluated.</span></span>
*  <span data-ttu-id="abcc0-2692">右運算元`+=`或是`-=`運算子，評估，再，如有需要，轉換成透過隱含的轉換的左運算元的類型 ([隱含轉換](conversions.md#implicit-conversions))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2692">The right operand of the `+=` or `-=` operator is evaluated, and, if required, converted to the type of the left operand through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
*  <span data-ttu-id="abcc0-2693">叫用事件的事件存取子時，具有組成的右運算元，評估後的引數清單，並視需要轉換。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2693">An event accessor of the event is invoked, with argument list consisting of the right operand, after evaluation and, if necessary, conversion.</span></span> <span data-ttu-id="abcc0-2694">如果運算子`+=`，則`add`存取子會叫用; 如果運算子`-=`，則`remove`存取子會叫用。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2694">If the operator was `+=`, the `add` accessor is invoked; if the operator was `-=`, the `remove` accessor is invoked.</span></span>

<span data-ttu-id="abcc0-2695">事件指派運算式不會產生值。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2695">An event assignment expression does not yield a value.</span></span> <span data-ttu-id="abcc0-2696">因此，事件指派運算式是有效的內容中，只有*statement_expression* ([運算式陳述式](statements.md#expression-statements))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2696">Thus, an event assignment expression is valid only in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>

## <a name="expression"></a><span data-ttu-id="abcc0-2697">運算式</span><span class="sxs-lookup"><span data-stu-id="abcc0-2697">Expression</span></span>

<span data-ttu-id="abcc0-2698">*運算式*是*non_assignment_expression*或是*指派*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2698">An *expression* is either a *non_assignment_expression* or an *assignment*.</span></span>

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

## <a name="constant-expressions"></a><span data-ttu-id="abcc0-2699">常數運算式</span><span class="sxs-lookup"><span data-stu-id="abcc0-2699">Constant expressions</span></span>

<span data-ttu-id="abcc0-2700">A *constant_expression*是可以在編譯時期完整評估的運算式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2700">A *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span>

```antlr
constant_expression
    : expression
    ;
```

<span data-ttu-id="abcc0-2701">必須是常數運算式`null`常值或具有下列一種類型的值： `sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char``float`， `double`， `decimal`， `bool`， `object`， `string`，或任何列舉類型。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2701">A constant expression must be the `null` literal or a value with one of  the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `object`, `string`, or any enumeration type.</span></span> <span data-ttu-id="abcc0-2702">在常數運算式中允許下列建構：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2702">Only the following constructs are permitted in constant expressions:</span></span>

*  <span data-ttu-id="abcc0-2703">常值 (包括`null`常值)。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2703">Literals (including the `null` literal).</span></span>
*  <span data-ttu-id="abcc0-2704">若要參考`const`類別和結構類型的成員。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2704">References to `const` members of class and struct types.</span></span>
*  <span data-ttu-id="abcc0-2705">列舉型別之成員的參考。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2705">References to members of enumeration types.</span></span>
*  <span data-ttu-id="abcc0-2706">若要參考`const`參數或區域變數</span><span class="sxs-lookup"><span data-stu-id="abcc0-2706">References to `const` parameters or local variables</span></span>
*  <span data-ttu-id="abcc0-2707">括號括住的子運算式，也就是本身為常數運算式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2707">Parenthesized sub-expressions, which are themselves constant expressions.</span></span>
*  <span data-ttu-id="abcc0-2708">轉型運算式，提供目標型別是上面所列類型之一。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2708">Cast expressions, provided the target type is one of the types listed above.</span></span>
*  <span data-ttu-id="abcc0-2709">`checked` 和`unchecked`運算式</span><span class="sxs-lookup"><span data-stu-id="abcc0-2709">`checked` and `unchecked` expressions</span></span>
*  <span data-ttu-id="abcc0-2710">預設值運算式</span><span class="sxs-lookup"><span data-stu-id="abcc0-2710">Default value expressions</span></span>
*  <span data-ttu-id="abcc0-2711">Nameof 運算式</span><span class="sxs-lookup"><span data-stu-id="abcc0-2711">Nameof expressions</span></span>
*  <span data-ttu-id="abcc0-2712">預先定義`+`， `-`， `!`，和`~`一元運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2712">The predefined `+`, `-`, `!`, and `~` unary operators.</span></span>
*  <span data-ttu-id="abcc0-2713">預先定義`+`， `-`， `*`， `/`， `%`， `<<`， `>>`， `&`， `|`， `^`， `&&`， `||`， `==`， `!=`， `<`， `>`， `<=`，和`>=`二元運算子，可讓您提供每一個運算元是上面所列的型別。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2713">The predefined `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, `&&`, `||`, `==`, `!=`, `<`, `>`, `<=`, and `>=` binary operators, provided each operand is of a type listed above.</span></span>
*  <span data-ttu-id="abcc0-2714">`?:`條件運算子。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2714">The `?:` conditional operator.</span></span>

<span data-ttu-id="abcc0-2715">在常數運算式中允許下列轉換：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2715">The following conversions are permitted in constant expressions:</span></span>

*  <span data-ttu-id="abcc0-2716">身分識別轉換</span><span class="sxs-lookup"><span data-stu-id="abcc0-2716">Identity conversions</span></span>
*  <span data-ttu-id="abcc0-2717">數值轉換</span><span class="sxs-lookup"><span data-stu-id="abcc0-2717">Numeric conversions</span></span>
*  <span data-ttu-id="abcc0-2718">列舉型別轉換</span><span class="sxs-lookup"><span data-stu-id="abcc0-2718">Enumeration conversions</span></span>
*  <span data-ttu-id="abcc0-2719">常數運算式轉換</span><span class="sxs-lookup"><span data-stu-id="abcc0-2719">Constant expression conversions</span></span>
*  <span data-ttu-id="abcc0-2720">提供轉換的來源是常數運算式評估為 null 值的隱含和明確參考轉換。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2720">Implicit and explicit reference conversions, provided that the source of the conversions is a constant expression that evaluates to the null value.</span></span>

<span data-ttu-id="abcc0-2721">其他轉換，包括 boxing，常數運算式中不允許非 null 值 unboxing 和隱含參考轉換。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2721">Other conversions including boxing, unboxing and implicit reference conversions of non-null values are not permitted in constant expressions.</span></span> <span data-ttu-id="abcc0-2722">例如: </span><span class="sxs-lookup"><span data-stu-id="abcc0-2722">For example:</span></span>
```csharp
class C {
    const object i = 5;         // error: boxing conversion not permitted
    const object str = "hello"; // error: implicit reference conversion
}
```
<span data-ttu-id="abcc0-2723">初始化 i 是錯誤，因為 boxing 轉換為必要項。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2723">the initialization of i is an error because a boxing conversion is required.</span></span> <span data-ttu-id="abcc0-2724">Str 的初始化是錯誤，因為從非 null 值的隱含參考轉換為必要項。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2724">The initialization of str is an error because an implicit reference conversion from a non-null value is required.</span></span>

<span data-ttu-id="abcc0-2725">每當運算式，可滿足上述需求，是在編譯時期評估運算式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2725">Whenever an expression fulfills the requirements listed above, the expression is evaluated at compile-time.</span></span> <span data-ttu-id="abcc0-2726">這是 true，即使運算式是包含非常數的建構較大運算式的子運算式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2726">This is true even if the expression is a sub-expression of a larger expression that contains non-constant constructs.</span></span>

<span data-ttu-id="abcc0-2727">常數運算式的編譯時期評估為非常數運算式，執行階段評估使用相同的規則，不同之處在於其中的執行階段評估就會擲出例外狀況，評估編譯時間會導致發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2727">The compile-time evaluation of constant expressions uses the same rules as run-time evaluation of non-constant expressions, except that where run-time evaluation would have thrown an exception, compile-time evaluation causes a compile-time error to occur.</span></span>

<span data-ttu-id="abcc0-2728">除非常數運算式明確放入`unchecked`內容、 發生編譯時期評估的運算式一律在整數型別算術運算和轉換的溢位會造成編譯時期錯誤 ([常數運算式](expressions.md#constant-expressions))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2728">Unless a constant expression is explicitly placed in an `unchecked` context, overflows that occur in integral-type arithmetic operations and conversions during the compile-time evaluation of the expression always cause compile-time errors ([Constant expressions](expressions.md#constant-expressions)).</span></span>

<span data-ttu-id="abcc0-2729">常數運算式可以出現在下面所列的內容。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2729">Constant expressions occur in the contexts listed below.</span></span> <span data-ttu-id="abcc0-2730">在這些內容中，如果運算式不能在編譯時期完整評估，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2730">In these contexts, a compile-time error occurs if an expression cannot be fully evaluated at compile-time.</span></span>

*  <span data-ttu-id="abcc0-2731">常數宣告 ([常數](classes.md#constants))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2731">Constant declarations ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="abcc0-2732">列舉型別成員宣告 ([列舉成員](enums.md#enum-members))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2732">Enumeration member declarations ([Enum members](enums.md#enum-members)).</span></span>
*  <span data-ttu-id="abcc0-2733">預設引數的型式參數清單 ([方法參數](classes.md#method-parameters))</span><span class="sxs-lookup"><span data-stu-id="abcc0-2733">Default arguments of formal parameter lists ([Method parameters](classes.md#method-parameters))</span></span>
*  <span data-ttu-id="abcc0-2734">`case` 標籤`switch`陳述式 ([switch 陳述式](statements.md#the-switch-statement))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2734">`case` labels of a `switch` statement ([The switch statement](statements.md#the-switch-statement)).</span></span>
*  <span data-ttu-id="abcc0-2735">`goto case` 陳述式 ([goto 陳述式](statements.md#the-goto-statement))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2735">`goto case` statements ([The goto statement](statements.md#the-goto-statement)).</span></span>
*  <span data-ttu-id="abcc0-2736">維度中陣列建立運算式的長度 ([陣列建立運算式](expressions.md#array-creation-expressions))，其中包含初始設定式。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2736">Dimension lengths in an array creation expression ([Array creation expressions](expressions.md#array-creation-expressions)) that includes an initializer.</span></span>
*  <span data-ttu-id="abcc0-2737">屬性 ([屬性](attributes.md))。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2737">Attributes ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="abcc0-2738">隱含的常數運算式轉換 ([隱含的常數運算式轉換](conversions.md#implicit-constant-expression-conversions)) 允許類型的常數運算式`int`轉換成`sbyte`， `byte`， `short`， `ushort`， `uint`，或`ulong`，前提是常數運算式的值是目的地類型的範圍內。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2738">An implicit constant expression conversion ([Implicit constant expression conversions](conversions.md#implicit-constant-expression-conversions)) permits a constant expression of type `int` to be converted to `sbyte`, `byte`, `short`, `ushort`, `uint`, or `ulong`, provided the value of the constant expression is within the range of the destination type.</span></span>

## <a name="boolean-expressions"></a><span data-ttu-id="abcc0-2739">布林運算式</span><span class="sxs-lookup"><span data-stu-id="abcc0-2739">Boolean expressions</span></span>

<span data-ttu-id="abcc0-2740">A *boolean_expression*是運算式，產生的結果為類型`bool`; 可直接或透過應用程式的`operator true`在下列中所指定的特定內容中。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2740">A *boolean_expression* is an expression that yields a result of type `bool`; either directly or through application of `operator true` in certain contexts as specified in the following.</span></span>

```antlr
boolean_expression
    : expression
    ;
```

<span data-ttu-id="abcc0-2741">控制條件式運算式*if_statement* ([if 陳述式](statements.md#the-if-statement))， *while_statement* ([while 陳述式](statements.md#the-while-statement))，*do_statement* ([do 陳述式](statements.md#the-do-statement))，或*for_statement* ([陳述式](statements.md#the-for-statement)) 是*boolean_運算式*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2741">The controlling conditional expression of an *if_statement* ([The if statement](statements.md#the-if-statement)), *while_statement* ([The while statement](statements.md#the-while-statement)), *do_statement* ([The do statement](statements.md#the-do-statement)), or *for_statement* ([The for statement](statements.md#the-for-statement)) is a *boolean_expression*.</span></span> <span data-ttu-id="abcc0-2742">控制條件式運算式`?:`運算子 ([條件運算子](expressions.md#conditional-operator)) 會遵循相同的規則*boolean_expression*，但基於運算子的分類優先順序作為*conditional_or_expression*。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2742">The controlling conditional expression of the `?:` operator ([Conditional operator](expressions.md#conditional-operator)) follows the same rules as a *boolean_expression*, but for reasons of operator precedence is classified as a *conditional_or_expression*.</span></span>

<span data-ttu-id="abcc0-2743">A *boolean_expression* `E`必須是能夠產生之類型值的`bool`、，如下所示：</span><span class="sxs-lookup"><span data-stu-id="abcc0-2743">A *boolean_expression* `E` is required to be able to produce a value of type `bool`, as follows:</span></span>

*  <span data-ttu-id="abcc0-2744">如果`E`隱含轉換為`bool`則會在執行階段套用該隱含轉換。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2744">If `E` is implicitly convertible to `bool` then at runtime that implicit conversion is applied.</span></span>
*  <span data-ttu-id="abcc0-2745">否則一元運算子多載解析 ([一元運算子多載解析](expressions.md#unary-operator-overload-resolution)) 用來尋找唯一的最佳實作的運算子`true`上`E`，而且該實作會在執行階段套用。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2745">Otherwise, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is used to find a unique best implementation of operator `true` on `E`, and that implementation is applied at runtime.</span></span>
*  <span data-ttu-id="abcc0-2746">如果找到這樣的運算子，則繫結階段會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2746">If no such operator is found, a binding-time error occurs.</span></span>

<span data-ttu-id="abcc0-2747">`DBBool`結構中的型別[資料庫，則為 true 的型別](structs.md#database-boolean-type)提供之類型的實作範例`operator true`和`operator false`。</span><span class="sxs-lookup"><span data-stu-id="abcc0-2747">The `DBBool` struct type in [Database boolean type](structs.md#database-boolean-type) provides an example of a type that implements `operator true` and `operator false`.</span></span>
