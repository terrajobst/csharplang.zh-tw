---
ms.openlocfilehash: 4676bcd3f0a92260b4e5e20a0aa5b5ec00bf204e
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704064"
---
# <a name="lexical-structure"></a><span data-ttu-id="978b7-101">語彙結構</span><span class="sxs-lookup"><span data-stu-id="978b7-101">Lexical structure</span></span>

## <a name="programs"></a><span data-ttu-id="978b7-102">Programs</span><span class="sxs-lookup"><span data-stu-id="978b7-102">Programs</span></span>

<span data-ttu-id="978b7-103">C# ***程式***是由一或多個***原始***程式檔所組成，已知正式為***編譯單位***（[編譯單位](namespaces.md#compilation-units)）。</span><span class="sxs-lookup"><span data-stu-id="978b7-103">A C# ***program*** consists of one or more ***source files***, known formally as ***compilation units*** ([Compilation units](namespaces.md#compilation-units)).</span></span> <span data-ttu-id="978b7-104">原始程式檔是 Unicode 字元的排序序列。</span><span class="sxs-lookup"><span data-stu-id="978b7-104">A source file is an ordered sequence of Unicode characters.</span></span> <span data-ttu-id="978b7-105">來源檔案通常與檔案系統中的檔案具有一對一的對應關係，但不需要這項對應。</span><span class="sxs-lookup"><span data-stu-id="978b7-105">Source files typically have a one-to-one correspondence with files in a file system, but this correspondence is not required.</span></span> <span data-ttu-id="978b7-106">若要獲得最大的可攜性，建議使用 UTF-8 編碼來編碼檔案系統中的檔案。</span><span class="sxs-lookup"><span data-stu-id="978b7-106">For maximal portability, it is recommended that files in a file system be encoded with the UTF-8 encoding.</span></span>

<span data-ttu-id="978b7-107">就概念上而言，程式是使用三個步驟來編譯：</span><span class="sxs-lookup"><span data-stu-id="978b7-107">Conceptually speaking, a program is compiled using three steps:</span></span>

1. <span data-ttu-id="978b7-108">轉換，將檔案從特定字元大型和編碼配置轉換成 Unicode 字元序列。</span><span class="sxs-lookup"><span data-stu-id="978b7-108">Transformation, which converts a file from a particular character repertoire and encoding scheme into a sequence of Unicode characters.</span></span>
2. <span data-ttu-id="978b7-109">詞法分析，會將 Unicode 輸入字元的資料流程轉譯為權杖串流。</span><span class="sxs-lookup"><span data-stu-id="978b7-109">Lexical analysis, which translates a stream of Unicode input characters into a stream of tokens.</span></span>
3. <span data-ttu-id="978b7-110">語法分析，會將權杖串流轉譯成可執行檔程式碼。</span><span class="sxs-lookup"><span data-stu-id="978b7-110">Syntactic analysis, which translates the stream of tokens into executable code.</span></span>

## <a name="grammars"></a><span data-ttu-id="978b7-111">語法</span><span class="sxs-lookup"><span data-stu-id="978b7-111">Grammars</span></span>

<span data-ttu-id="978b7-112">此規格會使用兩個文法C#來呈現程式設計語言的語法。</span><span class="sxs-lookup"><span data-stu-id="978b7-112">This specification presents the syntax of the C# programming language using two grammars.</span></span> <span data-ttu-id="978b7-113">「***詞法文法***」（[詞法](lexical-structure.md#lexical-grammar)文法）定義如何將 Unicode 字元結合成表單行結束字元、空白字元、批註、標記和前置處理指示詞。</span><span class="sxs-lookup"><span data-stu-id="978b7-113">The ***lexical grammar*** ([Lexical grammar](lexical-structure.md#lexical-grammar)) defines how Unicode characters are combined to form line terminators, white space, comments, tokens, and pre-processing directives.</span></span> <span data-ttu-id="978b7-114">***語法文法***（[語法文法](lexical-structure.md#syntactic-grammar)）定義如何將詞彙文法產生的標記結合成表單C#程式。</span><span class="sxs-lookup"><span data-stu-id="978b7-114">The ***syntactic grammar*** ([Syntactic grammar](lexical-structure.md#syntactic-grammar)) defines how the tokens resulting from the lexical grammar are combined to form C# programs.</span></span>

### <a name="grammar-notation"></a><span data-ttu-id="978b7-115">文法標記法</span><span class="sxs-lookup"><span data-stu-id="978b7-115">Grammar notation</span></span>

<span data-ttu-id="978b7-116">詞法和語法文法會使用 ANTLR 文法工具的標記法，以巴克斯 Backus-naur 形式呈現。</span><span class="sxs-lookup"><span data-stu-id="978b7-116">The lexical and syntactic grammars are presented in Backus-Naur form using the notation of the ANTLR grammar tool.</span></span>

### <a name="lexical-grammar"></a><span data-ttu-id="978b7-117">語彙文法</span><span class="sxs-lookup"><span data-stu-id="978b7-117">Lexical grammar</span></span>

<span data-ttu-id="978b7-118">的詞法文法會C#以[詞法分析](lexical-structure.md#lexical-analysis)、[標記](lexical-structure.md#tokens)和[前置處理](lexical-structure.md#pre-processing-directives)指示詞來呈現。</span><span class="sxs-lookup"><span data-stu-id="978b7-118">The lexical grammar of C# is presented in [Lexical analysis](lexical-structure.md#lexical-analysis), [Tokens](lexical-structure.md#tokens), and [Pre-processing directives](lexical-structure.md#pre-processing-directives).</span></span> <span data-ttu-id="978b7-119">詞法文法的終端符號是 Unicode 字元集的字元，而詞法文法會指定如何將字元結合成表單標記（[標記](lexical-structure.md#tokens)）、空白字元（[空白字元](lexical-structure.md#white-space)）、批註（[批註](lexical-structure.md#comments)）和前置處理指示詞（[前置處理指示](lexical-structure.md#pre-processing-directives)詞）。</span><span class="sxs-lookup"><span data-stu-id="978b7-119">The terminal symbols of the lexical grammar are the characters of the Unicode character set, and the lexical grammar specifies how characters are combined to form tokens ([Tokens](lexical-structure.md#tokens)), white space ([White space](lexical-structure.md#white-space)), comments ([Comments](lexical-structure.md#comments)), and pre-processing directives ([Pre-processing directives](lexical-structure.md#pre-processing-directives)).</span></span>

