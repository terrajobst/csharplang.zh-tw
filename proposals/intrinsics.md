---
ms.openlocfilehash: ac4c8760e3b6a0934f01ae634f666af60aa1c0fe
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484563"
---
# <a name="compiler-intrinsics"></a><span data-ttu-id="563b6-101">編譯器內建</span><span class="sxs-lookup"><span data-stu-id="563b6-101">Compiler Intrinsics</span></span>

## <a name="summary"></a><span data-ttu-id="563b6-102">摘要</span><span class="sxs-lookup"><span data-stu-id="563b6-102">Summary</span></span>

<span data-ttu-id="563b6-103">此提案提供的語言結構會公開目前無法有效率地存取的低層級 IL opcode，或完全： `ldftn`、`ldvirtftn`、`ldtoken` 和 `calli`。</span><span class="sxs-lookup"><span data-stu-id="563b6-103">This proposal provides language constructs that expose low level IL opcodes that cannot currently be accessed efficiently, or at all: `ldftn`, `ldvirtftn`, `ldtoken` and `calli`.</span></span> <span data-ttu-id="563b6-104">在高效能程式碼中，這些低層級的 opcode 可能很重要，而開發人員需要有效率的方式來存取它們。</span><span class="sxs-lookup"><span data-stu-id="563b6-104">These low level opcodes can be important in high performance code and developers need an efficient way to access them.</span></span>

## <a name="motivation"></a><span data-ttu-id="563b6-105">動機</span><span class="sxs-lookup"><span data-stu-id="563b6-105">Motivation</span></span>

<span data-ttu-id="563b6-106">下列問題會說明這項功能的動機和背景（這是功能的可能執行）：</span><span class="sxs-lookup"><span data-stu-id="563b6-106">The motivations and background for this feature are described in the following issue (as is a potential implementation of the feature):</span></span> 

https://github.com/dotnet/csharplang/issues/191

<span data-ttu-id="563b6-107">這項替代設計提案會在複習原始提案的原型執行後，透過 @msjabby 以及整個重要程式碼基底的使用方式來完成。</span><span class="sxs-lookup"><span data-stu-id="563b6-107">This alternate design proposal comes after reviewing a prototype implementation of the original proposal by @msjabby as well as the use throughout a significant code base.</span></span> <span data-ttu-id="563b6-108">這種設計是透過 @mjsabby、@tmat 和 @jkotas的大量輸入進行。</span><span class="sxs-lookup"><span data-stu-id="563b6-108">This design was done with significant input from @mjsabby, @tmat and @jkotas.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="563b6-109">詳細設計</span><span class="sxs-lookup"><span data-stu-id="563b6-109">Detailed Design</span></span> 

### <a name="allow-address-of-to-target-methods"></a><span data-ttu-id="563b6-110">允許目標方法的位址</span><span class="sxs-lookup"><span data-stu-id="563b6-110">Allow address of to target methods</span></span>

<span data-ttu-id="563b6-111">方法群組現在將允許當做位址運算式的引數。</span><span class="sxs-lookup"><span data-stu-id="563b6-111">Method groups will now be allowed as arguments to an address-of expression.</span></span> <span data-ttu-id="563b6-112">這類運算式的類型將會 `void*`。</span><span class="sxs-lookup"><span data-stu-id="563b6-112">The type of such an expression will be `void*`.</span></span> 

``` csharp
class Util { 
    public static void Log() { } 
}

// ldftn Util.Log
void* ptr = &Util.Log; 
```

<span data-ttu-id="563b6-113">假設這裡沒有委派轉換，在方法群組中篩選成員的唯一機制是透過靜態/實例存取。</span><span class="sxs-lookup"><span data-stu-id="563b6-113">Given there is no delegate conversion here the only mechanism for filtering members in the method group is by static / instance access.</span></span> <span data-ttu-id="563b6-114">如果無法區分成員，則會發生編譯時期錯誤。</span><span class="sxs-lookup"><span data-stu-id="563b6-114">If that cannot distinguish the members then a compile time error will occur.</span></span>

``` csharp
class Util { 
    public void Log() { } 
    public void Log(string p1) { } 
    public static void Log(int i) { };
}

unsafe {
    // Error: Method group Log has more than one applicable candidate.
    void* ptr1 = &Log; 

    // Okay: only one static member to consider here.
    void* ptr2 = &Util.Log;
}
```

