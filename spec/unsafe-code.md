---
ms.openlocfilehash: dbea611280a644adc25247b9887986e129c59b68
ms.sourcegitcommit: a5e393b018b04dfa55aae0000290ca087b508495
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/14/2019
ms.locfileid: "72310367"
---
# <a name="unsafe-code"></a>Unsafe 程式碼

如先前C#章節中所定義的核心語言，與 C 和C++將指標省略為資料類型的方式不同。 相反地C# ，會提供參考以及建立由垃圾收集行程管理之物件的能力。 這種設計與其他功能結合，可C#提供比 C 或C++更安全的語言。 在核心C#語言中，只是不可能有未初始化的變數、「無關聯的」指標，或是索引陣列超出其界限的運算式。 因此，災禍 C 和C++程式的整個錯誤類別會被排除。

實際上，每個指標類型在 C 中C++的結構，或的引用C#類型都是，不過在某些情況下，指標類型的存取會是必要的。 例如，與基礎作業系統互動、存取記憶體對應的裝置，或執行時間緊迫的演算法，在不需要存取指標的情況下，可能不可行或不切實際。 為了滿足此需求， C#提供了撰寫 unsafe 程式***代碼***的功能。

在 unsafe 程式碼中，您可以宣告和操作指標、執行指標和整數類型之間的轉換、接受變數的位址等等。 就意義而言，撰寫 unsafe C#程式碼很像是在程式中撰寫 C 程式碼。

不安全的程式碼實際上是從開發人員和使用者的角度來看的「安全」功能。 不安全的程式碼必須以修飾詞清楚標示 `unsafe`，讓開發人員不會意外地使用不安全的功能，而且執行引擎可以確保不安全的程式碼無法在未受信任的環境中執行。

## <a name="unsafe-contexts"></a>Unsafe 內容

不安全的C#功能僅適用于 unsafe 內容。 不安全的內容是藉由在類型或成員的宣告中包含 `unsafe` 修飾詞，或藉由採用*unsafe_statement*而引進：

*  類別、結構、介面或委派的宣告可能包含 `unsafe` 修飾詞，在這種情況下，該類型宣告的整個文字範圍（包括類別、結構或介面的主體）會被視為不安全的內容。
*  欄位、方法、屬性、事件、索引子、運算子、實例的程式化、析構函數或靜態的函式的宣告可能包含 @no__t 0 修飾詞，在這種情況下，該成員宣告的整個文字範圍會被視為不安全的內容。
*  *Unsafe_statement*可讓您在*區塊*內使用不安全的內容。 相關聯*區塊*的整個文字範圍會被視為不安全的內容。

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

在範例中

```csharp
public unsafe struct Node
{
    public int Value;
    public Node* Left;
    public Node* Right;
}
```

結構宣告中指定的 `unsafe` 修飾詞會導致結構宣告的整個文字範圍成為不安全的內容。 因此，您可以將 `Left` 和 @no__t 1 欄位宣告為指標類型。 也可以撰寫上述範例

```csharp
public struct Node
{
    public int Value;
    public unsafe Node* Left;
    public unsafe Node* Right;
}
```

在這裡，欄位宣告中的 `unsafe` 修飾詞會導致這些宣告被視為不安全的內容。

除了建立不安全的內容，因此允許使用指標類型，`unsafe` 修飾詞對類型或成員不會有任何影響。 在範例中

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

`A` 中 `F` 方法上的 `unsafe` 修飾詞，只會使 `F` 的文字範圍成為不安全的內容，而不會使用語言的 unsafe 功能。 在 `B` 的 `F` 覆寫中，不需要重新指定 `unsafe` 修飾詞，除非在 `B` 中的 `F` 方法本身需要存取不安全的功能。

當指標類型是方法簽章的一部分時，這種情況會略有不同

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

在這裡，因為 `F` 的簽章包含指標類型，所以只能在不安全的內容中寫入。 不過，不安全的內容可以藉由讓整個類別不安全（如 `A` 中的情況），或在方法宣告中包含 `unsafe` 修飾詞來引進，如同 `B` 中的情況。

## <a name="pointer-types"></a>指標類型

