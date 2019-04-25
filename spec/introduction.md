---
ms.openlocfilehash: db10046af5d635b430951679a448e23680b18b87
ms.sourcegitcommit: 4cc6d73a765ac9827ab00c48ad9f09204baf888f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/18/2019
ms.locfileid: "59426808"
---
# <a name="introduction"></a><span data-ttu-id="dbe42-101">簡介</span><span class="sxs-lookup"><span data-stu-id="dbe42-101">Introduction</span></span>

<span data-ttu-id="dbe42-102">C# (發音為 "See Sharp") 是簡單、物件導向、型別安全的現代化程式設計語言。</span><span class="sxs-lookup"><span data-stu-id="dbe42-102">C# (pronounced "See Sharp") is a simple, modern, object-oriented, and type-safe programming language.</span></span> <span data-ttu-id="dbe42-103">C#有源自於是 C 系列語言中，而且會立即熟悉 C， C++，和 Java 程式設計人員。</span><span class="sxs-lookup"><span data-stu-id="dbe42-103">C# has its roots in the C family of languages and will be immediately familiar to C, C++, and Java programmers.</span></span> <span data-ttu-id="dbe42-104">C# 由標準化為 ECMA International ***ECMA-334***標準，並由 ISO/IEC 作為***ISO/IEC 23270***標準。</span><span class="sxs-lookup"><span data-stu-id="dbe42-104">C# is standardized by ECMA International as the ***ECMA-334*** standard and by ISO/IEC as the ***ISO/IEC 23270*** standard.</span></span> <span data-ttu-id="dbe42-105">Microsoft 的 C# 編譯器為.NET Framework 是符合這些標準實作。</span><span class="sxs-lookup"><span data-stu-id="dbe42-105">Microsoft's C# compiler for the .NET Framework is a conforming implementation of both of these standards.</span></span>

<span data-ttu-id="dbe42-106">C# 是物件導向的語言，但 C# 更進一步支援「元件導向」程式設計。</span><span class="sxs-lookup"><span data-stu-id="dbe42-106">C# is an object-oriented language, but C# further includes support for ***component-oriented*** programming.</span></span> <span data-ttu-id="dbe42-107">現代軟體設計逐漸依賴功能性上獨立與屬於自我描述套件的軟體元件。</span><span class="sxs-lookup"><span data-stu-id="dbe42-107">Contemporary software design increasingly relies on software components in the form of self-contained and self-describing packages of functionality.</span></span> <span data-ttu-id="dbe42-108">這類元件的關鍵在於它們呈現含有屬性、方法和事件的程式設計模型；它們提供元件相關宣告資訊的屬性，且併入自己的文件。</span><span class="sxs-lookup"><span data-stu-id="dbe42-108">Key to such components is that they present a programming model with properties, methods, and events; they have attributes that provide declarative information about the component; and they incorporate their own documentation.</span></span> <span data-ttu-id="dbe42-109">C# 提供語言建構來直接支援這些概念，使 C# 中，以建立和使用軟體元件非常自然的語言。</span><span class="sxs-lookup"><span data-stu-id="dbe42-109">C# provides language constructs to directly support these concepts, making C# a very natural language in which to create and use software components.</span></span>

<span data-ttu-id="dbe42-110">以下幾個 C# 功能可協助建構強固且耐用的應用程式：***記憶體回收***自動回收未使用的物件; 所佔用的記憶體***例外狀況處理***提供錯誤偵測和復原; 的結構化和可延伸方法並***型別安全***語言設計，因此無法讀取未初始化的變數，要編製索引超過其範圍中，或執行未檢查的陣列類型轉換。</span><span class="sxs-lookup"><span data-stu-id="dbe42-110">Several C# features aid in the construction of robust and durable applications: ***Garbage collection*** automatically reclaims memory occupied by unused objects; ***exception handling*** provides a structured and extensible approach to error detection and recovery; and the ***type-safe*** design of the language makes it impossible to read from uninitialized variables, to index arrays beyond their bounds, or to perform unchecked type casts.</span></span>

<span data-ttu-id="dbe42-111">C# 有***統一的型別系統***。</span><span class="sxs-lookup"><span data-stu-id="dbe42-111">C# has a ***unified type system***.</span></span> <span data-ttu-id="dbe42-112">所有的 C# 型別 (包括 `int` 和 `double` 等基本型別) 都繼承自單一的 `object` 根型別。</span><span class="sxs-lookup"><span data-stu-id="dbe42-112">All C# types, including primitive types such as `int` and `double`, inherit from a single root `object` type.</span></span> <span data-ttu-id="dbe42-113">因此，所有型別都擁有相同的一組常見作業，且任何型別的值都能以相同方式儲存、傳送及操作。</span><span class="sxs-lookup"><span data-stu-id="dbe42-113">Thus, all types share a set of common operations, and values of any type can be stored, transported, and operated upon in a consistent manner.</span></span> <span data-ttu-id="dbe42-114">此外，C# 支援使用者定義的參考型別和數值型別，因此能動態配置物件，以及內嵌儲存輕量結構。</span><span class="sxs-lookup"><span data-stu-id="dbe42-114">Furthermore, C# supports both user-defined reference types and value types, allowing dynamic allocation of objects as well as in-line storage of lightweight structures.</span></span>

<span data-ttu-id="dbe42-115">若要確保，C# 程式和程式庫可以隨著時間以相容的方式，更強調已置於***版本控制***在 C# 的設計。</span><span class="sxs-lookup"><span data-stu-id="dbe42-115">To ensure that C# programs and libraries can evolve over time in a compatible manner, much emphasis has been placed on ***versioning*** in C#'s design.</span></span> <span data-ttu-id="dbe42-116">許多程式設計語言並不注重此問題，因此當推出新版的相依程式庫時，以那些語言撰寫的程式需要進行較多修改才能繼續運作。</span><span class="sxs-lookup"><span data-stu-id="dbe42-116">Many programming languages pay little attention to this issue, and, as a result, programs written in those languages break more often than necessary when newer versions of dependent libraries are introduced.</span></span> <span data-ttu-id="dbe42-117">已直接受到版本控制考量的 C# 設計層面包括個別`virtual`和`override`修飾詞，方法多載解析，規則支援明確介面成員宣告。</span><span class="sxs-lookup"><span data-stu-id="dbe42-117">Aspects of C#'s design that were directly influenced by versioning considerations include the separate `virtual` and `override` modifiers, the rules for method overload resolution, and support for explicit interface member declarations.</span></span>

<span data-ttu-id="dbe42-118">本章的其餘部分會說明 C# 語言的基本功能。</span><span class="sxs-lookup"><span data-stu-id="dbe42-118">The rest of this chapter describes the essential features of the C# language.</span></span> <span data-ttu-id="dbe42-119">雖然之後的章節會說明規則和例外狀況詳細資料導向和數學的方式，但這一章致力為了清楚起見和求簡單明瞭，但會犧牲完整性。</span><span class="sxs-lookup"><span data-stu-id="dbe42-119">Although later chapters describe rules and exceptions in a detail-oriented and sometimes mathematical manner, this chapter strives for clarity and brevity at the expense of completeness.</span></span> <span data-ttu-id="dbe42-120">目的是要提供讀者會加速早期的程式撰寫和後面的章節則讀取的語言簡介。</span><span class="sxs-lookup"><span data-stu-id="dbe42-120">The intent is to provide the reader with an introduction to the language that will facilitate the writing of early programs and the reading of later chapters.</span></span>

## <a name="hello-world"></a><span data-ttu-id="dbe42-121">Hello World</span><span class="sxs-lookup"><span data-stu-id="dbe42-121">Hello world</span></span>

<span data-ttu-id="dbe42-122">“Hello, World” 程式通常用來介紹程式設計語言。</span><span class="sxs-lookup"><span data-stu-id="dbe42-122">The "Hello, World" program is traditionally used to introduce a programming language.</span></span> <span data-ttu-id="dbe42-123">以下是以 C# 撰寫的：</span><span class="sxs-lookup"><span data-stu-id="dbe42-123">Here it is in C#:</span></span>

```csharp
using System;

class Hello
{
    static void Main() {
        Console.WriteLine("Hello, World");
    }
}
```

<span data-ttu-id="dbe42-124">C# 原始程式檔的副檔名通常是 `.cs`。</span><span class="sxs-lookup"><span data-stu-id="dbe42-124">C# source files typically have the file extension `.cs`.</span></span> <span data-ttu-id="dbe42-125">假設"Hello，World"程式儲存在檔案`hello.cs`，可以使用命令列，Microsoft C# 編譯器編譯程式</span><span class="sxs-lookup"><span data-stu-id="dbe42-125">Assuming that the "Hello, World" program is stored in the file `hello.cs`, the program can be compiled with the Microsoft C# compiler using the command line</span></span>
```
csc hello.cs
```
<span data-ttu-id="dbe42-126">這會產生名為可執行組件`hello.exe`。</span><span class="sxs-lookup"><span data-stu-id="dbe42-126">which produces an executable assembly named `hello.exe`.</span></span> <span data-ttu-id="dbe42-127">執行時，此應用程式所產生的輸出</span><span class="sxs-lookup"><span data-stu-id="dbe42-127">The output produced by this application when it is run is</span></span>
```
Hello, World
```

<span data-ttu-id="dbe42-128">“Hello, World” 程式的開頭為 `using` 指示詞，會參考 `System` 命名空間。</span><span class="sxs-lookup"><span data-stu-id="dbe42-128">The "Hello, World" program starts with a `using` directive that references the `System` namespace.</span></span> <span data-ttu-id="dbe42-129">命名空間提供組織 C# 程式和程式庫的階層式方法。</span><span class="sxs-lookup"><span data-stu-id="dbe42-129">Namespaces provide a hierarchical means of organizing C# programs and libraries.</span></span> <span data-ttu-id="dbe42-130">命名空間包含型別和其他命名空間，例如 `System` 命名空間包含數個型別 (如程式中參考的 `Console` 類別)，和數個其他命名空間 (如 `IO` 和 `Collections`)。</span><span class="sxs-lookup"><span data-stu-id="dbe42-130">Namespaces contain types and other namespaces—for example, the `System` namespace contains a number of types, such as the `Console` class referenced in the program, and a number of other namespaces, such as `IO` and `Collections`.</span></span> <span data-ttu-id="dbe42-131">使用 `using` 指示詞參考指定的命名空間，就能以非限定的方式使用屬於該命名空間成員的型別。</span><span class="sxs-lookup"><span data-stu-id="dbe42-131">A `using` directive that references a given namespace enables unqualified use of the types that are members of that namespace.</span></span> <span data-ttu-id="dbe42-132">因為 `using` 指示詞的緣故，該程式可以使用 `Console.WriteLine` 當作 `System.Console.WriteLine` 的縮寫。</span><span class="sxs-lookup"><span data-stu-id="dbe42-132">Because of the `using` directive, the program can use `Console.WriteLine` as shorthand for `System.Console.WriteLine`.</span></span>

<span data-ttu-id="dbe42-133">“Hello, World” 程式宣告的 `Hello` 類別包含單一成員，即名為 `Main` 的方法。</span><span class="sxs-lookup"><span data-stu-id="dbe42-133">The `Hello` class declared by the "Hello, World" program has a single member, the method named `Main`.</span></span> <span data-ttu-id="dbe42-134">`Main`方法以宣告`static`修飾詞。</span><span class="sxs-lookup"><span data-stu-id="dbe42-134">The `Main` method is declared with the `static` modifier.</span></span> <span data-ttu-id="dbe42-135">執行個體方法可以使用關鍵字 `this` 參考特定的封入物件執行個體，但靜態方法卻不需要參考特定物件即可運作。</span><span class="sxs-lookup"><span data-stu-id="dbe42-135">While instance methods can reference a particular enclosing object instance using the keyword `this`, static methods operate without reference to a particular object.</span></span> <span data-ttu-id="dbe42-136">依照慣例，會使用名為 `Main` 的靜態方法做為程式的進入點。</span><span class="sxs-lookup"><span data-stu-id="dbe42-136">By convention, a static method named `Main` serves as the entry point of a program.</span></span>

<span data-ttu-id="dbe42-137">程式的輸出是由 `System` 命名空間中 `Console` 類別的 `WriteLine` 方法產生。</span><span class="sxs-lookup"><span data-stu-id="dbe42-137">The output of the program is produced by the `WriteLine` method of the `Console` class in the `System` namespace.</span></span> <span data-ttu-id="dbe42-138">這個類別會提供.NET Framework 類別庫，其中，根據預設，Microsoft C# 編譯器會自動參考。</span><span class="sxs-lookup"><span data-stu-id="dbe42-138">This class is provided by the .NET Framework class libraries, which, by default, are automatically referenced by the Microsoft C# compiler.</span></span> <span data-ttu-id="dbe42-139">請注意，C# 本身不需要個別執行階段程式庫。</span><span class="sxs-lookup"><span data-stu-id="dbe42-139">Note that C# itself does not have a separate runtime library.</span></span> <span data-ttu-id="dbe42-140">相反地，.NET Framework 是執行階段程式庫的 C#。</span><span class="sxs-lookup"><span data-stu-id="dbe42-140">Instead, the .NET Framework is the runtime library of C#.</span></span>

## <a name="program-structure"></a><span data-ttu-id="dbe42-141">程式結構</span><span class="sxs-lookup"><span data-stu-id="dbe42-141">Program structure</span></span>

<span data-ttu-id="dbe42-142">C# 語言的重要組織概念如下：程式、命名空間、型別、成員及組件。</span><span class="sxs-lookup"><span data-stu-id="dbe42-142">The key organizational concepts in C# are ***programs***, ***namespaces***, ***types***, ***members***, and ***assemblies***.</span></span> <span data-ttu-id="dbe42-143">C# 程式包含一個或多個原始程式檔。</span><span class="sxs-lookup"><span data-stu-id="dbe42-143">C# programs consist of one or more source files.</span></span> <span data-ttu-id="dbe42-144">程式宣告型別，其中包含成員並可以依據命名空間分組。</span><span class="sxs-lookup"><span data-stu-id="dbe42-144">Programs declare types, which contain members and can be organized into namespaces.</span></span> <span data-ttu-id="dbe42-145">類別和介面都是型別的範例。</span><span class="sxs-lookup"><span data-stu-id="dbe42-145">Classes and interfaces are examples of types.</span></span> <span data-ttu-id="dbe42-146">欄位、方法、屬性及事件都是成員的範例。</span><span class="sxs-lookup"><span data-stu-id="dbe42-146">Fields, methods, properties, and events are examples of members.</span></span> <span data-ttu-id="dbe42-147">在編譯 C# 語言時，會將它們實際封裝為組件。</span><span class="sxs-lookup"><span data-stu-id="dbe42-147">When C# programs are compiled, they are physically packaged into assemblies.</span></span> <span data-ttu-id="dbe42-148">組件通常具有副檔名`.exe`或`.dll`，這取決於它們實作是否***應用程式***或是***程式庫***。</span><span class="sxs-lookup"><span data-stu-id="dbe42-148">Assemblies typically have the file extension `.exe` or `.dll`, depending on whether they implement ***applications*** or ***libraries***.</span></span>

<span data-ttu-id="dbe42-149">此範例</span><span class="sxs-lookup"><span data-stu-id="dbe42-149">The example</span></span>

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
<span data-ttu-id="dbe42-150">宣告類別，名為`Stack`稱為命名空間中`Acme.Collections`。</span><span class="sxs-lookup"><span data-stu-id="dbe42-150">declares a class named `Stack` in a namespace called `Acme.Collections`.</span></span> <span data-ttu-id="dbe42-151">此類別的完整名稱是 `Acme.Collections.Stack`。</span><span class="sxs-lookup"><span data-stu-id="dbe42-151">The fully qualified name of this class is `Acme.Collections.Stack`.</span></span> <span data-ttu-id="dbe42-152">該類別包含數個成員︰一個名為 `top` 的欄位、兩個名為 `Push` 和 `Pop` 的方法以及名為 `Entry` 的巢狀類別。</span><span class="sxs-lookup"><span data-stu-id="dbe42-152">The class contains several members: a field named `top`, two methods named `Push` and `Pop`, and a nested class named `Entry`.</span></span> <span data-ttu-id="dbe42-153">`Entry` 類別更包含三個成員︰一個名為 `next` 的欄位、一個名為 `data` 的欄位以及建構函式。</span><span class="sxs-lookup"><span data-stu-id="dbe42-153">The `Entry` class further contains three members: a field named `next`, a field named `data`, and a constructor.</span></span> <span data-ttu-id="dbe42-154">假設此範例的原始程式碼儲存在檔案 `acme.cs`，命令列</span><span class="sxs-lookup"><span data-stu-id="dbe42-154">Assuming that the source code of the example is stored in the file `acme.cs`, the command line</span></span>

```
csc /t:library acme.cs
```
<span data-ttu-id="dbe42-155">會將範例編譯為程式庫 (不需要 `Main` 進入點的程式碼)，並產生名為 `acme.dll` 的組件。</span><span class="sxs-lookup"><span data-stu-id="dbe42-155">compiles the example as a library (code without a `Main` entry point) and produces an assembly named `acme.dll`.</span></span>

<span data-ttu-id="dbe42-156">組件包含可執行的程式碼的形式***Intermediate Language*** (IL) 指令，而符號資訊採用的形式***中繼資料***。</span><span class="sxs-lookup"><span data-stu-id="dbe42-156">Assemblies contain executable code in the form of ***Intermediate Language*** (IL) instructions, and symbolic information in the form of ***metadata***.</span></span> <span data-ttu-id="dbe42-157">執行前，組件中的 IL 程式碼會由 .NET 通用語言執行平台的 Just-In-Time (JIT) 編譯器自動轉換為處理器特定程式碼。</span><span class="sxs-lookup"><span data-stu-id="dbe42-157">Before it is executed, the IL code in an assembly is automatically converted to processor-specific code by the Just-In-Time (JIT) compiler of .NET Common Language Runtime.</span></span>

<span data-ttu-id="dbe42-158">由於組件是包含程式碼和中繼資料的功能自我描述單位，所以不需要提供 C# 中的 `#include` 指示詞和標頭檔。</span><span class="sxs-lookup"><span data-stu-id="dbe42-158">Because an assembly is a self-describing unit of functionality containing both code and metadata, there is no need for `#include` directives and header files in C#.</span></span> <span data-ttu-id="dbe42-159">特定組件中所包含的公用型別和成員只能在編譯程式時，使用 C# 程式來參考該組件。</span><span class="sxs-lookup"><span data-stu-id="dbe42-159">The public types and members contained in a particular assembly are made available in a C# program simply by referencing that assembly when compiling the program.</span></span> <span data-ttu-id="dbe42-160">例如，此程式使用來自 `acme.dll` 組件的 `Acme.Collections.Stack` 類別︰</span><span class="sxs-lookup"><span data-stu-id="dbe42-160">For example, this program uses the `Acme.Collections.Stack` class from the `acme.dll` assembly:</span></span>

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
<span data-ttu-id="dbe42-161">如果程式儲存在檔案`test.cs`，當`test.cs`編譯`acme.dll`可以使用編譯器的參考組件`/r`選項：</span><span class="sxs-lookup"><span data-stu-id="dbe42-161">If the program is stored in the file `test.cs`, when `test.cs` is compiled, the `acme.dll` assembly can be referenced using the compiler's `/r` option:</span></span>

```
csc /r:acme.dll test.cs
```
<span data-ttu-id="dbe42-162">這樣會建立名為 `test.exe` 可執行檔組件，執行時會產生以下輸出︰</span><span class="sxs-lookup"><span data-stu-id="dbe42-162">This creates an executable assembly named `test.exe`, which, when run, produces the output:</span></span>

```
100
10
1
```
<span data-ttu-id="dbe42-163">C# 允許以數個原始程式檔儲存程式的原始程式文字。</span><span class="sxs-lookup"><span data-stu-id="dbe42-163">C# permits the source text of a program to be stored in several source files.</span></span> <span data-ttu-id="dbe42-164">編譯有多檔案的 C# 程式時，所有原始程式檔都會一起處理，而原始程式檔可以隨意地互相參考。概念上，就如同將所有原始程式檔都串連成一個大型檔案，然後再進行處理。</span><span class="sxs-lookup"><span data-stu-id="dbe42-164">When a multi-file C# program is compiled, all of the source files are processed together, and the source files can freely reference each other—conceptually, it is as if all the source files were concatenated into one large file before being processed.</span></span> <span data-ttu-id="dbe42-165">C# 中不再需要向前宣告，原因是在極少數的例外狀況中，宣告順序並不重要。</span><span class="sxs-lookup"><span data-stu-id="dbe42-165">Forward declarations are never needed in C# because, with very few exceptions, declaration order is insignificant.</span></span> <span data-ttu-id="dbe42-166">C# 不會限制原始程式檔只能宣告一個公用型別，也不會要求原始程式檔符合原始程式檔中所宣告的型別名稱。</span><span class="sxs-lookup"><span data-stu-id="dbe42-166">C# does not limit a source file to declaring only one public type nor does it require the name of the source file to match a type declared in the source file.</span></span>

## <a name="types-and-variables"></a><span data-ttu-id="dbe42-167">型別與變數</span><span class="sxs-lookup"><span data-stu-id="dbe42-167">Types and variables</span></span>

<span data-ttu-id="dbe42-168">C# 中有兩種型別：***實值型別***和***參考型別***。</span><span class="sxs-lookup"><span data-stu-id="dbe42-168">There are two kinds of types in C#: ***value types*** and ***reference types***.</span></span> <span data-ttu-id="dbe42-169">實值型別的變數直接包含其資料，而參考型別的變數則將參考儲存到其資料，後者即是物件。</span><span class="sxs-lookup"><span data-stu-id="dbe42-169">Variables of value types directly contain their data whereas variables of reference types store references to their data, the latter being known as objects.</span></span> <span data-ttu-id="dbe42-170">使用參考型別時，這兩種變數可以參考相同的物件，因此對其中一個變數進行的作業可能會影響另一個變數所參考的物件。</span><span class="sxs-lookup"><span data-stu-id="dbe42-170">With reference types, it is possible for two variables to reference the same object and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="dbe42-171">使用實值型別時，變數各有自己的資料複本，因此在某一個變數上進行的作業不可能會影響其他變數 (但 `ref` 和 `out` 參數變數除外)。</span><span class="sxs-lookup"><span data-stu-id="dbe42-171">With value types, the variables each have their own copy of the data, and it is not possible for operations on one to affect the other (except in the case of `ref` and `out` parameter variables).</span></span>

