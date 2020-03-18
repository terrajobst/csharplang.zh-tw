---
ms.openlocfilehash: 340a1dc5a2eac653458d7d29f74551e5fe28b6d5
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484556"
---
# <a name="operators-should-be-exposed-for-systemintptr-and-systemuintptr"></a><span data-ttu-id="ae981-101">應該公開 `System.IntPtr` 和 `System.UIntPtr` 的運算子</span><span class="sxs-lookup"><span data-stu-id="ae981-101">Operators should be exposed for `System.IntPtr` and `System.UIntPtr`</span></span>

* <span data-ttu-id="ae981-102">[x] 提議</span><span class="sxs-lookup"><span data-stu-id="ae981-102">[x] Proposed</span></span>
* <span data-ttu-id="ae981-103">[] 原型：未啟動</span><span class="sxs-lookup"><span data-stu-id="ae981-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="ae981-104">[] 執行：未啟動</span><span class="sxs-lookup"><span data-stu-id="ae981-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="ae981-105">[] 規格：未啟動</span><span class="sxs-lookup"><span data-stu-id="ae981-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="ae981-106">摘要</span><span class="sxs-lookup"><span data-stu-id="ae981-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="ae981-107">CLR 支援一組 `System.IntPtr` 和 `System.UIntPtr` 類型的運算子（`native int`）。</span><span class="sxs-lookup"><span data-stu-id="ae981-107">The CLR supports a set of operators for the `System.IntPtr` and `System.UIntPtr` types (`native int`).</span></span> <span data-ttu-id="ae981-108">這些運算子可以在通用語言基礎結構規格（`ECMA-335`） `III.1.5` 中看到。</span><span class="sxs-lookup"><span data-stu-id="ae981-108">These operators can be seen in `III.1.5` of the Common Language Infrastructure specification (`ECMA-335`).</span></span> <span data-ttu-id="ae981-109">不過，不支援這些運算子C#。</span><span class="sxs-lookup"><span data-stu-id="ae981-109">However, these operators are not supported by C#.</span></span>

<span data-ttu-id="ae981-110">應該針對 `System.IntPtr` 和 `System.UIntPtr`支援的一組完整運算子提供語言支援。</span><span class="sxs-lookup"><span data-stu-id="ae981-110">Language support should be provided for the full set of operators supported by `System.IntPtr` and `System.UIntPtr`.</span></span> <span data-ttu-id="ae981-111">這些運算子包括： `Add`、`Divide`、`Multiply`、`Remainder`、`Subtract`、`Negate`、`Equals`、`Compare`、`And`、`Not`、`Or`、`XOr`、`ShiftLeft`、`ShiftRight`。</span><span class="sxs-lookup"><span data-stu-id="ae981-111">These operators are: `Add`, `Divide`, `Multiply`, `Remainder`, `Subtract`, `Negate`, `Equals`, `Compare`, `And`, `Not`, `Or`, `XOr`, `ShiftLeft`, `ShiftRight`.</span></span>

## <a name="motivation"></a><span data-ttu-id="ae981-112">動機</span><span class="sxs-lookup"><span data-stu-id="ae981-112">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="ae981-113">現今，使用者可以使用各種C#工具和架構，輕鬆地撰寫以多個平臺為目標的應用程式，例如： `Xamarin`、`.NET Core`、`Mono`等等。</span><span class="sxs-lookup"><span data-stu-id="ae981-113">Today, users can easily write C# applications targeting multiple platforms using various tools and frameworks, such as: `Xamarin`, `.NET Core`, `Mono`, etc...</span></span>

<span data-ttu-id="ae981-114">撰寫跨平臺程式碼時，通常需要撰寫以特定方式與特定目標平臺互動的 interop 程式碼。</span><span class="sxs-lookup"><span data-stu-id="ae981-114">When writing cross-platform code, it is often necessary to write interop code that interacts with a particular target platform in a specific manner.</span></span> <span data-ttu-id="ae981-115">這可能包括撰寫圖形程式碼、呼叫一些系統 API，或與現有的原生程式庫互動。</span><span class="sxs-lookup"><span data-stu-id="ae981-115">This could include writing graphics code, calling some System API, or interacting with an existing native library.</span></span>

