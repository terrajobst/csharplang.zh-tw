---
ms.openlocfilehash: 6088a5cd41926d828013f1b8e5736fd2b7939e44
ms.sourcegitcommit: da452002c3f472165a0e1fa7759f494cc703ae31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "79485039"
---
# <a name="compile-time-enforcement-of-safety-for-ref-like-types"></a><span data-ttu-id="c73cd-101">類似 ref 類型的編譯時間強制執行安全性</span><span class="sxs-lookup"><span data-stu-id="c73cd-101">Compile time enforcement of safety for ref-like types</span></span>

## <a name="introduction"></a><span data-ttu-id="c73cd-102">簡介</span><span class="sxs-lookup"><span data-stu-id="c73cd-102">Introduction</span></span>

<span data-ttu-id="c73cd-103">處理 `Span<T>` 和 `ReadOnlySpan<T>` 等類型時，其他安全性規則的主要原因是，這類類型必須限制在執行堆疊中。</span><span class="sxs-lookup"><span data-stu-id="c73cd-103">The main reason for the additional safety rules when dealing with types like `Span<T>` and `ReadOnlySpan<T>` is that such types must be confined to the execution stack.</span></span>
 
<span data-ttu-id="c73cd-104">有兩個原因： `Span<T>` 和類似的類型必須是僅限堆疊的類型。</span><span class="sxs-lookup"><span data-stu-id="c73cd-104">There are two reasons why `Span<T>` and similar types must be a stack-only types.</span></span>

1. <span data-ttu-id="c73cd-105">`Span<T>` 在語義上是包含參考和範圍 `(ref T data, int length)`的結構。</span><span class="sxs-lookup"><span data-stu-id="c73cd-105">`Span<T>` is semantically a struct containing a reference and a range - `(ref T data, int length)`.</span></span> <span data-ttu-id="c73cd-106">不論實際的實值為何，對這類結構的寫入都不是不可部分完成的。</span><span class="sxs-lookup"><span data-stu-id="c73cd-106">Regardless of actual implementation, writes to such struct would not be atomic.</span></span> <span data-ttu-id="c73cd-107">這類結構的並行「撕裂」可能會導致 `length` 不符合 `data`，因而導致超出範圍的存取和型別安全違規，最終可能會導致在看似「安全」的程式碼中發生 GC 堆積損毀。</span><span class="sxs-lookup"><span data-stu-id="c73cd-107">Concurrent "tearing" of such struct would lead to the possibility of `length` not matching the `data`, causing out-of-range accesses and type-safety violations, which ultimately could result in GC heap corruption in seemingly "safe" code.</span></span>
2. <span data-ttu-id="c73cd-108">某些 `Span<T>` 的執行實際上會在其中一個欄位中包含 managed 指標。</span><span class="sxs-lookup"><span data-stu-id="c73cd-108">Some implementations of `Span<T>` literally contain a managed pointer in one of its fields.</span></span> <span data-ttu-id="c73cd-109">Managed 指標不支援做為堆積物件的欄位，以及管理將 managed 指標放在 GC 堆積上的程式碼，通常會在 JIT 時損毀。</span><span class="sxs-lookup"><span data-stu-id="c73cd-109">Managed pointers are not supported as fields of heap objects and code that manages to put a managed pointer on the GC heap typically crashes at JIT time.</span></span>

<span data-ttu-id="c73cd-110">如果 `Span<T>` 的實例被限制為只存在於執行堆疊上，則上述所有問題都可以緩解。</span><span class="sxs-lookup"><span data-stu-id="c73cd-110">All the above problems would be alleviated if instances of `Span<T>` are constrained to exist only on the execution stack.</span></span> 

<span data-ttu-id="c73cd-111">造成其他問題的原因是組合。</span><span class="sxs-lookup"><span data-stu-id="c73cd-111">An additional problem arises due to composition.</span></span> <span data-ttu-id="c73cd-112">通常最好是建立更複雜的資料類型，以內嵌 `Span<T>` 和 `ReadOnlySpan<T>` 實例。</span><span class="sxs-lookup"><span data-stu-id="c73cd-112">It would be generally desirable to build more complex data types that would embed `Span<T>` and `ReadOnlySpan<T>` instances.</span></span> <span data-ttu-id="c73cd-113">這類複合類型必須是結構，而且會共用 `Span<T>`的所有風險和需求。</span><span class="sxs-lookup"><span data-stu-id="c73cd-113">Such composite types would have to be structs and would share all the hazards and requirements of `Span<T>`.</span></span> <span data-ttu-id="c73cd-114">因此，此處所述的安全性規則應視為適用于 **_類似 ref 類型_** 的完整範圍。</span><span class="sxs-lookup"><span data-stu-id="c73cd-114">As a result the safety rules described here should be viewed as applicable to the whole range of **_ref-like types_**.</span></span>

