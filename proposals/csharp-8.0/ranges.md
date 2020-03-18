---
ms.openlocfilehash: d6519ff57b4a98c4eec8ccbf310303432ac3255e
ms.sourcegitcommit: 65ea1e6dc02853e37e7f2088e2b6cc08d01d1044
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "79485025"
---
# <a name="ranges"></a><span data-ttu-id="cc7b6-101">範圍</span><span class="sxs-lookup"><span data-stu-id="cc7b6-101">Ranges</span></span>

## <a name="summary"></a><span data-ttu-id="cc7b6-102">摘要</span><span class="sxs-lookup"><span data-stu-id="cc7b6-102">Summary</span></span>

<span data-ttu-id="cc7b6-103">這項功能是關於提供兩個新的運算子，可讓您在執行時間建立 `System.Index` 和 `System.Range` 物件，並使用它們來編制索引/配量集合。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-103">This feature is about delivering two new operators that allow constructing `System.Index` and `System.Range` objects, and using them to index/slice collections at runtime.</span></span>

## <a name="overview"></a><span data-ttu-id="cc7b6-104">概觀</span><span class="sxs-lookup"><span data-stu-id="cc7b6-104">Overview</span></span>

### <a name="well-known-types-and-members"></a><span data-ttu-id="cc7b6-105">知名的型別和成員</span><span class="sxs-lookup"><span data-stu-id="cc7b6-105">Well-known types and members</span></span>

<span data-ttu-id="cc7b6-106">若要針對 `System.Index` 和 `System.Range`使用新的語法形式，可能需要新的知名類型和成員，視使用的語法格式而定。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-106">To use the new syntactic forms for `System.Index` and `System.Range`, new well-known types and members may be necessary, depending on which syntactic forms are used.</span></span>

<span data-ttu-id="cc7b6-107">若要使用 "hat" 運算子（`^`），需要下列各項</span><span class="sxs-lookup"><span data-stu-id="cc7b6-107">To use the "hat" operator (`^`), the following is required</span></span>

```csharp
namespace System
{
    public readonly struct Index
    {
        public Index(int value, bool fromEnd);
    }
}
```

<span data-ttu-id="cc7b6-108">若要使用 `System.Index` 類型做為陣列元素存取中的引數，則需要下列成員：</span><span class="sxs-lookup"><span data-stu-id="cc7b6-108">To use the `System.Index` type as an argument in an array element access, the following member is required:</span></span>

```csharp
int System.Index.GetOffset(int length);
```

<span data-ttu-id="cc7b6-109">`System.Range` 的 `..` 語法需要 `System.Range` 類型，以及下列一個或多個成員：</span><span class="sxs-lookup"><span data-stu-id="cc7b6-109">The `..` syntax for `System.Range` will require the `System.Range` type, as well as one or more of the following members:</span></span>

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

<span data-ttu-id="cc7b6-110">`..` 語法允許不存在任何一個、兩個或不具有任何引數。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-110">The `..` syntax allows for either, both, or none of its arguments to be absent.</span></span> <span data-ttu-id="cc7b6-111">不論引數的數目為何，`Range` 的函式一律都足以使用 `Range` 語法。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-111">Regardless of the number of arguments, the `Range` constructor is always sufficient for using the `Range` syntax.</span></span> <span data-ttu-id="cc7b6-112">不過，如果有任何其他成員存在，而且其中一個或多個 `..` 引數遺失，則可能會替代適當的成員。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-112">However, if any of the other members are present and one or more of the `..` arguments are missing, the appropriate member may be substituted.</span></span>

<span data-ttu-id="cc7b6-113">最後，若要在陣列元素存取運算式中使用 `System.Range` 類型的值，下列成員必須存在：</span><span class="sxs-lookup"><span data-stu-id="cc7b6-113">Finally, for a value of type `System.Range` to be used in an array element access expression, the following member must be present:</span></span>

```csharp
namespace System.Runtime.CompilerServices
{
    public static class RuntimeHelpers
    {
        public static T[] GetSubArray<T>(T[] array, System.Range range);
    }
}
```

### <a name="systemindex"></a><span data-ttu-id="cc7b6-114">System.object</span><span class="sxs-lookup"><span data-stu-id="cc7b6-114">System.Index</span></span>

<span data-ttu-id="cc7b6-115">C#沒有任何方法可以從結尾編制集合的索引，而是大部分的索引子使用「從開始」的概念，或執行「長度-i」運算式。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-115">C# has no way of indexing a collection from the end, but rather most indexers use the "from start" notion, or do a "length - i" expression.</span></span> <span data-ttu-id="cc7b6-116">我們引進了新的索引運算式，表示「從結尾」。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-116">We introduce a new Index expression that means "from the end".</span></span> <span data-ttu-id="cc7b6-117">此功能會引進新的一元前置詞 "hat" 運算子。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-117">The feature will introduce a new unary prefix "hat" operator.</span></span> <span data-ttu-id="cc7b6-118">其單一運算元必須可轉換為 `System.Int32`。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-118">Its single operand must be convertible to `System.Int32`.</span></span> <span data-ttu-id="cc7b6-119">它會降低為適當的 `System.Index` factory 方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-119">It will be lowered into the appropriate `System.Index` factory method call.</span></span>

