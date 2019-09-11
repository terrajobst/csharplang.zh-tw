---
ms.openlocfilehash: 8bc4bf6310fb8a8457beee167f18d30aaca10a8e
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876887"
---
# <a name="introduction"></a><span data-ttu-id="bbaec-101">簡介</span><span class="sxs-lookup"><span data-stu-id="bbaec-101">Introduction</span></span>

<span data-ttu-id="bbaec-102">C# (發音為 "See Sharp") 是簡單、物件導向、型別安全的現代化程式設計語言。</span><span class="sxs-lookup"><span data-stu-id="bbaec-102">C# (pronounced "See Sharp") is a simple, modern, object-oriented, and type-safe programming language.</span></span> <span data-ttu-id="bbaec-103">C#具有 C 語言系列中的根目錄，而且 C、 C++和 JAVA 程式設計人員會立即熟悉。</span><span class="sxs-lookup"><span data-stu-id="bbaec-103">C# has its roots in the C family of languages and will be immediately familiar to C, C++, and Java programmers.</span></span> <span data-ttu-id="bbaec-104">C#是由 ECMA 國際標準化為***ecma-334***標準，並以 ISO/Iec 作為***iso/iec 23270***標準。</span><span class="sxs-lookup"><span data-stu-id="bbaec-104">C# is standardized by ECMA International as the ***ECMA-334*** standard and by ISO/IEC as the ***ISO/IEC 23270*** standard.</span></span> <span data-ttu-id="bbaec-105">適用于C# .NET Framework 的 Microsoft 編譯器是這兩項標準的一致實現。</span><span class="sxs-lookup"><span data-stu-id="bbaec-105">Microsoft's C# compiler for the .NET Framework is a conforming implementation of both of these standards.</span></span>

<span data-ttu-id="bbaec-106">C# 是物件導向的語言，但 C# 更進一步支援「元件導向」程式設計。</span><span class="sxs-lookup"><span data-stu-id="bbaec-106">C# is an object-oriented language, but C# further includes support for ***component-oriented*** programming.</span></span> <span data-ttu-id="bbaec-107">現代軟體設計逐漸依賴功能性上獨立與屬於自我描述套件的軟體元件。</span><span class="sxs-lookup"><span data-stu-id="bbaec-107">Contemporary software design increasingly relies on software components in the form of self-contained and self-describing packages of functionality.</span></span> <span data-ttu-id="bbaec-108">這類元件的關鍵在於它們呈現含有屬性、方法和事件的程式設計模型；它們提供元件相關宣告資訊的屬性，且併入自己的文件。</span><span class="sxs-lookup"><span data-stu-id="bbaec-108">Key to such components is that they present a programming model with properties, methods, and events; they have attributes that provide declarative information about the component; and they incorporate their own documentation.</span></span> <span data-ttu-id="bbaec-109">C#提供語言結構來直接支援這些概念， C#並採用非常自然的語言來建立和使用軟體元件。</span><span class="sxs-lookup"><span data-stu-id="bbaec-109">C# provides language constructs to directly support these concepts, making C# a very natural language in which to create and use software components.</span></span>

<span data-ttu-id="bbaec-110">以下幾個 C# 功能可協助建構強固且耐用的應用程式：***垃圾收集***會自動回收未使用的物件所佔用的記憶體;***例外狀況處理***提供了一種結構化且可擴充的方法來偵測及復原錯誤;而語言的***型別安全***設計則無法從未初始化的變數讀取、將陣列索引超出其界限，或執行未檢查的型別轉換。</span><span class="sxs-lookup"><span data-stu-id="bbaec-110">Several C# features aid in the construction of robust and durable applications: ***Garbage collection*** automatically reclaims memory occupied by unused objects; ***exception handling*** provides a structured and extensible approach to error detection and recovery; and the ***type-safe*** design of the language makes it impossible to read from uninitialized variables, to index arrays beyond their bounds, or to perform unchecked type casts.</span></span>

<span data-ttu-id="bbaec-111">C# 有***統一的型別系統***。</span><span class="sxs-lookup"><span data-stu-id="bbaec-111">C# has a ***unified type system***.</span></span> <span data-ttu-id="bbaec-112">所有的 C# 型別 (包括 `int` 和 `double` 等基本型別) 都繼承自單一的 `object` 根型別。</span><span class="sxs-lookup"><span data-stu-id="bbaec-112">All C# types, including primitive types such as `int` and `double`, inherit from a single root `object` type.</span></span> <span data-ttu-id="bbaec-113">因此，所有型別都擁有相同的一組常見作業，且任何型別的值都能以相同方式儲存、傳送及操作。</span><span class="sxs-lookup"><span data-stu-id="bbaec-113">Thus, all types share a set of common operations, and values of any type can be stored, transported, and operated upon in a consistent manner.</span></span> <span data-ttu-id="bbaec-114">此外，C# 支援使用者定義的參考型別和數值型別，因此能動態配置物件，以及內嵌儲存輕量結構。</span><span class="sxs-lookup"><span data-stu-id="bbaec-114">Furthermore, C# supports both user-defined reference types and value types, allowing dynamic allocation of objects as well as in-line storage of lightweight structures.</span></span>

<span data-ttu-id="bbaec-115">為了確保C#程式和程式庫能夠以相容的方式在一段時間內演變， C#我們將著重于設計的***版本***設定。</span><span class="sxs-lookup"><span data-stu-id="bbaec-115">To ensure that C# programs and libraries can evolve over time in a compatible manner, much emphasis has been placed on ***versioning*** in C#'s design.</span></span> <span data-ttu-id="bbaec-116">許多程式設計語言並不注重此問題，因此當推出新版的相依程式庫時，以那些語言撰寫的程式需要進行較多修改才能繼續運作。</span><span class="sxs-lookup"><span data-stu-id="bbaec-116">Many programming languages pay little attention to this issue, and, as a result, programs written in those languages break more often than necessary when newer versions of dependent libraries are introduced.</span></span> <span data-ttu-id="bbaec-117">C#設計的層面會受到版本控制的直接影響，包括個別`virtual`的和`override`修飾詞、方法多載解析的規則，以及對明確介面成員宣告的支援。</span><span class="sxs-lookup"><span data-stu-id="bbaec-117">Aspects of C#'s design that were directly influenced by versioning considerations include the separate `virtual` and `override` modifiers, the rules for method overload resolution, and support for explicit interface member declarations.</span></span>

<span data-ttu-id="bbaec-118">本章的其餘部分將說明此C#語言的基本功能。</span><span class="sxs-lookup"><span data-stu-id="bbaec-118">The rest of this chapter describes the essential features of the C# language.</span></span> <span data-ttu-id="bbaec-119">雖然稍後的章節會以詳細和有時是數學的方式來描述規則和例外狀況，但這一章會以完整性和最簡單的代價來進行。</span><span class="sxs-lookup"><span data-stu-id="bbaec-119">Although later chapters describe rules and exceptions in a detail-oriented and sometimes mathematical manner, this chapter strives for clarity and brevity at the expense of completeness.</span></span> <span data-ttu-id="bbaec-120">其目的是要為讀者提供可協助撰寫早期程式的語言簡介，以及閱讀後面的章節。</span><span class="sxs-lookup"><span data-stu-id="bbaec-120">The intent is to provide the reader with an introduction to the language that will facilitate the writing of early programs and the reading of later chapters.</span></span>

## <a name="hello-world"></a><span data-ttu-id="bbaec-121">Hello World</span><span class="sxs-lookup"><span data-stu-id="bbaec-121">Hello world</span></span>

<span data-ttu-id="bbaec-122">“Hello, World” 程式通常用來介紹程式設計語言。</span><span class="sxs-lookup"><span data-stu-id="bbaec-122">The "Hello, World" program is traditionally used to introduce a programming language.</span></span> <span data-ttu-id="bbaec-123">以下是以 C# 撰寫的：</span><span class="sxs-lookup"><span data-stu-id="bbaec-123">Here it is in C#:</span></span>

```csharp
using System;

class Hello
{
    static void Main() {
        Console.WriteLine("Hello, World");
    }
}
```

<span data-ttu-id="bbaec-124">C# 原始程式檔的副檔名通常是 `.cs`。</span><span class="sxs-lookup"><span data-stu-id="bbaec-124">C# source files typically have the file extension `.cs`.</span></span> <span data-ttu-id="bbaec-125">假設 "Hello，World" 程式儲存在檔案中`hello.cs`，則可以使用命令列以 Microsoft C#編譯器來編譯器</span><span class="sxs-lookup"><span data-stu-id="bbaec-125">Assuming that the "Hello, World" program is stored in the file `hello.cs`, the program can be compiled with the Microsoft C# compiler using the command line</span></span>
```
csc hello.cs
```
<span data-ttu-id="bbaec-126">這會產生名為`hello.exe`的可執行元件。</span><span class="sxs-lookup"><span data-stu-id="bbaec-126">which produces an executable assembly named `hello.exe`.</span></span> <span data-ttu-id="bbaec-127">此應用程式執行時所產生的輸出為</span><span class="sxs-lookup"><span data-stu-id="bbaec-127">The output produced by this application when it is run is</span></span>
```
Hello, World
```

<span data-ttu-id="bbaec-128">“Hello, World” 程式的開頭為 `using` 指示詞，會參考 `System` 命名空間。</span><span class="sxs-lookup"><span data-stu-id="bbaec-128">The "Hello, World" program starts with a `using` directive that references the `System` namespace.</span></span> <span data-ttu-id="bbaec-129">命名空間提供組織 C# 程式和程式庫的階層式方法。</span><span class="sxs-lookup"><span data-stu-id="bbaec-129">Namespaces provide a hierarchical means of organizing C# programs and libraries.</span></span> <span data-ttu-id="bbaec-130">命名空間包含型別和其他命名空間，例如 `System` 命名空間包含數個型別 (如程式中參考的 `Console` 類別)，和數個其他命名空間 (如 `IO` 和 `Collections`)。</span><span class="sxs-lookup"><span data-stu-id="bbaec-130">Namespaces contain types and other namespaces—for example, the `System` namespace contains a number of types, such as the `Console` class referenced in the program, and a number of other namespaces, such as `IO` and `Collections`.</span></span> <span data-ttu-id="bbaec-131">使用 `using` 指示詞參考指定的命名空間，就能以非限定的方式使用屬於該命名空間成員的型別。</span><span class="sxs-lookup"><span data-stu-id="bbaec-131">A `using` directive that references a given namespace enables unqualified use of the types that are members of that namespace.</span></span> <span data-ttu-id="bbaec-132">因為 `using` 指示詞的緣故，該程式可以使用 `Console.WriteLine` 當作 `System.Console.WriteLine` 的縮寫。</span><span class="sxs-lookup"><span data-stu-id="bbaec-132">Because of the `using` directive, the program can use `Console.WriteLine` as shorthand for `System.Console.WriteLine`.</span></span>

<span data-ttu-id="bbaec-133">“Hello, World” 程式宣告的 `Hello` 類別包含單一成員，即名為 `Main` 的方法。</span><span class="sxs-lookup"><span data-stu-id="bbaec-133">The `Hello` class declared by the "Hello, World" program has a single member, the method named `Main`.</span></span> <span data-ttu-id="bbaec-134">`Main`方法是`static`使用修飾詞來宣告。</span><span class="sxs-lookup"><span data-stu-id="bbaec-134">The `Main` method is declared with the `static` modifier.</span></span> <span data-ttu-id="bbaec-135">執行個體方法可以使用關鍵字 `this` 參考特定的封入物件執行個體，但靜態方法卻不需要參考特定物件即可運作。</span><span class="sxs-lookup"><span data-stu-id="bbaec-135">While instance methods can reference a particular enclosing object instance using the keyword `this`, static methods operate without reference to a particular object.</span></span> <span data-ttu-id="bbaec-136">依照慣例，會使用名為 `Main` 的靜態方法做為程式的進入點。</span><span class="sxs-lookup"><span data-stu-id="bbaec-136">By convention, a static method named `Main` serves as the entry point of a program.</span></span>

<span data-ttu-id="bbaec-137">程式的輸出是由 `System` 命名空間中 `Console` 類別的 `WriteLine` 方法產生。</span><span class="sxs-lookup"><span data-stu-id="bbaec-137">The output of the program is produced by the `WriteLine` method of the `Console` class in the `System` namespace.</span></span> <span data-ttu-id="bbaec-138">這個類別是由 .NET Framework 的類別庫所提供，預設會由 Microsoft C#編譯器自動參考。</span><span class="sxs-lookup"><span data-stu-id="bbaec-138">This class is provided by the .NET Framework class libraries, which, by default, are automatically referenced by the Microsoft C# compiler.</span></span> <span data-ttu-id="bbaec-139">請注意C# ，本身並沒有個別的執行時間程式庫。</span><span class="sxs-lookup"><span data-stu-id="bbaec-139">Note that C# itself does not have a separate runtime library.</span></span> <span data-ttu-id="bbaec-140">相反地，.NET Framework 是的執行時間連結C#庫。</span><span class="sxs-lookup"><span data-stu-id="bbaec-140">Instead, the .NET Framework is the runtime library of C#.</span></span>

## <a name="program-structure"></a><span data-ttu-id="bbaec-141">程式結構</span><span class="sxs-lookup"><span data-stu-id="bbaec-141">Program structure</span></span>

<span data-ttu-id="bbaec-142">C# 語言的重要組織概念如下：程式、命名空間、型別、成員及組件。</span><span class="sxs-lookup"><span data-stu-id="bbaec-142">The key organizational concepts in C# are ***programs***, ***namespaces***, ***types***, ***members***, and ***assemblies***.</span></span> <span data-ttu-id="bbaec-143">C# 程式包含一個或多個原始程式檔。</span><span class="sxs-lookup"><span data-stu-id="bbaec-143">C# programs consist of one or more source files.</span></span> <span data-ttu-id="bbaec-144">程式宣告型別，其中包含成員並可以依據命名空間分組。</span><span class="sxs-lookup"><span data-stu-id="bbaec-144">Programs declare types, which contain members and can be organized into namespaces.</span></span> <span data-ttu-id="bbaec-145">類別和介面都是型別的範例。</span><span class="sxs-lookup"><span data-stu-id="bbaec-145">Classes and interfaces are examples of types.</span></span> <span data-ttu-id="bbaec-146">欄位、方法、屬性及事件都是成員的範例。</span><span class="sxs-lookup"><span data-stu-id="bbaec-146">Fields, methods, properties, and events are examples of members.</span></span> <span data-ttu-id="bbaec-147">在編譯 C# 語言時，會將它們實際封裝為組件。</span><span class="sxs-lookup"><span data-stu-id="bbaec-147">When C# programs are compiled, they are physically packaged into assemblies.</span></span> <span data-ttu-id="bbaec-148">元件通常會有`.exe`副檔名或`.dll`，視其是否執行***應用程式***或連結***庫***而定。</span><span class="sxs-lookup"><span data-stu-id="bbaec-148">Assemblies typically have the file extension `.exe` or `.dll`, depending on whether they implement ***applications*** or ***libraries***.</span></span>

<span data-ttu-id="bbaec-149">範例</span><span class="sxs-lookup"><span data-stu-id="bbaec-149">The example</span></span>

```csharp
using System;

namespace Acme.Collections
{
    public class Stack
    {
        Entry top;

        public void Push(object data) {
            top = new Entry(top, data);
        }

        public object Pop() {
            if (top == null) throw new InvalidOperationException();
            object result = top.data;
            top = top.next;
            return result;
        }

        class Entry
        {
            public Entry next;
            public object data;
    
            public Entry(Entry next, object data) {
                this.next = next;
                this.data = data;
            }
        }
    }
}
```
<span data-ttu-id="bbaec-150">在名`Stack` `Acme.Collections`為的命名空間中宣告名為的類別。</span><span class="sxs-lookup"><span data-stu-id="bbaec-150">declares a class named `Stack` in a namespace called `Acme.Collections`.</span></span> <span data-ttu-id="bbaec-151">此類別的完整名稱是 `Acme.Collections.Stack`。</span><span class="sxs-lookup"><span data-stu-id="bbaec-151">The fully qualified name of this class is `Acme.Collections.Stack`.</span></span> <span data-ttu-id="bbaec-152">該類別包含數個成員︰一個名為 `top` 的欄位、兩個名為 `Push` 和 `Pop` 的方法以及名為 `Entry` 的巢狀類別。</span><span class="sxs-lookup"><span data-stu-id="bbaec-152">The class contains several members: a field named `top`, two methods named `Push` and `Pop`, and a nested class named `Entry`.</span></span> <span data-ttu-id="bbaec-153">`Entry` 類別更包含三個成員︰一個名為 `next` 的欄位、一個名為 `data` 的欄位以及建構函式。</span><span class="sxs-lookup"><span data-stu-id="bbaec-153">The `Entry` class further contains three members: a field named `next`, a field named `data`, and a constructor.</span></span> <span data-ttu-id="bbaec-154">假設此範例的原始程式碼儲存在檔案 `acme.cs`，命令列</span><span class="sxs-lookup"><span data-stu-id="bbaec-154">Assuming that the source code of the example is stored in the file `acme.cs`, the command line</span></span>

```
csc /t:library acme.cs
```
<span data-ttu-id="bbaec-155">會將範例編譯為程式庫 (不需要 `Main` 進入點的程式碼)，並產生名為 `acme.dll` 的組件。</span><span class="sxs-lookup"><span data-stu-id="bbaec-155">compiles the example as a library (code without a `Main` entry point) and produces an assembly named `acme.dll`.</span></span>

<span data-ttu-id="bbaec-156">元件包含***中繼語言***（IL）指令形式的可執行程式碼，以及***中繼資料***形式的符號資訊。</span><span class="sxs-lookup"><span data-stu-id="bbaec-156">Assemblies contain executable code in the form of ***Intermediate Language*** (IL) instructions, and symbolic information in the form of ***metadata***.</span></span> <span data-ttu-id="bbaec-157">執行前，組件中的 IL 程式碼會由 .NET 通用語言執行平台的 Just-In-Time (JIT) 編譯器自動轉換為處理器特定程式碼。</span><span class="sxs-lookup"><span data-stu-id="bbaec-157">Before it is executed, the IL code in an assembly is automatically converted to processor-specific code by the Just-In-Time (JIT) compiler of .NET Common Language Runtime.</span></span>

<span data-ttu-id="bbaec-158">由於組件是包含程式碼和中繼資料的功能自我描述單位，所以不需要提供 C# 中的 `#include` 指示詞和標頭檔。</span><span class="sxs-lookup"><span data-stu-id="bbaec-158">Because an assembly is a self-describing unit of functionality containing both code and metadata, there is no need for `#include` directives and header files in C#.</span></span> <span data-ttu-id="bbaec-159">特定組件中所包含的公用型別和成員只能在編譯程式時，使用 C# 程式來參考該組件。</span><span class="sxs-lookup"><span data-stu-id="bbaec-159">The public types and members contained in a particular assembly are made available in a C# program simply by referencing that assembly when compiling the program.</span></span> <span data-ttu-id="bbaec-160">例如，此程式使用來自 `acme.dll` 組件的 `Acme.Collections.Stack` 類別︰</span><span class="sxs-lookup"><span data-stu-id="bbaec-160">For example, this program uses the `Acme.Collections.Stack` class from the `acme.dll` assembly:</span></span>

```csharp
using System;
using Acme.Collections;

class Test
{
    static void Main() {
        Stack s = new Stack();
        s.Push(1);
        s.Push(10);
        s.Push(100);
        Console.WriteLine(s.Pop());
        Console.WriteLine(s.Pop());
        Console.WriteLine(s.Pop());
    }
}
```
<span data-ttu-id="bbaec-161">如果程式儲存`test.cs`在檔案中，則在編譯時`test.cs` ， `acme.dll`可以使用編譯器的`/r`選項來參考元件：</span><span class="sxs-lookup"><span data-stu-id="bbaec-161">If the program is stored in the file `test.cs`, when `test.cs` is compiled, the `acme.dll` assembly can be referenced using the compiler's `/r` option:</span></span>

```
csc /r:acme.dll test.cs
```
<span data-ttu-id="bbaec-162">這樣會建立名為 `test.exe` 可執行檔組件，執行時會產生以下輸出︰</span><span class="sxs-lookup"><span data-stu-id="bbaec-162">This creates an executable assembly named `test.exe`, which, when run, produces the output:</span></span>

```
100
10
1
```
<span data-ttu-id="bbaec-163">C# 允許以數個原始程式檔儲存程式的原始程式文字。</span><span class="sxs-lookup"><span data-stu-id="bbaec-163">C# permits the source text of a program to be stored in several source files.</span></span> <span data-ttu-id="bbaec-164">編譯有多檔案的 C# 程式時，所有原始程式檔都會一起處理，而原始程式檔可以隨意地互相參考。概念上，就如同將所有原始程式檔都串連成一個大型檔案，然後再進行處理。</span><span class="sxs-lookup"><span data-stu-id="bbaec-164">When a multi-file C# program is compiled, all of the source files are processed together, and the source files can freely reference each other—conceptually, it is as if all the source files were concatenated into one large file before being processed.</span></span> <span data-ttu-id="bbaec-165">C# 中不再需要向前宣告，原因是在極少數的例外狀況中，宣告順序並不重要。</span><span class="sxs-lookup"><span data-stu-id="bbaec-165">Forward declarations are never needed in C# because, with very few exceptions, declaration order is insignificant.</span></span> <span data-ttu-id="bbaec-166">C# 不會限制原始程式檔只能宣告一個公用型別，也不會要求原始程式檔符合原始程式檔中所宣告的型別名稱。</span><span class="sxs-lookup"><span data-stu-id="bbaec-166">C# does not limit a source file to declaring only one public type nor does it require the name of the source file to match a type declared in the source file.</span></span>

## <a name="types-and-variables"></a><span data-ttu-id="bbaec-167">型別與變數</span><span class="sxs-lookup"><span data-stu-id="bbaec-167">Types and variables</span></span>

<span data-ttu-id="bbaec-168">C# 中有兩種型別：***實值型別***和***參考型別***。</span><span class="sxs-lookup"><span data-stu-id="bbaec-168">There are two kinds of types in C#: ***value types*** and ***reference types***.</span></span> <span data-ttu-id="bbaec-169">實值型別的變數直接包含其資料，而參考型別的變數則將參考儲存到其資料，後者即是物件。</span><span class="sxs-lookup"><span data-stu-id="bbaec-169">Variables of value types directly contain their data whereas variables of reference types store references to their data, the latter being known as objects.</span></span> <span data-ttu-id="bbaec-170">使用參考型別時，這兩種變數可以參考相同的物件，因此對其中一個變數進行的作業可能會影響另一個變數所參考的物件。</span><span class="sxs-lookup"><span data-stu-id="bbaec-170">With reference types, it is possible for two variables to reference the same object and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="bbaec-171">使用實值型別時，變數各有自己的資料複本，因此在某一個變數上進行的作業不可能會影響其他變數 (但 `ref` 和 `out` 參數變數除外)。</span><span class="sxs-lookup"><span data-stu-id="bbaec-171">With value types, the variables each have their own copy of the data, and it is not possible for operations on one to affect the other (except in the case of `ref` and `out` parameter variables).</span></span>

<span data-ttu-id="bbaec-172">C#的實值型別會進一步分成***簡單***型別、***列舉***型別、***結構***型別和***可為 null***的型別，而且C#的參考型別會進一步分割成***類別類型***、***介面類別型***、***陣列類型***和***委派類型***。</span><span class="sxs-lookup"><span data-stu-id="bbaec-172">C#'s value types are further divided into ***simple types***, ***enum types***, ***struct types***, and ***nullable types***, and C#'s reference types are further divided into ***class types***, ***interface types***, ***array types***, and ***delegate types***.</span></span>

