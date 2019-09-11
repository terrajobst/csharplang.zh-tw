---
ms.openlocfilehash: a01cf9387b8dc47de036bf0bd1496c19a441d81c
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876805"
---
# <a name="variables"></a>變數

變數代表儲存位置。 每個變數都有一個類型，可決定可以在變數中儲存的值。 C#是型別安全的語言，而且C#編譯器保證儲存在變數中的值一定是適當的型別。 變數的值可以透過指派或透過使用`++`和`--`運算子來變更。

必須先***明確指派***變數（[明確指派](variables.md#definite-assignment)），才能取得其值。

如下列各節所述，一***開始指派***或一***開始未***指派變數。 一開始指派的變數具有妥善定義的初始值，而且一律會被視為明確指派的值。 一開始未指派的變數沒有初始值。 對於一開始未指派的變數，若要在特定位置被視為明確指派，對變數的指派必須發生在導致該位置的每個可能的執行路徑中。

## <a name="variable-categories"></a>變數類別目錄

C#定義變數的七個類別：靜態變數、執行個體變數、陣列元素、值參數、參考參數、輸出參數和本機變數。 下列各節會說明每個類別。

在範例中
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
`x`是一個靜態變數， `y`它是一個執行個體變數`v[0]` ，是一個陣列元素`a` ，是一個`c`值參數`b` ，是一個參考參數，是一個輸出參數， `i`而是一個本機變數.

### <a name="static-variables"></a>靜態變數

以`static`修飾詞宣告的欄位稱為***靜態變數***。 靜態變數在執行其包含類型的靜態函式（[靜態](classes.md#static-constructors)的函式）之前就會存在，而且當相關聯的應用程式域停止存在時，就會停止存在。

靜態變數的初始值是變數類型的預設值（[預設值](variables.md#default-values)）。

基於明確指派檢查的目的，會將靜態變數視為一開始指派。

### <a name="instance-variables"></a>執行個體變數

未使用`static`修飾詞宣告的欄位稱為「***執行個體變數***」。

#### <a name="instance-variables-in-classes"></a>類別中的執行個體變數

當建立該類別的新實例時，類別的執行個體變數就會存在，而當該實例沒有任何參考和實例的析構函式（如果有的話）已執行時，就會停止存在。

類別之執行個體變數的初始值是變數類型的預設值（[預設](variables.md#default-values)值）。

基於明確指派檢查的目的，會將類別的執行個體變數視為一開始指派。

#### <a name="instance-variables-in-structs"></a>結構中的執行個體變數

結構的執行個體變數與它所屬的結構變數具有完全相同的存留期。 換句話說，當結構型別的變數出現或不存在時，就會執行結構的執行個體變數。

結構之執行個體變數的初始指派狀態與包含 struct 變數相同。 換句話說，當結構變數被視為一開始被指派時，也就是它的執行個體變數，而當結構變數被視為一開始未指派時，它的執行個體變數就同樣不會被指派。

### <a name="array-elements"></a>陣列元素

當建立陣列實例時，陣列的元素會存在，而且當沒有該陣列實例的參考時，就會停止存在。

陣列中每個元素的初始值都是陣列元素類型的預設值（[預設](variables.md#default-values)值）。

基於明確指派檢查的目的，會將陣列元素視為一開始指派。

### <a name="value-parameters"></a>值參數

未使用`ref`或`out`修飾詞宣告的參數是***值參數***。

值參數在叫用函式成員（方法、實例參數化、存取子或運算子）或參數所屬的匿名函式時，會存在，且會使用調用中提供的引數值進行初始化。 值參數在函式成員或匿名函式傳回時，通常會停止存在。 不過，如果值參數是由匿名函式（匿名函式[運算式](expressions.md#anonymous-function-expressions)）所捕捉，則其存留時間至少會延伸到從該匿名函式建立的委派或運算式樹狀結構符合垃圾收集的資格。

基於明確指派檢查的目的，會將值參數視為一開始指派。

### <a name="reference-parameters"></a>傳址參數

使用`ref`修飾詞宣告的參數是***參考參數***。

參考參數不會建立新的儲存位置。 相反地，參考參數代表的儲存位置與在函式成員或匿名函式呼叫中指定為引數的變數相同。 因此，參考參數的值一律與基礎變數相同。

下列明確指派規則適用于參考參數。 請注意[輸出參數](variables.md#output-parameters)中所述輸出參數的不同規則。

*  變數必須是明確指派的（[明確指派](variables.md#definite-assignment)），才能在函式成員或委派調用中傳遞做為參考參數。
*  在函式成員或匿名函式內，會將參考參數視為一開始指派。

在結構類型的實例方法或實例存取子中，關鍵字`this`的行為與結構類型的參考參數完全相同（[此存取權](expressions.md#this-access)）。

### <a name="output-parameters"></a>輸出參數

使用`out`修飾詞宣告的參數是***output 參數***。

輸出參數不會建立新的儲存位置。 相反地，output 參數代表的儲存位置與指定為函式成員或委派調用中引數的變數相同。 因此，輸出參數的值一律與基礎變數相同。

下列明確指派規則適用于輸出參數。 請注意[參考參數](variables.md#reference-parameters)中所述參考參數的不同規則。

*  變數不需要明確指派，就可以在函式成員或委派調用中傳遞做為輸出參數。
*  在函式成員或委派調用的正常完成之後，每個當做輸出參數傳遞的變數都會視為該執行路徑中的指派。
*  在函數成員或匿名函式內，會將輸出參數視為一開始未指派。
*  函式成員或匿名函式的每個輸出參數都必須明確指派（[明確指派](variables.md#definite-assignment)），函式成員或匿名函式才會正常傳回。

在結構類型的實例函式中，關鍵字`this`的行為與結構類型的輸出參數完全相同（[此存取權](expressions.md#this-access)）。

### <a name="local-variables"></a>區域變數

***本機變數***是由*local_variable_declaration*所宣告，這可能會發生在*區塊*、 *for_statement*、 *switch_statement*或*using_statement*中。或是由*try_statement*的*foreach_statement*或*specific_catch_clause* 。

本機變數的存留期是程式執行的部分，在這段期間，一定會保留儲存區。 此存留期至少會從 entry 延伸到*區塊*、 *for_statement*、 *switch_statement*、 *using_statement*、 *foreach_statement*或與其關聯的*specific_catch_clause* ，直到該*區塊*、 *for_statement*、 *switch_statement*、 *using_statement*、 *foreach_statement*或*specific_catch_clause*的執行會以任何方式結束。 （輸入封閉的*區塊*或呼叫方法會暫停，但不會結束、執行目前的*區塊*、 *for_statement*、 *switch_statement*、 *using_statement*、 *foreach_statement*或*specific_catch_clause*）。如果本機變數是由匿名函式（已[捕捉的外部變數](expressions.md#captured-outer-variables)）所捕捉，其存留期至少會擴充到從匿名函式建立的委派或運算式樹狀架構，以及任何其他參考已捕捉的變數符合垃圾收集的資格。

如果父*區塊*、 *for_statement*、 *switch_statement*、 *using_statement*、 *foreach_statement*或*specific_catch_clause*以遞迴方式輸入，則會建立每個本機變數的新實例。時間和其*local_variable_initializer*（如果有的話）會每次評估。

*Local_variable_declaration*引進的區域變數不會自動初始化，因此沒有預設值。 基於明確指派檢查的目的， *local_variable_declaration*引進的本機變數會被視為一開始未指派。 *Local_variable_declaration*可能包含*local_variable_initializer*，在此情況下，只會將變數視為明確指派給初始化運算式（宣告[語句](variables.md#declaration-statements)）。

在*local_variable_declaration*所引進的本機變數範圍內，在其*local_variable_declarator*之前的文字位置中參考該區域變數是編譯時期錯誤。 如果區域變數宣告是隱含的（[區域變數](statements.md#local-variable-declarations)宣告），則在其*local_variable_declarator*中參考該變數也會是錯誤。

*Foreach_statement*或*specific_catch_clause*所引進的本機變數會被視為在其整個範圍中明確指派。

本機變數的實際存留期與執行相依。 例如，編譯器可能會以靜態方式判斷區塊中的區域變數僅用於該區塊的一小部分。 使用這項分析，編譯器可能會產生程式碼，使變數的存放區具有比其包含區塊更短的存留期。

本機參考變數所參考的儲存體會在該區域參考變數（[自動記憶體管理](basic-concepts.md#automatic-memory-management)）的存留期以外獨立回收。

## <a name="default-values"></a>預設值

下列類別的變數會自動初始化為其預設值：

*  靜態變數。
*  類別實例的執行個體變數。
*  陣列元素。

變數的預設值取決於變數的類型，並依照下列方式決定：

*  對於*value_type*的變數，預設值與*value_type*的預設函式（[預設](types.md#default-constructors)的函式）所計算的值相同。
*  若為*reference_type*的變數，預設值為`null`。

預設值的初始化通常是藉由讓記憶體管理員或垃圾收集行程將記憶體初始化為全位-零，再配置給使用。 基於這個理由，使用全位-零來代表 null 參考是很方便的。

## <a name="definite-assignment"></a>明確指派

在函式成員可執行程式碼的指定位置上，如果編譯器可以透過特定的靜態流程分析（[判斷明確指派的精確規則](variables.md#precise-rules-for-determining-definite-assignment)）來證明，變數就會被視為***明確指派***。已自動初始化，或已是至少一個指派的目標。 非正式規定的明確指派規則包括：

*  最初指派的變數（[初始指派的變數](variables.md#initially-assigned-variables)）一律會被視為明確指派。
*  如果所有導致該位置的可能執行路徑至少包含下列其中一項，則最初解除指派的變數（一[開始未指派的變數](variables.md#initially-unassigned-variables)）會被視為在指定的位置上明確指派：
    * 簡單指派（[簡單指派](expressions.md#simple-assignment)），其中變數為左運算元。
    * 傳遞變數做為輸出參數的調用運算式（[調用運算式](expressions.md#invocation-expressions)）或物件建立運算式（[物件建立運算式](expressions.md#object-creation-expressions)）。
    * 對於區域變數，這是包含變數初始化運算式的本機變數宣告（[區域變數](statements.md#local-variable-declarations)宣告）。

在[最初指派的變數](variables.md#initially-assigned-variables)、一[開始未指派的變數](variables.md#initially-unassigned-variables)，以及[判斷明確指派的精確規則](variables.md#precise-rules-for-determining-definite-assignment)中，會描述上述非正式規則的基礎正式規格。

系統會個別追蹤*struct_type*變數之執行個體變數的明確指派狀態。 除了上述規則，下列規則適用于*struct_type*變數及其執行個體變數：

*  如果將執行個體變數的包含*struct_type*變數視為明確指派，則會將它視為明確指派。
*  如果將每個執行個體變數視為明確指派，則會將*struct_type*變數視為明確指派。

明確指派是下列內容中的需求：

*  變數必須在取得其值的每個位置明確指派。 這可確保永遠不會發生未定義的值。 運算式中出現的變數會被視為取得變數的值，但不包括
    * 變數是簡單指派的左運算元，
    * 變數會當做輸出參數傳遞，或
    * 變數是*struct_type*變數，並會當做成員存取的左運算元。
*  變數必須在做為參考參數傳遞的每個位置上明確指派。 這可確保所叫用的函式成員可以考慮最初指派的參考參數。
*  函式成員的所有輸出參數都必須在函式成員傳回的每個位置（透過`return`語句或透過執行到達函式成員主體的結尾）明確指派。 這可確保函式成員不會在輸出參數中傳回未定義的值，因此可讓編譯器考慮使用變數做為輸出參數（相當於對變數的指派）的函數成員調用。
*  Struct_type 實例的函式的變數必須在該實例的函式傳回的每個位置明確指派。`this`

### <a name="initially-assigned-variables"></a>初始指派的變數

下列類別的變數分類為一開始指派：

*  靜態變數。
*  類別實例的執行個體變數。
*  最初指派之結構變數的執行個體變數。
*  陣列元素。
*  值參數。
*  參考參數。
*  在`catch` 子句`foreach`或語句中宣告的變數。

### <a name="initially-unassigned-variables"></a>一開始未指派的變數

下列類別的變數分類為一開始未指派：

*  一開始未指派之結構變數的執行個體變數。
*  輸出參數，包括`this`結構實例函式的變數。
*  區域變數，但`catch`子句`foreach`或語句中所宣告的變數除外。

### <a name="precise-rules-for-determining-definite-assignment"></a>判斷明確指派的精確規則

為了判斷每個使用的變數都是明確指派的，編譯器必須使用與本節所述相同的進程。

編譯器會處理具有一或多個初始未指派變數的每個函式成員的主體。 針對每個最初未指派的變數*v*，編譯器會在函式成員中的下列每個點上，判斷*v*的***明確指派狀態***：

*  在每個語句的開頭
*  每個語句的結尾點（[結束點和](statements.md#end-points-and-reachability)可連線）
*  在將控制項傳輸至另一個語句或語句結束點的每個 arc 上
*  在每個運算式的開頭
*  在每個運算式的結尾處

*V*的明確指派狀態可以是：

*  明確指派。 這表示在此點的所有可能控制流程上，已將值指派給*v* 。
*  未明確指派。 針對類型`bool`之運算式結尾的變數狀態，未明確指派之變數的狀態可能會屬於下列子狀態之一（但不一定）：
    * 在 true 運算式之後明確指派。 此狀態表示如果布林運算式評估為 true，則會明確指派*v* ，但如果布林運算式評估為 false，則不一定會指派。
    * 在 false 運算式之後明確指派。 此狀態表示如果布林運算式評估為 false，則會明確指派*v* ，但如果布林運算式評估為 true，則不一定會指派。

下列規則控制變數*v*的狀態在每個位置的決定方式。

#### <a name="general-rules-for-statements"></a>語句的一般規則

*  *v*不會在函式成員主體的開頭明確指派。
*  *v*會在任何無法連線的語句開頭明確指派。
*  在任何其他語句開頭的*v*明確指派狀態是藉由檢查所有目標為該語句開頭之控制流程傳輸上*v*的明確指派狀態來決定。 如果（而且只有在） *v*是在所有此類控制流程傳輸上明確指派，則會在語句的開頭明確指派*v* 。 一組可能的控制流程傳輸的判斷方式，與檢查語句可連線性（[端點和](statements.md#end-points-and-reachability)可連線性）相同。
*  *V*在`checked`區塊、 、、`lock`、 `if` 、、、、`do`、或的結束點的明確指派狀態。 `while` `unchecked` `for` `foreach` `using` `switch`語句的決定方式是在所有以該語句的結束點為目標的控制流程傳輸上，檢查*v*的明確指派狀態。 如果在所有這類的控制流程傳輸上都已明確指派*v* ，則會在語句的結束點明確指派*v* 。 別的*v*不會在語句的結束點明確指派。 一組可能的控制流程傳輸的判斷方式，與檢查語句可連線性（[端點和](statements.md#end-points-and-reachability)可連線性）相同。

#### <a name="block-statements-checked-and-unchecked-statements"></a>Block 語句、checked 和 unchecked 語句

在控制項上， *v*的明確指派狀態會傳送到區塊中語句清單的第一個語句（如果語句清單是空的，則為該區塊的結束點），這與*區塊前面的*明確指派語句相同。、 `checked`或`unchecked`語句。

#### <a name="expression-statements"></a>運算式陳述式

針對運算式語句包含運算式*expr*的*stmt* ：

*  *v*在*expr*開頭具有相同的明確指派狀態，如同在*stmt*開頭。
*  如果*v*是在*expr*結尾明確指派，則會在*stmt*的結束點明確指派它;別的它不會在*stmt*的結束點明確指派。

#### <a name="declaration-statements"></a>宣告陳述式

*  如果*stmt*不是沒有初始化運算式的宣告語句，則*v* *在 stmt 的*結束點具有相同的明確指派狀態，如同在*stmt*的開頭。
*  如果*stmt*是具有初始化運算式的宣告語句，則*v*的明確指派狀態會決定為語句清單，每個具有初始化運算式的*宣告都有*一個指派語句（順序為宣告）。

#### <a name="if-statements"></a>If 語句

針對格式為的語句`if` stmt：
```csharp
if ( expr ) then_stmt else else_stmt
```

*  *v*在*expr*開頭具有相同的明確指派狀態，如同在*stmt*開頭。
*  如果在*expr*的結尾明確指派*v* ，則在控制流程傳輸至*then_stmt* ，以及如果沒有 else*子句時，* 會將它明確指派給*else_stmt*或指標的端點。
*  如果*v*在*expr*結尾處的「true expression 之後明確指派」狀態，則會在控制流程傳輸至*then_stmt*時明確指派，而不會在控制流程傳輸上明確指派給任一*else_stmt* ，或如果沒有 else 子句，則為*stmt*的結束點。
*  如果*v*在*expr*結尾處具有「在 false 運算式之後明確指派」狀態，則會在控制流程傳送至*else_stmt*時明確指派，而不會在控制流程傳輸至 then_stmt 時明確指派。 . 只有在*then_stmt*的端點明確指派時，才會在*stmt*的端點上明確指派。
*  否則 *，如果沒有*else 子句，則會將*v*視為不會在控制流程傳輸到*then_stmt*或*else_stmt*時明確指派。

#### <a name="switch-statements"></a>Switch 語句

在具有控制運算式*expr*的語句stmt`switch`中：

*  *V*在*expr*開頭的明確指派狀態，與*stmt*開頭的*v*狀態相同。
*  在控制流程上的*v*明確指派狀態傳送至可連線的 switch block 語句清單，與*expr*結尾的*v*明確指派狀態相同。

#### <a name="while-statements"></a>While 語句

針對格式為的語句`while` stmt：
```csharp
while ( expr ) while_body
```

*  *v*在*expr*開頭具有相同的明確指派狀態，如同在*stmt*開頭。
*  如果在*expr*結尾明確指派*v* ，則會在控制流程傳輸上明確指派給*while_body*和*stmt*的結束點。
*  如果*v*在*expr*結尾處的「true expression 之後明確指派」狀態，則會在控制流程傳送至*while_body*時明確指派，但不會在*stmt*的端點上明確指派。
*  如果*v*在*expr*結尾處具有「在 false 運算式之後明確指派」狀態，則會在控制流程傳輸到*stmt*的結束點時明確指派，但不會在控制流程傳輸上明確指派 *_body*。

#### <a name="do-statements"></a>Do 語句

針對格式為的語句`do` stmt：
```csharp
do do_body while ( expr ) ;
```

*  *v*在控制流程上具有相同的明確指派狀態，從*stmt*開頭到*do_body* ，如同在*stmt*的開頭。
*  *v*在*expr*的開頭具有相同的明確指派狀態，如同*do_body*的結束點。
*  如果在*expr*的結尾明確指派*v* ，則會在控制流程傳輸上，將它明確指派給*stmt*的結束點。
*  如果*v*在*expr*結尾處具有「在 false 運算式之後明確指派」狀態，則會在控制流程傳輸上，將它明確指派給*stmt*的結束點。

#### <a name="for-statements"></a>適用于語句

`for`語句的明確指派檢查，其格式如下：
```csharp
for ( for_initializer ; for_condition ; for_iterator ) embedded_statement
```
執行方式就如同撰寫語句一樣：
```csharp
{
    for_initializer ;
    while ( for_condition ) {
        embedded_statement ;
        for_iterator ;
    }
}
```

如果`for`語句中省略了*for_condition* ，則評估明確的指派會繼續進行，如同上述擴充`true`中的 for_condition 已被取代。

#### <a name="break-continue-and-goto-statements"></a>Break、continue 和 goto 語句

`break`在、或`continue`  語句`goto`所造成的控制流程傳輸上，v 的明確指派狀態，與語句開頭的*v*明確指派狀態相同。

#### <a name="throw-statements"></a>Throw 語句

適用于表單的語句*stmt*
```csharp
throw expr ;
```

*V*在*expr*開頭的明確指派狀態，與*stmt*開頭的*v*明確指派狀態相同。

#### <a name="return-statements"></a>Return 語句

適用于表單的語句*stmt*
```csharp
return expr ;
```

*  *V*在*expr*開頭的明確指派狀態，與*stmt*開頭的*v*明確指派狀態相同。
*  如果*v*是輸出參數，則必須明確地指派其中一個：
    * after *expr*之後
    * 或，位於括`finally` 住`try`語句- `try`之或`finally` 的區塊-結尾。 `catch` - `finally` `return`

針對格式為的語句 stmt：
```csharp
return ;
```

*  如果*v*是輸出參數，則必須明確地指派其中一個：
    * 在*stmt*之前
    * 或，位於括`finally` 住`try`語句- `try`之或`finally` 的區塊-結尾。 `catch` - `finally` `return`

#### <a name="try-catch-statements"></a>Try-catch 語句

針對格式為的語句*stmt* ：
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
```

*  *V*在*try_block*開頭的明確指派狀態，與*stmt*開頭的*v*明確指派狀態相同。
*  *V*在*catch_block_i*開頭的明確指派狀態（適用于任何*i*）與*stmt*開頭的*v*明確指派狀態相同。
*  如果在*try_block*的端點和每個*catch_block_i* （針對每個*i*從1到 n）明確指派*v* ，則會明確指派*v*在*stmt*結束點的明確指派狀態。 ).

#### <a name="try-finally-statements"></a>Try-finally 語句

針對格式為的語句`try` stmt：
```csharp
try try_block finally finally_block
```

*  *V*在*try_block*開頭的明確指派狀態，與*stmt*開頭的*v*明確指派狀態相同。
*  *V*在*finally_block*開頭的明確指派狀態，與*stmt*開頭的*v*明確指派狀態相同。
*  只有在下列其中一項條件成立時，才會明確指派*v*在*stmt*結束點的明確指派狀態：
    * *v*在*try_block*的端點明確指派
    * *v*在*finally_block*的端點明確指派

如果在 try_block 內開始進行控制流程傳輸（ `goto`例如，語句），並在 *try_block*之外結束，則如果*v*為，則也會將*v*視為明確指派給該控制流程傳輸明確指派于*finally_block*的端點。 （這不是唯一的情況，如果基於此控制流程轉移的另一個原因明確指派*v* ，則仍會被視為明確指派）。

#### <a name="try-catch-finally-statements"></a>Try-catch-finally 語句

適用`try` 于窗`catch`體之語句的`finally`明確指派分析： - -
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
finally *finally_block*
```
其執行方式就如同`try`語句是`finally` -包含`try` -語句的語句：`catch`
```csharp
try {
    try try_block
    catch(...) catch_block_1
    ...
    catch(...) catch_block_n
}
finally finally_block
```

下列範例示範`try`語句的不同區塊（[try 語句](statements.md#the-try-statement)）如何影響明確指派。
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

#### <a name="foreach-statements"></a>Foreach 語句

針對格式為的語句`foreach` stmt：
```csharp
foreach ( type identifier in expr ) embedded_statement
```

*  *V*在*expr*開頭的明確指派狀態，與*stmt*開頭的*v*狀態相同。
*  *V*在控制流程傳輸到*embedded_statement*或*stmt*結束點的明確指派狀態，與*expr*結尾的*v*狀態相同。

#### <a name="using-statements"></a>Using 語句

針對格式為的語句`using` stmt：
```csharp
using ( resource_acquisition ) embedded_statement
```

*  *V*在*resource_acquisition*開頭的明確指派狀態，與*stmt*開頭的*v*狀態相同。
*  在控制流程傳輸至*embedded_statement*上， *v*的明確指派狀態與*resource_acquisition*結尾的*v*狀態相同。

#### <a name="lock-statements"></a>Lock 語句

針對格式為的語句`lock` stmt：
```csharp
lock ( expr ) embedded_statement
```

*  *V*在*expr*開頭的明確指派狀態，與*stmt*開頭的*v*狀態相同。
*  在控制流程傳輸至*embedded_statement*上， *v*的明確指派狀態與*expr*結尾的*v*狀態相同。

#### <a name="yield-statements"></a>Yield 語句

針對格式為的語句`yield return` stmt：
```csharp
yield return expr ;
```

*  *V*在*expr*開頭的明確指派狀態，與*stmt*開頭的*v*狀態相同。
*  *V*在*stmt*結尾的明確指派狀態，與*expr*結尾的*v*狀態相同。
*  `yield break`語句對明確的指派狀態不會有任何影響。

#### <a name="general-rules-for-simple-expressions"></a>簡單運算式的一般規則

下列規則適用于這類運算式：常值（[常](expressions.md#literals)值）、簡單名稱（[簡單名稱](expressions.md#simple-names)）、成員存取運算式（[成員存取](expressions.md#member-access)）、未編制索引的基底存取運算式（[基底存取](expressions.md#base-access)）、 `typeof`運算式（[typeof 運算子](expressions.md#the-typeof-operator)）、預設值運算式（[預設值運算式](expressions.md#default-value-expressions)）和`nameof`運算式（[Nameof 運算式](expressions.md#nameof-expressions)）。

*  在這類運算式結尾的*v*明確指派狀態，與運算式開頭的*v*明確指派狀態相同。

#### <a name="general-rules-for-expressions-with-embedded-expressions"></a>具有內嵌運算式之運算式的一般規則

下列規則適用于這類運算式：括號運算式（[括號運算式](expressions.md#parenthesized-expressions)）、元素存取運算式（專案[存取](expressions.md#element-access)）、具有索引編制的基底存取運算式（[基底存取](expressions.md#base-access)）、遞增和遞減運算式（後置[遞增和遞減運算子](expressions.md#postfix-increment-and-decrement-operators)、[前置遞增和遞減運算子](expressions.md#prefix-increment-and-decrement-operators)）、cast 運算式（[cast 運算式](expressions.md#cast-expressions)）、 `+`一元`-`、 `~`、 `*`、運算式，binary `+` `-` ，`*`， ，，`/`，，，，，，， ，，，，`>=`，，，， `%` `<<` `>>` `<` `<=` `>``==`、 、、`is` 、`^` 、、運算式（[算術運算子](expressions.md#arithmetic-operators)、[移位運算子](expressions.md#shift-operators)、關聯式和類型測試） `|` `as` `!=` `&` [運算子](expressions.md#relational-and-type-testing-operators)、[邏輯運算子](expressions.md#logical-operators)）、複合指派運算式（[複合](expressions.md#compound-assignment) `checked`指派）和`unchecked`運算式（[checked 和 unchecked 運算子](expressions.md#the-checked-and-unchecked-operators)），加上陣列和委派建立運算式（[新的運算子](expressions.md#the-new-operator)）。

這些運算式中的每一個都有一或多個以固定順序無條件評估的子運算式。 例如，二元`%`運算子會評估運算子的左邊，然後是右手邊。 索引作業會評估索引運算式，然後依序由左至右來評估每個索引運算式。 針對具有子運算式*e1、e2、...、eN*的運算式*expr*，會依照該順序評估：

*  在*e1*開頭的*v*明確指派狀態，與*expr*開頭的明確指派狀態相同。
*  *V*在*ei*開頭的明確指派狀態（*i*大於1）與前一個子運算式結尾的明確指派狀態相同。
*  *V*在*expr*結尾的明確指派狀態，與*eN*結尾的明確指派狀態相同

#### <a name="invocation-expressions-and-object-creation-expressions"></a>調用運算式和物件建立運算式

針對格式為的調用運算式*expr* ：
```csharp
primary_expression ( arg1 , arg2 , ... , argN )
```
或表單的物件建立運算式：
```csharp
new type ( arg1 , arg2 , ... , argN )
```

*  針對叫用運算式， *v*之前的*primary_expression*明確指派狀態與*expr**之前的*狀態相同。
*  針對叫用運算式， *v*的明確指派狀態在*arg1*之前，會與*primary_expression*之後的*v*狀態相同。
*  若為物件建立運算式，*則在* *arg1*前面的明確指派狀態會與*expr*前面的*v*狀態相同。
*  針對每個引數*argi*，在*argi*之後， *v*的明確指派狀態是由一般運算式規則所決定`ref` ， `out`忽略任何或修飾詞。
*  針對任何*i*大於1的每個引數*argi* ， *v*在*argi*之前的明確指派狀態與上一個*arg*之後的*v*狀態相同。
*  如果變數*v*在任何引數中`out`當做引數傳遞（也就是表單`out v`的引數），則會明確指派*v*在*expr*之後的狀態。 別的*v* after *expr*的狀態與 *...argn*之後的*v*狀態相同。
*  陣列初始化運算式（[陣列建立運算式](expressions.md#array-creation-expressions)）、物件初始化運算式（[物件初始化](expressions.md#object-initializers)運算式）、集合初始化運算式（[集合初始化](expressions.md#collection-initializers)運算式）和匿名物件初始化運算式（[匿名物件建立）運算式](expressions.md#anonymous-object-creation-expressions)），明確的指派狀態是由根據來定義這些結構的擴充所決定。

#### <a name="simple-assignment-expressions"></a>簡單指派運算式

針對格式`w = expr_rhs`為的運算式*expr* ：

*  *V*在*expr_rhs*之前的明確指派狀態，與*expr**前面的*明確指派狀態相同。
*  *V* after *expr*之後的明確指派狀態取決於：
   * 如果*w*與*v*是相同的變數，則會明確指派*v* after *expr*之後的明確指派狀態。
   * 否則，如果指派發生在結構型別的實例函式中，則如果*w*是屬性存取，在所建立的實例上指定自動實作為屬性*P* ，而*v*是的隱藏支援欄位*P*，則會明確指派*v* after *expr*之後的明確指派狀態。
   * 否則， *v* after *expr*之後的明確指派狀態，會與*expr_rhs*之後之*v*的明確指派狀態相同。

#### <a name="-conditional-and-expressions"></a>& & （條件式和）運算式

針對格式`expr_first && expr_second`為的運算式*expr* ：

*  *V*在*expr_first*之前的明確指派狀態，與*expr**前面的*明確指派狀態相同。
*  如果在*expr_first*之後的*v*狀態已明確指派，或「在 true 運算式之後明確指派」，則*expr_second* *的明確*指派狀態會明確指派。 否則，它不會被明確指派。
*  *V* after *expr*之後的明確指派狀態取決於：
    * 如果*expr_first*是具有值`false`的常數運算式，則*v* after *expr*的明確指派狀態會與*expr_first*之後的*v*明確指派狀態相同。
    * 否則，如果已明確指派*expr_first*後*v*的狀態，則會將*v*的狀態明確指派給*expr* 。
    * 否則，如果已明確指派*expr_second*後*v*的狀態，且*expr_first*之後的*v*狀態是「在 false 運算式之後明確指派」，則*v*的狀態一定會是*expr*指派.
    * 否則，如果*expr_second*後的*v*狀態是明確指派的，或「在 true 運算式之後明確指派」，則在*expr*之後的*v*狀態會是「在 true 運算式之後明確指派」。
    * 否則，如果*expr_first*之後的*v*狀態是「在 false 運算式之後明確指派」，而且*expr_second*之後的*v*狀態是「在 false 運算式之後明確指派」，則*v*的狀態會在之後*expr*是「在 false 運算式之後明確指派」。
    * 否則，在*expr*之後的*v*狀態不會明確指派。

在範例中
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
系統會`i`將變數視為明確指派給`if`語句的其中一個內嵌語句，而不是另一個。 在方法`if` `F`的語句中，變數`i`是在第一個內嵌語句中明確指派的，因為運算式`(i = y)`的執行一律會在此內嵌語句的執行前面。 相反地，變數`i`不會在第二個內嵌語句中明確指派，因為`x >= 0`可能已測試 false，導致變數`i`被取消指派。

#### <a name="-conditional-or-expressions"></a>||（條件式或）運算式

針對格式`expr_first || expr_second`為的運算式*expr* ：

*  *V*在*expr_first*之前的明確指派狀態，與*expr**前面的*明確指派狀態相同。
*  如果在*expr_first*之後的*v*狀態已明確指派，或「在 false 運算式之後明確指派」，則在*expr_second*之前的明確指派狀態是明確*指派的。* 否則，它不會被明確指派。
*  *V* after*運算式*的明確指派語句取決於：
    * 如果*expr_first*是具有值`true`的常數運算式，則*v* after *expr*的明確指派狀態會與*expr_first*之後的*v*明確指派狀態相同。
    * 否則，如果已明確指派*expr_first*後*v*的狀態，則會將*v*的狀態明確指派給*expr* 。
    * 否則，如果已明確指派*expr_second*後*v*的狀態，且*expr_first*之後的*v*狀態是「在 true 運算式之後明確指派」，則*v*的狀態一定會是*expr*指派.
    * 否則，如果已明確指派*expr_second*之後的*v*狀態，或「在 false 運算式之後明確指派」，則*v* after *expr*後面的狀態會是「在 false 運算式之後明確指派」。
    * 否則，如果*expr_first*後*v*的狀態是「在 true 運算式之後明確指派」，而且*expr_second*之後的*v*狀態是「在 true 運算式之後明確指派」，則*v*的狀態會在*expr 之後*是「在 true 運算式之後明確指派」。
    * 否則，在*expr*之後的*v*狀態不會明確指派。

在範例中
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
系統會`i`將變數視為明確指派給`if`語句的其中一個內嵌語句，而不是另一個。 在方法`if` `G`的語句中，變數`i`是在第二個內嵌的語句中明確指派的，因為`(i = y)`運算式的執行一律優先于此內嵌語句的執行。 相反地，變數`i`不會在第一個內嵌語句中明確指派，因為`x >= 0`可能已測試過 true，導致變數`i`被取消指派。

#### <a name="-logical-negation-expressions"></a>! （邏輯否定）運算式

針對格式`! expr_operand`為的運算式*expr* ：

*  *V*在*expr_operand*之前的明確指派狀態，與*expr**前面的*明確指派狀態相同。
*  *V* after *expr*之後的明確指派狀態取決於：
    * 如果 * expr_operand * 之後的*v*狀態是明確指派的，則會將*v*的狀態明確指派給*expr* after。
    * 如果 * expr_operand * 之後的*v*狀態未明確指派，則不會明確指派*v* after *expr*的狀態。
    * 如果 * expr_operand * 之後的*v*狀態是「在 false 運算式之後明確指派」，則*v* after *expr*後面的狀態會是「在 true 運算式之後明確指派」。
    * 如果 * expr_operand * 後面的*v*狀態是「在 true 運算式之後明確指派」，則*v* after *expr*後面的狀態會是「在 false 運算式之後明確指派」。

#### <a name="-null-coalescing-expressions"></a>?? （null 聯合）運算式

針對格式`expr_first ?? expr_second`為的運算式*expr* ：

*  *V*在*expr_first*之前的明確指派狀態，與*expr**前面的*明確指派狀態相同。
*  *V*在*expr_second*之前的明確指派狀態，與*expr_first*之後的*v*明確指派狀態相同。
*  *V* after*運算式*的明確指派語句取決於：
    * 如果*expr_first*是具有值 null 的常數運算式（[常數運算式](expressions.md#constant-expressions)），則*v* after *expr*的狀態會與*expr_second*之後的*v*狀態相同。
*  否則， *v* after *expr*的狀態會與*expr_first*之後之*v*的明確指派狀態相同。

#### <a name="-conditional-expressions"></a>？：（條件式）運算式

針對格式`expr_cond ? expr_true : expr_false`為的運算式*expr* ：

*  *V*在*expr_cond*之前的明確指派狀態，與*expr*之前的*v*狀態相同。
*  只有在下列其中一項保留時，才會明確指派*expr_true* *的明確*指派狀態：
    * *expr_cond*是具有值的常數運算式`false`
    * *expr_cond*之後的*v*狀態是明確指派的，或「在 true 運算式之後明確指派」。
*  只有在下列其中一項保留時，才會明確指派*expr_false* *的明確*指派狀態：
    * *expr_cond*是具有值的常數運算式`true`
*  *expr_cond*之後的*v*狀態是明確指派的，或「在 false 運算式之後明確指派」。
*  *V* after *expr*之後的明確指派狀態取決於：
    * 如果*expr_cond*是具有值`true`的常數運算式（[常數運算式](expressions.md#constant-expressions)），則*v* after *expr*的狀態會與*expr_true*之後的*v*狀態相同。
    * 否則，如果*expr_cond*是具有值`false`的常數運算式（[常數運算式](expressions.md#constant-expressions)），則*v* after *expr*的狀態會與*expr_false*之後的*v*狀態相同。
    * 否則，如果已明確指派*expr_true*後*v*的狀態，且明確指派*expr_false* *之後的 v 狀態*，則會明確指派*v* after *expr*的狀態。
    * 否則，在*expr*之後的*v*狀態不會明確指派。

#### <a name="anonymous-functions"></a>匿名函式

針對具有主體（*區塊*或*運算式*）*主體*的*lambda_expression*或*anonymous_method_expression* *expr* ：

*  外部變數*v*在*主體*之前的明確指派狀態，與*expr*之前的*v*狀態相同。 也就是說，外部變數的明確指派狀態是繼承自匿名函式的內容。
*  外部變數*v*在*expr*之後的明確指派狀態，與*expr*之前的*v*狀態相同。

範例
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
會產生編譯時期錯誤，因為`max`在宣告匿名函式的情況下，並未明確指派。 範例
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
也會產生編譯時期錯誤，因為在匿名函`n`式中的指派不會影響匿名函式`n`外部的明確指派狀態。

## <a name="variable-references"></a>變數參考

*Variable_reference*是分類為變數的*運算式*。 *Variable_reference*代表可以存取的儲存位置，以提取目前的值並儲存新的值。

```antlr
variable_reference
    : expression
    ;
```

在 C 和C++中， *variable_reference*稱為*左*值。

## <a name="atomicity-of-variable-references"></a>變數參考的不可部分完成性

下列資料類型的讀取和寫入是不可部分完成`bool`的`char`： `byte`、 `sbyte` `uint`、 `short`、 `ushort`、、 `int` `float`、、、和參考型別。 此外，在先前清單中具有基礎類型的列舉類型的讀取和寫入也是不可部分完成的。 其他類型的讀取和寫入，包括`long`、 `ulong`、 `double`和`decimal`，以及使用者定義類型，則不保證是不可部分完成的。 除了針對該目的而設計的程式庫函數之外，並不保證不可部分完成的讀取-修改-寫入，例如，在遞增或遞減的情況下。