<span data-ttu-id="cc7b6-120">我們使用下列額外的語法形式，來增強*unary_expression*的文法：</span><span class="sxs-lookup"><span data-stu-id="cc7b6-120">We augment the grammar for *unary_expression* with the following additional syntax form:</span></span>

```antlr
unary_expression
    : '^' unary_expression
    ;
```

<span data-ttu-id="cc7b6-121">我們會*從 end*運算子呼叫此索引。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-121">We call this the *index from end* operator.</span></span> <span data-ttu-id="cc7b6-122">*來自結束*運算子的預先定義索引如下所示：</span><span class="sxs-lookup"><span data-stu-id="cc7b6-122">The predefined *index from end* operators are as follows:</span></span>

```csharp
    System.Index operator ^(int fromEnd);
```

<span data-ttu-id="cc7b6-123">只有大於或等於零的輸入值才會定義此運算子的行為。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-123">The behavior of this operator is only defined for input values greater than or equal to zero.</span></span>

<span data-ttu-id="cc7b6-124">範例：</span><span class="sxs-lookup"><span data-stu-id="cc7b6-124">Examples:</span></span>

```csharp
var thirdItem = list[2];                // list[2]
var lastItem = list[^1];                // list[Index.CreateFromEnd(1)]

var multiDimensional = list[3, ^2]      // list[3, Index.CreateFromEnd(2)]
```

#### <a name="systemrange"></a><span data-ttu-id="cc7b6-125">系統. 範圍</span><span class="sxs-lookup"><span data-stu-id="cc7b6-125">System.Range</span></span>

<span data-ttu-id="cc7b6-126">C#沒有語法方式可存取集合的「範圍」或「配量」。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-126">C# has no syntactic way to access "ranges" or "slices" of collections.</span></span> <span data-ttu-id="cc7b6-127">使用者通常會被迫執行複雜的結構，以篩選/操作記憶體的磁區，或利用 LINQ 方法，例如 `list.Skip(5).Take(2)`。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-127">Usually users are forced to implement complex structures to filter/operate on slices of memory, or resort to LINQ methods like `list.Skip(5).Take(2)`.</span></span> <span data-ttu-id="cc7b6-128">隨著 `System.Span<T>` 和其他類似的類型，在語言/執行時間中更深入的層級上支援這類作業會變得更加重要，而且介面是統一的。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-128">With the addition of `System.Span<T>` and other similar types, it becomes more important to have this kind of operation supported on a deeper level in the language/runtime, and have the interface unified.</span></span>

<span data-ttu-id="cc7b6-129">語言將會引進新的範圍運算子 `x..y`。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-129">The language will introduce a new range operator `x..y`.</span></span> <span data-ttu-id="cc7b6-130">它是可接受兩個運算式的二元中置運算子。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-130">It is a binary infix operator that accepts two expressions.</span></span> <span data-ttu-id="cc7b6-131">可以省略任一運算元（以下範例），而且必須可以轉換成 `System.Index`。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-131">Either operand can be omitted (examples below), and they have to be convertible to `System.Index`.</span></span> <span data-ttu-id="cc7b6-132">它會降低為適當的 `System.Range` factory 方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-132">It will be lowered to the appropriate `System.Range` factory method call.</span></span>

<span data-ttu-id="cc7b6-133">-我們會將C# *multiplicative_expression*的文法規則取代為下列內容（以引進新的優先順序層級）：</span><span class="sxs-lookup"><span data-stu-id="cc7b6-133">-We replace the C# grammar rules for *multiplicative_expression* with the following (in order to introduce a new precedence level):</span></span>

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

<span data-ttu-id="cc7b6-134">*範圍運算子*的所有形式都具有相同的優先順序。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-134">All forms of the *range operator* have the same precedence.</span></span> <span data-ttu-id="cc7b6-135">這個新的優先順序群組低於*一元運算子*，且高於*mulitiplicative 算術運算子*。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-135">This new precedence group is lower than the *unary operators* and higher than the *mulitiplicative arithmetic operators*.</span></span>

<span data-ttu-id="cc7b6-136">我們呼叫 `..` 運算子的*範圍運算子*。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-136">We call the `..` operator the *range operator*.</span></span> <span data-ttu-id="cc7b6-137">您可以大致瞭解內建的範圍運算子，以對應到此表單的內建運算子調用：</span><span class="sxs-lookup"><span data-stu-id="cc7b6-137">The built-in range operator can roughly be understood to correspond to the invocation of a built-in operator of this form:</span></span>

```csharp
    System.Range operator ..(Index start = 0, Index end = ^0);
```