<span data-ttu-id="bbaec-173">下表提供類型系統C#的總覽。</span><span class="sxs-lookup"><span data-stu-id="bbaec-173">The following table provides an overview of C#'s type system.</span></span>

| <span data-ttu-id="bbaec-174">__分類__</span><span class="sxs-lookup"><span data-stu-id="bbaec-174">__Category__</span></span>    |                 | <span data-ttu-id="bbaec-175">__描述__</span><span class="sxs-lookup"><span data-stu-id="bbaec-175">__Description__</span></span> |
|-----------------|-----------------|-----------------|
| <span data-ttu-id="bbaec-176">值類型</span><span class="sxs-lookup"><span data-stu-id="bbaec-176">Value types</span></span>     | <span data-ttu-id="bbaec-177">簡單型別</span><span class="sxs-lookup"><span data-stu-id="bbaec-177">Simple types</span></span>    | <span data-ttu-id="bbaec-178">帶正負號的整數︰`sbyte`、`short`、`int`、`long`</span><span class="sxs-lookup"><span data-stu-id="bbaec-178">Signed integral: `sbyte`, `short`, `int`, `long`</span></span> |
|                 |                 | <span data-ttu-id="bbaec-179">不帶正負號的整數︰`byte`、`ushort`、`uint`、`ulong`</span><span class="sxs-lookup"><span data-stu-id="bbaec-179">Unsigned integral: `byte`, `ushort`, `uint`, `ulong`</span></span> |
|                 |                 | <span data-ttu-id="bbaec-180">Unicode 字元：`char`</span><span class="sxs-lookup"><span data-stu-id="bbaec-180">Unicode characters: `char`</span></span> |
|                 |                 | <span data-ttu-id="bbaec-181">IEEE 浮點數：`float`、`double`</span><span class="sxs-lookup"><span data-stu-id="bbaec-181">IEEE floating point: `float`, `double`</span></span> |
|                 |                 | <span data-ttu-id="bbaec-182">高精確度十進位︰`decimal`</span><span class="sxs-lookup"><span data-stu-id="bbaec-182">High-precision decimal: `decimal`</span></span> |
|                 |                 | <span data-ttu-id="bbaec-183">布林值：`bool`</span><span class="sxs-lookup"><span data-stu-id="bbaec-183">Boolean: `bool`</span></span> |
|                 | <span data-ttu-id="bbaec-184">列舉型別</span><span class="sxs-lookup"><span data-stu-id="bbaec-184">Enum types</span></span>      | <span data-ttu-id="bbaec-185">使用者定義型別，格式為 `enum E {...}`</span><span class="sxs-lookup"><span data-stu-id="bbaec-185">User-defined types of the form `enum E {...}`</span></span> |
|                 | <span data-ttu-id="bbaec-186">結構型別</span><span class="sxs-lookup"><span data-stu-id="bbaec-186">Struct types</span></span>    | <span data-ttu-id="bbaec-187">使用者定義型別，格式為 `struct S {...}`</span><span class="sxs-lookup"><span data-stu-id="bbaec-187">User-defined types of the form `struct S {...}`</span></span> |
|                 | <span data-ttu-id="bbaec-188">可為 Null 的型別</span><span class="sxs-lookup"><span data-stu-id="bbaec-188">Nullable types</span></span>  | <span data-ttu-id="bbaec-189">含有 `null` 值的所有其他數值型別的擴充</span><span class="sxs-lookup"><span data-stu-id="bbaec-189">Extensions of all other value types with a `null` value</span></span> |
| <span data-ttu-id="bbaec-190">參考型別</span><span class="sxs-lookup"><span data-stu-id="bbaec-190">Reference types</span></span> | <span data-ttu-id="bbaec-191">類別型別</span><span class="sxs-lookup"><span data-stu-id="bbaec-191">Class types</span></span>     | <span data-ttu-id="bbaec-192">所有其他型別的基底類別︰`object`</span><span class="sxs-lookup"><span data-stu-id="bbaec-192">Ultimate base class of all other types: `object`</span></span> |
|                 |                 | <span data-ttu-id="bbaec-193">Unicode 字串：`string`</span><span class="sxs-lookup"><span data-stu-id="bbaec-193">Unicode strings: `string`</span></span> |
|                 |                 | <span data-ttu-id="bbaec-194">使用者定義型別，格式為 `class C {...}`</span><span class="sxs-lookup"><span data-stu-id="bbaec-194">User-defined types of the form `class C {...}`</span></span> |
|                 | <span data-ttu-id="bbaec-195">介面型別</span><span class="sxs-lookup"><span data-stu-id="bbaec-195">Interface types</span></span> | <span data-ttu-id="bbaec-196">使用者定義型別，格式為 `interface I {...}`</span><span class="sxs-lookup"><span data-stu-id="bbaec-196">User-defined types of the form `interface I {...}`</span></span> |
|                 | <span data-ttu-id="bbaec-197">陣列型別</span><span class="sxs-lookup"><span data-stu-id="bbaec-197">Array types</span></span>     | <span data-ttu-id="bbaec-198">單一維度和多維度，例如 `int[]` 和 `int[,]`</span><span class="sxs-lookup"><span data-stu-id="bbaec-198">Single- and multi-dimensional, for example, `int[]` and `int[,]`</span></span> |
|                 | <span data-ttu-id="bbaec-199">委派型別</span><span class="sxs-lookup"><span data-stu-id="bbaec-199">Delegate types</span></span>  | <span data-ttu-id="bbaec-200">使用者定義類型的格式，例如`delegate int  D(...)`</span><span class="sxs-lookup"><span data-stu-id="bbaec-200">User-defined types of the form e.g. `delegate int  D(...)`</span></span> |

<span data-ttu-id="bbaec-201">八種整數型別支援 8 位元、16 位元、32 位元和 64 位元的值 (帶正負號或不帶正負號)。</span><span class="sxs-lookup"><span data-stu-id="bbaec-201">The eight integral types provide support for 8-bit, 16-bit, 32-bit, and 64-bit values in signed or unsigned form.</span></span>

<span data-ttu-id="bbaec-202">這兩個浮點類型（ `float`和`double`）會使用32位單精確度和64位雙精確度 IEEE 754 格式來表示。</span><span class="sxs-lookup"><span data-stu-id="bbaec-202">The two floating point types, `float` and `double`, are represented using the 32-bit single-precision and 64-bit double-precision IEEE 754 formats.</span></span>

<span data-ttu-id="bbaec-203">`decimal` 型別是 128 位元的資料型別，適合財務和貨幣計算。</span><span class="sxs-lookup"><span data-stu-id="bbaec-203">The `decimal` type is a 128-bit data type suitable for financial and monetary calculations.</span></span>

<span data-ttu-id="bbaec-204">C#的`bool`類型是用來代表布林值，也就`true`是或`false`的值。</span><span class="sxs-lookup"><span data-stu-id="bbaec-204">C#'s `bool` type is used to represent boolean values—values that are either `true` or `false`.</span></span>

<span data-ttu-id="bbaec-205">C# 中的字元和字串處理使用 Unicode 編碼方式。</span><span class="sxs-lookup"><span data-stu-id="bbaec-205">Character and string processing in C# uses Unicode encoding.</span></span> <span data-ttu-id="bbaec-206">`char` 型別代表 UTF-16 程式碼單位，而 `string` 型別代表一系列的 UTF-16 程式碼單位。</span><span class="sxs-lookup"><span data-stu-id="bbaec-206">The `char` type represents a UTF-16 code unit, and the `string` type represents a sequence of UTF-16 code units.</span></span>

<span data-ttu-id="bbaec-207">下表摘要C#說明的數位類型。</span><span class="sxs-lookup"><span data-stu-id="bbaec-207">The following table summarizes C#'s numeric types.</span></span>


| <span data-ttu-id="bbaec-208">__分類__</span><span class="sxs-lookup"><span data-stu-id="bbaec-208">__Category__</span></span>      | <span data-ttu-id="bbaec-209">__一些__</span><span class="sxs-lookup"><span data-stu-id="bbaec-209">__Bits__</span></span> | <span data-ttu-id="bbaec-210">__型別__</span><span class="sxs-lookup"><span data-stu-id="bbaec-210">__Type__</span></span>  | <span data-ttu-id="bbaec-211">__範圍/精確度__</span><span class="sxs-lookup"><span data-stu-id="bbaec-211">__Range/Precision__</span></span> |
|-------------------|----------|-----------|---------------------|
| <span data-ttu-id="bbaec-212">帶正負號的整數</span><span class="sxs-lookup"><span data-stu-id="bbaec-212">Signed integral</span></span>   | <span data-ttu-id="bbaec-213">8</span><span class="sxs-lookup"><span data-stu-id="bbaec-213">8</span></span>        | `sbyte`   | <span data-ttu-id="bbaec-214">-128...127</span><span class="sxs-lookup"><span data-stu-id="bbaec-214">-128...127</span></span> |
|                   | <span data-ttu-id="bbaec-215">16</span><span class="sxs-lookup"><span data-stu-id="bbaec-215">16</span></span>       | `short`   | <span data-ttu-id="bbaec-216">-32768 ... 32，767</span><span class="sxs-lookup"><span data-stu-id="bbaec-216">-32,768...32,767</span></span> |
|                   | <span data-ttu-id="bbaec-217">32</span><span class="sxs-lookup"><span data-stu-id="bbaec-217">32</span></span>       | `int`     | <span data-ttu-id="bbaec-218">-2147483648 ... 2，147，483，647</span><span class="sxs-lookup"><span data-stu-id="bbaec-218">-2,147,483,648...2,147,483,647</span></span> |
|                   | <span data-ttu-id="bbaec-219">64</span><span class="sxs-lookup"><span data-stu-id="bbaec-219">64</span></span>       | `long`    | <span data-ttu-id="bbaec-220">-9223372036854775808 ... 9，223，372，036，854，775，807</span><span class="sxs-lookup"><span data-stu-id="bbaec-220">-9,223,372,036,854,775,808...9,223,372,036,854,775,807</span></span> |
| <span data-ttu-id="bbaec-221">不帶正負號的整數</span><span class="sxs-lookup"><span data-stu-id="bbaec-221">Unsigned integral</span></span> | <span data-ttu-id="bbaec-222">8</span><span class="sxs-lookup"><span data-stu-id="bbaec-222">8</span></span>        | `byte`    | <span data-ttu-id="bbaec-223">0...255</span><span class="sxs-lookup"><span data-stu-id="bbaec-223">0...255</span></span> |
|                   | <span data-ttu-id="bbaec-224">16</span><span class="sxs-lookup"><span data-stu-id="bbaec-224">16</span></span>       | `ushort`  | <span data-ttu-id="bbaec-225">0 ... 65，535</span><span class="sxs-lookup"><span data-stu-id="bbaec-225">0...65,535</span></span> |
|                   | <span data-ttu-id="bbaec-226">32</span><span class="sxs-lookup"><span data-stu-id="bbaec-226">32</span></span>       | `uint`    | <span data-ttu-id="bbaec-227">0 ... 4，294，967，</span><span class="sxs-lookup"><span data-stu-id="bbaec-227">0...4,294,967,295</span></span> |
|                   | <span data-ttu-id="bbaec-228">64</span><span class="sxs-lookup"><span data-stu-id="bbaec-228">64</span></span>       | `ulong`   | <span data-ttu-id="bbaec-229">0 ... 18，446，744，073，709，551，615</span><span class="sxs-lookup"><span data-stu-id="bbaec-229">0...18,446,744,073,709,551,615</span></span> |
| <span data-ttu-id="bbaec-230">浮點</span><span class="sxs-lookup"><span data-stu-id="bbaec-230">Floating point</span></span>    | <span data-ttu-id="bbaec-231">32</span><span class="sxs-lookup"><span data-stu-id="bbaec-231">32</span></span>       | `float`   | <span data-ttu-id="bbaec-232">1.5 × 10 ^ −45到3.4 × 10 ^ 38，7位數精確度</span><span class="sxs-lookup"><span data-stu-id="bbaec-232">1.5 × 10^−45 to 3.4 × 10^38, 7-digit precision</span></span> |
|                   | <span data-ttu-id="bbaec-233">64</span><span class="sxs-lookup"><span data-stu-id="bbaec-233">64</span></span>       | `double`  | <span data-ttu-id="bbaec-234">5.0 × 10 ^ −324到1.7 × 10 ^ 308，15位數精確度</span><span class="sxs-lookup"><span data-stu-id="bbaec-234">5.0 × 10^−324 to 1.7 × 10^308, 15-digit precision</span></span> |
| <span data-ttu-id="bbaec-235">Decimal</span><span class="sxs-lookup"><span data-stu-id="bbaec-235">Decimal</span></span>           | <span data-ttu-id="bbaec-236">128</span><span class="sxs-lookup"><span data-stu-id="bbaec-236">128</span></span>      | `decimal` | <span data-ttu-id="bbaec-237">1.0 × 10 ^ −28至7.9 × 10 ^ 28，28位數精確度</span><span class="sxs-lookup"><span data-stu-id="bbaec-237">1.0 × 10^−28 to 7.9 × 10^28, 28-digit precision</span></span> |

<span data-ttu-id="bbaec-238">C# 程式使用***型別宣告***來建立新型別。</span><span class="sxs-lookup"><span data-stu-id="bbaec-238">C# programs use ***type declarations*** to create new types.</span></span> <span data-ttu-id="bbaec-239">型別宣告指定新型別的名稱成員。</span><span class="sxs-lookup"><span data-stu-id="bbaec-239">A type declaration specifies the name and the members of the new type.</span></span> <span data-ttu-id="bbaec-240">有五C#種類型的類別可由使用者定義：類別類型、結構類型、介面類別型、列舉類型和委派類型。</span><span class="sxs-lookup"><span data-stu-id="bbaec-240">Five of C#'s categories of types are user-definable: class types, struct types, interface types, enum types, and delegate types.</span></span>

<span data-ttu-id="bbaec-241">類別類型會定義包含資料成員（欄位）和函式成員（方法、屬性和其他專案）的資料結構。</span><span class="sxs-lookup"><span data-stu-id="bbaec-241">A class type defines a data structure that contains data members (fields) and function members (methods, properties, and others).</span></span> <span data-ttu-id="bbaec-242">類別型別支援單一繼承和多型，這些是可供衍生類別將基底類別延伸及特製化的機制。</span><span class="sxs-lookup"><span data-stu-id="bbaec-242">Class types support single inheritance and polymorphism, mechanisms whereby derived classes can extend and specialize base classes.</span></span>

<span data-ttu-id="bbaec-243">結構型別類似于類別型別，因為它代表具有資料成員和函式成員的結構。</span><span class="sxs-lookup"><span data-stu-id="bbaec-243">A struct type is similar to a class type in that it represents a structure with data members and function members.</span></span> <span data-ttu-id="bbaec-244">不過，不同于類別，結構是實數值型別，不需要堆積配置。</span><span class="sxs-lookup"><span data-stu-id="bbaec-244">However, unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="bbaec-245">結構型別不支援使用者指定的繼承，且所有結構型別都隱含地繼承自 `object` 型別。</span><span class="sxs-lookup"><span data-stu-id="bbaec-245">Struct types do not support user-specified inheritance, and all struct types implicitly inherit from type `object`.</span></span>

<span data-ttu-id="bbaec-246">介面型別會將合約定義為一組命名的公用函式成員。</span><span class="sxs-lookup"><span data-stu-id="bbaec-246">An interface type defines a contract as a named set of public function members.</span></span> <span data-ttu-id="bbaec-247">實作用介面的類別或結構必須提供介面的函式成員的執行。</span><span class="sxs-lookup"><span data-stu-id="bbaec-247">A class or struct that implements an interface must provide implementations of the interface's function members.</span></span> <span data-ttu-id="bbaec-248">介面可能繼承自多個基底介面，而類別或結構可能會執行多個介面。</span><span class="sxs-lookup"><span data-stu-id="bbaec-248">An interface may inherit from multiple base interfaces, and a class or struct may implement multiple interfaces.</span></span>

<span data-ttu-id="bbaec-249">委派類型代表具有特定參數清單和傳回類型之方法的參考。</span><span class="sxs-lookup"><span data-stu-id="bbaec-249">A delegate type represents references to methods with a particular parameter list and return type.</span></span> <span data-ttu-id="bbaec-250">委派讓您可將方法視為實體，而實體能指派給變數或當作參數來傳遞。</span><span class="sxs-lookup"><span data-stu-id="bbaec-250">Delegates make it possible to treat methods as entities that can be assigned to variables and passed as parameters.</span></span> <span data-ttu-id="bbaec-251">委派就類似其他程式設計語言中的函式指標，但與函式指標的不同之處是，委派是物件導向且為型別安全。</span><span class="sxs-lookup"><span data-stu-id="bbaec-251">Delegates are similar to the concept of function pointers found in some other languages, but unlike function pointers, delegates are object-oriented and type-safe.</span></span>

<span data-ttu-id="bbaec-252">類別、結構、介面和委派類型全都支援泛型，因此可以使用其他類型來參數化。</span><span class="sxs-lookup"><span data-stu-id="bbaec-252">Class, struct, interface and delegate types all support generics, whereby they can be parameterized with other types.</span></span>

<span data-ttu-id="bbaec-253">列舉類型是具有已命名常數的相異類型。</span><span class="sxs-lookup"><span data-stu-id="bbaec-253">An enum type is a distinct type with named constants.</span></span> <span data-ttu-id="bbaec-254">每個列舉類型都具有基礎類型，這必須是八個整數類型的其中一種。</span><span class="sxs-lookup"><span data-stu-id="bbaec-254">Every enum type has an underlying type, which must be one of the eight integral types.</span></span> <span data-ttu-id="bbaec-255">列舉類型的值集合與基礎類型的一組值相同。</span><span class="sxs-lookup"><span data-stu-id="bbaec-255">The set of values of an enum type is the same as the set of values of the underlying type.</span></span>

<span data-ttu-id="bbaec-256">C# 支援任何型別的單一維度及多維陣列。</span><span class="sxs-lookup"><span data-stu-id="bbaec-256">C# supports single- and multi-dimensional arrays of any type.</span></span> <span data-ttu-id="bbaec-257">不同於上述型別，陣列型別不需宣告即可使用。</span><span class="sxs-lookup"><span data-stu-id="bbaec-257">Unlike the types listed above, array types do not have to be declared before they can be used.</span></span> <span data-ttu-id="bbaec-258">而陣列型別的建構方法，是在型別名稱之後加上方括弧。</span><span class="sxs-lookup"><span data-stu-id="bbaec-258">Instead, array types are constructed by following a type name with square brackets.</span></span> <span data-ttu-id="bbaec-259">例如， `int[]`是的一維`int[][]` `int` `int`陣列， `int[,]`是的二維陣列，而且是的一維陣列的一維陣列`int`。</span><span class="sxs-lookup"><span data-stu-id="bbaec-259">For example, `int[]` is a single-dimensional array of `int`, `int[,]` is a two-dimensional array of `int`, and `int[][]` is a single-dimensional array of single-dimensional arrays of `int`.</span></span>

<span data-ttu-id="bbaec-260">可為 null 的型別也不需要宣告，就可以使用它們。</span><span class="sxs-lookup"><span data-stu-id="bbaec-260">Nullable types also do not have to be declared before they can be used.</span></span> <span data-ttu-id="bbaec-261">針對每一個不可為 null 的`T`實數值型別，有一個`T?`對應的可為 null 類型， `null`可以保留額外的值。</span><span class="sxs-lookup"><span data-stu-id="bbaec-261">For each non-nullable value type `T` there is a corresponding nullable type `T?`, which can hold an additional value `null`.</span></span> <span data-ttu-id="bbaec-262">例如， `int?`是可以保存任何32位整數或值`null`的類型。</span><span class="sxs-lookup"><span data-stu-id="bbaec-262">For instance, `int?` is a type that can hold any 32 bit integer or the value `null`.</span></span>