<span data-ttu-id="dbe42-172">C# 的實值型別可進一步細分***簡單型別***，***列舉型別***，***結構型別***，以及***可為 null 的型別***，和 C# 參考型別可進一步細分***類別型別***，***介面型別***，***陣列型別***，以及***委派型別***。</span><span class="sxs-lookup"><span data-stu-id="dbe42-172">C#'s value types are further divided into ***simple types***, ***enum types***, ***struct types***, and ***nullable types***, and C#'s reference types are further divided into ***class types***, ***interface types***, ***array types***, and ***delegate types***.</span></span>

<span data-ttu-id="dbe42-173">下表提供 C# 類型系統的概觀。</span><span class="sxs-lookup"><span data-stu-id="dbe42-173">The following table provides an overview of C#'s type system.</span></span>

| <span data-ttu-id="dbe42-174">__分類__</span><span class="sxs-lookup"><span data-stu-id="dbe42-174">__Category__</span></span>    |                 | <span data-ttu-id="dbe42-175">__描述__</span><span class="sxs-lookup"><span data-stu-id="dbe42-175">__Description__</span></span> |
|-----------------|-----------------|-----------------|
| <span data-ttu-id="dbe42-176">值類型</span><span class="sxs-lookup"><span data-stu-id="dbe42-176">Value types</span></span>     | <span data-ttu-id="dbe42-177">簡單型別</span><span class="sxs-lookup"><span data-stu-id="dbe42-177">Simple types</span></span>    | <span data-ttu-id="dbe42-178">帶正負號的整數︰`sbyte`、`short`、`int`、`long`</span><span class="sxs-lookup"><span data-stu-id="dbe42-178">Signed integral: `sbyte`, `short`, `int`, `long`</span></span> |
|                 |                 | <span data-ttu-id="dbe42-179">不帶正負號的整數︰`byte`、`ushort`、`uint`、`ulong`</span><span class="sxs-lookup"><span data-stu-id="dbe42-179">Unsigned integral: `byte`, `ushort`, `uint`, `ulong`</span></span> |
|                 |                 | <span data-ttu-id="dbe42-180">Unicode 字元：`char`</span><span class="sxs-lookup"><span data-stu-id="dbe42-180">Unicode characters: `char`</span></span> |
|                 |                 | <span data-ttu-id="dbe42-181">IEEE 浮點數：`float`、`double`</span><span class="sxs-lookup"><span data-stu-id="dbe42-181">IEEE floating point: `float`, `double`</span></span> |
|                 |                 | <span data-ttu-id="dbe42-182">高精確度十進位︰`decimal`</span><span class="sxs-lookup"><span data-stu-id="dbe42-182">High-precision decimal: `decimal`</span></span> |
|                 |                 | <span data-ttu-id="dbe42-183">布林值：`bool`</span><span class="sxs-lookup"><span data-stu-id="dbe42-183">Boolean: `bool`</span></span> |
|                 | <span data-ttu-id="dbe42-184">列舉型別</span><span class="sxs-lookup"><span data-stu-id="dbe42-184">Enum types</span></span>      | <span data-ttu-id="dbe42-185">使用者定義型別，格式為 `enum E {...}`</span><span class="sxs-lookup"><span data-stu-id="dbe42-185">User-defined types of the form `enum E {...}`</span></span> |
|                 | <span data-ttu-id="dbe42-186">結構型別</span><span class="sxs-lookup"><span data-stu-id="dbe42-186">Struct types</span></span>    | <span data-ttu-id="dbe42-187">使用者定義型別，格式為 `struct S {...}`</span><span class="sxs-lookup"><span data-stu-id="dbe42-187">User-defined types of the form `struct S {...}`</span></span> |
|                 | <span data-ttu-id="dbe42-188">可為 Null 的型別</span><span class="sxs-lookup"><span data-stu-id="dbe42-188">Nullable types</span></span>  | <span data-ttu-id="dbe42-189">含有 `null` 值的所有其他數值型別的擴充</span><span class="sxs-lookup"><span data-stu-id="dbe42-189">Extensions of all other value types with a `null` value</span></span> |
| <span data-ttu-id="dbe42-190">參考型別</span><span class="sxs-lookup"><span data-stu-id="dbe42-190">Reference types</span></span> | <span data-ttu-id="dbe42-191">類別型別</span><span class="sxs-lookup"><span data-stu-id="dbe42-191">Class types</span></span>     | <span data-ttu-id="dbe42-192">所有其他型別的基底類別︰`object`</span><span class="sxs-lookup"><span data-stu-id="dbe42-192">Ultimate base class of all other types: `object`</span></span> |
|                 |                 | <span data-ttu-id="dbe42-193">Unicode 字串：`string`</span><span class="sxs-lookup"><span data-stu-id="dbe42-193">Unicode strings: `string`</span></span> |
|                 |                 | <span data-ttu-id="dbe42-194">使用者定義型別，格式為 `class C {...}`</span><span class="sxs-lookup"><span data-stu-id="dbe42-194">User-defined types of the form `class C {...}`</span></span> |
|                 | <span data-ttu-id="dbe42-195">介面型別</span><span class="sxs-lookup"><span data-stu-id="dbe42-195">Interface types</span></span> | <span data-ttu-id="dbe42-196">使用者定義型別，格式為 `interface I {...}`</span><span class="sxs-lookup"><span data-stu-id="dbe42-196">User-defined types of the form `interface I {...}`</span></span> |
|                 | <span data-ttu-id="dbe42-197">陣列型別</span><span class="sxs-lookup"><span data-stu-id="dbe42-197">Array types</span></span>     | <span data-ttu-id="dbe42-198">單一維度和多維度，例如 `int[]` 和 `int[,]`</span><span class="sxs-lookup"><span data-stu-id="dbe42-198">Single- and multi-dimensional, for example, `int[]` and `int[,]`</span></span> |
|                 | <span data-ttu-id="dbe42-199">委派型別</span><span class="sxs-lookup"><span data-stu-id="dbe42-199">Delegate types</span></span>  | <span data-ttu-id="dbe42-200">使用者定義型別，格式例如 `delegate int  D(...)`</span><span class="sxs-lookup"><span data-stu-id="dbe42-200">User-defined types of the form e.g. `delegate int  D(...)`</span></span> |

<span data-ttu-id="dbe42-201">八種整數型別支援 8 位元、16 位元、32 位元和 64 位元的值 (帶正負號或不帶正負號)。</span><span class="sxs-lookup"><span data-stu-id="dbe42-201">The eight integral types provide support for 8-bit, 16-bit, 32-bit, and 64-bit values in signed or unsigned form.</span></span>

<span data-ttu-id="dbe42-202">兩個浮動點類型`float`和`double`，會使用 32 位元單精確度和 64 位元雙精確度 IEEE 754 格式來表示。</span><span class="sxs-lookup"><span data-stu-id="dbe42-202">The two floating point types, `float` and `double`, are represented using the 32-bit single-precision and 64-bit double-precision IEEE 754 formats.</span></span>

<span data-ttu-id="dbe42-203">`decimal` 型別是 128 位元的資料型別，適合財務和貨幣計算。</span><span class="sxs-lookup"><span data-stu-id="dbe42-203">The `decimal` type is a 128-bit data type suitable for financial and monetary calculations.</span></span>

<span data-ttu-id="dbe42-204">C#`bool`類型用來代表布林值 — 值，不是`true`或`false`。</span><span class="sxs-lookup"><span data-stu-id="dbe42-204">C#'s `bool` type is used to represent boolean values—values that are either `true` or `false`.</span></span>

<span data-ttu-id="dbe42-205">C# 中的字元和字串處理使用 Unicode 編碼方式。</span><span class="sxs-lookup"><span data-stu-id="dbe42-205">Character and string processing in C# uses Unicode encoding.</span></span> <span data-ttu-id="dbe42-206">`char` 型別代表 UTF-16 程式碼單位，而 `string` 型別代表一系列的 UTF-16 程式碼單位。</span><span class="sxs-lookup"><span data-stu-id="dbe42-206">The `char` type represents a UTF-16 code unit, and the `string` type represents a sequence of UTF-16 code units.</span></span>

<span data-ttu-id="dbe42-207">下表摘要說明 C# 的數值型別。</span><span class="sxs-lookup"><span data-stu-id="dbe42-207">The following table summarizes C#'s numeric types.</span></span>


| <span data-ttu-id="dbe42-208">__分類__</span><span class="sxs-lookup"><span data-stu-id="dbe42-208">__Category__</span></span>      | <span data-ttu-id="dbe42-209">__位元__</span><span class="sxs-lookup"><span data-stu-id="dbe42-209">__Bits__</span></span> | <span data-ttu-id="dbe42-210">__Type__</span><span class="sxs-lookup"><span data-stu-id="dbe42-210">__Type__</span></span>  | <span data-ttu-id="dbe42-211">__範圍/精確度__</span><span class="sxs-lookup"><span data-stu-id="dbe42-211">__Range/Precision__</span></span> |
|-------------------|----------|-----------|---------------------|
| <span data-ttu-id="dbe42-212">帶正負號的整數</span><span class="sxs-lookup"><span data-stu-id="dbe42-212">Signed integral</span></span>   | <span data-ttu-id="dbe42-213">8</span><span class="sxs-lookup"><span data-stu-id="dbe42-213">8</span></span>        | `sbyte`   | <span data-ttu-id="dbe42-214">-128...127</span><span class="sxs-lookup"><span data-stu-id="dbe42-214">-128...127</span></span> |
|                   | <span data-ttu-id="dbe42-215">16</span><span class="sxs-lookup"><span data-stu-id="dbe42-215">16</span></span>       | `short`   | <span data-ttu-id="dbe42-216">-32,768...32,767</span><span class="sxs-lookup"><span data-stu-id="dbe42-216">-32,768...32,767</span></span> |
|                   | <span data-ttu-id="dbe42-217">32</span><span class="sxs-lookup"><span data-stu-id="dbe42-217">32</span></span>       | `int`     | <span data-ttu-id="dbe42-218">-2,147,483,648...2,147,483,647</span><span class="sxs-lookup"><span data-stu-id="dbe42-218">-2,147,483,648...2,147,483,647</span></span> |
|                   | <span data-ttu-id="dbe42-219">64</span><span class="sxs-lookup"><span data-stu-id="dbe42-219">64</span></span>       | `long`    | <span data-ttu-id="dbe42-220">-9,223,372,036,854,775,808...9,223,372,036,854,775,807</span><span class="sxs-lookup"><span data-stu-id="dbe42-220">-9,223,372,036,854,775,808...9,223,372,036,854,775,807</span></span> |
| <span data-ttu-id="dbe42-221">不帶正負號的整數</span><span class="sxs-lookup"><span data-stu-id="dbe42-221">Unsigned integral</span></span> | <span data-ttu-id="dbe42-222">8</span><span class="sxs-lookup"><span data-stu-id="dbe42-222">8</span></span>        | `byte`    | <span data-ttu-id="dbe42-223">0...255</span><span class="sxs-lookup"><span data-stu-id="dbe42-223">0...255</span></span> |
|                   | <span data-ttu-id="dbe42-224">16</span><span class="sxs-lookup"><span data-stu-id="dbe42-224">16</span></span>       | `ushort`  | <span data-ttu-id="dbe42-225">0...65,535</span><span class="sxs-lookup"><span data-stu-id="dbe42-225">0...65,535</span></span> |
|                   | <span data-ttu-id="dbe42-226">32</span><span class="sxs-lookup"><span data-stu-id="dbe42-226">32</span></span>       | `uint`    | <span data-ttu-id="dbe42-227">0...4,294,967,295</span><span class="sxs-lookup"><span data-stu-id="dbe42-227">0...4,294,967,295</span></span> |
|                   | <span data-ttu-id="dbe42-228">64</span><span class="sxs-lookup"><span data-stu-id="dbe42-228">64</span></span>       | `ulong`   | <span data-ttu-id="dbe42-229">0...18,446,744,073,709,551,615</span><span class="sxs-lookup"><span data-stu-id="dbe42-229">0...18,446,744,073,709,551,615</span></span> |
| <span data-ttu-id="dbe42-230">浮點</span><span class="sxs-lookup"><span data-stu-id="dbe42-230">Floating point</span></span>    | <span data-ttu-id="dbe42-231">32</span><span class="sxs-lookup"><span data-stu-id="dbe42-231">32</span></span>       | `float`   | <span data-ttu-id="dbe42-232">1.5 × 10 ^-45 到 3.4 × 10 ^38，7 位數精確度</span><span class="sxs-lookup"><span data-stu-id="dbe42-232">1.5 × 10^−45 to 3.4 × 10^38, 7-digit precision</span></span> |
|                   | <span data-ttu-id="dbe42-233">64</span><span class="sxs-lookup"><span data-stu-id="dbe42-233">64</span></span>       | `double`  | <span data-ttu-id="dbe42-234">5.0 × 10 ^324 到 1.7 × 10 ^308 之內，15 位數精確度</span><span class="sxs-lookup"><span data-stu-id="dbe42-234">5.0 × 10^−324 to 1.7 × 10^308, 15-digit precision</span></span> |
| <span data-ttu-id="dbe42-235">Decimal</span><span class="sxs-lookup"><span data-stu-id="dbe42-235">Decimal</span></span>           | <span data-ttu-id="dbe42-236">128</span><span class="sxs-lookup"><span data-stu-id="dbe42-236">128</span></span>      | `decimal` | <span data-ttu-id="dbe42-237">1.0 × 10 ^ −28 到 7.9 × 10 ^ 月 28 日 28 位數精確度</span><span class="sxs-lookup"><span data-stu-id="dbe42-237">1.0 × 10^−28 to 7.9 × 10^28, 28-digit precision</span></span> |

<span data-ttu-id="dbe42-238">C# 程式使用***型別宣告***來建立新型別。</span><span class="sxs-lookup"><span data-stu-id="dbe42-238">C# programs use ***type declarations*** to create new types.</span></span> <span data-ttu-id="dbe42-239">型別宣告指定新型別的名稱成員。</span><span class="sxs-lookup"><span data-stu-id="dbe42-239">A type declaration specifies the name and the members of the new type.</span></span> <span data-ttu-id="dbe42-240">可由使用者定義五個 C# 類別的型別︰ 類別型別、 結構型別、 介面型別、 列舉型別及委派型別。</span><span class="sxs-lookup"><span data-stu-id="dbe42-240">Five of C#'s categories of types are user-definable: class types, struct types, interface types, enum types, and delegate types.</span></span>

<span data-ttu-id="dbe42-241">類別型別定義資料結構，其中包含資料成員 （欄位） 和函式成員 （方法、 屬性和其他項目）。</span><span class="sxs-lookup"><span data-stu-id="dbe42-241">A class type defines a data structure that contains data members (fields) and function members (methods, properties, and others).</span></span> <span data-ttu-id="dbe42-242">類別型別支援單一繼承和多型，這些是可供衍生類別將基底類別延伸及特製化的機制。</span><span class="sxs-lookup"><span data-stu-id="dbe42-242">Class types support single inheritance and polymorphism, mechanisms whereby derived classes can extend and specialize base classes.</span></span>

<span data-ttu-id="dbe42-243">它代表與資料成員和函式成員的結構與類別類型類似的結構類型。</span><span class="sxs-lookup"><span data-stu-id="dbe42-243">A struct type is similar to a class type in that it represents a structure with data members and function members.</span></span> <span data-ttu-id="dbe42-244">不過，不同於類別，結構是實值類型，而且不需要堆積配置。</span><span class="sxs-lookup"><span data-stu-id="dbe42-244">However, unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="dbe42-245">結構型別不支援使用者指定的繼承，且所有結構型別都隱含地繼承自 `object` 型別。</span><span class="sxs-lookup"><span data-stu-id="dbe42-245">Struct types do not support user-specified inheritance, and all struct types implicitly inherit from type `object`.</span></span>

<span data-ttu-id="dbe42-246">介面型別定義為一組具名的公用函式成員的合約。</span><span class="sxs-lookup"><span data-stu-id="dbe42-246">An interface type defines a contract as a named set of public function members.</span></span> <span data-ttu-id="dbe42-247">類別或結構實作介面，必須提供介面的函式成員的實作。</span><span class="sxs-lookup"><span data-stu-id="dbe42-247">A class or struct that implements an interface must provide implementations of the interface's function members.</span></span> <span data-ttu-id="dbe42-248">介面可以繼承自多個基底介面，以及類別或結構可以實作多個介面。</span><span class="sxs-lookup"><span data-stu-id="dbe42-248">An interface may inherit from multiple base interfaces, and a class or struct may implement multiple interfaces.</span></span>

<span data-ttu-id="dbe42-249">委派型別代表具有特定參數清單和傳回類型的方法的參考。</span><span class="sxs-lookup"><span data-stu-id="dbe42-249">A delegate type represents references to methods with a particular parameter list and return type.</span></span> <span data-ttu-id="dbe42-250">委派讓您可將方法視為實體，而實體能指派給變數或當作參數來傳遞。</span><span class="sxs-lookup"><span data-stu-id="dbe42-250">Delegates make it possible to treat methods as entities that can be assigned to variables and passed as parameters.</span></span> <span data-ttu-id="dbe42-251">委派就類似其他程式設計語言中的函式指標，但與函式指標的不同之處是，委派是物件導向且為型別安全。</span><span class="sxs-lookup"><span data-stu-id="dbe42-251">Delegates are similar to the concept of function pointers found in some other languages, but unlike function pointers, delegates are object-oriented and type-safe.</span></span>

<span data-ttu-id="dbe42-252">類別、 結構、 介面及委派型別全都支援泛型，因此它們可以參數化與其他型別。</span><span class="sxs-lookup"><span data-stu-id="dbe42-252">Class, struct, interface and delegate types all support generics, whereby they can be parameterized with other types.</span></span>

<span data-ttu-id="dbe42-253">列舉型別是具名常數的不同類型。</span><span class="sxs-lookup"><span data-stu-id="dbe42-253">An enum type is a distinct type with named constants.</span></span> <span data-ttu-id="dbe42-254">每個列舉型別具有基礎類型，必須是其中一種八種整數類資料型別。</span><span class="sxs-lookup"><span data-stu-id="dbe42-254">Every enum type has an underlying type, which must be one of the eight integral types.</span></span> <span data-ttu-id="dbe42-255">一組值的列舉型別是一組與基礎型別的值相同。</span><span class="sxs-lookup"><span data-stu-id="dbe42-255">The set of values of an enum type is the same as the set of values of the underlying type.</span></span>

<span data-ttu-id="dbe42-256">C# 支援任何型別的單一維度及多維陣列。</span><span class="sxs-lookup"><span data-stu-id="dbe42-256">C# supports single- and multi-dimensional arrays of any type.</span></span> <span data-ttu-id="dbe42-257">不同於上述型別，陣列型別不需宣告即可使用。</span><span class="sxs-lookup"><span data-stu-id="dbe42-257">Unlike the types listed above, array types do not have to be declared before they can be used.</span></span> <span data-ttu-id="dbe42-258">而陣列型別的建構方法，是在型別名稱之後加上方括弧。</span><span class="sxs-lookup"><span data-stu-id="dbe42-258">Instead, array types are constructed by following a type name with square brackets.</span></span> <span data-ttu-id="dbe42-259">例如，`int[]`是一維陣列`int`，`int[,]`是二維陣列`int`，並`int[][]`是一維陣列的一維陣列`int`。</span><span class="sxs-lookup"><span data-stu-id="dbe42-259">For example, `int[]` is a single-dimensional array of `int`, `int[,]` is a two-dimensional array of `int`, and `int[][]` is a single-dimensional array of single-dimensional arrays of `int`.</span></span>

<span data-ttu-id="dbe42-260">可為 null 的型別也沒有宣告才能使用。</span><span class="sxs-lookup"><span data-stu-id="dbe42-260">Nullable types also do not have to be declared before they can be used.</span></span> <span data-ttu-id="dbe42-261">針對每個不可為 null 的實值型別`T`沒有對應的可為 null 型別`T?`，其中可包含其他值`null`。</span><span class="sxs-lookup"><span data-stu-id="dbe42-261">For each non-nullable value type `T` there is a corresponding nullable type `T?`, which can hold an additional value `null`.</span></span> <span data-ttu-id="dbe42-262">比方說，`int?`是可以保留任何 32 位元整數或值的型別`null`。</span><span class="sxs-lookup"><span data-stu-id="dbe42-262">For instance, `int?` is a type that can hold any 32 bit integer or the value `null`.</span></span>

<span data-ttu-id="dbe42-263">C# 類型系統統一，任何類型的值可視為物件。</span><span class="sxs-lookup"><span data-stu-id="dbe42-263">C#'s type system is unified such that a value of any type can be treated as an object.</span></span> <span data-ttu-id="dbe42-264">C# 中的每個型別都直接或間接衍生自 `object` 類別型別，而 `object` 是所有型別的基底類別。</span><span class="sxs-lookup"><span data-stu-id="dbe42-264">Every type in C# directly or indirectly derives from the `object` class type, and `object` is the ultimate base class of all types.</span></span> <span data-ttu-id="dbe42-265">參考型別的值之所以會視為物件，只是將這些值當作 `object` 型別來檢視。</span><span class="sxs-lookup"><span data-stu-id="dbe42-265">Values of reference types are treated as objects simply by viewing the values as type `object`.</span></span> <span data-ttu-id="dbe42-266">實值類型的值時，會被當作物件上，執行***boxing***並***unboxing***作業。</span><span class="sxs-lookup"><span data-stu-id="dbe42-266">Values of value types are treated as objects by performing ***boxing*** and ***unboxing*** operations.</span></span> <span data-ttu-id="dbe42-267">在下列範例中，`int` 值會轉換成 `object`，並再次轉換回 `int`。</span><span class="sxs-lookup"><span data-stu-id="dbe42-267">In the following example, an `int` value is converted to `object` and back again to `int`.</span></span>

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
<span data-ttu-id="dbe42-268">當實值型別的值轉換成輸入`object`物件執行個體，也稱為 「 box 」 配置給包含值，且值複製到該方塊。</span><span class="sxs-lookup"><span data-stu-id="dbe42-268">When a value of a value type is converted to type `object`, an object instance, also called a "box," is allocated to hold the value, and the value is copied into that box.</span></span> <span data-ttu-id="dbe42-269">相反地，當`object`參考轉換為實值型別、 核取設為所參考的物件 方塊中的正確的實值型別，而且，如果檢查成功，方塊中的值複製出來。</span><span class="sxs-lookup"><span data-stu-id="dbe42-269">Conversely, when an `object` reference is cast to a value type, a check is made that the referenced object is a box of the correct value type, and, if the check succeeds, the value in the box is copied out.</span></span>

