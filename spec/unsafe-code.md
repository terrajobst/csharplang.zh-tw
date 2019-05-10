---
ms.openlocfilehash: 90001cf3d48f216787fc65e59166ec57c5d0ca34
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488806"
---
# <a name="unsafe-code"></a>Unsafe 程式碼

核心C#的語言，定義在前述章節中，與不同值得注意的是 C 和C++中的指標其省略，做為資料類型。 相反地，C# 提供參考，並且能夠建立由記憶體回收行程所管理的物件。 這項設計，搭配其他功能，讓C#更安全語言比 C 或C++。 在 core C# 語言中不只可以有未初始化的變數、 「 懸空 」 的指標或索引的陣列，其界限外的運算式。 錯誤種類頭痛的 C 和C++因此會刪除程式。

幾乎每個指標類型時建構在 C 或C++中有參考型別對應C#，然而，有很多情況下，指標類型的存取權是不可或缺。 例如，與基礎作業系統互動、 存取記憶體對應的裝置，或實作關鍵時間的演算法不可能可行或切合實際，而不需要存取指標。 若要解決這項需求，C# 讓您能夠撰寫***unsafe 程式碼***。

Unsafe 程式碼中就可以宣告並對指標執行指標與整數類資料類型，來取得變數的位址之間的轉換等等。 就某方面來說，撰寫不安全的程式碼很像是撰寫 C# 程式中的 C 程式碼一樣。

Unsafe 程式碼實際上是 「 安全 」 的功能從開發人員和使用者的觀點來看。 Unsafe 程式碼必須明確標示以修飾詞`unsafe`，因此開發人員不可能會不小心，不安全的功能使用，並確保無法在不受信任的環境中執行不安全的程式碼如何執行引擎。

## <a name="unsafe-contexts"></a>不安全的內容

C# 的不安全的功能是僅適用於不安全的內容。 不安全的內容以包含引入`unsafe`修飾詞宣告的型別或成員，或採用*unsafe_statement*:

*  類別、 結構、 介面或委派的宣告可能包含`unsafe`修飾詞，在其中案例 （包括類別、 結構或介面的主體） 該型別宣告的整個文字範圍被視為不安全的內容。
*  欄位、 方法、 屬性、 事件、 索引子、 運算子、 執行個體建構函式、 解構函式或靜態建構函式的宣告可能包含`unsafe`修飾詞，在其中案例該成員宣告的整個文字範圍被視為不安全內容。
*  *Unsafe_statement*可讓您使用不安全的內容內*區塊*。 整個文字範圍相關聯*區塊*會被視為不安全的內容。

相關聯的文法生產如下所示。

```antlr
class_modifier_unsafe
    : 'unsafe'
    ;

struct_modifier_unsafe
    : 'unsafe'
    ;

interface_modifier_unsafe
    : 'unsafe'
    ;

delegate_modifier_unsafe
    : 'unsafe'
    ;

field_modifier_unsafe
    : 'unsafe'
    ;

method_modifier_unsafe
    : 'unsafe'
    ;

property_modifier_unsafe
    : 'unsafe'
    ;

event_modifier_unsafe
    : 'unsafe'
    ;

indexer_modifier_unsafe
    : 'unsafe'
    ;

operator_modifier_unsafe
    : 'unsafe'
    ;

constructor_modifier_unsafe
    : 'unsafe'
    ;

destructor_declaration_unsafe
    : attributes? 'extern'? 'unsafe'? '~' identifier '(' ')' destructor_body
    | attributes? 'unsafe'? 'extern'? '~' identifier '(' ')' destructor_body
    ;

static_constructor_modifiers_unsafe
    : 'extern'? 'unsafe'? 'static'
    | 'unsafe'? 'extern'? 'static'
    | 'extern'? 'static' 'unsafe'?
    | 'unsafe'? 'static' 'extern'?
    | 'static' 'extern'? 'unsafe'?
    | 'static' 'unsafe'? 'extern'?
    ;

embedded_statement_unsafe
    : unsafe_statement
    | fixed_statement
    ;

unsafe_statement
    : 'unsafe' block
    ;
```

在範例

```csharp
public unsafe struct Node
{
    public int Value;
    public Node* Left;
    public Node* Right;
}
```

`unsafe`結構宣告中指定的修飾詞會造成在結構宣告，變得不安全的內容的整個文字範圍。 因此，就可以宣告`Left`和`Right`是指標類型的欄位。 也可以撰寫上面的範例

```csharp
public struct Node
{
    public int Value;
    public unsafe Node* Left;
    public unsafe Node* Right;
}
```

在這裡，`unsafe`中的欄位宣告修飾詞會造成這些宣告，才會被視為不安全的內容。

建立不安全的內容，因此允許使用指標類型以外`unsafe`修飾詞都不會影響類型或成員。 在範例

```csharp
public class A
{
    public unsafe virtual void F() {
        char* p;
        ...
    }
}

public class B: A
{
    public override void F() {
        base.F();
        ...
    }
}
```

`unsafe`修飾詞`F`方法中的`A`只會導致文字範圍`F`變得不安全的內容可以在其中使用不安全的語言功能。 中的覆寫`F`中`B`，則不需要重新指定`unsafe`修飾詞，除非，當然`F`中的方法`B`本身必須存取不安全的功能。

指標類型是方法簽章的一部分時，情況就會稍有不同

```csharp
public unsafe class A
{
    public virtual void F(char* p) {...}
}

public class B: A
{
    public unsafe override void F(char* p) {...}
}
```

在這裡，因為`F`的簽章包含指標類型，它只能寫入 unsafe 內容中。 不過，不安全的內容可以藉由任一整個類別不安全，在此情況下，在引入`A`，或包含`unsafe`修飾詞在方法宣告中，在此情況下，在`B`。

## <a name="pointer-types"></a>指標類型

