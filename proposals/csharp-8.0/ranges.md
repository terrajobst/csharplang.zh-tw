---
ms.openlocfilehash: 50f2bd2d0a84064cfe35fe65b9e5c59c052d19ac
ms.sourcegitcommit: 1dbb8e82bed5012a58a3a035bf2c3737ed570d07
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/24/2020
ms.locfileid: "80122941"
---
# <a name="ranges"></a>範圍

## <a name="summary"></a>摘要

這項功能是關於提供兩個新的運算子，可讓您在執行時間建立 `System.Index` 和 `System.Range` 物件，並使用它們來編制索引/配量集合。

## <a name="overview"></a>概觀

### <a name="well-known-types-and-members"></a>知名的型別和成員

若要針對 `System.Index` 和 `System.Range`使用新的語法形式，可能需要新的知名類型和成員，視使用的語法格式而定。

若要使用 "hat" 運算子（`^`），需要下列各項

```csharp
namespace System
{
    public readonly struct Index
    {
        public Index(int value, bool fromEnd);
    }
}
```

若要使用 `System.Index` 類型做為陣列元素存取中的引數，則需要下列成員：

```csharp
int System.Index.GetOffset(int length);
```

`System.Range` 的 `..` 語法需要 `System.Range` 類型，以及下列一個或多個成員：

```csharp
namespace System
{
    public readonly struct Range
    {
        public Range(System.Index start, System.Index end);
        public static Range StartAt(System.Index start);
        public static Range EndAt(System.Index end);
        public static Range All { get; }
    }
}
```

`..` 語法允許不存在任何一個、兩個或不具有任何引數。 不論引數的數目為何，`Range` 的函式一律都足以使用 `Range` 語法。 不過，如果有任何其他成員存在，而且其中一個或多個 `..` 引數遺失，則可能會替代適當的成員。

最後，若要在陣列元素存取運算式中使用 `System.Range` 類型的值，下列成員必須存在：

```csharp
namespace System.Runtime.CompilerServices
{
    public static class RuntimeHelpers
    {
        public static T[] GetSubArray<T>(T[] array, System.Range range);
    }
}
```

### <a name="systemindex"></a>System.object

C#沒有任何方法可以從結尾編制集合的索引，而是大部分的索引子使用「從開始」的概念，或執行「長度-i」運算式。 我們引進了新的索引運算式，表示「從結尾」。 此功能會引進新的一元前置詞 "hat" 運算子。 其單一運算元必須可轉換為 `System.Int32`。 它會降低為適當的 `System.Index` factory 方法呼叫。

我們使用下列額外的語法形式，來增強*unary_expression*的文法：

```antlr
unary_expression
    : '^' unary_expression
    ;
```

我們會*從 end*運算子呼叫此索引。 *來自結束*運算子的預先定義索引如下所示：

```csharp
    System.Index operator ^(int fromEnd);
```

只有大於或等於零的輸入值才會定義此運算子的行為。

範例：

```csharp
var thirdItem = list[2];                // list[2]
var lastItem = list[^1];                // list[Index.CreateFromEnd(1)]

var multiDimensional = list[3, ^2]      // list[3, Index.CreateFromEnd(2)]
```

#### <a name="systemrange"></a>系統. 範圍

C#沒有語法方式可存取集合的「範圍」或「配量」。 使用者通常會被迫執行複雜的結構，以篩選/操作記憶體的磁區，或利用 LINQ 方法，例如 `list.Skip(5).Take(2)`。 隨著 `System.Span<T>` 和其他類似的類型，在語言/執行時間中更深入的層級上支援這類作業會變得更加重要，而且介面是統一的。

語言將會引進新的範圍運算子 `x..y`。 它是可接受兩個運算式的二元中置運算子。 可以省略任一運算元（以下範例），而且必須可以轉換成 `System.Index`。 它會降低為適當的 `System.Range` factory 方法呼叫。

我們會以C#下列內容取代*multiplicative_expression*的文法規則（以引進新的優先順序層級）：

