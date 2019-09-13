---
ms.openlocfilehash: adf81842e3c763c7bbdd3f10bb884dc1207b9099
ms.sourcegitcommit: 0489cb64b7dfb328813d757f4d447a15b85a5851
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/11/2019
ms.locfileid: "70912428"
---
# <a name="documentation-comments"></a><span data-ttu-id="93194-101">文件註解</span><span class="sxs-lookup"><span data-stu-id="93194-101">Documentation comments</span></span>

<span data-ttu-id="93194-102">C#提供一種機制，讓程式設計人員使用包含 XML 文字的特殊批註語法來記錄其程式碼。</span><span class="sxs-lookup"><span data-stu-id="93194-102">C# provides a mechanism for programmers to document their code using a special comment syntax that contains XML text.</span></span> <span data-ttu-id="93194-103">在原始程式碼檔中，具有特定形式的批註可用來指示工具從這些批註產生 XML，並將它們放在其前面的原始程式碼專案。</span><span class="sxs-lookup"><span data-stu-id="93194-103">In source code files, comments having a certain form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="93194-104">使用這種語法的批註稱為***檔批註***。</span><span class="sxs-lookup"><span data-stu-id="93194-104">Comments using such syntax are called ***documentation comments***.</span></span> <span data-ttu-id="93194-105">它們必須緊接在使用者定義型別（例如類別、委派或介面）或成員（例如欄位、事件、屬性或方法）前面。</span><span class="sxs-lookup"><span data-stu-id="93194-105">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method).</span></span> <span data-ttu-id="93194-106">XML 產生工具稱為***檔***產生器。</span><span class="sxs-lookup"><span data-stu-id="93194-106">The XML generation tool is called the ***documentation generator***.</span></span> <span data-ttu-id="93194-107">（此產生器可能是C#編譯器本身，但不需要）。檔產生器所產生的輸出稱為***檔***檔。</span><span class="sxs-lookup"><span data-stu-id="93194-107">(This generator could be, but need not be, the C# compiler itself.) The output produced by the documentation generator is called the ***documentation file***.</span></span> <span data-ttu-id="93194-108">檔集是用來做為***文檔檢視器***的輸入;一種工具，目的是要產生某種類型資訊和其相關檔的視覺化顯示方式。</span><span class="sxs-lookup"><span data-stu-id="93194-108">A documentation file is used as input to a ***documentation viewer***; a tool intended to produce some sort of visual display of type information and its associated documentation.</span></span>

<span data-ttu-id="93194-109">此規格建議要用於檔批註中的一組標記，但不需要使用這些標記，而且只要遵循格式正確之 XML 的規則，就可以使用其他標記。</span><span class="sxs-lookup"><span data-stu-id="93194-109">This specification suggests a set of tags to be used in documentation comments, but use of these tags is not required, and other tags may be used if desired, as long the rules of well-formed XML are followed.</span></span>

## <a name="introduction"></a><span data-ttu-id="93194-110">簡介</span><span class="sxs-lookup"><span data-stu-id="93194-110">Introduction</span></span>

