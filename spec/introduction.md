---
ms.openlocfilehash: db10046af5d635b430951679a448e23680b18b87
ms.sourcegitcommit: 4cc6d73a765ac9827ab00c48ad9f09204baf888f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/18/2019
ms.locfileid: "59426808"
---
# <a name="introduction"></a>簡介

C# (發音為 "See Sharp") 是簡單、物件導向、型別安全的現代化程式設計語言。 C#有源自於是 C 系列語言中，而且會立即熟悉 C， C++，和 Java 程式設計人員。 C# 由標準化為 ECMA International ***ECMA-334***標準，並由 ISO/IEC 作為***ISO/IEC 23270***標準。 Microsoft 的 C# 編譯器為.NET Framework 是符合這些標準實作。

C# 是物件導向的語言，但 C# 更進一步支援「元件導向」程式設計。 現代軟體設計逐漸依賴功能性上獨立與屬於自我描述套件的軟體元件。 這類元件的關鍵在於它們呈現含有屬性、方法和事件的程式設計模型；它們提供元件相關宣告資訊的屬性，且併入自己的文件。 C# 提供語言建構來直接支援這些概念，使 C# 中，以建立和使用軟體元件非常自然的語言。

以下幾個 C# 功能可協助建構強固且耐用的應用程式：***記憶體回收***自動回收未使用的物件; 所佔用的記憶體***例外狀況處理***提供錯誤偵測和復原; 的結構化和可延伸方法並***型別安全***語言設計，因此無法讀取未初始化的變數，要編製索引超過其範圍中，或執行未檢查的陣列類型轉換。

C# 有***統一的型別系統***。 所有的 C# 型別 (包括 `int` 和 `double` 等基本型別) 都繼承自單一的 `object` 根型別。 因此，所有型別都擁有相同的一組常見作業，且任何型別的值都能以相同方式儲存、傳送及操作。 此外，C# 支援使用者定義的參考型別和數值型別，因此能動態配置物件，以及內嵌儲存輕量結構。

若要確保，C# 程式和程式庫可以隨著時間以相容的方式，更強調已置於***版本控制***在 C# 的設計。 許多程式設計語言並不注重此問題，因此當推出新版的相依程式庫時，以那些語言撰寫的程式需要進行較多修改才能繼續運作。 已直接受到版本控制考量的 C# 設計層面包括個別`virtual`和`override`修飾詞，方法多載解析，規則支援明確介面成員宣告。

本章的其餘部分會說明 C# 語言的基本功能。 雖然之後的章節會說明規則和例外狀況詳細資料導向和數學的方式，但這一章致力為了清楚起見和求簡單明瞭，但會犧牲完整性。 目的是要提供讀者會加速早期的程式撰寫和後面的章節則讀取的語言簡介。

## <a name="hello-world"></a>Hello World

“Hello, World” 程式通常用來介紹程式設計語言。 以下是以 C# 撰寫的：

```csharp
using System;

class Hello
{
    static void Main() {
        Console.WriteLine("Hello, World");
    }
}
```

C# 原始程式檔的副檔名通常是 `.cs`。 假設"Hello，World"程式儲存在檔案`hello.cs`，可以使用命令列，Microsoft C# 編譯器編譯程式
```
csc hello.cs
```
這會產生名為可執行組件`hello.exe`。 執行時，此應用程式所產生的輸出
```
Hello, World
```

“Hello, World” 程式的開頭為 `using` 指示詞，會參考 `System` 命名空間。 命名空間提供組織 C# 程式和程式庫的階層式方法。 命名空間包含型別和其他命名空間，例如 `System` 命名空間包含數個型別 (如程式中參考的 `Console` 類別)，和數個其他命名空間 (如 `IO` 和 `Collections`)。 使用 `using` 指示詞參考指定的命名空間，就能以非限定的方式使用屬於該命名空間成員的型別。 因為 `using` 指示詞的緣故，該程式可以使用 `Console.WriteLine` 當作 `System.Console.WriteLine` 的縮寫。

“Hello, World” 程式宣告的 `Hello` 類別包含單一成員，即名為 `Main` 的方法。 `Main`方法以宣告`static`修飾詞。 執行個體方法可以使用關鍵字 `this` 參考特定的封入物件執行個體，但靜態方法卻不需要參考特定物件即可運作。 依照慣例，會使用名為 `Main` 的靜態方法做為程式的進入點。

程式的輸出是由 `System` 命名空間中 `Console` 類別的 `WriteLine` 方法產生。 這個類別會提供.NET Framework 類別庫，其中，根據預設，Microsoft C# 編譯器會自動參考。 請注意，C# 本身不需要個別執行階段程式庫。 相反地，.NET Framework 是執行階段程式庫的 C#。

## <a name="program-structure"></a>程式結構

C# 語言的重要組織概念如下：程式、命名空間、型別、成員及組件。 C# 程式包含一個或多個原始程式檔。 程式宣告型別，其中包含成員並可以依據命名空間分組。 類別和介面都是型別的範例。 欄位、方法、屬性及事件都是成員的範例。 在編譯 C# 語言時，會將它們實際封裝為組件。 組件通常具有副檔名`.exe`或`.dll`，這取決於它們實作是否***應用程式***或是***程式庫***。

此範例

```csharp
using System;

namespace Acme.Collections
{
    public class Stack
    {
        Entry top;

        public void Push(object data) {
            top = new Entry(top, data);
        }

        public object Pop() {
            if (top == null) throw new InvalidOperationException();
            object result = top.data;
            top = top.next;
            return result;
        }

        class Entry
        {
            public Entry next;
            public object data;
    
            public Entry(Entry next, object data) {
                this.next = next;
                this.data = data;
            }
        }
    }
}
```
宣告類別，名為`Stack`稱為命名空間中`Acme.Collections`。 此類別的完整名稱是 `Acme.Collections.Stack`。 該類別包含數個成員︰一個名為 `top` 的欄位、兩個名為 `Push` 和 `Pop` 的方法以及名為 `Entry` 的巢狀類別。 `Entry` 類別更包含三個成員︰一個名為 `next` 的欄位、一個名為 `data` 的欄位以及建構函式。 假設此範例的原始程式碼儲存在檔案 `acme.cs`，命令列

```
csc /t:library acme.cs
```
會將範例編譯為程式庫 (不需要 `Main` 進入點的程式碼)，並產生名為 `acme.dll` 的組件。

組件包含可執行的程式碼的形式***Intermediate Language*** (IL) 指令，而符號資訊採用的形式***中繼資料***。 執行前，組件中的 IL 程式碼會由 .NET 通用語言執行平台的 Just-In-Time (JIT) 編譯器自動轉換為處理器特定程式碼。

由於組件是包含程式碼和中繼資料的功能自我描述單位，所以不需要提供 C# 中的 `#include` 指示詞和標頭檔。 特定組件中所包含的公用型別和成員只能在編譯程式時，使用 C# 程式來參考該組件。 例如，此程式使用來自 `acme.dll` 組件的 `Acme.Collections.Stack` 類別︰

```csharp
using System;
using Acme.Collections;

class Test
{
    static void Main() {
        Stack s = new Stack();
        s.Push(1);
        s.Push(10);
        s.Push(100);
        Console.WriteLine(s.Pop());
        Console.WriteLine(s.Pop());
        Console.WriteLine(s.Pop());
    }
}
```
如果程式儲存在檔案`test.cs`，當`test.cs`編譯`acme.dll`可以使用編譯器的參考組件`/r`選項：

```
csc /r:acme.dll test.cs
```
這樣會建立名為 `test.exe` 可執行檔組件，執行時會產生以下輸出︰

```
100
10
1
```
C# 允許以數個原始程式檔儲存程式的原始程式文字。 編譯有多檔案的 C# 程式時，所有原始程式檔都會一起處理，而原始程式檔可以隨意地互相參考。概念上，就如同將所有原始程式檔都串連成一個大型檔案，然後再進行處理。 C# 中不再需要向前宣告，原因是在極少數的例外狀況中，宣告順序並不重要。 C# 不會限制原始程式檔只能宣告一個公用型別，也不會要求原始程式檔符合原始程式檔中所宣告的型別名稱。

## <a name="types-and-variables"></a>型別與變數

C# 中有兩種型別：***實值型別***和***參考型別***。 實值型別的變數直接包含其資料，而參考型別的變數則將參考儲存到其資料，後者即是物件。 使用參考型別時，這兩種變數可以參考相同的物件，因此對其中一個變數進行的作業可能會影響另一個變數所參考的物件。 使用實值型別時，變數各有自己的資料複本，因此在某一個變數上進行的作業不可能會影響其他變數 (但 `ref` 和 `out` 參數變數除外)。

C# 的實值型別可進一步細分***簡單型別***，***列舉型別***，***結構型別***，以及***可為 null 的型別***，和 C# 參考型別可進一步細分***類別型別***，***介面型別***，***陣列型別***，以及***委派型別***。

