---
ms.openlocfilehash: 11e9d21bda9e69be692c5c5f5aee80c2da1894ab
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484619"
---
# <a name="unmanaged-type-constraint"></a><span data-ttu-id="ab937-101">非受控類型條件約束</span><span class="sxs-lookup"><span data-stu-id="ab937-101">Unmanaged type constraint</span></span>

## <a name="summary"></a><span data-ttu-id="ab937-102">摘要</span><span class="sxs-lookup"><span data-stu-id="ab937-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="ab937-103">非受控條件約束功能會將語言強制授與C#語言規格中稱為「非受控類型」的類型類別。 這在第18.2 節中定義為類型，這不是參考型別，而且不包含任何嵌套層級的參考類型欄位。</span><span class="sxs-lookup"><span data-stu-id="ab937-103">The unmanaged constraint feature will give language enforcement to the class of types known as "unmanaged types" in the C# language spec.  This is defined in section 18.2 as a type which is not a reference type and doesn't contain reference type fields at any level of nesting.</span></span>  

## <a name="motivation"></a><span data-ttu-id="ab937-104">動機</span><span class="sxs-lookup"><span data-stu-id="ab937-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="ab937-105">主要動機是讓您更輕鬆地在中C#撰寫低層級的 interop 程式碼。</span><span class="sxs-lookup"><span data-stu-id="ab937-105">The primary motivation is to make it easier to author low level interop code in C#.</span></span> <span data-ttu-id="ab937-106">非受控型別是 interop 程式碼的其中一個核心構成要素，但是在泛型中缺少支援，就無法在所有非受控型別上建立可重複使用的常式。</span><span class="sxs-lookup"><span data-stu-id="ab937-106">Unmanaged types are one of the core building blocks for interop code, yet the lack of support in generics makes it impossible to create re-usable routines across all unmanaged types.</span></span> <span data-ttu-id="ab937-107">相反地，開發人員會被迫針對其媒體櫃中的每個非受控類型，撰寫相同的定案盤子程式碼：</span><span class="sxs-lookup"><span data-stu-id="ab937-107">Instead developers are forced to author the same boiler plate code for every unmanaged type in their library:</span></span>

```csharp
int Hash(Point point) { ... } 
int Hash(TimeSpan timeSpan) { ... } 
```

<span data-ttu-id="ab937-108">若要啟用這種類型的案例，語言將引進新的條件約束：非受控：</span><span class="sxs-lookup"><span data-stu-id="ab937-108">To enable this type of scenario the language will be introducing a new constraint: unmanaged:</span></span>

```csharp
void Hash<T>(T value) where T : unmanaged
{
    ...
}
```

<span data-ttu-id="ab937-109">只有符合C#語言規格中非受控類型定義的類型，才能符合此條件約束。另一個查看的方法是，類型滿足非受控條件約束，iff 也可以用它做為指標。</span><span class="sxs-lookup"><span data-stu-id="ab937-109">This constraint can only be met by types which fit into the unmanaged type definition in the C# language spec. Another way of looking at it is that a type satisfies the unmanaged constraint iff it can also be used as a pointer.</span></span> 

```csharp
Hash(new Point()); // Okay 
Hash(42); // Okay
Hash("hello") // Error: Type string does not satisfy the unmanaged constraint
```

<span data-ttu-id="ab937-110">具有非受控條件約束的類型參數可以使用非受控類型的所有可用功能：指標、固定等等。</span><span class="sxs-lookup"><span data-stu-id="ab937-110">Type parameters with the unmanaged constraint can use all the features available to unmanaged types: pointers, fixed, etc ...</span></span> 

```csharp
void Hash<T>(T value) where T : unmanaged
{
    // Okay
    fixed (T* p = &value) 
    { 
        ...
    }
}
```

<span data-ttu-id="ab937-111">這個條件約束也可以讓結構化資料和位元組資料流程之間具有有效率的轉換。</span><span class="sxs-lookup"><span data-stu-id="ab937-111">This constraint will also make it possible to have efficient conversions between structured data and streams of bytes.</span></span> <span data-ttu-id="ab937-112">這是網路堆疊和序列化層中常見的作業：</span><span class="sxs-lookup"><span data-stu-id="ab937-112">This is an operation that is common in networking stacks and serialization layers:</span></span>