<span data-ttu-id="978b7-120">程式中的每個來源檔案都必須符合詞法文法的*輸入*生產（[詞法分析](lexical-structure.md#lexical-analysis)）。 C#</span><span class="sxs-lookup"><span data-stu-id="978b7-120">Every source file in a C# program must conform to the *input* production of the lexical grammar ([Lexical analysis](lexical-structure.md#lexical-analysis)).</span></span>

### <a name="syntactic-grammar"></a><span data-ttu-id="978b7-121">語法文法</span><span class="sxs-lookup"><span data-stu-id="978b7-121">Syntactic grammar</span></span>

<span data-ttu-id="978b7-122">的語法文法C#會在本章後續的章節和附錄中提供。</span><span class="sxs-lookup"><span data-stu-id="978b7-122">The syntactic grammar of C# is presented in the chapters and appendices that follow this chapter.</span></span> <span data-ttu-id="978b7-123">語法文法的終端符號是由詞法文法所定義的標記，而語法文法則指定如何將標記結合成表單C#程式。</span><span class="sxs-lookup"><span data-stu-id="978b7-123">The terminal symbols of the syntactic grammar are the tokens defined by the lexical grammar, and the syntactic grammar specifies how tokens are combined to form C# programs.</span></span>

<span data-ttu-id="978b7-124">程式中的每個C#來源檔案都必須符合語法文法（[編譯單位](namespaces.md#compilation-units)）的*compilation_unit*實際執行。</span><span class="sxs-lookup"><span data-stu-id="978b7-124">Every source file in a C# program must conform to the *compilation_unit* production of the syntactic grammar ([Compilation units](namespaces.md#compilation-units)).</span></span>

## <a name="lexical-analysis"></a><span data-ttu-id="978b7-125">詞法分析</span><span class="sxs-lookup"><span data-stu-id="978b7-125">Lexical analysis</span></span>

<span data-ttu-id="978b7-126">*輸入*生產會定義C#原始檔的詞法結構。</span><span class="sxs-lookup"><span data-stu-id="978b7-126">The *input* production defines the lexical structure of a C# source file.</span></span> <span data-ttu-id="978b7-127">程式中的C#每個來源檔案都必須符合此詞法文法生產。</span><span class="sxs-lookup"><span data-stu-id="978b7-127">Each source file in a C# program must conform to this lexical grammar production.</span></span>

```antlr
input
    : input_section?
    ;

input_section
    : input_section_part+
    ;

input_section_part
    : input_element* new_line
    | pp_directive
    ;

input_element
    : whitespace
    | comment
    | token
    ;
```

<span data-ttu-id="978b7-128">五個基本元素組成C#原始檔的詞法結構：行結束字元（[行結束字元](lexical-structure.md#line-terminators)）、空白字元（[空白字元](lexical-structure.md#white-space)）、批註（[批註](lexical-structure.md#comments)）、標記（[標記](lexical-structure.md#tokens)）和前置處理指示詞（[前置處理](lexical-structure.md#pre-processing-directives)指示詞）。</span><span class="sxs-lookup"><span data-stu-id="978b7-128">Five basic elements make up the lexical structure of a C# source file: Line terminators ([Line terminators](lexical-structure.md#line-terminators)), white space ([White space](lexical-structure.md#white-space)), comments ([Comments](lexical-structure.md#comments)), tokens ([Tokens](lexical-structure.md#tokens)), and pre-processing directives ([Pre-processing directives](lexical-structure.md#pre-processing-directives)).</span></span> <span data-ttu-id="978b7-129">在這些基本元素中，只有標記在C#程式的語法文法中是很重要的（[語法文法](lexical-structure.md#syntactic-grammar)）。</span><span class="sxs-lookup"><span data-stu-id="978b7-129">Of these basic elements, only tokens are significant in the syntactic grammar of a C# program ([Syntactic grammar](lexical-structure.md#syntactic-grammar)).</span></span>

<span data-ttu-id="978b7-130">C#來源檔案的詞法處理包含將檔案縮減成一系列標記，以成為語法分析的輸入。</span><span class="sxs-lookup"><span data-stu-id="978b7-130">The lexical processing of a C# source file consists of reducing the file into a sequence of tokens which becomes the input to the syntactic analysis.</span></span> <span data-ttu-id="978b7-131">行結束字元、空白字元和批註可以用來分隔標記，而前置處理指示詞可能會導致略過原始C#程式檔的區段，但這些詞法元素也不會影響程式的語法結構。</span><span class="sxs-lookup"><span data-stu-id="978b7-131">Line terminators, white space, and comments can serve to separate tokens, and pre-processing directives can cause sections of the source file to be skipped, but otherwise these lexical elements have no impact on the syntactic structure of a C# program.</span></span>

<span data-ttu-id="978b7-132">在插入字串常值（插入[字串常](lexical-structure.md#interpolated-string-literals)值）的情況下，單一 token 一開始是由詞法分析所產生，但會細分為多個輸入元素，這些專案會重複受到詞法分析，直到所有插入已解析字串常值。</span><span class="sxs-lookup"><span data-stu-id="978b7-132">In the case of interpolated string literals ([Interpolated string literals](lexical-structure.md#interpolated-string-literals)) a single token is initially produced by lexical analysis, but is broken up into several input elements which are repeatedly subjected to lexical analysis until all interpolated string literals have been resolved.</span></span> <span data-ttu-id="978b7-133">然後，產生的權杖會作為語法分析的輸入。</span><span class="sxs-lookup"><span data-stu-id="978b7-133">The resulting tokens then serve as input to the syntactic analysis.</span></span>

<span data-ttu-id="978b7-134">當數個詞法文法生產符合原始檔中的字元序列時，詞法處理一律會形成最長可能的詞法元素。</span><span class="sxs-lookup"><span data-stu-id="978b7-134">When several lexical grammar productions match a sequence of characters in a source file, the lexical processing always forms the longest possible lexical element.</span></span> <span data-ttu-id="978b7-135">例如，字元序列`//`會當做單行批註的開頭來處理，因為該詞法元素的長度超過單一`/`標記。</span><span class="sxs-lookup"><span data-stu-id="978b7-135">For example, the character sequence `//` is processed as the beginning of a single-line comment because that lexical element is longer than a single `/` token.</span></span>

### <a name="line-terminators"></a><span data-ttu-id="978b7-136">行結束字元</span><span class="sxs-lookup"><span data-stu-id="978b7-136">Line terminators</span></span>

<span data-ttu-id="978b7-137">行結束字元會將C#原始程式檔的字元分成幾行。</span><span class="sxs-lookup"><span data-stu-id="978b7-137">Line terminators divide the characters of a C# source file into lines.</span></span>

```antlr
new_line
    : '<Carriage return character (U+000D)>'
    | '<Line feed character (U+000A)>'
    | '<Carriage return character (U+000D) followed by line feed character (U+000A)>'
    | '<Next line character (U+0085)>'
    | '<Line separator character (U+2028)>'
    | '<Paragraph separator character (U+2029)>'
    ;
```

<span data-ttu-id="978b7-138">為了與可新增檔案結尾標記的原始程式碼編輯工具相容，並讓原始C#程式檔被視為一系列適當終止的行，下列轉換會依序套用到程式中的每個來源檔案：</span><span class="sxs-lookup"><span data-stu-id="978b7-138">For compatibility with source code editing tools that add end-of-file markers, and to enable a source file to be viewed as a sequence of properly terminated lines, the following transformations are applied, in order, to every source file in a C# program:</span></span>

*  <span data-ttu-id="978b7-139">如果來源檔案的最後一個字元是控制 z 字元（`U+001A`），就會刪除這個字元。</span><span class="sxs-lookup"><span data-stu-id="978b7-139">If the last character of the source file is a Control-Z character (`U+001A`), this character is deleted.</span></span>
*  <span data-ttu-id="978b7-140">如果原始程式檔不是`U+000D`空的，而且原始程式檔的最後一個字元不是回車（`U+000D`）、換行（`U+000A`）、分行符號（）、分行符號（），則會將換行`U+2028`字元（）加入來源檔案的結尾。）或段落分隔符號（`U+2029`）。</span><span class="sxs-lookup"><span data-stu-id="978b7-140">A carriage-return character (`U+000D`) is added to the end of the source file if that source file is non-empty and if the last character of the source file is not a carriage return (`U+000D`), a line feed (`U+000A`), a line separator (`U+2028`), or a paragraph separator (`U+2029`).</span></span>

### <a name="comments"></a><span data-ttu-id="978b7-141">註解</span><span class="sxs-lookup"><span data-stu-id="978b7-141">Comments</span></span>

<span data-ttu-id="978b7-142">支援兩種形式的批註：單行批註和分隔批註。</span><span class="sxs-lookup"><span data-stu-id="978b7-142">Two forms of comments are supported: single-line comments and delimited comments.</span></span> <span data-ttu-id="978b7-143">***單行批註***會以字元`//`開頭，並延伸至原始程式列的結尾。</span><span class="sxs-lookup"><span data-stu-id="978b7-143">***Single-line comments*** start with the characters `//` and extend to the end of the source line.</span></span> <span data-ttu-id="978b7-144">***分隔的批註***會以字元`/*`開頭，並以字元`*/`結尾。</span><span class="sxs-lookup"><span data-stu-id="978b7-144">***Delimited comments*** start with the characters `/*` and end with the characters `*/`.</span></span> <span data-ttu-id="978b7-145">分隔的批註可能會跨越多行。</span><span class="sxs-lookup"><span data-stu-id="978b7-145">Delimited comments may span multiple lines.</span></span>

```antlr
comment
    : single_line_comment
    | delimited_comment
    ;

single_line_comment
    : '//' input_character*
    ;

input_character
    : '<Any Unicode character except a new_line_character>'
    ;

new_line_character
    : '<Carriage return character (U+000D)>'
    | '<Line feed character (U+000A)>'
    | '<Next line character (U+0085)>'
    | '<Line separator character (U+2028)>'
    | '<Paragraph separator character (U+2029)>'
    ;

delimited_comment
    : '/*' delimited_comment_section* asterisk+ '/'
    ;

delimited_comment_section
    : '/'
    | asterisk* not_slash_or_asterisk
    ;

asterisk
    : '*'
    ;

not_slash_or_asterisk
    : '<Any Unicode character except / or *>'
    ;
```

<span data-ttu-id="978b7-146">批註不會進行嵌套。</span><span class="sxs-lookup"><span data-stu-id="978b7-146">Comments do not nest.</span></span> <span data-ttu-id="978b7-147">`/*`字元序列`/*`和`*/`在`//`批註中沒有特殊意義，而且字元順序`//`和在分隔的批註中沒有特殊意義。</span><span class="sxs-lookup"><span data-stu-id="978b7-147">The character sequences `/*` and `*/` have no special meaning within a `//` comment, and the character sequences `//` and `/*` have no special meaning within a delimited comment.</span></span>

<span data-ttu-id="978b7-148">批註不會在字元和字串常值中處理。</span><span class="sxs-lookup"><span data-stu-id="978b7-148">Comments are not processed within character and string literals.</span></span>

<span data-ttu-id="978b7-149">範例</span><span class="sxs-lookup"><span data-stu-id="978b7-149">The example</span></span>
```csharp
/* Hello, world program
   This program writes "hello, world" to the console
*/
class Hello
{
    static void Main() {
        System.Console.WriteLine("hello, world");
    }
}
```
<span data-ttu-id="978b7-150">包含分隔的批註。</span><span class="sxs-lookup"><span data-stu-id="978b7-150">includes a delimited comment.</span></span>

<span data-ttu-id="978b7-151">範例</span><span class="sxs-lookup"><span data-stu-id="978b7-151">The example</span></span>
```csharp
// Hello, world program
// This program writes "hello, world" to the console
//
class Hello // any name will do for this class
{
    static void Main() { // this method must be named "Main"
        System.Console.WriteLine("hello, world");
    }
}
```
<span data-ttu-id="978b7-152">顯示數個單行批註。</span><span class="sxs-lookup"><span data-stu-id="978b7-152">shows several single-line comments.</span></span>

### <a name="white-space"></a><span data-ttu-id="978b7-153">空白字元</span><span class="sxs-lookup"><span data-stu-id="978b7-153">White space</span></span>

<span data-ttu-id="978b7-154">空白字元會定義為具有 Unicode 類別 Zs 的任何字元（包含空白字元），以及水準定位字元、垂直定位字元和表單摘要字元。</span><span class="sxs-lookup"><span data-stu-id="978b7-154">White space is defined as any character with Unicode class Zs (which includes the space character) as well as the horizontal tab character, the vertical tab character, and the form feed character.</span></span>

```antlr
whitespace
    : '<Any character with Unicode class Zs>'
    | '<Horizontal tab character (U+0009)>'
    | '<Vertical tab character (U+000B)>'
    | '<Form feed character (U+000C)>'
    ;
```

## <a name="tokens"></a><span data-ttu-id="978b7-155">語彙基元</span><span class="sxs-lookup"><span data-stu-id="978b7-155">Tokens</span></span>

<span data-ttu-id="978b7-156">有數種類型的權杖：識別碼、關鍵字、常值、運算子和標點符號。</span><span class="sxs-lookup"><span data-stu-id="978b7-156">There are several kinds of tokens: identifiers, keywords, literals, operators, and punctuators.</span></span> <span data-ttu-id="978b7-157">空白字元和批註不是標記，但它們會當做標記的分隔符號。</span><span class="sxs-lookup"><span data-stu-id="978b7-157">White space and comments are not tokens, though they act as separators for tokens.</span></span>

```antlr
token
    : identifier
    | keyword
    | integer_literal
    | real_literal
    | character_literal
    | string_literal
    | interpolated_string_literal
    | operator_or_punctuator
    ;
```

### <a name="unicode-character-escape-sequences"></a><span data-ttu-id="978b7-158">Unicode 字元逸出序列</span><span class="sxs-lookup"><span data-stu-id="978b7-158">Unicode character escape sequences</span></span>

<span data-ttu-id="978b7-159">Unicode 字元逸出序列代表 Unicode 字元。</span><span class="sxs-lookup"><span data-stu-id="978b7-159">A Unicode character escape sequence represents a Unicode character.</span></span> <span data-ttu-id="978b7-160">Unicode 字元逸出序列會在識別碼（[識別碼](lexical-structure.md#identifiers)）、字元常值（[字元常](lexical-structure.md#character-literals)值）和一般字串常值（[字串常](lexical-structure.md#string-literals)值）中處理。</span><span class="sxs-lookup"><span data-stu-id="978b7-160">Unicode character escape sequences are processed in identifiers ([Identifiers](lexical-structure.md#identifiers)), character literals ([Character literals](lexical-structure.md#character-literals)), and regular string literals ([String literals](lexical-structure.md#string-literals)).</span></span> <span data-ttu-id="978b7-161">Unicode 字元 escape 不會在任何其他位置處理（例如，形成 operator、標點符號或關鍵字）。</span><span class="sxs-lookup"><span data-stu-id="978b7-161">A Unicode character escape is not processed in any other location (for example, to form an operator, punctuator, or keyword).</span></span>

```antlr
unicode_escape_sequence
    : '\\u' hex_digit hex_digit hex_digit hex_digit
    | '\\U' hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit
    ;
```

<span data-ttu-id="978b7-162">Unicode 逸出序列代表以 "`\u`" 或 "`\U`" 字元後面的十六進位數位所形成的單一 unicode 字元。</span><span class="sxs-lookup"><span data-stu-id="978b7-162">A Unicode escape sequence represents the single Unicode character formed by the hexadecimal number following the "`\u`" or "`\U`" characters.</span></span> <span data-ttu-id="978b7-163">由於C#會在字元和字串值中使用 Unicode 程式碼點的16位編碼，因此字元常值中不允許使用 u + 10000 到 U + 10ffff 且範圍內的 unicode 字元，而且會在字串常值中以 Unicode 代理配對來表示。</span><span class="sxs-lookup"><span data-stu-id="978b7-163">Since C# uses a 16-bit encoding of Unicode code points in characters and string values, a Unicode character in the range U+10000 to U+10FFFF is not permitted in a character literal and is represented using a Unicode surrogate pair in a string literal.</span></span> <span data-ttu-id="978b7-164">不支援使用高於0x10FFFF 之程式碼點的 Unicode 字元。</span><span class="sxs-lookup"><span data-stu-id="978b7-164">Unicode characters with code points above 0x10FFFF are not supported.</span></span>

<span data-ttu-id="978b7-165">不會執行多個翻譯。</span><span class="sxs-lookup"><span data-stu-id="978b7-165">Multiple translations are not performed.</span></span> <span data-ttu-id="978b7-166">例如，字串常值 "`\u005Cu005C`" 相當於 "`\u005C`"，而不`\`是 ""。</span><span class="sxs-lookup"><span data-stu-id="978b7-166">For instance, the string literal "`\u005Cu005C`" is equivalent to "`\u005C`" rather than "`\`".</span></span> <span data-ttu-id="978b7-167">Unicode 值`\u005C`是字元 "`\`"。</span><span class="sxs-lookup"><span data-stu-id="978b7-167">The Unicode value `\u005C` is the character "`\`".</span></span>

<span data-ttu-id="978b7-168">範例</span><span class="sxs-lookup"><span data-stu-id="978b7-168">The example</span></span>
```csharp
class Class1
{
    static void Test(bool \u0066) {
        char c = '\u0066';
        if (\u0066)
            System.Console.WriteLine(c.ToString());
    }        
}
```
<span data-ttu-id="978b7-169">顯示數個的`\u0066`用法，這是字母 "`f`" 的轉義順序。</span><span class="sxs-lookup"><span data-stu-id="978b7-169">shows several uses of `\u0066`, which is the escape sequence for the letter "`f`".</span></span> <span data-ttu-id="978b7-170">程式相當於</span><span class="sxs-lookup"><span data-stu-id="978b7-170">The program is equivalent to</span></span>
```csharp
class Class1
{
    static void Test(bool f) {
        char c = 'f';
        if (f)
            System.Console.WriteLine(c.ToString());
    }        
}
```

### <a name="identifiers"></a><span data-ttu-id="978b7-171">識別項</span><span class="sxs-lookup"><span data-stu-id="978b7-171">Identifiers</span></span>

<span data-ttu-id="978b7-172">本節中所提供的識別碼規則與 Unicode 標準附錄31所建議的規則完全對應，不同之處在于允許使用底線做為初始字元（如同 C 程式設計語言的傳統），Unicode 逸出序列是允許在識別碼中使用，並`@`允許 "" 字元作為前置詞，以啟用關鍵字做為識別碼。</span><span class="sxs-lookup"><span data-stu-id="978b7-172">The rules for identifiers given in this section correspond exactly to those recommended by the Unicode Standard Annex 31, except that underscore is allowed as an initial character (as is traditional in the C programming language), Unicode escape sequences are permitted in identifiers, and the "`@`" character is allowed as a prefix to enable keywords to be used as identifiers.</span></span>

```antlr
identifier
    : available_identifier
    | '@' identifier_or_keyword
    ;

available_identifier
    : '<An identifier_or_keyword that is not a keyword>'
    ;

identifier_or_keyword
    : identifier_start_character identifier_part_character*
    ;

identifier_start_character
    : letter_character
    | '_'
    ;

identifier_part_character
    : letter_character
    | decimal_digit_character
    | connecting_character
    | combining_character
    | formatting_character
    ;

letter_character
    : '<A Unicode character of classes Lu, Ll, Lt, Lm, Lo, or Nl>'
    | '<A unicode_escape_sequence representing a character of classes Lu, Ll, Lt, Lm, Lo, or Nl>'
    ;

combining_character
    : '<A Unicode character of classes Mn or Mc>'
    | '<A unicode_escape_sequence representing a character of classes Mn or Mc>'
    ;

decimal_digit_character
    : '<A Unicode character of the class Nd>'
    | '<A unicode_escape_sequence representing a character of the class Nd>'
    ;

connecting_character
    : '<A Unicode character of the class Pc>'
    | '<A unicode_escape_sequence representing a character of the class Pc>'
    ;

formatting_character
    : '<A Unicode character of the class Cf>'
    | '<A unicode_escape_sequence representing a character of the class Cf>'
    ;
```

<span data-ttu-id="978b7-173">如需前述 Unicode 字元類別的詳細資訊，請參閱 Unicode 標準版本3.0，第4.5 節。</span><span class="sxs-lookup"><span data-stu-id="978b7-173">For information on the Unicode character classes mentioned above, see The Unicode Standard, Version 3.0, section 4.5.</span></span>

<span data-ttu-id="978b7-174">有效識別碼的範例包括 "`identifier1`"、"`_identifier2`" 和 "`@if`"。</span><span class="sxs-lookup"><span data-stu-id="978b7-174">Examples of valid identifiers include "`identifier1`", "`_identifier2`", and "`@if`".</span></span>

<span data-ttu-id="978b7-175">符合規範之程式中的識別碼必須是 Unicode 正規化表單 C 所定義的標準格式，如 Unicode Standard 附錄15所定義。</span><span class="sxs-lookup"><span data-stu-id="978b7-175">An identifier in a conforming program must be in the canonical format defined by Unicode Normalization Form C, as defined by Unicode Standard Annex 15.</span></span> <span data-ttu-id="978b7-176">當發現不是正規化格式 C 的識別碼時，其行為是由執行定義;不過，不需要進行診斷。</span><span class="sxs-lookup"><span data-stu-id="978b7-176">The behavior when encountering an identifier not in Normalization Form C is implementation-defined; however, a diagnostic is not required.</span></span>

<span data-ttu-id="978b7-177">前置詞 "`@`" 可讓您使用關鍵字做為識別碼，這在與其他程式設計語言互動時非常有用。</span><span class="sxs-lookup"><span data-stu-id="978b7-177">The prefix "`@`" enables the use of keywords as identifiers, which is useful when interfacing with other programming languages.</span></span> <span data-ttu-id="978b7-178">字元`@`實際上並不是識別碼的一部分，因此可能會在其他語言中看到此識別碼做為一般識別碼，但不含前置詞。</span><span class="sxs-lookup"><span data-stu-id="978b7-178">The character `@` is not actually part of the identifier, so the identifier might be seen in other languages as a normal identifier, without the prefix.</span></span> <span data-ttu-id="978b7-179">具有`@`前置詞的識別碼稱為***逐字識別碼***。</span><span class="sxs-lookup"><span data-stu-id="978b7-179">An identifier with an `@` prefix is called a ***verbatim identifier***.</span></span> <span data-ttu-id="978b7-180">允許不是`@`關鍵字之識別碼的前置詞，但強烈建議您不要使用樣式。</span><span class="sxs-lookup"><span data-stu-id="978b7-180">Use of the `@` prefix for identifiers that are not keywords is permitted, but strongly discouraged as a matter of style.</span></span>

<span data-ttu-id="978b7-181">範例：</span><span class="sxs-lookup"><span data-stu-id="978b7-181">The example:</span></span>
```csharp
class @class
{
    public static void @static(bool @bool) {
        if (@bool)
            System.Console.WriteLine("true");
        else
            System.Console.WriteLine("false");
    }    
}

class Class1
{
    static void M() {
        cl\u0061ss.st\u0061tic(true);
    }
}
```
<span data-ttu-id="978b7-182">定義名為 "`class`" 的類別，其具有`static`名為 "" 的靜態方法，其`bool`採用名為 "" 的參數。</span><span class="sxs-lookup"><span data-stu-id="978b7-182">defines a class named "`class`" with a static method named "`static`" that takes a parameter named "`bool`".</span></span> <span data-ttu-id="978b7-183">請注意，因為關鍵字中不允許使用 Unicode 轉義，所以 token`cl\u0061ss`"" 是識別碼，而與 "`@class`" 的識別碼相同。</span><span class="sxs-lookup"><span data-stu-id="978b7-183">Note that since Unicode escapes are not permitted in keywords, the token "`cl\u0061ss`" is an identifier, and is the same identifier as "`@class`".</span></span>

<span data-ttu-id="978b7-184">如果在套用下列轉換之後，將兩個識別碼視為相同，則順序如下：</span><span class="sxs-lookup"><span data-stu-id="978b7-184">Two identifiers are considered the same if they are identical after the following transformations are applied, in order:</span></span>

*  <span data-ttu-id="978b7-185">已移除前置`@`詞 "" （如果使用的話）。</span><span class="sxs-lookup"><span data-stu-id="978b7-185">The prefix "`@`", if used, is removed.</span></span>
*  <span data-ttu-id="978b7-186">每個*unicode_escape_sequence*都會轉換成其對應的 unicode 字元。</span><span class="sxs-lookup"><span data-stu-id="978b7-186">Each *unicode_escape_sequence* is transformed into its corresponding Unicode character.</span></span>
*  <span data-ttu-id="978b7-187">所有*formatting_character*都已移除。</span><span class="sxs-lookup"><span data-stu-id="978b7-187">Any *formatting_character*s are removed.</span></span>

<span data-ttu-id="978b7-188">包含兩個連續底線字元（`U+005F`）的識別碼會保留供實作為使用。</span><span class="sxs-lookup"><span data-stu-id="978b7-188">Identifiers containing two consecutive underscore characters (`U+005F`) are reserved for use by the implementation.</span></span> <span data-ttu-id="978b7-189">例如，執行可能會提供以兩個底線開頭的擴充關鍵字。</span><span class="sxs-lookup"><span data-stu-id="978b7-189">For example, an implementation might provide extended keywords that begin with two underscores.</span></span>

### <a name="keywords"></a><span data-ttu-id="978b7-190">關鍵字</span><span class="sxs-lookup"><span data-stu-id="978b7-190">Keywords</span></span>

<span data-ttu-id="978b7-191">***關鍵字***是保留的類似識別碼的字元序列，不能用來做為識別碼，除非前面加`@`上字元。</span><span class="sxs-lookup"><span data-stu-id="978b7-191">A ***keyword*** is an identifier-like sequence of characters that is reserved, and cannot be used as an identifier except when prefaced by the `@` character.</span></span>

```antlr
keyword
    : 'abstract' | 'as'       | 'base'       | 'bool'      | 'break'
    | 'byte'     | 'case'     | 'catch'      | 'char'      | 'checked'
    | 'class'    | 'const'    | 'continue'   | 'decimal'   | 'default'
    | 'delegate' | 'do'       | 'double'     | 'else'      | 'enum'
    | 'event'    | 'explicit' | 'extern'     | 'false'     | 'finally'
    | 'fixed'    | 'float'    | 'for'        | 'foreach'   | 'goto'
    | 'if'       | 'implicit' | 'in'         | 'int'       | 'interface'
    | 'internal' | 'is'       | 'lock'       | 'long'      | 'namespace'
    | 'new'      | 'null'     | 'object'     | 'operator'  | 'out'
    | 'override' | 'params'   | 'private'    | 'protected' | 'public'
    | 'readonly' | 'ref'      | 'return'     | 'sbyte'     | 'sealed'
    | 'short'    | 'sizeof'   | 'stackalloc' | 'static'    | 'string'
    | 'struct'   | 'switch'   | 'this'       | 'throw'     | 'true'
    | 'try'      | 'typeof'   | 'uint'       | 'ulong'     | 'unchecked'
    | 'unsafe'   | 'ushort'   | 'using'      | 'virtual'   | 'void'
    | 'volatile' | 'while'
    ;
```

<span data-ttu-id="978b7-192">在文法的某些位置中，特定識別碼具有特殊意義，但不是關鍵字。</span><span class="sxs-lookup"><span data-stu-id="978b7-192">In some places in the grammar, specific identifiers have special meaning, but are not keywords.</span></span> <span data-ttu-id="978b7-193">這類識別碼有時稱為「內容關鍵字」。</span><span class="sxs-lookup"><span data-stu-id="978b7-193">Such identifiers are sometimes referred to as "contextual keywords".</span></span> <span data-ttu-id="978b7-194">例如，在屬性宣告中，"`get`" 和 "`set`" 識別碼具有特殊意義（[存取](classes.md#accessors)子）。</span><span class="sxs-lookup"><span data-stu-id="978b7-194">For example, within a property declaration, the "`get`" and "`set`" identifiers have special meaning ([Accessors](classes.md#accessors)).</span></span> <span data-ttu-id="978b7-195">這些位置中絕不`get`允許`set`或以外的識別碼，因此此用法不會與使用這些單字做為識別碼衝突。</span><span class="sxs-lookup"><span data-stu-id="978b7-195">An identifier other than `get` or `set` is never permitted in these locations, so this use does not conflict with a use of these words as identifiers.</span></span> <span data-ttu-id="978b7-196">在其他情況下，例如在隱含類型區域變數`var`宣告（[區域變數](statements.md#local-variable-declarations)宣告）中使用識別碼 ""，內容關鍵字可能會與宣告的名稱衝突。</span><span class="sxs-lookup"><span data-stu-id="978b7-196">In other cases, such as with the identifier "`var`" in implicitly typed local variable declarations ([Local variable declarations](statements.md#local-variable-declarations)), a contextual keyword can conflict with declared names.</span></span> <span data-ttu-id="978b7-197">在這種情況下，宣告的名稱優先于使用識別碼做為內容關鍵字。</span><span class="sxs-lookup"><span data-stu-id="978b7-197">In such cases, the declared name takes precedence over the use of the identifier as a contextual keyword.</span></span>

### <a name="literals"></a><span data-ttu-id="978b7-198">常值</span><span class="sxs-lookup"><span data-stu-id="978b7-198">Literals</span></span>

<span data-ttu-id="978b7-199">***常***值是值的原始程式碼表示。</span><span class="sxs-lookup"><span data-stu-id="978b7-199">A ***literal*** is a source code representation of a value.</span></span>

```antlr
literal
    : boolean_literal
    | integer_literal
    | real_literal
    | character_literal
    | string_literal
    | null_literal
    ;
```

#### <a name="boolean-literals"></a><span data-ttu-id="978b7-200">布林值常值</span><span class="sxs-lookup"><span data-stu-id="978b7-200">Boolean literals</span></span>

<span data-ttu-id="978b7-201">有兩個布林常值： `true`和`false`。</span><span class="sxs-lookup"><span data-stu-id="978b7-201">There are two boolean literal values: `true` and `false`.</span></span>

```antlr
boolean_literal
    : 'true'
    | 'false'
    ;
```

<span data-ttu-id="978b7-202">*Boolean_literal*的類型為 `bool`。</span><span class="sxs-lookup"><span data-stu-id="978b7-202">The type of a *boolean_literal* is `bool`.</span></span>

#### <a name="integer-literals"></a><span data-ttu-id="978b7-203">整數常值</span><span class="sxs-lookup"><span data-stu-id="978b7-203">Integer literals</span></span>

<span data-ttu-id="978b7-204">整數常值是用`int`來寫入`long`、 `uint`、和`ulong`類型的值。</span><span class="sxs-lookup"><span data-stu-id="978b7-204">Integer literals are used to write values of types `int`, `uint`, `long`, and `ulong`.</span></span> <span data-ttu-id="978b7-205">整數常值有兩種可能的形式： decimal 和十六進位。</span><span class="sxs-lookup"><span data-stu-id="978b7-205">Integer literals have two possible forms: decimal and hexadecimal.</span></span>

```antlr
integer_literal
    : decimal_integer_literal
    | hexadecimal_integer_literal
    ;

decimal_integer_literal
    : decimal_digit+ integer_type_suffix?
    ;

decimal_digit
    : '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'
    ;

integer_type_suffix
    : 'U' | 'u' | 'L' | 'l' | 'UL' | 'Ul' | 'uL' | 'ul' | 'LU' | 'Lu' | 'lU' | 'lu'
    ;

hexadecimal_integer_literal
    : '0x' hex_digit+ integer_type_suffix?
    | '0X' hex_digit+ integer_type_suffix?
    ;

hex_digit
    : '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'
    | 'A' | 'B' | 'C' | 'D' | 'E' | 'F' | 'a' | 'b' | 'c' | 'd' | 'e' | 'f';
```

<span data-ttu-id="978b7-206">整數常值的類型是依照下列方式決定：</span><span class="sxs-lookup"><span data-stu-id="978b7-206">The type of an integer literal is determined as follows:</span></span>

*  <span data-ttu-id="978b7-207">如果常值沒有後置詞，則會有下列類型的第一個，其中可以表示其值`int`： `uint`、 `long`、 `ulong`、。</span><span class="sxs-lookup"><span data-stu-id="978b7-207">If the literal has no suffix, it has the first of these types in which its value can be represented: `int`, `uint`, `long`, `ulong`.</span></span>
*  <span data-ttu-id="978b7-208">如果常值是`U`以或`u`做為後置字元，它會有下列類型的第一個，其中可以表示`ulong`其值： `uint`、。</span><span class="sxs-lookup"><span data-stu-id="978b7-208">If the literal is suffixed by `U` or `u`, it has the first of these types in which its value can be represented: `uint`, `ulong`.</span></span>
*  <span data-ttu-id="978b7-209">如果常值是`L`以或`l`做為後置字元，它會有下列類型的第一個，其中可以表示`ulong`其值： `long`、。</span><span class="sxs-lookup"><span data-stu-id="978b7-209">If the literal is suffixed by `L` or `l`, it has the first of these types in which its value can be represented: `long`, `ulong`.</span></span>
*  <span data-ttu-id="978b7-210">如果常值的後置`UL`字元`Ul`是`uL`、 `ul`、 `LU`、 `Lu`、 `lU`、、 `lu`或，則其類型`ulong`為。</span><span class="sxs-lookup"><span data-stu-id="978b7-210">If the literal is suffixed by `UL`, `Ul`, `uL`, `ul`, `LU`, `Lu`, `lU`, or `lu`, it is of type `ulong`.</span></span>

<span data-ttu-id="978b7-211">如果整數常值所代表的值超出`ulong`類型的範圍，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="978b7-211">If the value represented by an integer literal is outside the range of the `ulong` type, a compile-time error occurs.</span></span>

<span data-ttu-id="978b7-212">就樣式而言，撰寫類型`L` `long`的常值時，建議使用 ""，而`l`不是 ""，因為很容易就能將字母 "`l`" 與數位 "`1`" 混淆。</span><span class="sxs-lookup"><span data-stu-id="978b7-212">As a matter of style, it is suggested that "`L`" be used instead of "`l`" when writing literals of type `long`, since it is easy to confuse the letter "`l`" with the digit "`1`".</span></span>

<span data-ttu-id="978b7-213">若要允許最小`int`的`long`可能和值寫入為十進位整數常值，則會有下列兩個規則：</span><span class="sxs-lookup"><span data-stu-id="978b7-213">To permit the smallest possible `int` and `long` values to be written as decimal integer literals, the following two rules exist:</span></span>

* <span data-ttu-id="978b7-214">當*decimal_integer_literal*的值為2147483648（2 ^ 31）且沒有*integer_type_suffix*顯示為緊接在一元減號運算子標記（[一元減號運算子](expressions.md#unary-minus-operator)）後面的 token 時，結果會是類型 `int` 的常數，其值為-2147483648 （-2 ^ 31）。</span><span class="sxs-lookup"><span data-stu-id="978b7-214">When a *decimal_integer_literal* with the value 2147483648 (2^31) and no *integer_type_suffix* appears as the token immediately following a unary minus operator token ([Unary minus operator](expressions.md#unary-minus-operator)), the result is a constant of type `int` with the value -2147483648 (-2^31).</span></span> <span data-ttu-id="978b7-215">在所有其他情況下，這類*decimal_integer_literal*的類型 `uint`。</span><span class="sxs-lookup"><span data-stu-id="978b7-215">In all other situations, such a *decimal_integer_literal* is of type `uint`.</span></span>
* <span data-ttu-id="978b7-216">當值為9223372036854775808（2 ^ 63）且沒有*integer_type_suffix*或*integer_type_suffix* `L` 或 `l` 的*decimal_integer_literal*出現為緊接在一元減號運算子 token 後面的 token （[一元減號運算子](expressions.md#unary-minus-operator)），結果會是類型 `long` 的常數，其值為-9223372036854775808 （-2 ^ 63）。</span><span class="sxs-lookup"><span data-stu-id="978b7-216">When a *decimal_integer_literal* with the value 9223372036854775808 (2^63) and no *integer_type_suffix* or the *integer_type_suffix* `L` or `l` appears as the token immediately following a unary minus operator token ([Unary minus operator](expressions.md#unary-minus-operator)), the result is a constant of type `long` with the value -9223372036854775808 (-2^63).</span></span> <span data-ttu-id="978b7-217">在所有其他情況下，這類*decimal_integer_literal*的類型 `ulong`。</span><span class="sxs-lookup"><span data-stu-id="978b7-217">In all other situations, such a *decimal_integer_literal* is of type `ulong`.</span></span>

#### <a name="real-literals"></a><span data-ttu-id="978b7-218">實際常值</span><span class="sxs-lookup"><span data-stu-id="978b7-218">Real literals</span></span>

<span data-ttu-id="978b7-219">Real 常值是用來寫入、 `float` `double`和`decimal`類型的值。</span><span class="sxs-lookup"><span data-stu-id="978b7-219">Real literals are used to write values of types `float`, `double`, and `decimal`.</span></span>

```antlr
real_literal
    : decimal_digit+ '.' decimal_digit+ exponent_part? real_type_suffix?
    | '.' decimal_digit+ exponent_part? real_type_suffix?
    | decimal_digit+ exponent_part real_type_suffix?
    | decimal_digit+ real_type_suffix
    ;

exponent_part
    : 'e' sign? decimal_digit+
    | 'E' sign? decimal_digit+
    ;

sign
    : '+'
    | '-'
    ;

real_type_suffix
    : 'F' | 'f' | 'D' | 'd' | 'M' | 'm'
    ;
```

<span data-ttu-id="978b7-220">如果未指定*real_type_suffix* ，則實際常值的類型為 `double`。</span><span class="sxs-lookup"><span data-stu-id="978b7-220">If no *real_type_suffix* is specified, the type of the real literal is `double`.</span></span> <span data-ttu-id="978b7-221">否則，real 類型尾碼會決定實際常值的類型，如下所示：</span><span class="sxs-lookup"><span data-stu-id="978b7-221">Otherwise, the real type suffix determines the type of the real literal, as follows:</span></span>

*  <span data-ttu-id="978b7-222">`F` `float`或的實數常值為類型`f` 。</span><span class="sxs-lookup"><span data-stu-id="978b7-222">A real literal suffixed by `F` or `f` is of type `float`.</span></span> <span data-ttu-id="978b7-223">例如`1f`，常`float`值`1.5f`、、和`123.456F`都是型別。 `1e10f`</span><span class="sxs-lookup"><span data-stu-id="978b7-223">For example, the literals `1f`, `1.5f`, `1e10f`, and `123.456F` are all of type `float`.</span></span>
*  <span data-ttu-id="978b7-224">`D` `double`或的實數常值為類型`d` 。</span><span class="sxs-lookup"><span data-stu-id="978b7-224">A real literal suffixed by `D` or `d` is of type `double`.</span></span> <span data-ttu-id="978b7-225">例如`1d`，常`double`值`1.5d`、、和`123.456D`都是型別。 `1e10d`</span><span class="sxs-lookup"><span data-stu-id="978b7-225">For example, the literals `1d`, `1.5d`, `1e10d`, and `123.456D` are all of type `double`.</span></span>
*  <span data-ttu-id="978b7-226">`M` `decimal`或的實數常值為類型`m` 。</span><span class="sxs-lookup"><span data-stu-id="978b7-226">A real literal suffixed by `M` or `m` is of type `decimal`.</span></span> <span data-ttu-id="978b7-227">例如`1m`，常`decimal`值`1.5m`、、和`123.456M`都是型別。 `1e10m`</span><span class="sxs-lookup"><span data-stu-id="978b7-227">For example, the literals `1m`, `1.5m`, `1e10m`, and `123.456M` are all of type `decimal`.</span></span> <span data-ttu-id="978b7-228">這個常值會藉由`decimal`取得精確的值來轉換成值，並在必要時，使用四進位的舍入（[decimal 類型](types.md#the-decimal-type)）來四捨五入到最接近的可顯示值。</span><span class="sxs-lookup"><span data-stu-id="978b7-228">This literal is converted to a `decimal` value by taking the exact value, and, if necessary, rounding to the nearest representable value using banker's rounding ([The decimal type](types.md#the-decimal-type)).</span></span> <span data-ttu-id="978b7-229">除非將值舍入或值為零（在後者的情況下，正負號和小數值會是0），否則會保留常值中明顯的任何尺規。</span><span class="sxs-lookup"><span data-stu-id="978b7-229">Any scale apparent in the literal is preserved unless the value is rounded or the value is zero (in which latter case the sign and scale will be 0).</span></span> <span data-ttu-id="978b7-230">因此，會剖析`2.900m`常值以形成具有正負號`0`、係數`2900`和小`3`數位數的 decimal。</span><span class="sxs-lookup"><span data-stu-id="978b7-230">Hence, the literal `2.900m` will be parsed to form the decimal with sign `0`, coefficient `2900`, and scale `3`.</span></span>

<span data-ttu-id="978b7-231">如果指定的常值無法以指示的類型表示，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="978b7-231">If the specified literal cannot be represented in the indicated type, a compile-time error occurs.</span></span>

<span data-ttu-id="978b7-232">類型`float` 或`double`的實數常值，是使用 IEEE 「舍入至最接近」模式來決定。</span><span class="sxs-lookup"><span data-stu-id="978b7-232">The value of a real literal of type `float` or `double` is determined by using the IEEE "round to nearest" mode.</span></span>

<span data-ttu-id="978b7-233">請注意，在實際常值中，小數點後面一律需要十進位數。</span><span class="sxs-lookup"><span data-stu-id="978b7-233">Note that in a real literal, decimal digits are always required after the decimal point.</span></span> <span data-ttu-id="978b7-234">例如，是`1.3F`實數常值，但`1.F`不是。</span><span class="sxs-lookup"><span data-stu-id="978b7-234">For example, `1.3F` is a real literal but `1.F` is not.</span></span>

#### <a name="character-literals"></a><span data-ttu-id="978b7-235">字元常值</span><span class="sxs-lookup"><span data-stu-id="978b7-235">Character literals</span></span>

<span data-ttu-id="978b7-236">字元常值代表單一字元，通常是由引號中的字元所組成，如中`'a'`所示。</span><span class="sxs-lookup"><span data-stu-id="978b7-236">A character literal represents a single character, and usually consists of a character in quotes, as in `'a'`.</span></span>

<span data-ttu-id="978b7-237">注意：ANTLR 文法標記法會造成下列混淆！</span><span class="sxs-lookup"><span data-stu-id="978b7-237">Note: The ANTLR grammar notation makes the following confusing!</span></span> <span data-ttu-id="978b7-238">在 ANTLR 中，當您`\'`撰寫時，會代表單一`'`引號。</span><span class="sxs-lookup"><span data-stu-id="978b7-238">In ANTLR, when you write `\'` it stands for a single quote `'`.</span></span> <span data-ttu-id="978b7-239">當您撰寫`\\`時，它代表單一反斜線`\`。</span><span class="sxs-lookup"><span data-stu-id="978b7-239">And when you write `\\` it stands for a single backslash `\`.</span></span> <span data-ttu-id="978b7-240">因此，字元常值的第一個規則就是以單引號開頭，然後輸入一個字元，再加上一個單引號。</span><span class="sxs-lookup"><span data-stu-id="978b7-240">Therefore the first rule for a character literal means it starts with a single quote, then a character, then a single quote.</span></span> <span data-ttu-id="978b7-241">而11個可能的簡單逸出序列`\'`包括`\"`、 `\\`、 `\0`、 `\a`、 `\b`、 `\f`、 `\n` 、、`\t`、、 `\r` `\v`.</span><span class="sxs-lookup"><span data-stu-id="978b7-241">And the eleven possible simple escape sequences are `\'`, `\"`, `\\`, `\0`, `\a`, `\b`, `\f`, `\n`, `\r`, `\t`, `\v`.</span></span>

```antlr
character_literal
    : '\'' character '\''
    ;

character
    : single_character
    | simple_escape_sequence
    | hexadecimal_escape_sequence
    | unicode_escape_sequence
    ;

single_character
    : '<Any character except \' (U+0027), \\ (U+005C), and new_line_character>'
    ;

simple_escape_sequence
    : '\\\'' | '\\"' | '\\\\' | '\\0' | '\\a' | '\\b' | '\\f' | '\\n' | '\\r' | '\\t' | '\\v'
    ;

hexadecimal_escape_sequence
    : '\\x' hex_digit hex_digit? hex_digit? hex_digit?;
```

<span data-ttu-id="978b7-242">在字元中後面接著反斜線字元`\`（）的字元必須是下列其中一個字元： `'`、 `a` `"`、 `\`、 `0`、、、 `f` `b`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span><span class="sxs-lookup"><span data-stu-id="978b7-242">A character that follows a backslash character (`\`) in a *character* must be one of the following characters: `'`, `"`, `\`, `0`, `a`, `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span></span> <span data-ttu-id="978b7-243">否則，會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="978b7-243">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="978b7-244">十六進位的逸出序列代表單一 Unicode 字元，其值是由 "`\x`" 後面的十六進位數位所組成。</span><span class="sxs-lookup"><span data-stu-id="978b7-244">A hexadecimal escape sequence represents a single Unicode character, with the value formed by the hexadecimal number following "`\x`".</span></span>

<span data-ttu-id="978b7-245">如果字元常值所代表的值大於`U+FFFF`，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="978b7-245">If the value represented by a character literal is greater than `U+FFFF`, a compile-time error occurs.</span></span>

<span data-ttu-id="978b7-246">字元常值中的 unicode 字元逸出序列（[unicode 字元逸出序列](lexical-structure.md#unicode-character-escape-sequences)）必須在到`U+0000` `U+FFFF`的範圍內。</span><span class="sxs-lookup"><span data-stu-id="978b7-246">A Unicode character escape sequence ([Unicode character escape sequences](lexical-structure.md#unicode-character-escape-sequences)) in a character literal must be in the range `U+0000` to `U+FFFF`.</span></span>

<span data-ttu-id="978b7-247">簡單的逸出序列代表 Unicode 字元編碼，如下表所述。</span><span class="sxs-lookup"><span data-stu-id="978b7-247">A simple escape sequence represents a Unicode character encoding, as described in the table below.</span></span>


| <span data-ttu-id="978b7-248">__Escape 序列__</span><span class="sxs-lookup"><span data-stu-id="978b7-248">__Escape sequence__</span></span> | <span data-ttu-id="978b7-249">__字元名稱__</span><span class="sxs-lookup"><span data-stu-id="978b7-249">__Character name__</span></span> | <span data-ttu-id="978b7-250">__Unicode 編碼__</span><span class="sxs-lookup"><span data-stu-id="978b7-250">__Unicode encoding__</span></span> |
|---------------------|--------------------|----------------------|
| `\'`                | <span data-ttu-id="978b7-251">單引號</span><span class="sxs-lookup"><span data-stu-id="978b7-251">Single quote</span></span>       | `0x0027`             | 
| `\"`                | <span data-ttu-id="978b7-252">雙引號</span><span class="sxs-lookup"><span data-stu-id="978b7-252">Double quote</span></span>       | `0x0022`             | 
| `\\`                | <span data-ttu-id="978b7-253">反斜線</span><span class="sxs-lookup"><span data-stu-id="978b7-253">Backslash</span></span>          | `0x005C`             | 
| `\0`                | <span data-ttu-id="978b7-254">Null</span><span class="sxs-lookup"><span data-stu-id="978b7-254">Null</span></span>               | `0x0000`             | 
| `\a`                | <span data-ttu-id="978b7-255">警示</span><span class="sxs-lookup"><span data-stu-id="978b7-255">Alert</span></span>              | `0x0007`             | 
| `\b`                | <span data-ttu-id="978b7-256">退格鍵</span><span class="sxs-lookup"><span data-stu-id="978b7-256">Backspace</span></span>          | `0x0008`             | 
| `\f`                | <span data-ttu-id="978b7-257">換頁字元</span><span class="sxs-lookup"><span data-stu-id="978b7-257">Form feed</span></span>          | `0x000C`             | 
| `\n`                | <span data-ttu-id="978b7-258">換行</span><span class="sxs-lookup"><span data-stu-id="978b7-258">New line</span></span>           | `0x000A`             | 
| `\r`                | <span data-ttu-id="978b7-259">歸位字元</span><span class="sxs-lookup"><span data-stu-id="978b7-259">Carriage return</span></span>    | `0x000D`             | 
| `\t`                | <span data-ttu-id="978b7-260">水平 Tab</span><span class="sxs-lookup"><span data-stu-id="978b7-260">Horizontal tab</span></span>     | `0x0009`             | 
| `\v`                | <span data-ttu-id="978b7-261">垂直 Tab</span><span class="sxs-lookup"><span data-stu-id="978b7-261">Vertical tab</span></span>       | `0x000B`             | 

<span data-ttu-id="978b7-262">*Character_literal*的類型為 `char`。</span><span class="sxs-lookup"><span data-stu-id="978b7-262">The type of a *character_literal* is `char`.</span></span>

#### <a name="string-literals"></a><span data-ttu-id="978b7-263">字串常值</span><span class="sxs-lookup"><span data-stu-id="978b7-263">String literals</span></span>

<span data-ttu-id="978b7-264">C#支援兩種格式的字串常值：***一般字串常***值和***逐字字串常***值。</span><span class="sxs-lookup"><span data-stu-id="978b7-264">C# supports two forms of string literals: ***regular string literals*** and ***verbatim string literals***.</span></span>

<span data-ttu-id="978b7-265">一般字串常值是由以雙引號括住的零或多個字元所`"hello"`組成，如所示，而且可能包含簡單`\t`的逸出序列（例如，用於定位字元）和十六進位和 Unicode 逸出序列。</span><span class="sxs-lookup"><span data-stu-id="978b7-265">A regular string literal consists of zero or more characters enclosed in double quotes, as in `"hello"`, and may include both simple escape sequences (such as `\t` for the tab character), and hexadecimal and Unicode escape sequences.</span></span>

<span data-ttu-id="978b7-266">逐字字串常值包含一個`@`字元，後面接著雙引號字元、零或多個字元，以及右雙引號字元。</span><span class="sxs-lookup"><span data-stu-id="978b7-266">A verbatim string literal consists of an `@` character followed by a double-quote character, zero or more characters, and a closing double-quote character.</span></span> <span data-ttu-id="978b7-267">一個簡單的範例`@"hello"`是。</span><span class="sxs-lookup"><span data-stu-id="978b7-267">A simple example is `@"hello"`.</span></span> <span data-ttu-id="978b7-268">在逐字字串常值中，分隔符號之間的字元會逐字轉譯，唯一的例外狀況是*quote_escape_sequence*。</span><span class="sxs-lookup"><span data-stu-id="978b7-268">In a verbatim string literal, the characters between the delimiters are interpreted verbatim, the only exception being a *quote_escape_sequence*.</span></span> <span data-ttu-id="978b7-269">特別的是，簡單的逸出序列和十六進位和 Unicode 逸出序列不會在逐字字串常值中處理。</span><span class="sxs-lookup"><span data-stu-id="978b7-269">In particular, simple escape sequences, and hexadecimal and Unicode escape sequences are not processed in verbatim string literals.</span></span> <span data-ttu-id="978b7-270">逐字字串常值可能會跨越多行。</span><span class="sxs-lookup"><span data-stu-id="978b7-270">A verbatim string literal may span multiple lines.</span></span>

```antlr
string_literal
    : regular_string_literal
    | verbatim_string_literal
    ;

regular_string_literal
    : '"' regular_string_literal_character* '"'
    ;

regular_string_literal_character
    : single_regular_string_literal_character
    | simple_escape_sequence
    | hexadecimal_escape_sequence
    | unicode_escape_sequence
    ;

single_regular_string_literal_character
    : '<Any character except " (U+0022), \\ (U+005C), and new_line_character>'
    ;

verbatim_string_literal
    : '@"' verbatim_string_literal_character* '"'
    ;

verbatim_string_literal_character
    : single_verbatim_string_literal_character
    | quote_escape_sequence
    ;

single_verbatim_string_literal_character
    : '<any character except ">'
    ;

quote_escape_sequence
    : '""'
    ;
```

<span data-ttu-id="978b7-271">*Regular_string_literal_character*中的反斜線字元（`\`）後面的字元必須是下列其中一個字元： `'`、`"`、`\`、`0`、`a`、`b`、`f`、`n`、0、1、2、3、4、5。</span><span class="sxs-lookup"><span data-stu-id="978b7-271">A character that follows a backslash character (`\`) in a *regular_string_literal_character* must be one of the following characters: `'`, `"`, `\`, `0`, `a`, `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span></span> <span data-ttu-id="978b7-272">否則，會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="978b7-272">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="978b7-273">範例</span><span class="sxs-lookup"><span data-stu-id="978b7-273">The example</span></span>
```csharp
string a = "hello, world";                   // hello, world
string b = @"hello, world";                  // hello, world

string c = "hello \t world";                 // hello      world
string d = @"hello \t world";                // hello \t world

string e = "Joe said \"Hello\" to me";       // Joe said "Hello" to me
string f = @"Joe said ""Hello"" to me";      // Joe said "Hello" to me

string g = "\\\\server\\share\\file.txt";    // \\server\share\file.txt
string h = @"\\server\share\file.txt";       // \\server\share\file.txt

string i = "one\r\ntwo\r\nthree";
string j = @"one
two
three";
```
<span data-ttu-id="978b7-274">顯示各種不同的字串常值。</span><span class="sxs-lookup"><span data-stu-id="978b7-274">shows a variety of string literals.</span></span> <span data-ttu-id="978b7-275">最後一個字串常`j`值是跨越多行的逐字字串常值。</span><span class="sxs-lookup"><span data-stu-id="978b7-275">The last string literal, `j`, is a verbatim string literal that spans multiple lines.</span></span> <span data-ttu-id="978b7-276">引號之間的字元（包括分行符號之類的空白字元）會逐字保留。</span><span class="sxs-lookup"><span data-stu-id="978b7-276">The characters between the quotation marks, including white space such as new line characters, are preserved verbatim.</span></span>

<span data-ttu-id="978b7-277">由於十六進位的逸出序列可以有可變數目的十六進位數位，字串常`"\x123"`值會包含具有十六進位值123的單一字元。</span><span class="sxs-lookup"><span data-stu-id="978b7-277">Since a hexadecimal escape sequence can have a variable number of hex digits, the string literal `"\x123"` contains a single character with hex value 123.</span></span> <span data-ttu-id="978b7-278">若要建立字串，其中包含十六進位值12後面接著字元3的字元，則可以改`"\x00123"`為`"\x12" + "3"`寫入或。</span><span class="sxs-lookup"><span data-stu-id="978b7-278">To create a string containing the character with hex value 12 followed by the character 3, one could write `"\x00123"` or `"\x12" + "3"` instead.</span></span>

<span data-ttu-id="978b7-279">*String_literal*的類型為 `string`。</span><span class="sxs-lookup"><span data-stu-id="978b7-279">The type of a *string_literal* is `string`.</span></span>

<span data-ttu-id="978b7-280">每個字串常值不一定會產生新的字串實例。</span><span class="sxs-lookup"><span data-stu-id="978b7-280">Each string literal does not necessarily result in a new string instance.</span></span> <span data-ttu-id="978b7-281">當兩個或多個根據字串等號比較運算子（[字串等號比較](expressions.md#string-equality-operators)運算子）相等的字串常值出現在相同的程式中時，這些字串常值會參考相同的字串實例。</span><span class="sxs-lookup"><span data-stu-id="978b7-281">When two or more string literals that are equivalent according to the string equality operator ([String equality operators](expressions.md#string-equality-operators)) appear in the same program, these string literals refer to the same string instance.</span></span> <span data-ttu-id="978b7-282">例如，產生的輸出是由</span><span class="sxs-lookup"><span data-stu-id="978b7-282">For instance, the output produced by</span></span>
```csharp
class Test
{
    static void Main() {
        object a = "hello";
        object b = "hello";
        System.Console.WriteLine(a == b);
    }
}
```
<span data-ttu-id="978b7-283">是`True` ，因為這兩個常值會參考相同的字串實例。</span><span class="sxs-lookup"><span data-stu-id="978b7-283">is `True` because the two literals refer to the same string instance.</span></span>

#### <a name="interpolated-string-literals"></a><span data-ttu-id="978b7-284">內插字串常值</span><span class="sxs-lookup"><span data-stu-id="978b7-284">Interpolated string literals</span></span>

<span data-ttu-id="978b7-285">內插字串常值類似于字串常值，但包含以`{`和`}`分隔的洞，也就是運算式。</span><span class="sxs-lookup"><span data-stu-id="978b7-285">Interpolated string literals are similar to string literals, but contain holes delimited by `{` and `}`, wherein expressions can occur.</span></span> <span data-ttu-id="978b7-286">在執行時間，系統會評估運算式，其目的是要將其文字形式替換成出現洞處的字串。</span><span class="sxs-lookup"><span data-stu-id="978b7-286">At runtime, the expressions are evaluated with the purpose of having their textual forms substituted into the string at the place where the hole occurs.</span></span> <span data-ttu-id="978b7-287">字串插補的語法和語義會在區段（[字串插值](expressions.md#interpolated-strings)）中說明。</span><span class="sxs-lookup"><span data-stu-id="978b7-287">The syntax and semantics of string interpolation are described in section ([Interpolated strings](expressions.md#interpolated-strings)).</span></span>

<span data-ttu-id="978b7-288">如同字串常值，插入字串常值可以是一般或逐字。</span><span class="sxs-lookup"><span data-stu-id="978b7-288">Like string literals, interpolated string literals can be either regular or verbatim.</span></span> <span data-ttu-id="978b7-289">內插的一般字串常值`$"`是`"`以和分隔，而內插逐字字串`$@"`常`"`值則是以和分隔。</span><span class="sxs-lookup"><span data-stu-id="978b7-289">Interpolated regular string literals are delimited by `$"` and `"`, and interpolated verbatim string literals are delimited by `$@"` and `"`.</span></span>

<span data-ttu-id="978b7-290">就像其他常值一樣，插入字串常值的詞法分析一開始會產生單一權杖，如以下文法所示。</span><span class="sxs-lookup"><span data-stu-id="978b7-290">Like other literals, lexical analysis of an interpolated string literal initially results in a single token, as per the grammar below.</span></span> <span data-ttu-id="978b7-291">不過，在語法分析之前，插入字串常值的單一 token 會針對部分包含洞的字串，分成數個標記，而出現在洞孔中的輸入元素會再次分析。</span><span class="sxs-lookup"><span data-stu-id="978b7-291">However, before syntactic analysis, the single token of an interpolated string literal is broken into several tokens for the parts of the string enclosing the holes, and the input elements occurring in the holes are lexically analysed again.</span></span> <span data-ttu-id="978b7-292">這可能會產生更多要處理的插入字串常值，但如果詞法正確，最後會導致一系列標記，以供語法分析處理。</span><span class="sxs-lookup"><span data-stu-id="978b7-292">This may in turn produce more interpolated string literals to be processed, but, if lexically correct, will eventually lead to a sequence of tokens for syntactic analysis to process.</span></span>

```antlr
interpolated_string_literal
    : '$' interpolated_regular_string_literal
    | '$' interpolated_verbatim_string_literal
    ;

interpolated_regular_string_literal
    : interpolated_regular_string_whole
    | interpolated_regular_string_start  interpolated_regular_string_literal_body interpolated_regular_string_end
    ;

interpolated_regular_string_literal_body
    : regular_balanced_text
    | interpolated_regular_string_literal_body interpolated_regular_string_mid regular_balanced_text
    ;

interpolated_regular_string_whole
    : '"' interpolated_regular_string_character* '"'
    ;

interpolated_regular_string_start
    : '"' interpolated_regular_string_character* '{'
    ;

interpolated_regular_string_mid
    : interpolation_format? '}' interpolated_regular_string_characters_after_brace? '{'
    ;

interpolated_regular_string_end
    : interpolation_format? '}' interpolated_regular_string_characters_after_brace? '"'
    ;

interpolated_regular_string_characters_after_brace
    : interpolated_regular_string_character_no_brace
    | interpolated_regular_string_characters_after_brace interpolated_regular_string_character
    ;

interpolated_regular_string_character
    : single_interpolated_regular_string_character
    | simple_escape_sequence
    | hexadecimal_escape_sequence
    | unicode_escape_sequence
    | open_brace_escape_sequence
    | close_brace_escape_sequence
    ;

interpolated_regular_string_character_no_brace
    : '<Any interpolated_regular_string_character except close_brace_escape_sequence and any hexadecimal_escape_sequence or unicode_escape_sequence designating } (U+007D)>'
    ;

single_interpolated_regular_string_character
    : '<Any character except \" (U+0022), \\ (U+005C), { (U+007B), } (U+007D), and new_line_character>'
    ;

open_brace_escape_sequence
    : '{{'
    ;

close_brace_escape_sequence
    : '}}'
    ;
    
regular_balanced_text
    : regular_balanced_text_part+
    ;

regular_balanced_text_part
    : single_regular_balanced_text_character
    | delimited_comment
    | '@' identifier_or_keyword
    | string_literal
    | interpolated_string_literal
    | '(' regular_balanced_text ')'
    | '[' regular_balanced_text ']'
    | '{' regular_balanced_text '}'
    ;
    
single_regular_balanced_text_character
    : '<Any character except / (U+002F), @ (U+0040), \" (U+0022), $ (U+0024), ( (U+0028), ) (U+0029), [ (U+005B), ] (U+005D), { (U+007B), } (U+007D) and new_line_character>'
    | '</ (U+002F), if not directly followed by / (U+002F) or * (U+002A)>'
    ;
    
interpolation_format
    : interpolation_format_character+
    ;
    
interpolation_format_character
    : '<Any character except \" (U+0022), : (U+003A), { (U+007B) and } (U+007D)>'
    ;
    
interpolated_verbatim_string_literal
    : interpolated_verbatim_string_whole
    | interpolated_verbatim_string_start interpolated_verbatim_string_literal_body interpolated_verbatim_string_end
    ;

interpolated_verbatim_string_literal_body
    : verbatim_balanced_text
    | interpolated_verbatim_string_literal_body interpolated_verbatim_string_mid verbatim_balanced_text
    ;
    
interpolated_verbatim_string_whole
    : '@"' interpolated_verbatim_string_character* '"'
    ;
    
interpolated_verbatim_string_start
    : '@"' interpolated_verbatim_string_character* '{'
    ;
    
interpolated_verbatim_string_mid
    : interpolation_format? '}' interpolated_verbatim_string_characters_after_brace? '{'
    ;
    
interpolated_verbatim_string_end
    : interpolation_format? '}' interpolated_verbatim_string_characters_after_brace? '"'
    ;
    
interpolated_verbatim_string_characters_after_brace
    : interpolated_verbatim_string_character_no_brace
    | interpolated_verbatim_string_characters_after_brace interpolated_verbatim_string_character
    ;
    
interpolated_verbatim_string_character
    : single_interpolated_verbatim_string_character
    | quote_escape_sequence
    | open_brace_escape_sequence
    | close_brace_escape_sequence
    ;
    
interpolated_verbatim_string_character_no_brace
    : '<Any interpolated_verbatim_string_character except close_brace_escape_sequence>'
    ;
    
single_interpolated_verbatim_string_character
    : '<Any character except \" (U+0022), { (U+007B) and } (U+007D)>'
    ;
    
verbatim_balanced_text
    : verbatim_balanced_text_part+
    ;

verbatim_balanced_text_part
    : single_verbatim_balanced_text_character
    | comment
    | '@' identifier_or_keyword
    | string_literal
    | interpolated_string_literal
    | '(' verbatim_balanced_text ')'
    | '[' verbatim_balanced_text ']'
    | '{' verbatim_balanced_text '}'
    ;
    
single_verbatim_balanced_text_character
    : '<Any character except / (U+002F), @ (U+0040), \" (U+0022), $ (U+0024), ( (U+0028), ) (U+0029), [ (U+005B), ] (U+005D), { (U+007B) and } (U+007D)>'
    | '</ (U+002F), if not directly followed by / (U+002F) or * (U+002A)>'
    ;
```

<span data-ttu-id="978b7-293">*Interpolated_string_literal* token 會依照*interpolated_string_literal*中的出現順序，以多個權杖和其他輸入元素的方式向量：</span><span class="sxs-lookup"><span data-stu-id="978b7-293">An *interpolated_string_literal* token is reinterpreted as multiple tokens and other input elements as follows, in order of occurrence in the *interpolated_string_literal*:</span></span>

* <span data-ttu-id="978b7-294">下列專案的出現次數會向量為不同的個別權杖：前置的 `$` 號、 *interpolated_regular_string_whole*、 *interpolated_regular_string_start*、 *interpolated_regular_string_mid*、 *interpolated_regular_string_end*、 *interpolated_verbatim_string_whole*、 *interpolated_verbatim_string_start*、 *interpolated_verbatim_string_mid*和*interpolated_verbatim_string_end*。</span><span class="sxs-lookup"><span data-stu-id="978b7-294">Occurrences of the following are reinterpreted as separate individual tokens: the leading `$` sign, *interpolated_regular_string_whole*, *interpolated_regular_string_start*, *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_whole*, *interpolated_verbatim_string_start*, *interpolated_verbatim_string_mid* and *interpolated_verbatim_string_end*.</span></span>
* <span data-ttu-id="978b7-295">在這些專案之間出現的*regular_balanced_text*和*verbatim_balanced_text*會重新處理為*input_section* （[詞法分析](lexical-structure.md#lexical-analysis)），並會向量為輸入元素的結果序列。</span><span class="sxs-lookup"><span data-stu-id="978b7-295">Occurrences of *regular_balanced_text* and *verbatim_balanced_text* between these are reprocessed as an *input_section* ([Lexical analysis](lexical-structure.md#lexical-analysis)) and are reinterpreted as the resulting sequence of input elements.</span></span> <span data-ttu-id="978b7-296">這些可能會輪流包含要向量的插入字串常值標記。</span><span class="sxs-lookup"><span data-stu-id="978b7-296">These may in turn include interpolated string literal tokens to be reinterpreted.</span></span>

<span data-ttu-id="978b7-297">語法分析會將 token 重新合併為*interpolated_string_expression* （[字串插值](expressions.md#interpolated-strings)）。</span><span class="sxs-lookup"><span data-stu-id="978b7-297">Syntactic analysis will recombine the tokens into an *interpolated_string_expression* ([Interpolated strings](expressions.md#interpolated-strings)).</span></span>

<span data-ttu-id="978b7-298">待辦事項範例</span><span class="sxs-lookup"><span data-stu-id="978b7-298">Examples TODO</span></span>


#### <a name="the-null-literal"></a><span data-ttu-id="978b7-299">Null 常值</span><span class="sxs-lookup"><span data-stu-id="978b7-299">The null literal</span></span>

```antlr
null_literal
    : 'null'
    ;
```

<span data-ttu-id="978b7-300">*Null_literal*可以隱含地轉換成參考型別或可為 null 的類型。</span><span class="sxs-lookup"><span data-stu-id="978b7-300">The  *null_literal* can be implicitly converted to a reference type or nullable type.</span></span>

### <a name="operators-and-punctuators"></a><span data-ttu-id="978b7-301">運算子和標點符號</span><span class="sxs-lookup"><span data-stu-id="978b7-301">Operators and punctuators</span></span>

<span data-ttu-id="978b7-302">有數種運算子和標點符號。</span><span class="sxs-lookup"><span data-stu-id="978b7-302">There are several kinds of operators and punctuators.</span></span> <span data-ttu-id="978b7-303">運算式中會使用運算子來描述涉及一或多個運算元的作業。</span><span class="sxs-lookup"><span data-stu-id="978b7-303">Operators are used in expressions to describe operations involving one or more operands.</span></span> <span data-ttu-id="978b7-304">例如，運算式`a + b`會`+`使用運算子來加入兩個運算元`a`和`b`。</span><span class="sxs-lookup"><span data-stu-id="978b7-304">For example, the expression `a + b` uses the `+` operator to add the two operands `a` and `b`.</span></span> <span data-ttu-id="978b7-305">標點符號適用于分組和分隔。</span><span class="sxs-lookup"><span data-stu-id="978b7-305">Punctuators are for grouping and separating.</span></span>

```antlr
operator_or_punctuator
    : '{'  | '}'  | '['  | ']'  | '('   | ')'  | '.'  | ','  | ':'  | ';'
    | '+'  | '-'  | '*'  | '/'  | '%'   | '&'  | '|'  | '^'  | '!'  | '~'
    | '='  | '<'  | '>'  | '?'  | '??'  | '::' | '++' | '--' | '&&' | '||'
    | '->' | '==' | '!=' | '<=' | '>='  | '+=' | '-=' | '*=' | '/=' | '%='
    | '&=' | '|=' | '^=' | '<<' | '<<=' | '=>'
    ;

right_shift
    : '>>'
    ;

right_shift_assignment
    : '>>='
    ;
```

<span data-ttu-id="978b7-306">*Right_shift*和*right_shift_assignment*生產中的分隔號是用來指出，與語法文法中的其他生產不同，標記之間不允許任何類型的字元（甚至不是空白字元）。</span><span class="sxs-lookup"><span data-stu-id="978b7-306">The vertical bar in the *right_shift* and *right_shift_assignment* productions are used to indicate that, unlike other productions in the syntactic grammar, no characters of any kind (not even whitespace) are allowed between the tokens.</span></span> <span data-ttu-id="978b7-307">這些生產會特別處理，以啟用*type_parameter_list*s （[類型參數](classes.md#type-parameters)）的正確處理。</span><span class="sxs-lookup"><span data-stu-id="978b7-307">These productions are treated specially in order to enable the correct  handling of *type_parameter_list*s ([Type parameters](classes.md#type-parameters)).</span></span>

## <a name="pre-processing-directives"></a><span data-ttu-id="978b7-308">前置處理指示詞</span><span class="sxs-lookup"><span data-stu-id="978b7-308">Pre-processing directives</span></span>

<span data-ttu-id="978b7-309">前置處理指示詞可讓您有條件地略過原始程式檔區段、報告錯誤和警告條件，以及描繪原始程式碼的不同區域。</span><span class="sxs-lookup"><span data-stu-id="978b7-309">The pre-processing directives provide the ability to conditionally skip sections of source files, to report error and warning conditions, and to delineate distinct regions of source code.</span></span> <span data-ttu-id="978b7-310">「前置處理指示詞」一詞僅用於與 C 和C++程式設計語言一致性。</span><span class="sxs-lookup"><span data-stu-id="978b7-310">The term "pre-processing directives" is used only for consistency with the C and C++ programming languages.</span></span> <span data-ttu-id="978b7-311">在C#中，沒有個別的前置處理步驟。前置處理指示詞會在詞法分析階段中處理。</span><span class="sxs-lookup"><span data-stu-id="978b7-311">In C#, there is no separate pre-processing step; pre-processing directives are processed as part of the lexical analysis phase.</span></span>

```antlr
pp_directive
    : pp_declaration
    | pp_conditional
    | pp_line
    | pp_diagnostic
    | pp_region
    | pp_pragma
    ;
```

<span data-ttu-id="978b7-312">下列是可用的前置處理指示詞：</span><span class="sxs-lookup"><span data-stu-id="978b7-312">The following pre-processing directives are available:</span></span>

*  <span data-ttu-id="978b7-313">`#define`和`#undef`，分別用來定義和取消定義條件式編譯[符號（宣告](lexical-structure.md#declaration-directives)指示詞）。</span><span class="sxs-lookup"><span data-stu-id="978b7-313">`#define` and `#undef`, which are used to define and undefine, respectively, conditional compilation symbols ([Declaration directives](lexical-structure.md#declaration-directives)).</span></span>
*  <span data-ttu-id="978b7-314">`#if`、 `#elif`、 `#else`和`#endif`，用來有條件地略過原始程式碼區段（條件式[編譯](lexical-structure.md#conditional-compilation-directives)指示詞）。</span><span class="sxs-lookup"><span data-stu-id="978b7-314">`#if`, `#elif`, `#else`, and `#endif`, which are used to conditionally skip sections of source code ([Conditional compilation directives](lexical-structure.md#conditional-compilation-directives)).</span></span>
*  <span data-ttu-id="978b7-315">`#line`，用來控制針對錯誤和警告發出的行號（程式[行](lexical-structure.md#line-directives)指示詞）。</span><span class="sxs-lookup"><span data-stu-id="978b7-315">`#line`, which is used to control line numbers emitted for errors and warnings ([Line directives](lexical-structure.md#line-directives)).</span></span>
*  <span data-ttu-id="978b7-316">`#error`和`#warning`，分別用來發出錯誤和警告（[診斷](lexical-structure.md#diagnostic-directives)指示詞）。</span><span class="sxs-lookup"><span data-stu-id="978b7-316">`#error` and `#warning`, which are used to issue errors and warnings, respectively ([Diagnostic directives](lexical-structure.md#diagnostic-directives)).</span></span>
*  <span data-ttu-id="978b7-317">`#region`和`#endregion`，用來明確地標記原始程式碼區段（[區域](lexical-structure.md#region-directives)指示詞）。</span><span class="sxs-lookup"><span data-stu-id="978b7-317">`#region` and `#endregion`, which are used to explicitly mark sections of source code ([Region directives](lexical-structure.md#region-directives)).</span></span>
*  <span data-ttu-id="978b7-318">`#pragma`，用來指定編譯器的選擇性內容資訊（[Pragma](lexical-structure.md#pragma-directives)指示詞）。</span><span class="sxs-lookup"><span data-stu-id="978b7-318">`#pragma`, which is used to specify optional contextual information to the compiler ([Pragma directives](lexical-structure.md#pragma-directives)).</span></span>

<span data-ttu-id="978b7-319">前置處理指示詞一律會佔用個別的源程式碼，並且一律以`#`字元和前置處理指示詞名稱開頭。</span><span class="sxs-lookup"><span data-stu-id="978b7-319">A pre-processing directive always occupies a separate line of source code and always begins with a `#` character and a pre-processing directive name.</span></span> <span data-ttu-id="978b7-320">空白字元可能會在`#`字元之前，以及`#`字元與指示詞名稱之間發生。</span><span class="sxs-lookup"><span data-stu-id="978b7-320">White space may occur before the `#` character and between the `#` character and the directive name.</span></span>

<span data-ttu-id="978b7-321">`#define`包含、、 、、`#elif` `#undef` `#if` 、、`#line`或指示詞的`#endregion`原始程式列，其結尾可能會是單行批註。 `#else` `#endif`</span><span class="sxs-lookup"><span data-stu-id="978b7-321">A source line containing a `#define`, `#undef`, `#if`, `#elif`, `#else`, `#endif`, `#line`, or `#endregion` directive may end with a single-line comment.</span></span> <span data-ttu-id="978b7-322">包含前置處理指示`/* */`詞的來源行上不允許使用分隔的批註（批註的樣式）。</span><span class="sxs-lookup"><span data-stu-id="978b7-322">Delimited comments (the `/* */` style of comments) are not permitted on source lines containing pre-processing directives.</span></span>

<span data-ttu-id="978b7-323">前置處理指示詞不是標記，而且不是的語法文法的一部分C#。</span><span class="sxs-lookup"><span data-stu-id="978b7-323">Pre-processing directives are not tokens and are not part of the syntactic grammar of C#.</span></span> <span data-ttu-id="978b7-324">不過，前置處理指示詞可以用來包含或排除權杖的順序，而這種方式可以影響C#程式的意義。</span><span class="sxs-lookup"><span data-stu-id="978b7-324">However, pre-processing directives can be used to include or exclude sequences of tokens and can in that way affect the meaning of a C# program.</span></span> <span data-ttu-id="978b7-325">例如，在編譯時，程式：</span><span class="sxs-lookup"><span data-stu-id="978b7-325">For example, when compiled, the program:</span></span>
```csharp
#define A
#undef B

class C
{
#if A
    void F() {}
#else
    void G() {}
#endif

#if B
    void H() {}
#else
    void I() {}
#endif
}
```
<span data-ttu-id="978b7-326">會產生與程式完全相同的標記順序：</span><span class="sxs-lookup"><span data-stu-id="978b7-326">results in the exact same sequence of tokens as the program:</span></span>
```csharp
class C
{
    void F() {}
    void I() {}
}
```

<span data-ttu-id="978b7-327">因此，雖然在詞法上，這兩個程式在語法上完全不同，但它們是相同的。</span><span class="sxs-lookup"><span data-stu-id="978b7-327">Thus, whereas lexically, the two programs are quite different, syntactically, they are identical.</span></span>

### <a name="conditional-compilation-symbols"></a><span data-ttu-id="978b7-328">條件式編譯符號</span><span class="sxs-lookup"><span data-stu-id="978b7-328">Conditional compilation symbols</span></span>

<span data-ttu-id="978b7-329">`#if`、 、和`#else`指示詞所提供的條件式編譯功能，是透過前置處理運算式（[前置處理運算式](lexical-structure.md#pre-processing-expressions)）和條件式來控制`#endif` `#elif`編譯符號。</span><span class="sxs-lookup"><span data-stu-id="978b7-329">The conditional compilation functionality provided by the `#if`, `#elif`, `#else`, and `#endif` directives is controlled through pre-processing expressions ([Pre-processing expressions](lexical-structure.md#pre-processing-expressions)) and conditional compilation symbols.</span></span>

```antlr
conditional_symbol
    : '<Any identifier_or_keyword except true or false>'
    ;
```

<span data-ttu-id="978b7-330">條件式編譯符號有兩種可能的狀態：***已定義***或***未定義***。</span><span class="sxs-lookup"><span data-stu-id="978b7-330">A conditional compilation symbol has two possible states: ***defined*** or ***undefined***.</span></span> <span data-ttu-id="978b7-331">在原始程式檔的詞法處理開始時，除非已由外部機制（例如命令列編譯器選項）明確定義，否則不會定義條件式編譯符號。</span><span class="sxs-lookup"><span data-stu-id="978b7-331">At the beginning of the lexical processing of a source file, a conditional compilation symbol is undefined unless it has been explicitly defined by an external mechanism (such as a command-line compiler option).</span></span> <span data-ttu-id="978b7-332">`#define`當處理指示詞時，該指示詞中名為的條件式編譯符號會在該原始程式檔中定義。</span><span class="sxs-lookup"><span data-stu-id="978b7-332">When a `#define` directive is processed, the conditional compilation symbol named in that directive becomes defined in that source file.</span></span> <span data-ttu-id="978b7-333">符號會保持定義，直到`#undef`處理該相同符號的指示詞，或直到到達原始程式檔結尾為止。</span><span class="sxs-lookup"><span data-stu-id="978b7-333">The symbol remains defined until an `#undef` directive for that same symbol is processed, or until the end of the source file is reached.</span></span> <span data-ttu-id="978b7-334">其含意是，一個原始`#define`程式`#undef`檔中的和指示詞不會影響相同程式中的其他來源檔案。</span><span class="sxs-lookup"><span data-stu-id="978b7-334">An implication of this is that `#define` and `#undef` directives in one source file have no effect on other source files in the same program.</span></span>

<span data-ttu-id="978b7-335">在前置處理運算式中參考時，定義的條件式編譯符號會有布林值`true`，且未定義的條件式編譯符號會有`false`布林值。</span><span class="sxs-lookup"><span data-stu-id="978b7-335">When referenced in a pre-processing expression, a defined conditional compilation symbol has the boolean value `true`, and an undefined conditional compilation symbol has the boolean value `false`.</span></span> <span data-ttu-id="978b7-336">在前置處理運算式中參考條件式編譯符號之前，並不需要明確宣告。</span><span class="sxs-lookup"><span data-stu-id="978b7-336">There is no requirement that conditional compilation symbols be explicitly declared before they are referenced in pre-processing expressions.</span></span> <span data-ttu-id="978b7-337">相反地，未宣告的符號只是未定義的`false`，因此具有值。</span><span class="sxs-lookup"><span data-stu-id="978b7-337">Instead, undeclared symbols are simply undefined and thus have the value `false`.</span></span>

<span data-ttu-id="978b7-338">條件式編譯符號的命名空間是不同的，而且與C#程式中所有其他已命名的實體不同。</span><span class="sxs-lookup"><span data-stu-id="978b7-338">The name space for conditional compilation symbols is distinct and separate from all other named entities in a C# program.</span></span> <span data-ttu-id="978b7-339">條件式編譯符號只能在`#define`和`#undef`指示詞以及前置處理運算式中參考。</span><span class="sxs-lookup"><span data-stu-id="978b7-339">Conditional compilation symbols can only be referenced in `#define` and `#undef` directives and in pre-processing expressions.</span></span>

### <a name="pre-processing-expressions"></a><span data-ttu-id="978b7-340">前置處理運算式</span><span class="sxs-lookup"><span data-stu-id="978b7-340">Pre-processing expressions</span></span>

<span data-ttu-id="978b7-341">前置處理運算式可以出現在和`#if` `#elif`指示詞中。</span><span class="sxs-lookup"><span data-stu-id="978b7-341">Pre-processing expressions can occur in `#if` and `#elif` directives.</span></span> <span data-ttu-id="978b7-342">前置處理`!`運算式中`!=`允許`&&`使用`||`運算子`==`、、和，而括弧可用於群組。</span><span class="sxs-lookup"><span data-stu-id="978b7-342">The operators `!`, `==`, `!=`, `&&` and `||` are permitted in pre-processing expressions, and parentheses may be used for grouping.</span></span>

```antlr
pp_expression
    : whitespace? pp_or_expression whitespace?
    ;

pp_or_expression
    : pp_and_expression
    | pp_or_expression whitespace? '||' whitespace? pp_and_expression
    ;

pp_and_expression
    : pp_equality_expression
    | pp_and_expression whitespace? '&&' whitespace? pp_equality_expression
    ;

pp_equality_expression
    : pp_unary_expression
    | pp_equality_expression whitespace? '==' whitespace? pp_unary_expression
    | pp_equality_expression whitespace? '!=' whitespace? pp_unary_expression
    ;

pp_unary_expression
    : pp_primary_expression
    | '!' whitespace? pp_unary_expression
    ;

pp_primary_expression
    : 'true'
    | 'false'
    | conditional_symbol
    | '(' whitespace? pp_expression whitespace? ')'
    ;
```

<span data-ttu-id="978b7-343">在前置處理運算式中參考時，定義的條件式編譯符號會有布林值`true`，且未定義的條件式編譯符號會有`false`布林值。</span><span class="sxs-lookup"><span data-stu-id="978b7-343">When referenced in a pre-processing expression, a defined conditional compilation symbol has the boolean value `true`, and an undefined conditional compilation symbol has the boolean value `false`.</span></span>

<span data-ttu-id="978b7-344">預先處理運算式的評估一律會產生布林值。</span><span class="sxs-lookup"><span data-stu-id="978b7-344">Evaluation of a pre-processing expression always yields a boolean value.</span></span> <span data-ttu-id="978b7-345">前置處理運算式的評估規則與常數運算式（[常數運算式](expressions.md#constant-expressions)）相同，不同之處在于唯一可以參考的使用者定義實體是條件式編譯符號。</span><span class="sxs-lookup"><span data-stu-id="978b7-345">The rules of evaluation for a pre-processing expression are the same as those for a constant expression ([Constant expressions](expressions.md#constant-expressions)), except that the only user-defined entities that can be referenced are conditional compilation symbols.</span></span>

### <a name="declaration-directives"></a><span data-ttu-id="978b7-346">宣告指示詞</span><span class="sxs-lookup"><span data-stu-id="978b7-346">Declaration directives</span></span>

<span data-ttu-id="978b7-347">宣告指示詞是用來定義或取消定義條件式編譯符號。</span><span class="sxs-lookup"><span data-stu-id="978b7-347">The declaration directives are used to define or undefine conditional compilation symbols.</span></span>

```antlr
pp_declaration
    : whitespace? '#' whitespace? 'define' whitespace conditional_symbol pp_new_line
    | whitespace? '#' whitespace? 'undef' whitespace conditional_symbol pp_new_line
    ;

pp_new_line
    : whitespace? single_line_comment? new_line
    ;
```

<span data-ttu-id="978b7-348">處理`#define`指示詞會使指定的條件式編譯符號變成已定義，從指示詞後面的來源行開始。</span><span class="sxs-lookup"><span data-stu-id="978b7-348">The processing of a `#define` directive causes the given conditional compilation symbol to become defined, starting with the source line that follows the directive.</span></span> <span data-ttu-id="978b7-349">同樣地，處理`#undef`指示詞會使指定的條件式編譯符號變成未定義，從指示詞後面的來源行開始。</span><span class="sxs-lookup"><span data-stu-id="978b7-349">Likewise, the processing of an `#undef` directive causes the given conditional compilation symbol to become undefined, starting with the source line that follows the directive.</span></span>

<span data-ttu-id="978b7-350">來源`#define`檔案`#undef`中的任何和指示詞必須在來源檔案中的第一個*標記（[token](lexical-structure.md#tokens)* ）之前發生，否則會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="978b7-350">Any `#define` and `#undef` directives in a source file must occur before the first *token* ([Tokens](lexical-structure.md#tokens)) in the source file; otherwise a compile-time error occurs.</span></span> <span data-ttu-id="978b7-351">在直覺的詞彙`#define`中`#undef` ，和指示詞必須在原始程式檔中的任何「實際程式碼」之前。</span><span class="sxs-lookup"><span data-stu-id="978b7-351">In intuitive terms, `#define` and `#undef` directives must precede any "real code" in the source file.</span></span>

<span data-ttu-id="978b7-352">範例：</span><span class="sxs-lookup"><span data-stu-id="978b7-352">The example:</span></span>
```csharp
#define Enterprise

#if Professional || Enterprise
    #define Advanced
#endif

namespace Megacorp.Data
{
    #if Advanced
    class PivotTable {...}
    #endif
}
```
<span data-ttu-id="978b7-353">有效，因為`#define`指示詞在來源檔案中的第`namespace`一個標記（關鍵字）前面。</span><span class="sxs-lookup"><span data-stu-id="978b7-353">is valid because the `#define` directives precede the first token (the `namespace` keyword) in the source file.</span></span>

<span data-ttu-id="978b7-354">下列範例會產生編譯時期錯誤，因為會遵循實際`#define`的程式碼：</span><span class="sxs-lookup"><span data-stu-id="978b7-354">The following example results in a compile-time error because a `#define` follows real code:</span></span>
```csharp
#define A
namespace N
{
    #define B
    #if B
    class Class1 {}
    #endif
}
```

<span data-ttu-id="978b7-355">可能會定義已定義的條件式編譯符號，而不會有該符號的任何中間`#undef`。 `#define`</span><span class="sxs-lookup"><span data-stu-id="978b7-355">A `#define` may define a conditional compilation symbol that is already defined, without there being any intervening `#undef` for that symbol.</span></span> <span data-ttu-id="978b7-356">下列範例會定義條件式編譯符號`A` ，然後再次定義它。</span><span class="sxs-lookup"><span data-stu-id="978b7-356">The example below defines a conditional compilation symbol `A` and then defines it again.</span></span>
```csharp
#define A
#define A
```

<span data-ttu-id="978b7-357">`#undef`可能會「取消定義」未定義的條件式編譯符號。</span><span class="sxs-lookup"><span data-stu-id="978b7-357">A `#undef` may "undefine" a conditional compilation symbol that is not defined.</span></span> <span data-ttu-id="978b7-358">下列範例會定義條件式編譯符號`A` ，然後將它重新定義為兩次`#undef` ; 雖然第二個沒有作用，但仍然有效。</span><span class="sxs-lookup"><span data-stu-id="978b7-358">The example below defines a conditional compilation symbol `A` and then undefines it twice; although the second `#undef` has no effect, it is still valid.</span></span>
```csharp
#define A
#undef A
#undef A
```

### <a name="conditional-compilation-directives"></a><span data-ttu-id="978b7-359">條件式編譯指示詞</span><span class="sxs-lookup"><span data-stu-id="978b7-359">Conditional compilation directives</span></span>

<span data-ttu-id="978b7-360">條件式編譯指示詞是用來有條件地包含或排除來源檔案的部分。</span><span class="sxs-lookup"><span data-stu-id="978b7-360">The conditional compilation directives are used to conditionally include or exclude portions of a source file.</span></span>

```antlr
pp_conditional
    : pp_if_section pp_elif_section* pp_else_section? pp_endif
    ;

pp_if_section
    : whitespace? '#' whitespace? 'if' whitespace pp_expression pp_new_line conditional_section?
    ;

pp_elif_section
    : whitespace? '#' whitespace? 'elif' whitespace pp_expression pp_new_line conditional_section?
    ;

pp_else_section:
    | whitespace? '#' whitespace? 'else' pp_new_line conditional_section?
    ;

pp_endif
    : whitespace? '#' whitespace? 'endif' pp_new_line
    ;

conditional_section
    : input_section
    | skipped_section
    ;

skipped_section
    : skipped_section_part+
    ;

skipped_section_part
    : skipped_characters? new_line
    | pp_directive
    ;

skipped_characters
    : whitespace? not_number_sign input_character*
    ;

not_number_sign
    : '<Any input_character except #>'
    ;
```

<span data-ttu-id="978b7-361">如語法所指示，條件式編譯指示詞必須撰寫成由、依序`#if` 、指示詞、零或多個`#elif`指示詞、零或`#endif`一個`#else`指示詞和指示片語成的集合。</span><span class="sxs-lookup"><span data-stu-id="978b7-361">As indicated by the syntax, conditional compilation directives must be written as sets consisting of, in order, an `#if` directive, zero or more `#elif` directives, zero or one `#else` directive, and an `#endif` directive.</span></span> <span data-ttu-id="978b7-362">在指示詞之間，是原始程式碼的條件式區段。</span><span class="sxs-lookup"><span data-stu-id="978b7-362">Between the directives are conditional sections of source code.</span></span> <span data-ttu-id="978b7-363">每個區段都是由緊接在前面的指示詞所控制。</span><span class="sxs-lookup"><span data-stu-id="978b7-363">Each section is controlled by the immediately preceding directive.</span></span> <span data-ttu-id="978b7-364">條件式區段本身可能會包含嵌套的條件式編譯指示詞，前提是這些指示詞會形成完整集合。</span><span class="sxs-lookup"><span data-stu-id="978b7-364">A conditional section may itself contain nested conditional compilation directives provided these directives form complete sets.</span></span>

<span data-ttu-id="978b7-365">*Pp_conditional*最多會選取其中一個包含的*conditional_section*，以進行一般的詞法處理：</span><span class="sxs-lookup"><span data-stu-id="978b7-365">A *pp_conditional* selects at most one of the contained *conditional_section*s for normal lexical processing:</span></span>

*  <span data-ttu-id="978b7-366">@No__t-1 和 @no__t 2 指示詞的*pp_expression*s 會依序評估，直到其中一個產生 `true` 為止。</span><span class="sxs-lookup"><span data-stu-id="978b7-366">The *pp_expression*s of the `#if` and `#elif` directives are evaluated in order until one yields `true`.</span></span> <span data-ttu-id="978b7-367">如果運算式產生 `true`，則會選取對應指示詞的*conditional_section* 。</span><span class="sxs-lookup"><span data-stu-id="978b7-367">If an expression yields `true`, the *conditional_section* of the corresponding directive is selected.</span></span>
*  <span data-ttu-id="978b7-368">如果所有*pp_expression*s 都會產生 `false`，而且如果有 @no__t 2 指示詞，則會選取 `#else` 指示詞的*conditional_section* 。</span><span class="sxs-lookup"><span data-stu-id="978b7-368">If all *pp_expression*s yield `false`, and if an `#else` directive is present, the *conditional_section* of the `#else` directive is selected.</span></span>
*  <span data-ttu-id="978b7-369">否則，不會選取任何*conditional_section* 。</span><span class="sxs-lookup"><span data-stu-id="978b7-369">Otherwise, no *conditional_section* is selected.</span></span>

<span data-ttu-id="978b7-370">選取的*conditional_section*（如果有的話）會當做一般*input_section*處理：區段中包含的原始程式碼必須遵守詞法文法;權杖是從區段中的原始程式碼產生的;而區段中的前置處理指示詞具有指定的效果。</span><span class="sxs-lookup"><span data-stu-id="978b7-370">The selected *conditional_section*, if any, is processed as a normal *input_section*: the source code contained in the section must adhere to the lexical grammar; tokens are generated from the source code in the section; and pre-processing directives in the section have the prescribed effects.</span></span>

<span data-ttu-id="978b7-371">其餘的*conditional_section*（如果有的話）會當做*skipped_section*s 來處理：除了前置處理指示詞以外，區段中的原始程式碼不需要遵守詞法文法;不會從區段中的原始程式碼產生任何權杖;區段中的和前置處理指示詞必須是正確的，但不會進行處理。</span><span class="sxs-lookup"><span data-stu-id="978b7-371">The remaining *conditional_section*s, if any, are processed as *skipped_section*s: except for pre-processing directives, the source code in the section need not adhere to the lexical grammar; no tokens are generated from the source code in the section; and pre-processing directives in the section must be lexically correct but are not otherwise processed.</span></span> <span data-ttu-id="978b7-372">在處理為*skipped_section*的*conditional_section*中，任何嵌套的*conditional_section*s （包含在嵌套 `#if` ... `#endif` 和 `#region` ... `#endregion` 結構）也會處理為*skipped_區段*s。</span><span class="sxs-lookup"><span data-stu-id="978b7-372">Within a *conditional_section* that is being processed as a *skipped_section*, any nested *conditional_section*s (contained in nested `#if`...`#endif` and `#region`...`#endregion` constructs) are also processed as *skipped_section*s.</span></span>

<span data-ttu-id="978b7-373">下列範例說明條件式編譯指示詞可以如何加以嵌套：</span><span class="sxs-lookup"><span data-stu-id="978b7-373">The following example illustrates how conditional compilation directives can nest:</span></span>
```csharp
#define Debug       // Debugging on
#undef Trace        // Tracing off

class PurchaseTransaction
{
    void Commit() {
        #if Debug
            CheckConsistency();
            #if Trace
                WriteToLog(this.ToString());
            #endif
        #endif
        CommitHelper();
    }
}
```

<span data-ttu-id="978b7-374">除了前置處理指示詞之外，略過的原始程式碼不會受限於詞彙分析。</span><span class="sxs-lookup"><span data-stu-id="978b7-374">Except for pre-processing directives, skipped source code is not subject to lexical analysis.</span></span> <span data-ttu-id="978b7-375">例如，雖然`#else`區段中未結束的批註，下列內容有效：</span><span class="sxs-lookup"><span data-stu-id="978b7-375">For example, the following is valid despite the unterminated comment in the `#else` section:</span></span>
```csharp
#define Debug        // Debugging on

class PurchaseTransaction
{
    void Commit() {
        #if Debug
            CheckConsistency();
        #else
            /* Do something else
        #endif
    }
}
```

<span data-ttu-id="978b7-376">不過要注意的是，即使在原始程式碼的略過區段中，前置處理指示詞也必須是正確的。</span><span class="sxs-lookup"><span data-stu-id="978b7-376">Note, however, that pre-processing directives are required to be lexically correct even in skipped sections of source code.</span></span>

<span data-ttu-id="978b7-377">當前置處理指示詞出現在多行輸入元素內時，不會處理它們。</span><span class="sxs-lookup"><span data-stu-id="978b7-377">Pre-processing directives are not processed when they appear inside multi-line input elements.</span></span> <span data-ttu-id="978b7-378">例如，程式：</span><span class="sxs-lookup"><span data-stu-id="978b7-378">For example, the program:</span></span>
```csharp
class Hello
{
    static void Main() {
        System.Console.WriteLine(@"hello, 
#if Debug
        world
#else
        Nebraska
#endif
        ");
    }
}
```
<span data-ttu-id="978b7-379">輸出中的結果：</span><span class="sxs-lookup"><span data-stu-id="978b7-379">results in the output:</span></span>
```console
hello,
#if Debug
        world
#else
        Nebraska
#endif
```

<span data-ttu-id="978b7-380">在什麼嗎的情況下，所處理的前置處理指示詞集合可能取決於*pp_expression*的評估。</span><span class="sxs-lookup"><span data-stu-id="978b7-380">In peculiar cases, the set of pre-processing directives that is processed might depend on the evaluation of the *pp_expression*.</span></span> <span data-ttu-id="978b7-381">範例：</span><span class="sxs-lookup"><span data-stu-id="978b7-381">The example:</span></span>
```csharp
#if X
    /*
#else
    /* */ class Q { }
#endif
```
<span data-ttu-id="978b7-382">一律`class` `X`會產生相同的 token 資料流程（ `Q` `{` `}`），不論是否已定義。</span><span class="sxs-lookup"><span data-stu-id="978b7-382">always produces the same token stream (`class` `Q` `{` `}`), regardless of whether or not `X` is defined.</span></span> <span data-ttu-id="978b7-383">如果`X`已定義，則唯一處理的指示`#if`詞`#endif`為和，因為多行批註。</span><span class="sxs-lookup"><span data-stu-id="978b7-383">If `X` is defined, the only processed directives are `#if` and `#endif`, due to the multi-line comment.</span></span> <span data-ttu-id="978b7-384">如果`X`未定義，則三個指示`#if`詞`#else`（ `#endif`、、）是指示詞集合的一部分。</span><span class="sxs-lookup"><span data-stu-id="978b7-384">If `X` is undefined, then three directives (`#if`, `#else`, `#endif`) are part of the directive set.</span></span>

### <a name="diagnostic-directives"></a><span data-ttu-id="978b7-385">診斷指示詞</span><span class="sxs-lookup"><span data-stu-id="978b7-385">Diagnostic directives</span></span>

<span data-ttu-id="978b7-386">診斷指示詞用來明確產生錯誤和警告訊息，其報告方式與其他編譯時期錯誤和警告相同。</span><span class="sxs-lookup"><span data-stu-id="978b7-386">The diagnostic directives are used to explicitly generate error and warning messages that are reported in the same way as other compile-time errors and warnings.</span></span>

```antlr
pp_diagnostic
    : whitespace? '#' whitespace? 'error' pp_message
    | whitespace? '#' whitespace? 'warning' pp_message
    ;

pp_message
    : new_line
    | whitespace input_character* new_line
    ;
```

<span data-ttu-id="978b7-387">範例：</span><span class="sxs-lookup"><span data-stu-id="978b7-387">The example:</span></span>
```csharp
#warning Code review needed before check-in

#if Debug && Retail
    #error A build can't be both debug and retail
#endif

class Test {...}
```
<span data-ttu-id="978b7-388">如果條件式符號`Debug`和`Retail`皆已定義，一律會產生警告（「簽入前需要程式碼審核」），並產生編譯時期錯誤（「組建不能同時為 debug 和 retail」）。</span><span class="sxs-lookup"><span data-stu-id="978b7-388">always produces a warning ("Code review needed before check-in"), and produces a compile-time error ("A build can't be both debug and retail") if the conditional symbols `Debug` and `Retail` are both defined.</span></span> <span data-ttu-id="978b7-389">請注意， *pp_message*可以包含任意文字;具體而言，它不需要包含格式正確的 token，如單字中的單引號所示 `can't`。</span><span class="sxs-lookup"><span data-stu-id="978b7-389">Note that a *pp_message* can contain arbitrary text; specifically, it need not contain well-formed tokens, as shown by the single quote in the word `can't`.</span></span>

### <a name="region-directives"></a><span data-ttu-id="978b7-390">Region 指示詞</span><span class="sxs-lookup"><span data-stu-id="978b7-390">Region directives</span></span>

<span data-ttu-id="978b7-391">區域指示詞是用來明確標示原始程式碼的區域。</span><span class="sxs-lookup"><span data-stu-id="978b7-391">The region directives are used to explicitly mark regions of source code.</span></span>

```antlr
pp_region
    : pp_start_region conditional_section? pp_end_region
    ;

pp_start_region
    : whitespace? '#' whitespace? 'region' pp_message
    ;

pp_end_region
    : whitespace? '#' whitespace? 'endregion' pp_message
    ;
```

<span data-ttu-id="978b7-392">不會將任何語義意義附加至區域;區域可供程式設計人員或自動化工具用來標示原始程式碼的某個區段。</span><span class="sxs-lookup"><span data-stu-id="978b7-392">No semantic meaning is attached to a region; regions are intended for use by the programmer or by automated tools to mark a section of source code.</span></span> <span data-ttu-id="978b7-393">`#region` 或`#endregion`指示詞中指定的訊息同樣沒有語義意義，只是用來識別區域。</span><span class="sxs-lookup"><span data-stu-id="978b7-393">The message specified in a `#region` or `#endregion` directive likewise has no semantic meaning; it merely serves to identify the region.</span></span> <span data-ttu-id="978b7-394">比對 `#region` 和 @no__t 1 指示詞可能會有不同的*pp_message*。</span><span class="sxs-lookup"><span data-stu-id="978b7-394">Matching `#region` and `#endregion` directives may have different *pp_message*s.</span></span>

<span data-ttu-id="978b7-395">區域的詞法處理：</span><span class="sxs-lookup"><span data-stu-id="978b7-395">The lexical processing of a region:</span></span>
```csharp
#region
...
#endregion
```
<span data-ttu-id="978b7-396">完全對應于格式的條件式編譯指示詞的詞法處理：</span><span class="sxs-lookup"><span data-stu-id="978b7-396">corresponds exactly to the lexical processing of a conditional compilation directive of the form:</span></span>
```csharp
#if true
...
#endif
```

### <a name="line-directives"></a><span data-ttu-id="978b7-397">行指示詞</span><span class="sxs-lookup"><span data-stu-id="978b7-397">Line directives</span></span>

<span data-ttu-id="978b7-398">行指示詞可用來改變編譯器在輸出中報告的行號和原始程式檔名稱（例如警告和錯誤），以及呼叫端資訊屬性（[呼叫端資訊](attributes.md#caller-info-attributes)屬性）所使用的來原始檔案名。</span><span class="sxs-lookup"><span data-stu-id="978b7-398">Line directives may be used to alter the line numbers and source file names that are reported by the compiler in output such as warnings and errors, and that are used by caller info attributes ([Caller info attributes](attributes.md#caller-info-attributes)).</span></span>

<span data-ttu-id="978b7-399">行指示詞最常用於元程式設計工具中，以C#產生其他文字輸入的原始程式碼。</span><span class="sxs-lookup"><span data-stu-id="978b7-399">Line directives are most commonly used in meta-programming tools that generate C# source code from some other text input.</span></span>

```antlr
pp_line
    : whitespace? '#' whitespace? 'line' whitespace line_indicator pp_new_line
    ;

line_indicator
    : decimal_digit+ whitespace file_name
    | decimal_digit+
    | 'default'
    | 'hidden'
    ;

file_name
    : '"' file_name_character+ '"'
    ;

file_name_character
    : '<Any input_character except ">'
    ;
```

<span data-ttu-id="978b7-400">當沒有`#line`指示詞時，編譯器會在其輸出中報告真正的行號和原始程式檔名稱。</span><span class="sxs-lookup"><span data-stu-id="978b7-400">When no `#line` directives are present, the compiler reports true line numbers and source file names in its output.</span></span> <span data-ttu-id="978b7-401">在處理包含不是 `default` 之*line_indicator*的 `#line` 指示詞時，編譯器會將指示詞後面的那一行視為具有指定的行號（如果有指定，則會使用檔案名）。</span><span class="sxs-lookup"><span data-stu-id="978b7-401">When processing a `#line` directive that includes a *line_indicator* that is not `default`, the compiler treats the line after the directive as having the given line number (and file name, if specified).</span></span>

<span data-ttu-id="978b7-402">`#line default`指示詞會反轉所有前面 #line 指示詞的效果。</span><span class="sxs-lookup"><span data-stu-id="978b7-402">A `#line default` directive reverses the effect of all preceding #line directives.</span></span> <span data-ttu-id="978b7-403">編譯器會報告後續行的 true 行資訊，就像未處理`#line`任何指示詞一樣。</span><span class="sxs-lookup"><span data-stu-id="978b7-403">The compiler reports true line information for subsequent lines, precisely as if no `#line` directives had been processed.</span></span>

<span data-ttu-id="978b7-404">指示`#line hidden`詞不會影響錯誤訊息中所報告的檔案和行號，而是會影響來源層級的偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="978b7-404">A `#line hidden` directive has no effect on the file and line numbers reported in error messages, but does affect source level debugging.</span></span> <span data-ttu-id="978b7-405">在偵錯工具中，指示詞`#line hidden`和後續`#line`指示詞（不`#line hidden`是）之間的所有程式程式碼都沒有行號資訊。</span><span class="sxs-lookup"><span data-stu-id="978b7-405">When debugging, all lines between a `#line hidden` directive and the subsequent `#line` directive (that is not `#line hidden`) have no line number information.</span></span> <span data-ttu-id="978b7-406">當您在偵錯工具中逐步執行程式碼時，將會完全略過這幾行。</span><span class="sxs-lookup"><span data-stu-id="978b7-406">When stepping through code in the debugger, these lines will be skipped entirely.</span></span>

<span data-ttu-id="978b7-407">請注意， *file_name*與一般字串常值不同之處在于，不會處理該逸出字元;"`\`" 字元只會在*file_name*內指定一般的反斜線字元。</span><span class="sxs-lookup"><span data-stu-id="978b7-407">Note that a *file_name* differs from a regular string literal in that escape characters are not processed; the "`\`" character simply designates an ordinary backslash character within a *file_name*.</span></span>

### <a name="pragma-directives"></a><span data-ttu-id="978b7-408">Pragma 指示詞</span><span class="sxs-lookup"><span data-stu-id="978b7-408">Pragma directives</span></span>

<span data-ttu-id="978b7-409">`#pragma`前置處理指示詞是用來指定編譯器的選擇性內容資訊。</span><span class="sxs-lookup"><span data-stu-id="978b7-409">The `#pragma` preprocessing directive is used to specify optional contextual information to the compiler.</span></span> <span data-ttu-id="978b7-410">指示詞中`#pragma`提供的資訊永遠不會變更程式的語義。</span><span class="sxs-lookup"><span data-stu-id="978b7-410">The information supplied in a `#pragma` directive will never change program semantics.</span></span>

```antlr
pp_pragma
    : whitespace? '#' whitespace? 'pragma' whitespace pragma_body pp_new_line
    ;

pragma_body
    : pragma_warning_body
    ;
```

<span data-ttu-id="978b7-411">C#提供`#pragma`用來控制編譯器警告的指示詞。</span><span class="sxs-lookup"><span data-stu-id="978b7-411">C# provides `#pragma` directives to control compiler warnings.</span></span> <span data-ttu-id="978b7-412">語言的未來版本可能包含額外`#pragma`的指示詞。</span><span class="sxs-lookup"><span data-stu-id="978b7-412">Future versions of the language may include additional `#pragma` directives.</span></span> <span data-ttu-id="978b7-413">為了確保與其他C#編譯器的互通性C# ，Microsoft 編譯器不會發出未知`#pragma`指示詞的編譯錯誤; 不過，這類指示詞的確會產生警告。</span><span class="sxs-lookup"><span data-stu-id="978b7-413">To ensure interoperability with other C# compilers, the Microsoft C# compiler does not issue compilation errors for unknown `#pragma` directives; such directives do however generate warnings.</span></span>

#### <a name="pragma-warning"></a><span data-ttu-id="978b7-414">pragma 警告</span><span class="sxs-lookup"><span data-stu-id="978b7-414">Pragma warning</span></span>

<span data-ttu-id="978b7-415">`#pragma warning`指示詞用來在編譯後續的程式文字時，停用或還原所有或一組特定的警告訊息。</span><span class="sxs-lookup"><span data-stu-id="978b7-415">The `#pragma warning` directive is used to disable or restore all or a particular set of warning messages during compilation of the subsequent program text.</span></span>

```antlr
pragma_warning_body
    : 'warning' whitespace warning_action
    | 'warning' whitespace warning_action whitespace warning_list
    ;

warning_action
    : 'disable'
    | 'restore'
    ;

warning_list
    : decimal_digit+ (whitespace? ',' whitespace? decimal_digit+)*
    ;
```

<span data-ttu-id="978b7-416">`#pragma warning`省略警告清單的指示詞會影響所有的警告。</span><span class="sxs-lookup"><span data-stu-id="978b7-416">A `#pragma warning` directive that omits the warning list affects all warnings.</span></span> <span data-ttu-id="978b7-417">`#pragma warning`包含警告清單的指示詞只會影響清單中所指定的警告。</span><span class="sxs-lookup"><span data-stu-id="978b7-417">A `#pragma warning` directive that includes a warning list affects only those warnings that are specified in the list.</span></span>

<span data-ttu-id="978b7-418">指示`#pragma warning disable`詞會停用所有或指定的一組警告。</span><span class="sxs-lookup"><span data-stu-id="978b7-418">A `#pragma warning disable` directive disables all or the given set of warnings.</span></span>

<span data-ttu-id="978b7-419">`#pragma warning restore`指示詞會將所有或指定的警告集還原到編譯單位開始時生效的狀態。</span><span class="sxs-lookup"><span data-stu-id="978b7-419">A `#pragma warning restore` directive restores all or the given set of warnings to the state that was in effect at the beginning of the compilation unit.</span></span> <span data-ttu-id="978b7-420">請注意，如果已在外部停用特定警告`#pragma warning restore` ，則（不論是所有或特定的警告）都不會重新啟用該警告。</span><span class="sxs-lookup"><span data-stu-id="978b7-420">Note that if a particular warning was disabled externally, a `#pragma warning restore` (whether for all or the specific warning) will not re-enable that warning.</span></span>

<span data-ttu-id="978b7-421">下列範例顯示使用 Microsoft `#pragma warning` C#編譯器中的警告編號，在參考過時的成員時，暫時停用所報告的警告。</span><span class="sxs-lookup"><span data-stu-id="978b7-421">The following example shows use of `#pragma warning` to temporarily disable the warning reported when obsoleted members are referenced, using the warning number from the Microsoft C# compiler.</span></span>
```csharp
using System;

class Program
{
    [Obsolete]
    static void Foo() {}

    static void Main() {
#pragma warning disable 612
    Foo();
#pragma warning restore 612
    }
}
```
