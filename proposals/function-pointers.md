---
ms.openlocfilehash: d9080202f9413f8beb80db222d47f5fc082ae641
ms.sourcegitcommit: f3170512e7a3193efbcea52ec330648375e36915
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/11/2020
ms.locfileid: "79485501"
---
# <a name="function-pointers"></a><span data-ttu-id="b2616-101">函式指標</span><span class="sxs-lookup"><span data-stu-id="b2616-101">Function Pointers</span></span>

## <a name="summary"></a><span data-ttu-id="b2616-102">摘要</span><span class="sxs-lookup"><span data-stu-id="b2616-102">Summary</span></span>

<span data-ttu-id="b2616-103">本提案提供的語言結構會公開目前無法有效率地存取的 IL opcode，或目前的C# `ldftn` 和 `calli`。</span><span class="sxs-lookup"><span data-stu-id="b2616-103">This proposal provides language constructs that expose IL opcodes that cannot currently be accessed efficiently, or at all, in C# today: `ldftn` and `calli`.</span></span> <span data-ttu-id="b2616-104">這些 IL opcode 在高效能程式碼中很重要，而開發人員需要有效率的方式來存取它們。</span><span class="sxs-lookup"><span data-stu-id="b2616-104">These IL opcodes can be important in high performance code and developers need an efficient way to access them.</span></span>

## <a name="motivation"></a><span data-ttu-id="b2616-105">動機</span><span class="sxs-lookup"><span data-stu-id="b2616-105">Motivation</span></span>

<span data-ttu-id="b2616-106">下列問題會說明這項功能的動機和背景（這是功能的可能執行）：</span><span class="sxs-lookup"><span data-stu-id="b2616-106">The motivations and background for this feature are described in the following issue (as is a potential implementation of the feature):</span></span>

https://github.com/dotnet/csharplang/issues/191