<span data-ttu-id="563b6-115">此內容中的 addressof 運算式會以下列方式執行：</span><span class="sxs-lookup"><span data-stu-id="563b6-115">The addressof expression in this context will be implemented in the following manner:</span></span>

- <span data-ttu-id="563b6-116">ldftn：當此方法為非虛擬時。</span><span class="sxs-lookup"><span data-stu-id="563b6-116">ldftn: when the method is non-virtual.</span></span>
- <span data-ttu-id="563b6-117">ldvirtftn：當方法為虛擬時。</span><span class="sxs-lookup"><span data-stu-id="563b6-117">ldvirtftn: when the method is virtual.</span></span>

<span data-ttu-id="563b6-118">這項功能的限制：</span><span class="sxs-lookup"><span data-stu-id="563b6-118">Restrictions of this feature:</span></span>

- <span data-ttu-id="563b6-119">只有在對值使用調用運算式時，才能指定實例方法</span><span class="sxs-lookup"><span data-stu-id="563b6-119">Instance methods can only be specified when using an invocation expression on a value</span></span>
- <span data-ttu-id="563b6-120">區域函數不能用在 `&`中。</span><span class="sxs-lookup"><span data-stu-id="563b6-120">Local functions cannot be used in `&`.</span></span> <span data-ttu-id="563b6-121">語言不會指定這些方法的執行詳細資料。</span><span class="sxs-lookup"><span data-stu-id="563b6-121">The implementation details of these methods are deliberately not specified by the language.</span></span> <span data-ttu-id="563b6-122">這包括它們是靜態的還是實例，或是它們所發出的簽章。</span><span class="sxs-lookup"><span data-stu-id="563b6-122">This includes whether they are static vs. instance or exactly what signature they are emitted with.</span></span>

### <a name="handleof"></a><span data-ttu-id="563b6-123">handleof</span><span class="sxs-lookup"><span data-stu-id="563b6-123">handleof</span></span>

<span data-ttu-id="563b6-124">`handleof` 內容關鍵字會使用 `ldtoken` 指令，將欄位、成員或類型轉譯為其對等的 `RuntimeHandle` 類型。</span><span class="sxs-lookup"><span data-stu-id="563b6-124">The `handleof` contextual keyword will translate a field, member or type into their equivalent `RuntimeHandle` type using the `ldtoken` instruction.</span></span> <span data-ttu-id="563b6-125">運算式的確切類型將取決於 `handleof`中的名稱類型：</span><span class="sxs-lookup"><span data-stu-id="563b6-125">The exact type of the expression will depend on the kind of the name in `handleof`:</span></span>

- <span data-ttu-id="563b6-126">欄位： `RuntimeFieldHandle`</span><span class="sxs-lookup"><span data-stu-id="563b6-126">field: `RuntimeFieldHandle`</span></span>
- <span data-ttu-id="563b6-127">類型： `RuntimeTypeHandle`</span><span class="sxs-lookup"><span data-stu-id="563b6-127">type: `RuntimeTypeHandle`</span></span>
- <span data-ttu-id="563b6-128">方法： `RuntimeMethodHandle`</span><span class="sxs-lookup"><span data-stu-id="563b6-128">method: `RuntimeMethodHandle`</span></span>

<span data-ttu-id="563b6-129">`handleof` 的引數與 `nameof`相同。</span><span class="sxs-lookup"><span data-stu-id="563b6-129">The arguments to `handleof` are identical to `nameof`.</span></span> <span data-ttu-id="563b6-130">它必須是簡單名稱、限定名稱、成員存取、具有指定成員的基本存取權，或具有指定成員的此存取權。</span><span class="sxs-lookup"><span data-stu-id="563b6-130">It must be a simple name, qualified name, member access, base access with a specified member, or this access with a specified member.</span></span> <span data-ttu-id="563b6-131">引數運算式會識別程式碼定義，但永遠不會加以評估。</span><span class="sxs-lookup"><span data-stu-id="563b6-131">The argument expression identifies a code definition, but it is never evaluated.</span></span>