<span data-ttu-id="bbaec-263">C#的類型系統是統一的，因此可以將任何類型的值視為物件。</span><span class="sxs-lookup"><span data-stu-id="bbaec-263">C#'s type system is unified such that a value of any type can be treated as an object.</span></span> <span data-ttu-id="bbaec-264">C# 中的每個型別都直接或間接衍生自 `object` 類別型別，而 `object` 是所有型別的基底類別。</span><span class="sxs-lookup"><span data-stu-id="bbaec-264">Every type in C# directly or indirectly derives from the `object` class type, and `object` is the ultimate base class of all types.</span></span> <span data-ttu-id="bbaec-265">參考型別的值之所以會視為物件，只是將這些值當作 `object` 型別來檢視。</span><span class="sxs-lookup"><span data-stu-id="bbaec-265">Values of reference types are treated as objects simply by viewing the values as type `object`.</span></span> <span data-ttu-id="bbaec-266">實數值型別的值會藉由執行***裝箱***和***取消裝箱***作業來視為物件。</span><span class="sxs-lookup"><span data-stu-id="bbaec-266">Values of value types are treated as objects by performing ***boxing*** and ***unboxing*** operations.</span></span> <span data-ttu-id="bbaec-267">在下列範例中，`int` 值會轉換成 `object`，並再次轉換回 `int`。</span><span class="sxs-lookup"><span data-stu-id="bbaec-267">In the following example, an `int` value is converted to `object` and back again to `int`.</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        int i = 123;
        object o = i;          // Boxing
        int j = (int)o;        // Unboxing
    }
}
```
<span data-ttu-id="bbaec-268">當實數值型別的值轉換成類型`object`時，會設定物件實例（也稱為「方塊」）來保存值，並將值複製到該方塊中。</span><span class="sxs-lookup"><span data-stu-id="bbaec-268">When a value of a value type is converted to type `object`, an object instance, also called a "box," is allocated to hold the value, and the value is copied into that box.</span></span> <span data-ttu-id="bbaec-269">相反地，當`object`參考轉換成實值型別時，會檢查參考的物件是否為正確值型別的方塊，如果檢查成功，則會將方塊中的值複製出來。</span><span class="sxs-lookup"><span data-stu-id="bbaec-269">Conversely, when an `object` reference is cast to a value type, a check is made that the referenced object is a box of the correct value type, and, if the check succeeds, the value in the box is copied out.</span></span>

<span data-ttu-id="bbaec-270">C#的統一型別系統實際上表示實值型別可以「依需求」變成物件。</span><span class="sxs-lookup"><span data-stu-id="bbaec-270">C#'s unified type system effectively means that value types can become objects "on demand."</span></span> <span data-ttu-id="bbaec-271">因為整合，使用 `object` 型別的一般用途程式庫就能搭配參考型別和實值型別使用。</span><span class="sxs-lookup"><span data-stu-id="bbaec-271">Because of the unification, general-purpose libraries that use type `object` can be used with both reference types and value types.</span></span>

<span data-ttu-id="bbaec-272">C# 中有數種***變數***，包括欄位、陣列元素、區域變數和參數。</span><span class="sxs-lookup"><span data-stu-id="bbaec-272">There are several kinds of ***variables*** in C#, including fields, array elements, local variables, and parameters.</span></span> <span data-ttu-id="bbaec-273">變數代表儲存位置，而每個變數都有一個決定可以儲存在變數中之值的型別，如下表所示。</span><span class="sxs-lookup"><span data-stu-id="bbaec-273">Variables represent storage locations, and every variable has a type that determines what values can be stored in the variable, as shown by the following table.</span></span>


| <span data-ttu-id="bbaec-274">__變數的類型__</span><span class="sxs-lookup"><span data-stu-id="bbaec-274">__Type of Variable__</span></span>    | <span data-ttu-id="bbaec-275">__可能的內容__</span><span class="sxs-lookup"><span data-stu-id="bbaec-275">__Possible Contents__</span></span> |
|-------------------------|-----------------------|
| <span data-ttu-id="bbaec-276">不可為 Null 的實值型別</span><span class="sxs-lookup"><span data-stu-id="bbaec-276">Non-nullable value type</span></span> | <span data-ttu-id="bbaec-277">該型別的值</span><span class="sxs-lookup"><span data-stu-id="bbaec-277">A value of that exact type</span></span> |
| <span data-ttu-id="bbaec-278">可為 Null 的實值型別</span><span class="sxs-lookup"><span data-stu-id="bbaec-278">Nullable value type</span></span>     | <span data-ttu-id="bbaec-279">Null 值或該精確類型的值</span><span class="sxs-lookup"><span data-stu-id="bbaec-279">A null value or a value of that exact type</span></span> |
| `object`                | <span data-ttu-id="bbaec-280">Null 參考、任何參考型別之物件的參考，或任何實數值型別之已裝箱值的參考</span><span class="sxs-lookup"><span data-stu-id="bbaec-280">A null reference, a reference to an object of any reference type, or a reference to a boxed value of any value type</span></span> |
| <span data-ttu-id="bbaec-281">類別型別</span><span class="sxs-lookup"><span data-stu-id="bbaec-281">Class type</span></span>              | <span data-ttu-id="bbaec-282">Null 參考、該類別型別之實例的參考，或衍生自該類別型別之類別的實例參考</span><span class="sxs-lookup"><span data-stu-id="bbaec-282">A null reference, a reference to an instance of that class type, or a reference to an instance of a class derived from that class type</span></span> |
| <span data-ttu-id="bbaec-283">介面類型</span><span class="sxs-lookup"><span data-stu-id="bbaec-283">Interface type</span></span>          | <span data-ttu-id="bbaec-284">Null 參考、實作為實介面型別之類別型別的實例參考，或實值型別（實作為介面型別）的參考</span><span class="sxs-lookup"><span data-stu-id="bbaec-284">A null reference, a reference to an instance of a class type that implements that interface type, or a reference to a boxed value of a value type that implements that interface type</span></span> |
| <span data-ttu-id="bbaec-285">陣列型別</span><span class="sxs-lookup"><span data-stu-id="bbaec-285">Array type</span></span>              | <span data-ttu-id="bbaec-286">Null 參考、該陣列型別之實例的參考，或相容的陣列型別之實例的參考</span><span class="sxs-lookup"><span data-stu-id="bbaec-286">A null reference, a reference to an instance of that array type, or a reference to an instance of a compatible array type</span></span> |
| <span data-ttu-id="bbaec-287">委派類型</span><span class="sxs-lookup"><span data-stu-id="bbaec-287">Delegate type</span></span>           | <span data-ttu-id="bbaec-288">Null 參考或該委派型別之實例的參考</span><span class="sxs-lookup"><span data-stu-id="bbaec-288">A null reference or a reference to an instance of that delegate type</span></span> |

## <a name="expressions"></a><span data-ttu-id="bbaec-289">運算式</span><span class="sxs-lookup"><span data-stu-id="bbaec-289">Expressions</span></span>

<span data-ttu-id="bbaec-290">「運算式」是由「運算元」和「運算子」建構而成。</span><span class="sxs-lookup"><span data-stu-id="bbaec-290">***Expressions*** are constructed from ***operands*** and ***operators***.</span></span> <span data-ttu-id="bbaec-291">運算式的運算子會指出要將哪些運算套用到運算元。</span><span class="sxs-lookup"><span data-stu-id="bbaec-291">The operators of an expression indicate which operations to apply to the operands.</span></span> <span data-ttu-id="bbaec-292">運算子範例包括 `+`、`-`、`*`、`/` 及 `new`。</span><span class="sxs-lookup"><span data-stu-id="bbaec-292">Examples of operators include `+`, `-`, `*`, `/`, and `new`.</span></span> <span data-ttu-id="bbaec-293">運算元範例包括常值、欄位、區域變數及運算式。</span><span class="sxs-lookup"><span data-stu-id="bbaec-293">Examples of operands include literals, fields, local variables, and expressions.</span></span>

<span data-ttu-id="bbaec-294">當運算式包含多個運算子時，運算子的「優先順序」會控制評估個別運算子的順序。</span><span class="sxs-lookup"><span data-stu-id="bbaec-294">When an expression contains multiple operators, the ***precedence*** of the operators controls the order in which the individual operators are evaluated.</span></span> <span data-ttu-id="bbaec-295">例如，運算式 `x + y * z` 會評估為 `x + (y * z)`，因為 `*` 運算子的優先順序高於 `+` 運算子。</span><span class="sxs-lookup"><span data-stu-id="bbaec-295">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the `+` operator.</span></span>

<span data-ttu-id="bbaec-296">大多數運算子都是可以「多載」的運算子。</span><span class="sxs-lookup"><span data-stu-id="bbaec-296">Most operators can be ***overloaded***.</span></span> <span data-ttu-id="bbaec-297">運算子多載可允許針對一個運算元屬於 (或兩個運算元都屬於) 使用者定義之類別或結構型別的運算式，指定使用者定義的運算子實作。</span><span class="sxs-lookup"><span data-stu-id="bbaec-297">Operator overloading permits user-defined operator implementations to be specified for operations where one or both of the operands are of a user-defined class or struct type.</span></span>

<span data-ttu-id="bbaec-298">下表摘要C#說明的運算子，以優先順序從最高到最低的順序列出運算子類別目錄。</span><span class="sxs-lookup"><span data-stu-id="bbaec-298">The following table summarizes C#'s operators, listing the operator categories in order of precedence from highest to lowest.</span></span> <span data-ttu-id="bbaec-299">相同分類中的運算子具有相等的優先順序。</span><span class="sxs-lookup"><span data-stu-id="bbaec-299">Operators in the same category have equal precedence.</span></span>


| <span data-ttu-id="bbaec-300">__分類__</span><span class="sxs-lookup"><span data-stu-id="bbaec-300">__Category__</span></span>                     | <span data-ttu-id="bbaec-301">__運算式__</span><span class="sxs-lookup"><span data-stu-id="bbaec-301">__Expression__</span></span>    | <span data-ttu-id="bbaec-302">__描述__</span><span class="sxs-lookup"><span data-stu-id="bbaec-302">__Description__</span></span> |
|----------------------------------|-------------------|-----------------|
| <span data-ttu-id="bbaec-303">主要</span><span class="sxs-lookup"><span data-stu-id="bbaec-303">Primary</span></span>                          | `x.m`             | <span data-ttu-id="bbaec-304">成員存取</span><span class="sxs-lookup"><span data-stu-id="bbaec-304">Member access</span></span> |
|                                  | `x(...)`          | <span data-ttu-id="bbaec-305">方法和委派叫用</span><span class="sxs-lookup"><span data-stu-id="bbaec-305">Method and delegate invocation</span></span> |
|                                  | `x[...]`          | <span data-ttu-id="bbaec-306">陣列和索引子存取</span><span class="sxs-lookup"><span data-stu-id="bbaec-306">Array and indexer access</span></span> |
|                                  | `x++`             | <span data-ttu-id="bbaec-307">後置遞增</span><span class="sxs-lookup"><span data-stu-id="bbaec-307">Post-increment</span></span> |
|                                  | `x--`             | <span data-ttu-id="bbaec-308">後置遞減</span><span class="sxs-lookup"><span data-stu-id="bbaec-308">Post-decrement</span></span> |
|                                  | `new T(...)`      | <span data-ttu-id="bbaec-309">建立物件和委派</span><span class="sxs-lookup"><span data-stu-id="bbaec-309">Object and delegate creation</span></span> |
|                                  | `new T(...){...}` | <span data-ttu-id="bbaec-310">使用初始設定式來建立物件</span><span class="sxs-lookup"><span data-stu-id="bbaec-310">Object creation with initializer</span></span> |
|                                  | `new {...}`       | <span data-ttu-id="bbaec-311">匿名物件初始設定式</span><span class="sxs-lookup"><span data-stu-id="bbaec-311">Anonymous object initializer</span></span> |
|                                  | `new T[...]`      | <span data-ttu-id="bbaec-312">建立陣列</span><span class="sxs-lookup"><span data-stu-id="bbaec-312">Array creation</span></span> |
|                                  | `typeof(T)`       | <span data-ttu-id="bbaec-313">取得 `T` 的 `System.Type` 物件</span><span class="sxs-lookup"><span data-stu-id="bbaec-313">Obtain `System.Type` object for `T`</span></span> |
|                                  | `checked(x)`      | <span data-ttu-id="bbaec-314">在核取的內容中評估運算式</span><span class="sxs-lookup"><span data-stu-id="bbaec-314">Evaluate expression in checked context</span></span> |
|                                  | `unchecked(x)`    | <span data-ttu-id="bbaec-315">在未核取的內容中評估運算式</span><span class="sxs-lookup"><span data-stu-id="bbaec-315">Evaluate expression in unchecked context</span></span> |
|                                  | `default(T)`      | <span data-ttu-id="bbaec-316">取得類型 `T` 的預設值</span><span class="sxs-lookup"><span data-stu-id="bbaec-316">Obtain default value of type `T`</span></span> |
|                                  | `delegate {...}`  | <span data-ttu-id="bbaec-317">匿名函式 (匿名方法)</span><span class="sxs-lookup"><span data-stu-id="bbaec-317">Anonymous function (anonymous method)</span></span> |
| <span data-ttu-id="bbaec-318">一元</span><span class="sxs-lookup"><span data-stu-id="bbaec-318">Unary</span></span>                            | `+x`              | <span data-ttu-id="bbaec-319">身分識別</span><span class="sxs-lookup"><span data-stu-id="bbaec-319">Identity</span></span> |
|                                  | `-x`              | <span data-ttu-id="bbaec-320">否定</span><span class="sxs-lookup"><span data-stu-id="bbaec-320">Negation</span></span> |
|                                  | `!x`              | <span data-ttu-id="bbaec-321">邏輯否定</span><span class="sxs-lookup"><span data-stu-id="bbaec-321">Logical negation</span></span> |
|                                  | `~x`              | <span data-ttu-id="bbaec-322">位元否定</span><span class="sxs-lookup"><span data-stu-id="bbaec-322">Bitwise negation</span></span> |
|                                  | `++x`             | <span data-ttu-id="bbaec-323">前置遞增</span><span class="sxs-lookup"><span data-stu-id="bbaec-323">Pre-increment</span></span> |
|                                  | `--x`             | <span data-ttu-id="bbaec-324">前置遞減</span><span class="sxs-lookup"><span data-stu-id="bbaec-324">Pre-decrement</span></span> |
|                                  | `(T)x`            | <span data-ttu-id="bbaec-325">將 `x` 明確轉換成類型 `T`</span><span class="sxs-lookup"><span data-stu-id="bbaec-325">Explicitly convert `x` to type `T`</span></span> |
|                                  | `await x`         | <span data-ttu-id="bbaec-326">以非同步方式等候 `x` 完成</span><span class="sxs-lookup"><span data-stu-id="bbaec-326">Asynchronously wait for `x` to complete</span></span> |
| <span data-ttu-id="bbaec-327">乘法類 (Multiplicative)</span><span class="sxs-lookup"><span data-stu-id="bbaec-327">Multiplicative</span></span>                   | `x * y`           | <span data-ttu-id="bbaec-328">乘法</span><span class="sxs-lookup"><span data-stu-id="bbaec-328">Multiplication</span></span> |
|                                  | `x / y`           | <span data-ttu-id="bbaec-329">除號</span><span class="sxs-lookup"><span data-stu-id="bbaec-329">Division</span></span> |
|                                  | `x % y`           | <span data-ttu-id="bbaec-330">餘數</span><span class="sxs-lookup"><span data-stu-id="bbaec-330">Remainder</span></span> |
| <span data-ttu-id="bbaec-331">加法類 (Additive)</span><span class="sxs-lookup"><span data-stu-id="bbaec-331">Additive</span></span>                         | `x + y`           | <span data-ttu-id="bbaec-332">加法、字串串連、委派組合</span><span class="sxs-lookup"><span data-stu-id="bbaec-332">Addition, string concatenation, delegate combination</span></span> |
|                                  | `x - y`           | <span data-ttu-id="bbaec-333">減法、委派移除</span><span class="sxs-lookup"><span data-stu-id="bbaec-333">Subtraction, delegate removal</span></span> |
| <span data-ttu-id="bbaec-334">Shift</span><span class="sxs-lookup"><span data-stu-id="bbaec-334">Shift</span></span>                            | `x << y`          | <span data-ttu-id="bbaec-335">左移</span><span class="sxs-lookup"><span data-stu-id="bbaec-335">Shift left</span></span> |
|                                  | `x >> y`          | <span data-ttu-id="bbaec-336">右移</span><span class="sxs-lookup"><span data-stu-id="bbaec-336">Shift right</span></span> |
| <span data-ttu-id="bbaec-337">關係和型別測試</span><span class="sxs-lookup"><span data-stu-id="bbaec-337">Relational and type testing</span></span>      | `x < y`           | <span data-ttu-id="bbaec-338">小於</span><span class="sxs-lookup"><span data-stu-id="bbaec-338">Less than</span></span> |
|                                  | `x > y`           | <span data-ttu-id="bbaec-339">大於</span><span class="sxs-lookup"><span data-stu-id="bbaec-339">Greater than</span></span> |
|                                  | `x <= y`          | <span data-ttu-id="bbaec-340">小於或等於</span><span class="sxs-lookup"><span data-stu-id="bbaec-340">Less than or equal</span></span> |
|                                  | `x >= y`          | <span data-ttu-id="bbaec-341">大於或等於</span><span class="sxs-lookup"><span data-stu-id="bbaec-341">Greater than or equal</span></span> |
|                                  | `x is T`          | <span data-ttu-id="bbaec-342">如果 `x` 是 `T`，便傳回 `true`，否則傳回 `false`</span><span class="sxs-lookup"><span data-stu-id="bbaec-342">Return `true` if `x` is a `T`, `false` otherwise</span></span> |
|                                  | `x as T`          | <span data-ttu-id="bbaec-343">傳回歸類為 `T` 類型的 `x`，或在 `x` 不是 `T` 的情況下傳回 `null`</span><span class="sxs-lookup"><span data-stu-id="bbaec-343">Return `x` typed as `T`, or `null` if `x` is not a `T`</span></span> |
| <span data-ttu-id="bbaec-344">相等</span><span class="sxs-lookup"><span data-stu-id="bbaec-344">Equality</span></span>                         | `x == y`          | <span data-ttu-id="bbaec-345">等於</span><span class="sxs-lookup"><span data-stu-id="bbaec-345">Equal</span></span>      |
|                                  | `x != y`          | <span data-ttu-id="bbaec-346">不等於</span><span class="sxs-lookup"><span data-stu-id="bbaec-346">Not equal</span></span> |
| <span data-ttu-id="bbaec-347">邏輯 AND</span><span class="sxs-lookup"><span data-stu-id="bbaec-347">Logical AND</span></span>                      | `x & y`           | <span data-ttu-id="bbaec-348">整數位元 AND、布林邏輯 AND</span><span class="sxs-lookup"><span data-stu-id="bbaec-348">Integer bitwise AND, boolean logical AND</span></span> |
| <span data-ttu-id="bbaec-349">邏輯 XOR</span><span class="sxs-lookup"><span data-stu-id="bbaec-349">Logical XOR</span></span>                      | `x ^ y`           | <span data-ttu-id="bbaec-350">整數位元 XOR、布林邏輯 XOR</span><span class="sxs-lookup"><span data-stu-id="bbaec-350">Integer bitwise XOR, boolean logical XOR</span></span> |
| <span data-ttu-id="bbaec-351">邏輯 OR</span><span class="sxs-lookup"><span data-stu-id="bbaec-351">Logical OR</span></span>                       | <code>x &#124; y</code> | <span data-ttu-id="bbaec-352">整數位元 OR、布林邏輯 OR</span><span class="sxs-lookup"><span data-stu-id="bbaec-352">Integer bitwise OR, boolean logical OR</span></span> |
| <span data-ttu-id="bbaec-353">條件式 AND</span><span class="sxs-lookup"><span data-stu-id="bbaec-353">Conditional AND</span></span>                  | `x && y`          | <span data-ttu-id="bbaec-354">只有在`y`為時才會 `x`評估`true`</span><span class="sxs-lookup"><span data-stu-id="bbaec-354">Evaluates `y` only if `x` is `true`</span></span> |
| <span data-ttu-id="bbaec-355">條件式 OR</span><span class="sxs-lookup"><span data-stu-id="bbaec-355">Conditional OR</span></span>                   | <code>x &#124;&#124; y</code> | <span data-ttu-id="bbaec-356">只有在`y`為時才會 `x`評估`false`</span><span class="sxs-lookup"><span data-stu-id="bbaec-356">Evaluates `y` only if `x` is `false`</span></span> |
| <span data-ttu-id="bbaec-357">Null 聯合</span><span class="sxs-lookup"><span data-stu-id="bbaec-357">Null coalescing</span></span>                  | `x ?? y`          | <span data-ttu-id="bbaec-358">若為`x` ，則評估為，否則為`null` `x` `y`</span><span class="sxs-lookup"><span data-stu-id="bbaec-358">Evaluates to `y` if `x` is `null`, to `x` otherwise</span></span> |
| <span data-ttu-id="bbaec-359">條件式</span><span class="sxs-lookup"><span data-stu-id="bbaec-359">Conditional</span></span>                      | `x ? y : z`       | <span data-ttu-id="bbaec-360">如果 `x` 是 `true`，便評估 `y`，如果 `x` 是 `false`，則評估 `z`</span><span class="sxs-lookup"><span data-stu-id="bbaec-360">Evaluates `y` if `x` is `true`, `z` if `x` is `false`</span></span> |
| <span data-ttu-id="bbaec-361">指派或匿名函式</span><span class="sxs-lookup"><span data-stu-id="bbaec-361">Assignment or anonymous function</span></span> | `x = y`           | <span data-ttu-id="bbaec-362">指派</span><span class="sxs-lookup"><span data-stu-id="bbaec-362">Assignment</span></span> |
|                                  | `x op= y`         | <span data-ttu-id="bbaec-363">複合指派;支援的運算子`*=`為`/=` `%=` `+=` `-=` `<<=` `>>=` `&=` `^=`<code>&#124;=</code></span><span class="sxs-lookup"><span data-stu-id="bbaec-363">Compound assignment; supported operators are `*=` `/=` `%=` `+=` `-=` `<<=` `>>=` `&=` `^=` <code>&#124;=</code></span></span> |
|                                  | `(T x) => y`      | <span data-ttu-id="bbaec-364">匿名函式 (Lambda 運算式)</span><span class="sxs-lookup"><span data-stu-id="bbaec-364">Anonymous function (lambda expression)</span></span> |

## <a name="statements"></a><span data-ttu-id="bbaec-365">陳述式</span><span class="sxs-lookup"><span data-stu-id="bbaec-365">Statements</span></span>

<span data-ttu-id="bbaec-366">程式的動作是藉由***陳述式***來表達。</span><span class="sxs-lookup"><span data-stu-id="bbaec-366">The actions of a program are expressed using ***statements***.</span></span> <span data-ttu-id="bbaec-367">C# 支援數種不同類型的陳述式，其中一些是以內嵌陳述式來定義。</span><span class="sxs-lookup"><span data-stu-id="bbaec-367">C# supports several different kinds of statements, a number of which are defined in terms of embedded statements.</span></span>

<span data-ttu-id="bbaec-368">「區塊」可允許在許可單一陳述式的內容中撰寫多個陳述式。</span><span class="sxs-lookup"><span data-stu-id="bbaec-368">A ***block*** permits multiple statements to be written in contexts where a single statement is allowed.</span></span> <span data-ttu-id="bbaec-369">區塊是由在 `{` 與 `}` 分隔符號之間撰寫的陳述式清單所組成。</span><span class="sxs-lookup"><span data-stu-id="bbaec-369">A block consists of a list of statements written between the delimiters `{` and `}`.</span></span>

<span data-ttu-id="bbaec-370">「宣告陳述式」可用來宣告區域變數和常數。</span><span class="sxs-lookup"><span data-stu-id="bbaec-370">***Declaration statements*** are used to declare local variables and constants.</span></span>

<span data-ttu-id="bbaec-371">「運算式陳述式」可用來評估運算式。</span><span class="sxs-lookup"><span data-stu-id="bbaec-371">***Expression statements*** are used to evaluate expressions.</span></span> <span data-ttu-id="bbaec-372">可當做語句使用的運算式包含方法叫用、使用運算子的`new`物件配置、使用`=`和複合指派運算子的指派、使用`++`和的「遞增」和「遞減」運算 and`--`運算子和 await 運算式。</span><span class="sxs-lookup"><span data-stu-id="bbaec-372">Expressions that can be used as statements include method invocations, object allocations using the `new` operator, assignments using `=` and the compound assignment operators, increment and decrement operations using the `++` and `--` operators and await expressions.</span></span>

<span data-ttu-id="bbaec-373">「選取範圍陳述式」可用來選取一些可能陳述式的其中之一，以根據某個運算式的值來執行。</span><span class="sxs-lookup"><span data-stu-id="bbaec-373">***Selection statements*** are used to select one of a number of possible statements for execution based on the value of some expression.</span></span> <span data-ttu-id="bbaec-374">在此群組中的是 `if` 和 `switch` 陳述式。</span><span class="sxs-lookup"><span data-stu-id="bbaec-374">In this group are the `if` and `switch` statements.</span></span>

<span data-ttu-id="bbaec-375">反復專案***語句***是用來重複執行內嵌語句。</span><span class="sxs-lookup"><span data-stu-id="bbaec-375">***Iteration statements*** are used to repeatedly execute an embedded statement.</span></span> <span data-ttu-id="bbaec-376">在此群組中的是 `while`、`do`、`for` 及 `foreach` 陳述式。</span><span class="sxs-lookup"><span data-stu-id="bbaec-376">In this group are the `while`, `do`, `for`, and `foreach` statements.</span></span>

<span data-ttu-id="bbaec-377">「跳躍陳述式」可用來轉移控制項。</span><span class="sxs-lookup"><span data-stu-id="bbaec-377">***Jump statements*** are used to transfer control.</span></span> <span data-ttu-id="bbaec-378">在此群組中的是 `break`、`continue`、`goto`、`throw`、`return` 及 `yield` 陳述式。</span><span class="sxs-lookup"><span data-stu-id="bbaec-378">In this group are the `break`, `continue`, `goto`, `throw`, `return`, and `yield` statements.</span></span>

<span data-ttu-id="bbaec-379">`try`...`catch` 陳述式可用來攔截在執行區塊時發生的例外狀況，而 `try`...`finally` 陳述式則可用來指定不論是否發生例外狀況都一律會執行的最終處理程式碼。</span><span class="sxs-lookup"><span data-stu-id="bbaec-379">The `try`...`catch` statement is used to catch exceptions that occur during execution of a block, and the `try`...`finally` statement is used to specify finalization code that is always executed, whether an exception occurred or not.</span></span>

<span data-ttu-id="bbaec-380">`checked` 和`unchecked`語句是用來控制整數型別算數運算和轉換的溢位檢查內容。</span><span class="sxs-lookup"><span data-stu-id="bbaec-380">The `checked` and `unchecked` statements are used to control the overflow checking context for integral-type arithmetic operations and conversions.</span></span>

<span data-ttu-id="bbaec-381">`lock` 陳述式可用來取得所指定物件的互斥鎖定、執行陳述式，然後釋放鎖定。</span><span class="sxs-lookup"><span data-stu-id="bbaec-381">The `lock` statement is used to obtain the mutual-exclusion lock for a given object, execute a statement, and then release the lock.</span></span>

<span data-ttu-id="bbaec-382">`using` 陳述式可用來取得資源、執行陳述式，然後處置該資源。</span><span class="sxs-lookup"><span data-stu-id="bbaec-382">The `using` statement is used to obtain a resource, execute a statement, and then dispose of that resource.</span></span>

<span data-ttu-id="bbaec-383">以下是每種語句的範例</span><span class="sxs-lookup"><span data-stu-id="bbaec-383">Below are examples of each kind of statement</span></span>

<span data-ttu-id="bbaec-384">__區域變數宣告__</span><span class="sxs-lookup"><span data-stu-id="bbaec-384">__Local variable declarations__</span></span>

```csharp
static void Main() {
   int a;
   int b = 2, c = 3;
   a = 1;
   Console.WriteLine(a + b + c);
}
```


<span data-ttu-id="bbaec-385">__區域常數宣告__</span><span class="sxs-lookup"><span data-stu-id="bbaec-385">__Local constant declaration__</span></span>

```csharp
static void Main() {
    const float pi = 3.1415927f;
    const int r = 25;
    Console.WriteLine(pi * r * r);
}
```


<span data-ttu-id="bbaec-386">__Expression 語句__</span><span class="sxs-lookup"><span data-stu-id="bbaec-386">__Expression statement__</span></span>

```csharp
static void Main() {
    int i;
    i = 123;                // Expression statement
    Console.WriteLine(i);   // Expression statement
    i++;                    // Expression statement
    Console.WriteLine(i);   // Expression statement
}
```

<span data-ttu-id="bbaec-387">__`if` 陳述式__</span><span class="sxs-lookup"><span data-stu-id="bbaec-387">__`if` statement__</span></span>

```csharp
static void Main(string[] args) {
    if (args.Length == 0) {
        Console.WriteLine("No arguments");
    }
    else {
        Console.WriteLine("One or more arguments");
    }
}
```


<span data-ttu-id="bbaec-388">__`switch` 陳述式__</span><span class="sxs-lookup"><span data-stu-id="bbaec-388">__`switch` statement__</span></span>

```csharp
static void Main(string[] args) {
    int n = args.Length;
    switch (n) {
        case 0:
            Console.WriteLine("No arguments");
            break;
        case 1:
            Console.WriteLine("One argument");
            break;
        default:
            Console.WriteLine("{0} arguments", n);
            break;
    }
}
```

<span data-ttu-id="bbaec-389">__`while` 陳述式__</span><span class="sxs-lookup"><span data-stu-id="bbaec-389">__`while` statement__</span></span>

```csharp
static void Main(string[] args) {
    int i = 0;
    while (i < args.Length) {
        Console.WriteLine(args[i]);
        i++;
    }
}
```


<span data-ttu-id="bbaec-390">__`do` 陳述式__</span><span class="sxs-lookup"><span data-stu-id="bbaec-390">__`do` statement__</span></span>

```csharp
static void Main() {
    string s;
    do {
        s = Console.ReadLine();
        if (s != null) Console.WriteLine(s);
    } while (s != null);
}
```

<span data-ttu-id="bbaec-391">__`for` 陳述式__</span><span class="sxs-lookup"><span data-stu-id="bbaec-391">__`for` statement__</span></span>

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        Console.WriteLine(args[i]);
    }
}
```