<span data-ttu-id="cc7b6-138">範例：</span><span class="sxs-lookup"><span data-stu-id="cc7b6-138">Examples:</span></span>

```csharp
var slice1 = list[2..^3];               // list[Range.Create(2, Index.CreateFromEnd(3))]
var slice2 = list[..^3];                // list[Range.ToEnd(Index.CreateFromEnd(3))]
var slice3 = list[2..];                 // list[Range.FromStart(2)]
var slice4 = list[..];                  // list[Range.All]

var multiDimensional = list[1..2, ..]   // list[Range.Create(1, 2), Range.All]
```

<span data-ttu-id="cc7b6-139">此外，`System.Index` 應該要有來自 `System.Int32`的隱含轉換，而不需要多載以混用多維度簽章的整數和索引。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-139">Moreover, `System.Index` should have an implicit conversion from `System.Int32`, in order not to need to overload for mixing integers and indexes over multi-dimensional signatures.</span></span>

## <a name="adding-index-and-range-support-to-existing-library-types"></a><span data-ttu-id="cc7b6-140">將索引和範圍支援加入至現有的程式庫類型</span><span class="sxs-lookup"><span data-stu-id="cc7b6-140">Adding Index and Range support to existing library types</span></span>

### <a name="implicit-index-support"></a><span data-ttu-id="cc7b6-141">隱含索引支援</span><span class="sxs-lookup"><span data-stu-id="cc7b6-141">Implicit Index support</span></span>

<span data-ttu-id="cc7b6-142">此語言會提供實例索引子成員，其中包含符合下列準則之類型 `Index` 類型的單一參數：</span><span class="sxs-lookup"><span data-stu-id="cc7b6-142">The language will provide an instance indexer member with a single parameter of type `Index` for types which meet the following criteria:</span></span>

- <span data-ttu-id="cc7b6-143">此類型為計算。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-143">The type is Countable.</span></span>
- <span data-ttu-id="cc7b6-144">型別具有可存取的實例索引子，其接受單一 `int` 做為引數。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-144">The type has an accessible instance indexer which takes a single `int` as the argument.</span></span>
- <span data-ttu-id="cc7b6-145">型別沒有可存取的實例索引子，其接受 `Index` 做為第一個參數。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-145">The type does not have an accessible instance indexer which takes an `Index` as the first parameter.</span></span> <span data-ttu-id="cc7b6-146">`Index` 必須是唯一的參數，否則其餘的參數必須是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-146">The `Index` must be the only parameter or the remaining parameters must be optional.</span></span>

<span data-ttu-id="cc7b6-147">如果類型具有名為 `Length` 的屬性，或具有可存取 getter 的 `Count` 和 `int`的傳回型別，則該型別為***計算***。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-147">A type is ***Countable*** if it  has a property named `Length` or `Count` with an accessible getter and a return type of `int`.</span></span> <span data-ttu-id="cc7b6-148">語言可以利用這個屬性，將類型 `Index` 的運算式轉換成運算式所在點的 `int`，而不需要使用類型 `Index`。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-148">The language can make use of this property to convert an expression of type `Index` into an `int` at the point of the expression without the need to use the type `Index` at all.</span></span> <span data-ttu-id="cc7b6-149">如果 `Length` 和 `Count` 都存在，將會偏好 `Length`。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-149">In case both `Length` and `Count` are present, `Length` will be preferred.</span></span> <span data-ttu-id="cc7b6-150">為了簡單起見，提案會使用名稱 `Length` 來代表 `Count` 或 `Length`。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-150">For simplicity going forward, the proposal will use the name `Length` to represent `Count` or `Length`.</span></span>

<span data-ttu-id="cc7b6-151">針對這類類型，語言將會像是 `T this[Index index]` 形式的索引成員，其中 `T` 是以 `int` 為基礎之索引子的傳回類型，包括任何 `ref` 樣式注釋。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-151">For such types, the language will act as if there is an index member of the form `T this[Index index]` where `T` is the return type of the `int` based indexer including any `ref` style annotations.</span></span> <span data-ttu-id="cc7b6-152">新成員將會有相同的 `get` 和 `set` 成員，其存取範圍與 `int` 索引子相符。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-152">The new member will have the same `get` and `set` members with matching accessibility as the `int` indexer.</span></span> 

<span data-ttu-id="cc7b6-153">新的索引子將會藉由將類型 `Index` 的引數轉換成 `int`，併發出對 `int` 為基礎之索引子的呼叫來執行。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-153">The new indexer will be implemented by converting the argument of type `Index` into an `int` and emitting a call to the `int` based indexer.</span></span> <span data-ttu-id="cc7b6-154">為了方便討論，讓我們使用 `receiver[expr]`的範例。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-154">For discussion purposes, lets use the example of `receiver[expr]`.</span></span> <span data-ttu-id="cc7b6-155">`int` 的 `expr` 轉換會如下所示：</span><span class="sxs-lookup"><span data-stu-id="cc7b6-155">The conversion of `expr` to `int` will occur as follows:</span></span>