下表提供 C# 類型系統的概觀。

| __分類__    |                 | __描述__ |
|-----------------|-----------------|-----------------|
| 值類型     | 簡單型別    | 帶正負號的整數︰`sbyte`、`short`、`int`、`long` |
|                 |                 | 不帶正負號的整數︰`byte`、`ushort`、`uint`、`ulong` |
|                 |                 | Unicode 字元：`char` |
|                 |                 | IEEE 浮點數：`float`、`double` |
|                 |                 | 高精確度十進位︰`decimal` |
|                 |                 | 布林值：`bool` |
|                 | 列舉型別      | 使用者定義型別，格式為 `enum E {...}` |
|                 | 結構型別    | 使用者定義型別，格式為 `struct S {...}` |
|                 | 可為 Null 的型別  | 含有 `null` 值的所有其他數值型別的擴充 |
| 參考型別 | 類別型別     | 所有其他型別的基底類別︰`object` |
|                 |                 | Unicode 字串：`string` |
|                 |                 | 使用者定義型別，格式為 `class C {...}` |
|                 | 介面型別 | 使用者定義型別，格式為 `interface I {...}` |
|                 | 陣列型別     | 單一維度和多維度，例如 `int[]` 和 `int[,]` |
|                 | 委派型別  | 使用者定義型別，格式例如 `delegate int  D(...)` |

八種整數型別支援 8 位元、16 位元、32 位元和 64 位元的值 (帶正負號或不帶正負號)。

兩個浮動點類型`float`和`double`，會使用 32 位元單精確度和 64 位元雙精確度 IEEE 754 格式來表示。

`decimal` 型別是 128 位元的資料型別，適合財務和貨幣計算。

C#`bool`類型用來代表布林值 — 值，不是`true`或`false`。

C# 中的字元和字串處理使用 Unicode 編碼方式。 `char` 型別代表 UTF-16 程式碼單位，而 `string` 型別代表一系列的 UTF-16 程式碼單位。

下表摘要說明 C# 的數值型別。


| __分類__      | __位元__ | __Type__  | __範圍/精確度__ |
|-------------------|----------|-----------|---------------------|
| 帶正負號的整數   | 8        | `sbyte`   | -128...127 |
|                   | 16       | `short`   | -32,768...32,767 |
|                   | 32       | `int`     | -2,147,483,648...2,147,483,647 |
|                   | 64       | `long`    | -9,223,372,036,854,775,808...9,223,372,036,854,775,807 |
| 不帶正負號的整數 | 8        | `byte`    | 0...255 |
|                   | 16       | `ushort`  | 0...65,535 |
|                   | 32       | `uint`    | 0...4,294,967,295 |
|                   | 64       | `ulong`   | 0...18,446,744,073,709,551,615 |
| 浮點    | 32       | `float`   | 1.5 × 10 ^-45 到 3.4 × 10 ^38，7 位數精確度 |
|                   | 64       | `double`  | 5.0 × 10 ^324 到 1.7 × 10 ^308 之內，15 位數精確度 |
| Decimal           | 128      | `decimal` | 1.0 × 10 ^ −28 到 7.9 × 10 ^ 月 28 日 28 位數精確度 |

C# 程式使用***型別宣告***來建立新型別。 型別宣告指定新型別的名稱成員。 可由使用者定義五個 C# 類別的型別︰ 類別型別、 結構型別、 介面型別、 列舉型別及委派型別。

類別型別定義資料結構，其中包含資料成員 （欄位） 和函式成員 （方法、 屬性和其他項目）。 類別型別支援單一繼承和多型，這些是可供衍生類別將基底類別延伸及特製化的機制。

它代表與資料成員和函式成員的結構與類別類型類似的結構類型。 不過，不同於類別，結構是實值類型，而且不需要堆積配置。 結構型別不支援使用者指定的繼承，且所有結構型別都隱含地繼承自 `object` 型別。

介面型別定義為一組具名的公用函式成員的合約。 類別或結構實作介面，必須提供介面的函式成員的實作。 介面可以繼承自多個基底介面，以及類別或結構可以實作多個介面。

委派型別代表具有特定參數清單和傳回類型的方法的參考。 委派讓您可將方法視為實體，而實體能指派給變數或當作參數來傳遞。 委派就類似其他程式設計語言中的函式指標，但與函式指標的不同之處是，委派是物件導向且為型別安全。

類別、 結構、 介面及委派型別全都支援泛型，因此它們可以參數化與其他型別。

列舉型別是具名常數的不同類型。 每個列舉型別具有基礎類型，必須是其中一種八種整數類資料型別。 一組值的列舉型別是一組與基礎型別的值相同。

C# 支援任何型別的單一維度及多維陣列。 不同於上述型別，陣列型別不需宣告即可使用。 而陣列型別的建構方法，是在型別名稱之後加上方括弧。 例如，`int[]`是一維陣列`int`，`int[,]`是二維陣列`int`，並`int[][]`是一維陣列的一維陣列`int`。

可為 null 的型別也沒有宣告才能使用。 針對每個不可為 null 的實值型別`T`沒有對應的可為 null 型別`T?`，其中可包含其他值`null`。 比方說，`int?`是可以保留任何 32 位元整數或值的型別`null`。

C# 類型系統統一，任何類型的值可視為物件。 C# 中的每個型別都直接或間接衍生自 `object` 類別型別，而 `object` 是所有型別的基底類別。 參考型別的值之所以會視為物件，只是將這些值當作 `object` 型別來檢視。 實值類型的值時，會被當作物件上，執行***boxing***並***unboxing***作業。 在下列範例中，`int` 值會轉換成 `object`，並再次轉換回 `int`。

```csharp
using System;

class Test
{
    static void Main() {
        int i = 123;
        object o = i;          // Boxing
        int j = (int)o;        // Unboxing
    }
}
```
當實值型別的值轉換成輸入`object`物件執行個體，也稱為 「 box 」 配置給包含值，且值複製到該方塊。 相反地，當`object`參考轉換為實值型別、 核取設為所參考的物件 方塊中的正確的實值型別，而且，如果檢查成功，方塊中的值複製出來。

C# 的統一型別系統實際上意味者實值型別可能會變得 「 依需求。 」 的物件 因為整合，使用 `object` 型別的一般用途程式庫就能搭配參考型別和實值型別使用。

C# 中有數種***變數***，包括欄位、陣列元素、區域變數和參數。 變數表示儲存位置，而每個變數具有一種類型，判斷哪些值可以儲存在變數中下, 表所示。


| __變數的類型__    | __可能的內容__ |
|-------------------------|-----------------------|
| 不可為 Null 的實值型別 | 該型別的值 |
| 可為 Null 的實值型別     | Null 值或該型別的值 |
| `object`                | Null 參考，任何參考型別中，物件的參考或任何實值型別的 boxed 值的參考 |
| 類別型別              | Null 參考，該類別類型的執行個體的參考或類別的執行個體的參考衍生自該類別類型 |
| 介面類型          | Null 參考，實作該介面型別中，類別類型的執行個體的參考或實值型別會實作該介面型別為 boxed 值的參考 |
| 陣列型別              | Null 參考，該陣列型別中，執行個體的參考或相容的陣列型別的執行個體的參考 |
| 委派類型           | Null 參考或該委派類型的執行個體的參考 |

## <a name="expressions"></a>運算式

「運算式」是由「運算元」和「運算子」建構而成。 運算式的運算子會指出要將哪些運算套用到運算元。 運算子範例包括 `+`、`-`、`*`、`/` 及 `new`。 運算元範例包括常值、欄位、區域變數及運算式。

當運算式包含多個運算子時，運算子的「優先順序」會控制評估個別運算子的順序。 例如，運算式 `x + y * z` 會評估為 `x + (y * z)`，因為 `*` 運算子的優先順序高於 `+` 運算子。

大多數運算子都是可以「多載」的運算子。 運算子多載可允許針對一個運算元屬於 (或兩個運算元都屬於) 使用者定義之類別或結構型別的運算式，指定使用者定義的運算子實作。

下表摘要說明 C# 運算子，列出運算子分類中的優先順序從最高到低排列順序。 相同分類中的運算子具有相等的優先順序。