<span data-ttu-id="93194-111">具有特殊形式的批註，可以用來指示工具從這些批註產生 XML，並將它們放在前面的原始程式碼元素。</span><span class="sxs-lookup"><span data-stu-id="93194-111">Comments having a special form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="93194-112">這類批註是以三個斜線（`///`）開頭的單行批註，或是以斜線和二顆星（`/**`）開頭的分隔批註。</span><span class="sxs-lookup"><span data-stu-id="93194-112">Such comments are single-line comments that start with three slashes (`///`), or delimited comments that start with a slash and two stars (`/**`).</span></span> <span data-ttu-id="93194-113">它們必須緊接在使用者定義型別（例如類別、委派或介面）或其標注的成員（例如欄位、事件、屬性或方法）前面。</span><span class="sxs-lookup"><span data-stu-id="93194-113">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method) that they annotate.</span></span> <span data-ttu-id="93194-114">屬性區段（[屬性規格](attributes.md#attribute-specification)）會視為宣告的一部分，因此檔批註必須在套用至類型或成員的屬性之前。</span><span class="sxs-lookup"><span data-stu-id="93194-114">Attribute sections ([Attribute specification](attributes.md#attribute-specification)) are considered part of declarations, so documentation comments must precede attributes applied to a type or member.</span></span>

<span data-ttu-id="93194-115">__語法：__</span><span class="sxs-lookup"><span data-stu-id="93194-115">__Syntax:__</span></span>

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

<span data-ttu-id="93194-116">在*single_line_doc_comment*中，如果在與目前*single_line_doc_comment*相鄰之每`///`個*single_line_doc_comment*的字元後面*有一個空白字元*，則該*空白字元不*會包含在 XML 輸出中。</span><span class="sxs-lookup"><span data-stu-id="93194-116">In a *single_line_doc_comment*, if there is a *whitespace* character following the `///` characters on each of the *single_line_doc_comment*s adjacent to the current *single_line_doc_comment*, then that *whitespace* character is not included in the XML output.</span></span>

<span data-ttu-id="93194-117">在分隔的-doc 批註中，如果第二行的第一個非空白字元是星號，而選擇性空白字元的模式相同，則會在分隔-doc 批註的每一行開頭重複一個星號字元，然後，重複模式的字元不會包含在 XML 輸出中。</span><span class="sxs-lookup"><span data-stu-id="93194-117">In a delimited-doc-comment, if the first non-whitespace character on the second line is an asterisk and the same pattern of optional whitespace characters and an asterisk character is repeated at the beginning of each of the line within the delimited-doc-comment, then the characters of the repeated pattern are not included in the XML output.</span></span> <span data-ttu-id="93194-118">此模式可能會在星號字元後面加上空白字元。</span><span class="sxs-lookup"><span data-stu-id="93194-118">The pattern may include whitespace characters after, as well as before, the asterisk character.</span></span>

<span data-ttu-id="93194-119">__範例:__</span><span class="sxs-lookup"><span data-stu-id="93194-119">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>
///
public class Point 
{
    /// <summary>method <c>draw</c> renders the point.</summary>
    void draw() {...}
}
```

<span data-ttu-id="93194-120">檔批註中的文字必須根據 XML 的規則（ https://www.w3.org/TR/REC-xml) ）正確地進行格式化。</span><span class="sxs-lookup"><span data-stu-id="93194-120">The text within documentation comments must be well formed according to the rules of XML (https://www.w3.org/TR/REC-xml).</span></span> <span data-ttu-id="93194-121">如果 XML 格式不正確，則會產生警告，而且檔檔案會包含批註，指出發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="93194-121">If the XML is ill formed, a warning is generated and the documentation file will contain a comment saying that an error was encountered.</span></span>

<span data-ttu-id="93194-122">雖然開發人員可以自由建立自己的標籤集，[但建議的標籤中會](documentation-comments.md#recommended-tags)定義建議的集合。</span><span class="sxs-lookup"><span data-stu-id="93194-122">Although developers are free to create their own set of tags, a recommended set is defined in [Recommended tags](documentation-comments.md#recommended-tags).</span></span> <span data-ttu-id="93194-123">其中一些建議的標記具有特殊意義：</span><span class="sxs-lookup"><span data-stu-id="93194-123">Some of the recommended tags have special meanings:</span></span>

*  <span data-ttu-id="93194-124">`<param>`標記是用來描述參數。</span><span class="sxs-lookup"><span data-stu-id="93194-124">The `<param>` tag is used to describe parameters.</span></span> <span data-ttu-id="93194-125">如果使用這類標記，檔產生器必須確認指定的參數是否存在，以及檔批註中是否有所有參數的描述。</span><span class="sxs-lookup"><span data-stu-id="93194-125">If such a tag is used, the documentation generator must verify that the specified parameter exists and that all parameters are described in documentation comments.</span></span> <span data-ttu-id="93194-126">如果這類驗證失敗，檔產生器會發出警告。</span><span class="sxs-lookup"><span data-stu-id="93194-126">If such verification fails, the documentation generator issues a warning.</span></span>
*  <span data-ttu-id="93194-127">`cref` 屬性可以附加至任何標記，以提供程式碼項目的參考。</span><span class="sxs-lookup"><span data-stu-id="93194-127">The `cref` attribute can be attached to any tag to provide a reference to a code element.</span></span> <span data-ttu-id="93194-128">檔產生器必須確認此程式碼元素存在。</span><span class="sxs-lookup"><span data-stu-id="93194-128">The documentation generator must verify that this code element exists.</span></span> <span data-ttu-id="93194-129">如果驗證失敗，檔產生器會發出警告。</span><span class="sxs-lookup"><span data-stu-id="93194-129">If the verification fails, the documentation generator issues a warning.</span></span> <span data-ttu-id="93194-130">尋找`cref`屬性中所描述的名稱時，檔產生器必須`using`根據原始程式碼中出現的語句，遵循命名空間的可見度。</span><span class="sxs-lookup"><span data-stu-id="93194-130">When looking for a name described in a `cref` attribute, the documentation generator must respect namespace visibility according to `using` statements appearing within the source code.</span></span> <span data-ttu-id="93194-131">若為泛型的程式碼專案，則無法使用一般泛型語法（也`List<T>`就是 ""），因為它會產生不正確 XML。</span><span class="sxs-lookup"><span data-stu-id="93194-131">For code elements that are generic, the normal generic syntax (that is, "`List<T>`") cannot be used because it produces invalid XML.</span></span> <span data-ttu-id="93194-132">括弧可以用來取代方括弧（也就是 "`List{T}`"），也可以使用 XML escape 語法（也就是 "`List&lt;T&gt;`"）。</span><span class="sxs-lookup"><span data-stu-id="93194-132">Braces can be used instead of brackets (that is, "`List{T}`"), or the XML escape syntax can be used (that is, "`List&lt;T&gt;`").</span></span>
*  <span data-ttu-id="93194-133">`<summary>`標記可供檔檢視器用來顯示類型或成員的其他資訊。</span><span class="sxs-lookup"><span data-stu-id="93194-133">The `<summary>` tag is intended to be used by a documentation viewer to display additional information about a type or member.</span></span>
*  <span data-ttu-id="93194-134">`<include>`標記包含來自外部 XML 檔案的資訊。</span><span class="sxs-lookup"><span data-stu-id="93194-134">The `<include>` tag includes information from an external XML file.</span></span>

<span data-ttu-id="93194-135">請注意，檔檔案不會提供有關類型和成員的完整資訊（例如，它不包含任何類型資訊）。</span><span class="sxs-lookup"><span data-stu-id="93194-135">Note carefully that the documentation file does not provide full information about the type and members (for example, it does not contain any type information).</span></span> <span data-ttu-id="93194-136">若要取得類型或成員的相關資訊，檔檔案必須與實際類型或成員的反映搭配使用。</span><span class="sxs-lookup"><span data-stu-id="93194-136">To get such information about a type or member, the documentation file must be used in conjunction with reflection on the actual type or member.</span></span>

## <a name="recommended-tags"></a><span data-ttu-id="93194-137">建議的標記</span><span class="sxs-lookup"><span data-stu-id="93194-137">Recommended tags</span></span>

<span data-ttu-id="93194-138">檔產生器必須接受並根據 XML 規則來處理任何有效的標記。</span><span class="sxs-lookup"><span data-stu-id="93194-138">The documentation generator must accept and process any tag that is valid according to the rules of XML.</span></span> <span data-ttu-id="93194-139">下列標記提供使用者檔中常用的功能。</span><span class="sxs-lookup"><span data-stu-id="93194-139">The following tags provide commonly used functionality in user documentation.</span></span> <span data-ttu-id="93194-140">（當然，還有其他標記可能）。</span><span class="sxs-lookup"><span data-stu-id="93194-140">(Of course, other tags are possible.)</span></span>


| <span data-ttu-id="93194-141">__標記__</span><span class="sxs-lookup"><span data-stu-id="93194-141">__Tag__</span></span>          | <span data-ttu-id="93194-142">__區段__</span><span class="sxs-lookup"><span data-stu-id="93194-142">__Section__</span></span>                                            | <span data-ttu-id="93194-143">__目的__</span><span class="sxs-lookup"><span data-stu-id="93194-143">__Purpose__</span></span>                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | <span data-ttu-id="93194-144">使用類似程式碼的字型來設定文字</span><span class="sxs-lookup"><span data-stu-id="93194-144">Set text in a code-like font</span></span>                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | <span data-ttu-id="93194-145">設定一或多行原始程式碼或程式輸出</span><span class="sxs-lookup"><span data-stu-id="93194-145">Set one or more lines of source code or program output</span></span> |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | <span data-ttu-id="93194-146">表示範例</span><span class="sxs-lookup"><span data-stu-id="93194-146">Indicate an example</span></span>                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | <span data-ttu-id="93194-147">識別方法可以擲回的例外狀況</span><span class="sxs-lookup"><span data-stu-id="93194-147">Identifies the exceptions a method can throw</span></span>           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | <span data-ttu-id="93194-148">包含來自外部檔案的 XML</span><span class="sxs-lookup"><span data-stu-id="93194-148">Includes XML from an external file</span></span>                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | <span data-ttu-id="93194-149">建立清單或資料表</span><span class="sxs-lookup"><span data-stu-id="93194-149">Create a list or table</span></span>                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | <span data-ttu-id="93194-150">允許將結構新增至文字</span><span class="sxs-lookup"><span data-stu-id="93194-150">Permit structure to be added to text</span></span>                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | <span data-ttu-id="93194-151">描述方法或函數的參數</span><span class="sxs-lookup"><span data-stu-id="93194-151">Describe a parameter for a method or constructor</span></span>       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | <span data-ttu-id="93194-152">識別單字為參數名稱</span><span class="sxs-lookup"><span data-stu-id="93194-152">Identify that a word is a parameter name</span></span>               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | <span data-ttu-id="93194-153">記錄成員的安全性存取範圍</span><span class="sxs-lookup"><span data-stu-id="93194-153">Document the security accessibility of a member</span></span>        |
| `<remarks>`      | [`<remarks>`](documentation-comments.md#remarks)       | <span data-ttu-id="93194-154">描述類型的其他資訊</span><span class="sxs-lookup"><span data-stu-id="93194-154">Describe additional information about a type</span></span>           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | <span data-ttu-id="93194-155">描述方法的傳回值</span><span class="sxs-lookup"><span data-stu-id="93194-155">Describe the return value of a method</span></span>                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | <span data-ttu-id="93194-156">指定連結</span><span class="sxs-lookup"><span data-stu-id="93194-156">Specify a link</span></span>                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | <span data-ttu-id="93194-157">產生另請參閱專案</span><span class="sxs-lookup"><span data-stu-id="93194-157">Generate a See Also entry</span></span>                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | <span data-ttu-id="93194-158">描述類型或類型的成員</span><span class="sxs-lookup"><span data-stu-id="93194-158">Describe a type or a member of a type</span></span>                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | <span data-ttu-id="93194-159">描述屬性</span><span class="sxs-lookup"><span data-stu-id="93194-159">Describe a property</span></span>                                    |
| `<typeparam>`    |                                                        | <span data-ttu-id="93194-160">描述泛型型別參數</span><span class="sxs-lookup"><span data-stu-id="93194-160">Describe a generic type parameter</span></span>                      |
| `<typeparamref>` |                                                        | <span data-ttu-id="93194-161">識別單字是類型參數名稱</span><span class="sxs-lookup"><span data-stu-id="93194-161">Identify that a word is a type parameter name</span></span>          |

### `<c>`

<span data-ttu-id="93194-162">這個標記會提供一個機制，指出描述中的文字片段應設定為特殊字型，例如用於程式碼區塊的。</span><span class="sxs-lookup"><span data-stu-id="93194-162">This tag provides a mechanism to indicate that a fragment of text within a description should be set in a special font such as that used for a block of code.</span></span> <span data-ttu-id="93194-163">針對實際程式碼的行， `<code>`請[`<code>`](documentation-comments.md#code)使用（）。</span><span class="sxs-lookup"><span data-stu-id="93194-163">For lines of actual code, use `<code>` ([`<code>`](documentation-comments.md#code)).</span></span>

<span data-ttu-id="93194-164">__語法：__</span><span class="sxs-lookup"><span data-stu-id="93194-164">__Syntax:__</span></span>

```xml
<c>text</c>
```

<span data-ttu-id="93194-165">__範例:__</span><span class="sxs-lookup"><span data-stu-id="93194-165">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>

public class Point 
{
    // ...
}
```

### `<code>`

<span data-ttu-id="93194-166">此標記用來以某種特殊字型設定一或多行原始程式碼或程式輸出。</span><span class="sxs-lookup"><span data-stu-id="93194-166">This tag is used to set one or more lines of source code or program output in some special font.</span></span> <span data-ttu-id="93194-167">若是敘述中的小型程式碼片段， `<c>`請[`<c>`](documentation-comments.md#c)使用（）。</span><span class="sxs-lookup"><span data-stu-id="93194-167">For small code fragments in narrative, use `<c>` ([`<c>`](documentation-comments.md#c)).</span></span>

<span data-ttu-id="93194-168">__語法：__</span><span class="sxs-lookup"><span data-stu-id="93194-168">__Syntax:__</span></span>

```xml
<code>source code or program output</code>
```

<span data-ttu-id="93194-169">__範例:__</span><span class="sxs-lookup"><span data-stu-id="93194-169">__Example:__</span></span>

```csharp
/// <summary>This method changes the point's location by
///    the given x- and y-offsets.
/// <example>For example:
/// <code>
///    Point p = new Point(3,5);
///    p.Translate(-1,3);
/// </code>
/// results in <c>p</c>'s having the value (2,8).
/// </example>
/// </summary>

public void Translate(int xor, int yor) {
    X += xor;
    Y += yor;
}   
```

### `<example>`

<span data-ttu-id="93194-170">此標記允許批註中的範例程式碼，以指定方法或其他程式庫成員的使用方式。</span><span class="sxs-lookup"><span data-stu-id="93194-170">This tag allows example code within a comment, to specify how a method or other library member may be used.</span></span> <span data-ttu-id="93194-171">一般來說，這也會牽涉到使用標記`<code>` （[`<code>`](documentation-comments.md#code)）。</span><span class="sxs-lookup"><span data-stu-id="93194-171">Ordinarily, this would also involve use of the tag `<code>` ([`<code>`](documentation-comments.md#code)) as well.</span></span>

<span data-ttu-id="93194-172">__語法：__</span><span class="sxs-lookup"><span data-stu-id="93194-172">__Syntax:__</span></span>

```xml
<example>description</example>
```

<span data-ttu-id="93194-173">__範例:__</span><span class="sxs-lookup"><span data-stu-id="93194-173">__Example:__</span></span>

<span data-ttu-id="93194-174">如`<code>`需[`<code>`](documentation-comments.md#code)範例，請參閱（）。</span><span class="sxs-lookup"><span data-stu-id="93194-174">See `<code>` ([`<code>`](documentation-comments.md#code)) for an example.</span></span>

### `<exception>`

<span data-ttu-id="93194-175">此標記提供一種方式來記載方法可以擲回的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="93194-175">This tag provides a way to document the exceptions a method can throw.</span></span>

<span data-ttu-id="93194-176">__語法：__</span><span class="sxs-lookup"><span data-stu-id="93194-176">__Syntax:__</span></span>

```xml
<exception cref="member">description</exception>
```

<span data-ttu-id="93194-177">其中</span><span class="sxs-lookup"><span data-stu-id="93194-177">where</span></span>

* <span data-ttu-id="93194-178">`member`這是成員的名稱。</span><span class="sxs-lookup"><span data-stu-id="93194-178">`member` is the name of a member.</span></span> <span data-ttu-id="93194-179">檔產生器會檢查指定的成員是否存在， `member`並轉譯為檔檔中的標準專案名稱。</span><span class="sxs-lookup"><span data-stu-id="93194-179">The documentation generator checks that the given member exists and translates `member` to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="93194-180">`description`這是擲回例外狀況的情況描述。</span><span class="sxs-lookup"><span data-stu-id="93194-180">`description` is a description of the circumstances in which the exception is thrown.</span></span>

<span data-ttu-id="93194-181">__範例:__</span><span class="sxs-lookup"><span data-stu-id="93194-181">__Example:__</span></span>

```csharp
public class DataBaseOperations
{
    /// <exception cref="MasterFileFormatCorruptException"></exception>
    /// <exception cref="MasterFileLockedOpenException"></exception>
    public static void ReadRecord(int flag) {
        if (flag == 1)
            throw new MasterFileFormatCorruptException();
        else if (flag == 2)
            throw new MasterFileLockedOpenException();
        // ...
    } 
}
```

### `<include>`

<span data-ttu-id="93194-182">此標籤可讓您從原始程式碼檔外部的 XML 檔中包含資訊。</span><span class="sxs-lookup"><span data-stu-id="93194-182">This tag allows including information from an XML document that is external to the source code file.</span></span> <span data-ttu-id="93194-183">外部檔案必須是格式正確的 XML 檔，而且會套用 XPath 運算式來指定要包含在該檔中的 XML。</span><span class="sxs-lookup"><span data-stu-id="93194-183">The external file must be a well-formed XML document, and an XPath expression is applied to that document to specify what XML from that document to include.</span></span> <span data-ttu-id="93194-184">然後會使用外部檔中選取的 XML 來取代標記。`<include>`</span><span class="sxs-lookup"><span data-stu-id="93194-184">The `<include>` tag is then replaced with the selected XML from the external document.</span></span>

<span data-ttu-id="93194-185">__語法：__</span><span class="sxs-lookup"><span data-stu-id="93194-185">__Syntax:__</span></span>

```
<include file="filename" path="xpath" />
```

<span data-ttu-id="93194-186">其中</span><span class="sxs-lookup"><span data-stu-id="93194-186">where</span></span>

* <span data-ttu-id="93194-187">`filename`這是外部 XML 檔案的檔案名。</span><span class="sxs-lookup"><span data-stu-id="93194-187">`filename` is the file name of an external XML file.</span></span> <span data-ttu-id="93194-188">檔案名的解讀方式相對於包含 include 標記的檔案。</span><span class="sxs-lookup"><span data-stu-id="93194-188">The file name is interpreted relative to the file that contains the include tag.</span></span>
* <span data-ttu-id="93194-189">`xpath`是一種 XPath 運算式，可選取外部 XML 檔案中的部分 XML。</span><span class="sxs-lookup"><span data-stu-id="93194-189">`xpath` is an XPath expression that selects some of the XML in the external XML file.</span></span>

<span data-ttu-id="93194-190">__範例:__</span><span class="sxs-lookup"><span data-stu-id="93194-190">__Example:__</span></span>

<span data-ttu-id="93194-191">如果原始程式碼包含如下的宣告：</span><span class="sxs-lookup"><span data-stu-id="93194-191">If the source code contained a declaration like:</span></span>

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

<span data-ttu-id="93194-192">而外部檔案 "檔 .xml" 具有下列內容：</span><span class="sxs-lookup"><span data-stu-id="93194-192">and the external file "docs.xml" had the following contents:</span></span>

```xml
<?xml version="1.0"?>
<extradoc>
  <class name="IntList">
     <summary>
        Contains a list of integers.
     </summary>
  </class>
  <class name="StringList">
     <summary>
        Contains a list of integers.
     </summary>
  </class>
</extradoc>
```

<span data-ttu-id="93194-193">然後，相同的檔會輸出，如同原始程式碼包含：</span><span class="sxs-lookup"><span data-stu-id="93194-193">then the same documentation is output as if the source code contained:</span></span>

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

<span data-ttu-id="93194-194">此標記用來建立專案的清單或資料表。</span><span class="sxs-lookup"><span data-stu-id="93194-194">This tag is used to create a list or table of items.</span></span> <span data-ttu-id="93194-195">它可能包含`<listheader>`區塊，以定義資料表或定義清單的標題資料列。</span><span class="sxs-lookup"><span data-stu-id="93194-195">It may contain a `<listheader>` block to define the heading row of either a table or definition list.</span></span> <span data-ttu-id="93194-196">（定義資料表時，只需要提供標題`term`中的專案。）</span><span class="sxs-lookup"><span data-stu-id="93194-196">(When defining a table, only an entry for `term` in the heading need be supplied.)</span></span>

<span data-ttu-id="93194-197">清單中的每個專案都是以`<item>`區塊來指定。</span><span class="sxs-lookup"><span data-stu-id="93194-197">Each item in the list is specified with an `<item>` block.</span></span> <span data-ttu-id="93194-198">建立定義清單時，必須同時`term`指定`description`和。</span><span class="sxs-lookup"><span data-stu-id="93194-198">When creating a definition list, both `term` and `description` must be specified.</span></span> <span data-ttu-id="93194-199">不過，如果是資料表、項目符號清單或編號清單，則`description`只需要指定。</span><span class="sxs-lookup"><span data-stu-id="93194-199">However, for a table, bulleted list, or numbered list, only `description` need be specified.</span></span>

<span data-ttu-id="93194-200">__語法：__</span><span class="sxs-lookup"><span data-stu-id="93194-200">__Syntax:__</span></span>

```xml
<list type="bullet" | "number" | "table">
   <listheader>
      <term>term</term>
      <description>*description*</description>
   </listheader>
   <item>
      <term>term</term>
      <description>*description*</description>
   </item>
    ...
   <item>
      <term>term</term>
      <description>description</description>
   </item>
</list>
```

<span data-ttu-id="93194-201">其中</span><span class="sxs-lookup"><span data-stu-id="93194-201">where</span></span>

* <span data-ttu-id="93194-202">`term`這是要定義的詞彙，其定義在`description`中。</span><span class="sxs-lookup"><span data-stu-id="93194-202">`term` is the term to define, whose definition is in `description`.</span></span>
* <span data-ttu-id="93194-203">`description`是專案符號或編號清單中的專案，或的定義`term`。</span><span class="sxs-lookup"><span data-stu-id="93194-203">`description` is either an item in a bullet or numbered list, or the definition of a `term`.</span></span>

<span data-ttu-id="93194-204">__範例:__</span><span class="sxs-lookup"><span data-stu-id="93194-204">__Example:__</span></span>

```csharp
public class MyClass
{
    /// <summary>Here is an example of a bulleted list:
    /// <list type="bullet">
    /// <item>
    /// <description>Item 1.</description>
    /// </item>
    /// <item>
    /// <description>Item 2.</description>
    /// </item>
    /// </list>
    /// </summary>
    public static void Main () {
        // ...
    }
}
```

### `<para>`

<span data-ttu-id="93194-205">此標記可用於其他標記（ `<summary>`例如（[`<remarks>`](documentation-comments.md#remarks)）或`<returns>` （[`<returns>`](documentation-comments.md#returns)）），並允許將結構新增至文字。</span><span class="sxs-lookup"><span data-stu-id="93194-205">This tag is for use inside other tags, such as `<summary>` ([`<remarks>`](documentation-comments.md#remarks)) or `<returns>` ([`<returns>`](documentation-comments.md#returns)), and permits structure to be added to text.</span></span>

<span data-ttu-id="93194-206">__語法：__</span><span class="sxs-lookup"><span data-stu-id="93194-206">__Syntax:__</span></span>

```xml
<para>content</para>
```

<span data-ttu-id="93194-207">其中`content`是段落的文字。</span><span class="sxs-lookup"><span data-stu-id="93194-207">where `content` is the text of the paragraph.</span></span>

<span data-ttu-id="93194-208">__範例:__</span><span class="sxs-lookup"><span data-stu-id="93194-208">__Example:__</span></span>

```csharp
/// <summary>This is the entry point of the Point class testing program.
/// <para>This program tests each method and operator, and
/// is intended to be run after any non-trivial maintenance has
/// been performed on the Point class.</para></summary>
public static void Main() {
    // ...
}
```

### `<param>`

<span data-ttu-id="93194-209">這個標記是用來描述方法、函數或索引子的參數。</span><span class="sxs-lookup"><span data-stu-id="93194-209">This tag is used to describe a parameter for a method, constructor, or indexer.</span></span>

<span data-ttu-id="93194-210">__語法：__</span><span class="sxs-lookup"><span data-stu-id="93194-210">__Syntax:__</span></span>

```xml
<param name="name">description</param>
```

<span data-ttu-id="93194-211">其中</span><span class="sxs-lookup"><span data-stu-id="93194-211">where</span></span>

* <span data-ttu-id="93194-212">`name`這是參數的名稱。</span><span class="sxs-lookup"><span data-stu-id="93194-212">`name` is the name of the parameter.</span></span>
* <span data-ttu-id="93194-213">`description`這是參數的描述。</span><span class="sxs-lookup"><span data-stu-id="93194-213">`description` is a description of the parameter.</span></span>

<span data-ttu-id="93194-214">__範例:__</span><span class="sxs-lookup"><span data-stu-id="93194-214">__Example:__</span></span>

```csharp
/// <summary>This method changes the point's location to
///    the given coordinates.</summary>
/// <param name="xor">the new x-coordinate.</param>
/// <param name="yor">the new y-coordinate.</param>
public void Move(int xor, int yor) {
    X = xor;
    Y = yor;
}
```

### `<paramref>`

<span data-ttu-id="93194-215">此標記用來表示單字是參數。</span><span class="sxs-lookup"><span data-stu-id="93194-215">This tag is used to indicate that a word is a parameter.</span></span> <span data-ttu-id="93194-216">您可以處理檔檔案，以某種不同的方式來格式化此參數。</span><span class="sxs-lookup"><span data-stu-id="93194-216">The documentation file can be processed to format this parameter in some distinct way.</span></span>

<span data-ttu-id="93194-217">__語法：__</span><span class="sxs-lookup"><span data-stu-id="93194-217">__Syntax:__</span></span>

```xml
<paramref name="name"/>
```

<span data-ttu-id="93194-218">其中`name`是參數的名稱。</span><span class="sxs-lookup"><span data-stu-id="93194-218">where `name` is the name of the parameter.</span></span>

<span data-ttu-id="93194-219">__範例:__</span><span class="sxs-lookup"><span data-stu-id="93194-219">__Example:__</span></span>

```csharp
/// <summary>This constructor initializes the new Point to
///    (<paramref name="xor"/>,<paramref name="yor"/>).</summary>
/// <param name="xor">the new Point's x-coordinate.</param>
/// <param name="yor">the new Point's y-coordinate.</param>

public Point(int xor, int yor) {
    X = xor;
    Y = yor;
}
```

### `<permission>`

<span data-ttu-id="93194-220">此標記允許記錄成員的安全性存取範圍。</span><span class="sxs-lookup"><span data-stu-id="93194-220">This tag allows the security accessibility of a member to be documented.</span></span>

<span data-ttu-id="93194-221">__語法：__</span><span class="sxs-lookup"><span data-stu-id="93194-221">__Syntax:__</span></span>

```xml
<permission cref="member">description</permission>
```

<span data-ttu-id="93194-222">其中</span><span class="sxs-lookup"><span data-stu-id="93194-222">where</span></span>

* <span data-ttu-id="93194-223">`member`這是成員的名稱。</span><span class="sxs-lookup"><span data-stu-id="93194-223">`member` is the name of a member.</span></span> <span data-ttu-id="93194-224">檔產生器會檢查指定的程式碼專案是否存在，並將*成員*轉譯為檔檔中的標準專案名稱。</span><span class="sxs-lookup"><span data-stu-id="93194-224">The documentation generator checks that the given code element exists and translates *member* to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="93194-225">`description`這是成員存取權的描述。</span><span class="sxs-lookup"><span data-stu-id="93194-225">`description` is a description of the access to the member.</span></span>

<span data-ttu-id="93194-226">__範例:__</span><span class="sxs-lookup"><span data-stu-id="93194-226">__Example:__</span></span>

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remarks>`

<span data-ttu-id="93194-227">此標記用來指定類型的額外資訊。</span><span class="sxs-lookup"><span data-stu-id="93194-227">This tag is used to specify extra information about a type.</span></span> <span data-ttu-id="93194-228">（使用`<summary>` （[`<summary>`](documentation-comments.md#summary)）來描述類型本身和類型的成員）。</span><span class="sxs-lookup"><span data-stu-id="93194-228">(Use `<summary>` ([`<summary>`](documentation-comments.md#summary)) to describe the type itself and the members of a type.)</span></span>

<span data-ttu-id="93194-229">__語法：__</span><span class="sxs-lookup"><span data-stu-id="93194-229">__Syntax:__</span></span>

```xml
<remarks>description</remarks>
```

<span data-ttu-id="93194-230">其中`description`是批註的文字。</span><span class="sxs-lookup"><span data-stu-id="93194-230">where `description` is the text of the remark.</span></span>

<span data-ttu-id="93194-231">__範例:__</span><span class="sxs-lookup"><span data-stu-id="93194-231">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a 
/// two-dimensional plane.</summary>
/// <remarks>Uses polar coordinates</remarks>
public class Point 
{
    // ...
}
```

### `<returns>`

<span data-ttu-id="93194-232">這個標記是用來描述方法的傳回值。</span><span class="sxs-lookup"><span data-stu-id="93194-232">This tag is used to describe the return value of a method.</span></span>

<span data-ttu-id="93194-233">__語法：__</span><span class="sxs-lookup"><span data-stu-id="93194-233">__Syntax:__</span></span>

```xml
<returns>description</returns>
```

<span data-ttu-id="93194-234">其中`description` ，是傳回值的描述。</span><span class="sxs-lookup"><span data-stu-id="93194-234">where `description` is a description of the return value.</span></span>

<span data-ttu-id="93194-235">__範例:__</span><span class="sxs-lookup"><span data-stu-id="93194-235">__Example:__</span></span>

```csharp
/// <summary>Report a point's location as a string.</summary>
/// <returns>A string representing a point's location, in the form (x,y),
///    without any leading, trailing, or embedded whitespace.</returns>
public override string ToString() {
    return "(" + X + "," + Y + ")";
}
```

### `<see>`

<span data-ttu-id="93194-236">此標記允許在文字內指定連結。</span><span class="sxs-lookup"><span data-stu-id="93194-236">This tag allows a link to be specified within text.</span></span> <span data-ttu-id="93194-237">使用`<seealso>` （[`<seealso>`](documentation-comments.md#seealso)）來表示要出現在 [另請參閱] 區段中的文字。</span><span class="sxs-lookup"><span data-stu-id="93194-237">Use `<seealso>` ([`<seealso>`](documentation-comments.md#seealso)) to indicate text that is to appear in a See Also section.</span></span>

<span data-ttu-id="93194-238">__語法：__</span><span class="sxs-lookup"><span data-stu-id="93194-238">__Syntax:__</span></span>

```xml
<see cref="member"/>
```

<span data-ttu-id="93194-239">其中`member`是成員的名稱。</span><span class="sxs-lookup"><span data-stu-id="93194-239">where `member` is the name of a member.</span></span> <span data-ttu-id="93194-240">檔產生器會檢查指定的程式碼專案是否存在，並將*成員*變更為所產生檔檔案中的元素名稱。</span><span class="sxs-lookup"><span data-stu-id="93194-240">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="93194-241">__範例:__</span><span class="sxs-lookup"><span data-stu-id="93194-241">__Example:__</span></span>

```csharp
/// <summary>This method changes the point's location to
///    the given coordinates.</summary>
/// <see cref="Translate"/>
public void Move(int xor, int yor) {
    X = xor;
    Y = yor;
}

/// <summary>This method changes the point's location by
///    the given x- and y-offsets.
/// </summary>
/// <see cref="Move"/>
public void Translate(int xor, int yor) {
    X += xor;
    Y += yor;
}
```

### `<seealso>`

<span data-ttu-id="93194-242">此標記允許針對 [另請參閱] 區段產生專案。</span><span class="sxs-lookup"><span data-stu-id="93194-242">This tag allows an entry to be generated for the See Also section.</span></span> <span data-ttu-id="93194-243">使用`<see>` [（`<see>`](documentation-comments.md#see)）可指定文字內的連結。</span><span class="sxs-lookup"><span data-stu-id="93194-243">Use `<see>` ([`<see>`](documentation-comments.md#see)) to specify a link from within text.</span></span>

<span data-ttu-id="93194-244">__語法：__</span><span class="sxs-lookup"><span data-stu-id="93194-244">__Syntax:__</span></span>

```xml
<seealso cref="member"/>
```

<span data-ttu-id="93194-245">其中`member`是成員的名稱。</span><span class="sxs-lookup"><span data-stu-id="93194-245">where `member` is the name of a member.</span></span> <span data-ttu-id="93194-246">檔產生器會檢查指定的程式碼專案是否存在，並將*成員*變更為所產生檔檔案中的元素名稱。</span><span class="sxs-lookup"><span data-stu-id="93194-246">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="93194-247">__範例:__</span><span class="sxs-lookup"><span data-stu-id="93194-247">__Example:__</span></span>

```csharp
/// <summary>This method determines whether two Points have the same
///    location.</summary>
/// <seealso cref="operator=="/>
/// <seealso cref="operator!="/>
public override bool Equals(object o) {
    // ...
}
```

### `<summary>`

這個標記可以用來描述類型或類型的成員。 <span data-ttu-id="93194-249">使用`<remarks>` [（`<remarks>`](documentation-comments.md#remarks)）來描述類型本身。</span><span class="sxs-lookup"><span data-stu-id="93194-249">Use `<remarks>` ([`<remarks>`](documentation-comments.md#remarks)) to describe the type itself.</span></span>

<span data-ttu-id="93194-250">__語法：__</span><span class="sxs-lookup"><span data-stu-id="93194-250">__Syntax:__</span></span>

```xml
<summary>description</summary>
```

<span data-ttu-id="93194-251">其中`description`是類型或成員的摘要。</span><span class="sxs-lookup"><span data-stu-id="93194-251">where `description` is a summary of the type or member.</span></span>

<span data-ttu-id="93194-252">__範例:__</span><span class="sxs-lookup"><span data-stu-id="93194-252">__Example:__</span></span>

```csharp
/// <summary>This constructor initializes the new Point to (0,0).</summary>
public Point() : this(0,0) {
}
```

### `<value>`

<span data-ttu-id="93194-253">此標記可讓您描述屬性。</span><span class="sxs-lookup"><span data-stu-id="93194-253">This tag allows a property to be described.</span></span>

<span data-ttu-id="93194-254">__語法：__</span><span class="sxs-lookup"><span data-stu-id="93194-254">__Syntax:__</span></span>

```xml
<value>property description</value>
```

<span data-ttu-id="93194-255">其中`property description`是屬性的描述。</span><span class="sxs-lookup"><span data-stu-id="93194-255">where `property description` is a description for the property.</span></span>

<span data-ttu-id="93194-256">__範例:__</span><span class="sxs-lookup"><span data-stu-id="93194-256">__Example:__</span></span>

```csharp
/// <value>Property <c>X</c> represents the point's x-coordinate.</value>
public int X
{
    get { return x; }
    set { x = value; }
}
```

### `<typeparam>`

<span data-ttu-id="93194-257">這個標記是用來描述類別、結構、介面、委派或方法的泛型型別參數。</span><span class="sxs-lookup"><span data-stu-id="93194-257">This tag is used to describe a generic type parameter for a class, struct, interface, delegate, or method.</span></span>

<span data-ttu-id="93194-258">__語法：__</span><span class="sxs-lookup"><span data-stu-id="93194-258">__Syntax:__</span></span>

```xml
<typeparam name="name">description</typeparam>
```

<span data-ttu-id="93194-259">其中`name` ，是類型參數的名稱，而`description`是其描述。</span><span class="sxs-lookup"><span data-stu-id="93194-259">where `name` is the name of the type parameter, and `description` is its description.</span></span>

<span data-ttu-id="93194-260">__範例:__</span><span class="sxs-lookup"><span data-stu-id="93194-260">__Example:__</span></span>

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

<span data-ttu-id="93194-261">這個標記是用來表示單字是類型參數。</span><span class="sxs-lookup"><span data-stu-id="93194-261">This tag is used to indicate that a word is a type parameter.</span></span> <span data-ttu-id="93194-262">您可以處理檔檔案，以某種不同的方式來格式化此類型參數。</span><span class="sxs-lookup"><span data-stu-id="93194-262">The documentation file can be processed to format this type parameter in some distinct way.</span></span>

<span data-ttu-id="93194-263">__語法：__</span><span class="sxs-lookup"><span data-stu-id="93194-263">__Syntax:__</span></span>

```xml
<typeparamref name="name"/>
```

<span data-ttu-id="93194-264">其中`name`是類型參數的名稱。</span><span class="sxs-lookup"><span data-stu-id="93194-264">where `name` is the name of the type parameter.</span></span>

<span data-ttu-id="93194-265">__範例:__</span><span class="sxs-lookup"><span data-stu-id="93194-265">__Example:__</span></span>

```csharp
/// <summary>This method fetches data and returns a list of <typeparamref name="T"/>.</summary>
/// <param name="query">query to execute</param>
public List<T> FetchData<T>(string query) {
    ...
}
```

## <a name="processing-the-documentation-file"></a><span data-ttu-id="93194-266">處理檔檔案</span><span class="sxs-lookup"><span data-stu-id="93194-266">Processing the documentation file</span></span>

<span data-ttu-id="93194-267">檔產生器會針對原始程式碼中以檔註解標記的每個元素，產生一個識別碼字串。</span><span class="sxs-lookup"><span data-stu-id="93194-267">The documentation generator generates an ID string for each element in the source code that is tagged with a documentation comment.</span></span> <span data-ttu-id="93194-268">此識別碼字串可唯一識別來源元素。</span><span class="sxs-lookup"><span data-stu-id="93194-268">This ID string uniquely identifies a source element.</span></span> <span data-ttu-id="93194-269">檔檢視器可以使用識別碼字串，來識別檔適用的對應中繼資料/反映專案。</span><span class="sxs-lookup"><span data-stu-id="93194-269">A documentation viewer can use an ID string to identify the corresponding metadata/reflection item to which the documentation applies.</span></span>

<span data-ttu-id="93194-270">檔檔案不是原始程式碼的階層式標記法，相反地，它是包含每個元素所產生之識別碼字串的一般清單。</span><span class="sxs-lookup"><span data-stu-id="93194-270">The documentation file is not a hierarchical representation of the source code; rather, it is a flat list with a generated ID string for each element.</span></span>

### <a name="id-string-format"></a><span data-ttu-id="93194-271">識別碼字串格式</span><span class="sxs-lookup"><span data-stu-id="93194-271">ID string format</span></span>

<span data-ttu-id="93194-272">當產生識別碼字串時，檔產生器會遵循下列規則：</span><span class="sxs-lookup"><span data-stu-id="93194-272">The documentation generator observes the following rules when it generates the ID strings:</span></span>

*  <span data-ttu-id="93194-273">字串中未放置任何空白字元。</span><span class="sxs-lookup"><span data-stu-id="93194-273">No white space is placed in the string.</span></span>

*  <span data-ttu-id="93194-274">字串的第一個部分會識別要記載的成員種類，方法是透過單一字元，後面接著冒號。</span><span class="sxs-lookup"><span data-stu-id="93194-274">The first part of the string identifies the kind of member being documented, via a single character followed by a colon.</span></span> <span data-ttu-id="93194-275">定義了下列種類的成員：</span><span class="sxs-lookup"><span data-stu-id="93194-275">The following kinds of members are defined:</span></span>

   | <span data-ttu-id="93194-276">__字元__</span><span class="sxs-lookup"><span data-stu-id="93194-276">__Character__</span></span> | <span data-ttu-id="93194-277">__描述__</span><span class="sxs-lookup"><span data-stu-id="93194-277">__Description__</span></span>                                             |
   |---------------|-------------------------------------------------------------|
   | <span data-ttu-id="93194-278">E</span><span class="sxs-lookup"><span data-stu-id="93194-278">E</span></span>             | <span data-ttu-id="93194-279">Event - 事件</span><span class="sxs-lookup"><span data-stu-id="93194-279">Event</span></span>                                                       |
   | <span data-ttu-id="93194-280">F</span><span class="sxs-lookup"><span data-stu-id="93194-280">F</span></span>             | <span data-ttu-id="93194-281">欄位</span><span class="sxs-lookup"><span data-stu-id="93194-281">Field</span></span>                                                       |
   | <span data-ttu-id="93194-282">M</span><span class="sxs-lookup"><span data-stu-id="93194-282">M</span></span>             | <span data-ttu-id="93194-283">方法（包括函式、析構函數和運算子）</span><span class="sxs-lookup"><span data-stu-id="93194-283">Method (including constructors, destructors, and operators)</span></span> |
   | <span data-ttu-id="93194-284">N</span><span class="sxs-lookup"><span data-stu-id="93194-284">N</span></span>             | <span data-ttu-id="93194-285">命名空間</span><span class="sxs-lookup"><span data-stu-id="93194-285">Namespace</span></span>                                                   |
   | <span data-ttu-id="93194-286">P</span><span class="sxs-lookup"><span data-stu-id="93194-286">P</span></span>             | <span data-ttu-id="93194-287">屬性（包括索引子）</span><span class="sxs-lookup"><span data-stu-id="93194-287">Property (including indexers)</span></span>                               |
   | <span data-ttu-id="93194-288">T</span><span class="sxs-lookup"><span data-stu-id="93194-288">T</span></span>             | <span data-ttu-id="93194-289">類型（例如類別、委派、列舉、介面和結構）</span><span class="sxs-lookup"><span data-stu-id="93194-289">Type (such as class, delegate, enum, interface, and struct)</span></span> |
   | <span data-ttu-id="93194-290">!</span><span class="sxs-lookup"><span data-stu-id="93194-290">!</span></span>             | <span data-ttu-id="93194-291">錯誤字串;字串的其餘部分會提供錯誤的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="93194-291">Error string; the rest of the string provides information about the error.</span></span> <span data-ttu-id="93194-292">例如，檔產生器會針對無法解析的連結產生錯誤資訊。</span><span class="sxs-lookup"><span data-stu-id="93194-292">For example, the documentation generator generates error information for links that cannot be resolved.</span></span> |

*  <span data-ttu-id="93194-293">字串的第二個部分是專案的完整名稱，從命名空間的根目錄開始。</span><span class="sxs-lookup"><span data-stu-id="93194-293">The second part of the string is the fully qualified name of the element, starting at the root of the namespace.</span></span> <span data-ttu-id="93194-294">元素的名稱、其封入類型及命名空間會以句號分隔。</span><span class="sxs-lookup"><span data-stu-id="93194-294">The name of the element, its enclosing type(s), and namespace are separated by periods.</span></span> <span data-ttu-id="93194-295">如果專案的名稱具有句點，則會以`#(U+0023)`字元取代。</span><span class="sxs-lookup"><span data-stu-id="93194-295">If the name of the item itself has periods, they are replaced by `#(U+0023)` characters.</span></span> <span data-ttu-id="93194-296">（假設沒有任何元素的名稱中有這個字元）。</span><span class="sxs-lookup"><span data-stu-id="93194-296">(It is assumed that no element has this character in its name.)</span></span>
*  <span data-ttu-id="93194-297">對於具有引數的方法和屬性，引數清單會遵循，並以括弧括住。</span><span class="sxs-lookup"><span data-stu-id="93194-297">For methods and properties with arguments, the argument list follows, enclosed in parentheses.</span></span> <span data-ttu-id="93194-298">如果沒有引數，則會省略括弧。</span><span class="sxs-lookup"><span data-stu-id="93194-298">For those without arguments, the parentheses are omitted.</span></span> <span data-ttu-id="93194-299">引數會以逗號分隔。</span><span class="sxs-lookup"><span data-stu-id="93194-299">The arguments are separated by commas.</span></span> <span data-ttu-id="93194-300">每個引數的編碼方式與 CLI 簽章相同，如下所示：</span><span class="sxs-lookup"><span data-stu-id="93194-300">The encoding of each argument is the same as a CLI signature, as follows:</span></span>
   *  <span data-ttu-id="93194-301">引數是以其完整名稱為基礎的檔案名稱表示，如下所示：</span><span class="sxs-lookup"><span data-stu-id="93194-301">Arguments are represented by their documentation name, which is based on their fully qualified name, modified as follows:</span></span>
      * <span data-ttu-id="93194-302">代表泛型型別的引數具有附加`` ` ``的（倒引號）字元，後面接著類型參數的數目</span><span class="sxs-lookup"><span data-stu-id="93194-302">Arguments that represent generic types have an appended `` ` `` (backtick) character followed by the number of type parameters</span></span>
      * <span data-ttu-id="93194-303">`out`具有或`ref`修飾詞的引數`@`具有下列型別名稱。</span><span class="sxs-lookup"><span data-stu-id="93194-303">Arguments having the `out` or `ref` modifier have an `@` following their type name.</span></span> <span data-ttu-id="93194-304">以傳值方式或 via `params`傳遞的引數沒有特殊的標記法。</span><span class="sxs-lookup"><span data-stu-id="93194-304">Arguments passed by value or via `params` have no special notation.</span></span>
      * <span data-ttu-id="93194-305">當做陣列的引數會以`[lowerbound:size, ... , lowerbound:size]`逗號數目小於一的位置表示，而每個維度的下限和大小（如果已知的話）則以十進位表示。</span><span class="sxs-lookup"><span data-stu-id="93194-305">Arguments that are arrays are represented as `[lowerbound:size, ... , lowerbound:size]` where the number of commas is the rank less one, and the lower bounds and size of each dimension, if known, are represented in decimal.</span></span> <span data-ttu-id="93194-306">如果未指定下限或大小，則會省略。</span><span class="sxs-lookup"><span data-stu-id="93194-306">If a lower bound or size is not specified, it is omitted.</span></span> <span data-ttu-id="93194-307">如果省略特定維度的下限和大小， `:`也會省略。</span><span class="sxs-lookup"><span data-stu-id="93194-307">If the lower bound and size for a particular dimension are omitted, the `:` is omitted as well.</span></span> <span data-ttu-id="93194-308">不規則陣列是以每個`[]`層級的一個來表示。</span><span class="sxs-lookup"><span data-stu-id="93194-308">Jagged arrays are represented by one `[]` per level.</span></span>
      * <span data-ttu-id="93194-309">具有 void 以外之指標類型的引數會使用`*`下列類型名稱來表示。</span><span class="sxs-lookup"><span data-stu-id="93194-309">Arguments that have pointer types other than void are represented using a `*` following the type name.</span></span> <span data-ttu-id="93194-310">Void 指標是使用的型別名稱來`System.Void`表示。</span><span class="sxs-lookup"><span data-stu-id="93194-310">A void pointer is represented using a type name of `System.Void`.</span></span>
      * <span data-ttu-id="93194-311">參考在型別上定義之泛型型別參數的引數`` ` `` ，是使用（倒引號）字元來編碼，後面接著類型參數的以零為起始的索引。</span><span class="sxs-lookup"><span data-stu-id="93194-311">Arguments that refer to generic type parameters defined on types are encoded using the `` ` `` (backtick) character followed by the zero-based index of the type parameter.</span></span>
      * <span data-ttu-id="93194-312">使用方法中定義的泛型型別參數的引數會使用``` `` ```雙重倒引號， `` ` ``而不是用於類型的。</span><span class="sxs-lookup"><span data-stu-id="93194-312">Arguments that use generic type parameters defined in methods use a double-backtick ``` `` ``` instead of the `` ` `` used for types.</span></span>
      * <span data-ttu-id="93194-313">參考結構化泛型型別的引數會使用泛型型別來編碼， `{`後面接著，後面接著以逗號分隔的類型引數清單， `}`後面接著。</span><span class="sxs-lookup"><span data-stu-id="93194-313">Arguments that refer to constructed generic types are encoded using the generic type, followed by `{`, followed by a comma-separated list of type arguments, followed by `}`.</span></span>

### <a name="id-string-examples"></a><span data-ttu-id="93194-314">識別碼字串範例</span><span class="sxs-lookup"><span data-stu-id="93194-314">ID string examples</span></span>

<span data-ttu-id="93194-315">下列範例會顯示一段程式C#代碼，以及從每個來源元素產生的識別碼字串，可以有檔批註：</span><span class="sxs-lookup"><span data-stu-id="93194-315">The following examples each show a fragment of C# code, along with the ID string produced from each source element capable of having a documentation comment:</span></span>

*  <span data-ttu-id="93194-316">類型是使用其完整名稱來表示，並以一般資訊增強：</span><span class="sxs-lookup"><span data-stu-id="93194-316">Types are represented using their fully qualified name, augmented with generic information:</span></span>

   ```csharp
   enum Color { Red, Blue, Green }

   namespace Acme
   {
       interface IProcess {...}

       struct ValueType {...}

       class Widget: IProcess
       {
           public class NestedClass {...}
           public interface IMenuItem {...}
           public delegate void Del(int i);
           public enum Direction { North, South, East, West }
       }

       class MyList<T>
       {
           class Helper<U,V> {...}
       }
   }

   "T:Color"
   "T:Acme.IProcess"
   "T:Acme.ValueType"
   "T:Acme.Widget"
   "T:Acme.Widget.NestedClass"
   "T:Acme.Widget.IMenuItem"
   "T:Acme.Widget.Del"
   "T:Acme.Widget.Direction"
   "T:Acme.MyList`1"
   "T:Acme.MyList`1.Helper`2"
   ```

*  <span data-ttu-id="93194-317">欄位是以其完整名稱來表示：</span><span class="sxs-lookup"><span data-stu-id="93194-317">Fields are represented by their fully qualified name:</span></span>

   ```csharp
   namespace Acme
   {
       struct ValueType
       {
           private int total;
       }
   
       class Widget: IProcess
       {
           public class NestedClass
           {
               private int value;
           }
   
           private string message;
           private static Color defaultColor;
           private const double PI = 3.14159;
           protected readonly double monthlyAverage;
           private long[] array1;
           private Widget[,] array2;
           private unsafe int *pCount;
           private unsafe float **ppValues;
       }
   }

   "F:Acme.ValueType.total"
   "F:Acme.Widget.NestedClass.value"
   "F:Acme.Widget.message"
   "F:Acme.Widget.defaultColor"
   "F:Acme.Widget.PI"
   "F:Acme.Widget.monthlyAverage"
   "F:Acme.Widget.array1"
   "F:Acme.Widget.array2"
   "F:Acme.Widget.pCount"
   "F:Acme.Widget.ppValues"
   ```

*  <span data-ttu-id="93194-318">建構函式。</span><span class="sxs-lookup"><span data-stu-id="93194-318">Constructors.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           static Widget() {...}
           public Widget() {...}
           public Widget(string s) {...}
       }
   }

   "M:Acme.Widget.#cctor"
   "M:Acme.Widget.#ctor"
   "M:Acme.Widget.#ctor(System.String)"
   ```

*  <span data-ttu-id="93194-319">函數.</span><span class="sxs-lookup"><span data-stu-id="93194-319">Destructors.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           ~Widget() {...}
       }
   }
   
   "M:Acme.Widget.Finalize"
   ```

*  <span data-ttu-id="93194-320">方法.</span><span class="sxs-lookup"><span data-stu-id="93194-320">Methods.</span></span>

   ```csharp
   namespace Acme
   {
       struct ValueType
       {
           public void M(int i) {...}
       }

       class Widget: IProcess
       {
           public class NestedClass
           {
               public void M(int i) {...}
           }

           public static void M0() {...}
           public void M1(char c, out float f, ref ValueType v) {...}
           public void M2(short[] x1, int[,] x2, long[][] x3) {...}
           public void M3(long[][] x3, Widget[][,,] x4) {...}
           public unsafe void M4(char *pc, Color **pf) {...}
           public unsafe void M5(void *pv, double *[][,] pd) {...}
           public void M6(int i, params object[] args) {...}
       }

       class MyList<T>
       {
           public void Test(T t) { }
       }

       class UseList
       {
           public void Process(MyList<int> list) { }
           public MyList<T> GetValues<T>(T inputValue) { return null; }
       }
   }

   "M:Acme.ValueType.M(System.Int32)"
   "M:Acme.Widget.NestedClass.M(System.Int32)"
   "M:Acme.Widget.M0"
   "M:Acme.Widget.M1(System.Char,System.Single@,Acme.ValueType@)"
   "M:Acme.Widget.M2(System.Int16[],System.Int32[0:,0:],System.Int64[][])"
   "M:Acme.Widget.M3(System.Int64[][],Acme.Widget[0:,0:,0:][])"
   "M:Acme.Widget.M4(System.Char*,Color**)"
   "M:Acme.Widget.M5(System.Void*,System.Double*[0:,0:][])"
   "M:Acme.Widget.M6(System.Int32,System.Object[])"
   "M:Acme.MyList`1.Test(`0)"
   "M:Acme.UseList.Process(Acme.MyList{System.Int32})"
   "M:Acme.UseList.GetValues``(``0)"
   ```

*  <span data-ttu-id="93194-321">屬性和索引子。</span><span class="sxs-lookup"><span data-stu-id="93194-321">Properties and indexers.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public int Width { get {...} set {...} }
           public int this[int i] { get {...} set {...} }
           public int this[string s, int i] { get {...} set {...} }
       }
   }

   "P:Acme.Widget.Width"
   "P:Acme.Widget.Item(System.Int32)"
   "P:Acme.Widget.Item(System.String,System.Int32)"
   ```

*  <span data-ttu-id="93194-322">事件.</span><span class="sxs-lookup"><span data-stu-id="93194-322">Events.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public event Del AnEvent;
       }
   }

   "E:Acme.Widget.AnEvent"
   ```

*  <span data-ttu-id="93194-323">一元運算子。</span><span class="sxs-lookup"><span data-stu-id="93194-323">Unary operators.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public static Widget operator+(Widget x) {...}
       }
   }

   "M:Acme.Widget.op_UnaryPlus(Acme.Widget)"
   ```

   <span data-ttu-id="93194-324">使用的`op_UnaryPlus`一組完整一元運算子函數名稱如下：、 `op_False` `op_Decrement` `op_LogicalNot` `op_OnesComplement` `op_UnaryNegation` `op_Increment`、、、、 、和。`op_True`</span><span class="sxs-lookup"><span data-stu-id="93194-324">The complete set of unary operator function names used is as follows: `op_UnaryPlus`, `op_UnaryNegation`, `op_LogicalNot`, `op_OnesComplement`, `op_Increment`, `op_Decrement`, `op_True`, and `op_False`.</span></span>

*  <span data-ttu-id="93194-325">二元運算子。</span><span class="sxs-lookup"><span data-stu-id="93194-325">Binary operators.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public static Widget operator+(Widget x1, Widget x2) {...}
       }
   }

   "M:Acme.Widget.op_Addition(Acme.Widget,Acme.Widget)"
   ```

   <span data-ttu-id="93194-326">使用的一組完整二元運算子函式名稱如下所示`op_Addition`： `op_Subtraction`、 `op_Multiply` `op_BitwiseOr`、 `op_Division`、 `op_Modulus`、 `op_BitwiseAnd`、 `op_ExclusiveOr` `op_LeftShift` `op_RightShift`、、、、、`op_Equality`、 、、`op_LessThan`、和`op_GreaterThan`。 `op_LessThanOrEqual` `op_Inequality` `op_GreaterThanOrEqual`</span><span class="sxs-lookup"><span data-stu-id="93194-326">The complete set of binary operator function names used is as follows: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_Modulus`, `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`, `op_LeftShift`, `op_RightShift`, `op_Equality`, `op_Inequality`, `op_LessThan`, `op_LessThanOrEqual`, `op_GreaterThan`, and `op_GreaterThanOrEqual`.</span></span>

*  <span data-ttu-id="93194-327">轉換運算子的尾端是 "`~`"，後面接著傳回型別。</span><span class="sxs-lookup"><span data-stu-id="93194-327">Conversion operators have a trailing "`~`" followed by the return type.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public static explicit operator int(Widget x) {...}
           public static implicit operator long(Widget x) {...}
       }
   }

   "M:Acme.Widget.op_Explicit(Acme.Widget)~System.Int32"
   "M:Acme.Widget.op_Implicit(Acme.Widget)~System.Int64"
   ```

## <a name="an-example"></a><span data-ttu-id="93194-328">範例</span><span class="sxs-lookup"><span data-stu-id="93194-328">An example</span></span>

### <a name="c-source-code"></a><span data-ttu-id="93194-329">C#原始程式碼</span><span class="sxs-lookup"><span data-stu-id="93194-329">C# source code</span></span>

<span data-ttu-id="93194-330">下列範例顯示`Point`類別的原始程式碼：</span><span class="sxs-lookup"><span data-stu-id="93194-330">The following example shows the source code of a `Point` class:</span></span>

```csharp
namespace Graphics
{

/// <summary>Class <c>Point</c> models a point in a two-dimensional plane.
/// </summary>
public class Point 
{