- <span data-ttu-id="cc7b6-156">當引數的格式為 `^expr2`，且 `expr2` 的類型為 `int`時，會將其轉譯為 `receiver.Length - expr2`。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-156">When the argument is of the form `^expr2` and the type of `expr2` is `int`, it will be translated to `receiver.Length - expr2`.</span></span>
- <span data-ttu-id="cc7b6-157">否則，它將會轉譯為 `expr.GetOffset(receiver.Length)`。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-157">Otherwise, it will be translated as `expr.GetOffset(receiver.Length)`.</span></span>

<span data-ttu-id="cc7b6-158">這可讓開發人員在現有的類型上使用 `Index` 功能，而不需要進行修改。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-158">This allows for developers to use the `Index` feature on existing types without the need for modification.</span></span> <span data-ttu-id="cc7b6-159">例如：</span><span class="sxs-lookup"><span data-stu-id="cc7b6-159">For example:</span></span>

``` csharp
List<char> list = ...;
var value = list[^1]; 

// Gets translated to 
var value = list[list.Count - 1]; 
```

<span data-ttu-id="cc7b6-160">`receiver` 和 `Length` 運算式會適當地溢出，以確保任何副作用只會執行一次。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-160">The `receiver` and `Length` expressions will be spilled as appropriate to ensure any side effects are only executed once.</span></span> <span data-ttu-id="cc7b6-161">例如：</span><span class="sxs-lookup"><span data-stu-id="cc7b6-161">For example:</span></span>

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

<span data-ttu-id="cc7b6-162">此程式碼會列印「Get Length 3」。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-162">This code will print "Get Length 3".</span></span>

### <a name="implicit-range-support"></a><span data-ttu-id="cc7b6-163">隱含範圍支援</span><span class="sxs-lookup"><span data-stu-id="cc7b6-163">Implicit Range support</span></span>

<span data-ttu-id="cc7b6-164">此語言會提供實例索引子成員，其中包含符合下列準則之類型 `Range` 類型的單一參數：</span><span class="sxs-lookup"><span data-stu-id="cc7b6-164">The language will provide an instance indexer member with a single parameter of type `Range` for types which meet the following criteria:</span></span>

- <span data-ttu-id="cc7b6-165">此類型為計算。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-165">The type is Countable.</span></span>
- <span data-ttu-id="cc7b6-166">類型具有名為 `Slice` 的可存取成員，其具有 `int`類型的兩個參數。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-166">The type has an accessible member named `Slice` which has two parameters of type `int`.</span></span>
- <span data-ttu-id="cc7b6-167">型別沒有實例索引子，它會採用單一 `Range` 做為第一個參數。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-167">The type does not have an instance indexer which takes a single `Range` as the first parameter.</span></span> <span data-ttu-id="cc7b6-168">`Range` 必須是唯一的參數，否則其餘的參數必須是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-168">The `Range` must be the only parameter or the remaining parameters must be optional.</span></span>

<span data-ttu-id="cc7b6-169">針對這類類型，語言將會系結，就好像有表單的索引成員 `T this[Range range]` 其中 `T` 是 `Slice` 方法的傳回型別，包括任何 `ref` 樣式注釋。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-169">For such types, the language will bind as if there is an index member of the form `T this[Range range]` where `T` is the return type of the `Slice` method including any `ref` style annotations.</span></span> <span data-ttu-id="cc7b6-170">新成員也會有與 `Slice`相符的存取範圍。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-170">The new member will also have matching accessibility with `Slice`.</span></span> 

<span data-ttu-id="cc7b6-171">當以 `Range` 為基礎的索引子系結至名為 `receiver`的運算式時，會將 `Range` 運算式轉換成兩個值，然後傳遞至 `Slice` 方法，藉此降低其限制。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-171">When the `Range` based indexer is bound on an expression named `receiver`, it will be lowered by converting the `Range` expression into two values that are then passed to the `Slice` method.</span></span> <span data-ttu-id="cc7b6-172">為了方便討論，讓我們使用 `receiver[expr]`的範例。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-172">For discussion purposes, lets use the example of `receiver[expr]`.</span></span>

<span data-ttu-id="cc7b6-173">`Slice` 的第一個引數會透過下列方式轉換具型別運算式來取得：</span><span class="sxs-lookup"><span data-stu-id="cc7b6-173">The first argument of `Slice` will be obtained by converting the typed expression in the following way:</span></span>