<span data-ttu-id="563b6-132">`handleof` 運算式會在執行時間進行評估，而且具有 `RuntimeHandle`的傳回類型。</span><span class="sxs-lookup"><span data-stu-id="563b6-132">The `handleof` expression is evaluated at runtime and has a return type of `RuntimeHandle`.</span></span> <span data-ttu-id="563b6-133">這可以在安全的程式碼和不安全的中執行。</span><span class="sxs-lookup"><span data-stu-id="563b6-133">This can be executed in safe code as well as unsafe.</span></span> 

``` 
RuntimeHandle stringHandle = handleof(string);
```

<span data-ttu-id="563b6-134">這項功能的限制：</span><span class="sxs-lookup"><span data-stu-id="563b6-134">Restrictions of this feature:</span></span>

- <span data-ttu-id="563b6-135">屬性不能用在 `handleof` 運算式中。</span><span class="sxs-lookup"><span data-stu-id="563b6-135">Properties cannot be used in a `handleof` expression.</span></span>
- <span data-ttu-id="563b6-136">當範圍內有現有的 `handleof` 名稱時，就無法使用 `handleof` 運算式。</span><span class="sxs-lookup"><span data-stu-id="563b6-136">The `handleof` expression cannot be used when there is an existing `handleof` name in scope.</span></span> <span data-ttu-id="563b6-137">例如，類型、命名空間等。</span><span class="sxs-lookup"><span data-stu-id="563b6-137">For example a type, namespace, etc ...</span></span>

### <a name="calli"></a><span data-ttu-id="563b6-138">calli</span><span class="sxs-lookup"><span data-stu-id="563b6-138">calli</span></span>

<span data-ttu-id="563b6-139">編譯器會新增新類型 `extern` 函數的支援，以有效率地轉譯成 `.calli` 指令。</span><span class="sxs-lookup"><span data-stu-id="563b6-139">The compiler will add support for a new type of `extern` function that efficiently translates into a `.calli` instruction.</span></span> <span data-ttu-id="563b6-140">Extern 屬性會以下列圖形的屬性標示：</span><span class="sxs-lookup"><span data-stu-id="563b6-140">The extern attribute will be marked with an attribute of the following shape:</span></span>

``` csharp
[AttributeUsage(AttributeTargets.Method)]
public sealed class CallIndirectAttribute : Attribute
{
    public CallingConvention CallingConvention { get; }
    public CallIndirectAttribute(CallingConvention callingConvention)
    {
        CallingConvention = callingConvention;
    }
}
```

<span data-ttu-id="563b6-141">這可讓開發人員以下列形式定義方法：</span><span class="sxs-lookup"><span data-stu-id="563b6-141">This allows developers to define methods in the following form:</span></span>

``` csharp
[CallIndirect(CallingConvention.Cdecl)]
static extern int MapValue(string s, void *ptr);

unsafe {
    var i = MapValue("42", &int.Parse);
    Console.WriteLine(i);
}
```

<span data-ttu-id="563b6-142">已套用 `CallIndirect` 屬性之方法的限制：</span><span class="sxs-lookup"><span data-stu-id="563b6-142">Restrictions on the method which has the `CallIndirect` attribute applied:</span></span>

- <span data-ttu-id="563b6-143">不能有 `DllImport` 屬性。</span><span class="sxs-lookup"><span data-stu-id="563b6-143">Cannot have a `DllImport` attribute.</span></span>
- <span data-ttu-id="563b6-144">不可以是泛型。</span><span class="sxs-lookup"><span data-stu-id="563b6-144">Cannot be generic.</span></span>

## <a name="open-issues"></a><span data-ttu-id="563b6-145">開啟問題</span><span class="sxs-lookup"><span data-stu-id="563b6-145">Open Issues</span></span>

### <a name="callingconvention"></a><span data-ttu-id="563b6-146">CallingConvention</span><span class="sxs-lookup"><span data-stu-id="563b6-146">CallingConvention</span></span>

<span data-ttu-id="563b6-147">所設計的 `CallIndirectAttribute` 會使用缺少受控呼叫慣例專案的 `CallingConvention` 列舉。</span><span class="sxs-lookup"><span data-stu-id="563b6-147">The `CallIndirectAttribute` as designed uses the `CallingConvention` enum which lacks an entry for managed calling conventions.</span></span> <span data-ttu-id="563b6-148">列舉必須擴充以包含此呼叫慣例，否則屬性需要採用不同的方法。</span><span class="sxs-lookup"><span data-stu-id="563b6-148">The enum either needs to be extended to include this calling convention or the attribute needs to take a different approach.</span></span>