在不安全的內容中，*類型*（[類型](types.md)）可能是*pointer_type* ，也可以是*value_type*或*reference_type*。 不過， *pointer_type*也可用於不安全內容外的 @no__t 1 運算式（[匿名物件建立運算式](expressions.md#anonymous-object-creation-expressions)），因為這種用法並不安全。

```antlr
type_unsafe
    : pointer_type
    ;
```

*Pointer_type*會寫成*unmanaged_type*或關鍵字 `void`，後面接著 `*` token：

```antlr
pointer_type
    : unmanaged_type '*'
    | 'void' '*'
    ;

unmanaged_type
    : type
    ;
```

指標類型中 `*` 之前指定的類型，稱為指標類型的***參考型別***。 它代表指標類型的值所指向的變數類型。

不同于參考（參考型別的值），垃圾收集行程不會追蹤指標，垃圾收集行程並不知道指標和其指向的資料。 因此，指標不允許指向參考或包含參考的結構，而且指標的參考型別必須是*unmanaged_type*。

*Unmanaged_type*是不是*reference_type*或結構化類型的任何類型，而且不包含任何嵌套層級的*reference_type*或結構化類型欄位。 換句話說， *unmanaged_type*是下列其中一項：

*  `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、0、1 或 2。
*  任何*enum_type*。
*  任何*pointer_type*。
*  任何不是結構化型別且僅包含*unmanaged_type*之欄位的使用者定義*struct_type* 。

混合使用指標和參考的直覺規則是，參考（物件）的物件可以包含指標，但不允許指標的物件包含參考。

下表提供指標類型的一些範例：

| __範例__ | __描述__                               |
|-------------|-----------------------------------------------|
| `byte*`     | @No__t 的指標-0                             |
| `char*`     | @No__t 的指標-0                             |
| `int**`     | 指標指向 `int`                   |
| `int*[]`    | @No__t-0 的一維指標陣列 |
| `void*`     | 未知類型的指標                       |

若為指定的執行，所有指標類型都必須具有相同的大小和表示。

不同于 C C++和，在相同宣告中宣告多個指標時， C#在中，`*` 會與基礎類型一併寫入，而不是在每個指標名稱上當做前置詞標點符號。 例如：

```csharp
int* pi, pj;    // NOT as int *pi, *pj;
```

類型為 `T*` 的指標值表示 `T` 類型的變數位址。 指標間接運算子 `*` （[指標間接](unsafe-code.md#pointer-indirection)取值）可用來存取這個變數。 例如，假設 `int*` 類型的變數 `P`，則運算式 `*P` 表示在 `P` 中所包含的位址上找到的 `int` 變數。

就像物件參考一樣，指標可能會 `null`。 將間接運算子套用至 @no__t 0 指標會導致實作為定義的行為。 值為 `null` 的指標是以全位-零表示。

@No__t-0 類型代表未知類型的指標。 因為參考型別未知，所以間接運算子不能套用至類型 `void*` 的指標，也不能對這類指標執行任何算數運算。 不過，類型 `void*` 的指標可以轉換成任何其他指標類型（反之亦然）。

指標類型是不同類別的類型。 不同于參考型別和實數值型別，指標類型不會繼承自 `object`，而且指標類型和 `object` 之間不會有任何轉換。 特別的是，指標不支援裝箱和取消裝箱（[裝箱和取消](types.md#boxing-and-unboxing)裝箱）。 不過，在不同的指標類型之間，以及指標類型與整數類型之間允許轉換。 這會在[指標轉換](unsafe-code.md#pointer-conversions)中說明。

*Pointer_type*不能當做類型引數（結構化[類型](types.md#constructed-types)）使用，而且類型推斷（[類型推斷](expressions.md#type-inference)）會在泛型方法呼叫上失敗，而此呼叫會將類型引數推斷為指標類型。

*Pointer_type*可以當做 volatile 欄位（[volatile 欄位](classes.md#volatile-fields)）的類型使用。

雖然指標可以做為 `ref` 或 @no__t 1 參數傳遞，但這麼做可能會造成未定義的行為，因為指標可能會設定為指向在呼叫的方法傳回時不再存在的區域變數，或其用來指向的固定物件。不再是固定的。 例如:

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

方法可以傳回某種類型的值，而該類型可以是指標。 例如，當給定的連續 `int` 的序列、該序列的元素計數，以及其他 `int` 值的指標時，如果發生相符，下列方法會傳回該序列中該值的位址。否則，它會傳回 `null`：

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

在不安全的內容中，有數個結構可用於操作指標：

*  @No__t-0 運算子可用來執行指標間接取值（[指標間接](unsafe-code.md#pointer-indirection)取值）。
*  @No__t-0 運算子可用來透過指標（[指標成員存取](unsafe-code.md#pointer-member-access)）來存取結構的成員。
*  @No__t-0 運算子可用來為指標（[指標元素存取](unsafe-code.md#pointer-element-access)）編制索引。
*  @No__t-0 運算子可用來取得變數的位址（[即位址運算子](unsafe-code.md#the-address-of-operator)）。
*  @No__t-0 和 @no__t 1 運算子可用來遞增和遞減指標（[指標遞增和遞減](unsafe-code.md#pointer-increment-and-decrement)）。
*  @No__t-0 和 @no__t 1 運算子可用來執行指標算術（[指標算術](unsafe-code.md#pointer-arithmetic)）。
*  @No__t-0、`!=`、@no__t 2、`>`、`<=` 和 @no__t 5 運算子可用來比較指標（[指標比較](unsafe-code.md#pointer-comparison)）。
*  @No__t-0 運算子可用來從呼叫堆疊配置記憶體（[固定大小緩衝區](unsafe-code.md#fixed-size-buffers)）。
*  您可以使用 `fixed` 語句來暫時修正變數，以便取得其位址（[fixed 語句](unsafe-code.md#the-fixed-statement)）。

## <a name="fixed-and-moveable-variables"></a>固定和可移動變數

Address 運算子（「位址」[運算子](unsafe-code.md#the-address-of-operator)）和 @no__t 1 語句（[fixed 語句](unsafe-code.md#the-fixed-statement)）會將變數分成兩個類別：已***修正變數***和***可移動的變數***。

固定的變數位於不受垃圾收集行程操作影響的儲存位置。 （固定變數的範例包括區域變數、值參數，以及透過引用指標所建立的變數）。另一方面，可移動變數位於可能會由垃圾收集行程重新配置或處置的儲存位置中。 （可移動變數的範例包括物件中的欄位和陣列的元素）。

@No__t-0 運算子（[通訊運算子](unsafe-code.md#the-address-of-operator)）允許取得固定變數的位址，而不受限制。 不過，因為可移動變數可能會由垃圾收集行程重新配置或處置，所以可移動變數的位址只能使用 `fixed` 語句（[fixed 語句](unsafe-code.md#the-fixed-statement)）取得，而且該位址只會針對@no__t 2 語句的持續時間。

具體而言，固定的變數是下列其中一項：

*  由參考區域變數或值參數的*simple_name* （[簡單名稱](expressions.md#simple-names)）所產生的變數，除非該變數是由匿名函數所捕捉。
*  由 `V.I` 格式的*member_access* （[成員存取](expressions.md#member-access)）所產生的變數，其中 `V` 是*struct_type*的固定變數。
*  從*pointer_indirection_expression* （[指標間接](unsafe-code.md#pointer-indirection)取值）形成的變數，其格式為 `*P`、格式為 `P->I` 的*pointer_member_access* （[指標成員存取](unsafe-code.md#pointer-member-access)），或*pointer_element_access* （[指標元素存取](unsafe-code.md#pointer-element-access)）的格式為 `P[E]`。

所有其他變數都會分類為可移動變數。

請注意，靜態欄位會分類為可移動變數。 另請注意，即使為參數提供的引數是固定的變數，@no__t 0 或 @no__t 1 參數也會分類為可移動變數。 最後，請注意，透過取值指標所產生的變數一律會分類為固定的變數。

## <a name="pointer-conversions"></a>指標轉換

在不安全的內容中，可用的隱含轉換集合（[隱含轉換](conversions.md#implicit-conversions)）會擴充以包含下列隱含指標轉換：

*  從任何*pointer_type*到類型 `void*`。
*  從 `null` 常值到任何*pointer_type*。

此外，在不安全的內容中，可用的明確轉換（[明確轉換](conversions.md#explicit-conversions)）集合會擴充以包含下列明確指標轉換：

*  從任何*pointer_type*到任何其他*pointer_type*。
*  從 `sbyte`，`byte`，`short`，`ushort`，`int`，`uint`，`long`，或 `ulong` 到任何*pointer_type*。
*  從任何*pointer_type*到 `sbyte`，`byte`，`short`，`ushort`，`int`，`uint`，`long` 或 `ulong`。

最後，在不安全的內容中，標準隱含轉換（[標準隱含轉換](conversions.md#standard-implicit-conversions)）的集合包含下列指標轉換：

*  從任何*pointer_type*到類型 `void*`。

兩個指標類型之間的轉換永遠不會變更實際的指標值。 換句話說，從一個指標類型轉換成另一個，並不會影響指標所提供的基礎位址。

當其中一個指標類型轉換成另一個時，如果產生的指標未正確對齊所指的類型，則如果已取值，則行為會是未定義的。 一般來說，「正確對齊」這個概念是可轉移的：如果類型的指標 `A` 已正確對齊型別的指標 `B`，而後者則是正確對齊型別 `C` 的指標，則 `A` 類型的指標會正確對齊類型的指標 `C`。

請考慮下列情況，其中具有一個類型的變數是透過不同類型的指標來存取：

```csharp
char c = 'A';
char* pc = &c;
void* pv = pc;
int* pi = (int*)pv;
int i = *pi;         // undefined
*pi = 123456;        // undefined
```

當指標類型轉換成 byte 的指標時，結果會指向變數的最低定址位元組。 連續遞增的結果（最大為變數的大小）會產生該變數剩餘位元組的指標。 例如，下列方法會將 double 中的每個位元組顯示為十六進位值：

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

當然，產生的輸出取決於 endian。

指標和整數之間的對應是執行定義的。 不過，在具有線性位址空間的 32 * 和64位 CPU 架構上，從整數類資料類型來回轉換的行為，通常會與這些整數類型之間的 `uint` 或 @no__t 1 值的轉換完全相同。

### <a name="pointer-arrays"></a>指標陣列

在不安全的內容中，可以構造指標陣列。 只有一些適用于其他陣列類型的轉換可以在指標陣列上使用：

*  隱含參考轉換（[隱含參考](conversions.md#implicit-reference-conversions)轉換）從任何*array_type*到 `System.Array`，而它所執行的介面也適用于指標陣列。 不過，嘗試透過 `System.Array` 或其所執行的介面來存取陣列專案，將會在執行時間導致例外狀況，因為指標類型無法轉換成 `object`。
*  從一維陣列類型的隱含和明確參考轉換（[隱含參考轉換](conversions.md#implicit-reference-conversions)、[明確參考](conversions.md#explicit-reference-conversions)轉換） `S[]` 到 `System.Collections.Generic.IList<T>`，而且其泛型基底介面永遠不會套用至指標陣列，由於指標類型不能當做類型引數使用，而且不會從指標類型轉換成非指標類型。
*  從 `System.Array` 的明確參考轉換（[明確參考轉換](conversions.md#explicit-reference-conversions)）以及它對任何*array_type*所實作用的介面，都會套用至指標陣列。
*  從 `System.Collections.Generic.IList<S>` 及其基底介面到一維陣列類型的明確參考轉換（[明確參考轉換](conversions.md#explicit-reference-conversions)） `T[]` 永遠不會套用至指標陣列，因為指標類型不能當做類型引數使用，而且有不會從指標類型轉換成非指標類型。

這些限制表示在[foreach 語句](statements.md#the-foreach-statement)中所描述的陣列上，@no__t 0 語句的展開無法套用至指標陣列。 相反地，表單的 foreach 語句

```csharp
foreach (V v in x) embedded_statement
```

其中 `x` 的類型是格式為 `T[,,...,]` 的陣列類型，而 `N` 是維度的數目減1，而 `T` 或 `V` 是指標類型，則會使用 nested for-迴圈展開，如下所示：

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

@No__t-0、`i0`、`i1`、...、`iN` 的變數無法在 `x` 或*embedded_statement*或程式的任何其他原始程式碼中看見或存取。 變數 `v` 在內嵌語句中是唯讀的。 如果沒有從 `T` （元素類型）到 `V` 的明確轉換（[指標轉換](unsafe-code.md#pointer-conversions)），就會產生錯誤，而且不會採取任何進一步的步驟。 如果 `x` 的值 `null`，則會在執行時間擲回 `System.NullReferenceException`。

## <a name="pointers-in-expressions"></a>運算式中的指標

在不安全的內容中，運算式可能會產生指標類型的結果，但在不安全的內容之外，運算式必須是指標類型才會發生編譯時期錯誤。 具體而言，在不安全的內容外部，如果有任何*simple_name* （[簡單名稱](expressions.md#simple-names)）、 *member_access* （[成員存取](expressions.md#member-access)）、 *invocation_expression* （[調用運算式](expressions.md#invocation-expressions)）或 *，就會發生編譯時期錯誤。element_access* （[元素存取](expressions.md#element-access)）是指標類型。

在不安全的內容中， *primary_no_array_creation_expression* （[主要運算式](expressions.md#primary-expressions)）和*unary_expression* （[一元運算子](expressions.md#unary-operators)）的生產會允許下列其他的結構：

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

下列各節將說明這些結構。 文法會隱含 unsafe 運算子的優先順序和關聯性。

### <a name="pointer-indirection"></a>指標間接取值

*Pointer_indirection_expression*包含星號（`*`），後面接著*unary_expression*。

```antlr
pointer_indirection_expression
    : '*' unary_expression
    ;
```

一元 `*` 運算子代表指標間接取值，用來取得指標指向的變數。 評估 `*P` 的結果，其中 `P` 是指標類型的運算式 `T*`，是 `T` 類型的變數。 將一元 `*` 運算子套用至類型 `void*` 的運算式或不是指標類型的運算式時，就會發生編譯時期錯誤。

將一元 `*` 運算子套用至 @no__t 1 指標的效果是由實作為定義的。 特別是，不保證此作業會擲回 `System.NullReferenceException`。

如果將不正確值指派給指標，則不會定義一元 `*` 運算子的行為。 在一元 `*` 運算子取值指標的無效值中，會有一個位址不適當地對應到所指向的類型（請參閱[指標轉換](unsafe-code.md#pointer-conversions)中的範例），以及其存留期結束後的變數位址。

基於明確指派分析的目的，評估格式為 `*P` 的運算式所產生的變數會被視為一開始指派（[最初指派的變數](variables.md#initially-assigned-variables)）。

### <a name="pointer-member-access"></a>指標成員存取

*Pointer_member_access*是由*primary_expression*所組成，後面接著 "`->`" 權杖，後面接著一個*識別碼*和一個選擇性的*type_argument_list*。

```antlr
pointer_member_access
    : primary_expression '->' identifier
    ;
```

在表單 `P->I` 的指標成員存取中，`P` 必須是 `void*` 以外指標類型的運算式，而且 `I` 必須代表 `P` 點之類型的可存取成員。

@No__t-0 格式的指標成員存取，會完全依照 `(*P).I` 進行評估。 如需指標間接運算子（`*`）的說明，請參閱[指標間接](unsafe-code.md#pointer-indirection)取值。 如需成員存取運算子（`.`）的說明，請參閱[成員存取](expressions.md#member-access)。

在範例中

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

`->` 運算子是用來存取欄位，並透過指標叫用結構的方法。 因為 `P->I` 的作業會精確地等同于 `(*P).I`，所以已撰寫 @no__t 2 方法：

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

*Pointer_element_access*包含*primary_no_array_creation_expression* ，後面接著以 "`[`" 和 "`]`" 括住的運算式。

```antlr
pointer_element_access
    : primary_no_array_creation_expression '[' expression ']'
    ;
```

在 `P[E]` 格式的指標專案存取中，`P` 必須是 `void*` 以外指標類型的運算式，而且 `E` 必須是可以隱含轉換成 `int`、`uint`、`long` 或 `ulong` 的運算式。

@No__t-0 格式的指標專案存取，會完全依照 `*(P + E)` 進行評估。 如需指標間接運算子（`*`）的說明，請參閱[指標間接](unsafe-code.md#pointer-indirection)取值。 如需指標加號運算子（`+`）的說明，請參閱[指標算術](unsafe-code.md#pointer-arithmetic)。

在範例中

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

指標專案存取是用來初始化 `for` 迴圈中的字元緩衝區。 由於作業 `P[E]` 完全等同于 `*(P + E)`，因此，此範例也可能已撰寫：

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

指標元素存取運算子不會檢查超出範圍的錯誤，而且不會定義存取超出範圍的元素時的行為。 這與 C 和C++相同。

### <a name="the-address-of-operator"></a>Address 運算子

*Addressof_expression*是由後面接著*unary_expression*的連字號（`&`）所組成。

```antlr
addressof_expression
    : '&' unary_expression
    ;
```

假設有一個運算式 `E`，這種類型 `T`，並分類為固定變數（[固定和可移動的變數](unsafe-code.md#fixed-and-moveable-variables)），則結構 `&E` 會計算 `E` 所提供的變數位址。 結果的類型為 `T*`，並分類為值。 如果 `E` 分類為唯讀區域變數，或如果 `E` 表示可移動的變數，則會發生編譯時期錯誤，如果 `E` 未分類為變數。 在最後一個案例中，固定的語句（[fixed 語句](unsafe-code.md#the-fixed-statement)）可以用來暫時「修正」變數，再取得其位址。 如[成員存取](expressions.md#member-access)中所述，在定義 @no__t 1 欄位的結構或類別之外，在實例的參數或靜態的函式以外，該欄位會被視為值，而不是變數。 因此，無法取得其位址。 同樣地，也無法取得常數的位址。

@No__t-0 運算子不需要明確指派其引數，但在 `&` 作業之後，套用運算子的變數會被視為在作業發生所在的執行路徑中明確指派。 程式設計人員必須負責確保在此情況下，確實會進行正確的變數初始化。

在範例中

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

`i` 會被視為依照用來初始化 `p` 的 `&i` 作業進行明確指派。 作用中 `*p` 的指派會初始化 `i`，但是加入此初始化是程式設計人員的責任，如果移除指派，則不會發生編譯時期錯誤。

@No__t-0 運算子的明確指派規則存在，因此可以避免區域變數的重複初始化。 例如，許多外部 Api 都會採用 API 所填入之結構的指標。 對這類 Api 的呼叫通常會傳遞本機結構變數的位址，而且若沒有此規則，就需要重複的結構變數初始化。

### <a name="pointer-increment-and-decrement"></a>指標遞增和遞減

在不安全的內容中，`++` 和 @no__t 1 運算子（後置[遞增和遞減運算子](expressions.md#postfix-increment-and-decrement-operators)和[前置遞增和遞減運算子](expressions.md#prefix-increment-and-decrement-operators)）可以套用至所有類型的指標變數，但不包括 `void*`。 因此，對於每個指標類型 `T*`，會隱含地定義下列運算子：

```csharp
T* operator ++(T* x);
T* operator --(T* x);
```

運算子會分別產生與 `x + 1` 和 `x - 1` （[指標算術](unsafe-code.md#pointer-arithmetic)）相同的結果。 換句話說，針對類型 `T*` 的指標變數，`++` 運算子會將 `sizeof(T)` 新增至變數中包含的位址，而 `--` 運算子會從變數中包含的位址減去 `sizeof(T)`。

如果指標遞增或遞減運算溢出指標類型的定義域，則結果會是實作為定義，但是不會產生任何例外狀況。

### <a name="pointer-arithmetic"></a>指標算術

在不安全的內容中，`+` 和 @no__t 1 運算子（[加法運算子](expressions.md#addition-operator)和[減法運算子](expressions.md#subtraction-operator)）可以套用至所有指標類型的值，但 `void*` 除外。 因此，對於每個指標類型 `T*`，會隱含地定義下列運算子：

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

假設有一個運算式 `P` 的指標型別 `T*` 和一個運算式 @no__t `int`，`uint`，`long` 或 `ulong`，則運算式 `P + N` 和 `N + P` 會計算 `T*` 類型的指標值，而結果是將 @no__t 新增至位址提供者為 1。 同樣地，運算式 `P - N` 會計算 `T*` 類型的指標值，這是從 `P` 所指定的位址減去 `N * sizeof(T)` 所產生的。

假設有兩個運算式（`P` 和 `Q`）的指標類型 `T*`，運算式 `P - Q` 會計算 `P` 和 `Q` 所指定的位址之間的差異，然後將該差異除以 `sizeof(T)`。 結果的類型一律為 `long`。 實際上，`P - Q` 會計算為 `((long)(P) - (long)(Q)) / sizeof(T)`。

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

這會產生輸出：

```console
p - q = -14
q - p = 14
```

如果指標算數運算溢出指標類型的定義域，則會以實作為定義的方式截斷結果，但不會產生任何例外狀況。

### <a name="pointer-comparison"></a>指標比較

在不安全的內容中，`==`、`!=`、`<`、`>`、`<=` 和 @no__t 5 運算子（[關聯式和類型測試運算子](expressions.md#relational-and-type-testing-operators)）可以套用至所有指標類型的值。 指標比較運算子如下：

```csharp
bool operator ==(void* x, void* y);
bool operator !=(void* x, void* y);
bool operator <(void* x, void* y);
bool operator >(void* x, void* y);
bool operator <=(void* x, void* y);
bool operator >=(void* x, void* y);
```

因為從任何指標類型到 `void*` 類型都有隱含的轉換，所以可以使用這些運算子來比較任何指標類型的運算元。 比較運算子會比較兩個運算元所指定的位址，如同它們是不帶正負號的整數。

### <a name="the-sizeof-operator"></a>Sizeof 運算子

`sizeof` 運算子會返回指定型別變數所佔用的位元組總數。 指定為 `sizeof` 之運算元的類型必須是*unmanaged_type* （[指標類型](unsafe-code.md#pointer-types)）。

```antlr
sizeof_expression
    : 'sizeof' '(' unmanaged_type ')'
    ;
```

@No__t-0 運算子的結果是 `int` 類型的值。 針對某些預先定義的類型，`sizeof` 運算子會產生常數值，如下表所示。


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

對於所有其他類型，`sizeof` 運算子的結果會定義為實作為值，而不是常數。

成員封裝到結構中的順序未指定。

基於對齊目的，結構的開頭可能會有未命名的填補、在結構中，以及在結構的結尾處。 用來做為填補的位內容是不確定的。

套用至具有結構類型的運算元時，結果會是該類型變數中的總位元組數，包括任何填補。

## <a name="the-fixed-statement"></a>Fixed 語句

在不安全的內容中， *embedded_statement* （[語句](statements.md)）生產會允許額外的結構，也就是 `fixed` 語句，這是用來「修正」可移動變數，使其位址在語句的持續時間內保持不變.

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

每個*fixed_pointer_declarator*都會宣告給定*pointer_type*的區域變數，並使用對應的*fixed_pointer_initializer*所計算的位址來初始化該本機變數。 在 `fixed` 語句中宣告的區域變數，可以在該變數宣告的右邊發生的任何*fixed_pointer_initializer*中，以及在 @no__t 3 語句的*embedded_statement*中存取。 由 @no__t 0 的語句所宣告的區域變數會被視為唯讀。 如果內嵌語句嘗試修改這個本機變數（透過指派或 `++` 和 @no__t 1 運算子），或將它當做 `ref` 或 `out` 參數傳遞，就會發生編譯時期錯誤。

*Fixed_pointer_initializer*可以是下列其中一項：

*  Token "`&`" 後面接著*variable_reference* （[判斷明確指派的精確規則](variables.md#precise-rules-for-determining-definite-assignment)） `T` 的非受控類型的可移動變數（[固定和可移動變數](unsafe-code.md#fixed-and-moveable-variables)），前提是 `T*` 的類型為可隱含轉換為 `fixed` 語句中提供的指標類型。 在此情況下，初始化運算式會計算給定變數的位址，並保證在 `fixed` 語句期間，變數會保留在固定位址。
*  *Array_type*的運算式，其中包含非受控類型的專案 `T`，前提是類型 `T*` 可以隱含地轉換成 `fixed` 語句中提供的指標類型。 在此情況下，初始化運算式會計算陣列中第一個元素的位址，而整個陣列保證會在 `fixed` 語句期間保持固定位址。 如果陣列運算式為 null，或者陣列有零個元素，則初始化運算式會將位址計算為等於零。
*  類型為 `string` 的運算式，但前提是類型 `char*` 可以隱含地轉換成 `fixed` 語句中提供的指標類型。 在此情況下，初始化運算式會計算字串中第一個字元的位址，而在 `fixed` 語句期間，整個字串保證會保留在固定位址。 如果字串運算式為 null，則 `fixed` 語句的行為會定義為「執行中」。
*  *Simple_name*或*member_access* ，參考可移動變數的固定大小緩衝區成員，前提是固定大小緩衝區成員的類型可以隱含地轉換成在 `fixed` 語句中指定的指標類型。 在此情況下，初始化運算式會計算固定大小緩衝區之第一個元素的指標（[運算式中的固定大小緩衝區](unsafe-code.md#fixed-size-buffers-in-expressions)），而固定大小的緩衝區保證會在 `fixed` 語句期間保留在固定位址。

對於*fixed_pointer_initializer*所計算的每個位址，`fixed` 語句可確保在 @no__t 2 語句期間，垃圾收集行程不會重新配置或處置位址所參考的變數。 例如，如果*fixed_pointer_initializer*所計算的位址參考了物件的欄位或陣列實例的元素，則 @no__t 1 語句可保證包含的物件實例在執行期間不會重新置放或處置語句的存留期。

程式設計人員必須負責確保 @no__t 0 語句所建立的指標不會在執行這些語句之後存留下來。 例如，當由 @no__t 0 的語句所建立的指標傳遞給外部 Api 時，程式設計人員必須負責確保 Api 不會保留這些指標的任何記憶體。

固定物件可能會造成堆積的片段（因為無法移動）。 基於這個理由，只有在絕對必要時才應該修正物件，而且只會在最短的時間內完成。

範例

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

示範 `fixed` 語句的數種用法。 第一個語句會修正並取得靜態欄位的位址，第二個語句會修正並取得實例欄位的位址，而第三個語句會修正並取得陣列元素的位址。 在每個案例中，使用一般的 `&` 運算子就會發生錯誤，因為變數全都分類為可移動變數。

上述範例中的第四個 `fixed` 語句，會產生與第三個類似的結果。

這個 `fixed` 語句的範例會使用 `string`：

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

在一維陣列的 unsafe 內容陣列元素中，會以遞增的索引順序儲存，從索引 `0` 開始，並以索引 `Length - 1` 結束。 若為多維陣列，會儲存陣列元素，以便先增加最右邊維度的索引，然後再將下一個左維度放在左側。 在 `fixed` 語句中，取得 `p` 的指標至陣列實例 `a`，指標值範圍從 `p` 到 `p + a.Length - 1` 表示陣列中元素的位址。 同樣地，範圍從 `p[0]` 到 `p[a.Length - 1]` 的變數則代表實際的陣列元素。 假設陣列的儲存方式，我們可以將任何維度的陣列視為線性。

例如:

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

這會產生輸出：

```console
[0,0,0] =  0 [0,0,1] =  1 [0,0,2] =  2 [0,0,3] =  3
[0,1,0] =  4 [0,1,1] =  5 [0,1,2] =  6 [0,1,3] =  7
[0,2,0] =  8 [0,2,1] =  9 [0,2,2] = 10 [0,2,3] = 11
[1,0,0] = 12 [1,0,1] = 13 [1,0,2] = 14 [1,0,3] = 15
[1,1,0] = 16 [1,1,1] = 17 [1,1,2] = 18 [1,1,3] = 19
[1,2,0] = 20 [1,2,1] = 21 [1,2,2] = 22 [1,2,3] = 23
```

在範例中

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

`fixed` 語句是用來修正陣列，因此其位址可以傳遞至接受指標的方法。

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

fixed 語句用來修正結構的固定大小緩衝區，使其位址可以當做指標使用。

藉由修正字串實例所產生的 @no__t 0 值，一律會指向以 null 結束的字串。 在取得指標 `p` 到字串實例 `s` 的 fixed 語句中，指標值的範圍從 `p` 到 `p + s.Length - 1` 代表字串中的字元位址，而指標值 `p + s.Length` 一律指向 null 字元（值為 `'\0'`）的字元。

透過固定指標修改 managed 類型的物件，可能會導致未定義的行為。 例如，因為字串是不可變的，所以程式設計人員必須負責確保固定字串的指標所參考的字元不會修改。

呼叫需要 "C 樣式" 字串的外部 Api 時，自動 null 終止的字串會特別方便。 不過，請注意，字串實例允許包含 null 字元。 如果有這類 null 字元，當視為以 null 終止的 `char*` 時，字串會顯示為截斷。

## <a name="fixed-size-buffers"></a>固定大小的緩衝區

固定大小緩衝區是用來將「C 樣式」的內嵌陣列宣告為結構的成員，而且主要適用于與非受控 Api 互動。

### <a name="fixed-size-buffer-declarations"></a>固定大小的緩衝區宣告

***固定大小緩衝區***是一個成員，代表指定類型變數之固定長度緩衝區的儲存區。 固定大小的緩衝區宣告會引進一或多個指定元素類型的固定大小緩衝區。 只有在結構宣告中才允許固定大小緩衝區，而且只能在不安全的內容中發生（[不安全](unsafe-code.md#unsafe-contexts)的內容）。

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

固定大小的緩衝區宣告可能包括一組屬性（[屬性](attributes.md)）、一個 @no__t 的修飾[詞（修飾](classes.md#modifiers)詞）、四個存取修飾詞（[型別參數和條件約束](classes.md#type-parameters-and-constraints)）的有效組合，以及一個 `unsafe` 個修飾詞（[Unsafe內容）。](unsafe-code.md#unsafe-contexts) 屬性和修飾詞適用于固定大小緩衝區宣告所宣告的所有成員。 在固定大小的緩衝區宣告中多次出現相同的修飾詞時，就會發生錯誤。

固定大小的緩衝區宣告不允許包含 `static` 修飾詞。

固定大小緩衝區宣告的緩衝區元素類型會指定宣告所引進之緩衝區的元素類型。 Buffer 元素類型必須是其中一個預先定義的類型 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、0 或 1。

Buffer 元素類型後面接著固定大小的緩衝區宣告子清單，其中每個宣告子都會引進新的成員。 固定大小的緩衝區宣告子包含命名成員的識別碼，後面接著以 `[` 和 @no__t 1 標記括住的常數運算式。 常數運算式代表該固定大小緩衝區宣告子所引進之成員中的元素數目。 常數運算式的類型必須可以隱含地轉換成類型 `int`，而且值必須是非零的正整數。

固定大小緩衝區的元素一定會在記憶體中依序排列。

宣告多個固定大小緩衝區的固定大小緩衝區宣告相當於具有相同屬性和元素類型的單一固定大小緩衝區宣告的多個宣告。 例如：

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

### <a name="fixed-size-buffers-in-expressions"></a>運算式中的固定大小緩衝區

固定大小緩衝區成員的成員查閱（[運算子](expressions.md#operators)）會繼續與欄位的成員查閱相同。

您可以使用*simple_name* （[型別推斷](expressions.md#type-inference)）或*member_access* （動態多載[解析的編譯時間檢查](expressions.md#compile-time-checking-of-dynamic-overload-resolution)），在運算式中參考固定大小的緩衝區。

當固定大小的緩衝區成員當做簡單名稱來參考時，其效果會與表單 `this.I` 的成員存取相同，其中 `I` 是固定大小的緩衝區成員。

在表單 `E.I` 的成員存取中，如果 `E` 是結構類型，而且該結構類型中的成員查閱 `I` 識別固定大小成員，則會評估 `E.I`，其分類方式如下：

*  如果不安全的內容中出現運算式 `E.I`，就會發生編譯時期錯誤。
*  如果 `E` 分類為值，則會發生編譯時期錯誤。
*  否則，如果 `E` 是可移動變數（[固定和可移動的](unsafe-code.md#fixed-and-moveable-variables)變數），而且運算式 `E.I` 不是*fixed_pointer_initializer* （[fixed 語句](unsafe-code.md#the-fixed-statement)），就會發生編譯時期錯誤。
*  否則，`E` 會參考固定的變數，而運算式的結果會是在 `E` 中，`I` 之固定大小緩衝區成員的第一個元素的指標。 結果的類型為 `S*`，其中 `S` 是 `I` 的元素類型，且分類為值。

您可以使用第一個元素的指標作業來存取固定大小緩衝區的後續元素。 不同于陣列的存取權，對固定大小緩衝區的元素存取是不安全的作業，而且不會進行範圍檢查。

下列範例會宣告並使用具有固定大小緩衝區成員的結構。

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

### <a name="definite-assignment-checking"></a>明確的指派檢查

固定大小緩衝區不受限於明確的指派檢查（[明確指派](variables.md#definite-assignment)），而且會忽略固定大小的緩衝區成員，以用於結構類型變數的明確指派檢查。

當固定大小緩衝區成員的最外層包含結構變數為靜態變數、類別實例的執行個體變數或陣列元素時，固定大小緩衝區的元素會自動初始化為其預設值（預設值）[值](variables.md#default-values)）。 在所有其他情況下，固定大小緩衝區的初始內容是未定義的。

## <a name="stack-allocation"></a>堆疊配置

在不安全的內容中，本機變數宣告（[區域變數](statements.md#local-variable-declarations)宣告）可能包含從呼叫堆疊配置記憶體的堆疊配置初始化運算式。

```antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

*Unmanaged_type*會指出將儲存在新配置位置的專案類型，而*運算式*會指出這些專案的數目。 結合在一起，即可指定所需的配置大小。 因為堆疊配置的大小不能為負數，所以會發生編譯時期錯誤，將專案數目指定為評估為負值的*constant_expression* 。

格式為 `stackalloc T[E]` 的堆疊配置初始化運算式需要 `T` 為非受控類型（[指標類型](unsafe-code.md#pointer-types)），而 `E` 則為類型 `int` 的運算式。 結構會從呼叫堆疊配置 `E * sizeof(T)` 個位元組，並將類型 `T*` 的指標傳回至新配置的區塊。 如果 `E` 是負值，則行為是未定義的。 如果 `E` 為零，則不會進行任何配置，而傳回的指標會定義為執行。 如果沒有足夠的記憶體可配置指定大小的區塊，則會擲回 @no__t 0。

新配置記憶體的內容尚未被定義。

@No__t-0 或 @no__t 1 區塊（[try 語句](statements.md#the-try-statement)）中不允許堆疊配置初始化運算式。

沒有任何方法可以明確釋放使用 `stackalloc` 所配置的記憶體。 在函式成員執行期間建立的所有堆疊配置記憶體區塊，會在該函式成員傳回時自動捨棄。 這會對應到 `alloca` 函式，這是通常在 C 和C++實作為中找到的延伸模組。

在範例中

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

`IntToString` 方法中會使用 @no__t 0 初始化運算式，在堆疊上配置16個字元的緩衝區。 當方法傳回時，會自動捨棄緩衝區。

## <a name="dynamic-memory-allocation"></a>動態記憶體配置

除了 `stackalloc` 運算子以外， C#不會提供任何預先定義的結構來管理非垃圾收集的記憶體。 這類服務通常是透過支援類別庫來提供，或直接從基礎作業系統匯入。 例如，下列的 `Memory` 類別說明如何從下列來源C#存取基礎作業系統的堆積功能：

```csharp
using System;
using System.Runtime.InteropServices;

public static unsafe class Memory
{
    // Handle for the process heap. This handle is used in all calls to the
    // HeapXXX APIs in the methods below.
    private static readonly IntPtr s_heap = GetProcessHeap();

    // Allocates a memory block of the given size. The allocated memory is
    // automatically initialized to zero.
    public static void* Alloc(int size)
    {
        void* result = HeapAlloc(s_heap, HEAP_ZERO_MEMORY, (UIntPtr)size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Copies count bytes from src to dst. The source and destination
    // blocks are permitted to overlap.
    public static void Copy(void* src, void* dst, int count)
    {
        byte* ps = (byte*)src;
        byte* pd = (byte*)dst;
        if (ps > pd)
        {
            for (; count != 0; count--) *pd++ = *ps++;
        }
        else if (ps < pd)
        {
            for (ps += count, pd += count; count != 0; count--) *--pd = *--ps;
        }
    }

    // Frees a memory block.
    public static void Free(void* block)
    {
        if (!HeapFree(s_heap, 0, block)) throw new InvalidOperationException();
    }

    // Re-allocates a memory block. If the reallocation request is for a
    // larger size, the additional region of memory is automatically
    // initialized to zero.
    public static void* ReAlloc(void* block, int size)
    {
        void* result = HeapReAlloc(s_heap, HEAP_ZERO_MEMORY, block, (UIntPtr)size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Returns the size of a memory block.
    public static int SizeOf(void* block)
    {
        int result = (int)HeapSize(s_heap, 0, block);
        if (result == -1) throw new InvalidOperationException();
        return result;
    }

    // Heap API flags
    private const int HEAP_ZERO_MEMORY = 0x00000008;

    // Heap API functions
    [DllImport("kernel32")]
    private static extern IntPtr GetProcessHeap();

    [DllImport("kernel32")]
    private static extern void* HeapAlloc(IntPtr hHeap, int flags, UIntPtr size);

    [DllImport("kernel32")]
    private static extern bool HeapFree(IntPtr hHeap, int flags, void* block);

    [DllImport("kernel32")]
    private static extern void* HeapReAlloc(IntPtr hHeap, int flags, void* block, UIntPtr size);

    [DllImport("kernel32")]
    private static extern UIntPtr HeapSize(IntPtr hHeap, int flags, void* block);
}
```

以下提供使用 `Memory` 類別的範例：

```csharp
class Test
{
    static unsafe void Main()
    {
        byte* buffer = null;
        try
        {
            const int Size = 256;
            buffer = (byte*)Memory.Alloc(Size);
            for (int i = 0; i < Size; i++) buffer[i] = (byte)i;
            byte[] array = new byte[Size];
            fixed (byte* p = array) Memory.Copy(buffer, p, Size);
            for (int i = 0; i < Size; i++) Console.WriteLine(array[i]);
        }
        finally
        {
            if (buffer != null) Memory.Free(buffer);
        }
    }
}
```

此範例會透過 `Memory.Alloc` 配置256位元組的記憶體，並將值從0增加到255的記憶體區塊初始化。 然後，它會配置256元素位元組陣列，並使用 `Memory.Copy` 將記憶體區塊的內容複寫到位元組陣列。 最後，記憶體區塊會使用 `Memory.Free` 來釋放，而位元組陣列的內容則會在主控台上輸出。