<span data-ttu-id="dbe42-270">C# 的統一型別系統實際上意味者實值型別可能會變得 「 依需求。 」 的物件</span><span class="sxs-lookup"><span data-stu-id="dbe42-270">C#'s unified type system effectively means that value types can become objects "on demand."</span></span> <span data-ttu-id="dbe42-271">因為整合，使用 `object` 型別的一般用途程式庫就能搭配參考型別和實值型別使用。</span><span class="sxs-lookup"><span data-stu-id="dbe42-271">Because of the unification, general-purpose libraries that use type `object` can be used with both reference types and value types.</span></span>

<span data-ttu-id="dbe42-272">C# 中有數種***變數***，包括欄位、陣列元素、區域變數和參數。</span><span class="sxs-lookup"><span data-stu-id="dbe42-272">There are several kinds of ***variables*** in C#, including fields, array elements, local variables, and parameters.</span></span> <span data-ttu-id="dbe42-273">變數表示儲存位置，而每個變數具有一種類型，判斷哪些值可以儲存在變數中下, 表所示。</span><span class="sxs-lookup"><span data-stu-id="dbe42-273">Variables represent storage locations, and every variable has a type that determines what values can be stored in the variable, as shown by the following table.</span></span>


| <span data-ttu-id="dbe42-274">__變數的類型__</span><span class="sxs-lookup"><span data-stu-id="dbe42-274">__Type of Variable__</span></span>    | <span data-ttu-id="dbe42-275">__可能的內容__</span><span class="sxs-lookup"><span data-stu-id="dbe42-275">__Possible Contents__</span></span> |
|-------------------------|-----------------------|
| <span data-ttu-id="dbe42-276">不可為 Null 的實值型別</span><span class="sxs-lookup"><span data-stu-id="dbe42-276">Non-nullable value type</span></span> | <span data-ttu-id="dbe42-277">該型別的值</span><span class="sxs-lookup"><span data-stu-id="dbe42-277">A value of that exact type</span></span> |
| <span data-ttu-id="dbe42-278">可為 Null 的實值型別</span><span class="sxs-lookup"><span data-stu-id="dbe42-278">Nullable value type</span></span>     | <span data-ttu-id="dbe42-279">Null 值或該型別的值</span><span class="sxs-lookup"><span data-stu-id="dbe42-279">A null value or a value of that exact type</span></span> |
| `object`                | <span data-ttu-id="dbe42-280">Null 參考，任何參考型別中，物件的參考或任何實值型別的 boxed 值的參考</span><span class="sxs-lookup"><span data-stu-id="dbe42-280">A null reference, a reference to an object of any reference type, or a reference to a boxed value of any value type</span></span> |
| <span data-ttu-id="dbe42-281">類別型別</span><span class="sxs-lookup"><span data-stu-id="dbe42-281">Class type</span></span>              | <span data-ttu-id="dbe42-282">Null 參考，該類別類型的執行個體的參考或類別的執行個體的參考衍生自該類別類型</span><span class="sxs-lookup"><span data-stu-id="dbe42-282">A null reference, a reference to an instance of that class type, or a reference to an instance of a class derived from that class type</span></span> |
| <span data-ttu-id="dbe42-283">介面類型</span><span class="sxs-lookup"><span data-stu-id="dbe42-283">Interface type</span></span>          | <span data-ttu-id="dbe42-284">Null 參考，實作該介面型別中，類別類型的執行個體的參考或實值型別會實作該介面型別為 boxed 值的參考</span><span class="sxs-lookup"><span data-stu-id="dbe42-284">A null reference, a reference to an instance of a class type that implements that interface type, or a reference to a boxed value of a value type that implements that interface type</span></span> |
| <span data-ttu-id="dbe42-285">陣列型別</span><span class="sxs-lookup"><span data-stu-id="dbe42-285">Array type</span></span>              | <span data-ttu-id="dbe42-286">Null 參考，該陣列型別中，執行個體的參考或相容的陣列型別的執行個體的參考</span><span class="sxs-lookup"><span data-stu-id="dbe42-286">A null reference, a reference to an instance of that array type, or a reference to an instance of a compatible array type</span></span> |
| <span data-ttu-id="dbe42-287">委派類型</span><span class="sxs-lookup"><span data-stu-id="dbe42-287">Delegate type</span></span>           | <span data-ttu-id="dbe42-288">Null 參考或該委派類型的執行個體的參考</span><span class="sxs-lookup"><span data-stu-id="dbe42-288">A null reference or a reference to an instance of that delegate type</span></span> |

## <a name="expressions"></a><span data-ttu-id="dbe42-289">運算式</span><span class="sxs-lookup"><span data-stu-id="dbe42-289">Expressions</span></span>

<span data-ttu-id="dbe42-290">「運算式」是由「運算元」和「運算子」建構而成。</span><span class="sxs-lookup"><span data-stu-id="dbe42-290">***Expressions*** are constructed from ***operands*** and ***operators***.</span></span> <span data-ttu-id="dbe42-291">運算式的運算子會指出要將哪些運算套用到運算元。</span><span class="sxs-lookup"><span data-stu-id="dbe42-291">The operators of an expression indicate which operations to apply to the operands.</span></span> <span data-ttu-id="dbe42-292">運算子範例包括 `+`、`-`、`*`、`/` 及 `new`。</span><span class="sxs-lookup"><span data-stu-id="dbe42-292">Examples of operators include `+`, `-`, `*`, `/`, and `new`.</span></span> <span data-ttu-id="dbe42-293">運算元範例包括常值、欄位、區域變數及運算式。</span><span class="sxs-lookup"><span data-stu-id="dbe42-293">Examples of operands include literals, fields, local variables, and expressions.</span></span>

<span data-ttu-id="dbe42-294">當運算式包含多個運算子時，運算子的「優先順序」會控制評估個別運算子的順序。</span><span class="sxs-lookup"><span data-stu-id="dbe42-294">When an expression contains multiple operators, the ***precedence*** of the operators controls the order in which the individual operators are evaluated.</span></span> <span data-ttu-id="dbe42-295">例如，運算式 `x + y * z` 會評估為 `x + (y * z)`，因為 `*` 運算子的優先順序高於 `+` 運算子。</span><span class="sxs-lookup"><span data-stu-id="dbe42-295">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the `+` operator.</span></span>

<span data-ttu-id="dbe42-296">大多數運算子都是可以「多載」的運算子。</span><span class="sxs-lookup"><span data-stu-id="dbe42-296">Most operators can be ***overloaded***.</span></span> <span data-ttu-id="dbe42-297">運算子多載可允許針對一個運算元屬於 (或兩個運算元都屬於) 使用者定義之類別或結構型別的運算式，指定使用者定義的運算子實作。</span><span class="sxs-lookup"><span data-stu-id="dbe42-297">Operator overloading permits user-defined operator implementations to be specified for operations where one or both of the operands are of a user-defined class or struct type.</span></span>

<span data-ttu-id="dbe42-298">下表摘要說明 C# 運算子，列出運算子分類中的優先順序從最高到低排列順序。</span><span class="sxs-lookup"><span data-stu-id="dbe42-298">The following table summarizes C#'s operators, listing the operator categories in order of precedence from highest to lowest.</span></span> <span data-ttu-id="dbe42-299">相同分類中的運算子具有相等的優先順序。</span><span class="sxs-lookup"><span data-stu-id="dbe42-299">Operators in the same category have equal precedence.</span></span>


| <span data-ttu-id="dbe42-300">__分類__</span><span class="sxs-lookup"><span data-stu-id="dbe42-300">__Category__</span></span>                     | <span data-ttu-id="dbe42-301">__運算式__</span><span class="sxs-lookup"><span data-stu-id="dbe42-301">__Expression__</span></span>    | <span data-ttu-id="dbe42-302">__描述__</span><span class="sxs-lookup"><span data-stu-id="dbe42-302">__Description__</span></span> |
|----------------------------------|-------------------|-----------------|
| <span data-ttu-id="dbe42-303">主要</span><span class="sxs-lookup"><span data-stu-id="dbe42-303">Primary</span></span>                          | `x.m`             | <span data-ttu-id="dbe42-304">成員存取</span><span class="sxs-lookup"><span data-stu-id="dbe42-304">Member access</span></span> |
|                                  | `x(...)`          | <span data-ttu-id="dbe42-305">方法和委派叫用</span><span class="sxs-lookup"><span data-stu-id="dbe42-305">Method and delegate invocation</span></span> |
|                                  | `x[...]`          | <span data-ttu-id="dbe42-306">陣列和索引子存取</span><span class="sxs-lookup"><span data-stu-id="dbe42-306">Array and indexer access</span></span> |
|                                  | `x++`             | <span data-ttu-id="dbe42-307">後置遞增</span><span class="sxs-lookup"><span data-stu-id="dbe42-307">Post-increment</span></span> |
|                                  | `x--`             | <span data-ttu-id="dbe42-308">後置遞減</span><span class="sxs-lookup"><span data-stu-id="dbe42-308">Post-decrement</span></span> |
|                                  | `new T(...)`      | <span data-ttu-id="dbe42-309">建立物件和委派</span><span class="sxs-lookup"><span data-stu-id="dbe42-309">Object and delegate creation</span></span> |
|                                  | `new T(...){...}` | <span data-ttu-id="dbe42-310">使用初始設定式來建立物件</span><span class="sxs-lookup"><span data-stu-id="dbe42-310">Object creation with initializer</span></span> |
|                                  | `new {...}`       | <span data-ttu-id="dbe42-311">匿名物件初始設定式</span><span class="sxs-lookup"><span data-stu-id="dbe42-311">Anonymous object initializer</span></span> |
|                                  | `new T[...]`      | <span data-ttu-id="dbe42-312">建立陣列</span><span class="sxs-lookup"><span data-stu-id="dbe42-312">Array creation</span></span> |
|                                  | `typeof(T)`       | <span data-ttu-id="dbe42-313">取得 `T` 的 `System.Type` 物件</span><span class="sxs-lookup"><span data-stu-id="dbe42-313">Obtain `System.Type` object for `T`</span></span> |
|                                  | `checked(x)`      | <span data-ttu-id="dbe42-314">在核取的內容中評估運算式</span><span class="sxs-lookup"><span data-stu-id="dbe42-314">Evaluate expression in checked context</span></span> |
|                                  | `unchecked(x)`    | <span data-ttu-id="dbe42-315">在未核取的內容中評估運算式</span><span class="sxs-lookup"><span data-stu-id="dbe42-315">Evaluate expression in unchecked context</span></span> |
|                                  | `default(T)`      | <span data-ttu-id="dbe42-316">取得類型 `T` 的預設值</span><span class="sxs-lookup"><span data-stu-id="dbe42-316">Obtain default value of type `T`</span></span> |
|                                  | `delegate {...}`  | <span data-ttu-id="dbe42-317">匿名函式 (匿名方法)</span><span class="sxs-lookup"><span data-stu-id="dbe42-317">Anonymous function (anonymous method)</span></span> |
| <span data-ttu-id="dbe42-318">一元</span><span class="sxs-lookup"><span data-stu-id="dbe42-318">Unary</span></span>                            | `+x`              | <span data-ttu-id="dbe42-319">身分識別</span><span class="sxs-lookup"><span data-stu-id="dbe42-319">Identity</span></span> |
|                                  | `-x`              | <span data-ttu-id="dbe42-320">否定</span><span class="sxs-lookup"><span data-stu-id="dbe42-320">Negation</span></span> |
|                                  | `!x`              | <span data-ttu-id="dbe42-321">邏輯否定</span><span class="sxs-lookup"><span data-stu-id="dbe42-321">Logical negation</span></span> |
|                                  | `~x`              | <span data-ttu-id="dbe42-322">位元否定</span><span class="sxs-lookup"><span data-stu-id="dbe42-322">Bitwise negation</span></span> |
|                                  | `++x`             | <span data-ttu-id="dbe42-323">前置遞增</span><span class="sxs-lookup"><span data-stu-id="dbe42-323">Pre-increment</span></span> |
|                                  | `--x`             | <span data-ttu-id="dbe42-324">前置遞減</span><span class="sxs-lookup"><span data-stu-id="dbe42-324">Pre-decrement</span></span> |
|                                  | `(T)x`            | <span data-ttu-id="dbe42-325">將 `x` 明確轉換成類型 `T`</span><span class="sxs-lookup"><span data-stu-id="dbe42-325">Explicitly convert `x` to type `T`</span></span> |
|                                  | `await x`         | <span data-ttu-id="dbe42-326">以非同步方式等候 `x` 完成</span><span class="sxs-lookup"><span data-stu-id="dbe42-326">Asynchronously wait for `x` to complete</span></span> |
| <span data-ttu-id="dbe42-327">乘法類 (Multiplicative)</span><span class="sxs-lookup"><span data-stu-id="dbe42-327">Multiplicative</span></span>                   | `x * y`           | <span data-ttu-id="dbe42-328">乘法</span><span class="sxs-lookup"><span data-stu-id="dbe42-328">Multiplication</span></span> |
|                                  | `x / y`           | <span data-ttu-id="dbe42-329">除號</span><span class="sxs-lookup"><span data-stu-id="dbe42-329">Division</span></span> |
|                                  | `x % y`           | <span data-ttu-id="dbe42-330">餘數</span><span class="sxs-lookup"><span data-stu-id="dbe42-330">Remainder</span></span> |
| <span data-ttu-id="dbe42-331">加法類 (Additive)</span><span class="sxs-lookup"><span data-stu-id="dbe42-331">Additive</span></span>                         | `x + y`           | <span data-ttu-id="dbe42-332">加法、字串串連、委派組合</span><span class="sxs-lookup"><span data-stu-id="dbe42-332">Addition, string concatenation, delegate combination</span></span> |
|                                  | `x - y`           | <span data-ttu-id="dbe42-333">減法、委派移除</span><span class="sxs-lookup"><span data-stu-id="dbe42-333">Subtraction, delegate removal</span></span> |
| <span data-ttu-id="dbe42-334">Shift</span><span class="sxs-lookup"><span data-stu-id="dbe42-334">Shift</span></span>                            | `x << y`          | <span data-ttu-id="dbe42-335">左移</span><span class="sxs-lookup"><span data-stu-id="dbe42-335">Shift left</span></span> |
|                                  | `x >> y`          | <span data-ttu-id="dbe42-336">右移</span><span class="sxs-lookup"><span data-stu-id="dbe42-336">Shift right</span></span> |
| <span data-ttu-id="dbe42-337">關係和型別測試</span><span class="sxs-lookup"><span data-stu-id="dbe42-337">Relational and type testing</span></span>      | `x < y`           | <span data-ttu-id="dbe42-338">小於</span><span class="sxs-lookup"><span data-stu-id="dbe42-338">Less than</span></span> |
|                                  | `x > y`           | <span data-ttu-id="dbe42-339">大於</span><span class="sxs-lookup"><span data-stu-id="dbe42-339">Greater than</span></span> |
|                                  | `x <= y`          | <span data-ttu-id="dbe42-340">小於或等於</span><span class="sxs-lookup"><span data-stu-id="dbe42-340">Less than or equal</span></span> |
|                                  | `x >= y`          | <span data-ttu-id="dbe42-341">大於或等於</span><span class="sxs-lookup"><span data-stu-id="dbe42-341">Greater than or equal</span></span> |
|                                  | `x is T`          | <span data-ttu-id="dbe42-342">如果 `x` 是 `T`，便傳回 `true`，否則傳回 `false`</span><span class="sxs-lookup"><span data-stu-id="dbe42-342">Return `true` if `x` is a `T`, `false` otherwise</span></span> |
|                                  | `x as T`          | <span data-ttu-id="dbe42-343">傳回歸類為 `T` 類型的 `x`，或在 `x` 不是 `T` 的情況下傳回 `null`</span><span class="sxs-lookup"><span data-stu-id="dbe42-343">Return `x` typed as `T`, or `null` if `x` is not a `T`</span></span> |
| <span data-ttu-id="dbe42-344">相等</span><span class="sxs-lookup"><span data-stu-id="dbe42-344">Equality</span></span>                         | `x == y`          | <span data-ttu-id="dbe42-345">等於</span><span class="sxs-lookup"><span data-stu-id="dbe42-345">Equal</span></span>      |
|                                  | `x != y`          | <span data-ttu-id="dbe42-346">不等於</span><span class="sxs-lookup"><span data-stu-id="dbe42-346">Not equal</span></span> |
| <span data-ttu-id="dbe42-347">邏輯 AND</span><span class="sxs-lookup"><span data-stu-id="dbe42-347">Logical AND</span></span>                      | `x & y`           | <span data-ttu-id="dbe42-348">整數位元 AND、布林邏輯 AND</span><span class="sxs-lookup"><span data-stu-id="dbe42-348">Integer bitwise AND, boolean logical AND</span></span> |
| <span data-ttu-id="dbe42-349">邏輯 XOR</span><span class="sxs-lookup"><span data-stu-id="dbe42-349">Logical XOR</span></span>                      | `x ^ y`           | <span data-ttu-id="dbe42-350">整數位元 XOR、布林邏輯 XOR</span><span class="sxs-lookup"><span data-stu-id="dbe42-350">Integer bitwise XOR, boolean logical XOR</span></span> |
| <span data-ttu-id="dbe42-351">邏輯 OR</span><span class="sxs-lookup"><span data-stu-id="dbe42-351">Logical OR</span></span>                       | <code>x &#124; y</code> | <span data-ttu-id="dbe42-352">整數位元 OR、布林邏輯 OR</span><span class="sxs-lookup"><span data-stu-id="dbe42-352">Integer bitwise OR, boolean logical OR</span></span> |
| <span data-ttu-id="dbe42-353">條件式 AND</span><span class="sxs-lookup"><span data-stu-id="dbe42-353">Conditional AND</span></span>                  | `x && y`          | <span data-ttu-id="dbe42-354">會評估`y`只有當`x`是 `true`</span><span class="sxs-lookup"><span data-stu-id="dbe42-354">Evaluates `y` only if `x` is `true`</span></span> |
| <span data-ttu-id="dbe42-355">條件式 OR</span><span class="sxs-lookup"><span data-stu-id="dbe42-355">Conditional OR</span></span>                   | <code>x &#124;&#124; y</code> | <span data-ttu-id="dbe42-356">會評估`y`只有當`x`是 `false`</span><span class="sxs-lookup"><span data-stu-id="dbe42-356">Evaluates `y` only if `x` is `false`</span></span> |
| <span data-ttu-id="dbe42-357">Null 聯合</span><span class="sxs-lookup"><span data-stu-id="dbe42-357">Null coalescing</span></span>                  | `x ?? y`          | <span data-ttu-id="dbe42-358">評估為`y`如果`x`是`null`至`x`否則</span><span class="sxs-lookup"><span data-stu-id="dbe42-358">Evaluates to `y` if `x` is `null`, to `x` otherwise</span></span> |
| <span data-ttu-id="dbe42-359">條件式</span><span class="sxs-lookup"><span data-stu-id="dbe42-359">Conditional</span></span>                      | `x ? y : z`       | <span data-ttu-id="dbe42-360">如果 `x` 是 `true`，便評估 `y`，如果 `x` 是 `false`，則評估 `z`</span><span class="sxs-lookup"><span data-stu-id="dbe42-360">Evaluates `y` if `x` is `true`, `z` if `x` is `false`</span></span> |
| <span data-ttu-id="dbe42-361">指派或匿名函式</span><span class="sxs-lookup"><span data-stu-id="dbe42-361">Assignment or anonymous function</span></span> | `x = y`           | <span data-ttu-id="dbe42-362">指派</span><span class="sxs-lookup"><span data-stu-id="dbe42-362">Assignment</span></span> |
|                                  | `x op= y`         | <span data-ttu-id="dbe42-363">複合指派;支援的運算子 `*=` `/=` `%=` `+=` `-=` `<<=` `>>=` `&=` `^=` <code>&#124;=</code></span><span class="sxs-lookup"><span data-stu-id="dbe42-363">Compound assignment; supported operators are `*=` `/=` `%=` `+=` `-=` `<<=` `>>=` `&=` `^=` <code>&#124;=</code></span></span> |
|                                  | `(T x) => y`      | <span data-ttu-id="dbe42-364">匿名函式 (Lambda 運算式)</span><span class="sxs-lookup"><span data-stu-id="dbe42-364">Anonymous function (lambda expression)</span></span> |

## <a name="statements"></a><span data-ttu-id="dbe42-365">陳述式</span><span class="sxs-lookup"><span data-stu-id="dbe42-365">Statements</span></span>

<span data-ttu-id="dbe42-366">程式的動作是藉由***陳述式***來表達。</span><span class="sxs-lookup"><span data-stu-id="dbe42-366">The actions of a program are expressed using ***statements***.</span></span> <span data-ttu-id="dbe42-367">C# 支援數種不同類型的陳述式，其中一些是以內嵌陳述式來定義。</span><span class="sxs-lookup"><span data-stu-id="dbe42-367">C# supports several different kinds of statements, a number of which are defined in terms of embedded statements.</span></span>

<span data-ttu-id="dbe42-368">「區塊」可允許在許可單一陳述式的內容中撰寫多個陳述式。</span><span class="sxs-lookup"><span data-stu-id="dbe42-368">A ***block*** permits multiple statements to be written in contexts where a single statement is allowed.</span></span> <span data-ttu-id="dbe42-369">區塊是由在 `{` 與 `}` 分隔符號之間撰寫的陳述式清單所組成。</span><span class="sxs-lookup"><span data-stu-id="dbe42-369">A block consists of a list of statements written between the delimiters `{` and `}`.</span></span>

