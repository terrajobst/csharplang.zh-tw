# <a name="statements"></a>陳述式

C# 提供各種不同的陳述式。 大部分的這些陳述式會熟悉的開發人員有程式設計 C 和 c + + 的經驗。

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

*Embedded_statement*非終端項用於出現在其他陳述式的陳述式。 善用*embedded_statement*而非*陳述式*排除的宣告陳述式和標記陳述式，在這些內容中的使用。 此範例
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
導致編譯時期錯誤，因為`if`陳述式需要*embedded_statement*而非*陳述式*其如果分支。 如果此程式碼允許的然後將變數`i`就會被宣告，但它永遠都用。 不過請注意，加上`i`的區塊中的宣告，此範例是有效的。

## <a name="end-points-and-reachability"></a>結束點和連線能力

每個陳述式已***結束點***。 簡單來說，陳述式的結束點是緊接在後面的陳述式的位置。 複合陳述式 （包含內嵌的陳述式的陳述式） 執行規則指定控制到達結束點的內嵌陳述式時所要採取的動作。 比方說，當控制項到達結束點的區塊中的陳述式，控制權會轉移至下一個陳述式區塊中。

如果陳述式可能會執行觸達，陳述式要***連線到***。 相反地，如果沒有，則會執行陳述式不可能，陳述式要***無法連線到***。

在範例
```csharp
void F() {
    Console.WriteLine("reachable");
    goto Label;
    Console.WriteLine("unreachable");
    Label:
    Console.WriteLine("reachable");
}
```
第二個引動過程的`Console.WriteLine`是不可能執行到的因為不會執行陳述式可能會發生。

如果編譯器判斷陳述式是不可能執行到，則會回報警告。 它是特別不是錯誤有無法到達陳述式。

