---
ms.openlocfilehash: 6088a5cd41926d828013f1b8e5736fd2b7939e44
ms.sourcegitcommit: da452002c3f472165a0e1fa7759f494cc703ae31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "79485039"
---
# <a name="compile-time-enforcement-of-safety-for-ref-like-types"></a>類似 ref 類型的編譯時間強制執行安全性

## <a name="introduction"></a>簡介

處理 `Span<T>` 和 `ReadOnlySpan<T>` 等類型時，其他安全性規則的主要原因是，這類類型必須限制在執行堆疊中。
 
有兩個原因： `Span<T>` 和類似的類型必須是僅限堆疊的類型。

1. `Span<T>` 在語義上是包含參考和範圍 `(ref T data, int length)`的結構。 不論實際的實值為何，對這類結構的寫入都不是不可部分完成的。 這類結構的並行「撕裂」可能會導致 `length` 不符合 `data`，因而導致超出範圍的存取和型別安全違規，最終可能會導致在看似「安全」的程式碼中發生 GC 堆積損毀。
2. 某些 `Span<T>` 的執行實際上會在其中一個欄位中包含 managed 指標。 Managed 指標不支援做為堆積物件的欄位，以及管理將 managed 指標放在 GC 堆積上的程式碼，通常會在 JIT 時損毀。

如果 `Span<T>` 的實例被限制為只存在於執行堆疊上，則上述所有問題都可以緩解。 

造成其他問題的原因是組合。 通常最好是建立更複雜的資料類型，以內嵌 `Span<T>` 和 `ReadOnlySpan<T>` 實例。 這類複合類型必須是結構，而且會共用 `Span<T>`的所有風險和需求。 因此，此處所述的安全性規則應視為適用于 **_類似 ref 類型_** 的完整範圍。