| __分類__                     | __運算式__    | __描述__ |
|----------------------------------|-------------------|-----------------|
| 主要                          | `x.m`             | 成員存取 |
|                                  | `x(...)`          | 方法和委派叫用 |
|                                  | `x[...]`          | 陣列和索引子存取 |
|                                  | `x++`             | 後置遞增 |
|                                  | `x--`             | 後置遞減 |
|                                  | `new T(...)`      | 建立物件和委派 |
|                                  | `new T(...){...}` | 使用初始設定式來建立物件 |
|                                  | `new {...}`       | 匿名物件初始設定式 |
|                                  | `new T[...]`      | 建立陣列 |
|                                  | `typeof(T)`       | 取得 `T` 的 `System.Type` 物件 |
|                                  | `checked(x)`      | 在核取的內容中評估運算式 |
|                                  | `unchecked(x)`    | 在未核取的內容中評估運算式 |
|                                  | `default(T)`      | 取得類型 `T` 的預設值 |
|                                  | `delegate {...}`  | 匿名函式 (匿名方法) |
| 一元                            | `+x`              | 身分識別 |
|                                  | `-x`              | 否定 |
|                                  | `!x`              | 邏輯否定 |
|                                  | `~x`              | 位元否定 |
|                                  | `++x`             | 前置遞增 |
|                                  | `--x`             | 前置遞減 |
|                                  | `(T)x`            | 將 `x` 明確轉換成類型 `T` |
|                                  | `await x`         | 以非同步方式等候 `x` 完成 |
| 乘法類 (Multiplicative)                   | `x * y`           | 乘法 |
|                                  | `x / y`           | 除號 |
|                                  | `x % y`           | 餘數 |
| 加法類 (Additive)                         | `x + y`           | 加法、字串串連、委派組合 |
|                                  | `x - y`           | 減法、委派移除 |
| Shift                            | `x << y`          | 左移 |
|                                  | `x >> y`          | 右移 |
| 關係和型別測試      | `x < y`           | 小於 |
|                                  | `x > y`           | 大於 |
|                                  | `x <= y`          | 小於或等於 |
|                                  | `x >= y`          | 大於或等於 |
|                                  | `x is T`          | 如果 `x` 是 `T`，便傳回 `true`，否則傳回 `false` |
|                                  | `x as T`          | 傳回歸類為 `T` 類型的 `x`，或在 `x` 不是 `T` 的情況下傳回 `null` |
| 相等                         | `x == y`          | 等於      |
|                                  | `x != y`          | 不等於 |
| 邏輯 AND                      | `x & y`           | 整數位元 AND、布林邏輯 AND |
| 邏輯 XOR                      | `x ^ y`           | 整數位元 XOR、布林邏輯 XOR |
| 邏輯 OR                       | <code>x &#124; y</code> | 整數位元 OR、布林邏輯 OR |
| 條件式 AND                  | `x && y`          | 會評估`y`只有當`x`是 `true` |
| 條件式 OR                   | <code>x &#124;&#124; y</code> | 會評估`y`只有當`x`是 `false` |
| Null 聯合                  | `x ?? y`          | 評估為`y`如果`x`是`null`至`x`否則 |
| 條件式                      | `x ? y : z`       | 如果 `x` 是 `true`，便評估 `y`，如果 `x` 是 `false`，則評估 `z` |
| 指派或匿名函式 | `x = y`           | 指派 |
|                                  | `x op= y`         | 複合指派;支援的運算子 `*=` `/=` `%=` `+=` `-=` `<<=` `>>=` `&=` `^=` <code>&#124;=</code> |
|                                  | `(T x) => y`      | 匿名函式 (Lambda 運算式) |

## <a name="statements"></a>陳述式

程式的動作是藉由***陳述式***來表達。 C# 支援數種不同類型的陳述式，其中一些是以內嵌陳述式來定義。

「區塊」可允許在許可單一陳述式的內容中撰寫多個陳述式。 區塊是由在 `{` 與 `}` 分隔符號之間撰寫的陳述式清單所組成。

「宣告陳述式」可用來宣告區域變數和常數。

「運算式陳述式」可用來評估運算式。 可用來當做陳述式的運算式包含方法引動過程中，物件使用的配置`new`運算子、 指派使用`=`和複合指派運算子、 使用的遞增和遞減運算`++`和`--`運算子和 await 運算式。

「選取範圍陳述式」可用來選取一些可能陳述式的其中之一，以根據某個運算式的值來執行。 在此群組中的是 `if` 和 `switch` 陳述式。

***反覆運算陳述式***用來重複執行內嵌陳述式。 在此群組中的是 `while`、`do`、`for` 及 `foreach` 陳述式。

「跳躍陳述式」可用來轉移控制項。 在此群組中的是 `break`、`continue`、`goto`、`throw`、`return` 及 `yield` 陳述式。

`try`...`catch` 陳述式可用來攔截在執行區塊時發生的例外狀況，而 `try`...`finally` 陳述式則可用來指定不論是否發生例外狀況都一律會執行的最終處理程式碼。

`checked`和`unchecked`陳述式用來控制溢位檢查整數型別算術運算和轉換的內容。

`lock` 陳述式可用來取得所指定物件的互斥鎖定、執行陳述式，然後釋放鎖定。

`using` 陳述式可用來取得資源、執行陳述式，然後處置該資源。

以下是每一種陳述式的範例

__本機變數宣告__

```csharp
static void Main() {
   int a;
   int b = 2, c = 3;
   a = 1;
   Console.WriteLine(a + b + c);
}
```


__區域常數宣告__

```csharp
static void Main() {
    const float pi = 3.1415927f;
    const int r = 25;
    Console.WriteLine(pi * r * r);
}
```


__運算式陳述式__

```csharp
static void Main() {
    int i;
    i = 123;                // Expression statement
    Console.WriteLine(i);   // Expression statement
    i++;                    // Expression statement
    Console.WriteLine(i);   // Expression statement
}
```

__`if` 陳述式__

```csharp
static void Main(string[] args) {
    if (args.Length == 0) {
        Console.WriteLine("No arguments");
    }
    else {
        Console.WriteLine("One or more arguments");
    }
}
```


__`switch` 陳述式__

```csharp
static void Main(string[] args) {
    int n = args.Length;
    switch (n) {
        case 0:
            Console.WriteLine("No arguments");
            break;
        case 1:
            Console.WriteLine("One argument");
            break;
        default:
            Console.WriteLine("{0} arguments", n);
            break;
    }
}
```

__`while` 陳述式__

```csharp
static void Main(string[] args) {
    int i = 0;
    while (i < args.Length) {
        Console.WriteLine(args[i]);
        i++;
    }
}
```


__`do` 陳述式__

```csharp
static void Main() {
    string s;
    do {
        s = Console.ReadLine();
        if (s != null) Console.WriteLine(s);
    } while (s != null);
}
```

__`for` 陳述式__

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        Console.WriteLine(args[i]);
    }
}
```

__`foreach` 陳述式__

```csharp
static void Main(string[] args) {
    foreach (string s in args) {
        Console.WriteLine(s);
    }
}
```

__`break` 陳述式__

```csharp
static void Main() {
    while (true) {
        string s = Console.ReadLine();
        if (s == null) break;
        Console.WriteLine(s);
    }
}
```

__`continue` 陳述式__

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        if (args[i].StartsWith("/")) continue;
        Console.WriteLine(args[i]);
    }
}
```

__`goto` 陳述式__

```csharp
static void Main(string[] args) {
    int i = 0;
    goto check;
    loop:
    Console.WriteLine(args[i++]);
    check:
    if (i < args.Length) goto loop;
}
```

__`return` 陳述式__

```csharp
static int Add(int a, int b) {
    return a + b;
}

static void Main() {
    Console.WriteLine(Add(1, 2));
    return;
}
```

__`yield` 陳述式__

```csharp
static IEnumerable<int> Range(int from, int to) {
    for (int i = from; i < to; i++) {
        yield return i;
    }
    yield break;
}

static void Main() {
    foreach (int x in Range(-10,10)) {
        Console.WriteLine(x);
    }
}
```

__`throw` 和`try`陳述式__

```csharp
static double Divide(double x, double y) {
    if (y == 0) throw new DivideByZeroException();
    return x / y;
}

static void Main(string[] args) {
    try {
        if (args.Length != 2) {
            throw new Exception("Two numbers required");
        }
        double x = double.Parse(args[0]);
        double y = double.Parse(args[1]);
        Console.WriteLine(Divide(x, y));
    }
    catch (Exception e) {
        Console.WriteLine(e.Message);
    }
    finally {
        Console.WriteLine("Good bye!");
    }
}
```

__`checked` 和`unchecked`陳述式__

```csharp
static void Main() {
    int i = int.MaxValue;
    checked {
        Console.WriteLine(i + 1);        // Exception
    }
    unchecked {
        Console.WriteLine(i + 1);        // Overflow
    }
}
```

__`lock` 陳述式__

```csharp
class Account
{
    decimal balance;
    public void Withdraw(decimal amount) {
        lock (this) {
            if (amount > balance) {
                throw new Exception("Insufficient funds");
            }
            balance -= amount;
        }
    }
}
```

__`using` 陳述式__

```csharp
static void Main() {
    using (TextWriter w = File.CreateText("test.txt")) {
        w.WriteLine("Line one");
        w.WriteLine("Line two");
        w.WriteLine("Line three");
    }
}
```

## <a name="classes-and-objects"></a>類別與物件

