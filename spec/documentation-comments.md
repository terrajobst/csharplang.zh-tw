# <a name="documentation-comments"></a>文件註解

C# 提供他們所使用的特殊註解語法包含 XML 文字的程式碼的文件的程式設計人員的機制。 在原始程式碼檔案中，具有一種特定形式的註解可用來指示要從這些註解和來源的程式碼項目，它們在前面產生的 XML 工具。 註解使用這種語法會呼叫***文件註解***。 後面必須緊接著使用者定義類型 （例如類別、 委派或介面） 或成員 （例如欄位、 事件、 屬性或方法）。 XML 產生工具會呼叫***文件產生器***。 （這個產生器可能是的但不是需要 C# 編譯器本身）。文件產生器所產生的輸出稱為***文件檔案***。 文件檔案做為輸入***文件檢視器***; 要產生某種類型的類型資訊和其相關聯的文件的視覺顯示的工具。

此規格的建議一組標記用於文件註解，這些標記的使用不必要的但其他標記可用如有需要，做為長時間的格式正確的 XML 規則遵循。

## <a name="introduction"></a>簡介

具有一種特殊形式的註解可以用來指示要從這些註解和來源的程式碼項目，它們在前面產生的 XML 工具。 這類的註解會啟動具有三個斜線的單行註解 (`///`)，或分隔註解開頭為斜線和兩顆星 (`/**`)。 後面必須緊接著使用者定義型別 （例如類別、 委派或介面） 或加註的成員 （例如欄位、 事件、 屬性或方法）。 屬性區段 ([屬性規格](attributes.md#attribute-specification)) 會視為宣告的一部分，因此文件註解前面必須加上套用至類型或成員的屬性。

__語法：__

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

在  *single_line_doc_comment*，如果沒有*空白字元*之後的字元`///`上每個字元*single_line_doc_comment*相鄰的 s至目前*single_line_doc_comment*，，然後*空白字元*XML 輸出中不包含字元。

分隔-文件-註解中，如果在第二行的第一個非白格字元星號和選擇性空白字元和星號字元相同的模式重複出現的每個分隔-文件-註解內的行開頭然後重複模式的字元不會包含在 XML 輸出。 模式之後，以及在星號字元之前，可能包含空白的字元。

__範例：__

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

內文件註解的文字必須根據規則的 XML 格式正確 (http://www.w3.org/TR/REC-xml)。 如果 XML 的格式不正確，則會產生警告，且文件檔案會包含註解指出發現錯誤。

雖然開發人員可以自由建立自己的標記集，在定義一組建議[建議標記](documentation-comments.md#recommended-tags)。 其中一些建議的標記具有特殊意義：

*  `<param>`標記用來描述參數。 如果使用這類標記，則文件產生器必須確認指定的參數存在，而且文件註解，說明所有的參數。 如果這類驗證失敗，文件產生器就會發出警告。
*  `cref` 屬性可以附加至任何標記，以提供程式碼項目的參考。 文件產生器必須確認此程式碼項目存在。 如果驗證失敗時，文件產生器就會發出警告。 當尋找名稱中所述`cref`屬性，文件產生器必須遵守命名空間的可見性根據`using`的原始程式碼中出現的陳述式。 程式碼項目都是泛型，正常的一般語法 (亦即 「`List<T>`") 無法使用，因為它會產生無效的 XML。 使用大括號來取代方括號 (即"`List{T}`")，也可以使用 XML 逸出語法 (即"`List&lt;T&gt;`」)。
*  `<summary>`標記要供文件檢視器來顯示類型或成員的其他資訊。
*  `<include>`標記包含從外部 XML 檔的資訊。

請小心注意文件檔案不會提供型別和成員的完整資訊 （例如，不包含任何類型資訊）。 若要取得這類的型別或成員的相關資訊，則必須使用搭配反映實際的型別或成員的文件檔案。

## <a name="recommended-tags"></a>建議的標記

文件產生器必須接受並處理任何標記，來根據規則的 XML 有效。 下列標記提供使用者文件中的常用的功能。 （當然，可能還有其他標記。）


| __標記__          | __區段__                                            | __目的__                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | 設定文字，以類似的程式碼的字型                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | 設定一或多個來源的程式碼或程式的輸出行 |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | 指示範例                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | 識別的方法可以擲回的例外狀況           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | 包含從外部檔案的 XML                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | 建立清單或資料表                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | 允許加入文字的結構                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | 描述的方法或建構函式的參數       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | 識別文字是參數名稱               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | 文件的安全性存取成員        |
| `<remark>`       | [`<remark>`](documentation-comments.md#remark)         | 描述類型的其他資訊           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | 描述方法的傳回值                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | 指定的連結                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | 產生另請參閱項目                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | 描述類型成員                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | 描述屬性                                    |
| `<typeparam>`    |                                                        | 描述泛型類型參數                      |
| `<typeparamref>` |                                                        | 識別文字是型別參數名稱          |

### `<c>`

此標記會提供一個機制，表示應該在特殊的字型，例如用於程式碼區塊中設定的描述內的文字片段。 實際的程式碼行，使用`<code>`([`<code>`](documentation-comments.md#code))。

__語法：__

```xml
<c>text</c>
```

__範例：__

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>

public class Point 
{
    // ...
}
```

### `<code>`

此標記用來設定一或多個來源的程式碼或程式的輸出行中有特殊字型。 對於小的程式碼片段，敘述中，使用`<c>`([`<c>`](documentation-comments.md#c))。

__語法：__

```xml
<code>source code or program output</code>
```

__範例：__

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

這個標記可讓在註解，以指定的方法或其他程式庫成員可能使用方式的範例程式碼。 一般情況下，這也會涉及使用標記`<code>`([`<code>`](documentation-comments.md#code)) 以及。

__語法：__

```xml
<example>description</example>
```

__範例：__

請參閱`<code>`([`<code>`](documentation-comments.md#code)) 的範例。

### `<exception>`

此標記可用來記載方法可以擲回的例外狀況。

__語法：__

```xml
<exception cref="member">description</exception>
```

其中

* `member` 是成員的名稱。 文件產生器會檢查指定的成員存在，並將轉譯`member`文件檔案中的標準的項目名稱。
* `description` 是描述例外狀況會擲回的情況。

__範例：__

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

這個標記可讓包括從 XML 文件外部的原始程式檔的相關資訊。 外部檔案必須是語式正確的 XML 文件，和 XPath 運算式套用至文件會指定要包含哪些 XML 從該文件。 `<include>`標記取代選取的 XML 從外部文件。

__語法：__

```
<include file="filename" path="xpath" />
```

其中

* `filename` 為外部 XML 檔的檔案名稱。 檔案名稱會解譯相對包含 include 標記的檔案。
* `xpath` 是選取某些 XML 的外部 XML 檔案中的 XPath 運算式。

__範例：__

如果原始程式碼包含如下的宣告：

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

而且 「 docs.xml"的外部檔案具有下列內容：

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

然後相同的文件是輸出，因為如果原始程式碼包含：

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

此標記用來建立清單或資料表的項目。 它可能包含`<listheader>`區塊來定義資料表或定義清單的標題資料列。 (當定義資料表時，只有一個項目`term`需要提供標題中。)

在清單中的每個項目指定`<item>`區塊。 建立定義清單中，當兩者`term`和`description`必須指定。 不過，對於資料表、 項目符號清單或編號的清單，只`description`需要指定。

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

* `term` 若要定義，其定義位於詞彙`description`。
* `description` 可能是項目符號或編號的清單中的項目或定義`term`。

__範例：__

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

此標記是內其他標記中，使用這類`<summary>`([`<remark>`](documentation-comments.md#remark)) 或`<returns>`([`<returns>`](documentation-comments.md#returns))，並允許加入文字的結構。

__語法：__

```xml
<para>content</para>
```

其中`content`段落的文字。

__範例：__

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

此標記用來描述方法、 建構函式或索引子的參數。

__語法：__

```xml
<param name="name">description</param>
```

其中

* `name` 為參數的名稱。
* `description` 是參數的描述。

__範例：__

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

此標記用來指出文字為參數。 可以處理文件檔案，以某種明顯方式格式化此參數。

__語法：__

```xml
<paramref name="name"/>
```

其中`name`是參數的名稱。

__範例：__

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

這個標記可讓安全性存取範圍的成員，才能加以記錄。

__語法：__

```xml
<permission cref="member">description</permission>
```

其中

* `member` 是成員的名稱。 文件產生器會檢查指定的程式碼項目存在，並將轉譯*成員*文件檔案中的標準的項目名稱。
* `description` 是權之成員的描述。

__範例：__

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remark>`

此標記用來指定類型的相關額外資訊。 (使用`<summary>`([`<summary>`](documentation-comments.md#summary)) 來描述本身的類型和類型的成員。)

__語法：__

```xml
<remark>description</remark>
```

其中`description`這是備註的文字。

__範例：__

```csharp
/// <summary>Class <c>Point</c> models a point in a 
/// two-dimensional plane.</summary>
/// <remark>Uses polar coordinates</remark>
public class Point 
{
    // ...
}
```

### `<returns>`

此標記用來描述方法的傳回值。

__語法：__

```xml
<returns>description</returns>
```

其中`description`是傳回值的描述。

__範例：__

```csharp
/// <summary>Report a point's location as a string.</summary>
/// <returns>A string representing a point's location, in the form (x,y),
///    without any leading, trailing, or embedded whitespace.</returns>
public override string ToString() {
    return "(" + X + "," + Y + ")";
}
```

### `<see>`

這個標記可讓文字內指定的連結。 使用`<seealso>`([`<seealso>`](documentation-comments.md#seealso)) 來指示要出現在另請參閱 > 一節中的文字。

__語法：__

```xml
<see cref="member"/>
```

其中`member`是成員的名稱。 文件產生器會檢查指定的程式碼項目存在，而且變更*成員*產生的文件檔案中的項目名稱。

__範例：__

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

這個標記可讓 「 請參閱 」 一節要產生的項目。 使用`<see>`([`<see>`](documentation-comments.md#see)) 來指定文字中的連結。

__語法：__

```xml
<seealso cref="member"/>
```

其中`member`是成員的名稱。 文件產生器會檢查指定的程式碼項目存在，而且變更*成員*產生的文件檔案中的項目名稱。

__範例：__

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

此標記可用來描述類型或類型的成員。 使用`<remark>`([`<remark>`](documentation-comments.md#remark)) 來描述類型本身。

__語法：__

```xml
<summary>description</summary>
```

其中`description`是類型或成員的摘要。

__範例：__

```csharp
/// <summary>This constructor initializes the new Point to (0,0).</summary>
public Point() : this(0,0) {
}
```

### `<value>`

這個標記可讓您描述屬性。

__語法：__

```xml
<value>property description</value>
```

其中`property description`是屬性的描述。

__範例：__

```csharp
/// <value>Property <c>X</c> represents the point's x-coordinate.</value>
public int X
{
    get { return x; }
    set { x = value; }
}
```

### `<typeparam>`

此標記用來描述類別、 結構、 介面、 委派或方法的泛型型別參數。

__語法：__

```xml
<typeparam name="name">description</typeparam>
```

何處`name`的型別參數名稱和`description`是其描述。

__範例：__

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

此標記用來指出字組的型別參數。 可以處理文件檔案，以某種明顯方式格式化此型別參數。

__語法：__

```xml
<typeparamref name="name"/>
```

其中`name`是型別參數的名稱。

__範例：__

```csharp
/// <summary>This method fetches data and returns a list of <typeparamref name="T"/>.</summary>
/// <param name="query">query to execute</param>
public List<T> FetchData<T>(string query) {
    ...
}
```

## <a name="processing-the-documentation-file"></a>處理文件檔案

文件產生器產生標記的文件註解的原始程式碼中的每個項目識別碼字串。 此識別碼字串可唯一識別來源項目。 文件檢視器可以使用的識別碼字串，識別要套用的文件的對應中繼資料/反映項目。

文件檔案不是原始碼; 的階層式表示法而是與產生的識別碼字串，每個項目的一般清單。

### <a name="id-string-format"></a>識別碼字串格式

產生識別碼字串時，文件產生器就會遵守下列規則：

*  字串中未放置任何空白字元。

*  字串的第一個部分會識別所記載，透過單一字元，後面接著冒號的成員種類。 會定義下列類型的成員：

   | __字元__ | __描述__                                             |
   |---------------|-------------------------------------------------------------|
   | E             | Event - 事件                                                       |
   | F             | 欄位                                                       |
   | M             | 方法 （包括建構函式、 解構函式和運算子） |
   | N             | 命名空間                                                   |
   | P             | 屬性 （包括索引子）                               |
   | T             | 型別 （例如類別、 委派、 列舉、 介面和結構） |
   | !             | 錯誤字串;字串的其餘部分會提供錯誤的相關資訊。 例如，文件產生器會產生無法解析的連結資訊時發生錯誤。 |

*  字串的第二個部分是在命名空間的根開始之項目的完整的名稱。 項目、 其封入類型，以及命名空間的名稱並以句號分隔。 如果項目本身的名稱包含句點，它們會被取代的`#(U+0023)`字元。 （它會假設沒有任何項目，其名稱中有這個字元）。
*  適用於方法和引數時，引數清單如下所示，括號括住的屬性。 對於不含引數，就會省略括弧。 引數會以逗號分隔。 每個引數的編碼相同 CLI 簽章，如下所示：
   *  引數會以其文件的名稱，此作業取決於其完整名稱，修改，如下所示：
      * 代表泛型類型引數具有附加"'"字元後面的型別參數數目
      * 引數具有`out`或是`ref`修飾詞具有`@`遵循其型別名稱。 傳遞引數傳值方式或透過`params`有任何特殊的標記法。
      * 是陣列的引數會表示為`[lowerbound:size, ... , lowerbound:size]`其中逗號數目是以下其中一個，陣序規範，而下限和每個維度大小，如果已知，會以十進位。 如果未指定下限或大小，會將其省略。 如果省略的下限和大小的特定維度，「`:`」 也會省略。 不規則的陣列由其中一個 「`[]`」 每個層級。
      * 具有非 void 的指標類型的引數都使用代表`*`遵循的型別名稱。 Void 的指標表示使用的型別名稱`System.Void`。
      * 參考類型上定義的泛型型別參數的引數使用編碼"'"字元後面接著型別參數的以零為起始的索引。
      * 使用在方法中定義的泛型類型參數的引數使用雙倒 」\`\`"而不是"\`"用於類型。
      * 使用泛型型別，後面接著編碼參考建構的泛型類型引數"{"，後面接著逗號分隔的清單型別引數，再接著"}"。

### <a name="id-string-examples"></a>識別碼字串範例

下列範例各自都顯示 C# 程式碼片段，以及每個來源項目能夠使文件註解產生的識別碼字串：

*  類型被表示使用其完整的名稱，夾帶的一般資訊：

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

*  欄位會以其完整名稱表示：

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

*  解構函式。

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

*  方法。

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

*  屬性與索引子。

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

*  事件。

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

   一組完整的一元運算子函式名稱使用如下所示： `op_UnaryPlus`， `op_UnaryNegation`， `op_LogicalNot`， `op_OnesComplement`， `op_Increment`， `op_Decrement`， `op_True`，以及`op_False`。

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

   一組完整的二元運算子函式名稱使用如下所示： `op_Addition`， `op_Subtraction`， `op_Multiply`， `op_Division`， `op_Modulus`， `op_BitwiseAnd`， `op_BitwiseOr`， `op_ExclusiveOr`， `op_LeftShift`， `op_RightShift`，`op_Equality`， `op_Inequality`， `op_LessThan`， `op_LessThanOrEqual`， `op_GreaterThan`，和`op_GreaterThanOrEqual`。

*  轉換運算子可讓您有尾端"`~`"後面接著的傳回型別。

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

### <a name="c-source-code"></a>C# 原始程式碼

下列範例顯示的原始程式碼`Point`類別：

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

以下是一個文件產生器類別中指定的原始碼時所產生的輸出`Point`，如上所示：

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
