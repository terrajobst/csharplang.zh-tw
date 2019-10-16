---
ms.openlocfilehash: 7248a91976c479dc1b6b64b799639635617a7bec
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704048"
---
# <a name="statements"></a>陳述式

C#提供各種語句。 對於以 C 和C++編寫的開發人員而言，大部分的語句都是熟悉的。

```antlr
statement
    : labeled_statement
    | declaration_statement
    | embedded_statement
    ;

embedded_statement
    : block
    | empty_statement
    | expression_statement
    | selection_statement
    | iteration_statement
    | jump_statement
    | try_statement
    | checked_statement
    | unchecked_statement
    | lock_statement
    | using_statement
    | yield_statement
    | embedded_statement_unsafe
    ;
```

*Embedded_statement*語法會用於出現在其他語句中的語句。 使用*embedded_statement*而不是*語句*，會排除在這些內容中使用宣告語句和標記語句。 範例
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
會導致編譯時期錯誤，因為 `if` 語句需要*embedded_statement* ，而不是其 if 分支的*語句*。 如果允許此程式碼，則會宣告`i`變數，但永遠不會使用它。 不過，請注意，藉由`i`將宣告放在區塊中，此範例是有效的。

## <a name="end-points-and-reachability"></a>端點和連線能力

每個語句都有一個***結束點***。 就直覺而言，語句的結束點是緊接在語句後面的位置。 複合陳述式的執行規則（包含內嵌語句的語句）會指定當控制項到達內嵌語句的結束點時，所採取的動作。 例如，當控制項到達區塊中語句的結束點時，控制權會轉移到區塊中的下一個語句。

如果執行可能會觸達語句，則表示語句***可供連線。*** 相反地，如果不可能執行語句，則會將***語句視為無法連線。***

在範例中
```csharp
void F() {
    Console.WriteLine("reachable");
    goto Label;
    Console.WriteLine("unreachable");
    Label:
    Console.WriteLine("reachable");
}
```
因為不可能執行`Console.WriteLine`語句，所以無法連接的第二個調用。

如果編譯器判斷無法連線到語句，就會回報警告。 這對於無法連線的語句而言特別不是錯誤。