<span data-ttu-id="ae981-116">此 interop 程式碼通常必須處理控制碼、非受控記憶體，或甚至是平臺特定大小的整數。</span><span class="sxs-lookup"><span data-stu-id="ae981-116">This interop code often has to deal with handles, unmanaged memory, or even just platform-specific sized integers.</span></span>

<span data-ttu-id="ae981-117">執行時間藉由定義一組可用於 `native int` （`System.IntPtr`）和 `native unsigned int` （`System.UIntPtr`）基本類型的運算子，來提供這項支援。</span><span class="sxs-lookup"><span data-stu-id="ae981-117">The runtime provides support for this by defining a set of operators that can be used on the `native int` (`System.IntPtr`) and `native unsigned int` (`System.UIntPtr`) primitive types.</span></span>

<span data-ttu-id="ae981-118">C#已不支援這些運算子，因此使用者必須解決此問題。</span><span class="sxs-lookup"><span data-stu-id="ae981-118">C# has never supported these operators and so users have to work around the issue.</span></span> <span data-ttu-id="ae981-119">這通常會增加程式碼的複雜度，並減少程式碼維護性。</span><span class="sxs-lookup"><span data-stu-id="ae981-119">This often increases code complexity and lowers code maintainability.</span></span>

<span data-ttu-id="ae981-120">因此，語言應開始支援這些運算子，以協助加強語言，以進一步支援這些需求。</span><span class="sxs-lookup"><span data-stu-id="ae981-120">As such, the language should begin to support these operators to help advance the language to better support these requirements.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="ae981-121">詳細設計</span><span class="sxs-lookup"><span data-stu-id="ae981-121">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="ae981-122">支援的完整運算子集定義于通用語言基礎結構規格（`ECMA-335`）的 `III.1.5` 中。</span><span class="sxs-lookup"><span data-stu-id="ae981-122">The full set of operators supported are defined in `III.1.5` of the Common Language Infrastructure specification (`ECMA-335`).</span></span> <span data-ttu-id="ae981-123">此規格可在這裡取得： [https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf)。</span><span class="sxs-lookup"><span data-stu-id="ae981-123">The specification is available here: [https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf).</span></span>

* <span data-ttu-id="ae981-124">下面提供運算子的摘要以方便您使用。</span><span class="sxs-lookup"><span data-stu-id="ae981-124">A summary of the operators is provided below for convenience.</span></span>
* <span data-ttu-id="ae981-125">CLI 規格所定義的無法驗證的運算子不會列出，而且目前不屬於本提案（雖然值得一併考慮）。</span><span class="sxs-lookup"><span data-stu-id="ae981-125">The unverifiable operators defined by the CLI spec are not listed and are not currently part of this proposal (although it may be worth considering these as well).</span></span>
* <span data-ttu-id="ae981-126">提供關鍵字（例如 `nint` 和 `nuint`），也不提供要針對 `System.IntPtr` 和 `System.UIntPtr` （例如0n）宣告的常值，這不是本提案的一部分（但也值得考慮這一點）。</span><span class="sxs-lookup"><span data-stu-id="ae981-126">Providing a keyword (such as `nint` and `nuint`) nor providing a way to for literals to be declared for `System.IntPtr` and `System.UIntPtr` (such as 0n) is not part of this proposal (although it may be worth considering these as well).</span></span>

### <a name="unary-plus-operator"></a><span data-ttu-id="ae981-127">一元加號運算子</span><span class="sxs-lookup"><span data-stu-id="ae981-127">Unary Plus Operator</span></span>

```csharp
System.IntPtr operator +(System.IntPtr)
```