***類別***是 C# 最基本的型別。 類別是以單一單位結合狀態 (欄位) 和動作 (方法及其他函式成員) 的資料結構。 類別可以為動態建立的類別「執行個體」(稱為「物件」) 提供定義。 類別支援「繼承」和「多型」，這些是可供「衍生類別」將「基底類別」延伸及特製化的機制。

建立新類別時，是使用類別宣告來建立。 類別宣告的開頭是一個標頭，此標頭會指定類別的屬性和修飾詞、類別的名稱、基底類別 (如果提供)，以及類別所實作的介面。 此標頭後面會接著類別主體，此主體是由在 `{` 與 `}` 分隔符號之間撰寫的成員宣告清單所組成。

以下是一個名為 `Point` 之簡單類別的宣告：

```csharp
public class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
建立類別執行個體時，是使用 `new` 運算子來建立，此運算子會為新執行個體配置記憶體、叫用建構函式來將執行個體初始化，然後傳回對執行個體的參考。 下列陳述式會建立兩個`Point`物件，並將這些物件的參考儲存在兩個變數：

```
Point p1 = new Point(0, 0);
Point p2 = new Point(10, 20);
```
不再使用物件時，會自動回收物件所佔用的記憶體。 在 C# 中，既沒有必要也不可能明確地將物件解除配置。

### <a name="members"></a>成員

類別的成員都是***靜態成員***或是***執行個體成員***。 靜態成員隸屬於類別，而執行個體成員則隸屬於物件 (類別的執行個體)。

下表提供的類別可以包含的成員類型的概觀。


| __成員__   | __描述__ |
|------------  |-----------------|
| 常數    | 與類別關聯的常數值 |
| 欄位       | 類別的變數 |
| 方法      | 類別所能執行的計算和動作 |
| 屬性   | 與讀取和寫入具名的類別特性關聯的動作 |
| 索引子     | 與編製陣列之類的類別執行個體關聯的動作 |
| 事件       | 類別所能產生的通知 |
| 運算子    | 類別所支援的轉換和運算式運算子 |
| 建構函式 | 將類別執行個體或類別本身初始化所需的動作 |
| 解構函式  | 在永久捨棄類別執行個體之前所要執行的動作 |
| 型別        | 類別所宣告的巢狀型別 |

### <a name="accessibility"></a>協助工具選項

類別的每個成員都有關聯的存取能力，用來控制能夠存取成員的程式文字區域。 存取能力有五種可能的形式。 下表將摘要說明這些報表：


| __協助工具選項__    | __意義__ |
|----------------------|-----------------|
| `public`             | 存取不受限制 |
| `protected`          | 存取僅限於此類別或此類別所衍生的類別 |
| `internal`           | 存取僅限於此程式 |
| `protected internal` | 存取僅限於此程式或此類別所衍生的類別 |
| `private`            | 存取僅限於此類別 |

### <a name="type-parameters"></a>型別參數

類別定義可以在類別名稱後面以角括弧括住型別參數名稱清單，來定義一組型別參數。 型別參數可在類別宣告的主體中用來定義類別的成員。 在下列範例中，`Pair` 的型別參數是 `TFirst` 和 `TSecond`：

```csharp
public class Pair<TFirst,TSecond>
{
    public TFirst First;
    public TSecond Second;
}
```
類別類型宣告為會採用型別參數，稱為泛型類別型別。 結構、介面及委派型別也可以是泛型型別。

使用泛型類別時，必須為每個型別參數提供型別參數：

```csharp
Pair<int,string> pair = new Pair<int,string> { First = 1, Second = "two" };
int i = pair.First;     // TFirst is int
string s = pair.Second; // TSecond is string
```
泛型型別與型別引數提供，例如`Pair<int,string>
    `以上版本，稱為建構的型別。

### <a name="base-classes"></a>基底類別

類別宣告可以在類別名稱和型別參數後面加上冒號和基底類別的名稱，來指定基底類別。 省略基底類別規格即等同於衍生自類型 `object`。 在下列範例中，`Point3D` 的基底類別是 `Point`，而 `Point` 的基底類別是 `object`：

```csharp
public class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

public class Point3D: Point
{
    public int z;

    public Point3D(int x, int y, int z): base(x, y) {
        this.z = z;
    }
}
```
類別會繼承其基底類別的成員。 繼承意謂類別以隱含方式包含其基底類別，除了執行個體和靜態建構函式，以及基底類別的解構函式的所有成員。 衍生類別可以在其繼承的成員中新增新的成員，但無法移除所繼承成員的定義。 在先前的範例中，`Point3D` 會從 `Point` 繼承 `x` 和 `y` 欄位，而每個 `Point3D` 執行個體都會包含 `x`、`y` 及 `z` 這三個欄位。

在類別型別到其任何基底類別型別之間都存在著隱含轉換。 因此，類別型別的變數可以參考該類別的執行個體，或任何衍生類別的執行個體。 例如，以先前的類別宣告為例，`Point` 型別的變數可以參考 `Point` 或 `Point3D`：

```csharp
Point a = new Point(10, 20);
Point b = new Point3D(10, 20, 30);
```

### <a name="fields"></a>欄位

欄位是具有類別或類別的執行個體相關聯的變數。

欄位宣告`static`修飾詞定義***靜態欄位***。 靜態欄位只會識別一個儲存位置。 不論建立多少個類別執行個體，都只會有一個靜態欄位複本。

欄位宣告不含`static`修飾詞定義***執行個體欄位***。 每個類別執行個體都包含一個該類別所有執行個體欄位的個別複本。

在下列範例中，每個 `Color` 類別執行個體都具有 `r`、`g` 及 `b` 執行個體欄位的個別複本，但只有一個 `Black`、`White`、`Red`、`Green` 及 `Blue` 靜態欄位的複本：

```csharp
public class Color
{
    public static readonly Color Black = new Color(0, 0, 0);
    public static readonly Color White = new Color(255, 255, 255);
    public static readonly Color Red = new Color(255, 0, 0);
    public static readonly Color Green = new Color(0, 255, 0);
    public static readonly Color Blue = new Color(0, 0, 255);
    private byte r, g, b;

    public Color(byte r, byte g, byte b) {
        this.r = r;
        this.g = g;
        this.b = b;
    }
}
```
如先前的範例所示，可以使用 `readonly` 修飾詞來宣告「唯讀欄位」。 指派給`readonly`欄位的欄位的宣告，或在相同類別中的建構函式才會發生。

### <a name="methods"></a>方法

「方法」是實作物件或類別所能執行之計算或動作的成員。 存取「靜態方法」時，是透過類別來存取。 存取「執行個體方法」時，是透過類別的執行個體來存取。

方法有 （可能是空的） 清單***參數***，代表值或變數的參考傳遞給方法，以及***傳回型別***，指定所計算並傳回值的型別此方法。 方法的傳回型別是`void`如果它不會傳回值。

與型別相同，方法也可能有一組型別參數，而呼叫方法時，必須為這些參數指定型別引數。 與型別不同的是，型別引數通常可以從方法呼叫的引數推斷，而不需要明確指定。

在宣告方法的類別中，方法的「簽章」必須是唯一的。 方法的簽章是由方法的名稱、型別參數的數目以及其參數的數目、修飾詞和型別所組成。 方法的簽章並不包括傳回型別。

#### <a name="parameters"></a>參數

參數是用來將值或變數參考傳遞給方法。 方法的參數會從叫用方法時所指定的「引數」取得其實際值。 參數有四種：值參數、參考參數、輸出參數，以及參數陣列。

「值參數」適用於傳遞輸入參數。 值參數會對應至區域變數，此變數會從針對參數傳遞的引數取得其初始值。 對值參數進行修改並不會影響已針對參數傳遞的引數。

只要指定預設值，值參數便可以成為選用參數，如此即可省略對應的引數。

「參考參數」同時適用於傳遞輸入和輸出參數。 針對參考參數傳遞的引數必須是變數，而在執行方法的期間，參考參數會代表與引數變數相同的儲存位置。 宣告參考參數時，是使用 `ref` 修飾詞來宣告。 下列範例示範 `ref` 參數的用法。

```csharp
using System;

class Test
{
    static void Swap(ref int x, ref int y) {
        int temp = x;
        x = y;
        y = temp;
    }

    static void Main() {
        int i = 1, j = 2;
        Swap(ref i, ref j);
        Console.WriteLine("{0} {1}", i, j);            // Outputs "2 1"
    }
}
```
「輸出參數」適用於傳遞輸出參數。 輸出參數與參考參數類似，不同之處在於呼叫端所提供引數的初始值並不重要。 宣告輸出參數時，是使用 `out` 修飾詞來宣告。 下列範例示範 `out` 參數的用法。

```csharp
using System;

class Test
{
    static void Divide(int x, int y, out int result, out int remainder) {
        result = x / y;
        remainder = x % y;
    }