為了判斷是否可連線到特定的語句或端點，編譯器會根據針對每個語句所定義的可連線性規則來執行流程分析。 流程分析會考慮常數運算式（[常數運算式](expressions.md#constant-expressions)）的值，以控制語句的行為，但不會考慮非常數運算式的可能值。 換句話說，針對控制流程分析的用途，會將指定類型的非常數運算式視為具有該類型的任何可能值。

在範例中
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
`if`語句的布林運算式是常數運算式，因為`==`運算子的兩個運算元都是常數。 當常數運算式在編譯時期進行評估時，如果產生值`false`，則會將`Console.WriteLine`調用視為無法存取。 不過，如果`i`變更為本機變數
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
叫用會被視為可連線，但實際上永遠不會執行此調用。`Console.WriteLine`

函式成員的*區塊*一律視為可連線。 藉由連續評估區塊中每個語句的可連線性規則，就可以判斷任何給定語句的可連線性。

在範例中
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
第二個`Console.WriteLine`的可連線性決定如下：

*  可以連線`Console.WriteLine`到第一個運算式語句，因為可以連接`F`到方法的區塊。
*  可以連線到第一個`Console.WriteLine`運算式語句的結束點，因為可以連接該語句。
*  可以`if`連線到語句，因為可連線到第一個`Console.WriteLine`運算式語句的結束點。
*  第二`Console.WriteLine`個運算式語句可以連接，因為`if`語句的布林運算式沒有常數值`false`。

在兩種情況下，如果語句的結束點可供存取，就會發生編譯時期錯誤：

*  `switch`因為語句不允許參數區段「落到」下一個參數區段，所以參數區段之語句清單的結束點就會發生編譯時期錯誤。 如果發生此錯誤，通常表示`break`遺漏語句。
*  如果函式成員的區塊結束點會計算可連線的值，則會發生編譯時期錯誤。 如果發生此錯誤，通常`return`表示遺漏語句。

## <a name="blocks"></a>區塊

「區塊」可允許在許可單一陳述式的內容中撰寫多個陳述式。

```antlr
block
    : '{' statement_list? '}'
    ;
```

*區塊*是由選擇性的*statement_list* （[語句清單](statements.md#statement-lists)）所組成，以大括弧括住。 如果省略語句清單，則區塊會被視為空白。

區塊可以包含宣告語句（宣告[語句](statements.md#declaration-statements)）。 區塊中宣告的區域變數或常數的範圍是區塊。

區塊的執行方式如下：

*  如果區塊是空的，控制權就會傳送到區塊的結束點。
*  如果區塊不是空的，控制權就會傳送至語句清單。 當控制項到達語句清單的終點時，控制權會轉移到區塊的結束點。

如果可以連線到區塊本身，則可以連接到區塊的語句清單。

如果區塊是空的或語句清單的結束點可供連線，則區塊的結束點可供連線。

包含一個或多個`yield`語句（[yield 語句](statements.md#the-yield-statement)）的區塊稱為 iterator 區塊。 Iterator 區塊是用來將函式成員當做反覆運算器（[反覆運算](classes.md#iterators)器）來執行。 有一些額外的限制適用于 iterator 區塊：

*  `return`語句會出現在 iterator 區塊中（但`yield return`允許語句），這是編譯時期錯誤。
*  反覆運算器區塊包含不安全內容（[不安全](unsafe-code.md#unsafe-contexts)的內容）時，就會發生編譯時期錯誤。 反覆運算器區塊一律會定義安全的內容，即使其宣告是嵌套在不安全的內容中。

### <a name="statement-lists"></a>語句清單

***語句清單***是由一或多個依序寫入的語句所組成。 語句清單會出現在*區塊*s （[區塊](statements.md#blocks)）和*switch_block*s （[switch 語句](statements.md#the-switch-statement)）中。

```antlr
statement_list
    : statement+
    ;
```

語句清單是藉由將控制項傳輸至第一個語句來執行。 當控制項到達語句的結束點時，控制權會轉移到下一個語句。 當控制項到達最後一個語句的終點時，控制權會轉移到語句清單的結束點。

如果下列至少一項為真，則語句清單中的語句可連線：

*  語句是第一個語句，而語句清單本身是可連線的。
*  可以觸達上述語句的結束點。
*  語句是一個標記的語句，而標籤是由`goto`可連接的語句所參考。

如果清單中的最後一個語句的結束點可供連線，則語句清單的結束點就會是可到達的。

## <a name="the-empty-statement"></a>空陳述式

*Empty_statement*不會執行任何操作。

```antlr
empty_statement
    : ';'
    ;
```

當需要語句的內容中沒有要執行的作業時，會使用空的語句。

執行空的語句只會將控制權轉移到語句的結束點。 因此，如果可以連線到空的語句，就可以觸達空語句的結束點。

在撰寫`while`具有 null 主體的語句時，可以使用空的語句：
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

此外，空的語句也可以用來在區塊的結尾 "`}`" 之前宣告標籤：
```csharp
void F() {
    ...
    if (done) goto exit;
    ...
    exit: ;
}
```

## <a name="labeled-statements"></a>標記陳述式

*Labeled_statement*允許語句前面加上標籤。 區塊中允許標記的語句，但不允許做為內嵌語句。

```antlr
labeled_statement
    : identifier ':' statement
    ;
```

標記的語句會宣告具有*識別碼*所指定之名稱的標籤。 標籤的範圍是宣告標籤的整個區塊，包括任何嵌套的區塊。 這是兩個具有相同名稱的標籤，具有重迭範圍的編譯時期錯誤。

標籤可以從`goto`標籤範圍內的語句（[goto 語句](statements.md#the-goto-statement)）參考。 這表示`goto`語句可以在區塊內和區塊外傳輸控制項，但絕不會進入區塊。

標籤有自己的宣告空間，而且不會干擾其他識別碼。 範例
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
是有效的，而且使用`x`名稱同時做為參數和標籤。

執行標籤的語句時，會完全對應到在標籤之後執行語句。

除了一般控制流程所提供的可連線性之外，如果可連接的`goto`語句參考標籤，則可連接標記的語句。 異常`try` `try` `finally`如果語句是在包含區塊的內，而且加上標籤的語句位於之外，而且無法連線到`finally`區塊的結束點，則無法從存取標記的語句`goto`該`goto`語句）。

## <a name="declaration-statements"></a>宣告陳述式

*Declaration_statement*會宣告本機變數或常數。 區塊中允許宣告語句，但不允許做為內嵌語句。

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a>區域變數宣告

*Local_variable_declaration*會宣告一或多個本機變數。

```antlr
local_variable_declaration
    : local_variable_type local_variable_declarators
    ;

local_variable_type
    : type
    | 'var'
    ;

local_variable_declarators
    : local_variable_declarator
    | local_variable_declarators ',' local_variable_declarator
    ;

local_variable_declarator
    : identifier
    | identifier '=' local_variable_initializer
    ;

local_variable_initializer
    : expression
    | array_initializer
    | local_variable_initializer_unsafe
    ;
```

*Local_variable_declaration*的*local_variable_type*會直接指定宣告所引進的變數類型，或指示識別碼 `var`，該類型應根據初始化運算式來推斷。 類型後面接著*local_variable_declarator*的清單，其中每個都會引進新的變數。 *Local_variable_declarator*包含命名變數的*識別碼*，並可選擇性地後面加上 "`=`" 權杖和*local_variable_initializer* ，以提供變數的初始值。

在本機變數宣告的內容中，識別碼 var 會當做內容關鍵字（[關鍵字](lexical-structure.md#keywords)）。當*local_variable_type*指定為 `var`，而且沒有任何名為 `var` 的類型在範圍內時，宣告就是隱含類型的***區域變數***宣告，其類型是從相關聯的初始化運算式運算式的類型推斷而來。 隱含類型的區域變數宣告受到下列限制：

*  *Local_variable_declaration*不能包含多個*local_variable_declarator*s。
*  *Local_variable_declarator*必須包含*local_variable_initializer*。
*  *Local_variable_initializer*必須是*運算式*。
*  初始化*運算式運算式*必須有編譯時期類型。
*  初始化*運算式運算式*不能參考宣告的變數本身

以下是不正確的隱含類型區域變數宣告範例：

```csharp
var x;               // Error, no initializer to infer type from
var y = {1, 2, 3};   // Error, array initializer not permitted
var z = null;        // Error, null does not have a type
var u = x => x + 1;  // Error, anonymous functions do not have a type
var v = v++;         // Error, initializer cannot refer to variable itself
```

區域變數的值是使用*simple_name* （[簡單名稱](expressions.md#simple-names)）在運算式中取得，而區域變數的值則是使用*指派*（[指派運算子](expressions.md#assignment-operators)）來修改。 在取得其值的每個位置，都必須明確指派本機變數（[明確](variables.md#definite-assignment)指派）。

在*local_variable_declaration*中宣告的本機變數範圍是宣告發生所在的區塊。 在本機變數的*local_variable_declarator*之前，參考位於文字位置的區域變數是錯誤的。 在區域變數的範圍內，宣告具有相同名稱的另一個本機變數或常數會發生編譯時期錯誤。

宣告多個變數的區域變數宣告相當於多個具有相同類型之單一變數的宣告。 此外，區域變數宣告中的變數初始化運算式會完全對應至緊接在宣告之後插入的指派語句。

範例
```csharp
void F() {
    int x = 1, y, z = x * 2;
}
```
完全對應至
```csharp
void F() {
    int x; x = 1;
    int y;
    int z; z = x * 2;
}
```

在隱含類型的區域變數宣告中，所宣告的區域變數類型會被視為與用來初始化變數的運算式類型相同。 例如:
```csharp
var i = 5;
var s = "Hello";
var d = 1.0;
var numbers = new int[] {1, 2, 3};
var orders = new Dictionary<int,Order>();
```

上述隱含型別區域變數宣告相當於下列明確類型的宣告：
```csharp
int i = 5;
string s = "Hello";
double d = 1.0;
int[] numbers = new int[] {1, 2, 3};
Dictionary<int,Order> orders = new Dictionary<int,Order>();
```

### <a name="local-constant-declarations"></a>區域常數宣告

*Local_constant_declaration*會宣告一個或多個本機常數。

```antlr
local_constant_declaration
    : 'const' type constant_declarators
    ;

constant_declarators
    : constant_declarator (',' constant_declarator)*
    ;

constant_declarator
    : identifier '=' constant_expression
    ;
```

*Local_constant_declaration*的*類型*會指定宣告所引進的常數類型。 類型後面接著*constant_declarator*的清單，其中每個都會引進新的常數。 *Constant_declarator*包含命名常數的*識別碼*，後面接著 "`=`" token，接著是提供常數值的*constant_expression* （[常數運算式](expressions.md#constant-expressions)）。

區域常數值宣告的*類型*和*constant_expression*必須遵循與常數成員宣告（[常數](classes.md#constants)）相同的規則。

本機常數的值是使用*simple_name* （[簡單名稱](expressions.md#simple-names)）在運算式中取得。

本機常數的範圍是宣告發生所在的區塊。 在*constant_declarator*之前的文字位置中參考本機常數是錯誤的。 在本機常數的範圍內，宣告具有相同名稱的另一個本機變數或常數的編譯時期錯誤。

宣告多個常數的本機常數宣告相當於多個具有相同類型之單一常數的宣告。

## <a name="expression-statements"></a>運算式陳述式

*Expression_statement*會評估指定的運算式。 會捨棄運算式所計算的值（如果有的話）。

```antlr
expression_statement
    : statement_expression ';'
    ;

statement_expression
    : invocation_expression
    | null_conditional_invocation_expression
    | object_creation_expression
    | assignment
    | post_increment_expression
    | post_decrement_expression
    | pre_increment_expression
    | pre_decrement_expression
    | await_expression
    ;
```

並非所有運算式都允許當做語句。 特別是，不允許只`x + y`計算`x == 1`值（將被捨棄）的運算式，做為語句。

執行*expression_statement*會評估包含的運算式，然後將控制權轉移至*expression_statement*的結束點。 如果可以連線到該*expression_statement* ，就可以連線到*expression_statement*的結束點。

## <a name="selection-statements"></a>選取範圍陳述式

選取範圍語句根據某個運算式的值，選取其中一個可能的語句來執行。

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a>If 語句

`if`語句會根據布林運算式的值來選取要執行的語句。

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

元件會與語法允許的最接近前面`if`的程式關聯。 `else` 因此， `if`表單的語句
```csharp
if (x) if (y) F(); else G();
```
相當於
```csharp
if (x) {
    if (y) {
        F();
    }
    else {
        G();
    }
}
```

`if`語句的執行方式如下：

*  會評估*boolean_expression* （[布林運算式](expressions.md#boolean-expressions)）。
*  如果布林運算式產生`true`，控制權就會傳送至第一個內嵌語句。 當控制項到達該語句的結束點時，控制權就會傳送至`if`語句的結束點。
*  如果布林運算式產生`false` ，而且如果有某個`else`部分存在，控制權就會傳送至第二個內嵌語句。 當控制項到達該語句的結束點時，控制權就會傳送至`if`語句的結束點。
*  如果布林運算式產生`false` ，而且`else`如果元件不存在，控制權就會傳送至`if`語句的結束點。

如果可以連線到`if` `if`語句，而且布林運算式沒有常數值`false`，則可以連接語句的第一個內嵌語句。

`if`語句的第二個內嵌語句（如果有的話）可以連接到`if`語句，而且布林運算式沒有常數值`true`。

如果至少有一個內嵌`if`語句的端點可供存取，則語句的結束點可供連線。 此外，如果可以`if`連線到`if`語句，而且布林運算式`else`沒有常數值`true`，就可以連接沒有部分之語句的結束點。

### <a name="the-switch-statement"></a>Switch 語句

Switch 語句會選取執行語句清單，其中包含對應至 switch 運算式值的相關聯參數標籤。

```antlr
switch_statement
    : 'switch' '(' expression ')' switch_block
    ;

switch_block
    : '{' switch_section* '}'
    ;

switch_section
    : switch_label+ statement_list
    ;

switch_label
    : 'case' constant_expression ':'
    | 'default' ':'
    ;
```

*Switch_statement*是由關鍵字 `switch` 組成，後面加上括弧括住的運算式（稱為 switch 運算式），後面接著*switch_block*。 *Switch_block*是由零個或多個*switch_section*組成，以大括弧括住。 每個*switch_section*都包含一個或多個*switch_label*，後面接著*statement_list* （[語句清單](statements.md#statement-lists)）。

`switch`語句的***管理類型***是由 switch 運算式所建立。

*  如果 switch 運算式的類型為 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`bool`、`char`、0 或*enum_type*，或者它是對應于其中一種類型的可為 null 的類型，那就是 2 語句的管理類型。
*  否則，只有一個使用者定義的隱含轉換（[使用者定義的轉換](conversions.md#user-defined-conversions)），必須從 switch 運算式的類型，到下列其中一種可能的管理類型： `sbyte`、 `byte`、 `short`、 `ushort`、 `int`、 `uint`、 、`long` 、`char`、或，這是對應至其中一種類型的可為 null 類型。 `string` `ulong`
*  否則，如果不存在這類隱含轉換，或如果有多個這類隱含轉換存在，就會發生編譯時期錯誤。

每個`case`標籤的常數運算式都必須代表可隱含轉換（[隱含轉換](conversions.md#implicit-conversions)）為`switch`語句之治理類型的值。 如果相同`case` `switch`語句中有兩個或多個標籤指定相同的常數值，就會發生編譯時期錯誤。

Switch 語句中最多隻能`default`有一個標籤。

`switch`語句的執行方式如下：

*  會評估 switch 運算式，並將其轉換為管理類型。
*  如果相同`case` `switch`語句的標籤中指定的其中一個常數等於 switch 運算式的值，則會將控制權轉移到符合`case`的標籤後面的語句清單。
*  `case`如果相同`switch`語句中的標籤中指定的常數都不等於 switch `default`運算式的值，而且如果有標籤，則`default`會將控制權轉移至語句清單，並遵循標誌.
*  `case`如果相同`switch`語句的標籤中指定的常數都不等於 switch 運算式的值，而且如果沒有`default`標籤，則控制權`switch`會傳送至語句的結束點。

如果可以連線到 switch 區段之語句清單的結束點，就會發生編譯時期錯誤。 這就是所謂的「不流經」規則。 範例
```csharp
switch (i) {
case 0:
    CaseZero();
    break;
case 1:
    CaseOne();
    break;
default:
    CaseOthers();
    break;
}
```
有效，因為沒有參數區段具有可連線的結束點。 不同于 C C++和，switch 區段的執行不允許「落在」到下一個參數區段，而範例
```csharp
switch (i) {
case 0:
    CaseZero();
case 1:
    CaseZeroOrOne();
default:
    CaseAny();
}
```
會導致編譯時期錯誤。 執行 switch 區段之後，執行另一個參數區段時，必須使用明確`goto case`或`goto default`語句：
```csharp
switch (i) {
case 0:
    CaseZero();
    goto case 1;
case 1:
    CaseZeroOrOne();
    goto default;
default:
    CaseAny();
    break;
}
```

*Switch_section*中允許多個標籤。 範例
```csharp
switch (i) {
case 0:
    CaseZero();
    break;
case 1:
    CaseOne();
    break;
case 2:
default:
    CaseTwo();
    break;
}
```
有效。 此範例不會違反「不流經」規則，因為標籤 `case 2:`，而 `default:` 是相同*switch_section*的一部分。

「無範圍」規則可防止 C 中發生的常見錯誤類別，以及C++ `break`不小心省略語句的情況。 此外，由於這項規則， `switch`語句的 switch 區段可以任意重新排列，而不會影響語句的行為。 例如，上述`switch`語句的區段可以反轉，而不會影響語句的行為：
```csharp
switch (i) {
default:
    CaseAny();
    break;
case 1:
    CaseZeroOrOne();
    goto default;
case 0:
    CaseZero();
    goto case 1;
}
```

Switch 區段的語句清單通常會在`break`、 `goto case`或`goto default`語句中結束，但不允許任何呈現語句清單之結束點的結構。 例如， `while`已知由布林運算式`true`所控制的語句，絕對不會到達其結束點。 同樣地， `throw`或`return`語句一律會將控制權轉移到別處，而且永遠不會到達其結束點。 因此，下列範例是有效的：
```csharp
switch (i) {
case 0:
    while (true) F();
case 1:
    throw new ArgumentException();
case 2:
    return;
}
```

`switch`語句的管理類型可以是類型`string`。 例如:
```csharp
void DoCommand(string command) {
    switch (command.ToLower()) {
    case "run":
        DoRun();
        break;
    case "save":
        DoSave();
        break;
    case "quit":
        DoQuit();
        break;
    default:
        InvalidCommand(command);
        break;
    }
}
```

如同字串等號比較運算子（[字串等號比較](expressions.md#string-equality-operators)運算子`switch` ），語句會區分大小寫，而且只有在 switch 運算式`case`字串完全符合標籤常數時，才會執行指定的參數區段。

當`switch`語句的管理類型為`string`時，會允許此`null`值作為 case 標籤常數。

*Switch_block*的*statement_list*可能包含宣告語句（宣告[語句](statements.md#declaration-statements)）。 在 switch 區塊中宣告的區域變數或常數的範圍是 switch 區塊。

如果可以連線到`switch`語句，而且至少有下列其中一項為真，則可以連接指定參數區段的語句清單：

*  Switch 運算式是一個非常數值。
*  Switch 運算式是與 switch 區段中的`case`標籤相符的常數值。
*  Switch 運算式是不符合任何`case`標籤的常數值，而 switch 區段`default`包含標籤。
*  Switch 區段的切換標籤是由`goto case`可連接的或`goto default`語句所參考。

如果下列至少一項`switch`為真，則語句的結束點可供連線：

*  語句包含可連接`switch`的`break`語句，它會結束語句。 `switch`
*  可以`switch`連線到語句，switch 運算式為非常數值，且沒有任何`default`標籤存在。
*  可以`switch`連線到語句，switch 運算式是不符合任何`case`標籤的常數值，而且沒有任何`default`標籤存在。

## <a name="iteration-statements"></a>反覆運算陳述式

反覆運算語句會重複執行內嵌語句。

```antlr
iteration_statement
    : while_statement
    | do_statement
    | for_statement
    | foreach_statement
    ;
```

### <a name="the-while-statement"></a>While 語句

`while`語句有條件地執行內嵌語句零次或多次。

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

`while`語句的執行方式如下：

*  會評估*boolean_expression* （[布林運算式](expressions.md#boolean-expressions)）。
*  如果布林運算式產生`true`，控制權就會傳送至內嵌語句。 當和（如果控制）到達內嵌語句的結束點（可能是從`continue`語句執行）時，控制權就會傳送至`while`語句的開頭。
*  如果布林運算式產生`false`，控制權就會傳送至`while`語句的結束點。

在`while`語句的內嵌語句中`break` ，可以使用語句（[break 語句](statements.md#the-break-statement)）將`while`控制項傳送至語句的結束點（因此會結束內嵌語句的反覆運算），而`continue`語句（[continue 語句](statements.md#the-continue-statement)）可用來將控制權轉移到內嵌語句的結束點（因而執行`while`語句的另一個反復專案）。

如果可以連線到`while` `while`語句，而且布林運算式沒有常數值`false`，則可以連接語句的內嵌語句。

如果下列至少一項`while`為真，則語句的結束點可供連線：

*  語句包含可連接`while`的`break`語句，它會結束語句。 `while`
*  可以`while`連線到語句，且布林運算式沒有常數值。 `true`

### <a name="the-do-statement"></a>Do 語句

`do`語句會有條件地執行內嵌語句一次或多次。

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

`do`語句的執行方式如下：

*  控制項會傳送至內嵌語句。
*  當控制項到達內嵌語句的結束點（可能是從執行 `continue` 語句）時，就會評估*boolean_expression* （[布林運算式](expressions.md#boolean-expressions)）。 如果布林運算式產生`true`，控制權就會傳送至`do`語句的開頭。 否則，控制權會轉移到`do`語句的結束點。

在`do`語句的內嵌語句中`break` ，可以使用語句（[break 語句](statements.md#the-break-statement)）將`do`控制項傳送至語句的結束點（因此會結束內嵌語句的反覆運算），而`continue`語句（[continue 語句](statements.md#the-continue-statement)）可用來將控制權轉移到內嵌語句的結束點。

如果可以連線到`do` `do`語句，則可以連接語句的內嵌語句。

如果下列至少一項`do`為真，則語句的結束點可供連線：

*  語句包含可連接`do`的`break`語句，它會結束語句。 `do`
*  內嵌語句的結束點可供連線，且布林運算式沒有常數值`true`。

### <a name="the-for-statement"></a>For 語句

`for`語句會評估初始化運算式的序列，然後，當條件為 true 時，會重複執行內嵌語句，並評估反覆運算運算式的序列。

```antlr
for_statement
    : 'for' '(' for_initializer? ';' for_condition? ';' for_iterator? ')' embedded_statement
    ;

for_initializer
    : local_variable_declaration
    | statement_expression_list
    ;

for_condition
    : boolean_expression
    ;

for_iterator
    : statement_expression_list
    ;

statement_expression_list
    : statement_expression (',' statement_expression)*
    ;
```

*For_initializer*（如果有的話）是由*local_variable_declaration* （[區域變數](statements.md#local-variable-declarations)宣告）或*statement_expression*s （[expression 語句](statements.md#expression-statements)）清單（以逗號分隔）所組成。 *For_initializer*所宣告的區域變數範圍會從變數的*local_variable_declarator*開始，並延伸至內嵌語句的結尾。 範圍包含*for_condition*和*for_iterator*。

*For_condition*（如果有的話）必須是*boolean_expression* （[布林運算式](expressions.md#boolean-expressions)）。

*For_iterator*（如果有的話）包含以逗號分隔的*Statement_expression*s （[expression 語句](statements.md#expression-statements)）清單。

For 語句的執行方式如下：

*  如果*for_initializer*存在，變數初始化運算式或語句運算式就會依撰寫的循序執行。 此步驟只會執行一次。
*  如果*for_condition*存在，則會進行評估。
*  如果*for_condition*不存在，或評估產生 `true`，則會將控制權轉移到內嵌語句。 當控制項到達內嵌語句的結束點（可能是從執行 @no__t 0 的語句）時，會依序評估*for_iterator*的運算式（如果有的話），然後再執行另一個反復專案，從上述步驟中的*for_condition*評估。
*  如果*for_condition*存在，且評估產生 `false`，則控制權會轉移至 @no__t 2 語句的結束點。

在 `for` 語句的 embedded 語句中，@no__t 1 語句（[break 語句](statements.md#the-break-statement)）可以用來將控制權轉移至 `for` 語句的結束點（因此，會結束內嵌語句的反復專案）和 `continue` 語句（[Continue 語句](statements.md#the-continue-statement)）可用來將控制權轉移到內嵌語句的結束點（因此，從*for_condition*開始，執行*for_iterator*並執行 `for` 語句的另一個反復專案）。

如果下列其中一項`for`為真，就可以觸達語句的內嵌語句：

*  @No__t-0 語句可供連線，而且沒有任何*for_condition*存在。
*  @No__t-0 語句可供連線，而且有*for_condition*存在，而且沒有常數值 `false`。

如果下列至少一項`for`為真，則語句的結束點可供連線：

*  語句包含可連接`for`的`break`語句，它會結束語句。 `for`
*  @No__t-0 語句可供連線，而且有*for_condition*存在，而且沒有常數值 `true`。

### <a name="the-foreach-statement"></a>Foreach 語句

`foreach`語句會列舉集合的元素，並針對集合的每個專案執行內嵌語句。

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

`foreach`語句的*類型*和*識別碼*會宣告語句的***反覆運算變數***。 如果 @no__t 0 的識別碼指定為*local_variable_type*，而且沒有任何名為 `var` 的類型在範圍內，則反復專案變數就會被視為***隱含類型的反復專案變數***，而且其類型會被視為 `foreach` 的元素類型。語句，如下所示。 反覆運算變數會對應到唯讀區域變數，其範圍會延伸到內嵌語句。 在`foreach`語句執行期間，反覆運算變數代表目前正在執行反復專案的集合元素。 如果內嵌的語句嘗試修改反復專案變數（透過指派`++`或和`--`運算子），或將反覆運算變數`ref`當做或`out`參數傳遞，就會發生編譯時期錯誤。

在下列中，為了簡單起見`IEnumerable`， `IEnumerator` `IEnumerable<T>` 、和`IEnumerator<T>`會參考命名空間`System.Collections`和`System.Collections.Generic`中的對應類型。

Foreach 語句的編譯時間處理會先決定運算式的***集合類型***、***列舉數值型別***和***元素類型***。 這項決定會繼續進行，如下所示：

*  如果運算式的`X`類型是陣列類型，則會有從`X`到`IEnumerable`介面的隱含參考轉換（因為`System.Array`會執行此介面）。 ***集合類型*** `IEnumerable`為介面，***列舉數值型別***為`IEnumerator`介面，而***元素類型***為數組類型`X`的元素類型。
*  如果運算式的`X`類型 `dynamic`是，則會隱含地從*運算式*轉換成`IEnumerable`介面（隱含的[動態轉換](conversions.md#implicit-dynamic-conversions)）。 ***集合類型***為`IEnumerable`介面，而列舉值***類型***為`IEnumerator`介面。 如果 `var` 識別碼指定為*local_variable_type* ，則***元素類型***會 `dynamic`，否則會 `object`。
*  否則，請判斷類型`X`是否具有適當`GetEnumerator`的方法：
   * 在具有識別碼`GetEnumerator`和沒有類型`X`引數的類型上執行成員查閱。 如果成員查閱不會產生符合的結果，或產生不明確的結果，或產生不是方法群組的比對，請檢查是否有可列舉的介面，如下所述。 如果成員查閱除了方法群組以外，或沒有相符專案以外，建議您發出警告。
   * 使用產生的方法群組和空的引數清單來執行多載解析。 如果多載解析不會產生適用的方法、導致不明確的情況，或產生單一最佳方法，但該方法是靜態或非公用的，請檢查是否有可列舉的介面，如下所述。 如果多載解析除了明確的公用實例方法以外，或沒有適用的方法，建議您發出警告。
   * 如果`GetEnumerator`方法的傳回`E`型別不是類別、結構或介面型別，就會產生錯誤，而且不會採取任何進一步的步驟。
   * 成員查閱是在上`E`執行，且`Current`具有識別碼，而且沒有類型引數。 如果成員查閱不會產生相符專案，則結果會是錯誤，或結果是除了允許讀取的公用實例屬性以外的任何專案，會產生錯誤，而且不會採取任何進一步的步驟。
   * 成員查閱是在上`E`執行，且`MoveNext`具有識別碼，而且沒有類型引數。 如果成員查閱不會產生相符專案，則結果會是錯誤，或結果是除了方法群組以外的任何專案，會產生錯誤，而且不會採取進一步的步驟。
   * 多載解析是在具有空白引數清單的方法群組上執行。 如果多載解析不會產生適用的方法、導致不明確的結果，或產生單一最佳方法，但該方法是靜態或非公用的，或其傳回型`bool`別不是，則會產生錯誤，而且不會採取任何進一步的步驟。
   * ***集合型***別為`X`，***列舉值型***別`E`為， `Current`而***元素型***別為屬性的型別。

*  否則，請檢查可列舉的介面：
   * 如果`Ti`所有類型之間有隱含的從`X`轉換成`IEnumerable<Ti>`的型別`T` ，則會有唯一的類型`T` ，這不`dynamic`是，而且所有其他`Ti`都有從`IEnumerable<T>`到`IEnumerator<T>` `IEnumerable<T>`的隱含轉換，則集合類型為介面、列舉數值型別為介面，而元素類型為`IEnumerable<Ti>` `T`.
   * 否則，如果有多個這種類型`T`，則會產生錯誤，而且不會採取任何進一步的步驟。
   * 否則，如果有`X`從`System.Collections.IEnumerable`到介面的隱含轉換，則***集合類型***為此介面、***列舉數值型別***為介面`System.Collections.IEnumerator`，而***元素類型***為`object`.
   * 否則會產生錯誤，而且不會採取任何進一步的步驟。

如果成功，上述步驟會明確產生集合類型`C`、列舉數值型別`E`和元素類型`T`。 表單的 foreach 語句
```csharp
foreach (V v in x) embedded_statement
```
接著會展開為：
```csharp
{
    E e = ((C)(x)).GetEnumerator();
    try {
        while (e.MoveNext()) {
            V v = (V)(T)e.Current;
            embedded_statement
        }
    }
    finally {
        ... // Dispose e
    }
}
```

`e` 運算式`x`或內嵌語句或程式的任何其他原始程式碼都看不到或無法存取變數。 此變數`v`在內嵌語句中是唯讀的。 如果沒有從 `T` （專案類型）到 `V` （foreach 語句中的*local_variable_type* ）的明確轉換（[明確](conversions.md#explicit-conversions)轉換），就會產生錯誤，而且不會採取任何進一步的步驟。 如果`x`的值`null`為， `System.NullReferenceException`則會在執行時間擲回。

實作為執行指定的 foreach 語句有不同的方式，例如基於效能的考慮，前提是該行為與上述擴充一致。

While 迴圈內 `v` 的位置，對於*embedded_statement*中發生的任何匿名函式而言，都很重要。

例如:
```csharp
int[] values = { 7, 9, 13 };
Action f = null;

foreach (var value in values)
{
    if (f == null) f = () => Console.WriteLine("First value: " + value);
}

f();
```
如果`v`是在 while 迴圈外宣告，則會在所有反復專案之間共用，而在 for 迴圈之後的值會是最後的`13`值，這`f`就是的調用會列印的結果。 相反地，因為每個反復專案都`v`有自己的變數， `f`所以第一個反復專案中所捕捉的會`7`繼續保留值，這就是要列印的內容。 （注意：在 while 迴圈C# `v`外宣告的舊版。）

Finally 區塊的主體會根據下列步驟來進行結構化：

*  如果有從`E` `System.IDisposable`到介面的隱含轉換，則
   *  如果`E`是不可為 null 的實數值型別，則 finally 子句會展開為對等的語義：

      ```csharp
      finally {
          ((System.IDisposable)e).Dispose();
      }
      ```

   *  否則，finally 子句會展開為的語義對應項：

      ```csharp
      finally {
          if (e != null) ((System.IDisposable)e).Dispose();
      }
      ```

   但如果`E`是實值型別，或具現化為實值型別的`e`型別參數，則將`System.IDisposable`轉換成將不會造成裝箱。

*  否則，如果`E`是密封型別，則 finally 子句會展開為空的區塊：

   ```csharp
   finally {
   }
   ```

*  否則，finally 子句會展開為：

   ```csharp
   finally {
       System.IDisposable d = e as System.IDisposable;
       if (d != null) d.Dispose();
   }
   ```    

   任何使用者程式`d`代碼都看不到或無法存取本機變數。 特別的是，它不會與範圍包含 finally 區塊的任何其他變數發生衝突。

`foreach`遍歷陣列元素的順序如下所示：對於一維陣列元素，會以遞增的索引順序來進行遍歷， `0`從索引開始， `Length - 1`並以索引結尾。 若為多維陣列，則會將專案進行遍歷，讓最右邊維度的索引先增加，然後是下一個左邊的維度，依此類推。

下列範例會依照元素順序，在二維陣列中列印每個值：
```csharp
using System;

class Test
{
    static void Main() {
        double[,] values = {
            {1.2, 2.3, 3.4, 4.5},
            {5.6, 6.7, 7.8, 8.9}
        };

        foreach (double elementValue in values)
            Console.Write("{0} ", elementValue);

        Console.WriteLine();
    }
}
```
產生的輸出如下所示：
```console
1.2 2.3 3.4 4.5 5.6 6.7 7.8 8.9
```

在範例中
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
的類型`n`會推斷`int`為`numbers`，其為的元素類型。

## <a name="jump-statements"></a>跳躍陳述式

跳躍語句會無條件地傳輸控制權。

```antlr
jump_statement
    : break_statement
    | continue_statement
    | goto_statement
    | return_statement
    | throw_statement
    ;
```

跳躍語句傳送控制項的位置稱為跳躍語句的***目標***。

當跳躍語句出現在區塊內，而且該跳躍陳述式的目標位於該區塊外時，跳躍語句就會被視為結束區塊。 雖然跳躍語句可能會將控制項從區塊轉移出來，但它無法將控制權轉移到區塊中。

執行跳躍語句會因為中間`try`語句的存在而變得很複雜。 如果沒有這類`try`語句，跳躍語句會無條件地將控制權從跳躍語句轉移到其目標。 在存在這類中間`try`語句時，執行會更複雜。 如果跳躍語句結束一個或多個`try`具有相關聯`finally`區塊的區塊， `finally`則控制權一開始會傳送到最`try`內層語句的區塊。 當控制項到達`finally`區塊的結束點時，控制權會轉移`finally`到下一個封入`try`語句的區塊。 這個程式會重複執行， `finally`直到所有中間`try`語句的區塊都已執行為止。

在範例中
```csharp
using System;

class Test
{
    static void Main() {
        while (true) {
            try {
                try {
                    Console.WriteLine("Before break");
                    break;
                }
                finally {
                    Console.WriteLine("Innermost finally block");
                }
            }
            finally {
                Console.WriteLine("Outermost finally block");
            }
        }
        Console.WriteLine("After break");
    }
}
```
在`finally`控制權轉移至跳躍`try`語句的目標之前，會執行與兩個語句相關聯的區塊。

產生的輸出如下所示：
```console
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a>Break 語句

`for` `do` `switch` `while`語句會結束最接近的封閉式、、、或`foreach`語句。 `break`

```antlr
break_statement
    : 'break' ';'
    ;
```

`break`語句的目標是`switch`最接近之封入、 `for` `while` `do`、、或`foreach`語句的結束點。 `switch` `while`如果語句不是`for`以、、 `do`、或`foreach`語句括住，就會發生編譯時期錯誤。 `break`

當多`switch`個`while` `break` 、、、或`foreach`語句彼此嵌套時，語句只會套用到最內層的語句。 `do` `for` 若要在多個嵌套層級之間`goto`傳送控制，必須使用語句（[goto 語句](statements.md#the-goto-statement)）。

語句無法結束區塊（[try 語句）。](statements.md#the-try-statement) `finally` `break` 當語句在區塊內發生時`break` ，語句的目標必須在相同`finally`的區塊內，否則會發生編譯時期錯誤。 `finally` `break`

`break`語句的執行方式如下：

*  `try` `finally` `finally` `try`如果語句結束一個或多個具有相關聯區塊的區塊，則控制權一開始會傳送到最內層語句的區塊。 `break` 當控制項到達`finally`區塊的結束點時，控制權會轉移`finally`到下一個封入`try`語句的區塊。 這個程式會重複執行， `finally`直到所有中間`try`語句的區塊都已執行為止。
*  控制權會傳送至`break`語句的目標。

因為語句會無條件地將控制權轉移到別處，所以永遠`break`無法連線到語句的結束點。 `break`

### <a name="the-continue-statement"></a>Continue 語句

`do` `while` `foreach` `for`語句會啟動最接近之封閉式、、或語句的新反復專案。 `continue`

```antlr
continue_statement
    : 'continue' ';'
    ;
```

`continue`語句的目標是最接近之封閉式`while`、 `do`、 `for`或`foreach`語句之內嵌語句的結束點。 `do` `for`如果語句不是以`while`、、或`foreach`語句括住，就會發生編譯時期錯誤。 `continue`

當多`while`個`do`、 `for`、 `continue`或`foreach`語句彼此嵌套時，語句只會套用到最內層的語句。 若要在多個嵌套層級之間`goto`傳送控制，必須使用語句（[goto 語句](statements.md#the-goto-statement)）。

語句無法結束區塊（[try 語句）。](statements.md#the-try-statement) `finally` `continue` 當語句在區塊內發生時`continue` ，語句的目標必須在相同`finally`的區塊內，否則會發生編譯時期錯誤。 `finally` `continue`

`continue`語句的執行方式如下：

*  `try` `finally` `finally` `try`如果語句結束一個或多個具有相關聯區塊的區塊，則控制權一開始會傳送到最內層語句的區塊。 `continue` 當控制項到達`finally`區塊的結束點時，控制權會轉移`finally`到下一個封入`try`語句的區塊。 這個程式會重複執行， `finally`直到所有中間`try`語句的區塊都已執行為止。
*  控制權會傳送至`continue`語句的目標。

因為語句會無條件地將控制權轉移到別處，所以永遠`continue`無法連線到語句的結束點。 `continue`

### <a name="the-goto-statement"></a>goto 陳述式

`goto`語句會將控制項傳輸至以標籤標記的語句。

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

*Identifier*語句的目標`goto`是具有指定標籤的標記語句。 如果目前的函式成員中沒有指定名稱的標籤，或如果`goto`語句不在標籤的範圍內，則會發生編譯時期錯誤。 此規則允許使用`goto`語句，將控制項從嵌套的範圍（但不是）轉換成嵌套的範圍。 在範例中
```csharp
using System;

class Test
{
    static void Main(string[] args) {
        string[,] table = {
            {"Red", "Blue", "Green"},
            {"Monday", "Wednesday", "Friday"}
        };

        foreach (string str in args) {
            int row, colm;
            for (row = 0; row <= 1; ++row)
                for (colm = 0; colm <= 2; ++colm)
                    if (str == table[row,colm])
                         goto done;

            Console.WriteLine("{0} not found", str);
            continue;
    done:
            Console.WriteLine("Found {0} at [{1}][{2}]", str, row, colm);
        }
    }
}
```
`goto`語句是用來將控制項從嵌套的範圍中轉移出來。

`goto case`語句的目標是`switch`立即封入語句（ `case` [switch 語句](statements.md#the-switch-statement)）中的語句清單，其中包含具有給定常數值的標籤。 如果 @no__t 0 的語句不是以 @no__t 1 的語句括住，則為，如果*constant_expression*無法隱含轉換（[隱含轉換](conversions.md#implicit-conversions)）為最接近的封閉式 `switch` 語句的治理類型，或最接近的封入`switch` 語句不包含具有給定常數值的 `case` 標籤，發生編譯時期錯誤。

`goto default`語句的目標是`switch`立即封入語句（ `default` [switch 語句](statements.md#the-switch-statement)）中的語句清單，其中包含標籤。 如果語句未以`switch`語句括住，或最接近的封閉式`switch`語句不包含`default`標籤，則會發生編譯時期錯誤。 `goto default`

語句無法結束區塊（[try 語句）。](statements.md#the-try-statement) `finally` `goto` 當語句在區塊內發生時`goto` ，語句的目標必須在相同`finally`的區塊內，否則會發生編譯時期錯誤。 `finally` `goto`

`goto`語句的執行方式如下：

*  `try` `finally` `finally` `try`如果語句結束一個或多個具有相關聯區塊的區塊，則控制權一開始會傳送到最內層語句的區塊。 `goto` 當控制項到達`finally`區塊的結束點時，控制權會轉移`finally`到下一個封入`try`語句的區塊。 這個程式會重複執行， `finally`直到所有中間`try`語句的區塊都已執行為止。
*  控制權會傳送至`goto`語句的目標。

因為語句會無條件地將控制權轉移到別處，所以永遠`goto`無法連線到語句的結束點。 `goto`

### <a name="the-return-statement"></a>Return 語句

語句會將控制權傳回給出現`return`語句之函數的目前呼叫端。 `return`

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

`add` `void` `set`  `return`沒有運算式的語句只能用在不會計算值的函式成員中，也就是具有結果類型（[方法主體](classes.md#method-body)）的方法、屬性或索引子的存取子、事件`remove`的存取子、實例的函數、靜態的函式或析構函數。

具有運算式的`get` 語句只能在計算值的函式成員中使用，也就是具有非void結果類型的方法、屬性或索引子的存取子，或使用者`return`定義的運算子。 隱含轉換（[隱含](conversions.md#implicit-conversions)轉換）必須存在於運算式的類型與包含函式成員的傳回類型之間。

Return 語句也可以在匿名函式運算式的主體中使用（[匿名](expressions.md#anonymous-function-expressions)函式運算式），並參與判斷哪些函式有哪些轉換存在。

`return`語句出現`finally`在區塊（[try 語句](statements.md#the-try-statement)）中時，會發生編譯時期錯誤。

`return`語句的執行方式如下：

*  `return`如果語句指定運算式，則會評估運算式，並將產生的值轉換為包含函式的傳回類型（藉由隱含轉換）。 轉換的結果會成為函式所產生的結果值。
*  `finally` `catch` `try` `finally`如果語句是以一或多個具有相關聯區塊的區塊括住，則控制項一開始會傳送到最`try`內層語句的區塊。 `return` 當控制項到達`finally`區塊的結束點時，控制權會轉移`finally`到下一個封入`try`語句的區塊。 此程式會重複執行， `finally`直到所有封閉式`try`語句的區塊都已執行為止。
*  如果包含函數不是非同步函式，則會將控制權傳回給包含函式的呼叫端，以及結果值（如果有的話）。
*  如果包含函數是非同步函式，控制權會傳回給目前的呼叫端，而結果值（如果有的話）則會記錄在傳回工作中，如（[枚舉器介面](classes.md#enumerator-interfaces)）中所述。

因為語句會無條件地將控制權轉移到別處，所以永遠`return`無法連線到語句的結束點。 `return`

### <a name="the-throw-statement"></a>Throw 語句

`throw`語句會擲回例外狀況。

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

具有運算式的語句會擲回評估運算式所產生的值。 `throw` 運算式必須代表類別類型的值`System.Exception`，其衍生自`System.Exception`或具有`System.Exception` （或其子類別）做為其有效基類的類型參數類型。 如果運算式的評估產生`null` `System.NullReferenceException` ，會改為擲回。

沒有運算式的`catch` `catch`語句只能用在區塊中，在此情況下，語句會重新擲回該區塊目前正在處理的例外狀況。 `throw`

因為語句會無條件地將控制權轉移到別處，所以永遠`throw`無法連線到語句的結束點。 `throw`

當擲回例外狀況時，控制權會傳送至封`catch`入`try`語句中可處理例外狀況的第一個子句。 從例外狀況點到將控制項傳輸至適當的例外狀況處理常式所擲回的進程稱為「***例外狀況傳播***」。 例外狀況的傳播包含重複評估下列步驟，直到找到符合`catch`例外狀況的子句為止。 在此描述中，***擲回點***是一開始擲回例外狀況的位置。

*  在目前的函式成員中`try` ，會檢查括住擲回點的每個語句。 針對每個`S`語句，從最內層`try`的語句開始，並以`try`最外層的語句結束，會評估下列步驟：

   * `catch`如果的`try`區塊包含擲回點，且如果有一或多個子句，則`catch`會依照外觀的順序檢查子句，以根據中指定的規則來尋找適當的例外狀況處理常式`S`一節[try 語句](statements.md#the-try-statement)。 如果找到相符`catch`的子句，就會藉由將控制權轉移至該`catch`子句的區塊來完成例外狀況傳播。

   * `try`否則，如果區塊`catch`或的`S`區塊包含擲回點，而且如果`S`有`finally`區塊，控制權就會傳送至`finally`區塊。 `finally`如果區塊擲回另一個例外狀況，則會終止處理目前的例外狀況。 否則，當控制項到達`finally`區塊的結束點時，就會繼續處理目前的例外狀況。

*  如果例外狀況處理常式不在目前的函式呼叫中，則函式調用會終止，併發生下列其中一種情況：

   * 如果目前的函式是非非同步，則會針對函式的呼叫者重複上述步驟，其擲回點對應于叫用函式成員的語句。

   * 如果目前的函式為非同步和工作傳回，則會將例外狀況記錄在傳回工作中，此工作會放入「錯誤」或「已取消」狀態，如[列舉值介面](classes.md#enumerator-interfaces)中所述。

   * 如果目前的函式為 async 且傳回 void，則會通知目前線程的同步處理內容，如可列舉[介面](classes.md#enumerable-interfaces)中所述。

*  如果例外狀況處理終止目前線程中的所有函式成員調用，表示執行緒沒有例外狀況的處理常式，則執行緒本身就會終止。 這類終止的影響是「實作為定義」。

## <a name="the-try-statement"></a>Try 語句

`try`語句提供一個機制來攔截執行區塊期間所發生的例外狀況。 此外， `try`語句還能指定當控制項`try`離開語句時，一律會執行的程式碼區塊。

```antlr
try_statement
    : 'try' block catch_clause+
    | 'try' block finally_clause
    | 'try' block catch_clause+ finally_clause
    ;

catch_clause
    : 'catch' exception_specifier? exception_filter?  block
    ;

exception_specifier
    : '(' type identifier? ')'
    ;

exception_filter
    : 'when' '(' expression ')'
    ;

finally_clause
    : 'finally' block
    ;
```

語句有三種可能的`try`形式：

*  後面`try`接著一或多個`catch`區塊的區塊。
*  區塊後面接著`finally`區塊。 `try`
*  後面接著一個或多個`catch`區塊`finally`的區塊，後面接著一個區塊。`try`

當 `catch` 子句指定*exception_specifier*時，類型必須 `System.Exception`、衍生自 `System.Exception` 的類型，或具有 `System.Exception` （或其子類別）做為其有效基類的類型參數類型。

當 @no__t 0 子句同時指定具有*識別碼*的*exception_specifier*時，會宣告給定名稱和類型的***例外狀況變數***。 例外狀況變數會對應至範圍中的區域變數，該變數會透過`catch`子句擴充。 在執行*exception_filter*和*區塊*時，例外狀況變數代表目前正在處理的例外狀況。 基於明確指派檢查的目的，會將例外狀況變數視為在其整個範圍中明確指派。

除非子句包含例外狀況變數名稱，否則無法存取篩選和`catch`區塊中的例外狀況物件。 `catch`

未指定*exception_specifier*的 @no__t 0 子句稱為一般的 `catch` 子句。

某些程式設計語言可能會支援無法以衍生自`System.Exception`的物件形式表示的例外狀況，雖然程式C#代碼不會產生這類例外狀況。 General `catch`子句可用來攔截這類例外狀況。 因此，一般`catch`子句在語義上與指定類型`System.Exception`的不同，前者也會攔截來自其他語言的例外狀況。

為了找出例外狀況的處理常式， `catch`會以詞法順序檢查子句。 如果子句指定類型，但沒有例外狀況篩選準則，則在相同`try`語句中，後面`catch`的子句會發生編譯時期錯誤，以指定與該類型相同或衍生自的類型。 `catch` 如果子句未指定型別，而且沒有篩選準則，它就必須`catch`是該`try`語句的最後一個子句。 `catch`

在區塊內`throw` ，沒有運算式的語句（[throw 語句](statements.md#the-throw-statement)）可以用來重新擲回`catch`區塊攔截到的例外狀況。 `catch` 對例外狀況變數的指派不會改變被重新擲回的例外狀況。

在範例中
```csharp
using System;

class Test
{
    static void F() {
        try {
            G();
        }
        catch (Exception e) {
            Console.WriteLine("Exception in F: " + e.Message);
            e = new Exception("F");
            throw;                // re-throw
        }
    }

    static void G() {
        throw new Exception("G");
    }

    static void Main() {
        try {
            F();
        }
        catch (Exception e) {
            Console.WriteLine("Exception in Main: " + e.Message);
        }
    }
}
```
方法`F`會攔截例外狀況、將一些診斷資訊寫入主控台、改變例外狀況變數，然後重新擲回例外狀況。 重新擲回的例外狀況是原始的例外狀況，因此產生的輸出為：
```console
Exception in F: G
Exception in Main: G
```

如果第一個 catch 區塊`e`已擲回，而不是重新擲回目前的例外狀況，則產生的輸出會如下所示：
```console
Exception in F: G
Exception in Main: F
```

當`break`、或`finally` `continue`語句將控制權轉移到區塊時，就會發生編譯時期錯誤。`goto` `break`當、 `continue` `finally`或語句出現在區塊中時，語句的目標必須在相同的區塊內，否則就會發生編譯時期錯誤。`finally` `goto`

在`return` 區塊`finally`中發生語句時，會產生編譯時期錯誤。

`try`語句的執行方式如下：

*  控制權會傳送至`try`區塊。
*  當控制項到達`try`區塊的結束點時：
   *  如果語句具有區塊，則會執行`finally`區塊。 `finally` `try`
   *  控制權會傳送至`try`語句的結束點。

*  如果在`try`區塊執行期間將例外`try`狀況傳播至語句：
   *  `catch`子句（如果有的話）會依照外觀的順序來檢查，以找出適用于例外狀況的處理常式。 `catch`如果子句未指定類型，或指定例外狀況類型或例外狀況類型的基底類型：
      *  `catch`如果子句宣告例外狀況變數，則會將例外狀況物件指派給例外狀況變數。
      *  `catch`如果子句宣告例外狀況篩選準則，就會評估篩選準則。 如果評估為`false`，則 catch 子句不相符，而且搜尋會繼續進行適當處理常式的任何`catch`後續子句。
      *  否則， `catch`子句會被視為相符，而且控制權會轉移至相符`catch`的區塊。
      *  當控制項到達`catch`區塊的結束點時：
         * 如果語句具有區塊，則會執行`finally`區塊。 `finally` `try`
         * 控制權會傳送至`try`語句的結束點。
      *  如果在`catch`區塊執行期間將例外`try`狀況傳播至語句：
         *  如果語句具有區塊，則會執行`finally`區塊。 `finally` `try`
         *  例外狀況會傳播至下一個封閉式`try`語句。
   *  如果語句沒有子句，或如果沒有`catch`子句符合此例外狀況： `catch` `try`
      *  如果語句具有區塊，則會執行`finally`區塊。 `finally` `try`
      *  例外狀況會傳播至下一個封閉式`try`語句。

當控制項`finally` `try`離開語句時，一律會執行區塊的語句。 無論是因為執行`break`、 `continue`、 `goto`或`return`語句而造成控制轉移，或是因為將例外狀況傳播到`try`以外的原因而造成的，都是如此。句.

如果在`finally`區塊執行期間擲回例外狀況，而且不是在相同的 finally 區塊內攔截到，則會將例外狀況傳播至`try`下一個封入語句。 如果正在傳播另一個例外狀況，則會遺失該例外狀況。 傳播例外狀況的程式會在`throw`語句的描述中進一步討論（[throw 語句](statements.md#the-throw-statement)）。

如果`try`可以連線到`try` `try`語句，則可以連接語句的區塊。

如果`catch`可以連線到`try` `try`語句，就可以連線到語句的區塊。

如果`finally`可以連線到`try` `try`語句，則可以連接語句的區塊。

如果下列兩個條件`try`都成立，就可以觸達語句的結束點：

*  可以連線到`try`區塊的結束點，或至少有一個`catch`區塊的端點可供連線。
*  如果有`finally`區塊存在，就可以連線到區塊的結束點。 `finally`

## <a name="the-checked-and-unchecked-statements"></a>Checked 和 unchecked 語句

和語句是用來控制整數型別算數運算和轉換的***溢位檢查內容。*** `checked` `unchecked`

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

語句會在檢查的`unchecked`內容中評估*區塊*中的所有運算式，而語句會導致區塊中的所有運算式在未檢查的內容中進行評估。 `checked`

`checked`和語句`unchecked`相當于和`unchecked`運算子（[checked 和 unchecked 運算子](expressions.md#the-checked-and-unchecked-operators)），不同之處在于它們會在區塊上運作，而不是運算式。 `checked`

## <a name="the-lock-statement"></a>Lock 語句

`lock`語句會取得指定物件的互斥鎖定、執行語句，然後釋放鎖定。

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

@No__t-0 語句的運算式必須代表已知為*reference_type*之類型的值。 在 `lock` 語句的運算式中，不會執行隱含的裝箱轉換（[裝箱轉換](conversions.md#boxing-conversions)），因此該運算式會發生編譯時期錯誤，表示*value_type*的值。

表單的語句`lock`
```csharp
lock (x) ...
```
其中 `x` 是*reference_type*的運算式，它會精確地等同于
```csharp
bool __lockWasTaken = false;
try {
    System.Threading.Monitor.Enter(x, ref __lockWasTaken);
    ...
}
finally {
    if (__lockWasTaken) System.Threading.Monitor.Exit(x);
}
```
但只會評估 `x` 一次。

持有互斥鎖定時，在相同執行執行緒中執行的程式碼也可以取得和釋放鎖定。 不過，在其他執行緒中執行的程式碼會被封鎖而無法取得鎖定，直到釋放鎖定為止。

不`System.Type`建議鎖定物件來同步處理靜態資料的存取。 其他程式碼可能會鎖定相同的類型，這可能會導致鎖死。 較好的方法是藉由鎖定私用靜態物件來同步處理靜態資料的存取。 例如:
```csharp
class Cache
{
    private static readonly object synchronizationObject = new object();

    public static void Add(object x) {
        lock (Cache.synchronizationObject) {
            ...
        }
    }

    public static void Remove(object x) {
        lock (Cache.synchronizationObject) {
            ...
        }
    }
}
```

## <a name="the-using-statement"></a>using 陳述式

`using`語句會取得一或多個資源、執行語句，然後處置資源。

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

***資源***是執行的類別或結構`System.IDisposable`，其中包含名為`Dispose`的單一無參數方法。 使用資源的程式碼可以呼叫`Dispose` ，以指出不再需要資源。 如果`Dispose`未呼叫，則自動處置最後會成為垃圾收集的結果。

如果*resource_acquisition*的格式為*local_variable_declaration* ，則*local_variable_declaration*的類型必須是 `dynamic` 或可以隱含地轉換成 `System.IDisposable` 的類型。 如果*resource_acquisition*的形式為*expression* ，則此運算式必須可以隱含地轉換為 `System.IDisposable`。

在*resource_acquisition*中宣告的區域變數是唯讀的，而且必須包含初始化運算式。 如果內嵌語句嘗試修改這些本機變數（透過指派`++`或和`--`運算子）、接受其位址`ref` ，或將它們當做或`out`參數傳遞，就會發生編譯時期錯誤。

`using`語句會轉譯成三個部分： [取得]、[使用方式] 和 [處置]。 資源的使用會隱含地包含在`try` `finally`包含子句的語句中。 此`finally`子句會處置資源。 如果取得`Dispose`資源，則不會對進行呼叫，也不會擲回任何例外狀況。 `null` 如果資源的類型`dynamic`為，則會在取得期間透過隱含動態轉換（[隱含動態](conversions.md#implicit-dynamic-conversions)轉換）動態`IDisposable`轉換成，以確保在使用方式之前轉換成功供.

表單的語句`using`
```csharp
using (ResourceType resource = expression) statement
```
對應至三個可能擴充的其中一個。 當`ResourceType`是不可為 null 的實數值型別時，展開為
```csharp
{
    ResourceType resource = expression;
    try {
        statement;
    }
    finally {
        ((IDisposable)resource).Dispose();
    }
}
```

否則，當`ResourceType`是可為 null 的實值型別或以外`dynamic`的引用型別時，展開就是
```csharp
{
    ResourceType resource = expression;
    try {
        statement;
    }
    finally {
        if (resource != null) ((IDisposable)resource).Dispose();
    }
}
```

否則，當`ResourceType`為`dynamic`時，展開為
```csharp
{
    ResourceType resource = expression;
    IDisposable d = (IDisposable)resource;
    try {
        statement;
    }
    finally {
        if (d != null) d.Dispose();
    }
}
```

在任一擴充中， `resource`此變數在內嵌語句中都是唯讀的， `d`而且內嵌語句中的變數無法存取，且不會對其隱藏。

執行時，您可以使用不同的方式來執行指定的 using 語句，例如基於效能的考慮，前提是該行為與上述展開一致。

表單的語句`using`
```csharp
using (expression) statement
```
有三個可能的擴充。 在此情況`ResourceType`下`expression`，會隱含地編譯時間型別（如果有的話）。 否則，介面`IDisposable`本身會當做使用。 `ResourceType` 在內嵌的語句中，變數無法在中存取，而且不會隱藏。`resource`

當*resource_acquisition*採用*local_variable_declaration*的形式時，可以取得指定類型的多個資源。 表單的語句`using`
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
會精確地等同于一系列的`using`嵌套語句：
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

下列範例會建立名為`log.txt`的檔案，並將兩行文字寫入檔案。 然後，此範例會開啟相同的檔案來讀取，並將包含的文字行複製到主控台。
```csharp
using System;
using System.IO;

class Test
{
    static void Main() {
        using (TextWriter w = File.CreateText("log.txt")) {
            w.WriteLine("This is line one");
            w.WriteLine("This is line two");
        }

        using (TextReader r = File.OpenText("log.txt")) {
            string s;
            while ((s = r.ReadLine()) != null) {
                Console.WriteLine(s);
            }

        }
    }
}
```

`IDisposable` `using`由於和類別`TextReader`會實作為介面，因此此範例可以使用語句來確保基礎檔案在寫入或讀取作業之後正確地關閉。 `TextWriter`

## <a name="the-yield-statement"></a>Yield 語句

語句會用於反覆運算器區塊（[區塊](statements.md#blocks)），以產生反覆運算器的值給枚舉器物件（[枚舉器物件](classes.md#enumerator-objects)）或可列舉物件（[可列舉物件](classes.md#enumerable-objects)），或表示反復專案結束。`yield`

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

`yield`不是保留字;只有當緊接在`return`或`break`關鍵字之前使用時，它才有特殊意義。 在其他內容中`yield` ，可以當做識別碼使用。

`yield`語句可能出現的位置有幾項限制，如下所述。

*  在*method_body*、 *operator_body*或*accessor_body*外出現的 @no__t 0 語句（任一形式）時，會發生編譯時期錯誤。
*  在匿名函式中出現的`yield`語句（其中一種形式）會發生編譯時期錯誤。
*  `yield`語句的`finally` 子句`try` （其中一種形式）會發生編譯時期錯誤。
*  `yield return`語句`catch` 出現`try`在包含任何子句之語句中的任何位置時，會發生編譯時期錯誤。

下列範例顯示語句的`yield`一些有效和無效用法。

```csharp
delegate IEnumerable<int> D();

IEnumerator<int> GetEnumerator() {
    try {
        yield return 1;        // Ok
        yield break;           // Ok
    }
    finally {
        yield return 2;        // Error, yield in finally
        yield break;           // Error, yield in finally
    }

    try {
        yield return 3;        // Error, yield return in try...catch
        yield break;           // Ok
    }
    catch {
        yield return 4;        // Error, yield return in try...catch
        yield break;           // Ok
    }

    D d = delegate { 
        yield return 5;        // Error, yield in an anonymous function
    }; 
}

int MyMethod() {
    yield return 1;            // Error, wrong return type for an iterator block
}
```

隱含轉換（[隱含](conversions.md#implicit-conversions)轉換）必須存在於`yield return`語句中的運算式類型到反覆運算器的產生類型（[yield 類型](classes.md#yield-type)）。

`yield return`語句的執行方式如下：

*  語句中所指定的運算式會進行評估、隱含地轉換成 yield 型別，以及指派`Current`給列舉值物件的屬性。
*  反覆運算器區塊的執行已暫停。 如果語句位於一個或多個`try`區塊內，此時不`finally`會執行相關聯的區塊。 `yield return`
*  列舉`MoveNext` 值`true`物件的方法會傳回其呼叫端，表示枚舉器物件已成功前進到下一個專案。

下一次呼叫列舉值物件的`MoveNext`方法時，會繼續從上次暫停的位置執行反覆運算器區塊。

`yield break`語句的執行方式如下：

*  `try` `finally` `finally` `try`如果語句是以一或多個具有相關聯區塊的區塊括住，則控制項一開始會傳送到最內層語句的區塊。 `yield break` 當控制項到達`finally`區塊的結束點時，控制權會轉移`finally`到下一個封入`try`語句的區塊。 此程式會重複執行， `finally`直到所有封閉式`try`語句的區塊都已執行為止。
*  控制項會傳回 iterator 區塊的呼叫端。 這可以是列舉`MoveNext`值物件`Dispose`的方法或方法。

因為語句會無條件地將控制權轉移到別處，所以永遠`yield break`無法連線到語句的結束點。 `yield break`