```antlr
range_expression
    : unary_expression
    | range_expression? '..' range_expression?
    ;

multiplicative_expression
    : range_expression
    | multiplicative_expression '*' range_expression
    | multiplicative_expression '/' range_expression
    | multiplicative_expression '%' range_expression
    ;
```

*範圍運算子*的所有形式都具有相同的優先順序。 這個新的優先順序群組低於*一元運算子*，且高於*mulitiplicative 算術運算子*。

我們呼叫 `..` 運算子的*範圍運算子*。 您可以大致瞭解內建的範圍運算子，以對應到此表單的內建運算子調用：

```csharp
    System.Range operator ..(Index start = 0, Index end = ^0);
```

範例：

```csharp
var slice1 = list[2..^3];               // list[Range.Create(2, Index.CreateFromEnd(3))]
var slice2 = list[..^3];                // list[Range.ToEnd(Index.CreateFromEnd(3))]
var slice3 = list[2..];                 // list[Range.FromStart(2)]
var slice4 = list[..];                  // list[Range.All]

var multiDimensional = list[1..2, ..]   // list[Range.Create(1, 2), Range.All]
```

此外，`System.Index` 應該要有來自 `System.Int32`的隱含轉換，而不需要多載以混用多維度簽章的整數和索引。

## <a name="adding-index-and-range-support-to-existing-library-types"></a>將索引和範圍支援加入至現有的程式庫類型

### <a name="implicit-index-support"></a>隱含索引支援

此語言會提供實例索引子成員，其中包含符合下列準則之類型 `Index` 類型的單一參數：

- 此類型為計算。
- 型別具有可存取的實例索引子，其接受單一 `int` 做為引數。
- 型別沒有可存取的實例索引子，其接受 `Index` 做為第一個參數。 `Index` 必須是唯一的參數，否則其餘的參數必須是選擇性的。

如果類型具有名為 `Length` 的屬性，或具有可存取 getter 的 `Count` 和 `int`的傳回型別，則該型別為***計算***。 語言可以利用這個屬性，將類型 `Index` 的運算式轉換成運算式所在點的 `int`，而不需要使用類型 `Index`。 如果 `Length` 和 `Count` 都存在，將會偏好 `Length`。 為了簡單起見，提案會使用名稱 `Length` 來代表 `Count` 或 `Length`。

針對這類類型，語言將會像是 `T this[Index index]` 形式的索引子成員，其中 `T` 是以 `int` 為基礎之索引子的傳回類型，包括任何 `ref` 樣式注釋。 新成員將會有相同的 `get` 和 `set` 成員，其存取範圍與 `int` 索引子相符。 

新的索引子將會藉由將類型 `Index` 的引數轉換成 `int`，併發出對 `int` 為基礎之索引子的呼叫來執行。 為了方便討論，讓我們使用 `receiver[expr]`的範例。 `int` 的 `expr` 轉換會如下所示：

- 當引數的格式為 `^expr2`，且 `expr2` 的類型為 `int`時，會將其轉譯為 `receiver.Length - expr2`。
- 否則，它將會轉譯為 `expr.GetOffset(receiver.Length)`。

這可讓開發人員在現有的類型上使用 `Index` 功能，而不需要進行修改。 例如：

``` csharp
List<char> list = ...;
var value = list[^1]; 

// Gets translated to 
var value = list[list.Count - 1]; 
```

