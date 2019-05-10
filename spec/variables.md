---
ms.openlocfilehash: b7bb7dd575d9e2e6d5dd85bdd3e535411e29fcf4
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488870"
---
# <a name="variables"></a><span data-ttu-id="6d25d-101">變數</span><span class="sxs-lookup"><span data-stu-id="6d25d-101">Variables</span></span>

<span data-ttu-id="6d25d-102">變數代表儲存體位置。</span><span class="sxs-lookup"><span data-stu-id="6d25d-102">Variables represent storage locations.</span></span> <span data-ttu-id="6d25d-103">每個變數具有一種類型，判斷哪些值可以儲存在變數中。</span><span class="sxs-lookup"><span data-stu-id="6d25d-103">Every variable has a type that determines what values can be stored in the variable.</span></span> <span data-ttu-id="6d25d-104">C# 是型別安全的語言，以及 C# 編譯器保證變數中儲存的值永遠都適當的型別。</span><span class="sxs-lookup"><span data-stu-id="6d25d-104">C# is a type-safe language, and the C# compiler guarantees that values stored in variables are always of the appropriate type.</span></span> <span data-ttu-id="6d25d-105">可以變更變數的值，透過指派或透過使用`++`和`--`運算子。</span><span class="sxs-lookup"><span data-stu-id="6d25d-105">The value of a variable can be changed through assignment or through use of the `++` and `--` operators.</span></span>