    /// <summary>Instance variable <c>x</c> represents the point's
    ///    x-coordinate.</summary>
    private int x;

    /// <summary>Instance variable <c>y</c> represents the point's
    ///    y-coordinate.</summary>
    private int y;

    /// <value>Property <c>X</c> represents the point's x-coordinate.</value>
    public int X
    {
        get { return x; }
        set { x = value; }
    }

    /// <value>Property <c>Y</c> represents the point's y-coordinate.</value>
    public int Y
    {
        get { return y; }
        set { y = value; }
    }

    /// <summary>This constructor initializes the new Point to
    ///    (0,0).</summary>
    public Point() : this(0,0) {}

    /// <summary>This constructor initializes the new Point to
    ///    (<paramref name="xor"/>,<paramref name="yor"/>).</summary>
    /// <param><c>xor</c> is the new Point's x-coordinate.</param>
    /// <param><c>yor</c> is the new Point's y-coordinate.</param>
    public Point(int xor, int yor) {
        X = xor;
        Y = yor;
    }

    /// <summary>This method changes the point's location to
    ///    the given coordinates.</summary>
    /// <param><c>xor</c> is the new x-coordinate.</param>
    /// <param><c>yor</c> is the new y-coordinate.</param>
    /// <see cref="Translate"/>
    public void Move(int xor, int yor) {
        X = xor;
        Y = yor;
    }

