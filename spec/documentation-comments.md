# <a name="documentation-comments"></a><span data-ttu-id="8aa4e-101">文件註解</span><span class="sxs-lookup"><span data-stu-id="8aa4e-101">Documentation comments</span></span>

<span data-ttu-id="8aa4e-102">C# 提供他們所使用的特殊註解語法包含 XML 文字的程式碼的文件的程式設計人員的機制。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-102">C# provides a mechanism for programmers to document their code using a special comment syntax that contains XML text.</span></span> <span data-ttu-id="8aa4e-103">在原始程式碼檔案中，具有一種特定形式的註解可用來指示要從這些註解和來源的程式碼項目，它們在前面產生的 XML 工具。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-103">In source code files, comments having a certain form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="8aa4e-104">註解使用這種語法會呼叫***文件註解***。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-104">Comments using such syntax are called ***documentation comments***.</span></span> <span data-ttu-id="8aa4e-105">後面必須緊接著使用者定義類型 （例如類別、 委派或介面） 或成員 （例如欄位、 事件、 屬性或方法）。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-105">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method).</span></span> <span data-ttu-id="8aa4e-106">XML 產生工具會呼叫***文件產生器***。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-106">The XML generation tool is called the ***documentation generator***.</span></span> <span data-ttu-id="8aa4e-107">（這個產生器可能是的但不是需要 C# 編譯器本身）。文件產生器所產生的輸出稱為***文件檔案***。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-107">(This generator could be, but need not be, the C# compiler itself.) The output produced by the documentation generator is called the ***documentation file***.</span></span> <span data-ttu-id="8aa4e-108">文件檔案做為輸入***文件檢視器***; 要產生某種類型的類型資訊和其相關聯的文件的視覺顯示的工具。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-108">A documentation file is used as input to a ***documentation viewer***; a tool intended to produce some sort of visual display of type information and its associated documentation.</span></span>

<span data-ttu-id="8aa4e-109">此規格的建議一組標記用於文件註解，這些標記的使用不必要的但其他標記可用如有需要，做為長時間的格式正確的 XML 規則遵循。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-109">This specification suggests a set of tags to be used in documentation comments, but use of these tags is not required, and other tags may be used if desired, as long the rules of well-formed XML are followed.</span></span>

## <a name="introduction"></a><span data-ttu-id="8aa4e-110">簡介</span><span class="sxs-lookup"><span data-stu-id="8aa4e-110">Introduction</span></span>