## <a name="considerations"></a><span data-ttu-id="563b6-149">考量</span><span class="sxs-lookup"><span data-stu-id="563b6-149">Considerations</span></span>

### <a name="disambiguating-method-groups"></a><span data-ttu-id="563b6-150">厘清方法群組</span><span class="sxs-lookup"><span data-stu-id="563b6-150">Disambiguating method groups</span></span>

<span data-ttu-id="563b6-151">有一些功能討論，可讓您更輕鬆地區分傳遞給傳址運算式的方法群組。</span><span class="sxs-lookup"><span data-stu-id="563b6-151">There was some discussion around features that would make it easier to disambiguate method groups passed to an address-of expression.</span></span> <span data-ttu-id="563b6-152">例如，可能會將簽章元素新增至語法：</span><span class="sxs-lookup"><span data-stu-id="563b6-152">For instance potentially adding signature elements to the syntax:</span></span>

``` csharp
class Util {
    public static void Log() { ... }
    public static void Log(string) { ... }
}

unsafe {
    // Error: ambiguous Log
    void *ptr1 = &Util.Log;

    // Use Util.Log();
    void *ptr2 = &Util.Log();
}
```

<span data-ttu-id="563b6-153">這會被拒絕，因為您可能無法進行很引人注目的情況，也無法在此想像出簡單的語法。</span><span class="sxs-lookup"><span data-stu-id="563b6-153">This was rejected because a compelling case could not be made nor could a simple syntax be envisioned here.</span></span> <span data-ttu-id="563b6-154">另外還有一個相當直接的解決方法：簡單地定義另一個明確的方法，並C#使用程式碼呼叫所需的函式。</span><span class="sxs-lookup"><span data-stu-id="563b6-154">Also there is a fairly straight forward work around: simple define another method that is unambiguous and uses C# code to call into the desired function.</span></span> 

``` csharp
class Workaround {
    public static void LocalLog() => Util.Log();
}
unsafe { 
    void* ptr = &Workaround.LocalLog;
}
```

<span data-ttu-id="563b6-155">如果 `static` 區域函式進入語言，這會變得更簡單。</span><span class="sxs-lookup"><span data-stu-id="563b6-155">This becomes even simpler if `static` local functions enter the language.</span></span> <span data-ttu-id="563b6-156">然後，您可以在使用不明確的作業通訊的相同函式中定義因應的工作：</span><span class="sxs-lookup"><span data-stu-id="563b6-156">Then the work around could be defined in the same function that used the ambiguous address-of operation:</span></span>

``` csharp
unsafe { 
    static void LocalLog() => Util.Log();
    void* ptr = &Workaround.LocalLog;
}
```

### <a name="loadtypetokenint32"></a><span data-ttu-id="563b6-157">LoadTypeTokenInt32</span><span class="sxs-lookup"><span data-stu-id="563b6-157">LoadTypeTokenInt32</span></span>

<span data-ttu-id="563b6-158">在編譯時期，允許將元資料標記載入為 `int` 值的原始提案。</span><span class="sxs-lookup"><span data-stu-id="563b6-158">The original proposal allowed for metadata tokens to be loaded as `int` values at compile time.</span></span> <span data-ttu-id="563b6-159">基本上具有與 `handleof` 相同之引數的 `tokenof`，但會在編譯時期評估為 `int` 常數。</span><span class="sxs-lookup"><span data-stu-id="563b6-159">Essentially have `tokenof` that has the same arguments as `handleof` but is evaluated at compile time to an `int` constant.</span></span> 

<span data-ttu-id="563b6-160">這會遭到拒絕，因為它會造成 IL 重寫（其中 .NET 有許多）的嚴重問題。</span><span class="sxs-lookup"><span data-stu-id="563b6-160">This was rejected as it causes significant problem for IL rewrites (of which .NET has many).</span></span> <span data-ttu-id="563b6-161">這種 rewriters 通常會以可能會使這些值失效的方式來操作中繼資料資料表。</span><span class="sxs-lookup"><span data-stu-id="563b6-161">Such rewriters often manipulate the metadata tables in a way that could invalidate these values.</span></span> <span data-ttu-id="563b6-162">當這類 rewriters 儲存為簡單 `int` 值時，沒有合理的方式可更新這些值。</span><span class="sxs-lookup"><span data-stu-id="563b6-162">There is no reasonable way for such rewriters to update these values when they are stored as simple `int` values.</span></span>