[草稿語言規格](#draft-language-specification)的目的是要確保類似 ref 類型的值只會出現在堆疊上。

## <a name="generalized-ref-like-types-in-source-code"></a>原始碼中的一般化 `ref-like` 類型

`ref-like` 結構會使用 `ref` 修飾詞，在原始程式碼中明確標示：

```csharp
ref struct TwoSpans<T>
{
    // can have ref-like instance fields
    public Span<T> first;
    public Span<T> second;
} 

// error: arrays of ref-like types are not allowed. 
TwoSpans<T>[] arr = null;

```

將結構指定為類似參考，可讓結構擁有類似 ref 的實例欄位，也會讓類似 ref 類型的所有需求都適用于結構。 

## <a name="metadata-representation-or-ref-like-structs"></a>中繼資料標記法或類似參考的結構

類似 Ref 的結構將會以**CompilerServices. IsRefLikeAttribute**屬性標記。

屬性會加入至一般基底程式庫，例如 `mscorlib`。 在無法使用屬性的情況下，編譯器會產生與其他內嵌隨選屬性（如 `IsReadOnlyAttribute`）類似的內部。

系統會採用額外的量值，以防止在編譯器中使用類似 ref 的結構，而不熟悉安全性規則（這C#包括執行這項功能之前的編譯器）。 

沒有其他可在舊編譯器中使用但未提供服務的其他好方法，將會在所有 ref like 結構中加入具有已知字串的 `Obsolete` 屬性。 知道如何使用類似 ref 類型的編譯器將會忽略這種特定形式的 `Obsolete`。

典型的中繼資料標記法：

```csharp
    [IsRefLike]
    [Obsolete("Types with embedded references are not supported in this version of your compiler.")]
    public struct TwoSpans<T>
    {
       // . . . .
    }
```

注意：這並不是為了讓舊版編譯器上的類似 ref 型別使用的目標失敗100%。 這很難達成，而且不是絕對必要的。 例如，一律會有方法可以使用動態程式碼來避開 `Obsolete`，例如透過反映建立類似 ref 類型的陣列。

特別是，如果使用者想要實際將 `Obsolete` 或 `Deprecated` 屬性放在類似 ref 的型別上，我們將不會發出預先定義的選項，因為 `Obsolete` 屬性無法套用一次以上。  

## <a name="examples"></a>範例：

```csharp
SpanLikeType M1(ref SpanLikeType x, Span<byte> y)
{
    // this is all valid, unconcerned with stack-referring stuff
    var local = new SpanLikeType(y);
    x = local;
    return x;
}

void Test1(ref SpanLikeType param1, Span<byte> param2)
{
    Span<byte> stackReferring1 = stackalloc byte[10];
    var stackReferring2 = new SpanLikeType(stackReferring1);

    // this is allowed
    stackReferring2 = M1(ref stackReferring2, stackReferring1);

    // this is NOT allowed
    stackReferring2 = M1(ref param1, stackReferring1);

    // this is NOT allowed
    param1 = M1(ref stackReferring2, stackReferring1);

    // this is NOT allowed
    param2 = stackReferring1.Slice(10);

    // this is allowed
    param1 = new SpanLikeType(param2);

    // this is allowed
    stackReferring2 = param1;
}

ref SpanLikeType M2(ref SpanLikeType x)
{
    return ref x;
}

ref SpanLikeType Test2(ref SpanLikeType param1, Span<byte> param2)
{
    Span<byte> stackReferring1 = stackalloc byte[10];
    var stackReferring2 = new SpanLikeType(stackReferring1);

    ref var stackReferring3 = M2(ref stackReferring2);

    // this is allowed
    stackReferring3 = M1(ref stackReferring2, stackReferring1);

    // this is allowed
    M2(ref stackReferring3) = stackReferring2;

    // this is NOT allowed
    M1(ref param1) = stackReferring2;

    // this is NOT allowed
    param1 = stackReferring3;

    // this is NOT allowed
    return ref stackReferring3;

    // this is allowed
    return ref param1;
}

```

----------------

## <a name="draft-language-specification"></a>草稿語言規格

下面我們會針對類似 ref 的類型（`ref struct`s）描述一組安全性規則，以確保這些類型的值只會出現在堆疊上。 如果區域變數無法以傳址方式傳遞，則可能會有不同、較簡單的安全性規則。 此規格也允許重新指派 ref 區域變數的安全。

### <a name="overview"></a>概觀

在編譯時期，我們會將與每個運算式相關聯的概念，是運算式允許的範圍，也就是「安全到 escape」。 同樣地，對於每個左值，我們都會維護一個概念，說明允許其參考的範圍（「ref-安全到 escape」）。 針對指定的左值運算式，這些可能會不同。

這些類似 ref 區域變數功能的「安全傳回」，但更精細。 運算式的「安全轉型」只會記錄（或不是）它是否可以將封入方法完全以完整的方式來進行封入，此安全轉型會記錄它可能會被跳到的範圍（它可能不會被 escape 的範圍）。 基本的安全機制會強制執行，如下所示。 假設從具有安全對 escape 範圍 S1 的運算式 E1 指派到（左值）運算式，並具有安全對 escape 範圍 S2，如果 S2 的範圍超出 S1，則會產生錯誤。 根據結構，S1 和 S2 這兩個範圍是在一個嵌套關聯性中，因為合法的運算式一律會從包含該運算式的某個範圍內恢復安全。

在這段時間內，為了分析的目的，只支援兩個範圍-外部至方法，以及方法的最上層範圍。 這是因為無法建立具有內部範圍的 ref like 值，而且 ref 區域變數不支援重新指派。 不過，這些規則可支援兩個以上的範圍層級。

計算運算式之*安全回復*狀態的精確規則，以及管理運算式之合法性的規則，請遵循。

### <a name="ref-safe-to-escape"></a>ref-安全到 escape

「 *Ref-安全轉型*」是一種範圍，其中包含左值運算式，可安全地供左值的 ref 進行換用。 如果該範圍是整個方法，我們會假設左值的 ref 可*安全地*從方法傳回。

### <a name="safe-to-escape"></a>安全對 escape

「*安全轉型*」是一種範圍，其中包含一個運算式，可安全地將此值換成。 如果該範圍是整個方法，我們會說，此值可*安全地*從方法傳回。

其類型不是 `ref struct` 類型的運算式，可從整個封入方法中*安全地*傳回。 否則，我們會參考下列規則。

#### <a name="parameters"></a>參數

指定型式參數的左值是*ref-安全到 escape* （依參考），如下所示：
- 如果參數是 `ref`、`out`或 `in` 參數，則從整個方法（例如，透過 `return ref` 語句），它是*ref-safe to escape* ;別的
- 如果參數是結構類型的 `this` 參數，則它是對方法的最上層範圍（但不是從整個方法本身）進行的*ref-安全*轉型。[範例](#struct-this-escape)
- 否則，參數會是值參數，而且它是對方法的最上層範圍（但不是從方法本身）進行*ref-安全*轉型。

表示使用型式參數的運算式，是從整個方法（例如，透過 `return` 語句）的*安全對 escape* （藉由值）。 這也適用于 `this` 參數。

#### <a name="locals"></a>區域變數

指定區域變數的左值是*ref-安全對 escape （以*傳址方式），如下所示：
- 如果變數是 `ref` 變數，則會從其初始化運算式的*ref 安全轉轉義來*取得其*ref-安全到 escape* 。別的
- 變數是 ref 安全的，會將其宣告所在的範圍*設為 escape* 。

為右值的運算式，指定區域變數的使用方式，是*安全對 escape* （以傳值方式），如下所示：
- 但是上述的一般規則，其類型不是 `ref struct` 類型的本機，則是從整個封入方法中*安全地*傳回。
- 如果變數是 `foreach` 迴圈的反復專案變數，則變數的*安全到 escape*範圍會與 `foreach` 迴圈運算式的*安全對 escape*相同。
- `ref struct` 類型的本機，而且在宣告點未初始化的區域，會從整個封入方法中*安全地*傳回。
- 否則，變數的類型會是 `ref struct` 類型，而變數的宣告則需要初始化運算式。 變數的*安全到 escape*範圍與其初始化運算式的*安全對 escape*是相同的。

#### <a name="field-reference"></a>欄位參考

左值，指定對欄位的參考（`e.F`）是*ref-安全對 escape* （依參考），如下所示：
- 如果 `e` 是參考型別，則會從整個方法中*以 ref 安全*的方式進行 escape;別的
- 如果 `e` 是實值型別，則會從 `e`的*ref 安全到 escape* ，取得其*ref 安全到 escape* 。

指定欄位（`e.F`）之參考的右值，具有與 `e`的*安全到 escape*相同的*安全轉轉義*範圍。

#### <a name="operators-including-"></a>包括 `?:` 的運算子

使用者定義運算子的應用會被視為方法調用。

針對產生右值的運算子（例如 `e1 + e2` 或 `c ? e1 : e2`），結果的*安全對 escape*是運算子之運算元的*安全轉轉義*之間最少的範圍。  因此，如果是會產生右值的一元運算子，例如 `+e`，則結果的*安全對 escape*是運算元的*安全*轉型。

適用于產生左值的運算子，例如 `c ? ref e1 : ref e2`
- 結果的*ref 安全 to escape*是運算子之運算元的*ref 安全對 escape*之間最小的範圍。
- 這些運算元的*安全轉轉義*必須同意，這是產生之左值的*安全對 escape* 。

#### <a name="method-invocation"></a>方法引動過程

由 ref 傳回方法叫用所產生的左值，`e1.M(e2, ...)` 是*ref-安全地*將下列範圍的最小值轉義：
- 整個封入方法
- 所有 `ref` 和 `out` 引數運算式的*ref-安全-escape* （接收者除外）
- 針對方法的每個 `in` 參數，如果有對應的運算式為左值，則為其*ref-安全到 escape*，否則為最接近的封入範圍
- 所有引數運算式的*安全對 escape* （包括接收者）

> 注意：必須要有最後一個專案符號，才能處理常式代碼，例如
> ```csharp
> var sp = new Span(...)
> return ref sp[0];
> ```
> 或
> ```csharp
> return ref M(sp, 0);
> ```

從方法叫用所產生的右值 `e1.M(e2, ...)` 可從下列最小的範圍中獲得*安全的轉義*：
- 整個封入方法
- 所有引數運算式的*安全對 escape* （包括接收者）

#### <a name="an-rvalue"></a>右值
右值是從最接近的封入範圍進行*ref-安全到 escape* 。 這種情況發生在類似 `M(ref d.Length)` 的調用中，例如，`d` 的類型 `dynamic`。 它也會與（也可能是 subsumes）我們處理對應至 `in` 參數的引數一致。 *

#### <a name="property-invocations"></a>屬性調用

屬性調用（`get` 或 `set`）會被視為上述規則的基礎方法的方法調用。

#### `stackalloc`

Stackalloc 運算式是可*安全地*對方法之最上層範圍（而不是從整個方法本身）進行轉義的右值。

#### <a name="constructor-invocations"></a>函式呼叫

叫用遵守函式的 `new` 運算式，與視為傳回所要建立之類型的方法調用的規則相同。

此外，如果有初始化運算式，則不會以遞迴方式，在物件初始化運算式運算式的所有引數/運算元的最小範圍內，不能有更多*的安全對* *escape* 。 

#### <a name="span-constructor"></a>Span 構造函式
語言依賴 `Span<T>` 不具有下列格式的函式：

```csharp
void Example(ref int x)
{
    // Create a span of length one
    var span = new Span<int>(ref x); 
}
```

這類的「函式」會使 `Span<T>` 用來做為與 `ref` 欄位不區分的欄位。 本檔中所述的安全性規則取決於或 .NET 中C#的 `ref` 欄位不是有效的結構。

#### <a name="default-expressions"></a>`default` 運算式

`default` 運算式可從整個封入方法中進行*安全的轉義*。

## <a name="language-constraints"></a>語言條件約束

我們想要確保沒有任何 `ref` 的區域變數，而且沒有任何 `ref struct` 類型的變數，指的是堆疊記憶體或不再處於作用中的變數。 因此，我們有下列語言條件約束：

- Ref 參數和 ref 區域變數，或 `ref struct` 類型的參數或本機，都無法提升為 lambda 或區域函數。

- Ref 參數和 `ref struct` 類型的參數都不能是反覆運算器方法或 `async` 方法的引數。

- Ref local 和 `ref struct` 類型的本機都不能在 `yield return` 語句或 `await` 運算式的範圍中。

- `ref struct` 類型不能當做類型引數使用，或做為元組類型中的元素類型。

- `ref struct` 類型可能不是欄位的宣告型別，但它可能是另一個 `ref struct`之實例欄位的宣告型別。

- `ref struct` 類型可能不是陣列的元素類型。

- 可能不會將 `ref struct` 類型的值裝箱：
  - 沒有從 `ref struct` 類型轉換成類型 `object` 或類型 `System.ValueType`。
  - 可能不會宣告 `ref struct` 類型來執行任何介面
  - `object` 或 `System.ValueType` 中未宣告的實例方法，但在 `ref struct` 類型中未覆寫，可能會以該 `ref struct` 類型的接收者來呼叫。
  - 方法轉換至委派類型時，不可以捕捉 `ref struct` 類型的實例方法。

- 針對 ref 重新指派 `ref e1 = ref e2`，`e2` 的*ref 安全對 escape*必須至少與 `e1`的*ref 安全到 escape*的範圍相同。

- 針對 ref return 語句 `return ref e1`，`e1` 的*ref 安全對 escape*必須從整個方法中*以 ref 安全*的方式來進行 escape。 （TODO：我們也需要一個規則，`e1` 必須從整個方法進行*安全的 escape* ，或這是多餘的？）

- `return e1`的 return 語句中，`e1` 的*安全對 escape*必須是整個方法的*安全*對 escape。

- 對於指派 `e1 = e2`，如果 `e1` 的類型是 `ref struct` 類型，則 `e2` 的*安全-escape*必須至少與 `e1`的*安全轉轉義*的範圍相同。

- 若為方法叫用，如果 `ref struct` 類型的 `ref` 或 `out` 引數（包括接收者），具有安全的*escape* E1，則沒有引數（包括接收者）可能會比 E1 更窄的*安全到 escape* 。 [範例](#method-arguments-must-match)

- 區域函式或匿名函式不能參考在封閉範圍中宣告之 `ref struct` 類型的本機或參數。

> ***開啟問題：*** 我們需要一些規則，讓我們在需要于 await 運算式溢出 `ref struct` 類型的堆疊值時產生錯誤，例如在程式碼中。
> ```csharp
> Foo(new Span<int>(...), await e2);
> ```

## <a name="explanations"></a>說明
這些說明和範例有助於說明上述許多安全性規則的存在原因

### <a name="method-arguments-must-match"></a>方法引數必須相符
當叫用具有 `out`的方法時，`ref` 參數是 `ref struct` （包括接收者），則所有 `ref struct` 都必須具有相同的存留期。 這是必要的C# ，因為必須根據方法簽章中可用的資訊和呼叫位置的值存留期，讓所有 it 決策的存留期安全。 

當有 `ref` 的參數 `ref struct` 時，就會有這個可能性可以交換其內容。 因此，在呼叫位置，我們必須確定所有這些**可能**的交換都相容。 如果語言未強制執行，則會允許類似下列的錯誤程式碼。

```csharp
void M1(ref Span<int> s1)
{
    Span<int> s2 = stackalloc int[1];
    Swap(ref s1, ref s2);
}

void Swap(ref Span<int> x, ref int Span<int> y)
{
    // This will effectively assign the stackalloc to the s1 parameter and allow it
    // to escape to the caller of M1
    ref x = ref y; 
}
```

接收者的限制是必要的，因為雖然沒有任何內容是 ref 安全的，但它可以儲存提供的值。 這表示，在存留期不相符的情況下，您可以透過下列方式建立型別安全漏洞：

```csharp
ref struct S
{
    public Span<int> Span;

    public void Set(Span<int> span)
    {
        Span = span;
    }
}

void Broken(ref S s)
{
    Span<int> span = stackalloc int[1];

    // The result of a stackalloc is now stored in s.Span and escaped to the caller
    // of Broken
    s.Set(span); 
}
```

### <a name="struct-this-escape"></a>結構此 Escape
當有範圍的安全性規則時，實例成員中的 `this` 值會模型化為成員的參數。 現在，`struct` `this` 的類型實際上是 `ref S` 在 `class` 中，它只會 `S` （針對名為 S 的 `class / struct` 成員）。 

但是 `this` 與其他 `ref` 參數具有不同的轉義規則。 具體而言，它並不是 ref-安全到 escape，而其他參數則是：

```csharp
ref struct S
{ 
    int Field;

    // Illegal because this isn't safe to escape as ref
    ref int Get() => ref Field;

    // Legal
    ref int GetParam(ref int p) => ref p;
}
```

這種限制的原因實際上與 `struct` 成員調用很少。 在接收者為右值的 `struct` 成員上，有一些需要處理成員調用的相關規則。 但這非常平易近人。 

這種限制的原因實際上是關於介面調用。 具體而言，它會向下說明下列範例是否應該或不應該編譯;

```csharp
interface I1
{
    ref int Get();
}

ref int Use<T>(T p)
    where T : I1
{
    return ref p.Get();
}
```

請考慮將 `T` 具現化為 `struct`的情況。 如果 `this` 參數是 ref-安全到 escape，則 `p.Get` 的傳回可能會指向堆疊（具體而言，它可能是 `T`具現化型別內的欄位）。 這表示語言無法允許此範例進行編譯，因為它可能會將 `ref` 傳回堆疊位置。 另一方面，如果 `this` 不是 ref-安全到 escape，那麼 `p.Get` 就無法參考堆疊，因此可以安全地傳回。 

這就是為什麼 `struct` 中的 `this` escapability 其實是關於介面的原因。 它絕對可以運作，但卻有取捨。 設計最後會導致介面變得更有彈性。 

不過，我們未來可能會放寬這項功能。 

## <a name="future-considerations"></a>未來的考量

### <a name="length-one-spant-over-ref-values"></a>長度一個範圍\<T > 超過 ref 值
雖然今天不合法，但在某些情況下，在值上建立一個 `Span<T>` 實例的長度會很有説明：

```csharp
void RefExample()
{
    int x = ...;

    // Today creating a length one Span<int> requires a stackalloc and a new 
    // local
    Span<int> span1 = stackalloc [] { x };
    Use(span1);
    x = span1[0]; 

    // Simpler to just allow length one span
    var span2 = new Span<int>(ref x);
    Use(span2);
}
```

如果我們對[固定大小的緩衝區](https://github.com/dotnet/csharplang/blob/master/proposals/fixed-sized-buffers.md)提出限制，讓 `Span<T>` 的實例變得更長，這項功能會更具吸引力。 

如果您需要關閉此路徑，則語言可以藉由確保這類 `Span<T>` 實例僅限在使用中，來滿足此需求。 也就是說，它們只會在建立的範圍內受到*保護*。 這可確保語言永遠不需要考慮 `ref` 值透過 `ref struct`的 `ref struct` 傳回或欄位來將方法進行轉義。 不過，這可能也需要進一步的變更來辨識這類的函式，以這種方式來捕捉 `ref` 參數。