- <span data-ttu-id="cc7b6-174">當 `expr` 的格式為 `expr1..expr2` （其中可以省略 `expr2`），而且 `expr1` 的類型為 `int`時，會以 `expr1`的形式發出。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-174">When `expr` is of the form `expr1..expr2` (where `expr2` can be omitted) and `expr1` has type `int`, then it will be emitted as `expr1`.</span></span>
- <span data-ttu-id="cc7b6-175">當 `expr` 的格式為 `^expr1..expr2` （可省略 `expr2`）時，將會以 `receiver.Length - expr1`的形式發出。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-175">When `expr` is of the form `^expr1..expr2` (where `expr2` can be omitted), then it will be emitted as `receiver.Length - expr1`.</span></span>
- <span data-ttu-id="cc7b6-176">當 `expr` 的格式為 `..expr2` （可省略 `expr2`）時，將會以 `0`的形式發出。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-176">When `expr` is of the form `..expr2` (where `expr2` can be omitted), then it will be emitted as `0`.</span></span>
- <span data-ttu-id="cc7b6-177">否則，將會以 `expr.Start.GetOffset(receiver.Length)`的形式發出。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-177">Otherwise, it will be emitted as `expr.Start.GetOffset(receiver.Length)`.</span></span>

<span data-ttu-id="cc7b6-178">這個值會在第二個 `Slice` 引數的計算中重複使用。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-178">This value will be re-used in the calculation of the second `Slice` argument.</span></span> <span data-ttu-id="cc7b6-179">這麼做時，就會將它稱為 `start`。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-179">When doing so it will be referred to as `start`.</span></span> <span data-ttu-id="cc7b6-180">`Slice` 的第二個引數會透過下列方式轉換範圍類型運算式來取得：</span><span class="sxs-lookup"><span data-stu-id="cc7b6-180">The second argument of `Slice` will be obtained by converting the range typed expression in the following way:</span></span>

- <span data-ttu-id="cc7b6-181">當 `expr` 的格式為 `expr1..expr2` （其中可以省略 `expr1`），而且 `expr2` 的類型為 `int`時，會以 `expr2 - start`的形式發出。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-181">When `expr` is of the form `expr1..expr2` (where `expr1` can be omitted) and `expr2` has type `int`, then it will be emitted as `expr2 - start`.</span></span>
- <span data-ttu-id="cc7b6-182">當 `expr` 的格式為 `expr1..^expr2` （可省略 `expr1`）時，將會以 `(receiver.Length - expr2) - start`的形式發出。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-182">When `expr` is of the form `expr1..^expr2` (where `expr1` can be omitted), then it will be emitted as `(receiver.Length - expr2) - start`.</span></span>
- <span data-ttu-id="cc7b6-183">當 `expr` 的格式為 `expr1..` （可省略 `expr1`）時，將會以 `receiver.Length - start`的形式發出。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-183">When `expr` is of the form `expr1..` (where `expr1` can be omitted), then it will be emitted as `receiver.Length - start`.</span></span>
- <span data-ttu-id="cc7b6-184">否則，將會以 `expr.End.GetOffset(receiver.Length) - start`的形式發出。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-184">Otherwise, it will be emitted as `expr.End.GetOffset(receiver.Length) - start`.</span></span>

<span data-ttu-id="cc7b6-185">`receiver`、`Length` 和 `expr` 運算式會適當地溢出，以確保任何副作用只會執行一次。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-185">The `receiver`, `Length` and `expr` expressions will be spilled as appropriate to ensure any side effects are only executed once.</span></span> <span data-ttu-id="cc7b6-186">例如：</span><span class="sxs-lookup"><span data-stu-id="cc7b6-186">For example:</span></span>

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

<span data-ttu-id="cc7b6-187">此程式碼會列印「Get Length 2」。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-187">This code will print "Get Length 2".</span></span>

<span data-ttu-id="cc7b6-188">語言會特別以下列已知型別為例：</span><span class="sxs-lookup"><span data-stu-id="cc7b6-188">The language will special case the following known types:</span></span> 

- <span data-ttu-id="cc7b6-189">`string`：將使用 `Substring` 的方法，而不是 `Slice`。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-189">`string`: the method `Substring` will be used instead of `Slice`.</span></span>
- <span data-ttu-id="cc7b6-190">`array`：將使用 `System.Reflection.CompilerServices.GetSubArray` 的方法，而不是 `Slice`。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-190">`array`: the method `System.Reflection.CompilerServices.GetSubArray` will be used instead of `Slice`.</span></span>

## <a name="alternatives"></a><span data-ttu-id="cc7b6-191">替代方案</span><span class="sxs-lookup"><span data-stu-id="cc7b6-191">Alternatives</span></span>

<span data-ttu-id="cc7b6-192">新的運算子（`^` 和 `..`）是語法的一種。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-192">The new operators (`^` and `..`) are syntactic sugar.</span></span> <span data-ttu-id="cc7b6-193">這項功能可以透過明確呼叫 `System.Index` 和 `System.Range` factory 方法來執行，但它會產生更多的重複使用程式碼，而且會圈子體驗。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-193">The functionality can be implemented by explicit calls to `System.Index` and `System.Range` factory methods, but it will result in a lot more boilerplate code, and the experience will be unintuitive.</span></span>

## <a name="il-representation"></a><span data-ttu-id="cc7b6-194">IL 標記法</span><span class="sxs-lookup"><span data-stu-id="cc7b6-194">IL Representation</span></span>