<span data-ttu-id="563b6-163">具有中繼資料專案之不透明控制碼的基本概念，將繼續由執行時間小組探索。</span><span class="sxs-lookup"><span data-stu-id="563b6-163">The underlying idea of having an opaque handle for metadata entries will continue to be explored by the runtime team.</span></span> 

## <a name="future-considerations"></a><span data-ttu-id="563b6-164">未來的考量</span><span class="sxs-lookup"><span data-stu-id="563b6-164">Future Considerations</span></span>

### <a name="static-local-functions"></a><span data-ttu-id="563b6-165">靜態區域函式</span><span class="sxs-lookup"><span data-stu-id="563b6-165">static local functions</span></span>

<span data-ttu-id="563b6-166">這是指允許在區域函式上進行 `static` 修飾詞[的提案](https://github.com/dotnet/csharplang/issues/1565)。</span><span class="sxs-lookup"><span data-stu-id="563b6-166">This refers to [the proposal](https://github.com/dotnet/csharplang/issues/1565) to allow the `static` modifier on local functions.</span></span> <span data-ttu-id="563b6-167">這類函式會保證會當做 `static` 發出，並以原始程式碼中指定的完整簽章。</span><span class="sxs-lookup"><span data-stu-id="563b6-167">Such a function would be guaranteed to be emitted as `static` and with the exact signature specified in source code.</span></span> <span data-ttu-id="563b6-168">這類函式應該是 `&` 的有效引數，因為它不包含本機函式目前所沒有的任何問題。</span><span class="sxs-lookup"><span data-stu-id="563b6-168">Such a function should be a valid argument to `&` as it contains none of the problems local functions have today.</span></span>

### <a name="nativecallableattribute"></a><span data-ttu-id="563b6-169">NativeCallableAttribute</span><span class="sxs-lookup"><span data-stu-id="563b6-169">NativeCallableAttribute</span></span>

<span data-ttu-id="563b6-170">CLR 有一項功能，可讓 managed 方法發出，使其可以直接從機器碼呼叫。</span><span class="sxs-lookup"><span data-stu-id="563b6-170">The CLR has a feature that allows for managed methods to be emitted in such a way that they are directly callable from native code.</span></span> <span data-ttu-id="563b6-171">這是藉由將 `NativeCallableAttribute` 新增至方法來完成。</span><span class="sxs-lookup"><span data-stu-id="563b6-171">This is done by adding the `NativeCallableAttribute` to methods.</span></span> <span data-ttu-id="563b6-172">這種方法只能從機器碼呼叫，因此只能在簽章中包含可複製的類型。</span><span class="sxs-lookup"><span data-stu-id="563b6-172">Such a method is only callable from native code and hence must contain only blittable types in the signature.</span></span> <span data-ttu-id="563b6-173">從 managed 程式碼呼叫會導致執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="563b6-173">Calling from managed code results in a runtime error.</span></span> 

<span data-ttu-id="563b6-174">這項功能適用于此提案，因為它允許：</span><span class="sxs-lookup"><span data-stu-id="563b6-174">This feature would pattern well with this proposal as it would allow:</span></span>

- <span data-ttu-id="563b6-175">將 managed 程式碼中定義的函式傳遞至機器碼，做為函式指標（透過的位址），而不會造成 managed 或機器碼的額外負荷。</span><span class="sxs-lookup"><span data-stu-id="563b6-175">Passing a function defined in managed code to native code as a function pointer (via address-of) with no overhead in managed or native code.</span></span> 
- <span data-ttu-id="563b6-176">執行時間可能會在 managed 程式碼中引進這類函式的「使用網站錯誤」，以避免在編譯時期叫用這些函式。</span><span class="sxs-lookup"><span data-stu-id="563b6-176">Runtime can introduce use site errors for such functions in managed code to prevent them from being invoked at compile time.</span></span>