```csharp
System.UIntPtr operator +(System.UIntPtr)
```

### <a name="unary-minus-operator"></a><span data-ttu-id="ae981-128">一元減號運算子</span><span class="sxs-lookup"><span data-stu-id="ae981-128">Unary Minus Operator</span></span>

```csharp
System.IntPtr operator -(System.IntPtr)
```

### <a name="bitwise-complement-operator"></a><span data-ttu-id="ae981-129">位補數運算子</span><span class="sxs-lookup"><span data-stu-id="ae981-129">Bitwise Complement Operator</span></span>

```csharp
System.IntPtr operator ~(System.IntPtr)
```

```csharp
System.UIntPtr operator ~(System.UIntPtr)
```

### <a name="cast-operators"></a><span data-ttu-id="ae981-130">轉型運算子</span><span class="sxs-lookup"><span data-stu-id="ae981-130">Cast Operators</span></span>

```csharp
explicit operator sbyte(System.IntPtr)               // Truncate
explicit operator short(System.IntPtr)               // Truncate
explicit operator int(System.IntPtr)                 // Truncate
explicit operator long(System.IntPtr)                // Sign Extend

explicit operator byte(System.IntPtr)                // Truncate
explicit operator ushort(System.IntPtr)              // Truncate
explicit operator uint(System.IntPtr)                // Truncate
explicit operator ulong(System.IntPtr)               // Zero Extend

explicit operator System.IntPtr(int)                 // Sign Extend
explicit operator System.IntPtr(long)                // Truncate

explicit operator System.IntPtr(uint)                // Sign Extend
explicit operator System.IntPtr(ulong)               // Truncate

explicit operator System.IntPtr(System.IntPtr)
explicit operator System.IntPtr(System.UIntPtr)
```

```csharp
explicit operator sbyte(System.UIntPtr)               // Truncate
explicit operator short(System.UIntPtr)               // Truncate
explicit operator int(System.UIntPtr)                 // Truncate
explicit operator long(System.UIntPtr)                // Sign Extend

explicit operator byte(System.UIntPtr)                // Truncate
explicit operator ushort(System.UIntPtr)              // Truncate
explicit operator uint(System.UIntPtr)                // Truncate
explicit operator ulong(System.UIntPtr)               // Zero Extend

explicit operator System.UIntPtr(int)                 // Zero Extend
explicit operator System.UIntPtr(long)                // Truncate

explicit operator System.UIntPtr(uint)                // Zero Extend
explicit operator System.UIntPtr(ulong)               // Truncate

explicit operator System.UIntPtr(System.IntPtr)
explicit operator System.UIntPtr(System.UIntPtr)
```

### <a name="multiplication-operator"></a><span data-ttu-id="ae981-131">乘法運算子</span><span class="sxs-lookup"><span data-stu-id="ae981-131">Multiplication Operator</span></span>

```csharp
System.IntPtr operator *(int, System.IntPtr)
System.IntPtr operator *(System.IntPtr, int)
System.IntPtr operator *(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator *(uint, System.UIntPtr)
System.UIntPtr operator *(System.UIntPtr, uint)
System.UIntPtr operator *(System.UIntPtr, System.UIntPtr)
```

### <a name="division-operator"></a><span data-ttu-id="ae981-132">除法運算子</span><span class="sxs-lookup"><span data-stu-id="ae981-132">Division Operator</span></span>

```csharp
System.IntPtr operator /(int, System.IntPtr)
System.IntPtr operator /(System.IntPtr, int)
System.IntPtr operator /(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator /(uint, System.UIntPtr)
System.UIntPtr operator /(System.UIntPtr, uint)
System.UIntPtr operator /(System.UIntPtr, System.UIntPtr)
```

### <a name="remainder-operator"></a><span data-ttu-id="ae981-133">餘數運算子</span><span class="sxs-lookup"><span data-stu-id="ae981-133">Remainder Operator</span></span>