<span data-ttu-id="bbaec-392">__`foreach` 陳述式__</span><span class="sxs-lookup"><span data-stu-id="bbaec-392">__`foreach` statement__</span></span>

```csharp
static void Main(string[] args) {
    foreach (string s in args) {
        Console.WriteLine(s);
    }
}
```

<span data-ttu-id="bbaec-393">__`break` 陳述式__</span><span class="sxs-lookup"><span data-stu-id="bbaec-393">__`break` statement__</span></span>

```csharp
static void Main() {
    while (true) {
        string s = Console.ReadLine();
        if (s == null) break;
        Console.WriteLine(s);
    }
}
```

<span data-ttu-id="bbaec-394">__`continue` 陳述式__</span><span class="sxs-lookup"><span data-stu-id="bbaec-394">__`continue` statement__</span></span>

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        if (args[i].StartsWith("/")) continue;
        Console.WriteLine(args[i]);
    }
}
```

<span data-ttu-id="bbaec-395">__`goto` 陳述式__</span><span class="sxs-lookup"><span data-stu-id="bbaec-395">__`goto` statement__</span></span>

```csharp
static void Main(string[] args) {
    int i = 0;
    goto check;
    loop:
    Console.WriteLine(args[i++]);
    check:
    if (i < args.Length) goto loop;
}
```

<span data-ttu-id="bbaec-396">__`return` 陳述式__</span><span class="sxs-lookup"><span data-stu-id="bbaec-396">__`return` statement__</span></span>

```csharp
static int Add(int a, int b) {
    return a + b;
}

static void Main() {
    Console.WriteLine(Add(1, 2));
    return;
}
```

<span data-ttu-id="bbaec-397">__`yield` 陳述式__</span><span class="sxs-lookup"><span data-stu-id="bbaec-397">__`yield` statement__</span></span>

```csharp
static IEnumerable<int> Range(int from, int to) {
    for (int i = from; i < to; i++) {
        yield return i;
    }
    yield break;
}

static void Main() {
    foreach (int x in Range(-10,10)) {
        Console.WriteLine(x);
    }
}
```

<span data-ttu-id="bbaec-398">__`throw`和`try`語句__</span><span class="sxs-lookup"><span data-stu-id="bbaec-398">__`throw` and `try` statements__</span></span>

```csharp
static double Divide(double x, double y) {
    if (y == 0) throw new DivideByZeroException();
    return x / y;
}

static void Main(string[] args) {
    try {
        if (args.Length != 2) {
            throw new Exception("Two numbers required");
        }
        double x = double.Parse(args[0]);
        double y = double.Parse(args[1]);
        Console.WriteLine(Divide(x, y));
    }
    catch (Exception e) {
        Console.WriteLine(e.Message);
    }
    finally {
        Console.WriteLine("Good bye!");
    }
}
```

<span data-ttu-id="bbaec-399">__`checked`和`unchecked`語句__</span><span class="sxs-lookup"><span data-stu-id="bbaec-399">__`checked` and `unchecked` statements__</span></span>

```csharp
static void Main() {
    int i = int.MaxValue;
    checked {
        Console.WriteLine(i + 1);        // Exception
    }
    unchecked {
        Console.WriteLine(i + 1);        // Overflow
    }
}
```

<span data-ttu-id="bbaec-400">__`lock` 陳述式__</span><span class="sxs-lookup"><span data-stu-id="bbaec-400">__`lock` statement__</span></span>

```csharp
class Account
{
    decimal balance;
    public void Withdraw(decimal amount) {
        lock (this) {
            if (amount > balance) {
                throw new Exception("Insufficient funds");
            }
            balance -= amount;
        }
    }
}
```

<span data-ttu-id="bbaec-401">__`using` 陳述式__</span><span class="sxs-lookup"><span data-stu-id="bbaec-401">__`using` statement__</span></span>

```csharp
static void Main() {
    using (TextWriter w = File.CreateText("test.txt")) {
        w.WriteLine("Line one");
        w.WriteLine("Line two");
        w.WriteLine("Line three");
    }
}
```

## <a name="classes-and-objects"></a><span data-ttu-id="bbaec-402">類別與物件</span><span class="sxs-lookup"><span data-stu-id="bbaec-402">Classes and objects</span></span>

<span data-ttu-id="bbaec-403">***類別***是 C# 最基本的型別。</span><span class="sxs-lookup"><span data-stu-id="bbaec-403">***Classes*** are the most fundamental of C#'s types.</span></span> <span data-ttu-id="bbaec-404">類別是以單一單位結合狀態 (欄位) 和動作 (方法及其他函式成員) 的資料結構。</span><span class="sxs-lookup"><span data-stu-id="bbaec-404">A class is a data structure that combines state (fields) and actions (methods and other function members) in a single unit.</span></span> <span data-ttu-id="bbaec-405">類別可以為動態建立的類別「執行個體」(稱為「物件」) 提供定義。</span><span class="sxs-lookup"><span data-stu-id="bbaec-405">A class provides a definition for dynamically created ***instances*** of the class, also known as ***objects***.</span></span> <span data-ttu-id="bbaec-406">類別支援「繼承」和「多型」，這些是可供「衍生類別」將「基底類別」延伸及特製化的機制。</span><span class="sxs-lookup"><span data-stu-id="bbaec-406">Classes support ***inheritance*** and ***polymorphism***, mechanisms whereby ***derived classes*** can extend and specialize ***base classes***.</span></span>

<span data-ttu-id="bbaec-407">建立新類別時，是使用類別宣告來建立。</span><span class="sxs-lookup"><span data-stu-id="bbaec-407">New classes are created using class declarations.</span></span> <span data-ttu-id="bbaec-408">類別宣告的開頭是一個標頭，此標頭會指定類別的屬性和修飾詞、類別的名稱、基底類別 (如果提供)，以及類別所實作的介面。</span><span class="sxs-lookup"><span data-stu-id="bbaec-408">A class declaration starts with a header that specifies the attributes and modifiers of the class, the name of the class, the base class (if given), and the interfaces implemented by the class.</span></span> <span data-ttu-id="bbaec-409">此標頭後面會接著類別主體，此主體是由在 `{` 與 `}` 分隔符號之間撰寫的成員宣告清單所組成。</span><span class="sxs-lookup"><span data-stu-id="bbaec-409">The header is followed by the class body, which consists of a list of member declarations written between the delimiters `{` and `}`.</span></span>

<span data-ttu-id="bbaec-410">以下是一個名為 `Point` 之簡單類別的宣告：</span><span class="sxs-lookup"><span data-stu-id="bbaec-410">The following is a declaration of a simple class named `Point`:</span></span>

```csharp
public class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
<span data-ttu-id="bbaec-411">建立類別執行個體時，是使用 `new` 運算子來建立，此運算子會為新執行個體配置記憶體、叫用建構函式來將執行個體初始化，然後傳回對執行個體的參考。</span><span class="sxs-lookup"><span data-stu-id="bbaec-411">Instances of classes are created using the `new` operator, which allocates memory for a new instance, invokes a constructor to initialize the instance, and returns a reference to the instance.</span></span> <span data-ttu-id="bbaec-412">下列語句會建立兩`Point`個物件，並在兩個變數中儲存這些物件的參考：</span><span class="sxs-lookup"><span data-stu-id="bbaec-412">The following statements create two `Point` objects and store references to those objects in two variables:</span></span>

```csharp
Point p1 = new Point(0, 0);
Point p2 = new Point(10, 20);
```
<span data-ttu-id="bbaec-413">當物件不再使用時，會自動回收物件所佔用的記憶體。</span><span class="sxs-lookup"><span data-stu-id="bbaec-413">The memory occupied by an object is automatically reclaimed when the object is no longer in use.</span></span> <span data-ttu-id="bbaec-414">在 C# 中，既沒有必要也不可能明確地將物件解除配置。</span><span class="sxs-lookup"><span data-stu-id="bbaec-414">It is neither necessary nor possible to explicitly deallocate objects in C#.</span></span>

### <a name="members"></a><span data-ttu-id="bbaec-415">成員</span><span class="sxs-lookup"><span data-stu-id="bbaec-415">Members</span></span>

<span data-ttu-id="bbaec-416">類別的成員可能是***靜態成員***或***實例成員***。</span><span class="sxs-lookup"><span data-stu-id="bbaec-416">The members of a class are either ***static members*** or ***instance members***.</span></span> <span data-ttu-id="bbaec-417">靜態成員隸屬於類別，而執行個體成員則隸屬於物件 (類別的執行個體)。</span><span class="sxs-lookup"><span data-stu-id="bbaec-417">Static members belong to classes, and instance members belong to objects (instances of classes).</span></span>

<span data-ttu-id="bbaec-418">下表提供類別可以包含之成員種類的總覽。</span><span class="sxs-lookup"><span data-stu-id="bbaec-418">The following table provides an overview of the kinds of members a class can contain.</span></span>


| <span data-ttu-id="bbaec-419">__成員國__</span><span class="sxs-lookup"><span data-stu-id="bbaec-419">__Member__</span></span>   | <span data-ttu-id="bbaec-420">__描述__</span><span class="sxs-lookup"><span data-stu-id="bbaec-420">__Description__</span></span> |
|------------  |-----------------|
| <span data-ttu-id="bbaec-421">常數</span><span class="sxs-lookup"><span data-stu-id="bbaec-421">Constants</span></span>    | <span data-ttu-id="bbaec-422">與類別關聯的常數值</span><span class="sxs-lookup"><span data-stu-id="bbaec-422">Constant values associated with the class</span></span> |
| <span data-ttu-id="bbaec-423">欄位</span><span class="sxs-lookup"><span data-stu-id="bbaec-423">Fields</span></span>       | <span data-ttu-id="bbaec-424">類別的變數</span><span class="sxs-lookup"><span data-stu-id="bbaec-424">Variables of the class</span></span> |
| <span data-ttu-id="bbaec-425">方法</span><span class="sxs-lookup"><span data-stu-id="bbaec-425">Methods</span></span>      | <span data-ttu-id="bbaec-426">類別所能執行的計算和動作</span><span class="sxs-lookup"><span data-stu-id="bbaec-426">Computations and actions that can be performed by the class</span></span> |
| <span data-ttu-id="bbaec-427">屬性</span><span class="sxs-lookup"><span data-stu-id="bbaec-427">Properties</span></span>   | <span data-ttu-id="bbaec-428">與讀取和寫入具名的類別特性關聯的動作</span><span class="sxs-lookup"><span data-stu-id="bbaec-428">Actions associated with reading and writing named properties of the class</span></span> |
| <span data-ttu-id="bbaec-429">索引子</span><span class="sxs-lookup"><span data-stu-id="bbaec-429">Indexers</span></span>     | <span data-ttu-id="bbaec-430">與編製陣列之類的類別執行個體關聯的動作</span><span class="sxs-lookup"><span data-stu-id="bbaec-430">Actions associated with indexing instances of the class like an array</span></span> |
| <span data-ttu-id="bbaec-431">事件</span><span class="sxs-lookup"><span data-stu-id="bbaec-431">Events</span></span>       | <span data-ttu-id="bbaec-432">類別所能產生的通知</span><span class="sxs-lookup"><span data-stu-id="bbaec-432">Notifications that can be generated by the class</span></span> |
| <span data-ttu-id="bbaec-433">運算子</span><span class="sxs-lookup"><span data-stu-id="bbaec-433">Operators</span></span>    | <span data-ttu-id="bbaec-434">類別所支援的轉換和運算式運算子</span><span class="sxs-lookup"><span data-stu-id="bbaec-434">Conversions and expression operators supported by the class</span></span> |
| <span data-ttu-id="bbaec-435">建構函式</span><span class="sxs-lookup"><span data-stu-id="bbaec-435">Constructors</span></span> | <span data-ttu-id="bbaec-436">將類別執行個體或類別本身初始化所需的動作</span><span class="sxs-lookup"><span data-stu-id="bbaec-436">Actions required to initialize instances of the class or the class itself</span></span> |
| <span data-ttu-id="bbaec-437">解構函式</span><span class="sxs-lookup"><span data-stu-id="bbaec-437">Destructors</span></span>  | <span data-ttu-id="bbaec-438">在永久捨棄類別執行個體之前所要執行的動作</span><span class="sxs-lookup"><span data-stu-id="bbaec-438">Actions to perform before instances of the class are permanently discarded</span></span> |
| <span data-ttu-id="bbaec-439">型別</span><span class="sxs-lookup"><span data-stu-id="bbaec-439">Types</span></span>        | <span data-ttu-id="bbaec-440">類別所宣告的巢狀型別</span><span class="sxs-lookup"><span data-stu-id="bbaec-440">Nested types declared by the class</span></span> |

### <a name="accessibility"></a><span data-ttu-id="bbaec-441">協助工具選項</span><span class="sxs-lookup"><span data-stu-id="bbaec-441">Accessibility</span></span>

<span data-ttu-id="bbaec-442">類別的每個成員都有關聯的存取能力，用來控制能夠存取成員的程式文字區域。</span><span class="sxs-lookup"><span data-stu-id="bbaec-442">Each member of a class has an associated accessibility, which controls the regions of program text that are able to access the member.</span></span> <span data-ttu-id="bbaec-443">存取能力有五種可能的形式。</span><span class="sxs-lookup"><span data-stu-id="bbaec-443">There are five possible forms of accessibility.</span></span> <span data-ttu-id="bbaec-444">下表將摘要說明這些報表：</span><span class="sxs-lookup"><span data-stu-id="bbaec-444">These are summarized in the following table.</span></span>


| <span data-ttu-id="bbaec-445">__協助工具選項__</span><span class="sxs-lookup"><span data-stu-id="bbaec-445">__Accessibility__</span></span>    | <span data-ttu-id="bbaec-446">__含義__</span><span class="sxs-lookup"><span data-stu-id="bbaec-446">__Meaning__</span></span> |
|----------------------|-----------------|
| `public`             | <span data-ttu-id="bbaec-447">存取不受限制</span><span class="sxs-lookup"><span data-stu-id="bbaec-447">Access not limited</span></span> |
| `protected`          | <span data-ttu-id="bbaec-448">存取僅限於此類別或此類別所衍生的類別</span><span class="sxs-lookup"><span data-stu-id="bbaec-448">Access limited to this class or classes derived from this class</span></span> |
| `internal`           | <span data-ttu-id="bbaec-449">存取僅限於此程式</span><span class="sxs-lookup"><span data-stu-id="bbaec-449">Access limited to this program</span></span> |
| `protected internal` | <span data-ttu-id="bbaec-450">存取僅限於此程式或此類別所衍生的類別</span><span class="sxs-lookup"><span data-stu-id="bbaec-450">Access limited to this program or classes derived from this class</span></span> |
| `private`            | <span data-ttu-id="bbaec-451">存取僅限於此類別</span><span class="sxs-lookup"><span data-stu-id="bbaec-451">Access limited to this class</span></span> |

### <a name="type-parameters"></a><span data-ttu-id="bbaec-452">型別參數</span><span class="sxs-lookup"><span data-stu-id="bbaec-452">Type parameters</span></span>

<span data-ttu-id="bbaec-453">類別定義可以在類別名稱後面以角括弧括住型別參數名稱清單，來定義一組型別參數。</span><span class="sxs-lookup"><span data-stu-id="bbaec-453">A class definition may specify a set of type parameters by following the class name with angle brackets enclosing a list of type parameter names.</span></span> <span data-ttu-id="bbaec-454">型別參數可以在類別宣告的主體中用來定義類別的成員。</span><span class="sxs-lookup"><span data-stu-id="bbaec-454">The type parameters can the be used in the body of the class declarations to define the members of the class.</span></span> <span data-ttu-id="bbaec-455">在下列範例中，`Pair` 的型別參數是 `TFirst` 和 `TSecond`：</span><span class="sxs-lookup"><span data-stu-id="bbaec-455">In the following example, the type parameters of `Pair` are `TFirst` and `TSecond`:</span></span>

```csharp
public class Pair<TFirst,TSecond>
{
    public TFirst First;
    public TSecond Second;
}
```
<span data-ttu-id="bbaec-456">宣告為接受型別參數的類別型別稱為「泛型類別型別」。</span><span class="sxs-lookup"><span data-stu-id="bbaec-456">A class type that is declared to take type parameters is called a generic class type.</span></span> <span data-ttu-id="bbaec-457">結構、介面及委派型別也可以是泛型型別。</span><span class="sxs-lookup"><span data-stu-id="bbaec-457">Struct, interface and delegate types can also be generic.</span></span>

<span data-ttu-id="bbaec-458">使用泛型類別時，必須為每個型別參數提供型別參數：</span><span class="sxs-lookup"><span data-stu-id="bbaec-458">When the generic class is used, type arguments must be provided for each of the type parameters:</span></span>

```csharp
Pair<int,string> pair = new Pair<int,string> { First = 1, Second = "two" };
int i = pair.First;     // TFirst is int
string s = pair.Second; // TSecond is string
```
<span data-ttu-id="bbaec-459">提供型別引數的泛型型別`Pair<int,string>` （如上所示）稱為「結構化型別」。</span><span class="sxs-lookup"><span data-stu-id="bbaec-459">A generic type with type arguments provided, like `Pair<int,string>` above, is called a constructed type.</span></span>

### <a name="base-classes"></a><span data-ttu-id="bbaec-460">基底類別</span><span class="sxs-lookup"><span data-stu-id="bbaec-460">Base classes</span></span>

<span data-ttu-id="bbaec-461">類別宣告可以在類別名稱和型別參數後面加上冒號和基底類別的名稱，來指定基底類別。</span><span class="sxs-lookup"><span data-stu-id="bbaec-461">A class declaration may specify a base class by following the class name and type parameters with a colon and the name of the base class.</span></span> <span data-ttu-id="bbaec-462">省略基底類別規格即等同於衍生自類型 `object`。</span><span class="sxs-lookup"><span data-stu-id="bbaec-462">Omitting a base class specification is the same as deriving from type `object`.</span></span> <span data-ttu-id="bbaec-463">在下列範例中，`Point3D` 的基底類別是 `Point`，而 `Point` 的基底類別是 `object`：</span><span class="sxs-lookup"><span data-stu-id="bbaec-463">In the following example, the base class of `Point3D` is `Point`, and the base class of `Point` is `object`:</span></span>

```csharp
public class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

public class Point3D: Point
{
    public int z;

    public Point3D(int x, int y, int z): base(x, y) {
        this.z = z;
    }
}
```
<span data-ttu-id="bbaec-464">類別會繼承其基底類別的成員。</span><span class="sxs-lookup"><span data-stu-id="bbaec-464">A class inherits the members of its base class.</span></span> <span data-ttu-id="bbaec-465">繼承表示，類別會隱含包含其基類的所有成員，但實例和靜態的函式除外，以及基類的析構函數。</span><span class="sxs-lookup"><span data-stu-id="bbaec-465">Inheritance means that a class implicitly contains all members of its base class, except for the instance and static constructors, and the destructors of the base class.</span></span> <span data-ttu-id="bbaec-466">衍生類別可以在其繼承的成員中新增新的成員，但無法移除所繼承成員的定義。</span><span class="sxs-lookup"><span data-stu-id="bbaec-466">A derived class can add new members to those it inherits, but it cannot remove the definition of an inherited member.</span></span> <span data-ttu-id="bbaec-467">在先前的範例中，`Point3D` 會從 `Point` 繼承 `x` 和 `y` 欄位，而每個 `Point3D` 執行個體都會包含 `x`、`y` 及 `z` 這三個欄位。</span><span class="sxs-lookup"><span data-stu-id="bbaec-467">In the previous example, `Point3D` inherits the `x` and `y` fields from `Point`, and every `Point3D` instance contains three fields, `x`, `y`, and `z`.</span></span>

<span data-ttu-id="bbaec-468">在類別型別到其任何基底類別型別之間都存在著隱含轉換。</span><span class="sxs-lookup"><span data-stu-id="bbaec-468">An implicit conversion exists from a class type to any of its base class types.</span></span> <span data-ttu-id="bbaec-469">因此，類別型別的變數可以參考該類別的執行個體，或任何衍生類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="bbaec-469">Therefore, a variable of a class type can reference an instance of that class or an instance of any derived class.</span></span> <span data-ttu-id="bbaec-470">例如，以先前的類別宣告為例，`Point` 型別的變數可以參考 `Point` 或 `Point3D`：</span><span class="sxs-lookup"><span data-stu-id="bbaec-470">For example, given the previous class declarations, a variable of type `Point` can reference either a `Point` or a `Point3D`:</span></span>

```csharp
Point a = new Point(10, 20);
Point b = new Point3D(10, 20, 30);
```

