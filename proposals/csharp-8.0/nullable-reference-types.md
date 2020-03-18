---
ms.openlocfilehash: ecdad8c863d0695bc901e4d96d9ca3decbc248eb
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484612"
---
# <a name="nullable-reference-types-in-c"></a><span data-ttu-id="8bc1c-101">中的可為 null 的參考型別C#</span><span class="sxs-lookup"><span data-stu-id="8bc1c-101">Nullable reference types in C#</span></span> #

<span data-ttu-id="8bc1c-102">這項功能的目標是：</span><span class="sxs-lookup"><span data-stu-id="8bc1c-102">The goal of this feature is to:</span></span>

* <span data-ttu-id="8bc1c-103">允許開發人員表達參考型別的變數、參數或結果是否應為 null。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-103">Allow developers to express whether a variable, parameter or result of a reference type is intended to be null or not.</span></span>
* <span data-ttu-id="8bc1c-104">當此類變數、參數和結果未根據該意圖使用時，提供警告。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-104">Provide warnings when such variables, parameters and results are not used according to that intent.</span></span>

## <a name="expression-of-intent"></a><span data-ttu-id="8bc1c-105">意圖運算式</span><span class="sxs-lookup"><span data-stu-id="8bc1c-105">Expression of intent</span></span>

<span data-ttu-id="8bc1c-106">此語言已包含實數值型別的 `T?` 語法。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-106">The language already contains the `T?` syntax for value types.</span></span> <span data-ttu-id="8bc1c-107">將此語法延伸至參考型別相當簡單。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-107">It is straightforward to extend this syntax to reference types.</span></span>

<span data-ttu-id="8bc1c-108">假設未參考型別的目的是要讓它成為非 null 的 `T`。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-108">It is assumed that the intent of an unadorned reference type `T` is for it to be non-null.</span></span>

## <a name="checking-of-nullable-references"></a><span data-ttu-id="8bc1c-109">檢查可為 null 的參考</span><span class="sxs-lookup"><span data-stu-id="8bc1c-109">Checking of nullable references</span></span>

<span data-ttu-id="8bc1c-110">流程分析會追蹤可為 null 的參考變數。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-110">A flow analysis tracks nullable reference variables.</span></span> <span data-ttu-id="8bc1c-111">當分析認為它們不是 null （例如，在檢查或指派之後）時，其值將被視為非 null 參考。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-111">Where the analysis deems that they would not be null (e.g. after a check or an assignment), their value will be considered a non-null reference.</span></span>

<span data-ttu-id="8bc1c-112">當流程分析無法建立開發人員知道的非 null 情況時，可為 null 的參考也可以使用後置 `x!` 運算子（"dammit" 運算子）明確地視為非 null。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-112">A nullable reference can also explicitly be treated as non-null with the postfix `x!` operator (the "dammit" operator), for when flow analysis cannot establish a non-null situation that the developer knows is there.</span></span>

<span data-ttu-id="8bc1c-113">否則，如果可為 null 參考被取值，或轉換成非 null 的型別，則會提供警告。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-113">Otherwise, a warning is given if a nullable reference is dereferenced, or is converted to a non-null type.</span></span>

<span data-ttu-id="8bc1c-114">從 `S[]` 轉換成 `T?[]` 和從 `S?[]` 到 `T[]`時，會提供警告。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-114">A warning is given when converting from `S[]` to `T?[]` and from `S?[]` to `T[]`.</span></span>

<span data-ttu-id="8bc1c-115">從 `C<S>` 轉換成 `C<T?>` 時，除非型別參數是協變數（`out`），而從 `C<S?>` 轉換成 `C<T>` （當型別參數是逆變性（`in`）時除外）。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-115">A warning is given when converting from `C<S>` to `C<T?>` except when the type parameter is covariant (`out`), and when converting from `C<S?>` to `C<T>` except when the type parameter is contravariant (`in`).</span></span>

<span data-ttu-id="8bc1c-116">如果類型參數具有非 null 的條件約束，則會在 `C<T?>` 上提供警告。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-116">A warning is given on `C<T?>` if the type parameter has non-null constraints.</span></span> 

## <a name="checking-of-non-null-references"></a><span data-ttu-id="8bc1c-117">檢查非 null 參考</span><span class="sxs-lookup"><span data-stu-id="8bc1c-117">Checking of non-null references</span></span>

<span data-ttu-id="8bc1c-118">如果將 null 常值指派給非 null 變數或當做非 null 參數傳遞，則會提供警告。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-118">A warning is given if a null literal is assigned to a non-null variable or passed as a non-null parameter.</span></span>