```csharp
System.IntPtr operator %(int, System.IntPtr)
System.IntPtr operator %(System.IntPtr, int)
System.IntPtr operator %(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator %(uint, System.UIntPtr)
System.UIntPtr operator %(System.UIntPtr, uint)
System.UIntPtr operator %(System.UIntPtr, System.UIntPtr)
```

### <a name="addition-operator"></a><span data-ttu-id="ae981-134">加法運算子</span><span class="sxs-lookup"><span data-stu-id="ae981-134">Addition Operator</span></span>

```csharp
System.IntPtr operator +(int, System.IntPtr)
System.IntPtr operator +(System.IntPtr, int)
System.IntPtr operator +(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator +(uint, System.UIntPtr)
System.UIntPtr operator +(System.UIntPtr, uint)
System.UIntPtr operator +(System.UIntPtr, System.UIntPtr)
```

### <a name="subtraction-operator"></a><span data-ttu-id="ae981-135">減法運算子</span><span class="sxs-lookup"><span data-stu-id="ae981-135">Subtraction Operator</span></span>

```csharp
System.IntPtr operator -(int, System.IntPtr)
System.IntPtr operator -(System.IntPtr, int)
System.IntPtr operator -(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator -(uint, System.UIntPtr)
System.UIntPtr operator -(System.UIntPtr, uint)
System.UIntPtr operator -(System.UIntPtr, System.UIntPtr)
```

### <a name="shift-operators"></a><span data-ttu-id="ae981-136">移位運算子</span><span class="sxs-lookup"><span data-stu-id="ae981-136">Shift Operators</span></span>

```csharp
System.IntPtr operator <<(System.IntPtr, int)
System.IntPtr operator >>(System.IntPtr, int)
```

```csharp
System.UIntPtr operator <<(System.UIntPtr, int)
System.UIntPtr operator >>(System.UIntPtr, int)
```

### <a name="integer-comparison-operators"></a><span data-ttu-id="ae981-137">整數比較運算子</span><span class="sxs-lookup"><span data-stu-id="ae981-137">Integer Comparison Operators</span></span>

```csharp
bool operator ==(int, System.IntPtr)
bool operator ==(System.IntPtr, int)
bool operator ==(System.IntPtr, System.IntPtr)

bool operator !=(int, System.IntPtr)
bool operator !=(System.IntPtr, int)
bool operator !=(System.IntPtr, System.IntPtr)

bool operator  <(int, System.IntPtr)
bool operator  <(System.IntPtr, int)
bool operator  <(System.IntPtr, System.IntPtr)

bool operator  >(int, System.IntPtr)
bool operator  >(System.IntPtr, int)
bool operator  >(System.IntPtr, System.IntPtr)

bool operator <=(int, System.IntPtr)
bool operator <=(System.IntPtr, int)
bool operator <=(System.IntPtr, System.IntPtr)

bool operator >=(int, System.IntPtr)
bool operator >=(System.IntPtr, int)
bool operator >=(System.IntPtr, System.IntPtr)
```

```csharp
bool operator ==(uint, System.UIntPtr)
bool operator ==(System.UIntPtr, uint)
bool operator ==(System.UIntPtr, System.UIntPtr)

bool operator !=(uint, System.UIntPtr)
bool operator !=(System.UIntPtr, uint)
bool operator !=(System.UIntPtr, System.UIntPtr)

bool operator  <(uint, System.UIntPtr)
bool operator  <(System.UIntPtr, uint)
bool operator  <(System.UIntPtr, System.UIntPtr)

bool operator  >(uint, System.UIntPtr)
bool operator  >(System.UIntPtr, uint)
bool operator  >(System.UIntPtr, System.UIntPtr)

bool operator <=(uint, System.UIntPtr)
bool operator <=(System.UIntPtr, uint)
bool operator <=(System.UIntPtr, System.UIntPtr)

bool operator >=(uint, System.UIntPtr)
bool operator >=(System.UIntPtr, uint)
bool operator >=(System.UIntPtr, System.UIntPtr)
```