<span data-ttu-id="c73cd-115">[草稿語言規格](#draft-language-specification)的目的是要確保類似 ref 類型的值只會出現在堆疊上。</span><span class="sxs-lookup"><span data-stu-id="c73cd-115">The [draft language specification](#draft-language-specification) is intended to ensure that values of a ref-like type occurs only on the stack.</span></span>

## <a name="generalized-ref-like-types-in-source-code"></a><span data-ttu-id="c73cd-116">原始碼中的一般化 `ref-like` 類型</span><span class="sxs-lookup"><span data-stu-id="c73cd-116">Generalized `ref-like` types in source code</span></span>

<span data-ttu-id="c73cd-117">`ref-like` 結構會使用 `ref` 修飾詞，在原始程式碼中明確標示：</span><span class="sxs-lookup"><span data-stu-id="c73cd-117">`ref-like` structs are explicitly marked in the source code using `ref` modifier:</span></span>

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

<span data-ttu-id="c73cd-118">將結構指定為類似參考，可讓結構擁有類似 ref 的實例欄位，也會讓類似 ref 類型的所有需求都適用于結構。</span><span class="sxs-lookup"><span data-stu-id="c73cd-118">Designating a struct as ref-like will allow the struct to have ref-like instance fields and will also make all the requirements of ref-like types applicable to the struct.</span></span> 

## <a name="metadata-representation-or-ref-like-structs"></a><span data-ttu-id="c73cd-119">中繼資料標記法或類似參考的結構</span><span class="sxs-lookup"><span data-stu-id="c73cd-119">Metadata representation or ref-like structs</span></span>

<span data-ttu-id="c73cd-120">類似 Ref 的結構將會以**CompilerServices. IsRefLikeAttribute**屬性標記。</span><span class="sxs-lookup"><span data-stu-id="c73cd-120">Ref-like structs will be marked with **System.Runtime.CompilerServices.IsRefLikeAttribute** attribute.</span></span>

<span data-ttu-id="c73cd-121">屬性會加入至一般基底程式庫，例如 `mscorlib`。</span><span class="sxs-lookup"><span data-stu-id="c73cd-121">The attribute will be added to common base libraries such as `mscorlib`.</span></span> <span data-ttu-id="c73cd-122">在無法使用屬性的情況下，編譯器會產生與其他內嵌隨選屬性（如 `IsReadOnlyAttribute`）類似的內部。</span><span class="sxs-lookup"><span data-stu-id="c73cd-122">In a case if the attribute is not available, compiler will generate an internal one similarly to other embedded-on-demand attributes such as `IsReadOnlyAttribute`.</span></span>

<span data-ttu-id="c73cd-123">系統會採用額外的量值，以防止在編譯器中使用類似 ref 的結構，而不熟悉安全性規則（這C#包括執行這項功能之前的編譯器）。</span><span class="sxs-lookup"><span data-stu-id="c73cd-123">An additional measure will be taken to prevent the use of ref-like structs in compilers not familiar with the safety rules (this includes C# compilers prior to the one in which this feature is implemented).</span></span> 

<span data-ttu-id="c73cd-124">沒有其他可在舊編譯器中使用但未提供服務的其他好方法，將會在所有 ref like 結構中加入具有已知字串的 `Obsolete` 屬性。</span><span class="sxs-lookup"><span data-stu-id="c73cd-124">Having no other good alternatives that work in old compilers without servicing, an `Obsolete` attribute with a known string will be added to all ref-like structs.</span></span> <span data-ttu-id="c73cd-125">知道如何使用類似 ref 類型的編譯器將會忽略這種特定形式的 `Obsolete`。</span><span class="sxs-lookup"><span data-stu-id="c73cd-125">Compilers that know how to use ref-like types will ignore this particular form of `Obsolete`.</span></span>

<span data-ttu-id="c73cd-126">典型的中繼資料標記法：</span><span class="sxs-lookup"><span data-stu-id="c73cd-126">A typical metadata representation:</span></span>

```csharp
    [IsRefLike]
    [Obsolete("Types with embedded references are not supported in this version of your compiler.")]
    public struct TwoSpans<T>
    {
       // . . . .
    }
```

<span data-ttu-id="c73cd-127">注意：這並不是為了讓舊版編譯器上的類似 ref 型別使用的目標失敗100%。</span><span class="sxs-lookup"><span data-stu-id="c73cd-127">NOTE: it is not the goal to make it so that any use of ref-like types on old compilers fails 100%.</span></span> <span data-ttu-id="c73cd-128">這很難達成，而且不是絕對必要的。</span><span class="sxs-lookup"><span data-stu-id="c73cd-128">That is hard to achieve and is not strictly necessary.</span></span> <span data-ttu-id="c73cd-129">例如，一律會有方法可以使用動態程式碼來避開 `Obsolete`，例如透過反映建立類似 ref 類型的陣列。</span><span class="sxs-lookup"><span data-stu-id="c73cd-129">For example there would always be a way to get around the `Obsolete` using dynamic code or, for example, creating an array of ref-like types through reflection.</span></span>

<span data-ttu-id="c73cd-130">特別是，如果使用者想要實際將 `Obsolete` 或 `Deprecated` 屬性放在類似 ref 的型別上，我們將不會發出預先定義的選項，因為 `Obsolete` 屬性無法套用一次以上。</span><span class="sxs-lookup"><span data-stu-id="c73cd-130">In particular, if user wants to actually put an `Obsolete` or `Deprecated` attribute on a ref-like type, we will have no choice other than not emitting the predefined one since `Obsolete` attribute cannot be applied more than once..</span></span>  

## <a name="examples"></a><span data-ttu-id="c73cd-131">範例：</span><span class="sxs-lookup"><span data-stu-id="c73cd-131">Examples:</span></span>

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

## <a name="draft-language-specification"></a><span data-ttu-id="c73cd-132">草稿語言規格</span><span class="sxs-lookup"><span data-stu-id="c73cd-132">Draft language specification</span></span>

<span data-ttu-id="c73cd-133">下面我們會針對類似 ref 的類型（`ref struct`s）描述一組安全性規則，以確保這些類型的值只會出現在堆疊上。</span><span class="sxs-lookup"><span data-stu-id="c73cd-133">Below we describe a set of safety rules for ref-like types (`ref struct`s) to ensure that values of these types occur only on the stack.</span></span> <span data-ttu-id="c73cd-134">如果區域變數無法以傳址方式傳遞，則可能會有不同、較簡單的安全性規則。</span><span class="sxs-lookup"><span data-stu-id="c73cd-134">A different, simpler set of safety rules would be possible if locals cannot be passed by reference.</span></span> <span data-ttu-id="c73cd-135">此規格也允許重新指派 ref 區域變數的安全。</span><span class="sxs-lookup"><span data-stu-id="c73cd-135">This specification would also permit the safe reassignment of ref locals.</span></span>

### <a name="overview"></a><span data-ttu-id="c73cd-136">概觀</span><span class="sxs-lookup"><span data-stu-id="c73cd-136">Overview</span></span>

<span data-ttu-id="c73cd-137">在編譯時期，我們會將與每個運算式相關聯的概念，是運算式允許的範圍，也就是「安全到 escape」。</span><span class="sxs-lookup"><span data-stu-id="c73cd-137">We associate with each expression at compile-time the concept of what scope that expression is permitted to escape to, "safe-to-escape".</span></span> <span data-ttu-id="c73cd-138">同樣地，對於每個左值，我們都會維護一個概念，說明允許其參考的範圍（「ref-安全到 escape」）。</span><span class="sxs-lookup"><span data-stu-id="c73cd-138">Similarly, for each lvalue we maintain a concept of what scope a reference to it is permitted to escape to, "ref-safe-to-escape".</span></span> <span data-ttu-id="c73cd-139">針對指定的左值運算式，這些可能會不同。</span><span class="sxs-lookup"><span data-stu-id="c73cd-139">For a given lvalue expression, these may be different.</span></span>

<span data-ttu-id="c73cd-140">這些類似 ref 區域變數功能的「安全傳回」，但更精細。</span><span class="sxs-lookup"><span data-stu-id="c73cd-140">These are analogous to the "safe to return" of the ref locals feature, but it is more fine-grained.</span></span> <span data-ttu-id="c73cd-141">運算式的「安全轉型」只會記錄（或不是）它是否可以將封入方法完全以完整的方式來進行封入，此安全轉型會記錄它可能會被跳到的範圍（它可能不會被 escape 的範圍）。</span><span class="sxs-lookup"><span data-stu-id="c73cd-141">Where the "safe-to-return" of an expression records only whether (or not) it may escape the enclosing method as a whole, the safe-to-escape records which scope it may escape to (which scope it may not escape beyond).</span></span> <span data-ttu-id="c73cd-142">基本的安全機制會強制執行，如下所示。</span><span class="sxs-lookup"><span data-stu-id="c73cd-142">The basic safety mechanism is enforced as follows.</span></span> <span data-ttu-id="c73cd-143">假設從具有安全對 escape 範圍 S1 的運算式 E1 指派到（左值）運算式，並具有安全對 escape 範圍 S2，如果 S2 的範圍超出 S1，則會產生錯誤。</span><span class="sxs-lookup"><span data-stu-id="c73cd-143">Given an assignment from an expression E1 with a safe-to-escape scope S1, to an (lvalue) expression E2 with safe-to-escape scope S2, it is an error if S2 is a wider scope than S1.</span></span> <span data-ttu-id="c73cd-144">根據結構，S1 和 S2 這兩個範圍是在一個嵌套關聯性中，因為合法的運算式一律會從包含該運算式的某個範圍內恢復安全。</span><span class="sxs-lookup"><span data-stu-id="c73cd-144">By construction, the two scopes S1 and S2 are in a nesting relationship, because a legal expression is always safe-to-return from some scope enclosing the expression.</span></span>

<span data-ttu-id="c73cd-145">在這段時間內，為了分析的目的，只支援兩個範圍-外部至方法，以及方法的最上層範圍。</span><span class="sxs-lookup"><span data-stu-id="c73cd-145">For the time being it is sufficient, for the purpose of the analysis, to support just two scopes - external to the method, and top-level scope of the method.</span></span> <span data-ttu-id="c73cd-146">這是因為無法建立具有內部範圍的 ref like 值，而且 ref 區域變數不支援重新指派。</span><span class="sxs-lookup"><span data-stu-id="c73cd-146">That is because ref-like values with inner scopes cannot be created and ref locals do not support re-assignment.</span></span> <span data-ttu-id="c73cd-147">不過，這些規則可支援兩個以上的範圍層級。</span><span class="sxs-lookup"><span data-stu-id="c73cd-147">The rules, however, can support more than two scope levels.</span></span>

<span data-ttu-id="c73cd-148">計算運算式之*安全回復*狀態的精確規則，以及管理運算式之合法性的規則，請遵循。</span><span class="sxs-lookup"><span data-stu-id="c73cd-148">The precise rules for computing the *safe-to-return* status of an expression, and the rules governing the legality of expressions, follow.</span></span>

### <a name="ref-safe-to-escape"></a><span data-ttu-id="c73cd-149">ref-安全到 escape</span><span class="sxs-lookup"><span data-stu-id="c73cd-149">ref-safe-to-escape</span></span>

<span data-ttu-id="c73cd-150">「 *Ref-安全轉型*」是一種範圍，其中包含左值運算式，可安全地供左值的 ref 進行換用。</span><span class="sxs-lookup"><span data-stu-id="c73cd-150">The *ref-safe-to-escape* is a scope, enclosing an lvalue expression, to which it is safe for a ref to the lvalue to escape to.</span></span> <span data-ttu-id="c73cd-151">如果該範圍是整個方法，我們會假設左值的 ref 可*安全地*從方法傳回。</span><span class="sxs-lookup"><span data-stu-id="c73cd-151">If that scope is the entire method, we say that a ref to the lvalue is *safe to return* from the method.</span></span>

### <a name="safe-to-escape"></a><span data-ttu-id="c73cd-152">安全對 escape</span><span class="sxs-lookup"><span data-stu-id="c73cd-152">safe-to-escape</span></span>

<span data-ttu-id="c73cd-153">「*安全轉型*」是一種範圍，其中包含一個運算式，可安全地將此值換成。</span><span class="sxs-lookup"><span data-stu-id="c73cd-153">The *safe-to-escape* is a scope, enclosing an expression, to which it is safe for the value to escape to.</span></span> <span data-ttu-id="c73cd-154">如果該範圍是整個方法，我們會說，此值可*安全地*從方法傳回。</span><span class="sxs-lookup"><span data-stu-id="c73cd-154">If that scope is the entire method, we say that a the value is *safe to return* from the method.</span></span>

<span data-ttu-id="c73cd-155">其類型不是 `ref struct` 類型的運算式，可從整個封入方法中*安全地*傳回。</span><span class="sxs-lookup"><span data-stu-id="c73cd-155">An expression whose type is not a `ref struct` type is *safe-to-return* from the entire enclosing method.</span></span> <span data-ttu-id="c73cd-156">否則，我們會參考下列規則。</span><span class="sxs-lookup"><span data-stu-id="c73cd-156">Otherwise we refer to the rules below.</span></span>

#### <a name="parameters"></a><span data-ttu-id="c73cd-157">參數</span><span class="sxs-lookup"><span data-stu-id="c73cd-157">Parameters</span></span>

<span data-ttu-id="c73cd-158">指定型式參數的左值是*ref-安全到 escape* （依參考），如下所示：</span><span class="sxs-lookup"><span data-stu-id="c73cd-158">An lvalue designating a formal parameter is *ref-safe-to-escape* (by reference) as follows:</span></span>
- <span data-ttu-id="c73cd-159">如果參數是 `ref`、`out`或 `in` 參數，則從整個方法（例如，透過 `return ref` 語句），它是*ref-safe to escape* ;別的</span><span class="sxs-lookup"><span data-stu-id="c73cd-159">If the parameter is a `ref`, `out`, or `in` parameter, it is *ref-safe-to-escape* from the entire method (e.g. by a `return ref` statement); otherwise</span></span>
- <span data-ttu-id="c73cd-160">如果參數是結構類型的 `this` 參數，則它是對方法的最上層範圍（但不是從整個方法本身）進行的*ref-安全*轉型。[範例](#struct-this-escape)</span><span class="sxs-lookup"><span data-stu-id="c73cd-160">If the parameter is the `this` parameter of a struct type, it is *ref-safe-to-escape* to the top-level scope of the method (but not from the entire method itself); [Sample](#struct-this-escape)</span></span>
- <span data-ttu-id="c73cd-161">否則，參數會是值參數，而且它是對方法的最上層範圍（但不是從方法本身）進行*ref-安全*轉型。</span><span class="sxs-lookup"><span data-stu-id="c73cd-161">Otherwise the parameter is a value parameter, and it is *ref-safe-to-escape* to the top-level scope of the method (but not from the method itself).</span></span>

<span data-ttu-id="c73cd-162">表示使用型式參數的運算式，是從整個方法（例如，透過 `return` 語句）的*安全對 escape* （藉由值）。</span><span class="sxs-lookup"><span data-stu-id="c73cd-162">An expression that is an rvalue designating the use of a formal parameter is *safe-to-escape* (by value) from the entire method (e.g. by a `return` statement).</span></span> <span data-ttu-id="c73cd-163">這也適用于 `this` 參數。</span><span class="sxs-lookup"><span data-stu-id="c73cd-163">This applies to the `this` parameter as well.</span></span>

#### <a name="locals"></a><span data-ttu-id="c73cd-164">區域變數</span><span class="sxs-lookup"><span data-stu-id="c73cd-164">Locals</span></span>

<span data-ttu-id="c73cd-165">指定區域變數的左值是*ref-安全對 escape （以*傳址方式），如下所示：</span><span class="sxs-lookup"><span data-stu-id="c73cd-165">An lvalue designating a local variable is *ref-safe-to-escape* (by reference) as follows:</span></span>
- <span data-ttu-id="c73cd-166">如果變數是 `ref` 變數，則會從其初始化運算式的*ref 安全轉轉義來*取得其*ref-安全到 escape* 。別的</span><span class="sxs-lookup"><span data-stu-id="c73cd-166">If the variable is a `ref` variable, then its *ref-safe-to-escape* is taken from the *ref-safe-to-escape* of its initializing expression; otherwise</span></span>
- <span data-ttu-id="c73cd-167">變數是 ref 安全的，會將其宣告所在的範圍*設為 escape* 。</span><span class="sxs-lookup"><span data-stu-id="c73cd-167">The variable is *ref-safe-to-escape* the scope in which it was declared.</span></span>

<span data-ttu-id="c73cd-168">為右值的運算式，指定區域變數的使用方式，是*安全對 escape* （以傳值方式），如下所示：</span><span class="sxs-lookup"><span data-stu-id="c73cd-168">An expression that is an rvalue designating the use of a local variable is *safe-to-escape* (by value) as follows:</span></span>
- <span data-ttu-id="c73cd-169">但是上述的一般規則，其類型不是 `ref struct` 類型的本機，則是從整個封入方法中*安全地*傳回。</span><span class="sxs-lookup"><span data-stu-id="c73cd-169">But the general rule above, a local whose type is not a `ref struct` type is *safe-to-return* from the entire enclosing method.</span></span>
- <span data-ttu-id="c73cd-170">如果變數是 `foreach` 迴圈的反復專案變數，則變數的*安全到 escape*範圍會與 `foreach` 迴圈運算式的*安全對 escape*相同。</span><span class="sxs-lookup"><span data-stu-id="c73cd-170">If the variable is an iteration variable of a `foreach` loop, then the variable's *safe-to-escape* scope is the same as the *safe-to-escape* of the `foreach` loop's expression.</span></span>
- <span data-ttu-id="c73cd-171">`ref struct` 類型的本機，而且在宣告點未初始化的區域，會從整個封入方法中*安全地*傳回。</span><span class="sxs-lookup"><span data-stu-id="c73cd-171">A local of `ref struct` type and uninitialized at the point of declaration is *safe-to-return* from the entire enclosing method.</span></span>
- <span data-ttu-id="c73cd-172">否則，變數的類型會是 `ref struct` 類型，而變數的宣告則需要初始化運算式。</span><span class="sxs-lookup"><span data-stu-id="c73cd-172">Otherwise the variable's type is a `ref struct` type, and the variable's declaration requires an initializer.</span></span> <span data-ttu-id="c73cd-173">變數的*安全到 escape*範圍與其初始化運算式的*安全對 escape*是相同的。</span><span class="sxs-lookup"><span data-stu-id="c73cd-173">The variable's *safe-to-escape* scope is the same as the *safe-to-escape* of its initializer.</span></span>

#### <a name="field-reference"></a><span data-ttu-id="c73cd-174">欄位參考</span><span class="sxs-lookup"><span data-stu-id="c73cd-174">Field reference</span></span>

<span data-ttu-id="c73cd-175">左值，指定對欄位的參考（`e.F`）是*ref-安全對 escape* （依參考），如下所示：</span><span class="sxs-lookup"><span data-stu-id="c73cd-175">An lvalue designating a reference to a field, `e.F`, is *ref-safe-to-escape* (by reference) as follows:</span></span>
- <span data-ttu-id="c73cd-176">如果 `e` 是參考型別，則會從整個方法中*以 ref 安全*的方式進行 escape;別的</span><span class="sxs-lookup"><span data-stu-id="c73cd-176">If `e` is of a reference type, it is *ref-safe-to-escape* from the entire method; otherwise</span></span>
- <span data-ttu-id="c73cd-177">如果 `e` 是實值型別，則會從 `e`的*ref 安全到 escape* ，取得其*ref 安全到 escape* 。</span><span class="sxs-lookup"><span data-stu-id="c73cd-177">If `e` is of a value type, its *ref-safe-to-escape* is taken from the *ref-safe-to-escape* of `e`.</span></span>

<span data-ttu-id="c73cd-178">指定欄位（`e.F`）之參考的右值，具有與 `e`的*安全到 escape*相同的*安全轉轉義*範圍。</span><span class="sxs-lookup"><span data-stu-id="c73cd-178">An rvalue designating a reference to a field, `e.F`, has a *safe-to-escape* scope that is the same as the *safe-to-escape* of `e`.</span></span>

#### <a name="operators-including-"></a><span data-ttu-id="c73cd-179">包括 `?:` 的運算子</span><span class="sxs-lookup"><span data-stu-id="c73cd-179">Operators including `?:`</span></span>

<span data-ttu-id="c73cd-180">使用者定義運算子的應用會被視為方法調用。</span><span class="sxs-lookup"><span data-stu-id="c73cd-180">The application of a user-defined operator is treated as a method invocation.</span></span>

<span data-ttu-id="c73cd-181">針對產生右值的運算子（例如 `e1 + e2` 或 `c ? e1 : e2`），結果的*安全對 escape*是運算子之運算元的*安全轉轉義*之間最少的範圍。</span><span class="sxs-lookup"><span data-stu-id="c73cd-181">For an operator that yields an rvalue, such as `e1 + e2` or `c ? e1 : e2`, the *safe-to-escape* of the result is the narrowest scope among the *safe-to-escape* of the operands of the operator.</span></span>  <span data-ttu-id="c73cd-182">因此，如果是會產生右值的一元運算子，例如 `+e`，則結果的*安全對 escape*是運算元的*安全*轉型。</span><span class="sxs-lookup"><span data-stu-id="c73cd-182">As a consequence, for a unary operator that yields an rvalue, such as `+e`, the *safe-to-escape* of the result is the *safe-to-escape* of the operand.</span></span>

<span data-ttu-id="c73cd-183">適用于產生左值的運算子，例如 `c ? ref e1 : ref e2`</span><span class="sxs-lookup"><span data-stu-id="c73cd-183">For an operator that yields an lvalue, such as `c ? ref e1 : ref e2`</span></span>
- <span data-ttu-id="c73cd-184">結果的*ref 安全 to escape*是運算子之運算元的*ref 安全對 escape*之間最小的範圍。</span><span class="sxs-lookup"><span data-stu-id="c73cd-184">the *ref-safe-to-escape* of the result is the narrowest scope among the *ref-safe-to-escape* of the operands of the operator.</span></span>
- <span data-ttu-id="c73cd-185">這些運算元的*安全轉轉義*必須同意，這是產生之左值的*安全對 escape* 。</span><span class="sxs-lookup"><span data-stu-id="c73cd-185">the *safe-to-escape* of the operands must agree, and that is the *safe-to-escape* of the resulting lvalue.</span></span>

#### <a name="method-invocation"></a><span data-ttu-id="c73cd-186">方法引動過程</span><span class="sxs-lookup"><span data-stu-id="c73cd-186">Method invocation</span></span>

<span data-ttu-id="c73cd-187">由 ref 傳回方法叫用所產生的左值，`e1.M(e2, ...)` 是*ref-安全地*將下列範圍的最小值轉義：</span><span class="sxs-lookup"><span data-stu-id="c73cd-187">An lvalue resulting from a ref-returning method invocation `e1.M(e2, ...)` is *ref-safe-to-escape* the smallest of the following scopes:</span></span>
- <span data-ttu-id="c73cd-188">整個封入方法</span><span class="sxs-lookup"><span data-stu-id="c73cd-188">The entire enclosing method</span></span>
- <span data-ttu-id="c73cd-189">所有 `ref` 和 `out` 引數運算式的*ref-安全-escape* （接收者除外）</span><span class="sxs-lookup"><span data-stu-id="c73cd-189">the *ref-safe-to-escape* of all `ref` and `out` argument expressions (excluding the receiver)</span></span>
- <span data-ttu-id="c73cd-190">針對方法的每個 `in` 參數，如果有對應的運算式為左值，則為其*ref-安全到 escape*，否則為最接近的封入範圍</span><span class="sxs-lookup"><span data-stu-id="c73cd-190">For each `in` parameter of the method, if there is a corresponding expression that is an lvalue, its *ref-safe-to-escape*, otherwise the nearest enclosing scope</span></span>
- <span data-ttu-id="c73cd-191">所有引數運算式的*安全對 escape* （包括接收者）</span><span class="sxs-lookup"><span data-stu-id="c73cd-191">the *safe-to-escape* of all argument expressions (including the receiver)</span></span>

> <span data-ttu-id="c73cd-192">注意：必須要有最後一個專案符號，才能處理常式代碼，例如</span><span class="sxs-lookup"><span data-stu-id="c73cd-192">Note: the last bullet is necessary to handle code such as</span></span>
> ```csharp
> var sp = new Span(...)
> return ref sp[0];
> ```
> <span data-ttu-id="c73cd-193">或</span><span class="sxs-lookup"><span data-stu-id="c73cd-193">or</span></span>
> ```csharp
> return ref M(sp, 0);
> ```

<span data-ttu-id="c73cd-194">從方法叫用所產生的右值 `e1.M(e2, ...)` 可從下列最小的範圍中獲得*安全的轉義*：</span><span class="sxs-lookup"><span data-stu-id="c73cd-194">An rvalue resulting from a method invocation `e1.M(e2, ...)` is *safe-to-escape* from the smallest of the following scopes:</span></span>
- <span data-ttu-id="c73cd-195">整個封入方法</span><span class="sxs-lookup"><span data-stu-id="c73cd-195">The entire enclosing method</span></span>
- <span data-ttu-id="c73cd-196">所有引數運算式的*安全對 escape* （包括接收者）</span><span class="sxs-lookup"><span data-stu-id="c73cd-196">the *safe-to-escape* of all argument expressions (including the receiver)</span></span>

#### <a name="an-rvalue"></a><span data-ttu-id="c73cd-197">右值</span><span class="sxs-lookup"><span data-stu-id="c73cd-197">An Rvalue</span></span>
<span data-ttu-id="c73cd-198">右值是從最接近的封入範圍進行*ref-安全到 escape* 。</span><span class="sxs-lookup"><span data-stu-id="c73cd-198">An rvalue is *ref-safe-to-escape* from the nearest enclosing scope.</span></span> <span data-ttu-id="c73cd-199">這種情況發生在類似 `M(ref d.Length)` 的調用中，例如，`d` 的類型 `dynamic`。</span><span class="sxs-lookup"><span data-stu-id="c73cd-199">This occurs for example in an invocation such as `M(ref d.Length)` where `d` is of type `dynamic`.</span></span> <span data-ttu-id="c73cd-200">它也會與（也可能是 subsumes）我們處理對應至 `in` 參數的引數一致。 \*</span><span class="sxs-lookup"><span data-stu-id="c73cd-200">It is also consistent with (and perhaps subsumes) our handling of arguments corresponding to `in` parameters.\*</span></span>

#### <a name="property-invocations"></a><span data-ttu-id="c73cd-201">屬性調用</span><span class="sxs-lookup"><span data-stu-id="c73cd-201">Property invocations</span></span>

<span data-ttu-id="c73cd-202">屬性調用（`get` 或 `set`）會被視為上述規則的基礎方法的方法調用。</span><span class="sxs-lookup"><span data-stu-id="c73cd-202">A property invocation (either `get` or `set`) it treated as a method invocation of the underlying method by the above rules.</span></span>

#### `stackalloc`

<span data-ttu-id="c73cd-203">Stackalloc 運算式是可*安全地*對方法之最上層範圍（而不是從整個方法本身）進行轉義的右值。</span><span class="sxs-lookup"><span data-stu-id="c73cd-203">A stackalloc expression is an rvalue that is *safe-to-escape* to the top-level scope of the method (but not from the entire method itself).</span></span>

#### <a name="constructor-invocations"></a><span data-ttu-id="c73cd-204">函式呼叫</span><span class="sxs-lookup"><span data-stu-id="c73cd-204">Constructor invocations</span></span>

<span data-ttu-id="c73cd-205">叫用遵守函式的 `new` 運算式，與視為傳回所要建立之類型的方法調用的規則相同。</span><span class="sxs-lookup"><span data-stu-id="c73cd-205">A `new` expression that invokes a constructor obeys the same rules as a method invocation that is considered to return the type being constructed.</span></span>

<span data-ttu-id="c73cd-206">此外，如果有初始化運算式，則不會以遞迴方式，在物件初始化運算式運算式的所有引數/運算元的最小範圍內，不能有更多*的安全對* *escape* 。</span><span class="sxs-lookup"><span data-stu-id="c73cd-206">In addition *safe-to-escape* is no wider than the smallest of the *safe-to-escape* of all arguments/operands of the object initializer expressions, recursively, if initializer is present.</span></span> 

#### <a name="span-constructor"></a><span data-ttu-id="c73cd-207">Span 構造函式</span><span class="sxs-lookup"><span data-stu-id="c73cd-207">Span constructor</span></span>
<span data-ttu-id="c73cd-208">語言依賴 `Span<T>` 不具有下列格式的函式：</span><span class="sxs-lookup"><span data-stu-id="c73cd-208">The language relies on `Span<T>` not having a constructor of the following form:</span></span>

```csharp
void Example(ref int x)
{
    // Create a span of length one
    var span = new Span<int>(ref x); 
}
```

<span data-ttu-id="c73cd-209">這類的「函式」會使 `Span<T>` 用來做為與 `ref` 欄位不區分的欄位。</span><span class="sxs-lookup"><span data-stu-id="c73cd-209">Such a constructor makes `Span<T>` which are used as fields indistinguishable from a `ref` field.</span></span> <span data-ttu-id="c73cd-210">本檔中所述的安全性規則取決於或 .NET 中C#的 `ref` 欄位不是有效的結構。</span><span class="sxs-lookup"><span data-stu-id="c73cd-210">The safety rules described in this document depend on `ref` fields not being a valid construct in C#, or .NET.</span></span>

#### <a name="default-expressions"></a><span data-ttu-id="c73cd-211">`default` 運算式</span><span class="sxs-lookup"><span data-stu-id="c73cd-211">`default` expressions</span></span>

<span data-ttu-id="c73cd-212">`default` 運算式可從整個封入方法中進行*安全的轉義*。</span><span class="sxs-lookup"><span data-stu-id="c73cd-212">A `default` expression is *safe-to-escape* from the entire enclosing method.</span></span>

## <a name="language-constraints"></a><span data-ttu-id="c73cd-213">語言條件約束</span><span class="sxs-lookup"><span data-stu-id="c73cd-213">Language Constraints</span></span>

<span data-ttu-id="c73cd-214">我們想要確保沒有任何 `ref` 的區域變數，而且沒有任何 `ref struct` 類型的變數，指的是堆疊記憶體或不再處於作用中的變數。</span><span class="sxs-lookup"><span data-stu-id="c73cd-214">We wish to ensure that no `ref` local variable, and no variable of `ref struct` type, refers to stack memory or variables that are no longer alive.</span></span> <span data-ttu-id="c73cd-215">因此，我們有下列語言條件約束：</span><span class="sxs-lookup"><span data-stu-id="c73cd-215">We therefore have the following language constraints:</span></span>

- <span data-ttu-id="c73cd-216">Ref 參數和 ref 區域變數，或 `ref struct` 類型的參數或本機，都無法提升為 lambda 或區域函數。</span><span class="sxs-lookup"><span data-stu-id="c73cd-216">Neither a ref parameter, nor a ref local, nor a parameter or local of a `ref struct` type can be lifted into a lambda or local function.</span></span>

- <span data-ttu-id="c73cd-217">Ref 參數和 `ref struct` 類型的參數都不能是反覆運算器方法或 `async` 方法的引數。</span><span class="sxs-lookup"><span data-stu-id="c73cd-217">Neither a ref parameter nor a parameter of a `ref struct` type may be an argument on an iterator method or an `async` method.</span></span>

- <span data-ttu-id="c73cd-218">Ref local 和 `ref struct` 類型的本機都不能在 `yield return` 語句或 `await` 運算式的範圍中。</span><span class="sxs-lookup"><span data-stu-id="c73cd-218">Neither a ref local, nor a local of a `ref struct` type may be in scope at the point of a `yield return` statement or an `await` expression.</span></span>

- <span data-ttu-id="c73cd-219">`ref struct` 類型不能當做類型引數使用，或做為元組類型中的元素類型。</span><span class="sxs-lookup"><span data-stu-id="c73cd-219">A `ref struct` type may not be used as a type argument, or as an element type in a tuple type.</span></span>

- <span data-ttu-id="c73cd-220">`ref struct` 類型可能不是欄位的宣告型別，但它可能是另一個 `ref struct`之實例欄位的宣告型別。</span><span class="sxs-lookup"><span data-stu-id="c73cd-220">A `ref struct` type may not be the declared type of a field, except that it may be the declared type of an instance field of another `ref struct`.</span></span>

- <span data-ttu-id="c73cd-221">`ref struct` 類型可能不是陣列的元素類型。</span><span class="sxs-lookup"><span data-stu-id="c73cd-221">A `ref struct` type may not be the element type of an array.</span></span>

- <span data-ttu-id="c73cd-222">可能不會將 `ref struct` 類型的值裝箱：</span><span class="sxs-lookup"><span data-stu-id="c73cd-222">A value of a `ref struct` type may not be boxed:</span></span>
  - <span data-ttu-id="c73cd-223">沒有從 `ref struct` 類型轉換成類型 `object` 或類型 `System.ValueType`。</span><span class="sxs-lookup"><span data-stu-id="c73cd-223">There is no conversion from a `ref struct` type to the type `object` or the type `System.ValueType`.</span></span>
  - <span data-ttu-id="c73cd-224">可能不會宣告 `ref struct` 類型來執行任何介面</span><span class="sxs-lookup"><span data-stu-id="c73cd-224">A `ref struct` type may not be declared to implement any interface</span></span>
  - <span data-ttu-id="c73cd-225">`object` 或 `System.ValueType` 中未宣告的實例方法，但在 `ref struct` 類型中未覆寫，可能會以該 `ref struct` 類型的接收者來呼叫。</span><span class="sxs-lookup"><span data-stu-id="c73cd-225">No instance method declared in `object` or in `System.ValueType` but not overridden in a `ref struct` type may be called with a receiver of that `ref struct` type.</span></span>
  - <span data-ttu-id="c73cd-226">方法轉換至委派類型時，不可以捕捉 `ref struct` 類型的實例方法。</span><span class="sxs-lookup"><span data-stu-id="c73cd-226">No instance method of a `ref struct` type may be captured by method conversion to a delegate type.</span></span>

- <span data-ttu-id="c73cd-227">針對 ref 重新指派 `ref e1 = ref e2`，`e2` 的*ref 安全對 escape*必須至少與 `e1`的*ref 安全到 escape*的範圍相同。</span><span class="sxs-lookup"><span data-stu-id="c73cd-227">For a ref reassignment `ref e1 = ref e2`, the *ref-safe-to-escape* of `e2` must be at least as wide a scope as the *ref-safe-to-escape* of `e1`.</span></span>

- <span data-ttu-id="c73cd-228">針對 ref return 語句 `return ref e1`，`e1` 的*ref 安全對 escape*必須從整個方法中*以 ref 安全*的方式來進行 escape。</span><span class="sxs-lookup"><span data-stu-id="c73cd-228">For a ref return statement `return ref e1`, the *ref-safe-to-escape* of `e1` must be *ref-safe-to-escape* from the entire method.</span></span> <span data-ttu-id="c73cd-229">（TODO：我們也需要一個規則，`e1` 必須從整個方法進行*安全的 escape* ，或這是多餘的？）</span><span class="sxs-lookup"><span data-stu-id="c73cd-229">(TODO: Do we also need a rule that `e1` must be *safe-to-escape* from the entire method, or is that redundant?)</span></span>

- <span data-ttu-id="c73cd-230">`return e1`的 return 語句中，`e1` 的*安全對 escape*必須是整個方法的*安全*對 escape。</span><span class="sxs-lookup"><span data-stu-id="c73cd-230">For a return statement `return e1`, the *safe-to-escape* of `e1` must be *safe-to-escape* from the entire method.</span></span>

- <span data-ttu-id="c73cd-231">對於指派 `e1 = e2`，如果 `e1` 的類型是 `ref struct` 類型，則 `e2` 的*安全-escape*必須至少與 `e1`的*安全轉轉義*的範圍相同。</span><span class="sxs-lookup"><span data-stu-id="c73cd-231">For an assignment `e1 = e2`, if the type of `e1` is a `ref struct` type, then the *safe-to-escape* of `e2` must be at least as wide a scope as the *safe-to-escape* of `e1`.</span></span>

- <span data-ttu-id="c73cd-232">若為方法叫用，如果 `ref struct` 類型的 `ref` 或 `out` 引數（包括接收者），具有安全的*escape* E1，則沒有引數（包括接收者）可能會比 E1 更窄的*安全到 escape* 。</span><span class="sxs-lookup"><span data-stu-id="c73cd-232">For a method invocation if there is a `ref` or `out` argument of a `ref struct` type (including the receiver), with *safe-to-escape* E1, then no argument (including the receiver) may have a narrower *safe-to-escape* than E1.</span></span> [<span data-ttu-id="c73cd-233">範例</span><span class="sxs-lookup"><span data-stu-id="c73cd-233">Sample</span></span>](#method-arguments-must-match)

- <span data-ttu-id="c73cd-234">區域函式或匿名函式不能參考在封閉範圍中宣告之 `ref struct` 類型的本機或參數。</span><span class="sxs-lookup"><span data-stu-id="c73cd-234">A local function or anonymous function may not refer to a local or parameter of `ref struct` type declared in an enclosing scope.</span></span>

> <span data-ttu-id="c73cd-235">***開啟問題：*** 我們需要一些規則，讓我們在需要于 await 運算式溢出 `ref struct` 類型的堆疊值時產生錯誤，例如在程式碼中。</span><span class="sxs-lookup"><span data-stu-id="c73cd-235">***Open Issue:*** We need some rule that permits us to produce an error when needing to spill a stack value of a `ref struct` type at an await expression, for example in the code</span></span>
> ```csharp
> Foo(new Span<int>(...), await e2);
> ```

## <a name="explanations"></a><span data-ttu-id="c73cd-236">說明</span><span class="sxs-lookup"><span data-stu-id="c73cd-236">Explanations</span></span>
<span data-ttu-id="c73cd-237">這些說明和範例有助於說明上述許多安全性規則的存在原因</span><span class="sxs-lookup"><span data-stu-id="c73cd-237">These explanations and samples help explain why many of the safety rules above exist</span></span>

### <a name="method-arguments-must-match"></a><span data-ttu-id="c73cd-238">方法引數必須相符</span><span class="sxs-lookup"><span data-stu-id="c73cd-238">Method Arguments Must Match</span></span>
<span data-ttu-id="c73cd-239">當叫用具有 `out`的方法時，`ref` 參數是 `ref struct` （包括接收者），則所有 `ref struct` 都必須具有相同的存留期。</span><span class="sxs-lookup"><span data-stu-id="c73cd-239">When invoking a method where there is an `out`, `ref` parameter that is a `ref struct` including the receiver then all of the `ref struct` need to have the same lifetime.</span></span> <span data-ttu-id="c73cd-240">這是必要的C# ，因為必須根據方法簽章中可用的資訊和呼叫位置的值存留期，讓所有 it 決策的存留期安全。</span><span class="sxs-lookup"><span data-stu-id="c73cd-240">This is necessary because C# must make all of it's decisions around lifetime safety based on the information available in the signature of the method and the lifetime of the values at the call site.</span></span> 

<span data-ttu-id="c73cd-241">當有 `ref` 的參數 `ref struct` 時，就會有這個可能性可以交換其內容。</span><span class="sxs-lookup"><span data-stu-id="c73cd-241">When there are `ref` parameters that are `ref struct` then there is the possiblity they could swap around their contents.</span></span> <span data-ttu-id="c73cd-242">因此，在呼叫位置，我們必須確定所有這些**可能**的交換都相容。</span><span class="sxs-lookup"><span data-stu-id="c73cd-242">Hence at the call site we must ensure all of these **potential** swaps are compatible.</span></span> <span data-ttu-id="c73cd-243">如果語言未強制執行，則會允許類似下列的錯誤程式碼。</span><span class="sxs-lookup"><span data-stu-id="c73cd-243">If the language didn't enforce that then it will allow for bad code like the following.</span></span>

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

<span data-ttu-id="c73cd-244">接收者的限制是必要的，因為雖然沒有任何內容是 ref 安全的，但它可以儲存提供的值。</span><span class="sxs-lookup"><span data-stu-id="c73cd-244">The restriction on the receiver is necessary because while none of its contents are ref-safe-to-escape it can store the provided values.</span></span> <span data-ttu-id="c73cd-245">這表示，在存留期不相符的情況下，您可以透過下列方式建立型別安全漏洞：</span><span class="sxs-lookup"><span data-stu-id="c73cd-245">This means with mismatched lifetimes you could create a type safety hole in the following way:</span></span>

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

### <a name="struct-this-escape"></a><span data-ttu-id="c73cd-246">結構此 Escape</span><span class="sxs-lookup"><span data-stu-id="c73cd-246">Struct This Escape</span></span>
<span data-ttu-id="c73cd-247">當有範圍的安全性規則時，實例成員中的 `this` 值會模型化為成員的參數。</span><span class="sxs-lookup"><span data-stu-id="c73cd-247">When it comes to span safety rules the `this` value in an instance member is modeled as a parameter to the member.</span></span> <span data-ttu-id="c73cd-248">現在，`struct` `this` 的類型實際上是 `ref S` 在 `class` 中，它只會 `S` （針對名為 S 的 `class / struct` 成員）。</span><span class="sxs-lookup"><span data-stu-id="c73cd-248">Now for a `struct` the type of `this` is actually `ref S` where in a `class` it's simply `S` (for members of a `class / struct` named S).</span></span> 

<span data-ttu-id="c73cd-249">但是 `this` 與其他 `ref` 參數具有不同的轉義規則。</span><span class="sxs-lookup"><span data-stu-id="c73cd-249">Yet `this` has different escaping rules than other `ref` parameters.</span></span> <span data-ttu-id="c73cd-250">具體而言，它並不是 ref-安全到 escape，而其他參數則是：</span><span class="sxs-lookup"><span data-stu-id="c73cd-250">Specifically it is not ref-safe-to-escape while other parameters are:</span></span>

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

<span data-ttu-id="c73cd-251">這種限制的原因實際上與 `struct` 成員調用很少。</span><span class="sxs-lookup"><span data-stu-id="c73cd-251">The reason for this restriction actually has little to do with `struct` member invocation.</span></span> <span data-ttu-id="c73cd-252">在接收者為右值的 `struct` 成員上，有一些需要處理成員調用的相關規則。</span><span class="sxs-lookup"><span data-stu-id="c73cd-252">There are some rules that need to be worked out with respect to member invocation on `struct` members where the receiver is an rvalue.</span></span> <span data-ttu-id="c73cd-253">但這非常平易近人。</span><span class="sxs-lookup"><span data-stu-id="c73cd-253">But that is very approachable.</span></span> 

<span data-ttu-id="c73cd-254">這種限制的原因實際上是關於介面調用。</span><span class="sxs-lookup"><span data-stu-id="c73cd-254">The reason for this restriction is actually about interface invocation.</span></span> <span data-ttu-id="c73cd-255">具體而言，它會向下說明下列範例是否應該或不應該編譯;</span><span class="sxs-lookup"><span data-stu-id="c73cd-255">Specifically it comes down to whether or not the following sample should or should not compile;</span></span>

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

<span data-ttu-id="c73cd-256">請考慮將 `T` 具現化為 `struct`的情況。</span><span class="sxs-lookup"><span data-stu-id="c73cd-256">Consider the case where `T` is instantiated as a `struct`.</span></span> <span data-ttu-id="c73cd-257">如果 `this` 參數是 ref-安全到 escape，則 `p.Get` 的傳回可能會指向堆疊（具體而言，它可能是 `T`具現化型別內的欄位）。</span><span class="sxs-lookup"><span data-stu-id="c73cd-257">If the `this` parameter is ref-safe-to-escape then the return of `p.Get` could point to the stack (specifically it could be a field inside of the instantiated type of `T`).</span></span> <span data-ttu-id="c73cd-258">這表示語言無法允許此範例進行編譯，因為它可能會將 `ref` 傳回堆疊位置。</span><span class="sxs-lookup"><span data-stu-id="c73cd-258">That means the language could not allow this sample to compile as it could be returning a `ref` to a stack location.</span></span> <span data-ttu-id="c73cd-259">另一方面，如果 `this` 不是 ref-安全到 escape，那麼 `p.Get` 就無法參考堆疊，因此可以安全地傳回。</span><span class="sxs-lookup"><span data-stu-id="c73cd-259">On the other hand if `this` is not ref-safe-to-escape then `p.Get` cannot refer to the stack and hence it's safe to return.</span></span> 

<span data-ttu-id="c73cd-260">這就是為什麼 `struct` 中的 `this` escapability 其實是關於介面的原因。</span><span class="sxs-lookup"><span data-stu-id="c73cd-260">This is why the escapability of `this` in a `struct` is really all about interfaces.</span></span> <span data-ttu-id="c73cd-261">它絕對可以運作，但卻有取捨。</span><span class="sxs-lookup"><span data-stu-id="c73cd-261">It can absolutely be made to work but it has a trade off.</span></span> <span data-ttu-id="c73cd-262">設計最後會導致介面變得更有彈性。</span><span class="sxs-lookup"><span data-stu-id="c73cd-262">The design eventually came down in favor of making interfaces more flexible.</span></span> 

<span data-ttu-id="c73cd-263">不過，我們未來可能會放寬這項功能。</span><span class="sxs-lookup"><span data-stu-id="c73cd-263">There is potential for us to relax this in the future though.</span></span> 

## <a name="future-considerations"></a><span data-ttu-id="c73cd-264">未來的考量</span><span class="sxs-lookup"><span data-stu-id="c73cd-264">Future Considerations</span></span>

### <a name="length-one-spant-over-ref-values"></a><span data-ttu-id="c73cd-265">長度一個範圍\<T > 超過 ref 值</span><span class="sxs-lookup"><span data-stu-id="c73cd-265">Length one Span\<T> over ref values</span></span>
<span data-ttu-id="c73cd-266">雖然今天不合法，但在某些情況下，在值上建立一個 `Span<T>` 實例的長度會很有説明：</span><span class="sxs-lookup"><span data-stu-id="c73cd-266">Though not legal today there are cases where creating a length one `Span<T>` instance over a value would be beneficial:</span></span>

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

<span data-ttu-id="c73cd-267">如果我們對[固定大小的緩衝區](https://github.com/dotnet/csharplang/blob/master/proposals/fixed-sized-buffers.md)提出限制，讓 `Span<T>` 的實例變得更長，這項功能會更具吸引力。</span><span class="sxs-lookup"><span data-stu-id="c73cd-267">This feature gets more compelling if we lift the restrictions on [fixed sized buffers](https://github.com/dotnet/csharplang/blob/master/proposals/fixed-sized-buffers.md) as it would allow for `Span<T>` instances of even greater length.</span></span> 

<span data-ttu-id="c73cd-268">如果您需要關閉此路徑，則語言可以藉由確保這類 `Span<T>` 實例僅限在使用中，來滿足此需求。</span><span class="sxs-lookup"><span data-stu-id="c73cd-268">If there is ever a need to go down this path then the language could accommodate this by ensuring such `Span<T>` instances were downward facing only.</span></span> <span data-ttu-id="c73cd-269">也就是說，它們只會在建立的範圍內受到*保護*。</span><span class="sxs-lookup"><span data-stu-id="c73cd-269">That is they were only ever *safe-to-escape* to the scope in which they were created.</span></span> <span data-ttu-id="c73cd-270">這可確保語言永遠不需要考慮 `ref` 值透過 `ref struct`的 `ref struct` 傳回或欄位來將方法進行轉義。</span><span class="sxs-lookup"><span data-stu-id="c73cd-270">This ensure the language never had to consider a `ref` value escaping a method via a `ref struct` return or field of `ref struct`.</span></span> <span data-ttu-id="c73cd-271">不過，這可能也需要進一步的變更來辨識這類的函式，以這種方式來捕捉 `ref` 參數。</span><span class="sxs-lookup"><span data-stu-id="c73cd-271">This would likely also require further changes to recognize such constructors as capturing a `ref` parameter in this way though.</span></span>