<span data-ttu-id="cc7b6-195">這兩個運算子將會降低為一般索引子/方法呼叫，而不會在後續的編譯器層中變更。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-195">These two operators will be lowered to regular indexer/method calls, with no change in subsequent compiler layers.</span></span>

## <a name="runtime-behavior"></a><span data-ttu-id="cc7b6-196">執行時間行為</span><span class="sxs-lookup"><span data-stu-id="cc7b6-196">Runtime behavior</span></span>

- <span data-ttu-id="cc7b6-197">編譯器可以優化內建類型（例如陣列和字串）的索引子，並將索引編制為適當的現有方法。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-197">Compiler can optimize indexers for built-in types like arrays and strings, and lower the indexing to the appropriate existing methods.</span></span>
- <span data-ttu-id="cc7b6-198">如果以負值進行結構化，`System.Index` 將會擲回。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-198">`System.Index` will throw if constructed with a negative value.</span></span>
- <span data-ttu-id="cc7b6-199">`^0` 不會擲回，但會轉譯為提供給它的集合/可列舉長度。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-199">`^0` does not throw, but it translates to the length of the collection/enumerable it is supplied to.</span></span>
- <span data-ttu-id="cc7b6-200">`Range.All` 在語義上等同于 `0..^0`，而且可以解構至這些索引。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-200">`Range.All` is semantically equivalent to `0..^0`, and can be deconstructed to these indices.</span></span>

## <a name="considerations"></a><span data-ttu-id="cc7b6-201">考量</span><span class="sxs-lookup"><span data-stu-id="cc7b6-201">Considerations</span></span>

### <a name="detect-indexable-based-on-icollection"></a><span data-ttu-id="cc7b6-202">根據 ICollection 偵測可編制索引</span><span class="sxs-lookup"><span data-stu-id="cc7b6-202">Detect Indexable based on ICollection</span></span>

<span data-ttu-id="cc7b6-203">這個行為的靈感是集合初始化運算式。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-203">The inspiration for this behavior was collection initializers.</span></span> <span data-ttu-id="cc7b6-204">使用類型的結構來傳達它已加入宣告功能。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-204">Using the structure of a type to convey that it had opted into a feature.</span></span> <span data-ttu-id="cc7b6-205">在集合初始化運算式類型的案例中，可以藉由執行介面 `IEnumerable` （非泛型）來加入宣告功能。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-205">In the case of collection initializers types can opt into the feature by implementing the interface `IEnumerable` (non generic).</span></span>

<span data-ttu-id="cc7b6-206">此提案一開始要求型別必須執行 `ICollection`，才能限定為可編制索引。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-206">This proposal initially required that types implement `ICollection` in order to qualify as Indexable.</span></span> <span data-ttu-id="cc7b6-207">不過，這需要幾個特殊案例：</span><span class="sxs-lookup"><span data-stu-id="cc7b6-207">That required a number of special cases though:</span></span>

- <span data-ttu-id="cc7b6-208">`ref struct`：這些型別無法實作為型別（例如 `Span<T>`，非常適合索引/範圍的支援）。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-208">`ref struct`: these cannot implement interfaces yet types like `Span<T>` are ideal for index / range support.</span></span> 
- <span data-ttu-id="cc7b6-209">`string`：不會執行 `ICollection`，而且加入該 `interface` 的成本會很大。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-209">`string`: does not implement `ICollection` and adding that `interface` has a large cost.</span></span>

<span data-ttu-id="cc7b6-210">這表示必須要有特殊大小寫，才能支援金鑰類型。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-210">This means to support key types special casing is already needed.</span></span> <span data-ttu-id="cc7b6-211">`string` 的特殊大小寫較不有趣，因為語言會在其他區域（`foreach` 減少、常數等等）中執行這項工作。`ref struct` 的特殊大小寫，是因為它的特殊大小寫是整個類別的類型。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-211">The special casing of `string` is less interesting as the language does this in other areas (`foreach` lowering, constants, etc ...). The special casing of `ref struct` is more concerning as it's special casing an entire class of types.</span></span> <span data-ttu-id="cc7b6-212">如果它們只具有名為 `Count` 的屬性，且其傳回型別為 `int`，它們就會標示為可編制索引。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-212">They get labeled as Indexable if they simply have a property named `Count` with a return type of `int`.</span></span> 

<span data-ttu-id="cc7b6-213">考慮到設計已經正規化，表示具有屬性 `Count` / `int` `Length` 的任何型別都是可編制索引的。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-213">After consideration the design was normalized to say that any type which has a property `Count` / `Length` with a return type of `int` is Indexable.</span></span> <span data-ttu-id="cc7b6-214">這會移除所有特殊大小寫，即使是 `string` 和陣列也一樣。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-214">That removes all special casing, even for `string` and arrays.</span></span>

### <a name="detect-just-count"></a><span data-ttu-id="cc7b6-215">僅偵測計數</span><span class="sxs-lookup"><span data-stu-id="cc7b6-215">Detect just Count</span></span>