### <a name="integer-logical-operators"></a><span data-ttu-id="ae981-138">整數邏輯運算子</span><span class="sxs-lookup"><span data-stu-id="ae981-138">Integer Logical Operators</span></span>

```csharp
System.IntPtr operator &(int, System.IntPtr)
System.IntPtr operator &(System.IntPtr, int)
System.IntPtr operator &(System.IntPtr, System.IntPtr)

System.IntPtr operator |(int, System.IntPtr)
System.IntPtr operator |(System.IntPtr, int)
System.IntPtr operator |(System.IntPtr, System.IntPtr)

System.IntPtr operator ^(int, System.IntPtr)
System.IntPtr operator ^(System.IntPtr, int)
System.IntPtr operator ^(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator &(uint, System.UIntPtr)
System.UIntPtr operator &(System.UIntPtr, uint)
System.UIntPtr operator &(System.UIntPtr, System.UIntPtr)

System.UIntPtr operator |(uint, System.UIntPtr)
System.UIntPtr operator |(System.UIntPtr, uint)
System.UIntPtr operator |(System.UIntPtr, System.UIntPtr)

System.UIntPtr operator ^(uint, System.UIntPtr)
System.UIntPtr operator ^(System.UIntPtr, uint)
System.UIntPtr operator ^(System.UIntPtr, System.UIntPtr)
```

## <a name="drawbacks"></a><span data-ttu-id="ae981-139">缺點</span><span class="sxs-lookup"><span data-stu-id="ae981-139">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="ae981-140">這些運算子的實際使用可能很小，而且僅限於正在寫入較低層級程式庫或 interop 程式碼的使用者。</span><span class="sxs-lookup"><span data-stu-id="ae981-140">The actual use of these operators may be small and limited to end-users who are writing lower level libraries or interop code.</span></span> <span data-ttu-id="ae981-141">大部分的使用者可能會耗用這些較低層級的程式庫本身，這會將原生大小的整數、控制碼和 interop 程式碼抽象化出去。</span><span class="sxs-lookup"><span data-stu-id="ae981-141">Most end-users would likely be consuming these lower level libraries themselves which would have the native sized integers, handles, and interop code abstracted away.</span></span> <span data-ttu-id="ae981-142">因此，他們不需要操作員本身。</span><span class="sxs-lookup"><span data-stu-id="ae981-142">As such, they would not have need of the operators themselves.</span></span>

## <a name="alternatives"></a><span data-ttu-id="ae981-143">替代方案</span><span class="sxs-lookup"><span data-stu-id="ae981-143">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="ae981-144">讓架構直接在 IL 中撰寫所需的運算子，藉此執行。</span><span class="sxs-lookup"><span data-stu-id="ae981-144">Have the framework implement the required operators by writing them directly in IL.</span></span> <span data-ttu-id="ae981-145">此外，執行時間可以為架構所定義的運算子提供內建支援，以便更有效地將結束效能優化。</span><span class="sxs-lookup"><span data-stu-id="ae981-145">Additionally, the runtime could provide intrinsic support for the operators defined by the framework, so as to better optimize the end performance.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="ae981-146">未解決的問題</span><span class="sxs-lookup"><span data-stu-id="ae981-146">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="ae981-147">設計的哪些部分仍待 TBD？</span><span class="sxs-lookup"><span data-stu-id="ae981-147">What parts of the design are still TBD?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="ae981-148">設計會議</span><span class="sxs-lookup"><span data-stu-id="ae981-148">Design meetings</span></span>

<span data-ttu-id="ae981-149">連結至會影響此提案的設計附注，並針對它們所導致的變更，逐一說明一個句子。</span><span class="sxs-lookup"><span data-stu-id="ae981-149">Link to design notes that affect this proposal, and describe in one sentence for each what changes they led to.</span></span>