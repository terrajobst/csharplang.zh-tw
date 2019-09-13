---
ms.openlocfilehash: adf81842e3c763c7bbdd3f10bb884dc1207b9099
ms.sourcegitcommit: 0489cb64b7dfb328813d757f4d447a15b85a5851
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/11/2019
ms.locfileid: "70912428"
---
# <a name="documentation-comments"></a>文件註解

C#提供一種機制，讓程式設計人員使用包含 XML 文字的特殊批註語法來記錄其程式碼。 在原始程式碼檔中，具有特定形式的批註可用來指示工具從這些批註產生 XML，並將它們放在其前面的原始程式碼專案。 使用這種語法的批註稱為***檔批註***。 它們必須緊接在使用者定義型別（例如類別、委派或介面）或成員（例如欄位、事件、屬性或方法）前面。 XML 產生工具稱為***檔***產生器。 （此產生器可能是C#編譯器本身，但不需要）。檔產生器所產生的輸出稱為***檔***檔。 檔集是用來做為***文檔檢視器***的輸入;一種工具，目的是要產生某種類型資訊和其相關檔的視覺化顯示方式。

此規格建議要用於檔批註中的一組標記，但不需要使用這些標記，而且只要遵循格式正確之 XML 的規則，就可以使用其他標記。

## <a name="introduction"></a>簡介