<span data-ttu-id="cc7b6-216">偵測屬性名稱 `Count` 或 `Length` 會使設計變得更複雜。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-216">Detecting on the property names `Count` or `Length` does complicate the design a bit.</span></span> <span data-ttu-id="cc7b6-217">只挑選一個進行標準化並不足夠，因為它最後會排除大量的類型：</span><span class="sxs-lookup"><span data-stu-id="cc7b6-217">Picking just one to standardize though is not sufficient as it ends up excluding a large number of types:</span></span>

- <span data-ttu-id="cc7b6-218">使用 `Length`：在系統集合和子命名空間中，排除非常多的每個集合。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-218">Use `Length`: excludes pretty much every collection in System.Collections and sub-namespaces.</span></span> <span data-ttu-id="cc7b6-219">這些通常是從 `ICollection` 衍生而來，因此偏好 `Count` 超過長度。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-219">Those tend to derive from `ICollection` and hence prefer `Count` over length.</span></span>
- <span data-ttu-id="cc7b6-220">使用 `Count`：排除 `string`、陣列、`Span<T>` 和大部分以 `ref struct` 為基礎的類型</span><span class="sxs-lookup"><span data-stu-id="cc7b6-220">Use `Count`: excludes `string`, arrays, `Span<T>` and most `ref struct` based types</span></span>

<span data-ttu-id="cc7b6-221">針對可編制索引之型別的初始偵測，其額外的複雜之處在于其在其他方面的簡化遠遠超過。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-221">The extra complication on the initial detection of Indexable types is outweighed by its simplification in other aspects.</span></span>

### <a name="choice-of-slice-as-a-name"></a><span data-ttu-id="cc7b6-222">選擇配量作為名稱</span><span class="sxs-lookup"><span data-stu-id="cc7b6-222">Choice of Slice as a name</span></span>

<span data-ttu-id="cc7b6-223">已選擇名稱 `Slice`，因為它是 .NET 中配量樣式作業的實際標準名稱。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-223">The name `Slice` was chosen as it's the de-facto standard name for slice style operations in .NET.</span></span> <span data-ttu-id="cc7b6-224">從 netcoreapp 2.1 開始，所有範圍樣式類型都會針對切割作業使用名稱 `Slice`。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-224">Starting with netcoreapp2.1 all span style types use the name `Slice` for slicing operations.</span></span> <span data-ttu-id="cc7b6-225">在 netcoreapp 2.1 之前，不會有任何配量範例來尋找範例。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-225">Prior to netcoreapp2.1 there really aren't any examples of slicing to look to for an example.</span></span> <span data-ttu-id="cc7b6-226">`List<T>`、`ArraySegment<T>`、`SortedList<T>` 之類的類型，很適合用於切割，但當加入類型時，概念並不存在。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-226">Types like `List<T>`, `ArraySegment<T>`, `SortedList<T>` would've been ideal for slicing but the concept didn't exist when types were added.</span></span> 

<span data-ttu-id="cc7b6-227">因此，`Slice` 是唯一的範例，就是選擇做為名稱。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-227">Thus, `Slice` being the sole example, it was chosen as the name.</span></span>

### <a name="index-target-type-conversion"></a><span data-ttu-id="cc7b6-228">索引目標型別轉換</span><span class="sxs-lookup"><span data-stu-id="cc7b6-228">Index target type conversion</span></span>

<span data-ttu-id="cc7b6-229">另一個在索引子運算式中查看 `Index` 轉換的方法，就是做為目標型別轉換。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-229">Another way to view the `Index` transformation in an indexer expression is as a target type conversion.</span></span> <span data-ttu-id="cc7b6-230">語言會改為將目標具類型的轉換指派給 `int`，而不是系結，如同 `return_type this[Index]`表單的成員。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-230">Instead of binding as if there is a member of the form `return_type this[Index]`, the language instead assigns a target typed conversion to `int`.</span></span> 

<span data-ttu-id="cc7b6-231">此概念可以一般化至計算類型的所有成員存取。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-231">This concept could be generalized to all member access on Countable types.</span></span> <span data-ttu-id="cc7b6-232">每當具有類型 `Index` 的運算式當做實例成員調用的引數使用，而接收者是計算時，則運算式會有目標型別轉換成 `int`。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-232">Whenever an expression with type `Index` is used as an argument to an instance member invocation and the receiver is Countable then the expression will have a target type conversion to `int`.</span></span> <span data-ttu-id="cc7b6-233">適用于這種轉換的成員調用包括方法、索引子、屬性、擴充方法等等。只有不會排除的函式，因為它們沒有接收者。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-233">The member invocations applicable for this conversion include methods, indexers, properties, extension methods, etc ... Only constructors are excluded as they have no receiver.</span></span> 