    /// <summary>This method changes the point's location by
    ///    the given x- and y-offsets.
    /// <example>For example:
    /// <code>
    ///    Point p = new Point(3,5);
    ///    p.Translate(-1,3);
    /// </code>
    /// results in <c>p</c>'s having the value (2,8).
    /// </example>
    /// </summary>
    /// <param><c>xor</c> is the relative x-offset.</param>
    /// <param><c>yor</c> is the relative y-offset.</param>
    /// <see cref="Move"/>
    public void Translate(int xor, int yor) {
        X += xor;
        Y += yor;
    }

    /// <summary>This method determines whether two Points have the same
    ///    location.</summary>
    /// <param><c>o</c> is the object to be compared to the current object.
    /// </param>
    /// <returns>True if the Points have the same location and they have
    ///    the exact same type; otherwise, false.</returns>
    /// <seealso cref="operator=="/>
    /// <seealso cref="operator!="/>
    public override bool Equals(object o) {
        if (o == null) {
            return false;
        }

        if (this == o) {
            return true;
        }

        if (GetType() == o.GetType()) {
            Point p = (Point)o;
            return (X == p.X) && (Y == p.Y);
        }
        return false;
    }

    /// <summary>Report a point's location as a string.</summary>
    /// <returns>A string representing a point's location, in the form (x,y),
    ///    without any leading, training, or embedded whitespace.</returns>
    public override string ToString() {
        return "(" + X + "," + Y + ")";
    }