<span data-ttu-id="dbe42-370">「宣告陳述式」可用來宣告區域變數和常數。</span><span class="sxs-lookup"><span data-stu-id="dbe42-370">***Declaration statements*** are used to declare local variables and constants.</span></span>

<span data-ttu-id="dbe42-371">「運算式陳述式」可用來評估運算式。</span><span class="sxs-lookup"><span data-stu-id="dbe42-371">***Expression statements*** are used to evaluate expressions.</span></span> <span data-ttu-id="dbe42-372">可用來當做陳述式的運算式包含方法引動過程中，物件使用的配置`new`運算子、 指派使用`=`和複合指派運算子、 使用的遞增和遞減運算`++`和`--`運算子和 await 運算式。</span><span class="sxs-lookup"><span data-stu-id="dbe42-372">Expressions that can be used as statements include method invocations, object allocations using the `new` operator, assignments using `=` and the compound assignment operators, increment and decrement operations using the `++` and `--` operators and await expressions.</span></span>

<span data-ttu-id="dbe42-373">「選取範圍陳述式」可用來選取一些可能陳述式的其中之一，以根據某個運算式的值來執行。</span><span class="sxs-lookup"><span data-stu-id="dbe42-373">***Selection statements*** are used to select one of a number of possible statements for execution based on the value of some expression.</span></span> <span data-ttu-id="dbe42-374">在此群組中的是 `if` 和 `switch` 陳述式。</span><span class="sxs-lookup"><span data-stu-id="dbe42-374">In this group are the `if` and `switch` statements.</span></span>

<span data-ttu-id="dbe42-375">***反覆運算陳述式***用來重複執行內嵌陳述式。</span><span class="sxs-lookup"><span data-stu-id="dbe42-375">***Iteration statements*** are used to repeatedly execute an embedded statement.</span></span> <span data-ttu-id="dbe42-376">在此群組中的是 `while`、`do`、`for` 及 `foreach` 陳述式。</span><span class="sxs-lookup"><span data-stu-id="dbe42-376">In this group are the `while`, `do`, `for`, and `foreach` statements.</span></span>

<span data-ttu-id="dbe42-377">「跳躍陳述式」可用來轉移控制項。</span><span class="sxs-lookup"><span data-stu-id="dbe42-377">***Jump statements*** are used to transfer control.</span></span> <span data-ttu-id="dbe42-378">在此群組中的是 `break`、`continue`、`goto`、`throw`、`return` 及 `yield` 陳述式。</span><span class="sxs-lookup"><span data-stu-id="dbe42-378">In this group are the `break`, `continue`, `goto`, `throw`, `return`, and `yield` statements.</span></span>

<span data-ttu-id="dbe42-379">`try`...`catch` 陳述式可用來攔截在執行區塊時發生的例外狀況，而 `try`...`finally` 陳述式則可用來指定不論是否發生例外狀況都一律會執行的最終處理程式碼。</span><span class="sxs-lookup"><span data-stu-id="dbe42-379">The `try`...`catch` statement is used to catch exceptions that occur during execution of a block, and the `try`...`finally` statement is used to specify finalization code that is always executed, whether an exception occurred or not.</span></span>

<span data-ttu-id="dbe42-380">`checked`和`unchecked`陳述式用來控制溢位檢查整數型別算術運算和轉換的內容。</span><span class="sxs-lookup"><span data-stu-id="dbe42-380">The `checked` and `unchecked` statements are used to control the overflow checking context for integral-type arithmetic operations and conversions.</span></span>

<span data-ttu-id="dbe42-381">`lock` 陳述式可用來取得所指定物件的互斥鎖定、執行陳述式，然後釋放鎖定。</span><span class="sxs-lookup"><span data-stu-id="dbe42-381">The `lock` statement is used to obtain the mutual-exclusion lock for a given object, execute a statement, and then release the lock.</span></span>

<span data-ttu-id="dbe42-382">`using` 陳述式可用來取得資源、執行陳述式，然後處置該資源。</span><span class="sxs-lookup"><span data-stu-id="dbe42-382">The `using` statement is used to obtain a resource, execute a statement, and then dispose of that resource.</span></span>

<span data-ttu-id="dbe42-383">以下是每一種陳述式的範例</span><span class="sxs-lookup"><span data-stu-id="dbe42-383">Below are examples of each kind of statement</span></span>

<span data-ttu-id="dbe42-384">__本機變數宣告__</span><span class="sxs-lookup"><span data-stu-id="dbe42-384">__Local variable declarations__</span></span>

```csharp
static void Main() {
   int a;
   int b = 2, c = 3;
   a = 1;
   Console.WriteLine(a + b + c);
}
```


<span data-ttu-id="dbe42-385">__區域常數宣告__</span><span class="sxs-lookup"><span data-stu-id="dbe42-385">__Local constant declaration__</span></span>

```csharp
static void Main() {
    const float pi = 3.1415927f;
    const int r = 25;
    Console.WriteLine(pi * r * r);
}
```


<span data-ttu-id="dbe42-386">__運算式陳述式__</span><span class="sxs-lookup"><span data-stu-id="dbe42-386">__Expression statement__</span></span>

```csharp
static void Main() {
    int i;
    i = 123;                // Expression statement
    Console.WriteLine(i);   // Expression statement
    i++;                    // Expression statement
    Console.WriteLine(i);   // Expression statement
}
```

<span data-ttu-id="dbe42-387">__`if` 陳述式__</span><span class="sxs-lookup"><span data-stu-id="dbe42-387">__`if` statement__</span></span>

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


<span data-ttu-id="dbe42-388">__`switch` 陳述式__</span><span class="sxs-lookup"><span data-stu-id="dbe42-388">__`switch` statement__</span></span>

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

<span data-ttu-id="dbe42-389">__`while` 陳述式__</span><span class="sxs-lookup"><span data-stu-id="dbe42-389">__`while` statement__</span></span>

```csharp
static void Main(string[] args) {
    int i = 0;
    while (i < args.Length) {
        Console.WriteLine(args[i]);
        i++;
    }
}
```


<span data-ttu-id="dbe42-390">__`do` 陳述式__</span><span class="sxs-lookup"><span data-stu-id="dbe42-390">__`do` statement__</span></span>

```csharp
static void Main() {
    string s;
    do {
        s = Console.ReadLine();
        if (s != null) Console.WriteLine(s);
    } while (s != null);
}
```

<span data-ttu-id="dbe42-391">__`for` 陳述式__</span><span class="sxs-lookup"><span data-stu-id="dbe42-391">__`for` statement__</span></span>

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        Console.WriteLine(args[i]);
    }
}
```

<span data-ttu-id="dbe42-392">__`foreach` 陳述式__</span><span class="sxs-lookup"><span data-stu-id="dbe42-392">__`foreach` statement__</span></span>

```csharp
static void Main(string[] args) {
    foreach (string s in args) {
        Console.WriteLine(s);
    }
}
```

<span data-ttu-id="dbe42-393">__`break` 陳述式__</span><span class="sxs-lookup"><span data-stu-id="dbe42-393">__`break` statement__</span></span>

```csharp
static void Main() {
    while (true) {
        string s = Console.ReadLine();
        if (s == null) break;
        Console.WriteLine(s);
    }
}
```

<span data-ttu-id="dbe42-394">__`continue` 陳述式__</span><span class="sxs-lookup"><span data-stu-id="dbe42-394">__`continue` statement__</span></span>

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        if (args[i].StartsWith("/")) continue;
        Console.WriteLine(args[i]);
    }
}
```

<span data-ttu-id="dbe42-395">__`goto` 陳述式__</span><span class="sxs-lookup"><span data-stu-id="dbe42-395">__`goto` statement__</span></span>

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

<span data-ttu-id="dbe42-396">__`return` 陳述式__</span><span class="sxs-lookup"><span data-stu-id="dbe42-396">__`return` statement__</span></span>

```csharp
static int Add(int a, int b) {
    return a + b;
}

static void Main() {
    Console.WriteLine(Add(1, 2));
    return;
}
```

<span data-ttu-id="dbe42-397">__`yield` 陳述式__</span><span class="sxs-lookup"><span data-stu-id="dbe42-397">__`yield` statement__</span></span>

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

<span data-ttu-id="dbe42-398">__`throw` 和`try`陳述式__</span><span class="sxs-lookup"><span data-stu-id="dbe42-398">__`throw` and `try` statements__</span></span>

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

<span data-ttu-id="dbe42-399">__`checked` 和`unchecked`陳述式__</span><span class="sxs-lookup"><span data-stu-id="dbe42-399">__`checked` and `unchecked` statements__</span></span>

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

<span data-ttu-id="dbe42-400">__`lock` 陳述式__</span><span class="sxs-lookup"><span data-stu-id="dbe42-400">__`lock` statement__</span></span>

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

<span data-ttu-id="dbe42-401">__`using` 陳述式__</span><span class="sxs-lookup"><span data-stu-id="dbe42-401">__`using` statement__</span></span>

```csharp
static void Main() {
    using (TextWriter w = File.CreateText("test.txt")) {
        w.WriteLine("Line one");
        w.WriteLine("Line two");
        w.WriteLine("Line three");
    }
}
```

## <a name="classes-and-objects"></a><span data-ttu-id="dbe42-402">類別與物件</span><span class="sxs-lookup"><span data-stu-id="dbe42-402">Classes and objects</span></span>

<span data-ttu-id="dbe42-403">***類別***是 C# 最基本的型別。</span><span class="sxs-lookup"><span data-stu-id="dbe42-403">***Classes*** are the most fundamental of C#'s types.</span></span> <span data-ttu-id="dbe42-404">類別是以單一單位結合狀態 (欄位) 和動作 (方法及其他函式成員) 的資料結構。</span><span class="sxs-lookup"><span data-stu-id="dbe42-404">A class is a data structure that combines state (fields) and actions (methods and other function members) in a single unit.</span></span> <span data-ttu-id="dbe42-405">類別可以為動態建立的類別「執行個體」(稱為「物件」) 提供定義。</span><span class="sxs-lookup"><span data-stu-id="dbe42-405">A class provides a definition for dynamically created ***instances*** of the class, also known as ***objects***.</span></span> <span data-ttu-id="dbe42-406">類別支援「繼承」和「多型」，這些是可供「衍生類別」將「基底類別」延伸及特製化的機制。</span><span class="sxs-lookup"><span data-stu-id="dbe42-406">Classes support ***inheritance*** and ***polymorphism***, mechanisms whereby ***derived classes*** can extend and specialize ***base classes***.</span></span>

<span data-ttu-id="dbe42-407">建立新類別時，是使用類別宣告來建立。</span><span class="sxs-lookup"><span data-stu-id="dbe42-407">New classes are created using class declarations.</span></span> <span data-ttu-id="dbe42-408">類別宣告的開頭是一個標頭，此標頭會指定類別的屬性和修飾詞、類別的名稱、基底類別 (如果提供)，以及類別所實作的介面。</span><span class="sxs-lookup"><span data-stu-id="dbe42-408">A class declaration starts with a header that specifies the attributes and modifiers of the class, the name of the class, the base class (if given), and the interfaces implemented by the class.</span></span> <span data-ttu-id="dbe42-409">此標頭後面會接著類別主體，此主體是由在 `{` 與 `}` 分隔符號之間撰寫的成員宣告清單所組成。</span><span class="sxs-lookup"><span data-stu-id="dbe42-409">The header is followed by the class body, which consists of a list of member declarations written between the delimiters `{` and `}`.</span></span>

<span data-ttu-id="dbe42-410">以下是一個名為 `Point` 之簡單類別的宣告：</span><span class="sxs-lookup"><span data-stu-id="dbe42-410">The following is a declaration of a simple class named `Point`:</span></span>

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
<span data-ttu-id="dbe42-411">建立類別執行個體時，是使用 `new` 運算子來建立，此運算子會為新執行個體配置記憶體、叫用建構函式來將執行個體初始化，然後傳回對執行個體的參考。</span><span class="sxs-lookup"><span data-stu-id="dbe42-411">Instances of classes are created using the `new` operator, which allocates memory for a new instance, invokes a constructor to initialize the instance, and returns a reference to the instance.</span></span> <span data-ttu-id="dbe42-412">下列陳述式會建立兩個`Point`物件，並將這些物件的參考儲存在兩個變數：</span><span class="sxs-lookup"><span data-stu-id="dbe42-412">The following statements create two `Point` objects and store references to those objects in two variables:</span></span>

```
Point p1 = new Point(0, 0);
Point p2 = new Point(10, 20);
```
<span data-ttu-id="dbe42-413">不再使用物件時，會自動回收物件所佔用的記憶體。</span><span class="sxs-lookup"><span data-stu-id="dbe42-413">The memory occupied by an object is automatically reclaimed when the object is no longer in use.</span></span> <span data-ttu-id="dbe42-414">在 C# 中，既沒有必要也不可能明確地將物件解除配置。</span><span class="sxs-lookup"><span data-stu-id="dbe42-414">It is neither necessary nor possible to explicitly deallocate objects in C#.</span></span>

### <a name="members"></a><span data-ttu-id="dbe42-415">成員</span><span class="sxs-lookup"><span data-stu-id="dbe42-415">Members</span></span>

<span data-ttu-id="dbe42-416">類別的成員都是***靜態成員***或是***執行個體成員***。</span><span class="sxs-lookup"><span data-stu-id="dbe42-416">The members of a class are either ***static members*** or ***instance members***.</span></span> <span data-ttu-id="dbe42-417">靜態成員隸屬於類別，而執行個體成員則隸屬於物件 (類別的執行個體)。</span><span class="sxs-lookup"><span data-stu-id="dbe42-417">Static members belong to classes, and instance members belong to objects (instances of classes).</span></span>

<span data-ttu-id="dbe42-418">下表提供的類別可以包含的成員類型的概觀。</span><span class="sxs-lookup"><span data-stu-id="dbe42-418">The following table provides an overview of the kinds of members a class can contain.</span></span>


| <span data-ttu-id="dbe42-419">__成員__</span><span class="sxs-lookup"><span data-stu-id="dbe42-419">__Member__</span></span>   | <span data-ttu-id="dbe42-420">__描述__</span><span class="sxs-lookup"><span data-stu-id="dbe42-420">__Description__</span></span> |
|------------  |-----------------|
| <span data-ttu-id="dbe42-421">常數</span><span class="sxs-lookup"><span data-stu-id="dbe42-421">Constants</span></span>    | <span data-ttu-id="dbe42-422">與類別關聯的常數值</span><span class="sxs-lookup"><span data-stu-id="dbe42-422">Constant values associated with the class</span></span> |
| <span data-ttu-id="dbe42-423">欄位</span><span class="sxs-lookup"><span data-stu-id="dbe42-423">Fields</span></span>       | <span data-ttu-id="dbe42-424">類別的變數</span><span class="sxs-lookup"><span data-stu-id="dbe42-424">Variables of the class</span></span> |
| <span data-ttu-id="dbe42-425">方法</span><span class="sxs-lookup"><span data-stu-id="dbe42-425">Methods</span></span>      | <span data-ttu-id="dbe42-426">類別所能執行的計算和動作</span><span class="sxs-lookup"><span data-stu-id="dbe42-426">Computations and actions that can be performed by the class</span></span> |
| <span data-ttu-id="dbe42-427">屬性</span><span class="sxs-lookup"><span data-stu-id="dbe42-427">Properties</span></span>   | <span data-ttu-id="dbe42-428">與讀取和寫入具名的類別特性關聯的動作</span><span class="sxs-lookup"><span data-stu-id="dbe42-428">Actions associated with reading and writing named properties of the class</span></span> |
| <span data-ttu-id="dbe42-429">索引子</span><span class="sxs-lookup"><span data-stu-id="dbe42-429">Indexers</span></span>     | <span data-ttu-id="dbe42-430">與編製陣列之類的類別執行個體關聯的動作</span><span class="sxs-lookup"><span data-stu-id="dbe42-430">Actions associated with indexing instances of the class like an array</span></span> |
| <span data-ttu-id="dbe42-431">事件</span><span class="sxs-lookup"><span data-stu-id="dbe42-431">Events</span></span>       | <span data-ttu-id="dbe42-432">類別所能產生的通知</span><span class="sxs-lookup"><span data-stu-id="dbe42-432">Notifications that can be generated by the class</span></span> |
| <span data-ttu-id="dbe42-433">運算子</span><span class="sxs-lookup"><span data-stu-id="dbe42-433">Operators</span></span>    | <span data-ttu-id="dbe42-434">類別所支援的轉換和運算式運算子</span><span class="sxs-lookup"><span data-stu-id="dbe42-434">Conversions and expression operators supported by the class</span></span> |
| <span data-ttu-id="dbe42-435">建構函式</span><span class="sxs-lookup"><span data-stu-id="dbe42-435">Constructors</span></span> | <span data-ttu-id="dbe42-436">將類別執行個體或類別本身初始化所需的動作</span><span class="sxs-lookup"><span data-stu-id="dbe42-436">Actions required to initialize instances of the class or the class itself</span></span> |
| <span data-ttu-id="dbe42-437">解構函式</span><span class="sxs-lookup"><span data-stu-id="dbe42-437">Destructors</span></span>  | <span data-ttu-id="dbe42-438">在永久捨棄類別執行個體之前所要執行的動作</span><span class="sxs-lookup"><span data-stu-id="dbe42-438">Actions to perform before instances of the class are permanently discarded</span></span> |
| <span data-ttu-id="dbe42-439">型別</span><span class="sxs-lookup"><span data-stu-id="dbe42-439">Types</span></span>        | <span data-ttu-id="dbe42-440">類別所宣告的巢狀型別</span><span class="sxs-lookup"><span data-stu-id="dbe42-440">Nested types declared by the class</span></span> |

### <a name="accessibility"></a><span data-ttu-id="dbe42-441">協助工具選項</span><span class="sxs-lookup"><span data-stu-id="dbe42-441">Accessibility</span></span>

<span data-ttu-id="dbe42-442">類別的每個成員都有關聯的存取能力，用來控制能夠存取成員的程式文字區域。</span><span class="sxs-lookup"><span data-stu-id="dbe42-442">Each member of a class has an associated accessibility, which controls the regions of program text that are able to access the member.</span></span> <span data-ttu-id="dbe42-443">存取能力有五種可能的形式。</span><span class="sxs-lookup"><span data-stu-id="dbe42-443">There are five possible forms of accessibility.</span></span> <span data-ttu-id="dbe42-444">下表將摘要說明這些報表：</span><span class="sxs-lookup"><span data-stu-id="dbe42-444">These are summarized in the following table.</span></span>


| <span data-ttu-id="dbe42-445">__協助工具選項__</span><span class="sxs-lookup"><span data-stu-id="dbe42-445">__Accessibility__</span></span>    | <span data-ttu-id="dbe42-446">__意義__</span><span class="sxs-lookup"><span data-stu-id="dbe42-446">__Meaning__</span></span> |
|----------------------|-----------------|
| `public`             | <span data-ttu-id="dbe42-447">存取不受限制</span><span class="sxs-lookup"><span data-stu-id="dbe42-447">Access not limited</span></span> |
| `protected`          | <span data-ttu-id="dbe42-448">存取僅限於此類別或此類別所衍生的類別</span><span class="sxs-lookup"><span data-stu-id="dbe42-448">Access limited to this class or classes derived from this class</span></span> |
| `internal`           | <span data-ttu-id="dbe42-449">存取僅限於此程式</span><span class="sxs-lookup"><span data-stu-id="dbe42-449">Access limited to this program</span></span> |
| `protected internal` | <span data-ttu-id="dbe42-450">存取僅限於此程式或此類別所衍生的類別</span><span class="sxs-lookup"><span data-stu-id="dbe42-450">Access limited to this program or classes derived from this class</span></span> |
| `private`            | <span data-ttu-id="dbe42-451">存取僅限於此類別</span><span class="sxs-lookup"><span data-stu-id="dbe42-451">Access limited to this class</span></span> |

### <a name="type-parameters"></a><span data-ttu-id="dbe42-452">型別參數</span><span class="sxs-lookup"><span data-stu-id="dbe42-452">Type parameters</span></span>

<span data-ttu-id="dbe42-453">類別定義可以在類別名稱後面以角括弧括住型別參數名稱清單，來定義一組型別參數。</span><span class="sxs-lookup"><span data-stu-id="dbe42-453">A class definition may specify a set of type parameters by following the class name with angle brackets enclosing a list of type parameter names.</span></span> <span data-ttu-id="dbe42-454">型別參數可在類別宣告的主體中用來定義類別的成員。</span><span class="sxs-lookup"><span data-stu-id="dbe42-454">The type parameters can the be used in the body of the class declarations to define the members of the class.</span></span> <span data-ttu-id="dbe42-455">在下列範例中，`Pair` 的型別參數是 `TFirst` 和 `TSecond`：</span><span class="sxs-lookup"><span data-stu-id="dbe42-455">In the following example, the type parameters of `Pair` are `TFirst` and `TSecond`:</span></span>

```csharp
public class Pair<TFirst,TSecond>
{
    public TFirst First;
    public TSecond Second;
}
```
<span data-ttu-id="dbe42-456">類別類型宣告為會採用型別參數，稱為泛型類別型別。</span><span class="sxs-lookup"><span data-stu-id="dbe42-456">A class type that is declared to take type parameters is called a generic class type.</span></span> <span data-ttu-id="dbe42-457">結構、介面及委派型別也可以是泛型型別。</span><span class="sxs-lookup"><span data-stu-id="dbe42-457">Struct, interface and delegate types can also be generic.</span></span>

<span data-ttu-id="dbe42-458">使用泛型類別時，必須為每個型別參數提供型別參數：</span><span class="sxs-lookup"><span data-stu-id="dbe42-458">When the generic class is used, type arguments must be provided for each of the type parameters:</span></span>

