---
ms.openlocfilehash: 4676bcd3f0a92260b4e5e20a0aa5b5ec00bf204e
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704064"
---
# <a name="lexical-structure"></a>語彙結構

## <a name="programs"></a>Programs

C# ***程式***是由一或多個***原始***程式檔所組成，已知正式為***編譯單位***（[編譯單位](namespaces.md#compilation-units)）。 原始程式檔是 Unicode 字元的排序序列。 來源檔案通常與檔案系統中的檔案具有一對一的對應關係，但不需要這項對應。 若要獲得最大的可攜性，建議使用 UTF-8 編碼來編碼檔案系統中的檔案。

就概念上而言，程式是使用三個步驟來編譯：

1. 轉換，將檔案從特定字元大型和編碼配置轉換成 Unicode 字元序列。
2. 詞法分析，會將 Unicode 輸入字元的資料流程轉譯為權杖串流。
3. 語法分析，會將權杖串流轉譯成可執行檔程式碼。

## <a name="grammars"></a>語法

此規格會使用兩個文法C#來呈現程式設計語言的語法。 「***詞法文法***」（[詞法](lexical-structure.md#lexical-grammar)文法）定義如何將 Unicode 字元結合成表單行結束字元、空白字元、批註、標記和前置處理指示詞。 ***語法文法***（[語法文法](lexical-structure.md#syntactic-grammar)）定義如何將詞彙文法產生的標記結合成表單C#程式。

### <a name="grammar-notation"></a>文法標記法

詞法和語法文法會使用 ANTLR 文法工具的標記法，以巴克斯 Backus-naur 形式呈現。

### <a name="lexical-grammar"></a>語彙文法

的詞法文法會C#以[詞法分析](lexical-structure.md#lexical-analysis)、[標記](lexical-structure.md#tokens)和[前置處理](lexical-structure.md#pre-processing-directives)指示詞來呈現。 詞法文法的終端符號是 Unicode 字元集的字元，而詞法文法會指定如何將字元結合成表單標記（[標記](lexical-structure.md#tokens)）、空白字元（[空白字元](lexical-structure.md#white-space)）、批註（[批註](lexical-structure.md#comments)）和前置處理指示詞（[前置處理指示](lexical-structure.md#pre-processing-directives)詞）。

