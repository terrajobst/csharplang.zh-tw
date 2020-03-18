---
ms.openlocfilehash: 39fb0aab5e8bb0d422f25fd2e92ab3d8256d3f59
ms.sourcegitcommit: b8f1103eb686c5d82e294837c9386d9b667da292
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2019
ms.locfileid: "79485109"
---
# <a name="readonly-references"></a><span data-ttu-id="3a43e-101">唯讀參考</span><span class="sxs-lookup"><span data-stu-id="3a43e-101">Readonly references</span></span>

* <span data-ttu-id="3a43e-102">[x] 提議</span><span class="sxs-lookup"><span data-stu-id="3a43e-102">[x] Proposed</span></span>
* <span data-ttu-id="3a43e-103">[x] 原型</span><span class="sxs-lookup"><span data-stu-id="3a43e-103">[x] Prototype</span></span>
* <span data-ttu-id="3a43e-104">[x] 執行：已啟動</span><span class="sxs-lookup"><span data-stu-id="3a43e-104">[x] Implementation: Started</span></span>
* <span data-ttu-id="3a43e-105">[] 規格：未啟動</span><span class="sxs-lookup"><span data-stu-id="3a43e-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="3a43e-106">摘要</span><span class="sxs-lookup"><span data-stu-id="3a43e-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="3a43e-107">「Readonly 參考」功能實際上是一組功能，可利用以傳址方式傳遞變數的效率，但不會公開要修改的資料：</span><span class="sxs-lookup"><span data-stu-id="3a43e-107">The "readonly references" feature is actually a group of features that leverage the efficiency of passing variables by reference, but without exposing the data to modifications:</span></span>  
- <span data-ttu-id="3a43e-108">`in` 參數</span><span class="sxs-lookup"><span data-stu-id="3a43e-108">`in` parameters</span></span>
- <span data-ttu-id="3a43e-109">`ref readonly` 傳回</span><span class="sxs-lookup"><span data-stu-id="3a43e-109">`ref readonly` returns</span></span>
- <span data-ttu-id="3a43e-110">`readonly` 結構</span><span class="sxs-lookup"><span data-stu-id="3a43e-110">`readonly` structs</span></span>
- <span data-ttu-id="3a43e-111">`ref`/`in` 擴充方法</span><span class="sxs-lookup"><span data-stu-id="3a43e-111">`ref`/`in` extension methods</span></span>
- <span data-ttu-id="3a43e-112">`ref readonly` 區域變數</span><span class="sxs-lookup"><span data-stu-id="3a43e-112">`ref readonly` locals</span></span>
- <span data-ttu-id="3a43e-113">`ref` 條件運算式</span><span class="sxs-lookup"><span data-stu-id="3a43e-113">`ref` conditional expressions</span></span>

## <a name="passing-arguments-as-readonly-references"></a><span data-ttu-id="3a43e-114">將引數傳遞為 readonly 參考。</span><span class="sxs-lookup"><span data-stu-id="3a43e-114">Passing arguments as readonly references.</span></span>

<span data-ttu-id="3a43e-115">現有的提案會將本主題 https://github.com/dotnet/roslyn/issues/115 視為唯讀參數的特殊案例，而不會有許多詳細資料。</span><span class="sxs-lookup"><span data-stu-id="3a43e-115">There is an existing proposal that touches this topic https://github.com/dotnet/roslyn/issues/115 as a special case of readonly parameters without going into many details.</span></span>
<span data-ttu-id="3a43e-116">我只想要知道自己的想法不是非常新的。</span><span class="sxs-lookup"><span data-stu-id="3a43e-116">Here I just want to acknowledge that the idea by itself is not very new.</span></span>

### <a name="motivation"></a><span data-ttu-id="3a43e-117">動機</span><span class="sxs-lookup"><span data-stu-id="3a43e-117">Motivation</span></span>

