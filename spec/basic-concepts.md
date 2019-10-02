---
ms.openlocfilehash: ff31585520c9090ad92893a930327112743c8e77
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704011"
---
# <a name="basic-concepts"></a><span data-ttu-id="a4c5c-101">基本概念</span><span class="sxs-lookup"><span data-stu-id="a4c5c-101">Basic concepts</span></span>

## <a name="application-startup"></a><span data-ttu-id="a4c5c-102">應用程式啟動</span><span class="sxs-lookup"><span data-stu-id="a4c5c-102">Application Startup</span></span>

<span data-ttu-id="a4c5c-103">具有***進入點***的元件稱為「***應用程式***」。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-103">An assembly that has an ***entry point*** is called an ***application***.</span></span> <span data-ttu-id="a4c5c-104">當應用程式執行時，會建立新的***應用程式域***。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-104">When an application is run, a new ***application domain*** is created.</span></span> <span data-ttu-id="a4c5c-105">同一部電腦上可能同時存在數個不同的應用程式具現化，而且每個都有自己的應用程式域。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-105">Several different instantiations of an application may exist on the same machine at the same time, and each has its own application domain.</span></span>

<span data-ttu-id="a4c5c-106">應用程式域會作為應用程式狀態的容器，藉此啟用應用程式隔離。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-106">An application domain enables application isolation by acting as a container for application state.</span></span> <span data-ttu-id="a4c5c-107">應用程式域會作為應用程式中所定義之型別的容器和界限，以及它所使用的類別庫。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-107">An application domain acts as a container and boundary for the types defined in the application and the class libraries it uses.</span></span> <span data-ttu-id="a4c5c-108">載入至一個應用程式域的類型與載入另一個應用程式域中的相同類型不同，而且在應用程式域之間不會直接共用物件的實例。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-108">Types loaded into one application domain are distinct from the same type loaded into another application domain, and instances of objects are not directly shared between application domains.</span></span> <span data-ttu-id="a4c5c-109">例如，每個應用程式域都有自己的靜態變數複本，適用于這些類型，而且針對每個應用程式域，最多可執行一次類型的靜態函式。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-109">For instance, each application domain has its own copy of static variables for these types, and a static constructor for a type is run at most once per application domain.</span></span> <span data-ttu-id="a4c5c-110">建立和終結應用程式域的執行是免費的。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-110">Implementations are free to provide implementation-specific policy or mechanisms for the creation and destruction of application domains.</span></span>

<span data-ttu-id="a4c5c-111">當執行環境呼叫指定的方法（稱為應用程式的進入點）時，就會***啟動應用程式***。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-111">***Application startup*** occurs when the execution environment calls a designated method, which is referred to as the application's entry point.</span></span> <span data-ttu-id="a4c5c-112">這個進入點方法一律會命名為 `Main`，而且可以有下列其中一個簽章：</span><span class="sxs-lookup"><span data-stu-id="a4c5c-112">This entry point method is always named `Main`, and can have one of the following signatures:</span></span>

```csharp
static void Main() {...}

static void Main(string[] args) {...}

static int Main() {...}

static int Main(string[] args) {...}
```

<span data-ttu-id="a4c5c-113">如圖所示，進入點可能會選擇性地傳回 `int` 值。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-113">As shown, the entry point may optionally return an `int` value.</span></span> <span data-ttu-id="a4c5c-114">此傳回值會用於應用程式終止（[應用程式終止](basic-concepts.md#application-termination)）。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-114">This return value is used in application termination ([Application termination](basic-concepts.md#application-termination)).</span></span>

<span data-ttu-id="a4c5c-115">進入點可能會選擇性地具有一個型式參數。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-115">The entry point may optionally have one formal parameter.</span></span> <span data-ttu-id="a4c5c-116">參數可能會有任何名稱，但參數的類型必須 `string[]`。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-116">The parameter may have any name, but the type of the parameter must be `string[]`.</span></span> <span data-ttu-id="a4c5c-117">如果正式參數存在，執行環境會建立並傳遞 `string[]` 引數，其中包含啟動應用程式時所指定的命令列引數。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-117">If the formal parameter is present, the execution environment creates and passes a `string[]` argument containing the command-line arguments that were specified when the application was started.</span></span> <span data-ttu-id="a4c5c-118">@No__t-0 引數永遠不會是 null，但如果未指定任何命令列引數，則其長度可能為零。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-118">The `string[]` argument is never null, but it may have a length of zero if no command-line arguments were specified.</span></span>

<span data-ttu-id="a4c5c-119">由於C#支援方法多載，因此類別或結構可能包含某個方法的多個定義，前提是每個都有不同的簽章。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-119">Since C# supports method overloading, a class or struct may contain multiple definitions of some method, provided each has a different signature.</span></span> <span data-ttu-id="a4c5c-120">不過，在單一程式中，沒有任何類別或結構可以包含一個以上的方法，稱為 `Main`，其定義會限定它做為應用程式進入點使用。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-120">However, within a single program, no class or struct may contain more than one method called `Main` whose definition qualifies it to be used as an application entry point.</span></span> <span data-ttu-id="a4c5c-121">不過，如果其他多載版本的 `Main`，則會允許它們有一個以上的參數，或其唯一的參數不是類型 `string[]`。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-121">Other overloaded versions of `Main` are permitted, however, provided they have more than one parameter, or their only parameter is other than type `string[]`.</span></span>

<span data-ttu-id="a4c5c-122">應用程式可由多個類別或結構組成。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-122">An application can be made up of multiple classes or structs.</span></span> <span data-ttu-id="a4c5c-123">其中有多個類別或結構可以包含名為 `Main` 的方法，其定義會限定其做為應用程式進入點使用。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-123">It is possible for more than one of these classes or structs to contain a method called `Main` whose definition qualifies it to be used as an application entry point.</span></span> <span data-ttu-id="a4c5c-124">在這種情況下，必須使用外部機制（例如命令列編譯器選項）來選取其中一個 `Main` 方法做為進入點。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-124">In such cases, an external mechanism (such as a command-line compiler option) must be used to select one of these `Main` methods as the entry point.</span></span>

<span data-ttu-id="a4c5c-125">在C#中，每個方法都必須定義為類別或結構的成員。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-125">In C#, every method must be defined as a member of a class or struct.</span></span> <span data-ttu-id="a4c5c-126">一般來說，方法的宣告存取範圍（宣告[存取](basic-concepts.md#declared-accessibility)子）是由其宣告中指定的存取修飾詞（[存取](classes.md#access-modifiers)修飾詞）所決定，而類型的宣告存取範圍則是由在其宣告中指定的存取修飾詞。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-126">Ordinarily, the declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)) of a method is determined by the access modifiers ([Access modifiers](classes.md#access-modifiers)) specified in its declaration, and similarly the declared accessibility of a type is determined by the access modifiers specified in its declaration.</span></span> <span data-ttu-id="a4c5c-127">為了讓給定類型的指定方法可供呼叫，類型和成員都必須是可存取的。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-127">In order for a given method of a given type to be callable, both the type and the member must be accessible.</span></span> <span data-ttu-id="a4c5c-128">不過，應用程式進入點是特殊案例。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-128">However, the application entry point is a special case.</span></span> <span data-ttu-id="a4c5c-129">具體而言，執行環境可以存取應用程式的進入點，不論其已宣告的存取範圍為何，不論其封入類型宣告的宣告存取範圍為何。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-129">Specifically, the execution environment can access the application's entry point regardless of its declared accessibility and regardless of the declared accessibility of its enclosing type declarations.</span></span>

<span data-ttu-id="a4c5c-130">應用程式進入點方法可能不在泛型類別宣告中。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-130">The application entry point method may not be in a generic class declaration.</span></span>

<span data-ttu-id="a4c5c-131">在所有其他方面，進入點方法的行為就像不是進入點。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-131">In all other respects, entry point methods behave like those that are not entry points.</span></span>

## <a name="application-termination"></a><span data-ttu-id="a4c5c-132">應用程式終止</span><span class="sxs-lookup"><span data-stu-id="a4c5c-132">Application termination</span></span>

<span data-ttu-id="a4c5c-133">***應用程式終止***會將控制權傳回給執行環境。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-133">***Application termination*** returns control to the execution environment.</span></span>

<span data-ttu-id="a4c5c-134">如果應用程式***進入點***方法的傳回型別是 `int`，則傳回的值會作為應用程式的***終止狀態碼***。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-134">If the return type of the application's ***entry point*** method is `int`, the value returned serves as the application's ***termination status code***.</span></span> <span data-ttu-id="a4c5c-135">此程式碼的目的是要允許成功或失敗到執行環境的通訊。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-135">The purpose of this code is to allow communication of success or failure to the execution environment.</span></span>

<span data-ttu-id="a4c5c-136">如果進入點方法的傳回型別為 `void`，而到達用來終止該方法的右大括弧（`}`），或執行沒有運算式的 @no__t 2 語句，則會產生終止狀態碼 `0`。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-136">If the return type of the entry point method is `void`, reaching the right brace (`}`) which terminates that method, or executing a `return` statement that has no expression, results in a termination status code of `0`.</span></span>

<span data-ttu-id="a4c5c-137">在應用程式終止之前，會呼叫其所有尚未進行垃圾收集之物件的析構函數，除非已隱藏這類清除（例如，藉由呼叫程式庫方法 `GC.SuppressFinalize`）。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-137">Prior to an application's termination, destructors for all of its objects that have not yet been garbage collected are called, unless such cleanup has been suppressed (by a call to the library method `GC.SuppressFinalize`, for example).</span></span>

## <a name="declarations"></a><span data-ttu-id="a4c5c-138">宣告</span><span class="sxs-lookup"><span data-stu-id="a4c5c-138">Declarations</span></span>

<span data-ttu-id="a4c5c-139">C#程式中的宣告會定義程式的組成元素。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-139">Declarations in a C# program define the constituent elements of the program.</span></span> <span data-ttu-id="a4c5c-140">C#程式是使用命名空間（[命名](namespaces.md)空間）來組織，其中可以包含型別宣告和嵌套的命名空間宣告。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-140">C# programs are organized using namespaces ([Namespaces](namespaces.md)), which can contain type declarations and nested namespace declarations.</span></span> <span data-ttu-id="a4c5c-141">類型宣告（[類型](namespaces.md#type-declarations)宣告）是用來定義類別（[類別](classes.md)）、結構（[結構](structs.md)）、介面（[介面](interfaces.md)）、[列舉（列舉](enums.md)）和委派（[委派](delegates.md)）。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-141">Type declarations ([Type declarations](namespaces.md#type-declarations)) are used to define classes ([Classes](classes.md)), structs ([Structs](structs.md)), interfaces ([Interfaces](interfaces.md)), enums ([Enums](enums.md)), and delegates ([Delegates](delegates.md)).</span></span> <span data-ttu-id="a4c5c-142">類型宣告中允許的成員類型，取決於類型宣告的形式。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-142">The kinds of members permitted in a type declaration depend on the form of the type declaration.</span></span> <span data-ttu-id="a4c5c-143">例如，類別宣告可以包含常數（[常數](classes.md#constants)）、欄位（[欄位](classes.md#fields)）、方法（[方法](classes.md#methods)）、屬性（[屬性](classes.md#properties)）、事件（[事件](classes.md#events)）、索引子（[索引子](classes.md#indexers)）的宣告、運算子（[運算子](classes.md#operators)）、實例的構造函式（[實例](classes.md#instance-constructors)的函式）、靜態的函式（[靜態](classes.md#static-constructors)的函式）、析構函式（[析構](classes.md#destructors)函式）和巢狀型別（[巢狀型別](classes.md#nested-types)）。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-143">For instance, class declarations can contain declarations for constants ([Constants](classes.md#constants)), fields ([Fields](classes.md#fields)), methods ([Methods](classes.md#methods)), properties ([Properties](classes.md#properties)), events ([Events](classes.md#events)), indexers ([Indexers](classes.md#indexers)), operators ([Operators](classes.md#operators)), instance constructors ([Instance constructors](classes.md#instance-constructors)), static constructors ([Static constructors](classes.md#static-constructors)), destructors ([Destructors](classes.md#destructors)), and nested types ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="a4c5c-144">宣告會在宣告所屬的宣告***空間***中定義名稱。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-144">A declaration defines a name in the ***declaration space*** to which the declaration belongs.</span></span> <span data-ttu-id="a4c5c-145">除了多載成員（簽章[和](basic-concepts.md#signatures-and-overloading)多載）以外，有兩個或多個宣告會在宣告空間中引進具有相同名稱的成員，這是編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-145">Except for overloaded members ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)), it is a compile-time error to have two or more declarations that introduce members with the same name in a declaration space.</span></span> <span data-ttu-id="a4c5c-146">宣告空間永遠不可能包含具有相同名稱的不同類型成員。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-146">It is never possible for a declaration space to contain different kinds of members with the same name.</span></span> <span data-ttu-id="a4c5c-147">例如，宣告空間絕不能包含具有相同名稱的欄位和方法。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-147">For example, a declaration space can never contain a field and a method by the same name.</span></span>

<span data-ttu-id="a4c5c-148">有數種不同類型的宣告空間，如下所述。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-148">There are several different types of declaration spaces, as described in the following.</span></span>

*  <span data-ttu-id="a4c5c-149">在程式的所有原始程式檔中，沒有封入*namespace_declaration*的*namespace_member_declaration*不是單一合併宣告空間的成員，稱為***全域宣告空間***。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-149">Within all source files of a program, *namespace_member_declaration*s with no enclosing *namespace_declaration* are members of a single combined declaration space called the ***global declaration space***.</span></span>
*  <span data-ttu-id="a4c5c-150">在程式的所有原始程式檔中，在具有相同完整命名空間名稱的*namespace_declaration*內， *namespace_member_declaration*s 都是單一合併宣告空間的成員。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-150">Within all source files of a program, *namespace_member_declaration*s within *namespace_declaration*s that have the same fully qualified namespace name are members of a single combined declaration space.</span></span>
*  <span data-ttu-id="a4c5c-151">每個類別、結構或介面宣告都會建立新的宣告空間。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-151">Each class, struct, or interface declaration creates a new declaration space.</span></span> <span data-ttu-id="a4c5c-152">名稱會透過*class_member_declaration*s、 *struct_member_declaration*s、 *interface_member_declaration*s 或*type_parameter*s 引進此宣告空間。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-152">Names are introduced into this declaration space through *class_member_declaration*s, *struct_member_declaration*s, *interface_member_declaration*s, or *type_parameter*s.</span></span> <span data-ttu-id="a4c5c-153">除了多載實例的「函式宣告」和「靜態」的「函式宣告」以外，類別或結構不能包含與類別或結構同名的成員宣告。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-153">Except for overloaded instance constructor declarations and static constructor declarations, a class or struct cannot contain a member declaration with the same name as the class or struct.</span></span> <span data-ttu-id="a4c5c-154">類別、結構或介面允許多載方法和索引子的宣告。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-154">A class, struct, or interface permits the declaration of overloaded methods and indexers.</span></span> <span data-ttu-id="a4c5c-155">此外，類別或結構允許宣告多載實例的函式和運算子。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-155">Furthermore, a class or struct permits the declaration of overloaded instance constructors and operators.</span></span> <span data-ttu-id="a4c5c-156">例如，類別、結構或介面可能包含多個具有相同名稱的方法宣告，但前提是這些方法宣告在其簽章（簽章[和](basic-concepts.md#signatures-and-overloading)多載）中不同。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-156">For example, a class, struct, or interface may contain multiple method declarations with the same name, provided these method declarations differ in their signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)).</span></span> <span data-ttu-id="a4c5c-157">請注意，基類不會參與類別的宣告空間，而且基底介面不會參與介面的宣告空間。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-157">Note that base classes do not contribute to the declaration space of a class, and base interfaces do not contribute to the declaration space of an interface.</span></span> <span data-ttu-id="a4c5c-158">因此，可以使用衍生類別或介面，宣告與繼承成員同名的成員。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-158">Thus, a derived class or interface is allowed to declare a member with the same name as an inherited member.</span></span> <span data-ttu-id="a4c5c-159">這類成員稱為***隱藏***繼承的成員。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-159">Such a member is said to ***hide*** the inherited member.</span></span>
*  <span data-ttu-id="a4c5c-160">每個委派宣告都會建立新的宣告空間。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-160">Each delegate declaration creates a new declaration space.</span></span> <span data-ttu-id="a4c5c-161">名稱會透過型式參數（*fixed_parameter*s 和*parameter_array*s）和*type_parameter*，引進此宣告空間。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-161">Names are introduced into this declaration space through formal parameters (*fixed_parameter*s and *parameter_array*s) and *type_parameter*s.</span></span>
*  <span data-ttu-id="a4c5c-162">每個列舉宣告都會建立新的宣告空間。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-162">Each enumeration declaration creates a new declaration space.</span></span> <span data-ttu-id="a4c5c-163">名稱會透過*enum_member_declarations*引進到此宣告空間中。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-163">Names are introduced into this declaration space through *enum_member_declarations*.</span></span>
*  <span data-ttu-id="a4c5c-164">每個方法宣告、索引子宣告、運算子宣告、實例的「函式宣告」和「匿名函式」都會建立新的宣告空間，稱為***本機變數宣告空間***。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-164">Each method declaration, indexer declaration, operator declaration, instance constructor declaration and anonymous function creates a new declaration space called a ***local variable declaration space***.</span></span> <span data-ttu-id="a4c5c-165">名稱會透過型式參數（*fixed_parameter*s 和*parameter_array*s）和*type_parameter*，引進此宣告空間。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-165">Names are introduced into this declaration space through formal parameters (*fixed_parameter*s and *parameter_array*s) and *type_parameter*s.</span></span> <span data-ttu-id="a4c5c-166">函式成員或匿名函式的主體（如果有的話）會被視為在區域變數宣告空間內嵌套。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-166">The body of the function member or anonymous function, if any, is considered to be nested within the local variable declaration space.</span></span> <span data-ttu-id="a4c5c-167">區域變數宣告空間和嵌套的區域變數宣告空間會包含相同名稱的元素，這是錯誤。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-167">It is an error for a local variable declaration space and a nested local variable declaration space to contain elements with the same name.</span></span> <span data-ttu-id="a4c5c-168">因此，在嵌套的宣告空間內，不可能在封閉宣告空間中宣告與區域變數或常數同名的區域變數或常數。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-168">Thus, within a nested declaration space it is not possible to declare a local variable or constant with the same name as a local variable or constant in an enclosing declaration space.</span></span> <span data-ttu-id="a4c5c-169">兩個宣告空格可以包含具有相同名稱的專案，但前提是兩者的宣告空間都不包含另一個。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-169">It is possible for two declaration spaces to contain elements with the same name as long as neither declaration space contains the other.</span></span>
*  <span data-ttu-id="a4c5c-170">每個*區塊*或*switch_block* ，以及*for*、 *foreach*和*using*語句都會針對本機變數和本機常數建立區域變數宣告空間。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-170">Each *block* or *switch_block* , as well as a *for*, *foreach* and *using* statement, creates a local variable declaration space for local variables and local constants .</span></span> <span data-ttu-id="a4c5c-171">名稱會透過*local_variable_declaration*s 和*local_constant_declaration*s 引進此宣告空間。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-171">Names are introduced into this declaration space through *local_variable_declaration*s and *local_constant_declaration*s.</span></span> <span data-ttu-id="a4c5c-172">請注意，在函式成員或匿名函式的主體中發生的區塊，會嵌套在這些函式所宣告的區域變數宣告空間中，以做為其參數。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-172">Note that blocks that occur as or within the body of a function member or anonymous function are nested within the local variable declaration space declared by those functions for their parameters.</span></span> <span data-ttu-id="a4c5c-173">因此，有一個錯誤，例如具有本機變數的方法和具有相同名稱的參數。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-173">Thus it is an error to have e.g. a method with a local variable and a parameter of the same name.</span></span>
*  <span data-ttu-id="a4c5c-174">每個*區塊*或*switch_block*會為標籤建立個別的宣告空間。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-174">Each *block* or *switch_block* creates a separate declaration space for labels.</span></span> <span data-ttu-id="a4c5c-175">名稱會透過*labeled_statement*在此宣告空間中引進，而名稱則會透過*goto_statement*來參考。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-175">Names are introduced into this declaration space through *labeled_statement*s, and the names are referenced through *goto_statement*s.</span></span> <span data-ttu-id="a4c5c-176">區塊的***標籤宣告空間***包含任何嵌套區塊。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-176">The ***label declaration space*** of a block includes any nested blocks.</span></span> <span data-ttu-id="a4c5c-177">因此，在嵌套區塊內，不可能宣告與封閉區塊中的標籤同名的標籤。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-177">Thus, within a nested block it is not possible to declare a label with the same name as a label in an enclosing block.</span></span>

