# <a name="lexical-structure"></a>語彙結構

## <a name="programs"></a>Programs

C#***計劃***包含一個或多個***原始程式檔***，稱為正式***編譯單位***([編譯單位](namespaces.md#compilation-units))。 原始程式檔是 Unicode 字元的排序的順序。 通常在檔案系統中，有一對一的對應檔案的原始程式檔，但不需要這種對應關係。 最大的可攜性，建議在檔案系統中的檔案會使用 utf-8 編碼的編碼方式。

在概念上來說，是在編譯程式使用三個步驟：

1. 轉換，將檔案從特定的字元項目和編碼配置轉換成 Unicode 字元序列。
2. 語彙分析，轉譯 token 的資料流中的 Unicode 輸入的字元資料流。
3. 語法分析，轉譯可執行程式碼中的語彙基元資料流。

## <a name="grammars"></a>文法

此規格會顯示 C# 程式設計語言使用兩個文法的語法。 ***語彙文法***([語彙文法](lexical-structure.md#lexical-grammar)) 會定義如何將 Unicode 字元結合以表單行終端符號、 空白字元、 註解、 權杖和前置處理指示詞。 ***語法的文法***([語法的文法](lexical-structure.md#syntactic-grammar)) 定義所產生的語彙文法的語彙基元組合表單 C# 程式的方式。

### <a name="grammar-notation"></a>文法標記法

語彙和語法的文法是以使用 ANTLR 文法檢查工具的標記法的巴克斯格式顯示。

### <a name="lexical-grammar"></a>語彙文法

語彙文法的 C# 所示[語彙分析](lexical-structure.md#lexical-analysis)，[語彙基元](lexical-structure.md#tokens)，並[前置處理指示詞](lexical-structure.md#pre-processing-directives)。 語彙文法終端符號 Unicode 字元集的字元，而且僅語彙文法可讓您指定的字元組合表單語彙基元的方式 ([語彙基元](lexical-structure.md#tokens))，泛空白字元 ([泛空白字元](lexical-structure.md#white-space))，註解 ([註解](lexical-structure.md#comments))，和前置處理指示詞 ([前置處理指示詞](lexical-structure.md#pre-processing-directives))。

在 C# 程式中的每個原始程式檔必須符合*輸入*生產語彙文法 ([語彙分析](lexical-structure.md#lexical-analysis))。

### <a name="syntactic-grammar"></a>語法的文法

C# 的語法文法所示的章節和附錄，依循本章內容而定。 語法的文法終端符號是由語彙文法中，定義語彙基元和語法的文法可讓您指定的語彙基元組合表單 C# 程式的方式。

每個原始程式檔中的C#程式都必須符合*compilation_unit*生產環境的語法文法 ([編譯單位](namespaces.md#compilation-units))。

## <a name="lexical-analysis"></a>語彙分析

*輸入*生產定義 C# 原始程式檔的語彙結構。 在 C# 程式中的每個原始程式檔必須符合此語彙文法生產環境。

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

五個基本項目組成的語彙結構C#原始程式檔：行結束字元 ([行結束字元](lexical-structure.md#line-terminators))，泛空白字元 ([泛空白字元](lexical-structure.md#white-space))，註解 ([註解](lexical-structure.md#comments))，語彙基元 ([權杖](lexical-structure.md#tokens))，以及前置處理指示詞 ([前置處理指示詞](lexical-structure.md#pre-processing-directives))。 這些基本項目，只有語彙基元是 C# 程式的句法的文法中 ([語法的文法](lexical-structure.md#syntactic-grammar))。

C# 原始程式檔語彙處理包含將檔案縮減成一連串的語彙基元會變得語法分析的輸入。 行終端符號、 空白字元和註解，可以當做分隔符號的語彙基元，但前置處理指示詞可能會導致略過，原始程式檔的區段，否則這些語彙項目語法的 C# 程式結構中沒有任何影響。