<span data-ttu-id="3a43e-118">在此功能C#之前，不會有有效率的方式來表示想要將結構變數傳遞至方法呼叫以供 readonly 使用，而不想要修改。</span><span class="sxs-lookup"><span data-stu-id="3a43e-118">Prior to this feature C# did not have an efficient way of expressing a desire to pass struct variables into method calls for readonly purposes with no intention of modifying.</span></span> <span data-ttu-id="3a43e-119">標準的傳值引數會隱含複製，這會增加不必要的成本。</span><span class="sxs-lookup"><span data-stu-id="3a43e-119">Regular by-value argument passing implies copying, which adds unnecessary costs.</span></span>  <span data-ttu-id="3a43e-120">這會讓使用者使用 ref 引數傳遞並依賴批註/檔，以指出資料不應該由被呼叫端進行變化。</span><span class="sxs-lookup"><span data-stu-id="3a43e-120">That drives users to use by-ref argument passing and rely on comments/documentation to indicate that the data is not supposed to be mutated by the callee.</span></span> <span data-ttu-id="3a43e-121">這不是很好的解決方案，因為有許多原因。</span><span class="sxs-lookup"><span data-stu-id="3a43e-121">It is not a good solution for many reasons.</span></span>  
<span data-ttu-id="3a43e-122">這些範例是圖形程式庫中的許多向量/矩陣數學運算子，例如[，同型元件已知會因為](https://msdn.microsoft.com/library/bb194944.aspx)效能考慮而純粹具有 ref 運算元。</span><span class="sxs-lookup"><span data-stu-id="3a43e-122">The examples are numerous - vector/matrix math operators in graphics libraries like [XNA](https://msdn.microsoft.com/library/bb194944.aspx) are known to have ref operands purely because of performance considerations.</span></span> <span data-ttu-id="3a43e-123">Roslyn 編譯器本身有程式碼，它會使用結構來避免配置，然後以傳址方式傳遞它們以避免複製成本。</span><span class="sxs-lookup"><span data-stu-id="3a43e-123">There is code in Roslyn compiler itself that uses structs to avoid allocations and then passes them by reference to avoid copying costs.</span></span>

### <a name="solution-in-parameters"></a><span data-ttu-id="3a43e-124">方案（`in` 參數）</span><span class="sxs-lookup"><span data-stu-id="3a43e-124">Solution (`in` parameters)</span></span>

<span data-ttu-id="3a43e-125">類似于 `out` 參數，`in` 參數會以 managed 參考的形式傳遞，並具有被呼叫端的其他保證。</span><span class="sxs-lookup"><span data-stu-id="3a43e-125">Similarly to the `out` parameters, `in` parameters are passed as managed references with additional guarantees from the callee.</span></span>  
<span data-ttu-id="3a43e-126">不同于在任何其他用途之前_必須_由被呼叫端指派的 `out` 參數，`in` 的參數完全無法由被呼叫端指派。</span><span class="sxs-lookup"><span data-stu-id="3a43e-126">Unlike `out` parameters which _must_ be assigned by the callee before any other use, `in` parameters cannot be assigned by the callee at all.</span></span>

<span data-ttu-id="3a43e-127">因此 `in` 參數可有效傳遞間接引數，而不會將引數公開給被呼叫端變化。</span><span class="sxs-lookup"><span data-stu-id="3a43e-127">As a result `in` parameters allow for effectiveness of indirect argument passing without exposing arguments to mutations by the callee.</span></span>

### <a name="declaring-in-parameters"></a><span data-ttu-id="3a43e-128">宣告 `in` 參數</span><span class="sxs-lookup"><span data-stu-id="3a43e-128">Declaring `in` parameters</span></span>

<span data-ttu-id="3a43e-129">`in` 參數是使用 `in` 關鍵字來宣告，做為參數簽章中的修飾詞。</span><span class="sxs-lookup"><span data-stu-id="3a43e-129">`in` parameters are declared by using `in` keyword as a modifier in the parameter signature.</span></span>

<span data-ttu-id="3a43e-130">就所有用途而言，會將 `in` 參數視為 `readonly` 變數。</span><span class="sxs-lookup"><span data-stu-id="3a43e-130">For all purposes the `in` parameter is treated as a `readonly` variable.</span></span> <span data-ttu-id="3a43e-131">在方法內使用 `in` 參數的大部分限制，與 `readonly` 欄位相同。</span><span class="sxs-lookup"><span data-stu-id="3a43e-131">Most of the restrictions on the use of `in` parameters inside the method are the same as with `readonly` fields.</span></span>

> <span data-ttu-id="3a43e-132">事實上，`in` 參數可能代表 `readonly` 欄位。</span><span class="sxs-lookup"><span data-stu-id="3a43e-132">Indeed an `in` parameter may represent a `readonly` field.</span></span> <span data-ttu-id="3a43e-133">限制的相似性並不是巧合。</span><span class="sxs-lookup"><span data-stu-id="3a43e-133">Similarity of restrictions is not a coincidence.</span></span>

<span data-ttu-id="3a43e-134">例如，具有結構類型之 `in` 參數的欄位，都會以遞迴方式分類為 `readonly` 變數。</span><span class="sxs-lookup"><span data-stu-id="3a43e-134">For example fields of an `in` parameter which has a struct type are all recursively classified as `readonly` variables .</span></span>

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

- <span data-ttu-id="3a43e-135">允許使用一般 byval 參數的任何地方都允許 `in` 參數。</span><span class="sxs-lookup"><span data-stu-id="3a43e-135">`in` parameters are allowed anywhere where ordinary byval parameters are allowed.</span></span> <span data-ttu-id="3a43e-136">這包括索引子、運算子（包括轉換）、委派、lambda、區域函數。</span><span class="sxs-lookup"><span data-stu-id="3a43e-136">This includes indexers, operators (including conversions), delegates, lambdas, local functions.</span></span>

> ```csharp
>  (in int x) => x                                                     // lambda expression  
>  TValue this[in TKey index];                                         // indexer
>  public static Vector3 operator +(in Vector3 x, in Vector3 y) => ... // operator
>  ```

- <span data-ttu-id="3a43e-137">`in` 不允許結合 `out` 或 `out` 未與結合的任何專案。</span><span class="sxs-lookup"><span data-stu-id="3a43e-137">`in` is not allowed in combination with `out` or with anything that `out` does not combine with.</span></span>

- <span data-ttu-id="3a43e-138">`ref`/`out`/`in` 差異，則不允許多載。</span><span class="sxs-lookup"><span data-stu-id="3a43e-138">It is not permitted to overload on `ref`/`out`/`in` differences.</span></span>

- <span data-ttu-id="3a43e-139">允許在一般的 byval 上多載，並 `in` 差異。</span><span class="sxs-lookup"><span data-stu-id="3a43e-139">It is permitted to overload on ordinary byval and `in` differences.</span></span>

- <span data-ttu-id="3a43e-140">基於 OHI 的目的（多載、隱藏、執行），`in` 的行為類似 `out` 參數。</span><span class="sxs-lookup"><span data-stu-id="3a43e-140">For the purpose of OHI (Overloading, Hiding, Implementing), `in` behaves similarly to an `out` parameter.</span></span>
<span data-ttu-id="3a43e-141">所有相同的規則都適用。</span><span class="sxs-lookup"><span data-stu-id="3a43e-141">All the same rules apply.</span></span>
<span data-ttu-id="3a43e-142">例如，覆寫方法必須符合可轉換類型之 `in` 參數的 `in` 參數。</span><span class="sxs-lookup"><span data-stu-id="3a43e-142">For example the overriding method will have to match `in` parameters with `in` parameters of an identity-convertible type.</span></span>

- <span data-ttu-id="3a43e-143">為了委派/lambda/方法群組轉換的目的，`in` 的行為類似于 `out` 參數。</span><span class="sxs-lookup"><span data-stu-id="3a43e-143">For the purpose of delegate/lambda/method group conversions, `in` behaves similarly to an `out` parameter.</span></span>
<span data-ttu-id="3a43e-144">Lambda 和適用的方法群組轉換候選項目必須符合目標委派 `in` 參數，以及可轉換身分識別類型的 `in` 參數。</span><span class="sxs-lookup"><span data-stu-id="3a43e-144">Lambdas and applicable method group conversion candidates will have to match `in` parameters of the target delegate with `in` parameters of an identity-convertible type.</span></span>

- <span data-ttu-id="3a43e-145">基於泛型變異數的目的，`in` 參數 nonvariant。</span><span class="sxs-lookup"><span data-stu-id="3a43e-145">For the purpose of generic variance, `in` parameters are nonvariant.</span></span>

> <span data-ttu-id="3a43e-146">注意：對於具有參考或基本類型的 `in` 參數，不會出現任何警告。</span><span class="sxs-lookup"><span data-stu-id="3a43e-146">NOTE: There are no warnings on `in` parameters that have reference or primitives types.</span></span>
<span data-ttu-id="3a43e-147">這在一般情況下可能毫無意義，但在某些情況下，使用者必須/想以 `in`傳遞基本類型。</span><span class="sxs-lookup"><span data-stu-id="3a43e-147">It may be pointless in general, but in some cases user must/want to pass primitives as `in`.</span></span> <span data-ttu-id="3a43e-148">範例-覆寫泛型方法（例如 `Method(in T param)`，當 `T` 取代為 `int`時，或具有如的方法時 `Volatile.Read(in int location)`</span><span class="sxs-lookup"><span data-stu-id="3a43e-148">Examples - overriding a generic method like `Method(in T param)` when `T` was substituted to be `int`, or when having methods like `Volatile.Read(in int location)`</span></span>
>
> <span data-ttu-id="3a43e-149">有一個分析器會在使用 `in` 參數的效率不佳的情況下發出警告，但這類分析的規則會太模糊而無法成為語言規格的一部分。</span><span class="sxs-lookup"><span data-stu-id="3a43e-149">It is conceivable to have an analyzer that warns in cases of inefficient use of `in` parameters, but the rules for such analysis would be too fuzzy to be a part of a language specification.</span></span>

### <a name="use-of-in-at-call-sites-in-arguments"></a><span data-ttu-id="3a43e-150">在呼叫網站使用 `in`。</span><span class="sxs-lookup"><span data-stu-id="3a43e-150">Use of `in` at call sites.</span></span> <span data-ttu-id="3a43e-151">（`in` 引數）</span><span class="sxs-lookup"><span data-stu-id="3a43e-151">(`in` arguments)</span></span>

<span data-ttu-id="3a43e-152">有兩種方式可以將引數傳遞給 `in` 參數。</span><span class="sxs-lookup"><span data-stu-id="3a43e-152">There are two ways to pass arguments to `in` parameters.</span></span>

#### <a name="in-arguments-can-match-in-parameters"></a><span data-ttu-id="3a43e-153">`in` 引數可以符合 `in` 參數：</span><span class="sxs-lookup"><span data-stu-id="3a43e-153">`in` arguments can match `in` parameters:</span></span>

<span data-ttu-id="3a43e-154">在呼叫位置具有 `in` 修飾詞的引數，可以符合 `in` 參數。</span><span class="sxs-lookup"><span data-stu-id="3a43e-154">An argument with an `in` modifier at the call site can match `in` parameters.</span></span>

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

- <span data-ttu-id="3a43e-155">`in` 引數必須是_可讀取_的左值（\*）。</span><span class="sxs-lookup"><span data-stu-id="3a43e-155">`in` argument must be a _readable_ LValue(\*).</span></span>
<span data-ttu-id="3a43e-156">範例： `M1(in 42)` 無效</span><span class="sxs-lookup"><span data-stu-id="3a43e-156">Example: `M1(in 42)` is invalid</span></span>

> <span data-ttu-id="3a43e-157">(\*)[左值/右](https://en.wikipedia.org/wiki/Value_(computer_science)#lrvalue)值的概念會因語言而異。</span><span class="sxs-lookup"><span data-stu-id="3a43e-157">(\*) The notion of [LValue/RValue](https://en.wikipedia.org/wiki/Value_(computer_science)#lrvalue) vary between languages.</span></span>  
<span data-ttu-id="3a43e-158">在這裡，按左值 I 表示一個運算式，代表可以直接參考的位置。</span><span class="sxs-lookup"><span data-stu-id="3a43e-158">Here, by LValue I mean an expression that represent a location that can be referred to directly.</span></span>
<span data-ttu-id="3a43e-159">和右值表示一個運算式，其會產生不會自行保存的暫存結果。</span><span class="sxs-lookup"><span data-stu-id="3a43e-159">And RValue means an expression that yields a temporary result which does not persist on its own.</span></span>  

- <span data-ttu-id="3a43e-160">特別是，傳遞 `readonly` 欄位、`in` 參數或其他正式 `readonly` 變數做為 `in` 引數，是有效的。</span><span class="sxs-lookup"><span data-stu-id="3a43e-160">In particular it is valid to pass `readonly` fields, `in` parameters or other formally `readonly` variables as `in` arguments.</span></span>
<span data-ttu-id="3a43e-161">範例： `dictionary[in Guid.Empty]` 是合法的。</span><span class="sxs-lookup"><span data-stu-id="3a43e-161">Example: `dictionary[in Guid.Empty]` is legal.</span></span> <span data-ttu-id="3a43e-162">`Guid.Empty` 是靜態的唯讀欄位。</span><span class="sxs-lookup"><span data-stu-id="3a43e-162">`Guid.Empty` is a static readonly field.</span></span>

- <span data-ttu-id="3a43e-163">`in` 引數必須具有類型身分識別，才能_轉換_為參數的類型。</span><span class="sxs-lookup"><span data-stu-id="3a43e-163">`in` argument must have type _identity-convertible_ to the type of the parameter.</span></span>
<span data-ttu-id="3a43e-164">範例： `M1<object>(in Guid.Empty)` 無效。</span><span class="sxs-lookup"><span data-stu-id="3a43e-164">Example: `M1<object>(in Guid.Empty)` is invalid.</span></span> <span data-ttu-id="3a43e-165">`Guid.Empty` 無法識別為可_轉換_成 `object`</span><span class="sxs-lookup"><span data-stu-id="3a43e-165">`Guid.Empty` is not _identity-convertible_ to `object`</span></span>

<span data-ttu-id="3a43e-166">上述規則的動機在於，`in` 引數可保證引數變數的_別名_。</span><span class="sxs-lookup"><span data-stu-id="3a43e-166">The motivation for the above rules is that `in` arguments guarantee _aliasing_ of the argument variable.</span></span> <span data-ttu-id="3a43e-167">被呼叫端一律會收到引數所表示之相同位置的直接參考。</span><span class="sxs-lookup"><span data-stu-id="3a43e-167">The callee always receives a direct reference to the same location as represented by the argument.</span></span>

- <span data-ttu-id="3a43e-168">在罕見的情況下，因為 `await` 運算式做為相同呼叫的運算元，所以 `in` 引數必須堆疊溢位，其行為與 `out` 和 `ref` 引數相同-如果變數無法以參考上透明的方式溢出，則會報告錯誤。</span><span class="sxs-lookup"><span data-stu-id="3a43e-168">in rare situations when `in` arguments must be stack-spilled due to `await` expressions used as operands of the same call, the behavior is the same as with `out` and `ref` arguments - if the variable cannot be spilled in referentially-transparent manner, an error is reported.</span></span>

<span data-ttu-id="3a43e-169">範例：</span><span class="sxs-lookup"><span data-stu-id="3a43e-169">Examples:</span></span>
1. <span data-ttu-id="3a43e-170">`M1(in staticField, await SomethingAsync())` 有效。</span><span class="sxs-lookup"><span data-stu-id="3a43e-170">`M1(in staticField, await SomethingAsync())`  is valid.</span></span>
<span data-ttu-id="3a43e-171">`staticField` 是一個可以多次存取的靜態欄位，而不會有可觀察的副作用。</span><span class="sxs-lookup"><span data-stu-id="3a43e-171">`staticField` is a static field which can be accessed more than once without observable side effects.</span></span> <span data-ttu-id="3a43e-172">因此，也可以提供副作用和別名需求的順序。</span><span class="sxs-lookup"><span data-stu-id="3a43e-172">Therefore both the order of side effects and aliasing requirements can be provided.</span></span>
2. <span data-ttu-id="3a43e-173">`M1(in RefReturningMethod(), await SomethingAsync())` 將會產生錯誤。</span><span class="sxs-lookup"><span data-stu-id="3a43e-173">`M1(in RefReturningMethod(), await SomethingAsync())`  will produce an error.</span></span>
<span data-ttu-id="3a43e-174">`RefReturningMethod()` 是傳回方法的 `ref`。</span><span class="sxs-lookup"><span data-stu-id="3a43e-174">`RefReturningMethod()` is a `ref` returning method.</span></span> <span data-ttu-id="3a43e-175">方法呼叫可能會有明顯的副作用，因此必須在 `SomethingAsync()` 運算元之前評估。</span><span class="sxs-lookup"><span data-stu-id="3a43e-175">A method call may have observable side effects, therefore it must be evaluated before the `SomethingAsync()` operand.</span></span> <span data-ttu-id="3a43e-176">不過，叫用的結果是無法跨 `await` 暫停點保留的參考，因此不可能進行直接參考需求。</span><span class="sxs-lookup"><span data-stu-id="3a43e-176">However the result of the invocation is a reference that cannot be preserved across the `await` suspension point which make the direct reference requirement impossible.</span></span>

> <span data-ttu-id="3a43e-177">注意：堆疊溢位錯誤會被視為是實作為特定的限制。</span><span class="sxs-lookup"><span data-stu-id="3a43e-177">NOTE: the stack spilling errors are considered to be implementation-specific limitations.</span></span> <span data-ttu-id="3a43e-178">因此，它們不會影響多載解析或 lambda 推斷。</span><span class="sxs-lookup"><span data-stu-id="3a43e-178">Therefore they do not have effect on overload resolution or lambda inference.</span></span>

#### <a name="ordinary-byval-arguments-can-match-in-parameters"></a><span data-ttu-id="3a43e-179">一般的 byval 引數可以符合 `in` 參數：</span><span class="sxs-lookup"><span data-stu-id="3a43e-179">Ordinary byval arguments can match `in` parameters:</span></span>

<span data-ttu-id="3a43e-180">不含修飾詞的一般引數可以符合 `in` 參數。</span><span class="sxs-lookup"><span data-stu-id="3a43e-180">Regular arguments without modifiers can match `in` parameters.</span></span> <span data-ttu-id="3a43e-181">在這種情況下，引數具有與一般 byval 引數相同的寬鬆條件約束。</span><span class="sxs-lookup"><span data-stu-id="3a43e-181">In such case the arguments have the same relaxed constraints as an ordinary byval arguments would have.</span></span>

<span data-ttu-id="3a43e-182">此案例的動機在於，當引數不能當做直接參考（例如：常值、計算或 `await`ed 結果，或有更特定類型的引數）傳遞時，Api 中 `in` 的參數可能會導致使用者不便。</span><span class="sxs-lookup"><span data-stu-id="3a43e-182">The motivation for this scenario is that `in` parameters in APIs may result in inconveniences for the user when arguments cannot be passed as a direct reference - ex: literals, computed or `await`-ed results or arguments that happen to have more specific types.</span></span>  
<span data-ttu-id="3a43e-183">所有這些案例都有一個簡單的解決方案，就是將引數值儲存在適當類型的暫存區域中，並將該本機當做 `in` 引數來傳遞。</span><span class="sxs-lookup"><span data-stu-id="3a43e-183">All these cases have a trivial solution of storing the argument value in a temporary local of appropriate type and passing that local as an `in` argument.</span></span>  
<span data-ttu-id="3a43e-184">若要減少這類樣板化程式碼的需求，編譯器可以視需要執行相同的轉換，而不會在呼叫位置出現 `in` 修飾詞。</span><span class="sxs-lookup"><span data-stu-id="3a43e-184">To reduce the need for such boilerplate code compiler can perform the same transformation, if needed, when `in` modifier is not present at the call site.</span></span>  

<span data-ttu-id="3a43e-185">此外，在某些情況下（例如運算子的調用）或 `in` 擴充方法，沒有任何語法方式可以指定 `in`。</span><span class="sxs-lookup"><span data-stu-id="3a43e-185">In addition, in some cases, such as invocation of operators, or `in` extension methods, there is no syntactical way to specify `in` at all.</span></span> <span data-ttu-id="3a43e-186">這只需要在符合 `in` 參數時，指定一般 byval 引數的行為。</span><span class="sxs-lookup"><span data-stu-id="3a43e-186">That alone requires specifying the behavior of ordinary byval arguments when they match `in` parameters.</span></span>

<span data-ttu-id="3a43e-187">尤其是：</span><span class="sxs-lookup"><span data-stu-id="3a43e-187">In particular:</span></span>

- <span data-ttu-id="3a43e-188">傳遞右值是有效的。</span><span class="sxs-lookup"><span data-stu-id="3a43e-188">it is valid to pass RValues.</span></span>
<span data-ttu-id="3a43e-189">在這種情況下，會傳遞暫存的參考。</span><span class="sxs-lookup"><span data-stu-id="3a43e-189">A reference to a temporary is passed in such case.</span></span>
<span data-ttu-id="3a43e-190">範例：</span><span class="sxs-lookup"><span data-stu-id="3a43e-190">Example:</span></span>
```csharp
Print("hello");      // not an error.

void Print<T>(in T x)
{
  //. . .
}
```

- <span data-ttu-id="3a43e-191">允許隱含轉換。</span><span class="sxs-lookup"><span data-stu-id="3a43e-191">implicit conversions are allowed.</span></span>

> <span data-ttu-id="3a43e-192">這實際上是傳遞右值的特殊案例</span><span class="sxs-lookup"><span data-stu-id="3a43e-192">This is actually a special case of passing an RValue</span></span>  

<span data-ttu-id="3a43e-193">在這種情況下，會傳遞暫時保留轉換值的參考。</span><span class="sxs-lookup"><span data-stu-id="3a43e-193">A reference to a temporary holding converted value is passed in such case.</span></span>
<span data-ttu-id="3a43e-194">範例：</span><span class="sxs-lookup"><span data-stu-id="3a43e-194">Example:</span></span>
```csharp
Print<int>(Short.MaxValue)     // not an error.
```

- <span data-ttu-id="3a43e-195">在 `in` 擴充方法的接收者案例中（相對於 `ref` 擴充方法），則允許右值或隱含的_這個引數轉換_。</span><span class="sxs-lookup"><span data-stu-id="3a43e-195">in a case of a receiver of an `in` extension method (as opposed to `ref` extension methods), RValues or implicit _this-argument-conversions_ are allowed.</span></span>
<span data-ttu-id="3a43e-196">在這種情況下，會傳遞暫時保留轉換值的參考。</span><span class="sxs-lookup"><span data-stu-id="3a43e-196">A reference to a temporary holding converted value is passed in such case.</span></span>
<span data-ttu-id="3a43e-197">範例：</span><span class="sxs-lookup"><span data-stu-id="3a43e-197">Example:</span></span>
```csharp
public static IEnumerable<T> Concat<T>(in this (IEnumerable<T>, IEnumerable<T>) arg)  => . . .;

("aa", "bb").Concat<char>()    // not an error.
```
<span data-ttu-id="3a43e-198">本檔會進一步提供有關 `ref`/`in` 擴充方法的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="3a43e-198">More information on `ref`/`in` extension methods is provided further in this document.</span></span>

- <span data-ttu-id="3a43e-199">因 `await` 運算元而造成的引數溢出可能會在必要時，以傳值方式溢出。</span><span class="sxs-lookup"><span data-stu-id="3a43e-199">argument spilling due to `await` operands could spill "by-value", if necessary.</span></span>
<span data-ttu-id="3a43e-200">在提供引數直接參考的情況下，由於中間 `await`，會改為溢出引數值的複本。</span><span class="sxs-lookup"><span data-stu-id="3a43e-200">In scenarios where providing a direct reference to the argument is not possible due to intervening `await` a copy of the argument's value is spilled instead.</span></span>  
<span data-ttu-id="3a43e-201">範例：</span><span class="sxs-lookup"><span data-stu-id="3a43e-201">Example:</span></span>
```csharp
M1(RefReturningMethod(), await SomethingAsync())   // not an error.
```
<span data-ttu-id="3a43e-202">因為副作用調用的結果是無法在 `await` 暫停之間保留的參考，所以會改為保留包含實際值的暫存（如同在一般的 byval 參數案例中一樣）。</span><span class="sxs-lookup"><span data-stu-id="3a43e-202">Since the result of a side-effecting invocation is a reference that cannot be preserved across `await` suspension, a temporary containing the actual value will be preserved instead (as it would in an ordinary byval parameter case).</span></span>

#### <a name="omitted-optional-arguments"></a><span data-ttu-id="3a43e-203">省略的選擇性引數</span><span class="sxs-lookup"><span data-stu-id="3a43e-203">Omitted optional arguments</span></span>

<span data-ttu-id="3a43e-204">允許 `in` 參數指定預設值。</span><span class="sxs-lookup"><span data-stu-id="3a43e-204">It is permitted for an `in` parameter to specify a default value.</span></span> <span data-ttu-id="3a43e-205">讓對應引數成為選擇性的。</span><span class="sxs-lookup"><span data-stu-id="3a43e-205">That makes the corresponding argument optional.</span></span>

<span data-ttu-id="3a43e-206">省略呼叫位置的選擇性引數會導致透過暫時性傳遞預設值。</span><span class="sxs-lookup"><span data-stu-id="3a43e-206">Omitting optional argument at the call site results in passing the default value via a temporary.</span></span>

```csharp
Print("hello");      // not an error, same as
Print("hello", c: Color.Black);

void Print(string s, in Color c = Color.Black)
{
    // . . .
}
```

### <a name="aliasing-behavior-in-general"></a><span data-ttu-id="3a43e-207">一般的別名行為</span><span class="sxs-lookup"><span data-stu-id="3a43e-207">Aliasing behavior in general</span></span>

<span data-ttu-id="3a43e-208">就像 `ref` 和 `out` 變數一樣，`in` 變數是現有位置的參考/別名。</span><span class="sxs-lookup"><span data-stu-id="3a43e-208">Just like `ref` and `out` variables, `in` variables are references/aliases to existing locations.</span></span>

<span data-ttu-id="3a43e-209">雖然無法將被呼叫端寫入其中，但讀取 `in` 參數可能會觀察不同的值，做為其他評估的副作用。</span><span class="sxs-lookup"><span data-stu-id="3a43e-209">While callee is not allowed to write into them, reading an `in` parameter can observe different values as a side effect of other evaluations.</span></span>

<span data-ttu-id="3a43e-210">範例：</span><span class="sxs-lookup"><span data-stu-id="3a43e-210">Example:</span></span>

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

### <a name="in-parameters-and-capturing-of-local-variables"></a><span data-ttu-id="3a43e-211">`in` 參數和區域變數的捕捉。</span><span class="sxs-lookup"><span data-stu-id="3a43e-211">`in` parameters and capturing of local variables.</span></span>  
<span data-ttu-id="3a43e-212">基於 lambda/非同步捕捉的目的，`in` 參數的行為與 `out` 和 `ref` 參數相同。</span><span class="sxs-lookup"><span data-stu-id="3a43e-212">For the purpose of lambda/async capturing `in` parameters behave the same as `out` and `ref` parameters.</span></span>

- <span data-ttu-id="3a43e-213">無法在關閉中捕捉 `in` 參數</span><span class="sxs-lookup"><span data-stu-id="3a43e-213">`in` parameters cannot be captured in a closure</span></span>
- <span data-ttu-id="3a43e-214">反覆運算器方法中不允許 `in` 參數</span><span class="sxs-lookup"><span data-stu-id="3a43e-214">`in` parameters are not allowed in iterator methods</span></span>
- <span data-ttu-id="3a43e-215">非同步方法中不允許 `in` 參數</span><span class="sxs-lookup"><span data-stu-id="3a43e-215">`in` parameters are not allowed in async methods</span></span>

### <a name="temporary-variables"></a><span data-ttu-id="3a43e-216">暫存變數。</span><span class="sxs-lookup"><span data-stu-id="3a43e-216">Temporary variables.</span></span>  
<span data-ttu-id="3a43e-217">某些使用 `in` 參數傳遞可能需要間接使用暫存區域變數：</span><span class="sxs-lookup"><span data-stu-id="3a43e-217">Some uses of `in` parameter passing may require indirect use of a temporary local variable:</span></span>  
- <span data-ttu-id="3a43e-218">當呼叫站使用 `in`時，一律會將 `in` 引數當做直接別名傳遞。</span><span class="sxs-lookup"><span data-stu-id="3a43e-218">`in` arguments are always passed as direct aliases when call-site uses `in`.</span></span> <span data-ttu-id="3a43e-219">這種情況下永遠不會使用暫時的。</span><span class="sxs-lookup"><span data-stu-id="3a43e-219">Temporary is never used in such case.</span></span>
- <span data-ttu-id="3a43e-220">當呼叫站未使用 `in`時，`in` 引數不需要是直接別名。</span><span class="sxs-lookup"><span data-stu-id="3a43e-220">`in` arguments are not required to be direct aliases when call-site does not use `in`.</span></span> <span data-ttu-id="3a43e-221">當引數不是左值時，可能會使用暫存。</span><span class="sxs-lookup"><span data-stu-id="3a43e-221">When argument is not an LValue, a temporary may be used.</span></span>
- <span data-ttu-id="3a43e-222">`in` 參數可能有預設值。</span><span class="sxs-lookup"><span data-stu-id="3a43e-222">`in` parameter may have default value.</span></span> <span data-ttu-id="3a43e-223">在呼叫位置省略對應的引數時，預設值會透過暫時性傳遞。</span><span class="sxs-lookup"><span data-stu-id="3a43e-223">When corresponding argument is omitted at the call site, the default value are passed via a temporary.</span></span>
- <span data-ttu-id="3a43e-224">`in` 引數可能會有隱含的轉換，包括不會保留身分識別的。</span><span class="sxs-lookup"><span data-stu-id="3a43e-224">`in` arguments may have implicit conversions, including those that do not preserve identity.</span></span> <span data-ttu-id="3a43e-225">在這些情況下，會使用暫時的。</span><span class="sxs-lookup"><span data-stu-id="3a43e-225">A temporary is used in those cases.</span></span>
- <span data-ttu-id="3a43e-226">一般結構呼叫的接收者可能無法寫入左值（**現有的案例！** ）。</span><span class="sxs-lookup"><span data-stu-id="3a43e-226">receivers of ordinary struct calls may not be writeable LValues (**existing case!**).</span></span> <span data-ttu-id="3a43e-227">在這些情況下，會使用暫時的。</span><span class="sxs-lookup"><span data-stu-id="3a43e-227">A temporary is used in those cases.</span></span>

<span data-ttu-id="3a43e-228">引數而暫存物件的存留時間符合呼叫網站最接近的範圍。</span><span class="sxs-lookup"><span data-stu-id="3a43e-228">The life time of the argument temporaries matches the closest encompassing scope of the call-site.</span></span>

<span data-ttu-id="3a43e-229">暫存變數的正式運作時間在包含以傳址方式傳回之變數的 escape 分析的案例中，在語義上很重要。</span><span class="sxs-lookup"><span data-stu-id="3a43e-229">The formal life time of temporary variables is semantically significant in scenarios involving escape analysis of variables returned by reference.</span></span>

### <a name="metadata-representation-of-in-parameters"></a><span data-ttu-id="3a43e-230">`in` 參數的中繼資料標記法。</span><span class="sxs-lookup"><span data-stu-id="3a43e-230">Metadata representation of `in` parameters.</span></span>
<span data-ttu-id="3a43e-231">當 `System.Runtime.CompilerServices.IsReadOnlyAttribute` 套用至 byref 參數時，表示參數是 `in` 參數。</span><span class="sxs-lookup"><span data-stu-id="3a43e-231">When `System.Runtime.CompilerServices.IsReadOnlyAttribute` is applied to a byref parameter, it means that the parameter is an `in` parameter.</span></span>

<span data-ttu-id="3a43e-232">此外，如果方法是*抽象*或*虛擬*，則這類參數的簽章（而且只有這類參數）必須有 `modreq[System.Runtime.InteropServices.InAttribute]`。</span><span class="sxs-lookup"><span data-stu-id="3a43e-232">In addition, if the method is *abstract* or *virtual*, then the signature of such parameters (and only such parameters) must have `modreq[System.Runtime.InteropServices.InAttribute]`.</span></span>

<span data-ttu-id="3a43e-233">**動機**：這是為了確保在方法覆寫/執行 `in` 參數相符的情況下，會進行這項作業。</span><span class="sxs-lookup"><span data-stu-id="3a43e-233">**Motivation**: this is done to ensure that in a case of method overriding/implementing the `in` parameters match.</span></span>

<span data-ttu-id="3a43e-234">相同的需求適用于委派中的 `Invoke` 方法。</span><span class="sxs-lookup"><span data-stu-id="3a43e-234">Same requirements apply to `Invoke` methods in delegates.</span></span>

<span data-ttu-id="3a43e-235">**動機**：這是為了確保現有編譯器在建立或指派委派時，不會直接忽略 `readonly`。</span><span class="sxs-lookup"><span data-stu-id="3a43e-235">**Motivation**: this is to ensure that existing compilers cannot simply ignore `readonly` when creating or assigning delegates.</span></span>

## <a name="returning-by-readonly-reference"></a><span data-ttu-id="3a43e-236">由 readonly 參考傳回。</span><span class="sxs-lookup"><span data-stu-id="3a43e-236">Returning by readonly reference.</span></span>

### <a name="motivation"></a><span data-ttu-id="3a43e-237">動機</span><span class="sxs-lookup"><span data-stu-id="3a43e-237">Motivation</span></span>
<span data-ttu-id="3a43e-238">這個子功能的動機大致上是對稱于 `in` 參數的原因-避免複製，但在傳回端上。</span><span class="sxs-lookup"><span data-stu-id="3a43e-238">The motivation for this sub-feature is roughly symmetrical to the reasons for the `in` parameters - avoiding copying, but on the returning side.</span></span> <span data-ttu-id="3a43e-239">在此功能之前，方法或索引子有兩個選項：1）以傳址方式傳回，並公開到可能的變化或2）以傳值方式傳回，因而導致複製。</span><span class="sxs-lookup"><span data-stu-id="3a43e-239">Prior to this feature, a method or an indexer had two options: 1) return by reference and be exposed to possible mutations or 2) return by value which results in copying.</span></span>