<span data-ttu-id="8bc1c-119">如果函式未明確初始化非 null 的參考欄位，也會提供警告。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-119">A warning is also given if a constructor does not explicitly initialize non-null reference fields.</span></span>

<span data-ttu-id="8bc1c-120">我們無法充分追蹤非 null 參考陣列的所有元素都已初始化。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-120">We cannot adequately track that all elements of an array of non-null references are initialized.</span></span> <span data-ttu-id="8bc1c-121">不過，如果在讀取或傳遞陣列之前，未將新建立之陣列的元素指派給，我們可能會發出警告。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-121">However, we could issue a warning if no element of a newly created array is assigned to before the array is read from or passed on.</span></span> <span data-ttu-id="8bc1c-122">這可能會處理常見的情況，而不會有太多雜訊。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-122">That might handle the common case without being too noisy.</span></span>

<span data-ttu-id="8bc1c-123">我們需要決定 `default(T)` 是否會產生警告，或直接視為 `T?`類型。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-123">We need to decide whether `default(T)` generates a warning, or is simply treated as being of the type `T?`.</span></span>

## <a name="metadata-representation"></a><span data-ttu-id="8bc1c-124">中繼資料標記法</span><span class="sxs-lookup"><span data-stu-id="8bc1c-124">Metadata representation</span></span>

<span data-ttu-id="8bc1c-125">可 null 性裝飾應該在中繼資料中表示為屬性。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-125">Nullability adornments should be represented in metadata as attributes.</span></span> <span data-ttu-id="8bc1c-126">這表示下層編譯器會忽略它們。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-126">This means that downlevel compilers will ignore them.</span></span>

<span data-ttu-id="8bc1c-127">我們需要決定是否只包含可為 null 的注釋，或在元件中也有指出非 null 是否為 "on" 的指示。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-127">We need to decide if only nullable annotations are included, or there's also some indication of whether non-null was "on" in the assembly.</span></span>

## <a name="generics"></a><span data-ttu-id="8bc1c-128">泛型</span><span class="sxs-lookup"><span data-stu-id="8bc1c-128">Generics</span></span>

<span data-ttu-id="8bc1c-129">如果型別參數 `T` 具有不可為 null 的條件約束，則在其範圍內會將其視為不可為 null。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-129">If a type parameter `T` has non-nullable constraints, it is treated as non-nullable within its scope.</span></span>

<span data-ttu-id="8bc1c-130">如果類型參數不受限制，或只有可為 null 的條件約束，則此情況會比較複雜：這表示對應的類型自*變數可以是 nullable 或*不可為 null。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-130">If a type parameter is unconstrained or has only nullable constraints, the situation is a little more complex: this means that the corresponding type argument could be *either* nullable or non-nullable.</span></span> <span data-ttu-id="8bc1c-131">在這種情況下，最安全的做法是將類型參數視為可為 null 和不可為 null，並在違反*任一項時*發出警告。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-131">The safe thing to do in that situation is to treat the type parameter as *both* nullable and non-nullable, giving warnings when either is violated.</span></span> 

<span data-ttu-id="8bc1c-132">值得考慮是否應允許明確的可為 null 參考條件約束。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-132">It is worth considering whether explicit nullable reference constraints should be allowed.</span></span> <span data-ttu-id="8bc1c-133">不過，請注意，在某些情況下，我們無法避免以可為 null 的參考型別*隱含*為條件約束（繼承的條件約束）。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-133">Note, however, that we cannot avoid having nullable reference types *implicitly* be constraints in certain cases (inherited constraints).</span></span>

<span data-ttu-id="8bc1c-134">`class` 條件約束為非 null。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-134">The `class` constraint is non-null.</span></span> <span data-ttu-id="8bc1c-135">我們可以考慮 `class?` 是否應為有效的可為 null 條件約束，表示「可為 null 的參考型別」。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-135">We can consider whether `class?` should be a valid nullable constraint denoting "nullable reference type".</span></span>

## <a name="type-inference"></a><span data-ttu-id="8bc1c-136">型別推斷</span><span class="sxs-lookup"><span data-stu-id="8bc1c-136">Type inference</span></span>

<span data-ttu-id="8bc1c-137">在型別推斷中，如果參與的型別是可為 null 的參考型別，則產生的型別應該是可為 null。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-137">In type inference, if a contributing type is a nullable reference type, the resulting type should be nullable.</span></span> <span data-ttu-id="8bc1c-138">換句話說，null 會傳播。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-138">In other words, nullness is propagated.</span></span>

