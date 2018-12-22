# <a name="variables"></a>變數

變數代表儲存體位置。 每個變數具有一種類型，判斷哪些值可以儲存在變數中。 C# 是型別安全的語言，以及 C# 編譯器保證變數中儲存的值永遠都適當的型別。 可以變更變數的值，透過指派或透過使用`++`和`--`運算子。

變數必須是***明確指派***([明確指派](variables.md#definite-assignment)) 才能取得其值。

下列各節所述，變數都是***初始指派***或是***最初未指派***。 一開始指派的變數具有定義完善的初始值，一律會視為明確指派。 一開始未指派的變數具有沒有初始值。 一開始未指派的變數會視為已明確指派在特定位置，就指派值給變數必須在該位置的每個可能的執行路徑中。

## <a name="variable-categories"></a>變數類別

C# 定義的變數的七個類別： 靜態變數、 執行個體變數、 陣列元素、 值參數、 參考參數、 輸出參數和區域變數。 下列各節說明這些類別。

在範例
```csharp
class A
{
    public static int x;
    int y;

    void F(int[] v, int a, ref int b, out int c) {
        int i = 1;
        c = a + b++;
    }
}
```
`x` 是靜態的變數，`y`是執行個體變數，`v[0]`是陣列項目`a`值的參數`b`是參考參數，`c`是一個 output 參數，和`i`是區域變數。

### <a name="static-variables"></a>靜態變數

欄位宣告`static`修飾詞會呼叫***靜態變數***。 靜態變數進入是否存在，再執行靜態建構函式 ([靜態建構函式](classes.md#static-constructors)) 包含的類型和中止時的相關聯的應用程式定義域就不會存在。

靜態變數的初始值是預設值 ([預設值](variables.md#default-values)) 的變數的類型。

明確設定檢查的目的而言，靜態變數會被視為一開始指派。

### <a name="instance-variables"></a>執行個體變數

欄位宣告不含`static`修飾詞會呼叫***執行個體變數***。

#### <a name="instance-variables-in-classes"></a>在類別中的執行個體變數

類別的執行個體變數會進入存在，該類別的新執行個體建立時，並沒有參考至該執行個體和執行個體的解構函式 （如果有的話） 執行時停止。

類別的執行個體變數的初始值是預設值 ([預設值](variables.md#default-values)) 的變數的類型。

為了明確設定檢查，類別的執行個體變數會被視為一開始指派。

#### <a name="instance-variables-in-structs"></a>在結構中的執行個體變數

結構的執行個體變數具有完全相同的存留期與其所屬的結構變數。 換句話說，當結構類型的變數進入存在或不再存在，因此過的執行個體變數的結構。

結構的執行個體變數的初始的指派狀態會是包含的結構變數相同。 換句話說，當結構變數會被視為一開始指派，因此也是其執行個體變數，以及結構變數會被視為一開始未指派的當其執行個體變數同樣是未指派。

### <a name="array-elements"></a>陣列項目

陣列的項目進入建立陣列執行個體時，存在，且存在時沒有參考至該陣列執行個體。

每個陣列元素的初始值是預設值 ([預設值](variables.md#default-values)) 的陣列元素的型別。

為了明確設定檢查，陣列元素會被視為一開始指派。

### <a name="value-parameters"></a>值參數

未宣告的參數`ref`或是`out`修飾詞***實值參數***。

進入函式成員 （方法、 執行個體建構函式，存取子或運算子） 或匿名函式引動過程時存在的實值參數的參數所屬的與指定引動過程的引數的值進行初始化。 實值參數通常會停止存在的函式成員 」 或 「 匿名函式傳回時。 不過，如果值參數會擷取匿名函式 ([匿名函式運算式](expressions.md#anonymous-function-expressions))、 存留期會延伸到至少直到委派或從該匿名函式建立的運算式樹狀結構不符合資格記憶體回收。

為了明確設定檢查，實值參數會被視為一開始指派。

### <a name="reference-parameters"></a>傳址參數

參數以宣告`ref`修飾詞***參考參數***。

參考參數不會建立新的存放位置。 相反地，參考參數表示相同的儲存位置，為給定中的函式成員 」 或 「 匿名函式引動過程的引數的變數。 因此，參考參數的值是一律為基礎的變數相同。

下列的明確指派規則適用於參考參數。 請注意，不同的規則，如中所述的輸出參數[輸出參數](variables.md#output-parameters)。

*  變數必須明確進行指派 ([明確指派](variables.md#definite-assignment)) 將它傳遞為函式成員或委派引動過程中參考參數之前。
*  在函式成員或匿名函式中，參考參數視為一開始指派。

執行個體方法或結構類型的執行個體存取子內`this`關鍵字的行為就像參考參數的結構類型 ([這項存取](expressions.md#this-access))。

### <a name="output-parameters"></a>輸出參數

參數以宣告`out`修飾詞***輸出參數***。

輸出參數不會建立新的存放位置。 相反地，輸出參數會表示為變數引數，函式成員或委派引動過程中所指定相同的儲存位置。 因此，輸出參數的值是一律為基礎的變數相同。

下列的明確指派規則適用於輸出參數。 請注意，不同的規則，參考參數中所述[參考參數](variables.md#reference-parameters)。

*  變數必須未明確指派可以傳遞做為輸出參數，在函式成員或委派引動過程之前。
*  下列一般函式成員或委派引動過程完成時，每個輸出參數會被視為已傳遞的變數指派給該執行路徑中。
*  在函式成員或匿名函式中，輸出參數會被視為一開始未指派。
*  函式成員或匿名函式的每個輸出參數必須明確進行指派 ([明確指派](variables.md#definite-assignment)) 的函式之前成員或匿名函式正常地傳回。

結構類型的執行個體建構函式內`this`關鍵字的行為完全相同的結構類型的輸出參數 ([這項存取](expressions.md#this-access))。

### <a name="local-variables"></a>區域變數

A***區域變數***所宣告*local_variable_declaration*，這可能會發生在*區塊*，則*for_statement*， *switch_statement*或 a *using_statement*; 或藉由*foreach_statement*或*specific_catch_clause*如*try_statement*。

本機變數的存留期是程式執行期間要保留給它保證儲存體的部分。 此存留期擴充至少從進入*區塊*， *for_statement*， *switch_statement*， *using_statement*， *foreach_statement*，或*specific_catch_clause*與它相關聯，執行，直到*區塊*， *for_statement*， *switch_statement*， *using_statement*， *foreach_statement*，或*specific_catch_clause*以任何方式結束。 (輸入括住*區塊*或呼叫方法暫停執行，但未結束，執行目前*區塊*， *for_statement*， *switch_statement*， *using_statement*， *foreach_statement*，或*specific_catch_clause*。)如果要將本機變數擷取匿名函式 ([擷取的外部變數](expressions.md#captured-outer-variables))，其存留期擴充至少直到建立匿名函式，以及前往的任何其他物件的委派或運算式樹狀結構參考擷取的變數，可進行記憶體回收。

如果父代*區塊*， *for_statement*， *switch_statement*， *using_statement*， *foreach_statement*，或*specific_catch_clause*輸入以遞迴方式，區域變數的新執行個體每次都會建立，並將其*local_variable_initializer*，如果任何項目，會評估每一次。

所導入的本機變數*local_variable_declaration*不會自動初始化，因此沒有預設值。 為了明確設定檢查，本機變數則是所引進*local_variable_declaration*會被視為一開始未指派。 A *local_variable_declaration*可能會包含*local_variable_initializer*，在此情況下變數會被視為已明確指派只有在初始化運算式之後 ([宣告陳述式](variables.md#declaration-statements))。

所導入的本機變數的範圍內*local_variable_declaration*，它是編譯時期錯誤來參考該區域變數之前的文字位置及其*local_variable_declarator*. 如果是隱含的本機變數宣告 ([區域變數宣告](statements.md#local-variable-declarations))，還有參考此變數中的錯誤及其*local_variable_declarator*。

所導入的本機變數*foreach_statement*或是*specific_catch_clause*會被視為已明確指派的整個範圍中。

本機變數的實際的存留期是視實作而定。 例如，編譯器可能會以靜態方式判斷區塊中的本機變數僅用於該區塊的一小部分中。 使用這項分析，編譯器所產生的結果具有較短的存留時間，比其包含區塊的變數的儲存體中的程式碼。

區域參考變數所參考的儲存體回收與該區域參考變數的存留期無關 ([自動記憶體管理](basic-concepts.md#automatic-memory-management))。

## <a name="default-values"></a>預設值

下列類別的變數會自動初始化為其預設值：

*  靜態變數。
*  類別執行個體的執行個體變數。
*  陣列項目。

變數的預設值取決於該變數的類型，並以下列方式決定：

*  變數*value_type*，預設值是所計算的值相同*value_type*的預設建構函式 ([預設建構函式](types.md#default-constructors))。
*  變數*reference_type*，預設值是`null`。

初始化為預設值通常是藉由讓記憶體管理員，或配置供使用之前，記憶體回收行程會初始化零的所有位元的記憶體。 基於這個理由，很方便地使用零的所有位元來代表 null 參考。

## <a name="definite-assignment"></a>明確指派

在函式成員的指定位置，可執行的程式碼中的變數稱為***明確指派***如果編譯器可以證明，特殊的靜態流程分析 ([精確判斷明確規則指派](variables.md#precise-rules-for-determining-definite-assignment))，已自動初始化變數，或已經過至少一個指派的目標。 明確設定的規則非正式地所述，包括：

*  一開始指派的變數 ([最初指派變數](variables.md#initially-assigned-variables)) 一律會視為已明確指派。
*  一開始未指派的變數 ([一開始未指派的變數](variables.md#initially-unassigned-variables)) 會被視為已明確指派中的特定位置如果該位置的所有可能的執行路徑包含至少下列其中之一：
    * 簡單指派 ([簡單指派](expressions.md#simple-assignment)) 中的變數是左的運算元。
    * 引動過程運算式 ([引動過程運算式](expressions.md#invocation-expressions)) 或物件建立運算式 ([物件建立運算式](expressions.md#object-creation-expressions))，將變數傳遞做為輸出參數。
    * 區域變數，區域變數宣告 ([區域變數宣告](statements.md#local-variable-declarations))，其中包含變數的初始設定式。

上述的非正式規則的基礎正式規格所述[最初指派變數](variables.md#initially-assigned-variables)，[一開始未指派的變數](variables.md#initially-unassigned-variables)，和[來判斷精確的規則明確指派](variables.md#precise-rules-for-determining-definite-assignment)。

明確指派的狀態變數的執行個體*struct_type*變數會追蹤個別也集體。 在上述規則，進行其他的下列規則適用於*struct_type*變數和其執行個體的變數：

*  執行個體變數會視為已明確指派，如果其包含*struct_type*變數會被視為已明確指派。
*  A *struct_type*變數被視為已明確指派，如果每一個它的執行個體變數被視為已明確指派。

明確指派是在下列情況的需求：

*  必須明確指派變數，在其中取得它的值是每個位置。 這可確保未定義的值永遠不會發生。 若要取得變數的值，除非視為變數在運算式中的相符項目
    * 變數是指派的簡單的左的運算元
    * 變數會傳遞做為輸出參數，或
    * 變數是*struct_type*變數，且為成員存取的左運算元。
*  在每個位置，它會傳遞做為參考參數，必須明確指派變數。 這可確保叫用的函式成員，可以考慮一開始指派參考參數。
*  函式成員的所有輸出參數必須明確地都指派每個位置其中函式成員會傳回 (透過`return`陳述式或到達函式成員主體結尾的執行)。 這可確保，函式成員不會傳回未定義的值在輸出參數，如此可讓編譯器考慮使用函式成員引動過程的變數做為輸出參數相當於指派給變數。
*  `this`的變數*struct_type*執行個體建構函式必須在每個位置，其中該執行個體建構函式會傳回明確指派。

### <a name="initially-assigned-variables"></a>一開始指派的變數

下列類別的變數會歸類為初始指派：

*  靜態變數。
*  類別執行個體的執行個體變數。
*  執行個體的初始設定的結構變數的變數。
*  陣列項目。
*  值的參數。
*  參考參數。
*  中所宣告的變數`catch`子句或`foreach`陳述式。

### <a name="initially-unassigned-variables"></a>一開始未指派的變數

下列類別的變數會歸類為一開始未設定：

*  一開始未指派的結構變數的執行個體變數。
*  輸出參數，包括`this`結構執行個體建構函式的變數。
*  本機變數，但不包括在中宣告`catch`子句或`foreach`陳述式。

### <a name="precise-rules-for-determining-definite-assignment"></a>精確判斷明確指派規則

若要判斷已明確指派每個使用的變數，編譯器必須使用相當於這一節所述的程序。

編譯器會處理具有一或多個一開始未指派的變數的每個函式成員的主體。 每一開始未指派的變數*v*，編譯器會判斷***明確指派狀態***for *v*在每個函式成員中的下列各點：

*  每個陳述式的開頭
*  在結束點 ([結束點和可執行性](statements.md#end-points-and-reachability)) 的每個陳述式
*  在每個弧形的將控制權轉移到另一個陳述式或陳述式的結束點
*  在每個運算式的開頭
*  每個運算式的結尾

明確指派狀態*v*可以是：

*  明確指派。 這表示在到目前為止，所有可能的控制流程*v*已被指派值。
*  未明確指派。 變數類型的運算式結尾處的狀態`bool`，歸類到其中一個下列子狀態的變數未明確指派可能 （但不一定） 的狀態：
    * 明確指派之後，則為 true 的運算式。 此狀態表示*v*已明確指派，如果布林運算式評估為 true，但就不需要指派如果布林運算式評估為 false。
    * 明確指派之後，則為 false 的運算式。 此狀態表示*v*已明確指派，如果布林運算式評估為 false，但就不需要指派如果布林運算式評估為 true。

下列規則可控制如何狀態變數*v*決定每個位置。

#### <a name="general-rules-for-statements"></a>陳述式的一般規則

*  *v*函式成員主體中的開頭處未明確指派。
*  *v*已明確指派任何無法到達陳述式的開頭。
*  明確指派狀態*v*開頭的任何其他陳述式檢查來判斷是明確指派狀態*v*上所有的開頭為目標的控制流程傳輸陳述式。 如果 （而且只有當） *v*已明確指派所有此類的控制流程傳輸，然後*v*已明確指派的陳述式開頭。 檢查陳述式連線能力的相同方式決定可能的控制流程傳輸集合 ([結束點和可執行性](statements.md#end-points-and-reachability))。
*  明確指派狀態*v*區塊中，結束點`checked`， `unchecked`， `if`， `while`， `do`， `for`， `foreach`， `lock`， `using`，或`switch`陳述式檢查來判斷是明確指派狀態*v*上所有以該陳述式的結束點為目標的控制流程傳輸。 如果*v*已明確指派所有此類的控制流程傳輸，然後*v*已明確指派陳述式的結束點。 否則*v*未明確指派陳述式的結束點。 檢查陳述式連線能力的相同方式決定可能的控制流程傳輸集合 ([結束點和可執行性](statements.md#end-points-and-reachability))。

#### <a name="block-statements-checked-and-unchecked-statements"></a>區塊陳述式，檢查和 unchecked 陳述式

明確指派狀態*v*控制項傳輸至區塊中的陳述式清單的第一個陳述式 （或區塊中，如果陳述式清單是空的結束點） 等同於明確指派陳述式*v*區塊，前面`checked`，或`unchecked`陳述式。

#### <a name="expression-statements"></a>運算式陳述式

運算式陳述式*stmt*構成的運算式*expr*:

*  *v*具有相同的明確指派狀態的開頭*expr*做為開頭*stmt*。
*  如果*v*如果在結尾明確指派*expr*，它會明確指派的結束點*stmt*; 否則它未明確指派結束點*stmt*。

#### <a name="declaration-statements"></a>宣告陳述式

*  如果*stmt*是宣告陳述式沒有初始設定式，然後*v*具有相同的明確指派狀態的結束點*stmt* 開頭做為*stmt*。
*  如果*stmt*是宣告陳述式與初始設定式，然後明確指派狀態*v*取決於如同*stmt*是陳述式清單中的，具有一個指派初始設定式 （依照順序宣告） 的每個宣告陳述式。

#### <a name="if-statements"></a>如果陳述式

針對`if`陳述式*stmt*的表單：
```csharp
if ( expr ) then_stmt else else_stmt
```

*  *v*具有相同的明確指派狀態的開頭*expr*做為開頭*stmt*。
*  如果*v*已明確指派的結尾*expr*，然後它會明確指派在控制流程傳輸至*then_stmt* ，然後*else_stmt*或結尾處*stmt*如果沒有 else 子句。
*  如果*v*結尾有 「 明確指派，則為 true 的運算式之後 」 的狀態*expr*，然後它會明確指派在控制流程傳輸至*then_stmt*，而非在控制流程傳輸至其中一個已明確指派*else_stmt*或是的高端點*stmt*如果沒有 else 子句。
*  如果*v*結尾有 「 明確指派 false 的運算式之後 」 的狀態*expr*，然後它會明確指派在控制流程傳輸至*else_stmt*，而非在控制流程傳輸至明確指派*then_stmt*。 它在端點的明確指派*stmt*才會明確指派的高端點位於*then_stmt*。
*  否則*v*被視為未明確指派在控制流程傳輸至*then_stmt*或是*else_stmt*，或端點的*stmt*如果沒有 else 子句。

#### <a name="switch-statements"></a>Switch 陳述式

在 `switch`陳述式*stmt*控制運算式具有*expr*:

*  明確指派狀態*v*開頭*expr*等同於狀態*v*開頭*stmt*。
*  明確指派狀態*v*上的控制流程傳送至連線到交換器區塊陳述式清單是明確的指派狀態的相同*v*結尾*expr*.

#### <a name="while-statements"></a>Do-while 陳述式

針對`while`陳述式*stmt*的表單：
```csharp
while ( expr ) while_body
```

*  *v*具有相同的明確指派狀態的開頭*expr*做為開頭*stmt*。
*  如果*v*已明確指派的結尾*expr*，然後它會明確指派在控制流程傳輸至*while_body*並結束點*stmt*。
*  如果*v*結尾有 「 明確指派，則為 true 的運算式之後 」 的狀態*expr*，然後它會明確指派在控制流程傳輸至*while_body*，但不是在端點的明確指派*stmt*。
*  如果*v*結尾有 「 明確指派 false 的運算式之後 」 的狀態*expr*，然後它會明確指派在控制流程傳輸至結束點*stmt*但在控制流程傳輸至未明確指派*while_body*。

#### <a name="do-statements"></a>陳述式

針對`do`陳述式*stmt*的表單：
```csharp
do do_body while ( expr ) ;
```

*  *v*具有相同的明確指派狀態，在控制流程傳輸，從開頭*stmt*要*do_body*做為開頭*stmt*。
*  *v*具有相同的明確指派狀態的開頭*expr*個結束點*do_body*。
*  如果*v*已明確指派的結尾*expr*，然後它會明確指派在控制流程傳輸至結束點*stmt*。
*  如果*v*結尾有 「 明確指派 false 的運算式之後 」 的狀態*expr*，然後它會明確指派在控制流程傳輸至結束點*stmt*.

#### <a name="for-statements"></a>陳述式

正在檢查的明確指派`for`表單的陳述式：
```csharp
for ( for_initializer ; for_condition ; for_iterator ) embedded_statement
```
完成所撰寫的陳述式：
```csharp
{
    for_initializer ;
    while ( for_condition ) {
        embedded_statement ;
        for_iterator ;
    }
}
```

如果*for_condition*中會省略`for`陳述式，則評估的明確指派繼續如同*for_condition*已被取代`true`上述擴充.

#### <a name="break-continue-and-goto-statements"></a>中斷、 繼續和 goto 陳述式

明確指派狀態*v*上所造成的控制流程傳輸`break`， `continue`，或`goto`陳述式是明確的指派狀態的相同*v*在陳述式的開頭。

#### <a name="throw-statements"></a>Throw 陳述式

陳述式*stmt*的表單
```csharp
throw expr ;
```

明確指派狀態*v*開頭*expr*明確指派狀態的相同*v*開頭*stmt*.

#### <a name="return-statements"></a>Return 陳述式

陳述式*stmt*的表單
```csharp
return expr ;
```

*  明確指派狀態*v*開頭*expr*明確指派狀態的相同*v*開頭*stmt*.
*  如果*v*是一個 output 參數，則必須將它明確地指派：
    * 之後*expr*
    * 或在結尾`finally`區塊`try` - `finally`或是`try` - `catch` - `finally`圍住`return`陳述式。

為表單的陳述式陳述式：
```csharp
return ;
```

*  如果*v*是一個 output 參數，則必須將它明確地指派：
    * 之前*陳述式*
    * 或在結尾`finally`區塊`try` - `finally`或是`try` - `catch` - `finally`圍住`return`陳述式。

#### <a name="try-catch-statements"></a>Try catch 陳述式

陳述式*stmt*的表單：
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
```

*  明確指派狀態*v*開頭*try_block*明確指派狀態的相同*v*開頭*stmt*.
*  明確指派狀態*v*開頭*catch_block_i* (針對任何*我*) 的明確指派狀態相同*v*開頭*stmt*。
*  明確指派狀態*v*在結尾處*stmt*是明確指派 （並且唯有） *v*明確指派的高端點*try_block*和每*catch_block_i* (針對每個*我*從 1 到*n*)。

#### <a name="try-finally-statements"></a>Try finally 陳述式

針對`try`陳述式*stmt*的表單：
```csharp
try try_block finally finally_block
```

*  明確指派狀態*v*開頭*try_block*明確指派狀態的相同*v*開頭*stmt*.
*  明確指派狀態*v*開頭*finally_block*明確指派狀態的相同*v*開頭*陳述式*.
*  明確指派狀態*v*在結尾處*stmt*是明確指派 （並且唯有） 至少下列其中一項條件成立：
    * *v*明確指派的高端點*try_block*
    * *v*明確指派的高端點*finally_block*

如果控制流程傳輸 (例如`goto`陳述式) 已變更內開始*try_block*，結束外部*try_block*，然後*v*也是如果被視為已明確指派上該控制流程傳輸*v*明確指派的高端點*finally_block*。 (這不是唯一的 if — 如果*v*明確指派的另一個原因，在 控制流程移轉後，則它仍視為已明確指派。)

#### <a name="try-catch-finally-statements"></a>請嘗試為 try-catch-finally 陳述式

明確指派分析`try` - `catch` - `finally`表單的陳述式：
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
finally *finally_block*
```
完成陳述式所`try` - `finally`封入陳述式`try` - `catch`陳述式：
```csharp
try {
    try try_block
    catch(...) catch_block_1
    ...
    catch(...) catch_block_n
}
finally finally_block
```

下列範例示範如何不同的區塊`try`陳述式 ([try 陳述式](statements.md#the-try-statement)) 會影響明確指派。
```csharp
class A
{
    static void F() {
        int i, j;
        try {
            goto LABEL;
            // neither i nor j definitely assigned
            i = 1;
            // i definitely assigned
        }

        catch {
            // neither i nor j definitely assigned
            i = 3;
            // i definitely assigned
        }

        finally {
            // neither i nor j definitely assigned
            j = 5;
            // j definitely assigned
            }
        // i and j definitely assigned
        LABEL:;
        // j definitely assigned

    }
}
```

#### <a name="foreach-statements"></a>Foreach 陳述式

針對`foreach`陳述式*stmt*的表單：
```csharp
foreach ( type identifier in expr ) embedded_statement
```

*  明確指派狀態*v*開頭*expr*等同於狀態*v*開頭*stmt*。
*  明確指派狀態*v*在控制流程傳輸至*embedded_statement*或結束點*stmt*的狀態相同*v*結尾處*expr*。

#### <a name="using-statements"></a>Using 陳述式

針對`using`陳述式*stmt*的表單：
```csharp
using ( resource_acquisition ) embedded_statement
```

*  明確指派狀態*v*開頭*resource_acquisition*等同於狀態*v*開頭*stmt*.
*  明確指派狀態*v*在控制流程傳輸至*embedded_statement*等同於狀態*v*結尾*resource_取得*。

#### <a name="lock-statements"></a>Lock 陳述式

針對`lock`陳述式*stmt*的表單：
```csharp
lock ( expr ) embedded_statement
```

*  明確指派狀態*v*開頭*expr*等同於狀態*v*開頭*stmt*。
*  明確指派狀態*v*在控制流程傳輸至*embedded_statement*等同於狀態*v*結尾*expr*.

#### <a name="yield-statements"></a>Yield 陳述式

針對`yield return`陳述式*stmt*的表單：
```csharp
yield return expr ;
```

*  明確指派狀態*v*開頭*expr*等同於狀態*v*開頭*stmt*。
*  明確指派狀態*v*結尾*stmt*的狀態相同*v*結尾*expr*。
*  A`yield break`陳述式已明確指派狀態不會影響。

#### <a name="general-rules-for-simple-expressions"></a>簡單運算式的一般規則

下列規則適用於這些種類的運算式： 常值 ([常值](expressions.md#literals))，簡單名稱 ([簡單名稱](expressions.md#simple-names))，成員存取運算式 ([成員存取](expressions.md#member-access))，非編製索引的基底存取運算式 ([基底存取](expressions.md#base-access))，`typeof`運算式 ([typeof 運算子](expressions.md#the-typeof-operator))，預設值運算式 ([預設值運算式](expressions.md#default-value-expressions)) 和`nameof`運算式 ([Nameof 運算式](expressions.md#nameof-expressions))。

*  明確指派狀態*v*結尾的這類運算式會明確指派狀態的相同*v*運算式的開頭。

#### <a name="general-rules-for-expressions-with-embedded-expressions"></a>一般規則運算式使用內嵌的運算式

下列規則適用於這些種類的運算式： 括號括住運算式 ([括號括住運算式](expressions.md#parenthesized-expressions))，元素存取運算式 ([項目存取](expressions.md#element-access))、 基底存取運算式編製索引 ([基底存取](expressions.md#base-access))、 遞增和遞減運算式 ([後置遞增和遞減運算子](expressions.md#postfix-increment-and-decrement-operators)，[前置遞增和遞減運算子](expressions.md#prefix-increment-and-decrement-operators))，轉換運算式 ([轉型運算式](expressions.md#cast-expressions))，一元`+`， `-`， `~`，`*`二進位運算式`+`， `-`， `*`，`/`, `%`, `<<`, `>>`, `<`, `<=`, `>`, `>=`, `==`, `!=`, `is`, `as`, `&`， `|`，`^`運算式 ([算術運算子](expressions.md#arithmetic-operators)，[移位運算子](expressions.md#shift-operators)，[關係和類型測試運算子](expressions.md#relational-and-type-testing-operators)[邏輯運算子](expressions.md#logical-operators))，複合指派運算式 ([複合指派](expressions.md#compound-assignment))，`checked`並`unchecked`運算式 ([checked 與 unchecked運算子](expressions.md#the-checked-and-unchecked-operators))，再加上陣列和委派建立運算式 ([new 運算子](expressions.md#the-new-operator))。

每個運算式有一或多個固定的順序會無條件地進行評估的子運算式。 例如，二進位檔`%`左手邊的運算子，則右手邊，運算子會評估。 編製索引的作業會評估索引的運算式，並接著會評估每個索引運算式中，從左到右的順序。 運算式*expr*，其中包含子運算式*e1、 e2，...，eN*、 依序評估：

*  明確指派狀態*v*開頭*e1*等同於在開頭的明確指派狀態*expr*。
*  明確指派狀態*v*開頭*ei* (*我*大於一) 等同於先前的子運算式的結尾的明確指派狀態。
*  明確指派狀態*v*結尾*expr*等同於在結尾處的明確指派狀態*eN*

#### <a name="invocation-expressions-and-object-creation-expressions"></a>引動過程運算式和物件建立運算式

引動過程運算式*expr*的表單：
```csharp
primary_expression ( arg1 , arg2 , ... , argN )
```
或表單的物件建立運算式：
```csharp
new type ( arg1 , arg2 , ... , argN )
```

*  引動過程運算式，明確指派狀態*v*之前*primary_expression*等同於狀態*v*之前*expr*.
*  引動過程運算式，明確指派狀態*v*之前*arg1*等同於狀態*v*之後*primary_expression*.
*  為物件建立運算式，明確指派狀態*v*之前*arg1*等同於狀態*v*之前*expr*。
*  每個引數*argi*，明確指派狀態*v*之後*argi*取決於一般的運算式規則，並忽略任何`ref`或`out`修飾詞。
*  每個引數*argi*任何*我*大於一，明確指派狀態*v*之前*argi*相同的狀態*v*之後先前*arg*。
*  如果變數*v*被當做`out`引數 (也就是表單的引數`out v`) 中的引數，則狀態的任何*v*之後*expr*已明確指派。 否則狀態*v*之後*expr*等同於狀態*v*之後*argN*。
*  陣列初始設定式 ([陣列建立運算式](expressions.md#array-creation-expressions))，物件初始設定式 ([物件初始設定式](expressions.md#object-initializers))，集合初始設定式 ([集合初始設定式](expressions.md#collection-initializers)) 和匿名物件初始設定式 ([匿名物件建立運算式](expressions.md#anonymous-object-creation-expressions))，明確指派狀態取決於這些建構所定義的擴充。

#### <a name="simple-assignment-expressions"></a>簡單指派運算式

運算式*expr*表單的`w = expr_rhs`:

*  明確指派狀態*v*之前*expr_rhs*明確指派狀態的相同*v*之前*expr*。
*  明確指派狀態*v*之後*expr*取決於：
   * 如果*w*是做為相同的變數*v*，然後明確指派的狀態*v*之後*expr*明確指派。
   * 否則，如果指派就會發生結構類型的執行個體建構函式內，如果*w*是指定的自動實作的屬性的屬性存取*P*所建構的執行個體上並*v*是隱藏的支援欄位*P*，然後明確指派的狀態*v*之後*expr*絕對指派。
   * 否則，明確指派狀態*v*之後*expr*明確指派狀態的相同*v*之後*expr_rhs*。

#### <a name="-conditional-and-expressions"></a>& & (條件式 AND) 運算式

運算式*expr*表單的`expr_first && expr_second`:

*  明確指派狀態*v*之前*expr_first*明確指派狀態的相同*v*之前*expr*。
*  明確指派狀態*v*之前*expr_second*如果已明確指派的狀態*v*之後*expr_first*是明確指派或者 「 已明確指派之後，則為 true 的運算式"。 否則，它未明確指派。
*  明確指派狀態*v*之後*expr*取決於：
    * 如果*expr_first*的值是常數運算式`false`，然後明確指派的狀態*v*之後*expr*明確指派相同狀態*v*之後*expr_first*。
    * 否則，如果狀態*v*之後*expr_first*已明確指派，然後狀態*v*之後*expr*明確指派。
    * 否則，如果狀態*v*之後*expr_second*已明確指派，和狀態*v*之後*expr_first* "絕對已指派，則為 false 的運算式之後 」，則狀態*v*之後*expr*明確指派。
    * 否則，如果狀態*v*之後*expr_second*明確指派或"，則為 true 的運算式之後明確指派 」，則狀態*v*之後*expr* "肯定之後被指派，則為 true 的運算式"。
    * 否則，如果狀態*v*之後*expr_first*是 」，則為 false 的運算式之後明確指派 」，和狀態*v*之後*expr_second* "，則為 false 的運算式之後明確指派 」，則狀態*v*之後*expr* 「 明確指派 false 的運算式之後 」。
    * 否則，狀態*v*之後*expr*未明確指派。

在範例
```csharp
class A
{
    static void F(int x, int y) {
        int i;
        if (x >= 0 && (i = y) >= 0) {
            // i definitely assigned
        }
        else {
            // i not definitely assigned
        }
        // i not definitely assigned
    }
}
```
變數`i`會被視為已明確指派其中一個內嵌的陳述式的`if`陳述式而不是在其他。 中`if`方法中的陳述式`F`，該變數`i`明確指派第一個內嵌陳述式因為運算式的執行`(i = y)`一律會在執行內嵌陳述式之前。 相較之下，變數`i`未明確指派在第二個內嵌的陳述式，因為`x >= 0`測試結果為 false，導致變數`i`未獲指派。

#### <a name="-conditional-or-expressions"></a>||(條件式 OR) 運算式

運算式*expr*表單的`expr_first || expr_second`:

*  明確指派狀態*v*之前*expr_first*明確指派狀態的相同*v*之前*expr*。
*  明確指派狀態*v*之前*expr_second*如果已明確指派的狀態*v*之後*expr_first*是明確指派或者 「 已明確指派 false 的運算式之後"。 否則，它未明確指派。
*  明確指派陳述式*v*之後*expr*取決於：
    * 如果*expr_first*的值是常數運算式`true`，然後明確指派的狀態*v*之後*expr*明確指派相同狀態*v*之後*expr_first*。
    * 否則，如果狀態*v*之後*expr_first*已明確指派，然後狀態*v*之後*expr*明確指派。
    * 否則，如果狀態*v*之後*expr_second*已明確指派，和狀態*v*之後*expr_first* "絕對已指派，則為 true 的運算式之後 」，則狀態*v*之後*expr*明確指派。
    * 否則，如果狀態*v*之後*expr_second*明確指派或 「 false 的運算式之後明確指派 」，則狀態*v*之後*expr* 「 明確指派 false 的運算式之後 」。
    * 否則，如果狀態*v*之後*expr_first*是 」，則為 true 的運算式之後明確指派 」，和狀態*v*之後*expr_second*"，則為 true 的運算式之後明確指派 」，則狀態*v*之後*expr* "肯定之後被指派，則為 true 的運算式"。
    * 否則，狀態*v*之後*expr*未明確指派。

在範例
```csharp
class A
{
    static void G(int x, int y) {
        int i;
        if (x >= 0 || (i = y) >= 0) {
            // i not definitely assigned
        }
        else {
            // i definitely assigned
        }
        // i not definitely assigned
    }
}
```
變數`i`會被視為已明確指派其中一個內嵌的陳述式的`if`陳述式而不是在其他。 在`if`方法中的陳述式`G`，變數`i`因為已明確指派第二個內嵌的陳述式中執行運算式`(i = y)`一律會在執行內嵌陳述式之前。 相較之下，變數`i`未明確指派第一個內嵌陳述式，因為`x >= 0`測試結果為 true，導致變數`i`未獲指派。

#### <a name="-logical-negation-expressions"></a>! （邏輯否定） 運算式

運算式*expr*表單的`! expr_operand`:

*  明確指派狀態*v*之前*expr_operand*明確指派狀態的相同*v*之前*expr*。
*  明確指派狀態*v*之後*expr*取決於：
    * 如果狀態*v*之後 * expr_operand * 已明確指派，然後狀態*v*之後*expr*明確指派。
    * 如果狀態*v*之後 * expr_operand * 未明確指派，然後狀態*v*之後*expr*未明確指派。
    * 如果狀態*v*之後 * expr_operand *"，則為 false 的運算式之後明確指派 」，則狀態*v*之後*expr* "肯定之後被指派 true運算式"。
    * 如果狀態*v*之後 * expr_operand *"，則為 true 的運算式之後明確指派 」，則狀態*v*之後*expr* "肯定之後被指派為 false運算式"。

#### <a name="-null-coalescing-expressions"></a>?? （null 聯合） 的運算式

運算式*expr*表單的`expr_first ?? expr_second`:

*  明確指派狀態*v*之前*expr_first*明確指派狀態的相同*v*之前*expr*。
*  明確指派狀態*v*之前*expr_second*明確指派狀態的相同*v*之後*expr_first*。
*  明確指派陳述式*v*之後*expr*取決於：
    * 如果*expr_first*是常數運算式 ([常數運算式](expressions.md#constant-expressions)) 值為 null，則狀態*v*之後*expr*相同隨著*v*之後*expr_second*。
*  否則，狀態*v*之後*expr*明確指派狀態的相同*v*之後*expr_first*。

#### <a name="-conditional-expressions"></a>？: （條件） 運算式

運算式*expr*表單的`expr_cond ? expr_true : expr_false`:

*  明確指派狀態*v*之前*expr_cond*等同於狀態*v*之前*expr*。
*  明確指派狀態*v*之前*expr_true*已明確指派，如果且只有下列其中一種保留：
    * *expr_cond*是常數運算式的值 `false`
    * 狀態*v*之後*expr_cond*是明確指派或 「 絕對，則為 true 的運算式之後指派 」。
*  明確指派狀態*v*之前*expr_false*已明確指派，如果且只有下列其中一種保留：
    * *expr_cond*是常數運算式的值 `true`
*  狀態*v*之後*expr_cond*是明確指派或 「 絕對，則為 false 的運算式之後指派 」。
*  明確指派狀態*v*之後*expr*取決於：
    * 如果*expr_cond*是常數運算式 ([常數運算式](expressions.md#constant-expressions)) 值`true`然後狀態*v*之後*expr*狀態相同*v*之後*expr_true*。
    * 否則，如果*expr_cond*是常數運算式 ([常數運算式](expressions.md#constant-expressions)) 值`false`然後狀態*v*之後*expr*的狀態相同*v*之後*expr_false*。
    * 否則，如果狀態*v*之後*expr_true*明確指派和狀態*v*之後*expr_false*絕對指派，則狀態*v*之後*expr*明確指派。
    * 否則，狀態*v*之後*expr*未明確指派。

#### <a name="anonymous-functions"></a>匿名函式

針對*lambda_expression*或是*anonymous_method_expression* *expr*內文 (任一*區塊*或*運算式*)*主體*:

*  外部變數的明確指派狀態*v*之前*主體*等同於狀態*v*之前*expr*。 也就是明確的指派狀態的外部變數被繼承自匿名函式的內容。
*  外部變數的明確指派狀態*v*之後*expr*等同於狀態*v*之前*expr*。

此範例
```csharp
delegate bool Filter(int i);

void F() {
    int max;

    // Error, max is not definitely assigned
    Filter f = (int n) => n < max;

    max = 5;
    DoWork(f);
}
```
會產生編譯時期錯誤，因為`max`未明確指派的匿名函式宣告的位置。 此範例
```csharp
delegate void D();

void F() {
    int n;
    D d = () => { n = 1; };

    d();

    // Error, n is not definitely assigned
    Console.WriteLine(n);
}
```
也會產生編譯時期錯誤，因為指派給`n`匿名函式中明確指派狀態就不會影響`n`匿名函式之外。

## <a name="variable-references"></a>變數參考

A *variable_reference*是*運算式*，分類為變數。 A *variable_reference*代表要擷取目前的值和儲存新的值，可以存取的儲存位置。

```antlr
variable_reference
    : expression
    ;
```

在 C 和 c + + *variable_reference*稱為*左值*。

## <a name="atomicity-of-variable-references"></a>不可部分完成性的變數參考

讀取和寫入的下列資料類型是不可部分完成： `bool`， `char`， `byte`， `sbyte`， `short`， `ushort`， `uint`， `int`， `float`，和參考型別。 此外，讀取和寫入與上述清單中的基礎類型的列舉型別也是不可部分完成。 讀取和寫入的其他類型，包括`long`， `ulong`， `double`，和`decimal`，以及使用者定義型別，不保證是不可部分完成。 除了針對該用途而設計的程式庫函式，則無法保證的不可部分完成讀取-修改-寫入，例如在遞增或遞減的情況下。