    static void Main() {
        int res, rem;
        Divide(10, 3, out res, out rem);
        Console.WriteLine("{0} {1}", res, rem);    // Outputs "3 1"
    }
}
```
「參數陣列」可允許將數目不固定的引數傳遞給方法。 宣告參數陣列時，是使用 `params` 修飾詞來宣告。 只有方法的最後一個參數可以是參數陣列，而參數陣列的型別必須是單一維度陣列型別。 `Write`並`WriteLine`方法`System.Console`類別是參數陣列用法的好例子。 其宣告方式如下。

```csharp
public class Console
{
    public static void Write(string fmt, params object[] args) {...}
    public static void WriteLine(string fmt, params object[] args) {...}
    ...
}
```
在使用參數陣列的方法內，參數陣列的行為與陣列型別的一般參數完全相同。 不過，在叫用含有參數陣列的方法時，可以傳遞單一的參數陣列型別引數，或是傳遞任意數目的參數陣列元素型別引數。 在後者的案例中，會自動建立陣列執行個體並以指定的引數將其初始化。 以下範例

```csharp
Console.WriteLine("x={0} y={1} z={2}", x, y, z);
```
等同於撰寫下列程式碼。

```csharp
string s = "x={0} y={1} z={2}";
object[] args = new object[3];
args[0] = x;
args[1] = y;
args[2] = z;
Console.WriteLine(s, args);
```

#### <a name="method-body-and-local-variables"></a>方法主體和區域變數

方法的主體會指定要叫用方法時執行的陳述式。

方法主體可以宣告方法叫用專屬的變數。 這類變數稱為「區域變數」。 區域變數宣告會指定型別名稱、變數名稱，還可能指定初始值。 下列範例會宣告一個初始值為零的區域變數 `i`，以及一個沒有初始值的區域變數 `j`。

```csharp
using System;

class Squares
{
    static void Main() {
        int i = 0;
        int j;
        while (i < 10) {
            j = i * i;
            Console.WriteLine("{0} x {0} = {1}", i, j);
            i = i + 1;
        }
    }
}
```
C# 要求必須「明確指派」區域變數，才能取得其值。 例如，如果先前 `i` 的宣告並未包含初始值，編譯器就會在 `i` 後續被使用時回報錯誤，因為在這些時間點並不會在程式中明確指派 `i`。

方法可以使用 `return` 陳述式將控制權交還給其呼叫端。 在傳回 `void` 的方法中，`return` 陳述式不能指定運算式。 在方法中傳回非`void`，`return`陳述式必須包含計算傳回值的運算式。

#### <a name="static-and-instance-methods"></a>靜態和執行個體方法

方法宣告`static`修飾詞***靜態方法***。 靜態方法不會在特定的執行個體上運作，並且只能直接存取靜態成員。

方法宣告為不含`static`修飾詞***執行個體方法***。 執行個體方法會在特定的執行個體上運作，並且既可存取靜態成員也可存取執行個體成員。 透過 `this` 可以明確存取叫用執行個體方法時所在的執行個體。 參考靜態方法中的 `this` 會產生錯誤。

下列 `Entity` 類別同時含有靜態成員和執行個體成員。

```csharp
class Entity
{
    static int nextSerialNo;
    int serialNo;

    public Entity() {
        serialNo = nextSerialNo++;
    }

    public int GetSerialNo() {
        return serialNo;
    }

    public static int GetNextSerialNo() {
        return nextSerialNo;
    }

    public static void SetNextSerialNo(int value) {
        nextSerialNo = value;
    }
}
```
每個 `Entity` 執行個體都包含一個序號 (並且可能包含一些其他這裡未顯示的資訊)。 `Entity` 建構函式 (類似於執行個體方法) 會將具有下一個可用序號的新執行個體初始化。 由於此建構函式式執行個體成員，因此它既可存取 `serialNo` 執行個體欄位，也可存取 `nextSerialNo` 靜態欄位。

`GetNextSerialNo` 和 `SetNextSerialNo` 靜態方法可以存取 `nextSerialNo` 靜態欄位，但如果直接存取 `serialNo` 執行個體欄位，則會產生錯誤。

下列範例示範使用`Entity`類別。

```csharp
using System;

class Test
{
    static void Main() {
        Entity.SetNextSerialNo(1000);
        Entity e1 = new Entity();
        Entity e2 = new Entity();
        Console.WriteLine(e1.GetSerialNo());           // Outputs "1000"
        Console.WriteLine(e2.GetSerialNo());           // Outputs "1001"
        Console.WriteLine(Entity.GetNextSerialNo());   // Outputs "1002"
    }
}
```
請注意，`SetNextSerialNo` 和 `GetNextSerialNo` 靜態方法的叫用位置是在類別上，而 `GetSerialNo` 執行個體方法的叫用位置則是在類別的執行個體上。

#### <a name="virtual-override-and-abstract-methods"></a>虛擬、覆寫及抽象方法

當執行個體方法宣告包含 `virtual` 修飾詞時，該方法即稱為「虛擬方法」。 若未`virtual`修飾詞存在，則此方法即稱為***非虛擬方法***。

叫用虛擬方法時，叫用所針對之執行個體的「執行階段型別」會決定要叫用的實際方法實作。 在非虛擬方法叫用中，決定因素則是執行個體的「編譯階段型別」。

在衍生類別中可以「覆寫」虛擬方法。 當執行個體方法宣告包含`override`修飾詞，此方法會覆寫具有相同的簽章的繼承虛擬方法。 虛擬方法宣告會導入新的方法，而覆寫方法宣告則是會提供現有已繼承之虛擬方法的新實作，來將該方法特製化。

***抽象***方法是虛擬的方法沒有實作。 抽象方法以宣告`abstract`修飾詞，但只有在也會宣告一個類別則允許`abstract`。 抽象方法必須在每個非抽象的衍生類別中被覆寫。

下列範例會宣告一個抽象類別 `Expression` 和三個衍生的類別 `Constant`、`VariableReference` 及 `Operation`，前者代表一個運算式樹狀架構節點，後者則會實作常數、變數參考及算數運算式的運算式樹狀架構節點。 (這是類似，但不是會與混淆運算式樹狀架構型別中介紹[運算式樹狀架構類型](types.md#expression-tree-types))。

```csharp
using System;
using System.Collections;

public abstract class Expression
{
    public abstract double Evaluate(Hashtable vars);
}

public class Constant: Expression
{
    double value;

    public Constant(double value) {
        this.value = value;
    }

    public override double Evaluate(Hashtable vars) {
        return value;
    }
}

public class VariableReference: Expression
{
    string name;

    public VariableReference(string name) {
        this.name = name;
    }

    public override double Evaluate(Hashtable vars) {
        object value = vars[name];
        if (value == null) {
            throw new Exception("Unknown variable: " + name);
        }
        return Convert.ToDouble(value);
    }
}

public class Operation: Expression
{
    Expression left;
    char op;
    Expression right;

    public Operation(Expression left, char op, Expression right) {
        this.left = left;
        this.op = op;
        this.right = right;
    }

    public override double Evaluate(Hashtable vars) {
        double x = left.Evaluate(vars);
        double y = right.Evaluate(vars);
        switch (op) {
            case '+': return x + y;
            case '-': return x - y;
            case '*': return x * y;
            case '/': return x / y;
        }
        throw new Exception("Unknown operator");
    }
}
```
先前的四個類別可用來建構算數運算式的模型。 例如，在使用這些類別執行個體的情況下，可以將運算式 `x + 3` 表示如下。

```csharp
Expression e = new Operation(
    new VariableReference("x"),
    '+',
    new Constant(3));
```
系統會叫用 `Expression` 執行個體的 `Evaluate` 方法來評估指定的運算式並產生 `double` 值。 此方法會採用當做引數`Hashtable`，其中包含變數名稱 （作為項目的索引鍵） 和值 （作為項目的值）。 `Evaluate`方法是虛擬的抽象方法，這表示非抽象衍生的類別必須覆寫它提供實際的實作。

`Constant` 的 `Evaluate` 實作會直接傳回預存的常數。 A`VariableReference`的實作會查詢雜湊表中的變數名稱，並傳回結果的值。 `Operation` 的實作會先評估左邊和右邊的運算元 (透過以遞迴方式叫用其 `Evaluate` 方法)，然後才執行指定的算數運算。

下列程式會使用 `Expression` 類別來評估不同 `x` 和 `y` 值的 `x * (y + 2)` 運算式。

```csharp
using System;
using System.Collections;