若要判斷是否可連線的特定陳述式或結束點，編譯器會執行資料流程分析，根據每個陳述式所定義的可執行性規則。 流量分析會考量常數運算式的值 ([常數運算式](expressions.md#constant-expressions))，以控制行為的陳述式，但不是會考慮非常數運算式的可能值。 換句話說，為了控制流程分析的詳細資訊，是非常數運算式，指定型別的被視為該類型的任何可能的值。

在範例
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
布林值運算式`if`陳述式是常數運算式，因為這兩個運算元的`==`運算子都是常數。 因為在編譯時期評估常數的運算式，產生值`false`，則`Console.WriteLine`引動過程會被視為無法連線。 不過，如果`i`已變更為本機變數
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
`Console.WriteLine`引動過程會被視為可以連線，即使事實上，它將永遠不會執行。

*區塊*函式的成員一律會視為連線。 藉由後續評估每個陳述式區塊中的可執行性規則，您可以判斷任何指定的陳述式的連線能力。

在範例
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
第二個連線能力`Console.WriteLine`判斷方式如下：

*  第一個`Console.WriteLine`運算式陳述式因為區塊`F`方法可連線。
*  第一個結束點`Console.WriteLine`運算式陳述式是可連線，因為該陳述式連接。
*  `if`陳述式是可連線，因為結束點的第一個`Console.WriteLine`運算式陳述式。
*  第二個`Console.WriteLine`運算式陳述式因為的布林運算式`if`陳述式並沒有常數值`false`。

有兩種情況，就可陳述式的結束點的編譯時期錯誤：

*  因為`switch`陳述式不允許下一節中，切換至 「 繼續 」 參數區段，它是陳述式清單，可連線的 switch 區段的結束點的編譯時期錯誤。 如果發生此錯誤，它通常是表示，`break`遺漏陳述式。
*  它會計算要連線到值的函式成員的區塊的結束點的編譯時期錯誤。 如果發生此錯誤，通常就表示，`return`遺漏陳述式。

## <a name="blocks"></a>區塊

「區塊」可允許在許可單一陳述式的內容中撰寫多個陳述式。

```antlr
block
    : '{' statement_list? '}'
    ;
```

A*區塊*包含選擇性*statement_list* ([陳述式會列出](statements.md#statement-lists))、 大括號括住。 如果省略陳述式清單，則區塊稱為空白。

區塊可能包含宣告陳述式 ([宣告陳述式](statements.md#declaration-statements))。 本機變數或常數的範圍內的區塊中宣告是區塊。

區塊會執行，如下所示：

*  如果區塊是空的則控制權會轉移到區塊的結束點。
*  如果區塊不是空的則控制權會轉移到陳述式清單中。 時，並控制到達結束點的陳述式清單，控制權會轉移到區塊的結束點。

如果區塊本身是可連線到區塊的陳述式清單。

如果區塊是空白，或結束點的陳述式清單是可連線到區塊的結束點。

A*區塊*，其中包含一或多個`yield`陳述式 ([yield 陳述式](statements.md#the-yield-statement)) 呼叫迭代器區塊。 迭代器區塊用來實作的迭代器的函式成員 ([迭代器](classes.md#iterators))。 一些其他的限制將套用至迭代器區塊：

*  它是編譯時期錯誤`return`才會出現在迭代器區塊的陳述式 (但`yield return`允許陳述式)。
*  它是迭代器區塊包含不安全的內容的編譯時間錯誤 ([Unsafe 內容](unsafe-code.md#unsafe-contexts))。 迭代器區塊一律會定義安全的內容中，即使其宣告為巢狀方式置於不安全的內容。

### <a name="statement-lists"></a>陳述式清單

A***陳述式清單***順序中編寫的一或多個陳述式所組成。 陳述式清單中發生*區塊*s ([區塊](statements.md#blocks)) 並在*switch_block*s ([switch 陳述式](statements.md#the-switch-statement))。

```antlr
statement_list
    : statement+
    ;
```

將控制權傳輸至第一個陳述式時，會執行陳述式清單。 時，並控制到達結束點，陳述式，控制權會轉移至下一個陳述式。 時，並控制到達結束點的最後一個陳述式，控制權會轉移至陳述式清單的結束點。

陳述式清單中的陳述式是可連線，如果至少下列其中一項條件成立：

*  陳述式是第一個陳述式，且連線到本身，陳述式清單。
*  連線到前面的陳述式結束點。
*  陳述式是加上標籤的陳述式和標籤由可存取參考`goto`陳述式。

如果在清單中的最後一個陳述式結束點連線到連線到結束點的陳述式清單。

## <a name="the-empty-statement"></a>空陳述式

*Empty_statement*不執行任何動作。

```antlr
empty_statement
    : ';'
    ;
```

沒有任何作業的內容中執行需要一個陳述式的情況時，會使用空的陳述式。

執行空白的陳述式只會將控制權傳輸至陳述式的結束點。 因此，空的陳述式結束點是空的陳述式是否可連線到。

寫入時，就可以使用空的陳述式`while`null 主體陳述式：
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

此外，空的陳述式可用來宣告結尾之前的標籤 「`}`"區塊的：
```csharp
void F() {
    ...
    if (done) goto exit;
    ...
    exit: ;
}
```

## <a name="labeled-statements"></a>標記陳述式

A *labeled_statement*允許要加上標籤的陳述式。 標記陳述式允許在區塊內，但不是允許做為內嵌的陳述式。

```antlr
labeled_statement
    : identifier ':' statement
    ;
```

Labeled 陳述式使用指定的名稱會宣告一個標籤*識別碼*。 標籤的範圍是整個區塊標籤宣告，包括所有巢狀區塊。 它是具有重疊範圍的相同名稱的兩個標籤的編譯時期錯誤。

您可以從參考的標籤`goto`陳述式 ([goto 陳述式](statements.md#the-goto-statement)) 範圍內的標籤。 這表示`goto`陳述式可以傳輸控制區塊內和區塊，但永遠不會分成區塊。

標籤會有自己的宣告空間，而且不會干擾其他識別項。 此範例
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
無效，而且會使用名稱`x`做為參數和標籤。

加上標籤的陳述式的執行就相當於下列標籤陳述式執行。

除了一般控制流程所提供的連線能力，加上標籤的陳述式是如果標籤參考可連線到`goto`陳述式。 (例外狀況：如果`goto`陳述式位於`try`包含`finally`區塊，並加上標籤的陳述式已超出`try`，和結束點的`finally`區塊是無法連上，然後加上標籤的陳述式不是可從連接`goto`陳述式。)

## <a name="declaration-statements"></a>宣告陳述式

A *declaration_statement*宣告本機變數或常數。 宣告陳述式允許在區塊內，但不是允許做為內嵌的陳述式。

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a>本機變數宣告

A *local_variable_declaration*會宣告一個或多個本機變數。

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

*Local_variable_type*的*local_variable_declaration*直接指定變數的宣告所引進的類型，或具有識別項表示`var`，應該根據初始設定式推斷類型。 類型後面接著一份*local_variable_declarator*s，其中每一個導入了新的變數。 A *local_variable_declarator*組成*識別項*可命名變數，並且選擇性地加 」`=`"語彙基元和*local_variable_initializer*提供變數的初始值。

在區域變數宣告的內容中，識別碼 var 做為內容的關鍵字 ([關鍵字](lexical-structure.md#keywords))。當*local_variable_type*指定為`var`且沒有名為的型別`var`是在範圍內，宣告是***隱含類型區域變數宣告***，其型別是從相關聯的初始設定式運算式的型別推斷。 隱含類型區域變數宣告受限於下列限制：

*  *Local_variable_declaration*不能包含多個*local_variable_declarator*s。
*  *Local_variable_declarator*必須包含*local_variable_initializer*。
*  *Local_variable_initializer*必須*運算式*。
*  初始設定式*運算式*類型必須是編譯時期。
*  初始設定式*運算式*宣告的變數本身不能參考

不正確隱含類型區域變數宣告的範例如下：

```csharp
var x;               // Error, no initializer to infer type from
var y = {1, 2, 3};   // Error, array initializer not permitted
var z = null;        // Error, null does not have a type
var u = x => x + 1;  // Error, anonymous functions do not have a type
var v = v++;         // Error, initializer cannot refer to variable itself
```

在運算式中使用，取得區域變數的值*simple_name* ([簡單名稱](expressions.md#simple-names))，並使用修改的本機變數值*指派*([指派運算子](expressions.md#assignment-operators))。 必須明確地指派本機變數 ([明確指派](variables.md#definite-assignment)) 在其中取得它的值是每個位置。

範圍中宣告的區域變數*local_variable_declaration*是區塊中宣告已發生。 它是錯誤，請參閱之前的文字位置中的區域變數*local_variable_declarator*的區域變數。 本機變數的範圍，它是宣告另一個本機變數或常數具有相同名稱的編譯時期錯誤。

區域變數宣告會宣告多個變數，相當於多個具有相同類型的單一變數宣告。 此外，在本機變數中宣告的變數初始設定式，就相當於宣告之後，立即插入在指派陳述式。

此範例
```csharp
void F() {
    int x = 1, y, z = x * 2;
}
```
完全對應
```csharp
void F() {
    int x; x = 1;
    int y;
    int z; z = x * 2;
}
```

隱含類型區域變數宣告中，所宣告的本機變數的類型會是用來將變數初始化運算式的類型相同。 例如: 
```csharp
var i = 5;
var s = "Hello";
var d = 1.0;
var numbers = new int[] {1, 2, 3};
var orders = new Dictionary<int,Order>();
```

隱含類型區域變數宣告上述是相當於下列明確類型宣告：
```csharp
int i = 5;
string s = "Hello";
double d = 1.0;
int[] numbers = new int[] {1, 2, 3};
Dictionary<int,Order> orders = new Dictionary<int,Order>();
```

### <a name="local-constant-declarations"></a>區域常數宣告

A *local_constant_declaration*宣告一或多個區域的常數。

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

*型別*的*local_constant_declaration*指定的宣告所引進的常數類型。 類型後面接著一份*constant_declarator*s，其中每一個導入了新的常數。 A *constant_declarator*組成*識別項*該名稱的常數，後面加上 「`=`"語彙基元，後面接著*constant_expression* ([常數運算式](expressions.md#constant-expressions)) 提供常數值。

*型別*並*constant_expression*的區域常數宣告必須遵循相同的常數成員宣告的規則 ([常數](classes.md#constants))。

本機常數的值取得在運算式中使用*simple_name* ([簡單名稱](expressions.md#simple-names))。

本機常數的範圍是區塊中宣告已發生。 它是錯誤之前的文字位置的區域常數是指其*constant_declarator*。 範圍內的區域常數，它可以是宣告另一個本機變數或常數具有相同名稱的編譯時期錯誤。

在區域常數宣告會宣告多個常數，相當於單一常數的多個具有相同類型的宣告。

## <a name="expression-statements"></a>運算式陳述式

*Expression_statement*評估指定的運算式。 值計算運算式，如果有的話，將會被捨棄。

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

並非所有運算式都允許做為陳述式中。 特別是運算式，例如在`x + y`和`x == 1`，只是計算值 （這將會被捨棄）、 不允許做為陳述式。

執行*expression_statement*會評估包含的運算式，並再將控制權傳輸至結束點*expression_statement*。 結束點*expression_statement*連線，如果該*expression_statement*連線。

## <a name="selection-statements"></a>選取範圍陳述式

選取範圍陳述式選取其中一個可能根據某個運算式的值來執行的陳述式的數目。

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a>If 陳述式

`if`陳述式選取根據布林運算式的值來執行的陳述式。

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

`else`部分是語彙最接近的前面加上相關聯`if`所允許的語法。 因此，`if`表單的陳述式
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

`if`陳述式，如下所示：

*  *Boolean_expression* ([布林運算式](expressions.md#boolean-expressions)) 進行評估。
*  如果布林運算式會產生`true`，控制權會轉移到第一個內嵌的陳述式。 時，並控制到達結束點，該陳述式，控制權會轉移至結束點`if`陳述式。
*  如果布林運算式會產生`false`而如果`else`一部份存在，則控制權會轉移到第二個內嵌的陳述式。 時，並控制到達結束點，該陳述式，控制權會轉移至結束點`if`陳述式。
*  如果布林運算式會產生`false`而如果`else`組件不存在，控制權會轉移至結束點`if`陳述式。

第一個內嵌的陳述式`if`陳述式是連線到如果`if`陳述式和布林值的運算式不是常數值`false`。

第二個內嵌的陳述式`if`陳述式，如果有的話，會連線到如果`if`陳述式和布林值的運算式不是常數值`true`。

結束點`if`陳述式是連線到其內嵌的陳述式中至少一個結束點是否可連線。 此外，結束點`if`陳述式沒有`else`部分是連線到如果`if`陳述式和布林值的運算式不是常數值`true`。

### <a name="the-switch-statement"></a>Switch 陳述式

執行選取的 switch 陳述式具有對應至對 switch 運算式的值相關聯的參數標籤的陳述式清單。

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

A *switch_statement*組成關鍵字`switch`，後面接著括號運算式 （稱為 switch 運算式），再接著*switch_block*。 *Switch_block*包含零或多個*switch_section*s，大括號括住。 每個*switch_section*包含一個或多個*switch_label*s 後面*statement_list* ([陳述式會列出](statements.md#statement-lists))。

***型別來控管***的`switch`陳述式會建立 switch 運算式的方法。

*  對 switch 運算式的類型是否`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `bool`， `char`， `string`，或*enum_type*，或如果它是可為 null 的型別對應至其中一個類型，則這是在管理類型`switch`陳述式。
*  否則，只有一個使用者定義的隱含轉換 ([使用者定義轉換](conversions.md#user-defined-conversions)) 控管類型的下列可能的其中一個 switch 運算式的類型必須存在： `sbyte`， `byte`， `short``ushort`， `int`， `uint`， `long`， `ulong`， `char`， `string`，或為 null 的類型對應至其中一種類型。
*  否則，如果沒有這類隱含的轉換存在，或存在多個這類隱含轉換，就會發生編譯時期錯誤。

每個常數的運算式`case`標籤必須表示為隱含轉換的值 ([隱含轉換](conversions.md#implicit-conversions)) 的控管的型別`switch`陳述式。 如果兩個或多個，就會發生編譯時期錯誤`case`標籤在同一個`switch`陳述式指定相同的常數值。

可以有最多一個`default`switch 陳述式中的標籤。

A`switch`陳述式，如下所示：

*  Switch 運算式會評估，並控管的型別轉換。
*  如果在指定的其中一個常數`case`標籤在同一個`switch`陳述式是對 switch 運算式的值相等，控制權會轉移到下列相符的陳述式清單`case`標籤。
*  如果沒有在指定的常數`case`標籤在同一個`switch`陳述式會等於 switch 運算式的值，才`default`標籤存在，則控制權會轉移到之後的陳述式清單`default`標籤。
*  如果沒有任何常數中指定`case`標籤在同一個`switch`陳述式會等於 switch 運算式的值，如果沒有`default`標籤存在，則控制權會轉移至結束點`switch`陳述式。

如果陳述式清單的參數區段的結束點連線到，就會發生編譯時期錯誤。 這稱為 「 不落入 」 規則。 此範例
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
因為沒有 switch 區段具有可聯繫的端點，則會有效。 不同於 C 和 c + + 中，執行的 switch 區段不允許 「 繼續 」 至下一步 的 參數 區段中，與範例
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
會導致編譯時期錯誤。 執行的 switch 區段時接著執行另一個交換器區段，明確`goto case`或`goto default`必須使用陳述式：
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

允許使用多個標籤*switch_section*。 此範例
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
是有效的。 範例沒有違反 「 不落入 」 規則，因為標籤`case 2:`並`default:`都屬於相同*switch_section*。

「 不落入 」 規則會防止發生在 C 和 c + + 中的 bug 的一般類別時`break`不小心省略陳述式。 此外，因為這項規則的參數區段`switch`陳述式可以任意重新排列而不會影響陳述式的行為。 例如，區段`switch`可以反轉上述陳述式，而不會影響陳述式的行為：
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

陳述式清單的參數區段通常結尾`break`， `goto case`，或`goto default`允許陳述式，但是呈現的陳述式清單的結束點無法連線到任何建構。 例如，`while`控制的布林運算式的陳述式`true`已知永遠不會觸達其結束點。 同樣地，`throw`或`return`陳述式會一律傳送其他位置的控制項，並永遠不會到達其結束點。 因此，下列範例是有效的：
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

控管的型別`switch`陳述式可能是型別`string`。 例如: 
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

像是字串等號比較運算子 ([字串等號比較運算子](expressions.md#string-equality-operators))，則`switch`陳述式會區分大小寫和參數的運算式字串完全相符時，才會執行指定的參數區段`case`標籤常數。

當控管的輸入`switch`陳述式`string`，值`null`允許做為 case 標籤常數。

*Statement_list*之*switch_block*可能包含宣告陳述式 ([宣告陳述式](statements.md#declaration-statements))。 本機變數或常數的範圍中的 switch 區塊宣告是 switch 區塊。

指定的參數區段的陳述式清單是連線到如果`switch`陳述式是連線到，而且下列其中一項條件成立：

*  Switch 運算式是非常數的值。
*  Switch 運算式會比對的常數值`case`參數區段中的標籤。
*  Switch 運算式是常數值不符合任何`case`標籤和 [參數] 區段包含`default`標籤。
*  可存取參考的參數區段的參數標籤`goto case`或`goto default`陳述式。

結束點`switch`陳述式是可連線，如果下列其中一項條件成立：

*  `switch`陳述式包含可存取`break`陳述式，結束`switch`陳述式。
*  `switch`陳述式是可連線，switch 運算式是非常數的值，但不含任何`default`標籤已存在。
*  `switch`陳述式並連線到，switch 運算式不符合任何常數值`case`標籤，但不含任何`default`標籤已存在。

## <a name="iteration-statements"></a>反覆運算陳述式

反覆運算陳述式會重複執行內嵌陳述式。

```antlr
iteration_statement
    : while_statement
    | do_statement
    | for_statement
    | foreach_statement
    ;
```

### <a name="the-while-statement"></a>While 陳述式

`while`陳述式有條件地執行內嵌陳述式零或多次。

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

A`while`陳述式，如下所示：

*  *Boolean_expression* ([布林運算式](expressions.md#boolean-expressions)) 進行評估。
*  如果布林運算式會產生`true`，控制權會轉移到內嵌的陳述式。 時，並控制到達內嵌的陳述式的結束點 (可能是從執行`continue`陳述式)，控制權會轉移到開頭`while`陳述式。
*  如果布林運算式會產生`false`，控制權會轉移至結束點`while`陳述式。

內嵌的陳述式內`while`陳述式中，`break`陳述式 ([break 陳述式](statements.md#the-break-statement)) 可用來將控制權移轉給結束點`while`（因而結束反覆項目內嵌的陳述式陳述式），以及`continue`陳述式 ([continue 陳述式](statements.md#the-continue-statement)) 可用來將控制權移轉給內嵌的陳述式的結束點 (藉此來執行的另一個反覆項目`while`陳述式)。

內嵌的陳述式的`while`陳述式是連線到如果`while`陳述式和布林值的運算式不是常數值`false`。

結束點`while`陳述式是可連線，如果下列其中一項條件成立：

*  `while`陳述式包含可存取`break`陳述式，結束`while`陳述式。
*  `while`陳述式和布林值的運算式不是常數值`true`。

### <a name="the-do-statement"></a>Do 陳述式

`do`陳述式有條件地執行內嵌陳述式一次以上。

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

A`do`陳述式，如下所示：

*  控制權會轉移到內嵌的陳述式。
*  時，並控制到達內嵌的陳述式的結束點 (可能是從執行`continue`陳述式)，則*boolean_expression* ([布林運算式](expressions.md#boolean-expressions)) 會評估。 如果布林運算式會產生`true`，控制權會轉移到開頭`do`陳述式。 否則，控制權會轉移到結束點`do`陳述式。

內嵌的陳述式內`do`陳述式中，`break`陳述式 ([break 陳述式](statements.md#the-break-statement)) 可用來將控制權移轉給結束點`do`（因而結束反覆項目內嵌的陳述式陳述式），以及`continue`陳述式 ([continue 陳述式](statements.md#the-continue-statement)) 可用來將控制權移轉給內嵌的陳述式的結束點。

內嵌的陳述式的`do`陳述式是連線到如果`do`陳述式。

結束點`do`陳述式是可連線，如果下列其中一項條件成立：

*  `do`陳述式包含可存取`break`陳述式，結束`do`陳述式。
*  內嵌的陳述式的結束點是連線和布林值的運算式不是常數值`true`。

### <a name="the-for-statement"></a>陳述式

`for`陳述式會評估初始化運算式的順序，然後，條件為 true，重複執行內嵌陳述式，並評估一系列的反覆項目運算式。

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

*For_initializer*，如果有的話，包含*local_variable_declaration* ([區域變數宣告](statements.md#local-variable-declarations)) 或一份*statement_運算式*s ([運算式陳述式](statements.md#expression-statements)) 以逗號分隔。 所宣告的本機變數的範圍*for_initializer*開始*local_variable_declarator*變數並延伸到內嵌的陳述式結尾。 其範圍包括*for_condition*並*for_iterator*。

*For_condition*，如果有的話，必須*boolean_expression* ([布林運算式](expressions.md#boolean-expressions))。

*For_iterator*，如果有的話，包含一份*statement_expression*s ([運算式陳述式](statements.md#expression-statements)) 以逗號分隔。

FOR 陳述式會執行，如下所示：

*  如果*for_initializer*顯示出來，變數的初始設定式，或者將它們寫入的順序會執行陳述式的運算式。 此步驟只會執行一次。
*  如果*for_condition*已存在，它會進行評估。
*  如果*for_condition*不存在，或評估會產生`true`，控制權會轉移到內嵌的陳述式。 時，並控制到達內嵌的陳述式的結束點 (可能是從執行`continue`陳述式)，運算式*for_iterator*，如果任何項目，會評估的順序，而則是另一個反覆項目執行，開始評估*for_condition*上述步驟中。
*  如果*for_condition*存在且評估會產生`false`，控制權會轉移至結束點`for`陳述式。

內嵌的陳述式內`for`陳述式中，`break`陳述式 ([break 陳述式](statements.md#the-break-statement)) 可用來將控制權移轉給結束點`for`（因而結束反覆項目內嵌的陳述式陳述式），以及`continue`陳述式 ([continue 陳述式](statements.md#the-continue-statement)) 可用來將控制權移轉給內嵌的陳述式的結束點 (因此執行*for_iterator*和執行另一個反覆項目`for`陳述式，從開始*for_condition*)。

內嵌的陳述式的`for`陳述式是可連線，如果下列其中一項條件成立：

*  `for`陳述式和 no *for_condition*存在。
*  `for`陳述式是連線到與*for_condition*存在且不需要的常數值`false`。

結束點`for`陳述式是可連線，如果下列其中一項條件成立：

*  `for`陳述式包含可存取`break`陳述式，結束`for`陳述式。
*  `for`陳述式是連線到與*for_condition*存在且不需要的常數值`true`。

### <a name="the-foreach-statement"></a>Foreach 陳述式

`foreach`陳述式會列舉執行內嵌陳述式的集合中的每個項目集合中的項目。

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

*型別*並*識別項*的`foreach`陳述式宣告***反覆運算變數***陳述式。 如果`var`識別項指定為*local_variable_type*，且沒有名為的型別`var`是在範圍內，反覆運算變數即為***隱含型別的反覆運算變數***，其類型會視為的項目類型和`foreach`陳述式中的，依下列指定。 反覆運算變數對應到唯讀區域變數，包括內嵌的陳述式的範圍。 在執行期間`foreach`陳述式中，反覆運算變數代表目前正在執行反覆項目集合項目。 如果內嵌的陳述式嘗試修改反覆項目變數，就會發生編譯時期錯誤 (透過指派或`++`並`--`運算子)，或傳遞的反覆項目變數`ref`或`out`參數。

在下列命令，為求簡潔， `IEnumerable`， `IEnumerator`，`IEnumerable<T>`並`IEnumerator<T>`命名空間中的對應類型是指`System.Collections`和`System.Collections.Generic`。

編譯時間處理 foreach 陳述式會先判斷***集合型別***，***列舉值型別***並***項目型別***運算式。 這項判斷程序進行，如下所示：

*  如果型別`X`的*運算式*是陣列型別，則會從隱含參考轉換`X`來`IEnumerable`介面 (因為`System.Array`實作這個介面)。 ***集合型別***是`IEnumerable`介面***列舉值型別***是`IEnumerator`介面和***項目型別***的項目類型陣列類型`X`。
*  如果型別`X`的*運算式*是`dynamic`的隱含轉換就*運算式*至`IEnumerable`介面 ([隱含的動態轉換](conversions.md#implicit-dynamic-conversions))。 ***集合型別***是`IEnumerable`介面並***列舉值型別***是`IEnumerator`介面。 如果`var`識別項指定為*local_variable_type*則***項目型別***是`dynamic`，否則就是`object`。
*  否則，請判斷是否型別`X`具有適當`GetEnumerator`方法：
   * 對類型的成員查閱`X`識別碼`GetEnumerator`不使用類型引數。 如果成員查詢不會產生相符項目，會產生模稜兩可，或會產生相符項目不是方法群組，請檢查可列舉的介面，如下所述。 如果成員查詢產生的任何項目以外的方法群組或沒有相符項目，就會發出警告建議。
   * 執行使用產生的方法群組和空白的引數清單的多載解析。 如果多載解析會導致沒有適用的方法，導致模稜兩可，或產生單一的最佳方法，但是該方法是靜態或並非公用，檢查可列舉的介面，如下所述。 如果多載解析產生的任何項目以外的模稜兩可的公用執行個體方法或沒有適用的方法，就會發出警告建議。
   * 如果傳回型別`E`的`GetEnumerator`方法不是類別、 結構或介面類型時，錯誤會產生並採取任何進一步的步驟。
   * 成員查詢會針對`E`具有識別碼`Current`不使用類型引數。 如果成員查詢產生沒有相符項目，就會產生錯誤，或結果不是 已允許讀取的公用執行個體屬性，會產生錯誤並採取任何進一步的步驟。
   * 成員查詢會針對`E`具有識別碼`MoveNext`不使用類型引數。 如果成員查詢未產生符合項，發生錯誤，或結果方法群組以外的任何項目，就會發生錯誤，也會採取任何進一步的步驟。
   * 多載解析會對空的引數清單的方法群組。 如果沒有適用的方法，導致模稜兩可或在單一的最佳方法，但該方法的結果中的多載解析結果是靜態或並非公用，或其傳回型別不是`bool`，會產生錯誤並採取任何進一步的步驟。
   * ***集合型別***是`X`，則***列舉值型別***是`E`，和***項目型別***是種`Current`屬性。

*  否則，請檢查可列舉的介面：
   * 所有類型，如果`Ti`包括不的隱含轉換`X`要`IEnumerable<Ti>`，沒有唯一的型別`T`使得`T`不是`dynamic`和其他所有`Ti`沒有隱含轉換`IEnumerable<T>`來`IEnumerable<Ti>`，則***集合型別***介面`IEnumerable<T>`，則***列舉值類型***介面`IEnumerator<T>`，而***項目型別***是`T`。
   * 否則，如果有多個這類的型別`T`，然後會產生錯誤並採取任何進一步的步驟。
   * 否則，如果沒有隱含轉換`X`要`System.Collections.IEnumerable`介面，則***集合型別***是這個介面，***列舉值類型***介面`System.Collections.IEnumerator`，而***項目型別***是`object`。
   * 否則，會產生錯誤並採取任何進一步的步驟。

上述步驟中，如果成功，就會明確產生集合型別`C`，列舉值型別`E`和 項目型別`T`。 Foreach 陳述式的格式
```csharp
foreach (V v in x) embedded_statement
```
會展開為：
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

變數`e`看見或存取運算式不是`x`或內嵌的陳述式或程式的任何其他原始程式碼。 變數`v`是唯讀，在內嵌的陳述式。 如果沒有明確的轉換 ([明確轉換](conversions.md#explicit-conversions)) 從`T`（項目類型） 來`V`( *local_variable_type* foreach 陳述式中)，會產生錯誤也採取任何進一步的步驟。 如果`x`具有值`null`、`System.NullReferenceException`會在執行階段擲回。

實作允許以不同的方式實作指定的 foreach 陳述式，基於效能考量，例如，只要是與上述延伸一致的行為。

放置`v`內 while 迴圈是很重要的擷取所發生的任何匿名函式的方式*embedded_statement*。

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
如果`v`deklarovalo 之外 while 迴圈中，它會共用所有的反覆項目，以及它的值之後迴圈會是最後的值， `13`，這是什麼引動過程的`f`會列印。 相反地，因為每個反覆項目有它自己的變數`v`，其中一個擷取`f`第一次反覆運算都會繼續保存值`7`，也就是會列印。 (注意： 舊版 C# 宣告`v`外部的 while 迴圈。)

本文最後的區塊建構根據下列步驟：

*  如果沒有隱含轉換`E`至`System.IDisposable`介面，然後
   *  如果`E`不可為 null 的實值型別則 finally 子句會展開以語意相等的：

      ```csharp
      finally {
          ((System.IDisposable)e).Dispose();
      }
      ```

   *  否則 finally 子句會展開以語意相等的：

      ```csharp
      finally {
          if (e != null) ((System.IDisposable)e).Dispose();
      }
      ```

   差異在於，如果`E`是實值型別或實值類型，然後轉型的具現化的型別參數`e`到`System.IDisposable`不會發生 boxing。

*  否則，如果`E`是密封型別，finally 子句會展開為空白區塊：

   ```csharp
   finally {
   }
   ```

*  否則，最後的子句會展開以：

   ```csharp
   finally {
       System.IDisposable d = e as System.IDisposable;
       if (d != null) d.Dispose();
   }
   ```    

   區域變數`d`不可見或存取的任何使用者程式碼。 特別是，它不會衝突與任何其他變數其範圍，包括 finally 區塊。

順序`foreach`周遊陣列的元素，如下所示：一維陣列項目會以遞增索引順序周遊，從索引 `0`和結束索引`Length - 1`。 多維陣列，最右邊的維度的索引會開始遞增，則下一步 的左的維度，依此類推到左邊，就會周遊一個項目。

下列範例會列印出在二維陣列中，項目順序中的每個值：
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
```csharp
1.2 2.3 3.4 4.5 5.6 6.7 7.8 8.9
```

在範例
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
型別`n`就會推斷`int`的項目類型`numbers`。

## <a name="jump-statements"></a>跳躍陳述式

跳躍陳述式無條件地將控制權轉移。

```antlr
jump_statement
    : break_statement
    | continue_statement
    | goto_statement
    | return_statement
    | throw_statement
    ;
```

跳躍陳述式將控制權傳輸至位置稱為***目標***跳躍陳述式。

當跳躍陳述式，就會發生在區塊內，該跳躍陳述式的目標是在區塊之外，即稱為跳躍陳述式***結束***區塊。 跳躍陳述式可能會傳送出區塊的控制項，而它永遠不會將控制權轉移到區塊。

跳躍陳述式的執行複雜的中介`try`陳述式。 如果沒有這類`try`陳述式，跳躍陳述式無條件地將控制權從跳躍陳述式到其目標。 如果存在這類的中介`try`陳述式中，執行將會更複雜。 如果跳躍陳述式結束一個或多個`try`區塊相關聯`finally`區塊，控制最初交給`finally`最內層區塊`try`陳述式。 時，並控制到達結束點`finally`區塊中，控制傳輸至`finally`區塊下一步 的封入`try`陳述式。 此程序會重複直到`finally`的所有區塊中介`try`陳述式執行。

在範例
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
`finally`兩個相關聯的區塊`try`控制權會轉移到跳躍陳述式的目標之前，會執行陳述式。

產生的輸出如下所示：
```
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a>Break 陳述式

`break`陳述式會結束最內層`switch`， `while`， `do`， `for`，或`foreach`陳述式。

```antlr
break_statement
    : 'break' ';'
    ;
```

目標`break`陳述式會結束點的最接近的封閉式`switch`， `while`， `do`， `for`，或`foreach`陳述式。 如果`break`陳述式不加`switch`， `while`， `do`， `for`，或`foreach`陳述式，會發生編譯時期錯誤。

當多個`switch`， `while`， `do`， `for`，或`foreach`陳述式巢狀置於彼此`break`陳述式僅適用於最內層的陳述式。 跨多個巢狀層級，將控制項傳遞至`goto`陳述式 ([goto 陳述式](statements.md#the-goto-statement)) 必須使用。

A`break`陳述式無法結束`finally`區塊 ([try 陳述式](statements.md#the-try-statement))。 當`break`陳述式內，就會發生`finally`封鎖的目標`break`陳述式必須在相同`finally`封鎖; 否則會發生編譯時期錯誤。

A`break`陳述式，如下所示：

*  如果`break`陳述式會結束一個或多個`try`區塊相關聯`finally`區塊，控制項一開始會傳送到`finally`的最內層區塊`try`陳述式。 時，並控制到達結束點`finally`區塊中，控制傳輸至`finally`區塊下一步 的封入`try`陳述式。 此程序會重複直到`finally`的所有區塊中介`try`陳述式執行。
*  控制權會轉移到目標的`break`陳述式。

因為`break`陳述式無條件地將控制權傳輸其他位置、 結束點`break`陳述式絕不會是可連線。

### <a name="the-continue-statement"></a>Continue 陳述式

`continue`陳述式開始執行新的反覆查看的最接近的封閉式`while`， `do`， `for`，或`foreach`陳述式。

```antlr
continue_statement
    : 'continue' ';'
    ;
```

目標`continue`陳述式是內嵌的陳述式的最接近的封閉式終點`while`， `do`， `for`，或`foreach`陳述式。 如果`continue`陳述式不加`while`， `do`， `for`，或`foreach`陳述式，會發生編譯時期錯誤。

當多個`while`， `do`， `for`，或`foreach`陳述式巢狀置於彼此`continue`陳述式僅適用於最內層的陳述式。 跨多個巢狀層級，將控制項傳遞至`goto`陳述式 ([goto 陳述式](statements.md#the-goto-statement)) 必須使用。

A`continue`陳述式無法結束`finally`區塊 ([try 陳述式](statements.md#the-try-statement))。 當`continue`陳述式內，就會發生`finally`封鎖的目標`continue`陳述式必須在相同`finally`封鎖; 否則會發生編譯時期錯誤。

A`continue`陳述式，如下所示：

*  如果`continue`陳述式會結束一個或多個`try`區塊相關聯`finally`區塊，控制項一開始會傳送到`finally`的最內層區塊`try`陳述式。 時，並控制到達結束點`finally`區塊中，控制傳輸至`finally`區塊下一步 的封入`try`陳述式。 此程序會重複直到`finally`的所有區塊中介`try`陳述式執行。
*  控制權會轉移到目標的`continue`陳述式。

因為`continue`陳述式無條件地將控制權傳輸其他位置、 結束點`continue`陳述式絕不會是可連線。

### <a name="the-goto-statement"></a>goto 陳述式

`goto`陳述式將控制權傳輸至標籤來標記的陳述式。

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

目標`goto`*識別碼*陳述式是以指定標記的標記陳述式。 如果不存在具有指定名稱的標籤，這是在目前的函式成員，或如果`goto`陳述式不是範圍內的標籤，就會發生編譯時期錯誤。 此規則允許使用`goto`陳述式來轉移控制項巢狀範圍，但不用到巢狀範圍。 在範例
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
`goto`陳述式用來傳送超出巢狀範圍的控制項。

目標`goto case`陳述式是在直接封入陳述式清單`switch`陳述式 ([switch 陳述式](statements.md#the-switch-statement))，其中包含`case`使用指定的常數值的標籤。 如果`goto case`陳述式不加`switch`陳述式中，如果*constant_expression*並未隱含表示可轉換 ([隱含轉換](conversions.md#implicit-conversions)) 控管的型別最接近的封閉式`switch`陳述式，或如果最接近的封閉式`switch`陳述式不包含`case`使用指定的常數值，標籤就會發生編譯時期錯誤。

目標`goto default`陳述式是在直接封入陳述式清單`switch`陳述式 ([switch 陳述式](statements.md#the-switch-statement))，其中包含`default`標籤。 如果`goto default`陳述式不加`switch`陳述式，或如果最接近的封閉式`switch`陳述式不包含`default`加上標籤，就會發生編譯時期錯誤。

A`goto`陳述式無法結束`finally`區塊 ([try 陳述式](statements.md#the-try-statement))。 當`goto`陳述式內，就會發生`finally`封鎖的目標`goto`陳述式必須在相同`finally`區塊，否則會發生編譯時期錯誤或。

A`goto`陳述式，如下所示：

*  如果`goto`陳述式會結束一個或多個`try`區塊相關聯`finally`區塊，控制項一開始會傳送到`finally`的最內層區塊`try`陳述式。 時，並控制到達結束點`finally`區塊中，控制傳輸至`finally`區塊下一步 的封入`try`陳述式。 此程序會重複直到`finally`的所有區塊中介`try`陳述式執行。
*  控制權會轉移到目標的`goto`陳述式。

因為`goto`陳述式無條件地將控制權傳輸其他位置、 結束點`goto`陳述式絕不會是可連線。

### <a name="the-return-statement"></a>Return 陳述式

`return`陳述式將控制權傳回給目前的呼叫端函式所在的`return`陳述式隨即出現。

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

A`return`但沒有運算式的陳述式只能用於不會計算值時，也就是方法的結果類型的函式成員 ([方法主體](classes.md#method-body)) `void`，則`set`屬性存取子或索引子`add`和`remove`存取子的事件、 執行個體建構函式、 靜態建構函式或解構函式。

A`return`陳述式的運算式只能在計算值，也就是具有非 void 結果類型、 方法的函式成員`get`存取子的屬性或索引子或使用者定義的運算子。 隱含的轉換 ([隱含轉換](conversions.md#implicit-conversions)) 必須存在的運算式類型包含函式成員的傳回型別。

陳述式也可以使用匿名函式運算式的主體中傳回 ([匿名函式運算式](expressions.md#anonymous-function-expressions))，並參與，判斷哪些轉換存在這些函式。

它是編譯時期錯誤`return`陳述式才會出現在`finally`區塊 ([try 陳述式](statements.md#the-try-statement))。

A`return`陳述式，如下所示：

*  如果`return`陳述式會指定運算式中，會評估運算式，而且產生的值已轉換為包含函式的傳回類型的隱含轉換。 轉換的結果會成為函式所產生的結果值。
*  如果`return`陳述式會包含一或多個`try`或是`catch`區塊相關聯`finally`區塊，控制項一開始會傳送到`finally`最內層區塊`try`陳述式。 時，並控制到達結束點`finally`區塊中，控制傳輸至`finally`區塊下一步 的封入`try`陳述式。 此程序會重複直到`finally`區塊的所有封入`try`陳述式執行。
*  如果包含的函式不是非同步函式，控制權會傳回結果值，以及包含函式的呼叫端，如果有的話。
*  如果包含的函式是 async 函式，控制權給目前的呼叫端，而且結果值，如果有的話，會記錄在傳回的工作中所述 ([列舉程式介面](classes.md#enumerator-interfaces))。

因為`return`陳述式無條件地將控制權傳輸其他位置、 結束點`return`陳述式絕不會是可連線。

### <a name="the-throw-statement"></a>Throw 陳述式

`throw`陳述式會擲回的例外狀況。

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

A`throw`與運算式的陳述式會擲回的評估運算式所產生的值。 運算式必須將類別類型的值表示`System.Exception`，類別類型衍生自`System.Exception`型別參數類型具有或`System.Exception`（或子類別） 為有效的基底類別。 如果運算式的評估會產生`null`、`System.NullReferenceException`會改為擲回。

A`throw`但沒有運算式的陳述式可以只能用於`catch`封鎖，請在此情況下該陳述式重新擲回例外狀況正在處理由該`catch`區塊。

因為`throw`陳述式無條件地將控制權傳輸其他位置、 結束點`throw`陳述式絕不會是可連線。

擲回例外狀況時，控制權會轉移到第一個`catch`子句中為封入`try`可以處理的例外狀況的陳述式。 從點將控制權傳輸至適當的例外狀況處理常式所擲回的例外狀況的點進行的程序稱為***例外狀況傳播***。 重複評估直到下列步驟所組成的例外狀況傳播`catch`找到符合的例外狀況的子句。 在此說明中，***擲回點***一開始是例外狀況時的位置。

*  在目前的函式成員中，每個`try`擲回點封入陳述式會檢查。 每個陳述式`S`，從最內層`try`陳述式，直到最外層`try`陳述式，會評估下列步驟：

   * 如果`try`區塊`S`圍住擲回點與 S 有一或多個`catch`子句，`catch`子句會檢查中出現的順序來尋找適合的例外狀況，根據規則中指定的處理常式一節[try 陳述式](statements.md#the-try-statement)。 如果相符`catch`子句，藉由將控制權傳輸至該區塊完成例外狀況傳播`catch`子句。

   * 否則，如果`try`區塊或`catch`區塊`S`圍住擲回點而如果`S`有`finally`區塊中，控制傳輸至`finally`區塊。 如果`finally`區塊擲回另一個例外狀況，終止目前的例外狀況的處理。 否則，當控制項到達結束點`finally`區塊，目前的例外狀況的處理會繼續。

*  如果目前的函式引動過程中找不到的例外狀況處理常式，已終止的函式引動過程，並發生下列其中一項：

   * 如果目前的函式為非同步，上述步驟會使用對應至從中叫用函式成員的陳述式擲回點重複函式的呼叫端。

   * 如果目前的函式是 async 和傳回工作，例外狀況會記錄在傳回的工作中，放入發生錯誤或已取消狀態中所述[列舉程式介面](classes.md#enumerator-interfaces)。

   * 如果目前的函式是 async 和傳回 void，目前執行緒的同步處理內容會通知中所述[可列舉的介面](classes.md#enumerable-interfaces)。

*  如果例外狀況處理終止目前的執行緒中所有函式的成員引動過程，指出執行緒有例外狀況，沒有處理常式接著執行緒就會自行結束。 這類終止的影響是由實作定義。

## <a name="the-try-statement"></a>Try 陳述式

`try`陳述式所提供的機制來攔截區塊的執行期間發生的例外狀況。 此外，`try`陳述式讓您能夠指定程式碼一律會執行，當控制離開區塊`try`陳述式。

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

有三種可能的形式`try`陳述式：

*  A`try`區塊後面接著一或多個`catch`區塊。
*  A`try`區塊後面`finally`區塊。
*  A`try`區塊後面接著一或多個`catch`區塊後面`finally`區塊。

當`catch`子句會指定*exception_specifier*的類型必須是`System.Exception`，型別衍生自`System.Exception`或型別參數具有`System.Exception`（或子類別） 作為其有效基底類別。

時`catch`子句會指定這兩*exception_specifier*具有*識別碼*，則***例外狀況變數***宣告指定名稱和類型。 例外狀況變數對應至範圍涵蓋的本機變數`catch`子句。 在執行期間*exception_filter*並*區塊*，例外狀況變數代表目前正在處理的例外狀況。 明確設定檢查的目的而言，例外狀況變數會被視為在其整個範圍中明確指派。

除非`catch`子句會包含例外狀況變數名稱，就無法存取在篩選中的例外狀況物件和`catch`區塊。

A`catch`未指定的子句*exception_specifier*稱為一般`catch`子句。

有些程式語言可能支援的例外狀況，不是可表示為物件衍生自`System.Exception`，不過這類例外狀況可能永遠不會產生 C# 程式碼。 一般`catch`子句可用來攔截這類例外狀況。 因此，一般`catch`子句會從指定的型別語意不相同`System.Exception`，在於前者可能也攔截例外狀況的其他語言。

若要找出例外狀況處理常式`catch`語彙順序進行檢查的子句。 如果`catch`子句指定型別，但沒有例外狀況篩選條件，用於更新的版本是編譯時期錯誤`catch`子句在同一個`try`陳述式，以指定的類型相同，或衍生自輸入。 如果`catch`子句會指定沒有型別和任何篩選條件，它必須是最後一個`catch`子句，`try`陳述式。

內`catch`區塊中，`throw`陳述式 ([throw 陳述式](statements.md#the-throw-statement)) 沒有運算式可以用來重新擲回已攔截的例外狀況`catch`區塊。 例外狀況變數的指派不會改變會重新擲回的例外狀況。

在範例
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
此方法`F`攔截到例外狀況、 一些診斷資訊寫入主控台，改變例外狀況變數和重新擲回例外狀況。 重新擲回的例外狀況會是原始的例外狀況，如此所產生的輸出：
```
Exception in F: G
Exception in Main: G
```

如果第一個 catch 區塊擲回`e`而不是重新擲回目前例外狀況，產生的輸出應如下：
```csharp
Exception in F: G
Exception in Main: F
```

它是編譯時期錯誤`break`， `continue`，或`goto`陳述式時的控制權轉移`finally`區塊。 當`break`， `continue`，或`goto`陳述式中發生`finally`區塊中，陳述式的目標必須是在相同`finally`區塊，或否則會發生編譯時期錯誤。

它是編譯時期錯誤`return`陳述式中發生`finally`區塊。

A`try`陳述式，如下所示：

*  控制權會轉移到`try`區塊。
*  時，並控制到達結束點`try`區塊：
   *  如果`try`陳述式有`finally`區塊中，`finally`區塊執行。
   *  控制權會轉移至結束點`try`陳述式。

*  如果例外狀況會傳播到`try`陳述式執行期間`try`區塊：
   *  `catch`子句中，如果有的話，會檢查中出現的順序來尋找適合的處理常式，例外狀況。 如果`catch`子句未指定類型，或指定的例外狀況型別或基底類型的例外狀況類型：
      *  如果`catch`子句宣告的例外狀況變數、 例外狀況物件指派給例外狀況變數。
      *  如果`catch`子句宣告例外狀況篩選條件，對篩選進行評估。 如果評估為`false`，catch 子句不是相符項目，則會繼續搜尋透過任何後續`catch`適當的處理常式的子句。
      *  否則，請`catch`子句會被視為相符項目，而且控制權會轉移至對應的`catch`區塊。
      *  時，並控制到達結束點`catch`區塊：
         * 如果`try`陳述式有`finally`區塊中，`finally`區塊執行。
         * 控制權會轉移至結束點`try`陳述式。
      *  如果例外狀況會傳播到`try`陳述式執行期間`catch`區塊：
         *  如果`try`陳述式有`finally`區塊中，`finally`區塊執行。
         *  例外狀況會傳播到下一步 的封入`try`陳述式。
   *  如果`try`陳述式沒有`catch`子句或者如果沒有任何`catch`子句與相符的例外狀況：
      *  如果`try`陳述式有`finally`區塊中，`finally`區塊執行。
      *  例外狀況會傳播到下一步 的封入`try`陳述式。

陳述式`finally`當控制離開區塊一律會執行`try`陳述式。 這適用於控制傳輸是否正常執行，因為執行的結果就會發生`break`， `continue`， `goto`，或`return`陳述式，或由於傳播出的例外狀況`try`陳述式。

如果在執行期間擲回例外狀況`finally`區塊中，並不會攔截內相同的 finally 區塊，例外狀況會傳播到下一步 的封入`try`陳述式。 如果另一個例外狀況傳播的過程中，該例外狀況將會遺失。 討論的傳播例外狀況程序的描述中進一步`throw`陳述式 ([throw 陳述式](statements.md#the-throw-statement))。

`try`區塊`try`陳述式是連線到如果`try`陳述式。

A`catch`區塊`try`陳述式是連線到如果`try`陳述式。

`finally`區塊`try`陳述式是連線到如果`try`陳述式。

結束點`try`陳述式是連線到兩個項目時：

*  終點`try`區塊是連線或結束點的至少一個`catch`區塊可連線。
*  如果`finally`區塊已存在，結束點`finally`區塊可連線。

## <a name="the-checked-and-unchecked-statements"></a>Checked 與 unchecked 陳述式

`checked`並`unchecked`陳述式可用來控制***溢位檢查內容***整數型別算術運算和轉換。

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

`checked`陳述式會導致中的所有運算式*區塊*要檢查的內容中評估並`unchecked`陳述式會都導致中的所有運算式*區塊*来進行評估unchecked 的內容。

`checked`並`unchecked`陳述式是相當於`checked`並`unchecked`運算子 ([checked 與 unchecked 運算子](expressions.md#the-checked-and-unchecked-operators))，不同之處在於他們對區塊，而不是運算式.

## <a name="the-lock-statement"></a>Lock 陳述式

`lock`陳述式會取得指定物件的互斥鎖定、 執行陳述式，並再釋放鎖定。

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

運算式`lock`陳述式必須將已知類型的值表示*reference_type*。 沒有任何隱含 boxing 轉換 ([Boxing 轉換](conversions.md#boxing-conversions)) 的運算式執行曾執行`lock`陳述式，因此是要表示的值運算式的編譯時期錯誤*value_type*.

A`lock`表單的陳述式
```csharp
lock (x) ...
```
何處`x`是的運算式*reference_type*，就相當於
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

持有的互斥鎖定時，在相同的執行緒中執行的程式碼亦可取得再釋放鎖定。 不過，其他執行緒中執行的程式碼會封鎖取得鎖定，直到釋放鎖定為止。

鎖定`System.Type`不建議您若要同步處理靜態資料的存取權的物件。 其他程式碼可能會鎖定在相同的型別，可能會導致死結。 更好的方法是藉由鎖定私用靜態物件同步處理靜態資料的存取。 例如: 
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

`using`陳述式會取得一或多個資源、 執行陳述式，然後處置的資源。

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

A ***resource***是類別或結構實作`System.IDisposable`，其中包含名為單一無參數方法`Dispose`。 使用資源的程式碼可以呼叫`Dispose`表示不再需要資源時。 如果`Dispose`未呼叫，然後自動處置最後發生由於記憶體回收。

如果格式*resource_acquisition*是*local_variable_declaration*然後的型別*local_variable_declaration*必須是`dynamic`或型別可隱含地轉換為`System.IDisposable`。 如果格式*resource_acquisition*是*運算式*則此運算式必須是隱含地轉換成`System.IDisposable`。

在宣告區域變數*resource_acquisition*是唯讀的並必須包含初始設定式。 如果內嵌的陳述式嘗試修改這些本機變數，就會發生編譯時期錯誤 (透過指派或`++`並`--`運算子)、 取得位址，或將其做為傳遞`ref`或`out`參數。

A`using`陳述式會轉譯成三個部分︰ 取得、 使用量以及各種可供使用。 使用資源以隱含方式住`try`陳述式，其中包含`finally`子句。 這`finally`子句處置的資源。 如果`null`取得資源，則不需要呼叫`Dispose`進行，而且會擲回任何例外狀況。 如果資源是型別`dynamic`則會以動態方式轉換透過隱含的動態轉換 ([隱含的動態轉換](conversions.md#implicit-dynamic-conversions)) 來`IDisposable`併購，為了確保在轉換期間成功之前的使用量和處置。

A`using`表單的陳述式
```csharp
using (ResourceType resource = expression) statement
```
對應至其中的三個可能的擴充。 當`ResourceType`不可為 null 的實值型別中，展開
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

否則，當`ResourceType`而不是可為 null 的實值型別或參考型別`dynamic`，擴充
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

否則，當`ResourceType`是`dynamic`，擴充
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

其中一個擴充中，`resource`變數是唯讀，在內嵌的陳述式，和`d`變數是在中，無法存取或不可見，內嵌的陳述式。

實作允許以不同的方式實作指定 using 陳述式，基於效能考量，例如，只要是與上述延伸一致的行為。

A`using`表單的陳述式
```csharp
using (expression) statement
```
具有相同的三個可能展開。 在此情況下`ResourceType`是隱含的編譯時期型別`expression`，如果有的話。 否則介面`IDisposable`本身做為`ResourceType`。 `resource`變數是在中，無法存取或不可見，內嵌的陳述式。

當*resource_acquisition*採用的形式*local_variable_declaration*，就能夠取得指定類型的多項資源。 A`using`表單的陳述式
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
精確地相當於一連串的巢狀`using`陳述式：
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

下列範例會建立名為`log.txt`並寫入檔案中的兩行文字。 此範例會開啟該相同的檔案進行讀取，並將包含的行文字複製到主控台。
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

由於`TextWriter`並`TextReader`類別會實作`IDisposable`介面，可以使用範例`using`陳述式，以確保基礎檔案會正確關閉下列寫入或讀取作業。

## <a name="the-yield-statement"></a>Yield 陳述式

`yield`陳述式會在 iterator 區塊 ([區塊](statements.md#blocks)) 來產生列舉值物件的值 ([列舉值物件](classes.md#enumerator-objects)) 或可列舉的物件 ([的可列舉物件](classes.md#enumerable-objects))迭代器，或表示反覆項目結束。

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

`yield` 不是保留的字;它具有特殊意義之前，立即使用時，才`return`或`break`關鍵字。 在其他內容中，`yield`可用來當做識別項。

有幾項限制，在何處`yield`陳述式可以出現，如下列所述。

*  它是編譯時期錯誤`yield`（的任一形式） 的陳述式才會出現外*method_body*， *operator_body*或*accessor_body*
*  它是編譯時期錯誤`yield`（的任一形式） 的陳述式以匿名函式中出現。
*  它是編譯時期錯誤`yield`（的任一形式） 的陳述式才會出現在`finally`子句`try`陳述式。
*  它是編譯時期錯誤`yield return`陳述式出現在任何地方`try`陳述式，其中包含所有`catch`子句。

下列範例示範一些有效和無效使用`yield`陳述式。

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

隱含的轉換 ([隱含轉換](conversions.md#implicit-conversions)) 中的運算式的型別必須存在於`yield return`陳述式來產生型別 ([產生類型](classes.md#yield-type)) 迭代器。

A`yield return`陳述式，如下所示：

*  陳述式中指定的運算式會評估，以隱含方式轉換產生的型別，並指派給`Current`列舉值物件的屬性。
*  迭代器區塊執行的已暫停。 如果`yield return`陳述式位於一或多個`try`區塊中使用，相關聯`finally`區塊不會執行這一次。
*  `MoveNext`列舉值物件的方法會傳回`true`給其呼叫端，指出列舉值物件成功地前移至下一個項目。

Enumerator 物件的下一個呼叫`MoveNext`方法繼續執行從上次已中暫止的迭代器區塊。

A`yield break`陳述式，如下所示：

*  如果`yield break`陳述式會包含一或多個`try`區塊相關聯`finally`區塊，控制項一開始會傳送到`finally`的最內層區塊`try`陳述式。 時，並控制到達結束點`finally`區塊中，控制傳輸至`finally`區塊下一步 的封入`try`陳述式。 此程序會重複直到`finally`區塊的所有封入`try`陳述式執行。
*  程式控制權回到呼叫端的迭代器區塊。 這是`MoveNext`方法或`Dispose`列舉值物件的方法。

因為`yield break`陳述式無條件地將控制權傳輸其他位置、 結束點`yield break`陳述式絕不會是可連線。