<span data-ttu-id="b2616-107">這是[編譯器內建函式](https://github.com/dotnet/csharplang/blob/master/proposals/intrinsics.md)的替代設計提案</span><span class="sxs-lookup"><span data-stu-id="b2616-107">This is an alternate design proposal to [compiler intrinsics](https://github.com/dotnet/csharplang/blob/master/proposals/intrinsics.md)</span></span>

## <a name="detailed-design"></a><span data-ttu-id="b2616-108">詳細設計</span><span class="sxs-lookup"><span data-stu-id="b2616-108">Detailed Design</span></span>

### <a name="function-pointers"></a><span data-ttu-id="b2616-109">函式指標</span><span class="sxs-lookup"><span data-stu-id="b2616-109">Function pointers</span></span>

<span data-ttu-id="b2616-110">語言可讓您使用 `delegate*` 語法來宣告函式指標。</span><span class="sxs-lookup"><span data-stu-id="b2616-110">The language will allow for the declaration of function pointers using the `delegate*` syntax.</span></span> <span data-ttu-id="b2616-111">下一節將詳細說明完整的語法，但其目的是要類似 `Func` 和 `Action` 型別宣告所使用的語法。</span><span class="sxs-lookup"><span data-stu-id="b2616-111">The full syntax is described in detail in the next section but it is meant to resemble the syntax used by `Func` and `Action` type declarations.</span></span>

``` csharp
unsafe class Example {
    void Example(Action<int> a, delegate*<int, void> f) {
        a(42);
        f(42);
    }
}
```

<span data-ttu-id="b2616-112">這些類型是使用如 ECMA-335 中所述的函式指標類型來表示。</span><span class="sxs-lookup"><span data-stu-id="b2616-112">These types are represented using the function pointer type as outlined in ECMA-335.</span></span> <span data-ttu-id="b2616-113">這表示 `delegate*` 的調用會使用 `calli`，其中 `delegate` 的調用會在 `Invoke` 方法上使用 `callvirt`。</span><span class="sxs-lookup"><span data-stu-id="b2616-113">This means invocation of a `delegate*` will use `calli` where invocation of a `delegate` will use `callvirt` on the `Invoke` method.</span></span>
<span data-ttu-id="b2616-114">在語法上，這兩個結構的調用都相同。</span><span class="sxs-lookup"><span data-stu-id="b2616-114">Syntactically though invocation is identical for both constructs.</span></span>

<span data-ttu-id="b2616-115">方法指標的 ECMA-335 定義包含呼叫慣例做為類型簽章的一部分（第7.1 節）。</span><span class="sxs-lookup"><span data-stu-id="b2616-115">The ECMA-335 definition of method pointers includes the calling convention as part of the type signature (section 7.1).</span></span>
<span data-ttu-id="b2616-116">預設的呼叫慣例將會 `managed`。</span><span class="sxs-lookup"><span data-stu-id="b2616-116">The default calling convention will be `managed`.</span></span> <span data-ttu-id="b2616-117">在 `delegate*` 語法之後加入適當的修飾詞，即可指定替代形式： `managed`、`cdecl`、`stdcall`、`thiscall`或 `unmanaged`。</span><span class="sxs-lookup"><span data-stu-id="b2616-117">Alternate forms can be specified by adding the appropriate modifier after the `delegate*` syntax: `managed`, `cdecl`, `stdcall`, `thiscall`, or `unmanaged`.</span></span> <span data-ttu-id="b2616-118">範例：</span><span class="sxs-lookup"><span data-stu-id="b2616-118">Example:</span></span>

``` csharp
// This method will be invoked using the cdecl calling convention
delegate* cdecl<int, int>;

// This method will be invoked using the stdcall calling convention
delegate* stdcall<int, int>;
```

<span data-ttu-id="b2616-119">`delegate*` 類型之間的轉換是根據其簽章（包括呼叫慣例）來完成。</span><span class="sxs-lookup"><span data-stu-id="b2616-119">Conversions between `delegate*` types is done based on their signature including the calling convention.</span></span>

``` csharp
unsafe class Example {
    void Conversions() {
        delegate*<int, int, int> p1 = ...;
        delegate* managed<int, int, int> p2 = ...;
        delegate* cdecl<int, int, int> p3 = ...;

        p1 = p2; // okay p1 and p2 have compatible signatures
        Console.WriteLine(p2 == p1); // True
        p2 = p3; // error: calling conventions are incompatible
    }
}
```

<span data-ttu-id="b2616-120">`delegate*` 類型是指標類型，這表示它具有標準指標類型的所有功能和限制：</span><span class="sxs-lookup"><span data-stu-id="b2616-120">A `delegate*` type is a pointer type which means it has all of the capabilities and restrictions of a standard pointer type:</span></span>

- <span data-ttu-id="b2616-121">僅在 `unsafe` 內容中有效。</span><span class="sxs-lookup"><span data-stu-id="b2616-121">Only valid in an `unsafe` context.</span></span>
- <span data-ttu-id="b2616-122">只能從 `unsafe` 內容呼叫包含 `delegate*` 參數或傳回類型的方法。</span><span class="sxs-lookup"><span data-stu-id="b2616-122">Methods which contain a `delegate*` parameter or return type can only be called from an `unsafe` context.</span></span>
- <span data-ttu-id="b2616-123">無法轉換成 `object`。</span><span class="sxs-lookup"><span data-stu-id="b2616-123">Cannot be converted to `object`.</span></span>
- <span data-ttu-id="b2616-124">不能當做泛型引數使用。</span><span class="sxs-lookup"><span data-stu-id="b2616-124">Cannot be used as a generic argument.</span></span>
- <span data-ttu-id="b2616-125">可以將 `delegate*` 隱含地轉換成 `void*`。</span><span class="sxs-lookup"><span data-stu-id="b2616-125">Can implicitly convert `delegate*` to `void*`.</span></span>
- <span data-ttu-id="b2616-126">可以從 `void*` 明確轉換成 `delegate*`。</span><span class="sxs-lookup"><span data-stu-id="b2616-126">Can explicitly convert from `void*` to `delegate*`.</span></span>

<span data-ttu-id="b2616-127">限制：</span><span class="sxs-lookup"><span data-stu-id="b2616-127">Restrictions:</span></span>

- <span data-ttu-id="b2616-128">自訂屬性不能套用至 `delegate*` 或其任何元素。</span><span class="sxs-lookup"><span data-stu-id="b2616-128">Custom attributes cannot be applied to a `delegate*` or any of its elements.</span></span>
- <span data-ttu-id="b2616-129">`delegate*` 參數不能標記為 `params`</span><span class="sxs-lookup"><span data-stu-id="b2616-129">A `delegate*` parameter cannot be marked as `params`</span></span>
- <span data-ttu-id="b2616-130">`delegate*` 類型具有一般指標類型的所有限制。</span><span class="sxs-lookup"><span data-stu-id="b2616-130">A `delegate*` type has all of the restrictions of a normal pointer type.</span></span>

### <a name="function-pointer-syntax"></a><span data-ttu-id="b2616-131">函式指標語法</span><span class="sxs-lookup"><span data-stu-id="b2616-131">Function pointer syntax</span></span>

<span data-ttu-id="b2616-132">完整的函式指標語法是以下列文法表示：</span><span class="sxs-lookup"><span data-stu-id="b2616-132">The full function pointer syntax is represented by the following grammar:</span></span>

```antlr
pointer_type
    : ...
    | funcptr_type
    ;

funcptr_type
    : 'delegate' '*' calling_convention? '<' (funcptr_parameter_modifier? type ',')* funcptr_return_modifier? return_type '>'
    ;

calling_convention
    : 'cdecl'
    | 'managed'
    | 'stdcall'
    | 'thiscall'
    | 'unmanaged'
    ;

funcptr_parameter_modifier
    : 'ref'
    | 'out'
    | 'in'
    ;

funcptr_return_modifier
    : 'ref'
    | 'ref readonly'
    ;
```

<span data-ttu-id="b2616-133">`unmanaged` 呼叫慣例代表目前平臺上機器碼的預設呼叫慣例，而且會編碼為 winapi。</span><span class="sxs-lookup"><span data-stu-id="b2616-133">The `unmanaged` calling convention represents the default calling convention for native code on the current platform, and is encoded as winapi.</span></span>
<span data-ttu-id="b2616-134">所有 `calling_convention`在前面加上 `delegate*`時都是內容關鍵字。</span><span class="sxs-lookup"><span data-stu-id="b2616-134">All `calling_convention`s are contextual keywords when preceded by a `delegate*`.</span></span>

``` csharp
delegate int Func1(string s);
delegate Func1 Func2(Func1 f);

// Function pointer equivalent without calling convention
delegate*<string, int>;
delegate*<delegate*<string, int>, delegate*<string, int>>;

// Function pointer equivalent with calling convention
delegate* managed<string, int>;
delegate*<delegate* managed<string, int>, delegate*<string, int>>;
```

### <a name="function-pointer-conversions"></a><span data-ttu-id="b2616-135">函式指標轉換</span><span class="sxs-lookup"><span data-stu-id="b2616-135">Function pointer conversions</span></span>

<span data-ttu-id="b2616-136">在不安全的內容中，可用的隱含轉換集合（隱含轉換）會擴充以包含下列隱含指標轉換：</span><span class="sxs-lookup"><span data-stu-id="b2616-136">In an unsafe context, the set of available implicit conversions (Implicit conversions) is extended to include the following implicit pointer conversions:</span></span>
- [<span data-ttu-id="b2616-137">_現有的轉換_</span><span class="sxs-lookup"><span data-stu-id="b2616-137">_Existing conversions_</span></span>](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#pointer-conversions)
- <span data-ttu-id="b2616-138">從_funcptr\_輸入_`F0` 至另一個_funcptr\_類型_`F1`，但前提是下列所有條件皆成立：</span><span class="sxs-lookup"><span data-stu-id="b2616-138">From _funcptr\_type_ `F0` to another _funcptr\_type_ `F1`, provided all of the following are true:</span></span>
    - <span data-ttu-id="b2616-139">`F0` 和 `F1` 的參數數目相同，而且 `F0` 中 `D0n` 的每個參數都有與 `ref`中對應參數 `out`相同的 `in`、`D1n` 或 `F1`修飾詞。</span><span class="sxs-lookup"><span data-stu-id="b2616-139">`F0` and `F1` have the same number of parameters, and each parameter `D0n` in `F0` has the same `ref`, `out`, or `in` modifiers as the corresponding parameter `D1n` in `F1`.</span></span>
    - <span data-ttu-id="b2616-140">針對每個值參數（不含 `ref`、`out`或 `in` 修飾詞的參數），會從 `F0` 中的參數類型，將識別轉換、隱含參考轉換或隱含指標轉換存在 `F1`中的對應參數類型。</span><span class="sxs-lookup"><span data-stu-id="b2616-140">For each value parameter (a parameter with no `ref`, `out`, or `in` modifier), an identity conversion, implicit reference conversion, or implicit pointer conversion exists from the parameter type in `F0` to the corresponding parameter type in `F1`.</span></span>
    - <span data-ttu-id="b2616-141">針對每個 `ref`、`out`或 `in` 參數，`F0` 中的參數類型與 `F1`中對應的參數類型相同。</span><span class="sxs-lookup"><span data-stu-id="b2616-141">For each `ref`, `out`, or `in` parameter, the parameter type in `F0` is the same as the corresponding parameter type in `F1`.</span></span>
    - <span data-ttu-id="b2616-142">如果傳回類型是以傳值方式傳回（不 `ref` 或 `ref readonly`），則會從 `F1` 的傳回類型，將識別、隱含參考或隱含指標轉換存在於 `F0`的傳回類型。</span><span class="sxs-lookup"><span data-stu-id="b2616-142">If the return type is by value (no `ref` or `ref readonly`), an identity, implicit reference, or implicit pointer conversion exists from the return type of `F1` to the return type of `F0`.</span></span>
    - <span data-ttu-id="b2616-143">如果傳回型別是以傳址方式（`ref` 或 `ref readonly`），則 `F1` 的傳回型別和 `ref` 修飾詞會與 `ref` 的傳回型別和 `F0`修飾詞相同。</span><span class="sxs-lookup"><span data-stu-id="b2616-143">If the return type is by reference (`ref` or `ref readonly`), the return type and `ref` modifiers of `F1` are the same as the return type and `ref` modifiers of `F0`.</span></span>
    - <span data-ttu-id="b2616-144">`F0` 的呼叫慣例與 `F1`的呼叫慣例相同。</span><span class="sxs-lookup"><span data-stu-id="b2616-144">The calling convention of `F0` is the same as the calling convention of `F1`.</span></span>

### <a name="allow-address-of-to-target-methods"></a><span data-ttu-id="b2616-145">允許目標方法的位址</span><span class="sxs-lookup"><span data-stu-id="b2616-145">Allow address-of to target methods</span></span>

<span data-ttu-id="b2616-146">方法群組現在將允許當做位址運算式的引數。</span><span class="sxs-lookup"><span data-stu-id="b2616-146">Method groups will now be allowed as arguments to an address-of expression.</span></span> <span data-ttu-id="b2616-147">這類運算式的類型會是具有目標方法的對等簽章和 managed 呼叫慣例的 `delegate*`：</span><span class="sxs-lookup"><span data-stu-id="b2616-147">The type of such an expression will be a `delegate*` which has the equivalent signature of the target method and a managed calling convention:</span></span>

``` csharp
unsafe class Util {
    public static void Log() { }

    void Use() {
        delegate*<void> ptr1 = &Util.Log;

        // Error: type "delegate*<void>" not compatible with "delegate*<int>";
        delegate*<int> ptr2 = &Util.Log;

        // Okay. Conversion to void* is always allowed.
        void* v = &Util.Log;
   }
}
```

<span data-ttu-id="b2616-148">在不安全的內容中，如果下列所有條件都成立，則方法 `M` 與函式指標型別相容 `F`：</span><span class="sxs-lookup"><span data-stu-id="b2616-148">In an unsafe context, a method `M` is compatible with a function pointer type `F` if all of the following are true:</span></span>
- <span data-ttu-id="b2616-149">`M` 和 `F` 的參數數目相同，而且 `D` 中的每個參數都有相同的 `ref`、`out`或 `in` 修飾詞，做為 `F`中的對應參數。</span><span class="sxs-lookup"><span data-stu-id="b2616-149">`M` and `F` have the same number of parameters, and each parameter in `D` has the same `ref`, `out`, or `in` modifiers as the corresponding parameter in `F`.</span></span>
- <span data-ttu-id="b2616-150">針對每個值參數（不含 `ref`、`out`或 `in` 修飾詞的參數），會從 `M` 中的參數類型，將識別轉換、隱含參考轉換或隱含指標轉換存在 `F`中的對應參數類型。</span><span class="sxs-lookup"><span data-stu-id="b2616-150">For each value parameter (a parameter with no `ref`, `out`, or `in` modifier), an identity conversion, implicit reference conversion, or implicit pointer conversion exists from the parameter type in `M` to the corresponding parameter type in `F`.</span></span>
- <span data-ttu-id="b2616-151">針對每個 `ref`、`out`或 `in` 參數，`M` 中的參數類型與 `F`中對應的參數類型相同。</span><span class="sxs-lookup"><span data-stu-id="b2616-151">For each `ref`, `out`, or `in` parameter, the parameter type in `M` is the same as the corresponding parameter type in `F`.</span></span>
- <span data-ttu-id="b2616-152">如果傳回類型是以傳值方式傳回（不 `ref` 或 `ref readonly`），則會從 `F` 的傳回類型，將識別、隱含參考或隱含指標轉換存在於 `M`的傳回類型。</span><span class="sxs-lookup"><span data-stu-id="b2616-152">If the return type is by value (no `ref` or `ref readonly`), an identity, implicit reference, or implicit pointer conversion exists from the return type of `F` to the return type of `M`.</span></span>
- <span data-ttu-id="b2616-153">如果傳回型別是以傳址方式（`ref` 或 `ref readonly`），則 `F` 的傳回型別和 `ref` 修飾詞會與 `ref` 的傳回型別和 `M`修飾詞相同。</span><span class="sxs-lookup"><span data-stu-id="b2616-153">If the return type is by reference (`ref` or `ref readonly`), the return type and `ref` modifiers of `F` are the same as the return type and `ref` modifiers of `M`.</span></span>
- <span data-ttu-id="b2616-154">`M` 的呼叫慣例與 `F`的呼叫慣例相同。</span><span class="sxs-lookup"><span data-stu-id="b2616-154">The calling convention of `M` is the same as the calling convention of `F`.</span></span>
- <span data-ttu-id="b2616-155">`M` 是靜態方法。</span><span class="sxs-lookup"><span data-stu-id="b2616-155">`M` is a static method.</span></span>

<span data-ttu-id="b2616-156">在不安全的內容中，從目標是方法群組 `E` 到相容函式指標型別的隱含轉換存在 `F` 如果 `E` 包含至少一個適用于其一般格式的方法，並使用 `F`的參數類型和修飾詞所建立的引數清單，如下所述。</span><span class="sxs-lookup"><span data-stu-id="b2616-156">In an unsafe context, an implicit conversion exists from an address-of expression whose target is a method group `E` to a compatible function pointer type `F` if `E` contains at least one method that is applicable in its normal form to an argument list constructed by use of the parameter types and modifiers of `F`, as described in the following.</span></span>
- <span data-ttu-id="b2616-157">選取的單一方法 `M` 對應至表單 `E(A)` 的方法調用，並具有下列修改：</span><span class="sxs-lookup"><span data-stu-id="b2616-157">A single method `M` is selected corresponding to a method invocation of the form `E(A)` with the following modifications:</span></span>
   - <span data-ttu-id="b2616-158">引數清單 `A` 是運算式的清單，每一個都會分類為一個變數，並使用對應的_正式\_參數_的類型和修飾詞（`ref`、`out`或 `in`）\_`D`清單。</span><span class="sxs-lookup"><span data-stu-id="b2616-158">The arguments list `A` is a list of expressions, each classified as a variable and with the type and modifier (`ref`, `out`, or `in`) of the corresponding _formal\_parameter\_list_ of `D`.</span></span>
   - <span data-ttu-id="b2616-159">候選方法只是那些僅適用于其一般格式的方法，而不是適用于其擴充形式的方法。</span><span class="sxs-lookup"><span data-stu-id="b2616-159">The candidate methods are only those methods that are only those methods that are applicable in their normal form, not those applicable in their expanded form.</span></span>
- <span data-ttu-id="b2616-160">如果方法調用的演算法產生錯誤，則會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="b2616-160">If the algorithm of Method invocations produces an error, then a compile-time error occurs.</span></span> <span data-ttu-id="b2616-161">否則，此演算法會產生一個最佳方法，`M` 具有與 `F` 相同數目的參數，並將轉換視為存在。</span><span class="sxs-lookup"><span data-stu-id="b2616-161">Otherwise, the algorithm produces a single best method `M` having the same number of parameters as `F` and the conversion is considered to exist.</span></span>
- <span data-ttu-id="b2616-162">選取的方法 `M` 必須與函式指標類型 `F`相容（如上面所定義）。</span><span class="sxs-lookup"><span data-stu-id="b2616-162">The selected method `M` must be compatible (as defined above) with the function pointer type `F`.</span></span> <span data-ttu-id="b2616-163">否則，會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="b2616-163">Otherwise, a compile-time error occurs.</span></span>
- <span data-ttu-id="b2616-164">轉換的結果是 `F`類型的函式指標。</span><span class="sxs-lookup"><span data-stu-id="b2616-164">The result of the conversion is a function pointer of type `F`.</span></span>

<span data-ttu-id="b2616-165">隱含的轉換存在於其目標為方法群組的運算式位址，如果 `E`中只有一個靜態方法 `M`，則會 `E` `void*`。</span><span class="sxs-lookup"><span data-stu-id="b2616-165">An implicit conversion exists from an address-of expression whose target is a method group `E` to `void*` if there is only one static method `M` in `E`.</span></span>
<span data-ttu-id="b2616-166">如果有一個靜態方法，則會 `M``E` 的單一最佳方法。</span><span class="sxs-lookup"><span data-stu-id="b2616-166">If there is one static method, then the single best method from `E` is `M`.</span></span>
<span data-ttu-id="b2616-167">否則，會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="b2616-167">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="b2616-168">這表示開發人員可以相依于多載解析規則，以搭配使用位址運算子：</span><span class="sxs-lookup"><span data-stu-id="b2616-168">This means developers can depend on overload resolution rules to work in conjunction with the address-of operator:</span></span>

``` csharp
unsafe class Util {
    public static void Log() { }
    public static void Log(string p1) { }
    public static void Log(int i) { };

    void Use() {
        delegate*<void> a1 = &Log; // Log()
        delegate*<int, void> a2 = &Log; // Log(int i)

        // Error: ambiguous conversion from method group Log to "void*"
        void* v = &Log;
    }
```

<span data-ttu-id="b2616-169">將使用 `ldftn` 指令來實作為位址的運算子。</span><span class="sxs-lookup"><span data-stu-id="b2616-169">The address-of operator will be implemented using the `ldftn` instruction.</span></span>

<span data-ttu-id="b2616-170">這項功能的限制：</span><span class="sxs-lookup"><span data-stu-id="b2616-170">Restrictions of this feature:</span></span>

- <span data-ttu-id="b2616-171">僅適用于標示為 `static`的方法。</span><span class="sxs-lookup"><span data-stu-id="b2616-171">Only applies to methods marked as `static`.</span></span>
- <span data-ttu-id="b2616-172">非`static` 的區域函式不能用在 `&`中。</span><span class="sxs-lookup"><span data-stu-id="b2616-172">Non-`static` local functions cannot be used in `&`.</span></span> <span data-ttu-id="b2616-173">語言不會指定這些方法的執行詳細資料。</span><span class="sxs-lookup"><span data-stu-id="b2616-173">The implementation details of these methods are deliberately not specified by the language.</span></span> <span data-ttu-id="b2616-174">這包括它們是靜態的還是實例，或是它們所發出的簽章。</span><span class="sxs-lookup"><span data-stu-id="b2616-174">This includes whether they are static vs. instance or exactly what signature they are emitted with.</span></span>

### <a name="better-function-member"></a><span data-ttu-id="b2616-175">更好的函式成員</span><span class="sxs-lookup"><span data-stu-id="b2616-175">Better function member</span></span>

<span data-ttu-id="b2616-176">較佳的函式成員規格將會變更，以包含下列程式程式碼：</span><span class="sxs-lookup"><span data-stu-id="b2616-176">The better function member specification will be changed to include the following line:</span></span>

> <span data-ttu-id="b2616-177">`delegate*` 比 `void*` 更具體</span><span class="sxs-lookup"><span data-stu-id="b2616-177">A `delegate*` is more specific than `void*`</span></span>

<span data-ttu-id="b2616-178">這表示可以在 `void*` 和 `delegate*` 上多載，但仍 sensibly 使用 address 運算子。</span><span class="sxs-lookup"><span data-stu-id="b2616-178">This means that it is possible to overload on `void*` and a `delegate*` and still sensibly use the address-of operator.</span></span>

## <a name="open-issues"></a><span data-ttu-id="b2616-179">開啟問題</span><span class="sxs-lookup"><span data-stu-id="b2616-179">Open Issues</span></span>

### <a name="nativecallableattribute"></a><span data-ttu-id="b2616-180">NativeCallableAttribute</span><span class="sxs-lookup"><span data-stu-id="b2616-180">NativeCallableAttribute</span></span>

<span data-ttu-id="b2616-181">這是 CLR 在叫用時，用來避免 managed 至原生序言的屬性。</span><span class="sxs-lookup"><span data-stu-id="b2616-181">This is an attribute used by the CLR to avoid the managed to native prologue when invoking.</span></span> <span data-ttu-id="b2616-182">這個屬性所標記的方法只能從機器碼呼叫，而不是 managed （無法呼叫方法、建立委派等 ...）。</span><span class="sxs-lookup"><span data-stu-id="b2616-182">Methods marked by this attribute are only callable from native code, not managed (can’t call methods, create a delegate, etc …).</span></span> <span data-ttu-id="b2616-183">屬性對 mscorlib 而言並非特別的;執行時間會將具有相同名稱的任何屬性視為具有相同的語法。</span><span class="sxs-lookup"><span data-stu-id="b2616-183">The attribute is not special to mscorlib; the runtime will treat any attribute with this name with the same semantics.</span></span>

<span data-ttu-id="b2616-184">執行時間和語言可以搭配使用，以完全支援這種方式。</span><span class="sxs-lookup"><span data-stu-id="b2616-184">It's possible for the runtime and language to work together to fully support this.</span></span> <span data-ttu-id="b2616-185">語言可以選擇將具有 `NativeCallable` 屬性的 `static` 成員視為具有指定呼叫慣例的 `delegate*`。</span><span class="sxs-lookup"><span data-stu-id="b2616-185">The language could choose to treat address-of `static` members with a `NativeCallable` attribute as a `delegate*` with the specified calling convention.</span></span>

``` csharp
unsafe class NativeCallableExample {
    [NativeCallable(CallingConvention.CDecl)]
    static void CloseHandle(IntPtr p) => Marshal.FreeHGlobal(p);

    void Use() {
        delegate*<IntPtr, void> p1 = &CloseHandle; // Error: Invalid calling convention

        delegate* cdecl<IntPtr, void> p2 = &CloseHandle; // Okay
    }
}

```

<span data-ttu-id="b2616-186">此外，語言可能也會想要：</span><span class="sxs-lookup"><span data-stu-id="b2616-186">Additionally the language would likely also want to:</span></span>

- <span data-ttu-id="b2616-187">將以 `NativeCallable` 標記之方法的任何 managed 呼叫，標示為錯誤。</span><span class="sxs-lookup"><span data-stu-id="b2616-187">Flag any managed calls to a method tagged with `NativeCallable` as an error.</span></span> <span data-ttu-id="b2616-188">假設無法從 managed 程式碼叫用函式，編譯器應該會防止開發人員嘗試這樣的調用。</span><span class="sxs-lookup"><span data-stu-id="b2616-188">Given the function can't be invoked from managed code the compiler should prevent developers from attempting such an invocation.</span></span>
- <span data-ttu-id="b2616-189">當方法標記為 `NativeCallable`時，防止方法群組轉換為 `delegate`。</span><span class="sxs-lookup"><span data-stu-id="b2616-189">Prevent method group conversions to `delegate` when the method is tagged with `NativeCallable`.</span></span>

<span data-ttu-id="b2616-190">不過，這不是支援 `NativeCallable` 的必要動作。</span><span class="sxs-lookup"><span data-stu-id="b2616-190">This is not necessary to support `NativeCallable` though.</span></span> <span data-ttu-id="b2616-191">編譯器可以使用現有的語法來支援 `NativeCallable` 屬性。</span><span class="sxs-lookup"><span data-stu-id="b2616-191">The compiler can support the `NativeCallable` attribute as is using the existing syntax.</span></span> <span data-ttu-id="b2616-192">程式只需要在轉換成正確的 `delegate*` 簽章之前，轉型為 `void*`。</span><span class="sxs-lookup"><span data-stu-id="b2616-192">The program would simply need to cast to `void*` before casting to the correct `delegate*` signature.</span></span> <span data-ttu-id="b2616-193">這不會比今天的支援更糟。</span><span class="sxs-lookup"><span data-stu-id="b2616-193">That would be no worse than the support today.</span></span>

``` csharp
void* v = &CloseHandle;
delegate* cdecl<IntPtr, bool> f1 = (delegate* cdecl<IntPtr, bool>)v;
```

### <a name="extensible-set-of-unmanaged-calling-conventions"></a><span data-ttu-id="b2616-194">一組可擴充的非受控呼叫慣例</span><span class="sxs-lookup"><span data-stu-id="b2616-194">Extensible set of unmanaged calling conventions</span></span>

<span data-ttu-id="b2616-195">目前 ECMA-335 編碼所支援的一組非受控呼叫慣例已過期。</span><span class="sxs-lookup"><span data-stu-id="b2616-195">The set of unmanaged calling conventions supported by the current ECMA-335 encodings is outdated.</span></span> <span data-ttu-id="b2616-196">我們已看到新增支援更多非受控呼叫慣例的要求，例如：</span><span class="sxs-lookup"><span data-stu-id="b2616-196">We have seen requests to add support for more unmanaged calling conventions, for example:</span></span>

- <span data-ttu-id="b2616-197">[vectorcall](https://docs.microsoft.com/cpp/cpp/vectorcall) https://github.com/dotnet/coreclr/issues/12120</span><span class="sxs-lookup"><span data-stu-id="b2616-197">[vectorcall](https://docs.microsoft.com/cpp/cpp/vectorcall) https://github.com/dotnet/coreclr/issues/12120</span></span>
- <span data-ttu-id="b2616-198">明確 StdCall 此 https://github.com/dotnet/coreclr/pull/23974#issuecomment-482991750</span><span class="sxs-lookup"><span data-stu-id="b2616-198">StdCall with explicit this https://github.com/dotnet/coreclr/pull/23974#issuecomment-482991750</span></span>

<span data-ttu-id="b2616-199">這項功能的設計應允許在未來視需要擴充非受控呼叫慣例的集合。</span><span class="sxs-lookup"><span data-stu-id="b2616-199">The design of this feature should allow extending the set of unmanaged calling conventions as needed in future.</span></span> <span data-ttu-id="b2616-200">這些問題包含用於編碼呼叫慣例的有限空間（`IMAGE_CEE_CS_CALLCONV_MASK`中的12個值中所採用），以及需要觸及以加入新呼叫慣例的位置數目。</span><span class="sxs-lookup"><span data-stu-id="b2616-200">The problems include limited space for encoding calling conventions (12 out of 16 values are taken in `IMAGE_CEE_CS_CALLCONV_MASK`) and number of places that need to be touched in order to add a new calling convention.</span></span> <span data-ttu-id="b2616-201">可能的解決方法是使用[`System.Runtime.InteropServices.CallingConvention`](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.callingconvention)列舉來引進代表呼叫慣例的新編碼方式。</span><span class="sxs-lookup"><span data-stu-id="b2616-201">A potential solution is to introduce a new encoding that represents the calling convention using [`System.Runtime.InteropServices.CallingConvention`](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.callingconvention) enum.</span></span>

<span data-ttu-id="b2616-202">如需參考， https://github.com/llvm/llvm-project/blob/master/llvm/include/llvm/IR/CallingConv.h 具有 LLVM 支援的呼叫慣例清單。</span><span class="sxs-lookup"><span data-stu-id="b2616-202">For reference, https://github.com/llvm/llvm-project/blob/master/llvm/include/llvm/IR/CallingConv.h has the list of calling conventions supported by LLVM.</span></span> <span data-ttu-id="b2616-203">雖然 .NET 不太可能需要支援所有的專案，但它會示範呼叫慣例的空間非常豐富。</span><span class="sxs-lookup"><span data-stu-id="b2616-203">While it is unlikely that .NET will ever need to support all of them, it demonstrates that the space of calling conventions is very rich.</span></span>

## <a name="considerations"></a><span data-ttu-id="b2616-204">考量</span><span class="sxs-lookup"><span data-stu-id="b2616-204">Considerations</span></span>

### <a name="allow-instance-methods"></a><span data-ttu-id="b2616-205">允許實例方法</span><span class="sxs-lookup"><span data-stu-id="b2616-205">Allow instance methods</span></span>

<span data-ttu-id="b2616-206">您可以利用 `EXPLICITTHIS` CLI 呼叫慣例（在程式碼中C#名為 `instance`），擴充提案以支援實例方法。</span><span class="sxs-lookup"><span data-stu-id="b2616-206">The proposal could be extended to support instance methods by taking advantage of the `EXPLICITTHIS` CLI calling convention (named `instance` in C# code).</span></span> <span data-ttu-id="b2616-207">這種形式的 CLI 函式指標會將 `this` 參數放成函數指標語法的明確第一個參數。</span><span class="sxs-lookup"><span data-stu-id="b2616-207">This form of CLI function pointers puts the `this` parameter as an explicit first parameter of the function pointer syntax.</span></span>

``` csharp
unsafe class Instance {
    void Use() {
        delegate* instance<Instance, string> f = &ToString;
        f(this);
    }
}
```

<span data-ttu-id="b2616-208">這聽起來很好，但會對提案加上一些複雜的部分。</span><span class="sxs-lookup"><span data-stu-id="b2616-208">This is sound but adds some complication to the proposal.</span></span> <span data-ttu-id="b2616-209">特別是因為呼叫慣例不同的函式指標 `instance` 和 `managed` 會是不相容的，即使這兩個案例都是用來叫C#用具有相同簽章的 managed 方法也一樣。</span><span class="sxs-lookup"><span data-stu-id="b2616-209">Particularly because function pointers which differed by the calling convention `instance` and `managed` would be incompatible even though both cases are used to invoke managed methods with the same C# signature.</span></span> <span data-ttu-id="b2616-210">此外，在每個案例中，如果有一個簡單的解決方法，這會是很有説明的：使用 `static` 區域函數。</span><span class="sxs-lookup"><span data-stu-id="b2616-210">Also in every case considered where this would be valuable to have there was a simple work around: use a `static` local function.</span></span>

``` csharp
unsafe class Instance {
    void Use() {
        static string toString(Instance i) = i.ToString();
        delgate*<Instance, string> f = &toString;
        f(this);
    }
}
```

### <a name="dont-require-unsafe-at-declaration"></a><span data-ttu-id="b2616-211">宣告時不需要 unsafe</span><span class="sxs-lookup"><span data-stu-id="b2616-211">Don't require unsafe at declaration</span></span>

<span data-ttu-id="b2616-212">不需要在每次使用 `delegate*`時都要求 `unsafe`，而是只在將方法群組轉換成 `delegate*`時才需要它。</span><span class="sxs-lookup"><span data-stu-id="b2616-212">Instead of requiring `unsafe` at every use of a `delegate*`, only require it at the point where a method group is converted to a `delegate*`.</span></span> <span data-ttu-id="b2616-213">這就是核心安全問題的運作位置（知道包含的元件無法在值為「作用中」時卸載）。</span><span class="sxs-lookup"><span data-stu-id="b2616-213">This is where the core safety issues come into play (knowing that the containing assembly cannot be unloaded while the value is alive).</span></span> <span data-ttu-id="b2616-214">在其他位置上需要 `unsafe` 可能會被視為過度。</span><span class="sxs-lookup"><span data-stu-id="b2616-214">Requiring `unsafe` on the other locations can be seen as excessive.</span></span>

<span data-ttu-id="b2616-215">這就是設計原本的預期方式。</span><span class="sxs-lookup"><span data-stu-id="b2616-215">This is how the design was originally intended.</span></span> <span data-ttu-id="b2616-216">但是產生的語言規則覺得非常難以用。</span><span class="sxs-lookup"><span data-stu-id="b2616-216">But the resulting language rules felt very awkward.</span></span> <span data-ttu-id="b2616-217">不可能隱藏這是指標值，即使沒有 `unsafe` 關鍵字，它還是會繼續查看。</span><span class="sxs-lookup"><span data-stu-id="b2616-217">It's impossible to hide the fact that this is a pointer value and it kept peeking through even without the `unsafe` keyword.</span></span> <span data-ttu-id="b2616-218">例如，不允許轉換為 `object`，它不能是 `class`的成員等等 .。。其C#設計是要求所有指標使用 `unsafe`，因此這種設計會遵循這種方式。</span><span class="sxs-lookup"><span data-stu-id="b2616-218">For example the conversion to `object` can't be allowed, it can't be a member of a `class`, etc ... The C# design is to require `unsafe` for all pointer uses and hence this design follows that.</span></span>

<span data-ttu-id="b2616-219">開發人員仍然能夠在 `delegate*` 的值之上呈現_安全_的包裝函式，就像今天一般的指標類型一樣。</span><span class="sxs-lookup"><span data-stu-id="b2616-219">Developers will still be capable of presenting a _safe_ wrapper on top of `delegate*` values the same way that they do for normal pointer types today.</span></span> <span data-ttu-id="b2616-220">請考慮：</span><span class="sxs-lookup"><span data-stu-id="b2616-220">Consider:</span></span>

``` csharp
unsafe struct Action {
    delegate*<void> _ptr;

    Action(delegate*<void> ptr) => _ptr = ptr;
    public void Invoke() => _ptr();
}
```

### <a name="using-delegates"></a><span data-ttu-id="b2616-221">使用委派</span><span class="sxs-lookup"><span data-stu-id="b2616-221">Using delegates</span></span>

<span data-ttu-id="b2616-222">不使用新的語法元素 `delegate*`，只要使用現有的 `delegate` 類型，並在類型後面加上 `*` 即可：</span><span class="sxs-lookup"><span data-stu-id="b2616-222">Instead of using a new syntax element, `delegate*`, simply use existing `delegate` types with a `*` following the type:</span></span>

``` csharp
Func<object, object, bool>* ptr = &object.ReferenceEquals;
```

<span data-ttu-id="b2616-223">藉由使用指定 `CallingConvention` 值的屬性來標注 `delegate` 類型，即可完成處理呼叫慣例。</span><span class="sxs-lookup"><span data-stu-id="b2616-223">Handling calling convention can be done by annotating the `delegate` types with an attribute that specifies a `CallingConvention` value.</span></span> <span data-ttu-id="b2616-224">缺少屬性會表示 managed 呼叫慣例。</span><span class="sxs-lookup"><span data-stu-id="b2616-224">The lack of an attribute would signify the managed calling convention.</span></span>

<span data-ttu-id="b2616-225">在 IL 中編碼此項會造成問題。</span><span class="sxs-lookup"><span data-stu-id="b2616-225">Encoding this in IL is problematic.</span></span> <span data-ttu-id="b2616-226">基礎值必須以指標表示，但它也需要：</span><span class="sxs-lookup"><span data-stu-id="b2616-226">The underlying value needs to be represented as a pointer yet it also must:</span></span>

1. <span data-ttu-id="b2616-227">具有唯一的類型，可允許具有不同函式指標類型的多載。</span><span class="sxs-lookup"><span data-stu-id="b2616-227">Have a unique type to allow for overloads with different function pointer types.</span></span>
1. <span data-ttu-id="b2616-228">適用于跨元件界限的 OHI 用途。</span><span class="sxs-lookup"><span data-stu-id="b2616-228">Be equivalent for OHI purposes across assembly boundaries.</span></span>

<span data-ttu-id="b2616-229">最後一點特別有問題。</span><span class="sxs-lookup"><span data-stu-id="b2616-229">The last point is particularly problematic.</span></span> <span data-ttu-id="b2616-230">這表示使用 `Func<int>*` 的每個元件都必須在中繼資料中編碼對等的類型，即使 `Func<int>*` 是在元件中定義的，仍不會受到控制。</span><span class="sxs-lookup"><span data-stu-id="b2616-230">This mean that every assembly which uses `Func<int>*` must encode an equivalent type in metadata even though `Func<int>*` is defined in an assembly though don't control.</span></span>
<span data-ttu-id="b2616-231">此外，在非 mscorlib 的元件中，以名稱 `System.Func<T>` 定義的其他類型，必須與在 mscorlib.dll 中定義的版本不同。</span><span class="sxs-lookup"><span data-stu-id="b2616-231">Additionally any other type which is defined with the name `System.Func<T>` in an assembly that is not mscorlib must be different than the version defined in mscorlib.</span></span>

<span data-ttu-id="b2616-232">探索到的一個選項是將這類指標發出為 `mod_req(Func<int>) void*`。</span><span class="sxs-lookup"><span data-stu-id="b2616-232">One option that was explored was emitting such a pointer as `mod_req(Func<int>) void*`.</span></span> <span data-ttu-id="b2616-233">不過，如果 `mod_req` 無法系結至 `TypeSpec`，因而無法以一般具現化為目標，就無法使用這種方式。</span><span class="sxs-lookup"><span data-stu-id="b2616-233">This doesn't work though as a `mod_req` cannot bind to a `TypeSpec` and hence cannot target generic instantiations.</span></span>

### <a name="named-function-pointers"></a><span data-ttu-id="b2616-234">命名函式指標</span><span class="sxs-lookup"><span data-stu-id="b2616-234">Named function pointers</span></span>

<span data-ttu-id="b2616-235">函式指標語法可能很麻煩，特別是在複雜的情況下，例如嵌套函式指標。</span><span class="sxs-lookup"><span data-stu-id="b2616-235">The function pointer syntax can be cumbersome, particularly in complex cases like nested function pointers.</span></span> <span data-ttu-id="b2616-236">而不是讓開發人員在每次語言允許函式指標的命名宣告時，都輸入簽章，如同使用 `delegate`所做的一樣。</span><span class="sxs-lookup"><span data-stu-id="b2616-236">Rather than have developers type out the signature every time the language could allow for named declarations of function pointers as is done with `delegate`.</span></span>

``` csharp
func* void Action();

unsafe class NamedExample {
    void M(Action a) {
        a();
    }
}
```

<span data-ttu-id="b2616-237">這裡的問題之一是基礎 CLI 基本類型沒有名稱，因此這純粹是C#家發明，而且需要一些中繼資料工作才能啟用。</span><span class="sxs-lookup"><span data-stu-id="b2616-237">Part of the problem here is the underlying CLI primitive doesn't have names hence this would be purely a C# invention and require a bit of metadata work to enable.</span></span> <span data-ttu-id="b2616-238">這是雖可行的，但對工作來說相當重要。</span><span class="sxs-lookup"><span data-stu-id="b2616-238">That is doable but is a significant about of work.</span></span> <span data-ttu-id="b2616-239">基本上，它C#只需要為這些名稱提供類型 def 資料表的隨附。</span><span class="sxs-lookup"><span data-stu-id="b2616-239">It essentially requires C# to have a companion to the type def table purely for these names.</span></span>

<span data-ttu-id="b2616-240">此外，當已檢查命名函式指標的引數時，我們發現它們可以同樣適用于許多其他案例。</span><span class="sxs-lookup"><span data-stu-id="b2616-240">Also when the arguments for named function pointers were examined we found they could apply equally well to a number of other scenarios.</span></span> <span data-ttu-id="b2616-241">比方說，宣告命名的元組是很方便的，因為在所有情況下都不需要輸入完整的簽章。</span><span class="sxs-lookup"><span data-stu-id="b2616-241">For example it would be just as convenient to declare named tuples to reduce the need to type out the full signature in all cases.</span></span>

``` csharp
(int x, int y) Point;

class NamedTupleExample {
    void M(Point p) {
        Console.WriteLine(p.x);
    }
}
```

<span data-ttu-id="b2616-242">討論之後，我們決定不允許 `delegate*` 類型的命名宣告。</span><span class="sxs-lookup"><span data-stu-id="b2616-242">After discussion we decided to not allow named declaration of `delegate*` types.</span></span> <span data-ttu-id="b2616-243">如果我們發現，根據客戶的使用意見反應，有很大的需求，我們將調查適用于函式指標、元組、泛型等的命名解決方案。這可能類似于其他建議的格式，例如語言中的完整 `typedef` 支援。</span><span class="sxs-lookup"><span data-stu-id="b2616-243">If we find there is significant need for this based on customer usage feedback then we will investigate a naming solution that works for function pointers, tuples, generics, etc ... This is likely to be similar in form to other suggestions like full `typedef` support in the language.</span></span>

## <a name="future-considerations"></a><span data-ttu-id="b2616-244">未來的考量</span><span class="sxs-lookup"><span data-stu-id="b2616-244">Future Considerations</span></span>

### <a name="static-local-functions"></a><span data-ttu-id="b2616-245">靜態區域函式</span><span class="sxs-lookup"><span data-stu-id="b2616-245">static local functions</span></span>

<span data-ttu-id="b2616-246">這是指允許在區域函式上進行 `static` 修飾詞[的提案](https://github.com/dotnet/csharplang/issues/1565)。</span><span class="sxs-lookup"><span data-stu-id="b2616-246">This refers to [the proposal](https://github.com/dotnet/csharplang/issues/1565) to allow the `static` modifier on local functions.</span></span> <span data-ttu-id="b2616-247">這類函式會保證會當做 `static` 發出，並以原始程式碼中指定的完整簽章。</span><span class="sxs-lookup"><span data-stu-id="b2616-247">Such a function would be guaranteed to be emitted as `static` and with the exact signature specified in source code.</span></span> <span data-ttu-id="b2616-248">這類函式應該是 `&` 的有效引數，因為它包含目前沒有任何問題的本機函式</span><span class="sxs-lookup"><span data-stu-id="b2616-248">Such a function should be a valid argument to `&` as it contains none of the problems local functions have today</span></span>

### <a name="static-delegates"></a><span data-ttu-id="b2616-249">靜態委派</span><span class="sxs-lookup"><span data-stu-id="b2616-249">static delegates</span></span>

<span data-ttu-id="b2616-250">這是指允許宣告 `delegate` 型別的[提案](https://github.com/dotnet/csharplang/issues/302)，而這些型別只能參考 `static` 的成員。</span><span class="sxs-lookup"><span data-stu-id="b2616-250">This refers to [the proposal](https://github.com/dotnet/csharplang/issues/302) to allow for the declaration of `delegate` types which can only refer to `static` members.</span></span> <span data-ttu-id="b2616-251">其優點是這類 `delegate` 實例可以在效能敏感性案例中，免費和更好的配置。</span><span class="sxs-lookup"><span data-stu-id="b2616-251">The advantage being that such `delegate` instances can be allocation free and better in performance sensitive scenarios.</span></span>

<span data-ttu-id="b2616-252">如果函式指標功能已實作為，則可能會關閉 `static delegate` 提案。該功能的提議優點是配置的免費特性。</span><span class="sxs-lookup"><span data-stu-id="b2616-252">If the function pointer feature is implemented the `static delegate` proposal will likely be closed out. The proposed advantage of that feature is the allocation free nature.</span></span> <span data-ttu-id="b2616-253">不過，最近的調查發現，因為元件卸載而無法達到此目的。</span><span class="sxs-lookup"><span data-stu-id="b2616-253">However recent investigations have found that is not possible to achieve due to assembly unloading.</span></span> <span data-ttu-id="b2616-254">`static delegate` 中必須要有一個強式控制碼，才能從它所參考的方法中將元件卸載。</span><span class="sxs-lookup"><span data-stu-id="b2616-254">There must be a strong handle from the `static delegate` to the method it refers to in order to keep the assembly from being unloaded out from under it.</span></span>

<span data-ttu-id="b2616-255">若要維護每個 `static delegate` 實例，您必須配置新的控制碼，以執行此提議的目標計數器。</span><span class="sxs-lookup"><span data-stu-id="b2616-255">To maintain every `static delegate` instance would be required to allocate a new handle which runs counter to the goals of the proposal.</span></span> <span data-ttu-id="b2616-256">有一些設計可將配置分攤到每個呼叫網站的單一配置，但這有點複雜，而且似乎不值得取捨。</span><span class="sxs-lookup"><span data-stu-id="b2616-256">There were some designs where the allocation could be amortized to a single allocation per call-site but that was a bit complex and didn't seem worth the trade off.</span></span>

<span data-ttu-id="b2616-257">這表示開發人員基本上必須在下列取捨之間做決定：</span><span class="sxs-lookup"><span data-stu-id="b2616-257">That means developers essentially have to decide between the following trade offs:</span></span>

1. <span data-ttu-id="b2616-258">元件卸載的安全性：這需要配置，因此 `delegate` 已經是足夠的選項。</span><span class="sxs-lookup"><span data-stu-id="b2616-258">Safety in the face of assembly unloading: this requires allocations and hence `delegate` is already a sufficient option.</span></span>
1. <span data-ttu-id="b2616-259">元件卸載沒有任何安全性：使用 `delegate*`。</span><span class="sxs-lookup"><span data-stu-id="b2616-259">No safety in face of assembly unloading: use a `delegate*`.</span></span> <span data-ttu-id="b2616-260">這可以包裝在 `struct` 中，以允許在其餘程式碼中的 `unsafe` 內容之外使用。</span><span class="sxs-lookup"><span data-stu-id="b2616-260">This can be wrapped in a `struct` to allow usage outside an `unsafe` context in the rest of the code.</span></span>