class Test
{
    static void Main() {
        Expression e = new Operation(
            new VariableReference("x"),
            '*',
            new Operation(
                new VariableReference("y"),
                '+',
                new Constant(2)
            )
        );
        Hashtable vars = new Hashtable();
        vars["x"] = 3;
        vars["y"] = 5;
        Console.WriteLine(e.Evaluate(vars));        // Outputs "21"
        vars["x"] = 1.5;
        vars["y"] = 9;
        Console.WriteLine(e.Evaluate(vars));        // Outputs "16.5"
    }
}
```

#### <a name="method-overloading"></a>方法多載

方法「多載」可允許相同類別中的多個方法擁有相同的名稱，只要它們的簽章是唯一的即可。 編譯多載方法的叫用時，編譯器會使用「多載解析」來判斷要叫用的特定方法。 多載解析會尋找一個與引數最相符的方法，或者，如果找不到任何一個最相符的方法，則會回報錯誤。 下列範例示範多載解析的實際運作情況。 `Main` 方法中每項叫用的註解會顯示實際叫用的方法是哪一個。

```csharp
class Test
{
    static void F() {
        Console.WriteLine("F()");
    }

    static void F(object x) {
        Console.WriteLine("F(object)");
    }

    static void F(int x) {
        Console.WriteLine("F(int)");
    }

    static void F(double x) {
        Console.WriteLine("F(double)");
    }

    static void F<T>(T x) {
        Console.WriteLine("F<T>(T)");
    }

    static void F(double x, double y) {
        Console.WriteLine("F(double, double)");
    }

    static void Main() {
        F();                 // Invokes F()
        F(1);                // Invokes F(int)
        F(1.0);              // Invokes F(double)
        F("abc");            // Invokes F(object)
        F((double)1);        // Invokes F(double)
        F((object)1);        // Invokes F(object)
        F<int>(1);           // Invokes F<T>(T)
        F(1, 1);             // Invokes F(double, double)
    }
}
```
如範例所示，透過將引數明確轉換成確切的參數型別和 (或) 明確提供型別引數，即一律可以選取特定的方法。

### <a name="other-function-members"></a>其他函式成員

包含可執行程式碼的成員統稱為類別的「函式成員」。 上節中所述的方法是主要的函式成員類型。 本節說明 C# 所支援的函式成員的其他媒體類型： 建構函式、 屬性、 索引子、 事件、 運算子和解構函式。

下列程式碼會顯示泛型類別，稱為`List<T>`，它會實作可成長的物件清單。 此類別包含數個最常見的函式成員類型。


```csharp
public class List<T> {
    // Constant...
    const int defaultCapacity = 4;

    // Fields...
    T[] items;
    int count;

    // Constructors...
    public List(int capacity = defaultCapacity) {
        items = new T[capacity];
    }

    // Properties...
    public int Count {
        get { return count; }
    }
    public int Capacity {
        get {
            return items.Length;
        }
        set {
            if (value < count) value = count;
            if (value != items.Length) {
                T[] newItems = new T[value];
                Array.Copy(items, 0, newItems, 0, count);
                items = newItems;
            }
        }
    }

    // Indexer...
    public T this[int index] {
        get {
            return items[index];
        }
        set {
            items[index] = value;
            OnChanged();
        }
    }

    // Methods...
    public void Add(T item) {
        if (count == Capacity) Capacity = count * 2;
        items[count] = item;
        count++;
        OnChanged();
    }
    protected virtual void OnChanged() {
        if (Changed != null) Changed(this, EventArgs.Empty);
    }
    public override bool Equals(object other) {
        return Equals(this, other as List<T>);
    }
    static bool Equals(List<T> a, List<T> b) {
        if (a == null) return b == null;
        if (b == null || a.count != b.count) return false;
        for (int i = 0; i < a.count; i++) {
            if (!object.Equals(a.items[i], b.items[i])) {
                return false;
            }
        }
        return true;
    }

    // Event...
    public event EventHandler Changed;

    // Operators...
    public static bool operator ==(List<T> a, List<T> b) {
        return Equals(a, b);
    }
    public static bool operator !=(List<T> a, List<T> b) {
        return !Equals(a, b);
    }
}
```

#### <a name="constructors"></a>建構函式

C# 同時支援執行個體建構函式和靜態建構函式。 「執行個體建構函式」是實作將類別執行個體初始化所需之動作的成員。 「靜態建構函式」是實作第一次將類別本身載入時將其初始化所需之動作的成員。

建構函式的宣告方式與方法類似，但不含傳回型別且名稱會與包含它的類別相同。 如果建構函式宣告包含`static`修飾詞，它會宣告靜態建構函式。 否則，所宣告的會是執行個體建構函式。

執行個體建構函式可以多載。 例如，`List<T>
` 類別會宣告兩個執行個體建構函式，一個沒有任何參數，另一個會採用 `int` 參數。 叫用執行個體建構函式時，是使用 `new` 運算子來叫用。 下列陳述式會配置兩個`List<string>
`執行個體使用的建構函式的每個`List`類別。

```csharp
List<string> list1 = new List<string>();
List<string> list2 = new List<string>(10);
```
與其他成員不同，類別並不會繼承執行個體建構函式，而且除了類別中實際宣告的執行個體建構函式以外，類別即不會再有任何執行個體建構函式。 如果沒有為類別提供任何執行個體建構函式，則會自動提供一個沒有任何參數的空建構函式。

#### <a name="properties"></a>屬性

「屬性」是欄位的自然延伸。 兩者都是具有關聯型別的具名成員，並且用來存取欄位和屬性的語法是相同的。 不過，與欄位不同的是，屬性並不會指示儲存位置。 取而代之的是，屬性會有「存取子」，這些存取子會指定讀取或寫入其值時要執行的陳述式。

屬性宣告等 欄位中，不同之處在於宣告的結尾`get`存取子和 （或)`set`存取子分隔符號之間撰寫`{`和`}`而不是指以分號結尾。 同時具有屬性`get`存取子和`set`存取子是***讀寫屬性***，此屬性，只有`get`存取子則***唯讀屬性***，和屬性，只有`set`存取子已經***唯寫屬性***。

A`get`存取子對應到無參數的方法具有傳回值的屬性型別。 在運算式中，參考屬性時，做為目標的指派，除了`get`屬性存取子會叫用來計算屬性的值。

A`set`存取子對應至方法，與具有單一參數具名`value`且沒有傳回型別。 屬性參考時指派的目標為或的運算元`++`或是`--`，則`set`提供新值的引數叫用存取子。

`List<T>
` 類別會宣告 `Count` 和 `Capacity` 這兩個屬性，它們分別是唯讀屬性和讀寫屬性。 以下是這些屬性的用法範例。

```csharp
List<string> names = new List<string>();
names.Capacity = 100;            // Invokes set accessor
int i = names.Count;             // Invokes get accessor
int j = names.Capacity;          // Invokes get accessor
```
與欄位和方法類似，C# 也同時支援執行個體屬性和靜態屬性。 使用宣告靜態屬性`static`修飾詞和執行個體屬性的宣告而不需要它。

屬性的存取子可以是虛擬的。 當屬性宣告包含 `virtual`、`abstract` 或 `override` 修飾詞時，會套用至該屬性的存取子。

#### <a name="indexers"></a>索引子

「索引子」是可讓物件以和陣列相同的方式進行索引編製的成員。 索引子宣告為屬性不同之處在於成員的名稱是`this`後面接著參數清單分隔符號之間撰寫`[`和`]`。 索引子的存取子中會提供參數。 與屬性類似，索引子可以是讀寫、唯讀及唯寫的，而索引子的存取子可以是虛擬的。

`List` 類別會宣告一個採用 `int` 參數的單一讀寫索引子。 此索引子使得系統能夠以 `int` 值編製 `List` 執行個體的索引。 例如

```csharp
List<string> names = new List<string>();
names.Add("Liz");
names.Add("Martha");
names.Add("Beth");
for (int i = 0; i < names.Count; i++) {
    string s = names[i];
    names[i] = s.ToUpper();
}
```
索引子可被多載，這意謂著類別可以宣告多個索引子，只要其參數的號碼或型別不同即可。

#### <a name="events"></a>事件

「事件」是可讓類別或物件提供通知的成員。 事件宣告方式與欄位類似之處在於宣告包含`event`關鍵字和類型必須是委派型別。

在宣告事件成員的類別內，事件的行為與委派型別的欄位相同 (前提是該事件不是抽象事件且未宣告存取子)。 欄位會儲存對委派項目的參考，該委派項目代表已新增到事件中的事件處理常式。 如果沒有事件處理常式都存在，則欄位會`null`。

`List<T>
` 類別會宣告一個名為 `Changed` 的單一事件成員，此成員會指出新項目已新增到清單中。 `Changed`引發事件時`OnChanged`虛擬方法，會先檢查事件是否`null`（亦即沒有處理常式是否都存在）。 引發事件的概念完全等同於叫用該事件所代表的委派項目，因此，就引發事件而言，並沒有任何特殊的語言建構。

用戶端是透過「事件處理常式」來對事件進行反應。 附加事件處理常式時，是使用 `+=` 運算子，移除時，則是使用 `-=` 運算子。 下列範例會將事件處理常式附加到 `List<string>
` 的 `Changed` 事件。

```csharp
using System;