具有特殊形式的批註，可以用來指示工具從這些批註產生 XML，並將它們放在前面的原始程式碼元素。 這類批註是以三個斜線（`///`）開頭的單行批註，或是以斜線和二顆星（`/**`）開頭的分隔批註。 它們必須緊接在使用者定義型別（例如類別、委派或介面）或其標注的成員（例如欄位、事件、屬性或方法）前面。 屬性區段（[屬性規格](attributes.md#attribute-specification)）會視為宣告的一部分，因此檔批註必須在套用至類型或成員的屬性之前。

__語法：__

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

在*single_line_doc_comment*中，如果在與目前*single_line_doc_comment*相鄰之每`///`個*single_line_doc_comment*的字元後面*有一個空白字元*，則該*空白字元不*會包含在 XML 輸出中。

在分隔的-doc 批註中，如果第二行的第一個非空白字元是星號，而選擇性空白字元的模式相同，則會在分隔-doc 批註的每一行開頭重複一個星號字元，然後，重複模式的字元不會包含在 XML 輸出中。 此模式可能會在星號字元後面加上空白字元。

__範例:__

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>
///
public class Point 
{
    /// <summary>method <c>draw</c> renders the point.</summary>
    void draw() {...}
}
```

檔批註中的文字必須根據 XML 的規則（ https://www.w3.org/TR/REC-xml) ）正確地進行格式化。 如果 XML 格式不正確，則會產生警告，而且檔檔案會包含批註，指出發生錯誤。

雖然開發人員可以自由建立自己的標籤集，[但建議的標籤中會](documentation-comments.md#recommended-tags)定義建議的集合。 其中一些建議的標記具有特殊意義：

*  `<param>`標記是用來描述參數。 如果使用這類標記，檔產生器必須確認指定的參數是否存在，以及檔批註中是否有所有參數的描述。 如果這類驗證失敗，檔產生器會發出警告。
*  `cref` 屬性可以附加至任何標記，以提供程式碼項目的參考。 檔產生器必須確認此程式碼元素存在。 如果驗證失敗，檔產生器會發出警告。 尋找`cref`屬性中所描述的名稱時，檔產生器必須`using`根據原始程式碼中出現的語句，遵循命名空間的可見度。 若為泛型的程式碼專案，則無法使用一般泛型語法（也`List<T>`就是 ""），因為它會產生不正確 XML。 括弧可以用來取代方括弧（也就是 "`List{T}`"），也可以使用 XML escape 語法（也就是 "`List&lt;T&gt;`"）。
*  `<summary>`標記可供檔檢視器用來顯示類型或成員的其他資訊。
*  `<include>`標記包含來自外部 XML 檔案的資訊。

請注意，檔檔案不會提供有關類型和成員的完整資訊（例如，它不包含任何類型資訊）。 若要取得類型或成員的相關資訊，檔檔案必須與實際類型或成員的反映搭配使用。

## <a name="recommended-tags"></a>建議的標記

檔產生器必須接受並根據 XML 規則來處理任何有效的標記。 下列標記提供使用者檔中常用的功能。 （當然，還有其他標記可能）。


| __標記__          | __區段__                                            | __目的__                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | 使用類似程式碼的字型來設定文字                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | 設定一或多行原始程式碼或程式輸出 |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | 表示範例                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | 識別方法可以擲回的例外狀況           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | 包含來自外部檔案的 XML                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | 建立清單或資料表                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | 允許將結構新增至文字                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | 描述方法或函數的參數       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | 識別單字為參數名稱               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | 記錄成員的安全性存取範圍        |
| `<remarks>`      | [`<remarks>`](documentation-comments.md#remarks)       | 描述類型的其他資訊           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | 描述方法的傳回值                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | 指定連結                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | 產生另請參閱專案                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | 描述類型或類型的成員                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | 描述屬性                                    |
| `<typeparam>`    |                                                        | 描述泛型型別參數                      |
| `<typeparamref>` |                                                        | 識別單字是類型參數名稱          |

### `<c>`

這個標記會提供一個機制，指出描述中的文字片段應設定為特殊字型，例如用於程式碼區塊的。 針對實際程式碼的行， `<code>`請[`<code>`](documentation-comments.md#code)使用（）。

__語法：__

```xml
<c>text</c>
```

__範例:__

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>

public class Point 
{
    // ...
}
```

### `<code>`

此標記用來以某種特殊字型設定一或多行原始程式碼或程式輸出。 若是敘述中的小型程式碼片段， `<c>`請[`<c>`](documentation-comments.md#c)使用（）。

__語法：__

```xml
<code>source code or program output</code>
```

__範例:__

```csharp
/// <summary>This method changes the point's location by
///    the given x- and y-offsets.
/// <example>For example:
/// <code>
///    Point p = new Point(3,5);
///    p.Translate(-1,3);
/// </code>
/// results in <c>p</c>'s having the value (2,8).
/// </example>
/// </summary>

public void Translate(int xor, int yor) {
    X += xor;
    Y += yor;
}   
```

### `<example>`

此標記允許批註中的範例程式碼，以指定方法或其他程式庫成員的使用方式。 一般來說，這也會牽涉到使用標記`<code>` （[`<code>`](documentation-comments.md#code)）。

__語法：__

```xml
<example>description</example>
```

__範例:__

如`<code>`需[`<code>`](documentation-comments.md#code)範例，請參閱（）。

### `<exception>`

此標記提供一種方式來記載方法可以擲回的例外狀況。

__語法：__

```xml
<exception cref="member">description</exception>
```

其中

* `member`這是成員的名稱。 檔產生器會檢查指定的成員是否存在， `member`並轉譯為檔檔中的標準專案名稱。
* `description`這是擲回例外狀況的情況描述。

__範例:__

```csharp
public class DataBaseOperations
{
    /// <exception cref="MasterFileFormatCorruptException"></exception>
    /// <exception cref="MasterFileLockedOpenException"></exception>
    public static void ReadRecord(int flag) {
        if (flag == 1)
            throw new MasterFileFormatCorruptException();
        else if (flag == 2)
            throw new MasterFileLockedOpenException();
        // ...
    } 
}
```

### `<include>`

此標籤可讓您從原始程式碼檔外部的 XML 檔中包含資訊。 外部檔案必須是格式正確的 XML 檔，而且會套用 XPath 運算式來指定要包含在該檔中的 XML。 然後會使用外部檔中選取的 XML 來取代標記。`<include>`

__語法：__

```
<include file="filename" path="xpath" />
```

其中

* `filename`這是外部 XML 檔案的檔案名。 檔案名的解讀方式相對於包含 include 標記的檔案。
* `xpath`是一種 XPath 運算式，可選取外部 XML 檔案中的部分 XML。

__範例:__

如果原始程式碼包含如下的宣告：

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

而外部檔案 "檔 .xml" 具有下列內容：

```xml
<?xml version="1.0"?>
<extradoc>
  <class name="IntList">
     <summary>
        Contains a list of integers.
     </summary>
  </class>
  <class name="StringList">
     <summary>
        Contains a list of integers.
     </summary>
  </class>
</extradoc>
```

然後，相同的檔會輸出，如同原始程式碼包含：

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

此標記用來建立專案的清單或資料表。 它可能包含`<listheader>`區塊，以定義資料表或定義清單的標題資料列。 （定義資料表時，只需要提供標題`term`中的專案。）

清單中的每個專案都是以`<item>`區塊來指定。 建立定義清單時，必須同時`term`指定`description`和。 不過，如果是資料表、項目符號清單或編號清單，則`description`只需要指定。

__語法：__

```xml
<list type="bullet" | "number" | "table">
   <listheader>
      <term>term</term>
      <description>*description*</description>
   </listheader>
   <item>
      <term>term</term>
      <description>*description*</description>
   </item>
    ...
   <item>
      <term>term</term>
      <description>description</description>
   </item>
</list>
```

其中

* `term`這是要定義的詞彙，其定義在`description`中。
* `description`是專案符號或編號清單中的專案，或的定義`term`。

__範例:__

```csharp
public class MyClass
{
    /// <summary>Here is an example of a bulleted list:
    /// <list type="bullet">
    /// <item>
    /// <description>Item 1.</description>
    /// </item>
    /// <item>
    /// <description>Item 2.</description>
    /// </item>
    /// </list>
    /// </summary>
    public static void Main () {
        // ...
    }
}
```

### `<para>`

此標記可用於其他標記（ `<summary>`例如（[`<remarks>`](documentation-comments.md#remarks)）或`<returns>` （[`<returns>`](documentation-comments.md#returns)）），並允許將結構新增至文字。

__語法：__

```xml
<para>content</para>
```

其中`content`是段落的文字。

__範例:__

```csharp
/// <summary>This is the entry point of the Point class testing program.
/// <para>This program tests each method and operator, and
/// is intended to be run after any non-trivial maintenance has
/// been performed on the Point class.</para></summary>
public static void Main() {
    // ...
}
```

### `<param>`

這個標記是用來描述方法、函數或索引子的參數。

__語法：__

```xml
<param name="name">description</param>
```

其中

* `name`這是參數的名稱。
* `description`這是參數的描述。

__範例:__

```csharp
/// <summary>This method changes the point's location to
///    the given coordinates.</summary>
/// <param name="xor">the new x-coordinate.</param>
/// <param name="yor">the new y-coordinate.</param>
public void Move(int xor, int yor) {
    X = xor;
    Y = yor;
}
```

### `<paramref>`

此標記用來表示單字是參數。 您可以處理檔檔案，以某種不同的方式來格式化此參數。

__語法：__

```xml
<paramref name="name"/>
```

其中`name`是參數的名稱。

__範例:__

```csharp
/// <summary>This constructor initializes the new Point to
///    (<paramref name="xor"/>,<paramref name="yor"/>).</summary>
/// <param name="xor">the new Point's x-coordinate.</param>
/// <param name="yor">the new Point's y-coordinate.</param>

public Point(int xor, int yor) {
    X = xor;
    Y = yor;
}
```

### `<permission>`

此標記允許記錄成員的安全性存取範圍。

__語法：__

```xml
<permission cref="member">description</permission>
```

其中

* `member`這是成員的名稱。 檔產生器會檢查指定的程式碼專案是否存在，並將*成員*轉譯為檔檔中的標準專案名稱。
* `description`這是成員存取權的描述。

__範例:__

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remarks>`

此標記用來指定類型的額外資訊。 （使用`<summary>` （[`<summary>`](documentation-comments.md#summary)）來描述類型本身和類型的成員）。

__語法：__

```xml
<remarks>description</remarks>
```

其中`description`是批註的文字。

__範例:__

```csharp
/// <summary>Class <c>Point</c> models a point in a 
/// two-dimensional plane.</summary>
/// <remarks>Uses polar coordinates</remarks>
public class Point 
{
    // ...
}
```

### `<returns>`

這個標記是用來描述方法的傳回值。

__語法：__

```xml
<returns>description</returns>
```

其中`description` ，是傳回值的描述。

__範例:__

```csharp
/// <summary>Report a point's location as a string.</summary>
/// <returns>A string representing a point's location, in the form (x,y),
///    without any leading, trailing, or embedded whitespace.</returns>
public override string ToString() {
    return "(" + X + "," + Y + ")";
}
```

### `<see>`

此標記允許在文字內指定連結。 使用`<seealso>` （[`<seealso>`](documentation-comments.md#seealso)）來表示要出現在 [另請參閱] 區段中的文字。

__語法：__

```xml
<see cref="member"/>
```

其中`member`是成員的名稱。 檔產生器會檢查指定的程式碼專案是否存在，並將*成員*變更為所產生檔檔案中的元素名稱。

__範例:__

```csharp
/// <summary>This method changes the point's location to
///    the given coordinates.</summary>
/// <see cref="Translate"/>
public void Move(int xor, int yor) {
    X = xor;
    Y = yor;
}

/// <summary>This method changes the point's location by
///    the given x- and y-offsets.
/// </summary>
/// <see cref="Move"/>
public void Translate(int xor, int yor) {
    X += xor;
    Y += yor;
}
```

### `<seealso>`

此標記允許針對 [另請參閱] 區段產生專案。 使用`<see>` [（`<see>`](documentation-comments.md#see)）可指定文字內的連結。

__語法：__

```xml
<seealso cref="member"/>
```

其中`member`是成員的名稱。 檔產生器會檢查指定的程式碼專案是否存在，並將*成員*變更為所產生檔檔案中的元素名稱。

__範例:__

```csharp
/// <summary>This method determines whether two Points have the same
///    location.</summary>
/// <seealso cref="operator=="/>
/// <seealso cref="operator!="/>
public override bool Equals(object o) {
    // ...
}
```

### `<summary>`

這個標記可以用來描述類型或類型的成員。 使用`<remarks>` [（`<remarks>`](documentation-comments.md#remarks)）來描述類型本身。

__語法：__

```xml
<summary>description</summary>
```

其中`description`是類型或成員的摘要。

__範例:__

```csharp
/// <summary>This constructor initializes the new Point to (0,0).</summary>
public Point() : this(0,0) {
}
```

### `<value>`

此標記可讓您描述屬性。

__語法：__

```xml
<value>property description</value>
```

其中`property description`是屬性的描述。

__範例:__

```csharp
/// <value>Property <c>X</c> represents the point's x-coordinate.</value>
public int X
{
    get { return x; }
    set { x = value; }
}
```

### `<typeparam>`

這個標記是用來描述類別、結構、介面、委派或方法的泛型型別參數。

__語法：__

```xml
<typeparam name="name">description</typeparam>
```

其中`name` ，是類型參數的名稱，而`description`是其描述。

__範例:__

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

這個標記是用來表示單字是類型參數。 您可以處理檔檔案，以某種不同的方式來格式化此類型參數。

__語法：__

```xml
<typeparamref name="name"/>
```

其中`name`是類型參數的名稱。

__範例:__

```csharp
/// <summary>This method fetches data and returns a list of <typeparamref name="T"/>.</summary>
/// <param name="query">query to execute</param>
public List<T> FetchData<T>(string query) {
    ...
}
```

## <a name="processing-the-documentation-file"></a>處理檔檔案

檔產生器會針對原始程式碼中以檔註解標記的每個元素，產生一個識別碼字串。 此識別碼字串可唯一識別來源元素。 檔檢視器可以使用識別碼字串，來識別檔適用的對應中繼資料/反映專案。

檔檔案不是原始程式碼的階層式標記法，相反地，它是包含每個元素所產生之識別碼字串的一般清單。

### <a name="id-string-format"></a>識別碼字串格式

當產生識別碼字串時，檔產生器會遵循下列規則：

*  字串中未放置任何空白字元。

*  字串的第一個部分會識別要記載的成員種類，方法是透過單一字元，後面接著冒號。 定義了下列種類的成員：

   | __字元__ | __描述__                                             |
   |---------------|-------------------------------------------------------------|
   | E             | Event - 事件                                                       |
   | F             | 欄位                                                       |
   | M             | 方法（包括函式、析構函數和運算子） |
   | N             | 命名空間                                                   |
   | P             | 屬性（包括索引子）                               |
   | T             | 類型（例如類別、委派、列舉、介面和結構） |
   | !             | 錯誤字串;字串的其餘部分會提供錯誤的相關資訊。 例如，檔產生器會針對無法解析的連結產生錯誤資訊。 |

*  字串的第二個部分是專案的完整名稱，從命名空間的根目錄開始。 元素的名稱、其封入類型及命名空間會以句號分隔。 如果專案的名稱具有句點，則會以`#(U+0023)`字元取代。 （假設沒有任何元素的名稱中有這個字元）。
*  對於具有引數的方法和屬性，引數清單會遵循，並以括弧括住。 如果沒有引數，則會省略括弧。 引數會以逗號分隔。 每個引數的編碼方式與 CLI 簽章相同，如下所示：
   *  引數是以其完整名稱為基礎的檔案名稱表示，如下所示：
      * 代表泛型型別的引數具有附加`` ` ``的（倒引號）字元，後面接著類型參數的數目
      * `out`具有或`ref`修飾詞的引數`@`具有下列型別名稱。 以傳值方式或 via `params`傳遞的引數沒有特殊的標記法。
      * 當做陣列的引數會以`[lowerbound:size, ... , lowerbound:size]`逗號數目小於一的位置表示，而每個維度的下限和大小（如果已知的話）則以十進位表示。 如果未指定下限或大小，則會省略。 如果省略特定維度的下限和大小， `:`也會省略。 不規則陣列是以每個`[]`層級的一個來表示。
      * 具有 void 以外之指標類型的引數會使用`*`下列類型名稱來表示。 Void 指標是使用的型別名稱來`System.Void`表示。
      * 參考在型別上定義之泛型型別參數的引數`` ` `` ，是使用（倒引號）字元來編碼，後面接著類型參數的以零為起始的索引。
      * 使用方法中定義的泛型型別參數的引數會使用``` `` ```雙重倒引號， `` ` ``而不是用於類型的。
      * 參考結構化泛型型別的引數會使用泛型型別來編碼， `{`後面接著，後面接著以逗號分隔的類型引數清單， `}`後面接著。

### <a name="id-string-examples"></a>識別碼字串範例

下列範例會顯示一段程式C#代碼，以及從每個來源元素產生的識別碼字串，可以有檔批註：

*  類型是使用其完整名稱來表示，並以一般資訊增強：

   ```csharp
   enum Color { Red, Blue, Green }

   namespace Acme
   {
       interface IProcess {...}

       struct ValueType {...}

       class Widget: IProcess
       {
           public class NestedClass {...}
           public interface IMenuItem {...}
           public delegate void Del(int i);
           public enum Direction { North, South, East, West }
       }

       class MyList<T>
       {
           class Helper<U,V> {...}
       }
   }

   "T:Color"
   "T:Acme.IProcess"
   "T:Acme.ValueType"
   "T:Acme.Widget"
   "T:Acme.Widget.NestedClass"
   "T:Acme.Widget.IMenuItem"
   "T:Acme.Widget.Del"
   "T:Acme.Widget.Direction"
   "T:Acme.MyList`1"
   "T:Acme.MyList`1.Helper`2"
   ```

*  欄位是以其完整名稱來表示：

   ```csharp
   namespace Acme
   {
       struct ValueType
       {
           private int total;
       }
   
       class Widget: IProcess
       {
           public class NestedClass
           {
               private int value;
           }
   
           private string message;
           private static Color defaultColor;
           private const double PI = 3.14159;
           protected readonly double monthlyAverage;
           private long[] array1;
           private Widget[,] array2;
           private unsafe int *pCount;
           private unsafe float **ppValues;
       }
   }

   "F:Acme.ValueType.total"
   "F:Acme.Widget.NestedClass.value"
   "F:Acme.Widget.message"
   "F:Acme.Widget.defaultColor"
   "F:Acme.Widget.PI"
   "F:Acme.Widget.monthlyAverage"
   "F:Acme.Widget.array1"
   "F:Acme.Widget.array2"
   "F:Acme.Widget.pCount"
   "F:Acme.Widget.ppValues"
   ```

*  建構函式。

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           static Widget() {...}
           public Widget() {...}
           public Widget(string s) {...}
       }
   }

   "M:Acme.Widget.#cctor"
   "M:Acme.Widget.#ctor"
   "M:Acme.Widget.#ctor(System.String)"
   ```

*  函數.

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           ~Widget() {...}
       }
   }
   
   "M:Acme.Widget.Finalize"
   ```

*  方法.

   ```csharp
   namespace Acme
   {
       struct ValueType
       {
           public void M(int i) {...}
       }

       class Widget: IProcess
       {
           public class NestedClass
           {
               public void M(int i) {...}
           }

           public static void M0() {...}
           public void M1(char c, out float f, ref ValueType v) {...}
           public void M2(short[] x1, int[,] x2, long[][] x3) {...}
           public void M3(long[][] x3, Widget[][,,] x4) {...}
           public unsafe void M4(char *pc, Color **pf) {...}
           public unsafe void M5(void *pv, double *[][,] pd) {...}
           public void M6(int i, params object[] args) {...}
       }

       class MyList<T>
       {
           public void Test(T t) { }
       }

       class UseList
       {
           public void Process(MyList<int> list) { }
           public MyList<T> GetValues<T>(T inputValue) { return null; }
       }
   }

   "M:Acme.ValueType.M(System.Int32)"
   "M:Acme.Widget.NestedClass.M(System.Int32)"
   "M:Acme.Widget.M0"
   "M:Acme.Widget.M1(System.Char,System.Single@,Acme.ValueType@)"
   "M:Acme.Widget.M2(System.Int16[],System.Int32[0:,0:],System.Int64[][])"
   "M:Acme.Widget.M3(System.Int64[][],Acme.Widget[0:,0:,0:][])"
   "M:Acme.Widget.M4(System.Char*,Color**)"
   "M:Acme.Widget.M5(System.Void*,System.Double*[0:,0:][])"
   "M:Acme.Widget.M6(System.Int32,System.Object[])"
   "M:Acme.MyList`1.Test(`0)"
   "M:Acme.UseList.Process(Acme.MyList{System.Int32})"
   "M:Acme.UseList.GetValues``(``0)"
   ```

*  屬性和索引子。

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public int Width { get {...} set {...} }
           public int this[int i] { get {...} set {...} }
           public int this[string s, int i] { get {...} set {...} }
       }
   }

   "P:Acme.Widget.Width"
   "P:Acme.Widget.Item(System.Int32)"
   "P:Acme.Widget.Item(System.String,System.Int32)"
   ```

*  事件.

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public event Del AnEvent;
       }
   }

   "E:Acme.Widget.AnEvent"
   ```

*  一元運算子。

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public static Widget operator+(Widget x) {...}
       }
   }

   "M:Acme.Widget.op_UnaryPlus(Acme.Widget)"
   ```

   使用的`op_UnaryPlus`一組完整一元運算子函數名稱如下：、 `op_False` `op_Decrement` `op_LogicalNot` `op_OnesComplement` `op_UnaryNegation` `op_Increment`、、、、 、和。`op_True`

*  二元運算子。

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public static Widget operator+(Widget x1, Widget x2) {...}
       }
   }

   "M:Acme.Widget.op_Addition(Acme.Widget,Acme.Widget)"
   ```

   使用的一組完整二元運算子函式名稱如下所示`op_Addition`： `op_Subtraction`、 `op_Multiply` `op_BitwiseOr`、 `op_Division`、 `op_Modulus`、 `op_BitwiseAnd`、 `op_ExclusiveOr` `op_LeftShift` `op_RightShift`、、、、、`op_Equality`、 、、`op_LessThan`、和`op_GreaterThan`。 `op_LessThanOrEqual` `op_Inequality` `op_GreaterThanOrEqual`

*  轉換運算子的尾端是 "`~`"，後面接著傳回型別。

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public static explicit operator int(Widget x) {...}
           public static implicit operator long(Widget x) {...}
       }
   }

   "M:Acme.Widget.op_Explicit(Acme.Widget)~System.Int32"
   "M:Acme.Widget.op_Implicit(Acme.Widget)~System.Int64"
   ```