在 unsafe 內容中，*型別*([型別](types.md)) 可能*pointer_type* ，以及*value_type*或*reference_type*. 不過， *pointer_type*也可用於`typeof`運算式 ([匿名物件建立運算式](expressions.md#anonymous-object-creation-expressions)) 不安全的內容外，這類使用量不是不安全。

```antlr
type_unsafe
    : pointer_type
    ;
```

A *pointer_type*寫成*unmanaged_type*或關鍵字`void`，後面接著`*`語彙基元：

```antlr
pointer_type
    : unmanaged_type '*'
    | 'void' '*'
    ;

unmanaged_type
    : type
    ;
```

前面所指定的型別`*`指標呼叫的型別***參照型別***與指標類型。 它代表指標類型的值所指向變數的型別。

不同於參考 （參考類型的值），記憶體回收行程不會追蹤指標--記憶體回收行程並不知道指標和它們所指向的資料。 針對此原因的指標不允許指向參考或結構，其中包含參考，而指標的參考型別必須是*unmanaged_type*。

*Unmanaged_type*是不是任何型別*reference_type*或建構的型別，而且未包含*reference_type*或建構的任何層級的型別欄位巢狀。 亦即*unmanaged_type*是下列其中之一：

*  `sbyte``byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char`， `float`， `double`， `decimal`，或`bool`。
*  任何*enum_type*。
*  任何*pointer_type*。
*  任何使用者定義*struct_type*中不是建構的類型，並包含的欄位*unmanaged_type*僅。

混合的指標和參考的直覺式規則是允許包含指標、 參考 （物件），但不是允許包含參考的指標的參考。

指標類型的一些範例會提供下表中：

| __範例__ | __描述__                               |
|-------------|-----------------------------------------------|
| `byte*`     | 指標 `byte`                             |
| `char*`     | 指標 `char`                             |
| `int**`     | 指標的指標 `int`                   |
| `int*[]`    | 單一維度陣列的指標 `int` |
| `void*`     | 未知的類型指標                       |

對於給定的實作，所有指標類型必須都具有相同的大小和表示法。

不同於 C 和C++，當在相同的宣告中宣告多個指標C#`*`基礎型別，以及寫入不為每個指標名稱的前置標點符號。 例如

```csharp
int* pi, pj;    // NOT as int *pi, *pj;
```

具有類型指標的值`T*`表示的型別變數的位址`T`。 指標間接運算子`*`([指標間接取值](unsafe-code.md#pointer-indirection)) 可用來存取這個變數。 例如，假設變數`P`型別的`int*`，運算式`*P`代表`int`變數中包含的位址，請參閱`P`。

指標可能是物件參考，例如`null`。 間接取值運算子，以套用`null`指標會產生實作定義行為。 值的指標`null`由零的所有位元。

`void*`型別代表未知類型的指標。 間接取值運算子參照類型是未知的因為無法套用至類型的指標`void*`，也可以執行的任何計算這類的指標。 不過，類型的指標`void*`可轉換成任何其他指標類型 （反之亦然）。

指標類型是一個不同的類別的型別。 不同於參考型別和實值型別，指標類型不是繼承自`object`以及指標類型之間的任何轉換存在和`object`。 特別是，boxing 和 unboxing ([Boxing 和 unboxing](types.md#boxing-and-unboxing)) 不支援指標。 不過，在不同的指標類型之間以及指標類型和整數類資料類型之間被可以進行轉換。 這中所述[指標轉換](unsafe-code.md#pointer-conversions)。

A *pointer_type*不能作為類型引數 ([建構型別](types.md#constructed-types))，與型別推斷 ([型別推斷](expressions.md#type-inference)) 上會有推斷的一般方法呼叫失敗輸入是指標類型的引數。

A *pointer_type*皆可用為 volatile 欄位的型別 ([Volatile 欄位](classes.md#volatile-fields))。

雖然指標可以當做`ref`或`out`參數，這樣可能會導致未定義的行為，因為指標可能也會設定為指向區域變數已不存在時呼叫的方法傳回時或它的固定的物件用來點，不會再修正。 例如: 

```csharp
using System;

class Test
{
    static int value = 20;

    unsafe static void F(out int* pi1, ref int* pi2) {
        int i = 10;
        pi1 = &i;

        fixed (int* pj = &value) {
            // ...
            pi2 = pj;
        }
    }

    static void Main() {
        int i = 10;
        unsafe {
            int* px1;
            int* px2 = &i;

            F(out px1, ref px2);

            Console.WriteLine("*px1 = {0}, *px2 = {1}",
                *px1, *px2);    // undefined behavior
        }
    }
}
```

一種方法可以傳回某種類型的值和該類型可以是指標。 例如，當指定指標的連續序列`int`s，該序列的項目計數，以及一些其他`int`值，下列方法會傳回該值的地址以該序列中，如果相符，否則傳回`null`:

```csharp
unsafe static int* Find(int* pi, int size, int value) {
    for (int i = 0; i < size; ++i) {
        if (*pi == value) 
            return pi;
        ++pi;
    }
    return null;
}
```

在 unsafe 內容中，數個建構可用來操作指標：

*  `*`運算子可用來執行指標間接取值 ([指標間接取值](unsafe-code.md#pointer-indirection))。
*  `->`運算子可用來透過指標存取結構的成員 ([指標成員存取](unsafe-code.md#pointer-member-access))。
*  `[]`運算子可用來編製索引的指標 ([指標元素存取](unsafe-code.md#pointer-element-access))。
*  `&`運算子可用來取得變數位址 ([傳址運算子](unsafe-code.md#the-address-of-operator))。
*  `++`並`--`運算子可能會用來遞增和遞減指標 ([指標遞增和遞減](unsafe-code.md#pointer-increment-and-decrement))。
*  `+`並`-`運算子可用來執行指標算術 ([指標算術](unsafe-code.md#pointer-arithmetic))。
*  `==`， `!=`， `<`， `>`， `<=`，和`=>`運算子可用來比較指標 ([指標比較](unsafe-code.md#pointer-comparison))。
*  `stackalloc`運算子可用來配置呼叫堆疊的記憶體 ([固定大小緩衝區](unsafe-code.md#fixed-size-buffers))。
*  `fixed`陳述式可用來暫時修正變數，因此您可以取得其位址 ([fixed 陳述式](unsafe-code.md#the-fixed-statement))。

## <a name="fixed-and-moveable-variables"></a>固定和可移動變數

傳址運算子 ([傳址運算子](unsafe-code.md#the-address-of-operator)) 和`fixed`陳述式 ([fixed 陳述式](unsafe-code.md#the-fixed-statement)) 分成兩大類別的變數：***已修正變數***並***可移動的變數***。

固定的變數位於儲存體位置，不會受到記憶體回收行程的作業。 （固定變數的範例包括本機變數、 值參數和變數建立為指標取值）。相反地，可移動的變數存在於受限於記憶體回收行程的重新配置或配置的儲存體位置中。 （可移動的變數的範例包括欄位中物件和陣列的元素。）

`&`運算子 ([傳址運算子](unsafe-code.md#the-address-of-operator)) 允許固定的變數，以取得無限制的位址。 不過，因為可移動的變數受限於記憶體回收行程的重新配置或配置，可移動的變數的位址可以只使用來取得`fixed`陳述式 ([fixed 陳述式](unsafe-code.md#the-fixed-statement))，和該位址只會針對該持續時間會保持有效`fixed`陳述式。

準確地說，固定的變數可以是下列其中一項：

*  變數所得*simple_name* ([簡單名稱](expressions.md#simple-names))，指的本機變數或值參數，除非該變數會擷取匿名函式。
*  變數所得*member_access* ([成員存取](expressions.md#member-access)) 的表單`V.I`，其中`V`是固定的變數*struct_type*。
*  變數的所產生的*pointer_indirection_expression* ([指標間接取值](unsafe-code.md#pointer-indirection)) 的形式`*P`，則*pointer_member_access* ([指標成員存取](unsafe-code.md#pointer-member-access)) 的形式`P->I`，或*pointer_element_access* ([指標元素存取](unsafe-code.md#pointer-element-access)) 表單的`P[E]`。

所有其他變數會分類為可移動的變數。

請注意靜態欄位會分類為可移動的變數。 也請注意，`ref`或`out`參數分類為可移動的變數，即使指定給參數的引數是固定的變數。 最後，請注意，所產生的取值指標變數一律會分類為固定變數。

## <a name="pointer-conversions"></a>指標轉換

在 unsafe 內容中，可用的隱含轉換的集合 ([隱含轉換](conversions.md#implicit-conversions)) 已擴充而納入下列的隱含指標轉換：

*  從任何*pointer_type*型別`void*`。
*  從`null`常值的任何*pointer_type*。

此外，在不安全的內容，可用的明確轉換的集合 ([明確轉換](conversions.md#explicit-conversions)) 已擴充而納入下列明確指標轉換：

*  從任何*pointer_type*任何其他*pointer_type*。
*  從`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`，或`ulong`任何*pointer_type*。
*  從任何*pointer_type*要`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`，或`ulong`。

最後，在不安全的內容，標準的隱含轉換的集合 ([標準的隱含轉換](conversions.md#standard-implicit-conversions)) 包含下列指標的轉換：

*  從任何*pointer_type*型別`void*`。

兩個指標類型之間轉換永遠不會變更實際的指標值。 換句話說，從一個指標類型轉換為另一個沒有指標所提供的基礎位址上的任何作用。

當一個指標類型轉換到另一個，如果產生的指標不指向類型正確對齊時，如果已取值的結果行為是未定義。 一般情況下，「 正確對齊 」 的概念是可轉移的： 如果類型的指標`A`正確對齊類型的指標`B`，它會依次正確對齊的類型的指標`C`，然後指向的型別`A`正確對齊類型的指標`C`。

請考慮在其中有一種類型的變數透過不同類型的指標存取下列情況：

```csharp
char c = 'A';
char* pc = &c;
void* pv = pc;
int* pi = (int*)pv;
int i = *pi;         // undefined
*pi = 123456;        // undefined
```

當指標類型轉換成位元組，指標結果會指向的最低定址位元組的變數。 後續的增量，產生的結果，但不超過變數的大小會產生剩餘的位元組，該變數的指標。 例如，下列方法會顯示每個八個位元組的雙精度浮點數中做為十六進位的值：

```csharp
using System;

class Test
{
    unsafe static void Main() {
      double d = 123.456e23;
        unsafe {
           byte* pb = (byte*)&d;
            for (int i = 0; i < sizeof(double); ++i)
               Console.Write("{0:X2} ", *pb++);
            Console.WriteLine();
        }
    }
}
```

當然，產生的輸出取決於位元組序。

指標與整數之間的對應是由實作定義。 不過，在 32 * 和 64 位元 CPU 架構與線性位址空間的指標的轉換整數類資料類型通常和行為完全相同的轉換`uint`或`ulong`分別，或從這些整數類資料類型的值。

### <a name="pointer-arrays"></a>指標陣列

在 unsafe 內容中，可以建構陣列的指標。 只對某些適用於其他陣列類型的轉換可在指標陣列：

*  隱含參考轉換 ([隱含參考轉換](conversions.md#implicit-reference-conversions)) 從任何*array_type*到`System.Array`，它也會實作的介面將套用至指標陣列。 不過，任何嘗試存取陣列項目，透過`System.Array`或它所實作的介面將會導致在執行階段發生例外狀況為指標類型不可以轉換成`object`。
*  隱含和明確參考轉換 ([隱含參考轉換](conversions.md#implicit-reference-conversions)，[明確參考轉換](conversions.md#explicit-reference-conversions)) 從一維陣列的型別`S[]`至`System.Collections.Generic.IList<T>`和其一般基底介面永遠不會用於指標的陣列，因為指標類型不能當做類型引數，而且沒有指標類型的轉換成非指標類型。
*  明確參考轉換 ([明確參考轉換](conversions.md#explicit-reference-conversions)) 從`System.Array`介面的方式實作任何*array_type*適用於指標的陣列。
*  明確參考轉換 ([明確參考轉換](conversions.md#explicit-reference-conversions)) 從`System.Collections.Generic.IList<S>`和其基底介面的一維陣列類型`T[]`永遠不會套用至指標陣列，因為不能是指標類型使用做為型別引數，並沒有指標類型的轉換成非指標類型。

這些限制表示，用於擴充`foreach`陣列上的陳述式中所述[foreach 陳述式](statements.md#the-foreach-statement)不適用於指標的陣列。 相反地，在表單的 foreach 陳述式

```csharp
foreach (V v in x) embedded_statement
```

其中的型別`x`是陣列類型的形式`T[,,...,]`，`N`會減 1 的維度數目並`T`或`V`是指標類型，會擴大使用巢狀的 for 迴圈，如下所示：

```csharp
{
    T[,,...,] a = x;
    for (int i0 = a.GetLowerBound(0); i0 <= a.GetUpperBound(0); i0++)
    for (int i1 = a.GetLowerBound(1); i1 <= a.GetUpperBound(1); i1++)
    ...
    for (int iN = a.GetLowerBound(N); iN <= a.GetUpperBound(N); iN++) {
        V v = (V)a.GetValue(i0,i1,...,iN);
        embedded_statement
    }
}
```

變數`a`， `i0`， `i1`、...、`iN`不可見的或存取`x`或*embedded_statement*或任何其他來源的程式碼的程式。 變數`v`是唯讀，在內嵌的陳述式。 如果沒有明確的轉換 ([指標轉換](unsafe-code.md#pointer-conversions)) 從`T`（項目型別） 到`V`，會產生錯誤並採取任何進一步的步驟。 如果`x`具有值`null`、`System.NullReferenceException`會在執行階段擲回。

## <a name="pointers-in-expressions"></a>運算式中的指標

在 unsafe 內容中，運算式可能會產生結果的指標類型，但不安全的內容之外，它是指標類型運算式的編譯時期錯誤。 準確地說，不安全的內容之外發生編譯時期錯誤，就會發生的話*simple_name* ([簡單名稱](expressions.md#simple-names))， *member_access* ([成員存取](expressions.md#member-access))， *invocation_expression* ([引動過程運算式](expressions.md#invocation-expressions))，或*element_access* ([元素存取](expressions.md#element-access)) 的指標類型。

在 unsafe 內容中， *primary_no_array_creation_expression* ([主要運算式](expressions.md#primary-expressions)) 和*unary_expression* ([的一元運算子](expressions.md#unary-operators))生產允許下列的其他建構函式：

```antlr
primary_no_array_creation_expression_unsafe
    : pointer_member_access
    | pointer_element_access
    | sizeof_expression
    ;

unary_expression_unsafe
    : pointer_indirection_expression
    | addressof_expression
    ;
```

這些建構是由下列各節所述。 文法所默許的優先順序和順序關聯性的不安全的運算子。

### <a name="pointer-indirection"></a>指標間接取值

A *pointer_indirection_expression*包含星號 (`*`) 後面接著*unary_expression*。

```antlr
pointer_indirection_expression
    : '*' unary_expression
    ;
```

一元`*`表示指標間接取值運算子，並用來取得指標所指向的變數。 評估的結果`*P`，其中`P`是指標類型的運算式`T*`，這類型的變數`T`。 它是編譯時期錯誤套用一元`*`類型的運算式的運算子`void*`或不是指標類型的運算式。

套用一元的效果`*`運算子來`null`指標是由實作定義。 特別是，則此作業會擲回無法保證`System.NullReferenceException`。

如果無效的值已指派給指標，也就是一元的行為`*`運算子是未定義。 無效值的指標取值的一元`*`運算子會不一致的類型指的位址 (在中的範例，請參閱[指標轉換](unsafe-code.md#pointer-conversions))，以及之後的變數的位址存留期結束。

為了明確設定分析的詳細資訊，藉由評估格式的運算式所產生的變數`*P`被視為一開始指派 ([最初指派變數](variables.md#initially-assigned-variables))。

### <a name="pointer-member-access"></a>指標成員存取

A *pointer_member_access*組成*primary_expression*，後面接著"`->`"語彙基元，後面接著*識別碼*和選擇性的*type_argument_list*。

```antlr
pointer_member_access
    : primary_expression '->' identifier
    ;
```

在表單的指標成員存取`P->I`，`P`必須是指標類型的運算式而非`void*`，和`I`必須表示成的型別可存取成員`P`點。

指標成員存取的表單`P->I`評估為完全`(*P).I`。 如需指標間接運算子的說明 (`*`)，請參閱 <<c2> [ 指標間接取值](unsafe-code.md#pointer-indirection)。 如需說明的成員存取運算子 (`.`)，請參閱 <<c2> [ 成員存取](expressions.md#member-access)。

在範例

```csharp
using System;

struct Point
{
    public int x;
    public int y;

    public override string ToString() {
        return "(" + x + "," + y + ")";
    }
}

class Test
{
    static void Main() {
        Point point;
        unsafe {
            Point* p = &point;
            p->x = 10;
            p->y = 20;
            Console.WriteLine(p->ToString());
        }
    }
}
```

`->`運算子用來存取欄位，並叫用透過指標結構的方法。 因為作業`P->I`就相當於`(*P).I`，則`Main`方法可以同樣撰寫：

```csharp
class Test
{
    static void Main() {
        Point point;
        unsafe {
            Point* p = &point;
            (*p).x = 10;
            (*p).y = 20;
            Console.WriteLine((*p).ToString());
        }
    }
}
```

### <a name="pointer-element-access"></a>指標元素存取

A *pointer_element_access*組成*primary_no_array_creation_expression*後面接著括住的運算式"`[`"和"`]`」。

```antlr
pointer_element_access
    : primary_no_array_creation_expression '[' expression ']'
    ;
```

在表單的指標元素存取`P[E]`，`P`必須是指標類型的運算式而非`void*`，和`E`必須是能夠隱含轉換成的運算式`int`， `uint`， `long`，或`ulong`。

表單的指標元素存取`P[E]`評估為完全`*(P + E)`。 如需指標間接運算子的說明 (`*`)，請參閱 <<c2> [ 指標間接取值](unsafe-code.md#pointer-indirection)。 如需指標的加法運算子的說明 (`+`)，請參閱 <<c2> [ 指標算術](unsafe-code.md#pointer-arithmetic)。

在範例

```csharp
class Test
{
    static void Main() {
        unsafe {
            char* p = stackalloc char[256];
            for (int i = 0; i < 256; i++) p[i] = (char)i;
        }
    }
}
```

指標元素存取用來初始化中的字元緩衝區`for`迴圈。 因為作業`P[E]`就相當於`*(P + E)`，範例可以同樣撰寫：

```csharp
class Test
{
    static void Main() {
        unsafe {
            char* p = stackalloc char[256];
            for (int i = 0; i < 256; i++) *(p + i) = (char)i;
        }
    }
}
```

指標的項目存取運算子不會超出範圍檢查錯誤及存取時的行為項目未定義超出範圍。 這等同於 C 和C++。

### <a name="the-address-of-operator"></a>傳址運算子

*Addressof_expression*包含連字號 (`&`) 後面接著*unary_expression*。

```antlr
addressof_expression
    : '&' unary_expression
    ;
```

指定運算式`E`類型`T`歸類為固定變數和 ([固定和可移動變數](unsafe-code.md#fixed-and-moveable-variables))，建構`&E`計算所指定變數的位址`E`. 結果的型別是`T*`和分類為值。 如果，就會發生編譯時期錯誤`E`未分類為變數中，如果`E`歸類為唯讀的本機變數，或如果`E`代表可移動的變數。 在最後一個案例中，在 fixed 陳述式 ([fixed 陳述式](unsafe-code.md#the-fixed-statement)) 可用來暫時 「 修正 」 變數，然後再取得其位址。 中所述[成員存取](expressions.md#member-access)、 執行個體建構函式或靜態的建構函式的結構或類別來定義外部`readonly` 欄位中，該欄位會被視為值，而不是變數。 因此，無法取得其位址。 同樣地，您無法取得常數的位址。

`&`運算子不需要它的引數，明確指派，但下列`&`作業中，套用運算子的變數會被視為已明確指派作業發生所在的執行路徑中。 它是以確保該正確地初始化變數的程式設計人員的責任實際未發生在此情況下。

在範例

```csharp
using System;

class Test
{
    static void Main() {
        int i;
        unsafe {
            int* p = &i;
            *p = 123;
        }
        Console.WriteLine(i);
    }
}
```

`i` 會被視為明確指派後面`&i`用來初始化作業`p`。 指派給`*p`實際上是初始化`i`，這項初始化包含負責程式設計人員，但如果已移除指派，就會發生任何編譯時期錯誤。

明確指派之規則`&`運算子存在，您可以避免多餘的初始化的區域變數。 比方說，許多外部 Api 需要 api 會填入結構的指標。 這類 Api 的呼叫一般為 pass 本機結構變數的位址，且沒有規則，多餘的初始化結構變數的需要。

### <a name="pointer-increment-and-decrement"></a>指標遞增和遞減

在 unsafe 內容中，`++`並`--`運算子 ([後置遞增和遞減運算子](expressions.md#postfix-increment-and-decrement-operators)並[前置遞增和遞減運算子](expressions.md#prefix-increment-and-decrement-operators)) 可以套用至指標以外的所有類型的變數`void*`。 因此，針對每個指標類型`T*`，隱含定義了下列運算子：

```csharp
T* operator ++(T* x);
T* operator --(T* x);
```

這些運算子會產生與相同的結果`x + 1`並`x - 1`分別 ([指標算術](unsafe-code.md#pointer-arithmetic))。 換句話說，之指標變數的型別`T*`，則`++`運算子會加入`sizeof(T)`變數中所包含的位址和`--`運算子減去`sizeof(T)`從變數中所包含的位址。

如果指標遞增或遞減運算讓指標類型的定義域溢位，結果是由實作定義，但會產生任何例外狀況。

### <a name="pointer-arithmetic"></a>指標算術

在 unsafe 內容中，`+`並`-`運算子 ([加法運算子](expressions.md#addition-operator)並[減法運算子](expressions.md#subtraction-operator)) 可以套用至值以外的所有指標類型`void*`。 因此，針對每個指標類型`T*`，隱含定義了下列運算子：

```csharp
T* operator +(T* x, int y);
T* operator +(T* x, uint y);
T* operator +(T* x, long y);
T* operator +(T* x, ulong y);

T* operator +(int x, T* y);
T* operator +(uint x, T* y);
T* operator +(long x, T* y);
T* operator +(ulong x, T* y);

T* operator -(T* x, int y);
T* operator -(T* x, uint y);
T* operator -(T* x, long y);
T* operator -(T* x, ulong y);

long operator -(T* x, T* y);
```

指定運算式`P`指標型別的`T*`和運算式`N`型別的`int`， `uint`， `long`，或`ulong`，運算式`P + N`和`N + P`計算指標值的型別`T*`，會將`N * sizeof(T)`所指定的位址`P`。 同樣地，運算式`P - N`計算類型的指標值`T*`所產生減去`N * sizeof(T)`所指定的位址`P`。

給定兩個運算式，`P`並`Q`，指標類型的`T*`，運算式`P - Q`計算指定的位址之間的差異`P`和`Q`並再將該差異除以`sizeof(T)`. 結果的型別一律為`long`。 實際上`P - Q`會計算為`((long)(P) - (long)(Q)) / sizeof(T)`。

例如: 

```csharp
using System;

class Test
{
    static void Main() {
        unsafe {
            int* values = stackalloc int[20];
            int* p = &values[1];
            int* q = &values[15];
            Console.WriteLine("p - q = {0}", p - q);
            Console.WriteLine("q - p = {0}", q - p);
        }
    }
}
```

產生下列輸出：

```
p - q = -14
q - p = 14
```

如果指標的算術運算溢位的指標類型的網域，結果會截斷以實作定義的方式，但會產生任何例外狀況。

### <a name="pointer-comparison"></a>指標比較

在 unsafe 內容中， `==`， `!=`， `<`， `>`， `<=`，和`=>`運算子 ([關係和類型測試運算子](expressions.md#relational-and-type-testing-operators)) 可以套用至所有的值指標類型。 指標比較運算子包括︰

```csharp
bool operator ==(void* x, void* y);
bool operator !=(void* x, void* y);
bool operator <(void* x, void* y);
bool operator >(void* x, void* y);
bool operator <=(void* x, void* y);
bool operator >=(void* x, void* y);
```

因為隱含轉換存在任何指標類型，若要從`void*`能夠使用這些運算子比較型別，任何指標類型的運算元。 比較運算子比較兩個運算元所指定，如同它們是不帶正負號的整數的位址。

### <a name="the-sizeof-operator"></a>Sizeof 運算子

`sizeof`運算子會傳回指定型別的變數所佔用的位元組數目。 做為運算元所指定的型別`sizeof`必須是*unmanaged_type* ([指標類型](unsafe-code.md#pointer-types))。

```antlr
sizeof_expression
    : 'sizeof' '(' unmanaged_type ')'
    ;
```

結果`sizeof`運算子是值型別的`int`。 針對某些預先定義的型別，`sizeof`運算子會產生常數值下, 表所示。


| __運算式__   | __結果__ |
|------------------|------------|
| `sizeof(sbyte)`  | `1`        |
| `sizeof(byte)`   | `1`        |
| `sizeof(short)`  | `2`        |
| `sizeof(ushort)` | `2`        |
| `sizeof(int)`    | `4`        |
| `sizeof(uint)`   | `4`        |
| `sizeof(long)`   | `8`        |
| `sizeof(ulong)`  | `8`        |
| `sizeof(char)`   | `2`        |
| `sizeof(float)`  | `4`        |
| `sizeof(double)` | `8`        |
| `sizeof(bool)`   | `1`        |

對於所有其他類型，結果`sizeof`運算子是由實作定義，並會分類為值，而不是常數。

未指定封裝至結構的成員的順序。

用於對齊用途，可能有未命名的填補結構，結構內的開頭和結尾的結構。 用來做為填補字元位元的內容就無法確定。

當套用至具有結構類型的運算元，結果會是該型別，包括任何填補字元變數中的位元組總數。

## <a name="the-fixed-statement"></a>Fixed 陳述式

在 unsafe 內容中， *embedded_statement* ([陳述式](statements.md)) 生產環境允許額外的建構，`fixed`陳述式，這用來 「 修正 」 可移動的變數，其位址會維持不變的陳述式的持續時間。

```antlr
fixed_statement
    : 'fixed' '(' pointer_type fixed_pointer_declarators ')' embedded_statement
    ;

fixed_pointer_declarators
    : fixed_pointer_declarator (','  fixed_pointer_declarator)*
    ;

fixed_pointer_declarator
    : identifier '=' fixed_pointer_initializer
    ;

fixed_pointer_initializer
    : '&' variable_reference
    | expression
    ;
```

每個*fixed_pointer_declarator*宣告的區域變數指定*pointer_type* ，並使用計算由相對應的位址初始化，區域變數*fixed_pointer_initializer*。 在宣告的區域變數`fixed`陳述式是在任何可存取*fixed_pointer_initializer*發生該變數的宣告，並在右邊的 s *embedded_statement*的`fixed`陳述式。 所宣告的區域變數`fixed`陳述式會被視為唯讀。 如果內嵌的陳述式嘗試修改這個本機變數，就會發生編譯時期錯誤 (透過指派或`++`並`--`運算子) 或將它傳遞為`ref`或`out`參數。

A *fixed_pointer_initializer*可以是下列其中之一：

*  語彙基元"`&`"後面接著*variable_reference* ([精確規則決定明確](variables.md#precise-rules-for-determining-definite-assignment)) 可移動的變數 ([固定和可移動變數](unsafe-code.md#fixed-and-moveable-variables))非受控型別的`T`，提供型別`T*`隱含地轉換成指標類型所提供`fixed`陳述式。 在此情況下，初始設定式會計算指定變數的位址，並保證變數會在固定位址的持續時間`fixed`陳述式。
*  運算式*array_type* unmanaged 類型的項目`T`，提供型別`T*`隱含地轉換成指標類型所提供`fixed`陳述式。 在此情況下，初始設定式計算陣列中的第一個元素的位址，並將維持固定位址的持續時間保證，整個陣列`fixed`陳述式。 如果陣列運算式為 null，或如果陣列中有零個項目，則初始設定式會計算等於零的位址。
*  類型的運算式`string`，提供型別`char*`隱含地轉換成指標類型所提供`fixed`陳述式。 在此情況下，初始設定式計算，以字串的第一個字元的位址，而且整個字串保證維持在固定位址的持續時間`fixed`陳述式。 行為`fixed`陳述式會實作定義的字串運算式是否為 null。
*  A *simple_name*或是*member_access*提供固定的大小緩衝區成員的類型是隱含地轉換成指標型別，指定參考的可移動的變數，固定的大小緩衝區成員在 `fixed`陳述式。 初始設定式在此情況下，計算的第一個元素的固定的大小緩衝區的指標 ([運算式中固定大小緩衝區](unsafe-code.md#fixed-size-buffers-in-expressions))，並保證的固定的大小緩衝區期間保持固定位址`fixed`陳述式。

每個位址來計算*fixed_pointer_initializer* `fixed`陳述式可確保位址所參考的變數不是重新配置或記憶體回收行程期間的處置`fixed`陳述式。 例如，如果所計算的地址*fixed_pointer_initializer*參考物件的欄位或陣列執行個體的項目`fixed`陳述式可以保證包含的物件執行個體不會重新定位或處置陳述式的存留期間。

程式設計人員必須負責確保所建立的指標`fixed`陳述式不會存留超過執行這些陳述式。 例如，當指標建立`fixed`陳述式傳遞至外部 Api、 程式設計人員必須負責確保 Api 保有這些指標的任何記憶體。

（因為它們不能移動），則固定的物件可能會造成堆積的片段。 基於這個理由，才將物件固定只在絕對必要時，然後只針對最短的時間可能量。

此範例

```csharp
class Test
{
    static int x;
    int y;

    unsafe static void F(int* p) {
        *p = 1;
    }

    static void Main() {
        Test t = new Test();
        int[] a = new int[10];
        unsafe {
            fixed (int* p = &x) F(p);
            fixed (int* p = &t.y) F(p);
            fixed (int* p = &a[0]) F(p);
            fixed (int* p = a) F(p);
        }
    }
}
```

示範了多個`fixed`陳述式。 第一個陳述式修正，並取得靜態欄位的位址、 第二個陳述式修正，並取得的地址的執行個體欄位，和第三個陳述式修正，並取得陣列元素的位址。 每個案例中就會使用一般錯誤`&`運算子，因為變數會歸類為可移動的變數。

第四個`fixed`在上述範例中的陳述式會產生類似的結果，第三個。

此範例中的`fixed`陳述式會使用`string`:

```csharp
class Test
{
    static string name = "xx";

    unsafe static void F(char* p) {
        for (int i = 0; p[i] != '\0'; ++i)
            Console.WriteLine(p[i]);
    }

    static void Main() {
        unsafe {
            fixed (char* p = name) F(p);
            fixed (char* p = "xx") F(p);
        }
    }
}
```

在 unsafe 內容中的一維陣列的陣列項目會儲存在遞增索引順序，從索引`0`和結束索引`Length - 1`。 針對多維陣列，陣列項目會儲存，最右邊的維度的索引會增加第一次，然後下一步 左邊的維度，依此類推左邊。 內`fixed`取得的指標的陳述式`p`陣列執行個體`a`，範圍從指標值`p`到`p + a.Length - 1`代表陣列中元素的位址。 同樣地，範圍介於變數`p[0]`至`p[a.Length - 1]`代表實際的陣列項目。 給定陣列所儲存的方式，我們可以處理任何維度的陣列，就好像線性。

例如：

```csharp
using System;

class Test
{
    static void Main() {
        int[,,] a = new int[2,3,4];
        unsafe {
            fixed (int* p = a) {
                for (int i = 0; i < a.Length; ++i)    // treat as linear
                    p[i] = i;
            }
        }

        for (int i = 0; i < 2; ++i)
            for (int j = 0; j < 3; ++j) {
                for (int k = 0; k < 4; ++k)
                    Console.Write("[{0},{1},{2}] = {3,2} ", i, j, k, a[i,j,k]);
                Console.WriteLine();
            }
    }
}
```

產生下列輸出：

```
[0,0,0] =  0 [0,0,1] =  1 [0,0,2] =  2 [0,0,3] =  3
[0,1,0] =  4 [0,1,1] =  5 [0,1,2] =  6 [0,1,3] =  7
[0,2,0] =  8 [0,2,1] =  9 [0,2,2] = 10 [0,2,3] = 11
[1,0,0] = 12 [1,0,1] = 13 [1,0,2] = 14 [1,0,3] = 15
[1,1,0] = 16 [1,1,1] = 17 [1,1,2] = 18 [1,1,3] = 19
[1,2,0] = 20 [1,2,1] = 21 [1,2,2] = 22 [1,2,3] = 23
```

在範例

```csharp
class Test
{
    unsafe static void Fill(int* p, int count, int value) {
        for (; count != 0; count--) *p++ = value;
    }

    static void Main() {
        int[] a = new int[100];
        unsafe {
            fixed (int* p = a) Fill(p, 100, -1);
        }
    }
}
```

`fixed`陳述式用來修正陣列，因此它的位址可以傳遞至採用指標的方法。

在下列範例中：

```csharp
unsafe struct Font
{
    public int size;
    public fixed char name[32];
}

class Test
{
    unsafe static void PutString(string s, char* buffer, int bufSize) {
        int len = s.Length;
        if (len > bufSize) len = bufSize;
        for (int i = 0; i < len; i++) buffer[i] = s[i];
        for (int i = len; i < bufSize; i++) buffer[i] = (char)0;
    }

    Font f;

    unsafe static void Main()
    {
        Test test = new Test();
        test.f.size = 10;
        fixed (char* p = test.f.name) {
            PutString("Times New Roman", p, 32);
        }
    }
}
```

fixed 陳述式用來修正固定的大小緩衝區的結構，因此可以使用其位址的指標。

A`char*`藉由修正一律指向以 null 結束的字串的字串執行個體所產生的值。 取得指標 fixed 陳述式內`p`字串執行個體`s`，範圍從指標值`p`來`p + s.Length - 1`代表字串中，而指標值中字元的位址`p + s.Length`永遠指向 null 字元 (具有值的字元`'\0'`)。

透過固定的指標的 managed 類型的修改的物件可以產生未定義的行為。 例如，字串是不可變的因為它是程式設計人員必須負責確保不會修改為固定字串的指標所參考的字元。

呼叫外部"C style"字串的 Api 時，自動 null 終止的字串會格外有用。 不過請注意，字串執行個體可以含有 null 字元。 如果存在這類的 null 字元，字串會被截斷視為以 null 結束時`char*`。

## <a name="fixed-size-buffers"></a>固定的大小緩衝區

固定的大小緩衝區用來將"C style"內嵌陣列宣告為結構的成員，並且主要適用於與 unmanaged Api 互動。

### <a name="fixed-size-buffer-declarations"></a>固定的大小緩衝區宣告

A***固定大小緩衝區***成員，表示儲存體的固定的長度的緩衝區的指定類型的變數。 固定的大小緩衝區宣告導入了一個或多個固定的大小的緩衝區的指定項目類型。 只允許在結構宣告固定的大小緩衝區，並且只能在不安全的內容中 ([Unsafe 內容](unsafe-code.md#unsafe-contexts))。

```antlr
struct_member_declaration_unsafe
    : fixed_size_buffer_declaration
    ;

fixed_size_buffer_declaration
    : attributes? fixed_size_buffer_modifier* 'fixed' buffer_element_type fixed_size_buffer_declarator+ ';'
    ;

fixed_size_buffer_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'unsafe'
    ;

buffer_element_type
    : type
    ;

fixed_size_buffer_declarator
    : identifier '[' constant_expression ']'
    ;
```

固定的大小緩衝區宣告可能包含一組屬性 ([屬性](attributes.md))，則`new`修飾詞 ([修飾詞](classes.md#modifiers))，是有效的四種存取修飾詞組合 ([類型參數和條件約束](classes.md#type-parameters-and-constraints)) 和`unsafe`修飾詞 ([Unsafe 內容](unsafe-code.md#unsafe-contexts))。 屬性和修飾詞套用至所有固定的大小緩衝區宣告所宣告的成員。 它會在固定的大小緩衝區宣告中出現多次相同的修飾詞產生錯誤。

固定的大小緩衝區宣告不允許包含`static`修飾詞。

固定的大小緩衝區宣告的緩衝區項目類型指定緩衝區的項目類型，宣告所引進。 緩衝區元素類型必須是其中一個預先定義的型別`sbyte`， `byte`， `short`， `ushort`， `int`， `uint`， `long`， `ulong`， `char`， `float`， `double`，或`bool`。

緩衝區元素型別後面接著一份固定的大小緩衝區的宣告子，其中每一個導入了新的成員。 固定的大小緩衝區宣告子包含名稱的成員，後面接著括住常數運算式的識別項`[`和`]`語彙基元。 常數運算式，即表示該固定的大小緩衝區宣告子所導入的成員中的項目數。 常數運算式的類型必須隱含轉換成類型`int`，而且此值必須是非零的正整數。

固定的大小緩衝區的項目一定會循序配置在記憶體中。

固定的大小緩衝區宣告會宣告多個固定的大小緩衝區就相當於單一的固定的大小緩衝區宣告具有相同的屬性和項目類型的多個宣告的。 例如

```csharp
unsafe struct A
{
   public fixed int x[5], y[10], z[100];
}
```

相當於

```csharp
unsafe struct A
{
   public fixed int x[5];
   public fixed int y[10];
   public fixed int z[100];
}
```

### <a name="fixed-size-buffers-in-expressions"></a>在運算式中的固定的大小緩衝區

成員查閱 ([運算子](expressions.md#operators)) 固定大小的緩衝區成員進行成員查詢欄位的完全相同。

在運算式中使用，可參考的固定的大小緩衝區*simple_name* ([型別推斷](expressions.md#type-inference)) 或*member_access* ([編譯時間檢查動態的多載解析](expressions.md#compile-time-checking-of-dynamic-overload-resolution))。

當以簡單名稱參考固定的大小緩衝區成員時，效果等同於表單的成員存取`this.I`，其中`I`是固定的大小緩衝區。

在表單的成員存取`E.I`，如果`E`的結構類型和成員查閱`I`在於結構類型會識別固定的大小的成員，然後`E.I`是評估分類，如下所示：

*  如果運算式`E.I`不會發生在不安全的內容中，就會發生編譯時期錯誤。
*  如果`E`會分類為值，就會發生編譯時期錯誤。
*  否則，如果`E`是可移動的變數 ([固定和可移動變數](unsafe-code.md#fixed-and-moveable-variables)) 和運算式`E.I`不*fixed_pointer_initializer* ([固定陳述式](unsafe-code.md#the-fixed-statement))，就會發生編譯時期錯誤。
*  否則，請`E`參考為固定的變數和運算式的結果是固定的大小緩衝區成員的第一個元素的指標`I`在`E`。 結果會是類型`S*`，其中`S`的項目類型`I`，並會分類為值。

固定的大小緩衝區的後續項目，可以使用指標作業，從第一個項目來存取。 不同於陣列的存取權，存取權的固定的大小緩衝區項目是不安全的作業，並不檢查的範圍。

下列範例會宣告，並使用固定的大小緩衝區成員的結構。

```csharp
unsafe struct Font
{
    public int size;
    public fixed char name[32];
}

class Test
{
    unsafe static void PutString(string s, char* buffer, int bufSize) {
        int len = s.Length;
        if (len > bufSize) len = bufSize;
        for (int i = 0; i < len; i++) buffer[i] = s[i];
        for (int i = len; i < bufSize; i++) buffer[i] = (char)0;
    }

    unsafe static void Main()
    {
        Font f;
        f.size = 10;
        PutString("Times New Roman", f.name, 32);
    }
}
```

### <a name="definite-assignment-checking"></a>明確設定檢查

固定的大小緩衝區並不受限於明確設定檢查 ([明確指派](variables.md#definite-assignment))，固定的大小緩衝區的成員也會檢查結構型別變數的明確指派的目的被忽略。

當最外層的固定的大小緩衝區成員包含結構變數的靜態變數、 類別執行個體或陣列元素的執行個體變數，固定的大小緩衝區的項目會自動初始化為其預設值 ([預設值](variables.md#default-values))。 在其他情況下，固定的大小緩衝區的初始內容未定義。

## <a name="stack-allocation"></a>堆疊配置

在 unsafe 內容中，本機變數宣告 ([區域變數宣告](statements.md#local-variable-declarations)) 可能包含的堆疊配置初始設定式在呼叫堆疊中所配置的記憶體。

```antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

*Unmanaged_type*表示類型的項目會儲存在新配置的位置，而*運算式*指出這些項目數目。 這些結合起來，指定所需的配置大小。 堆疊配置的大小不能是負的因為它是編譯時期錯誤，以指定的項目數目*constant_expression*評估為負數值。

表單的堆疊配置初始設定式`stackalloc T[E]`需要`T`非受控型別 ([指標型別](unsafe-code.md#pointer-types)) 和`E`是類型的運算式`int`。 建構配置`E * sizeof(T)`位元組從呼叫堆疊，並傳回型別的指標`T*`，新配置的區塊。 如果`E`是負值，則行為是未定義。 如果`E`為零，則會進行任何配置，並傳回的指標，是由實作定義。 如果不是記憶體不足，無法配置指定大小的區塊`System.StackOverflowException`就會擲回。

新配置內容會是記憶體的未定義。

堆疊配置的初始設定式中不允許`catch`或是`finally`區塊 ([try 陳述式](statements.md#the-try-statement))。

沒有任何方法，明確釋放出記憶體配置使用`stackalloc`。 該函式成員傳回時，會自動捨棄函式成員的執行期間建立的所有堆疊配置的記憶體區塊。 這會對應至`alloca`函式，C 中經常發現的延伸模組和C++實作。

在範例

```csharp
using System;

class Test
{
    static string IntToString(int value) {
        int n = value >= 0? value: -value;
        unsafe {
            char* buffer = stackalloc char[16];
            char* p = buffer + 16;
            do {
                *--p = (char)(n % 10 + '0');
                n /= 10;
            } while (n != 0);
            if (value < 0) *--p = '-';
            return new string(p, 0, (int)(buffer + 16 - p));
        }
    }

    static void Main() {
        Console.WriteLine(IntToString(12345));
        Console.WriteLine(IntToString(-999));
    }
}
```

`stackalloc`初始設定式會在`IntToString`方法來配置堆疊上的 16 個字元的緩衝區。 方法傳回時，緩衝區會自動被捨棄。

## <a name="dynamic-memory-allocation"></a>動態記憶體配置

除了`stackalloc`運算子，C# 提供任何預先定義的建構來管理非記憶體回收回收的記憶體。 這類服務通常會提供支援類別庫，或直接從基礎作業系統匯入。 比方說，`Memory`下列類別示範從 C# 存取堆積函式的基礎作業系統可能方式：

```csharp
using System;
using System.Runtime.InteropServices;

public unsafe class Memory
{
    // Handle for the process heap. This handle is used in all calls to the
    // HeapXXX APIs in the methods below.
    static int ph = GetProcessHeap();

    // Private instance constructor to prevent instantiation.
    private Memory() {}

    // Allocates a memory block of the given size. The allocated memory is
    // automatically initialized to zero.
    public static void* Alloc(int size) {
        void* result = HeapAlloc(ph, HEAP_ZERO_MEMORY, size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Copies count bytes from src to dst. The source and destination
    // blocks are permitted to overlap.
    public static void Copy(void* src, void* dst, int count) {
        byte* ps = (byte*)src;
        byte* pd = (byte*)dst;
        if (ps > pd) {
            for (; count != 0; count--) *pd++ = *ps++;
        }
        else if (ps < pd) {
            for (ps += count, pd += count; count != 0; count--) *--pd = *--ps;
        }
    }

    // Frees a memory block.
    public static void Free(void* block) {
        if (!HeapFree(ph, 0, block)) throw new InvalidOperationException();
    }

    // Re-allocates a memory block. If the reallocation request is for a
    // larger size, the additional region of memory is automatically
    // initialized to zero.
    public static void* ReAlloc(void* block, int size) {
        void* result = HeapReAlloc(ph, HEAP_ZERO_MEMORY, block, size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Returns the size of a memory block.
    public static int SizeOf(void* block) {
        int result = HeapSize(ph, 0, block);
        if (result == -1) throw new InvalidOperationException();
        return result;
    }

    // Heap API flags
    const int HEAP_ZERO_MEMORY = 0x00000008;

    // Heap API functions
    [DllImport("kernel32")]
    static extern int GetProcessHeap();

    [DllImport("kernel32")]
    static extern void* HeapAlloc(int hHeap, int flags, int size);

    [DllImport("kernel32")]
    static extern bool HeapFree(int hHeap, int flags, void* block);

    [DllImport("kernel32")]
    static extern void* HeapReAlloc(int hHeap, int flags, void* block, int size);

    [DllImport("kernel32")]
    static extern int HeapSize(int hHeap, int flags, void* block);
}
```

範例會使用`Memory`類別如下：

```csharp
class Test
{
    static void Main() {
        unsafe {
            byte* buffer = (byte*)Memory.Alloc(256);
            try {
                for (int i = 0; i < 256; i++) buffer[i] = (byte)i;
                byte[] array = new byte[256];
                fixed (byte* p = array) Memory.Copy(buffer, p, 256); 
            }
            finally {
                Memory.Free(buffer);
            }
            for (int i = 0; i < 256; i++) Console.WriteLine(array[i]);
        }
    }
}
```

此範例會配置 256 個位元組的記憶體，透過`Memory.Alloc`並增加從 0 到 255 之間的值初始化的記憶體區塊。 然後會配置 256 元素位元組陣列，並使用`Memory.Copy`複製位元組陣列的記憶體區塊的內容。 最後，使用釋放記憶體區塊`Memory.Free`和位元組陣列的內容會在主控台上的輸出。