### <a name="solution-ref-readonly-returns"></a><span data-ttu-id="3a43e-240">方案（`ref readonly` 傳回）</span><span class="sxs-lookup"><span data-stu-id="3a43e-240">Solution (`ref readonly` returns)</span></span>  
<span data-ttu-id="3a43e-241">此功能可讓成員以傳址方式傳回變數，而不會將它們公開給變化。</span><span class="sxs-lookup"><span data-stu-id="3a43e-241">The feature allows a member to return variables by reference without exposing them to mutations.</span></span>

### <a name="declaring-ref-readonly-returning-members"></a><span data-ttu-id="3a43e-242">宣告 `ref readonly` 傳回成員</span><span class="sxs-lookup"><span data-stu-id="3a43e-242">Declaring `ref readonly` returning members</span></span>

<span data-ttu-id="3a43e-243">傳回簽章 `ref readonly` 的修飾片語合是用來表示成員會傳回 readonly 參考。</span><span class="sxs-lookup"><span data-stu-id="3a43e-243">A combination of modifiers `ref readonly` on the return signature is used to to indicate that the member returns a readonly reference.</span></span>

<span data-ttu-id="3a43e-244">就所有用途而言，會將 `ref readonly` 成員視為 `readonly` 變數，類似于 `readonly` 欄位和 `in` 參數。</span><span class="sxs-lookup"><span data-stu-id="3a43e-244">For all purposes a `ref readonly` member is treated as a `readonly` variable - similar to `readonly` fields and `in` parameters.</span></span>