class Test
{
    static int changeCount;

    static void ListChanged(object sender, EventArgs e) {
        changeCount++;
    }

    static void Main() {
        List<string> names = new List<string>();
        names.Changed += new EventHandler(ListChanged);
        names.Add("Liz");
        names.Add("Martha");
        names.Add("Beth");
        Console.WriteLine(changeCount);        // Outputs "3"
    }
}
```
針對需要控制事件之基礎儲存的進階案例，事件宣告可以明確提供 `add` 和 `remove` 存取子，這有些類似於屬性的 `set` 存取子。

#### <a name="operators"></a>運算子

「運算子」是定義將特定運算式運算子套用到類別執行個體之意義的成員。 可定義的運算子有三種：一元運算子、二元運算子及轉換運算子。 所有運算子都必須宣告為 `public` 和 `static`。

`List<T>
` 類別會宣告 `operator==` 和 `operator!=` 這兩個運算子，藉此賦予將這些運算子套用到 `List` 執行個體的運算式新意義。 具體而言，運算子會定義兩個相等`List<T>
`做為比較所包含的物件使用的每個執行個體其`Equals`方法。 下列範例會使用 `==` 運算子來比較兩個 `List<int>
` 執行個體。

```csharp
using System;

class Test
{
    static void Main() {
        List<int> a = new List<int>();
        a.Add(1);
        a.Add(2);
        List<int> b = new List<int>();
        b.Add(1);
        b.Add(2);
        Console.WriteLine(a == b);        // Outputs "True"
        b.Add(3);
        Console.WriteLine(a == b);        // Outputs "False"
    }
}
```

第一個 `Console.WriteLine` 會輸出 `True`，因為兩個清單所包含物件的數目相同、值相同且順序相同。 如果 `List<T>
` 並未定義 `operator==`，則第一個 `Console.WriteLine` 所輸出的會是 `False`，因為 `a` 和 `b` 參考不同的 `List<int>
` 執行個體。

#### <a name="destructors"></a>解構函式

A***解構函式***成員實作來解構類別的執行個體所需的動作。 解構函式不能有參數，它們不能有存取範圍修飾詞，和他們無法明確地叫用。 執行個體的解構函式會在記憶體回收期間自動叫用。

記憶體回收行程是相當大的自由決定何時回收物件，並執行解構函式。 具體來說，解構函式引動過程的時機並沒有決定性，並可能在任何執行緒上執行解構函式。 如需這些和其他原因，類別應該只有沒有任何其他解決方案可行時，會實作解構函式。

`using` 陳述式提供較佳的物件解構方法。

## <a name="structs"></a>結構

和類別一樣，***結構***是可包含資料成員和函式成員的資料結構，但不同於類別，結構是實值型別，不需要堆積配置。 結構型別的變數直接儲存結構的資料，而類別型別的變數則儲存動態配置物件的參考。 結構型別不支援使用者指定的繼承，且所有結構型別都隱含地繼承自 `object` 型別。

結構特別適用於含有實值語意的小型資料結構。 複數、座標系統中的點或字典中的索引鍵/值組都是結構的良好範例。 針對小型資料結構使用結構而不使用類別，在應用程式執行的記憶體配置數目上有很大的差別。 比方說，下列程式會建立並初始化 100 個點的陣列。 使用 `Point` 做為類別，會具現化 101 個不同的物件 — 一個代表陣列，剩下的每個代表 100 個元素。

```csharp
class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

class Test
{
    static void Main() {
        Point[] points = new Point[100];
        for (int i = 0; i < 100; i++) points[i] = new Point(i, i);
    }
}
```
替代方案是使`Point`結構。

```csharp
struct Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
現在，只有一個物件會具現化 — 代表陣列的物件 — 而 `Point` 執行個體會以內嵌方式儲存在陣列中。

結構建構函式使用 `new` 運算子叫用，但並不代表會配置記憶體。 結構建構函式只會傳回結構值本身 (通常是在堆疊上的暫存位置)，然後再視需要複製此值，而不是動態配置一個物件並傳回該物件的參考。

使用類別時，這兩種變數可以參考相同的物件，因此對其中一個變數進行的作業可能會影響另一個變數參考的物件。 使用結構時，變數各有自己的資料複本，因此在某一個變數上進行的作業不可能會影響其他變數。 例如，下列程式碼片段所產生的輸出取決於是否`Point`是類別或結構。

```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 20;
Console.WriteLine(b.x);
```
如果`Point`是類別，而輸出則是`20`因為`a`和`b`參考相同的物件。 如果`Point`是結構，而輸出則是`10`因為指派`a`要`b`建立一份值，和此複本不會受到後續指派給`a.x`。

前一個範例會反白顯示結構的兩個限制。 首先，複製整個結構通常較複製物件參考沒有效率，因此，結構的指派和實值參數傳遞會比參考型別耗用更多資源。 再者，除了 `ref` 和 `out` 參數外，不可能建立結構的參考，因此在許多情況下無法使用它們。

## <a name="arrays"></a>陣列

***陣列***是一種資料結構，其中包含一些可透過計算索引存取的變數。 陣列中包含的變數 (也稱為陣列的***元素***) 屬於相同的型別，這種型別稱為陣列的***元素型別***。

陣列型別是參考型別，而陣列變數的宣告只是預留空間給陣列執行個體的參考。 在執行階段使用動態建立實際的陣列執行個體`new`運算子。 `new`作業指定***長度***新陣列執行個體，這固定的執行個體的存留期。 陣列元素的索引範圍在 `0` 到 `Length - 1` 之間。 `new` 運算子會自動將陣列的元素初始化為其預設值，例如，針對所有數值型別，此值為零，而針對所有參考型別，此值為 `null`。

下列範例會建立 `int` 元素的陣列、初始化陣列，並印出陣列的內容。

```csharp
using System;

class Test
{
    static void Main() {
        int[] a = new int[10];
        for (int i = 0; i < a.Length; i++) {
            a[i] = i * i;
        }
        for (int i = 0; i < a.Length; i++) {
            Console.WriteLine("a[{0}] = {1}", i, a[i]);
        }
    }
}
```
這個範例會建立並操作***一維陣列***。 C# 也支援***多維陣列***。 一個陣列型別的維度數目 (亦稱為陣列型別的***順位***)，是寫入陣列型別的方括弧之間的逗號數目加一。 下列範例會配置一維、 二維和三維陣列。

```csharp
int[] a1 = new int[10];
int[,] a2 = new int[10, 5];
int[,,] a3 = new int[10, 5, 2];
```
`a1` 陣列包含 10 個元素、`a2`陣列包含 50 (10 × 5) 個元素，`a3` 陣列包含 100 (10 × 5 × 2) 個元素。

陣列的元素型別可以是任一型別，包括陣列型別。 包含陣列型別之元素的陣列有時稱為***不規則陣列***，因為元素陣列的長度不需完全相同。 下列範例會配置一個 `int` 型別的陣列：

```csharp
int[][] a = new int[3][];
a[0] = new int[10];
a[1] = new int[5];
a[2] = new int[20];
```
第一行建立包含三個元素的陣列，每個元素的型別均為 `int[]`，每個元素的初始值均為 `null`。 接下來的幾行則使用不同長度的個別陣列執行個體的參考初始化三個元素。

`new`運算子允許使用指定的陣列元素的初始值***陣列初始設定式***，這是一份分隔符號之間撰寫的運算式`{`和`}`。 下列範例使用三個元素配置並初始化 `int[]`。

```csharp
int[] a = new int[] {1, 2, 3};
```
請注意，從之間的運算式數目推斷陣列的長度`{`和`}`。 本機變數與欄位宣告可以進一步縮短，就不需要重新敘述陣列型別。

```csharp
int[] a = {1, 2, 3};
```
先前的兩個範例等同於下列範例：

```csharp
int[] t = new int[3];
t[0] = 1;
t[1] = 2;
t[2] = 3;
int[] a = t;
```
## <a name="interfaces"></a>介面

「介面」定義可由類別和結構實作的合約。 介面可以包含方法、屬性、事件和索引子。 介面不提供它所定義之成員的實作 (它只會指定必須由類別提供的成員或實作介面的結構)。

介面可以採用「多重繼承」。 在下列範例中，介面 `IComboBox` 同時繼承自 `ITextBox` 和 `IListBox`。

```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

interface IComboBox: ITextBox, IListBox {}
```
類別和結構可以實作多個介面。 在下列範例中，類別 `EditBox` 可以實作 `IControl` 與 `IDataBound` 兩者。

```csharp
interface IDataBound
{
    void Bind(Binder b);
}

public class EditBox: IControl, IDataBound
{
    public void Paint() {...}
    public void Bind(Binder b) {...}
}
```
當類別或結構實作特定介面時，可以將該類別或結構的執行個體隱含地轉換為該介面型別。 例如