    /// <summary>This operator determines whether two Points have the same
    ///    location.</summary>
    /// <param><c>p1</c> is the first Point to be compared.</param>
    /// <param><c>p2</c> is the second Point to be compared.</param>
    /// <returns>True if the Points have the same location and they have
    ///    the exact same type; otherwise, false.</returns>
    /// <seealso cref="Equals"/>
    /// <seealso cref="operator!="/>
    public static bool operator==(Point p1, Point p2) {
        if ((object)p1 == null || (object)p2 == null) {
            return false;
        }

        if (p1.GetType() == p2.GetType()) {
            return (p1.X == p2.X) && (p1.Y == p2.Y);
        }

        return false;
    }

    /// <summary>This operator determines whether two Points have the same
    ///    location.</summary>
    /// <param><c>p1</c> is the first Point to be compared.</param>
    /// <param><c>p2</c> is the second Point to be compared.</param>
    /// <returns>True if the Points do not have the same location and the
    ///    exact same type; otherwise, false.</returns>
    /// <seealso cref="Equals"/>
    /// <seealso cref="operator=="/>
    public static bool operator!=(Point p1, Point p2) {
        return !(p1 == p2);
    }

    /// <summary>This is the entry point of the Point class testing
    /// program.
    /// <para>This program tests each method and operator, and
    /// is intended to be run after any non-trivial maintenance has
    /// been performed on the Point class.</para></summary>
    public static void Main() {
        // class test code goes here
    }
}
}
```

### <a name="resulting-xml"></a><span data-ttu-id="93194-331">產生的 XML</span><span class="sxs-lookup"><span data-stu-id="93194-331">Resulting XML</span></span>

<span data-ttu-id="93194-332">以下是一個檔產生器在提供類別`Point`的原始程式碼時所產生的輸出，如上所示：</span><span class="sxs-lookup"><span data-stu-id="93194-332">Here is the output produced by one documentation generator when given the source code for class `Point`, shown above:</span></span>

```xml
<?xml version="1.0"?>
<doc>
    <assembly>
        <name>Point</name>
    </assembly>
    <members>
        <member name="T:Graphics.Point">
            <summary>Class <c>Point</c> models a point in a two-dimensional
            plane.
            </summary>
        </member>

        <member name="F:Graphics.Point.x">
            <summary>Instance variable <c>x</c> represents the point's
            x-coordinate.</summary>
        </member>

        <member name="F:Graphics.Point.y">
            <summary>Instance variable <c>y</c> represents the point's
            y-coordinate.</summary>
        </member>

        <member name="M:Graphics.Point.#ctor">
            <summary>This constructor initializes the new Point to
        (0,0).</summary>
        </member>

        <member name="M:Graphics.Point.#ctor(System.Int32,System.Int32)">
            <summary>This constructor initializes the new Point to
            (<paramref name="xor"/>,<paramref name="yor"/>).</summary>
            <param><c>xor</c> is the new Point's x-coordinate.</param>
            <param><c>yor</c> is the new Point's y-coordinate.</param>
        </member>

        <member name="M:Graphics.Point.Move(System.Int32,System.Int32)">
            <summary>This method changes the point's location to
            the given coordinates.</summary>
            <param><c>xor</c> is the new x-coordinate.</param>
            <param><c>yor</c> is the new y-coordinate.</param>
            <see cref="M:Graphics.Point.Translate(System.Int32,System.Int32)"/>
        </member>

        <member
            name="M:Graphics.Point.Translate(System.Int32,System.Int32)">
            <summary>This method changes the point's location by
            the given x- and y-offsets.
            <example>For example:
            <code>
            Point p = new Point(3,5);
            p.Translate(-1,3);
            </code>
            results in <c>p</c>'s having the value (2,8).
            </example>
            </summary>
            <param><c>xor</c> is the relative x-offset.</param>
            <param><c>yor</c> is the relative y-offset.</param>
            <see cref="M:Graphics.Point.Move(System.Int32,System.Int32)"/>
        </member>

        <member name="M:Graphics.Point.Equals(System.Object)">
            <summary>This method determines whether two Points have the same
            location.</summary>
            <param><c>o</c> is the object to be compared to the current
            object.
            </param>
            <returns>True if the Points have the same location and they have
            the exact same type; otherwise, false.</returns>
            <seealso
      cref="M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)"/>
            <seealso
      cref="M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)"/>
        </member>

        <member name="M:Graphics.Point.ToString">
            <summary>Report a point's location as a string.</summary>
            <returns>A string representing a point's location, in the form
            (x,y),
            without any leading, training, or embedded whitespace.</returns>
        </member>

        <member
       name="M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)">
            <summary>This operator determines whether two Points have the
            same
            location.</summary>
            <param><c>p1</c> is the first Point to be compared.</param>
            <param><c>p2</c> is the second Point to be compared.</param>
            <returns>True if the Points have the same location and they have
            the exact same type; otherwise, false.</returns>
            <seealso cref="M:Graphics.Point.Equals(System.Object)"/>
            <seealso
     cref="M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)"/>
        </member>

        <member
      name="M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)">
            <summary>This operator determines whether two Points have the
            same
            location.</summary>
            <param><c>p1</c> is the first Point to be compared.</param>
            <param><c>p2</c> is the second Point to be compared.</param>
            <returns>True if the Points do not have the same location and
            the
            exact same type; otherwise, false.</returns>
            <seealso cref="M:Graphics.Point.Equals(System.Object)"/>
            <seealso
      cref="M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)"/>
        </member>

        <member name="M:Graphics.Point.Main">
            <summary>This is the entry point of the Point class testing
            program.
            <para>This program tests each method and operator, and
            is intended to be run after any non-trivial maintenance has
            been performed on the Point class.</para></summary>
        </member>

        <member name="P:Graphics.Point.X">
            <value>Property <c>X</c> represents the point's
            x-coordinate.</value>
        </member>

        <member name="P:Graphics.Point.Y">
            <value>Property <c>Y</c> represents the point's
            y-coordinate.</value>
        </member>
    </members>
</doc>
```