`receiver` 和 `Length` 運算式會適當地溢出，以確保任何副作用只會執行一次。 例如：

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int this[int index] => _array[index];
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        int i = Get()[^1];
        Console.WriteLine(i);
    }
}
```

此程式碼會列印「Get Length 3」。

### <a name="implicit-range-support"></a>隱含範圍支援

此語言會提供實例索引子成員，其中包含符合下列準則之類型 `Range` 類型的單一參數：

- 此類型為計算。
- 類型具有名為 `Slice` 的可存取成員，其具有 `int`類型的兩個參數。
- 型別沒有實例索引子，它會採用單一 `Range` 做為第一個參數。 `Range` 必須是唯一的參數，否則其餘的參數必須是選擇性的。

針對這類類型，語言會系結，就好像有一個 `T this[Range range]` 形式的索引子成員，其中 `T` 是 `Slice` 方法的傳回型別，包括任何 `ref` 樣式注釋。 新成員也會有與 `Slice`相符的存取範圍。 

當以 `Range` 為基礎的索引子系結至名為 `receiver`的運算式時，會將 `Range` 運算式轉換成兩個值，然後傳遞至 `Slice` 方法，藉此降低其限制。 為了方便討論，讓我們使用 `receiver[expr]`的範例。

`Slice` 的第一個引數會透過下列方式轉換範圍類型運算式來取得：

- 當 `expr` 的格式為 `expr1..expr2` （其中可以省略 `expr2`），而且 `expr1` 的類型為 `int`時，會以 `expr1`的形式發出。
- 當 `expr` 的格式為 `^expr1..expr2` （可省略 `expr2`）時，將會以 `receiver.Length - expr1`的形式發出。
- 當 `expr` 的格式為 `..expr2` （可省略 `expr2`）時，將會以 `0`的形式發出。
- 否則，將會以 `expr.Start.GetOffset(receiver.Length)`的形式發出。

這個值會在第二個 `Slice` 引數的計算中重複使用。 這麼做時，就會將它稱為 `start`。 `Slice` 的第二個引數會透過下列方式轉換範圍類型運算式來取得：

- 當 `expr` 的格式為 `expr1..expr2` （其中可以省略 `expr1`），而且 `expr2` 的類型為 `int`時，會以 `expr2 - start`的形式發出。
- 當 `expr` 的格式為 `expr1..^expr2` （可省略 `expr1`）時，將會以 `(receiver.Length - expr2) - start`的形式發出。
- 當 `expr` 的格式為 `expr1..` （可省略 `expr1`）時，將會以 `receiver.Length - start`的形式發出。
- 否則，將會以 `expr.End.GetOffset(receiver.Length) - start`的形式發出。

`receiver`、`Length`和 `expr` 運算式會適當地溢出，以確保任何副作用只會執行一次。 例如：

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int[] Slice(int start, int length) { 
        var slice = new int[length];
        Array.Copy(_array, start, slice, 0, length);
        return slice;
    }
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        var array = Get()[0..2];
        Console.WriteLine(array.length);
    }
}
```

此程式碼會列印「Get Length 2」。

語言會特別以下列已知型別為例： 

- `string`：將使用 `Substring` 的方法，而不是 `Slice`。
- `array`：將使用 `System.Reflection.CompilerServices.GetSubArray` 的方法，而不是 `Slice`。

## <a name="alternatives"></a>替代方案

新的運算子（`^` 和 `..`）是語法的一種。 這項功能可以透過明確呼叫 `System.Index` 和 `System.Range` factory 方法來執行，但它會產生更多的重複使用程式碼，而且會圈子體驗。

## <a name="il-representation"></a>IL 標記法

這兩個運算子將會降低為一般索引子/方法呼叫，而不會在後續的編譯器層中變更。

## <a name="runtime-behavior"></a>執行時間行為

- 編譯器可以優化內建類型（例如陣列和字串）的索引子，並將索引編制為適當的現有方法。
- 如果以負值進行結構化，`System.Index` 將會擲回。
- `^0` 不會擲回，但會轉譯為提供給它的集合/可列舉長度。
- `Range.All` 在語義上等同于 `0..^0`，而且可以解構至這些索引。

## <a name="considerations"></a>考量

### <a name="detect-indexable-based-on-icollection"></a>根據 ICollection 偵測可編制索引

這個行為的靈感是集合初始化運算式。 使用類型的結構來傳達它已加入宣告功能。 在集合初始化運算式類型的案例中，可以藉由執行介面 `IEnumerable` （非泛型）來加入宣告功能。

此提案一開始要求型別必須執行 `ICollection`，才能限定為可編制索引。 不過，這需要幾個特殊案例：

- `ref struct`：這些型別無法實作為型別（例如 `Span<T>`，非常適合索引/範圍的支援）。 
- `string`：不會執行 `ICollection`，而且加入該 `interface` 的成本會很大。