### <a name="fields"></a><span data-ttu-id="bbaec-471">欄位</span><span class="sxs-lookup"><span data-stu-id="bbaec-471">Fields</span></span>

<span data-ttu-id="bbaec-472">欄位是與類別或類別的實例相關聯的變數。</span><span class="sxs-lookup"><span data-stu-id="bbaec-472">A field is a variable that is associated with a class or with an instance of a class.</span></span>

<span data-ttu-id="bbaec-473">以`static`修飾詞宣告的欄位會定義***靜態欄位***。</span><span class="sxs-lookup"><span data-stu-id="bbaec-473">A field declared with the `static` modifier defines a ***static field***.</span></span> <span data-ttu-id="bbaec-474">靜態欄位只會識別一個儲存位置。</span><span class="sxs-lookup"><span data-stu-id="bbaec-474">A static field identifies exactly one storage location.</span></span> <span data-ttu-id="bbaec-475">不論建立多少個類別執行個體，都只會有一個靜態欄位複本。</span><span class="sxs-lookup"><span data-stu-id="bbaec-475">No matter how many instances of a class are created, there is only ever one copy of a static field.</span></span>

<span data-ttu-id="bbaec-476">未使用修飾詞所`static`宣告的欄位會定義***實例欄位***。</span><span class="sxs-lookup"><span data-stu-id="bbaec-476">A field declared without the `static` modifier defines an ***instance field***.</span></span> <span data-ttu-id="bbaec-477">每個類別執行個體都包含一個該類別所有執行個體欄位的個別複本。</span><span class="sxs-lookup"><span data-stu-id="bbaec-477">Every instance of a class contains a separate copy of all the instance fields of that class.</span></span>

<span data-ttu-id="bbaec-478">在下列範例中，每個 `Color` 類別執行個體都具有 `r`、`g` 及 `b` 執行個體欄位的個別複本，但只有一個 `Black`、`White`、`Red`、`Green` 及 `Blue` 靜態欄位的複本：</span><span class="sxs-lookup"><span data-stu-id="bbaec-478">In the following example, each instance of the `Color` class has a separate copy of the `r`, `g`, and `b` instance fields, but there is only one copy of the `Black`, `White`, `Red`, `Green`, and `Blue` static fields:</span></span>

```csharp
public class Color
{
    public static readonly Color Black = new Color(0, 0, 0);
    public static readonly Color White = new Color(255, 255, 255);
    public static readonly Color Red = new Color(255, 0, 0);
    public static readonly Color Green = new Color(0, 255, 0);
    public static readonly Color Blue = new Color(0, 0, 255);
    private byte r, g, b;

    public Color(byte r, byte g, byte b) {
        this.r = r;
        this.g = g;
        this.b = b;
    }
}
```
<span data-ttu-id="bbaec-479">如先前的範例所示，可以使用 `readonly` 修飾詞來宣告「唯讀欄位」。</span><span class="sxs-lookup"><span data-stu-id="bbaec-479">As shown in the previous example, ***read-only fields*** may be declared with a `readonly` modifier.</span></span> <span data-ttu-id="bbaec-480">對`readonly`欄位的指派只會當做欄位宣告的一部分，或在相同類別的函式中發生。</span><span class="sxs-lookup"><span data-stu-id="bbaec-480">Assignment to a `readonly` field can only occur as part of the field's declaration or in a constructor in the same class.</span></span>

### <a name="methods"></a><span data-ttu-id="bbaec-481">方法</span><span class="sxs-lookup"><span data-stu-id="bbaec-481">Methods</span></span>

<span data-ttu-id="bbaec-482">「方法」是實作物件或類別所能執行之計算或動作的成員。</span><span class="sxs-lookup"><span data-stu-id="bbaec-482">A ***method*** is a member that implements a computation or action that can be performed by an object or class.</span></span> <span data-ttu-id="bbaec-483">存取「靜態方法」時，是透過類別來存取。</span><span class="sxs-lookup"><span data-stu-id="bbaec-483">***Static methods*** are accessed through the class.</span></span> <span data-ttu-id="bbaec-484">存取「執行個體方法」時，是透過類別的執行個體來存取。</span><span class="sxs-lookup"><span data-stu-id="bbaec-484">***Instance methods*** are accessed through instances of the class.</span></span>

<span data-ttu-id="bbaec-485">方法具有（可能是空的）***參數***清單，其代表傳遞至方法的值或變數參考，以及傳回***類型***，指定方法所計算和傳回的數值型別。</span><span class="sxs-lookup"><span data-stu-id="bbaec-485">Methods have a (possibly empty) list of ***parameters***, which represent values or variable references passed to the method, and a ***return type***, which specifies the type of the value computed and returned by the method.</span></span> <span data-ttu-id="bbaec-486">如果方法的傳回型別`void`不會傳回值，則為。</span><span class="sxs-lookup"><span data-stu-id="bbaec-486">A method's return type is `void` if it does not return a value.</span></span>

<span data-ttu-id="bbaec-487">與型別相同，方法也可能有一組型別參數，而呼叫方法時，必須為這些參數指定型別引數。</span><span class="sxs-lookup"><span data-stu-id="bbaec-487">Like types, methods may also have a set of type parameters, for which type arguments must be specified when the method is called.</span></span> <span data-ttu-id="bbaec-488">與型別不同的是，型別引數通常可以從方法呼叫的引數推斷，而不需要明確指定。</span><span class="sxs-lookup"><span data-stu-id="bbaec-488">Unlike types, the type arguments can often be inferred from the arguments of a method call and need not be explicitly given.</span></span>

<span data-ttu-id="bbaec-489">在宣告方法的類別中，方法的「簽章」必須是唯一的。</span><span class="sxs-lookup"><span data-stu-id="bbaec-489">The ***signature*** of a method must be unique in the class in which the method is declared.</span></span> <span data-ttu-id="bbaec-490">方法的簽章是由方法的名稱、型別參數的數目以及其參數的數目、修飾詞和型別所組成。</span><span class="sxs-lookup"><span data-stu-id="bbaec-490">The signature of a method consists of the name of the method, the number of type parameters and the number, modifiers, and types of its parameters.</span></span> <span data-ttu-id="bbaec-491">方法的簽章並不包括傳回型別。</span><span class="sxs-lookup"><span data-stu-id="bbaec-491">The signature of a method does not include the return type.</span></span>

#### <a name="parameters"></a><span data-ttu-id="bbaec-492">參數</span><span class="sxs-lookup"><span data-stu-id="bbaec-492">Parameters</span></span>

<span data-ttu-id="bbaec-493">參數是用來將值或變數參考傳遞給方法。</span><span class="sxs-lookup"><span data-stu-id="bbaec-493">Parameters are used to pass values or variable references to methods.</span></span> <span data-ttu-id="bbaec-494">方法的參數會從叫用方法時所指定的「引數」取得其實際值。</span><span class="sxs-lookup"><span data-stu-id="bbaec-494">The parameters of a method get their actual values from the ***arguments*** that are specified when the method is invoked.</span></span> <span data-ttu-id="bbaec-495">參數有四種：值參數、參考參數、輸出參數，以及參數陣列。</span><span class="sxs-lookup"><span data-stu-id="bbaec-495">There are four kinds of parameters: value parameters, reference parameters, output parameters, and parameter arrays.</span></span>

<span data-ttu-id="bbaec-496">「值參數」適用於傳遞輸入參數。</span><span class="sxs-lookup"><span data-stu-id="bbaec-496">A ***value parameter*** is used for input parameter passing.</span></span> <span data-ttu-id="bbaec-497">值參數會對應至區域變數，此變數會從針對參數傳遞的引數取得其初始值。</span><span class="sxs-lookup"><span data-stu-id="bbaec-497">A value parameter corresponds to a local variable that gets its initial value from the argument that was passed for the parameter.</span></span> <span data-ttu-id="bbaec-498">對值參數進行修改並不會影響已針對參數傳遞的引數。</span><span class="sxs-lookup"><span data-stu-id="bbaec-498">Modifications to a value parameter do not affect the argument that was passed for the parameter.</span></span>

<span data-ttu-id="bbaec-499">只要指定預設值，值參數便可以成為選用參數，如此即可省略對應的引數。</span><span class="sxs-lookup"><span data-stu-id="bbaec-499">Value parameters can be optional, by specifying a default value so that corresponding arguments can be omitted.</span></span>

<span data-ttu-id="bbaec-500">「參考參數」同時適用於傳遞輸入和輸出參數。</span><span class="sxs-lookup"><span data-stu-id="bbaec-500">A ***reference parameter*** is used for both input and output parameter passing.</span></span> <span data-ttu-id="bbaec-501">針對參考參數傳遞的引數必須是變數，而在執行方法的期間，參考參數會代表與引數變數相同的儲存位置。</span><span class="sxs-lookup"><span data-stu-id="bbaec-501">The argument passed for a reference parameter must be a variable, and during execution of the method, the reference parameter represents the same storage location as the argument variable.</span></span> <span data-ttu-id="bbaec-502">宣告參考參數時，是使用 `ref` 修飾詞來宣告。</span><span class="sxs-lookup"><span data-stu-id="bbaec-502">A reference parameter is declared with the `ref` modifier.</span></span> <span data-ttu-id="bbaec-503">下列範例示範 `ref` 參數的用法。</span><span class="sxs-lookup"><span data-stu-id="bbaec-503">The following example shows the use of `ref` parameters.</span></span>

```csharp
using System;

class Test
{
    static void Swap(ref int x, ref int y) {
        int temp = x;
        x = y;
        y = temp;
    }

    static void Main() {
        int i = 1, j = 2;
        Swap(ref i, ref j);
        Console.WriteLine("{0} {1}", i, j);            // Outputs "2 1"
    }
}
```
<span data-ttu-id="bbaec-504">「輸出參數」適用於傳遞輸出參數。</span><span class="sxs-lookup"><span data-stu-id="bbaec-504">An ***output parameter*** is used for output parameter passing.</span></span> <span data-ttu-id="bbaec-505">輸出參數與參考參數類似，不同之處在於呼叫端所提供引數的初始值並不重要。</span><span class="sxs-lookup"><span data-stu-id="bbaec-505">An output parameter is similar to a reference parameter except that the initial value of the caller-provided argument is unimportant.</span></span> <span data-ttu-id="bbaec-506">宣告輸出參數時，是使用 `out` 修飾詞來宣告。</span><span class="sxs-lookup"><span data-stu-id="bbaec-506">An output parameter is declared with the `out` modifier.</span></span> <span data-ttu-id="bbaec-507">下列範例示範 `out` 參數的用法。</span><span class="sxs-lookup"><span data-stu-id="bbaec-507">The following example shows the use of `out` parameters.</span></span>

```csharp
using System;

class Test
{
    static void Divide(int x, int y, out int result, out int remainder) {
        result = x / y;
        remainder = x % y;
    }

    static void Main() {
        int res, rem;
        Divide(10, 3, out res, out rem);
        Console.WriteLine("{0} {1}", res, rem);    // Outputs "3 1"
    }
}
```
<span data-ttu-id="bbaec-508">「參數陣列」可允許將數目不固定的引數傳遞給方法。</span><span class="sxs-lookup"><span data-stu-id="bbaec-508">A ***parameter array*** permits a variable number of arguments to be passed to a method.</span></span> <span data-ttu-id="bbaec-509">宣告參數陣列時，是使用 `params` 修飾詞來宣告。</span><span class="sxs-lookup"><span data-stu-id="bbaec-509">A parameter array is declared with the `params` modifier.</span></span> <span data-ttu-id="bbaec-510">只有方法的最後一個參數可以是參數陣列，而參數陣列的型別必須是單一維度陣列型別。</span><span class="sxs-lookup"><span data-stu-id="bbaec-510">Only the last parameter of a method can be a parameter array, and the type of a parameter array must be a single-dimensional array type.</span></span> <span data-ttu-id="bbaec-511">`System.Console`類別`Write`的`WriteLine`和方法是參數陣列使用方式的良好範例。</span><span class="sxs-lookup"><span data-stu-id="bbaec-511">The `Write` and `WriteLine` methods of the `System.Console` class are good examples of parameter array usage.</span></span> <span data-ttu-id="bbaec-512">其宣告方式如下。</span><span class="sxs-lookup"><span data-stu-id="bbaec-512">They are declared as follows.</span></span>

```csharp
public class Console
{
    public static void Write(string fmt, params object[] args) {...}
    public static void WriteLine(string fmt, params object[] args) {...}
    ...
}
```
<span data-ttu-id="bbaec-513">在使用參數陣列的方法內，參數陣列的行為與陣列型別的一般參數完全相同。</span><span class="sxs-lookup"><span data-stu-id="bbaec-513">Within a method that uses a parameter array, the parameter array behaves exactly like a regular parameter of an array type.</span></span> <span data-ttu-id="bbaec-514">不過，在叫用含有參數陣列的方法時，可以傳遞單一的參數陣列型別引數，或是傳遞任意數目的參數陣列元素型別引數。</span><span class="sxs-lookup"><span data-stu-id="bbaec-514">However, in an invocation of a method with a parameter array, it is possible to pass either a single argument of the parameter array type or any number of arguments of the element type of the parameter array.</span></span> <span data-ttu-id="bbaec-515">在後者的案例中，會自動建立陣列執行個體並以指定的引數將其初始化。</span><span class="sxs-lookup"><span data-stu-id="bbaec-515">In the latter case, an array instance is automatically created and initialized with the given arguments.</span></span> <span data-ttu-id="bbaec-516">以下範例</span><span class="sxs-lookup"><span data-stu-id="bbaec-516">This example</span></span>

```csharp
Console.WriteLine("x={0} y={1} z={2}", x, y, z);
```
<span data-ttu-id="bbaec-517">等同於撰寫下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="bbaec-517">is equivalent to writing the following.</span></span>

```csharp
string s = "x={0} y={1} z={2}";
object[] args = new object[3];
args[0] = x;
args[1] = y;
args[2] = z;
Console.WriteLine(s, args);
```

#### <a name="method-body-and-local-variables"></a><span data-ttu-id="bbaec-518">方法主體和區域變數</span><span class="sxs-lookup"><span data-stu-id="bbaec-518">Method body and local variables</span></span>

<span data-ttu-id="bbaec-519">方法的主體會指定叫用方法時要執行的語句。</span><span class="sxs-lookup"><span data-stu-id="bbaec-519">A method's body specifies the statements to execute when the method is invoked.</span></span>

<span data-ttu-id="bbaec-520">方法主體可以宣告方法叫用專屬的變數。</span><span class="sxs-lookup"><span data-stu-id="bbaec-520">A method body can declare variables that are specific to the invocation of the method.</span></span> <span data-ttu-id="bbaec-521">這類變數稱為「區域變數」。</span><span class="sxs-lookup"><span data-stu-id="bbaec-521">Such variables are called ***local variables***.</span></span> <span data-ttu-id="bbaec-522">區域變數宣告會指定型別名稱、變數名稱，還可能指定初始值。</span><span class="sxs-lookup"><span data-stu-id="bbaec-522">A local variable declaration specifies a type name, a variable name, and possibly an initial value.</span></span> <span data-ttu-id="bbaec-523">下列範例會宣告一個初始值為零的區域變數 `i`，以及一個沒有初始值的區域變數 `j`。</span><span class="sxs-lookup"><span data-stu-id="bbaec-523">The following example declares a local variable `i` with an initial value of zero and a local variable `j` with no initial value.</span></span>

```csharp
using System;

class Squares
{
    static void Main() {
        int i = 0;
        int j;
        while (i < 10) {
            j = i * i;
            Console.WriteLine("{0} x {0} = {1}", i, j);
            i = i + 1;
        }
    }
}
```
<span data-ttu-id="bbaec-524">C# 要求必須「明確指派」區域變數，才能取得其值。</span><span class="sxs-lookup"><span data-stu-id="bbaec-524">C# requires a local variable to be ***definitely assigned*** before its value can be obtained.</span></span> <span data-ttu-id="bbaec-525">例如，如果先前 `i` 的宣告並未包含初始值，編譯器就會在 `i` 後續被使用時回報錯誤，因為在這些時間點並不會在程式中明確指派 `i`。</span><span class="sxs-lookup"><span data-stu-id="bbaec-525">For example, if the declaration of the previous `i` did not include an initial value, the compiler would report an error for the subsequent usages of `i` because `i` would not be definitely assigned at those points in the program.</span></span>

<span data-ttu-id="bbaec-526">方法可以使用 `return` 陳述式將控制權交還給其呼叫端。</span><span class="sxs-lookup"><span data-stu-id="bbaec-526">A method can use `return` statements to return control to its caller.</span></span> <span data-ttu-id="bbaec-527">在傳回 `void` 的方法中，`return` 陳述式不能指定運算式。</span><span class="sxs-lookup"><span data-stu-id="bbaec-527">In a method returning `void`, `return` statements cannot specify an expression.</span></span> <span data-ttu-id="bbaec-528">在傳回非`void`的方法中， `return`語句必須包含計算傳回值的運算式。</span><span class="sxs-lookup"><span data-stu-id="bbaec-528">In a method returning non-`void`, `return` statements must include an expression that computes the return value.</span></span>

#### <a name="static-and-instance-methods"></a><span data-ttu-id="bbaec-529">靜態和執行個體方法</span><span class="sxs-lookup"><span data-stu-id="bbaec-529">Static and instance methods</span></span>

<span data-ttu-id="bbaec-530">使用`static`修飾詞宣告的方法是***靜態方法***。</span><span class="sxs-lookup"><span data-stu-id="bbaec-530">A method declared with a `static` modifier is a ***static method***.</span></span> <span data-ttu-id="bbaec-531">靜態方法不會在特定的執行個體上運作，並且只能直接存取靜態成員。</span><span class="sxs-lookup"><span data-stu-id="bbaec-531">A static method does not operate on a specific instance and can only directly access static members.</span></span>

<span data-ttu-id="bbaec-532">未使用`static`修飾詞宣告的方法是***實例方法***。</span><span class="sxs-lookup"><span data-stu-id="bbaec-532">A method declared without a `static` modifier is an ***instance method***.</span></span> <span data-ttu-id="bbaec-533">執行個體方法會在特定的執行個體上運作，並且既可存取靜態成員也可存取執行個體成員。</span><span class="sxs-lookup"><span data-stu-id="bbaec-533">An instance method operates on a specific instance and can access both static and instance members.</span></span> <span data-ttu-id="bbaec-534">透過 `this` 可以明確存取叫用執行個體方法時所在的執行個體。</span><span class="sxs-lookup"><span data-stu-id="bbaec-534">The instance on which an instance method was invoked can be explicitly accessed as `this`.</span></span> <span data-ttu-id="bbaec-535">參考靜態方法中的 `this` 會產生錯誤。</span><span class="sxs-lookup"><span data-stu-id="bbaec-535">It is an error to refer to `this` in a static method.</span></span>

<span data-ttu-id="bbaec-536">下列 `Entity` 類別同時含有靜態成員和執行個體成員。</span><span class="sxs-lookup"><span data-stu-id="bbaec-536">The following `Entity` class has both static and instance members.</span></span>

```csharp
class Entity
{
    static int nextSerialNo;
    int serialNo;

    public Entity() {
        serialNo = nextSerialNo++;
    }

    public int GetSerialNo() {
        return serialNo;
    }

    public static int GetNextSerialNo() {
        return nextSerialNo;
    }

    public static void SetNextSerialNo(int value) {
        nextSerialNo = value;
    }
}
```
<span data-ttu-id="bbaec-537">每個 `Entity` 執行個體都包含一個序號 (並且可能包含一些其他這裡未顯示的資訊)。</span><span class="sxs-lookup"><span data-stu-id="bbaec-537">Each `Entity` instance contains a serial number (and presumably some other information that is not shown here).</span></span> <span data-ttu-id="bbaec-538">`Entity` 建構函式 (類似於執行個體方法) 會將具有下一個可用序號的新執行個體初始化。</span><span class="sxs-lookup"><span data-stu-id="bbaec-538">The `Entity` constructor (which is like an instance method) initializes the new instance with the next available serial number.</span></span> <span data-ttu-id="bbaec-539">由於此建構函式式執行個體成員，因此它既可存取 `serialNo` 執行個體欄位，也可存取 `nextSerialNo` 靜態欄位。</span><span class="sxs-lookup"><span data-stu-id="bbaec-539">Because the constructor is an instance member, it is permitted to access both the `serialNo` instance field and the `nextSerialNo` static field.</span></span>

<span data-ttu-id="bbaec-540">`GetNextSerialNo` 和 `SetNextSerialNo` 靜態方法可以存取 `nextSerialNo` 靜態欄位，但如果直接存取 `serialNo` 執行個體欄位，則會產生錯誤。</span><span class="sxs-lookup"><span data-stu-id="bbaec-540">The `GetNextSerialNo` and `SetNextSerialNo` static methods can access the `nextSerialNo` static field, but it would be an error for them to directly access the `serialNo` instance field.</span></span>

<span data-ttu-id="bbaec-541">下列範例示範如何使用`Entity`類別。</span><span class="sxs-lookup"><span data-stu-id="bbaec-541">The following example shows the use of the `Entity` class.</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        Entity.SetNextSerialNo(1000);
        Entity e1 = new Entity();
        Entity e2 = new Entity();
        Console.WriteLine(e1.GetSerialNo());           // Outputs "1000"
        Console.WriteLine(e2.GetSerialNo());           // Outputs "1001"
        Console.WriteLine(Entity.GetNextSerialNo());   // Outputs "1002"
    }
}
```
<span data-ttu-id="bbaec-542">請注意，`SetNextSerialNo` 和 `GetNextSerialNo` 靜態方法的叫用位置是在類別上，而 `GetSerialNo` 執行個體方法的叫用位置則是在類別的執行個體上。</span><span class="sxs-lookup"><span data-stu-id="bbaec-542">Note that the `SetNextSerialNo` and `GetNextSerialNo` static methods are invoked on the class whereas the `GetSerialNo` instance method is invoked on instances of the class.</span></span>

#### <a name="virtual-override-and-abstract-methods"></a><span data-ttu-id="bbaec-543">虛擬、覆寫及抽象方法</span><span class="sxs-lookup"><span data-stu-id="bbaec-543">Virtual, override, and abstract methods</span></span>

<span data-ttu-id="bbaec-544">當執行個體方法宣告包含 `virtual` 修飾詞時，該方法即稱為「虛擬方法」。</span><span class="sxs-lookup"><span data-stu-id="bbaec-544">When an instance method declaration includes a `virtual` modifier, the method is said to be a ***virtual method***.</span></span> <span data-ttu-id="bbaec-545">當沒有`virtual`任何修飾詞時，方法就稱為***非虛擬方法***。</span><span class="sxs-lookup"><span data-stu-id="bbaec-545">When no `virtual` modifier is present, the method is said to be a ***non-virtual method***.</span></span>

<span data-ttu-id="bbaec-546">叫用虛擬方法時，叫用所針對之執行個體的「執行階段型別」會決定要叫用的實際方法實作。</span><span class="sxs-lookup"><span data-stu-id="bbaec-546">When a virtual method is invoked, the ***run-time type*** of the instance for which that invocation takes place determines the actual method implementation to invoke.</span></span> <span data-ttu-id="bbaec-547">在非虛擬方法叫用中，決定因素則是執行個體的「編譯階段型別」。</span><span class="sxs-lookup"><span data-stu-id="bbaec-547">In a nonvirtual method invocation, the ***compile-time type*** of the instance is the determining factor.</span></span>

<span data-ttu-id="bbaec-548">在衍生類別中可以「覆寫」虛擬方法。</span><span class="sxs-lookup"><span data-stu-id="bbaec-548">A virtual method can be ***overridden*** in a derived class.</span></span> <span data-ttu-id="bbaec-549">當實例方法宣告包含`override`修飾詞時，此方法會使用相同的簽章來覆寫繼承的虛擬方法。</span><span class="sxs-lookup"><span data-stu-id="bbaec-549">When an instance method declaration includes an `override` modifier, the method overrides an inherited virtual method with the same signature.</span></span> <span data-ttu-id="bbaec-550">虛擬方法宣告會導入新的方法，而覆寫方法宣告則是會提供現有已繼承之虛擬方法的新實作，來將該方法特製化。</span><span class="sxs-lookup"><span data-stu-id="bbaec-550">Whereas a virtual method declaration introduces a new method, an override method declaration specializes an existing inherited virtual method by providing a new implementation of that method.</span></span>

<span data-ttu-id="bbaec-551">***抽象***方法是不執行的虛擬方法。</span><span class="sxs-lookup"><span data-stu-id="bbaec-551">An ***abstract*** method is a virtual method with no implementation.</span></span> <span data-ttu-id="bbaec-552">抽象方法是以`abstract`修飾詞宣告，而且只允許在`abstract`也宣告的類別中使用。</span><span class="sxs-lookup"><span data-stu-id="bbaec-552">An abstract method is declared with the `abstract` modifier and is permitted only in a class that is also declared `abstract`.</span></span> <span data-ttu-id="bbaec-553">抽象方法必須在每個非抽象的衍生類別中被覆寫。</span><span class="sxs-lookup"><span data-stu-id="bbaec-553">An abstract method must be overridden in every non-abstract derived class.</span></span>

<span data-ttu-id="bbaec-554">下列範例會宣告一個抽象類別 `Expression` 和三個衍生的類別 `Constant`、`VariableReference` 及 `Operation`，前者代表一個運算式樹狀架構節點，後者則會實作常數、變數參考及算數運算式的運算式樹狀架構節點。</span><span class="sxs-lookup"><span data-stu-id="bbaec-554">The following example declares an abstract class, `Expression`, which represents an expression tree node, and three derived classes, `Constant`, `VariableReference`, and `Operation`, which implement expression tree nodes for constants, variable references, and arithmetic operations.</span></span> <span data-ttu-id="bbaec-555">（這類似于，但不會與[運算式樹狀架構類型](types.md#expression-tree-types)中引進的運算式樹狀架構類型混淆）。</span><span class="sxs-lookup"><span data-stu-id="bbaec-555">(This is similar to, but not to be confused with the expression tree types introduced in [Expression tree types](types.md#expression-tree-types)).</span></span>

```csharp
using System;
using System.Collections;