```csharp
Pair<int,string> pair = new Pair<int,string> { First = 1, Second = "two" };
int i = pair.First;     // TFirst is int
string s = pair.Second; // TSecond is string
```
<span data-ttu-id="dbe42-459">泛型型別與型別引數提供，例如`Pair<int,string>
    `以上版本，稱為建構的型別。</span><span class="sxs-lookup"><span data-stu-id="dbe42-459">A generic type with type arguments provided, like `Pair<int,string>
    ` above, is called a constructed type.</span></span>

### <a name="base-classes"></a><span data-ttu-id="dbe42-460">基底類別</span><span class="sxs-lookup"><span data-stu-id="dbe42-460">Base classes</span></span>

<span data-ttu-id="dbe42-461">類別宣告可以在類別名稱和型別參數後面加上冒號和基底類別的名稱，來指定基底類別。</span><span class="sxs-lookup"><span data-stu-id="dbe42-461">A class declaration may specify a base class by following the class name and type parameters with a colon and the name of the base class.</span></span> <span data-ttu-id="dbe42-462">省略基底類別規格即等同於衍生自類型 `object`。</span><span class="sxs-lookup"><span data-stu-id="dbe42-462">Omitting a base class specification is the same as deriving from type `object`.</span></span> <span data-ttu-id="dbe42-463">在下列範例中，`Point3D` 的基底類別是 `Point`，而 `Point` 的基底類別是 `object`：</span><span class="sxs-lookup"><span data-stu-id="dbe42-463">In the following example, the base class of `Point3D` is `Point`, and the base class of `Point` is `object`:</span></span>

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
<span data-ttu-id="dbe42-464">類別會繼承其基底類別的成員。</span><span class="sxs-lookup"><span data-stu-id="dbe42-464">A class inherits the members of its base class.</span></span> <span data-ttu-id="dbe42-465">繼承意謂類別以隱含方式包含其基底類別，除了執行個體和靜態建構函式，以及基底類別的解構函式的所有成員。</span><span class="sxs-lookup"><span data-stu-id="dbe42-465">Inheritance means that a class implicitly contains all members of its base class, except for the instance and static constructors, and the destructors of the base class.</span></span> <span data-ttu-id="dbe42-466">衍生類別可以在其繼承的成員中新增新的成員，但無法移除所繼承成員的定義。</span><span class="sxs-lookup"><span data-stu-id="dbe42-466">A derived class can add new members to those it inherits, but it cannot remove the definition of an inherited member.</span></span> <span data-ttu-id="dbe42-467">在先前的範例中，`Point3D` 會從 `Point` 繼承 `x` 和 `y` 欄位，而每個 `Point3D` 執行個體都會包含 `x`、`y` 及 `z` 這三個欄位。</span><span class="sxs-lookup"><span data-stu-id="dbe42-467">In the previous example, `Point3D` inherits the `x` and `y` fields from `Point`, and every `Point3D` instance contains three fields, `x`, `y`, and `z`.</span></span>

<span data-ttu-id="dbe42-468">在類別型別到其任何基底類別型別之間都存在著隱含轉換。</span><span class="sxs-lookup"><span data-stu-id="dbe42-468">An implicit conversion exists from a class type to any of its base class types.</span></span> <span data-ttu-id="dbe42-469">因此，類別型別的變數可以參考該類別的執行個體，或任何衍生類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="dbe42-469">Therefore, a variable of a class type can reference an instance of that class or an instance of any derived class.</span></span> <span data-ttu-id="dbe42-470">例如，以先前的類別宣告為例，`Point` 型別的變數可以參考 `Point` 或 `Point3D`：</span><span class="sxs-lookup"><span data-stu-id="dbe42-470">For example, given the previous class declarations, a variable of type `Point` can reference either a `Point` or a `Point3D`:</span></span>

```csharp
Point a = new Point(10, 20);
Point b = new Point3D(10, 20, 30);
```

### <a name="fields"></a><span data-ttu-id="dbe42-471">欄位</span><span class="sxs-lookup"><span data-stu-id="dbe42-471">Fields</span></span>

<span data-ttu-id="dbe42-472">欄位是具有類別或類別的執行個體相關聯的變數。</span><span class="sxs-lookup"><span data-stu-id="dbe42-472">A field is a variable that is associated with a class or with an instance of a class.</span></span>

<span data-ttu-id="dbe42-473">欄位宣告`static`修飾詞定義***靜態欄位***。</span><span class="sxs-lookup"><span data-stu-id="dbe42-473">A field declared with the `static` modifier defines a ***static field***.</span></span> <span data-ttu-id="dbe42-474">靜態欄位只會識別一個儲存位置。</span><span class="sxs-lookup"><span data-stu-id="dbe42-474">A static field identifies exactly one storage location.</span></span> <span data-ttu-id="dbe42-475">不論建立多少個類別執行個體，都只會有一個靜態欄位複本。</span><span class="sxs-lookup"><span data-stu-id="dbe42-475">No matter how many instances of a class are created, there is only ever one copy of a static field.</span></span>

<span data-ttu-id="dbe42-476">欄位宣告不含`static`修飾詞定義***執行個體欄位***。</span><span class="sxs-lookup"><span data-stu-id="dbe42-476">A field declared without the `static` modifier defines an ***instance field***.</span></span> <span data-ttu-id="dbe42-477">每個類別執行個體都包含一個該類別所有執行個體欄位的個別複本。</span><span class="sxs-lookup"><span data-stu-id="dbe42-477">Every instance of a class contains a separate copy of all the instance fields of that class.</span></span>

<span data-ttu-id="dbe42-478">在下列範例中，每個 `Color` 類別執行個體都具有 `r`、`g` 及 `b` 執行個體欄位的個別複本，但只有一個 `Black`、`White`、`Red`、`Green` 及 `Blue` 靜態欄位的複本：</span><span class="sxs-lookup"><span data-stu-id="dbe42-478">In the following example, each instance of the `Color` class has a separate copy of the `r`, `g`, and `b` instance fields, but there is only one copy of the `Black`, `White`, `Red`, `Green`, and `Blue` static fields:</span></span>

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
<span data-ttu-id="dbe42-479">如先前的範例所示，可以使用 `readonly` 修飾詞來宣告「唯讀欄位」。</span><span class="sxs-lookup"><span data-stu-id="dbe42-479">As shown in the previous example, ***read-only fields*** may be declared with a `readonly` modifier.</span></span> <span data-ttu-id="dbe42-480">指派給`readonly`欄位的欄位的宣告，或在相同類別中的建構函式才會發生。</span><span class="sxs-lookup"><span data-stu-id="dbe42-480">Assignment to a `readonly` field can only occur as part of the field's declaration or in a constructor in the same class.</span></span>

### <a name="methods"></a><span data-ttu-id="dbe42-481">方法</span><span class="sxs-lookup"><span data-stu-id="dbe42-481">Methods</span></span>

<span data-ttu-id="dbe42-482">「方法」是實作物件或類別所能執行之計算或動作的成員。</span><span class="sxs-lookup"><span data-stu-id="dbe42-482">A ***method*** is a member that implements a computation or action that can be performed by an object or class.</span></span> <span data-ttu-id="dbe42-483">存取「靜態方法」時，是透過類別來存取。</span><span class="sxs-lookup"><span data-stu-id="dbe42-483">***Static methods*** are accessed through the class.</span></span> <span data-ttu-id="dbe42-484">存取「執行個體方法」時，是透過類別的執行個體來存取。</span><span class="sxs-lookup"><span data-stu-id="dbe42-484">***Instance methods*** are accessed through instances of the class.</span></span>

<span data-ttu-id="dbe42-485">方法有 （可能是空的） 清單***參數***，代表值或變數的參考傳遞給方法，以及***傳回型別***，指定所計算並傳回值的型別此方法。</span><span class="sxs-lookup"><span data-stu-id="dbe42-485">Methods have a (possibly empty) list of ***parameters***, which represent values or variable references passed to the method, and a ***return type***, which specifies the type of the value computed and returned by the method.</span></span> <span data-ttu-id="dbe42-486">方法的傳回型別是`void`如果它不會傳回值。</span><span class="sxs-lookup"><span data-stu-id="dbe42-486">A method's return type is `void` if it does not return a value.</span></span>

<span data-ttu-id="dbe42-487">與型別相同，方法也可能有一組型別參數，而呼叫方法時，必須為這些參數指定型別引數。</span><span class="sxs-lookup"><span data-stu-id="dbe42-487">Like types, methods may also have a set of type parameters, for which type arguments must be specified when the method is called.</span></span> <span data-ttu-id="dbe42-488">與型別不同的是，型別引數通常可以從方法呼叫的引數推斷，而不需要明確指定。</span><span class="sxs-lookup"><span data-stu-id="dbe42-488">Unlike types, the type arguments can often be inferred from the arguments of a method call and need not be explicitly given.</span></span>

<span data-ttu-id="dbe42-489">在宣告方法的類別中，方法的「簽章」必須是唯一的。</span><span class="sxs-lookup"><span data-stu-id="dbe42-489">The ***signature*** of a method must be unique in the class in which the method is declared.</span></span> <span data-ttu-id="dbe42-490">方法的簽章是由方法的名稱、型別參數的數目以及其參數的數目、修飾詞和型別所組成。</span><span class="sxs-lookup"><span data-stu-id="dbe42-490">The signature of a method consists of the name of the method, the number of type parameters and the number, modifiers, and types of its parameters.</span></span> <span data-ttu-id="dbe42-491">方法的簽章並不包括傳回型別。</span><span class="sxs-lookup"><span data-stu-id="dbe42-491">The signature of a method does not include the return type.</span></span>

#### <a name="parameters"></a><span data-ttu-id="dbe42-492">參數</span><span class="sxs-lookup"><span data-stu-id="dbe42-492">Parameters</span></span>

<span data-ttu-id="dbe42-493">參數是用來將值或變數參考傳遞給方法。</span><span class="sxs-lookup"><span data-stu-id="dbe42-493">Parameters are used to pass values or variable references to methods.</span></span> <span data-ttu-id="dbe42-494">方法的參數會從叫用方法時所指定的「引數」取得其實際值。</span><span class="sxs-lookup"><span data-stu-id="dbe42-494">The parameters of a method get their actual values from the ***arguments*** that are specified when the method is invoked.</span></span> <span data-ttu-id="dbe42-495">參數有四種：值參數、參考參數、輸出參數，以及參數陣列。</span><span class="sxs-lookup"><span data-stu-id="dbe42-495">There are four kinds of parameters: value parameters, reference parameters, output parameters, and parameter arrays.</span></span>

<span data-ttu-id="dbe42-496">「值參數」適用於傳遞輸入參數。</span><span class="sxs-lookup"><span data-stu-id="dbe42-496">A ***value parameter*** is used for input parameter passing.</span></span> <span data-ttu-id="dbe42-497">值參數會對應至區域變數，此變數會從針對參數傳遞的引數取得其初始值。</span><span class="sxs-lookup"><span data-stu-id="dbe42-497">A value parameter corresponds to a local variable that gets its initial value from the argument that was passed for the parameter.</span></span> <span data-ttu-id="dbe42-498">對值參數進行修改並不會影響已針對參數傳遞的引數。</span><span class="sxs-lookup"><span data-stu-id="dbe42-498">Modifications to a value parameter do not affect the argument that was passed for the parameter.</span></span>

<span data-ttu-id="dbe42-499">只要指定預設值，值參數便可以成為選用參數，如此即可省略對應的引數。</span><span class="sxs-lookup"><span data-stu-id="dbe42-499">Value parameters can be optional, by specifying a default value so that corresponding arguments can be omitted.</span></span>

<span data-ttu-id="dbe42-500">「參考參數」同時適用於傳遞輸入和輸出參數。</span><span class="sxs-lookup"><span data-stu-id="dbe42-500">A ***reference parameter*** is used for both input and output parameter passing.</span></span> <span data-ttu-id="dbe42-501">針對參考參數傳遞的引數必須是變數，而在執行方法的期間，參考參數會代表與引數變數相同的儲存位置。</span><span class="sxs-lookup"><span data-stu-id="dbe42-501">The argument passed for a reference parameter must be a variable, and during execution of the method, the reference parameter represents the same storage location as the argument variable.</span></span> <span data-ttu-id="dbe42-502">宣告參考參數時，是使用 `ref` 修飾詞來宣告。</span><span class="sxs-lookup"><span data-stu-id="dbe42-502">A reference parameter is declared with the `ref` modifier.</span></span> <span data-ttu-id="dbe42-503">下列範例示範 `ref` 參數的用法。</span><span class="sxs-lookup"><span data-stu-id="dbe42-503">The following example shows the use of `ref` parameters.</span></span>

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
<span data-ttu-id="dbe42-504">「輸出參數」適用於傳遞輸出參數。</span><span class="sxs-lookup"><span data-stu-id="dbe42-504">An ***output parameter*** is used for output parameter passing.</span></span> <span data-ttu-id="dbe42-505">輸出參數與參考參數類似，不同之處在於呼叫端所提供引數的初始值並不重要。</span><span class="sxs-lookup"><span data-stu-id="dbe42-505">An output parameter is similar to a reference parameter except that the initial value of the caller-provided argument is unimportant.</span></span> <span data-ttu-id="dbe42-506">宣告輸出參數時，是使用 `out` 修飾詞來宣告。</span><span class="sxs-lookup"><span data-stu-id="dbe42-506">An output parameter is declared with the `out` modifier.</span></span> <span data-ttu-id="dbe42-507">下列範例示範 `out` 參數的用法。</span><span class="sxs-lookup"><span data-stu-id="dbe42-507">The following example shows the use of `out` parameters.</span></span>

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
<span data-ttu-id="dbe42-508">「參數陣列」可允許將數目不固定的引數傳遞給方法。</span><span class="sxs-lookup"><span data-stu-id="dbe42-508">A ***parameter array*** permits a variable number of arguments to be passed to a method.</span></span> <span data-ttu-id="dbe42-509">宣告參數陣列時，是使用 `params` 修飾詞來宣告。</span><span class="sxs-lookup"><span data-stu-id="dbe42-509">A parameter array is declared with the `params` modifier.</span></span> <span data-ttu-id="dbe42-510">只有方法的最後一個參數可以是參數陣列，而參數陣列的型別必須是單一維度陣列型別。</span><span class="sxs-lookup"><span data-stu-id="dbe42-510">Only the last parameter of a method can be a parameter array, and the type of a parameter array must be a single-dimensional array type.</span></span> <span data-ttu-id="dbe42-511">`Write`並`WriteLine`方法`System.Console`類別是參數陣列用法的好例子。</span><span class="sxs-lookup"><span data-stu-id="dbe42-511">The `Write` and `WriteLine` methods of the `System.Console` class are good examples of parameter array usage.</span></span> <span data-ttu-id="dbe42-512">其宣告方式如下。</span><span class="sxs-lookup"><span data-stu-id="dbe42-512">They are declared as follows.</span></span>

```csharp
public class Console
{
    public static void Write(string fmt, params object[] args) {...}
    public static void WriteLine(string fmt, params object[] args) {...}
    ...
}
```
<span data-ttu-id="dbe42-513">在使用參數陣列的方法內，參數陣列的行為與陣列型別的一般參數完全相同。</span><span class="sxs-lookup"><span data-stu-id="dbe42-513">Within a method that uses a parameter array, the parameter array behaves exactly like a regular parameter of an array type.</span></span> <span data-ttu-id="dbe42-514">不過，在叫用含有參數陣列的方法時，可以傳遞單一的參數陣列型別引數，或是傳遞任意數目的參數陣列元素型別引數。</span><span class="sxs-lookup"><span data-stu-id="dbe42-514">However, in an invocation of a method with a parameter array, it is possible to pass either a single argument of the parameter array type or any number of arguments of the element type of the parameter array.</span></span> <span data-ttu-id="dbe42-515">在後者的案例中，會自動建立陣列執行個體並以指定的引數將其初始化。</span><span class="sxs-lookup"><span data-stu-id="dbe42-515">In the latter case, an array instance is automatically created and initialized with the given arguments.</span></span> <span data-ttu-id="dbe42-516">以下範例</span><span class="sxs-lookup"><span data-stu-id="dbe42-516">This example</span></span>

```csharp
Console.WriteLine("x={0} y={1} z={2}", x, y, z);
```
<span data-ttu-id="dbe42-517">等同於撰寫下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="dbe42-517">is equivalent to writing the following.</span></span>

```csharp
string s = "x={0} y={1} z={2}";
object[] args = new object[3];
args[0] = x;
args[1] = y;
args[2] = z;
Console.WriteLine(s, args);
```

#### <a name="method-body-and-local-variables"></a><span data-ttu-id="dbe42-518">方法主體和區域變數</span><span class="sxs-lookup"><span data-stu-id="dbe42-518">Method body and local variables</span></span>

<span data-ttu-id="dbe42-519">方法的主體會指定要叫用方法時執行的陳述式。</span><span class="sxs-lookup"><span data-stu-id="dbe42-519">A method's body specifies the statements to execute when the method is invoked.</span></span>

<span data-ttu-id="dbe42-520">方法主體可以宣告方法叫用專屬的變數。</span><span class="sxs-lookup"><span data-stu-id="dbe42-520">A method body can declare variables that are specific to the invocation of the method.</span></span> <span data-ttu-id="dbe42-521">這類變數稱為「區域變數」。</span><span class="sxs-lookup"><span data-stu-id="dbe42-521">Such variables are called ***local variables***.</span></span> <span data-ttu-id="dbe42-522">區域變數宣告會指定型別名稱、變數名稱，還可能指定初始值。</span><span class="sxs-lookup"><span data-stu-id="dbe42-522">A local variable declaration specifies a type name, a variable name, and possibly an initial value.</span></span> <span data-ttu-id="dbe42-523">下列範例會宣告一個初始值為零的區域變數 `i`，以及一個沒有初始值的區域變數 `j`。</span><span class="sxs-lookup"><span data-stu-id="dbe42-523">The following example declares a local variable `i` with an initial value of zero and a local variable `j` with no initial value.</span></span>

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
<span data-ttu-id="dbe42-524">C# 要求必須「明確指派」區域變數，才能取得其值。</span><span class="sxs-lookup"><span data-stu-id="dbe42-524">C# requires a local variable to be ***definitely assigned*** before its value can be obtained.</span></span> <span data-ttu-id="dbe42-525">例如，如果先前 `i` 的宣告並未包含初始值，編譯器就會在 `i` 後續被使用時回報錯誤，因為在這些時間點並不會在程式中明確指派 `i`。</span><span class="sxs-lookup"><span data-stu-id="dbe42-525">For example, if the declaration of the previous `i` did not include an initial value, the compiler would report an error for the subsequent usages of `i` because `i` would not be definitely assigned at those points in the program.</span></span>

<span data-ttu-id="dbe42-526">方法可以使用 `return` 陳述式將控制權交還給其呼叫端。</span><span class="sxs-lookup"><span data-stu-id="dbe42-526">A method can use `return` statements to return control to its caller.</span></span> <span data-ttu-id="dbe42-527">在傳回 `void` 的方法中，`return` 陳述式不能指定運算式。</span><span class="sxs-lookup"><span data-stu-id="dbe42-527">In a method returning `void`, `return` statements cannot specify an expression.</span></span> <span data-ttu-id="dbe42-528">在方法中傳回非`void`，`return`陳述式必須包含計算傳回值的運算式。</span><span class="sxs-lookup"><span data-stu-id="dbe42-528">In a method returning non-`void`, `return` statements must include an expression that computes the return value.</span></span>

#### <a name="static-and-instance-methods"></a><span data-ttu-id="dbe42-529">靜態和執行個體方法</span><span class="sxs-lookup"><span data-stu-id="dbe42-529">Static and instance methods</span></span>

<span data-ttu-id="dbe42-530">方法宣告`static`修飾詞***靜態方法***。</span><span class="sxs-lookup"><span data-stu-id="dbe42-530">A method declared with a `static` modifier is a ***static method***.</span></span> <span data-ttu-id="dbe42-531">靜態方法不會在特定的執行個體上運作，並且只能直接存取靜態成員。</span><span class="sxs-lookup"><span data-stu-id="dbe42-531">A static method does not operate on a specific instance and can only directly access static members.</span></span>

<span data-ttu-id="dbe42-532">方法宣告為不含`static`修飾詞***執行個體方法***。</span><span class="sxs-lookup"><span data-stu-id="dbe42-532">A method declared without a `static` modifier is an ***instance method***.</span></span> <span data-ttu-id="dbe42-533">執行個體方法會在特定的執行個體上運作，並且既可存取靜態成員也可存取執行個體成員。</span><span class="sxs-lookup"><span data-stu-id="dbe42-533">An instance method operates on a specific instance and can access both static and instance members.</span></span> <span data-ttu-id="dbe42-534">透過 `this` 可以明確存取叫用執行個體方法時所在的執行個體。</span><span class="sxs-lookup"><span data-stu-id="dbe42-534">The instance on which an instance method was invoked can be explicitly accessed as `this`.</span></span> <span data-ttu-id="dbe42-535">參考靜態方法中的 `this` 會產生錯誤。</span><span class="sxs-lookup"><span data-stu-id="dbe42-535">It is an error to refer to `this` in a static method.</span></span>

<span data-ttu-id="dbe42-536">下列 `Entity` 類別同時含有靜態成員和執行個體成員。</span><span class="sxs-lookup"><span data-stu-id="dbe42-536">The following `Entity` class has both static and instance members.</span></span>

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
<span data-ttu-id="dbe42-537">每個 `Entity` 執行個體都包含一個序號 (並且可能包含一些其他這裡未顯示的資訊)。</span><span class="sxs-lookup"><span data-stu-id="dbe42-537">Each `Entity` instance contains a serial number (and presumably some other information that is not shown here).</span></span> <span data-ttu-id="dbe42-538">`Entity` 建構函式 (類似於執行個體方法) 會將具有下一個可用序號的新執行個體初始化。</span><span class="sxs-lookup"><span data-stu-id="dbe42-538">The `Entity` constructor (which is like an instance method) initializes the new instance with the next available serial number.</span></span> <span data-ttu-id="dbe42-539">由於此建構函式式執行個體成員，因此它既可存取 `serialNo` 執行個體欄位，也可存取 `nextSerialNo` 靜態欄位。</span><span class="sxs-lookup"><span data-stu-id="dbe42-539">Because the constructor is an instance member, it is permitted to access both the `serialNo` instance field and the `nextSerialNo` static field.</span></span>