<span data-ttu-id="3a43e-245">例如，具有結構類型 `ref readonly` 成員的欄位，都會以遞迴方式分類為 `readonly` 變數。</span><span class="sxs-lookup"><span data-stu-id="3a43e-245">For example fields of `ref readonly` member which has a struct type are all recursively classified as `readonly` variables.</span></span> <span data-ttu-id="3a43e-246">-允許將它們當做 `in` 引數傳遞，但不能當做 `ref` 或 `out` 引數。</span><span class="sxs-lookup"><span data-stu-id="3a43e-246">- It is permitted to pass them as `in` arguments, but not as `ref` or `out` arguments.</span></span>

```csharp
ref readonly Guid Method1()
{
}

Method2(in Method1()); // valid. Can pass as `in` argument.

Method3(ref Method1()); // not valid. Cannot pass as `ref` argument
```

- <span data-ttu-id="3a43e-247">允許在相同的位置 `ref` 傳回 `ref readonly` 傳回。</span><span class="sxs-lookup"><span data-stu-id="3a43e-247">`ref readonly` returns are allowed in the same places were `ref` returns are allowed.</span></span>
<span data-ttu-id="3a43e-248">這包括索引子、委派、lambda、區域函數。</span><span class="sxs-lookup"><span data-stu-id="3a43e-248">This includes indexers, delegates, lambdas, local functions.</span></span>

- <span data-ttu-id="3a43e-249">不允許在 `ref`/`ref readonly`/差異上多載。</span><span class="sxs-lookup"><span data-stu-id="3a43e-249">It is not permitted to overload on `ref`/`ref readonly` /  differences.</span></span>

- <span data-ttu-id="3a43e-250">允許在一般的 byval 上多載，並 `ref readonly` 傳回差異。</span><span class="sxs-lookup"><span data-stu-id="3a43e-250">It is permitted to overload on ordinary byval and `ref readonly` return differences.</span></span>

- <span data-ttu-id="3a43e-251">基於 OHI （多載、隱藏、執行）的目的，`ref readonly` 類似，但與 `ref`不同。</span><span class="sxs-lookup"><span data-stu-id="3a43e-251">For the purpose of OHI (Overloading, Hiding, Implementing), `ref readonly` is similar but distinct from `ref`.</span></span>
<span data-ttu-id="3a43e-252">例如，覆寫 `ref readonly` 一的方法，本身必須 `ref readonly` 而且具有可轉換身分識別的類型。</span><span class="sxs-lookup"><span data-stu-id="3a43e-252">For example the a method that overrides `ref readonly` one, must itself be `ref readonly` and have identity-convertible type.</span></span>

- <span data-ttu-id="3a43e-253">為了委派/lambda/方法群組轉換的目的，`ref readonly` 類似，但不同于 `ref`。</span><span class="sxs-lookup"><span data-stu-id="3a43e-253">For the purpose of delegate/lambda/method group conversions, `ref readonly` is similar but distinct from `ref`.</span></span>
<span data-ttu-id="3a43e-254">Lambda 和適用的方法群組轉換候選項目必須符合目標委派 `ref readonly` 傳回，並具有可轉換身分識別之類型的 `ref readonly` 傳回。</span><span class="sxs-lookup"><span data-stu-id="3a43e-254">Lambdas and applicable method group conversion candidates have to match `ref readonly` return of the target delegate with `ref readonly` return of the type that is identity-convertible.</span></span>

- <span data-ttu-id="3a43e-255">基於泛型變異數的目的，`ref readonly` 傳回 nonvariant。</span><span class="sxs-lookup"><span data-stu-id="3a43e-255">For the purpose of generic variance, `ref readonly` returns are nonvariant.</span></span>

> <span data-ttu-id="3a43e-256">注意：具有參考或基本類型的 `ref readonly` 傳回不會出現任何警告。</span><span class="sxs-lookup"><span data-stu-id="3a43e-256">NOTE: There are no warnings on `ref readonly` returns that have reference or primitives types.</span></span>
<span data-ttu-id="3a43e-257">這在一般情況下可能毫無意義，但在某些情況下，使用者必須/想以 `in`傳遞基本類型。</span><span class="sxs-lookup"><span data-stu-id="3a43e-257">It may be pointless in general, but in some cases user must/want to pass primitives as `in`.</span></span> <span data-ttu-id="3a43e-258">範例-將 `T` 取代為 `int`時，覆寫泛型方法（例如 `ref readonly T Method()`）。</span><span class="sxs-lookup"><span data-stu-id="3a43e-258">Examples - overriding a generic method like `ref readonly T Method()` when `T` was substituted to be `int`.</span></span>
>
><span data-ttu-id="3a43e-259">有一個分析器會在使用 `ref readonly` 傳回的效率不佳的情況下發出警告，但這類分析的規則會太模糊而無法成為語言規格的一部分。</span><span class="sxs-lookup"><span data-stu-id="3a43e-259">It is conceivable to have an analyzer that warns in cases of inefficient use of `ref readonly` returns, but the rules for such analysis would be too fuzzy to be a part of a language specification.</span></span>

### <a name="returning-from-ref-readonly-members"></a><span data-ttu-id="3a43e-260">從 `ref readonly` 成員返回</span><span class="sxs-lookup"><span data-stu-id="3a43e-260">Returning from `ref readonly` members</span></span>
<span data-ttu-id="3a43e-261">在方法主體中，語法與一般 ref 傳回相同。</span><span class="sxs-lookup"><span data-stu-id="3a43e-261">Inside the method body the syntax is the same as with regular ref returns.</span></span> <span data-ttu-id="3a43e-262">系統會從包含的方法推斷 `readonly`。</span><span class="sxs-lookup"><span data-stu-id="3a43e-262">The `readonly` will be inferred from the containing method.</span></span>

<span data-ttu-id="3a43e-263">動機是 `return ref readonly <expression>` 不需要長時間，而且只允許不相符的 `readonly` 部分一律會導致錯誤。</span><span class="sxs-lookup"><span data-stu-id="3a43e-263">The motivation is that `return ref readonly <expression>` is unnecessary long and only allows for mismatches on the `readonly` part that would always result in errors.</span></span>
<span data-ttu-id="3a43e-264">不過，`ref` 是與其他案例一致的情況，其中會透過嚴格的別名和傳值方式來傳遞專案。</span><span class="sxs-lookup"><span data-stu-id="3a43e-264">The `ref` is, however, required for consistency with other scenarios where something is passed via strict aliasing vs. by value.</span></span>