public abstract class Expression
{
    public abstract double Evaluate(Hashtable vars);
}

public class Constant: Expression
{
    double value;

    public Constant(double value) {
        this.value = value;
    }

    public override double Evaluate(Hashtable vars) {
        return value;
    }
}

public class VariableReference: Expression
{
    string name;

    public VariableReference(string name) {
        this.name = name;
    }

    public override double Evaluate(Hashtable vars) {
        object value = vars[name];
        if (value == null) {
            throw new Exception("Unknown variable: " + name);
        }
        return Convert.ToDouble(value);
    }
}

public class Operation: Expression
{
    Expression left;
    char op;
    Expression right;

    public Operation(Expression left, char op, Expression right) {
        this.left = left;
        this.op = op;
        this.right = right;
    }

    public override double Evaluate(Hashtable vars) {
        double x = left.Evaluate(vars);
        double y = right.Evaluate(vars);
        switch (op) {
            case '+': return x + y;
            case '-': return x - y;
            case '*': return x * y;
            case '/': return x / y;
        }
        throw new Exception("Unknown operator");
    }
}
```
<span data-ttu-id="bbaec-556">先前的四個類別可用來建構算數運算式的模型。</span><span class="sxs-lookup"><span data-stu-id="bbaec-556">The previous four classes can be used to model arithmetic expressions.</span></span> <span data-ttu-id="bbaec-557">例如，在使用這些類別執行個體的情況下，可以將運算式 `x + 3` 表示如下。</span><span class="sxs-lookup"><span data-stu-id="bbaec-557">For example, using instances of these classes, the expression `x + 3` can be represented as follows.</span></span>

```csharp
Expression e = new Operation(
    new VariableReference("x"),
    '+',
    new Constant(3));
```
<span data-ttu-id="bbaec-558">系統會叫用 `Expression` 執行個體的 `Evaluate` 方法來評估指定的運算式並產生 `double` 值。</span><span class="sxs-lookup"><span data-stu-id="bbaec-558">The `Evaluate` method of an `Expression` instance is invoked to evaluate the given expression and produce a `double` value.</span></span> <span data-ttu-id="bbaec-559">方法會採用包含變數名稱（ `Hashtable`作為專案的索引鍵）和值（作為專案值）的引數。</span><span class="sxs-lookup"><span data-stu-id="bbaec-559">The method takes as an argument a `Hashtable` that contains variable names (as keys of the entries) and values (as values of the entries).</span></span> <span data-ttu-id="bbaec-560">`Evaluate`方法是虛擬抽象方法，這表示非抽象衍生類別必須覆寫它，以提供實際的實值。</span><span class="sxs-lookup"><span data-stu-id="bbaec-560">The `Evaluate` method is a virtual abstract method, meaning that non-abstract derived classes must override it to provide an actual implementation.</span></span>

<span data-ttu-id="bbaec-561">`Constant` 的 `Evaluate` 實作會直接傳回預存的常數。</span><span class="sxs-lookup"><span data-stu-id="bbaec-561">A `Constant`'s implementation of `Evaluate` simply returns the stored constant.</span></span> <span data-ttu-id="bbaec-562">`VariableReference`的執行會在 hashtable 中查詢變數名稱，並傳回產生的值。</span><span class="sxs-lookup"><span data-stu-id="bbaec-562">A `VariableReference`'s implementation looks up the variable name in the hashtable and returns the resulting value.</span></span> <span data-ttu-id="bbaec-563">`Operation` 的實作會先評估左邊和右邊的運算元 (透過以遞迴方式叫用其 `Evaluate` 方法)，然後才執行指定的算數運算。</span><span class="sxs-lookup"><span data-stu-id="bbaec-563">An `Operation`'s implementation first evaluates the left and right operands (by recursively invoking their `Evaluate` methods) and then performs the given arithmetic operation.</span></span>

<span data-ttu-id="bbaec-564">下列程式會使用 `Expression` 類別來評估不同 `x` 和 `y` 值的 `x * (y + 2)` 運算式。</span><span class="sxs-lookup"><span data-stu-id="bbaec-564">The following program uses the `Expression` classes to evaluate the expression `x * (y + 2)` for different values of `x` and `y`.</span></span>

```csharp
using System;
using System.Collections;

class Test
{
    static void Main() {
        Expression e = new Operation(
            new VariableReference("x"),
            '*',
            new Operation(
                new VariableReference("y"),
                '+',
                new Constant(2)
            )
        );
        Hashtable vars = new Hashtable();
        vars["x"] = 3;
        vars["y"] = 5;
        Console.WriteLine(e.Evaluate(vars));        // Outputs "21"
        vars["x"] = 1.5;
        vars["y"] = 9;
        Console.WriteLine(e.Evaluate(vars));        // Outputs "16.5"
    }
}
```

#### <a name="method-overloading"></a><span data-ttu-id="bbaec-565">方法多載</span><span class="sxs-lookup"><span data-stu-id="bbaec-565">Method overloading</span></span>

<span data-ttu-id="bbaec-566">方法「多載」可允許相同類別中的多個方法擁有相同的名稱，只要它們的簽章是唯一的即可。</span><span class="sxs-lookup"><span data-stu-id="bbaec-566">Method ***overloading*** permits multiple methods in the same class to have the same name as long as they have unique signatures.</span></span> <span data-ttu-id="bbaec-567">編譯多載方法的叫用時，編譯器會使用「多載解析」來判斷要叫用的特定方法。</span><span class="sxs-lookup"><span data-stu-id="bbaec-567">When compiling an invocation of an overloaded method, the compiler uses ***overload resolution*** to determine the specific method to invoke.</span></span> <span data-ttu-id="bbaec-568">多載解析會尋找一個與引數最相符的方法，或者，如果找不到任何一個最相符的方法，則會回報錯誤。</span><span class="sxs-lookup"><span data-stu-id="bbaec-568">Overload resolution finds the one method that best matches the arguments or reports an error if no single best match can be found.</span></span> <span data-ttu-id="bbaec-569">下列範例示範多載解析的實際運作情況。</span><span class="sxs-lookup"><span data-stu-id="bbaec-569">The following example shows overload resolution in effect.</span></span> <span data-ttu-id="bbaec-570">`Main` 方法中每項叫用的註解會顯示實際叫用的方法是哪一個。</span><span class="sxs-lookup"><span data-stu-id="bbaec-570">The comment for each invocation in the `Main` method shows which method is actually invoked.</span></span>

```csharp
class Test
{
    static void F() {
        Console.WriteLine("F()");
    }

    static void F(object x) {
        Console.WriteLine("F(object)");
    }

    static void F(int x) {
        Console.WriteLine("F(int)");
    }

    static void F(double x) {
        Console.WriteLine("F(double)");
    }

    static void F<T>(T x) {
        Console.WriteLine("F<T>(T)");
    }

    static void F(double x, double y) {
        Console.WriteLine("F(double, double)");
    }

    static void Main() {
        F();                 // Invokes F()
        F(1);                // Invokes F(int)
        F(1.0);              // Invokes F(double)
        F("abc");            // Invokes F(object)
        F((double)1);        // Invokes F(double)
        F((object)1);        // Invokes F(object)
        F<int>(1);           // Invokes F<T>(T)
        F(1, 1);             // Invokes F(double, double)
    }
}
```
<span data-ttu-id="bbaec-571">如範例所示，透過將引數明確轉換成確切的參數型別和 (或) 明確提供型別引數，即一律可以選取特定的方法。</span><span class="sxs-lookup"><span data-stu-id="bbaec-571">As shown by the example, a particular method can always be selected by explicitly casting the arguments to the exact parameter types and/or explicitly supplying type arguments.</span></span>

### <a name="other-function-members"></a><span data-ttu-id="bbaec-572">其他函式成員</span><span class="sxs-lookup"><span data-stu-id="bbaec-572">Other function members</span></span>

<span data-ttu-id="bbaec-573">包含可執行程式碼的成員統稱為類別的「函式成員」。</span><span class="sxs-lookup"><span data-stu-id="bbaec-573">Members that contain executable code are collectively known as the ***function members*** of a class.</span></span> <span data-ttu-id="bbaec-574">上節中所述的方法是主要的函式成員類型。</span><span class="sxs-lookup"><span data-stu-id="bbaec-574">The preceding section describes methods, which are the primary kind of function members.</span></span> <span data-ttu-id="bbaec-575">本節描述支援的其他函數成員類型：「程式C#」、「屬性」、「索引子」、「事件」、「運算子」和「析構函數」。</span><span class="sxs-lookup"><span data-stu-id="bbaec-575">This section describes the other kinds of function members supported by C#: constructors, properties, indexers, events, operators, and destructors.</span></span>

<span data-ttu-id="bbaec-576">下列程式碼示範名`List<T>`為的泛型類別，它會執行物件的可成長清單。</span><span class="sxs-lookup"><span data-stu-id="bbaec-576">The following code shows a generic class called `List<T>`, which implements a growable list of objects.</span></span> <span data-ttu-id="bbaec-577">此類別包含數個最常見的函式成員類型。</span><span class="sxs-lookup"><span data-stu-id="bbaec-577">The class contains several examples of the most common kinds of function members.</span></span>


```csharp
public class List<T> {
    // Constant...
    const int defaultCapacity = 4;

    // Fields...
    T[] items;
    int count;

    // Constructors...
    public List(int capacity = defaultCapacity) {
        items = new T[capacity];
    }

    // Properties...
    public int Count {
        get { return count; }
    }
    public int Capacity {
        get {
            return items.Length;
        }
        set {
            if (value < count) value = count;
            if (value != items.Length) {
                T[] newItems = new T[value];
                Array.Copy(items, 0, newItems, 0, count);
                items = newItems;
            }
        }
    }

    // Indexer...
    public T this[int index] {
        get {
            return items[index];
        }
        set {
            items[index] = value;
            OnChanged();
        }
    }

    // Methods...
    public void Add(T item) {
        if (count == Capacity) Capacity = count * 2;
        items[count] = item;
        count++;
        OnChanged();
    }
    protected virtual void OnChanged() {
        if (Changed != null) Changed(this, EventArgs.Empty);
    }
    public override bool Equals(object other) {
        return Equals(this, other as List<T>);
    }
    static bool Equals(List<T> a, List<T> b) {
        if (a == null) return b == null;
        if (b == null || a.count != b.count) return false;
        for (int i = 0; i < a.count; i++) {
            if (!object.Equals(a.items[i], b.items[i])) {
                return false;
            }
        }
        return true;
    }

    // Event...
    public event EventHandler Changed;

    // Operators...
    public static bool operator ==(List<T> a, List<T> b) {
        return Equals(a, b);
    }
    public static bool operator !=(List<T> a, List<T> b) {
        return !Equals(a, b);
    }
}
```

#### <a name="constructors"></a><span data-ttu-id="bbaec-578">建構函式</span><span class="sxs-lookup"><span data-stu-id="bbaec-578">Constructors</span></span>

<span data-ttu-id="bbaec-579">C# 同時支援執行個體建構函式和靜態建構函式。</span><span class="sxs-lookup"><span data-stu-id="bbaec-579">C# supports both instance and static constructors.</span></span> <span data-ttu-id="bbaec-580">「執行個體建構函式」是實作將類別執行個體初始化所需之動作的成員。</span><span class="sxs-lookup"><span data-stu-id="bbaec-580">An ***instance constructor*** is a member that implements the actions required to initialize an instance of a class.</span></span> <span data-ttu-id="bbaec-581">「靜態建構函式」是實作第一次將類別本身載入時將其初始化所需之動作的成員。</span><span class="sxs-lookup"><span data-stu-id="bbaec-581">A ***static constructor*** is a member that implements the actions required to initialize a class itself when it is first loaded.</span></span>

<span data-ttu-id="bbaec-582">建構函式的宣告方式與方法類似，但不含傳回型別且名稱會與包含它的類別相同。</span><span class="sxs-lookup"><span data-stu-id="bbaec-582">A constructor is declared like a method with no return type and the same name as the containing class.</span></span> <span data-ttu-id="bbaec-583">如果函式宣告包含`static`修飾詞，則會宣告靜態的函式。</span><span class="sxs-lookup"><span data-stu-id="bbaec-583">If a constructor declaration includes a `static` modifier, it declares a static constructor.</span></span> <span data-ttu-id="bbaec-584">否則，所宣告的會是執行個體建構函式。</span><span class="sxs-lookup"><span data-stu-id="bbaec-584">Otherwise, it declares an instance constructor.</span></span>

<span data-ttu-id="bbaec-585">實例的函式可以多載。</span><span class="sxs-lookup"><span data-stu-id="bbaec-585">Instance constructors can be overloaded.</span></span> <span data-ttu-id="bbaec-586">例如，`List<T>` 類別會宣告兩個執行個體建構函式，一個沒有任何參數，另一個會採用 `int` 參數。</span><span class="sxs-lookup"><span data-stu-id="bbaec-586">For example, the `List<T>` class declares two instance constructors, one with no parameters and one that takes an `int` parameter.</span></span> <span data-ttu-id="bbaec-587">叫用執行個體建構函式時，是使用 `new` 運算子來叫用。</span><span class="sxs-lookup"><span data-stu-id="bbaec-587">Instance constructors are invoked using the `new` operator.</span></span> <span data-ttu-id="bbaec-588">下列語句會使用`List<string>` `List`類別的每個函式來配置兩個實例。</span><span class="sxs-lookup"><span data-stu-id="bbaec-588">The following statements allocate two `List<string>` instances using each of the constructors of the `List` class.</span></span>

```csharp
List<string> list1 = new List<string>();
List<string> list2 = new List<string>(10);
```
<span data-ttu-id="bbaec-589">與其他成員不同，類別並不會繼承執行個體建構函式，而且除了類別中實際宣告的執行個體建構函式以外，類別即不會再有任何執行個體建構函式。</span><span class="sxs-lookup"><span data-stu-id="bbaec-589">Unlike other members, instance constructors are not inherited, and a class has no instance constructors other than those actually declared in the class.</span></span> <span data-ttu-id="bbaec-590">如果沒有為類別提供任何執行個體建構函式，則會自動提供一個沒有任何參數的空建構函式。</span><span class="sxs-lookup"><span data-stu-id="bbaec-590">If no instance constructor is supplied for a class, then an empty one with no parameters is automatically provided.</span></span>

#### <a name="properties"></a><span data-ttu-id="bbaec-591">屬性</span><span class="sxs-lookup"><span data-stu-id="bbaec-591">Properties</span></span>

<span data-ttu-id="bbaec-592">「屬性」是欄位的自然延伸。</span><span class="sxs-lookup"><span data-stu-id="bbaec-592">***Properties*** are a natural extension of fields.</span></span> <span data-ttu-id="bbaec-593">兩者都是具有關聯型別的具名成員，並且用來存取欄位和屬性的語法是相同的。</span><span class="sxs-lookup"><span data-stu-id="bbaec-593">Both are named members with associated types, and the syntax for accessing fields and properties is the same.</span></span> <span data-ttu-id="bbaec-594">不過，與欄位不同的是，屬性並不會指示儲存位置。</span><span class="sxs-lookup"><span data-stu-id="bbaec-594">However, unlike fields, properties do not denote storage locations.</span></span> <span data-ttu-id="bbaec-595">取而代之的是，屬性會有「存取子」，這些存取子會指定讀取或寫入其值時要執行的陳述式。</span><span class="sxs-lookup"><span data-stu-id="bbaec-595">Instead, properties have ***accessors*** that specify the statements to be executed when their values are read or written.</span></span>

<span data-ttu-id="bbaec-596">屬性的宣告方式與欄位類似，不同之處在于宣告的結尾`get`是以存取子和`set` （或）在分隔符號`{`之間`}`寫入的存取子，而不是以分號結尾。</span><span class="sxs-lookup"><span data-stu-id="bbaec-596">A property is declared like a field, except that the declaration ends with a `get` accessor and/or a `set` accessor written between the delimiters `{` and `}` instead of ending in a semicolon.</span></span> <span data-ttu-id="bbaec-597">同時`get`具有存取子`set`和存取子的屬性是***讀寫屬性*** `get` ，只有存取子的屬性是***唯讀屬性***，而`set`只有存取子的屬性是***僅限寫入的屬性***。</span><span class="sxs-lookup"><span data-stu-id="bbaec-597">A property that has both a `get` accessor and a `set` accessor is a ***read-write property***, a property that has only a `get` accessor is a ***read-only property***, and a property that has only a `set` accessor is a ***write-only property***.</span></span>

<span data-ttu-id="bbaec-598">`get`存取子會對應至具有屬性類型傳回值的無參數方法。</span><span class="sxs-lookup"><span data-stu-id="bbaec-598">A `get` accessor corresponds to a parameterless method with a return value of the property type.</span></span> <span data-ttu-id="bbaec-599">除了指派的目標以外，在運算式中參考屬性時，會叫用屬性的`get`存取子來計算屬性的值。</span><span class="sxs-lookup"><span data-stu-id="bbaec-599">Except as the target of an assignment, when a property is referenced in an expression, the `get` accessor of the property is invoked to compute the value of the property.</span></span>

<span data-ttu-id="bbaec-600">存取子會對應至方法，其中具有名為`value`的單一參數，而且沒有傳回型別。 `set`</span><span class="sxs-lookup"><span data-stu-id="bbaec-600">A `set` accessor corresponds to a method with a single parameter named `value` and no return type.</span></span> <span data-ttu-id="bbaec-601">當屬性當做指派的目標或當做`++`或`--`的運算元來參考時， `set`會使用提供新值的引數來叫用存取子。</span><span class="sxs-lookup"><span data-stu-id="bbaec-601">When a property is referenced as the target of an assignment or as the operand of `++` or `--`, the `set` accessor is invoked with an argument that provides the new value.</span></span>

<span data-ttu-id="bbaec-602">`List<T>` 類別會宣告 `Count` 和 `Capacity` 這兩個屬性，它們分別是唯讀屬性和讀寫屬性。</span><span class="sxs-lookup"><span data-stu-id="bbaec-602">The `List<T>` class declares two properties, `Count` and `Capacity`, which are read-only and read-write, respectively.</span></span> <span data-ttu-id="bbaec-603">以下是這些屬性的用法範例。</span><span class="sxs-lookup"><span data-stu-id="bbaec-603">The following is an example of use of these properties.</span></span>

```csharp
List<string> names = new List<string>();
names.Capacity = 100;            // Invokes set accessor
int i = names.Count;             // Invokes get accessor
int j = names.Capacity;          // Invokes get accessor
```
<span data-ttu-id="bbaec-604">與欄位和方法類似，C# 也同時支援執行個體屬性和靜態屬性。</span><span class="sxs-lookup"><span data-stu-id="bbaec-604">Similar to fields and methods, C# supports both instance properties and static properties.</span></span> <span data-ttu-id="bbaec-605">靜態屬性會以`static`修飾詞宣告，而實例屬性則不會宣告。</span><span class="sxs-lookup"><span data-stu-id="bbaec-605">Static properties are declared with the `static` modifier, and instance properties are declared without it.</span></span>

<span data-ttu-id="bbaec-606">屬性的存取子可以是虛擬的。</span><span class="sxs-lookup"><span data-stu-id="bbaec-606">The accessor(s) of a property can be virtual.</span></span> <span data-ttu-id="bbaec-607">當屬性宣告包含 `virtual`、`abstract` 或 `override` 修飾詞時，會套用至該屬性的存取子。</span><span class="sxs-lookup"><span data-stu-id="bbaec-607">When a property declaration includes a `virtual`, `abstract`, or `override` modifier, it applies to the accessor(s) of the property.</span></span>

#### <a name="indexers"></a><span data-ttu-id="bbaec-608">索引子</span><span class="sxs-lookup"><span data-stu-id="bbaec-608">Indexers</span></span>

<span data-ttu-id="bbaec-609">「索引子」是可讓物件以和陣列相同的方式進行索引編製的成員。</span><span class="sxs-lookup"><span data-stu-id="bbaec-609">An ***indexer*** is a member that enables objects to be indexed in the same way as an array.</span></span> <span data-ttu-id="bbaec-610">索引子的宣告方式與屬性類似，不同之處在於成員的名稱是 `this`，後面接著在 `[` 與 `]` 分隔符號之間撰寫的參數清單。</span><span class="sxs-lookup"><span data-stu-id="bbaec-610">An indexer is declared like a property except that the name of the member is `this` followed by a parameter list written between the delimiters `[` and `]`.</span></span> <span data-ttu-id="bbaec-611">索引子的存取子中會提供參數。</span><span class="sxs-lookup"><span data-stu-id="bbaec-611">The parameters are available in the accessor(s) of the indexer.</span></span> <span data-ttu-id="bbaec-612">與屬性類似，索引子可以是讀寫、唯讀及唯寫的，而索引子的存取子可以是虛擬的。</span><span class="sxs-lookup"><span data-stu-id="bbaec-612">Similar to properties, indexers can be read-write, read-only, and write-only, and the accessor(s) of an indexer can be virtual.</span></span>

<span data-ttu-id="bbaec-613">`List` 類別會宣告一個採用 `int` 參數的單一讀寫索引子。</span><span class="sxs-lookup"><span data-stu-id="bbaec-613">The `List` class declares a single read-write indexer that takes an `int` parameter.</span></span> <span data-ttu-id="bbaec-614">此索引子使得系統能夠以 `int` 值編製 `List` 執行個體的索引。</span><span class="sxs-lookup"><span data-stu-id="bbaec-614">The indexer makes it possible to index `List` instances with `int` values.</span></span> <span data-ttu-id="bbaec-615">例如：</span><span class="sxs-lookup"><span data-stu-id="bbaec-615">For example</span></span>

```csharp
List<string> names = new List<string>();
names.Add("Liz");
names.Add("Martha");
names.Add("Beth");
for (int i = 0; i < names.Count; i++) {
    string s = names[i];
    names[i] = s.ToUpper();
}
```
<span data-ttu-id="bbaec-616">索引子可被多載，這意謂著類別可以宣告多個索引子，只要其參數的號碼或型別不同即可。</span><span class="sxs-lookup"><span data-stu-id="bbaec-616">Indexers can be overloaded, meaning that a class can declare multiple indexers as long as the number or types of their parameters differ.</span></span>

#### <a name="events"></a><span data-ttu-id="bbaec-617">事件</span><span class="sxs-lookup"><span data-stu-id="bbaec-617">Events</span></span>

<span data-ttu-id="bbaec-618">「事件」是可讓類別或物件提供通知的成員。</span><span class="sxs-lookup"><span data-stu-id="bbaec-618">An ***event*** is a member that enables a class or object to provide notifications.</span></span> <span data-ttu-id="bbaec-619">事件的宣告方式與欄位類似，不同之處在于宣告`event`包含關鍵字，而類型必須是委派類型。</span><span class="sxs-lookup"><span data-stu-id="bbaec-619">An event is declared like a field except that the declaration includes an `event` keyword and the type must be a delegate type.</span></span>

<span data-ttu-id="bbaec-620">在宣告事件成員的類別內，事件的行為與委派型別的欄位相同 (前提是該事件不是抽象事件且未宣告存取子)。</span><span class="sxs-lookup"><span data-stu-id="bbaec-620">Within a class that declares an event member, the event behaves just like a field of a delegate type (provided the event is not abstract and does not declare accessors).</span></span> <span data-ttu-id="bbaec-621">欄位會儲存對委派項目的參考，該委派項目代表已新增到事件中的事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="bbaec-621">The field stores a reference to a delegate that represents the event handlers that have been added to the event.</span></span> <span data-ttu-id="bbaec-622">如果沒有任何事件控制碼存在，則欄位`null`為。</span><span class="sxs-lookup"><span data-stu-id="bbaec-622">If no event handles are present, the field is `null`.</span></span>

<span data-ttu-id="bbaec-623">`List<T>` 類別會宣告一個名為 `Changed` 的單一事件成員，此成員會指出新項目已新增到清單中。</span><span class="sxs-lookup"><span data-stu-id="bbaec-623">The `List<T>` class declares a single event member called `Changed`, which indicates that a new item has been added to the list.</span></span> <span data-ttu-id="bbaec-624">事件是由虛擬方法引發，它會先檢查事件是否為`null` （表示沒有處理常式存在）。 `OnChanged` `Changed`</span><span class="sxs-lookup"><span data-stu-id="bbaec-624">The `Changed` event is raised by the `OnChanged` virtual method, which first checks whether the event is `null` (meaning that no handlers are present).</span></span> <span data-ttu-id="bbaec-625">引發事件的概念完全等同於叫用該事件所代表的委派項目，因此，就引發事件而言，並沒有任何特殊的語言建構。</span><span class="sxs-lookup"><span data-stu-id="bbaec-625">The notion of raising an event is precisely equivalent to invoking the delegate represented by the event—thus, there are no special language constructs for raising events.</span></span>

<span data-ttu-id="bbaec-626">用戶端是透過「事件處理常式」來對事件進行反應。</span><span class="sxs-lookup"><span data-stu-id="bbaec-626">Clients react to events through ***event handlers***.</span></span> <span data-ttu-id="bbaec-627">附加事件處理常式時，是使用 `+=` 運算子，移除時，則是使用 `-=` 運算子。</span><span class="sxs-lookup"><span data-stu-id="bbaec-627">Event handlers are attached using the `+=` operator and removed using the `-=` operator.</span></span> <span data-ttu-id="bbaec-628">下列範例會將事件處理常式附加到 `List<string>` 的 `Changed` 事件。</span><span class="sxs-lookup"><span data-stu-id="bbaec-628">The following example attaches an event handler to the `Changed` event of a `List<string>`.</span></span>

```csharp
using System;