<span data-ttu-id="cc7b6-234">針對具有 `Index`類型的任何運算式，目標型別轉換將會實作為下列各項。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-234">The target type conversion will be implemented as follows for any expression which has a type of `Index`.</span></span> <span data-ttu-id="cc7b6-235">為了方便討論，我們使用 `receiver[expr]`的範例：</span><span class="sxs-lookup"><span data-stu-id="cc7b6-235">For discussion purposes lets use the example of `receiver[expr]`:</span></span>

- <span data-ttu-id="cc7b6-236">當 `expr` 的格式為 `^expr2` 且 `expr2` 的類型為 `int`時，會將其轉譯為 `receiver.Length - expr2`。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-236">When `expr` is of the form `^expr2` and the type of `expr2` is `int`, it will be translated to `receiver.Length - expr2`.</span></span>
- <span data-ttu-id="cc7b6-237">否則，它將會轉譯為 `expr.GetOffset(receiver.Length)`。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-237">Otherwise, it will be translated as `expr.GetOffset(receiver.Length)`.</span></span>

<span data-ttu-id="cc7b6-238">`receiver` 和 `Length` 運算式會適當地溢出，以確保任何副作用只會執行一次。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-238">The `receiver` and `Length` expressions will be spilled as appropriate to ensure any side effects are only executed once.</span></span> <span data-ttu-id="cc7b6-239">例如：</span><span class="sxs-lookup"><span data-stu-id="cc7b6-239">For example:</span></span>

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

<span data-ttu-id="cc7b6-240">此程式碼會列印「Get Length 3」。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-240">This code will print "Get Length 3".</span></span> 

<span data-ttu-id="cc7b6-241">對於具有代表索引之參數的任何成員而言，這項功能會很有説明。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-241">This feature would be beneficial to any member which had a parameter that represented an index.</span></span> <span data-ttu-id="cc7b6-242">例如 `List<T>.InsertAt` 。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-242">For example `List<T>.InsertAt`.</span></span> <span data-ttu-id="cc7b6-243">這也可能會造成混淆，因為語言無法提供任何有關運算式是否適用于編制索引的指引。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-243">This also has the potential for confusion as the language can't give any guidance as to whether or not an expression is meant for indexing.</span></span> <span data-ttu-id="cc7b6-244">它可以做的就是在計算型別上叫用成員時，將任何 `Index` 運算式轉換成 `int`。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-244">All it can do is convert any `Index` expression to `int` when invoking a member on a Countable type.</span></span> 

<span data-ttu-id="cc7b6-245">限制：</span><span class="sxs-lookup"><span data-stu-id="cc7b6-245">Restrictions:</span></span>

- <span data-ttu-id="cc7b6-246">只有當類型為 `Index` 的運算式直接是成員的引數時，才適用這種轉換。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-246">This conversion is only applicable when the expression with type `Index` is directly an argument to the member.</span></span> <span data-ttu-id="cc7b6-247">它不適用於任何嵌套的運算式。</span><span class="sxs-lookup"><span data-stu-id="cc7b6-247">It would not apply to any nested expressions.</span></span>

## <a name="decisions-made-during-implementation"></a><span data-ttu-id="cc7b6-248">執行期間所做的決策</span><span class="sxs-lookup"><span data-stu-id="cc7b6-248">Decisions made during implementation</span></span>

- <span data-ttu-id="cc7b6-249">模式中的所有成員都必須是實例成員</span><span class="sxs-lookup"><span data-stu-id="cc7b6-249">All members in the pattern must be instance members</span></span>
- <span data-ttu-id="cc7b6-250">如果找到長度方法，但其傳回類型錯誤，請繼續尋找計數</span><span class="sxs-lookup"><span data-stu-id="cc7b6-250">If a Length method is found but it has the wrong return type, continue looking for Count</span></span>
- <span data-ttu-id="cc7b6-251">用於索引模式的索引子必須只有一個 int 參數</span><span class="sxs-lookup"><span data-stu-id="cc7b6-251">The indexer used for the Index pattern must have exactly one int parameter</span></span>
- <span data-ttu-id="cc7b6-252">用於範圍模式的配量方法必須剛好有兩個 int 參數</span><span class="sxs-lookup"><span data-stu-id="cc7b6-252">The Slice method used for the Range pattern must have exactly two int parameters</span></span>
- <span data-ttu-id="cc7b6-253">尋找模式成員時，我們會尋找原始定義，而不是結構化成員</span><span class="sxs-lookup"><span data-stu-id="cc7b6-253">When looking for the pattern members, we look for original definitions, not constructed members</span></span>

## <a name="design-meetings"></a><span data-ttu-id="cc7b6-254">設計會議</span><span class="sxs-lookup"><span data-stu-id="cc7b6-254">Design Meetings</span></span>

- https://github.com/dotnet/csharplang/blob/master/meetings/2019/LDM-2019-04-01.md

## <a name="questions"></a><span data-ttu-id="cc7b6-255">問題</span><span class="sxs-lookup"><span data-stu-id="cc7b6-255">Questions</span></span>
