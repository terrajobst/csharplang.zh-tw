# <a name="lexical-structure"></a><span data-ttu-id="69a47-101">語彙結構</span><span class="sxs-lookup"><span data-stu-id="69a47-101">Lexical structure</span></span>

## <a name="programs"></a><span data-ttu-id="69a47-102">Programs</span><span class="sxs-lookup"><span data-stu-id="69a47-102">Programs</span></span>

<span data-ttu-id="69a47-103">C#***計劃***包含一個或多個***原始程式檔***，稱為正式***編譯單位***([編譯單位](namespaces.md#compilation-units))。</span><span class="sxs-lookup"><span data-stu-id="69a47-103">A C# ***program*** consists of one or more ***source files***, known formally as ***compilation units*** ([Compilation units](namespaces.md#compilation-units)).</span></span> <span data-ttu-id="69a47-104">原始程式檔是 Unicode 字元的排序的順序。</span><span class="sxs-lookup"><span data-stu-id="69a47-104">A source file is an ordered sequence of Unicode characters.</span></span> <span data-ttu-id="69a47-105">通常在檔案系統中，有一對一的對應檔案的原始程式檔，但不需要這種對應關係。</span><span class="sxs-lookup"><span data-stu-id="69a47-105">Source files typically have a one-to-one correspondence with files in a file system, but this correspondence is not required.</span></span> <span data-ttu-id="69a47-106">最大的可攜性，建議在檔案系統中的檔案會使用 utf-8 編碼的編碼方式。</span><span class="sxs-lookup"><span data-stu-id="69a47-106">For maximal portability, it is recommended that files in a file system be encoded with the UTF-8 encoding.</span></span>

<span data-ttu-id="69a47-107">在概念上來說，是在編譯程式使用三個步驟：</span><span class="sxs-lookup"><span data-stu-id="69a47-107">Conceptually speaking, a program is compiled using three steps:</span></span>

1. <span data-ttu-id="69a47-108">轉換，將檔案從特定的字元項目和編碼配置轉換成 Unicode 字元序列。</span><span class="sxs-lookup"><span data-stu-id="69a47-108">Transformation, which converts a file from a particular character repertoire and encoding scheme into a sequence of Unicode characters.</span></span>
2. <span data-ttu-id="69a47-109">語彙分析，轉譯 token 的資料流中的 Unicode 輸入的字元資料流。</span><span class="sxs-lookup"><span data-stu-id="69a47-109">Lexical analysis, which translates a stream of Unicode input characters into a stream of tokens.</span></span>
3. <span data-ttu-id="69a47-110">語法分析，轉譯可執行程式碼中的語彙基元資料流。</span><span class="sxs-lookup"><span data-stu-id="69a47-110">Syntactic analysis, which translates the stream of tokens into executable code.</span></span>

## <a name="grammars"></a><span data-ttu-id="69a47-111">文法</span><span class="sxs-lookup"><span data-stu-id="69a47-111">Grammars</span></span>

<span data-ttu-id="69a47-112">此規格會顯示 C# 程式設計語言使用兩個文法的語法。</span><span class="sxs-lookup"><span data-stu-id="69a47-112">This specification presents the syntax of the C# programming language using two grammars.</span></span> <span data-ttu-id="69a47-113">***語彙文法***([語彙文法](lexical-structure.md#lexical-grammar)) 會定義如何將 Unicode 字元結合以表單行終端符號、 空白字元、 註解、 權杖和前置處理指示詞。</span><span class="sxs-lookup"><span data-stu-id="69a47-113">The ***lexical grammar*** ([Lexical grammar](lexical-structure.md#lexical-grammar)) defines how Unicode characters are combined to form line terminators, white space, comments, tokens, and pre-processing directives.</span></span> <span data-ttu-id="69a47-114">***語法的文法***([語法的文法](lexical-structure.md#syntactic-grammar)) 定義所產生的語彙文法的語彙基元組合表單 C# 程式的方式。</span><span class="sxs-lookup"><span data-stu-id="69a47-114">The ***syntactic grammar*** ([Syntactic grammar](lexical-structure.md#syntactic-grammar)) defines how the tokens resulting from the lexical grammar are combined to form C# programs.</span></span>

### <a name="grammar-notation"></a><span data-ttu-id="69a47-115">文法標記法</span><span class="sxs-lookup"><span data-stu-id="69a47-115">Grammar notation</span></span>

<span data-ttu-id="69a47-116">語彙和語法的文法是以使用 ANTLR 文法檢查工具的標記法的巴克斯格式顯示。</span><span class="sxs-lookup"><span data-stu-id="69a47-116">The lexical and syntactic grammars are presented in Backus-Naur form using the notation of the ANTLR grammar tool.</span></span>

### <a name="lexical-grammar"></a><span data-ttu-id="69a47-117">語彙文法</span><span class="sxs-lookup"><span data-stu-id="69a47-117">Lexical grammar</span></span>

<span data-ttu-id="69a47-118">語彙文法的 C# 所示[語彙分析](lexical-structure.md#lexical-analysis)，[語彙基元](lexical-structure.md#tokens)，並[前置處理指示詞](lexical-structure.md#pre-processing-directives)。</span><span class="sxs-lookup"><span data-stu-id="69a47-118">The lexical grammar of C# is presented in [Lexical analysis](lexical-structure.md#lexical-analysis), [Tokens](lexical-structure.md#tokens), and [Pre-processing directives](lexical-structure.md#pre-processing-directives).</span></span> <span data-ttu-id="69a47-119">語彙文法終端符號 Unicode 字元集的字元，而且僅語彙文法可讓您指定的字元組合表單語彙基元的方式 ([語彙基元](lexical-structure.md#tokens))，泛空白字元 ([泛空白字元](lexical-structure.md#white-space))，註解 ([註解](lexical-structure.md#comments))，和前置處理指示詞 ([前置處理指示詞](lexical-structure.md#pre-processing-directives))。</span><span class="sxs-lookup"><span data-stu-id="69a47-119">The terminal symbols of the lexical grammar are the characters of the Unicode character set, and the lexical grammar specifies how characters are combined to form tokens ([Tokens](lexical-structure.md#tokens)), white space ([White space](lexical-structure.md#white-space)), comments ([Comments](lexical-structure.md#comments)), and pre-processing directives ([Pre-processing directives](lexical-structure.md#pre-processing-directives)).</span></span>

<span data-ttu-id="69a47-120">在 C# 程式中的每個原始程式檔必須符合*輸入*生產語彙文法 ([語彙分析](lexical-structure.md#lexical-analysis))。</span><span class="sxs-lookup"><span data-stu-id="69a47-120">Every source file in a C# program must conform to the *input* production of the lexical grammar ([Lexical analysis](lexical-structure.md#lexical-analysis)).</span></span>

### <a name="syntactic-grammar"></a><span data-ttu-id="69a47-121">語法的文法</span><span class="sxs-lookup"><span data-stu-id="69a47-121">Syntactic grammar</span></span>

<span data-ttu-id="69a47-122">C# 的語法文法所示的章節和附錄，依循本章內容而定。</span><span class="sxs-lookup"><span data-stu-id="69a47-122">The syntactic grammar of C# is presented in the chapters and appendices that follow this chapter.</span></span> <span data-ttu-id="69a47-123">語法的文法終端符號是由語彙文法中，定義語彙基元和語法的文法可讓您指定的語彙基元組合表單 C# 程式的方式。</span><span class="sxs-lookup"><span data-stu-id="69a47-123">The terminal symbols of the syntactic grammar are the tokens defined by the lexical grammar, and the syntactic grammar specifies how tokens are combined to form C# programs.</span></span>

<span data-ttu-id="69a47-124">在 C# 程式中的每個原始程式檔必須符合*compilation_unit*生產的語法文法 ([編譯單位](namespaces.md#compilation-units))。</span><span class="sxs-lookup"><span data-stu-id="69a47-124">Every source file in a C# program must conform to the *compilation_unit* production of the syntactic grammar ([Compilation units](namespaces.md#compilation-units)).</span></span>

## <a name="lexical-analysis"></a><span data-ttu-id="69a47-125">語彙分析</span><span class="sxs-lookup"><span data-stu-id="69a47-125">Lexical analysis</span></span>

<span data-ttu-id="69a47-126">*輸入*生產定義 C# 原始程式檔的語彙結構。</span><span class="sxs-lookup"><span data-stu-id="69a47-126">The *input* production defines the lexical structure of a C# source file.</span></span> <span data-ttu-id="69a47-127">在 C# 程式中的每個原始程式檔必須符合此語彙文法生產環境。</span><span class="sxs-lookup"><span data-stu-id="69a47-127">Each source file in a C# program must conform to this lexical grammar production.</span></span>

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

<span data-ttu-id="69a47-128">五個基本項目組成 C# 原始程式檔的語彙結構： 行結束字元 ([行結束字元](lexical-structure.md#line-terminators))，泛空白字元 ([泛空白字元](lexical-structure.md#white-space))，註解 ([註解](lexical-structure.md#comments))，權杖 ([語彙基元](lexical-structure.md#tokens))，和前置處理指示詞 ([前置處理指示詞](lexical-structure.md#pre-processing-directives))。</span><span class="sxs-lookup"><span data-stu-id="69a47-128">Five basic elements make up the lexical structure of a C# source file: Line terminators ([Line terminators](lexical-structure.md#line-terminators)), white space ([White space](lexical-structure.md#white-space)), comments ([Comments](lexical-structure.md#comments)), tokens ([Tokens](lexical-structure.md#tokens)), and pre-processing directives ([Pre-processing directives](lexical-structure.md#pre-processing-directives)).</span></span> <span data-ttu-id="69a47-129">這些基本項目，只有語彙基元是 C# 程式的句法的文法中 ([語法的文法](lexical-structure.md#syntactic-grammar))。</span><span class="sxs-lookup"><span data-stu-id="69a47-129">Of these basic elements, only tokens are significant in the syntactic grammar of a C# program ([Syntactic grammar](lexical-structure.md#syntactic-grammar)).</span></span>

<span data-ttu-id="69a47-130">C# 原始程式檔語彙處理包含將檔案縮減成一連串的語彙基元會變得語法分析的輸入。</span><span class="sxs-lookup"><span data-stu-id="69a47-130">The lexical processing of a C# source file consists of reducing the file into a sequence of tokens which becomes the input to the syntactic analysis.</span></span> <span data-ttu-id="69a47-131">行終端符號、 空白字元和註解，可以當做分隔符號的語彙基元，但前置處理指示詞可能會導致略過，原始程式檔的區段，否則這些語彙項目語法的 C# 程式結構中沒有任何影響。</span><span class="sxs-lookup"><span data-stu-id="69a47-131">Line terminators, white space, and comments can serve to separate tokens, and pre-processing directives can cause sections of the source file to be skipped, but otherwise these lexical elements have no impact on the syntactic structure of a C# program.</span></span>