<span data-ttu-id="dbe42-540">`GetNextSerialNo` 和 `SetNextSerialNo` 靜態方法可以存取 `nextSerialNo` 靜態欄位，但如果直接存取 `serialNo` 執行個體欄位，則會產生錯誤。</span><span class="sxs-lookup"><span data-stu-id="dbe42-540">The `GetNextSerialNo` and `SetNextSerialNo` static methods can access the `nextSerialNo` static field, but it would be an error for them to directly access the `serialNo` instance field.</span></span>

<span data-ttu-id="dbe42-541">下列範例示範使用`Entity`類別。</span><span class="sxs-lookup"><span data-stu-id="dbe42-541">The following example shows the use of the `Entity` class.</span></span>

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
<span data-ttu-id="dbe42-542">請注意，`SetNextSerialNo` 和 `GetNextSerialNo` 靜態方法的叫用位置是在類別上，而 `GetSerialNo` 執行個體方法的叫用位置則是在類別的執行個體上。</span><span class="sxs-lookup"><span data-stu-id="dbe42-542">Note that the `SetNextSerialNo` and `GetNextSerialNo` static methods are invoked on the class whereas the `GetSerialNo` instance method is invoked on instances of the class.</span></span>

#### <a name="virtual-override-and-abstract-methods"></a><span data-ttu-id="dbe42-543">虛擬、覆寫及抽象方法</span><span class="sxs-lookup"><span data-stu-id="dbe42-543">Virtual, override, and abstract methods</span></span>

<span data-ttu-id="dbe42-544">當執行個體方法宣告包含 `virtual` 修飾詞時，該方法即稱為「虛擬方法」。</span><span class="sxs-lookup"><span data-stu-id="dbe42-544">When an instance method declaration includes a `virtual` modifier, the method is said to be a ***virtual method***.</span></span> <span data-ttu-id="dbe42-545">若未`virtual`修飾詞存在，則此方法即稱為***非虛擬方法***。</span><span class="sxs-lookup"><span data-stu-id="dbe42-545">When no `virtual` modifier is present, the method is said to be a ***non-virtual method***.</span></span>

<span data-ttu-id="dbe42-546">叫用虛擬方法時，叫用所針對之執行個體的「執行階段型別」會決定要叫用的實際方法實作。</span><span class="sxs-lookup"><span data-stu-id="dbe42-546">When a virtual method is invoked, the ***run-time type*** of the instance for which that invocation takes place determines the actual method implementation to invoke.</span></span> <span data-ttu-id="dbe42-547">在非虛擬方法叫用中，決定因素則是執行個體的「編譯階段型別」。</span><span class="sxs-lookup"><span data-stu-id="dbe42-547">In a nonvirtual method invocation, the ***compile-time type*** of the instance is the determining factor.</span></span>

<span data-ttu-id="dbe42-548">在衍生類別中可以「覆寫」虛擬方法。</span><span class="sxs-lookup"><span data-stu-id="dbe42-548">A virtual method can be ***overridden*** in a derived class.</span></span> <span data-ttu-id="dbe42-549">當執行個體方法宣告包含`override`修飾詞，此方法會覆寫具有相同的簽章的繼承虛擬方法。</span><span class="sxs-lookup"><span data-stu-id="dbe42-549">When an instance method declaration includes an `override` modifier, the method overrides an inherited virtual method with the same signature.</span></span> <span data-ttu-id="dbe42-550">虛擬方法宣告會導入新的方法，而覆寫方法宣告則是會提供現有已繼承之虛擬方法的新實作，來將該方法特製化。</span><span class="sxs-lookup"><span data-stu-id="dbe42-550">Whereas a virtual method declaration introduces a new method, an override method declaration specializes an existing inherited virtual method by providing a new implementation of that method.</span></span>

<span data-ttu-id="dbe42-551">***抽象***方法是虛擬的方法沒有實作。</span><span class="sxs-lookup"><span data-stu-id="dbe42-551">An ***abstract*** method is a virtual method with no implementation.</span></span> <span data-ttu-id="dbe42-552">抽象方法以宣告`abstract`修飾詞，但只有在也會宣告一個類別則允許`abstract`。</span><span class="sxs-lookup"><span data-stu-id="dbe42-552">An abstract method is declared with the `abstract` modifier and is permitted only in a class that is also declared `abstract`.</span></span> <span data-ttu-id="dbe42-553">抽象方法必須在每個非抽象的衍生類別中被覆寫。</span><span class="sxs-lookup"><span data-stu-id="dbe42-553">An abstract method must be overridden in every non-abstract derived class.</span></span>