## <a name="an-example"></a>範例

### <a name="c-source-code"></a>C#原始程式碼

下列範例顯示`Point`類別的原始程式碼：

```csharp
namespace Graphics
{

/// <summary>Class <c>Point</c> models a point in a two-dimensional plane.
/// </summary>
public class Point 
{

    /// <summary>Instance variable <c>x</c> represents the point's
    ///    x-coordinate.</summary>
    private int x;

    /// <summary>Instance variable <c>y</c> represents the point's
    ///    y-coordinate.</summary>
    private int y;

    /// <value>Property <c>X</c> represents the point's x-coordinate.</value>
    public int X
    {
        get { return x; }
        set { x = value; }
    }

    /// <value>Property <c>Y</c> represents the point's y-coordinate.</value>
    public int Y
    {
        get { return y; }
        set { y = value; }
    }

    /// <summary>This constructor initializes the new Point to
    ///    (0,0).</summary>
    public Point() : this(0,0) {}

    /// <summary>This constructor initializes the new Point to
    ///    (<paramref name="xor"/>,<paramref name="yor"/>).</summary>
    /// <param><c>xor</c> is the new Point's x-coordinate.</param>
    /// <param><c>yor</c> is the new Point's y-coordinate.</param>
    public Point(int xor, int yor) {
        X = xor;
        Y = yor;
    }

    /// <summary>This method changes the point's location to
    ///    the given coordinates.</summary>
    /// <param><c>xor</c> is the new x-coordinate.</param>
    /// <param><c>yor</c> is the new y-coordinate.</param>
    /// <see cref="Translate"/>
    public void Move(int xor, int yor) {
        X = xor;
        Y = yor;
    }

    /// <summary>This method changes the point's location by
    ///    the given x- and y-offsets.
    /// <example>For example:
    /// <code>
    ///    Point p = new Point(3,5);
    ///    p.Translate(-1,3);
    /// </code>
    /// results in <c>p</c>'s having the value (2,8).
    /// </example>
    /// </summary>
    /// <param><c>xor</c> is the relative x-offset.</param>
    /// <param><c>yor</c> is the relative y-offset.</param>
    /// <see cref="Move"/>
    public void Translate(int xor, int yor) {
        X += xor;
        Y += yor;
    }

    /// <summary>This method determines whether two Points have the same
    ///    location.</summary>
    /// <param><c>o</c> is the object to be compared to the current object.
    /// </param>
    /// <returns>True if the Points have the same location and they have
    ///    the exact same type; otherwise, false.</returns>
    /// <seealso cref="operator=="/>
    /// <seealso cref="operator!="/>
    public override bool Equals(object o) {
        if (o == null) {
            return false;
        }

        if (this == o) {
            return true;
        }

        if (GetType() == o.GetType()) {
            Point p = (Point)o;
            return (X == p.X) && (Y == p.Y);
        }
        return false;
    }

    /// <summary>Report a point's location as a string.</summary>
    /// <returns>A string representing a point's location, in the form (x,y),
    ///    without any leading, training, or embedded whitespace.</returns>
    public override string ToString() {
        return "(" + X + "," + Y + ")";
    }

    /// <summary>This operator determines whether two Points have the same
    ///    location.</summary>
    /// <param><c>p1</c> is the first Point to be compared.</param>
    /// <param><c>p2</c> is the second Point to be compared.</param>
    /// <returns>True if the Points have the same location and they have
    ///    the exact same type; otherwise, false.</returns>
    /// <seealso cref="Equals"/>
    /// <seealso cref="operator!="/>
    public static bool operator==(Point p1, Point p2) {
        if ((object)p1 == null || (object)p2 == null) {
            return false;
        }

        if (p1.GetType() == p2.GetType()) {
            return (p1.X == p2.X) && (p1.Y == p2.Y);
        }

        return false;
    }

    /// <summary>This operator determines whether two Points have the same
    ///    location.</summary>
    /// <param><c>p1</c> is the first Point to be compared.</param>
    /// <param><c>p2</c> is the second Point to be compared.</param>
    /// <returns>True if the Points do not have the same location and the
    ///    exact same type; otherwise, false.</returns>
    /// <seealso cref="Equals"/>
    /// <seealso cref="operator=="/>
    public static bool operator!=(Point p1, Point p2) {
        return !(p1 == p2);
    }

    /// <summary>This is the entry point of the Point class testing
    /// program.
    /// <para>This program tests each method and operator, and
    /// is intended to be run after any non-trivial maintenance has
    /// been performed on the Point class.</para></summary>
    public static void Main() {
        // class test code goes here
    }
}
}
```