class Test
{
    static int changeCount;

    static void ListChanged(object sender, EventArgs e) {
        changeCount++;
    }

    static void Main() {
        List<string> names = new List<string>();
        names.Changed += new EventHandler(ListChanged);
        names.Add("Liz");
        names.Add("Martha");
        names.Add("Beth");
        Console.WriteLine(changeCount);        // Outputs "3"
    }
}
```
<span data-ttu-id="bbaec-629">針對需要控制事件之基礎儲存的進階案例，事件宣告可以明確提供 `add` 和 `remove` 存取子，這有些類似於屬性的 `set` 存取子。</span><span class="sxs-lookup"><span data-stu-id="bbaec-629">For advanced scenarios where control of the underlying storage of an event is desired, an event declaration can explicitly provide `add` and `remove` accessors, which are somewhat similar to the `set` accessor of a property.</span></span>

#### <a name="operators"></a><span data-ttu-id="bbaec-630">運算子</span><span class="sxs-lookup"><span data-stu-id="bbaec-630">Operators</span></span>

<span data-ttu-id="bbaec-631">「運算子」是定義將特定運算式運算子套用到類別執行個體之意義的成員。</span><span class="sxs-lookup"><span data-stu-id="bbaec-631">An ***operator*** is a member that defines the meaning of applying a particular expression operator to instances of a class.</span></span> <span data-ttu-id="bbaec-632">可定義的運算子有三種：一元運算子、二元運算子及轉換運算子。</span><span class="sxs-lookup"><span data-stu-id="bbaec-632">Three kinds of operators can be defined: unary operators, binary operators, and conversion operators.</span></span> <span data-ttu-id="bbaec-633">所有運算子都必須宣告為 `public` 和 `static`。</span><span class="sxs-lookup"><span data-stu-id="bbaec-633">All operators must be declared as `public` and `static`.</span></span>

<span data-ttu-id="bbaec-634">`List<T>` 類別會宣告 `operator==` 和 `operator!=` 這兩個運算子，藉此賦予將這些運算子套用到 `List` 執行個體的運算式新意義。</span><span class="sxs-lookup"><span data-stu-id="bbaec-634">The `List<T>` class declares two operators, `operator==` and `operator!=`, and thus gives new meaning to expressions that apply those operators to `List` instances.</span></span> <span data-ttu-id="bbaec-635">具體而言，運算子會使用其`List<T>` `Equals`方法來比較所包含的每個物件，以定義兩個實例是否相等。</span><span class="sxs-lookup"><span data-stu-id="bbaec-635">Specifically, the operators define equality of two `List<T>` instances as comparing each of the contained objects using their `Equals` methods.</span></span> <span data-ttu-id="bbaec-636">下列範例會使用 `==` 運算子來比較兩個 `List<int>` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="bbaec-636">The following example uses the `==` operator to compare two `List<int>` instances.</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        List<int> a = new List<int>();
        a.Add(1);
        a.Add(2);
        List<int> b = new List<int>();
        b.Add(1);
        b.Add(2);
        Console.WriteLine(a == b);        // Outputs "True"
        b.Add(3);
        Console.WriteLine(a == b);        // Outputs "False"
    }
}
```

<span data-ttu-id="bbaec-637">第一個 `Console.WriteLine` 會輸出 `True`，因為兩個清單所包含物件的數目相同、值相同且順序相同。</span><span class="sxs-lookup"><span data-stu-id="bbaec-637">The first `Console.WriteLine` outputs `True` because the two lists contain the same number of objects with the same values in the same order.</span></span> <span data-ttu-id="bbaec-638">如果 `List<T>` 並未定義 `operator==`，則第一個 `Console.WriteLine` 所輸出的會是 `False`，因為 `a` 和 `b` 參考不同的 `List<int>` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="bbaec-638">Had `List<T>` not defined `operator==`, the first `Console.WriteLine` would have output `False` because `a` and `b` reference different `List<int>` instances.</span></span>

#### <a name="destructors"></a><span data-ttu-id="bbaec-639">解構函式</span><span class="sxs-lookup"><span data-stu-id="bbaec-639">Destructors</span></span>

<span data-ttu-id="bbaec-640">「***析構函***式」是一個成員，它會執行銷毀類別的實例所需的動作。</span><span class="sxs-lookup"><span data-stu-id="bbaec-640">A ***destructor*** is a member that implements the actions required to destruct an instance of a class.</span></span> <span data-ttu-id="bbaec-641">析構函數不能有參數，它們不能有存取範圍修飾詞，也不能明確叫用。</span><span class="sxs-lookup"><span data-stu-id="bbaec-641">Destructors cannot have parameters, they cannot have accessibility modifiers, and they cannot be invoked explicitly.</span></span> <span data-ttu-id="bbaec-642">實例的析構函式會在垃圾收集期間自動叫用。</span><span class="sxs-lookup"><span data-stu-id="bbaec-642">The destructor for an instance is invoked automatically during garbage collection.</span></span>

<span data-ttu-id="bbaec-643">垃圾收集行程允許寬緯度來決定何時收集物件和執行析構函數。</span><span class="sxs-lookup"><span data-stu-id="bbaec-643">The garbage collector is allowed wide latitude in deciding when to collect objects and run destructors.</span></span> <span data-ttu-id="bbaec-644">具體而言，析構函式調用的時機不具決定性，而且可以在任何執行緒上執行析構函數。</span><span class="sxs-lookup"><span data-stu-id="bbaec-644">Specifically, the timing of destructor invocations is not deterministic, and destructors may be executed on any thread.</span></span> <span data-ttu-id="bbaec-645">基於這些理由和其他原因，只有在沒有其他解決方案可行的情況下，類別才應該執行析構函數。</span><span class="sxs-lookup"><span data-stu-id="bbaec-645">For these and other reasons, classes should implement destructors only when no other solutions are feasible.</span></span>

<span data-ttu-id="bbaec-646">`using` 陳述式提供較佳的物件解構方法。</span><span class="sxs-lookup"><span data-stu-id="bbaec-646">The `using` statement provides a better approach to object destruction.</span></span>

## <a name="structs"></a><span data-ttu-id="bbaec-647">結構</span><span class="sxs-lookup"><span data-stu-id="bbaec-647">Structs</span></span>

<span data-ttu-id="bbaec-648">和類別一樣，***結構***是可包含資料成員和函式成員的資料結構，但不同於類別，結構是實值型別，不需要堆積配置。</span><span class="sxs-lookup"><span data-stu-id="bbaec-648">Like classes, ***structs*** are data structures that can contain data members and function members, but unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="bbaec-649">結構型別的變數直接儲存結構的資料，而類別型別的變數則儲存動態配置物件的參考。</span><span class="sxs-lookup"><span data-stu-id="bbaec-649">A variable of a struct type directly stores the data of the struct, whereas a variable of a class type stores a reference to a dynamically allocated object.</span></span> <span data-ttu-id="bbaec-650">結構型別不支援使用者指定的繼承，且所有結構型別都隱含地繼承自 `object` 型別。</span><span class="sxs-lookup"><span data-stu-id="bbaec-650">Struct types do not support user-specified inheritance, and all struct types implicitly inherit from type `object`.</span></span>

<span data-ttu-id="bbaec-651">結構特別適用於含有實值語意的小型資料結構。</span><span class="sxs-lookup"><span data-stu-id="bbaec-651">Structs are particularly useful for small data structures that have value semantics.</span></span> <span data-ttu-id="bbaec-652">複數、座標系統中的點或字典中的索引鍵/值組都是結構的良好範例。</span><span class="sxs-lookup"><span data-stu-id="bbaec-652">Complex numbers, points in a coordinate system, or key-value pairs in a dictionary are all good examples of structs.</span></span> <span data-ttu-id="bbaec-653">針對小型資料結構使用結構而不使用類別，在應用程式執行的記憶體配置數目上有很大的差別。</span><span class="sxs-lookup"><span data-stu-id="bbaec-653">The use of structs rather than classes for small data structures can make a large difference in the number of memory allocations an application performs.</span></span> <span data-ttu-id="bbaec-654">比方說，下列程式會建立並初始化 100 個點的陣列。</span><span class="sxs-lookup"><span data-stu-id="bbaec-654">For example, the following program creates and initializes an array of 100 points.</span></span> <span data-ttu-id="bbaec-655">使用 `Point` 做為類別，會具現化 101 個不同的物件 — 一個代表陣列，剩下的每個代表 100 個元素。</span><span class="sxs-lookup"><span data-stu-id="bbaec-655">With `Point` implemented as a class, 101 separate objects are instantiated—one for the array and one each for the 100 elements.</span></span>

```csharp
class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

class Test
{
    static void Main() {
        Point[] points = new Point[100];
        for (int i = 0; i < 100; i++) points[i] = new Point(i, i);
    }
}
```
<span data-ttu-id="bbaec-656">另一個方法是建立`Point`結構。</span><span class="sxs-lookup"><span data-stu-id="bbaec-656">An alternative is to make `Point` a struct.</span></span>

```csharp
struct Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
<span data-ttu-id="bbaec-657">現在，只有一個物件會具現化 — 代表陣列的物件 — 而 `Point` 執行個體會以內嵌方式儲存在陣列中。</span><span class="sxs-lookup"><span data-stu-id="bbaec-657">Now, only one object is instantiated—the one for the array—and the `Point` instances are stored in-line in the array.</span></span>

<span data-ttu-id="bbaec-658">結構建構函式使用 `new` 運算子叫用，但並不代表會配置記憶體。</span><span class="sxs-lookup"><span data-stu-id="bbaec-658">Struct constructors are invoked with the `new` operator, but that does not imply that memory is being allocated.</span></span> <span data-ttu-id="bbaec-659">結構建構函式只會傳回結構值本身 (通常是在堆疊上的暫存位置)，然後再視需要複製此值，而不是動態配置一個物件並傳回該物件的參考。</span><span class="sxs-lookup"><span data-stu-id="bbaec-659">Instead of dynamically allocating an object and returning a reference to it, a struct constructor simply returns the struct value itself (typically in a temporary location on the stack), and this value is then copied as necessary.</span></span>

<span data-ttu-id="bbaec-660">使用類別時，這兩種變數可以參考相同的物件，因此對其中一個變數進行的作業可能會影響另一個變數參考的物件。</span><span class="sxs-lookup"><span data-stu-id="bbaec-660">With classes, it is possible for two variables to reference the same object and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="bbaec-661">使用結構時，變數各有自己的資料複本，因此在某一個變數上進行的作業不可能會影響其他變數。</span><span class="sxs-lookup"><span data-stu-id="bbaec-661">With structs, the variables each have their own copy of the data, and it is not possible for operations on one to affect the other.</span></span> <span data-ttu-id="bbaec-662">例如，下列程式碼片段所產生的輸出取決於是否`Point`為類別或結構。</span><span class="sxs-lookup"><span data-stu-id="bbaec-662">For example, the output produced by the following code fragment depends on whether `Point` is a class or a struct.</span></span>

```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 20;
Console.WriteLine(b.x);
```
<span data-ttu-id="bbaec-663">如果`Point`是類別，則輸出`a`會是`20` ，而且`b`會參考相同的物件。</span><span class="sxs-lookup"><span data-stu-id="bbaec-663">If `Point` is a class, the output is `20` because `a` and `b` reference the same object.</span></span> <span data-ttu-id="bbaec-664">如果`Point`是結構，則輸出會是`10` ， `b`因為的指派`a`會建立值的複本，而此複本不會受到後續指派至`a.x`的影響。</span><span class="sxs-lookup"><span data-stu-id="bbaec-664">If `Point` is a struct, the output is `10` because the assignment of `a` to `b` creates a copy of the value, and this copy is unaffected by the subsequent assignment to `a.x`.</span></span>

<span data-ttu-id="bbaec-665">前一個範例會反白顯示結構的兩個限制。</span><span class="sxs-lookup"><span data-stu-id="bbaec-665">The previous example highlights two of the limitations of structs.</span></span> <span data-ttu-id="bbaec-666">首先，複製整個結構通常較複製物件參考沒有效率，因此，結構的指派和實值參數傳遞會比參考型別耗用更多資源。</span><span class="sxs-lookup"><span data-stu-id="bbaec-666">First, copying an entire struct is typically less efficient than copying an object reference, so assignment and value parameter passing can be more expensive with structs than with reference types.</span></span> <span data-ttu-id="bbaec-667">再者，除了 `ref` 和 `out` 參數外，不可能建立結構的參考，因此在許多情況下無法使用它們。</span><span class="sxs-lookup"><span data-stu-id="bbaec-667">Second, except for `ref` and `out` parameters, it is not possible to create references to structs, which rules out their usage in a number of situations.</span></span>

## <a name="arrays"></a><span data-ttu-id="bbaec-668">陣列</span><span class="sxs-lookup"><span data-stu-id="bbaec-668">Arrays</span></span>

<span data-ttu-id="bbaec-669">***陣列***是一種資料結構，其中包含一些可透過計算索引存取的變數。</span><span class="sxs-lookup"><span data-stu-id="bbaec-669">An ***array*** is a data structure that contains a number of variables that are accessed through computed indices.</span></span> <span data-ttu-id="bbaec-670">陣列中包含的變數 (也稱為陣列的***元素***) 屬於相同的型別，這種型別稱為陣列的***元素型別***。</span><span class="sxs-lookup"><span data-stu-id="bbaec-670">The variables contained in an array, also called the ***elements*** of the array, are all of the same type, and this type is called the ***element type*** of the array.</span></span>

<span data-ttu-id="bbaec-671">陣列型別是參考型別，而陣列變數的宣告只是預留空間給陣列執行個體的參考。</span><span class="sxs-lookup"><span data-stu-id="bbaec-671">Array types are reference types, and the declaration of an array variable simply sets aside space for a reference to an array instance.</span></span> <span data-ttu-id="bbaec-672">實際的陣列實例會使用`new`運算子，在執行時間以動態方式建立。</span><span class="sxs-lookup"><span data-stu-id="bbaec-672">Actual array instances are created dynamically at run-time using the `new` operator.</span></span> <span data-ttu-id="bbaec-673">作業會指定新陣列實例的長度，然後在實例的存留期內修正此問題。  `new`</span><span class="sxs-lookup"><span data-stu-id="bbaec-673">The `new` operation specifies the ***length*** of the new array instance, which is then fixed for the lifetime of the instance.</span></span> <span data-ttu-id="bbaec-674">陣列元素的索引範圍在 `0` 到 `Length - 1` 之間。</span><span class="sxs-lookup"><span data-stu-id="bbaec-674">The indices of the elements of an array range from `0` to `Length - 1`.</span></span> <span data-ttu-id="bbaec-675">`new` 運算子會自動將陣列的元素初始化為其預設值，例如，針對所有數值型別，此值為零，而針對所有參考型別，此值為 `null`。</span><span class="sxs-lookup"><span data-stu-id="bbaec-675">The `new` operator automatically initializes the elements of an array to their default value, which, for example, is zero for all numeric types and `null` for all reference types.</span></span>

<span data-ttu-id="bbaec-676">下列範例會建立 `int` 元素的陣列、初始化陣列，並印出陣列的內容。</span><span class="sxs-lookup"><span data-stu-id="bbaec-676">The following example creates an array of `int` elements, initializes the array, and prints out the contents of the array.</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        int[] a = new int[10];
        for (int i = 0; i < a.Length; i++) {
            a[i] = i * i;
        }
        for (int i = 0; i < a.Length; i++) {
            Console.WriteLine("a[{0}] = {1}", i, a[i]);
        }
    }
}
```
<span data-ttu-id="bbaec-677">這個範例會建立並操作***一維陣列***。</span><span class="sxs-lookup"><span data-stu-id="bbaec-677">This example creates and operates on a ***single-dimensional array***.</span></span> <span data-ttu-id="bbaec-678">C# 也支援***多維陣列***。</span><span class="sxs-lookup"><span data-stu-id="bbaec-678">C# also supports ***multi-dimensional arrays***.</span></span> <span data-ttu-id="bbaec-679">一個陣列型別的維度數目 (亦稱為陣列型別的***順位***)，是寫入陣列型別的方括弧之間的逗號數目加一。</span><span class="sxs-lookup"><span data-stu-id="bbaec-679">The number of dimensions of an array type, also known as the ***rank*** of the array type, is one plus the number of commas written between the square brackets of the array type.</span></span> <span data-ttu-id="bbaec-680">下列範例會配置一維、二維和三維陣列。</span><span class="sxs-lookup"><span data-stu-id="bbaec-680">The following example allocates a one-dimensional, a two-dimensional, and a three-dimensional array.</span></span>

```csharp
int[] a1 = new int[10];
int[,] a2 = new int[10, 5];
int[,,] a3 = new int[10, 5, 2];
```
<span data-ttu-id="bbaec-681">`a1` 陣列包含 10 個元素、`a2`陣列包含 50 (10 × 5) 個元素，`a3` 陣列包含 100 (10 × 5 × 2) 個元素。</span><span class="sxs-lookup"><span data-stu-id="bbaec-681">The `a1` array contains 10 elements, the `a2` array contains 50 (10 × 5) elements, and the `a3` array contains 100 (10 × 5 × 2) elements.</span></span>

<span data-ttu-id="bbaec-682">陣列的元素型別可以是任一型別，包括陣列型別。</span><span class="sxs-lookup"><span data-stu-id="bbaec-682">The element type of an array can be any type, including an array type.</span></span> <span data-ttu-id="bbaec-683">包含陣列型別之元素的陣列有時稱為***不規則陣列***，因為元素陣列的長度不需完全相同。</span><span class="sxs-lookup"><span data-stu-id="bbaec-683">An array with elements of an array type is sometimes called a ***jagged array*** because the lengths of the element arrays do not all have to be the same.</span></span> <span data-ttu-id="bbaec-684">下列範例會配置一個 `int` 型別的陣列：</span><span class="sxs-lookup"><span data-stu-id="bbaec-684">The following example allocates an array of arrays of `int`:</span></span>

```csharp
int[][] a = new int[3][];
a[0] = new int[10];
a[1] = new int[5];
a[2] = new int[20];
```
<span data-ttu-id="bbaec-685">第一行建立包含三個元素的陣列，每個元素的型別均為 `int[]`，每個元素的初始值均為 `null`。</span><span class="sxs-lookup"><span data-stu-id="bbaec-685">The first line creates an array with three elements, each of type `int[]` and each with an initial value of `null`.</span></span> <span data-ttu-id="bbaec-686">接下來的幾行則使用不同長度的個別陣列執行個體的參考初始化三個元素。</span><span class="sxs-lookup"><span data-stu-id="bbaec-686">The subsequent lines then initialize the three elements with references to individual array instances of varying lengths.</span></span>

<span data-ttu-id="bbaec-687">運算子允許使用***陣列初始化***運算式來指定陣列元素的初始值，這是在分隔符號`{`和`}`之間寫入的運算式清單。 `new`</span><span class="sxs-lookup"><span data-stu-id="bbaec-687">The `new` operator permits the initial values of the array elements to be specified using an ***array initializer***, which is a list of expressions written between the delimiters `{` and `}`.</span></span> <span data-ttu-id="bbaec-688">下列範例使用三個元素配置並初始化 `int[]`。</span><span class="sxs-lookup"><span data-stu-id="bbaec-688">The following example allocates and initializes an `int[]` with three elements.</span></span>