> <span data-ttu-id="3a43e-265">不同于具有 `in` 參數的案例，`ref readonly` 傳回的永遠不會透過本機複本傳回。</span><span class="sxs-lookup"><span data-stu-id="3a43e-265">Unlike the case with `in` parameters, `ref readonly` returns never return via a local copy.</span></span> <span data-ttu-id="3a43e-266">考慮到複製會在傳回這類做法時立即停止存在，是毫無意義且危險的。</span><span class="sxs-lookup"><span data-stu-id="3a43e-266">Considering that the copy would cease to exist immediately upon returning such practice would be pointless and dangerous.</span></span> <span data-ttu-id="3a43e-267">因此，`ref readonly` 傳回一律是直接參考。</span><span class="sxs-lookup"><span data-stu-id="3a43e-267">Therefore `ref readonly` returns are always direct references.</span></span>

<span data-ttu-id="3a43e-268">範例：</span><span class="sxs-lookup"><span data-stu-id="3a43e-268">Example:</span></span>

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

- <span data-ttu-id="3a43e-269">`return ref` 的引數必須是左值（**現有規則**）</span><span class="sxs-lookup"><span data-stu-id="3a43e-269">An argument of `return ref` must be an LValue (**existing rule**)</span></span>
- <span data-ttu-id="3a43e-270">`return ref` 的引數必須是「安全地傳回」（**現有規則**）</span><span class="sxs-lookup"><span data-stu-id="3a43e-270">An argument of `return ref` must be "safe to return" (**existing rule**)</span></span>
- <span data-ttu-id="3a43e-271">在 `ref readonly` 成員中，`return ref` 的引數_不需要是可寫入_的。</span><span class="sxs-lookup"><span data-stu-id="3a43e-271">In a `ref readonly` member an argument of `return ref` is _not required to be writeable_ .</span></span>
<span data-ttu-id="3a43e-272">例如，這類成員可以 ref-傳回 readonly 欄位或其中一個 `in` 參數。</span><span class="sxs-lookup"><span data-stu-id="3a43e-272">For example such member can ref-return a readonly field or one of its `in` parameters.</span></span>

### <a name="safe-to-return-rules"></a><span data-ttu-id="3a43e-273">可安全地傳回規則。</span><span class="sxs-lookup"><span data-stu-id="3a43e-273">Safe to Return rules.</span></span>
<span data-ttu-id="3a43e-274">一般安全傳回參考的規則也適用于 readonly 參考。</span><span class="sxs-lookup"><span data-stu-id="3a43e-274">Normal safe to return rules for references will apply to readonly references as well.</span></span>

<span data-ttu-id="3a43e-275">請注意，您可以從一般的 `ref` 本機/參數/傳回來取得 `ref readonly`，但不能從相反的方式取得。</span><span class="sxs-lookup"><span data-stu-id="3a43e-275">Note that a `ref readonly` can be obtained from a regular `ref` local/parameter/return, but not the other way around.</span></span> <span data-ttu-id="3a43e-276">否則，會以與一般 `ref` 傳回相同的方式推斷 `ref readonly` 傳回的安全。</span><span class="sxs-lookup"><span data-stu-id="3a43e-276">Otherwise the safety of `ref readonly` returns is inferred the same way as for regular `ref` returns.</span></span>

<span data-ttu-id="3a43e-277">考慮到右值可以當做 `in` 參數傳遞，並以 `ref readonly` 的方式傳回，我們需要一個以上的規則-**右值不是以傳址方式傳回安全**。</span><span class="sxs-lookup"><span data-stu-id="3a43e-277">Considering that RValues can be passed as `in` parameter and returned as `ref readonly` we need one more rule - **RValues are not safe-to-return by reference**.</span></span>

> <span data-ttu-id="3a43e-278">當右值透過複製傳遞至 `in` 參數，然後以 `ref readonly`的形式傳回時，請考慮這種情況。</span><span class="sxs-lookup"><span data-stu-id="3a43e-278">Consider the situation when an RValue is passed to an `in` parameter via a copy and then returned back in a form of a `ref readonly`.</span></span> <span data-ttu-id="3a43e-279">在呼叫端的內容中，這類調用的結果是本機資料的參考，因此不安全傳回。</span><span class="sxs-lookup"><span data-stu-id="3a43e-279">In the context of the caller the result of such invocation is a reference to local data and as such is unsafe to return.</span></span>
> <span data-ttu-id="3a43e-280">一旦右值不安全地傳回，現有的規則 `#6` 已經處理此情況。</span><span class="sxs-lookup"><span data-stu-id="3a43e-280">Once RValues are not safe to return, the existing rule `#6` already handles this case.</span></span>

<span data-ttu-id="3a43e-281">範例：</span><span class="sxs-lookup"><span data-stu-id="3a43e-281">Example:</span></span>
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

<span data-ttu-id="3a43e-282">已更新 `safe to return` 規則：</span><span class="sxs-lookup"><span data-stu-id="3a43e-282">Updated `safe to return` rules:</span></span>

1.  <span data-ttu-id="3a43e-283">**可以安全地傳回堆積上變數的 refs**</span><span class="sxs-lookup"><span data-stu-id="3a43e-283">**refs to variables on the heap are safe to return**</span></span>
2.  <span data-ttu-id="3a43e-284">**ref/in 參數可安全地**傳回
`in` 參數自然只能以 readonly 的形式傳回。</span><span class="sxs-lookup"><span data-stu-id="3a43e-284">**ref/in parameters are safe to return**
`in` parameters naturally can only be returned as readonly.</span></span>
3.  <span data-ttu-id="3a43e-285">**out 參數可放心**傳回（但必須明確指派，如同今天的案例）</span><span class="sxs-lookup"><span data-stu-id="3a43e-285">**out parameters are safe to return** (but must be definitely assigned, as is already the case today)</span></span>
4.  <span data-ttu-id="3a43e-286">**只要接收者可以安全傳回，實例結構欄位就可以安全地傳回。**</span><span class="sxs-lookup"><span data-stu-id="3a43e-286">**instance struct fields are safe to return as long as the receiver is safe to return**</span></span>
5.  <span data-ttu-id="3a43e-287">**' this ' 不安全地從結構成員傳回**</span><span class="sxs-lookup"><span data-stu-id="3a43e-287">**'this' is not safe to return from struct members**</span></span>
6.  <span data-ttu-id="3a43e-288">**如果傳遞至該方法做為正式參數的所有 refs/out 都是安全的，則從另一個方法傳回的 ref 會是安全的。** 
*明確地說，如果接收者是結構、類別或型別為泛型型別參數，則不會有任何不相關的情況。*</span><span class="sxs-lookup"><span data-stu-id="3a43e-288">**a ref, returned from another method is safe to return if all refs/outs passed to that method as formal parameters were safe to return.**
*Specifically it is irrelevant if receiver is safe to return, regardless whether receiver is a struct, class or typed as a generic type parameter.*</span></span>
7.  <span data-ttu-id="3a43e-289">**右值不安全，無法以傳址方式傳回。** 
*特別右值可安全地當做 in 參數傳遞。*</span><span class="sxs-lookup"><span data-stu-id="3a43e-289">**RValues are not safe to return by reference.**
*Specifically RValues are safe to pass as in parameters.*</span></span>

> <span data-ttu-id="3a43e-290">注意：牽涉到類似 ref 型別和 ref-的類型時，還有其他關於傳回安全的規則。</span><span class="sxs-lookup"><span data-stu-id="3a43e-290">NOTE: There are additional rules regarding safety of returns that come into play when ref-like types and ref-reassignments are involved.</span></span>
> <span data-ttu-id="3a43e-291">規則同樣適用于 `ref` 和 `ref readonly` 成員，因此此處並未提及。</span><span class="sxs-lookup"><span data-stu-id="3a43e-291">The rules equally apply to `ref` and `ref readonly` members and therefore are not mentioned here.</span></span>

### <a name="aliasing-behavior"></a><span data-ttu-id="3a43e-292">別名行為。</span><span class="sxs-lookup"><span data-stu-id="3a43e-292">Aliasing behavior.</span></span>
<span data-ttu-id="3a43e-293">`ref readonly` 成員提供與一般 `ref` 成員相同的別名行為（除了為 readonly 以外）。</span><span class="sxs-lookup"><span data-stu-id="3a43e-293">`ref readonly` members provide the same aliasing behavior as ordinary `ref` members (except for being readonly).</span></span>
<span data-ttu-id="3a43e-294">因此，為了在 lambda、非同步、反覆運算器、堆疊溢位等中進行捕獲，同樣的限制也適用。</span><span class="sxs-lookup"><span data-stu-id="3a43e-294">Therefore for the purpose of capturing in lambdas, async, iterators, stack spilling etc... the same restrictions apply.</span></span> <span data-ttu-id="3a43e-295">亦即.</span><span class="sxs-lookup"><span data-stu-id="3a43e-295">- I.E.</span></span> <span data-ttu-id="3a43e-296">由於無法捕獲實際的參考，而且因為成員評估有副作用的性質，因此不允許這類案例。</span><span class="sxs-lookup"><span data-stu-id="3a43e-296">due to inability to capture the actual references and due to side-effecting nature of member evaluation such scenarios are disallowed.</span></span>

> <span data-ttu-id="3a43e-297">當 `ref readonly` 傳回是一般結構方法的接收者時，允許並需要進行複製，這會將 `this` 視為一般的可寫入參考。</span><span class="sxs-lookup"><span data-stu-id="3a43e-297">It is permitted and required to make a copy when `ref readonly` return is a receiver of regular struct methods, which take `this` as an ordinary writeable reference.</span></span> <span data-ttu-id="3a43e-298">在過去，在這類調用套用至 readonly 變數的所有情況下，都會進行本機複本。</span><span class="sxs-lookup"><span data-stu-id="3a43e-298">Historically in all cases where such invocations are applied to readonly variable a local copy is made.</span></span>

### <a name="metadata-representation"></a><span data-ttu-id="3a43e-299">中繼資料標記法。</span><span class="sxs-lookup"><span data-stu-id="3a43e-299">Metadata representation.</span></span>
<span data-ttu-id="3a43e-300">當 `System.Runtime.CompilerServices.IsReadOnlyAttribute` 套用至傳回的 byref 傳回方法時，這表示該方法會傳回 readonly 參考。</span><span class="sxs-lookup"><span data-stu-id="3a43e-300">When `System.Runtime.CompilerServices.IsReadOnlyAttribute` is applied to the return of a byref returning method, it means that the method returns a readonly reference.</span></span>

<span data-ttu-id="3a43e-301">此外，這類方法的結果簽章（而且只有那些方法）必須有 `modreq[System.Runtime.CompilerServices.IsReadOnlyAttribute]`。</span><span class="sxs-lookup"><span data-stu-id="3a43e-301">In addition, the result signature of such methods (and only those methods) must have `modreq[System.Runtime.CompilerServices.IsReadOnlyAttribute]`.</span></span>

<span data-ttu-id="3a43e-302">**動機**：這是為了確保現有編譯器在叫用具有 `ref readonly` 傳回的方法時，不會直接忽略 `readonly`</span><span class="sxs-lookup"><span data-stu-id="3a43e-302">**Motivation**: this is to ensure that existing compilers cannot simply ignore `readonly` when invoking methods with `ref readonly` returns</span></span>

## <a name="readonly-structs"></a><span data-ttu-id="3a43e-303">Readonly 結構</span><span class="sxs-lookup"><span data-stu-id="3a43e-303">Readonly structs</span></span>
<span data-ttu-id="3a43e-304">在簡短的功能中，會為結構的所有實例成員 `this` 參數，但不包括函式、`in` 參數。</span><span class="sxs-lookup"><span data-stu-id="3a43e-304">In short - a feature that makes `this` parameter of all instance members of a struct, except for constructors, an `in` parameter.</span></span>