### <a name="resulting-xml"></a>產生的 XML

以下是一個檔產生器在提供類別`Point`的原始程式碼時所產生的輸出，如上所示：

```xml
<?xml version="1.0"?>
<doc>
    <assembly>
        <name>Point</name>
    </assembly>
    <members>
        <member name="T:Graphics.Point">
            <summary>Class <c>Point</c> models a point in a two-dimensional
            plane.
            </summary>
        </member>

        <member name="F:Graphics.Point.x">
            <summary>Instance variable <c>x</c> represents the point's
            x-coordinate.</summary>
        </member>

        <member name="F:Graphics.Point.y">
            <summary>Instance variable <c>y</c> represents the point's
            y-coordinate.</summary>
        </member>

        <member name="M:Graphics.Point.#ctor">
            <summary>This constructor initializes the new Point to
        (0,0).</summary>
        </member>

        <member name="M:Graphics.Point.#ctor(System.Int32,System.Int32)">
            <summary>This constructor initializes the new Point to
            (<paramref name="xor"/>,<paramref name="yor"/>).</summary>
            <param><c>xor</c> is the new Point's x-coordinate.</param>
            <param><c>yor</c> is the new Point's y-coordinate.</param>
        </member>

        <member name="M:Graphics.Point.Move(System.Int32,System.Int32)">
            <summary>This method changes the point's location to
            the given coordinates.</summary>
            <param><c>xor</c> is the new x-coordinate.</param>
            <param><c>yor</c> is the new y-coordinate.</param>
            <see cref="M:Graphics.Point.Translate(System.Int32,System.Int32)"/>
        </member>

        <member
            name="M:Graphics.Point.Translate(System.Int32,System.Int32)">
            <summary>This method changes the point's location by
            the given x- and y-offsets.
            <example>For example:
            <code>
            Point p = new Point(3,5);
            p.Translate(-1,3);
            </code>
            results in <c>p</c>'s having the value (2,8).
            </example>
            </summary>
            <param><c>xor</c> is the relative x-offset.</param>
            <param><c>yor</c> is the relative y-offset.</param>
            <see cref="M:Graphics.Point.Move(System.Int32,System.Int32)"/>
        </member>

        <member name="M:Graphics.Point.Equals(System.Object)">
            <summary>This method determines whether two Points have the same
            location.</summary>
            <param><c>o</c> is the object to be compared to the current
            object.
            </param>
            <returns>True if the Points have the same location and they have
            the exact same type; otherwise, false.</returns>
            <seealso
      cref="M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)"/>
            <seealso
      cref="M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)"/>
        </member>

        <member name="M:Graphics.Point.ToString">
            <summary>Report a point's location as a string.</summary>
            <returns>A string representing a point's location, in the form
            (x,y),
            without any leading, training, or embedded whitespace.</returns>
        </member>

        <member
       name="M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)">
            <summary>This operator determines whether two Points have the
            same
            location.</summary>
            <param><c>p1</c> is the first Point to be compared.</param>
            <param><c>p2</c> is the second Point to be compared.</param>
            <returns>True if the Points have the same location and they have
            the exact same type; otherwise, false.</returns>
            <seealso cref="M:Graphics.Point.Equals(System.Object)"/>
            <seealso
     cref="M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)"/>
        </member>

        <member
      name="M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)">
            <summary>This operator determines whether two Points have the
            same
            location.</summary>
            <param><c>p1</c> is the first Point to be compared.</param>
            <param><c>p2</c> is the second Point to be compared.</param>
            <returns>True if the Points do not have the same location and
            the
            exact same type; otherwise, false.</returns>
            <seealso cref="M:Graphics.Point.Equals(System.Object)"/>
            <seealso
      cref="M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)"/>
        </member>

        <member name="M:Graphics.Point.Main">
            <summary>This is the entry point of the Point class testing
            program.
            <para>This program tests each method and operator, and
            is intended to be run after any non-trivial maintenance has
            been performed on the Point class.</para></summary>
        </member>

        <member name="P:Graphics.Point.X">
            <value>Property <c>X</c> represents the point's
            x-coordinate.</value>
        </member>

        <member name="P:Graphics.Point.Y">
            <value>Property <c>Y</c> represents the point's
            y-coordinate.</value>
        </member>
    </members>
</doc>
```