```csharp
int[] a = new int[] {1, 2, 3};
```
<span data-ttu-id="bbaec-689">請注意，陣列的長度是從和`{` `}`之間的運算式數目推斷而來。</span><span class="sxs-lookup"><span data-stu-id="bbaec-689">Note that the length of the array is inferred from the number of expressions between `{` and `}`.</span></span> <span data-ttu-id="bbaec-690">本機變數與欄位宣告可以進一步縮短，就不需要重新敘述陣列型別。</span><span class="sxs-lookup"><span data-stu-id="bbaec-690">Local variable and field declarations can be shortened further such that the array type does not have to be restated.</span></span>

```csharp
int[] a = {1, 2, 3};
```
<span data-ttu-id="bbaec-691">先前的兩個範例等同於下列範例：</span><span class="sxs-lookup"><span data-stu-id="bbaec-691">Both of the previous examples are equivalent to the following:</span></span>

```csharp
int[] t = new int[3];
t[0] = 1;
t[1] = 2;
t[2] = 3;
int[] a = t;
```
## <a name="interfaces"></a><span data-ttu-id="bbaec-692">介面</span><span class="sxs-lookup"><span data-stu-id="bbaec-692">Interfaces</span></span>

<span data-ttu-id="bbaec-693">「介面」定義可由類別和結構實作的合約。</span><span class="sxs-lookup"><span data-stu-id="bbaec-693">An ***interface*** defines a contract that can be implemented by classes and structs.</span></span> <span data-ttu-id="bbaec-694">介面可以包含方法、屬性、事件和索引子。</span><span class="sxs-lookup"><span data-stu-id="bbaec-694">An interface can contain methods, properties, events, and indexers.</span></span> <span data-ttu-id="bbaec-695">介面不提供它所定義之成員的實作 (它只會指定必須由類別提供的成員或實作介面的結構)。</span><span class="sxs-lookup"><span data-stu-id="bbaec-695">An interface does not provide implementations of the members it defines—it merely specifies the members that must be supplied by classes or structs that implement the interface.</span></span>

<span data-ttu-id="bbaec-696">介面可以採用「多重繼承」。</span><span class="sxs-lookup"><span data-stu-id="bbaec-696">Interfaces may employ ***multiple inheritance***.</span></span> <span data-ttu-id="bbaec-697">在下列範例中，介面 `IComboBox` 同時繼承自 `ITextBox` 和 `IListBox`。</span><span class="sxs-lookup"><span data-stu-id="bbaec-697">In the following example, the interface `IComboBox` inherits from both `ITextBox` and `IListBox`.</span></span>

```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

interface IComboBox: ITextBox, IListBox {}
```
<span data-ttu-id="bbaec-698">類別和結構可以實作多個介面。</span><span class="sxs-lookup"><span data-stu-id="bbaec-698">Classes and structs can implement multiple interfaces.</span></span> <span data-ttu-id="bbaec-699">在下列範例中，類別 `EditBox` 可以實作 `IControl` 與 `IDataBound` 兩者。</span><span class="sxs-lookup"><span data-stu-id="bbaec-699">In the following example, the class `EditBox` implements both `IControl` and `IDataBound`.</span></span>

```csharp
interface IDataBound
{
    void Bind(Binder b);
}

public class EditBox: IControl, IDataBound
{
    public void Paint() {...}
    public void Bind(Binder b) {...}
}
```
<span data-ttu-id="bbaec-700">當類別或結構實作特定介面時，可以將該類別或結構的執行個體隱含地轉換為該介面型別。</span><span class="sxs-lookup"><span data-stu-id="bbaec-700">When a class or struct implements a particular interface, instances of that class or struct can be implicitly converted to that interface type.</span></span> <span data-ttu-id="bbaec-701">例如</span><span class="sxs-lookup"><span data-stu-id="bbaec-701">For example</span></span>

```csharp
EditBox editBox = new EditBox();
IControl control = editBox;
IDataBound dataBound = editBox;
```
<span data-ttu-id="bbaec-702">在實作特定介面的執行個體無法確知的情況下，可以使用動態型別轉換。</span><span class="sxs-lookup"><span data-stu-id="bbaec-702">In cases where an instance is not statically known to implement a particular interface, dynamic type casts can be used.</span></span> <span data-ttu-id="bbaec-703">例如，下列語句會使用動態類型轉換來取得物件的`IControl`和`IDataBound`介面執行。</span><span class="sxs-lookup"><span data-stu-id="bbaec-703">For example, the following statements use dynamic type casts to obtain an object's `IControl` and `IDataBound` interface implementations.</span></span> <span data-ttu-id="bbaec-704">因為物件的實際類型為`EditBox`，所以轉換成功。</span><span class="sxs-lookup"><span data-stu-id="bbaec-704">Because the actual type of the object is `EditBox`, the casts succeed.</span></span>

```csharp
object obj = new EditBox();
IControl control = (IControl)obj;
IDataBound dataBound = (IDataBound)obj;
```
<span data-ttu-id="bbaec-705">在上一個`EditBox`類別中`Paint` ， `IControl` `IDataBound` `public`來自介面的方法和來自介面的方法，都是使用成員來執行。`Bind`</span><span class="sxs-lookup"><span data-stu-id="bbaec-705">In the previous `EditBox` class, the `Paint` method from the `IControl` interface and the `Bind` method from the `IDataBound` interface are implemented using `public` members.</span></span> <span data-ttu-id="bbaec-706">C#也支援***明確介面成員***的執行，可使用類別或結構來避免建立成員`public`。</span><span class="sxs-lookup"><span data-stu-id="bbaec-706">C# also supports ***explicit interface member implementations***, using which the class or struct can avoid making the members `public`.</span></span> <span data-ttu-id="bbaec-707">明確介面成員實作是使用介面成員完整名稱來撰寫。</span><span class="sxs-lookup"><span data-stu-id="bbaec-707">An explicit interface member implementation is written using the fully qualified interface member name.</span></span> <span data-ttu-id="bbaec-708">例如，`EditBox` 類別可以使用明確介面成員實作來實作 `IControl.Paint` 和 `IDataBound.Bind` 方法，如下所示。</span><span class="sxs-lookup"><span data-stu-id="bbaec-708">For example, the `EditBox` class could implement the `IControl.Paint` and `IDataBound.Bind` methods using explicit interface member implementations as follows.</span></span>

```csharp
public class EditBox: IControl, IDataBound
{
    void IControl.Paint() {...}
    void IDataBound.Bind(Binder b) {...}
}
```
<span data-ttu-id="bbaec-709">明確介面成員只能透過介面型別來存取。</span><span class="sxs-lookup"><span data-stu-id="bbaec-709">Explicit interface members can only be accessed via the interface type.</span></span> <span data-ttu-id="bbaec-710">例如，您只能藉由`IControl.Paint`先將`EditBox`參考`IControl`轉換`EditBox`為介面類別型，叫用前一個類別所提供的執行。</span><span class="sxs-lookup"><span data-stu-id="bbaec-710">For example, the implementation of `IControl.Paint` provided by the previous `EditBox` class can only be invoked by first converting the `EditBox` reference to the `IControl` interface type.</span></span>

```csharp
EditBox editBox = new EditBox();
editBox.Paint();                        // Error, no such method
IControl control = editBox;
control.Paint();                        // Ok
```

## <a name="enums"></a><span data-ttu-id="bbaec-711">列舉</span><span class="sxs-lookup"><span data-stu-id="bbaec-711">Enums</span></span>

<span data-ttu-id="bbaec-712">「列舉型別」是含一組具名常數的相異實值型別。</span><span class="sxs-lookup"><span data-stu-id="bbaec-712">An ***enum type*** is a distinct value type with a set of named constants.</span></span> <span data-ttu-id="bbaec-713">下列範例會宣告並`Color`使用名為的列舉型別搭配三個常數值`Green`、 `Blue` `Red`和。</span><span class="sxs-lookup"><span data-stu-id="bbaec-713">The following example declares and uses an enum type named `Color` with three constant values, `Red`, `Green`, and `Blue`.</span></span>

```csharp
using System;

enum Color
{
    Red,
    Green,
    Blue
}

class Test
{
    static void PrintColor(Color color) {
        switch (color) {
            case Color.Red:
                Console.WriteLine("Red");
                break;
            case Color.Green:
                Console.WriteLine("Green");
                break;
            case Color.Blue:
                Console.WriteLine("Blue");
                break;
            default:
                Console.WriteLine("Unknown color");
                break;
        }
    }

    static void Main() {
        Color c = Color.Red;
        PrintColor(c);
        PrintColor(Color.Blue);
    }
}
```
<span data-ttu-id="bbaec-714">每個列舉型別都有對應的整數型別，稱為列舉型別的***基礎型***別。</span><span class="sxs-lookup"><span data-stu-id="bbaec-714">Each enum type has a corresponding integral type called the ***underlying type*** of the enum type.</span></span> <span data-ttu-id="bbaec-715">未明確宣告基礎類型的列舉類型具有的`int`基礎類型。</span><span class="sxs-lookup"><span data-stu-id="bbaec-715">An enum type that does not explicitly declare an underlying type has an underlying type of `int`.</span></span> <span data-ttu-id="bbaec-716">列舉類型的儲存格式和可能值的範圍取決於其基礎類型。</span><span class="sxs-lookup"><span data-stu-id="bbaec-716">An enum type's storage format and range of possible values are determined by its underlying type.</span></span> <span data-ttu-id="bbaec-717">列舉型別可接受的值集合不受其列舉成員的限制。</span><span class="sxs-lookup"><span data-stu-id="bbaec-717">The set of values that an enum type can take on is not limited by its enum members.</span></span> <span data-ttu-id="bbaec-718">特別是，列舉之基礎類型的任何值都可以轉換成列舉類型，而且是該列舉類型的相異有效值。</span><span class="sxs-lookup"><span data-stu-id="bbaec-718">In particular, any value of the underlying type of an enum can be cast to the enum type and is a distinct valid value of that enum type.</span></span>

<span data-ttu-id="bbaec-719">下列範例會宣告名為`Alignment`且基礎類型為的`sbyte`列舉類型。</span><span class="sxs-lookup"><span data-stu-id="bbaec-719">The following example declares an enum type named `Alignment` with an underlying type of `sbyte`.</span></span>

```csharp
enum Alignment: sbyte
{
    Left = -1,
    Center = 0,
    Right = 1
}
```
<span data-ttu-id="bbaec-720">如前一個範例所示，enum 成員宣告可以包含指定成員值的常數運算式。</span><span class="sxs-lookup"><span data-stu-id="bbaec-720">As shown by the previous example, an enum member declaration can include a constant expression that specifies the value of the member.</span></span> <span data-ttu-id="bbaec-721">每個列舉成員的常數值必須在列舉之基礎類型的範圍內。</span><span class="sxs-lookup"><span data-stu-id="bbaec-721">The constant value for each enum member must be in the range of the underlying type of the enum.</span></span> <span data-ttu-id="bbaec-722">當列舉成員宣告未明確指定值時，會給予成員零值（如果它是列舉類型中的第一個成員），或以文字方式上一個列舉成員的值加一。</span><span class="sxs-lookup"><span data-stu-id="bbaec-722">When an enum member declaration does not explicitly specify a value, the member is given the value zero (if it is the first member in the enum type) or the value of the textually preceding enum member plus one.</span></span>

<span data-ttu-id="bbaec-723">您可以使用類型轉換，將列舉值轉換成整數值，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="bbaec-723">Enum values can be converted to integral values and vice versa using type casts.</span></span> <span data-ttu-id="bbaec-724">例如：</span><span class="sxs-lookup"><span data-stu-id="bbaec-724">For example</span></span>

```csharp
int i = (int)Color.Blue;        // int i = 2;
Color c = (Color)2;             // Color c = Color.Blue;
```
<span data-ttu-id="bbaec-725">任何列舉類型的預設值都是轉換成列舉類型的整數值零。</span><span class="sxs-lookup"><span data-stu-id="bbaec-725">The default value of any enum type is the integral value zero converted to the enum type.</span></span> <span data-ttu-id="bbaec-726">在變數自動初始化為預設值的情況下，這是提供給列舉類型變數的值。</span><span class="sxs-lookup"><span data-stu-id="bbaec-726">In cases where variables are automatically initialized to a default value, this is the value given to variables of enum types.</span></span> <span data-ttu-id="bbaec-727">為了讓列舉類型的預設值便於使用，常`0`值會隱含轉換成任何列舉類型。</span><span class="sxs-lookup"><span data-stu-id="bbaec-727">In order for the default value of an enum type to be easily available, the literal `0` implicitly converts to any enum type.</span></span> <span data-ttu-id="bbaec-728">因此，允許以下的情況。</span><span class="sxs-lookup"><span data-stu-id="bbaec-728">Thus, the following is permitted.</span></span>

```csharp
Color c = 0;
```

## <a name="delegates"></a><span data-ttu-id="bbaec-729">委派</span><span class="sxs-lookup"><span data-stu-id="bbaec-729">Delegates</span></span>

<span data-ttu-id="bbaec-730">「委派型別」代表對方法的參考，其中含有特定參數清單與傳回型別。</span><span class="sxs-lookup"><span data-stu-id="bbaec-730">A ***delegate type*** represents references to methods with a particular parameter list and return type.</span></span> <span data-ttu-id="bbaec-731">委派讓您可將方法視為實體，而實體能指派給變數或當作參數來傳遞。</span><span class="sxs-lookup"><span data-stu-id="bbaec-731">Delegates make it possible to treat methods as entities that can be assigned to variables and passed as parameters.</span></span> <span data-ttu-id="bbaec-732">委派就類似其他程式設計語言中的函式指標，但與函式指標的不同之處是，委派是物件導向且為型別安全。</span><span class="sxs-lookup"><span data-stu-id="bbaec-732">Delegates are similar to the concept of function pointers found in some other languages, but unlike function pointers, delegates are object-oriented and type-safe.</span></span>

<span data-ttu-id="bbaec-733">下列範例會宣告並使用名為 `Function` 的委派型別。</span><span class="sxs-lookup"><span data-stu-id="bbaec-733">The following example declares and uses a delegate type named `Function`.</span></span>

```csharp
using System;

delegate double Function(double x);

class Multiplier
{
    double factor;

    public Multiplier(double factor) {
        this.factor = factor;
    }

    public double Multiply(double x) {
        return x * factor;
    }
}

class Test
{
    static double Square(double x) {
        return x * x;
    }

    static double[] Apply(double[] a, Function f) {
        double[] result = new double[a.Length];
        for (int i = 0; i < a.Length; i++) result[i] = f(a[i]);
        return result;
    }

    static void Main() {
        double[] a = {0.0, 0.5, 1.0};
        double[] squares = Apply(a, Square);
        double[] sines = Apply(a, Math.Sin);
        Multiplier m = new Multiplier(2.0);
        double[] doubles =  Apply(a, m.Multiply);
    }
}
```
<span data-ttu-id="bbaec-734">`Function` 委派型別的執行個體可以參考任何採用 `double` 引數並傳回 `double` 值的方法。</span><span class="sxs-lookup"><span data-stu-id="bbaec-734">An instance of the `Function` delegate type can reference any method that takes a `double` argument and returns a `double` value.</span></span> <span data-ttu-id="bbaec-735">方法會將指定的`Function`套用`double[]`至的專案， `double[]`並傳回包含結果的。 `Apply`</span><span class="sxs-lookup"><span data-stu-id="bbaec-735">The `Apply` method applies a given `Function` to the elements of a `double[]`, returning a `double[]` with the results.</span></span> <span data-ttu-id="bbaec-736">在 `Main` 方法中，是使用 `Apply` 將三個不同的函式套用到 `double[]`。</span><span class="sxs-lookup"><span data-stu-id="bbaec-736">In the `Main` method, `Apply` is used to apply three different functions to a `double[]`.</span></span>

<span data-ttu-id="bbaec-737">委派可以參考靜態方法 (例如上一個範例中的 `Square` 或 `Math.Sin`)，或是參考執行個體方法 (例如上一個範例中的 `m.Multiply`)。</span><span class="sxs-lookup"><span data-stu-id="bbaec-737">A delegate can reference either a static method (such as `Square` or `Math.Sin` in the previous example) or an instance method (such as `m.Multiply` in the previous example).</span></span> <span data-ttu-id="bbaec-738">參考執行個體方法的委派也會參考特定的物件，而且透過委派來叫用執行個體方法時，該物件就會變成叫用中的 `this`。</span><span class="sxs-lookup"><span data-stu-id="bbaec-738">A delegate that references an instance method also references a particular object, and when the instance method is invoked through the delegate, that object becomes `this` in the invocation.</span></span>

<span data-ttu-id="bbaec-739">您也可以使用匿名函式來建立委派，這些函式是動態建立的「內嵌方法」。</span><span class="sxs-lookup"><span data-stu-id="bbaec-739">Delegates can also be created using anonymous functions, which are "inline methods" that are created on the fly.</span></span> <span data-ttu-id="bbaec-740">匿名函式可以看見周圍方法的區域變數。</span><span class="sxs-lookup"><span data-stu-id="bbaec-740">Anonymous functions can see the local variables of the surrounding methods.</span></span> <span data-ttu-id="bbaec-741">因此，您可以更輕鬆地撰寫上述的乘數範例，而`Multiplier`不需要使用類別：</span><span class="sxs-lookup"><span data-stu-id="bbaec-741">Thus, the multiplier example above can be written more easily without using a `Multiplier` class:</span></span>

```csharp
double[] doubles =  Apply(a, (double x) => x * 2.0);
```
<span data-ttu-id="bbaec-742">委派有一個有趣且實用的屬性，就是它並不知道或並不在乎所參考方法的類別；唯一重要的是所參考的方法具有與委派相同的參數和傳回型別。</span><span class="sxs-lookup"><span data-stu-id="bbaec-742">An interesting and useful property of a delegate is that it does not know or care about the class of the method it references; all that matters is that the referenced method has the same parameters and return type as the delegate.</span></span>

## <a name="attributes"></a><span data-ttu-id="bbaec-743">屬性</span><span class="sxs-lookup"><span data-stu-id="bbaec-743">Attributes</span></span>

<span data-ttu-id="bbaec-744">C# 程式中的型別、成員和其他實體支援控制其某方面行為的修飾詞。</span><span class="sxs-lookup"><span data-stu-id="bbaec-744">Types, members, and other entities in a C# program support modifiers that control certain aspects of their behavior.</span></span> <span data-ttu-id="bbaec-745">例如，方法的協助工具是使用 `public`、`protected`、`internal` 和 `private` 修飾詞控制。</span><span class="sxs-lookup"><span data-stu-id="bbaec-745">For example, the accessibility of a method is controlled using the `public`, `protected`, `internal`, and `private` modifiers.</span></span> <span data-ttu-id="bbaec-746">C# 將此能力一般化，宣告式資訊的使用者定義型別才能附加至程式實體，並在執行階段擷取。</span><span class="sxs-lookup"><span data-stu-id="bbaec-746">C# generalizes this capability such that user-defined types of declarative information can be attached to program entities and retrieved at run-time.</span></span> <span data-ttu-id="bbaec-747">程式是透過定義和使用***屬性***指定這項額外的宣告式資訊。</span><span class="sxs-lookup"><span data-stu-id="bbaec-747">Programs specify this additional declarative information by defining and using ***attributes***.</span></span>

<span data-ttu-id="bbaec-748">下列範例宣告的 `HelpAttribute` 屬性可置於程式實體，以提供其相關文件的連結。</span><span class="sxs-lookup"><span data-stu-id="bbaec-748">The following example declares a `HelpAttribute` attribute that can be placed on program entities to provide links to their associated documentation.</span></span>

```csharp
using System;

public class HelpAttribute: Attribute
{
    string url;
    string topic;

    public HelpAttribute(string url) {
        this.url = url;
    }

    public string Url {
        get { return url; }
    }

    public string Topic {
        get { return topic; }
        set { topic = value; }
    }
}
```
<span data-ttu-id="bbaec-749">所有屬性類別均衍生自`System.Attribute` .NET Framework 所提供的基類。</span><span class="sxs-lookup"><span data-stu-id="bbaec-749">All attribute classes derive from the `System.Attribute` base class provided by the .NET Framework.</span></span> <span data-ttu-id="bbaec-750">在相關聯的宣告之前，於方括弧中提供屬性的名稱 (及任何引數) 即可套用屬性。</span><span class="sxs-lookup"><span data-stu-id="bbaec-750">Attributes can be applied by giving their name, along with any arguments, inside square brackets just before the associated declaration.</span></span> <span data-ttu-id="bbaec-751">如果屬性名稱的結尾`Attribute`是，則參考該屬性時，可以省略該部分的名稱。</span><span class="sxs-lookup"><span data-stu-id="bbaec-751">If an attribute's name ends in `Attribute`, that part of the name can be omitted when the attribute is referenced.</span></span> <span data-ttu-id="bbaec-752">例如，`HelpAttribute` 屬性可以下列方式使用。</span><span class="sxs-lookup"><span data-stu-id="bbaec-752">For example, the `HelpAttribute` attribute can be used as follows.</span></span>

```csharp
[Help("http://msdn.microsoft.com/.../MyClass.htm")]
public class Widget
{
    [Help("http://msdn.microsoft.com/.../MyClass.htm", Topic = "Display")]
    public void Display(string text) {}
}
```
<span data-ttu-id="bbaec-753">這個範例會將`HelpAttribute`附加`Widget`至類別，並`HelpAttribute`將另`Display`一個連結至類別中的方法。</span><span class="sxs-lookup"><span data-stu-id="bbaec-753">This example attaches a `HelpAttribute` to the `Widget` class and another `HelpAttribute` to the `Display` method in the class.</span></span> <span data-ttu-id="bbaec-754">屬性類別的公用建構函式控制將屬性附加至程式實體時必須提供的資訊。</span><span class="sxs-lookup"><span data-stu-id="bbaec-754">The public constructors of an attribute class control the information that must be provided when the attribute is attached to a program entity.</span></span> <span data-ttu-id="bbaec-755">透過參考屬性類別的公用讀寫屬性可提供其他資訊 (例如先前對 `Topic` 的參考 )。</span><span class="sxs-lookup"><span data-stu-id="bbaec-755">Additional information can be provided by referencing public read-write properties of the attribute class (such as the reference to the `Topic` property previously).</span></span>

<span data-ttu-id="bbaec-756">下列範例顯示如何在執行時間使用反映來抓取指定程式實體的屬性資訊。</span><span class="sxs-lookup"><span data-stu-id="bbaec-756">The following example shows how attribute information for a given program entity can be retrieved at run-time using reflection.</span></span>

```csharp
using System;
using System.Reflection;

class Test
{
    static void ShowHelp(MemberInfo member) {
        HelpAttribute a = Attribute.GetCustomAttribute(member,
            typeof(HelpAttribute)) as HelpAttribute;
        if (a == null) {
            Console.WriteLine("No help for {0}", member);
        }
        else {
            Console.WriteLine("Help for {0}:", member);
            Console.WriteLine("  Url={0}, Topic={1}", a.Url, a.Topic);
        }
    }

    static void Main() {
        ShowHelp(typeof(Widget));
        ShowHelp(typeof(Widget).GetMethod("Display"));
    }
}
```
<span data-ttu-id="bbaec-757">透過反射要求特定的屬性時，會以程式來源中提供的資訊叫用屬性類別的建構函式，並傳回產生的屬性執行個體。</span><span class="sxs-lookup"><span data-stu-id="bbaec-757">When a particular attribute is requested through reflection, the constructor for the attribute class is invoked with the information provided in the program source, and the resulting attribute instance is returned.</span></span> <span data-ttu-id="bbaec-758">如果是透過屬性提供其他資訊，傳回屬性執行個體之前，這些屬性會設為指定的值。</span><span class="sxs-lookup"><span data-stu-id="bbaec-758">If additional information was provided through properties, those properties are set to the given values before the attribute instance is returned.</span></span>
