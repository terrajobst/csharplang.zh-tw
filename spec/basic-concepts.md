---
ms.openlocfilehash: 1c3d05674f8f7b69e70e0d9e06021537fc45f7ed
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229557"
---
# <a name="basic-concepts"></a><span data-ttu-id="b58e2-101">基本概念</span><span class="sxs-lookup"><span data-stu-id="b58e2-101">Basic concepts</span></span>

## <a name="application-startup"></a><span data-ttu-id="b58e2-102">應用程式啟動</span><span class="sxs-lookup"><span data-stu-id="b58e2-102">Application Startup</span></span>

<span data-ttu-id="b58e2-103">組件具有***進入點***稱為***應用程式***。</span><span class="sxs-lookup"><span data-stu-id="b58e2-103">An assembly that has an ***entry point*** is called an ***application***.</span></span> <span data-ttu-id="b58e2-104">應用程式時執行，新***應用程式定義域***建立。</span><span class="sxs-lookup"><span data-stu-id="b58e2-104">When an application is run, a new ***application domain*** is created.</span></span> <span data-ttu-id="b58e2-105">數個不同的具現化的應用程式可能存在同一部電腦上，在此同時，而且各有自己的應用程式定義域。</span><span class="sxs-lookup"><span data-stu-id="b58e2-105">Several different instantiations of an application may exist on the same machine at the same time, and each has its own application domain.</span></span>

<span data-ttu-id="b58e2-106">應用程式定義域做為應用程式狀態的容器，以啟用應用程式隔離。</span><span class="sxs-lookup"><span data-stu-id="b58e2-106">An application domain enables application isolation by acting as a container for application state.</span></span> <span data-ttu-id="b58e2-107">應用程式定義域做為容器和應用程式和它所使用的類別庫中定義之類型的界限。</span><span class="sxs-lookup"><span data-stu-id="b58e2-107">An application domain acts as a container and boundary for the types defined in the application and the class libraries it uses.</span></span> <span data-ttu-id="b58e2-108">一個應用程式定義域所載入的型別是不同的載入另一個應用程式定義域，相同的型別和物件的執行個體無法直接共用應用程式定義域之間。</span><span class="sxs-lookup"><span data-stu-id="b58e2-108">Types loaded into one application domain are distinct from the same type loaded into another application domain, and instances of objects are not directly shared between application domains.</span></span> <span data-ttu-id="b58e2-109">比方說，每個應用程式定義域都有自己一份靜態變數，針對這些類型，和類型的靜態建構函式執行一次，每個應用程式網域。</span><span class="sxs-lookup"><span data-stu-id="b58e2-109">For instance, each application domain has its own copy of static variables for these types, and a static constructor for a type is run at most once per application domain.</span></span> <span data-ttu-id="b58e2-110">實作可自行提供實作特定原則或機制的建立和解構的應用程式定義域。</span><span class="sxs-lookup"><span data-stu-id="b58e2-110">Implementations are free to provide implementation-specific policy or mechanisms for the creation and destruction of application domains.</span></span>

<span data-ttu-id="b58e2-111">***應用程式啟動***發生於執行環境會呼叫指定的方法，指應用程式的進入點。</span><span class="sxs-lookup"><span data-stu-id="b58e2-111">***Application startup*** occurs when the execution environment calls a designated method, which is referred to as the application's entry point.</span></span> <span data-ttu-id="b58e2-112">此進入點方法的名稱永遠`Main`，且可以有一個下列簽章：</span><span class="sxs-lookup"><span data-stu-id="b58e2-112">This entry point method is always named `Main`, and can have one of the following signatures:</span></span>

```csharp
static void Main() {...}

static void Main(string[] args) {...}

static int Main() {...}

static int Main(string[] args) {...}
```