<span data-ttu-id="8aa4e-111">具有一種特殊形式的註解可以用來指示要從這些註解和來源的程式碼項目，它們在前面產生的 XML 工具。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-111">Comments having a special form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="8aa4e-112">這類的註解會啟動具有三個斜線的單行註解 (`///`)，或分隔註解開頭為斜線和兩顆星 (`/**`)。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-112">Such comments are single-line comments that start with three slashes (`///`), or delimited comments that start with a slash and two stars (`/**`).</span></span> <span data-ttu-id="8aa4e-113">後面必須緊接著使用者定義型別 （例如類別、 委派或介面） 或加註的成員 （例如欄位、 事件、 屬性或方法）。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-113">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method) that they annotate.</span></span> <span data-ttu-id="8aa4e-114">屬性區段 ([屬性規格](attributes.md#attribute-specification)) 會視為宣告的一部分，因此文件註解前面必須加上套用至類型或成員的屬性。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-114">Attribute sections ([Attribute specification](attributes.md#attribute-specification)) are considered part of declarations, so documentation comments must precede attributes applied to a type or member.</span></span>

<span data-ttu-id="8aa4e-115">__語法：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-115">__Syntax:__</span></span>

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

<span data-ttu-id="8aa4e-116">在  *single_line_doc_comment*，如果沒有*空白字元*之後的字元`///`上每個字元*single_line_doc_comment*相鄰的 s至目前*single_line_doc_comment*，，然後*空白字元*XML 輸出中不包含字元。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-116">In a *single_line_doc_comment*, if there is a *whitespace* character following the `///` characters on each of the *single_line_doc_comment*s adjacent to the current *single_line_doc_comment*, then that *whitespace* character is not included in the XML output.</span></span>

<span data-ttu-id="8aa4e-117">分隔-文件-註解中，如果在第二行的第一個非白格字元星號和選擇性空白字元和星號字元相同的模式重複出現的每個分隔-文件-註解內的行開頭然後重複模式的字元不會包含在 XML 輸出。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-117">In a delimited-doc-comment, if the first non-whitespace character on the second line is an asterisk and the same pattern of optional whitespace characters and an asterisk character is repeated at the beginning of each of the line within the delimited-doc-comment, then the characters of the repeated pattern are not included in the XML output.</span></span> <span data-ttu-id="8aa4e-118">模式之後，以及在星號字元之前，可能包含空白的字元。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-118">The pattern may include whitespace characters after, as well as before, the asterisk character.</span></span>

<span data-ttu-id="8aa4e-119">__範例：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-119">__Example:__</span></span>

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

<span data-ttu-id="8aa4e-120">內文件註解的文字必須根據規則的 XML 格式正確 (http://www.w3.org/TR/REC-xml)。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-120">The text within documentation comments must be well formed according to the rules of XML (http://www.w3.org/TR/REC-xml).</span></span> <span data-ttu-id="8aa4e-121">如果 XML 的格式不正確，則會產生警告，且文件檔案會包含註解指出發現錯誤。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-121">If the XML is ill formed, a warning is generated and the documentation file will contain a comment saying that an error was encountered.</span></span>

<span data-ttu-id="8aa4e-122">雖然開發人員可以自由建立自己的標記集，在定義一組建議[建議標記](documentation-comments.md#recommended-tags)。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-122">Although developers are free to create their own set of tags, a recommended set is defined in [Recommended tags](documentation-comments.md#recommended-tags).</span></span> <span data-ttu-id="8aa4e-123">其中一些建議的標記具有特殊意義：</span><span class="sxs-lookup"><span data-stu-id="8aa4e-123">Some of the recommended tags have special meanings:</span></span>

*  <span data-ttu-id="8aa4e-124">`<param>`標記用來描述參數。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-124">The `<param>` tag is used to describe parameters.</span></span> <span data-ttu-id="8aa4e-125">如果使用這類標記，則文件產生器必須確認指定的參數存在，而且文件註解，說明所有的參數。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-125">If such a tag is used, the documentation generator must verify that the specified parameter exists and that all parameters are described in documentation comments.</span></span> <span data-ttu-id="8aa4e-126">如果這類驗證失敗，文件產生器就會發出警告。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-126">If such verification fails, the documentation generator issues a warning.</span></span>
*  <span data-ttu-id="8aa4e-127">`cref` 屬性可以附加至任何標記，以提供程式碼項目的參考。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-127">The `cref` attribute can be attached to any tag to provide a reference to a code element.</span></span> <span data-ttu-id="8aa4e-128">文件產生器必須確認此程式碼項目存在。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-128">The documentation generator must verify that this code element exists.</span></span> <span data-ttu-id="8aa4e-129">如果驗證失敗時，文件產生器就會發出警告。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-129">If the verification fails, the documentation generator issues a warning.</span></span> <span data-ttu-id="8aa4e-130">當尋找名稱中所述`cref`屬性，文件產生器必須遵守命名空間的可見性根據`using`的原始程式碼中出現的陳述式。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-130">When looking for a name described in a `cref` attribute, the documentation generator must respect namespace visibility according to `using` statements appearing within the source code.</span></span> <span data-ttu-id="8aa4e-131">程式碼項目都是泛型，正常的一般語法 (亦即 「`List<T>`") 無法使用，因為它會產生無效的 XML。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-131">For code elements that are generic, the normal generic syntax (ie "`List<T>`") cannot be used because it produces invalid XML.</span></span> <span data-ttu-id="8aa4e-132">使用大括號來取代方括號 (即"`List{T}`")，也可以使用 XML 逸出語法 (即"`List&lt;T&gt;`」)。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-132">Braces can be used instead of brackets (ie "`List{T}`"), or the XML escape syntax can be used (ie "`List&lt;T&gt;`").</span></span>
*  <span data-ttu-id="8aa4e-133">`<summary>`標記要供文件檢視器來顯示類型或成員的其他資訊。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-133">The `<summary>` tag is intended to be used by a documentation viewer to display additional information about a type or member.</span></span>
*  <span data-ttu-id="8aa4e-134">`<include>`標記包含從外部 XML 檔的資訊。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-134">The `<include>` tag includes information from an external XML file.</span></span>

<span data-ttu-id="8aa4e-135">請小心注意文件檔案不會提供型別和成員的完整資訊 （例如，不包含任何類型資訊）。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-135">Note carefully that the documentation file does not provide full information about the type and members (for example, it does not contain any type information).</span></span> <span data-ttu-id="8aa4e-136">若要取得這類的型別或成員的相關資訊，則必須使用搭配反映實際的型別或成員的文件檔案。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-136">To get such information about a type or member, the documentation file must be used in conjunction with reflection on the actual type or member.</span></span>

## <a name="recommended-tags"></a><span data-ttu-id="8aa4e-137">建議的標記</span><span class="sxs-lookup"><span data-stu-id="8aa4e-137">Recommended tags</span></span>

<span data-ttu-id="8aa4e-138">文件產生器必須接受並處理任何標記，來根據規則的 XML 有效。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-138">The documentation generator must accept and process any tag that is valid according to the rules of XML.</span></span> <span data-ttu-id="8aa4e-139">下列標記提供使用者文件中的常用的功能。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-139">The following tags provide commonly used functionality in user documentation.</span></span> <span data-ttu-id="8aa4e-140">（當然，可能還有其他標記。）</span><span class="sxs-lookup"><span data-stu-id="8aa4e-140">(Of course, other tags are possible.)</span></span>


| <span data-ttu-id="8aa4e-141">__標記__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-141">__Tag__</span></span>          | <span data-ttu-id="8aa4e-142">__區段__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-142">__Section__</span></span>                                            | <span data-ttu-id="8aa4e-143">__目的__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-143">__Purpose__</span></span>                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | <span data-ttu-id="8aa4e-144">設定文字，以類似的程式碼的字型</span><span class="sxs-lookup"><span data-stu-id="8aa4e-144">Set text in a code-like font</span></span>                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | <span data-ttu-id="8aa4e-145">設定一或多個來源的程式碼或程式的輸出行</span><span class="sxs-lookup"><span data-stu-id="8aa4e-145">Set one or more lines of source code or program output</span></span> |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | <span data-ttu-id="8aa4e-146">指示範例</span><span class="sxs-lookup"><span data-stu-id="8aa4e-146">Indicate an example</span></span>                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | <span data-ttu-id="8aa4e-147">識別的方法可以擲回的例外狀況</span><span class="sxs-lookup"><span data-stu-id="8aa4e-147">Identifies the exceptions a method can throw</span></span>           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | <span data-ttu-id="8aa4e-148">包含從外部檔案的 XML</span><span class="sxs-lookup"><span data-stu-id="8aa4e-148">Includes XML from an external file</span></span>                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | <span data-ttu-id="8aa4e-149">建立清單或資料表</span><span class="sxs-lookup"><span data-stu-id="8aa4e-149">Create a list or table</span></span>                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | <span data-ttu-id="8aa4e-150">允許加入文字的結構</span><span class="sxs-lookup"><span data-stu-id="8aa4e-150">Permit structure to be added to text</span></span>                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | <span data-ttu-id="8aa4e-151">描述的方法或建構函式的參數</span><span class="sxs-lookup"><span data-stu-id="8aa4e-151">Describe a parameter for a method or constructor</span></span>       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | <span data-ttu-id="8aa4e-152">識別文字是參數名稱</span><span class="sxs-lookup"><span data-stu-id="8aa4e-152">Identify that a word is a parameter name</span></span>               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | <span data-ttu-id="8aa4e-153">文件的安全性存取成員</span><span class="sxs-lookup"><span data-stu-id="8aa4e-153">Document the security accessibility of a member</span></span>        |
| `<remark>`       | [`<remark>`](documentation-comments.md#remark)         | <span data-ttu-id="8aa4e-154">描述類型的其他資訊</span><span class="sxs-lookup"><span data-stu-id="8aa4e-154">Describe additional information about a type</span></span>           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | <span data-ttu-id="8aa4e-155">描述方法的傳回值</span><span class="sxs-lookup"><span data-stu-id="8aa4e-155">Describe the return value of a method</span></span>                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | <span data-ttu-id="8aa4e-156">指定的連結</span><span class="sxs-lookup"><span data-stu-id="8aa4e-156">Specify a link</span></span>                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | <span data-ttu-id="8aa4e-157">產生另請參閱項目</span><span class="sxs-lookup"><span data-stu-id="8aa4e-157">Generate a See Also entry</span></span>                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | <span data-ttu-id="8aa4e-158">描述類型成員</span><span class="sxs-lookup"><span data-stu-id="8aa4e-158">Describe a type or a member of a type</span></span>                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | <span data-ttu-id="8aa4e-159">描述屬性</span><span class="sxs-lookup"><span data-stu-id="8aa4e-159">Describe a property</span></span>                                    |
| `<typeparam>`    |                                                        | <span data-ttu-id="8aa4e-160">描述泛型類型參數</span><span class="sxs-lookup"><span data-stu-id="8aa4e-160">Describe a generic type parameter</span></span>                      |
| `<typeparamref>` |                                                        | <span data-ttu-id="8aa4e-161">識別文字是型別參數名稱</span><span class="sxs-lookup"><span data-stu-id="8aa4e-161">Identify that a word is a type parameter name</span></span>          |

### `<c>`

<span data-ttu-id="8aa4e-162">此標記會提供一個機制，表示應該在特殊的字型，例如用於程式碼區塊中設定的描述內的文字片段。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-162">This tag provides a mechanism to indicate that a fragment of text within a description should be set in a special font such as that used for a block of code.</span></span> <span data-ttu-id="8aa4e-163">實際的程式碼行，使用`<code>`([`<code>`](documentation-comments.md#code))。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-163">For lines of actual code, use `<code>` ([`<code>`](documentation-comments.md#code)).</span></span>

<span data-ttu-id="8aa4e-164">__語法：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-164">__Syntax:__</span></span>

```xml
<c>text</c>
```

<span data-ttu-id="8aa4e-165">__範例：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-165">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>

public class Point 
{
    // ...
}
```

### `<code>`

<span data-ttu-id="8aa4e-166">此標記用來設定一或多個來源的程式碼或程式的輸出行中有特殊字型。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-166">This tag is used to set one or more lines of source code or program output in some special font.</span></span> <span data-ttu-id="8aa4e-167">對於小的程式碼片段，敘述中，使用`<c>`([`<c>`](documentation-comments.md#c))。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-167">For small code fragments in narrative, use `<c>` ([`<c>`](documentation-comments.md#c)).</span></span>

<span data-ttu-id="8aa4e-168">__語法：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-168">__Syntax:__</span></span>

```xml
<code>source code or program output</code>
```

<span data-ttu-id="8aa4e-169">__範例：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-169">__Example:__</span></span>

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

<span data-ttu-id="8aa4e-170">這個標記可讓在註解，以指定的方法或其他程式庫成員可能使用方式的範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-170">This tag allows example code within a comment, to specify how a method or other library member may be used.</span></span> <span data-ttu-id="8aa4e-171">一般情況下，這也會涉及使用標記`<code>`([`<code>`](documentation-comments.md#code)) 以及。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-171">Ordinarily, this would also involve use of the tag `<code>` ([`<code>`](documentation-comments.md#code)) as well.</span></span>

<span data-ttu-id="8aa4e-172">__語法：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-172">__Syntax:__</span></span>

```xml
<example>description</example>
```

<span data-ttu-id="8aa4e-173">__範例：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-173">__Example:__</span></span>

<span data-ttu-id="8aa4e-174">請參閱`<code>`([`<code>`](documentation-comments.md#code)) 的範例。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-174">See `<code>` ([`<code>`](documentation-comments.md#code)) for an example.</span></span>

### `<exception>`

<span data-ttu-id="8aa4e-175">此標記可用來記載方法可以擲回的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-175">This tag provides a way to document the exceptions a method can throw.</span></span>

<span data-ttu-id="8aa4e-176">__語法：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-176">__Syntax:__</span></span>

```xml
<exception cref="member">description</exception>
```

<span data-ttu-id="8aa4e-177">其中</span><span class="sxs-lookup"><span data-stu-id="8aa4e-177">where</span></span>

* <span data-ttu-id="8aa4e-178">`member` 是成員的名稱。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-178">`member` is the name of a member.</span></span> <span data-ttu-id="8aa4e-179">文件產生器會檢查指定的成員存在，並將轉譯`member`文件檔案中的標準的項目名稱。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-179">The documentation generator checks that the given member exists and translates `member` to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="8aa4e-180">`description` 是描述例外狀況會擲回的情況。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-180">`description` is a description of the circumstances in which the exception is thrown.</span></span>

<span data-ttu-id="8aa4e-181">__範例：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-181">__Example:__</span></span>

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

<span data-ttu-id="8aa4e-182">這個標記可讓包括從 XML 文件外部的原始程式檔的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-182">This tag allows including information from an XML document that is external to the source code file.</span></span> <span data-ttu-id="8aa4e-183">外部檔案必須是語式正確的 XML 文件，和 XPath 運算式套用至文件會指定要包含哪些 XML 從該文件。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-183">The external file must be a well-formed XML document, and an XPath expression is applied to that document to specify what XML from that document to include.</span></span> <span data-ttu-id="8aa4e-184">`<include>`標記取代選取的 XML 從外部文件。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-184">The `<include>` tag is then replaced with the selected XML from the external document.</span></span>

<span data-ttu-id="8aa4e-185">__語法：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-185">__Syntax:__</span></span>

```
<include file="filename" path="xpath" />
```

<span data-ttu-id="8aa4e-186">其中</span><span class="sxs-lookup"><span data-stu-id="8aa4e-186">where</span></span>

* <span data-ttu-id="8aa4e-187">`filename` 為外部 XML 檔的檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-187">`filename` is the file name of an external XML file.</span></span> <span data-ttu-id="8aa4e-188">檔案名稱會解譯相對包含 include 標記的檔案。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-188">The file name is interpreted relative to the file that contains the include tag.</span></span>
* <span data-ttu-id="8aa4e-189">`xpath` 是選取某些 XML 的外部 XML 檔案中的 XPath 運算式。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-189">`xpath` is an XPath expression that selects some of the XML in the external XML file.</span></span>

<span data-ttu-id="8aa4e-190">__範例：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-190">__Example:__</span></span>

<span data-ttu-id="8aa4e-191">如果原始程式碼包含如下的宣告：</span><span class="sxs-lookup"><span data-stu-id="8aa4e-191">If the source code contained a declaration like:</span></span>

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

<span data-ttu-id="8aa4e-192">而且 「 docs.xml"的外部檔案具有下列內容：</span><span class="sxs-lookup"><span data-stu-id="8aa4e-192">and the external file "docs.xml" had the following contents:</span></span>

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

<span data-ttu-id="8aa4e-193">然後相同的文件是輸出，因為如果原始程式碼包含：</span><span class="sxs-lookup"><span data-stu-id="8aa4e-193">then the same documentation is output as if the source code contained:</span></span>

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

<span data-ttu-id="8aa4e-194">此標記用來建立清單或資料表的項目。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-194">This tag is used to create a list or table of items.</span></span> <span data-ttu-id="8aa4e-195">它可能包含`<listheader>`區塊來定義資料表或定義清單的標題資料列。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-195">It may contain a `<listheader>` block to define the heading row of either a table or definition list.</span></span> <span data-ttu-id="8aa4e-196">(當定義資料表時，只有一個項目`term`需要提供標題中。)</span><span class="sxs-lookup"><span data-stu-id="8aa4e-196">(When defining a table, only an entry for `term` in the heading need be supplied.)</span></span>

<span data-ttu-id="8aa4e-197">在清單中的每個項目指定`<item>`區塊。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-197">Each item in the list is specified with an `<item>` block.</span></span> <span data-ttu-id="8aa4e-198">建立定義清單中，當兩者`term`和`description`必須指定。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-198">When creating a definition list, both `term` and `description` must be specified.</span></span> <span data-ttu-id="8aa4e-199">不過，對於資料表、 項目符號清單或編號的清單，只`description`需要指定。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-199">However, for a table, bulleted list, or numbered list, only `description` need be specified.</span></span>

<span data-ttu-id="8aa4e-200">__語法：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-200">__Syntax:__</span></span>

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

<span data-ttu-id="8aa4e-201">其中</span><span class="sxs-lookup"><span data-stu-id="8aa4e-201">where</span></span>

* <span data-ttu-id="8aa4e-202">`term` 若要定義，其定義位於詞彙`description`。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-202">`term` is the term to define, whose definition is in `description`.</span></span>
* <span data-ttu-id="8aa4e-203">`description` 可能是項目符號或編號的清單中的項目或定義`term`。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-203">`description` is either an item in a bullet or numbered list, or the definition of a `term`.</span></span>

<span data-ttu-id="8aa4e-204">__範例：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-204">__Example:__</span></span>

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

<span data-ttu-id="8aa4e-205">此標記是內其他標記中，使用這類`<summary>`([`<remark>`](documentation-comments.md#remark)) 或`<returns>`([`<returns>`](documentation-comments.md#returns))，並允許加入文字的結構。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-205">This tag is for use inside other tags, such as `<summary>` ([`<remark>`](documentation-comments.md#remark)) or `<returns>` ([`<returns>`](documentation-comments.md#returns)), and permits structure to be added to text.</span></span>

<span data-ttu-id="8aa4e-206">__語法：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-206">__Syntax:__</span></span>

```xml
<para>content</para>
```

<span data-ttu-id="8aa4e-207">其中`content`段落的文字。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-207">where `content` is the text of the paragraph.</span></span>

<span data-ttu-id="8aa4e-208">__範例：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-208">__Example:__</span></span>

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

<span data-ttu-id="8aa4e-209">此標記用來描述方法、 建構函式或索引子的參數。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-209">This tag is used to describe a parameter for a method, constructor, or indexer.</span></span>

<span data-ttu-id="8aa4e-210">__語法：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-210">__Syntax:__</span></span>

```xml
<param name="name">description</param>
```

<span data-ttu-id="8aa4e-211">其中</span><span class="sxs-lookup"><span data-stu-id="8aa4e-211">where</span></span>

* <span data-ttu-id="8aa4e-212">`name` 為參數的名稱。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-212">`name` is the name of the parameter.</span></span>
* <span data-ttu-id="8aa4e-213">`description` 是參數的描述。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-213">`description` is a description of the parameter.</span></span>

<span data-ttu-id="8aa4e-214">__範例：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-214">__Example:__</span></span>

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

<span data-ttu-id="8aa4e-215">此標記用來指出文字為參數。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-215">This tag is used to indicate that a word is a parameter.</span></span> <span data-ttu-id="8aa4e-216">可以處理文件檔案，以某種明顯方式格式化此參數。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-216">The documentation file can be processed to format this parameter in some distinct way.</span></span>

<span data-ttu-id="8aa4e-217">__語法：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-217">__Syntax:__</span></span>

```xml
<paramref name="name"/>
```

<span data-ttu-id="8aa4e-218">其中`name`是參數的名稱。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-218">where `name` is the name of the parameter.</span></span>

<span data-ttu-id="8aa4e-219">__範例：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-219">__Example:__</span></span>

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

<span data-ttu-id="8aa4e-220">這個標記可讓安全性存取範圍的成員，才能加以記錄。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-220">This tag allows the security accessibility of a member to be documented.</span></span>

<span data-ttu-id="8aa4e-221">__語法：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-221">__Syntax:__</span></span>

```xml
<permission cref="member">description</permission>
```

<span data-ttu-id="8aa4e-222">其中</span><span class="sxs-lookup"><span data-stu-id="8aa4e-222">where</span></span>

* <span data-ttu-id="8aa4e-223">`member` 是成員的名稱。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-223">`member` is the name of a member.</span></span> <span data-ttu-id="8aa4e-224">文件產生器會檢查指定的程式碼項目存在，並將轉譯*成員*文件檔案中的標準的項目名稱。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-224">The documentation generator checks that the given code element exists and translates *member* to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="8aa4e-225">`description` 是權之成員的描述。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-225">`description` is a description of the access to the member.</span></span>

<span data-ttu-id="8aa4e-226">__範例：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-226">__Example:__</span></span>

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remark>`

<span data-ttu-id="8aa4e-227">此標記用來指定類型的相關額外資訊。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-227">This tag is used to specify extra information about a type.</span></span> <span data-ttu-id="8aa4e-228">(使用`<summary>`([`<summary>`](documentation-comments.md#summary)) 來描述本身的類型和類型的成員。)</span><span class="sxs-lookup"><span data-stu-id="8aa4e-228">(Use `<summary>` ([`<summary>`](documentation-comments.md#summary)) to describe the type itself and the members of a type.)</span></span>

<span data-ttu-id="8aa4e-229">__語法：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-229">__Syntax:__</span></span>

```xml
<remark>description</remark>
```

<span data-ttu-id="8aa4e-230">其中`description`這是備註的文字。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-230">where `description` is the text of the remark.</span></span>

<span data-ttu-id="8aa4e-231">__範例：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-231">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a 
/// two-dimensional plane.</summary>
/// <remark>Uses polar coordinates</remark>
public class Point 
{
    // ...
}
```

### `<returns>`

<span data-ttu-id="8aa4e-232">此標記用來描述方法的傳回值。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-232">This tag is used to describe the return value of a method.</span></span>

<span data-ttu-id="8aa4e-233">__語法：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-233">__Syntax:__</span></span>

```xml
<returns>description</returns>
```

<span data-ttu-id="8aa4e-234">其中`description`是傳回值的描述。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-234">where `description` is a description of the return value.</span></span>

<span data-ttu-id="8aa4e-235">__範例：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-235">__Example:__</span></span>

```csharp
/// <summary>Report a point's location as a string.</summary>
/// <returns>A string representing a point's location, in the form (x,y),
///    without any leading, trailing, or embedded whitespace.</returns>
public override string ToString() {
    return "(" + X + "," + Y + ")";
}
```

### `<see>`

<span data-ttu-id="8aa4e-236">這個標記可讓文字內指定的連結。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-236">This tag allows a link to be specified within text.</span></span> <span data-ttu-id="8aa4e-237">使用`<seealso>`([`<seealso>`](documentation-comments.md#seealso)) 來指示要出現在另請參閱 > 一節中的文字。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-237">Use `<seealso>` ([`<seealso>`](documentation-comments.md#seealso)) to indicate text that is to appear in a See Also section.</span></span>

<span data-ttu-id="8aa4e-238">__語法：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-238">__Syntax:__</span></span>

```xml
<see cref="member"/>
```

<span data-ttu-id="8aa4e-239">其中`member`是成員的名稱。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-239">where `member` is the name of a member.</span></span> <span data-ttu-id="8aa4e-240">文件產生器會檢查指定的程式碼項目存在，而且變更*成員*產生的文件檔案中的項目名稱。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-240">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="8aa4e-241">__範例：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-241">__Example:__</span></span>

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

<span data-ttu-id="8aa4e-242">這個標記可讓 「 請參閱 」 一節要產生的項目。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-242">This tag allows an entry to be generated for the See Also section.</span></span> <span data-ttu-id="8aa4e-243">使用`<see>`([`<see>`](documentation-comments.md#see)) 來指定文字中的連結。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-243">Use `<see>` ([`<see>`](documentation-comments.md#see)) to specify a link from within text.</span></span>

<span data-ttu-id="8aa4e-244">__語法：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-244">__Syntax:__</span></span>

```xml
<seealso cref="member"/>
```

<span data-ttu-id="8aa4e-245">其中`member`是成員的名稱。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-245">where `member` is the name of a member.</span></span> <span data-ttu-id="8aa4e-246">文件產生器會檢查指定的程式碼項目存在，而且變更*成員*產生的文件檔案中的項目名稱。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-246">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="8aa4e-247">__範例：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-247">__Example:__</span></span>

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

此標記可用來描述類型或類型的成員。 <span data-ttu-id="8aa4e-249">使用`<remark>`([`<remark>`](documentation-comments.md#remark)) 來描述類型本身。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-249">Use `<remark>` ([`<remark>`](documentation-comments.md#remark)) to describe the type itself.</span></span>

<span data-ttu-id="8aa4e-250">__語法：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-250">__Syntax:__</span></span>

```xml
<summary>description</summary>
```

<span data-ttu-id="8aa4e-251">其中`description`是類型或成員的摘要。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-251">where `description` is a summary of the type or member.</span></span>

<span data-ttu-id="8aa4e-252">__範例：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-252">__Example:__</span></span>

```csharp
/// <summary>This constructor initializes the new Point to (0,0).</summary>
public Point() : this(0,0) {
}
```

### `<value>`

<span data-ttu-id="8aa4e-253">這個標記可讓您描述屬性。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-253">This tag allows a property to be described.</span></span>

<span data-ttu-id="8aa4e-254">__語法：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-254">__Syntax:__</span></span>

```xml
<value>property description</value>
```

<span data-ttu-id="8aa4e-255">其中`property description`是屬性的描述。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-255">where `property description` is a description for the property.</span></span>

<span data-ttu-id="8aa4e-256">__範例：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-256">__Example:__</span></span>

```csharp
/// <value>Property <c>X</c> represents the point's x-coordinate.</value>
public int X
{
    get { return x; }
    set { x = value; }
}
```

### `<typeparam>`

<span data-ttu-id="8aa4e-257">此標記用來描述類別、 結構、 介面、 委派或方法的泛型型別參數。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-257">This tag is used to describe a generic type parameter for a class, struct, interface, delegate, or method.</span></span>

<span data-ttu-id="8aa4e-258">__語法：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-258">__Syntax:__</span></span>

```xml
<typeparam name="name">description</typeparam>
```

<span data-ttu-id="8aa4e-259">何處`name`的型別參數名稱和`description`是其描述。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-259">where `name` is the name of the type parameter, and `description` is its description.</span></span>

<span data-ttu-id="8aa4e-260">__範例：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-260">__Example:__</span></span>

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

<span data-ttu-id="8aa4e-261">此標記用來指出字組的型別參數。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-261">This tag is used to indicate that a word is a type parameter.</span></span> <span data-ttu-id="8aa4e-262">可以處理文件檔案，以某種明顯方式格式化此型別參數。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-262">The documentation file can be processed to format this type parameter in some distinct way.</span></span>

<span data-ttu-id="8aa4e-263">__語法：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-263">__Syntax:__</span></span>

```xml
<typeparamref name="name"/>
```

<span data-ttu-id="8aa4e-264">其中`name`是型別參數的名稱。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-264">where `name` is the name of the type parameter.</span></span>

<span data-ttu-id="8aa4e-265">__範例：__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-265">__Example:__</span></span>

```csharp
/// <summary>This method fetches data and returns a list of <typeparamref name="T"/>.</summary>
/// <param name="query">query to execute</param>
public List<T> FetchData<T>(string query) {
    ...
}
```

## <a name="processing-the-documentation-file"></a><span data-ttu-id="8aa4e-266">處理文件檔案</span><span class="sxs-lookup"><span data-stu-id="8aa4e-266">Processing the documentation file</span></span>

<span data-ttu-id="8aa4e-267">文件產生器產生標記的文件註解的原始程式碼中的每個項目識別碼字串。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-267">The documentation generator generates an ID string for each element in the source code that is tagged with a documentation comment.</span></span> <span data-ttu-id="8aa4e-268">此識別碼字串可唯一識別來源項目。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-268">This ID string uniquely identifies a source element.</span></span> <span data-ttu-id="8aa4e-269">文件檢視器可以使用的識別碼字串，識別要套用的文件的對應中繼資料/反映項目。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-269">A documentation viewer can use an ID string to identify the corresponding metadata/reflection item to which the documentation applies.</span></span>

<span data-ttu-id="8aa4e-270">文件檔案不是原始碼; 的階層式表示法而是與產生的識別碼字串，每個項目的一般清單。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-270">The documentation file is not a hierarchical representation of the source code; rather, it is a flat list with a generated ID string for each element.</span></span>

### <a name="id-string-format"></a><span data-ttu-id="8aa4e-271">識別碼字串格式</span><span class="sxs-lookup"><span data-stu-id="8aa4e-271">ID string format</span></span>

<span data-ttu-id="8aa4e-272">產生識別碼字串時，文件產生器就會遵守下列規則：</span><span class="sxs-lookup"><span data-stu-id="8aa4e-272">The documentation generator observes the following rules when it generates the ID strings:</span></span>

*  <span data-ttu-id="8aa4e-273">字串中未放置任何空白字元。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-273">No white space is placed in the string.</span></span>

*  <span data-ttu-id="8aa4e-274">字串的第一個部分會識別所記載，透過單一字元，後面接著冒號的成員種類。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-274">The first part of the string identifies the kind of member being documented, via a single character followed by a colon.</span></span> <span data-ttu-id="8aa4e-275">會定義下列類型的成員：</span><span class="sxs-lookup"><span data-stu-id="8aa4e-275">The following kinds of members are defined:</span></span>

   | <span data-ttu-id="8aa4e-276">__字元__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-276">__Character__</span></span> | <span data-ttu-id="8aa4e-277">__描述__</span><span class="sxs-lookup"><span data-stu-id="8aa4e-277">__Description__</span></span>                                             |
   |---------------|-------------------------------------------------------------|
   | <span data-ttu-id="8aa4e-278">E</span><span class="sxs-lookup"><span data-stu-id="8aa4e-278">E</span></span>             | <span data-ttu-id="8aa4e-279">Event - 事件</span><span class="sxs-lookup"><span data-stu-id="8aa4e-279">Event</span></span>                                                       |
   | <span data-ttu-id="8aa4e-280">F</span><span class="sxs-lookup"><span data-stu-id="8aa4e-280">F</span></span>             | <span data-ttu-id="8aa4e-281">欄位</span><span class="sxs-lookup"><span data-stu-id="8aa4e-281">Field</span></span>                                                       |
   | <span data-ttu-id="8aa4e-282">M</span><span class="sxs-lookup"><span data-stu-id="8aa4e-282">M</span></span>             | <span data-ttu-id="8aa4e-283">方法 （包括建構函式、 解構函式和運算子）</span><span class="sxs-lookup"><span data-stu-id="8aa4e-283">Method (including constructors, destructors, and operators)</span></span> |
   | <span data-ttu-id="8aa4e-284">N</span><span class="sxs-lookup"><span data-stu-id="8aa4e-284">N</span></span>             | <span data-ttu-id="8aa4e-285">命名空間</span><span class="sxs-lookup"><span data-stu-id="8aa4e-285">Namespace</span></span>                                                   |
   | <span data-ttu-id="8aa4e-286">P</span><span class="sxs-lookup"><span data-stu-id="8aa4e-286">P</span></span>             | <span data-ttu-id="8aa4e-287">屬性 （包括索引子）</span><span class="sxs-lookup"><span data-stu-id="8aa4e-287">Property (including indexers)</span></span>                               |
   | <span data-ttu-id="8aa4e-288">T</span><span class="sxs-lookup"><span data-stu-id="8aa4e-288">T</span></span>             | <span data-ttu-id="8aa4e-289">型別 （例如類別、 委派、 列舉、 介面和結構）</span><span class="sxs-lookup"><span data-stu-id="8aa4e-289">Type (such as class, delegate, enum, interface, and struct)</span></span> |
   | <span data-ttu-id="8aa4e-290">!</span><span class="sxs-lookup"><span data-stu-id="8aa4e-290">!</span></span>             | <span data-ttu-id="8aa4e-291">錯誤字串;字串的其餘部分會提供錯誤的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-291">Error string; the rest of the string provides information about the error.</span></span> <span data-ttu-id="8aa4e-292">例如，文件產生器會產生無法解析的連結資訊時發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-292">For example, the documentation generator generates error information for links that cannot be resolved.</span></span> |

*  <span data-ttu-id="8aa4e-293">字串的第二個部分是在命名空間的根開始之項目的完整的名稱。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-293">The second part of the string is the fully qualified name of the element, starting at the root of the namespace.</span></span> <span data-ttu-id="8aa4e-294">項目、 其封入類型，以及命名空間的名稱並以句號分隔。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-294">The name of the element, its enclosing type(s), and namespace are separated by periods.</span></span> <span data-ttu-id="8aa4e-295">如果項目本身的名稱包含句點，它們會被取代的`#(U+0023)`字元。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-295">If the name of the item itself has periods, they are replaced by `#(U+0023)` characters.</span></span> <span data-ttu-id="8aa4e-296">（它會假設沒有任何項目，其名稱中有這個字元）。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-296">(It is assumed that no element has this character in its name.)</span></span>
*  <span data-ttu-id="8aa4e-297">適用於方法和引數時，引數清單如下所示，括號括住的屬性。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-297">For methods and properties with arguments, the argument list follows, enclosed in parentheses.</span></span> <span data-ttu-id="8aa4e-298">對於不含引數，就會省略括弧。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-298">For those without arguments, the parentheses are omitted.</span></span> <span data-ttu-id="8aa4e-299">引數會以逗號分隔。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-299">The arguments are separated by commas.</span></span> <span data-ttu-id="8aa4e-300">每個引數的編碼相同 CLI 簽章，如下所示：</span><span class="sxs-lookup"><span data-stu-id="8aa4e-300">The encoding of each argument is the same as a CLI signature, as follows:</span></span>
   *  <span data-ttu-id="8aa4e-301">引數會以其文件的名稱，此作業取決於其完整名稱，修改，如下所示：</span><span class="sxs-lookup"><span data-stu-id="8aa4e-301">Arguments are represented by their documentation name, which is based on their fully qualified name, modified as follows:</span></span>
      * <span data-ttu-id="8aa4e-302">代表泛型類型引數具有附加"'"字元後面的型別參數數目</span><span class="sxs-lookup"><span data-stu-id="8aa4e-302">Arguments that represent generic types have an appended "'" character followed by the number of type parameters</span></span>
      * <span data-ttu-id="8aa4e-303">引數具有`out`或是`ref`修飾詞具有`@`遵循其型別名稱。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-303">Arguments having the `out` or `ref` modifier have an `@` following their type name.</span></span> <span data-ttu-id="8aa4e-304">傳遞引數傳值方式或透過`params`有任何特殊的標記法。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-304">Arguments passed by value or via `params` have no special notation.</span></span>
      * <span data-ttu-id="8aa4e-305">是陣列的引數會表示為`[lowerbound:size, ... , lowerbound:size]`其中逗號數目是以下其中一個，陣序規範，而下限和每個維度大小，如果已知，會以十進位。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-305">Arguments that are arrays are represented as `[lowerbound:size, ... , lowerbound:size]` where the number of commas is the rank less one, and the lower bounds and size of each dimension, if known, are represented in decimal.</span></span> <span data-ttu-id="8aa4e-306">如果未指定下限或大小，會將其省略。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-306">If a lower bound or size is not specified, it is omitted.</span></span> <span data-ttu-id="8aa4e-307">如果省略的下限和大小的特定維度，「`:`」 也會省略。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-307">If the lower bound and size for a particular dimension are omitted, the "`:`" is omitted as well.</span></span> <span data-ttu-id="8aa4e-308">不規則的陣列由其中一個 「`[]`」 每個層級。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-308">Jagged arrays are represented by one "`[]`" per level.</span></span>
      * <span data-ttu-id="8aa4e-309">具有非 void 的指標類型的引數都使用代表`*`遵循的型別名稱。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-309">Arguments that have pointer types other than void are represented using a `*` following the type name.</span></span> <span data-ttu-id="8aa4e-310">Void 的指標表示使用的型別名稱`System.Void`。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-310">A void pointer is represented using a type name of `System.Void`.</span></span>
      * <span data-ttu-id="8aa4e-311">參考類型上定義的泛型型別參數的引數使用編碼"'"字元後面接著型別參數的以零為起始的索引。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-311">Arguments that refer to generic type parameters defined on types are encoded using the "\`" character followed by the zero-based index of the type parameter.</span></span>
      * <span data-ttu-id="8aa4e-312">使用在方法中定義的泛型類型參數的引數使用雙倒 」\`\`"而不是"\`"用於類型。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-312">Arguments that use generic type parameters defined in methods use a double-backtick "\`\`" instead of the "\`" used for types.</span></span>
      * <span data-ttu-id="8aa4e-313">使用泛型型別，後面接著編碼參考建構的泛型類型引數"{"，後面接著逗號分隔的清單型別引數，再接著"}"。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-313">Arguments that refer to constructed generic types are encoded using the generic type, followed by "{", followed by a comma-separated list of type arguments, followed by "}".</span></span>

### <a name="id-string-examples"></a><span data-ttu-id="8aa4e-314">識別碼字串範例</span><span class="sxs-lookup"><span data-stu-id="8aa4e-314">ID string examples</span></span>

<span data-ttu-id="8aa4e-315">下列範例各自都顯示 C# 程式碼片段，以及每個來源項目能夠使文件註解產生的識別碼字串：</span><span class="sxs-lookup"><span data-stu-id="8aa4e-315">The following examples each show a fragment of C# code, along with the ID string produced from each source element capable of having a documentation comment:</span></span>

*  <span data-ttu-id="8aa4e-316">類型被表示使用其完整的名稱，夾帶的一般資訊：</span><span class="sxs-lookup"><span data-stu-id="8aa4e-316">Types are represented using their fully qualified name, augmented with generic information:</span></span>

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

*  <span data-ttu-id="8aa4e-317">欄位會以其完整名稱表示：</span><span class="sxs-lookup"><span data-stu-id="8aa4e-317">Fields are represented by their fully qualified name:</span></span>

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

*  <span data-ttu-id="8aa4e-318">建構函式。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-318">Constructors.</span></span>

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

*  <span data-ttu-id="8aa4e-319">解構函式。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-319">Destructors.</span></span>

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

*  <span data-ttu-id="8aa4e-320">方法。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-320">Methods.</span></span>

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

*  <span data-ttu-id="8aa4e-321">屬性與索引子。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-321">Properties and indexers.</span></span>

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

*  <span data-ttu-id="8aa4e-322">事件。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-322">Events.</span></span>

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

*  <span data-ttu-id="8aa4e-323">一元運算子。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-323">Unary operators.</span></span>

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

   <span data-ttu-id="8aa4e-324">一組完整的一元運算子函式名稱使用如下所示： `op_UnaryPlus`， `op_UnaryNegation`， `op_LogicalNot`， `op_OnesComplement`， `op_Increment`， `op_Decrement`， `op_True`，以及`op_False`。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-324">The complete set of unary operator function names used is as follows: `op_UnaryPlus`, `op_UnaryNegation`, `op_LogicalNot`, `op_OnesComplement`, `op_Increment`, `op_Decrement`, `op_True`, and `op_False`.</span></span>

*  <span data-ttu-id="8aa4e-325">二元運算子。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-325">Binary operators.</span></span>

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

   <span data-ttu-id="8aa4e-326">一組完整的二元運算子函式名稱使用如下所示： `op_Addition`， `op_Subtraction`， `op_Multiply`， `op_Division`， `op_Modulus`， `op_BitwiseAnd`， `op_BitwiseOr`， `op_ExclusiveOr`， `op_LeftShift`， `op_RightShift`，`op_Equality`， `op_Inequality`， `op_LessThan`， `op_LessThanOrEqual`， `op_GreaterThan`，和`op_GreaterThanOrEqual`。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-326">The complete set of binary operator function names used is as follows: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_Modulus`, `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`, `op_LeftShift`, `op_RightShift`, `op_Equality`, `op_Inequality`, `op_LessThan`, `op_LessThanOrEqual`, `op_GreaterThan`, and `op_GreaterThanOrEqual`.</span></span>

*  <span data-ttu-id="8aa4e-327">轉換運算子可讓您有尾端"`~`"後面接著的傳回型別。</span><span class="sxs-lookup"><span data-stu-id="8aa4e-327">Conversion operators have a trailing "`~`" followed by the return type.</span></span>

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

## <a name="an-example"></a><span data-ttu-id="8aa4e-328">範例</span><span class="sxs-lookup"><span data-stu-id="8aa4e-328">An example</span></span>

### <a name="c-source-code"></a><span data-ttu-id="8aa4e-329">C# 原始程式碼</span><span class="sxs-lookup"><span data-stu-id="8aa4e-329">C# source code</span></span>

<span data-ttu-id="8aa4e-330">下列範例顯示的原始程式碼`Point`類別：</span><span class="sxs-lookup"><span data-stu-id="8aa4e-330">The following example shows the source code of a `Point` class:</span></span>

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

### <a name="resulting-xml"></a><span data-ttu-id="8aa4e-331">產生的 XML</span><span class="sxs-lookup"><span data-stu-id="8aa4e-331">Resulting XML</span></span>

<span data-ttu-id="8aa4e-332">以下是一個文件產生器類別中指定的原始碼時所產生的輸出`Point`，如上所示：</span><span class="sxs-lookup"><span data-stu-id="8aa4e-332">Here is the output produced by one documentation generator when given the source code for class `Point`, shown above:</span></span>

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