<span data-ttu-id="6d25d-106">變數必須是***明確指派***([明確指派](variables.md#definite-assignment)) 才能取得其值。</span><span class="sxs-lookup"><span data-stu-id="6d25d-106">A variable must be ***definitely assigned*** ([Definite assignment](variables.md#definite-assignment)) before its value can be obtained.</span></span>

<span data-ttu-id="6d25d-107">下列各節所述，變數都是***初始指派***或是***最初未指派***。</span><span class="sxs-lookup"><span data-stu-id="6d25d-107">As described in the following sections, variables are either ***initially assigned*** or ***initially unassigned***.</span></span> <span data-ttu-id="6d25d-108">一開始指派的變數具有定義完善的初始值，一律會視為明確指派。</span><span class="sxs-lookup"><span data-stu-id="6d25d-108">An initially assigned variable has a well-defined initial value and is always considered definitely assigned.</span></span> <span data-ttu-id="6d25d-109">一開始未指派的變數具有沒有初始值。</span><span class="sxs-lookup"><span data-stu-id="6d25d-109">An initially unassigned variable has no initial value.</span></span> <span data-ttu-id="6d25d-110">一開始未指派的變數會視為已明確指派在特定位置，就指派值給變數必須在該位置的每個可能的執行路徑中。</span><span class="sxs-lookup"><span data-stu-id="6d25d-110">For an initially unassigned variable to be considered definitely assigned at a certain location, an assignment to the variable must occur in every possible execution path leading to that location.</span></span>

## <a name="variable-categories"></a><span data-ttu-id="6d25d-111">變數類別</span><span class="sxs-lookup"><span data-stu-id="6d25d-111">Variable categories</span></span>

<span data-ttu-id="6d25d-112">C# 定義的變數的七個類別： 靜態變數、 執行個體變數、 陣列元素、 值參數、 參考參數、 輸出參數和區域變數。</span><span class="sxs-lookup"><span data-stu-id="6d25d-112">C# defines seven categories of variables: static variables, instance variables, array elements, value parameters, reference parameters, output parameters, and local variables.</span></span> <span data-ttu-id="6d25d-113">下列各節說明這些類別。</span><span class="sxs-lookup"><span data-stu-id="6d25d-113">The sections that follow describe each of these categories.</span></span>

<span data-ttu-id="6d25d-114">在範例</span><span class="sxs-lookup"><span data-stu-id="6d25d-114">In the example</span></span>
```csharp
class A
{
    public static int x;
    int y;

    void F(int[] v, int a, ref int b, out int c) {
        int i = 1;
        c = a + b++;
    }
}
```
<span data-ttu-id="6d25d-115">`x` 是靜態的變數，`y`是執行個體變數，`v[0]`是陣列項目`a`值的參數`b`是參考參數，`c`是一個 output 參數，和`i`是區域變數。</span><span class="sxs-lookup"><span data-stu-id="6d25d-115">`x` is a static variable, `y` is an instance variable, `v[0]` is an array element, `a` is a value parameter, `b` is a reference parameter, `c` is an output parameter, and `i` is a local variable.</span></span>

### <a name="static-variables"></a><span data-ttu-id="6d25d-116">靜態變數</span><span class="sxs-lookup"><span data-stu-id="6d25d-116">Static variables</span></span>

<span data-ttu-id="6d25d-117">欄位宣告`static`修飾詞會呼叫***靜態變數***。</span><span class="sxs-lookup"><span data-stu-id="6d25d-117">A field declared with the `static` modifier is called a ***static variable***.</span></span> <span data-ttu-id="6d25d-118">靜態變數進入是否存在，再執行靜態建構函式 ([靜態建構函式](classes.md#static-constructors)) 包含的類型和中止時的相關聯的應用程式定義域就不會存在。</span><span class="sxs-lookup"><span data-stu-id="6d25d-118">A static variable comes into existence before execution of the static constructor ([Static constructors](classes.md#static-constructors)) for its containing type, and ceases to exist when the associated application domain ceases to exist.</span></span>

<span data-ttu-id="6d25d-119">靜態變數的初始值是預設值 ([預設值](variables.md#default-values)) 的變數的類型。</span><span class="sxs-lookup"><span data-stu-id="6d25d-119">The initial value of a static variable is the default value ([Default values](variables.md#default-values)) of the variable's type.</span></span>

<span data-ttu-id="6d25d-120">明確設定檢查的目的而言，靜態變數會被視為一開始指派。</span><span class="sxs-lookup"><span data-stu-id="6d25d-120">For purposes of definite assignment checking, a static variable is considered initially assigned.</span></span>

### <a name="instance-variables"></a><span data-ttu-id="6d25d-121">執行個體變數</span><span class="sxs-lookup"><span data-stu-id="6d25d-121">Instance variables</span></span>

<span data-ttu-id="6d25d-122">欄位宣告不含`static`修飾詞會呼叫***執行個體變數***。</span><span class="sxs-lookup"><span data-stu-id="6d25d-122">A field declared without the `static` modifier is called an ***instance variable***.</span></span>

#### <a name="instance-variables-in-classes"></a><span data-ttu-id="6d25d-123">在類別中的執行個體變數</span><span class="sxs-lookup"><span data-stu-id="6d25d-123">Instance variables in classes</span></span>

<span data-ttu-id="6d25d-124">類別的執行個體變數會進入存在，該類別的新執行個體建立時，並沒有參考至該執行個體和執行個體的解構函式 （如果有的話） 執行時停止。</span><span class="sxs-lookup"><span data-stu-id="6d25d-124">An instance variable of a class comes into existence when a new instance of that class is created, and ceases to exist when there are no references to that instance and the instance's destructor (if any) has executed.</span></span>

<span data-ttu-id="6d25d-125">類別的執行個體變數的初始值是預設值 ([預設值](variables.md#default-values)) 的變數的類型。</span><span class="sxs-lookup"><span data-stu-id="6d25d-125">The initial value of an instance variable of a class is the default value ([Default values](variables.md#default-values)) of the variable's type.</span></span>

<span data-ttu-id="6d25d-126">為了明確設定檢查，類別的執行個體變數會被視為一開始指派。</span><span class="sxs-lookup"><span data-stu-id="6d25d-126">For the purpose of definite assignment checking, an instance variable of a class is considered initially assigned.</span></span>

#### <a name="instance-variables-in-structs"></a><span data-ttu-id="6d25d-127">在結構中的執行個體變數</span><span class="sxs-lookup"><span data-stu-id="6d25d-127">Instance variables in structs</span></span>

<span data-ttu-id="6d25d-128">結構的執行個體變數具有完全相同的存留期與其所屬的結構變數。</span><span class="sxs-lookup"><span data-stu-id="6d25d-128">An instance variable of a struct has exactly the same lifetime as the struct variable to which it belongs.</span></span> <span data-ttu-id="6d25d-129">換句話說，當結構類型的變數進入存在或不再存在，因此過的執行個體變數的結構。</span><span class="sxs-lookup"><span data-stu-id="6d25d-129">In other words, when a variable of a struct type comes into existence or ceases to exist, so too do the instance variables of the struct.</span></span>

<span data-ttu-id="6d25d-130">結構的執行個體變數的初始的指派狀態會是包含的結構變數相同。</span><span class="sxs-lookup"><span data-stu-id="6d25d-130">The initial assignment state of an instance variable of a struct is the same as that of the containing struct variable.</span></span> <span data-ttu-id="6d25d-131">換句話說，當結構變數會被視為一開始指派，因此也是其執行個體變數，以及結構變數會被視為一開始未指派的當其執行個體變數同樣是未指派。</span><span class="sxs-lookup"><span data-stu-id="6d25d-131">In other words, when a struct variable is considered initially assigned, so too are its instance variables, and when a struct variable is considered initially unassigned, its instance variables are likewise unassigned.</span></span>

### <a name="array-elements"></a><span data-ttu-id="6d25d-132">陣列項目</span><span class="sxs-lookup"><span data-stu-id="6d25d-132">Array elements</span></span>

<span data-ttu-id="6d25d-133">陣列的項目進入建立陣列執行個體時，存在，且存在時沒有參考至該陣列執行個體。</span><span class="sxs-lookup"><span data-stu-id="6d25d-133">The elements of an array come into existence when an array instance is created, and cease to exist when there are no references to that array instance.</span></span>

<span data-ttu-id="6d25d-134">每個陣列元素的初始值是預設值 ([預設值](variables.md#default-values)) 的陣列元素的型別。</span><span class="sxs-lookup"><span data-stu-id="6d25d-134">The initial value of each of the elements of an array is the default value ([Default values](variables.md#default-values)) of the type of the array elements.</span></span>

<span data-ttu-id="6d25d-135">為了明確設定檢查，陣列元素會被視為一開始指派。</span><span class="sxs-lookup"><span data-stu-id="6d25d-135">For the purpose of definite assignment checking, an array element is considered initially assigned.</span></span>

### <a name="value-parameters"></a><span data-ttu-id="6d25d-136">值參數</span><span class="sxs-lookup"><span data-stu-id="6d25d-136">Value parameters</span></span>

<span data-ttu-id="6d25d-137">未宣告的參數`ref`或是`out`修飾詞***實值參數***。</span><span class="sxs-lookup"><span data-stu-id="6d25d-137">A parameter declared without a `ref` or `out` modifier is a ***value parameter***.</span></span>

<span data-ttu-id="6d25d-138">進入函式成員 （方法、 執行個體建構函式，存取子或運算子） 或匿名函式引動過程時存在的實值參數的參數所屬的與指定引動過程的引數的值進行初始化。</span><span class="sxs-lookup"><span data-stu-id="6d25d-138">A value parameter comes into existence upon invocation of the function member (method, instance constructor, accessor, or operator) or anonymous function to which the parameter belongs, and is initialized with the value of the argument given in the invocation.</span></span> <span data-ttu-id="6d25d-139">實值參數通常會停止存在的函式成員 」 或 「 匿名函式傳回時。</span><span class="sxs-lookup"><span data-stu-id="6d25d-139">A value parameter normally ceases to exist upon return of the function member or anonymous function.</span></span> <span data-ttu-id="6d25d-140">不過，如果值參數會擷取匿名函式 ([匿名函式運算式](expressions.md#anonymous-function-expressions))、 存留期會延伸到至少直到委派或從該匿名函式建立的運算式樹狀結構不符合資格記憶體回收。</span><span class="sxs-lookup"><span data-stu-id="6d25d-140">However, if the value parameter is captured by an anonymous function ([Anonymous function expressions](expressions.md#anonymous-function-expressions)), its life time extends at least until the delegate or expression tree created from that anonymous function is eligible for garbage collection.</span></span>

<span data-ttu-id="6d25d-141">為了明確設定檢查，實值參數會被視為一開始指派。</span><span class="sxs-lookup"><span data-stu-id="6d25d-141">For the purpose of definite assignment checking, a value parameter is considered initially assigned.</span></span>

### <a name="reference-parameters"></a><span data-ttu-id="6d25d-142">傳址參數</span><span class="sxs-lookup"><span data-stu-id="6d25d-142">Reference parameters</span></span>

<span data-ttu-id="6d25d-143">參數以宣告`ref`修飾詞***參考參數***。</span><span class="sxs-lookup"><span data-stu-id="6d25d-143">A parameter declared with a `ref` modifier is a ***reference parameter***.</span></span>

<span data-ttu-id="6d25d-144">參考參數不會建立新的存放位置。</span><span class="sxs-lookup"><span data-stu-id="6d25d-144">A reference parameter does not create a new storage location.</span></span> <span data-ttu-id="6d25d-145">相反地，參考參數表示相同的儲存位置，為給定中的函式成員 」 或 「 匿名函式引動過程的引數的變數。</span><span class="sxs-lookup"><span data-stu-id="6d25d-145">Instead, a reference parameter represents the same storage location as the variable given as the argument in the function member or anonymous function invocation.</span></span> <span data-ttu-id="6d25d-146">因此，參考參數的值是一律為基礎的變數相同。</span><span class="sxs-lookup"><span data-stu-id="6d25d-146">Thus, the value of a reference parameter is always the same as the underlying variable.</span></span>

<span data-ttu-id="6d25d-147">下列的明確指派規則適用於參考參數。</span><span class="sxs-lookup"><span data-stu-id="6d25d-147">The following definite assignment rules apply to reference parameters.</span></span> <span data-ttu-id="6d25d-148">請注意，不同的規則，如中所述的輸出參數[輸出參數](variables.md#output-parameters)。</span><span class="sxs-lookup"><span data-stu-id="6d25d-148">Note the different rules for output parameters described in [Output parameters](variables.md#output-parameters).</span></span>

*  <span data-ttu-id="6d25d-149">變數必須明確進行指派 ([明確指派](variables.md#definite-assignment)) 將它傳遞為函式成員或委派引動過程中參考參數之前。</span><span class="sxs-lookup"><span data-stu-id="6d25d-149">A variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before it can be passed as a reference parameter in a function member or delegate invocation.</span></span>
*  <span data-ttu-id="6d25d-150">在函式成員或匿名函式中，參考參數視為一開始指派。</span><span class="sxs-lookup"><span data-stu-id="6d25d-150">Within a function member or anonymous function, a reference parameter is considered initially assigned.</span></span>

<span data-ttu-id="6d25d-151">執行個體方法或結構類型的執行個體存取子內`this`關鍵字的行為就像參考參數的結構類型 ([這項存取](expressions.md#this-access))。</span><span class="sxs-lookup"><span data-stu-id="6d25d-151">Within an instance method or instance accessor of a struct type, the `this` keyword behaves exactly as a reference parameter of the struct type ([This access](expressions.md#this-access)).</span></span>

### <a name="output-parameters"></a><span data-ttu-id="6d25d-152">輸出參數</span><span class="sxs-lookup"><span data-stu-id="6d25d-152">Output parameters</span></span>

<span data-ttu-id="6d25d-153">參數以宣告`out`修飾詞***輸出參數***。</span><span class="sxs-lookup"><span data-stu-id="6d25d-153">A parameter declared with an `out` modifier is an ***output parameter***.</span></span>

<span data-ttu-id="6d25d-154">輸出參數不會建立新的存放位置。</span><span class="sxs-lookup"><span data-stu-id="6d25d-154">An output parameter does not create a new storage location.</span></span> <span data-ttu-id="6d25d-155">相反地，輸出參數會表示為變數引數，函式成員或委派引動過程中所指定相同的儲存位置。</span><span class="sxs-lookup"><span data-stu-id="6d25d-155">Instead, an output parameter represents the same storage location as the variable given as the argument in the function member or delegate invocation.</span></span> <span data-ttu-id="6d25d-156">因此，輸出參數的值是一律為基礎的變數相同。</span><span class="sxs-lookup"><span data-stu-id="6d25d-156">Thus, the value of an output parameter is always the same as the underlying variable.</span></span>

<span data-ttu-id="6d25d-157">下列的明確指派規則適用於輸出參數。</span><span class="sxs-lookup"><span data-stu-id="6d25d-157">The following definite assignment rules apply to output parameters.</span></span> <span data-ttu-id="6d25d-158">請注意，不同的規則，參考參數中所述[參考參數](variables.md#reference-parameters)。</span><span class="sxs-lookup"><span data-stu-id="6d25d-158">Note the different rules for reference parameters described in [Reference parameters](variables.md#reference-parameters).</span></span>

*  <span data-ttu-id="6d25d-159">變數必須未明確指派可以傳遞做為輸出參數，在函式成員或委派引動過程之前。</span><span class="sxs-lookup"><span data-stu-id="6d25d-159">A variable need not be definitely assigned before it can be passed as an output parameter in a function member or delegate invocation.</span></span>
*  <span data-ttu-id="6d25d-160">下列一般函式成員或委派引動過程完成時，每個輸出參數會被視為已傳遞的變數指派給該執行路徑中。</span><span class="sxs-lookup"><span data-stu-id="6d25d-160">Following the normal completion of a function member or delegate invocation, each variable that was passed as an output parameter is considered assigned in that execution path.</span></span>
*  <span data-ttu-id="6d25d-161">在函式成員或匿名函式中，輸出參數會被視為一開始未指派。</span><span class="sxs-lookup"><span data-stu-id="6d25d-161">Within a function member or anonymous function, an output parameter is considered initially unassigned.</span></span>
*  <span data-ttu-id="6d25d-162">函式成員或匿名函式的每個輸出參數必須明確進行指派 ([明確指派](variables.md#definite-assignment)) 的函式之前成員或匿名函式正常地傳回。</span><span class="sxs-lookup"><span data-stu-id="6d25d-162">Every output parameter of a function member or anonymous function must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before the function member or anonymous function returns normally.</span></span>

<span data-ttu-id="6d25d-163">結構類型的執行個體建構函式內`this`關鍵字的行為完全相同的結構類型的輸出參數 ([這項存取](expressions.md#this-access))。</span><span class="sxs-lookup"><span data-stu-id="6d25d-163">Within an instance constructor of a struct type, the `this` keyword behaves exactly as an output parameter of the struct type ([This access](expressions.md#this-access)).</span></span>

### <a name="local-variables"></a><span data-ttu-id="6d25d-164">區域變數</span><span class="sxs-lookup"><span data-stu-id="6d25d-164">Local variables</span></span>

<span data-ttu-id="6d25d-165">A***區域變數***所宣告*local_variable_declaration*，這可能會發生在*區塊*，則*for_statement*， *switch_statement*或 a *using_statement*; 或藉由*foreach_statement*或*specific_catch_clause*如*try_statement*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-165">A ***local variable*** is declared by a *local_variable_declaration*, which may occur in a *block*, a *for_statement*, a *switch_statement* or a *using_statement*; or by a *foreach_statement* or a *specific_catch_clause* for a *try_statement*.</span></span>

<span data-ttu-id="6d25d-166">本機變數的存留期是程式執行期間要保留給它保證儲存體的部分。</span><span class="sxs-lookup"><span data-stu-id="6d25d-166">The lifetime of a local variable is the portion of program execution during which storage is guaranteed to be reserved for it.</span></span> <span data-ttu-id="6d25d-167">此存留期擴充至少從進入*區塊*， *for_statement*， *switch_statement*， *using_statement*， *foreach_statement*，或*specific_catch_clause*與它相關聯，執行，直到*區塊*， *for_statement*， *switch_statement*， *using_statement*， *foreach_statement*，或*specific_catch_clause*以任何方式結束。</span><span class="sxs-lookup"><span data-stu-id="6d25d-167">This lifetime extends at least from entry into the *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause* with which it is associated, until execution of that *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause* ends in any way.</span></span> <span data-ttu-id="6d25d-168">(輸入括住*區塊*或呼叫方法暫停執行，但未結束，執行目前*區塊*， *for_statement*， *switch_statement*， *using_statement*， *foreach_statement*，或*specific_catch_clause*。)如果要將本機變數擷取匿名函式 ([擷取的外部變數](expressions.md#captured-outer-variables))，其存留期擴充至少直到建立匿名函式，以及前往的任何其他物件的委派或運算式樹狀結構參考擷取的變數，可進行記憶體回收。</span><span class="sxs-lookup"><span data-stu-id="6d25d-168">(Entering an enclosed *block* or calling a method suspends, but does not end, execution of the current *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause*.) If the local variable is captured by an anonymous function ([Captured outer variables](expressions.md#captured-outer-variables)), its lifetime extends at least until the delegate or expression tree created from the anonymous function, along with any other objects that come to reference the captured variable, are eligible for garbage collection.</span></span>

<span data-ttu-id="6d25d-169">如果父代*區塊*， *for_statement*， *switch_statement*， *using_statement*， *foreach_statement*，或*specific_catch_clause*輸入以遞迴方式，區域變數的新執行個體每次都會建立，並將其*local_variable_initializer*，如果任何項目，會評估每一次。</span><span class="sxs-lookup"><span data-stu-id="6d25d-169">If the parent *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause* is entered recursively, a new instance of the local variable is created each time, and its *local_variable_initializer*, if any, is evaluated each time.</span></span>

<span data-ttu-id="6d25d-170">所導入的本機變數*local_variable_declaration*不會自動初始化，因此沒有預設值。</span><span class="sxs-lookup"><span data-stu-id="6d25d-170">A local variable introduced by a *local_variable_declaration* is not automatically initialized and thus has no default value.</span></span> <span data-ttu-id="6d25d-171">為了明確設定檢查，本機變數則是所引進*local_variable_declaration*會被視為一開始未指派。</span><span class="sxs-lookup"><span data-stu-id="6d25d-171">For the purpose of definite assignment checking, a local variable introduced by a *local_variable_declaration* is considered initially unassigned.</span></span> <span data-ttu-id="6d25d-172">A *local_variable_declaration*可能會包含*local_variable_initializer*，在此情況下變數會被視為已明確指派只有在初始化運算式之後 ([宣告陳述式](variables.md#declaration-statements))。</span><span class="sxs-lookup"><span data-stu-id="6d25d-172">A *local_variable_declaration* may include a *local_variable_initializer*, in which case the variable is considered definitely assigned only after the initializing expression ([Declaration statements](variables.md#declaration-statements)).</span></span>

<span data-ttu-id="6d25d-173">所導入的本機變數的範圍內*local_variable_declaration*，它是編譯時期錯誤來參考該區域變數之前的文字位置及其*local_variable_declarator*.</span><span class="sxs-lookup"><span data-stu-id="6d25d-173">Within the scope of a local variable introduced by a *local_variable_declaration*, it is a compile-time error to refer to that local variable in a textual position that precedes its *local_variable_declarator*.</span></span> <span data-ttu-id="6d25d-174">如果是隱含的本機變數宣告 ([區域變數宣告](statements.md#local-variable-declarations))，還有參考此變數中的錯誤及其*local_variable_declarator*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-174">If the local variable declaration is implicit ([Local variable declarations](statements.md#local-variable-declarations)), it is also an error to refer to the variable within its *local_variable_declarator*.</span></span>

<span data-ttu-id="6d25d-175">所導入的本機變數*foreach_statement*或是*specific_catch_clause*會被視為已明確指派的整個範圍中。</span><span class="sxs-lookup"><span data-stu-id="6d25d-175">A local variable introduced by a *foreach_statement* or a *specific_catch_clause* is considered definitely assigned in its entire scope.</span></span>

<span data-ttu-id="6d25d-176">本機變數的實際的存留期是視實作而定。</span><span class="sxs-lookup"><span data-stu-id="6d25d-176">The actual lifetime of a local variable is implementation-dependent.</span></span> <span data-ttu-id="6d25d-177">例如，編譯器可能會以靜態方式判斷區塊中的本機變數僅用於該區塊的一小部分中。</span><span class="sxs-lookup"><span data-stu-id="6d25d-177">For example, a compiler might statically determine that a local variable in a block is only used for a small portion of that block.</span></span> <span data-ttu-id="6d25d-178">使用這項分析，編譯器所產生的結果具有較短的存留時間，比其包含區塊的變數的儲存體中的程式碼。</span><span class="sxs-lookup"><span data-stu-id="6d25d-178">Using this analysis, the compiler could generate code that results in the variable's storage having a shorter lifetime than its containing block.</span></span>

<span data-ttu-id="6d25d-179">區域參考變數所參考的儲存體回收與該區域參考變數的存留期無關 ([自動記憶體管理](basic-concepts.md#automatic-memory-management))。</span><span class="sxs-lookup"><span data-stu-id="6d25d-179">The storage referred to by a local reference variable is reclaimed independently of the lifetime of that local reference variable ([Automatic memory management](basic-concepts.md#automatic-memory-management)).</span></span>

## <a name="default-values"></a><span data-ttu-id="6d25d-180">預設值</span><span class="sxs-lookup"><span data-stu-id="6d25d-180">Default values</span></span>

<span data-ttu-id="6d25d-181">下列類別的變數會自動初始化為其預設值：</span><span class="sxs-lookup"><span data-stu-id="6d25d-181">The following categories of variables are automatically initialized to their default values:</span></span>

*  <span data-ttu-id="6d25d-182">靜態變數。</span><span class="sxs-lookup"><span data-stu-id="6d25d-182">Static variables.</span></span>
*  <span data-ttu-id="6d25d-183">類別執行個體的執行個體變數。</span><span class="sxs-lookup"><span data-stu-id="6d25d-183">Instance variables of class instances.</span></span>
*  <span data-ttu-id="6d25d-184">陣列項目。</span><span class="sxs-lookup"><span data-stu-id="6d25d-184">Array elements.</span></span>

<span data-ttu-id="6d25d-185">變數的預設值取決於該變數的類型，並以下列方式決定：</span><span class="sxs-lookup"><span data-stu-id="6d25d-185">The default value of a variable depends on the type of the variable and is determined as follows:</span></span>

*  <span data-ttu-id="6d25d-186">變數*value_type*，預設值是所計算的值相同*value_type*的預設建構函式 ([預設建構函式](types.md#default-constructors))。</span><span class="sxs-lookup"><span data-stu-id="6d25d-186">For a variable of a *value_type*, the default value is the same as the value computed by the *value_type*'s default constructor ([Default constructors](types.md#default-constructors)).</span></span>
*  <span data-ttu-id="6d25d-187">變數*reference_type*，預設值是`null`。</span><span class="sxs-lookup"><span data-stu-id="6d25d-187">For a variable of a *reference_type*, the default value is `null`.</span></span>

<span data-ttu-id="6d25d-188">初始化為預設值通常是藉由讓記憶體管理員，或配置供使用之前，記憶體回收行程會初始化零的所有位元的記憶體。</span><span class="sxs-lookup"><span data-stu-id="6d25d-188">Initialization to default values is typically done by having the memory manager or garbage collector initialize memory to all-bits-zero before it is allocated for use.</span></span> <span data-ttu-id="6d25d-189">基於這個理由，很方便地使用零的所有位元來代表 null 參考。</span><span class="sxs-lookup"><span data-stu-id="6d25d-189">For this reason, it is convenient to use all-bits-zero to represent the null reference.</span></span>

## <a name="definite-assignment"></a><span data-ttu-id="6d25d-190">明確指派</span><span class="sxs-lookup"><span data-stu-id="6d25d-190">Definite assignment</span></span>

<span data-ttu-id="6d25d-191">在函式成員的指定位置，可執行的程式碼中的變數稱為***明確指派***如果編譯器可以證明，特殊的靜態流程分析 ([精確判斷明確規則指派](variables.md#precise-rules-for-determining-definite-assignment))，已自動初始化變數，或已經過至少一個指派的目標。</span><span class="sxs-lookup"><span data-stu-id="6d25d-191">At a given location in the executable code of a function member, a variable is said to be ***definitely assigned*** if the compiler can prove, by a particular static flow analysis ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)), that the variable has been automatically initialized or has been the target of at least one assignment.</span></span> <span data-ttu-id="6d25d-192">明確設定的規則非正式地所述，包括：</span><span class="sxs-lookup"><span data-stu-id="6d25d-192">Informally stated, the rules of definite assignment are:</span></span>

*  <span data-ttu-id="6d25d-193">一開始指派的變數 ([最初指派變數](variables.md#initially-assigned-variables)) 一律會視為已明確指派。</span><span class="sxs-lookup"><span data-stu-id="6d25d-193">An initially assigned variable ([Initially assigned variables](variables.md#initially-assigned-variables)) is always considered definitely assigned.</span></span>
*  <span data-ttu-id="6d25d-194">一開始未指派的變數 ([一開始未指派的變數](variables.md#initially-unassigned-variables)) 會被視為已明確指派中的特定位置如果該位置的所有可能的執行路徑包含至少下列其中之一：</span><span class="sxs-lookup"><span data-stu-id="6d25d-194">An initially unassigned variable ([Initially unassigned variables](variables.md#initially-unassigned-variables)) is considered definitely assigned at a given location if all possible execution paths leading to that location contain at least one of the following:</span></span>
    * <span data-ttu-id="6d25d-195">簡單指派 ([簡單指派](expressions.md#simple-assignment)) 中的變數是左的運算元。</span><span class="sxs-lookup"><span data-stu-id="6d25d-195">A simple assignment ([Simple assignment](expressions.md#simple-assignment)) in which the variable is the left operand.</span></span>
    * <span data-ttu-id="6d25d-196">引動過程運算式 ([引動過程運算式](expressions.md#invocation-expressions)) 或物件建立運算式 ([物件建立運算式](expressions.md#object-creation-expressions))，將變數傳遞做為輸出參數。</span><span class="sxs-lookup"><span data-stu-id="6d25d-196">An invocation expression ([Invocation expressions](expressions.md#invocation-expressions)) or object creation expression ([Object creation expressions](expressions.md#object-creation-expressions)) that passes the variable as an output parameter.</span></span>
    * <span data-ttu-id="6d25d-197">區域變數，區域變數宣告 ([區域變數宣告](statements.md#local-variable-declarations))，其中包含變數的初始設定式。</span><span class="sxs-lookup"><span data-stu-id="6d25d-197">For a local variable, a local variable declaration ([Local variable declarations](statements.md#local-variable-declarations)) that includes a variable initializer.</span></span>

<span data-ttu-id="6d25d-198">上述的非正式規則的基礎正式規格所述[最初指派變數](variables.md#initially-assigned-variables)，[一開始未指派的變數](variables.md#initially-unassigned-variables)，和[來判斷精確的規則明確指派](variables.md#precise-rules-for-determining-definite-assignment)。</span><span class="sxs-lookup"><span data-stu-id="6d25d-198">The formal specification underlying the above informal rules is described in [Initially assigned variables](variables.md#initially-assigned-variables), [Initially unassigned variables](variables.md#initially-unassigned-variables), and [Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment).</span></span>

<span data-ttu-id="6d25d-199">明確指派的狀態變數的執行個體*struct_type*變數會追蹤個別也集體。</span><span class="sxs-lookup"><span data-stu-id="6d25d-199">The definite assignment states of instance variables of a *struct_type* variable are tracked individually as well as collectively.</span></span> <span data-ttu-id="6d25d-200">在上述規則，進行其他的下列規則適用於*struct_type*變數和其執行個體的變數：</span><span class="sxs-lookup"><span data-stu-id="6d25d-200">In additional to the rules above, the following rules apply to *struct_type* variables and their instance variables:</span></span>

*  <span data-ttu-id="6d25d-201">執行個體變數會視為已明確指派，如果其包含*struct_type*變數會被視為已明確指派。</span><span class="sxs-lookup"><span data-stu-id="6d25d-201">An instance variable is considered definitely assigned if its containing *struct_type* variable is considered definitely assigned.</span></span>
*  <span data-ttu-id="6d25d-202">A *struct_type*變數被視為已明確指派，如果每一個它的執行個體變數被視為已明確指派。</span><span class="sxs-lookup"><span data-stu-id="6d25d-202">A *struct_type* variable is considered definitely assigned if each of its instance variables is considered definitely assigned.</span></span>

<span data-ttu-id="6d25d-203">明確指派是在下列情況的需求：</span><span class="sxs-lookup"><span data-stu-id="6d25d-203">Definite assignment is a requirement in the following contexts:</span></span>

*  <span data-ttu-id="6d25d-204">必須明確指派變數，在其中取得它的值是每個位置。</span><span class="sxs-lookup"><span data-stu-id="6d25d-204">A variable must be definitely assigned at each location where its value is obtained.</span></span> <span data-ttu-id="6d25d-205">這可確保未定義的值永遠不會發生。</span><span class="sxs-lookup"><span data-stu-id="6d25d-205">This ensures that undefined values never occur.</span></span> <span data-ttu-id="6d25d-206">若要取得變數的值，除非視為變數在運算式中的相符項目</span><span class="sxs-lookup"><span data-stu-id="6d25d-206">The occurrence of a variable in an expression is considered to obtain the value of the variable, except when</span></span>
    * <span data-ttu-id="6d25d-207">變數是指派的簡單的左的運算元</span><span class="sxs-lookup"><span data-stu-id="6d25d-207">the variable is the left operand of a simple assignment,</span></span>
    * <span data-ttu-id="6d25d-208">變數會傳遞做為輸出參數，或</span><span class="sxs-lookup"><span data-stu-id="6d25d-208">the variable is passed as an output parameter, or</span></span>
    * <span data-ttu-id="6d25d-209">變數是*struct_type*變數，且為成員存取的左運算元。</span><span class="sxs-lookup"><span data-stu-id="6d25d-209">the variable is a *struct_type* variable and occurs as the left operand of a member access.</span></span>
*  <span data-ttu-id="6d25d-210">在每個位置，它會傳遞做為參考參數，必須明確指派變數。</span><span class="sxs-lookup"><span data-stu-id="6d25d-210">A variable must be definitely assigned at each location where it is passed as a reference parameter.</span></span> <span data-ttu-id="6d25d-211">這可確保叫用的函式成員，可以考慮一開始指派參考參數。</span><span class="sxs-lookup"><span data-stu-id="6d25d-211">This ensures that the function member being invoked can consider the reference parameter initially assigned.</span></span>
*  <span data-ttu-id="6d25d-212">函式成員的所有輸出參數必須明確地都指派每個位置其中函式成員會傳回 (透過`return`陳述式或到達函式成員主體結尾的執行)。</span><span class="sxs-lookup"><span data-stu-id="6d25d-212">All output parameters of a function member must be definitely assigned at each location where the function member returns (through a `return` statement or through execution reaching the end of the function member body).</span></span> <span data-ttu-id="6d25d-213">這可確保，函式成員不會傳回未定義的值在輸出參數，如此可讓編譯器考慮使用函式成員引動過程的變數做為輸出參數相當於指派給變數。</span><span class="sxs-lookup"><span data-stu-id="6d25d-213">This ensures that function members do not return undefined values in output parameters, thus enabling the compiler to consider a function member invocation that takes a variable as an output parameter equivalent to an assignment to the variable.</span></span>
*  <span data-ttu-id="6d25d-214">`this`的變數*struct_type*執行個體建構函式必須在每個位置，其中該執行個體建構函式會傳回明確指派。</span><span class="sxs-lookup"><span data-stu-id="6d25d-214">The `this` variable of a *struct_type* instance constructor must be definitely assigned at each location where that instance constructor returns.</span></span>

### <a name="initially-assigned-variables"></a><span data-ttu-id="6d25d-215">一開始指派的變數</span><span class="sxs-lookup"><span data-stu-id="6d25d-215">Initially assigned variables</span></span>

<span data-ttu-id="6d25d-216">下列類別的變數會歸類為初始指派：</span><span class="sxs-lookup"><span data-stu-id="6d25d-216">The following categories of variables are classified as initially assigned:</span></span>

*  <span data-ttu-id="6d25d-217">靜態變數。</span><span class="sxs-lookup"><span data-stu-id="6d25d-217">Static variables.</span></span>
*  <span data-ttu-id="6d25d-218">類別執行個體的執行個體變數。</span><span class="sxs-lookup"><span data-stu-id="6d25d-218">Instance variables of class instances.</span></span>
*  <span data-ttu-id="6d25d-219">執行個體的初始設定的結構變數的變數。</span><span class="sxs-lookup"><span data-stu-id="6d25d-219">Instance variables of initially assigned struct variables.</span></span>
*  <span data-ttu-id="6d25d-220">陣列項目。</span><span class="sxs-lookup"><span data-stu-id="6d25d-220">Array elements.</span></span>
*  <span data-ttu-id="6d25d-221">值的參數。</span><span class="sxs-lookup"><span data-stu-id="6d25d-221">Value parameters.</span></span>
*  <span data-ttu-id="6d25d-222">參考參數。</span><span class="sxs-lookup"><span data-stu-id="6d25d-222">Reference parameters.</span></span>
*  <span data-ttu-id="6d25d-223">中所宣告的變數`catch`子句或`foreach`陳述式。</span><span class="sxs-lookup"><span data-stu-id="6d25d-223">Variables declared in a `catch` clause or a `foreach` statement.</span></span>

### <a name="initially-unassigned-variables"></a><span data-ttu-id="6d25d-224">一開始未指派的變數</span><span class="sxs-lookup"><span data-stu-id="6d25d-224">Initially unassigned variables</span></span>

<span data-ttu-id="6d25d-225">下列類別的變數會歸類為一開始未設定：</span><span class="sxs-lookup"><span data-stu-id="6d25d-225">The following categories of variables are classified as initially unassigned:</span></span>

*  <span data-ttu-id="6d25d-226">一開始未指派的結構變數的執行個體變數。</span><span class="sxs-lookup"><span data-stu-id="6d25d-226">Instance variables of initially unassigned struct variables.</span></span>
*  <span data-ttu-id="6d25d-227">輸出參數，包括`this`結構執行個體建構函式的變數。</span><span class="sxs-lookup"><span data-stu-id="6d25d-227">Output parameters, including the `this` variable of struct instance constructors.</span></span>
*  <span data-ttu-id="6d25d-228">本機變數，但不包括在中宣告`catch`子句或`foreach`陳述式。</span><span class="sxs-lookup"><span data-stu-id="6d25d-228">Local variables, except those declared in a `catch` clause or a `foreach` statement.</span></span>

### <a name="precise-rules-for-determining-definite-assignment"></a><span data-ttu-id="6d25d-229">精確判斷明確指派規則</span><span class="sxs-lookup"><span data-stu-id="6d25d-229">Precise rules for determining definite assignment</span></span>

<span data-ttu-id="6d25d-230">若要判斷已明確指派每個使用的變數，編譯器必須使用相當於這一節所述的程序。</span><span class="sxs-lookup"><span data-stu-id="6d25d-230">In order to determine that each used variable is definitely assigned, the compiler must use a process that is equivalent to the one described in this section.</span></span>

<span data-ttu-id="6d25d-231">編譯器會處理具有一或多個一開始未指派的變數的每個函式成員的主體。</span><span class="sxs-lookup"><span data-stu-id="6d25d-231">The compiler processes the body of each function member that has one or more initially unassigned variables.</span></span> <span data-ttu-id="6d25d-232">每一開始未指派的變數*v*，編譯器會判斷***明確指派狀態***for *v*在每個函式成員中的下列各點：</span><span class="sxs-lookup"><span data-stu-id="6d25d-232">For each initially unassigned variable *v*, the compiler determines a ***definite assignment state*** for *v* at each of the following points in the function member:</span></span>

*  <span data-ttu-id="6d25d-233">每個陳述式的開頭</span><span class="sxs-lookup"><span data-stu-id="6d25d-233">At the beginning of each statement</span></span>
*  <span data-ttu-id="6d25d-234">在結束點 ([結束點和可執行性](statements.md#end-points-and-reachability)) 的每個陳述式</span><span class="sxs-lookup"><span data-stu-id="6d25d-234">At the end point ([End points and reachability](statements.md#end-points-and-reachability)) of each statement</span></span>
*  <span data-ttu-id="6d25d-235">在每個弧形的將控制權轉移到另一個陳述式或陳述式的結束點</span><span class="sxs-lookup"><span data-stu-id="6d25d-235">On each arc which transfers control to another statement or to the end point of a statement</span></span>
*  <span data-ttu-id="6d25d-236">在每個運算式的開頭</span><span class="sxs-lookup"><span data-stu-id="6d25d-236">At the beginning of each expression</span></span>
*  <span data-ttu-id="6d25d-237">每個運算式的結尾</span><span class="sxs-lookup"><span data-stu-id="6d25d-237">At the end of each expression</span></span>

<span data-ttu-id="6d25d-238">明確指派狀態*v*可以是：</span><span class="sxs-lookup"><span data-stu-id="6d25d-238">The definite assignment state of *v* can be either:</span></span>

*  <span data-ttu-id="6d25d-239">明確指派。</span><span class="sxs-lookup"><span data-stu-id="6d25d-239">Definitely assigned.</span></span> <span data-ttu-id="6d25d-240">這表示在到目前為止，所有可能的控制流程*v*已被指派值。</span><span class="sxs-lookup"><span data-stu-id="6d25d-240">This indicates that on all possible control flows to this point, *v* has been assigned a value.</span></span>
*  <span data-ttu-id="6d25d-241">未明確指派。</span><span class="sxs-lookup"><span data-stu-id="6d25d-241">Not definitely assigned.</span></span> <span data-ttu-id="6d25d-242">變數類型的運算式結尾處的狀態`bool`，歸類到其中一個下列子狀態的變數未明確指派可能 （但不一定） 的狀態：</span><span class="sxs-lookup"><span data-stu-id="6d25d-242">For the state of a variable at the end of an expression of type `bool`, the state of a variable that isn't definitely assigned may (but doesn't necessarily) fall into one of the following sub-states:</span></span>
    * <span data-ttu-id="6d25d-243">明確指派之後，則為 true 的運算式。</span><span class="sxs-lookup"><span data-stu-id="6d25d-243">Definitely assigned after true expression.</span></span> <span data-ttu-id="6d25d-244">此狀態表示*v*已明確指派，如果布林運算式評估為 true，但就不需要指派如果布林運算式評估為 false。</span><span class="sxs-lookup"><span data-stu-id="6d25d-244">This state indicates that *v* is definitely assigned if the boolean expression evaluated as true, but is not necessarily assigned if the boolean expression evaluated as false.</span></span>
    * <span data-ttu-id="6d25d-245">明確指派之後，則為 false 的運算式。</span><span class="sxs-lookup"><span data-stu-id="6d25d-245">Definitely assigned after false expression.</span></span> <span data-ttu-id="6d25d-246">此狀態表示*v*已明確指派，如果布林運算式評估為 false，但就不需要指派如果布林運算式評估為 true。</span><span class="sxs-lookup"><span data-stu-id="6d25d-246">This state indicates that *v* is definitely assigned if the boolean expression evaluated as false, but is not necessarily assigned if the boolean expression evaluated as true.</span></span>

<span data-ttu-id="6d25d-247">下列規則可控制如何狀態變數*v*決定每個位置。</span><span class="sxs-lookup"><span data-stu-id="6d25d-247">The following rules govern how the state of a variable *v* is determined at each location.</span></span>

#### <a name="general-rules-for-statements"></a><span data-ttu-id="6d25d-248">陳述式的一般規則</span><span class="sxs-lookup"><span data-stu-id="6d25d-248">General rules for statements</span></span>

*  <span data-ttu-id="6d25d-249">*v*函式成員主體中的開頭處未明確指派。</span><span class="sxs-lookup"><span data-stu-id="6d25d-249">*v* is not definitely assigned at the beginning of a function member body.</span></span>
*  <span data-ttu-id="6d25d-250">*v*已明確指派任何無法到達陳述式的開頭。</span><span class="sxs-lookup"><span data-stu-id="6d25d-250">*v* is definitely assigned at the beginning of any unreachable statement.</span></span>
*  <span data-ttu-id="6d25d-251">明確指派狀態*v*開頭的任何其他陳述式檢查來判斷是明確指派狀態*v*上所有的開頭為目標的控制流程傳輸陳述式。</span><span class="sxs-lookup"><span data-stu-id="6d25d-251">The definite assignment state of *v* at the beginning of any other statement is determined by checking the definite assignment state of *v* on all control flow transfers that target the beginning of that statement.</span></span> <span data-ttu-id="6d25d-252">如果 （而且只有當） *v*已明確指派所有此類的控制流程傳輸，然後*v*已明確指派的陳述式開頭。</span><span class="sxs-lookup"><span data-stu-id="6d25d-252">If (and only if) *v* is definitely assigned on all such control flow transfers, then *v* is definitely assigned at the beginning of the statement.</span></span> <span data-ttu-id="6d25d-253">檢查陳述式連線能力的相同方式決定可能的控制流程傳輸集合 ([結束點和可執行性](statements.md#end-points-and-reachability))。</span><span class="sxs-lookup"><span data-stu-id="6d25d-253">The set of possible control flow transfers is determined in the same way as for checking statement reachability ([End points and reachability](statements.md#end-points-and-reachability)).</span></span>
*  <span data-ttu-id="6d25d-254">明確指派狀態*v*區塊中，結束點`checked`， `unchecked`， `if`， `while`， `do`， `for`， `foreach`， `lock`， `using`，或`switch`陳述式檢查來判斷是明確指派狀態*v*上所有以該陳述式的結束點為目標的控制流程傳輸。</span><span class="sxs-lookup"><span data-stu-id="6d25d-254">The definite assignment state of *v* at the end point of a block, `checked`, `unchecked`, `if`, `while`, `do`, `for`, `foreach`, `lock`, `using`, or `switch` statement is determined by checking the definite assignment state of *v* on all control flow transfers that target the end point of that statement.</span></span> <span data-ttu-id="6d25d-255">如果*v*已明確指派所有此類的控制流程傳輸，然後*v*已明確指派陳述式的結束點。</span><span class="sxs-lookup"><span data-stu-id="6d25d-255">If *v* is definitely assigned on all such control flow transfers, then *v* is definitely assigned at the end point of the statement.</span></span> <span data-ttu-id="6d25d-256">否則*v*未明確指派陳述式的結束點。</span><span class="sxs-lookup"><span data-stu-id="6d25d-256">Otherwise; *v* is not definitely assigned at the end point of the statement.</span></span> <span data-ttu-id="6d25d-257">檢查陳述式連線能力的相同方式決定可能的控制流程傳輸集合 ([結束點和可執行性](statements.md#end-points-and-reachability))。</span><span class="sxs-lookup"><span data-stu-id="6d25d-257">The set of possible control flow transfers is determined in the same way as for checking statement reachability ([End points and reachability](statements.md#end-points-and-reachability)).</span></span>

#### <a name="block-statements-checked-and-unchecked-statements"></a><span data-ttu-id="6d25d-258">區塊陳述式，檢查和 unchecked 陳述式</span><span class="sxs-lookup"><span data-stu-id="6d25d-258">Block statements, checked, and unchecked statements</span></span>

<span data-ttu-id="6d25d-259">明確指派狀態*v*控制項傳輸至區塊中的陳述式清單的第一個陳述式 （或區塊中，如果陳述式清單是空的結束點） 等同於明確指派陳述式*v*區塊，前面`checked`，或`unchecked`陳述式。</span><span class="sxs-lookup"><span data-stu-id="6d25d-259">The definite assignment state of *v* on the control transfer to the first statement of the statement list in the block (or to the end point of the block, if the statement list is empty) is the same as the definite assignment statement of *v* before the block, `checked`, or `unchecked` statement.</span></span>

#### <a name="expression-statements"></a><span data-ttu-id="6d25d-260">運算式陳述式</span><span class="sxs-lookup"><span data-stu-id="6d25d-260">Expression statements</span></span>

<span data-ttu-id="6d25d-261">運算式陳述式*stmt*構成的運算式*expr*:</span><span class="sxs-lookup"><span data-stu-id="6d25d-261">For an expression statement *stmt* that consists of the expression *expr*:</span></span>

*  <span data-ttu-id="6d25d-262">*v*具有相同的明確指派狀態的開頭*expr*做為開頭*stmt*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-262">*v* has the same definite assignment state at the beginning of *expr* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="6d25d-263">如果*v*如果在結尾明確指派*expr*，它會明確指派的結束點*stmt*; 否則它未明確指派結束點*stmt*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-263">If *v* if definitely assigned at the end of *expr*, it is definitely assigned at the end point of *stmt*; otherwise; it is not definitely assigned at the end point of *stmt*.</span></span>

#### <a name="declaration-statements"></a><span data-ttu-id="6d25d-264">宣告陳述式</span><span class="sxs-lookup"><span data-stu-id="6d25d-264">Declaration statements</span></span>

*  <span data-ttu-id="6d25d-265">如果*stmt*是宣告陳述式沒有初始設定式，然後*v*具有相同的明確指派狀態的結束點*stmt* 開頭做為*stmt*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-265">If *stmt* is a declaration statement without initializers, then *v* has the same definite assignment state at the end point of *stmt* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="6d25d-266">如果*stmt*是宣告陳述式與初始設定式，然後明確指派狀態*v*取決於如同*stmt*是陳述式清單中的，具有一個指派初始設定式 （依照順序宣告） 的每個宣告陳述式。</span><span class="sxs-lookup"><span data-stu-id="6d25d-266">If *stmt* is a declaration statement with initializers, then the definite assignment state for *v* is determined as if *stmt* were a statement list, with one assignment statement for each declaration with an initializer (in the order of declaration).</span></span>

#### <a name="if-statements"></a><span data-ttu-id="6d25d-267">如果陳述式</span><span class="sxs-lookup"><span data-stu-id="6d25d-267">If statements</span></span>

<span data-ttu-id="6d25d-268">針對`if`陳述式*stmt*的表單：</span><span class="sxs-lookup"><span data-stu-id="6d25d-268">For an `if` statement *stmt* of the form:</span></span>
```csharp
if ( expr ) then_stmt else else_stmt
```

*  <span data-ttu-id="6d25d-269">*v*具有相同的明確指派狀態的開頭*expr*做為開頭*stmt*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-269">*v* has the same definite assignment state at the beginning of *expr* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="6d25d-270">如果*v*已明確指派的結尾*expr*，然後它會明確指派在控制流程傳輸至*then_stmt* ，然後*else_stmt*或結尾處*stmt*如果沒有 else 子句。</span><span class="sxs-lookup"><span data-stu-id="6d25d-270">If *v* is definitely assigned at the end of *expr*, then it is definitely assigned on the control flow transfer to *then_stmt* and to either *else_stmt* or to the end-point of *stmt* if there is no else clause.</span></span>
*  <span data-ttu-id="6d25d-271">如果*v*結尾有 「 明確指派，則為 true 的運算式之後 」 的狀態*expr*，然後它會明確指派在控制流程傳輸至*then_stmt*，而非在控制流程傳輸至其中一個已明確指派*else_stmt*或是的高端點*stmt*如果沒有 else 子句。</span><span class="sxs-lookup"><span data-stu-id="6d25d-271">If *v* has the state "definitely assigned after true expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to *then_stmt*, and not definitely assigned on the control flow transfer to either *else_stmt* or to the end-point of *stmt* if there is no else clause.</span></span>
*  <span data-ttu-id="6d25d-272">如果*v*結尾有 「 明確指派 false 的運算式之後 」 的狀態*expr*，然後它會明確指派在控制流程傳輸至*else_stmt*，而非在控制流程傳輸至明確指派*then_stmt*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-272">If *v* has the state "definitely assigned after false expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to *else_stmt*, and not definitely assigned on the control flow transfer to *then_stmt*.</span></span> <span data-ttu-id="6d25d-273">它在端點的明確指派*stmt*才會明確指派的高端點位於*then_stmt*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-273">It is definitely assigned at the end-point of *stmt* if and only if it is definitely assigned at the end-point of *then_stmt*.</span></span>
*  <span data-ttu-id="6d25d-274">否則*v*被視為未明確指派在控制流程傳輸至*then_stmt*或是*else_stmt*，或端點的*stmt*如果沒有 else 子句。</span><span class="sxs-lookup"><span data-stu-id="6d25d-274">Otherwise, *v* is considered not definitely assigned on the control flow transfer to either the *then_stmt* or *else_stmt*, or to the end-point of *stmt* if there is no else clause.</span></span>

#### <a name="switch-statements"></a><span data-ttu-id="6d25d-275">Switch 陳述式</span><span class="sxs-lookup"><span data-stu-id="6d25d-275">Switch statements</span></span>

<span data-ttu-id="6d25d-276">在 `switch`陳述式*stmt*控制運算式具有*expr*:</span><span class="sxs-lookup"><span data-stu-id="6d25d-276">In a `switch` statement *stmt* with a controlling expression *expr*:</span></span>

*  <span data-ttu-id="6d25d-277">明確指派狀態*v*開頭*expr*等同於狀態*v*開頭*stmt*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-277">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="6d25d-278">明確指派狀態*v*上的控制流程傳送至連線到交換器區塊陳述式清單是明確的指派狀態的相同*v*結尾*expr*.</span><span class="sxs-lookup"><span data-stu-id="6d25d-278">The definite assignment state of *v* on the control flow transfer to a reachable switch block statement list is the same as the definite assignment state of *v* at the end of *expr*.</span></span>

#### <a name="while-statements"></a><span data-ttu-id="6d25d-279">Do-while 陳述式</span><span class="sxs-lookup"><span data-stu-id="6d25d-279">While statements</span></span>

<span data-ttu-id="6d25d-280">針對`while`陳述式*stmt*的表單：</span><span class="sxs-lookup"><span data-stu-id="6d25d-280">For a `while` statement *stmt* of the form:</span></span>
```csharp
while ( expr ) while_body
```

*  <span data-ttu-id="6d25d-281">*v*具有相同的明確指派狀態的開頭*expr*做為開頭*stmt*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-281">*v* has the same definite assignment state at the beginning of *expr* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="6d25d-282">如果*v*已明確指派的結尾*expr*，然後它會明確指派在控制流程傳輸至*while_body*並結束點*stmt*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-282">If *v* is definitely assigned at the end of *expr*, then it is definitely assigned on the control flow transfer to *while_body* and to the end point of *stmt*.</span></span>
*  <span data-ttu-id="6d25d-283">如果*v*結尾有 「 明確指派，則為 true 的運算式之後 」 的狀態*expr*，然後它會明確指派在控制流程傳輸至*while_body*，但不是在端點的明確指派*stmt*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-283">If *v* has the state "definitely assigned after true expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to *while_body*, but not definitely assigned at the end-point of *stmt*.</span></span>
*  <span data-ttu-id="6d25d-284">如果*v*結尾有 「 明確指派 false 的運算式之後 」 的狀態*expr*，然後它會明確指派在控制流程傳輸至結束點*stmt*但在控制流程傳輸至未明確指派*while_body*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-284">If *v* has the state "definitely assigned after false expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to the end point of *stmt*, but not definitely assigned on the control flow transfer to *while_body*.</span></span>

#### <a name="do-statements"></a><span data-ttu-id="6d25d-285">陳述式</span><span class="sxs-lookup"><span data-stu-id="6d25d-285">Do statements</span></span>

<span data-ttu-id="6d25d-286">針對`do`陳述式*stmt*的表單：</span><span class="sxs-lookup"><span data-stu-id="6d25d-286">For a `do` statement *stmt* of the form:</span></span>
```csharp
do do_body while ( expr ) ;
```

*  <span data-ttu-id="6d25d-287">*v*具有相同的明確指派狀態，在控制流程傳輸，從開頭*stmt*要*do_body*做為開頭*stmt*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-287">*v* has the same definite assignment state on the control flow transfer from the beginning of *stmt* to *do_body* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="6d25d-288">*v*具有相同的明確指派狀態的開頭*expr*個結束點*do_body*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-288">*v* has the same definite assignment state at the beginning of *expr* as at the end point of *do_body*.</span></span>
*  <span data-ttu-id="6d25d-289">如果*v*已明確指派的結尾*expr*，然後它會明確指派在控制流程傳輸至結束點*stmt*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-289">If *v* is definitely assigned at the end of *expr*, then it is definitely assigned on the control flow transfer to the end point of *stmt*.</span></span>
*  <span data-ttu-id="6d25d-290">如果*v*結尾有 「 明確指派 false 的運算式之後 」 的狀態*expr*，然後它會明確指派在控制流程傳輸至結束點*stmt*.</span><span class="sxs-lookup"><span data-stu-id="6d25d-290">If *v* has the state "definitely assigned after false expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to the end point of *stmt*.</span></span>

#### <a name="for-statements"></a><span data-ttu-id="6d25d-291">陳述式</span><span class="sxs-lookup"><span data-stu-id="6d25d-291">For statements</span></span>

<span data-ttu-id="6d25d-292">正在檢查的明確指派`for`表單的陳述式：</span><span class="sxs-lookup"><span data-stu-id="6d25d-292">Definite assignment checking for a `for` statement of the form:</span></span>
```csharp
for ( for_initializer ; for_condition ; for_iterator ) embedded_statement
```
<span data-ttu-id="6d25d-293">完成所撰寫的陳述式：</span><span class="sxs-lookup"><span data-stu-id="6d25d-293">is done as if the statement were written:</span></span>
```csharp
{
    for_initializer ;
    while ( for_condition ) {
        embedded_statement ;
        for_iterator ;
    }
}
```

<span data-ttu-id="6d25d-294">如果*for_condition*中會省略`for`陳述式，則評估的明確指派繼續如同*for_condition*已被取代`true`上述擴充.</span><span class="sxs-lookup"><span data-stu-id="6d25d-294">If the *for_condition* is omitted from the `for` statement, then evaluation of definite assignment proceeds as if *for_condition* were replaced with `true` in the above expansion.</span></span>

#### <a name="break-continue-and-goto-statements"></a><span data-ttu-id="6d25d-295">中斷、 繼續和 goto 陳述式</span><span class="sxs-lookup"><span data-stu-id="6d25d-295">Break, continue, and goto statements</span></span>

<span data-ttu-id="6d25d-296">明確指派狀態*v*上所造成的控制流程傳輸`break`， `continue`，或`goto`陳述式是明確的指派狀態的相同*v*在陳述式的開頭。</span><span class="sxs-lookup"><span data-stu-id="6d25d-296">The definite assignment state of *v* on the control flow transfer caused by a `break`, `continue`, or `goto` statement is the same as the definite assignment state of *v* at the beginning of the statement.</span></span>

#### <a name="throw-statements"></a><span data-ttu-id="6d25d-297">Throw 陳述式</span><span class="sxs-lookup"><span data-stu-id="6d25d-297">Throw statements</span></span>

<span data-ttu-id="6d25d-298">陳述式*stmt*的表單</span><span class="sxs-lookup"><span data-stu-id="6d25d-298">For a statement *stmt* of the form</span></span>
```csharp
throw expr ;
```

<span data-ttu-id="6d25d-299">明確指派狀態*v*開頭*expr*明確指派狀態的相同*v*開頭*stmt*.</span><span class="sxs-lookup"><span data-stu-id="6d25d-299">The definite assignment state of *v* at the beginning of *expr* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>

#### <a name="return-statements"></a><span data-ttu-id="6d25d-300">Return 陳述式</span><span class="sxs-lookup"><span data-stu-id="6d25d-300">Return statements</span></span>

<span data-ttu-id="6d25d-301">陳述式*stmt*的表單</span><span class="sxs-lookup"><span data-stu-id="6d25d-301">For a statement *stmt* of the form</span></span>
```csharp
return expr ;
```

*  <span data-ttu-id="6d25d-302">明確指派狀態*v*開頭*expr*明確指派狀態的相同*v*開頭*stmt*.</span><span class="sxs-lookup"><span data-stu-id="6d25d-302">The definite assignment state of *v* at the beginning of *expr* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="6d25d-303">如果*v*是一個 output 參數，則必須將它明確地指派：</span><span class="sxs-lookup"><span data-stu-id="6d25d-303">If *v* is an output parameter, then it must be definitely assigned either:</span></span>
    * <span data-ttu-id="6d25d-304">之後*expr*</span><span class="sxs-lookup"><span data-stu-id="6d25d-304">after *expr*</span></span>
    * <span data-ttu-id="6d25d-305">或在結尾`finally`區塊`try` - `finally`或是`try` - `catch` - `finally`圍住`return`陳述式。</span><span class="sxs-lookup"><span data-stu-id="6d25d-305">or at the end of the `finally` block of a `try`-`finally` or `try`-`catch`-`finally` that encloses the `return` statement.</span></span>

<span data-ttu-id="6d25d-306">為表單的陳述式陳述式：</span><span class="sxs-lookup"><span data-stu-id="6d25d-306">For a statement stmt of the form:</span></span>
```csharp
return ;
```

*  <span data-ttu-id="6d25d-307">如果*v*是一個 output 參數，則必須將它明確地指派：</span><span class="sxs-lookup"><span data-stu-id="6d25d-307">If *v* is an output parameter, then it must be definitely assigned either:</span></span>
    * <span data-ttu-id="6d25d-308">before *stmt*</span><span class="sxs-lookup"><span data-stu-id="6d25d-308">before *stmt*</span></span>
    * <span data-ttu-id="6d25d-309">或在結尾`finally`區塊`try` - `finally`或是`try` - `catch` - `finally`圍住`return`陳述式。</span><span class="sxs-lookup"><span data-stu-id="6d25d-309">or at the end of the `finally` block of a `try`-`finally` or `try`-`catch`-`finally` that encloses the `return` statement.</span></span>

#### <a name="try-catch-statements"></a><span data-ttu-id="6d25d-310">Try catch 陳述式</span><span class="sxs-lookup"><span data-stu-id="6d25d-310">Try-catch statements</span></span>

<span data-ttu-id="6d25d-311">陳述式*stmt*的表單：</span><span class="sxs-lookup"><span data-stu-id="6d25d-311">For a statement *stmt* of the form:</span></span>
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
```

*  <span data-ttu-id="6d25d-312">明確指派狀態*v*開頭*try_block*明確指派狀態的相同*v*開頭*stmt*.</span><span class="sxs-lookup"><span data-stu-id="6d25d-312">The definite assignment state of *v* at the beginning of *try_block* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="6d25d-313">明確指派狀態*v*開頭*catch_block_i* (針對任何*我*) 的明確指派狀態相同*v*開頭*stmt*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-313">The definite assignment state of *v* at the beginning of *catch_block_i* (for any *i*) is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="6d25d-314">明確指派狀態*v*在結尾處*stmt*是明確指派 （並且唯有） *v*明確指派的高端點*try_block*和每*catch_block_i* (針對每個*我*從 1 到*n*)。</span><span class="sxs-lookup"><span data-stu-id="6d25d-314">The definite assignment state of *v* at the end-point of *stmt* is definitely assigned if (and only if) *v* is definitely assigned at the end-point of *try_block* and every *catch_block_i* (for every *i* from 1 to *n*).</span></span>

#### <a name="try-finally-statements"></a><span data-ttu-id="6d25d-315">Try finally 陳述式</span><span class="sxs-lookup"><span data-stu-id="6d25d-315">Try-finally statements</span></span>

<span data-ttu-id="6d25d-316">針對`try`陳述式*stmt*的表單：</span><span class="sxs-lookup"><span data-stu-id="6d25d-316">For a `try` statement *stmt* of the form:</span></span>
```csharp
try try_block finally finally_block
```

*  <span data-ttu-id="6d25d-317">明確指派狀態*v*開頭*try_block*明確指派狀態的相同*v*開頭*stmt*.</span><span class="sxs-lookup"><span data-stu-id="6d25d-317">The definite assignment state of *v* at the beginning of *try_block* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="6d25d-318">明確指派狀態*v*開頭*finally_block*明確指派狀態的相同*v*開頭*陳述式*.</span><span class="sxs-lookup"><span data-stu-id="6d25d-318">The definite assignment state of *v* at the beginning of *finally_block* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="6d25d-319">明確指派狀態*v*在結尾處*stmt*是明確指派 （並且唯有） 至少下列其中一項條件成立：</span><span class="sxs-lookup"><span data-stu-id="6d25d-319">The definite assignment state of *v* at the end-point of *stmt* is definitely assigned if (and only if) at least one of the following is true:</span></span>
    * <span data-ttu-id="6d25d-320">*v*明確指派的高端點*try_block*</span><span class="sxs-lookup"><span data-stu-id="6d25d-320">*v* is definitely assigned at the end-point of *try_block*</span></span>
    * <span data-ttu-id="6d25d-321">*v*明確指派的高端點*finally_block*</span><span class="sxs-lookup"><span data-stu-id="6d25d-321">*v* is definitely assigned at the end-point of *finally_block*</span></span>

<span data-ttu-id="6d25d-322">如果控制流程傳輸 (例如`goto`陳述式) 已變更內開始*try_block*，結束外部*try_block*，然後*v*也是如果被視為已明確指派上該控制流程傳輸*v*明確指派的高端點*finally_block*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-322">If a control flow transfer (for example, a `goto` statement) is made that begins within *try_block*, and ends outside of *try_block*, then *v* is also considered definitely assigned on that control flow transfer if *v* is definitely assigned at the end-point of *finally_block*.</span></span> <span data-ttu-id="6d25d-323">(這不是唯一的 if — 如果*v*明確指派的另一個原因，在 控制流程移轉後，則它仍視為已明確指派。)</span><span class="sxs-lookup"><span data-stu-id="6d25d-323">(This is not an only if—if *v* is definitely assigned for another reason on this control flow transfer, then it is still considered definitely assigned.)</span></span>

#### <a name="try-catch-finally-statements"></a><span data-ttu-id="6d25d-324">請嘗試為 try-catch-finally 陳述式</span><span class="sxs-lookup"><span data-stu-id="6d25d-324">Try-catch-finally statements</span></span>

<span data-ttu-id="6d25d-325">明確指派分析`try` - `catch` - `finally`表單的陳述式：</span><span class="sxs-lookup"><span data-stu-id="6d25d-325">Definite assignment analysis for a `try`-`catch`-`finally` statement of the form:</span></span>
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
finally *finally_block*
```
<span data-ttu-id="6d25d-326">完成陳述式所`try` - `finally`封入陳述式`try` - `catch`陳述式：</span><span class="sxs-lookup"><span data-stu-id="6d25d-326">is done as if the statement were a `try`-`finally` statement enclosing a `try`-`catch` statement:</span></span>
```csharp
try {
    try try_block
    catch(...) catch_block_1
    ...
    catch(...) catch_block_n
}
finally finally_block
```

<span data-ttu-id="6d25d-327">下列範例示範如何不同的區塊`try`陳述式 ([try 陳述式](statements.md#the-try-statement)) 會影響明確指派。</span><span class="sxs-lookup"><span data-stu-id="6d25d-327">The following example demonstrates how the different blocks of a `try` statement ([The try statement](statements.md#the-try-statement)) affect definite assignment.</span></span>
```csharp
class A
{
    static void F() {
        int i, j;
        try {
            goto LABEL;
            // neither i nor j definitely assigned
            i = 1;
            // i definitely assigned
        }

        catch {
            // neither i nor j definitely assigned
            i = 3;
            // i definitely assigned
        }

        finally {
            // neither i nor j definitely assigned
            j = 5;
            // j definitely assigned
            }
        // i and j definitely assigned
        LABEL:;
        // j definitely assigned

    }
}
```

#### <a name="foreach-statements"></a><span data-ttu-id="6d25d-328">Foreach 陳述式</span><span class="sxs-lookup"><span data-stu-id="6d25d-328">Foreach statements</span></span>

<span data-ttu-id="6d25d-329">針對`foreach`陳述式*stmt*的表單：</span><span class="sxs-lookup"><span data-stu-id="6d25d-329">For a `foreach` statement *stmt* of the form:</span></span>
```csharp
foreach ( type identifier in expr ) embedded_statement
```

*  <span data-ttu-id="6d25d-330">明確指派狀態*v*開頭*expr*等同於狀態*v*開頭*stmt*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-330">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="6d25d-331">明確指派狀態*v*在控制流程傳輸至*embedded_statement*或結束點*stmt*的狀態相同*v*結尾處*expr*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-331">The definite assignment state of *v* on the control flow transfer to *embedded_statement* or to the end point of *stmt* is the same as the state of *v* at the end of *expr*.</span></span>

#### <a name="using-statements"></a><span data-ttu-id="6d25d-332">Using 陳述式</span><span class="sxs-lookup"><span data-stu-id="6d25d-332">Using statements</span></span>

<span data-ttu-id="6d25d-333">針對`using`陳述式*stmt*的表單：</span><span class="sxs-lookup"><span data-stu-id="6d25d-333">For a `using` statement *stmt* of the form:</span></span>
```csharp
using ( resource_acquisition ) embedded_statement
```

*  <span data-ttu-id="6d25d-334">明確指派狀態*v*開頭*resource_acquisition*等同於狀態*v*開頭*stmt*.</span><span class="sxs-lookup"><span data-stu-id="6d25d-334">The definite assignment state of *v* at the beginning of *resource_acquisition* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="6d25d-335">明確指派狀態*v*在控制流程傳輸至*embedded_statement*等同於狀態*v*結尾*resource_取得*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-335">The definite assignment state of *v* on the control flow transfer to *embedded_statement* is the same as the state of *v* at the end of *resource_acquisition*.</span></span>

#### <a name="lock-statements"></a><span data-ttu-id="6d25d-336">Lock 陳述式</span><span class="sxs-lookup"><span data-stu-id="6d25d-336">Lock statements</span></span>

<span data-ttu-id="6d25d-337">針對`lock`陳述式*stmt*的表單：</span><span class="sxs-lookup"><span data-stu-id="6d25d-337">For a `lock` statement *stmt* of the form:</span></span>
```csharp
lock ( expr ) embedded_statement
```

*  <span data-ttu-id="6d25d-338">明確指派狀態*v*開頭*expr*等同於狀態*v*開頭*stmt*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-338">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="6d25d-339">明確指派狀態*v*在控制流程傳輸至*embedded_statement*等同於狀態*v*結尾*expr*.</span><span class="sxs-lookup"><span data-stu-id="6d25d-339">The definite assignment state of *v* on the control flow transfer to *embedded_statement* is the same as the state of *v* at the end of *expr*.</span></span>

#### <a name="yield-statements"></a><span data-ttu-id="6d25d-340">Yield 陳述式</span><span class="sxs-lookup"><span data-stu-id="6d25d-340">Yield statements</span></span>

<span data-ttu-id="6d25d-341">針對`yield return`陳述式*stmt*的表單：</span><span class="sxs-lookup"><span data-stu-id="6d25d-341">For a `yield return` statement *stmt* of the form:</span></span>
```csharp
yield return expr ;
```

*  <span data-ttu-id="6d25d-342">明確指派狀態*v*開頭*expr*等同於狀態*v*開頭*stmt*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-342">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="6d25d-343">明確指派狀態*v*結尾*stmt*的狀態相同*v*結尾*expr*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-343">The definite assignment state of *v* at the end of *stmt* is the same as the state of *v* at the end of *expr*.</span></span>
*  <span data-ttu-id="6d25d-344">A`yield break`陳述式已明確指派狀態不會影響。</span><span class="sxs-lookup"><span data-stu-id="6d25d-344">A `yield break` statement has no effect on the definite assignment state.</span></span>

#### <a name="general-rules-for-simple-expressions"></a><span data-ttu-id="6d25d-345">簡單運算式的一般規則</span><span class="sxs-lookup"><span data-stu-id="6d25d-345">General rules for simple expressions</span></span>

<span data-ttu-id="6d25d-346">下列規則適用於這些種類的運算式： 常值 ([常值](expressions.md#literals))，簡單名稱 ([簡單名稱](expressions.md#simple-names))，成員存取運算式 ([成員存取](expressions.md#member-access))，非編製索引的基底存取運算式 ([基底存取](expressions.md#base-access))，`typeof`運算式 ([typeof 運算子](expressions.md#the-typeof-operator))，預設值運算式 ([預設值運算式](expressions.md#default-value-expressions)) 和`nameof`運算式 ([Nameof 運算式](expressions.md#nameof-expressions))。</span><span class="sxs-lookup"><span data-stu-id="6d25d-346">The following rule applies to these kinds of expressions: literals ([Literals](expressions.md#literals)), simple names ([Simple names](expressions.md#simple-names)), member access expressions ([Member access](expressions.md#member-access)), non-indexed base access expressions ([Base access](expressions.md#base-access)), `typeof` expressions ([The typeof operator](expressions.md#the-typeof-operator)), default value expressions ([Default value expressions](expressions.md#default-value-expressions)) and `nameof` expressions ([Nameof expressions](expressions.md#nameof-expressions)).</span></span>

*  <span data-ttu-id="6d25d-347">明確指派狀態*v*結尾的這類運算式會明確指派狀態的相同*v*運算式的開頭。</span><span class="sxs-lookup"><span data-stu-id="6d25d-347">The definite assignment state of *v* at the end of such an expression is the same as the definite assignment state of *v* at the beginning of the expression.</span></span>

#### <a name="general-rules-for-expressions-with-embedded-expressions"></a><span data-ttu-id="6d25d-348">一般規則運算式使用內嵌的運算式</span><span class="sxs-lookup"><span data-stu-id="6d25d-348">General rules for expressions with embedded expressions</span></span>

<span data-ttu-id="6d25d-349">下列規則適用於這些種類的運算式： 括號括住運算式 ([括號括住運算式](expressions.md#parenthesized-expressions))，元素存取運算式 ([項目存取](expressions.md#element-access))、 基底存取運算式編製索引 ([基底存取](expressions.md#base-access))、 遞增和遞減運算式 ([後置遞增和遞減運算子](expressions.md#postfix-increment-and-decrement-operators)，[前置遞增和遞減運算子](expressions.md#prefix-increment-and-decrement-operators))，轉換運算式 ([轉型運算式](expressions.md#cast-expressions))，一元`+`， `-`， `~`，`*`二進位運算式`+`， `-`， `*`，`/`, `%`, `<<`, `>>`, `<`, `<=`, `>`, `>=`, `==`, `!=`, `is`, `as`, `&`， `|`，`^`運算式 ([算術運算子](expressions.md#arithmetic-operators)，[移位運算子](expressions.md#shift-operators)，[關係和類型測試運算子](expressions.md#relational-and-type-testing-operators)[邏輯運算子](expressions.md#logical-operators))，複合指派運算式 ([複合指派](expressions.md#compound-assignment))，`checked`並`unchecked`運算式 ([checked 與 unchecked運算子](expressions.md#the-checked-and-unchecked-operators))，再加上陣列和委派建立運算式 ([new 運算子](expressions.md#the-new-operator))。</span><span class="sxs-lookup"><span data-stu-id="6d25d-349">The following rules apply to these kinds of expressions: parenthesized expressions ([Parenthesized expressions](expressions.md#parenthesized-expressions)), element access expressions ([Element access](expressions.md#element-access)), base access expressions with indexing ([Base access](expressions.md#base-access)), increment and decrement expressions ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators), [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), cast expressions ([Cast expressions](expressions.md#cast-expressions)), unary `+`, `-`, `~`, `*` expressions, binary `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `<`, `<=`, `>`, `>=`, `==`, `!=`, `is`, `as`, `&`, `|`, `^` expressions ([Arithmetic operators](expressions.md#arithmetic-operators), [Shift operators](expressions.md#shift-operators), [Relational and type-testing operators](expressions.md#relational-and-type-testing-operators), [Logical operators](expressions.md#logical-operators)), compound assignment expressions ([Compound assignment](expressions.md#compound-assignment)), `checked` and `unchecked` expressions ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)), plus array and delegate creation expressions ([The new operator](expressions.md#the-new-operator)).</span></span>

<span data-ttu-id="6d25d-350">每個運算式有一或多個固定的順序會無條件地進行評估的子運算式。</span><span class="sxs-lookup"><span data-stu-id="6d25d-350">Each of these expressions has one or more sub-expressions that are unconditionally evaluated in a fixed order.</span></span> <span data-ttu-id="6d25d-351">例如，二進位檔`%`左手邊的運算子，則右手邊，運算子會評估。</span><span class="sxs-lookup"><span data-stu-id="6d25d-351">For example, the binary `%` operator evaluates the left hand side of the operator, then the right hand side.</span></span> <span data-ttu-id="6d25d-352">編製索引的作業會評估索引的運算式，並接著會評估每個索引運算式中，從左到右的順序。</span><span class="sxs-lookup"><span data-stu-id="6d25d-352">An indexing operation evaluates the indexed expression, and then evaluates each of the index expressions, in order from left to right.</span></span> <span data-ttu-id="6d25d-353">運算式*expr*，其中包含子運算式*e1、 e2，...，eN*、 依序評估：</span><span class="sxs-lookup"><span data-stu-id="6d25d-353">For an expression *expr*, which has sub-expressions *e1, e2, ..., eN*, evaluated in that order:</span></span>

*  <span data-ttu-id="6d25d-354">明確指派狀態*v*開頭*e1*等同於在開頭的明確指派狀態*expr*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-354">The definite assignment state of *v* at the beginning of *e1* is the same as the definite assignment state at the beginning of *expr*.</span></span>
*  <span data-ttu-id="6d25d-355">明確指派狀態*v*開頭*ei* (*我*大於一) 等同於先前的子運算式的結尾的明確指派狀態。</span><span class="sxs-lookup"><span data-stu-id="6d25d-355">The definite assignment state of *v* at the beginning of *ei* (*i* greater than one) is the same as the definite assignment state at the end of the previous sub-expression.</span></span>
*  <span data-ttu-id="6d25d-356">明確指派狀態*v*結尾*expr*等同於在結尾處的明確指派狀態*eN*</span><span class="sxs-lookup"><span data-stu-id="6d25d-356">The definite assignment state of *v* at the end of *expr* is the same as the definite assignment state at the end of *eN*</span></span>

#### <a name="invocation-expressions-and-object-creation-expressions"></a><span data-ttu-id="6d25d-357">引動過程運算式和物件建立運算式</span><span class="sxs-lookup"><span data-stu-id="6d25d-357">Invocation expressions and object creation expressions</span></span>

<span data-ttu-id="6d25d-358">引動過程運算式*expr*的表單：</span><span class="sxs-lookup"><span data-stu-id="6d25d-358">For an invocation expression *expr* of the form:</span></span>
```csharp
primary_expression ( arg1 , arg2 , ... , argN )
```
<span data-ttu-id="6d25d-359">或表單的物件建立運算式：</span><span class="sxs-lookup"><span data-stu-id="6d25d-359">or an object creation expression of the form:</span></span>
```csharp
new type ( arg1 , arg2 , ... , argN )
```

*  <span data-ttu-id="6d25d-360">引動過程運算式，明確指派狀態*v*之前*primary_expression*等同於狀態*v*之前*expr*.</span><span class="sxs-lookup"><span data-stu-id="6d25d-360">For an invocation expression, the definite assignment state of *v* before *primary_expression* is the same as the state of *v* before *expr*.</span></span>
*  <span data-ttu-id="6d25d-361">引動過程運算式，明確指派狀態*v*之前*arg1*等同於狀態*v*之後*primary_expression*.</span><span class="sxs-lookup"><span data-stu-id="6d25d-361">For an invocation expression, the definite assignment state of *v* before *arg1* is the same as the state of *v* after *primary_expression*.</span></span>
*  <span data-ttu-id="6d25d-362">為物件建立運算式，明確指派狀態*v*之前*arg1*等同於狀態*v*之前*expr*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-362">For an object creation expression, the definite assignment state of *v* before *arg1* is the same as the state of *v* before *expr*.</span></span>
*  <span data-ttu-id="6d25d-363">每個引數*argi*，明確指派狀態*v*之後*argi*取決於一般的運算式規則，並忽略任何`ref`或`out`修飾詞。</span><span class="sxs-lookup"><span data-stu-id="6d25d-363">For each argument *argi*, the definite assignment state of *v* after *argi* is determined by the normal expression rules, ignoring any `ref` or `out` modifiers.</span></span>
*  <span data-ttu-id="6d25d-364">每個引數*argi*任何*我*大於一，明確指派狀態*v*之前*argi*相同的狀態*v*之後先前*arg*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-364">For each argument *argi* for any *i* greater than one, the definite assignment state of *v* before *argi* is the same as the state of *v* after the previous *arg*.</span></span>
*  <span data-ttu-id="6d25d-365">如果變數*v*被當做`out`引數 (也就是表單的引數`out v`) 中的引數，則狀態的任何*v*之後*expr*已明確指派。</span><span class="sxs-lookup"><span data-stu-id="6d25d-365">If the variable *v* is passed as an `out` argument (i.e., an argument of the form `out v`) in any of the arguments, then the state of *v* after *expr* is definitely assigned.</span></span> <span data-ttu-id="6d25d-366">否則狀態*v*之後*expr*等同於狀態*v*之後*argN*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-366">Otherwise; the state of *v* after *expr* is the same as the state of *v* after *argN*.</span></span>
*  <span data-ttu-id="6d25d-367">陣列初始設定式 ([陣列建立運算式](expressions.md#array-creation-expressions))，物件初始設定式 ([物件初始設定式](expressions.md#object-initializers))，集合初始設定式 ([集合初始設定式](expressions.md#collection-initializers)) 和匿名物件初始設定式 ([匿名物件建立運算式](expressions.md#anonymous-object-creation-expressions))，明確指派狀態取決於這些建構所定義的擴充。</span><span class="sxs-lookup"><span data-stu-id="6d25d-367">For array initializers ([Array creation expressions](expressions.md#array-creation-expressions)), object initializers ([Object initializers](expressions.md#object-initializers)), collection initializers ([Collection initializers](expressions.md#collection-initializers)) and anonymous object initializers ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)), the definite assignment state is determined by the expansion that these constructs are defined in terms of.</span></span>

#### <a name="simple-assignment-expressions"></a><span data-ttu-id="6d25d-368">簡單指派運算式</span><span class="sxs-lookup"><span data-stu-id="6d25d-368">Simple assignment expressions</span></span>

<span data-ttu-id="6d25d-369">運算式*expr*表單的`w = expr_rhs`:</span><span class="sxs-lookup"><span data-stu-id="6d25d-369">For an expression *expr* of the form `w = expr_rhs`:</span></span>

*  <span data-ttu-id="6d25d-370">明確指派狀態*v*之前*expr_rhs*明確指派狀態的相同*v*之前*expr*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-370">The definite assignment state of *v* before *expr_rhs* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="6d25d-371">明確指派狀態*v*之後*expr*取決於：</span><span class="sxs-lookup"><span data-stu-id="6d25d-371">The definite assignment state of *v* after *expr* is determined by:</span></span>
   * <span data-ttu-id="6d25d-372">如果*w*是做為相同的變數*v*，然後明確指派的狀態*v*之後*expr*明確指派。</span><span class="sxs-lookup"><span data-stu-id="6d25d-372">If *w* is the same variable as *v*, then the definite assignment state of *v* after *expr* is definitely assigned.</span></span>
   * <span data-ttu-id="6d25d-373">否則，如果指派就會發生結構類型的執行個體建構函式內，如果*w*是指定的自動實作的屬性的屬性存取*P*所建構的執行個體上並*v*是隱藏的支援欄位*P*，然後明確指派的狀態*v*之後*expr*絕對指派。</span><span class="sxs-lookup"><span data-stu-id="6d25d-373">Otherwise, if the assignment occurs within the instance constructor of a struct type, if *w* is a property access designating an automatically implemented property *P* on the instance being constructed and *v* is the hidden backing field of *P*, then the definite assignment state of *v* after *expr* is definitely assigned.</span></span>
   * <span data-ttu-id="6d25d-374">否則，明確指派狀態*v*之後*expr*明確指派狀態的相同*v*之後*expr_rhs*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-374">Otherwise, the definite assignment state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_rhs*.</span></span>

#### <a name="-conditional-and-expressions"></a><span data-ttu-id="6d25d-375">& & (條件式 AND) 運算式</span><span class="sxs-lookup"><span data-stu-id="6d25d-375">&& (conditional AND) expressions</span></span>

<span data-ttu-id="6d25d-376">運算式*expr*表單的`expr_first && expr_second`:</span><span class="sxs-lookup"><span data-stu-id="6d25d-376">For an expression *expr* of the form `expr_first && expr_second`:</span></span>

*  <span data-ttu-id="6d25d-377">明確指派狀態*v*之前*expr_first*明確指派狀態的相同*v*之前*expr*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-377">The definite assignment state of *v* before *expr_first* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="6d25d-378">明確指派狀態*v*之前*expr_second*如果已明確指派的狀態*v*之後*expr_first*是明確指派或者 「 已明確指派之後，則為 true 的運算式"。</span><span class="sxs-lookup"><span data-stu-id="6d25d-378">The definite assignment state of *v* before *expr_second* is definitely assigned if the state of *v* after *expr_first* is either definitely assigned or "definitely assigned after true expression".</span></span> <span data-ttu-id="6d25d-379">否則，它未明確指派。</span><span class="sxs-lookup"><span data-stu-id="6d25d-379">Otherwise, it is not definitely assigned.</span></span>
*  <span data-ttu-id="6d25d-380">明確指派狀態*v*之後*expr*取決於：</span><span class="sxs-lookup"><span data-stu-id="6d25d-380">The definite assignment state of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="6d25d-381">如果*expr_first*的值是常數運算式`false`，然後明確指派的狀態*v*之後*expr*明確指派相同狀態*v*之後*expr_first*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-381">If *expr_first* is a constant expression with the value `false`, then the definite assignment state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_first*.</span></span>
    * <span data-ttu-id="6d25d-382">否則，如果狀態*v*之後*expr_first*已明確指派，然後狀態*v*之後*expr*明確指派。</span><span class="sxs-lookup"><span data-stu-id="6d25d-382">Otherwise, if the state of *v* after *expr_first* is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="6d25d-383">否則，如果狀態*v*之後*expr_second*已明確指派，和狀態*v*之後*expr_first* "絕對已指派，則為 false 的運算式之後 」，則狀態*v*之後*expr*明確指派。</span><span class="sxs-lookup"><span data-stu-id="6d25d-383">Otherwise, if the state of *v* after *expr_second* is definitely assigned, and the state of *v* after *expr_first* is "definitely assigned after false expression", then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="6d25d-384">否則，如果狀態*v*之後*expr_second*明確指派或"，則為 true 的運算式之後明確指派 」，則狀態*v*之後*expr* "肯定之後被指派，則為 true 的運算式"。</span><span class="sxs-lookup"><span data-stu-id="6d25d-384">Otherwise, if the state of *v* after *expr_second* is definitely assigned or "definitely assigned after true expression", then the state of *v* after *expr* is "definitely assigned after true expression".</span></span>
    * <span data-ttu-id="6d25d-385">否則，如果狀態*v*之後*expr_first*是 」，則為 false 的運算式之後明確指派 」，和狀態*v*之後*expr_second* "，則為 false 的運算式之後明確指派 」，則狀態*v*之後*expr* 「 明確指派 false 的運算式之後 」。</span><span class="sxs-lookup"><span data-stu-id="6d25d-385">Otherwise, if the state of *v* after *expr_first* is "definitely assigned after false expression", and the state of *v* after *expr_second* is "definitely assigned after false expression", then the state of *v* after *expr* is "definitely assigned after false expression".</span></span>
    * <span data-ttu-id="6d25d-386">否則，狀態*v*之後*expr*未明確指派。</span><span class="sxs-lookup"><span data-stu-id="6d25d-386">Otherwise, the state of *v* after *expr* is not definitely assigned.</span></span>

<span data-ttu-id="6d25d-387">在範例</span><span class="sxs-lookup"><span data-stu-id="6d25d-387">In the example</span></span>
```csharp
class A
{
    static void F(int x, int y) {
        int i;
        if (x >= 0 && (i = y) >= 0) {
            // i definitely assigned
        }
        else {
            // i not definitely assigned
        }
        // i not definitely assigned
    }
}
```
<span data-ttu-id="6d25d-388">變數`i`會被視為已明確指派其中一個內嵌的陳述式的`if`陳述式而不是在其他。</span><span class="sxs-lookup"><span data-stu-id="6d25d-388">the variable `i` is considered definitely assigned in one of the embedded statements of an `if` statement but not in the other.</span></span> <span data-ttu-id="6d25d-389">中`if`方法中的陳述式`F`，該變數`i`明確指派第一個內嵌陳述式因為運算式的執行`(i = y)`一律會在執行內嵌陳述式之前。</span><span class="sxs-lookup"><span data-stu-id="6d25d-389">In the `if` statement in method `F`, the variable `i` is definitely assigned in the first embedded statement because execution of the expression `(i = y)` always precedes execution of this embedded statement.</span></span> <span data-ttu-id="6d25d-390">相較之下，變數`i`未明確指派在第二個內嵌的陳述式，因為`x >= 0`測試結果為 false，導致變數`i`未獲指派。</span><span class="sxs-lookup"><span data-stu-id="6d25d-390">In contrast, the variable `i` is not definitely assigned in the second embedded statement, since `x >= 0` might have tested false, resulting in the variable `i` being unassigned.</span></span>

#### <a name="-conditional-or-expressions"></a><span data-ttu-id="6d25d-391">||(條件式 OR) 運算式</span><span class="sxs-lookup"><span data-stu-id="6d25d-391">|| (conditional OR) expressions</span></span>

<span data-ttu-id="6d25d-392">運算式*expr*表單的`expr_first || expr_second`:</span><span class="sxs-lookup"><span data-stu-id="6d25d-392">For an expression *expr* of the form `expr_first || expr_second`:</span></span>

*  <span data-ttu-id="6d25d-393">明確指派狀態*v*之前*expr_first*明確指派狀態的相同*v*之前*expr*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-393">The definite assignment state of *v* before *expr_first* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="6d25d-394">明確指派狀態*v*之前*expr_second*如果已明確指派的狀態*v*之後*expr_first*是明確指派或者 「 已明確指派 false 的運算式之後"。</span><span class="sxs-lookup"><span data-stu-id="6d25d-394">The definite assignment state of *v* before *expr_second* is definitely assigned if the state of *v* after *expr_first* is either definitely assigned or "definitely assigned after false expression".</span></span> <span data-ttu-id="6d25d-395">否則，它未明確指派。</span><span class="sxs-lookup"><span data-stu-id="6d25d-395">Otherwise, it is not definitely assigned.</span></span>
*  <span data-ttu-id="6d25d-396">明確指派陳述式*v*之後*expr*取決於：</span><span class="sxs-lookup"><span data-stu-id="6d25d-396">The definite assignment statement of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="6d25d-397">如果*expr_first*的值是常數運算式`true`，然後明確指派的狀態*v*之後*expr*明確指派相同狀態*v*之後*expr_first*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-397">If *expr_first* is a constant expression with the value `true`, then the definite assignment state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_first*.</span></span>
    * <span data-ttu-id="6d25d-398">否則，如果狀態*v*之後*expr_first*已明確指派，然後狀態*v*之後*expr*明確指派。</span><span class="sxs-lookup"><span data-stu-id="6d25d-398">Otherwise, if the state of *v* after *expr_first* is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="6d25d-399">否則，如果狀態*v*之後*expr_second*已明確指派，和狀態*v*之後*expr_first* "絕對已指派，則為 true 的運算式之後 」，則狀態*v*之後*expr*明確指派。</span><span class="sxs-lookup"><span data-stu-id="6d25d-399">Otherwise, if the state of *v* after *expr_second* is definitely assigned, and the state of *v* after *expr_first* is "definitely assigned after true expression", then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="6d25d-400">否則，如果狀態*v*之後*expr_second*明確指派或 「 false 的運算式之後明確指派 」，則狀態*v*之後*expr* 「 明確指派 false 的運算式之後 」。</span><span class="sxs-lookup"><span data-stu-id="6d25d-400">Otherwise, if the state of *v* after *expr_second* is definitely assigned or "definitely assigned after false expression", then the state of *v* after *expr* is "definitely assigned after false expression".</span></span>
    * <span data-ttu-id="6d25d-401">否則，如果狀態*v*之後*expr_first*是 」，則為 true 的運算式之後明確指派 」，和狀態*v*之後*expr_second*"，則為 true 的運算式之後明確指派 」，則狀態*v*之後*expr* "肯定之後被指派，則為 true 的運算式"。</span><span class="sxs-lookup"><span data-stu-id="6d25d-401">Otherwise, if the state of *v* after *expr_first* is "definitely assigned after true expression", and the state of *v* after *expr_second* is "definitely assigned after true expression", then the state of *v* after *expr* is "definitely assigned after true expression".</span></span>
    * <span data-ttu-id="6d25d-402">否則，狀態*v*之後*expr*未明確指派。</span><span class="sxs-lookup"><span data-stu-id="6d25d-402">Otherwise, the state of *v* after *expr* is not definitely assigned.</span></span>

<span data-ttu-id="6d25d-403">在範例</span><span class="sxs-lookup"><span data-stu-id="6d25d-403">In the example</span></span>
```csharp
class A
{
    static void G(int x, int y) {
        int i;
        if (x >= 0 || (i = y) >= 0) {
            // i not definitely assigned
        }
        else {
            // i definitely assigned
        }
        // i not definitely assigned
    }
}
```
<span data-ttu-id="6d25d-404">變數`i`會被視為已明確指派其中一個內嵌的陳述式的`if`陳述式而不是在其他。</span><span class="sxs-lookup"><span data-stu-id="6d25d-404">the variable `i` is considered definitely assigned in one of the embedded statements of an `if` statement but not in the other.</span></span> <span data-ttu-id="6d25d-405">在`if`方法中的陳述式`G`，變數`i`因為已明確指派第二個內嵌的陳述式中執行運算式`(i = y)`一律會在執行內嵌陳述式之前。</span><span class="sxs-lookup"><span data-stu-id="6d25d-405">In the `if` statement in method `G`, the variable `i` is definitely assigned in the second embedded statement because execution of the expression `(i = y)` always precedes execution of this embedded statement.</span></span> <span data-ttu-id="6d25d-406">相較之下，變數`i`未明確指派第一個內嵌陳述式，因為`x >= 0`測試結果為 true，導致變數`i`未獲指派。</span><span class="sxs-lookup"><span data-stu-id="6d25d-406">In contrast, the variable `i` is not definitely assigned in the first embedded statement, since `x >= 0` might have tested true, resulting in the variable `i` being unassigned.</span></span>

#### <a name="-logical-negation-expressions"></a><span data-ttu-id="6d25d-407">!</span><span class="sxs-lookup"><span data-stu-id="6d25d-407">!</span></span> <span data-ttu-id="6d25d-408">（邏輯否定） 運算式</span><span class="sxs-lookup"><span data-stu-id="6d25d-408">(logical negation) expressions</span></span>

<span data-ttu-id="6d25d-409">運算式*expr*表單的`! expr_operand`:</span><span class="sxs-lookup"><span data-stu-id="6d25d-409">For an expression *expr* of the form `! expr_operand`:</span></span>

*  <span data-ttu-id="6d25d-410">明確指派狀態*v*之前*expr_operand*明確指派狀態的相同*v*之前*expr*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-410">The definite assignment state of *v* before *expr_operand* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="6d25d-411">明確指派狀態*v*之後*expr*取決於：</span><span class="sxs-lookup"><span data-stu-id="6d25d-411">The definite assignment state of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="6d25d-412">如果狀態*v*之後 \* expr_operand \* 已明確指派，然後狀態*v*之後*expr*明確指派。</span><span class="sxs-lookup"><span data-stu-id="6d25d-412">If the state of *v* after \*expr_operand \*is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="6d25d-413">如果狀態*v*之後 \* expr_operand \* 未明確指派，然後狀態*v*之後*expr*未明確指派。</span><span class="sxs-lookup"><span data-stu-id="6d25d-413">If the state of *v* after \*expr_operand \*is not definitely assigned, then the state of *v* after *expr* is not definitely assigned.</span></span>
    * <span data-ttu-id="6d25d-414">如果狀態*v*之後 \* expr_operand *"，則為 false 的運算式之後明確指派 」，則狀態*v*之後*expr\* "肯定之後被指派 true運算式"。</span><span class="sxs-lookup"><span data-stu-id="6d25d-414">If the state of *v* after \*expr_operand \*is "definitely assigned after false expression", then the state of *v* after *expr* is "definitely assigned after true expression".</span></span>
    * <span data-ttu-id="6d25d-415">如果狀態*v*之後 \* expr_operand *"，則為 true 的運算式之後明確指派 」，則狀態*v*之後*expr\* "肯定之後被指派為 false運算式"。</span><span class="sxs-lookup"><span data-stu-id="6d25d-415">If the state of *v* after \*expr_operand \*is "definitely assigned after true expression", then the state of *v* after *expr* is "definitely assigned after false expression".</span></span>

#### <a name="-null-coalescing-expressions"></a><span data-ttu-id="6d25d-416">??</span><span class="sxs-lookup"><span data-stu-id="6d25d-416">??</span></span> <span data-ttu-id="6d25d-417">（null 聯合） 的運算式</span><span class="sxs-lookup"><span data-stu-id="6d25d-417">(null coalescing) expressions</span></span>

<span data-ttu-id="6d25d-418">運算式*expr*表單的`expr_first ?? expr_second`:</span><span class="sxs-lookup"><span data-stu-id="6d25d-418">For an expression *expr* of the form `expr_first ?? expr_second`:</span></span>

*  <span data-ttu-id="6d25d-419">明確指派狀態*v*之前*expr_first*明確指派狀態的相同*v*之前*expr*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-419">The definite assignment state of *v* before *expr_first* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="6d25d-420">明確指派狀態*v*之前*expr_second*明確指派狀態的相同*v*之後*expr_first*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-420">The definite assignment state of *v* before *expr_second* is the same as the definite assignment state of *v* after *expr_first*.</span></span>
*  <span data-ttu-id="6d25d-421">明確指派陳述式*v*之後*expr*取決於：</span><span class="sxs-lookup"><span data-stu-id="6d25d-421">The definite assignment statement of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="6d25d-422">如果*expr_first*是常數運算式 ([常數運算式](expressions.md#constant-expressions)) 值為 null，則狀態*v*之後*expr*相同隨著*v*之後*expr_second*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-422">If *expr_first* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) with value null, then the the state of *v* after *expr* is the same as the state of *v* after *expr_second*.</span></span>
*  <span data-ttu-id="6d25d-423">否則，狀態*v*之後*expr*明確指派狀態的相同*v*之後*expr_first*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-423">Otherwise, the state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_first*.</span></span>

#### <a name="-conditional-expressions"></a><span data-ttu-id="6d25d-424">？: （條件） 運算式</span><span class="sxs-lookup"><span data-stu-id="6d25d-424">?: (conditional) expressions</span></span>

<span data-ttu-id="6d25d-425">運算式*expr*表單的`expr_cond ? expr_true : expr_false`:</span><span class="sxs-lookup"><span data-stu-id="6d25d-425">For an expression *expr* of the form `expr_cond ? expr_true : expr_false`:</span></span>

*  <span data-ttu-id="6d25d-426">明確指派狀態*v*之前*expr_cond*等同於狀態*v*之前*expr*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-426">The definite assignment state of *v* before *expr_cond* is the same as the state of *v* before *expr*.</span></span>
*  <span data-ttu-id="6d25d-427">明確指派狀態*v*之前*expr_true*已明確指派，如果且只有下列其中一種保留：</span><span class="sxs-lookup"><span data-stu-id="6d25d-427">The definite assignment state of *v* before *expr_true* is definitely assigned if and only if one of the following holds:</span></span>
    * <span data-ttu-id="6d25d-428">*expr_cond*是常數運算式的值 `false`</span><span class="sxs-lookup"><span data-stu-id="6d25d-428">*expr_cond* is a constant expression with the value `false`</span></span>
    * <span data-ttu-id="6d25d-429">狀態*v*之後*expr_cond*是明確指派或 「 絕對，則為 true 的運算式之後指派 」。</span><span class="sxs-lookup"><span data-stu-id="6d25d-429">the state of *v* after *expr_cond* is definitely assigned or "definitely assigned after true expression".</span></span>
*  <span data-ttu-id="6d25d-430">明確指派狀態*v*之前*expr_false*已明確指派，如果且只有下列其中一種保留：</span><span class="sxs-lookup"><span data-stu-id="6d25d-430">The definite assignment state of *v* before *expr_false* is definitely assigned if and only if one of the following holds:</span></span>
    * <span data-ttu-id="6d25d-431">*expr_cond*是常數運算式的值 `true`</span><span class="sxs-lookup"><span data-stu-id="6d25d-431">*expr_cond* is a constant expression with the value `true`</span></span>
*  <span data-ttu-id="6d25d-432">狀態*v*之後*expr_cond*是明確指派或 「 絕對，則為 false 的運算式之後指派 」。</span><span class="sxs-lookup"><span data-stu-id="6d25d-432">the state of *v* after *expr_cond* is definitely assigned or "definitely assigned after false expression".</span></span>
*  <span data-ttu-id="6d25d-433">明確指派狀態*v*之後*expr*取決於：</span><span class="sxs-lookup"><span data-stu-id="6d25d-433">The definite assignment state of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="6d25d-434">如果*expr_cond*是常數運算式 ([常數運算式](expressions.md#constant-expressions)) 值`true`然後狀態*v*之後*expr*狀態相同*v*之後*expr_true*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-434">If *expr_cond* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) with value `true` then the state of *v* after *expr* is the same as the state of *v* after *expr_true*.</span></span>
    * <span data-ttu-id="6d25d-435">否則，如果*expr_cond*是常數運算式 ([常數運算式](expressions.md#constant-expressions)) 值`false`然後狀態*v*之後*expr*的狀態相同*v*之後*expr_false*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-435">Otherwise, if *expr_cond* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) with value `false` then the state of *v* after *expr* is the same as the state of *v* after *expr_false*.</span></span>
    * <span data-ttu-id="6d25d-436">否則，如果狀態*v*之後*expr_true*明確指派和狀態*v*之後*expr_false*絕對指派，則狀態*v*之後*expr*明確指派。</span><span class="sxs-lookup"><span data-stu-id="6d25d-436">Otherwise, if the state of *v* after *expr_true* is definitely assigned and the state of *v* after *expr_false* is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="6d25d-437">否則，狀態*v*之後*expr*未明確指派。</span><span class="sxs-lookup"><span data-stu-id="6d25d-437">Otherwise, the state of *v* after *expr* is not definitely assigned.</span></span>

#### <a name="anonymous-functions"></a><span data-ttu-id="6d25d-438">匿名函式</span><span class="sxs-lookup"><span data-stu-id="6d25d-438">Anonymous functions</span></span>

<span data-ttu-id="6d25d-439">針對*lambda_expression*或是*anonymous_method_expression* *expr*內文 (任一*區塊*或*運算式*)*主體*:</span><span class="sxs-lookup"><span data-stu-id="6d25d-439">For a *lambda_expression* or *anonymous_method_expression* *expr* with a body (either *block* or *expression*) *body*:</span></span>

*  <span data-ttu-id="6d25d-440">外部變數的明確指派狀態*v*之前*主體*等同於狀態*v*之前*expr*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-440">The definite assignment state of an outer variable *v* before *body* is the same as the state of *v* before *expr*.</span></span> <span data-ttu-id="6d25d-441">也就是明確的指派狀態的外部變數被繼承自匿名函式的內容。</span><span class="sxs-lookup"><span data-stu-id="6d25d-441">That is, definite assignment state of outer variables is inherited from the context of the anonymous function.</span></span>
*  <span data-ttu-id="6d25d-442">外部變數的明確指派狀態*v*之後*expr*等同於狀態*v*之前*expr*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-442">The definite assignment state of an outer variable *v* after *expr* is the same as the state of *v* before *expr*.</span></span>

<span data-ttu-id="6d25d-443">此範例</span><span class="sxs-lookup"><span data-stu-id="6d25d-443">The example</span></span>
```csharp
delegate bool Filter(int i);

void F() {
    int max;

    // Error, max is not definitely assigned
    Filter f = (int n) => n < max;

    max = 5;
    DoWork(f);
}
```
<span data-ttu-id="6d25d-444">會產生編譯時期錯誤，因為`max`未明確指派的匿名函式宣告的位置。</span><span class="sxs-lookup"><span data-stu-id="6d25d-444">generates a compile-time error since `max` is not definitely assigned where the anonymous function is declared.</span></span> <span data-ttu-id="6d25d-445">此範例</span><span class="sxs-lookup"><span data-stu-id="6d25d-445">The example</span></span>
```csharp
delegate void D();

void F() {
    int n;
    D d = () => { n = 1; };

    d();

    // Error, n is not definitely assigned
    Console.WriteLine(n);
}
```
<span data-ttu-id="6d25d-446">也會產生編譯時期錯誤，因為指派給`n`匿名函式中明確指派狀態就不會影響`n`匿名函式之外。</span><span class="sxs-lookup"><span data-stu-id="6d25d-446">also generates a compile-time error since the assignment to `n` in the anonymous function has no affect on the definite assignment state of `n` outside the anonymous function.</span></span>

## <a name="variable-references"></a><span data-ttu-id="6d25d-447">變數參考</span><span class="sxs-lookup"><span data-stu-id="6d25d-447">Variable references</span></span>

<span data-ttu-id="6d25d-448">A *variable_reference*是*運算式*，分類為變數。</span><span class="sxs-lookup"><span data-stu-id="6d25d-448">A *variable_reference* is an *expression* that is classified as a variable.</span></span> <span data-ttu-id="6d25d-449">A *variable_reference*代表要擷取目前的值和儲存新的值，可以存取的儲存位置。</span><span class="sxs-lookup"><span data-stu-id="6d25d-449">A *variable_reference* denotes a storage location that can be accessed both to fetch the current value and to store a new value.</span></span>

```antlr
variable_reference
    : expression
    ;
```

<span data-ttu-id="6d25d-450">在 C 和C++，則*variable_reference*稱為*左值*。</span><span class="sxs-lookup"><span data-stu-id="6d25d-450">In C and C++, a *variable_reference* is known as an *lvalue*.</span></span>

## <a name="atomicity-of-variable-references"></a><span data-ttu-id="6d25d-451">不可部分完成性的變數參考</span><span class="sxs-lookup"><span data-stu-id="6d25d-451">Atomicity of variable references</span></span>

<span data-ttu-id="6d25d-452">讀取和寫入的下列資料類型是不可部分完成： `bool`， `char`， `byte`， `sbyte`， `short`， `ushort`， `uint`， `int`， `float`，和參考型別。</span><span class="sxs-lookup"><span data-stu-id="6d25d-452">Reads and writes of the following data types are atomic: `bool`, `char`, `byte`, `sbyte`, `short`, `ushort`, `uint`, `int`, `float`, and reference types.</span></span> <span data-ttu-id="6d25d-453">此外，讀取和寫入與上述清單中的基礎類型的列舉型別也是不可部分完成。</span><span class="sxs-lookup"><span data-stu-id="6d25d-453">In addition, reads and writes of enum types with an underlying type in the previous list are also atomic.</span></span> <span data-ttu-id="6d25d-454">讀取和寫入的其他類型，包括`long`， `ulong`， `double`，和`decimal`，以及使用者定義型別，不保證是不可部分完成。</span><span class="sxs-lookup"><span data-stu-id="6d25d-454">Reads and writes of other types, including `long`, `ulong`, `double`, and `decimal`, as well as user-defined types, are not guaranteed to be atomic.</span></span> <span data-ttu-id="6d25d-455">除了針對該用途而設計的程式庫函式，則無法保證的不可部分完成讀取-修改-寫入，例如在遞增或遞減的情況下。</span><span class="sxs-lookup"><span data-stu-id="6d25d-455">Aside from the library functions designed for that purpose, there is no guarantee of atomic read-modify-write, such as in the case of increment or decrement.</span></span>