<span data-ttu-id="8bc1c-139">我們應該考慮 `null` 常值做為參與運算式是否應參與 null。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-139">We should consider whether the `null` literal as a participating expression should contribute nullness.</span></span> <span data-ttu-id="8bc1c-140">今天並不是：對於實值型別，會導致錯誤，而針對參考型別，null 會成功轉換為純文字類型。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-140">It doesn't today: for value types it leads to an error, whereas for reference types the null successfully converts to the plain type.</span></span> 

```csharp
string? n = "world";
var x = b ? "Hello" : n; // string?
var y = b ? "Hello" : null; // string? or error
var z = b ? 7 : null; // Error today, could be int?
```

## <a name="breaking-changes"></a><span data-ttu-id="8bc1c-141">重大變更</span><span class="sxs-lookup"><span data-stu-id="8bc1c-141">Breaking changes</span></span>

<span data-ttu-id="8bc1c-142">非 null 警告是對現有程式碼的明顯重大變更，應該伴隨加入宣告機制。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-142">Non-null warnings are an obvious breaking change on existing code, and should be accompanied with an opt-in mechanism.</span></span>

<span data-ttu-id="8bc1c-143">較不明顯的是，可為 null 的型別發出的警告（如上所述）是對現有程式碼的重大變更，而這些案例中的 null 屬性是隱含的：</span><span class="sxs-lookup"><span data-stu-id="8bc1c-143">Less obviously, warnings from nullable types (as described above) are a breaking change on existing code in certain scenarios where the nullability is implicit:</span></span>

* <span data-ttu-id="8bc1c-144">不受限制的類型參數將會視為隱含可為 null，因此將它們指派給 `object` 或存取，例如 `ToString` 會產生警告。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-144">Unconstrained type parameters will be treated as implicitly nullable, so assigning them to `object` or accessing e.g. `ToString` will yield warnings.</span></span>
* <span data-ttu-id="8bc1c-145">如果類型推斷從 `null` 運算式推斷 null，則現有程式碼有時會產生可為 null 而不是不可為 null 的類型，這可能會導致新的警告。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-145">if type inference infers nullness from `null` expressions, then existing code will sometimes yield nullable rather than non-nullable types, which can lead to new warnings.</span></span>

<span data-ttu-id="8bc1c-146">因此可為 null 的警告也必須是選擇性的</span><span class="sxs-lookup"><span data-stu-id="8bc1c-146">So nullable warnings also need to be optional</span></span>

<span data-ttu-id="8bc1c-147">最後，將批註新增至現有的 API，將會對已選擇警告的使用者進行重大變更（當他們升級程式庫時）。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-147">Finally, adding annotations to an existing API will be a breaking change to users who have opted in to warnings, when they upgrade the library.</span></span> <span data-ttu-id="8bc1c-148">這也是可以加入宣告或退出的功能。「我想要修正 bug，但我尚未準備好處理新批註」</span><span class="sxs-lookup"><span data-stu-id="8bc1c-148">This, too, merits the ability to opt in or out. "I want the bug fixes, but I am not ready to deal with their new annotations"</span></span>

<span data-ttu-id="8bc1c-149">總而言之，您必須能夠加入宣告/退出：</span><span class="sxs-lookup"><span data-stu-id="8bc1c-149">In summary, you need to be able to opt in/out of:</span></span>
* <span data-ttu-id="8bc1c-150">可為 null 的警告</span><span class="sxs-lookup"><span data-stu-id="8bc1c-150">Nullable warnings</span></span>
* <span data-ttu-id="8bc1c-151">非 null 的警告</span><span class="sxs-lookup"><span data-stu-id="8bc1c-151">Non-null warnings</span></span>
* <span data-ttu-id="8bc1c-152">來自其他檔案中批註的警告</span><span class="sxs-lookup"><span data-stu-id="8bc1c-152">Warnings from annotations in other files</span></span>

<span data-ttu-id="8bc1c-153">加入宣告的資料細微性會建議類似分析器的模型，其中程式碼的大量可以加入宣告和登出，使用者可以選擇使用 pragma 和嚴重性層級。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-153">The granularity of the opt-in suggests an analyzer-like model, where swaths of code can opt in and out with pragmas and severity levels can be chosen by the user.</span></span> <span data-ttu-id="8bc1c-154">此外，每個程式庫的選項（「忽略 JSON.NET 中的注釋直到我準備好處理掉」）可能可以在程式碼中表示為屬性。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-154">Additionally, per-library options ("ignore the annotations from JSON.NET until I'm ready to deal with the fall out") may be expressible in code as attributes.</span></span>