<span data-ttu-id="b58e2-113">如所示，進入點可以選擇性地傳回`int`值。</span><span class="sxs-lookup"><span data-stu-id="b58e2-113">As shown, the entry point may optionally return an `int` value.</span></span> <span data-ttu-id="b58e2-114">這會傳回值會在應用程式終止 ([應用程式終止](basic-concepts.md#application-termination))。</span><span class="sxs-lookup"><span data-stu-id="b58e2-114">This return value is used in application termination ([Application termination](basic-concepts.md#application-termination)).</span></span>

<span data-ttu-id="b58e2-115">進入點可能會選擇一個型式參數。</span><span class="sxs-lookup"><span data-stu-id="b58e2-115">The entry point may optionally have one formal parameter.</span></span> <span data-ttu-id="b58e2-116">參數可能會有任何名稱，但必須是參數的型別`string[]`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-116">The parameter may have any name, but the type of the parameter must be `string[]`.</span></span> <span data-ttu-id="b58e2-117">如果有型式參數，執行環境建立並傳遞`string[]`引數包含命令列引數所指定的應用程式啟動時。</span><span class="sxs-lookup"><span data-stu-id="b58e2-117">If the formal parameter is present, the execution environment creates and passes a `string[]` argument containing the command-line arguments that were specified when the application was started.</span></span> <span data-ttu-id="b58e2-118">`string[]`引數絕不會是 null，但它可能會有長度為零，如果未不指定任何命令列引數。</span><span class="sxs-lookup"><span data-stu-id="b58e2-118">The `string[]` argument is never null, but it may have a length of zero if no command-line arguments were specified.</span></span>

<span data-ttu-id="b58e2-119">由於 C# 支援多載的方法，類別或結構可能包含多個定義的一些方法，都各自具有不同的簽章。</span><span class="sxs-lookup"><span data-stu-id="b58e2-119">Since C# supports method overloading, a class or struct may contain multiple definitions of some method, provided each has a different signature.</span></span> <span data-ttu-id="b58e2-120">不過，在單一程式中，任何類別或結構可以包含多個方法呼叫`Main`定義已限定它用做為應用程式進入點。</span><span class="sxs-lookup"><span data-stu-id="b58e2-120">However, within a single program, no class or struct may contain more than one method called `Main` whose definition qualifies it to be used as an application entry point.</span></span> <span data-ttu-id="b58e2-121">其他多載的版本`Main`可以，不過，提供了一個以上的參數，或其唯一參數是型別以外`string[]`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-121">Other overloaded versions of `Main` are permitted, however, provided they have more than one parameter, or their only parameter is other than type `string[]`.</span></span>

<span data-ttu-id="b58e2-122">應用程式可以組成多個類別或結構。</span><span class="sxs-lookup"><span data-stu-id="b58e2-122">An application can be made up of multiple classes or structs.</span></span> <span data-ttu-id="b58e2-123">您有一個以上的這些類別或結構以包含呼叫的方法`Main`定義已限定它用做為應用程式進入點。</span><span class="sxs-lookup"><span data-stu-id="b58e2-123">It is possible for more than one of these classes or structs to contain a method called `Main` whose definition qualifies it to be used as an application entry point.</span></span> <span data-ttu-id="b58e2-124">在此情況下，必須使用外部機制 （例如命令列編譯器選項） 來選取其中一種`Main`做為進入點方法。</span><span class="sxs-lookup"><span data-stu-id="b58e2-124">In such cases, an external mechanism (such as a command-line compiler option) must be used to select one of these `Main` methods as the entry point.</span></span>

<span data-ttu-id="b58e2-125">在 C# 中，每個方法都必須定義為類別或結構的成員。</span><span class="sxs-lookup"><span data-stu-id="b58e2-125">In C#, every method must be defined as a member of a class or struct.</span></span> <span data-ttu-id="b58e2-126">一般情況下，宣告存取範圍 ([宣告存取範圍](basic-concepts.md#declared-accessibility)) 的方法取決於存取修飾詞 ([存取修飾詞](classes.md#access-modifiers)) 中指定其宣告，但同樣的宣告型別的存取範圍取決於其宣告中指定的存取修飾詞。</span><span class="sxs-lookup"><span data-stu-id="b58e2-126">Ordinarily, the declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)) of a method is determined by the access modifiers ([Access modifiers](classes.md#access-modifiers)) specified in its declaration, and similarly the declared accessibility of a type is determined by the access modifiers specified in its declaration.</span></span> <span data-ttu-id="b58e2-127">為了指定型別的指定方法要能夠呼叫，型別和成員必須是可存取。</span><span class="sxs-lookup"><span data-stu-id="b58e2-127">In order for a given method of a given type to be callable, both the type and the member must be accessible.</span></span> <span data-ttu-id="b58e2-128">不過，應用程式進入點是特殊案例。</span><span class="sxs-lookup"><span data-stu-id="b58e2-128">However, the application entry point is a special case.</span></span> <span data-ttu-id="b58e2-129">具體來說，執行環境可以存取應用程式的進入點，無論其已宣告存取範圍，也不論其封入型別宣告的宣告存取範圍。</span><span class="sxs-lookup"><span data-stu-id="b58e2-129">Specifically, the execution environment can access the application's entry point regardless of its declared accessibility and regardless of the declared accessibility of its enclosing type declarations.</span></span>

<span data-ttu-id="b58e2-130">應用程式進入點方法可能不在泛型類別宣告中。</span><span class="sxs-lookup"><span data-stu-id="b58e2-130">The application entry point method may not be in a generic class declaration.</span></span>

<span data-ttu-id="b58e2-131">在其他方面，進入點方法的行為類似的不是進入點。</span><span class="sxs-lookup"><span data-stu-id="b58e2-131">In all other respects, entry point methods behave like those that are not entry points.</span></span>

## <a name="application-termination"></a><span data-ttu-id="b58e2-132">應用程式終止</span><span class="sxs-lookup"><span data-stu-id="b58e2-132">Application termination</span></span>

<span data-ttu-id="b58e2-133">***應用程式終止***將控制權傳回給執行環境。</span><span class="sxs-lookup"><span data-stu-id="b58e2-133">***Application termination*** returns control to the execution environment.</span></span>

<span data-ttu-id="b58e2-134">如果傳回類型的應用程式***進入點***方式`int`，傳回的值做為應用程式的***終止狀態碼***。</span><span class="sxs-lookup"><span data-stu-id="b58e2-134">If the return type of the application's ***entry point*** method is `int`, the value returned serves as the application's ***termination status code***.</span></span> <span data-ttu-id="b58e2-135">此程式碼的目的是允許的成功或失敗的執行環境的通訊。</span><span class="sxs-lookup"><span data-stu-id="b58e2-135">The purpose of this code is to allow communication of success or failure to the execution environment.</span></span>

<span data-ttu-id="b58e2-136">進入點方法的傳回型別是否`void`，到達右大括號 (`}`)，這會終止方法，或執行`return`陳述式沒有運算式，會導致終止狀態碼`0`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-136">If the return type of the entry point method is `void`, reaching the right brace (`}`) which terminates that method, or executing a `return` statement that has no expression, results in a termination status code of `0`.</span></span>

<span data-ttu-id="b58e2-137">應用程式的終止前的所有其尚未經過記憶體回收的物件解構函式呼叫時，除非已隱藏這類清除 (程式庫方法呼叫`GC.SuppressFinalize`，例如)。</span><span class="sxs-lookup"><span data-stu-id="b58e2-137">Prior to an application's termination, destructors for all of its objects that have not yet been garbage collected are called, unless such cleanup has been suppressed (by a call to the library method `GC.SuppressFinalize`, for example).</span></span>

## <a name="declarations"></a><span data-ttu-id="b58e2-138">宣告</span><span class="sxs-lookup"><span data-stu-id="b58e2-138">Declarations</span></span>

<span data-ttu-id="b58e2-139">在 C# 程式中的宣告會定義程式的組成項目。</span><span class="sxs-lookup"><span data-stu-id="b58e2-139">Declarations in a C# program define the constituent elements of the program.</span></span> <span data-ttu-id="b58e2-140">C# 程式組織方式使用命名空間 ([命名空間](namespaces.md))，其中可以包含類型宣告和巢狀命名空間宣告。</span><span class="sxs-lookup"><span data-stu-id="b58e2-140">C# programs are organized using namespaces ([Namespaces](namespaces.md)), which can contain type declarations and nested namespace declarations.</span></span> <span data-ttu-id="b58e2-141">型別宣告 ([型別宣告](namespaces.md#type-declarations)) 用來定義類別 ([類別](classes.md))，結構 ([結構](structs.md))，介面 ([介面](interfaces.md))，列舉 ([列舉](enums.md))，和委派 ([委派](delegates.md))。</span><span class="sxs-lookup"><span data-stu-id="b58e2-141">Type declarations ([Type declarations](namespaces.md#type-declarations)) are used to define classes ([Classes](classes.md)), structs ([Structs](structs.md)), interfaces ([Interfaces](interfaces.md)), enums ([Enums](enums.md)), and delegates ([Delegates](delegates.md)).</span></span> <span data-ttu-id="b58e2-142">型別宣告中允許的成員類型取決於型別宣告的形式。</span><span class="sxs-lookup"><span data-stu-id="b58e2-142">The kinds of members permitted in a type declaration depend on the form of the type declaration.</span></span> <span data-ttu-id="b58e2-143">比方說，類別的宣告可以包含常數的宣告 ([常數](classes.md#constants))，欄位 ([欄位](classes.md#fields))，方法 ([方法](classes.md#methods))，屬性 ([屬性](classes.md#properties))，事件 ([事件](classes.md#events))，索引子 ([的索引子](classes.md#indexers))，運算子 ([運算子](classes.md#operators))，執行個體建構函式 ([執行個體建構函式](classes.md#instance-constructors))，靜態建構函式 ([靜態建構函式](classes.md#static-constructors))，解構函式 ([解構函式](classes.md#destructors))，以及巢狀類型 ([巢狀型別](classes.md#nested-types)).</span><span class="sxs-lookup"><span data-stu-id="b58e2-143">For instance, class declarations can contain declarations for constants ([Constants](classes.md#constants)), fields ([Fields](classes.md#fields)), methods ([Methods](classes.md#methods)), properties ([Properties](classes.md#properties)), events ([Events](classes.md#events)), indexers ([Indexers](classes.md#indexers)), operators ([Operators](classes.md#operators)), instance constructors ([Instance constructors](classes.md#instance-constructors)), static constructors ([Static constructors](classes.md#static-constructors)), destructors ([Destructors](classes.md#destructors)), and nested types ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="b58e2-144">宣告會定義中的名稱***宣告空間***所屬的宣告。</span><span class="sxs-lookup"><span data-stu-id="b58e2-144">A declaration defines a name in the ***declaration space*** to which the declaration belongs.</span></span> <span data-ttu-id="b58e2-145">除了多載成員 ([簽章和多載](basic-concepts.md#signatures-and-overloading))，它是編譯時期錯誤有兩個或多個宣告引入的宣告空間中同名的成員。</span><span class="sxs-lookup"><span data-stu-id="b58e2-145">Except for overloaded members ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)), it is a compile-time error to have two or more declarations that introduce members with the same name in a declaration space.</span></span> <span data-ttu-id="b58e2-146">您不可能包含不同類型的成員具有相同名稱的宣告空間。</span><span class="sxs-lookup"><span data-stu-id="b58e2-146">It is never possible for a declaration space to contain different kinds of members with the same name.</span></span> <span data-ttu-id="b58e2-147">例如，宣告空間可以永遠不會包含欄位和方法，以相同的名稱。</span><span class="sxs-lookup"><span data-stu-id="b58e2-147">For example, a declaration space can never contain a field and a method by the same name.</span></span>

<span data-ttu-id="b58e2-148">有許多不同類型的宣告空間，如下列所述。</span><span class="sxs-lookup"><span data-stu-id="b58e2-148">There are several different types of declaration spaces, as described in the following.</span></span>

*  <span data-ttu-id="b58e2-149">內的程式，所有的原始程式檔*namespace_member_declaration*秒，沒有封入*namespace_declaration*屬於稱為單一合併的宣告空間***全域宣告空間***。</span><span class="sxs-lookup"><span data-stu-id="b58e2-149">Within all source files of a program, *namespace_member_declaration*s with no enclosing *namespace_declaration* are members of a single combined declaration space called the ***global declaration space***.</span></span>
*  <span data-ttu-id="b58e2-150">中的程式，所有的原始程式檔*namespace_member_declaration*內*namespace_declaration*具有相同的完整命名空間名稱都是單一的結合宣告的成員空間。</span><span class="sxs-lookup"><span data-stu-id="b58e2-150">Within all source files of a program, *namespace_member_declaration*s within *namespace_declaration*s that have the same fully qualified namespace name are members of a single combined declaration space.</span></span>
*  <span data-ttu-id="b58e2-151">每個類別、 結構或介面宣告會建立新的宣告空間。</span><span class="sxs-lookup"><span data-stu-id="b58e2-151">Each class, struct, or interface declaration creates a new declaration space.</span></span> <span data-ttu-id="b58e2-152">名稱會導入此宣告空間*class_member_declaration*s *struct_member_declaration*s *interface_member_declaration*s，或*type_parameter*s。</span><span class="sxs-lookup"><span data-stu-id="b58e2-152">Names are introduced into this declaration space through *class_member_declaration*s, *struct_member_declaration*s, *interface_member_declaration*s, or *type_parameter*s.</span></span> <span data-ttu-id="b58e2-153">除了多載的執行個體建構函式宣告和靜態建構函式宣告、 類別或結構不能包含具有相同名稱的類別或結構宣告的成員。</span><span class="sxs-lookup"><span data-stu-id="b58e2-153">Except for overloaded instance constructor declarations and static constructor declarations, a class or struct cannot contain a member declaration with the same name as the class or struct.</span></span> <span data-ttu-id="b58e2-154">類別、 結構或介面允許多載的方法和索引子的宣告。</span><span class="sxs-lookup"><span data-stu-id="b58e2-154">A class, struct, or interface permits the declaration of overloaded methods and indexers.</span></span> <span data-ttu-id="b58e2-155">此外，類別或結構允許多載的執行個體建構函式和運算子的宣告。</span><span class="sxs-lookup"><span data-stu-id="b58e2-155">Furthermore, a class or struct permits the declaration of overloaded instance constructors and operators.</span></span> <span data-ttu-id="b58e2-156">比方說，類別、 結構或介面可能包含多個方法宣告具有相同的名稱，提供這些方法宣告不同，其簽章中 ([簽章和多載](basic-concepts.md#signatures-and-overloading))。</span><span class="sxs-lookup"><span data-stu-id="b58e2-156">For example, a class, struct, or interface may contain multiple method declarations with the same name, provided these method declarations differ in their signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)).</span></span> <span data-ttu-id="b58e2-157">請注意，類別的宣告空間並不會提供基底類別的基底介面並不會提供介面的宣告空間。</span><span class="sxs-lookup"><span data-stu-id="b58e2-157">Note that base classes do not contribute to the declaration space of a class, and base interfaces do not contribute to the declaration space of an interface.</span></span> <span data-ttu-id="b58e2-158">因此，衍生的類別或介面允許宣告具有相同名稱的成員，做為繼承的成員。</span><span class="sxs-lookup"><span data-stu-id="b58e2-158">Thus, a derived class or interface is allowed to declare a member with the same name as an inherited member.</span></span> <span data-ttu-id="b58e2-159">這類成員即為***隱藏***繼承的成員。</span><span class="sxs-lookup"><span data-stu-id="b58e2-159">Such a member is said to ***hide*** the inherited member.</span></span>
*  <span data-ttu-id="b58e2-160">每個委派宣告會建立新的宣告空間。</span><span class="sxs-lookup"><span data-stu-id="b58e2-160">Each delegate declaration creates a new declaration space.</span></span> <span data-ttu-id="b58e2-161">名稱會導入此型式參數的宣告空間 (*fixed_parameter*s 並*parameter_array*s) 和*type_parameter*s。</span><span class="sxs-lookup"><span data-stu-id="b58e2-161">Names are introduced into this declaration space through formal parameters (*fixed_parameter*s and *parameter_array*s) and *type_parameter*s.</span></span>
*  <span data-ttu-id="b58e2-162">每個列舉型別宣告會建立新的宣告空間。</span><span class="sxs-lookup"><span data-stu-id="b58e2-162">Each enumeration declaration creates a new declaration space.</span></span> <span data-ttu-id="b58e2-163">名稱會導入此宣告空間*enum_member_declarations*。</span><span class="sxs-lookup"><span data-stu-id="b58e2-163">Names are introduced into this declaration space through *enum_member_declarations*.</span></span>
*  <span data-ttu-id="b58e2-164">每個方法宣告、 宣告索引子、 運算子宣告、 執行個體建構函式宣告和匿名函式會建立新的宣告空間，稱為***區域變數宣告空間***。</span><span class="sxs-lookup"><span data-stu-id="b58e2-164">Each method declaration, indexer declaration, operator declaration, instance constructor declaration and anonymous function creates a new declaration space called a ***local variable declaration space***.</span></span> <span data-ttu-id="b58e2-165">名稱會導入此型式參數的宣告空間 (*fixed_parameter*s 並*parameter_array*s) 和*type_parameter*s。</span><span class="sxs-lookup"><span data-stu-id="b58e2-165">Names are introduced into this declaration space through formal parameters (*fixed_parameter*s and *parameter_array*s) and *type_parameter*s.</span></span> <span data-ttu-id="b58e2-166">主體的函式成員 」 或 「 匿名函式，如果有的話，視為巢狀於本機變數的宣告空間。</span><span class="sxs-lookup"><span data-stu-id="b58e2-166">The body of the function member or anonymous function, if any, is considered to be nested within the local variable declaration space.</span></span> <span data-ttu-id="b58e2-167">它是本機變數的宣告空間和巢狀的本機變數宣告空間，以包含具有相同名稱的項目時發生。</span><span class="sxs-lookup"><span data-stu-id="b58e2-167">It is an error for a local variable declaration space and a nested local variable declaration space to contain elements with the same name.</span></span> <span data-ttu-id="b58e2-168">因此，在巢狀的宣告空間內不可以在封入的宣告空間中宣告本機變數或常數，其名稱相同的本機變數或常數。</span><span class="sxs-lookup"><span data-stu-id="b58e2-168">Thus, within a nested declaration space it is not possible to declare a local variable or constant with the same name as a local variable or constant in an enclosing declaration space.</span></span> <span data-ttu-id="b58e2-169">可以包含具有相同名稱的項目，只要空間不包含其他兩個宣告空間。</span><span class="sxs-lookup"><span data-stu-id="b58e2-169">It is possible for two declaration spaces to contain elements with the same name as long as neither declaration space contains the other.</span></span>
*  <span data-ttu-id="b58e2-170">每個*區塊*或*switch_block* ，以及*如*， *foreach*並*使用*陳述式，會建立區域變數宣告區域變數和區域常數的空間。</span><span class="sxs-lookup"><span data-stu-id="b58e2-170">Each *block* or *switch_block* , as well as a *for*, *foreach* and *using* statement, creates a local variable declaration space for local variables and local constants .</span></span> <span data-ttu-id="b58e2-171">名稱會導入此宣告空間*local_variable_declaration*s 並*local_constant_declaration*s。</span><span class="sxs-lookup"><span data-stu-id="b58e2-171">Names are introduced into this declaration space through *local_variable_declaration*s and *local_constant_declaration*s.</span></span> <span data-ttu-id="b58e2-172">請注意，這些函式宣告為參數的本機變數的宣告空間內巢狀或函式成員或匿名函式主體內，就會發生的區塊。</span><span class="sxs-lookup"><span data-stu-id="b58e2-172">Note that blocks that occur as or within the body of a function member or anonymous function are nested within the local variable declaration space declared by those functions for their parameters.</span></span> <span data-ttu-id="b58e2-173">因此，它是錯誤的本機變數與相同名稱的參數，方法。</span><span class="sxs-lookup"><span data-stu-id="b58e2-173">Thus it is an error to have e.g. a method with a local variable and a parameter of the same name.</span></span>
*  <span data-ttu-id="b58e2-174">每個*區塊*或是*switch_block*建立個別的宣告空間的標籤。</span><span class="sxs-lookup"><span data-stu-id="b58e2-174">Each *block* or *switch_block* creates a separate declaration space for labels.</span></span> <span data-ttu-id="b58e2-175">名稱會導入此宣告空間*labeled_statement*和名稱會透過參考*goto_statement*s。</span><span class="sxs-lookup"><span data-stu-id="b58e2-175">Names are introduced into this declaration space through *labeled_statement*s, and the names are referenced through *goto_statement*s.</span></span> <span data-ttu-id="b58e2-176">***標籤宣告空間***區塊包含任何巢狀的區塊。</span><span class="sxs-lookup"><span data-stu-id="b58e2-176">The ***label declaration space*** of a block includes any nested blocks.</span></span> <span data-ttu-id="b58e2-177">因此，巢狀區塊內不可以宣告具有相同名稱做為標籤，以在封閉區塊中的標籤。</span><span class="sxs-lookup"><span data-stu-id="b58e2-177">Thus, within a nested block it is not possible to declare a label with the same name as a label in an enclosing block.</span></span>

<span data-ttu-id="b58e2-178">名稱宣告的文字順序通常是沒有意義。</span><span class="sxs-lookup"><span data-stu-id="b58e2-178">The textual order in which names are declared is generally of no significance.</span></span> <span data-ttu-id="b58e2-179">特別是，文字順序並不重要的宣告和使用命名空間、 常數、 方法、 屬性、 事件、 索引子、 運算子、 執行個體建構函式、 解構函式、 靜態的建構函式和類型。</span><span class="sxs-lookup"><span data-stu-id="b58e2-179">In particular, textual order is not significant for the declaration and use of namespaces, constants, methods, properties, events, indexers, operators, instance constructors, destructors, static constructors, and types.</span></span> <span data-ttu-id="b58e2-180">依宣告順序是重要功能如下：</span><span class="sxs-lookup"><span data-stu-id="b58e2-180">Declaration order is significant in the following ways:</span></span>

*  <span data-ttu-id="b58e2-181">欄位宣告的區域變數宣告的宣告順序決定其初始設定式 （如果有的話） 執行的順序。</span><span class="sxs-lookup"><span data-stu-id="b58e2-181">Declaration order for field declarations and local variable declarations determines the order in which their initializers (if any) are executed.</span></span>
*  <span data-ttu-id="b58e2-182">必須定義區域變數，再使用它們 ([範圍](basic-concepts.md#scopes))。</span><span class="sxs-lookup"><span data-stu-id="b58e2-182">Local variables must be defined before they are used ([Scopes](basic-concepts.md#scopes)).</span></span>
*  <span data-ttu-id="b58e2-183">列舉的成員宣告的宣告順序 ([列舉成員](enums.md#enum-members)) 時，重要*constant_expression*會省略值。</span><span class="sxs-lookup"><span data-stu-id="b58e2-183">Declaration order for enum member declarations ([Enum members](enums.md#enum-members)) is significant when *constant_expression* values are omitted.</span></span>

<span data-ttu-id="b58e2-184">命名空間的宣告空間是 「 開啟已結束 」，而兩個命名空間宣告具有相同的完整名稱提供給相同的宣告空間。</span><span class="sxs-lookup"><span data-stu-id="b58e2-184">The declaration space of a namespace is "open ended", and two namespace declarations with the same fully qualified name contribute to the same declaration space.</span></span> <span data-ttu-id="b58e2-185">例如</span><span class="sxs-lookup"><span data-stu-id="b58e2-185">For example</span></span>
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

<span data-ttu-id="b58e2-186">上面的兩個命名空間宣告提供給相同的宣告空間，在此情況下宣告兩個類別的完整格式名稱具有`Megacorp.Data.Customer`和`Megacorp.Data.Order`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-186">The two namespace declarations above contribute to the same declaration space, in this case declaring two classes with the fully qualified names `Megacorp.Data.Customer` and `Megacorp.Data.Order`.</span></span> <span data-ttu-id="b58e2-187">這兩種宣告參與相同的宣告空間，因為它會造成編譯時期錯誤如果每一個包含具有相同名稱的類別的宣告。</span><span class="sxs-lookup"><span data-stu-id="b58e2-187">Because the two declarations contribute to the same declaration space, it would have caused a compile-time error if each contained a declaration of a class with the same name.</span></span>

<span data-ttu-id="b58e2-188">如上所述，在區塊的宣告空間會包含任何巢狀的區塊。</span><span class="sxs-lookup"><span data-stu-id="b58e2-188">As specified above, the declaration space of a block includes any nested blocks.</span></span> <span data-ttu-id="b58e2-189">因此，在下列範例中，`F`並`G`方法會產生編譯時期錯誤，因為名稱`i`外部區塊中宣告，因此不能重新宣告中的內部區塊。</span><span class="sxs-lookup"><span data-stu-id="b58e2-189">Thus, in the following example, the `F` and `G` methods result in a compile-time error because the name `i` is declared in the outer block and cannot be redeclared in the inner block.</span></span> <span data-ttu-id="b58e2-190">不過，`H`並`I`方法都有效，因為這兩個`i`的另一個非巢狀區塊中宣告。</span><span class="sxs-lookup"><span data-stu-id="b58e2-190">However, the `H` and `I` methods are valid since the two `i`'s are declared in separate non-nested blocks.</span></span>

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

## <a name="members"></a><span data-ttu-id="b58e2-191">成員</span><span class="sxs-lookup"><span data-stu-id="b58e2-191">Members</span></span>

<span data-ttu-id="b58e2-192">命名空間和型別具有***成員***。</span><span class="sxs-lookup"><span data-stu-id="b58e2-192">Namespaces and types have ***members***.</span></span> <span data-ttu-id="b58e2-193">實體的成員是透過限定名稱，後面接著參考的實體，開始使用正式推出 」`.`"語彙基元，後面接著的成員名稱。</span><span class="sxs-lookup"><span data-stu-id="b58e2-193">The members of an entity are generally available through the use of a qualified name that starts with a reference to the entity, followed by a "`.`" token, followed by the name of the member.</span></span>

<span data-ttu-id="b58e2-194">類型宣告中是宣告類型的成員或***繼承***型別的基底類別。</span><span class="sxs-lookup"><span data-stu-id="b58e2-194">Members of a type are either declared in the type declaration or ***inherited*** from the base class of the type.</span></span> <span data-ttu-id="b58e2-195">當型別繼承自基底類別的基底類別的執行個體建構函式、 解構函式和靜態建構函式除外的所有成員會變成衍生型別的成員。</span><span class="sxs-lookup"><span data-stu-id="b58e2-195">When a type inherits from a base class, all members of the base class, except instance constructors, destructors and static constructors, become members of the derived type.</span></span> <span data-ttu-id="b58e2-196">基底類別成員的宣告存取範圍不會控制是否要繼承的成員 — 繼承會延伸到任何不是執行個體建構函式、 靜態的建構函式或解構函式的成員。</span><span class="sxs-lookup"><span data-stu-id="b58e2-196">The declared accessibility of a base class member does not control whether the member is inherited—inheritance extends to any member that isn't an instance constructor, static constructor, or destructor.</span></span> <span data-ttu-id="b58e2-197">不過，繼承的成員可能無法存取在衍生的型別，因為其已宣告存取範圍 ([宣告存取範圍](basic-concepts.md#declared-accessibility)) 或因為它隱藏的宣告型別本身 ([透過隱藏繼承](basic-concepts.md#hiding-through-inheritance))。</span><span class="sxs-lookup"><span data-stu-id="b58e2-197">However, an inherited member may not be accessible in a derived type, either because of its declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)) or because it is hidden by a declaration in the type itself ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)).</span></span>

### <a name="namespace-members"></a><span data-ttu-id="b58e2-198">命名空間成員</span><span class="sxs-lookup"><span data-stu-id="b58e2-198">Namespace members</span></span>

<span data-ttu-id="b58e2-199">命名空間和型別有沒有封入命名空間都隸屬***全域命名空間***。</span><span class="sxs-lookup"><span data-stu-id="b58e2-199">Namespaces and types that have no enclosing namespace are members of the ***global namespace***.</span></span> <span data-ttu-id="b58e2-200">這直接對應到全域宣告空間中宣告的名稱。</span><span class="sxs-lookup"><span data-stu-id="b58e2-200">This corresponds directly to the names declared in the global declaration space.</span></span>

<span data-ttu-id="b58e2-201">命名空間和命名空間內宣告的型別是該命名空間的成員。</span><span class="sxs-lookup"><span data-stu-id="b58e2-201">Namespaces and types declared within a namespace are members of that namespace.</span></span> <span data-ttu-id="b58e2-202">這直接對應至命名空間的宣告空間中宣告的名稱。</span><span class="sxs-lookup"><span data-stu-id="b58e2-202">This corresponds directly to the names declared in the declaration space of the namespace.</span></span>

<span data-ttu-id="b58e2-203">命名空間沒有存取限制。</span><span class="sxs-lookup"><span data-stu-id="b58e2-203">Namespaces have no access restrictions.</span></span> <span data-ttu-id="b58e2-204">它不可以宣告 private、 protected 或 internal 的命名空間，而且命名空間名稱永遠是可公開存取。</span><span class="sxs-lookup"><span data-stu-id="b58e2-204">It is not possible to declare private, protected, or internal namespaces, and namespace names are always publicly accessible.</span></span>

### <a name="struct-members"></a><span data-ttu-id="b58e2-205">結構成員</span><span class="sxs-lookup"><span data-stu-id="b58e2-205">Struct members</span></span>

<span data-ttu-id="b58e2-206">結構成員會在結構中宣告的成員和繼承自結構的直接基底類別成員`System.ValueType`間接基底類別和`object`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-206">The members of a struct are the members declared in the struct and the members inherited from the struct's direct base class `System.ValueType` and the indirect base class `object`.</span></span>

<span data-ttu-id="b58e2-207">簡單類型的成員直接對應到此結構的類型別名的簡單類型的成員：</span><span class="sxs-lookup"><span data-stu-id="b58e2-207">The members of a simple type correspond directly to the members of the struct type aliased by the simple type:</span></span>

*  <span data-ttu-id="b58e2-208">成員`sbyte`的成員包括`System.SByte`結構。</span><span class="sxs-lookup"><span data-stu-id="b58e2-208">The members of `sbyte` are the members of the `System.SByte` struct.</span></span>
*  <span data-ttu-id="b58e2-209">成員`byte`的成員包括`System.Byte`結構。</span><span class="sxs-lookup"><span data-stu-id="b58e2-209">The members of `byte` are the members of the `System.Byte` struct.</span></span>
*  <span data-ttu-id="b58e2-210">成員`short`的成員包括`System.Int16`結構。</span><span class="sxs-lookup"><span data-stu-id="b58e2-210">The members of `short` are the members of the `System.Int16` struct.</span></span>
*  <span data-ttu-id="b58e2-211">成員`ushort`的成員包括`System.UInt16`結構。</span><span class="sxs-lookup"><span data-stu-id="b58e2-211">The members of `ushort` are the members of the `System.UInt16` struct.</span></span>
*  <span data-ttu-id="b58e2-212">成員`int`的成員包括`System.Int32`結構。</span><span class="sxs-lookup"><span data-stu-id="b58e2-212">The members of `int` are the members of the `System.Int32` struct.</span></span>
*  <span data-ttu-id="b58e2-213">成員`uint`的成員包括`System.UInt32`結構。</span><span class="sxs-lookup"><span data-stu-id="b58e2-213">The members of `uint` are the members of the `System.UInt32` struct.</span></span>
*  <span data-ttu-id="b58e2-214">成員`long`的成員包括`System.Int64`結構。</span><span class="sxs-lookup"><span data-stu-id="b58e2-214">The members of `long` are the members of the `System.Int64` struct.</span></span>
*  <span data-ttu-id="b58e2-215">成員`ulong`的成員包括`System.UInt64`結構。</span><span class="sxs-lookup"><span data-stu-id="b58e2-215">The members of `ulong` are the members of the `System.UInt64` struct.</span></span>
*  <span data-ttu-id="b58e2-216">成員`char`的成員包括`System.Char`結構。</span><span class="sxs-lookup"><span data-stu-id="b58e2-216">The members of `char` are the members of the `System.Char` struct.</span></span>
*  <span data-ttu-id="b58e2-217">成員`float`的成員包括`System.Single`結構。</span><span class="sxs-lookup"><span data-stu-id="b58e2-217">The members of `float` are the members of the `System.Single` struct.</span></span>
*  <span data-ttu-id="b58e2-218">成員`double`的成員包括`System.Double`結構。</span><span class="sxs-lookup"><span data-stu-id="b58e2-218">The members of `double` are the members of the `System.Double` struct.</span></span>
*  <span data-ttu-id="b58e2-219">成員`decimal`的成員包括`System.Decimal`結構。</span><span class="sxs-lookup"><span data-stu-id="b58e2-219">The members of `decimal` are the members of the `System.Decimal` struct.</span></span>
*  <span data-ttu-id="b58e2-220">成員`bool`的成員包括`System.Boolean`結構。</span><span class="sxs-lookup"><span data-stu-id="b58e2-220">The members of `bool` are the members of the `System.Boolean` struct.</span></span>

### <a name="enumeration-members"></a><span data-ttu-id="b58e2-221">列舉型別成員</span><span class="sxs-lookup"><span data-stu-id="b58e2-221">Enumeration members</span></span>

<span data-ttu-id="b58e2-222">列舉的成員會宣告為列舉中的常數和列舉型別的直接基底類別繼承的成員`System.Enum`和間接基底類別`System.ValueType`和`object`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-222">The members of an enumeration are the constants declared in the enumeration and the members inherited from the enumeration's direct base class `System.Enum` and the indirect base classes `System.ValueType` and `object`.</span></span>

### <a name="class-members"></a><span data-ttu-id="b58e2-223">類別成員</span><span class="sxs-lookup"><span data-stu-id="b58e2-223">Class members</span></span>

<span data-ttu-id="b58e2-224">類別的成員會在類別中宣告的成員和繼承自基底類別成員 (除了類別`object`具有沒有基底類別)。</span><span class="sxs-lookup"><span data-stu-id="b58e2-224">The members of a class are the members declared in the class and the members inherited from the base class (except for class `object` which has no base class).</span></span> <span data-ttu-id="b58e2-225">繼承自基底類別的成員包括常數、 欄位、 方法、 屬性、 事件、 索引子、 運算子和類型的基底類別，但不是執行個體建構函式、 解構函式和基底類別的靜態建構函式。</span><span class="sxs-lookup"><span data-stu-id="b58e2-225">The members inherited from the base class include the constants, fields, methods, properties, events, indexers, operators, and types of the base class, but not the instance constructors, destructors and static constructors of the base class.</span></span> <span data-ttu-id="b58e2-226">基底類別成員會繼承其存取範圍無關。</span><span class="sxs-lookup"><span data-stu-id="b58e2-226">Base class members are inherited without regard to their accessibility.</span></span>

<span data-ttu-id="b58e2-227">在類別宣告可能包含常數、 欄位、 方法、 屬性、 事件、 索引子、 運算子、 執行個體建構函式、 解構函式、 靜態建構函式和類型的宣告。</span><span class="sxs-lookup"><span data-stu-id="b58e2-227">A class declaration may contain declarations of constants, fields, methods, properties, events, indexers, operators, instance constructors, destructors, static constructors and types.</span></span>

<span data-ttu-id="b58e2-228">成員`object`和`string`直接對應到類別類型的成員在別名：</span><span class="sxs-lookup"><span data-stu-id="b58e2-228">The members of `object` and `string` correspond directly to the members of the class types they alias:</span></span>

*  <span data-ttu-id="b58e2-229">成員`object`的成員包括`System.Object`類別。</span><span class="sxs-lookup"><span data-stu-id="b58e2-229">The members of `object` are the members of the `System.Object` class.</span></span>
*  <span data-ttu-id="b58e2-230">成員`string`的成員包括`System.String`類別。</span><span class="sxs-lookup"><span data-stu-id="b58e2-230">The members of `string` are the members of the `System.String` class.</span></span>

### <a name="interface-members"></a><span data-ttu-id="b58e2-231">介面成員</span><span class="sxs-lookup"><span data-stu-id="b58e2-231">Interface members</span></span>

<span data-ttu-id="b58e2-232">介面的成員是在介面和介面的所有基底介面中宣告的成員。</span><span class="sxs-lookup"><span data-stu-id="b58e2-232">The members of an interface are the members declared in the interface and in all base interfaces of the interface.</span></span> <span data-ttu-id="b58e2-233">在類別成員`object`則不會，嚴格地說，任何介面的成員 ([介面成員](interfaces.md#interface-members))。</span><span class="sxs-lookup"><span data-stu-id="b58e2-233">The members in class `object` are not, strictly speaking, members of any interface ([Interface members](interfaces.md#interface-members)).</span></span> <span data-ttu-id="b58e2-234">不過，在類別成員`object`可透過任何介面類型中進行成員查詢 ([成員查閱](expressions.md#member-lookup))。</span><span class="sxs-lookup"><span data-stu-id="b58e2-234">However, the members in class `object` are available via member lookup in any interface type ([Member lookup](expressions.md#member-lookup)).</span></span>

### <a name="array-members"></a><span data-ttu-id="b58e2-235">陣列成員</span><span class="sxs-lookup"><span data-stu-id="b58e2-235">Array members</span></span>

<span data-ttu-id="b58e2-236">陣列的成員會繼承自類別的成員`System.Array`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-236">The members of an array are the members inherited from class `System.Array`.</span></span>

### <a name="delegate-members"></a><span data-ttu-id="b58e2-237">委派成員</span><span class="sxs-lookup"><span data-stu-id="b58e2-237">Delegate members</span></span>

<span data-ttu-id="b58e2-238">委派的成員會繼承自類別的成員`System.Delegate`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-238">The members of a delegate are the members inherited from class `System.Delegate`.</span></span>

## <a name="member-access"></a><span data-ttu-id="b58e2-239">成員存取</span><span class="sxs-lookup"><span data-stu-id="b58e2-239">Member access</span></span>

<span data-ttu-id="b58e2-240">成員宣告允許成員存取控制。</span><span class="sxs-lookup"><span data-stu-id="b58e2-240">Declarations of members allow control over member access.</span></span> <span data-ttu-id="b58e2-241">成員存取範圍藉由宣告存取範圍 ([宣告存取範圍](basic-concepts.md#declared-accessibility)) 成員的結合可存取性，立即包含類型中，如果有的話。</span><span class="sxs-lookup"><span data-stu-id="b58e2-241">The accessibility of a member is established by the declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)) of the member combined with the accessibility of the immediately containing type, if any.</span></span>

<span data-ttu-id="b58e2-242">當允許存取特定的成員時，該成員即為***存取***。</span><span class="sxs-lookup"><span data-stu-id="b58e2-242">When access to a particular member is allowed, the member is said to be ***accessible***.</span></span> <span data-ttu-id="b58e2-243">相反地，當不被允許存取特定的成員，該成員要***無法存取***。</span><span class="sxs-lookup"><span data-stu-id="b58e2-243">Conversely, when access to a particular member is disallowed, the member is said to be ***inaccessible***.</span></span> <span data-ttu-id="b58e2-244">存取範圍定義域中包含文字的位置存取發生時，允許存取成員 ([存取範圍定義域](basic-concepts.md#accessibility-domains)) 的成員。</span><span class="sxs-lookup"><span data-stu-id="b58e2-244">Access to a member is permitted when the textual location in which the access takes place is included in the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the member.</span></span>

### <a name="declared-accessibility"></a><span data-ttu-id="b58e2-245">已宣告存取範圍</span><span class="sxs-lookup"><span data-stu-id="b58e2-245">Declared accessibility</span></span>

<span data-ttu-id="b58e2-246">***宣告存取範圍***的成員可以是下列其中之一：</span><span class="sxs-lookup"><span data-stu-id="b58e2-246">The ***declared accessibility*** of a member can be one of the following:</span></span>

*  <span data-ttu-id="b58e2-247">公用、 選取包含`public`成員宣告中的修飾詞。</span><span class="sxs-lookup"><span data-stu-id="b58e2-247">Public, which is selected by including a `public` modifier in the member declaration.</span></span> <span data-ttu-id="b58e2-248">直覺式的意義`public`是 「 不受限制的存取 」。</span><span class="sxs-lookup"><span data-stu-id="b58e2-248">The intuitive meaning of `public` is "access not limited".</span></span>
*  <span data-ttu-id="b58e2-249">受保護，會選取包括`protected`成員宣告中的修飾詞。</span><span class="sxs-lookup"><span data-stu-id="b58e2-249">Protected, which is selected by including a `protected` modifier in the member declaration.</span></span> <span data-ttu-id="b58e2-250">直覺式的意義`protected`」 存取僅限於包含類別或類型衍生自包含類別 」。</span><span class="sxs-lookup"><span data-stu-id="b58e2-250">The intuitive meaning of `protected` is "access limited to the containing class or types derived from the containing class".</span></span>
*  <span data-ttu-id="b58e2-251">內部，會選取 包括`internal`成員宣告中的修飾詞。</span><span class="sxs-lookup"><span data-stu-id="b58e2-251">Internal, which is selected by including an `internal` modifier in the member declaration.</span></span> <span data-ttu-id="b58e2-252">直覺式的意義`internal`是 「 限於此程式的存取 」。</span><span class="sxs-lookup"><span data-stu-id="b58e2-252">The intuitive meaning of `internal` is "access limited to this program".</span></span>
*  <span data-ttu-id="b58e2-253">受保護的內部 （亦即 protected 或 internal），會選取包括`protected`和`internal`成員宣告中的修飾詞。</span><span class="sxs-lookup"><span data-stu-id="b58e2-253">Protected internal (meaning protected or internal), which is selected by including both a `protected` and an `internal` modifier in the member declaration.</span></span> <span data-ttu-id="b58e2-254">直覺式的意義`protected internal`是 「 存取僅限於此程式或衍生自包含類別的型別 」。</span><span class="sxs-lookup"><span data-stu-id="b58e2-254">The intuitive meaning of `protected internal` is "access limited to this program or types derived from the containing class".</span></span>
*  <span data-ttu-id="b58e2-255">選取包含私用`private`成員宣告中的修飾詞。</span><span class="sxs-lookup"><span data-stu-id="b58e2-255">Private, which is selected by including a `private` modifier in the member declaration.</span></span> <span data-ttu-id="b58e2-256">直覺式的意義`private`是 「 存取限於包含型別 」。</span><span class="sxs-lookup"><span data-stu-id="b58e2-256">The intuitive meaning of `private` is "access limited to the containing type".</span></span>

<span data-ttu-id="b58e2-257">根據宣告的成員所發生的內容，允許特定類型的宣告存取範圍。</span><span class="sxs-lookup"><span data-stu-id="b58e2-257">Depending on the context in which a member declaration takes place, only certain types of declared accessibility are permitted.</span></span> <span data-ttu-id="b58e2-258">此外，當成員宣告不包含任何存取修飾詞，宣告進行的內容會決定預設的宣告存取範圍。</span><span class="sxs-lookup"><span data-stu-id="b58e2-258">Furthermore, when a member declaration does not include any access modifiers, the context in which the declaration takes place determines the default declared accessibility.</span></span>

*  <span data-ttu-id="b58e2-259">命名空間以隱含方式具有`public`宣告存取範圍。</span><span class="sxs-lookup"><span data-stu-id="b58e2-259">Namespaces implicitly have `public` declared accessibility.</span></span> <span data-ttu-id="b58e2-260">命名空間宣告中不能使用任何存取修飾詞。</span><span class="sxs-lookup"><span data-stu-id="b58e2-260">No access modifiers are allowed on namespace declarations.</span></span>
*  <span data-ttu-id="b58e2-261">在編譯單位或命名空間中宣告的型別可以有`public`或是`internal`宣告存取範圍和預設值是`internal`宣告存取範圍。</span><span class="sxs-lookup"><span data-stu-id="b58e2-261">Types declared in compilation units or namespaces can have `public` or `internal` declared accessibility and default to `internal` declared accessibility.</span></span>
*  <span data-ttu-id="b58e2-262">類別成員可以有五種類型的宣告存取範圍，預設為`private`宣告存取範圍。</span><span class="sxs-lookup"><span data-stu-id="b58e2-262">Class members can have any of the five kinds of declared accessibility and default to `private` declared accessibility.</span></span> <span data-ttu-id="b58e2-263">(請注意，型別宣告為類別的成員可以具有任何五種類型的宣告存取範圍，而型別宣告為命名空間的成員可以只有`public`或`internal`宣告存取範圍。)</span><span class="sxs-lookup"><span data-stu-id="b58e2-263">(Note that a type declared as a member of a class can have any of the five kinds of declared accessibility, whereas a type declared as a member of a namespace can have only `public` or `internal` declared accessibility.)</span></span>
*  <span data-ttu-id="b58e2-264">結構的成員可以有`public`， `internal`，或`private`宣告存取範圍和預設值是`private`宣告存取範圍，因為會隱含地密封結構。</span><span class="sxs-lookup"><span data-stu-id="b58e2-264">Struct members can have `public`, `internal`, or `private` declared accessibility and default to `private` declared accessibility because structs are implicitly sealed.</span></span> <span data-ttu-id="b58e2-265">結構 （也就不會繼承該結構） 中導入的結構成員不能有`protected`或`protected internal`宣告存取範圍。</span><span class="sxs-lookup"><span data-stu-id="b58e2-265">Struct members introduced in a struct (that is, not inherited by that struct) cannot have `protected` or `protected internal` declared accessibility.</span></span> <span data-ttu-id="b58e2-266">(請注意，型別宣告為結構的成員可以有`public`， `internal`，或`private`宣告存取範圍，而型別宣告為命名空間的成員可以只有`public`或`internal`宣告存取範圍。)</span><span class="sxs-lookup"><span data-stu-id="b58e2-266">(Note that a type declared as a member of a struct can have `public`, `internal`, or `private` declared accessibility, whereas a type declared as a member of a namespace can have only `public` or `internal` declared accessibility.)</span></span>
*  <span data-ttu-id="b58e2-267">介面成員以隱含方式具有`public`宣告存取範圍。</span><span class="sxs-lookup"><span data-stu-id="b58e2-267">Interface members implicitly have `public` declared accessibility.</span></span> <span data-ttu-id="b58e2-268">介面成員宣告中不能使用任何存取修飾詞。</span><span class="sxs-lookup"><span data-stu-id="b58e2-268">No access modifiers are allowed on interface member declarations.</span></span>
*  <span data-ttu-id="b58e2-269">列舉型別成員以隱含方式具有`public`宣告存取範圍。</span><span class="sxs-lookup"><span data-stu-id="b58e2-269">Enumeration members implicitly have `public` declared accessibility.</span></span> <span data-ttu-id="b58e2-270">列舉型別成員宣告中不能使用任何存取修飾詞。</span><span class="sxs-lookup"><span data-stu-id="b58e2-270">No access modifiers are allowed on enumeration member declarations.</span></span>

### <a name="accessibility-domains"></a><span data-ttu-id="b58e2-271">存取範圍定義域</span><span class="sxs-lookup"><span data-stu-id="b58e2-271">Accessibility domains</span></span>

<span data-ttu-id="b58e2-272">***存取範圍定義域***的成員包含的程式文字中允許的成員存取 （可能是脫離） 的各節。</span><span class="sxs-lookup"><span data-stu-id="b58e2-272">The ***accessibility domain*** of a member consists of the (possibly disjoint) sections of program text in which access to the member is permitted.</span></span> <span data-ttu-id="b58e2-273">為了定義成員的存取範圍定義域，成員要***最上層***如果它未宣告的型別中，而且成員要***巢狀***如果另一個類型內宣告。</span><span class="sxs-lookup"><span data-stu-id="b58e2-273">For purposes of defining the accessibility domain of a member, a member is said to be ***top-level*** if it is not declared within a type, and a member is said to be ***nested*** if it is declared within another type.</span></span> <span data-ttu-id="b58e2-274">此外，***程式文字***程式的定義為所有程式的程式，所有的原始程式檔中所包含的文字，並且程式文字類型的定義為所有的程式中包含文字*type_declaration*s 該型別 （包括，可能巢狀類型的類型）。</span><span class="sxs-lookup"><span data-stu-id="b58e2-274">Furthermore, the ***program text*** of a program is defined as all program text contained in all source files of the program, and the program text of a type is defined as all program text contained in the *type_declaration*s of that type (including, possibly, types that are nested within the type).</span></span>

<span data-ttu-id="b58e2-275">預先定義的類型的存取範圍定義域 (例如`object`， `int`，或`double`) 不受限制。</span><span class="sxs-lookup"><span data-stu-id="b58e2-275">The accessibility domain of a predefined type (such as `object`, `int`, or `double`) is unlimited.</span></span>

<span data-ttu-id="b58e2-276">最上層的存取範圍定義域未繫結的型別`T`([繫結和解除繫結型別](types.md#bound-and-unbound-types)) 在程式中宣告`P`定義如下：</span><span class="sxs-lookup"><span data-stu-id="b58e2-276">The accessibility domain of a top-level unbound type `T` ([Bound and unbound types](types.md#bound-and-unbound-types)) that is declared in a program `P` is defined as follows:</span></span>

*  <span data-ttu-id="b58e2-277">如果宣告存取範圍`T`是`public`的存取範圍定義域`T`是程式文字`P`和 參考的任何程式`P`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-277">If the declared accessibility of `T` is `public`, the accessibility domain of `T` is the program text of `P` and any program that references `P`.</span></span>
*  <span data-ttu-id="b58e2-278">如果 `T` 的宣告存取範圍是 `internal`，`T` 的存取範圍領域就是 `P` 的程式文字。</span><span class="sxs-lookup"><span data-stu-id="b58e2-278">If the declared accessibility of `T` is `internal`, the accessibility domain of `T` is the program text of `P`.</span></span>

<span data-ttu-id="b58e2-279">這些定義它如下所示，最上層的未繫結類型的存取範圍定義域一律至少是在宣告中，輸入的的程式的程式文字。</span><span class="sxs-lookup"><span data-stu-id="b58e2-279">From these definitions it follows that the accessibility domain of a top-level unbound type is always at least the program text of the program in which that type is declared.</span></span>

<span data-ttu-id="b58e2-280">建構類型的存取範圍定義域`T<A1, ..., An>`交集的未繫結的泛型類型的存取範圍定義域`T`和類型引數的存取範圍定義域`A1, ..., An`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-280">The accessibility domain for a constructed type `T<A1, ..., An>` is the intersection of the accessibility domain of the unbound generic type `T` and the accessibility domains of the type arguments `A1, ..., An`.</span></span>

<span data-ttu-id="b58e2-281">巢狀成員的存取範圍定義域`M`型別宣告`T`程式內`P`定義，如下所示 (注意的是，`M`本身可能就是類型):</span><span class="sxs-lookup"><span data-stu-id="b58e2-281">The accessibility domain of a nested member `M` declared in a type `T` within a program `P` is defined as follows (noting that `M` itself may possibly be a type):</span></span>

*  <span data-ttu-id="b58e2-282">如果 `M` 的宣告存取範圍是 `public`，`M` 的存取範圍領域就是 `T` 的存取範圍領域。</span><span class="sxs-lookup"><span data-stu-id="b58e2-282">If the declared accessibility of `M` is `public`, the accessibility domain of `M` is the accessibility domain of `T`.</span></span>
*  <span data-ttu-id="b58e2-283">如果宣告存取範圍`M`是`protected internal`讓`D`是等位的程式文字`P`以及任何類型的程式文字衍生自`T`，外部宣告的所在`P`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-283">If the declared accessibility of `M` is `protected internal`, let `D` be the union of the program text of `P` and the program text of any type derived from `T`, which is declared outside `P`.</span></span> <span data-ttu-id="b58e2-284">存取範圍定義域`M`交集的存取範圍領域`T`使用`D`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-284">The accessibility domain of `M` is the intersection of the accessibility domain of `T` with `D`.</span></span>
*  <span data-ttu-id="b58e2-285">如果宣告存取範圍`M`是`protected`讓`D`是等位的程式文字`T`以及任何類型的程式文字衍生自`T`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-285">If the declared accessibility of `M` is `protected`, let `D` be the union of the program text of `T` and the program text of any type derived from `T`.</span></span> <span data-ttu-id="b58e2-286">存取範圍定義域`M`交集的存取範圍領域`T`使用`D`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-286">The accessibility domain of `M` is the intersection of the accessibility domain of `T` with `D`.</span></span>
*  <span data-ttu-id="b58e2-287">如果 `M` 的宣告存取範圍是 `internal`，`M` 的存取範圍領域是 `T` 的存取範圍領域與 `P` 的程式文字的交集。</span><span class="sxs-lookup"><span data-stu-id="b58e2-287">If the declared accessibility of `M` is `internal`, the accessibility domain of `M` is the intersection of the accessibility domain of `T` with the program text of `P`.</span></span>
*  <span data-ttu-id="b58e2-288">如果 `M` 的宣告存取範圍是 `private`，`M` 的存取範圍領域就是 `T` 的程式文字。</span><span class="sxs-lookup"><span data-stu-id="b58e2-288">If the declared accessibility of `M` is `private`, the accessibility domain of `M` is the program text of `T`.</span></span>

<span data-ttu-id="b58e2-289">這些定義它如下所示的巢狀成員的存取範圍領域是一律至少在其中宣告成員型別的程式文字。</span><span class="sxs-lookup"><span data-stu-id="b58e2-289">From these definitions it follows that the accessibility domain of a nested member is always at least the program text of the type in which the member is declared.</span></span> <span data-ttu-id="b58e2-290">此外，它會遵循成員存取範圍領域是永遠不會宣告成員的型別存取範圍領域與更廣。</span><span class="sxs-lookup"><span data-stu-id="b58e2-290">Furthermore, it follows that the accessibility domain of a member is never more inclusive than the accessibility domain of the type in which the member is declared.</span></span>

<span data-ttu-id="b58e2-291">簡單來說，類型或成員時`M`是存取，會評估下列步驟以確保允許的存取：</span><span class="sxs-lookup"><span data-stu-id="b58e2-291">In intuitive terms, when a type or member `M` is accessed, the following steps are evaluated to ensure that the access is permitted:</span></span>

*  <span data-ttu-id="b58e2-292">首先，如果`M`在宣告的類型 （但不要編譯單位或命名空間），如果該類型不能存取，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="b58e2-292">First, if `M` is declared within a type (as opposed to a compilation unit or a namespace), a compile-time error occurs if that type is not accessible.</span></span>
*  <span data-ttu-id="b58e2-293">然後，如果`M`是`public`，允許存取。</span><span class="sxs-lookup"><span data-stu-id="b58e2-293">Then, if `M` is `public`, the access is permitted.</span></span>
*  <span data-ttu-id="b58e2-294">否則，如果`M`是`protected internal`，允許的存取，以在程式內發生`M`宣告，如果它就會發生在類別或衍生自類別所在`M`宣告並透過衍生類別型別 ([受保護執行個體存取成員](basic-concepts.md#protected-access-for-instance-members))。</span><span class="sxs-lookup"><span data-stu-id="b58e2-294">Otherwise, if `M` is `protected internal`, the access is permitted if it occurs within the program in which `M` is declared, or if it occurs within a class derived from the class in which `M` is declared and takes place through the derived class type ([Protected access for instance members](basic-concepts.md#protected-access-for-instance-members)).</span></span>
*  <span data-ttu-id="b58e2-295">否則，如果`M`是`protected`，所在類別內發生，允許的存取`M`宣告，如果它就會發生在類別或衍生自類別所在`M`宣告並透過衍生類別型別 ([受保護執行個體存取成員](basic-concepts.md#protected-access-for-instance-members))。</span><span class="sxs-lookup"><span data-stu-id="b58e2-295">Otherwise, if `M` is `protected`, the access is permitted if it occurs within the class in which `M` is declared, or if it occurs within a class derived from the class in which `M` is declared and takes place through the derived class type ([Protected access for instance members](basic-concepts.md#protected-access-for-instance-members)).</span></span>
*  <span data-ttu-id="b58e2-296">否則，如果`M`已`internal`，以在程式內發生，允許的存取`M`宣告。</span><span class="sxs-lookup"><span data-stu-id="b58e2-296">Otherwise, if `M` is `internal`, the access is permitted if it occurs within the program in which `M` is declared.</span></span>
*  <span data-ttu-id="b58e2-297">否則，如果`M`已`private`，在其中的型別內發生，允許的存取`M`宣告。</span><span class="sxs-lookup"><span data-stu-id="b58e2-297">Otherwise, if `M` is `private`, the access is permitted if it occurs within the type in which `M` is declared.</span></span>
*  <span data-ttu-id="b58e2-298">否則，型別或成員無法存取，而且就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="b58e2-298">Otherwise, the type or member is inaccessible, and a compile-time error occurs.</span></span>

<span data-ttu-id="b58e2-299">在範例</span><span class="sxs-lookup"><span data-stu-id="b58e2-299">In the example</span></span>
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
<span data-ttu-id="b58e2-300">類別和成員具有下列的存取範圍定義域：</span><span class="sxs-lookup"><span data-stu-id="b58e2-300">the classes and members have the following accessibility domains:</span></span>

*  <span data-ttu-id="b58e2-301">存取範圍定義域`A`和`A.X`不受限制。</span><span class="sxs-lookup"><span data-stu-id="b58e2-301">The accessibility domain of `A` and `A.X` is unlimited.</span></span>
*  <span data-ttu-id="b58e2-302">存取範圍定義域`A.Y`， `B`， `B.X`， `B.Y`， `B.C`， `B.C.X`，和`B.C.Y`是包含程式的程式文字。</span><span class="sxs-lookup"><span data-stu-id="b58e2-302">The accessibility domain of `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X`, and `B.C.Y` is the program text of the containing program.</span></span>
*  <span data-ttu-id="b58e2-303">存取範圍定義域`A.Z`是程式文字`A`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-303">The accessibility domain of `A.Z` is the program text of `A`.</span></span>
*  <span data-ttu-id="b58e2-304">存取範圍定義域`B.Z`並`B.D`是程式文字`B`，包括的程式文字`B.C`和`B.D`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-304">The accessibility domain of `B.Z` and `B.D` is the program text of `B`, including the program text of `B.C` and `B.D`.</span></span>
*  <span data-ttu-id="b58e2-305">存取範圍定義域`B.C.Z`是程式文字`B.C`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-305">The accessibility domain of `B.C.Z` is the program text of `B.C`.</span></span>
*  <span data-ttu-id="b58e2-306">存取範圍定義域`B.D.X`並`B.D.Y`是程式文字`B`，包括的程式文字`B.C`和`B.D`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-306">The accessibility domain of `B.D.X` and `B.D.Y` is the program text of `B`, including the program text of `B.C` and `B.D`.</span></span>
*  <span data-ttu-id="b58e2-307">存取範圍定義域`B.D.Z`是程式文字`B.D`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-307">The accessibility domain of `B.D.Z` is the program text of `B.D`.</span></span>

<span data-ttu-id="b58e2-308">如範例所示，成員存取範圍領域是類型的永遠不會超過包含。</span><span class="sxs-lookup"><span data-stu-id="b58e2-308">As the example illustrates, the accessibility domain of a member is never larger than that of a containing type.</span></span> <span data-ttu-id="b58e2-309">比方說，即使所有`X`成員具有公用的宣告存取範圍以外的所有`A.X`有會受限於包含類型的存取範圍定義域。</span><span class="sxs-lookup"><span data-stu-id="b58e2-309">For example, even though all `X` members have public declared accessibility, all but `A.X` have accessibility domains that are constrained by a containing type.</span></span>

<span data-ttu-id="b58e2-310">中所述[成員](basic-concepts.md#members)，衍生型別會繼承基底類別，執行個體建構函式、 解構函式和靜態建構函式除外的所有成員。</span><span class="sxs-lookup"><span data-stu-id="b58e2-310">As described in [Members](basic-concepts.md#members), all members of a base class, except for instance constructors, destructors and static constructors, are inherited by derived types.</span></span> <span data-ttu-id="b58e2-311">這包括基底類別的平均私用成員。</span><span class="sxs-lookup"><span data-stu-id="b58e2-311">This includes even private members of a base class.</span></span> <span data-ttu-id="b58e2-312">不過，在私用成員的存取範圍定義域包含只能宣告成員類型的程式文字。</span><span class="sxs-lookup"><span data-stu-id="b58e2-312">However, the accessibility domain of a private member includes only the program text of the type in which the member is declared.</span></span> <span data-ttu-id="b58e2-313">在範例</span><span class="sxs-lookup"><span data-stu-id="b58e2-313">In the example</span></span>
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
<span data-ttu-id="b58e2-314">`B`類別會繼承私用成員`x`從`A`類別。</span><span class="sxs-lookup"><span data-stu-id="b58e2-314">the `B` class inherits the private member `x` from the `A` class.</span></span> <span data-ttu-id="b58e2-315">因為是私用成員，才可存取*class_body*的`A`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-315">Because the member is private, it is only accessible within the *class_body* of `A`.</span></span> <span data-ttu-id="b58e2-316">因此，若要存取`b.x`成功地`A.F`方法，但在失敗`B.F`方法。</span><span class="sxs-lookup"><span data-stu-id="b58e2-316">Thus, the access to `b.x` succeeds in the `A.F` method, but fails in the `B.F` method.</span></span>

### <a name="protected-access-for-instance-members"></a><span data-ttu-id="b58e2-317">受保護執行個體成員的存取</span><span class="sxs-lookup"><span data-stu-id="b58e2-317">Protected access for instance members</span></span>

<span data-ttu-id="b58e2-318">當`protected`執行個體成員存取以外的程式文字的類別中宣告，及何時`protected internal`執行個體成員存取外部程式宣告它的程式文字、 存取權必須在進行中衍生自其宣告所在類別的類別宣告。</span><span class="sxs-lookup"><span data-stu-id="b58e2-318">When a `protected` instance member is accessed outside the program text of the class in which it is declared, and when a `protected internal` instance member is accessed outside the program text of the program in which it is declared, the access must take place within a class declaration that derives from the class in which it is declared.</span></span> <span data-ttu-id="b58e2-319">此外，所以需要透過該衍生的類別類型或從中建構類別類型的執行個體進行存取。</span><span class="sxs-lookup"><span data-stu-id="b58e2-319">Furthermore, the access is required to take place through an instance of that derived class type or a class type constructed from it.</span></span> <span data-ttu-id="b58e2-320">這項限制會防止存取的其他衍生的類別，受保護的成員，即使成員繼承自相同基底類別所衍生的類別。</span><span class="sxs-lookup"><span data-stu-id="b58e2-320">This restriction prevents one derived class from accessing protected members of other derived classes, even when the members are inherited from the same base class.</span></span>

<span data-ttu-id="b58e2-321">可讓`B`是宣告受保護執行個體成員的基底類別`M`，並讓`D`是衍生自類別`B`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-321">Let `B` be a base class that declares a protected instance member `M`, and let `D` be a class that derives from `B`.</span></span> <span data-ttu-id="b58e2-322">內*class_body*的`D`，存取`M`可以採用下列格式之一：</span><span class="sxs-lookup"><span data-stu-id="b58e2-322">Within the *class_body* of `D`, access to `M` can take one of the following forms:</span></span>

*  <span data-ttu-id="b58e2-323">不合格*type_name*或是*primary_expression*表單的`M`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-323">An unqualified *type_name* or *primary_expression* of the form `M`.</span></span>
*  <span data-ttu-id="b58e2-324">A *primary_expression*的表單`E.M`，提供的型別`E`是`T`或衍生自類別`T`，其中`T`是類別類型`D`，或類別類型從建構 `D`</span><span class="sxs-lookup"><span data-stu-id="b58e2-324">A *primary_expression* of the form `E.M`, provided the type of `E` is `T` or a class derived from `T`, where `T` is the class type `D`, or a class type constructed from `D`</span></span>
*  <span data-ttu-id="b58e2-325">A *primary_expression*表單的`base.M`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-325">A *primary_expression* of the form `base.M`.</span></span>

<span data-ttu-id="b58e2-326">除了這些形式的存取權限，在衍生的類別可以存取受保護執行個體建構函式中的基底類別*constructor_initializer* ([建構函式初始設定式](classes.md#constructor-initializers))。</span><span class="sxs-lookup"><span data-stu-id="b58e2-326">In addition to these forms of access, a derived class can access a protected instance constructor of a base class in a *constructor_initializer* ([Constructor initializers](classes.md#constructor-initializers)).</span></span>

<span data-ttu-id="b58e2-327">在範例</span><span class="sxs-lookup"><span data-stu-id="b58e2-327">In the example</span></span>
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
<span data-ttu-id="b58e2-328">內`A`，就能夠存取`x`透過兩個執行個體`A`並`B`，因為在任一情況下存取是透過執行個體進行`A`或衍生自類別`A`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-328">within `A`, it is possible to access `x` through instances of both `A` and `B`, since in either case the access takes place through an instance of `A` or a class derived from `A`.</span></span> <span data-ttu-id="b58e2-329">不過，在`B`，就不能夠存取`x`的執行個體透過`A`，因為`A`不是衍生自`B`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-329">However, within `B`, it is not possible to access `x` through an instance of `A`, since `A` does not derive from `B`.</span></span>

<span data-ttu-id="b58e2-330">在範例</span><span class="sxs-lookup"><span data-stu-id="b58e2-330">In the example</span></span>
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
<span data-ttu-id="b58e2-331">三個指派`x`已允許，因為它們全都需要透過建構自泛型類型的類別類型的執行個體。</span><span class="sxs-lookup"><span data-stu-id="b58e2-331">the three assignments to `x` are permitted because they all take place through instances of class types constructed from the generic type.</span></span>

### <a name="accessibility-constraints"></a><span data-ttu-id="b58e2-332">存取範圍條件約束</span><span class="sxs-lookup"><span data-stu-id="b58e2-332">Accessibility constraints</span></span>

<span data-ttu-id="b58e2-333">在 C# 語言中的數個建構需要為類型***至少像那樣***成員或另一個型別。</span><span class="sxs-lookup"><span data-stu-id="b58e2-333">Several constructs in the C# language require a type to be ***at least as accessible as*** a member or another type.</span></span> <span data-ttu-id="b58e2-334">型別`T`即為至少一個成員或類型像那樣`M`如果的存取範圍定義域`T`的存取範圍定義域的超集`M`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-334">A type `T` is said to be at least as accessible as a member or type `M` if the accessibility domain of `T` is a superset of the accessibility domain of `M`.</span></span> <span data-ttu-id="b58e2-335">亦即`T`是至少像那樣`M`如果`T`所在的所有內容中存取`M`存取。</span><span class="sxs-lookup"><span data-stu-id="b58e2-335">In other words, `T` is at least as accessible as `M` if `T` is accessible in all contexts in which `M` is accessible.</span></span>

<span data-ttu-id="b58e2-336">協助工具條件約束如下：</span><span class="sxs-lookup"><span data-stu-id="b58e2-336">The following accessibility constraints exist:</span></span>

*  <span data-ttu-id="b58e2-337">類別類型的直接基底類別至少必須可以像類別類型本身一樣地存取。</span><span class="sxs-lookup"><span data-stu-id="b58e2-337">The direct base class of a class type must be at least as accessible as the class type itself.</span></span>
*  <span data-ttu-id="b58e2-338">介面類型的明確基底介面至少必須可以像介面類型本身一樣地存取。</span><span class="sxs-lookup"><span data-stu-id="b58e2-338">The explicit base interfaces of an interface type must be at least as accessible as the interface type itself.</span></span>
*  <span data-ttu-id="b58e2-339">委派類型的傳回類型和參數類型至少必須可以像委派類型本身一樣地存取。</span><span class="sxs-lookup"><span data-stu-id="b58e2-339">The return type and parameter types of a delegate type must be at least as accessible as the delegate type itself.</span></span>
*  <span data-ttu-id="b58e2-340">常數的類型至少必須可以像常數本身一樣地存取。</span><span class="sxs-lookup"><span data-stu-id="b58e2-340">The type of a constant must be at least as accessible as the constant itself.</span></span>
*  <span data-ttu-id="b58e2-341">欄位的類型至少必須可以像欄位本身一樣地存取。</span><span class="sxs-lookup"><span data-stu-id="b58e2-341">The type of a field must be at least as accessible as the field itself.</span></span>
*  <span data-ttu-id="b58e2-342">方法的傳回類型和參數類型至少必須可以像方法本身一樣地存取。</span><span class="sxs-lookup"><span data-stu-id="b58e2-342">The return type and parameter types of a method must be at least as accessible as the method itself.</span></span>
*  <span data-ttu-id="b58e2-343">屬性的類型至少必須可以像屬性本身一樣地存取。</span><span class="sxs-lookup"><span data-stu-id="b58e2-343">The type of a property must be at least as accessible as the property itself.</span></span>
*  <span data-ttu-id="b58e2-344">事件的類型至少必須可以像事件本身一樣地存取。</span><span class="sxs-lookup"><span data-stu-id="b58e2-344">The type of an event must be at least as accessible as the event itself.</span></span>
*  <span data-ttu-id="b58e2-345">索引子的類型和參數類型至少必須可以像索引子本身一樣地存取。</span><span class="sxs-lookup"><span data-stu-id="b58e2-345">The type and parameter types of an indexer must be at least as accessible as the indexer itself.</span></span>
*  <span data-ttu-id="b58e2-346">運算子的傳回類型和參數類型至少必須可以像運算子本身一樣地存取。</span><span class="sxs-lookup"><span data-stu-id="b58e2-346">The return type and parameter types of an operator must be at least as accessible as the operator itself.</span></span>
*  <span data-ttu-id="b58e2-347">執行個體建構函式的參數類型必須是至少一樣地存取本身的執行個體建構函式。</span><span class="sxs-lookup"><span data-stu-id="b58e2-347">The parameter types of an instance constructor must be at least as accessible as the instance constructor itself.</span></span>

<span data-ttu-id="b58e2-348">在範例</span><span class="sxs-lookup"><span data-stu-id="b58e2-348">In the example</span></span>
```csharp
class A {...}

public class B: A {...}
```
<span data-ttu-id="b58e2-349">`B`類別會產生編譯時期錯誤，因為`A`不是至少像那樣`B`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-349">the `B` class results in a compile-time error because `A` is not at least as accessible as `B`.</span></span>

<span data-ttu-id="b58e2-350">同樣地，在範例</span><span class="sxs-lookup"><span data-stu-id="b58e2-350">Likewise, in the example</span></span>
```csharp
class A {...}

public class B
{
    A F() {...}

    internal A G() {...}

    public A H() {...}
}
```
<span data-ttu-id="b58e2-351">`H`方法中的`B`導致編譯時期錯誤，因為傳回型別`A`不至少像方法那樣。</span><span class="sxs-lookup"><span data-stu-id="b58e2-351">the `H` method in `B` results in a compile-time error because the return type `A` is not at least as accessible as the method.</span></span>

## <a name="signatures-and-overloading"></a><span data-ttu-id="b58e2-352">簽章和多載</span><span class="sxs-lookup"><span data-stu-id="b58e2-352">Signatures and overloading</span></span>

<span data-ttu-id="b58e2-353">方法、 執行個體建構函式、 索引子和運算子的特性在於他們***簽章***:</span><span class="sxs-lookup"><span data-stu-id="b58e2-353">Methods, instance constructors, indexers, and operators are characterized by their ***signatures***:</span></span>

*  <span data-ttu-id="b58e2-354">方法的簽章包含方法、 類型參數的數目和類型和類型 （值、 參考或輸出） 的每個型式參數，由左到右的順序的名稱。</span><span class="sxs-lookup"><span data-stu-id="b58e2-354">The signature of a method consists of the name of the method, the number of type parameters and the type and kind (value, reference, or output) of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="b58e2-355">對於這些用途，不是由其名稱，但類型引數清單中之方法的序數位置識別之方法的型式參數的型別，就會發生的任何型別參數。</span><span class="sxs-lookup"><span data-stu-id="b58e2-355">For these purposes, any type parameter of the method that occurs in the type of a formal parameter is identified not by its name, but by its ordinal position in the type argument list of the method.</span></span> <span data-ttu-id="b58e2-356">方法的簽章特別不包含傳回的型別，`params`修飾詞，可供指定的最右邊的參數或選擇性型別參數條件約束。</span><span class="sxs-lookup"><span data-stu-id="b58e2-356">The signature of a method specifically does not include the return type, the `params` modifier that may be specified for the right-most parameter, nor the optional type parameter constraints.</span></span>
*  <span data-ttu-id="b58e2-357">執行個體建構函式的簽章包含型別和類型 （值、 參考或輸出） 的每個型式參數，由左到右的順序。</span><span class="sxs-lookup"><span data-stu-id="b58e2-357">The signature of an instance constructor consists of the type and kind (value, reference, or output) of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="b58e2-358">執行個體建構函式的簽章特別不包含`params`修飾詞，可供指定的最右邊的參數。</span><span class="sxs-lookup"><span data-stu-id="b58e2-358">The signature of an instance constructor specifically does not include the `params` modifier that may be specified for the right-most parameter.</span></span>
*  <span data-ttu-id="b58e2-359">索引子的簽章所組成的每個型式參數，由左到右的順序型別。</span><span class="sxs-lookup"><span data-stu-id="b58e2-359">The signature of an indexer consists of the type of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="b58e2-360">索引子的簽章特別不包含此項目類型，也不會包含`params`修飾詞，可供指定的最右邊的參數。</span><span class="sxs-lookup"><span data-stu-id="b58e2-360">The signature of an indexer specifically does not include the element type, nor does it include the `params` modifier that may be specified for the right-most parameter.</span></span>
*  <span data-ttu-id="b58e2-361">運算子的簽章所組成的運算子和其型式參數，由左到右的順序的每個型別的名稱。</span><span class="sxs-lookup"><span data-stu-id="b58e2-361">The signature of an operator consists of the name of the operator and the type of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="b58e2-362">特別是運算子的簽章不包含的結果型別。</span><span class="sxs-lookup"><span data-stu-id="b58e2-362">The signature of an operator specifically does not include the result type.</span></span>

<span data-ttu-id="b58e2-363">簽章是啟用機制***多載***類別、 結構和介面中的成員：</span><span class="sxs-lookup"><span data-stu-id="b58e2-363">Signatures are the enabling mechanism for ***overloading*** of members in classes, structs, and interfaces:</span></span>

*  <span data-ttu-id="b58e2-364">方法的多載，可讓類別、 結構或介面宣告具有相同名稱，並提供它們的簽章的多個方法中是唯一的類別、 結構或介面。</span><span class="sxs-lookup"><span data-stu-id="b58e2-364">Overloading of methods permits a class, struct, or interface to declare multiple methods with the same name, provided their signatures are unique within that class, struct, or interface.</span></span>
*  <span data-ttu-id="b58e2-365">執行個體建構函式多載允許類別或結構宣告多個執行個體建構函式，前提是它們的簽章是該類別或結構內唯一的。</span><span class="sxs-lookup"><span data-stu-id="b58e2-365">Overloading of instance constructors permits a class or struct to declare multiple instance constructors, provided their signatures are unique within that class or struct.</span></span>
*  <span data-ttu-id="b58e2-366">多載的索引子允許類別、 結構或介面宣告多個索引子，前提是它們的簽章中的類別、 結構或介面的唯一的。</span><span class="sxs-lookup"><span data-stu-id="b58e2-366">Overloading of indexers permits a class, struct, or interface to declare multiple indexers, provided their signatures are unique within that class, struct, or interface.</span></span>
*  <span data-ttu-id="b58e2-367">多載的運算子允許類別或結構宣告具有相同名稱，並提供它們的簽章的多個運算子都是唯一的類別或結構內。</span><span class="sxs-lookup"><span data-stu-id="b58e2-367">Overloading of operators permits a class or struct to declare multiple operators with the same name, provided their signatures are unique within that class or struct.</span></span>

<span data-ttu-id="b58e2-368">雖然`out`並`ref`參數修飾詞會視為簽章的一部分，在單一類型中宣告的成員不能單獨由不同簽章中`ref`和`out`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-368">Although `out` and `ref` parameter modifiers are considered part of a signature, members declared in a single type cannot differ in signature solely by `ref` and `out`.</span></span> <span data-ttu-id="b58e2-369">如果兩個成員會宣告相同的型別會是相同的簽章中使用這兩種方法中的所有參數，就會發生編譯時期錯誤`out`修飾詞變更為`ref`修飾詞。</span><span class="sxs-lookup"><span data-stu-id="b58e2-369">A compile-time error occurs if two members are declared in the same type with signatures that would be the same if all parameters in both methods with `out` modifiers were changed to `ref` modifiers.</span></span> <span data-ttu-id="b58e2-370">用於簽章相符的其他用途 （例如隱藏或覆寫），`ref`和`out`視為簽章的一部分，且彼此不相符。</span><span class="sxs-lookup"><span data-stu-id="b58e2-370">For other purposes of signature matching (e.g., hiding or overriding), `ref` and `out` are considered part of the signature and do not match each other.</span></span> <span data-ttu-id="b58e2-371">(這項限制可讓C#程式來執行在 Common Language Infrastructure (CLI)，這不會提供方法來定義只會在不同的方法，輕易地轉譯`ref`並`out`。)</span><span class="sxs-lookup"><span data-stu-id="b58e2-371">(This restriction is to allow C#  programs to be easily translated to run on the Common Language Infrastructure (CLI), which does not provide a way to define methods that differ solely in `ref` and `out`.)</span></span>

<span data-ttu-id="b58e2-372">簽章類型基於`object`和`dynamic`都會視為相同。</span><span class="sxs-lookup"><span data-stu-id="b58e2-372">For the purposes of signatures, the types `object` and `dynamic` are considered the same.</span></span> <span data-ttu-id="b58e2-373">單一類型中宣告的成員可以因此是相同的簽章單獨由`object`和`dynamic`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-373">Members declared in a single type can therefore not differ in signature solely by `object` and `dynamic`.</span></span>

<span data-ttu-id="b58e2-374">下列範例會顯示一組多載的方法宣告，以及它們的簽章。</span><span class="sxs-lookup"><span data-stu-id="b58e2-374">The following example shows a set of overloaded method declarations along with their signatures.</span></span>
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

<span data-ttu-id="b58e2-375">請注意任何`ref`並`out`參數修飾詞 ([方法參數](classes.md#method-parameters)) 簽章的一部分。</span><span class="sxs-lookup"><span data-stu-id="b58e2-375">Note that any `ref` and `out` parameter modifiers ([Method parameters](classes.md#method-parameters)) are part of a signature.</span></span> <span data-ttu-id="b58e2-376">因此，`F(int)`和`F(ref int)`是唯一的簽章。</span><span class="sxs-lookup"><span data-stu-id="b58e2-376">Thus, `F(int)` and `F(ref int)` are unique signatures.</span></span> <span data-ttu-id="b58e2-377">不過，`F(ref int)`並`F(out int)`不能在相同的介面中宣告，因為它們的簽章單獨由不同`ref`和`out`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-377">However, `F(ref int)` and `F(out int)` cannot be declared within the same interface because their signatures differ solely by `ref` and `out`.</span></span> <span data-ttu-id="b58e2-378">另外，請注意，傳回型別和`params`修飾詞不屬於簽章，因此無法單獨根據傳回型別或包含或排除的多載`params`修飾詞。</span><span class="sxs-lookup"><span data-stu-id="b58e2-378">Also, note that the return type and the `params` modifier are not part of a signature, so it is not possible to overload solely based on return type or on the inclusion or exclusion of the `params` modifier.</span></span> <span data-ttu-id="b58e2-379">這樣一來，一種方法的宣告`F(int)`和`F(params string[])`上述導致編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="b58e2-379">As such, the declarations of the methods `F(int)` and `F(params string[])` identified above result in a compile-time error.</span></span>

## <a name="scopes"></a><span data-ttu-id="b58e2-380">範圍</span><span class="sxs-lookup"><span data-stu-id="b58e2-380">Scopes</span></span>

<span data-ttu-id="b58e2-381">***範圍***名稱是在其中是可以參考的名稱，但是不限定的名稱所宣告的實體的程式文字的區域。</span><span class="sxs-lookup"><span data-stu-id="b58e2-381">The ***scope*** of a name is the region of program text within which it is possible to refer to the entity declared by the name without qualification of the name.</span></span> <span data-ttu-id="b58e2-382">範圍可以***巢狀***，和內部範圍可能會重新宣告的名稱來自外部範圍的意義 (這不會不過，移除所加諸的限制[宣告](basic-concepts.md#declarations)巢狀區塊內不是可以宣告區域變數與封閉區塊中的區域變數名稱相同）。</span><span class="sxs-lookup"><span data-stu-id="b58e2-382">Scopes can be ***nested***, and an inner scope may redeclare the meaning of a name from an outer scope (this does not, however, remove the restriction imposed by [Declarations](basic-concepts.md#declarations) that within a nested block it is not possible to declare a local variable with the same name as a local variable in an enclosing block).</span></span> <span data-ttu-id="b58e2-383">來自外部範圍的名稱然後要***隱藏***區域中的程式文字涵蓋內部範圍中，而存取的外部名稱也僅能由限定名稱。</span><span class="sxs-lookup"><span data-stu-id="b58e2-383">The name from the outer scope is then said to be ***hidden*** in the region of program text covered by the inner scope, and access to the outer name is only possible by qualifying the name.</span></span>

*  <span data-ttu-id="b58e2-384">命名空間成員的範圍宣告*namespace_member_declaration* ([命名空間成員](namespaces.md#namespace-members)) 與沒有封入*namespace_declaration*是整個計劃文字。</span><span class="sxs-lookup"><span data-stu-id="b58e2-384">The scope of a namespace member declared by a *namespace_member_declaration* ([Namespace members](namespaces.md#namespace-members)) with no enclosing *namespace_declaration* is the entire program text.</span></span>
*  <span data-ttu-id="b58e2-385">命名空間成員的範圍宣告*namespace_member_declaration*內*namespace_declaration*其完整的名稱是`N`是*namespace_body*的每個*namespace_declaration*其完整的名稱是`N`開頭或`N`，後面接著句號。</span><span class="sxs-lookup"><span data-stu-id="b58e2-385">The scope of a namespace member declared by a *namespace_member_declaration* within a *namespace_declaration* whose fully qualified name is `N` is the *namespace_body* of every *namespace_declaration* whose fully qualified name is `N` or starts with `N`, followed by a period.</span></span>
*  <span data-ttu-id="b58e2-386">名稱所定義的範圍*extern_alias_directive*擴充*using_directive*s *global_attributes*並*namespace_member_宣告*立即包含編譯單位或命名空間主體的 s。</span><span class="sxs-lookup"><span data-stu-id="b58e2-386">The scope of name defined by an *extern_alias_directive* extends over the *using_directive*s, *global_attributes* and *namespace_member_declaration*s of its immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="b58e2-387">*Extern_alias_directive*並沒有提供給基礎的宣告空間的任何新成員。</span><span class="sxs-lookup"><span data-stu-id="b58e2-387">An *extern_alias_directive* does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="b58e2-388">亦即*extern_alias_directive*不是可轉移的但相反地，會影響僅編譯單位或命名空間主體中發生。</span><span class="sxs-lookup"><span data-stu-id="b58e2-388">In other words, an *extern_alias_directive* is not transitive, but, rather, affects only the compilation unit or namespace body in which it occurs.</span></span>
*  <span data-ttu-id="b58e2-389">名稱的範圍定義，或藉由匯入*using_directive* ([Using 指示詞](namespaces.md#using-directives)) 擴充*namespace_member_declaration*s 的*compilation_unit*或是*namespace_body*所在*using_directive* ，就會發生。</span><span class="sxs-lookup"><span data-stu-id="b58e2-389">The scope of a name defined or imported by a *using_directive* ([Using directives](namespaces.md#using-directives)) extends over the *namespace_member_declaration*s of the *compilation_unit* or *namespace_body* in which the *using_directive* occurs.</span></span> <span data-ttu-id="b58e2-390">A *using_directive*得零個或多個命名空間、 類型或成員名稱可以在特定*compilation_unit*或是*namespace_body*，但不會提供給基礎的宣告空間的任何新成員。</span><span class="sxs-lookup"><span data-stu-id="b58e2-390">A *using_directive* may make zero or more namespace, type or member names available within a particular *compilation_unit* or *namespace_body*, but does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="b58e2-391">亦即*using_directive*不是可轉移，但只會影響*compilation_unit*或是*namespace_body*它發生所在。</span><span class="sxs-lookup"><span data-stu-id="b58e2-391">In other words, a *using_directive* is not transitive but rather affects only the *compilation_unit* or *namespace_body* in which it occurs.</span></span>
*  <span data-ttu-id="b58e2-392">所宣告的型別參數的範圍*type_parameter_list*上*class_declaration* ([類別宣告](classes.md#class-declarations)) 是*class_base*， *type_parameter_constraints_clause*s，並*class_body*該*class_declaration*。</span><span class="sxs-lookup"><span data-stu-id="b58e2-392">The scope of a type parameter declared by a *type_parameter_list* on a *class_declaration* ([Class declarations](classes.md#class-declarations)) is the *class_base*, *type_parameter_constraints_clause*s, and *class_body* of that *class_declaration*.</span></span>
*  <span data-ttu-id="b58e2-393">所宣告的型別參數的範圍*type_parameter_list*上*struct_declaration* ([結構宣告](structs.md#struct-declarations)) 是*struct_interfaces*， *type_parameter_constraints_clause*s，並*struct_body*該*struct_declaration*。</span><span class="sxs-lookup"><span data-stu-id="b58e2-393">The scope of a type parameter declared by a *type_parameter_list* on a *struct_declaration* ([Struct declarations](structs.md#struct-declarations)) is the *struct_interfaces*, *type_parameter_constraints_clause*s, and *struct_body* of that *struct_declaration*.</span></span>
*  <span data-ttu-id="b58e2-394">所宣告的型別參數的範圍*type_parameter_list*上*interface_declaration* ([介面宣告](interfaces.md#interface-declarations)) 是*interface_base*， *type_parameter_constraints_clause*s，並*interface_body*該*interface_declaration*。</span><span class="sxs-lookup"><span data-stu-id="b58e2-394">The scope of a type parameter declared by a *type_parameter_list* on an *interface_declaration* ([Interface declarations](interfaces.md#interface-declarations)) is the *interface_base*, *type_parameter_constraints_clause*s, and *interface_body* of that *interface_declaration*.</span></span>
*  <span data-ttu-id="b58e2-395">所宣告的型別參數的範圍*type_parameter_list*上*delegate_declaration* ([委派宣告](delegates.md#delegate-declarations)) 是*return_type*， *formal_parameter_list*，以及*type_parameter_constraints_clause*的 s *delegate_declaration*。</span><span class="sxs-lookup"><span data-stu-id="b58e2-395">The scope of a type parameter declared by a *type_parameter_list* on a *delegate_declaration* ([Delegate declarations](delegates.md#delegate-declarations)) is the *return_type*, *formal_parameter_list*, and *type_parameter_constraints_clause*s of that *delegate_declaration*.</span></span>
*  <span data-ttu-id="b58e2-396">所宣告的成員範圍*class_member_declaration* ([類別主體](classes.md#class-body)) 會*class_body*中宣告已發生。</span><span class="sxs-lookup"><span data-stu-id="b58e2-396">The scope of a member declared by a *class_member_declaration* ([Class body](classes.md#class-body)) is the *class_body* in which the declaration occurs.</span></span> <span data-ttu-id="b58e2-397">此外，類別成員的範圍會延伸到*class_body*的衍生類別中的存取範圍領域包含 ([存取範圍定義域](basic-concepts.md#accessibility-domains)) 的成員。</span><span class="sxs-lookup"><span data-stu-id="b58e2-397">In addition, the scope of a class member extends to the *class_body* of those derived classes that are included in the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the member.</span></span>
*  <span data-ttu-id="b58e2-398">所宣告的成員範圍*struct_member_declaration* ([結構成員](structs.md#struct-members)) 會*struct_body*中宣告已發生。</span><span class="sxs-lookup"><span data-stu-id="b58e2-398">The scope of a member declared by a *struct_member_declaration* ([Struct members](structs.md#struct-members)) is the *struct_body* in which the declaration occurs.</span></span>
*  <span data-ttu-id="b58e2-399">所宣告的成員範圍*enum_member_declaration* ([列舉成員](enums.md#enum-members)) 會*enum_body*中宣告已發生。</span><span class="sxs-lookup"><span data-stu-id="b58e2-399">The scope of a member declared by an *enum_member_declaration*  ([Enum members](enums.md#enum-members)) is the *enum_body* in which the declaration occurs.</span></span>
*  <span data-ttu-id="b58e2-400">參數的範圍中宣告*method_declaration* ([方法](classes.md#methods)) 會*method_body*該*method_declaration*。</span><span class="sxs-lookup"><span data-stu-id="b58e2-400">The scope of a parameter declared in a *method_declaration* ([Methods](classes.md#methods)) is the *method_body* of that *method_declaration*.</span></span>
*  <span data-ttu-id="b58e2-401">參數的範圍中宣告*indexer_declaration* ([的索引子](classes.md#indexers)) 會*accessor_declarations*的*indexer_declaration*.</span><span class="sxs-lookup"><span data-stu-id="b58e2-401">The scope of a parameter declared in an *indexer_declaration* ([Indexers](classes.md#indexers)) is the *accessor_declarations* of that *indexer_declaration*.</span></span>
*  <span data-ttu-id="b58e2-402">參數的範圍中宣告*operator_declaration* ([運算子](classes.md#operators)) 會*區塊*該*operator_declaration*。</span><span class="sxs-lookup"><span data-stu-id="b58e2-402">The scope of a parameter declared in an *operator_declaration* ([Operators](classes.md#operators)) is the *block* of that *operator_declaration*.</span></span>
*  <span data-ttu-id="b58e2-403">參數的範圍中宣告*constructor_declaration* ([執行個體建構函式](classes.md#instance-constructors)) 會*constructor_initializer*並*區塊*該*constructor_declaration*。</span><span class="sxs-lookup"><span data-stu-id="b58e2-403">The scope of a parameter declared in a *constructor_declaration* ([Instance constructors](classes.md#instance-constructors)) is the *constructor_initializer* and *block* of that *constructor_declaration*.</span></span>
*  <span data-ttu-id="b58e2-404">參數的範圍中宣告*lambda_expression* ([匿名函式運算式](expressions.md#anonymous-function-expressions)) 會*anonymous_function_body*的*lambda_運算式*</span><span class="sxs-lookup"><span data-stu-id="b58e2-404">The scope of a parameter declared in a *lambda_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) is the *anonymous_function_body* of that *lambda_expression*</span></span>
*  <span data-ttu-id="b58e2-405">參數的範圍中宣告*anonymous_method_expression* ([匿名函式運算式](expressions.md#anonymous-function-expressions)) 會*區塊*的*anonymous_method_expression*。</span><span class="sxs-lookup"><span data-stu-id="b58e2-405">The scope of a parameter declared in an *anonymous_method_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) is the *block* of that *anonymous_method_expression*.</span></span>
*  <span data-ttu-id="b58e2-406">標記的範圍中宣告*labeled_statement* ([標示為陳述式](statements.md#labeled-statements)) 會*區塊*中宣告已發生。</span><span class="sxs-lookup"><span data-stu-id="b58e2-406">The scope of a label declared in a *labeled_statement* ([Labeled statements](statements.md#labeled-statements)) is the *block* in which the declaration occurs.</span></span>
*  <span data-ttu-id="b58e2-407">在宣告的本機變數的範圍*local_variable_declaration* ([區域變數宣告](statements.md#local-variable-declarations)) 區塊中宣告已發生。</span><span class="sxs-lookup"><span data-stu-id="b58e2-407">The scope of a local variable declared in a *local_variable_declaration* ([Local variable declarations](statements.md#local-variable-declarations)) is the block in which the declaration occurs.</span></span>
*  <span data-ttu-id="b58e2-408">範圍中宣告的區域變數*switch_block*的`switch`陳述式 ([switch 陳述式](statements.md#the-switch-statement)) 是*switch_block*。</span><span class="sxs-lookup"><span data-stu-id="b58e2-408">The scope of a local variable declared in a *switch_block* of a `switch` statement ([The switch statement](statements.md#the-switch-statement)) is the *switch_block*.</span></span>
*  <span data-ttu-id="b58e2-409">範圍中宣告的區域變數*for_initializer*的`for`陳述式 ([陳述式](statements.md#the-for-statement)) 是*for_initializer*， *for_condition*，則*for_iterator*，並包含*陳述式*的`for`陳述式。</span><span class="sxs-lookup"><span data-stu-id="b58e2-409">The scope of a local variable declared in a *for_initializer* of a `for` statement ([The for statement](statements.md#the-for-statement)) is the *for_initializer*, the *for_condition*, the *for_iterator*, and the contained *statement* of the `for` statement.</span></span>
*  <span data-ttu-id="b58e2-410">本機常數的範圍中宣告*local_constant_declaration* ([區域常數宣告](statements.md#local-constant-declarations)) 是在區塊中宣告已發生。</span><span class="sxs-lookup"><span data-stu-id="b58e2-410">The scope of a local constant declared in a *local_constant_declaration* ([Local constant declarations](statements.md#local-constant-declarations)) is the block in which the declaration occurs.</span></span> <span data-ttu-id="b58e2-411">它是編譯時期錯誤之前的文字位置的區域常數是指其*constant_declarator*。</span><span class="sxs-lookup"><span data-stu-id="b58e2-411">It is a compile-time error to refer to a local constant in a textual position that precedes its *constant_declarator*.</span></span>
*  <span data-ttu-id="b58e2-412">變數的範圍宣告的一部分*foreach_statement*， *using_statement*， *lock_statement*或是*query_expression>* 是取決於擴充的指定建構。</span><span class="sxs-lookup"><span data-stu-id="b58e2-412">The scope of a variable declared as part of a *foreach_statement*, *using_statement*, *lock_statement* or *query_expression* is determined by the expansion of the given construct.</span></span>

<span data-ttu-id="b58e2-413">範圍內的命名空間、 類別、 結構或列舉的成員就可以參考之前的成員宣告的文字位置中的成員。</span><span class="sxs-lookup"><span data-stu-id="b58e2-413">Within the scope of a namespace, class, struct, or enumeration member it is possible to refer to the member in a textual position that precedes the declaration of the member.</span></span> <span data-ttu-id="b58e2-414">例如</span><span class="sxs-lookup"><span data-stu-id="b58e2-414">For example</span></span>
```csharp
class A
{
    void F() {
        i = 1;
    }

    int i = 0;
}
```
<span data-ttu-id="b58e2-415">在這裡，它是適用於`F`來參考`i`其宣告之前。</span><span class="sxs-lookup"><span data-stu-id="b58e2-415">Here, it is valid for `F` to refer to `i` before it is declared.</span></span>

<span data-ttu-id="b58e2-416">本機變數的範圍，它是參考之前的文字位置的本機變數的編譯時期錯誤*local_variable_declarator*的區域變數。</span><span class="sxs-lookup"><span data-stu-id="b58e2-416">Within the scope of a local variable, it is a compile-time error to refer to the local variable in a textual position that precedes the *local_variable_declarator* of the local variable.</span></span> <span data-ttu-id="b58e2-417">例如</span><span class="sxs-lookup"><span data-stu-id="b58e2-417">For example</span></span>
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

<span data-ttu-id="b58e2-418">在 `F`上述的方法中，第一次指派給`i`特別未參考外部範圍中宣告的欄位。</span><span class="sxs-lookup"><span data-stu-id="b58e2-418">In the `F` method above, the first assignment to `i` specifically does not refer to the field declared in the outer scope.</span></span> <span data-ttu-id="b58e2-419">相反地，本機變數所指的是，則會導致編譯時期錯誤因為賦予在前面的變數宣告。</span><span class="sxs-lookup"><span data-stu-id="b58e2-419">Rather, it refers to the local variable and it results in a compile-time error because it textually precedes the declaration of the variable.</span></span> <span data-ttu-id="b58e2-420">在 `G`方法中，使用`j`中宣告的初始設定式`j`是有效的因為前面使用沒有*local_variable_declarator*。</span><span class="sxs-lookup"><span data-stu-id="b58e2-420">In the `G` method, the use of `j` in the initializer for the declaration of `j` is valid because the use does not precede the *local_variable_declarator*.</span></span> <span data-ttu-id="b58e2-421">在`H`方法，後續*local_variable_declarator*正確是指在稍早宣告的本機變數*local_variable_declarator*在相同*local_variable_declaration*。</span><span class="sxs-lookup"><span data-stu-id="b58e2-421">In the `H` method, a subsequent *local_variable_declarator* correctly refers to a local variable declared in an earlier *local_variable_declarator* within the same *local_variable_declaration*.</span></span>

<span data-ttu-id="b58e2-422">本機變數的範圍規則被設計來保證運算式的內容中使用名稱的意義永遠相同區塊內。</span><span class="sxs-lookup"><span data-stu-id="b58e2-422">The scoping rules for local variables are designed to guarantee that the meaning of a name used in an expression context is always the same within a block.</span></span> <span data-ttu-id="b58e2-423">如果本機變數的範圍將僅從其宣告區塊的結尾，在上述範例中，第一次指派會將指派給執行個體變數，然後第二項指派會指派給本機變數，可能會導致稍後會重新排列區塊的陳述式一樣，就會產生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="b58e2-423">If the scope of a local variable were to extend only from its declaration to the end of the block, then in the example above, the first assignment would assign to the instance variable and the second assignment would assign to the local variable, possibly leading to compile-time errors if the statements of the block were later to be rearranged.</span></span>

<span data-ttu-id="b58e2-424">區塊內的名稱的意義可能有所不同的內容中使用的名稱。</span><span class="sxs-lookup"><span data-stu-id="b58e2-424">The meaning of a name within a block may differ based on the context in which the name is used.</span></span> <span data-ttu-id="b58e2-425">在範例</span><span class="sxs-lookup"><span data-stu-id="b58e2-425">In the example</span></span>
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
<span data-ttu-id="b58e2-426">名稱`A`運算式內容中使用參考本機變數`A`來參考類別類型內容中`A`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-426">the name `A` is used in an expression context to refer to the local variable `A` and in a type context to refer to the class `A`.</span></span>

### <a name="name-hiding"></a><span data-ttu-id="b58e2-427">隱藏名稱</span><span class="sxs-lookup"><span data-stu-id="b58e2-427">Name hiding</span></span>

<span data-ttu-id="b58e2-428">實體的範圍通常包含比實體的宣告空間更多的程式文字。</span><span class="sxs-lookup"><span data-stu-id="b58e2-428">The scope of an entity typically encompasses more program text than the declaration space of the entity.</span></span> <span data-ttu-id="b58e2-429">特別是，實體的範圍可能包含引入新的宣告空間，包含具有相同名稱的實體宣告。</span><span class="sxs-lookup"><span data-stu-id="b58e2-429">In particular, the scope of an entity may include declarations that introduce new declaration spaces containing entities of the same name.</span></span> <span data-ttu-id="b58e2-430">這類宣告會造成原始實體變成***隱藏***。</span><span class="sxs-lookup"><span data-stu-id="b58e2-430">Such declarations cause the original entity to become ***hidden***.</span></span> <span data-ttu-id="b58e2-431">相反地，實體要***可見***未隱藏時。</span><span class="sxs-lookup"><span data-stu-id="b58e2-431">Conversely, an entity is said to be ***visible*** when it is not hidden.</span></span>

<span data-ttu-id="b58e2-432">透過巢狀結構，和透過繼承的範圍重疊時的範圍重疊時，就會發生名稱隱藏。</span><span class="sxs-lookup"><span data-stu-id="b58e2-432">Name hiding occurs when scopes overlap through nesting and when scopes overlap through inheritance.</span></span> <span data-ttu-id="b58e2-433">隱藏的兩種類型的特性是由下列各節所述。</span><span class="sxs-lookup"><span data-stu-id="b58e2-433">The characteristics of the two types of hiding are described in the following sections.</span></span>

#### <a name="hiding-through-nesting"></a><span data-ttu-id="b58e2-434">透過巢狀的隱藏</span><span class="sxs-lookup"><span data-stu-id="b58e2-434">Hiding through nesting</span></span>

<span data-ttu-id="b58e2-435">隱藏透過巢狀的名稱可能是由於巢狀命名空間或命名空間、 巢狀類別或結構內，因為參數和區域變數宣告的型別中的類型。</span><span class="sxs-lookup"><span data-stu-id="b58e2-435">Name hiding through nesting can occur as a result of nesting namespaces or types within namespaces, as a result of nesting types within classes or structs, and as a result of parameter and local variable declarations.</span></span>

<span data-ttu-id="b58e2-436">在範例</span><span class="sxs-lookup"><span data-stu-id="b58e2-436">In the example</span></span>
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
<span data-ttu-id="b58e2-437">內`F`方法中，執行個體變數`i`隱藏區域變數`i`，但內`G`方法，`i`仍然是指執行個體變數。</span><span class="sxs-lookup"><span data-stu-id="b58e2-437">within the `F` method, the instance variable `i` is hidden by the local variable `i`, but within the `G` method, `i` still refers to the instance variable.</span></span>

<span data-ttu-id="b58e2-438">當內部範圍中的名稱會隱藏外部範圍中的名稱時，它會隱藏該名稱的所有多載的項目。</span><span class="sxs-lookup"><span data-stu-id="b58e2-438">When a name in an inner scope hides a name in an outer scope, it hides all overloaded occurrences of that name.</span></span> <span data-ttu-id="b58e2-439">在範例</span><span class="sxs-lookup"><span data-stu-id="b58e2-439">In the example</span></span>
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
<span data-ttu-id="b58e2-440">在呼叫`F(1)`叫用`F`中所宣告`Inner`因為的所有外部的項目`F`內部的宣告會隱藏。</span><span class="sxs-lookup"><span data-stu-id="b58e2-440">the call `F(1)` invokes the `F` declared in `Inner` because all outer occurrences of `F` are hidden by the inner declaration.</span></span> <span data-ttu-id="b58e2-441">基於相同理由，也就是呼叫`F("Hello")`會導致編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="b58e2-441">For the same reason, the call `F("Hello")` results in a compile-time error.</span></span>

#### <a name="hiding-through-inheritance"></a><span data-ttu-id="b58e2-442">經由繼承隱藏</span><span class="sxs-lookup"><span data-stu-id="b58e2-442">Hiding through inheritance</span></span>

<span data-ttu-id="b58e2-443">當類別或結構重新宣告繼承自基底類別的名稱時，就會發生經由繼承隱藏的名稱。</span><span class="sxs-lookup"><span data-stu-id="b58e2-443">Name hiding through inheritance occurs when classes or structs redeclare names that were inherited from base classes.</span></span> <span data-ttu-id="b58e2-444">這種類型的名稱隱藏採用下列格式之一：</span><span class="sxs-lookup"><span data-stu-id="b58e2-444">This type of name hiding takes one of the following forms:</span></span>

*  <span data-ttu-id="b58e2-445">常數、 欄位、 屬性、 事件或類別或結構中引入的型別會隱藏所有具有相同名稱的基底類別成員。</span><span class="sxs-lookup"><span data-stu-id="b58e2-445">A constant, field, property, event, or type introduced in a class or struct hides all base class members with the same name.</span></span>
*  <span data-ttu-id="b58e2-446">所有非方法的基底類別成員具有相同的名稱，並使用相同的簽章 （方法名稱和參數計數、 修飾詞和型別） 的所有基底類別方法，則會隱藏在類別或結構中引入的方法。</span><span class="sxs-lookup"><span data-stu-id="b58e2-446">A method introduced in a class or struct hides all non-method base class members with the same name, and all base class methods with the same signature (method name and parameter count, modifiers, and types).</span></span>
*  <span data-ttu-id="b58e2-447">在類別或結構中引入的索引子會隱藏所有具有相同的簽章 （參數計數和型別） 的基底類別索引子。</span><span class="sxs-lookup"><span data-stu-id="b58e2-447">An indexer introduced in a class or struct hides all base class indexers with the same signature (parameter count and types).</span></span>

<span data-ttu-id="b58e2-448">控管運算子宣告的規則 ([運算子](classes.md#operators)) 將會導致在衍生類別宣告基底類別中的運算子具有相同的簽章的運算子。</span><span class="sxs-lookup"><span data-stu-id="b58e2-448">The rules governing operator declarations ([Operators](classes.md#operators)) make it impossible for a derived class to declare an operator with the same signature as an operator in a base class.</span></span> <span data-ttu-id="b58e2-449">因此，運算子永遠不會隱藏另一個。</span><span class="sxs-lookup"><span data-stu-id="b58e2-449">Thus, operators never hide one another.</span></span>

<span data-ttu-id="b58e2-450">相對於隱藏外部範圍中的名稱，隱藏繼承的範圍從可存取的名稱，將會導致要報告警告。</span><span class="sxs-lookup"><span data-stu-id="b58e2-450">Contrary to hiding a name from an outer scope, hiding an accessible name from an inherited scope causes a warning to be reported.</span></span> <span data-ttu-id="b58e2-451">在範例</span><span class="sxs-lookup"><span data-stu-id="b58e2-451">In the example</span></span>
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
<span data-ttu-id="b58e2-452">deklarace`F`在`Derived`會回報警告。</span><span class="sxs-lookup"><span data-stu-id="b58e2-452">the declaration of `F` in `Derived` causes a warning to be reported.</span></span> <span data-ttu-id="b58e2-453">隱藏繼承的名稱是刻意不發生錯誤，因為這樣會妨礙個別發展的基底類別。</span><span class="sxs-lookup"><span data-stu-id="b58e2-453">Hiding an inherited name is specifically not an error, since that would preclude separate evolution of base classes.</span></span> <span data-ttu-id="b58e2-454">例如，上述這種情況就可能會發生因為較新版`Base`導入`F`未出現在較早版本的類別中的方法。</span><span class="sxs-lookup"><span data-stu-id="b58e2-454">For example, the above situation might have come about because a later version of `Base` introduced an `F` method that wasn't present in an earlier version of the class.</span></span> <span data-ttu-id="b58e2-455">上述這種情況已發生錯誤，然後分開建立版本的類別庫中的基底類別所做的任何變更，都可能會讓衍生的類別，變成無效。</span><span class="sxs-lookup"><span data-stu-id="b58e2-455">Had the above situation been an error, then any change made to a base class in a separately versioned class library could potentially cause derived classes to become invalid.</span></span>

<span data-ttu-id="b58e2-456">隱藏繼承的名稱所造成的警告會消除，可以透過使用`new`修飾詞：</span><span class="sxs-lookup"><span data-stu-id="b58e2-456">The warning caused by hiding an inherited name can be eliminated through use of the `new` modifier:</span></span>
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

<span data-ttu-id="b58e2-457">`new`修飾詞表示`F`在`Derived`是 「 新增 」，而且，它確實能隱藏繼承的成員。</span><span class="sxs-lookup"><span data-stu-id="b58e2-457">The `new` modifier indicates that the `F` in `Derived` is "new", and that it is indeed intended to hide the inherited member.</span></span>

<span data-ttu-id="b58e2-458">新成員的宣告會隱藏繼承的成員只在新成員的範圍內。</span><span class="sxs-lookup"><span data-stu-id="b58e2-458">A declaration of a new member hides an inherited member only within the scope of the new member.</span></span>

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

<span data-ttu-id="b58e2-459">在範例中的宣告之上`F`中`Derived`隱藏`F`，繼承自`Base`，但由於新`F`中`Derived`具有私人存取權，其範圍不會延伸到`MoreDerived`.</span><span class="sxs-lookup"><span data-stu-id="b58e2-459">In the example above, the declaration of `F` in `Derived` hides the `F` that was inherited from `Base`, but since the new `F` in `Derived` has private access, its scope does not extend to `MoreDerived`.</span></span> <span data-ttu-id="b58e2-460">因此，呼叫`F()`中`MoreDerived.G`有效，且將會叫用`Base.F`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-460">Thus, the call `F()` in `MoreDerived.G` is valid and will invoke `Base.F`.</span></span>

## <a name="namespace-and-type-names"></a><span data-ttu-id="b58e2-461">命名空間和類型名稱</span><span class="sxs-lookup"><span data-stu-id="b58e2-461">Namespace and type names</span></span>

<span data-ttu-id="b58e2-462">中的數種內容C#程式需要*namespace_name*或是*type_name*來指定。</span><span class="sxs-lookup"><span data-stu-id="b58e2-462">Several contexts in a C# program require a *namespace_name* or a *type_name* to be specified.</span></span>

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

<span data-ttu-id="b58e2-463">A *namespace_name*是*namespace_or_type_name*參考命名空間。</span><span class="sxs-lookup"><span data-stu-id="b58e2-463">A *namespace_name* is a *namespace_or_type_name* that refers to a namespace.</span></span> <span data-ttu-id="b58e2-464">接下來的解析，如下所述*namespace_or_type_name*的*namespace_name*必須參考的命名空間，或否則會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="b58e2-464">Following resolution as described below, the *namespace_or_type_name* of a *namespace_name* must refer to a namespace, or otherwise a compile-time error occurs.</span></span> <span data-ttu-id="b58e2-465">任何類型引數 ([型別引數](types.md#type-arguments)) 中可以存在*namespace_name* （僅限型別可以有型別引數）。</span><span class="sxs-lookup"><span data-stu-id="b58e2-465">No type arguments ([Type arguments](types.md#type-arguments)) can be present in a *namespace_name* (only types can have type arguments).</span></span>

<span data-ttu-id="b58e2-466">A *type_name*是*namespace_or_type_name* ，參考型別。</span><span class="sxs-lookup"><span data-stu-id="b58e2-466">A *type_name* is a *namespace_or_type_name* that refers to a type.</span></span> <span data-ttu-id="b58e2-467">接下來的解析，如下所述*namespace_or_type_name*的*type_name*必須參考型別，否則會發生編譯時期錯誤或。</span><span class="sxs-lookup"><span data-stu-id="b58e2-467">Following resolution as described below, the *namespace_or_type_name* of a *type_name* must refer to a type, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="b58e2-468">如果*namespace_or_type_name*限定-別名-成員中所述，是它的意義[命名空間別名限定詞](namespaces.md#namespace-alias-qualifiers)。</span><span class="sxs-lookup"><span data-stu-id="b58e2-468">If the *namespace_or_type_name* is a qualified-alias-member its meaning is as described in [Namespace alias qualifiers](namespaces.md#namespace-alias-qualifiers).</span></span> <span data-ttu-id="b58e2-469">否則，請*namespace_or_type_name*有四種形式之一：</span><span class="sxs-lookup"><span data-stu-id="b58e2-469">Otherwise, a *namespace_or_type_name* has one of four forms:</span></span>

*  `I`
*  `I<A1, ..., Ak>`
*  `N.I`
*  `N.I<A1, ..., Ak>`

<span data-ttu-id="b58e2-470">何處`I`是單一的識別項，`N`是*namespace_or_type_name*並`<A1, ..., Ak>`是選擇性*type_argument_list*。</span><span class="sxs-lookup"><span data-stu-id="b58e2-470">where `I` is a single identifier, `N` is a *namespace_or_type_name* and `<A1, ..., Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="b58e2-471">若未*type_argument_list*會指定，請考慮`k`為零。</span><span class="sxs-lookup"><span data-stu-id="b58e2-471">When no *type_argument_list* is specified, consider `k` to be zero.</span></span>

<span data-ttu-id="b58e2-472">意義*namespace_or_type_name*判斷方式如下：</span><span class="sxs-lookup"><span data-stu-id="b58e2-472">The meaning of a *namespace_or_type_name* is determined as follows:</span></span>

*   <span data-ttu-id="b58e2-473">如果*namespace_or_type_name*的形式`I`，或格式`I<A1, ..., Ak>`:</span><span class="sxs-lookup"><span data-stu-id="b58e2-473">If the *namespace_or_type_name* is of the form `I` or of the form `I<A1, ..., Ak>`:</span></span>
    * <span data-ttu-id="b58e2-474">如果`K`為零， *namespace_or_type_name*出現在泛型方法宣告中 ([方法](classes.md#methods))，如果宣告包含型別參數 ([類型參數](classes.md#type-parameters)) 名稱 `I`，則*namespace_or_type_name*指的是該型別參數。</span><span class="sxs-lookup"><span data-stu-id="b58e2-474">If `K` is zero and the *namespace_or_type_name* appears within a generic method declaration ([Methods](classes.md#methods)) and if that declaration includes a type parameter ([Type parameters](classes.md#type-parameters)) with name `I`, then the *namespace_or_type_name* refers to that type parameter.</span></span>
    * <span data-ttu-id="b58e2-475">否則，如果*namespace_or_type_name*會出現在類型宣告中，然後針對每個執行個體類型 `T`([執行個體類型](classes.md#the-instance-type))，從該類型的執行個體類型宣告，並繼續每個封入類別或結構宣告的執行個體類型 （如果有的話）：</span><span class="sxs-lookup"><span data-stu-id="b58e2-475">Otherwise, if the *namespace_or_type_name* appears within a type declaration, then for each instance type `T` ([The instance type](classes.md#the-instance-type)), starting with the instance type of that type declaration and continuing with the instance type of each enclosing class or struct declaration (if any):</span></span>
        * <span data-ttu-id="b58e2-476">如果`K`是零，宣告`T`包含名稱的型別參數 `I`，然後在*namespace_or_type_name*指的是該型別參數。</span><span class="sxs-lookup"><span data-stu-id="b58e2-476">If `K` is zero and the declaration of `T` includes a type parameter with name `I`, then the *namespace_or_type_name* refers to that type parameter.</span></span>
        * <span data-ttu-id="b58e2-477">否則，如果*namespace_or_type_name*出現在類型宣告的主體內並`T`或其任何基底型別包含具有名稱的巢狀可存取型別 `I`和`K`  型別參數，則*namespace_or_type_name*建構具有指定的型別引數的型別參考。</span><span class="sxs-lookup"><span data-stu-id="b58e2-477">Otherwise, if the *namespace_or_type_name* appears within the body of the type declaration, and `T` or any of its base types contain a nested accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span> <span data-ttu-id="b58e2-478">如果沒有這樣的多個型別，會選取衍生程度較大的型別內宣告的型別。</span><span class="sxs-lookup"><span data-stu-id="b58e2-478">If there is more than one such type, the type declared within the more derived type is selected.</span></span> <span data-ttu-id="b58e2-479">請注意，非類型成員 （常數、 欄位、 方法、 屬性、 索引子、 運算子、 執行個體建構函式、 解構函式和靜態建構函式） 和具有不同數目的型別參數的型別成員時，會忽略判斷的意義*namespace_or_type_name*。</span><span class="sxs-lookup"><span data-stu-id="b58e2-479">Note that non-type members (constants, fields, methods, properties, indexers, operators, instance constructors, destructors, and static constructors) and type members with a different number of type parameters are ignored when determining the meaning of the *namespace_or_type_name*.</span></span>
    * <span data-ttu-id="b58e2-480">如果不成功，然後，每個命名空間的前一個步驟 `N`，從命名空間，其中*namespace_or_type_name*發生時，每個封入命名空間 （如果有的話），繼續進行，結尾為全域命名空間，直到找到實體為止，會評估下列步驟：</span><span class="sxs-lookup"><span data-stu-id="b58e2-480">If the previous steps were unsuccessful then, for each namespace `N`, starting with the namespace in which the *namespace_or_type_name* occurs, continuing with each enclosing namespace (if any), and ending with the global namespace, the following steps are evaluated until an entity is located:</span></span>
        * <span data-ttu-id="b58e2-481">如果`K`為零並`I`中的命名空間名稱 `N`，然後：</span><span class="sxs-lookup"><span data-stu-id="b58e2-481">If `K` is zero and `I` is the name of a namespace in `N`, then:</span></span>
            * <span data-ttu-id="b58e2-482">如果位置所在*namespace_or_type_name*就會發生加上命名空間宣告`N`且命名空間宣告包含*extern_alias_directive*或*using_alias_directive* ，將名稱產生關聯 `I`命名空間或類型，則*namespace_or_type_name*模稜兩可並發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="b58e2-482">If the location where the *namespace_or_type_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *namespace_or_type_name* is ambiguous and a compile-time error occurs.</span></span>
            * <span data-ttu-id="b58e2-483">否則，請*namespace_or_type_name*名為命名空間是指`I`在`N`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-483">Otherwise, the *namespace_or_type_name* refers to the namespace named `I` in `N`.</span></span>
        * <span data-ttu-id="b58e2-484">否則，如果`N`包含可存取的型別具有名稱 `I`並`K` 型別參數，然後：</span><span class="sxs-lookup"><span data-stu-id="b58e2-484">Otherwise, if `N` contains an accessible type having name `I` and `K` type parameters, then:</span></span>
            * <span data-ttu-id="b58e2-485">如果`K`是零，位置所在*namespace_or_type_name*就會發生加上命名空間宣告`N`和命名空間宣告包含*extern_alias_directive*或*using_alias_directive* ，將關聯名稱 `I`命名空間或類型，則*namespace_or_type_name*是模稜兩可和編譯時間會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="b58e2-485">If `K` is zero and the location where the *namespace_or_type_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *namespace_or_type_name* is ambiguous and a compile-time error occurs.</span></span>
            * <span data-ttu-id="b58e2-486">否則，請*namespace_or_type_name*指的是使用指定的型別引數建構的類型。</span><span class="sxs-lookup"><span data-stu-id="b58e2-486">Otherwise, the *namespace_or_type_name* refers to the type constructed with the given type arguments.</span></span>
        * <span data-ttu-id="b58e2-487">否則，如果位置所在*namespace_or_type_name*就會發生加上命名空間宣告`N`:</span><span class="sxs-lookup"><span data-stu-id="b58e2-487">Otherwise, if the location where the *namespace_or_type_name* occurs is enclosed by a namespace declaration for `N`:</span></span>
            * <span data-ttu-id="b58e2-488">如果`K`為零，而命名空間宣告包含*extern_alias_directive*或是*using_alias_directive*名稱建立關聯的 `I`與匯入的命名空間或型別，則*namespace_or_type_name*參考到該命名空間或型別。</span><span class="sxs-lookup"><span data-stu-id="b58e2-488">If `K` is zero and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with an imported namespace or type, then the *namespace_or_type_name* refers to that namespace or type.</span></span>
            * <span data-ttu-id="b58e2-489">否則，如果所匯入的命名空間和類型的宣告*using_namespace_directive*s 並*using_alias_directive*的命名空間宣告包含一個可存取的型別具有名稱 `I`並`K` 型別參數，則*namespace_or_type_name*建構具有指定的型別引數的型別參考。</span><span class="sxs-lookup"><span data-stu-id="b58e2-489">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_alias_directive*s of the namespace declaration contain exactly one accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span>
            * <span data-ttu-id="b58e2-490">否則，如果所匯入的命名空間和類型的宣告*using_namespace_directive*s 並*using_alias_directive*的命名空間宣告包含一個以上的存取類型具有名稱 `I`並`K` 型別參數，則*namespace_or_type_name*模稜兩可並發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="b58e2-490">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_alias_directive*s of the namespace declaration contain more than one accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* is ambiguous and an error occurs.</span></span>
    * <span data-ttu-id="b58e2-491">否則，請*namespace_or_type_name*是未定義，而且會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="b58e2-491">Otherwise, the *namespace_or_type_name* is undefined and a compile-time error occurs.</span></span>
*  <span data-ttu-id="b58e2-492">否則，請*namespace_or_type_name*的形式`N.I`，或格式`N.I<A1, ..., Ak>`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-492">Otherwise, the *namespace_or_type_name* is of the form `N.I` or of the form `N.I<A1, ..., Ak>`.</span></span> <span data-ttu-id="b58e2-493">`N` 會先解析成*namespace_or_type_name*。</span><span class="sxs-lookup"><span data-stu-id="b58e2-493">`N` is first resolved as a *namespace_or_type_name*.</span></span> <span data-ttu-id="b58e2-494">如果解析`N`不成功，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="b58e2-494">If the resolution of `N` is not successful, a compile-time error occurs.</span></span> <span data-ttu-id="b58e2-495">否則，請`N.I`或`N.I<A1, ..., Ak>`問題解決之後，如下所示：</span><span class="sxs-lookup"><span data-stu-id="b58e2-495">Otherwise, `N.I` or `N.I<A1, ..., Ak>` is resolved as follows:</span></span>
    * <span data-ttu-id="b58e2-496">如果`K`為零並`N`命名空間是指和`N`包含具有名稱的巢狀命名空間`I`，則*namespace_or_type_name*是指該巢狀命名空間。</span><span class="sxs-lookup"><span data-stu-id="b58e2-496">If `K` is zero and `N` refers to a namespace and `N` contains a nested namespace with name `I`, then the *namespace_or_type_name* refers to that nested namespace.</span></span>
    * <span data-ttu-id="b58e2-497">否則，如果`N`所參考的命名空間並`N`包含可存取的型別具有名稱 `I`並`K` 型別參數，則*namespace_or_type_name*指的是若要建構使用指定的型別引數的型別。</span><span class="sxs-lookup"><span data-stu-id="b58e2-497">Otherwise, if `N` refers to a namespace and `N` contains an accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span>
    * <span data-ttu-id="b58e2-498">否則，如果`N`參考 （可能是建構） 的類別或結構類型和`N`或任何其基底類別包含具有名稱的巢狀可存取類型 `I`並`K` 類型參數，則*namespace_or_type_name*建構具有指定的型別引數的型別參考。</span><span class="sxs-lookup"><span data-stu-id="b58e2-498">Otherwise, if `N` refers to a (possibly constructed) class or struct type and `N` or any of its base classes contain a nested accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span> <span data-ttu-id="b58e2-499">如果沒有這樣的多個型別，會選取衍生程度較大的型別內宣告的型別。</span><span class="sxs-lookup"><span data-stu-id="b58e2-499">If there is more than one such type, the type declared within the more derived type is selected.</span></span> <span data-ttu-id="b58e2-500">請注意，如果的意義`N.I`解析的基底類別規格的一部分會決定`N`然後的直接基底類別`N`會被視為物件 ([基底類別](classes.md#base-classes))。</span><span class="sxs-lookup"><span data-stu-id="b58e2-500">Note that if the meaning of `N.I` is being determined as part of resolving the base class specification of `N` then the direct base class of `N` is considered to be object ([Base classes](classes.md#base-classes)).</span></span>
    * <span data-ttu-id="b58e2-501">否則，請`N.I`是無效*namespace_or_type_name*，並發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="b58e2-501">Otherwise, `N.I` is an invalid *namespace_or_type_name*, and a compile-time error occurs.</span></span>

<span data-ttu-id="b58e2-502">A *namespace_or_type_name*允許參考靜態類別 ([靜態類別](classes.md#static-classes)) 只有當</span><span class="sxs-lookup"><span data-stu-id="b58e2-502">A *namespace_or_type_name* is permitted to reference a static class ([Static classes](classes.md#static-classes)) only if</span></span>

*  <span data-ttu-id="b58e2-503">*Namespace_or_type_name*是`T`中*namespace_or_type_name*格式的`T.I`，或</span><span class="sxs-lookup"><span data-stu-id="b58e2-503">The *namespace_or_type_name* is the `T` in a *namespace_or_type_name* of the form `T.I`, or</span></span>
*  <span data-ttu-id="b58e2-504">*Namespace_or_type_name*是`T`中*typeof_expression* ([引數清單](expressions.md#argument-lists)1) 的形式`typeof(T)`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-504">The *namespace_or_type_name* is the `T` in a *typeof_expression* ([Argument lists](expressions.md#argument-lists)1) of the form `typeof(T)`.</span></span>

### <a name="fully-qualified-names"></a><span data-ttu-id="b58e2-505">完整限定的名稱</span><span class="sxs-lookup"><span data-stu-id="b58e2-505">Fully qualified names</span></span>

<span data-ttu-id="b58e2-506">每個命名空間和型別都***完整限定的名稱***，可唯一識別的命名空間或在所有其他項目之間的型別。</span><span class="sxs-lookup"><span data-stu-id="b58e2-506">Every namespace and type has a ***fully qualified name***, which uniquely identifies the namespace or type amongst all others.</span></span> <span data-ttu-id="b58e2-507">命名空間或型別的完整格式的名稱`N`判斷方式如下：</span><span class="sxs-lookup"><span data-stu-id="b58e2-507">The fully qualified name of a namespace or type `N` is determined as follows:</span></span>

*  <span data-ttu-id="b58e2-508">如果`N`成員的完整限定的名稱是全域命名空間中， `N`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-508">If `N` is a member of the global namespace, its fully qualified name is `N`.</span></span>
*  <span data-ttu-id="b58e2-509">否則，其完整的名稱是`S.N`，其中`S`命名空間或型別所在的完整格式名稱`N`宣告。</span><span class="sxs-lookup"><span data-stu-id="b58e2-509">Otherwise, its fully qualified name is `S.N`, where `S` is the fully qualified name of the namespace or type in which `N` is declared.</span></span>

<span data-ttu-id="b58e2-510">換句話說，完整的名稱的`N`就是完整的階層式路徑的識別項會導致`N`，並且從全域命名空間。</span><span class="sxs-lookup"><span data-stu-id="b58e2-510">In other words, the fully qualified name of `N` is the complete hierarchical path of identifiers that lead to `N`, starting from the global namespace.</span></span> <span data-ttu-id="b58e2-511">由於每個命名空間或類型成員必須有唯一的名稱，它會依照命名空間或型別的完整格式的名稱永遠是唯一。</span><span class="sxs-lookup"><span data-stu-id="b58e2-511">Because every member of a namespace or type must have a unique name, it follows that the fully qualified name of a namespace or type is always unique.</span></span>

<span data-ttu-id="b58e2-512">下列範例將說明幾種命名空間和類型，以及其相關聯的完整名稱。</span><span class="sxs-lookup"><span data-stu-id="b58e2-512">The example below shows several namespace and type declarations along with their associated fully qualified names.</span></span>
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

## <a name="automatic-memory-management"></a><span data-ttu-id="b58e2-513">自動記憶體管理</span><span class="sxs-lookup"><span data-stu-id="b58e2-513">Automatic memory management</span></span>

<span data-ttu-id="b58e2-514">C# 採用自動記憶體管理，讓開發人員從手動配置及釋放物件所佔用的記憶體。</span><span class="sxs-lookup"><span data-stu-id="b58e2-514">C# employs automatic memory management, which frees developers from manually allocating and freeing the memory occupied by objects.</span></span> <span data-ttu-id="b58e2-515">自動記憶體管理原則由實作***記憶體回收行程***。</span><span class="sxs-lookup"><span data-stu-id="b58e2-515">Automatic memory management policies are implemented by a ***garbage collector***.</span></span> <span data-ttu-id="b58e2-516">物件的記憶體管理生命週期如下所示：</span><span class="sxs-lookup"><span data-stu-id="b58e2-516">The memory management life cycle of an object is as follows:</span></span>

1. <span data-ttu-id="b58e2-517">建立物件時，會為它配置記憶體、 建構函式執行時，和該物件會被視為即時。</span><span class="sxs-lookup"><span data-stu-id="b58e2-517">When the object is created, memory is allocated for it, the constructor is run, and the object is considered live.</span></span>
2. <span data-ttu-id="b58e2-518">如果物件或一部分，無法透過任何可能執行的存取，以外執行解構函式時，物件會被視為不再使用中，並成為適合進行解構。</span><span class="sxs-lookup"><span data-stu-id="b58e2-518">If the object, or any part of it, cannot be accessed by any possible continuation of execution, other than the running of destructors, the object is considered no longer in use, and it becomes eligible for destruction.</span></span> <span data-ttu-id="b58e2-519">C# 編譯器和記憶體回收行程可能會選擇分析程式碼，以判斷哪一個物件的參考可能會在未來使用。</span><span class="sxs-lookup"><span data-stu-id="b58e2-519">The C# compiler and the garbage collector may choose to analyze code to determine which references to an object may be used in the future.</span></span> <span data-ttu-id="b58e2-520">比方說，如果在範圍內的區域變數是唯一的現有參考的物件，但永遠不會參考該區域變數中從目前的執行執行的任何可能的接續點程序中，記憶體回收行程可能 （但不是才能） 為不再使用中處理物件。</span><span class="sxs-lookup"><span data-stu-id="b58e2-520">For instance, if a local variable that is in scope is the only existing reference to an object, but that local variable is never referred to in any possible continuation of execution from the current execution point in the procedure, the garbage collector may (but is not required to) treat the object as no longer in use.</span></span>
3. <span data-ttu-id="b58e2-521">適合解構物件之後，在稍後時間解構函式 ([解構函式](classes.md#destructors)) （如果有的話） 的執行物件。</span><span class="sxs-lookup"><span data-stu-id="b58e2-521">Once the object is eligible for destruction, at some unspecified later time the destructor ([Destructors](classes.md#destructors)) (if any) for the object is run.</span></span> <span data-ttu-id="b58e2-522">在正常情況下，雖然實作特定的 Api 可能會允許覆寫此行為，是一次執行物件的解構函式。</span><span class="sxs-lookup"><span data-stu-id="b58e2-522">Under normal circumstances the destructor for the object is run once only, though implementation-specific APIs may allow this behavior to be overridden.</span></span>
4. <span data-ttu-id="b58e2-523">當執行物件的解構函式時，如果透過任何可能的執行，包括解構函式的執行，則無法存取該物件或一部分，該物件會被視為無法存取，而且該物件會變成可進行記憶體回收。</span><span class="sxs-lookup"><span data-stu-id="b58e2-523">Once the destructor for an object is run, if that object, or any part of it, cannot be accessed by any possible continuation of execution, including the running of destructors, the object is considered inaccessible and the object becomes eligible for collection.</span></span>
5. <span data-ttu-id="b58e2-524">最後，某個時間點之後的物件會變成可進行記憶體回收，記憶體回收行程釋放的記憶體與該物件相關聯。</span><span class="sxs-lookup"><span data-stu-id="b58e2-524">Finally, at some time after the object becomes eligible for collection, the garbage collector frees the memory associated with that object.</span></span>

<span data-ttu-id="b58e2-525">記憶體回收行程會維護物件使用量的相關資訊，並使用這項資訊以提供記憶體管理決策，例如在重新定位的物件，以及當物件已不再使用中或無法存取，請找出新建立的物件，記憶體中的位置。</span><span class="sxs-lookup"><span data-stu-id="b58e2-525">The garbage collector maintains information about object usage, and uses this information to make memory management decisions, such as where in memory to locate a newly created object, when to relocate an object, and when an object is no longer in use or inaccessible.</span></span>

<span data-ttu-id="b58e2-526">假設有記憶體回收行程的其他語言，例如 C# 的設計是要讓記憶體回收行程可能會實作各種不同的記憶體管理原則。</span><span class="sxs-lookup"><span data-stu-id="b58e2-526">Like other languages that assume the existence of a garbage collector, C# is designed so that the garbage collector may implement a wide range of memory management policies.</span></span> <span data-ttu-id="b58e2-527">比方說，C# 不需要執行解構函式或物件會收集到的只要他們有資格，或任何特定的順序，或在任何特定的執行緒上，執行解構函式。</span><span class="sxs-lookup"><span data-stu-id="b58e2-527">For instance, C# does not require that destructors be run or that objects be collected as soon as they are eligible, or that destructors be run in any particular order, or on any particular thread.</span></span>

<span data-ttu-id="b58e2-528">記憶體回收行程控制行為，以某種程度上，透過在類別上的靜態方法`System.GC`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-528">The behavior of the garbage collector can be controlled, to some degree, via static methods on the class `System.GC`.</span></span> <span data-ttu-id="b58e2-529">這個類別可用來要求回收，解構函式將會執行 （或未執行） 等等。</span><span class="sxs-lookup"><span data-stu-id="b58e2-529">This class can be used to request a collection to occur, destructors to be run (or not run), and so forth.</span></span>

<span data-ttu-id="b58e2-530">因為記憶體回收行程相當大的自由決定何時回收物件，並執行解構函式中的，合格的實作可能會產生不同於下列程式碼所示的輸出。</span><span class="sxs-lookup"><span data-stu-id="b58e2-530">Since the garbage collector is allowed wide latitude in deciding when to collect objects and run destructors, a conforming implementation may produce output that differs from that shown by the following code.</span></span> <span data-ttu-id="b58e2-531">程式</span><span class="sxs-lookup"><span data-stu-id="b58e2-531">The program</span></span>
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
<span data-ttu-id="b58e2-532">建立類別的執行個體`A`類別的執行個體和`B`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-532">creates an instance of class `A` and an instance of class `B`.</span></span> <span data-ttu-id="b58e2-533">這些物件有資格使用記憶體回收時變數`b`會將值指派給`null`，因為在此時間之後就無法存取它們的任何使用者撰寫程式碼。</span><span class="sxs-lookup"><span data-stu-id="b58e2-533">These objects become eligible for garbage collection when the variable `b` is assigned the value `null`, since after this time it is impossible for any user-written code to access them.</span></span> <span data-ttu-id="b58e2-534">輸出可以是</span><span class="sxs-lookup"><span data-stu-id="b58e2-534">The output could be either</span></span>
```
Destruct instance of A
Destruct instance of B
```
<span data-ttu-id="b58e2-535">或</span><span class="sxs-lookup"><span data-stu-id="b58e2-535">or</span></span>
```
Destruct instance of B
Destruct instance of A
```
<span data-ttu-id="b58e2-536">因為語言會施加在其中物件被記憶體回收的順序沒有限制。</span><span class="sxs-lookup"><span data-stu-id="b58e2-536">because the language imposes no constraints on the order in which objects are garbage collected.</span></span>

<span data-ttu-id="b58e2-537">在難以察覺的情況下，「 適合進行解構"和"可進行記憶體回收 」 之間的差別可能很重要。</span><span class="sxs-lookup"><span data-stu-id="b58e2-537">In subtle cases, the distinction between "eligible for destruction" and "eligible for collection" can be important.</span></span> <span data-ttu-id="b58e2-538">例如，套用至物件的</span><span class="sxs-lookup"><span data-stu-id="b58e2-538">For example,</span></span>
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

<span data-ttu-id="b58e2-539">在上述程式中，如果選擇要執行的解構函式的記憶體回收行程`A`的解構函式之前`B`，則可能是此程式的輸出：</span><span class="sxs-lookup"><span data-stu-id="b58e2-539">In the above program, if the garbage collector chooses to run the destructor of `A` before the destructor of `B`, then the output of this program might be:</span></span>
```
Destruct instance of A
Destruct instance of B
A.F
RefA is not null
```

<span data-ttu-id="b58e2-540">請注意，雖然執行個體`A`無法在使用中和`A`的執行解構函式、 方法仍可能`A`(在此情況下， `F`) 從另一個解構函式呼叫。</span><span class="sxs-lookup"><span data-stu-id="b58e2-540">Note that although the instance of `A` was not in use and `A`'s destructor was run, it is still possible for methods of `A` (in this case, `F`) to be called from another destructor.</span></span> <span data-ttu-id="b58e2-541">另請注意執行解構函式可能會導致要再次變成主線程式可用的物件。</span><span class="sxs-lookup"><span data-stu-id="b58e2-541">Also, note that running of a destructor may cause an object to become usable from the mainline program again.</span></span> <span data-ttu-id="b58e2-542">在此情況下，執行`B`的解構函式造成的執行個體`A`，先前在非使用可從即時參考`Test.RefA`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-542">In this case, the running of `B`'s destructor caused an instance of `A` that was previously not in use to become accessible from the live reference `Test.RefA`.</span></span> <span data-ttu-id="b58e2-543">在呼叫之後`WaitForPendingFinalizers`，執行個體`B`符合資格的集合，但執行個體`A`不是，因為參考`Test.RefA`。</span><span class="sxs-lookup"><span data-stu-id="b58e2-543">After the call to `WaitForPendingFinalizers`, the instance of `B` is eligible for collection, but the instance of `A` is not, because of the reference `Test.RefA`.</span></span>

<span data-ttu-id="b58e2-544">為了避免混淆和非預期的行為，它通常是個不錯的主意的解構函式只儲存在其物件自己的欄位中，資料上執行清除作業，而非參考的物件或靜態欄位上執行任何動作。</span><span class="sxs-lookup"><span data-stu-id="b58e2-544">To avoid confusion and unexpected behavior, it is generally a good idea for destructors to only perform cleanup on data stored in their object's own fields, and not to perform any actions on referenced objects or static fields.</span></span>

<span data-ttu-id="b58e2-545">使用解構函式的替代方式是讓實作類別`System.IDisposable`介面。</span><span class="sxs-lookup"><span data-stu-id="b58e2-545">An alternative to using destructors is to let a class implement the `System.IDisposable` interface.</span></span> <span data-ttu-id="b58e2-546">這可讓用戶端的物件，判斷何時要釋放物件的資源通常為中的資源存取物件`using`陳述式 ([using 陳述式](statements.md#the-using-statement))。</span><span class="sxs-lookup"><span data-stu-id="b58e2-546">This allows the client of the object to determine when to release the resources of the object, typically by accessing the object as a resource in a `using` statement ([The using statement](statements.md#the-using-statement)).</span></span>

## <a name="execution-order"></a><span data-ttu-id="b58e2-547">執行順序</span><span class="sxs-lookup"><span data-stu-id="b58e2-547">Execution order</span></span>

<span data-ttu-id="b58e2-548">C# 程式的執行程序，每個執行中執行緒的副作用會保留在重要的執行時間點。</span><span class="sxs-lookup"><span data-stu-id="b58e2-548">Execution of a C# program proceeds such that the side effects of each executing thread are preserved at critical execution points.</span></span> <span data-ttu-id="b58e2-549">A***副作用***定義為讀取或寫入 volatile 欄位，寫入靜態變數時，寫入外部資源，並擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b58e2-549">A ***side effect*** is defined as a read or write of a volatile field, a write to a non-volatile variable, a write to an external resource, and the throwing of an exception.</span></span> <span data-ttu-id="b58e2-550">這些副作用的順序必須保留在其中的關鍵執行點是變動性的欄位參考 ([Volatile 欄位](classes.md#volatile-fields))，`lock`陳述式 ([lock 陳述式](statements.md#the-lock-statement))，以及執行緒建立與終止。</span><span class="sxs-lookup"><span data-stu-id="b58e2-550">The critical execution points at which the order of these side effects must be preserved are references to volatile fields ([Volatile fields](classes.md#volatile-fields)), `lock` statements ([The lock statement](statements.md#the-lock-statement)), and thread creation and termination.</span></span> <span data-ttu-id="b58e2-551">執行環境是可用來變更執行的 C# 程式，受限於下列條件約束的順序：</span><span class="sxs-lookup"><span data-stu-id="b58e2-551">The execution environment is free to change the order of execution of a C# program, subject to the following constraints:</span></span>

*  <span data-ttu-id="b58e2-552">資料相依性會保留在執行的執行緒。</span><span class="sxs-lookup"><span data-stu-id="b58e2-552">Data dependence is preserved within a thread of execution.</span></span> <span data-ttu-id="b58e2-553">亦即，如同原始依照程式順序所執行的執行緒中的所有陳述式，會計算每個變數的值。</span><span class="sxs-lookup"><span data-stu-id="b58e2-553">That is, the value of each variable is computed as if all statements in the thread were executed in original program order.</span></span>
*  <span data-ttu-id="b58e2-554">初始設定順序保留規則 ([欄位初始化](classes.md#field-initialization)並[區域變數初始設定式](classes.md#variable-initializers))。</span><span class="sxs-lookup"><span data-stu-id="b58e2-554">Initialization ordering rules are preserved ([Field initialization](classes.md#field-initialization) and [Variable initializers](classes.md#variable-initializers)).</span></span>
*  <span data-ttu-id="b58e2-555">變動性的讀取和寫入方面保留的副作用的順序 ([Volatile 欄位](classes.md#volatile-fields))。</span><span class="sxs-lookup"><span data-stu-id="b58e2-555">The ordering of side effects is preserved with respect to volatile reads and writes ([Volatile fields](classes.md#volatile-fields)).</span></span> <span data-ttu-id="b58e2-556">此外，如果它可以推斷不會使用該運算式的值和任何必要的副作用所產生 （包括任何因呼叫方法或存取 volatile 欄位） 的執行環境需要無法評估運算式的一部分。</span><span class="sxs-lookup"><span data-stu-id="b58e2-556">Additionally, the execution environment need not evaluate part of an expression if it can deduce that that expression's value is not used and that no needed side effects are produced (including any caused by calling a method or accessing a volatile field).</span></span> <span data-ttu-id="b58e2-557">當程式執行中斷的非同步事件 （例如另一個執行緒所擲回例外狀況） 時，不保證可預見的副作用會顯示在原始程式的順序。</span><span class="sxs-lookup"><span data-stu-id="b58e2-557">When program execution is interrupted by an asynchronous event (such as an exception thrown by another thread), it is not guaranteed that the observable side effects are visible in the original program order.</span></span>