<span data-ttu-id="dbe42-554">下列範例會宣告一個抽象類別 `Expression` 和三個衍生的類別 `Constant`、`VariableReference` 及 `Operation`，前者代表一個運算式樹狀架構節點，後者則會實作常數、變數參考及算數運算式的運算式樹狀架構節點。</span><span class="sxs-lookup"><span data-stu-id="dbe42-554">The following example declares an abstract class, `Expression`, which represents an expression tree node, and three derived classes, `Constant`, `VariableReference`, and `Operation`, which implement expression tree nodes for constants, variable references, and arithmetic operations.</span></span> <span data-ttu-id="dbe42-555">(這是類似，但不是會與混淆運算式樹狀架構型別中介紹[運算式樹狀架構類型](types.md#expression-tree-types))。</span><span class="sxs-lookup"><span data-stu-id="dbe42-555">(This is similar to, but not to be confused with the expression tree types introduced in [Expression tree types](types.md#expression-tree-types)).</span></span>

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
<span data-ttu-id="dbe42-556">先前的四個類別可用來建構算數運算式的模型。</span><span class="sxs-lookup"><span data-stu-id="dbe42-556">The previous four classes can be used to model arithmetic expressions.</span></span> <span data-ttu-id="dbe42-557">例如，在使用這些類別執行個體的情況下，可以將運算式 `x + 3` 表示如下。</span><span class="sxs-lookup"><span data-stu-id="dbe42-557">For example, using instances of these classes, the expression `x + 3` can be represented as follows.</span></span>

```csharp
Expression e = new Operation(
    new VariableReference("x"),
    '+',
    new Constant(3));
```
<span data-ttu-id="dbe42-558">系統會叫用 `Expression` 執行個體的 `Evaluate` 方法來評估指定的運算式並產生 `double` 值。</span><span class="sxs-lookup"><span data-stu-id="dbe42-558">The `Evaluate` method of an `Expression` instance is invoked to evaluate the given expression and produce a `double` value.</span></span> <span data-ttu-id="dbe42-559">此方法會採用當做引數`Hashtable`，其中包含變數名稱 （作為項目的索引鍵） 和值 （作為項目的值）。</span><span class="sxs-lookup"><span data-stu-id="dbe42-559">The method takes as an argument a `Hashtable` that contains variable names (as keys of the entries) and values (as values of the entries).</span></span> <span data-ttu-id="dbe42-560">`Evaluate`方法是虛擬的抽象方法，這表示非抽象衍生的類別必須覆寫它提供實際的實作。</span><span class="sxs-lookup"><span data-stu-id="dbe42-560">The `Evaluate` method is a virtual abstract method, meaning that non-abstract derived classes must override it to provide an actual implementation.</span></span>

<span data-ttu-id="dbe42-561">`Constant` 的 `Evaluate` 實作會直接傳回預存的常數。</span><span class="sxs-lookup"><span data-stu-id="dbe42-561">A `Constant`'s implementation of `Evaluate` simply returns the stored constant.</span></span> <span data-ttu-id="dbe42-562">A`VariableReference`的實作會查詢雜湊表中的變數名稱，並傳回結果的值。</span><span class="sxs-lookup"><span data-stu-id="dbe42-562">A `VariableReference`'s implementation looks up the variable name in the hashtable and returns the resulting value.</span></span> <span data-ttu-id="dbe42-563">`Operation` 的實作會先評估左邊和右邊的運算元 (透過以遞迴方式叫用其 `Evaluate` 方法)，然後才執行指定的算數運算。</span><span class="sxs-lookup"><span data-stu-id="dbe42-563">An `Operation`'s implementation first evaluates the left and right operands (by recursively invoking their `Evaluate` methods) and then performs the given arithmetic operation.</span></span>

<span data-ttu-id="dbe42-564">下列程式會使用 `Expression` 類別來評估不同 `x` 和 `y` 值的 `x * (y + 2)` 運算式。</span><span class="sxs-lookup"><span data-stu-id="dbe42-564">The following program uses the `Expression` classes to evaluate the expression `x * (y + 2)` for different values of `x` and `y`.</span></span>

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

#### <a name="method-overloading"></a><span data-ttu-id="dbe42-565">方法多載</span><span class="sxs-lookup"><span data-stu-id="dbe42-565">Method overloading</span></span>

<span data-ttu-id="dbe42-566">方法「多載」可允許相同類別中的多個方法擁有相同的名稱，只要它們的簽章是唯一的即可。</span><span class="sxs-lookup"><span data-stu-id="dbe42-566">Method ***overloading*** permits multiple methods in the same class to have the same name as long as they have unique signatures.</span></span> <span data-ttu-id="dbe42-567">編譯多載方法的叫用時，編譯器會使用「多載解析」來判斷要叫用的特定方法。</span><span class="sxs-lookup"><span data-stu-id="dbe42-567">When compiling an invocation of an overloaded method, the compiler uses ***overload resolution*** to determine the specific method to invoke.</span></span> <span data-ttu-id="dbe42-568">多載解析會尋找一個與引數最相符的方法，或者，如果找不到任何一個最相符的方法，則會回報錯誤。</span><span class="sxs-lookup"><span data-stu-id="dbe42-568">Overload resolution finds the one method that best matches the arguments or reports an error if no single best match can be found.</span></span> <span data-ttu-id="dbe42-569">下列範例示範多載解析的實際運作情況。</span><span class="sxs-lookup"><span data-stu-id="dbe42-569">The following example shows overload resolution in effect.</span></span> <span data-ttu-id="dbe42-570">`Main` 方法中每項叫用的註解會顯示實際叫用的方法是哪一個。</span><span class="sxs-lookup"><span data-stu-id="dbe42-570">The comment for each invocation in the `Main` method shows which method is actually invoked.</span></span>

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
<span data-ttu-id="dbe42-571">如範例所示，透過將引數明確轉換成確切的參數型別和 (或) 明確提供型別引數，即一律可以選取特定的方法。</span><span class="sxs-lookup"><span data-stu-id="dbe42-571">As shown by the example, a particular method can always be selected by explicitly casting the arguments to the exact parameter types and/or explicitly supplying type arguments.</span></span>

### <a name="other-function-members"></a><span data-ttu-id="dbe42-572">其他函式成員</span><span class="sxs-lookup"><span data-stu-id="dbe42-572">Other function members</span></span>

<span data-ttu-id="dbe42-573">包含可執行程式碼的成員統稱為類別的「函式成員」。</span><span class="sxs-lookup"><span data-stu-id="dbe42-573">Members that contain executable code are collectively known as the ***function members*** of a class.</span></span> <span data-ttu-id="dbe42-574">上節中所述的方法是主要的函式成員類型。</span><span class="sxs-lookup"><span data-stu-id="dbe42-574">The preceding section describes methods, which are the primary kind of function members.</span></span> <span data-ttu-id="dbe42-575">本節說明 C# 所支援的函式成員的其他媒體類型： 建構函式、 屬性、 索引子、 事件、 運算子和解構函式。</span><span class="sxs-lookup"><span data-stu-id="dbe42-575">This section describes the other kinds of function members supported by C#: constructors, properties, indexers, events, operators, and destructors.</span></span>

<span data-ttu-id="dbe42-576">下列程式碼會顯示泛型類別，稱為`List<T>`，它會實作可成長的物件清單。</span><span class="sxs-lookup"><span data-stu-id="dbe42-576">The following code shows a generic class called `List<T>`, which implements a growable list of objects.</span></span> <span data-ttu-id="dbe42-577">此類別包含數個最常見的函式成員類型。</span><span class="sxs-lookup"><span data-stu-id="dbe42-577">The class contains several examples of the most common kinds of function members.</span></span>


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

#### <a name="constructors"></a><span data-ttu-id="dbe42-578">建構函式</span><span class="sxs-lookup"><span data-stu-id="dbe42-578">Constructors</span></span>

<span data-ttu-id="dbe42-579">C# 同時支援執行個體建構函式和靜態建構函式。</span><span class="sxs-lookup"><span data-stu-id="dbe42-579">C# supports both instance and static constructors.</span></span> <span data-ttu-id="dbe42-580">「執行個體建構函式」是實作將類別執行個體初始化所需之動作的成員。</span><span class="sxs-lookup"><span data-stu-id="dbe42-580">An ***instance constructor*** is a member that implements the actions required to initialize an instance of a class.</span></span> <span data-ttu-id="dbe42-581">「靜態建構函式」是實作第一次將類別本身載入時將其初始化所需之動作的成員。</span><span class="sxs-lookup"><span data-stu-id="dbe42-581">A ***static constructor*** is a member that implements the actions required to initialize a class itself when it is first loaded.</span></span>

<span data-ttu-id="dbe42-582">建構函式的宣告方式與方法類似，但不含傳回型別且名稱會與包含它的類別相同。</span><span class="sxs-lookup"><span data-stu-id="dbe42-582">A constructor is declared like a method with no return type and the same name as the containing class.</span></span> <span data-ttu-id="dbe42-583">如果建構函式宣告包含`static`修飾詞，它會宣告靜態建構函式。</span><span class="sxs-lookup"><span data-stu-id="dbe42-583">If a constructor declaration includes a `static` modifier, it declares a static constructor.</span></span> <span data-ttu-id="dbe42-584">否則，所宣告的會是執行個體建構函式。</span><span class="sxs-lookup"><span data-stu-id="dbe42-584">Otherwise, it declares an instance constructor.</span></span>

<span data-ttu-id="dbe42-585">執行個體建構函式可以多載。</span><span class="sxs-lookup"><span data-stu-id="dbe42-585">Instance constructors can be overloaded.</span></span> <span data-ttu-id="dbe42-586">例如，`List<T>
` 類別會宣告兩個執行個體建構函式，一個沒有任何參數，另一個會採用 `int` 參數。</span><span class="sxs-lookup"><span data-stu-id="dbe42-586">For example, the `List<T>
` class declares two instance constructors, one with no parameters and one that takes an `int` parameter.</span></span> <span data-ttu-id="dbe42-587">叫用執行個體建構函式時，是使用 `new` 運算子來叫用。</span><span class="sxs-lookup"><span data-stu-id="dbe42-587">Instance constructors are invoked using the `new` operator.</span></span> <span data-ttu-id="dbe42-588">下列陳述式會配置兩個`List<string>
`執行個體使用的建構函式的每個`List`類別。</span><span class="sxs-lookup"><span data-stu-id="dbe42-588">The following statements allocate two `List<string>
` instances using each of the constructors of the `List` class.</span></span>

```csharp
List<string> list1 = new List<string>();
List<string> list2 = new List<string>(10);
```
<span data-ttu-id="dbe42-589">與其他成員不同，類別並不會繼承執行個體建構函式，而且除了類別中實際宣告的執行個體建構函式以外，類別即不會再有任何執行個體建構函式。</span><span class="sxs-lookup"><span data-stu-id="dbe42-589">Unlike other members, instance constructors are not inherited, and a class has no instance constructors other than those actually declared in the class.</span></span> <span data-ttu-id="dbe42-590">如果沒有為類別提供任何執行個體建構函式，則會自動提供一個沒有任何參數的空建構函式。</span><span class="sxs-lookup"><span data-stu-id="dbe42-590">If no instance constructor is supplied for a class, then an empty one with no parameters is automatically provided.</span></span>

#### <a name="properties"></a><span data-ttu-id="dbe42-591">屬性</span><span class="sxs-lookup"><span data-stu-id="dbe42-591">Properties</span></span>

<span data-ttu-id="dbe42-592">「屬性」是欄位的自然延伸。</span><span class="sxs-lookup"><span data-stu-id="dbe42-592">***Properties*** are a natural extension of fields.</span></span> <span data-ttu-id="dbe42-593">兩者都是具有關聯型別的具名成員，並且用來存取欄位和屬性的語法是相同的。</span><span class="sxs-lookup"><span data-stu-id="dbe42-593">Both are named members with associated types, and the syntax for accessing fields and properties is the same.</span></span> <span data-ttu-id="dbe42-594">不過，與欄位不同的是，屬性並不會指示儲存位置。</span><span class="sxs-lookup"><span data-stu-id="dbe42-594">However, unlike fields, properties do not denote storage locations.</span></span> <span data-ttu-id="dbe42-595">取而代之的是，屬性會有「存取子」，這些存取子會指定讀取或寫入其值時要執行的陳述式。</span><span class="sxs-lookup"><span data-stu-id="dbe42-595">Instead, properties have ***accessors*** that specify the statements to be executed when their values are read or written.</span></span>

<span data-ttu-id="dbe42-596">屬性宣告等 欄位中，不同之處在於宣告的結尾`get`存取子和 （或)`set`存取子分隔符號之間撰寫`{`和`}`而不是指以分號結尾。</span><span class="sxs-lookup"><span data-stu-id="dbe42-596">A property is declared like a field, except that the declaration ends with a `get` accessor and/or a `set` accessor written between the delimiters `{` and `}` instead of ending in a semicolon.</span></span> <span data-ttu-id="dbe42-597">同時具有屬性`get`存取子和`set`存取子是***讀寫屬性***，此屬性，只有`get`存取子則***唯讀屬性***，和屬性，只有`set`存取子已經***唯寫屬性***。</span><span class="sxs-lookup"><span data-stu-id="dbe42-597">A property that has both a `get` accessor and a `set` accessor is a ***read-write property***, a property that has only a `get` accessor is a ***read-only property***, and a property that has only a `set` accessor is a ***write-only property***.</span></span>

<span data-ttu-id="dbe42-598">A`get`存取子對應到無參數的方法具有傳回值的屬性型別。</span><span class="sxs-lookup"><span data-stu-id="dbe42-598">A `get` accessor corresponds to a parameterless method with a return value of the property type.</span></span> <span data-ttu-id="dbe42-599">在運算式中，參考屬性時，做為目標的指派，除了`get`屬性存取子會叫用來計算屬性的值。</span><span class="sxs-lookup"><span data-stu-id="dbe42-599">Except as the target of an assignment, when a property is referenced in an expression, the `get` accessor of the property is invoked to compute the value of the property.</span></span>

<span data-ttu-id="dbe42-600">A`set`存取子對應至方法，與具有單一參數具名`value`且沒有傳回型別。</span><span class="sxs-lookup"><span data-stu-id="dbe42-600">A `set` accessor corresponds to a method with a single parameter named `value` and no return type.</span></span> <span data-ttu-id="dbe42-601">屬性參考時指派的目標為或的運算元`++`或是`--`，則`set`提供新值的引數叫用存取子。</span><span class="sxs-lookup"><span data-stu-id="dbe42-601">When a property is referenced as the target of an assignment or as the operand of `++` or `--`, the `set` accessor is invoked with an argument that provides the new value.</span></span>

<span data-ttu-id="dbe42-602">`List<T>
` 類別會宣告 `Count` 和 `Capacity` 這兩個屬性，它們分別是唯讀屬性和讀寫屬性。</span><span class="sxs-lookup"><span data-stu-id="dbe42-602">The `List<T>
` class declares two properties, `Count` and `Capacity`, which are read-only and read-write, respectively.</span></span> <span data-ttu-id="dbe42-603">以下是這些屬性的用法範例。</span><span class="sxs-lookup"><span data-stu-id="dbe42-603">The following is an example of use of these properties.</span></span>

```csharp
List<string> names = new List<string>();
names.Capacity = 100;            // Invokes set accessor
int i = names.Count;             // Invokes get accessor
int j = names.Capacity;          // Invokes get accessor
```
<span data-ttu-id="dbe42-604">與欄位和方法類似，C# 也同時支援執行個體屬性和靜態屬性。</span><span class="sxs-lookup"><span data-stu-id="dbe42-604">Similar to fields and methods, C# supports both instance properties and static properties.</span></span> <span data-ttu-id="dbe42-605">使用宣告靜態屬性`static`修飾詞和執行個體屬性的宣告而不需要它。</span><span class="sxs-lookup"><span data-stu-id="dbe42-605">Static properties are declared with the `static` modifier, and instance properties are declared without it.</span></span>

<span data-ttu-id="dbe42-606">屬性的存取子可以是虛擬的。</span><span class="sxs-lookup"><span data-stu-id="dbe42-606">The accessor(s) of a property can be virtual.</span></span> <span data-ttu-id="dbe42-607">當屬性宣告包含 `virtual`、`abstract` 或 `override` 修飾詞時，會套用至該屬性的存取子。</span><span class="sxs-lookup"><span data-stu-id="dbe42-607">When a property declaration includes a `virtual`, `abstract`, or `override` modifier, it applies to the accessor(s) of the property.</span></span>

#### <a name="indexers"></a><span data-ttu-id="dbe42-608">索引子</span><span class="sxs-lookup"><span data-stu-id="dbe42-608">Indexers</span></span>

<span data-ttu-id="dbe42-609">「索引子」是可讓物件以和陣列相同的方式進行索引編製的成員。</span><span class="sxs-lookup"><span data-stu-id="dbe42-609">An ***indexer*** is a member that enables objects to be indexed in the same way as an array.</span></span> <span data-ttu-id="dbe42-610">索引子宣告為屬性不同之處在於成員的名稱是`this`後面接著參數清單分隔符號之間撰寫`[`和`]`。</span><span class="sxs-lookup"><span data-stu-id="dbe42-610">An indexer is declared like a property except that the name of the member is `this` followed by a parameter list written between the delimiters `[` and `]`.</span></span> <span data-ttu-id="dbe42-611">索引子的存取子中會提供參數。</span><span class="sxs-lookup"><span data-stu-id="dbe42-611">The parameters are available in the accessor(s) of the indexer.</span></span> <span data-ttu-id="dbe42-612">與屬性類似，索引子可以是讀寫、唯讀及唯寫的，而索引子的存取子可以是虛擬的。</span><span class="sxs-lookup"><span data-stu-id="dbe42-612">Similar to properties, indexers can be read-write, read-only, and write-only, and the accessor(s) of an indexer can be virtual.</span></span>

<span data-ttu-id="dbe42-613">`List` 類別會宣告一個採用 `int` 參數的單一讀寫索引子。</span><span class="sxs-lookup"><span data-stu-id="dbe42-613">The `List` class declares a single read-write indexer that takes an `int` parameter.</span></span> <span data-ttu-id="dbe42-614">此索引子使得系統能夠以 `int` 值編製 `List` 執行個體的索引。</span><span class="sxs-lookup"><span data-stu-id="dbe42-614">The indexer makes it possible to index `List` instances with `int` values.</span></span> <span data-ttu-id="dbe42-615">例如</span><span class="sxs-lookup"><span data-stu-id="dbe42-615">For example</span></span>

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
<span data-ttu-id="dbe42-616">索引子可被多載，這意謂著類別可以宣告多個索引子，只要其參數的號碼或型別不同即可。</span><span class="sxs-lookup"><span data-stu-id="dbe42-616">Indexers can be overloaded, meaning that a class can declare multiple indexers as long as the number or types of their parameters differ.</span></span>

#### <a name="events"></a><span data-ttu-id="dbe42-617">事件</span><span class="sxs-lookup"><span data-stu-id="dbe42-617">Events</span></span>

<span data-ttu-id="dbe42-618">「事件」是可讓類別或物件提供通知的成員。</span><span class="sxs-lookup"><span data-stu-id="dbe42-618">An ***event*** is a member that enables a class or object to provide notifications.</span></span> <span data-ttu-id="dbe42-619">事件宣告方式與欄位類似之處在於宣告包含`event`關鍵字和類型必須是委派型別。</span><span class="sxs-lookup"><span data-stu-id="dbe42-619">An event is declared like a field except that the declaration includes an `event` keyword and the type must be a delegate type.</span></span>

<span data-ttu-id="dbe42-620">在宣告事件成員的類別內，事件的行為與委派型別的欄位相同 (前提是該事件不是抽象事件且未宣告存取子)。</span><span class="sxs-lookup"><span data-stu-id="dbe42-620">Within a class that declares an event member, the event behaves just like a field of a delegate type (provided the event is not abstract and does not declare accessors).</span></span> <span data-ttu-id="dbe42-621">欄位會儲存對委派項目的參考，該委派項目代表已新增到事件中的事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="dbe42-621">The field stores a reference to a delegate that represents the event handlers that have been added to the event.</span></span> <span data-ttu-id="dbe42-622">如果沒有事件處理常式都存在，則欄位會`null`。</span><span class="sxs-lookup"><span data-stu-id="dbe42-622">If no event handles are present, the field is `null`.</span></span>

<span data-ttu-id="dbe42-623">`List<T>
` 類別會宣告一個名為 `Changed` 的單一事件成員，此成員會指出新項目已新增到清單中。</span><span class="sxs-lookup"><span data-stu-id="dbe42-623">The `List<T>
` class declares a single event member called `Changed`, which indicates that a new item has been added to the list.</span></span> <span data-ttu-id="dbe42-624">`Changed`引發事件時`OnChanged`虛擬方法，會先檢查事件是否`null`（亦即沒有處理常式是否都存在）。</span><span class="sxs-lookup"><span data-stu-id="dbe42-624">The `Changed` event is raised by the `OnChanged` virtual method, which first checks whether the event is `null` (meaning that no handlers are present).</span></span> <span data-ttu-id="dbe42-625">引發事件的概念完全等同於叫用該事件所代表的委派項目，因此，就引發事件而言，並沒有任何特殊的語言建構。</span><span class="sxs-lookup"><span data-stu-id="dbe42-625">The notion of raising an event is precisely equivalent to invoking the delegate represented by the event—thus, there are no special language constructs for raising events.</span></span>

<span data-ttu-id="dbe42-626">用戶端是透過「事件處理常式」來對事件進行反應。</span><span class="sxs-lookup"><span data-stu-id="dbe42-626">Clients react to events through ***event handlers***.</span></span> <span data-ttu-id="dbe42-627">附加事件處理常式時，是使用 `+=` 運算子，移除時，則是使用 `-=` 運算子。</span><span class="sxs-lookup"><span data-stu-id="dbe42-627">Event handlers are attached using the `+=` operator and removed using the `-=` operator.</span></span> <span data-ttu-id="dbe42-628">下列範例會將事件處理常式附加到 `List<string>
` 的 `Changed` 事件。</span><span class="sxs-lookup"><span data-stu-id="dbe42-628">The following example attaches an event handler to the `Changed` event of a `List<string>
`.</span></span>

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
<span data-ttu-id="dbe42-629">針對需要控制事件之基礎儲存的進階案例，事件宣告可以明確提供 `add` 和 `remove` 存取子，這有些類似於屬性的 `set` 存取子。</span><span class="sxs-lookup"><span data-stu-id="dbe42-629">For advanced scenarios where control of the underlying storage of an event is desired, an event declaration can explicitly provide `add` and `remove` accessors, which are somewhat similar to the `set` accessor of a property.</span></span>

#### <a name="operators"></a><span data-ttu-id="dbe42-630">運算子</span><span class="sxs-lookup"><span data-stu-id="dbe42-630">Operators</span></span>

<span data-ttu-id="dbe42-631">「運算子」是定義將特定運算式運算子套用到類別執行個體之意義的成員。</span><span class="sxs-lookup"><span data-stu-id="dbe42-631">An ***operator*** is a member that defines the meaning of applying a particular expression operator to instances of a class.</span></span> <span data-ttu-id="dbe42-632">可定義的運算子有三種：一元運算子、二元運算子及轉換運算子。</span><span class="sxs-lookup"><span data-stu-id="dbe42-632">Three kinds of operators can be defined: unary operators, binary operators, and conversion operators.</span></span> <span data-ttu-id="dbe42-633">所有運算子都必須宣告為 `public` 和 `static`。</span><span class="sxs-lookup"><span data-stu-id="dbe42-633">All operators must be declared as `public` and `static`.</span></span>

<span data-ttu-id="dbe42-634">`List<T>
` 類別會宣告 `operator==` 和 `operator!=` 這兩個運算子，藉此賦予將這些運算子套用到 `List` 執行個體的運算式新意義。</span><span class="sxs-lookup"><span data-stu-id="dbe42-634">The `List<T>
` class declares two operators, `operator==` and `operator!=`, and thus gives new meaning to expressions that apply those operators to `List` instances.</span></span> <span data-ttu-id="dbe42-635">具體而言，運算子會定義兩個相等`List<T>
`做為比較所包含的物件使用的每個執行個體其`Equals`方法。</span><span class="sxs-lookup"><span data-stu-id="dbe42-635">Specifically, the operators define equality of two `List<T>
` instances as comparing each of the contained objects using their `Equals` methods.</span></span> <span data-ttu-id="dbe42-636">下列範例會使用 `==` 運算子來比較兩個 `List<int>
` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="dbe42-636">The following example uses the `==` operator to compare two `List<int>
` instances.</span></span>

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

<span data-ttu-id="dbe42-637">第一個 `Console.WriteLine` 會輸出 `True`，因為兩個清單所包含物件的數目相同、值相同且順序相同。</span><span class="sxs-lookup"><span data-stu-id="dbe42-637">The first `Console.WriteLine` outputs `True` because the two lists contain the same number of objects with the same values in the same order.</span></span> <span data-ttu-id="dbe42-638">如果 `List<T>
` 並未定義 `operator==`，則第一個 `Console.WriteLine` 所輸出的會是 `False`，因為 `a` 和 `b` 參考不同的 `List<int>
` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="dbe42-638">Had `List<T>
` not defined `operator==`, the first `Console.WriteLine` would have output `False` because `a` and `b` reference different `List<int>
` instances.</span></span>

#### <a name="destructors"></a><span data-ttu-id="dbe42-639">解構函式</span><span class="sxs-lookup"><span data-stu-id="dbe42-639">Destructors</span></span>

<span data-ttu-id="dbe42-640">A***解構函式***成員實作來解構類別的執行個體所需的動作。</span><span class="sxs-lookup"><span data-stu-id="dbe42-640">A ***destructor*** is a member that implements the actions required to destruct an instance of a class.</span></span> <span data-ttu-id="dbe42-641">解構函式不能有參數，它們不能有存取範圍修飾詞，和他們無法明確地叫用。</span><span class="sxs-lookup"><span data-stu-id="dbe42-641">Destructors cannot have parameters, they cannot have accessibility modifiers, and they cannot be invoked explicitly.</span></span> <span data-ttu-id="dbe42-642">執行個體的解構函式會在記憶體回收期間自動叫用。</span><span class="sxs-lookup"><span data-stu-id="dbe42-642">The destructor for an instance is invoked automatically during garbage collection.</span></span>

<span data-ttu-id="dbe42-643">記憶體回收行程是相當大的自由決定何時回收物件，並執行解構函式。</span><span class="sxs-lookup"><span data-stu-id="dbe42-643">The garbage collector is allowed wide latitude in deciding when to collect objects and run destructors.</span></span> <span data-ttu-id="dbe42-644">具體來說，解構函式引動過程的時機並沒有決定性，並可能在任何執行緒上執行解構函式。</span><span class="sxs-lookup"><span data-stu-id="dbe42-644">Specifically, the timing of destructor invocations is not deterministic, and destructors may be executed on any thread.</span></span> <span data-ttu-id="dbe42-645">如需這些和其他原因，類別應該只有沒有任何其他解決方案可行時，會實作解構函式。</span><span class="sxs-lookup"><span data-stu-id="dbe42-645">For these and other reasons, classes should implement destructors only when no other solutions are feasible.</span></span>

<span data-ttu-id="dbe42-646">`using` 陳述式提供較佳的物件解構方法。</span><span class="sxs-lookup"><span data-stu-id="dbe42-646">The `using` statement provides a better approach to object destruction.</span></span>

## <a name="structs"></a><span data-ttu-id="dbe42-647">結構</span><span class="sxs-lookup"><span data-stu-id="dbe42-647">Structs</span></span>

<span data-ttu-id="dbe42-648">和類別一樣，***結構***是可包含資料成員和函式成員的資料結構，但不同於類別，結構是實值型別，不需要堆積配置。</span><span class="sxs-lookup"><span data-stu-id="dbe42-648">Like classes, ***structs*** are data structures that can contain data members and function members, but unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="dbe42-649">結構型別的變數直接儲存結構的資料，而類別型別的變數則儲存動態配置物件的參考。</span><span class="sxs-lookup"><span data-stu-id="dbe42-649">A variable of a struct type directly stores the data of the struct, whereas a variable of a class type stores a reference to a dynamically allocated object.</span></span> <span data-ttu-id="dbe42-650">結構型別不支援使用者指定的繼承，且所有結構型別都隱含地繼承自 `object` 型別。</span><span class="sxs-lookup"><span data-stu-id="dbe42-650">Struct types do not support user-specified inheritance, and all struct types implicitly inherit from type `object`.</span></span>

<span data-ttu-id="dbe42-651">結構特別適用於含有實值語意的小型資料結構。</span><span class="sxs-lookup"><span data-stu-id="dbe42-651">Structs are particularly useful for small data structures that have value semantics.</span></span> <span data-ttu-id="dbe42-652">複數、座標系統中的點或字典中的索引鍵/值組都是結構的良好範例。</span><span class="sxs-lookup"><span data-stu-id="dbe42-652">Complex numbers, points in a coordinate system, or key-value pairs in a dictionary are all good examples of structs.</span></span> <span data-ttu-id="dbe42-653">針對小型資料結構使用結構而不使用類別，在應用程式執行的記憶體配置數目上有很大的差別。</span><span class="sxs-lookup"><span data-stu-id="dbe42-653">The use of structs rather than classes for small data structures can make a large difference in the number of memory allocations an application performs.</span></span> <span data-ttu-id="dbe42-654">比方說，下列程式會建立並初始化 100 個點的陣列。</span><span class="sxs-lookup"><span data-stu-id="dbe42-654">For example, the following program creates and initializes an array of 100 points.</span></span> <span data-ttu-id="dbe42-655">使用 `Point` 做為類別，會具現化 101 個不同的物件 — 一個代表陣列，剩下的每個代表 100 個元素。</span><span class="sxs-lookup"><span data-stu-id="dbe42-655">With `Point` implemented as a class, 101 separate objects are instantiated—one for the array and one each for the 100 elements.</span></span>

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
<span data-ttu-id="dbe42-656">替代方案是使`Point`結構。</span><span class="sxs-lookup"><span data-stu-id="dbe42-656">An alternative is to make `Point` a struct.</span></span>

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
<span data-ttu-id="dbe42-657">現在，只有一個物件會具現化 — 代表陣列的物件 — 而 `Point` 執行個體會以內嵌方式儲存在陣列中。</span><span class="sxs-lookup"><span data-stu-id="dbe42-657">Now, only one object is instantiated—the one for the array—and the `Point` instances are stored in-line in the array.</span></span>

<span data-ttu-id="dbe42-658">結構建構函式使用 `new` 運算子叫用，但並不代表會配置記憶體。</span><span class="sxs-lookup"><span data-stu-id="dbe42-658">Struct constructors are invoked with the `new` operator, but that does not imply that memory is being allocated.</span></span> <span data-ttu-id="dbe42-659">結構建構函式只會傳回結構值本身 (通常是在堆疊上的暫存位置)，然後再視需要複製此值，而不是動態配置一個物件並傳回該物件的參考。</span><span class="sxs-lookup"><span data-stu-id="dbe42-659">Instead of dynamically allocating an object and returning a reference to it, a struct constructor simply returns the struct value itself (typically in a temporary location on the stack), and this value is then copied as necessary.</span></span>

<span data-ttu-id="dbe42-660">使用類別時，這兩種變數可以參考相同的物件，因此對其中一個變數進行的作業可能會影響另一個變數參考的物件。</span><span class="sxs-lookup"><span data-stu-id="dbe42-660">With classes, it is possible for two variables to reference the same object and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="dbe42-661">使用結構時，變數各有自己的資料複本，因此在某一個變數上進行的作業不可能會影響其他變數。</span><span class="sxs-lookup"><span data-stu-id="dbe42-661">With structs, the variables each have their own copy of the data, and it is not possible for operations on one to affect the other.</span></span> <span data-ttu-id="dbe42-662">例如，下列程式碼片段所產生的輸出取決於是否`Point`是類別或結構。</span><span class="sxs-lookup"><span data-stu-id="dbe42-662">For example, the output produced by the following code fragment depends on whether `Point` is a class or a struct.</span></span>

```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 20;
Console.WriteLine(b.x);
```
<span data-ttu-id="dbe42-663">如果`Point`是類別，而輸出則是`20`因為`a`和`b`參考相同的物件。</span><span class="sxs-lookup"><span data-stu-id="dbe42-663">If `Point` is a class, the output is `20` because `a` and `b` reference the same object.</span></span> <span data-ttu-id="dbe42-664">如果`Point`是結構，而輸出則是`10`因為指派`a`要`b`建立一份值，和此複本不會受到後續指派給`a.x`。</span><span class="sxs-lookup"><span data-stu-id="dbe42-664">If `Point` is a struct, the output is `10` because the assignment of `a` to `b` creates a copy of the value, and this copy is unaffected by the subsequent assignment to `a.x`.</span></span>

<span data-ttu-id="dbe42-665">前一個範例會反白顯示結構的兩個限制。</span><span class="sxs-lookup"><span data-stu-id="dbe42-665">The previous example highlights two of the limitations of structs.</span></span> <span data-ttu-id="dbe42-666">首先，複製整個結構通常較複製物件參考沒有效率，因此，結構的指派和實值參數傳遞會比參考型別耗用更多資源。</span><span class="sxs-lookup"><span data-stu-id="dbe42-666">First, copying an entire struct is typically less efficient than copying an object reference, so assignment and value parameter passing can be more expensive with structs than with reference types.</span></span> <span data-ttu-id="dbe42-667">再者，除了 `ref` 和 `out` 參數外，不可能建立結構的參考，因此在許多情況下無法使用它們。</span><span class="sxs-lookup"><span data-stu-id="dbe42-667">Second, except for `ref` and `out` parameters, it is not possible to create references to structs, which rules out their usage in a number of situations.</span></span>

## <a name="arrays"></a><span data-ttu-id="dbe42-668">陣列</span><span class="sxs-lookup"><span data-stu-id="dbe42-668">Arrays</span></span>

<span data-ttu-id="dbe42-669">***陣列***是一種資料結構，其中包含一些可透過計算索引存取的變數。</span><span class="sxs-lookup"><span data-stu-id="dbe42-669">An ***array*** is a data structure that contains a number of variables that are accessed through computed indices.</span></span> <span data-ttu-id="dbe42-670">陣列中包含的變數 (也稱為陣列的***元素***) 屬於相同的型別，這種型別稱為陣列的***元素型別***。</span><span class="sxs-lookup"><span data-stu-id="dbe42-670">The variables contained in an array, also called the ***elements*** of the array, are all of the same type, and this type is called the ***element type*** of the array.</span></span>

<span data-ttu-id="dbe42-671">陣列型別是參考型別，而陣列變數的宣告只是預留空間給陣列執行個體的參考。</span><span class="sxs-lookup"><span data-stu-id="dbe42-671">Array types are reference types, and the declaration of an array variable simply sets aside space for a reference to an array instance.</span></span> <span data-ttu-id="dbe42-672">在執行階段使用動態建立實際的陣列執行個體`new`運算子。</span><span class="sxs-lookup"><span data-stu-id="dbe42-672">Actual array instances are created dynamically at run-time using the `new` operator.</span></span> <span data-ttu-id="dbe42-673">`new`作業指定***長度***新陣列執行個體，這固定的執行個體的存留期。</span><span class="sxs-lookup"><span data-stu-id="dbe42-673">The `new` operation specifies the ***length*** of the new array instance, which is then fixed for the lifetime of the instance.</span></span> <span data-ttu-id="dbe42-674">陣列元素的索引範圍在 `0` 到 `Length - 1` 之間。</span><span class="sxs-lookup"><span data-stu-id="dbe42-674">The indices of the elements of an array range from `0` to `Length - 1`.</span></span> <span data-ttu-id="dbe42-675">`new` 運算子會自動將陣列的元素初始化為其預設值，例如，針對所有數值型別，此值為零，而針對所有參考型別，此值為 `null`。</span><span class="sxs-lookup"><span data-stu-id="dbe42-675">The `new` operator automatically initializes the elements of an array to their default value, which, for example, is zero for all numeric types and `null` for all reference types.</span></span>

<span data-ttu-id="dbe42-676">下列範例會建立 `int` 元素的陣列、初始化陣列，並印出陣列的內容。</span><span class="sxs-lookup"><span data-stu-id="dbe42-676">The following example creates an array of `int` elements, initializes the array, and prints out the contents of the array.</span></span>

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
<span data-ttu-id="dbe42-677">這個範例會建立並操作***一維陣列***。</span><span class="sxs-lookup"><span data-stu-id="dbe42-677">This example creates and operates on a ***single-dimensional array***.</span></span> <span data-ttu-id="dbe42-678">C# 也支援***多維陣列***。</span><span class="sxs-lookup"><span data-stu-id="dbe42-678">C# also supports ***multi-dimensional arrays***.</span></span> <span data-ttu-id="dbe42-679">一個陣列型別的維度數目 (亦稱為陣列型別的***順位***)，是寫入陣列型別的方括弧之間的逗號數目加一。</span><span class="sxs-lookup"><span data-stu-id="dbe42-679">The number of dimensions of an array type, also known as the ***rank*** of the array type, is one plus the number of commas written between the square brackets of the array type.</span></span> <span data-ttu-id="dbe42-680">下列範例會配置一維、 二維和三維陣列。</span><span class="sxs-lookup"><span data-stu-id="dbe42-680">The following example allocates a one-dimensional, a two-dimensional, and a three-dimensional array.</span></span>

```csharp
int[] a1 = new int[10];
int[,] a2 = new int[10, 5];
int[,,] a3 = new int[10, 5, 2];
```
<span data-ttu-id="dbe42-681">`a1` 陣列包含 10 個元素、`a2`陣列包含 50 (10 × 5) 個元素，`a3` 陣列包含 100 (10 × 5 × 2) 個元素。</span><span class="sxs-lookup"><span data-stu-id="dbe42-681">The `a1` array contains 10 elements, the `a2` array contains 50 (10 × 5) elements, and the `a3` array contains 100 (10 × 5 × 2) elements.</span></span>

<span data-ttu-id="dbe42-682">陣列的元素型別可以是任一型別，包括陣列型別。</span><span class="sxs-lookup"><span data-stu-id="dbe42-682">The element type of an array can be any type, including an array type.</span></span> <span data-ttu-id="dbe42-683">包含陣列型別之元素的陣列有時稱為***不規則陣列***，因為元素陣列的長度不需完全相同。</span><span class="sxs-lookup"><span data-stu-id="dbe42-683">An array with elements of an array type is sometimes called a ***jagged array*** because the lengths of the element arrays do not all have to be the same.</span></span> <span data-ttu-id="dbe42-684">下列範例會配置一個 `int` 型別的陣列：</span><span class="sxs-lookup"><span data-stu-id="dbe42-684">The following example allocates an array of arrays of `int`:</span></span>

```csharp
int[][] a = new int[3][];
a[0] = new int[10];
a[1] = new int[5];
a[2] = new int[20];
```
<span data-ttu-id="dbe42-685">第一行建立包含三個元素的陣列，每個元素的型別均為 `int[]`，每個元素的初始值均為 `null`。</span><span class="sxs-lookup"><span data-stu-id="dbe42-685">The first line creates an array with three elements, each of type `int[]` and each with an initial value of `null`.</span></span> <span data-ttu-id="dbe42-686">接下來的幾行則使用不同長度的個別陣列執行個體的參考初始化三個元素。</span><span class="sxs-lookup"><span data-stu-id="dbe42-686">The subsequent lines then initialize the three elements with references to individual array instances of varying lengths.</span></span>

<span data-ttu-id="dbe42-687">`new`運算子允許使用指定的陣列元素的初始值***陣列初始設定式***，這是一份分隔符號之間撰寫的運算式`{`和`}`。</span><span class="sxs-lookup"><span data-stu-id="dbe42-687">The `new` operator permits the initial values of the array elements to be specified using an ***array initializer***, which is a list of expressions written between the delimiters `{` and `}`.</span></span> <span data-ttu-id="dbe42-688">下列範例使用三個元素配置並初始化 `int[]`。</span><span class="sxs-lookup"><span data-stu-id="dbe42-688">The following example allocates and initializes an `int[]` with three elements.</span></span>

```csharp
int[] a = new int[] {1, 2, 3};
```
<span data-ttu-id="dbe42-689">請注意，從之間的運算式數目推斷陣列的長度`{`和`}`。</span><span class="sxs-lookup"><span data-stu-id="dbe42-689">Note that the length of the array is inferred from the number of expressions between `{` and `}`.</span></span> <span data-ttu-id="dbe42-690">本機變數與欄位宣告可以進一步縮短，就不需要重新敘述陣列型別。</span><span class="sxs-lookup"><span data-stu-id="dbe42-690">Local variable and field declarations can be shortened further such that the array type does not have to be restated.</span></span>

```csharp
int[] a = {1, 2, 3};
```
<span data-ttu-id="dbe42-691">先前的兩個範例等同於下列範例：</span><span class="sxs-lookup"><span data-stu-id="dbe42-691">Both of the previous examples are equivalent to the following:</span></span>

```csharp
int[] t = new int[3];
t[0] = 1;
t[1] = 2;
t[2] = 3;
int[] a = t;
```
## <a name="interfaces"></a><span data-ttu-id="dbe42-692">介面</span><span class="sxs-lookup"><span data-stu-id="dbe42-692">Interfaces</span></span>

<span data-ttu-id="dbe42-693">「介面」定義可由類別和結構實作的合約。</span><span class="sxs-lookup"><span data-stu-id="dbe42-693">An ***interface*** defines a contract that can be implemented by classes and structs.</span></span> <span data-ttu-id="dbe42-694">介面可以包含方法、屬性、事件和索引子。</span><span class="sxs-lookup"><span data-stu-id="dbe42-694">An interface can contain methods, properties, events, and indexers.</span></span> <span data-ttu-id="dbe42-695">介面不提供它所定義之成員的實作 (它只會指定必須由類別提供的成員或實作介面的結構)。</span><span class="sxs-lookup"><span data-stu-id="dbe42-695">An interface does not provide implementations of the members it defines—it merely specifies the members that must be supplied by classes or structs that implement the interface.</span></span>

<span data-ttu-id="dbe42-696">介面可以採用「多重繼承」。</span><span class="sxs-lookup"><span data-stu-id="dbe42-696">Interfaces may employ ***multiple inheritance***.</span></span> <span data-ttu-id="dbe42-697">在下列範例中，介面 `IComboBox` 同時繼承自 `ITextBox` 和 `IListBox`。</span><span class="sxs-lookup"><span data-stu-id="dbe42-697">In the following example, the interface `IComboBox` inherits from both `ITextBox` and `IListBox`.</span></span>

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
<span data-ttu-id="dbe42-698">類別和結構可以實作多個介面。</span><span class="sxs-lookup"><span data-stu-id="dbe42-698">Classes and structs can implement multiple interfaces.</span></span> <span data-ttu-id="dbe42-699">在下列範例中，類別 `EditBox` 可以實作 `IControl` 與 `IDataBound` 兩者。</span><span class="sxs-lookup"><span data-stu-id="dbe42-699">In the following example, the class `EditBox` implements both `IControl` and `IDataBound`.</span></span>

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
<span data-ttu-id="dbe42-700">當類別或結構實作特定介面時，可以將該類別或結構的執行個體隱含地轉換為該介面型別。</span><span class="sxs-lookup"><span data-stu-id="dbe42-700">When a class or struct implements a particular interface, instances of that class or struct can be implicitly converted to that interface type.</span></span> <span data-ttu-id="dbe42-701">例如</span><span class="sxs-lookup"><span data-stu-id="dbe42-701">For example</span></span>

```csharp
EditBox editBox = new EditBox();
IControl control = editBox;
IDataBound dataBound = editBox;
```
<span data-ttu-id="dbe42-702">在實作特定介面的執行個體無法確知的情況下，可以使用動態型別轉換。</span><span class="sxs-lookup"><span data-stu-id="dbe42-702">In cases where an instance is not statically known to implement a particular interface, dynamic type casts can be used.</span></span> <span data-ttu-id="dbe42-703">例如，下列陳述式會使用動態型別轉換取得的物件`IControl`和`IDataBound`介面實作。</span><span class="sxs-lookup"><span data-stu-id="dbe42-703">For example, the following statements use dynamic type casts to obtain an object's `IControl` and `IDataBound` interface implementations.</span></span> <span data-ttu-id="dbe42-704">因為物件的實際型別是`EditBox`，轉型成功。</span><span class="sxs-lookup"><span data-stu-id="dbe42-704">Because the actual type of the object is `EditBox`, the casts succeed.</span></span>

```csharp
object obj = new EditBox();
IControl control = (IControl)obj;
IDataBound dataBound = (IDataBound)obj;
```
<span data-ttu-id="dbe42-705">在上一個`EditBox`類別，`Paint`方法，從`IControl`介面並`Bind`方法，從`IDataBound`使用實作介面`public`成員。</span><span class="sxs-lookup"><span data-stu-id="dbe42-705">In the previous `EditBox` class, the `Paint` method from the `IControl` interface and the `Bind` method from the `IDataBound` interface are implemented using `public` members.</span></span> <span data-ttu-id="dbe42-706">C# 也支援***明確介面成員實作***，利用這種類別或結構可以避免將成員設為`public`。</span><span class="sxs-lookup"><span data-stu-id="dbe42-706">C# also supports ***explicit interface member implementations***, using which the class or struct can avoid making the members `public`.</span></span> <span data-ttu-id="dbe42-707">明確介面成員實作是使用介面成員完整名稱來撰寫。</span><span class="sxs-lookup"><span data-stu-id="dbe42-707">An explicit interface member implementation is written using the fully qualified interface member name.</span></span> <span data-ttu-id="dbe42-708">例如，`EditBox` 類別可以使用明確介面成員實作來實作 `IControl.Paint` 和 `IDataBound.Bind` 方法，如下所示。</span><span class="sxs-lookup"><span data-stu-id="dbe42-708">For example, the `EditBox` class could implement the `IControl.Paint` and `IDataBound.Bind` methods using explicit interface member implementations as follows.</span></span>

```csharp
public class EditBox: IControl, IDataBound
{
    void IControl.Paint() {...}
    void IDataBound.Bind(Binder b) {...}
}
```
<span data-ttu-id="dbe42-709">明確介面成員只能透過介面型別來存取。</span><span class="sxs-lookup"><span data-stu-id="dbe42-709">Explicit interface members can only be accessed via the interface type.</span></span> <span data-ttu-id="dbe42-710">比方說，實作`IControl.Paint`提供先前`EditBox`可以只藉由設定轉換的第一個叫用類別`EditBox`參考`IControl`介面類型。</span><span class="sxs-lookup"><span data-stu-id="dbe42-710">For example, the implementation of `IControl.Paint` provided by the previous `EditBox` class can only be invoked by first converting the `EditBox` reference to the `IControl` interface type.</span></span>

```csharp
EditBox editBox = new EditBox();
editBox.Paint();                        // Error, no such method
IControl control = editBox;
control.Paint();                        // Ok
```

## <a name="enums"></a><span data-ttu-id="dbe42-711">列舉</span><span class="sxs-lookup"><span data-stu-id="dbe42-711">Enums</span></span>

<span data-ttu-id="dbe42-712">「列舉型別」是含一組具名常數的相異實值型別。</span><span class="sxs-lookup"><span data-stu-id="dbe42-712">An ***enum type*** is a distinct value type with a set of named constants.</span></span> <span data-ttu-id="dbe42-713">下列範例宣告，並使用名為列舉型別`Color`具有三個的常數值`Red`， `Green`，和`Blue`。</span><span class="sxs-lookup"><span data-stu-id="dbe42-713">The following example declares and uses an enum type named `Color` with three constant values, `Red`, `Green`, and `Blue`.</span></span>

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
<span data-ttu-id="dbe42-714">每個列舉類型都有對應的整數類資料類型，稱為***基礎型別***的列舉型別。</span><span class="sxs-lookup"><span data-stu-id="dbe42-714">Each enum type has a corresponding integral type called the ***underlying type*** of the enum type.</span></span> <span data-ttu-id="dbe42-715">沒有明確宣告基礎型別的列舉型別都具有基礎類型的`int`。</span><span class="sxs-lookup"><span data-stu-id="dbe42-715">An enum type that does not explicitly declare an underlying type has an underlying type of `int`.</span></span> <span data-ttu-id="dbe42-716">列舉類型的儲存格式和可能的值範圍是其基礎類型所決定。</span><span class="sxs-lookup"><span data-stu-id="dbe42-716">An enum type's storage format and range of possible values are determined by its underlying type.</span></span> <span data-ttu-id="dbe42-717">列舉成員不會受限於一組可對列舉類型的值。</span><span class="sxs-lookup"><span data-stu-id="dbe42-717">The set of values that an enum type can take on is not limited by its enum members.</span></span> <span data-ttu-id="dbe42-718">特別的是，列舉的基礎類型的任何值可以轉換為列舉型別，並為該列舉型別的相異有效值。</span><span class="sxs-lookup"><span data-stu-id="dbe42-718">In particular, any value of the underlying type of an enum can be cast to the enum type and is a distinct valid value of that enum type.</span></span>

<span data-ttu-id="dbe42-719">下列範例宣告名為列舉型別`Alignment`的基礎類型`sbyte`。</span><span class="sxs-lookup"><span data-stu-id="dbe42-719">The following example declares an enum type named `Alignment` with an underlying type of `sbyte`.</span></span>

```csharp
enum Alignment: sbyte
{
    Left = -1,
    Center = 0,
    Right = 1
}
```
<span data-ttu-id="dbe42-720">如先前的範例所示，列舉成員宣告可以包含常數的運算式，指定成員的值。</span><span class="sxs-lookup"><span data-stu-id="dbe42-720">As shown by the previous example, an enum member declaration can include a constant expression that specifies the value of the member.</span></span> <span data-ttu-id="dbe42-721">每個列舉成員的常值必須是列舉的基礎類型的範圍中。</span><span class="sxs-lookup"><span data-stu-id="dbe42-721">The constant value for each enum member must be in the range of the underlying type of the enum.</span></span> <span data-ttu-id="dbe42-722">列舉成員宣告並未明確指定值，該成員指定的值 （如果它是列舉型別中的第一個成員） 的零或一加賦予上述的列舉成員的值。</span><span class="sxs-lookup"><span data-stu-id="dbe42-722">When an enum member declaration does not explicitly specify a value, the member is given the value zero (if it is the first member in the enum type) or the value of the textually preceding enum member plus one.</span></span>

<span data-ttu-id="dbe42-723">列舉值可以轉換成整數值和相反的情況使用類型轉換。</span><span class="sxs-lookup"><span data-stu-id="dbe42-723">Enum values can be converted to integral values and vice versa using type casts.</span></span> <span data-ttu-id="dbe42-724">例如</span><span class="sxs-lookup"><span data-stu-id="dbe42-724">For example</span></span>

```csharp
int i = (int)Color.Blue;        // int i = 2;
Color c = (Color)2;             // Color c = Color.Blue;
```
<span data-ttu-id="dbe42-725">任何列舉類型的預設值是整數值轉換為列舉型別為零。</span><span class="sxs-lookup"><span data-stu-id="dbe42-725">The default value of any enum type is the integral value zero converted to the enum type.</span></span> <span data-ttu-id="dbe42-726">在其中的變數會自動初始化為預設值的情況下，這是提供給列舉類型的變數的值。</span><span class="sxs-lookup"><span data-stu-id="dbe42-726">In cases where variables are automatically initialized to a default value, this is the value given to variables of enum types.</span></span> <span data-ttu-id="dbe42-727">預設值的列舉型別可供輕鬆，常值的順序`0`會隱含轉換成任何列舉型別。</span><span class="sxs-lookup"><span data-stu-id="dbe42-727">In order for the default value of an enum type to be easily available, the literal `0` implicitly converts to any enum type.</span></span> <span data-ttu-id="dbe42-728">因此，允許以下的情況。</span><span class="sxs-lookup"><span data-stu-id="dbe42-728">Thus, the following is permitted.</span></span>

```csharp
Color c = 0;
```

## <a name="delegates"></a><span data-ttu-id="dbe42-729">委派</span><span class="sxs-lookup"><span data-stu-id="dbe42-729">Delegates</span></span>

<span data-ttu-id="dbe42-730">「委派型別」代表對方法的參考，其中含有特定參數清單與傳回型別。</span><span class="sxs-lookup"><span data-stu-id="dbe42-730">A ***delegate type*** represents references to methods with a particular parameter list and return type.</span></span> <span data-ttu-id="dbe42-731">委派讓您可將方法視為實體，而實體能指派給變數或當作參數來傳遞。</span><span class="sxs-lookup"><span data-stu-id="dbe42-731">Delegates make it possible to treat methods as entities that can be assigned to variables and passed as parameters.</span></span> <span data-ttu-id="dbe42-732">委派就類似其他程式設計語言中的函式指標，但與函式指標的不同之處是，委派是物件導向且為型別安全。</span><span class="sxs-lookup"><span data-stu-id="dbe42-732">Delegates are similar to the concept of function pointers found in some other languages, but unlike function pointers, delegates are object-oriented and type-safe.</span></span>

<span data-ttu-id="dbe42-733">下列範例會宣告並使用名為 `Function` 的委派型別。</span><span class="sxs-lookup"><span data-stu-id="dbe42-733">The following example declares and uses a delegate type named `Function`.</span></span>

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
<span data-ttu-id="dbe42-734">`Function` 委派型別的執行個體可以參考任何採用 `double` 引數並傳回 `double` 值的方法。</span><span class="sxs-lookup"><span data-stu-id="dbe42-734">An instance of the `Function` delegate type can reference any method that takes a `double` argument and returns a `double` value.</span></span> <span data-ttu-id="dbe42-735">`Apply`方法適用於給定`Function`的項目`double[]`，其中會傳回`double[]`結果。</span><span class="sxs-lookup"><span data-stu-id="dbe42-735">The `Apply` method applies a given `Function` to the elements of a `double[]`, returning a `double[]` with the results.</span></span> <span data-ttu-id="dbe42-736">在 `Main` 方法中，是使用 `Apply` 將三個不同的函式套用到 `double[]`。</span><span class="sxs-lookup"><span data-stu-id="dbe42-736">In the `Main` method, `Apply` is used to apply three different functions to a `double[]`.</span></span>

<span data-ttu-id="dbe42-737">委派可以參考靜態方法 (例如上一個範例中的 `Square` 或 `Math.Sin`)，或是參考執行個體方法 (例如上一個範例中的 `m.Multiply`)。</span><span class="sxs-lookup"><span data-stu-id="dbe42-737">A delegate can reference either a static method (such as `Square` or `Math.Sin` in the previous example) or an instance method (such as `m.Multiply` in the previous example).</span></span> <span data-ttu-id="dbe42-738">參考執行個體方法的委派也會參考特定的物件，而且透過委派來叫用執行個體方法時，該物件就會變成叫用中的 `this`。</span><span class="sxs-lookup"><span data-stu-id="dbe42-738">A delegate that references an instance method also references a particular object, and when the instance method is invoked through the delegate, that object becomes `this` in the invocation.</span></span>

<span data-ttu-id="dbe42-739">您也可以使用匿名函式來建立委派，這些函式是動態建立的「內嵌方法」。</span><span class="sxs-lookup"><span data-stu-id="dbe42-739">Delegates can also be created using anonymous functions, which are "inline methods" that are created on the fly.</span></span> <span data-ttu-id="dbe42-740">匿名函式可以看見周圍方法的區域變數。</span><span class="sxs-lookup"><span data-stu-id="dbe42-740">Anonymous functions can see the local variables of the surrounding methods.</span></span> <span data-ttu-id="dbe42-741">因此，上面的乘數範例可以撰寫更輕鬆地沒有使用`Multiplier`類別：</span><span class="sxs-lookup"><span data-stu-id="dbe42-741">Thus, the multiplier example above can be written more easily without using a `Multiplier` class:</span></span>

```csharp
double[] doubles =  Apply(a, (double x) => x * 2.0);
```
<span data-ttu-id="dbe42-742">委派有一個有趣且實用的屬性，就是它並不知道或並不在乎所參考方法的類別；唯一重要的是所參考的方法具有與委派相同的參數和傳回型別。</span><span class="sxs-lookup"><span data-stu-id="dbe42-742">An interesting and useful property of a delegate is that it does not know or care about the class of the method it references; all that matters is that the referenced method has the same parameters and return type as the delegate.</span></span>

## <a name="attributes"></a><span data-ttu-id="dbe42-743">屬性</span><span class="sxs-lookup"><span data-stu-id="dbe42-743">Attributes</span></span>

<span data-ttu-id="dbe42-744">C# 程式中的型別、成員和其他實體支援控制其某方面行為的修飾詞。</span><span class="sxs-lookup"><span data-stu-id="dbe42-744">Types, members, and other entities in a C# program support modifiers that control certain aspects of their behavior.</span></span> <span data-ttu-id="dbe42-745">例如，方法的協助工具是使用 `public`、`protected`、`internal` 和 `private` 修飾詞控制。</span><span class="sxs-lookup"><span data-stu-id="dbe42-745">For example, the accessibility of a method is controlled using the `public`, `protected`, `internal`, and `private` modifiers.</span></span> <span data-ttu-id="dbe42-746">C# 將此能力一般化，宣告式資訊的使用者定義型別才能附加至程式實體，並在執行階段擷取。</span><span class="sxs-lookup"><span data-stu-id="dbe42-746">C# generalizes this capability such that user-defined types of declarative information can be attached to program entities and retrieved at run-time.</span></span> <span data-ttu-id="dbe42-747">程式是透過定義和使用***屬性***指定這項額外的宣告式資訊。</span><span class="sxs-lookup"><span data-stu-id="dbe42-747">Programs specify this additional declarative information by defining and using ***attributes***.</span></span>

<span data-ttu-id="dbe42-748">下列範例宣告的 `HelpAttribute` 屬性可置於程式實體，以提供其相關文件的連結。</span><span class="sxs-lookup"><span data-stu-id="dbe42-748">The following example declares a `HelpAttribute` attribute that can be placed on program entities to provide links to their associated documentation.</span></span>

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
<span data-ttu-id="dbe42-749">所有屬性的類別都衍生自`System.Attribute`基底.NET Framework 所提供的類別。</span><span class="sxs-lookup"><span data-stu-id="dbe42-749">All attribute classes derive from the `System.Attribute` base class provided by the .NET Framework.</span></span> <span data-ttu-id="dbe42-750">在相關聯的宣告之前，於方括弧中提供屬性的名稱 (及任何引數) 即可套用屬性。</span><span class="sxs-lookup"><span data-stu-id="dbe42-750">Attributes can be applied by giving their name, along with any arguments, inside square brackets just before the associated declaration.</span></span> <span data-ttu-id="dbe42-751">如果屬性名稱的結尾`Attribute`，則參考該屬性時，就可以省略該部分的名稱。</span><span class="sxs-lookup"><span data-stu-id="dbe42-751">If an attribute's name ends in `Attribute`, that part of the name can be omitted when the attribute is referenced.</span></span> <span data-ttu-id="dbe42-752">例如，`HelpAttribute` 屬性可以下列方式使用。</span><span class="sxs-lookup"><span data-stu-id="dbe42-752">For example, the `HelpAttribute` attribute can be used as follows.</span></span>

```csharp
[Help("http://msdn.microsoft.com/.../MyClass.htm")]
public class Widget
{
    [Help("http://msdn.microsoft.com/.../MyClass.htm", Topic = "Display")]
    public void Display(string text) {}
}
```
<span data-ttu-id="dbe42-753">這個範例會附加`HelpAttribute`要`Widget`類別，而另一個`HelpAttribute`到`Display`類別中的方法。</span><span class="sxs-lookup"><span data-stu-id="dbe42-753">This example attaches a `HelpAttribute` to the `Widget` class and another `HelpAttribute` to the `Display` method in the class.</span></span> <span data-ttu-id="dbe42-754">屬性類別的公用建構函式控制將屬性附加至程式實體時必須提供的資訊。</span><span class="sxs-lookup"><span data-stu-id="dbe42-754">The public constructors of an attribute class control the information that must be provided when the attribute is attached to a program entity.</span></span> <span data-ttu-id="dbe42-755">透過參考屬性類別的公用讀寫屬性可提供其他資訊 (例如先前對 `Topic` 的參考 )。</span><span class="sxs-lookup"><span data-stu-id="dbe42-755">Additional information can be provided by referencing public read-write properties of the attribute class (such as the reference to the `Topic` property previously).</span></span>

<span data-ttu-id="dbe42-756">下列範例示範如何擷取指定的程式實體的屬性資訊，在執行階段使用反映。</span><span class="sxs-lookup"><span data-stu-id="dbe42-756">The following example shows how attribute information for a given program entity can be retrieved at run-time using reflection.</span></span>

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
<span data-ttu-id="dbe42-757">透過反射要求特定的屬性時，會以程式來源中提供的資訊叫用屬性類別的建構函式，並傳回產生的屬性執行個體。</span><span class="sxs-lookup"><span data-stu-id="dbe42-757">When a particular attribute is requested through reflection, the constructor for the attribute class is invoked with the information provided in the program source, and the resulting attribute instance is returned.</span></span> <span data-ttu-id="dbe42-758">如果是透過屬性提供其他資訊，傳回屬性執行個體之前，這些屬性會設為指定的值。</span><span class="sxs-lookup"><span data-stu-id="dbe42-758">If additional information was provided through properties, those properties are set to the given values before the attribute instance is returned.</span></span>