<span data-ttu-id="8bc1c-155">加入宣告/轉換體驗的設計對於這項功能的成功和實用性非常重要。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-155">The design of the opt-in/transition experience is crucial to the success and usefulness of this feature.</span></span> <span data-ttu-id="8bc1c-156">我們需要確保：</span><span class="sxs-lookup"><span data-stu-id="8bc1c-156">We need to make sure that:</span></span>

* <span data-ttu-id="8bc1c-157">使用者可以依想要的方式逐漸採用 null 屬性檢查</span><span class="sxs-lookup"><span data-stu-id="8bc1c-157">Users can adopt nullability checking gradually as they want to</span></span>
* <span data-ttu-id="8bc1c-158">程式庫作者可以新增 null 屬性注釋，而不必擔心中斷的客戶</span><span class="sxs-lookup"><span data-stu-id="8bc1c-158">Library authors can add nullability annotations without fear of breaking customers</span></span>
* <span data-ttu-id="8bc1c-159">儘管如此，「設定的麻煩」也沒有意義</span><span class="sxs-lookup"><span data-stu-id="8bc1c-159">Despite these, there is not a sense of "configuration nightmare"</span></span>

## <a name="tweaks"></a><span data-ttu-id="8bc1c-160">微調</span><span class="sxs-lookup"><span data-stu-id="8bc1c-160">Tweaks</span></span>

<span data-ttu-id="8bc1c-161">我們可以考慮不要在區域變數上使用 `?` 的注釋，而只是要觀察是否要根據指派給它們的專案來使用它們。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-161">We could consider not using the `?` annotations on locals, but just observing whether they are used in accordance with what gets assigned to them.</span></span> <span data-ttu-id="8bc1c-162">我不喜歡這麼做;我認為我們應該一致地讓人們表達其意圖。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-162">I don't favor this; I think we should uniformly let people express their intent.</span></span>

<span data-ttu-id="8bc1c-163">我們可以在參數上考慮使用速記 `T! x`，這會自動產生執行時間 null 檢查。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-163">We could consider a shorthand `T! x` on parameters, that auto-generates a runtime null check.</span></span>

<span data-ttu-id="8bc1c-164">泛型型別的特定模式（例如 `FirstOrDefault` 或 `TryGet`）對不可為 null 的型別引數有些許的行為，因為在某些情況下，它們會明確產生預設值。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-164">Certain patterns on generic types, such as `FirstOrDefault` or `TryGet`, have slightly weird behavior with non-nullable type arguments, because they explicitly yield default values in certain situations.</span></span> <span data-ttu-id="8bc1c-165">我們可以試著將類型系統的細微之處，納入更好的效能。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-165">We could try to nuance the type system to accommodate these better.</span></span> <span data-ttu-id="8bc1c-166">例如，即使類型引數可能已經是可為 null，我們還是可以允許不受限制的類型參數 `?`。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-166">For instance, we could allow `?` on unconstrained type parameters, even though the type argument could already be nullable.</span></span> <span data-ttu-id="8bc1c-167">我不確定它是值得的，而且它會導致與可為 null 的實*值*型別互動相關的奇怪。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-167">I doubt that it is worth it, and it leads to weirdness related to interaction with nullable *value* types.</span></span> 

## <a name="nullable-value-types"></a><span data-ttu-id="8bc1c-168">可為 Null 的實值型別</span><span class="sxs-lookup"><span data-stu-id="8bc1c-168">Nullable value types</span></span>

<span data-ttu-id="8bc1c-169">我們也可以考慮針對可為 null 的實值型別採用上述的某些語義。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-169">We could consider adopting some of the above semantics for nullable value types as well.</span></span>

<span data-ttu-id="8bc1c-170">我們已提到型別推斷，我們可以從 `(7, null)`推斷 `int?`，而不只是產生錯誤。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-170">We already mentioned type inference, where we could infer `int?` from `(7, null)`, instead of just giving an error.</span></span>

<span data-ttu-id="8bc1c-171">另一個機會是將流程分析套用至可為 null 的實數值型別。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-171">Another opportunity is to apply the flow analysis to nullable value types.</span></span> <span data-ttu-id="8bc1c-172">當它們被視為非 null 時，我們實際上可以允許以特定方式（例如成員存取），以不可為 null 的型別來使用。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-172">When they are deemed non-null, we could actually allow using as the non-nullable type in certain ways (e.g. member access).</span></span> <span data-ttu-id="8bc1c-173">我們只需要特別注意，您可以在可為 null 的實值型別*上做的*事情，基於回溯相容性的理由，將是慣用的。</span><span class="sxs-lookup"><span data-stu-id="8bc1c-173">We just have to be careful that the things that you can *already* do on a nullable value type will be preferred, for back compat reasons.</span></span>