### <a name="motivation"></a><span data-ttu-id="3a43e-305">動機</span><span class="sxs-lookup"><span data-stu-id="3a43e-305">Motivation</span></span>
<span data-ttu-id="3a43e-306">編譯器必須假設結構實例上的任何方法呼叫可能會修改實例。</span><span class="sxs-lookup"><span data-stu-id="3a43e-306">Compiler must assume that any method call on a struct instance may modify the instance.</span></span> <span data-ttu-id="3a43e-307">事實上，可寫入的參考會以 `this` 參數的形式傳遞至方法，並完整啟用此行為。</span><span class="sxs-lookup"><span data-stu-id="3a43e-307">Indeed a writeable reference is passed to the method as `this` parameter and fully enables this behavior.</span></span> <span data-ttu-id="3a43e-308">為了在 `readonly` 變數上允許這類調用，會將調用套用至暫存複本。</span><span class="sxs-lookup"><span data-stu-id="3a43e-308">To allow such invocations on `readonly` variables, the invocations are applied to temp copies.</span></span> <span data-ttu-id="3a43e-309">這可能會圈子，而且有時會基於效能考慮，強制使用者放棄 `readonly`。</span><span class="sxs-lookup"><span data-stu-id="3a43e-309">That could be unintuitive and sometimes forces people to abandon `readonly` for performance reasons.</span></span>  
<span data-ttu-id="3a43e-310">範例： https://codeblog.jonskeet.uk/2014/07/16/micro-optimization-the-surprising-inefficiency-of-readonly-fields/</span><span class="sxs-lookup"><span data-stu-id="3a43e-310">Example: https://codeblog.jonskeet.uk/2014/07/16/micro-optimization-the-surprising-inefficiency-of-readonly-fields/</span></span>

<span data-ttu-id="3a43e-311">新增 `in` 參數的支援之後，`ref readonly` 會傳回防禦性複製的問題，因為 readonly 變數會變得更常見。</span><span class="sxs-lookup"><span data-stu-id="3a43e-311">After adding support for `in` parameters and `ref readonly` returns the problem of defensive copying will get worse since readonly variables will become more common.</span></span>

### <a name="solution"></a><span data-ttu-id="3a43e-312">解決方法</span><span class="sxs-lookup"><span data-stu-id="3a43e-312">Solution</span></span>
<span data-ttu-id="3a43e-313">在結構宣告上允許 `readonly` 修飾詞，這會導致 `this` 被視為所有結構實例方法上的 `in` 參數，但不包括函式。</span><span class="sxs-lookup"><span data-stu-id="3a43e-313">Allow `readonly` modifier on struct declarations which would result in `this` being treated as `in` parameter on all struct instance methods except for constructors.</span></span>

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

### <a name="restrictions-on-members-of-readonly-struct"></a><span data-ttu-id="3a43e-314">Readonly 結構成員的限制</span><span class="sxs-lookup"><span data-stu-id="3a43e-314">Restrictions on members of readonly struct</span></span>
- <span data-ttu-id="3a43e-315">Readonly 結構的實例欄位必須為 readonly。</span><span class="sxs-lookup"><span data-stu-id="3a43e-315">Instance fields of a readonly struct must be readonly.</span></span>  
<span data-ttu-id="3a43e-316">**動機：** 只能寫入外部，而不能透過成員。</span><span class="sxs-lookup"><span data-stu-id="3a43e-316">**Motivation:** can only be written to externally, but not through members.</span></span>
- <span data-ttu-id="3a43e-317">Readonly 結構的實例 autoproperties 必須是「僅限取得」。</span><span class="sxs-lookup"><span data-stu-id="3a43e-317">Instance autoproperties of a readonly struct must be get-only.</span></span>  
<span data-ttu-id="3a43e-318">**動機：** 實例欄位的限制結果。</span><span class="sxs-lookup"><span data-stu-id="3a43e-318">**Motivation:** consequence of restriction on instance fields.</span></span>
- <span data-ttu-id="3a43e-319">Readonly 結構可能不會宣告類似欄位的事件。</span><span class="sxs-lookup"><span data-stu-id="3a43e-319">Readonly struct may not declare field-like events.</span></span>  
<span data-ttu-id="3a43e-320">**動機：** 實例欄位的限制結果。</span><span class="sxs-lookup"><span data-stu-id="3a43e-320">**Motivation:** consequence of restriction on instance fields.</span></span>

### <a name="metadata-representation"></a><span data-ttu-id="3a43e-321">中繼資料標記法。</span><span class="sxs-lookup"><span data-stu-id="3a43e-321">Metadata representation.</span></span>
<span data-ttu-id="3a43e-322">當 `System.Runtime.CompilerServices.IsReadOnlyAttribute` 套用至實數值型別時，表示該類型為 `readonly struct`。</span><span class="sxs-lookup"><span data-stu-id="3a43e-322">When `System.Runtime.CompilerServices.IsReadOnlyAttribute` is applied to a value type, it means that the type is a `readonly struct`.</span></span>

<span data-ttu-id="3a43e-323">尤其是：</span><span class="sxs-lookup"><span data-stu-id="3a43e-323">In particular:</span></span>
-  <span data-ttu-id="3a43e-324">`IsReadOnlyAttribute` 類型的識別並不重要。</span><span class="sxs-lookup"><span data-stu-id="3a43e-324">The identity of the `IsReadOnlyAttribute` type is unimportant.</span></span> <span data-ttu-id="3a43e-325">事實上，編譯器可以視需要將它內嵌在包含元件中。</span><span class="sxs-lookup"><span data-stu-id="3a43e-325">In fact it can be embedded by the compiler in the containing assembly if needed.</span></span>

## <a name="refin-extension-methods"></a><span data-ttu-id="3a43e-326">`ref`/`in` 擴充方法</span><span class="sxs-lookup"><span data-stu-id="3a43e-326">`ref`/`in` extension methods</span></span>
<span data-ttu-id="3a43e-327">實際上，現有的提案（ https://github.com/dotnet/roslyn/issues/165) 和對應的原型 PR （ https://github.com/dotnet/roslyn/pull/15650)。</span><span class="sxs-lookup"><span data-stu-id="3a43e-327">There is actually an existing proposal (https://github.com/dotnet/roslyn/issues/165) and corresponding prototype PR (https://github.com/dotnet/roslyn/pull/15650).</span></span>
<span data-ttu-id="3a43e-328">我只想知道這個想法並非全新的。</span><span class="sxs-lookup"><span data-stu-id="3a43e-328">I just want to acknowledge that this idea is not entirely new.</span></span> <span data-ttu-id="3a43e-329">不過，這與此處相關，因為 `ref readonly` 適當地會移除這類方法最爭議的問題，這是如何使用右值接收器。</span><span class="sxs-lookup"><span data-stu-id="3a43e-329">It is, however, relevant here since `ref readonly` elegantly removes the most contentious issue about such methods - what to do with RValue receivers.</span></span>

<span data-ttu-id="3a43e-330">一般概念是讓擴充方法以傳址方式接受 `this` 參數，只要此型別已知為結構型別即可。</span><span class="sxs-lookup"><span data-stu-id="3a43e-330">The general idea is allowing extension methods to take the `this` parameter by reference, as long as the type is known to be a struct type.</span></span>

```csharp
public static void Extension(ref this Guid self)
{
    // do something
}
```

<span data-ttu-id="3a43e-331">撰寫這類擴充方法的原因主要是：</span><span class="sxs-lookup"><span data-stu-id="3a43e-331">The reasons for writing such extension methods are primarily:</span></span>  
1.  <span data-ttu-id="3a43e-332">當接收者是大型結構時避免複製</span><span class="sxs-lookup"><span data-stu-id="3a43e-332">Avoid copying when receiver is a large struct</span></span>
2.  <span data-ttu-id="3a43e-333">允許改變結構上的擴充方法</span><span class="sxs-lookup"><span data-stu-id="3a43e-333">Allow mutating extension methods on structs</span></span>

<span data-ttu-id="3a43e-334">我們不想在類別上允許這種情況的原因</span><span class="sxs-lookup"><span data-stu-id="3a43e-334">The reasons why we do not want to allow this on classes</span></span>  
1.  <span data-ttu-id="3a43e-335">這是非常有限的用途。</span><span class="sxs-lookup"><span data-stu-id="3a43e-335">It would be of very limited purpose.</span></span>
2.  <span data-ttu-id="3a43e-336">它會中斷長時間的持續性，因為方法呼叫無法在叫用後將非`null` 的接收者變成 `null`。</span><span class="sxs-lookup"><span data-stu-id="3a43e-336">It would break long standing invariant that a method call cannot turn non-`null` receiver to become `null` after invocation.</span></span>
> <span data-ttu-id="3a43e-337">事實上，除非 `ref` 或 `out`_明確_指派或傳遞非`null` 變數，否則無法變成 `null`。</span><span class="sxs-lookup"><span data-stu-id="3a43e-337">In fact, currently a non-`null` variable cannot become `null` unless _explicitly_ assigned or passed by `ref` or `out`.</span></span>
> <span data-ttu-id="3a43e-338">這麼做可大幅輔助可讀性或其他形式的「這是 null，這裡」的分析。</span><span class="sxs-lookup"><span data-stu-id="3a43e-338">That greatly aids readability or other forms of "can this be a null here" analysis.</span></span>
3.  <span data-ttu-id="3a43e-339">使用 null 條件存取的「評估一次」語義，會很難進行協調。</span><span class="sxs-lookup"><span data-stu-id="3a43e-339">It would be hard to reconcile with "evaluate once" semantics of null-conditional accesses.</span></span>
<span data-ttu-id="3a43e-340">範例： `obj.stringField?.RefExtension(...)`-需要捕捉 `stringField` 的複本，讓 null 檢查有意義，但是在 RefExtension 內 `this` 的指派不會反映回欄位。</span><span class="sxs-lookup"><span data-stu-id="3a43e-340">Example: `obj.stringField?.RefExtension(...)` - need to capture a copy of `stringField` to make the null check meaningful, but then assignments to `this` inside RefExtension would not be reflected back to the field.</span></span>

<span data-ttu-id="3a43e-341">在以傳址方式接受第一個引數的**結構**上宣告擴充方法的功能，是長期的要求。</span><span class="sxs-lookup"><span data-stu-id="3a43e-341">An ability to declare extension methods on **structs** that take the first argument by reference was a long-standing request.</span></span> <span data-ttu-id="3a43e-342">其中一個封鎖考慮是「如果接收者不是左值，會發生什麼事？」。</span><span class="sxs-lookup"><span data-stu-id="3a43e-342">One of the blocking consideration was "what happens if receiver is not an LValue?".</span></span>

- <span data-ttu-id="3a43e-343">您也可以將任何擴充方法當做靜態方法呼叫（有時候這是解決多義性的唯一方法）。</span><span class="sxs-lookup"><span data-stu-id="3a43e-343">There is a precedent that any extension method could also be called as a static method (sometimes it is the only way to resolve ambiguity).</span></span> <span data-ttu-id="3a43e-344">它會規定應該不允許右值接收器。</span><span class="sxs-lookup"><span data-stu-id="3a43e-344">It would dictate that RValue receivers should be disallowed.</span></span>
- <span data-ttu-id="3a43e-345">另一方面，當牽涉到結構實例方法時，會在類似情況下對複本進行調用。</span><span class="sxs-lookup"><span data-stu-id="3a43e-345">On the other hand there is a practice of making invocation on a copy in similar situations when struct instance methods are involved.</span></span>

<span data-ttu-id="3a43e-346">「隱含複製」存在的原因是因為大部分的結構方法實際上不會修改結構，而無法表示。</span><span class="sxs-lookup"><span data-stu-id="3a43e-346">The reason why the "implicit copying" exists is because the majority of struct methods do not actually modify the struct while not being able to indicate that.</span></span> <span data-ttu-id="3a43e-347">因此，最實用的解決方法是只在複本上進行調用，但這種作法是已知的效能和造成錯誤。</span><span class="sxs-lookup"><span data-stu-id="3a43e-347">Therefore the most practical solution was to just make the invocation on a copy, but this practice is known for harming performance and causing bugs.</span></span>