```csharp
EditBox editBox = new EditBox();
IControl control = editBox;
IDataBound dataBound = editBox;
```
在實作特定介面的執行個體無法確知的情況下，可以使用動態型別轉換。 例如，下列陳述式會使用動態型別轉換取得的物件`IControl`和`IDataBound`介面實作。 因為物件的實際型別是`EditBox`，轉型成功。

```csharp
object obj = new EditBox();
IControl control = (IControl)obj;
IDataBound dataBound = (IDataBound)obj;
```
在上一個`EditBox`類別，`Paint`方法，從`IControl`介面並`Bind`方法，從`IDataBound`使用實作介面`public`成員。 C# 也支援***明確介面成員實作***，利用這種類別或結構可以避免將成員設為`public`。 明確介面成員實作是使用介面成員完整名稱來撰寫。 例如，`EditBox` 類別可以使用明確介面成員實作來實作 `IControl.Paint` 和 `IDataBound.Bind` 方法，如下所示。

```csharp
public class EditBox: IControl, IDataBound
{
    void IControl.Paint() {...}
    void IDataBound.Bind(Binder b) {...}
}
```
明確介面成員只能透過介面型別來存取。 比方說，實作`IControl.Paint`提供先前`EditBox`可以只藉由設定轉換的第一個叫用類別`EditBox`參考`IControl`介面類型。

```csharp
EditBox editBox = new EditBox();
editBox.Paint();                        // Error, no such method
IControl control = editBox;
control.Paint();                        // Ok
```

## <a name="enums"></a>列舉

「列舉型別」是含一組具名常數的相異實值型別。 下列範例宣告，並使用名為列舉型別`Color`具有三個的常數值`Red`， `Green`，和`Blue`。

```csharp
using System;

enum Color
{
    Red,
    Green,
    Blue
}

class Test
{
    static void PrintColor(Color color) {
        switch (color) {
            case Color.Red:
                Console.WriteLine("Red");
                break;
            case Color.Green:
                Console.WriteLine("Green");
                break;
            case Color.Blue:
                Console.WriteLine("Blue");
                break;
            default:
                Console.WriteLine("Unknown color");
                break;
        }
    }

    static void Main() {
        Color c = Color.Red;
        PrintColor(c);
        PrintColor(Color.Blue);
    }
}
```
每個列舉類型都有對應的整數類資料類型，稱為***基礎型別***的列舉型別。 沒有明確宣告基礎型別的列舉型別都具有基礎類型的`int`。 列舉類型的儲存格式和可能的值範圍是其基礎類型所決定。 列舉成員不會受限於一組可對列舉類型的值。 特別的是，列舉的基礎類型的任何值可以轉換為列舉型別，並為該列舉型別的相異有效值。

下列範例宣告名為列舉型別`Alignment`的基礎類型`sbyte`。

```csharp
enum Alignment: sbyte
{
    Left = -1,
    Center = 0,
    Right = 1
}
```
如先前的範例所示，列舉成員宣告可以包含常數的運算式，指定成員的值。 每個列舉成員的常值必須是列舉的基礎類型的範圍中。 列舉成員宣告並未明確指定值，該成員指定的值 （如果它是列舉型別中的第一個成員） 的零或一加賦予上述的列舉成員的值。

列舉值可以轉換成整數值和相反的情況使用類型轉換。 例如

```csharp
int i = (int)Color.Blue;        // int i = 2;
Color c = (Color)2;             // Color c = Color.Blue;
```
任何列舉類型的預設值是整數值轉換為列舉型別為零。 在其中的變數會自動初始化為預設值的情況下，這是提供給列舉類型的變數的值。 預設值的列舉型別可供輕鬆，常值的順序`0`會隱含轉換成任何列舉型別。 因此，允許以下的情況。

```csharp
Color c = 0;
```

## <a name="delegates"></a>委派

「委派型別」代表對方法的參考，其中含有特定參數清單與傳回型別。 委派讓您可將方法視為實體，而實體能指派給變數或當作參數來傳遞。 委派就類似其他程式設計語言中的函式指標，但與函式指標的不同之處是，委派是物件導向且為型別安全。

下列範例會宣告並使用名為 `Function` 的委派型別。

```csharp
using System;

delegate double Function(double x);

class Multiplier
{
    double factor;

    public Multiplier(double factor) {
        this.factor = factor;
    }

    public double Multiply(double x) {
        return x * factor;
    }
}

class Test
{
    static double Square(double x) {
        return x * x;
    }

    static double[] Apply(double[] a, Function f) {
        double[] result = new double[a.Length];
        for (int i = 0; i < a.Length; i++) result[i] = f(a[i]);
        return result;
    }

    static void Main() {
        double[] a = {0.0, 0.5, 1.0};
        double[] squares = Apply(a, Square);
        double[] sines = Apply(a, Math.Sin);
        Multiplier m = new Multiplier(2.0);
        double[] doubles =  Apply(a, m.Multiply);
    }
}
```
`Function` 委派型別的執行個體可以參考任何採用 `double` 引數並傳回 `double` 值的方法。 `Apply`方法適用於給定`Function`的項目`double[]`，其中會傳回`double[]`結果。 在 `Main` 方法中，是使用 `Apply` 將三個不同的函式套用到 `double[]`。

委派可以參考靜態方法 (例如上一個範例中的 `Square` 或 `Math.Sin`)，或是參考執行個體方法 (例如上一個範例中的 `m.Multiply`)。 參考執行個體方法的委派也會參考特定的物件，而且透過委派來叫用執行個體方法時，該物件就會變成叫用中的 `this`。

您也可以使用匿名函式來建立委派，這些函式是動態建立的「內嵌方法」。 匿名函式可以看見周圍方法的區域變數。 因此，上面的乘數範例可以撰寫更輕鬆地沒有使用`Multiplier`類別：

```csharp
double[] doubles =  Apply(a, (double x) => x * 2.0);
```
委派有一個有趣且實用的屬性，就是它並不知道或並不在乎所參考方法的類別；唯一重要的是所參考的方法具有與委派相同的參數和傳回型別。

## <a name="attributes"></a>屬性

C# 程式中的型別、成員和其他實體支援控制其某方面行為的修飾詞。 例如，方法的協助工具是使用 `public`、`protected`、`internal` 和 `private` 修飾詞控制。 C# 將此能力一般化，宣告式資訊的使用者定義型別才能附加至程式實體，並在執行階段擷取。 程式是透過定義和使用***屬性***指定這項額外的宣告式資訊。

下列範例宣告的 `HelpAttribute` 屬性可置於程式實體，以提供其相關文件的連結。

```csharp
using System;

public class HelpAttribute: Attribute
{
    string url;
    string topic;

    public HelpAttribute(string url) {
        this.url = url;
    }

    public string Url {
        get { return url; }
    }

    public string Topic {
        get { return topic; }
        set { topic = value; }
    }
}
```
所有屬性的類別都衍生自`System.Attribute`基底.NET Framework 所提供的類別。 在相關聯的宣告之前，於方括弧中提供屬性的名稱 (及任何引數) 即可套用屬性。 如果屬性名稱的結尾`Attribute`，則參考該屬性時，就可以省略該部分的名稱。 例如，`HelpAttribute` 屬性可以下列方式使用。

```csharp
[Help("http://msdn.microsoft.com/.../MyClass.htm")]
public class Widget
{
    [Help("http://msdn.microsoft.com/.../MyClass.htm", Topic = "Display")]
    public void Display(string text) {}
}
```
這個範例會附加`HelpAttribute`要`Widget`類別，而另一個`HelpAttribute`到`Display`類別中的方法。 屬性類別的公用建構函式控制將屬性附加至程式實體時必須提供的資訊。 透過參考屬性類別的公用讀寫屬性可提供其他資訊 (例如先前對 `Topic` 的參考 )。

下列範例示範如何擷取指定的程式實體的屬性資訊，在執行階段使用反映。

```csharp
using System;
using System.Reflection;

class Test
{
    static void ShowHelp(MemberInfo member) {
        HelpAttribute a = Attribute.GetCustomAttribute(member,
            typeof(HelpAttribute)) as HelpAttribute;
        if (a == null) {
            Console.WriteLine("No help for {0}", member);
        }
        else {
            Console.WriteLine("Help for {0}:", member);
            Console.WriteLine("  Url={0}, Topic={1}", a.Url, a.Topic);
        }
    }

    static void Main() {
        ShowHelp(typeof(Widget));
        ShowHelp(typeof(Widget).GetMethod("Display"));
    }
}
```
透過反射要求特定的屬性時，會以程式來源中提供的資訊叫用屬性類別的建構函式，並傳回產生的屬性執行個體。 如果是透過屬性提供其他資訊，傳回屬性執行個體之前，這些屬性會設為指定的值。