這表示必須要有特殊大小寫，才能支援金鑰類型。 `string` 的特殊大小寫較不有趣，因為語言會在其他區域（`foreach` 減少、常數等等）中執行這項工作。`ref struct` 的特殊大小寫，是因為它的特殊大小寫是整個類別的類型。 如果它們只具有名為 `Count` 的屬性，且其傳回型別為 `int`，它們就會標示為可編制索引。 

考慮到設計已經正規化，表示具有屬性 `Count` / `int` `Length` 的任何型別都是可編制索引的。 這會移除所有特殊大小寫，即使是 `string` 和陣列也一樣。

### <a name="detect-just-count"></a>僅偵測計數

偵測屬性名稱 `Count` 或 `Length` 會使設計變得更複雜。 只挑選一個進行標準化並不足夠，因為它最後會排除大量的類型：

- 使用 `Length`：在系統集合和子命名空間中，排除非常多的每個集合。 這些通常是從 `ICollection` 衍生而來，因此偏好 `Count` 超過長度。
- 使用 `Count`：排除 `string`、陣列、`Span<T>` 和大部分以 `ref struct` 為基礎的類型

針對可編制索引之型別的初始偵測，其額外的複雜之處在于其在其他方面的簡化遠遠超過。

### <a name="choice-of-slice-as-a-name"></a>選擇配量作為名稱

已選擇名稱 `Slice`，因為它是 .NET 中配量樣式作業的實際標準名稱。 從 netcoreapp 2.1 開始，所有範圍樣式類型都會針對切割作業使用名稱 `Slice`。 在 netcoreapp 2.1 之前，不會有任何配量範例來尋找範例。 `List<T>`、`ArraySegment<T>`、`SortedList<T>` 之類的類型，很適合用於切割，但當加入類型時，概念並不存在。 

因此，`Slice` 是唯一的範例，就是選擇做為名稱。

### <a name="index-target-type-conversion"></a>索引目標型別轉換

另一個在索引子運算式中查看 `Index` 轉換的方法，就是做為目標型別轉換。 語言會改為將目標具類型的轉換指派給 `int`，而不是系結，如同 `return_type this[Index]`表單的成員。 

此概念可以一般化至計算類型的所有成員存取。 每當具有類型 `Index` 的運算式當做實例成員調用的引數使用，而接收者是計算時，則運算式會有目標型別轉換成 `int`。 適用于這種轉換的成員調用包括方法、索引子、屬性、擴充方法等等。只有不會排除的函式，因為它們沒有接收者。 

針對具有 `Index`類型的任何運算式，目標型別轉換將會實作為下列各項。 為了方便討論，我們使用 `receiver[expr]`的範例：

- 當 `expr` 的格式為 `^expr2` 且 `expr2` 的類型為 `int`時，會將其轉譯為 `receiver.Length - expr2`。
- 否則，它將會轉譯為 `expr.GetOffset(receiver.Length)`。

`receiver` 和 `Length` 運算式會適當地溢出，以確保任何副作用只會執行一次。 例如：

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int GetAt(int index) => _array[index];
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        int i = Get().GetAt(^1);
        Console.WriteLine(i);
    }
}
```

此程式碼會列印「Get Length 3」。 

對於具有代表索引之參數的任何成員而言，這項功能會很有説明。 例如 `List<T>.InsertAt` 。 這也可能會造成混淆，因為語言無法提供任何有關運算式是否適用于編制索引的指引。 它可以做的就是在計算型別上叫用成員時，將任何 `Index` 運算式轉換成 `int`。 

限制：

- 只有當類型為 `Index` 的運算式直接是成員的引數時，才適用這種轉換。 它不適用於任何嵌套的運算式。

## <a name="decisions-made-during-implementation"></a>執行期間所做的決策

- 模式中的所有成員都必須是實例成員
- 如果找到長度方法，但其傳回類型錯誤，請繼續尋找計數
- 用於索引模式的索引子必須只有一個 int 參數
- 用於範圍模式的配量方法必須剛好有兩個 int 參數
- 尋找模式成員時，我們會尋找原始定義，而不是結構化成員

## <a name="design-meetings"></a>設計會議

- https://github.com/dotnet/csharplang/blob/master/meetings/2019/LDM-2019-04-01.md

## <a name="questions"></a>問題