<span data-ttu-id="a4c5c-178">宣告名稱的文字順序通常沒有任何意義。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-178">The textual order in which names are declared is generally of no significance.</span></span> <span data-ttu-id="a4c5c-179">特別的是，文字順序對於宣告和使用命名空間、常數、方法、屬性、事件、索引子、運算子、實例函式、析構函式、靜態函式和類型而言並不重要。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-179">In particular, textual order is not significant for the declaration and use of namespaces, constants, methods, properties, events, indexers, operators, instance constructors, destructors, static constructors, and types.</span></span> <span data-ttu-id="a4c5c-180">宣告順序在下列方面很重要：</span><span class="sxs-lookup"><span data-stu-id="a4c5c-180">Declaration order is significant in the following ways:</span></span>

*  <span data-ttu-id="a4c5c-181">欄位宣告和區域變數宣告的宣告順序會決定其初始化運算式（如果有的話）的執行順序。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-181">Declaration order for field declarations and local variable declarations determines the order in which their initializers (if any) are executed.</span></span>
*  <span data-ttu-id="a4c5c-182">您必須先定義區域變數，才能使用這些變數（[範圍](basic-concepts.md#scopes)）。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-182">Local variables must be defined before they are used ([Scopes](basic-concepts.md#scopes)).</span></span>
*  <span data-ttu-id="a4c5c-183">省略*constant_expression*值時，列舉成員宣告（[列舉成員](enums.md#enum-members)）的宣告順序是很重要的。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-183">Declaration order for enum member declarations ([Enum members](enums.md#enum-members)) is significant when *constant_expression* values are omitted.</span></span>

<span data-ttu-id="a4c5c-184">命名空間的宣告空間是「開放式結束」，而兩個具有相同完整名稱的命名空間宣告會參與相同的宣告空間。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-184">The declaration space of a namespace is "open ended", and two namespace declarations with the same fully qualified name contribute to the same declaration space.</span></span> <span data-ttu-id="a4c5c-185">例如：</span><span class="sxs-lookup"><span data-stu-id="a4c5c-185">For example</span></span>
```csharp
namespace Megacorp.Data
{
    class Customer
    {
        ...
    }
}

namespace Megacorp.Data
{
    class Order
    {
        ...
    }
}
```

<span data-ttu-id="a4c5c-186">上述兩個命名空間宣告會構成相同的宣告空間，在此案例中，宣告兩個具有完整名稱 `Megacorp.Data.Customer` 和 `Megacorp.Data.Order` 的類別。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-186">The two namespace declarations above contribute to the same declaration space, in this case declaring two classes with the fully qualified names `Megacorp.Data.Customer` and `Megacorp.Data.Order`.</span></span> <span data-ttu-id="a4c5c-187">由於這兩個宣告會構成相同的宣告空間，因此如果每個宣告都包含具有相同名稱的類別宣告，則會導致編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-187">Because the two declarations contribute to the same declaration space, it would have caused a compile-time error if each contained a declaration of a class with the same name.</span></span>

<span data-ttu-id="a4c5c-188">如上面所指定，區塊的宣告空間包含任何嵌套區塊。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-188">As specified above, the declaration space of a block includes any nested blocks.</span></span> <span data-ttu-id="a4c5c-189">因此，在下列範例中，`F` 和 @no__t 1 方法會導致編譯時期錯誤，因為名稱 `i` 是在外部區塊中宣告，而且不能在內部區塊中重新宣告。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-189">Thus, in the following example, the `F` and `G` methods result in a compile-time error because the name `i` is declared in the outer block and cannot be redeclared in the inner block.</span></span> <span data-ttu-id="a4c5c-190">不過，`H` 和 @no__t 1 方法有效，因為兩個 `i` 會在個別的非嵌套區塊中宣告。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-190">However, the `H` and `I` methods are valid since the two `i`'s are declared in separate non-nested blocks.</span></span>

```csharp
class A
{
    void F() {
        int i = 0;
        if (true) {
            int i = 1;            
        }
    }

    void G() {
        if (true) {
            int i = 0;
        }
        int i = 1;                
    }

    void H() {
        if (true) {
            int i = 0;
        }
        if (true) {
            int i = 1;
        }
    }

    void I() {
        for (int i = 0; i < 10; i++)
            H();
        for (int i = 0; i < 10; i++)
            H();
    }
}
```

## <a name="members"></a><span data-ttu-id="a4c5c-191">成員</span><span class="sxs-lookup"><span data-stu-id="a4c5c-191">Members</span></span>

<span data-ttu-id="a4c5c-192">命名空間和類型具有***成員***。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-192">Namespaces and types have ***members***.</span></span> <span data-ttu-id="a4c5c-193">實體的成員通常是透過使用以實體的參考開頭的限定名稱，後面接著 "`.`" token，再加上成員名稱來提供。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-193">The members of an entity are generally available through the use of a qualified name that starts with a reference to the entity, followed by a "`.`" token, followed by the name of the member.</span></span>

<span data-ttu-id="a4c5c-194">類型的成員可以在類型宣告中宣告，或***繼承***自類型的基類。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-194">Members of a type are either declared in the type declaration or ***inherited*** from the base class of the type.</span></span> <span data-ttu-id="a4c5c-195">當型別繼承自基類時，基類的所有成員（實例函式除外、析構函式和靜態的函式）都會變成衍生型別的成員。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-195">When a type inherits from a base class, all members of the base class, except instance constructors, destructors and static constructors, become members of the derived type.</span></span> <span data-ttu-id="a4c5c-196">基類成員的宣告存取範圍不會控制是否繼承成員，而繼承會延伸至不是實例的函式、靜態的函數或析構函數的任何成員。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-196">The declared accessibility of a base class member does not control whether the member is inherited—inheritance extends to any member that isn't an instance constructor, static constructor, or destructor.</span></span> <span data-ttu-id="a4c5c-197">不過，繼承的成員可能無法在衍生型別中存取，因為它已宣告的存取範圍（宣告的[存取](basic-concepts.md#declared-accessibility)範圍），或因為型別本身的宣告隱藏了（[透過繼承隱藏](basic-concepts.md#hiding-through-inheritance)）。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-197">However, an inherited member may not be accessible in a derived type, either because of its declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)) or because it is hidden by a declaration in the type itself ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)).</span></span>

### <a name="namespace-members"></a><span data-ttu-id="a4c5c-198">命名空間成員</span><span class="sxs-lookup"><span data-stu-id="a4c5c-198">Namespace members</span></span>

<span data-ttu-id="a4c5c-199">沒有封閉式命名空間的命名空間和類型是***全域命名空間***的成員。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-199">Namespaces and types that have no enclosing namespace are members of the ***global namespace***.</span></span> <span data-ttu-id="a4c5c-200">這會直接對應到全域宣告空間中宣告的名稱。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-200">This corresponds directly to the names declared in the global declaration space.</span></span>

<span data-ttu-id="a4c5c-201">命名空間中宣告的命名空間和類型是該命名空間的成員。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-201">Namespaces and types declared within a namespace are members of that namespace.</span></span> <span data-ttu-id="a4c5c-202">這會直接對應至命名空間宣告空間中宣告的名稱。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-202">This corresponds directly to the names declared in the declaration space of the namespace.</span></span>

<span data-ttu-id="a4c5c-203">命名空間沒有存取限制。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-203">Namespaces have no access restrictions.</span></span> <span data-ttu-id="a4c5c-204">您不能宣告私用、受保護的或內部命名空間，而且命名空間名稱一律可公開存取。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-204">It is not possible to declare private, protected, or internal namespaces, and namespace names are always publicly accessible.</span></span>

### <a name="struct-members"></a><span data-ttu-id="a4c5c-205">結構成員</span><span class="sxs-lookup"><span data-stu-id="a4c5c-205">Struct members</span></span>

<span data-ttu-id="a4c5c-206">結構的成員是在結構中宣告的成員，以及從結構的直接基類繼承而來的成員 `System.ValueType` 和間接基類 `object`。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-206">The members of a struct are the members declared in the struct and the members inherited from the struct's direct base class `System.ValueType` and the indirect base class `object`.</span></span>

<span data-ttu-id="a4c5c-207">簡單類型的成員會直接對應至簡單類型所別名之結構類型的成員：</span><span class="sxs-lookup"><span data-stu-id="a4c5c-207">The members of a simple type correspond directly to the members of the struct type aliased by the simple type:</span></span>

*  <span data-ttu-id="a4c5c-208">@No__t-0 的成員是 `System.SByte` 結構的成員。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-208">The members of `sbyte` are the members of the `System.SByte` struct.</span></span>
*  <span data-ttu-id="a4c5c-209">@No__t-0 的成員是 `System.Byte` 結構的成員。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-209">The members of `byte` are the members of the `System.Byte` struct.</span></span>
*  <span data-ttu-id="a4c5c-210">@No__t-0 的成員是 `System.Int16` 結構的成員。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-210">The members of `short` are the members of the `System.Int16` struct.</span></span>
*  <span data-ttu-id="a4c5c-211">@No__t-0 的成員是 `System.UInt16` 結構的成員。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-211">The members of `ushort` are the members of the `System.UInt16` struct.</span></span>
*  <span data-ttu-id="a4c5c-212">@No__t-0 的成員是 `System.Int32` 結構的成員。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-212">The members of `int` are the members of the `System.Int32` struct.</span></span>
*  <span data-ttu-id="a4c5c-213">@No__t-0 的成員是 `System.UInt32` 結構的成員。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-213">The members of `uint` are the members of the `System.UInt32` struct.</span></span>
*  <span data-ttu-id="a4c5c-214">@No__t-0 的成員是 `System.Int64` 結構的成員。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-214">The members of `long` are the members of the `System.Int64` struct.</span></span>
*  <span data-ttu-id="a4c5c-215">@No__t-0 的成員是 `System.UInt64` 結構的成員。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-215">The members of `ulong` are the members of the `System.UInt64` struct.</span></span>
*  <span data-ttu-id="a4c5c-216">@No__t-0 的成員是 `System.Char` 結構的成員。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-216">The members of `char` are the members of the `System.Char` struct.</span></span>
*  <span data-ttu-id="a4c5c-217">@No__t-0 的成員是 `System.Single` 結構的成員。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-217">The members of `float` are the members of the `System.Single` struct.</span></span>
*  <span data-ttu-id="a4c5c-218">@No__t-0 的成員是 `System.Double` 結構的成員。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-218">The members of `double` are the members of the `System.Double` struct.</span></span>
*  <span data-ttu-id="a4c5c-219">@No__t-0 的成員是 `System.Decimal` 結構的成員。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-219">The members of `decimal` are the members of the `System.Decimal` struct.</span></span>
*  <span data-ttu-id="a4c5c-220">@No__t-0 的成員是 `System.Boolean` 結構的成員。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-220">The members of `bool` are the members of the `System.Boolean` struct.</span></span>

### <a name="enumeration-members"></a><span data-ttu-id="a4c5c-221">列舉成員</span><span class="sxs-lookup"><span data-stu-id="a4c5c-221">Enumeration members</span></span>

<span data-ttu-id="a4c5c-222">列舉的成員是列舉型別中所宣告的常數，以及從列舉的直接基類繼承而來的成員 `System.Enum` 和間接基類 `System.ValueType` 和 `object`。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-222">The members of an enumeration are the constants declared in the enumeration and the members inherited from the enumeration's direct base class `System.Enum` and the indirect base classes `System.ValueType` and `object`.</span></span>

### <a name="class-members"></a><span data-ttu-id="a4c5c-223">類別成員</span><span class="sxs-lookup"><span data-stu-id="a4c5c-223">Class members</span></span>

<span data-ttu-id="a4c5c-224">類別的成員是在類別中宣告的成員，以及繼承自基類的成員（除了沒有基類的類別 `object` 除外）。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-224">The members of a class are the members declared in the class and the members inherited from the base class (except for class `object` which has no base class).</span></span> <span data-ttu-id="a4c5c-225">繼承自基類的成員包含常數、欄位、方法、屬性、事件、索引子、運算子和基類的類型，但不包括實例的函式、析構函式和基類的靜態函式。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-225">The members inherited from the base class include the constants, fields, methods, properties, events, indexers, operators, and types of the base class, but not the instance constructors, destructors and static constructors of the base class.</span></span> <span data-ttu-id="a4c5c-226">繼承基類成員，而不考慮其存取範圍。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-226">Base class members are inherited without regard to their accessibility.</span></span>

<span data-ttu-id="a4c5c-227">類別宣告可能包含常數、欄位、方法、屬性、事件、索引子、運算子、實例函式、析構函數、靜態函式和類型的宣告。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-227">A class declaration may contain declarations of constants, fields, methods, properties, events, indexers, operators, instance constructors, destructors, static constructors and types.</span></span>