<span data-ttu-id="3a43e-348">現在，有 `in` 參數的可用性，延伸模組可能會通知意圖。</span><span class="sxs-lookup"><span data-stu-id="3a43e-348">Now, with availability of `in` parameters, it is possible for an extension to signal the intent.</span></span> <span data-ttu-id="3a43e-349">因此，可以藉由要求使用可寫入的接收者來呼叫 `ref` 延伸模組來解決之間謎，而 `in` 延伸模組則允許隱含複製（如有必要）。</span><span class="sxs-lookup"><span data-stu-id="3a43e-349">Therefore the conundrum can be resolved by requiring `ref` extensions to be called with writeable receivers while `in` extensions permit implicit copying if necessary.</span></span>

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

### <a name="in-extensions-and-generics"></a><span data-ttu-id="3a43e-350">`in` 擴充功能和泛型。</span><span class="sxs-lookup"><span data-stu-id="3a43e-350">`in` extensions and generics.</span></span>
<span data-ttu-id="3a43e-351">`ref` 擴充方法的目的，是要直接或藉由叫用改變成員的方式來變更接收器。</span><span class="sxs-lookup"><span data-stu-id="3a43e-351">The purpose of `ref` extension methods is to mutate the receiver directly or by invoking mutating members.</span></span> <span data-ttu-id="3a43e-352">因此，只要 `T` 限制為結構，就會允許 `ref this T` 延伸模組。</span><span class="sxs-lookup"><span data-stu-id="3a43e-352">Therefore `ref this T` extensions are allowed as long as `T` is constrained to be a struct.</span></span>

<span data-ttu-id="3a43e-353">另一方面，`in` 擴充方法會特別存在，以減少隱含複製。</span><span class="sxs-lookup"><span data-stu-id="3a43e-353">On the other hand `in` extension methods exist specifically to reduce implicit copying.</span></span> <span data-ttu-id="3a43e-354">不過，任何使用 `in T` 參數都必須透過介面成員來完成。</span><span class="sxs-lookup"><span data-stu-id="3a43e-354">However any use of an `in T` parameter will have to be done through an interface member.</span></span> <span data-ttu-id="3a43e-355">由於所有介面成員都被視為改變，因此任何這類使用都需要複製。</span><span class="sxs-lookup"><span data-stu-id="3a43e-355">Since all interface members are considered mutating, any such use would require a copy.</span></span> <span data-ttu-id="3a43e-356">-而不是減少複製，效果會相反。</span><span class="sxs-lookup"><span data-stu-id="3a43e-356">- Instead of reducing copying, the effect would be the opposite.</span></span> <span data-ttu-id="3a43e-357">因此，當 `T` 是泛型型別參數，而不是任何條件約束時，就不允許 `in this T`。</span><span class="sxs-lookup"><span data-stu-id="3a43e-357">Therefore `in this T` is not allowed when `T` is a generic type parameter regardless of constraints.</span></span>

### <a name="valid-kinds-of-extension-methods-recap"></a><span data-ttu-id="3a43e-358">有效的延伸方法類型（回顧）：</span><span class="sxs-lookup"><span data-stu-id="3a43e-358">Valid kinds of extension methods (recap):</span></span>
<span data-ttu-id="3a43e-359">擴充方法中的下列 `this` 宣告形式現在是允許的：</span><span class="sxs-lookup"><span data-stu-id="3a43e-359">The following forms of `this` declaration in an extension method are now allowed:</span></span>
1) <span data-ttu-id="3a43e-360">`this T arg` 標準的 byval 延伸模組。</span><span class="sxs-lookup"><span data-stu-id="3a43e-360">`this T arg` - regular byval extension.</span></span> <span data-ttu-id="3a43e-361">（**現有的案例**）</span><span class="sxs-lookup"><span data-stu-id="3a43e-361">(**existing case**)</span></span>
- <span data-ttu-id="3a43e-362">T 可以是任何類型，包括參考型別或類型參數。</span><span class="sxs-lookup"><span data-stu-id="3a43e-362">T can be any type, including reference types or type parameters.</span></span>
<span data-ttu-id="3a43e-363">實例將會是呼叫之後的相同變數。</span><span class="sxs-lookup"><span data-stu-id="3a43e-363">Instance will be the same variable after the call.</span></span>
<span data-ttu-id="3a43e-364">允許隱含轉換_此引數轉換_種類。</span><span class="sxs-lookup"><span data-stu-id="3a43e-364">Allows implicit conversions of _this-argument-conversion_ kind.</span></span>
<span data-ttu-id="3a43e-365">可以在右值上呼叫。</span><span class="sxs-lookup"><span data-stu-id="3a43e-365">Can be called on RValues.</span></span>

- <span data-ttu-id="3a43e-366">`in this T self` - `in` 延伸模組。</span><span class="sxs-lookup"><span data-stu-id="3a43e-366">`in this T self` - `in` extension.</span></span>
<span data-ttu-id="3a43e-367">T 必須是實際的結構類型。</span><span class="sxs-lookup"><span data-stu-id="3a43e-367">T must be an actual struct type.</span></span>
<span data-ttu-id="3a43e-368">實例將會是呼叫之後的相同變數。</span><span class="sxs-lookup"><span data-stu-id="3a43e-368">Instance will be the same variable after the call.</span></span>
<span data-ttu-id="3a43e-369">允許隱含轉換_此引數轉換_種類。</span><span class="sxs-lookup"><span data-stu-id="3a43e-369">Allows implicit conversions of _this-argument-conversion_ kind.</span></span>
<span data-ttu-id="3a43e-370">可以在右值上呼叫（如有需要，可在暫存上叫用）。</span><span class="sxs-lookup"><span data-stu-id="3a43e-370">Can be called on RValues (may be invoked on a temp if needed).</span></span>

- <span data-ttu-id="3a43e-371">`ref this T self` - `ref` 延伸模組。</span><span class="sxs-lookup"><span data-stu-id="3a43e-371">`ref this T self` - `ref` extension.</span></span>
<span data-ttu-id="3a43e-372">T 必須是結構類型，或限制為結構的泛型型別參數。</span><span class="sxs-lookup"><span data-stu-id="3a43e-372">T must be a struct type or a generic type parameter constrained to be a struct.</span></span>
<span data-ttu-id="3a43e-373">實例可能會由調用寫入。</span><span class="sxs-lookup"><span data-stu-id="3a43e-373">Instance may be written to by the invocation.</span></span>
<span data-ttu-id="3a43e-374">僅允許身分識別轉換。</span><span class="sxs-lookup"><span data-stu-id="3a43e-374">Allows only identity conversions.</span></span>
<span data-ttu-id="3a43e-375">必須在可寫入的左值上呼叫。</span><span class="sxs-lookup"><span data-stu-id="3a43e-375">Must be called on writeable LValue.</span></span> <span data-ttu-id="3a43e-376">（絕不會透過 temp 叫用）。</span><span class="sxs-lookup"><span data-stu-id="3a43e-376">(never invoked via a temp).</span></span>

## <a name="readonly-ref-locals"></a><span data-ttu-id="3a43e-377">Readonly ref 區域變數。</span><span class="sxs-lookup"><span data-stu-id="3a43e-377">Readonly ref locals.</span></span>

### <a name="motivation"></a><span data-ttu-id="3a43e-378">動機.</span><span class="sxs-lookup"><span data-stu-id="3a43e-378">Motivation.</span></span>
<span data-ttu-id="3a43e-379">一旦引進 `ref readonly` 成員之後，就可以清楚地使用它們需要與適當的本機類型配對。</span><span class="sxs-lookup"><span data-stu-id="3a43e-379">Once `ref readonly` members were introduced, it was clear from the use that they need to be paired with appropriate kind of local.</span></span> <span data-ttu-id="3a43e-380">成員的評估可能會產生或觀察副作用，因此，如果結果必須使用超過一次，則需要加以儲存。</span><span class="sxs-lookup"><span data-stu-id="3a43e-380">Evaluation of a member may produce or observe side effects, therefore if the result must be used more than once, it needs to be stored.</span></span> <span data-ttu-id="3a43e-381">一般 `ref` 區域變數在此不會有説明，因為無法為其指派 `readonly` 參考。</span><span class="sxs-lookup"><span data-stu-id="3a43e-381">Ordinary `ref` locals do not help here since they cannot be assigned a `readonly` reference.</span></span>   

### <a name="solution"></a><span data-ttu-id="3a43e-382">解.</span><span class="sxs-lookup"><span data-stu-id="3a43e-382">Solution.</span></span>
<span data-ttu-id="3a43e-383">允許宣告 `ref readonly` 區域變數。</span><span class="sxs-lookup"><span data-stu-id="3a43e-383">Allow declaring `ref readonly` locals.</span></span> <span data-ttu-id="3a43e-384">這是一種新的 `ref` 區域變數，無法寫入。</span><span class="sxs-lookup"><span data-stu-id="3a43e-384">This is a new kind of `ref` locals that is not writeable.</span></span> <span data-ttu-id="3a43e-385">因此 `ref readonly` 區域變數可以接受 readonly 變數的參考，而不會將這些變數公開給寫入。</span><span class="sxs-lookup"><span data-stu-id="3a43e-385">As a result `ref readonly` locals can accept references to readonly variables without exposing these variables to writes.</span></span>

### <a name="declaring-and-using-ref-readonly-locals"></a><span data-ttu-id="3a43e-386">宣告和使用 `ref readonly` 區域變數。</span><span class="sxs-lookup"><span data-stu-id="3a43e-386">Declaring and using `ref readonly` locals.</span></span>

<span data-ttu-id="3a43e-387">這類區域變數的語法會在宣告網站使用 `ref readonly` 修飾詞（依該特定順序）。</span><span class="sxs-lookup"><span data-stu-id="3a43e-387">The syntax of such locals uses `ref readonly` modifiers at declaration site (in that specific order).</span></span> <span data-ttu-id="3a43e-388">類似于一般 `ref` 區域變數，`ref readonly` 區域變數必須在宣告時以 ref 初始化。</span><span class="sxs-lookup"><span data-stu-id="3a43e-388">Similarly to ordinary `ref` locals, `ref readonly` locals must be ref-initialized at declaration.</span></span> <span data-ttu-id="3a43e-389">不同于一般 `ref` 區域變數，`ref readonly` 區域變數可以參考 `readonly` 的左值，例如 `in` 參數、`readonly` 欄位 `ref readonly` 方法。</span><span class="sxs-lookup"><span data-stu-id="3a43e-389">Unlike regular `ref` locals, `ref readonly` locals can refer to `readonly` LValues like `in` parameters, `readonly` fields, `ref readonly` methods.</span></span>

<span data-ttu-id="3a43e-390">就所有用途而言，會將 `ref readonly` 本機視為 `readonly` 變數。</span><span class="sxs-lookup"><span data-stu-id="3a43e-390">For all purposes a `ref readonly` local is treated as a `readonly` variable.</span></span> <span data-ttu-id="3a43e-391">使用的大部分限制與 `readonly` 欄位或 `in` 參數相同。</span><span class="sxs-lookup"><span data-stu-id="3a43e-391">Most of the restrictions on the use are the same as with `readonly` fields or `in` parameters.</span></span>

<span data-ttu-id="3a43e-392">例如，具有結構類型之 `in` 參數的欄位，都會以遞迴方式分類為 `readonly` 變數。</span><span class="sxs-lookup"><span data-stu-id="3a43e-392">For example fields of an `in` parameter which has a struct type are all recursively classified as `readonly` variables .</span></span>   

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

