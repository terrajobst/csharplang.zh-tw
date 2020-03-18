---
ms.openlocfilehash: 39fb0aab5e8bb0d422f25fd2e92ab3d8256d3f59
ms.sourcegitcommit: b8f1103eb686c5d82e294837c9386d9b667da292
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2019
ms.locfileid: "79485109"
---
# <a name="readonly-references"></a>唯讀參考

* [x] 提議
* [x] 原型
* [x] 執行：已啟動
* [] 規格：未啟動

## <a name="summary"></a>摘要
[summary]: #summary

「Readonly 參考」功能實際上是一組功能，可利用以傳址方式傳遞變數的效率，但不會公開要修改的資料：  
- `in` 參數
- `ref readonly` 傳回
- `readonly` 結構
- `ref`/`in` 擴充方法
- `ref readonly` 區域變數
- `ref` 條件運算式

## <a name="passing-arguments-as-readonly-references"></a>將引數傳遞為 readonly 參考。

現有的提案會將本主題 https://github.com/dotnet/roslyn/issues/115 視為唯讀參數的特殊案例，而不會有許多詳細資料。
我只想要知道自己的想法不是非常新的。

### <a name="motivation"></a>動機

在此功能C#之前，不會有有效率的方式來表示想要將結構變數傳遞至方法呼叫以供 readonly 使用，而不想要修改。 標準的傳值引數會隱含複製，這會增加不必要的成本。  這會讓使用者使用 ref 引數傳遞並依賴批註/檔，以指出資料不應該由被呼叫端進行變化。 這不是很好的解決方案，因為有許多原因。  
這些範例是圖形程式庫中的許多向量/矩陣數學運算子，例如[，同型元件已知會因為](https://msdn.microsoft.com/library/bb194944.aspx)效能考慮而純粹具有 ref 運算元。 Roslyn 編譯器本身有程式碼，它會使用結構來避免配置，然後以傳址方式傳遞它們以避免複製成本。

### <a name="solution-in-parameters"></a>方案（`in` 參數）

類似于 `out` 參數，`in` 參數會以 managed 參考的形式傳遞，並具有被呼叫端的其他保證。  
不同于在任何其他用途之前_必須_由被呼叫端指派的 `out` 參數，`in` 的參數完全無法由被呼叫端指派。

因此 `in` 參數可有效傳遞間接引數，而不會將引數公開給被呼叫端變化。

### <a name="declaring-in-parameters"></a>宣告 `in` 參數

`in` 參數是使用 `in` 關鍵字來宣告，做為參數簽章中的修飾詞。

就所有用途而言，會將 `in` 參數視為 `readonly` 變數。 在方法內使用 `in` 參數的大部分限制，與 `readonly` 欄位相同。

> 事實上，`in` 參數可能代表 `readonly` 欄位。 限制的相似性並不是巧合。

例如，具有結構類型之 `in` 參數的欄位，都會以遞迴方式分類為 `readonly` 變數。

```csharp
static Vector3 Add (in Vector3 v1, in Vector3 v2)
{
    // not OK!!
    v1 = default(Vector3);

    // not OK!!
    v1.X = 0;

    // not OK!!
    foo(ref v1.X);

    // OK
    return new Vector3(v1.X + v2.X, v1.Y + v2.Y, v1.Z + v2.Z);
}
```

- 允許使用一般 byval 參數的任何地方都允許 `in` 參數。 這包括索引子、運算子（包括轉換）、委派、lambda、區域函數。

> ```csharp
>  (in int x) => x                                                     // lambda expression  
>  TValue this[in TKey index];                                         // indexer
>  public static Vector3 operator +(in Vector3 x, in Vector3 y) => ... // operator
>  ```

- `in` 不允許結合 `out` 或 `out` 未與結合的任何專案。

- `ref`/`out`/`in` 差異，則不允許多載。

- 允許在一般的 byval 上多載，並 `in` 差異。

- 基於 OHI 的目的（多載、隱藏、執行），`in` 的行為類似 `out` 參數。
所有相同的規則都適用。
例如，覆寫方法必須符合可轉換類型之 `in` 參數的 `in` 參數。

- 為了委派/lambda/方法群組轉換的目的，`in` 的行為類似于 `out` 參數。
Lambda 和適用的方法群組轉換候選項目必須符合目標委派 `in` 參數，以及可轉換身分識別類型的 `in` 參數。

- 基於泛型變異數的目的，`in` 參數 nonvariant。

> 注意：對於具有參考或基本類型的 `in` 參數，不會出現任何警告。
這在一般情況下可能毫無意義，但在某些情況下，使用者必須/想以 `in`傳遞基本類型。 範例-覆寫泛型方法（例如 `Method(in T param)`，當 `T` 取代為 `int`時，或具有如的方法時 `Volatile.Read(in int location)`
>
> 有一個分析器會在使用 `in` 參數的效率不佳的情況下發出警告，但這類分析的規則會太模糊而無法成為語言規格的一部分。

### <a name="use-of-in-at-call-sites-in-arguments"></a>在呼叫網站使用 `in`。 （`in` 引數）

有兩種方式可以將引數傳遞給 `in` 參數。

#### <a name="in-arguments-can-match-in-parameters"></a>`in` 引數可以符合 `in` 參數：

在呼叫位置具有 `in` 修飾詞的引數，可以符合 `in` 參數。

```csharp
int x = 1;

void M1<T>(in T x)
{
  // . . .
}

var x = M1(in x);  // in argument to a method

class D
{
    public string this[in Guid index];
}

D dictionary = . . . ;
var y = dictionary[in Guid.Empty]; // in argument to an indexer
```

- `in` 引數必須是_可讀取_的左值（*）。
範例： `M1(in 42)` 無效

> (*)[左值/右](https://en.wikipedia.org/wiki/Value_(computer_science)#lrvalue)值的概念會因語言而異。  
在這裡，按左值 I 表示一個運算式，代表可以直接參考的位置。
和右值表示一個運算式，其會產生不會自行保存的暫存結果。  

- 特別是，傳遞 `readonly` 欄位、`in` 參數或其他正式 `readonly` 變數做為 `in` 引數，是有效的。
範例： `dictionary[in Guid.Empty]` 是合法的。 `Guid.Empty` 是靜態的唯讀欄位。

- `in` 引數必須具有類型身分識別，才能_轉換_為參數的類型。
範例： `M1<object>(in Guid.Empty)` 無效。 `Guid.Empty` 無法識別為可_轉換_成 `object`

上述規則的動機在於，`in` 引數可保證引數變數的_別名_。 被呼叫端一律會收到引數所表示之相同位置的直接參考。

- 在罕見的情況下，因為 `await` 運算式做為相同呼叫的運算元，所以 `in` 引數必須堆疊溢位，其行為與 `out` 和 `ref` 引數相同-如果變數無法以參考上透明的方式溢出，則會報告錯誤。

範例：
1. `M1(in staticField, await SomethingAsync())` 有效。
`staticField` 是一個可以多次存取的靜態欄位，而不會有可觀察的副作用。 因此，也可以提供副作用和別名需求的順序。
2. `M1(in RefReturningMethod(), await SomethingAsync())` 將會產生錯誤。
`RefReturningMethod()` 是傳回方法的 `ref`。 方法呼叫可能會有明顯的副作用，因此必須在 `SomethingAsync()` 運算元之前評估。 不過，叫用的結果是無法跨 `await` 暫停點保留的參考，因此不可能進行直接參考需求。

> 注意：堆疊溢位錯誤會被視為是實作為特定的限制。 因此，它們不會影響多載解析或 lambda 推斷。

#### <a name="ordinary-byval-arguments-can-match-in-parameters"></a>一般的 byval 引數可以符合 `in` 參數：

不含修飾詞的一般引數可以符合 `in` 參數。 在這種情況下，引數具有與一般 byval 引數相同的寬鬆條件約束。

此案例的動機在於，當引數不能當做直接參考（例如：常值、計算或 `await`ed 結果，或有更特定類型的引數）傳遞時，Api 中 `in` 的參數可能會導致使用者不便。  
所有這些案例都有一個簡單的解決方案，就是將引數值儲存在適當類型的暫存區域中，並將該本機當做 `in` 引數來傳遞。  
若要減少這類樣板化程式碼的需求，編譯器可以視需要執行相同的轉換，而不會在呼叫位置出現 `in` 修飾詞。  

此外，在某些情況下（例如運算子的調用）或 `in` 擴充方法，沒有任何語法方式可以指定 `in`。 這只需要在符合 `in` 參數時，指定一般 byval 引數的行為。

尤其是：

- 傳遞右值是有效的。
在這種情況下，會傳遞暫存的參考。
範例：
```csharp
Print("hello");      // not an error.

void Print<T>(in T x)
{
  //. . .
}
```

- 允許隱含轉換。

> 這實際上是傳遞右值的特殊案例  

在這種情況下，會傳遞暫時保留轉換值的參考。
範例：
```csharp
Print<int>(Short.MaxValue)     // not an error.
```

- 在 `in` 擴充方法的接收者案例中（相對於 `ref` 擴充方法），則允許右值或隱含的_這個引數轉換_。
在這種情況下，會傳遞暫時保留轉換值的參考。
範例：
```csharp
public static IEnumerable<T> Concat<T>(in this (IEnumerable<T>, IEnumerable<T>) arg)  => . . .;

("aa", "bb").Concat<char>()    // not an error.
```
本檔會進一步提供有關 `ref`/`in` 擴充方法的詳細資訊。

- 因 `await` 運算元而造成的引數溢出可能會在必要時，以傳值方式溢出。
在提供引數直接參考的情況下，由於中間 `await`，會改為溢出引數值的複本。  
範例：
```csharp
M1(RefReturningMethod(), await SomethingAsync())   // not an error.
```
因為副作用調用的結果是無法在 `await` 暫停之間保留的參考，所以會改為保留包含實際值的暫存（如同在一般的 byval 參數案例中一樣）。

#### <a name="omitted-optional-arguments"></a>省略的選擇性引數

允許 `in` 參數指定預設值。 讓對應引數成為選擇性的。

省略呼叫位置的選擇性引數會導致透過暫時性傳遞預設值。

```csharp
Print("hello");      // not an error, same as
Print("hello", c: Color.Black);

void Print(string s, in Color c = Color.Black)
{
    // . . .
}
```

### <a name="aliasing-behavior-in-general"></a>一般的別名行為

就像 `ref` 和 `out` 變數一樣，`in` 變數是現有位置的參考/別名。

雖然無法將被呼叫端寫入其中，但讀取 `in` 參數可能會觀察不同的值，做為其他評估的副作用。

範例：

```C#
static Vector3 v = Vector3.UnitY;

static void Main()
{
    Test(v);
}

static void Test(in Vector3 v1)
{
    Debug.Assert(v1 == Vector3.UnitY);
    // changes v1 deterministically (no races required)
    ChangeV();
    Debug.Assert(v1 == Vector3.UnitX);
}

static void ChangeV()
{
    v = Vector3.UnitX;
}
```

### <a name="in-parameters-and-capturing-of-local-variables"></a>`in` 參數和區域變數的捕捉。  
基於 lambda/非同步捕捉的目的，`in` 參數的行為與 `out` 和 `ref` 參數相同。

- 無法在關閉中捕捉 `in` 參數
- 反覆運算器方法中不允許 `in` 參數
- 非同步方法中不允許 `in` 參數

### <a name="temporary-variables"></a>暫存變數。  
某些使用 `in` 參數傳遞可能需要間接使用暫存區域變數：  
- 當呼叫站使用 `in`時，一律會將 `in` 引數當做直接別名傳遞。 這種情況下永遠不會使用暫時的。
- 當呼叫站未使用 `in`時，`in` 引數不需要是直接別名。 當引數不是左值時，可能會使用暫存。
- `in` 參數可能有預設值。 在呼叫位置省略對應的引數時，預設值會透過暫時性傳遞。
- `in` 引數可能會有隱含的轉換，包括不會保留身分識別的。 在這些情況下，會使用暫時的。
- 一般結構呼叫的接收者可能無法寫入左值（**現有的案例！** ）。 在這些情況下，會使用暫時的。

引數而暫存物件的存留時間符合呼叫網站最接近的範圍。

暫存變數的正式運作時間在包含以傳址方式傳回之變數的 escape 分析的案例中，在語義上很重要。

### <a name="metadata-representation-of-in-parameters"></a>`in` 參數的中繼資料標記法。
當 `System.Runtime.CompilerServices.IsReadOnlyAttribute` 套用至 byref 參數時，表示參數是 `in` 參數。

此外，如果方法是*抽象*或*虛擬*，則這類參數的簽章（而且只有這類參數）必須有 `modreq[System.Runtime.InteropServices.InAttribute]`。

**動機**：這是為了確保在方法覆寫/執行 `in` 參數相符的情況下，會進行這項作業。

相同的需求適用于委派中的 `Invoke` 方法。

**動機**：這是為了確保現有編譯器在建立或指派委派時，不會直接忽略 `readonly`。

## <a name="returning-by-readonly-reference"></a>由 readonly 參考傳回。

### <a name="motivation"></a>動機
這個子功能的動機大致上是對稱于 `in` 參數的原因-避免複製，但在傳回端上。 在此功能之前，方法或索引子有兩個選項：1）以傳址方式傳回，並公開到可能的變化或2）以傳值方式傳回，因而導致複製。

### <a name="solution-ref-readonly-returns"></a>方案（`ref readonly` 傳回）  
此功能可讓成員以傳址方式傳回變數，而不會將它們公開給變化。

### <a name="declaring-ref-readonly-returning-members"></a>宣告 `ref readonly` 傳回成員

傳回簽章 `ref readonly` 的修飾片語合是用來表示成員會傳回 readonly 參考。

就所有用途而言，會將 `ref readonly` 成員視為 `readonly` 變數，類似于 `readonly` 欄位和 `in` 參數。

例如，具有結構類型 `ref readonly` 成員的欄位，都會以遞迴方式分類為 `readonly` 變數。 -允許將它們當做 `in` 引數傳遞，但不能當做 `ref` 或 `out` 引數。

```csharp
ref readonly Guid Method1()
{
}

Method2(in Method1()); // valid. Can pass as `in` argument.

Method3(ref Method1()); // not valid. Cannot pass as `ref` argument
```

- 允許在相同的位置 `ref` 傳回 `ref readonly` 傳回。
這包括索引子、委派、lambda、區域函數。

- 不允許在 `ref`/`ref readonly`/差異上多載。

- 允許在一般的 byval 上多載，並 `ref readonly` 傳回差異。

- 基於 OHI （多載、隱藏、執行）的目的，`ref readonly` 類似，但與 `ref`不同。
例如，覆寫 `ref readonly` 一的方法，本身必須 `ref readonly` 而且具有可轉換身分識別的類型。

- 為了委派/lambda/方法群組轉換的目的，`ref readonly` 類似，但不同于 `ref`。
Lambda 和適用的方法群組轉換候選項目必須符合目標委派 `ref readonly` 傳回，並具有可轉換身分識別之類型的 `ref readonly` 傳回。

- 基於泛型變異數的目的，`ref readonly` 傳回 nonvariant。

> 注意：具有參考或基本類型的 `ref readonly` 傳回不會出現任何警告。
這在一般情況下可能毫無意義，但在某些情況下，使用者必須/想以 `in`傳遞基本類型。 範例-將 `T` 取代為 `int`時，覆寫泛型方法（例如 `ref readonly T Method()`）。
>
>有一個分析器會在使用 `ref readonly` 傳回的效率不佳的情況下發出警告，但這類分析的規則會太模糊而無法成為語言規格的一部分。

### <a name="returning-from-ref-readonly-members"></a>從 `ref readonly` 成員返回
在方法主體中，語法與一般 ref 傳回相同。 系統會從包含的方法推斷 `readonly`。

動機是 `return ref readonly <expression>` 不需要長時間，而且只允許不相符的 `readonly` 部分一律會導致錯誤。
不過，`ref` 是與其他案例一致的情況，其中會透過嚴格的別名和傳值方式來傳遞專案。

> 不同于具有 `in` 參數的案例，`ref readonly` 傳回的永遠不會透過本機複本傳回。 考慮到複製會在傳回這類做法時立即停止存在，是毫無意義且危險的。 因此，`ref readonly` 傳回一律是直接參考。

範例：

```csharp
struct ImmutableArray<T>
{
    private readonly T[] array;

    public ref readonly T ItemRef(int i)
    {
        // returning a readonly reference to an array element
        return ref this.array[i];
    }
}

```

- `return ref` 的引數必須是左值（**現有規則**）
- `return ref` 的引數必須是「安全地傳回」（**現有規則**）
- 在 `ref readonly` 成員中，`return ref` 的引數_不需要是可寫入_的。
例如，這類成員可以 ref-傳回 readonly 欄位或其中一個 `in` 參數。

### <a name="safe-to-return-rules"></a>可安全地傳回規則。
一般安全傳回參考的規則也適用于 readonly 參考。

請注意，您可以從一般的 `ref` 本機/參數/傳回來取得 `ref readonly`，但不能從相反的方式取得。 否則，會以與一般 `ref` 傳回相同的方式推斷 `ref readonly` 傳回的安全。

考慮到右值可以當做 `in` 參數傳遞，並以 `ref readonly` 的方式傳回，我們需要一個以上的規則-**右值不是以傳址方式傳回安全**。

> 當右值透過複製傳遞至 `in` 參數，然後以 `ref readonly`的形式傳回時，請考慮這種情況。 在呼叫端的內容中，這類調用的結果是本機資料的參考，因此不安全傳回。
> 一旦右值不安全地傳回，現有的規則 `#6` 已經處理此情況。

範例：
```csharp
ref readonly Vector3 Test1()
{
    // can pass an RValue as "in" (via a temp copy)
    // but the result is not safe to return
    // because the RValue argument was not safe to return by reference
    return ref Test2(default(Vector3));
}

ref readonly Vector3 Test2(in Vector3 r)
{
    // this is ok, r is returnable
    return ref r;
}
```

已更新 `safe to return` 規則：

1.  **可以安全地傳回堆積上變數的 refs**
2.  **ref/in 參數可安全地**傳回
`in` 參數自然只能以 readonly 的形式傳回。
3.  **out 參數可放心**傳回（但必須明確指派，如同今天的案例）
4.  **只要接收者可以安全傳回，實例結構欄位就可以安全地傳回。**
5.  **' this ' 不安全地從結構成員傳回**
6.  **如果傳遞至該方法做為正式參數的所有 refs/out 都是安全的，則從另一個方法傳回的 ref 會是安全的。** 
*明確地說，如果接收者是結構、類別或型別為泛型型別參數，則不會有任何不相關的情況。*
7.  **右值不安全，無法以傳址方式傳回。** 
*特別右值可安全地當做 in 參數傳遞。*

> 注意：牽涉到類似 ref 型別和 ref-的類型時，還有其他關於傳回安全的規則。
> 規則同樣適用于 `ref` 和 `ref readonly` 成員，因此此處並未提及。

### <a name="aliasing-behavior"></a>別名行為。
`ref readonly` 成員提供與一般 `ref` 成員相同的別名行為（除了為 readonly 以外）。
因此，為了在 lambda、非同步、反覆運算器、堆疊溢位等中進行捕獲，同樣的限制也適用。 亦即. 由於無法捕獲實際的參考，而且因為成員評估有副作用的性質，因此不允許這類案例。

> 當 `ref readonly` 傳回是一般結構方法的接收者時，允許並需要進行複製，這會將 `this` 視為一般的可寫入參考。 在過去，在這類調用套用至 readonly 變數的所有情況下，都會進行本機複本。

### <a name="metadata-representation"></a>中繼資料標記法。
當 `System.Runtime.CompilerServices.IsReadOnlyAttribute` 套用至傳回的 byref 傳回方法時，這表示該方法會傳回 readonly 參考。

此外，這類方法的結果簽章（而且只有那些方法）必須有 `modreq[System.Runtime.CompilerServices.IsReadOnlyAttribute]`。

**動機**：這是為了確保現有編譯器在叫用具有 `ref readonly` 傳回的方法時，不會直接忽略 `readonly`

## <a name="readonly-structs"></a>Readonly 結構
在簡短的功能中，會為結構的所有實例成員 `this` 參數，但不包括函式、`in` 參數。

### <a name="motivation"></a>動機
編譯器必須假設結構實例上的任何方法呼叫可能會修改實例。 事實上，可寫入的參考會以 `this` 參數的形式傳遞至方法，並完整啟用此行為。 為了在 `readonly` 變數上允許這類調用，會將調用套用至暫存複本。 這可能會圈子，而且有時會基於效能考慮，強制使用者放棄 `readonly`。  
範例： https://codeblog.jonskeet.uk/2014/07/16/micro-optimization-the-surprising-inefficiency-of-readonly-fields/

新增 `in` 參數的支援之後，`ref readonly` 會傳回防禦性複製的問題，因為 readonly 變數會變得更常見。

### <a name="solution"></a>解決方法
在結構宣告上允許 `readonly` 修飾詞，這會導致 `this` 被視為所有結構實例方法上的 `in` 參數，但不包括函式。

```csharp
static void Test(in Vector3 v1)
{
    // no need to make a copy of v1 since Vector3 is a readonly struct
    System.Console.WriteLine(v1.ToString());
}

readonly struct Vector3
{
    . . .

    public override string ToString()
    {
        // not OK!!  `this` is an `in` parameter
        foo(ref this.X);

        // OK
        return $"X: {X}, Y: {Y}, Z: {Z}";
    }
}
```

### <a name="restrictions-on-members-of-readonly-struct"></a>Readonly 結構成員的限制
- Readonly 結構的實例欄位必須為 readonly。  
**動機：** 只能寫入外部，而不能透過成員。
- Readonly 結構的實例 autoproperties 必須是「僅限取得」。  
**動機：** 實例欄位的限制結果。
- Readonly 結構可能不會宣告類似欄位的事件。  
**動機：** 實例欄位的限制結果。

### <a name="metadata-representation"></a>中繼資料標記法。
當 `System.Runtime.CompilerServices.IsReadOnlyAttribute` 套用至實數值型別時，表示該類型為 `readonly struct`。

尤其是：
-  `IsReadOnlyAttribute` 類型的識別並不重要。 事實上，編譯器可以視需要將它內嵌在包含元件中。

## <a name="refin-extension-methods"></a>`ref`/`in` 擴充方法
實際上，現有的提案（ https://github.com/dotnet/roslyn/issues/165) 和對應的原型 PR （ https://github.com/dotnet/roslyn/pull/15650)。
我只想知道這個想法並非全新的。 不過，這與此處相關，因為 `ref readonly` 適當地會移除這類方法最爭議的問題，這是如何使用右值接收器。

一般概念是讓擴充方法以傳址方式接受 `this` 參數，只要此型別已知為結構型別即可。

```csharp
public static void Extension(ref this Guid self)
{
    // do something
}
```

撰寫這類擴充方法的原因主要是：  
1.  當接收者是大型結構時避免複製
2.  允許改變結構上的擴充方法

我們不想在類別上允許這種情況的原因  
1.  這是非常有限的用途。
2.  它會中斷長時間的持續性，因為方法呼叫無法在叫用後將非`null` 的接收者變成 `null`。
> 事實上，除非 `ref` 或 `out`_明確_指派或傳遞非`null` 變數，否則無法變成 `null`。
> 這麼做可大幅輔助可讀性或其他形式的「這是 null，這裡」的分析。
3.  使用 null 條件存取的「評估一次」語義，會很難進行協調。
範例： `obj.stringField?.RefExtension(...)`-需要捕捉 `stringField` 的複本，讓 null 檢查有意義，但是在 RefExtension 內 `this` 的指派不會反映回欄位。

在以傳址方式接受第一個引數的**結構**上宣告擴充方法的功能，是長期的要求。 其中一個封鎖考慮是「如果接收者不是左值，會發生什麼事？」。

- 您也可以將任何擴充方法當做靜態方法呼叫（有時候這是解決多義性的唯一方法）。 它會規定應該不允許右值接收器。
- 另一方面，當牽涉到結構實例方法時，會在類似情況下對複本進行調用。

「隱含複製」存在的原因是因為大部分的結構方法實際上不會修改結構，而無法表示。 因此，最實用的解決方法是只在複本上進行調用，但這種作法是已知的效能和造成錯誤。

現在，有 `in` 參數的可用性，延伸模組可能會通知意圖。 因此，可以藉由要求使用可寫入的接收者來呼叫 `ref` 延伸模組來解決之間謎，而 `in` 延伸模組則允許隱含複製（如有必要）。

```csharp
// this can be called on either RValue or an LValue
public static void Reader(in this Guid self)
{
    // do something nonmutating.
    WriteLine(self == default(Guid));
}

// this can be called only on an LValue
public static void Mutator(ref this Guid self)
{
    // can mutate self
    self = new Guid();
}
```

### <a name="in-extensions-and-generics"></a>`in` 擴充功能和泛型。
`ref` 擴充方法的目的，是要直接或藉由叫用改變成員的方式來變更接收器。 因此，只要 `T` 限制為結構，就會允許 `ref this T` 延伸模組。

另一方面，`in` 擴充方法會特別存在，以減少隱含複製。 不過，任何使用 `in T` 參數都必須透過介面成員來完成。 由於所有介面成員都被視為改變，因此任何這類使用都需要複製。 -而不是減少複製，效果會相反。 因此，當 `T` 是泛型型別參數，而不是任何條件約束時，就不允許 `in this T`。

### <a name="valid-kinds-of-extension-methods-recap"></a>有效的延伸方法類型（回顧）：
擴充方法中的下列 `this` 宣告形式現在是允許的：
1) `this T arg` 標準的 byval 延伸模組。 （**現有的案例**）
- T 可以是任何類型，包括參考型別或類型參數。
實例將會是呼叫之後的相同變數。
允許隱含轉換_此引數轉換_種類。
可以在右值上呼叫。

- `in this T self` - `in` 延伸模組。
T 必須是實際的結構類型。
實例將會是呼叫之後的相同變數。
允許隱含轉換_此引數轉換_種類。
可以在右值上呼叫（如有需要，可在暫存上叫用）。

- `ref this T self` - `ref` 延伸模組。
T 必須是結構類型，或限制為結構的泛型型別參數。
實例可能會由調用寫入。
僅允許身分識別轉換。
必須在可寫入的左值上呼叫。 （絕不會透過 temp 叫用）。

## <a name="readonly-ref-locals"></a>Readonly ref 區域變數。

### <a name="motivation"></a>動機.
一旦引進 `ref readonly` 成員之後，就可以清楚地使用它們需要與適當的本機類型配對。 成員的評估可能會產生或觀察副作用，因此，如果結果必須使用超過一次，則需要加以儲存。 一般 `ref` 區域變數在此不會有説明，因為無法為其指派 `readonly` 參考。   

### <a name="solution"></a>解.
允許宣告 `ref readonly` 區域變數。 這是一種新的 `ref` 區域變數，無法寫入。 因此 `ref readonly` 區域變數可以接受 readonly 變數的參考，而不會將這些變數公開給寫入。

### <a name="declaring-and-using-ref-readonly-locals"></a>宣告和使用 `ref readonly` 區域變數。

這類區域變數的語法會在宣告網站使用 `ref readonly` 修飾詞（依該特定順序）。 類似于一般 `ref` 區域變數，`ref readonly` 區域變數必須在宣告時以 ref 初始化。 不同于一般 `ref` 區域變數，`ref readonly` 區域變數可以參考 `readonly` 的左值，例如 `in` 參數、`readonly` 欄位 `ref readonly` 方法。

就所有用途而言，會將 `ref readonly` 本機視為 `readonly` 變數。 使用的大部分限制與 `readonly` 欄位或 `in` 參數相同。

例如，具有結構類型之 `in` 參數的欄位，都會以遞迴方式分類為 `readonly` 變數。   

```csharp
static readonly ref Vector3 M1() => . . .

static readonly ref Vector3 M1_Trace()
{
    // OK
    ref readonly var r1 = ref M1();

    // Not valid. Need an LValue
    ref readonly Vector3 r2 = ref default(Vector3);

    // Not valid. r1 is readonly.
    Mutate(ref r1);

    // OK.
    Print(in r1);

    // OK.
    return ref r1;
}
```

### <a name="restrictions-on-use-of-ref-readonly-locals"></a>使用 `ref readonly` 區域變數的限制
除了其 `readonly` 本質以外，`ref readonly` 區域變數的行為就像一般 `ref` 區域變數，而且受到完全相同的限制。  
如需在「閉」中捕捉的相關限制，請在 `async` 方法中宣告，或 `safe-to-return` 分析同樣適用于 `ref readonly` 區域變數。

## <a name="ternary-ref-expressions-aka-conditional-lvalues"></a>三元 `ref` 運算式。 （也稱為「條件式左值」）

### <a name="motivation"></a>動機
使用 `ref` 和 `ref readonly` 區域變數公開的需求，必須根據條件，使用一個或另一個目標變數來對這類區域變數進行 ref 初始化。

一般的因應措施是引進如下的方法：

```csharp
ref T Choice(bool condition, ref T consequence, ref T alternative)
{
    if (condition)
    {
         return ref consequence;
    }
    else
    {
         return ref alternative;
    }
}
```

請注意，`Choice` 不是三元的確切取代，因為_所有_引數都必須在呼叫位置評估，這會導致圈子行為和 bug。

下列作業將無法如預期般運作：

```csharp
    // will crash with NRE because 'arr[0]' will be executed unconditionally
    ref var r = ref Choice(arr != null, ref arr[0], ref otherArr[0]);
```

### <a name="solution"></a>解決方法
允許特殊類型的條件運算式，根據條件評估為其中一個左值引數的參考。

### <a name="using-ref-ternary-expression"></a>使用 `ref` 的三元運算式。

條件運算式 `ref` 類別的語法為 `<condition> ? ref <consequence> : ref <alternative>;`

就像使用一般條件運算式一樣 `<consequence>` 或 `<alternative>` 會根據布林值條件運算式的結果進行評估。

不同于一般條件運算式，`ref` 條件運算式：
- 需要 `<consequence>` 和 `<alternative>` 左值。
- `ref` 條件運算式本身為左值，而
- 如果 `<consequence>` 和 `<alternative>` 都是可寫入的左值，則可寫入 `ref` 條件運算式

範例：  
`ref` 三元是左值，因此可以透過傳址方式傳遞/指派/傳回;
```csharp
     // pass by reference
     foo(ref (arr != null ? ref arr[0]: ref otherArr[0]));

     // return by reference
     return ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

做為左值，也可以指派給。
```csharp
     // assign to
     (arr != null ? ref arr[0]: ref otherArr[0]) = 1;

     // error. readOnlyField is readonly and thus conditional expression is readonly
     (arr != null ? ref arr[0]: ref obj.readOnlyField) = 1;
```

可用來做為方法呼叫的接收者，並在必要時略過複製。
```csharp
     // no copies
     (arr != null ? ref arr[0]: ref otherArr[0]).StructMethod();

     // invoked on a copy.
     // The receiver is `readonly` because readOnlyField is readonly.
     (arr != null ? ref arr[0]: ref obj.readOnlyField).StructMethod();

     // no copies. `ReadonlyStructMethod` is a method on a `readonly` struct
     // and can be invoked directly on a readonly receiver
     (arr != null ? ref arr[0]: ref obj.readOnlyField).ReadonlyStructMethod();
```

`ref` 三元也可以用在一般（非 ref）內容中。
```csharp
     // only an example
     // a regular ternary could work here just the same
     int x = (arr != null ? ref arr[0]: ref otherArr[0]);
```

### <a name="drawbacks"></a>缺點
[drawbacks]: #drawbacks

我可以看到兩個主要引數，以增強對參考和唯讀參考的支援：

1) 這裡解決的問題非常舊。 為什麼突然解決這些問題，特別是因為它不會協助現有的程式碼？

如我們在C#新網域中發現和使用 .net，有些問題會變得更明顯。  
作為環境的範例，比計算額外負荷的平均值更重要，我可以列出

* 針對計算費用和回應能力的雲端/資料中心案例，是競爭優勢。
* 具有延遲之軟性需求的遊戲/VR/AR     

這項功能不會犧牲任何現有的優勢（例如型別安全），同時允許在某些常見案例中降低額外負荷。

2) 我們可以合理保證被呼叫者在加入宣告 `readonly` 合約時，會由規則扮演嗎？

使用 `out`時，我們會有類似的信任。 不正確的 `out` 執行可能會導致未指定的行為，但事實上，這很少發生。  

建立熟悉的正式驗證規則 `ref readonly` 會進一步緩和信任問題。

### <a name="alternatives"></a>替代方案
[alternatives]: #alternatives

主要的競爭設計其實就是「不執行任何動作」。

### <a name="unresolved-questions"></a>未解決的問題
[unresolved]: #unresolved-questions

### <a name="design-meetings"></a>設計會議

https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-02-22.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-01.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-08-28.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-25.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-27.md