<span data-ttu-id="a4c5c-228">@No__t-0 和 `string` 的成員直接對應至其別名的類別類型成員：</span><span class="sxs-lookup"><span data-stu-id="a4c5c-228">The members of `object` and `string` correspond directly to the members of the class types they alias:</span></span>

*  <span data-ttu-id="a4c5c-229">@No__t-0 的成員是 `System.Object` 類別的成員。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-229">The members of `object` are the members of the `System.Object` class.</span></span>
*  <span data-ttu-id="a4c5c-230">@No__t-0 的成員是 `System.String` 類別的成員。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-230">The members of `string` are the members of the `System.String` class.</span></span>

### <a name="interface-members"></a><span data-ttu-id="a4c5c-231">介面成員</span><span class="sxs-lookup"><span data-stu-id="a4c5c-231">Interface members</span></span>

<span data-ttu-id="a4c5c-232">介面的成員是介面中宣告的成員，以及介面的所有基底介面。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-232">The members of an interface are the members declared in the interface and in all base interfaces of the interface.</span></span> <span data-ttu-id="a4c5c-233">類別中的成員 `object` 不是嚴格的說，就是任何介面（[介面成員](interfaces.md#interface-members)）的成員。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-233">The members in class `object` are not, strictly speaking, members of any interface ([Interface members](interfaces.md#interface-members)).</span></span> <span data-ttu-id="a4c5c-234">不過，可以透過任何介面類別型（[成員查閱](expressions.md#member-lookup)）中的成員查閱，取得類別中 `object` 的成員。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-234">However, the members in class `object` are available via member lookup in any interface type ([Member lookup](expressions.md#member-lookup)).</span></span>

### <a name="array-members"></a><span data-ttu-id="a4c5c-235">陣列成員</span><span class="sxs-lookup"><span data-stu-id="a4c5c-235">Array members</span></span>

<span data-ttu-id="a4c5c-236">陣列的成員是繼承自類別 `System.Array` 的成員。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-236">The members of an array are the members inherited from class `System.Array`.</span></span>

### <a name="delegate-members"></a><span data-ttu-id="a4c5c-237">委派成員</span><span class="sxs-lookup"><span data-stu-id="a4c5c-237">Delegate members</span></span>

<span data-ttu-id="a4c5c-238">委派的成員是繼承自類別 `System.Delegate` 的成員。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-238">The members of a delegate are the members inherited from class `System.Delegate`.</span></span>

## <a name="member-access"></a><span data-ttu-id="a4c5c-239">成員存取</span><span class="sxs-lookup"><span data-stu-id="a4c5c-239">Member access</span></span>

<span data-ttu-id="a4c5c-240">成員的宣告允許對成員存取進行控制。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-240">Declarations of members allow control over member access.</span></span> <span data-ttu-id="a4c5c-241">成員的存取範圍是由成員的宣告存取範圍（宣告的[存取](basic-concepts.md#declared-accessibility)範圍）所建立，並結合立即包含類型的存取範圍（如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-241">The accessibility of a member is established by the declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)) of the member combined with the accessibility of the immediately containing type, if any.</span></span>

<span data-ttu-id="a4c5c-242">允許存取特定成員時，該成員會被視為可***存取***。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-242">When access to a particular member is allowed, the member is said to be ***accessible***.</span></span> <span data-ttu-id="a4c5c-243">相反地，如果不允許存取特定成員，則會將該***成員視為無法存取。***</span><span class="sxs-lookup"><span data-stu-id="a4c5c-243">Conversely, when access to a particular member is disallowed, the member is said to be ***inaccessible***.</span></span> <span data-ttu-id="a4c5c-244">當存取進行的文字位置包含在成員的存取範圍定義域（[存取](basic-concepts.md#accessibility-domains)範圍網域）中時，允許存取成員。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-244">Access to a member is permitted when the textual location in which the access takes place is included in the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the member.</span></span>

### <a name="declared-accessibility"></a><span data-ttu-id="a4c5c-245">已宣告存取範圍</span><span class="sxs-lookup"><span data-stu-id="a4c5c-245">Declared accessibility</span></span>

<span data-ttu-id="a4c5c-246">成員的宣告***存取***範圍可以是下列其中一項：</span><span class="sxs-lookup"><span data-stu-id="a4c5c-246">The ***declared accessibility*** of a member can be one of the following:</span></span>

*  <span data-ttu-id="a4c5c-247">Public，其選取方式是在成員宣告中包含 `public` 修飾詞。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-247">Public, which is selected by including a `public` modifier in the member declaration.</span></span> <span data-ttu-id="a4c5c-248">@No__t-0 的直覺意義是「存取不受限制」。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-248">The intuitive meaning of `public` is "access not limited".</span></span>
*  <span data-ttu-id="a4c5c-249">Protected，藉由在成員宣告中包含 `protected` 修飾詞來選取。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-249">Protected, which is selected by including a `protected` modifier in the member declaration.</span></span> <span data-ttu-id="a4c5c-250">@No__t-0 的直覺意義是「存取限於包含類別或衍生自包含類別的類型」。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-250">The intuitive meaning of `protected` is "access limited to the containing class or types derived from the containing class".</span></span>
*  <span data-ttu-id="a4c5c-251">內部：在成員宣告中包含 `internal` 修飾詞，以選取此選項。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-251">Internal, which is selected by including an `internal` modifier in the member declaration.</span></span> <span data-ttu-id="a4c5c-252">@No__t-0 的直覺意義是「僅限存取此程式」。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-252">The intuitive meaning of `internal` is "access limited to this program".</span></span>
*  <span data-ttu-id="a4c5c-253">受保護的內部（表示 protected 或 internal），其選取方式是在成員宣告中包含 `protected` 和 @no__t 1 修飾詞。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-253">Protected internal (meaning protected or internal), which is selected by including both a `protected` and an `internal` modifier in the member declaration.</span></span> <span data-ttu-id="a4c5c-254">@No__t-0 的直覺意義是「存取限制為此程式或衍生自包含類別的類型」。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-254">The intuitive meaning of `protected internal` is "access limited to this program or types derived from the containing class".</span></span>
*  <span data-ttu-id="a4c5c-255">私用，藉由在成員宣告中包含 `private` 修飾詞來選取。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-255">Private, which is selected by including a `private` modifier in the member declaration.</span></span> <span data-ttu-id="a4c5c-256">@No__t-0 的直覺意義是「存取限制為包含的類型」。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-256">The intuitive meaning of `private` is "access limited to the containing type".</span></span>

<span data-ttu-id="a4c5c-257">視成員宣告發生的內容而定，只允許特定類型的已宣告存取範圍。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-257">Depending on the context in which a member declaration takes place, only certain types of declared accessibility are permitted.</span></span> <span data-ttu-id="a4c5c-258">此外，當成員宣告不包含任何存取修飾詞時，宣告所處的內容會決定預設宣告的存取範圍。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-258">Furthermore, when a member declaration does not include any access modifiers, the context in which the declaration takes place determines the default declared accessibility.</span></span>

*  <span data-ttu-id="a4c5c-259">命名空間隱含具有 `public` 宣告的存取範圍。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-259">Namespaces implicitly have `public` declared accessibility.</span></span> <span data-ttu-id="a4c5c-260">命名空間宣告上不允許有任何存取修飾詞。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-260">No access modifiers are allowed on namespace declarations.</span></span>
*  <span data-ttu-id="a4c5c-261">在編譯單位或命名空間中宣告的類型，可以有 `public` 或 @no__t 1 宣告的存取範圍，而且預設為 @no__t 2 宣告的存取範圍。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-261">Types declared in compilation units or namespaces can have `public` or `internal` declared accessibility and default to `internal` declared accessibility.</span></span>
*  <span data-ttu-id="a4c5c-262">類別成員可以具有五種宣告存取範圍的任何一種，而且預設為 @no__t 0 宣告的存取範圍。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-262">Class members can have any of the five kinds of declared accessibility and default to `private` declared accessibility.</span></span> <span data-ttu-id="a4c5c-263">（請注意，宣告為類別成員的型別可以擁有任何五種宣告的存取範圍，而宣告為命名空間成員的型別只能 `public` 或 @no__t 1 宣告的存取範圍）。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-263">(Note that a type declared as a member of a class can have any of the five kinds of declared accessibility, whereas a type declared as a member of a namespace can have only `public` or `internal` declared accessibility.)</span></span>
*  <span data-ttu-id="a4c5c-264">結構成員可以有 `public`、`internal` 或 @no__t 2 宣告的存取範圍，而且預設為 @no__t 3 宣告的存取範圍，因為結構是隱含密封的。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-264">Struct members can have `public`, `internal`, or `private` declared accessibility and default to `private` declared accessibility because structs are implicitly sealed.</span></span> <span data-ttu-id="a4c5c-265">結構中引進的結構成員（也就是，不是由該結構繼承）不能有 `protected` 或 @no__t 1 宣告的存取範圍。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-265">Struct members introduced in a struct (that is, not inherited by that struct) cannot have `protected` or `protected internal` declared accessibility.</span></span> <span data-ttu-id="a4c5c-266">（請注意，宣告為結構成員的型別可以有 `public`、`internal` 或 @no__t 2 宣告的存取範圍，而宣告為命名空間成員的型別只能有 `public` 或 `internal` 宣告的存取範圍）。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-266">(Note that a type declared as a member of a struct can have `public`, `internal`, or `private` declared accessibility, whereas a type declared as a member of a namespace can have only `public` or `internal` declared accessibility.)</span></span>
*  <span data-ttu-id="a4c5c-267">介面成員隱含具有 `public` 宣告的存取範圍。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-267">Interface members implicitly have `public` declared accessibility.</span></span> <span data-ttu-id="a4c5c-268">介面成員宣告上不允許有任何存取修飾詞。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-268">No access modifiers are allowed on interface member declarations.</span></span>
*  <span data-ttu-id="a4c5c-269">列舉成員隱含具有 `public` 宣告的存取範圍。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-269">Enumeration members implicitly have `public` declared accessibility.</span></span> <span data-ttu-id="a4c5c-270">列舉成員宣告上不允許有任何存取修飾詞。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-270">No access modifiers are allowed on enumeration member declarations.</span></span>

### <a name="accessibility-domains"></a><span data-ttu-id="a4c5c-271">存取範圍網域</span><span class="sxs-lookup"><span data-stu-id="a4c5c-271">Accessibility domains</span></span>

<span data-ttu-id="a4c5c-272">成員的***存取範圍定義域***是由允許存取成員的程式文字（可能不相鄰）區段所組成。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-272">The ***accessibility domain*** of a member consists of the (possibly disjoint) sections of program text in which access to the member is permitted.</span></span> <span data-ttu-id="a4c5c-273">為了定義成員的存取範圍定義域，如果成員未在型別中宣告，則會將其視為***最上層***，而如果成員是在另一個型別內宣告，則會將其視為***嵌套***。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-273">For purposes of defining the accessibility domain of a member, a member is said to be ***top-level*** if it is not declared within a type, and a member is said to be ***nested*** if it is declared within another type.</span></span> <span data-ttu-id="a4c5c-274">此外，程式的***程式文字***會定義為程式的所有原始程式檔中包含的所有程式文字，而類型的程式文字則定義為該類型之*type_declaration*中包含的所有程式文字（包括、可能是在類型中嵌套的類型）。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-274">Furthermore, the ***program text*** of a program is defined as all program text contained in all source files of the program, and the program text of a type is defined as all program text contained in the *type_declaration*s of that type (including, possibly, types that are nested within the type).</span></span>

<span data-ttu-id="a4c5c-275">預先定義類型的存取範圍定義域（例如 `object`、`int` 或 `double`）是無限制的。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-275">The accessibility domain of a predefined type (such as `object`, `int`, or `double`) is unlimited.</span></span>