### <a name="restrictions-on-use-of-ref-readonly-locals"></a><span data-ttu-id="3a43e-393">使用 `ref readonly` 區域變數的限制</span><span class="sxs-lookup"><span data-stu-id="3a43e-393">Restrictions on use of `ref readonly` locals</span></span>
<span data-ttu-id="3a43e-394">除了其 `readonly` 本質以外，`ref readonly` 區域變數的行為就像一般 `ref` 區域變數，而且受到完全相同的限制。</span><span class="sxs-lookup"><span data-stu-id="3a43e-394">Except for their `readonly` nature, `ref readonly` locals behave like ordinary `ref` locals and are subject to exactly same restrictions.</span></span>  
<span data-ttu-id="3a43e-395">如需在「閉」中捕捉的相關限制，請在 `async` 方法中宣告，或 `safe-to-return` 分析同樣適用于 `ref readonly` 區域變數。</span><span class="sxs-lookup"><span data-stu-id="3a43e-395">For example restrictions related to capturing in closures, declaring in `async` methods or the `safe-to-return` analysis equally applies to `ref readonly` locals.</span></span>

## <a name="ternary-ref-expressions-aka-conditional-lvalues"></a><span data-ttu-id="3a43e-396">三元 `ref` 運算式。</span><span class="sxs-lookup"><span data-stu-id="3a43e-396">Ternary `ref` expressions.</span></span> <span data-ttu-id="3a43e-397">（也稱為「條件式左值」）</span><span class="sxs-lookup"><span data-stu-id="3a43e-397">(aka "Conditional LValues")</span></span>

### <a name="motivation"></a><span data-ttu-id="3a43e-398">動機</span><span class="sxs-lookup"><span data-stu-id="3a43e-398">Motivation</span></span>
<span data-ttu-id="3a43e-399">使用 `ref` 和 `ref readonly` 區域變數公開的需求，必須根據條件，使用一個或另一個目標變數來對這類區域變數進行 ref 初始化。</span><span class="sxs-lookup"><span data-stu-id="3a43e-399">Use of `ref` and `ref readonly` locals exposed a need to ref-initialize such locals with one or another target variable based on a condition.</span></span>

<span data-ttu-id="3a43e-400">一般的因應措施是引進如下的方法：</span><span class="sxs-lookup"><span data-stu-id="3a43e-400">A typical workaround is to introduce a method like:</span></span>

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

<span data-ttu-id="3a43e-401">請注意，`Choice` 不是三元的確切取代，因為_所有_引數都必須在呼叫位置評估，這會導致圈子行為和 bug。</span><span class="sxs-lookup"><span data-stu-id="3a43e-401">Note that `Choice` is not an exact replacement of a ternary since _all_ arguments must be evaluated at the call site, which was leading to unintuitive behavior and bugs.</span></span>

<span data-ttu-id="3a43e-402">下列作業將無法如預期般運作：</span><span class="sxs-lookup"><span data-stu-id="3a43e-402">The following will not work as expected:</span></span>

```csharp
    // will crash with NRE because 'arr[0]' will be executed unconditionally
    ref var r = ref Choice(arr != null, ref arr[0], ref otherArr[0]);
```

### <a name="solution"></a><span data-ttu-id="3a43e-403">解決方法</span><span class="sxs-lookup"><span data-stu-id="3a43e-403">Solution</span></span>
<span data-ttu-id="3a43e-404">允許特殊類型的條件運算式，根據條件評估為其中一個左值引數的參考。</span><span class="sxs-lookup"><span data-stu-id="3a43e-404">Allow special kind of conditional expression that evaluates to a reference to one of LValue argument based on a condition.</span></span>

### <a name="using-ref-ternary-expression"></a><span data-ttu-id="3a43e-405">使用 `ref` 的三元運算式。</span><span class="sxs-lookup"><span data-stu-id="3a43e-405">Using `ref` ternary expression.</span></span>

<span data-ttu-id="3a43e-406">條件運算式 `ref` 類別的語法為 `<condition> ? ref <consequence> : ref <alternative>;`</span><span class="sxs-lookup"><span data-stu-id="3a43e-406">The syntax for the `ref` flavor of a conditional expression is `<condition> ? ref <consequence> : ref <alternative>;`</span></span>

<span data-ttu-id="3a43e-407">就像使用一般條件運算式一樣 `<consequence>` 或 `<alternative>` 會根據布林值條件運算式的結果進行評估。</span><span class="sxs-lookup"><span data-stu-id="3a43e-407">Just like with the ordinary conditional expression only `<consequence>` or `<alternative>` is evaluated depending on result of the boolean condition expression.</span></span>

<span data-ttu-id="3a43e-408">不同于一般條件運算式，`ref` 條件運算式：</span><span class="sxs-lookup"><span data-stu-id="3a43e-408">Unlike ordinary conditional expression, `ref` conditional expression:</span></span>
- <span data-ttu-id="3a43e-409">需要 `<consequence>` 和 `<alternative>` 左值。</span><span class="sxs-lookup"><span data-stu-id="3a43e-409">requires that `<consequence>` and `<alternative>` are LValues.</span></span>
- <span data-ttu-id="3a43e-410">`ref` 條件運算式本身為左值，而</span><span class="sxs-lookup"><span data-stu-id="3a43e-410">`ref` conditional expression itself is an LValue and</span></span>
- <span data-ttu-id="3a43e-411">如果 `<consequence>` 和 `<alternative>` 都是可寫入的左值，則可寫入 `ref` 條件運算式</span><span class="sxs-lookup"><span data-stu-id="3a43e-411">`ref` conditional expression is writeable if both `<consequence>` and `<alternative>` are writeable LValues</span></span>

<span data-ttu-id="3a43e-412">範例：</span><span class="sxs-lookup"><span data-stu-id="3a43e-412">Examples:</span></span>  
<span data-ttu-id="3a43e-413">`ref` 三元是左值，因此可以透過傳址方式傳遞/指派/傳回;</span><span class="sxs-lookup"><span data-stu-id="3a43e-413">`ref` ternary is an LValue and as such it can be passed/assigned/returned by reference;</span></span>
```csharp
     // pass by reference
     foo(ref (arr != null ? ref arr[0]: ref otherArr[0]));

     // return by reference
     return ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

<span data-ttu-id="3a43e-414">做為左值，也可以指派給。</span><span class="sxs-lookup"><span data-stu-id="3a43e-414">Being an LValue, it can also be assigned to.</span></span>
```csharp
     // assign to
     (arr != null ? ref arr[0]: ref otherArr[0]) = 1;

     // error. readOnlyField is readonly and thus conditional expression is readonly
     (arr != null ? ref arr[0]: ref obj.readOnlyField) = 1;
```

<span data-ttu-id="3a43e-415">可用來做為方法呼叫的接收者，並在必要時略過複製。</span><span class="sxs-lookup"><span data-stu-id="3a43e-415">Can be used as a receiver of a method call and skip copying if necessary.</span></span>
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

<span data-ttu-id="3a43e-416">`ref` 三元也可以用在一般（非 ref）內容中。</span><span class="sxs-lookup"><span data-stu-id="3a43e-416">`ref` ternary can be used in a regular (not ref) context as well.</span></span>
```csharp
     // only an example
     // a regular ternary could work here just the same
     int x = (arr != null ? ref arr[0]: ref otherArr[0]);
```

### <a name="drawbacks"></a><span data-ttu-id="3a43e-417">缺點</span><span class="sxs-lookup"><span data-stu-id="3a43e-417">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="3a43e-418">我可以看到兩個主要引數，以增強對參考和唯讀參考的支援：</span><span class="sxs-lookup"><span data-stu-id="3a43e-418">I can see two major arguments against enhanced support for references and readonly references:</span></span>

1) <span data-ttu-id="3a43e-419">這裡解決的問題非常舊。</span><span class="sxs-lookup"><span data-stu-id="3a43e-419">The problems that are solved here are very old.</span></span> <span data-ttu-id="3a43e-420">為什麼突然解決這些問題，特別是因為它不會協助現有的程式碼？</span><span class="sxs-lookup"><span data-stu-id="3a43e-420">Why suddenly solve them now, especially since it would not help existing code?</span></span>

<span data-ttu-id="3a43e-421">如我們在C#新網域中發現和使用 .net，有些問題會變得更明顯。</span><span class="sxs-lookup"><span data-stu-id="3a43e-421">As we find C# and .Net used in new domains, some problems become more prominent.</span></span>  
<span data-ttu-id="3a43e-422">作為環境的範例，比計算額外負荷的平均值更重要，我可以列出</span><span class="sxs-lookup"><span data-stu-id="3a43e-422">As examples of environments that are more critical than average about computation overheads, I can list</span></span>

* <span data-ttu-id="3a43e-423">針對計算費用和回應能力的雲端/資料中心案例，是競爭優勢。</span><span class="sxs-lookup"><span data-stu-id="3a43e-423">cloud/datacenter scenarios where computation is billed for and responsiveness is a competitive advantage.</span></span>
* <span data-ttu-id="3a43e-424">具有延遲之軟性需求的遊戲/VR/AR</span><span class="sxs-lookup"><span data-stu-id="3a43e-424">Games/VR/AR with soft-realtime requirements on latencies</span></span>     

<span data-ttu-id="3a43e-425">這項功能不會犧牲任何現有的優勢（例如型別安全），同時允許在某些常見案例中降低額外負荷。</span><span class="sxs-lookup"><span data-stu-id="3a43e-425">This feature does not sacrifice any of the existing strengths such as type-safety, while allowing to lower overheads in some common scenarios.</span></span>

2) <span data-ttu-id="3a43e-426">我們可以合理保證被呼叫者在加入宣告 `readonly` 合約時，會由規則扮演嗎？</span><span class="sxs-lookup"><span data-stu-id="3a43e-426">Can we reasonably guarantee that the callee will play by the rules when it opts into `readonly` contracts?</span></span>

<span data-ttu-id="3a43e-427">使用 `out`時，我們會有類似的信任。</span><span class="sxs-lookup"><span data-stu-id="3a43e-427">We have similar trust when using `out`.</span></span> <span data-ttu-id="3a43e-428">不正確的 `out` 執行可能會導致未指定的行為，但事實上，這很少發生。</span><span class="sxs-lookup"><span data-stu-id="3a43e-428">Incorrect implementation of `out` can cause unspecified behavior, but in reality it rarely happens.</span></span>  

<span data-ttu-id="3a43e-429">建立熟悉的正式驗證規則 `ref readonly` 會進一步緩和信任問題。</span><span class="sxs-lookup"><span data-stu-id="3a43e-429">Making the formal verification rules familiar with `ref readonly` would further mitigate the trust issue.</span></span>

### <a name="alternatives"></a><span data-ttu-id="3a43e-430">替代方案</span><span class="sxs-lookup"><span data-stu-id="3a43e-430">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="3a43e-431">主要的競爭設計其實就是「不執行任何動作」。</span><span class="sxs-lookup"><span data-stu-id="3a43e-431">The main competing design is really "do nothing".</span></span>

### <a name="unresolved-questions"></a><span data-ttu-id="3a43e-432">未解決的問題</span><span class="sxs-lookup"><span data-stu-id="3a43e-432">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

### <a name="design-meetings"></a><span data-ttu-id="3a43e-433">設計會議</span><span class="sxs-lookup"><span data-stu-id="3a43e-433">Design meetings</span></span>

<span data-ttu-id="3a43e-434">https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-02-22.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-01.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-08-28.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-25.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-27.md</span><span class="sxs-lookup"><span data-stu-id="3a43e-434">https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-02-22.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-01.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-08-28.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-25.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-27.md</span></span>