在字串插值常值的情況下 ([插補字串常值](lexical-structure.md#interpolated-string-literals)) 單一語彙基元一開始所產生的語彙分析，但是會分成數個輸入項目重複受制於語彙分析直到所有的內插的字串常值已獲得解決。 所產生的權杖則做為輸入語法的分析。

當數個語彙文法解析符合的原始程式檔中的字元序列時，語彙處理永遠會形成最長的可能語彙項目。 例如，字元序列`//`您的單行註解的開頭為處理，因為該語彙項目已超過單一`/`語彙基元。

### <a name="line-terminators"></a>行終端符號

行結束字元會分成幾行中的 C# 原始程式檔中的字元。

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

來源與相容性程式碼編輯新增的檔案結尾標記的工具，並讓原始程式檔案，以適當地檢視以一連串的結束行，下列轉換會套用，才能在 C# 程式中的每個原始程式檔：

*  如果原始程式檔的最後一個字元是控制 Z 字元 (`U+001A`)，在刪除此字元。
*  歸位字元 (`U+000D`) 加入至原始程式檔的結尾該原始程式檔是否為非空白，如果原始程式檔的最後一個字元不是歸位字元 (`U+000D`)、 換行字元 (`U+000A`)，行分隔符號 (`U+2028`)，或段落分隔符號 (`U+2029`)。

### <a name="comments"></a>註解

支援兩種形式的註解： 單行註解和分隔註解。 ***單行註解***的開頭字元`//`並擴充至原始程式檔的結尾。 ***分隔註解***的開頭字元`/*`和以字元結束`*/`。 分隔註解可能會跨越多行。

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

註解不進行巢狀處理。 字元序列`/*`並`*/`內沒有特殊意義`//`註解和字元序列`//`和`/*`分隔註解中沒有特殊意義。

註解不會處理內字元和字串常值。

此範例
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
包含分隔註解。

此範例
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
顯示數個單行註解。

### <a name="white-space"></a>空白字元

泛空白字元被定義為包含 Unicode 類別 Zs （其中包括空白字元） 的任何字元，以及水平定位字元、 垂直定位字元和表單換頁字元。

```antlr
whitespace
    : '<Any character with Unicode class Zs>'
    | '<Horizontal tab character (U+0009)>'
    | '<Vertical tab character (U+000B)>'
    | '<Form feed character (U+000C)>'
    ;
```

## <a name="tokens"></a>語彙基元

有數種類型的語彙基元： 識別項、 關鍵字、 常值、 運算子和標點符號。 泛空白字元和註解不屬於語彙基元，但它們可做為語彙基元的分隔符號。

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

### <a name="unicode-character-escape-sequences"></a>Unicode 字元的逸出序列

Unicode 字元的逸出序列代表 Unicode 字元。 Unicode 字元的逸出序列會處理在識別項 ([識別碼](lexical-structure.md#identifiers))，字元常值 ([字元常值](lexical-structure.md#character-literals))，和規則字串常值 ([字串常值](lexical-structure.md#string-literals)). Unicode 逸出字元不會處理任何其他位置 （例如，以形成運算子、 標點符號或關鍵字）。

```antlr
unicode_escape_sequence
    : '\\u' hex_digit hex_digit hex_digit hex_digit
    | '\\U' hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit
    ;
```

Unicode 逸出序列表示單一 Unicode 字元後面的十六進位數字的格式 」`\u`「 或 」`\U`"字元。 因為 C# 使用中的字元和字串值的 Unicode 字碼指標的 16 位元編碼方式，Unicode 字元 U + 10ffff，且範圍 u+10000 中的不允許的字元常值，並表示常值字串中使用 Unicode surrogate 字組。 不支援上述 0x10FFFF 字碼指標的 Unicode 字元。

不會執行多個翻譯。 比方說，字串常值 」`\u005Cu005C`"就相當於"`\u005C`"而非"`\`」。 Unicode 值`\u005C`是字元"`\`」。

此範例
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
顯示了多個`\u0066`，也就是逸出序列，以找到字母"`f`」。 程式相當於
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

### <a name="identifiers"></a>識別項

本節中提供的識別碼的規則，確實對應到這些建議由 Unicode 標準附錄 31，不同之處在於底線字元可以是初始 （通常是傳統的 C 程式設計語言），Unicode 逸出序列允許在識別項，而"`@`」 字元做為前置詞允許啟用關鍵字當做識別項。

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

如先前所述的 Unicode 字元類別的相關資訊，請參閱 Unicode Standard 3.0 版，區段 4.5。

有效識別項的範例包括 「`identifier1`"，"`_identifier2`"，和 「`@if`"。

合格的程式中的識別項必須是 Unicode 正規化表單 C 所定義的標準格式，所定義的 Unicode 標準附錄 15。 遇到不在正規化表單 C 識別項時的行為是由實作定義;不過，診斷不需要。

前置詞"`@`」 可讓您使用的關鍵字當做識別項，這與其他程式設計語言互動時很有用。 字元`@`不是實際的一部分的識別碼，因此識別項可能在其他語言中視為一般的識別碼，沒有前置詞。 使用識別項`@`稱為前置詞***逐字識別項***。 使用`@`允許，但強烈不建議的樣式的識別項不是關鍵字的前置詞。

範例：
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
定義類別，名為"`class`「 使用靜態方法，名為"`static`"會接受名為"`bool`」。 請注意，因為 Unicode 逸出中不允許使用關鍵字，語彙基元"`cl\u0061ss`」 是一個識別項，而且相同的識別碼 」`@class`"。

兩個識別項都會視為相同，如果套用下列轉換，順序之後，它們都一樣：

*  前置詞"`@`」，如果使用，會移除。
*  每個*unicode_escape_sequence*轉換成其對應的 Unicode 字元。
*  任何*formatting_character*已移除。

識別碼包含兩個連續底線字元 (`U+005F`) 已保留供實作。 比方說，實作可能會提供擴充的關鍵字開頭為兩個底線。

### <a name="keywords"></a>關鍵字

A***關鍵字***是識別項類似的一連串字元已保留，且不能做為識別項，除了當前面加上`@`字元。

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

在文法中某些地方，特定識別項具有特殊意義，，但不是關鍵字。 這類識別項有時稱為 「 內容相關的關鍵字 」。 比方說，在屬性宣告中，「`get`"和"`set`」 的識別項具有特殊意義 ([存取子](classes.md#accessors))。 以外的識別項`get`或`set`永遠不會用在這些位置中，所以這種使用不會使用這些字做為識別碼衝突。 在其他情況下，如在具有識別項 」`var`「 隱含類型區域變數宣告中 ([區域變數宣告](statements.md#local-variable-declarations))，宣告名稱發生衝突的內容關鍵字。 在此情況下，宣告的名稱會將優先順序高於識別碼作為內容的關鍵字。

### <a name="literals"></a>常值

A***常值***是值的來源的程式碼表示法。

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

#### <a name="boolean-literals"></a>布林值常值

有兩個布林常值：`true`和`false`。

```antlr
boolean_literal
    : 'true'
    | 'false'
    ;
```

型別*boolean_literal*是`bool`。

#### <a name="integer-literals"></a>整數常值

整數常值用來寫入類型的值`int`， `uint`， `long`，和`ulong`。 整數常值有兩個可能的表單： 十進位和十六進位。

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

整數常值的類型決定，如下所示：

*  如果常值沒有後置詞，它會有這些類型中可表示其值的第一個： `int`， `uint`， `long`， `ulong`。
*  如果常值加`U`或是`u`，它有一種類型中可表示其值的第一個： `uint`， `ulong`。
*  如果常值加`L`或是`l`，它有一種類型中可表示其值的第一個： `long`， `ulong`。
*  如果常值加`UL`， `Ul`， `uL`， `ul`， `LU`， `Lu`， `lU`，或`lu`，它是型別`ulong`。

如果整數常值所代表的值超出範圍`ulong`輸入時，就會發生編譯時期錯誤。

為樣式的問題，建議您，「`L`「 使用而不是 」`l`"寫入類型的常值時`long`，因為很容易混淆字母"`l`"與數字 」`1`"。

若要允許的最小可能`int`和`long`值寫入為十進位整數常值，下列兩個規則存在：

* 當*decimal_integer_literal* 2147483648 的值 (2 ^31) 和 no *integer_type_suffix*會顯示為一元減號運算子語彙基元的正後方的語彙基元 ([一元減號運算子](expressions.md#unary-minus-operator))，結果是類型的常數`int`值介於-2147483648 (-2 ^31)。 在其他情況下，這類*decimal_integer_literal*別的`uint`。
* 當*decimal_integer_literal*是 9223372036854775808 (2 ^63) 和 no *integer_type_suffix*或*integer_type_suffix* `L`或`l`會顯示為一元減號運算子語彙基元的正後方的語彙基元 ([一元減號運算子](expressions.md#unary-minus-operator))，結果是類型的常數`long`值-9223372036854775808 (-2 ^63)。 在其他情況下，這類*decimal_integer_literal*別的`ulong`。

#### <a name="real-literals"></a>實數常值

實數常值用來寫入類型的值`float`， `double`，和`decimal`。

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

如果沒有*real_type_suffix*指定，實數常值的型別是`double`。 否則為實數類型後置詞決定實數常值，型的別，如下所示：

*  實數常值加上`F`或是`f`別的`float`。 例如，常值`1f`， `1.5f`， `1e10f`，和`123.456F`類型的所有`float`。
*  實數常值加上`D`或是`d`別的`double`。 例如，常值`1d`， `1.5d`， `1e10d`，和`123.456D`類型的所有`double`。
*  實數常值加上`M`或是`m`別的`decimal`。 例如，常值`1m`， `1.5m`， `1e10m`，和`123.456M`類型的所有`decimal`。 這個常值轉換成`decimal`值所採取的確切的值，如有必要，數值四捨五入成最接近的值，可顯示使用銀行進位 ([decimal 型別](types.md#the-decimal-type))。 除非此值會無條件或值為零 （在此後者的情況下正負號和小數位數會是 0），則會保留任何明顯常值中的小數點位數。 因此，常值`2.900m`將會剖析為使用正負號的十進位數值`0`，係數`2900`，和小數位數`3`。

如果指定的常值無法表示成指定型別，就會發生編譯時期錯誤。

實數常值類型的值`float`或`double`取決於使用 IEEE"捨入為最接近 」 模式。

請注意，在實數常值、 十進位數字一向是必要的小數點後。 例如，`1.3F`是實際常值，但`1.F`不是。

#### <a name="character-literals"></a>字元常值

字元常值代表單一字元，而且通常包含的字元以引號括住，如`'a'`。

注意:ANTLR 文法標記法可讓下列令人混淆 ！ 在 ANTLR，當您撰寫`\'`它代表單引號`'`。 當您撰寫並`\\`它代表單一反斜線`\`。 因此它的開頭的單引號字元，然後單引號字元常值的第一個規則所表示。 和十一個可能的簡單逸出序列`\'`， `\"`， `\\`， `\0`， `\a`， `\b`， `\f`， `\n`， `\r`， `\t`， `\v`.

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

接下來的反斜線字元的字元 (`\`) 中*字元*必須是下列字元： `'`， `"`， `\`， `0`， `a`， `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`. 否則，會發生編譯時期錯誤。

十六進位逸出序列表示單一 Unicode 字元，後面的十六進位數字的格式值"`\x`」。

字元常值所代表的值是否大於`U+FFFF`，就會發生編譯時期錯誤。

Unicode 字元的逸出序列 ([Unicode 字元的逸出序列](lexical-structure.md#unicode-character-escape-sequences)) 中的字元常值必須在範圍內`U+0000`至`U+FFFF`。

簡單逸出序列表示 Unicode 字元編碼方式 」 下, 表中所述。


| __逸出序列__ | __字元的名稱__ | __Unicode 編碼__ |
|---------------------|--------------------|----------------------|
| `\'`                | 單引號       | `0x0027`             | 
| `\"`                | 雙引號       | `0x0022`             | 
| `\\`                | 反斜線          | `0x005C`             | 
| `\0`                | Null               | `0x0000`             | 
| `\a`                | 警示              | `0x0007`             | 
| `\b`                | 退格鍵          | `0x0008`             | 
| `\f`                | 換頁字元          | `0x000C`             | 
| `\n`                | 換行           | `0x000A`             | 
| `\r`                | 歸位字元    | `0x000D`             | 
| `\t`                | 水平 Tab     | `0x0009`             | 
| `\v`                | 垂直 Tab       | `0x000B`             | 

型別*character_literal*是`char`。

#### <a name="string-literals"></a>字串常值

C# 支援兩種形式的字串常值：***規則字串常值***並***逐字字串常值***。

規則字串常值包含零個或多個字元以住雙引號括住，如`"hello"`，而且可能包含兩個簡單的逸出序列 (例如`\t` 索引標籤上的字元)，和十六進位和 Unicode 逸出序列。

逐字字串常值組成`@`字元後面的雙引號字元、 零或多個字元，以及右雙引號字元。 是一個簡單的例子`@"hello"`。 逐字字串常值，在分隔符號之間的字元逐字解譯，唯一的例外狀況正在*quote_escape_sequence*。 特別是，不會處理簡單的逸出序列和十六進位和 Unicode 逸出序列加上逐字字串常值中。 逐字字串常值可能會跨越多行。

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

接下來的反斜線字元的字元 (`\`) 中*regular_string_literal_character*必須是下列字元： `'`， `"`， `\`， `0`， `a`, `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`. 否則，會發生編譯時期錯誤。

此範例
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
顯示各種不同的字串常值。 最後一個的字串常值， `j`，為逐字字串常值跨越多行。 加上引號，包括新行字元，例如泛空白字元的字元都會逐字。

由於十六進位逸出序列可能會有變動數目的十六進位數字、 字串常值`"\x123"`包含單一字元十六進位值為 123。 若要建立包含具有十六進位值為 12，後面接著字元 3 的字元的字串，其中可以撰寫`"\x00123"`或`"\x12" + "3"`改。

型別*string_literal*是`string`。

每個字串常值不會不一定會導致新的字串執行個體。 兩個或多個字串常值的相等時會根據字串等號比較運算子 ([字串等號比較運算子](expressions.md#string-equality-operators)) 會出現在相同的程式中，這些字串常值參考相同的字串執行個體。 比方說，所產生的輸出
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
是`True`因為兩個常值參考相同的字串執行個體。

#### <a name="interpolated-string-literals"></a>字串插值常值

字串插值常值字串常值，類似，但包含分隔的漏洞`{`和`}`，其中運算式可能會發生。 在執行階段，會評估運算式，目的是讓其文字的形式，將替代至漏洞發生的所在位置的字串。 語法和語意的字串內插補點一節所述 ([插補字串](expressions.md#interpolated-strings))。

字串常值，例如字串插值常值可以是一般或逐字規範。 內插補點的規則字串常值以分隔`$"`並`"`，且由分隔逐字字串插值常值`$@"`和`"`。

其他常值，例如插入的字串常值的語彙分析一開始會產生單一語彙基元，根據下列文法。 不過，語法的分析，插入的字串常值的單一權杖分為封閉式漏洞，字串的各部分的數個權杖前後所發生的漏洞之輸入項目的語彙再次分析。 接著，這可能會產生更插補的字串常值，以進行處理，但是，如果語彙修正，最後會導致一連串的語彙基元的語法分析來處理。

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

*Interpolated_string_literal*語彙基元會解譯為多個權杖和其他輸入項目，如下所示，在訂單中項目的*interpolated_string_literal*:

* 下列項目會解譯為不同的個別語彙基元： 前置`$`號*interpolated_regular_string_whole*， *interpolated_regular_string_start*，*interpolated_regular_string_mid*， *interpolated_regular_string_end*， *interpolated_verbatim_string_whole*， *interpolated_verbatim_string_start*， *interpolated_verbatim_string_mid*並*interpolated_verbatim_string_end*。
* 出現次數*regular_balanced_text*並*verbatim_balanced_text*兩者之間會重新處理成*input_section* ([語彙分析](lexical-structure.md#lexical-analysis)) 和會解譯為結果序列的輸入項目。 接著，這些可能包含重新解譯的字串插值常值語彙基元。

語法分析將會重新合併到語彙基元*interpolated_string_expression* ([插補字串](expressions.md#interpolated-strings))。

範例待辦事項


#### <a name="the-null-literal"></a>Null 常值

```antlr
null_literal
    : 'null'
    ;
```

*Null_literal*可以隱含地轉換為參考型別或可為 null 的型別。

### <a name="operators-and-punctuators"></a>運算子和標點符號

有數種運算子和標點符號。 運算式中使用運算子來說明涉及一或多個運算元的作業。 例如，運算式`a + b`會使用`+`運算子，將兩個運算元`a`和`b`。 標點符號會分組和區隔。

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

中的直條*right_shift*並*right_shift_assignment*生產用來表示，不同於其他生產中的語法的文法，任何種類的任何字元 （甚至不空白字元） 允許語彙基元之間。 這些實際執行的處理特別，以便能夠正確處理*type_parameter_list*s ([型別參數](classes.md#type-parameters))。

## <a name="pre-processing-directives"></a>前置處理指示詞

前置處理指示詞可讓您有條件地略過原始程式檔，以報告錯誤和警告狀況的區段，並區分的來源程式碼的特定區域。 「 前置處理指示詞 」 一詞只適用於 C 和 c + + 程式設計語言與一致性。 在 C# 中，沒有任何個別的前置處理步驟;語彙分析階段期間，會處理前置處理指示詞。

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

可使用下列的前置處理指示詞：

*  `#define` 並`#undef`，這用來定義和未定義，分別，條件式編譯符號 ([宣告指示詞](lexical-structure.md#declaration-directives))。
*  `#if``#elif`， `#else`，並`#endif`，用來有條件地略過的原始程式碼區段 ([條件式編譯指示詞](lexical-structure.md#conditional-compilation-directives))。
*  `#line`用以控制發出的錯誤和警告的行號 ([行指示詞](lexical-structure.md#line-directives))。
*  `#error` 並`#warning`，用來分別發出錯誤和警告，([診斷指示詞](lexical-structure.md#diagnostic-directives))。
*  `#region` 並`#endregion`，用來明確地標示的原始程式碼區段 ([區域指示詞](lexical-structure.md#region-directives))。
*  `#pragma`用以指定至編譯器的選擇性內容資訊 ([Pragma 指示詞](lexical-structure.md#pragma-directives))。

前置處理指示詞一定會佔用一行的來源程式碼，並一律以開頭`#`字元和前置處理指示詞的名稱。 之前的空格可能會發生`#`字元之間及`#`字元和指示詞的名稱。

包含來源線條`#define`， `#undef`， `#if`， `#elif`， `#else`， `#endif`， `#line`，或`#endregion`指示詞結尾的單行註解。 分隔註解 (`/* */`樣式的註解) 不允許使用包含前置處理指示詞的原始程式行。

前置處理指示詞沒有權杖，而且不是 C# 的語法文法的一部分。 不過，前置處理指示詞可用來包含或排除的語彙基元序列，而且會以該方式影響 C# 程式的意義。 例如，當編譯時，程式：
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
確切的相同順序的語彙基元做為程式的結果：
```csharp
class C
{
    void F() {}
    void I() {}
}
```

因此，而語彙，這兩個程式是很不一樣，在語法上，它們完全相同。

### <a name="conditional-compilation-symbols"></a>條件式編譯符號

所提供的條件式編譯功能`#if`， `#elif`， `#else`，以及`#endif`指示詞透過預先處理運算式 ([預先處理運算式](lexical-structure.md#pre-processing-expressions))和條件式編譯符號。

```antlr
conditional_symbol
    : '<Any identifier_or_keyword except true or false>'
    ;
```

條件式編譯符號有兩個可能的狀態：***定義***或是***未定義***。 在原始程式檔的語彙處理開始，除非它已經由外部機制 （例如命令列編譯器選項） 來明確定義，是未定義條件式編譯符號。 當`#define`處理指示詞時，該指示詞命名的條件式編譯符號會變成已定義在該原始程式檔中。 符號會保持其定義之前`#undef`指示詞會處理該相同的符號，或是來源檔案的結尾為止。 含意在於`#define`和`#undef`一個原始程式檔中的指示詞不影響相同程式中其他原始程式檔。

定義的條件式編譯符號前置處理的運算式中參考時，具有布林值`true`，而且未定義的條件式編譯符號，則為 true 的值`false`。 不需要在預先處理運算式中參考這些條件式編譯符號先明確宣告。 相反地，未宣告的符號不會直接定義，且因此該值`false`。

條件式編譯符號的命名空間是相異，且不同於 C# 程式中的其他所有具名實體。 條件式編譯符號只能參考中`#define`和`#undef`指示詞在前置處理的運算式。

### <a name="pre-processing-expressions"></a>前置處理的運算式

前置處理運算式可以出現在`#if`和`#elif`指示詞。 運算子`!`， `==`， `!=`，`&&`和`||`預先處理運算式中允許和括號可能會用來分組。

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

定義的條件式編譯符號前置處理的運算式中參考時，具有布林值`true`，而且未定義的條件式編譯符號，則為 true 的值`false`。

一律會前置處理的運算式的評估會產生布林值。 前置處理的運算式評估的規則是一樣的常數運算式 ([常數運算式](expressions.md#constant-expressions))，但使用者定義實體可以被參考的條件式編譯符號.

### <a name="declaration-directives"></a>宣告指示詞

宣告指示詞用來定義或取消定義條件式編譯符號。

```antlr
pp_declaration
    : whitespace? '#' whitespace? 'define' whitespace conditional_symbol pp_new_line
    | whitespace? '#' whitespace? 'undef' whitespace conditional_symbol pp_new_line
    ;

pp_new_line
    : whitespace? single_line_comment? new_line
    ;
```

處理`#define`指示詞會導致變成已定義，指定的條件式編譯符號開頭指示詞之後的原始程式行。 同樣地，在處理`#undef`指示詞會讓指定的條件式編譯符號會變成未定義，並遵循指示詞的原始程式行以開始。

任何`#define`並`#undef`原始程式檔中的指示詞必須出現之前，先*語彙基元*([權杖](lexical-structure.md#tokens)) 在來源檔案中; 否則編譯時期錯誤就會發生。 簡單來說，`#define`和`#undef`指示詞必須在任何 「 實際程式碼 」 之前的原始程式檔中。

範例：
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
無效，因為`#define`指示詞前面的第一個語彙基元 (`namespace`關鍵字) 中的原始程式檔。

下列範例會產生編譯時期錯誤，因為`#define`遵循真正的程式碼：
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

A`#define`可能會定義條件式編譯符號已定義，而不需要那里任何中介`#undef`該符號。 以下範例定義的條件式編譯符號`A`，然後再次定義。
```csharp
#define A
#define A
```

A`#undef`可能 「 取消 」 未定義的條件式編譯符號。 以下範例定義的條件式編譯符號`A`雖然然後取消它兩次; 定義和第二個`#undef`沒有任何作用，它是否仍然有效。
```csharp
#define A
#undef A
#undef A
```

### <a name="conditional-compilation-directives"></a>條件式編譯指示詞

條件式編譯指示詞可有條件地包含或排除的原始程式檔部分。

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

如語法所示，條件式編譯指示詞必須寫成集組成，依序`#if`指示詞，零或多個`#elif`指示詞、 零或一個`#else`指示詞，和`#endif`指示詞。 指示詞之間是原始程式碼的條件式區段。 每個區段會受到正前面的指示詞。 條件式區段可能本身包含巢狀的條件式編譯指示詞提供這些指示詞會形成完整的設定。

A *pp_conditional*選取最多一個內含*conditional_section*語彙處理 s:

*  *Pp_expression*之`#if`並`#elif`指示詞的評估順序，直到其中一個會產生`true`。 如果運算式會產生`true`，則*conditional_section*選取對應的指示詞。
*  如果要將所有*pp_expression*s yield `false`，而且如果`#else`指示詞已存在， *conditional_section*的`#else`指示詞已選取。
*  否則為否*conditional_section*已選取。

所選*conditional_section*，如果任何項目，都處理為一般*input_section*： 一節中所包含的原始程式碼必須遵守語彙文法; 從來源產生權杖區段; 中的程式碼還有一節中的前置處理指示詞指定的效果。

其餘*conditional_section*視為處理 s，如果有的話*skipped_section*除了前置處理指示詞 s，來源中的程式碼區段需要遵守語彙文法，否從來源中的程式碼區段中; 產生權杖和一節中的前置處理指示詞必須是正確的語彙，但不是會否則處理。 內*conditional_section* ，為正在處理*skipped_section*任何巢狀*conditional_section*s (包含在巢狀`#if`...`#endif`和`#region`...`#endregion`建構) 也會做為處理*skipped_section*s。

下列範例說明如何在條件式編譯指示詞可以巢狀：
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

前置處理指示詞，除了已略過的原始碼不受限於語彙分析。 例如，以下是儘管未結束的註解中有效`#else`區段：
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

不過請注意，前置處理指示詞所需的原始碼的略過區段中，甚至是語彙正確。

出現在多行輸入項目內時，不會處理前置處理指示詞。 例如，此方案：
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
在輸出中的結果：
```
hello,
#if Debug
        world
#else
        Nebraska
#endif
```

在罕見的情況下，處理前置處理指示詞的集合可能會取決於評估*pp_expression*。 範例：
```csharp
#if X
    /*
#else
    /* */ class Q { }
#endif
```
一律會產生相同的語彙基元資料流 (`class` `Q` `{` `}`)，而不論是否`X`定義。 如果`X`是定義，是唯一的處理指示詞`#if`和`#endif`，因為多行註解。 如果`X`是未定義，然後三個指示詞 (`#if`， `#else`， `#endif`) 會指示詞組的一部分。

### <a name="diagnostic-directives"></a>診斷指示詞

診斷指示詞用來明確地產生錯誤和警告訊息中其他的編譯時間錯誤和警告相同的方式所報告。

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

範例：
```csharp
#warning Code review needed before check-in

#if Debug && Retail
    #error A build can't be both debug and retail
#endif

class Test {...}
```
一律會產生警告 （「 程式碼檢閱需要簽入之前 」），並會產生編譯時期錯誤 （「 組建不能是零售和偵錯 」） 如果條件式符號`Debug`和`Retail`兩者都已定義。 請注意， *pp_message*可以包含任意文字; 具體來說，它不需要包含語式正確的語彙基元，單引號的文字中所示`can't`。

### <a name="region-directives"></a>區域指示詞

區域指示詞用來明確標記的原始碼的區域。

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

任何語意意義被不附加到區域;區域被專供程式設計師，或透過自動化工具來標示的原始程式碼區段。 中指定的訊息`#region`或`#endregion`指示詞同樣沒有語意意義; 它只是可用來識別區域。 比對`#region`並`#endregion`指示詞可能會具有不同*pp_message*s。

區域語彙處理：
```csharp
#region
...
#endregion
```
就相當於語彙處理表單的條件式編譯指示詞：
```csharp
#if true
...
#endif
```

### <a name="line-directives"></a>Line 指示詞

Line 指示詞可用來變更的行號和原始程式檔名稱，例如警告和錯誤輸出中，編譯器會報告和所使用的呼叫端資訊屬性 ([呼叫端資訊屬性](attributes.md#caller-info-attributes))。

從輸入的一些其他文字產生 C# 原始程式碼的中繼程式設計工具中最常使用 line 指示詞。

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

若未`#line`指示詞存在，則編譯器會報告，則為 true 的行號和在其輸出中的原始程式檔名稱。 處理時`#line`指示詞，其中包含*line_indicator*不是`default`，編譯器會為具有指定的行號 （和檔案名稱，如果指定） 指示詞之後的行。

A`#line default`指示詞會反轉所有上述 #line 指示詞的效果。 編譯器會精確地說，如果沒有報告接下來的幾行，則為 true 的行資訊`#line`有處理指示詞。

A`#line hidden`指示詞沒有任何作用，檔案和行號報告錯誤訊息，但是會影響來源層級偵錯。 偵錯時，所有行之間`#line hidden`指示詞和後續`#line`指示詞 (不是`#line hidden`) 有沒有行號資訊。 時逐步執行偵錯工具中的程式碼，將會完全略過這些行。

請注意， *file_name*不同於一般的字串常值，不會處理逸出字元; 「`\`"字元只可指定在一般的反斜線字元*file_name*.

### <a name="pragma-directives"></a>Pragma 指示詞

`#pragma`前置處理器指示詞用來指定選擇性的內容資訊給編譯器。 中提供的資訊`#pragma`指示詞永遠不會變更程式的語意。

```antlr
pp_pragma
    : whitespace? '#' whitespace? 'pragma' whitespace pragma_body pp_new_line
    ;

pragma_body
    : pragma_warning_body
    ;
```

C# 提供`#pragma`指示詞，可控制編譯器警告。 語言的未來版本可能包含額外`#pragma`指示詞。 若要確保與其他 C# 編譯器的互通性，Microsoft C# 編譯器不會發出未知的編譯錯誤`#pragma`指示詞; 這類指示詞不過產生警告。

#### <a name="pragma-warning"></a>Pragma 警告

`#pragma warning`指示詞用來停用或還原所有或後續的程式文字編譯期間的一組特定的警告訊息。

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

A`#pragma warning`省略警告清單的指示詞會影響所有警告。 A`#pragma warning`指示詞包含警告清單會影響的只在清單中指定的警告。

A`#pragma warning disable`指示詞會停用所有或一組指定的警告。

A`#pragma warning restore`指示詞還原所有或一組指定的警告，已在作用中的編譯單位開始處的狀態。 請注意，如果特定的警告已停用外部、 `#pragma warning restore` (是否所有或特定的警告) 並不會重新啟動該警告。

下列範例示範使用`#pragma warning`暫時停用警告時，報告而言已經過時參考成員，使用 Microsoft C# 編譯器的警告編號。
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