<span data-ttu-id="a4c5c-276">在程式 `P` 中宣告之最上層未系結類型 `T` （系結[和未](types.md#bound-and-unbound-types)系結類型）的存取範圍定義域定義如下：</span><span class="sxs-lookup"><span data-stu-id="a4c5c-276">The accessibility domain of a top-level unbound type `T` ([Bound and unbound types](types.md#bound-and-unbound-types)) that is declared in a program `P` is defined as follows:</span></span>

*  <span data-ttu-id="a4c5c-277">如果 `T` 的宣告存取範圍是 `public`，`T` 的存取範圍定義域就是 `P` 的程式文字，以及參考 `P` 的任何程式。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-277">If the declared accessibility of `T` is `public`, the accessibility domain of `T` is the program text of `P` and any program that references `P`.</span></span>
*  <span data-ttu-id="a4c5c-278">如果 `T` 的宣告存取範圍是 `internal`，`T` 的存取範圍領域就是 `P` 的程式文字。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-278">If the declared accessibility of `T` is `internal`, the accessibility domain of `T` is the program text of `P`.</span></span>

<span data-ttu-id="a4c5c-279">從這些定義來看，最上層未系結類型的存取範圍定義域一律至少是宣告該類型之程式的程式文字。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-279">From these definitions it follows that the accessibility domain of a top-level unbound type is always at least the program text of the program in which that type is declared.</span></span>

<span data-ttu-id="a4c5c-280">結構化類型的存取範圍定義域 `T<A1, ..., An>` 是未系結泛型型別的存取範圍定義域的交集 `T` 以及類型引數 `A1, ..., An` 的存取範圍定義域。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-280">The accessibility domain for a constructed type `T<A1, ..., An>` is the intersection of the accessibility domain of the unbound generic type `T` and the accessibility domains of the type arguments `A1, ..., An`.</span></span>

<span data-ttu-id="a4c5c-281">在程式 `P` 的 @no__t 類型中宣告的嵌套成員 `M` 的存取範圍定義域，如下所示（請注意，`M` 本身可能是類型）：</span><span class="sxs-lookup"><span data-stu-id="a4c5c-281">The accessibility domain of a nested member `M` declared in a type `T` within a program `P` is defined as follows (noting that `M` itself may possibly be a type):</span></span>

*  <span data-ttu-id="a4c5c-282">如果 `M` 的宣告存取範圍是 `public`，`M` 的存取範圍領域就是 `T` 的存取範圍領域。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-282">If the declared accessibility of `M` is `public`, the accessibility domain of `M` is the accessibility domain of `T`.</span></span>
*  <span data-ttu-id="a4c5c-283">如果 `M` 的宣告存取範圍是 `protected internal`，請讓 `D` 成為 `P` 的程式文字和任何衍生自 `T` 之類型的程式文字的聯集，而這是在 `P` 外部宣告。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-283">If the declared accessibility of `M` is `protected internal`, let `D` be the union of the program text of `P` and the program text of any type derived from `T`, which is declared outside `P`.</span></span> <span data-ttu-id="a4c5c-284">@No__t-0 的存取範圍定義域是 `T` 的存取範圍網域與 `D` 的交集。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-284">The accessibility domain of `M` is the intersection of the accessibility domain of `T` with `D`.</span></span>
*  <span data-ttu-id="a4c5c-285">如果 `M` 的宣告存取範圍是 `protected`，請讓 `D` 成為 `T` 的程式文字和任何衍生自 `T` 之類型的程式文字的聯集。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-285">If the declared accessibility of `M` is `protected`, let `D` be the union of the program text of `T` and the program text of any type derived from `T`.</span></span> <span data-ttu-id="a4c5c-286">@No__t-0 的存取範圍定義域是 `T` 的存取範圍網域與 `D` 的交集。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-286">The accessibility domain of `M` is the intersection of the accessibility domain of `T` with `D`.</span></span>
*  <span data-ttu-id="a4c5c-287">如果 `M` 的宣告存取範圍是 `internal`，`M` 的存取範圍領域是 `T` 的存取範圍領域與 `P` 的程式文字的交集。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-287">If the declared accessibility of `M` is `internal`, the accessibility domain of `M` is the intersection of the accessibility domain of `T` with the program text of `P`.</span></span>
*  <span data-ttu-id="a4c5c-288">如果 `M` 的宣告存取範圍是 `private`，`M` 的存取範圍領域就是 `T` 的程式文字。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-288">If the declared accessibility of `M` is `private`, the accessibility domain of `M` is the program text of `T`.</span></span>

<span data-ttu-id="a4c5c-289">從這些定義來看，嵌套成員的存取範圍定義域一律至少是宣告該成員之類型的程式文字。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-289">From these definitions it follows that the accessibility domain of a nested member is always at least the program text of the type in which the member is declared.</span></span> <span data-ttu-id="a4c5c-290">此外，它還會遵循成員的存取範圍定義域，而不會比宣告該成員之類型的存取範圍定義域更包含在內。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-290">Furthermore, it follows that the accessibility domain of a member is never more inclusive than the accessibility domain of the type in which the member is declared.</span></span>

<span data-ttu-id="a4c5c-291">以直覺的角度來說，當存取類型或成員 `M` 時，會評估下列步驟，以確保允許存取：</span><span class="sxs-lookup"><span data-stu-id="a4c5c-291">In intuitive terms, when a type or member `M` is accessed, the following steps are evaluated to ensure that the access is permitted:</span></span>

*  <span data-ttu-id="a4c5c-292">首先，如果在型別（相對於編譯單位或命名空間）內宣告 `M`，則如果無法存取該類型，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-292">First, if `M` is declared within a type (as opposed to a compilation unit or a namespace), a compile-time error occurs if that type is not accessible.</span></span>
*  <span data-ttu-id="a4c5c-293">然後，如果 `M` `public`，則允許存取。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-293">Then, if `M` is `public`, the access is permitted.</span></span>
*  <span data-ttu-id="a4c5c-294">否則，如果 `M` 是 `protected internal`，則允許存取（如果是在宣告 `M` 的程式中），或是發生在衍生自類別的類別中（其中已宣告 `M`，並會透過衍生類別類型進行）（[Protected實例成員的存取權](basic-concepts.md#protected-access-for-instance-members)）。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-294">Otherwise, if `M` is `protected internal`, the access is permitted if it occurs within the program in which `M` is declared, or if it occurs within a class derived from the class in which `M` is declared and takes place through the derived class type ([Protected access for instance members](basic-concepts.md#protected-access-for-instance-members)).</span></span>
*  <span data-ttu-id="a4c5c-295">否則，如果 `M` 是 `protected`，則如果在宣告 `M` 的類別內，或在衍生自類別的類別（其中已宣告了 `M`，並透過衍生類別類型執行）中，就會允許存取（[Protected實例成員的存取權](basic-concepts.md#protected-access-for-instance-members)）。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-295">Otherwise, if `M` is `protected`, the access is permitted if it occurs within the class in which `M` is declared, or if it occurs within a class derived from the class in which `M` is declared and takes place through the derived class type ([Protected access for instance members](basic-concepts.md#protected-access-for-instance-members)).</span></span>
*  <span data-ttu-id="a4c5c-296">否則，如果 `M` 是 `internal`，則允許在宣告 `M` 的程式中進行存取。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-296">Otherwise, if `M` is `internal`, the access is permitted if it occurs within the program in which `M` is declared.</span></span>
*  <span data-ttu-id="a4c5c-297">否則，如果 `M` 是 `private`，則如果在宣告 `M` 的類型內發生，則允許存取。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-297">Otherwise, if `M` is `private`, the access is permitted if it occurs within the type in which `M` is declared.</span></span>
*  <span data-ttu-id="a4c5c-298">否則，無法存取類型或成員，而且會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-298">Otherwise, the type or member is inaccessible, and a compile-time error occurs.</span></span>

<span data-ttu-id="a4c5c-299">在範例中</span><span class="sxs-lookup"><span data-stu-id="a4c5c-299">In the example</span></span>
```csharp
public class A
{
    public static int X;
    internal static int Y;
    private static int Z;
}

internal class B
{
    public static int X;
    internal static int Y;
    private static int Z;

    public class C
    {
        public static int X;
        internal static int Y;
        private static int Z;
    }

    private class D
    {
        public static int X;
        internal static int Y;
        private static int Z;
    }
}
```
<span data-ttu-id="a4c5c-300">類別和成員具有下列存取範圍網域：</span><span class="sxs-lookup"><span data-stu-id="a4c5c-300">the classes and members have the following accessibility domains:</span></span>

*  <span data-ttu-id="a4c5c-301">@No__t-0 和 `A.X` 的存取範圍網域不受限制。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-301">The accessibility domain of `A` and `A.X` is unlimited.</span></span>
*  <span data-ttu-id="a4c5c-302">@No__t-0、`B`、`B.X`、`B.Y`、`B.C`、`B.C.X` 和 `B.C.Y` 的存取範圍定義域是包含程式的程式文字。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-302">The accessibility domain of `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X`, and `B.C.Y` is the program text of the containing program.</span></span>
*  <span data-ttu-id="a4c5c-303">@No__t-0 的存取範圍網域是 `A` 的程式文字。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-303">The accessibility domain of `A.Z` is the program text of `A`.</span></span>
*  <span data-ttu-id="a4c5c-304">@No__t-0 的存取範圍網域和 `B.D` 是 `B` 的程式文字，包括 `B.C` 和 `B.D` 的程式文字。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-304">The accessibility domain of `B.Z` and `B.D` is the program text of `B`, including the program text of `B.C` and `B.D`.</span></span>
*  <span data-ttu-id="a4c5c-305">@No__t-0 的存取範圍網域是 `B.C` 的程式文字。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-305">The accessibility domain of `B.C.Z` is the program text of `B.C`.</span></span>
*  <span data-ttu-id="a4c5c-306">@No__t-0 的存取範圍網域和 `B.D.Y` 是 `B` 的程式文字，包括 `B.C` 和 `B.D` 的程式文字。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-306">The accessibility domain of `B.D.X` and `B.D.Y` is the program text of `B`, including the program text of `B.C` and `B.D`.</span></span>
*  <span data-ttu-id="a4c5c-307">@No__t-0 的存取範圍網域是 `B.D` 的程式文字。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-307">The accessibility domain of `B.D.Z` is the program text of `B.D`.</span></span>

<span data-ttu-id="a4c5c-308">如範例所示，成員的存取範圍定義域絕不會大於包含類型的存取範圍網域。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-308">As the example illustrates, the accessibility domain of a member is never larger than that of a containing type.</span></span> <span data-ttu-id="a4c5c-309">例如，即使所有 @no__t 0 的成員都具有公用的已宣告存取範圍，但 `A.X` 則擁有受限於包含類型的存取範圍網域。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-309">For example, even though all `X` members have public declared accessibility, all but `A.X` have accessibility domains that are constrained by a containing type.</span></span>

<span data-ttu-id="a4c5c-310">如[成員](basic-concepts.md#members)中所述，衍生類型會繼承基類的所有成員，但實例的函式、析構函式和靜態的函式除外。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-310">As described in [Members](basic-concepts.md#members), all members of a base class, except for instance constructors, destructors and static constructors, are inherited by derived types.</span></span> <span data-ttu-id="a4c5c-311">這也包括基類的私用成員。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-311">This includes even private members of a base class.</span></span> <span data-ttu-id="a4c5c-312">不過，私用成員的存取範圍定義域只會包含宣告該成員之類型的程式文字。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-312">However, the accessibility domain of a private member includes only the program text of the type in which the member is declared.</span></span> <span data-ttu-id="a4c5c-313">在範例中</span><span class="sxs-lookup"><span data-stu-id="a4c5c-313">In the example</span></span>
```csharp
class A
{
    int x;

    static void F(B b) {
        b.x = 1;        // Ok
    }
}

class B: A
{
    static void F(B b) {
        b.x = 1;        // Error, x not accessible
    }
}
```
<span data-ttu-id="a4c5c-314">@no__t 0 類別會從 @no__t 2 類別繼承 `x` 的私用成員。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-314">the `B` class inherits the private member `x` from the `A` class.</span></span> <span data-ttu-id="a4c5c-315">因為此成員是私用的，所以只能在 `A` 的*class_body*中存取。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-315">Because the member is private, it is only accessible within the *class_body* of `A`.</span></span> <span data-ttu-id="a4c5c-316">因此，`b.x` 的存取會成功在 `A.F` 方法中，但會在 @no__t 2 方法中失敗。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-316">Thus, the access to `b.x` succeeds in the `A.F` method, but fails in the `B.F` method.</span></span>

### <a name="protected-access-for-instance-members"></a><span data-ttu-id="a4c5c-317">實例成員的受保護存取</span><span class="sxs-lookup"><span data-stu-id="a4c5c-317">Protected access for instance members</span></span>

<span data-ttu-id="a4c5c-318">當 `protected` 實例成員在宣告它的類別的程式文字之外存取時，以及在宣告的程式文字外部存取 @no__t 1 實例成員時，必須在類別內進行存取。衍生自其宣告所在之類別的宣告。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-318">When a `protected` instance member is accessed outside the program text of the class in which it is declared, and when a `protected internal` instance member is accessed outside the program text of the program in which it is declared, the access must take place within a class declaration that derives from the class in which it is declared.</span></span> <span data-ttu-id="a4c5c-319">此外，您必須透過衍生類別型別的實例或從它所構成的類別型別，來進行存取。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-319">Furthermore, the access is required to take place through an instance of that derived class type or a class type constructed from it.</span></span> <span data-ttu-id="a4c5c-320">這項限制可防止一個衍生類別存取其他衍生類別的受保護成員，即使成員繼承自相同的基類也一樣。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-320">This restriction prevents one derived class from accessing protected members of other derived classes, even when the members are inherited from the same base class.</span></span>

<span data-ttu-id="a4c5c-321">Let `B` 是宣告受保護實例成員 `M` 的基類，讓 `D` 是衍生自 `B` 的類別。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-321">Let `B` be a base class that declares a protected instance member `M`, and let `D` be a class that derives from `B`.</span></span> <span data-ttu-id="a4c5c-322">在 `D` 的*class_body*中，`M` 的存取權可以採用下列其中一種格式：</span><span class="sxs-lookup"><span data-stu-id="a4c5c-322">Within the *class_body* of `D`, access to `M` can take one of the following forms:</span></span>

*  <span data-ttu-id="a4c5c-323">不合格的*type_name*或格式為 `M` 的*primary_expression* 。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-323">An unqualified *type_name* or *primary_expression* of the form `M`.</span></span>
*  <span data-ttu-id="a4c5c-324">@No__t-1 格式的*primary_expression* ，前提是 `E` 的類型為 `T`，或衍生自 `T` 的類別，其中 `T` 是類別類型 `D`，或從 `D` 所構成的類別類型</span><span class="sxs-lookup"><span data-stu-id="a4c5c-324">A *primary_expression* of the form `E.M`, provided the type of `E` is `T` or a class derived from `T`, where `T` is the class type `D`, or a class type constructed from `D`</span></span>
*  <span data-ttu-id="a4c5c-325">@No__t-1 格式的*primary_expression* 。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-325">A *primary_expression* of the form `base.M`.</span></span>

<span data-ttu-id="a4c5c-326">除了這些存取形式，衍生的類別也可以在*constructor_initializer*中存取基類的受保護實例（「函式[初始化運算式](classes.md#constructor-initializers)」）。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-326">In addition to these forms of access, a derived class can access a protected instance constructor of a base class in a *constructor_initializer* ([Constructor initializers](classes.md#constructor-initializers)).</span></span>

<span data-ttu-id="a4c5c-327">在範例中</span><span class="sxs-lookup"><span data-stu-id="a4c5c-327">In the example</span></span>
```csharp
public class A
{
    protected int x;

    static void F(A a, B b) {
        a.x = 1;        // Ok
        b.x = 1;        // Ok
    }
}

public class B: A
{
    static void F(A a, B b) {
        a.x = 1;        // Error, must access through instance of B
        b.x = 1;        // Ok
    }
}
```
<span data-ttu-id="a4c5c-328">在 `A` 中，可以透過 `A` 和 `B` 的實例存取 `x`，因為在任一情況下，都會透過 `A` 的實例或從 `A` 衍生的類別來進行存取。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-328">within `A`, it is possible to access `x` through instances of both `A` and `B`, since in either case the access takes place through an instance of `A` or a class derived from `A`.</span></span> <span data-ttu-id="a4c5c-329">不過，在 `B` 中，無法透過 `A` 的實例存取 `x`，因為 `A` 不是從 `B` 衍生而來。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-329">However, within `B`, it is not possible to access `x` through an instance of `A`, since `A` does not derive from `B`.</span></span>

<span data-ttu-id="a4c5c-330">在範例中</span><span class="sxs-lookup"><span data-stu-id="a4c5c-330">In the example</span></span>
```csharp
class C<T>
{
    protected T x;
}

class D<T>: C<T>
{
    static void F() {
        D<T> dt = new D<T>();
        D<int> di = new D<int>();
        D<string> ds = new D<string>();
        dt.x = default(T);
        di.x = 123;
        ds.x = "test";
    }
}
```
<span data-ttu-id="a4c5c-331">允許 `x` 的三個指派，因為它們都是透過從泛型型別所建立的類別類型實例進行。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-331">the three assignments to `x` are permitted because they all take place through instances of class types constructed from the generic type.</span></span>

### <a name="accessibility-constraints"></a><span data-ttu-id="a4c5c-332">協助工具條件約束</span><span class="sxs-lookup"><span data-stu-id="a4c5c-332">Accessibility constraints</span></span>

<span data-ttu-id="a4c5c-333">在C#語言中，數個結構的類型必須至少可以如同成員或其他類型***一樣存取***。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-333">Several constructs in the C# language require a type to be ***at least as accessible as*** a member or another type.</span></span> <span data-ttu-id="a4c5c-334">如果 `T` 的存取範圍定義域是 `M` 的存取範圍領域的超集合，則 `T` 的類型會被視為至少可以如同成員或類型 `M` 一樣存取。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-334">A type `T` is said to be at least as accessible as a member or type `M` if the accessibility domain of `T` is a superset of the accessibility domain of `M`.</span></span> <span data-ttu-id="a4c5c-335">換句話說，如果在可存取 `M` 的所有內容中，`T` 可供存取，則 `T` 至少會如 `M` 一樣存取。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-335">In other words, `T` is at least as accessible as `M` if `T` is accessible in all contexts in which `M` is accessible.</span></span>

<span data-ttu-id="a4c5c-336">存在下列協助工具條件約束：</span><span class="sxs-lookup"><span data-stu-id="a4c5c-336">The following accessibility constraints exist:</span></span>

*  <span data-ttu-id="a4c5c-337">類別類型的直接基底類別至少必須可以像類別類型本身一樣地存取。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-337">The direct base class of a class type must be at least as accessible as the class type itself.</span></span>
*  <span data-ttu-id="a4c5c-338">介面類型的明確基底介面至少必須可以像介面類型本身一樣地存取。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-338">The explicit base interfaces of an interface type must be at least as accessible as the interface type itself.</span></span>
*  <span data-ttu-id="a4c5c-339">委派類型的傳回類型和參數類型至少必須可以像委派類型本身一樣地存取。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-339">The return type and parameter types of a delegate type must be at least as accessible as the delegate type itself.</span></span>
*  <span data-ttu-id="a4c5c-340">常數的類型至少必須可以像常數本身一樣地存取。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-340">The type of a constant must be at least as accessible as the constant itself.</span></span>
*  <span data-ttu-id="a4c5c-341">欄位的類型至少必須可以像欄位本身一樣地存取。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-341">The type of a field must be at least as accessible as the field itself.</span></span>
*  <span data-ttu-id="a4c5c-342">方法的傳回類型和參數類型至少必須可以像方法本身一樣地存取。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-342">The return type and parameter types of a method must be at least as accessible as the method itself.</span></span>
*  <span data-ttu-id="a4c5c-343">屬性的類型至少必須可以像屬性本身一樣地存取。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-343">The type of a property must be at least as accessible as the property itself.</span></span>
*  <span data-ttu-id="a4c5c-344">事件的類型至少必須可以像事件本身一樣地存取。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-344">The type of an event must be at least as accessible as the event itself.</span></span>
*  <span data-ttu-id="a4c5c-345">索引子的類型和參數類型至少必須可以像索引子本身一樣地存取。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-345">The type and parameter types of an indexer must be at least as accessible as the indexer itself.</span></span>
*  <span data-ttu-id="a4c5c-346">運算子的傳回類型和參數類型至少必須可以像運算子本身一樣地存取。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-346">The return type and parameter types of an operator must be at least as accessible as the operator itself.</span></span>
*  <span data-ttu-id="a4c5c-347">實例構造函式的參數類型必須至少與實例的函式本身一樣可以存取。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-347">The parameter types of an instance constructor must be at least as accessible as the instance constructor itself.</span></span>

<span data-ttu-id="a4c5c-348">在範例中</span><span class="sxs-lookup"><span data-stu-id="a4c5c-348">In the example</span></span>
```csharp
class A {...}

public class B: A {...}
```
<span data-ttu-id="a4c5c-349">@no__t 0 類別會導致編譯時期錯誤，因為 `A` 的存取權至少不如 `B`。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-349">the `B` class results in a compile-time error because `A` is not at least as accessible as `B`.</span></span>

<span data-ttu-id="a4c5c-350">同樣地，在此範例中</span><span class="sxs-lookup"><span data-stu-id="a4c5c-350">Likewise, in the example</span></span>
```csharp
class A {...}

public class B
{
    A F() {...}

    internal A G() {...}

    public A H() {...}
}
```
<span data-ttu-id="a4c5c-351">`B` 中的 `H` 方法會導致編譯時期錯誤，因為傳回的類型 `A` 不是至少與方法相同的存取權。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-351">the `H` method in `B` results in a compile-time error because the return type `A` is not at least as accessible as the method.</span></span>

## <a name="signatures-and-overloading"></a><span data-ttu-id="a4c5c-352">簽章和多載</span><span class="sxs-lookup"><span data-stu-id="a4c5c-352">Signatures and overloading</span></span>

<span data-ttu-id="a4c5c-353">方法、實例的函式、索引子和運算子都是***以其簽章為特徵：***</span><span class="sxs-lookup"><span data-stu-id="a4c5c-353">Methods, instance constructors, indexers, and operators are characterized by their ***signatures***:</span></span>

*  <span data-ttu-id="a4c5c-354">方法的簽章包含方法的名稱、型別參數的數目，以及每一個型式參數的類型和種類（值、參考或輸出），以從左至右的順序來考慮。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-354">The signature of a method consists of the name of the method, the number of type parameters and the type and kind (value, reference, or output) of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="a4c5c-355">基於這些目的，在型式參數類型中發生之方法的任何型別參數，都不是以其名稱來識別，而是依其在方法的型別引數清單中的序數位置。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-355">For these purposes, any type parameter of the method that occurs in the type of a formal parameter is identified not by its name, but by its ordinal position in the type argument list of the method.</span></span> <span data-ttu-id="a4c5c-356">方法的簽章特別不包含傳回型別、可為最右邊參數指定的 `params` 修飾詞，也不包括選擇性的型別參數條件約束。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-356">The signature of a method specifically does not include the return type, the `params` modifier that may be specified for the right-most parameter, nor the optional type parameter constraints.</span></span>
*  <span data-ttu-id="a4c5c-357">實例函式的簽章包含其每一個型式參數的類型和種類（值、參考或輸出），並以從左至右的順序來考慮。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-357">The signature of an instance constructor consists of the type and kind (value, reference, or output) of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="a4c5c-358">實例函式的簽章特別不包含可為最右邊參數指定的 `params` 修飾詞。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-358">The signature of an instance constructor specifically does not include the `params` modifier that may be specified for the right-most parameter.</span></span>
*  <span data-ttu-id="a4c5c-359">索引子的簽章是由它的每個型式參數的類型所組成，並以由左至右的順序來表示。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-359">The signature of an indexer consists of the type of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="a4c5c-360">索引子的簽章特別不包含元素類型，也不包含可為最右邊參數指定的 `params` 修飾詞。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-360">The signature of an indexer specifically does not include the element type, nor does it include the `params` modifier that may be specified for the right-most parameter.</span></span>
*  <span data-ttu-id="a4c5c-361">運算子的簽章是由運算子的名稱和每個型式參數的類型所組成，並以從左至右的順序來考慮。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-361">The signature of an operator consists of the name of the operator and the type of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="a4c5c-362">運算子的簽章特別不包含結果型別。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-362">The signature of an operator specifically does not include the result type.</span></span>

<span data-ttu-id="a4c5c-363">簽章是在類別、結構和介面中，多載***成員的啟用***機制：</span><span class="sxs-lookup"><span data-stu-id="a4c5c-363">Signatures are the enabling mechanism for ***overloading*** of members in classes, structs, and interfaces:</span></span>

*  <span data-ttu-id="a4c5c-364">方法的多載可讓類別、結構或介面宣告多個具有相同名稱的方法，但前提是其簽章在該類別、結構或介面中是唯一的。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-364">Overloading of methods permits a class, struct, or interface to declare multiple methods with the same name, provided their signatures are unique within that class, struct, or interface.</span></span>
*  <span data-ttu-id="a4c5c-365">如果類別或結構的簽章在該類別或結構內是唯一的，則實例函式的多載可讓您宣告多個實例的函式。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-365">Overloading of instance constructors permits a class or struct to declare multiple instance constructors, provided their signatures are unique within that class or struct.</span></span>
*  <span data-ttu-id="a4c5c-366">索引子的多載可讓類別、結構或介面宣告多個索引子，但前提是其簽章在該類別、結構或介面中是唯一的。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-366">Overloading of indexers permits a class, struct, or interface to declare multiple indexers, provided their signatures are unique within that class, struct, or interface.</span></span>
*  <span data-ttu-id="a4c5c-367">運算子的多載可讓類別或結構宣告多個具有相同名稱的運算子，但前提是其簽章在該類別或結構中是唯一的。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-367">Overloading of operators permits a class or struct to declare multiple operators with the same name, provided their signatures are unique within that class or struct.</span></span>

<span data-ttu-id="a4c5c-368">雖然 `out` 和 @no__t 1 參數修飾詞是簽章的一部分，但在單一類型中宣告的成員，在簽章中不能單獨由 `ref` 和 `out` 而有所不同。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-368">Although `out` and `ref` parameter modifiers are considered part of a signature, members declared in a single type cannot differ in signature solely by `ref` and `out`.</span></span> <span data-ttu-id="a4c5c-369">如果兩個具有 `out` 修飾詞的方法中的所有參數都已變更為 `ref` 修飾詞，則在相同類型中使用相同的簽章宣告兩個成員時，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-369">A compile-time error occurs if two members are declared in the same type with signatures that would be the same if all parameters in both methods with `out` modifiers were changed to `ref` modifiers.</span></span> <span data-ttu-id="a4c5c-370">針對簽章比對的其他用途（例如，隱藏或覆寫），`ref`，而 `out` 則視為簽章的一部分，而且彼此不相符。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-370">For other purposes of signature matching (e.g., hiding or overriding), `ref` and `out` are considered part of the signature and do not match each other.</span></span> <span data-ttu-id="a4c5c-371">（這項限制是為了C#讓程式能夠輕鬆地轉譯成在通用語言基礎結構（CLI）上執行，這不會提供方法來定義只在 `ref` 和 `out` 中不同的方法）。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-371">(This restriction is to allow C#  programs to be easily translated to run on the Common Language Infrastructure (CLI), which does not provide a way to define methods that differ solely in `ref` and `out`.)</span></span>

<span data-ttu-id="a4c5c-372">基於簽章的目的，`object` 和 `dynamic` 的類型會被視為相同。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-372">For the purposes of signatures, the types `object` and `dynamic` are considered the same.</span></span> <span data-ttu-id="a4c5c-373">因此，在單一類型中宣告的成員，在簽章中可能不會單獨受到 `object` 和 `dynamic` 的差異。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-373">Members declared in a single type can therefore not differ in signature solely by `object` and `dynamic`.</span></span>

<span data-ttu-id="a4c5c-374">下列範例會顯示一組多載的方法宣告以及其簽章。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-374">The following example shows a set of overloaded method declarations along with their signatures.</span></span>
```csharp
interface ITest
{
    void F();                        // F()

    void F(int x);                   // F(int)

    void F(ref int x);               // F(ref int)

    void F(out int x);               // F(out int)      error

    void F(int x, int y);            // F(int, int)

    int F(string s);                 // F(string)

    int F(int x);                    // F(int)          error

    void F(string[] a);              // F(string[])

    void F(params string[] a);       // F(string[])     error
}
```

<span data-ttu-id="a4c5c-375">請注意，任何 `ref` 和 @no__t 1 參數修飾詞（[方法參數](classes.md#method-parameters)）都是簽章的一部分。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-375">Note that any `ref` and `out` parameter modifiers ([Method parameters](classes.md#method-parameters)) are part of a signature.</span></span> <span data-ttu-id="a4c5c-376">因此，`F(int)` 和 `F(ref int)` 是唯一的簽章。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-376">Thus, `F(int)` and `F(ref int)` are unique signatures.</span></span> <span data-ttu-id="a4c5c-377">不過，不能在相同的介面內宣告 `F(ref int)` 和 `F(out int)`，因為它們的簽章不同于 `ref` 和 `out`。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-377">However, `F(ref int)` and `F(out int)` cannot be declared within the same interface because their signatures differ solely by `ref` and `out`.</span></span> <span data-ttu-id="a4c5c-378">另請注意，傳回型別和 `params` 修飾詞不是簽章的一部分，因此不可能根據傳回型別或包含或排除 `params` 修飾詞而單獨進行多載。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-378">Also, note that the return type and the `params` modifier are not part of a signature, so it is not possible to overload solely based on return type or on the inclusion or exclusion of the `params` modifier.</span></span> <span data-ttu-id="a4c5c-379">因此，上述的方法宣告 `F(int)`，而 `F(params string[])` 則會導致編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-379">As such, the declarations of the methods `F(int)` and `F(params string[])` identified above result in a compile-time error.</span></span>

## <a name="scopes"></a><span data-ttu-id="a4c5c-380">範圍</span><span class="sxs-lookup"><span data-stu-id="a4c5c-380">Scopes</span></span>

<span data-ttu-id="a4c5c-381">名稱的***範圍***是程式文字的區域，可以在其中參考名稱所宣告的實體，而不需要限定名稱。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-381">The ***scope*** of a name is the region of program text within which it is possible to refer to the entity declared by the name without qualification of the name.</span></span> <span data-ttu-id="a4c5c-382">範圍可以是***嵌套***的，而且內部範圍可能會從外部範圍重新宣告名稱的意義（不過，這不會移除嵌套區塊內的宣告所[加諸](basic-concepts.md#declarations)的限制，因此無法使用與封閉區塊中的本機變數同名）。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-382">Scopes can be ***nested***, and an inner scope may redeclare the meaning of a name from an outer scope (this does not, however, remove the restriction imposed by [Declarations](basic-concepts.md#declarations) that within a nested block it is not possible to declare a local variable with the same name as a local variable in an enclosing block).</span></span> <span data-ttu-id="a4c5c-383">外部範圍中的名稱會被視為***隱藏***于內部範圍所涵蓋的程式文字區域中，而且只有限定名稱才能存取外部名稱。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-383">The name from the outer scope is then said to be ***hidden*** in the region of program text covered by the inner scope, and access to the outer name is only possible by qualifying the name.</span></span>

*  <span data-ttu-id="a4c5c-384">*Namespace_member_declaration* （[命名空間成員](namespaces.md#namespace-members)）所宣告的命名空間成員範圍（不含封閉的*namespace_declaration* ）是整個程式文字。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-384">The scope of a namespace member declared by a *namespace_member_declaration* ([Namespace members](namespaces.md#namespace-members)) with no enclosing *namespace_declaration* is the entire program text.</span></span>
*  <span data-ttu-id="a4c5c-385">*Namespace_declaration*內的*namespace_member_declaration*所宣告的命名空間成員範圍，其完整名稱為 `N` 是每個*namespace_declaration*的*namespace_body* ，其完整限定名稱為 `N`，或以 `N` 開頭，後面接著句號。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-385">The scope of a namespace member declared by a *namespace_member_declaration* within a *namespace_declaration* whose fully qualified name is `N` is the *namespace_body* of every *namespace_declaration* whose fully qualified name is `N` or starts with `N`, followed by a period.</span></span>
*  <span data-ttu-id="a4c5c-386">*Extern_alias_directive*所定義的名稱範圍會延伸到其立即包含編譯單位或命名空間主體的*using_directive*s、 *global_attributes*和*namespace_member_declaration*s。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-386">The scope of name defined by an *extern_alias_directive* extends over the *using_directive*s, *global_attributes* and *namespace_member_declaration*s of its immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="a4c5c-387">*Extern_alias_directive*不會對基礎宣告空間提供任何新成員。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-387">An *extern_alias_directive* does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="a4c5c-388">換句話說， *extern_alias_directive*不是可轉移的，而是只會影響其發生所在的編譯單位或命名空間主體。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-388">In other words, an *extern_alias_directive* is not transitive, but, rather, affects only the compilation unit or namespace body in which it occurs.</span></span>
*  <span data-ttu-id="a4c5c-389">*Using_directive* （[using](namespaces.md#using-directives)指示詞）所定義或匯入之名稱的範圍會延伸到*compilation_unit*或*namespace_body*的*namespace_member_declaration*s，其中*using_directive*發生。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-389">The scope of a name defined or imported by a *using_directive* ([Using directives](namespaces.md#using-directives)) extends over the *namespace_member_declaration*s of the *compilation_unit* or *namespace_body* in which the *using_directive* occurs.</span></span> <span data-ttu-id="a4c5c-390">*Using_directive*可能會在特定的*compilation_unit*或*namespace_body*內提供零個或多個命名空間、類型或成員名稱，但不會將任何新成員貢獻給基礎宣告空間。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-390">A *using_directive* may make zero or more namespace, type or member names available within a particular *compilation_unit* or *namespace_body*, but does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="a4c5c-391">換句話說， *using_directive*無法轉移，而只會影響其發生的*compilation_unit*或*namespace_body* 。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-391">In other words, a *using_directive* is not transitive but rather affects only the *compilation_unit* or *namespace_body* in which it occurs.</span></span>
*  <span data-ttu-id="a4c5c-392">*Class_declaration* （[類別](classes.md#class-declarations)宣告）上*type_parameter_list*所宣告之類型參數的範圍是該的*class_base*、 *type_parameter_constraints_clause*s 和*class_body* *class_declaration*。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-392">The scope of a type parameter declared by a *type_parameter_list* on a *class_declaration* ([Class declarations](classes.md#class-declarations)) is the *class_base*, *type_parameter_constraints_clause*s, and *class_body* of that *class_declaration*.</span></span>
*  <span data-ttu-id="a4c5c-393">*Struct_declaration*上的*type_parameter_list*所宣告之類型參數的範圍（[結構](structs.md#struct-declarations)宣告）為*struct_interfaces*、 *type_parameter_constraints_clause*s 和*struct_body* ，屬於這*struct_declaration*。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-393">The scope of a type parameter declared by a *type_parameter_list* on a *struct_declaration* ([Struct declarations](structs.md#struct-declarations)) is the *struct_interfaces*, *type_parameter_constraints_clause*s, and *struct_body* of that *struct_declaration*.</span></span>
*  <span data-ttu-id="a4c5c-394">*Interface_declaration* （[介面](interfaces.md#interface-declarations)宣告）上*type_parameter_list*所宣告之類型參數的範圍為*interface_base*、 *type_parameter_constraints_clause*s 和*interface_body*的*interface_declaration*。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-394">The scope of a type parameter declared by a *type_parameter_list* on an *interface_declaration* ([Interface declarations](interfaces.md#interface-declarations)) is the *interface_base*, *type_parameter_constraints_clause*s, and *interface_body* of that *interface_declaration*.</span></span>
*  <span data-ttu-id="a4c5c-395">*Delegate_declaration* （[委派](delegates.md#delegate-declarations)宣告）上*type_parameter_list*所宣告之類型參數的範圍為*return_type*、 *formal_parameter_list*和*type_parameter_constraints_clause* *delegate_declaration*。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-395">The scope of a type parameter declared by a *type_parameter_list* on a *delegate_declaration* ([Delegate declarations](delegates.md#delegate-declarations)) is the *return_type*, *formal_parameter_list*, and *type_parameter_constraints_clause*s of that *delegate_declaration*.</span></span>
*  <span data-ttu-id="a4c5c-396">*Class_member_declaration* （[類別主體](classes.md#class-body)）所宣告的成員範圍是宣告發生所在的*class_body* 。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-396">The scope of a member declared by a *class_member_declaration* ([Class body](classes.md#class-body)) is the *class_body* in which the declaration occurs.</span></span> <span data-ttu-id="a4c5c-397">此外，類別成員的範圍會延伸至成員的存取範圍定義域（[存取範圍定義域](basic-concepts.md#accessibility-domains)）中所包含的衍生類別的*class_body* 。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-397">In addition, the scope of a class member extends to the *class_body* of those derived classes that are included in the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the member.</span></span>
*  <span data-ttu-id="a4c5c-398">*Struct_member_declaration* （[結構成員](structs.md#struct-members)）所宣告的成員範圍是宣告發生所在的*struct_body* 。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-398">The scope of a member declared by a *struct_member_declaration* ([Struct members](structs.md#struct-members)) is the *struct_body* in which the declaration occurs.</span></span>
*  <span data-ttu-id="a4c5c-399">*Enum_member_declaration* （[列舉成員](enums.md#enum-members)）所宣告的成員範圍是宣告發生所在的*enum_body* 。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-399">The scope of a member declared by an *enum_member_declaration*  ([Enum members](enums.md#enum-members)) is the *enum_body* in which the declaration occurs.</span></span>
*  <span data-ttu-id="a4c5c-400">在*method_declaration* （[方法](classes.md#methods)）中宣告的參數範圍是該*method_declaration*的*method_body* 。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-400">The scope of a parameter declared in a *method_declaration* ([Methods](classes.md#methods)) is the *method_body* of that *method_declaration*.</span></span>
*  <span data-ttu-id="a4c5c-401">在*indexer_declaration* （[索引子](classes.md#indexers)）中宣告的參數範圍是該*indexer_declaration*的*accessor_declarations* 。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-401">The scope of a parameter declared in an *indexer_declaration* ([Indexers](classes.md#indexers)) is the *accessor_declarations* of that *indexer_declaration*.</span></span>
*  <span data-ttu-id="a4c5c-402">在*operator_declaration* （[運算子](classes.md#operators)）中宣告的參數範圍是該*operator_declaration*的*區塊*。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-402">The scope of a parameter declared in an *operator_declaration* ([Operators](classes.md#operators)) is the *block* of that *operator_declaration*.</span></span>
*  <span data-ttu-id="a4c5c-403">在*constructor_declaration*中宣告的參數範圍（[實例](classes.md#instance-constructors)的程式，也就是該*constructor_declaration*的*constructor_initializer*和*區塊*）。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-403">The scope of a parameter declared in a *constructor_declaration* ([Instance constructors](classes.md#instance-constructors)) is the *constructor_initializer* and *block* of that *constructor_declaration*.</span></span>
*  <span data-ttu-id="a4c5c-404">在*lambda_expression*中宣告的參數範圍（[匿名函數運算式](expressions.md#anonymous-function-expressions)）是該*lambda_expression*的*anonymous_function_body* 。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-404">The scope of a parameter declared in a *lambda_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) is the *anonymous_function_body* of that *lambda_expression*</span></span>
*  <span data-ttu-id="a4c5c-405">在*anonymous_method_expression*中宣告的參數範圍（[匿名函數運算式](expressions.md#anonymous-function-expressions)）是該*anonymous_method_expression*的*區塊*。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-405">The scope of a parameter declared in an *anonymous_method_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) is the *block* of that *anonymous_method_expression*.</span></span>
*  <span data-ttu-id="a4c5c-406">在*labeled_statement*中宣告的標籤範圍（加上[標籤的語句](statements.md#labeled-statements)）是宣告發生所在的*區塊*。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-406">The scope of a label declared in a *labeled_statement* ([Labeled statements](statements.md#labeled-statements)) is the *block* in which the declaration occurs.</span></span>
*  <span data-ttu-id="a4c5c-407">在*local_variable_declaration*中宣告的本機變數範圍（[區域變數](statements.md#local-variable-declarations)宣告）是發生宣告的區塊。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-407">The scope of a local variable declared in a *local_variable_declaration* ([Local variable declarations](statements.md#local-variable-declarations)) is the block in which the declaration occurs.</span></span>
*  <span data-ttu-id="a4c5c-408">在 @no__t 1 語句（[switch 語句](statements.md#the-switch-statement)）的*switch_block*中宣告的區域變數範圍是*switch_block*。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-408">The scope of a local variable declared in a *switch_block* of a `switch` statement ([The switch statement](statements.md#the-switch-statement)) is the *switch_block*.</span></span>
*  <span data-ttu-id="a4c5c-409">在 @no__t 1 語句（[for 語句](statements.md#the-for-statement)）的*for_initializer*中宣告的區域變數範圍是*for_initializer*、 *for_condition*、 *for_iterator*，以及的包含*語句*。`for` 語句。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-409">The scope of a local variable declared in a *for_initializer* of a `for` statement ([The for statement](statements.md#the-for-statement)) is the *for_initializer*, the *for_condition*, the *for_iterator*, and the contained *statement* of the `for` statement.</span></span>
*  <span data-ttu-id="a4c5c-410">在*local_constant_declaration*中宣告的本機常數範圍（[區域常數](statements.md#local-constant-declarations)宣告）是宣告發生所在的區塊。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-410">The scope of a local constant declared in a *local_constant_declaration* ([Local constant declarations](statements.md#local-constant-declarations)) is the block in which the declaration occurs.</span></span> <span data-ttu-id="a4c5c-411">這是編譯時期錯誤，可在其*constant_declarator*之前的文字位置參考本機常數。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-411">It is a compile-time error to refer to a local constant in a textual position that precedes its *constant_declarator*.</span></span>
*  <span data-ttu-id="a4c5c-412">宣告為*foreach_statement*、 *using_statement*、 *lock_statement*或*q*之一部分之變數的範圍是由指定結構的展開所決定。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-412">The scope of a variable declared as part of a *foreach_statement*, *using_statement*, *lock_statement* or *query_expression* is determined by the expansion of the given construct.</span></span>

<span data-ttu-id="a4c5c-413">在命名空間、類別、結構或列舉成員的範圍內，您可以在成員宣告之前，參考文字位置中的成員。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-413">Within the scope of a namespace, class, struct, or enumeration member it is possible to refer to the member in a textual position that precedes the declaration of the member.</span></span> <span data-ttu-id="a4c5c-414">例如：</span><span class="sxs-lookup"><span data-stu-id="a4c5c-414">For example</span></span>
```csharp
class A
{
    void F() {
        i = 1;
    }

    int i = 0;
}
```
<span data-ttu-id="a4c5c-415">在這裡，它是有效的，`F` 會在宣告之前參考 `i`。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-415">Here, it is valid for `F` to refer to `i` before it is declared.</span></span>

<span data-ttu-id="a4c5c-416">在本機變數的範圍內，會發生編譯時期錯誤，以在本機變數*local_variable_declarator*之前的文字位置參考本機變數。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-416">Within the scope of a local variable, it is a compile-time error to refer to the local variable in a textual position that precedes the *local_variable_declarator* of the local variable.</span></span> <span data-ttu-id="a4c5c-417">例如：</span><span class="sxs-lookup"><span data-stu-id="a4c5c-417">For example</span></span>
```csharp
class A
{
    int i = 0;

    void F() {
        i = 1;                  // Error, use precedes declaration
        int i;
        i = 2;
    }

    void G() {
        int j = (j = 1);        // Valid
    }

    void H() {
        int a = 1, b = ++a;    // Valid
    }
}
```

<span data-ttu-id="a4c5c-418">在上述的 `F` 方法中，第一次指派至 `i` 特別不會參考外部範圍中所宣告的欄位。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-418">In the `F` method above, the first assignment to `i` specifically does not refer to the field declared in the outer scope.</span></span> <span data-ttu-id="a4c5c-419">相反地，它會參考區域變數，而且會導致編譯時期錯誤，因為它是以一詞的方式在變數的宣告前面。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-419">Rather, it refers to the local variable and it results in a compile-time error because it textually precedes the declaration of the variable.</span></span> <span data-ttu-id="a4c5c-420">在 `G` 方法中，在 `j` 宣告的初始化運算式中使用 `j` 是有效的，因為使用不在*local_variable_declarator*之前。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-420">In the `G` method, the use of `j` in the initializer for the declaration of `j` is valid because the use does not precede the *local_variable_declarator*.</span></span> <span data-ttu-id="a4c5c-421">在 `H` 方法中，後續的*local_variable_declarator*會正確地參考在相同*local_variable_declaration*內先前*local_variable_declarator*中所宣告的本機變數。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-421">In the `H` method, a subsequent *local_variable_declarator* correctly refers to a local variable declared in an earlier *local_variable_declarator* within the same *local_variable_declaration*.</span></span>

<span data-ttu-id="a4c5c-422">本機變數的範圍規則是設計來保證運算式內容中所使用之名稱的意義在區塊內一律是相同的。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-422">The scoping rules for local variables are designed to guarantee that the meaning of a name used in an expression context is always the same within a block.</span></span> <span data-ttu-id="a4c5c-423">如果區域變數的範圍只會從其宣告延伸到區塊結尾，則在上述範例中，第一個指派會指派給執行個體變數，而第二個指派會指派給本機變數，可能會導致如果稍後要重新排列區塊的語句，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-423">If the scope of a local variable were to extend only from its declaration to the end of the block, then in the example above, the first assignment would assign to the instance variable and the second assignment would assign to the local variable, possibly leading to compile-time errors if the statements of the block were later to be rearranged.</span></span>

<span data-ttu-id="a4c5c-424">區塊內名稱的意義可能會根據使用名稱的內容而有所不同。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-424">The meaning of a name within a block may differ based on the context in which the name is used.</span></span> <span data-ttu-id="a4c5c-425">在範例中</span><span class="sxs-lookup"><span data-stu-id="a4c5c-425">In the example</span></span>
```csharp
using System;

class A {}

class Test
{
    static void Main() {
        string A = "hello, world";
        string s = A;                            // expression context

        Type t = typeof(A);                      // type context

        Console.WriteLine(s);                    // writes "hello, world"
        Console.WriteLine(t);                    // writes "A"
    }
}
```
<span data-ttu-id="a4c5c-426">運算式內容中會使用 `A` 的名稱來參考本機變數 `A`，並在類型內容中參考類別 `A`。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-426">the name `A` is used in an expression context to refer to the local variable `A` and in a type context to refer to the class `A`.</span></span>

### <a name="name-hiding"></a><span data-ttu-id="a4c5c-427">隱藏名稱</span><span class="sxs-lookup"><span data-stu-id="a4c5c-427">Name hiding</span></span>

<span data-ttu-id="a4c5c-428">實體的範圍通常會包含比實體的宣告空間更多的程式文字。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-428">The scope of an entity typically encompasses more program text than the declaration space of the entity.</span></span> <span data-ttu-id="a4c5c-429">特別是，實體的範圍可能包括引入新宣告空間的宣告，其中包含相同名稱的實體。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-429">In particular, the scope of an entity may include declarations that introduce new declaration spaces containing entities of the same name.</span></span> <span data-ttu-id="a4c5c-430">這類宣告會使原始實體變成***隱藏狀態***。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-430">Such declarations cause the original entity to become ***hidden***.</span></span> <span data-ttu-id="a4c5c-431">相反地，實體會在未隱藏時被視為***可見***。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-431">Conversely, an entity is said to be ***visible*** when it is not hidden.</span></span>

<span data-ttu-id="a4c5c-432">範圍重迭時，會發生名稱隱藏，而範圍則是透過繼承來重迭。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-432">Name hiding occurs when scopes overlap through nesting and when scopes overlap through inheritance.</span></span> <span data-ttu-id="a4c5c-433">下列各節將說明這兩種隱藏類型的特性。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-433">The characteristics of the two types of hiding are described in the following sections.</span></span>

#### <a name="hiding-through-nesting"></a><span data-ttu-id="a4c5c-434">透過嵌套隱藏</span><span class="sxs-lookup"><span data-stu-id="a4c5c-434">Hiding through nesting</span></span>

<span data-ttu-id="a4c5c-435">藉由嵌套命名空間或命名空間內的型別，可能會因為在類別或結構內嵌套型別，以及做為參數和區域變數宣告的結果，而導致名稱隱藏。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-435">Name hiding through nesting can occur as a result of nesting namespaces or types within namespaces, as a result of nesting types within classes or structs, and as a result of parameter and local variable declarations.</span></span>

<span data-ttu-id="a4c5c-436">在範例中</span><span class="sxs-lookup"><span data-stu-id="a4c5c-436">In the example</span></span>
```csharp
class A
{
    int i = 0;

    void F() {
        int i = 1;
    }

    void G() {
        i = 1;
    }
}
```
<span data-ttu-id="a4c5c-437">在 `F` 方法中，`i` 的執行個體變數會由本機變數 `i` 隱藏，但是在 `G` 方法中，`i` 仍然是指執行個體變數。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-437">within the `F` method, the instance variable `i` is hidden by the local variable `i`, but within the `G` method, `i` still refers to the instance variable.</span></span>

<span data-ttu-id="a4c5c-438">當內部範圍中的名稱隱藏外部範圍中的名稱時，它會隱藏該名稱的所有多載出現次數。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-438">When a name in an inner scope hides a name in an outer scope, it hides all overloaded occurrences of that name.</span></span> <span data-ttu-id="a4c5c-439">在範例中</span><span class="sxs-lookup"><span data-stu-id="a4c5c-439">In the example</span></span>
```csharp
class Outer
{
    static void F(int i) {}

    static void F(string s) {}

    class Inner
    {
        void G() {
            F(1);              // Invokes Outer.Inner.F
            F("Hello");        // Error
        }

        static void F(long l) {}
    }
}
```
<span data-ttu-id="a4c5c-440">呼叫 `F(1)` 會叫用 `Inner` 中所宣告的 `F`，因為內部宣告會隱藏所有 `F` 的外部出現專案。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-440">the call `F(1)` invokes the `F` declared in `Inner` because all outer occurrences of `F` are hidden by the inner declaration.</span></span> <span data-ttu-id="a4c5c-441">基於相同的原因，呼叫 `F("Hello")` 會導致編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-441">For the same reason, the call `F("Hello")` results in a compile-time error.</span></span>

#### <a name="hiding-through-inheritance"></a><span data-ttu-id="a4c5c-442">透過繼承隱藏</span><span class="sxs-lookup"><span data-stu-id="a4c5c-442">Hiding through inheritance</span></span>

<span data-ttu-id="a4c5c-443">當類別或結構重新宣告繼承自基類的名稱時，會透過繼承來隱藏名稱。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-443">Name hiding through inheritance occurs when classes or structs redeclare names that were inherited from base classes.</span></span> <span data-ttu-id="a4c5c-444">這種類型的名稱隱藏會採用下列其中一種形式：</span><span class="sxs-lookup"><span data-stu-id="a4c5c-444">This type of name hiding takes one of the following forms:</span></span>

*  <span data-ttu-id="a4c5c-445">在類別或結構中引進的常數、欄位、屬性、事件或類型，會隱藏所有具有相同名稱的基類成員。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-445">A constant, field, property, event, or type introduced in a class or struct hides all base class members with the same name.</span></span>
*  <span data-ttu-id="a4c5c-446">在類別或結構中引進的方法會隱藏所有具有相同名稱的非方法基類成員，以及具有相同簽章的所有基類方法（方法名稱和參數計數、修飾詞和類型）。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-446">A method introduced in a class or struct hides all non-method base class members with the same name, and all base class methods with the same signature (method name and parameter count, modifiers, and types).</span></span>
*  <span data-ttu-id="a4c5c-447">在類別或結構中引進的索引子會隱藏所有具有相同簽章（參數計數和類型）的基類索引子。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-447">An indexer introduced in a class or struct hides all base class indexers with the same signature (parameter count and types).</span></span>

<span data-ttu-id="a4c5c-448">管理運算子宣告（[運算子](classes.md#operators)）的規則讓衍生類別無法宣告運算子，其簽章與基類中的運算子具有相同的簽章。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-448">The rules governing operator declarations ([Operators](classes.md#operators)) make it impossible for a derived class to declare an operator with the same signature as an operator in a base class.</span></span> <span data-ttu-id="a4c5c-449">因此，運算子絕對不會隱藏另一個。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-449">Thus, operators never hide one another.</span></span>

<span data-ttu-id="a4c5c-450">相反地，隱藏外部範圍中的名稱，從繼承的範圍隱藏可存取的名稱會導致回報警告。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-450">Contrary to hiding a name from an outer scope, hiding an accessible name from an inherited scope causes a warning to be reported.</span></span> <span data-ttu-id="a4c5c-451">在範例中</span><span class="sxs-lookup"><span data-stu-id="a4c5c-451">In the example</span></span>
```csharp
class Base
{
    public void F() {}
}

class Derived: Base
{
    public void F() {}        // Warning, hiding an inherited name
}
```
<span data-ttu-id="a4c5c-452">`Derived` 中 `F` 的宣告會導致回報警告。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-452">the declaration of `F` in `Derived` causes a warning to be reported.</span></span> <span data-ttu-id="a4c5c-453">隱藏繼承的名稱特別不是錯誤，因為這會排除基類的個別進化。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-453">Hiding an inherited name is specifically not an error, since that would preclude separate evolution of base classes.</span></span> <span data-ttu-id="a4c5c-454">例如，上述情況可能會出現，因為較新版本的 `Base` 引進了不存在於舊版類別中的 @no__t 1 方法。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-454">For example, the above situation might have come about because a later version of `Base` introduced an `F` method that wasn't present in an earlier version of the class.</span></span> <span data-ttu-id="a4c5c-455">如果上述情況是錯誤，則對個別版本化類別庫中的基類所做的任何變更，可能會導致衍生類別變成無效。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-455">Had the above situation been an error, then any change made to a base class in a separately versioned class library could potentially cause derived classes to become invalid.</span></span>

<span data-ttu-id="a4c5c-456">藉由使用 `new` 修飾詞，可以消除隱藏繼承名稱所造成的警告：</span><span class="sxs-lookup"><span data-stu-id="a4c5c-456">The warning caused by hiding an inherited name can be eliminated through use of the `new` modifier:</span></span>
```csharp
class Base
{
    public void F() {}
}

class Derived: Base
{
    new public void F() {}
}
```

<span data-ttu-id="a4c5c-457">@No__t-0 修飾詞表示 `Derived` 中的 `F` 是 "new"，而它實際上是用來隱藏繼承的成員。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-457">The `new` modifier indicates that the `F` in `Derived` is "new", and that it is indeed intended to hide the inherited member.</span></span>

<span data-ttu-id="a4c5c-458">新成員的宣告只會在新成員的範圍內隱藏繼承的成員。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-458">A declaration of a new member hides an inherited member only within the scope of the new member.</span></span>

```csharp
class Base
{
    public static void F() {}
}

class Derived: Base
{
    new private static void F() {}    // Hides Base.F in Derived only
}

class MoreDerived: Derived
{
    static void G() { F(); }          // Invokes Base.F
}
```

<span data-ttu-id="a4c5c-459">在上述範例中，`Derived` 中 `F` 的宣告會隱藏繼承自 `Base` 的 `F`，但因為 `Derived` 中新的 `F` 具有私用存取權，所以其範圍不會延伸至 `MoreDerived`。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-459">In the example above, the declaration of `F` in `Derived` hides the `F` that was inherited from `Base`, but since the new `F` in `Derived` has private access, its scope does not extend to `MoreDerived`.</span></span> <span data-ttu-id="a4c5c-460">因此，`MoreDerived.G` 中的呼叫 `F()` 是有效的，而且將會叫用 `Base.F`。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-460">Thus, the call `F()` in `MoreDerived.G` is valid and will invoke `Base.F`.</span></span>

## <a name="namespace-and-type-names"></a><span data-ttu-id="a4c5c-461">命名空間和類型名稱</span><span class="sxs-lookup"><span data-stu-id="a4c5c-461">Namespace and type names</span></span>

<span data-ttu-id="a4c5c-462">程式中的數個內容需要指定*namespace_name*或*type_name。* C#</span><span class="sxs-lookup"><span data-stu-id="a4c5c-462">Several contexts in a C# program require a *namespace_name* or a *type_name* to be specified.</span></span>

```antlr
namespace_name
    : namespace_or_type_name
    ;

type_name
    : namespace_or_type_name
    ;

namespace_or_type_name
    : identifier type_argument_list?
    | namespace_or_type_name '.' identifier type_argument_list?
    | qualified_alias_member
    ;
```

<span data-ttu-id="a4c5c-463">*Namespace_name*是參考命名空間的*namespace_or_type_name* 。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-463">A *namespace_name* is a *namespace_or_type_name* that refers to a namespace.</span></span> <span data-ttu-id="a4c5c-464">依照下面所述的解決方式， *namespace_name*的*namespace_or_type_name*必須參考命名空間，否則會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-464">Following resolution as described below, the *namespace_or_type_name* of a *namespace_name* must refer to a namespace, or otherwise a compile-time error occurs.</span></span> <span data-ttu-id="a4c5c-465">*Namespace_name*中不能有類型引數（[類型引數](types.md#type-arguments)）（只有類型可以有類型引數）。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-465">No type arguments ([Type arguments](types.md#type-arguments)) can be present in a *namespace_name* (only types can have type arguments).</span></span>

<span data-ttu-id="a4c5c-466">*Type_name*是參考類型的*namespace_or_type_name* 。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-466">A *type_name* is a *namespace_or_type_name* that refers to a type.</span></span> <span data-ttu-id="a4c5c-467">如下所述的解決方法， *type_name*的*namespace_or_type_name*必須參考型別，否則會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-467">Following resolution as described below, the *namespace_or_type_name* of a *type_name* must refer to a type, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="a4c5c-468">如果*namespace_or_type_name*是限定別名成員，則其意義會如[命名空間別名限定詞](namespaces.md#namespace-alias-qualifiers)中所述。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-468">If the *namespace_or_type_name* is a qualified-alias-member its meaning is as described in [Namespace alias qualifiers](namespaces.md#namespace-alias-qualifiers).</span></span> <span data-ttu-id="a4c5c-469">否則， *namespace_or_type_name*會有四種形式的其中一種：</span><span class="sxs-lookup"><span data-stu-id="a4c5c-469">Otherwise, a *namespace_or_type_name* has one of four forms:</span></span>

*  `I`
*  `I<A1, ..., Ak>`
*  `N.I`
*  `N.I<A1, ..., Ak>`

<span data-ttu-id="a4c5c-470">其中 `I` 是單一識別碼，`N` 是*namespace_or_type_name* ，而 `<A1, ..., Ak>` 是選擇性的*type_argument_list*。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-470">where `I` is a single identifier, `N` is a *namespace_or_type_name* and `<A1, ..., Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="a4c5c-471">未指定*type_argument_list*時，請考慮 `k` 為零。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-471">When no *type_argument_list* is specified, consider `k` to be zero.</span></span>

<span data-ttu-id="a4c5c-472">*Namespace_or_type_name*的意義是依照下列方式決定：</span><span class="sxs-lookup"><span data-stu-id="a4c5c-472">The meaning of a *namespace_or_type_name* is determined as follows:</span></span>

*   <span data-ttu-id="a4c5c-473">如果*namespace_or_type_name*的格式為 `I`，或形式 `I<A1, ..., Ak>`：</span><span class="sxs-lookup"><span data-stu-id="a4c5c-473">If the *namespace_or_type_name* is of the form `I` or of the form `I<A1, ..., Ak>`:</span></span>
    * <span data-ttu-id="a4c5c-474">如果 `K` 為零，且*namespace_or_type_name*出現在泛型方法宣告（[方法](classes.md#methods)）中，而且該宣告包含名稱為 @ no__t-4 的型別參數（[型別參數](classes.md#type-parameters)），則*namespace_or_type_名稱*參考該類型參數。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-474">If `K` is zero and the *namespace_or_type_name* appears within a generic method declaration ([Methods](classes.md#methods)) and if that declaration includes a type parameter ([Type parameters](classes.md#type-parameters)) with name `I`, then the *namespace_or_type_name* refers to that type parameter.</span></span>
    * <span data-ttu-id="a4c5c-475">否則，如果*namespace_or_type_name*出現在類型宣告中，則針對每個實例類型 @ no__t-1 （[實例類型](classes.md#the-instance-type)），從該類型宣告的實例類型開始，然後繼續每個的實例類型。封入類別或結構宣告（如果有的話）：</span><span class="sxs-lookup"><span data-stu-id="a4c5c-475">Otherwise, if the *namespace_or_type_name* appears within a type declaration, then for each instance type `T` ([The instance type](classes.md#the-instance-type)), starting with the instance type of that type declaration and continuing with the instance type of each enclosing class or struct declaration (if any):</span></span>
        * <span data-ttu-id="a4c5c-476">如果 `K` 為零，且 `T` 的宣告包含名稱為 @ no__t-2 的類型參數，則*namespace_or_type_name*會參考該類型參數。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-476">If `K` is zero and the declaration of `T` includes a type parameter with name `I`, then the *namespace_or_type_name* refers to that type parameter.</span></span>
        * <span data-ttu-id="a4c5c-477">否則，如果*namespace_or_type_name*出現在類型宣告的主體內，而且 `T` 或其任何基底類型包含具有 name @ no__t-2 和 `K` @ no__t-4type 參數的嵌套可存取型別，則*namespace_or_type_name*是指以指定的型別引數所構成的型別。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-477">Otherwise, if the *namespace_or_type_name* appears within the body of the type declaration, and `T` or any of its base types contain a nested accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span> <span data-ttu-id="a4c5c-478">如果有多個這種類型，則會選取在更多衍生類型內宣告的類型。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-478">If there is more than one such type, the type declared within the more derived type is selected.</span></span> <span data-ttu-id="a4c5c-479">請注意，在決定的意義時，不會忽略類型成員（常數、欄位、方法、屬性、索引子、運算子、實例的函式、析構函式和靜態的函式）和類型成員的類型。*namespace_or_type_name*。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-479">Note that non-type members (constants, fields, methods, properties, indexers, operators, instance constructors, destructors, and static constructors) and type members with a different number of type parameters are ignored when determining the meaning of the *namespace_or_type_name*.</span></span>
    * <span data-ttu-id="a4c5c-480">如果先前的步驟不成功，則針對每個命名空間 @ no__t-0，從*namespace_or_type_name*發生所在的命名空間開始，繼續執行每個封入命名空間（如果有的話），並以全域命名空間結束，如下所示系統會評估步驟，直到實體找到為止：</span><span class="sxs-lookup"><span data-stu-id="a4c5c-480">If the previous steps were unsuccessful then, for each namespace `N`, starting with the namespace in which the *namespace_or_type_name* occurs, continuing with each enclosing namespace (if any), and ending with the global namespace, the following steps are evaluated until an entity is located:</span></span>
        * <span data-ttu-id="a4c5c-481">如果 `K` 為零，且 `I` 是 @ no__t-2 中的命名空間名稱，則：</span><span class="sxs-lookup"><span data-stu-id="a4c5c-481">If `K` is zero and `I` is the name of a namespace in `N`, then:</span></span>
            * <span data-ttu-id="a4c5c-482">如果發生*namespace_or_type_name*的位置是以 `N` 的命名空間宣告括住，且命名空間宣告包含*extern_alias_directive*或*using_alias_directive* ，而該名稱與 @ no 關聯具有命名空間或類型的 __t-4，則*namespace_or_type_name*是不明確的，而且會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-482">If the location where the *namespace_or_type_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *namespace_or_type_name* is ambiguous and a compile-time error occurs.</span></span>
            * <span data-ttu-id="a4c5c-483">否則， *namespace_or_type_name*會參考 `N` 中名為 `I` 的命名空間。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-483">Otherwise, the *namespace_or_type_name* refers to the namespace named `I` in `N`.</span></span>
        * <span data-ttu-id="a4c5c-484">否則，如果 `N` 包含名稱為 @ no__t-1 的可存取類型和 `K` @ no__t-3type 參數，則：</span><span class="sxs-lookup"><span data-stu-id="a4c5c-484">Otherwise, if `N` contains an accessible type having name `I` and `K` type parameters, then:</span></span>
            * <span data-ttu-id="a4c5c-485">如果 `K` 為零，且發生*namespace_or_type_name*的位置是以 `N` 的命名空間宣告括住，且命名空間宣告包含*extern_alias_directive*或*using_alias_directive* ，將名稱 @ no__t-5 關聯至命名空間或類型，然後*namespace_or_type_name*會不明確，且會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-485">If `K` is zero and the location where the *namespace_or_type_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *namespace_or_type_name* is ambiguous and a compile-time error occurs.</span></span>
            * <span data-ttu-id="a4c5c-486">否則， *namespace_or_type_name*會參考以指定的型別引數所構成的型別。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-486">Otherwise, the *namespace_or_type_name* refers to the type constructed with the given type arguments.</span></span>
        * <span data-ttu-id="a4c5c-487">否則，如果發生*namespace_or_type_name*的位置是由 `N` 的命名空間宣告所括住：</span><span class="sxs-lookup"><span data-stu-id="a4c5c-487">Otherwise, if the location where the *namespace_or_type_name* occurs is enclosed by a namespace declaration for `N`:</span></span>
            * <span data-ttu-id="a4c5c-488">如果 `K` 為零，且命名空間宣告包含*extern_alias_directive*或*using_alias_directive* ，其會將名稱 @ no__t-3 與匯入的命名空間或類型產生關聯，則*namespace_or_type_name*會參考該命名空間或類型。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-488">If `K` is zero and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with an imported namespace or type, then the *namespace_or_type_name* refers to that namespace or type.</span></span>
            * <span data-ttu-id="a4c5c-489">否則，如果命名空間宣告的*using_namespace_directive*s 和*using_alias_directive*所匯入的命名空間和類型宣告，只包含一個名稱為 @ no__t-2 且 `K` @ no__t-4type 的可存取類型參數，然後*namespace_or_type_name*會參考以給定類型引數所構成的類型。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-489">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_alias_directive*s of the namespace declaration contain exactly one accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span>
            * <span data-ttu-id="a4c5c-490">否則，如果由命名空間宣告的*using_namespace_directive*s 和*using_alias_directive*所匯入的命名空間和類型宣告包含一個以上名稱為 @ no__t-2 且 `K` @ no__t-4type 的可存取類型參數，則*namespace_or_type_name*不明確，且會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-490">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_alias_directive*s of the namespace declaration contain more than one accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* is ambiguous and an error occurs.</span></span>
    * <span data-ttu-id="a4c5c-491">否則， *namespace_or_type_name*會是未定義的，而且會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-491">Otherwise, the *namespace_or_type_name* is undefined and a compile-time error occurs.</span></span>
*  <span data-ttu-id="a4c5c-492">否則， *namespace_or_type_name*的格式為 `N.I`，或形式 `N.I<A1, ..., Ak>`。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-492">Otherwise, the *namespace_or_type_name* is of the form `N.I` or of the form `N.I<A1, ..., Ak>`.</span></span> <span data-ttu-id="a4c5c-493">`N` 會第一次解析為*namespace_or_type_name*。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-493">`N` is first resolved as a *namespace_or_type_name*.</span></span> <span data-ttu-id="a4c5c-494">如果 `N` 的解決方式不成功，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-494">If the resolution of `N` is not successful, a compile-time error occurs.</span></span> <span data-ttu-id="a4c5c-495">否則，`N.I` 或 `N.I<A1, ..., Ak>` 會解析如下：</span><span class="sxs-lookup"><span data-stu-id="a4c5c-495">Otherwise, `N.I` or `N.I<A1, ..., Ak>` is resolved as follows:</span></span>
    * <span data-ttu-id="a4c5c-496">如果 `K` 為零，且 `N` 參考命名空間，而 `N` 包含名為 `I` 的嵌套命名空間，則*namespace_or_type_name*會參考該 nested 命名空間。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-496">If `K` is zero and `N` refers to a namespace and `N` contains a nested namespace with name `I`, then the *namespace_or_type_name* refers to that nested namespace.</span></span>
    * <span data-ttu-id="a4c5c-497">否則，如果 `N` 指的是命名空間，而 `N` 包含具有 name @ no__t-2 和 `K` @ no__t-4type 參數的可存取型別，則*namespace_or_type_name*會參考以給定型別引數所結構化的型別。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-497">Otherwise, if `N` refers to a namespace and `N` contains an accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span>
    * <span data-ttu-id="a4c5c-498">否則，如果 `N` 指的是（可能是已建立）的類別或結構類型，而 `N` 或其任何基類包含具有 name @ no__t-2 和 `K` @ no__t-4type 參數的嵌套可存取型別，則*namespace_or_type_name*會參考該型別是以指定的型別引數所構成。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-498">Otherwise, if `N` refers to a (possibly constructed) class or struct type and `N` or any of its base classes contain a nested accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span> <span data-ttu-id="a4c5c-499">如果有多個這種類型，則會選取在更多衍生類型內宣告的類型。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-499">If there is more than one such type, the type declared within the more derived type is selected.</span></span> <span data-ttu-id="a4c5c-500">請注意，如果在解析 `N` 的基類規格中判斷 `N.I` 的意義，則 `N` 的直接基類會視為物件（[基類](classes.md#base-classes)）。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-500">Note that if the meaning of `N.I` is being determined as part of resolving the base class specification of `N` then the direct base class of `N` is considered to be object ([Base classes](classes.md#base-classes)).</span></span>
    * <span data-ttu-id="a4c5c-501">否則，`N.I` 是不正確*namespace_or_type_name*，而且發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-501">Otherwise, `N.I` is an invalid *namespace_or_type_name*, and a compile-time error occurs.</span></span>

<span data-ttu-id="a4c5c-502">只有在時，才允許*namespace_or_type_name*參考靜態類別（[靜態類別](classes.md#static-classes)）</span><span class="sxs-lookup"><span data-stu-id="a4c5c-502">A *namespace_or_type_name* is permitted to reference a static class ([Static classes](classes.md#static-classes)) only if</span></span>

*  <span data-ttu-id="a4c5c-503">*Namespace_or_type_name*是 `T.I` 格式的*namespace_or_type_name*中的 `T`，或</span><span class="sxs-lookup"><span data-stu-id="a4c5c-503">The *namespace_or_type_name* is the `T` in a *namespace_or_type_name* of the form `T.I`, or</span></span>
*  <span data-ttu-id="a4c5c-504">*Namespace_or_type_name*是表單`T` *typeof_expression* 的（[引數清單](expressions.md#argument-lists)1）中`typeof(T)`的。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-504">The *namespace_or_type_name* is the `T` in a *typeof_expression* ([Argument lists](expressions.md#argument-lists)1) of the form `typeof(T)`.</span></span>

### <a name="fully-qualified-names"></a><span data-ttu-id="a4c5c-505">完整名稱</span><span class="sxs-lookup"><span data-stu-id="a4c5c-505">Fully qualified names</span></span>

<span data-ttu-id="a4c5c-506">每個命名空間和類型都有***完整名稱***，可唯一識別命名空間或在所有其他專案之間的類型。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-506">Every namespace and type has a ***fully qualified name***, which uniquely identifies the namespace or type amongst all others.</span></span> <span data-ttu-id="a4c5c-507">命名空間或類型的完整名稱 `N` 的決定方式如下：</span><span class="sxs-lookup"><span data-stu-id="a4c5c-507">The fully qualified name of a namespace or type `N` is determined as follows:</span></span>

*  <span data-ttu-id="a4c5c-508">如果 `N` 是全域命名空間的成員，其完整名稱會是 `N`。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-508">If `N` is a member of the global namespace, its fully qualified name is `N`.</span></span>
*  <span data-ttu-id="a4c5c-509">否則，其完整名稱會 `S.N`，其中 `S` 是宣告 `N` 之命名空間或類型的完整名稱。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-509">Otherwise, its fully qualified name is `S.N`, where `S` is the fully qualified name of the namespace or type in which `N` is declared.</span></span>

<span data-ttu-id="a4c5c-510">換句話說，`N` 的完整名稱是從全域命名空間開始，會導致 `N` 之識別碼的完整階層式路徑。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-510">In other words, the fully qualified name of `N` is the complete hierarchical path of identifiers that lead to `N`, starting from the global namespace.</span></span> <span data-ttu-id="a4c5c-511">因為命名空間或類型的每個成員都必須有唯一的名稱，所以命名空間或類型的完整名稱一定是唯一的。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-511">Because every member of a namespace or type must have a unique name, it follows that the fully qualified name of a namespace or type is always unique.</span></span>

<span data-ttu-id="a4c5c-512">下列範例顯示數個命名空間和類型宣告，以及其相關聯的完整名稱。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-512">The example below shows several namespace and type declarations along with their associated fully qualified names.</span></span>
```csharp
class A {}                // A

namespace X               // X
{
    class B               // X.B
    {
        class C {}        // X.B.C
    }

    namespace Y           // X.Y
    {
        class D {}        // X.Y.D
    }
}

namespace X.Y             // X.Y
{
    class E {}            // X.Y.E
}
```

## <a name="automatic-memory-management"></a><span data-ttu-id="a4c5c-513">自動記憶體管理</span><span class="sxs-lookup"><span data-stu-id="a4c5c-513">Automatic memory management</span></span>

<span data-ttu-id="a4c5c-514">C#採用自動記憶體管理，讓開發人員無法手動設定和釋放物件所佔用的記憶體。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-514">C# employs automatic memory management, which frees developers from manually allocating and freeing the memory occupied by objects.</span></span> <span data-ttu-id="a4c5c-515">自動記憶體管理原則是由***垃圾收集***行程所執行。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-515">Automatic memory management policies are implemented by a ***garbage collector***.</span></span> <span data-ttu-id="a4c5c-516">物件的記憶體管理生命週期如下所示：</span><span class="sxs-lookup"><span data-stu-id="a4c5c-516">The memory management life cycle of an object is as follows:</span></span>

1. <span data-ttu-id="a4c5c-517">建立物件時，系統會為其配置記憶體、執行此函式，並將物件視為即時。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-517">When the object is created, memory is allocated for it, the constructor is run, and the object is considered live.</span></span>
2. <span data-ttu-id="a4c5c-518">如果任何可能的執行接續都無法存取物件（或其中的任何部分），則會將物件視為不再使用，而且它會變成符合終結的資格。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-518">If the object, or any part of it, cannot be accessed by any possible continuation of execution, other than the running of destructors, the object is considered no longer in use, and it becomes eligible for destruction.</span></span> <span data-ttu-id="a4c5c-519">C#編譯器和垃圾收集行程可能會選擇分析程式碼，以判斷未來可能會使用的物件參考。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-519">The C# compiler and the garbage collector may choose to analyze code to determine which references to an object may be used in the future.</span></span> <span data-ttu-id="a4c5c-520">比方說，如果範圍內的區域變數是物件的唯一現有參考，但在程式的目前執行點執行任何可能的接續時，該區域變數絕對不會被參照，垃圾收集行程可能會（但不是必要）將物件視為不再使用。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-520">For instance, if a local variable that is in scope is the only existing reference to an object, but that local variable is never referred to in any possible continuation of execution from the current execution point in the procedure, the garbage collector may (but is not required to) treat the object as no longer in use.</span></span>
3. <span data-ttu-id="a4c5c-521">一旦物件符合終結條件的資格，在某些情況下，物件的析構函式（[析構](classes.md#destructors)函數）（如果有的話）會在未指定的時間執行。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-521">Once the object is eligible for destruction, at some unspecified later time the destructor ([Destructors](classes.md#destructors)) (if any) for the object is run.</span></span> <span data-ttu-id="a4c5c-522">在正常情況下，物件的析構函式只會執行一次，但執行特定的 Api 可能會允許覆寫此行為。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-522">Under normal circumstances the destructor for the object is run once only, though implementation-specific APIs may allow this behavior to be overridden.</span></span>
4. <span data-ttu-id="a4c5c-523">一旦執行物件的析構函式之後，如果該物件或其中的任何部分無法透過任何可能的接續來存取（包括執行緒的執行），則會將物件視為無法存取，而且物件會變成符合集合的資格。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-523">Once the destructor for an object is run, if that object, or any part of it, cannot be accessed by any possible continuation of execution, including the running of destructors, the object is considered inaccessible and the object becomes eligible for collection.</span></span>
5. <span data-ttu-id="a4c5c-524">最後，在物件變成可進行回收的一段時間後，垃圾收集行程就會釋出與該物件相關聯的記憶體。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-524">Finally, at some time after the object becomes eligible for collection, the garbage collector frees the memory associated with that object.</span></span>

<span data-ttu-id="a4c5c-525">垃圾收集行程會維護物件使用方式的相關資訊，並使用這項資訊來進行記憶體管理決策，例如在記憶體中尋找新建立的物件、何時重新放置物件，以及何時不再使用或無法存取物件。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-525">The garbage collector maintains information about object usage, and uses this information to make memory management decisions, such as where in memory to locate a newly created object, when to relocate an object, and when an object is no longer in use or inaccessible.</span></span>

<span data-ttu-id="a4c5c-526">就像其他假設垃圾收集行程存在的語言一樣， C#是設計成讓垃圾收集行程能夠執行各式各樣的記憶體管理原則。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-526">Like other languages that assume the existence of a garbage collector, C# is designed so that the garbage collector may implement a wide range of memory management policies.</span></span> <span data-ttu-id="a4c5c-527">例如， C#不需要執行析構函數，或在物件符合資格時立即予以收集，或是以任何特定的順序或在任何特定的執行緒上執行該析構函數。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-527">For instance, C# does not require that destructors be run or that objects be collected as soon as they are eligible, or that destructors be run in any particular order, or on any particular thread.</span></span>

<span data-ttu-id="a4c5c-528">您可以透過類別上的靜態方法，將垃圾收集行程的行為控制到某種程度，`System.GC`。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-528">The behavior of the garbage collector can be controlled, to some degree, via static methods on the class `System.GC`.</span></span> <span data-ttu-id="a4c5c-529">這個類別可以用來要求要進行的集合、要執行的析構函數（或不執行）等等。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-529">This class can be used to request a collection to occur, destructors to be run (or not run), and so forth.</span></span>

<span data-ttu-id="a4c5c-530">由於垃圾收集行程允許寬緯度來決定收集物件和執行析構函數的時機，因此符合規範的執行可能會產生與下列程式碼所示不同的輸出。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-530">Since the garbage collector is allowed wide latitude in deciding when to collect objects and run destructors, a conforming implementation may produce output that differs from that shown by the following code.</span></span> <span data-ttu-id="a4c5c-531">程式</span><span class="sxs-lookup"><span data-stu-id="a4c5c-531">The program</span></span>
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("Destruct instance of A");
    }
}

class B
{
    object Ref;

    public B(object o) {
        Ref = o;
    }

    ~B() {
        Console.WriteLine("Destruct instance of B");
    }
}

class Test
{
    static void Main() {
        B b = new B(new A());
        b = null;
        GC.Collect();
        GC.WaitForPendingFinalizers();
    }
}
```
<span data-ttu-id="a4c5c-532">建立類別的實例 `A` 和類別的實例 `B`。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-532">creates an instance of class `A` and an instance of class `B`.</span></span> <span data-ttu-id="a4c5c-533">當指派 `null` 的值給 `b` 的變數時，這些物件就會成為垃圾收集的資格，因為在這段時間之後，任何使用者撰寫的程式碼都無法存取它們。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-533">These objects become eligible for garbage collection when the variable `b` is assigned the value `null`, since after this time it is impossible for any user-written code to access them.</span></span> <span data-ttu-id="a4c5c-534">輸出可以是</span><span class="sxs-lookup"><span data-stu-id="a4c5c-534">The output could be either</span></span>

```console
Destruct instance of A
Destruct instance of B
```
<span data-ttu-id="a4c5c-535">或</span><span class="sxs-lookup"><span data-stu-id="a4c5c-535">or</span></span>
```console
Destruct instance of B
Destruct instance of A
```
<span data-ttu-id="a4c5c-536">因為此語言對物件進行垃圾收集的順序沒有限制。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-536">because the language imposes no constraints on the order in which objects are garbage collected.</span></span>

<span data-ttu-id="a4c5c-537">在微妙的情況下，「符合終結資格」和「符合集合資格」的差異可能很重要。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-537">In subtle cases, the distinction between "eligible for destruction" and "eligible for collection" can be important.</span></span> <span data-ttu-id="a4c5c-538">例如，套用至物件的</span><span class="sxs-lookup"><span data-stu-id="a4c5c-538">For example,</span></span>
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("Destruct instance of A");
    }

    public void F() {
        Console.WriteLine("A.F");
        Test.RefA = this;
    }
}

class B
{
    public A Ref;

    ~B() {
        Console.WriteLine("Destruct instance of B");
        Ref.F();
    }
}

class Test
{
    public static A RefA;
    public static B RefB;

    static void Main() {
        RefB = new B();
        RefA = new A();
        RefB.Ref = RefA;
        RefB = null;
        RefA = null;

        // A and B now eligible for destruction
        GC.Collect();
        GC.WaitForPendingFinalizers();

        // B now eligible for collection, but A is not
        if (RefA != null)
            Console.WriteLine("RefA is not null");
    }
}
```

<span data-ttu-id="a4c5c-539">在上述程式中，如果垃圾收集行程選擇在 `B` 的析構函式之前執行 `A` 的析構函數，則此程式的輸出可能會是：</span><span class="sxs-lookup"><span data-stu-id="a4c5c-539">In the above program, if the garbage collector chooses to run the destructor of `A` before the destructor of `B`, then the output of this program might be:</span></span>
```console
Destruct instance of A
Destruct instance of B
A.F
RefA is not null
```

<span data-ttu-id="a4c5c-540">請注意，雖然 `A` 的實例不在使用中，且已執行 `A` 的析構函式，但仍有可能從另一個析構函式呼叫 `A` （在此案例中為 `F`）的方法。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-540">Note that although the instance of `A` was not in use and `A`'s destructor was run, it is still possible for methods of `A` (in this case, `F`) to be called from another destructor.</span></span> <span data-ttu-id="a4c5c-541">此外，請注意，執行析構函式可能會再次導致物件可從主執行緒序中使用。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-541">Also, note that running of a destructor may cause an object to become usable from the mainline program again.</span></span> <span data-ttu-id="a4c5c-542">在此情況下，執行 @no__t 0 的「析構函式」會導致先前未使用的 `A` 實例變成可從即時參考 `Test.RefA` 存取。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-542">In this case, the running of `B`'s destructor caused an instance of `A` that was previously not in use to become accessible from the live reference `Test.RefA`.</span></span> <span data-ttu-id="a4c5c-543">在呼叫 `WaitForPendingFinalizers` 之後，`B` 的實例就適合進行集合，但 `A` 的實例則不是，因為參考 `Test.RefA`。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-543">After the call to `WaitForPendingFinalizers`, the instance of `B` is eligible for collection, but the instance of `A` is not, because of the reference `Test.RefA`.</span></span>

<span data-ttu-id="a4c5c-544">為了避免混淆和非預期的行為，讓析構函數只能對儲存在其物件本身欄位中的資料執行清除，而不是在參考的物件或靜態欄位上執行任何動作，通常是個不錯的主意。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-544">To avoid confusion and unexpected behavior, it is generally a good idea for destructors to only perform cleanup on data stored in their object's own fields, and not to perform any actions on referenced objects or static fields.</span></span>

<span data-ttu-id="a4c5c-545">使用析構函數的替代方法是讓類別實作為 @no__t 0 介面。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-545">An alternative to using destructors is to let a class implement the `System.IDisposable` interface.</span></span> <span data-ttu-id="a4c5c-546">這可讓物件的用戶端判斷何時釋放物件的資源，通常是以 @no__t 0 的語句（[using 語句](statements.md#the-using-statement)）中的資源存取物件。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-546">This allows the client of the object to determine when to release the resources of the object, typically by accessing the object as a resource in a `using` statement ([The using statement](statements.md#the-using-statement)).</span></span>

## <a name="execution-order"></a><span data-ttu-id="a4c5c-547">執行順序</span><span class="sxs-lookup"><span data-stu-id="a4c5c-547">Execution order</span></span>

<span data-ttu-id="a4c5c-548">執行C#程式會繼續進行，讓每個執行中線程的副作用都會保留在關鍵執行點上。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-548">Execution of a C# program proceeds such that the side effects of each executing thread are preserved at critical execution points.</span></span> <span data-ttu-id="a4c5c-549">***副作用***會定義為 volatile 欄位的讀取或寫入、對非 volatile 變數的寫入、對外部資源的寫入，以及擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-549">A ***side effect*** is defined as a read or write of a volatile field, a write to a non-volatile variable, a write to an external resource, and the throwing of an exception.</span></span> <span data-ttu-id="a4c5c-550">這些副作用的順序必須保留的關鍵執行點是變動性欄位（[volatile 欄位](classes.md#volatile-fields)）、@no__t 1 語句（[lock 語句](statements.md#the-lock-statement)）和執行緒建立和終止的參考。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-550">The critical execution points at which the order of these side effects must be preserved are references to volatile fields ([Volatile fields](classes.md#volatile-fields)), `lock` statements ([The lock statement](statements.md#the-lock-statement)), and thread creation and termination.</span></span> <span data-ttu-id="a4c5c-551">執行環境可以自由變更C#程式的執行順序，但受限於下列條件約束：</span><span class="sxs-lookup"><span data-stu-id="a4c5c-551">The execution environment is free to change the order of execution of a C# program, subject to the following constraints:</span></span>

*  <span data-ttu-id="a4c5c-552">資料相關性會在執行的執行緒中保留。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-552">Data dependence is preserved within a thread of execution.</span></span> <span data-ttu-id="a4c5c-553">也就是說，每個變數的值都會計算，就像執行緒中的所有語句都是以原始程式循序執行一樣。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-553">That is, the value of each variable is computed as if all statements in the thread were executed in original program order.</span></span>
*  <span data-ttu-id="a4c5c-554">系統會保留初始化順序規則（[欄位初始化](classes.md#field-initialization)和[變數初始化運算式](classes.md#variable-initializers)）。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-554">Initialization ordering rules are preserved ([Field initialization](classes.md#field-initialization) and [Variable initializers](classes.md#variable-initializers)).</span></span>
*  <span data-ttu-id="a4c5c-555">副作用的順序會根據變動性讀取和寫入（[volatile 欄位](classes.md#volatile-fields)）來保留。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-555">The ordering of side effects is preserved with respect to volatile reads and writes ([Volatile fields](classes.md#volatile-fields)).</span></span> <span data-ttu-id="a4c5c-556">此外，如果執行環境可以推算運算式的值未使用，而且不會產生任何所需的副作用（包括任何因呼叫方法或存取 volatile 欄位所造成的結果），則不需要評估運算式的一部分。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-556">Additionally, the execution environment need not evaluate part of an expression if it can deduce that that expression's value is not used and that no needed side effects are produced (including any caused by calling a method or accessing a volatile field).</span></span> <span data-ttu-id="a4c5c-557">當非同步事件中斷程式執行時（例如由另一個執行緒擲回的例外狀況），不保證會在原始程式順序中看到可觀察的副作用。</span><span class="sxs-lookup"><span data-stu-id="a4c5c-557">When program execution is interrupted by an asynchronous event (such as an exception thrown by another thread), it is not guaranteed that the observable side effects are visible in the original program order.</span></span>