```csharp
Span<byte> Convert<T>(ref T value) where T : unmanaged 
{
    ...
}
```

<span data-ttu-id="ab937-113">這類常式的優點很有利，因為它們在編譯時期是可證明安全的，而且可以免費配置。</span><span class="sxs-lookup"><span data-stu-id="ab937-113">Such routines are advantageous because they are provably safe at compile time and allocation free.</span></span>  <span data-ttu-id="ab937-114">目前的 Interop 作者不能這麼做（即使是在效能非常重要的層級也一樣）。</span><span class="sxs-lookup"><span data-stu-id="ab937-114">Interop authors today can not do this (even though it's at a layer where perf is critical).</span></span>  <span data-ttu-id="ab937-115">相反地，他們必須依賴配置具有昂貴執行時間檢查的常式，以確認值的管理是否正確。</span><span class="sxs-lookup"><span data-stu-id="ab937-115">Instead they need to rely on allocating routines that have expensive runtime checks to verify values are correctly unmanaged.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="ab937-116">詳細設計</span><span class="sxs-lookup"><span data-stu-id="ab937-116">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="ab937-117">語言會引進名為 `unmanaged`的新條件約束。</span><span class="sxs-lookup"><span data-stu-id="ab937-117">The language will introduce a new constraint named `unmanaged`.</span></span> <span data-ttu-id="ab937-118">為了滿足此條件約束，型別必須是結構，而且該型別的所有欄位都必須屬於下列其中一個類別：</span><span class="sxs-lookup"><span data-stu-id="ab937-118">In order to satisfy this constraint a type must be a struct and all the fields of the type must fall into one of the following categories:</span></span>

- <span data-ttu-id="ab937-119">具有類型 `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`、`decimal`、`bool`、`IntPtr` 或 `UIntPtr`。</span><span class="sxs-lookup"><span data-stu-id="ab937-119">Have the type `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `IntPtr` or `UIntPtr`.</span></span>
- <span data-ttu-id="ab937-120">這是任何 `enum` 類型。</span><span class="sxs-lookup"><span data-stu-id="ab937-120">Be any `enum` type.</span></span>
- <span data-ttu-id="ab937-121">這是指標類型。</span><span class="sxs-lookup"><span data-stu-id="ab937-121">Be a pointer type.</span></span>
- <span data-ttu-id="ab937-122">這是 satsifies `unmanaged` 條件約束的使用者定義結構。</span><span class="sxs-lookup"><span data-stu-id="ab937-122">Be a user defined struct that satsifies the `unmanaged` constraint.</span></span>

<span data-ttu-id="ab937-123">編譯器產生的實例欄位（例如，支援自動執行的屬性）也必須符合這些條件約束。</span><span class="sxs-lookup"><span data-stu-id="ab937-123">Compiler generated instance fields, such as those backing auto-implemented properties, must also meet these constraints.</span></span> 

<span data-ttu-id="ab937-124">例如：</span><span class="sxs-lookup"><span data-stu-id="ab937-124">For example:</span></span>

```csharp
// Unmanaged type
struct Point 
{ 
    int X;
    int Y {get; set;}
}

// Not an unmanaged type
struct Student 
{ 
    string FirstName;
    string LastName;
}
``` 

<span data-ttu-id="ab937-125">`unmanaged` 條件約束無法與 `struct`、`class` 或 `new()`結合。</span><span class="sxs-lookup"><span data-stu-id="ab937-125">The `unmanaged` constraint cannot be combined with `struct`, `class` or `new()`.</span></span> <span data-ttu-id="ab937-126">這種限制衍生自 `unmanaged` 暗示的事實 `struct` 因此其他條件約束並不合理。</span><span class="sxs-lookup"><span data-stu-id="ab937-126">This restriction derives from the fact that `unmanaged` implies `struct` hence the other constraints do not make sense.</span></span>

<span data-ttu-id="ab937-127">`unmanaged` 條件約束並不會由 CLR 強制執行，只有語言才可。</span><span class="sxs-lookup"><span data-stu-id="ab937-127">The `unmanaged` constraint is not enforced by CLR, only by the language.</span></span> <span data-ttu-id="ab937-128">若要防止其他語言的錯誤使用，具有此條件約束的方法將會受到 mod 需求的保護。這會讓其他語言無法使用非受控類型的類型引數。</span><span class="sxs-lookup"><span data-stu-id="ab937-128">To prevent mis-use by other languages, methods which have this constraint will be protected by a mod-req. This will prevent other languages from using type arguments which are not unmanaged types.</span></span>

<span data-ttu-id="ab937-129">條件約束中 `unmanaged` 的 token 不是關鍵字，也不是內容關鍵字。</span><span class="sxs-lookup"><span data-stu-id="ab937-129">The token `unmanaged` in the constraint is not a keyword, nor a contextual keyword.</span></span> <span data-ttu-id="ab937-130">相反地 `var`，它會在該位置進行評估，並將會執行下列其中一個動作：</span><span class="sxs-lookup"><span data-stu-id="ab937-130">Instead it is like `var` in that it is evaluated at that location and will either:</span></span>

- <span data-ttu-id="ab937-131">系結至名為 `unmanaged`的使用者定義或參考型別：這將會被視為任何其他命名類型條件約束的處理方式。</span><span class="sxs-lookup"><span data-stu-id="ab937-131">Bind to user defined or referenced type named `unmanaged`: This will be treated just as any other named type constraint is treated.</span></span> 
- <span data-ttu-id="ab937-132">系結至 no 型別：這會被視為 `unmanaged` 條件約束。</span><span class="sxs-lookup"><span data-stu-id="ab937-132">Bind to no type: This will be interpreted as the `unmanaged` constraint.</span></span>

<span data-ttu-id="ab937-133">在此情況下，有一個名為 `unmanaged` 的型別，而且在目前的內容中沒有限定性，就無法使用 `unmanaged` 條件約束。</span><span class="sxs-lookup"><span data-stu-id="ab937-133">In the case there is a type named `unmanaged` and it is available without qualification in the current context, then there will be no way to use the `unmanaged` constraint.</span></span> <span data-ttu-id="ab937-134">這等同于功能 `var` 的規則和相同名稱的使用者定義型別。</span><span class="sxs-lookup"><span data-stu-id="ab937-134">This parallels the rules surrounding the feature `var` and user defined types of the same name.</span></span> 

## <a name="drawbacks"></a><span data-ttu-id="ab937-135">缺點</span><span class="sxs-lookup"><span data-stu-id="ab937-135">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="ab937-136">這項功能的主要缺點是，它會為少數開發人員提供服務：通常是低層級的程式庫作者或架構。</span><span class="sxs-lookup"><span data-stu-id="ab937-136">The primary drawback of this feature is that it serves a small number of developers: typically low level library authors or frameworks.</span></span>  <span data-ttu-id="ab937-137">因此，對於少數的開發人員而言，這是非常寶貴的語言時間。</span><span class="sxs-lookup"><span data-stu-id="ab937-137">Hence it's spending precious language time for a small number of developers.</span></span> 

<span data-ttu-id="ab937-138">不過，這些架構通常是大部分 .NET 應用程式的基礎。</span><span class="sxs-lookup"><span data-stu-id="ab937-138">Yet these frameworks are often the basis for the majority of .NET applications out there.</span></span>  <span data-ttu-id="ab937-139">因此，此層級的效能/正確性會在 .NET 生態系統上有 ripple 效應。</span><span class="sxs-lookup"><span data-stu-id="ab937-139">Hence performance / correctness wins at this level can have a ripple effect on the .NET ecosystem.</span></span>  <span data-ttu-id="ab937-140">這使得功能值得考慮，即使是有限的物件也是如此。</span><span class="sxs-lookup"><span data-stu-id="ab937-140">This makes the feature worth considering even with the limited audience.</span></span>

## <a name="alternatives"></a><span data-ttu-id="ab937-141">替代方案</span><span class="sxs-lookup"><span data-stu-id="ab937-141">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="ab937-142">有幾個需要考慮的替代方案：</span><span class="sxs-lookup"><span data-stu-id="ab937-142">There are a couple of alternatives to consider:</span></span>

- <span data-ttu-id="ab937-143">現狀：此功能不會在其本身的優勢中證明，而開發人員會繼續使用隱含的 opt 行為。</span><span class="sxs-lookup"><span data-stu-id="ab937-143">The status quo:  The feature is not justified on its own merits and developers continue to use the implicit opt in behavior.</span></span>

## <a name="questions"></a><span data-ttu-id="ab937-144">問題</span><span class="sxs-lookup"><span data-stu-id="ab937-144">Questions</span></span>
[quesions]: #questions

### <a name="metadata-representation"></a><span data-ttu-id="ab937-145">中繼資料標記法</span><span class="sxs-lookup"><span data-stu-id="ab937-145">Metadata Representation</span></span>

<span data-ttu-id="ab937-146">F#語言會編碼簽章檔案中的條件約束， C#這表示無法重複使用其標記法。</span><span class="sxs-lookup"><span data-stu-id="ab937-146">The F# language encodes the constraint in the signature file which means C# cannot re-use their representation.</span></span> <span data-ttu-id="ab937-147">必須為此條件約束選擇新的屬性。</span><span class="sxs-lookup"><span data-stu-id="ab937-147">A new attribute will need to be chosen for this constraint.</span></span> <span data-ttu-id="ab937-148">此外，具有此條件約束的方法必須受到 mod 需求的保護。</span><span class="sxs-lookup"><span data-stu-id="ab937-148">Additionally a method which has this constraint must be protected by a mod-req.</span></span>

### <a name="blittable-vs-unmanaged"></a><span data-ttu-id="ab937-149">可直接管理的與非受控</span><span class="sxs-lookup"><span data-stu-id="ab937-149">Blittable vs. Unmanaged</span></span>
<span data-ttu-id="ab937-150">此F#語言具有非常[類似的功能](https://docs.microsoft.com/dotnet/articles/fsharp/language-reference/generics/constraints)，使用不受管理的關鍵字。</span><span class="sxs-lookup"><span data-stu-id="ab937-150">The F# language has a very [similar feature](https://docs.microsoft.com/dotnet/articles/fsharp/language-reference/generics/constraints) which uses the keyword unmanaged.</span></span> <span data-ttu-id="ab937-151">可直接連線的名稱來自 Midori 中的使用。</span><span class="sxs-lookup"><span data-stu-id="ab937-151">The blittable name comes from the use in Midori.</span></span>  <span data-ttu-id="ab937-152">可能想要在此尋找優先順序，並改用非受控。</span><span class="sxs-lookup"><span data-stu-id="ab937-152">May want to look to precedence here and use unmanaged instead.</span></span> 

<span data-ttu-id="ab937-153">**解決**方式語言決定使用非受控</span><span class="sxs-lookup"><span data-stu-id="ab937-153">**Resolution** The language decide to use unmanaged</span></span> 

### <a name="verifier"></a><span data-ttu-id="ab937-154">驗證器</span><span class="sxs-lookup"><span data-stu-id="ab937-154">Verifier</span></span>

<span data-ttu-id="ab937-155">是否需要更新驗證器/執行時間，以瞭解如何使用泛型型別參數的指標？</span><span class="sxs-lookup"><span data-stu-id="ab937-155">Does the verifier / runtime need to be updated to understand the use of pointers to generic type parameters?</span></span>  <span data-ttu-id="ab937-156">或者，它可以直接在沒有變更的情況下工作嗎？</span><span class="sxs-lookup"><span data-stu-id="ab937-156">Or can it simply work as is without changes?</span></span>

<span data-ttu-id="ab937-157">**解決**方式不需要任何變更。</span><span class="sxs-lookup"><span data-stu-id="ab937-157">**Resolution** No changes needed.</span></span> <span data-ttu-id="ab937-158">所有指標類型都只是無法驗證。</span><span class="sxs-lookup"><span data-stu-id="ab937-158">All pointer types are simply unverifiable.</span></span> 

## <a name="design-meetings"></a><span data-ttu-id="ab937-159">設計會議</span><span class="sxs-lookup"><span data-stu-id="ab937-159">Design meetings</span></span>

<span data-ttu-id="ab937-160">n/a</span><span class="sxs-lookup"><span data-stu-id="ab937-160">n/a</span></span>