程式中的每個來源檔案都必須符合詞法文法的*輸入*生產（[詞法分析](lexical-structure.md#lexical-analysis)）。 C#

### <a name="syntactic-grammar"></a>語法文法

的語法文法C#會在本章後續的章節和附錄中提供。 語法文法的終端符號是由詞法文法所定義的標記，而語法文法則指定如何將標記結合成表單C#程式。

程式中的每個C#來源檔案都必須符合語法文法（[編譯單位](namespaces.md#compilation-units)）的*compilation_unit*實際執行。

## <a name="lexical-analysis"></a>詞法分析

*輸入*生產會定義C#原始檔的詞法結構。 程式中的C#每個來源檔案都必須符合此詞法文法生產。

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

五個基本元素組成C#原始檔的詞法結構：行結束字元（[行結束字元](lexical-structure.md#line-terminators)）、空白字元（[空白字元](lexical-structure.md#white-space)）、批註（[批註](lexical-structure.md#comments)）、標記（[標記](lexical-structure.md#tokens)）和前置處理指示詞（[前置處理](lexical-structure.md#pre-processing-directives)指示詞）。 在這些基本元素中，只有標記在C#程式的語法文法中是很重要的（[語法文法](lexical-structure.md#syntactic-grammar)）。

C#來源檔案的詞法處理包含將檔案縮減成一系列標記，以成為語法分析的輸入。 行結束字元、空白字元和批註可以用來分隔標記，而前置處理指示詞可能會導致略過原始C#程式檔的區段，但這些詞法元素也不會影響程式的語法結構。

在插入字串常值（插入[字串常](lexical-structure.md#interpolated-string-literals)值）的情況下，單一 token 一開始是由詞法分析所產生，但會細分為多個輸入元素，這些專案會重複受到詞法分析，直到所有插入已解析字串常值。 然後，產生的權杖會作為語法分析的輸入。

當數個詞法文法生產符合原始檔中的字元序列時，詞法處理一律會形成最長可能的詞法元素。 例如，字元序列`//`會當做單行批註的開頭來處理，因為該詞法元素的長度超過單一`/`標記。

### <a name="line-terminators"></a>行結束字元

行結束字元會將C#原始程式檔的字元分成幾行。

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

為了與可新增檔案結尾標記的原始程式碼編輯工具相容，並讓原始C#程式檔被視為一系列適當終止的行，下列轉換會依序套用到程式中的每個來源檔案：

*  如果來源檔案的最後一個字元是控制 z 字元（`U+001A`），就會刪除這個字元。
*  如果原始程式檔不是`U+000D`空的，而且原始程式檔的最後一個字元不是回車（`U+000D`）、換行（`U+000A`）、分行符號（）、分行符號（），則會將換行`U+2028`字元（）加入來源檔案的結尾。）或段落分隔符號（`U+2029`）。

### <a name="comments"></a>註解

支援兩種形式的批註：單行批註和分隔批註。 ***單行批註***會以字元`//`開頭，並延伸至原始程式列的結尾。 ***分隔的批註***會以字元`/*`開頭，並以字元`*/`結尾。 分隔的批註可能會跨越多行。

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

批註不會進行嵌套。 `/*`字元序列`/*`和`*/`在`//`批註中沒有特殊意義，而且字元順序`//`和在分隔的批註中沒有特殊意義。

批註不會在字元和字串常值中處理。

範例
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
包含分隔的批註。

範例
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
顯示數個單行批註。

### <a name="white-space"></a>空白字元

空白字元會定義為具有 Unicode 類別 Zs 的任何字元（包含空白字元），以及水準定位字元、垂直定位字元和表單摘要字元。

```antlr
whitespace
    : '<Any character with Unicode class Zs>'
    | '<Horizontal tab character (U+0009)>'
    | '<Vertical tab character (U+000B)>'
    | '<Form feed character (U+000C)>'
    ;
```

## <a name="tokens"></a>語彙基元

有數種類型的權杖：識別碼、關鍵字、常值、運算子和標點符號。 空白字元和批註不是標記，但它們會當做標記的分隔符號。

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

### <a name="unicode-character-escape-sequences"></a>Unicode 字元逸出序列

Unicode 字元逸出序列代表 Unicode 字元。 Unicode 字元逸出序列會在識別碼（[識別碼](lexical-structure.md#identifiers)）、字元常值（[字元常](lexical-structure.md#character-literals)值）和一般字串常值（[字串常](lexical-structure.md#string-literals)值）中處理。 Unicode 字元 escape 不會在任何其他位置處理（例如，形成 operator、標點符號或關鍵字）。

```antlr
unicode_escape_sequence
    : '\\u' hex_digit hex_digit hex_digit hex_digit
    | '\\U' hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit
    ;
```

Unicode 逸出序列代表以 "`\u`" 或 "`\U`" 字元後面的十六進位數位所形成的單一 unicode 字元。 由於C#會在字元和字串值中使用 Unicode 程式碼點的16位編碼，因此字元常值中不允許使用 u + 10000 到 U + 10ffff 且範圍內的 unicode 字元，而且會在字串常值中以 Unicode 代理配對來表示。 不支援使用高於0x10FFFF 之程式碼點的 Unicode 字元。

不會執行多個翻譯。 例如，字串常值 "`\u005Cu005C`" 相當於 "`\u005C`"，而不`\`是 ""。 Unicode 值`\u005C`是字元 "`\`"。

範例
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
顯示數個的`\u0066`用法，這是字母 "`f`" 的轉義順序。 程式相當於
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

本節中所提供的識別碼規則與 Unicode 標準附錄31所建議的規則完全對應，不同之處在于允許使用底線做為初始字元（如同 C 程式設計語言的傳統），Unicode 逸出序列是允許在識別碼中使用，並`@`允許 "" 字元作為前置詞，以啟用關鍵字做為識別碼。

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

如需前述 Unicode 字元類別的詳細資訊，請參閱 Unicode 標準版本3.0，第4.5 節。

有效識別碼的範例包括 "`identifier1`"、"`_identifier2`" 和 "`@if`"。

符合規範之程式中的識別碼必須是 Unicode 正規化表單 C 所定義的標準格式，如 Unicode Standard 附錄15所定義。 當發現不是正規化格式 C 的識別碼時，其行為是由執行定義;不過，不需要進行診斷。

前置詞 "`@`" 可讓您使用關鍵字做為識別碼，這在與其他程式設計語言互動時非常有用。 字元`@`實際上並不是識別碼的一部分，因此可能會在其他語言中看到此識別碼做為一般識別碼，但不含前置詞。 具有`@`前置詞的識別碼稱為***逐字識別碼***。 允許不是`@`關鍵字之識別碼的前置詞，但強烈建議您不要使用樣式。

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
定義名為 "`class`" 的類別，其具有`static`名為 "" 的靜態方法，其`bool`採用名為 "" 的參數。 請注意，因為關鍵字中不允許使用 Unicode 轉義，所以 token`cl\u0061ss`"" 是識別碼，而與 "`@class`" 的識別碼相同。

如果在套用下列轉換之後，將兩個識別碼視為相同，則順序如下：

*  已移除前置`@`詞 "" （如果使用的話）。
*  每個*unicode_escape_sequence*都會轉換成其對應的 unicode 字元。
*  所有*formatting_character*都已移除。

包含兩個連續底線字元（`U+005F`）的識別碼會保留供實作為使用。 例如，執行可能會提供以兩個底線開頭的擴充關鍵字。

### <a name="keywords"></a>關鍵字

***關鍵字***是保留的類似識別碼的字元序列，不能用來做為識別碼，除非前面加`@`上字元。

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

在文法的某些位置中，特定識別碼具有特殊意義，但不是關鍵字。 這類識別碼有時稱為「內容關鍵字」。 例如，在屬性宣告中，"`get`" 和 "`set`" 識別碼具有特殊意義（[存取](classes.md#accessors)子）。 這些位置中絕不`get`允許`set`或以外的識別碼，因此此用法不會與使用這些單字做為識別碼衝突。 在其他情況下，例如在隱含類型區域變數`var`宣告（[區域變數](statements.md#local-variable-declarations)宣告）中使用識別碼 ""，內容關鍵字可能會與宣告的名稱衝突。 在這種情況下，宣告的名稱優先于使用識別碼做為內容關鍵字。

### <a name="literals"></a>常值

***常***值是值的原始程式碼表示。

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

有兩個布林常值： `true`和`false`。

```antlr
boolean_literal
    : 'true'
    | 'false'
    ;
```

*Boolean_literal*的類型為 `bool`。

#### <a name="integer-literals"></a>整數常值

整數常值是用`int`來寫入`long`、 `uint`、和`ulong`類型的值。 整數常值有兩種可能的形式： decimal 和十六進位。

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

整數常值的類型是依照下列方式決定：

*  如果常值沒有後置詞，則會有下列類型的第一個，其中可以表示其值`int`： `uint`、 `long`、 `ulong`、。
*  如果常值是`U`以或`u`做為後置字元，它會有下列類型的第一個，其中可以表示`ulong`其值： `uint`、。
*  如果常值是`L`以或`l`做為後置字元，它會有下列類型的第一個，其中可以表示`ulong`其值： `long`、。
*  如果常值的後置`UL`字元`Ul`是`uL`、 `ul`、 `LU`、 `Lu`、 `lU`、、 `lu`或，則其類型`ulong`為。

如果整數常值所代表的值超出`ulong`類型的範圍，就會發生編譯時期錯誤。

就樣式而言，撰寫類型`L` `long`的常值時，建議使用 ""，而`l`不是 ""，因為很容易就能將字母 "`l`" 與數位 "`1`" 混淆。

若要允許最小`int`的`long`可能和值寫入為十進位整數常值，則會有下列兩個規則：

* 當*decimal_integer_literal*的值為2147483648（2 ^ 31）且沒有*integer_type_suffix*顯示為緊接在一元減號運算子標記（[一元減號運算子](expressions.md#unary-minus-operator)）後面的 token 時，結果會是類型 `int` 的常數，其值為-2147483648 （-2 ^ 31）。 在所有其他情況下，這類*decimal_integer_literal*的類型 `uint`。
* 當值為9223372036854775808（2 ^ 63）且沒有*integer_type_suffix*或*integer_type_suffix* `L` 或 `l` 的*decimal_integer_literal*出現為緊接在一元減號運算子 token 後面的 token （[一元減號運算子](expressions.md#unary-minus-operator)），結果會是類型 `long` 的常數，其值為-9223372036854775808 （-2 ^ 63）。 在所有其他情況下，這類*decimal_integer_literal*的類型 `ulong`。

#### <a name="real-literals"></a>實際常值

Real 常值是用來寫入、 `float` `double`和`decimal`類型的值。

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

如果未指定*real_type_suffix* ，則實際常值的類型為 `double`。 否則，real 類型尾碼會決定實際常值的類型，如下所示：

*  `F` `float`或的實數常值為類型`f` 。 例如`1f`，常`float`值`1.5f`、、和`123.456F`都是型別。 `1e10f`
*  `D` `double`或的實數常值為類型`d` 。 例如`1d`，常`double`值`1.5d`、、和`123.456D`都是型別。 `1e10d`
*  `M` `decimal`或的實數常值為類型`m` 。 例如`1m`，常`decimal`值`1.5m`、、和`123.456M`都是型別。 `1e10m` 這個常值會藉由`decimal`取得精確的值來轉換成值，並在必要時，使用四進位的舍入（[decimal 類型](types.md#the-decimal-type)）來四捨五入到最接近的可顯示值。 除非將值舍入或值為零（在後者的情況下，正負號和小數值會是0），否則會保留常值中明顯的任何尺規。 因此，會剖析`2.900m`常值以形成具有正負號`0`、係數`2900`和小`3`數位數的 decimal。

如果指定的常值無法以指示的類型表示，就會發生編譯時期錯誤。

類型`float` 或`double`的實數常值，是使用 IEEE 「舍入至最接近」模式來決定。

請注意，在實際常值中，小數點後面一律需要十進位數。 例如，是`1.3F`實數常值，但`1.F`不是。

#### <a name="character-literals"></a>字元常值

字元常值代表單一字元，通常是由引號中的字元所組成，如中`'a'`所示。

注意：ANTLR 文法標記法會造成下列混淆！ 在 ANTLR 中，當您`\'`撰寫時，會代表單一`'`引號。 當您撰寫`\\`時，它代表單一反斜線`\`。 因此，字元常值的第一個規則就是以單引號開頭，然後輸入一個字元，再加上一個單引號。 而11個可能的簡單逸出序列`\'`包括`\"`、 `\\`、 `\0`、 `\a`、 `\b`、 `\f`、 `\n` 、、`\t`、、 `\r` `\v`.

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

在字元中後面接著反斜線字元`\`（）的字元必須是下列其中一個字元： `'`、 `a` `"`、 `\`、 `0`、、、 `f` `b`, `n`, `r`, `t`, `u`, `U`, `x`, `v`. 否則，會發生編譯時期錯誤。

十六進位的逸出序列代表單一 Unicode 字元，其值是由 "`\x`" 後面的十六進位數位所組成。

如果字元常值所代表的值大於`U+FFFF`，就會發生編譯時期錯誤。

字元常值中的 unicode 字元逸出序列（[unicode 字元逸出序列](lexical-structure.md#unicode-character-escape-sequences)）必須在到`U+0000` `U+FFFF`的範圍內。

簡單的逸出序列代表 Unicode 字元編碼，如下表所述。


| __Escape 序列__ | __字元名稱__ | __Unicode 編碼__ |
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

*Character_literal*的類型為 `char`。

#### <a name="string-literals"></a>字串常值

C#支援兩種格式的字串常值：***一般字串常***值和***逐字字串常***值。

一般字串常值是由以雙引號括住的零或多個字元所`"hello"`組成，如所示，而且可能包含簡單`\t`的逸出序列（例如，用於定位字元）和十六進位和 Unicode 逸出序列。

逐字字串常值包含一個`@`字元，後面接著雙引號字元、零或多個字元，以及右雙引號字元。 一個簡單的範例`@"hello"`是。 在逐字字串常值中，分隔符號之間的字元會逐字轉譯，唯一的例外狀況是*quote_escape_sequence*。 特別的是，簡單的逸出序列和十六進位和 Unicode 逸出序列不會在逐字字串常值中處理。 逐字字串常值可能會跨越多行。

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

*Regular_string_literal_character*中的反斜線字元（`\`）後面的字元必須是下列其中一個字元： `'`、`"`、`\`、`0`、`a`、`b`、`f`、`n`、0、1、2、3、4、5。 否則，會發生編譯時期錯誤。

範例
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
顯示各種不同的字串常值。 最後一個字串常`j`值是跨越多行的逐字字串常值。 引號之間的字元（包括分行符號之類的空白字元）會逐字保留。

由於十六進位的逸出序列可以有可變數目的十六進位數位，字串常`"\x123"`值會包含具有十六進位值123的單一字元。 若要建立字串，其中包含十六進位值12後面接著字元3的字元，則可以改`"\x00123"`為`"\x12" + "3"`寫入或。

*String_literal*的類型為 `string`。

每個字串常值不一定會產生新的字串實例。 當兩個或多個根據字串等號比較運算子（[字串等號比較](expressions.md#string-equality-operators)運算子）相等的字串常值出現在相同的程式中時，這些字串常值會參考相同的字串實例。 例如，產生的輸出是由
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
是`True` ，因為這兩個常值會參考相同的字串實例。

#### <a name="interpolated-string-literals"></a>內插字串常值

內插字串常值類似于字串常值，但包含以`{`和`}`分隔的洞，也就是運算式。 在執行時間，系統會評估運算式，其目的是要將其文字形式替換成出現洞處的字串。 字串插補的語法和語義會在區段（[字串插值](expressions.md#interpolated-strings)）中說明。

如同字串常值，插入字串常值可以是一般或逐字。 內插的一般字串常值`$"`是`"`以和分隔，而內插逐字字串`$@"`常`"`值則是以和分隔。

就像其他常值一樣，插入字串常值的詞法分析一開始會產生單一權杖，如以下文法所示。 不過，在語法分析之前，插入字串常值的單一 token 會針對部分包含洞的字串，分成數個標記，而出現在洞孔中的輸入元素會再次分析。 這可能會產生更多要處理的插入字串常值，但如果詞法正確，最後會導致一系列標記，以供語法分析處理。

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

*Interpolated_string_literal* token 會依照*interpolated_string_literal*中的出現順序，以多個權杖和其他輸入元素的方式向量：

* 下列專案的出現次數會向量為不同的個別權杖：前置的 `$` 號、 *interpolated_regular_string_whole*、 *interpolated_regular_string_start*、 *interpolated_regular_string_mid*、 *interpolated_regular_string_end*、 *interpolated_verbatim_string_whole*、 *interpolated_verbatim_string_start*、 *interpolated_verbatim_string_mid*和*interpolated_verbatim_string_end*。
* 在這些專案之間出現的*regular_balanced_text*和*verbatim_balanced_text*會重新處理為*input_section* （[詞法分析](lexical-structure.md#lexical-analysis)），並會向量為輸入元素的結果序列。 這些可能會輪流包含要向量的插入字串常值標記。

語法分析會將 token 重新合併為*interpolated_string_expression* （[字串插值](expressions.md#interpolated-strings)）。

待辦事項範例


#### <a name="the-null-literal"></a>Null 常值

```antlr
null_literal
    : 'null'
    ;
```

*Null_literal*可以隱含地轉換成參考型別或可為 null 的類型。

### <a name="operators-and-punctuators"></a>運算子和標點符號

有數種運算子和標點符號。 運算式中會使用運算子來描述涉及一或多個運算元的作業。 例如，運算式`a + b`會`+`使用運算子來加入兩個運算元`a`和`b`。 標點符號適用于分組和分隔。

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

*Right_shift*和*right_shift_assignment*生產中的分隔號是用來指出，與語法文法中的其他生產不同，標記之間不允許任何類型的字元（甚至不是空白字元）。 這些生產會特別處理，以啟用*type_parameter_list*s （[類型參數](classes.md#type-parameters)）的正確處理。

## <a name="pre-processing-directives"></a>前置處理指示詞

前置處理指示詞可讓您有條件地略過原始程式檔區段、報告錯誤和警告條件，以及描繪原始程式碼的不同區域。 「前置處理指示詞」一詞僅用於與 C 和C++程式設計語言一致性。 在C#中，沒有個別的前置處理步驟。前置處理指示詞會在詞法分析階段中處理。

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

下列是可用的前置處理指示詞：

*  `#define`和`#undef`，分別用來定義和取消定義條件式編譯[符號（宣告](lexical-structure.md#declaration-directives)指示詞）。
*  `#if`、 `#elif`、 `#else`和`#endif`，用來有條件地略過原始程式碼區段（條件式[編譯](lexical-structure.md#conditional-compilation-directives)指示詞）。
*  `#line`，用來控制針對錯誤和警告發出的行號（程式[行](lexical-structure.md#line-directives)指示詞）。
*  `#error`和`#warning`，分別用來發出錯誤和警告（[診斷](lexical-structure.md#diagnostic-directives)指示詞）。
*  `#region`和`#endregion`，用來明確地標記原始程式碼區段（[區域](lexical-structure.md#region-directives)指示詞）。
*  `#pragma`，用來指定編譯器的選擇性內容資訊（[Pragma](lexical-structure.md#pragma-directives)指示詞）。

前置處理指示詞一律會佔用個別的源程式碼，並且一律以`#`字元和前置處理指示詞名稱開頭。 空白字元可能會在`#`字元之前，以及`#`字元與指示詞名稱之間發生。

`#define`包含、、 、、`#elif` `#undef` `#if` 、、`#line`或指示詞的`#endregion`原始程式列，其結尾可能會是單行批註。 `#else` `#endif` 包含前置處理指示`/* */`詞的來源行上不允許使用分隔的批註（批註的樣式）。

前置處理指示詞不是標記，而且不是的語法文法的一部分C#。 不過，前置處理指示詞可以用來包含或排除權杖的順序，而這種方式可以影響C#程式的意義。 例如，在編譯時，程式：
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
會產生與程式完全相同的標記順序：
```csharp
class C
{
    void F() {}
    void I() {}
}
```

因此，雖然在詞法上，這兩個程式在語法上完全不同，但它們是相同的。

### <a name="conditional-compilation-symbols"></a>條件式編譯符號

`#if`、 、和`#else`指示詞所提供的條件式編譯功能，是透過前置處理運算式（[前置處理運算式](lexical-structure.md#pre-processing-expressions)）和條件式來控制`#endif` `#elif`編譯符號。

```antlr
conditional_symbol
    : '<Any identifier_or_keyword except true or false>'
    ;
```

條件式編譯符號有兩種可能的狀態：***已定義***或***未定義***。 在原始程式檔的詞法處理開始時，除非已由外部機制（例如命令列編譯器選項）明確定義，否則不會定義條件式編譯符號。 `#define`當處理指示詞時，該指示詞中名為的條件式編譯符號會在該原始程式檔中定義。 符號會保持定義，直到`#undef`處理該相同符號的指示詞，或直到到達原始程式檔結尾為止。 其含意是，一個原始`#define`程式`#undef`檔中的和指示詞不會影響相同程式中的其他來源檔案。

在前置處理運算式中參考時，定義的條件式編譯符號會有布林值`true`，且未定義的條件式編譯符號會有`false`布林值。 在前置處理運算式中參考條件式編譯符號之前，並不需要明確宣告。 相反地，未宣告的符號只是未定義的`false`，因此具有值。

條件式編譯符號的命名空間是不同的，而且與C#程式中所有其他已命名的實體不同。 條件式編譯符號只能在`#define`和`#undef`指示詞以及前置處理運算式中參考。

### <a name="pre-processing-expressions"></a>前置處理運算式

前置處理運算式可以出現在和`#if` `#elif`指示詞中。 前置處理`!`運算式中`!=`允許`&&`使用`||`運算子`==`、、和，而括弧可用於群組。

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

在前置處理運算式中參考時，定義的條件式編譯符號會有布林值`true`，且未定義的條件式編譯符號會有`false`布林值。

預先處理運算式的評估一律會產生布林值。 前置處理運算式的評估規則與常數運算式（[常數運算式](expressions.md#constant-expressions)）相同，不同之處在于唯一可以參考的使用者定義實體是條件式編譯符號。

### <a name="declaration-directives"></a>宣告指示詞

宣告指示詞是用來定義或取消定義條件式編譯符號。

```antlr
pp_declaration
    : whitespace? '#' whitespace? 'define' whitespace conditional_symbol pp_new_line
    | whitespace? '#' whitespace? 'undef' whitespace conditional_symbol pp_new_line
    ;

pp_new_line
    : whitespace? single_line_comment? new_line
    ;
```

處理`#define`指示詞會使指定的條件式編譯符號變成已定義，從指示詞後面的來源行開始。 同樣地，處理`#undef`指示詞會使指定的條件式編譯符號變成未定義，從指示詞後面的來源行開始。

來源`#define`檔案`#undef`中的任何和指示詞必須在來源檔案中的第一個*標記（[token](lexical-structure.md#tokens)* ）之前發生，否則會發生編譯時期錯誤。 在直覺的詞彙`#define`中`#undef` ，和指示詞必須在原始程式檔中的任何「實際程式碼」之前。

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
有效，因為`#define`指示詞在來源檔案中的第`namespace`一個標記（關鍵字）前面。

下列範例會產生編譯時期錯誤，因為會遵循實際`#define`的程式碼：
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

可能會定義已定義的條件式編譯符號，而不會有該符號的任何中間`#undef`。 `#define` 下列範例會定義條件式編譯符號`A` ，然後再次定義它。
```csharp
#define A
#define A
```

`#undef`可能會「取消定義」未定義的條件式編譯符號。 下列範例會定義條件式編譯符號`A` ，然後將它重新定義為兩次`#undef` ; 雖然第二個沒有作用，但仍然有效。
```csharp
#define A
#undef A
#undef A
```

### <a name="conditional-compilation-directives"></a>條件式編譯指示詞

條件式編譯指示詞是用來有條件地包含或排除來源檔案的部分。

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

如語法所指示，條件式編譯指示詞必須撰寫成由、依序`#if` 、指示詞、零或多個`#elif`指示詞、零或`#endif`一個`#else`指示詞和指示片語成的集合。 在指示詞之間，是原始程式碼的條件式區段。 每個區段都是由緊接在前面的指示詞所控制。 條件式區段本身可能會包含嵌套的條件式編譯指示詞，前提是這些指示詞會形成完整集合。

*Pp_conditional*最多會選取其中一個包含的*conditional_section*，以進行一般的詞法處理：

*  @No__t-1 和 @no__t 2 指示詞的*pp_expression*s 會依序評估，直到其中一個產生 `true` 為止。 如果運算式產生 `true`，則會選取對應指示詞的*conditional_section* 。
*  如果所有*pp_expression*s 都會產生 `false`，而且如果有 @no__t 2 指示詞，則會選取 `#else` 指示詞的*conditional_section* 。
*  否則，不會選取任何*conditional_section* 。

選取的*conditional_section*（如果有的話）會當做一般*input_section*處理：區段中包含的原始程式碼必須遵守詞法文法;權杖是從區段中的原始程式碼產生的;而區段中的前置處理指示詞具有指定的效果。

其餘的*conditional_section*（如果有的話）會當做*skipped_section*s 來處理：除了前置處理指示詞以外，區段中的原始程式碼不需要遵守詞法文法;不會從區段中的原始程式碼產生任何權杖;區段中的和前置處理指示詞必須是正確的，但不會進行處理。 在處理為*skipped_section*的*conditional_section*中，任何嵌套的*conditional_section*s （包含在嵌套 `#if` ... `#endif` 和 `#region` ... `#endregion` 結構）也會處理為*skipped_區段*s。

下列範例說明條件式編譯指示詞可以如何加以嵌套：
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

除了前置處理指示詞之外，略過的原始程式碼不會受限於詞彙分析。 例如，雖然`#else`區段中未結束的批註，下列內容有效：
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

不過要注意的是，即使在原始程式碼的略過區段中，前置處理指示詞也必須是正確的。

當前置處理指示詞出現在多行輸入元素內時，不會處理它們。 例如，程式：
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
輸出中的結果：
```console
hello,
#if Debug
        world
#else
        Nebraska
#endif
```

在什麼嗎的情況下，所處理的前置處理指示詞集合可能取決於*pp_expression*的評估。 範例：
```csharp
#if X
    /*
#else
    /* */ class Q { }
#endif
```
一律`class` `X`會產生相同的 token 資料流程（ `Q` `{` `}`），不論是否已定義。 如果`X`已定義，則唯一處理的指示`#if`詞`#endif`為和，因為多行批註。 如果`X`未定義，則三個指示`#if`詞`#else`（ `#endif`、、）是指示詞集合的一部分。

### <a name="diagnostic-directives"></a>診斷指示詞

診斷指示詞用來明確產生錯誤和警告訊息，其報告方式與其他編譯時期錯誤和警告相同。

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
如果條件式符號`Debug`和`Retail`皆已定義，一律會產生警告（「簽入前需要程式碼審核」），並產生編譯時期錯誤（「組建不能同時為 debug 和 retail」）。 請注意， *pp_message*可以包含任意文字;具體而言，它不需要包含格式正確的 token，如單字中的單引號所示 `can't`。

### <a name="region-directives"></a>Region 指示詞

區域指示詞是用來明確標示原始程式碼的區域。

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

不會將任何語義意義附加至區域;區域可供程式設計人員或自動化工具用來標示原始程式碼的某個區段。 `#region` 或`#endregion`指示詞中指定的訊息同樣沒有語義意義，只是用來識別區域。 比對 `#region` 和 @no__t 1 指示詞可能會有不同的*pp_message*。

區域的詞法處理：
```csharp
#region
...
#endregion
```
完全對應于格式的條件式編譯指示詞的詞法處理：
```csharp
#if true
...
#endif
```

### <a name="line-directives"></a>行指示詞

行指示詞可用來改變編譯器在輸出中報告的行號和原始程式檔名稱（例如警告和錯誤），以及呼叫端資訊屬性（[呼叫端資訊](attributes.md#caller-info-attributes)屬性）所使用的來原始檔案名。

行指示詞最常用於元程式設計工具中，以C#產生其他文字輸入的原始程式碼。

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

當沒有`#line`指示詞時，編譯器會在其輸出中報告真正的行號和原始程式檔名稱。 在處理包含不是 `default` 之*line_indicator*的 `#line` 指示詞時，編譯器會將指示詞後面的那一行視為具有指定的行號（如果有指定，則會使用檔案名）。

`#line default`指示詞會反轉所有前面 #line 指示詞的效果。 編譯器會報告後續行的 true 行資訊，就像未處理`#line`任何指示詞一樣。

指示`#line hidden`詞不會影響錯誤訊息中所報告的檔案和行號，而是會影響來源層級的偵錯工具。 在偵錯工具中，指示詞`#line hidden`和後續`#line`指示詞（不`#line hidden`是）之間的所有程式程式碼都沒有行號資訊。 當您在偵錯工具中逐步執行程式碼時，將會完全略過這幾行。

請注意， *file_name*與一般字串常值不同之處在于，不會處理該逸出字元;"`\`" 字元只會在*file_name*內指定一般的反斜線字元。

### <a name="pragma-directives"></a>Pragma 指示詞

`#pragma`前置處理指示詞是用來指定編譯器的選擇性內容資訊。 指示詞中`#pragma`提供的資訊永遠不會變更程式的語義。

```antlr
pp_pragma
    : whitespace? '#' whitespace? 'pragma' whitespace pragma_body pp_new_line
    ;

pragma_body
    : pragma_warning_body
    ;
```

C#提供`#pragma`用來控制編譯器警告的指示詞。 語言的未來版本可能包含額外`#pragma`的指示詞。 為了確保與其他C#編譯器的互通性C# ，Microsoft 編譯器不會發出未知`#pragma`指示詞的編譯錯誤; 不過，這類指示詞的確會產生警告。

#### <a name="pragma-warning"></a>pragma 警告

`#pragma warning`指示詞用來在編譯後續的程式文字時，停用或還原所有或一組特定的警告訊息。

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

`#pragma warning`省略警告清單的指示詞會影響所有的警告。 `#pragma warning`包含警告清單的指示詞只會影響清單中所指定的警告。

指示`#pragma warning disable`詞會停用所有或指定的一組警告。

`#pragma warning restore`指示詞會將所有或指定的警告集還原到編譯單位開始時生效的狀態。 請注意，如果已在外部停用特定警告`#pragma warning restore` ，則（不論是所有或特定的警告）都不會重新啟用該警告。

下列範例顯示使用 Microsoft `#pragma warning` C#編譯器中的警告編號，在參考過時的成員時，暫時停用所報告的警告。
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
