---
ms.openlocfilehash: a01cf9387b8dc47de036bf0bd1496c19a441d81c
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876805"
---
# <a name="variables"></a><span data-ttu-id="85516-101">變數</span><span class="sxs-lookup"><span data-stu-id="85516-101">Variables</span></span>

<span data-ttu-id="85516-102">變數代表儲存位置。</span><span class="sxs-lookup"><span data-stu-id="85516-102">Variables represent storage locations.</span></span> <span data-ttu-id="85516-103">每個變數都有一個類型，可決定可以在變數中儲存的值。</span><span class="sxs-lookup"><span data-stu-id="85516-103">Every variable has a type that determines what values can be stored in the variable.</span></span> <span data-ttu-id="85516-104">C#是型別安全的語言，而且C#編譯器保證儲存在變數中的值一定是適當的型別。</span><span class="sxs-lookup"><span data-stu-id="85516-104">C# is a type-safe language, and the C# compiler guarantees that values stored in variables are always of the appropriate type.</span></span> <span data-ttu-id="85516-105">變數的值可以透過指派或透過使用`++`和`--`運算子來變更。</span><span class="sxs-lookup"><span data-stu-id="85516-105">The value of a variable can be changed through assignment or through use of the `++` and `--` operators.</span></span>

<span data-ttu-id="85516-106">必須先***明確指派***變數（[明確指派](variables.md#definite-assignment)），才能取得其值。</span><span class="sxs-lookup"><span data-stu-id="85516-106">A variable must be ***definitely assigned*** ([Definite assignment](variables.md#definite-assignment)) before its value can be obtained.</span></span>

<span data-ttu-id="85516-107">如下列各節所述，一***開始指派***或一***開始未***指派變數。</span><span class="sxs-lookup"><span data-stu-id="85516-107">As described in the following sections, variables are either ***initially assigned*** or ***initially unassigned***.</span></span> <span data-ttu-id="85516-108">一開始指派的變數具有妥善定義的初始值，而且一律會被視為明確指派的值。</span><span class="sxs-lookup"><span data-stu-id="85516-108">An initially assigned variable has a well-defined initial value and is always considered definitely assigned.</span></span> <span data-ttu-id="85516-109">一開始未指派的變數沒有初始值。</span><span class="sxs-lookup"><span data-stu-id="85516-109">An initially unassigned variable has no initial value.</span></span> <span data-ttu-id="85516-110">對於一開始未指派的變數，若要在特定位置被視為明確指派，對變數的指派必須發生在導致該位置的每個可能的執行路徑中。</span><span class="sxs-lookup"><span data-stu-id="85516-110">For an initially unassigned variable to be considered definitely assigned at a certain location, an assignment to the variable must occur in every possible execution path leading to that location.</span></span>

## <a name="variable-categories"></a><span data-ttu-id="85516-111">變數類別目錄</span><span class="sxs-lookup"><span data-stu-id="85516-111">Variable categories</span></span>

<span data-ttu-id="85516-112">C#定義變數的七個類別：靜態變數、執行個體變數、陣列元素、值參數、參考參數、輸出參數和本機變數。</span><span class="sxs-lookup"><span data-stu-id="85516-112">C# defines seven categories of variables: static variables, instance variables, array elements, value parameters, reference parameters, output parameters, and local variables.</span></span> <span data-ttu-id="85516-113">下列各節會說明每個類別。</span><span class="sxs-lookup"><span data-stu-id="85516-113">The sections that follow describe each of these categories.</span></span>

<span data-ttu-id="85516-114">在範例中</span><span class="sxs-lookup"><span data-stu-id="85516-114">In the example</span></span>
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
<span data-ttu-id="85516-115">`x`是一個靜態變數， `y`它是一個執行個體變數`v[0]` ，是一個陣列元素`a` ，是一個`c`值參數`b` ，是一個參考參數，是一個輸出參數， `i`而是一個本機變數.</span><span class="sxs-lookup"><span data-stu-id="85516-115">`x` is a static variable, `y` is an instance variable, `v[0]` is an array element, `a` is a value parameter, `b` is a reference parameter, `c` is an output parameter, and `i` is a local variable.</span></span>

### <a name="static-variables"></a><span data-ttu-id="85516-116">靜態變數</span><span class="sxs-lookup"><span data-stu-id="85516-116">Static variables</span></span>

<span data-ttu-id="85516-117">以`static`修飾詞宣告的欄位稱為***靜態變數***。</span><span class="sxs-lookup"><span data-stu-id="85516-117">A field declared with the `static` modifier is called a ***static variable***.</span></span> <span data-ttu-id="85516-118">靜態變數在執行其包含類型的靜態函式（[靜態](classes.md#static-constructors)的函式）之前就會存在，而且當相關聯的應用程式域停止存在時，就會停止存在。</span><span class="sxs-lookup"><span data-stu-id="85516-118">A static variable comes into existence before execution of the static constructor ([Static constructors](classes.md#static-constructors)) for its containing type, and ceases to exist when the associated application domain ceases to exist.</span></span>

<span data-ttu-id="85516-119">靜態變數的初始值是變數類型的預設值（[預設值](variables.md#default-values)）。</span><span class="sxs-lookup"><span data-stu-id="85516-119">The initial value of a static variable is the default value ([Default values](variables.md#default-values)) of the variable's type.</span></span>

<span data-ttu-id="85516-120">基於明確指派檢查的目的，會將靜態變數視為一開始指派。</span><span class="sxs-lookup"><span data-stu-id="85516-120">For purposes of definite assignment checking, a static variable is considered initially assigned.</span></span>

### <a name="instance-variables"></a><span data-ttu-id="85516-121">執行個體變數</span><span class="sxs-lookup"><span data-stu-id="85516-121">Instance variables</span></span>

<span data-ttu-id="85516-122">未使用`static`修飾詞宣告的欄位稱為「***執行個體變數***」。</span><span class="sxs-lookup"><span data-stu-id="85516-122">A field declared without the `static` modifier is called an ***instance variable***.</span></span>

#### <a name="instance-variables-in-classes"></a><span data-ttu-id="85516-123">類別中的執行個體變數</span><span class="sxs-lookup"><span data-stu-id="85516-123">Instance variables in classes</span></span>

<span data-ttu-id="85516-124">當建立該類別的新實例時，類別的執行個體變數就會存在，而當該實例沒有任何參考和實例的析構函式（如果有的話）已執行時，就會停止存在。</span><span class="sxs-lookup"><span data-stu-id="85516-124">An instance variable of a class comes into existence when a new instance of that class is created, and ceases to exist when there are no references to that instance and the instance's destructor (if any) has executed.</span></span>

<span data-ttu-id="85516-125">類別之執行個體變數的初始值是變數類型的預設值（[預設](variables.md#default-values)值）。</span><span class="sxs-lookup"><span data-stu-id="85516-125">The initial value of an instance variable of a class is the default value ([Default values](variables.md#default-values)) of the variable's type.</span></span>

<span data-ttu-id="85516-126">基於明確指派檢查的目的，會將類別的執行個體變數視為一開始指派。</span><span class="sxs-lookup"><span data-stu-id="85516-126">For the purpose of definite assignment checking, an instance variable of a class is considered initially assigned.</span></span>

#### <a name="instance-variables-in-structs"></a><span data-ttu-id="85516-127">結構中的執行個體變數</span><span class="sxs-lookup"><span data-stu-id="85516-127">Instance variables in structs</span></span>

<span data-ttu-id="85516-128">結構的執行個體變數與它所屬的結構變數具有完全相同的存留期。</span><span class="sxs-lookup"><span data-stu-id="85516-128">An instance variable of a struct has exactly the same lifetime as the struct variable to which it belongs.</span></span> <span data-ttu-id="85516-129">換句話說，當結構型別的變數出現或不存在時，就會執行結構的執行個體變數。</span><span class="sxs-lookup"><span data-stu-id="85516-129">In other words, when a variable of a struct type comes into existence or ceases to exist, so too do the instance variables of the struct.</span></span>

<span data-ttu-id="85516-130">結構之執行個體變數的初始指派狀態與包含 struct 變數相同。</span><span class="sxs-lookup"><span data-stu-id="85516-130">The initial assignment state of an instance variable of a struct is the same as that of the containing struct variable.</span></span> <span data-ttu-id="85516-131">換句話說，當結構變數被視為一開始被指派時，也就是它的執行個體變數，而當結構變數被視為一開始未指派時，它的執行個體變數就同樣不會被指派。</span><span class="sxs-lookup"><span data-stu-id="85516-131">In other words, when a struct variable is considered initially assigned, so too are its instance variables, and when a struct variable is considered initially unassigned, its instance variables are likewise unassigned.</span></span>

### <a name="array-elements"></a><span data-ttu-id="85516-132">陣列元素</span><span class="sxs-lookup"><span data-stu-id="85516-132">Array elements</span></span>

<span data-ttu-id="85516-133">當建立陣列實例時，陣列的元素會存在，而且當沒有該陣列實例的參考時，就會停止存在。</span><span class="sxs-lookup"><span data-stu-id="85516-133">The elements of an array come into existence when an array instance is created, and cease to exist when there are no references to that array instance.</span></span>

<span data-ttu-id="85516-134">陣列中每個元素的初始值都是陣列元素類型的預設值（[預設](variables.md#default-values)值）。</span><span class="sxs-lookup"><span data-stu-id="85516-134">The initial value of each of the elements of an array is the default value ([Default values](variables.md#default-values)) of the type of the array elements.</span></span>

<span data-ttu-id="85516-135">基於明確指派檢查的目的，會將陣列元素視為一開始指派。</span><span class="sxs-lookup"><span data-stu-id="85516-135">For the purpose of definite assignment checking, an array element is considered initially assigned.</span></span>

### <a name="value-parameters"></a><span data-ttu-id="85516-136">值參數</span><span class="sxs-lookup"><span data-stu-id="85516-136">Value parameters</span></span>

<span data-ttu-id="85516-137">未使用`ref`或`out`修飾詞宣告的參數是***值參數***。</span><span class="sxs-lookup"><span data-stu-id="85516-137">A parameter declared without a `ref` or `out` modifier is a ***value parameter***.</span></span>

<span data-ttu-id="85516-138">值參數在叫用函式成員（方法、實例參數化、存取子或運算子）或參數所屬的匿名函式時，會存在，且會使用調用中提供的引數值進行初始化。</span><span class="sxs-lookup"><span data-stu-id="85516-138">A value parameter comes into existence upon invocation of the function member (method, instance constructor, accessor, or operator) or anonymous function to which the parameter belongs, and is initialized with the value of the argument given in the invocation.</span></span> <span data-ttu-id="85516-139">值參數在函式成員或匿名函式傳回時，通常會停止存在。</span><span class="sxs-lookup"><span data-stu-id="85516-139">A value parameter normally ceases to exist upon return of the function member or anonymous function.</span></span> <span data-ttu-id="85516-140">不過，如果值參數是由匿名函式（匿名函式[運算式](expressions.md#anonymous-function-expressions)）所捕捉，則其存留時間至少會延伸到從該匿名函式建立的委派或運算式樹狀結構符合垃圾收集的資格。</span><span class="sxs-lookup"><span data-stu-id="85516-140">However, if the value parameter is captured by an anonymous function ([Anonymous function expressions](expressions.md#anonymous-function-expressions)), its life time extends at least until the delegate or expression tree created from that anonymous function is eligible for garbage collection.</span></span>

<span data-ttu-id="85516-141">基於明確指派檢查的目的，會將值參數視為一開始指派。</span><span class="sxs-lookup"><span data-stu-id="85516-141">For the purpose of definite assignment checking, a value parameter is considered initially assigned.</span></span>

### <a name="reference-parameters"></a><span data-ttu-id="85516-142">傳址參數</span><span class="sxs-lookup"><span data-stu-id="85516-142">Reference parameters</span></span>

<span data-ttu-id="85516-143">使用`ref`修飾詞宣告的參數是***參考參數***。</span><span class="sxs-lookup"><span data-stu-id="85516-143">A parameter declared with a `ref` modifier is a ***reference parameter***.</span></span>

<span data-ttu-id="85516-144">參考參數不會建立新的儲存位置。</span><span class="sxs-lookup"><span data-stu-id="85516-144">A reference parameter does not create a new storage location.</span></span> <span data-ttu-id="85516-145">相反地，參考參數代表的儲存位置與在函式成員或匿名函式呼叫中指定為引數的變數相同。</span><span class="sxs-lookup"><span data-stu-id="85516-145">Instead, a reference parameter represents the same storage location as the variable given as the argument in the function member or anonymous function invocation.</span></span> <span data-ttu-id="85516-146">因此，參考參數的值一律與基礎變數相同。</span><span class="sxs-lookup"><span data-stu-id="85516-146">Thus, the value of a reference parameter is always the same as the underlying variable.</span></span>

<span data-ttu-id="85516-147">下列明確指派規則適用于參考參數。</span><span class="sxs-lookup"><span data-stu-id="85516-147">The following definite assignment rules apply to reference parameters.</span></span> <span data-ttu-id="85516-148">請注意[輸出參數](variables.md#output-parameters)中所述輸出參數的不同規則。</span><span class="sxs-lookup"><span data-stu-id="85516-148">Note the different rules for output parameters described in [Output parameters](variables.md#output-parameters).</span></span>

*  <span data-ttu-id="85516-149">變數必須是明確指派的（[明確指派](variables.md#definite-assignment)），才能在函式成員或委派調用中傳遞做為參考參數。</span><span class="sxs-lookup"><span data-stu-id="85516-149">A variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before it can be passed as a reference parameter in a function member or delegate invocation.</span></span>
*  <span data-ttu-id="85516-150">在函式成員或匿名函式內，會將參考參數視為一開始指派。</span><span class="sxs-lookup"><span data-stu-id="85516-150">Within a function member or anonymous function, a reference parameter is considered initially assigned.</span></span>

<span data-ttu-id="85516-151">在結構類型的實例方法或實例存取子中，關鍵字`this`的行為與結構類型的參考參數完全相同（[此存取權](expressions.md#this-access)）。</span><span class="sxs-lookup"><span data-stu-id="85516-151">Within an instance method or instance accessor of a struct type, the `this` keyword behaves exactly as a reference parameter of the struct type ([This access](expressions.md#this-access)).</span></span>

### <a name="output-parameters"></a><span data-ttu-id="85516-152">輸出參數</span><span class="sxs-lookup"><span data-stu-id="85516-152">Output parameters</span></span>

<span data-ttu-id="85516-153">使用`out`修飾詞宣告的參數是***output 參數***。</span><span class="sxs-lookup"><span data-stu-id="85516-153">A parameter declared with an `out` modifier is an ***output parameter***.</span></span>

<span data-ttu-id="85516-154">輸出參數不會建立新的儲存位置。</span><span class="sxs-lookup"><span data-stu-id="85516-154">An output parameter does not create a new storage location.</span></span> <span data-ttu-id="85516-155">相反地，output 參數代表的儲存位置與指定為函式成員或委派調用中引數的變數相同。</span><span class="sxs-lookup"><span data-stu-id="85516-155">Instead, an output parameter represents the same storage location as the variable given as the argument in the function member or delegate invocation.</span></span> <span data-ttu-id="85516-156">因此，輸出參數的值一律與基礎變數相同。</span><span class="sxs-lookup"><span data-stu-id="85516-156">Thus, the value of an output parameter is always the same as the underlying variable.</span></span>

<span data-ttu-id="85516-157">下列明確指派規則適用于輸出參數。</span><span class="sxs-lookup"><span data-stu-id="85516-157">The following definite assignment rules apply to output parameters.</span></span> <span data-ttu-id="85516-158">請注意[參考參數](variables.md#reference-parameters)中所述參考參數的不同規則。</span><span class="sxs-lookup"><span data-stu-id="85516-158">Note the different rules for reference parameters described in [Reference parameters](variables.md#reference-parameters).</span></span>

*  <span data-ttu-id="85516-159">變數不需要明確指派，就可以在函式成員或委派調用中傳遞做為輸出參數。</span><span class="sxs-lookup"><span data-stu-id="85516-159">A variable need not be definitely assigned before it can be passed as an output parameter in a function member or delegate invocation.</span></span>
*  <span data-ttu-id="85516-160">在函式成員或委派調用的正常完成之後，每個當做輸出參數傳遞的變數都會視為該執行路徑中的指派。</span><span class="sxs-lookup"><span data-stu-id="85516-160">Following the normal completion of a function member or delegate invocation, each variable that was passed as an output parameter is considered assigned in that execution path.</span></span>
*  <span data-ttu-id="85516-161">在函數成員或匿名函式內，會將輸出參數視為一開始未指派。</span><span class="sxs-lookup"><span data-stu-id="85516-161">Within a function member or anonymous function, an output parameter is considered initially unassigned.</span></span>
*  <span data-ttu-id="85516-162">函式成員或匿名函式的每個輸出參數都必須明確指派（[明確指派](variables.md#definite-assignment)），函式成員或匿名函式才會正常傳回。</span><span class="sxs-lookup"><span data-stu-id="85516-162">Every output parameter of a function member or anonymous function must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before the function member or anonymous function returns normally.</span></span>

<span data-ttu-id="85516-163">在結構類型的實例函式中，關鍵字`this`的行為與結構類型的輸出參數完全相同（[此存取權](expressions.md#this-access)）。</span><span class="sxs-lookup"><span data-stu-id="85516-163">Within an instance constructor of a struct type, the `this` keyword behaves exactly as an output parameter of the struct type ([This access](expressions.md#this-access)).</span></span>

### <a name="local-variables"></a><span data-ttu-id="85516-164">區域變數</span><span class="sxs-lookup"><span data-stu-id="85516-164">Local variables</span></span>

<span data-ttu-id="85516-165">***本機變數***是由*local_variable_declaration*所宣告，這可能會發生在*區塊*、 *for_statement*、 *switch_statement*或*using_statement*中。或是由*try_statement*的*foreach_statement*或*specific_catch_clause* 。</span><span class="sxs-lookup"><span data-stu-id="85516-165">A ***local variable*** is declared by a *local_variable_declaration*, which may occur in a *block*, a *for_statement*, a *switch_statement* or a *using_statement*; or by a *foreach_statement* or a *specific_catch_clause* for a *try_statement*.</span></span>

<span data-ttu-id="85516-166">本機變數的存留期是程式執行的部分，在這段期間，一定會保留儲存區。</span><span class="sxs-lookup"><span data-stu-id="85516-166">The lifetime of a local variable is the portion of program execution during which storage is guaranteed to be reserved for it.</span></span> <span data-ttu-id="85516-167">此存留期至少會從 entry 延伸到*區塊*、 *for_statement*、 *switch_statement*、 *using_statement*、 *foreach_statement*或與其關聯的*specific_catch_clause* ，直到該*區塊*、 *for_statement*、 *switch_statement*、 *using_statement*、 *foreach_statement*或*specific_catch_clause*的執行會以任何方式結束。</span><span class="sxs-lookup"><span data-stu-id="85516-167">This lifetime extends at least from entry into the *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause* with which it is associated, until execution of that *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause* ends in any way.</span></span> <span data-ttu-id="85516-168">（輸入封閉的*區塊*或呼叫方法會暫停，但不會結束、執行目前的*區塊*、 *for_statement*、 *switch_statement*、 *using_statement*、 *foreach_statement*或*specific_catch_clause*）。如果本機變數是由匿名函式（已[捕捉的外部變數](expressions.md#captured-outer-variables)）所捕捉，其存留期至少會擴充到從匿名函式建立的委派或運算式樹狀架構，以及任何其他參考已捕捉的變數符合垃圾收集的資格。</span><span class="sxs-lookup"><span data-stu-id="85516-168">(Entering an enclosed *block* or calling a method suspends, but does not end, execution of the current *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause*.) If the local variable is captured by an anonymous function ([Captured outer variables](expressions.md#captured-outer-variables)), its lifetime extends at least until the delegate or expression tree created from the anonymous function, along with any other objects that come to reference the captured variable, are eligible for garbage collection.</span></span>

<span data-ttu-id="85516-169">如果父*區塊*、 *for_statement*、 *switch_statement*、 *using_statement*、 *foreach_statement*或*specific_catch_clause*以遞迴方式輸入，則會建立每個本機變數的新實例。時間和其*local_variable_initializer*（如果有的話）會每次評估。</span><span class="sxs-lookup"><span data-stu-id="85516-169">If the parent *block*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, or *specific_catch_clause* is entered recursively, a new instance of the local variable is created each time, and its *local_variable_initializer*, if any, is evaluated each time.</span></span>

<span data-ttu-id="85516-170">*Local_variable_declaration*引進的區域變數不會自動初始化，因此沒有預設值。</span><span class="sxs-lookup"><span data-stu-id="85516-170">A local variable introduced by a *local_variable_declaration* is not automatically initialized and thus has no default value.</span></span> <span data-ttu-id="85516-171">基於明確指派檢查的目的， *local_variable_declaration*引進的本機變數會被視為一開始未指派。</span><span class="sxs-lookup"><span data-stu-id="85516-171">For the purpose of definite assignment checking, a local variable introduced by a *local_variable_declaration* is considered initially unassigned.</span></span> <span data-ttu-id="85516-172">*Local_variable_declaration*可能包含*local_variable_initializer*，在此情況下，只會將變數視為明確指派給初始化運算式（宣告[語句](variables.md#declaration-statements)）。</span><span class="sxs-lookup"><span data-stu-id="85516-172">A *local_variable_declaration* may include a *local_variable_initializer*, in which case the variable is considered definitely assigned only after the initializing expression ([Declaration statements](variables.md#declaration-statements)).</span></span>

<span data-ttu-id="85516-173">在*local_variable_declaration*所引進的本機變數範圍內，在其*local_variable_declarator*之前的文字位置中參考該區域變數是編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="85516-173">Within the scope of a local variable introduced by a *local_variable_declaration*, it is a compile-time error to refer to that local variable in a textual position that precedes its *local_variable_declarator*.</span></span> <span data-ttu-id="85516-174">如果區域變數宣告是隱含的（[區域變數](statements.md#local-variable-declarations)宣告），則在其*local_variable_declarator*中參考該變數也會是錯誤。</span><span class="sxs-lookup"><span data-stu-id="85516-174">If the local variable declaration is implicit ([Local variable declarations](statements.md#local-variable-declarations)), it is also an error to refer to the variable within its *local_variable_declarator*.</span></span>

<span data-ttu-id="85516-175">*Foreach_statement*或*specific_catch_clause*所引進的本機變數會被視為在其整個範圍中明確指派。</span><span class="sxs-lookup"><span data-stu-id="85516-175">A local variable introduced by a *foreach_statement* or a *specific_catch_clause* is considered definitely assigned in its entire scope.</span></span>

<span data-ttu-id="85516-176">本機變數的實際存留期與執行相依。</span><span class="sxs-lookup"><span data-stu-id="85516-176">The actual lifetime of a local variable is implementation-dependent.</span></span> <span data-ttu-id="85516-177">例如，編譯器可能會以靜態方式判斷區塊中的區域變數僅用於該區塊的一小部分。</span><span class="sxs-lookup"><span data-stu-id="85516-177">For example, a compiler might statically determine that a local variable in a block is only used for a small portion of that block.</span></span> <span data-ttu-id="85516-178">使用這項分析，編譯器可能會產生程式碼，使變數的存放區具有比其包含區塊更短的存留期。</span><span class="sxs-lookup"><span data-stu-id="85516-178">Using this analysis, the compiler could generate code that results in the variable's storage having a shorter lifetime than its containing block.</span></span>

<span data-ttu-id="85516-179">本機參考變數所參考的儲存體會在該區域參考變數（[自動記憶體管理](basic-concepts.md#automatic-memory-management)）的存留期以外獨立回收。</span><span class="sxs-lookup"><span data-stu-id="85516-179">The storage referred to by a local reference variable is reclaimed independently of the lifetime of that local reference variable ([Automatic memory management](basic-concepts.md#automatic-memory-management)).</span></span>

## <a name="default-values"></a><span data-ttu-id="85516-180">預設值</span><span class="sxs-lookup"><span data-stu-id="85516-180">Default values</span></span>

<span data-ttu-id="85516-181">下列類別的變數會自動初始化為其預設值：</span><span class="sxs-lookup"><span data-stu-id="85516-181">The following categories of variables are automatically initialized to their default values:</span></span>

*  <span data-ttu-id="85516-182">靜態變數。</span><span class="sxs-lookup"><span data-stu-id="85516-182">Static variables.</span></span>
*  <span data-ttu-id="85516-183">類別實例的執行個體變數。</span><span class="sxs-lookup"><span data-stu-id="85516-183">Instance variables of class instances.</span></span>
*  <span data-ttu-id="85516-184">陣列元素。</span><span class="sxs-lookup"><span data-stu-id="85516-184">Array elements.</span></span>

<span data-ttu-id="85516-185">變數的預設值取決於變數的類型，並依照下列方式決定：</span><span class="sxs-lookup"><span data-stu-id="85516-185">The default value of a variable depends on the type of the variable and is determined as follows:</span></span>

*  <span data-ttu-id="85516-186">對於*value_type*的變數，預設值與*value_type*的預設函式（[預設](types.md#default-constructors)的函式）所計算的值相同。</span><span class="sxs-lookup"><span data-stu-id="85516-186">For a variable of a *value_type*, the default value is the same as the value computed by the *value_type*'s default constructor ([Default constructors](types.md#default-constructors)).</span></span>
*  <span data-ttu-id="85516-187">若為*reference_type*的變數，預設值為`null`。</span><span class="sxs-lookup"><span data-stu-id="85516-187">For a variable of a *reference_type*, the default value is `null`.</span></span>

<span data-ttu-id="85516-188">預設值的初始化通常是藉由讓記憶體管理員或垃圾收集行程將記憶體初始化為全位-零，再配置給使用。</span><span class="sxs-lookup"><span data-stu-id="85516-188">Initialization to default values is typically done by having the memory manager or garbage collector initialize memory to all-bits-zero before it is allocated for use.</span></span> <span data-ttu-id="85516-189">基於這個理由，使用全位-零來代表 null 參考是很方便的。</span><span class="sxs-lookup"><span data-stu-id="85516-189">For this reason, it is convenient to use all-bits-zero to represent the null reference.</span></span>

## <a name="definite-assignment"></a><span data-ttu-id="85516-190">明確指派</span><span class="sxs-lookup"><span data-stu-id="85516-190">Definite assignment</span></span>

<span data-ttu-id="85516-191">在函式成員可執行程式碼的指定位置上，如果編譯器可以透過特定的靜態流程分析（[判斷明確指派的精確規則](variables.md#precise-rules-for-determining-definite-assignment)）來證明，變數就會被視為***明確指派***。已自動初始化，或已是至少一個指派的目標。</span><span class="sxs-lookup"><span data-stu-id="85516-191">At a given location in the executable code of a function member, a variable is said to be ***definitely assigned*** if the compiler can prove, by a particular static flow analysis ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)), that the variable has been automatically initialized or has been the target of at least one assignment.</span></span> <span data-ttu-id="85516-192">非正式規定的明確指派規則包括：</span><span class="sxs-lookup"><span data-stu-id="85516-192">Informally stated, the rules of definite assignment are:</span></span>

*  <span data-ttu-id="85516-193">最初指派的變數（[初始指派的變數](variables.md#initially-assigned-variables)）一律會被視為明確指派。</span><span class="sxs-lookup"><span data-stu-id="85516-193">An initially assigned variable ([Initially assigned variables](variables.md#initially-assigned-variables)) is always considered definitely assigned.</span></span>
*  <span data-ttu-id="85516-194">如果所有導致該位置的可能執行路徑至少包含下列其中一項，則最初解除指派的變數（一[開始未指派的變數](variables.md#initially-unassigned-variables)）會被視為在指定的位置上明確指派：</span><span class="sxs-lookup"><span data-stu-id="85516-194">An initially unassigned variable ([Initially unassigned variables](variables.md#initially-unassigned-variables)) is considered definitely assigned at a given location if all possible execution paths leading to that location contain at least one of the following:</span></span>
    * <span data-ttu-id="85516-195">簡單指派（[簡單指派](expressions.md#simple-assignment)），其中變數為左運算元。</span><span class="sxs-lookup"><span data-stu-id="85516-195">A simple assignment ([Simple assignment](expressions.md#simple-assignment)) in which the variable is the left operand.</span></span>
    * <span data-ttu-id="85516-196">傳遞變數做為輸出參數的調用運算式（[調用運算式](expressions.md#invocation-expressions)）或物件建立運算式（[物件建立運算式](expressions.md#object-creation-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="85516-196">An invocation expression ([Invocation expressions](expressions.md#invocation-expressions)) or object creation expression ([Object creation expressions](expressions.md#object-creation-expressions)) that passes the variable as an output parameter.</span></span>
    * <span data-ttu-id="85516-197">對於區域變數，這是包含變數初始化運算式的本機變數宣告（[區域變數](statements.md#local-variable-declarations)宣告）。</span><span class="sxs-lookup"><span data-stu-id="85516-197">For a local variable, a local variable declaration ([Local variable declarations](statements.md#local-variable-declarations)) that includes a variable initializer.</span></span>

<span data-ttu-id="85516-198">在[最初指派的變數](variables.md#initially-assigned-variables)、一[開始未指派的變數](variables.md#initially-unassigned-variables)，以及[判斷明確指派的精確規則](variables.md#precise-rules-for-determining-definite-assignment)中，會描述上述非正式規則的基礎正式規格。</span><span class="sxs-lookup"><span data-stu-id="85516-198">The formal specification underlying the above informal rules is described in [Initially assigned variables](variables.md#initially-assigned-variables), [Initially unassigned variables](variables.md#initially-unassigned-variables), and [Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment).</span></span>

<span data-ttu-id="85516-199">系統會個別追蹤*struct_type*變數之執行個體變數的明確指派狀態。</span><span class="sxs-lookup"><span data-stu-id="85516-199">The definite assignment states of instance variables of a *struct_type* variable are tracked individually as well as collectively.</span></span> <span data-ttu-id="85516-200">除了上述規則，下列規則適用于*struct_type*變數及其執行個體變數：</span><span class="sxs-lookup"><span data-stu-id="85516-200">In addition to the rules above, the following rules apply to *struct_type* variables and their instance variables:</span></span>

*  <span data-ttu-id="85516-201">如果將執行個體變數的包含*struct_type*變數視為明確指派，則會將它視為明確指派。</span><span class="sxs-lookup"><span data-stu-id="85516-201">An instance variable is considered definitely assigned if its containing *struct_type* variable is considered definitely assigned.</span></span>
*  <span data-ttu-id="85516-202">如果將每個執行個體變數視為明確指派，則會將*struct_type*變數視為明確指派。</span><span class="sxs-lookup"><span data-stu-id="85516-202">A *struct_type* variable is considered definitely assigned if each of its instance variables is considered definitely assigned.</span></span>

<span data-ttu-id="85516-203">明確指派是下列內容中的需求：</span><span class="sxs-lookup"><span data-stu-id="85516-203">Definite assignment is a requirement in the following contexts:</span></span>

*  <span data-ttu-id="85516-204">變數必須在取得其值的每個位置明確指派。</span><span class="sxs-lookup"><span data-stu-id="85516-204">A variable must be definitely assigned at each location where its value is obtained.</span></span> <span data-ttu-id="85516-205">這可確保永遠不會發生未定義的值。</span><span class="sxs-lookup"><span data-stu-id="85516-205">This ensures that undefined values never occur.</span></span> <span data-ttu-id="85516-206">運算式中出現的變數會被視為取得變數的值，但不包括</span><span class="sxs-lookup"><span data-stu-id="85516-206">The occurrence of a variable in an expression is considered to obtain the value of the variable, except when</span></span>
    * <span data-ttu-id="85516-207">變數是簡單指派的左運算元，</span><span class="sxs-lookup"><span data-stu-id="85516-207">the variable is the left operand of a simple assignment,</span></span>
    * <span data-ttu-id="85516-208">變數會當做輸出參數傳遞，或</span><span class="sxs-lookup"><span data-stu-id="85516-208">the variable is passed as an output parameter, or</span></span>
    * <span data-ttu-id="85516-209">變數是*struct_type*變數，並會當做成員存取的左運算元。</span><span class="sxs-lookup"><span data-stu-id="85516-209">the variable is a *struct_type* variable and occurs as the left operand of a member access.</span></span>
*  <span data-ttu-id="85516-210">變數必須在做為參考參數傳遞的每個位置上明確指派。</span><span class="sxs-lookup"><span data-stu-id="85516-210">A variable must be definitely assigned at each location where it is passed as a reference parameter.</span></span> <span data-ttu-id="85516-211">這可確保所叫用的函式成員可以考慮最初指派的參考參數。</span><span class="sxs-lookup"><span data-stu-id="85516-211">This ensures that the function member being invoked can consider the reference parameter initially assigned.</span></span>
*  <span data-ttu-id="85516-212">函式成員的所有輸出參數都必須在函式成員傳回的每個位置（透過`return`語句或透過執行到達函式成員主體的結尾）明確指派。</span><span class="sxs-lookup"><span data-stu-id="85516-212">All output parameters of a function member must be definitely assigned at each location where the function member returns (through a `return` statement or through execution reaching the end of the function member body).</span></span> <span data-ttu-id="85516-213">這可確保函式成員不會在輸出參數中傳回未定義的值，因此可讓編譯器考慮使用變數做為輸出參數（相當於對變數的指派）的函數成員調用。</span><span class="sxs-lookup"><span data-stu-id="85516-213">This ensures that function members do not return undefined values in output parameters, thus enabling the compiler to consider a function member invocation that takes a variable as an output parameter equivalent to an assignment to the variable.</span></span>
*  <span data-ttu-id="85516-214">Struct_type 實例的函式的變數必須在該實例的函式傳回的每個位置明確指派。`this`</span><span class="sxs-lookup"><span data-stu-id="85516-214">The `this` variable of a *struct_type* instance constructor must be definitely assigned at each location where that instance constructor returns.</span></span>

### <a name="initially-assigned-variables"></a><span data-ttu-id="85516-215">初始指派的變數</span><span class="sxs-lookup"><span data-stu-id="85516-215">Initially assigned variables</span></span>

<span data-ttu-id="85516-216">下列類別的變數分類為一開始指派：</span><span class="sxs-lookup"><span data-stu-id="85516-216">The following categories of variables are classified as initially assigned:</span></span>

*  <span data-ttu-id="85516-217">靜態變數。</span><span class="sxs-lookup"><span data-stu-id="85516-217">Static variables.</span></span>
*  <span data-ttu-id="85516-218">類別實例的執行個體變數。</span><span class="sxs-lookup"><span data-stu-id="85516-218">Instance variables of class instances.</span></span>
*  <span data-ttu-id="85516-219">最初指派之結構變數的執行個體變數。</span><span class="sxs-lookup"><span data-stu-id="85516-219">Instance variables of initially assigned struct variables.</span></span>
*  <span data-ttu-id="85516-220">陣列元素。</span><span class="sxs-lookup"><span data-stu-id="85516-220">Array elements.</span></span>
*  <span data-ttu-id="85516-221">值參數。</span><span class="sxs-lookup"><span data-stu-id="85516-221">Value parameters.</span></span>
*  <span data-ttu-id="85516-222">參考參數。</span><span class="sxs-lookup"><span data-stu-id="85516-222">Reference parameters.</span></span>
*  <span data-ttu-id="85516-223">在`catch` 子句`foreach`或語句中宣告的變數。</span><span class="sxs-lookup"><span data-stu-id="85516-223">Variables declared in a `catch` clause or a `foreach` statement.</span></span>

### <a name="initially-unassigned-variables"></a><span data-ttu-id="85516-224">一開始未指派的變數</span><span class="sxs-lookup"><span data-stu-id="85516-224">Initially unassigned variables</span></span>

<span data-ttu-id="85516-225">下列類別的變數分類為一開始未指派：</span><span class="sxs-lookup"><span data-stu-id="85516-225">The following categories of variables are classified as initially unassigned:</span></span>

*  <span data-ttu-id="85516-226">一開始未指派之結構變數的執行個體變數。</span><span class="sxs-lookup"><span data-stu-id="85516-226">Instance variables of initially unassigned struct variables.</span></span>
*  <span data-ttu-id="85516-227">輸出參數，包括`this`結構實例函式的變數。</span><span class="sxs-lookup"><span data-stu-id="85516-227">Output parameters, including the `this` variable of struct instance constructors.</span></span>
*  <span data-ttu-id="85516-228">區域變數，但`catch`子句`foreach`或語句中所宣告的變數除外。</span><span class="sxs-lookup"><span data-stu-id="85516-228">Local variables, except those declared in a `catch` clause or a `foreach` statement.</span></span>

### <a name="precise-rules-for-determining-definite-assignment"></a><span data-ttu-id="85516-229">判斷明確指派的精確規則</span><span class="sxs-lookup"><span data-stu-id="85516-229">Precise rules for determining definite assignment</span></span>

<span data-ttu-id="85516-230">為了判斷每個使用的變數都是明確指派的，編譯器必須使用與本節所述相同的進程。</span><span class="sxs-lookup"><span data-stu-id="85516-230">In order to determine that each used variable is definitely assigned, the compiler must use a process that is equivalent to the one described in this section.</span></span>

<span data-ttu-id="85516-231">編譯器會處理具有一或多個初始未指派變數的每個函式成員的主體。</span><span class="sxs-lookup"><span data-stu-id="85516-231">The compiler processes the body of each function member that has one or more initially unassigned variables.</span></span> <span data-ttu-id="85516-232">針對每個最初未指派的變數*v*，編譯器會在函式成員中的下列每個點上，判斷*v*的***明確指派狀態***：</span><span class="sxs-lookup"><span data-stu-id="85516-232">For each initially unassigned variable *v*, the compiler determines a ***definite assignment state*** for *v* at each of the following points in the function member:</span></span>

*  <span data-ttu-id="85516-233">在每個語句的開頭</span><span class="sxs-lookup"><span data-stu-id="85516-233">At the beginning of each statement</span></span>
*  <span data-ttu-id="85516-234">每個語句的結尾點（[結束點和](statements.md#end-points-and-reachability)可連線）</span><span class="sxs-lookup"><span data-stu-id="85516-234">At the end point ([End points and reachability](statements.md#end-points-and-reachability)) of each statement</span></span>
*  <span data-ttu-id="85516-235">在將控制項傳輸至另一個語句或語句結束點的每個 arc 上</span><span class="sxs-lookup"><span data-stu-id="85516-235">On each arc which transfers control to another statement or to the end point of a statement</span></span>
*  <span data-ttu-id="85516-236">在每個運算式的開頭</span><span class="sxs-lookup"><span data-stu-id="85516-236">At the beginning of each expression</span></span>
*  <span data-ttu-id="85516-237">在每個運算式的結尾處</span><span class="sxs-lookup"><span data-stu-id="85516-237">At the end of each expression</span></span>

<span data-ttu-id="85516-238">*V*的明確指派狀態可以是：</span><span class="sxs-lookup"><span data-stu-id="85516-238">The definite assignment state of *v* can be either:</span></span>

*  <span data-ttu-id="85516-239">明確指派。</span><span class="sxs-lookup"><span data-stu-id="85516-239">Definitely assigned.</span></span> <span data-ttu-id="85516-240">這表示在此點的所有可能控制流程上，已將值指派給*v* 。</span><span class="sxs-lookup"><span data-stu-id="85516-240">This indicates that on all possible control flows to this point, *v* has been assigned a value.</span></span>
*  <span data-ttu-id="85516-241">未明確指派。</span><span class="sxs-lookup"><span data-stu-id="85516-241">Not definitely assigned.</span></span> <span data-ttu-id="85516-242">針對類型`bool`之運算式結尾的變數狀態，未明確指派之變數的狀態可能會屬於下列子狀態之一（但不一定）：</span><span class="sxs-lookup"><span data-stu-id="85516-242">For the state of a variable at the end of an expression of type `bool`, the state of a variable that isn't definitely assigned may (but doesn't necessarily) fall into one of the following sub-states:</span></span>
    * <span data-ttu-id="85516-243">在 true 運算式之後明確指派。</span><span class="sxs-lookup"><span data-stu-id="85516-243">Definitely assigned after true expression.</span></span> <span data-ttu-id="85516-244">此狀態表示如果布林運算式評估為 true，則會明確指派*v* ，但如果布林運算式評估為 false，則不一定會指派。</span><span class="sxs-lookup"><span data-stu-id="85516-244">This state indicates that *v* is definitely assigned if the boolean expression evaluated as true, but is not necessarily assigned if the boolean expression evaluated as false.</span></span>
    * <span data-ttu-id="85516-245">在 false 運算式之後明確指派。</span><span class="sxs-lookup"><span data-stu-id="85516-245">Definitely assigned after false expression.</span></span> <span data-ttu-id="85516-246">此狀態表示如果布林運算式評估為 false，則會明確指派*v* ，但如果布林運算式評估為 true，則不一定會指派。</span><span class="sxs-lookup"><span data-stu-id="85516-246">This state indicates that *v* is definitely assigned if the boolean expression evaluated as false, but is not necessarily assigned if the boolean expression evaluated as true.</span></span>

<span data-ttu-id="85516-247">下列規則控制變數*v*的狀態在每個位置的決定方式。</span><span class="sxs-lookup"><span data-stu-id="85516-247">The following rules govern how the state of a variable *v* is determined at each location.</span></span>

#### <a name="general-rules-for-statements"></a><span data-ttu-id="85516-248">語句的一般規則</span><span class="sxs-lookup"><span data-stu-id="85516-248">General rules for statements</span></span>

*  <span data-ttu-id="85516-249">*v*不會在函式成員主體的開頭明確指派。</span><span class="sxs-lookup"><span data-stu-id="85516-249">*v* is not definitely assigned at the beginning of a function member body.</span></span>
*  <span data-ttu-id="85516-250">*v*會在任何無法連線的語句開頭明確指派。</span><span class="sxs-lookup"><span data-stu-id="85516-250">*v* is definitely assigned at the beginning of any unreachable statement.</span></span>
*  <span data-ttu-id="85516-251">在任何其他語句開頭的*v*明確指派狀態是藉由檢查所有目標為該語句開頭之控制流程傳輸上*v*的明確指派狀態來決定。</span><span class="sxs-lookup"><span data-stu-id="85516-251">The definite assignment state of *v* at the beginning of any other statement is determined by checking the definite assignment state of *v* on all control flow transfers that target the beginning of that statement.</span></span> <span data-ttu-id="85516-252">如果（而且只有在） *v*是在所有此類控制流程傳輸上明確指派，則會在語句的開頭明確指派*v* 。</span><span class="sxs-lookup"><span data-stu-id="85516-252">If (and only if) *v* is definitely assigned on all such control flow transfers, then *v* is definitely assigned at the beginning of the statement.</span></span> <span data-ttu-id="85516-253">一組可能的控制流程傳輸的判斷方式，與檢查語句可連線性（[端點和](statements.md#end-points-and-reachability)可連線性）相同。</span><span class="sxs-lookup"><span data-stu-id="85516-253">The set of possible control flow transfers is determined in the same way as for checking statement reachability ([End points and reachability](statements.md#end-points-and-reachability)).</span></span>
*  <span data-ttu-id="85516-254">*V*在`checked`區塊、 、、`lock`、 `if` 、、、、`do`、或的結束點的明確指派狀態。 `while` `unchecked` `for` `foreach` `using` `switch`語句的決定方式是在所有以該語句的結束點為目標的控制流程傳輸上，檢查*v*的明確指派狀態。</span><span class="sxs-lookup"><span data-stu-id="85516-254">The definite assignment state of *v* at the end point of a block, `checked`, `unchecked`, `if`, `while`, `do`, `for`, `foreach`, `lock`, `using`, or `switch` statement is determined by checking the definite assignment state of *v* on all control flow transfers that target the end point of that statement.</span></span> <span data-ttu-id="85516-255">如果在所有這類的控制流程傳輸上都已明確指派*v* ，則會在語句的結束點明確指派*v* 。</span><span class="sxs-lookup"><span data-stu-id="85516-255">If *v* is definitely assigned on all such control flow transfers, then *v* is definitely assigned at the end point of the statement.</span></span> <span data-ttu-id="85516-256">別的*v*不會在語句的結束點明確指派。</span><span class="sxs-lookup"><span data-stu-id="85516-256">Otherwise; *v* is not definitely assigned at the end point of the statement.</span></span> <span data-ttu-id="85516-257">一組可能的控制流程傳輸的判斷方式，與檢查語句可連線性（[端點和](statements.md#end-points-and-reachability)可連線性）相同。</span><span class="sxs-lookup"><span data-stu-id="85516-257">The set of possible control flow transfers is determined in the same way as for checking statement reachability ([End points and reachability](statements.md#end-points-and-reachability)).</span></span>

#### <a name="block-statements-checked-and-unchecked-statements"></a><span data-ttu-id="85516-258">Block 語句、checked 和 unchecked 語句</span><span class="sxs-lookup"><span data-stu-id="85516-258">Block statements, checked, and unchecked statements</span></span>

<span data-ttu-id="85516-259">在控制項上， *v*的明確指派狀態會傳送到區塊中語句清單的第一個語句（如果語句清單是空的，則為該區塊的結束點），這與*區塊前面的*明確指派語句相同。、 `checked`或`unchecked`語句。</span><span class="sxs-lookup"><span data-stu-id="85516-259">The definite assignment state of *v* on the control transfer to the first statement of the statement list in the block (or to the end point of the block, if the statement list is empty) is the same as the definite assignment statement of *v* before the block, `checked`, or `unchecked` statement.</span></span>

#### <a name="expression-statements"></a><span data-ttu-id="85516-260">運算式陳述式</span><span class="sxs-lookup"><span data-stu-id="85516-260">Expression statements</span></span>

<span data-ttu-id="85516-261">針對運算式語句包含運算式*expr*的*stmt* ：</span><span class="sxs-lookup"><span data-stu-id="85516-261">For an expression statement *stmt* that consists of the expression *expr*:</span></span>

*  <span data-ttu-id="85516-262">*v*在*expr*開頭具有相同的明確指派狀態，如同在*stmt*開頭。</span><span class="sxs-lookup"><span data-stu-id="85516-262">*v* has the same definite assignment state at the beginning of *expr* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="85516-263">如果*v*是在*expr*結尾明確指派，則會在*stmt*的結束點明確指派它;別的它不會在*stmt*的結束點明確指派。</span><span class="sxs-lookup"><span data-stu-id="85516-263">If *v* if definitely assigned at the end of *expr*, it is definitely assigned at the end point of *stmt*; otherwise; it is not definitely assigned at the end point of *stmt*.</span></span>

#### <a name="declaration-statements"></a><span data-ttu-id="85516-264">宣告陳述式</span><span class="sxs-lookup"><span data-stu-id="85516-264">Declaration statements</span></span>

*  <span data-ttu-id="85516-265">如果*stmt*不是沒有初始化運算式的宣告語句，則*v* *在 stmt 的*結束點具有相同的明確指派狀態，如同在*stmt*的開頭。</span><span class="sxs-lookup"><span data-stu-id="85516-265">If *stmt* is a declaration statement without initializers, then *v* has the same definite assignment state at the end point of *stmt* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="85516-266">如果*stmt*是具有初始化運算式的宣告語句，則*v*的明確指派狀態會決定為語句清單，每個具有初始化運算式的*宣告都有*一個指派語句（順序為宣告）。</span><span class="sxs-lookup"><span data-stu-id="85516-266">If *stmt* is a declaration statement with initializers, then the definite assignment state for *v* is determined as if *stmt* were a statement list, with one assignment statement for each declaration with an initializer (in the order of declaration).</span></span>

#### <a name="if-statements"></a><span data-ttu-id="85516-267">If 語句</span><span class="sxs-lookup"><span data-stu-id="85516-267">If statements</span></span>

<span data-ttu-id="85516-268">針對格式為的語句`if` stmt：</span><span class="sxs-lookup"><span data-stu-id="85516-268">For an `if` statement *stmt* of the form:</span></span>
```csharp
if ( expr ) then_stmt else else_stmt
```

*  <span data-ttu-id="85516-269">*v*在*expr*開頭具有相同的明確指派狀態，如同在*stmt*開頭。</span><span class="sxs-lookup"><span data-stu-id="85516-269">*v* has the same definite assignment state at the beginning of *expr* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="85516-270">如果在*expr*的結尾明確指派*v* ，則在控制流程傳輸至*then_stmt* ，以及如果沒有 else*子句時，* 會將它明確指派給*else_stmt*或指標的端點。</span><span class="sxs-lookup"><span data-stu-id="85516-270">If *v* is definitely assigned at the end of *expr*, then it is definitely assigned on the control flow transfer to *then_stmt* and to either *else_stmt* or to the end-point of *stmt* if there is no else clause.</span></span>
*  <span data-ttu-id="85516-271">如果*v*在*expr*結尾處的「true expression 之後明確指派」狀態，則會在控制流程傳輸至*then_stmt*時明確指派，而不會在控制流程傳輸上明確指派給任一*else_stmt* ，或如果沒有 else 子句，則為*stmt*的結束點。</span><span class="sxs-lookup"><span data-stu-id="85516-271">If *v* has the state "definitely assigned after true expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to *then_stmt*, and not definitely assigned on the control flow transfer to either *else_stmt* or to the end-point of *stmt* if there is no else clause.</span></span>
*  <span data-ttu-id="85516-272">如果*v*在*expr*結尾處具有「在 false 運算式之後明確指派」狀態，則會在控制流程傳送至*else_stmt*時明確指派，而不會在控制流程傳輸至 then_stmt 時明確指派。 .</span><span class="sxs-lookup"><span data-stu-id="85516-272">If *v* has the state "definitely assigned after false expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to *else_stmt*, and not definitely assigned on the control flow transfer to *then_stmt*.</span></span> <span data-ttu-id="85516-273">只有在*then_stmt*的端點明確指派時，才會在*stmt*的端點上明確指派。</span><span class="sxs-lookup"><span data-stu-id="85516-273">It is definitely assigned at the end-point of *stmt* if and only if it is definitely assigned at the end-point of *then_stmt*.</span></span>
*  <span data-ttu-id="85516-274">否則 *，如果沒有*else 子句，則會將*v*視為不會在控制流程傳輸到*then_stmt*或*else_stmt*時明確指派。</span><span class="sxs-lookup"><span data-stu-id="85516-274">Otherwise, *v* is considered not definitely assigned on the control flow transfer to either the *then_stmt* or *else_stmt*, or to the end-point of *stmt* if there is no else clause.</span></span>

#### <a name="switch-statements"></a><span data-ttu-id="85516-275">Switch 語句</span><span class="sxs-lookup"><span data-stu-id="85516-275">Switch statements</span></span>

<span data-ttu-id="85516-276">在具有控制運算式*expr*的語句stmt`switch`中：</span><span class="sxs-lookup"><span data-stu-id="85516-276">In a `switch` statement *stmt* with a controlling expression *expr*:</span></span>

*  <span data-ttu-id="85516-277">*V*在*expr*開頭的明確指派狀態，與*stmt*開頭的*v*狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-277">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="85516-278">在控制流程上的*v*明確指派狀態傳送至可連線的 switch block 語句清單，與*expr*結尾的*v*明確指派狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-278">The definite assignment state of *v* on the control flow transfer to a reachable switch block statement list is the same as the definite assignment state of *v* at the end of *expr*.</span></span>

#### <a name="while-statements"></a><span data-ttu-id="85516-279">While 語句</span><span class="sxs-lookup"><span data-stu-id="85516-279">While statements</span></span>

<span data-ttu-id="85516-280">針對格式為的語句`while` stmt：</span><span class="sxs-lookup"><span data-stu-id="85516-280">For a `while` statement *stmt* of the form:</span></span>
```csharp
while ( expr ) while_body
```

*  <span data-ttu-id="85516-281">*v*在*expr*開頭具有相同的明確指派狀態，如同在*stmt*開頭。</span><span class="sxs-lookup"><span data-stu-id="85516-281">*v* has the same definite assignment state at the beginning of *expr* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="85516-282">如果在*expr*結尾明確指派*v* ，則會在控制流程傳輸上明確指派給*while_body*和*stmt*的結束點。</span><span class="sxs-lookup"><span data-stu-id="85516-282">If *v* is definitely assigned at the end of *expr*, then it is definitely assigned on the control flow transfer to *while_body* and to the end point of *stmt*.</span></span>
*  <span data-ttu-id="85516-283">如果*v*在*expr*結尾處的「true expression 之後明確指派」狀態，則會在控制流程傳送至*while_body*時明確指派，但不會在*stmt*的端點上明確指派。</span><span class="sxs-lookup"><span data-stu-id="85516-283">If *v* has the state "definitely assigned after true expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to *while_body*, but not definitely assigned at the end-point of *stmt*.</span></span>
*  <span data-ttu-id="85516-284">如果*v*在*expr*結尾處具有「在 false 運算式之後明確指派」狀態，則會在控制流程傳輸到*stmt*的結束點時明確指派，但不會在控制流程傳輸上明確指派 *_body*。</span><span class="sxs-lookup"><span data-stu-id="85516-284">If *v* has the state "definitely assigned after false expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to the end point of *stmt*, but not definitely assigned on the control flow transfer to *while_body*.</span></span>

#### <a name="do-statements"></a><span data-ttu-id="85516-285">Do 語句</span><span class="sxs-lookup"><span data-stu-id="85516-285">Do statements</span></span>

<span data-ttu-id="85516-286">針對格式為的語句`do` stmt：</span><span class="sxs-lookup"><span data-stu-id="85516-286">For a `do` statement *stmt* of the form:</span></span>
```csharp
do do_body while ( expr ) ;
```

*  <span data-ttu-id="85516-287">*v*在控制流程上具有相同的明確指派狀態，從*stmt*開頭到*do_body* ，如同在*stmt*的開頭。</span><span class="sxs-lookup"><span data-stu-id="85516-287">*v* has the same definite assignment state on the control flow transfer from the beginning of *stmt* to *do_body* as at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="85516-288">*v*在*expr*的開頭具有相同的明確指派狀態，如同*do_body*的結束點。</span><span class="sxs-lookup"><span data-stu-id="85516-288">*v* has the same definite assignment state at the beginning of *expr* as at the end point of *do_body*.</span></span>
*  <span data-ttu-id="85516-289">如果在*expr*的結尾明確指派*v* ，則會在控制流程傳輸上，將它明確指派給*stmt*的結束點。</span><span class="sxs-lookup"><span data-stu-id="85516-289">If *v* is definitely assigned at the end of *expr*, then it is definitely assigned on the control flow transfer to the end point of *stmt*.</span></span>
*  <span data-ttu-id="85516-290">如果*v*在*expr*結尾處具有「在 false 運算式之後明確指派」狀態，則會在控制流程傳輸上，將它明確指派給*stmt*的結束點。</span><span class="sxs-lookup"><span data-stu-id="85516-290">If *v* has the state "definitely assigned after false expression" at the end of *expr*, then it is definitely assigned on the control flow transfer to the end point of *stmt*.</span></span>

#### <a name="for-statements"></a><span data-ttu-id="85516-291">適用于語句</span><span class="sxs-lookup"><span data-stu-id="85516-291">For statements</span></span>

<span data-ttu-id="85516-292">`for`語句的明確指派檢查，其格式如下：</span><span class="sxs-lookup"><span data-stu-id="85516-292">Definite assignment checking for a `for` statement of the form:</span></span>
```csharp
for ( for_initializer ; for_condition ; for_iterator ) embedded_statement
```
<span data-ttu-id="85516-293">執行方式就如同撰寫語句一樣：</span><span class="sxs-lookup"><span data-stu-id="85516-293">is done as if the statement were written:</span></span>
```csharp
{
    for_initializer ;
    while ( for_condition ) {
        embedded_statement ;
        for_iterator ;
    }
}
```

<span data-ttu-id="85516-294">如果`for`語句中省略了*for_condition* ，則評估明確的指派會繼續進行，如同上述擴充`true`中的 for_condition 已被取代。</span><span class="sxs-lookup"><span data-stu-id="85516-294">If the *for_condition* is omitted from the `for` statement, then evaluation of definite assignment proceeds as if *for_condition* were replaced with `true` in the above expansion.</span></span>

#### <a name="break-continue-and-goto-statements"></a><span data-ttu-id="85516-295">Break、continue 和 goto 語句</span><span class="sxs-lookup"><span data-stu-id="85516-295">Break, continue, and goto statements</span></span>

<span data-ttu-id="85516-296">`break`在、或`continue`  語句`goto`所造成的控制流程傳輸上，v 的明確指派狀態，與語句開頭的*v*明確指派狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-296">The definite assignment state of *v* on the control flow transfer caused by a `break`, `continue`, or `goto` statement is the same as the definite assignment state of *v* at the beginning of the statement.</span></span>

#### <a name="throw-statements"></a><span data-ttu-id="85516-297">Throw 語句</span><span class="sxs-lookup"><span data-stu-id="85516-297">Throw statements</span></span>

<span data-ttu-id="85516-298">適用于表單的語句*stmt*</span><span class="sxs-lookup"><span data-stu-id="85516-298">For a statement *stmt* of the form</span></span>
```csharp
throw expr ;
```

<span data-ttu-id="85516-299">*V*在*expr*開頭的明確指派狀態，與*stmt*開頭的*v*明確指派狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-299">The definite assignment state of *v* at the beginning of *expr* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>

#### <a name="return-statements"></a><span data-ttu-id="85516-300">Return 語句</span><span class="sxs-lookup"><span data-stu-id="85516-300">Return statements</span></span>

<span data-ttu-id="85516-301">適用于表單的語句*stmt*</span><span class="sxs-lookup"><span data-stu-id="85516-301">For a statement *stmt* of the form</span></span>
```csharp
return expr ;
```

*  <span data-ttu-id="85516-302">*V*在*expr*開頭的明確指派狀態，與*stmt*開頭的*v*明確指派狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-302">The definite assignment state of *v* at the beginning of *expr* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="85516-303">如果*v*是輸出參數，則必須明確地指派其中一個：</span><span class="sxs-lookup"><span data-stu-id="85516-303">If *v* is an output parameter, then it must be definitely assigned either:</span></span>
    * <span data-ttu-id="85516-304">after *expr*之後</span><span class="sxs-lookup"><span data-stu-id="85516-304">after *expr*</span></span>
    * <span data-ttu-id="85516-305">或，位於括`finally` 住`try`語句- `try`之或`finally` 的區塊-結尾。 `catch` - `finally` `return`</span><span class="sxs-lookup"><span data-stu-id="85516-305">or at the end of the `finally` block of a `try`-`finally` or `try`-`catch`-`finally` that encloses the `return` statement.</span></span>

<span data-ttu-id="85516-306">針對格式為的語句 stmt：</span><span class="sxs-lookup"><span data-stu-id="85516-306">For a statement stmt of the form:</span></span>
```csharp
return ;
```

*  <span data-ttu-id="85516-307">如果*v*是輸出參數，則必須明確地指派其中一個：</span><span class="sxs-lookup"><span data-stu-id="85516-307">If *v* is an output parameter, then it must be definitely assigned either:</span></span>
    * <span data-ttu-id="85516-308">在*stmt*之前</span><span class="sxs-lookup"><span data-stu-id="85516-308">before *stmt*</span></span>
    * <span data-ttu-id="85516-309">或，位於括`finally` 住`try`語句- `try`之或`finally` 的區塊-結尾。 `catch` - `finally` `return`</span><span class="sxs-lookup"><span data-stu-id="85516-309">or at the end of the `finally` block of a `try`-`finally` or `try`-`catch`-`finally` that encloses the `return` statement.</span></span>

#### <a name="try-catch-statements"></a><span data-ttu-id="85516-310">Try-catch 語句</span><span class="sxs-lookup"><span data-stu-id="85516-310">Try-catch statements</span></span>

<span data-ttu-id="85516-311">針對格式為的語句*stmt* ：</span><span class="sxs-lookup"><span data-stu-id="85516-311">For a statement *stmt* of the form:</span></span>
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
```

*  <span data-ttu-id="85516-312">*V*在*try_block*開頭的明確指派狀態，與*stmt*開頭的*v*明確指派狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-312">The definite assignment state of *v* at the beginning of *try_block* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="85516-313">*V*在*catch_block_i*開頭的明確指派狀態（適用于任何*i*）與*stmt*開頭的*v*明確指派狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-313">The definite assignment state of *v* at the beginning of *catch_block_i* (for any *i*) is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="85516-314">如果在*try_block*的端點和每個*catch_block_i* （針對每個*i*從1到 n）明確指派*v* ，則會明確指派*v*在*stmt*結束點的明確指派狀態。 ).</span><span class="sxs-lookup"><span data-stu-id="85516-314">The definite assignment state of *v* at the end-point of *stmt* is definitely assigned if (and only if) *v* is definitely assigned at the end-point of *try_block* and every *catch_block_i* (for every *i* from 1 to *n*).</span></span>

#### <a name="try-finally-statements"></a><span data-ttu-id="85516-315">Try-finally 語句</span><span class="sxs-lookup"><span data-stu-id="85516-315">Try-finally statements</span></span>

<span data-ttu-id="85516-316">針對格式為的語句`try` stmt：</span><span class="sxs-lookup"><span data-stu-id="85516-316">For a `try` statement *stmt* of the form:</span></span>
```csharp
try try_block finally finally_block
```

*  <span data-ttu-id="85516-317">*V*在*try_block*開頭的明確指派狀態，與*stmt*開頭的*v*明確指派狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-317">The definite assignment state of *v* at the beginning of *try_block* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="85516-318">*V*在*finally_block*開頭的明確指派狀態，與*stmt*開頭的*v*明確指派狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-318">The definite assignment state of *v* at the beginning of *finally_block* is the same as the definite assignment state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="85516-319">只有在下列其中一項條件成立時，才會明確指派*v*在*stmt*結束點的明確指派狀態：</span><span class="sxs-lookup"><span data-stu-id="85516-319">The definite assignment state of *v* at the end-point of *stmt* is definitely assigned if (and only if) at least one of the following is true:</span></span>
    * <span data-ttu-id="85516-320">*v*在*try_block*的端點明確指派</span><span class="sxs-lookup"><span data-stu-id="85516-320">*v* is definitely assigned at the end-point of *try_block*</span></span>
    * <span data-ttu-id="85516-321">*v*在*finally_block*的端點明確指派</span><span class="sxs-lookup"><span data-stu-id="85516-321">*v* is definitely assigned at the end-point of *finally_block*</span></span>

<span data-ttu-id="85516-322">如果在 try_block 內開始進行控制流程傳輸（ `goto`例如，語句），並在 *try_block*之外結束，則如果*v*為，則也會將*v*視為明確指派給該控制流程傳輸明確指派于*finally_block*的端點。</span><span class="sxs-lookup"><span data-stu-id="85516-322">If a control flow transfer (for example, a `goto` statement) is made that begins within *try_block*, and ends outside of *try_block*, then *v* is also considered definitely assigned on that control flow transfer if *v* is definitely assigned at the end-point of *finally_block*.</span></span> <span data-ttu-id="85516-323">（這不是唯一的情況，如果基於此控制流程轉移的另一個原因明確指派*v* ，則仍會被視為明確指派）。</span><span class="sxs-lookup"><span data-stu-id="85516-323">(This is not an only if—if *v* is definitely assigned for another reason on this control flow transfer, then it is still considered definitely assigned.)</span></span>

#### <a name="try-catch-finally-statements"></a><span data-ttu-id="85516-324">Try-catch-finally 語句</span><span class="sxs-lookup"><span data-stu-id="85516-324">Try-catch-finally statements</span></span>

<span data-ttu-id="85516-325">適用`try` 于窗`catch`體之語句的`finally`明確指派分析： - -</span><span class="sxs-lookup"><span data-stu-id="85516-325">Definite assignment analysis for a `try`-`catch`-`finally` statement of the form:</span></span>
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
finally *finally_block*
```
<span data-ttu-id="85516-326">其執行方式就如同`try`語句是`finally` -包含`try` -語句的語句：`catch`</span><span class="sxs-lookup"><span data-stu-id="85516-326">is done as if the statement were a `try`-`finally` statement enclosing a `try`-`catch` statement:</span></span>
```csharp
try {
    try try_block
    catch(...) catch_block_1
    ...
    catch(...) catch_block_n
}
finally finally_block
```

<span data-ttu-id="85516-327">下列範例示範`try`語句的不同區塊（[try 語句](statements.md#the-try-statement)）如何影響明確指派。</span><span class="sxs-lookup"><span data-stu-id="85516-327">The following example demonstrates how the different blocks of a `try` statement ([The try statement](statements.md#the-try-statement)) affect definite assignment.</span></span>
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

#### <a name="foreach-statements"></a><span data-ttu-id="85516-328">Foreach 語句</span><span class="sxs-lookup"><span data-stu-id="85516-328">Foreach statements</span></span>

<span data-ttu-id="85516-329">針對格式為的語句`foreach` stmt：</span><span class="sxs-lookup"><span data-stu-id="85516-329">For a `foreach` statement *stmt* of the form:</span></span>
```csharp
foreach ( type identifier in expr ) embedded_statement
```

*  <span data-ttu-id="85516-330">*V*在*expr*開頭的明確指派狀態，與*stmt*開頭的*v*狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-330">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="85516-331">*V*在控制流程傳輸到*embedded_statement*或*stmt*結束點的明確指派狀態，與*expr*結尾的*v*狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-331">The definite assignment state of *v* on the control flow transfer to *embedded_statement* or to the end point of *stmt* is the same as the state of *v* at the end of *expr*.</span></span>

#### <a name="using-statements"></a><span data-ttu-id="85516-332">Using 語句</span><span class="sxs-lookup"><span data-stu-id="85516-332">Using statements</span></span>

<span data-ttu-id="85516-333">針對格式為的語句`using` stmt：</span><span class="sxs-lookup"><span data-stu-id="85516-333">For a `using` statement *stmt* of the form:</span></span>
```csharp
using ( resource_acquisition ) embedded_statement
```

*  <span data-ttu-id="85516-334">*V*在*resource_acquisition*開頭的明確指派狀態，與*stmt*開頭的*v*狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-334">The definite assignment state of *v* at the beginning of *resource_acquisition* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="85516-335">在控制流程傳輸至*embedded_statement*上， *v*的明確指派狀態與*resource_acquisition*結尾的*v*狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-335">The definite assignment state of *v* on the control flow transfer to *embedded_statement* is the same as the state of *v* at the end of *resource_acquisition*.</span></span>

#### <a name="lock-statements"></a><span data-ttu-id="85516-336">Lock 語句</span><span class="sxs-lookup"><span data-stu-id="85516-336">Lock statements</span></span>

<span data-ttu-id="85516-337">針對格式為的語句`lock` stmt：</span><span class="sxs-lookup"><span data-stu-id="85516-337">For a `lock` statement *stmt* of the form:</span></span>
```csharp
lock ( expr ) embedded_statement
```

*  <span data-ttu-id="85516-338">*V*在*expr*開頭的明確指派狀態，與*stmt*開頭的*v*狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-338">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="85516-339">在控制流程傳輸至*embedded_statement*上， *v*的明確指派狀態與*expr*結尾的*v*狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-339">The definite assignment state of *v* on the control flow transfer to *embedded_statement* is the same as the state of *v* at the end of *expr*.</span></span>

#### <a name="yield-statements"></a><span data-ttu-id="85516-340">Yield 語句</span><span class="sxs-lookup"><span data-stu-id="85516-340">Yield statements</span></span>

<span data-ttu-id="85516-341">針對格式為的語句`yield return` stmt：</span><span class="sxs-lookup"><span data-stu-id="85516-341">For a `yield return` statement *stmt* of the form:</span></span>
```csharp
yield return expr ;
```

*  <span data-ttu-id="85516-342">*V*在*expr*開頭的明確指派狀態，與*stmt*開頭的*v*狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-342">The definite assignment state of *v* at the beginning of *expr* is the same as the state of *v* at the beginning of *stmt*.</span></span>
*  <span data-ttu-id="85516-343">*V*在*stmt*結尾的明確指派狀態，與*expr*結尾的*v*狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-343">The definite assignment state of *v* at the end of *stmt* is the same as the state of *v* at the end of *expr*.</span></span>
*  <span data-ttu-id="85516-344">`yield break`語句對明確的指派狀態不會有任何影響。</span><span class="sxs-lookup"><span data-stu-id="85516-344">A `yield break` statement has no effect on the definite assignment state.</span></span>

#### <a name="general-rules-for-simple-expressions"></a><span data-ttu-id="85516-345">簡單運算式的一般規則</span><span class="sxs-lookup"><span data-stu-id="85516-345">General rules for simple expressions</span></span>

<span data-ttu-id="85516-346">下列規則適用于這類運算式：常值（[常](expressions.md#literals)值）、簡單名稱（[簡單名稱](expressions.md#simple-names)）、成員存取運算式（[成員存取](expressions.md#member-access)）、未編制索引的基底存取運算式（[基底存取](expressions.md#base-access)）、 `typeof`運算式（[typeof 運算子](expressions.md#the-typeof-operator)）、預設值運算式（[預設值運算式](expressions.md#default-value-expressions)）和`nameof`運算式（[Nameof 運算式](expressions.md#nameof-expressions)）。</span><span class="sxs-lookup"><span data-stu-id="85516-346">The following rule applies to these kinds of expressions: literals ([Literals](expressions.md#literals)), simple names ([Simple names](expressions.md#simple-names)), member access expressions ([Member access](expressions.md#member-access)), non-indexed base access expressions ([Base access](expressions.md#base-access)), `typeof` expressions ([The typeof operator](expressions.md#the-typeof-operator)), default value expressions ([Default value expressions](expressions.md#default-value-expressions)) and `nameof` expressions ([Nameof expressions](expressions.md#nameof-expressions)).</span></span>

*  <span data-ttu-id="85516-347">在這類運算式結尾的*v*明確指派狀態，與運算式開頭的*v*明確指派狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-347">The definite assignment state of *v* at the end of such an expression is the same as the definite assignment state of *v* at the beginning of the expression.</span></span>

#### <a name="general-rules-for-expressions-with-embedded-expressions"></a><span data-ttu-id="85516-348">具有內嵌運算式之運算式的一般規則</span><span class="sxs-lookup"><span data-stu-id="85516-348">General rules for expressions with embedded expressions</span></span>

<span data-ttu-id="85516-349">下列規則適用于這類運算式：括號運算式（[括號運算式](expressions.md#parenthesized-expressions)）、元素存取運算式（專案[存取](expressions.md#element-access)）、具有索引編制的基底存取運算式（[基底存取](expressions.md#base-access)）、遞增和遞減運算式（後置[遞增和遞減運算子](expressions.md#postfix-increment-and-decrement-operators)、[前置遞增和遞減運算子](expressions.md#prefix-increment-and-decrement-operators)）、cast 運算式（[cast 運算式](expressions.md#cast-expressions)）、 `+`一元`-`、 `~`、 `*`、運算式，binary `+` `-` ，`*`， ，，`/`，，，，，，， ，，，，`>=`，，，， `%` `<<` `>>` `<` `<=` `>``==`、 、、`is` 、`^` 、、運算式（[算術運算子](expressions.md#arithmetic-operators)、[移位運算子](expressions.md#shift-operators)、關聯式和類型測試） `|` `as` `!=` `&` [運算子](expressions.md#relational-and-type-testing-operators)、[邏輯運算子](expressions.md#logical-operators)）、複合指派運算式（[複合](expressions.md#compound-assignment) `checked`指派）和`unchecked`運算式（[checked 和 unchecked 運算子](expressions.md#the-checked-and-unchecked-operators)），加上陣列和委派建立運算式（[新的運算子](expressions.md#the-new-operator)）。</span><span class="sxs-lookup"><span data-stu-id="85516-349">The following rules apply to these kinds of expressions: parenthesized expressions ([Parenthesized expressions](expressions.md#parenthesized-expressions)), element access expressions ([Element access](expressions.md#element-access)), base access expressions with indexing ([Base access](expressions.md#base-access)), increment and decrement expressions ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators), [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), cast expressions ([Cast expressions](expressions.md#cast-expressions)), unary `+`, `-`, `~`, `*` expressions, binary `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `<`, `<=`, `>`, `>=`, `==`, `!=`, `is`, `as`, `&`, `|`, `^` expressions ([Arithmetic operators](expressions.md#arithmetic-operators), [Shift operators](expressions.md#shift-operators), [Relational and type-testing operators](expressions.md#relational-and-type-testing-operators), [Logical operators](expressions.md#logical-operators)), compound assignment expressions ([Compound assignment](expressions.md#compound-assignment)), `checked` and `unchecked` expressions ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)), plus array and delegate creation expressions ([The new operator](expressions.md#the-new-operator)).</span></span>

<span data-ttu-id="85516-350">這些運算式中的每一個都有一或多個以固定順序無條件評估的子運算式。</span><span class="sxs-lookup"><span data-stu-id="85516-350">Each of these expressions has one or more sub-expressions that are unconditionally evaluated in a fixed order.</span></span> <span data-ttu-id="85516-351">例如，二元`%`運算子會評估運算子的左邊，然後是右手邊。</span><span class="sxs-lookup"><span data-stu-id="85516-351">For example, the binary `%` operator evaluates the left hand side of the operator, then the right hand side.</span></span> <span data-ttu-id="85516-352">索引作業會評估索引運算式，然後依序由左至右來評估每個索引運算式。</span><span class="sxs-lookup"><span data-stu-id="85516-352">An indexing operation evaluates the indexed expression, and then evaluates each of the index expressions, in order from left to right.</span></span> <span data-ttu-id="85516-353">針對具有子運算式*e1、e2、...、eN*的運算式*expr*，會依照該順序評估：</span><span class="sxs-lookup"><span data-stu-id="85516-353">For an expression *expr*, which has sub-expressions *e1, e2, ..., eN*, evaluated in that order:</span></span>

*  <span data-ttu-id="85516-354">在*e1*開頭的*v*明確指派狀態，與*expr*開頭的明確指派狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-354">The definite assignment state of *v* at the beginning of *e1* is the same as the definite assignment state at the beginning of *expr*.</span></span>
*  <span data-ttu-id="85516-355">*V*在*ei*開頭的明確指派狀態（*i*大於1）與前一個子運算式結尾的明確指派狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-355">The definite assignment state of *v* at the beginning of *ei* (*i* greater than one) is the same as the definite assignment state at the end of the previous sub-expression.</span></span>
*  <span data-ttu-id="85516-356">*V*在*expr*結尾的明確指派狀態，與*eN*結尾的明確指派狀態相同</span><span class="sxs-lookup"><span data-stu-id="85516-356">The definite assignment state of *v* at the end of *expr* is the same as the definite assignment state at the end of *eN*</span></span>

#### <a name="invocation-expressions-and-object-creation-expressions"></a><span data-ttu-id="85516-357">調用運算式和物件建立運算式</span><span class="sxs-lookup"><span data-stu-id="85516-357">Invocation expressions and object creation expressions</span></span>

<span data-ttu-id="85516-358">針對格式為的調用運算式*expr* ：</span><span class="sxs-lookup"><span data-stu-id="85516-358">For an invocation expression *expr* of the form:</span></span>
```csharp
primary_expression ( arg1 , arg2 , ... , argN )
```
<span data-ttu-id="85516-359">或表單的物件建立運算式：</span><span class="sxs-lookup"><span data-stu-id="85516-359">or an object creation expression of the form:</span></span>
```csharp
new type ( arg1 , arg2 , ... , argN )
```

*  <span data-ttu-id="85516-360">針對叫用運算式， *v*之前的*primary_expression*明確指派狀態與*expr\*\*之前的*狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-360">For an invocation expression, the definite assignment state of *v* before *primary_expression* is the same as the state of *v* before *expr*.</span></span>
*  <span data-ttu-id="85516-361">針對叫用運算式， *v*的明確指派狀態在*arg1*之前，會與*primary_expression*之後的*v*狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-361">For an invocation expression, the definite assignment state of *v* before *arg1* is the same as the state of *v* after *primary_expression*.</span></span>
*  <span data-ttu-id="85516-362">若為物件建立運算式，*則在* *arg1*前面的明確指派狀態會與*expr*前面的*v*狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-362">For an object creation expression, the definite assignment state of *v* before *arg1* is the same as the state of *v* before *expr*.</span></span>
*  <span data-ttu-id="85516-363">針對每個引數*argi*，在*argi*之後， *v*的明確指派狀態是由一般運算式規則所決定`ref` ， `out`忽略任何或修飾詞。</span><span class="sxs-lookup"><span data-stu-id="85516-363">For each argument *argi*, the definite assignment state of *v* after *argi* is determined by the normal expression rules, ignoring any `ref` or `out` modifiers.</span></span>
*  <span data-ttu-id="85516-364">針對任何*i*大於1的每個引數*argi* ， *v*在*argi*之前的明確指派狀態與上一個*arg*之後的*v*狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-364">For each argument *argi* for any *i* greater than one, the definite assignment state of *v* before *argi* is the same as the state of *v* after the previous *arg*.</span></span>
*  <span data-ttu-id="85516-365">如果變數*v*在任何引數中`out`當做引數傳遞（也就是表單`out v`的引數），則會明確指派*v*在*expr*之後的狀態。</span><span class="sxs-lookup"><span data-stu-id="85516-365">If the variable *v* is passed as an `out` argument (i.e., an argument of the form `out v`) in any of the arguments, then the state of *v* after *expr* is definitely assigned.</span></span> <span data-ttu-id="85516-366">別的*v* after *expr*的狀態與 *...argn*之後的*v*狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-366">Otherwise; the state of *v* after *expr* is the same as the state of *v* after *argN*.</span></span>
*  <span data-ttu-id="85516-367">陣列初始化運算式（[陣列建立運算式](expressions.md#array-creation-expressions)）、物件初始化運算式（[物件初始化](expressions.md#object-initializers)運算式）、集合初始化運算式（[集合初始化](expressions.md#collection-initializers)運算式）和匿名物件初始化運算式（[匿名物件建立）運算式](expressions.md#anonymous-object-creation-expressions)），明確的指派狀態是由根據來定義這些結構的擴充所決定。</span><span class="sxs-lookup"><span data-stu-id="85516-367">For array initializers ([Array creation expressions](expressions.md#array-creation-expressions)), object initializers ([Object initializers](expressions.md#object-initializers)), collection initializers ([Collection initializers](expressions.md#collection-initializers)) and anonymous object initializers ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)), the definite assignment state is determined by the expansion that these constructs are defined in terms of.</span></span>

#### <a name="simple-assignment-expressions"></a><span data-ttu-id="85516-368">簡單指派運算式</span><span class="sxs-lookup"><span data-stu-id="85516-368">Simple assignment expressions</span></span>

<span data-ttu-id="85516-369">針對格式`w = expr_rhs`為的運算式*expr* ：</span><span class="sxs-lookup"><span data-stu-id="85516-369">For an expression *expr* of the form `w = expr_rhs`:</span></span>

*  <span data-ttu-id="85516-370">*V*在*expr_rhs*之前的明確指派狀態，與*expr\*\*前面的*明確指派狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-370">The definite assignment state of *v* before *expr_rhs* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="85516-371">*V* after *expr*之後的明確指派狀態取決於：</span><span class="sxs-lookup"><span data-stu-id="85516-371">The definite assignment state of *v* after *expr* is determined by:</span></span>
   * <span data-ttu-id="85516-372">如果*w*與*v*是相同的變數，則會明確指派*v* after *expr*之後的明確指派狀態。</span><span class="sxs-lookup"><span data-stu-id="85516-372">If *w* is the same variable as *v*, then the definite assignment state of *v* after *expr* is definitely assigned.</span></span>
   * <span data-ttu-id="85516-373">否則，如果指派發生在結構型別的實例函式中，則如果*w*是屬性存取，在所建立的實例上指定自動實作為屬性*P* ，而*v*是的隱藏支援欄位*P*，則會明確指派*v* after *expr*之後的明確指派狀態。</span><span class="sxs-lookup"><span data-stu-id="85516-373">Otherwise, if the assignment occurs within the instance constructor of a struct type, if *w* is a property access designating an automatically implemented property *P* on the instance being constructed and *v* is the hidden backing field of *P*, then the definite assignment state of *v* after *expr* is definitely assigned.</span></span>
   * <span data-ttu-id="85516-374">否則， *v* after *expr*之後的明確指派狀態，會與*expr_rhs*之後之*v*的明確指派狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-374">Otherwise, the definite assignment state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_rhs*.</span></span>

#### <a name="-conditional-and-expressions"></a><span data-ttu-id="85516-375">& & （條件式和）運算式</span><span class="sxs-lookup"><span data-stu-id="85516-375">&& (conditional AND) expressions</span></span>

<span data-ttu-id="85516-376">針對格式`expr_first && expr_second`為的運算式*expr* ：</span><span class="sxs-lookup"><span data-stu-id="85516-376">For an expression *expr* of the form `expr_first && expr_second`:</span></span>

*  <span data-ttu-id="85516-377">*V*在*expr_first*之前的明確指派狀態，與*expr\*\*前面的*明確指派狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-377">The definite assignment state of *v* before *expr_first* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="85516-378">如果在*expr_first*之後的*v*狀態已明確指派，或「在 true 運算式之後明確指派」，則*expr_second* *的明確*指派狀態會明確指派。</span><span class="sxs-lookup"><span data-stu-id="85516-378">The definite assignment state of *v* before *expr_second* is definitely assigned if the state of *v* after *expr_first* is either definitely assigned or "definitely assigned after true expression".</span></span> <span data-ttu-id="85516-379">否則，它不會被明確指派。</span><span class="sxs-lookup"><span data-stu-id="85516-379">Otherwise, it is not definitely assigned.</span></span>
*  <span data-ttu-id="85516-380">*V* after *expr*之後的明確指派狀態取決於：</span><span class="sxs-lookup"><span data-stu-id="85516-380">The definite assignment state of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="85516-381">如果*expr_first*是具有值`false`的常數運算式，則*v* after *expr*的明確指派狀態會與*expr_first*之後的*v*明確指派狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-381">If *expr_first* is a constant expression with the value `false`, then the definite assignment state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_first*.</span></span>
    * <span data-ttu-id="85516-382">否則，如果已明確指派*expr_first*後*v*的狀態，則會將*v*的狀態明確指派給*expr* 。</span><span class="sxs-lookup"><span data-stu-id="85516-382">Otherwise, if the state of *v* after *expr_first* is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="85516-383">否則，如果已明確指派*expr_second*後*v*的狀態，且*expr_first*之後的*v*狀態是「在 false 運算式之後明確指派」，則*v*的狀態一定會是*expr*指派.</span><span class="sxs-lookup"><span data-stu-id="85516-383">Otherwise, if the state of *v* after *expr_second* is definitely assigned, and the state of *v* after *expr_first* is "definitely assigned after false expression", then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="85516-384">否則，如果*expr_second*後的*v*狀態是明確指派的，或「在 true 運算式之後明確指派」，則在*expr*之後的*v*狀態會是「在 true 運算式之後明確指派」。</span><span class="sxs-lookup"><span data-stu-id="85516-384">Otherwise, if the state of *v* after *expr_second* is definitely assigned or "definitely assigned after true expression", then the state of *v* after *expr* is "definitely assigned after true expression".</span></span>
    * <span data-ttu-id="85516-385">否則，如果*expr_first*之後的*v*狀態是「在 false 運算式之後明確指派」，而且*expr_second*之後的*v*狀態是「在 false 運算式之後明確指派」，則*v*的狀態會在之後*expr*是「在 false 運算式之後明確指派」。</span><span class="sxs-lookup"><span data-stu-id="85516-385">Otherwise, if the state of *v* after *expr_first* is "definitely assigned after false expression", and the state of *v* after *expr_second* is "definitely assigned after false expression", then the state of *v* after *expr* is "definitely assigned after false expression".</span></span>
    * <span data-ttu-id="85516-386">否則，在*expr*之後的*v*狀態不會明確指派。</span><span class="sxs-lookup"><span data-stu-id="85516-386">Otherwise, the state of *v* after *expr* is not definitely assigned.</span></span>

<span data-ttu-id="85516-387">在範例中</span><span class="sxs-lookup"><span data-stu-id="85516-387">In the example</span></span>
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
<span data-ttu-id="85516-388">系統會`i`將變數視為明確指派給`if`語句的其中一個內嵌語句，而不是另一個。</span><span class="sxs-lookup"><span data-stu-id="85516-388">the variable `i` is considered definitely assigned in one of the embedded statements of an `if` statement but not in the other.</span></span> <span data-ttu-id="85516-389">在方法`if` `F`的語句中，變數`i`是在第一個內嵌語句中明確指派的，因為運算式`(i = y)`的執行一律會在此內嵌語句的執行前面。</span><span class="sxs-lookup"><span data-stu-id="85516-389">In the `if` statement in method `F`, the variable `i` is definitely assigned in the first embedded statement because execution of the expression `(i = y)` always precedes execution of this embedded statement.</span></span> <span data-ttu-id="85516-390">相反地，變數`i`不會在第二個內嵌語句中明確指派，因為`x >= 0`可能已測試 false，導致變數`i`被取消指派。</span><span class="sxs-lookup"><span data-stu-id="85516-390">In contrast, the variable `i` is not definitely assigned in the second embedded statement, since `x >= 0` might have tested false, resulting in the variable `i` being unassigned.</span></span>

#### <a name="-conditional-or-expressions"></a><span data-ttu-id="85516-391">||（條件式或）運算式</span><span class="sxs-lookup"><span data-stu-id="85516-391">|| (conditional OR) expressions</span></span>

<span data-ttu-id="85516-392">針對格式`expr_first || expr_second`為的運算式*expr* ：</span><span class="sxs-lookup"><span data-stu-id="85516-392">For an expression *expr* of the form `expr_first || expr_second`:</span></span>

*  <span data-ttu-id="85516-393">*V*在*expr_first*之前的明確指派狀態，與*expr\*\*前面的*明確指派狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-393">The definite assignment state of *v* before *expr_first* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="85516-394">如果在*expr_first*之後的*v*狀態已明確指派，或「在 false 運算式之後明確指派」，則在*expr_second*之前的明確指派狀態是明確*指派的。*</span><span class="sxs-lookup"><span data-stu-id="85516-394">The definite assignment state of *v* before *expr_second* is definitely assigned if the state of *v* after *expr_first* is either definitely assigned or "definitely assigned after false expression".</span></span> <span data-ttu-id="85516-395">否則，它不會被明確指派。</span><span class="sxs-lookup"><span data-stu-id="85516-395">Otherwise, it is not definitely assigned.</span></span>
*  <span data-ttu-id="85516-396">*V* after*運算式*的明確指派語句取決於：</span><span class="sxs-lookup"><span data-stu-id="85516-396">The definite assignment statement of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="85516-397">如果*expr_first*是具有值`true`的常數運算式，則*v* after *expr*的明確指派狀態會與*expr_first*之後的*v*明確指派狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-397">If *expr_first* is a constant expression with the value `true`, then the definite assignment state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_first*.</span></span>
    * <span data-ttu-id="85516-398">否則，如果已明確指派*expr_first*後*v*的狀態，則會將*v*的狀態明確指派給*expr* 。</span><span class="sxs-lookup"><span data-stu-id="85516-398">Otherwise, if the state of *v* after *expr_first* is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="85516-399">否則，如果已明確指派*expr_second*後*v*的狀態，且*expr_first*之後的*v*狀態是「在 true 運算式之後明確指派」，則*v*的狀態一定會是*expr*指派.</span><span class="sxs-lookup"><span data-stu-id="85516-399">Otherwise, if the state of *v* after *expr_second* is definitely assigned, and the state of *v* after *expr_first* is "definitely assigned after true expression", then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="85516-400">否則，如果已明確指派*expr_second*之後的*v*狀態，或「在 false 運算式之後明確指派」，則*v* after *expr*後面的狀態會是「在 false 運算式之後明確指派」。</span><span class="sxs-lookup"><span data-stu-id="85516-400">Otherwise, if the state of *v* after *expr_second* is definitely assigned or "definitely assigned after false expression", then the state of *v* after *expr* is "definitely assigned after false expression".</span></span>
    * <span data-ttu-id="85516-401">否則，如果*expr_first*後*v*的狀態是「在 true 運算式之後明確指派」，而且*expr_second*之後的*v*狀態是「在 true 運算式之後明確指派」，則*v*的狀態會在*expr 之後*是「在 true 運算式之後明確指派」。</span><span class="sxs-lookup"><span data-stu-id="85516-401">Otherwise, if the state of *v* after *expr_first* is "definitely assigned after true expression", and the state of *v* after *expr_second* is "definitely assigned after true expression", then the state of *v* after *expr* is "definitely assigned after true expression".</span></span>
    * <span data-ttu-id="85516-402">否則，在*expr*之後的*v*狀態不會明確指派。</span><span class="sxs-lookup"><span data-stu-id="85516-402">Otherwise, the state of *v* after *expr* is not definitely assigned.</span></span>

<span data-ttu-id="85516-403">在範例中</span><span class="sxs-lookup"><span data-stu-id="85516-403">In the example</span></span>
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
<span data-ttu-id="85516-404">系統會`i`將變數視為明確指派給`if`語句的其中一個內嵌語句，而不是另一個。</span><span class="sxs-lookup"><span data-stu-id="85516-404">the variable `i` is considered definitely assigned in one of the embedded statements of an `if` statement but not in the other.</span></span> <span data-ttu-id="85516-405">在方法`if` `G`的語句中，變數`i`是在第二個內嵌的語句中明確指派的，因為`(i = y)`運算式的執行一律優先于此內嵌語句的執行。</span><span class="sxs-lookup"><span data-stu-id="85516-405">In the `if` statement in method `G`, the variable `i` is definitely assigned in the second embedded statement because execution of the expression `(i = y)` always precedes execution of this embedded statement.</span></span> <span data-ttu-id="85516-406">相反地，變數`i`不會在第一個內嵌語句中明確指派，因為`x >= 0`可能已測試過 true，導致變數`i`被取消指派。</span><span class="sxs-lookup"><span data-stu-id="85516-406">In contrast, the variable `i` is not definitely assigned in the first embedded statement, since `x >= 0` might have tested true, resulting in the variable `i` being unassigned.</span></span>

#### <a name="-logical-negation-expressions"></a><span data-ttu-id="85516-407">!</span><span class="sxs-lookup"><span data-stu-id="85516-407">!</span></span> <span data-ttu-id="85516-408">（邏輯否定）運算式</span><span class="sxs-lookup"><span data-stu-id="85516-408">(logical negation) expressions</span></span>

<span data-ttu-id="85516-409">針對格式`! expr_operand`為的運算式*expr* ：</span><span class="sxs-lookup"><span data-stu-id="85516-409">For an expression *expr* of the form `! expr_operand`:</span></span>

*  <span data-ttu-id="85516-410">*V*在*expr_operand*之前的明確指派狀態，與*expr\*\*前面的*明確指派狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-410">The definite assignment state of *v* before *expr_operand* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="85516-411">*V* after *expr*之後的明確指派狀態取決於：</span><span class="sxs-lookup"><span data-stu-id="85516-411">The definite assignment state of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="85516-412">如果 \* expr_operand \* 之後的*v*狀態是明確指派的，則會將*v*的狀態明確指派給*expr* after。</span><span class="sxs-lookup"><span data-stu-id="85516-412">If the state of *v* after \*expr_operand \*is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="85516-413">如果 \* expr_operand \* 之後的*v*狀態未明確指派，則不會明確指派*v* after *expr*的狀態。</span><span class="sxs-lookup"><span data-stu-id="85516-413">If the state of *v* after \*expr_operand \*is not definitely assigned, then the state of *v* after *expr* is not definitely assigned.</span></span>
    * <span data-ttu-id="85516-414">如果 \* expr_operand \* 之後的*v*狀態是「在 false 運算式之後明確指派」，則*v* after *expr*後面的狀態會是「在 true 運算式之後明確指派」。</span><span class="sxs-lookup"><span data-stu-id="85516-414">If the state of *v* after \*expr_operand \*is "definitely assigned after false expression", then the state of *v* after *expr* is "definitely assigned after true expression".</span></span>
    * <span data-ttu-id="85516-415">如果 \* expr_operand \* 後面的*v*狀態是「在 true 運算式之後明確指派」，則*v* after *expr*後面的狀態會是「在 false 運算式之後明確指派」。</span><span class="sxs-lookup"><span data-stu-id="85516-415">If the state of *v* after \*expr_operand \*is "definitely assigned after true expression", then the state of *v* after *expr* is "definitely assigned after false expression".</span></span>

#### <a name="-null-coalescing-expressions"></a><span data-ttu-id="85516-416">??</span><span class="sxs-lookup"><span data-stu-id="85516-416">??</span></span> <span data-ttu-id="85516-417">（null 聯合）運算式</span><span class="sxs-lookup"><span data-stu-id="85516-417">(null coalescing) expressions</span></span>

<span data-ttu-id="85516-418">針對格式`expr_first ?? expr_second`為的運算式*expr* ：</span><span class="sxs-lookup"><span data-stu-id="85516-418">For an expression *expr* of the form `expr_first ?? expr_second`:</span></span>

*  <span data-ttu-id="85516-419">*V*在*expr_first*之前的明確指派狀態，與*expr\*\*前面的*明確指派狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-419">The definite assignment state of *v* before *expr_first* is the same as the definite assignment state of *v* before *expr*.</span></span>
*  <span data-ttu-id="85516-420">*V*在*expr_second*之前的明確指派狀態，與*expr_first*之後的*v*明確指派狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-420">The definite assignment state of *v* before *expr_second* is the same as the definite assignment state of *v* after *expr_first*.</span></span>
*  <span data-ttu-id="85516-421">*V* after*運算式*的明確指派語句取決於：</span><span class="sxs-lookup"><span data-stu-id="85516-421">The definite assignment statement of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="85516-422">如果*expr_first*是具有值 null 的常數運算式（[常數運算式](expressions.md#constant-expressions)），則*v* after *expr*的狀態會與*expr_second*之後的*v*狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-422">If *expr_first* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) with value null, then the state of *v* after *expr* is the same as the state of *v* after *expr_second*.</span></span>
*  <span data-ttu-id="85516-423">否則， *v* after *expr*的狀態會與*expr_first*之後之*v*的明確指派狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-423">Otherwise, the state of *v* after *expr* is the same as the definite assignment state of *v* after *expr_first*.</span></span>

#### <a name="-conditional-expressions"></a><span data-ttu-id="85516-424">？：（條件式）運算式</span><span class="sxs-lookup"><span data-stu-id="85516-424">?: (conditional) expressions</span></span>

<span data-ttu-id="85516-425">針對格式`expr_cond ? expr_true : expr_false`為的運算式*expr* ：</span><span class="sxs-lookup"><span data-stu-id="85516-425">For an expression *expr* of the form `expr_cond ? expr_true : expr_false`:</span></span>

*  <span data-ttu-id="85516-426">*V*在*expr_cond*之前的明確指派狀態，與*expr*之前的*v*狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-426">The definite assignment state of *v* before *expr_cond* is the same as the state of *v* before *expr*.</span></span>
*  <span data-ttu-id="85516-427">只有在下列其中一項保留時，才會明確指派*expr_true* *的明確*指派狀態：</span><span class="sxs-lookup"><span data-stu-id="85516-427">The definite assignment state of *v* before *expr_true* is definitely assigned if and only if one of the following holds:</span></span>
    * <span data-ttu-id="85516-428">*expr_cond*是具有值的常數運算式`false`</span><span class="sxs-lookup"><span data-stu-id="85516-428">*expr_cond* is a constant expression with the value `false`</span></span>
    * <span data-ttu-id="85516-429">*expr_cond*之後的*v*狀態是明確指派的，或「在 true 運算式之後明確指派」。</span><span class="sxs-lookup"><span data-stu-id="85516-429">the state of *v* after *expr_cond* is definitely assigned or "definitely assigned after true expression".</span></span>
*  <span data-ttu-id="85516-430">只有在下列其中一項保留時，才會明確指派*expr_false* *的明確*指派狀態：</span><span class="sxs-lookup"><span data-stu-id="85516-430">The definite assignment state of *v* before *expr_false* is definitely assigned if and only if one of the following holds:</span></span>
    * <span data-ttu-id="85516-431">*expr_cond*是具有值的常數運算式`true`</span><span class="sxs-lookup"><span data-stu-id="85516-431">*expr_cond* is a constant expression with the value `true`</span></span>
*  <span data-ttu-id="85516-432">*expr_cond*之後的*v*狀態是明確指派的，或「在 false 運算式之後明確指派」。</span><span class="sxs-lookup"><span data-stu-id="85516-432">the state of *v* after *expr_cond* is definitely assigned or "definitely assigned after false expression".</span></span>
*  <span data-ttu-id="85516-433">*V* after *expr*之後的明確指派狀態取決於：</span><span class="sxs-lookup"><span data-stu-id="85516-433">The definite assignment state of *v* after *expr* is determined by:</span></span>
    * <span data-ttu-id="85516-434">如果*expr_cond*是具有值`true`的常數運算式（[常數運算式](expressions.md#constant-expressions)），則*v* after *expr*的狀態會與*expr_true*之後的*v*狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-434">If *expr_cond* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) with value `true` then the state of *v* after *expr* is the same as the state of *v* after *expr_true*.</span></span>
    * <span data-ttu-id="85516-435">否則，如果*expr_cond*是具有值`false`的常數運算式（[常數運算式](expressions.md#constant-expressions)），則*v* after *expr*的狀態會與*expr_false*之後的*v*狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-435">Otherwise, if *expr_cond* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) with value `false` then the state of *v* after *expr* is the same as the state of *v* after *expr_false*.</span></span>
    * <span data-ttu-id="85516-436">否則，如果已明確指派*expr_true*後*v*的狀態，且明確指派*expr_false* *之後的 v 狀態*，則會明確指派*v* after *expr*的狀態。</span><span class="sxs-lookup"><span data-stu-id="85516-436">Otherwise, if the state of *v* after *expr_true* is definitely assigned and the state of *v* after *expr_false* is definitely assigned, then the state of *v* after *expr* is definitely assigned.</span></span>
    * <span data-ttu-id="85516-437">否則，在*expr*之後的*v*狀態不會明確指派。</span><span class="sxs-lookup"><span data-stu-id="85516-437">Otherwise, the state of *v* after *expr* is not definitely assigned.</span></span>

#### <a name="anonymous-functions"></a><span data-ttu-id="85516-438">匿名函式</span><span class="sxs-lookup"><span data-stu-id="85516-438">Anonymous functions</span></span>

<span data-ttu-id="85516-439">針對具有主體（*區塊*或*運算式*）*主體*的*lambda_expression*或*anonymous_method_expression* *expr* ：</span><span class="sxs-lookup"><span data-stu-id="85516-439">For a *lambda_expression* or *anonymous_method_expression* *expr* with a body (either *block* or *expression*) *body*:</span></span>

*  <span data-ttu-id="85516-440">外部變數*v*在*主體*之前的明確指派狀態，與*expr*之前的*v*狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-440">The definite assignment state of an outer variable *v* before *body* is the same as the state of *v* before *expr*.</span></span> <span data-ttu-id="85516-441">也就是說，外部變數的明確指派狀態是繼承自匿名函式的內容。</span><span class="sxs-lookup"><span data-stu-id="85516-441">That is, definite assignment state of outer variables is inherited from the context of the anonymous function.</span></span>
*  <span data-ttu-id="85516-442">外部變數*v*在*expr*之後的明確指派狀態，與*expr*之前的*v*狀態相同。</span><span class="sxs-lookup"><span data-stu-id="85516-442">The definite assignment state of an outer variable *v* after *expr* is the same as the state of *v* before *expr*.</span></span>

<span data-ttu-id="85516-443">範例</span><span class="sxs-lookup"><span data-stu-id="85516-443">The example</span></span>
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
<span data-ttu-id="85516-444">會產生編譯時期錯誤，因為`max`在宣告匿名函式的情況下，並未明確指派。</span><span class="sxs-lookup"><span data-stu-id="85516-444">generates a compile-time error since `max` is not definitely assigned where the anonymous function is declared.</span></span> <span data-ttu-id="85516-445">範例</span><span class="sxs-lookup"><span data-stu-id="85516-445">The example</span></span>
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
<span data-ttu-id="85516-446">也會產生編譯時期錯誤，因為在匿名函`n`式中的指派不會影響匿名函式`n`外部的明確指派狀態。</span><span class="sxs-lookup"><span data-stu-id="85516-446">also generates a compile-time error since the assignment to `n` in the anonymous function has no affect on the definite assignment state of `n` outside the anonymous function.</span></span>

## <a name="variable-references"></a><span data-ttu-id="85516-447">變數參考</span><span class="sxs-lookup"><span data-stu-id="85516-447">Variable references</span></span>

<span data-ttu-id="85516-448">*Variable_reference*是分類為變數的*運算式*。</span><span class="sxs-lookup"><span data-stu-id="85516-448">A *variable_reference* is an *expression* that is classified as a variable.</span></span> <span data-ttu-id="85516-449">*Variable_reference*代表可以存取的儲存位置，以提取目前的值並儲存新的值。</span><span class="sxs-lookup"><span data-stu-id="85516-449">A *variable_reference* denotes a storage location that can be accessed both to fetch the current value and to store a new value.</span></span>

```antlr
variable_reference
    : expression
    ;
```

<span data-ttu-id="85516-450">在 C 和C++中， *variable_reference*稱為*左*值。</span><span class="sxs-lookup"><span data-stu-id="85516-450">In C and C++, a *variable_reference* is known as an *lvalue*.</span></span>

## <a name="atomicity-of-variable-references"></a><span data-ttu-id="85516-451">變數參考的不可部分完成性</span><span class="sxs-lookup"><span data-stu-id="85516-451">Atomicity of variable references</span></span>

<span data-ttu-id="85516-452">下列資料類型的讀取和寫入是不可部分完成`bool`的`char`： `byte`、 `sbyte` `uint`、 `short`、 `ushort`、、 `int` `float`、、、和參考型別。</span><span class="sxs-lookup"><span data-stu-id="85516-452">Reads and writes of the following data types are atomic: `bool`, `char`, `byte`, `sbyte`, `short`, `ushort`, `uint`, `int`, `float`, and reference types.</span></span> <span data-ttu-id="85516-453">此外，在先前清單中具有基礎類型的列舉類型的讀取和寫入也是不可部分完成的。</span><span class="sxs-lookup"><span data-stu-id="85516-453">In addition, reads and writes of enum types with an underlying type in the previous list are also atomic.</span></span> <span data-ttu-id="85516-454">其他類型的讀取和寫入，包括`long`、 `ulong`、 `double`和`decimal`，以及使用者定義類型，則不保證是不可部分完成的。</span><span class="sxs-lookup"><span data-stu-id="85516-454">Reads and writes of other types, including `long`, `ulong`, `double`, and `decimal`, as well as user-defined types, are not guaranteed to be atomic.</span></span> <span data-ttu-id="85516-455">除了針對該目的而設計的程式庫函數之外，並不保證不可部分完成的讀取-修改-寫入，例如，在遞增或遞減的情況下。</span><span class="sxs-lookup"><span data-stu-id="85516-455">Aside from the library functions designed for that purpose, there is no guarantee of atomic read-modify-write, such as in the case of increment or decrement.</span></span>