<span data-ttu-id="69a47-132">在字串插值常值的情況下 ([插補字串常值](lexical-structure.md#interpolated-string-literals)) 單一語彙基元一開始所產生的語彙分析，但是會分成數個輸入項目重複受制於語彙分析直到所有的內插的字串常值已獲得解決。</span><span class="sxs-lookup"><span data-stu-id="69a47-132">In the case of interpolated string literals ([Interpolated string literals](lexical-structure.md#interpolated-string-literals)) a single token is initially produced by lexical analysis, but is broken up into several input elements which are repeatedly subjected to lexical analysis until all interpolated string literals have been resolved.</span></span> <span data-ttu-id="69a47-133">所產生的權杖則做為輸入語法的分析。</span><span class="sxs-lookup"><span data-stu-id="69a47-133">The resulting tokens then serve as input to the syntactic analysis.</span></span>

<span data-ttu-id="69a47-134">當數個語彙文法解析符合的原始程式檔中的字元序列時，語彙處理永遠會形成最長的可能語彙項目。</span><span class="sxs-lookup"><span data-stu-id="69a47-134">When several lexical grammar productions match a sequence of characters in a source file, the lexical processing always forms the longest possible lexical element.</span></span> <span data-ttu-id="69a47-135">例如，字元序列`//`您的單行註解的開頭為處理，因為該語彙項目已超過單一`/`語彙基元。</span><span class="sxs-lookup"><span data-stu-id="69a47-135">For example, the character sequence `//` is processed as the beginning of a single-line comment because that lexical element is longer than a single `/` token.</span></span>

### <a name="line-terminators"></a><span data-ttu-id="69a47-136">行終端符號</span><span class="sxs-lookup"><span data-stu-id="69a47-136">Line terminators</span></span>

<span data-ttu-id="69a47-137">行結束字元會分成幾行中的 C# 原始程式檔中的字元。</span><span class="sxs-lookup"><span data-stu-id="69a47-137">Line terminators divide the characters of a C# source file into lines.</span></span>

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

<span data-ttu-id="69a47-138">來源與相容性程式碼編輯新增的檔案結尾標記的工具，並讓原始程式檔案，以適當地檢視以一連串的結束行，下列轉換會套用，才能在 C# 程式中的每個原始程式檔：</span><span class="sxs-lookup"><span data-stu-id="69a47-138">For compatibility with source code editing tools that add end-of-file markers, and to enable a source file to be viewed as a sequence of properly terminated lines, the following transformations are applied, in order, to every source file in a C# program:</span></span>

*  <span data-ttu-id="69a47-139">如果原始程式檔的最後一個字元是控制 Z 字元 (`U+001A`)，在刪除此字元。</span><span class="sxs-lookup"><span data-stu-id="69a47-139">If the last character of the source file is a Control-Z character (`U+001A`), this character is deleted.</span></span>
*  <span data-ttu-id="69a47-140">歸位字元 (`U+000D`) 加入至原始程式檔的結尾該原始程式檔是否為非空白，如果原始程式檔的最後一個字元不是歸位字元 (`U+000D`)、 換行字元 (`U+000A`)，行分隔符號 (`U+2028`)，或段落分隔符號 (`U+2029`)。</span><span class="sxs-lookup"><span data-stu-id="69a47-140">A carriage-return character (`U+000D`) is added to the end of the source file if that source file is non-empty and if the last character of the source file is not a carriage return (`U+000D`), a line feed (`U+000A`), a line separator (`U+2028`), or a paragraph separator (`U+2029`).</span></span>

### <a name="comments"></a><span data-ttu-id="69a47-141">註解</span><span class="sxs-lookup"><span data-stu-id="69a47-141">Comments</span></span>

<span data-ttu-id="69a47-142">支援兩種形式的註解： 單行註解和分隔註解。</span><span class="sxs-lookup"><span data-stu-id="69a47-142">Two forms of comments are supported: single-line comments and delimited comments.</span></span> <span data-ttu-id="69a47-143">***單行註解***的開頭字元`//`並擴充至原始程式檔的結尾。</span><span class="sxs-lookup"><span data-stu-id="69a47-143">***Single-line comments*** start with the characters `//` and extend to the end of the source line.</span></span> <span data-ttu-id="69a47-144">***分隔註解***的開頭字元`/*`和以字元結束`*/`。</span><span class="sxs-lookup"><span data-stu-id="69a47-144">***Delimited comments*** start with the characters `/*` and end with the characters `*/`.</span></span> <span data-ttu-id="69a47-145">分隔註解可能會跨越多行。</span><span class="sxs-lookup"><span data-stu-id="69a47-145">Delimited comments may span multiple lines.</span></span>

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
    : '/*' delimited_comment_section* asterisk* '/'
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

<span data-ttu-id="69a47-146">註解不進行巢狀處理。</span><span class="sxs-lookup"><span data-stu-id="69a47-146">Comments do not nest.</span></span> <span data-ttu-id="69a47-147">字元序列`/*`並`*/`內沒有特殊意義`//`註解和字元序列`//`和`/*`分隔註解中沒有特殊意義。</span><span class="sxs-lookup"><span data-stu-id="69a47-147">The character sequences `/*` and `*/` have no special meaning within a `//` comment, and the character sequences `//` and `/*` have no special meaning within a delimited comment.</span></span>

<span data-ttu-id="69a47-148">註解不會處理內字元和字串常值。</span><span class="sxs-lookup"><span data-stu-id="69a47-148">Comments are not processed within character and string literals.</span></span>

<span data-ttu-id="69a47-149">此範例</span><span class="sxs-lookup"><span data-stu-id="69a47-149">The example</span></span>
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
<span data-ttu-id="69a47-150">包含分隔註解。</span><span class="sxs-lookup"><span data-stu-id="69a47-150">includes a delimited comment.</span></span>

<span data-ttu-id="69a47-151">此範例</span><span class="sxs-lookup"><span data-stu-id="69a47-151">The example</span></span>
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
<span data-ttu-id="69a47-152">顯示數個單行註解。</span><span class="sxs-lookup"><span data-stu-id="69a47-152">shows several single-line comments.</span></span>

### <a name="white-space"></a><span data-ttu-id="69a47-153">空白字元</span><span class="sxs-lookup"><span data-stu-id="69a47-153">White space</span></span>

<span data-ttu-id="69a47-154">泛空白字元被定義為包含 Unicode 類別 Zs （其中包括空白字元） 的任何字元，以及水平定位字元、 垂直定位字元和表單換頁字元。</span><span class="sxs-lookup"><span data-stu-id="69a47-154">White space is defined as any character with Unicode class Zs (which includes the space character) as well as the horizontal tab character, the vertical tab character, and the form feed character.</span></span>

```antlr
whitespace
    : '<Any character with Unicode class Zs>'
    | '<Horizontal tab character (U+0009)>'
    | '<Vertical tab character (U+000B)>'
    | '<Form feed character (U+000C)>'
    ;
```

## <a name="tokens"></a><span data-ttu-id="69a47-155">語彙基元</span><span class="sxs-lookup"><span data-stu-id="69a47-155">Tokens</span></span>

<span data-ttu-id="69a47-156">有數種類型的語彙基元： 識別項、 關鍵字、 常值、 運算子和標點符號。</span><span class="sxs-lookup"><span data-stu-id="69a47-156">There are several kinds of tokens: identifiers, keywords, literals, operators, and punctuators.</span></span> <span data-ttu-id="69a47-157">泛空白字元和註解不屬於語彙基元，但它們可做為語彙基元的分隔符號。</span><span class="sxs-lookup"><span data-stu-id="69a47-157">White space and comments are not tokens, though they act as separators for tokens.</span></span>

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

### <a name="unicode-character-escape-sequences"></a><span data-ttu-id="69a47-158">Unicode 字元的逸出序列</span><span class="sxs-lookup"><span data-stu-id="69a47-158">Unicode character escape sequences</span></span>

<span data-ttu-id="69a47-159">Unicode 字元的逸出序列代表 Unicode 字元。</span><span class="sxs-lookup"><span data-stu-id="69a47-159">A Unicode character escape sequence represents a Unicode character.</span></span> <span data-ttu-id="69a47-160">Unicode 字元的逸出序列會處理在識別項 ([識別碼](lexical-structure.md#identifiers))，字元常值 ([字元常值](lexical-structure.md#character-literals))，和規則字串常值 ([字串常值](lexical-structure.md#string-literals)).</span><span class="sxs-lookup"><span data-stu-id="69a47-160">Unicode character escape sequences are processed in identifiers ([Identifiers](lexical-structure.md#identifiers)), character literals ([Character literals](lexical-structure.md#character-literals)), and regular string literals ([String literals](lexical-structure.md#string-literals)).</span></span> <span data-ttu-id="69a47-161">Unicode 逸出字元不會處理任何其他位置 （例如，以形成運算子、 標點符號或關鍵字）。</span><span class="sxs-lookup"><span data-stu-id="69a47-161">A Unicode character escape is not processed in any other location (for example, to form an operator, punctuator, or keyword).</span></span>

```antlr
unicode_escape_sequence
    : '\\u' hex_digit hex_digit hex_digit hex_digit
    | '\\U' hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit
    ;
```

<span data-ttu-id="69a47-162">Unicode 逸出序列表示單一 Unicode 字元後面的十六進位數字的格式 」`\u`「 或 」`\U`"字元。</span><span class="sxs-lookup"><span data-stu-id="69a47-162">A Unicode escape sequence represents the single Unicode character formed by the hexadecimal number following the "`\u`" or "`\U`" characters.</span></span> <span data-ttu-id="69a47-163">因為 C# 使用中的字元和字串值的 Unicode 字碼指標的 16 位元編碼方式，Unicode 字元 U + 10ffff，且範圍 u+10000 中的不允許的字元常值，並表示常值字串中使用 Unicode surrogate 字組。</span><span class="sxs-lookup"><span data-stu-id="69a47-163">Since C# uses a 16-bit encoding of Unicode code points in characters and string values, a Unicode character in the range U+10000 to U+10FFFF is not permitted in a character literal and is represented using a Unicode surrogate pair in a string literal.</span></span> <span data-ttu-id="69a47-164">不支援上述 0x10FFFF 字碼指標的 Unicode 字元。</span><span class="sxs-lookup"><span data-stu-id="69a47-164">Unicode characters with code points above 0x10FFFF are not supported.</span></span>

<span data-ttu-id="69a47-165">不會執行多個翻譯。</span><span class="sxs-lookup"><span data-stu-id="69a47-165">Multiple translations are not performed.</span></span> <span data-ttu-id="69a47-166">比方說，字串常值 」`\u005Cu005C`"就相當於"`\u005C`"而非"`\`」。</span><span class="sxs-lookup"><span data-stu-id="69a47-166">For instance, the string literal "`\u005Cu005C`" is equivalent to "`\u005C`" rather than "`\`".</span></span> <span data-ttu-id="69a47-167">Unicode 值`\u005C`是字元"`\`」。</span><span class="sxs-lookup"><span data-stu-id="69a47-167">The Unicode value `\u005C` is the character "`\`".</span></span>

<span data-ttu-id="69a47-168">此範例</span><span class="sxs-lookup"><span data-stu-id="69a47-168">The example</span></span>
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
<span data-ttu-id="69a47-169">顯示了多個`\u0066`，也就是逸出序列，以找到字母"`f`」。</span><span class="sxs-lookup"><span data-stu-id="69a47-169">shows several uses of `\u0066`, which is the escape sequence for the letter "`f`".</span></span> <span data-ttu-id="69a47-170">程式相當於</span><span class="sxs-lookup"><span data-stu-id="69a47-170">The program is equivalent to</span></span>
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

### <a name="identifiers"></a><span data-ttu-id="69a47-171">識別項</span><span class="sxs-lookup"><span data-stu-id="69a47-171">Identifiers</span></span>

<span data-ttu-id="69a47-172">本節中提供的識別碼的規則，確實對應到這些建議由 Unicode 標準附錄 31，不同之處在於底線字元可以是初始 （通常是傳統的 C 程式設計語言），Unicode 逸出序列允許在識別項，而"`@`」 字元做為前置詞允許啟用關鍵字當做識別項。</span><span class="sxs-lookup"><span data-stu-id="69a47-172">The rules for identifiers given in this section correspond exactly to those recommended by the Unicode Standard Annex 31, except that underscore is allowed as an initial character (as is traditional in the C programming language), Unicode escape sequences are permitted in identifiers, and the "`@`" character is allowed as a prefix to enable keywords to be used as identifiers.</span></span>

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

<span data-ttu-id="69a47-173">如先前所述的 Unicode 字元類別的相關資訊，請參閱 Unicode Standard 3.0 版，區段 4.5。</span><span class="sxs-lookup"><span data-stu-id="69a47-173">For information on the Unicode character classes mentioned above, see The Unicode Standard, Version 3.0, section 4.5.</span></span>

<span data-ttu-id="69a47-174">有效識別項的範例包括 「`identifier1`"，"`_identifier2`"，和 「`@if`"。</span><span class="sxs-lookup"><span data-stu-id="69a47-174">Examples of valid identifiers include "`identifier1`", "`_identifier2`", and "`@if`".</span></span>

<span data-ttu-id="69a47-175">合格的程式中的識別項必須是 Unicode 正規化表單 C 所定義的標準格式，所定義的 Unicode 標準附錄 15。</span><span class="sxs-lookup"><span data-stu-id="69a47-175">An identifier in a conforming program must be in the canonical format defined by Unicode Normalization Form C, as defined by Unicode Standard Annex 15.</span></span> <span data-ttu-id="69a47-176">遇到不在正規化表單 C 識別項時的行為是由實作定義;不過，診斷不需要。</span><span class="sxs-lookup"><span data-stu-id="69a47-176">The behavior when encountering an identifier not in Normalization Form C is implementation-defined; however, a diagnostic is not required.</span></span>

<span data-ttu-id="69a47-177">前置詞"`@`」 可讓您使用的關鍵字當做識別項，這與其他程式設計語言互動時很有用。</span><span class="sxs-lookup"><span data-stu-id="69a47-177">The prefix "`@`" enables the use of keywords as identifiers, which is useful when interfacing with other programming languages.</span></span> <span data-ttu-id="69a47-178">字元`@`不是實際的一部分的識別碼，因此識別項可能在其他語言中視為一般的識別碼，沒有前置詞。</span><span class="sxs-lookup"><span data-stu-id="69a47-178">The character `@` is not actually part of the identifier, so the identifier might be seen in other languages as a normal identifier, without the prefix.</span></span> <span data-ttu-id="69a47-179">使用識別項`@`稱為前置詞***逐字識別項***。</span><span class="sxs-lookup"><span data-stu-id="69a47-179">An identifier with an `@` prefix is called a ***verbatim identifier***.</span></span> <span data-ttu-id="69a47-180">使用`@`允許，但強烈不建議的樣式的識別項不是關鍵字的前置詞。</span><span class="sxs-lookup"><span data-stu-id="69a47-180">Use of the `@` prefix for identifiers that are not keywords is permitted, but strongly discouraged as a matter of style.</span></span>

<span data-ttu-id="69a47-181">範例：</span><span class="sxs-lookup"><span data-stu-id="69a47-181">The example:</span></span>
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
<span data-ttu-id="69a47-182">定義類別，名為"`class`「 使用靜態方法，名為"`static`"會接受名為"`bool`」。</span><span class="sxs-lookup"><span data-stu-id="69a47-182">defines a class named "`class`" with a static method named "`static`" that takes a parameter named "`bool`".</span></span> <span data-ttu-id="69a47-183">請注意，因為 Unicode 逸出中不允許使用關鍵字，語彙基元"`cl\u0061ss`」 是一個識別項，而且相同的識別碼 」`@class`"。</span><span class="sxs-lookup"><span data-stu-id="69a47-183">Note that since Unicode escapes are not permitted in keywords, the token "`cl\u0061ss`" is an identifier, and is the same identifier as "`@class`".</span></span>

<span data-ttu-id="69a47-184">兩個識別項都會視為相同，如果套用下列轉換，順序之後，它們都一樣：</span><span class="sxs-lookup"><span data-stu-id="69a47-184">Two identifiers are considered the same if they are identical after the following transformations are applied, in order:</span></span>

*  <span data-ttu-id="69a47-185">前置詞"`@`」，如果使用，會移除。</span><span class="sxs-lookup"><span data-stu-id="69a47-185">The prefix "`@`", if used, is removed.</span></span>
*  <span data-ttu-id="69a47-186">每個*unicode_escape_sequence*轉換成其對應的 Unicode 字元。</span><span class="sxs-lookup"><span data-stu-id="69a47-186">Each *unicode_escape_sequence* is transformed into its corresponding Unicode character.</span></span>
*  <span data-ttu-id="69a47-187">任何*formatting_character*已移除。</span><span class="sxs-lookup"><span data-stu-id="69a47-187">Any *formatting_character*s are removed.</span></span>

<span data-ttu-id="69a47-188">識別碼包含兩個連續底線字元 (`U+005F`) 已保留供實作。</span><span class="sxs-lookup"><span data-stu-id="69a47-188">Identifiers containing two consecutive underscore characters (`U+005F`) are reserved for use by the implementation.</span></span> <span data-ttu-id="69a47-189">比方說，實作可能會提供擴充的關鍵字開頭為兩個底線。</span><span class="sxs-lookup"><span data-stu-id="69a47-189">For example, an implementation might provide extended keywords that begin with two underscores.</span></span>

### <a name="keywords"></a><span data-ttu-id="69a47-190">關鍵字</span><span class="sxs-lookup"><span data-stu-id="69a47-190">Keywords</span></span>

<span data-ttu-id="69a47-191">A***關鍵字***是識別項類似的一連串字元已保留，且不能做為識別項，除了當前面加上`@`字元。</span><span class="sxs-lookup"><span data-stu-id="69a47-191">A ***keyword*** is an identifier-like sequence of characters that is reserved, and cannot be used as an identifier except when prefaced by the `@` character.</span></span>

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

<span data-ttu-id="69a47-192">在文法中某些地方，特定識別項具有特殊意義，，但不是關鍵字。</span><span class="sxs-lookup"><span data-stu-id="69a47-192">In some places in the grammar, specific identifiers have special meaning, but are not keywords.</span></span> <span data-ttu-id="69a47-193">這類識別項有時稱為 「 內容相關的關鍵字 」。</span><span class="sxs-lookup"><span data-stu-id="69a47-193">Such identifiers are sometimes referred to as "contextual keywords".</span></span> <span data-ttu-id="69a47-194">比方說，在屬性宣告中，「`get`"和"`set`」 的識別項具有特殊意義 ([存取子](classes.md#accessors))。</span><span class="sxs-lookup"><span data-stu-id="69a47-194">For example, within a property declaration, the "`get`" and "`set`" identifiers have special meaning ([Accessors](classes.md#accessors)).</span></span> <span data-ttu-id="69a47-195">以外的識別項`get`或`set`永遠不會用在這些位置中，所以這種使用不會使用這些字做為識別碼衝突。</span><span class="sxs-lookup"><span data-stu-id="69a47-195">An identifier other than `get` or `set` is never permitted in these locations, so this use does not conflict with a use of these words as identifiers.</span></span> <span data-ttu-id="69a47-196">在其他情況下，如在具有識別項 」`var`「 隱含類型區域變數宣告中 ([區域變數宣告](statements.md#local-variable-declarations))，宣告名稱發生衝突的內容關鍵字。</span><span class="sxs-lookup"><span data-stu-id="69a47-196">In other cases, such as with the identifier "`var`" in implicitly typed local variable declarations ([Local variable declarations](statements.md#local-variable-declarations)), a contextual keyword can conflict with declared names.</span></span> <span data-ttu-id="69a47-197">在此情況下，宣告的名稱會將優先順序高於識別碼作為內容的關鍵字。</span><span class="sxs-lookup"><span data-stu-id="69a47-197">In such cases, the declared name takes precedence over the use of the identifier as a contextual keyword.</span></span>

### <a name="literals"></a><span data-ttu-id="69a47-198">常值</span><span class="sxs-lookup"><span data-stu-id="69a47-198">Literals</span></span>

<span data-ttu-id="69a47-199">A***常值***是值的來源的程式碼表示法。</span><span class="sxs-lookup"><span data-stu-id="69a47-199">A ***literal*** is a source code representation of a value.</span></span>

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

#### <a name="boolean-literals"></a><span data-ttu-id="69a47-200">布林值常值</span><span class="sxs-lookup"><span data-stu-id="69a47-200">Boolean literals</span></span>

<span data-ttu-id="69a47-201">有兩個布林常值：`true`和`false`。</span><span class="sxs-lookup"><span data-stu-id="69a47-201">There are two boolean literal values: `true` and `false`.</span></span>

```antlr
boolean_literal
    : 'true'
    | 'false'
    ;
```

<span data-ttu-id="69a47-202">型別*boolean_literal*是`bool`。</span><span class="sxs-lookup"><span data-stu-id="69a47-202">The type of a *boolean_literal* is `bool`.</span></span>

#### <a name="integer-literals"></a><span data-ttu-id="69a47-203">整數常值</span><span class="sxs-lookup"><span data-stu-id="69a47-203">Integer literals</span></span>

<span data-ttu-id="69a47-204">整數常值用來寫入類型的值`int`， `uint`， `long`，和`ulong`。</span><span class="sxs-lookup"><span data-stu-id="69a47-204">Integer literals are used to write values of types `int`, `uint`, `long`, and `ulong`.</span></span> <span data-ttu-id="69a47-205">整數常值有兩個可能的表單： 十進位和十六進位。</span><span class="sxs-lookup"><span data-stu-id="69a47-205">Integer literals have two possible forms: decimal and hexadecimal.</span></span>

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

<span data-ttu-id="69a47-206">整數常值的類型決定，如下所示：</span><span class="sxs-lookup"><span data-stu-id="69a47-206">The type of an integer literal is determined as follows:</span></span>

*  <span data-ttu-id="69a47-207">如果常值沒有後置詞，它會有這些類型中可表示其值的第一個： `int`， `uint`， `long`， `ulong`。</span><span class="sxs-lookup"><span data-stu-id="69a47-207">If the literal has no suffix, it has the first of these types in which its value can be represented: `int`, `uint`, `long`, `ulong`.</span></span>
*  <span data-ttu-id="69a47-208">如果常值加`U`或是`u`，它有一種類型中可表示其值的第一個： `uint`， `ulong`。</span><span class="sxs-lookup"><span data-stu-id="69a47-208">If the literal is suffixed by `U` or `u`, it has the first of these types in which its value can be represented: `uint`, `ulong`.</span></span>
*  <span data-ttu-id="69a47-209">如果常值加`L`或是`l`，它有一種類型中可表示其值的第一個： `long`， `ulong`。</span><span class="sxs-lookup"><span data-stu-id="69a47-209">If the literal is suffixed by `L` or `l`, it has the first of these types in which its value can be represented: `long`, `ulong`.</span></span>
*  <span data-ttu-id="69a47-210">如果常值加`UL`， `Ul`， `uL`， `ul`， `LU`， `Lu`， `lU`，或`lu`，它是型別`ulong`。</span><span class="sxs-lookup"><span data-stu-id="69a47-210">If the literal is suffixed by `UL`, `Ul`, `uL`, `ul`, `LU`, `Lu`, `lU`, or `lu`, it is of type `ulong`.</span></span>

<span data-ttu-id="69a47-211">如果整數常值所代表的值超出範圍`ulong`輸入時，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="69a47-211">If the value represented by an integer literal is outside the range of the `ulong` type, a compile-time error occurs.</span></span>

<span data-ttu-id="69a47-212">為樣式的問題，建議您，「`L`「 使用而不是 」`l`"寫入類型的常值時`long`，因為很容易混淆字母"`l`"與數字 」`1`"。</span><span class="sxs-lookup"><span data-stu-id="69a47-212">As a matter of style, it is suggested that "`L`" be used instead of "`l`" when writing literals of type `long`, since it is easy to confuse the letter "`l`" with the digit "`1`".</span></span>

<span data-ttu-id="69a47-213">若要允許的最小可能`int`和`long`值寫入為十進位整數常值，下列兩個規則存在：</span><span class="sxs-lookup"><span data-stu-id="69a47-213">To permit the smallest possible `int` and `long` values to be written as decimal integer literals, the following two rules exist:</span></span>

* <span data-ttu-id="69a47-214">當*decimal_integer_literal* 2147483648 的值 (2 ^31) 和 no *integer_type_suffix*會顯示為一元減號運算子語彙基元的正後方的語彙基元 ([一元減號運算子](expressions.md#unary-minus-operator))，結果是類型的常數`int`值介於-2147483648 (-2 ^31)。</span><span class="sxs-lookup"><span data-stu-id="69a47-214">When a *decimal_integer_literal* with the value 2147483648 (2^31) and no *integer_type_suffix* appears as the token immediately following a unary minus operator token ([Unary minus operator](expressions.md#unary-minus-operator)), the result is a constant of type `int` with the value -2147483648 (-2^31).</span></span> <span data-ttu-id="69a47-215">在其他情況下，這類*decimal_integer_literal*別的`uint`。</span><span class="sxs-lookup"><span data-stu-id="69a47-215">In all other situations, such a *decimal_integer_literal* is of type `uint`.</span></span>
* <span data-ttu-id="69a47-216">當*decimal_integer_literal*是 9223372036854775808 (2 ^63) 和 no *integer_type_suffix*或*integer_type_suffix* `L`或`l`會顯示為一元減號運算子語彙基元的正後方的語彙基元 ([一元減號運算子](expressions.md#unary-minus-operator))，結果是類型的常數`long`值-9223372036854775808 (-2 ^63)。</span><span class="sxs-lookup"><span data-stu-id="69a47-216">When a *decimal_integer_literal* with the value 9223372036854775808 (2^63) and no *integer_type_suffix* or the *integer_type_suffix* `L` or `l` appears as the token immediately following a unary minus operator token ([Unary minus operator](expressions.md#unary-minus-operator)), the result is a constant of type `long` with the value -9223372036854775808 (-2^63).</span></span> <span data-ttu-id="69a47-217">在其他情況下，這類*decimal_integer_literal*別的`ulong`。</span><span class="sxs-lookup"><span data-stu-id="69a47-217">In all other situations, such a *decimal_integer_literal* is of type `ulong`.</span></span>

#### <a name="real-literals"></a><span data-ttu-id="69a47-218">實數常值</span><span class="sxs-lookup"><span data-stu-id="69a47-218">Real literals</span></span>

<span data-ttu-id="69a47-219">實數常值用來寫入類型的值`float`， `double`，和`decimal`。</span><span class="sxs-lookup"><span data-stu-id="69a47-219">Real literals are used to write values of types `float`, `double`, and `decimal`.</span></span>

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

<span data-ttu-id="69a47-220">如果沒有*real_type_suffix*指定，實數常值的型別是`double`。</span><span class="sxs-lookup"><span data-stu-id="69a47-220">If no *real_type_suffix* is specified, the type of the real literal is `double`.</span></span> <span data-ttu-id="69a47-221">否則為實數類型後置詞決定實數常值，型的別，如下所示：</span><span class="sxs-lookup"><span data-stu-id="69a47-221">Otherwise, the real type suffix determines the type of the real literal, as follows:</span></span>

*  <span data-ttu-id="69a47-222">實數常值加上`F`或是`f`別的`float`。</span><span class="sxs-lookup"><span data-stu-id="69a47-222">A real literal suffixed by `F` or `f` is of type `float`.</span></span> <span data-ttu-id="69a47-223">例如，常值`1f`， `1.5f`， `1e10f`，和`123.456F`類型的所有`float`。</span><span class="sxs-lookup"><span data-stu-id="69a47-223">For example, the literals `1f`, `1.5f`, `1e10f`, and `123.456F` are all of type `float`.</span></span>
*  <span data-ttu-id="69a47-224">實數常值加上`D`或是`d`別的`double`。</span><span class="sxs-lookup"><span data-stu-id="69a47-224">A real literal suffixed by `D` or `d` is of type `double`.</span></span> <span data-ttu-id="69a47-225">例如，常值`1d`， `1.5d`， `1e10d`，和`123.456D`類型的所有`double`。</span><span class="sxs-lookup"><span data-stu-id="69a47-225">For example, the literals `1d`, `1.5d`, `1e10d`, and `123.456D` are all of type `double`.</span></span>
*  <span data-ttu-id="69a47-226">實數常值加上`M`或是`m`別的`decimal`。</span><span class="sxs-lookup"><span data-stu-id="69a47-226">A real literal suffixed by `M` or `m` is of type `decimal`.</span></span> <span data-ttu-id="69a47-227">例如，常值`1m`， `1.5m`， `1e10m`，和`123.456M`類型的所有`decimal`。</span><span class="sxs-lookup"><span data-stu-id="69a47-227">For example, the literals `1m`, `1.5m`, `1e10m`, and `123.456M` are all of type `decimal`.</span></span> <span data-ttu-id="69a47-228">這個常值轉換成`decimal`值所採取的確切的值，如有必要，數值四捨五入成最接近的值，可顯示使用銀行進位 ([decimal 型別](types.md#the-decimal-type))。</span><span class="sxs-lookup"><span data-stu-id="69a47-228">This literal is converted to a `decimal` value by taking the exact value, and, if necessary, rounding to the nearest representable value using banker's rounding ([The decimal type](types.md#the-decimal-type)).</span></span> <span data-ttu-id="69a47-229">除非此值會無條件或值為零 （在此後者的情況下正負號和小數位數會是 0），則會保留任何明顯常值中的小數點位數。</span><span class="sxs-lookup"><span data-stu-id="69a47-229">Any scale apparent in the literal is preserved unless the value is rounded or the value is zero (in which latter case the sign and scale will be 0).</span></span> <span data-ttu-id="69a47-230">因此，常值`2.900m`將會剖析為使用正負號的十進位數值`0`，係數`2900`，和小數位數`3`。</span><span class="sxs-lookup"><span data-stu-id="69a47-230">Hence, the literal `2.900m` will be parsed to form the decimal with sign `0`, coefficient `2900`, and scale `3`.</span></span>

<span data-ttu-id="69a47-231">如果指定的常值無法表示成指定型別，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="69a47-231">If the specified literal cannot be represented in the indicated type, a compile-time error occurs.</span></span>

<span data-ttu-id="69a47-232">實數常值類型的值`float`或`double`取決於使用 IEEE"捨入為最接近 」 模式。</span><span class="sxs-lookup"><span data-stu-id="69a47-232">The value of a real literal of type `float` or `double` is determined by using the IEEE "round to nearest" mode.</span></span>

<span data-ttu-id="69a47-233">請注意，在實數常值、 十進位數字一向是必要的小數點後。</span><span class="sxs-lookup"><span data-stu-id="69a47-233">Note that in a real literal, decimal digits are always required after the decimal point.</span></span> <span data-ttu-id="69a47-234">例如，`1.3F`是實際常值，但`1.F`不是。</span><span class="sxs-lookup"><span data-stu-id="69a47-234">For example, `1.3F` is a real literal but `1.F` is not.</span></span>

#### <a name="character-literals"></a><span data-ttu-id="69a47-235">字元常值</span><span class="sxs-lookup"><span data-stu-id="69a47-235">Character literals</span></span>

<span data-ttu-id="69a47-236">字元常值代表單一字元，而且通常包含的字元以引號括住，如`'a'`。</span><span class="sxs-lookup"><span data-stu-id="69a47-236">A character literal represents a single character, and usually consists of a character in quotes, as in `'a'`.</span></span>

<span data-ttu-id="69a47-237">請注意： ANTLR 文法標記法會讓下列令人混淆 ！</span><span class="sxs-lookup"><span data-stu-id="69a47-237">Note: The ANTLR grammar notation makes the following confusing!</span></span> <span data-ttu-id="69a47-238">在 ANTLR，當您撰寫`\'`它代表單引號`'`。</span><span class="sxs-lookup"><span data-stu-id="69a47-238">In ANTLR, when you write `\'` it stands for a single quote `'`.</span></span> <span data-ttu-id="69a47-239">當您撰寫並`\\`它代表單一反斜線`\`。</span><span class="sxs-lookup"><span data-stu-id="69a47-239">And when you write `\\` it stands for a single backslash `\`.</span></span> <span data-ttu-id="69a47-240">因此它的開頭的單引號字元，然後單引號字元常值的第一個規則所表示。</span><span class="sxs-lookup"><span data-stu-id="69a47-240">Therefore the first rule for a character literal means it starts with a single quote, then a character, then a single quote.</span></span> <span data-ttu-id="69a47-241">和十一個可能的簡單逸出序列`\'`， `\"`， `\\`， `\0`， `\a`， `\b`， `\f`， `\n`， `\r`， `\t`， `\v`.</span><span class="sxs-lookup"><span data-stu-id="69a47-241">And the eleven possible simple escape sequences are `\'`, `\"`, `\\`, `\0`, `\a`, `\b`, `\f`, `\n`, `\r`, `\t`, `\v`.</span></span>

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

<span data-ttu-id="69a47-242">接下來的反斜線字元的字元 (`\`) 中*字元*必須是下列字元： `'`， `"`， `\`， `0`， `a`， `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span><span class="sxs-lookup"><span data-stu-id="69a47-242">A character that follows a backslash character (`\`) in a *character* must be one of the following characters: `'`, `"`, `\`, `0`, `a`, `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span></span> <span data-ttu-id="69a47-243">否則，會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="69a47-243">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="69a47-244">十六進位逸出序列表示單一 Unicode 字元，後面的十六進位數字的格式值"`\x`」。</span><span class="sxs-lookup"><span data-stu-id="69a47-244">A hexadecimal escape sequence represents a single Unicode character, with the value formed by the hexadecimal number following "`\x`".</span></span>

<span data-ttu-id="69a47-245">字元常值所代表的值是否大於`U+FFFF`，就會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="69a47-245">If the value represented by a character literal is greater than `U+FFFF`, a compile-time error occurs.</span></span>

<span data-ttu-id="69a47-246">Unicode 字元的逸出序列 ([Unicode 字元的逸出序列](lexical-structure.md#unicode-character-escape-sequences)) 中的字元常值必須在範圍內`U+0000`至`U+FFFF`。</span><span class="sxs-lookup"><span data-stu-id="69a47-246">A Unicode character escape sequence ([Unicode character escape sequences](lexical-structure.md#unicode-character-escape-sequences)) in a character literal must be in the range `U+0000` to `U+FFFF`.</span></span>

<span data-ttu-id="69a47-247">簡單逸出序列表示 Unicode 字元編碼方式 」 下, 表中所述。</span><span class="sxs-lookup"><span data-stu-id="69a47-247">A simple escape sequence represents a Unicode character encoding, as described in the table below.</span></span>


| <span data-ttu-id="69a47-248">__逸出序列__</span><span class="sxs-lookup"><span data-stu-id="69a47-248">__Escape sequence__</span></span> | <span data-ttu-id="69a47-249">__字元的名稱__</span><span class="sxs-lookup"><span data-stu-id="69a47-249">__Character name__</span></span> | <span data-ttu-id="69a47-250">__Unicode 編碼__</span><span class="sxs-lookup"><span data-stu-id="69a47-250">__Unicode encoding__</span></span> |
|---------------------|--------------------|----------------------|
| `\'`                | <span data-ttu-id="69a47-251">單引號</span><span class="sxs-lookup"><span data-stu-id="69a47-251">Single quote</span></span>       | `0x0027`             | 
| `\"`                | <span data-ttu-id="69a47-252">雙引號</span><span class="sxs-lookup"><span data-stu-id="69a47-252">Double quote</span></span>       | `0x0022`             | 
| `\\`                | <span data-ttu-id="69a47-253">反斜線</span><span class="sxs-lookup"><span data-stu-id="69a47-253">Backslash</span></span>          | `0x005C`             | 
| `\0`                | <span data-ttu-id="69a47-254">Null</span><span class="sxs-lookup"><span data-stu-id="69a47-254">Null</span></span>               | `0x0000`             | 
| `\a`                | <span data-ttu-id="69a47-255">警示</span><span class="sxs-lookup"><span data-stu-id="69a47-255">Alert</span></span>              | `0x0007`             | 
| `\b`                | <span data-ttu-id="69a47-256">退格鍵</span><span class="sxs-lookup"><span data-stu-id="69a47-256">Backspace</span></span>          | `0x0008`             | 
| `\f`                | <span data-ttu-id="69a47-257">換頁字元</span><span class="sxs-lookup"><span data-stu-id="69a47-257">Form feed</span></span>          | `0x000C`             | 
| `\n`                | <span data-ttu-id="69a47-258">換行</span><span class="sxs-lookup"><span data-stu-id="69a47-258">New line</span></span>           | `0x000A`             | 
| `\r`                | <span data-ttu-id="69a47-259">歸位字元</span><span class="sxs-lookup"><span data-stu-id="69a47-259">Carriage return</span></span>    | `0x000D`             | 
| `\t`                | <span data-ttu-id="69a47-260">水平 Tab</span><span class="sxs-lookup"><span data-stu-id="69a47-260">Horizontal tab</span></span>     | `0x0009`             | 
| `\v`                | <span data-ttu-id="69a47-261">垂直 Tab</span><span class="sxs-lookup"><span data-stu-id="69a47-261">Vertical tab</span></span>       | `0x000B`             | 

<span data-ttu-id="69a47-262">型別*character_literal*是`char`。</span><span class="sxs-lookup"><span data-stu-id="69a47-262">The type of a *character_literal* is `char`.</span></span>

#### <a name="string-literals"></a><span data-ttu-id="69a47-263">字串常值</span><span class="sxs-lookup"><span data-stu-id="69a47-263">String literals</span></span>

<span data-ttu-id="69a47-264">C# 支援兩種形式的字串常值：***規則字串常值***並***逐字字串常值***。</span><span class="sxs-lookup"><span data-stu-id="69a47-264">C# supports two forms of string literals: ***regular string literals*** and ***verbatim string literals***.</span></span>

<span data-ttu-id="69a47-265">規則字串常值包含零個或多個字元以住雙引號括住，如`"hello"`，而且可能包含兩個簡單的逸出序列 (例如`\t` 索引標籤上的字元)，和十六進位和 Unicode 逸出序列。</span><span class="sxs-lookup"><span data-stu-id="69a47-265">A regular string literal consists of zero or more characters enclosed in double quotes, as in `"hello"`, and may include both simple escape sequences (such as `\t` for the tab character), and hexadecimal and Unicode escape sequences.</span></span>

<span data-ttu-id="69a47-266">逐字字串常值組成`@`字元後面的雙引號字元、 零或多個字元，以及右雙引號字元。</span><span class="sxs-lookup"><span data-stu-id="69a47-266">A verbatim string literal consists of an `@` character followed by a double-quote character, zero or more characters, and a closing double-quote character.</span></span> <span data-ttu-id="69a47-267">是一個簡單的例子`@"hello"`。</span><span class="sxs-lookup"><span data-stu-id="69a47-267">A simple example is `@"hello"`.</span></span> <span data-ttu-id="69a47-268">逐字字串常值，在分隔符號之間的字元逐字解譯，唯一的例外狀況正在*quote_escape_sequence*。</span><span class="sxs-lookup"><span data-stu-id="69a47-268">In a verbatim string literal, the characters between the delimiters are interpreted verbatim, the only exception being a *quote_escape_sequence*.</span></span> <span data-ttu-id="69a47-269">特別是，不會處理簡單的逸出序列和十六進位和 Unicode 逸出序列加上逐字字串常值中。</span><span class="sxs-lookup"><span data-stu-id="69a47-269">In particular, simple escape sequences, and hexadecimal and Unicode escape sequences are not processed in verbatim string literals.</span></span> <span data-ttu-id="69a47-270">逐字字串常值可能會跨越多行。</span><span class="sxs-lookup"><span data-stu-id="69a47-270">A verbatim string literal may span multiple lines.</span></span>

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

<span data-ttu-id="69a47-271">接下來的反斜線字元的字元 (`\`) 中*regular_string_literal_character*必須是下列字元： `'`， `"`， `\`， `0`， `a`, `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span><span class="sxs-lookup"><span data-stu-id="69a47-271">A character that follows a backslash character (`\`) in a *regular_string_literal_character* must be one of the following characters: `'`, `"`, `\`, `0`, `a`, `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span></span> <span data-ttu-id="69a47-272">否則，會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="69a47-272">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="69a47-273">此範例</span><span class="sxs-lookup"><span data-stu-id="69a47-273">The example</span></span>
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
<span data-ttu-id="69a47-274">顯示各種不同的字串常值。</span><span class="sxs-lookup"><span data-stu-id="69a47-274">shows a variety of string literals.</span></span> <span data-ttu-id="69a47-275">最後一個的字串常值， `j`，為逐字字串常值跨越多行。</span><span class="sxs-lookup"><span data-stu-id="69a47-275">The last string literal, `j`, is a verbatim string literal that spans multiple lines.</span></span> <span data-ttu-id="69a47-276">加上引號，包括新行字元，例如泛空白字元的字元都會逐字。</span><span class="sxs-lookup"><span data-stu-id="69a47-276">The characters between the quotation marks, including white space such as new line characters, are preserved verbatim.</span></span>

<span data-ttu-id="69a47-277">由於十六進位逸出序列可能會有變動數目的十六進位數字、 字串常值`"\x123"`包含單一字元十六進位值為 123。</span><span class="sxs-lookup"><span data-stu-id="69a47-277">Since a hexadecimal escape sequence can have a variable number of hex digits, the string literal `"\x123"` contains a single character with hex value 123.</span></span> <span data-ttu-id="69a47-278">若要建立包含具有十六進位值為 12，後面接著字元 3 的字元的字串，其中可以撰寫`"\x00123"`或`"\x12" + "3"`改。</span><span class="sxs-lookup"><span data-stu-id="69a47-278">To create a string containing the character with hex value 12 followed by the character 3, one could write `"\x00123"` or `"\x12" + "3"` instead.</span></span>

<span data-ttu-id="69a47-279">型別*string_literal*是`string`。</span><span class="sxs-lookup"><span data-stu-id="69a47-279">The type of a *string_literal* is `string`.</span></span>

<span data-ttu-id="69a47-280">每個字串常值不會不一定會導致新的字串執行個體。</span><span class="sxs-lookup"><span data-stu-id="69a47-280">Each string literal does not necessarily result in a new string instance.</span></span> <span data-ttu-id="69a47-281">兩個或多個字串常值的相等時會根據字串等號比較運算子 ([字串等號比較運算子](expressions.md#string-equality-operators)) 會出現在相同的程式中，這些字串常值參考相同的字串執行個體。</span><span class="sxs-lookup"><span data-stu-id="69a47-281">When two or more string literals that are equivalent according to the string equality operator ([String equality operators](expressions.md#string-equality-operators)) appear in the same program, these string literals refer to the same string instance.</span></span> <span data-ttu-id="69a47-282">比方說，所產生的輸出</span><span class="sxs-lookup"><span data-stu-id="69a47-282">For instance, the output produced by</span></span>
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
<span data-ttu-id="69a47-283">是`True`因為兩個常值參考相同的字串執行個體。</span><span class="sxs-lookup"><span data-stu-id="69a47-283">is `True` because the two literals refer to the same string instance.</span></span>

#### <a name="interpolated-string-literals"></a><span data-ttu-id="69a47-284">字串插值常值</span><span class="sxs-lookup"><span data-stu-id="69a47-284">Interpolated string literals</span></span>

<span data-ttu-id="69a47-285">字串插值常值字串常值，類似，但包含分隔的漏洞`{`和`}`，其中運算式可能會發生。</span><span class="sxs-lookup"><span data-stu-id="69a47-285">Interpolated string literals are similar to string literals, but contain holes delimited by `{` and `}`, wherein expressions can occur.</span></span> <span data-ttu-id="69a47-286">在執行階段，會評估運算式，目的是讓其文字的形式，將替代至漏洞發生的所在位置的字串。</span><span class="sxs-lookup"><span data-stu-id="69a47-286">At runtime, the expressions are evaluated with the purpose of having their textual forms substituted into the string at the place where the hole occurs.</span></span> <span data-ttu-id="69a47-287">語法和語意的字串內插補點一節所述 ([插補字串](expressions.md#interpolated-strings))。</span><span class="sxs-lookup"><span data-stu-id="69a47-287">The syntax and semantics of string interpolation are described in section ([Interpolated strings](expressions.md#interpolated-strings)).</span></span>

<span data-ttu-id="69a47-288">字串常值，例如字串插值常值可以是一般或逐字規範。</span><span class="sxs-lookup"><span data-stu-id="69a47-288">Like string literals, interpolated string literals can be either regular or verbatim.</span></span> <span data-ttu-id="69a47-289">內插補點的規則字串常值以分隔`$"`並`"`，且由分隔逐字字串插值常值`$@"`和`"`。</span><span class="sxs-lookup"><span data-stu-id="69a47-289">Interpolated regular string literals are delimited by `$"` and `"`, and interpolated verbatim string literals are delimited by `$@"` and `"`.</span></span>

<span data-ttu-id="69a47-290">其他常值，例如插入的字串常值的語彙分析一開始會產生單一語彙基元，根據下列文法。</span><span class="sxs-lookup"><span data-stu-id="69a47-290">Like other literals, lexical analysis of an interpolated string literal initially results in a single token, as per the grammar below.</span></span> <span data-ttu-id="69a47-291">不過，語法的分析，插入的字串常值的單一權杖分為封閉式漏洞，字串的各部分的數個權杖前後所發生的漏洞之輸入項目的語彙再次分析。</span><span class="sxs-lookup"><span data-stu-id="69a47-291">However, before syntactic analysis, the single token of an interpolated string literal is broken into several tokens for the parts of the string enclosing the holes, and the input elements occurring in the holes are lexically analysed again.</span></span> <span data-ttu-id="69a47-292">接著，這可能會產生更插補的字串常值，以進行處理，但是，如果語彙修正，最後會導致一連串的語彙基元的語法分析來處理。</span><span class="sxs-lookup"><span data-stu-id="69a47-292">This may in turn produce more interpolated string literals to be processed, but, if lexically correct, will eventually lead to a sequence of tokens for syntactic analysis to process.</span></span>

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

<span data-ttu-id="69a47-293">*Interpolated_string_literal*語彙基元會解譯為多個權杖和其他輸入項目，如下所示，在訂單中項目的*interpolated_string_literal*:</span><span class="sxs-lookup"><span data-stu-id="69a47-293">An *interpolated_string_literal* token is reinterpreted as multiple tokens and other input elements as follows, in order of occurrence in the *interpolated_string_literal*:</span></span>

* <span data-ttu-id="69a47-294">下列項目會解譯為不同的個別語彙基元： 前置`$`號*interpolated_regular_string_whole*， *interpolated_regular_string_start*，*interpolated_regular_string_mid*， *interpolated_regular_string_end*， *interpolated_verbatim_string_whole*， *interpolated_verbatim_string_start*， *interpolated_verbatim_string_mid*並*interpolated_verbatim_string_end*。</span><span class="sxs-lookup"><span data-stu-id="69a47-294">Occurrences of the following are reinterpreted as separate individual tokens: the leading `$` sign, *interpolated_regular_string_whole*, *interpolated_regular_string_start*, *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_whole*, *interpolated_verbatim_string_start*, *interpolated_verbatim_string_mid* and *interpolated_verbatim_string_end*.</span></span>
* <span data-ttu-id="69a47-295">出現次數*regular_balanced_text*並*verbatim_balanced_text*兩者之間會重新處理成*input_section* ([語彙分析](lexical-structure.md#lexical-analysis)) 和會解譯為結果序列的輸入項目。</span><span class="sxs-lookup"><span data-stu-id="69a47-295">Occurrences of *regular_balanced_text* and *verbatim_balanced_text* between these are reprocessed as an *input_section* ([Lexical analysis](lexical-structure.md#lexical-analysis)) and are reinterpreted as the resulting sequence of input elements.</span></span> <span data-ttu-id="69a47-296">接著，這些可能包含重新解譯的字串插值常值語彙基元。</span><span class="sxs-lookup"><span data-stu-id="69a47-296">These may in turn include interpolated string literal tokens to be reinterpreted.</span></span>

<span data-ttu-id="69a47-297">語法分析將會重新合併到語彙基元*interpolated_string_expression* ([插補字串](expressions.md#interpolated-strings))。</span><span class="sxs-lookup"><span data-stu-id="69a47-297">Syntactic analysis will recombine the tokens into an *interpolated_string_expression* ([Interpolated strings](expressions.md#interpolated-strings)).</span></span>

<span data-ttu-id="69a47-298">範例待辦事項</span><span class="sxs-lookup"><span data-stu-id="69a47-298">Examples TODO</span></span>


#### <a name="the-null-literal"></a><span data-ttu-id="69a47-299">Null 常值</span><span class="sxs-lookup"><span data-stu-id="69a47-299">The null literal</span></span>

```antlr
null_literal
    : 'null'
    ;
```

<span data-ttu-id="69a47-300">*Null_literal*可以隱含地轉換為參考型別或可為 null 的型別。</span><span class="sxs-lookup"><span data-stu-id="69a47-300">The  *null_literal* can be implicitly converted to a reference type or nullable type.</span></span>

### <a name="operators-and-punctuators"></a><span data-ttu-id="69a47-301">運算子和標點符號</span><span class="sxs-lookup"><span data-stu-id="69a47-301">Operators and punctuators</span></span>

<span data-ttu-id="69a47-302">有數種運算子和標點符號。</span><span class="sxs-lookup"><span data-stu-id="69a47-302">There are several kinds of operators and punctuators.</span></span> <span data-ttu-id="69a47-303">運算式中使用運算子來說明涉及一或多個運算元的作業。</span><span class="sxs-lookup"><span data-stu-id="69a47-303">Operators are used in expressions to describe operations involving one or more operands.</span></span> <span data-ttu-id="69a47-304">例如，運算式`a + b`會使用`+`運算子，將兩個運算元`a`和`b`。</span><span class="sxs-lookup"><span data-stu-id="69a47-304">For example, the expression `a + b` uses the `+` operator to add the two operands `a` and `b`.</span></span> <span data-ttu-id="69a47-305">標點符號會分組和區隔。</span><span class="sxs-lookup"><span data-stu-id="69a47-305">Punctuators are for grouping and separating.</span></span>

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

<span data-ttu-id="69a47-306">中的直條*right_shift*並*right_shift_assignment*生產用來表示，不同於其他生產中的語法的文法，任何種類的任何字元 （甚至不空白字元） 允許語彙基元之間。</span><span class="sxs-lookup"><span data-stu-id="69a47-306">The vertical bar in the *right_shift* and *right_shift_assignment* productions are used to indicate that, unlike other productions in the syntactic grammar, no characters of any kind (not even whitespace) are allowed between the tokens.</span></span> <span data-ttu-id="69a47-307">這些實際執行的處理特別，以便能夠正確處理*type_parameter_list*s ([型別參數](classes.md#type-parameters))。</span><span class="sxs-lookup"><span data-stu-id="69a47-307">These productions are treated specially in order to enable the correct  handling of *type_parameter_list*s ([Type parameters](classes.md#type-parameters)).</span></span>

## <a name="pre-processing-directives"></a><span data-ttu-id="69a47-308">前置處理指示詞</span><span class="sxs-lookup"><span data-stu-id="69a47-308">Pre-processing directives</span></span>

<span data-ttu-id="69a47-309">前置處理指示詞可讓您有條件地略過原始程式檔，以報告錯誤和警告狀況的區段，並區分的來源程式碼的特定區域。</span><span class="sxs-lookup"><span data-stu-id="69a47-309">The pre-processing directives provide the ability to conditionally skip sections of source files, to report error and warning conditions, and to delineate distinct regions of source code.</span></span> <span data-ttu-id="69a47-310">「 前置處理指示詞 」 一詞只適用於 C 和 c + + 程式設計語言與一致性。</span><span class="sxs-lookup"><span data-stu-id="69a47-310">The term "pre-processing directives" is used only for consistency with the C and C++ programming languages.</span></span> <span data-ttu-id="69a47-311">在 C# 中，沒有任何個別的前置處理步驟;語彙分析階段期間，會處理前置處理指示詞。</span><span class="sxs-lookup"><span data-stu-id="69a47-311">In C#, there is no separate pre-processing step; pre-processing directives are processed as part of the lexical analysis phase.</span></span>

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

<span data-ttu-id="69a47-312">可使用下列的前置處理指示詞：</span><span class="sxs-lookup"><span data-stu-id="69a47-312">The following pre-processing directives are available:</span></span>

*  <span data-ttu-id="69a47-313">`#define` 並`#undef`，這用來定義和未定義，分別，條件式編譯符號 ([宣告指示詞](lexical-structure.md#declaration-directives))。</span><span class="sxs-lookup"><span data-stu-id="69a47-313">`#define` and `#undef`, which are used to define and undefine, respectively, conditional compilation symbols ([Declaration directives](lexical-structure.md#declaration-directives)).</span></span>
*  <span data-ttu-id="69a47-314">`#if``#elif`， `#else`，並`#endif`，用來有條件地略過的原始程式碼區段 ([條件式編譯指示詞](lexical-structure.md#conditional-compilation-directives))。</span><span class="sxs-lookup"><span data-stu-id="69a47-314">`#if`, `#elif`, `#else`, and `#endif`, which are used to conditionally skip sections of source code ([Conditional compilation directives](lexical-structure.md#conditional-compilation-directives)).</span></span>
*  <span data-ttu-id="69a47-315">`#line`用以控制發出的錯誤和警告的行號 ([行指示詞](lexical-structure.md#line-directives))。</span><span class="sxs-lookup"><span data-stu-id="69a47-315">`#line`, which is used to control line numbers emitted for errors and warnings ([Line directives](lexical-structure.md#line-directives)).</span></span>
*  <span data-ttu-id="69a47-316">`#error` 並`#warning`，用來分別發出錯誤和警告，([診斷指示詞](lexical-structure.md#diagnostic-directives))。</span><span class="sxs-lookup"><span data-stu-id="69a47-316">`#error` and `#warning`, which are used to issue errors and warnings, respectively ([Diagnostic directives](lexical-structure.md#diagnostic-directives)).</span></span>
*  <span data-ttu-id="69a47-317">`#region` 並`#endregion`，用來明確地標示的原始程式碼區段 ([區域指示詞](lexical-structure.md#region-directives))。</span><span class="sxs-lookup"><span data-stu-id="69a47-317">`#region` and `#endregion`, which are used to explicitly mark sections of source code ([Region directives](lexical-structure.md#region-directives)).</span></span>
*  <span data-ttu-id="69a47-318">`#pragma`用以指定至編譯器的選擇性內容資訊 ([Pragma 指示詞](lexical-structure.md#pragma-directives))。</span><span class="sxs-lookup"><span data-stu-id="69a47-318">`#pragma`, which is used to specify optional contextual information to the compiler ([Pragma directives](lexical-structure.md#pragma-directives)).</span></span>

<span data-ttu-id="69a47-319">前置處理指示詞一定會佔用一行的來源程式碼，並一律以開頭`#`字元和前置處理指示詞的名稱。</span><span class="sxs-lookup"><span data-stu-id="69a47-319">A pre-processing directive always occupies a separate line of source code and always begins with a `#` character and a pre-processing directive name.</span></span> <span data-ttu-id="69a47-320">之前的空格可能會發生`#`字元之間及`#`字元和指示詞的名稱。</span><span class="sxs-lookup"><span data-stu-id="69a47-320">White space may occur before the `#` character and between the `#` character and the directive name.</span></span>

<span data-ttu-id="69a47-321">包含來源線條`#define`， `#undef`， `#if`， `#elif`， `#else`， `#endif`， `#line`，或`#endregion`指示詞結尾的單行註解。</span><span class="sxs-lookup"><span data-stu-id="69a47-321">A source line containing a `#define`, `#undef`, `#if`, `#elif`, `#else`, `#endif`, `#line`, or `#endregion` directive may end with a single-line comment.</span></span> <span data-ttu-id="69a47-322">分隔註解 (`/* */`樣式的註解) 不允許使用包含前置處理指示詞的原始程式行。</span><span class="sxs-lookup"><span data-stu-id="69a47-322">Delimited comments (the `/* */` style of comments) are not permitted on source lines containing pre-processing directives.</span></span>

<span data-ttu-id="69a47-323">前置處理指示詞沒有權杖，而且不是 C# 的語法文法的一部分。</span><span class="sxs-lookup"><span data-stu-id="69a47-323">Pre-processing directives are not tokens and are not part of the syntactic grammar of C#.</span></span> <span data-ttu-id="69a47-324">不過，前置處理指示詞可用來包含或排除的語彙基元序列，而且會以該方式影響 C# 程式的意義。</span><span class="sxs-lookup"><span data-stu-id="69a47-324">However, pre-processing directives can be used to include or exclude sequences of tokens and can in that way affect the meaning of a C# program.</span></span> <span data-ttu-id="69a47-325">例如，當編譯時，程式：</span><span class="sxs-lookup"><span data-stu-id="69a47-325">For example, when compiled, the program:</span></span>
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
<span data-ttu-id="69a47-326">確切的相同順序的語彙基元做為程式的結果：</span><span class="sxs-lookup"><span data-stu-id="69a47-326">results in the exact same sequence of tokens as the program:</span></span>
```csharp
class C
{
    void F() {}
    void I() {}
}
```

<span data-ttu-id="69a47-327">因此，而語彙，這兩個程式是很不一樣，在語法上，它們完全相同。</span><span class="sxs-lookup"><span data-stu-id="69a47-327">Thus, whereas lexically, the two programs are quite different, syntactically, they are identical.</span></span>

### <a name="conditional-compilation-symbols"></a><span data-ttu-id="69a47-328">條件式編譯符號</span><span class="sxs-lookup"><span data-stu-id="69a47-328">Conditional compilation symbols</span></span>

<span data-ttu-id="69a47-329">所提供的條件式編譯功能`#if`， `#elif`， `#else`，以及`#endif`指示詞透過預先處理運算式 ([預先處理運算式](lexical-structure.md#pre-processing-expressions))和條件式編譯符號。</span><span class="sxs-lookup"><span data-stu-id="69a47-329">The conditional compilation functionality provided by the `#if`, `#elif`, `#else`, and `#endif` directives is controlled through pre-processing expressions ([Pre-processing expressions](lexical-structure.md#pre-processing-expressions)) and conditional compilation symbols.</span></span>

```antlr
conditional_symbol
    : '<Any identifier_or_keyword except true or false>'
    ;
```

<span data-ttu-id="69a47-330">條件式編譯符號有兩個可能的狀態：***定義***或是***未定義***。</span><span class="sxs-lookup"><span data-stu-id="69a47-330">A conditional compilation symbol has two possible states: ***defined*** or ***undefined***.</span></span> <span data-ttu-id="69a47-331">在原始程式檔的語彙處理開始，除非它已經由外部機制 （例如命令列編譯器選項） 來明確定義，是未定義條件式編譯符號。</span><span class="sxs-lookup"><span data-stu-id="69a47-331">At the beginning of the lexical processing of a source file, a conditional compilation symbol is undefined unless it has been explicitly defined by an external mechanism (such as a command-line compiler option).</span></span> <span data-ttu-id="69a47-332">當`#define`處理指示詞時，該指示詞命名的條件式編譯符號會變成已定義在該原始程式檔中。</span><span class="sxs-lookup"><span data-stu-id="69a47-332">When a `#define` directive is processed, the conditional compilation symbol named in that directive becomes defined in that source file.</span></span> <span data-ttu-id="69a47-333">符號會保持其定義之前`#undef`指示詞會處理該相同的符號，或是來源檔案的結尾為止。</span><span class="sxs-lookup"><span data-stu-id="69a47-333">The symbol remains defined until an `#undef` directive for that same symbol is processed, or until the end of the source file is reached.</span></span> <span data-ttu-id="69a47-334">含意在於`#define`和`#undef`一個原始程式檔中的指示詞不影響相同程式中其他原始程式檔。</span><span class="sxs-lookup"><span data-stu-id="69a47-334">An implication of this is that `#define` and `#undef` directives in one source file have no effect on other source files in the same program.</span></span>

<span data-ttu-id="69a47-335">定義的條件式編譯符號前置處理的運算式中參考時，具有布林值`true`，而且未定義的條件式編譯符號，則為 true 的值`false`。</span><span class="sxs-lookup"><span data-stu-id="69a47-335">When referenced in a pre-processing expression, a defined conditional compilation symbol has the boolean value `true`, and an undefined conditional compilation symbol has the boolean value `false`.</span></span> <span data-ttu-id="69a47-336">不需要在預先處理運算式中參考這些條件式編譯符號先明確宣告。</span><span class="sxs-lookup"><span data-stu-id="69a47-336">There is no requirement that conditional compilation symbols be explicitly declared before they are referenced in pre-processing expressions.</span></span> <span data-ttu-id="69a47-337">相反地，未宣告的符號不會直接定義，且因此該值`false`。</span><span class="sxs-lookup"><span data-stu-id="69a47-337">Instead, undeclared symbols are simply undefined and thus have the value `false`.</span></span>

<span data-ttu-id="69a47-338">條件式編譯符號的命名空間是相異，且不同於 C# 程式中的其他所有具名實體。</span><span class="sxs-lookup"><span data-stu-id="69a47-338">The name space for conditional compilation symbols is distinct and separate from all other named entities in a C# program.</span></span> <span data-ttu-id="69a47-339">條件式編譯符號只能參考中`#define`和`#undef`指示詞在前置處理的運算式。</span><span class="sxs-lookup"><span data-stu-id="69a47-339">Conditional compilation symbols can only be referenced in `#define` and `#undef` directives and in pre-processing expressions.</span></span>

### <a name="pre-processing-expressions"></a><span data-ttu-id="69a47-340">前置處理的運算式</span><span class="sxs-lookup"><span data-stu-id="69a47-340">Pre-processing expressions</span></span>

<span data-ttu-id="69a47-341">前置處理運算式可以出現在`#if`和`#elif`指示詞。</span><span class="sxs-lookup"><span data-stu-id="69a47-341">Pre-processing expressions can occur in `#if` and `#elif` directives.</span></span> <span data-ttu-id="69a47-342">運算子`!`， `==`， `!=`，`&&`和`||`預先處理運算式中允許和括號可能會用來分組。</span><span class="sxs-lookup"><span data-stu-id="69a47-342">The operators `!`, `==`, `!=`, `&&` and `||` are permitted in pre-processing expressions, and parentheses may be used for grouping.</span></span>

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

<span data-ttu-id="69a47-343">定義的條件式編譯符號前置處理的運算式中參考時，具有布林值`true`，而且未定義的條件式編譯符號，則為 true 的值`false`。</span><span class="sxs-lookup"><span data-stu-id="69a47-343">When referenced in a pre-processing expression, a defined conditional compilation symbol has the boolean value `true`, and an undefined conditional compilation symbol has the boolean value `false`.</span></span>

<span data-ttu-id="69a47-344">一律會前置處理的運算式的評估會產生布林值。</span><span class="sxs-lookup"><span data-stu-id="69a47-344">Evaluation of a pre-processing expression always yields a boolean value.</span></span> <span data-ttu-id="69a47-345">前置處理的運算式評估的規則是一樣的常數運算式 ([常數運算式](expressions.md#constant-expressions))，但使用者定義實體可以被參考的條件式編譯符號.</span><span class="sxs-lookup"><span data-stu-id="69a47-345">The rules of evaluation for a pre-processing expression are the same as those for a constant expression ([Constant expressions](expressions.md#constant-expressions)), except that the only user-defined entities that can be referenced are conditional compilation symbols.</span></span>

### <a name="declaration-directives"></a><span data-ttu-id="69a47-346">宣告指示詞</span><span class="sxs-lookup"><span data-stu-id="69a47-346">Declaration directives</span></span>

<span data-ttu-id="69a47-347">宣告指示詞用來定義或取消定義條件式編譯符號。</span><span class="sxs-lookup"><span data-stu-id="69a47-347">The declaration directives are used to define or undefine conditional compilation symbols.</span></span>

```antlr
pp_declaration
    : whitespace? '#' whitespace? 'define' whitespace conditional_symbol pp_new_line
    | whitespace? '#' whitespace? 'undef' whitespace conditional_symbol pp_new_line
    ;

pp_new_line
    : whitespace? single_line_comment? new_line
    ;
```

<span data-ttu-id="69a47-348">處理`#define`指示詞會導致變成已定義，指定的條件式編譯符號開頭指示詞之後的原始程式行。</span><span class="sxs-lookup"><span data-stu-id="69a47-348">The processing of a `#define` directive causes the given conditional compilation symbol to become defined, starting with the source line that follows the directive.</span></span> <span data-ttu-id="69a47-349">同樣地，在處理`#undef`指示詞會讓指定的條件式編譯符號會變成未定義，並遵循指示詞的原始程式行以開始。</span><span class="sxs-lookup"><span data-stu-id="69a47-349">Likewise, the processing of an `#undef` directive causes the given conditional compilation symbol to become undefined, starting with the source line that follows the directive.</span></span>

<span data-ttu-id="69a47-350">任何`#define`並`#undef`原始程式檔中的指示詞必須出現之前，先*語彙基元*([權杖](lexical-structure.md#tokens)) 在來源檔案中; 否則編譯時期錯誤就會發生。</span><span class="sxs-lookup"><span data-stu-id="69a47-350">Any `#define` and `#undef` directives in a source file must occur before the first *token* ([Tokens](lexical-structure.md#tokens)) in the source file; otherwise a compile-time error occurs.</span></span> <span data-ttu-id="69a47-351">簡單來說，`#define`和`#undef`指示詞必須在任何 「 實際程式碼 」 之前的原始程式檔中。</span><span class="sxs-lookup"><span data-stu-id="69a47-351">In intuitive terms, `#define` and `#undef` directives must precede any "real code" in the source file.</span></span>

<span data-ttu-id="69a47-352">範例：</span><span class="sxs-lookup"><span data-stu-id="69a47-352">The example:</span></span>
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
<span data-ttu-id="69a47-353">無效，因為`#define`指示詞前面的第一個語彙基元 (`namespace`關鍵字) 中的原始程式檔。</span><span class="sxs-lookup"><span data-stu-id="69a47-353">is valid because the `#define` directives precede the first token (the `namespace` keyword) in the source file.</span></span>

<span data-ttu-id="69a47-354">下列範例會產生編譯時期錯誤，因為`#define`遵循真正的程式碼：</span><span class="sxs-lookup"><span data-stu-id="69a47-354">The following example results in a compile-time error because a `#define` follows real code:</span></span>
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

<span data-ttu-id="69a47-355">A`#define`可能會定義條件式編譯符號已定義，而不需要那里任何中介`#undef`該符號。</span><span class="sxs-lookup"><span data-stu-id="69a47-355">A `#define` may define a conditional compilation symbol that is already defined, without there being any intervening `#undef` for that symbol.</span></span> <span data-ttu-id="69a47-356">以下範例定義的條件式編譯符號`A`，然後再次定義。</span><span class="sxs-lookup"><span data-stu-id="69a47-356">The example below defines a conditional compilation symbol `A` and then defines it again.</span></span>
```csharp
#define A
#define A
```

<span data-ttu-id="69a47-357">A`#undef`可能 「 取消 」 未定義的條件式編譯符號。</span><span class="sxs-lookup"><span data-stu-id="69a47-357">A `#undef` may "undefine" a conditional compilation symbol that is not defined.</span></span> <span data-ttu-id="69a47-358">以下範例定義的條件式編譯符號`A`雖然然後取消它兩次; 定義和第二個`#undef`沒有任何作用，它是否仍然有效。</span><span class="sxs-lookup"><span data-stu-id="69a47-358">The example below defines a conditional compilation symbol `A` and then undefines it twice; although the second `#undef` has no effect, it is still valid.</span></span>
```csharp
#define A
#undef A
#undef A
```

### <a name="conditional-compilation-directives"></a><span data-ttu-id="69a47-359">條件式編譯指示詞</span><span class="sxs-lookup"><span data-stu-id="69a47-359">Conditional compilation directives</span></span>

<span data-ttu-id="69a47-360">條件式編譯指示詞可有條件地包含或排除的原始程式檔部分。</span><span class="sxs-lookup"><span data-stu-id="69a47-360">The conditional compilation directives are used to conditionally include or exclude portions of a source file.</span></span>

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

<span data-ttu-id="69a47-361">如語法所示，條件式編譯指示詞必須寫成集組成，依序`#if`指示詞，零或多個`#elif`指示詞、 零或一個`#else`指示詞，和`#endif`指示詞。</span><span class="sxs-lookup"><span data-stu-id="69a47-361">As indicated by the syntax, conditional compilation directives must be written as sets consisting of, in order, an `#if` directive, zero or more `#elif` directives, zero or one `#else` directive, and an `#endif` directive.</span></span> <span data-ttu-id="69a47-362">指示詞之間是原始程式碼的條件式區段。</span><span class="sxs-lookup"><span data-stu-id="69a47-362">Between the directives are conditional sections of source code.</span></span> <span data-ttu-id="69a47-363">每個區段會受到正前面的指示詞。</span><span class="sxs-lookup"><span data-stu-id="69a47-363">Each section is controlled by the immediately preceding directive.</span></span> <span data-ttu-id="69a47-364">條件式區段可能本身包含巢狀的條件式編譯指示詞提供這些指示詞會形成完整的設定。</span><span class="sxs-lookup"><span data-stu-id="69a47-364">A conditional section may itself contain nested conditional compilation directives provided these directives form complete sets.</span></span>

<span data-ttu-id="69a47-365">A *pp_conditional*選取最多一個內含*conditional_section*語彙處理 s:</span><span class="sxs-lookup"><span data-stu-id="69a47-365">A *pp_conditional* selects at most one of the contained *conditional_section*s for normal lexical processing:</span></span>

*  <span data-ttu-id="69a47-366">*Pp_expression*之`#if`並`#elif`指示詞的評估順序，直到其中一個會產生`true`。</span><span class="sxs-lookup"><span data-stu-id="69a47-366">The *pp_expression*s of the `#if` and `#elif` directives are evaluated in order until one yields `true`.</span></span> <span data-ttu-id="69a47-367">如果運算式會產生`true`，則*conditional_section*選取對應的指示詞。</span><span class="sxs-lookup"><span data-stu-id="69a47-367">If an expression yields `true`, the *conditional_section* of the corresponding directive is selected.</span></span>
*  <span data-ttu-id="69a47-368">如果要將所有*pp_expression*s yield `false`，而且如果`#else`指示詞已存在， *conditional_section*的`#else`指示詞已選取。</span><span class="sxs-lookup"><span data-stu-id="69a47-368">If all *pp_expression*s yield `false`, and if an `#else` directive is present, the *conditional_section* of the `#else` directive is selected.</span></span>
*  <span data-ttu-id="69a47-369">否則為否*conditional_section*已選取。</span><span class="sxs-lookup"><span data-stu-id="69a47-369">Otherwise, no *conditional_section* is selected.</span></span>

<span data-ttu-id="69a47-370">所選*conditional_section*，如果任何項目，都處理為一般*input_section*： 一節中所包含的原始程式碼必須遵守語彙文法; 從來源產生權杖區段; 中的程式碼還有一節中的前置處理指示詞指定的效果。</span><span class="sxs-lookup"><span data-stu-id="69a47-370">The selected *conditional_section*, if any, is processed as a normal *input_section*: the source code contained in the section must adhere to the lexical grammar; tokens are generated from the source code in the section; and pre-processing directives in the section have the prescribed effects.</span></span>

<span data-ttu-id="69a47-371">其餘*conditional_section*視為處理 s，如果有的話*skipped_section*除了前置處理指示詞 s，來源中的程式碼區段需要遵守語彙文法，否從來源中的程式碼區段中; 產生權杖和一節中的前置處理指示詞必須是正確的語彙，但不是會否則處理。</span><span class="sxs-lookup"><span data-stu-id="69a47-371">The remaining *conditional_section*s, if any, are processed as *skipped_section*s: except for pre-processing directives, the source code in the section need not adhere to the lexical grammar; no tokens are generated from the source code in the section; and pre-processing directives in the section must be lexically correct but are not otherwise processed.</span></span> <span data-ttu-id="69a47-372">內*conditional_section* ，為正在處理*skipped_section*任何巢狀*conditional_section*s (包含在巢狀`#if`...`#endif`和`#region`...`#endregion`建構) 也會做為處理*skipped_section*s。</span><span class="sxs-lookup"><span data-stu-id="69a47-372">Within a *conditional_section* that is being processed as a *skipped_section*, any nested *conditional_section*s (contained in nested `#if`...`#endif` and `#region`...`#endregion` constructs) are also processed as *skipped_section*s.</span></span>

<span data-ttu-id="69a47-373">下列範例說明如何在條件式編譯指示詞可以巢狀：</span><span class="sxs-lookup"><span data-stu-id="69a47-373">The following example illustrates how conditional compilation directives can nest:</span></span>
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

<span data-ttu-id="69a47-374">前置處理指示詞，除了已略過的原始碼不受限於語彙分析。</span><span class="sxs-lookup"><span data-stu-id="69a47-374">Except for pre-processing directives, skipped source code is not subject to lexical analysis.</span></span> <span data-ttu-id="69a47-375">例如，以下是儘管未結束的註解中有效`#else`區段：</span><span class="sxs-lookup"><span data-stu-id="69a47-375">For example, the following is valid despite the unterminated comment in the `#else` section:</span></span>
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

<span data-ttu-id="69a47-376">不過請注意，前置處理指示詞所需的原始碼的略過區段中，甚至是語彙正確。</span><span class="sxs-lookup"><span data-stu-id="69a47-376">Note, however, that pre-processing directives are required to be lexically correct even in skipped sections of source code.</span></span>

<span data-ttu-id="69a47-377">出現在多行輸入項目內時，不會處理前置處理指示詞。</span><span class="sxs-lookup"><span data-stu-id="69a47-377">Pre-processing directives are not processed when they appear inside multi-line input elements.</span></span> <span data-ttu-id="69a47-378">例如，此方案：</span><span class="sxs-lookup"><span data-stu-id="69a47-378">For example, the program:</span></span>
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
<span data-ttu-id="69a47-379">在輸出中的結果：</span><span class="sxs-lookup"><span data-stu-id="69a47-379">results in the output:</span></span>
```
hello,
#if Debug
        world
#else
        Nebraska
#endif
```

<span data-ttu-id="69a47-380">在罕見的情況下，處理前置處理指示詞的集合可能會取決於評估*pp_expression*。</span><span class="sxs-lookup"><span data-stu-id="69a47-380">In peculiar cases, the set of pre-processing directives that is processed might depend on the evaluation of the *pp_expression*.</span></span> <span data-ttu-id="69a47-381">範例：</span><span class="sxs-lookup"><span data-stu-id="69a47-381">The example:</span></span>
```csharp
#if X
    /*
#else
    /* */ class Q { }
#endif
```
<span data-ttu-id="69a47-382">一律會產生相同的語彙基元資料流 (`class` `Q` `{` `}`)，而不論是否`X`定義。</span><span class="sxs-lookup"><span data-stu-id="69a47-382">always produces the same token stream (`class` `Q` `{` `}`), regardless of whether or not `X` is defined.</span></span> <span data-ttu-id="69a47-383">如果`X`是定義，是唯一的處理指示詞`#if`和`#endif`，因為多行註解。</span><span class="sxs-lookup"><span data-stu-id="69a47-383">If `X` is defined, the only processed directives are `#if` and `#endif`, due to the multi-line comment.</span></span> <span data-ttu-id="69a47-384">如果`X`是未定義，然後三個指示詞 (`#if`， `#else`， `#endif`) 會指示詞組的一部分。</span><span class="sxs-lookup"><span data-stu-id="69a47-384">If `X` is undefined, then three directives (`#if`, `#else`, `#endif`) are part of the directive set.</span></span>

### <a name="diagnostic-directives"></a><span data-ttu-id="69a47-385">診斷指示詞</span><span class="sxs-lookup"><span data-stu-id="69a47-385">Diagnostic directives</span></span>

<span data-ttu-id="69a47-386">診斷指示詞用來明確地產生錯誤和警告訊息中其他的編譯時間錯誤和警告相同的方式所報告。</span><span class="sxs-lookup"><span data-stu-id="69a47-386">The diagnostic directives are used to explicitly generate error and warning messages that are reported in the same way as other compile-time errors and warnings.</span></span>

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

<span data-ttu-id="69a47-387">範例：</span><span class="sxs-lookup"><span data-stu-id="69a47-387">The example:</span></span>
```csharp
#warning Code review needed before check-in

#if Debug && Retail
    #error A build can't be both debug and retail
#endif

class Test {...}
```
<span data-ttu-id="69a47-388">一律會產生警告 （「 程式碼檢閱需要簽入之前 」），並會產生編譯時期錯誤 （「 組建不能是零售和偵錯 」） 如果條件式符號`Debug`和`Retail`兩者都已定義。</span><span class="sxs-lookup"><span data-stu-id="69a47-388">always produces a warning ("Code review needed before check-in"), and produces a compile-time error ("A build can't be both debug and retail") if the conditional symbols `Debug` and `Retail` are both defined.</span></span> <span data-ttu-id="69a47-389">請注意， *pp_message*可以包含任意文字; 具體來說，它不需要包含語式正確的語彙基元，單引號的文字中所示`can't`。</span><span class="sxs-lookup"><span data-stu-id="69a47-389">Note that a *pp_message* can contain arbitrary text; specifically, it need not contain well-formed tokens, as shown by the single quote in the word `can't`.</span></span>

### <a name="region-directives"></a><span data-ttu-id="69a47-390">區域指示詞</span><span class="sxs-lookup"><span data-stu-id="69a47-390">Region directives</span></span>

<span data-ttu-id="69a47-391">區域指示詞用來明確標記的原始碼的區域。</span><span class="sxs-lookup"><span data-stu-id="69a47-391">The region directives are used to explicitly mark regions of source code.</span></span>

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

<span data-ttu-id="69a47-392">任何語意意義被不附加到區域;區域被專供程式設計師，或透過自動化工具來標示的原始程式碼區段。</span><span class="sxs-lookup"><span data-stu-id="69a47-392">No semantic meaning is attached to a region; regions are intended for use by the programmer or by automated tools to mark a section of source code.</span></span> <span data-ttu-id="69a47-393">中指定的訊息`#region`或`#endregion`指示詞同樣沒有語意意義; 它只是可用來識別區域。</span><span class="sxs-lookup"><span data-stu-id="69a47-393">The message specified in a `#region` or `#endregion` directive likewise has no semantic meaning; it merely serves to identify the region.</span></span> <span data-ttu-id="69a47-394">比對`#region`並`#endregion`指示詞可能會具有不同*pp_message*s。</span><span class="sxs-lookup"><span data-stu-id="69a47-394">Matching `#region` and `#endregion` directives may have different *pp_message*s.</span></span>

<span data-ttu-id="69a47-395">區域語彙處理：</span><span class="sxs-lookup"><span data-stu-id="69a47-395">The lexical processing of a region:</span></span>
```csharp
#region
...
#endregion
```
<span data-ttu-id="69a47-396">就相當於語彙處理表單的條件式編譯指示詞：</span><span class="sxs-lookup"><span data-stu-id="69a47-396">corresponds exactly to the lexical processing of a conditional compilation directive of the form:</span></span>
```csharp
#if true
...
#endif
```

### <a name="line-directives"></a><span data-ttu-id="69a47-397">Line 指示詞</span><span class="sxs-lookup"><span data-stu-id="69a47-397">Line directives</span></span>

<span data-ttu-id="69a47-398">Line 指示詞可用來變更的行號和原始程式檔名稱，例如警告和錯誤輸出中，編譯器會報告和所使用的呼叫端資訊屬性 ([呼叫端資訊屬性](attributes.md#caller-info-attributes))。</span><span class="sxs-lookup"><span data-stu-id="69a47-398">Line directives may be used to alter the line numbers and source file names that are reported by the compiler in output such as warnings and errors, and that are used by caller info attributes ([Caller info attributes](attributes.md#caller-info-attributes)).</span></span>

<span data-ttu-id="69a47-399">從輸入的一些其他文字產生 C# 原始程式碼的中繼程式設計工具中最常使用 line 指示詞。</span><span class="sxs-lookup"><span data-stu-id="69a47-399">Line directives are most commonly used in meta-programming tools that generate C# source code from some other text input.</span></span>

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

<span data-ttu-id="69a47-400">若未`#line`指示詞存在，則編譯器會報告，則為 true 的行號和在其輸出中的原始程式檔名稱。</span><span class="sxs-lookup"><span data-stu-id="69a47-400">When no `#line` directives are present, the compiler reports true line numbers and source file names in its output.</span></span> <span data-ttu-id="69a47-401">處理時`#line`指示詞，其中包含*line_indicator*不是`default`，編譯器會為具有指定的行號 （和檔案名稱，如果指定） 指示詞之後的行。</span><span class="sxs-lookup"><span data-stu-id="69a47-401">When processing a `#line` directive that includes a *line_indicator* that is not `default`, the compiler treats the line after the directive as having the given line number (and file name, if specified).</span></span>

<span data-ttu-id="69a47-402">A`#line default`指示詞會反轉所有上述 #line 指示詞的效果。</span><span class="sxs-lookup"><span data-stu-id="69a47-402">A `#line default` directive reverses the effect of all preceding #line directives.</span></span> <span data-ttu-id="69a47-403">編譯器會精確地說，如果沒有報告接下來的幾行，則為 true 的行資訊`#line`有處理指示詞。</span><span class="sxs-lookup"><span data-stu-id="69a47-403">The compiler reports true line information for subsequent lines, precisely as if no `#line` directives had been processed.</span></span>

<span data-ttu-id="69a47-404">A`#line hidden`指示詞沒有任何作用，檔案和行號報告錯誤訊息，但是會影響來源層級偵錯。</span><span class="sxs-lookup"><span data-stu-id="69a47-404">A `#line hidden` directive has no effect on the file and line numbers reported in error messages, but does affect source level debugging.</span></span> <span data-ttu-id="69a47-405">偵錯時，所有行之間`#line hidden`指示詞和後續`#line`指示詞 (不是`#line hidden`) 有沒有行號資訊。</span><span class="sxs-lookup"><span data-stu-id="69a47-405">When debugging, all lines between a `#line hidden` directive and the subsequent `#line` directive (that is not `#line hidden`) have no line number information.</span></span> <span data-ttu-id="69a47-406">時逐步執行偵錯工具中的程式碼，將會完全略過這些行。</span><span class="sxs-lookup"><span data-stu-id="69a47-406">When stepping through code in the debugger, these lines will be skipped entirely.</span></span>

<span data-ttu-id="69a47-407">請注意， *file_name*不同於一般的字串常值，不會處理逸出字元; 「`\`"字元只可指定在一般的反斜線字元*file_name*.</span><span class="sxs-lookup"><span data-stu-id="69a47-407">Note that a *file_name* differs from a regular string literal in that escape characters are not processed; the "`\`" character simply designates an ordinary backslash character within a *file_name*.</span></span>

### <a name="pragma-directives"></a><span data-ttu-id="69a47-408">Pragma 指示詞</span><span class="sxs-lookup"><span data-stu-id="69a47-408">Pragma directives</span></span>

<span data-ttu-id="69a47-409">`#pragma`前置處理器指示詞用來指定選擇性的內容資訊給編譯器。</span><span class="sxs-lookup"><span data-stu-id="69a47-409">The `#pragma` preprocessing directive is used to specify optional contextual information to the compiler.</span></span> <span data-ttu-id="69a47-410">中提供的資訊`#pragma`指示詞永遠不會變更程式的語意。</span><span class="sxs-lookup"><span data-stu-id="69a47-410">The information supplied in a `#pragma` directive will never change program semantics.</span></span>

```antlr
pp_pragma
    : whitespace? '#' whitespace? 'pragma' whitespace pragma_body pp_new_line
    ;

pragma_body
    : pragma_warning_body
    ;
```

<span data-ttu-id="69a47-411">C# 提供`#pragma`指示詞，可控制編譯器警告。</span><span class="sxs-lookup"><span data-stu-id="69a47-411">C# provides `#pragma` directives to control compiler warnings.</span></span> <span data-ttu-id="69a47-412">語言的未來版本可能包含額外`#pragma`指示詞。</span><span class="sxs-lookup"><span data-stu-id="69a47-412">Future versions of the language may include additional `#pragma` directives.</span></span> <span data-ttu-id="69a47-413">若要確保與其他 C# 編譯器的互通性，Microsoft C# 編譯器不會發出未知的編譯錯誤`#pragma`指示詞; 這類指示詞不過產生警告。</span><span class="sxs-lookup"><span data-stu-id="69a47-413">To ensure interoperability with other C# compilers, the Microsoft C# compiler does not issue compilation errors for unknown `#pragma` directives; such directives do however generate warnings.</span></span>

#### <a name="pragma-warning"></a><span data-ttu-id="69a47-414">Pragma 警告</span><span class="sxs-lookup"><span data-stu-id="69a47-414">Pragma warning</span></span>

<span data-ttu-id="69a47-415">`#pragma warning`指示詞用來停用或還原所有或後續的程式文字編譯期間的一組特定的警告訊息。</span><span class="sxs-lookup"><span data-stu-id="69a47-415">The `#pragma warning` directive is used to disable or restore all or a particular set of warning messages during compilation of the subsequent program text.</span></span>

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

<span data-ttu-id="69a47-416">A`#pragma warning`省略警告清單的指示詞會影響所有警告。</span><span class="sxs-lookup"><span data-stu-id="69a47-416">A `#pragma warning` directive that omits the warning list affects all warnings.</span></span> <span data-ttu-id="69a47-417">A`#pragma warning`指示詞包含警告清單會影響的只在清單中指定的警告。</span><span class="sxs-lookup"><span data-stu-id="69a47-417">A `#pragma warning` directive the includes a warning list affects only those warnings that are specified in the list.</span></span>

<span data-ttu-id="69a47-418">A`#pragma warning disable`指示詞會停用所有或一組指定的警告。</span><span class="sxs-lookup"><span data-stu-id="69a47-418">A `#pragma warning disable` directive disables all or the given set of warnings.</span></span>

<span data-ttu-id="69a47-419">A`#pragma warning restore`指示詞還原所有或一組指定的警告，已在作用中的編譯單位開始處的狀態。</span><span class="sxs-lookup"><span data-stu-id="69a47-419">A `#pragma warning restore` directive restores all or the given set of warnings to the state that was in effect at the beginning of the compilation unit.</span></span> <span data-ttu-id="69a47-420">請注意，如果特定的警告已停用外部、 `#pragma warning restore` (是否所有或特定的警告) 並不會重新啟動該警告。</span><span class="sxs-lookup"><span data-stu-id="69a47-420">Note that if a particular warning was disabled externally, a `#pragma warning restore` (whether for all or the specific warning) will not re-enable that warning.</span></span>

<span data-ttu-id="69a47-421">下列範例示範使用`#pragma warning`暫時停用警告時，報告而言已經過時參考成員，使用 Microsoft C# 編譯器的警告編號。</span><span class="sxs-lookup"><span data-stu-id="69a47-421">The following example shows use of `#pragma warning` to temporarily disable the warning reported when obsoleted members are referenced, using the warning number from the Microsoft C# compiler.</span></span>
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
